---
title: Spark Streaming & processamento de eventos exatamente uma vez - Azure HDInsight
description: Como configurar o Apache Spark Streaming para processar um evento uma vez e apenas uma vez.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 11/15/2018
ms.openlocfilehash: ee4f9b84e822cb370e5fe3d55fcceb9c8a9f2ab9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74228980"
---
# <a name="create-apache-spark-streaming-jobs-with-exactly-once-event-processing"></a>Crie tarefas do Apache Spark Streaming com processamento de eventos exatamente uma vez

Os aplicativos de processamento de fluxo tomam diferentes abordagens de como lidam com mensagens de reprocessamento após alguma falha no sistema:

* Pelo menos uma vez: cada mensagem é garantida para ser processada, mas pode ser processada mais de uma vez.
* No máximo, uma vez: cada mensagem pode ou não ser processada. Se uma mensagem é processada, ela só é processada uma vez.
* Exatamente uma vez: cada mensagem é garantida para ser processada uma vez e apenas uma vez.

Este artigo mostra como configurar o streaming do Spark para obter um processamento exatamente uma vez.

## <a name="exactly-once-semantics-with-apache-spark-streaming"></a>Semântica exatamente uma vez com o Apache Spark Streaming

Primeiro, considere como todos os pontos de falha do sistema são reiniciados após terem um problema e como você poderá evitar a perda de dados. Um aplicativo de streaming do Spark possui:

* Uma fonte de entrada.
* Um ou mais processos receptores que efetuam pull de dados da fonte de entrada.
* Tarefas que processam os dados.
* Um coletor de saída.
* Um processo de driver que gerencia o trabalho de execução longa.

As semânticas exatamente uma vez exigem que nenhum dado seja perdido em nenhum ponto e que o processamento de mensagens seja reiniciado, independentemente de onde a falha ocorrer.

### <a name="replayable-sources"></a>Fontes reproduzíveis

A fonte em que seu aplicativo de streaming do Spark estiver lendo os eventos deverá ser *reproduzível*. Isso significa que, nos casos em que a mensagem for recuperada, mas o sistema falhar antes que a mensagem possa ser persistente ou processada, a fonte deverá fornecer a mesma mensagem novamente.

No Azure, os Hubs de Eventos do Azure e o [Apache Kafka](https://kafka.apache.org/) no HDInsight fornecem fontes reproduzíveis. Outro exemplo de uma fonte reproduzível é um sistema de arquivos tolerante a falhas como [HDFS do Apache Hadoop](https://hadoop.apache.org/docs/r1.2.1/hdfs_design.html), blobs de Armazenamento do Azure, ou Azure Data Lake Storage, em que todos os dados são mantidos para sempre e a qualquer momento você poderá reler os dados em sua totalidade.

### <a name="reliable-receivers"></a>Receptores confiáveis

No streaming do Spark, fontes como Hubs de Eventos e Kafka possuem *receptores confiáveis*, onde cada receptor rastreia seu progresso pela leitura da fonte. Um receptor confiável mantém seu estado em um armazenamento tolerante a falhas, seja no [Apache ZooKeeper](https://zookeeper.apache.org/) ou em pontos de verificação do Spark Streaming gravados no HDFS. Se esse receptor falhar e for reiniciado posteriormente, ele poderá recomeçar de onde parou.

### <a name="use-the-write-ahead-log"></a>Usar log write-ahead

O streaming do Spark fornece suporte para uso de um log write-ahead, onde cada evento recebido é gravado primeiro no diretório do ponto de verificação do Spark em armazenamento tolerante a falhas e, em seguida, armazenado em um RDD (Conjunto de dados distribuídos resilientes). No Azure, o armazenamento tolerante a falhas é o HDFS com suporte pelo Armazenamento do Azure ou Azure Data Lake Storage. No seu aplicativo de streaming do Spark, o log write-ahead é habilitado para todos os receptores, configurando a `spark.streaming.receiver.writeAheadLog.enable` definição de configuração para `true`. O log write-ahead fornece tolerância a falhas para falhas do driver e dos executores.

Para os trabalhos executando tarefas em dados de eventos, cada RDD é, por definição, replicado e distribuído em vários trabalhos. Se uma tarefa falhar porque o trabalhador executá-lo caiu, a tarefa será reiniciada em outro trabalhador que tenha uma réplica dos dados do evento, para que o evento não seja perdido.

### <a name="use-checkpoints-for-drivers"></a>Usar pontos de verificação para drivers

Os drivers de trabalho precisam ser reiniciáveis. Se o driver executando seu aplicativo de streaming do Spark falhar, ele tornará todos os receptores em execução, tarefas e quaisquer RDDs armazenando dados de eventos inoperantes. Nesse caso, será necessário salvar o progresso do trabalho para que você possa retomá-lo posteriormente. Isto é conseguido verificando periodicamente o DAG (Grafo Direcionado Acíclico) do DStream para armazenamento tolerante a falhas. Os metadados do DAG incluem a configuração usada para criar o aplicativo de streaming, as operações que definem o aplicativo e os lotes que estão na fila, mas ainda não foram concluídos. Esses metadados permitem que um driver falhado seja reiniciado a partir das informações do ponto de verificação. Quando o driver reiniciar, ele iniciará novos receptores que recuperam os dados de eventos de volta aos RDDs do log write-ahead.

Os pontos de verificação são habilitados no streaming do Spark em duas etapas.

1. No objeto StreamingContext, configure o caminho de armazenamento para os pontos de verificação:

    ```Scala
    val ssc = new StreamingContext(spark, Seconds(1))
    ssc.checkpoint("/path/to/checkpoints")
    ```

    No HDInsight, esses pontos de verificação devem ser salvos no armazenamento padrão anexado ao cluster, no Armazenamento do Azure ou Azure Data Lake Storage.

2. Em seguida, especifique um intervalo de ponto de verificação (em segundos) no DStream. Em cada intervalo, os dados de estado derivados do evento de entrada persistem ao armazenamento. Os dados de estado persistentes podem reduzir a computação necessária ao recompilar o estado do evento de origem.

    ```Scala
    val lines = ssc.socketTextStream("hostname", 9999)
    lines.checkpoint(30)
    ssc.start()
    ssc.awaitTermination()
    ```

### <a name="use-idempotent-sinks"></a>Usar coletores idempotent

O fundo de destino para o qual seu trabalho escreve resultados deve ser capaz de lidar com a situação em que é dado o mesmo resultado mais de uma vez. O coletor deverá ser capaz de detectar esses resultados duplicados e ignorá-los. Um coletor *idempotent* pode ser chamado várias vezes com os mesmos dados sem mudança de estado.

É possível criar coletores idempotentes implementando a lógica que primeiro verifica a existência do resultado recebido no armazenamento de dados. Se o resultado já existir, a gravação deverá aparecer para ter êxito na perspectiva do trabalho do Spark, mas, na realidade, o armazenamento de dados ignorou os dados duplicados. Se o resultado não existir, então a pia deve inserir este novo resultado em seu armazenamento.

Por exemplo, é possível usar um procedimento armazenado com Banco de Dados SQL do Azure que insere eventos em uma tabela. Esse procedimento armazenado primeiro procura o evento por campos-chave e somente quando nenhum evento correspondente é encontrado, o registro será inserido na tabela.

Outro exemplo é usar um sistema de arquivos particionado como blobs de armazenamento do Azure ou Azure Data Lake Storage. Neste caso, sua lógica de pia não precisa verificar a existência de um arquivo. Se o arquivo que representa o evento existe, ele é simplesmente substituído com os mesmos dados. Caso contrário, um novo arquivo será criado no caminho computado.

## <a name="next-steps"></a>Próximas etapas

* [Visão geral do Streaming do Apache Spark](apache-spark-streaming-overview.md)
* [Criando trabalhos de Streaming do Apache Spark altamente disponíveis no Apache Hadoop YARN](apache-spark-streaming-high-availability.md)

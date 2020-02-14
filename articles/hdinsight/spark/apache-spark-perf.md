---
title: Otimizar os trabalhos do Spark para desempenho – Microsoft Azure HDInsight
description: Mostre estratégias comuns para o melhor desempenho de clusters de Apache Spark no Azure HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 02/12/2020
ms.openlocfilehash: 3d8f4a28961be7e0ece517e00026d9711d8f67e9
ms.sourcegitcommit: 333af18fa9e4c2b376fa9aeb8f7941f1b331c11d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/13/2020
ms.locfileid: "77198864"
---
# <a name="optimize-apache-spark-jobs-in-hdinsight"></a>Otimizar Apache Spark trabalhos no HDInsight

Saiba como otimizar a configuração de cluster do [Apache Spark](https://spark.apache.org/) para a carga de trabalho específica.  O desafio mais comum é a pressão de memória, devido a configurações incorretas (especialmente executores de tamanho errado), operações de execução longa e tarefas que resultam em operações cartesianas. É possível acelerar os trabalhos com o armazenamento em cache apropriado e permitir [distorção de dados](#optimize-joins-and-shuffles). Para obter o melhor desempenho, monitore e revise as execuções de trabalho do Spark de execução longa e consumo de recursos. Para obter informações sobre como começar a usar o Apache Spark no HDInsight, consulte [criar Apache Spark cluster usando portal do Azure](apache-spark-jupyter-spark-sql-use-portal.md).

As seções a seguir descrevem as recomendações e otimizações de trabalho do Spark comuns.

## <a name="choose-the-data-abstraction"></a>Escolha a abstração de dados

As versões anteriores do Spark usam RDDs para abstrair dados, Spark 1,3 e 1,6 introduziu dataframes e DataSets, respectivamente. Considere os seguintes méritos relativos:

* **DataFrames**
    * Melhor escolha na maioria das situações.
    * Fornece otimização de consulta através do Catalyst.
    * Geração de código em estágio inteiro.
    * Acesso direto à memória.
    * Baixa sobrecarga de coleta de lixo (GC).
    * Não é tão amigável para desenvolvedores como os Conjuntos de Dados, pois não há verificações de tempo de compilação ou programação de objeto de domínio.
* **Conjuntos de Dados**
    * Bom em pipelines ETL complexos, onde o impacto no desempenho é aceitável.
    * Não é bom em agregações onde o impacto no desempenho pode ser considerável.
    * Fornece otimização de consulta através do Catalyst.
    * Amigável para o desenvolvedor ao fornecer programação de objeto de domínio e verificações de tempo de compilação.
    * Adicionar sobrecarga de serialização/desserialização.
    * Alta sobrecarga de GC.
    * Interrompe a geração de código em estágio inteiro.
* **RDDs**
    * Você não precisa usar o RDDs, a menos que precise criar um novo RDD personalizado.
    * Nenhuma otimização de consulta através do Catalyst.
    * Nenhuma geração de código em estágio inteiro.
    * Alta sobrecarga de GC.
    * Deve usar as API herdadas do Spark 1.x.

## <a name="use-optimal-data-format"></a>Usar o formato de dados ideal

O Spark fornece suporte a muitos formatos, como csv, json, xml, parquet, orc e avro. O Spark pode ser estendido para fornecer suporte a muitos outros formatos com fontes de dados externos - para obter mais informações, consulte [Pacotes do Apache Spark](https://spark-packages.org).

O melhor formato para desempenho é parquet com *compactação snappy*, que é o padrão no Spark 2.x. O parquet armazena dados em formato colunar e é altamente otimizado no Spark.

## <a name="select-default-storage"></a>Selecionar o armazenamento padrão

Ao criar um novo cluster Spark, você pode selecionar o armazenamento de BLOBs do Azure ou Azure Data Lake Storage como o armazenamento padrão do cluster. Ambas as opções oferecem o benefício do armazenamento de longo prazo para clusters transitórios, para que seus dados não sejam excluídos automaticamente quando você excluir o cluster. É possível recriar um cluster transitório e ainda acessar seus dados.

| Tipo de Armazenamento | Sistema de Arquivos | Velocidade | Transitório | Casos de uso |
| --- | --- | --- | --- | --- |
| Armazenamento do Blobs do Azure | **wasb:** //url/ | **Standard** | Sim | Cluster transitório |
| Armazenamento de BLOBs do Azure (seguro) | **wasbs:** //URL/ | **Standard** | Sim | Cluster transitório |
| Azure Data Lake Storage Gen 2| **abfs:** //URL/ | **Mais rápido** | Sim | Cluster transitório |
| Azure Data Lake Storage Gen 1| **adl:** //url/ | **Mais rápido** | Sim | Cluster transitório |
| HDFS local | **hdfs:** //url/ | **Mais rápida** | Não | Cluster interativo 24/7 |

Para obter uma descrição completa das opções de armazenamento disponíveis para clusters HDInsight, consulte [comparar as opções de armazenamento para uso com clusters do Azure hdinsight](../hdinsight-hadoop-compare-storage-options.md).

## <a name="use-the-cache"></a>Usar o cache

O Spark fornece os próprios mecanismos de cache nativo que podem ser utilizados através de diferentes métodos, como `.persist()`, `.cache()` e `CACHE TABLE`. Esse cache nativo é efetivo com pequenos conjuntos de dados, bem como em pipelines ETL, onde é necessário armazenar em cache resultados intermediários. No entanto, o cache nativo do Spark atualmente não funciona bem com o particionamento, uma vez que uma tabela armazenada em cache não mantém os dados de particionamento. Uma técnica de cache mais genérica e confiável é o *cache de camada de armazenamento*.

* Cache do Spark Nativo (não recomendado)
    * Bom para pequenos conjuntos de dados.
    * Não funciona com particionamento, o que pode ser alterado em versões futuras do Spark.

* Cache de nível de armazenamento (recomendado)
    * Pode ser implementado no HDInsight usando o recurso de [cache de e/s](apache-spark-improve-performance-iocache.md) .
    * Usa cache SSD e em memória.

* HDFS local (recomendado)
    * caminho `hdfs://mycluster`.
    * Usa cache SSD.
    * Os dados armazenados em cache serão perdidos ao excluir o cluster, e que será necessário recompilar o cache.

## <a name="use-memory-efficiently"></a>Usar a memória de maneira eficiente

O Spark opera colocando dados na memória, de modo que gerenciar recursos de memória é um aspecto chave da otimização da execução de trabalhos do Spark.  Há várias técnicas que podem ser aplicadas para usar a memória do cluster de maneira eficiente.

* Preferir partições de dados menores e considerar o tamanho, os tipos e a distribuição de dados na sua estratégia de particionamento.
* Considerar a [serialização de dados Kryo](https://github.com/EsotericSoftware/kryo), mais recente e eficiente, em vez da serialização Java padrão.
* Preferir utilizar o YARN, pois separa `spark-submit` em lotes.
* Monitorar e sintonizar as definições da configuração do Spark.

Para sua referência, a estrutura da memória do Spark e alguns parâmetros da memória do executor chave são mostrados na próxima imagem.

### <a name="spark-memory-considerations"></a>Considerações de memória do Spark

Se você estiver usando [Apache HADOOP yarn](https://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html), yarn controlará a soma máxima da memória usada por todos os contêineres em cada nó do Spark.  O diagrama a seguir mostra os objetos de chave e os respectivos relacionamentos.

![Gerenciamento de memória YARN do Spark](./media/apache-spark-perf/apache-yarn-spark-memory.png)

Para endereçar mensagens de "memória insuficiente", tente:

* Revisar as ordens aleatórias de armazenamento do DAG. Reduzir a redução do lado do mapa, os dados de origem de pré-partição (ou fazer bucket), maximizar as ordens aleatórias simples e reduzir a quantidade de dados enviados.
* Preferir `ReduceByKey` com o limite de memória fixa para `GroupByKey`, que fornece agregações, janelas e outras funções, mas tem limite de memória ilimitado.
* Preferir `TreeReduce`, que faz mais trabalho nos executores ou partições, para `Reduce`, que faz todo o trabalho no driver.
* Aproveitar os Conjuntos de Dados em vez dos objetos RDD de nível inferior.
* Criar ComplexTypes que encapsulam ações, como "Top N", várias agregações ou operações de janelas.

Para obter etapas de solução de problemas adicionais, consulte [OutOfMemoryError Exceptions for Apache Spark no Azure HDInsight](apache-spark-troubleshoot-outofmemory.md).

## <a name="optimize-data-serialization"></a>Otimizar a serialização de dados

Os trabalhos do Spark são distribuídos, por isso a serialização de dados apropriada é importante para obter o melhor desempenho.  Há duas opções de serialização para Spark:

* A serialização Java é o padrão.
* A serialização Kryo é um formato mais recente e pode resultar em serialização mais rápida e compacta do que Java.  O kryo exige que você registre as classes em seu programa e ainda não oferece suporte a todos os tipos serializáveis.

## <a name="use-bucketing"></a>Usar bucketing

O bucketing é semelhante ao particionamento de dados, mas cada bucket pode conter um conjunto de valores de colunas em vez de apenas um. O Bucketing funciona bem para particionamento em números de valores grandes (em milhões ou mais), como identificadores de produto. Um bucket é determinado por hash e chave de bucket da linha. As tabelas em bucket oferecem otimizações exclusivas porque armazenam metadados como foram particionados e classificados.

Alguns recursos de bucket avançados são:

* Otimização de consulta baseada em informações meta de bucket.
* Agregações otimizadas.
* Junções otimizadas.

É possível utilizar particionamento e bucket ao mesmo tempo.

## <a name="optimize-joins-and-shuffles"></a>Ordens aleatórias e junções otimizadas

Se houver trabalhos lentos em uma Junção ou Ordem Aleatória, provavelmente a causa será a *distorção de dados*, que é a assimetria nos dados de trabalho. Por exemplo, um trabalho de mapa pode levar 20 segundos, mas executar um trabalho onde os dados são unidos ou em ordem aleatória leva horas. Para corrigir a distorção de dados, será necessário usar salt da chave inteira ou um *salt isolado* para apenas um subconjunto de chaves. Se você estiver usando um Salt isolado, deverá filtrar ainda mais para isolar seu subconjunto de chaves com Salt em junções de mapa. Outra opção é primeiro introduzir uma coluna de bucket e pré-agregado em buckets.

Outro fator que causa junções lentas pode ser o tipo de junção. Por padrão, o Spark usa o tipo de junção `SortMerge`. Este tipo de junção é mais adequado para conjuntos de dados grandes, mas, de outra forma, é computacionalmente caro porque primeiro deve classificar os lados esquerdo e direito dos dados antes de mesclá-los.

Uma junção `Broadcast` é mais adequada para conjuntos de dados menores ou, onde um lado da junção é muito menor do que o outro lado. Esse tipo de junção difunde um lado para todos os executores e, portanto, requer mais memória para difusões em geral.

É possível alterar o tipo de junção na configuração, definindo `spark.sql.autoBroadcastJoinThreshold` ou você pode definir uma dica de junção usando as APIs do DataFrame (`dataframe.join(broadcast(df2))`).

```scala
// Option 1
spark.conf.set("spark.sql.autoBroadcastJoinThreshold", 1*1024*1024*1024)

// Option 2
val df1 = spark.table("FactTableA")
val df2 = spark.table("dimMP")
df1.join(broadcast(df2), Seq("PK")).
    createOrReplaceTempView("V_JOIN")

sql("SELECT col1, col2 FROM V_JOIN")
```

Se você estiver usando tabelas segmentadas, terá um terceiro tipo de junção, a `Merge` join. Um conjunto de dados corretamente pré-particionado e pré-classificado ignorará a fase de classificação cara de uma junção `SortMerge`.

A ordem de questões de junção, particularmente em consultas mais complexas. Inicie com as junções mais seletivas. Além disso, mova junções que aumentam o número de linhas após as agregações, quando possível.

Para gerenciar o paralelismo para junções cartesianas, você pode adicionar estruturas aninhadas, janelas e, talvez, ignorar uma ou mais etapas em seu trabalho do Spark.

## <a name="customize-cluster-configuration"></a>Personalizar a configuração do cluster

Dependendo da carga de trabalho do cluster Spark, você poderá determinar que uma configuração Spark não padrão resulte em execução de trabalho do Spark mais otimizada.  Execute testes de benchmark com cargas de trabalho de exemplo para validar quaisquer configurações de cluster não padrão.

Aqui, são apresentado alguns parâmetros comuns que podem ser ajustados:

* `--num-executors` define o número apropriado de executores.
* `--executor-cores` define o número de núcleos para cada executor. Normalmente, você deverá ter executores de tamanho médio, pois outros processos consomem parte da memória disponível.
* `--executor-memory` define o tamanho da memória para cada executor, que controla o tamanho de heap no YARN. É necessário deixar alguma memória para sobrecarga de execução.

### <a name="select-the-correct-executor-size"></a>Selecionar o tamanho correto de executor

Ao decidir a configuração de executor, considere a sobrecarga de coleta de lixo Java (GC).

* Fatores para reduzir o tamanho do executor:
    1. Reduzir o tamanho do heap abaixo de 32 GB para manter a sobrecarga de GC < 10%.
    2. Reduzir o número de núcleos para manter a sobrecarga de GC < 10%.

* Fatores para aumentar o tamanho do executor:
    1. Reduzir a sobrecarga de comunicação entre os executores.
    2. Reduzir o número de conexões abertas entre executores (N2) em clusters maiores (> 100 executores).
    3. Aumentar o tamanho do heap para acomodar tarefas intensivas de memória.
    4. Opcional: reduzir a sobrecarga da memória por executor.
    5. Opcional: aumentar a utilização e a simultaneidade ao sobrecarregar a CPU.

Como regra geral, ao selecionar o tamanho do executor:

1. Inicie com 30 GB por executor e distribua núcleos de computadores disponíveis.
2. Aumente o número de núcleos de executor para clusters maiores (> 100 executores).
3. Modifique o tamanho com base nas execuções de avaliação e nos fatores anteriores, como a sobrecarga de GC.

Ao executar consultas simultâneas, considere o seguinte:

1. Inicie com 30 GB por executor e todos os núcleos de computadores.
2. Crie várias aplicações do Spark paralelas com subscrição excessiva de CPU (cerca de 30% de melhoria de latência).
3. Distribua consultas em aplicativos paralelos.
4. Modifique o tamanho com base nas execuções de avaliação e nos fatores anteriores, como a sobrecarga de GC.

Para obter mais informações sobre como usar o Ambari para configurar executores, consulte [configurações de Apache Spark-executores do Spark](apache-spark-settings.md#configuring-spark-executors).

Monitore o desempenho da consulta para verificar exceções ou outros problemas de desempenho, observando a exibição da linha do tempo, o gráfico SQL, as estatísticas de trabalho e, assim por diante. Para obter informações sobre como depurar trabalhos do Spark usando o YARN e o servidor de histórico do Spark, consulte [depurar Apache Spark trabalhos em execução no Azure HDInsight](apache-spark-job-debugging.md). Para obter dicas sobre como usar o servidor de linha do tempo YARN, confira [acessar logs de aplicativo do YARN Apache Hadoop](../hdinsight-hadoop-access-yarn-app-logs-linux.md).

Às vezes, um ou alguns dos executores são mais lentos que os outros e as tarefas demoram muito mais tempo para serem executadas. Isso geralmente ocorre em clusters maiores (> 30 nós). Nesse caso, divida o trabalho em um número maior de tarefas para que o agendador possa compensar tarefas lentas. Por exemplo, tenha pelo menos duas vezes mais tarefas que o número de núcleos de executor no aplicativo. Você também pode habilitar a execução especulativa de tarefas com `conf: spark.speculation = true`.

## <a name="optimize-job-execution"></a>Otimizar a execução do trabalho

* Armazene em cache conforme necessário, por exemplo, se você utilizar os dados duas vezes, armazene-os em cache.
* Difundir variáveis a todos os executores. As variáveis são serializadas apenas uma vez, resultando em pesquisas mais rápidas.
* Use o pool de threads no driver, o que resulta em operação mais rápida para muitas tarefas.

Monitore regularmente os trabalhos em execução verificar se há problemas de desempenho. Se precisar de mais informações sobre certos problemas, considere uma das seguintes ferramentas de criação de perfil de desempenho:

* [Ferramenta Intel PAL](https://github.com/intel-hadoop/PAT) monitora a CPU, o armazenamento e a utilização da largura de banda da rede.
* Perfis de [Controle de Missão Oracle Java 8](https://www.oracle.com/technetwork/java/javaseproducts/mission-control/java-mission-control-1998576.html) do Spark e código de executor.

A chave para desempenho de consultas do Spark 2.x é o mecanismo Tungsten, que depende da geração de código de estágio inteiro. Em alguns casos, a geração de código de estágio inteiro pode ser desabilitada. Por exemplo, se você utilizar um tipo não mutável (`string`) na expressão de agregação, `SortAggregate` aparecerá em vez de `HashAggregate`. Por exemplo, para um melhor desempenho, tente o seguinte e, em seguida, habilite novamente a geração de código:

```sql
MAX(AMOUNT) -> MAX(cast(AMOUNT as DOUBLE))
```

## <a name="next-steps"></a>Próximas etapas

* [Depurar trabalhos do Apache Spark em execução no Azure HDInsight](apache-spark-job-debugging.md)
* [Gerenciar recursos para um cluster do Apache Spark no HDInsight](apache-spark-resource-manager.md)
* [Use a API REST do Apache Spark para enviar trabalhos remotos para um cluster do Apache Spark](apache-spark-livy-rest-interface.md)
* [Ajuste do Apache Spark](https://spark.apache.org/docs/latest/tuning.html)
* [Como realmente ajustar o funcionamento dos trabalhos do Apache Spark](https://www.slideshare.net/ilganeli/how-to-actually-tune-your-spark-jobs-so-they-work)
* [Serialização Kryo](https://github.com/EsotericSoftware/kryo)

---
title: Ingestão de dados do Azure Data Explorer
description: Saiba mais sobre as diferentes maneiras de ingerir (carregar) com os dados no Azure Data Explorer
author: orspod
ms.author: orspodek
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
ms.date: 02/18/2019
ms.openlocfilehash: 4846a19c403cce16bed704ed4e7c70499f3b5d13
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79246390"
---
# <a name="azure-data-explorer-data-ingestion"></a>Ingestão de dados do Azure Data Explorer

A ingestão de dados é o processo usado para carregar os registros de dados de uma ou mais fontes para criar ou atualizar uma tabela no Azure Data Explorer. Depois de ingeridos, os dados ficam disponíveis para consulta. O diagrama a seguir mostra o fluxo de ponta a ponta para trabalhar no Azure Data Explorer, incluindo a ingestão de dados.

![Fluxo de dados](media/ingest-data-overview/data-flow.png)

O serviço de gerenciamento de dados do Azure Data Explorer, que é responsável pela ingestão de dados, fornece a seguinte funcionalidade:

1. **Pull de dados**: extrai dados de fontes externas (Hubs de Eventos) ou de solicitações de ingestão de leitura de uma Fila do Azure.

1. **Envio em lote**: dados em lote que fluem para o mesmo banco de dados e tabela para otimizar a produtividade da ingestão do lote.

1. **Validação**: Validação preliminar e conversão de formato, se necessário.

1. **Manipulação de dados**: correspondência de esquema, organização, indexação, codificação e compactação dos dados.

1. **Ponto de persistência no fluxo de ingestão**: gerenciar a carga de ingestão no mecanismo e lidar com novas tentativas após falhas transitórias.

1. **Confirmar a ingestão de dados**: disponibiliza os dados para consulta.

## <a name="ingestion-methods"></a>Métodos de ingestão

O Azure Data Explorer dá suporte a vários métodos de ingestão, cada um com seus próprios cenários de destino, vantagens e desvantagens. O Azure Data Explorer oferece pipelines e conectores para serviços comuns, ingestão programática usando SDKs e acesso direto ao mecanismo para fins de exploração.

### <a name="ingestion-using-pipelines-connectors-and-plugins"></a>Ingestão usando pipelines, conectores e plug-ins

O Azure Data Explorer atualmente dá suporte ao seguinte:

* Pipeline de Grade de Eventos, que pode ser gerenciado usando o assistente de gerenciamento no portal do Azure. Para saber mais informações, confira [Ingerir Blobs do Azure no Azure Data Explorer](ingest-data-event-grid.md).

* Pipeline de Hub de Eventos, que pode ser gerenciado usando o assistente de gerenciamento no portal do Azure. Para obter mais informações, confira [Ingerir dados do Hub de Eventos no Azure Data Explorer](ingest-data-event-hub.md).

* Plug-in Logstash, confira [Ingerir dados do Logstash para o Azure Data Explorer](ingest-data-logstash.md).

* Conector Kafka, confira [Ingerir dados do Kafka no Azure Data Explorer](ingest-data-kafka.md).

### <a name="ingestion-using-integration-services"></a>Ingestão usando os serviços de integração

* A Azure Data Factory (ADF), um serviço de integração de dados totalmente gerenciado para cargas de trabalho analíticas no Azure, para copiar dados de e para o Azure Data Explorer usando [armazenamentos e formatos de dados suportados](/azure/data-factory/copy-activity-overview#supported-data-stores-and-formats). Para obter mais informações, consulte [Copiar dados da Fábrica de Dados do Azure para o Azure Data Explorer](/azure/data-explorer/data-factory-load-data).

### <a name="programmatic-ingestion"></a>Ingestão programática

O Azure Data Explorer fornece SDKs que podem ser usados para consulta e ingestão de dados. A ingestão programática é otimizada para redução de custos de ingestão (COGs), minimizando as transações de armazenamento durante e após o processo de ingestão.

**Projetos de software livre e SDKs disponíveis**:

O Kusto oferece um SDK cliente que pode ser usado para ingerir e consultar dados:

* [SDK do Python](/azure/kusto/api/python/kusto-python-client-library)

* [.NET SDK](/azure/kusto/api/netfx/about-the-sdk)

* [Java SDK](/azure/kusto/api/java/kusto-java-client-library)

* [SDK do Node](/azure/kusto/api/node/kusto-node-client-library)

* [REST API](/azure/kusto/api/netfx/kusto-ingest-client-rest)

**Técnicas programáticas de ingestão**:

* Ingestão de dados por meio do serviço de gerenciamento de dados do Azure Data Explorer (ingestão confiável e de alta produtividade):

    [**Ingestão em lote**](/azure/kusto/api/netfx/kusto-ingest-queued-ingest-sample) (fornecida pelo SDK): o cliente carrega os dados no Armazenamento de Blobs do Azure (designado pelo serviço de gerenciamento de dados do Azure Data Explorer) e envia uma notificação a uma Fila do Azure. A ingestão em lote é a técnica recomendada para ingestão de dados de alto volume, confiáveis e baratos.

* Ingestão de dados direta no mecanismo do Azure Data Explorer (mais apropriada para exploração e criação de protótipos):

  * **Ingestão inline**: comando de controle (.ingerir inline) contendo dados em banda destina-se a fins de teste ad hoc.

  * **Ingestão de consulta**: o comando de controle (.set, .set-or-append, .set-or-replace) que aponta para os resultados da consulta é usado para a geração de relatórios ou tabelas temporárias pequenas.

  * **Ingestão de armazenamento**: o comando de controle (.ingest into) com os dados armazenados externamente (por exemplo, Armazenamento de Blobs do Azure) permite a ingestão eficiente de dados em massa.

**Latência de métodos diferentes**:

| Método | Latency |
| --- | --- |
| **Ingestão embutida** | Imediata |
| **Ingestão de consulta** | Tempo de consulta + tempo de processamento |
| **Ingestão de armazenamento** | Tempo de download + tempo de processamento |
| **Ingestão na fila** | Tempo de envio em lote + tempo de processamento |
| |

O tempo de processamento depende do tamanho dos dados e é de poucos segundos. O padrão de tempo do envio em lote é de cinco minutos.

## <a name="choosing-the-most-appropriate-ingestion-method"></a>Escolhendo o método de ingestão mais apropriado

Antes de iniciar a ingestão de dados, você deve se perguntar o seguinte.

* Onde meus dados residem? 
* Qual é o formato dos dados e ele pode ser alterado? 
* Quais são os campos obrigatórios a serem consultados? 
* Qual é o volume de dados e a velocidade que se espera? 
* Quantos tipos de evento se esperada (refletidos como o número de tabelas)? 
* Qual a frequência esperada para alterações do esquema de eventos? 
* Quantos nós gerarão os dados? 
* Qual é o sistema operacional de origem? 
* Quais são os requisitos de latência? 
* Um dos pipelines de ingestão gerenciados existentes pode ser usado? 

Para organizações com uma infra-estrutura existente baseada em um serviço de mensagens como Event Hub e IoT Hub, usar um conector é provavelmente a solução mais apropriada. A ingestão na fila é apropriada para grandes volumes de dados.

## <a name="supported-data-formats"></a>Formatos de dados com suporte

Para todos os métodos de ingestão diferentes da ingestão de consulta, formate os dados de modo que o Azure Data Explorer possa analisá-los. 
* Os formatos de dados suportados são: TXT, CSV, TSV, TSVE, PSV, SCSV, SOH, JSON (line-separated, multi-line), Avro, Orc e Parquet. 
* Suporta compactação ZIP e GZIP.

> [!NOTE]
> Quando os dados estão sendo ingeridos, os tipos de dados são inferidos com base nas colunas da tabela de destino. Se um registro estiver incompleto ou um campo não puder ser analisado como o tipo de dados necessário, as colunas da tabela correspondentes serão preenchidas com valores nulos.

## <a name="ingestion-recommendations-and-limitations"></a>Limitações e recomendações de ingestão

* A política de retenção efetiva de dados ingeridos é derivada da política de retenção do banco de dados. Veja [política de retenção](/azure/kusto/concepts/retentionpolicy) para obter detalhes. A ingestão de dados requer as permissões **Gestor de tabela** ou **Gestor de banco de dados**.
* A ingestão dá suporte para um tamanho do arquivo máximo de 5 GB. A recomendação é ingerir arquivos entre 100 MB e 1 GB.

## <a name="schema-mapping"></a>Mapeamento de esquema

O mapeamento de esquema ajuda a associar os campos de dados de origem às colunas da tabela de destino.

* O [mapeamento de CSV](/azure/kusto/management/mappings?branch=master#csv-mapping) (opcional) funciona com todos os formatos com base em classificação ordinal. Ele pode ser realizado usando o parâmetro do comando de ingestão ou [pré-criado na tabela](/azure/kusto/management/create-ingestion-mapping-command) e referenciado pelo parâmetro do comando de ingestão.
* O [Mapeamento de JSON](/azure/kusto/management/mappings?branch=master#json-mapping) (obrigatório) e o [Mapeamento de Avro](/azure/kusto/management/mappings?branch=master#avro-mapping) (obrigatório) podem ser executados usando o parâmetro do comando de ingestão. Eles também podem ser [previamente criados na tabela](/azure/kusto/management/create-ingestion-mapping-command) e referenciados no parâmetro do comando de ingestão.

## <a name="next-steps"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Ingerir dados do Hub de Eventos no Azure Data Explorer](ingest-data-event-hub.md)

> [!div class="nextstepaction"]
> [Ingerir dados usando a assinatura de Grade de Eventos no Azure Data Explorer](ingest-data-event-grid.md)

> [!div class="nextstepaction"]
> [Ingerir dados de Kafka no Azure Data Explorer](ingest-data-kafka.md)

> [!div class="nextstepaction"]
> [Ingerir dados usando a biblioteca Python do Azure Data Explorer](python-ingest-data.md)

> [!div class="nextstepaction"]
> [Ingerir dados usando a biblioteca Dode do Explorador de Dados do Azure](node-ingest-data.md)

> [!div class="nextstepaction"]
> [Ingerir dados usando o SDK do .NET Standard do Azure Data Explorer (Versão prévia)](net-standard-ingest-data.md)

> [!div class="nextstepaction"]
> [ingerir dados do Logstash para o Azure Data Explorer](ingest-data-logstash.md)

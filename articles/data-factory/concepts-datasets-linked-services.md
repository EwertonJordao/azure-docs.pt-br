---
title: Conjunto de dados
description: Saiba mais sobre conjuntos de dados no Data Factory. Os conjuntos de dados representam os dados de entrada/saída.
services: data-factory
documentationcenter: ''
author: djpmsft
ms.author: daperlov
manager: jroth
ms.reviewer: maghan
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 04/25/2019
ms.openlocfilehash: 122725bff616a49d27981b88f465e04418db9526
ms.sourcegitcommit: 0b80a5802343ea769a91f91a8cdbdf1b67a932d3
ms.contentlocale: pt-BR
ms.lasthandoff: 05/25/2020
ms.locfileid: "83826104"
---
# <a name="datasets-in-azure-data-factory"></a>Conjuntos de dados no Azure Data Factory
> [!div class="op_single_selector" title1="Selecione a versão do serviço Data Factory que você está usando:"]
> * [Versão 1](v1/data-factory-create-datasets.md)
> * [Versão atual](concepts-datasets-linked-services.md)

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]


Este artigo descreve o que são conjuntos de dados, como eles são definidos no formato JSON e como são usados em pipelines do Azure Data Factory.

Se estiver conhecendo o Azure Data Factory agora, consulte [Introdução ao Azure Data Factory](introduction.md) para obter uma visão geral.

## <a name="overview"></a>Visão geral
Uma fábrica de dados pode ter um ou mais pipelines. Um **pipeline** é um agrupamento lógico de **atividades** que juntas executam uma tarefa. As atividades em um pipeline definem ações para executar em seus dados. Por outro lado, um **conjunto de dados** é uma exibição nomeada de dados que simplesmente aponta ou faz referência aos dados que você deseja usar em suas **atividades** como entradas e saídas. Conjuntos de dados identificam dados em armazenamentos de dados diferentes, como tabelas, arquivos, pastas e documentos. Por exemplo, um conjunto de dados de Blob do Azure especifica o contêiner de blobs e a pasta no armazenamento de Blobs dos quais a atividade deve ler os dados.

Antes de criar um conjunto de dados, crie um [**serviço vinculado**](concepts-linked-services.md) para vincular seu armazenamento de dados ao data factory. Serviços vinculados são como cadeias de conexão, que definem as informações de conexão necessárias para o Data Factory para se conectar a recursos externos. Pense dessa maneira: o conjunto de dados representa a estrutura dos dados nos armazenamentos de dados vinculados e o serviço vinculado define a conexão à fonte de dados. Por exemplo, um serviço vinculado do Armazenamento do Azure vincula uma conta de armazenamento ao data factory. Um conjunto de dados de Blob do Azure representa o contêiner de blob e a pasta na conta de Armazenamento do Azure que contém os blobs de entrada a serem processados.

Veja abaixo um cenário de exemplo. Para copiar dados do armazenamento de Blob em um Banco de Dados SQL, crie dois serviços vinculados: Armazenamento do Azure e Banco de Dados SQL do Azure. Em seguida, crie dois conjuntos de dados: O conjunto de dados de Blob do Azure (que se refere ao serviço vinculado do Armazenamento do Azure) e o conjunto de dados de Tabela do SQL do Azure (que se refere ao serviço vinculado do Banco de Dados SQL do Azure). Os serviços vinculados do Armazenamento do Azure e do Banco de Dados SQL do Azure contêm cadeias de conexão que o Data Factory usa em runtime para se conectar ao Armazenamento do Azure e ao Banco de Dados SQL do Azure, respectivamente. O conjunto de dados de Blob do Azure especifica o contêiner de blobs e a pasta de blobs que contém os blobs de entrada no armazenamento de Blobs. O conjunto de dados de Tabela do SQL do Azure especifica a tabela do SQL no Banco de Dados SQL no qual os dados serão copiados.

O seguinte diagrama mostra a relação entre pipeline, atividade, conjunto de dados e serviço vinculado no Data Factory:

![Relação entre pipeline, atividade e conjunto de dados, serviços vinculados](media/concepts-datasets-linked-services/relationship-between-data-factory-entities.png)


## <a name="dataset-json"></a>Conjunto de dados do JSON
Um conjunto de dados no Data Factory é definido no formato JSON a seguir:

```json
{
    "name": "<name of dataset>",
    "properties": {
        "type": "<type of dataset: AzureBlob, AzureSql etc...>",
        "linkedServiceName": {
                "referenceName": "<name of linked service>",
                "type": "LinkedServiceReference",
        },
        "structure": [
            {
                "name": "<Name of the column>",
                "type": "<Name of the type>"
            }
        ],
        "typeProperties": {
            "<type specific property>": "<value>",
            "<type specific property 2>": "<value 2>",
        }
    }
}
```
A tabela a seguir descreve as propriedades no JSON acima:

Propriedade | Descrição | Obrigatório |
-------- | ----------- | -------- |
name | Nome do conjunto de dados. Consulte [Azure Data Factory – Regras de nomenclatura](naming-rules.md). |  Sim |
type | Tipo de conjunto de dados. Especifique um dos tipos compatíveis com o Data Factory (por exemplo: AzureBlob, AzureSqlTable). <br/><br/>Para obter detalhes, consulte [Tipos de conjunto de dados](#dataset-type). | Sim |
estrutura | Esquema do conjunto de dados. Para obter detalhes, veja [Esquema de conjunto de dados](#dataset-structure-or-schema). | Não |
typeProperties | As propriedades de tipo são diferentes para cada tipo (por exemplo: Blob do Azure, tabela do SQL Azure). Para obter detalhes sobre os tipos com suporte e suas propriedades, consulte [Tipo de conjunto de dados](#dataset-type). | Sim |

### <a name="data-flow-compatible-dataset"></a>Conjunto de dados compatíveis com o Fluxo de Dados



Veja [tipos de conjunto de dados com suporte](#dataset-type) para obter uma lista de tipos de conjuntos de dado que são compatíveis com o [Fluxo de Dados](concepts-data-flow-overview.md). Os conjuntos de dados que são compatíveis com o Fluxo de Dados exigem definições refinadas de conjunto de dados para transformações. Portanto, a definição de JSON é ligeiramente diferente. Em vez de uma propriedade _structure_, os conjuntos de dados que são compatíveis com o Fluxo de Dados têm uma propriedade _schema_.

No Fluxo de Dados, os conjuntos de dados são usados em transformações da fonte e do coletor. Os conjuntos de dados definem os esquemas de dados básicos. Se os dados não tiverem um esquema, use o descompasso de esquema para a fonte e o coletor. O esquema no conjunto de dados representa o tipo e a forma de dados físicos.

Quando define o esquema no conjunto de dados, você terá os respectivos tipos de dados, formatos de dados, localização do arquivo e informações de conexão do serviço vinculado associado. Os metadados dos conjuntos de dados aparecem na transformação da fonte como a projeção da *fonte*. A projeção na transformação da fonte representa os dados do Fluxo de Dados com nomes e tipos definidos.

Quando importar o esquema de um conjunto de dados do Fluxo de Dados, selecione o botão **Importar esquema** e importe da fonte ou de um arquivo local. Na maioria dos casos, você importará o esquema diretamente da fonte. Mas se já tiver um arquivo de esquema local (um arquivo Parquet ou CSV com cabeçalhos), você poderá instruir o Data Factory a basear-se no esquema nesse arquivo.


```json
{
    "name": "<name of dataset>",
    "properties": {
        "type": "<type of dataset: AzureBlob, AzureSql etc...>",
        "linkedServiceName": {
                "referenceName": "<name of linked service>",
                "type": "LinkedServiceReference",
        },
        "schema": [
            {
                "name": "<Name of the column>",
                "type": "<Name of the type>"
            }
        ],
        "typeProperties": {
            "<type specific property>": "<value>",
            "<type specific property 2>": "<value 2>",
        }
    }
}
```

A tabela a seguir descreve as propriedades no JSON acima:

Propriedade | Descrição | Obrigatório |
-------- | ----------- | -------- |
name | Nome do conjunto de dados. Consulte [Azure Data Factory – Regras de nomenclatura](naming-rules.md). |  Sim |
type | Tipo de conjunto de dados. Especifique um dos tipos compatíveis com o Data Factory (por exemplo: AzureBlob, AzureSqlTable). <br/><br/>Para obter detalhes, consulte [Tipos de conjunto de dados](#dataset-type). | Sim |
esquema | Esquema do conjunto de dados. Para obter detalhes, veja [Conjuntos de dados compatíveis com o Fluxo de Dados](#dataset-type). | Não |
typeProperties | As propriedades de tipo são diferentes para cada tipo (por exemplo: Blob do Azure, tabela do SQL Azure). Para obter detalhes sobre os tipos com suporte e suas propriedades, consulte [Tipo de conjunto de dados](#dataset-type). | Sim |


## <a name="dataset-example"></a>Exemplo de conjunto de dados
No exemplo a seguir, o conjunto de dados representa uma tabela chamada MyTable em um Banco de Dados SQL.

```json
{
    "name": "DatasetSample",
    "properties": {
        "type": "AzureSqlTable",
        "linkedServiceName": {
                "referenceName": "MyAzureSqlLinkedService",
                "type": "LinkedServiceReference",
        },
        "typeProperties":
        {
            "tableName": "MyTable"
        },
    }
}

```
Observe os seguintes pontos:

- O tipo foi definido como AzureSqlTable.
- A propriedade de tipo tableName (específica do tipo AzureSqlTable) foi definida como MyTable.
- linkedServiceName se refere a um serviço vinculado do tipo AzureSqlDatabase, que é definido no seguinte snippet de JSON.

## <a name="dataset-type"></a>Tipo de conjunto de dados
Há muitos tipos diferentes de conjuntos de dados, dependendo do armazenamento de dados que você usa. Veja a lista de armazenamentos de dados compatíveis com o Data Factory no artigo [Visão geral do conector](connector-overview.md). Clique em um armazenamento de dados para saber como criar um serviço vinculado e um conjunto de dados para esse armazenamento de dados.

No exemplo na seção anterior, o tipo do conjunto de dados é definido como **AzureSqlTable**. Da mesma forma, para um conjunto de dados de Blob do Azure, o tipo do conjunto de dados é definido como **AzureBlob**, conforme mostrado no seguinte JSON:

```json
{
    "name": "AzureBlobInput",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": {
                "referenceName": "MyAzureStorageLinkedService",
                "type": "LinkedServiceReference",
        },

        "typeProperties": {
            "fileName": "input.log",
            "folderPath": "adfgetstarted/inputdata",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
            }
        }
    }
}
```

## <a name="dataset-structure-or-schema"></a>Estrutura ou esquema do conjunto de dados
O conjunto de dados da seção **Estrutura** ou da seção **Esquema** (compatível com o Fluxo de Dados) são opcionais. Ela define o esquema do conjunto de dados contendo uma coleção de nomes e tipos de dados de colunas. Use a seção de estrutura para fornecer informações de tipo que são usadas para converter tipos e mapear colunas da origem para o destino.

Cada coluna da seção Estrutura contém as seguintes propriedades:

Propriedade | Descrição | Obrigatório
-------- | ----------- | --------
name | Nome da coluna. | Sim
type | Tipo de dados da coluna. O Data Factory dá suporte aos seguintes tipos de dados provisórios como valores permitidos: **Int16, Int32, Int64, Single, Double, Decimal, Byte[], Boolean, String, Guid, Datetime, Datetimeoffset e Timespan** | Não
culture | Cultura baseada em .NET a ser usada quando o tipo é um tipo .NET `Datetime` ou `Datetimeoffset`. O padrão é `en-us`. | Não
format | O formato de cadeia de caracteres a ser usado quando o tipo é um tipo .NET `Datetime` ou `Datetimeoffset`. Consulte [Data personalizada e cadeias de caracteres de formato de hora](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings) sobre como formatar a data e hora. | Não

### <a name="example"></a>Exemplo
No exemplo a seguir, suponha que os dados de Blob de origem estão em formato CSV e contém três colunas: ID do usuário, nome e data do último logon. Eles são do tipo Int64, cadeia de caracteres e data e hora com um formato de data e hora personalizado usando nomes abreviados em francês para o dia da semana.

Defina a estrutura do conjunto de dados de Blob conforme demonstrado a seguir, juntamente com definições de tipo para as colunas:

```json
"structure":
[
    { "name": "userid", "type": "Int64"},
    { "name": "name", "type": "String"},
    { "name": "lastlogindate", "type": "Datetime", "culture": "fr-fr", "format": "ddd-MM-YYYY"}
]
```

### <a name="guidance"></a>Orientação

As diretrizes a seguir ajudam você a compreender quando incluir informações de estrutura e o que incluir na seção **estrutura**. Saiba mais sobre como o data factory mapeia dados de origem até o coletor e quando especificar informações de estrutura do [Esquema e mapeamento de tipo](copy-activity-schema-and-type-mapping.md).

- **Para fontes de dados esquema forte**, especifique a seção de estrutura somente se desejar mapear colunas de origem para colunas do coletor; além disso, seus nomes não são os mesmos. Esse tipo de fonte de dados estruturada armazena informações de esquema de dados e de tipo juntamente com os próprios dados. Exemplos de fontes de dados estruturadas incluem SQL Server, Oracle e Banco de Dados SQL do Azure.<br/><br/>Como as informações de tipo já estão disponíveis para fontes de dados estruturadas, não inclua informações de tipo quando incluir a seção de estrutura.
- **Para fontes de dados de esquema fraco/sem esquema, por exemplo, arquivo de texto no armazenamento de blob**, inclua a estrutura quando o conjunto de dados for uma entrada para uma atividade de cópia, e os tipos de dados do conjunto de dados da fonte precisarem ser convertidos em tipos nativos para o coletor. E também inclua a estrutura quando quiser mapear as colunas da fonte para as colunas do coletor

## <a name="create-datasets"></a>Criar conjuntos de dados
Você pode criar conjuntos de dados usando uma dessas ferramentas ou SDKs: [API do .NET](quickstart-create-data-factory-dot-net.md), [PowerShell](quickstart-create-data-factory-powershell.md), [API REST](quickstart-create-data-factory-rest-api.md), modelo do Azure Resource Manager e Portal do Azure

## <a name="current-version-vs-version-1-datasets"></a>Conjuntos de dados da versão atual vs. versão 1

A seguir, são apresentadas algumas diferenças entre os conjuntos de dados do Data Factory e Data Factory versão 1:

- A propriedade externa não é tem suporte na versão atual. Ela é substituída por um [gatilho](concepts-pipeline-execution-triggers.md).
- Não há suporte para as propriedades de disponibilidade e política na versão atual. A hora de início de um pipeline depende de [gatilhos](concepts-pipeline-execution-triggers.md).
- Não há suporte para conjuntos de dados com escopo (conjuntos de dados definidos em um pipeline) na versão atual.

## <a name="next-steps"></a>Próximas etapas
Consulte os seguintes tutoriais para obter instruções passo a passo para criar pipelines e conjuntos de dados usando uma destas ferramentas ou SDKs.

- [Início rápido: criar um data factory usando o .NET](quickstart-create-data-factory-dot-net.md)
- [Início rápido: criar um data factory usando o PowerShell](quickstart-create-data-factory-powershell.md)
- [Início rápido: criar um data factory usando a API REST](quickstart-create-data-factory-rest-api.md)
- [Início rápido: criar um data factory usando o portal do Azure](quickstart-create-data-factory-portal.md)

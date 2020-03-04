---
title: Criar uma conexão de dados de grade de eventos para o Azure Data Explorer usandoC#
description: Neste artigo, você aprenderá a criar uma conexão de dados de grade de eventos para o Azure C#data Explorer usando o.
author: lucygoldbergmicrosoft
ms.author: lugoldbe
ms.reviewer: orspodek
ms.service: data-explorer
ms.topic: conceptual
ms.date: 10/07/2019
ms.openlocfilehash: 03963f60cc364dd36ad55c0a28e92e3b585bb38d
ms.sourcegitcommit: d4a4f22f41ec4b3003a22826f0530df29cf01073
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/03/2020
ms.locfileid: "78255074"
---
# <a name="create-an-event-grid-data-connection-for-azure-data-explorer-by-using-c"></a>Criar uma conexão de dados de grade de eventos para o Azure Data Explorer usandoC#

> [!div class="op_single_selector"]
> * [Portal](ingest-data-event-grid.md)
> * [C#](data-connection-event-grid-csharp.md)
> * [Python](data-connection-event-grid-python.md)
> * [Modelo do Azure Resource Manager](data-connection-event-grid-resource-manager.md)


O Azure Data Explorer é um serviço de exploração de dados rápido e altamente escalonável para dados de log e telemetria. O Azure Data Explorer oferece ingestão (carregamento de dados) de hubs de eventos, hubs IoT e Blobs gravados em contêineres de BLOB. Neste artigo, você cria uma conexão de dados de grade de eventos para o Azure C#data Explorer usando o.

## <a name="prerequisites"></a>Prerequisites

* Se você não tiver o Visual Studio 2019 instalado, poderá baixar e usar o [Visual Studio 2019 Community Edition](https://www.visualstudio.com/downloads/)gratuito. Verifique se você habilitou o **desenvolvimento do Azure** durante a instalação do Visual Studio.
* Caso você não tenha uma assinatura do Azure, crie uma [conta gratuita do Azure](https://azure.microsoft.com/free/) antes de começar.
* Criar [um cluster e um banco de dados](create-cluster-database-csharp.md)
* Criar [mapeamento de tabela e coluna](net-standard-ingest-data.md#create-a-table-on-your-test-cluster)
* Definir [políticas de banco de dados e tabela](database-table-policies-csharp.md) (opcional)
* Crie uma [conta de armazenamento com uma assinatura de grade de eventos](ingest-data-event-grid.md#create-an-event-grid-subscription-in-your-storage-account).

[!INCLUDE [data-explorer-data-connection-install-nuget-csharp](../../includes/data-explorer-data-connection-install-nuget-csharp.md)]

[!INCLUDE [data-explorer-authentication](../../includes/data-explorer-authentication.md)]

## <a name="add-an-event-grid-data-connection"></a>Adicionar uma conexão de dados de grade de eventos

O exemplo a seguir mostra como adicionar uma conexão de dados de grade de eventos programaticamente. Consulte [criar uma conexão de dados de grade de eventos no Azure data Explorer](ingest-data-event-grid.md#create-an-event-grid-data-connection-in-azure-data-explorer) para adicionar uma conexão de dados de grade de eventos usando o portal do Azure.

```csharp
var tenantId = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx";//Directory (tenant) ID
var clientId = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx";//Application ID
var clientSecret = "xxxxxxxxxxxxxx";//Client Secret
var subscriptionId = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx";
var authenticationContext = new AuthenticationContext($"https://login.windows.net/{tenantId}");
var credential = new ClientCredential(clientId, clientSecret);
var result = await authenticationContext.AcquireTokenAsync(resource: "https://management.core.windows.net/", clientCredential: credential);

var credentials = new TokenCredentials(result.AccessToken, result.AccessTokenType);

var kustoManagementClient = new KustoManagementClient(credentials)
{
    SubscriptionId = subscriptionId
};

var resourceGroupName = "testrg";
//The cluster and database that are created as part of the Prerequisites
var clusterName = "mykustocluster";
var databaseName = "mykustodatabase";
var dataConnectionName = "myeventhubconnect";
//The event hub and storage account that are created as part of the Prerequisites
var eventHubResourceId = "/subscriptions/xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx/resourceGroups/xxxxxx/providers/Microsoft.EventHub/namespaces/xxxxxx/eventhubs/xxxxxx";
var storageAccountResourceId = "/subscriptions/xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx/resourceGroups/xxxxxx/providers/Microsoft.Storage/storageAccounts/xxxxxx";
var consumerGroup = "$Default";
var location = "Central US";
//The table and column mapping are created as part of the Prerequisites
var tableName = "StormEvents";
var mappingRuleName = "StormEvents_CSV_Mapping";
var dataFormat = DataFormat.CSV;

await kustoManagementClient.DataConnections.CreateOrUpdateAsync(resourceGroupName, clusterName, databaseName, dataConnectionName,
    new EventGridDataConnection(storageAccountResourceId, eventHubResourceId, consumerGroup, tableName: tableName, location: location, mappingRuleName: mappingRuleName, dataFormat: dataFormat));
```

|**Configuração** | **Valor sugerido** | **Descrição do campo**|
|---|---|---|
| tenantId | *xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx* | ID do locatário. Também conhecida como ID de diretório.|
| subscriptionId | *xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx* | A ID da assinatura que você usa para a criação de recursos.|
| clientId | *xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx* | A ID do cliente do aplicativo que pode acessar recursos em seu locatário.|
| clientSecret | *xxxxxxxxxxxxxx* | O segredo do cliente do aplicativo que pode acessar recursos em seu locatário. |
| resourceGroupName | *testrg* | O nome do grupo de recursos que contém o cluster.|
| clusterName | *mykustocluster* | O nome do cluster.|
| databaseName | *mykustodatabase* | O nome do banco de dados de destino no cluster.|
| dataconnectionname | *myeventhubconnect* | O nome desejado da sua conexão de dados.|
| tableName | *StormEvents* | O nome da tabela de destino no banco de dados de destino.|
| mappingRuleName | *StormEvents_CSV_Mapping* | O nome do mapeamento de coluna relacionado à tabela de destino.|
| Formato de DataFormat | *CSV* | O formato de dados da mensagem.|
| eventHubResourceId | *ID do recurso* | A ID de recurso do hub de eventos em que a grade de eventos está configurada para enviar eventos. |
| storageAccountResourceId | *ID do recurso* | A ID de recurso da sua conta de armazenamento que contém os dados para ingestão. |
| consumerGroup | *$Default* | O grupo de consumidores do hub de eventos.|
| local | *Centro dos EUA* | O local do recurso de conexão de dados.|

## <a name="generate-sample-data"></a>Gerar dados de exemplo

Agora que o Azure Data Explorer e a conta de armazenamento estão conectados, você pode criar dados de exemplo e fazer upload deles no Armazenamento de Blobs.

Esse script cria um novo contêiner na conta de armazenamento, carrega um arquivo existente (como um blob) nesse contêiner e, em seguida, lista os blobs no contêiner.

```csharp
var azureStorageAccountConnectionString=<storage_account_connection_string>;

var containerName=<container_name>;
var blobName=<blob_name>;
var localFileName=<file_to_upload>;

// Creating the container
var azureStorageAccount = CloudStorageAccount.Parse(azureStorageAccountConnectionString);
var blobClient = azureStorageAccount.CreateCloudBlobClient();
var container = blobClient.GetContainerReference(containerName);
container.CreateIfNotExists();

// Set metadata and upload file to blob
var blob = container.GetBlockBlobReference(blobName);
blob.Metadata.Add("rawSizeBytes", "4096‬"); // the uncompressed size is 4096 bytes
blob.Metadata.Add("kustoIngestionMappingReference", "mapping_v2‬");
blob.UploadFromFile(localFileName);

// List blobs
var blobs = container.ListBlobs();
```

> [!NOTE]
> O Azure Data Explorer não excluirá os BLOBs após a ingestão. Mantenha os blobs por três a cinco dias usando o [ciclo de vida do armazenamento de BLOBs do Azure](https://docs.microsoft.com/azure/storage/blobs/storage-lifecycle-management-concepts?tabs=azure-portal) para gerenciar a exclusão de BLOB.

[!INCLUDE [data-explorer-data-connection-clean-resources-csharp](../../includes/data-explorer-data-connection-clean-resources-csharp.md)]

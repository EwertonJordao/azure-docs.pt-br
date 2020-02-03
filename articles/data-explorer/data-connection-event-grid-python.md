---
title: Criar uma conexão de dados de grade de eventos para o Azure Data Explorer usando o Python
description: Neste artigo, você aprenderá a criar uma conexão de dados de grade de eventos para o Azure Data Explorer usando o Python.
author: lucygoldbergmicrosoft
ms.author: lugoldbe
ms.reviewer: orspodek
ms.service: data-explorer
ms.topic: conceptual
ms.date: 10/07/2019
ms.openlocfilehash: 9cf65edc3bdd5f675ba1972501139b9ecebcfafd
ms.sourcegitcommit: 42517355cc32890b1686de996c7913c98634e348
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/02/2020
ms.locfileid: "76964389"
---
# <a name="create-an-event-grid-data-connection-for-azure-data-explorer-by-using-python"></a>Criar uma conexão de dados de grade de eventos para o Azure Data Explorer usando o Python

> [!div class="op_single_selector"]
> * [Portal](ingest-data-event-grid.md)
> * [C#](data-connection-event-grid-csharp.md)
> * [Python](data-connection-event-grid-python.md)
> * [Modelo do Azure Resource Manager](data-connection-event-grid-resource-manager.md)

O Azure Data Explorer é um serviço de exploração de dados rápido e altamente escalonável para dados de log e telemetria. O Azure Data Explorer oferece ingestão (carregamento de dados) de hubs de eventos, hubs IoT e Blobs gravados em contêineres de BLOB. Neste artigo, você cria uma conexão de dados de grade de eventos para o Azure Data Explorer usando o Python.

## <a name="prerequisites"></a>Pré-requisitos

* Caso você não tenha uma assinatura do Azure, crie uma [conta gratuita do Azure](https://azure.microsoft.com/free/) antes de começar.
* Criar [um cluster e um banco de dados](create-cluster-database-python.md)
* Criar [mapeamento de tabela e coluna](python-ingest-data.md#create-a-table-on-your-cluster)
* Definir [políticas de banco de dados e tabela](database-table-policies-python.md) (opcional)
* Crie uma [conta de armazenamento com uma assinatura de grade de eventos](ingest-data-event-grid.md#create-an-event-grid-subscription-in-your-storage-account).

[!INCLUDE [data-explorer-data-connection-install-package-python](../../includes/data-explorer-data-connection-install-package-python.md)]

[!INCLUDE [data-explorer-authentication](../../includes/data-explorer-authentication.md)]

## <a name="add-an-event-grid-data-connection"></a>Adicionar uma conexão de dados de grade de eventos

O exemplo a seguir mostra como adicionar uma conexão de dados de grade de eventos programaticamente. Consulte [criar uma conexão de dados de grade de eventos no Azure data Explorer](ingest-data-event-grid.md#create-an-event-grid-data-connection-in-azure-data-explorer) para adicionar uma conexão de dados de grade de eventos usando o portal do Azure.


```Python
from azure.mgmt.kusto import KustoManagementClient
from azure.mgmt.kusto.models import EventGridDataConnection
from azure.common.credentials import ServicePrincipalCredentials

#Directory (tenant) ID
tenant_id = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx"
#Application ID
client_id = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx"
#Client Secret
client_secret = "xxxxxxxxxxxxxx"
subscription_id = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx"
credentials = ServicePrincipalCredentials(
        client_id=client_id,
        secret=client_secret,
        tenant=tenant_id
    )
kusto_management_client = KustoManagementClient(credentials, subscription_id)

resource_group_name = "testrg"
#The cluster and database that are created as part of the Prerequisites
cluster_name = "mykustocluster"
database_name = "mykustodatabase"
data_connection_name = "myeventhubconnect"
#The event hub and storage account that are created as part of the Prerequisites
event_hub_resource_id = "/subscriptions/xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx/resourceGroups/xxxxxx/providers/Microsoft.EventHub/namespaces/xxxxxx/eventhubs/xxxxxx"
storage_account_resource_id = "/subscriptions/xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx/resourceGroups/xxxxxx/providers/Microsoft.Storage/storageAccounts/xxxxxx"
consumer_group = "$Default"
location = "Central US"
#The table and column mapping that are created as part of the Prerequisites
table_name = "StormEvents"
mapping_rule_name = "StormEvents_CSV_Mapping"
data_format = "csv"

#Returns an instance of LROPoller, check https://docs.microsoft.com/python/api/msrest/msrest.polling.lropoller?view=azure-python
poller = kusto_management_client.data_connections.create_or_update(resource_group_name=resource_group_name, cluster_name=cluster_name, database_name=database_name, data_connection_name=data_connection_name,
                                            parameters=EventGridDataConnection(storage_account_resource_id=storage_account_resource_id, event_hub_resource_id=event_hub_resource_id, 
                                                                                consumer_group=consumer_group, table_name=table_name, location=location, mapping_rule_name=mapping_rule_name, data_format=data_format))
```
|**Configuração** | **Valor sugerido** | **Descrição do campo**|
|---|---|---|
| tenant_id | *xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx* | ID do locatário. Também conhecida como ID de diretório.|
| subscription_id | *xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx* | A ID da assinatura que você usa para a criação de recursos.|
| client_id | *xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx* | A ID do cliente do aplicativo que pode acessar recursos em seu locatário.|
| client_secret | *xxxxxxxxxxxxxx* | O segredo do cliente do aplicativo que pode acessar recursos em seu locatário. |
| resource_group_name | *testrg* | O nome do grupo de recursos que contém o cluster.|
| cluster_name | *mykustocluster* | O nome do cluster.|
| database_name | *mykustodatabase* | O nome do banco de dados de destino no cluster.|
| data_connection_name | *myeventhubconnect* | O nome desejado da sua conexão de dados.|
| table_name | *StormEvents* | O nome da tabela de destino no banco de dados de destino.|
| mapping_rule_name | *StormEvents_CSV_Mapping* | O nome do mapeamento de coluna relacionado à tabela de destino.|
| data_format | *CSV* | O formato de dados da mensagem.|
| event_hub_resource_id | *ID do recurso* | A ID de recurso do hub de eventos em que a grade de eventos está configurada para enviar eventos. |
| storage_account_resource_id | *ID do recurso* | A ID de recurso da sua conta de armazenamento que contém os dados para ingestão. |
| consumer_group | *$Default* | O grupo de consumidores do hub de eventos.|
| local | *Centro dos EUA* | O local do recurso de conexão de dados.|

[!INCLUDE [data-explorer-data-connection-clean-resources-python](../../includes/data-explorer-data-connection-clean-resources-python.md)]
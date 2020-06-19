---
title: Provisionar taxa de transferência de banco de dados no Azure Cosmos DB
description: Saiba como provisionar a taxa de transferência no nível de banco de dados no Azure Cosmos DB com o portal do Azure, a CLI, o PowerShell e vários outros SDKs.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/28/2019
ms.author: mjbrown
ms.openlocfilehash: cc89c0052097ba9250370334386943ec4107140c
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83655330"
---
# <a name="provision-standard-manual-throughput-on-a-database-in-azure-cosmos-db"></a>Provisionar taxa de transferência padrão (manual) em um banco de dados no Azure Cosmos DB

Este artigo explica como provisionar a taxa de transferência padrão (manual) em um banco de dados no Azure Cosmos DB. Você pode provisionar a taxa de transferência para um único [contêiner](how-to-provision-container-throughput.md) ou para um banco de dados e compartilhá-la entre os contêineres dentro dele. Para saber quando usar taxa de transferência no nível de banco de dados e de contêiner, consulte o artigo [Casos de uso para provisionar taxa de transferência em contêineres e bancos de dados](set-throughput.md). É possível provisionar a taxa de transferência no nível do banco de dados usando o portal do Azure ou os SDKs do Azure Cosmos DB.

## <a name="provision-throughput-using-azure-portal"></a>Provisionar a taxa de transferência usando o portal do Azure

### <a name="sql-core-api"></a><a id="portal-sql"></a>API de SQL (Core)

1. Entre no [portal do Azure](https://portal.azure.com/).

1. [Crie uma conta do Azure Cosmos](create-sql-api-dotnet.md#create-account) ou selecione uma existente.

1. Abra o painel **Data Explorer** e selecione **Novo Banco de Dados**. Forneça os seguintes detalhes:

   * Insira uma ID do banco de dados.
   * Selecione **Provisionar taxa de transferência**.
   * Insira uma taxa de transferência (por exemplo, 1000 RUs).
   * Selecione **OK**.

    ![Captura de tela da caixa de diálogo Novo Banco de Dados](./media/how-to-provision-database-throughput/provision-database-throughput-portal-all-api.png)

## <a name="provision-throughput-using-azure-cli-or-powershell"></a>Provisionar taxa de transferência com a CLI do Azure ou o PowerShell

Para criar um banco de dados com taxa de transferência compartilhada, consulte:

* [Criar um banco de dados usando a CLI do Azure](manage-with-cli.md#create-a-database-with-shared-throughput)
* [Criar um banco de dados usando o PowerShell](manage-with-powershell.md#create-db-ru)

## <a name="provision-throughput-using-net-sdk"></a>Provisionar a taxa de transferência usando o SDK .NET

> [!Note]
> Você pode usar os SDKs do Cosmos para a API de SQL para provisionar a taxa de transferência de todas as APIs. Opcionalmente, você pode usar o exemplo a seguir para a API do Cassandra também.

### <a name="all-apis"></a><a id="dotnet-all"></a>Todas as APIs

# <a name="net-sdk-v2"></a>[.NET SDK V2](#tab/dotnetv2)

```csharp
//set the throughput for the database
RequestOptions options = new RequestOptions
{
    OfferThroughput = 500
};

//create the database
await client.CreateDatabaseIfNotExistsAsync(
    new Database {Id = databaseName},  
    options);
```

# <a name="net-sdk-v3"></a>[.NET SDK V3](#tab/dotnetv3)

[!code-csharp[](~/samples-cosmosdb-dotnet-v3/Microsoft.Azure.Cosmos/tests/Microsoft.Azure.Cosmos.Tests/SampleCodeForDocs/DatabaseDocsSampleCode.cs?name=DatabaseCreateWithThroughput)]

---

### <a name="cassandra-api"></a><a id="dotnet-cassandra"></a>API do Cassandra

Um comando semelhante pode ser executado por qualquer driver compatível com CQL.

```csharp
// Create a Cassandra keyspace and provision throughput of 400 RU/s
session.Execute("CREATE KEYSPACE IF NOT EXISTS myKeySpace WITH cosmosdb_provisioned_throughput=400");
```
 
## <a name="next-steps"></a>Próximas etapas

Consulte os seguintes artigos para saber mais sobre taxa de transferência provisionada no Azure Cosmos DB:

* [Taxa de transferência provisionada para dimensionamento global](scaling-throughput.md)
* [Provisionar a taxa de transferência para contêineres e bancos de dados](set-throughput.md)
* [Como provisionar a taxa de transferência padrão (manual) para um contêiner](how-to-provision-container-throughput.md)
* [Como provisionar a taxa de transferência de dimensionamento automático para um contêiner](how-to-provision-autoscale-throughput.md)
* [Unidades de solicitação e taxa de transferência no Azure Cosmos DB](request-units.md)

---
title: 'Tutorial: Como consultar com SQL no Azure Cosmos DB?'
description: 'Tutorial: Saiba como consultar com consultas SQL no Azure Cosmos DB usando o Playground para Consultas'
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.custom: tutorial-develop, mvc
ms.topic: tutorial
ms.date: 11/05/2019
ms.reviewer: sngun
ms.openlocfilehash: 2a6033ef1d2b7dda04b1510d42fa49141e0b79b4
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/09/2020
ms.locfileid: "88135990"
---
# <a name="tutorial-query-azure-cosmos-db-by-using-the-sql-api"></a>Tutorial: Consultar o Azure Cosmos DB usando a API do SQL

A [API do SQL](documentdb-introduction.md) do Azure Cosmos DB oferece suporte à consulta de documentos usando SQL. Este artigo fornece um exemplo de documento e dois exemplos de consultas SQL e resultados.

Este artigo aborda as seguintes tarefas: 

> [!div class="checklist"]
> * Consultar dados com SQL

## <a name="sample-document"></a>Exemplo de documento

As consultas de SQL neste artigo usam o seguinte exemplo de documento.

```json
{
  "id": "WakefieldFamily",
  "parents": [
      { "familyName": "Wakefield", "givenName": "Robin" },
      { "familyName": "Miller", "givenName": "Ben" }
  ],
  "children": [
      {
        "familyName": "Merriam", 
        "givenName": "Jesse", 
        "gender": "female", "grade": 1,
        "pets": [
            { "givenName": "Goofy" },
            { "givenName": "Shadow" }
        ]
      },
      { 
        "familyName": "Miller", 
         "givenName": "Lisa", 
         "gender": "female", 
         "grade": 8 }
  ],
  "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
  "creationDate": 1431620462,
  "isRegistered": false
}
```

## <a name="where-can-i-run-sql-queries"></a>Onde é possível executar consultas SQL?

Você pode executar consultas usando o Data Explorer no Portal do Azure, por meio da [API REST e SDKs](sql-api-sdk-dotnet.md) e até mesmo o [Playground de consultas](https://www.documentdb.com/sql/demo), que executa consultas em um conjunto existente de dados de exemplo.

Para saber mais sobre consultas SQL, confira:
* [Consulta e sintaxe SQL](sql-query-getting-started.md)

## <a name="prerequisites"></a>Pré-requisitos

Este tutorial presume que você tem uma conta e uma coleção do Azure Cosmos DB. Não tem nenhum desses recursos? Conclua o [início rápido de 5 minutos](create-cosmosdb-resources-portal.md).

## <a name="example-query-1"></a>Exemplo de consulta 1

Com base no exemplo de documento de família acima, a consulta SQL a seguir retorna os documentos cujo campo de ID corresponde a `WakefieldFamily`. Por se tratar de uma instrução `SELECT *`, a saída da consulta será todo o documento JSON:

**Consulta**

```sql
    SELECT * 
    FROM Families f 
    WHERE f.id = "WakefieldFamily"
```

**Resultados**

```json
{
  "id": "WakefieldFamily",
  "parents": [
      { "familyName": "Wakefield", "givenName": "Robin" },
      { "familyName": "Miller", "givenName": "Ben" }
  ],
  "children": [
      {
        "familyName": "Merriam", 
        "givenName": "Jesse", 
        "gender": "female", "grade": 1,
        "pets": [
            { "givenName": "Goofy" },
            { "givenName": "Shadow" }
        ]
      },
      { 
        "familyName": "Miller", 
         "givenName": "Lisa", 
         "gender": "female", 
         "grade": 8 }
  ],
  "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
  "creationDate": 1431620462,
  "isRegistered": false
}
```

## <a name="example-query-2"></a>Exemplo de consulta 2

A consulta a seguir retorna todos os nomes dos filhos na família cuja ID corresponde a `WakefieldFamily`.

**Consulta**

```sql
    SELECT c.givenName 
    FROM Families f 
    JOIN c IN f.children 
    WHERE f.id = 'WakefieldFamily'
```

**Resultados**

```
[
    {
        "givenName": "Jesse"
    },
    {
        "givenName": "Lisa"
    }
]
```


## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você executou as seguintes tarefas:

> [!div class="checklist"]
> * Aprendeu a consultar usando SQL  

Agora você pode prosseguir para o próximo tutorial e aprender a distribuir seus dados globalmente.

> [!div class="nextstepaction"]
> [Distribuir os dados globalmente](tutorial-global-distribution-sql-api.md)


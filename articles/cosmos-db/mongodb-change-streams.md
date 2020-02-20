---
title: Alterar fluxos na API do Azure Cosmos DB para MongoDB
description: Saiba como usar a API do Change streams n Azure Cosmos DB para MongoDB para obter as alterações feitas em seus dados.
author: srchi
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: conceptual
ms.date: 11/16/2019
ms.author: srchi
ms.openlocfilehash: ec1ec1a8a80953f8988355341ee7128bd29b982d
ms.sourcegitcommit: 64def2a06d4004343ec3396e7c600af6af5b12bb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77467770"
---
# <a name="change-streams-in-azure-cosmos-dbs-api-for-mongodb"></a>Alterar fluxos na API do Azure Cosmos DB para MongoDB

O suporte do [feed de alterações](change-feed.md) na api do Azure Cosmos DB para MongoDB está disponível usando a API de fluxos de alteração. Usando a API de fluxos de alteração, seus aplicativos podem obter as alterações feitas na coleção ou nos itens em um único fragmento. Posteriormente, você pode executar ações adicionais com base nos resultados. As alterações nos itens na coleção são capturadas na ordem de seu tempo de modificação e a ordem de classificação é garantida por chave de fragmentação.

> [!NOTE]
> Para usar os fluxos de alteração, crie a conta com a versão 3,6 da API do Azure Cosmos DB para MongoDB ou uma versão posterior. Se você executar os exemplos de fluxo de alterações em uma versão anterior, poderá ver o erro de `Unrecognized pipeline stage name: $changeStream`. 

O exemplo a seguir mostra como obter fluxos de alteração em todos os itens na coleção. Este exemplo cria um cursor para observar itens quando eles são inseridos, atualizados ou substituídos. O estágio de $match, estágio de $project e a opção fullDocument são necessários para obter os fluxos de alteração. Não há suporte para a observação de operações de exclusão usando fluxos de alteração no momento. Como alternativa, você pode adicionar um marcador flexível nos itens que estão sendo excluídos. Por exemplo, você pode adicionar um atributo no item chamado "excluído" e defini-lo como "true" e definir um TTL no item, para que você possa excluí-lo automaticamente, bem como rastreá-lo.

```javascript
var cursor = db.coll.watch(
    [
        { $match: { "operationType": { $in: ["insert", "update", "replace"] } } },
        { $project: { "_id": 1, "fullDocument": 1, "ns": 1, "documentKey": 1 } }
    ],
    { fullDocument: "updateLookup" });

while (!cursor.isExhausted()) {
    if (cursor.hasNext()) {
        printjson(cursor.next());
    }
}
```

O exemplo a seguir mostra como obter alterações nos itens em um único fragmento. Este exemplo obtém as alterações de itens que têm a chave de fragmentação igual a "a" e o valor de chave de fragmento igual a "1".

```javascript
var cursor = db.coll.watch(
    [
        { 
            $match: { 
                $and: [
                    { "fullDocument.a": 1 }, 
                    { "operationType": { $in: ["insert", "update", "replace"] } }
                ]
            }
        },
        { $project: { "_id": 1, "fullDocument": 1, "ns": 1, "documentKey": 1} }
    ],
    { fullDocument: "updateLookup" });

```

## <a name="current-limitations"></a>Limitações atuais

As seguintes limitações são aplicáveis ao usar fluxos de alteração:

* As propriedades `operationType` e `updateDescription` ainda não têm suporte no documento de saída.
* Atualmente, há suporte para os tipos de operações `insert`, `update`e `replace`. Ainda não há suporte para a operação de exclusão ou outros eventos.

Devido a essas limitações, as opções estágio de $match, estágio de $project e fullDocument são necessárias, conforme mostrado nos exemplos anteriores.

## <a name="error-handling"></a>Tratamento de erros

Há suporte para os seguintes códigos de erro e mensagens ao usar fluxos de alteração:

* **Código de erro HTTP 429** -quando o fluxo de alteração é limitado, ele retorna uma página vazia.

* **NamespaceNotFound (OperationType Invalidate)** – se você executar o fluxo de alterações na coleção que não existe ou se a coleção for descartada, um erro de `NamespaceNotFound` será retornado. Como a propriedade `operationType` não pode ser retornada no documento de saída, em vez do erro `operationType Invalidate`, o erro `NamespaceNotFound` é retornado.

## <a name="next-steps"></a>Próximas etapas

* [Use a vida útil para expirar os dados automaticamente na API do Azure Cosmos DB para MongoDB](mongodb-time-to-live.md)
* [Indexação na API de Azure Cosmos DB para MongoDB](mongodb-indexing.md)

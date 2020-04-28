---
title: Alias no Azure Cosmos DB
description: Saiba como usar o alias em consultas do Azure Cosmos DB SQL para diferenciar duas propriedades com o mesmo nome
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 12/02/2019
ms.author: mjbrown
ms.openlocfilehash: 74849eec4c5808a584894321269c49c41f0b8a5c
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2020
ms.locfileid: "74873464"
---
# <a name="aliasing-in-azure-cosmos-db"></a>Alias no Azure Cosmos DB

Você pode explicitamente alias de valores em consultas. Se uma consulta tiver duas propriedades com o mesmo nome, use alias para renomear uma ou ambas as propriedades para que elas sejam desambiguadas no resultado projetado.

## <a name="examples"></a>Exemplos

A palavra-chave as usada para alias é opcional, conforme mostrado no exemplo a seguir ao projetar o segundo valor como `NameInfo`:

```sql
    SELECT 
           { "state": f.address.state, "city": f.address.city } AS AddressInfo,
           { "name": f.id } NameInfo
    FROM Families f
    WHERE f.id = "AndersenFamily"
```

Os resultados são:

```json
    [{
      "AddressInfo": {
        "state": "WA",
        "city": "Seattle"
      },
      "NameInfo": {
        "name": "AndersenFamily"
      }
    }]
```

## <a name="next-steps"></a>Próximas etapas

- [Amostras do .NET no Azure Cosmos DB](https://github.com/Azure/azure-cosmos-dotnet-v3)
- [Cláusula SELECT](sql-query-select.md)
- [Cláusula FROM](sql-query-from.md)

---
title: ARRAY_SLICE na linguagem de consulta Azure Cosmos DB
description: Saiba mais sobre como a função de sistema SQL de fatia de matriz em Azure Cosmos DB retorna parte de uma expressão de matriz
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/03/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: a98cb17d22f41776ff788d12ced6aa988ad0b10e
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/09/2020
ms.locfileid: "78303317"
---
# <a name="array_slice-azure-cosmos-db"></a>ARRAY_SLICE (Azure Cosmos DB)
 Retorna parte de uma expressão de matriz.
  
## <a name="syntax"></a>Sintaxe
  
```sql
ARRAY_SLICE (<arr_expr>, <num_expr> [, <num_expr>])  
```  
  
## <a name="arguments"></a>Argumentos
  
*arr_expr*  
   É qualquer expressão de matriz.  
  
*num_expr*  
   Índice numérico com base em zero no qual começar a matriz. Valores negativos podem ser usados para especificar o índice inicial relativo ao último elemento da matriz, ou seja, -1 faz referência ao último elemento na matriz.  

*num_expr* Expressão numérica opcional que define o número máximo de elementos na matriz resultante.    

## <a name="return-types"></a>Tipos de retorno
  
  Retorna uma expressão de matriz.  
  
## <a name="examples"></a>Exemplos
  
  O exemplo a seguir mostra como obter diferentes fatias de uma matriz usando `ARRAY_SLICE` .  
  
```sql
SELECT
           ARRAY_SLICE(["apples", "strawberries", "bananas"], 1) AS s1,  
           ARRAY_SLICE(["apples", "strawberries", "bananas"], 1, 1) AS s2,
           ARRAY_SLICE(["apples", "strawberries", "bananas"], -2, 1) AS s3,
           ARRAY_SLICE(["apples", "strawberries", "bananas"], -2, 2) AS s4,
           ARRAY_SLICE(["apples", "strawberries", "bananas"], 1, 0) AS s5,
           ARRAY_SLICE(["apples", "strawberries", "bananas"], 1, 1000) AS s6,
           ARRAY_SLICE(["apples", "strawberries", "bananas"], 1, -100) AS s7      
  
```  
  
 Este é o conjunto de resultados.  
  
```json
[{  
           "s1": ["strawberries", "bananas"],   
           "s2": ["strawberries"],
           "s3": ["strawberries"],  
           "s4": ["strawberries", "bananas"], 
           "s5": [],
           "s6": ["strawberries", "bananas"],
           "s7": [] 
}]  
```  

## <a name="remarks"></a>Comentários

Essa função do sistema não usará o índice.

## <a name="next-steps"></a>Próximas etapas

- [Funções de matriz Azure Cosmos DB](sql-query-array-functions.md)
- [Funções de sistema do Azure Cosmos DB](sql-query-system-functions.md)
- [Introdução ao Azure Cosmos DB](introduction.md)

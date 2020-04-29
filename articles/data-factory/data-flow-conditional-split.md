---
title: Transformação de divisão condicional no fluxo de dados de mapeamento
description: Dividir dados em fluxos diferentes usando a transformação Divisão condicional no fluxo de dados de mapeamento Azure Data Factory
author: kromerm
ms.author: makromer
ms.reviewer: daperlov
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 10/16/2019
ms.openlocfilehash: bd9241e526d7cf42f0697afb8635c085a08c80d8
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81606475"
---
# <a name="conditional-split-transformation-in-mapping-data-flow"></a>Transformação de divisão condicional no fluxo de dados de mapeamento

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

A transformação Divisão condicional roteia as linhas de dados para diferentes fluxos com base em condições de correspondência. A transformação Divisão condicional é semelhante a uma estrutura de decisão de caso em uma linguagem de programação. A transformação avalia expressões, e com base nos resultados, direciona a linha de dados para o fluxo especificado.

## <a name="configuration"></a>Configuração

A configuração **Split on** determina se a linha de dados flui para o primeiro fluxo de correspondência ou para cada fluxo ao qual ela corresponde.

Use o construtor de expressões de fluxo de dados para inserir uma expressão para a condição de divisão. Para adicionar uma nova condição, clique no ícone de adição em uma linha existente. Um fluxo padrão também pode ser adicionado para linhas que não correspondem a nenhuma condição.

![Divisão condicional](media/data-flow/conditionalsplit1.png "opções de divisão condicional")

## <a name="data-flow-script"></a>Script de fluxo de dados

### <a name="syntax"></a>Sintaxe

```
<incomingStream>
    split(
        <conditionalExpression1>
        <conditionalExpression2>
        ...
        disjoint: {true | false}
    ) ~> <splitTx>@(stream1, stream2, ..., <defaultStream>)
```

### <a name="example"></a>Exemplo

O exemplo abaixo é uma transformação de divisão condicional `SplitByYear` chamada que usa o fluxo `CleanData`de entrada. Essa transformação tem duas condições `year < 1960` de divisão `year > 1980`e. `disjoint`é false porque os dados vão para a primeira condição de correspondência. Cada linha que corresponde à primeira condição vai para o `moviesBefore1960`fluxo de saída. Todas as linhas restantes que correspondem à segunda condição vão para `moviesAFter1980`o fluxo de saída. Todas as outras linhas fluem por meio do `AllOtherMovies`fluxo padrão.

No Data Factory UX, essa transformação é semelhante à imagem abaixo:

![Divisão condicional](media/data-flow/conditionalsplit1.png "opções de divisão condicional")

O script de fluxo de dados para essa transformação está no trecho de código abaixo:

```
CleanData
    split(
        year < 1960,
        year > 1980,
        disjoint: false
    ) ~> SplitByYear@(moviesBefore1960, moviesAfter1980, AllOtherMovies)
```

## <a name="next-steps"></a>Próximas etapas

As transformações de fluxo de dados comuns usadas com divisão condicional são a [transformação de junção](data-flow-join.md), a [transformação de pesquisa](data-flow-lookup.md)e a [transformação selecionar](data-flow-select.md)

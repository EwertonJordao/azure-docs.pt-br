---
title: Transformação de pesquisa no fluxo de dados de mapeamento
description: Dados de referência de outra fonte usando a transformação de pesquisa no fluxo de dados de mapeamento.
author: kromerm
ms.reviewer: daperlov
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 05/28/2020
ms.openlocfilehash: a4fcdad0efda1ab2a43be65865e3aac59f7ef3e3
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/09/2020
ms.locfileid: "84187605"
---
# <a name="lookup-transformation-in-mapping-data-flow"></a>Transformação de pesquisa no fluxo de dados de mapeamento

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Use a transformação de pesquisa para fazer referência a dados de outra fonte em um stream de fluxo de dados. A transformação de pesquisa acrescenta colunas dos dados correspondentes aos dados de origem.

Uma transformação de pesquisa é semelhante a uma união externa esquerda. Todas as linhas do fluxo primário existirão no fluxo de saída com colunas adicionais do fluxo de pesquisa.

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE4xsVT]

## <a name="configuration"></a>Configuração

![Transformação Pesquisa](media/data-flow/lookup1.png "Pesquisa")

**Fluxo primário:** O fluxo de entrada de dados. Esse fluxo é equivalente ao lado esquerdo de uma união.

**Fluxo de pesquisa:** Os dados que são acrescentados ao fluxo primário. A determinação de quais dados são adicionados é feita pelas condições de pesquisa. Esse fluxo é equivalente ao lado direito de uma união.

**Corresponder várias linhas:** Se habilitada, uma linha com várias correspondências no fluxo principal retornará várias linhas. Caso contrário, apenas uma linha será retornada com base na condição “Corresponder a”.

**Corresponder a:** Visível somente se “Corresponder a várias linhas” não estiver selecionado. Escolha se deseja corresponder a qualquer linha, a primeira correspondência ou a última correspondência. Qualquer linha será recomendada à medida que for executada mais rapidamente. Se a primeira linha ou a última linha for selecionada, será necessário especificar as condições de classificação.

**Condições de pesquisa:** Escolha a quais colunas será feita a correspondência. Se a condição de igualdade for atendida, as linhas serão consideradas uma correspondência. Focalize e selecione "Coluna computada" para extrair um valor usando a [linguagem de expressão do fluxo de dados](data-flow-expression-functions.md).

A transformação de pesquisa só dá suporte a correspondências de igualdade. Para personalizar a expressão de pesquisa a incluir outros operadores, como maior que, é recomendável usar uma [união cruzada na transformação de união](data-flow-join.md#custom-cross-join). Uma união cruzada evitará erros de produto cartesiano possíveis na execução.

Todas as colunas de ambos os fluxos são incluídas nos dados de saída. Para remover colunas duplicadas ou indesejadas, adicione um [Selecionar transformação](data-flow-select.md) após a transformação de pesquisa. As colunas também podem ser descartadas ou renomeadas em uma transformação de coletor.

### <a name="non-equi-joins"></a>Uniões não equivalentes

Para usar um operador condicional como diferente de (!=) ou maior que (>) em suas condições de pesquisa, altere a lista suspensa do operador entre as duas colunas. Uniões não equivalentes exigem que pelo menos um dos dois fluxos sejam transmitidos usando a transmissão **Fixa** na guia **Otimizar**.

![Pesquisa sem equivalência](media/data-flow/non-equi-lookup.png "Pesquisa sem equivalência")

## <a name="analyzing-matched-rows"></a>Análise de linhas correspondentes

Após a transformação de pesquisa, a função `isMatch()` poderá ser usada para ver se a pesquisa correspondeu a linhas individuais.

![Padrão de pesquisa](media/data-flow/lookup111.png "Padrão de pesquisa")

Um exemplo desse padrão é usar a transformação de divisão condicional para dividir na função `isMatch()`. No exemplo acima, as linhas correspondentes passam pelo fluxo superior, e o fluxo de linhas não correspondentes pelo fluxo ```NoMatch```.

## <a name="testing-lookup-conditions"></a>Teste de condições de pesquisa

Ao testar a transformação de pesquisa com a visualização de dados no modo de depuração, use um pequeno conjunto de dados conhecidos. Ao fazer a amostragem de linhas de um grande conjunto de grande, você não consegue prever quais linhas e chaves serão lidas para o teste. O resultado é não determinístico, o que significa que suas condições de junção podem não retornar correspondência alguma.

## <a name="broadcast-optimization"></a>Otimização de transmissão

![União de transmissão](media/data-flow/broadcast.png "União de transmissão")

Em transformação de junções, pesquisas e ocorrências, se um ou ambos os fluxos de dados se ajustarem à memória do nó de trabalho, você poderá otimizar o desempenho habilitando a **Difusão**. Por padrão, o mecanismo do Spark decidirá automaticamente se deseja ou não transmitir um lado. Para escolher manualmente o lado a ser transmitido, selecione **Fixo**.

Não é recomendável desabilitar a transmissão por meio da opção **Desativar**, a menos que suas uniões estejam tendo erros de tempo limite.

## <a name="data-flow-script"></a>Script de fluxo de dados

### <a name="syntax"></a>Sintaxe

```
<leftStream>, <rightStream>
    lookup(
        <lookupConditionExpression>,
        multiple: { true | false },
        pickup: { 'first' | 'last' | 'any' },  ## Only required if false is selected for multiple
        { desc | asc }( <sortColumn>, { true | false }), ## Only required if 'first' or 'last' is selected. true/false determines whether to put nulls first
        broadcast: { 'auto' | 'left' | 'right' | 'both' | 'off' }
    ) ~> <lookupTransformationName>
```
### <a name="example"></a>Exemplo

![Transformação Pesquisa](media/data-flow/lookup-dsl-example.png "Pesquisa")

O script de fluxo de dados para a configuração de pesquisa acima está no trecho de código abaixo.

```
SQLProducts, DimProd lookup(ProductID == ProductKey,
    multiple: false,
    pickup: 'first',
    asc(ProductKey, true),
    broadcast: 'auto')~> LookupKeys
```
## 
Próximas etapas

* As transformações de [união](data-flow-join.md) e de [ocorrências](data-flow-exists.md) ambas levam várias entradas de fluxo
* Usar uma [transformação de divisão condicional](data-flow-conditional-split.md) com ```isMatch()``` para dividir as linhas em valores correspondentes e não correspondentes

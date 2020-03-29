---
title: Pontuações de previsão - LUIS
titleSuffix: Azure Cognitive Services
description: Um escore de predição indica o grau de confiança que o serviço de API LUIS tem para os resultados de previsão, com base em um pronunciamento do usuário.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 10/10/2019
ms.author: diberry
ms.openlocfilehash: b360bc82b80e834492b524acc5c4535b0409eda1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2020
ms.locfileid: "74280818"
---
# <a name="prediction-scores-indicate-prediction-accuracy-for-intent-and-entities"></a>Pontuações de previsão indicam a precisão da previsão de intenções e entidades

Um escore de predição indica o grau de confiança que luis tem para os resultados de previsão de uma declaração de usuário.

Uma pontuação de previsão é entre zero (0) e um (1). Um exemplo de uma pontuação com alta confiança do LUIS é 0,99. Um exemplo de uma pontuação com baixa confiança é 0,01. 

|Valor da pontuação|Confiança|
|--|--|
|1|correspondência definida|
|0.99|alta confiança|
|0,01|baixa confiança|
|0|falha definida na correspondência|

## <a name="top-scoring-intent"></a>Intenção com maior pontuação

Toda previsão de declaração retorna uma intenção com maior pontuação. Esta previsão é uma comparação numérica dos escores de previsão. 

## <a name="proximity-of-scores-to-each-other"></a>Proximidade de pontuações entre si

As 2 melhores pontuações podem ter uma diferença muito pequena entre eles. LUIS não indica essa proximidade além de retornar a pontuação máxima.  

## <a name="return-prediction-score-for-all-intents"></a>Retornar pontuação de previsão para todas as intenções

Um resultado do ponto de extremidade ou do teste pode incluir todas as intenções. Esta configuração é definida no ponto final usando o par de nome/valor de consulta correto.

|API de previsão|Nome de consulta|
|--|--|
|V3|`show-all-intents=true`|
|V2|`verbose=true`|

## <a name="review-intents-with-similar-scores"></a>Examinar intenções com pontuações semelhantes

Revisar a pontuação para todas as intenções é uma boa maneira de verificar que não só é a intenção correta identificada, mas que a pontuação da próxima intenção identificada é significativa e consistentemente menor para declarações.

Se várias intenções tiverem pontuações de previsão próximas, com base no contexto de uma declaração, o LUIS poderá alternar entre as intenções. Para corrigir essa situação, continue a adicionar expressões a cada intenção com uma variedade mais ampla de diferenças contextuais ou você pode ter o aplicativo do cliente, como um bot de bate-papo, fazer escolhas programáticas sobre como lidar com as 2 intenções principais.

As duas intenções, que são muito bem pontuadas, podem inverter devido ao **treinamento não determinístico**. A pontuação superior poderia se tornar a parte superior da segunda e a segunda pontuação superior poderia se tornar a primeira pontuação superior. Para evitar essa situação, adicione expressões de exemplo a cada uma das duas principais intenções para essa expressão com escolha de palavra e contexto que diferencie as duas intenções. As duas intenções devem ter aproximadamente o mesmo número de declarações de exemplo. Um princípio de separação para evitar a inversão devido ao treinamento, é uma diferença de 15% em pontuações.

Você pode desativar o **treinamento não determinista** [treinando com todos os dados](luis-how-to-train.md#train-with-all-data).

## <a name="differences-with-predictions-between-different-training-sessions"></a>Diferenças com previsões entre diferentes sessões de treinamento

Quando você treina o mesmo modelo em um aplicativo diferente, e as pontuações não são as mesmas, essa diferença é porque há **treinamento não determinístico** (um elemento de aleatoriedade). Em segundo lugar, qualquer sobreposição de uma expressão para mais de uma intenção significa que a principal intenção da mesma declaração pode mudar com base em treinamento.

Se o seu bot de bate-papo requer uma pontuação LUIS específica para indicar confiança em uma intenção, você deve usar a diferença de pontuação entre as duas intenções principais. Esta situação proporciona flexibilidade para variações no treinamento.

Você pode desativar o **treinamento não determinista** [treinando com todos os dados](luis-how-to-train.md#train-with-all-data).

## <a name="e-exponent-notation"></a>Notação E (exponencial)

As pontuações de previsão podem usar a notação exponencial, _aparecendo_ acima do intervalo 0-1 range, como `9.910309E-07`. Essa pontuação é uma indicação de um número muito **pequeno**.

|Pontuação da notação E |Pontuação real|
|--|--|
|9,910309E-07|.0000009910309|

## <a name="punctuation"></a>Pontuação

[Saiba mais](luis-concept-utterance.md#punctuation-marks) sobre como usar ou ignorar a pontuação. 

## <a name="next-steps"></a>Próximas etapas

Confira [Adicionar entidades](luis-how-to-add-entities.md) para saber como adicionar entidades ao seu aplicativo LUIS.

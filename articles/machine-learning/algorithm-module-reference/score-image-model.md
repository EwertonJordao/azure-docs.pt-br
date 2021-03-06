---
title: Usar o módulo modelo de imagem de Pontuação
titleSuffix: Azure Machine Learning
description: Saiba como usar o módulo modelo de imagem de pontuação em Azure Machine Learning para gerar previsões usando um modelo de imagem treinado.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 05/26/2020
ms.openlocfilehash: 88ca997e2d22283babf582b10d9b0eeb7de122c0
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/09/2020
ms.locfileid: "90905190"
---
# <a name="score-image-model"></a>Pontuar Modelo de Imagem

Este artigo descreve um módulo no designer de Azure Machine Learning.

Use este módulo para gerar previsões usando um modelo de imagem treinado em dados de imagem de entrada.

## <a name="how-to-configure-score-image-model"></a>Como configurar o modelo de imagem de Pontuação

1. Adicione o módulo **modelo de imagem de Pontuação** ao seu pipeline.

2. Anexe um modelo de imagem treinado e um conjunto de dados que contenha dado de imagem de entrada. 

    Os dados devem ser do tipo ImageDirectory. Consulte [converter em](convert-to-image-directory.md) módulo de diretório de imagem para obter mais informações sobre como obter um diretório de imagem. O esquema do conjunto de dados de entrada também deve corresponder ao esquema dos dados usados para treinar o modelo.

3. Enviar o pipeline.

## <a name="results"></a>Resultados

Depois de ter gerado um conjunto de pontuações usando o [modelo de imagem de Pontuação](score-image-model.md), para gerar um conjunto de métricas usadas para avaliar a precisão do modelo (desempenho), você pode conectar este módulo e o conjunto de resultados de classificação para avaliar o [modelo](evaluate-model.md), 

### <a name="publish-scores-as-a-web-service"></a>Publicar pontuações como um serviço Web

Um uso comum de pontuação é retornar a saída como parte de um serviço Web de previsão. Para obter mais informações, consulte [este tutorial](https://docs.microsoft.com/azure/machine-learning/tutorial-designer-automobile-price-deploy) sobre como implantar um ponto de extremidade em tempo real com base em um pipeline no designer de Azure Machine Learning.

## <a name="next-steps"></a>Próximas etapas

Confira o [conjunto de módulos disponíveis](module-reference.md) no Azure Machine Learning. 

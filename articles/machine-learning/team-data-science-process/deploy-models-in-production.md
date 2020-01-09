---
title: Implantar modelos em produção - Processo de Ciência de Dados de Equipe
description: Como implantar modelos em produção, possibilitando que desempenham um papel ativo na tomada de decisões de negócios.
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 11/30/2017
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: ed48ab9f85d28bec926120fea87a5ccf8bb6f9af
ms.sourcegitcommit: ce4a99b493f8cf2d2fd4e29d9ba92f5f942a754c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/28/2019
ms.locfileid: "75526763"
---
# <a name="deploy-models-to-production-to-play-an-active-role-in-making-business-decisions"></a>Implantar modelos para produção para desempenhar um papel ativo na tomada de decisões de negócios

A implantação de produção permite que um modelo execute uma função ativa em uma empresa. Previsões de um modelo implantado podem ser usadas para decisões de negócios.

## <a name="production-platforms"></a>Plataformas de produção

Há várias abordagens e plataformas para colocar modelos em produção. Veja algumas opções:

- [Onde implantar modelos com Azure Machine Learning](../how-to-deploy-and-where.md)
- [Implantação de um modelo no SQL-server](https://docs.microsoft.com/sql/advanced-analytics/tutorials/sqldev-py6-operationalize-the-model)
- [Microsoft Machine Learning Server](https://docs.microsoft.com/sql/advanced-analytics/r/r-server-standalone)

>[!NOTE]
>Antes da implantação, é preciso garantir que a latência da pontuação de modelo é baixa o suficiente para usar em produção.
>

>[!NOTE]
>Para implantações que usam o Azure Machine Learning Studio, consulte [Implantar um serviço Web do Machine Learning](../studio/deploy-a-machine-learning-web-service.md).
>

## <a name="ab-testing"></a>Testes de A/B

Quando vários modelos estão em produção, pode ser útil executar [Testes de A/B](https://en.wikipedia.org/wiki/A/B_testing) para comparar o desempenho dos modelos. 
 
## <a name="next-steps"></a>Próximos passos

Também são fornecidas instruções passo a passo que demonstram todas as etapas do processo para **cenários específicos**. Eles estão listados e vinculados a descrições em miniatura no artigo [Instruções passo a passo de exemplo](walkthroughs.md). Eles ilustram como combinar a nuvem, as ferramentas locais e os serviços em um fluxo de trabalho ou pipeline para criar um aplicativo inteligente. 

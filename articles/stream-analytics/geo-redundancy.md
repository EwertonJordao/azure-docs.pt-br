---
title: Obtenha redundância geográfica para trabalhos de Azure Stream Analytics
description: Este artigo descreve como obter redundância geográfica de trabalhos de Azure Stream Analytics em vez de failover geográfico.
author: mamccrea
ms.author: mamccrea
ms.service: stream-analytics
ms.topic: how-to
ms.date: 08/26/2020
ms.openlocfilehash: 44b0394773485f8054eddc94c7d7e9b91baebc6d
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/09/2020
ms.locfileid: "88951087"
---
# <a name="achieve-geo-redundancy-for-azure-stream-analytics-jobs"></a>Obtenha redundância geográfica para trabalhos de Azure Stream Analytics

O Azure Stream Analytics não fornece failover geográfico automático, mas você pode obter redundância geográfica implantando trabalhos de Stream Analytics idênticos em várias regiões do Azure. Cada trabalho se conecta a uma entrada local e a fontes de saída locais. É responsabilidade de seu aplicativo enviar dados de entrada para as duas entradas regionais e reconciliar entre as duas saídas regionais. Os trabalhos de Stream Analytics são duas entidades separadas.

O diagrama a seguir ilustra um exemplo de implantação de trabalho de Stream Analytics com redundância geográfica com entrada do hub de eventos e saída do banco de dados do Azure.

:::image type="content" source="media/geo-redundancy/geo-redundant-jobs.png" alt-text="diagrama de trabalhos do Stream Analytics com redundância geográfica":::

## <a name="primarysecondary-strategy"></a>Estratégia primária/secundária

Seu aplicativo precisa gerenciar qual banco de dados de saída de região é considerado o primário e que é considerado o secundário. Em uma falha de região primária, o aplicativo alterna para o banco de dados secundário e inicia a leitura de atualizações desse banco de dados. O mecanismo real que permite minimizar leituras duplicadas depende de seu aplicativo.Você pode simplificar esse processo gravando informações adicionais na saída. Por exemplo, você pode adicionar um carimbo de data/hora ou uma ID de sequência a cada saída para fazer com que as linhas duplicadas sejam ignoradas em uma operação trivial. Depois que a região primária é restaurada, ela é capturada com o banco de dados secundário usando uma mecânica semelhante.

Embora diferentes tipos de entrada e saída permitam diferentes opções de replicação geográfica, é recomendável usar o padrão descrito neste artigo para obter redundância geográfica porque ela fornece flexibilidade e controle para os produtores de eventos e consumidores de eventos.

## <a name="next-steps"></a>Próximas etapas

* [Monitorar e gerenciar trabalhos do Azure Stream Analytics com o PowerShell](stream-analytics-monitor-and-manage-jobs-use-powershell.md)
* [Depuração controlada por dados no Azure Stream Analytics](stream-analytics-job-diagram-with-metrics.md)
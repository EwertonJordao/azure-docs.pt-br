---
title: incluir arquivo
description: incluir arquivo
services: digital-twins
ms.service: digital-twins
ms.topic: include
ms.date: 02/03/2020
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.custom: include file
ms.openlocfilehash: f097479b67a7b48eca4a2710db3bd7eed6ddc982
ms.sourcegitcommit: 4f6a7a2572723b0405a21fea0894d34f9d5b8e12
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/04/2020
ms.locfileid: "77013704"
---
O a seguir resume os principais limites em disponibilidade geral.

### <a name="sku-ingress-rates-and-capacities"></a>Taxas e capacidades de entrada do SKU

As taxas e as capacidades de entrada de SKU S1 e S2 fornecem flexibilidade ao configurar um novo ambiente de Time Series Insights.

| Capacidade de SKU S1 | Taxa de entrada | Capacidade máxima de armazenamento
| --- | --- | --- |
| 1 | 1 GB (1 milhão de eventos) | 30 GB (30 milhões de eventos) por mês |
| 10 | 10 GB (10 milhões de eventos) | 300 GB (300 milhões de eventos) por mês |

| Capacidade de SKU S2 | Taxa de entrada | Capacidade máxima de armazenamento
| --- | --- | --- |
| 1 | 10 GB (10 milhões de eventos) | 300 GB (300 milhões de eventos) por mês |
| 10 | 100 GB (100 milhões de eventos) | 3 TB (3 bilhões de eventos) por mês |

> [!NOTE]
> As capacidades são dimensionadas linearmente, portanto, uma SKU S1 com capacidade 2 dá suporte a 2 GB (2 milhões) de eventos por taxa de entrada por dia e 60 GB (60 milhões de eventos) por mês.

Os ambientes de SKU S2 dão suporte a substancialmente mais eventos por mês e têm uma capacidade de entrada significativamente mais alta.

| SKU  | Contagem de eventos por mês  | Tamanho do evento por mês  | Contagem de eventos por minuto | Tamanho do evento por minuto  |
|---------|---------|---------|---------|---------|
| S1     |   30 milhões     |  30 GB     |  720    |  720 KB   |
 |S2     |   300 milhões    |   300 GB   | 7\.200   | 7\.200 KB  |

### <a name="property-limits"></a>Limites de propriedade

Os limites de propriedade de GA dependem do ambiente de SKU selecionado. As propriedades de evento fornecidas têm as colunas JSON, CSV e de gráfico correspondentes que podem ser exibidas no [Gerenciador de time Series insights](https://docs.microsoft.com/azure/time-series-insights/time-series-quickstart).

| SKU | Propriedades máximas |
| --- | --- |
| S1 | Propriedades de 600 (colunas) |
| S2 | Propriedades de 800 (colunas) |

### <a name="event-sources"></a>Origens de eventos

Há suporte para no máximo duas origens de evento por instância. 

* Saiba como [Adicionar uma origem do hub de eventos](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-how-to-add-an-event-source-eventhub).
* Configurar [uma origem do Hub IOT](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-how-to-add-an-event-source-iothub).

### <a name="api-limits"></a>Limites de API

Os limites da API REST para Time Series Insights disponibilidade geral são especificados na [documentação de referência da API REST](https://docs.microsoft.com/rest/api/time-series-insights/ga-query-api#limits).
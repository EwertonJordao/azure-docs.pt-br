---
title: Exibir métricas com Azure Monitor
description: Este artigo mostra como monitorar as métricas com os gráficos de portal do Azure e CLI do Azure.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 08/31/2020
ms.author: inhenkel
ms.custom: devx-track-azurecli
ms.openlocfilehash: 09c306b85c910e8ec2021abf15013935c44b0df3
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91707703"
---
# <a name="monitor-media-services-metrics"></a>Monitorar as métricas dos Serviços de Mídia do Microsoft Azure

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

[Azure monitor](../../azure-monitor/overview.md) permite que você monitore as métricas e os logs de diagnóstico que ajudam a entender como seus aplicativos estão sendo executados. Para obter uma descrição detalhada desse recurso e ver por que você desejaria usar os logs de diagnóstico e as métricas dos serviços de mídia do Azure, consulte [monitorar as métricas dos serviços de mídia e os logs de diagnóstico](media-services-metrics-diagnostic-logs.md).

O Azure Monitor fornece várias maneiras de interagir com as métricas, incluindo o gráfico delas no portal, acessá-las por meio da API REST ou consultá-las usando CLI do Azure. Este artigo mostra como monitorar as métricas com os gráficos de portal do Azure e CLI do Azure.

## <a name="prerequisites"></a>Pré-requisitos

- [Criar uma conta dos Serviços de Mídia](./create-account-howto.md)
- Examinar  [os logs de diagnóstico e as métricas dos serviços de mídia](media-services-metrics-diagnostic-logs.md)

## <a name="view-metrics-in-azure-portal"></a>Exibir métricas no portal do Azure

1. Entre no Portal do Azure em https://portal.azure.com.
1. Navegue até sua conta dos serviços de mídia do Azure e selecione **métricas**.
1. Clique na caixa de **recurso** e selecione o recurso para o qual você deseja monitorar as métricas.

    A janela **selecionar um recurso** é exibida à direita com a lista de recursos disponíveis para você. Nesse caso, você verá:

    * &lt;Nome da conta dos serviços de mídia&gt;
    * &lt;Nome da conta de serviços de mídia do &gt; / &lt; ponto de extremidade de streaming&gt;
    * &lt;nome da conta de armazenamento&gt;

    Selecione o recurso e pressione **aplicar**. Para obter detalhes sobre os recursos e as métricas com suporte, consulte [monitorar métricas dos serviços de mídia](media-services-metrics-diagnostic-logs.md).

    ![Captura de tela que mostra o recurso selecionado e realça o botão Aplicar.](media/media-services-metrics/metrics02.png)

    > [!NOTE]
    > Para alternar entre os recursos para os quais você deseja monitorar as métricas, clique na caixa de **recurso** novamente e repita esta etapa.
1. (Opcionalmente) dê um nome ao seu gráfico (edite o nome pressionando o lápis na parte superior).
1. Adicione métricas que você deseja exibir.

    ![Métricas](media/media-services-metrics/metrics03.png)
1. Você pode fixar o gráfico em seu painel.

## <a name="view-metrics-with-azure-cli"></a>Exibir métricas com CLI do Azure

Para obter métricas de "egresso" com CLI do Azure, você executaria o seguinte `az monitor metrics` comando:

```azurecli-interactive
az monitor metrics list --resource \
   "/subscriptions/<subscription id>/resourcegroups/<resource group name>/providers/Microsoft.Media/mediaservices/<Media Services account name>/streamingendpoints/<streaming endpoint name>" \
   --metric "Egress"
```

Para obter outras métricas, substitua "egresso" pelo nome da métrica em que você está interessado.

## <a name="see-also"></a>Confira também

* [Métricas do Azure Monitor](../../azure-monitor/platform/data-platform.md)
* [Crie, exiba e gerencie alertas de métrica usando Azure monitor](../../azure-monitor/platform/alerts-metric.md).

## <a name="next-steps"></a>Próximas etapas

[Logs de diagnóstico](media-services-diagnostic-logs-howto.md)

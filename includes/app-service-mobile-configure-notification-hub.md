---
author: conceptdev
ms.author: crdun
ms.service: app-service-mobile
ms.topic: include
ms.date: 08/23/2018
ms.openlocfilehash: da3793c428c624ce3a224cbd7606ab26c4a50803
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2020
ms.locfileid: "67172425"
---
O recurso Aplicativos Móveis do Serviço de Aplicativo do Azure usa [Hubs de Notificação] para enviar por push e, portanto, você deve configurar um hub de notificação para seu aplicativo móvel.

1. No [Portal do Azure], vá para **Serviços de Aplicativos** e, em seguida, selecione o back-end do aplicativo. Em **Configurações**, selecione **Push**.
2. Para adicionar um recurso do hub de notificação ao aplicativo, selecione **Conectar**. Você pode criar um hub ou conectar-se a um existente.

    ![Configurar um hub](./media/app-service-mobile-create-notification-hub/configure-hub-flow.png)

Agora você se conectou a um hub de notificação para o projeto de back-end dos Aplicativos Móveis. Posteriormente, você configura esse hub de notificação para conectar um PNS (Sistema de Notificação de Plataforma) a fim de enviar por push para dispositivos.

[Portal Azure]: https://portal.azure.com/
[Hubs de notificação do Azure]: https://azure.microsoft.com/documentation/articles/notification-hubs-push-notification-overview/

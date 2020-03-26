---
title: 'Início Rápido: Habilitar o serviço Central de Segurança do Azure para IoT no Hub IoT'
description: Saiba como integrar e habilitar o serviço de segurança Central de Segurança do Azure para IoT no Hub IoT do Azure.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: 670e6d2b-e168-4b14-a9bf-51a33c2a9aad
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/08/2019
ms.author: mlottner
ms.openlocfilehash: 0b1f09cfaf107802e1ce6586b3f96b14269aaced
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "74664835"
---
# <a name="quickstart-onboard-azure-security-center-for-iot-service-in-iot-hub"></a>Início Rápido: Integrar o serviço Central de Segurança do Azure para IoT no Hub IoT

Este artigo oferece uma explicação de como habilitar o serviço Central de Segurança do Azure para IoT em seu Hub IoT existente. Se você não tiver, no momento, um Hub IoT, confira [Criar um Hub IoT usando o portal do Azure](https://docs.microsoft.com/azure/iot-hub/iot-hub-create-through-portal) para começar a usar. 

> [!NOTE]
> No momento, a Central de Segurança do Azure para IoT só é compatível com a camada standard de Hubs IoT.


## <a name="prerequisites-for-enabling-the-service"></a>Pré-requisitos para habilitar o serviço

- Espaço de trabalho do Log Analytics
  - Dois tipos de informação são armazenados por padrão no workspace do Log Analytics pela Central de Segurança do Azure para IoT; **alertas de segurança** e **recomendações**. 
  - Você pode optar por adicionar armazenamento de um tipo de informações adicionais **eventos brutos**. Observe que armazenar **eventos brutos** no Log Analytics transporta os custos de armazenamento adicionais. 
- Hub IoT (camada standard)
- Atenda a todos os [pré-requisitos de serviço](service-prerequisites.md) 

## <a name="enable-azure-security-center-for-iot-on-your-iot-hub"></a>Habilitar a Central de Segurança do Azure para IoT em seu Hub IoT 

Para habilitar a segurança no Hub IoT: 

1. Abra o **Hub IoT** no portal do Azure. 
1. No menu **Segurança**, clique em **Proteger sua solução de IoT**.    


Parabéns! Você concluiu a habilitação da Central de Segurança do Azure para IoT em seu Hub IoT. 

### <a name="geolocation-and-ip-address-handling"></a>Geolocalização e manipulação de endereço IP

Para proteger a solução de IoT, os endereços IP de conexões de entrada e saída nos dispositivos IoT, no IoT Edge e nos Hubs IoT são coletados e armazenados por padrão. Essas informações são essenciais para detectar a conectividade anormal de fontes de IP suspeitas. Por exemplo, quando são feitas tentativas de estabelecer conexões de uma fonte IP em um botnet conhecido ou de uma fonte IP fora de sua geolocalização. O serviço Central de Segurança do Azure para IoT oferece a flexibilidade para habilitar e desabilitar a coleta de dados do endereço IP a qualquer momento. 

Para habilitar ou desabilitar a coleta de dados de endereços IP: 

1. Abra o Hub IoT e, em seguida, selecione **Visão Geral** no menu **Segurança**. 
2. Escolha a tela **Configurações** e modifique a geolocalização e/ou as configurações de manipulação de IP como desejar.

### <a name="log-analytics-creation"></a>Criação do Log Analytics

Quando a Central de Segurança do Azure para IoT é ativada, um workspace padrão do Azure Log Analytics é criado para armazenar eventos de segurança brutos, alertas e recomendações para os dispositivos IoT, o IoT Edge e o Hub IoT. Todos os meses, os cinco (5) primeiros GB de dados ingeridos por cliente no serviço Azure Log Analytics são gratuitos. Cada GB de dados ingerido em seu workspace do Azure Log Analytics é retido gratuitamente nos primeiros 31 dias. Saiba mais sobre os preços do [Log Analytics](https://azure.microsoft.com/pricing/details/monitor/).

Para alterar a configuração do workspace do Log Analytics:

1. Abra o Hub IoT e, em seguida, selecione **Visão Geral** no menu **Segurança**. 
2. Escolha a tela **Configurações** e modifique a configuração do workspace das configurações do Log Analytics como desejar.

### <a name="customize-your-iot-security-solution"></a>Personalizar sua solução de segurança da IoT
Por padrão, a ativação da solução Central de Segurança do Azure para IoT protege automaticamente todos os Hubs IoT em sua assinatura do Azure. 

Para ativar ou desligar o serviço Central de Segurança do Azure para IoT em um Hub IoT específico: 

1. Abra o Hub IoT e, em seguida, selecione **Visão Geral** no menu **Segurança**. 
2. Escolha a tela **Configurações** e modifique as configurações de segurança de qualquer hub IoT em sua assinatura do Azure como desejar.


## <a name="next-steps"></a>Próximas etapas

Passe para o próximo artigo para configurar sua solução...

> [!div class="nextstepaction"]
> [Configurar sua solução](quickstart-configure-your-solution.md)

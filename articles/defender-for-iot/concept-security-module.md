---
title: Módulo de segurança e dispositivo gêmeos
description: Saiba mais sobre o conceito de gêmeos do módulo de segurança e como eles são usados no defender para IoT.
services: defender-for-iot
ms.service: defender-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/24/2019
ms.author: mlottner
ms.openlocfilehash: a5382313c837482f116f498f3a05c36447062b0a
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/09/2020
ms.locfileid: "90933825"
---
# <a name="security-module"></a>Módulo de segurança

Este artigo explica como o defender para IoT usa dispositivos gêmeos e módulos.

## <a name="device-twins"></a>Dispositivos gêmeos

Para soluções de IoT criadas no Azure, dispositivos gêmeos desempenham um papel fundamental tanto no gerenciamento do dispositivo quanto na automação do processo.

O Defender para IoT oferece integração completa à plataforma de gerenciamento de dispositivos IoT que você já tem, permitindo gerenciar seu status de segurança de dispositivo e fazer uso das funcionalidades existentes de controle de dispositivo. A integração é obtida por meio do uso do mecanismo de entrelaçamento do Hub IoT.

Saiba mais sobre o conceito de [dispositivo gêmeos](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-device-twins) no Hub IOT do Azure.

## <a name="security-module-twins"></a>Gêmeos do módulo de segurança

O defender para IoT mantém um módulo de segurança para cada dispositivo no serviço.
O módulo de segurança de tudo contém todas as informações relevantes à segurança do dispositivo para cada dispositivo específico em sua solução.
As propriedades de segurança do dispositivo são mantidas em um módulo de segurança dedicado para comunicação mais segura e para habilitar atualizações e manutenção que exigem menos recursos.

Confira [Criar módulo de segurança](quickstart-create-security-twin.md) e [Configurar agentes de segurança](how-to-agent-configuration.md) para saber como criar, personalizar e configurar o entrelaçamento. Consulte [entendendo o módulo gêmeos](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-module-twins) para saber mais sobre o conceito de módulo gêmeos no Hub IOT.

## <a name="see-also"></a>Confira também

- [Visão geral do defender for IoT](overview.md)
- [Implantar agentes de segurança](how-to-deploy-agent.md)
- [Métodos de autenticação do agente de segurança](concept-security-agent-authentication-methods.md)

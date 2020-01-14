---
title: Criar um aplicativo Azure IoT Central | Microsoft Docs
description: Crie um novo aplicativo Azure IoT Central. Crie um aplicativo de Avaliação ou de Pagamento Conforme o Uso usando um modelo de aplicativo.
author: viv-liu
ms.author: viviali
ms.date: 08/02/2019
ms.topic: quickstart
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: corywink
ms.openlocfilehash: cb9968d3bcc30fe8e0f0023bcf7101cde5e4a196
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75453908"
---
# <a name="create-an-azure-iot-central-application"></a>Crie um aplicativo Azure IoT Central

[!INCLUDE [iot-central-original-pnp](../../../includes/iot-central-original-pnp-note.md)]

Como um _construtor_, use a interface do usuário do Azure IoT Central para definir seu aplicativo Microsoft Azure IoT Central. Este início rápido mostra como criar um aplicativo Azure IoT Central que contém um _modelo de dispositivo_ de exemplo. O aplicativo que você cria não usa nenhuma versão prévia dos recursos.

## <a name="create-an-application"></a>Criar um aplicativo

Navegue até o site do [Build do Azure IoT Central](https://aka.ms/iotcentral). Em seguida, entre com uma conta pessoal, corporativa ou de estudante da Microsoft.

Para começar a criar um aplicativo do Azure IoT Central sem a versão prévia dos recursos habilitada, selecione **Build**. Esse link leva você para a página **Crie um aplicativo IoT**.

![Página do build do Azure IoT Central](media/quick-deploy-iot-central/iotcentralcreate.png)

Em seguida, selecione **Aplicativo personalizado**.

Para criar um novo aplicativo Azure IoT Central:

1. O Azure IoT Central sugere automaticamente um nome de aplicativo com base no modelo de aplicativo selecionado. Aceite esse nome ou insira seu próprio nome de aplicativo amigável, como **Contoso IoT**. O Azure IoT Central também gera uma URL exclusiva para você, com base no nome do aplicativo. Você terá a liberdade para alterar esse prefixo de URL para algo mais fácil de memorizar se desejar.

1. Selecione o modelo de **aplicativo herdado** que não use recursos da versão prévia.

    | Modelo de aplicativo | DESCRIÇÃO |
    | -------------------- | ----------- |
    | Aplicativo herdado   | Cria um aplicativo vazio para você preencher com seus próprios dispositivos e modelos de dispositivos. |

1. Escolha um plano de pagamento:
   - Os aplicativos de **avaliação gratuita de 7 dias** são gratuitos por sete dias antes de expirarem. Eles podem ser convertidos em **Pagamento Conforme o Uso** em qualquer momento antes de expirarem. Se você criar um aplicativo de **Avaliação**, precisará inserir suas informações de contato e escolher se deseja receber informações e dicas da Microsoft.
   - Os aplicativos de **Pagamento Conforme o Uso** são cobrados por dispositivo, com os cinco primeiros dispositivos gratuitos. Se você criar um aplicativo de **Pagamento Conforme o Uso**, precisará escolher o *Directory*, a *Assinatura do Azure* e a *Localização*:
        - *Directory* é o AD (Azure Active Directory) para criar o aplicativo. Ele contém identidades de usuário, credenciais e outras informações organizacionais. Se você não tiver um Azure AD, ele será gerado quando você criar uma assinatura do Azure.
        - Uma *Assinatura do Azure* permite que você crie instâncias de serviços do Azure. O IoT Central provisiona recursos em sua assinatura. Se você não tiver uma assinatura do Azure, poderá criar uma na [página de entrada do Azure](https://aka.ms/createazuresubscription). Depois de criar a assinatura do Azure, volte à página **Criar um aplicativo**. A nova assinatura aparece na lista suspensa **Assinatura do Azure**.
        - A *Localização* é a [geografia](https://azure.microsoft.com/global-infrastructure/geographies/) em que você deseja criar seu aplicativo. Normalmente, você deve escolher a localização fisicamente mais próxima de seus dispositivos para obter um desempenho ideal. No momento, o Azure IoT Central está disponível nos **Estados Unidos**, na **Austrália**, no **Pacífico Asiático** e na **Europa**.  Depois de escolher uma localização, você não poderá mover o aplicativo posteriormente para outra localização.

        Saiba mais sobre preços na [Página de preços da microsoft IoT Central](https://azure.microsoft.com/pricing/details/iot-central/).

1. Preencha as informações adicionais necessárias para o plano de pagamento selecionado anteriormente, na Etapa 1.

1. Selecione **Criar** na parte inferior da página.

## <a name="next-steps"></a>Próximas etapas

Neste início rápido, você criou um aplicativo IoT Central. Aqui estão sugestões para as próximas etapas:

> [!div class="nextstepaction"]
> [Definir um novo tipo de dispositivo em seu aplicativo do Azure IoT Central](./tutorial-define-device-type.md)

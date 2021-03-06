---
title: Conectar o código do dispositivo de componente do Node.js de exemplo do IoT Plug and Play ao Hub IoT | Microsoft Docs
description: Crie e execute o código do dispositivo do Node.js de exemplo do IoT Plug and Play que usa vários componentes e se conecta a um hub IoT. Use a ferramenta Azure IoT Explorer para exibir as informações enviadas pelo dispositivo ao hub.
author: olivakar
ms.author: olkar
ms.date: 07/10/2020
ms.topic: tutorial
ms.service: iot-pnp
services: iot-pnp
ms.custom: devx-track-js
ms.openlocfilehash: ea7b1ba159aa5d11a20ff565390ce0b24e38c1d2
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91577179"
---
# <a name="tutorial-connect-a-sample-iot-plug-and-play-multiple-component-device-application-to-iot-hub-nodejs"></a>Tutorial: Conectar um aplicativo de exemplo de dispositivo de vários componentes do IoT Plug and Play ao Hub IoT (Node.js)

[!INCLUDE [iot-pnp-tutorials-device-selector.md](../../includes/iot-pnp-tutorials-device-selector.md)]

Este tutorial mostra como criar um aplicativo de exemplo de dispositivo do IoT Plug and Play com componentes, como conectá-lo ao hub IoT e como usar a ferramenta Azure IoT Explorer para ver as informações que ele envia ao hub. O aplicativo de exemplo é escrito para Node.js e é incluído no SDK do Dispositivo do Hub IoT do Azure para Node.js. Um construtor de soluções pode usar a ferramenta Azure IoT Explorer para compreender as funcionalidades de um dispositivo IoT Plug and Play sem a necessidade de ver nenhum código de dispositivo.

## <a name="prerequisites"></a>Pré-requisitos

[!INCLUDE [iot-pnp-prerequisites](../../includes/iot-pnp-prerequisites.md)]

Para concluir este tutorial, você precisa do Node.js em seu computador de desenvolvimento. Você pode baixar a versão mais recente recomendada para várias plataformas de [nodejs.org](https://nodejs.org).

Você pode verificar a versão atual do Node.js no computador de desenvolvimento usando o seguinte comando:

```cmd/sh
node --version
```

## <a name="download-the-code"></a>Baixar o código

Se você concluiu o [Início Rápido: Conecte um aplicativo de exemplo de dispositivo do IoT Plug and Play em execução no Windows ao Hub IoT (Node)](quickstart-connect-device-node.md). Você já clonou o repositório.

Abra um prompt de comando no diretório de sua escolha. Execute o seguinte comando para clonar o repositório GitHub do [SDK do Microsoft Azure IoT para Node.js](https://github.com/Azure/azure-iot-sdk-node) nesta localização:

```cmd/sh
git clone https://github.com/Azure/azure-iot-sdk-node
```

## <a name="install-required-libraries"></a>Instalar as bibliotecas necessárias

Use o SDK do dispositivo para criar o código de exemplo incluído. O aplicativo que você cria simula um dispositivo Plug and Play com vários componentes que se conectam a um hub IoT. O aplicativo envia a telemetria e as propriedades e recebe comandos.

1. Em uma janela de terminal local, acesse a pasta do repositório clonado e navegue até a pasta */azure-iot-sdk-node/device/samples/pnp*. Em seguida, execute o seguinte comando para instalar as bibliotecas necessárias:

```cmd/sh
npm install
```

Isso instalará os arquivos npm relevantes necessários para executar os exemplos na pasta.

## <a name="review-the-code"></a>Examine o código

Navegue até a pasta *azure-iot-sdk-node\device\samples\pnp*.

A pasta *azure-iot-sdk-node\device\samples\pnp* contém o código de exemplo do dispositivo controlador de temperatura do IoT Plug and Play.

O código no arquivo *pnpTemperatureController.js* implementa um dispositivo controlador de temperatura do IoT Plug and Play. O modelo que este exemplo implementa usa [vários componentes](concepts-components.md). O [arquivo de modelo de DTDL (linguagem de definição de Gêmeos Digitais) do dispositivo de temperatura](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/samples/TemperatureController.json) define a telemetria, as propriedades e os comandos que o dispositivo implementa.

Abra o arquivo *pnpTemperatureController.js* em um editor de código de sua escolha. O código de exemplo mostra como:

- Definir o `modelId`, que é o DTMI do dispositivo que está sendo implementado. Esse DTMI é definido pelo usuário e deve corresponder ao DTMI do [modelo de DTDL do controlador de temperatura](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/samples/TemperatureController.json).

- Implementar os componentes definidos no modelo de DTDL do controlador de temperatura. Os componentes em um controlador de temperatura real devem implementar essas duas interfaces. Essas duas interfaces já estão publicadas em um repositório central. Neste exemplo, as duas interfaces são:

  - Termostato
  - Informações do dispositivo desenvolvidas pelo Azure

- Definir os nomes dos componentes. Este exemplo tem dois termostatos e um componente de informações do dispositivo.

- Definir o nome do comando. Estes são os comandos aos quais o dispositivo responde.

- Definir a constante `serialNumber`. O `serialNumber` é corrigido é fixo para qualquer dispositivo específico.

- Definir os manipuladores de comando.

- Definir as funções para enviar respostas de comando.

- Definir funções auxiliares para registrar solicitações de comando em log.

- Definir uma função auxiliar para criar as propriedades.

- Definir um ouvinte para atualizações de propriedade.

- Definir uma função para enviar telemetria deste dispositivo. Tanto os termostatos quanto o componente padrão enviam telemetria. Essa função recebe o nome do componente como parâmetro.

- Definir uma função `main` que:

  - Usa o SDK do dispositivo para criar um cliente de dispositivo e conectar-se ao hub IoT. O dispositivo fornece a `modelId` para que o Hub IoT possa identificar o dispositivo como um dispositivo IoT Plug and Play.

  - Inicia a escuta de solicitações de comando usando a função `onDeviceMethod`. A função configura um ouvinte para solicitações de comando do serviço:

    - O dispositivo DTDL define os comandos `reboot` e `getMaxMinReport`.
    - A função `commandHandler` define a maneira que o dispositivo responde a um comando.

  - Inicia o envio de telemetria usando `setInterval` e `sendTelemetry`.

  - Usa a função `helperCreateReportedPropertiesPatch` para criar as propriedades e a `updateComponentReportedProperties` para atualizar as propriedades.

  - Usa `desiredPropertyPatchListener` para escutar atualizações de propriedade.

  - Desabilita todos os ouvintes e tarefas e sai do loop quando você pressiona **Q** ou **q**.

[!INCLUDE [iot-pnp-environment](../../includes/iot-pnp-environment.md)]

Para saber mais sobre a configuração do exemplo, confira o [leiame de exemplo](https://github.com/Azure/azure-iot-sdk-node/blob/master/device/samples/pnp/readme.md).

Agora que você já viu o código, use o seguinte comando para executar o exemplo:

```cmd\sh
node pnpTemperatureController.js
```

Você vê a saída a seguir, indicando que o dispositivo começou a enviar dados telemétricos para o hub e agora está pronto para receber comandos e atualizações de propriedade.

![Mensagens de confirmação do dispositivo](media/tutorial-multiple-components-node/multiple-component.png)

Mantenha o exemplo em execução enquanto você conclui as próximas etapas.

## <a name="use-azure-iot-explorer-to-validate-the-code"></a>Usar o Azure IoT Explorer para validar o código

Após iniciar o exemplo de cliente do dispositivo, use a ferramenta Azure IoT Explorer para verificar se ele está funcionando.

[!INCLUDE [iot-pnp-iot-explorer.md](../../includes/iot-pnp-iot-explorer.md)]

[!INCLUDE [iot-pnp-clean-resources.md](../../includes/iot-pnp-clean-resources.md)]

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você aprendeu a conectar um dispositivo IoT Plug and Play com componentes a um hub IoT. Para saber mais sobre os modelos de dispositivos IoT Plug and Play, confira:

> [!div class="nextstepaction"]
> [Guia do desenvolvedor de modelagem do IoT Plug and Play](concepts-developer-guide-device-csharp.md)

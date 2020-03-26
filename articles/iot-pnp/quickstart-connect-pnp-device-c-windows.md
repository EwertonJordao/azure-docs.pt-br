---
title: Conectar o código do dispositivo de exemplo do IoT Plug and Play Preview ao Hub IoT (Windows) | Microsoft Docs
description: Criar e executar um código do dispositivo de exemplo da versão prévia do IoT Plug and Play no Windows que se conecta a um Hub IoT. Use a ferramenta Azure IoT Explorer para exibir as informações enviadas pelo dispositivo ao hub.
author: ChrisGMsft
ms.author: chrisgre
ms.date: 12/26/2019
ms.topic: quickstart
ms.service: iot-pnp
services: iot-pnp
ms.custom: mvc
ms.openlocfilehash: 1c8b6a5d133d8838c2c7a23f8881092affa424dc
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "75531175"
---
# <a name="quickstart-connect-a-sample-iot-plug-and-play-preview-device-application-running-on-windows-to-iot-hub-c-windows"></a>Início Rápido: Conectar um aplicativo do dispositivo de exemplo da versão prévia do IoT Plug and Play com Windows ao Hub IoT (Windows para C)

[!INCLUDE [iot-pnp-quickstarts-2-selector.md](../../includes/iot-pnp-quickstarts-2-selector.md)]

Este guia de início rápido mostra como criar um aplicativo de exemplo de dispositivo IoT Plug and Play, conectá-lo ao Hub IoT e usar a ferramenta Azure IoT Explorer para exibir as informações que ele envia ao Hub. O aplicativo de exemplo é escrito em C e está incluído no SDK do dispositivo Hub IoT do Azure para C. Um desenvolvedor de soluções pode usar a ferramenta do Azure IoT Explorer para compreender as funcionalidades de um dispositivo IoT Plug and Play sem a necessidade de exibir nenhum código de dispositivo.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este início rápido, você precisa instalar o seguinte software no computador local:

* [Visual Studio (Community, Professional ou Enterprise)](https://visualstudio.microsoft.com/downloads/) – inclua o componente **Gerenciador de pacotes NuGet** e a carga de trabalho **Desenvolvimento para desktop com C++** ao instalar o Visual Studio.
* [Git](https://git-scm.com/download/).
* [CMake](https://cmake.org/download/).

### <a name="install-the-azure-iot-explorer"></a>Instalar o Azure IoT Explorer

Baixe e instale a versão mais recente do **Explorador do Azure IoT** na página do [repositório](https://github.com/Azure/azure-iot-explorer/releases) da ferramenta, selecionando o arquivo .msi em "Assets" (Ativos) para obter a atualização mais recente.

[!INCLUDE [iot-pnp-prepare-iot-hub.md](../../includes/iot-pnp-prepare-iot-hub.md)]

Execute o seguinte comando para obter a _cadeia de conexão do hub IoT_ para o seu hub (nota para usar mais tarde):

```azurecli-interactive
az iot hub show-connection-string --hub-name <YourIoTHubName> --output table
```

## <a name="prepare-the-development-environment"></a>Preparar o ambiente de desenvolvimento

Neste início rápido, você preparará um ambiente de desenvolvimento que pode ser usado para clonar e compilar o SDK do dispositivo Hub IoT do Azure para C.

Abra um prompt de comando no diretório de sua escolha. Execute o seguinte comando para clonar o repositório GitHub dos [SDKs de C e Bibliotecas do IoT do Azure](https://github.com/Azure/azure-iot-sdk-c) nesta localização:

```cmd/sh
git clone https://github.com/Azure/azure-iot-sdk-c --recursive -b public-preview
```

Essa operação deve demorar alguns minutos.

## <a name="build-the-code"></a>Compilar o código

Use o SDK do dispositivo para criar o código de exemplo incluído. O aplicativo criado simula um dispositivo que se conecta a um Hub IoT. O aplicativo envia a telemetria e as propriedades e recebe comandos.

1. Crie um subdiretório `cmake` na pasta raiz do SDK do dispositivo e navegue até essa pasta:

    ```cmd\sh
    cd <root folder>\azure-iot-sdk-c
    mkdir cmake
    cd cmake
    ```

1. Execute os seguintes comandos para compilar o SDK do dispositivo e o stub do código gerado:

    ```cmd\sh
    cmake ..
    cmake --build . -- /m /p:Configuration=Release
    ```

    > [!NOTE]
    > Se o CMake não conseguir localizar o compilador C++, você obterá erros de build ao executar o comando anterior. Se isso acontecer, tente executar este comando no [prompt de comando do Visual Studio](https://docs.microsoft.com/dotnet/framework/tools/developer-command-prompt-for-vs).

## <a name="run-the-device-sample"></a>Executar o exemplo de dispositivo

Execute um aplicativo de exemplo no SDK para simular um dispositivo IoT Plug and Play que envia telemetria ao Hub IoT. Para executar o aplicativo de exemplo, use estes comandos e passe a _cadeia de conexão do dispositivo_ como um parâmetro.

```cmd\sh
cd digitaltwin_client\samples\digitaltwin_sample_device\Release
copy ..\EnvironmentalSensor.interface.json .
digitaltwin_sample_device.exe "<YourDeviceConnectionString>"
```

Agora, o dispositivo está pronto para receber comandos e atualizações de propriedade e começou a enviar dados telemétricos para o hub. Mantenha o exemplo em execução enquanto você conclui as próximas etapas.

## <a name="use-the-azure-iot-explorer-to-validate-the-code"></a>Usar o Azure IoT Explorer para validar o código

[!INCLUDE [iot-pnp-iot-explorer-1.md](../../includes/iot-pnp-iot-explorer-1.md)]

4. Para garantir que a ferramenta possa ler as definições de modelo de interface do seu dispositivo, selecione **Configurações**. No menu Configurações, **No dispositivo conectado** pode já aparecer nas configurações de Plug and Play; se não aparecer, selecione **+ Adicionar fonte de definição de módulo** e, em seguida, **No dispositivo conectado** para adicioná-la.

1. Novamente na página de visão geral **Dispositivos**, localize a identidade do dispositivo criada anteriormente. Com o aplicativo do dispositivo ainda em execução no prompt de comando, verifique se o **Estado da conexão** do dispositivo no Azure IoT Explorer está sendo relatado como _Conectado_ (caso contrário, clique em **Atualizar** até que ele esteja). Selecione o dispositivo para exibir mais detalhes.

1. Expanda a interface com a ID **urn:YOUR_COMPANY_NAME_HERE:EnvironmentalSensor:1** para revelar a interface e os primitivos do IoT Plug and Play: propriedades, comandos e telemetria.

[!INCLUDE [iot-pnp-iot-explorer-2.md](../../includes/iot-pnp-iot-explorer-2.md)]

[!INCLUDE [iot-pnp-clean-resources.md](../../includes/iot-pnp-clean-resources.md)]

## <a name="next-steps"></a>Próximas etapas

Neste início rápido, você aprendeu a conectar um dispositivo IoT Plug and Play a um Hub IoT. Para saber mais sobre como criar uma solução que interage com os dispositivos IoT Plug and Play, confira:

> [!div class="nextstepaction"]
> [Instruções: Conectar-se a um dispositivo de IoT Plug and Play Preview e interagir com ele](howto-develop-solution.md)

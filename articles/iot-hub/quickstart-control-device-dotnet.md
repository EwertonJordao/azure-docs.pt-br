---
title: Início rápido de controle de um dispositivo do Hub IoT do Azure (.NET) | Microsoft Docs
description: Neste início rápido, você executa dois aplicativos C# de exemplo. Um deles é um aplicativo back-end que pode controlar remotamente os dispositivos conectados ao seu hub. O outro aplicativo simula um dispositivo conectado ao seu hub que pode ser controlado remotamente.
author: robinsh
manager: philmea
ms.author: robinsh
ms.service: iot-hub
services: iot-hub
ms.devlang: csharp
ms.topic: quickstart
ms.custom:
- mvc
- mqtt
ms.date: 03/04/2020
ms.openlocfilehash: 560ab582102cc92689093bb0e36acf2fcbc5a30a
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/29/2020
ms.locfileid: "81771019"
---
# <a name="quickstart-control-a-device-connected-to-an-iot-hub-net"></a>Início Rápido: Controlar um dispositivo conectado a um hub IoT (.NET)

[!INCLUDE [iot-hub-quickstarts-2-selector](../../includes/iot-hub-quickstarts-2-selector.md)]

O Hub IoT é um serviço do Azure que habilita a ingestão de grandes volumes de telemetria de dispositivos na nuvem e o gerenciamento de seus dispositivos IoT pela nuvem para armazenamento e processamento. Neste início rápido, você usa um *método direto* para controlar um dispositivo simulado conectado ao seu hub IoT. Você pode usar métodos diretos para alterar remotamente o comportamento de um dispositivo conectado ao seu hub IoT.

O início rápido usa dois aplicativos previamente escritos em .NET:

* Um aplicativo de dispositivo simulado que responde aos métodos diretos chamados de um aplicativo de back-end. Para receber as chamadas de método direto, esse aplicativo se conecta a um ponto de extremidade específico do dispositivo em seu hub IoT.

* Um aplicativo de back-end que chama os métodos diretos no dispositivo simulado. Para chamar um método direto em um dispositivo, esse aplicativo se conecta a um ponto de extremidade do lado do serviço em seu hub IoT.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Se você não tiver uma assinatura do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

Os dois exemplos de aplicativo executados neste início rápido são escritos usando o C#. É necessário ter o SDK do .NET Core 2.1.0 ou maior no computador de desenvolvimento.

Você pode fazer o download do SDK do .NET Core para várias plataformas a partir do [.NET](https://www.microsoft.com/net/download/all).

Verifique a versão atual do C# no computador de desenvolvimento usando o seguinte comando:

```cmd/sh
dotnet --version
```

Execute o comando a seguir para adicionar a Extensão do Microsoft Azure IoT para a CLI do Azure à instância do Cloud Shell. A Extensão de IoT adiciona comandos específicos do Hub IoT, do IoT Edge e do DPS (Serviço de Provisionamento de Dispositivos IoT) à CLI do Azure.

```azurecli-interactive
az extension add --name azure-iot
```

[!INCLUDE [iot-hub-cli-version-info](../../includes/iot-hub-cli-version-info.md)]

Caso ainda não tenha feito isso, baixe os exemplos do C# do Azure IoT do https://github.com/Azure-Samples/azure-iot-samples-csharp/archive/master.zip e extraia o arquivo ZIP.

Verifique se a porta 8883 está aberta no firewall. A amostra de dispositivo deste início rápido usa o protocolo MQTT, que se comunica pela porta 8883. Essa porta poderá ser bloqueada em alguns ambientes de rede corporativos e educacionais. Para obter mais informações e maneiras de resolver esse problema, confira [Como se conectar ao Hub IoT (MQTT)](iot-hub-mqtt-support.md#connecting-to-iot-hub).

## <a name="create-an-iot-hub"></a>Crie um hub IoT

Se tiver concluído o [Início Rápido: enviar telemetria de um dispositivo para um hub IoT](quickstart-send-telemetry-dotnet.md), pode ignorar esta etapa.

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-device"></a>Registrar um dispositivo

Se tiver concluído o [Início Rápido: enviar telemetria de um dispositivo para um hub IoT](quickstart-send-telemetry-dotnet.md), pode ignorar esta etapa.

Um dispositivo deve ser registrado no hub IoT antes de poder se conectar. Neste início rápido, você usa o Azure Cloud Shell para registrar um dispositivo simulado.

1. Execute o comando a seguir no Azure Cloud Shell para criar a identidade do dispositivo.

   **YourIoTHubName**: substitua o espaço reservado abaixo pelo nome escolhido para o hub IoT.

   **MyDotnetDevice**: esse é o nome do dispositivo que está sendo registrado. É recomendável usar **MyDotnetDevice** conforme mostrado. Se escolher um nome diferente para seu dispositivo, você também precisará usar esse nome ao longo deste artigo, bem como atualizar o nome do dispositivo nos aplicativos de exemplo antes de executá-los.

    ```azurecli-interactive
    az iot hub device-identity create \
      --hub-name {YourIoTHubName} --device-id MyDotnetDevice
    ```

2. Execute os seguintes comandos no Azure Cloud Shell para obter a _cadeia de conexão de dispositivo_ referente ao dispositivo que você acabou de registrar:

   **YourIoTHubName**: substitua o espaço reservado abaixo pelo nome escolhido para o hub IoT.

    ```azurecli-interactive
    az iot hub device-identity show-connection-string \
      --hub-name {YourIoTHubName} \
      --device-id MyDotnetDevice \
      --output table
    ```

    Tome nota da cadeia de conexão do dispositivo, que se parece com:

   `HostName={YourIoTHubName}.azure-devices.net;DeviceId=MyNodeDevice;SharedAccessKey={YourSharedAccessKey}`

    Você usará esse valor posteriormente no início rápido.

## <a name="retrieve-the-service-connection-string"></a>Recuperar a cadeia de conexão de serviço

Você também precisa de uma _cadeia de conexão de serviço_ do seu hub IoT para permitir que aplicativos de back-end se conectem ao hub e recuperem mensagens. O comando abaixo recupera a cadeia de conexão de serviço para o hub IoT:

```azurecli-interactive
az iot hub show-connection-string --policy-name service --name {YourIoTHubName} --output table
```

Tome nota da cadeia de conexão de serviço, que se parece com:

   `HostName={YourIoTHubName}.azure-devices.net;SharedAccessKeyName=service;SharedAccessKey={YourSharedAccessKey}`

Você usará esse valor posteriormente no início rápido. A cadeia de conexão do serviço é diferente da cadeia de conexão do dispositivo que você anotou na etapa anterior.

## <a name="listen-for-direct-method-calls"></a>Escutar chamadas de método direto

O aplicativo de dispositivo simulado se conecta a um ponto de extremidade específico do dispositivo em seu hub IoT, envia telemetria simulada e escuta chamadas de método direto de seu hub. Neste início rápido, a chamada de método direto do hub informa ao dispositivo para alterar o intervalo de envio da telemetria. O dispositivo simulado envia uma confirmação para o hub depois de executar o método direto.

1. Em uma janela de terminal local, navegue até a pasta raiz do projeto C# de exemplo. Em seguida, navegue até a pasta **iot-hub\Quickstarts\simulated-device-2**.

2. Abra o arquivo **SimulatedDevice.cs** em seu editor de texto preferido.

    Substitua o valor da variável `s_connectionString` pela cadeia de conexão do dispositivo que você anotou anteriormente. Salve suas alterações em **SimulatedDevice.cs**.

3. Na janela de terminal local, execute os seguintes comandos para instalar os pacotes necessários para o aplicativo de dispositivo simulado:

    ```cmd/sh
    dotnet restore
    ```

4. Na janela de terminal local, execute os seguintes comandos para compilar e executar o aplicativo de dispositivo simulado:

    ```cmd/sh
    dotnet run
    ```

    A captura de tela a seguir mostra o resultado à medida que o aplicativo de dispositivo simulado envia telemetria para o seu hub IoT:

    ![Executar o dispositivo simulado](./media/quickstart-control-device-dotnet/SimulatedDevice-1.png)

## <a name="call-the-direct-method"></a>Chamar o método direto

O aplicativo de back-end se conecta a um ponto de extremidade do lado do serviço em seu Hub IoT. O aplicativo faz chamadas de método direto para um dispositivo por meio do hub IoT e escuta as confirmações. Um aplicativo de back-end do Hub IoT normalmente é executado na nuvem.

1. Em outra janela de terminal local, navegue até a pasta raiz do projeto C# de exemplo. Em seguida, navegue até a pasta **iot-hub\Quickstarts\back-end-application**.

2. Abra o arquivo **BackEndApplication.cs** em seu editor de texto preferido.

    Substitua o valor da variável `s_connectionString` pela cadeia de conexão do serviço que você anotou anteriormente. Salve suas alterações em **BackEndApplication.cs**.

3. Na janela de terminal local, execute os seguintes comandos para instalar as bibliotecas necessárias para o aplicativo de back-end:

    ```cmd/sh
    dotnet restore
    ```

4. Na janela de terminal local, execute os comandos a seguir para compilar e executar o aplicativo back-end:

    ```cmd/sh
    dotnet run
    ```

    A seguinte captura de tela mostra a saída enquanto o aplicativo faz uma chamada de método direto ao dispositivo e recebe uma confirmação:

    ![Executar o aplicativo de back-end](./media/quickstart-control-device-dotnet/BackEndApplication.png)

    Após executar o aplicativo de back-end, você verá uma mensagem na janela do console com o dispositivo simulado em execução, e a taxa de mudança de envio das mensagens:

    ![Alteração no cliente simulado](./media/quickstart-control-device-dotnet/SimulatedDevice-2.png)

## <a name="clean-up-resources"></a>Limpar os recursos

[!INCLUDE [iot-hub-quickstarts-clean-up-resources](../../includes/iot-hub-quickstarts-clean-up-resources.md)]

## <a name="next-steps"></a>Próximas etapas

Neste início rápido, você já chamou um método direto em um dispositivo de um aplicativo de back-end e respondeu à chamada de método direto em um aplicativo de dispositivo simulado.

Para saber como rotear mensagens de dispositivo para nuvem para destinos diferentes na nuvem, continue n próximo tutorial.

> [!div class="nextstepaction"]
> [Tutorial: Encaminhar a telemetria para pontos de extremidade diferentes para processamento](tutorial-routing.md)

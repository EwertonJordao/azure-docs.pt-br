---
title: Criar ativos técnicos do módulo Azure IoT Edge | Mercado Azure
description: Crie os recursos técnicos para um módulo IoT Edge.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/06/2020
ms.author: dsindona
ms.openlocfilehash: cd9cd9f3986ef1944d9f9119296af5512b352d1d
ms.sourcegitcommit: bd5fee5c56f2cbe74aa8569a1a5bce12a3b3efa6
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80744309"
---
# <a name="prepare-your-iot-edge-module-technical-assets"></a>Prepare seus recursos técnicos do módulo IoT Edge

>[!Important]
>A partir de 30 de março de 2020, começaremos a mover o gerenciamento das ofertas do seu módulo IoT Edge para o Partner Center. Após a migração, você criará e gerenciará suas ofertas no Partner Center. Siga as instruções em [Criar uma oferta de módulo IoT Edge](https://aka.ms/AzureIoTTechAsset) para gerenciar suas ofertas migradas.

Este artigo descreve os requisitos que os recursos técnicos do módulo IoT Edge precisam atender antes de serem publicados no Azure Marketplace.

## <a name="understanding-iot-edge-modules-and-getting-started"></a>Entendendo os módulos do IoT Edge e começando

Um módulo IoT Edge é um contêiner compatível com Docker que é feito para ser executado em um dispositivo IoT Edge.

- Para saber mais sobre os módulos do IoT Edge, confira [Entender os módulos do Azure IoT Edge](https://docs.microsoft.com/azure/iot-edge/iot-edge-modules).
- Para se familiarizar com o desenvolvimento de módulo do IoT Edge, consulte [requisitos e as ferramentas para desenvolvimento de módulos do IoT Edge](https://docs.microsoft.com/azure/iot-edge/module-development).

## <a name="technical-requirements"></a>Requisitos técnicos

Os requisitos técnicos a seguir devem ser atendidos para que o módulo IoT Edge seja certificado e publicado no Azure Marketplace.

### <a name="platform-support"></a>Suporte a plataforma

Seu módulo IoT Edge deve suportar uma das seguintes opções de plataforma.

#### <a name="tier-1-platforms-supported-by-iot-edge"></a>Plataformas de nível 1 suportadas pelo IoT Edge

Suporta todas as plataformas da Camada 1 suportadas pelo IoT Edge (conforme registrado no [suporte do Azure IoT Edge](https://docs.microsoft.com/azure/iot-edge/support)). Recomendamos essa opção porque fornece uma melhor experiência do cliente. Módulos que atendam a este critério serão exibidos. Um módulo usando esta opção de plataforma deve:

- Forneça uma `latest`tag e uma tag de versão (por exemplo,`1.0.1`) que sejam tags de manifesto construídas com a ferramenta de [manifesto do GitHub](https://github.com/estesp/manifest-tool).
- Use a [guia Mercado](./cpp-marketplace-tab.md) para adicionar um link a [dispositivos compatíveis com a IoT Edge compatíveis](https://aka.ms/iot-edge-certified). Este link resolve para `https://aka.ms/iot-edge-certified`, um website em que os clientes podem navegar ou pesquisar dispositivos certificados. Este site também é conhecido como o catálogo de dispositivos do [Azure IoT Edge Certified](https://catalog.azureiotsolutions.com/).

#### <a name="a-subset-of-tier-1-platforms-supported-by-iot-edge"></a>Um subconjunto das plataformas de nível 1 com suporte do IoT Edge
  
Suportar um subconjunto (pelo menos um) de plataformas de Nível 1 suportadas pelo IoT Edge (conforme registrado no [suporte do Azure IoT Edge](https://docs.microsoft.com/azure/iot-edge/support)). Um módulo usando esta opção de plataforma deve:

- Forneça uma `latest`tag e uma tag de versão (por exemplo, `1.0.1`) que sejam tags de manifesto construídas com a [manifest-tool](https://github.com/estesp/manifest-tool) do GitHub se mais de uma plataforma for suportada. Tags de manifesto são opcionais somente quando uma plataforma é suportada
- Use a guia [Marketplace](./cpp-marketplace-tab.md) para fornecer um link para pelo menos um dispositivo IoT Edge no catálogo de dispositivos do [Azure IoT Edge Certified](https://catalog.azureiotsolutions.com/).

### <a name="device-dimensions"></a>Device dimensions

As dimensões do módulo IoT Edge (CPU / RAM / Armazenamento / GPU / etc.) Nos dispositivos IoT Edge de destino devem atender aos seguintes requisitos:

- O módulo deve **trabalhar com pelo menos um dispositivo** certificado pela IoT Edge no catálogo de dispositivos do [Azure IoT Edge Certified](https://catalog.azureiotsolutions.com/).
- Os **Requisitos mínimos de hardware** devem ser documentados como o último parágrafo na descrição da oferta (sob a [guia Marketplace](./cpp-marketplace-tab.md)). Opcionalmente, você também pode listar os requisitos de hardware recomendados se eles forem significativamente diferentes. Por exemplo, adicione a seguinte seção no final da descrição da sua oferta:

  ```html
    <p><u>Minimum hardware requirements:</u> Linux x64 and arm32  OS, 1GB of RAM, 500 Mb of storage</p>
  ```

### <a name="configuration"></a>Configuração

Ele também inclui as definições de configuração padrão para simplificar a implantação em um dispositivo IoT Edge. O contêiner também pode incluir o SDK do Módulo de Borda da IoT para ativar a comunicação com o hub EdgeHub e IoT.

#### <a name="default-configuration"></a>Configuração padrão

Os módulos IoT Edge devem poder começar com as configurações padrão fornecidas na guia [SKU do portal Cloud Partner](./cpp-skus-tab.md). As seguintes configurações padrão estão disponíveis:

- Padrão **rotas**
- Padrão **propriedades desejadas do gêmeo**
- Padrão **variáveis de ambiente**
- Padrão **createOptions**

Em um cenário em que um parâmetro necessário para um valor padrão não faz sentido (por exemplo, o endereço IP do servidor de um cliente), você adiciona um parâmetro como o valor padrão. Este valor é colocado entre colchetes e em maiúsculas. Para este exemplo, você configuraria a seguinte variável de ambiente padrão:

```
    ServerIPAddress = <MY_SERVER_IP_ADDRESS>
```

#### <a name="configuration-documentation"></a>Documentação de configuração

Todas as configurações de um módulo IoT Edge devem ser claramente documentadas (como usar suas rotas, propriedades desejadas duplas, variáveis de ambiente, createOptions e assim por diante.) Forneça um link para sua documentação, ou a documentação deve fazer parte da sua descrição de oferta/sku.

### <a name="tags-and-versioning"></a>Marcas e controle de versão

Os clientes devem ser capazes de implantar facilmente um módulo e obter automaticamente atualizações do mercado (em um cenário de desenvolvedor.) Eles também devem ser capazes de usar e congelar uma versão exata que testaram (em um cenário de produção.)

Para atender às expectativas desses clientes e ser publicado no mercado, os módulos IoT Edge devem atender aos seguintes requisitos:

- Inclua uma tag `latest` de manifesto que aponte a versão mais recente para todas as plataformas compatíveis.
- As tags de versão devem ter o formato X.Y.Z, onde X, Y e Z são inteiros.
- Inclua uma tag "versão", como `1.0.1`, que aponte para uma versão específica em todas as plataformas compatíveis.
- Não atualize as tags "versão", como `1.0.1`, porque elas devem ser imutáveis.

>[!Note]
>Opcionalmente, o controle de versão pode incluir marcas "distribuir a versão", como `2.0` e `1.0`. Isso suporta a manutenção de várias versões principais em paralelo.

### <a name="telemetry"></a>Telemetria

Os módulos que usam o SDK do módulo IoT devem definir o identificador de módulo exclusivo como `PublisherId.OfferId.SkuId` para fins de telemetria. Um identificador exclusivo permite que o Azure Marketplace identifique o número de instâncias do módulo em execução.

 Use os métodos a seguir dos SDKs do módulo IoT para definir o `ProductInfo` como esse identificador:

- [C\#](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.deviceclient.productinfo?view=azure-dotnet#Microsoft_Azure_Devices_Client_DeviceClient_ProductInfo) 
- [C](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/Iothub_sdk_options.md)
- [Python](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/Iothub_sdk_options.md)
- [Java](https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.device.productinfo?view=azure-java-stable)

Para os módulos que não usam o SDK do módulo IoT, insights menos precisos estão disponíveis no Cloud Partner Portal, como o número de downloads.

### <a name="security"></a>Segurança

Os módulos IoT Edge devem solicitar o acesso menos privilegiado ao host possível. [Módulo Privilegiado](https://docs.docker.com/engine/reference/run/#runtime-privilege-and-linux-capabilities) deve ser evitado.

### <a name="module-iot-sdk"></a>SDK de IoT de módulo

Incluir o SDK do Módulo IoT não é um pré-requisito para a certificação. No entanto, incluir o SDK do Módulo do IoT pode fornecer uma melhor experiência ao usuário. Por exemplo, para dar suporte ao roteamento ou ao enviar mensagens para a nuvem.

O SDK do módulo IoT é necessário para obter dados de telemetria sobre o número de instâncias do módulo em execução.


## <a name="recertification-process"></a>Processo recertificação

<!-- Add legal time windows-->
 Os parceiros serão notificados sempre que houver uma alteração significativa que afete seus módulos, como:

- Camada de suporte de os / arch de camada 1 suportada pelo IoT Edge
- Módulo do IoT do SDK
- Runtime do IoT Edge
- As diretrizes de certificação do módulo do IoT Edge

Os parceiros terão que atualizar seus módulos e recertificá-los usando a ferramenta Cloud Partner Portal.

## <a name="host-your-iot-edge-module-in-an-azure-container-registry"></a>Hospede seu módulo IoT Edge em um Registro de Contêiner do Azure

Para carregar seu módulo IoT Edge no Cloud Partner Portal, primeiro você precisa hospedá-lo em um [Azure Container Registry](https://azure.microsoft.com/services/container-registry/) (ACR). O módulo deve incluir todas as tags que você deseja publicar, incluindo as tags de imagem referenciadas por uma tag de manifesto.


## <a name="next-steps"></a>Próximas etapas

- [Criar sua oferta de módulo do IoT Edge](./cpp-create-offer.md)

---
author: baanders
description: incluir arquivo dos tutoriais dos Gêmeos Digitais do Azure – configurando o projeto de exemplo
ms.service: digital-twins
ms.topic: include
ms.date: 5/25/2020
ms.author: baanders
ms.openlocfilehash: 4ac748c606d8ec3c8ba754c34d9c9e7512344a83
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91292638"
---
## <a name="configure-the-sample-project"></a>Configurar o projeto de exemplo

A seguir, configure um aplicativo cliente de exemplo que vai interagir com sua instância dos Gêmeos Digitais do Azure.

Navegue no computador até o arquivo baixado anteriormente de [*Amostras dos Gêmeos Digitais do Azure*](https://docs.microsoft.com/samples/azure-samples/digital-twins-samples/digital-twins-samples) (e descompacte-o se ainda não tiver feito isso).

Na pasta, navegue até _AdtSampleApp_. Abra _**AdtE2ESample.sln**_ no Visual Studio 2019. 

No Visual Studio, use o painel do *Gerenciador de Soluções* para criar uma cópia do arquivo _SampleClientApp > **serviceConfig.json.TEMPLATE**_ (você pode usar os menus de seleção com o botão direito do mouse para copiar e colar). Renomeie a cópia como *serviceConfig.json*. Ele funcionará como um arquivo JSON predefinido com as variáveis de configuração necessárias para executar o projeto.

Selecione o arquivo *serviceConfig.json* para abri-lo na janela de edição. Altere o `tenantId` para a *ID do diretório*, `clientId` para a *ID do aplicativo* e `instanceUrl` para a URL do *nome do host* de sua instância dos Gêmeos Digitais do Azure (com *https://* na frente, conforme mostrado abaixo).

```json
{
  "tenantId": "<your-directory-ID>",
  "clientId": "<your-application-ID>",
  "instanceUrl": "https://<your-Azure-Digital-Twins-instance-hostName>"
}
```



Salve e feche o arquivo. 

Em seguida, configure o arquivo *serviceConfig.json* para ser copiado para o diretório de saída quando você criar o *SampleClientApp*. Para fazer isso, selecione com o botão direito do mouse o arquivo *serviceConfig.json* e escolha *Propriedades*. No inspetor de *Propriedades*, altere o valor da propriedade *Copiar para o Diretório de Saída* para *Copiar se mais recente*.

:::image type="content" source="../articles/digital-twins/media/includes/copy-config.png" alt-text="Trecho da janela do Visual Studio mostrando o painel do Gerenciador de Soluções com serviceConfig.json realçado e o painel Propriedades com a propriedade 'Copiar para o Diretório de Saída' definida como 'Copiar se mais recente'" border="false":::

Mantenha o projeto _**AdtE2ESample**_ aberto no Visual Studio para continuar a usá-lo no tutorial.


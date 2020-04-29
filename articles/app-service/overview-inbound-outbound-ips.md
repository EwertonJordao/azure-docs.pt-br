---
title: Endereços IP de entrada/saída
description: Saiba como os endereços IP de entrada e saída são usados no serviço Azure App, quando eles mudam e como encontrar os endereços para seu aplicativo.
ms.topic: article
ms.date: 06/06/2019
ms.custom: seodec18
ms.openlocfilehash: 8bcd80fde95e467513590f3ed09b1dadd2646aee
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81537620"
---
# <a name="inbound-and-outbound-ip-addresses-in-azure-app-service"></a>Endereços IP de entrada e saída no Serviço de Aplicativo do Azure

O [Serviço de Aplicativo do Azure](overview.md) é um serviço multilocatário, exceto para [Ambientes de Serviço de Aplicativo](environment/intro.md). Aplicativos que não estão em um ambiente de Serviço de Aplicativo (não na [Camada isolada](https://azure.microsoft.com/pricing/details/app-service/)) compartilham a infraestrutura de rede com outros aplicativos. Como resultado, os endereços IP de entrada e saída de um aplicativo podem ser diferentes e até alterar em determinadas situações. 

[Ambientes de Serviço de Aplicativo](environment/intro.md) usam infraestruturas de rede dedicadas para que aplicativos executados em um ambiente de Serviço de Aplicativo obtenham endereços IP dedicados e estáticos para conexões de entrada e saída.

## <a name="when-inbound-ip-changes"></a>Quando ocorrem alterações do IP de entrada

Independentemente do número de instâncias dimensionadas, cada aplicativo tem um único endereço IP de entrada. O endereço IP de entrada pode alterar ao executar uma das ações a seguir:

- Excluir um aplicativo e recriá-lo em um grupo de recursos diferente.
- Excluir o último aplicativo em uma combinação de grupo de recursos _e_ região e recriá-lo.
- Exclua uma associação TLS existente, como durante a renovação do certificado (consulte [renovar certificado](configure-ssl-certificate.md#renew-certificate)).

## <a name="find-the-inbound-ip"></a>Localizar o IP de entrada

Basta executar o seguinte comando em um terminal local:

```bash
nslookup <app-name>.azurewebsites.net
```

## <a name="get-a-static-inbound-ip"></a>Obter um IP de entrada estático

Às vezes, é possível querer um endereço IP dedicado e estático para o aplicativo. Para obter um endereço IP de entrada estático, você precisa [proteger um domínio personalizado](configure-ssl-bindings.md#secure-a-custom-domain). Se você não precisar realmente da funcionalidade de TLS para proteger seu aplicativo, você pode até mesmo carregar um certificado autoassinado para essa associação. Em uma associação TLS baseada em IP, o certificado é associado ao endereço IP em si, portanto, o serviço de aplicativo provisiona um endereço IP estático para que ele aconteça. 

## <a name="when-outbound-ips-change"></a>Quando ocorrem alterações dos IPs de saída

Independentemente do número de instâncias dimensionadas, cada aplicativo tem um número definido de endereços IP de saída a qualquer momento. Qualquer conexão de saída do aplicativo do Serviço de Aplicativo, como um banco de dados back-end, usa um dos endereços IP de saída como o endereço IP de origem. Não é possível saber antecipadamente o endereço IP que uma determinada instância de aplicativo usará para estabelecer a conexão de saída, portanto, o serviço de back-end deverá abrir o firewall para todos os endereços IP de saída do aplicativo.

O conjunto de endereços IP de saída para o aplicativo é alterado quando você dimensiona o aplicativo entre as camadas inferiores (**Básico**, **Standard** e **Premium**) e a camada **Premium V2**.

Você pode encontrar o conjunto de todos os endereços IP de saída possíveis que seu aplicativo pode usar, independentemente dos tipos de preço, procurando `possibleOutboundIpAddresses` a propriedade ou no campo **endereços IP de saída adicionais** na folha **Propriedades** no portal do Azure. Consulte [Localizar IPs de saída](#find-outbound-ips).

## <a name="find-outbound-ips"></a>Localizar IPs de saída

Para localizar os endereços IP de saída atualmente usados pelo aplicativo no Portal do Azure, clique em **Propriedades** na navegação esquerda do aplicativo. Eles são listados no campo **endereços IP de saída** .

É possível localizar as mesmas informações, executando o comando a seguir no [Cloud Shell](../cloud-shell/quickstart.md).

```azurecli-interactive
az webapp show --resource-group <group_name> --name <app_name> --query outboundIpAddresses --output tsv
```

```azurepowershell
(Get-AzWebApp -ResourceGroup <group_name> -name <app_name>).OutboundIpAddresses
```

Para localizar _todos os_ endereços IP de saída possíveis para seu aplicativo, independentemente dos tipos de preço, clique em **Propriedades** na navegação esquerda do seu aplicativo. Eles são listados no campo **endereços IP de saída adicionais** .

É possível localizar as mesmas informações, executando o comando a seguir no [Cloud Shell](../cloud-shell/quickstart.md).

```azurecli-interactive
az webapp show --resource-group <group_name> --name <app_name> --query possibleOutboundIpAddresses --output tsv
```

```azurepowershell
(Get-AzWebApp -ResourceGroup <group_name> -name <app_name>).PossibleOutboundIpAddresses
```

## <a name="next-steps"></a>Próximas etapas

Saiba como restringir o tráfego de entrada por endereços IP de origem.

> [!div class="nextstepaction"]
> [Restrições de IP estático](app-service-ip-restrictions.md)

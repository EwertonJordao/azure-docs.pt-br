---
title: Usar a CLI para implantar VMs de ponto do Azure (versão prévia)
description: Saiba como usar a CLI para implantar VMs pontuais do Azure para economizar custos.
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/11/2020
ms.author: cynthn
ms.openlocfilehash: 110e935671ab1d640b2ff3dc26c203b262e999fe
ms.sourcegitcommit: 76bc196464334a99510e33d836669d95d7f57643
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/12/2020
ms.locfileid: "77163085"
---
# <a name="preview-deploy-spot-vms-using-the-azure-cli"></a>Versão prévia: implantar VMs especiais usando o CLI do Azure

O uso de [VMs do Azure Spot](spot-vms.md) permite que você aproveite a capacidade não utilizada a uma economia de custo significativa. A qualquer momento, quando o Azure precisar da capacidade de volta, a infraestrutura do Azure removerá as VMs pontuais. Portanto, as VMs pontuais são ótimas para cargas de trabalho que podem lidar com interrupções como trabalhos de processamento em lotes, ambientes de desenvolvimento/teste, grandes cargas de trabalho de computação e muito mais.

Os preços para VMs pontuais são variáveis, com base na região e SKU. Para obter mais informações, consulte preços de VM para [Linux](https://azure.microsoft.com/pricing/details/virtual-machines/linux/) e [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/windows/). 

Você tem a opção de definir um preço máximo que está disposto a pagar, por hora, para a VM. O preço máximo de uma VM Spot pode ser definido em dólares americanos (USD), usando até 5 casas decimais. Por exemplo, o valor `0.98765`seria um preço máximo de $0.98765 USD por hora. Se você definir o preço máximo como `-1`, a VM não será removida com base no preço. O preço da VM será o preço atual para o ponto ou o preço de uma VM padrão, o que nunca é menor, desde que haja capacidade e cota disponível. Para obter mais informações sobre como definir o preço máximo, consulte [VMs pontuais – preços](spot-vms.md#pricing).

O processo para criar uma VM com o Spot usando o CLI do Azure é o mesmo detalhado no [artigo de início rápido](/azure/virtual-machines/linux/quick-create-cli). Basta adicionar o parâmetro '--priority spot ' e fornecer um preço máximo ou `-1`.

> [!IMPORTANT]
> Atualmente, as instâncias especiais estão em visualização pública.
> Esta versão de visualização não é recomendada para cargas de trabalho de produção. Alguns recursos podem não ter suporte ou podem ter restrição de recursos. Para obter mais informações, consulte [Termos de Uso Complementares de Versões Prévias do Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>



## <a name="install-azure-cli"></a>Instalar a CLI do Azure.

Para criar VMs pontuais, você precisa estar executando o CLI do Azure versão 2.0.74 ou posterior. Execute **az --version** para descobrir a versão. Se você precisar instalar ou atualizar, confira [Instalar a CLI do Azure](/cli/azure/install-azure-cli). 

Entrar no Azure usando [login az](/cli/azure/reference-index#az-login).

```azurecli
az login
```

## <a name="create-a-spot-vm"></a>Criar uma VM Spot

Este exemplo mostra como implantar uma VM de ponto do Linux que não será removida com base no preço. 

```azurecli
az group create -n mySpotGroup -l eastus
az vm create \
    --resource-group mySpotGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --priority Spot \
    --max-price -1
```

Depois que a VM for criada, você poderá consultar para ver o preço máximo de cobrança de todas as VMs no grupo de recursos.

```azurecli
az vm list \
   -g mySpotGroup \
   --query '[].{Name:name, MaxPrice:billingProfile.maxPrice}' \
   --output table
```

**Próximas etapas**

Você também pode criar uma VM Spot usando [Azure PowerShell](../windows/spot-powershell.md) ou um [modelo](spot-template.md).

Se você encontrar um erro, consulte [códigos de erro](../error-codes-spot.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

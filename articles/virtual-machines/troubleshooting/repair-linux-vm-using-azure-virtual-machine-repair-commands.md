---
title: Reparar uma VM do Linux usando os comandos de reparo da máquina virtual do Azure | Microsoft Docs
description: Este artigo fornece detalhes sobre como usar os comandos de reparo de máquina virtual do Azure para conectar o disco a outra VM Linux para corrigir erros e, em seguida, recriar sua VM original.
services: virtual-machines-linux
documentationcenter: ''
author: v-miegge
manager: dcscontentpm
editor: ''
tags: virtual-machines
ms.service: virtual-machines
ms.topic: troubleshooting
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.date: 09/10/2019
ms.author: v-miegge
ms.openlocfilehash: 49fdfde402938ce8d0ee1b141a47e68c99c502e7
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2020
ms.locfileid: "73796211"
---
# <a name="repair-a-linux-vm-by-using-the-azure-virtual-machine-repair-commands"></a>Reparar uma VM do Linux usando os comandos de reparo da Máquina Virtual do Azure

Se sua VM (máquina virtual) do Linux no Azure encontrar um erro de disco ou de inicialização, talvez seja necessário executar a mitigação no próprio disco. Um exemplo comum seria uma atualização de aplicativo com falha que impede a inicialização bem-sucedida da VM. Este artigo fornece detalhes sobre como usar os comandos de reparo de máquina virtual do Azure para conectar o disco a outra VM Linux para corrigir erros e, em seguida, recriar sua VM original.

> [!IMPORTANT]
> Os scripts neste artigo se aplicam somente às VMs que usam [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview).

## <a name="repair-process-overview"></a>Visão geral do processo de reparo

Agora você pode usar os comandos de reparo da máquina virtual do Azure para alterar o disco do sistema operacional de uma VM e não precisa mais excluir e recriar a VM.

Siga estas etapas para solucionar o problema da VM:

1. Iniciar o Azure Cloud Shell
2. Executar adicionar/atualizar extensão AZ
3. Executar AZ VM Repair Create
4. Executar etapas de mitigação
5. Executar AZ VM Repair Restore

Para obter mais informações e documentação, consulte [AZ VM Repair](https://docs.microsoft.com/cli/azure/ext/vm-repair/vm/repair).

## <a name="repair-process-example"></a>Exemplo de processo de reparo

1. Iniciar o Azure Cloud Shell

   O Azure Cloud Shell é um shell interativo grátis que pode ser usado para executar as etapas neste artigo. Ele inclui ferramentas comuns do Azure pré-instalados e configuradas para usar com sua conta.

   Para abrir o Cloud Shell, selecione **Experimente** no canto superior direito de um bloco de código. Você também pode abrir o Cloud Shell em uma guia separada do navegador indo até [https://shell.azure.com](https://shell.azure.com).

   Selecione **copiar** para copiar os blocos de código e, em seguida, Cole o código no Cloud Shell e selecione **Enter** para executá-lo.

   Se preferir instalar e usar a CLI localmente, este início rápido exigirá a CLI do Azure versão 2.0.30 ou posterior. Execute ``az --version`` para encontrar a versão. Se você precisar instalar ou atualizar seu CLI do Azure, consulte [instalar CLI do Azure](https://docs.microsoft.com/cli/azure/install-azure-cli).

2. Se esta for a primeira vez que você usou os `az vm repair` comandos, adicione a extensão da CLI de reparo de VM.

   ```azurecli-interactive
   az extension add -n vm-repair
   ```

   Se você tiver usado os `az vm repair` comandos anteriormente, aplique todas as atualizações à extensão de reparo de VM.

   ```azurecli-interactive
   az extension update -n vm-repair
   ```

3. Execute `az vm repair create`. Esse comando criará uma cópia do disco do sistema operacional para a VM não funcional, criará uma VM de reparo e anexará o disco.

   ```azurecli-interactive
   az vm repair create -g MyResourceGroup -n myVM --repair-username username --repair-password password!234 --verbose
   ```

4. Execute as etapas de mitigação necessárias na VM de reparo criada e vá para a etapa 5.

5. Execute `az vm repair restore`. Esse comando alternará o disco do sistema operacional reparado com o disco do sistema operacional original da VM.

   ```azurecli-interactive
   az vm repair restore -g MyResourceGroup -n MyVM --verbose
   ```

## <a name="verify-and-enable-boot-diagnostics"></a>Verificar e habilitar o diagnóstico de boot

O seguinte exemplo habilita a extensão de diagnóstico na VM chamada ``myVMDeployed`` no grupo de recursos chamado ``myResourceGroup``:

CLI do Azure

```azurecli-interactive
az vm boot-diagnostics enable --name myVMDeployed --resource-group myResourceGroup --storage https://mystor.blob.core.windows.net/
```

## <a name="next-steps"></a>Próximas etapas

* Se você estiver tendo problemas para se conectar à sua VM, consulte [solucionar problemas de conexões RDP para uma máquina virtual do Azure](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/troubleshoot-rdp-connection).
* Para problemas com o acesso a aplicativos executados em sua VM, consulte [solucionar problemas de conectividade de aplicativos em máquinas virtuais no Azure](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/troubleshoot-app-connection).
* Para obter mais informações sobre como usar o Resource Manager, consulte [Visão geral do Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview).

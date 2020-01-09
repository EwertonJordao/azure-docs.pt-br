---
title: Configuração pós-implantação usando extensões
description: Saiba como usar extensões de modelo do Azure Resource Manager para fornecer configurações de pós-implantação.
author: mumian
ms.topic: conceptual
ms.date: 12/14/2018
ms.author: jgao
ms.openlocfilehash: 5aa2f06d24a54a4ee5ea01330d3326f76faa41ac
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75478091"
---
# <a name="provide-post-deployment-configurations-by-using-extensions"></a>Fornecer configurações de pós-implantação usando extensões

As extensões de modelo são pequenos aplicativos que fornecem tarefas de configuração e automação pós-implantação e tarefas de automação nos recursos do Azure. Mais popular são as extensões de máquina virtual. Consulte [Recursos e extensões de máquina Virtual para Windows](../../virtual-machines/extensions/features-windows.md), e [recursos e extensões de máquina Virtual para Linux](../../virtual-machines/extensions/features-linux.md).

## <a name="extensions"></a>Extensões

As extensões existentes são:

- [Microsoft.Compute/virtualMachines/extensions](https://docs.microsoft.com/azure/templates/microsoft.compute/2018-10-01/virtualmachines/extensions)
- [Microsoft.Compute virtualMachineScaleSets/extensions](https://docs.microsoft.com/azure/templates/microsoft.compute/2018-10-01/virtualmachinescalesets/extensions)
- [Microsoft.HDInsight clusters/extensions](https://docs.microsoft.com/azure/templates/microsoft.hdinsight/2018-06-01-preview/clusters/extensions)
- [Microsoft.Sql servers/databases/extensions](https://docs.microsoft.com/azure/templates/microsoft.sql/2014-04-01/servers/databases/extensions) 
- [Microsoft.Web/sites/siteextensions](https://docs.microsoft.com/azure/templates/microsoft.web/2016-08-01/sites/siteextensions)

Para descobrir as extensões disponíveis, navegue até a [referência de modelo](https://docs.microsoft.com/azure/templates/). Em **Filtrar por título**, insira **extensão**.

Para saber como usar essas extensões, consulte:

- [Tutorial: implantar extensões de máquina virtual com modelos de Azure Resource Manager](template-tutorial-deploy-vm-extensions.md).
- [Tutorial: importar arquivos BACPAC do SQL com modelos de Azure Resource Manager](template-tutorial-deploy-sql-extensions-bacpac.md)

## <a name="next-steps"></a>Próximos passos

> [!div class="nextstepaction"]
> [Tutorial: implantar extensões de máquina virtual com modelos do Azure Resource Manager](template-tutorial-deploy-vm-extensions.md)

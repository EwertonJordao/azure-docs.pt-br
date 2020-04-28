---
title: Dados pessoais
description: Saiba como gerenciar dados pessoais associados às operações do Azure Resource Manager.
ms.topic: conceptual
ms.date: 05/14/2018
ms.openlocfilehash: 22cfc1b6096980f3d10db404a1c4e02f2de355d2
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2020
ms.locfileid: "75485254"
---
# <a name="manage-personal-data-associated-with-azure-resource-manager"></a>Gerenciar dados pessoais associados com o Azure Resource Manager

Para evitar a exposição de informações confidenciais, exclua todas as informações pessoais fornecidas em implantações, grupos de recursos ou marcas. O Azure Resource Manager fornece operações que permitem que você gerencie dados pessoais fornecidos em implantações, grupos de recursos ou marcas.

[!INCLUDE [Handle personal data](../../../includes/gdpr-intro-sentence.md)]

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="delete-personal-data-in-deployment-history"></a>Excluir dados pessoais no histórico de implantação

Para implantações, o Resource Manager mantém valores de parâmetro e mensagens de status no histórico de implantação. Esses valores são mantidos até que você exclua a implantação do histórico. Para ver se você forneceu dados pessoais nesses valores, lista as implantações. Se você encontrar dados pessoais, exclua as implantações do histórico.

Para listar **implantações** no histórico, use:

* [Listar por Grupo de Recursos](/rest/api/resources/deployments/listbyresourcegroup)
* [Get-AzResourceGroupDeployment](/powershell/module/az.resources/Get-AzResourceGroupDeployment)
* [az group deployment list](/cli/azure/group/deployment#az-group-deployment-list)

Para excluir **implantações** do histórico, use:

* [Delete (excluir)](/rest/api/resources/deployments/delete)
* [Remove-AzResourceGroupDeployment](/powershell/module/az.resources/Remove-AzResourceGroupDeployment)
* [az group deployment delete](/cli/azure/group/deployment#az-group-deployment-delete)

## <a name="delete-personal-data-in-resource-group-names"></a>Excluir dados pessoais em nomes de grupo de recursos

O nome do grupo de recursos é mantido até que você exclua o grupo de recursos. Para ver se você forneceu dados pessoais nesses valores, lista os grupos de recursos. Se você encontrar dados pessoais, [mova os recursos](move-resource-group-and-subscription.md) para um novo grupo de recursos e exclua o grupo de recursos com os dados pessoais no nome.

Para listar os **grupos de recursos**, use:

* [Lista](/rest/api/resources/resourcegroups/list)
* [Get-AzResourceGroup](/powershell/module/az.resources/Get-AzResourceGroup)
* [az group list](/cli/azure/group#az-group-list)

Para excluir os **grupos de recursos**, use:

* [Delete (excluir)](/rest/api/resources/resourcegroups/delete)
* [Remove-AzResourceGroup](/powershell/module/az.resources/Remove-AzResourceGroup)
* [az group delete](/cli/azure/group#az-group-delete)

## <a name="delete-personal-data-in-tags"></a>Excluir os dados pessoais em marcas

Nomes e valores de marcas são mantidos até que você exclua ou modifique a marca. Para ver se você forneceu dados pessoais nas marcas, lista as marcas. Se você encontrar dados pessoais, exclua as marcas.

Para listar as **marcas**, use:

* [Lista](/rest/api/resources/tags/list)
* [Get-AzTag](/powershell/module/az.resources/Get-AzTag)
* [az tag list](/cli/azure/tag#az-tag-list)

Para excluir as **marcas**, use:

* [Delete (excluir)](/rest/api/resources/tags/delete)
* [Remove-AzTag](/powershell/module/az.resources/Remove-AzTag)
* [az tag delete](/cli/azure/tag#az-tag-delete)

## <a name="next-steps"></a>Próximas etapas
* Para obter uma visão geral do Azure Resource Manager, confira [O que é o Resource Manager?](overview.md)
---
title: Excluir Azure Active Directory Domain Services | Microsoft Docs
description: Saiba como desabilitar, ou excluir, um Azure Active Directory Domain Services domínio gerenciado usando o portal do Azure
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.assetid: 89e407e1-e1e0-49d1-8b89-de11484eee46
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: conceptual
ms.date: 11/26/2019
ms.author: iainfou
ms.openlocfilehash: e1836f91b8afc1bb4f5b7e141949f3724c57c857
ms.sourcegitcommit: f15f548aaead27b76f64d73224e8f6a1a0fc2262
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/26/2020
ms.locfileid: "77614044"
---
# <a name="delete-an-azure-active-directory-domain-services-managed-domain-using-the-azure-portal"></a>Excluir um Azure Active Directory Domain Services domínio gerenciado usando o portal do Azure

Se você não precisar mais de um domínio gerenciado, poderá excluir uma instância do Azure Active Directory Domain Services (AD DS do Azure). Não há nenhuma opção para desativar ou desabilitar temporariamente um domínio gerenciado AD DS do Azure. Excluir o domínio gerenciado AD DS do Azure não exclui ou, de outra forma, afeta negativamente o locatário do Azure AD. Este artigo mostra como usar o portal do Azure para excluir um domínio gerenciado do Azure AD DS.

> [!WARNING]
> **A exclusão é permanente e não pode ser revertida.**
> Quando você exclui um domínio gerenciado do Azure AD DS, ocorrem as seguintes etapas:
>   * Controladores de domínio para o domínio gerenciado são desprovisionados e removidos da rede virtual.
>   * Os dados em um domínio gerenciado serão excluídos permanentemente. Esses dados incluem UOs personalizadas, GPOs, registros DNS personalizados, entidades de serviço, GMSAs, etc. que você criou.
>   * Os computadores que ingressaram no domínio gerenciado perdem sua relação de confiança com o domínio e precisam ser retirados do domínio.
>       * Você não pode entrar nesses computadores usando credenciais corporativas do AD. Em vez disso, você deve usar as credenciais de administrador local para o computador.

## <a name="delete-the-managed-domain"></a>Excluir o domínio gerenciado

Para excluir um domínio gerenciado AD DS do Azure, conclua as seguintes etapas:

1. Na portal do Azure, procure e selecione **Azure AD Domain Services**.
1. Selecione o nome do seu domínio gerenciado AD DS do Azure, como *aaddscontoso.com*.
1. Na página **Visão Geral**, selecione **Excluir**. Para confirmar a exclusão, digite o nome de domínio do domínio gerenciado novamente e, em seguida, selecione **excluir**.

Pode levar de 15-20 minutos ou mais para excluir o domínio gerenciado AD DS do Azure.

## <a name="next-steps"></a>Próximas etapas

Considere [compartilhar comentários][feedback] para os recursos que você gostaria de ver no Azure AD DS.

Se você quiser começar a usar o Azure AD DS novamente, consulte [criar e configurar uma instância de Azure Active Directory Domain Services][create-instance].

<!-- INTERNAL LINKS -->
[feedback]: contact-us.md
[create-instance]: tutorial-create-instance.md

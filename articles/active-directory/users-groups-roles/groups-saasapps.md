---
title: Usar um grupo para gerenciar o acesso a aplicativos SaaS-Azure AD | Microsoft Docs
description: Como usar grupos no Azure Active Directory para atribuir acesso a aplicativos SaaS que estão integrados ao Azure Active Directory.
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: how-to
ms.date: 11/08/2019
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5d026cd94ba5e51b64a136359ffc76fb59efef2b
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/09/2020
ms.locfileid: "84727901"
---
# <a name="using-a-group-to-manage-access-to-saas-applications"></a>Usar um grupo para gerenciar o acesso a aplicativos SaaS

Usando o Azure Active Directory (Azure AD) com um plano de licença de Azure AD Premium, você pode usar grupos para atribuir acesso a um aplicativo SaaS integrado ao Azure AD. Por exemplo, se você quiser atribuir acesso ao departamento de marketing usar cinco aplicativos SaaS diferentes, você pode criar um grupo que contém os usuários no departamento de marketing e atribuir esse grupo a esses cinco aplicativos SaaS que são necessários para o departamento de marketing. Dessa forma, você pode poupar tempo gerenciando a associação no departamento de marketing em um único lugar. Os usuários são atribuídos ao aplicativo quando eles são adicionados como membros do grupo de marketing, e sua atribuição é removida do aplicativo quando eles são removidos do grupo de marketing. Esse recurso pode ser usado com centenas de aplicativos que você adiciona na Galeria de aplicativos do Azure AD.

> [!IMPORTANT]
> Você pode usar esse recurso somente depois de iniciar uma Azure AD Premium avaliação ou compra Azure AD Premium plano de licença.
> A atribuição baseada em grupo tem suporte apenas para grupos de segurança.
> No momento, as associações de grupo aninhadas não têm suporte para atribuição com base em grupo de aplicativos.

## <a name="to-assign-access-for-a-user-or-group-to-a-saas-application"></a>Para atribuir acesso de um usuário ou um grupo a um aplicativo SaaS

1. No [Centro de administração do Azure AD](https://aad.portal.azure.com), selecione **Aplicativos empresariais**.
2. Selecione um aplicativo que você adicionou da Galeria de Aplicativos para abri-lo.
3. Selecione **Usuários e grupos** e, em seguida, selecione **Adicionar usuário**.
4. Em **Adicionar atribuição**, selecione **Usuários e grupos** para abrir a lista de seleção **usuários e grupos**.
6. Selecione quantos grupos ou usuários desejar, em seguida, clique ou toque em **Selecione** para adicioná-los à lista de **Adicionar atribuição**. Você também pode atribuir uma função a um usuário neste estágio.
7. Selecione **Atribuir** para atribuir usuários ou grupos ao aplicativo empresarial selecionado.

## <a name="next-steps"></a>Próximas etapas
Esses artigos fornecem mais informações sobre o Active Directory do Azure.

* [Gerenciando o acesso a recursos com grupos de Azure Active Directory](../fundamentals/active-directory-manage-groups.md)
* [Gerenciamento de aplicativos no Microsoft Azure Active Directory](../manage-apps/what-is-application-management.md)
* [Cmdlets do Azure Active Directory para definir configurações de grupo](groups-settings-cmdlets.md)
* [O que é o Azure Active Directory?](../fundamentals/active-directory-whatis.md)
* [Integração de suas identidades locais com o Active Directory do Azure](../hybrid/whatis-hybrid-identity.md)

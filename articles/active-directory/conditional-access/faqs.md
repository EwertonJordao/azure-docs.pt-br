---
title: Perguntas frequentes sobre acesso condicionado ao diretório ativo do Azure | Microsoft Docs
description: Obtenha respostas para perguntas frequentes sobre o Acesso Condicional no Diretório Ativo do Azure.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: article
ms.date: 01/15/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6842338bd27e4bea3436f0b249380ab773d60de6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77368079"
---
# <a name="azure-active-directory-conditional-access-faqs"></a>Perguntas frequentes sobre o acesso condicionado do diretório ativo do Azure

## <a name="which-applications-work-with-conditional-access-policies"></a>Quais aplicativos funcionam com políticas de acesso condicional?

Para obter informações sobre aplicativos que trabalham com políticas de acesso condicional, consulte [Aplicativos e navegadores que usam regras de acesso condicional no Azure Active Directory](concept-conditional-access-cloud-apps.md).

## <a name="are-conditional-access-policies-enforced-for-b2b-collaboration-and-guest-users"></a>As políticas de acesso condicional são aplicadas para a colaboração B2B e usuários convidados?

As políticas são impostas para usuários de colaboração B2B (entre empresas). No entanto, em alguns casos, um usuário pode não ser capaz de atender aos requisitos da política. Por exemplo, a organização de um usuário convidado pode não oferecer suporte à autenticação multifator. 

## <a name="does-a-sharepoint-online-policy-also-apply-to-onedrive-for-business"></a>A política do SharePoint Online também se aplica ao OneDrive for Business?

Sim. A política do SharePoint Online também se aplica ao OneDrive for Business.

## <a name="why-cant-i-set-a-policy-directly-on-client-apps-like-word-or-outlook"></a>Por que não posso definir uma política diretamente em aplicativos de clientes, como Word ou Outlook?

Uma política de acesso condicional define requisitos para acessar um serviço. É imposta quando ocorre a autenticação para esse serviço. A política não é definida diretamente em um aplicativo cliente. Em vez disso, ela será aplicada quando um cliente chamar um serviço. Por exemplo, um conjunto de políticas no SharePoint se aplica aos clientes que chamam o SharePoint. Um conjunto de políticas no Exchange se aplica ao Outlook.

## <a name="does-a-conditional-access-policy-apply-to-service-accounts"></a>Uma política de acesso condicional se aplica às contas de serviço?

As políticas de acesso condicional aplicam-se a todas as contas de usuário. Isso inclui as contas de usuário usadas como contas de serviço. Muitas vezes, uma conta de serviço que funciona sem vigilância não pode satisfazer os requisitos de uma política de Acesso Condicional. Por exemplo, autenticação multifator pode ser necessária. As contas de serviço podem ser excluídas de uma diretiva usando configurações de gerenciamento de políticas de acesso condicional. 

## <a name="are-microsoft-graph-apis-available-for-configuring-conditional-access-policies"></a>As APIs do Microsoft Graph estão disponíveis para configurar políticas de acesso condicional?

No momento, não. 

## <a name="what-is-the-default-exclusion-policy-for-unsupported-device-platforms"></a>Qual é a política de exclusão padrão para plataformas de dispositivos sem suporte?

Atualmente, as políticas de Acesso Condicional são aplicadas seletivamente aos usuários de dispositivos iOS e Android. Os aplicativos em outras plataformas de dispositivos não são, por padrão, afetados pela política de Acesso Condicional para dispositivos iOS e Android. A administração de locatário poderá substituir a política global para não permitir o acesso a usuários em plataformas sem suporte.

## <a name="how-do-conditional-access-policies-work-for-microsoft-teams"></a>Como funcionam as políticas de acesso condicional para as equipes do Microsoft?

O Microsoft Teams depende muito do Exchange Online e do SharePoint Online para cenários de produtividade de núcleo, como reuniões, calendários e compartilhamento de arquivos. As políticas de acesso condicional definidas para esses aplicativos em nuvem se aplicam às equipes do Microsoft quando um usuário entra diretamente no Microsoft Teams.

O Microsoft Teams também é suportado separadamente como um aplicativo em nuvem nas políticas de acesso condicionado ao diretório ativo do Azure. As políticas de acesso condicional definidas para um aplicativo em nuvem aplicam-se às equipes da Microsoft quando um usuário faz logon. No entanto, sem as políticas corretas em outros aplicativos, como o Exchange Online e o SharePoint Online, os usuários ainda poderão acessar esses recursos diretamente.

Os clientes de área de trabalho do Microsoft Teams para Windows e Mac oferecem suporte a autenticação moderna. A autenticação moderna traz a entrada com base no Azure Active Directory Authentication Library (ADAL) para aplicativos cliente do Microsoft Office entre plataformas.

## <a name="next-steps"></a>Próximas etapas

- Para configurar as políticas de acesso condicional para o seu ambiente, consulte as [práticas recomendadas para acesso condicional no Azure Active Directory](best-practices.md). 

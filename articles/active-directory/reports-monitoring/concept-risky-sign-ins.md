---
title: Relatório de logins arriscados no portal | Microsoft Docs
description: Saiba mais sobre o relatório de entradas de risco no portal do Azure Active Directory
services: active-directory
author: MarkusVi
manager: daveba
ms.assetid: 7728fcd7-3dd5-4b99-a0e4-949c69788c0f
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 03/04/2020
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: b77486064139895799ac5a48327377154f75da6d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78273831"
---
# <a name="risky-sign-ins-report-in-the-azure-active-directory-portal"></a>Relatório de entradas de risco no portal do Azure Active Directory

O Azure AD (Azure Active Directory) detecta ações suspeitas relacionadas às contas de usuário. Para cada ação detectada, um registro chamado **de detecção de risco** é criado. Para obter mais detalhes, consulte [detecções de risco Azure AD](concept-risk-events.md). 

Você pode acessar os relatórios de segurança do [portal do Azure](https://portal.azure.com) selecionando a folha **Azure Active Directory** e, em seguida, navegando até a seção **Segurança**. 

Existem dois relatórios de segurança diferentes que são calculados com base nas detecções de risco:

- **Entradas arriscadas** - uma entrada arriscada é um indicador para uma tentativa de logon que pode ter sido realizada por alguém que não é o proprietário legítimo de uma conta de usuário.

- **Usuários sinalizados para riscos** - um usuário arriscado é um indicador de uma conta de usuário que pode ter sido comprometida. 

![Entradas de risco](./media/concept-risky-sign-ins/10.png)

Para saber como configurar as políticas que desencadeiam essas detecções de risco, consulte [Como configurar a política de risco do usuário](../identity-protection/howto-user-risk-policy.md).  

## <a name="who-can-access-the-risky-sign-ins-report"></a>Quem pode acessar o relatório de entradas de risco?

Os relatórios de entradas de risco estão disponíveis para usuários nas funções a seguir:

- Administrador de segurança
- Administrador global
- Leitor de segurança

Para saber como atribuir funções administrativas a um usuário no Azure Active Directory, veja [Exibir e atribuir funções de administrador no Azure Active Directory](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-manage-roles-portal).

## <a name="what-azure-ad-license-do-you-need-to-access-a-security-report"></a>Qual licença do Azure AD você precisa para acessar a atividade de entrada?  

Todas as edições do Azure AD fornecem relatórios de entradas de risco. No entanto, o nível de granularidade do relatório varia entre as edições: 

- Na **edição Azure Active Directory Free,** você recebe uma lista de logins arriscados. 

- Além disso, a edição **Azure Active Directory Premium 1** permite examinar algumas das detecções de risco subjacentes que foram detectadas para cada relatório. 

- A edição **Azure Active Directory Premium 2** fornece as informações mais detalhadas sobre todas as detecções de risco subjacentes e também permite configurar políticas de segurança que respondam automaticamente aos níveis de risco configurados.

## <a name="risky-sign-ins-report-for-azure-ad-free-edition"></a>Relatório de logins arriscados para edição gratuita do Azure AD

A edição gratuita do Azure AD fornece uma lista de logins arriscados que foram detectados para seus usuários. Cada registro contém os seguintes atributos:

- **Usuário** - O nome do usuário que foi usado durante a operação de login.
- **IP** - O endereço IP do dispositivo usado para se conectar ao Azure Active Directory.
- **Local**: o local usado para se conectar ao Azure Active Directory. Trata-se do melhor esforço de aproximação com base em rastreamentos, dados de registro, pesquisas inversas e outras informações.
- **Hora da entrada** - o horário em que a entrada foi realizada
- **Status** - o status da entrada

![Entradas de risco](./media/concept-risky-sign-ins/01.png)

Com base na investigação de entrada arriscada, você poderá fornecer feedback ao Azure AD realizando as seguintes ações:

- Resolver
- Marcar como falso positivo
- Ignorar
- Reativar

![Entradas de risco](./media/concept-risky-sign-ins/21.png)

Este relatório também fornece uma opção para:

- Recursos do Search
- Baixar os dados do relatório

![Entradas de risco](./media/concept-risky-sign-ins/93.png)

## <a name="risky-sign-ins-report-for-azure-ad-premium-editions"></a>Relatório de entradas arriscadas para as edições Premium do Azure AD

O relatório de entradas arriscadas nas edições Premium do Azure AD fornece:

- Informações agregadas sobre os [tipos de detecção de risco](concept-risk-events.md) detectados. Com a **edição Azure AD Premium P1,** as detecções que não estão cobertas pela sua licença aparecem como o login de detecção de risco **com risco adicional detectado**. Com o **Azure AD Premium P2 Edition**, você obtém as informações mais detalhadas sobre todas as detecções subjacentes.

- Uma opção para baixar o relatório

![Entradas de risco](./media/concept-risky-sign-ins/456.png)

Quando você seleciona uma detecção de risco, você recebe uma visualização detalhada do relatório para essa detecção de risco que lhe permite:

- Uma opção para configurar uma [política de correção de risco de usuário](../identity-protection/howto-user-risk-policy.md)  

- Revise o cronograma de detecção para a detecção de riscos  

- Revise uma lista de usuários para os quais essa detecção de risco foi detectada

- Feche manualmente as detecções de risco. 

![Entradas de risco](./media/concept-risky-sign-ins/457.png)

> [!IMPORTANT]
> Às vezes, você pode encontrar uma detecção de risco sem uma entrada correspondente no [relatório de login](concept-sign-ins.md). Isso ocorre porque o Identity Protection avalia o risco para as entradas **interativa** e **não interativa**, enquanto o relatório de entradas mostra apenas as entradas interativas.

Ao selecionar um usuário, você obtém uma exibição detalhada do relatório deste usuário, que lhe habilita a:

- Abrir a exibição Todas as entradas

- Redefinir a senha do usuário

- Descartar todos os eventos

- Investigue as detecções de risco relatadas para o usuário. 

![Entradas de risco](./media/concept-risky-sign-ins/324.png)

Para investigar uma detecção de risco, selecione um da lista.  
Isso abre a lâmina **Detalhes** para esta detecção de risco. Na lâmina **Detalhes,** você tem a opção de fechar manualmente uma detecção de risco ou reativar uma detecção de risco fechada manualmente. 

![Entradas de risco](./media/concept-risky-sign-ins/325.png)

## <a name="next-steps"></a>Próximas etapas

- [Como configurar a política de risco do usuário](../identity-protection/howto-user-risk-policy.md)
- [Como configurar a política de correção de risco](../identity-protection/howto-user-risk-policy.md)
- [Tipos de detecção de risco](concept-risk-events.md)

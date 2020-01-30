---
title: Configurar a entrada para uma organização do Azure AD
titleSuffix: Azure AD B2C
description: Configurar assinatura para uma organização do Active Directory do Azure específica no Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/08/2019
ms.author: marsma
ms.subservice: B2C
ms.custom: fasttrack-edit
ms.openlocfilehash: b0ff1c2d913d0a4402b491f3c84ce0d35cd081df
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/29/2020
ms.locfileid: "76847583"
---
# <a name="set-up-sign-in-for-a-specific-azure-active-directory-organization-in-azure-active-directory-b2c"></a>Configurar assinatura para uma organização do Active Directory do Azure específica no Azure Active Directory B2C

Para usar um Azure Active Directory (Azure AD) como um [provedor de identidade](authorization-code-flow.md) no Azure AD B2C, é preciso criar um aplicativo que o represente. Este artigo mostra como habilitar a entrada de usuários a partir de uma organização específica do Azure AD usando um fluxo de usuário no Azure AD B2C.

## <a name="create-an-azure-ad-app"></a>Criar um aplicativo Azure AD

Para habilitar a entrada para usuários de uma organização específica do Azure AD, você precisa registrar um aplicativo no locatário organizacional do Azure AD, que não é o mesmo que o seu locatário do Azure Active Directory B2C.

1. Entre no [portal do Azure](https://portal.azure.com).
2. Verifique se você está usando o diretório que contém seu locatário do Azure AD. Selecione o **diretório +** filtro de assinatura no menu superior e escolha o diretório que contém seu locatário do Azure AD. Este não é o mesmo locatário que seu locatário de Azure AD B2C.
3. Escolha **Todos os serviços** no canto superior esquerdo do portal do Azure e pesquise e selecione **Registros de aplicativo**.
4. Selecione **Novo registro**.
5. Insira um nome para seu aplicativo. Por exemplo, `Azure AD B2C App`.
6. Aceite a seleção de **contas neste diretório organizacional somente** para este aplicativo.
7. Para o **URI de redirecionamento**, aceite o valor de **Web**e insira a URL a seguir em todas as letras minúsculas, em que `your-B2C-tenant-name` é substituído pelo nome do seu locatário Azure ad B2C. Por exemplo `https://fabrikam.b2clogin.com/fabrikam.onmicrosoft.com/oauth2/authresp`:

    ```
    https://your-B2C-tenant-name.b2clogin.com/your-B2C-tenant-name.onmicrosoft.com/oauth2/authresp
    ```

    Agora todas as URLs devem estar usando [b2clogin.com](b2clogin.md).

8. Clique em **Registrar**. Copie a **ID do aplicativo (cliente)** a ser usada posteriormente.
9. Selecione **certificados & segredos** no menu do aplicativo e, em seguida, selecione **novo segredo do cliente**.
10. Insira um nome para o segredo do cliente. Por exemplo, `Azure AD B2C App Secret`.
11. Selecione o período de validade. Para este aplicativo, aceite a seleção de **em 1 ano**.
12. Selecione **Adicionar** e copie o valor do novo segredo do cliente que será exibido para ser usado posteriormente.

## <a name="configure-azure-ad-as-an-identity-provider"></a>Configurar o Azure AD como um provedor de identidade

1. Verifique se você está usando o diretório que contém Azure AD B2C locatário. Selecione o **diretório +** filtro de assinatura no menu superior e escolha o diretório que contém seu locatário de Azure ad B2C.
1. Escolha **Todos os serviços** no canto superior esquerdo do Portal do Azure, pesquise **Azure AD B2C** e selecione-o.
1. Selecione **provedores de identidade**e, em seguida, selecione **novo provedor do OpenID Connect**.
1. Insira um **Nome**. Por exemplo, insira *Contoso Azure AD*.
1. Para **URL de metadados**, insira a URL a seguir substituindo `your-AD-tenant-domain` pelo nome de domínio do seu locatário do Azure AD:

    ```
    https://login.microsoftonline.com/your-AD-tenant-domain/.well-known/openid-configuration
    ```

    Por exemplo, `https://login.microsoftonline.com/contoso.onmicrosoft.com/.well-known/openid-configuration`.

    **Não use o** ponto de extremidade de metadados do Azure ad v 2.0, por exemplo `https://login.microsoftonline.com/contoso.onmicrosoft.com/v2.0/.well-known/openid-configuration`. Fazer isso resulta em um erro semelhante a `AADB2C: A claim with id 'UserId' was not found, which is required by ClaimsTransformation 'CreateAlternativeSecurityId' with id 'CreateAlternativeSecurityId' in policy 'B2C_1_SignUpOrIn' of tenant 'contoso.onmicrosoft.com'` ao tentar entrar.

1. Para **ID do cliente**, insira a ID do aplicativo que você registrou anteriormente.
1. Para **segredo do cliente**, insira o segredo do cliente que você registrou anteriormente.
1. Deixe os valores padrão para **escopo**, **tipo de resposta**e **modo de resposta**.
1. Adicional Insira um valor para **Domain_hint**. Por exemplo, *ContosoAD*. Esse é o valor a ser usado ao fazer referência a esse provedor de identidade usando *domain_hint* na solicitação.
1. Em **mapeamento de declarações do provedor de identidade**, insira os seguintes valores de mapeamento de declarações:

    * **ID de usuário**: *OID*
    * **Nome de exibição**: *nome*
    * **Nome fornecido**: *given_name*
    * **Sobrenome**: *family_name*
    * **Email**: *unique_name*

1. Clique em **Salvar**.

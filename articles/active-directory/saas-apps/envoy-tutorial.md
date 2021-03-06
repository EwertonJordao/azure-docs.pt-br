---
title: 'Tutorial: Integração do SSO (logon único) do Azure Active Directory ao Envoy | Microsoft Docs'
description: Saiba como configurar o logon único entre o Azure Active Directory e o Envoy.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 08/29/2019
ms.author: jeedes
ms.openlocfilehash: 1925ef48ee08a7b4f3b3b4c1b1319463d4134f6c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/09/2020
ms.locfileid: "88551543"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-envoy"></a>Tutorial: Integração do SSO (logon único) do Azure Active Directory ao Envoy

Neste tutorial, você aprenderá a integrar o Envoy ao Azure AD (Azure Active Directory). Quando integrar o Envoy ao Azure AD, você poderá:

* Controlar no Azure AD quem tem acesso ao Envoy.
* Permitir que os usuários sejam conectados automaticamente ao Envoy com suas contas do Azure AD.
* Gerenciar suas contas em um local central: o portal do Azure.

Para saber mais sobre a integração de aplicativos SaaS ao Azure AD, confira [O que é o acesso de aplicativos e o logon único com o Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Pré-requisitos

Para começar, você precisará dos seguintes itens:

* Uma assinatura do Azure AD. Caso você não tenha uma assinatura, obtenha uma [conta gratuita](https://azure.microsoft.com/free/).
* Assinatura habilitada para SSO (logon único) do Envoy.

## <a name="scenario-description"></a>Descrição do cenário

Neste tutorial, você configurará e testará o SSO do Azure AD em um ambiente de teste.

* O Envoy dá suporte ao SSO iniciado por **SP**

* O Envoy dá suporte ao provisionamento de usuário **Just-In-Time**

> [!NOTE]
> O identificador desse aplicativo é um valor de cadeia de caracteres fixo; portanto apenas uma instância pode ser configurada em um locatário.

## <a name="adding-envoy-from-the-gallery"></a>Adicionar o Envoy por meio da galeria

Para configurar a integração do Envoy com o Azure AD, você precisará adicionar o Envoy à sua lista de aplicativos SaaS gerenciados por meio da galeria.

1. Entre no [portal do Azure](https://portal.azure.com) usando uma conta corporativa ou de estudante ou uma conta pessoal da Microsoft.
1. No painel de navegação esquerdo, escolha o serviço **Azure Active Directory**.
1. Navegue até **Aplicativos Empresariais** e, em seguida, escolha **Todos os Aplicativos**.
1. Para adicionar um novo aplicativo, escolha **Novo aplicativo**.
1. Na seção **Adicionar da galeria**, digite **Envoy** na caixa de pesquisa.
1. Selecione **Envoy** no painel de resultados e adicione o aplicativo. Aguarde alguns segundos enquanto o aplicativo é adicionado ao seu locatário.

## <a name="configure-and-test-azure-ad-single-sign-on-for-envoy"></a>Configurar e testar logon único do Azure AD para o Envoy

Configure e teste o SSO do Azure AD com o Envoy usando um usuário de teste chamado **B.Fernandes**. Para que o SSO funcione, é necessário estabelecer uma relação de vínculo entre um usuário do Azure AD e o usuário relacionado do Envoy.

Para configurar e testar o SSO do Azure AD com o Envoy, conclua os seguintes blocos de construção:

1. **[Configurar o SSO do Azure AD](#configure-azure-ad-sso)** – para permitir que os usuários usem esse recurso.
    1. **[Criar um usuário de teste do Azure AD](#create-an-azure-ad-test-user)** para testar o logon único do Azure AD com B.Fernandes.
    1. **[Atribuir o usuário de teste do Azure AD](#assign-the-azure-ad-test-user)** – para permitir que B.Fernandes use o logon único do Azure AD.
1. **[Configurar o SSO do Envoy](#configure-envoy-sso)** – para definir as configurações de logon único no lado do aplicativo.
    1. **[Criar um usuário de teste do Envoy](#create-envoy-test-user)** – para ter um equivalente de B.Fernandes no Envoy que esteja vinculado à representação de usuário do Azure AD.
1. **[Testar o SSO](#test-sso)** – para verificar se a configuração funciona.

## <a name="configure-azure-ad-sso"></a>Configurar o SSO do Azure AD

Siga estas etapas para habilitar o SSO do Azure AD no portal do Azure.

1. No [portal do Azure](https://portal.azure.com/), na página de integração de aplicativos do **Envoy**, localize a seção **Gerenciar** e selecione **Logon único**.
1. Na página **Selecionar um método de logon único**, escolha **SAML**.
1. Na página **Configurar o logon único com o SAML**, clique no ícone de edição/caneta da **Configuração Básica do SAML** para editar as configurações.

   ![Editar a Configuração Básica de SAML](common/edit-urls.png)

1. Na seção **Configuração Básica do SAML**, insira os valores para os seguintes campos:

    Na caixa de texto **URL de logon**, digite um URL usando o seguinte padrão: `https://app.envoy.com/a/saml/auth/<company-ID-from-Envoy>`

    > [!NOTE]
    > O valor não é real. Atualize o valor com a URL de Logon real. Contate a [equipe de suporte ao cliente do Envoy](https://envoy.com/contact/) para obter o valor. Você também pode consultar os padrões exibidos na seção **Configuração Básica de SAML** no portal do Azure.

1. Na seção **Certificado de Autenticação SAML**, clique no botão **Editar** para abrir a caixa de diálogo **Certificado de Autenticação SAML**.

    ![Editar o Certificado de Autenticação SAML](common/edit-certificate.png)

1. Na seção **Certificado de Autenticação SAML**, copie o **Valor da Impressão Digital** e salve-o em seu computador.

    ![Copiar o valor da Impressão Digital](common/copy-thumbprint.png)

1. Na seção **Configurar o Envoy**, copie as URLs apropriadas de acordo com suas necessidades.

    ![Copiar URLs de configuração](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Criar um usuário de teste do Azure AD

Nesta seção, você criará um usuário de teste no portal do Azure chamado B.Fernandes.

1. No painel esquerdo do portal do Azure, escolha **Azure Active Directory**, **Usuários** e, em seguida, **Todos os usuários**.
1. Selecione **Novo usuário** na parte superior da tela.
1. Nas propriedades do **Usuário**, siga estas etapas:
   1. No campo **Nome**, insira `B.Simon`.  
   1. No campo **Nome de usuário**, insira username@companydomain.extension. Por exemplo, `B.Simon@contoso.com`.
   1. Marque a caixa de seleção **Mostrar senha** e, em seguida, anote o valor exibido na caixa **Senha**.
   1. Clique em **Criar**.

### <a name="assign-the-azure-ad-test-user"></a>Atribuir o usuário de teste do Azure AD

Nesta seção, você permitirá que B.Fernandes use o logon único do Azure concedendo a ela acesso ao Envoy.

1. No portal do Azure, selecione **Aplicativos empresariais** e, em seguida, selecione **Todos os aplicativos**.
1. Na lista de aplicativos, escolha **Envoy**.
1. Na página de visão geral do aplicativo, localize a seção **Gerenciar** e escolha **Usuários e grupos**.

   ![O link “Usuários e grupos”](common/users-groups-blade.png)

1. Escolha **Adicionar usuário** e, em seguida, **Usuários e grupos** na caixa de diálogo **Adicionar Atribuição**.

    ![O link Adicionar Usuário](common/add-assign-user.png)

1. Na caixa de diálogo **Usuários e grupos**, selecione **B.Fernandes** na lista Usuários e clique no botão **Selecionar** na parte inferior da tela.
1. Se você estiver esperando um valor de função na declaração SAML, na caixa de diálogo **Selecionar Função**, escolha a função apropriada para o usuário da lista e, em seguida, clique no botão **Escolher** na parte inferior da tela.
1. Na caixa de diálogo **Adicionar atribuição**, clique no botão **Atribuir**.

## <a name="configure-envoy-sso"></a>Configurar o SSO do Envoy

1. Para automatizar a configuração no Envoy, é necessário instalar a **Extensão do navegador de Entrada Segura dos Meus Aplicativos**, clicando em **Instalar a extensão**.

    ![Extensão Meus Aplicativos](common/install-myappssecure-extension.png)

2. Depois de adicionar a extensão ao navegador, clique em **Instalação do Envoy**. Você será direcionado ao aplicativo Envoy. De lá, forneça as credenciais de administrador para entrar no Envoy. A extensão do navegador configurará automaticamente o aplicativo e automatizará as etapas de 3 a 7.

    ![Configuração da instalação](common/setup-sso.png)

3. Se desejar configurar o Envoy manualmente, abra uma nova janela do navegador da Web, entre no site da empresa Envoy como administrador e execute as seguintes etapas:

4. Na barra de ferramentas na parte superior, clique em **Configurações**.

    ![Envoy](./media/envoy-tutorial/ic776782.png "Envoy")

5. Clique em **Empresa**.

    ![Empresa](./media/envoy-tutorial/ic776783.png "Empresa")

6. Clique em **SAML**.

    ![SAML](./media/envoy-tutorial/ic776784.png "SAML")

7. Na seção de configuração da **Autenticação SAML** , realize as seguintes etapas:

    ![Autenticação SAML](./media/envoy-tutorial/ic776785.png "Autenticação de SAML")
    
    >[!NOTE]
    >O valor para a ID de Local de Matriz é automaticamente gerada pelo aplicativo.
    
    a. Na caixa de texto **Impressão digital**, cole o valor da **Impressão Digital** do certificado copiado do Portal do Azure.
    
    b. Cole o valor da **URL do Logon** copiado do portal do Azure na caixa de texto **URL HTTP DO SAML DO PROVEDOR DE IDENTIDADE**.
    
    c. Clique em **Salvar alterações**.

### <a name="create-envoy-test-user"></a>Criar um usuário de teste do Envoy

Nesta seção, é criado um usuário chamado Brenda Fernandes no Envoy. O Envoy dá suporte ao o provisionamento de usuário Just-In-Time, que está habilitado por padrão. Não há itens de ação para você nesta seção. Se um usuário ainda não existir no Envoy, um novo será criado após a autenticação.

## <a name="test-sso"></a>Testar o SSO 

Nesta seção, você testará sua configuração de logon único do Azure AD usando o Painel de Acesso.

Ao clicar no bloco do Envoy no Painel de Acesso, você deverá ser conectado automaticamente ao Envoy, para o qual você configurou o SSO. Para saber mais sobre o Painel de Acesso, veja [Introdução ao Painel de Acesso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionais

- [ Lista de tutoriais sobre como integrar aplicativos SaaS com o Active Directory do Azure ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [O que é o acesso a aplicativos e logon único com o Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [O que é o acesso condicional no Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Experimente o Envoy com o Azure AD](https://aad.portal.azure.com/)


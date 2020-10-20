---
title: 'Tutorial: Integração do SSO (logon único) do Azure Active Directory ao Paylocity | Microsoft Docs'
description: Saiba como configurar o logon único entre o Azure Active Directory e o Paylocity.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 01/21/2020
ms.author: jeedes
ms.openlocfilehash: 2e772dc39374b178743a6c8b93e6b48e62b1e2ed
ms.sourcegitcommit: 83610f637914f09d2a87b98ae7a6ae92122a02f1
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/13/2020
ms.locfileid: "91995956"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-paylocity"></a>Tutorial: Integração do SSO (logon único) do Azure Active Directory com o Paylocity

Neste tutorial, você aprenderá a integrar o Paylocity ao Azure AD (Azure Active Directory). Com a integração do Paylocity ao Azure AD, você poderá:

* Controlar no Azure AD quem tem acesso ao Paylocity.
* Permitir que os usuários sejam conectados automaticamente ao Paylocity com suas contas do Azure AD.
* Gerenciar suas contas em um local central: o portal do Azure.

Para saber mais sobre a integração de aplicativos SaaS ao Azure AD, confira [O que é o acesso de aplicativos e o logon único com o Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Pré-requisitos

Para começar, você precisará dos seguintes itens:

* Uma assinatura do Azure AD. Caso você não tenha uma assinatura, obtenha uma [conta gratuita](https://azure.microsoft.com/free/).
* Uma assinatura habilitada para SSO (logon único) do Paylocity.

## <a name="scenario-description"></a>Descrição do cenário

Neste tutorial, você configurará e testará o SSO do Azure AD em um ambiente de teste.

* O Paylocity dá suporte ao SSO iniciado por **SP e IDP**

* Após configurar o Paylocity, você poderá impor controles de sessão, que protegem contra a exportação e infiltração de dados confidenciais de sua organização em tempo real. Os controles da sessão são estendidos do Acesso Condicional. [Saiba como impor o controle de sessão com o Microsoft Cloud App Security](https://docs.microsoft.com/cloud-app-security/proxy-deployment-aad).

## <a name="adding-paylocity-from-the-gallery"></a>Como adicionar o Paylocity da galeria

Para configurar a integração do Paylocity ao Azure AD, você precisa adicionar o Paylocity da galeria à sua lista de aplicativos SaaS gerenciados.

1. Entre no [portal do Azure](https://portal.azure.com) usando uma conta corporativa ou de estudante ou uma conta pessoal da Microsoft.
1. No painel de navegação esquerdo, escolha o serviço **Azure Active Directory**.
1. Navegue até **Aplicativos Empresariais** e, em seguida, escolha **Todos os Aplicativos**.
1. Para adicionar um novo aplicativo, escolha **Novo aplicativo**.
1. Na seção **Adicionar por meio da galeria**, digite **Paylocity** na caixa de pesquisa.
1. Selecione **Paylocity** no painel de resultados e adicione o aplicativo. Aguarde alguns segundos enquanto o aplicativo é adicionado ao seu locatário.

## <a name="configure-and-test-azure-ad-single-sign-on-for-paylocity"></a>Configurar e testar logon único do Azure AD para o Paylocity

Configure e teste o SSO do Azure AD com o Paylocity usando um usuário de teste chamado **B.Fernandes**. Para que o SSO funcione, é necessário estabelecer uma relação de vínculo entre um usuário do Azure AD e o usuário relacionado do Paylocity.

Para configurar e testar o SSO do Azure AD com o Paylocity, conclua os seguintes blocos de construção:

1. **[Configurar o SSO do Azure AD](#configure-azure-ad-sso)** – para permitir que os usuários usem esse recurso.
    * **[Criar um usuário de teste do Azure AD](#create-an-azure-ad-test-user)** para testar o logon único do Azure AD com B.Fernandes.
    * **[Atribuir o usuário de teste do Azure AD](#assign-the-azure-ad-test-user)** – para permitir que B.Fernandes use o logon único do Azure AD.
1. **[Configurar o SSO do Paylocity](#configure-paylocity-sso)** – para definir as configurações de logon único no lado do aplicativo.
    * **[Criar usuário de teste do Paylocity](#create-paylocity-test-user)** – para ter um equivalente de B.Fernandes no Paylocity que esteja vinculado à representação do usuário no Azure AD.
1. **[Testar o SSO](#test-sso)** – para verificar se a configuração funciona.

## <a name="configure-azure-ad-sso"></a>Configurar o SSO do Azure AD

Siga estas etapas para habilitar o SSO do Azure AD no portal do Azure.

1. No [portal do Azure](https://portal.azure.com/), na página de integração de aplicativos do **Paylocity**, localize a seção **Gerenciar** e escolha **Logon único**.
1. Na página **Selecionar um método de logon único**, escolha **SAML**.
1. Na página **Configurar o logon único com o SAML**, clique no ícone de edição/caneta da **Configuração Básica do SAML** para editar as configurações.

   ![Editar a Configuração Básica de SAML](common/edit-urls.png)

1. Na seção **Configuração Básica do SAML**, o usuário não precisa executar nenhuma etapa, pois o aplicativo já está pré-integrado ao Azure.

1. Clique em **Definir URLs adicionais** e execute o passo seguinte se quiser configurar a aplicação no modo **SP** iniciado:

    Na caixa de texto **URL de Logon**, digite uma URL: `https://access.paylocity.com/`

1. Clique em **Save** (Salvar).

1. Seu aplicativo Paylocity espera as declarações do SAML em um formato específico, o que exige que você adicione mapeamentos de atributo personalizado de acordo com a sua configuração de atributos do token SAML. A captura de tela a seguir mostra a lista de atributos padrão.

    ![image](common/default-attributes.png)

1. Além do indicado acima, o aplicativo Paylocity espera que mais alguns atributos sejam passados novamente na resposta SAML, que são mostrados abaixo. Esses atributos também são preenchidos previamente, mas você precisa atualizá-los com os valores reais.

    | Nome |  Atributo de Origem|
    | ---------------| --------------- |
    | PartnerID | `P8000010` |
    | PaylocityUser | `user.mail`|
    | PaylocityEntity | < `PaylocityEntity` > |

    > [!NOTE]
    > O PaylocityEntity é a ID da Empresa do Paylocity.

1. Na página **Configurar o logon único com o SAML**, na seção **Certificado de Autenticação SAML**, localize **XML de Metadados de Federação** e selecione **Baixar** para baixar o certificado e salvá-lo no computador.

    ![O link de download do Certificado](common/metadataxml.png)

1. Na página **Configurar logon único com SAML**, na seção **Certificado de Autenticação SAML**, clique em **Editar Ícone**.

    ![Uma captura de tela que mostra o "Certificado de Autenticação SAML" com a ação "Baixar" para "XML de Metadados de Federação" selecionada.](./media/paylocity-tutorial/edit-samlassertion.png)

1. Selecione **Opção de Assinatura** como **Assinar resposta SAML e declaração** e clique em **Salvar**.

    ![A Edição do Certificado de Autenticação SAML](./media/paylocity-tutorial/saml-assertion.png)

1. Na seção **Configurar o Paylocity**, copie as URLs apropriadas com base nas suas necessidades.

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

Nesta seção, você permitirá que B.Fernandes use o logon único do Azure permitindo a ela acesso ao Paylocity.

1. No portal do Azure, selecione **Aplicativos empresariais** e, em seguida, selecione **Todos os aplicativos**.
1. Na lista de aplicativos, escolha **Paylocity**.
1. Na página de visão geral do aplicativo, localize a seção **Gerenciar** e escolha **Usuários e grupos**.

   ![O link “Usuários e grupos”](common/users-groups-blade.png)

1. Escolha **Adicionar usuário** e, em seguida, **Usuários e grupos** na caixa de diálogo **Adicionar Atribuição**.

    ![O link Adicionar Usuário](common/add-assign-user.png)

1. Na caixa de diálogo **Usuários e grupos**, selecione **B.Fernandes** na lista Usuários e clique no botão **Selecionar** na parte inferior da tela.
1. Se você estiver esperando um valor de função na declaração SAML, na caixa de diálogo **Selecionar Função**, escolha a função apropriada para o usuário da lista e, em seguida, clique no botão **Escolher** na parte inferior da tela.
1. Na caixa de diálogo **Adicionar atribuição**, clique no botão **Atribuir**.

## <a name="configure-paylocity-sso"></a>Configurar o SSO do Paylocity

Para configurar o logon único no lado do **Paylocity**, é necessário enviar o **XML de Metadados de Federação** baixado e as URLs apropriadas copiadas do portal do Azure para a [equipe de suporte do Paylocity](mailto:service@paylocity.com). Eles definem essa configuração para ter a conexão de SSO de SAML definida corretamente em ambos os lados.

### <a name="create-paylocity-test-user"></a>Criar usuário de teste do Paylocity

Nesta seção, você criará um usuário chamado B.Fernandes no Paylocity. Trabalhe com a  [equipe de suporte do Paylocity](mailto:service@paylocity.com) para adicionar os usuários na plataforma do Paylocity. Os usuários devem ser criados e ativados antes de usar o logon único.

## <a name="test-sso"></a>Testar o SSO

Nesta seção, você testará sua configuração de logon único do Azure AD usando o Painel de Acesso.

Ao clicar no bloco do Paylocity no Painel de Acesso, você deverá ser conectado automaticamente ao Paylocity, para o qual você configurou o SSO. Para saber mais sobre o Painel de Acesso, veja [Introdução ao Painel de Acesso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionais

- [ Lista de tutoriais sobre como integrar aplicativos SaaS com o Active Directory do Azure ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [O que é o acesso a aplicativos e logon único com o Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [O que é o acesso condicional no Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Experimente o Paylocity com o Azure AD](https://aad.portal.azure.com/)

* [O que é controle de sessão no Microsoft Cloud App Security?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

* [Como proteger o Paylocity com visibilidade e controles avançados](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

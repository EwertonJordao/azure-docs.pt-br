---
title: 'Tutorial: Habilitar a autenticação em um aplicativo de página única'
titleSuffix: Azure AD B2C
description: Neste tutorial, saiba como usar o Azure Active Directory B2C para fornecer o logon do usuário para um SPA (aplicativo de página única) baseado no JavaScript.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.author: marsma
ms.date: 10/14/2019
ms.custom: mvc, seo-javascript-september2019
ms.topic: tutorial
ms.service: active-directory
ms.subservice: B2C
ms.openlocfilehash: f66d8e229535346525f117d8ebbfb37b893fe022
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/29/2020
ms.locfileid: "76849806"
---
# <a name="tutorial-enable-authentication-in-a-single-page-application-using-azure-active-directory-b2c-azure-ad-b2c"></a>Tutorial: Habilitar autenticação em um aplicativo de página única usando o Azure AD B2C (Azure Active Directory B2C)

Este tutorial mostra como usar o Azure AD B2C (Azure Active Directory B2C) para inscrever e conectar usuários em um SPA (aplicativo de página única). O Azure AD B2C permite que seus aplicativos se autentiquem com contas sociais, corporativas e do Azure Active Directory usando protocolos padrão abertos.

Neste tutorial, você aprenderá como:

> [!div class="checklist"]
> * Atualizar o aplicativo no Azure AD B2C
> * Configurar o exemplo para usar o aplicativo
> * Inscrever-se usando o fluxo de usuário

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Prerequisites

Você precisará dos seguintes recursos do Azure AD B2C em vigor antes de continuar com as etapas deste tutorial:

* [Locatário do Azure AD B2C](tutorial-create-tenant.md)
* [Aplicativo registrado](tutorial-register-applications.md) em seu locatário
* [Fluxos dos usuários criados](tutorial-create-user-flows.md) em seu locatário

Além disso, você precisará do seguinte no ambiente de desenvolvimento local:

* Editor de códigos, por exemplo, [Visual Studio Code](https://code.visualstudio.com/) ou [Visual Studio 2019](https://www.visualstudio.com/downloads/)
* [SDK do .NET Core 2.2](https://dotnet.microsoft.com/download) ou posterior
* [Node.js](https://nodejs.org/en/download/)

## <a name="update-the-application"></a>Atualizar o aplicativo

No segundo tutorial concluído como parte dos pré-requisitos, você registrou um aplicativo Web no Azure AD B2C. Para habilitar a comunicação com o exemplo no tutorial, é necessário adicionar um URI de redirecionamento ao aplicativo no Azure AD B2C.

Você pode usar a experiência **Aplicativos** atual ou nossa nova experiência **Registros de aplicativo (versão prévia)** unificada para atualizar o aplicativo. [Saiba mais sobre a nova experiência](https://aka.ms/b2cappregintro).

#### <a name="applicationstabapplications"></a>[Aplicativos](#tab/applications/)

1. Entre no [portal do Azure](https://portal.azure.com).
1. Verifique se você está usando o diretório que contém o locatário do Azure AD B2C selecionando o filtro **Diretório + assinatura** no menu superior e escolhendo o diretório que contém o locatário.
1. Selecione **Todos os serviços** no canto superior esquerdo do portal do Azure, pesquise pelo **Azure AD B2C** e selecione-o.
1. Selecione **Aplicativos** e, em seguida, selecione o aplicativo *webapp1*.
1. Em **URL de resposta**, adicione `http://localhost:6420`.
1. Clique em **Salvar**.
1. Na página de propriedades, registre a **ID do Aplicativo**. Você pode usar a ID do aplicativo em uma etapa posterior ao atualizar o código no aplicativo Web de página única.

#### <a name="app-registrations-previewtabapp-reg-preview"></a>[Registros de Aplicativo (versão prévia)](#tab/app-reg-preview/)

1. Entre no [portal do Azure](https://portal.azure.com).
1. Selecione o filtro **Diretório + assinatura** no menu superior e, em seguida, selecione o diretório que contém o locatário do Azure AD B2C.
1. No menu à esquerda, selecione **Azure AD B2C**. Ou selecione **Todos os serviços** e pesquise e selecione **Azure AD B2C**.
1. Selecione **Registros de aplicativo (versão prévia)** , selecione a guia **Aplicativos Próprios** e, em seguida, selecione o aplicativo *webapp1*.
1. Selecione **Autenticação** e, em seguida, selecione **Experimente agora a nova experiência** (se mostrado).
1. Em **Web**, selecione o link **Adicionar URI**, digite `http://localhost:6420` e, em seguida, selecione **Salvar**.
1. Selecione **Visão geral**.
1. Anote a **ID do aplicativo (cliente)** para uso em uma etapa posterior, na qual você atualize o código no aplicativo Web de página única.

* * *

## <a name="get-the-sample-code"></a>Obter o código de amostra

Neste tutorial, você configurará um exemplo de código baixado do GitHub. A amostra descreve como um aplicativo de página única pode usar o Azure AD B2C para a inscrição e a entrada de usuário e para chamar uma API Web protegida.

[Baixe um arquivo zip](https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp/archive/master.zip) ou clone o exemplo do GitHub.

```
git clone https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp.git
```

## <a name="update-the-sample"></a>Atualizar a amostra

Agora que você já obteve a amostra, atualize o código com o nome do locatário do Azure AD B2C e a ID do aplicativo registrada em uma etapa anterior.

1. Abra o arquivo `index.html` na raiz do diretório de exemplo.
1. Na definição `msalConfig`, modifique o valor de **clientId** com a ID do aplicativo registrada em uma etapa anterior. Em seguida, atualize o valor de URI **authority** com o nome do locatário do Azure AD B2C. Atualize também o URI com o nome do fluxo de usuário de inscrição/entrada criado em um dos pré-requisitos (por exemplo, *B2C_1_signupsignin1*).

    ```javascript
    var msalConfig = {
        auth: {
            clientId: "00000000-0000-0000-0000-000000000000", //This is your client ID
            authority: "https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/b2c_1_susi", //This is your tenant info
            validateAuthority: false
        },
        cache: {
            cacheLocation: "localStorage",
            storeAuthStateInCookie: true
        }
    };
    ```

    O nome do fluxo de usuário usado neste tutorial é **B2C_1_signupsignin1**. Se estiver usando outro nome de fluxo, especifique esse nome no valor `authority`.

## <a name="run-the-sample"></a>Execute o exemplo

1. Abra uma janela do console e altere para o diretório que contém a amostra. Por exemplo:

    ```console
    cd active-directory-b2c-javascript-msal-singlepageapp
    ```
1. Execute os seguintes comandos:

    ```
    npm install && npm update
    node server.js
    ```

    A janela do console exibe o número da porta do servidor do Node.js em execução localmente:

    ```
    Listening on port 6420...
    ```

1. Acesse `http://localhost:6420` no navegador para exibir o aplicativo.

O exemplo dá suporte à inscrição, conexão, edição de perfil e redefinição de senha. Este tutorial destaca como um usuário se inscreve usando um endereço de email.

### <a name="sign-up-using-an-email-address"></a>Criar conta usando um endereço de email

> [!WARNING]
> Depois de inscrever-se ou entrar, você poderá ver um [erro de permissões insuficientes](#error-insufficient-permissions). Devido à implementação atual do exemplo de código, esse erro é esperado. Esse problema será resolvido em uma versão futura do exemplo de código, momento em que esse aviso será removido.

1. Selecione **Logon** para iniciar o fluxo de usuário *B2C_1_signupsignin1* especificado em uma etapa anterior.
1. O Azure AD B2C apresenta uma página de entrada com um link de inscrição. Como você ainda não tem uma conta, selecione o link **Inscrever-se agora**.
1. O fluxo de trabalho de inscrição apresenta uma página para coletar e verificar a identidade do usuário usando um endereço de email. O fluxo de trabalho de inscrição também coleta a senha do usuário e os atributos solicitados definidos no fluxo de usuário.

    Use um endereço de email válido e valide-o usando o código de verificação. Defina uma senha. Insira valores para os atributos necessários.

    ![Página de inscrição apresentada pelo fluxo de usuário para entrada/inscrição](./media/tutorial-single-page-app/azure-ad-b2c-sign-up-workflow.png)

1. Selecione **Criar** para criar uma conta local no diretório do Azure AD B2C.

Quando você seleciona **Criar**, a página de inscrição é fechada e a página de entrada é exibida novamente.

Agora você pode usar seu endereço de email e sua senha para entrar no aplicativo.

### <a name="error-insufficient-permissions"></a>Erro: permissões insuficientes

Depois que você entrar, o aplicativo poderá retornar um erro de permissões insuficientes:

```Output
ServerError: AADB2C90205: This application does not have sufficient permissions against this web resource to perform the operation.
Correlation ID: ce15bbcc-0000-0000-0000-494a52e95cd7
Timestamp: 2019-07-20 22:17:27Z
```

Você recebe esse erro porque o aplicativo Web está tentando acessar uma API Web protegida pelo diretório de demonstração, *fabrikamb2c*. Como seu token de acesso só é válido para o diretório do Azure AD, a chamada à API não é autorizada.

Para corrigir esse erro, prossiga para o próximo tutorial da série (confira as [Próximas etapas](#next-steps)) a fim de criar uma API Web protegida para o diretório.

## <a name="next-steps"></a>Próximas etapas

Neste artigo, você aprendeu a:

> [!div class="checklist"]
> * Atualizar o aplicativo no Azure AD B2C
> * Configurar o exemplo para usar o aplicativo
> * Inscrever-se usando o fluxo de usuário

Agora passe para o próximo tutorial da série para permitir acesso a uma API Web protegida por meio do SPA:

> [!div class="nextstepaction"]
> [Tutorial: Permitir acesso a uma API Web do ASP.NET Core de um SPA usando o Azure AD B2C >](tutorial-single-page-app-webapi.md)

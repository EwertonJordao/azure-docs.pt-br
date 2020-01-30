---
title: 'Tutorial: Permitir acesso a uma API Web do ASP.NET Core de um aplicativo de página única'
titleSuffix: Azure AD B2C
description: Neste tutorial, saiba como usar o Active Directory B2C para proteger uma API Web do .NET Core e chamar a API de um aplicativo de página única do Node.js.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.author: marsma
ms.date: 07/24/2019
ms.custom: mvc
ms.topic: tutorial
ms.service: active-directory
ms.subservice: B2C
ms.openlocfilehash: 80b2165b0ec652358a3eb8ac9d55b64f525e4690
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/29/2020
ms.locfileid: "76849836"
---
# <a name="tutorial-grant-access-to-an-aspnet-core-web-api-from-a-single-page-application-using-azure-active-directory-b2c"></a>Tutorial: Permitir acesso a uma API Web ASP.NET Core de um aplicativo de página única usando o Azure Active Directory B2C

Este tutorial mostra como chamar um recurso da API Web do ASP.NET Core protegido pelo Azure AD B2C (Azure Active Directory B2C) em um aplicativo de página única.

Neste tutorial, você aprenderá como:

> [!div class="checklist"]
> * Adicionar um aplicativo API Web
> * Configurar escopos para uma API Web
> * Conceder permissões à API Web
> * Configurar o exemplo para usar o aplicativo

## <a name="prerequisites"></a>Prerequisites

* Conclua as etapas e os pré-requisitos no [Tutorial: habilitar autenticação em um aplicativo de página única usando o Azure Active Directory B2C](tutorial-single-page-app.md).
* Visual Studio 2019 (ou posterior) ou Visual Studio Code
* .NET Core 2.2 ou posterior
* Node.js

## <a name="add-a-web-api-application"></a>Adicionar um aplicativo API Web

[!INCLUDE [active-directory-b2c-appreg-webapi](../../includes/active-directory-b2c-appreg-webapi.md)]

## <a name="configure-scopes"></a>Configurar escopos

Os escopos fornecem uma maneira de controlar o acesso a recursos protegidos. Escopos são usados pela API Web para implementar o controle de acesso com base em escopo. Por exemplo, alguns usuários podem ter acesso de leitura e gravação, enquanto outros usuários podem ter permissões somente leitura. Neste tutorial, você define as permissões de leitura e gravação para a API Web.

[!INCLUDE [active-directory-b2c-scopes](../../includes/active-directory-b2c-scopes.md)]

Registre o valor em **ESCOPOS** do escopo `demo.read` a ser usado em uma etapa posterior ao configurar o aplicativo de página única. O valor de escopo completo é semelhante a `https://contosob2c.onmicrosoft.com/api/demo.read`.

## <a name="grant-permissions"></a>Conceder permissões

Para chamar uma API Web protegida de outro aplicativo, é necessário conceder a esse aplicativo permissões para a API Web.

No tutorial de pré-requisito, você criou um aplicativo Web chamado *webapp1*. Neste tutorial, você configura esse aplicativo para chamar a API Web criada em uma seção anterior, *webapi1*.

[!INCLUDE [active-directory-b2c-permissions-api](../../includes/active-directory-b2c-permissions-api.md)]

Seu aplicativo Web de página única é registrado para chamar a API Web protegida. Um usuário autentica-se com o Azure AD B2C para usar o aplicativo de página única. O aplicativo de página única obtém uma concessão de autorização do Azure AD B2C para acessar a API Web protegida.

## <a name="configure-the-sample"></a>Configurar o exemplo

Agora que a API Web está registrada e você tem escopos definidos, configure o código da API Web para usar o locatário do Azure AD B2C. Neste tutorial, você configurará um aplicativo Web de amostra do .NET Core baixado do GitHub.

[Baixe um \*arquivo morto .zip](https://github.com/Azure-Samples/active-directory-b2c-dotnetcore-webapi/archive/master.zip) ou clone a amostra de projeto da API Web do GitHub.

```console
git clone https://github.com/Azure-Samples/active-directory-b2c-dotnetcore-webapi.git
```

### <a name="configure-the-web-api"></a>Configurar a API Web

1. Abra o arquivo <em>B2C-WebApi/**appsettings.json**</em> no Visual Studio ou no Visual Studio Code.
1. Modifique o bloco `AzureAdB2C` para refletir o nome do locatário, a ID do aplicativo da API Web, o nome da sua política de inscrição/entrada e os escopos definidos anteriormente. O bloco deve ser semelhante ao exemplo a seguir (com valores `Tenant` e `ClientId` apropriados):

    ```json
    "AzureAdB2C": {
      "Tenant": "<your-tenant-name>.onmicrosoft.com",
      "ClientId": "<webapi-application-ID>",
      "Policy": "B2C_1_signupsignin1",

      "ScopeRead": "demo.read",
      "ScopeWrite": "demo.write"
    },
    ```

#### <a name="enable-cors"></a>Habilitar CORS

Para permitir que seu aplicativo de página única chame a API Web do ASP.NET Core, é necessário habilitar [CORS](https://docs.microsoft.com/aspnet/core/security/cors) na API Web.

1. Em *Startup.cs*, adicione CORS ao método `ConfigureServices()`.

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddCors();
    ```

1. Também dentro do método `ConfigureServices()`, defina o valor `jwtOptions.Authority` para o URI do emissor de token a seguir.

    Substitua `<your-tenant-name>` pelo nome do seu locatário B2C.

    ```csharp
    jwtOptions.Authority = $"https://<your-tenant-name>.b2clogin.com/{Configuration["AzureAdB2C:Tenant"]}/{Configuration["AzureAdB2C:Policy"]}/v2.0";
    ```

1. No método `Configure()`, configure CORS.

    ```csharp
    public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)
    {
        app.UseCors(builder =>
            builder.WithOrigins("http://localhost:6420").AllowAnyHeader().AllowAnyMethod());
    ```

1. (Somente Visual Studio) Em **propriedades**, no Gerenciador de Soluções, abra o arquivo *launchSettings.json* e localize o bloco `iisExpress`.
1. (Somente Visual Studio) Atualize o valor `applicationURL` com o número da porta especificado ao registrar o aplicativo *webapi1* em uma etapa anterior. Por exemplo:

    ```json
    "iisExpress": {
      "applicationUrl": "http://localhost:5000/",
      "sslPort": 0
    }
    ```

### <a name="configure-the-single-page-application"></a>Configure o aplicativo de página única

O SPA (aplicativo de página única) do [tutorial anterior](tutorial-single-page-app.md) da série usa o Azure AD B2C para inscrição e entrada do usuário e chama a API Web do ASP.NET Core protegida pelo locatário de demonstração *frabrikamb2c*.

Nesta seção, você atualiza o aplicativo de página única para chamar a API Web do ASP.NET Core protegida pelo *seu* locatário do Azure AD B2C e executada no computador local.

Para alterar as configurações no SPA:

1. Abra o arquivo *index.html* no projeto [active-directory-b2c-javascript-msal-singlepageapp][github-js-spa] que você baixou ou clonou no tutorial anterior.
1. Configure a amostra com o URI para o escopo *demo.read* criado anteriormente e a URL da API Web.
    1. Na definição `appConfig`, substitua o valor `b2cScopes` pelo URI completo do escopo (o valor do **ESCOPO** que você registrou anteriormente).
    1. Altere o valor de `webApi` para o URI de redirecionamento que você adicionou quando registrou o aplicativo de API Web em uma etapa anterior.

    A definição `appConfig` deve ser semelhante ao bloco de código (com o nome do locatário no lugar de `<your-tenant-name>`) a seguir:

    ```javascript
    // The current application coordinates were pre-registered in a B2C tenant.
    var appConfig = {
      b2cScopes: ["https://<your-tenant-name>.onmicrosoft.com/api/demo.read"],
      webApi: "http://localhost:5000/"
    };
    ```

## <a name="run-the-spa-and-web-api"></a>executar o SPA e a API Web

Por fim, você executa a API Web do ASP.NET Core e o aplicativo de página única do Node.js em seu computador local. Em seguida, você entra no aplicativo de página única e pressiona um botão para iniciar uma solicitação para a API protegida.

Embora ambos os aplicativos sejam executados localmente neste tutorial, eles usam o Azure AD B2C para inscrição/entrada segura e para permitir acesso à API Web protegida.

### <a name="run-the-aspnet-core-web-api"></a>Executar a API Web do ASP.NET Core

No Visual Studio, pressione **F5** para criar e depurar solução *B2C-WebAPI.sln*. Quando o projeto é iniciado, uma página da Web é exibida no navegador padrão anunciando que a API Web está disponível para solicitações.

Se preferir usar a CLI do `dotnet` em vez do Visual Studio:

1. abra uma janela do console e altere para o diretório que contém o arquivo *\*.csproj*. Por exemplo:

    `cd active-directory-b2c-dotnetcore-webapi/B2C-WebApi`

1. criar e executar a API Web executando `dotnet run`.

    Quando a API estiver em funcionamento, você deverá ver uma saída semelhante à seguinte (para o tutorial, você pode ignorar com segurança todos os avisos `NETSDK1059`):

    ```console
    $ dotnet run
    Hosting environment: Production
    Content root path: /home/user/active-directory-b2c-dotnetcore-webapi/B2C-WebApi
    Now listening on: http://localhost:5000
    Application started. Press Ctrl+C to shut down.
    ```

### <a name="run-the-single-page-app"></a>Executar o aplicativo de página única

1. Abra uma janela do console e altere para o diretório que contém a amostra do Node.js. Por exemplo:

    `cd active-directory-b2c-javascript-msal-singlepageapp`

1. Execute os seguintes comandos:

    ```console
    npm install && npm update
    node server.js
    ```

    A janela de console exibe o número da porta em que o aplicativo está hospedado.

    ```console
    Listening on port 6420...
    ```

1. Navegue até `http://localhost:6420` no navegador para exibir o aplicativo.
1. Entre usando o endereço de email e a senha empregados no [tutorial anterior](tutorial-single-page-app.md). Após o logon bem-sucedido, você deverá ver a mensagem `User 'Your Username' logged-in`.
1. Selecione o botão **Chamar API Web**. O SPA obtém uma concessão de autorização do Azure AD B2C e, em seguida, acessa a API Web protegida para exibir o conteúdo de sua página de índice:

    ```Output
    Web APi returned:
    "<html>\r\n<head>\r\n  <title>Azure AD B2C API Sample</title>\r\n ...
    ```

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você aprendeu a:

> [!div class="checklist"]
> * Adicionar um aplicativo API Web
> * Configurar escopos para uma API Web
> * Conceder permissões à API Web
> * Configurar o exemplo para usar o aplicativo

Agora que você já viu um SPA solicitar um recurso de uma API Web protegida, obtenha uma compreensão mais profunda sobre como esses tipos de aplicativos interagem entre si e com o Azure AD B2C.

> [!div class="nextstepaction"]
> [Tipos de aplicativos que podem ser usados no Active Directory B2C >](application-types.md)

<!-- Links - EXTERNAL -->
[github-js-spa]: https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp

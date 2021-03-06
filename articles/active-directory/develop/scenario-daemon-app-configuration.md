---
title: Configurar aplicativos de daemon que chamam APIs da Web-plataforma de identidade da Microsoft | Azure
description: Saiba como configurar o código para seu aplicativo daemon que chama APIs da Web (configuração de aplicativo)
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 09/19/2020
ms.author: jmprieur
ms.custom: aaddev, devx-track-python
ms.openlocfilehash: 8e065651a5527c0ab425614197ce128325454942
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91257666"
---
# <a name="daemon-app-that-calls-web-apis---code-configuration"></a>Aplicativo daemon que chama a configuração de código de APIs da Web

Saiba como configurar o código para seu aplicativo daemon que chama APIs da Web.

## <a name="msal-libraries-that-support-daemon-apps"></a>Bibliotecas MSAL que dão suporte a aplicativos daemon

Essas bibliotecas da Microsoft oferecem suporte a aplicativos de daemon:

  Biblioteca MSAL | Descrição
  ------------ | ----------
  ![MSAL.NET](media/sample-v2-code/logo_NET.png) <br/> MSAL.NET  | As plataformas .NET Framework e .NET Core têm suporte para a criação de aplicativos daemon. (UWP, Xamarin. iOS e Xamarin. Android não têm suporte porque essas plataformas são usadas para criar aplicativos cliente públicos.)
  ![Python](media/sample-v2-code/logo_python.png) <br/> MSAL Python | Suporte para aplicativos daemon em Python.
  ![Java](media/sample-v2-code/logo_java.png) <br/> MSAL Java | Suporte para aplicativos daemon em Java.

## <a name="configure-the-authority"></a>Configurar a autoridade

Os aplicativos daemon usam permissões de aplicativo em vez de permissões delegadas. Portanto, o tipo de conta com suporte não pode ser uma conta em nenhum diretório organizacional ou qualquer conta Microsoft pessoal (por exemplo, Skype, Xbox, Outlook.com). Não há nenhum administrador de locatários para conceder consentimento a um aplicativo daemon para uma conta pessoal da Microsoft. Você precisará escolher *contas em minha organização* ou *contas em qualquer organização*.

Portanto, a autoridade especificada na configuração do aplicativo deve ser locatário (especificando uma ID de locatário ou um nome de domínio associado à sua organização).

Se você for um ISV e quiser fornecer uma ferramenta multilocatário, poderá usar o `organizations` . Mas tenha em mente que você também precisará explicar aos seus clientes como conceder o consentimento do administrador. Para obter detalhes, consulte [solicitando consentimento para um locatário inteiro](v2-permissions-and-consent.md#requesting-consent-for-an-entire-tenant). Além disso, atualmente há uma limitação no MSAL: `organizations` é permitido somente quando as credenciais do cliente são um segredo do aplicativo (não um certificado).

## <a name="configure-and-instantiate-the-application"></a>Configurar e instanciar o aplicativo

Em bibliotecas MSAL, as credenciais do cliente (segredo ou certificado) são passadas como um parâmetro da construção do aplicativo cliente confidencial.

> [!IMPORTANT]
> Mesmo que seu aplicativo seja um aplicativo de console executado como um serviço, se for um aplicativo de daemon, ele precisará ser um aplicativo cliente confidencial.

### <a name="configuration-file"></a>Arquivo de configuração

O arquivo de configuração define:

- A instância de nuvem e a ID de locatário, que juntos compõem a *autoridade*.
- A ID do cliente que você obteve do registro do aplicativo.
- Um segredo do cliente ou um certificado.

# <a name="net"></a>[.NET](#tab/dotnet)

Aqui está um exemplo de como definir a configuração em um [*appsettings.jsno*](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-v2/blob/master/1-Call-MSGraph/daemon-console/appsettings.json) arquivo. Este exemplo é obtido do exemplo de código do [daemon do console do .NET Core](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-v2) no github.

```json
{
  "Instance": "https://login.microsoftonline.com/{0}",
  "Tenant": "[Enter here the tenantID or domain name for your Azure AD tenant]",
  "ClientId": "[Enter here the ClientId for your application]",
  "ClientSecret": "[Enter here a client secret for your application]",
  "CertificateName": "[Or instead of client secret: Enter here the name of a certificate (from the user cert store) as registered with your application]"
}
```

Você fornece um `ClientSecret` ou um `CertificateName` . Essas configurações são exclusivas.

# <a name="python"></a>[Python](#tab/python)

Quando você cria um cliente confidencial com segredos do cliente, o [parameters.jsno](https://github.com/Azure-Samples/ms-identity-python-daemon/blob/master/1-Call-MsGraph-WithSecret/parameters.json) arquivo de configuração no exemplo do [daemon Python](https://github.com/Azure-Samples/ms-identity-python-daemon) é o seguinte:

```Json
{
  "authority": "https://login.microsoftonline.com/<your_tenant_id>",
  "client_id": "your_client_id",
  "scope": [ "https://graph.microsoft.com/.default" ],
  "secret": "The secret generated by AAD during your confidential app registration",
  "endpoint": "https://graph.microsoft.com/v1.0/users"
}
```

Quando você cria um cliente confidencial com certificados, o [parameters.jsno](https://github.com/Azure-Samples/ms-identity-python-daemon/blob/master/2-Call-MsGraph-WithCertificate/parameters.json) arquivo de configuração no exemplo do [daemon Python](https://github.com/Azure-Samples/ms-identity-python-daemon) é o seguinte:

```Json
{
  "authority": "https://login.microsoftonline.com/<your_tenant_id>",
  "client_id": "your_client_id",
  "scope": [ "https://graph.microsoft.com/.default" ],
  "thumbprint": "790E... The thumbprint generated by AAD when you upload your public cert",
  "private_key_file": "server.pem",
  "endpoint": "https://graph.microsoft.com/v1.0/users"
}
```

# <a name="java"></a>[Java](#tab/java)

```Java
 private final static String CLIENT_ID = "";
 private final static String AUTHORITY = "https://login.microsoftonline.com/<tenant>/";
 private final static String CLIENT_SECRET = "";
 private final static Set<String> SCOPE = Collections.singleton("https://graph.microsoft.com/.default");
```

---

### <a name="instantiate-the-msal-application"></a>Instanciar o aplicativo MSAL

Para instanciar o aplicativo MSAL, você precisa adicionar, referenciar ou importar o pacote MSAL (dependendo do idioma).

A construção é diferente, dependendo se você estiver usando os segredos ou certificados do cliente (ou, como um cenário avançado, asserções assinadas).

#### <a name="reference-the-package"></a>Referenciar o pacote

Referencie o pacote MSAL no código do aplicativo.

# <a name="net"></a>[.NET](#tab/dotnet)

Adicione o pacote NuGet [Microsoft. Identity. Client](https://www.nuget.org/packages/Microsoft.Identity.Client) ao seu aplicativo e, em seguida, adicione uma `using` diretiva em seu código para fazer referência a ele.

No MSAL.NET, o aplicativo cliente confidencial é representado pela `IConfidentialClientApplication` interface.

```csharp
using Microsoft.Identity.Client;
IConfidentialClientApplication app;
```

# <a name="python"></a>[Python](#tab/python)

```python
import msal
import json
import sys
import logging
```

# <a name="java"></a>[Java](#tab/java)

```java
import com.microsoft.aad.msal4j.ClientCredentialFactory;
import com.microsoft.aad.msal4j.ClientCredentialParameters;
import com.microsoft.aad.msal4j.ConfidentialClientApplication;
import com.microsoft.aad.msal4j.IAuthenticationResult;
import com.microsoft.aad.msal4j.IClientCredential;
import com.microsoft.aad.msal4j.MsalException;
import com.microsoft.aad.msal4j.SilentParameters;
```

---

#### <a name="instantiate-the-confidential-client-application-with-a-client-secret"></a>Criar uma instância do aplicativo cliente confidencial com um segredo do cliente

Este é o código para instanciar o aplicativo cliente confidencial com um segredo do cliente:

# <a name="net"></a>[.NET](#tab/dotnet)

```csharp
app = ConfidentialClientApplicationBuilder.Create(config.ClientId)
           .WithClientSecret(config.ClientSecret)
           .WithAuthority(new Uri(config.Authority))
           .Build();
```

O `Authority` é uma concatenação da instância de nuvem e da ID de locatário, por exemplo `https://login.microsoftonline.com/contoso.onmicrosoft.com` ou `https://login.microsoftonline.com/eb1ed152-0000-0000-0000-32401f3f9abd` . No *appsettings.jsno* arquivo mostrado na seção [arquivo de configuração](#configuration-file) , eles são representados pelos `Instance` valores e `Tenant` , respectivamente.

No exemplo de código, o trecho anterior foi obtido de, `Authority` é uma propriedade na classe  [AuthenticationConfig](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-v2/blob/ffc4a9f5d9bdba5303e98a1af34232b434075ac7/1-Call-MSGraph/daemon-console/AuthenticationConfig.cs#L61-L70) e é definido como tal:

```csharp
/// <summary>
/// URL of the authority
/// </summary>
public string Authority
{
    get
    {
        return String.Format(CultureInfo.InvariantCulture, Instance, Tenant);
    }
}
```

# <a name="python"></a>[Python](#tab/python)

```Python
# Pass the parameters.json file as an argument to this Python script. E.g.: python your_py_file.py parameters.json
config = json.load(open(sys.argv[1]))

# Create a preferably long-lived app instance that maintains a token cache.
app = msal.ConfidentialClientApplication(
    config["client_id"], authority=config["authority"],
    client_credential=config["secret"],
    # token_cache=...  # Default cache is in memory only.
                       # You can learn how to use SerializableTokenCache from
                       # https://msal-python.rtfd.io/en/latest/#msal.SerializableTokenCache
    )
```

# <a name="java"></a>[Java](#tab/java)

```Java
IClientCredential credential = ClientCredentialFactory.createFromSecret(CLIENT_SECRET);

ConfidentialClientApplication cca =
        ConfidentialClientApplication
                .builder(CLIENT_ID, credential)
                .authority(AUTHORITY)
                .build();
```

---

#### <a name="instantiate-the-confidential-client-application-with-a-client-certificate"></a>Criar uma instância do aplicativo cliente confidencial com um certificado de cliente

Este é o código para criar um aplicativo com um certificado:

# <a name="net"></a>[.NET](#tab/dotnet)

```csharp
X509Certificate2 certificate = ReadCertificate(config.CertificateName);
app = ConfidentialClientApplicationBuilder.Create(config.ClientId)
    .WithCertificate(certificate)
    .WithAuthority(new Uri(config.Authority))
    .Build();
```

# <a name="python"></a>[Python](#tab/python)

```Python
# Pass the parameters.json file as an argument to this Python script. E.g.: python your_py_file.py parameters.json
config = json.load(open(sys.argv[1]))

# Create a preferably long-lived app instance that maintains a token cache.
app = msal.ConfidentialClientApplication(
    config["client_id"], authority=config["authority"],
    client_credential={"thumbprint": config["thumbprint"], "private_key": open(config['private_key_file']).read()},
    # token_cache=...  # Default cache is in memory only.
                       # You can learn how to use SerializableTokenCache from
                       # https://msal-python.rtfd.io/en/latest/#msal.SerializableTokenCache
    )
```

# <a name="java"></a>[Java](#tab/java)

No MSAL Java, há dois construtores para instanciar o aplicativo cliente confidencial com certificados:

```Java

InputStream pkcs12Certificate = ... ; /* Containing PCKS12-formatted certificate*/
string certificatePassword = ... ;    /* Contains the password to access the certificate */

IClientCredential credential = ClientCredentialFactory.createFromCertificate(pkcs12Certificate, certificatePassword);

ConfidentialClientApplication cca =
        ConfidentialClientApplication
                .builder(CLIENT_ID, credential)
                .authority(AUTHORITY)
                .build();
```

ou

```Java
PrivateKey key = getPrivateKey(); /* RSA private key to sign the assertion */
X509Certificate publicCertificate = getPublicCertificate(); /* x509 public certificate used as a thumbprint */

IClientCredential credential = ClientCredentialFactory.createFromCertificate(key, publicCertificate);

ConfidentialClientApplication cca =
        ConfidentialClientApplication
                .builder(CLIENT_ID, credential)
                .authority(AUTHORITY)
                .build();
```

---

#### <a name="advanced-scenario-instantiate-the-confidential-client-application-with-client-assertions"></a>Cenário avançado: instanciar o aplicativo cliente confidencial com declarações de cliente

# <a name="net"></a>[.NET](#tab/dotnet)

Em vez de um segredo do cliente ou um certificado, o aplicativo cliente confidencial também pode provar sua identidade usando as declarações do cliente.

MSAL.NET tem dois métodos para fornecer asserções assinadas ao aplicativo cliente confidencial:

- `.WithClientAssertion()`
- `.WithClientClaims()`

Ao usar `WithClientAssertion` o, você precisa fornecer um JWT assinado. Esse cenário avançado é detalhado em [declarações de cliente](msal-net-client-assertions.md).

```csharp
string signedClientAssertion = ComputeAssertion();
app = ConfidentialClientApplicationBuilder.Create(config.ClientId)
                                          .WithClientAssertion(signedClientAssertion)
                                          .Build();
```

Quando você usar `WithClientClaims` , o MSAL.net produzirá uma asserção assinada que contém as declarações esperadas pelo Azure AD, além de declarações de cliente adicionais que você deseja enviar.
Este código mostra como fazer isso:

```csharp
string ipAddress = "192.168.1.2";
var claims = new Dictionary<string, string> { { "client_ip", ipAddress } };
X509Certificate2 certificate = ReadCertificate(config.CertificateName);
app = ConfidentialClientApplicationBuilder.Create(config.ClientId)
                                          .WithAuthority(new Uri(config.Authority))
                                          .WithClientClaims(certificate, claims)
                                          .Build();
```

Novamente, para obter detalhes, consulte [declarações do cliente](msal-net-client-assertions.md).

# <a name="python"></a>[Python](#tab/python)

No MSAL Python, você pode fornecer declarações de cliente usando as declarações que serão assinadas por essa `ConfidentialClientApplication` chave privada.

```Python
# Pass the parameters.json file as an argument to this Python script. E.g.: python your_py_file.py parameters.json
config = json.load(open(sys.argv[1]))

# Create a preferably long-lived app instance that maintains a token cache.
app = msal.ConfidentialClientApplication(
    config["client_id"], authority=config["authority"],
    client_credential={"thumbprint": config["thumbprint"], "private_key": open(config['private_key_file']).read()},
    client_claims = {"client_ip": "x.x.x.x"}
    # token_cache=...  # Default cache is in memory only.
                       # You can learn how to use SerializableTokenCache from
                       # https://msal-python.rtfd.io/en/latest/#msal.SerializableTokenCache
    )
```

Para obter detalhes, consulte a documentação de referência do Python do MSAL para [ConfidentialClientApplication](https://msal-python.readthedocs.io/en/latest/#msal.ClientApplication.__init__).

# <a name="java"></a>[Java](#tab/java)

```Java
IClientCredential credential = ClientCredentialFactory.createFromClientAssertion(assertion);

ConfidentialClientApplication cca =
        ConfidentialClientApplication
                .builder(CLIENT_ID, credential)
                .authority(AUTHORITY)
                .build();
```

---

## <a name="next-steps"></a>Próximas etapas

# <a name="net"></a>[.NET](#tab/dotnet)

> [!div class="nextstepaction"]
> [Aplicativo de daemon-adquirindo tokens para o aplicativo](./scenario-daemon-acquire-token.md?tabs=dotnet)

# <a name="python"></a>[Python](#tab/python)

> [!div class="nextstepaction"]
> [Aplicativo de daemon-adquirindo tokens para o aplicativo](./scenario-daemon-acquire-token.md?tabs=python)

# <a name="java"></a>[Java](#tab/java)

> [!div class="nextstepaction"]
> [Aplicativo de daemon-adquirindo tokens para o aplicativo](./scenario-daemon-acquire-token.md?tabs=java)

---
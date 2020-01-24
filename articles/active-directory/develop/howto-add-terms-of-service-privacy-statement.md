---
title: Termos de serviço e política de privacidade para aplicativos | Azure
description: Saiba como você pode configurar os termos de declaração de privacidade e de serviço de aplicativo registrado para usar o AD do Azure.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 05/22/2019
ms.author: ryanwi
ms.reviwer: lenalepa, sureshja
ms.custom: aaddev
ms.openlocfilehash: f28c33f20556825d84edda34752ac64714327526
ms.sourcegitcommit: af6847f555841e838f245ff92c38ae512261426a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/23/2020
ms.locfileid: "76697329"
---
# <a name="how-to-configure-terms-of-service-and-privacy-statement-for-an-app"></a>Como: configurar os termos de serviço e a política de privacidade para um aplicativo

Os desenvolvedores que criam e gerenciar aplicativos que se integram ao Azure Active Directory (AD do Azure) e contas da Microsoft devem incluir links para os termos do aplicativo de serviço e declarações de privacidade. Os termos de serviço e a declaração de privacidade são revelados aos usuários por meio da experiência de consentimento do usuário. Eles ajudam seus usuários a saberem que podem confiar em seu aplicativo. Os termos da declaração de privacidade e de serviço são especialmente importantes para aplicativos de multilocação voltadas para o usuário – aplicativos que são usados por vários diretórios ou estão disponíveis para qualquer conta da Microsoft.

Você é responsável por criar os termos de serviço e a privacidade documentos de instrução para seu aplicativo e para fornecer as URLs a esses documentos. Para aplicativos multilocatários que falham ao fornecer esses links, a experiência de consentimento do usuário para seu aplicativo mostrará um alerta, o que pode evitar que usuários de consentimento ao seu aplicativo.

> [!NOTE]
> * Aplicativos de único locatário não exibirá um alerta.
> * Se um ou ambos os links estiverem faltando, seu aplicativo mostrará um alerta.

## <a name="user-consent-experience"></a>Experiência de consentimento do usuário

Os exemplos a seguir mostram o usuário experiência de consentimento quando as condições de declaração de privacidade e de serviço são configuradas e quando esses links não estão configurados.

![Capturas de tela com e sem uma declaração de privacidade e termos de serviço fornecido](./media/howto-add-terms-of-service-privacy-statement/user-consent-exp-privacy-statement-terms-service.png)

## <a name="formatting-links-to-the-terms-of-service-and-privacy-statement-documents"></a>Links de formatação para os termos de serviço e a privacidade documentos de instrução

Antes de adicionar links aos termos de serviço e aos documentos da declaração de privacidade do seu aplicativo, verifique se os URLs seguem essas diretrizes.

| Diretriz     | Description                           |
|---------------|---------------------------------------|
| Formatar        | URL Válida                             |
| Esquemas válidos | HTTP e HTTPS<br/>Recomendamos HTTPS |
| Comprimento máximo    | 2048 caracteres                       |

Exemplos: `https://myapp.com/terms-of-service` e `https://myapp.com/privacy-statement`

## <a name="adding-links-to-the-terms-of-service-and-privacy-statement"></a>Adicionando links aos termos de serviço e declaração de privacidade

Quando os termos de serviço e a declaração de privacidade estiverem prontos, você poderá adicionar links para esses documentos no seu aplicativo usando um destes métodos:

* [ através do portal do Azure ](#azure-portal)
* [ Usando o objeto de aplicativo JSON ](#app-object-json)
* [ Usando a API REST do MSGraph beta ](#msgraph-beta-rest-api)

### <a name="azure-portal"></a>Usando o portal do Azure
Siga estas etapas na portal do Azure.

1. Entre no [portal do Azure](https://portal.azure.com/).
2. Navegue até a seção **Registros de aplicativo** e selecione seu aplicativo.
3. Abra o painel de **identidade visual** .
4. Preencha os campos da **URL de termos de serviço** e **Política de privacidade**.
5. Salve suas alterações.

    ![As propriedades do aplicativo contêm termos de serviço e URLs de política de privacidade](./media/howto-add-terms-of-service-privacy-statement/azure-portal-terms-service-privacy-statement-urls.png)

### <a name="app-object-json"></a> Usando o objeto de aplicativo JSON

Se preferir modificar diretamente o objeto JSON do aplicativo, você poderá usar o editor de manifesto no Portal do Azure ou no Portal de Registro do Aplicativo para incluir links para os termos de serviço e a declaração de privacidade do seu aplicativo.

```json
    "informationalUrls": { 
        "termsOfService": "<your_terms_of_service_url>", 
        "privacy": "<your_privacy_statement_url>" 
    }
```

### <a name="msgraph-beta-rest-api"></a> Usando a API REST do MSGraph beta

Para atualizar programaticamente todos os seus aplicativos, você pode usar a API REST beta do MSGraph para atualizar todos os seus aplicativos para incluir links para os termos de serviço e documentos de declaração de privacidade.

```
PATCH https://graph.microsoft.com/beta/applications/{application id}
{ 
    "appId": "{your application id}", 
    "info": { 
        "termsOfServiceUrl": "<your_terms_of_service_url>", 
        "supportUrl": null, 
        "privacyStatementUrl": "<your_privacy_statement_url>", 
        "marketingUrl": null, 
        "logoUrl": null 
    }
}
```

> [!NOTE]
> * Tenha cuidado para não sobrescrever quaisquer valores preexistentes atribuídos a qualquer um desses campos: `supportUrl`, `marketingUrl` e `logoUrl`
> * A API REST beta do MSGraph só funcionará quando você fizer login com uma conta do AD do Azure. Contas pessoais da Microsoft não são suportadas.

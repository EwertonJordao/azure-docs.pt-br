---
title: Entrar e chamar o Microsoft Graph usando o iOS | Azure
description: Saiba como conectar usuários e chamar a API do Microsoft Graph de um aplicativo iOS.
services: active-directory
author: rwike77
manager: CelesteDG
ms.assetid: 42303177-9566-48ed-8abb-279fcf1e6ddb
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: quickstart
ms.date: 10/25/2019
ms.author: ryanwi
ms.custom: aaddev
ms.reviewer: brandwe
ms.openlocfilehash: 0f612a6f59455162d0acdffc8e05201be0a730d9
ms.sourcegitcommit: af6847f555841e838f245ff92c38ae512261426a
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/23/2020
ms.locfileid: "76703772"
---
# <a name="quickstart-sign-in-users-and-call-the-microsoft-graph-api-from-an-ios-app-v10"></a>Início Rápido: Conectar usuários e chamar a API do Microsoft Graph de um aplicativo iOS (v1.0)

A [plataforma de identidade da Microsoft](v2-overview.md) é uma evolução da plataforma de desenvolvedor do Azure AD (Azure Active Directory). Ela permite que os desenvolvedores criem aplicativos que se conectam a todas as identidades da Microsoft e obtêm tokens para chamar APIs da Microsoft, tais como o Microsoft Graph ou APIs que os desenvolvedores criaram.

A [MSAL (Biblioteca de Autenticação da Microsoft)](msal-overview.md) permite que os desenvolvedores adquiram tokens do ponto de extremidade da plataforma de identidade da Microsoft para acessar APIs Web seguras. A ADAL (Biblioteca de Autenticação do Active Directory) é integrada ao ponto de extremidade do Azure AD para desenvolvedores (v1.0), enquanto a MSAL é integrada ao ponto de extremidade da plataforma de identidade da Microsoft (v2.0).

Para novos aplicativos macOS e iOS, recomendamos que você use a plataforma de identidade da Microsoft (v2.0) e a MSAL para adquirir tokens e acessar APIs Web protegidas: [Início Rápido: Conectar usuários e chamar a API do Microsoft Graph de um aplicativo iOS ou macOS](quickstart-v2-ios.md).

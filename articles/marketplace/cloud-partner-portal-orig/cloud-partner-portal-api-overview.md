---
title: Referência de API do Portal parceiro em nuvem | Mercado Azure
description: Descrição dos pré-requisitos a serem usados e lista de operações de API do Marketplace.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: dsindona
ms.openlocfilehash: 296c667876807bdd05a6281de461df76207b5490
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80288539"
---
<a name="cloud-partner-portal-api-reference"></a>Referência de API do Portal do Cloud Partner
==================================

As APIs REST do Portal do Cloud Partner permitem a recuperação programática e a manipulação de cargas de trabalho, ofertas e perfis de publicação. As APIs usam RBAC (controle de acesso baseado em função) para impor permissões corretas no tempo de processamento.

Essa referência fornece os detalhes técnicos para as APIs REST do Portal do Cloud Partner. Os exemplos de conteúdo deste documento são apenas para referência e estão sujeitos a alterações à medida que novas funcionalidades são adicionadas.


<a name="prerequisites-and-considerations"></a>Pré-requisitos e considerações
-------------------------------

Antes de usar as APIs, você deverá revisar:

- O artigo de [Pré-requisitos](./cloud-partner-portal-api-prerequisites.md) para saber como adicionar uma entidade de serviço à sua conta e obter um token de acesso do Azure AD (Azure Active Directory) para autenticação. 
- Os dois de [controle de simultaneidade](./cloud-partner-portal-api-concurrency-control.md).
estratégias disponíveis para chamar essas APIs.
- As [considerações](./cloud-partner-portal-api-considerations.md) adicionais da API como controle de versão e tratamento de erros.


<a name="common-tasks"></a>Tarefas comuns
------------
Essa referência detalha as APIs para executar as seguintes tarefas comuns.


### <a name="offers"></a>Ofertas

-   [Recuperar todas as ofertas](./cloud-partner-portal-api-retrieve-offers.md)
-   [Recuperar uma oferta específica](./cloud-partner-portal-api-retrieve-specific-offer.md)
-   [Recuperar status da oferta](./cloud-partner-portal-api-retrieve-offer-status.md)
-   [Criar uma oferta](./cloud-partner-portal-api-creating-offer.md)
-   [Publicar uma oferta](./cloud-partner-portal-api-publish-offer.md)

### <a name="operations"></a>Operações

-   [Recuperar operações](./cloud-partner-portal-api-retrieve-operations.md)
-   [Cancelar operações](./cloud-partner-portal-api-cancel-operations.md)

### <a name="publish-an-app"></a>Publicar um aplicativo

-   [Vá ao vivo](./cloud-partner-portal-api-go-live.md)

### <a name="other-tasks"></a>Outras tarefas

-   [Definir preços para ofertas de máquinas virtuais](./cloud-partner-portal-api-setting-price.md)

### <a name="troubleshooting"></a>Solução de problemas

-   [Solucionando problemas de erros de autenticação](./cloud-partner-portal-api-troubleshooting-authentication-errors.md)

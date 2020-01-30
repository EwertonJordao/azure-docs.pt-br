---
title: Desabilitar a verificação de email durante a inscrição do cliente
titleSuffix: Azure AD B2C
description: Saiba como desabilitar a verificação de email durante a inscrição do cliente no Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/25/2018
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: c5b3598e33dac131c8881248a5f4b740a6302e20
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/29/2020
ms.locfileid: "76848012"
---
# <a name="disable-email-verification-during-customer-sign-up-in-azure-active-directory-b2c"></a>Desabilitar a verificação de email durante a inscrição do cliente no Azure Active Directory B2C

Por padrão, Azure Active Directory B2C (Azure AD B2C) verifica o endereço de email do cliente para contas locais (contas para usuários que se inscrevem com endereço de email ou nome de usuário). O Azure AD B2C garante endereços de email válidos exigindo que os clientes os verifiquem durante o processo de inscrição. Ele também impede que os atores mal-intencionados usem processos automatizados para gerar contas fraudulentas em seus aplicativos.

Alguns desenvolvedores de aplicativos preferem ignorar a verificação de email durante o processo de inscrição e, em vez disso, fazer com que os clientes verifiquem seu endereço de email posteriormente. Para dar suporte a isso, o Azure AD B2C pode ser configurado para desabilitar a verificação de email. Isso cria um processo de inscrição mais suave e dá aos desenvolvedores a flexibilidade para diferenciar os clientes que verificaram seu endereço de email dos clientes que não têm.

Siga estas etapas para desabilitar a verificação de email:

1. Entre no [Portal do Azure](https://portal.azure.com)
1. Use o filtro **diretório + assinatura** no menu superior para selecionar o diretório que contém seu locatário Azure ad B2C.
1. No menu à esquerda, selecione **Azure AD B2C**. Ou selecione **Todos os serviços** e pesquise e selecione **Azure AD B2C**.
1. Escolha **Fluxos de usuário**.
1. Selecione o fluxo de usuário para o qual você deseja desabilitar a verificação de email. Por exemplo, *B2C_1_signinsignup*.
1. Selecione **layouts de página**.
1. Selecione **a página de inscrição da conta local**.
1. Em **atributos de usuário**, selecione **endereço de email**.
1. Na lista suspensa **requer verificação** , selecione **não**.
1. Clique em **Salvar**. A verificação de email agora está desabilitada para este fluxo de usuário.

> [!WARNING]
> Desabilitar a verificação de email no processo de inscrição pode gerar spam. Se você desabilitar a verificação de email padrão fornecida pelo Azure AD B2C, recomendamos que implemente um sistema de verificação de substituição.

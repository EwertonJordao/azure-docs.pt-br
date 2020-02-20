---
title: Open Banking (PSD2) e SCA (Autenticação Forte do Cliente) para clientes do Azure
description: Este artigo explica por que a autenticação multifator é necessária para algumas compras do Azure e como concluir a autenticação.
author: bandersmsft
ms.reviewer: judupont
tags: billing
ms.service: cost-management-billing
ms.topic: conceptual
ms.date: 02/10/2020
ms.author: banders
ms.openlocfilehash: 1c4522bed191ef4142cc603bf0e1d22f086111ee
ms.sourcegitcommit: 333af18fa9e4c2b376fa9aeb8f7941f1b331c11d
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/13/2020
ms.locfileid: "77200581"
---
# <a name="open-banking-psd2-and-strong-customer-authentication-sca-for-azure-customers"></a>Open Banking (PSD2) e SCA (Autenticação Forte do Cliente) para clientes do Azure

A partir de 14 de setembro de 2019, os bancos nos 31 países do [Espaço Econômico Europeu](https://en.wikipedia.org/wiki/European_Economic_Area) são obrigados a verificar a identidade da pessoa que faz uma compra online antes que o pagamento seja processado. Essa verificação exige a autenticação multifator para ajudar a garantir que suas compras online sejam seguras e protegidas. A data desse requisito de verificação será atrasada para alguns países. Para obter mais informações, confira as [Perguntas frequentes da Microsoft sobre o PSD2](https://support.microsoft.com/en-us/help/4517854?preview).

## <a name="what-psd2-means-for-azure-customers"></a>O que o PSD2 significa para os clientes do Azure

Se você pagar pelo Azure com um cartão de crédito emitido por um banco no [Espaço Econômico Europeu](https://en.wikipedia.org/wiki/European_Economic_Area), talvez precise concluir a autenticação multifator para a forma de pagamento de sua conta. Talvez você precise concluir o desafio de autenticação multifator ao entrar na sua conta do Azure ou atualizá-la – mesmo que você não esteja fazendo uma compra no momento. Você também pode precisar fornecer a autenticação multifator ao alterar a forma de pagamento de sua conta do Azure, remover o limite de gastos ou efetuar um pagamento imediato no portal do Azure – como a liquidação de saldos pendentes ou a compra de créditos Azure.

Caso seu banco rejeite seus encargos mensais do Azure, você receberá um email do Azure indicando encargos devidos com instruções para corrigir isso. Conclua o desafio da autenticação multifator e liquide seus encargos pendentes no portal do Azure.

## <a name="complete-multi-factor-authentication-in-the-azure-portal"></a>Concluir a autenticação multifator no portal do Azure

As seções a seguir descrevem como concluir a autenticação multifator no portal do Azure. Use as informações que se aplicam ao seu caso.

### <a name="change-the-active-payment-method-of-your-azure-account"></a>Alterar a forma de pagamento ativa de sua conta do Azure

Altere a forma de pagamento ativa de sua conta do Azure seguindo estas etapas:

1. Entre no [portal do Azure](https://portal.azure.com) como o administrador da conta e navegue até **Gerenciamento de Custos + Cobrança**.
2. Na página **Visão Geral**, selecione a assinatura correspondente na grade **Minhas assinaturas**.
3. Em 'Cobrança', selecione **Formas de pagamento**. Adicione um novo cartão de crédito ou defina um cartão existente como a forma de pagamento ativa para a assinatura. Caso seu banco exija a autenticação multifator, você deverá concluir um desafio de autenticação durante o processo.

Para obter mais detalhes, confira [Adicionar, atualizar ou remover um cartão de crédito do Azure](change-credit-card.md).

### <a name="settle-outstanding-charges-for-azure-services"></a>Liquidar encargos pendentes dos serviços do Azure

Caso seu banco rejeite os encargos, o status de sua conta do Azure será alterado para **Devido** no portal do Azure. Verifique o status de sua conta seguindo estas etapas:

1. Entre no [Portal do Azure](https://portal.azure.com/) como Administrador da Conta.
2. Pesquise **Gerenciamento de Custos + Cobrança.**
3. Na página **Gerenciamento de Custos + Cobrança** **Visão Geral**, examine a coluna de status na grade **Minhas assinaturas**.
4. Caso sua assinatura seja rotulada **Devida**, clique em **Liquidar saldo**. Você deverá concluir a autenticação multifator durante o processo.

### <a name="settle-outstanding-charges-for-marketplace-and-reservation-purchases"></a>Liquidar encargos pendentes para compras do Marketplace e de reserva

As compras do Marketplace e de reserva são faturadas separadamente dos serviços do Azure. Caso seu banco rejeite os encargos do Marketplace ou de reserva, sua fatura será exibida como em atraso e você verá a opção de **Pagar agora** no portal do Azure. É possível pagar as faturas vencidas do Marketplace e de reserva seguindo estas etapas:

1. Entre no [Portal do Azure](https://portal.azure.com/) como Administrador da Conta.
2. Pesquise **Gerenciamento de Custos + Cobrança.**
3. Em 'Cobrança', selecione **Faturas**.
5. No filtro suspenso de assinatura, selecione a assinatura associada à sua compra do Marketplace ou de reserva.
6. Na grade de faturas, examine a coluna de tipo. Se o tipo for **Azure Marketplace e Reservas**, você verá um link **Pagar agora** se a fatura estiver devida ou vencida. Se você não vir a opção **Pagar agora**, significa que a fatura já foi paga. Você deverá concluir a autenticação multifator durante o processo de Pagar agora.

## <a name="next-steps"></a>Próximas etapas
- Confira [Resolver o saldo devido de sua assinatura do Azure](resolve-past-due-balance.md) caso você precise pagar uma fatura do Azure.

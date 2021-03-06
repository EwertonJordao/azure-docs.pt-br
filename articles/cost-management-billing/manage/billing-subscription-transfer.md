---
title: Transferir a propriedade de cobrança de uma assinatura do Azure
description: Descreve como transferir a propriedade da cobrança de uma assinatura do Azure para outra conta.
keywords: transferir a assinatura do Azure, assinatura de transferência do Azure, mover a assinatura do Azure para outra conta, alterar o proprietário da assinatura do Azure, transferir a assinatura do Azure para outra conta, cobrança de transferência do Azure
author: bandersmsft
ms.reviewer: amberb
tags: billing,top-support-issue
ms.service: cost-management-billing
ms.subservice: billing
ms.topic: how-to
ms.date: 09/23/2020
ms.author: banders
ms.custom: contperfq1
ms.openlocfilehash: 1fedad7dac4c253c68a0f27d0e0470123833a3be
ms.sourcegitcommit: 67e8e1caa8427c1d78f6426c70bf8339a8b4e01d
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/02/2020
ms.locfileid: "91666102"
---
# <a name="transfer-billing-ownership-of-an-azure-subscription-to-another-account"></a>Transferir a propriedade de cobrança de uma assinatura do Azure para outra conta

Este artigo mostra as etapas necessárias para transferir a propriedade da cobrança de uma assinatura do Azure para outra conta. Para transferir a propriedade da cobrança para uma assinatura, leia [Sobre a transferência da propriedade da cobrança para uma assinatura do Azure](../understand/subscription-transfer.md).

Se desejar manter a propriedade da cobrança, mas alterar o tipo de assinatura, confira [Mudar sua assinatura do Azure para outra oferta](switch-azure-offer.md). Para controlar quem pode acessar recursos na assinatura, confira [Funções internas do Azure](../../role-based-access-control/built-in-roles.md).

Se você for cliente EA (Contrato Enterprise), seus administradores corporativos poderão transferir a propriedade da cobrança de suas assinaturas entre contas. Para obter mais informações, confira [Alterar o proprietário da conta](ea-portal-get-started.md#change-account-owner).

Somente o administrador de cobrança de uma conta pode transferir a propriedade de uma assinatura.

## <a name="transfer-billing-ownership-of-an-azure-subscription"></a>Transferir a propriedade de cobrança de uma assinatura do Azure

1. Entre no [portal do Azure](https://portal.azure.com) como um administrador da conta de cobrança que tem a assinatura que você deseja transferir. Se você não tiver certeza se é administrador ou se precisar determinar quem é, confira [Determinar o administrador de cobrança da conta](../understand/subscription-transfer.md#whoisaa).
1. Pesquise **Gerenciamento de Custos + Cobrança**.  
   ![Captura de tela que mostra a pesquisa do portal do Azure](./media/billing-subscription-transfer/billing-search-cost-management-billing.png)
1. Selecione **Assinaturas** no painel à esquerda. Dependendo do seu nível de acesso, você pode precisar selecionar um escopo do orçamento e, em seguida, **Assinaturas** ou **assinaturas do Azure**.
1. Selecione **Transferir a propriedade de cobrança** para a assinatura que deseja transferir.  
   ![Selecione a assinatura a transferir](./media/billing-subscription-transfer/billing-select-subscription-to-transfer.png)
1. Insira o endereço de email do usuário administrador de cobrança da conta que será o novo proprietário da assinatura.
1. Se você estiver transferindo sua assinatura para uma conta em outro locatário do Azure AD, selecione se deseja mover a assinatura para o locatário da nova conta. Para obter mais informações, confira [Como transferir uma assinatura para uma conta em outro locatário do Azure AD](#transfer-a-subscription-to-another-azure-ad-tenant-account).
    > [!IMPORTANT]
    > Se você optar por migrar a assinatura para o locatário do Azure AD da nova conta, todas as [atribuições de função do Azure](../../role-based-access-control/role-assignments-portal.md) para acessar recursos na assinatura serão permanentemente removidas. Somente o usuário da nova conta que aceitar sua solicitação de transferência terá acesso para gerenciar os recursos da assinatura. Como alternativa, você pode desmarcar a opção **Locatário do Azure AD da assinatura** para transferir a propriedade da cobrança sem mover a assinatura para o locatário da nova conta. Se você fizer isso, as atribuições de função do Azure atuais para acessar os recursos do Azure serão mantidas.  
    ![Enviar página de transferência](./media/billing-subscription-transfer/billing-send-transfer-request.png)
1. Selecione **Enviar solicitação de transferência**.
1. O usuário recebe um email com instruções para revisar sua solicitação de transferência.  
   ![Email de transferência de assinatura enviado para o destinatário](./media/billing-subscription-transfer/billing-receiver-email.png)
1. Para aprovar a solicitação de transferência, o usuário seleciona o link no email e segue as instruções. Depois, o usuário seleciona uma forma de pagamento que será usada para pagar a assinatura. Se o usuário não tiver uma conta do Azure, precisará se inscrever em uma nova conta.  
   ![Página da Web da primeira transferência de assinatura](./media/billing-subscription-transfer/billing-accept-ownership-step1.png)
   ![Página da Web da segunda transferência de assinatura](./media/billing-subscription-transfer/billing-accept-ownership-step2.png)
   ![Página da Web da terceira transferência de assinatura](./media/billing-subscription-transfer/billing-accept-ownership-step3.png)
1. Êxito! Agora a assinatura será transferida.

## <a name="transfer-a-subscription-to-another-azure-ad-tenant-account"></a>Transferir uma assinatura para outra conta de locatário do Azure AD

Um locatário do Azure AD (Active Directory) é criado quando você se inscreve no Azure. O locatário representa sua conta. Você usa o locatário para gerenciar o acesso às assinaturas e aos recursos.

Quando você cria uma nova assinatura, ela é hospedada no locatário do Azure AD da sua conta. Se você quiser fornecer acesso à assinatura ou aos recursos dela a outras pessoas, será necessário convidá-las para ingressar em seu locatário. Isso ajuda a controlar o acesso às assinaturas e aos recursos.

Quando você transfere a propriedade de cobrança da sua assinatura para uma conta em outro locatário do Azure AD, você pode mover a assinatura para o locatário da nova conta. Se fizer isso, todos os usuários, os grupos ou as entidades de serviço que tinham [atribuições de função do Azure](../../role-based-access-control/role-assignments-portal.md) para gerenciar assinaturas e recursos perderão o acesso. Somente o usuário da nova conta que aceitar sua solicitação de transferência terá acesso para gerenciar os recursos. O novo proprietário deve adicionar manualmente esses usuários à assinatura para fornecer acesso ao usuário que a perdeu. Para obter mais informações, confira [Transferir uma assinatura do Azure para um diretório diferente do Azure AD](../../role-based-access-control/transfer-subscription.md).

## <a name="transfer-visual-studio-and-partner-network-subscriptions"></a>Transferir assinaturas do Visual Studio e do Partner Network

As assinaturas do Visual Studio e do Microsoft Partner Network têm crédito Azure mensal recorrente associado a elas. Quando você transfere essas assinaturas, o crédito não fica disponível na conta de cobrança de destino. A assinatura usará o crédito existente na conta de cobrança de destino. Por exemplo, suponha que Bob tenha transferido uma assinatura do Visual Studio Enterprise para a conta de Jane no dia nove de setembro e ela tenha aceitado a transferência. Após a conclusão da transferência, a assinatura passa a usar o crédito da conta de Jane. O crédito será redefinido todo nono dia do mês.

## <a name="next-steps-after-accepting-billing-ownership"></a>Próximas etapas após aceitar a propriedade de cobrança

Se você aceitou a propriedade de cobrança de uma assinatura do Azure, recomendamos que você examine as próximas etapas:

1. Examine e atualize o administrador do serviço, os coadministradores e as atribuições de função do Azure. Para saber mais, confira [Adicionar ou alterar os administradores de assinatura do Azure](add-change-subscription-administrator.md) e [Adicionar ou remover atribuições de função do Azure usando o portal do Azure](../../role-based-access-control/role-assignments-portal.md).
1. Atualize as credenciais associadas aos serviços dessa assinatura, incluindo:
   1. Certificados de gerenciamento que concedem ao usuário direitos de administrador aos recursos de assinatura. Para saber mais, confira [Criar e carregar um certificado de gerenciamento do Azure](../../cloud-services/cloud-services-certs-create.md)
   1. Chaves de acesso para serviços como Armazenamento. Para saber mais, consulte [Sobre as contas de Armazenamento do Azure](../../storage/common/storage-create-storage-account.md)
   1. Credenciais de Acesso Remoto para serviços como Máquinas Virtuais do Azure.
1. Se estiver trabalhando com um parceiro, considere a atualização da ID do parceiro na assinatura. Você pode atualizar a ID do parceiro no [Portal do Azure](https://portal.azure.com). Para obter mais informações, confira [Vincular uma ID de parceiro a suas contas do Azure](link-partner-id.md)

## <a name="troubleshooting"></a>Solução de problemas

Use as informações de solução de problemas a seguir se tiver problemas com a transferência de assinaturas.

### <a name="the-transfer-subscription-option-is-unavailable"></a>A opção "Transferir assinatura" não está disponível

<a name="no-button"></a> 

A transferência de assinatura via autoatendimento não está disponível para sua conta de cobrança. Atualmente, não damos suporte para a transferência da propriedade de cobrança de assinaturas em contas do EA (Contrato Enterprise) no portal do Azure. Além disso, as contas de Contrato de Cliente da Microsoft criadas enquanto trabalham com um representante da Microsoft não dão suporte à transferência de propriedade de cobrança.

###  <a name="not-all-subscription-types-can-transfer"></a>Nem todos os tipos de assinatura podem ser transferidos

Nem todos os tipos de assinatura são compatíveis com a transferência de propriedade de cobrança. Para exibir uma lista de tipos de assinatura que são compatíveis com transferências, confira [Tipos de assinatura compatíveis](../understand/subscription-transfer.md#supported-subscription-types)

###  <a name="access-denied-error-shown-when-trying-to-transfer-subscription-billing-ownership"></a>Erro de acesso negado mostrado durante a tentativa de transferir a propriedade da cobrança da assinatura

Você verá esse erro se estiver tentando transferir uma assinatura do Plano do Microsoft Azure e não tiver as permissões necessárias. Para transferir uma assinatura do plano do Microsoft Azure, você precisará ser um proprietário ou um colaborador na seção da fatura em que a assinatura é cobrada. Para obter mais informações, confira [Gerenciar assinaturas para a seção de fatura](../manage/understand-mca-roles.md#manage-subscriptions-for-invoice-section).

## <a name="need-help-contact-us"></a>Precisa de ajuda? Entre em contato conosco.

Caso tenha dúvidas ou precise de ajuda, [crie uma solicitação de suporte](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Próximas etapas

- Examine e atualize o administrador do serviço, os coadministradores e as atribuições de função do Azure. Para saber mais, confira [Adicionar ou alterar os administradores de assinatura do Azure](add-change-subscription-administrator.md) e [Adicionar ou remover atribuições de função do Azure usando o portal do Azure](../../role-based-access-control/role-assignments-portal.md).

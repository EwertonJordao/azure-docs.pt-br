---
title: Proteção avançada contra ameaças para Azure Cosmos DB
description: Saiba como Azure Cosmos DB fornece criptografia de dados em repouso e como ele é implementado.
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 12/13/2019
ms.custom: seodec18
ms.author: memildin
author: memildin
manager: rkarlin
ms.openlocfilehash: bcc1c6ffe7cdec4aed325a67969235ae993a5109
ms.sourcegitcommit: f15f548aaead27b76f64d73224e8f6a1a0fc2262
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/26/2020
ms.locfileid: "77614843"
---
# <a name="advanced-threat-protection-for-azure-cosmos-db-preview"></a>Proteção avançada contra ameaças para Azure Cosmos DB (versão prévia)

A proteção avançada contra ameaças para Azure Cosmos DB fornece uma camada adicional de inteligência de segurança que detecta tentativas incomuns e potencialmente prejudiciais de acessar ou explorar contas de Azure Cosmos DB. Essa camada de proteção permite que você resolva as ameaças, mesmo sem ser um especialista em segurança, e integre-as aos sistemas de monitoramento de segurança central.

Os alertas de segurança são disparados quando ocorrem anomalias na atividade. Esses alertas de segurança são integrados à [central de segurança do Azure](https://azure.microsoft.com/services/security-center/)e também são enviados por email para administradores de assinatura, com detalhes da atividade suspeita e recomendações sobre como investigar e corrigir as ameaças.

> [!NOTE]
>
> * A proteção avançada contra ameaças para Azure Cosmos DB está disponível no momento apenas para a API do SQL.
> * A proteção avançada contra ameaças para Azure Cosmos DB não está disponível no momento nas regiões do Azure governamental e soberanas Cloud.

Para uma experiência de investigação completa dos alertas de segurança, recomendamos habilitar o [log de diagnóstico no Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/logging), que registra as operações no próprio banco de dados, incluindo operações CRUD em todos os documentos, contêineres e bancos de dados.

## <a name="threat-types"></a>Tipos de ameaça

A proteção avançada contra ameaças para Azure Cosmos DB detecta atividades anormais que indicam tentativas incomuns e potencialmente prejudiciais de acessar ou explorar bancos de dados. Atualmente, ele pode disparar os seguintes alertas:

- **Acesso de locais incomuns**: esse alerta é disparado quando há uma alteração no padrão de acesso para uma conta do Azure Cosmos, em que alguém se conectou ao ponto de extremidade de Azure Cosmos DB de uma localização geográfica incomum. Em alguns casos, o alerta detecta uma ação legítima, o que significa uma nova operação de manutenção do aplicativo ou do desenvolvedor. Em outros casos, o alerta detecta uma ação mal-intencionada de um funcionário antigo, invasor externo, etc.

- **Extração de dados incomum**: esse alerta é disparado quando um cliente está extraindo uma quantidade incomum de dados de uma conta de Azure Cosmos DB. Isso pode ser o sintoma de alguns dados vazamento executados para transferir todos os dados armazenados na conta para um repositório de dados externo.

## <a name="set-up-advanced-threat-protection"></a>Configurar a proteção avançada contra ameaças

### <a name="set-up-atp-using-the-portal"></a>Configurar ATP usando o portal

1. Inicie o portal do Azure em [https://portal.azure.com](https://portal.azure.com/).

2. Na conta Azure Cosmos DB, no menu **configurações** , selecione **segurança avançada**.

    ![Configurar ATP](./media/cosmos-db-advanced-threat-protection/cosmos-db-atp.png)

3. Na folha configuração de **segurança avançada** :

    * Clique na opção **proteção avançada contra ameaças** para defini-la como **ativada**.
    * Clique em **Salvar** para salvar a política de Proteção avançada contra ameaças nova ou atualizada.   

### <a name="set-up-atp-using-rest-api"></a>Configurar ATP usando a API REST

Use comandos da API REST para criar, atualizar ou obter a configuração de proteção avançada contra ameaças para uma conta de Azure Cosmos DB específica.

* [Proteção avançada contra ameaças – criar](https://go.microsoft.com/fwlink/?linkid=2099745)
* [Proteção avançada contra ameaças-obter](https://go.microsoft.com/fwlink/?linkid=2099643)

### <a name="set-up-atp-using-azure-powershell"></a>Configurar ATP usando Azure PowerShell

Use os seguintes cmdlets do PowerShell:

* [Habilitar a proteção avançada contra ameaças](https://go.microsoft.com/fwlink/?linkid=2099607&clcid=0x409)
* [Obtenha proteção avançada contra ameaças](https://go.microsoft.com/fwlink/?linkid=2099608&clcid=0x409)
* [Desabilitar a proteção avançada contra ameaças](https://go.microsoft.com/fwlink/?linkid=2099709&clcid=0x409)

### <a name="using-azure-resource-manager-templates"></a>Usando modelos do Gerenciador de Recursos do Azure

Use um modelo de Azure Resource Manager para configurar o Cosmos DB com a proteção avançada contra ameaças habilitada.
Para obter mais informações, consulte [criar uma conta do CosmosDB com proteção avançada contra ameaças](https://azure.microsoft.com/resources/templates/201-cosmosdb-advanced-threat-protection-create-account/).

### <a name="using-azure-policy"></a>Usando Azure Policy

Use um Azure Policy para habilitar a proteção avançada contra ameaças para Cosmos DB.

1. Inicie a página de **definições de política** do Azure e procure a política **implantar proteção avançada contra ameaças para Cosmos DB** .

    ![Política de pesquisa](./media/cosmos-db-advanced-threat-protection/cosmos-db.png) 

1. Clique na política **implantar proteção avançada contra ameaças para CosmosDB** e, em seguida, clique em **atribuir**.

    ![Selecionar assinatura ou grupo](./media/cosmos-db-advanced-threat-protection/cosmos-db-atp-policy.png)


1. No campo **escopo** , clique nos três pontos, selecione uma assinatura do Azure ou um grupo de recursos e, em seguida, clique em **selecionar**.

    ![Página Definições de política](./media/cosmos-db-advanced-threat-protection/cosmos-db-atp-details.png)


1. Insira os outros parâmetros e clique em **atribuir**.

## <a name="manage-atp-security-alerts"></a>Gerenciar alertas de segurança ATP

Quando ocorre Azure Cosmos DB anomalias de atividade, um alerta de segurança é disparado com informações sobre o evento de segurança suspeito. 

 Na central de segurança do Azure, você pode examinar e gerenciar seus [alertas de segurança](../security-center/security-center-alerts-overview.md)atuais.  Clique em um alerta específico na [central de segurança](https://ms.portal.azure.com/#blade/Microsoft_Azure_Security/SecurityMenuBlade/0) para exibir as possíveis causas e as ações recomendadas para investigar e atenuar a ameaça em potencial. A imagem a seguir mostra um exemplo de detalhes de alerta fornecidos na central de segurança.

 ![Detalhes da ameaça](./media/cosmos-db-advanced-threat-protection/cosmos-db-alert-details.png)

Uma notificação por email também é enviada com os detalhes do alerta e as ações recomendadas. A imagem a seguir mostra um exemplo de um email de alerta.

 ![Detalhes do Alerta](./media/cosmos-db-advanced-threat-protection/cosmos-db-alert.png)

## <a name="cosmos-db-atp-alerts"></a>Cosmos DB alertas ATP

 Para ver uma lista dos alertas gerados durante o monitoramento de contas Azure Cosmos DB, consulte a seção [alertas de Cosmos DB](https://docs.microsoft.com/azure/security-center/alerts-reference#alerts-azurecosmos) na documentação da central de segurança do Azure.

## <a name="next-steps"></a>Próximas etapas

* Saiba mais sobre o [log de diagnóstico no Azure Cosmos DB](cosmosdb-monitor-resource-logs.md)
* Saiba mais sobre a [Central de Segurança do Azure](https://docs.microsoft.com/azure/security-center/security-center-intro)

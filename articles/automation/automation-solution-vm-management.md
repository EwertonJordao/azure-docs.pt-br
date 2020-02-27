---
title: Iniciar/Parar VMs durante a solução de fora do horário comercial
description: Essa solução de gerenciamento de VM é iniciada e interrompe sua Azure Resource Manager máquinas virtuais em um agendamento e monitora proativamente de logs de Azure Monitor.
services: automation
ms.subservice: process-automation
ms.date: 02/25/2020
ms.topic: conceptual
ms.openlocfilehash: cbf181b9a6d3860854c7b61cca0e6c50810cced9
ms.sourcegitcommit: f15f548aaead27b76f64d73224e8f6a1a0fc2262
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/26/2020
ms.locfileid: "77616055"
---
# <a name="startstop-vms-during-off-hours-solution-in-azure-automation"></a>Solução Iniciar/Parar VMs fora do horário comercial na Automação do Azure

A solução Iniciar/Parar VMs fora do horário comercial iniciar e interromper suas máquinas virtuais do Azure em agendas definidas pelo usuário, fornece informações sobre logs de Azure Monitor e envia emails opcionais usando [grupos de ações](../azure-monitor/platform/action-groups.md). A solução dá suporte ao Azure Resource Manager e VMs clássicas na maioria dos cenários. Para usar essa solução com VMs clássicas, você precisa de uma conta RunAs clássica, que não é criada por padrão. Para obter instruções sobre como criar uma conta RunAs clássica, consulte [contas Executar como clássicas](automation-create-standalone-account.md#classic-run-as-accounts).

> [!NOTE]
> A solução Iniciar/Parar VMs fora do horário comercial foi testada com os módulos do Azure que são importados para sua conta de automação quando você implanta a solução. Atualmente, a solução não funciona com versões mais recentes do módulo do Azure. Isso afeta apenas a conta de automação que você usa para executar a solução de Iniciar/Parar VMs fora do horário comercial. Você ainda pode usar versões mais recentes do módulo do Azure em suas outras contas de automação, conforme descrito em [como atualizar os módulos de Azure PowerShell na automação do Azure](automation-update-azure-modules.md)

Essa solução fornece uma opção de automação de baixo custo descentralizada para usuários que desejam otimizar seus custos de VM. Com essa solução, você pode:

- Agendar VMs para iniciar e parar.
- Agendar VMs para iniciar e parar em ordem crescente usando Marcas do Azure (incompatível com VMs clássicas).
- Parar VMs automaticamente com base em baixo uso da CPU.

A seguir estão as limitações da solução atual:

- Essa solução gerencia VMs em qualquer região, mas só pode ser usada na mesma assinatura que sua conta do Azure Automation.
- Esta solução está disponível no Azure e no AzureGov para qualquer região que ofereça suporte a um espaço de trabalho do Log Analytics, uma conta do Azure Automation e Alertas. As regiões do AzureGov atualmente não suportam a funcionalidade de e-mail.

> [!NOTE]
> Se você estiver usando a solução para VMs clássicas, todas as VMs serão processadas sequencialmente pelo serviço de nuvem. Máquinas virtuais ainda são processadas em paralelo entre diferentes serviços de nuvem. Se você tiver mais de 20 VMs por serviço de nuvem, é recomendável criar várias agendas com o runbook pai **ScheduledStartStop_Parent** e especificar 20 VMs por agenda. Nas propriedades da agenda, especifique como uma lista separada por vírgulas, nomes de VM no parâmetro **VMList** . Caso contrário, se o trabalho de automação para esta solução for executado mais de três horas, ele será temporariamente descarregado ou interrompido de acordo com o limite de [compartilhamento justo](automation-runbook-execution.md#fair-share) .
>
> As assinaturas do Azure CSP (Provedor de Soluções na Nuvem do Azure) dão suporte apenas ao modelo do Azure Resource Manager, serviços que não são do Azure Resource Manager não estão disponíveis no programa. Quando a solução Iniciar/Parar é executada, você pode receber erros, pois ele tem cmdlets para gerenciar os recursos clássicos. Para saber mais sobre CSP, confira [Serviços disponíveis em assinaturas do CSP](https://docs.microsoft.com/azure/cloud-solution-provider/overview/azure-csp-available-services). Se usar uma assinatura do CSP, você deverá modificar a variável [**External_EnableClassicVMs**](#variables) para **False** após a implantação.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="prerequisites"></a>Prerequisites

Os runbooks para esta solução funcionam com uma conta do [Azure Run As](automation-create-runas-account.md). A conta Executar como é o método de autenticação preferido, pois ela usa a autenticação de certificado em vez de uma senha que poderá expirar ou ser alterada com frequência.

Recomendamos que você use uma conta de automação separada para a solução iniciar/parar VM. Isso ocorre porque as versões de módulo do Azure são atualizadas com frequência e seus parâmetros podem ser alterados. A solução iniciar/parar VM não é atualizada na mesma cadência para que ela possa não funcionar com versões mais recentes dos cmdlets que ele usa. Também recomendamos que você teste as atualizações de módulo em uma conta de automação de teste antes de importá-las em suas contas de automação de produção.

### <a name="permissions-needed-to-deploy"></a>Permissões necessárias para implantar

Há certas permissões que um usuário deve ter para implantar a solução iniciar/parar VMs fora do horário de expediente. Essas permissões são diferentes se usar uma conta de automação criada previamente e Log Analytics espaço de trabalho ou criar novas durante a implantação. Se você for um colaborador na assinatura e um administrador global em seu locatário Azure Active Directory, não será necessário configurar as permissões a seguir. Se você não tiver esses direitos ou precisar configurar uma função personalizada, consulte as permissões necessárias abaixo.

#### <a name="pre-existing-automation-account-and-log-analytics-workspace"></a>Conta de automação pré-existente e espaço de trabalho de Log Analytics

Para implantar a solução iniciar/parar VMs fora do horário comercial em uma conta de automação existente e Log Analytics espaço de trabalho, o usuário que está implantando a solução requer as seguintes permissões no **grupo de recursos**. Para saber mais sobre as funções, confira [funções personalizadas para recursos do Azure](../role-based-access-control/custom-roles.md).

| Permissão | Escopo|
| --- | --- |
| Microsoft.Automation/automationAccounts/read | Grupo de recursos |
| Microsoft.Automation/automationAccounts/variables/write | Grupo de recursos |
| Microsoft.Automation/automationAccounts/schedules/write | Grupo de recursos |
| Microsoft.Automation/automationAccounts/runbooks/write | Grupo de recursos |
| Microsoft.Automation/automationAccounts/connections/write | Grupo de recursos |
| Microsoft.Automation/automationAccounts/certificates/write | Grupo de recursos |
| Microsoft.Automation/automationAccounts/modules/write | Grupo de recursos |
| Microsoft.Automation/automationAccounts/modules/read | Grupo de recursos |
| Microsoft.automation/automationAccounts/jobSchedules/write | Grupo de recursos |
| Microsoft.Automation/automationAccounts/jobs/write | Grupo de recursos |
| Microsoft.Automation/automationAccounts/jobs/read | Grupo de recursos |
| Microsoft.OperationsManagement/solutions/write | Grupo de recursos |
| Microsoft.OperationalInsights/workspaces/* | Grupo de recursos |
| Microsoft.Insights/diagnosticSettings/write | Grupo de recursos |
| Microsoft.Insights/ActionGroups/Write | Grupo de recursos |
| Microsoft.Insights/ActionGroups/read | Grupo de recursos |
| Microsoft.Resources/subscriptions/resourceGroups/read | Grupo de recursos |
| Microsoft.Resources/deployments/* | Grupo de recursos |

#### <a name="new-automation-account-and-a-new-log-analytics-workspace"></a>Nova conta de automação e um novo espaço de trabalho Log Analytics

Para implantar a solução iniciar/parar VMs fora do horário comercial em uma nova conta de automação e Log Analytics espaço de trabalho, o usuário que está implantando a solução precisa das permissões definidas na seção anterior, bem como as seguintes permissões:

- Coadministrador na assinatura – é necessário apenas criar a conta Executar como clássica se você pretende gerenciar VMs clássicas. [As contas runas clássicas](automation-create-standalone-account.md#classic-run-as-accounts) não são mais criadas por padrão.
- Um membro da função de **desenvolvedor de aplicativo** [Azure Active Directory](../active-directory/users-groups-roles/directory-assign-admin-roles.md) . Para obter mais informações sobre como configurar contas Executar como, consulte [permissões para configurar contas Executar como](manage-runas-account.md#permissions).
- Colaborador na assinatura ou nas permissões a seguir.

| Permissão |Escopo|
| --- | --- |
| Microsoft.Authorization/Operations/read | Subscription|
| Microsoft.Authorization/permissions/read |Subscription|
| Microsoft.Authorization/roleAssignments/read | Subscription |
| Microsoft.Authorization/roleAssignments/write | Subscription |
| Microsoft.Authorization/roleAssignments/delete | Subscription |
| Microsoft.Automation/automationAccounts/connections/read | Grupo de recursos |
| Microsoft.Automation/automationAccounts/certificates/read | Grupo de recursos |
| Microsoft.Automation/automationAccounts/write | Grupo de recursos |
| Microsoft.OperationalInsights/workspaces/write | Grupo de recursos |

## <a name="deploy-the-solution"></a>Implantar a solução

Execute as seguintes etapas para adicionar a solução Iniciar/Parar VMs fora do horário comercial à sua conta de Automação e, em seguida, configure as variáveis para personalizar a solução.

1. Em uma Conta de Automação, selecione **Iniciar/Parar VM** em **Recursos relacionados**. A partir daqui, você pode clicar em **Saiba mais sobre e habilite a solução**. Se você já tiver uma solução Iniciar/Parar VM implantada, selecione-a clicando em **Gerenciar a solução** e localizando-a na lista.

   ![Habilitar a conta de automação](./media/automation-solution-vm-management/enable-from-automation-account.png)

   > [!NOTE]
   > Você também pode criá-lo em qualquer lugar no portal do Azure, clicando em **criar um recurso**. Na página Marketplace, digite uma palavra-chave, como **Iniciar** ou **Iniciar/Parar**. Quando você começa a digitar, a lista é filtrada com base em sua entrada. Como alternativa, você pode digitar uma ou mais palavras-chave do nome completo da solução e, em seguida, pressionar Enter. Selecione **Iniciar/Parar VMs fora do horário** nos resultados da pesquisa.

2. Na página **Iniciar/Parar VMs durante as horas de folga** para a solução selecionada, revise as informações de resumo e clique em **Criar**.

   ![Portal do Azure](media/automation-solution-vm-management/azure-portal-01.png)

3. A página **Adicionar Solução** é exibida. Você será solicitado a configurar a solução antes de importá-la na sua assinatura da Automação.

   ![Página Adicionar Solução de Gerenciamento de VM](media/automation-solution-vm-management/azure-portal-add-solution-01.png)

4. Na página **Adicionar Solução**, selecione **Workspace**. Selecione um espaço de trabalho do Log Analytics que esteja vinculada à mesma assinatura do Azure na qual a conta de Automação está. Se você não tiver um workspace, selecione **Criar Novo Workspace**. Na página **log Analytics espaço de trabalho** , execute as seguintes etapas:
   - Especifique um nome para o novo **espaço de trabalho log Analytics**, como "ContosoLAWorkspace".
   - Selecione uma **Assinatura** à qual se vincular, escolhendo na lista suspensa, caso a assinatura selecionada por padrão não seja adequada.
   - Em **Grupo de Recursos**, você pode criar um novo grupo de recursos ou selecionar um existente.
   - Selecione um **Local**.
   - Selecione um **tipo de preço**. Escolha a opção **Por GB (autônomo)** . Os logs de Azure Monitor têm [preços](https://azure.microsoft.com/pricing/details/log-analytics/) atualizados e a camada por GB é a única opção.

   > [!NOTE]
   > Ao habilitar soluções, somente determinadas regiões têm suporte para vincular um espaço de trabalho do Log Analytics e uma Conta de Automação.
   >
   > Para obter uma lista dos pares de mapeamento com suporte, confira [mapeamento de região para conta de automação e espaço de trabalho de log Analytics](how-to/region-mappings.md).

5. Depois de fornecer as informações necessárias na página **Espaço de Trabalho do Log Analytics**, clique em **Criar**. Você pode acompanhar o progresso em **Notificações** no menu, que retornará a página **Adicionar Solução** ao terminar.
6. Na página **Adicionar Solução**, selecione **Conta de automação**. Se você estiver criando um novo espaço de trabalho do Log Analytics, poderá criar uma nova Conta de automação para associá-la ou selecionar uma conta de automação existente que ainda não esteja vinculada a um espaço de trabalho do Log Analytics. Selecione uma conta de automação existente ou clique em **Criar uma conta de automação** e, na página **Adicionar automação da conta**, forneça as seguintes informações:
   - No campo **Nome**, digite o nome da conta de Automação.

     Todas as outras opções são preenchidas automaticamente com base no espaço de trabalho do Log Analytics selecionado. Essas opções não podem ser modificadas. Uma conta Executar como do Azure é o método de autenticação padrão para os runbooks incluídos nesta solução. Depois de clicar em **OK**, as opções de configuração serão validadas e a conta de Automação será criada. Você pode acompanhar o progresso em **Notificações** no menu.

7. Por fim, na página **Adicionar Solução**, selecione **Configuração**. A página **Parâmetros** é exibida.

   ![Página de parâmetros para a solução](media/automation-solution-vm-management/azure-portal-add-solution-02.png)

   Aqui, você será solicitado a:
   - Especificar os **Nomes do ResourceGroup de destino**. Esses valores são nomes de grupos de recursos que contêm VMs a serem gerenciadas por essa solução. Você pode inserir mais de um nome e separá-los por vírgula (os valores não diferenciam maiúsculas de minúsculas). O uso de um caractere curinga tem suporte para selecionar VMs em todos os grupos de recursos na assinatura. Esse valor é armazenado nas variáveis **External_Start_ResourceGroupNames** e **External_Stop_ResourceGroupNames**.
   - Especifique a **Lista de exclusão de VM (cadeia de caracteres)** . Este valor é o nome de uma ou mais máquinas virtuais do grupo de recursos de destino. Você pode inserir mais de um nome e separá-los por vírgula (os valores não diferenciam maiúsculas de minúsculas). O uso de caracteres curingas é aceito. Esse valor é armazenado na variável **External_ExcludeVMNames**.
   - Selecione um **Agendamento**. Selecione uma data e hora para sua agenda. Uma agenda recorrente diária será criada a partir da hora que você selecionou. A seleção de uma região diferente não está disponível. Para configurar o agendamento de acordo com seu fuso horário específico após a configuração da solução, confira [Modificando o agendamento de inicialização e desligamento](#modify-the-startup-and-shutdown-schedules).
   - Para receber **Notificações por email** de um grupo de ações, aceite o valor padrão **Sim** e forneça um endereço de email válido. Se você selecionar **Não**, mas decidir mais tarde que deseja receber notificações por email, atualize o [grupo de ações](../azure-monitor/platform/action-groups.md) que foi criado, com endereços de email válidos separados por vírgula. Você também precisará habilitar as regras de alerta a seguir:

     - AutoStop_VM_Child
     - Scheduled_StartStop_Parent
     - Sequenced_StartStop_Parent

     > [!IMPORTANT]
     > O valor padrão para **Nomes do ResourceGroup de destino** é um **&ast;** . Isso direciona todas as VMs em uma assinatura. Se você não quiser que a solução direcione todas as VMs em sua assinatura, esse valor precisará ser atualizado para uma lista de nomes de grupos de recursos antes de habilitar os agendamentos.

8. Depois de configurar as definições iniciais necessárias para a solução, clique em **OK** para fechar a página **Parâmetros** e selecione **Criar**. Depois que todas as configurações forem validadas, a solução será implantada em sua assinatura. Esse processo pode levar vários segundos para ser finalizado e você pode acompanhar o progresso em **Notificações** no menu.

> [!NOTE]
> Se você tiver uma assinatura do provedor de soluções na nuvem do Azure (CSP do Azure), após a conclusão da implantação, em sua conta de automação, vá para **variáveis** em **recursos compartilhados** e defina a variável [**External_EnableClassicVMs**](#variables) como **false**. Isso faz com que a solução pare de procurar recursos de VM clássica.

## <a name="scenarios"></a>Cenários

A solução contém três cenários distintos. Esses cenários são:

### <a name="scenario-1-startstop-vms-on-a-schedule"></a>Cenário 1: Iniciar/parar VMs em um agendamento

Este cenário é a configuração padrão ao implantar a solução pela primeira vez. Por exemplo, você pode configurar a solução para parar todas as VMs em uma assinatura ao sair do trabalho à noite e iniciá-las pela manhã ao retornar ao escritório. Quando configurados durante a implantação, os agendamentos **Scheduled-StartVM** e **Scheduled-StopVM** iniciam e param VMs de destino. A configuração desta solução para simplesmente parar VMs tem suporte, consulte [Modificar agendamentos de inicialização e desligamento](#modify-the-startup-and-shutdown-schedules) para aprender a configurar um agendamento personalizado.

> [!NOTE]
> O fuso horário é o seu fuso horário atual de quando o parâmetro de hora do agendamento foi configurado. No entanto, o parâmetro é armazenado no formato UTC na Automação do Azure. Não é necessário executar nenhuma conversão de fuso horário, pois isso acontece durante a implantação.

Para controlar quais VMs estão no escopo, configure as seguintes variáveis: **External_Start_ResourceGroupNames**, **External_Stop_ResourceGroupNames** e **External_ExcludeVMNames**.

Você pode habilitar o direcionamento da ação a uma assinatura e um grupo de recursos ou direcionar a uma lista específica de VMs, mas não ambos.

#### <a name="target-the-start-and-stop-actions-against-a-subscription-and-resource-group"></a>Direcionar a ação de início e parada para uma assinatura e um grupo de recursos

1. Configure as variáveis **External_Stop_ResourceGroupNames** e **External_ExcludeVMNames** para especificar as VMs de destino.
2. Habilite e atualize os agendamentos **Scheduled-StartVM** e **Scheduled-StopVM**.
3. Execute o runbook **ScheduledStartStop_Parent** com o parâmetro ACTION definido como **iniciar** e o parâmetro WHATIF definido como **True** para visualizar as alterações.

#### <a name="target-the-start-and-stop-action-by-vm-list"></a>Direcionar a ação de início e parada por lista de VMs

1. Execute o runbook **ScheduledStartStop_Parent** com o parâmetro ACTION definido como **iniciar**, adicione uma lista de VMs separadas por vírgula no parâmetro *VMList* e defina o parâmetro WHATIF como **True**. Visualize as alterações.
1. Configure o parâmetro **External_ExcludeVMNames** com uma lista de VMs separadas por vírgula (VM1, VM2, VM3).
1. Esse cenário não segue as variáveis **External_Start_ResourceGroupNames** e **External_Stop_ResourceGroupnames**. Para este cenário, é necessário criar seu próprio agendamento da Automação. Para obter detalhes, consulte [Agendamento de runbooks na Automação do Azure](../automation/automation-schedules.md).

> [!NOTE]
> O valor de **Target ResourceGroup Names** é armazenado como o valor de **External_Start_ResourceGroupNames** e **External_Stop_ResourceGroupNames**. Para obter uma maior granularidade, você pode modificar cada uma das variáveis para direcionar diferentes grupos de recursos. Para iniciar a ação, use **External_Start_ResourceGroupNames** e, para parar a ação, use **External_Stop_ResourceGroupNames**. As VMs são adicionadas automaticamente aos agendamentos de início e parada.

### <a name="scenario-2-startstop-vms-in-sequence-by-using-tags"></a>Cenário 2: Iniciar/parar VMs na sequência usando marcas

Em um ambiente que inclui dois ou mais componentes em várias VMs compatíveis com cargas de trabalho distribuídas, é importante dar suporte ao sequenciamento de quais componentes são iniciados/parados em ordem. Para alcançar este cenário, execute as etapas a seguir:

#### <a name="target-the-start-and-stop-actions-against-a-subscription-and-resource-group"></a>Direcionar a ação de início e parada para uma assinatura e um grupo de recursos

1. Adicione as tags **sequencestart** e **sequencestop** com um valor inteiro positivo às VMs que são direcionadas nas variáveis **External_Start_ResourceGroupNames** e **External_Stop_ResourceGroupNames**. As ações iniciar e parar são executadas em ordem crescente. Para saber como marcar uma VM, consulte [Marcar uma Máquina Virtual do Windows no Azure](../virtual-machines/windows/tag.md) e [Marcar uma Máquina Virtual do Linux no Azure](../virtual-machines/linux/tag.md).
1. Modifique os agendamentos **Sequenced-StartVM** e **Sequenced-StopVM** de acordo com a data e hora que atendem às suas exigências e habilite o agendamento.
1. Execute o runbook **SequencedStartStop_Parent** com o parâmetro ACTION definido como **iniciar** e o parâmetro WHATIF definido como **True** para visualizar as alterações.
1. Visualize a ação e faça as alterações necessárias antes de implementar em VMs de produção. Quando estiver pronto, execute o runbook manualmente com o parâmetro definido como **False** ou permita que os agendamentos **Sequenced-StartVM** e **Sequenced-StopVM** da Automação sejam executados automaticamente de acordo com o agendamento prescrito.

#### <a name="target-the-start-and-stop-action-by-vm-list"></a>Direcionar a ação de início e parada por lista de VMs

1. Adicione as marcas **sequencestart** e **sequencestop** com um valor inteiro positivo às VMs que você planeja adicionar ao parâmetro **VMList**.
1. Execute o runbook **SequencedStartStop_Parent** com o parâmetro ACTION definido como **iniciar**, adicione uma lista de VMs separadas por vírgula no parâmetro *VMList* e defina o parâmetro WHATIF como **True**. Visualize as alterações.
1. Configure o parâmetro **External_ExcludeVMNames** com uma lista de VMs separadas por vírgula (VM1, VM2, VM3).
1. Esse cenário não segue as variáveis **External_Start_ResourceGroupNames** e **External_Stop_ResourceGroupnames**. Para este cenário, é necessário criar seu próprio agendamento da Automação. Para obter detalhes, consulte [Agendamento de runbooks na Automação do Azure](../automation/automation-schedules.md).
1. Visualize a ação e faça as alterações necessárias antes de implementar em VMs de produção. Quando estiver pronto, execute o runbook monitoring-and-diagnostics/monitoring-action-groups manualmente com o parâmetro definido como **False** ou permita que a Automação agende a execução de **Sequenced-StartVM** e de **Sequenced-StopVM** automaticamente após o agendamento prescrito.

## <a name="solution-components"></a>Componentes da solução

Essa solução inclui runbooks pré-configurados, agendas e integração com logs de Azure Monitor para que você possa personalizar a inicialização e o desligamento de suas máquinas virtuais para atender às suas necessidades de negócios.

### <a name="runbooks"></a>Runbooks

A tabela a seguir lista os runbooks implantados em sua conta da Automação por esta solução. Não faça alterações no código do runbook. Em vez disso, escreva seu próprio runbook para obter novas funcionalidades.

> [!IMPORTANT]
> Não execute diretamente nenhum runbook com "child" acrescentado ao nome.

Todos os runbooks pai incluem o parâmetro _WhatIf_. Quando definido como **True**, o _WhatIf_ é compatível com o detalhamento do comportamento exato que o runbook assume quando executado sem o parâmetro _WhatIf_ e valida as VMs corretas que estão sendo direcionadas. Um runbook só executa suas ações definidas quando o parâmetro _WhatIf_ é definido como **False**.

|Runbook | parâmetros | DESCRIÇÃO|
| --- | --- | ---|
|AutoStop_CreateAlert_Child | VMObject <br> AlertAction <br> WebHookURI | Chamada a partir do runbook pai. Este runbook cria alertas por recurso para o cenário AutoStop.|
|AutoStop_CreateAlert_Parent | VMList<br> WhatIf: Verdadeiro ou Falso  | Cria ou atualiza regras de alerta do Azure em VMs nos grupos de assinatura ou de recursos de destino. <br> VMList: lista de VMs separadas por vírgula. Por exemplo, _vm1, vm2, vm3_.<br> *WhatIf* valida a lógica de runbook sem execução.|
|AutoStop_Disable | none | Desabilita os alertas de AutoStop e o agendamento padrão.|
|AutoStop_StopVM_Child | WebHookData | Chamada a partir do runbook pai. As regras de alerta chamam esse runbook para parar a VM.|
|Bootstrap_Main | none | Usado uma vez para definir configurações de inicialização, como webhookURI, que normalmente não podem ser acessadas no Azure Resource Manager. Este runbook é removido automaticamente após a implantação bem-sucedida.|
|ScheduledStartStop_Child | VMName <br> Ação: Iniciar ou Parar <br> ResourceGroupName | Chamada a partir do runbook pai. Executa uma ação de iniciar ou parar para a parada agendada.|
|ScheduledStartStop_Parent | Ação: Iniciar ou Parar <br>VMList <br> WhatIf: Verdadeiro ou Falso | Esta configuração afeta todas as VMs na assinatura. Edite o **External_Start_ResourceGroupNames** e **External_Stop_ResourceGroupNames** para executar apenas nesses grupos de recursos de destino. Você também pode excluir VMs específicas atualizando a variável **External_ExcludeVMNames**.<br> VMList: lista de VMs separadas por vírgula. Por exemplo, _vm1, vm2, vm3_.<br> _WhatIf_ valida a lógica de runbook sem execução.|
|SequencedStartStop_Parent | Ação: Iniciar ou Parar <br> WhatIf: Verdadeiro ou Falso<br>VMList| Crie marcas com o nome **sequencestart** e **sequencestop** em cada VM para as quais você deseja sequenciar a atividade de iniciar/parar. Os nomes das tags diferenciam maiúsculas de minúsculas. O valor da marca deve ser um inteiro positivo (1, 2, 3) que corresponde à ordem em que você deseja iniciar ou parar. <br> VMList: lista de VMs separadas por vírgula. Por exemplo, _vm1, vm2, vm3_. <br> _WhatIf_ valida a lógica de runbook sem execução. <br> **Observação**: as VMs devem fazer parte dos grupos de recursos definidos como External_Start_ResourceGroupNames, External_Stop_ResourceGroupNames e External_ExcludeVMNames em variáveis da Automação do Azure. Elas devem ter as marcas apropriadas para que as ações entrem em vigor.|

### <a name="variables"></a>variáveis

A tabela a seguir lista as variáveis criadas na sua conta da Automação. Modifique apenas as variáveis com prefixo **External**. Modificar variáveis prefixadas com **Internal** causará efeitos indesejáveis.

|Variável | DESCRIÇÃO|
|---------|------------|
|External_AutoStop_Condition | O operador condicional exigido para configurar a condição antes de disparar um alerta. Os valores aceitáveis são **GreaterThan**, **GreaterThanOrEqual**, **LessThan** e **LessThanOrEqual**.|
|External_AutoStop_Description | O alerta para parar a VM se o percentual da CPU exceder o limite.|
|External_AutoStop_MetricName | O nome da métrica de desempenho para a qual a regra de alerta do Azure deverá ser configurada.|
|External_AutoStop_Threshold | O limite para a regra de alerta do Azure especificada na variável _External_AutoStop_MetricName_. Os valores percentuais podem variar de 1 a 100.|
|External_AutoStop_TimeAggregationOperator | O operador de agregação de tempo que é aplicado ao tamanho de janela selecionado para avaliar a condição. Os valores aceitáveis são **Médio**, **Mínimo**, **Máximo**, **Total** e **Último**.|
|External_AutoStop_TimeWindow | O tamanho da janela durante o qual o Azure analisa a métrica selecionada para disparar um alerta. Esse parâmetro aceita a entrada no formato Intervalo de tempo. Os valores possíveis são de 5 minutos até 6 horas.|
|External_EnableClassicVMs| Especifica se a solução destina-se a VMs clássicas. O valor padrão é True. Isso deve ser definido como False para assinaturas CSP. As VMs clássicas exigem uma [conta Executar como clássica](automation-create-standalone-account.md#classic-run-as-accounts).|
|External_ExcludeVMNames | Insira os nomes das VMs a serem excluídas, separando-os por vírgula, sem espaços. Isso é limitado a 140 VMs. Se você adicionar mais de 140 VMs a essa lista separada por vírgulas, as VMs definidas como excluídas poderão ser iniciadas ou interrompidas inadvertidamente.|
|External_Start_ResourceGroupNames | Especifica um ou mais grupos de recursos, separando os valores por vírgula, direcionados a ações de iniciar.|
|External_Stop_ResourceGroupNames | Especifica um ou mais grupos de recursos, separando os valores por vírgula, direcionados a ações de parar.|
|Internal_AutomationAccountName | Especifica o nome da conta de Automação.|
|Internal_AutoSnooze_WebhookUri | Especifica o URI do Webhook chamado para o cenário AutoStop.|
|Internal_AzureSubscriptionId | Especifica a ID da Assinatura do Azure.|
|Internal_ResourceGroupName | Especifica o nome do grupo de recursos da conta da Automação.|

Em todos os cenários, as variáveis **External_Start_ResourceGroupNames**, **External_Stop_ResourceGroupNames** e **External_ExcludeVMNames** são necessárias para direcionar VMs, com exceção da disponibilização de uma lista de VMs separadas por vírgula para os runbooks **AutoStop_CreateAlert_Parent**, **SequencedStartStop_Parent** e **ScheduledStartStop_Parent**. Em outras palavras, suas VMs devem residir em grupos de recursos de destino para que as ações iniciar e parar ocorram. A lógica funciona semelhante à política do Azure, isto é, você pode direcionar a assinatura ou o grupo de recursos e fazer com que as ações sejam herdadas por VMs recém-criadas. Com essa abordagem, não é necessário ter um agendamento distinto para cada VM nem gerenciar as ações iniciar e parar em escala.

### <a name="schedules"></a>Agendas

A tabela a seguir lista cada uma das agendas padrão criadas em sua conta de Automação. Você pode modificá-los ou criar suas próprias agendas personalizadas. Por padrão, todas as agendas são desabilitadas, exceto **Scheduled_StartVM** e **Scheduled_StopVM**.

Você não deve habilitar todas os agendamentos, porque isso poderá criar ações de agendamento sobrepostas. É melhor determinar quais otimizações você deseja executar e modificar de acordo. Consulte os exemplos de cenários na seção Visão geral para obter explicações adicionais.

|Nome da agenda | Frequência | DESCRIÇÃO|
|--- | --- | ---|
|Schedule_AutoStop_CreateAlert_Parent | A cada 8 horas | Executa o runbook AutoStop_CreateAlert_Parent a cada 8 horas, que, por sua vez, interrompe os valores baseados em VM em External_Start_ResourceGroupNames, External_Stop_ResourceGroupNames e External_ExcludeVMNames nas variáveis da Automação do Azure. Como alternativa, você pode especificar uma lista de VMs separadas por vírgula utilizando o parâmetro VMList.|
|Scheduled_StopVM | Definida pelo usuário, diariamente | Executa o runbook Scheduled_Parent com um parâmetro de _Parar_ todos os dias no horário especificado. Interrompe automaticamente todas as VMs que atendem às regras definidas pelas variáveis de ativo. Habilite o agendamento relacionado, Scheduled **-StartVM**.|
|Scheduled_StartVM | Definida pelo usuário, diariamente | Executa o runbook Scheduled_Parent com um parâmetro de _Iniciar_ todos os dias no horário especificado. Inicia automaticamente todas as VMs que atendem às regras definidas pelas variáveis adequadas. Habilite o agendamento relacionado, Scheduled **-StopVM**.|
|Sequenced-StopVM | 1h (UTC), toda sexta-feira | Executa o runbook Scheduled_Parent com um parâmetro de _Parar_ toda sexta-feira no horário especificado. Sequencialmente (crescente) interrompe todas as VMs com uma marca de **SequenceStop** definida pelas variáveis apropriadas. Para ver mais informações sobre os valores de tag e variáveis de ativos, confira a seção de Runbooks. Habilite o agendamento relacionado, **Sequenced-StartVM**.|
|Sequenced-StartVM | 13h (UTC), toda segunda-feira | Executa o runbook Scheduled_Parent com um parâmetro de _Parar_ toda segunda-feira no horário determinado. Inicia sequencialmente (em ordem decrescente) todas as VMs com uma marca de **SequenceStart** definida pelas variáveis adequadas. Para ver mais informações sobre os valores de tag e variáveis de ativos, confira a seção de Runbooks. Habilite o agendamento relacionado, **Sequenced-StopVM**.|

## <a name="azure-monitor-logs-records"></a>Registros de logs de Azure Monitor

A Automação cria dois tipos de registros no espaço de trabalho do Log Analytics: logs de trabalho e fluxos de trabalho.

### <a name="job-logs"></a>Logs de trabalho

|Propriedade | DESCRIÇÃO|
|----------|----------|
|Chamador |  Quem iniciou a operação. Os valores possíveis são um endereço de email ou o sistema para trabalhos agendados.|
|Categoria | Classificação do tipo de dados. Para a Automação, o valor é JobLogs.|
|CorrelationId | O GUID que é a ID de correlação do trabalho de runbook.|
|JobId | GUID que é a ID do trabalho de runbook.|
|operationName | Especifica o tipo de operação realizada no Azure. Para a Automação, o valor é Job.|
|resourceId | Especifica o tipo de recurso no Azure. Para a Automação, o valor é a conta da Automação associada ao runbook.|
|ResourceGroup | Especifica o nome do grupo de recursos do trabalho do runbook.|
|ResourceProvider | Especifica o serviço do Azure que fornece os recursos que você pode implantar e gerenciar. Para Automação, o valor é Automação do Azure.|
|ResourceType | Especifica o tipo de recurso no Azure. Para a Automação, o valor é a conta da Automação associada ao runbook.|
|resultType | O status do trabalho de runbook. Os valores possíveis são:<br>- Iniciado<br>- Parado<br>- Suspenso<br>- Com falha<br>- Êxito|
|resultDescription | Descreve o estado de resultado do trabalho de runbook. Os valores possíveis são:<br>- O trabalho foi iniciado<br>- O trabalho falhou<br>- Trabalho Concluído|
|RunbookName | Especifica o nome do runbook.|
|SourceSystem | Especifica o sistema de origem dos dados enviados. Para a Automação, o valor é OpsManager|
|StreamType | Especifica o tipo de evento. Os valores possíveis são:<br>- Detalhado<br>- Saída<br>- Erro<br>- Aviso|
|SubscriptionId | Especifica a ID da assinatura do trabalho.
|Hora | Data e hora da execução do trabalho de runbook.|

### <a name="job-streams"></a>Transmissões de trabalho

|Propriedade | DESCRIÇÃO|
|----------|----------|
|Chamador |  Quem iniciou a operação. Os valores possíveis são um endereço de email ou o sistema para trabalhos agendados.|
|Categoria | Classificação do tipo de dados. Para a Automação, o valor é JobStreams.|
|JobId | GUID que é a ID do trabalho de runbook.|
|operationName | Especifica o tipo de operação realizada no Azure. Para a Automação, o valor é Job.|
|ResourceGroup | Especifica o nome do grupo de recursos do trabalho do runbook.|
|resourceId | Especifica o tipo de ID de recurso no Azure. Para a Automação, o valor é a conta da Automação associada ao runbook.|
|ResourceProvider | Especifica o serviço do Azure que fornece os recursos que você pode implantar e gerenciar. Para Automação, o valor é Automação do Azure.|
|ResourceType | Especifica o tipo de recurso no Azure. Para a Automação, o valor é a conta da Automação associada ao runbook.|
|resultType | O resultado do trabalho de runbook no momento em que o evento foi gerado. Um valor possível é:<br>- InProgress|
|resultDescription | Inclui o fluxo de saída do runbook.|
|RunbookName | O nome do runbook.|
|SourceSystem | Especifica o sistema de origem dos dados enviados. Para a Automação, o valor é OpsManager.|
|StreamType | O tipo de fluxo de trabalho. Os valores possíveis são:<br>- Andamento<br>- Saída<br>- Aviso<br>- Erro<br>- Depurar<br>- Detalhado|
|Hora | Data e hora da execução do trabalho de runbook.|

Quando você executa uma pesquisa de logs que retorna registros da categoria de **JobLogs** ou **JobStreams**, pode selecionar a exibição **JobLogs** ou **JobStreams**, que exibe um conjunto de blocos resumindo as atualizações retornadas pela pesquisa.

## <a name="sample-log-searches"></a>Pesquisas de log de exemplo

A tabela a seguir fornece pesquisas de log de exemplo para os registros de alerta coletados por essa solução.

|Consulta | DESCRIÇÃO|
|----------|----------|
|Localizar trabalhos para o runbook ScheduledStartStop_Parent que foram finalizados com êxito | <code>search Category == "JobLogs" <br>&#124;  where ( RunbookName_s == "ScheduledStartStop_Parent" ) <br>&#124;  where ( ResultType == "Completed" )  <br>&#124;  summarize AggregatedValue = count() by ResultType, bin(TimeGenerated, 1h) <br>&#124;  sort by TimeGenerated desc</code>|
|Localizar trabalhos para o runbook SequencedStartStop_Parent que foram finalizados com êxito | <code>search Category == "JobLogs" <br>&#124;  where ( RunbookName_s == "SequencedStartStop_Parent" ) <br>&#124;  where ( ResultType == "Completed" ) <br>&#124;  summarize AggregatedValue = count() by ResultType, bin(TimeGenerated, 1h) <br>&#124;  sort by TimeGenerated desc</code>|

## <a name="viewing-the-solution"></a>Exibindo a solução

Para acessar a solução, navegue até sua conta de automação, selecione **Workspace** em **RECURSOS RELACIONADOS**. Na página log Analytics, selecione **soluções** em **geral**. Na página **Soluções**, selecione a solução **Start-Stop-VM[workspace]** na lista.

A seleção da solução exibe a página de soluções **Start-Stop-VM [workspace]** . Aqui você pode analisar detalhes importantes, como o bloco **StartStopVM**. Como no seu espaço de trabalho do Log Analytics, esse bloco exibe uma contagem e uma representação gráfica dos trabalhos de runbook da solução que foi iniciada e encerrada com êxito.

![Página da solução Gerenciamento de Atualizações de Automação](media/automation-solution-vm-management/azure-portal-vmupdate-solution-01.png)

Daqui, você pode executar outras análises dos registros de trabalho clicando no bloco de rosca. O painel da solução mostra o histórico de trabalhos e as consultas de pesquisa de logs pré-definidas. Alterne para o portal avançado do log Analytics para pesquisar com base em suas consultas de pesquisa.

## <a name="configure-email-notifications"></a>Configurar notificações por email

Para alterar as notificações por email depois que a solução for implantada, modifique o grupo de ações que foi criado durante a implantação.  

> [!NOTE]
> As assinaturas na nuvem do Azure Governamental não são compatíveis com a funcionalidade de email desta solução.

No portal do Azure, navegue até Monitorar -> grupos de ações. Selecione o grupo de ações intitulado **StartStop_VM_Notication**.

![Página da solução Gerenciamento de Atualizações de Automação](media/automation-solution-vm-management/azure-monitor.png)

Na página **StartStop_VM_Notification**, clique em **Editar detalhes** em **Detalhes**. Isso abre a página **Email/SMS/Push/Voz**. Atualize o endereço de email e clique em **OK** para salvar suas alterações.

![Página da solução Gerenciamento de Atualizações de Automação](media/automation-solution-vm-management/change-email.png)

Como alternativa, você pode adicionar mais ações no grupo de ações. Para saber mais sobre grupos de ações, confira [grupos de ações](../azure-monitor/platform/action-groups.md)

A seguir está um email de exemplo que é enviado quando a solução desliga as máquinas virtuais.

![Página da solução Gerenciamento de Atualizações de Automação](media/automation-solution-vm-management/email.png)

## <a name="add-exclude-vms"></a>Adicionar/excluir VMs

A solução fornece a capacidade de adicionar VMs para o destino da solução ou excluir máquinas específicas da solução.

### <a name="add-a-vm"></a>Adicionar uma VM

Há algumas opções que você pode usar para ter certeza de que uma VM está incluída na solução Iniciar/Parar quando ela é executada.

* Cada um dos [runbooks](#runbooks) pai da solução tem um parâmetro **VMList** . Você pode passar uma lista separada por vírgulas de nomes de VM para esse parâmetro ao agendar o runbook pai apropriado para sua situação e essas VMs serão incluídas quando a solução for executada.

* Para selecionar várias VMs, defina **External_Start_ResourceGroupNames** e **External_Stop_ResourceGroupNames** com os nomes de grupo de recursos que contêm as VMs que você deseja iniciar ou parar. Você também pode definir esse valor como `*` para fazer a solução ser executada em relação a todos os grupos de recursos na assinatura.

### <a name="exclude-a-vm"></a>Excluir uma VM

Para excluir uma VM da solução, você pode adicioná-la à variável **External_ExcludeVMNames**. Essa variável é uma lista separada por vírgulas de VMs específicas a serem excluídas da solução iniciar/parar. Essa lista é limitada a 140 VMs. Se você adicionar mais de 140 VMs a essa lista separada por vírgulas, as VMs definidas como excluídas poderão ser iniciadas ou interrompidas inadvertidamente.

## <a name="modify-the-startup-and-shutdown-schedules"></a>Modificar as agendas de inicialização e desligamento

O gerenciamento das agendas de inicialização e desligamento nesta solução segue as mesmas etapas descritas em [Agendando um runbook na Automação do Azure](automation-schedules.md). É necessário haver um agendamento separado para iniciar e parar VMs.

Há suporte para configurar a solução para simplesmente parar as VMs em um determinado momento. Neste cenário, você acabou de criar um agendamento **Parar** e nenhum agendamento **Iniciar** correspondente. Para fazer isso, você precisa:

1. Não deixe de adicionar os grupos de recursos das VMs que serão desligadas na variável **External_Stop_ResourceGroupNames**.
2. Crie sua própria agenda para a hora em que você deseja desligar as máquinas virtuais.
3. Navegue até o runbook **ScheduledStartStop_Parent** e clique em **Agenda**. Isso permite que você selecione a agenda que criou na etapa anterior.
4. Selecione **Parâmetros e configurações de execução** e defina o parâmetro ACTION como "Stop".
5. Clique em **OK** para salvar as alterações.

## <a name="update-the-solution"></a>Atualizar a solução

Se você tiver implantado uma versão anterior dessa solução, será preciso primeiro excluí-la da sua conta antes de implantar uma versão atualizada. Siga as etapas para [remover a solução](#remove-the-solution) e, em seguida, siga as etapas acima para [implantar a solução](#deploy-the-solution).

## <a name="remove-the-solution"></a>Remover a solução

Se decidir que não precisa mais usar a solução, você poderá excluí-la da conta de Automação. A exclusão da solução remove apenas os runbooks. Ela não exclui os agendamentos ou as variáveis que foram criadas quando a solução foi adicionada. Esses ativos deverão ser excluídos manualmente se não estiverem sendo usados com outros runbooks.

Para excluir a solução, execute as etapas a seguir:

1. Em sua conta de automação, em **recursos relacionados**, selecione **espaço de trabalho vinculado**.
1. Selecione **ir para o espaço de trabalho**.
1. Em **geral**, selecione **soluções**. 
1. Na página **Soluções**, selecione a solução **Start-Stop-VM[Workspace]** . Na página **VMManagementSolution[Workspace]** , no menu, selecione **Excluir**.<br><br> ![Excluir a Solução de Gerenciamento de VM](media/automation-solution-vm-management/vm-management-solution-delete.png)
1. Na janela **Excluir Solução**, confirme que deseja excluir a solução.
1. Enquanto as informações são verificadas e a solução é excluída, você pode acompanhar seu progresso no menu **Notificações**. Você é levado de volta à página **Soluções** após o início do processo de remoção da solução.

A conta de Automação e o espaço de trabalho do Log Analytics não serão excluídos como parte desse processo. Se você não deseja manter o espaço de trabalho do Log Analytics, é necessário excluí-lo manualmente. Isso pode ser feito no portal do Azure:

1. Em portal do Azure, procure e selecione **espaços de trabalho do log Analytics**.
1. Na página **log Analytics espaços de trabalho** , selecione o espaço de trabalho.
1. Selecione **Excluir** no menu da página de configurações do workspace.

Se você não deseja manter os componentes de conta de automação do Azure, você poderá excluir cada um manualmente. Para obter a lista de runbooks, variáveis e agendamentos criados pela solução, consulte a [componentes da solução](#solution-components).

## <a name="next-steps"></a>Próximas etapas

- Para saber mais sobre como construir diferentes consultas de pesquisa e examinar os logs de trabalho de automação com logs de Azure Monitor, consulte [pesquisas de log em logs de Azure monitor](../log-analytics/log-analytics-log-searches.md).
- Para saber mais sobre a execução de runbooks, como monitorar trabalhos de runbook e outros detalhes técnicos, confira [Acompanhar um trabalho de runbook](automation-runbook-execution.md).
- Para saber mais sobre os logs de Azure Monitor e fontes de coleta de dados, consulte [visão geral sobre coleta de dados do armazenamento do Azure em logs de Azure monitor](../azure-monitor/platform/collect-azure-metrics-logs.md).

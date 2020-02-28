---
title: Funções, permissões e segurança no Azure Monitor
description: Saiba como usar funções e permissões internas do Azure Monitor para restringir o acesso aos recursos de monitoramento.
author: johnkemnetz
services: azure-monitor
ms.topic: conceptual
ms.date: 11/27/2017
ms.author: johnkem
ms.subservice: ''
ms.openlocfilehash: 81309f0b5781e6302887a5b079ed359e70659834
ms.sourcegitcommit: 747a20b40b12755faa0a69f0c373bd79349f39e3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/27/2020
ms.locfileid: "77658958"
---
# <a name="roles-permissions-and-security-in-azure-monitor"></a>Funções, permissões e segurança no Azure Monitor

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Muitas equipes precisam regular estritamente o acesso aos dados e configurações de monitoramento. Por exemplo, se você tiver membros da equipe que trabalham exclusivamente no monitoramento (engenheiros de suporte, engenheiros de DevOps) ou se usar um provedor de serviços gerenciados, talvez você queira conceder a eles acesso apenas aos dados de monitoramento e restringir sua capacidade de criar, modificar ou excluir recursos. Este artigo mostra como aplicar uma função interna de RBAC de monitoramento a um usuário no Azure rapidamente ou criar sua própria função personalizada para um usuário que precise de permissões limitadas de monitoramento. Em seguida, ele aborda considerações de segurança para os recursos relacionados ao Azure Monitor e como você pode limitar o acesso aos dados contidos nos mesmos.

## <a name="built-in-monitoring-roles"></a>Funções internas de monitoramento
As funções internas do Azure Monitor são projetadas para ajudar a limitar o acesso aos recursos em uma assinatura, permitindo que as pessoas responsáveis pelo monitoramento da infra-estrutura obtenham e configurem os dados que precisam. O Azure Monitor fornece duas funções integradas: um Leitor de monitoramento e um Colaborador de monitoramento.

### <a name="monitoring-reader"></a>Leitor de monitoramento
Pessoas atribuídas à função de Leitor de monitoramento podem exibir todos os dados de monitoramento em uma assinatura, mas não podem modificar nenhum recurso nem editar nenhuma configuração relacionados ao monitoramento de recursos. Essa função é apropriada para os usuários em uma organização, como engenheiros de suporte ou de operações, que precisam ser capazes de:

* Exibir painéis de monitoramentos no portal e criar seus próprios painéis de monitoramentos privados.
* Exibir regras de alerta definidas nos [Alertas do Azure](alerts-overview.md)
* Consultar métricas usando a [API REST do Azure Monitor](https://msdn.microsoft.com/library/azure/dn931930.aspx), [os cmdlets do PowerShell](powershell-quickstart-samples.md) ou a [CLI de plataforma cruzada](cli-samples.md).
* Consultar o Log de atividades usando o portal, a API REST do Azure Monitor, os cmdlets do PowerShell ou a CLI de plataforma cruzada.
* Exibir as [configurações de diagnóstico](diagnostic-settings.md) para um recurso.
* Exibir o [perfil de log](activity-log-export.md) para uma assinatura.
* Exibir as configurações de autoescala.
* Exibir as configurações e a atividade do alerta.
* Acessar os dados do Application Insights e exiba os dados na Análise de AI.
* Pesquisar dados do espaço de trabalho do Log Analytics, incluindo dados de uso do espaço de trabalho.
* Exibir grupos de gerenciamento do Log Analytics.
* Recupere o esquema de pesquisa no espaço de trabalho Log Analytics.
* Liste pacotes de monitoramento no espaço de trabalho Log Analytics.
* Recuperar e executar pesquisas salvas no espaço de trabalho Log Analytics.
* Recupere a configuração de armazenamento do espaço de trabalho Log Analytics.

> [!NOTE]
> Essa função não concede acesso de leitura aos dados de log que foram transmitidos para um hub de eventos ou armazenados em uma conta de armazenamento. [Veja abaixo](#security-considerations-for-monitoring-data) para obter informações sobre como configurar o acesso a esses recursos.
> 
> 

### <a name="monitoring-contributor"></a>Colaborador de monitoramento
Pessoas atribuídas à função de Colaborador de monitoramento podem exibir todos os dados de monitoramento em uma assinatura e criar ou modificar as configurações de monitoramento, mas não podem modificar outros recursos. Essa função é um superconjunto da função Leitor de monitoramento e é apropriada para membros de uma equipe de monitoramento ou provedores de serviços gerenciados de uma organização que, além das permissões acima, também precisam ser capazes de:

* Publicra os painéis de monitoramentos como um painel compartilhado.
* Definir [configurações de diagnóstico](diagnostic-settings.md) para um recurso.\*
* Defina o [perfil de log](activity-log-export.md) para uma assinatura.\*
* Defina a atividade das regras de alerta e as configurações por meio dos [Alertas do Azure](alerts-overview.md).
* Criar testes Web e componentes do Application Insights.
* Listar as chaves compartilhadas do espaço de trabalho do Log Analytics.
* Habilite ou desabilite pacotes de monitoramento no espaço de trabalho Log Analytics.
* Crie e exclua e execute pesquisas salvas no espaço de trabalho Log Analytics.
* Crie e exclua a configuração de armazenamento Log Analytics espaço de trabalho.

\*usuário também deve receber a permissão ListKeys no recurso de destino (conta de armazenamento ou namespace do hub de eventos) para definir um perfil de log ou uma configuração de diagnóstico.

> [!NOTE]
> Essa função não concede acesso de leitura aos dados de log que foram transmitidos para um hub de eventos ou armazenados em uma conta de armazenamento. [Veja abaixo](#security-considerations-for-monitoring-data) para obter informações sobre como configurar o acesso a esses recursos.
> 
> 

## <a name="monitoring-permissions-and-custom-rbac-roles"></a>Monitoramento de permissões e funções RBAC personalizadas
Se as funções internas acima não atenderem às necessidades exatas de sua equipe, você poderá [criar uma função personalizada de RBAC](../../role-based-access-control/custom-roles.md) com permissões mais granulares. A seguir estão as operações RBAC do Azure Monitor e suas descrições.

| Operação | Descrição |
| --- | --- |
| Microsoft.Insights/ActionGroups/[Read, Write, Delete] |Grupos de ações de leitura/gravação/exclusão. |
| Microsoft.Insights/ActivityLogAlerts/[Read, Write, Delete] |Alertas de log de atividades de leitura/gravação/exclusão. |
| Microsoft.Insights/AlertRules/[Read, Write, Delete] |Regras de alerta de leitura/gravação/exclusão (dos alertas clássicos). |
| Microsoft.Insights/AlertRules/Incidents/Read |Listar incidentes (histórico da regra de alerta disparado) para regras de alerta. Isso se aplica somente ao portal. |
| Microsoft.Insights/AutoscaleSettings/[Read, Write, Delete] |Leitura/gravação/exclusão de configurações de dimensionamento automático. |
| Microsoft.Insights/DiagnosticSettings/[Read, Write, Delete] |Leitura/gravação/exclusão de configurações de diagnóstico. |
| Microsoft.Insights/EventCategories/Read |Enumere todas as categorias possíveis no Log de Atividades. Usado pelo portal do Azure. |
| Microsoft.Insights/eventtypes/digestevents/Read |Essa permissão é necessária para usuários que precisam de acesso aos Logs de atividade por meio do portal. |
| Microsoft.Insights/eventtypes/values/Read |Listar eventos do Log de atividades (eventos de gerenciamento) em um assinatura. Essa permissão é aplicável ao acesso programático e ao portal para o Log de atividades. |
| Microsoft.Insights/ExtendedDiagnosticSettings/[Read, Write, Delete] | Ler/gravar/excluir configurações de diagnóstico para logs de fluxo de rede. |
| Microsoft.Insights/LogDefinitions/Read |Essa permissão é necessária para usuários que precisam de acesso aos Logs de atividade por meio do portal. |
| Microsoft.Insights/LogProfiles/[Read, Write, Delete] |Perfis de log de leitura/gravação/exclusão (Log de Atividades de streaming para hub de eventos ou conta de armazenamento). |
| Microsoft.Insights/MetricAlerts/[Read, Write, Delete] |Ler/gravar/excluir alertas de métrica quase em tempo real |
| Microsoft.Insights/MetricDefinitions/Read |Ler definições de métricas (lista de tipos de métrica disponíveis para um recurso). |
| Microsoft.Insights/Metrics/Read |Ler as métricas para um recurso. |
| Microsoft.Insights/Register/Action |Registrar o provedor de recursos do Azure Monitor. |
| Microsoft.Insights/ScheduledQueryRules/[Read, Write, Delete] |Alertas do log de leitura/gravação/exclusão no Azure Monitor. |



> [!NOTE]
> O acesso a alertas, configurações de diagnóstico e métricas para um recurso requer que o usuário tenha acesso de Leitura ao tipo de recurso e ao escopo do recurso. Criar (“gravar”) uma configuração de diagnóstico ou perfil de log que faz o arquivamento para uma conta de armazenamento ou transmissão para hubs de eventos requer que o usuário também tenha uma permissão ListKeys no recurso de destino.
> 
> 

Por exemplo, usando a tabela acima, você pode criar uma função RBAC personalizada para um "Leitor de Log de atividades" como esta:

```powershell
$role = Get-AzRoleDefinition "Reader"
$role.Id = $null
$role.Name = "Activity Log Reader"
$role.Description = "Can view activity logs."
$role.Actions.Clear()
$role.Actions.Add("Microsoft.Insights/eventtypes/*")
$role.AssignableScopes.Clear()
$role.AssignableScopes.Add("/subscriptions/mySubscription")
New-AzRoleDefinition -Role $role 
```

## <a name="security-considerations-for-monitoring-data"></a>Considerações de segurança para dados de monitoramento
Dados de monitoramento — especialmente os arquivos de log — podem conter informações confidenciais, como endereços IP ou nomes de usuário. Os dados de monitoramento do Azure vêm em três formas básicas:

1. O Log de atividades, que descreve todas as ações do plano de controle na sua assinatura do Azure.
2. logs de recursos, que são logs emitidos por um recurso.
3. Métricas, que são emitidas pelos recursos.

Todos esses três tipos de dados podem ser armazenados em uma conta de armazenamento ou transmitidos ao Hub de eventos, que são recursos do Azure para fins gerais. Como esses são recursos de finalidade geral, criar, excluir e acessá-los são operações privilegiadas, reservadas para o administrador. Sugerimos que você use as seguintes práticas para recursos relacionados ao monitoramento para evitar o uso indevido:

* Use uma conta de armazenamento única e dedicada para os dados de monitoramento. Se você precisar separar dados de monitoramento em várias contas de armazenamento, nunca compartilhe o uso de uma conta de armazenamento entre dados de monitoramento e não de monitoramento, pois isso pode, inadvertidamente, dar a quem precisa apenas de acesso aos dados de monitoramento (por exemplo, uma solução SIEM de terceiros) acesso aos dados que não são de monitoramento.
* Use um namespace único e dedicado de Barramento de Serviço ou Hub de Eventos em todas as configurações de diagnóstico pelo mesmo motivo que o citado acima.
* Limite o acesso às contas de armazenamento ou hubs de eventos relacionados ao monitoramento mantendo os mesmos em um grupo de recursos separado e [use o escopo](../../role-based-access-control/overview.md#scope) nas suas funções de monitoramento para limitar o acesso somente ao grupo de recursos.
* Não conceda a permissão de ListKeys para contas de armazenamento ou hubs de eventos no escopo de assinatura quando um usuário precisa apenas de acesso aos dados de monitoramento. Em vez disso, conceda essas permissões ao usuário a um escopo de recurso ou grupo de recursos (se você tiver um grupo de recursos de monitoramento dedicado).

### <a name="limiting-access-to-monitoring-related-storage-accounts"></a>Limitando o acesso a contas de armazenamento relacionados ao monitoramento
Quando um usuário ou aplicativo precisa de acesso aos dados de monitoramento em uma conta de armazenamento, você deve [gerar uma SAS da conta](https://msdn.microsoft.com/library/azure/mt584140.aspx) na conta de armazenamento que contém os dados de monitoramento com acesso somente leitura no nível de serviço para armazenamento de blobs. No PowerShell, isso pode ser semelhante a:

```powershell
$context = New-AzStorageContext -ConnectionString "[connection string for your monitoring Storage Account]"
$token = New-AzStorageAccountSASToken -ResourceType Service -Service Blob -Permission "rl" -Context $context
```

Você pode fornecer o token para a entidade que precisa ler da conta de armazenamento e ele pode listar e ler todos os blobs na conta de armazenamento.

Como alternativa, se você precisar controlar essa permissão com o RBAC, você pode conceder a essa entidade a permissão Microsoft.Storage/storageAccounts/listkeys/action nessa conta de armazenamento específica. Isso é necessário para os usuários que precisam ser capazes de definir uma configuração de diagnóstico ou de perfil de log para arquivar em uma conta de armazenamento. Por exemplo, você pode criar a seguinte função RBAC personalizada para um usuário ou aplicativo que precisa apenas da leitura de uma conta de armazenamento:

```powershell
$role = Get-AzRoleDefinition "Reader"
$role.Id = $null
$role.Name = "Monitoring Storage Account Reader"
$role.Description = "Can get the storage account keys for a monitoring storage account."
$role.Actions.Clear()
$role.Actions.Add("Microsoft.Storage/storageAccounts/listkeys/action")
$role.Actions.Add("Microsoft.Storage/storageAccounts/Read")
$role.AssignableScopes.Clear()
$role.AssignableScopes.Add("/subscriptions/mySubscription/resourceGroups/myResourceGroup/providers/Microsoft.Storage/storageAccounts/myMonitoringStorageAccount")
New-AzRoleDefinition -Role $role 
```

> [!WARNING]
> A permissão de ListKeys permite que o usuário liste as chaves primárias e secundárias da conta de armazenamento. Essas chaves concedem ao usuário todas as permissões assinadas (ler, gravar, criar blobs, excluir blobs, etc.) em todos os serviços assinados (blob, fila, tabela, arquivo) na conta de armazenamento. Recomendamos usar uma SAS da conta descrita acima, quando possível.
> 
> 

### <a name="limiting-access-to-monitoring-related-event-hubs"></a>Limitando o acesso aos hubs de eventos relacionados ao monitoramento
Um padrão semelhante pode ser seguido com hubs de eventos, mas primeiro você precisa criar uma regra de autorização de escuta dedicada. Se você deseja conceder acesso a um aplicativo que precisa apenas escutar os hubs de eventos relacionados ao monitoramento, faça o seguinte:

1. Crie uma política de acesso compartilhado no(s) hubs de eventos que foram criados para transmitir dados de monitoramento com somente declarações de escuta. Isso pode ser feito no portal. Por exemplo, você pode chamá-la de "monitoringReadOnly." Se possível, dê essa chave diretamente ao consumidor e ignore a próxima etapa.
2. Se o consumidor precisar ser capaz de obter a chave ad hoc, conceda ao usuário a ação ListKeys para o Hub de eventos. Isso é necessário também para os usuários que precisam ser capazes de definir uma configuração de diagnóstico ou de perfil de log para transmissão para hubs de eventos. Por exemplo, você pode criar uma regra RBAC:
   
   ```powershell
   $role = Get-AzRoleDefinition "Reader"
   $role.Id = $null
   $role.Name = "Monitoring Event Hub Listener"
   $role.Description = "Can get the key to listen to an event hub streaming monitoring data."
   $role.Actions.Clear()
   $role.Actions.Add("Microsoft.ServiceBus/namespaces/authorizationrules/listkeys/action")
   $role.Actions.Add("Microsoft.ServiceBus/namespaces/Read")
   $role.AssignableScopes.Clear()
   $role.AssignableScopes.Add("/subscriptions/mySubscription/resourceGroups/myResourceGroup/providers/Microsoft.ServiceBus/namespaces/mySBNameSpace")
   New-AzRoleDefinition -Role $role 
   ```

## <a name="monitoring-within-a-secured-virtual-network"></a>Monitoramento dentro de uma Rede Virtual protegida

O Azure Monitor precisa de acesso aos seus recursos do Azure para fornecer os serviços que você habilita. Se você quiser monitorar seus recursos do Azure enquanto ainda os protege contra o acesso à Internet pública, você pode habilitar as configurações a seguir.

### <a name="secured-storage-accounts"></a>Contas de Armazenamento protegidas 

Os dados de monitoramento costumam ser gravados em uma conta de armazenamento. Convém verificar se os dados copiados para uma Conta de Armazenamento não podem ser acessados por usuários não autorizados. Para obter mais segurança, bloqueie o acesso à rede para permitir que somente os recursos autorizados e os serviços de confiança da Microsoft acessem uma conta de armazenamento restringindo uma conta de armazenamento para usar "redes selecionadas".
![](./media/roles-permissions-security/secured-storage-example.png) de diálogo Configurações do armazenamento do Azure Azure Monitor é considerado um destes "serviços confiáveis da Microsoft" se você permitir que serviços da Microsoft confiáveis acessem seu armazenamento protegido, o Azure monitor terá acesso à sua conta de armazenamento protegido; habilitar a gravação Azure Monitor logs de recursos, o log de atividades e as métricas para sua conta de armazenamento nessas condições protegidas. Isso também permitirá que o Log Analytics leia os logs do armazenamento protegido.   


Para saber mais, confira [Segurança de rede e Armazenamento do Azure](../../storage/common/storage-network-security.md)

## <a name="next-steps"></a>{1&gt;{2&gt;Próximas etapas&lt;2}&lt;1}
* [Leia sobre RBAC e permissões no Gerenciador de Recursos](../../role-based-access-control/overview.md)
* [Leia a visão geral do monitoramento no Azure](../../azure-monitor/overview.md)



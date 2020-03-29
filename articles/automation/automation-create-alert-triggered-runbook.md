---
title: Usar um alerta para disparar um runbook de Automação do Azure
description: Saiba como disparar um runbook para executar quando um alerta do Azure é acionado.
services: automation
ms.subservice: process-automation
ms.date: 04/29/2019
ms.topic: conceptual
ms.openlocfilehash: df28116c588ed77f02c78a42a85feb91ca339e7b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75366693"
---
# <a name="use-an-alert-to-trigger-an-azure-automation-runbook"></a>Usar um alerta para disparar um runbook de Automação do Azure

Você pode usar o [Azure Monitor](../azure-monitor/overview.md?toc=%2fazure%2fautomation%2ftoc.json) para monitorar logs e métricas de nível básico para a maioria dos serviços no Azure. Você pode chamar os runbooks da Automação do Azure usando [grupos de ações](../azure-monitor/platform/action-groups.md?toc=%2fazure%2fautomation%2ftoc.json) ou usando alertas clássicos para automatizar tarefas com base em alertas. Este artigo mostra como configurar e executar um runbook usando alertas.

## <a name="alert-types"></a>Tipos de alerta

Você pode usar runbooks de automação com três tipos de alerta:

* Alertas comuns
* Alertas do log de atividades
* Alertas de métrica quase em tempo real

> [!NOTE]
> O esquema de alerta comum padroniza a experiência de consumo para notificações de alerta no Azure hoje. Historicamente, os três tipos de alerta no Azure hoje (métrica, log e registro de atividades) tiveram seus próprios modelos de e-mail, schemas webhook, etc. Para saber mais, consulte [O esquema de alerta comum](../azure-monitor/platform/alerts-common-schema.md)

Quando um alerta chama um runbook, a chamada real é uma solicitação HTTP POST para o webhook. O corpo da solicitação POST contém um objeto com o formato JSON que tem propriedades úteis relacionadas ao alerta. A tabela a seguir lista links para o esquema de conteúdo para cada tipo de alerta:

|Alerta  |Descrição|Esquema de conteúdo  |
|---------|---------|---------|
|[Alerta comum](../azure-monitor/platform/alerts-common-schema.md?toc=%2fazure%2fautomation%2ftoc.json)|O esquema de alerta comum que padroniza a experiência de consumo para notificações de alerta no Azure hoje.|Esquema de carga de alerta comum|
|[Alerta do log de atividades](../azure-monitor/platform/activity-log-alerts.md?toc=%2fazure%2fautomation%2ftoc.json)    |Envia uma notificação quando qualquer novo evento no log de atividades do Azure corresponde a condições específicas. Por exemplo, quando uma operação `Delete VM` ocorre em **myProductionResourceGroup** ou quando um novo evento da Integridade do Serviço do Azure com um status **Ativo** é exibido.| [Esquema de conteúdo de alerta de log de atividades](../azure-monitor/platform/activity-log-alerts-webhook.md)        |
|[Alertas de métrica quase em tempo real](../azure-monitor/platform/alerts-metric-near-real-time.md?toc=%2fazure%2fautomation%2ftoc.json)    |Envia uma notificação mais rápido do que alertas de métrica quando uma ou mais métricas de nível de plataforma atendem às condições especificadas. Por exemplo, quando o valor de **% de CPU** em uma VM é maior que **90**e o valor de **Entrada de Rede** é maior do que **500 MB** para os últimos 5 minutos.| [Esquema de conteúdo de alerta de métrica quase em tempo real](../azure-monitor/platform/alerts-webhooks.md#payload-schema)          |

Já que os dados fornecidos por cada alerta são diferentes, cada tipo de alerta é tratado de forma diferente. Na próxima seção, você aprenderá a criar um runbook para lidar com diferentes tipos de alertas.

## <a name="create-a-runbook-to-handle-alerts"></a>Criar um runbook para tratar alertas

Para usar a Automação com alertas, será necessário um runbook que tenha lógica que gerencie o conteúdo JSON de alerta que é passado ao runbook. O seguinte exemplo de runbook deverá ser chamado a partir de um alerta do Azure.

Conforme descrito na seção anterior, cada tipo de alerta tem um esquema diferente. O script aceita os dados de webhook no parâmetro de entrada do runbook `WebhookData` de um alerta. Em seguida, o script avalia o conteúdo de JSON para determinar qual tipo de alerta foi usado.

Este exemplo usa um alerta de uma VM. Ele recupera os dados da VM do conteúdo e utiliza essas informações para interromper a VM. A conexão deve ser configurada na conta de Automação em que o runbook é executado. Ao usar alertas para disparar runbooks, é importante verificar o status do alerta no runbook que é disparado. O runbook vai disparar sempre que o alerta mudar de estado. Alertas têm vários estados. Os dois estados mais comuns são `Activated` e `Resolved`. Verifique se há esse estado em sua lógica de runbook para garantir que seu runbook não seja executado mais de uma vez. O exemplo neste artigo mostra como procurar alertas `Activated` somente.

O runbook usa o **AzureRunAsConnection** [Executar como conta](automation-create-runas-account.md) para se autenticar com o Azure para executar a ação de gerenciamento na VM.

Use este exemplo para criar um runbook chamado **Stop-AzureVmInResponsetoVMAlert**. Você pode modificar o script do PowerShell e usá-lo com muitos recursos diferentes.

1. Vá para sua conta da Automação do Azure.
2. Em **Automação de Processo**, selecione **Runbooks**.
3. Na parte superior da lista de runbooks, selecione **+ Criar um runbook**.
4. Na página **Adicionar runbook,** **digite Stop-AzureVmInResponsetoVMAlert** para obter o nome do runbook. Para o tipo de runbook, selecione **PowerShell**. Em seguida, selecione **Criar**.  
5. Copie o exemplo do PowerShell a seguir para a página **Editar**.

    ```powershell-interactive
    [OutputType("PSAzureOperationResponse")]
    param
    (
        [Parameter (Mandatory=$false)]
        [object] $WebhookData
    )
    $ErrorActionPreference = "stop"

    if ($WebhookData)
    {
        # Get the data object from WebhookData
        $WebhookBody = (ConvertFrom-Json -InputObject $WebhookData.RequestBody)

        # Get the info needed to identify the VM (depends on the payload schema)
        $schemaId = $WebhookBody.schemaId
        Write-Verbose "schemaId: $schemaId" -Verbose
        if ($schemaId -eq "azureMonitorCommonAlertSchema") {
            # This is the common Metric Alert schema (released March 2019)
            $Essentials = [object] ($WebhookBody.data).essentials
            # Get the first target only as this script doesn't handle multiple
            $alertTargetIdArray = (($Essentials.alertTargetIds)[0]).Split("/")
            $SubId = ($alertTargetIdArray)[2]
            $ResourceGroupName = ($alertTargetIdArray)[4]
            $ResourceType = ($alertTargetIdArray)[6] + "/" + ($alertTargetIdArray)[7]
            $ResourceName = ($alertTargetIdArray)[-1]
            $status = $Essentials.monitorCondition
        }
        elseif ($schemaId -eq "AzureMonitorMetricAlert") {
            # This is the near-real-time Metric Alert schema
            $AlertContext = [object] ($WebhookBody.data).context
            $SubId = $AlertContext.subscriptionId
            $ResourceGroupName = $AlertContext.resourceGroupName
            $ResourceType = $AlertContext.resourceType
            $ResourceName = $AlertContext.resourceName
            $status = ($WebhookBody.data).status
        }
        elseif ($schemaId -eq "Microsoft.Insights/activityLogs") {
            # This is the Activity Log Alert schema
            $AlertContext = [object] (($WebhookBody.data).context).activityLog
            $SubId = $AlertContext.subscriptionId
            $ResourceGroupName = $AlertContext.resourceGroupName
            $ResourceType = $AlertContext.resourceType
            $ResourceName = (($AlertContext.resourceId).Split("/"))[-1]
            $status = ($WebhookBody.data).status
        }
        elseif ($schemaId -eq $null) {
            # This is the original Metric Alert schema
            $AlertContext = [object] $WebhookBody.context
            $SubId = $AlertContext.subscriptionId
            $ResourceGroupName = $AlertContext.resourceGroupName
            $ResourceType = $AlertContext.resourceType
            $ResourceName = $AlertContext.resourceName
            $status = $WebhookBody.status
        }
        else {
            # Schema not supported
            Write-Error "The alert data schema - $schemaId - is not supported."
        }

        Write-Verbose "status: $status" -Verbose
        if (($status -eq "Activated") -or ($status -eq "Fired"))
        {
            Write-Verbose "resourceType: $ResourceType" -Verbose
            Write-Verbose "resourceName: $ResourceName" -Verbose
            Write-Verbose "resourceGroupName: $ResourceGroupName" -Verbose
            Write-Verbose "subscriptionId: $SubId" -Verbose

            # Determine code path depending on the resourceType
            if ($ResourceType -eq "Microsoft.Compute/virtualMachines")
            {
                # This is an Resource Manager VM
                Write-Verbose "This is an Resource Manager VM." -Verbose

                # Authenticate to Azure with service principal and certificate and set subscription
                Write-Verbose "Authenticating to Azure with service principal and certificate" -Verbose
                $ConnectionAssetName = "AzureRunAsConnection"
                Write-Verbose "Get connection asset: $ConnectionAssetName" -Verbose
                $Conn = Get-AutomationConnection -Name $ConnectionAssetName
                if ($Conn -eq $null)
                {
                    throw "Could not retrieve connection asset: $ConnectionAssetName. Check that this asset exists in the Automation account."
                }
                Write-Verbose "Authenticating to Azure with service principal." -Verbose
                Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint | Write-Verbose
                Write-Verbose "Setting subscription to work against: $SubId" -Verbose
                Set-AzureRmContext -SubscriptionId $SubId -ErrorAction Stop | Write-Verbose

                # Stop the Resource Manager VM
                Write-Verbose "Stopping the VM - $ResourceName - in resource group - $ResourceGroupName -" -Verbose
                Stop-AzureRmVM -Name $ResourceName -ResourceGroupName $ResourceGroupName -Force
                # [OutputType(PSAzureOperationResponse")]
            }
            else {
                # ResourceType not supported
                Write-Error "$ResourceType is not a supported resource type for this runbook."
            }
        }
        else {
            # The alert status was not 'Activated' or 'Fired' so no action taken
            Write-Verbose ("No action taken. Alert status: " + $status) -Verbose
        }
    }
    else {
        # Error
        Write-Error "This runbook is meant to be started from an Azure alert webhook only."
    }
    ```

6. Selecione **Publicar** para salvar e publicar o runbook.

## <a name="create-the-alert"></a>Crie o alerta

Os alertas utilizam grupos de ação, que são coleções de ações que são acionadas pelo alerta. Runbooks são apenas uma das muitas ações que você pode usar com grupos de ações.

1. Em sua conta de automação, selecione **Alertas** em **Monitoramento**.
1. Selecione **+ Nova regra de alerta**.
1. Clique **em Selecionar** em **Recurso**. Na **página Selecionar um recurso,** selecione sua VM para alertar e clique **em Feito**.
1. Clique **em Adicionar condição** em **Condição**. Selecione o sinal que deseja usar, por **exemplo, CPU porcentagem** e clique **em Feito**.
1. Na página **Configurar lógica de sinal,** digite seu **valor limite** em lógica **de alerta**e clique em **Feito**.
1. Em **grupos de ação,** selecione **Criar novo**.
1. Na página **do grupo de ação Adicionar,** dê ao seu grupo de ação um nome e um nome curto.
1. Dê um nome à ação. Para o tipo de ação, selecione **Manual de Automação**.
1. Selecione **Editar Detalhes**. Na página **Configurar Runbook**, em **Origem do runbook**, selecione **Usuário**.  
1. Selecione sua **Assinatura** e **Conta de automação** e selecione o runbook **Stop-AzureVmInResponsetoVMAlert**.  
1. Selecione **Sim** para **Ativar o esquema de alerta comum**.
1. Selecione **OK** para criar o grupo de ações.

    ![Adicionar página do grupo de ações](./media/automation-create-alert-triggered-runbook/add-action-group.png)

    Você pode usar este grupo de ações nos [alertas de log de atividades](../azure-monitor/platform/activity-log-alerts.md?toc=%2fazure%2fautomation%2ftoc.json) e [alertas quase em tempo real](../azure-monitor/platform/alerts-overview.md?toc=%2fazure%2fautomation%2ftoc.json) que você criar.

1. Em **Detalhes de alerta,** adicione um nome e uma descrição da regra de alerta e clique **em Criar regra de alerta**.

## <a name="next-steps"></a>Próximas etapas

* Para obter mais informações sobre como iniciar um runbook da Automação usando um webhook, veja [Iniciar um runbook com base em um webhook](automation-webhooks.md).
* Para obter detalhes sobre diferentes maneiras de iniciar um runbook, consulte [Como iniciar um runbook](automation-starting-a-runbook.md).
* Para saber como criar um alerta de registro de atividades, consulte [Criar alertas de registro de atividades](../azure-monitor/platform/activity-log-alerts.md?toc=%2fazure%2fautomation%2ftoc.json).
* Para saber como criar um alerta quase em tempo real, veja [Criar uma regra de alerta no Portal do Azure](../azure-monitor/platform/alerts-metric.md?toc=/azure/azure-monitor/toc.json).

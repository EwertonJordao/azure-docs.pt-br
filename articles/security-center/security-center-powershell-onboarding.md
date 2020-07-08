---
title: Integração à central de segurança do Azure com o PowerShell
description: Este documento orienta você pelo processo de integração da Central de Segurança do Azure usando os cmdlets do PowerShell.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: e400fcbf-f0a8-4e10-b571-5a0d0c3d0c67
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/02/2018
ms.author: memildin
ms.openlocfilehash: 0ca5cdcb0410d52f40e28c66a839bddcb34cc8a8
ms.sourcegitcommit: 845a55e6c391c79d2c1585ac1625ea7dc953ea89
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2020
ms.locfileid: "85963352"
---
# <a name="automate-onboarding-of-azure-security-center-using-powershell"></a>Automatizar integração da Central de Segurança do Azure usando o PowerShell

Você pode proteger suas cargas de trabalho do Azure programaticamente usando o módulo do PowerShell da Central de Segurança do Azure.
O uso do PowerShell permite automatizar tarefas e evitar o erro humano, que ocorrem com as tarefas manuais. Isso é especialmente útil em implantações em grande escala que envolvem dezenas de assinaturas com centenas e milhares de recursos. Tudo isso deve ser protegido desde o início.

A integração da Central de Segurança do Azure através do PowerShell permite que você automatize programaticamente a integração e o gerenciamento de seus recursos do Azure e adicione os controles de segurança necessários.

Este artigo fornece um exemplo de script do PowerShell que pode ser modificado e usado em seu ambiente para implantar a Central de Segurança em suas assinaturas. 

Neste exemplo, habilitaremos a Central de Segurança em uma assinatura com a ID: d07c0080-170c-4c24-861d-9c817742786c e aplicaremos as configurações recomendadas que fornecem um alto nível de proteção, com a implementação da camada Padrão da Central de Segurança que oferece proteção avançada contra ameaças e recursos de detecção de ameaças:

1. Defina o [nível de proteção padrão da central de segurança](https://azure.microsoft.com/pricing/details/security-center/). 
 
2. Defina o espaço de trabalho Log Analytics ao qual o agente de Log Analytics enviará os dados coletados nas VMs associadas à assinatura – neste exemplo, um espaço de trabalho definido pelo usuário existente (MyWorkspace).

3. Ative o provisionamento automático de agente da central de segurança, que [implanta o agente de log Analytics](security-center-enable-data-collection.md#auto-provision-mma).

5. Defina o ciso da organização [como o contato de segurança para alertas da central de segurança e eventos notáveis](security-center-provide-security-contact-details.md).

6. Atribua as [políticas de segurança padrão da](tutorial-security-policy.md) Central de Segurança.

## <a name="prerequisites"></a>Pré-requisitos

Essas etapas devem ser realizadas antes de executar os cmdlets da Central de Segurança:

1. Execute o PowerShell como administrador.

1. Execute os seguintes comandos no PowerShell:
      
    ```Set-ExecutionPolicy -ExecutionPolicy AllSigned```

    ```Install-Module -Name Az.Security -Force```

## <a name="onboard-security-center-using-powershell"></a>Integrar a Central de Segurança usando o PowerShell

1. Registre suas assinaturas no Provedor de recursos da Central de Segurança:

    ```Set-AzContext -Subscription "d07c0080-170c-4c24-861d-9c817742786c"```

    ```Register-AzResourceProvider -ProviderNamespace 'Microsoft.Security'```

1. Opcional: defina o nível de cobertura (tipo de preço) das assinaturas (se não estiver definido, o tipo de preço será definido como Gratuito):

    ```Set-AzContext -Subscription "d07c0080-170c-4c24-861d-9c817742786c"```

    ```Set-AzSecurityPricing -Name "default" -PricingTier "Standard"```

1. Configure um espaço de trabalho do Log Analytics no qual os agentes irão reportar. Você deve ter um espaço de trabalho do Log Analytics já criado ao qual as VMs da assinatura se reportarão. Você pode definir várias assinaturas para reportar ao mesmo workspace. Se não estiver definido, o workspace padrão será usado.

    ```Set-AzSecurityWorkspaceSetting -Name "default" -Scope "/subscriptions/d07c0080-170c-4c24-861d-9c817742786c" -WorkspaceId"/subscriptions/d07c0080-170c-4c24-861d-9c817742786c/resourceGroups/myRg/providers/Microsoft.OperationalInsights/workspaces/myWorkspace"```

1. Instalação de provisionamento automático do agente de Log Analytics em suas VMs do Azure:
    
    ```Set-AzContext -Subscription "d07c0080-170c-4c24-861d-9c817742786c"```
    
    ```Set-AzSecurityAutoProvisioningSetting -Name "default" -EnableAutoProvision```

    > [!NOTE]
    > É recomendável habilitar o provisionamento automático para garantir que suas máquinas virtuais do Azure sejam automaticamente protegidas pela Central de Segurança do Azure.
    >

1. Opcional: é altamente recomendável que você defina os detalhes do contato de segurança para as assinaturas que você integrou. Eles serão usados como destinatários de alertas e notificações gerados pela Central de Segurança:

    ```Set-AzSecurityContact -Name "default1" -Email "CISO@my-org.com" -Phone "2142754038" -AlertAdmin -NotifyOnAlert```

1. Atribua a iniciativa de política padrão da Central de Segurança:

    ```Register-AzResourceProvider -ProviderNamespace 'Microsoft.PolicyInsights'```

    ```$Policy = Get-AzPolicySetDefinition | where {$_.Properties.displayName -EQ 'Enable Monitoring in Azure Security Center'} New-AzPolicyAssignment -Name 'ASC Default <d07c0080-170c-4c24-861d-9c817742786c>' -DisplayName 'Security Center Default <subscription ID>' -PolicySetDefinition $Policy -Scope '/subscriptions/d07c0080-170c-4c24-861d-9c817742786c'```

Você integrou com êxito a central de segurança do Azure com o PowerShell.

Agora você pode usar esses cmdlets do PowerShell com scripts de automação para iterar programaticamente entre assinaturas e recursos. Isso economiza tempo e reduz a probabilidade de erro humano. Você pode usar este [exemplo de script](https://github.com/Microsoft/Azure-Security-Center/blob/master/quickstarts/ASC-Samples.ps1) como referência.




## <a name="see-also"></a>Consulte também
Para saber mais sobre como você pode usar o PowerShell para automatizar a integração à Central de Segurança, confira o artigo a seguir:

* [AZ. Security](https://docs.microsoft.com/powershell/module/az.security)

Para saber mais sobre a Central de Segurança, confira o seguinte artigo:

* [Configurando políticas de segurança na central de segurança do Azure](tutorial-security-policy.md) – saiba como configurar políticas de segurança para suas assinaturas e grupos de recursos do Azure.
* [Gerenciando e respondendo a alertas de segurança na central de segurança do Azure](security-center-managing-and-responding-alerts.md) – saiba como gerenciar e responder a alertas de segurança.
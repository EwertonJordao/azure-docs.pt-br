---
title: Configurar o consentimento do proprietário do grupo para aplicativos que acessam dados de grupo usando o Azure AD
description: Saiba como gerenciar se os proprietários de grupo e de equipe podem consentir com os aplicativos que terão acesso aos dados do grupo ou da equipe.
services: active-directory
author: kenwith
manager: celestedg
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: how-to
ms.date: 05/19/2020
ms.author: kenwith
ms.reviewer: arvindh, luleon, phsignor
ms.openlocfilehash: e590981fabcd20f23f25d12b4176b6730cb0fc3c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91804284"
---
# <a name="configure-group-owner-consent-to-apps-accessing-group-data"></a>Configurar o consentimento do proprietário do grupo em aplicativos que acessam dados do grupo

Os proprietários de grupos e equipes podem autorizar aplicativos, como aplicativos publicados por fornecedores terceirizados, para acessar os dados da sua organização associados a um grupo. Por exemplo, um proprietário de equipe no Microsoft Teams pode permitir que um aplicativo leia todas as mensagens do Teams na equipe ou liste o perfil básico dos membros de um grupo. Consulte [consentimento específico de recursos no Microsoft Teams](https://docs.microsoft.com/microsoftteams/resource-specific-consent) para saber mais.

## <a name="manage-group-owner-consent-to-apps"></a>Gerenciar consentimento do proprietário do grupo para aplicativos

Você pode configurar quais usuários têm permissão para consentir com os aplicativos que acessam os dados de seus grupos ou de equipes, ou você pode desabilitar isso para todos os usuários.

# <a name="portal"></a>[Portal](#tab/azure-portal)

Siga estas etapas para gerenciar o consentimento do proprietário do grupo para os aplicativos que acessam os dados do Grupo:

1. Entre no [portal do Azure](https://portal.azure.com) como um [Administrador Global](../users-groups-roles/directory-assign-admin-roles.md#global-administrator--company-administrator).
2. Selecione **Azure Active Directory** > **Aplicativos empresariais** > **Consentimento e permissões** > **Configurações de consentimento do usuário**.
3. Em **Consentimento do proprietário do grupo em aplicativos que acessam dados**, selecione a opção que deseja habilitar.
4. Selecione **salvar** para salvar suas configurações.

Neste exemplo, todos os proprietários de grupo têm permissão para dar consentimento aos aplicativos que acessam os dados de seus grupos:

:::image type="content" source="media/configure-user-consent-groups/group-owner-consent.png" alt-text="Configurações de consentimento do proprietário do grupo":::

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Você pode usar o módulo de versão prévia do PowerShell do Azure AD, [AzureADPreview](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0-preview&preserve-view=true), para habilitar ou desabilitar a capacidade dos proprietários do grupo de dar consentimento a aplicativos que acessam os dados da sua organização nos grupos que eles possuem.

1. Verifique se você está usando o módulo [AzureADPreview](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0-preview&preserve-view=true). Esta etapa será importante se você tiver instalado os módulos [AzureAD](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0&preserve-view=true) e [AzureADPreview](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0-preview&preserve-view=true)).

    ```powershell
    Remove-Module AzureAD
    Import-Module AzureADPreview
    ```

1. Conecte-se ao Azure AD PowerShell.

   ```powershell
   Connect-AzureAD
   ```

1. Recupere o valor atual das configurações do diretório **Configurações de Política de Consentimento** em seu locatário. Isso exige verificar se as configurações de diretório para esse recurso foram criadas e, se não, usar os valores do modelo de configurações de diretório correspondente.

    ```powershell
    $consentSettingsTemplateId = "dffd5d46-495d-40a9-8e21-954ff55e198a" # Consent Policy Settings
    $settings = Get-AzureADDirectorySetting -All $true | Where-Object { $_.TemplateId -eq $consentSettingsTemplateId }

    if (-not $settings) {
        $template = Get-AzureADDirectorySettingTemplate -Id $consentSettingsTemplateId
        $settings = $template.CreateDirectorySetting()
    }

    $enabledValue = $settings.Values | ? { $_.Name -eq "EnableGroupSpecificConsent" }
    $limitedToValue = $settings.Values | ? { $_.Name -eq "ConstrainGroupSpecificConsentToMembersOfGroupId" }
    ```

1. Entenda os valores de configuração. Há dois valores de configurações que definem quais usuários podem permitir que um aplicativo acesse os dados do seu grupo:

    | Configuração       | Type         | Descrição  |
    | ------------- | ------------ | ------------ |
    | _EnableGroupSpecificConsent_   | Boolean | Sinalizador que indica se os proprietários de grupos têm permissão para conceder permissões específicas do grupo. |
    | _ConstrainGroupSpecificConsentToMembersOfGroupId_ | Guid | Se _EnableGroupSpecificConsent_ for definido como "true" e esse valor for definido como a ID de objeto de um grupo, os membros do grupo identificado terão autorização para conceder permissões específicas do grupo aos grupos que eles possuem. |

1. Atualize os valores das configurações para a configuração desejada:

    ```powershell
    # Disable group-specific consent entirely
    $enabledValue.Value = "False"
    $limitedToValue.Value = ""
    ```

    ```powershell
    # Enable group-specific consent for all users
    $enabledValue.Value = "True"
    $limitedToValue.Value = ""
    ```

    ```powershell
    # Enable group-specific consent for users in a given group
    $enabledValue.Value = "True"
    $limitedToValue.Value = "{group-object-id}"
    ```

1. Salve suas configurações.

    ```powershell
    if ($settings.Id) {
        # Update an existing directory settings
        Set-AzureADDirectorySetting -Id $settings.Id -DirectorySetting $settings
    } else {
        # Create a new directory settings to override the default setting 
        New-AzureADDirectorySetting -DirectorySetting $settings
    }
    ```

---

## <a name="next-steps"></a>Próximas etapas

Para saber mais:

* [Definir configurações de consentimento do usuário](configure-user-consent.md)
* [Configurar o fluxo de trabalho de consentimento do administrador](configure-admin-consent-workflow.md)
* [Saiba como gerenciar o consentimento em aplicativos e avaliar solicitações de consentimento](manage-consent-requests.md)
* [Conceder consentimento de administrador em todo o locatário para um aplicativo](grant-admin-consent.md)
* [Permissões e consentimento na plataforma de identidade da Microsoft](../develop/active-directory-v2-scopes.md)

Para obter ajuda ou encontrar respostas às suas perguntas:
* [Azure AD no StackOverflow](https://stackoverflow.com/questions/tagged/azure-active-directory)

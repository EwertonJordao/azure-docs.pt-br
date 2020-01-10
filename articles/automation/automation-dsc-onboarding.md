---
title: Integrar computadores para gerenciamento por Configuração de Estado da Automação do Azure
description: Como configurar computadores para gerenciamento com a configuração de estado de automação do Azure
services: automation
ms.service: automation
ms.subservice: dsc
author: mgoedtel
ms.author: magoedte
ms.topic: conceptual
ms.date: 12/10/2019
manager: carmonm
ms.openlocfilehash: c5876dd293a97414ff4f48dbb8645e64226a6ba8
ms.sourcegitcommit: f53cd24ca41e878b411d7787bd8aa911da4bc4ec
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/10/2020
ms.locfileid: "75834117"
---
# <a name="onboarding-machines-for-management-by-azure-automation-state-configuration"></a>Integrar computadores para gerenciamento por Configuração de Estado da Automação do Azure

## <a name="why-manage-machines-with-azure-automation-state-configuration"></a>Por que gerenciar máquinas com a Configuração do Estação de Automação do Azure?

A configuração de estado de automação do Azure é um serviço de gerenciamento de configuração para nós de DSC (configuração de estado desejado) em qualquer datacenter local ou na nuvem.
Ela permite a escalabilidade entre milhares de máquinas rápida e facilmente a partir de um local central e seguro.
Você pode, facilmente, integrar máquinas, atribuir a elas configurações declarativas e exibir relatórios que mostram a conformidade de cada computador com o estado desejado especificado.
O serviço de configuração de estado da automação do Azure é para DSC quais runbooks de automação do Azure são para scripts do PowerShell.
Em outras palavras, da mesma forma que a Automação do Azure ajuda você a gerenciar os scripts do PowerShell, ela também ajuda a gerenciar configurações do DSC.
Para saber mais sobre os benefícios de usar a Configuração de Estado de Automação do Azure, consulte [Visão geral da Configuração de Estado de Automação do Azure](automation-dsc-overview.md).

A Configuração do Estado de Automação do Azure pode ser usada para gerenciar uma variedade de máquinas:

- Máquinas virtuais do Azure
- Máquinas virtuais do Azure (clássico)
- Máquinas físicas/virtuais do Windows locais ou em uma nuvem diferente do Azure (incluindo instâncias AWS EC2)
- Máquinas físicas/virtuais locais do Linux, no Azure, ou em uma nuvem diferente do Azure

Além disso, se você não estiver pronto para gerenciar a configuração da máquina a partir da nuvem, a Configuração do Estado de Automação do Azure também poderá ser usado como um ponto de extremidade apenas para relatório.
Isso permite que você defina as configurações (push) por meio da DSC e exiba detalhes de relatório na automação do Azure.

> [!NOTE]
> Gerenciar VMs do Azure com a Configuração do Estado está incluído sem custo adicional, se a extensão de DSC da máquina virtual instalada for maior que 2.70. Para obter mais informações, consulte a [**página de preços de automação**](https://azure.microsoft.com/pricing/details/automation/).

As seções a seguir descrevem como você pode integrar cada tipo de máquina à Configuração do Estado de Automação do Azure.

> [!NOTE]
>A implantação de DSC em um nó do Linux usa a pasta `/tmp` e os módulos como **nxAutomation** são baixados temporariamente para verificação antes de serem instalados em seu local apropriado. Para garantir que os módulos sejam instalados corretamente, o agente do Log Analytics para Linux precisa de permissão de leitura/gravação na pasta `/tmp`. O agente do Log Analytics para Linux é executado como o usuário `omsagent`. 
>
>Para conceder permissão de gravação para `omsagent` usuário, execute os seguintes comandos: `setfacl -m u:omsagent:rwx /tmp`
>

## <a name="azure-virtual-machines"></a>Máquinas virtuais do Azure

A Configuração do Estado de Automação do Azure permite que você integre facilmente máquinas virtuais do Azure para o gerenciamento de configuração, usando o portal do Azure, os modelos do Azure Resource Manager ou o PowerShell. Nos bastidores e sem um administrador precisar acessar remotamente a VM, a extensão de Configuração de Estado Desejado da VM do Azure registra a VM com a Configuração do Estado de Automação do Azure.
Como a extensão da Configuração de Estado Desejado da VM do Azure é executada de forma assíncrona, as etapas para acompanhar seu andamento ou para resolver problemas nele são fornecidas na seção [**Solução de problemas de integração de máquina virtual do Azure**](#troubleshooting-azure-virtual-machine-onboarding) a seguir.

### <a name="azure-portal"></a>Portal do Azure

No [Portal do Azure](https://portal.azure.com/), navegue até a conta da Automação do Azure na qual você deseja carregar as máquinas virtuais. Na página de Configuração Estado e na guia **Nós**, clique em **+ Adicionar**.

Selecione uma máquina virtual do Azure para carregar.

Se o computador não tiver a extensão de estado desejado do PowerShell instalada e o estado de energia estiver em execução, clique em **Conectar**.

Em **Registro**, digite os [valores do Gerenciador de Configuração Local de DSC do PowerShell](/powershell/scripting/dsc/managing-nodes/metaConfig) necessários para o seu caso de uso e, opcionalmente, uma configuração de nó para atribuir à VM.

![integração](./media/automation-dsc-onboarding/DSC_Onboarding_6.png)

### <a name="azure-resource-manager-templates"></a>Modelos do Azure Resource Manager

Máquinas virtuais do Azure podem ser implantadas e integradas à Configuração do Estado de Automação do Azure por meio de modelos do Gerenciador do Azure Resource Manager. Consulte [servidor gerenciado pelo serviço de configuração de estado desejado](https://azure.microsoft.com/resources/templates/101-automation-configuration/) para obter um modelo de exemplo que integra uma VM existente à configuração de estado de automação do Azure.
Se você estiver gerenciando um conjunto de dimensionamento de máquinas virtuais, consulte o modelo de exemplo [configuração de conjunto de dimensionamento de máquinas virtuais gerenciada pela automação do Azure](https://azure.microsoft.com/resources/templates/201-vmss-automation-dsc/).

### <a name="powershell"></a>PowerShell

O cmdlet [Register-AzAutomationDscNode](/powershell/module/az.automation/register-azautomationdscnode) pode ser usado para carregar máquinas virtuais no Azure usando o PowerShell.
No entanto, isso é atualmente implementado apenas para computadores que executam o Windows (o cmdlet dispara apenas a extensão do Windows).

### <a name="registering-virtual-machines-across-azure-subscriptions"></a>Registrando máquinas virtuais em assinaturas do Azure

A melhor maneira de registrar máquinas virtuais de outras assinaturas do Azure é usar a extensão de DSC em um modelo de implantação de Azure Resource Manager.
Os exemplos são fornecidos na [extensão de configuração de estado desejado com modelos de Azure Resource Manager](https://docs.microsoft.com/azure/virtual-machines/extensions/dsc-template).
Para localizar a chave de registro e a URL de registro para usar como parâmetros no modelo, consulte a seguinte seção de [**registro seguro**](#secure-registration) .

## <a name="physicalvirtual-windows-machines-on-premises-or-in-a-cloud-other-than-azure-including-aws-ec2-instances"></a>Máquinas físicas/virtuais do Windows locais ou em uma nuvem diferente do Azure (incluindo instâncias AWS EC2)

Os servidores Windows em execução no local ou em outros ambientes de nuvem também podem ser integrados à configuração de estado da automação do Azure, desde que tenham [acesso de saída ao Azure](automation-dsc-overview.md#network-planning):

1. Verifique se a versão mais recente do [WMF 5](https://aka.ms/wmf5latest) está instalada nos computadores que você deseja integrar à Configuração do Estado de Automação do Azure.
1. Siga as instruções na seção [**Gerando metaconfigurações DSC**](#generating-dsc-metaconfigurations) a seguir para gerar uma pasta com as metaconfigurações de DSC necessárias.
1. Aplique-se remotamente a metaconfiguração do DSC do PowerShell às máquinas que você deseja carregar. **O computador no qual este comando é executado deve ter a versão mais recente do [WMF 5](https://aka.ms/wmf5latest) instalada**:

   ```powershell
   Set-DscLocalConfigurationManager -Path C:\Users\joe\Desktop\DscMetaConfigs -ComputerName MyServer1, MyServer2
   ```

1. Se você não pode aplicar as metaconfigurações do DSC do PowerShell remotamente, copie a pasta de metaconfigurações da etapa 2 em cada computador para carregar. Ligue para **Set-DscLocalConfigurationManager** localmente em cada computador para carregar.
1. Usando o portal do Azure ou cmdlets, verifique se os computadores a serem integrados aparecem como nós de configuração de estado registrados em sua conta de automação do Azure.

## <a name="physicalvirtual-linux-machines-on-premises-or-in-a-cloud-other-than-azure"></a>Computadores Linux físicos/virtuais locais ou em uma nuvem diferente do Azure

Os servidores Linux em execução no local ou em outros ambientes de nuvem também podem ser integrados à configuração de estado da automação do Azure, desde que tenham [acesso de saída ao Azure](automation-dsc-overview.md#network-planning):

1. Certifique-se de que a versão mais recente da [PowerShell Desired State Configuration for Linux](https://github.com/Microsoft/PowerShell-DSC-for-Linux) está instalada nos computadores que você deseja integrar à Configuração do Estado de Automação do Azure.
2. Se o [padrões do Gerenciador de Configurações Local do DSC do PowerShell](/powershell/scripting/dsc/managing-nodes/metaConfig4) corresponde a seu caso de uso e você deseja integrar computadores de modo que como que eles **ambos** efetuem pull e gerem relatório para a Configuração do Estado de Automação do Azure:

   - Em cada computador Linux em que será carregada a Configuração de Estado de Automação do Azure, use `Register.py` para carregar usando os padrões do Gerenciador de Configurações Local do DSC do PowerShell:

     `/opt/microsoft/dsc/Scripts/Register.py <Automation account registration key> <Automation account registration URL>`

   - Para encontrar a chave de registro e a URL de registro de sua conta da Automação, confira a seção [**Proteger o registro**](#secure-registration) a seguir.

     Se os padrões de Configuration Manager local do DSC do PowerShell **não** corresponderem ao seu caso de uso ou se você quiser carregar computadores de forma que eles reportem para a configuração de estado da automação do Azure, siga as etapas 3-6. Caso contrário, vá diretamente para a etapa 6.

3. Siga as instruções na seção [**Gerando metaconfigurações DSC**](#generating-dsc-metaconfigurations) a seguir para gerar uma pasta com as metaconfigurações de DSC necessárias.
4. Aplique remotamente a metaconfiguração do DSC do PowerShell às máquinas que você deseja carrega:

    ```powershell
    $SecurePass = ConvertTo-SecureString -String '<root password>' -AsPlainText -Force
    $Cred = New-Object System.Management.Automation.PSCredential 'root', $SecurePass
    $Opt = New-CimSessionOption -UseSsl -SkipCACheck -SkipCNCheck -SkipRevocationCheck

    # need a CimSession for each Linux machine to onboard
    $Session = New-CimSession -Credential $Cred -ComputerName <your Linux machine> -Port 5986 -Authentication basic -SessionOption $Opt

    Set-DscLocalConfigurationManager -CimSession $Session -Path C:\Users\joe\Desktop\DscMetaConfigs
    ```

O computador no qual este comando é executado deve ter a versão mais recente do [WMF 5](https://aka.ms/wmf5latest) instalada.

1. Se você não puder aplicar as metaconfigurações do DSC do PowerShell remotamente, copie a metaconfiguração correspondente a esse computador da pasta na etapa 5 para o computador Linux. Em seguida, chame `SetDscLocalConfigurationManager.py` localmente em cada computador com Linux que deseja integrar à Configuração do Estado de Automação do Azure:

   `/opt/microsoft/dsc/Scripts/SetDscLocalConfigurationManager.py -configurationmof <path to metaconfiguration file>`

2. Usando o portal do Azure ou os cmdlets, verifique se as máquinas para carregar agora aparecem como nós DSC registrados em sua conta de Automação do Azure.

## <a name="generating-dsc-metaconfigurations"></a>Gerando metaconfigurações DSC

Para integrar genericamente qualquer máquina à configuração de estado de automação do Azure, é possível gerar uma [metaconfiguração de DSC](/powershell/scripting/dsc/managing-nodes/metaConfig) que diz ao agente de DSC para extrair e/ou relatar a configuração de estado da automação do Azure. As metaconfigurações da Configuração do Estado para o DSC de Automação do Azure podem ser geradas usando uma configuração de DSC do PowerShell, ou os cmdlets do PowerShell de Automação do Azure.

> [!NOTE]
> Metaconfigurações DSC contêm os segredos necessários para carregar um computador em uma conta de Automação para gerenciamento. Certifique-se de proteger corretamente quaisquer metaconfigurações de DSC que criar, ou exclua-as imediatamente após o uso.

### <a name="using-a-dsc-configuration"></a>Usando uma Configuração de DSC

1. Abra o VSCode (ou seu editor favorito) como administrador em um computador no seu ambiente local. O computador também deve ter a versão mais recente do [WMF 5](https://aka.ms/wmf5latest) instalada.
1. Compie o script a seguir localmente. Esse script contém uma configuração DSC do PowerShell para criar metaconfigurações e um comando para dar início à criação de metaconfigurações.

> [!NOTE]
> Nomes de Configuração de Nó do Estado diferenciam maiúsculas de minúsculas no portal. Se o caractere for incompatível o nó não aparecerá na guia **Nós**.

   ```powershell
   # The DSC configuration that will generate metaconfigurations
   [DscLocalConfigurationManager()]
   Configuration DscMetaConfigs
   {
        param
        (
            [Parameter(Mandatory=$True)]
            [String]$RegistrationUrl,

            [Parameter(Mandatory=$True)]
            [String]$RegistrationKey,

            [Parameter(Mandatory=$True)]
            [String[]]$ComputerName,

            [Int]$RefreshFrequencyMins = 30,

            [Int]$ConfigurationModeFrequencyMins = 15,

            [String]$ConfigurationMode = 'ApplyAndMonitor',

            [String]$NodeConfigurationName,

            [Boolean]$RebootNodeIfNeeded= $False,

            [String]$ActionAfterReboot = 'ContinueConfiguration',

            [Boolean]$AllowModuleOverwrite = $False,

            [Boolean]$ReportOnly
        )

        if(!$NodeConfigurationName -or $NodeConfigurationName -eq '')
        {
            $ConfigurationNames = $null
        }
        else
        {
            $ConfigurationNames = @($NodeConfigurationName)
        }

        if($ReportOnly)
        {
            $RefreshMode = 'PUSH'
        }
        else
        {
            $RefreshMode = 'PULL'
        }

        Node $ComputerName
        {
            Settings
            {
                RefreshFrequencyMins           = $RefreshFrequencyMins
                RefreshMode                    = $RefreshMode
                ConfigurationMode              = $ConfigurationMode
                AllowModuleOverwrite           = $AllowModuleOverwrite
                RebootNodeIfNeeded             = $RebootNodeIfNeeded
                ActionAfterReboot              = $ActionAfterReboot
                ConfigurationModeFrequencyMins = $ConfigurationModeFrequencyMins
            }

            if(!$ReportOnly)
            {
            ConfigurationRepositoryWeb AzureAutomationStateConfiguration
                {
                    ServerUrl          = $RegistrationUrl
                    RegistrationKey    = $RegistrationKey
                    ConfigurationNames = $ConfigurationNames
                }

                ResourceRepositoryWeb AzureAutomationStateConfiguration
                {
                ServerUrl       = $RegistrationUrl
                RegistrationKey = $RegistrationKey
                }
            }

            ReportServerWeb AzureAutomationStateConfiguration
            {
                ServerUrl       = $RegistrationUrl
                RegistrationKey = $RegistrationKey
            }
        }
   }

    # Create the metaconfigurations
    # NOTE: DSC Node Configuration names are case sensitive in the portal.
    # TODO: edit the below as needed for your use case
   $Params = @{
        RegistrationUrl = '<fill me in>';
        RegistrationKey = '<fill me in>';
        ComputerName = @('<some VM to onboard>', '<some other VM to onboard>');
        NodeConfigurationName = 'SimpleConfig.webserver';
        RefreshFrequencyMins = 30;
        ConfigurationModeFrequencyMins = 15;
        RebootNodeIfNeeded = $False;
        AllowModuleOverwrite = $False;
        ConfigurationMode = 'ApplyAndMonitor';
        ActionAfterReboot = 'ContinueConfiguration';
        ReportOnly = $False;  # Set to $True to have machines only report to AA DSC but not pull from it
   }

   # Use PowerShell splatting to pass parameters to the DSC configuration being invoked
   # For more info about splatting, run: Get-Help -Name about_Splatting
   DscMetaConfigs @Params
   ```

1. Preencha a chave de registro e a URL para sua conta de Automação, bem como os nomes dos computadores a carregar. Todos os outros parâmetros são opcionais. Para encontrar a chave de registro e a URL de registro de sua conta da Automação, confira a seção [**Proteger o registro**](#secure-registration) a seguir.
1. Se você deseja que as máquinas relatem informações de status de DSC à Configuração do Estado de Automação do Azure, mas não efetue pull da configuração de recepção ou módulos do PowerShell, defina o parâmetro **ReportOnly** como verdadeiro.
1. Execute o script. Agora você deve ter uma pasta chamada **DscMetaConfigs** no diretório de trabalho, que contém as metaconfigurações de DSC do PowerShell para os computadores a carregar (como um administrador):

    ```powershell
    Set-DscLocalConfigurationManager -Path ./DscMetaConfigs
    ```

### <a name="using-the-azure-automation-cmdlets"></a>Usando os cmdlets da Automação do Azure

Se o padrões do Gerenciador de Configurações Local do DSC do PowerShell correspondem a seu caso de uso e você deseja integrar computadores de modo que eles ambos efetuem pull e gerem relatório para a Configuração do Estado de Automação do Azure, os cmdlets de Automação do Azure fornecem um método simplificado para gerar as metaconfigurações de DSC necessárias:

1. Abra o console do PowerShell ou o VSCode como administrador em uma máquina no seu ambiente local.
2. Conecte-se ao Azure Resource Manager usando `Connect-AzAccount`
3. Baixe, da conta de Automação da qual você deseja carregar nós, as metaconfigurações do DSC do PowerShell para as máquinas que você deseja carregar:

   ```powershell
   # Define the parameters for Get-AzAutomationDscOnboardingMetaconfig using PowerShell Splatting
   $Params = @{
       ResourceGroupName = 'ContosoResources'; # The name of the Resource Group that contains your Azure Automation Account
       AutomationAccountName = 'ContosoAutomation'; # The name of the Azure Automation Account where you want a node on-boarded to
       ComputerName = @('web01', 'web02', 'sql01'); # The names of the computers that the meta configuration will be generated for
       OutputFolder = "$env:UserProfile\Desktop\";
   }
   # Use PowerShell splatting to pass parameters to the Azure Automation cmdlet being invoked
   # For more info about splatting, run: Get-Help -Name about_Splatting
   Get-AzAutomationDscOnboardingMetaconfig @Params
   ```

1. Agora você deve ter uma pasta chamada ***DscMetaConfigs***que contém as metaconfigurações de DSC do PowerShell para os computadores a carregar (como um administrador):

    ```powershell
    Set-DscLocalConfigurationManager -Path $env:UserProfile\Desktop\DscMetaConfigs
    ```

## <a name="secure-registration"></a>Proteger o registro

Os computadores podem ser carregados com segurança a uma conta de Automação do Azure por meio do protocolo de registro de WMF 5 DSC, que permite a um nó DSC se autenticar a um servidor de Recepção ou de Relatórios do DSC do PowerShell (incluindo a Configuração do Estado de Automação do Azure). O nó é registrado no servidor em uma **URL de Registro**, autenticando por meio de uma **Chave de registro**. Durante o registro, o nó de DSC e o servidor de Recepção/Relatórios DSC negociam um certificado exclusivo para este nó a ser usado para a autenticação do servidor após o registro. Esse processo impede que nós integrados representem um ao outro, por exemplo, se um nó for comprometido e estiver se comportando de maneira mal-intencionada. Após o registro, a Chave de registro não é usada para autenticação novamente e é excluída do nó.

Você pode obter as informações necessárias para o protocolo de registro da Configuração do Estado a partir das **Chaves** nas **Configurações de Conta**no portal do Azure. Abra a folha clicando no ícone de chave no painel **Noções Básicas** da conta da Automação.

![Chaves de automação do Azure e a URL](./media/automation-dsc-onboarding/DSC_Onboarding_4.png)

- A URL de registro é o campo de URL na folha Gerenciar chaves.
- A chave de registro é a Chave de Acesso Primária ou Chave de Acesso Secundária na folha Gerenciar chaves. Qualquer chave pode ser usada.

Para aumentar a segurança, as chaves de acesso primária e secundária de uma conta de Automação podem ser regeneradas a qualquer momento (na página **Gerenciar chaves** ) para impedir que os registros de nós futuros usem chaves anteriores.

## <a name="certificate-expiration-and-re-registration"></a>Expiração e novo registro do certificado

Depois de registrar um computador como um nó DSC na configuração de estado da automação do Azure, há várias razões pelas quais você pode precisar registrar novamente esse nó no futuro:

- Para versões do Windows Server anteriores ao Windows Server 2019, cada nó negocia automaticamente um certificado exclusivo para autenticação que expira após um ano. Atualmente, o protocolo de registro DSC do PowerShell não pode renovar automaticamente os certificados quando eles estão se aproximando da expiração, portanto, você precisa registrar novamente os nós após a hora de um ano. Antes de registrar novamente, verifique se cada nó está executando o Windows Management Framework 5,0 RTM. Se o certificado de autenticação de um nó expirar e o nó não for registrado novamente, o nó não poderá se comunicar com a automação do Azure e será marcado como ' sem resposta '. o novo registro realizado em 90 dias ou menos do tempo de expiração do certificado, ou a qualquer momento após o tempo de expiração do certificado, resultará em uma nova geração e uso de um certificado.  Uma resolução para esse problema está incluída no Windows Server 2019 e posterior.
- Para alterar quaisquer [valores do Gerenciador de Configuração Local do PowerShell DSC](/powershell/scripting/dsc/managing-nodes/metaConfig4) que foram definidos durante o registro inicial do nó, como ConfigurationMode. Atualmente, esses valores do agente DSC só podem ser alterados por meio de um novo registro. A única exceção é a Configuração de Nó atribuída ao nó, isso pode ser alterado diretamente no DSC de Automação do Azure.

o novo registro pode ser executado da mesma maneira que você registrou o nó inicialmente, usando qualquer um dos métodos de integração descritos neste documento. Você não precisa cancelar o registro de um nó da configuração de estado da automação do Azure antes de registrá-lo novamente.

## <a name="troubleshooting-azure-virtual-machine-onboarding"></a>Solução de problemas de integração de máquina virtual do Azure

O Configuração do Estado de Automação do Azure permite a fácil integração de VMs do Microsoft Azure ao gerenciamento de configuração. Nos bastidores, a extensão de Configuração de Estado Desejado da VM do Azure é usada para registrar a VM com a Configuração do Estado de Automação do Azure. Como a extensão de Configuração de Estado Desejado da VM do Azure é executada de forma assíncrona, acompanhar o seu progresso e solucionar os problemas de sua execução pode ser importante.

> [!NOTE]
> Qualquer método de integração de uma VM do Azure à Configuração do Estado de Automação do Azure que usa a extensão de Configuração de Estado Desejado da VM do Azure pode levar até uma hora para o nó mostrar como registrado na Automação do Azure. Isso se deve à instalação do Windows Management Framework 5.0 na VM pela extensão DSC da VM do Azure, que precisa integrar a VM à Configuração do Estado de Automação do Azure.

Para solucionar problemas ou exibir o status da extensão de Configuração de Estado Desejado da VM do Azure, no portal do Azure, navegue até a VM que está sendo integrada e clique em **Extensões** em **Configurações**. Em seguida, clique em **DSC** ou **DSCForLinux** dependendo do seu sistema operacional. Para obter mais detalhes, você pode clicar em **Exibir status detalhado**.

Para obter mais informações sobre solução de problemas, consulte [Solucionando problemas com a configuração de estado desejado (DSC) da automação do Azure](./troubleshoot/desired-state-configuration.md).

## <a name="next-steps"></a>Próximos passos

- Para começar, consulte [Introdução à Configuração de Estado da Automação do Azure](automation-dsc-getting-started.md)
- Para saber como compilar configurações de DSC para que possam ser atribuídas a nós de destino, consulte [Compilar configurações na Configuração de Estado da Automação do Azure](automation-dsc-compile.md)
- Para referência de cmdlet do PowerShell, consulte [Cmdlets da Configuração de Estado da Automação do Azure](/powershell/module/az.automation#automation)
- Para obter informações sobre preços, consulte [Preço da Configuração de Estado da Automação do Azure](https://azure.microsoft.com/pricing/details/automation/)
- Para ver um exemplo de uso da Configuração de Estado da Automação do Azure em um pipeline de implantação contínua, consulte [Implantação contínua usando Configuração de Estado da Automação do Azure e Chocolatey](automation-dsc-cd-chocolatey.md)

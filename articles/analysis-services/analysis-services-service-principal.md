---
title: Automatize tarefas dos Serviços de Análise do Azure com os diretores de serviço | Microsoft Docs
description: Saiba como criar um princípio de serviço para automatizar tarefas administrativas do Azure Analysis Services.
author: minewiskan
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 02/18/2020
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: dc163de9a7fb46d62f4bc2983e040e68bbf9231c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79266137"
---
# <a name="automation-with-service-principals"></a>Automação com entidades de serviço

As entidades de serviço são um recurso de aplicativo do Azure Active Directory que você cria em seu locatário para executar operações de nível de serviço e recursos autônomos. Eles são um tipo exclusivo de *identidade do usuário* com uma ID de aplicativo e senha ou certificado. Uma entidade de serviço tem apenas as permissões necessárias para executar tarefas definidas pelas funções e permissões para as quais é atribuída. 

No Analysis Services, as entidades de serviço são usadas com o Automação do Azure, o modo autônomo do PowerShell, aplicativos cliente personalizados e aplicativos Web para automatizar tarefas comuns. Por exemplo, servidores de provisionamento, modelos de implantação, atualização de dados, escalar verticalmente/reduzir verticalmente e pausar/continuar podem ser automatizados usando entidades de serviço. As permissões são atribuídas a entidades de serviço por meio da associação de funções, semelhante às contas comuns de UPN do Microsoft Azure AD.

Os Serviços de Análise também suportam operações realizadas por identidades gerenciadas usando diretores de serviços. Para saber mais, consulte [identidades gerenciadas para recursos do Azure](../active-directory/managed-identities-azure-resources/overview.md) e [serviços do Azure que suportam a autenticação Azure AD](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md#azure-analysis-services).  

## <a name="create-service-principals"></a>Criar entidades de serviço
 
As entidades de serviço podem ser criadas no portal do Azure ou usando o PowerShell. Para obter mais informações, consulte:

[Criar o principal de serviço - Portal Azure](../active-directory/develop/howto-create-service-principal-portal.md)   
[Criar entidade de serviço - PowerShell](../active-directory/develop/howto-authenticate-service-principal-powershell.md)

## <a name="store-credential-and-certificate-assets-in-azure-automation"></a>Armazenar credenciais e ativos de certificado na Automação do Azure

Credenciais e certificados da entidade de serviço podem ser armazenados de forma segura na Automação do Azure para operações de runbook. Para obter mais informações, consulte:

[Ativos de credencial na Azure Automation](../automation/automation-credentials.md)   
[Ativos de certificado na Automação do Azure](../automation/automation-certificates.md)

## <a name="add-service-principals-to-server-admin-role"></a>Adicionar entidades de serviço à função de administrador do servidor

Antes de poder usar uma entidade de serviço para operações de gerenciamento de servidor do Analysis Services, você deverá adicioná-la à função de administradores do servidor. Para saber mais, consulte [Adicionar uma entidade de serviço à função de administrador do servidor](analysis-services-addservprinc-admins.md).

## <a name="service-principals-in-connection-strings"></a>Entidades de serviço em cadeias de conexão

AppID de entidade de serviço e senha ou certificado podem ser usados em cadeias de conexão da mesma forma que um UPN.

### <a name="powershell"></a>PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

#### <a name="using-azanalysisservices-module"></a><a name="azmodule" />Usando o módulo Az.AnalysisServices

Ao usar um diretor de serviço para operações de `Connect-AzAccount` gerenciamento de recursos com o módulo [Az.AnalysisServices,](/powershell/module/az.analysisservices) use cmdlet. 

No exemplo a seguir, o appID e uma senha são usados para executar operações de plano de controle para sincronização em réplicas somente leitura e dimensionar/apagar:

```powershell
Param (
        [Parameter(Mandatory=$true)] [String] $AppId,
        [Parameter(Mandatory=$true)] [String] $PlainPWord,
        [Parameter(Mandatory=$true)] [String] $TenantId
       )
$PWord = ConvertTo-SecureString -String $PlainPWord -AsPlainText -Force
$Credential = New-Object -TypeName "System.Management.Automation.PSCredential" -ArgumentList $AppId, $PWord

# Connect using Az module
Connect-AzAccount -Credential $Credential -SubscriptionId "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxx"

# Syncronize a database for query scale out
Sync-AzAnalysisServicesInstance -Instance "asazure://westus.asazure.windows.net/testsvr" -Database "testdb"

# Scale up the server to an S1, set 2 read-only replicas, and remove the primary from the query pool. The new replicas will hydrate from the synchronized data.
Set-AzAnalysisServicesServer -Name "testsvr" -ResourceGroupName "testRG" -Sku "S1" -ReadonlyReplicaCount 2 -DefaultConnectionMode Readonly
```

#### <a name="using-sqlserver-module"></a>Usando o módulo SQLServer

No exemplo a seguir, appID e uma senha são usados para executar uma operação de atualização de modelo de banco de dados:

```powershell
Param (
        [Parameter(Mandatory=$true)] [String] $AppId,
        [Parameter(Mandatory=$true)] [String] $PlainPWord,
        [Parameter(Mandatory=$true)] [String] $TenantId
       )
$PWord = ConvertTo-SecureString -String $PlainPWord -AsPlainText -Force

$Credential = New-Object -TypeName "System.Management.Automation.PSCredential" -ArgumentList $AppId, $PWord

Invoke-ProcessTable -Server "asazure://westcentralus.asazure.windows.net/myserver" -TableName "MyTable" -Database "MyDb" -RefreshType "Full" -ServicePrincipal -ApplicationId $AppId -TenantId $TenantId -Credential $Credential
```

### <a name="amo-and-adomd"></a>AMO e ADOMD 

Ao conectar com aplicativos cliente e aplicativos Web, os pacotes instaláveis das [bibliotecas cliente AMO e ADOMD](analysis-services-data-providers.md) versão 15.0.2 e superior do NuGet dão suporte para entidades de serviço nas cadeias de conexão usando a sintaxe a seguir: `app:AppID` e senha ou `cert:thumbprint`. 

No exemplo a seguir, `appID` e `password` são usados para executar uma operação de atualização de modelo de banco de dados:

```csharp
string appId = "xxx";
string authKey = "yyy";
string connString = $"Provider=MSOLAP;Data Source=asazure://westus.asazure.windows.net/<servername>;User ID=app:{appId};Password={authKey};";
Server server = new Server();
server.Connect(connString);
Database db = server.Databases.FindByName("adventureworks");
Table tbl = db.Model.Tables.Find("DimDate");
tbl.RequestRefresh(RefreshType.Full);
db.Model.SaveChanges();
```

## <a name="next-steps"></a>Próximas etapas
[Faça login com o Azure PowerShell](https://docs.microsoft.com/powershell/azure/authenticate-azureps)   
[Adicionar uma entidade de serviço à função de administrador do servidor](analysis-services-addservprinc-admins.md)   

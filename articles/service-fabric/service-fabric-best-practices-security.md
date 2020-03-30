---
title: Melhores práticas de segurança do Azure Service Fabric
description: As melhores práticas e considerações de design para manter os clusters e aplicativos do Azure Service Fabric são seguros.
author: peterpogorski
ms.topic: conceptual
ms.date: 01/23/2019
ms.author: pepogors
ms.openlocfilehash: dcdc338bdcdb2c04f6b8894ccb358bc773b95c07
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79258922"
---
# <a name="azure-service-fabric-security"></a>Segurança do Azure Service Fabric 

Para saber mais sobre as [práticas de segurança recomendadas para o Azure](https://docs.microsoft.com/azure/security/), examine as [práticas de segurança recomendadas para o Azure Service Fabric](https://docs.microsoft.com/azure/security/fundamentals/service-fabric-best-practices)

## <a name="key-vault"></a>Key Vault

[O Azure Key Vault](https://docs.microsoft.com/azure/key-vault/) é o serviço de gerenciamento de segredos recomendado para clusters e aplicativos do Service Fabric.
> [!NOTE]
> Se os certificados/segredos de um Key Vault são implantados em um Conjunto de dimensionamento de máquinas virtuais como um Segredo de conjunto de dimensionamento de máquinas virtuais, o Key Vault e o Conjunto de dimensionamento de máquinas virtuais devem ser colocalizados.

## <a name="create-certificate-authority-issued-service-fabric-certificate"></a>Criar a autoridade de certificação que emitiu o certificado do Service Fabric

O certificado do Azure Key Vault pode ser criado ou importado em um Key Vault. Quando um certificado de Key Vault é criado, a chave privada é criada dentro do Key Vault e nunca é exposta ao proprietário do certificado. Veja a seguir maneiras de criar um certificado no Key Vault:

- Criar um certificado autoassinado para criar um par de chaves públicas-privadas e associá-lo a um certificado. O certificado será assinado por sua própria chave. 
- Criar um novo certificado manualmente para criar um par de chaves públicas-privadas e gerar uma solicitação de assinatura de certificado X.509. A solicitação de assinatura pode ser assinada por sua autoridade de registro ou a autoridade de certificação. O certificado X.509 assinado pode ser mesclado com o par de chaves pendente para concluir o certificado do KV no Key Vault. Embora esse método requeira mais etapas, ele oferece maior segurança porque a chave privada é criada no Key Vault e restrita a ele. Isso é explicado no diagrama a seguir. 

Examine os [métodos de criação de certificados do Azure Keyvault](https://docs.microsoft.com/azure/key-vault/create-certificate) para obter mais detalhes.

## <a name="deploy-key-vault-certificates-to-service-fabric-cluster-virtual-machine-scale-sets"></a>Implantar certificados do Key Vault em Conjuntos de Dimensionamento de Máquinas Virtuais no cluster do Service Fabric

Para implantar certificados de um keyvault colocalizado em um Conjunto de dimensionamento de máquinas virtuais, use o Conjunto de dimensionamento de máquinas virtuais [osProfile](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/createorupdate#virtualmachinescalesetosprofile). Veja a seguir as propriedades de modelo do Resource Manager :

```json
"secrets": [
   {
       "sourceVault": {
           "id": "[parameters('sourceVaultValue')]"
       },
       "vaultCertificates": [
          {
              "certificateStore": "[parameters('certificateStoreValue')]",
              "certificateUrl": "[parameters('certificateUrlValue')]"
          }
       ]
   }
]
```

> [!NOTE]
> O vault deve ser habilitado para a implantação do modelo do Resource Manager.

## <a name="apply-an-access-control-list-acl-to-your-certificate-for-your-service-fabric-cluster"></a>Aplicar uma ACL (Lista de Controle de Acesso) em seu certificado para o cluster do Service Fabric

O publicador das [extensões do Conjunto de dimensionamento de máquinas virtuais](https://docs.microsoft.com/cli/azure/vmss/extension?view=azure-cli-latest) Microsoft.Azure.ServiceFabric é usado para configurar a Segurança dos Nós.
Para aplicar uma ACL a seus certificados para os processos de cluster do Service Fabric, use as seguintes propriedades de modelo do Resource Manager:

```json
"certificate": {
   "commonNames": [
       "[parameters('certificateCommonName')]"
   ],
   "x509StoreName": "[parameters('certificateStoreValue')]"
}
```

## <a name="secure-a-service-fabric-cluster-certificate-by-common-name"></a>Proteger um certificado de cluster do Service Fabric com nome comum

Para proteger o cluster do Service Fabric por certificado `Common Name`, use a propriedade de modelo do Resource Manager [certificateCommonNames](https://docs.microsoft.com/rest/api/servicefabric/sfrp-model-clusterproperties#certificatecommonnames), da seguinte maneira:

```json
"certificateCommonNames": {
    "commonNames": [
        {
            "certificateCommonName": "[parameters('certificateCommonName')]",
            "certificateIssuerThumbprint": "[parameters('certificateIssuerThumbprint')]"
        }
    ],
    "x509StoreName": "[parameters('certificateStoreValue')]"
}
```

> [!NOTE]
> Os clusters do Service Fabric usarão o primeiro certificado válido que encontrarem no repositório de certificados do host. No Windows, esse será o certificado com a data de validade mais recente que corresponde à impressão digital do seu Nome Comum e Emissor.

Os domínios do Azure, como *\<SEU SUBDOMÍNIO\>.cloudapp.azure.com ou \<SEU SUBDOMÍNIO\>.trafficmanager.net, são propriedades da Microsoft. As autoridades de certificação não emitirão certificados para domínios de usuários não autorizados. A maioria dos usuários precisará comprar um domínio de um registrador ou ser um administrador de domínio autorizado para que uma autoridade de certificação emita um certificado com esse nome comum.

Para obter detalhes adicionais sobre como configurar o serviço DNS para resolver seu domínio para um endereço IP da Microsoft, examine as informações sobre como configurar o [DNS do Azure para hospedar seu domínio](https://docs.microsoft.com/azure/dns/dns-delegate-domain-azure-dns).

> [!NOTE]
> Após delegar seus servidores de nome de domínios a seus servidores de nome de zona DNS do Azure, adicione os dois registros a seguir na zona DNS:
> - Um registro 'A' para o domínio APEX que NÃO seja um `Alias record set` em todos os endereços IP que seu domínio personalizado resolverá.
> - Um registro de 'C' para subdomínios Microsoft que você provisionou que NÃO seja um `Alias record set`. Por exemplo, você pode usar o Gerenciador de Tráfego ou nome DNS do Load Balancer.

Para atualizar seu portal a fim de exibir um nome DNS personalizado de seu cluster do Service Fabric `"managementEndpoint"`, atualize as seguintes propriedades de modelo do Resource Manager do Cluster do Service Fabric:

```json
 "managementEndpoint": "[concat('https://<YOUR CUSTOM DOMAIN>:',parameters('nt0fabricHttpGatewayPort'))]",
```

## <a name="encrypting-service-fabric-package-secret-values"></a>Criptografar os valores de segredo de pacote do Service Fabric

Os valores comuns que são criptografados em pacotes do Service Fabric incluem as credenciais de Registro de Contêiner do Azure (ACR), variáveis de ambiente, configurações e chaves de conta de armazenamento de plug-in do volume do Azure.

Para [configurar um certificado de criptografia e criptografar segredos em clusters do Windows](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-secret-management-windows):

Gere um certificado autoassinado para criptografar seu segredo:

```powershell
New-SelfSignedCertificate -Type DocumentEncryptionCert -KeyUsage DataEncipherment -Subject mydataenciphermentcert -Provider 'Microsoft Enhanced Cryptographic Provider v1.0'
```

Use as instruções em [Implantar certificados do Key Vault no Conjunto de dimensionamento de máquinas virtuais do cluster do Service Fabric](#deploy-key-vault-certificates-to-service-fabric-cluster-virtual-machine-scale-sets) para implantar os certificados do Key Vault em seus Conjuntos de Dimensionamento de Máquinas Virtuais do Cluster do Service Fabric.

Criptografe seu segredo usando o seguinte comando do PowerShell e, em seguida, atualize o manifesto do aplicativo do Service Fabric com o valor criptografado:

``` powershell
Invoke-ServiceFabricEncryptText -CertStore -CertThumbprint "<thumbprint>" -Text "mysecret" -StoreLocation CurrentUser -StoreName My
```

Para [configurar um certificado de criptografia e criptografar segredos em clusters do Linux](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-secret-management-linux):

Gerar um certificado autoassinado para criptografar segredos:

```bash
user@linux:~$ openssl req -newkey rsa:2048 -nodes -keyout TestCert.prv -x509 -days 365 -out TestCert.pem
user@linux:~$ cat TestCert.prv >> TestCert.pem
```

Siga as instruções descritas em [Implantar certificados do Key Vault em Conjunto de dimensionamento de máquinas virtuais do cluster do Service Fabric](#deploy-key-vault-certificates-to-service-fabric-cluster-virtual-machine-scale-sets) em seus Conjuntos de Dimensionamento de Máquinas Virtuais do Cluster do Service Fabric.

Criptografe seu segredo usando os seguintes comandos e, em seguida, atualize o Manifesto do aplicativo do Service Fabric com o valor criptografado:

```bash
user@linux:$ echo "Hello World!" > plaintext.txt
user@linux:$ iconv -f ASCII -t UTF-16LE plaintext.txt -o plaintext_UTF-16.txt
user@linux:$ openssl smime -encrypt -in plaintext_UTF-16.txt -binary -outform der TestCert.pem | base64 > encrypted.txt
```

Depois de criptografar os valores protegidos, [especifique os segredos criptografados no aplicativo do Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-secret-management#specify-encrypted-secrets-in-an-application) e [descriptografe os segredos criptografados no código de serviço](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-secret-management#decrypt-encrypted-secrets-from-service-code).

## <a name="include-certificate-in-service-fabric-applications"></a>Incluir certificado em aplicativos de malha de serviço

Para dar acesso a segredos ao seu aplicativo, inclua o certificado adicionando um elemento **SecretsCertificate** ao manifesto do aplicativo.

```xml
<ApplicationManifest … >
  ...
  <Certificates>
    <SecretsCertificate Name="MyCert" X509FindType="FindByThumbprint" X509FindValue="[YourCertThumbrint]"/>
  </Certificates>
</ApplicationManifest>
```
## <a name="authenticate-service-fabric-applications-to-azure-resources-using-managed-service-identity-msi"></a>Autenticar aplicativos do Service Fabric para recursos do Azure usando a Identidade de Serviço Gerenciada (MSI)

Para saber mais sobre as identidades gerenciadas para os recursos do Azure, confira o artigo [O que são as identidades gerenciadas para recursos do Azure?](../active-directory/managed-identities-azure-resources/overview.md#how-does-the-managed-identities-for-azure-resources-work).
Os clusters do Azure Service Fabric são hospedados em Conjuntos de Dimensionamento de Máquinas Virtuais que dão suporte à [Identidade de Serviço Gerenciada](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/services-support-msi#azure-services-that-support-managed-identities-for-azure-resources).
Para obter uma lista de serviços nos quais a MSI pode ser usada para autenticação, confira os [serviços do Azure que dão suporte à autenticação do Azure Active Directory](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/services-support-msi#azure-services-that-support-azure-ad-authentication).


Para habilitar a identidade gerenciada atribuída ao sistema durante a criação de um conjunto de dimensionamento de máquinas virtuais ou um conjunto de dimensionamento de máquinas virtuais existente, declare a seguinte propriedade `"Microsoft.Compute/virtualMachinesScaleSets"`:

```json
"identity": { 
    "type": "SystemAssigned"
}
```
Confira o artigo [O que são as identidades gerenciadas para recursos do Azure?](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/qs-configure-template-windows-vmss#system-assigned-managed-identity) para saber mais.

Se você tiver criado uma [identidade gerenciada atribuída ao usuário](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-arm#create-a-user-assigned-managed-identity), declare o recurso a seguir em seu modelo para atribuí-la a seu conjunto de dimensionamento de máquinas virtuais. Substitua `\<USERASSIGNEDIDENTITYNAME\>` pelo nome da identidade gerenciada atribuída ao usuário que você criou:

```json
"identity": {
    "type": "userAssigned",
    "userAssignedIdentities": {
        "[resourceID('Microsoft.ManagedIdentity/userAssignedIdentities/',variables('<USERASSIGNEDIDENTITYNAME>'))]": {}
    }
}
```

Para que o aplicativo do Service Fabric possa usar a identidade gerenciada, é necessário conceder permissões aos Recursos do Azure de que ele necessita para se autenticar.
Os comandos a seguir concedem acesso a um Recurso do Azure:

```bash
principalid=$(az resource show --id /subscriptions/<YOUR SUBSCRIPTON>/resourceGroups/<YOUR RG>/providers/Microsoft.Compute/virtualMachineScaleSets/<YOUR SCALE SET> --api-version 2018-06-01 | python -c "import sys, json; print(json.load(sys.stdin)['identity']['principalId'])")

az role assignment create --assignee $principalid --role 'Contributor' --scope "/subscriptions/<YOUR SUBSCRIPTION>/resourceGroups/<YOUR RG>/providers/<PROVIDER NAME>/<RESOURCE TYPE>/<RESOURCE NAME>"
```

No código do aplicativo Service Fabric, [obtenha um token de acesso para](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/how-to-use-vm-token#get-a-token-using-http) o Azure Resource Manager, tornando um REST semelhante ao seguinte:

```bash
access_token=$(curl 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fmanagement.azure.com%2F' -H Metadata:true | python -c "import sys, json; print json.load(sys.stdin)['access_token']")

```

O aplicativo do Service Fabric poderá, então, usar o token de acesso para se autenticar nos Recursos do Azure que dão suporte ao Active Directory.
O exemplo a seguir mostra como fazer isso no recurso Cosmos DB:

```bash
cosmos_db_password=$(curl 'https://management.azure.com/subscriptions/<YOUR SUBSCRIPTION>/resourceGroups/<YOUR RG>/providers/Microsoft.DocumentDB/databaseAccounts/<YOUR ACCOUNT>/listKeys?api-version=2016-03-31' -X POST -d "" -H "Authorization: Bearer $access_token" | python -c "import sys, json; print(json.load(sys.stdin)['primaryMasterKey'])")
```
## <a name="windows-security-baselines"></a>Linhas de base de segurança do Windows
[Recomendamos que você implemente uma configuração padrão do setor que seja amplamente conhecida e bem testada, como as linhas de base de segurança da Microsoft, em vez de criar uma linha de base você mesmo;](https://docs.microsoft.com/windows/security/threat-protection/windows-security-baselines) uma opção para provisioná-los em seus Conjuntos de Escala de Máquina Virtual é usar o manipulador de extensão DSC (Desired State Configuration, configuração do estado desejado) do Azure, para configurar as VMs à medida que elas estiverem on-line, para que estejam executando o software de produção.

## <a name="azure-firewall"></a>Firewall do Azure
[O Azure Firewall é um serviço gerenciado de segurança de rede baseado em nuvem que protege os recursos da Rede Virtual do Azure. É um firewall totalmente imponente como um serviço com alta disponibilidade incorporada e escalabilidade irrestrita da nuvem;](https://docs.microsoft.com/azure/firewall/overview) isso permite limitar o tráfego HTTP/S de saída a uma lista especificada de nomes de domínio totalmente qualificados (FQDN), incluindo curingas. Esse recurso não exige a terminação SSL. É recomendado que você aproveite as [tags FQDN do Firewall Do Azure](https://docs.microsoft.com/azure/firewall/fqdn-tags) para atualizações do Windows e para permitir que o tráfego de rede para os pontos finais do Microsoft Windows Update possa fluir através do seu firewall. [Implantar o Azure Firewall usando um modelo](https://docs.microsoft.com/azure/firewall/deploy-template) fornece uma amostra para a definição de modelo de recurso Microsoft.Network/azureFirewalls. As regras de firewall comuns aos aplicativos de malha de serviço são permitir o seguinte para a rede virtual de clusters:

- *download.microsoft.com
- *A servicefabric.azure.com
- *.core.windows.net

Essas regras de firewall complementam os grupos de segurança de rede de saída permitidos, que incluiriam ServiceFabric e Storage, conforme destinos permitidos de sua rede virtual.

## <a name="tls-12"></a>TLS 1.2
[Etg](https://github.com/Azure/Service-Fabric-Troubleshooting-Guides/blob/master/Security/TLS%20Configuration.md)

## <a name="windows-defender"></a>Windows Defender 

Por padrão, o antivírus Windows Defender está instalado no Windows Server 2016. Para obter detalhes, confira o artigo [Antivírus Windows Defender no Windows Server 2016](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-antivirus/windows-defender-antivirus-on-windows-server-2016). A interface do usuário é instalada por padrão em alguns SKUs, mas não é necessária. Para reduzir qualquer impacto no desempenho e uma sobrecarga de consumo de recurso incorridos pelo Windows Defender, e se as políticas de segurança permitirem excluir processos e caminhos do software livre, declare as seguintes propriedades no modelo do Resource Manager da Extensão do Conjunto de dimensionamento de máquinas virtuais para excluir o cluster do Service Fabric das verificações:


```json
 {
    "name": "[concat('VMIaaSAntimalware','_vmNodeType0Name')]",
    "properties": {
        "publisher": "Microsoft.Azure.Security",
        "type": "IaaSAntimalware",
        "typeHandlerVersion": "1.5",
        "settings": {
            "AntimalwareEnabled": "true",
            "Exclusions": {
                "Paths": "[concat(parameters('svcFabData'), ';', parameters('svcFabLogs'), ';', parameters('svcFabRuntime'))]",
                "Processes": "Fabric.exe;FabricHost.exe;FabricInstallerService.exe;FabricSetup.exe;FabricDeployer.exe;ImageBuilder.exe;FabricGateway.exe;FabricDCA.exe;FabricFAS.exe;FabricUOS.exe;FabricRM.exe;FileStoreService.exe"
            },
            "RealtimeProtectionEnabled": "true",
            "ScheduledScanSettings": {
                "isEnabled": "true",
                "scanType": "Quick",
                "day": "7",
                "time": "120"
            }
        },
        "protectedSettings": null
    }
}
```

> [!NOTE]
> Confira a documentação sobre antimalware para obter as regras de configuração, caso não esteja usando o Windows Defender. O Windows Defender não tem suporte no Linux.

## <a name="platform-isolation"></a>Isolamento da plataforma
Por padrão, os aplicativos service fabric têm acesso ao próprio tempo de execução service fabric, que se manifesta de diferentes [formas: variáveis de ambiente](service-fabric-environment-variables-reference.md) apontando para caminhos de arquivo no host correspondentes a arquivos de aplicativos e malha, um ponto final de comunicação entre processos que aceita solicitações específicas do aplicativo e o certificado de cliente que a Fabric espera que o aplicativo use para se autenticar. Na eventualidade de o serviço hospedar-se código não confiável, é aconselhável desativar esse acesso ao tempo de execução do SF - a menos que explicitamente necessário. O acesso ao tempo de execução é removido usando a seguinte declaração na seção Políticas do manifesto de aplicação: 

```xml
<ServiceManifestImport>
    <Policies>
        <ServiceFabricRuntimeAccessPolicy RemoveServiceFabricRuntimeAccess="true"/>
    </Policies>
</ServiceManifestImport>

```

## <a name="next-steps"></a>Próximas etapas

* Crie um cluster em VMs ou computadores, executando o Windows Server: [Criação de cluster service fabric para O Windows Server](service-fabric-cluster-creation-for-windows-server.md).
* Crie um cluster em VMs ou computadores, executando o Linux: [Crie um cluster Linux](service-fabric-cluster-creation-via-portal.md).
* Saiba mais sobre as [opções de suporte do Service Fabric](service-fabric-support.md).

[Image1]: ./media/service-fabric-best-practices/generate-common-name-cert-portal.png

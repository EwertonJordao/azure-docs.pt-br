---
title: Azure Key Vault extensão de VM para Windows
description: Implante um agente que execute a atualização automática de segredos de Cofre de Chaves em máquinas virtuais usando uma extensão da máquina virtual.
services: virtual-machines-windows
author: msmbaldwin
tags: keyvault
ms.service: virtual-machines-windows
ms.topic: article
ms.date: 12/02/2019
ms.author: mbaldwin
ms.openlocfilehash: 8e014e7a1c564377582e4503218c4129619daa91
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80410726"
---
# <a name="key-vault-virtual-machine-extension-for-windows"></a>Extensão da máquina virtual de Key Vault para Windows

A extensão de VM Key Vault fornece a atualização automática de certificados armazenados em um cofre de chaves do Azure. Especificamente, os monitores de extensão observado de uma lista de certificados armazenados no cofre de chaves e, ao detectar uma alteração, recupera e instala os certificados correspondentes. Este documento detalha as plataformas com opções de plataformas, configurações e implantação com suporte para a extensão da VM de Key Vault para Windows. 

### <a name="operating-system"></a>Sistema operacional

A extensão de VM Key Vault dá suporte a versões anteriores do Windows:

- Windows Server 2019
- Windows Server 2016
- Windows Server 2012

### <a name="supported-certificate-content-types"></a>Tipos de conteúdo de certificado com suporte

- #12 PKCS
- PEM

## <a name="extension-schema"></a>Esquema de extensão

O JSON a seguir mostra o esquema para a extensão da VM de Key Vault. A extensão não requer configurações protegidas. todas as suas configurações são consideradas informações públicas. A extensão requer uma lista de certificados monitorados, a frequência de sondagem e o repositório de certificados de destino. Especificamente:  

```json
    {
      "type": "Microsoft.Compute/virtualMachines/extensions",
      "name": "KVVMExtensionForWindows",
      "apiVersion": "2019-07-01",
      "location": "<location>",
      "dependsOn": [
          "[concat('Microsoft.Compute/virtualMachines/', <vmName>)]"
      ],
      "properties": {
      "publisher": "Microsoft.Azure.KeyVault",
      "type": "KeyVaultForWindows",
      "typeHandlerVersion": "1.0",
      "autoUpgradeMinorVersion": true,
      "settings": {
        "secretsManagementSettings": {
          "pollingIntervalInS": <polling interval in seconds, e.g: "3600">,
          "certificateStoreName": <certificate store name, e.g.: "MY">,
          "linkOnRenewal": <Only Windows. This feature enables auto-rotation of SSL certificates, without necessitating a re-deployment or binding.  e.g.: false>,
          "certificateStoreLocation": <certificate store location, currently it works locally only e.g.: "LocalMachine">,
          "requireInitialSync": <initial synchronization of certificates e..g: true>,
          "observedCertificates": <list of KeyVault URIs representing monitored certificates, e.g.: "https://myvault.vault.azure.net/secrets/mycertificate"
        }      
      }
      }
    }
```

> [!NOTE]
> Suas URLs de certificados observadas devem estar no `https://myVaultName.vault.azure.net/secrets/myCertName`formato.
> 
> Isso ocorre porque o `/secrets` caminho retorna o certificado completo, incluindo a chave privada, enquanto o `/certificates` caminho não tem. Mais informações sobre certificados podem ser encontradas aqui: [Key Vault certificados](https://docs.microsoft.com/azure/key-vault/about-keys-secrets-and-certificates#key-vault-certificates)

### <a name="property-values"></a>Valores de propriedade

| Name | Valor/Exemplo | Tipo de Dados |
| ---- | ---- | ---- |
| apiVersion | 2019-07-01 | date |
| editor | Microsoft.Azure.KeyVault | cadeia de caracteres |
| type | KeyVaultForWindows | cadeia de caracteres |
| typeHandlerVersion | 1.0 | INT |
| pollingIntervalInS | 3600 | cadeia de caracteres |
| certificateStoreName | MY | cadeia de caracteres |
| linkOnRenewal | false | booleano |
| certificateStoreLocation  | LocalMachine | cadeia de caracteres |
| requiredInitialSync | true | booleano |
| observedCertificates  | ["https://myvault.vault.azure.net/secrets/mycertificate"] | Matriz de cadeia de caracteres


## <a name="template-deployment"></a>Implantação de modelo

Extensões de VM do Azure podem ser implantadas com modelos do Azure Resource Manager. Modelos são ideais ao implantar uma ou mais máquinas virtuais que exigem renovação de certificados pós-implantação. A extensão pode ser implantada em VMs individuais ou conjuntos de dimensionamento de máquinas virtuais. O esquema e a configuração são comuns a ambos os tipos de modelo. 

A configuração JSON para uma extensão de máquina virtual deve ser aninhada dentro do fragmento de recurso de máquina virtual do `"resources": []` modelo, especificamente objeto para o modelo de máquina virtual e, no caso do `"virtualMachineProfile":"extensionProfile":{"extensions" :[]` conjunto de dimensionamento de máquinas virtuais em objeto.

```json
    {
      "type": "Microsoft.Compute/virtualMachines/extensions",
      "name": "KeyVaultForWindows",
      "apiVersion": "2019-07-01",
      "location": "<location>",
      "dependsOn": [
          "[concat('Microsoft.Compute/virtualMachines/', <vmName>)]"
      ],
      "properties": {
      "publisher": "Microsoft.Azure.KeyVault",
      "type": "KeyVaultForWindows",
      "typeHandlerVersion": "1.0",
      "autoUpgradeMinorVersion": true,
      "settings": {
        "secretsManagementSettings": {
          "pollingIntervalInS": <polling interval in seconds, e.g: "3600">,
          "certificateStoreName": <certificate store name, e.g.: "MY">,
          "certificateStoreLocation": <certificate store location, currently it works locally only e.g.: "LocalMachine">,
          "observedCertificates": <list of KeyVault URIs representing monitored certificates, e.g.: "https://myvault.vault.azure.net/secrets/mycertificate"
        }      
      }
      }
    }
```


## <a name="azure-powershell-deployment"></a>Implantação do Azure PowerShell

O Azure PowerShell pode ser usado para implantar a extensão da VM do Diagnóstico de Key Vault em uma máquina virtual existente ou em um conjunto de dimensionamento de máquina virtual. 

* Para implantar a extensão em uma VM:
    
    ```powershell
        # Build settings
        $settings = '{"secretsManagementSettings": 
        { "pollingIntervalInS": "' + <pollingInterval> + 
        '", "certificateStoreName": "' + <certStoreName> + 
        '", "certificateStoreLocation": "' + <certStoreLoc> + 
        '", "observedCertificates": ["' + <observedCerts> + '"] } }'
        $extName =  "KeyVaultForWindows"
        $extPublisher = "Microsoft.Azure.KeyVault"
        $extType = "KeyVaultForWindows"
       
    
        # Start the deployment
        Set-AzVmExtension -TypeHandlerVersion "1.0" -ResourceGroupName <ResourceGroupName> -Location <Location> -VMName <VMName> -Name $extName -Publisher $extPublisher -Type $extType -SettingString $settings
    
    ```

* Para implantar a extensão em um conjunto de dimensionamento de máquinas virtuais:

    ```powershell
    
        # Build settings
        $settings = '{"secretsManagementSettings": 
        { "pollingIntervalInS": "' + <pollingInterval> + 
        '", "certificateStoreName": "' + <certStoreName> + 
        '", "certificateStoreLocation": "' + <certStoreLoc> + 
        '", "observedCertificates": ["' + <observedCerts> + '"] } }'
        $extName = "KeyVaultForWindows"
        $extPublisher = "Microsoft.Azure.KeyVault"
        $extType = "KeyVaultForWindows"
        
        # Add Extension to VMSS
        $vmss = Get-AzVmss -ResourceGroupName <ResourceGroupName> -VMScaleSetName <VmssName>
        Add-AzVmssExtension -VirtualMachineScaleSet $vmss  -Name $extName -Publisher $extPublisher -Type $extType -TypeHandlerVersion "1.0" -Setting $settings

        # Start the deployment
        Update-AzVmss -ResourceGroupName <ResourceGroupName> -VMScaleSetName <VmssName> -VirtualMachineScaleSet $vmss 
    
    ```

## <a name="azure-cli-deployment"></a>Implantação da CLI do Azure

O CLI do Azure pode ser usado para implantar a extensão de VM Key Vault em uma máquina virtual existente ou em um conjunto de dimensionamento de máquinas virtuais. 
 
* Para implantar a extensão em uma VM:
    
    ```azurecli
       # Start the deployment
         az vm extension set -n "KeyVaultForWindows" `
         --publisher Microsoft.Azure.KeyVault `
         -g "<resourcegroup>" `
         --vm-name "<vmName>" `
         --settings '{\"secretsManagementSettings\": { \"pollingIntervalInS\": \"<pollingInterval>\", \"certificateStoreName\": \"<certStoreName>\", \"certificateStoreLocation\": \"<certStoreLoc>\", \"observedCertificates\": [\ <observedCerts>\"] }}'
    ```

* Para implantar a extensão em um conjunto de dimensionamento de máquinas virtuais:

   ```azurecli
        # Start the deployment
        az vmss extension set -n "KeyVaultForWindows" `
         --publisher Microsoft.Azure.KeyVault `
         -g "<resourcegroup>" `
         --vmss-name "<vmName>" `
         --settings '{\"secretsManagementSettings\": { \"pollingIntervalInS\": \"<pollingInterval>\", \"certificateStoreName\": \"<certStoreName>\", \"certificateStoreLocation\": \"<certStoreLoc>\", \"observedCertificates\": [\ <observedCerts>\"] }}'
    ```

Por favor esteja ciente das seguintes restrições/exigências:
- Restrições de Key Vault:
  - Ele deve existir no momento da implantação 
  - A política de acesso Key Vault está definida para a identidade VM/VMSS usando o MSI


## <a name="troubleshoot-and-support"></a>Solução de problemas e suporte

### <a name="troubleshoot"></a>Solucionar problemas

Os dados sobre o estado das implantações de extensão podem ser recuperados no Portal do Azure usando o Azure PowerShell. Para ver o estado da implantação das extensões de uma determinada VM, execute o comando a seguir usando o Azure PowerShell.

## <a name="azure-powershell"></a>Azure PowerShell
```powershell
Get-AzVMExtension -VMName <vmName> -ResourceGroupname <resource group name>
```

## <a name="azure-cli"></a>CLI do Azure
```azurecli
 az vm get-instance-view --resource-group <resource group name> --name  <vmName> --query "instanceView.extensions"
```

A saída de execução da extensão é registrada no seguinte arquivo:

```
%windrive%\WindowsAzure\Logs\Plugins\Microsoft.Azure.KeyVault.KeyVaultForWindows\<version>\akvvm_service_<date>.log
```


### <a name="support"></a>Suporte

Se precisar de mais ajuda a qualquer momento neste artigo, você poderá entrar em contato com os especialistas do Azure nos [fóruns do Azure e do Stack Overflow do MSDN](https://azure.microsoft.com/support/forums/). Como alternativa, você pode registrar um incidente de suporte do Azure. Vá para o [site de suporte do Azure](https://azure.microsoft.com/support/options/) e selecione obter suporte. Para saber mais sobre como usar o suporte do Azure, leia as [Perguntas frequentes sobre o suporte do Microsoft Azure](https://azure.microsoft.com/support/faq/).

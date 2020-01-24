---
title: Sobre a rede na recuperação de desastre da VM do Azure com o Azure Site Recovery
description: Fornece uma visão geral da rede para a replicação de VMs do Azure usando o Azure Site Recovery.
services: site-recovery
author: sujayt
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 1/8/2020
ms.author: sutalasi
ms.openlocfilehash: 9fe3b4c0b7acc9c1e980d5885043d30503c211c4
ms.sourcegitcommit: 380e3c893dfeed631b4d8f5983c02f978f3188bf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/08/2020
ms.locfileid: "75754487"
---
# <a name="about-networking-in-azure-vm-disaster-recovery"></a>Sobre a rede na recuperação de desastre da VM do Azure



Este artigo fornece diretrizes de rede para quando você está replicando e recuperando VMs do Azure de uma região para outra usando o [Azure Site Recovery](site-recovery-overview.md).

## <a name="before-you-start"></a>Antes de começar

Saiba como o Site Recovery fornece recuperação de desastre para [esse cenário](azure-to-azure-architecture.md).

## <a name="typical-network-infrastructure"></a>Infraestrutura de rede típica

O diagrama a seguir ilustra um ambiente típico do Azure para aplicativos em execução em VMs do Azure:

![ambiente do cliente](./media/site-recovery-azure-to-azure-architecture/source-environment.png)

Caso você esteja usando o Azure ExpressRoute ou uma conexão VPN na rede local com o Azure, o ambiente terá a seguinte aparência:

![ambiente do cliente](./media/site-recovery-azure-to-azure-architecture/source-environment-expressroute.png)

As redes geralmente são protegidas usando firewalls e NSGs (grupos de segurança de rede). Os firewalls usam lista branca baseada em IP ou em URL para controlar a conectividade de rede. Os NSGs fornecem regras que usam intervalos de endereços IP para controlar a conectividade de rede.

>[!IMPORTANT]
> O uso de um proxy autenticado para controlar a conectividade de rede não é compatível com o Site Recovery e a replicação não pode ser habilitada.


## <a name="outbound-connectivity-for-urls"></a>Conectividade de saída para URLs

Se você está usando um proxy de firewall baseado em URL para controlar a conectividade de saída, permita estas URLs do Site Recovery:


**URL** | **Detalhes**  
--- | ---
*.blob.core.windows.net | Necessário para que os dados possam ser gravados para a conta de armazenamento de cache da região de origem da VM. Se você souber todas as contas de armazenamento em cache para suas VMs, poderá permitir o acesso às URLs de conta de armazenamento específicas (ex: cache1.blob.core.windows.net e cache2.blob.core.windows.net) em vez de *. blob.core.windows.net
login.microsoftonline.com | Necessário para autorização e autenticação para as URLs do serviço de recuperação de Site.
*.hypervrecoverymanager.windowsazure.com | Necessário para que a comunicação de serviço de recuperação de Site possa ocorrer da VM. Você pode usar o ' Site Recovery IP ' correspondente se o proxy de firewall oferecer suporte a IPs.
*.servicebus.windows.net | Necessário para que os dados de monitoramento e diagnóstico de recuperação de Site possam ser gravados da VM. Você pode usar o ' Site Recovery Monitoring IP ' correspondente se o proxy de firewall oferecer suporte a IPs.

## <a name="outbound-connectivity-for-ip-address-ranges"></a>Conectividade de saída para intervalos de endereços IP

Se você estiver usando um proxy de firewall baseado em IP ou NSG para controlar a conectividade de saída, esses intervalos de IP precisam ser permitidos.

- Todos os intervalos de endereços IP que correspondem às contas de armazenamento na região de origem
    - Crie uma [marcação de serviço de armazenamento](../virtual-network/security-overview.md#service-tags) com base na regra NSG para a região de origem.
    - Permita esses endereços para que os dados possam ser gravados da VM para a conta de armazenamento de cache.
- Criar uma [marca de serviço do Azure Active Directory (AAD)](../virtual-network/security-overview.md#service-tags) com base em regra NSG para permitir o acesso a todos os endereços IP correspondente para o AAD
    - Se novos endereços são adicionados no futuro para o Azure Active Directory (AAD) você precisará criar novas regras NSG.
- Crie uma regra de NSG baseada na marca de serviço EventsHub para a região de destino, permitindo o acesso ao monitoramento de Site Recovery.
- Crie uma regra de NSG baseada na marca de serviço AzureSiteRecovery para permitir acesso ao Site Recovery Service em qualquer região.
- É recomendável que você crie as regras de NSG necessárias em um NSG de teste e verifique se não há nenhum problema antes de criar as regras em um NSG de produção.


Se preferir usar Site Recovery intervalos de endereços IP (não recomendado), veja a tabela abaixo:

   **Target (destino)** | **IP do Site Recovery** |  **Monitoramento de Recuperação de site IP**
   --- | --- | ---
   Leste da Ásia | 52.175.17.132 | 13.94.47.61
   Sudeste Asiático | 52.187.58.193 | 13.76.179.223
   Índia Central | 52.172.187.37 | 104.211.98.185
   Sul da Índia | 52.172.46.220 | 104.211.224.190
   Centro-Norte dos EUA | 23.96.195.247 | 168.62.249.226
   Europa Setentrional | 40.69.212.238 | 52.169.18.8
   Oeste da Europa | 52.166.13.64 | 40.68.93.145
   Leste dos EUA | 13.82.88.226 | 104.45.147.24
   Oeste dos EUA | 40.83.179.48 | 104.40.26.199
   Centro-Sul dos EUA | 13.84.148.14 | 104.210.146.250
   EUA Central | 40.69.144.231 | 52.165.34.144
   Leste dos EUA 2 | 52.184.158.163 | 40.79.44.59
   Leste do Japão | 52.185.150.140 | 138.91.1.105
   Oeste do Japão | 52.175.146.69 | 138.91.17.38
   Sul do Brasil | 191.234.185.172 | 23.97.97.36
   Austrália Oriental | 104.210.113.114 | 191.239.64.144
   Sudeste da Austrália | 13.70.159.158 | 191.239.160.45
   Canadá Central | 52.228.36.192 | 40.85.226.62
   Leste do Canadá | 52.229.125.98 | 40.86.225.142
   Centro-Oeste dos EUA | 52.161.20.168 | 13.78.149.209
   Oeste dos EUA 2 | 52.183.45.166 | 13.66.228.204
   Oeste do Reino Unido | 51.141.3.203 | 51.141.14.113
   Sul do Reino Unido | 51.140.43.158 | 51.140.189.52
   Sul do Reino Unido 2 | 13.87.37.4| 13.87.34.139
   Norte do Reino Unido | 51.142.209.167 | 13.87.102.68
   Coreia Central | 52.231.28.253 | 52.231.32.85
   Sul da Coreia | 52.231.198.185 | 52.231.200.144
   França Central | 52.143.138.106 | 52.143.136.55
   Sul da França | 52.136.139.227 |52.136.136.62
   Austrália central| 20.36.34.70 | 20.36.46.142
   Austrália Central 2| 20.36.69.62 | 20.36.74.130
   Oeste da África do Sul | 102.133.72.51 | 102.133.26.128
   Norte da África do Sul | 102.133.160.44 | 102.133.154.128
   US Gov - Virgínia | 52.227.178.114 | 23.97.0.197
   US Gov - Iowa | 13.72.184.23 | 23.97.16.186
   US Gov - Arizona | 52.244.205.45 | 52.244.48.85
   US Gov - Texas | 52.238.119.218 | 52.238.116.60
   US DoD Leste | 52.181.164.103 | 52.181.162.129
   US DoD Central | 52.182.95.237 | 52.182.90.133
   Norte da China | 40.125.202.254 | 42.159.4.151
   Norte da China 2 | 40.73.35.193 | 40.73.33.230
   Leste da China | 42.159.205.45 | 42.159.132.40
   Leste da China 2 | 40.73.118.52| 40.73.100.125
   Norte da Alemanha| 51.116.208.58| 51.116.58.128
   Centro-oeste da Alemanha | 51.116.156.176 | 51.116.154.192
   Oeste da Suíça | 51.107.231.223| 51.107.154.128
   Norte da Suíça | 51.107.68.31| 51.107.58.128
   Leste da Noruega | 51.120.100.64| 51.120.98.128
   Oeste da Noruega | 51.120.220.65| 51.120.218.160

## <a name="example-nsg-configuration"></a>Exemplo de Configuração do NSG

Este exemplo mostra como configurar regras de NSG para uma VM a ser replicada.

- Se você está usando regras de NSG para controlar a conectividade de saída, use regras de "Permitir HTTPS de saída" para a port:443 para todos os intervalos de endereços IP necessários.
- O exemplo presume que o local de origem da VM é "Leste dos EUA" e que o local de destino é "EUA Central".

### <a name="nsg-rules---east-us"></a>Regras de NSG – Leste dos EUA

1. Crie uma regra de segurança HTTPS (443) de saída para "Storage.EastUS" no NSG conforme mostrado na captura de tela abaixo.

      ![storage-tag](./media/azure-to-azure-about-networking/storage-tag.png)

2. Crie uma regra de segurança HTTPS (443) de saída para "AzureActiveDirectory" no NSG conforme mostrado na captura de tela abaixo.

      ![aad-tag](./media/azure-to-azure-about-networking/aad-tag.png)

3. Semelhante às regras de segurança acima, crie a regra de segurança HTTPS de saída (443) para "EventHub. Centralus" no NSG que corresponde ao local de destino. Isso permite o acesso ao monitoramento de Site Recovery.

4. Crie uma regra de segurança HTTPS (443) de saída para "AzureSiteRecovery" no NSG. Isso permite o acesso ao Site Recovery Service em qualquer região.

### <a name="nsg-rules---central-us"></a>Regras de NSG – EUA Central

Essas regras são necessárias para que a replicação possa ser ativada da região de destino para a região de origem após o failover:

1. Crie uma regra de segurança HTTPS (443) de saída para "Storage.CentralUS" no NSG.

2. Crie uma regra de segurança HTTPS (443) de saída para "AzureActiveDirectory" no NSG.

3. Semelhante às regras de segurança acima, crie a regra de segurança HTTPS de saída (443) para "EventHub. Eastus" no NSG que corresponde ao local de origem. Isso permite o acesso ao monitoramento de Site Recovery.

4. Crie uma regra de segurança HTTPS (443) de saída para "AzureSiteRecovery" no NSG. Isso permite o acesso ao Site Recovery Service em qualquer região.

## <a name="network-virtual-appliance-configuration"></a>Configuração da solução de virtualização de rede

Se estiver usando NVAs (soluções de virtualização de rede) para controlar o tráfego de saída de rede das VMs, a solução poderá ficar limitada se todo o tráfego da replicação passar pela NVA. Recomendamos criar um ponto de extremidade de serviço de rede em sua rede virtual para "Armazenamento", para que o tráfego da replicação não acesse a NVA.

### <a name="create-network-service-endpoint-for-storage"></a>Criar ponto de extremidade de serviço de rede para Armazenamento
É possível criar um ponto de extremidade de serviço de rede em sua rede virtual para "Armazenamento" para que o tráfego da replicação não saia do limite do Azure.

- Selecione sua Rede Virtual do Azure e clique em 'Pontos de extremidade de serviço'

    ![storage-endpoint](./media/azure-to-azure-about-networking/storage-service-endpoint.png)

- Clique em 'Adicionar' e a guia 'Adicionar pontos de extremidade de serviço' será aberta
- Selecione 'Microsoft.Storage' em 'Serviço' e as sub-redes necessárias no campo 'Sub-redes' e clique em 'Adicionar'

>[!NOTE]
>Não restrinja o acesso à rede virtual às suas contas de armazenamento usadas para a ASR. Você deve permitir o acesso de 'Todas as redes'

### <a name="forced-tunneling"></a>Túnel forçado

Você pode substituir a rota de sistema padrão do Azure para o prefixo de endereço 0.0.0.0/0 com um [rota personalizados](../virtual-network/virtual-networks-udr-overview.md#custom-routes) e desviar o tráfego VM para uma solução de virtualização de rede (NVA) local, mas essa configuração não é recomendada para a recuperação de Site replicação. Se você estiver usando rotas personalizadas, deverá [criar um ponto de extremidade de serviço de rede virtual](azure-to-azure-about-networking.md#create-network-service-endpoint-for-storage) em sua rede virtual para "Armazenamento" para que o tráfego de replicação não saia do limite do Azure.

## <a name="next-steps"></a>Próximos passos
- Inicie a proteção de suas cargas de trabalho ao [replicar máquinas virtuais do Azure](site-recovery-azure-to-azure.md).
- Saiba mais sobre [retenção de endereço IP](site-recovery-retain-ip-azure-vm-failover.md) para failover de máquina virtual do Azure.
- Saiba mais sobre a recuperação de desastre de [máquinas virtuais do Azure com o ExpressRoute](azure-vm-disaster-recovery-with-expressroute.md).

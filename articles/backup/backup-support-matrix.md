---
title: Matriz de suporte do Backup do Azure
description: Fornece um resumo de configurações compatíveis e limitações do serviço de Backup do Azure.
ms.topic: conceptual
ms.date: 02/17/2019
ms.openlocfilehash: d036e527880a98d323e8de2f3a8721d7e12dbb07
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79273261"
---
# <a name="support-matrix-for-azure-backup"></a>Matriz de suporte para o backup do Azure

Você pode usar o [backup do Azure](backup-overview.md) para fazer backup de dados para a plataforma de nuvem Microsoft Azure. Este artigo resume as configurações de suporte geral e as limitações para cenários e implantações de backup do Azure.

Outras matrizes de suporte estão disponíveis:

- Matriz de suporte para [backup de VM (máquina virtual) do Azure](backup-support-matrix-iaas.md)
- Matriz de suporte para backup usando [o System Center Data Protection Manager (DPM)/Microsoft servidor de backup do Azure (mAbs)](backup-support-matrix-mabs-dpm.md)
- Matriz de suporte para backup usando o [agente de serviços de recuperação do Microsoft Azure (MARS)](backup-support-matrix-mars-agent.md)

[!INCLUDE [azure-lighthouse-supported-service](../../includes/azure-lighthouse-supported-service.md)]

## <a name="vault-support"></a>Suporte de cofre

O backup do Azure usa cofres dos serviços de recuperação para orquestrar e gerenciar backups. Ele também usa cofres para armazenar dados de backup.

A tabela a seguir descreve os recursos dos cofres dos serviços de recuperação:

**Recurso** | **Detalhes**
--- | ---
**Cofres na assinatura** | Até 500 cofres dos Serviços de Recuperação em uma assinatura única.
**Computadores em um cofre** | Até 1.000 VMs do Azure em um único cofre.<br/><br/> Até 50 servidores MABS podem ser registrados em um único cofre.
**Fontes de dados** | O tamanho máximo de uma [fonte de dados](https://docs.microsoft.com/azure/backup/backup-azure-backup-faq#how-is-the-data-source-size-determined) individual é de 54.400 GB. Esse limite não se aplica aos backups de VM do Azure. Nenhum limite se aplica à quantidade total de dados que você pode fazer backup para o cofre.
**Backups no cofre** | **VMs do Azure:** Uma vez por dia.<br/><br/>**Computadores protegidos pelo DPM/mAbs:** Duas vezes por dia.<br/><br/> Backup de **máquinas diretamente usando o agente Mars:** Três vezes por dia.
**Backups entre cofres** | O backup é dentro de uma região.<br/><br/> Você precisa de um cofre em cada região do Azure que contenha VMs que você deseja fazer backup. Você não pode fazer backup em uma região diferente.
**Mover cofres** | Você pode [mover cofres](https://docs.microsoft.com/azure/backup/backup-azure-move-recovery-services-vault) entre assinaturas ou entre grupos de recursos na mesma assinatura. No entanto, não há suporte para a movimentação de cofres entre regiões.
**Mover dados entre cofres** | Não há suporte para a movimentação de dados de backup entre cofres.
**Modificar o tipo de armazenamento do cofre** | Você pode modificar o tipo de replicação de armazenamento (armazenamento com redundância geográfica ou armazenamento com redundância local) para um cofre antes de os backups serem armazenados. Após o início dos backups no cofre, o tipo de replicação não poderá ser modificado.

## <a name="on-premises-backup-support"></a>Suporte de backup local

Veja os que têm suporte se você quiser fazer backup de computadores locais:

**Computador** | **O que é feito backup** | **Localidade** | **Recursos**
--- | --- | --- | ---
**Backup direto de computador Windows com o agente MARS** | Arquivos, pastas, estado do sistema | Faça backup no cofre dos serviços de recuperação. | Fazer backup três vezes por dia<br/><br/> Nenhum backup com reconhecimento de aplicativo<br/><br/> Restaurar arquivo, pasta, volume
**Backup direto de computador Linux com o agente MARS** | Backup sem suporte
**Fazer backup no DPM** | Arquivos, pastas, volumes, estado do sistema, dados de aplicativo | Fazer backup em armazenamento local do DPM. Em seguida, o DPM faz backup no cofre. | Instantâneos com reconhecimento de aplicativo<br/><br/> Granularidade total para backup e recuperação<br/><br/> Linux com suporte para VMs (Hyper-V/VMware)<br/><br/> Oracle sem suporte
**Fazer backup em MABS** | Arquivos, pastas, volumes, estado do sistema, dados de aplicativo | Fazer backup no armazenamento local do MABS. Em seguida, o MABS faz backup no cofre. | Instantâneos com reconhecimento de aplicativo<br/><br/> Granularidade total para backup e recuperação<br/><br/> Linux com suporte para VMs (Hyper-V/VMware)<br/><br/> Oracle sem suporte

## <a name="azure-vm-backup-support"></a>Suporte de backup de VM do Azure

### <a name="azure-vm-limits"></a>Limites de VM do Azure

**Limite** | **Detalhes**
--- | ---
**Discos de dados de VM do Azure** | Limite de 16 <br> Para se inscrever para a versão prévia privada de VMs com mais de 16 discos (até 32 discos), escreva para nós em AskAzureBackupTeam@microsoft.com
**Tamanho do disco de dados da VM do Azure** | O tamanho do disco individual pode ter até 32 TB e um máximo de 256 TB combinados para todos os discos em uma VM.

### <a name="azure-vm-backup-options"></a>Opções de backup de VM do Azure

Aqui estão as suportadas se você quiser fazer backup de VMs do Azure:

**Computador** | **O que é feito backup** | **Localidade** | **Recursos**
--- | --- | --- | ---
**Backup de VM do Azure usando a extensão de VM** | Toda a VM | Faça backup no cofre. | Extensão instalada quando você habilita o backup de uma VM.<br/><br/> Fazer backup uma vez por dia.<br/><br/> Backup com reconhecimento de aplicativo para VMs do Windows; backup consistente com o arquivo para VMs Linux. Você pode configurar a consistência de aplicativos para computadores Linux usando scripts personalizados.<br/><br/> Restaure a VM ou o disco.<br/><br/> Não é possível fazer backup de uma VM do Azure em uma localização local.
**Backup de VM do Azure usando o agente MARS** | Arquivos, pastas, estado do sistema | Faça backup no cofre. | Faça backup três vezes por dia.<br/><br/> Se você quiser fazer backup de arquivos ou pastas específicas em vez de toda a VM, o agente MARS poderá ser executado junto com a extensão de VM.
**VM do Azure com o DPM** | Arquivos, pastas, volumes, estado do sistema, dados de aplicativo | Faça backup no armazenamento local da VM do Azure que está executando o DPM. Em seguida, o DPM faz backup no cofre. | Instantâneos com reconhecimento de aplicativo.<br/><br/> Granularidade completa de backup e recuperação.<br/><br/> Com suporte do Linux para VMs (Hyper-V/VMware).<br/><br/> Sem suporte para o Oracle.
**VM do Azure com o MABS** | Arquivos, pastas, volumes, estado do sistema, dados de aplicativo | Faça backup no armazenamento local da VM do Azure que está executando o MABS. Em seguida, o MABS faz backup no cofre. | Instantâneos com reconhecimento de aplicativo.<br/><br/> Granularidade completa de backup e recuperação.<br/><br/> Com suporte do Linux para VMs (Hyper-V/VMware).<br/><br/> Sem suporte para o Oracle.

## <a name="linux-backup-support"></a>Suporte de backup do Linux

Veja os que têm suporte se você quiser fazer backup de computadores Linux:

**Tipo de backup** | **Linux (endossado pelo Azure)**
--- | ---
**Backup direto do computador local que está executando o Linux** | Sem suporte. O agente MARS só pode ser instalado em computadores Windows.
**Usando a extensão do agente para fazer backup da VM do Azure que está executando o Linux** | Backup consistente com o aplicativo usando [scripts personalizados](backup-azure-linux-app-consistent.md).<br/><br/> Recuperação em nível de arquivo.<br/><br/> Faça a restauração com a criação de uma VM de um ponto de recuperação ou um disco.
**Usando o DPM para fazer backup de computadores locais executando o Linux** | Backup consistente com o arquivo de VMs de convidado do Linux no Hyper-V e no VMWare.<br/><br/> Restauração de VM de VMs do Hyper-V e do VMWare Linux convidadas.
**Usando o MABS para fazer backup de computadores locais executando o Linux** | Backup consistente com o arquivo de VMs de convidado do Linux no Hyper-V e no VMWare.<br/><br/> Restauração de VM de VMs do Hyper-V e do VMWare Linux convidadas.
**Usando o MABS ou o DPM para fazer backup de VMs do Linux Azure** | Sem suporte.

## <a name="daylight-saving-time-support"></a>Suporte ao horário de verão

O backup do Azure não dá suporte ao ajuste de relógio automático para o horário de verão para backups de VM do Azure. Ele não desloca a hora do backup para frente ou para trás. Para garantir que o backup seja executado no momento desejado, modifique as políticas de backup manualmente, conforme necessário.

## <a name="disk-deduplication-support"></a>Suporte de eliminação de duplicação de disco

O suporte de eliminação de duplicação de disco ocorre da seguinte maneira:

- A eliminação de duplicação de disco tem suporte no local quando você usa o DPM ou o MABs para fazer backup de VMs do Hyper-V que executam o Windows. O Windows Server executa a eliminação de duplicação de dados (no nível do host) em discos rígidos virtuais (VHDs) anexados à VM como armazenamento de backup.
- Não há suporte para a eliminação de duplicação no Azure para qualquer componente de Backup. Quando o DPM e o MABS são implantados no Azure, os discos de armazenamento anexados à VM não podem ter eliminação de duplicação.

## <a name="security-and-encryption-support"></a>Suporte de segurança e criptografia

O backup do Azure dá suporte à criptografia para dados em trânsito e em repouso.

### <a name="network-traffic-to-azure"></a>Tráfego de rede para o Azure

- O tráfego de backup de servidores para o cofre dos serviços de recuperação é criptografado usando o criptografia AES 256.
- Os dados de backup são enviados por um link HTTPS seguro.
- Os dados de backup são armazenados no cofre dos serviços de recuperação em formato criptografado.
- Só você tem a senha para desbloquear esses dados. A Microsoft não pode descriptografar os dados de backup em momento algum.

    > [!WARNING]
    > Depois de configurar o cofre, só você terá acesso à chave de criptografia. A Microsoft nunca mantém uma cópia e não tem acesso à chave. Se a chave for perdida, a Microsoft não poderá recuperar os dados de backup.

### <a name="data-security"></a>Segurança de dados

- Quando estiver fazendo backup de VMs do Azure, você precisará configurar a criptografia *na* máquina virtual.
- O Backup do Azure dá suporte a Azure Disk Encryption, que usa o BitLocker em máquinas virtuais Windows e **dm-crypt** em máquinas virtuais Linux.
- No back-end, o backup do Azure usa o [azure criptografia do serviço de armazenamento](../storage/common/storage-service-encryption.md), que protege os dados em repouso.

**Computador** | **Em trânsito** | **Em repouso**
--- | --- | ---
**Computadores Windows locais sem o DPM/MABS** | ![Sim][green] | ![Sim][green]
**VMs do Azure** | ![Sim][green] | ![Sim][green]
**Máquinas Windows locais ou VMs do Azure com o DPM** | ![Sim][green] | ![Sim][green]
**Máquinas Windows locais ou VMs do Azure com MABS** | ![Sim][green] | ![Sim][green]

## <a name="compression-support"></a>Suporte à compactação

O backup dá suporte à compactação de tráfego de backup, conforme resumido na tabela a seguir.

- Para VMs do Azure, a extensão de VM lê os dados diretamente da conta de armazenamento do Azure pela rede de armazenamento, portanto, não é necessário compactar esse tráfego.
- Se você estiver usando o DPM ou o MABS, poderá economizar largura de banda compactando os dados antes de fazer backup.

**Computador** | **Compactar no DPM/MABS (TCP)** | **Compactar para o cofre (HTTPS)**
--- | --- | ---
**Backup direto em computadores locais do Windows** | NA | ![Sim][green]
**Backup de VMs do Azure usando a extensão de VM** | NA | NA
**Backup em computadores locais/do Azure usando o MABS/DPM** | ![Sim][green] | ![Sim][green]

## <a name="retention-limits"></a>Limites de retenção

**Configuração** | **Limites**
--- | ---
**Máximo de pontos de recuperação por instância protegida (máquina ou carga de trabalho)** | 9\.999
**Tempo máximo de expiração para um ponto de recuperação** | Sem limite
**Frequência máxima de backup para o DPM/MABS** | A cada 15 minutos para o SQL Server<br/><br/> Uma vez por hora para outras cargas de trabalho
**Frequência máxima de backup para o cofre** | **Máquinas Windows locais ou VMS do Azure executando Mars:** Três por dia<br/><br/> **DPM/mAbs:** Dois por dia<br/><br/> **Backup de VM do Azure:** Um por dia
**Retenção do ponto de recuperação** | Diária, semanal, mensal, anual
**Período de retenção máximo** | Depende da frequência de backup
**Pontos de recuperação no disco do DPM/MABS** | 64 para servidores de arquivos; 448 para servidores de aplicativos <br/><br/>Pontos de recuperação de fita ilimitados para o DPM local

## <a name="cross-region-restore"></a>Restauração entre regiões

O backup do Azure adicionou o recurso de restauração entre regiões para reforçar a disponibilidade de dados e a capacidade de resiliência, dando aos clientes controle total para restaurar dados em uma região secundária. Para configurar esse recurso, visite [o artigo definir a restauração entre regiões.](backup-create-rs-vault.md#set-cross-region-restore) Esse recurso tem suporte para os seguintes tipos de gerenciamento:

| Tipo de gerenciamento de backup | Suportado                                                    | Regiões com Suporte |
| ---------------------- | ------------------------------------------------------------ | ----------------- |
| VM do Azure               | Sim. Versão prévia limitada pública com suporte para VMs criptografadas e VMs com menos de 4 TB de disco | Centro-Oeste dos EUA   |
| Agente MARS/local | Não                                                           | N/D               |
| SQL/SCS HANA          | Não                                                           | N/D               |
| AFS                    | Não                                                           | N/D               |

## <a name="next-steps"></a>Próximas etapas

- [Examinar matriz de suporte](backup-support-matrix-iaas.md) para backup de VM do Azure.

[green]: ./media/backup-support-matrix/green.png
[yellow]: ./media/backup-support-matrix/yellow.png
[red]: ./media/backup-support-matrix/red.png

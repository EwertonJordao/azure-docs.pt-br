---
title: Perguntas frequentes-fazendo backup de VMs do Azure
description: Neste artigo, descubra respostas para perguntas comuns sobre como fazer backup de VMs do Azure com o serviço de backup do Azure.
ms.reviewer: sogup
ms.topic: conceptual
ms.date: 09/17/2019
ms.openlocfilehash: d70f4832daba59739d6798517902e921927194d6
ms.sourcegitcommit: 7221918fbe5385ceccf39dff9dd5a3817a0bd807
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/21/2020
ms.locfileid: "76293973"
---
# <a name="frequently-asked-questions-back-up-azure-vms"></a>Perguntas frequentes-fazer backup de VMs do Azure

Este artigo responde a perguntas comuns sobre o backup de VMs do Azure com o serviço de [backup do Azure](backup-introduction-to-azure-backup.md) .

## <a name="backup"></a>Backup

### <a name="which-vm-images-can-be-enabled-for-backup-when-i-create-them"></a>Quais imagens de VM podem ser habilitadas para backup quando eu as crio?

Ao criar uma VM, você pode habilitar o backup para VMs que executam [sistemas operacionais com suporte](backup-support-matrix-iaas.md#supported-backup-actions)

### <a name="is-the-backup-cost-included-in-the-vm-cost"></a>O custo de backup está incluído no custo da VM?

Não. Os custos de backup são separados dos custos de uma VM. Saiba mais sobre os [preços do backup do Azure](https://azure.microsoft.com/pricing/details/backup/).

### <a name="which-permissions-are-required-to-enable-backup-for-a-vm"></a>Quais permissões são necessárias para habilitar o backup de uma VM?

Se você for um colaborador de VM, poderá habilitar o backup na VM. Se você estiver usando uma função personalizada, precisará das seguintes permissões para habilitar o backup na VM:

- Microsoft.RecoveryServices/Vaults/write
- Microsoft.RecoveryServices/Vaults/read
- Microsoft.RecoveryServices/locations/*
- Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/*/read
- Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/read
- Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/write
- Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/write
- Microsoft.RecoveryServices/Vaults/backupPolicies/read
- Microsoft.RecoveryServices/Vaults/backupPolicies/write

Se o cofre e a VM dos serviços de recuperação tiverem grupos de recursos diferentes, verifique se você tem permissões de gravação no grupo de recursos para o cofre dos serviços de recuperação.  

### <a name="does-an-on-demand-backup-job-use-the-same-retention-schedule-as-scheduled-backups"></a>O trabalho de backup sob demanda usa o mesmo agendamento de retenção que os backups agendados?

Não. Especifique o período de retenção para um trabalho de backup sob demanda. Por padrão, ele é retido por 30 dias quando disparado de um portal.

### <a name="i-recently-enabled-azure-disk-encryption-on-some-vms-will-my-backups-continue-to-work"></a>Eu recentemente habilitei a Criptografia de Disco do Azure em algumas VMs. Meus backups continuarão a funcionar?

Forneça permissões para que o backup do Azure acesse Key Vault. Especifique as permissões no PowerShell, conforme descrito na seção **Habilitar backup** na documentação do [PowerShell do Backup do Azure](backup-azure-vms-automation.md).

### <a name="i-migrated-vm-disks-to-managed-disks-will-my-backups-continue-to-work"></a>Migrei os discos de uma VM para os discos gerenciados. Meus backups continuarão a funcionar?

Sim, os backups funcionam perfeitamente. Não é necessário reconfigurar nada.

### <a name="why-cant-i-see-my-vm-in-the-configure-backup-wizard"></a>Por que não consigo ver minha VM no assistente de Backup de Configuração?

O assistente lista apenas as VMs na mesma região do cofre e que ainda não estão sendo submetidas a backup.

### <a name="my-vm-is-shut-down-will-an-on-demand-or-a-scheduled-backup-work"></a>Minha VM está desligada. Será uma sob demanda ou um trabalho de backup agendado?

Sim. Os backups são executados quando um computador é desligado. O ponto de recuperação é marcado como consistente com a falha.

### <a name="can-i-cancel-an-in-progress-backup-job"></a>Posso cancelar um trabalho de backup em andamento?

Sim. Você poderá cancelar o trabalho de backup se ele estiver em um estado **Criar instantâneo**. Você não poderá cancelar um trabalho se a transferência de dados do instantâneo estiver em andamento.

### <a name="i-enabled-lock-on-resource-group-created-by-azure-backup-service-ie-azurebackuprg_geo_number-will-my-backups-continue-to-work"></a>Habilitei o bloqueio no grupo de recursos criado pelo serviço de backup do Azure (ou seja, `AzureBackupRG_<geo>_<number>`), meus backups continuarão a funcionar?

Se você bloquear o grupo de recursos criado pelo serviço de backup do Azure, os backups começarão a falhar, pois há um limite máximo de 18 pontos de restauração.

O usuário precisa remover o bloqueio e limpar a coleção de pontos de restauração desse grupo de recursos para que os backups futuros sejam bem-sucedidos, [siga estas etapas](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md#clean-up-restore-point-collection-from-azure-portal) para remover a coleção de pontos de restauração.

### <a name="does-azure-backup-support-standard-ssd-managed-disks"></a>O backup do Azure dá suporte a discos gerenciados por SSD padrão?

Sim, o backup do Azure dá suporte a [discos gerenciados SSD padrão](https://azure.microsoft.com/blog/announcing-general-availability-of-standard-ssd-disks-for-azure-virtual-machine-workloads/).

### <a name="can-we-back-up-a-vm-with-a-write-accelerator-wa-enabled-disk"></a>Podemos fazer backup de uma VM com um disco ativado pelo Acelerador de Gravação?

Os instantâneos não podem ser criados no disco habilitado pelo Acelerador de Gravação. No entanto, o serviço de Backup do Azure pode excluir o disco habilitado pelo Acelerador de Gravação do backup.

### <a name="i-have-a-vm-with-write-accelerator-wa-disks-and-sap-hana-installed-how-do-i-back-up"></a>Tenho uma VM com discos do Acelerador de Gravação e do SAP HANA instalados. Como fazer backup?

O Backup do Azure não pode fazer backup do disco habilitado pelo Acelerador de Gravação, mas pode excluí-lo do backup. No entanto, o backup não fornecerá a consistência do banco de dados porque não são feitos backups de informações do disco habilitado pelo Acelerador de Gravação. Você pode fazer backup de discos com essa configuração se desejar o backup em disco do sistema operacional e o backup dos discos que não são habilitados pelo Acelerador de Gravação.

Estamos executando a visualização privada para um backup de SAP HANA com um RPO de 15 minutos. Ele é criado de maneira semelhante ao backup do Banco de Dados do SQL e usa a interface backInt para soluções de terceiros certificadas pelo SAP HANA. Se você estiver interessado, envie um email para `AskAzureBackupTeam@microsoft.com` com o assunto **inscrever-se para a visualização privada para o backup de SAP Hana em VMs do Azure**.

### <a name="what-is-the-maximum-delay-i-can-expect-in-backup-start-time-from-the-scheduled-backup-time-i-have-set-in-my-vm-backup-policy"></a>Qual é o atraso máximo que posso esperar na hora de início do backup a partir do horário agendado, defini em minha política de backup da VM?

O backup agendado será disparado dentro de 2 horas da hora do backup agendado. Por exemplo, se as VMs 100 tiverem a hora de início do backup agendada às 2:00 AM, pelo máximo 4:00 de todas as 100 serão feitas com o trabalho de backup em andamento. Se os backups agendados tiverem sido pausados devido à interrupção e retomados/repetidos, o backup poderá ser iniciado fora dessa janela de duas horas agendada.

### <a name="what-is-the-minimum-allowed-retention-range-for-daily-backup-point"></a>Qual é o período de retenção mínimo permitido para o ponto de backup diário?

A política de backup de máquina virtual do Azure dá suporte a um período de retenção mínimo de sete dias até 9999 dias. Qualquer modificação em uma política de backup de VM existente com menos de sete dias exigirá uma atualização para atender ao período de retenção mínimo de sete dias.

### <a name="can-i-backup-or-restore-selective-disks-attached-to-a-vm"></a>Posso fazer backup ou restaurar discos seletivos anexados a uma VM?

O backup do Azure agora oferece suporte a backup e restauração de disco seletivo usando a solução de backup de máquina virtual do Azure.

Hoje, o backup do Azure dá suporte ao backup de todos os discos (sistema operacional e dados) em uma VM em conjunto usando a solução de backup de máquina virtual. Com a funcionalidade excluir disco, você obtém uma opção para fazer backup de um ou alguns dos vários discos de dados em uma VM. Isso fornece uma solução eficiente e econômica para suas necessidades de backup e restauração. Cada ponto de recuperação contém dados dos discos incluídos na operação de backup, o que permite que você tenha um subconjunto de discos restaurados do ponto de recuperação fornecido durante a operação de restauração. Isso se aplica à restauração tanto do instantâneo quanto do cofre.

Essa solução é particularmente útil nos seguintes cenários:
  
1. Você tem dados críticos para fazer backup em apenas um disco e não deseja fazer backup do restante dos discos anexados a uma VM. Isso minimiza os custos de armazenamento de backup.  
2. Você tem outras soluções de backup para parte dos dados da VM. Por exemplo, você faz o backup de seus bancos de dados ou de sua capacidade com uma solução de backup de carga de trabalho diferente e deseja usar o backup de nível de VM do Azure para o restante dos seus discos e dados para criar um sistema eficiente e robusto utilizando os melhores recursos disponíveis.

Para se inscrever na versão prévia, escreva para nós em AskAzureBackupTeam@microsoft.com

## <a name="restore"></a>Restaurar

### <a name="how-do-i-decide-whether-to-restore-disks-only-or-a-full-vm"></a>Como faço para decidir se quero restaurar somente a discos ou uma VM completa?

Pense na restauração da VM como uma opção de criação rápida para uma VM do Azure. Essa opção altera os nomes de disco, contêineres usados pelos discos, endereços IP públicos e nomes de interface de rede. A alteração mantém recursos exclusivos quando uma VM é criada. A VM não é adicionada ao conjunto de disponibilidade.

Você pode usar a opção de disco de restauração se você quer:

- Personalizar a VM que é criada. Por exemplo, altere o tamanho.
- Adicione definições de configuração que não estavam lá no momento do backup.
- Controlar a convenção de nomenclatura para os recursos que são criados.
- Adicionar a VM a um grupo de disponibilidade.
- Adicione qualquer outra configuração que deva ser configurada usando o PowerShell ou um modelo.

### <a name="can-i-restore-backups-of-unmanaged-vm-disks-after-i-upgrade-to-managed-disks"></a>Posso restaurar backups de discos de VMs não gerenciadas após o upgrade dos meus discos para discos gerenciados?

Sim, você pode usar os backups feitos antes de migrar os discos de não gerenciados para gerenciados.

### <a name="how-do-i-restore-a-vm-to-a-restore-point-before-the-vm-was-migrated-to-managed-disks"></a>Como faço para restaurar uma VM em um ponto de restauração antes que a VM seja migrada para os discos gerenciados?

O processo de restauração permanece o mesmo. Se o ponto de recuperação for de um ponto no tempo em que a VM tinha discos não gerenciados, você poderá [restaurar discos como não gerenciados](tutorial-restore-disk.md#unmanaged-disks-restore). Se a VM tivesse discos gerenciados, você pode [restaurar discos como discos gerenciados](tutorial-restore-disk.md#managed-disk-restore). Em seguida, você pode [criar uma VM a partir desses discos](tutorial-restore-disk.md#create-a-vm-from-the-restored-disk).

[Saiba mais](backup-azure-vms-automation.md#restore-an-azure-vm) sobre como fazer isso no PowerShell.

### <a name="can-i-restore-the-vm-thats-been-deleted"></a>Posso restaurar a VM que foi excluída?

Sim. Mesmo se você excluir a VM, poderá acessar o item de backup correspondente no cofre e restaurar a partir de um ponto de recuperação.

### <a name="how-to-restore-a-vm-to-the-same-availability-sets"></a>Como restaurar uma VM nos mesmos conjuntos de disponibilidade?

Para a VM do Azure do Disco Gerenciado, a restauração dos conjuntos de disponibilidade é ativada ao fornecer uma opção no modelo durante a restauração como discos gerenciados. Este modelo tem o parâmetro de entrada chamado **Conjuntos de disponibilidade**.

### <a name="how-do-we-get-faster-restore-performances"></a>Como conseguimos desempenhos de restauração mais rápidos?

A capacidade de [restauração instantânea](backup-instant-restore-capability.md) ajuda em backups mais rápidos e restaurações instantâneas a partir dos instantâneos.

### <a name="what-happens-when-we-change-the-key-vault-settings-for-the-encrypted-vm"></a>O que acontece quando alteramos as configurações do cofre de chaves para a VM criptografada?

Depois de alterar as configurações de keyvault para a VM criptografada, os backups continuarão a funcionar com o novo conjunto de detalhes. No entanto, após a restauração de um ponto de recuperação antes da alteração, você precisará restaurar os segredos em um keyvault antes de poder criar a VM a partir dele. Para obter mais informações, consulte este [artigo](https://docs.microsoft.com/azure/backup/backup-azure-restore-key-secret)

Operações como segredo/chave revertida não exigem essa etapa e o mesmo keyvault pode ser usado após a restauração.

### <a name="can-i-access-the-vm-once-restored-due-to-a-vm-having-broken-relationship-with-domain-controller"></a>Posso acessar a VM uma vez restaurada devido a uma VM que está tendo uma relação quebrada com o controlador de domínio?

Sim, você acessa a VM uma vez restaurada devido a uma VM que tem uma relação quebrada com o controlador de domínio. Para obter mais informações, consulte este [artigo](https://docs.microsoft.com/azure/backup/backup-azure-arm-restore-vms#post-restore-steps)

## <a name="manage-vm-backups"></a>Gerenciar backups de VM

### <a name="what-happens-if-i-modify-a-backup-policy"></a>O que acontecerá se eu alterar minha política de backup?

O backup da VM é feito usando as configurações de retenção e agendamento na política nova ou modificada.

- Se a retenção for estendida, os pontos de recuperação existentes serão marcados e mantidos de acordo com a nova política.
- Se a retenção for reduzida, eles serão marcados para remoção no próximo trabalho de limpeza e subsequentemente excluídos.

### <a name="how-do-i-move-a-vm-backed-up-by-azure-backup-to-a-different-resource-group"></a>Como faço para mover uma VM com backup feito pelo Backup do Azure para um grupo de recursos diferente?

1. Pare temporariamente o backup e mantenha os dados de backup.
2. Mova a VM para o grupo de recursos de destino.
3. O backup foi habilitado novamente no mesmo cofre ou novo.

Você pode restaurar a VM a partir dos pontos de restauração disponíveis que foram criados antes da operação de migração.

### <a name="is-there-a-limit-on-number-of-vms-that-can-beassociated-with-a-same-backup-policy"></a>Há um limite para o número de VMs que podem ser associadas a uma mesma política de backup?

Sim, há um limite de 100 VMs que podem ser associadas à mesma política de backup do Portal. Recomendamos que, para mais de 100 VMs, crie várias políticas de backup com o mesmo agendamento ou agendamento diferente.

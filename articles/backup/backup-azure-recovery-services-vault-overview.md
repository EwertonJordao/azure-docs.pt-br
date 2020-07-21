---
title: Visão geral de cofres de Serviços de Recuperação
description: Uma visão geral e a comparação entre os cofres de Serviços de Recuperação e os cofres de Backup do Azure.
ms.topic: conceptual
ms.date: 08/10/2018
ms.openlocfilehash: 11a218badfab141c41430c3f48a5e930bfa1af8b
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86539033"
---
# <a name="recovery-services-vaults-overview"></a>Visão geral dos cofres dos Serviços de Recuperação

Este artigo descreve os recursos de um cofre de Serviços de Recuperação. Um cofre de Serviços de Recuperação é uma entidade de armazenamento no Azure que hospeda dados. Os dados normalmente são cópias de dados ou informações de configuração de VMs (máquinas virtuais), cargas de trabalho, servidores ou estações de trabalho. Você pode usar cofres dos Serviços de Recuperação para armazenar dados de backup para vários serviços do Azure, como VMs de IaaS (Windows ou Linux) e bancos de dados SQL do Azure. Os cofres de Serviços de Recuperação dão suporte a System Center DPM, Windows Server, Servidor de Backup do Azure e outros. Os cofres dos Serviços de Recuperação facilitam a organização dos dados de backup, minimizando a sobrecarga de gerenciamento.

Com uma assinatura do Azure, você pode criar até 500 cofres de Serviços de Recuperação por assinatura por região.

## <a name="comparing-recovery-services-vaults-and-backup-vaults"></a>Comparação de cofres de Serviços de Recuperação e cofres de Backup

Se você ainda tiver cofres de backup, eles serão atualizados automaticamente para os cofres dos serviços de recuperação. Até novembro de 2017, todos os cofres de Backup foram atualizados para cofres dos Serviços de Recuperação.

Os cofres dos serviços de recuperação são baseados no modelo de Azure Resource Manager do Azure, no entanto, os cofres de backup eram baseados no modelo de Service Manager do Azure. Quando você atualiza um cofre de Backup para um cofre de Serviços de Recuperação, os dados de backup permanecem intactos durante e após o processo de atualização. Cofres de Serviços de Recuperação fornecem recursos não disponíveis a cofres de Backup, como:

- **Recursos aprimorados para ajudar a proteger dados de backup**: com os cofres de Serviços de Recuperação, o Backup do Azure fornece recursos de segurança para proteger backups em nuvem. Esses recursos de segurança asseguram que você possa proteger seus backups e recuperar dados com segurança, mesmo que os servidores de produção e de backup estejam comprometidos. [Saiba mais](backup-azure-security-feature.md)

- **Monitoramento central para seu ambiente de TI híbrida**: com os cofres de Serviços de Recuperação, você pode monitorar não apenas suas [VMs da IaaS do Azure](backup-azure-manage-vms.md), como também seus [ativos locais](backup-azure-manage-windows-server.md#manage-backup-items) de um portal central. [Saiba mais](https://azure.microsoft.com/blog/alerting-and-monitoring-for-azure-backup)

- **RBAC (Controle de Acesso Baseado em Função)**: o RBAC oferece controle de gerenciamento de acesso detalhado no Azure. [O Azure fornece várias funções internas](../role-based-access-control/built-in-roles.md), e o Backup do Azure tem três [funções internas para gerenciar pontos de recuperação](backup-rbac-rs-vault.md). Cofres de Serviços de Recuperação são compatíveis com RBAC, que restringe o acesso de backup e restauração ao conjunto definido de funções de usuário. [Saiba mais](backup-rbac-rs-vault.md)

- **Proteger todas as configurações de Máquinas Virtuais do Azure**: cofres de Serviços de Recuperação protegem VMs com base no Resource Manager, incluindo Premium Disks, Managed Disks e VMs Criptografadas. Atualizar um cofre de Backup para um cofre de Serviços de Recuperação possibilita atualizar as VMs baseadas no Service Manager para VMs baseadas no Resource Manager. Ao atualizar o cofre, você pode manter os seus pontos de recuperação de VM baseada no Service Manager e configurar a proteção para VMs atualizadas (habilitadas para Resource Manager). [Saiba mais](https://azure.microsoft.com/blog/azure-backup-recovery-services-vault-ga)

- **Restauração instantânea para VMs da IaaS**: usando os cofres de Serviços de Recuperação, você pode restaurar arquivos e pastas em uma VM IaaS sem restaurar toda a VM, o que permite tempos de restauração mais rápidos. Restauração instantânea para VMs da IaaS está disponível para VMs Linux e Windows. [Saiba mais](backup-instant-restore-capability.md)

## <a name="storage-settings-in-the-recovery-services-vault"></a>Configurações de armazenamento no cofre dos serviços de recuperação

Um cofre dos Serviços de Recuperação é uma entidade que armazena os backups e os pontos de recuperação criados ao longo do tempo. O cofre dos Serviços de Recuperação também contém as políticas de backup associadas às máquinas virtuais protegidas.

O backup do Azure manipula automaticamente o armazenamento para o cofre. Veja como [as configurações de armazenamento podem ser alteradas](./backup-create-rs-vault.md#set-storage-redundancy).

Para saber mais sobre a redundância de armazenamento, consulte estes artigos sobre redundância [geográfica](../storage/common/storage-redundancy.md) e [local](../storage/common/storage-redundancy.md) .

## <a name="managing-your-recovery-services-vaults-in-the-portal"></a>Gerenciando os cofres de Serviços de Recuperação no portal

A criação e o gerenciamento de cofres dos Serviços de Recuperação no portal do Azure são fáceis porque o serviço de Backup integra-se a outros serviços do Azure. Essa integração significa que você pode criar ou gerenciar um cofre de Serviços de Recuperação *no contexto do serviço de destino*. Por exemplo, para exibir os pontos de recuperação de uma VM, selecione a VM e clique em **Backup** no menu Operações.

![Detalhes do cofre dos serviços de recuperação VM](./media/backup-azure-recovery-services-vault-overview/rs-vault-in-context-vm.png)

Se a VM não tiver o backup configurado, será solicitado que você configure o backup. Se o backup estiver configurado, você verá informações de backup sobre a VM, incluindo uma lista de pontos de restauração.  

![O cofre de Serviços de Recuperação detalha a VM](./media/backup-azure-recovery-services-vault-overview/vm-recovery-point-list.png)

No exemplo anterior, **ContosoVM** é o nome da máquina virtual. **ContosoVM-demovault** é o nome do cofre de Serviços de Recuperação. Você não precisa se lembrar do nome do cofre de Serviços de Recuperação que armazena os pontos de recuperação; essas informações estão disponíveis na máquina virtual.  

Se um cofre dos Serviços de Recuperação proteger vários servidores, talvez seja mais lógico examinar a área segura dos Serviços de Recuperação. Você pode pesquisar em todos os cofres de Serviços de Recuperação na assinatura e escolher um na lista.

As seções a seguir contêm links para artigos que explicam como usar um cofre de Serviços de Recuperação em cada tipo de atividade.

> [!NOTE]
> O cofre dos Serviços de Recuperação não poderá ser criado com o mesmo nome, caso tenha sido excluído no período de 24 horas. Use um nome de recurso diferente, escolha um grupo de recursos diferente ou tente novamente após 24 horas.

### <a name="back-up-data"></a>Fazer backup de dados

- [Fazer backup de uma VM do Azure](backup-azure-vms-first-look-arm.md)
- [Fazer backup do Windows Server ou da estação de trabalho do Windows](./backup-windows-with-mars-agent.md)
- [Fazer backup de cargas de trabalho do DPM para o Azure](backup-azure-dpm-introduction.md)
- [Preparar-se para fazer backup de cargas de trabalho usando o Servidor de Backup do Azure](backup-azure-microsoft-azure-backup.md)

### <a name="manage-recovery-points"></a>Gerenciar pontos de recuperação

- [Gerenciar backups de VM do Azure](backup-azure-manage-vms.md)
- [Gerenciando arquivos e pastas](backup-azure-manage-windows-server.md)

### <a name="restore-data-from-the-vault"></a>Restaurar dados do cofre

- [Recuperar arquivos individuais de uma VM do Azure](backup-azure-restore-files-from-vm.md)
- [Restaurar uma VM do Azure](backup-azure-arm-restore-vms.md)

### <a name="secure-the-vault"></a>Proteger o cofre

- [Protegendo dados de backup de nuvem em cofres dos Serviços de Recuperação](backup-azure-security-feature.md)

## <a name="azure-advisor"></a>Assistente do Azure

O [Azure Advisor](../advisor/index.yml) é um consultor de nuvem personalizado que ajuda a otimizar o uso do Azure. Ele analisa o uso do Azure e fornece recomendações oportunas para ajudar a otimizar e proteger suas implantações. Ele fornece recomendações em quatro categorias: alta disponibilidade, segurança, desempenho e custo.

O assistente do Azure fornece [recomendações](../advisor/advisor-high-availability-recommendations.md#protect-your-virtual-machine-data-from-accidental-deletion) por hora para VMs que não são submetidas a backup, portanto, você nunca perde o backup de VMs importantes. Você também pode controlar as recomendações adiando-as.  Você pode clicar na recomendação e habilitar o backup em VMs em linha especificando o cofre (em que os backups serão armazenados) e a política de backup (agendamento de backups e retenção de cópias de backup).

![Assistente do Azure](./media/backup-azure-recovery-services-vault-overview/azure-advisor.png)

## <a name="next-steps"></a>Próximas etapas

Use os artigos a seguir para:</br>
[Fazer backup de uma VM IaaS](backup-azure-arm-vms-prepare.md)</br>
[Fazer backup de um Servidor de Backup do Azure](backup-azure-microsoft-azure-backup.md)</br>
[Fazer backup de um Windows Server](backup-windows-with-mars-agent.md)

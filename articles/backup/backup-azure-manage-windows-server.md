---
title: Gerenciar servidores e cofres dos Serviços de Recuperação do Microsoft Azure
description: Neste artigo, saiba como usar o painel de visão geral do cofre do Recovery Services para monitorar e gerenciar seus cofres dos Serviços de Recuperação.
ms.topic: conceptual
ms.date: 07/08/2019
ms.openlocfilehash: 1a4d23c157700f42422cfe7ca8fa1c49e2cf128a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80131966"
---
# <a name="monitor-and-manage-recovery-services-vaults"></a>Monitorar e gerenciar cofres dos Serviços de Recuperação

Este artigo explica como usar o painel de **Visão geral** do cofre dos Serviços de Recuperação para monitorar e gerenciar os cofres dos Serviços de Recuperação. Quando você abre um cofre dos Serviços de Recuperação na lista, o painel de **Visão geral** do cofre selecionado será aberto. O painel fornece vários detalhes sobre o cofre. Há *blocos* que mostram: o status dos alertas críticos e de aviso, trabalhos de backup em andamento e com falha e a quantidade de LRS (armazenamento com redundância local) e GRS (armazenamento com redundância geográfica) utilizada. Se você fizer backup de VMs do Azure no cofre, o bloco [**Status da Pré-Verificação de Backup** exibe todos os itens críticos ou de aviso](https://docs.microsoft.com/azure/backup/backup-azure-manage-windows-server#backup-pre-check-status). A imagem a seguir é o painel de **Visão geral** do **cofre da Contoso**. O bloco **Itens de Backup** mostra que existem nove itens registrados no cofre.

![painel do cofre dos serviços de recuperação](./media/backup-azure-manage-windows-server/rs-vault-blade.png)

Os pré-requisitos para este artigo são: uma assinatura do Azure, um cofre dos Serviços de Recuperação e a existência de pelo menos um item de backup configurado para o cofre.

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]

## <a name="open-a-recovery-services-vault"></a>Abrir um cofre dos Serviços de Recuperação

Para monitorar alertas ou exibir dados de gerenciamento sobre um cofre dos Serviços de Recuperação, abra o cofre.

1. Faça login no [portal Azure](https://portal.azure.com/) usando sua assinatura do Azure.

2. No portal do Azure, clique em **Todos os serviços**.

   ![Abrir a lista de cofres dos Serviços de Recuperação, etapa 1](./media/backup-azure-manage-windows-server/open-rs-vault-list.png)

3. Na caixa de diálogo **Todos os Serviços**, digite **Serviços de Recuperação**. Quando você começa a digitar, a lista é filtrada com base em sua entrada. Quando a opção **Cofres dos Serviços de Recuperação** for exibida, clique nela para abrir a lista de cofres dos Serviços de Recuperação na assinatura.

    ![Criar Cofre de Serviços de Recuperação - etapa 1](./media/backup-azure-manage-windows-server/list-of-rs-vaults.png) <br/>

4. Na lista de cofres, clique em um cofre para abrir seu painel de **Visão geral**.

    ![painel do cofre dos serviços de recuperação](./media/backup-azure-manage-windows-server/rs-vault-blade.png) <br/>

    O painel de Visão geral usa blocos para fornecer alertas e dados do trabalho de backup.

## <a name="monitor-backup-jobs-and-alerts"></a>Monitorar trabalhos e alertas de backup

O painel de **Visão geral** do cofre dos Serviços de Recuperação fornece os blocos para informações de Monitoramento e Uso. Os blocos na seção de Monitoramento exibem alertas Críticos e de Aviso e os trabalhos Em andamento e Com falha. Clique em um determinado alerta ou trabalho para abrir o menu de Alertas de Backup ou Trabalhos de Backup, filtrado para esse trabalho ou o alerta.

![Tarefas do painel Backup](./media/backup-azure-manage-windows-server/monitor-dashboard-tiles-warning.png)

A seção de Monitoramento mostra os resultados das consultas de **Alertas de Backup** e **Trabalhos de Backup** predefinidas. Os blocos de Monitoramento fornecem informações atualizadas sobre:

* Alertas Críticos e de Aviso para trabalhos de Backup (nas últimas 24 horas)
* Consulte o status de pré-verificação das VMs do Azure. Para obter informações completas sobre o status de pré-verificação, consulte [Status de pré-verificação de backup](#backup-pre-check-status).
* Os trabalhos de Backup em andamento e os trabalhos com falha (nas últimas 24 horas).

Os blocos de Uso fornecem:

* O número de itens de Backup configurados para o cofre.
* O armazenamento do Azure (separado por LRS e GRS) consumido pelo cofre.

Clique nos blocos (exceto o Armazenamento de Backup) para abrir o menu associado. Na imagem acima, o bloco de Alertas de Backup mostra três alertas Críticos. Clicar na linha de alertas Críticos no bloco Alertas de Backup, abre os Alertas de Backup filtrados para alertas Críticos.

![Menu alertas de backup filtrado para alertas críticos](./media/backup-azure-manage-windows-server/critical-backup-alerts.png)

O menu Alertas de Backup, na imagem acima, está filtrado por: Status está ativo, Severidade é Crítica e Hora são as 24 horas anteriores.

### <a name="backup-pre-check-status"></a>Status de pré-verificação de backup

Pré-verificação de backup verifique a configuração de suas VMs em busca de problemas que possam afetar negativamente os backups. Eles agregam essas informações para que você possa visualizá-la diretamente do painel do Cofre de Serviços de Recuperação e fornecer recomendações para medidas corretivas para garantir backups consistentes com arquivos ou consistentes com aplicativos. Eles não exigem infra-estrutura e não têm custo adicional.  

Backup Pré-Verificações executados como parte das operações de backup programadas para suas VMs do Azure. Eles terminam com um dos seguintes estados:

* **Passado**: Este estado indica que a configuração da VM deve levar a backups bem-sucedidos e nenhuma ação corretiva precisa ser tomada.
* **Aviso**: Este estado indica um ou mais problemas na configuração da VM que *podem* levar a falhas de backup. Ele fornece etapas *recomendadas* para garantir backups bem-sucedidos. Por exemplo, não ter o mais recente VM Agent instalado pode fazer com que os backups falhem intermitentemente. Esta situação forneceria um estado de alerta.
* **Crítico**: Este estado indica um ou mais problemas críticos na configuração da VM que levarão a falhas de backup e *fornecerão* etapas *necessárias* para garantir backups bem-sucedidos. Por exemplo, um problema de rede causado por uma atualização das regras do NSG de uma VM, fará com que os backups falhem, pois impede que a VM se comunique com o serviço de backup do Azure. Esta situação proporcionaria um estado crítico.

Siga as etapas abaixo para começar a resolver quaisquer problemas relatados pelas pré-verificações de backup para backups de VM em seu Cofre de Serviços de Recuperação.

* Selecione o bloco **status de pré-verificação de backup (Azure VMs)** no painel do Cofre de Serviços de Recuperação.
* Selecione qualquer VM com um status de pré-verificação de backup de **crítica** ou **aviso**. Esta ação abrirá o painel de detalhes da **VM.**
* Selecione a notificação do painel na parte superior do painel para revelar a descrição do problema de configuração e as etapas corretivas.

## <a name="manage-backup-alerts"></a>Gerenciar alertas de Backup

Para acessar o menu Alertas de Backup, no menu Cofre dos Serviços de Recuperação, clique em **Alertas de Backup**.

![Alertas de Backup](./media/backup-azure-manage-windows-server/backup-alerts-menu.png)

O relatório de Alertas de Backup lista os alertas para o cofre.

![Alertas de Backup](./media/backup-azure-manage-windows-server/backup-alerts.png)

### <a name="alerts"></a>Alertas

A lista de Alertas de Backup exibe as informações selecionadas para os alertas filtrados. No menu Alertas de Backup, é possível filtrar alertas Críticos ou de Aviso.

| Nível de alerta | Eventos que geram alertas |
| ----------- | ----------- |
| Crítico | Alertas críticos são recebidos quando: trabalhos de Backup falham, trabalhos de recuperação falham e quando você interrompe a proteção em um servidor, mas retém os dados.|
| Aviso | Alertas de aviso são recebidos quando: trabalhos de Backup são concluídos com avisos, por exemplo quando menos de 100 arquivos ficam sem backup devido a problemas de corrupção, ou quando o backup de mais de 1.000.000 de arquivos é feito com êxito). |
| Informativo | no momento, nenhum alerta informativo está em uso. |

### <a name="viewing-alert-details"></a>Exibir detalhes do alerta

O relatório de Alertas de Backup rastreia oito detalhes sobre cada alerta. Use o botão **Escolher colunas** para editar os detalhes no relatório.

![Alertas de Backup](./media/backup-azure-manage-windows-server/backup-alerts.png)

Por padrão, todos os detalhes, exceto **Última hora da ocorrência**, são exibidos no relatório.

* Alerta
* Item de backup
* Servidor protegido
* Severity
* Duration
* Hora de criação
* Status
* Última hora da ocorrência

### <a name="change-the-details-in-alerts-report"></a>Alterar os detalhes no relatório de alertas

1. Para alterar as informações de relatório no menu **Alertas de Backup**, clique em **Escolher colunas**.

   ![Alertas de Backup](./media/backup-azure-manage-windows-server/alerts-menu-choose-columns.png)

   O menu **Escolher colunas** será aberto.

2. No menu **Escolher colunas**, escolha os detalhes que você quer exibir no relatório.

    ![Menu Escolher colunas](./media/backup-azure-manage-windows-server/choose-columns-menu.png)

3. Clique em **Concluído** para salvar as alterações e fechar o menu Escolher colunas.

   Se você fizer alterações, mas não quiser mantê-las, clique em **Redefinir** para retornar as seleções para a última configuração salva.

### <a name="change-the-filter-in-alerts-report"></a>Alterar o filtro no relatório de alertas

Use o menu **Filtro** para alterar a Severidade, Status, Hora de início e Hora de término dos alertas.

> [!NOTE]
> Editar o filtro de Alertas de Backup não altera os alertas Críticos ou de Aviso no painel de Visão geral do cofre.
>  

1. Para alterar o filtro de Alertas de Backup, no menu Alertas de Backup, clique em **Filtro**.

   ![Escolher o menu Filtro](./media/backup-azure-manage-windows-server/alerts-menu-choose-filter.png)

   O menu Filtro é exibido.

   ![Escolher o menu Filtro](./media/backup-azure-manage-windows-server/filter-alert-menu.png)

2. Edite a Severidade, Status, Hora de início ou Hora de término e, sem seguida, clique em **Concluído** para salvar as alterações.

## <a name="configuring-notifications-for-alerts"></a>Configurar notificações para alertas

Configure notificações para gerar emails quando ocorrer um alerta Crítico ou de Aviso. É possível enviar alertas por email a cada hora, ou quando ocorre um alerta específico.

   ![Filtrar alertas](./media/backup-azure-manage-windows-server/configure-notification.png)

Por padrão, as notificações por email são **Ativadas**. Clique em **Desativar** para interromper as notificações por email.

No controle **Notificar**, escolha **Por alerta** se não quiser agrupamento ou não tiver muitos itens que possam gerar alertas. Cada alerta resulta em uma notificação (a configuração padrão), e um email de resolução é enviado imediatamente.

Se você selecionar **Resumo Horário**, um email será enviado aos destinatários explicando os alertas não resolvidos gerados na última hora. Um email de resolução é enviado no final da hora.

Escolha a severidade do alerta (Crítico ou Aviso) usada para gerar o email. Atualmente, não há alertas de Informações.

## <a name="manage-backup-items"></a>Gerenciar itens de Backup

Um cofre dos Serviços de Recuperação contém muitos tipos de dados de backup. [Saiba mais](backup-overview.md#what-can-i-back-up) sobre o que você pode fazer backup. Para gerenciar os diversos servidores, computadores, bancos de dados e cargas de trabalho, clique no bloco **Itens de Backup** para exibir o conteúdo do cofre.

![Bloco Itens de backup](./media/backup-azure-manage-windows-server/backup-items.png)

A lista de Itens de Backup, organizados por tipo de gerenciamento de Backup, é aberta.

![Lista de itens de backup](./media/backup-azure-manage-windows-server/list-backup-items.png)

Para explorar um tipo específico de instância protegida, clique no item na coluna Tipo de Gerenciamento de Backup. Por exemplo, na imagem acima, há duas máquinas virtuais do Azure protegidas nesse cofre. Clicar em **Máquina Virtual do Azure** abre a lista de máquinas virtuais protegidas no cofre.

![Lista de tipos de backup](./media/backup-azure-manage-windows-server/list-of-protected-virtual-machines.png)

A lista de máquinas virtuais tem dados úteis: o Grupo de Recursos associados, [Pré-verificação de Backup](https://docs.microsoft.com/azure/backup/backup-azure-manage-windows-server#backup-pre-check-status) anterior, último Status de Backup e a data do Ponto de Restauração mais recente. As reticências, na última coluna, abrem o menu para disparar tarefas comuns. Os dados úteis fornecidos nas colunas são diferentes para cada tipo de backup.

![Lista de tipos de backup](./media/backup-azure-manage-windows-server/ellipsis-menu.png)

## <a name="manage-backup-jobs"></a>Gerenciar trabalhos de Backup

O bloco **Trabalhos de Backup** no painel do cofre mostra o número de trabalhos que estão Em andamento ou Com falha nas últimas 24 horas. O bloco fornece uma breve visão sobre o menu Trabalhos de Backup.

![Faça backup de itens das configurações](./media/backup-azure-manage-windows-server/backup-jobs-tile.png)

Para ver detalhes adicionais sobre os trabalhos, clique em **Em andamento** ou **Com falha** para abrir o menu Trabalhos de Backup filtrado para esse estado.

### <a name="backup-jobs-menu"></a>Menu Trabalhos de Backup

O menu **Trabalhos de Backup** exibe informações sobre o Tipo de item, Operação, Status, Hora de início e Duração.  

Para abrir o menu Trabalhos de Backup, no menu principal do cofre, clique em **Trabalhos de Backup**.

![Faça backup de itens das configurações](./media/backup-azure-manage-windows-server/backup-jobs-menu-item.png)

A lista de Trabalhos de Backup é aberta.

![Faça backup de itens das configurações](./media/backup-azure-manage-windows-server/backup-jobs-list.png)

O menu Trabalhos de Backup mostra o status para todas as operações, em todos os tipos de backup, nas últimas 24 horas. Use **Filtro** para alterar os filtros. Os filtros são explicados nas seções a seguir.

Para alterar os filtros:

1. No menu Trabalhos de Backup do cofre, clique em **Filtro**.

   ![Faça backup de itens das configurações](./media/backup-azure-manage-windows-server/vault-backup-job-menu-filter.png)

    O menu Filtro é aberto.

   ![Faça backup de itens das configurações](./media/backup-azure-manage-windows-server/filter-menu-backup-jobs.png)

2. Escolha as configurações de filtro e clique em **Concluído**. A lista filtrada é atualizada com base nas novas configurações.

#### <a name="item-type"></a>Tipo de item

O Tipo de Item é o tipo de gerenciamento de backup da instância protegida. Há quatro tipos; consulte a lista a seguir. É possível exibir todos os tipos de item ou um tipo de item. Não é possível selecionar dois ou três tipos de item. Os tipos de Item disponíveis são:

* Todos os tipos de item
* Máquina virtual do Azure
* Arquivos e pastas
* Armazenamento do Azure
* Carga de trabalho do Azure

#### <a name="operation"></a>Operação

É possível exibir uma única operação ou todas as operações. Não é possível selecionar duas ou três operações. As Operações disponíveis são:

* Todas as operações
* Registrar 
* Configurar o backup
* Backup
* Restaurar
* Desabilitar o backup
* Excluir dados de backup

#### <a name="status"></a>Status

É possível exibir o Status de todos os de um. Não é possível selecionar dois ou três status. Os status disponíveis são:

* Todos os status
* Concluído
* Em andamento
* Falhou
* Canceled
* Concluído com avisos

#### <a name="start-time"></a>Hora de início

O dia e a hora em que a consulta começa. O padrão é um período de 24 horas.

#### <a name="end-time"></a>Hora de término

O dia e a hora em que a consulta termina.

### <a name="export-jobs"></a>Exportar trabalhos

Use **Exportar trabalhos** para criar uma planilha que contém todas as informações do menu Trabalhos. A planilha eletrônica tem uma planilha que contém um resumo de todos os trabalhos e planilhas individuais para cada trabalho.

Para exportar as informações de trabalhos em uma planilha eletrônica, clique em **Exportar trabalhos**. O serviço cria uma planilha usando o nome do cofre e a data, mas você pode alterar o nome.

## <a name="monitor-backup-usage"></a>Monitorar o uso do Backup

O bloco Armazenamento de Backup no painel mostra o armazenamento consumido no Azure. O uso do armazenamento é fornecido para:

* Uso do armazenamento LRS de nuvem associado ao cofre
* Uso do armazenamento GRS de nuvem associado ao cofre

## <a name="troubleshooting-monitoring-issues"></a>Problemas de monitoramento de solução de problemas

**Problema:** trabalhos e/ou alertas do agente de Backup do Azure não aparecem no portal.

**Etapas de solução de problemas:** o processo, ```OBRecoveryServicesManagementAgent```, envia os dados de alerta e de trabalho para o serviço de Backup do Azure. Ocasionalmente, esse processo pode ficar travado ou ser interrompido.

1. Para verificar se o processo não está em execução, abra o **Gerenciador de Tarefas** e verifique se ```OBRecoveryServicesManagementAgent``` está em execução.

2. Se o processo não estiver em execução, abra o **Painel de Controle** e navegue pela lista de serviços. Inicie ou reinicie o **agente de gerenciamento dos Serviços de Recuperação do Microsoft Azure**.

    Para saber mais, procure os logs em:<br/>
   `<AzureBackup_agent_install_folder>\Microsoft Azure Recovery Services Agent\Temp\GatewayProvider*` Por exemplo:<br/>
   `C:\Program Files\Microsoft Azure Recovery Services Agent\Temp\GatewayProvider0.errlog`

## <a name="next-steps"></a>Próximas etapas

* [Restaurar o Windows Server ou o Windows Client do Azure](backup-azure-restore-windows-server.md)
* Para saber mais sobre o Backup do Azure, confira [Visão geral do backup do Azure](backup-introduction-to-azure-backup.md)

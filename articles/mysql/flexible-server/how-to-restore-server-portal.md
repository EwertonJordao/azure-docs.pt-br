---
title: Restore-portal do Azure-banco de dados do Azure para MySQL-servidor flexível
description: Este artigo descreve como executar operações de restauração no banco de dados do Azure para MySQL por meio do portal do Azure.
author: kummanish
ms.author: manishku
ms.service: mysql
ms.topic: how-to
ms.date: 09/21/2020
ms.openlocfilehash: 1c81ddad8a11cbad361ff84caf6f7200a0c010d5
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/09/2020
ms.locfileid: "90932877"
---
# <a name="point-in-time-restore-of-a-azure-database-for-mysql---flexible-server-preview"></a>Restauração pontual de um banco de dados do Azure para MySQL – servidor flexível (versão prévia)


> [!IMPORTANT]
> O Banco de Dados do Azure para MySQL – Servidor Flexível está atualmente na versão prévia pública.

Este artigo fornece um procedimento passo a passo para executar recuperações pontuais no servidor flexível usando backups.

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este guia de instruções, você precisa:

-   Você deve ter um servidor flexível do banco de dados do Azure para MySQL.

## <a name="restore-to-the-latest-restore-point"></a>Restaurar para o ponto de restauração mais recente

Siga estas etapas para restaurar seu servidor flexível usando um backup existente mais antigo.

1.  Na [portal do Azure](https://portal.azure.com/), escolha o servidor flexível do qual você deseja restaurar o backup.

2.  Clique em **visão geral** no painel esquerdo.

3.  Na página Visão geral, clique em **restaurar**.

    Reservado

4.  Restore Page será mostrado com uma opção para escolher entre o ponto de **restauração mais recente** e o ponto de restauração personalizado.

5.  Selecione o **ponto de restauração mais recente**.


6.  Forneça um novo nome de servidor no campo **restaurar para o novo servidor** .

    :::image type="content" source="./media/concept-backup-restore/restore-blade-latest.png" alt-text="Hora de restauração mais antiga":::

8.  Clique em **OK**.

9.  Uma notificação será mostrada de que a operação de restauração foi iniciada.

## <a name="restoring-to-a-custom-restore-point"></a>Restaurando para um ponto de restauração personalizado

Siga estas etapas para restaurar seu servidor flexível usando um backup existente mais antigo.

1.  Na [portal do Azure](https://portal.azure.com/), escolha o servidor flexível do qual você deseja restaurar o backup.

2.  Na página Visão geral, clique em **restaurar**.

    Reservado

3.  Restore Page será mostrado com uma opção para escolher entre o ponto de restauração mais antigo e o ponto de restauração personalizado.

4.  Escolha **ponto de restauração personalizado**.

5.  Selecione data e hora.

6.  Forneça um novo nome de servidor no campo **restaurar para o novo servidor** .

6.  Forneça um novo nome de servidor no campo **restaurar para o novo servidor** . 
   
    :::image type="content" source="./media/concept-backup-restore/restore-blade-custom.png" alt-text="Hora de restauração mais antiga":::
 
7.  Clique em **OK**.

8.  Uma notificação será mostrada de que a operação de restauração foi iniciada.

## <a name="next-steps"></a>Próximas etapas

Espaço reservado

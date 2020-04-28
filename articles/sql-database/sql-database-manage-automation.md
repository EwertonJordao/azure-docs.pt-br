---
title: Gerenciar bancos de dados com a automação do Azure
description: Saiba mais sobre como o serviço de Automação do Azure pode ser usado para gerenciar bancos de dados SQL do Azure em grande escala.
services: sql-database
ms.service: sql-database
ms.subservice: operations
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: juliemsft
ms.author: jrasnick
ms.reviewer: carlrab
ms.date: 03/12/2019
ms.openlocfilehash: 9d826a75f05cf2031565f89e21d7f3667ecc8f17
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/27/2020
ms.locfileid: "73822818"
---
# <a name="managing-azure-sql-databases-using-azure-automation"></a>Gerenciando bancos de dados SQL do Azure usando a Automação do Azure

Este guia apresentará o serviço de Automação do Azure e como ele pode ser usado para simplificar o gerenciamento de seus bancos de dados SQL do Azure.

## <a name="what-is-azure-automation"></a>O que é Automação do Azure?

[Automação do Azure](https://azure.microsoft.com/services/automation/) é um serviço do Azure para simplificar o gerenciamento de nuvem por meio da automação de processo. Com a Automação do Azure, tarefas de execução longa, manuais, propensas a erros e repetidas com frequência podem ser automatizadas para melhorar a confiabilidade, a eficiência e o tempo de implantação para sua organização.

A Automação do Azure fornece um mecanismo de execução de fluxo de trabalho altamente confiável e altamente disponível, e que pode ser dimensionado para atender às suas necessidades à medida que sua organização cresce. Na Automação do Azure, os processos podem ser inicializados manualmente por sistemas de terceiros ou a intervalos agendados para que as tarefas aconteçam exatamente quando necessário.

Reduza o custo operacional e libere a equipe de TI/ DevOps para se concentrar no trabalho que adiciona valor comercial ao transferir as tarefas de gerenciamento de nuvem para serem executadas automaticamente pela Automação do Azure.

## <a name="how-can-azure-automation-help-manage-azure-sql-databases"></a>Como a Automação do Azure pode ajudar a gerenciar bancos de dados SQL do Azure?

O Banco de Dados SQL do Azure pode ser gerenciado na Automação do Azure usando os [cmdlets do PowerShell do Banco de Dados SQL do Azure](https://docs.microsoft.com/powershell/module/servicemanagement/azure/#sql) que estão disponíveis em [Ferramentas do Azure PowerShell](/powershell/azure/overview). A Automação do Azure tem esses cmdlets do PowerShell de banco de dados SQL do Azure disponíveis imediatamente para que você possa executar todas as suas tarefas de gerenciamento de banco de dados SQL dentro do serviço. Você também pode combinar esses cmdlets na Automação do Azure com os cmdlets de outros serviços do Azure para automatizar tarefas complexas em serviços do Azure e sistemas de terceiros.

A Automação do Azure também tem a capacidade de se comunicar com servidores SQL diretamente, emitindo comandos SQL usando o PowerShell.

A [galeria de runbooks de Automação do Azure](https://azure.microsoft.com/blog/20../../introducing-the-azure-automation-runbook-gallery/) contém uma variedade de runbooks da comunidade e da equipe de produto para você começar a automação do gerenciamento dos bancos de dados SQL do Azure, de outros serviços do Azure e de sistemas de terceiros. A galeria de runbooks inclui:

- [Executar consultas SQL em um banco de dados do SQL Server](https://gallery.technet.microsoft.com/scriptcenter/How-to-use-a-SQL-Command-be77f9d2)
- [Escalar verticalmente (para cima ou para baixo) um Banco de Dados SQL do Azure em uma agenda](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-Database-e957354f)
- [Truncar uma tabela SQL se seu banco de dados se aproximar do tamanho máximo](https://gallery.technet.microsoft.com/scriptcenter/Azure-Automation-Your-SQL-30f8736b)
- [Indexar tabelas em um Banco de Dados SQL do Azure se elas estiverem muito fragmentadas](https://gallery.technet.microsoft.com/scriptcenter/Indexes-tables-in-an-Azure-73a2a8ea)

## <a name="next-steps"></a>Próximas etapas

Agora que você aprendeu os fundamentos de Automação do Azure e como ela pode ser usada para gerenciar bancos de dados SQL do Azure, siga estes links para obter mais informações sobre a Automação do Azure.

- [Visão geral da automação do Azure](../automation/automation-intro.md)
- [Meu primeiro runbook](../automation/automation-first-runbook-graphical.md)
- [Automação do Azure: seu Agente SQL na nuvem](https://azure.microsoft.com/blog/20../../azure-automation-your-sql-agent-in-the-cloud/) 
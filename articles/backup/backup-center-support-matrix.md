---
title: Matriz de suporte para o centro de backup
description: Este artigo resume os cenários aos quais o centro de backup dá suporte para cada tipo de carga de trabalho
ms.topic: conceptual
ms.date: 09/07/2020
ms.openlocfilehash: 8effc2514abf1cac55abc28b625b869810536baf
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/09/2020
ms.locfileid: "90994048"
---
# <a name="support-matrix-for-backup-center"></a>Matriz de suporte para o centro de backup

O centro de backup fornece um único painel de vidro para que as empresas [administrem, monitorem, operem e analisem backups em escala](backup-center-overview.md). Este artigo resume os cenários aos quais o centro de backup dá suporte para cada tipo de carga de trabalho.

## <a name="supported-scenarios"></a>Cenários com suporte

| **Categoria** | **Cenário**  | **Cargas de trabalho com suporte**  | **Limites** |
| -------------| ------------- | ----------------------- |------------|
| Monitoramento   | Exibir todos os trabalhos | <li> Máquina Virtual do Azure <br><br> <li> Banco de dados do Azure para servidor PostgreSQL | <li> 7 dias de trabalhos disponíveis prontos para uso. <br> <li> Cada filtro/lista suspensa dá suporte a um máximo de 1000 itens. Portanto, o centro de backup pode ser usado para monitorar um máximo de 1000 assinaturas e 1000 cofres entre locatários. |
| Monitoramento | Exibir todas as instâncias de backup | <li> Máquina Virtual do Azure <br><br> <li> Banco de dados do Azure para servidor PostgreSQL | O mesmo que o descrito acima |
| Monitoramento | Exibir todas as políticas de backup | <li> Máquina Virtual do Azure <br><br> <li> Banco de dados do Azure para servidor PostgreSQL | O mesmo que o descrito acima |
| Monitoramento | Exibir todos os cofres | <li> Máquina Virtual do Azure <br><br> <li> Banco de dados do Azure para servidor PostgreSQL | O mesmo que o descrito acima |
| Ações | Configurar backup | <li> Máquina Virtual do Azure <br><br> <li> Banco de dados do Azure para servidor PostgreSQL | Consulte matrizes de suporte para backup de [VM do Azure](https://docs.microsoft.com/azure/backup/backup-support-matrix-iaas) e [banco de dados do Azure para backup de servidor PostgreSQL](backup-azure-database-postgresql.md#support-matrix) |
| Ações | Restaurar instância de backup | <li> Máquina Virtual do Azure <br><br> <li> Banco de dados do Azure para servidor PostgreSQL | Consulte matrizes de suporte para backup de [VM do Azure](https://docs.microsoft.com/azure/backup/backup-support-matrix-iaas) e [banco de dados do Azure para backup de servidor PostgreSQL](backup-azure-database-postgresql.md#support-matrix) |
| Ações | Criar cofre | <li> Máquina Virtual do Azure <br><br> <li> Banco de dados do Azure para servidor PostgreSQL | Consulte matrizes de suporte para o [cofre dos serviços de recuperação](https://docs.microsoft.com/azure/backup/backup-support-matrix#vault-support) |
| Ações | Criar política de backup | <li> Máquina Virtual do Azure <br><br> <li> Banco de dados do Azure para servidor PostgreSQL | Consulte matrizes de suporte para o [cofre dos serviços de recuperação](https://docs.microsoft.com/azure/backup/backup-support-matrix#vault-support) |
| Ações | Executar backup sob demanda para uma instância de backup | <li> Máquina Virtual do Azure <br><br> <li> Banco de dados do Azure para servidor PostgreSQL | Consulte matrizes de suporte para backup de [VM do Azure](https://docs.microsoft.com/azure/backup/backup-support-matrix-iaas) e [banco de dados do Azure para backup de servidor PostgreSQL](backup-azure-database-postgresql.md#support-matrix) |
| Ações | Parar o backup de uma instância de backup | <li> Máquina Virtual do Azure <br><br> <li> Banco de dados do Azure para servidor PostgreSQL | Consulte matrizes de suporte para backup de [VM do Azure](https://docs.microsoft.com/azure/backup/backup-support-matrix-iaas) e [banco de dados do Azure para backup de servidor PostgreSQL](backup-azure-database-postgresql.md#support-matrix) |
| Insights | Exibir relatórios de backup | <li> Máquina Virtual do Azure <br><br> <li> SQL na máquina virtual do Azure <br><br> <li> SAP HANA na máquina virtual do Azure <br><br> <li> Arquivos do Azure <br><br> <li> System Center Data Protection Manager <br><br> <li> Agente de backup do Azure (MARS) <br><br> <li> Servidor de Backup do Azure (MABS) | Consulte [cenários com suporte para relatórios de backup](https://docs.microsoft.com/azure/backup/configure-reports#supported-scenarios) |
| Governança | Exibir e atribuir políticas internas e personalizadas do Azure na categoria ' backup ' | N/D | N/D |
| Governança | Exibir fontes de fonte não configuradas para backup | <li> Máquina Virtual do Azure <br><br> <li> Banco de dados do Azure para servidor PostgreSQL | N/D |

## <a name="unsupported-scenarios"></a>Cenários sem suporte

| **Categoria** | **Cenário**  |
|--------------|---------------|
| Monitoramento | Exibir alertas em escala |
| Ações | Definir configurações de cofre em escala |
| Ações | Executar trabalho de restauração entre regiões do centro de backup |

## <a name="next-steps"></a>Próximas etapas

* [Examinar a matriz de suporte para o backup do Azure](https://docs.microsoft.com/azure/backup/backup-support-matrix)
* [Examinar a matriz de suporte para o backup de VM do Azure](https://docs.microsoft.com/azure/backup/backup-support-matrix-iaas)
* [Examine a matriz de suporte para o banco de dados do Azure para backup do servidor PostgreSQL](backup-azure-database-postgresql.md#support-matrix)

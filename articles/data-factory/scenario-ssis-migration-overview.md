---
title: Migrar cargas de trabalho SSIS locais para o SSIS no Azure Data Factory
description: Migre cargas de trabalho do SSIS locais para o SSIS no ADF.
services: data-factory
documentationcenter: ''
author: chugugrace
ms.author: chugu
ms.reviewer: ''
manager: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 9/3/2019
ms.openlocfilehash: 53085544be9477c03fdbbc27e709bd80dea25b92
ms.sourcegitcommit: faeabfc2fffc33be7de6e1e93271ae214099517f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/13/2020
ms.locfileid: "88186056"
---
# <a name="migrate-on-premises-ssis-workloads-to-ssis-in-adf"></a>Migrar cargas de trabalho do SSIS local para o SSIS no ADF

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

## <a name="overview"></a>Visão geral

Ao migrar suas cargas de trabalho de banco de dados do SQL Server local para os serviços de banco de dados do Azure, ou seja, o banco de dados SQL do Azure ou o SQL Instância Gerenciada do Azure, suas cargas de trabalho de ETL no SQL Server Integration Services (SSIS) como um dos serviços de valor agregado primário também precisarão ser migradas.

Azure-SSIS Integration Runtime (IR) no Azure Data Factory (ADF) dá suporte a pacotes SSIS em execução. Depois que Azure-SSIS IR for provisionado, você poderá usar ferramentas familiares, como SQL Server Data Tools (SSDT)/SQL Server Management Studio (SSMS) e utilitários de linha de comando, como dtinstall/dtutil/dtexec, para implantar e executar seus pacotes no Azure. Para obter mais informações, consulte [visão geral de elevação e deslocamento do Azure SSIS](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-lift-shift-ssis-packages-overview).

Este artigo destaca o processo de migração de suas cargas de trabalho de ETL do SSIS local para o SSIS no ADF. O processo de migração consiste em duas fases: **Avaliação** e **Migração**.

## <a name="assessment"></a>Avaliação

Para estabelecer um plano de migração completo, uma avaliação completa ajudará a identificar problemas com os pacotes SSIS de origem que impedirão uma migração bem-sucedida.

O AMD (Assistente de Migração de Dados) é uma ferramenta gratuita que pode ser baixada para essa finalidade e ser instalada e executada localmente. O projeto de avaliação DMA do tipo **Integration Services** pode ser criado para avaliar pacotes do SSIS em lotes e identificar problemas de compatibilidade que são apresentados nas seguintes categorias:

- Bloqueadores de migração: são problemas de compatibilidade que impedem a execução dos pacotes de origem de migração no Azure-SSIS IR. O DMA fornece orientação para ajudá-lo a resolver esses problemas.

- Problemas informativos: esses são recursos parcialmente suportados ou preteridos que são usados em pacotes de origem. O DMA fornece um conjunto abrangente de recomendações, abordagens alternativas disponíveis no Azure e etapas de mitigação para resolver.

### <a name="four-storage-types-for-ssis-packages"></a>Quatro tipos de armazenamento para pacotes SSIS

- Catálogo do SSIS (SSISDB). Isso foi introduzido com SQL Server 2012 e contém um conjunto de procedimentos armazenados, exibições e funções com valor de tabela usados para trabalhar com projetos/pacotes do SSIS.
- Sistema de arquivos.
- Banco de dados do sistema SQL Server (MSDB).
- Repositório de pacotes SSIS. Esta é uma camada de gerenciamento de pacotes sobre dois subtipos:
  - MSDB, que é um banco de dados do sistema em SQL Server usado para armazenar pacotes SSIS.
  - Sistema de arquivos gerenciado, que é uma pasta específica em SQL Server caminho de instalação usado para armazenar pacotes SSIS.

Atualmente, o DMA dá suporte à avaliação em lote de pacotes armazenados no **sistema de arquivos**, no **repositório de pacotes**e no catálogo do **SSIS** desde o **DMA versão v 5.0**.

Obtenha o [DMA](https://docs.microsoft.com/sql/dma/dma-overview)e [realize sua avaliação de pacote com ele](https://docs.microsoft.com/sql/dma/dma-assess-ssis).

## <a name="migration"></a>Migração

Dependendo dos tipos de [armazenamento](#four-storage-types-for-ssis-packages) dos pacotes SSIS de origem e do destino de migração das cargas de trabalho de banco de dados, as etapas para migrar  **pacotes SSIS** e **SQL Server Agent trabalhos** que agendam as execuções de pacote SSIS podem variar. Há dois cenários:

- [**Azure SQL instância gerenciada** como destino de carga de trabalho de banco de dados](#azure-sql-managed-instance-as-database-workload-destination)
- [**Banco de dados SQL do Azure** como destino de carga de trabalho](#azure-sql-database-as-database-workload-destination)

### <a name="azure-sql-managed-instance-as-database-workload-destination"></a>**Azure SQL instância gerenciada** como destino de carga de trabalho de banco de dados

| **Tipo de armazenamento de pacote** |Como migrar pacotes SSIS em lote|Como migrar trabalhos do SSIS em lote|
|-|-|-|
|SSISDB|[Migrar **SSISDB**](scenario-ssis-migration-ssisdb-mi.md)|<li>[Migrar trabalhos do SSIS para o agente do SQL Instância Gerenciada do Azure](scenario-ssis-migration-ssisdb-mi.md#ssis-jobs-to-sql-managed-instance-agent) <li>Converta-os em pipelines/atividades/gatilhos do ADF por meio de scripts/SSMS/ADF Portal. Para obter mais informações, consulte [recurso de agendamento do SSMS](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-schedule-packages-ssms).|
|Sistema de Arquivos|Reimplante-os em compartilhamentos de arquivos/arquivos do Azure por meio de dtinstall/dtutil/cópia manual ou para manter os sistemas de arquivos a serem acessados por meio de IR para VNet/auto-hospedado. Para obter mais informações, consulte [Utilitário dtutil](https://docs.microsoft.com/sql/integration-services/dtutil-utility).|<li>[Migrar trabalhos do SSIS para o agente do SQL Instância Gerenciada do Azure](scenario-ssis-migration-ssisdb-mi.md#ssis-jobs-to-sql-managed-instance-agent) <li> Migrar com o [Assistente de migração de trabalho SSIS no SSMS](how-to-migrate-ssis-job-ssms.md) <li>Converta-os em pipelines/atividades/gatilhos do ADF por meio de scripts/SSMS/ADF Portal. Para obter mais informações, consulte [recurso de agendamento do SSMS](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-schedule-packages-ssms).|
|SQL Server (MSDB)|Exportá-los para sistemas de arquivos/compartilhamentos de arquivos/arquivos do Azure por meio do SSMS/dtutil. Para obter mais informações, consulte [exportando pacotes SSIS](https://docs.microsoft.com/sql/integration-services/service/package-management-ssis-service#import-and-export-packages).|Converta-os em pipelines/atividades/gatilhos do ADF por meio de scripts/SSMS/ADF Portal. Para obter mais informações, consulte [recurso de agendamento do SSMS](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-schedule-packages-ssms).|
|Repositório de pacotes|Exportá-los para sistemas de arquivos/compartilhamentos de arquivos/arquivos do Azure por meio do SSMS/dtutil ou reimplantá-los em compartilhamentos de arquivos/arquivos do Azure por meio de dtinstall/dtutil/cópia manual ou mantê-los em sistemas de arquivos para acesso via VNet/infravermelho auto-hospedado. Para obter mais informações, consulte Utilitário dtutil. Para obter mais informações, consulte [Utilitário dtutil](https://docs.microsoft.com/sql/integration-services/dtutil-utility).|<li>[Migrar trabalhos do SSIS para o agente do SQL Instância Gerenciada do Azure](scenario-ssis-migration-ssisdb-mi.md#ssis-jobs-to-sql-managed-instance-agent) <li> Converta-os em pipelines/atividades/gatilhos do ADF por meio de scripts/SSMS/ADF Portal. Para obter mais informações, consulte [recurso de agendamento do SSMS](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-schedule-packages-ssms).|

### <a name="azure-sql-database-as-database-workload-destination"></a>**Banco de dados SQL do Azure** como destino de carga de trabalho

| **Tipo de armazenamento de pacote** |Como migrar pacotes SSIS em lote|Como migrar trabalhos em lote|
|-|-|-|
|SSISDB|Reimplante no Azure-SSISDB por meio de SSDT/SSMS. Para obter mais informações, consulte [implantando pacotes do SSIS no Azure](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-deploy-run-monitor-tutorial).|Converta-os em pipelines/atividades/gatilhos do ADF por meio de scripts/SSMS/ADF Portal. Para obter mais informações, consulte [recurso de agendamento do SSMS](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-schedule-packages-ssms).|
|Sistema de Arquivos|Reimplante-os em compartilhamentos de arquivos/arquivos do Azure por meio de dtinstall/dtutil/cópia manual ou para manter os sistemas de arquivos a serem acessados por meio de IR para VNet/auto-hospedado. Para obter mais informações, consulte [Utilitário dtutil](https://docs.microsoft.com/sql/integration-services/dtutil-utility).|<li> Migrar com o [Assistente de migração de trabalho SSIS no SSMS](how-to-migrate-ssis-job-ssms.md) <li> Converta-os em pipelines/atividades/gatilhos do ADF por meio de scripts/SSMS/ADF Portal. Para obter mais informações, consulte [recurso de agendamento do SSMS](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-schedule-packages-ssms).|
|SQL Server (MSDB)|Exportá-los para sistemas de arquivos/compartilhamentos de arquivos/arquivos do Azure por meio do SSMS/dtutil. Para obter mais informações, consulte [exportando pacotes SSIS](https://docs.microsoft.com/sql/integration-services/service/package-management-ssis-service#import-and-export-packages).|Converta-os em pipelines/atividades/gatilhos do ADF por meio de scripts/SSMS/ADF Portal. Para obter mais informações, consulte [recurso de agendamento do SSMS](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-schedule-packages-ssms).|
|Repositório de pacotes|Exportá-los para sistemas de arquivos/compartilhamentos de arquivos/arquivos do Azure por meio do SSMS/dtutil ou reimplantá-los em compartilhamentos de arquivos/arquivos do Azure por meio de dtinstall/dtutil/cópia manual ou mantê-los em sistemas de arquivos para acesso via VNet/infravermelho auto-hospedado. Para obter mais informações, consulte Utilitário dtutil. Para obter mais informações, consulte [Utilitário dtutil](https://docs.microsoft.com/sql/integration-services/dtutil-utility).|Converta-os em pipelines/atividades/gatilhos do ADF por meio de scripts/SSMS/ADF Portal. Para obter mais informações, consulte [recurso de agendamento do SSMS](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-schedule-packages-ssms).|

## <a name="additional-resources"></a>Recursos adicionais

- [Azure Data Factory](https://docs.microsoft.com/azure/data-factory/introduction)
- [Assistente de Migração de Dados](https://docs.microsoft.com/sql/dma/dma-overview)
- [Aumentar e deslocar cargas de trabalho do SSIS para a nuvem](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-lift-shift-ssis-packages-overview?view=sql-server-2017)
- [Migrar pacotes SSIS para Instância Gerenciada de SQL do Azure](https://docs.microsoft.com/azure/dms/how-to-migrate-ssis-packages-managed-instance)
- [Reimplantar pacotes no banco de dados SQL do Azure](https://docs.microsoft.com/azure/dms/how-to-migrate-ssis-packages)

## <a name="next-steps"></a>Próximas etapas

- [Validar pacotes do SSIS implantados no Azure](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-validate-packages)
- [Executar pacotes do SSIS implantados no Azure](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-run-packages)
- [Azure-SSIS Integration Runtime de monitor](https://docs.microsoft.com/azure/data-factory/monitor-integration-runtime#azure-ssis-integration-runtime)
- [Agendar execuções de pacote SSIS no Azure](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-schedule-packages)

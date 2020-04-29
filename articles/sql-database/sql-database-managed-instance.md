---
title: Visão geral da instância gerenciada do SQL
description: Este artigo descreve a instância gerenciada do banco de dados SQL do Azure.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: bonova
ms.author: bonova
ms.reviewer: sstein, carlrab, vanto
ms.date: 04/02/2020
ms.openlocfilehash: 04b07ff60c882501c49ad58607db867e7e99897c
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80879064"
---
# <a name="what-is-azure-sql-database-managed-instance"></a>O que é a instância gerenciada do banco de dados SQL do Azure?

A instância gerenciada é uma nova opção de implantação do Banco de Dados SQL do Azure, fornecendo quase 100% de compatibilidade com o mais recente Mecanismo de Banco de Dados do SQL Server (Enterprise Edition) local, fornecendo uma implementação de [VNet (rede virtual)](../virtual-network/virtual-networks-overview.md) nativa que aborda questões de segurança comuns e um [modelo corporativo](https://azure.microsoft.com/pricing/details/sql-database/) favorável para clientes do SQL Server local. O modelo de implantação de instância gerenciada permite que os clientes do SQL Server existentes façam lift-and-shift dos aplicativos locais para a nuvem com alterações mínimas do banco de dados e aplicativo. Ao mesmo tempo, a opção de implantação da instância gerenciada preserva todas as funcionalidades de PaaS (aplicação automática de patches e atualizações de versão, [backups automatizados](sql-database-automated-backups.md), [alta disponibilidade](sql-database-high-availability.md) ), reduzindo drasticamente a sobrecarga de gerenciamento e TCO.

> [!IMPORTANT]
> Para obter uma lista de regiões em que a opção de implantação de instância gerenciada está disponível no momento, veja [regiões com suporte](sql-database-managed-instance-resource-limits.md#supported-regions).

O diagrama a seguir apresenta os principais recursos da instância gerenciada:

![principais recursos](./media/sql-database-managed-instance/key-features.png)

O modelo de implantação da instância gerenciada foi desenvolvida para clientes que desejam migrar um grande número de aplicativos do ambiente local ou IaaS, autocompilados, ou fornecido pelo ISV para o ambiente de nuvem de PaaS totalmente gerenciado, com o menor esforço de migração possível. Usando o [DMS (serviço de migração de dados)](../dms/tutorial-sql-server-to-managed-instance.md#create-an-azure-database-migration-service-instance) totalmente automatizado no Azure, os clientes podem migrar e deslocar suas SQL Server locais para uma instância gerenciada que oferece compatibilidade com SQL Server local e isolamento completo de instâncias de clientes com suporte a VNet nativa.  Com o Software Assurance, você pode trocar suas licenças existentes por taxas com desconto em uma instância gerenciada usando o [benefício híbrido do Azure para SQL Server](https://azure.microsoft.com/pricing/hybrid-benefit/).  A instância gerenciada é o melhor destino da migração na nuvem para instâncias do SQL Server que exigem alta segurança e uma superfície de programação avançada.

A opção de implantação da instância gerenciada tem como objetivo entregar aproximadamente 100% de compatibilidade de área de superfície com a última versão do SQL Server local por meio de um plano de lançamento em etapas.

Para decidir entre as opções de implantação do Banco de Dados SQL do Azure: banco de dados individual, banco de dados em pool e instância gerenciada e SQL Server hospedado em máquina virtual. Veja [Como escolher a versão correta do SQL Server na nuvem do Azure](sql-database-paas-vs-sql-server-iaas.md).

## <a name="key-features-and-capabilities"></a>Principais recursos e capacidades

A instância gerenciada combina os melhores recursos disponíveis no Banco de Dados SQL do Azure e no Mecanismo de Banco de Dados do SQL Server.

> [!IMPORTANT]
> Uma instância gerenciada executa com todos os recursos da versão mais recente do SQL Server, incluindo operações online, correções de plano automático e outros aprimoramentos de desempenho do enterprise. Uma comparação dos recursos disponíveis é explicada em [Comparação de recursos: Banco de Dados SQL do Azure versus SQL Server](sql-database-features.md).

| **Benefícios de PaaS** | **Continuidade de negócios** |
| --- | --- |
|Sem gerenciamento e compra de hardware <br>Sem sobrecarga de gerenciamento para gerenciar infraestrutura subjacente <br>Rápido provisionamento e dimensionamento de serviço <br>Aplicação de patch automatizado e atualização da versão <br>Integração com outros serviços de dados PaaS |99,99% do SLA de tempo de atividade  <br>Compilado em [alta disponibilidade](sql-database-high-availability.md) <br>Dados protegidos com [backups automatizados](sql-database-automated-backups.md) <br>Período de retenção de backup configurável pelo cliente <br>[Backups](https://docs.microsoft.com/sql/t-sql/statements/backup-transact-sql?view=azuresqldb-mi-current) iniciados pelo usuário <br>Capacidade de [restauração pontual do banco de dados](sql-database-recovery-using-backups.md#point-in-time-restore) |
|**Segurança e conformidade** | **Gerenciamento**|
|Ambiente isolado ([integração VNet](sql-database-managed-instance-connectivity-architecture.md), serviço de locatário único, computação dedicada e armazenamento) <br>[TDE (Transparent Data Encryption)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql)<br>[Autenticação do Azure AD](sql-database-aad-authentication.md), suporte de logon único <br> <a href="/sql/t-sql/statements/create-login-transact-sql?view=azuresqldb-mi-current">Entidades de segurança do servidor do Azure AD (logons)</a>  <br>Cumpre os padrões de conformidade assim como o Banco de Dados SQL do Azure <br>[Auditoria do SQL](sql-database-managed-instance-auditing.md) <br>[Proteção avançada contra ameaças](sql-database-managed-instance-threat-detection.md) |API do Azure Resource Manager para automatizar o dimensionamento e provisionamento do serviço <br>Funcionalidade do Portal do Azure para dimensionamento e provisionamento manual do serviço <br>Serviço de Migração de Dados

> [!IMPORTANT]
> O banco de dados SQL do Azure (todas as opções de implantação) foi certificado em relação a vários padrões de conformidade. Para obter mais informações, consulte a [central de confiabilidade do Microsoft Azure](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942) , em que você pode encontrar a lista mais atual de certificações de conformidade do banco de dados SQL.

Os principais recursos de instâncias gerenciadas são mostrados na tabela a seguir:

|Recurso | Descrição|
|---|---|
| Compilação/versão do SQL Server | Mecanismo de BD do SQL Server (estável mais recente) |
| Backups automatizados gerenciados | Sim |
| Métricas e monitoramento de banco de dados e instância interna | Sim |
| Aplicação automática de patches de software | Sim |
| Os recursos mais recentes do mecanismo de banco de dados | Sim |
| Número de arquivos de dados (LINHAS) por banco de dados | Vários |
| Número de arquivos de log (LOG) por banco de dados | 1 |
| VNet - Implantação do Azure Resource Manager | Sim |
| VNet - Modelo de implantação clássico | Não |
| Suporte do Portal | Sim|
| Serviço de Integração Integrado (SSIS) | Não - o SSIS faz parte da [PaaS do Azure Data Factory](https://docs.microsoft.com/azure/data-factory/tutorial-deploy-ssis-packages-azure) |
| Serviço de análise interna (SSAS) | Não - SSAS é separado [PaaS](https://docs.microsoft.com/azure/analysis-services/analysis-services-overview) |
| Serviço interno de relatório (SSRS) | Não-use [Power bi relatórios paginados](https://docs.microsoft.com/power-bi/paginated-reports/paginated-reports-report-builder-power-bi) em vez ou hospede o SSRS na VM do Azure. Embora Instância Gerenciada não possa executar o SSRS como um serviço, ele pode hospedar bancos de dados de catálogo SSRS 2019 para um servidor de relatórios externo usando a autenticação SQL Server. |
|||

## <a name="vcore-based-purchasing-model"></a>Modelo de compra baseado em vCore

O [modelo de compra baseado em vCore](sql-database-service-tiers-vcore.md) para instâncias gerenciadas oferece flexibilidade, controle, transparência e uma maneira direta de traduzir os requisitos de carga de trabalho local para a nuvem. Este modelo permite que você altere computação, memória e armazenamento com base em suas necessidades de carga de trabalho. O modelo vCore também é elegível para economia de até 55% com o [benefício híbrido do Azure para SQL Server](https://azure.microsoft.com/pricing/hybrid-benefit/).

No modelo vCore, você pode escolher entre gerações de hardware.

- **Gen4** As CPUs lógicas são baseadas em processadores Intel E5-2673 v3 (Haswell) 2,4-GHz, SSD anexado, núcleos físicos, 7 GB de RAM por núcleo e tamanhos de computação entre 8 e 24 vCores.
- **Gen5** As CPUs lógicas são baseadas em processadores Intel E5-2673 V4 (Broadwell) 2,3-GHz e Intel SP-8160 (Skylake), SSD Fast NVMe, núcleo lógico Hyper-thread e tamanhos de computação entre 4 e 80 núcleos.

Encontre mais informações sobre a diferença entre as gerações de hardware em [limites de recursos de instância gerenciada](sql-database-managed-instance-resource-limits.md#hardware-generation-characteristics).

## <a name="managed-instance-service-tiers"></a>Camadas de serviço de instância gerenciada

A instância gerenciada está disponível em duas camadas de serviço:

- **Uso geral**: projetado para aplicativos com requisitos típicos de desempenho e latência de e/s.
- **Comercialmente crítico**: projetado para aplicativos com requisitos de latência de e/s baixos e impacto mínimo sobre operações de manutenção subjacentes na carga de trabalho.

Ambas as camadas de serviço garantem 99,99% de disponibilidade e permitem que você selecione de maneira independente o tamanho do armazenamento e a capacidade de computação. Para obter mais informações sobre a arquitetura de alta disponibilidade do Banco de Dados SQL do Azure, veja [Alta disponibilidade e Banco de Dados SQL do Azure](sql-database-high-availability.md).

### <a name="general-purpose-service-tier"></a>Camada de serviço de uso geral

A lista a seguir apresenta a principal característica da camada de serviço de Uso Geral:

- Design para a maioria dos aplicativos de negócios com requisitos de desempenho típicos
- Armazenamento de Blobs do Azure de alto desempenho (8 TB)
- [Alta disponibilidade](sql-database-high-availability.md#basic-standard-and-general-purpose-service-tier-availability) integrada, baseada no confiável Armazenamento de Blobs do Azure e no [Azure Service Fabric](../service-fabric/service-fabric-overview.md)

Para obter mais informações, veja [Camada de armazenamento em Camada de usos gerais](https://medium.com/azure-sqldb-managed-instance/file-layout-in-general-purpose-azure-sql-managed-instance-cf21fff9c76c) e [práticas recomendadas de desempenho de armazenamento e considerações para instâncias gerenciadas (uso geral)](https://blogs.msdn.microsoft.com/sqlcat/2018/07/20/storage-performance-best-practices-and-considerations-for-azure-sql-db-managed-instance-general-purpose/).

Encontre mais informações sobre a diferença entre as camadas de serviço em [limites de recursos de instância gerenciada](sql-database-managed-instance-resource-limits.md#service-tier-characteristics).

### <a name="business-critical-service-tier"></a>Camada de serviço comercialmente crítica

Camada de serviço Comercialmente Crítico desenvolvida para aplicativos com altos requisitos de E/S. Ele oferece maior resiliência a falhas usando várias réplicas isoladas.

A lista a seguir apresenta as características principais da camada de serviço Comercialmente Crítico:

- Projetada para aplicativos de negócios com requisitos de alta disponibilidade e desempenho mais alto
- Vem com armazenamento SSD local super rápido (até 1 TB no Gen 4 e até 4 TB no Gen 5)
- [Alta disponibilidade](sql-database-high-availability.md#premium-and-business-critical-service-tier-availability) interna com base em [grupos de disponibilidade Always on](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server) e [Service Fabric do Azure](../service-fabric/service-fabric-overview.md).
- Réplica de banco de [dados somente leitura](sql-database-read-scale-out.md) interna adicional que pode ser usada para relatórios e outras cargas de trabalho somente leitura
- [OLTP In-Memory](sql-database-in-memory.md) que pode ser usado para carga de trabalho com requisitos de alto desempenho  

Encontre mais informações sobre a diferença entre as camadas de serviço em [limites de recursos de instância gerenciada](sql-database-managed-instance-resource-limits.md#service-tier-characteristics).


## <a name="managed-instance-management-operations"></a>Operações de gerenciamento de instâncias gerenciadas

O banco de dados SQL do Azure fornece operações de gerenciamento que você pode usar para implantar automaticamente novas instâncias gerenciadas, atualizar propriedades de instância e excluir instâncias quando não forem mais necessárias. Esta seção fornece informações sobre operações de gerenciamento e suas durações típicas.

Para dar suporte a [implantações em redes virtuais do Azure (VNets)](../virtual-network/virtual-network-for-azure-services.md) e fornecer isolamento e segurança para clientes, a instância gerenciada depende de [clusters virtuais](sql-database-managed-instance-connectivity-architecture.md#high-level-connectivity-architecture), que representam um conjunto dedicado de máquinas virtuais isoladas implantadas dentro da sub-rede da rede virtual do cliente. Essencialmente, cada implantação de instância gerenciada em uma sub-rede vazia resulta em um novo build-out de cluster virtual.

As operações subsequentes em instâncias gerenciadas implantadas também podem ter efeitos sobre seu cluster virtual subjacente. Isso afeta a duração das operações de gerenciamento, pois a implantação de máquinas virtuais adicionais vem com uma sobrecarga que precisa ser considerada quando você planeja novas implantações ou atualizações em instâncias gerenciadas existentes.

Todas as operações de gerenciamento podem ser categorizadas da seguinte maneira:

- Implantação de instância (nova criação de instância). 
- Atualização de instância (alterando Propriedades de instância, como vCores ou armazenamento reservado.
- Exclusão da instância.

Normalmente, as operações em clusters virtuais são mais longas. A duração das operações em clusters virtuais varia – abaixo estão os valores que você normalmente pode esperar, com base em dados de telemetria de serviço existentes:

- Criação de cluster virtual. Essa é uma etapa síncrona em operações de gerenciamento de instância. **90% das operações são concluídas em 4 horas**.
- Redimensionamento do cluster virtual (expansão ou redução). A expansão é uma etapa síncrona, enquanto a redução é executada de forma assíncrona (sem afetar a duração das operações de gerenciamento de instância). **90% das expansões de cluster são concluídas em menos de 2,5 horas**.
- Exclusão do cluster virtual. A exclusão é uma etapa assíncrona, mas também pode ser [iniciada manualmente](sql-database-managed-instance-delete-virtual-cluster.md) em um cluster virtual vazio; nesse caso, ela é executada de forma síncrona. **90% das exclusões do cluster virtual são concluídas em 1,5 horas**.

Além disso, o gerenciamento de instâncias também pode incluir uma das operações em bancos de dados hospedados, o que resulta em durações mais longas:

- Anexando arquivos de banco de dados do armazenamento do Azure. Essa é uma etapa síncrona, como computação (vCore) ou expansão de armazenamento para cima ou para baixo na camada de serviço Uso Geral. **90% dessas operações são concluídas em 5 minutos**.
- Always On a propagação do grupo de disponibilidade. Essa é uma etapa síncrona, como computação (vCore) ou dimensionamento de armazenamento na camada de serviço Comercialmente Crítico, bem como na alteração da camada de serviço de Uso Geral para Comercialmente Crítico (ou vice-versa). A duração dessa operação é proporcional ao tamanho total do banco de dados, bem como à atividade atual do banco de dados (número de transações ativas). A atividade do banco de dados ao atualizar uma instância pode introduzir uma variação significativa na duração total. **90% dessas operações são executadas em 220 GB/hora ou superior**.

A tabela a seguir resume as operações e as durações gerais típicas:

|Categoria  |Operação  |Segmento de execução longa  |Duração estimada  |
|---------|---------|---------|---------|
|**Implantação** |Primeira instância em uma sub-rede vazia|Criação de cluster virtual|90% das operações terminam em 4 horas|
|Implantação |Primeira instância de outra geração de hardware em uma sub-rede não vazia (por exemplo, primeira instância de Gen 5 em uma sub-rede com instâncias de Gen 4)|Criação de cluster virtual *|90% das operações terminam em 4 horas|
|Implantação |Primeira instância de criação de 4 vCores, em uma sub-rede vazia ou não vazia|Criação de cluster virtual * *|90% das operações terminam em 4 horas|
|Implantação |Criação de instância subsequente dentro da sub-rede não vazia (2ª, 3ª, etc. instância)|Redimensionamento de cluster virtual|90% das operações são concluídas em 2,5 horas|
|**Atualização** |Alteração da propriedade de instância (senha de administrador, logon do AAD, Benefício Híbrido do Azure Flag)|N/D|Até 1 minuto|
|Atualizar |Expansão/redução do armazenamento de instância (Uso Geral camada de serviço)|Anexando arquivos de banco de dados|90% das operações são concluídas em 5 minutos|
|Atualizar |Expansão/redução do armazenamento de instância (Comercialmente Crítico camada de serviço)|-Redimensionamento de cluster virtual<br>-Always On propagação do grupo de disponibilidade|90% das operações são concluídas em 2,5 horas + tempo para propagar todos os bancos de dados (220 GB/hora)|
|Atualizar |Expansão e redução da vCores (computação de instância) (Uso Geral)|-Redimensionamento de cluster virtual<br>-Anexando arquivos de banco de dados|90% das operações são concluídas em 2,5 horas|
|Atualizar |Expansão e redução da vCores (computação de instância) (Comercialmente Crítico)|-Redimensionamento de cluster virtual<br>-Always On propagação do grupo de disponibilidade|90% das operações são concluídas em 2,5 horas + tempo para propagar todos os bancos de dados (220 GB/hora)|
|Atualizar |Expansão da instância para 4 vCores (Uso Geral)|-Redimensionamento do cluster virtual (se for feito pela primeira vez, pode exigir a criação do cluster virtual * *)<br>-Anexando arquivos de banco de dados|90% das operações terminam em 4 h 5 min * *|
|Atualizar |Expansão da instância para 4 vCores (Comercialmente Crítico)|-Redimensionamento do cluster virtual (se for feito pela primeira vez, pode exigir a criação do cluster virtual * *)<br>-Always On propagação do grupo de disponibilidade|90% das operações são concluídas em 4 horas + tempo para propagar todos os bancos de dados (220 GB/hora)|
|Atualizar |Alteração da camada de serviço de instância (Uso Geral para Comercialmente Crítico e vice-versa)|-Redimensionamento de cluster virtual<br>-Always On propagação do grupo de disponibilidade|90% das operações são concluídas em 2,5 horas + tempo para propagar todos os bancos de dados (220 GB/hora)|
|**Exclusão**|Exclusão da instância|Backup da parte final do log para todos os bancos de dados|90% de operações concluídas em até 1 minuto.<br>Observação: se a última instância na sub-rede for excluída, esta operação agendará a exclusão do cluster virtual após 12 horas * * *|
|Exclusão|Exclusão de cluster virtual (como operação iniciada pelo usuário)|Exclusão do cluster virtual|90% das operações terminam em até 1,5 horas|

\*O cluster virtual é criado por geração de hardware.

\*\*A opção de implantação 4 vCores foi lançada em junho de 2019 e requer uma nova versão de cluster virtual. Se você tivesse instâncias na sub-rede de destino que foram todas criadas antes de 12 de junho, um novo cluster virtual será implantado automaticamente para hospedar 4 instâncias vCore.

\*\*\*12 horas é a configuração atual, mas isso pode mudar no futuro, portanto, não faça uma dependência difícil. Se você precisar excluir um cluster virtual anteriormente (para liberar a sub-rede, por exemplo), consulte [excluir uma sub-rede depois de excluir uma instância gerenciada do banco de dados SQL do Azure](sql-database-managed-instance-delete-virtual-cluster.md).

### <a name="instance-availability-during-management"></a>Disponibilidade da instância durante o gerenciamento

As instâncias gerenciadas não estão disponíveis para aplicativos cliente durante operações de implantação e exclusão.

As instâncias gerenciadas estão disponíveis durante operações de atualização, mas há um breve tempo de inatividade causado pelo failover que ocorre no final das atualizações que normalmente duram até 10 segundos. A exceção a isso é a atualização do espaço de armazenamento reservado na camada de serviço Uso Geral que não provoca failover nem afeta a disponibilidade da instância.

> [!IMPORTANT]
> A duração de um failover pode variar significativamente no caso de transações de longa execução que ocorrem nos bancos de dados devido a [tempo de recuperação prolongado](sql-database-accelerated-database-recovery.md#the-current-database-recovery-process). Portanto, não é recomendável dimensionar a computação ou o armazenamento da instância gerenciada do banco de dados SQL do Azure ou alterar a camada de serviço ao mesmo tempo com as transações de longa execução (importação de dados, trabalhos de processamento de dados, recompilação de índice, etc.). O failover de banco de dados que será executado no final da operação cancelará as transações contínuas e resultará em tempo de recuperação prolongado.

> [!TIP]
> A atualização do espaço de armazenamento reservado na camada de serviço Uso Geral não incorre em failover nem afeta a disponibilidade da instância.

A [recuperação de banco de dados acelerada](sql-database-accelerated-database-recovery.md) não está disponível atualmente para instâncias gerenciadas do banco de dados SQL do Azure. Uma vez habilitado, esse recurso reduzirá significativamente a variabilidade do tempo de failover, mesmo no caso de transações de longa execução.

### <a name="canceling-management-operations"></a>Cancelando operações de gerenciamento

A tabela a seguir resume a capacidade de cancelar operações específicas de gerenciamento e as durações gerais típicas:

Categoria  |Operação  |Cancelável  |Duração estimada de cancelamento  |
|---------|---------|---------|---------|
|Implantação |Criação de instância |Não |  |
|Atualizar |Expansão/redução do armazenamento de instância (Uso Geral) |Não |  |
|Atualizar |Expansão/redução do armazenamento de instância (Comercialmente Crítico) |Sim |90% das operações são concluídas em 5 minutos |
|Atualizar |Expansão e redução da vCores (computação de instância) (Uso Geral) |Sim |90% das operações são concluídas em 5 minutos |
|Atualizar |Expansão e redução da vCores (computação de instância) (Comercialmente Crítico) |Sim |90% das operações são concluídas em 5 minutos |
|Atualizar |Alteração da camada de serviço de instância (Uso Geral para Comercialmente Crítico e vice-versa) |Sim |90% das operações são concluídas em 5 minutos |
|Excluir |Exclusão da instância |Não |  |
|Excluir |Exclusão de cluster virtual (como operação iniciada pelo usuário) |Não |  |

Para cancelar a operação de gerenciamento, vá para a folha visão geral e clique na caixa de notificação de operação em andamento. Do lado direito, uma tela com operação em andamento será exibida e haverá um botão para a operação de cancelamento. Depois de clicar em primeiro, você será solicitado a clicar novamente e confirmar que deseja cancelar a operação.

[![](./media/sql-database-managed-instance/canceling-operation.png)](./media/sql-database-managed-instance/canceling-operation.png#lightbox)

Depois que a solicitação de cancelamento for enviada e processada, você receberá uma notificação se cancelar envio tiver sido bem-sucedido ou não. 

No caso de cancelamento de sucesso, a operação de gerenciamento será cancelada em alguns minutos, resultando em uma falha.

![Cancelando resultado da operação](./media/sql-database-managed-instance/canceling-operation-result.png)

Se o botão Cancelar solicitação falhar ou cancelar não estiver ativo, isso significará que a operação de gerenciamento inseriu o estado não cancelável e que será concluída em alguns minutos. A operação de gerenciamento continuará sua execução até que seja concluída.

> [!IMPORTANT]
> No momento, há suporte para a operação de cancelamento no Portal.

## <a name="advanced-security-and-compliance"></a>Segurança e conformidade avançadas

A opção de implantação da instância gerenciada combina os recursos de segurança avançados fornecidos pela nuvem do Azure e pelo Mecanismo de Banco de Dados do SQL Server.

### <a name="managed-instance-security-isolation"></a>Isolamento de segurança da instância gerenciada

A instância gerenciada fornece isolamento de segurança adicional de outros locatários na nuvem do Azure. O isolamento de segurança inclui:

- [Implementação de rede virtual nativa](sql-database-managed-instance-connectivity-architecture.md) e conectividade com seu ambiente local usando o gateway de VPN ou a rota expressa do Azure.
- Em uma implantação padrão, o ponto de extremidade SQL é exposto somente por meio de um endereço IP privado, permitindo conectividade segura de redes privadas ou do Azure privada.
- Locatário único com infraestrutura subjacente dedicada (computação, armazenamento).

O diagrama a seguir descreve várias opções de conectividade para seus aplicativos:

![alta disponibilidade](./media/sql-database-managed-instance/application-deployment-topologies.png)  

Para obter mais detalhes sobre a integração VNet e a imposição de políticas de rede no nível da sub-rede, confira [Arquitetura de VNet para instâncias gerenciadas](sql-database-managed-instance-connectivity-architecture.md) e [Conectar seu aplicativo à instância gerenciada](sql-database-managed-instance-connect-app.md).

> [!IMPORTANT]
> Coloque várias instâncias gerenciadas na mesma sub-rede, onde quer que o permitido pela seus requisitos de segurança, como o que trará benefícios adicionais. Disposição das instâncias na mesma sub-rede será significativamente simplificar a manutenção de infraestrutura de rede e reduzir o tempo, o provisionamento, pois o provisionamento de longa duração está associado ao custo de implantação de primeira instância gerenciada em uma sub-rede de instância.

### <a name="azure-sql-database-security-features"></a>Recursos de segurança do Banco de Dados SQL do Azure

O Banco de Dados SQL do Azure fornece um conjunto de recursos de segurança avançados que podem ser usados para proteger os dados.

- A [auditoria de instância gerenciada](sql-database-managed-instance-auditing.md) rastreia eventos de banco de dados e os grava em um arquivo de log de auditoria colocado em sua conta de armazenamento do Azure. A auditoria pode ajudar a manter conformidade com as normas, entender a atividade do banco de dados e ter ideia das discrepâncias e anomalias que podem gerar preocupações comerciais ou violações suspeitas de seguranças.
- Criptografia de dados em movimento – a instância gerenciada protege os dados, fornecendo criptografia para dados em movimento usando a segurança de camada de transporte. Além do protocolo TLS, a opção de implantação da instância gerenciada oferece proteção de dados confidenciais em trânsito, em repouso e durante processamento de consulta com [Always Encrypted](/sql/relational-databases/security/encryption/always-encrypted-database-engine). Always Encrypted é pioneiro na indústria, oferecendo segurança de dados incomparável contra violações envolvendo o roubo de dados críticos. Por exemplo, com o Always Encrypted, números de cartão de crédito são armazenados sempre criptografados no banco de dados, mesmo durante a consulta de processamento, permitindo a descriptografia no ponto de uso por pessoal autorizado ou aplicativos que precisam processar os dados.
- A [proteção avançada contra ameaças](sql-database-managed-instance-threat-detection.md) complementa a [auditoria](sql-database-managed-instance-auditing.md) , fornecendo uma camada adicional de inteligência de segurança incorporada ao serviço que detecta tentativas incomuns e potencialmente prejudiciais de acessar ou explorar bancos de dados. Você é alertado sobre atividades suspeitas, vulnerabilidades potenciais, ataques de injeção de SQL, bem como padrões de acesso do banco de dados anormais. Alertas de proteção avançada contra ameaças podem ser exibidos na [central de segurança do Azure](https://azure.microsoft.com/services/security-center/) e fornecem detalhes de atividades suspeitas e recomendação de ações sobre como investigar e atenuar a ameaça.  
- [Dynamic data masking](/sql/relational-databases/security/dynamic-data-masking) limits sensitive data exposure by masking it to non-privileged users. A máscara de dados dinâmicos ajuda a impedir o acesso não autorizado a dados confidenciais, permitindo que você designe a quantidade de dados confidenciais a ser revelada, com impacto mínimo sobre a camada de aplicativo. Ele é um recurso de segurança baseado em políticas que oculta os dados confidenciais no conjunto de resultados de uma consulta em relação aos campos do banco de dados designados, sem que os dados no banco de dados sejam alterados.
- A [segurança em nível de linha](/sql/relational-databases/security/row-level-security) permite controlar o acesso às linhas em uma tabela de banco de dados com base nas características do usuário que executa uma consulta (por exemplo, uma associação de grupo ou um contexto de execução). A RLS (Segurança em Nível de Linha) simplifica o design e codificação de segurança em seu aplicativo. Ela permite implementar restrições de acesso à linha de dados. Por exemplo, garantindo que os funcionários possam acessar apenas as linhas de dados pertinentes ao seu departamento ou restringindo o acesso a dados apenas para dados relevantes.
- A [TDE (Transparent Data Encryption)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql) criptografa arquivos de dados de instância gerenciada, conhecido como dados de criptografia em repouso. A TDE realiza a criptografia e a descriptografia de E/S em tempo real dos arquivos de log e de dados. A criptografia usa uma chave de criptografia de banco de dados (DEK), que é armazenada no registro de inicialização do banco de dados para disponibilidade durante a recuperação. Você pode proteger todos os seus bancos de dados na instância gerenciada com a transparent data encryption. O TDE é uma tecnologia comprovada de criptografia em repouso SQL Server que é exigida por muitos padrões de conformidade para proteger contra roubo de mídia de armazenamento.

Há suporte para a migração de um banco de dados criptografado para a instância gerenciada por meio do DMS (Serviço de Migração de Banco de Dados) do Azure ou restauração nativa. Se você planeja migrar um banco de dados criptografado usando a restauração nativa, a migração do certificado TDE existente do SQL Server local ou SQL Server em uma máquina virtual para uma instância gerenciada é uma etapa necessária. Para obter mais informações sobre as opções de migração consulte [migração da instância do SQL Server para a instância gerenciada](sql-database-managed-instance-migrate.md).

## <a name="azure-active-directory-integration"></a>Integração do Active Directory do Azure

A opção de implantação de instância gerenciada dá suporte a logons e logons do mecanismo de Banco de Dados do SQL Server e logons integrados ao AAD (Azure Active Directory). As entidades de servidor do Azure AD (logons, **versão prévia pública**) são uma versão de nuvem do Azure de logons de banco de dados locais do Windows que você está usando no ambiente local. As entidades de segurança do servidor do Azure AD (logons) permitem que você especifique usuários e grupos do seu locatário Azure Active Directory como entidades de segurança com escopo de instância verdadeiras, capaz de executar qualquer operação em nível de instância, incluindo consultas entre bancos de dados dentro da mesma instância gerenciada.

Uma nova sintaxe é introduzida para criar entidades de segurança do servidor do Azure AD (logons), **do provedor externo**. Para obter mais informações sobre a sintaxe, consulte <a href="/sql/t-sql/statements/create-login-transact-sql?view=azuresqldb-mi-current">criar logon</a>e examine o artigo [provisionar um administrador de Azure Active Directory para sua instância gerenciada](sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-managed-instance) .

### <a name="azure-active-directory-integration-and-multi-factor-authentication"></a>Integração do Azure Active Directory e autenticação multifator

A opção de implantação de instância gerenciada permite gerenciar centralmente as identidades de usuário do banco de dados e de outros serviços da Microsoft com a [integração do Azure Active Directory](sql-database-aad-authentication.md). Esse recurso simplifica o gerenciamento de permissão e aprimora a segurança. O Azure Active Directory é compatível com [MFA](sql-database-ssms-mfa-authentication-configure.md) (autenticação multifator) para aumentar a segurança de aplicativos e dados e dá suporte a um processo de logon único.

### <a name="authentication"></a>Autenticação

A autenticação de instância gerenciada refere-se a como os usuários comprovam a identidade ao conectarem-se ao banco de dados. O Banco de Dados SQL dá suporte a dois tipos de autenticação:  

- **Autenticação do SQL**:

  Este método de autenticação usa um nome de usuário e senha.
- **Autenticação do Active Directory do Azure**:

  Esse método de autenticação usa identidades gerenciadas pelo Azure Active Directory e tem suporte para domínios gerenciados e integrados. Use autenticação do Active Directory (segurança integrada) [sempre que possível](https://docs.microsoft.com/sql/relational-databases/security/choose-an-authentication-mode).

### <a name="authorization"></a>Autorização

Autorização refere-se ao que um usuário pode fazer em um Banco de Dados SQL do Azure e é controlado pelas associações a uma função de banco de dados e pelas permissões no nível de objeto da conta de usuário. Uma instância gerenciada tem os mesmos recursos de autorização que o SQL Server 2017.

## <a name="database-migration"></a>Migração de banco de dados

A opção de implantação de instância gerenciada destina-se a cenários de usuários com migração de banco de dados em massa de implementações de bancos de dados locais ou IaaS. A instância gerenciada oferece suporte a várias opções de migração de banco de dados:

### <a name="back-up-and-restore"></a>Fazer backup e restaurar  

A abordagem de migração aproveita backups do SQL para o Armazenamento de Blobs do Azure. Os backups armazenados no blob de armazenamento do Azure podem ser restaurados diretamente na instância gerenciada usando o [comando T-SQL RESTORE](https://docs.microsoft.com/sql/t-sql/statements/restore-statements-transact-sql?view=azuresqldb-mi-current).

- Para obter um guia de início rápido mostrando como restaurar a Wide World Importers-arquivo de backup de banco de dados padrão, consulte [restaurar um arquivo de backup para uma instância gerenciada](sql-database-managed-instance-get-started-restore.md). Este guia de início rápido mostra que você precisa carregar um arquivo de backup no armazenamento de BLOBs do Azure e protegê-lo usando uma chave de assinatura de acesso compartilhado (SAS).
- Para obter informações sobre restauração de URL, consulte [Restauração nativa de URL](sql-database-managed-instance-migrate.md#native-restore-from-url).

> [!IMPORTANT]
> Os backups de uma instância gerenciada só podem ser restaurados para outra instância gerenciada. Eles não podem ser restaurados para um SQL Server local ou para um pool elástico/banco de dados individual.

### <a name="data-migration-service"></a>Serviço de Migração de Dados

O Serviço de Migração de Banco de Dados do Azure é um serviço totalmente gerenciado projetado para permitir migrações perfeitas de várias fontes de banco de dados para plataformas de dados do Azure com um tempo de inatividade mínimo. Esse serviço simplifica as tarefas necessárias para mover bancos de dados do SQL Server e de terceiros existentes para o Banco de Dados SQL do Azure (bancos de dados individuais, bancos de dados agrupados em pools elásticos e bancos de dados de instância em uma instância gerenciada) e o SQL Server na VM do Azure. Veja [Como migrar o banco de dados local para a instância gerenciada usando DMS](https://aka.ms/migratetoMIusingDMS).

## <a name="sql-features-supported"></a>Recursos do SQL com suporte

A opção de implantação de instância gerenciada tem como objetivo entregar aproximadamente 100% de compatibilidade de área de superfície com o SQL Server local em etapas, até a disponibilidade geral do serviço. Para uma lista de recursos e comparação, consulte [Comparação de recursos do Banco de Dados SQL](sql-database-features.md) e para uma lista de diferenças de T-SQL em instâncias gerenciadas em comparação com o SQL Server, consulte [Diferenças do T-SQL da instância gerenciada do SQL Server](sql-database-managed-instance-transact-sql-information.md).

A opção de implantação de instância gerenciada dá suporte à compatibilidade com versões anteriores para bancos de dados SQL 2008. A migração direta dos servidores do Banco de Dados do SQL 2005 tem suporte, o nível de compatibilidade para Bancos de Dados do SQL 2005 migrados é atualizado para o SQL 2008.
  
O diagrama a seguir apresenta a compatibilidade da área de superfície na instância gerenciada:  

![migração](./media/sql-database-managed-instance/migration.png)

### <a name="key-differences-between-sql-server-on-premises-and-in-a-managed-instance"></a>Principais diferenças entre SQL Server local e instância gerenciada

A opção de implantação de instância gerenciada se beneficia de estar sempre atualizada na nuvem, o que significa que alguns recursos no SQL Server local podem estar obsoletos, desativados ou ter alternativas. Há casos específicos em que as ferramentas precisam reconhecer que um recurso específico funciona de forma ligeiramente diferente ou que o serviço está em execução em um ambiente que você não controla totalmente:

- Alta disponibilidade baseia-se e pré-configurado usando a tecnologia semelhante [grupos de disponibilidade AlwaysOn](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server).
- Backups automatizados e restauração pontual. O cliente pode iniciar backups `copy-only` que não interferem na cadeia de backup automático.
- A instância gerenciada não permite especificar caminhos físicos completos para que todos os cenários correspondentes tenham suporte diferentemente: RESTOre DB não dá suporte ao MOVE, CREATE DB não permite caminhos físicos, BULK INSERT funciona apenas com BLOBs do Azure, etc.
- A instância gerenciada dá suporte à [autenticação do Azure ad](sql-database-aad-authentication.md) como alternativa à nuvem para autenticação do Windows.
- A instância gerenciada gerencia automaticamente o grupo de arquivos XTP e os arquivos para bancos de dados que contêm objetos OLTP in-memory
- A instância gerenciada dá suporte a SQL Server Integration Services (SSIS) e pode hospedar o catálogo do SSIS (SSISDB) que armazena os pacotes do SSIS, mas eles são executados em um Azure-SSIS Integration Runtime gerenciado (IR) no Azure Data Factory (ADF), consulte [criar Azure-SSIS ir no ADF](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime). Para comparar os recursos do SSIS no banco de dados SQL, consulte comparar um banco de dados [SQL do Azure banco de dados individual, pool elástico e instância gerenciada](../data-factory/create-azure-ssis-integration-runtime.md#comparison-of-a-sql-database-single-database-elastic-pool-and-managed-instance).

### <a name="managed-instance-administration-features"></a>Recursos de administração de instância gerenciada

A opção de implantação de instância gerenciada permite que o administrador do sistema gaste menos tempo em tarefas administrativas porque o serviço de Banco de Dados SQL as executa para você ou simplifica bastante essas tarefas. Por exemplo, [instalação de so/RDBMS e aplicação de patches](sql-database-high-availability.md), [redimensionamento e configuração de instância dinâmica](sql-database-single-database-scale.md), [backups](sql-database-automated-backups.md), [replicação de banco](replication-with-sql-database-managed-instance.md) de dados (incluindo bancos de dados do sistema), [configuração de alta disponibilidade](sql-database-high-availability.md)e configuração de fluxos de monitoramento de integridade e [desempenho](../azure-monitor/insights/azure-sql.md) .

> [!IMPORTANT]
> Para obter uma lista de recursos com suporte, suporte parcial e sem suporte, consulte [Recursos do Banco de Dados SQL](sql-database-features.md). Para obter uma lista de diferenças de T-SQL em instâncias gerenciadas versus SQL Server, consulte [diferenças de t-SQL de instância gerenciada de SQL Server](sql-database-managed-instance-transact-sql-information.md)

### <a name="how-to-programmatically-identify-a-managed-instance"></a>Como identificar programaticamente uma instância gerenciada

A tabela a seguir mostra várias propriedades, acessíveis por meio do Transact-SQL que podem ser usadas para detectar se o aplicativo está trabalhando com a instância gerenciada e recuperar propriedades importantes.

|Propriedade|Valor|Comentário|
|---|---|---|
|`@@VERSION`|Microsoft SQL Azure (RTM) - 12.0.2000.8 07/03/2018 Copyright (C) 2018 Microsoft Corporation.|Esse valor é o mesmo que no Banco de Dados SQL. Isso **não** indica o mecanismo SQL versão 12 (SQL Server 2014). A instância gerenciada sempre executa a versão mais recente do mecanismo SQL estável, que é igual ou superior à versão RTM mais recente disponível do SQL Server.  |
|`SERVERPROPERTY ('Edition')`|SQL Azure|Esse valor é o mesmo que no Banco de Dados SQL.|
|`SERVERPROPERTY('EngineEdition')`|8|Esse valor identifica exclusivamente a instância gerenciada.|
|`@@SERVERNAME`, `SERVERPROPERTY ('ServerName')`|Nome DNS da instância completo no seguinte formato:`<instanceName>`.`<dnsPrefix>`.database.windows.net, em que `<instanceName>` é o nome fornecido pelo cliente, enquanto `<dnsPrefix>` é a parte gerada automaticamente do nome, garantindo exclusividade de nome DNS global ("wcus17662feb9ce98", por exemplo)|Exemplo: my-managed-instance.wcus17662feb9ce98.database.windows.net|

## <a name="next-steps"></a>Próximas etapas

- Para saber como criar sua primeira instância gerenciada, consulte [Guia de início rápido](sql-database-managed-instance-get-started.md).
- Para obter uma lista de recursos e de comparação, consulte [Recursos comuns do SQL](sql-database-features.md).
- Para obter mais informações sobre a configuração de VNet, consulte [configuração de vnet de instância gerenciada](sql-database-managed-instance-connectivity-architecture.md).
- Para obter um início rápido que cria uma instância gerenciada e restaura um banco de dados de um arquivo de backup, consulte [criar uma instância gerenciada](sql-database-managed-instance-get-started.md).
- Para obter um tutorial usando o serviço de migração de banco de dados do Azure (DMS) para migração, consulte [migração de instância gerenciada usando DMS](../dms/tutorial-sql-server-to-managed-instance.md).
- Para monitoramento avançado do desempenho do banco de dados de instância gerenciada com inteligência de solução de problemas interna, consulte [monitorar o banco de dados SQL do Azure usando análise de SQL do Azure](../azure-monitor/insights/azure-sql.md).
- Para obter informações sobre preços, consulte [preços da instância gerenciada do banco de dados SQL](https://azure.microsoft.com/pricing/details/sql-database/managed/).

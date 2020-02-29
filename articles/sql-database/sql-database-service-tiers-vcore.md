---
title: Visão geral do modelo vCore
description: O modelo de compra vCore permite que você dimensione independentemente os recursos de computação e armazenamento, correspondam ao desempenho local e otimize o preço.
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: sashan, moslake, carlrab
ms.date: 11/27/2019
ms.openlocfilehash: e53fb46b7c13e1feb0cc24663fb0782b4de06f2b
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/29/2020
ms.locfileid: "78198024"
---
# <a name="vcore-model-overview"></a>Visão geral do modelo vCore

O modelo vCore (núcleo virtual) fornece vários benefícios:

- Limites de computação, memória, e/s e armazenamento mais altos.
- Controle sobre a geração de hardware para corresponder melhor aos requisitos de computação e de memória da carga de trabalho.
- Descontos de preço para [benefício híbrido do Azure (AHB)](sql-database-azure-hybrid-benefit.md) e [instância reservada (ri)](sql-database-reserved-capacity.md).
- Maior transparência nos detalhes de hardware que alimentam a computação; facilita o planejamento de migrações de implantações locais.

## <a name="service-tiers"></a>Camadas de serviço

As opções da camada de serviço no modelo vCore incluem Uso Geral, Comercialmente Crítico e hiperescala. A camada de serviço geralmente define a arquitetura de armazenamento, os limites de espaço e e/s e as opções de continuidade de negócios relacionadas à disponibilidade e à recuperação de desastres.

||**Propósito geral**|**Comercialmente crítico**|**Em hiperescala**|
|---|---|---|---|
|Mais adequado para|A maioria das cargas de trabalho comerciais. Oferece opções de armazenamento e de computação voltadas para o orçamento, equilibradas e escalonáveis. |Oferece aos aplicativos de negócios a resiliência mais alta para falhas usando várias réplicas isoladas e fornece o desempenho de e/s mais alto por réplica de banco de dados.|A maioria das cargas de trabalho de negócios com requisitos de armazenamento e escala de leitura altamente escalonáveis.  Oferece maior resiliência a falhas, permitindo a configuração de mais de uma réplica de banco de dados isolada. |
|Armazenamento|Usa o armazenamento remoto.<br/>**Computação provisionada de bancos de dados individuais e pools elásticos**:<br/>5 GB – 4 TB<br/>**Computação sem servidor**:<br/>5 GB-3 TB<br/>**Instância gerenciada**: 32 GB-8 TB |Usa o armazenamento SSD local.<br/>**Computação provisionada de bancos de dados individuais e pools elásticos**:<br/>5 GB – 4 TB<br/>**Instância gerenciada**:<br/>32 GB - 4 TB |Crescimento automático flexível do armazenamento, conforme necessário. Dá suporte a até 100 TB de armazenamento. Usa o armazenamento SSD local para o cache do pool de buffers local e o armazenamento de dados local. Usa o armazenamento remoto do Azure como armazenamento de dados de longo prazo final. |
|IOPS e taxa de transferência (aproximado)|**Bancos de dados individuais e pools elásticos**: consulte limites de recursos para [bancos de dados individuais](../sql-database/sql-database-vcore-resource-limits-single-databases.md) e [pools elásticos](../sql-database/sql-database-vcore-resource-limits-elastic-pools.md).<br/>**Instância gerenciada**: consulte [visão geral limites de recursos de instância gerenciada do banco de dados SQL do Azure](../sql-database/sql-database-managed-instance-resource-limits.md#service-tier-characteristics).|Consulte limites de recursos para [bancos de dados individuais](../sql-database/sql-database-vcore-resource-limits-single-databases.md) e [pools elásticos](../sql-database/sql-database-vcore-resource-limits-elastic-pools.md).|O hiperscale é uma arquitetura de várias camadas com cache em vários níveis. O IOPS e a taxa de transferência efetivos dependerão da carga de trabalho.|
|Disponibilidade|1 réplica, sem réplicas de escala de leitura|3 réplicas, 1 [réplica em escala de leitura](sql-database-read-scale-out.md),<br/>alta disponibilidade com redundância de zona (HA)|1 réplica de leitura/gravação, além [de 0-4 réplicas de escala de leitura](sql-database-read-scale-out.md)|
|Backups|[Armazenamento com redundância geográfica com acesso de leitura (ra-grs)](../storage/common/storage-designing-ha-apps-with-ragrs.md), 7-35 dias (7 dias por padrão)|[RA-GRS](../storage/common/storage-designing-ha-apps-with-ragrs.md), 7-35 dias (7 dias por padrão)|Backups baseados em instantâneo no armazenamento remoto do Azure. As restaurações usam esses instantâneos para recuperação rápida. Os backups são instantâneos e não afetam O desempenho de e/s de computação. As restaurações são rápidas e não são uma operação de tamanho de dados (levando minutos em vez de horas ou dias).|
|Na memória|Sem suporte|Suportado|Sem suporte|
|||


### <a name="choosing-a-service-tier"></a>Como escolher uma camada de serviço

Para obter informações sobre como selecionar uma camada de serviço para sua carga de trabalho específica, consulte os seguintes artigos:

- [Quando escolher a camada de serviço de uso geral](sql-database-service-tier-general-purpose.md#when-to-choose-this-service-tier)
- [Quando escolher a camada de serviço Comercialmente Crítico](sql-database-service-tier-business-critical.md#when-to-choose-this-service-tier)
- [Quando escolher a camada de serviço de hiperescala](sql-database-service-tier-hyperscale.md#who-should-consider-the-hyperscale-service-tier)


## <a name="compute-tiers"></a>Camadas de computação

As opções de camada de computação no modelo vCore incluem as camadas de computação provisionada e sem servidor.


### <a name="provisioned-compute"></a>Computação provisionada

A camada de computação provisionada fornece uma quantidade específica de recursos de computação que são continuamente provisionados independentemente da atividade de carga de trabalho e cobra pela quantidade de computação provisionada a um preço fixo por hora.


### <a name="serverless-compute"></a>Computação sem servidor

A [camada de computação sem servidor](sql-database-serverless.md) dimensiona automaticamente os recursos de computação com base na atividade de carga de trabalho e cobra pela quantidade de computação usada por segundo.



## <a name="hardware-generations"></a>Gerações de hardware

As opções de geração de hardware no modelo vCore incluem Gen 4/5, série M (versão prévia) e série Fsv2 (versão prévia). A geração de hardware geralmente define os limites de computação e de memória e outras características que afetam o desempenho da carga de trabalho.

### <a name="gen4gen5"></a>Gen4/Gen5

- O hardware Gen4/Gen5 fornece recursos de computação e memória equilibrados e é adequado para a maioria das cargas de trabalho de banco de dados que não têm mais memória, vCore mais elevado ou requisitos de vCore único mais rápidos, conforme fornecido pela série Fsv2 ou série M.

Para regiões em que o Gen4/Gen5 está disponível, consulte [disponibilidade de Gen4/Gen5](#gen4gen5-1).

### <a name="fsv2-seriespreview"></a>Série Fsv2 (versão prévia)

- A série Fsv2 é uma opção de hardware otimizado para computação que oferece baixa latência de CPU e alta velocidade de clock para a maioria das cargas de trabalho com maior demanda de CPU.
- Dependendo da carga de trabalho, a série Fsv2 pode fornecer mais desempenho de CPU por vCore do que Gen5, e o tamanho vCore de 72 pode fornecer mais desempenho de CPU para menos custo do que 80 vCores em Gen5. 
- O Fsv2 fornece menos memória e tempdb por vCore do que outros Hardwares, de modo que as cargas de trabalho sensíveis a esses limites podem querer considerar a Gen5 ou a série M em vez disso.  

Para regiões em que a Fsv2-Series está disponível, consulte [disponibilidade da série Fsv2](#fsv2-series).


### <a name="m-seriespreview"></a>Série M (visualização)

- A série M é uma opção de hardware com otimização de memória para cargas de trabalho que exigem mais memória e limites de computação mais altos do que o fornecido pelo Gen5.
- A série M fornece 29 GB por vCore e 128 vCores, o que aumenta o limite de memória em relação ao Gen5 por 8x a quase 4 TB.

Para habilitar o hardware da série M para uma assinatura e uma região, uma solicitação de suporte deve ser aberta. A assinatura deve ser um tipo de oferta paga, incluindo pré-pago ou Enterprise Agreement (EA).  Se a solicitação de suporte for aprovada, a experiência de seleção e provisionamento da série M segue o mesmo padrão que para outras gerações de hardware. Para regiões em que a série M está disponível, consulte [disponibilidade da série m](#m-series).


### <a name="compute-and-memory-specifications"></a>Especificações de memória e computação


|Geração de hardware  |Computação  |Memória  |
|:---------|:---------|:---------|
|Gen4     |-Processadores Intel E5-2673 v3 (Haswell) 2,4 GHz<br>-Provisionar até 24 vCores (1 vCore = 1 núcleo físico)  |-7 GB por vCore<br>-Provisionar até 168 GB|
|Gen5     |**Computação provisionada**<br>-Processadores Intel E5-2673 V4 (Broadwell) 2,3-GHz e Intel SP-8160 (Skylake) *<br>-Provisionar até 80 vCores (1 vCore = 1 Hyper-thread)<br><br>**Computação sem servidor**<br>-Processadores Intel E5-2673 V4 (Broadwell) 2,3-GHz e Intel SP-8160 (Skylake) *<br>-Dimensionamento automático para até 16 vCores (1 vCore = 1 Hyper-thread)|**Computação provisionada**<br>-5,1 GB por vCore<br>-Provisionar até 408 GB<br><br>**Computação sem servidor**<br>-Escala automática de até 24 GB por vCore<br>-Escala automática de até 48 GB máx.|
|Série Fsv2     |-Processadores Intel Xeon Platinum 8168 (SkyLake)<br>-Apresentando uma velocidade de clock de Turbo principal de 3,4 GHz e uma velocidade máxima de clock de único núcleo de 3,7 GHz.<br>-Provisionar 72 vCores (1 vCore = 1 Hyper-thread)|-1,9 GB por vCore<br>-Provisionar 136 GB|
|Série M     |-Processadores Intel Xeon E7-8890 v3 2,5 GHz<br>-Provisionar 128 vCores (1 vCore = 1 Hyper-thread)|-29 GB por vCore<br>-Provisionar 3,7 TB|

\* na exibição de gerenciamento dinâmico [Sys. dm_user_db_resource_governance](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-user-db-resource-governor-azure-sql-database) , a geração de hardware para bancos de dados Gen5 usando processadores Intel SP-8160 (Skylake) aparece como Gen6. Os limites de recursos para todos os bancos de dados do Gen5 são os mesmos, independentemente do tipo de processador (Broadwell ou Skylake).

Para obter mais informações sobre limites de recursos, consulte [limites de recursos para VCORE (bancos de dados individuais)](sql-database-vcore-resource-limits-single-databases.md)ou [limites de recursos para pools elásticos (VCORE)](sql-database-vcore-resource-limits-elastic-pools.md).

### <a name="selecting-a-hardware-generation"></a>Selecionando uma geração de hardware

No portal do Azure, você pode selecionar a geração de hardware para um banco de dados SQL ou pool no momento da criação ou pode alterar a geração de hardware de um pool ou banco de dados SQL existente.

**Para selecionar uma geração de hardware ao criar um banco de dados SQL ou pool**

Para obter informações detalhadas, consulte [criar um banco de dados SQL](sql-database-single-database-get-started.md).

Na guia **noções básicas** , selecione o link **configurar banco de dados** na seção **computação + armazenamento** e, em seguida, selecione o link **Alterar configuração** :

  ![configurar o banco de dados](media/sql-database-service-tiers-vcore/configure-sql-database.png)

Selecione a geração de hardware desejada:

  ![selecionar hardware](media/sql-database-service-tiers-vcore/select-hardware.png)


**Para alterar a geração de hardware de um pool ou banco de dados SQL existente**

Para um banco de dados, na página Visão geral, selecione o link **tipo de preço** :

  ![alterar hardware](media/sql-database-service-tiers-vcore/change-hardware.png)

Para um pool, na página Visão geral, selecione **Configurar**.

Siga as etapas para alterar a configuração e selecione a geração de hardware conforme descrito nas etapas anteriores.

**Para selecionar uma geração de hardware ao criar uma instância gerenciada**

Para obter informações detalhadas, consulte [criar uma instância gerenciada](sql-database-managed-instance-get-started.md).

Na guia **noções básicas** , selecione o link **configurar banco de dados** na seção **computação + armazenamento** e selecione a geração de hardware desejada:

  ![configurar instância gerenciada](media/sql-database-service-tiers-vcore/configure-managed-instance.png)
  
**Para alterar a geração de hardware de uma instância gerenciada existente**

# <a name="portal"></a>[Portal](#tab/azure-portal)

Na página instância gerenciada, selecione o link do **tipo de preço** posicionado na seção Configurações

![alterar hardware de instância gerenciada](media/sql-database-service-tiers-vcore/change-managed-instance-hardware.png)

Na página **tipo de preço** , você poderá alterar a geração de hardware conforme descrito nas etapas anteriores.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Use o seguinte script do PowerShell:

```powershell-interactive
Set-AzSqlInstance -Name "managedinstance1" -ResourceGroupName "ResourceGroup01" -ComputeGeneration Gen5
```

Para obter mais detalhes, verifique o comando [set-AzSqlInstance](https://docs.microsoft.com/powershell/module/az.sql/set-azsqlinstance) .

# <a name="azure-cli"></a>[CLI do Azure](#tab/azure-cli)

Use o seguinte comando da CLI:

```azurecli-interactive
az sql mi update -g mygroup -n myinstance --family Gen5
```

Para obter mais detalhes, consulte [AZ SQL Mi Update](https://docs.microsoft.com/cli/azure/sql/mi#az-sql-mi-update) Command.

---

### <a name="hardware-availability"></a>Disponibilidade de hardware

#### <a name="gen4gen5-1"></a>Gen4/Gen5

O hardware Gen4 está [sendo](https://azure.microsoft.com/updates/gen-4-hardware-on-azure-sql-database-approaching-end-of-life-in-2020/) descontinuado e não está mais disponível para as novas implantações. Todos os novos bancos de dados devem ser implantados em hardware Gen5.

O Gen5 está disponível na maioria das regiões em todo o mundo.

#### <a name="fsv2-series"></a>Série Fsv2

A Fsv2-Series está disponível nas seguintes regiões: Austrália Central, Austrália Central 2, leste da Austrália, sudeste da Austrália, sul do Brasil, central do Canadá, Ásia Oriental, leste dos EUA, França central, Índia central, Índia ocidental, Coreia central, sul da Coreia, norte Europa, norte da África do Sul, Sudeste Asiático, Sul do Reino Unido, Oeste do Reino Unido, Europa Ocidental, oeste dos EUA 2.


#### <a name="m-series"></a>Série M

A série M está disponível nas seguintes regiões: leste dos EUA, Europa Setentrional, Europa Ocidental, oeste dos EUA 2.
A série M também pode ter disponibilidade limitada em regiões adicionais. Você pode solicitar uma região diferente da listada aqui, mas o preenchimento em uma região diferente pode não ser possível.

Para habilitar a disponibilidade da série M em uma assinatura, o acesso deve ser solicitado por meio do [arquivamento de uma nova solicitação de suporte](#create-a-support-request-to-enable-m-series).


##### <a name="create-a-support-request-to-enable-m-series"></a>Crie uma solicitação de suporte para habilitar a série M: 

1. Selecione **ajuda + suporte** no Portal.
2. Selecione **Nova solicitação de suporte**.

Na página **noções básicas** , forneça o seguinte:

1. Para **tipo de problema**, selecione **Limites de serviço e assinatura (cotas)** .
2. Para **assinatura** = selecione a assinatura para habilitar a série M.
3. Para **tipo de cota**, selecione **banco de dados SQL**.
4. Selecione **Avançar** para ir para a página de **detalhes** .

Na página **detalhes** , forneça o seguinte:

1. Na seção **detalhes do problema** , selecione o link **fornecer detalhes** . 
2. Para **tipo de cota do banco de dados SQL** **, selecione série M**.
3. Para **região**, selecione a região para habilitar a série M.
    Para regiões em que a série M está disponível, consulte [disponibilidade da série m](#m-series).

Solicitações de suporte aprovadas normalmente são atendidas dentro de 5 dias úteis.


## <a name="next-steps"></a>Próximas etapas

- Para criar um banco de dados SQL, consulte [criando um banco de dados SQL usando o portal do Azure](sql-database-single-database-get-started.md).
- Para obter os tamanhos de computação específicos e as opções de tamanho de armazenamento disponíveis para bancos de dados individuais, consulte [limites de recursos baseados em vCore do banco de dados SQL para bancos de dados individuais](sql-database-vcore-resource-limits-single-databases.md).
- Para obter os tamanhos de computação específicos e as opções de tamanho de armazenamento disponíveis para pools elásticos, consulte [limites de recursos baseados em vCore do banco de dados SQL para pools elásticos](sql-database-vcore-resource-limits-elastic-pools.md)
- Para obter detalhes de preços, consulte a [página de preços do banco de dados SQL do Azure](https://azure.microsoft.com/pricing/details/sql-database/single/).

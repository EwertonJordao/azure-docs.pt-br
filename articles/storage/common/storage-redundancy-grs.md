---
title: Armazenamento com redundância geográfica (GRS) para durabilidade entre regiões
titleSuffix: Azure Storage
description: O Armazenamento com redundância geográfica (GRS) replica seus dados entre duas regiões que estão a centenas de milhas de distância. O GRS protege contra falhas de hardware no datacenter, além de desastres regionais.
services: storage
author: tamram
ms.service: storage
ms.topic: conceptual
ms.date: 01/02/2020
ms.author: tamram
ms.reviewer: artek
ms.subservice: common
ms.openlocfilehash: 6bb93c3fb6599a05978e11ef5fbc179ccfaa9ec2
ms.sourcegitcommit: 003e73f8eea1e3e9df248d55c65348779c79b1d6
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/02/2020
ms.locfileid: "75614894"
---
# <a name="geo-redundant-storage-grs-cross-regional-replication-for-azure-storage"></a>GRS (armazenamento com redundância geográfica): replicação inter-regional para Armazenamento do Microsoft Azure

[!INCLUDE [storage-common-redundancy-GRS](../../../includes/storage-common-redundancy-grs.md)]

## <a name="read-access-geo-redundant-storage"></a>Armazenamento com redundância geográfica com acesso de leitura

O RA-GRS (armazenamento com redundância geográfica com acesso de leitura) maximiza a disponibilidade para sua conta de armazenamento. RA-GRS fornece acesso somente leitura aos dados no local secundário, além de replicação geográfica em duas regiões.

Quando você habilita o acesso somente leitura aos dados na região secundária, os dados ficam disponíveis em um ponto de extremidade secundário, bem como no ponto de extremidade primário de sua conta de armazenamento. O ponto de extremidade secundário é semelhante ao ponto de extremidade primário, mas anexa o sufixo `–secondary` ao nome da conta. Por exemplo, se o ponto de extremidade primário para o serviço Blob for `myaccount.blob.core.windows.net`, seu ponto de extremidade secundário será `myaccount-secondary.blob.core.windows.net`. As chaves de acesso para sua conta de armazenamento são as mesmas para os pontos de extremidade primário e secundário.

Algumas considerações a ter em mente ao usar o RA-GRS:

- Seu aplicativo precisa gerenciar com qual ponto de extremidade está interagindo ao usar o RA-GRS.
- Como a replicação assíncrona envolve um atraso, as alterações que ainda não foram replicadas para a região secundária poderão ser pedidas se os dados não puderem ser recuperados na região primária.
- Você pode verificar a propriedade **hora da última sincronização** da sua conta de armazenamento. A **hora da última sincronização** é um valor de data/hora GMT. Todas as gravações primárias feitas antes da **hora da última sincronização** foram gravadas com êxito no local secundário, o que significa que elas estão disponíveis para serem lidas a partir do local secundário. As gravações primárias após a **hora da última sincronização** podem ou não estar disponíveis para leituras ainda. Você pode consultar esse valor usando o PowerShell, CLI do Azure ou uma das bibliotecas de cliente de armazenamento do Azure. Para obter mais informações, consulte **obtendo o horário da última sincronização** na [criação de aplicativos altamente disponíveis usando o armazenamento com redundância geográfica com acesso de leitura](storage-designing-ha-apps-with-ragrs.md#getting-the-last-sync-time).
- Se você iniciar um failover de conta (versão prévia) de uma conta GRS ou RA-GRS para a região secundária, o acesso de gravação a essa conta é restaurado após o failover ser concluído. Para obter mais informações, confira [Recuperação de desastre e failover de conta de armazenamento (versão prévia)](storage-disaster-recovery-guidance.md).
- O RA-GRS é para fins de alta disponibilidade. Para obter diretrizes de escalabilidade, examine a [lista de verificação de desempenho](storage-performance-checklist.md).
- Para sugestões sobre como projetar para alta disponibilidade com o RA-GRS, consulte [Projetando Aplicativos Altamente Disponíveis usando o armazenamento RA-GRS](storage-designing-ha-apps-with-ragrs.md).

## <a name="what-is-the-rpo-and-rto-with-grs"></a>O que é RPO e RTO com GRS?

**Objetivo do ponto de recuperação (RPO):** Em GRS e RA-GRS, o serviço de armazenamento replica de forma assíncrona os dados do local primário para o secundário. No caso de a região primária ficar indisponível, você pode executar um failover de conta (versão prévia) para a região secundária. Quando você inicia um failover, as alterações recentes que ainda não foram replicadas geograficamente poderão ser perdidas. O número de minutos de dados possíveis perdidos é conhecido como o RPO. O RPO indica o ponto no tempo em que os dados podem ser recuperados. Normalmente, o Armazenamento do Microsoft Azure tem um RPO inferior a 15 minutos, embora atualmente não exista nenhum SLA sobre quanto tempo leva a replicação geográfica.

**RTO (Objetivo de Tempo de Recuperação):** o RTO é uma medida de quanto tempo é necessário para fazer o failover e colocar a conta de armazenamento novamente online. O tempo para fazer o failover inclui as seguintes ações:

- O tempo até que o cliente inicie o failover da conta de armazenamento da região primária para a região secundária.
- O tempo exigido pelo Azure para fazer o failover, alterando as entradas DNS primárias para que elas apontem para o local secundário.

## <a name="paired-regions"></a>Regiões emparelhadas

Quando você cria uma conta de armazenamento, pode selecionar a região primária para a conta. A região secundária emparelhada é determinada com base na região primária e não pode ser alterada. Para obter informações atualizadas sobre regiões suportadas pelo Azure, consulte [Continuidade de negócios e recuperação de desastre (BCDR): regiões emparelhadas do Azure](../../best-practices-availability-paired-regions.md).

## <a name="see-also"></a>Consulte também

- [Replicação de Armazenamento do Azure](storage-redundancy.md)
- [Armazenamento redundante local (LRS): Redundância de dados de baixo custo para o Armazenamento do Azure](storage-redundancy-lrs.md)
- [ZRS (armazenamento com redundância de zona): aplicativos de Armazenamento do Microsoft Azure altamente disponíveis](storage-redundancy-zrs.md)

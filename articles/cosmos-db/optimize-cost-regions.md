---
title: Otimizar o custo para implantações de várias regiões no Azure Cosmos DB
description: Este artigo explica como gerenciar os custos de implantações de várias regiões no Azure Cosmos DB.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 07/31/2019
ms.openlocfilehash: 8d98c9a7e58f08d9ad63183805cd6cd0d2ab3b3d
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91570172"
---
# <a name="optimize-multi-region-cost-in-azure-cosmos-db"></a>Otimizar o custo de várias regiões no Azure Cosmos DB

Você pode adicionar ou remover regiões da sua conta do Azure Cosmos a qualquer momento. A taxa de transferência que você configura para vários bancos de dados e contêineres do Azure Cosmos é reservada em cada região associada à sua conta. Se a taxa de transferência provisionada por hora, que é a soma de RU/s configuradas em todos os bancos de dados e contêineres para sua conta do Azure Cosmos for `T` e o número de regiões do Azure associadas à sua conta de banco de dados for `N`, então, o total de taxa de transferência provisionada para sua conta do Cosmos, para uma determinada hora é igual a:

1. `T x N RU/s` se a sua conta do Azure Cosmos estiver configurada com uma única região de gravação. 

1. `T x (N+1) RU/s` se sua conta do Azure Cosmos for configurada com todas as regiões capazes de processar gravações. 

A taxa de transferência provisionada com região de gravação única custa US$ 0,008 por hora por 100 RU/s e a taxa de transferência provisionada com várias regiões graváveis custa US$ 0,016/hora por 100 RU/s. Para saber mais, confira a [página de Preços](https://azure.microsoft.com/pricing/details/cosmos-db/) do Microsoft Azure Cosmos DB.

## <a name="costs-for-multiple-write-regions"></a>Custos de várias regiões de gravação

Em um sistema de gravações de várias regiões, o RUs de rede disponível para operações de gravação aumenta `N` os tempos em que `N` é o número de regiões de gravação. Ao contrário de gravações de região única, cada região agora é gravável e deve dar suporte à resolução de conflitos. A quantidade de carga de trabalho para gravadores aumentou. Do ponto de vista do planejamento de custos, para executar `M` ru/s de gravações em todo o mundo, você precisará provisionar M `RUs` em um nível de contêiner ou banco de dados. Em seguida, você pode adicionar quantas regiões desejar e usá-las para gravações para executar `M` RU de gravações em todo o mundo. 

### <a name="example"></a>Exemplo

Considere que você tenha um contêiner no Oeste dos EUA provisionado com uma taxa de transferência de 10.000 RU/s e tenha armazenado 1 TB de dados este mês. Suponha que você adicionou três regiões (Leste dos EUA, Norte da Europa e Leste da Ásia), cada uma com o mesmo armazenamento e taxa de transferência, e quer a capacidade de gravar nos contêineres nas quatro regiões a partir do seu aplicativo globalmente distribuído. O valor total da sua fatura mensal (considerando 31 dias) em um mês é o seguinte:

|**Item**|**Uso (mensalmente)**|**Tarifa**|**Custo mensal**|
|----|----|----|----|
|Cobrança da taxa de transferência para o contêiner no Oeste dos EUA (várias regiões de gravação) |10 mil RU/s * 24 * 31 |US$ 0,016 por 100 RU/s por hora |US$ 1.190,40 |
|Cobrança da taxa de transferência para três regiões adicionais: Leste dos EUA, Norte da Europa e Leste da Ásia (várias regiões de gravação) |(3 + 1) * 10 mil RU/s * 24 * 31 |US$ 0,016 por 100 RU/s por hora |US$ 4.761,60 |
|Cobrança de armazenamento para o contêiner no Oeste dos EUA |1 TB (ou 1.024 GB) |US$ 0,25/GB |$256 |
|Cobrança de armazenamento para três regiões adicionais – Leste dos EUA, Norte da Europa e Leste da Ásia |3 * 1 TB (ou 3.072 GB) |US$ 0,25/GB |$768 |
|**Total**|||**$6976** |

## <a name="improve-throughput-utilization-on-a-per-region-basis"></a>Melhorar a utilização de taxa de transferência por região

Se você tiver uma utilização ineficaz, por exemplo, uma ou mais regiões subutilizadas ou superutilizadas, você pode usar as seguintes etapas para melhorar a utilização de taxa de transferência:  

1. Certifique-se de otimizar a taxa de transferência provisionada (RUs) na região de gravação primeiro e, em seguida, fazer o uso máximo das RUs em regiões de leitura usando o feed de alterações da região de leitura, etc. 

2. Leituras e gravações de várias regiões podem ser dimensionadas em todas as regiões associadas à conta do Azure Cosmos. 

3. Monitore a atividade em suas regiões e você pode adicionar e remover regiões sob demanda para dimensionar sua taxa de transferência de leitura e gravação.

## <a name="next-steps"></a>Próximas etapas

A seguir, você poderá saber mais sobre a otimização de custos no Azure Cosmos DB nos seguintes artigos:

* Saiba mais sobre [Otimizando para desenvolvimento e teste](optimize-dev-test.md)
* Saiba mais sobre [Entender sua cobrança do Azure Cosmos DB](understand-your-bill.md)
* Saiba mais sobre [Otimizando o custo da taxa de transferência](optimize-cost-throughput.md)
* Saiba mais sobre [Otimizando o custo de armazenamento](optimize-cost-storage.md)
* Saiba mais sobre [Otimizando o custo de leituras e gravações](optimize-cost-reads-writes.md)
* Saiba mais sobre [Otimizando o custo de consultas](optimize-cost-queries.md)


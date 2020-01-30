---
title: Dimensionar recursos
description: Este artigo explica como dimensionar o banco de dados, adicionando ou removendo os recursos alocados.
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: jrasnik, carlrab
ms.date: 06/25/2019
ms.openlocfilehash: c4366b2718271b1e27325e6946c5016e9230cea4
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/29/2020
ms.locfileid: "76835905"
---
# <a name="dynamically-scale-database-resources-with-minimal-downtime"></a>Dimensionar dinamicamente os recursos de banco de dados com o tempo de inatividade mínimo

O banco de dados SQL do Azure permite que você adicione dinamicamente mais recursos ao seu banco de dados com [tempo de inatividade](https://azure.microsoft.com/support/legal/sla/sql-database/v1_2/)mínimo; no entanto, há uma mudança no período em que a conectividade é perdida no banco de dados por um curto período de tempo, o que pode ser mitigado usando a lógica de repetição.

## <a name="overview"></a>Visão Geral

Quando a demanda de seu aplicativo aumenta de alguns dispositivos e clientes para milhões, o Banco de Dados SQL do Azure é dimensionado de maneira dinâmica e com tempo de inatividade mínimo. A escalabilidade é uma das características mais importantes do PaaS que possibilita adicionar dinamicamente mais recursos ao serviço, quando necessário. O Banco de Dados SQL do Azure permite que você altere com facilidade os recursos (potência da CPU, memória, taxa de transferência de E/S e armazenamento) alocados a seus bancos de dados.

Você pode atenuar problemas de desempenho devido ao aumento no uso do aplicativo que não pode ser corrigido usando métodos de indexação ou de reescrita de consulta. A adição de mais recursos permite que você responda rapidamente quando seu banco de dados atingir os limites de recursos atuais e precisar de mais capacidade para manipular a carga de trabalho de entrada. O Banco de Dados SQL do Azure também possibilita que você reduza verticalmente os recursos quando eles não forem necessários para reduzir o custo.

Você não precisa se preocupar com a compra do hardware e a alteração da infraestrutura subjacente. O dimensionamento do banco de dados pode ser feito com facilidade por meio do portal do Azure usando um controle deslizante.

![Dimensionar o desempenho do banco de dados](media/sql-database-scalability/scale-performance.svg)

O Banco de Dados SQL do Azure oferece o [modelo de compra baseado em DTU](sql-database-service-tiers-dtu.md) e o [modelo de compra baseado em vCore](sql-database-service-tiers-vcore.md).

- O [modelo de compra baseado em DTU](sql-database-service-tiers-dtu.md) oferece uma mistura de computação, memória e recursos de E/S em três camadas de serviço para dar suporte a cargas de trabalho leves e pesadas de banco de dados: Básico, Standard e Premium. Níveis de desempenho dentro de cada camada fornecem uma mistura diferente desses recursos, aos quais você pode adicionar recursos de armazenamento.
- O [modelo de compra baseado em vCore](sql-database-service-tiers-vcore.md) permite que você escolha o número de vCores, a quantidade ou memória e a quantidade e velocidade de armazenamento. Esse modelo de compra oferece três camadas de serviço: Uso Geral, Comercialmente Crítico e hiperescala.

Você pode criar seu primeiro aplicativo em um banco de dados individual pequeno com um baixo custo mensal na camada de serviço de Básica, Geral ou de Uso Geral e, depois, alterar a camada de serviço manualmente ou de forma programática a qualquer momento para a camada de serviço Premium ou Comercialmente Crítico para atender às necessidades da solução. Você pode ajustar o desempenho sem tempo de inatividade para seu aplicativo ou para seus clientes. A escalabilidade dinâmica permite que o banco de dados responda de forma transparente às mudanças rápidas de requisitos de recursos e que você pague apenas pelos recursos de que precisa, quando precisar deles.

> [!NOTE]
> A escalabilidade dinâmica é diferente do dimensionamento automático. O dimensionamento automático é quando um serviço é dimensionado automaticamente com base em critérios, enquanto a escalabilidade dinâmica permite o dimensionamento manual com um tempo de inatividade mínimo.

O Banco de Dados SQL do Azure Individual oferece suporte à escalabilidade dinâmica manual, mas não ao dimensionamento automático. Para uma experiência mais *automática*, considere o uso de pools elásticos, que permitem que os bancos de dados compartilhem recursos em um pool com base nas necessidades individuais do banco de dados.
No entanto, há scripts que podem ajudar a automatizar a escalabilidade de um Banco de Dados SQL do Azure individual. Para ver um exemplo, consulte [Usar o PowerShell para monitorar e dimensionar um Banco de Dados SQL individual](scripts/sql-database-monitor-and-scale-database-powershell.md).

Você pode alterar as [camadas de serviço de DTU](sql-database-service-tiers-dtu.md) ou as [características de vCore](sql-database-vcore-resource-limits-single-databases.md) a qualquer momento com tempo de inatividade mínimo para seu aplicativo (tempo médio de quatro segundos). Para muitos negócios e aplicativos, ser capaz de criar bancos de dados e ajustar o desempenho sob demanda é o suficiente, especialmente se os padrões de uso forem relativamente previsíveis. Mas se você tiver os padrões de uso imprevisíveis, pode ser difícil de gerenciar os custos e o seu modelo de negócios. Para este cenário, você pode usar um pool elástico com um determinado número de eDTUs que são compartilhados entre vários bancos de dados no pool.

![Introdução ao Banco de Dados SQL: DTUs de banco de dados individual por camada e por nível](./media/sql-database-what-is-a-dtu/single_db_dtus.png)

Todos os três tipos de Bancos de Dados SQL do Azure oferecem alguma capacidade de dimensionar dinamicamente os bancos de dados:

- Com um [banco de dados individual](sql-database-single-database-scale.md), você pode usar modelos de [DTU](sql-database-dtu-resource-limits-single-databases.md) ou [vCore](sql-database-vcore-resource-limits-single-databases.md) para definir a quantidade máxima de recursos que serão atribuídos a cada banco de dados.
- Uma [Instância Gerenciada](sql-database-managed-instance.md) usa o modo [vCores](sql-database-managed-instance.md#vcore-based-purchasing-model) e permite que você defina o máximo de núcleos de CPU e o máximo de armazenamento alocado para sua instância. Todos os bancos de dados na instância compartilharão os recursos alocados para a instância.
- Os [pools elásticos](sql-database-elastic-pool-scale.md) permitem que você defina o limite máximo de recursos por grupo de bancos de dados no pool.

Iniciar a ação escalar verticalmente ou reduzir horizontalmente em qualquer um dos tipos reiniciaria o processo do mecanismo de banco de dados e o moveria para uma máquina virtual diferente, se necessário. Mover o processo do mecanismo de banco de dados para uma nova máquina virtual é um **processo online** no qual você pode continuar usando o serviço de banco de dados SQL do Azure existente enquanto o processo está em andamento. Depois que o mecanismo de banco de dados de destino estiver totalmente inicializado e pronto para processar as consultas, as conexões serão [alternadas da origem para o mecanismo de banco de dados de destino](sql-database-single-database-scale.md#impact). 


> [!NOTE]
> Você pode esperar uma pequena interrupção de conexão quando o processo de escalar/reduzir verticalmente for concluído. Se você tiver implementado a [lógica de repetição para erros transitórios padrão](sql-database-connectivity-issues.md#retry-logic-for-transient-errors), não perceberá o failover.

## <a name="alternative-scale-methods"></a>Métodos alternativos de escala

O dimensionamento de recursos é a maneira mais fácil e mais eficiente de melhorar o desempenho do banco de dados sem alterar o código do banco de dados ou do aplicativo. Em alguns casos, até mesmo as mais altas camadas de serviço, tamanhos de computação e otimizações de desempenho podem não lidar com a carga de trabalho de uma maneira bem-sucedida e econômica. Nesse caso, você tem essas opções adicionais para dimensionar seu banco de dados:

- A [expansão de leitura](sql-database-read-scale-out.md) é um recurso disponível no qual você está obtendo uma réplica somente leitura de seus dados, em que você pode executar consultas somente leitura, como relatórios. A réplica somente leitura manipulará a carga de trabalho somente leitura sem afetar o uso de recursos no banco de dados primário.
- A [fragmentação de banco de dados](sql-database-elastic-scale-introduction.md) é um conjunto de técnicas que permite dividir os dados em vários bancos de dados e dimensioná-los de forma independente.

## <a name="next-steps"></a>Próximos passos

- Para obter informações sobre como melhorar o desempenho do banco de dados alterando o código do banco de dados, confira [Encontrar e aplicar recomendações de desempenho](sql-database-advisor-portal.md).
- Para obter informações sobre como deixar a inteligência interna do banco de dados otimizar o banco de dados, confira [Ajuste automático](sql-database-automatic-tuning.md).
- Para obter informações sobre a Escala de Leitura no serviço Banco de Dados SQL do Azure, confira como [usar réplicas somente leitura para balancear a carga de cargas de trabalho de consulta somente leitura](sql-database-read-scale-out.md).
- Para obter informações sobre a Fragmentação de um banco de dados, confira [Escalando horizontalmente com o Banco de Dados SQL do Azure](sql-database-elastic-scale-introduction.md).

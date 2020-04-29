---
title: Replicação de dados do Azure para MySQL
description: Saiba mais sobre como usar a replicação de dados para sincronizar de um servidor externo no banco de dados do Azure para o serviço MySQL.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 3/18/2020
ms.openlocfilehash: 20be34191355e6ade40e0f3b218818bfa5345a28
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79533225"
---
# <a name="replicate-data-into-azure-database-for-mysql"></a>Replicar dados no Banco de Dados do Azure para MySQL

A Replicação de Dados permite sincronizar os dados de um servidor MySQL externo no serviço Banco de Dados do Azure para MySQL. O servidor externo pode ser local, em máquinas virtuais ou um serviço de banco de dados hospedado por outros provedores de nuvem. A Replicação nos dados se baseia na replicação nativa baseada na posição do arquivo de log binário (binlog) para o MySQL. Para saber mais sobre a replicação do binlog, confira a [visão geral da replicação do binlog do MySQL](https://dev.mysql.com/doc/refman/5.7/en/binlog-replication-configuration-overview.html). 

## <a name="when-to-use-data-in-replication"></a>Quando usar a replicação nos dados
Os cenários principais nos quais considerar o uso da replicação nos dados são:

- **Sincronização de dados híbrida:** com a replicação nos dados, você pode manter os dados sincronizados entre os servidores locais e o Banco de Dados do Azure para MySQL. Essa sincronização é útil para criar aplicativos híbridos. Esse método é atraente quando você tem um servidor de banco de dados local existente, mas deseja mover os dados para uma região mais próxima aos usuários finais.
- **Sincronização de várias nuvens:** para soluções de nuvem complexas, use a replicação nos dados para sincronizar dados entre o Banco de Dados do Azure para MySQL e provedores de nuvem diferentes, incluindo máquinas virtuais e serviços de banco de dados nessas nuvens.
 
Para cenários de migração, use o [serviço de migração de banco de dados do Azure](https://azure.microsoft.com/services/database-migration/)(DMS).

## <a name="limitations-and-considerations"></a>Limitações e considerações

### <a name="data-not-replicated"></a>Dados não replicados
O [*banco de dados de sistema de mysql*](https://dev.mysql.com/doc/refman/5.7/en/system-schema.html) no servidor mestre não é replicado. Alterações nas contas e permissões no servidor mestre não são replicadas. Se você criar uma conta no servidor mestre e essa conta precisar acessar o servidor de réplica, crie manualmente a mesma conta no lado do servidor de réplica. Para entender quais tabelas estão contidas no banco de dados do sistema, confira o [Manual do MySQL](https://dev.mysql.com/doc/refman/5.7/en/system-schema.html).

### <a name="requirements"></a>Requisitos
- A versão do servidor mestre deve ser pelo menos a versão 5.6 do MySQL. 
- As versões do servidor principal e de réplica devem ser as mesmas. Por exemplo, ambos devem ser MySQL versão 5.6 ou ambos devem ser MySQL versão 5.7.
- Cada tabela deve ter uma chave primária.
- O servidor mestre deve usar o mecanismo MySQL InnoDB.
- O usuário deve ter permissões para configurar o log binário e criar novos usuários no servidor mestre.
- Se o servidor mestre tiver o SSL habilitado, verifique se o certificado de autoridade de certificação SSL fornecido para o domínio `mysql.az_replication_change_master` foi incluído no procedimento armazenado. Consulte os [exemplos](https://docs.microsoft.com/azure/mysql/howto-data-in-replication#link-master-and-replica-servers-to-start-data-in-replication) a seguir e o `master_ssl_ca` parâmetro.
- Verifique se o endereço IP do servidor mestre foi adicionado às regras de firewall do servidor de réplica do Banco de Dados do Azure para MySQL. Atualizar regras de firewall usando o [Portal do Azure](https://docs.microsoft.com/azure/mysql/howto-manage-firewall-using-portal) ou a [CLI do Azure](https://docs.microsoft.com/azure/mysql/howto-manage-firewall-using-cli).
- Assegure-se de que o computador que hospeda o servidor mestre permita tráfego de entrada e saída na porta 3306.
- Verifique se o servidor mestre tem um **endereço IP público**, se o DNS está acessível publicamente ou se tem um FQDN (nome de domínio totalmente qualificado).

### <a name="other"></a>Outros
- A replicação de dados têm suporte apenas em tipos de preços de Uso Geral e Otimizados para Memória.
- O GTID (identificadores de transação globais) não são compatíveis.

## <a name="next-steps"></a>Próximas etapas
- Saiba como [Configurar a replicação de dados em](howto-data-in-replication.md)
- Saiba mais sobre [replicar no Azure com réplicas de leitura](concepts-read-replicas.md)
- Saiba mais sobre como [migrar dados com tempo de inatividade mínimo usando DMS](howto-migrate-online.md)
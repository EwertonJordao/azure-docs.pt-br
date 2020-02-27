---
title: Backup e restauração-banco de dados do Azure para MySQL
description: Saiba mais sobre backups automáticos e restauração do Banco de Dados do Azure para servidor MySQL.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 12/02/2019
ms.openlocfilehash: 47fa4083c26f18149b0b69b05f2cfd0b227de868
ms.sourcegitcommit: 5a71ec1a28da2d6ede03b3128126e0531ce4387d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/26/2020
ms.locfileid: "77619581"
---
# <a name="backup-and-restore-in-azure-database-for-mysql"></a>Backup e restauração no Banco de Dados do Azure para MySQL

O Banco de Dados do Azure para MySQL cria backups de servidor automaticamente e os armazena no armazenamento com redundância geográfica ou local configurado pelo usuário. Os backups podem ser usados para restaurar o servidor pontualmente. Os recursos de backup e restauração são uma parte essencial de qualquer estratégia de continuidade dos negócios, pois eles protegem seus dados contra exclusão ou corrupção acidentais.

## <a name="backups"></a>Backups

O banco de dados do Azure para MySQL faz backups dos arquivos de data e do log de transações. Dependendo do tamanho máximo de armazenamento com suporte, pegamos backups completos e diferenciais (servidores de armazenamento máximo de 4 TB) ou backups de instantâneo (até 16 TB de servidores de armazenamento máximo). Esses backups permitem que você restaure um servidor pontualmente dentro de seu período de retenção de backup configurado. O período de retenção de backup padrão é de sete dias. Opcionalmente, você pode [configurá-lo](howto-restore-server-portal.md#set-backup-configuration) até 35 dias. Todos os backups são criptografados usando a criptografia AES de 256 bits.

Esses arquivos de backup não podem ser exportados. Os backups só podem ser usados para operações de restauração no banco de dados do Azure para MySQL. Você pode usar [mysqldump](concepts-migrate-dump-restore.md) para copiar um banco de dados.

### <a name="backup-frequency"></a>Frequência de backup

Geralmente, os backups completos ocorrem semanalmente, os backups diferenciais ocorrem duas vezes por dia para servidores com um armazenamento máximo com suporte de 4 TB. Os backups de instantâneo acontecem pelo menos uma vez por dia no caso de servidores compatíveis com até 16 TB de armazenamento. Os backups de log de transações em ambos os casos ocorrem a cada cinco minutos. O primeiro instantâneo do backup completo é agendado imediatamente após a criação de um servidor. O backup completo inicial pode levar mais tempo em um servidor restaurado grande. O ponto mais anterior para o qual um novo servidor pode ser restaurado é o momento da conclusão do backup completo inicial. Como os instantâneos são instantanious, os servidores com suporte para até 16 TB de armazenamento podem ser restaurados até o momento da criação.

### <a name="backup-redundancy-options"></a>Opções de redundância de backup

O Banco de Dados do Azure para MySQL fornece a flexibilidade de escolher entre o armazenamento de backup com redundância local ou com redundância geográfica nas camadas de Uso Geral e Otimizado para Memória. Quando os backups são armazenados no armazenamento de backup com redundância geográfica, eles não são somente armazenados dentro da região em que o servidor está hospedado, mas também replicados em um [datacenter emparelhado](https://docs.microsoft.com/azure/best-practices-availability-paired-regions). Isso fornece maior proteção e capacidade de restaurar o servidor em uma região diferente em caso de desastre. A camada Básica oferece apenas o armazenamento de backup de redundância local.

> [!IMPORTANT]
> A configuração do armazenamento com redundância local ou geográfica para backup só é permitida durante a criação do servidor. Quando o servidor é provisionado, você não pode alterar a opção de redundância do armazenamento de backup.

### <a name="backup-storage-cost"></a>Custo do armazenamento de backup

O Banco de Dados do Azure para MySQL fornece até 100% de seu armazenamento de servidor configurado como armazenamento de backup sem custo adicional. Normalmente, isso é adequado para uma retenção de backup de sete dias. O armazenamento de backup adicional que for utilizado será cobrado em GB-mês.

Por exemplo, se você tiver provisionado um servidor com 250 GB, terá 250 GB de armazenamento de backup sem custo adicional. O armazenamento acima dos 250 GB é cobrado.

## <a name="restore"></a>Restaurar

No Banco de Dados do Azure para MySQL, a execução de uma restauração cria um novo servidor de backup do servidor original.

Há dois tipos de restauração disponíveis:

- A **Restauração pontual** está disponível em qualquer opção de redundância de backup e cria um novo servidor na mesma região do servidor original.
- A **Restauração geográfica** está disponível somente se você configurou seu servidor para armazenamento com redundância geográfica; ele permite que você restaure o servidor em uma região diferente.

O tempo estimado de recuperação dependerá de vários fatores, incluindo os tamanhos dos bancos de dados, o tamanho do log de transações, a largura de banda de rede e o número total de bancos de dados de recuperação na mesma região e ao mesmo tempo. Normalmente, o tempo de recuperação é menor do que 12 horas.

> [!IMPORTANT]
> Excluir servidores **não é possível** ser restaurado. Se você excluir o servidor, todos os bancos de dados que pertencem ao servidor também serão excluídos e não poderão ser recuperados. Para proteger recursos do servidor, após a implantação, da exclusão acidental ou de alterações inesperadas, os administradores podem usar [bloqueios de gerenciamento](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-lock-resources).

### <a name="point-in-time-restore"></a>Restauração em um momento determinado

Independentemente de sua opção de redundância de backup, você pode executar uma restauração para qualquer ponto anterior dentro de seu período de retenção de backup. Um novo servidor é criado na mesma região do Azure do servidor original. Ele é criado com a configuração do servidor original para o tipo de preço, a geração de computação, o número de núcleos virtuais, o tamanho do armazenamento, o período de retenção de backup e a opção de redundância de backup.

A Restauração pontual é útil em vários cenários. Por exemplo, quando um usuário exclui dados acidentalmente, descarta uma tabela ou um banco de dados importante, ou se um aplicativo acidentalmente substitui dados bons por dados inválidos devido a um defeito no aplicativo.

Talvez seja necessário aguardar a execução do próximo backup de log de transações antes de poder restaurar para um ponto anterior nos últimos cinco minutos.

### <a name="geo-restore"></a>Restauração geográfica

É possível restaurar um servidor para outra região do Azure onde o serviço está disponível caso você tenha configurado o servidor para backups com redundância geográfica. Os servidores que dão suporte a até 4 TB de armazenamento podem ser restaurados para a região emparelhada geograficamente ou para qualquer região que ofereça suporte a até 16 TB de armazenamento. Para servidores que dão suporte a até 16 TB de armazenamento, os backups geográficos podem ser restaurados em qualquer região que dê suporte a servidores de 16 TB também. Examine os [tipos de preço do banco de dados do Azure para MySQL](concepts-pricing-tiers.md) para a lista de regiões com suporte.

A restauração geográfica é a opção de recuperação padrão quando o servidor não está disponível devido a um incidente na região em que ele está hospedado. Se um incidente de grande escala em uma região resultar na indisponibilidade do seu aplicativo de banco de dados, você poderá restaurar um servidor do backup com redundância geográfica para um servidor em qualquer outra região. Há um atraso entre quando um backup é feito e quando ele é replicado em uma região diferente. Esse atraso pode ser de até uma hora, então, em caso de desastre pode haver perda de dados de até uma hora.

Durante a restauração geográfica, as configurações de servidor que podem ser alteradas incluem as opções de geração de computação, vCore, período de retenção de backup e redundância de backup. Não há suporte para a alteração do tipo de preço (Básico, Uso Geral ou Otimizado para Memória) ou do tamanho de armazenamento durante a restauração geográfica.

### <a name="perform-post-restore-tasks"></a>Executar tarefas de pós-restauração

Após uma restauração de um dos mecanismos de recuperação, você deve executar as seguintes tarefas para colocar os usuários e os aplicativos novamente em execução:

- Se o novo servidor é usado para substituir o servidor original, redirecione clientes e aplicativos de cliente para o novo servidor
- Verifique se as regras de firewall adequadas no nível do servidor estão em vigor para que os usuários se conectem
- Verifique se as permissões e os logons adequados no nível do banco de dados estão em vigor
- Configurar os alertas, conforme apropriado

## <a name="next-steps"></a>Próximas etapas

- Para saber mais sobre continuidade dos negócios, confira a  [visão geral de continuidade dos negócios](concepts-business-continuity.md).
- Para restaurar para um determinado ponto no tempo usando o Portal do Azure, confira  [Restaurar um banco de dados para um ponto no tempo usando o Portal do Azure](howto-restore-server-portal.md).
- Para restaurar para um ponto no tempo usando a CLI do Azure, confira [Restaurar um banco de dados para um ponto no tempo usando a CLI](howto-restore-server-cli.md).

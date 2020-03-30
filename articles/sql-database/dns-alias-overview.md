---
title: Alias DNS
description: Seus aplicativos podem se conectar a um alias do nome do servidor do Banco de Dados SQL do Azure. Ao mesmo tempo, você pode alterar o Banco de Dados SQL para o qual o alias aponta a qualquer momento no intuito de facilitar testes e assim por diante.
services: sql-database
ms.service: sql-database
ms.subservice: operations
ms.custom: seo-lt-2019
ms.devlang: ''
ms.topic: conceptual
author: rohitnayakmsft
ms.author: rohitna
ms.reviewer: genemi, jrasnick, vanto
ms.date: 06/26/2019
ms.openlocfilehash: 05fa542a0ad1c72f73148eefd304a9771798598d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73820623"
---
# <a name="dns-alias-for-azure-sql-database"></a>Alias do DNS para Banco de Dados SQL do Azure

O Banco de Dados SQL do Azure tem um servidor de DNS (Sistema de Nomes de Domínio). As APIs REST e o PowerShell aceitam [chamadas para criar e gerenciar aliases de DNS](#anchor-powershell-code-62x) do nome do servidor do Banco de Dados SQL.

Um *alias do DNS* pode ser usado no lugar do nome do servidor do Banco de Dados SQL do Azure. Os programas cliente podem usar o alias em suas cadeias de conexão. O alias do DNS oferece uma camada de conversão que pode redirecionar os programas cliente a diferentes servidores. Essa camada poupa as dificuldades de ter que localizar e editar todos os clientes e suas cadeias de conexão.

Alguns usos comuns para um alias do DNS incluem os seguintes casos:

- Criar um nome fácil de lembrar para um servidor do SQL Server do Azure.
- Durante o desenvolvimento inicial, seu alias pode se referir a um servidor de teste do Banco de Dados SQL. Quando o aplicativo for lançado, você poderá modificar o alias para se referir ao servidor de produção. A transição de teste para a produção não exige nenhuma modificação nas configurações dos vários clientes que se conectam ao servidor de banco de dados.
- Suponha que único banco de dados em seu aplicativo seja movido para outro servidor do Banco de Dados SQL. Aqui você pode modificar o alias sem a necessidade de modificar as configurações dos vários clientes.
- Durante uma paralisação regional, você usa a restauração geográfica para recuperar seu banco de dados em um servidor e região diferentes. Você pode modificar seu alias existente para apontar para o novo servidor para que o aplicativo cliente existente possa se reconectar a ele. 

## <a name="domain-name-system-dns-of-the-internet"></a>DNS (Sistema de Nomes de Domínio) da Internet

A Internet depende do DNS. O DNS converte seus nomes amigáveis no nome do servidor do Banco de Dados SQL do Azure.

## <a name="scenarios-with-one-dns-alias"></a>Cenários com um alias do DNS

Suponha que você tenha que mudar o sistema para um novo servidor do Banco de Dados SQL do Azure. No passado, você precisava localizar e atualizar cada cadeia de conexão em cada programa cliente. Mas, agora, se as cadeias de conexão usam um alias do DNS, apenas uma propriedade do alias deve ser atualizada.

O recurso alias do DNS do Banco de Dados SQL do Azure pode ajudar nas seguintes situações:

### <a name="test-to-production"></a>Teste para a produção

Quando você começar a desenvolver os programas cliente, faça com que eles usem um alias do DNS nas cadeias de conexão. Você fazer com que as propriedades do alias apontem para uma versão de teste do seu servidor do Banco de Dados SQL do Azure.

Depois, quando o novo sistema for lançado em produção, você poderá atualizar as propriedades do alias para apontarem para o servidor do Banco de Dados SQL de produção. Não haverá necessidade de alteração nos programas cliente.

### <a name="cross-region-support"></a>Suporte entre regiões

Uma recuperação de desastre pode mudar seu servidor do Banco de Dados SQL para uma região geográfica diferente. Para um sistema que estava usando um alias DNS, a necessidade de encontrar e atualizar todas as strings de conexão para todos os clientes pode ser evitada. Em vez disso, você pode atualizar um alias para se referir ao novo servidor do Banco de Dados SQL que agora hospeda o seu banco de dados.

## <a name="properties-of-a-dns-alias"></a>Propriedades de um alias do DNS

As propriedades a seguir se aplicam a cada alias de DNS do seu servidor do Banco de Dados SQL:

- *Nome exclusivo:* cada nome de alias que você cria é exclusivo em todos os servidores do Banco de Dados SQL do Azure, assim como os nomes dos servidores.
- *Servidor é necessário:* um alias de DNS não pode ser criado se não faz referência a exatamente um servidor e esse servidor já deve existir. Um alias atualizado sempre deve fazer referência a exatamente um servidor existente.
  - Quando você remove um servidor do Banco de Dados SQL, o sistema do Azure também remove todos os aliases de DNS que fazem referência ao servidor.
- *Não associado a nenhuma região:* aliases de DNS não são associados a uma região. Todos os alias de DNS podem ser atualizados para fazer referência a um servidor do Banco de Dados SQL do Azure que resida em qualquer região geográfica.
  - No entanto, ao atualizar um alias para se referir a outro servidor, ambos os servidores devem existir na mesma *assinatura* do Azure.
- *Permissões:* para gerenciar um alias de DNS, o usuário deve ter permissões de *Colaborador do Server* ou superior. Para obter mais informações, consulte [Introdução ao Controle de Acesso Baseado em Função no portal do Azure](../role-based-access-control/overview.md).

## <a name="manage-your-dns-aliases"></a>Gerenciar seus aliases de DNS

Os cmdlets do PowerShell e das APIs REST estão disponíveis para que você possa gerenciar seus aliases de DNS de forma programática.

### <a name="rest-apis-for-managing-your-dns-aliases"></a>APIs REST para gerenciar seus aliases de DNS

A documentação das APIs REST está disponível perto do seguinte local:

- [API REST do Banco de Dados SQL do Azure](https://docs.microsoft.com/rest/api/sql/)

Além disso, as APIs REST podem ser vistas no GitHub em:

- [Servidor de Banco de Dados SQL do Azure, APIs REST de alias do DNS](https://github.com/Azure/azure-rest-api-specs/blob/master/specification/sql/resource-manager/Microsoft.Sql/preview/2017-03-01-preview/serverDnsAliases.json)

<a name="anchor-powershell-code-62x"/>

#### <a name="powershell-for-managing-your-dns-aliases"></a>PowerShell para gerenciar seus aliases de DNS

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> O módulo PowerShell Azure Resource Manager ainda é suportado pelo Banco de Dados SQL do Azure, mas todo o desenvolvimento futuro é para o módulo Az.Sql. Para obter esses cmdlets, consulte [AzureRM.Sql](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Os argumentos para os comandos no módulo Az e nos módulos AzureRm são substancialmente idênticos.

Os cmdlets do PowerShell que chamam as APIs REST estão disponíveis.

Há um exemplo de código de cmdlets do PowerShell usados para gerenciar aliases de DNS documentado em:

- [PowerShell para alias do DNS para o Banco de Dados SQL do Azure](dns-alias-powershell.md)

Os cmdlets usados no exemplo de código são os seguintes:

- [New-AzSqlServerDNSAlias](https://docs.microsoft.com/powershell/module/az.Sql/New-azSqlServerDnsAlias): Cria um novo alias De DNS no sistema de serviço sql de banco de dados Do Azure SQL. O alias refere-se ao servidor do Banco de Dados SQL do Azure 1.
- [Get-AzSqlServerDNSAlias](https://docs.microsoft.com/powershell/module/az.Sql/Get-azSqlServerDnsAlias): Obtenha e liste todos os aliases DNS atribuídos ao servidor SQL DB 1.
- [Set-AzSqlServerDNSAlias](https://docs.microsoft.com/powershell/module/az.Sql/Set-azSqlServerDnsAlias): Modifica o nome do servidor ao que o alias está configurado para se referir, do servidor 1 ao servidor SQL DB 2.
- [Remove-AzSqlServerDNSAlias](https://docs.microsoft.com/powershell/module/az.Sql/Remove-azSqlServerDnsAlias): Remova o alias DNS do servidor SQL DB 2, usando o nome do alias.

## <a name="limitations-during-preview"></a>Limitações durante a versão prévia

Atualmente, um alias de DNS tem as seguintes limitações:

- *Atraso de até 2 minutos:* leva até 2 minutos para que um alias de DNS seja atualizado ou removido.
  - Independentemente de qualquer breve atraso, o alias interrompe imediatamente as conexões de cliente que fazem referência ao servidor herdado.
- *Pesquisa de DNS:* por enquanto, a única maneira oficial de verificar a qual servidor um alias de DNS específico faz referência é por meio da execução de uma [pesquisa de DNS](https://docs.microsoft.com/windows-server/administration/windows-commands/nslookup).
- _A auditoria da tabela não é suportada:_ Não é possível usar um alias DNS em um servidor de banco de dados SQL do Azure que tenha *auditoria de tabela* ativada em um banco de dados.
  - A auditoria de tabela foi preterida.
  - É recomendável que você mude para a [Auditoria de Blob](sql-database-auditing.md).

## <a name="related-resources"></a>Recursos relacionados

- [Visão geral da continuidade de negócios com o Banco de Dados SQL do Azure](sql-database-business-continuity.md), incluindo recuperação de desastre.

## <a name="next-steps"></a>Próximas etapas

- [PowerShell para alias do DNS para o Banco de Dados SQL do Azure](dns-alias-powershell.md)

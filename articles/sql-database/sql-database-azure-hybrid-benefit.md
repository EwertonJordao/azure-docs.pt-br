---
title: Benefício Híbrido do Azure
description: Use licenças de SQL Server existentes para descontos do banco de dados SQL.
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: sashan, moslake, carlrab
ms.date: 11/13/2019
ms.openlocfilehash: d1a59e7ad86191bcc30b7d898d00f327c20fbc5e
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2020
ms.locfileid: "75945615"
---
# <a name="azure-hybrid-benefit"></a>Benefício Híbrido do Azure

Na camada de computação provisionada do modelo de compra baseado em vCore, você pode trocar suas licenças existentes por tarifas com desconto no banco de dados SQL usando [benefício híbrido do Azure para SQL Server](https://azure.microsoft.com/pricing/hybrid-benefit/). Esse benefício do Azure permite que você economize até 30 por cento ou ainda mais no banco de dados SQL do Azure usando suas licenças de SQL Server locais com o Software Assurance. Use a calculadora de Benefício Híbrido do Azure usando o link mencionado antes para os valores corretos. 

> [!NOTE]
> Alterar para Benefício Híbrido do Azure não requer nenhum tempo de inatividade.

![preços](./media/sql-database-service-tiers/pricing.png)

## <a name="choose-a-license-model"></a>Escolher um modelo de licença

Com Benefício Híbrido do Azure, você pode optar por pagar apenas pela infraestrutura subjacente do Azure usando sua licença de SQL Server existente para o próprio mecanismo de banco de dados SQL (preço de computação base) ou pode pagar pela infraestrutura subjacente e pela licença de SQL Server (preço incluído na licença).

Você pode escolher ou alterar seu modelo de licenciamento usando o portal do Azure ou usando uma das seguintes APIs:

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Para definir ou atualizar o tipo de licença usando o PowerShell:

- [New-AzSqlDatabase](/powershell/module/az.sql/new-azsqldatabase)
- [Set-AzSqlDatabase](/powershell/module/az.sql/set-azsqldatabase)
- [New-AzSqlInstance](/powershell/module/az.sql/new-azsqlinstance)
- [Set-AzSqlInstance](/powershell/module/az.sql/set-azsqlinstance)

# <a name="azure-cli"></a>[CLI do Azure](#tab/azure-cli)

Para definir ou atualizar o tipo de licença usando o CLI do Azure:

- [az sql db create](/cli/azure/sql/db#az-sql-db-create)
- [az sql db update](/cli/azure/sql/db#az-sql-db-update)
- [az sql mi create](/cli/azure/sql/mi#az-sql-mi-create)
- [az sql mi update](/cli/azure/sql/mi#az-sql-mi-update)

# <a name="rest-api"></a>[REST API](#tab/rest)

Para definir ou atualizar o tipo de licença usando a API REST:

- [Banco de Dados – Criar ou Atualizar](/rest/api/sql/databases/createorupdate)
- [Bancos de Dados – Atualizar](/rest/api/sql/databases/update)
- [Managed Instances - Create Or Update](/rest/api/sql/managedinstances/createorupdate)
- [Managed Instances - Update](/rest/api/sql/managedinstances/update)

* * *

## <a name="next-steps"></a>Próximas etapas

- Para escolher entre as opções de implantação do banco de dados SQL, consulte [escolher a opção de implantação correta no SQL do Azure](sql-database-paas-vs-sql-server-iaas.md).
- Para obter uma comparação dos recursos do banco de dados SQL, consulte [recursos do banco de dados SQL do Azure](sql-database-features.md).

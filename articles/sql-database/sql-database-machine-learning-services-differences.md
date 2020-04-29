---
title: Principais diferenças para Serviços de Machine Learning (versão prévia)
description: Este tópico descreve as principais diferenças entre os serviços do Machine Learning do Banco de Dados SQL do Azure (com R) e os serviços do Machine Learning do SQL Server.
services: sql-database
ms.service: sql-database
ms.subservice: machine-learning
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: dphansen
ms.author: davidph
ms.reviewer: carlrab
manager: cgronlun
ms.date: 11/20/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 34ba75b6126024c9cd43d6fe474f7c1b62dd990f
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81453142"
---
# <a name="key-differences-between-machine-learning-services-in-azure-sql-database-preview-and-sql-server"></a>Principais diferenças entre Serviços de Machine Learning no banco de dados SQL do Azure (visualização) e SQL Server

A funcionalidade do banco de dados SQL do Azure Serviços de Machine Learning (com R) no (visualização) é semelhante ao [SQL Server serviços de Machine Learning](https://docs.microsoft.com/sql/advanced-analytics/what-is-sql-server-machine-learning). Abaixo estão algumas diferenças importantes.

[!INCLUDE[ml-preview-note](../../includes/sql-database-ml-preview-note.md)]

## <a name="language-support"></a>Suporte ao idioma

O SQL Server tem suporte para R e Python por meio da [estrutura de extensibilidade](https://docs.microsoft.com/sql/advanced-analytics/concepts/extensibility-framework). O Banco de Dados SQL não dá suporte às duas linguagens. As principais diferenças são:

- O R é a única linguagem com suporte no Banco de Dados SQL. No momento, não há suporte para Python.
- A versão do R é 3.4.4.
- Não é necessário configurar `external scripts enabled` por meio de `sp_configure`.

## <a name="package-management"></a>Gerenciamento de pacotes

O gerenciamento e a instalação de pacotes R funcionam de maneira diferente entre o Banco de Dados SQL e o SQL Server. Essas diferenças são:

- Os pacotes R são instalados por meio de [sqlmlutils](https://github.com/Microsoft/sqlmlutils) ou [CREATE EXTERNAL LIBRARY](https://docs.microsoft.com/sql/t-sql/statements/create-external-library-transact-sql).
- Os pacotes não podem realizar chamadas de rede de saída. Essa limitação é semelhante às [regras de firewall padrão para serviços de Machine Learning](https://docs.microsoft.com//sql/advanced-analytics/security/firewall-configuration) em SQL Server, mas não pode ser alterada no banco de dados SQL.
- Não há suporte para pacotes que dependem de runtimes externos (como Java) ou que precisam acessar APIs de OS para instalação ou uso.

## <a name="writing-to-a-temporary-table"></a>Gravando em uma tabela temporária

Se você estiver usando o RODBC no banco de dados SQL do Azure, não poderá gravar em uma tabela temporária, seja ela criada dentro ou fora `sp_execute_external_script` da sessão. A solução alternativa é usar [RxOdbcData](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxodbcdata) e [rxDataStep](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxdatastep) (with overwrite = false e Append = "Rows") para gravar em uma tabela temporária global criada antes da `sp_execute_external_script` consulta.

## <a name="resource-governance"></a>Governança de recursos

Não é possível limitar os recursos de R por meio do [Resource Governor](https://docs.microsoft.com/sql/relational-databases/resource-governor/resource-governor) e de pools de recursos externos.

Durante a visualização pública, os recursos do R são definidos como um máximo de 20% dos recursos do banco de dados SQL e dependem da camada de serviço que você escolher. Para saber mais, confira [Modelos de compra do Banco de Dados SQL do Azure](https://docs.microsoft.com/azure/sql-database/sql-database-service-tiers).
### <a name="insufficient-memory-error"></a>Erro de memória insuficiente

Se não houver memória suficiente disponível para o R, você receberá uma mensagem de erro. As mensagens de erro comuns são:

- Não é possível se comunicar com o tempo de execução para o script ' R ' para a ID de solicitação: * * * * * * *. Verifique os requisitos do tempo de execução ' R '
- Ocorreu um erro de script ' R ' durante a execução de ' sp_execute_external_script ' com HRESULT 0x80004004. ... ocorreu um erro de script externo: ".. Não foi possível alocar memória (0 MB) na função C ' R_AllocStringBuffer ' "
- Ocorreu um erro de script externo: erro: não é possível alocar vetor de tamanho.

O uso de memória depende de quanto é usado em seus scripts R e o número de consultas paralelas sendo executadas. Se você receber os erros acima, poderá dimensionar seu banco de dados para uma camada de serviço superior para resolver isso.

## <a name="next-steps"></a>Próximas etapas

- Consulte a visão geral, [banco de dados SQL do Azure serviços de Machine Learning com R (versão prévia)](sql-database-machine-learning-services-overview.md).
- Para saber como usar o R para consultar o Serviços de Machine Learning do banco de dados SQL do Azure (versão prévia), consulte o [Guia de início rápido](sql-database-connect-query-r.md).
- Para começar com alguns scripts R simples, consulte [criar e executar scripts r simples no banco de dados SQL do Azure serviços de Machine Learning (versão prévia)](sql-database-quickstart-r-create-script.md).

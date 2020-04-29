---
title: Em vez de ETL, design ELT
description: Implementar estratégias de carregamento de dados flexíveis para o pool SQL Synapse no Azure Synapse Analytics
services: synapse-analytics
author: kevinvngo
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 02/19/2020
ms.author: kevin
ms.reviewer: igorstan
ms.custom: azure-synapse
ms.openlocfilehash: e99fd898956e11a4827d023691111a47e5a790c0
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80744968"
---
# <a name="data-loading-strategies-for-synapse-sql-pool"></a>Estratégias de carregamento de dados para o pool SQL Synapse

Os pools de SQL SMP tradicionais usam um processo ETL (extração, transformação e carregamento) para carregar dados. O Synapse pool SQL, no Azure Synapse Analytics, tem uma arquitetura MPP (processamento paralelo maciço) que aproveita a escalabilidade e a flexibilidade dos recursos de computação e armazenamento.

O uso de um processo ELT (extração, carregamento e transformação) aproveita o MPP e elimina os recursos necessários para a transformação de dados antes do carregamento.

Embora o pool do SQL dê suporte a muitos métodos de carregamento, incluindo opções populares de SQL Server como [bcp](/sql/tools/bcp-utility?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) e a [API SqlBulkCopy](/dotnet/api/system.data.sqlclient.sqlbulkcopy?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json), a maneira mais rápida e escalonável de carregar dados é por meio de tabelas externas do polybase e da [instrução de cópia](/sql/t-sql/statements/copy-into-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) (visualização).

Com o polybase e a instrução de cópia, você pode acessar dados externos armazenados no armazenamento de BLOBs do Azure ou Azure Data Lake Store por meio da linguagem T-SQL. Para obter mais flexibilidade ao carregar, é recomendável usar a instrução COPY.

> [!NOTE]  
> Atualmente, a instrução COPY está em visualização pública. Para fornecer comentários, envie um email para a seguinte lista de sqldwcopypreview@service.microsoft.comdistribuição:.

> [!VIDEO https://www.youtube.com/embed/l9-wP7OdhDk]

## <a name="what-is-elt"></a>O que é ELT?

O ELT (extração, carregamento e transformação) é um processo pelo qual os dados são extraídos de um sistema de origem, carregados em um pool do SQL e, em seguida, transformados.

As etapas básicas para implementar ELT são:

1. Extrair os dados de origem em arquivos de texto.
2. Descarregar os dados no armazenamento de Blobs do Azure ou no Azure Data Lake Store.
3. Preparar os dados para o carregamento.
4. Carregue os dados em tabelas de preparo com o polybase ou o comando de cópia.
5. Transformar os dados.
6. Inserir os dados nas tabelas de produção.

Para obter um tutorial de carregamento do polybase, consulte [usar o polybase para carregar dados do armazenamento de BLOBs do Azure](load-data-from-azure-blob-storage-using-polybase.md).

Para obter mais informações, consulte [Blog de padrão de carga](https://blogs.msdn.microsoft.com/sqlcat/20../../azure-sql-data-warehouse-loading-patterns-and-strategies/).

## <a name="1-extract-the-source-data-into-text-files"></a>1. Extraia os dados de origem em arquivos de texto

Obter dados de fora do seu sistema de origem depende da localização de armazenamento.  O objetivo é mover os dados para o polybase e a cópia com suporte de texto delimitado ou arquivos CSV.

### <a name="polybase-and-copy-external-file-formats"></a>Polybase e copiar formatos de arquivo externo

Com o polybase e a instrução de cópia, você pode carregar dados de texto delimitado codificado em UTF-8 e UTF-16 ou arquivos CSV. Além de texto delimitado ou arquivos CSV, ele é carregado a partir dos formatos de arquivo do Hadoop, como ORC e parquet. O polybase e a instrução de cópia também podem carregar dados de arquivos compactados gzip e de instantâneo.

Não há suporte para ASCII estendido, formato de largura fixa e formatos aninhados como o WinZip ou XML. Se você estiver exportando do SQL Server, poderá usar a [ferramenta de linha de comando bcp](/sql/tools/bcp-utility?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) para exportar os dados para arquivos de texto delimitados.

## <a name="2-land-the-data-into-azure-blob-storage-or-azure-data-lake-store"></a>2. Pouse os dados no armazenamento de BLOBs do Azure ou Azure Data Lake Store

Para colocar os dados no armazenamento do Azure, você pode movê-los para o [armazenamento de BLOBs do Azure](../../storage/blobs/storage-blobs-introduction.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) ou [Azure data Lake Store Gen2](../../data-lake-store/data-lake-store-overview.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json). Em qualquer localização, os dados devem ser armazenados em arquivos de texto. O polybase e a instrução de cópia podem ser carregados de qualquer um dos locais.

Ferramentas e serviços que você pode usar para mover dados para o Armazenamento do Microsoft Azure:

- O serviço [Azure ExpressRoute](../../expressroute/expressroute-introduction.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) melhora a taxa de transferência de rede, o desempenho e a previsibilidade. O ExpressRoute é um serviço que encaminha os dados por uma conexão privada dedicada para o Azure. As conexões do ExpressRoute não encaminham dados pela Internet pública. As conexões oferecem mais confiabilidade e velocidade, latências menores e maior segurança do que as conexões comuns pela Internet.
- O [Utilitário AZCopy](../../storage/common/storage-choose-data-transfer-solution.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) move os dados para o Armazenamento do Microsoft Azure pela internet pública. Isso funciona se os tamanhos dos seus dados forem inferiores a 10 TB. Para executar cargas regularmente com AZCopy, teste a velocidade da rede para ver se ela é aceitável.
- O [Azure Data Factory (ADF)](../../data-factory/introduction.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) tem um gateway que você pode instalar no seu servidor local. Em seguida, você pode criar um pipeline para mover os dados do seu servidor local para o Armazenamento do Microsoft Azure. Para usar Data Factory com a análise de SQL, consulte [carregando dados para análise de SQL](../../data-factory/load-azure-sql-data-warehouse.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json).

## <a name="3-prepare-the-data-for-loading"></a>3. preparar os dados para carregar

Talvez seja necessário preparar e limpar os dados em sua conta de armazenamento antes de carregar. A preparação de dados pode ser executada enquanto seus dados estiverem na origem, conforme você exporta os dados para arquivos de texto ou após os dados no Armazenamento do Microsoft Azure.  É mais fácil trabalhar com os dados o mais precocemente possível no processo.  

### <a name="define-external-tables"></a>Definir tabelas externas

Se você estiver usando o polybase, precisará definir tabelas externas em seu pool do SQL antes de carregar. As tabelas externas não são exigidas pela instrução de cópia. O PolyBase usa tabelas externas para definir e acessar os dados no Armazenamento do Microsoft Azure.

Uma tabela externa é semelhante a uma exibição de banco de dados. A tabela externa contém o esquema de tabela e aponta para os dados que são armazenados fora do pool do SQL.

Definir tabelas externas envolve a especificação da fonte de dados, o formato dos arquivos de texto e as definições de tabela. Os artigos de referência de sintaxe T-SQL que serão necessários são:

- [CREATE EXTERNAL DATA SOURCE](/sql/t-sql/statements/create-external-data-source-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
- [CREATE EXTERNAL FILE FORMAT](/sql/t-sql/statements/create-external-file-format-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
- [CREATE EXTERNAL TABLE](/sql/t-sql/statements/create-external-table-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)

Ao carregar parquet, o mapeamento de tipo de dados SQL é:

| **Tipo de dados parquet** | **Tipo de dados SQL** |
| :-------------------: | :---------------: |
|        TINYINT        |      TINYINT      |
|       SMALLINT        |     SMALLINT      |
|          INT          |        INT        |
|        BIGINT         |      BIGINT       |
|        booleano        |        bit        |
|        double         |       FLOAT       |
|         FLOAT         |       real        |
|        double         |       money       |
|        double         |    SMALLMONEY     |
|        cadeia de caracteres         |       NCHAR       |
|        cadeia de caracteres         |     NVARCHAR      |
|        cadeia de caracteres         |       char        |
|        cadeia de caracteres         |      varchar      |
|        binary         |      binary       |
|        binary         |     varbinary     |
|       timestamp       |       date        |
|       timestamp       |   smalldatetime   |
|       timestamp       |     datetime2     |
|       timestamp       |     DATETIME      |
|       timestamp       |       time        |
|         date          |       date        |
|        decimal        |      decimal      |

Para obter um exemplo de criação de objetos externos, consulte a etapa [Criar tabelas externas](load-data-from-azure-blob-storage-using-polybase.md#create-external-tables-for-the-sample-data) no tutorial de carregamento.

### <a name="format-text-files"></a>Arquivos de texto formatados

Se você estiver usando o polybase, os objetos externos definidos precisarão alinhar as linhas dos arquivos de texto com a tabela externa e a definição de formato de arquivo. Os dados em cada linha do arquivo de texto devem se alinhar com a definição da tabela.
Para formatar os arquivos de texto:

- Se os seus dados forem provenientes de uma fonte não relacional, você precisa transformá-los em linhas e colunas. Se os dados forem de uma fonte relacional ou não, os dados devem ser transformados para se alinharem com as definições de coluna para a tabela na qual você planeja carregar os dados.
- Formate os dados no arquivo de texto para alinhá-los com as colunas e os tipos de dados na tabela de destino. O desalinhamento entre os tipos de dados nos arquivos de texto externos e a tabela do pool do SQL faz com que as linhas sejam rejeitadas durante a carga.
- Separar os campos no arquivo de texto com um terminador.  Certifique-se de usar um caractere ou uma sequência de caracteres que não seja encontrada em seus dados de origem. Use o terminador especificado com [CRIAR FORMATO DE ARQUIVO EXTERNO](/sql/t-sql/statements/create-external-file-format-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest).

## <a name="4-load-the-data-using-polybase-or-the-copy-statement"></a>4. carregar os dados usando o polybase ou a instrução de cópia

É uma melhor prática carregar dados em uma tabela de preparo. Tabelas de preparo permitem manipular erros sem interferir nas tabelas de produção. Uma tabela de preparo também oferece a oportunidade de usar o SQL pool MPP para transformações de dados antes de inserir os dados em tabelas de produção.

A tabela precisará ser criada previamente ao carregar em uma tabela de preparo com cópia.

### <a name="options-for-loading-with-polybase-and-copy-statement"></a>Opções para carregamento com o polybase e a instrução de cópia

Para carregar dados com o PolyBase, é possível usar qualquer uma destas opções de carregamento:

- O [PolyBase com o T-SQL](load-data-from-azure-blob-storage-using-polybase.md) funciona bem quando os seus dados estiverem no armazenamento de Blobs do Azure ou no Azure Data Lake Store. Ele oferece mais controle sobre o processo de carregamento, mas também exige que você defina objetos de dados externos. Os outros métodos definem esses objetos em segundo plano, como mapear as tabelas de origem para as tabelas de destino.  Para coordenar as cargas de T-SQL, você pode usar o Azure Data Factory, SSIS ou as funções do Azure.
- O [PolyBase com o SSIS](/sql/integration-services/load-data-to-sql-data-warehouse?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) funciona bem quando os seus dados de origem estiverem no SQL Server, no SQL Server local ou na nuvem. O SSIS define a origem para mapeamentos de tabela de destino e também coordena a carga. Se você já tiver pacotes SSIS, você pode modificar os pacotes para trabalhar com o novo destino do data warehouse.
- O [polybase e a instrução de cópia com Azure data Factory (ADF)](../../data-factory/load-azure-sql-data-warehouse.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) é outra ferramenta de orquestração.  Ele define um pipeline e agenda de trabalhos.
- O [polybase com Azure Databricks](../../azure-databricks/databricks-extract-load-sql-data-warehouse.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) transfere dados de uma tabela para um dataframe do databricks e/ou grava dados de um dataframe do databricks em uma tabela usando o polybase.

### <a name="other-loading-options"></a>Outras opções de carregamento

Além do polybase e da instrução COPY, você pode usar o [bcp](/sql/tools/bcp-utility?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) ou a [API SqlBulkCopy](/dotnet/api/system.data.sqlclient.sqlbulkcopy?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json). o bcp é carregado diretamente no banco de dados sem passar pelo armazenamento de BLOBs do Azure e destina-se apenas a pequenas cargas.

> [!NOTE]
> O desempenho de carga dessas opções é mais lento do que o polybase e a instrução de cópia.

## <a name="5-transform-the-data"></a>5. transformar os dados

Enquanto os dados estão na tabela de preparo, execute as transformações que a sua carga de trabalho exige. Em seguida, mova os dados para uma tabela de produção.

## <a name="6-insert-the-data-into-production-tables"></a>6. inserir os dados em tabelas de produção

A instrução INSERT INTO... A instrução SELECT move os dados da tabela de preparo para a tabela permanente.

Ao criar um processo de ETL, tente executar o processo em uma amostra de teste pequena. Tente extrair 1000 linhas da tabela para um arquivo, movê-lo para o Azure e, em seguida, tente carregá-lo em uma tabela de preparo.

## <a name="partner-loading-solutions"></a>Soluções de carregamento de parceiros

Muitos de nossos parceiros têm soluções de carregamento. Para saber mais, consulte uma lista dos nossos [parceiros de solução](sql-data-warehouse-partner-business-intelligence.md).

## <a name="next-steps"></a>Próximas etapas

Para orientações sobre carregamento, consulte [Diretrizes para carregar dados](guidance-for-loading-data.md).

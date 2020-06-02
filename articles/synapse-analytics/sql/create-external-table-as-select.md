---
title: Armazenar os resultados da consulta no armazenamento
description: Neste artigo, você aprenderá a armazenar os resultados da consulta no armazenamento usando o SQL sob demanda (versão prévia).
services: synapse-analytics
author: vvasic-msft
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 04/15/2020
ms.author: vvasic
ms.reviewer: jrasnick, carlrab
ms.openlocfilehash: cbf6d42f3b1d130a6bf89f07bd3a7009ff0e8fa8
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83647526"
---
# <a name="store-query-results-to-storage-using-sql-on-demand-preview-using-azure-synapse-analytics"></a>Armazenar os resultados da consulta no armazenamento usando o SQL sob demanda (versão prévia) e o Azure Synapse Analytics

Neste artigo, você aprenderá a armazenar os resultados da consulta no armazenamento usando o SQL sob demanda (versão prévia).

## <a name="prerequisites"></a>Pré-requisitos

A primeira etapa é examinar os artigos abaixo e verificar se você atendeu aos pré-requisitos:

- [Configuração inicial](query-data-storage.md#first-time-setup)
- [Pré-requisitos](query-data-storage.md#prerequisites)

## <a name="create-external-table-as-select"></a>Create external table as select

Você pode usar a instrução CETAS (CREATE EXTERNAL TABLE AS SELECT) para armazenar os resultados da consulta no armazenamento.

> [!NOTE]
> Altere a primeira linha da consulta, ou seja, [mydbname], de modo que você esteja usando o banco de dados criado. Caso você não tenha criado um banco de dados, leia [Configuração inicial](query-data-storage.md#first-time-setup). Você precisa alterar a LOCALIZAÇÃO para que a fonte de dados externa MyDataSource aponte para a localização para a qual você tem permissão de gravação. 

```sql
USE [mydbname];
GO

CREATE DATABASE SCOPED CREDENTIAL [SasTokenWrite]
WITH IDENTITY = 'SHARED ACCESS SIGNATURE',
     SECRET = 'sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-04-18T20:42:12Z&st=2019-04-18T12:42:12Z&spr=https&sig=lQHczNvrk1KoYLCpFdSsMANd0ef9BrIPBNJ3VYEIq78%3D';
GO

CREATE EXTERNAL DATA SOURCE [MyDataSource] WITH (
    LOCATION = 'https://<storage account name>.blob.core.windows.net/csv', CREDENTIAL [SasTokenWrite]
);
GO

CREATE EXTERNAL FILE FORMAT [ParquetFF] WITH (
    FORMAT_TYPE = PARQUET,
    DATA_COMPRESSION = 'org.apache.hadoop.io.compress.SnappyCodec'
);
GO

CREATE EXTERNAL TABLE [dbo].[PopulationCETAS] WITH (
        LOCATION = 'populationParquet/',
        DATA_SOURCE = [MyDataSource],
        FILE_FORMAT = [ParquetFF]
) AS
SELECT
    *
FROM
    OPENROWSET(
        BULK 'https://sqlondemandstorage.blob.core.windows.net/csv/population-unix/population.csv',
        FORMAT='CSV'
    ) WITH (
        CountryCode varchar(4),
        CountryName varchar(64),
        Year int,
        PopulationCount int
    ) AS r;

```

## <a name="use-the-external-table"></a>Usar a tabela externa

Você pode usar a tabela externa criada por meio da CETAS como uma tabela externa normal.

> [!NOTE]
> Altere a primeira linha da consulta, ou seja, [mydbname], de modo que você esteja usando o banco de dados criado. Caso você não tenha criado um banco de dados, leia [Configuração inicial](query-data-storage.md#first-time-setup).

```sql
USE [mydbname];
GO

SELECT
    country_name, population
FROM PopulationCETAS
WHERE
    [year] = 2019
ORDER BY
    [population] DESC;
```

## <a name="next-steps"></a>Próximas etapas

Para obter mais informações sobre como consultar diferentes tipos de arquivo, veja os artigos [Consultar um arquivo CSV individual](query-single-csv-file.md), [Consultar arquivos Parquet](query-parquet-files.md) e [Consultar arquivos JSON](query-json-files.md).

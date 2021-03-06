---
title: 'Tutorial: Introdução à análise de dados em contas de armazenamento'
description: Neste tutorial, você aprenderá a analisar os dados localizados em uma conta de armazenamento.
services: synapse-analytics
author: saveenr
ms.author: saveenr
manager: julieMSFT
ms.reviewer: jrasnick
ms.service: synapse-analytics
ms.topic: tutorial
ms.date: 07/20/2020
ms.openlocfilehash: a0d5c758873413e549b31e3ec4cc41791fc8c371
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/09/2020
ms.locfileid: "89667420"
---
# <a name="analyze-data-in-a-storage-account"></a>Analisar dados em uma conta de armazenamento

Neste tutorial, você aprenderá a analisar os dados localizados em uma conta de armazenamento.

## <a name="overview"></a>Visão geral

Até agora nós abordamos cenários em que os dados residem em bancos de dados no workspace. Agora, mostraremos a você como trabalhar com arquivos em contas de armazenamento. Nesse cenário, usaremos a conta de armazenamento primária do workspace e do contêiner que especificamos ao criar o workspace.

* O nome da conta de armazenamento: **contosolake**
* O nome do contêiner na conta de armazenamento: **usuários**

### <a name="create-csv-and-parquet-files-in-your-storage-account"></a>Criar arquivos CSV e Parquet na conta de armazenamento

Execute o código a seguir em um notebook. Ele cria um arquivo CSV e um arquivo Parquet na conta de armazenamento.

```py
%%pyspark
df = spark.sql("SELECT * FROM nyctaxi.passengercountstats")
df = df.repartition(1) # This ensure we'll get a single file during write()
df.write.mode("overwrite").csv("/NYCTaxi/PassengerCountStats.csv")
df.write.mode("overwrite").parquet("/NYCTaxi/PassengerCountStats.parquet")
```

### <a name="analyze-data-in-a-storage-account"></a>Analisar dados em uma conta de armazenamento

1. No Synapse Studio, acesse o hub **Dados** e, em seguida, selecione **Vinculado**.
1. Navegue até **Contas de armazenamento** > **myworkspace (Primário – contosolake)** .
1. Selecione **usuários (Primário)** . Você deve ver a pasta **NYCTaxi**. Dentro dela, você verá duas pastas: **PassengerCountStats.csv** e **PassengerCountStats.parquet**.
1. Abra a pasta **PassengerCountStats.parquet**. Dentro dela, você verá um arquivo Parquet com um nome como `part-00000-2638e00c-0790-496b-a523-578da9a15019-c000.snappy.parquet`.
1. Clique com o botão direito do mouse em **.parquet** e selecione **novo notebook**. Ele cria um notebook que tem uma célula como esta:

    ```py
    %%pyspark
    data_path = spark.read.load('abfss://users@contosolake.dfs.core.windows.net/NYCTaxi/PassengerCountStats.parquet/part-00000-1f251a58-d8ac-4972-9215-8d528d490690-c000.snappy.parquet', format='parquet')
    data_path.show(100)
    ```

1. Execute a célula.
1. Clique com o botão direito do mouse no arquivo Parquet contido nele e selecione **Novo script SQL** > **SELECIONAR AS 100 PRIMEIRAS LINHAS**. Ele cria um script SQL como este:

    ```sql
    SELECT TOP 100 *
    FROM OPENROWSET(
        BULK 'https://contosolake.dfs.core.windows.net/users/NYCTaxi/PassengerCountStats.parquet/part-00000-1f251a58-d8ac-4972-9215-8d528d490690-c000.snappy.parquet',
        FORMAT='PARQUET'
    ) AS [r];
    ```

    Na janela de script, **Conectar-se a** é definido como **SQL sob demanda**.

1. Execute o script.



## <a name="next-steps"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Orquestrar atividades com pipelines](get-started-pipelines.md)

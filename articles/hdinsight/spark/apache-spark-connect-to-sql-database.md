---
title: Use o Apache Spark para ler e gravar dados no Banco de Dados SQL do Azure
description: Saiba como configurar uma conexão entre o cluster HDInsight Spark e um Banco de Dados SQL do Azure para ler dados, gravar dados e transmitir dados em um banco de dados SQL
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 03/05/2020
ms.openlocfilehash: 4e0c1626582297aa7d80cbbd4241b6f81e314f8f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78927453"
---
# <a name="use-hdinsight-spark-cluster-to-read-and-write-data-to-azure-sql-database"></a>Use o cluster HDInsight Spark para ler e gravar dados no Banco de Dados SQL do Azure

Aprenda a conectar um cluster Apache Spark no Azure HDInsight com um Banco de Dados SQL do Azure e, em seguida, leia, escreva e transmita dados no banco de dados SQL. As instruções deste artigo usam um [Caderno Jupyter](https://jupyter.org/) para executar os trechos do código Scala. No entanto, você pode criar um aplicativo autônomo em Scala ou Python e realizar as mesmas tarefas.

## <a name="prerequisites"></a>Pré-requisitos

* Cluster do Azure HDInsight Spark.  Siga as instruções em [Criar um cluster do Apache Spark no HDInsight ](apache-spark-jupyter-spark-sql.md).

* Banco de Dados SQL do Azure. Siga as instruções em [Criar um Banco de Dados SQL do Azure](../../sql-database/sql-database-get-started-portal.md). Certifique-se de criar um banco de dados com o esquema e dados **AdventureWorksLT** de exemplo. Além disso, certifique-se de criar uma regra de firewall no nível do servidor para permitir que o endereço IP do seu cliente acesse o banco de dados SQL no servidor. As instruções para adicionar a regra de firewall estão disponíveis no mesmo artigo. Depois de criar seu Banco de Dados SQL do Azure, certifique-se de manter os seguintes valores à mão. Esses valores serão necessários para se conectar ao banco de dados de um cluster Spark.

    * Nome do servidor que hospeda o Banco de Dados SQL do Azure.
    * Nome do banco de dados Azure SQL.
    * Azure SQL Database admin nome de usuário / senha.

* SQL Server Management Studio (SSMS). Siga as instruções em [Usar SSMS para conectar e consultar dados](../../sql-database/sql-database-connect-query-ssms.md).

## <a name="create-a-jupyter-notebook"></a>Criar um Jupyter Notebook

Comece criando um [Jititer Notebook](https://jupyter.org/) associado ao cluster Spark. Você usa esse notebook para executar os snippets de código usados neste artigo.

1. Do [portal do Azure](https://portal.azure.com/), abra o seu cluster.
1. Selecione o **Notebook Jupyter** abaixo dos **painéis do Cluster** no lado direito.  Se você não ver **cluster dashboards**, selecione **Visão geral** no menu esquerdo. Se você receber uma solicitação, insira as credenciais de administrador para o cluster.

    ![Caderno jupyter em Apache Spark](./media/apache-spark-connect-to-sql-database/hdinsight-spark-cluster-dashboard-jupyter-notebook.png "Caderno jupyter em Spark")

   > [!NOTE]  
   > Também é possível acessar o notebook Jupyter no cluster Spark, abrindo a seguinte URL no seu navegador. Substitua **CLUSTERNAME** pelo nome do cluster:
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`

1. No notebook Jupyter, no canto superior direito, clique em **Novo**e, em seguida, clique em **Spark** para criar um notebook Scala. Os notebooks Jupyter no cluster do Azure HDInsight Spark também fornecem o kernel **PySpark** para aplicativos Python2 e o kernel **PySpark3** para aplicativos Python3. Para este artigo, criamos um notebook Scala.

    ![Kernels para notebook Jupyter em Spark](./media/apache-spark-connect-to-sql-database/kernel-jupyter-notebook-on-spark.png "Kernels para notebook Jupyter em Spark")

    Para saber mais sobre os três kernels, consulte [clusters kernels de blocos de anotações do Jupyter de uso com o Apache Spark no HDInsight](apache-spark-jupyter-notebook-kernels.md).

   > [!NOTE]  
   > Neste artigo, usamos um kernel (Scala) Spark porque atualmente os dados de streaming do Spark para banco de dados SQL têm suporte apenas em Scala e Java. Embora a leitura e gravação em SQL possam ser feitas usando o Python, visando manter a consistência neste artigo nós usamos o Scala para todas as três operações.

1. Isso abre um novo notebook com um nome padrão, **Sem título**. Clique no nome do notebook e insira o nome de sua escolha.

    ![Fornecer um nome para o bloco de anotações](./media/apache-spark-connect-to-sql-database/hdinsight-spark-jupyter-notebook-name.png "Fornecer um nome para o bloco de anotações")

Agora, você pode iniciar a criação do seu aplicativo.

## <a name="read-data-from-azure-sql-database"></a>Leia dados do Banco de Dados SQL do Azure

Nesta seção, você faz a leitura dos dados de uma tabela (por exemplo, **SalesLT.Address**) que existe no banco de dados AdventureWorks.

1. Em um novo notebook Jupyter, em uma célula de código, cole o seguinte trecho e substitua os valores do espaço reservado pelos valores do seu Banco de Dados SQL Do Azure.

       // Declare the values for your Azure SQL database

       val jdbcUsername = "<SQL DB ADMIN USER>"
       val jdbcPassword = "<SQL DB ADMIN PWD>"
       val jdbcHostname = "<SQL SERVER NAME HOSTING SDL DB>" //typically, this is in the form or servername.database.windows.net
       val jdbcPort = 1433
       val jdbcDatabase ="<AZURE SQL DB NAME>"

    Pressione **SHIFT + ENTER** para executar a célula de código.  

1. Use o trecho abaixo para construir uma URL JDBC que você pode passar para as APIs do quadro de dados Spark. O código `Properties` cria um objeto para manter os parâmetros. Cole o snippet de código a seguir em uma célula de código e pressione **SHIFT+ENTER** para executar.

       import java.util.Properties

       val jdbc_url = s"jdbc:sqlserver://${jdbcHostname}:${jdbcPort};database=${jdbcDatabase};encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=60;"
       val connectionProperties = new Properties()
       connectionProperties.put("user", s"${jdbcUsername}")
       connectionProperties.put("password", s"${jdbcPassword}")         

1. Use o trecho abaixo para criar um dataframe com os dados de uma tabela em seu Banco de Dados SQL do Azure. Neste trecho, usamos uma `SalesLT.Address` tabela que está disponível como parte do banco de dados **AdventureWorksLT.** Cole o snippet de código a seguir em uma célula de código e pressione **SHIFT+ENTER** para executar.

       val sqlTableDF = spark.read.jdbc(jdbc_url, "SalesLT.Address", connectionProperties)

1. Agora, é possível executar operações no dataframe, como obter o esquema de dados:

       sqlTableDF.printSchema

    Você verá uma saída semelhante à seguinte:

    ![saída esquema](./media/apache-spark-connect-to-sql-database/read-from-sql-schema-output.png "saída esquema")

1. Também é possível executar operações como, por exemplo, recuperar as 10 melhores filas.

       sqlTableDF.show(10)

1. Ou, recuperar colunas específicas do conjunto de dados.

       sqlTableDF.select("AddressLine1", "City").show(10)

## <a name="write-data-into-azure-sql-database"></a>Escreva dados no banco de dados Do Azure SQL

Nesta seção, usamos um arquivo CSV de amostra disponível no cluster para criar uma tabela no Banco de Dados SQL do Azure e preenche-lo com dados. O arquivo CSV de exemplo (**HVAC.csv**) está disponível em todos os clusters HDInsight em `HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv`.

1. Em um novo notebook Jupyter, em uma célula de código, cole o seguinte trecho e substitua os valores do espaço reservado pelos valores do seu Banco de Dados SQL Do Azure.

       // Declare the values for your Azure SQL database

       val jdbcUsername = "<SQL DB ADMIN USER>"
       val jdbcPassword = "<SQL DB ADMIN PWD>"
       val jdbcHostname = "<SQL SERVER NAME HOSTING SDL DB>" //typically, this is in the form or servername.database.windows.net
       val jdbcPort = 1433
       val jdbcDatabase ="<AZURE SQL DB NAME>"

    Pressione **SHIFT + ENTER** para executar a célula de código.  

1. O trecho a seguir cria uma URL JDBC que você pode passar para as APIs do quadro de dados Spark. O código `Properties` cria um objeto para manter os parâmetros. Cole o snippet de código a seguir em uma célula de código e pressione **SHIFT+ENTER** para executar.

       import java.util.Properties

       val jdbc_url = s"jdbc:sqlserver://${jdbcHostname}:${jdbcPort};database=${jdbcDatabase};encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=60;"
       val connectionProperties = new Properties()
       connectionProperties.put("user", s"${jdbcUsername}")
       connectionProperties.put("password", s"${jdbcPassword}")

1. Use o snippet de código abaixo para extrair o esquema dos dados em HVAC.csv e usar o esquema para carregar os dados do CSV em um dataframe, `readDf`. Cole o snippet de código a seguir em uma célula de código e pressione **SHIFT+ENTER** para executar.

       val userSchema = spark.read.option("header", "true").csv("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv").schema
       val readDf = spark.read.format("csv").schema(userSchema).load("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

1. Use o dataframe `readDf` para criar uma tabela temporária, `temphvactable`. Em seguida, use a tabela temporária para criar uma tabela de hive, `hvactable_hive`.

       readDf.createOrReplaceTempView("temphvactable")
       spark.sql("create table hvactable_hive as select * from temphvactable")

1. Finalmente, use a tabela colmeia para criar uma tabela no Banco de Dados SQL do Azure. O trecho a `hvactable` seguir é criado no Banco de Dados SQL do Azure.

       spark.table("hvactable_hive").write.jdbc(jdbc_url, "hvactable", connectionProperties)

1. Conecte-se ao banco de dados SQL do Azure usando SSMS e verifique se você vê um `dbo.hvactable` lá.

    a. Inicie o SSMS e conecte-se ao Banco de Dados SQL do Azure, fornecendo detalhes de conexão como mostrado na captura de tela abaixo.

    ![Conecte-se ao banco de dados SQL usando O SSMS1](./media/apache-spark-connect-to-sql-database/connect-to-sql-db-ssms.png "Conecte-se ao banco de dados SQL usando O SSMS1")

    b. A partir **do Object Explorer,** expanda o banco de dados SQL do Azure e o nó tabela para ver o **dbo.hvactable** criado.

    ![Conecte-se ao banco de dados SQL usando SSMS2](./media/apache-spark-connect-to-sql-database/connect-to-sql-db-ssms-locate-table.png "Conecte-se ao banco de dados SQL usando SSMS2")

1. Execute uma consulta no SSMS para ver as colunas na tabela.

    ```sql
    SELECT * from hvactable
    ```

## <a name="stream-data-into-azure-sql-database"></a>Fluxo de dados no banco de dados SQL do Azure

Nesta seção, transmitimos `hvactable` dados para o que você já criou no Banco de Dados SQL do Azure na seção anterior.

1. Como primeiro passo, certifique-se de `hvactable`que não há registros no . Usando SSMS, execute a seguinte consulta na tabela.

    ```sql
    TRUNCATE TABLE [dbo].[hvactable]
    ```

1. Crie um novo notebook Jupyter no cluster do Azure HDInsight Spark. Em uma célula de código, cole o seguinte snippet de código e, em seguida, pressione **SHIFT + ENTER**:

       import org.apache.spark.sql._
       import org.apache.spark.sql.types._
       import org.apache.spark.sql.functions._
       import org.apache.spark.sql.streaming._
       import java.sql.{Connection,DriverManager,ResultSet}

1. Nós transmitimos dados do **HVAC.csv** para o `hvactable`. O arquivo HVAC.csv está `/HdiSamples/HdiSamples/SensorSampleData/HVAC/`disponível no cluster em . No snippet de código a seguir, primeiro recebemos o esquema dos dados a serem transmitidos. Em seguida, criamos um dataframe de transmissão usando esse esquema. Cole o snippet de código a seguir em uma célula de código e pressione **SHIFT+ENTER** para executar.

       val userSchema = spark.read.option("header", "true").csv("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv").schema
       val readStreamDf = spark.readStream.schema(userSchema).csv("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/") 
       readStreamDf.printSchema

1. A saída mostra o esquema de **HVAC.csv**. O `hvactable` tem o mesmo esquema também. A saída lista as colunas na tabela.

    ![tabela de esquema apache spark hdinsight](./media/apache-spark-connect-to-sql-database/hdinsight-schema-table.png "Esquema da tabela")

1. Finalmente, use o seguinte trecho para ler dados do HVAC.csv e transmiti-los para o `hvactable` banco de dados Do Azure SQL. Cole o trecho em uma célula de código, substitua os valores do espaço reservado pelos valores do seu Banco de Dados SQL Do Azure e pressione **SHIFT + ENTER** para executar.

       val WriteToSQLQuery  = readStreamDf.writeStream.foreach(new ForeachWriter[Row] {
          var connection:java.sql.Connection = _
          var statement:java.sql.Statement = _
          
          val jdbcUsername = "<SQL DB ADMIN USER>"
          val jdbcPassword = "<SQL DB ADMIN PWD>"
          val jdbcHostname = "<SQL SERVER NAME HOSTING SDL DB>" //typically, this is in the form or servername.database.windows.net
          val jdbcPort = 1433
          val jdbcDatabase ="<AZURE SQL DB NAME>"
          val driver = "com.microsoft.sqlserver.jdbc.SQLServerDriver"
          val jdbc_url = s"jdbc:sqlserver://${jdbcHostname}:${jdbcPort};database=${jdbcDatabase};encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=30;"
  
         def open(partitionId: Long, version: Long):Boolean = {
           Class.forName(driver)
           connection = DriverManager.getConnection(jdbc_url, jdbcUsername, jdbcPassword)
           statement = connection.createStatement
           true
         }
  
         def process(value: Row): Unit = {
           val Date  = value(0)
           val Time = value(1)
           val TargetTemp = value(2)
           val ActualTemp = value(3)
           val System = value(4)
           val SystemAge = value(5)
           val BuildingID = value(6)  
    
           val valueStr = "'" + Date + "'," + "'" + Time + "'," + "'" + TargetTemp + "'," + "'" + ActualTemp + "'," + "'" + System + "'," + "'" + SystemAge + "'," + "'" + BuildingID + "'"
           statement.execute("INSERT INTO " + "dbo.hvactable" + " VALUES (" + valueStr + ")")   
           }

         def close(errorOrNull: Throwable): Unit = {
            connection.close
          }
         })
        
         var streamingQuery = WriteToSQLQuery.start()

1. Verifique se os dados estão `hvactable` sendo transmitidos para o executando a seguinte consulta no SQL Server Management Studio (SSMS). Toda vez que você executa a consulta, ele mostra o número de linhas na tabela aumentando.

    ```sql
    SELECT COUNT(*) FROM hvactable
    ```

## <a name="next-steps"></a>Próximas etapas

* [Usar o cluster HDInsight Spark para analisar dados no Data Lake Storage](apache-spark-use-with-data-lake-store.md)
* [Processo de eventos de fluxo estruturado usando EventHub](apache-spark-eventhub-structured-streaming.md)
* [Usar o fluxo estruturado do Apache Spark com o Apache Kafka no HDInsight](../hdinsight-apache-kafka-spark-structured-streaming.md)

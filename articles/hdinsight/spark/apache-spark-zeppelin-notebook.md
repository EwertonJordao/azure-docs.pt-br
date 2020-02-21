---
title: Zeppelin notebooks & Apache Spark cluster – Azure HDInsight
description: Instruções passo a passo sobre como usar notebooks Zeppelin com clusters Apache Spark no Azure HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 02/18/2020
ms.openlocfilehash: c5c8a41aef92876ceaa66fb23c01c6ece1609f91
ms.sourcegitcommit: 98a5a6765da081e7f294d3cb19c1357d10ca333f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/20/2020
ms.locfileid: "77484801"
---
# <a name="use-apache-zeppelin-notebooks-with-apache-spark-cluster-on-azure-hdinsight"></a>Use os cadernos Apache Zeppelin com o cluster do Apache Spark no HDInsight do Azure

Os clusters do HDInsight Spark incluem os [blocos de anotações do Apache Zeppelin](https://zeppelin.apache.org/) que podem ser usados para executar os trabalhos [do Apache Spark](https://spark.apache.org/). Neste artigo, você aprenderá a usar o notebook Zeppelin em um cluster HDInsight.

## <a name="prerequisites"></a>Prerequisites

* Um cluster do Apache Spark no HDInsight. Para obter instruções, consulte o artigo sobre como [Criar clusters do Apache Spark no Azure HDInsight](apache-spark-jupyter-spark-sql.md).
* O esquema de URI para o armazenamento primário de clusters. Isso seria `wasb://` para o armazenamento de BLOBs do Azure, `abfs://` para Azure Data Lake Storage Gen2 ou `adl://` para Azure Data Lake Storage Gen1. Se a transferência segura estiver habilitada para o armazenamento de BLOB, o URI será `wasbs://`.  Para obter mais informações, consulte [exigir transferência segura no armazenamento do Azure](../../storage/common/storage-require-secure-transfer.md) .

## <a name="launch-an-apache-zeppelin-notebook"></a>Inicie um notebook do Apache Zeppelin

1. Na **visão geral**do cluster do Spark, selecione **Zeppelin Notebook** em **painéis do cluster**. Insira as credenciais de administrador para o cluster.  

   > [!NOTE]  
   > Você também pode acessar o Bloco de Notas Zeppelin de seu cluster abrindo a seguinte URL no navegador. Substitua **CLUSTERNAME** pelo nome do cluster:
   >
   > `https://CLUSTERNAME.azurehdinsight.net/zeppelin`

2. Crie um novo bloco de anotações. No painel de cabeçalho, navegue até o **bloco de anotações** > **criar nova anotação**.

    ![Criar um novo notebook Zeppelin](./media/apache-spark-zeppelin-notebook/hdinsight-create-zeppelin-notebook.png "Criar um novo bloco de anotações do Zeppelin")

    Insira um nome para o bloco de anotações e, em seguida, selecione **criar anotação**.

3. Verifique se o cabeçalho do bloco de anotações mostra um status conectado. Ele é indicado por um ponto verde no canto superior direito.

    ![Status do Zeppelin Notebook](./media/apache-spark-zeppelin-notebook/hdinsight-zeppelin-connected.png "Status do bloco de anotações do Zeppelin")

4. Carregar dados de exemplo em uma tabela temporária. Quando você cria um cluster Spark no HDInsight, o arquivo de dados de exemplo, `hvac.csv`, é copiado para a conta de armazenamento associada em `\HdiSamples\SensorSampleData\hvac`.

    No parágrafo vazio criado por padrão no novo bloco de anotações, cole o snippet a seguir.

    ```scala
    %livy2.spark
    //The above magic instructs Zeppelin to use the Livy Scala interpreter

    // Create an RDD using the default Spark context, sc
    val hvacText = sc.textFile("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

    // Define a schema
    case class Hvac(date: String, time: String, targettemp: Integer, actualtemp: Integer, buildingID: String)

    // Map the values in the .csv file to the schema
    val hvac = hvacText.map(s => s.split(",")).filter(s => s(0) != "Date").map(
        s => Hvac(s(0),
                s(1),
                s(2).toInt,
                s(3).toInt,
                s(6)
        )
    ).toDF()

    // Register as a temporary table called "hvac"
    hvac.registerTempTable("hvac")
    ```

    Pressione **Shift + Enter** ou selecione o botão **reproduzir** para o parágrafo para executar o trecho de código. O status no canto direito do parágrafo deve progredir de PRONTO, PENDENTE, EM EXCECUÇÃO para CONCLUÍDO. A saída é exibida na parte inferior do mesmo parágrafo. A captura de tela é semelhante ao seguinte:

    ![Criar uma tabela temporária com base em dados brutos](./media/apache-spark-zeppelin-notebook/hdinsight-zeppelin-load-data.png "Criar uma tabela temporária por meio de dados brutos")

    Você também pode fornecer um título para cada parágrafo. No canto direito do parágrafo, selecione o ícone de **configurações** (Sprocket) e, em seguida, selecione **Mostrar título**.  

    > [!NOTE]  
    > O intérprete% spark2 não é suportado nos portáteis Zeppelin em todas as versões do HDInsight, e o intérprete% sh não será suportado pelo HDInsight 4.0 em diante.

5. Agora você pode executar instruções SQL do Spark na tabela `hvac`. Cole a seguinte consulta em um novo parágrafo. A consulta recupera a ID do prédio e a diferença entre as temperaturas almejada e real para cada prédio em uma determinada data. Pressione **SHIFT+ENTER**.

    ```sql
    %sql
    select buildingID, (targettemp - actualtemp) as temp_diff, date from hvac where date = "6/1/13"
    ```  

    A instrução **%sql** no início informa ao bloco de anotações para usar o interpretador Scala Livy.

6. Selecione o ícone de **gráfico de barras** para alterar a exibição.  **as configurações**, que aparecem depois que você selecionou **gráfico de barras**, permitem que você escolha **chaves**e **valores**.  A captura de tela a seguir mostra o resultado.

    ![Executar uma instrução SQL do Spark usando o notebook1](./media/apache-spark-zeppelin-notebook/hdinsight-zeppelin-spark-query-1.png "Executar uma instrução SQL do Spark usando o notebook1")

7. Você também pode executar instruções Spark SQL usando variáveis na consulta. O próximo trecho mostra como definir uma variável, `Temp`, na consulta com os possíveis valores com os quais você deseja consultar. Quando você executa a consulta pela primeira vez, uma lista suspensa é preenchida automaticamente com os valores especificados para a variável.

    ```sql
    %sql  
    select buildingID, date, targettemp, (targettemp - actualtemp) as temp_diff from hvac where targettemp > "${Temp = 65,65|75|85}"
    ```

    Cole esse snippet em um novo parágrafo e pressione **SHIFT+ENTER**. Em seguida, selecione **65** na lista suspensa **Temp** .

8. Selecione o ícone de **gráfico de barras** para alterar a exibição.  Em seguida, selecione **configurações** e faça as seguintes alterações:

   * **Grupos:**  Adicione **targettemp**.  
   * **Valores:** uma. Remover **Data**.  2. Adicionar **temp_diff**.  3.  Altere o agregador de **sum** para **AVG**.  

     A captura de tela a seguir mostra o resultado.

     ![Executar uma instrução SQL do Spark usando o notebook2](./media/apache-spark-zeppelin-notebook/hdinsight-zeppelin-spark-query-2.png "Executar uma instrução SQL do Spark usando o notebook2")

## <a name="how-do-i-use-external-packages-with-the-notebook"></a>Como usar pacotes externos com o notebook?

Você pode configurar o notebook Zeppelin no cluster Apache Spark no HDInsight para usar pacotes externos, contribuídos pela Comunidade, que não estão incluídos prontos para uso no cluster. Você pode pesquisar o [Repositório do Maven](https://search.maven.org/) para obter uma lista de pacotes que estão disponíveis. Você também pode obter uma lista de pacotes disponíveis de outras fontes. Por exemplo, uma lista completa dos pacotes enviados pela comunidade está disponível em [Pacotes do Spark](https://spark-packages.org/).

Neste artigo, você verá como usar o pacote [Spark-CSV](https://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) com o notebook Jupyter.

1. Abra as configurações do interpretador. No canto superior direito, selecione o nome de usuário conectado e, em seguida, selecione **intérprete**.

    ![Iniciar interpretador](./media/apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "Saída do Hive")

2. Role até **livy2**e, em seguida, selecione **Editar**.

    ![Alterar dispositivo1 do intérprete](./media/apache-spark-zeppelin-notebook/zeppelin-use-external-package-1.png "Alterar dispositivo1 do intérprete")

3. Navegue até a chave `livy.spark.jars.packages`e defina seu valor no formato `group:id:version`. Dessa forma, se você quiser usar o pacote [spark csv](https://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar), deverá definir o valor da chave como `com.databricks:spark-csv_2.10:1.4.0`.

    ![Alterar settings2 do intérprete](./media/apache-spark-zeppelin-notebook/zeppelin-use-external-package-2.png "Alterar settings2 do intérprete")

    Selecione **salvar** e **OK** para reiniciar o intérprete Livy.

4. se quiser saber como chegar ao valor da chave inserida acima, confira como.

    a. Localize o pacote no Repositório Maven. Para este artigo, usamos o [Spark-CSV](https://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar).

    b. No repositório, colete os valores para **GroupId**, **ArtifactId** e **Version**.

    ![Usar pacotes externos com o Jupyter Notebook](./media/apache-spark-zeppelin-notebook/use-external-packages-with-jupyter.png "Usar pacotes externos com o bloco de notas Jupyter")

    c. Concatene os três valores, separados por dois pontos ( **:** ).

        com.databricks:spark-csv_2.10:1.4.0

## <a name="where-are-the-zeppelin-notebooks-saved"></a>Onde os notebooks Zeppelin são salvos?

Os notebooks Zeppelin são salvos nos nós de cabeçalho do cluster. Portanto, se você excluir o cluster, os notebooks também serão excluídos. Se você quiser preservar os notebooks para uso posterior em outros clusters, deverá exportá-los depois de concluir os trabalhos em execução. Para exportar um bloco de anotações, selecione o ícone **Exportar** , conforme mostrado na imagem abaixo.

![Baixar bloco de anotações](./media/apache-spark-zeppelin-notebook/zeppelin-download-notebook.png "Baixar o bloco de anotações")

Isso salva o notebook como um arquivo JSON em seu local de download.

## <a name="livy-session-management"></a>Gerenciamento de sessões do Livy

Quando você executa o primeiro parágrafo de código no notebook Zeppelin, uma nova sessão do Livy é criada em seu cluster HDInsight Spark. Essa sessão será compartilhada entre todos os notebooks Zeppelin que você criar posteriormente. Se, por algum motivo, a sessão Livy for eliminada (reinicialização do cluster e assim por diante), você não poderá executar trabalhos do notebook Zeppelin.

Nesse caso, você deverá executar as etapas a seguir antes de iniciar a execução de trabalhos de um notebook do Zeppelin.  

1. Reinicie o interpretador Livy no notebook Zeppelin. Para fazer isso, abra as configurações do intérprete selecionando o nome de usuário conectado no canto superior direito e, em seguida, selecione **intérprete**.

    ![Iniciar interpretador](./media/apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "Saída do Hive")

2. Role até **livy2**e selecione **reiniciar**.

    ![Reiniciar o intérprete Livy](./media/apache-spark-zeppelin-notebook/hdinsight-zeppelin-restart-interpreter.png "Reiniciar o intérprete Zeppelin")

3. Execute uma célula de código de um notebook Zeppelin existente. Isso cria uma nova sessão do Livy no cluster HDInsight.

## <a name="general-information"></a>Informações gerais

### <a name="validate-service"></a>Validar serviço

Para validar o serviço do Ambari, navegue até `https://CLUSTERNAME.azurehdinsight.net/#/main/services/ZEPPELIN/summary` em que CLUSTERname é o nome do cluster.

Para validar o serviço de uma linha de comando, use SSH para o nó principal. Alterne o usuário para Zeppelin usando o comando `sudo su zeppelin`. Comandos de status:

|Comando |DESCRIÇÃO |
|---|---|
|`/usr/hdp/current/zeppelin-server/bin/zeppelin-daemon.sh status`|Status do serviço.|
|`/usr/hdp/current/zeppelin-server/bin/zeppelin-daemon.sh --version`|Versão do serviço.|
|`ps -aux | grep zeppelin`|Identifique o PID.|

### <a name="log-locations"></a>Locais de log

|Serviço |Caminho |
|---|---|
|Zeppelin-servidor|/usr/hdp/current/zeppelin-server/|
|Logs do servidor|/var/log/zeppelin|
|Intérprete de configuração, Shiro, site. xml, Log4J|/usr/HDP/Current/Zeppelin-Server/conf ou/etc/Zeppelin/conf|
|Diretório PID|/var/run/zeppelin|

### <a name="enable-debug-logging"></a>Habilitar o log de depuração

1. Navegue até `https://CLUSTERNAME.azurehdinsight.net/#/main/services/ZEPPELIN/summary` em que CLUSTERname é o nome do cluster.

1. Navegue até **configurações** > **avançado Zeppelin-log4j-Properties** > **log4j_properties_content**.

1. Modifique `log4j.appender.dailyfile.Threshold = INFO` para `log4j.appender.dailyfile.Threshold = DEBUG`.

1. Adicione `log4j.logger.org.apache.zeppelin.realm=DEBUG`.

1. Salve as alterações e reinicie o serviço.

## <a name="next-steps"></a>Próximas etapas

[Visão geral: Apache Spark no Azure HDInsight](apache-spark-overview.md)

### <a name="scenarios"></a>Cenários

* [Apache Spark com BI: execute análise de dados interativa usando o Spark no HDInsight com ferramentas de BI](apache-spark-use-bi-tools.md)
* [Apache Spark com Machine Learning: use o Spark no HDInsight para analisar a temperatura do edifício usando dados de HVAC](apache-spark-ipython-notebook-machine-learning.md)
* [Apache Spark com Machine Learning: use o Spark no HDInsight para prever os resultados da inspeção de alimentos](apache-spark-machine-learning-mllib-ipython.md)
* [Análise de log do site usando o Apache Spark no HDInsight](apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Criar e executar aplicativos

* [Criar um aplicativo autônomo usando Scala](apache-spark-create-standalone-application.md)
* [Execute trabalhos remotamente em um cluster do Apache Spark usando o Apache Livy](apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Ferramentas e extensões

* [Use o Plugin do HDInsight Tools para o IntelliJ IDEA criar e enviar os aplicativos Scala do Apache Spark](apache-spark-intellij-tool-plugin.md)
* [Use o Plugin do HDInsight Tools para o IntelliJ IDEA para depurar os aplicativos do Apache Spark remotamente](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Kernels disponíveis para o notebook Jupyter no cluster do Apache Spark para HDInsight](apache-spark-jupyter-notebook-kernels.md)
* [Usar pacotes externos com blocos de notas Jupyter](apache-spark-jupyter-notebook-use-external-packages.md)
* [Instalar o Jupyter em seu computador e conectar-se a um cluster Spark do HDInsight](apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Gerenciar recursos

* [Gerenciar os recursos de cluster do Apache Spark no Azure HDInsight](apache-spark-resource-manager.md)
* [Rastrear e depurar trabalhos em execução em um cluster do Apache Spark no HDInsight](apache-spark-job-debugging.md)

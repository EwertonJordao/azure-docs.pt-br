---
title: 'Tutorial-Azure Toolkit for IntelliJ: aplicativo Spark – HDInsight'
description: Tutorial – Usar o Azure Toolkit for IntelliJ para desenvolver aplicativos Spark escritos em Scala e enviá-los a um cluster Spark do HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: tutorial
ms.date: 09/04/2019
ms.openlocfilehash: 2631a0906a0f0886bdc106f1afef99860a6fe00b
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78381623"
---
# <a name="tutorial-use-azure-toolkit-for-intellij-to-create-apache-spark-applications-for-hdinsight-cluster"></a>Tutorial: usar Azure Toolkit for IntelliJ para criar aplicativos Apache Spark para o cluster HDInsight

Este tutorial demonstra como desenvolver aplicativos Apache Spark no Azure HDInsight usando o plug-in do **Kit de ferramentas do Azure** para o IDE IntelliJ. O [Azure HDInsight](../hdinsight-overview.md) é um serviço de análise de software livre gerenciado na nuvem que permite que você use estruturas de software livre, como Hadoop, Apache Spark, Apache Hive e Apache Kafka.

Você pode usar o plug-in do **Kit de ferramentas do Azure** de algumas maneiras:

* Desenvolva e envie um aplicativo do Spark para um cluster HDInsight Spark.
* Acessar os recursos de cluster Spark do Azure HDInsight.
* Desenvolver e executar um aplicativo Scala Spark localmente.

Neste tutorial, você aprenderá como:
> [!div class="checklist"]
> * Usar o plug-in Azure Toolkit for IntelliJ
> * Desenvolver aplicativos do Apache Spark
> * Enviar um aplicativo para o cluster HDInsight do Azure

## <a name="prerequisites"></a>{1&gt;{2&gt;Pré-requisitos&lt;2}&lt;1}

* Um cluster do Apache Spark no HDInsight. Para obter instruções, consulte o artigo sobre como [Criar clusters do Apache Spark no Azure HDInsight](apache-spark-jupyter-spark-sql.md).

* [Kit de desenvolvimento Oracle Java](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).  Este tutorial usa o Java versão 8.0.202.

* IntelliJ IDEA. Este artigo usa a [INTELLIJ Idea Community ver.  2018.3.4](https://www.jetbrains.com/idea/download/).

* Azure Toolkit for IntelliJ.  Confira [Installing the Azure Toolkit for IntelliJ](https://docs.microsoft.com/java/azure/intellij/azure-toolkit-for-intellij-installation?view=azure-java-stable) (Instalação do Azure Toolkit for IntelliJ).

## <a name="install-scala-plugin-for-intellij-idea"></a>Instalar o plug-in Scala para IntelliJ IDEA

Siga estas etapas para instalar o plug-in Scala:

1. Abra o IntelliJ IDEA.

2. Na tela de boas-vindas, navegue até **Configurar** > **Plug-ins** para abrir a janela **Plug-ins**.

    ![Habilitar o plug-in do Scala no IntelliJ IDEA](./media/apache-spark-intellij-tool-plugin/enable-scala-plugin1.png)

3. Selecione **Instalar** para o plug-in Scala caracterizado na nova janela.  

    ![Instalar o plug-in do Scala no IntelliJ IDEA](./media/apache-spark-intellij-tool-plugin/install-scala-plugin.png)

4. Depois que o plug-in foi instalado com êxito, você deve reiniciar o IDE.

## <a name="create-a-spark-scala-application-for-an-hdinsight-spark-cluster"></a>Criar um aplicativo Scala Spark para um cluster HDInsight Spark

1. Inicie o IntelliJ IDEA e selecione **Criar novo projeto** para abrir a janela **Novo projeto**.

2. Selecione **Azure Spark/HDInsight** no painel esquerdo.

3. Selecione **Projeto Spark (Scala)** na janela principal.

4. Na lista suspensa **Ferramenta de build**, selecione uma das seguintes opções:
   * **Maven** para obter suporte ao assistente de criação de projetos Scala.
   * **SBT** para gerenciar as dependências e para criar no projeto Scala.

     ![A caixa de diálogo Novo Projeto no IntelliJ IDEA](./media/apache-spark-intellij-tool-plugin/create-hdi-scala-app.png)

5. Selecione **Avançar**.

6. Na janela **Novo Projeto**, forneça as seguintes informações:  

    |  Propriedade   | Descrição   |  
    | ----- | ----- |  
    |Nome do projeto| Digite um nome.  Este tutorial usa `myApp`.|  
    |Local do&nbsp;projeto| Insira o local desejado para salvar o projeto.|
    |SDK do projeto| Isso poderá ficar em branco no primeiro uso do IDEA.  Selecione **Novo...** e navegue até o JDK.|
    |Versão do Spark|O assistente de criação integra a versão apropriada para o SDK do Spark e o SDK do Scala. Se a versão do cluster do Spark for anterior à 2.0, selecione **Spark 1.x**. Caso contrário, selecione **Spark2.x**. Esse exemplo usa o **Spark 2.3.0 (Scala 2.11.8)** .|

    ![Selecionar o SDK do Apache Spark](./media/apache-spark-intellij-tool-plugin/intellij-new-project.png)

7. Selecione **Concluir**.  Pode levar alguns minutos antes que o projeto fique disponível.

8. O projeto do Spark cria automaticamente um artefato para você. Para exibir o artefato, faça o seguinte:

   a. Na barra de menus, navegue até **Arquivo** > **Estrutura do projeto...** .

   b. Na janela **Estrutura do Projeto**, selecione **Artefatos**.  

   c. Selecione **Cancelar** depois de exibir o artefato.

      ![Informações do artefato na caixa de diálogo](./media/apache-spark-intellij-tool-plugin/default-artifact-dialog.png)

9. Adicione o código-fonte do aplicativo seguindo estas etapas:

    a. Em Projeto, navegue até **myApp** > **src** > **principal** > **scala**.  

    b. Clique com o botão direito do mouse em **scala** e, em seguida, navegue até **Novo** > **classe Scala**.

   ![Comandos para criar uma classe Scala do Projeto](./media/apache-spark-intellij-tool-plugin/hdi-spark-scala-code.png)

   c. Na caixa de diálogo **Criar Nova Classe do Scala**, forneça um nome, selecione **Objeto** na lista suspensa **Tipo** e selecione **OK**.

     ![Criar caixa de diálogo Nova Classe Scala](./media/apache-spark-intellij-tool-plugin/hdi-spark-scala-code-object.png)

   d. O arquivo **myApp.scala** então se abre na exibição principal. Substitua o código padrão pelo código localizado abaixo:  

        ```scala
        import org.apache.spark.SparkConf
        import org.apache.spark.SparkContext
    
        object myApp{
            def main (arg: Array[String]): Unit = {
            val conf = new SparkConf().setAppName("myApp")
            val sc = new SparkContext(conf)
    
            val rdd = sc.textFile("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")
    
            //find the rows that have only one digit in the seventh column in the CSV file
            val rdd1 =  rdd.filter(s => s.split(",")(6).length() == 1)
    
            rdd1.saveAsTextFile("wasbs:///HVACOut")
            }
    
        }
        ```

    O código lê os dados no HVAC.csv (disponível em todos os clusters Spark do HDInsight), recupera as linhas com apenas um dígito na sétima coluna no arquivo CSV e grava a saída em `/HVACOut` no contêiner padrão de armazenamento do cluster.

## <a name="connect-to-your-hdinsight-cluster"></a>Conectar-se ao cluster HDInsight

O usuário pode [entrar a assinatura do Azure](#sign-in-to-your-azure-subscription) ou [vincular um cluster HDInsight](#link-a-cluster) usando a credencial de usuário/senha ou domínio unido do Ambari para conectar seu cluster do HDInsight.

### <a name="sign-in-to-your-azure-subscription"></a>Entre em sua assinatura do Azure

1. Na barra de menus, navegue até **Exibição** > **Janelas de Ferramentas** > **Azure Explorer**.

   ![IntelliJ IDEA mostrar o Azure Explorer](./media/apache-spark-intellij-tool-plugin/show-azure-explorer1.png)

2. No Azure Explorer, clique com o botão direito do mouse no nó **Azure** e, em seguida, selecione **Entrar**.

   ![IntelliJ IDEA explorer clique com o botão direito do mouse no Azure](./media/apache-spark-intellij-tool-plugin/explorer-rightclick-azure.png)

3. Na caixa de diálogo **Entrar no Azure**, escolha **Logon do Dispositivo** e selecione **Entrar**.

    ![IntelliJ IDEA logon no dispositivo de entrada do Azure](./media/apache-spark-intellij-tool-plugin/intellij-view-explorer2.png)

4. Na caixa de diálogo **Logon no Dispositivo do Azure**, clique em **Copiar e Abrir**.

   ![IntelliJ IDEA logon no dispositivo do Azure](./media/apache-spark-intellij-tool-plugin/intellij-view-explorer5.png)

5. Na interface do navegador, cole o código e clique em **Avançar**.

   ![Microsoft caixa de diálogo inserir código para HDI](./media/apache-spark-intellij-tool-plugin/intellij-view-explorer6.png)

6. Insira suas credenciais do Azure e feche o navegador.

   ![Microsoft caixa de diálogo email para HDI](./media/apache-spark-intellij-tool-plugin/intellij-view-explorer7.png)

7. Depois que você estiver conectado, a caixa de diálogo **Selecionar Assinaturas** listará todas as assinaturas do Azure associadas às credenciais. Selecione sua assinatura e, em seguida, selecione o botão **Selecionar**.

    ![A caixa de diálogo Selecionar Assinaturas](./media/apache-spark-intellij-tool-plugin/Select-Subscriptions.png)

8. No **Azure Explorer**, expanda **HDInsight** para exibir os clusters Spark do HDInsight em suas assinaturas.

    ![IntelliJ IDEA exibição principal do Azure Explorer](./media/apache-spark-intellij-tool-plugin/intellij-view-explorer3.png)

9. Para exibir os recursos (por exemplo, contas de armazenamento) associados ao cluster, você poderá expandir ainda mais um nó de nome de cluster.

    ![Contas de armazenamento do Azure Explorer](./media/apache-spark-intellij-tool-plugin/intellij-view-explorer4.png)

### <a name="link-a-cluster"></a>Vincular um cluster

Você pode vincular um cluster HDInsight usando o nome de usuário gerenciado do Apache Ambari. Da mesma forma, para um cluster HDInsight ingressado no domínio, crie o vínculo usando o domínio e o nome de usuário, como `user1@contoso.com`. Also you can link Livy Service cluster.

1. Na barra de menus, navegue até **Exibição** > **Janelas de Ferramentas** > **Azure Explorer**.

1. No Azure Explorer, clique com o botão direito do mouse em **HDInsight** nó e selecione **Vincular um Cluster**.

   ![Menu de contexto do cluster do link do Azure Explorer](./media/apache-spark-intellij-tool-plugin/link-a-cluster-context-menu.png)

1. As opções disponíveis na janela **Vincular um Cluster** vão variar conforme o valor selecionado na lista suspensa **Tipo de Recurso de Link**.  Insira seus valores e, em seguida, selecione **OK**.

    * **Cluster do HDInsight**  
  
        |Propriedade |{1&gt;Valor&lt;1} |
        |----|----|
        |Tipo de Recurso de Link|Selecione **Cluster do HDInsight** na lista suspensa.|
        |Nome/URL do cluster| Insira o nome do cluster.|
        |Tipo de autenticação| Deixe como **Autenticação Básica**|
        |Nome do Usuário| Insira o nome de usuário do cluster, o padrão é admin.|
        |Senha| Insira a senha do nome de usuário.|

        ![Caixa de diálogo vincular um cluster do IntelliJ IDEA](./media/apache-spark-intellij-tool-plugin/link-hdinsight-cluster-dialog.png)

    * **Serviço Livy**  
  
        |Propriedade |{1&gt;Valor&lt;1} |
        |----|----|
        |Tipo de Recurso de Link|Selecione **Serviço Livy** na lista suspensa.|
        |Ponto de Extremidade do Livy| Inserir o Ponto de Extremidade Livy|
        |Nome do Cluster| Insira o nome do cluster.|
        |Ponto de Extremidade do Yarn|Opcional.|
        |Tipo de autenticação| Deixe como **Autenticação Básica**|
        |Nome do Usuário| Insira o nome de usuário do cluster, o padrão é admin.|
        |Senha| Insira a senha do nome de usuário.|

        ![Caixa de diálogo vincular cluster do Livy do IntelliJ IDEA](./media/apache-spark-intellij-tool-plugin/link-livy-cluster-dialog.png)

1. Você pode ver seu cluster vinculado do nó do **HDInsight**.

   ![Azure Explorer cluster vinculado1](./media/apache-spark-intellij-tool-plugin/hdinsight-linked-cluster.png)

1. Também é possível desvincular um cluster a partir do **Azure Explorer**.

   ![Azure Explorer cluster não vinculado](./media/apache-spark-intellij-tool-plugin/hdi-unlinked-cluster.png)

## <a name="run-a-spark-scala-application-on-an-hdinsight-spark-cluster"></a>Executar um aplicativo Scala Spark em um cluster HDInsight Spark

Depois de criar um aplicativo Scala, você poderá enviá-lo ao cluster.

1. De Projeto, navegue até **myApp** > **src** > **main** > **scala** > **myApp**.  Clique com o botão direito do mouse em **myApp** e selecione **Enviar Aplicativo Spark** (provavelmente estará localizado na parte inferior da lista).

      ![O comando Enviar Aplicativo Spark para HDInsight](./media/apache-spark-intellij-tool-plugin/hdi-submit-spark-app-1.png)

2. Na janela da caixa de diálogo **Enviar aplicativo Spark** , selecione **1. Spark no HDInsight**.

3. Na janela **Editar configuração**, forneça os seguintes valores e, em seguida, selecione **OK**:

    |Propriedade |{1&gt;Valor&lt;1} |
    |----|----|
    |Clusters do Spark (somente Linux)|Selecione o cluster HDInsight Spark no qual você deseja executar o aplicativo.|
    |Selecione um Artefato para enviar|Deixe a configuração padrão.|
    |Nome da classe principal|O valor padrão é a classe principal do arquivo selecionado. Você pode alterar a classe selecionando as reticência ( **...** ) e escolhendo outra classe.|
    |Configurações de trabalho|Você pode alterar os valores de e/ou as chaves padrão. Para obter mais informações, confira [API REST do Apache Livy](https://livy.incubator.apache.org/docs/latest/rest-api.html).|
    |Argumentos de linha de comando|Você pode inserir argumentos separados por espaço para a classe principal se necessário.|
    |Arquivos Referenciados e Jars Referenciados|você pode inserir os caminhos para os Jars e os arquivos referenciados, se houver. Você também pode procurar arquivos no sistema de arquivos virtual do Azure, que atualmente suporta apenas o cluster ADLS Gen 2. Para obter mais informações: [configuração de Apache Spark](https://spark.apache.org/docs/latest/configuration.html#runtime-environment).  Confira também [Como carregar recursos para o cluster](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-storage-explorer).|
    |Armazenamento de Upload de Trabalho|Expanda para revelar opções adicionais.|
    |Tipo de armazenamento|Selecione **Usar Blob do Azure para carregar** na lista suspensa.|
    |Conta de Armazenamento|Insira sua conta de armazenamento.|
    |Chave de armazenamento|Insira sua chave de armazenamento.|
    |Contêiner de armazenamento|Selecione seu contêiner de armazenamento na lista suspensa depois que **Conta de Armazenamento** e **Chave de Armazenamento** tiverem sido inseridas.|

    ![A caixa de diálogo Envio do Spark](./media/apache-spark-intellij-tool-plugin/hdi-submit-spark-app-02.png)

4. Selecione **SparkJobRun** para enviar seu projeto para o cluster selecionado. A guia **Trabalho do Spark Remoto no Cluster** exibe o andamento da execução do trabalho na parte inferior. Você pode parar o aplicativo clicando no botão vermelho. Para saber como acessar a saída do trabalho, confira a seção "Acessar e gerenciar clusters Spark do HDInsight usando o Kit de Ferramentas do Azure para IntelliJ" mais adiante neste artigo.  

    ![A janela Envio do Apache Spark](./media/apache-spark-intellij-tool-plugin/hdi-spark-app-result.png)

## <a name="debug-apache-spark-applications-locally-or-remotely-on-an-hdinsight-cluster"></a>Depurar aplicativos do Apache Spark local ou remotamente em um cluster do HDInsight

Também recomendamos outra forma de enviar o aplicativo Spark ao cluster. Você pode fazer isso definido os parâmetros no IDE **Executar/Depurar configurações**. Para obter mais informações, consulte [Depurar aplicativos do Apache Spark local ou remotamente em um cluster do HDInsight com o Azure Toolkit for IntelliJ por meio do SSH](apache-spark-intellij-tool-debug-remotely-through-ssh.md).

## <a name="access-and-manage-hdinsight-spark-clusters-by-using-azure-toolkit-for-intellij"></a>Acessar e gerenciar clusters Spark do HDInsight usando o Kit de Ferramentas do Azure para IntelliJ

Você pode executar várias operações usando o Kit de Ferramentas do Azure para IntelliJ.  A maioria das operações é iniciada pelo **Azure Explorer**.  Na barra de menus, navegue até **Exibição** > **Janelas de Ferramentas** > **Azure Explorer**.

### <a name="access-the-job-view"></a>Acessar a exibição do trabalho

1. No Azure Explorer, navegue até **HDInsight** > \<Seu Cluster> > **Trabalhos**.

    ![IntelliJ nó de exibição de trabalho do Azure Explorer](./media/apache-spark-intellij-tool-plugin/intellij-job-view-node.png)

2. No painel direito, a guia **Exibição de Trabalho do Spark** exibe todos os aplicativos que foram executados no cluster. Selecione o nome do aplicativo do qual você deseja ver mais detalhes.

    ![Spark detalhes do aplicativo de exibição de trabalho](./media/apache-spark-intellij-tool-plugin/intellij-view-job-logs.png)

3. Para exibir informações básicas do trabalho em execução, passe o mouse sobre o grafo de trabalhos. Para exibir o grafo de estágios e as informações geradas pelos trabalhos, escolha um nó no grafo de trabalhos.

    ![Detalhes de preparação de trabalhos da exibição de trabalho do Spark](./media/apache-spark-intellij-tool-plugin/Job-graph-stage-info.png)

4. Para exibir logs usados frequentemente, como *Stderr do Driver*, *Stdout do Driver* e *Informações do diretório*, escolha a guia **Log**.

    ![Spark detalhes do Log de Exibição de Trabalho](./media/apache-spark-intellij-tool-plugin/intellij-job-log-info.png)

5. Você também pode exibir a interface do usuário de histórico do Spark e a interface do usuário do YARN (no nível do aplicativo) selecionando um link na parte superior da janela.

### <a name="access-the-spark-history-server"></a>Acessar o servidor de histórico do Spark

1. No Azure Explorer, expanda o **HDInsight**, clique com o botão direito do mouse no nome do cluster Spark e selecione **Abrir interface do usuário do Histórico Spark**.  
2. Quando for solicitado, insira as credenciais do administrador do cluster, que você especificou ao configurar o cluster.

3. No painel do servidor de histórico do Spark, é possível usar o nome do aplicativo para procurar o que você acabou de executar. No código anterior, você definiu o nome do aplicativo usando `val conf = new SparkConf().setAppName("myApp")`. Portanto, o nome do aplicativo Spark é **myApp**.

### <a name="start-the-ambari-portal"></a>Iniciar o portal do Ambari

1. No Azure Explorer, expanda **HDInsight**, clique com o botão direito do mouse no nome do cluster Spark e selecione **Abrir Portal de Gerenciamento do Cluster (Ambari)** .  

2. Quando solicitado, insira as credenciais de administrador para o cluster. Você especificou essas credenciais durante o processo de configuração do cluster.

### <a name="manage-azure-subscriptions"></a>Gerenciar assinaturas do Azure

Por padrão, o Kit de Ferramentas do Azure para IntelliJ listam os clusters Spark de todas as suas assinaturas do Azure. Se for necessário, você poderá especificar as assinaturas que deseja acessar.  

1. No Azure Explorer, clique com o botão direito do mouse no nó-raiz **Azure** e selecione **Selecionar Assinaturas**.  

2. Na janela **Selecionar Assinaturas**, desmarque as caixas de seleção ao lado das assinaturas que você não deseja acessar e, em seguida, selecione **Fechar**.

## <a name="spark-console"></a>Console do Spark

Você pode executar o Console Local do Spark (Scala) ou o Console de Sessão Interativa do Spark Livy (Scala).

### <a name="spark-local-consolescala"></a>Console Local do Spark (Scala)

Certifique-se de que você satisfez o pré-requisito do WINUTILS.EXE.

1. Na barra de menus, navegue até **Executar** > **Editar Configurações...** .

2. Na janela **Executar/depurar configurações**, no painel esquerdo, navegue até **Apache Spark no HDInsight** >  **[Spark no HDInsight] myApp**.

3. Na janela principal, selecione a guia **Executar Localmente**.

4. Forneça os seguintes valores e, em seguida, selecione **OK**:

    |Propriedade |{1&gt;Valor&lt;1} |
    |----|----|
    |Classe principal do trabalho|O valor padrão é a classe principal do arquivo selecionado. Você pode alterar a classe selecionando as reticência ( **...** ) e escolhendo outra classe.|
    |Variáveis de ambiente|Verifique se o valor de HADOOP_HOME está correto.|
    |Localização de WINUTILS.exe|Verifique se o caminho está correto.|

    ![Configuração do conjunto de console local](./media/apache-spark-intellij-tool-plugin/console-set-configuration.png)

5. De Projeto, navegue até **myApp** > **src** > **main** > **scala** > **myApp**.  

6. Na barra de menus, navegue até **Ferramentas** > **Console do Spark** > **Executar Console Local do Spark (Scala)** .

7. Em seguida, duas caixas de diálogo poderão ser exibidas para perguntar se você deseja corrigir automaticamente as dependências. Em caso afirmativo, selecione **Autocorreção**.

    ![IntelliJ IDEA Spark caixa de diálogo correção automática1](./media/apache-spark-intellij-tool-plugin/intellij-console-autofix1.png)

    ![IntelliJ IDEA Spark caixa de diálogo correção automática2](./media/apache-spark-intellij-tool-plugin/intellij-console-autofix2.png)

8. O console deve ser semelhante à imagem abaixo. Na janela do console, digite `sc.appName` e pressione ENTER.  O resultado será exibido. Você pode encerrar o console local clicando no botão vermelho.

    ![IntelliJ IDEA resultado do console local](./media/apache-spark-intellij-tool-plugin/local-console-result.png)

### <a name="spark-livy-interactive-session-consolescala"></a>Console de Sessão Interativa do Spark Livy (Scala)

1. Na barra de menus, navegue até **Executar** > **Editar Configurações...** .

2. Na janela **Executar/depurar configurações**, no painel esquerdo, navegue até **Apache Spark no HDInsight** >  **[Spark no HDInsight] myApp**.

3. Na janela principal, selecione a guia **Executar Remotamente no Cluster**.

4. Forneça os seguintes valores e, em seguida, selecione **OK**:

    |Propriedade |{1&gt;Valor&lt;1} |
    |----|----|
    |Clusters do Spark (somente Linux)|Selecione o cluster HDInsight Spark no qual você deseja executar o aplicativo.|
    |Nome da classe principal|O valor padrão é a classe principal do arquivo selecionado. Você pode alterar a classe selecionando as reticência ( **...** ) e escolhendo outra classe.|

    ![Configuração do conjunto de console interativo](./media/apache-spark-intellij-tool-plugin/interactive-console-configuration.png)

5. De Projeto, navegue até **myApp** > **src** > **main** > **scala** > **myApp**.  

6. Na barra de menus, navegue até **Ferramentas** > **Console do Spark** > **Executar Console de Sessão Interativa do Spark Livy (Scala)** .

7. O console deve ser semelhante à imagem abaixo. Na janela do console, digite `sc.appName` e pressione ENTER.  O resultado será exibido. Você pode encerrar o console local clicando no botão vermelho.

    ![IntelliJ IDEA resultado do Console Interativo](./media/apache-spark-intellij-tool-plugin/interactive-console-result.png)

### <a name="send-selection-to-spark-console"></a>Enviar seleção para o Console do Spark

É conveniente para você prever o resultado do script enviando algum código para o console local ou Console (Scala) de sessão interativa Livy. Você pode realçar código no arquivo do Scala e, em seguida, clicar com o botão direito do mouse em **Enviar Seleção ao Console do Spark**. O código selecionado será enviado para o console e será executado. O resultado será exibido após o código no console. O console verificará erros, se houver.  

   ![Enviar seleção para o Console do Spark](./media/apache-spark-intellij-tool-plugin/send-selection-to-console.png)

## <a name="integrate-with-hdinsight-identity-broker-hib"></a>Integrar com o (HIB) Agente de Identidade do HDInsight 

### <a name="connect-to-your-hdinsight-esp-cluster-with-id-broker-hib"></a>Conectar-se ao cluster do HDInsight ESP com o HIB (Agente de IDs)

Você pode seguir as etapas normais para entrar na assinatura do Azure para se conectar a seu cluster do HDInsight ESP com o HIB (Agente de IDs). Depois de entrar, você verá a lista de clusters no Azure Explorer. Para obter mais instruções, confira [Conectar-se a seu cluster do HDInsight](#connect-to-your-hdinsight-cluster).

### <a name="run-a-spark-scala-application-on-an-hdinsight-esp-cluster-with-id-broker-hib"></a>Executar um aplicativo Spark Scala em um cluster do HDInsight com HIB (Agente de IDs)

Você pode seguir as etapas normais para enviar o trabalho ao cluster do HDInsight ESP com o HIB (Agente de IDs). Veja [Executar um aplicativo do Spark Scala em um cluster do HDInsight Spark](#run-a-spark-scala-application-on-an-hdinsight-spark-cluster) para obter mais instruções.

Fazemos upload dos arquivos necessários em uma pasta nomeada com sua conta de entrada e você pode ver o caminho de upload no arquivo de configuração.

   ![fazer upload do caminho na configuração](./media/apache-spark-intellij-tool-plugin/upload-path-in-the-configuration.png)

### <a name="spark-console-on-an-hdinsight-esp-cluster-with-id-broker-hib"></a>Console do Spark em um cluster do HDInsight ESP com o HIB (Agente de IDs)

Você pode executar o Console Local do Spark (Scala) ou executar o Console de Sessão Interativa do Spark Livy (Scala) em um cluster do HDInsight ESP com o HIB (Agente de IDs). Veja o [Console do Spark](#spark-console) para obter mais instruções.

   > [!NOTE]  
   > No momento, o cluster do HDInsight ESP com o HIB (Agente de IDs) não é compatível com a [vinculação de um cluster](#link-a-cluster) e a [depuração de aplicativos do Apache Spark remotamente](#debug-apache-spark-applications-locally-or-remotely-on-an-hdinsight-cluster).

## <a name="reader-only-role"></a>Função somente leitura

Quando os usuários enviam um trabalho a um cluster com permissão de função somente leitura, as credenciais do Ambari são solicitadas.

### <a name="link-cluster-from-context-menu"></a>Vincular cluster do menu de contexto

1. Entrar com conta com função somente leitura.

2. No **Azure Explorer**, expanda **HDInsight** para exibir os clusters do HDInsight em sua assinatura. Os clusters marcados **"Role:Reader"** só tem permissão para a função somente leitura.

    ![IntelliJ Azure Explorer Role:Reader](./media/apache-spark-intellij-tool-plugin/intellij-view-explorer15.png)

3. Clique com botão direito do mouse no cluster com permissão para a função somente leitura. Selecione **Vincular este cluster** no menu de contexto para vincular o cluster. Insira o nome de usuário e a senha do Ambari.

    ![IntelliJ Azure Explorer vincular este cluster](./media/apache-spark-intellij-tool-plugin/intellij-view-explorer11.png)

4. Se o cluster for vinculado com êxito, o HDInsight será atualizado.
   O estágio do cluster será se tornará vinculado.
  
    ![IntelliJ Azure Explorer caixa de diálogo vinculada](./media/apache-spark-intellij-tool-plugin/intellij-view-explorer8.png)

### <a name="link-cluster-by-expanding-jobs-node"></a>Vincular cluster expandindo o nó Trabalhos

1. Clique no nó **Trabalhos**, a janela **Acesso negado ao cluster Trabalhos** é exibida.

2. Clique em **Vincular este cluster** para vincular o cluster.

    ![caixa de diálogo de acesso negado ao cluster trabalhos](./media/apache-spark-intellij-tool-plugin/intellij-view-explorer9.png)

### <a name="link-cluster-from-rundebug-configurations-window"></a>Vincular cluster da janela Configurações de execução/depuração

1. Crie uma configuração do HDInsight. Depois selecione **Executar Remotamente em Cluster**.

2. Selecione um cluster, que tenha a permissão de função somente leitura, para **Clusters do Spark (somente Linux)** . Mensagem de aviso mostrada. Você pode clicar em **vincular este cluster** para vincular o cluster.

   ![IntelliJ IDEA executar/depurar criação de configuração](./media/apache-spark-intellij-tool-plugin/create-configuration.png)

### <a name="view-storage-accounts"></a>Exibir Contas de Armazenamento

* Para clusters com permissão de função somente leitura, clique no nó **Contas de Armazenamento**, a janela **Acesso Negado ao Armazenamento** aparece. Você pode clicar em **Abrir Gerenciador de Armazenamento do Azure** para abrir o Gerenciador de Armazenamento.

   ![IntelliJ IDEA Acesso de Armazenamento Negado](./media/apache-spark-intellij-tool-plugin/intellij-view-explorer14.png)

   ![IntelliJ IDEA botão Acesso de Armazenamento Negado](./media/apache-spark-intellij-tool-plugin/intellij-view-explorer10.png)

* Para clusters vinculados, clique no nó **Contas de Armazenamento**, a janela **Acesso Negado ao Armazenamento** aparece. Você pode clicar em **Abrir Armazenamento do Azure** para abrir o Gerenciador de Armazenamento.

   ![IntelliJ IDEA Acesso de Armazenamento Negado2](./media/apache-spark-intellij-tool-plugin/intellij-view-explorer13.png)

   ![IntelliJ IDEA botão Acesso de Armazenamento Negado2](./media/apache-spark-intellij-tool-plugin/intellij-view-explorer12.png)

## <a name="convert-existing-intellij-idea-applications-to-use-azure-toolkit-for-intellij"></a>Converter aplicativos IntelliJ IDEA existentes a fim de usar o Kit de Ferramentas do Azure para IntelliJ

Você pode converter os aplicativos Scala Spark existentes criados no IDEA do IntelliJ para serem compatíveis com o Kit de Ferramentas do Azure para IntelliJ. Em seguida, você pode usar o plug-in para enviar os aplicativos a um cluster Spark do HDInsight.

1. Para um aplicativo Scala Spark existente criado no IDEA do IntelliJ, abra o arquivo .iml associado.

2. Há um elemento **module** no nível de raiz, como o seguinte:

        ```
        <module org.jetbrains.idea.maven.project.MavenProjectsManager.isMavenModule="true" type="JAVA_MODULE" version="4">
        ```

   Edite o elemento para adicionar `UniqueKey="HDInsightTool"` , de modo que o elemento **module** seja semelhante ao seguinte:

        ```
        <module org.jetbrains.idea.maven.project.MavenProjectsManager.isMavenModule="true" type="JAVA_MODULE" version="4" UniqueKey="HDInsightTool">
        ```

3. Salve as alterações. Seu aplicativo deve ser compatível com o Kit de Ferramentas do Azure para IntelliJ. Você pode testar isso clicando com o botão direito do mouse no nome do projeto no Projeto. Agora, o menu pop-up tem a opção **Enviar Aplicativo Spark ao HDInsight**.

## <a name="clean-up-resources"></a>Limpar os recursos

Se não for continuar a usar este aplicativo, exclua o cluster que criou seguindo estas etapas:

1. Entre no [portal do Azure](https://portal.azure.com/).

1. Na caixa **Pesquisar** na parte superior, digite **HDInsight**.

1. Selecione **Clusters do HDInsight** em **Serviços**.

1. Na lista de clusters do HDInsight exibida, selecione **…** ao lado do cluster que você criou para este tutorial.

1. Selecione **Excluir**. Selecione **Sim**.

![Excluir cluster HDInsight no portal do Azure](./media/apache-spark-intellij-tool-plugin/hdinsight-azure-portal-delete-cluster.png "Excluir cluster HDInsight")

## <a name="next-steps"></a>{1&gt;{2&gt;Próximas etapas&lt;2}&lt;1}

Neste tutorial, você aprendeu como usar o plug-in do Azure Toolkit for IntelliJ para desenvolver aplicativos do Apache Spark escritos em [Scala](https://www.scala-lang.org/) e depois os enviou diretamente do ambiente de desenvolvimento integrado IntelliJ (IDE) para um cluster do HDInsight Spark. Avance para o próximo artigo para ver como os dados que você registrou no Apache Spark podem ser removidos em uma ferramenta de análise de BI, assim como Power BI.

> [!div class="nextstepaction"]
> [Analisar dados de Apache Spark usando Power BI](apache-spark-use-bi-tools.md)

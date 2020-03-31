---
title: Hue com Hadoop em clusters HDInsight baseados em Linux – Azure
description: Saiba como instalar o Hue em clusters do HDInsight e usar o túnel para rotear as solicitações para a Matiz. Use o Hue para procurar armazenamento e executar o Hive ou o Pig.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive,hdiseo17may2017
ms.date: 11/28/2019
ms.openlocfilehash: 69acfd4f2edab9be1b1dcfbb52eafbd00aec712f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75934575"
---
# <a name="install-and-use-hue-on-hdinsight-hadoop-clusters"></a>Instalar e usar o Hue em clusters de Hadoop do HDInsight

Saiba como instalar o Hue em clusters do HDInsight e usar o túnel para rotear as solicitações para a Matiz.

## <a name="what-is-hue"></a>O que é o Hue?

Matiz é um conjunto de aplicativos Web usados para interagir com um cluster Apache Hadoop. Você pode usar o Hue para procurar o armazenamento associado a um cluster de Hadoop (WASB, no caso de clusters HDInsight), executar trabalhos de Hive e scripts do Pig, etc. Os componentes a seguir são disponibilizados com as instalações do Hue em um cluster Hadoop do HDInsight.

* Editor de Hive Beeswax
* Apache Pig
* Gerenciador do Metastore
* Apache Oozie
* Navegador de Arquivos (que dialoga com o contêiner padrão WASB)
* Navegador de Trabalhos

> [!WARNING]  
> Há suporte total a componentes fornecidos com o cluster HDInsight e o Suporte da Microsoft ajudará a isolar e resolver problemas relacionados a esses componentes.
>
> Componentes personalizados recebem suporte comercialmente razoável para ajudá-lo a solucionar o problema. Isso pode resultar na resolução do problema ou na solicitação de você buscar nos canais disponíveis as tecnologias de código-fonte aberto, onde é possível encontrar conhecimento aprofundado sobre essa tecnologia. Por exemplo, existem muitos sites comunitários que podem ser usados, [https://stackoverflow.com](https://stackoverflow.com)como: [fórum MSDN para HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), . Também os projetos Apache [https://apache.org](https://apache.org)têm sites de projetos em , por exemplo: [Hadoop](https://hadoop.apache.org/).

## <a name="install-hue-using-script-actions"></a>Instalar o Hue usando Ações de Script

Use as informações na tabela abaixo para sua Ação de Script. Consulte [Personalizar clusters HDInsight com ações de script](hdinsight-hadoop-customize-cluster-linux.md) para obter instruções específicas sobre o uso de Ações de Script.

> [!NOTE]  
> Para instalar o Hue em clusters HDInsight, o tamanho do nó de cabeçalho recomendado é de, pelo menos, A4 (8 núcleos, memória de 14 GB).

|Propriedade |Valor |
|---|---|
|Tipo de script:|- Personalizado|
|Nome|Instalar o Hue|
|URI do script Bash|`https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh`|
|Tipos de nó:|Head|

## <a name="use-hue-with-hdinsight-clusters"></a>Usar o Hue com clusters do HDInsight

O túnel SSH é a única maneira de acessar o Hue no cluster a partir do momento em que ele está em execução. O túnel via SSH permite que o tráfego vá diretamente para o nó de cabeçalho do cluster no qual o Hue está sendo executado. Depois que o cluster terminar o provisionamento, use as seguintes etapas para usar o Hue em um cluster HDInsight.

> [!NOTE]  
> É recomendável usar o navegador Web Firefox para seguir as instruções abaixo.

1. Use as informações em [Usar túnel SSH para acessar a interface do usuário da Web do Apache Ambari, ResourceManager, JobHistory, NameNode, Oozie e outras interfaces do usuário da Web](hdinsight-linux-ambari-ssh-tunnel.md) para criar um túnel SSH a partir do sistema de cliente para o cluster HDInsight, e em seguida, configurar seu navegador da Web para usar o túnel como um proxy.

1. Use [o comando ssh](./hdinsight-hadoop-linux-use-ssh-unix.md) para se conectar ao seu cluster. Edite o comando abaixo substituindo CLUSTERNAME pelo nome do seu cluster e, em seguida, digite o comando:

    ```cmd
    ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
    ```

1. Após a conexão, use o seguinte comando para obter o nome de domínio totalmente qualificado do nó de cabeçalho primário:

    ```bash
    hostname -f
    ```

    Isso retornará um nome semelhante ao seguinte:

        myhdi-nfebtpfdv1nubcidphpap2eq2b.ex.internal.cloudapp.net

    Esse é o nome do host do nó de cabeçalho primário onde o site da Hue está localizado.

1. Use o navegador para abrir o Portal do Hue em `http://HOSTNAME:8888`. Substitua HOSTNAME pelo nome obtido na etapa anterior.

   > [!NOTE]  
   > Ao fazer logon pela primeira vez, será solicitado que você crie uma conta para poder fazer logon no portal do Hue. As credenciais que você especificar aqui serão limitadas ao portal e não serão relacionadas às credenciais de usuário SSH ou de administrador que você especificou durante o provisionamento do cluster.

    ![Janela de login do portal de matiz HDInsight](./media/hdinsight-hadoop-hue-linux/hdinsight-hue-portal-login.png "Especificar credenciais para o portal Hue")

### <a name="run-a-hive-query"></a>Executar um trabalho do Hive

1. No portal Hue, selecione **Editores de consulta**e selecione **Hive** para abrir o editor da Colmeia.

    ![Portal de matiz HDInsight usa editor de colmeia](./media/hdinsight-hadoop-hue-linux/hdinsight-hue-portal-use-hive.png "Usar o Hive")

2. Na guia **Ajuda**, em **Banco de dados**, você deverá ver **hivesampletable**. Essa é uma tabela de exemplo que é enviada juntamente com todos os clusters de Hadoop no HDInsight. Insira uma consulta de exemplo no painel direito e veja a saída na guia **Resultados** no painel abaixo, como mostrado na captura de tela.

    ![Consulta da colmeia do portal HDInsight](./media/hdinsight-hadoop-hue-linux/hdinsight-hue-portal-hive-query.png "Executar consulta Hive")

    Você também pode usar a guia **Gráfico** para ver uma representação visual do resultado.

### <a name="browse-the-cluster-storage"></a>Procurar no armazenamento de cluster

1. No portal Hue, selecione **File Browser** no canto superior direito da barra de menu.
2. Por padrão, o navegador de arquivos é aberto no diretório **/user/myuser** . Selecione a barra para a frente antes do diretório do usuário no caminho para ir até a raiz do contêiner de armazenamento Azure associado ao cluster.

    ![Navegador de arquivos do portal de matiz HDInsight](./media/hdinsight-hadoop-hue-linux/hdinsight-hue-portal-file-browser.png "Usar navegador de arquivos")

3. Clique com o botão direito do mouse em um arquivo ou pasta para ver as operações disponíveis. Use o botão **Carregar** no canto superior direito para carregar arquivos no diretório atual. Use o botão **Novo** para criar novos arquivos ou diretórios.

> [!NOTE]  
> O navegador de arquivos do Hue só pode mostrar o conteúdo do contêiner padrão associado ao cluster do HDInsight. Quaisquer contêineres/contas de armazenamento adicionais associados ao cluster não poderão ser acessados usando o navegador de arquivos. No entanto, os contêineres adicionais associados ao cluster sempre estarão acessíveis para os trabalhos do Hive. Por exemplo, ao digitar o comando `dfs -ls wasbs://newcontainer@mystore.blob.core.windows.net` no editor do Hive, você poderá ver também o conteúdo de contêineres adicionais. Neste comando, **newcontainer** não é o contêiner padrão associado a um cluster.

## <a name="important-considerations"></a>Considerações importantes

1. O script usado para instalar o Hue instala-o apenas no nó de cabeçalho primário do cluster.

1. Durante a instalação, vários serviços do Hadoop (HDFS, YARN, MR2, Oozie) são reiniciados para atualizar a configuração. Depois que o script termina de instalar o Hue, pode levar algum tempo para que outros serviços do Hadoop sejam iniciados. Isso pode, inicialmente, afetar o desempenho do Hue. Depois que todos os serviços tiverem sido iniciados, o Hue estará totalmente funcional.

1. A matiz não reconhece os trabalhos do Apache Tez, que é o padrão atual do Hive. Se você quiser usar o MapReduce como o mecanismo de execução do Hive, atualize o script para usar o comando a seguir em seu script:

        set hive.execution.engine=mr;

1. Com clusters do Linux, você pode ter um cenário no qual os serviços estão em execução no nó de cabeçalho primário enquanto o Gerenciador de Recursos pode estar em execução no secundário. Um cenário como esse pode resultar em erros (mostrados abaixo) ao usar o Hue para exibir detalhes de trabalhos EM EXECUÇÃO no cluster. No entanto, você pode exibir os detalhes do trabalho após ele ser concluído.

   ![Mensagem de amostra de erro do portal Hue](./media/hdinsight-hadoop-hue-linux/hdinsight-hue-portal-error.png "Erro no portal do Hue")

   Isso ocorre devido a um problema conhecido. Como solução alternativa, modifique o Ambari para que o Gerenciador de Recursos ativo também seja executado no nó de cabeçalho primário.

1. O Hue entende o WebHDFS, enquanto os clusters HDInsight utilizam o Armazenamento do Azure com o `wasbs://`. Portanto, o script personalizado utilizado com a ação de script instala WebWasb, que é um serviço compatível com WebHDFS para conversar com o WASB. Portanto, embora em alguns lugares o portal do Hue esteja marcado como HDFS (como quando você move o mouse sobre o **Navegador de Arquivos**), ele deve ser interpretado como WASB.

## <a name="next-steps"></a>Próximas etapas

[Instalar o R em clusters HDInsight](hdinsight-hadoop-r-scripts-linux.md). Use a personalização do cluster para instalar o R em clusters de Hadoop do HDInsight. R é uma linguagem e ambiente de software livre para computação estatística. Ele fornece centenas de funções estatísticas internas e sua própria linguagem de programação, que combina aspectos de programação funcional e de programação orientada a objetos. Ele também fornece recursos abrangentes de grafos.

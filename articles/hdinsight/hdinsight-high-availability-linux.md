---
title: Alta disponibilidade para Hadoop – Azure HDInsight
description: Saiba como clusters HDInsight melhoram a confiabilidade e a disponibilidade usando um nó principal adicional. Saiba como isso afeta os serviços do Hadoop, como o Ambari e o Hive, e também como se conectar individualmente com cada nó principal usando SSH.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
keywords: alta disponibilidade hadoop
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 10/28/2019
ms.openlocfilehash: 8c3e377faef4e18bff01fd7001751d1f1e347b8d
ms.sourcegitcommit: f0f73c51441aeb04a5c21a6e3205b7f520f8b0e1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/05/2020
ms.locfileid: "77030856"
---
# <a name="availability-and-reliability-of-apache-hadoop-clusters-in-hdinsight"></a>Disponibilidade e confiabilidade dos clusters Apache Hadoop em HDInsight

Os clusters HDInsight fornecem dois nós de cabeçalho para aumentar a disponibilidade e a confiabilidade dos serviços e trabalhos do Apache Hadoop em execução.

O Hadoop atinge a alta disponibilidade e confiabilidade replicando serviços e dados em múltiplos nós em um cluster. No entanto, em geral, as distribuições padrão do Hadoop têm apenas um único nó de cabeçalho. Qualquer falha do único nó de cabeçalho poderá fazer com que o cluster pare de funcionar. O HDInsight fornece dois nós principais para melhorar a disponibilidade e a confiabilidade do Hadoop.

## <a name="availability-and-reliability-of-nodes"></a>Disponibilidade e confiabilidade dos nós

Os nós em um cluster HDInsight são implementados com o uso de Máquinas Virtuais do Azure. As seções a seguir abordam os tipos de nós individuais usados com o HDInsight.

> [!NOTE]  
> Nem todos os tipos de nó são usados para um tipo de cluster. Por exemplo, um tipo de cluster Hadoop não tem nenhum nó Nimbus. Para obter mais informações sobre os nós usados pelos tipos de cluster HDInsight, veja a seção “Tipos de cluster” do documento [Criar clusters Hadoop baseados em Linux no HDInsight](hdinsight-hadoop-provision-linux-clusters.md#cluster-types).

### <a name="head-nodes"></a>Nós de cabeçalho

Para garantir a alta disponibilidade dos serviços do Hadoop, o HDInsight oferece dois nós principais. Ambos os nós de cabeçalho estão ativos e em execução no cluster HDInsight simultaneamente. Alguns serviços, como Apache HDFS ou Apache Hadoop YARN, só estão “ativos” em um nó de cabeçalho a qualquer momento. Outros serviços, como HiveServer2 ou MetaStore Hive estão ativos em ambos os nós de cabeçalho ao mesmo tempo.

Para obter os nomes de host para diferentes tipos de nó em seu cluster, use a [API REST do amAmbari](hdinsight-hadoop-manage-ambari-rest-api.md#example-get-the-fqdn-of-cluster-nodes).

> [!IMPORTANT]  
> Não associa o valor numérico com a possibilidade de um nó ser primário ou secundário. O valor numérico está presente apenas para fornecer um nome exclusivo para cada nó.

### <a name="nimbus-nodes"></a>Nós Nimbus

Nós Nimbus estão disponíveis nos clusters Apache Storm. Os nós Nimbus fornecem funcionalidade semelhante ao JobTracker do Hadoop, distribuindo e monitorando o processamento nos nós de trabalho. O HDInsight oferece dois nós Nimbus para clusters Storm

### <a name="apache-zookeeper-nodes"></a>Nós do Apache ZooKeeper 3.4.6

Os nós [ZooKeeper](https://zookeeper.apache.org/) são usados para eleição de líder de serviços mestres em nós principais. Eles também são usados para garantir que os serviços, os nós de dados (trabalho) e os gateways saibam em qual nó de cabeçalho um serviço mestre está ativo. Por padrão, o HDInsight fornece três nós do ZooKeeper.

### <a name="worker-nodes"></a>Nós de trabalho

Os nós de trabalho executam a análise de dados real quando um trabalho é enviado para o cluster. Se um nó de trabalho falhar, a tarefa que ele estava executando será enviada para outro nó de trabalho. Por padrão, o HDInsight cria quatro nós de trabalho. Você pode alterar esse número para atender às suas necessidades, durante e após a criação do cluster.

### <a name="edge-node"></a>nó de borda

Um nó de borda não participa ativamente da análise de dados no cluster. Ele é usado por desenvolvedores ou cientistas de dados ao trabalhar com o Hadoop. O nó de borda reside na mesma Rede Virtual do Azure que os outros nós no cluster e pode acessar diretamente todos os outros nós. O nó de borda pode ser usado sem retirar recursos dos serviços críticos do Hadoop ou de trabalhos de análise.

Atualmente, o ML Services no HDInsight é o único tipo de cluster que fornece um nó de borda por padrão. Para o ML Services no HDInsight, o nó de borda é usado para testar o código R localmente no nó antes de enviá-lo ao cluster para o processamento distribuído.

Para obter informações sobre como usar um nó de borda com outros tipos de cluster, consulte o documento [Usar nós de borda no HDInsight](hdinsight-apps-use-edge-node.md).

## <a name="accessing-the-nodes"></a>Acessando os nós

O acesso ao cluster pela internet é fornecido por meio de um gateway público. O acesso é limitado à conexão com os nós de cabeçalho e, se houver, o nó de borda. O acesso aos serviços em execução nos nós de cabeçalho não é afetado por ter vários nós de cabeçalho. O gateway público encaminha solicitações ao nó principal que hospeda o serviço solicitado. Por exemplo, se, no momento, o Apache Ambari estiver hospedado no nó de cabeçalho secundário, o gateway encaminhará solicitações de entrada para o Ambari nesse nó.

O acesso ao gateway público é limitado às portas 443 (HTTPS), 22 e 23.

|Porta |Descrição |
|---|---|
|443|Usado para acessar o Ambari e outra interface do usuário da Web ou APIs REST hospedadas nos nós de cabeçalho.|
|22|Usado para acessar o nó principal ou o nó de borda primário com SSH.|
|23|Usado para acessar o nó principal secundário com SSH. Por exemplo, o `ssh username@mycluster-ssh.azurehdinsight.net` se conectará ao nó principal de cabeçalho do cluster chamado **mycluster**.|

Para saber mais sobre como usar SSH, consulte o documento [Usar SSH com HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

### <a name="internal-fully-qualified-domain-names-fqdn"></a>Nomes internos de domínio totalmente qualificado (FQDN)

Os nós em um cluster HDInsight possuem um endereço IP interno e o FQDN que só pode ser acessado do cluster. Ao acessar serviços em cluster usando o endereço IP ou FQDN interno, você deve usar Ambari para verificar o IP ou FQDN para usar quando acessar o serviço.

Por exemplo, o serviço de Apache Oozie pode ser executado somente em um nó de cabeçalho e usar o comando `oozie` de uma sessão SSH requer a URL para o serviço. Essa URL pode ser recuperada do Ambari usando o seguinte comando:

```bash
export password='PASSWORD'
export clusterName="CLUSTERNAME"

curl -u admin:$password "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations?type=oozie-site&tag=TOPOLOGY_RESOLVED" | grep oozie.base.url
```

Esse comando retorna um valor semelhante ao seguinte, que contém a URL interna a ser usada com o comando `oozie`:

```output
"oozie.base.url": "http://<ACTIVE-HEADNODE-NAME>cx.internal.cloudapp.net:11000/oozie"
```

Para saber mais sobre como trabalhar com a API REST do Ambari, consulte [Monitorar e gerenciar o HDInsight usando a API REST do Apache Ambari](hdinsight-hadoop-manage-ambari-rest-api.md).

### <a name="accessing-other-node-types"></a>Acessando outros tipos de nó

Você pode se conectar a nós que não são diretamente acessíveis pela Internet usando os seguintes métodos:

|Método |Descrição |
|---|---|
|SSH|Assim que estiver conectado a um nó de cabeçalho usando o SSH, você poderá usar o SSH por meio do nó de cabeçalho para se conectar aos outros nós no cluster. Para saber mais, consulte o documento [Usar SSH com HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).|
|Túnel SSH|Se precisar acessar um serviço Web hospedado em um dos nós que não está exposto à Internet, você deverá usar um túnel SSH. Para saber mais, consulte o documento [Usar túnel SSH com HDInsight](hdinsight-linux-ambari-ssh-tunnel.md).|
|Rede Virtual do Azure|Caso o cluster HDInsight faça parte de uma Rede Virtual do Azure, qualquer recurso contido na mesma Rede Virtual poderá acessar diretamente todos os nós no cluster. Para obter mais informações, consulte o documento [planejar uma rede virtual para o HDInsight](hdinsight-plan-virtual-network-deployment.md) .|

## <a name="how-to-check-on-a-service-status"></a>Como verificar o status do serviço

A interface do usuário da Web do Ambari ou a API REST do Ambari pode ser usada para verificar o status dos serviços executados nos nós de cabeçalho.

### <a name="ambari-web-ui"></a>Interface do usuário da Ambari Web

A interface do usuário da Ambari Web é visível em `https://CLUSTERNAME.azurehdinsight.net`. Substitua **CLUSTERNAME** pelo nome do cluster. Se solicitado, insira as credenciais do usuário HTTP para o cluster. O nome de usuário padrão HTTP é **admin** e a senha é a senha que você digitou ao criar o cluster.

Ao chegar na página Ambari, os serviços instalados serão listados à esquerda da página.

![Serviços instalados do Apache Ambari](./media/hdinsight-high-availability-linux/hdinsight-installed-services.png)

Há uma série de ícones que podem aparecer ao lado de um serviço para indicar o status. Todos os alertas relacionados a um serviço podem ser visualizados usando o link **Alertas** na parte superior da página.  O Ambari oferece vários alertas predefinidos.

Os alertas a seguir ajudam a monitorar a disponibilidade de um cluster:

| Nome do alerta                               | Descrição                                                                                                                                                                                  |
|------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Status do monitor de métrica                    | Esse alerta indica o status do processo de monitor de métricas conforme determinado pelo script de status do monitor.                                                                                   |
| Pulsação do agente Ambari                   | Esse alerta será disparado se o servidor tiver perdido o contato com um agente.                                                                                                                        |
| Processo do servidor ZooKeeper                 | Esse alerta em nível de host será disparado se o processo do servidor ZooKeeper não puder ser determinado como ativo e escutando na rede.                                                               |
| Status do servidor de metadados IOCache           | Esse alerta no nível do host será disparado se o servidor de metadados IOCache não puder ser determinado como ativo e respondendo às solicitações do cliente                                                            |
| Interface do usuário da Web do amJournalNode                       | Esse alerta no nível do host será disparado se a interface do usuário da Web do amJournalNode estiver inacessível.                                                                                                                 |
| Servidor Spark2 thrift                     | Esse alerta em nível de host será disparado se o servidor Spark2 Thrift não puder ser determinado como ativo.                                                                                                |
| Processo do servidor de histórico                   | Esse alerta em nível de host será disparado se o processo do servidor de histórico não puder ser estabelecido para estar ativo e escutando na rede.                                                                |
| IU da Web do servidor de histórico                    | Esse alerta no nível do host será disparado se a interface do usuário da Web do servidor de histórico estiver inacessível.                                                                                                              |
| interface do usuário da Web do `ResourceManager`                   | Esse alerta no nível do host será disparado se a interface do usuário da Web do `ResourceManager` estiver inacessível.                                                                                                             |
| Resumo de integridade do NodeManager               | Esse alerta de nível de serviço é disparado se houver NodeManagers não íntegros                                                                                                                    |
| Linha do tempo de aplicativo da Web                      | Esse alerta no nível do host será disparado se a interface do usuário da Web do servidor de linha do tempo do aplicativo estiver inacessível.                                                                                                         |
| Resumo de integridade do datanode                  | Esse alerta de nível de serviço é disparado se houver datanodes não íntegros                                                                                                                       |
| Interface do usuário da Web do amNameNode                          | Esse alerta no nível do host será disparado se a interface do usuário da Web do amNameNode estiver inacessível.                                                                                                                    |
| Processo do controlador de failover ZooKeeper    | Esse alerta em nível de host será disparado se o processo do controlador de failover ZooKeeper não puder ser confirmado para estar ativo e escutando na rede.                                                   |
| Interface do usuário da Web do Oozie Server                      | Esse alerta no nível do host será disparado se a interface do usuário da Web do Oozie Server estiver inacessível.                                                                                                                |
| Status do servidor Oozie                      | Esse alerta no nível do host será disparado se o servidor Oozie não puder ser determinado como ativo e respondendo às solicitações do cliente.                                                                      |
| Processo de metastore do hive                   | Esse alerta em nível de host será disparado se o processo de metastore do hive não puder ser determinado como ativo e escutando na rede.                                                                 |
| Processo de HiveServer2                      | Esse alerta no nível do host será disparado se o HiveServer não puder ser determinado como ativo e respondendo às solicitações do cliente.                                                                        |
| Status do servidor WebHCat                    | Esse alerta em nível de host será disparado se o status do servidor de `templeton` não estiver íntegro.                                                                                                            |
| Porcentagem de servidores ZooKeeper disponíveis      | Esse alerta será disparado se o número de servidores ZooKeeper no cluster for maior que o limite crítico configurado. Ele agrega os resultados das verificações de processo do ZooKeeper.     |
| Servidor Spark2 Livy                       | Esse alerta no nível do host será disparado se o servidor Livy2 não puder ser determinado como ativo.                                                                                                        |
| Servidor de histórico do Spark2                    | Esse alerta em nível de host será disparado se o servidor de histórico Spark2 não puder ser determinado como ativo.                                                                                               |
| Processo do coletor de métricas                | Esse alerta será disparado se o coletor de métricas não puder ser confirmado para estar ativo e escutando na porta configurada por um número de segundos igual ao limite.                                 |
| Coletor de métricas – processo de HBase Master | Esse alerta será disparado se os processos do HBase Master do coletor de métricas não puderem ser confirmados como ativos e escutando na rede o limite crítico configurado, fornecido em segundos. |
| Percentual de monitores de métricas disponíveis       | Esse alerta será disparado se um percentual de processos de monitor de métricas não estiver ativo e escutando na rede para o aviso e os limites críticos configurados.                             |
| Porcentagem de NodeManagers disponíveis           | Esse alerta será disparado se o número de NodeManagers inativos no cluster for maior que o limite crítico configurado. Ele agrega os resultados das verificações de processo do NodeManager.        |
| Integridade do NodeManager                       | Esse alerta no nível do host verifica a propriedade de integridade do nó disponível no componente NodeManager.                                                                                              |
| Interface do usuário da Web do NodeManager                       | Esse alerta em nível de host será disparado se a interface do usuário da Web NodeManager não estiver acessível.                                                                                                                 |
| Integridade de alta disponibilidade do NameNode        | Esse alerta de nível de serviço será disparado se o NameNode ativo ou o NameNode em espera não estiverem em execução.                                                                                     |
| Processo do datanode                         | Esse alerta em nível de host será disparado se os processos individuais de datanode não puderem ser estabelecidos para serem ativados e escutando na rede.                                                         |
| Interface do usuário da Web do datanode                          | Esse alerta no nível do host será disparado se a interface do usuário da Web do datanode estiver inacessível.                                                                                                                    |
| Porcentagem de JournalNodes disponíveis           | Esse alerta será disparado se o número de JournalNodes inativos no cluster for maior que o limite crítico configurado. Ele agrega os resultados das verificações de processo do JournalNode.        |
| Porcentagem de datanodes disponíveis              | Esse alerta será disparado se o número de datanodes inativos no cluster for maior que o limite crítico configurado. Ele agrega os resultados de verificações de processo de datanode.              |
| Status do servidor Zeppelin                   | Esse alerta no nível do host será disparado se o servidor Zeppelin não puder ser determinado como ativo e respondendo às solicitações do cliente.                                                                   |
| Processo interativo do HiveServer2          | Esse alerta no nível do host será disparado se o HiveServerInteractive não puder ser determinado como ativo e respondendo às solicitações do cliente.                                                             |
| Aplicativo LLAP                         | Esse alerta será disparado se o aplicativo LLAP não puder ser determinado como ativo e respondendo a solicitações.                                                                                    |

Você pode selecionar cada serviço para exibir mais informações sobre ele.

Enquanto a página de serviço fornece informações sobre o status e a configuração de cada serviço, ela não fornece informações sobre em qual nó de cabeçalho o serviço está sendo executado. Para exibir essas informações, use o link **Hosts** na parte superior da página. Isso exibe os hosts do cluster, incluindo os nós de cabeçalho.

![Lista de hosts do Apache Ambari cabeçalho](./media/hdinsight-high-availability-linux/hdinsight-hosts-list.png)

Selecionar o link para um dos nós de cabeçalho exibirá os serviços e componentes em execução nesse nó.

![Status do componente Apache Ambari](./media/hdinsight-high-availability-linux/hdinsight-node-services.png)

Para saber mais sobre como usar o Ambari, confira [Monitorar e gerenciar o HDInsight usando a interface de usuário da Web do Apache Ambari](hdinsight-hadoop-manage-ambari.md).

### <a name="ambari-rest-api"></a>API REST do Ambari

A API REST do Ambari está disponível pela internet. O gateway público HDInsight manipula as solicitações de roteamento para o nó de cabeçalho que hospeda a API REST.

Você pode usar o seguinte comando para verificar o estado de um serviço por meio da API de REST do Ambari:

```bash
curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SERVICENAME?fields=ServiceInfo/state
```

* Substitua a **SENHA** pela senha da conta do usuário HTTP (administrador).
* Substitua **CLUSTERNAME** pelo nome do cluster.
* Substitua o **SERVICENAME** pelo nome do serviço desejado para verificar o status.

Por exemplo, para verificar o status do serviço **HDFS** em um cluster chamado **mycluster**, por uma senha de **senha**, você usaria o seguinte comando:

```bash
curl -u admin:password https://mycluster.azurehdinsight.net/api/v1/clusters/mycluster/services/HDFS?fields=ServiceInfo/state
```

A resposta é semelhante ao JSON a seguir:

```json
{
    "href" : "http://mycluster.wutj3h4ic1zejluqhxzvckxq0g.cx.internal.cloudapp.net:8080/api/v1/clusters/mycluster/services/HDFS?fields=ServiceInfo/state",
    "ServiceInfo" : {
    "cluster_name" : "mycluster",
    "service_name" : "HDFS",
    "state" : "STARTED"
    }
}
```

A URL nos informa que o serviço está sendo executado no momento em um nó de cabeçalho chamado **mycluster. wutj3h4ic1zejluqhxzvckxq0g**.

O estado indica que o serviço está sendo executado ou **INICIADO**.

Se você não souber quais serviços estão instalados no cluster, poderá usar o seguinte comando para recuperar uma lista:

```bash
curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services
```

Para saber mais sobre como trabalhar com a API REST do Ambari, consulte [Monitorar e gerenciar o HDInsight usando a API REST do Apache Ambari](hdinsight-hadoop-manage-ambari-rest-api.md).

#### <a name="service-components"></a>Componentes do serviço

Serviços podem conter componentes os quais você deseja verificar o status de individualmente. Por exemplo, o HDFS contém o componente NameNode. Para exibir informações sobre um componente, o comando seria:

```bash
curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SERVICE/components/component
```

Se você não souber quais componentes são fornecidos por um serviço, poderá usar o seguinte comando para recuperar uma lista:

```bash
curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SERVICE/components/component
```

## <a name="how-to-access-log-files-on-the-head-nodes"></a>Como acessar arquivos de log nos nós de cabeçalho

### <a name="ssh"></a>SSH

Enquanto estiver conectado a um nó de cabeçalho por meio do SSH, os arquivos de log podem ser encontrados em **/var/log**. Por exemplo, **/var/log/hadoop-yarn/yarn** contêm logs para YARN.

Cada nó de cabeçalho pode ter entradas de log exclusivo, portanto você deve verificar os logs em ambos.

### <a name="sftp"></a>SFTP

Também é possível se conectar ao nó de cabeçalho usando o Protocolo FTP do SSH ou SFTP e baixar os arquivos de log diretamente.

Semelhante ao uso de um cliente SSH, ao se conectar com o cluster, é necessário fornecer o nome de conta de usuário SSH e o endereço SSH do cluster. Por exemplo, `sftp username@mycluster-ssh.azurehdinsight.net`. Forneça a senha da conta quando solicitado ou uma chave pública usando o parâmetro `-i`.

Uma vez conectado, você verá um prompt de `sftp>`. Neste prompt, é possível alterar os diretórios, além de carregar e baixar arquivos. Por exemplo, os seguintes comandos alteram os diretórios para o diretório **/var/log/hadoop/hdfs** e baixam todos os arquivos no diretório em seguida.

    cd /var/log/hadoop/hdfs
    get *

Para obter uma lista dos comandos disponíveis, insira `help` no prompt `sftp>`.

> [!NOTE]  
> Há também interfaces gráficas que permitem visualizar o sistema de arquivos quando você estiver conectado usando SFTP. Por exemplo, [MobaXTerm](https://mobaxterm.mobatek.net/) permite que você procure o sistema de arquivos usando uma interface semelhante à do Windows Explorer.

### <a name="ambari"></a>Ambari

> [!NOTE]  
> Para acessar os arquivos de log usando o Ambari, use um túnel SSH. As interfaces da web para os serviços individuais não são expostas publicamente na Internet. Para saber mais sobre como usar um túnel SSH, veja o documento [Usar túnel SSH](hdinsight-linux-ambari-ssh-tunnel.md).

Na interface de usuário da Web do Ambari, selecione o serviço do qual você deseja exibir os logs (por exemplo, YARN). Em seguida, use os **Links Rápidos** a fim de selecionar para qual nó de cabeçalho exibir os logs.

![Usando links rápidos para exibir logs](./media/hdinsight-high-availability-linux/quick-links-view-logs.png)

## <a name="how-to-configure-the-node-size"></a>Como configurar o tamanho do nó

O tamanho de um nó só pode ser selecionado durante a criação do cluster. Você pode encontrar uma lista de diferentes tamanhos de VM disponíveis para o HDInsight na [página de preços do HDInsight](https://azure.microsoft.com/pricing/details/hdinsight/).

Ao criar um cluster, você pode especificar o tamanho dos nós. As informações a seguir fornecem orientações sobre como especificar o tamanho usando o [portal do Azure](https://portal.azure.com/), o [módulo Azure PowerShell Az](/powershell/azureps-cmdlets-docs)e o [CLI do Azure](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest):

* **Portal do Azure**: ao criar um cluster, você pode definir o tamanho dos nós usados pelo cluster:

    ![Imagem do Assistente de criação de cluster com a seleção de tamanho do nó](./media/hdinsight-high-availability-linux/azure-portal-cluster-configuration-pricing-hadoop.png)

* **CLI do Azure**: ao usar o comando [`az hdinsight create`](https://docs.microsoft.com/cli/azure/hdinsight?view=azure-cli-latest#az-hdinsight-create) , você pode definir o tamanho dos nós de cabeçalho, trabalho e ZooKeeper usando os parâmetros `--headnode-size`, `--workernode-size`e `--zookeepernode-size`.

* **Azure PowerShell**: ao usar o cmdlet [New-AzHDInsightCluster](https://docs.microsoft.com/powershell/module/az.hdinsight/new-azhdinsightcluster) , você pode definir o tamanho dos nós de cabeçalho, trabalho e ZooKeeper usando os parâmetros `-HeadNodeSize`, `-WorkerNodeSize`e `-ZookeeperNodeSize`.

## <a name="next-steps"></a>{1&gt;{2&gt;Próximas etapas&lt;2}&lt;1}

Para saber mais sobre os itens discutidos neste artigo, consulte:

* [Referência REST do Apache Ambari](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md)
* [Instalar e configurar a CLI do Azure.](https://docs.microsoft.com//cli/azure/install-azure-cli?view=azure-cli-latest)
* [Instalar e configurar o módulo de Azure PowerShell AZ](/powershell/azure/overview)
* [Gerenciar clusters HDInsight usando o Apache Ambari](hdinsight-hadoop-manage-ambari.md)
* [Provisionar os clusters HDInsight baseados em Linux](hdinsight-hadoop-provision-linux-clusters.md)

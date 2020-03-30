---
title: 'Transformação de dados: processos & transformar dados '
description: Aprenda a transformar ou processar dados no Azure Data Factory usando o Hadoop, o Machine Learning ou o Azure Data Lake Analytics.
services: data-factory
documentationcenter: ''
author: djpmsft
ms.author: daperlov
manager: jroth
ms.reviewer: maghan
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/10/2018
ms.openlocfilehash: 5b3e2db9b9769dee7599a2446b272e04cc0bedf7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74703383"
---
# <a name="transform-data-in-azure-data-factory"></a>Transformar dados no Azure Data Factory
> [!div class="op_single_selector"]
> * [Colméia](data-factory-hive-activity.md)  
> * [Pig](data-factory-pig-activity.md)  
> * [Mapreduce](data-factory-map-reduce.md)  
> * [Streaming do Hadoop](data-factory-hadoop-streaming-activity.md)
> * [Aprendizado de Máquina](data-factory-azure-ml-batch-execution-activity.md) 
> * [Procedimento armazenado](data-factory-stored-proc-activity.md)
> * [U-SQL da Análise Data Lake](data-factory-usql-activity.md)
> * [Personalizado do .NET](data-factory-use-custom-activities.md)

## <a name="overview"></a>Visão geral
> [!NOTE]
> Este artigo aplica-se à versão 1 do Data Factory. Se você estiver usando a versão atual do serviço Data Factory, consulte [atividades de transformação de dados no Data Factory ](../transform-data.md).

Este artigo explica as atividades de transformação de dados no Azure Data Factory que você pode usar para transformar e processar dados brutos em previsões e ideias. Uma atividade de transformação é executada em um ambiente de cálculo, como um cluster do Azure HDInsight ou um Lote do Azure. Ela fornece links para artigos com informações detalhadas sobre cada atividade de transformação.

O Data Factory dá suporte às seguintes atividades de transformação de dados, que podem ser adicionadas aos [pipelines](data-factory-create-pipelines.md) individualmente ou encadeadas a outra atividade.

> [!NOTE]
> Para uma explicação com instruções passo a passo, confira o artigo [Criar um pipeline com a transformação do Hive](data-factory-build-your-first-pipeline.md) .  
> 
> 

## <a name="hdinsight-hive-activity"></a>Atividade de Hive do HDInsight
A atividade de Hive do HDInsight em um pipeline de Data Factory executa consultas de Hive em seu próprio cluster ou no cluster sob demanda do HDInsight baseado em Windows/Linux. Consulte o artigo [Atividade colmeia](data-factory-hive-activity.md) para obter detalhes sobre esta atividade. 

## <a name="hdinsight-pig-activity"></a>Atividade de Pig do HDInsight
A atividade de Pig do HDInsight em um pipeline de Data Factory executa consultas de Pig em seu próprio cluster ou no cluster sob demanda do HDInsight baseado em Windows/Linux. Consulte o artigo [Atividade suína](data-factory-pig-activity.md) para obter detalhes sobre esta atividade. 

## <a name="hdinsight-mapreduce-activity"></a>Atividade de MapReduce do HDInsight
A atividade de MapReduce do HDInsight em um pipeline do Data Factory executa programas MapReduce no seu próprio cluster HDInsight baseado em Windows/Linux, ou em um sob demanda. Consulte [MapReduce Activity](data-factory-map-reduce.md) para obter detalhes sobre essa atividade.

## <a name="hdinsight-streaming-activity"></a>Atividade de Streaming do HDInsight
A Atividade de Streaming do HDInsight em um pipeline do Data Factory executa programas de Transmissão do Hadoop em seu próprio ou cluster HDInsight sob demanda baseado no Windows/Linux. Confira [Atividade de HDInsight Streaming](data-factory-hadoop-streaming-activity.md) para obter detalhes sobre essa atividade.

## <a name="hdinsight-spark-activity"></a>Atividade do HDInsight Spark
A atividade do HDInsight Spark em um pipeline do Data Factory executa programas do Spark em seu próprio cluster HDInsight. Para obter detalhes, consulte [Invocar programas Spark do Azure Data Factory](data-factory-spark.md) para obter detalhes. 

## <a name="machine-learning-activities"></a>Atividades de Machine Learning
O Azure Data Factory permite que você crie facilmente pipelines que usam o serviço Web do Azure Machine Learning publicado para a análise preditiva. Usando a [atividade de execução](data-factory-azure-ml-batch-execution-activity.md#invoking-a-web-service-using-batch-execution-activity) em lote em um pipeline da Fábrica de Dados Do Azure, você pode invocar um serviço web de Aprendizado de Máquina para fazer previsões sobre os dados em lote.

Ao longo do tempo, os modelos de previsão nos testes de pontuação do Machine Learning precisam ser readaptados usando novos conjuntos de dados de entrada. Depois de concluir o novo treinamento, você deseja atualizar o serviço Web de pontuação com o modelo do Machine Learning readaptado. Você pode usar a [Atividade de recurso de atualização](data-factory-azure-ml-batch-execution-activity.md#updating-models-using-update-resource-activity) para atualizar o serviço web com o modelo recém-treinado.  

Confira [Usar atividades de Machine Learning](data-factory-azure-ml-batch-execution-activity.md) para obter detalhes sobre essas atividades de Machine Learning. 

## <a name="stored-procedure-activity"></a>Atividade de procedimento armazenado
Você pode usar a atividade de Procedimento Armazenado do SQL Server em um pipeline de Data Factory para invocar um procedimento armazenado em um dos seguintes repositórios de dados: Banco de Dados SQL do Azure, SQL Data Warehouse, Banco de Dados SQL Server na sua empresa ou uma VM do Azure. Consulte o artigo [Atividade do Procedimento Armazenado](data-factory-stored-proc-activity.md) para obter detalhes.  

## <a name="data-lake-analytics-u-sql-activity"></a>Atividade do U-SQL do Data Lake Analytics
A atividade de U-SQL do Data Lake Analytics executa um script U-SQL em um cluster do Azure Data Lake Analytics. Consulte o artigo [Data Analytics U-SQL Activity](data-factory-usql-activity.md) para obter detalhes. 

## <a name="net-custom-activity"></a>Atividade personalizada do .NET
Se precisar transformar dados de uma maneira que não tenha suporte do Data Factory, você poderá criar uma atividade personalizada com sua própria lógica de processamento de dados e usar a atividade no pipeline. Você pode configurar a atividade personalizada do .NET para que seja executada usando um serviço de Lote do Azure ou um cluster do Azure HDInsight. Confira o artigo [Usar atividades personalizadas](data-factory-use-custom-activities.md) para obter detalhes. 

Você pode criar uma atividade personalizada para executar scripts R em seu cluster HDInsight com R instalado. Veja [Executar scripts R usando o Azure Data Factory](https://github.com/Azure/Azure-DataFactory/tree/master/SamplesV1/RunRScriptUsingADFSample). 

## <a name="compute-environments"></a>Ambientes de computação
Crie um serviço vinculado para o ambiente de computação e, em seguida, usar o serviço vinculado ao definir uma atividade de transformação. Há dois tipos de ambientes de computação com suporte do Data Factory. 

1. **Sob demanda**: nesse caso, o ambiente de computação é totalmente gerenciado pelo Data Factory. Ele é automaticamente criado pelo serviço Data Factory antes de um trabalho ser enviado a fim de processar os dados e é removido após a conclusão do trabalho. Você pode configurar e controlar as configurações granulares do ambiente de computação sob demanda para execução do trabalho, gerenciamento de cluster e ações de inicialização. 
2. **Traga seu próprio**: nesse caso, você pode registrar no Data Factory seu próprio ambiente de computação (por exemplo, o cluster HDInsight) como um serviço vinculado. O ambiente de computação é gerenciado por você e o serviço Data Factory o utiliza para executar as atividades. 

Veja o artigo [Serviços vinculados de computação](data-factory-compute-linked-services.md) para saber mais sobre os serviços de computação com suporte do Data Factory. 

## <a name="summary"></a>Resumo
O Azure Data Factory dá suporte às atividades de transformação de dados e ambientes de computação para as atividades a seguir. As atividades de transformação podem ser adicionadas a pipelines tanto individualmente quanto encadeadas a outra atividade.

| Atividades de transformação de dados | Ambiente de computação |
|:--- |:--- |
| [Colméia](data-factory-hive-activity.md) |HDInsight [Hadoop] |
| [Pig](data-factory-pig-activity.md) |HDInsight [Hadoop] |
| [Mapreduce](data-factory-map-reduce.md) |HDInsight [Hadoop] |
| [Streaming do Hadoop](data-factory-hadoop-streaming-activity.md) |HDInsight [Hadoop] |
| [Atividades de Machine Learning: execução do Lote e recurso de atualização](data-factory-azure-ml-batch-execution-activity.md) |VM do Azure |
| [Procedimento armazenado](data-factory-stored-proc-activity.md) |SQL Azure, Azure SQL Data Warehouse ou SQL Server |
| [U-SQL da Análise Data Lake](data-factory-usql-activity.md) |Análise Azure Data Lake |
| [Dotnet](data-factory-use-custom-activities.md) |HDInsight [Hadoop] ou Lote do Azure |


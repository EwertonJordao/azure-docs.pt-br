---
title: Ingestão de dados & automação
titleSuffix: Azure Machine Learning
description: Saiba mais sobre as opções de ingestão de dados para treinar seus modelos de aprendizado de máquina.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: nibaccam
author: nibaccam
ms.author: nibaccam
ms.date: 02/26/2020
ms.custom: devx-track-python
ms.openlocfilehash: 18bbecbe811a9f0bc6a56194830c7e92d8770979
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/09/2020
ms.locfileid: "90890167"
---
# <a name="data-ingestion-options-for-azure-machine-learning-workflows"></a>Opções de ingestão de dados para fluxos de trabalho de Azure Machine Learning

Neste artigo, você aprende os prós e contras das opções de ingestão de dados disponíveis com Azure Machine Learning. 

Escolha:
+ [Azure data Factory](#azure-data-factory) pipelines, especificamente criados para extrair, carregar e transformar dados

+ [Azure Machine Learning o SDK do Python](#azure-machine-learning-python-sdk), fornecendo uma solução de código personalizado para tarefas de ingestão de dados.

+ uma combinação de ambos

A ingestão de dados é o processo no qual os dados não estruturados são extraídos de uma ou várias fontes e, em seguida, preparados para o treinamento de modelos de aprendizado de máquina. Ele também é demorado, especialmente se feito manualmente, e se você tiver grandes quantidades de dados de várias fontes. Automatizar esse esforço libera recursos e garante que seus modelos usem os dados mais recentes e aplicáveis.

## <a name="azure-data-factory"></a>Azure Data Factory

O [Azure data Factory](https://docs.microsoft.com/azure/data-factory/introduction) oferece suporte nativo para monitoramento de fonte de dados e gatilhos para pipelines de ingestão de dados.  

A tabela a seguir resume os prós e contras para usar Azure Data Factory para seus fluxos de trabalho de ingestão de dados.

|Vantagens|Desvantagens
---|---
Especificamente criado para extrair, carregar e transformar dados.|Atualmente oferece um conjunto limitado de tarefas de pipeline de Azure Data Factory 
Permite que você crie fluxos de trabalho controlados por dados para orquestrar a movimentação de dados e transformações em escala.|Caro de construir e manter. Consulte a [página de preços](https://azure.microsoft.com/pricing/details/data-factory/data-pipeline/) do Azure data Factory para obter mais informações.
Integrado com várias ferramentas do Azure, como [Azure Databricks](https://docs.microsoft.com/azure/data-factory/transform-data-using-databricks-notebook) e [Azure Functions](https://docs.microsoft.com/azure/data-factory/control-flow-azure-function-activity) | Não executa scripts nativamente, em vez de se basear em computação separada para execuções de script 
Dá suporte nativo à ingestão de dados disparados por fonte de dados| 
Os processos de preparação de dados e treinamento de modelo são separados.|
Recurso de linhagem de dados inserida para fluxos de Azure Data Factory|
Fornece uma [interface do usuário](https://docs.microsoft.com/azure/data-factory/quickstart-create-data-factory-portal) de experiência de código baixa para abordagens sem scripts |

Essas etapas e o diagrama a seguir ilustram o fluxo de trabalho de ingestão de dados Azure Data Factory.

1. Efetuar pull dos dados de suas fontes
1. Transformar e salvar os dados em um contêiner de blob de saída, que serve como armazenamento de dados para Azure Machine Learning
1. Com os dados preparados armazenados, o pipeline de Azure Data Factory invoca um pipeline de Machine Learning de treinamento que recebe os dados preparados para treinamento de modelo


    ![Ingestão de dados do ADF](media/concept-data-ingestion/data-ingest-option-one.svg)
    
Saiba como criar um pipeline de ingestão de dados para Machine Learning com [Azure data Factory](how-to-data-ingest-adf.md).

## <a name="azure-machine-learning-python-sdk"></a>SDK do Python do Azure Machine Learning 

Com o [SDK do Python](https://docs.microsoft.com/python/api/overview/azure/ml), você pode incorporar tarefas de ingestão de dados em uma etapa [Azure Machine Learning pipeline](how-to-create-your-first-pipeline.md) .

A tabela a seguir resume os prós e con para usar o SDK e uma etapa de pipelines do ML para tarefas de ingestão de dados.

Vantagens| Desvantagens
---|---
Configurar seus próprios scripts Python | O não dá suporte nativo ao disparo de alterações da fonte de dados. Requer o aplicativo lógico ou implementações de função do Azure
Preparação de dados como parte de cada execução de treinamento de modelo|Requer habilidades de desenvolvimento para criar um script de ingestão de dados
Dá suporte a scripts de preparação de dados em vários destinos de computação, incluindo [Azure Machine Learning computação](concept-compute-target.md#azure-machine-learning-compute-managed) |Não fornece uma interface do usuário para criar o mecanismo de ingestão

No diagrama a seguir, o pipeline de Azure Machine Learning consiste em duas etapas: ingestão de dados e treinamento de modelo. A etapa de ingestão de dados abrange tarefas que podem ser realizadas usando bibliotecas Python e o SDK do Python, como a extração de dados de fontes locais/da Web e transformações de dados, como o valor ausente imputação. Em seguida, a etapa de treinamento usa os dados preparados como entrada para seu script de treinamento para treinar o modelo de aprendizado de máquina. 

![Pipeline do Azure + ingestão de dados do SDK](media/concept-data-ingestion/data-ingest-option-two.png)

## <a name="next-steps"></a>Próximas etapas

Siga estes artigos de instruções:
* [Criar um pipeline de ingestão de dados com Azure Data Factory](how-to-data-ingest-adf.md)

* [Automatize e gerencie pipelines de ingestão de dados com Azure pipelines](how-to-cicd-data-ingestion.md).

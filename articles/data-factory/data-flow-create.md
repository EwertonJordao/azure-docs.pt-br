---
title: Criar um fluxo de dados de mapeamento
description: Como criar um fluxo de dados de mapeamento de Azure Data Factory
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 02/12/2019
ms.openlocfilehash: cd1342ae5912a9a91f1bd8d1bdacecdb5a2bbdb0
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/09/2020
ms.locfileid: "81416521"
---
# <a name="create-azure-data-factory-data-flow"></a>Criar Fluxo de Dados do Azure Data Factory

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

Os Fluxos de Dados de Mapeamento no ADF fornecem uma maneira de transformar dados em grande escala sem a necessidade de nenhuma codificação. Você pode criar um trabalho de transformação de dados no designer de fluxo de dados, criando uma série de transformações. Comece com qualquer número de transformações de código-fonte, seguidas pelas etapas de transformação de dados. Em seguida, conclua seu fluxo de dados com o coletor para descarregar seus resultados em um destino.

Comece pela primeira vez criando uma nova Data Factory v2 do portal do Azure. Depois de criar sua nova fábrica, clique no bloco “Criar e Monitorar” para iniciar a IU do Data Factory.

![Opções de fluxo de dados](media/data-flow/v2portal.png "criação de fluxo de dados")

Quando estiver na IU do Data Factory, você pode usar Fluxos de Dados de exemplo. Os exemplos estão disponíveis na Galeria de Modelos do ADF. Em ADF, crie “Pipeline de modelo” e selecione a categoria de Fluxo de Dados da galeria de modelos.

![Opções de fluxo de dados](media/data-flow/template.png "criação de fluxo de dados")

Será solicitado que você insira suas informações de conta do Armazenamento de Blobs do Azure.

![Opções de fluxo de dados](media/data-flow/template2.png "criação de fluxo de dados 2")

[Os dados usados para esses exemplos podem ser encontrados aqui](https://github.com/kromerm/adfdataflowdocs/tree/master/sampledata). Baixe os dados de exemplo e armazene os arquivos em suas contas de armazenamento de Blobs do Azure para que você possa executar os exemplos.

## <a name="create-new-data-flow"></a>Criar novo fluxo de dados

Use o botão criar recurso "sinal de adição" na interface do usuário do ADF para criar fluxos de dados.

![Opções de fluxo de dados](media/data-flow/newresource.png "Novo recurso")

## <a name="next-steps"></a>Próximas etapas

Comece a criar sua transformação de dados com uma [transformação de origem](data-flow-source.md).

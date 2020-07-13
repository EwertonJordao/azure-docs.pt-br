---
title: 'Início Rápido: Compilar, implantar e usar um modelo personalizado – Tradutor Personalizado'
titleSuffix: Azure Cognitive Services
description: Neste início rápido, você percorre passo a passo o processo de criação de um sistema de tradução usando o Tradutor Personalizado.
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.date: 05/26/2020
ms.author: swmachan
ms.topic: quickstart
ms.openlocfilehash: b0992c4d18fdb9cb5201ab3ef52fba8ee3feb7a2
ms.sourcegitcommit: 845a55e6c391c79d2c1585ac1625ea7dc953ea89
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2020
ms.locfileid: "85964372"
---
# <a name="quickstart-build-deploy-and-use-a-custom-model-for-translation"></a>Início Rápido: Compilar, implantar e usar um modelo personalizado para tradução

Este artigo fornece instruções passo a passo para criar um sistema de tradução com o Tradutor Personalizado.

## <a name="prerequisites"></a>Pré-requisitos

1. Para entrar e usar o Portal do [Tradutor Personalizado](https://portal.customtranslator.azure.ai), você precisará de uma [conta da Microsoft](https://signup.live.com) ou de uma [conta do Azure AD](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-whatis) (conta da organização hospedada no Azure).

2. Uma assinatura do Tradutor via portal do Azure. Você precisará de uma chave de assinatura do Tradutor para associar ao seu workspace no Tradutor Personalizado. Consulte [como se inscrever para o Tradutor](https://docs.microsoft.com/azure/cognitive-services/translator/translator-text-how-to-signup).

3. Quando você tiver ambos os itens acima, entre no portal do [Tradutor Personalizado](https://portal.customtranslator.azure.ai) para criar workspaces e projetos, carregar arquivos e criar/implantar modelos.

## <a name="create-a-workspace"></a>Criar um workspace

Se você for um usuário iniciante, será solicitado a concordar com os Termos de Serviço, criar um workspace e associar seu workspace à assinatura do Tradutor.

![Criar workspace](media/quickstart/terms-of-service.png)
![Criar workspace](media/quickstart/create-workspace-1.png)
![Criar workspace](media/quickstart/create-workspace-2.png)
![Criar workspace](media/quickstart/create-workspace-3.png)
![Criar workspace](media/quickstart/create-workspace-4.png)
![Criar workspace](media/quickstart/create-workspace-5.png)
![Criar workspace](media/quickstart/create-workspace-6.png)

Nas visitas subsequentes ao portal do Tradutor Personalizado, navegue até a página Configurações, na qual você pode gerenciar seu espaço de trabalho, criar mais espaços de trabalho, associar sua chave de assinatura do Tradutor a seus espaços de trabalho, adicionar coproprietários e alterar uma chave de assinatura.

## <a name="create-a-project"></a>Criar um projeto

Na página de aterrissagem do portal do Tradutor Personalizado, clique em Novo Projeto. Na caixa de diálogo, você pode inserir o nome desejado para o projeto, o par de idiomas e a categoria, além de outros campos relevantes. Em seguida, salve seu projeto. Para obter mais detalhes, visite [Criar Projeto](how-to-create-project.md).

![Criar projeto](media/quickstart/ct-how-to-create-project.png)


## <a name="upload-documents"></a>Carregar documentos

Em seguida, carregue os conjuntos de documentos [treinamento](training-and-model.md#training-document-type-for-custom-translator), [ajuste](training-and-model.md#tuning-document-type-for-custom-translator) e [testes](training-and-model.md#testing-dataset-for-custom-translator). Você pode carregar documentos [paralelos](what-are-parallel-documents.md) e em combinação. Você também pode carregar um [dicionário](what-is-dictionary.md).

Você pode carregar documentos na guia documentos ou na página de um projeto específico.

![Carregar documentos](media/quickstart/ct-how-to-upload.png)

Ao carregar documentos, escolha o tipo (treinamento, ajuste ou testes) e o par de idiomas. Ao carregar documentos paralelos, você precisará especificar também um nome de documento. Para obter mais detalhes, visite [Carregar documento](how-to-upload-document.md).

## <a name="create-a-model"></a>Criar um modelo

Após o upload de todos os documentos necessários, a próxima etapa será criar seu modelo.

Selecione o projeto que você criou. Você verá todos os documentos carregados que compartilham um par de idiomas com este projeto. Selecione os documentos que você deseja incluir em seu modelo. Você pode selecionar [treinamento](training-and-model.md#training-document-type-for-custom-translator), [ajuste](training-and-model.md#tuning-document-type-for-custom-translator) e [testes](training-and-model.md#testing-dataset-for-custom-translator) dos dados, ou selecionar apenas os dados de treinamento e permitir que o Tradutor Personalizado compile, ajuste e teste os conjuntos do seu modelo.

![Criar um modelo](media/quickstart/ct-how-to-train.png)

Após terminar de selecionar os documentos desejados, clique no botão Criar Modelo para criar seu modelo e começar o treinamento. Você pode ver o status do seu treinamento e os detalhes de todos os modelos treinados na guia Modelos.

Para obter mais detalhes, visite [Criar um Modelo](how-to-train-model.md).

## <a name="analyze-your-model"></a>Analisar seu modelo

Após a conclusão bem-sucedida de seu treinamento, inspecione os resultados. A pontuação BLEU é uma métrica que indica a qualidade da sua tradução. Você pode comparar manualmente as traduções feitas com o seu modelo personalizado com as traduções fornecidas em seu conjunto de testes navegando até a guia "Teste" e clicando em "Resultados do Sistema". Inspecionar manualmente algumas dessas traduções lhe dará uma boa ideia da qualidade da tradução produzida pelo seu sistema. Para obter mais detalhes, visite [Resultados de Teste do Sistema](how-to-view-system-test-results.md).

## <a name="deploy-a-trained-model"></a>Implantar um modelo treinado

Quando você estiver pronto para implantar seu modelo treinado, clique no botão "Implantar". Você pode ter um modelo implantado por projeto, e você pode exibir o status da implantação na coluna Status. Para obter mais detalhes, visite [Implantação de Modelo](how-to-view-system-test-results.md#deploy-a-model)

![Implantar um modelo treinado](media/quickstart/ct-how-to-deploy.png)

## <a name="use-a-deployed-model"></a>Usar um modelo implantado

Modelos implantados podem ser acessados por meio do Tradutor especificando a CategoryID](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-translate?tabs=curl). Mais informações sobre o Tradutor podem ser encontradas na página da Web [Referência da API](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference).

## <a name="next-steps"></a>Próximas etapas

- Saiba como navegar pelo [workspace do Tradutor Personalizado e gerenciar seus projetos](workspace-and-project.md).

---
title: 'Tutorial: Dados contextuais com funções — LUIS'
titleSuffix: Azure Cognitive Services
description: Encontre dados relacionados com base no contexto. Por exemplo, os locais de origem e de destino de uma mudança física de um edifício e escritório para outro estão relacionados.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: tutorial
ms.date: 12/17/2019
ms.author: diberry
ms.openlocfilehash: cd646ef061a0be06a9b1a56b72a4f35d9796aa63
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75447851"
---
# <a name="tutorial-extract-contextually-related-data-from-an-utterance"></a>Tutorial: Extrair dados relacionados ao contexto de um enunciado

Neste tutorial, localize partes relacionadas de dados com base no contexto. Por exemplo, os locais de origem e destino para uma transferência de uma cidade para outra. As duas partes de dados podem ser necessárias e estão relacionadas entre si.

Uma função pode ser usada com qualquer tipo de entidade predefinida ou personalizada e usada em padrões e enunciados de exemplo.

**Neste tutorial, você aprenderá a:**

> [!div class="checklist"]
> * Criar novo aplicativo
> * Adicionar intenção
> * Obter informações de origem e destino usando funções
> * Treinar
> * Publicar
> * Obter intenções e funções de entidade do ponto de extremidade

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="related-data"></a>Dados relacionados

Este aplicativo determina para onde um funcionário deve ser movido, da cidade de origem para a cidade de destino. Ele usa uma entidade predefinida GeographyV2 para identificar os nomes das cidades e usa funções para determinar os tipos de localização (origem e destino) no enunciado.

Uma função deve ser usada quando os dados de entidade a serem extraídos:

* Estão relacionados entre si no contexto do enunciado.
* Usam uma escolha de palavra específica para indicar cada função. Exemplos dessas palavras: de/para, deixando/indo, saindo/entrando.
* Com frequência, ambas as funções estão no mesmo enunciado, permitindo que o LUIS aprenda com esse uso contextual frequente.
* Precisam ser agrupados e processados pelo aplicativo cliente como uma unidade de informações.

## <a name="create-a-new-app"></a>Criar um novo aplicativo

1. Entre na versão prévia do portal do LUIS com a URL [https://preview.luis.ai](https://preview.luis.ai).

1. Selecione **Criar aplicativo**, insira o nome `HumanResources` e mantenha a cultura padrão, **Inglês**. Deixe a descrição em branco.

1. Selecione **Concluído**.

## <a name="create-an-intent-to-move-employees-between-cities"></a>Criar uma intenção para mover os funcionários entre as cidades

1. [!INCLUDE [Start in Build section](../../../includes/cognitive-services-luis-tutorial-build-section.md)]

1. Selecione **Criar nova intenção**.

1. Insira `MoveEmployeeToCity` na caixa de diálogo pop-up, depois selecione **Concluído**.

    > [!div class="mx-imgBorder"]
    > ![Captura de tela da caixa de diálogo Criar intenção com](./media/tutorial-entity-roles/create-new-intent-move-employee-to-city.png)

1. Adicione enunciados de exemplo para a intenção.

    |Exemplo de enunciados|
    |--|
    |mover Marcelo Costa saindo de Seattle indo para Orlando|
    |transferir Samuel Ferreira de Seattle para Cairo|
    |Remover Jorge Andrade de Tampa, trazendo-o para Atlanta |
    |mover Clara Barbosa para Tulsa de Chicago|
    |mover Samuel Ferreira saindo de Cairo indo para Tampa|
    |Mudar Marina Azevedo para Oakland de Redmond|
    |Nicolau Mendes de São Francisco para Redmond|
    |Transferir Valério Pereira de São Diego para Bellevue |
    |remover Fabio Pena de Kansas City e movê-lo para Chicago|

    > [!div class="mx-imgBorder"]
    > ![Captura de tela do LUIS com novos enunciados na intenção MoveEmployee](./media/tutorial-entity-roles/hr-enter-utterances.png)

## <a name="add-prebuilt-entity-geographyv2"></a>Adicionar a entidade predefinida geographyV2

A entidade predefinida, geographyV2, extrai informações de localização, incluindo nomes de cidades. Como os enunciados têm dois nomes de cidades, relacionados um ao outro no contexto, use funções para extrair esse contexto.

1. Selecione **Entidades** no painel de navegação à esquerda.

1. Selecione **Adicionar entidade predefinida** e, em seguida, selecione `geo` na barra de pesquisa para filtrar as entidades predefinidas.

    > [!div class="mx-imgBorder"]
    > ![Adicionar a entidade predefinida geographyV2 ao aplicativo](media/tutorial-entity-roles/add-geographyV2-prebuilt-entity.png)

1. Marque a caixa de seleção e selecione **Concluído**.
1. Na lista **Entidades**, selecione **geographyV2** para abrir a nova entidade.
1. Adicione duas funções, `Origin`, e `Destination`.

    > [!div class="mx-imgBorder"]
    > ![Adicionar funções à entidade predefinida](media/tutorial-entity-roles/add-roles-to-prebuilt-entity.png)

1. Selecione **Intenções** no painel de navegação à esquerda e, em seguida, selecione a intenção **MoveEmployeeToCity**. Observe que os nomes das cidades são rotulados com a entidade predefinida **geographyV2**.
1. Na barra de ferramentas de contexto, selecione a **Paleta de entidades**.

    > [!div class="mx-imgBorder"]
    > ![Selecione a Paleta de entidades na barra de ferramentas de conteúdo](media/tutorial-entity-roles/intent-detail-context-toolbar-select-entity-palette.png)

1. Selecione a entidade predefinida, **geographyV2** e, em seguida, selecione o **Inspetor de entidade**.
1. No **Inspetor de entidade**, selecione uma função, **Destino**. Isso altera o cursor do mouse. Use o cursor para rotular o texto em todos os enunciados, que é a localização de destino.

    > [!div class="mx-imgBorder"]
    > ![Selecionar Função na Paleta de Entidades](media/tutorial-entity-roles/entity-palette-select-entity-role.png)


1. Retorne ao **Inspetor de entidade**, altere a função para **Origem**. Use o cursor para rotular o texto em todos os enunciados, que é a localização de origem.

## <a name="add-example-utterances-to-the-none-intent"></a>Adicionar enunciados de exemplo à intenção None

[!INCLUDE [Follow these steps to add the None intent to the app](../../../includes/cognitive-services-luis-create-the-none-intent.md)]

## <a name="train-the-app-so-the-changes-to-the-intent-can-be-tested"></a>Faça o treinamento do aplicativo para que as alterações na intenção possam ser testadas

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish-the-app-so-the-trained-model-is-queryable-from-the-endpoint"></a>Publicar o aplicativo para que o modelo em que foi feito o treinamento possa ser consultado por meio do ponto de extremidade

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="get-intent-and-entity-prediction-from-endpoint"></a>Obter a previsão de intenção e de entidade do ponto de extremidade

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]


1. Vá até o final da URL na barra de endereços e insira `Please move Carl Chamerlin from Tampa to Portland`. O último parâmetro de querystring é `q`, o enunciado **consulta**. Esse enunciado não é igual a nenhum dos enunciados rotulados e, portanto, é um bom teste e deve retornar a intenção `MoveEmployee` com a entidade extraída.

    ```json
    {
      "query": "Please move Carl Chamerlin from Tampa to Portland",
      "topScoringIntent": {
        "intent": "MoveEmployeeToCity",
        "score": 0.9706451
      },
      "intents": [
        {
          "intent": "MoveEmployeeToCity",
          "score": 0.9706451
        },
        {
          "intent": "None",
          "score": 0.0307451729
        }
      ],
      "entities": [
        {
          "entity": "tampa",
          "type": "builtin.geographyV2.city",
          "startIndex": 32,
          "endIndex": 36,
          "role": "Origin"
        },
        {
          "entity": "portland",
          "type": "builtin.geographyV2.city",
          "startIndex": 41,
          "endIndex": 48,
          "role": "Destination"
        }
      ]
    }
    ```

    A intenção correta é prevista, e a matriz de entidades tem as funções de origem e de destino na propriedade **entities** correspondente.

## <a name="clean-up-resources"></a>Limpar os recursos

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="related-information"></a>Informações relacionadas

* [Conceitos sobre entidades](luis-concept-entity-types.md)
* [Conceitos sobre funções](luis-concept-roles.md)
* [Lista de entidades predefinidas](luis-reference-prebuilt-entities.md)
* [Como treinar](luis-how-to-train.md)
* [Como publicar](luis-how-to-publish-app.md)
* [Como testar no portal do LUIS](luis-interactive-test.md)
* [Funções](luis-concept-roles.md)

## <a name="next-steps"></a>Próximas etapas

Este tutorial criou uma intenção e adicionou enunciados de exemplo para os dados obtidos de acordo com o contexto de locais de origem e destino. Depois que o aplicativo estiver treinado e publicado, o aplicativo cliente poderá usar essas informações para criar um tíquete de mudança com as informações relevantes.

> [!div class="nextstepaction"]
> [Saiba como adicionar uma entidade de composição](luis-tutorial-composite-entity.md)

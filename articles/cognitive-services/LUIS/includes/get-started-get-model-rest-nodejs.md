---
title: Obter o modelo com chamadas REST em C#
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 05/18/2020
ms.author: diberry
ms.openlocfilehash: 19f72dbb62fc2084bf0c9609fb3782e083c911af
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83655508"
---
## <a name="prerequisites"></a>Pré-requisitos

* Reconhecimento vocal do Azure – Criar chave de 32 caracteres do recurso e a URL de ponto de extremidade. Crie com o [portal do Azure](../luis-how-to-azure-subscription.md#create-resources-in-the-azure-portal) ou com a [CLI do Azure](../luis-how-to-azure-subscription.md#create-resources-in-azure-cli).
* Importe o aplicativo [Pizza](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/luis/apps/pizza-with-machine-learned-entity.json) do repositório do GitHub `Azure-Samples/cognitive-services-sample-data-files`.
* ID do aplicativo LUIS para o aplicativo Pizza importado. A ID do aplicativo é mostrada no painel do aplicativo.
* A ID da versão no aplicativo que recebe os enunciados.
* Linguagem de programação [Node.js](https://nodejs.org/)
* [Visual Studio Code](https://code.visualstudio.com/)

## <a name="example-utterances-json-file"></a>Exemplo de arquivo JSON de enunciados

[!INCLUDE [Quickstart explanation of example utterance JSON file](get-started-get-model-json-example-utterances.md)]

## <a name="create-the-nodejs-project"></a>Criar o projeto do Node.js

1. Crie uma pasta para armazenar seu projeto Node.js, tal como `node-model-with-rest`.

1. Abra um novo prompt de comando, navegue até a pasta que você criou e execute o seguinte comando:

    ```console
    npm init
    ```

    Pressione Enter em cada prompt para aceitar as configurações padrão.

1. Instale o módulo solicitação-promessa inserindo o seguinte comando:

    ```console
    npm install --save request-promise
    ```

## <a name="change-model-programmatically"></a>Alterar o modelo de maneira programática

1. Crie um arquivo chamado `model.js`. Adicione os códigos a seguir:

    ```javascript
    var request = require('request-promise');

    //////////
    // Values to modify.

    // YOUR-APP-ID: The App ID GUID found on the www.luis.ai Application Settings page.
    const LUIS_appId = "YOUR-APP-ID";

    // YOUR-AUTHORING-KEY: Your LUIS authoring key, 32 character value.
    const LUIS_authoringKey = "YOUR-AUTHORING-KEY";

    // YOUR-AUTHORING-ENDPOINT: Replace this with your authoring key endpoint.
    // For example, "https://your-resource-name.api.cognitive.microsoft.com/"
    const LUIS_endpoint = "YOUR-AUTHORING-ENDPOINT";

    // NOTE: Replace this your version number. The Pizza app uses a version number of "0.1".
    const LUIS_versionId = "0.1";
    //////////

    const addUtterancesURI = `${LUIS_endpoint}luis/authoring/v3.0-preview/apps/${LUIS_appId}/versions/${LUIS_versionId}/examples`;
    const addTrainURI = `${LUIS_endpoint}luis/authoring/v3.0-preview/apps/${LUIS_appId}/versions/${LUIS_versionId}/train`;

    const utterances = [
        {
            'text': 'order a pizza',
            'intentName': 'ModifyOrder',
            'entityLabels': [
                {
                    'entityName': 'Order',
                    'startCharIndex': 6,
                    'endCharIndex': 12
                }
            ]
        },
        {
            'text': 'order a large pepperoni pizza',
            'intentName': 'ModifyOrder',
            'entityLabels': [
                {
                    'entityName': 'Order',
                    'startCharIndex': 6,
                    'endCharIndex': 28
                },
                {
                    'entityName': 'FullPizzaWithModifiers',
                    'startCharIndex': 6,
                    'endCharIndex': 28
                },
                {
                    'entityName': 'PizzaType',
                    'startCharIndex': 14,
                    'endCharIndex': 28
                },
                {
                    'entityName': 'Size',
                    'startCharIndex': 8,
                    'endCharIndex': 12
                }
            ]
        },
        {
            'text': 'I want two large pepperoni pizzas on thin crust',
            'intentName': 'ModifyOrder',
            'entityLabels': [
                {
                    'entityName': 'Order',
                    'startCharIndex': 7,
                    'endCharIndex': 46
                },
                {
                    'entityName': 'FullPizzaWithModifiers',
                    'startCharIndex': 7,
                    'endCharIndex': 46
                },
                {
                    'entityName': 'PizzaType',
                    'startCharIndex': 17,
                    'endCharIndex': 32
                },
                {
                    'entityName': 'Size',
                    'startCharIndex': 11,
                    'endCharIndex': 15
                },
                {
                    'entityName': 'Quantity',
                    'startCharIndex': 7,
                    'endCharIndex': 9
                },
                {
                    'entityName': 'Crust',
                    'startCharIndex': 37,
                    'endCharIndex': 46
                }
            ]
        }
    ];

    // Main function.
    const main = async() =>{

        await addUtterances(utterances);
        await train("POST");
        await train("GET");

    }

    // Adds the utterances to the model.
    const addUtterances = async (utterances) => {

        const options = {
            uri: addUtterancesURI,
            method: 'POST',
            headers: {
                'Ocp-Apim-Subscription-Key': LUIS_authoringKey
            },
            json: true,
            body: utterances
        };

        const response = await request(options)
        console.log("addUtterance:\n" + JSON.stringify(response, null, 2));
    }

    // With verb === "POST", sends a training request.
    // With verb === "GET", obtains the training status.
    const train = async (verb) => {

        const options = {
            uri: addTrainURI,
            method: verb,
            headers: {
                'Ocp-Apim-Subscription-Key': LUIS_authoringKey
            },
            json: true,
            body: null // The body can be empty for a training request
        };

        const response = await request(options)
        console.log("train " + verb + ":\n" + JSON.stringify(response, null, 2));
    }

    // MAIN
    main().then(() => console.log("done")).catch((err)=> console.log(err));
    ```

1. Substitua os valores que começam com `YOUR-` por seus valores.

    |Informações|Finalidade|
    |--|--|
    |`YOUR-APP-ID`| Sua ID do aplicativo LUIS. |
    |`YOUR-AUTHORING-KEY`|Sua chave de criação de 32 caracteres.|
    |`YOUR-AUTHORING-ENDPOINT`| Seu ponto de extremidade da URL de criação. Por exemplo, `https://replace-with-your-resource-name.api.cognitive.microsoft.com/`. Você definiu o nome do recurso quando você criou o recurso.|

    As chaves e os recursos atribuídos estão visíveis no portal do LUIS na seção Gerenciar, na página **recursos do Azure**. A ID do aplicativo está disponível na mesma seção Gerenciar, na página **Configurações do Aplicativo**.

1. No prompt de comando, insira o seguinte comando para executar o projeto:

    ```console
    node model.js
    ```

## <a name="clean-up-resources"></a>Limpar os recursos

Ao concluir este início rápido, exclua a pasta do projeto do sistema de arquivos.

## <a name="next-steps"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Práticas recomendadas para um aplicativo](../luis-concept-best-practices.md)
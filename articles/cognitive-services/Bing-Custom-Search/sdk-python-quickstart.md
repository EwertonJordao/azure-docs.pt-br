---
title: 'Início Rápido: Chamar o ponto de extremidade da Pesquisa Personalizada do Bing usando o SDK do Python'
titleSuffix: Azure Cognitive Services
description: Use este início rápido para começar a solicitar os resultados da pesquisa na instância da Pesquisa Personalizada do Bing, usando o SDK do Python.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-custom-search
ms.topic: quickstart
ms.date: 12/09/2019
ms.author: aahi
ms.openlocfilehash: d67075fad719b1780682c705f0e17f15c5801559
ms.sourcegitcommit: b95983c3735233d2163ef2a81d19a67376bfaf15
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/11/2020
ms.locfileid: "77136160"
---
# <a name="quickstart-call-your-bing-custom-search-endpoint-using-the-python-sdk"></a>Início Rápido: Chamar o ponto de extremidade da Pesquisa Personalizada do Bing usando o SDK do Python 

Use este início rápido para começar a solicitar os resultados da pesquisa na instância da Pesquisa Personalizada do Bing, usando o SDK do Python. Embora a Pesquisa Personalizada do Bing tenha uma API REST compatível com a maioria das linguagens de programação, o SDK da Pesquisa Personalizada do Bing fornece uma maneira fácil de integrar o serviço aos seus aplicativos. O código-fonte desse exemplo pode ser encontrado no [GitHub](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples/blob/master/samples/search/custom_search_samples.py) com anotações e tratamento de erro adicionais.

## <a name="prerequisites"></a>Pré-requisitos

- Uma instância da Pesquisa Personalizada do Bing. Confira [Início Rápido: Criar sua primeira instância da Pesquisa Personalizada do Bing](quick-start.md) para obter mais informações.
- Python [2.x ou 3.x](https://www.python.org/) 
- O [SDK da Pesquisa Personalizada do Bing para Python](https://pypi.org/project/azure-cognitiveservices-search-customsearch/) 

## <a name="install-the-python-sdk"></a>Instalar o SDK do Python

Instale o SDK da Pesquisa Personalizada do Bing com o comando a seguir.

```Console
python -m pip install azure-cognitiveservices-search-customsearch
```


## <a name="create-a-new-application"></a>Crie um novo aplicativo

Crie um novo arquivo Python em seu IDE ou editor favorito e adicione as importações a seguir.

```python
from azure.cognitiveservices.search.customsearch import CustomSearchClient
from msrest.authentication import CognitiveServicesCredentials
```

## <a name="create-a-search-client-and-send-a-request"></a>Criar um cliente de pesquisa e enviar uma solicitação

1. Criar uma variável para a chave de assinatura.

    ```python
    subscription_key = 'your-subscription-key'
    endpoint = 'your-custom-endpoint'
    ```

2. Crie uma instância de `CustomSearchClient` usando um objeto `CognitiveServicesCredentials` com a chave da assinatura. 

    ```python
    client = CustomSearchClient(endpoint, CognitiveServicesCredentials(subscription_key))
    ```

3. Envie uma solicitação de pesquisa com `client.custom_instance.search()`. Acrescente o termo de pesquisa ao parâmetro `query` e defina `custom_config` para a sua ID de configuração personalizada para usar a instância de pesquisa. Você pode obter sua ID do [portal da Pesquisa Personalizada do Bing](https://www.customsearch.ai/), clicando na guia **Produção**.

    ```python
    web_data = client.custom_instance.search(query="xbox", custom_config="your-configuration-id")
    ```

## <a name="view-the-search-results"></a>Exibir os resultados da pesquisa

Se resultados da pesquisa de páginas da Web foram encontrados, obtenha o primeiro deles e imprima seu nome, a URL e o total de páginas da Web encontradas.

```python
if web_data.web_pages.value:
    first_web_result = web_data.web_pages.value[0]
    print("Web Pages result count: {}".format(len(web_data.web_pages.value)))
    print("First Web Page name: {}".format(first_web_result.name))
    print("First Web Page url: {}".format(first_web_result.url))
else:
    print("Didn't see any web data..")
```

## <a name="next-steps"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Criar um aplicativo Web de Pesquisa Personalizada](./tutorials/custom-search-web-page.md)

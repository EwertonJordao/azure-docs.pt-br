---
title: 'Início Rápido: Executar uma pesquisa de notícias usando Python e a API REST de Pesquisa de Notícias do Bing'
titleSuffix: Azure Cognitive Services
description: Use este Início Rápido para enviar uma solicitação para a API REST de Pesquisa de Notícias do Bing usando Python e receber uma resposta JSON.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-news-search
ms.topic: quickstart
ms.date: 12/12/2019
ms.author: aahi
ms.custom: seodec2018
ms.openlocfilehash: 1c424c75a4df193ec412355607c68abeda0560a5
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75448488"
---
# <a name="quickstart-perform-a-news-search-using-python-and-the-bing-news-search-rest-api"></a>Início Rápido: Executar uma pesquisa de notícias usando Python e a API REST de Pesquisa de Notícias do Bing

Use este início rápido para fazer sua primeira chamada à API de Pesquisa de Notícias do Bing e receber uma resposta JSON. Este aplicativo JavaScript simples envia uma consulta de pesquisa para a API e processa os resultados. Embora esse aplicativo seja escrito no Python, a API é um serviço Web RESTful compatível com a maioria das linguagens de programação.

Execute este exemplo de código como um Jupyter notebook em [MyBinder](https://mybinder.org) clicando no selo launch Binder: 

[![Associador](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=BingNewsSearchAPI.ipynb)

O código-fonte dessa amostra também está disponível no [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/python/Search/BingNewsSearchv7.py).

## <a name="prerequisites"></a>Pré-requisitos

[!INCLUDE [cognitive-services-bing-news-search-signup-requirements](../../../includes/cognitive-services-bing-news-search-signup-requirements.md)]

## <a name="create-and-initialize-the-application"></a>Criar e inicializar o aplicativo

1. Crie um arquivo do Python em seu IDE ou editor favorito e importe o módulo de solicitação. Crie variáveis para a chave de assinatura, um ponto de extremidade e um termo de pesquisa. Você pode usar o ponto de extremidade global abaixo ou o ponto de extremidade de [subdomínio personalizado](../../cognitive-services/cognitive-services-custom-subdomains.md) exibido no portal do Azure para seu recurso.

```python
import requests

subscription_key = "your subscription key"
search_term = "Microsoft"
search_url = "https://api.cognitive.microsoft.com/bing/v7.0/news/search"
```

### <a name="create-parameters-for-the-request"></a>Criar parâmetros para a solicitação

1. Adicione a chave de assinatura a um novo dicionário usando `"Ocp-Apim-Subscription-Key"` como a chave. Faça o mesmo para os parâmetros de pesquisa.

    ```python
    headers = {"Ocp-Apim-Subscription-Key" : subscription_key}
    params  = {"q": search_term, "textDecorations": True, "textFormat": "HTML"}
    ```

## <a name="send-a-request-and-get-a-response"></a>Enviar uma solicitação e obter uma resposta

1. Use a biblioteca de solicitações para chamar a API da Pesquisa Visual do Bing usando a chave de assinatura e os objetos de dicionário criados na última etapa.

    ```python
    response = requests.get(search_url, headers=headers, params=params)
    response.raise_for_status()
    search_results = response.json()
    ```

2. `search_results` contém a resposta da API como um objeto JSON. Acesse as descrições dos artigos contidos na resposta.
    
    ```python
    descriptions = [article["description"] for article in search_results["value"]]
    ```

## <a name="displaying-the-results"></a>Exibindo os resultados

Essas descrições podem então ser renderizadas como uma tabela com a palavra-chave de pesquisa realçada em **negrito**.

```python
from IPython.display import HTML
rows = "\n".join(["<tr><td>{0}</td></tr>".format(desc)
                  for desc in descriptions])
HTML("<table>"+rows+"</table>")
```

## <a name="next-steps"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Criar um aplicativo Web de página única](tutorial-bing-news-search-single-page-app.md)

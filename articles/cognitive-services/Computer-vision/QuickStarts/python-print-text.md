---
title: 'Início Rápido: Extrair texto impresso – REST, Python'
titleSuffix: Azure Cognitive Services
description: Neste início rápido, você extrairá o texto impresso de uma imagem usando a API de Pesquisa Visual Computacional com Python.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: quickstart
ms.date: 04/14/2020
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 9a88112fbd0838bc554f97cf4d0e731839c71af8
ms.sourcegitcommit: 50673ecc5bf8b443491b763b5f287dde046fdd31
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/20/2020
ms.locfileid: "83684106"
---
# <a name="quickstart-extract-printed-text-ocr-using-the-computer-vision-rest-api-and-python"></a>Início Rápido: Extrair um texto impresso (OCR) usando a API REST da Pesquisa Visual Computacional e o Python

> [!NOTE]
> Se você estiver extraindo texto no idioma inglês, considere o uso da nova [Operação de leitura](https://docs.microsoft.com/azure/cognitive-services/computer-vision/concept-recognizing-text). Há um [Início Rápido do Python](https://docs.microsoft.com/azure/cognitive-services/computer-vision/quickstarts/python-hand-text) disponível. 

Neste início rápido, você extrairá texto impresso com o OCR (reconhecimento óptico de caracteres) de uma imagem usando a API REST da Pesquisa Visual Computacional. Com o método [OCR](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fc), você pode detectar texto impresso em uma imagem e extrair os caracteres reconhecidos em um fluxo de caracteres utilizável por computador.

Você pode executar este início rápido passo a passo usando um Jupyter Notebook em [MyBinder](https://mybinder.org). Para inicializar o Associador, selecione o botão a seguir:

[![Associador](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=VisionAPI.ipynb)

Se você não tiver uma assinatura do Azure, crie uma [conta gratuita](https://azure.microsoft.com/try/cognitive-services/) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

- Será necessário ter o [Python](https://www.python.org/downloads/) instalado se quiser executar o exemplo localmente.
- Você precisa ter uma chave de assinatura para a Pesquisa Visual Computacional. É possível obter uma chave de avaliação gratuita em [Experimente os Serviços Cognitivos](https://azure.microsoft.com/try/cognitive-services/?api=computer-vision). Ou siga as instruções em [Criar uma conta dos Serviços Cognitivos](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) para assinar a Pesquisa Visual Computacional e obter sua chave. Em seguida, [crie variáveis de ambiente](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) para a chave e a cadeia de caracteres do ponto de extremidade de serviço, denominadas `COMPUTER_VISION_SUBSCRIPTION_KEY` e `COMPUTER_VISION_ENDPOINT`, respectivamente.

## <a name="create-and-run-the-sample"></a>Criar e executar o exemplo

Para criar e executar o exemplo, siga estas etapas:

1. Copie o código a seguir em um editor de texto.
1. Outra opção é substituir o valor de `image_url` pela URL de uma imagem diferente da qual você deseja extrair texto impresso.
1. Salve o código como um arquivo com uma extensão `.py`. Por exemplo, `get-printed-text.py`.
1. Abra una janela de prompt de comando.
1. No prompt, use o comando `python` para executar o exemplo. Por exemplo, `python get-printed-text.py`.

```python
import os
import sys
import requests
# If you are using a Jupyter notebook, uncomment the following line.
# %matplotlib inline
import matplotlib.pyplot as plt
from matplotlib.patches import Rectangle
from PIL import Image
from io import BytesIO

# Add your Computer Vision subscription key and endpoint to your environment variables.
if 'COMPUTER_VISION_SUBSCRIPTION_KEY' in os.environ:
    subscription_key = os.environ['COMPUTER_VISION_SUBSCRIPTION_KEY']
else:
    print("\nSet the COMPUTER_VISION_SUBSCRIPTION_KEY environment variable.\n**Restart your shell or IDE for changes to take effect.**")
    sys.exit()

if 'COMPUTER_VISION_ENDPOINT' in os.environ:
    endpoint = os.environ['COMPUTER_VISION_ENDPOINT']

ocr_url = endpoint + "vision/v3.0/ocr"

# Set image_url to the URL of an image that you want to analyze.
image_url = "https://upload.wikimedia.org/wikipedia/commons/thumb/a/af/" + \
    "Atomist_quote_from_Democritus.png/338px-Atomist_quote_from_Democritus.png"

headers = {'Ocp-Apim-Subscription-Key': subscription_key}
params = {'language': 'unk', 'detectOrientation': 'true'}
data = {'url': image_url}
response = requests.post(ocr_url, headers=headers, params=params, json=data)
response.raise_for_status()

analysis = response.json()

# Extract the word bounding boxes and text.
line_infos = [region["lines"] for region in analysis["regions"]]
word_infos = []
for line in line_infos:
    for word_metadata in line:
        for word_info in word_metadata["words"]:
            word_infos.append(word_info)
word_infos

# Display the image and overlay it with the extracted text.
plt.figure(figsize=(5, 5))
image = Image.open(BytesIO(requests.get(image_url).content))
ax = plt.imshow(image, alpha=0.5)
for word in word_infos:
    bbox = [int(num) for num in word["boundingBox"].split(",")]
    text = word["text"]
    origin = (bbox[0], bbox[1])
    patch = Rectangle(origin, bbox[2], bbox[3],
                      fill=False, linewidth=2, color='y')
    ax.axes.add_patch(patch)
    plt.text(origin[0], origin[1], text, fontsize=20, weight="bold", va="top")
plt.show()
plt.axis("off")
```

## <a name="upload-image-from-local-storage"></a>Carregar imagem do armazenamento local

Se desejar analisar uma imagem local, defina o cabeçalho Content-Type como application/octet-stream e defina o corpo da solicitação como uma matriz de bytes em vez de dados JSON.

```python
image_path = "<path-to-local-image-file>"
# Read the image into a byte array
image_data = open(image_path, "rb").read()
# Set Content-Type to octet-stream
headers = {'Ocp-Apim-Subscription-Key': subscription_key, 'Content-Type': 'application/octet-stream'}
# put the byte array into your post request
response = requests.post(ocr_url, headers=headers, params=params, data = image_data)
```


## <a name="examine-the-response"></a>Examinar a resposta

Uma resposta com êxito é retornada em JSON. A página da Web de exemplo analisa e exibe uma resposta bem-sucedida na janela do prompt de comando, semelhante ao exemplo a seguir:

```json
{
  "language": "en",
  "orientation": "Up",
  "textAngle": 0,
  "regions": [
    {
      "boundingBox": "21,16,304,451",
      "lines": [
        {
          "boundingBox": "28,16,288,41",
          "words": [
            {
              "boundingBox": "28,16,288,41",
              "text": "NOTHING"
            }
          ]
        },
        {
          "boundingBox": "27,66,283,52",
          "words": [
            {
              "boundingBox": "27,66,283,52",
              "text": "EXISTS"
            }
          ]
        },
        {
          "boundingBox": "27,128,292,49",
          "words": [
            {
              "boundingBox": "27,128,292,49",
              "text": "EXCEPT"
            }
          ]
        },
        {
          "boundingBox": "24,188,292,54",
          "words": [
            {
              "boundingBox": "24,188,292,54",
              "text": "ATOMS"
            }
          ]
        },
        {
          "boundingBox": "22,253,297,32",
          "words": [
            {
              "boundingBox": "22,253,105,32",
              "text": "AND"
            },
            {
              "boundingBox": "144,253,175,32",
              "text": "EMPTY"
            }
          ]
        },
        {
          "boundingBox": "21,298,304,60",
          "words": [
            {
              "boundingBox": "21,298,304,60",
              "text": "SPACE."
            }
          ]
        },
        {
          "boundingBox": "26,387,294,37",
          "words": [
            {
              "boundingBox": "26,387,210,37",
              "text": "Everything"
            },
            {
              "boundingBox": "249,389,71,27",
              "text": "else"
            }
          ]
        },
        {
          "boundingBox": "127,431,198,36",
          "words": [
            {
              "boundingBox": "127,431,31,29",
              "text": "is"
            },
            {
              "boundingBox": "172,431,153,36",
              "text": "opinion."
            }
          ]
        }
      ]
    }
  ]
}
```

## <a name="next-steps"></a>Próximas etapas

Em seguida, explore um aplicativo Python que usa a Pesquisa Visual Computacional para executar OCR (reconhecimento óptico de caracteres), crie miniaturas com recorte inteligente e detecte, categorize, marque e descreva recursos visuais em imagens.

> [!div class="nextstepaction"]
> [Tutorial do Python da API da Pesquisa Visual Computacional](../Tutorials/PythonTutorial.md)

* Para testar rapidamente a API da Pesquisa Visual Computacional, experimente o [Abrir o console de teste de API](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa/console).

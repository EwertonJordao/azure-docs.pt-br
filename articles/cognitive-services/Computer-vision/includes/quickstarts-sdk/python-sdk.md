---
title: 'Início Rápido: Biblioteca de clientes da Pesquisa Visual Computacional para Python'
description: Introdução à biblioteca de clientes da Pesquisa Visual Computacional para Python com este início rápido.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: include
ms.date: 12/05/2019
ms.author: pafarley
ms.openlocfilehash: 32a6d7bf481b903cde4a777a10059d8ef2968ccd
ms.sourcegitcommit: 55b2bbbd47809b98c50709256885998af8b7d0c5
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/18/2020
ms.locfileid: "85073360"
---
<a name="HOLTop"></a>

[Documentação de referência](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision) | [Código-fonte da biblioteca](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/cognitiveservices/azure-cognitiveservices-vision-computervision) | [Pacote (PiPy)](https://pypi.org/project/azure-cognitiveservices-vision-computervision/) | [Exemplos](https://azure.microsoft.com/resources/samples/?service=cognitive-services&term=vision&sort=0)

## <a name="prerequisites"></a>Pré-requisitos

* Uma assinatura do Azure – [crie uma gratuitamente](https://azure.microsoft.com/free/cognitive-services/)
* [Python 3.x](https://www.python.org/)
* Depois de obter sua assinatura do Azure, <a href="https://portal.azure.com/#create/Microsoft.CognitiveServicesComputerVision"  title="Crie um recurso da Pesquisa Visual Computacional"  target="_blank">crie um recurso da Pesquisa Visual Computacional <span class="docon docon-navigate-external x-hidden-focus"></span></a> no portal do Azure para obter a chave e o ponto de extremidade. Após a implantação, clique em **Ir para o recurso**.
    * Você precisará da chave e do ponto de extremidade do recurso criado para conectar seu aplicativo ao serviço de Pesquisa Visual Computacional. Cole a chave e o ponto de extremidade no código abaixo mais adiante no guia de início rápido.
    * Use o tipo de preço gratuito (`F0`) para experimentar o serviço e atualizar mais tarde para um nível pago para produção.
* [Crie variáveis de ambiente](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) para a chave e a URL do ponto de extremidade, chamados `COMPUTER_VISION_SUBSCRIPTION_KEY` e `COMPUTER_VISION_ENDPOINT`, respectivamente.

## <a name="setting-up"></a>Configurando
 
### <a name="create-a-new-python-application"></a>Criar um novo aplicativo Python

Crie um script do Python&mdash;*quickstart-file.py*, por exemplo. Em seguida, abra-o em seu editor ou IDE preferencial e importe as bibliotecas a seguir.

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_imports)]

Em seguida, crie variáveis para a chave e o ponto de extremidade do Azure do recurso.

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_vars)]

> [!NOTE]
> Se você criou a variável de ambiente depois de iniciar o aplicativo, será necessário fechar e recarregar o editor, o IDE ou o shell em execução para acessar a variável.

### <a name="install-the-client-library"></a>Instalar a biblioteca de clientes

É possível instalar a biblioteca de clientes com:

```console
pip install --upgrade azure-cognitiveservices-vision-computervision
```

## <a name="object-model"></a>Modelo de objeto

As seguintes classes e interfaces lidam com alguns dos principais recursos do SDK do Python da Pesquisa Visual Computacional.

|Nome|Descrição|
|---|---|
|[ComputerVisionClientOperationsMixin](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.operations.computervisionclientoperationsmixin?view=azure-python)| Essa classe manipula diretamente todas as operações de imagem, como análise de imagem, detecção de texto e geração de miniaturas.|
| [ComputerVisionClient](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.computervisionclient?view=azure-python) | Essa classe é necessária para toda a funcionalidade da Pesquisa Visual Computacional. Você a instancia com suas informações de assinatura e a usa para produzir instâncias de outras classes. Ela implementa **ComputerVisionClientOperationsMixin**.|
|[VisualFeatureTypes](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.models.visualfeaturetypes?view=azure-python)| Essa enumeração define os diferentes tipos de análise de imagem que podem ser feitos em uma operação de análise padrão. Especifique um conjunto de valores **VisualFeatureTypes** dependendo de suas necessidades. |

## <a name="code-examples"></a>Exemplos de código

Esses snippets de códigos mostram como executar as seguintes tarefas com a biblioteca de clientes da Pesquisa Visual Computacional para Python:

* [Autenticar o cliente](#authenticate-the-client)
* [Analisar uma imagem](#analyze-an-image)
* [Ler texto impresso e manuscrito](#read-printed-and-handwritten-text)

## <a name="authenticate-the-client"></a>Autenticar o cliente

> [!NOTE]
> Este guia de início rápido pressupõe que você já [criou uma variável de ambiente](../../../cognitive-services-apis-create-account.md#configure-an-environment-variable-for-authentication) para a chave da Pesquisa Visual Computacional, denominada `COMPUTER_VISION_SUBSCRIPTION_KEY`.

Instancie um cliente com o ponto de extremidade e a chave. Crie um objeto [CognitiveServicesCredentials](https://docs.microsoft.com/python/api/msrest/msrest.authentication.cognitiveservicescredentials?view=azure-python) com sua chave e use-o com seu ponto de extremidade para criar um objeto [ComputerVisionClient](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.computervisionclient?view=azure-python).

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_client)]

## <a name="analyze-an-image"></a>Analisar uma imagem

Salve uma referência à URL de uma imagem que você deseja analisar.

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_remoteimage)]

### <a name="get-image-description"></a>Obter a descrição da imagem

O código a seguir obtém a lista de legendas geradas para a imagem. Confira [Descrever imagens](../../concept-describing-images.md) para obter mais detalhes.

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_describe)]

### <a name="get-image-category"></a>Obter categoria da imagem

O código a seguir obtém a categoria detectada da imagem. Confira [Categorizar imagens](../../concept-categorizing-images.md) para obter detalhes.

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_categorize)]

### <a name="get-image-tags"></a>Obter marcas da imagem

O código a seguir obtém o conjunto de marcas detectadas na imagem. Confira [Marcas de conteúdo](../../concept-tagging-images.md) para obter mais detalhes.

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_tags)]

### <a name="detect-objects"></a>Detectar objetos

O código a seguir detecta objetos comuns na imagem e imprime-os no console. Confira [Detecção de objetos](../../concept-object-detection.md) para obter mais detalhes.

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_objects)]

### <a name="detect-brands"></a>Detectar marcas

O código a seguir detecta logotipos e marcas corporativas na imagem e imprime-os no console. Confira [Detecção de marca](../../concept-brand-detection.md) para obter mais detalhes.

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_brands)]

### <a name="detect-faces"></a>Detectar faces

O código a seguir retorna os rostos detectados na imagem com suas coordenadas de retângulo e atributos de rosto selecionados. Confira [Detecção facial](../../concept-detecting-faces.md) para obter mais detalhes.

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_faces)]

### <a name="detect-adult-racy-or-gory-content"></a>Detectar conteúdo para adulto, erótico ou violento

O código a seguir imprime a presença detectada de conteúdo para adulto na imagem. Confira [Conteúdo para adulto, erótico e violento](../../concept-detecting-adult-content.md) para obter mais detalhes.

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_adult)]

### <a name="get-image-color-scheme"></a>Obter esquema de cores da imagem

O código a seguir imprime os atributos de cor detectados na imagem, como as cores dominantes e a cor de destaque. Confira [Esquemas de cores](../../concept-detecting-color-schemes.md) para obter mais detalhes.

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_color)]

### <a name="get-domain-specific-content"></a>Obter conteúdo específico do domínio

A Pesquisa Visual Computacional pode usar o modelo especializado para realizar análise adicional em imagens. Confira [Conteúdo específico do domínio](../../concept-detecting-domain-content.md) para obter mais detalhes. 

O código a seguir analisa dados sobre celebridades detectados na imagem.

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_celebs)]

O código a seguir analisa dados sobre pontos de referência detectados na imagem.

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_landmarks)]

### <a name="get-the-image-type"></a>Obter o tipo de imagem

O código a seguir imprime informações sobre o tipo de imagem – seja clip-art ou desenho de linha.

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_type)]

## <a name="read-printed-and-handwritten-text"></a>Ler texto impresso e manuscrito

A Pesquisa Visual Computacional pode ler um texto visível em uma imagem e convertê-lo em um fluxo de caracteres. Faça isso em duas partes.

### <a name="call-the-read-api"></a>Chamar a API de Leitura

Primeiro, use o código a seguir para chamar o método **read** para a imagem especificada. Isso retorna uma ID de operação e inicia um processo assíncrono para ler o conteúdo da imagem.

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_read_call)]

### <a name="get-read-results"></a>Obter os resultados da leitura

Em seguida, obtenha a ID da operação retornada da chamada **read** e use-a para consultar o serviço para obter os resultados da operação. O código a seguir verifica a operação em intervalos de um segundo até que os resultados sejam retornados. Em seguida, ele imprime os dados de texto extraídos no console.

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_read_response)]

## <a name="run-the-application"></a>Executar o aplicativo

Execute o aplicativo com o comando `python` no seu arquivo de início rápido.

```console
python quickstart-file.py
```

## <a name="clean-up-resources"></a>Limpar os recursos

Se quiser limpar e remover uma assinatura dos Serviços Cognitivos, você poderá excluir o recurso ou grupo de recursos. Excluir o grupo de recursos também exclui todos os recursos associados a ele.

* [Portal](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#clean-up-resources)
* [CLI do Azure](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account-cli#clean-up-resources)


## <a name="next-steps"></a>Próximas etapas

Neste início rápido, você aprendeu a usar a biblioteca da Pesquisa Visual Computacional para Python para realizar tarefas básicas. Em seguida, explore a documentação de referência para saber mais sobre a biblioteca.


> [!div class="nextstepaction"]
>[Referência da API da Pesquisa Visual Computacional (Python)](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision)

* [O que é a Pesquisa Visual Computacional?](../../Home.md)
* O código-fonte desta amostra pode ser encontrado no [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/python/ComputerVision/ComputerVisionQuickstart.py).

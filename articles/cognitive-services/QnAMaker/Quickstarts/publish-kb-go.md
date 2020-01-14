---
title: 'Início Rápido: Publicar base de dados de conhecimento, REST, Go – QnA Maker'
titleSuffix: Azure Cognitive Services
description: Este início rápido baseado na REST Go publica a base de dados de conhecimento e cria um ponto de extremidade que pode ser chamado no aplicativo ou no chatbot.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: quickstart
ms.date: 12/16/2019
ms.author: diberry
ms.openlocfilehash: c66f4f2b126b3b2b86ea55112d1bdc79b4bf4571
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75447434"
---
# <a name="quickstart-publish-a-knowledge-base-in-qna-maker-using-go"></a>Início Rápido: Publicar uma base de dados de conhecimento no QnA Maker usando Go

Esse início rápido baseado em REST orienta você durante a publicação programática de sua KB (base de dados de conhecimento). A publicação envia por push a versão mais recente da base de dados de conhecimento para um índice dedicado da Azure Cognitive Search e cria um ponto de extremidade que pode ser chamado no aplicativo ou no chatbot.

Este início rápido chama as APIs de QnA Maker:
* [Publicar](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/publish) – esta API não requer nenhuma informação no corpo da solicitação.

## <a name="prerequisites"></a>Prerequisites

* [Go 1.10.1](https://golang.org/dl/)
* Você precisa ter um [serviço QnA Maker](../How-To/set-up-qnamaker-service-azure.md). Para recuperar a chave e o ponto de extremidade (que inclui o nome do recurso), selecione **Início Rápido** para o recurso no portal do Azure.

* A ID da KB (base de dados de conhecimento) do QnA Maker encontrada na URL no parâmetro de cadeia de caracteres de consulta `kbid`, conforme mostrada abaixo.

    ![ID da base de dados de conhecimento do QnA Maker](../media/qnamaker-quickstart-kb/qna-maker-id.png)

    Se ainda não tiver uma base de dados de conhecimento, você poderá criar uma de exemplo para usar neste início rápido: [Criar uma nova base de dados de conhecimento](create-new-kb-csharp.md).

> [!NOTE]
> Os arquivos da solução completa estão disponíveis no repositório GitHub [**Azure-Samples/cognitive-services-qnamaker-go**](https://github.com/Azure-Samples/cognitive-services-qnamaker-go/tree/master/documentation-samples/quickstarts/publish-knowledge-base).

## <a name="create-a-go-file"></a>Criar um arquivo Go

Abra o VSCode e crie um novo arquivo denominado `publish-kb.go`.

## <a name="add-the-required-dependencies"></a>Adicione as dependências necessárias

Na parte superior de `publish-kb.go`, adicione as seguintes linhas para adicionar as dependências necessárias ao projeto:

[!code-go[Add the required dependencies](~/samples-qnamaker-go/documentation-samples/quickstarts/publish-knowledge-base/publish-kb.go?range=3-7 "Add the required dependencies")]

## <a name="create-the-main-function"></a>Criar a função principal

Após as dependências exigidas, adicione a classe a seguir:

```Go
package main

func main() {

}
```

## <a name="add-required-constants"></a>Adicionar constantes necessárias

Dentro da função **principal**,


 adicione as constantes necessárias para acessar o QnA Maker. Substitua os valores pelos seus próprios.

[!code-go[Add the required constants](~/samples-qnamaker-go/documentation-samples/quickstarts/publish-knowledge-base/publish-kb.go?range=16-20 "Add the required constants")]

## <a name="add-post-request-to-publish-kb"></a>Adicionar solicitação POST para publicar base de dados de conhecimento

Após as constantes necessárias, adicione o código a seguir, o qual faz uma solicitação HTTPS à API de QnA Maker para publicar uma base de dados de conhecimento e recebe a resposta:

[!code-go[Add a POST request to publish KB](~/samples-qnamaker-go/documentation-samples/quickstarts/get-answer/get-answer.go?range=35-48 "Add a POST request to publish KB")]

A chamada à API retorna um status 204 de publicação bem-sucedida sem nenhum conteúdo no corpo da resposta. O código adiciona conteúdo para respostas 204.

Para qualquer outra resposta, essa resposta é retornada inalterada.

## <a name="build-and-run-the-program"></a>Compilar e executar o programa

Insira o comando a seguir para compilar o arquivo. O prompt de comando não retorna nenhuma informação para um build bem-sucedido.

```bash
go build publish-kb.go
```

Insira o comando a seguir em uma linha de comando para executar o programa. Ele envia a solicitação à API do QnA Maker para publicar a base de dados de conhecimento, depois imprime 204 em caso de êxito ou erros.

```bash
./publish-kb
```

[!INCLUDE [Clean up files and knowledge base](../../../../includes/cognitive-services-qnamaker-quickstart-cleanup-resources.md)]

## <a name="next-steps"></a>Próximas etapas

Após a publicação da base de dados de conhecimento, você precisa da [URL do ponto de extremidade para gerar uma resposta](../Tutorials/create-publish-answer.md#generating-an-answer).

> [!div class="nextstepaction"]
> [Referência da API REST do QnA Maker (V4)](https://go.microsoft.com/fwlink/?linkid=2092179)

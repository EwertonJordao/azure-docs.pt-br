---
title: Melhorar a base de dados de conhecimento – QnA Maker
titleSuffix: Azure Cognitive Services
description: Melhore a qualidade de sua base de dados de conhecimento com o aprendizado ativo. Revisar, aceitar ou rejeitar, adicionar sem remover ou alterar as perguntas existentes.
author: diberry
manager: nitinme
services: cognitive-services
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 02/27/2020
ms.author: diberry
ms.openlocfilehash: dea2bf3b34ca336f3932dd85bf587184ab6881db
ms.sourcegitcommit: 3c925b84b5144f3be0a9cd3256d0886df9fa9dc0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/28/2020
ms.locfileid: "77914986"
---
# <a name="use-active-learning-to-improve-your-knowledge-base"></a>Usar o aprendizado ativo para melhorar sua base de dados de conhecimento

O [aprendizado ativo](../Concepts/active-learning-suggestions.md) permite que você aprimore a qualidade de sua base de dados de conhecimento sugerindo perguntas alternativas, com base em envios de usuários, para seu par de perguntas e respostas. Você examina essas sugestões, adicionando-as às perguntas existentes ou rejeitando-as.

Sua base de dados de conhecimento não é alterada automaticamente. Para que qualquer alteração entre em vigor, você deve aceitar as sugestões. Essas sugestões adicionam perguntas, mas não alteram nem removem perguntas existentes.


## <a name="upgrade-your-runtime-version-to-use-active-learning"></a>Atualizar sua versão de tempo de execução para usar o aprendizado ativo

O aprendizado ativo é compatível com a versão de runtime 4.4.0 e superior. Se sua base de dados de conhecimento foi criada em uma versão anterior, [atualize seu runtime](set-up-qnamaker-service-azure.md#get-the-latest-runtime-updates) para usar esse recurso.

## <a name="turn-on-active-learning-to-see-suggestions"></a>Ativar o aprendizado ativo para ver sugestões

O aprendizado ativo está desativado por padrão. Ative-o para ver sugestões de perguntas. Depois de ativar o aprendizado ativo, você precisa enviar informações do aplicativo cliente para QnA Maker. Para obter mais informações, consulte [o fluxo arquitetônico para usar GenerateAnswer e treinar APIs de um bot](#architectural-flow-for-using-generateanswer-and-train-apis-from-a-bot).

1. Selecione **publicar** para publicar a base de dados de conhecimento. Consultas de aprendizado ativo são coletadas somente do ponto de extremidade de previsão da API GenerateAnswer. As consultas ao painel de teste no portal de QnA Maker não afetam o aprendizado ativo.

1. Para ativar o aprendizado ativo no portal de QnA Maker, vá para o canto superior direito, selecione seu **nome**, vá para [**configurações de serviço**](https://www.qnamaker.ai/UserSettings).

    ![Ative as alternativas de perguntas sugeridas do aprendizado ativo na página Configurações de serviço. Selecione seu nome de usuário no menu superior direito e, em seguida, selecione configurações de serviço.](../media/improve-knowledge-base/Endpoint-Keys.png)


1. Localize o serviço QnA Maker e, em seguida, ative o **aprendizado ativo**.

    > [!div class="mx-imgBorder"]
    > [![na página Configurações de serviço, alterne o recurso de aprendizado ativo. Se não for possível alternar o recurso, talvez seja necessário atualizar seu serviço.](../media/improve-knowledge-base/turn-active-learning-on-at-service-setting.png)](../media/improve-knowledge-base/turn-active-learning-on-at-service-setting.png#lightbox)

    > [!Note]
    > A versão exata na imagem anterior é mostrada apenas como um exemplo. Sua versão pode ser diferente.

    Quando o **aprendizado ativo** é habilitado, a base de dados de conhecimento sugere novas perguntas em intervalos regulares com base em perguntas enviadas pelo usuário. Você pode desabilitar o **aprendizado ativo** desativando a configuração novamente.

## <a name="accept-an-active-learning-suggestion-in-the-knowledge-base"></a>Aceitar uma sugestão de aprendizado ativo na base de dados de conhecimento

O aprendizado ativo altera a base de dados de conhecimento ou Serviço de Pesquisa depois que você aprova a sugestão, em seguida, salva e treina. Se você aprovar a sugestão, ela será adicionada como uma pergunta alternativa.

1. Para ver as perguntas sugeridas, na página **Editar** base de dados de conhecimento, selecione **Opções de exibição**e, em seguida, selecione **Mostrar sugestões de aprendizado ativo**.

    [![na seção editar do portal, selecione Mostrar sugestões para ver as novas alternativas de pergunta do aprendizado ativo.](../media/improve-knowledge-base/show-suggestions-button.png)](../media/improve-knowledge-base/show-suggestions-button.png#lightbox)

1. Filtre a base de dados de conhecimento com pares de perguntas e respostas para mostrar apenas sugestões selecionando **Filtrar por sugestões**.

    [![use a alternância filtrar por sugestões para exibir apenas as alternativas de perguntas sugeridas do aprendizado ativo.](../media/improve-knowledge-base/filter-by-suggestions.png)](../media/improve-knowledge-base/filter-by-suggestions.png#lightbox)

1. Cada par de QnA sugere a nova pergunta alternativas com uma marca de seleção, `✔`, para aceitar a pergunta ou uma `x` para rejeitar as sugestões. Selecione a marca de seleção para adicionar a pergunta.

    [![selecionar ou rejeitar as alternativas de pergunta sugeridas do aprendizado ativo, selecionando a marca de seleção verde ou de exclusão vermelha.](../media/improve-knowledge-base/accept-active-learning-suggestions.png)](../media/improve-knowledge-base/accept-active-learning-suggestions.png#lightbox)

    Você pode adicionar ou excluir _todas as sugestões_ selecionando **Adicionar tudo** ou **rejeitar tudo** na barra de ferramentas contextual.

1. Selecione **Salvar e Treinar** para salvar as alterações à base de dados de conhecimento.

1. Selecione **publicar** para permitir que as alterações estejam disponíveis na [API GenerateAnswer](metadata-generateanswer-usage.md#generateanswer-request-configuration).

    Quando 5 ou mais consultas semelhantes são clusterizadas, a cada 30 minutos, QnA Maker sugere as perguntas alternativas para que você aceite ou rejeite.


<a name="#score-proximity-between-knowledge-base-questions"></a>

### <a name="architectural-flow-for-using-generateanswer-and-train-apis-from-a-bot"></a>Fluxo de arquitetura para usar GenerateAnswer e treinar APIs de um bot

Um bot ou outro aplicativo cliente deve usar o seguinte fluxo de arquitetura para usar o aprendizado ativo:

* O bot [Obtém a resposta da base de dados de conhecimento](#use-the-top-property-in-the-generateanswer-request-to-get-several-matching-answers) com a API GenerateAnswer, usando a propriedade `top` para obter várias respostas.
* O bot determina os comentários explícitos:
    * Usando sua própria [lógica de negócios personalizada](#use-the-score-property-along-with-business-logic-to-get-list-of-answers-to-show-user), filtre pontuações baixas.
    * No bot ou no aplicativo cliente, exiba a lista de possíveis respostas para o usuário e obtenha a resposta selecionada do usuário.
* O bot [envia a resposta selecionada de volta para QnA Maker](#bot-framework-sample-code) com a [API de treinamento](#train-api).


### <a name="use-the-top-property-in-the-generateanswer-request-to-get-several-matching-answers"></a>Use a propriedade Top na solicitação GenerateAnswer para obter várias respostas correspondentes

Ao enviar uma pergunta para QnA Maker para uma resposta, a propriedade `top` do corpo JSON define o número de respostas a serem retornadas.

```json
{
    "question": "wi-fi",
    "isTest": false,
    "top": 3
}
```

### <a name="use-the-score-property-along-with-business-logic-to-get-list-of-answers-to-show-user"></a>Use a propriedade Score junto com a lógica de negócios para obter a lista de respostas para mostrar o usuário

Quando o aplicativo cliente (como um bot de chat) recebe a resposta, as três principais perguntas são retornadas. Use a propriedade `score` para analisar a proximidade entre as pontuações. Esse intervalo de proximidade é determinado pela sua própria lógica de negócios.

```json
{
    "answers": [
        {
            "questions": [
                "Wi-Fi Direct Status Indicator"
            ],
            "answer": "**Wi-Fi Direct Status Indicator**\n\nStatus bar icons indicate your current Wi-Fi Direct connection status:  \n\nWhen your device is connected to another device using Wi-Fi Direct, '$  \n\n+ •+ ' Wi-Fi Direct is displayed in the Status bar.",
            "score": 74.21,
            "id": 607,
            "source": "Bugbash KB.pdf",
            "metadata": []
        },
        {
            "questions": [
                "Wi-Fi - Connections"
            ],
            "answer": "**Wi-Fi**\n\nWi-Fi is a term used for certain types of Wireless Local Area Networks (WLAN). Wi-Fi communication requires access to a wireless Access Point (AP).",
            "score": 74.15,
            "id": 599,
            "source": "Bugbash KB.pdf",
            "metadata": []
        },
        {
            "questions": [
                "Turn Wi-Fi On or Off"
            ],
            "answer": "**Turn Wi-Fi On or Off**\n\nTurning Wi-Fi on makes your device able to discover and connect to compatible in-range wireless APs.  \n\n1.  From a Home screen, tap ::: Apps > e Settings .\n2.  Tap Connections > Wi-Fi , and then tap On/Off to turn Wi-Fi on or off.",
            "score": 69.99,
            "id": 600,
            "source": "Bugbash KB.pdf",
            "metadata": []
        }
    ]
}
```

## <a name="client-application-follow-up-when-questions-have-similar-scores"></a>Acompanhamento do aplicativo cliente quando perguntas têm pontuações semelhantes

O aplicativo cliente exibe as perguntas com uma opção para que o usuário selecione _a única pergunta_ que mais representa sua intenção.

Depois que o usuário seleciona uma das perguntas existentes, o aplicativo cliente envia a opção do usuário como comentários usando a API de treinamento do QnA Maker. Este comentário conclui o loop de comentários do aprendizado ativo.

## <a name="train-api"></a>API de treinamento

Os comentários do aprendizado ativo são enviados para QnA Maker com a solicitação Train API POST. A assinatura de API é:

```http
POST https://<QnA-Maker-resource-name>.azurewebsites.net/qnamaker/knowledgebases/<knowledge-base-ID>/train
Authorization: EndpointKey <endpoint-key>
Content-Type: application/json
{"feedbackRecords": [{"userId": "1","userQuestion": "<question-text>","qnaId": 1}]}
```

|Propriedade de solicitação HTTP|{1&gt;Nome&lt;1}|Tipo|Finalidade|
|--|--|--|--|
|Parâmetro de rota de URL|ID da base de dados de conhecimento|string|o GUID da base de dados de conhecimento.|
|Subdomínio personalizado|Nome do recurso QnAMaker|string|O nome do recurso é usado como o subdomínio personalizado para seu QnA Maker. Isso estará disponível na página configurações depois que você publicar a base de dados de conhecimento. Ele é listado como o `host`.|
|Cabeçalho|Content-Type|string|o tipo de mídia do corpo enviado para a API. O valor padrão é: `application/json`|
|Cabeçalho|Autorização|string|sua chave de ponto de extremidade (EndpointKey xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx).|
|Corpo da postagem|Objeto JSON|JSON|Os comentários de treinamento|

O corpo JSON tem várias configurações:

|Propriedade de corpo JSON|Tipo|Finalidade|
|--|--|--|--|
|`feedbackRecords`|matriz|Lista de comentários.|
|`userId`|string|A ID de usuário da pessoa que está aceitando as perguntas sugeridas. O formato da ID de usuário cabe a você. Por exemplo, um endereço de email pode ser uma ID de usuário válida em sua arquitetura. Opcional.|
|`userQuestion`|string|Texto exato da consulta do usuário. Obrigatório.|
|`qnaID`|número|ID de pergunta, encontrada na [resposta de GenerateAnswer](metadata-generateanswer-usage.md#generateanswer-response-properties). |

Um exemplo de corpo JSON é semelhante a:

```json
{
    "feedbackRecords": [
        {
            "userId": "1",
            "userQuestion": "<question-text>",
            "qnaId": 1
        }
    ]
}
```

Uma resposta bem-sucedida retorna um status de 204 e nenhum corpo de resposta JSON.

### <a name="batch-many-feedback-records-into-a-single-call"></a>Lote de muitos registros de comentários em uma única chamada

No aplicativo do lado do cliente, como um bot, você pode armazenar os dados e, em seguida, enviar vários registros em um único corpo JSON na matriz de `feedbackRecords`.

Um exemplo de corpo JSON é semelhante a:

```json
{
    "feedbackRecords": [
        {
            "userId": "1",
            "userQuestion": "How do I ...",
            "qnaId": 1
        },
        {
            "userId": "2",
            "userQuestion": "Where is ...",
            "qnaId": 40
        },
        {
            "userId": "3",
            "userQuestion": "When do I ...",
            "qnaId": 33
        }
    ]
}
```



<a name="active-learning-is-saved-in-the-exported-apps-tsv-file"></a>

## <a name="bot-framework-sample-code"></a>Código de exemplo do bot Framework

O código do bot Framework precisa chamar a API de treinamento se a consulta do usuário deve ser usada para o aprendizado ativo. Há duas partes de código para escrever:

* Determinar se a consulta deve ser usada para o aprendizado ativo
* Enviar consulta de volta para a API de treinamento do QnA Maker para aprendizagem ativa

No [exemplo de bot do Azure](https://aka.ms/activelearningsamplebot), ambas as atividades foram programadas.

### <a name="example-c-code-for-train-api-with-bot-framework-4x"></a>Código C# de exemplo para a API de treinamento com o bot Framework 4. x

O código a seguir ilustra como enviar informações de volta para QnA Maker com a API de treinamento. Este [exemplo de código completo](https://github.com/microsoft/BotBuilder-Samples/tree/master/experimental/qnamaker-activelearning/csharp_dotnetcore) está disponível no github.

```csharp
public class FeedbackRecords
{
    // <summary>
    /// List of feedback records
    /// </summary>
    [JsonProperty("feedbackRecords")]
    public FeedbackRecord[] Records { get; set; }
}

/// <summary>
/// Active learning feedback record
/// </summary>
public class FeedbackRecord
{
    /// <summary>
    /// User id
    /// </summary>
    public string UserId { get; set; }

    /// <summary>
    /// User question
    /// </summary>
    public string UserQuestion { get; set; }

    /// <summary>
    /// QnA Id
    /// </summary>
    public int QnaId { get; set; }
}

/// <summary>
/// Method to call REST-based QnAMaker Train API for Active Learning
/// </summary>
/// <param name="endpoint">Endpoint URI of the runtime</param>
/// <param name="FeedbackRecords">Feedback records train API</param>
/// <param name="kbId">Knowledgebase Id</param>
/// <param name="key">Endpoint key</param>
/// <param name="cancellationToken"> Cancellation token</param>
public async static void CallTrain(string endpoint, FeedbackRecords feedbackRecords, string kbId, string key, CancellationToken cancellationToken)
{
    var uri = endpoint + "/knowledgebases/" + kbId + "/train/";

    using (var client = new HttpClient())
    {
        using (var request = new HttpRequestMessage())
        {
            request.Method = HttpMethod.Post;
            request.RequestUri = new Uri(uri);
            request.Content = new StringContent(JsonConvert.SerializeObject(feedbackRecords), Encoding.UTF8, "application/json");
            request.Headers.Add("Authorization", "EndpointKey " + key);

            var response = await client.SendAsync(request, cancellationToken);
            await response.Content.ReadAsStringAsync();
        }
    }
}
```

### <a name="example-nodejs-code-for-train-api-with-bot-framework-4x"></a>Código do node. js de exemplo para a API de treinamento com o bot Framework 4. x

O código a seguir ilustra como enviar informações de volta para QnA Maker com a API de treinamento. Este [exemplo de código completo](https://github.com/microsoft/BotBuilder-Samples/blob/master/experimental/qnamaker-activelearning/javascript_nodejs) está disponível no github.

```javascript
async callTrain(stepContext){

    var trainResponses = stepContext.values[this.qnaData];
    var currentQuery = stepContext.values[this.currentQuery];

    if(trainResponses.length > 1){
        var reply = stepContext.context.activity.text;
        var qnaResults = trainResponses.filter(r => r.questions[0] == reply);

        if(qnaResults.length > 0){

            stepContext.values[this.qnaData] = qnaResults;

            var feedbackRecords = {
                FeedbackRecords:[
                    {
                        UserId:stepContext.context.activity.id,
                        UserQuestion: currentQuery,
                        QnaId: qnaResults[0].id
                    }
                ]
            };

            // Call Active Learning Train API
            this.activeLearningHelper.callTrain(this.qnaMaker.endpoint.host, feedbackRecords, this.qnaMaker.endpoint.knowledgeBaseId, this.qnaMaker.endpoint.endpointKey);

            return await stepContext.next(qnaResults);
        }
        else{

            return await stepContext.endDialog();
        }
    }

    return await stepContext.next(stepContext.result);
}
```

## <a name="active-learning-is-saved-in-the-exported-knowledge-base"></a>O aprendizado ativo é salvo na base de dados de conhecimento exportada

Quando seu aplicativo tiver o aprendizado ativo habilitado e você exportar o aplicativo, a coluna `SuggestedQuestions` no arquivo TSV manterá os dados de aprendizado ativos.

A coluna `SuggestedQuestions` é um objeto JSON de informações de comentários implícitos, `autosuggested`e explícitos `usersuggested`. Um exemplo desse objeto JSON para uma única pergunta de `help` enviada pelo usuário é:

```JSON
[
    {
        "clusterHead": "help",
        "totalAutoSuggestedCount": 1,
        "totalUserSuggestedCount": 0,
        "alternateQuestionList": [
            {
                "question": "help",
                "autoSuggestedCount": 1,
                "userSuggestedCount": 0
            }
        ]
    }
]
```

Você também pode usar a API de alterações de download para examinar essas alterações, usando REST ou qualquer um dos SDKs baseados em linguagem:
* [REST API](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75fc)
* [SDK .NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.alterationsextensions.getasync?view=azure-dotnet)


Quando você Reimporta esse aplicativo, o aprendizado ativo continua a coletar informações e recomendar sugestões para sua base de dados de conhecimento.



## <a name="best-practices"></a>Práticas recomendadas

Para as práticas recomendadas ao usar o aprendizado ativo, veja [Práticas recomendadas](../Concepts/best-practices.md#active-learning).

## <a name="next-steps"></a>{1&gt;{2&gt;Próximas etapas&lt;2}&lt;1}

> [!div class="nextstepaction"]
> [Usar metadados com a API GenerateAnswer](metadata-generateanswer-usage.md)

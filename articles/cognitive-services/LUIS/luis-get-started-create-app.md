---
title: 'Início Rápido: criar aplicativo – LUIS'
description: Neste início rápido, crie um aplicativo LUIS que use o domínio predefinido `HomeAutomation` para ligar e desligar luzes e dispositivos. Este domínio predefinido fornece intenções, entidades e exemplos de enunciados a você. Quando terminar, você terá um ponto de extremidade do LUIS em execução na nuvem.
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: quickstart
ms.date: 05/05/2020
ms.openlocfilehash: 28bf79b61c0278a3f45820a23cd2c69f0b609700
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91316471"
---
# <a name="quickstart-use-prebuilt-home-automation-app"></a>Início Rápido: usar aplicativo de automação Inicial predefinido

Neste início rápido, crie um aplicativo LUIS que use o domínio predefinido `HomeAutomation` para ligar e desligar luzes e dispositivos. Este domínio predefinido fornece intenções, entidades e exemplos de enunciados a você. Quando terminar, você terá um ponto de extremidade do LUIS em execução na nuvem.

[!INCLUDE [Sign in to LUIS](./includes/sign-in-process.md)]

[!INCLUDE [Select authoring resource](./includes/select-authoring-resource.md)]

## <a name="create-a-new-app"></a>Criar um novo aplicativo
Você pode criar e gerenciar seus aplicativos em **Meus Aplicativos**.

1. Na lista Meus aplicativos, selecione **+ Novo aplicativo para conversa**, então na lista de opções, escolha **+ Novo aplicativo para conversa** novamente.

1. Na caixa de diálogo, dê um nome ao aplicativo `Home Automation`.
1. Selecione **Inglês** como a cultura.
1. Insira uma descrição opcional.
1. Não selecione um recurso de previsão se você ainda não criou o recurso. Para usar o ponto de extremidade de previsão do aplicativo (preparo ou produção), você precisa atribuir um recurso de previsão.
1. Selecione **Concluído**.

    LUIS cria o aplicativo.

    ![Na caixa de diálogo, dê ao seu aplicativo o nome `Automação Residencial`](./media/create-new-app-details.png)

    >[!NOTE]
    >A cultura não poderá ser alterada depois que o aplicativo for criado.

## <a name="add-prebuilt-domain"></a>Adicionar domínio predefinido

1. Na navegação à esquerda, selecione **Domínios predefinidos**.
1. Pesquise **Automação Inicial**.
1. Selecione **Adicionar domínio** no cartão HomeAutomation.

    > [!div class="mx-imgBorder"]
    > ![Selecione "Domínios predefinidos" e pesquise por "HomeAutomation". Selecione "Adicionar domínio" no cartão HomeAutomation.](media/luis-quickstart-new-app/home-automation.png)

    Quando o domínio for adicionado com êxito, a caixa de domínio predefinido exibirá um botão **Remover domínio**.

## <a name="intents-and-entities"></a>Intenções e entidades

1. Selecione **intenções** para examinar as intenções do domínio HomeAutomation. As intenções de domínio predefinidas têm enunciados de exemplo.

    > [!div class="mx-imgBorder"]
    > ![Captura de tela da lista de intenções de HomeAutomation](media/luis-quickstart-new-app/home-automation-intents.png "Captura de tela da lista de intenções de HomeAutomation")

    > [!NOTE]
    > **None** é uma intenção fornecida por todos os aplicativos LUIS. Você pode usá-la para lidar com enunciados que não correspondem à funcionalidade que seu aplicativo fornece.

1. Selecione a intenção **HomeAutomation.TurnOff**. A intenção contém uma lista de enunciados de exemplo rotulados com entidades.

    > [!div class="mx-imgBorder"]
    > [![Captura de tela da intenção HomeAutomation.TurnOff](media/luis-quickstart-new-app/home-automation-turnoff.png "Captura de tela da intenção HomeAutomation.TurnOff")](media/luis-quickstart-new-app/home-automation-turnoff.png)

## <a name="train-the-luis-app"></a>Treinar o aplicativo LUIS

[!INCLUDE [LUIS How to Train steps](includes/howto-train.md)]

## <a name="test-your-app"></a>Testar seu aplicativo
Depois de treinar o aplicativo, você pode testá-lo.

1. Selecione **Teste** no menu de navegação superior direito.

1. Digite um enunciado de teste, como `Turn off the lights` no painel de teste interativo e pressione Enter.

    ```
    Turn off the lights
    ```

    Neste exemplo, `Turn off the lights` é identificado corretamente como a intenção de pontuação superior de **HomeAutomation.TurnOff**.

    ![Captura de tela do painel de Teste com o enunciado realçado](media/luis-quickstart-new-app/review-test-inspection-pane-in-portal.png)

1. Selecione **Inspecionar** para exibir mais informações sobre a previsão.

    > [!div class="mx-imgBorder"]
    > ![Captura de tela do Painel de teste com informações de inspeção](media/luis-quickstart-new-app/test.png)

1. Feche o painel de teste.

<a name="publish-your-app"></a>

## <a name="publish-the-app-to-get-the-endpoint-url"></a>Publicar o aplicativo para obter a URL do ponto de extremidade

[!INCLUDE [LUIS How to Publish steps](./includes/howto-publish.md)]

<a name="query-the-v2-api-prediction-endpoint"></a>

## <a name="query-the-v3-api-prediction-endpoint"></a>Consultar o ponto de extremidade de previsão da API V3

[!INCLUDE [LUIS How to get endpoint first step](./includes/v3-prediction-endpoint.md)]

2. Na barra de endereços do navegador, para a cadeia de caracteres de consulta, verifique se as barras de nome e valor a seguir estão na URL. Se elas não estiverem na cadeia de caracteres de consulta, adicione-as:

    |Par chave/valor|
    |--|
    |`verbose=true`|
    |`show-all-intents=true`|

3. Na barra de endereços do navegador, vá até o final da URL e insira `turn off the living room light` para o valor _consulta_ e pressione Enter.

    ```json
    {
        "query": "turn off the living room light",
        "prediction": {
            "topIntent": "HomeAutomation.TurnOff",
            "intents": {
                "HomeAutomation.TurnOff": {
                    "score": 0.969448864
                },
                "HomeAutomation.QueryState": {
                    "score": 0.0122336326
                },
                "HomeAutomation.TurnUp": {
                    "score": 0.006547436
                },
                "HomeAutomation.TurnDown": {
                    "score": 0.0050634006
                },
                "HomeAutomation.SetDevice": {
                    "score": 0.004951761
                },
                "HomeAutomation.TurnOn": {
                    "score": 0.00312553928
                },
                "None": {
                    "score": 0.000552945654
                }
            },
            "entities": {
                "HomeAutomation.Location": [
                    "living room"
                ],
                "HomeAutomation.DeviceName": [
                    [
                        "living room light"
                    ]
                ],
                "HomeAutomation.DeviceType": [
                    [
                        "light"
                    ]
                ],
                "$instance": {
                    "HomeAutomation.Location": [
                        {
                            "type": "HomeAutomation.Location",
                            "text": "living room",
                            "startIndex": 13,
                            "length": 11,
                            "score": 0.902181149,
                            "modelTypeId": 1,
                            "modelType": "Entity Extractor",
                            "recognitionSources": [
                                "model"
                            ]
                        }
                    ],
                    "HomeAutomation.DeviceName": [
                        {
                            "type": "HomeAutomation.DeviceName",
                            "text": "living room light",
                            "startIndex": 13,
                            "length": 17,
                            "modelTypeId": 5,
                            "modelType": "List Entity Extractor",
                            "recognitionSources": [
                                "model"
                            ]
                        }
                    ],
                    "HomeAutomation.DeviceType": [
                        {
                            "type": "HomeAutomation.DeviceType",
                            "text": "light",
                            "startIndex": 25,
                            "length": 5,
                            "modelTypeId": 5,
                            "modelType": "List Entity Extractor",
                            "recognitionSources": [
                                "model"
                            ]
                        }
                    ]
                }
            }
        }
    }
    ```

    Saiba mais sobre o [ponto de extremidade de previsão V3](luis-migration-api-v3.md).


## <a name="clean-up-resources"></a>Limpar os recursos

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Próximas etapas

Você pode chamar o ponto de extremidade do código:

> [!div class="nextstepaction"]
> [Chamar um ponto de extremidade do LUIS usando código](luis-get-started-cs-get-intent.md)

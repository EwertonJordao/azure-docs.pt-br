---
title: O que é Serviço de fala?
titleSuffix: Azure Cognitive Services
description: O serviço de Fala é a unificação da conversão de fala em texto, da conversão de texto em fala e da tradução de fala em uma única assinatura do Azure. Adicione serviços de fala a seus aplicativos, ferramentas e dispositivos usando o SDK de Fala, o SDK de Dispositivos de Fala ou APIs REST.
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: overview
ms.date: 08/20/2020
ms.author: trbye
ms.openlocfilehash: 57577c840c93888b2b96971994724f085ad44ebe
ms.sourcegitcommit: d0541eccc35549db6381fa762cd17bc8e72b3423
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/09/2020
ms.locfileid: "89569298"
---
# <a name="what-is-the-speech-service"></a>O que é Serviço de fala?

O serviço de Fala é a unificação da conversão de fala em texto, da conversão de texto em fala e da tradução de fala em uma única assinatura do Azure. É fácil habilitar fala em seus aplicativos, ferramentas e dispositivos com a [CLI de fala](spx-overview.md), o [SDK de Fala](speech-sdk-reference.md), o [SDK de Dispositivos de Fala](https://aka.ms/sdsdk-quickstart), o [Speech Studio](https://speech.microsoft.com/) ou as [APIs REST](rest-apis.md).

> [!IMPORTANT]
> O serviço de Fala substituiu a API de Fala do Bing e a Tradução de Fala. Confira _Guias de instruções > Migração_ para obter instruções de migração.

Os recursos a seguir fazem parte do serviço de Fala. Use os links nesta tabela para saber mais sobre os casos de uso comuns de cada recurso ou navegue pela referência de API.

| Serviço | Recurso | Descrição | . | REST |
|---------|---------|-------------|-----|------|
| [Conversão de fala em texto](speech-to-text.md) | Conversão de fala em texto em tempo real | A conversão de fala em texto transcreve ou converte, em tempo real, transmissões de áudio ou arquivos locais em texto que seus aplicativos, ferramentas ou dispositivos podem consumir ou exibir. Use a conversão de fala em texto com o [LUIS (Reconhecimento vocal)](https://docs.microsoft.com/azure/cognitive-services/luis/) para derivar intenções do usuário de dados de fala transcrita e agir com base em comandos de voz. | [Sim](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) | [Sim](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis) |
| | [Conversão de fala em texto em lote](batch-transcription.md) | A conversão de fala em texto em lote permite a transcrição de fala em texto assíncrona de grandes volumes de dados de áudio de fala armazenados no Armazenamento de Blobs do Azure. Além de converter áudio de fala em texto, a conversão de fala em texto em lote também permite a análise de sentimentos e a diarização. | Não | [Sim](https://westus.cris.ai/swagger/ui/index) |
| | [Conversa em vários dispositivos](multi-device-conversation.md) | Conecte vários dispositivos ou clientes em uma conversa para enviar mensagens baseadas em fala ou texto, com suporte fácil para transcrição e tradução| Sim | Não |
| | [Transcrição de Conversas](conversation-transcription-service.md) | Permite o reconhecimento de fala em tempo real, a identificação do locutor e a diarização. É perfeito para transcrição de reuniões presenciais com a capacidade de distinguir os locutores. | Sim | Não |
| | [Criar Modelos de Fala Personalizada](#customize-your-speech-experience) | Se estiver usando a conversão de fala em texto para funcionalidades de reconhecimento e transcrição em um ambiente exclusivo, você poderá criar e treinar modelos acústicos, de idioma e de pronúncia personalizados para lidar com o ruído ambiente ou vocabulário específico do setor. | Não | [Sim](https://westus.cris.ai/swagger/ui/index) |
| [Conversão de texto em fala](text-to-speech.md) | Conversão de texto em fala | A conversão de texto em fala converte o texto de entrada em uma fala sintetizada semelhante à humana usando [Linguagem de marcação de síntese de fala (SSML)](speech-synthesis-markup.md). Escolha entre as vozes padrão e vozes neurais (consulte [Suporte para idiomas](language-support.md)). | [Sim](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) | [Sim](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis) |
| | [Criar Vozes Personalizadas](#customize-your-speech-experience) | Crie fontes de voz personalizadas exclusivas para sua marca ou produto. | Não | [Sim](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis) |
| [Tradução de Fala](speech-translation.md) | Tradução de fala | A tradução de fala possibilita a tradução em tempo real e em vários idiomas de fala para seus aplicativos, suas ferramentas e seus dispositivos. Use esse serviço para tradução de fala em fala e fala em texto. | [Sim](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) | Não |
| [Assistentes de voz](voice-assistants.md) | Assistentes de voz | Assistentes de voz que usam o serviço de Fala capacitam os desenvolvedores a criarem interfaces de conversação natural, semelhante à humana, para seus aplicativos e suas experiências. O serviço de assistente de voz fornece interação rápida e confiável entre um dispositivo e uma implementação de assistente que usa o Canal de Fala de Linha Direta do Bot Framework ou o serviço integrado de Comandos Personalizados (versão prévia) para a conclusão da tarefa. | [Sim](voice-assistants.md) | Não |
| [Reconhecimento do Locutor](speaker-recognition-overview.md) | Identificação e verificação do locutor | O serviço de Reconhecimento do Locutor fornece algoritmos que verificam e identificam os locutores por suas características de voz exclusivas. O Reconhecimento do Locutor é usado para responder à pergunta "quem está falando?". | Sim | [Sim](https://docs.microsoft.com/rest/api/speakerrecognition/) |


[!INCLUDE [TLS 1.2 enforcement](../../../includes/cognitive-services-tls-announcement.md)]

## <a name="try-the-speech-service-for-free"></a>Experimente o serviço de Fala gratuitamente

Nas etapas a seguir, você precisará de uma conta Microsoft e de uma conta do Azure. Se você não tiver uma conta Microsoft, poderá inscrever-se em uma gratuitamente no [portal da conta Microsoft](https://account.microsoft.com/account). Selecione **Entrar com a conta da Microsoft** e, quando solicitado a entrar, clique em **Criar uma conta Microsoft**. Siga as etapas para criar e confirmar sua nova conta Microsoft.

Depois de ter uma conta Microsoft, acesse a [página de inscrição do Azure](https://azure.microsoft.com/free/ai/), selecione **Iniciar gratuitamente** e crie uma conta do Azure usando uma conta Microsoft.

> [!NOTE]
> O serviço de Fala tem duas camadas de serviço: gratuita e assinatura, que têm diferentes limitações e benefícios. Quando você inscreve-se em uma conta gratuita do Azure, ela vem com US$ 200 em crédito de serviço que podem ser aplicados a uma assinatura paga do serviço de Fala, válidos por até 30 dias.
>
> Se você usar a camada de serviço de fala gratuita e de baixo volume, poderá manter essa assinatura gratuita mesmo após a expiração da avaliação gratuita ou do crédito de serviço.
>
> Para obter mais informações, confira [Preço dos Serviços Cognitivos – serviço de Fala](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/).

### <a name="create-the-azure-resource"></a>Criar o recurso do Azure

Para adicionar um recurso de serviço de Fala (camada gratuita ou paga) à sua conta do Azure:

1. Entre no [portal do Azure](https://portal.azure.com/) usando sua conta Microsoft.

1. Selecione **Criar um recurso** na parte superior esquerda do portal. Caso não veja **Criar um recurso**, você sempre pode encontrá-lo selecionando o menu recolhido no canto superior esquerdo da tela.

1. Na janela **Novo**, digite "fala" na caixa de pesquisa e pressione ENTER.

1. Nos resultados da pesquisa, selecione **Fala**.

   ![resultados da pesquisa de fala](media/index/speech-search.png)

1. Selecione **Criar** e:

   - Dê um nome exclusivo ao seu novo recurso. O nome ajuda a distinguir entre várias assinaturas associadas ao mesmo serviço.
   - Escolha a assinatura do Azure a qual o novo recurso está associado para determinar como os valores serão cobrados.
   - Escolha a [região](regions.md) em que o recurso será usado.
   - Escolha um tipo de preço gratuito (F0) ou pago (S0). Para obter informações completas sobre preços e cotas de uso para cada camada, selecione **Exibir detalhes de preço completos**.
   - Crie um grupo de recursos para esta assinatura de Fala ou atribua a assinatura a um grupo de recursos existente. Os grupos de recurso ajudam você a manter suas diversas assinaturas do Azure organizadas.
   - Selecione **Criar**. Isso direcionará você à visão geral de implantação e exibirá mensagens sobre o progresso da implantação.

> [!NOTE]
> Você pode criar um número ilimitado de assinaturas da camada Standard em uma ou várias regiões. No entanto, você só pode criar uma assinatura da camada gratuita. As implantações de modelo na camada gratuita que permanecerem inutilizadas por 7 dias serão desativadas automaticamente.

São necessários alguns instantes para a implantação do novo recurso de Fala. Após a conclusão da implantação, selecione **Ir para recurso** e, no painel de navegação esquerdo, selecione **Chaves** para exibir as chaves de assinatura do serviço de Fala. Cada assinatura tem duas chaves; você pode usar uma das chaves em seu aplicativo. Para copiar/colar rapidamente uma chave no editor de código ou em outra localização, selecione o botão copiar ao lado de cada chave, alterne as janelas para colar o conteúdo da área de transferência na localização desejada.

> [!IMPORTANT]
> Essas chaves de assinatura são usadas para acessar sua API do Serviço Cognitivo. Não compartilhe suas chaves. Armazene-as com segurança – por exemplo, usando o Azure Key Vault. Recomendamos a regeneração regular dessas chaves. Apenas uma chave é necessária para fazer uma chamada à API. Ao regenerar a primeira chave, você pode usar a segunda chave para obter acesso contínuo ao serviço.

## <a name="complete-a-quickstart"></a>Concluir um guia de início rápido

Oferecemos guias de início rápido nas linguagens de programação mais populares, todos eles desenvolvidos para ensinar padrões de design básicos e para você executar seu código em menos de 10 minutos. Confira a lista a seguir para obter o guia de início rápido para cada recurso.

* [Início rápido de conversão de fala em texto](get-started-speech-to-text.md)
* [Início rápido da conversão de texto em fala](get-started-text-to-speech.md)
* [Início rápido da tradução de fala](speech-translation-basics.md)
* [Início rápido do reconhecimento de intenção](quickstarts/intent-recognition.md)
* [Início rápido do reconhecimento de locutor](speaker-recognition-basics.md)

Após ter tido a chance de começar a usar o serviço de Fala, experimente os tutoriais que mostram como abordar diferentes cenários.

- [Tutorial: Reconhecer intenções de fala com o SDK de Fala e o LUIS, C#](how-to-recognize-intents-from-speech-csharp.md)
- [Tutorial: Habilitar por voz o bot com o SDK de Fala, C#](tutorial-voice-enable-your-bot-speech-sdk.md)
- [Tutorial: Criar um aplicativo Flask para traduzir texto, analisar o sentimento e sintetizar a conversão de texto traduzido em fala, REST](https://docs.microsoft.com/azure/cognitive-services/translator/tutorial-build-flask-app-translation-synthesis?toc=%2fazure%2fcognitive-services%2fspeech-service%2ftoc.json&bc=%2fazure%2fcognitive-services%2fspeech-service%2fbreadcrumb%2ftoc.json&toc=%2Fen-us%2Fazure%2Fcognitive-services%2Fspeech-service%2Ftoc.json&bc=%2Fen-us%2Fazure%2Fbread%2Ftoc.json)

## <a name="get-sample-code"></a>Obter código de exemplo

Há um código de exemplo disponível no GitHub para o serviço de Fala. Esses exemplos abordam cenários comuns, como ler áudio de um arquivo ou fluxo, reconhecimento contínuo e único e trabalho com modelos personalizados. Use estes links para exibir exemplos de SDK e REST:

- [Amostras de conversão de fala em texto, conversão de texto em fala e tradução de fala (SDK)](https://github.com/Azure-Samples/cognitive-services-speech-sdk)
- [Exemplos de transcrição em lote (REST)](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/samples/batch)
- [Exemplos de conversão de texto em fala (REST)](https://github.com/Azure-Samples/Cognitive-Speech-TTS)
- [Exemplos de assistente de voz (SDK)](https://aka.ms/csspeech/samples)

## <a name="customize-your-speech-experience"></a>Personalize sua experiência de fala

O serviço de Fala funciona bem com modelos internos, no entanto, pode ser interessante personalizar e ajustar ainda mais a experiência para seu ambiente ou produto. As opções de personalização vão do ajuste do modelo acústico a fontes de voz exclusivas para sua marca.

Outros produtos oferecem modelos de fala ajustados para fins específicos, como serviços de saúde ou seguros, mas estão disponíveis para todas as pessoas igualmente. A personalização na Fala do Azure se torna parte da sua *única* vantagem competitiva que não está disponível para nenhum outro usuário ou cliente. Em outras palavras, seus modelos são privados e personalizados para seu caso de uso.

| Serviço de Fala | Plataforma | Descrição |
| -------------- | -------- | ----------- |
| Conversão de fala em texto | [Fala Personalizada](https://aka.ms/customspeech) | Personalize modelos de reconhecimento de fala de acordo com suas necessidades e com os dados disponíveis. Supere as barreiras do reconhecimento de fala, como estilo de fala, vocabulário e ruído de fundo. |
| Conversão de texto em fala | [Voz personalizada](https://aka.ms/customvoice) | Crie uma voz reconhecível e exclusiva para seus aplicativos de Conversão de texto em fala usando seus dados de fala disponíveis. Você pode ajustar ainda mais as saídas de voz ajustando um conjunto de parâmetros de voz. |

## <a name="reference-docs"></a>Documentos de Referência

- [SDK da fala](speech-sdk-reference.md)
- [SDK de Dispositivos de Fala](speech-devices-sdk.md)
- [API REST: conversão de fala em texto](rest-speech-to-text.md)
- [API REST: conversão de texto em fala](rest-text-to-speech.md)
- [API REST: transcrição e personalização em lote](https://westus.cris.ai/swagger/ui/index)

## <a name="next-steps"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Introdução à conversão de fala em texto](speech-to-text-basics.md)
> [Introdução à conversão de texto em fala](get-started-text-to-speech.md)

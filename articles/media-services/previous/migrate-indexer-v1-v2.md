---
title: Migrar do indexador v1 e v2 para os serviços de mídia do Azure Video Indexer | Microsoft Docs
description: Este tópico discute como migrar do Azure Media Indexer v1 e v2 para os serviços de mídia do Azure Video Indexer.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/20/2019
ms.author: juliako
ms.openlocfilehash: e6b7c8cbcf6685ca2e781789fc508d005bcb5f88
ms.sourcegitcommit: 2c586a0fbec6968205f3dc2af20e89e01f1b74b5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/14/2020
ms.locfileid: "92018892"
---
# <a name="migrate-from-media-indexer-and-media-indexer-2-to-video-indexer"></a>Migrar do indexador de mídia e do indexador de mídia 2 para Video Indexer

[!INCLUDE [media services api v2 logo](./includes/v2-hr.md)]

O processador de mídia [Azure Media indexer](media-services-index-content.md) e os processadores de mídia de [Azure Media indexer 2 Preview](./legacy-components.md) estão sendo desativados. Para ver as datas de desativação, consulte o tópico de [componentes herdados](legacy-components.md). O [Video Indexer dos Serviços de Mídia do Azure](../video-indexer/index.yml) substitui esses processadores de mídia herdados.

Os serviços de mídia do Azure Video Indexer se baseiam em Análise de Mídia do Azure, Pesquisa Cognitiva do Azure, serviços cognitivas (como o API de Detecção Facial, o Microsoft Translator, o API da Pesquisa Visual Computacional e o serviço Fala Personalizada). Ele permite que você extraia os insights de seus vídeos usando os modelos de áudio e vídeo do Video Indexer. Para ver quais cenários Video Indexer podem ser usados no, quais recursos ele oferece e como começar, consulte [Video indexer modelos de vídeo e áudio](../video-indexer/video-indexer-overview.md). 

Você pode extrair informações de seus arquivos de vídeo e áudio usando as [predefinições do analisador dos serviços de mídia do Azure v3](../latest/analyzing-video-audio-files-concept.md) ou diretamente usando as [APIs de video indexer](https://api-portal.videoindexer.ai/). Atualmente, há uma sobreposição entre os recursos oferecidos pelas APIs de Video Indexer e as APIs dos serviços de mídia v3.

> [!NOTE]
> Para entender quando você desejaria usar as predefinições do Video Indexer vs. Media Services Analyzer, confira o [documento de comparação](../video-indexer/compare-video-indexer-with-media-services-presets.md). 

Este artigo discute as etapas para migrar do Azure Media Indexer e Azure Media Indexer 2 para os serviços de mídia do Azure Video Indexer.  

## <a name="migration-options"></a>Opções de migração 

|Se você precisar  |e, em seguida, |
|---|---|
|uma solução que fornece uma transcrição de conversão de fala em texto para qualquer formato de arquivo de mídia em formatos de arquivo de legenda codificada: VTT, SRT ou TTML<br/>Além de informações adicionais sobre áudio, como: palavras-chave, tópico inferência, eventos acústicos, diarization do palestrante, extração e tradução de entidades| atualize seus aplicativos para usar os recursos de Video Indexer do Azure por meio da API REST do Video Indexer v2 ou da predefinição do analisador de áudio do Azure Media Services V3.|
|recursos de conversão de fala em texto| Use os serviços cognitivas Speech API diretamente.|  

## <a name="getting-started-with-video-indexer"></a>Introdução ao Video Indexer

A seção a seguir indica os links relevantes: [como posso começar a usar o video indexer?](../video-indexer/video-indexer-overview.md#how-can-i-get-started-with-video-indexer) 

## <a name="getting-started-with-media-services-v3-apis"></a>Introdução às APIs dos serviços de mídia v3

A API dos serviços de mídia do Azure v3 permite que você extraia informações de seus arquivos de vídeo e áudio por meio das [predefinições do analisador dos serviços de mídia do Azure v3](../latest/analyzing-video-audio-files-concept.md). 

**AudioAnalyzerPreset** permite a extração de várias informações de áudio de um arquivo de áudio ou vídeo. A saída inclui um arquivo VTT ou TTML para a transcrição de áudio e um arquivo JSON (com todas as informações de áudio adicionais). As informações de áudio incluem palavras-chave, indexação de orador e análise de sentimentos de fala. O AudioAnalyzerPreset também dá suporte à detecção de idioma para idiomas específicos. Para obter informações detalhadas, consulte [transformações](/rest/api/media/transforms/createorupdate#audioanalyzerpreset).

### <a name="get-started"></a>Introdução

Para iniciar, confira:

* [Tutorial](../latest/analyze-videos-tutorial-with-api.md)
* Exemplos de AudioAnalyzerPreset: SDK do [Java](https://github.com/Azure-Samples/media-services-v3-java/tree/master/AudioAnalytics/AudioAnalyzer) ou [SDK do .net](https://github.com/Azure-Samples/media-services-v3-dotnet/tree/master/AudioAnalytics/AudioAnalyzer)
* Exemplos de VideoAnalyzerPreset: SDK do [Java](https://github.com/Azure-Samples/media-services-v3-java/tree/master/VideoAnalytics/VideoAnalyzer) ou [SDK do .net](https://github.com/Azure-Samples/media-services-v3-dotnet/tree/master/VideoAnalytics/VideoAnalyzer)

## <a name="getting-started-with-cognitive-services-speech-services"></a>Introdução aos serviços de fala de serviços cognitivas

Os [Serviços cognitivas do Azure](../../cognitive-services/index.yml) fornecem um serviço de fala para texto que transcreve fluxos de áudio para texto em tempo real que seus aplicativos, ferramentas ou dispositivos podem consumir ou exibir. Você pode usar a conversão de fala em texto para [personalizar seu próprio modelo acústico, modelo de linguagem ou modelo de pronúncia](../../cognitive-services/speech-service/how-to-custom-speech-train-model.md). Para obter mais informações, consulte [serviços de cognitiva de fala a texto](../../cognitive-services/speech-service/speech-to-text.md). 

> [!NOTE] 
> O serviço de fala em texto não usa formatos de arquivo de vídeo e só usa [determinados formatos de áudio](../../cognitive-services/speech-service/rest-speech-to-text.md#audio-formats). 

Para obter mais informações sobre o serviço de conversão de texto em fala e como começar, consulte [o que é conversão de fala em texto?](../../cognitive-services/speech-service/speech-to-text.md)

## <a name="known-differences-from-deprecated-services"></a>Diferenças conhecidas de serviços preteridos 

Você descobrirá que Video Indexer, os serviços de mídia do Azure v3 AudioAnalyzerPreset e serviços de fala de serviços cognitivas são mais confiáveis e produz uma saída de qualidade melhor do que os processadores desativados Azure Media Indexer 1 e Azure Media Indexer 2.  

Algumas diferenças conhecidas incluem: 

* Serviços de fala de serviços cognitivas não dão suporte à extração de palavras-chave. No entanto, Video Indexer e os serviços de mídia v3 AudioAnalyzerPreset oferecem um conjunto mais robusto de palavras-chave no formato de arquivo JSON. 

## <a name="need-help"></a>Precisa de ajuda?

Abra um tíquete de suporte navegando até [Nova solicitação de suporte](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest)

## <a name="next-steps"></a>Próximas etapas

* [Componentes herdados](legacy-components.md)
* [Página de preços](https://azure.microsoft.com/pricing/details/media-services/#encoding)
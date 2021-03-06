---
title: Texturas
description: Fluxo de trabalho de recurso de textura
author: florianborn71
ms.author: flborn
ms.date: 02/05/2020
ms.topic: conceptual
ms.custom: devx-track-csharp
ms.openlocfilehash: dc38b53705c24cb12a001237a9a80ec66ec33e14
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/09/2020
ms.locfileid: "89613785"
---
# <a name="textures"></a>Texturas

As texturas são um [recurso compartilhado](../concepts/lifetime.md) imutável. As texturas podem ser carregadas do [armazenamento de blobs](../how-tos/conversion/blob-storage.md) e aplicadas aos modelos diretamente, como demonstrado no [Tutorial: Alteração do ambiente e dos materiais](../tutorials/unity/materials-lighting-effects/materials-lighting-effects.md). No entanto, geralmente as texturas serão parte de um [modelo convertido](../how-tos/conversion/model-conversion.md), em que eles são referenciados por seus [materiais](materials.md).

## <a name="texture-types"></a>Tipos de textura

Tipos de textura diferentes têm diferentes casos de uso:

* As **texturas 2D** são usadas principalmente em [materiais](materials.md).
* **Cubemaps** podem ser usados para [céu](../overview/features/sky.md).

## <a name="supported-texture-formats"></a>Formatos de textura compatíveis

Todas as texturas para o Application Request Routing devem estar no formato [DDS](https://en.wikipedia.org/wiki/DirectDraw_Surface). Preferencialmente com mipmaps e compactação de textura. Veja [a ferramenta de linha de comando TexConv](../resources/tools/tex-conv.md) se quiser automatizar o processo de conversão.

## <a name="loading-textures"></a>Carregar texturas

Ao carregar uma textura, é preciso especificar seu tipo esperado. Se o tipo não corresponder, o carregamento da textura falhará.
Carregar uma textura com o mesmo URI duas vezes retornará o mesmo objeto de textura, pois é um [recurso compartilhado](../concepts/lifetime.md).

De forma semelhante ao carregamento de modelos, há duas variantes de endereçamento de um ativo de textura no armazenamento do blob de origem:

* O ativo de textura pode ser endereçado por seu URI de SAS. A função de carregamento relevante é `LoadTextureFromSASAsync` com o parâmetro `LoadTextureFromSASParams`. Use essa variante também ao carregar [texturas integradas](../overview/features/sky.md#built-in-environment-maps).
* Lide com a textura diretamente pelos parâmetros de Armazenamento de Blobs, caso o [armazenamento de blobs esteja vinculado à conta](../how-tos/create-an-account.md#link-storage-accounts). A função de carregamento relevante nesse caso é `LoadTextureAsync` com o parâmetro `LoadTextureParams`.

O código de exemplo a seguir mostra como carregar uma textura por meio de seu URI de SAS (ou textura integrada). Observe que apenas a função/o parâmetro de carregamento difere para o outro caso:

```cs
LoadTextureAsync _textureLoad = null;
void LoadMyTexture(AzureSession session, string textureUri)
{
    _textureLoad = session.Actions.LoadTextureFromSASAsync(new LoadTextureFromSASParams(textureUri, TextureType.Texture2D));
    _textureLoad.Completed +=
        (LoadTextureAsync res) =>
        {
            if (res.IsRanToCompletion)
            {
                //use res.Result
            }
            else
            {
                System.Console.WriteLine("Texture loading failed!");
            }
            _textureLoad = null;
        };
}
```

```cpp
void LoadMyTexture(ApiHandle<AzureSession> session, std::string textureUri)
{
    LoadTextureFromSASParams params;
    params.TextureType = TextureType::Texture2D;
    params.TextureUrl = std::move(textureUri);
    ApiHandle<LoadTextureAsync> textureLoad = *session->Actions()->LoadTextureFromSASAsync(params);
    textureLoad->Completed([](ApiHandle<LoadTextureAsync> res)
    {
        if (res->GetIsRanToCompletion())
        {
            //use res->Result()
        }
        else
        {
            printf("Texture loading failed!");
        }
    });
}
```

Dependendo do uso da textura, pode haver restrições para o tipo de textura e o conteúdo. Por exemplo, o mapa de irregularidades de um [material PBR](../overview/features/pbr-materials.md) deve ser em escala de cinza.

> [!CAUTION]
> Todas as funções *assíncronas* no Application Request Routing retornam objetos de operação assíncronos. É preciso armazenar uma referência a esses objetos até que a operação seja concluída. Caso contrário, o coletor de lixo do C# pode excluir a operação antecipadamente e pode nunca ser concluído. No código de exemplo acima, a variável de membro "_textureLoad" é usada para manter uma referência até que o evento *Concluído* chegue.

## <a name="api-documentation"></a>Documentação da API

* [Classe de textura C#](https://docs.microsoft.com/dotnet/api/microsoft.azure.remoterendering.texture)
* [C# Remotemanager. LoadTextureAsync ()](https://docs.microsoft.com/dotnet/api/microsoft.azure.remoterendering.remotemanager.loadtextureasync)
* [C# Remotemanager. LoadTextureFromSASAsync ()](https://docs.microsoft.com/dotnet/api/microsoft.azure.remoterendering.remotemanager.loadtexturefromsasasync)
* [Classe de textura C++](https://docs.microsoft.com/cpp/api/remote-rendering/texture)
* [C++ Remotomanager:: LoadTextureAsync ()](https://docs.microsoft.com/cpp/api/remote-rendering/remotemanager#loadtextureasync)
* [C++ Remotomanager:: LoadTextureFromSASAsync ()](https://docs.microsoft.com/cpp/api/remote-rendering/remotemanager#loadtexturefromsasasync)

## <a name="next-steps"></a>Próximas etapas

* [Materiais](materials.md)
* [Céu](../overview/features/sky.md)

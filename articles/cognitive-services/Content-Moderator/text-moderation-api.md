---
title: Moderação de texto – Content Moderator
titleSuffix: Azure Cognitive Services
description: Use moderação de texto para possíveis textos indesejados, dados pessoais e listas personalizadas de termos.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: conceptual
ms.date: 01/10/2019
ms.author: pafarley
ms.openlocfilehash: 5a07f0749b59efc96b67df3ad5ed2fbf353be614
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2020
ms.locfileid: "74538837"
---
# <a name="learn-text-moderation-concepts"></a>Aprender os conceitos de moderação de texto

Use a moderação de texto assistida por computador do Content Moderator e os recursos de [análise humana](Review-Tool-User-Guide/human-in-the-loop.md) para moderar o conteúdo de texto.

Você bloqueia, aprova ou analisa o conteúdo com base nas políticas e limites. Use-o para aumentar a moderação humana de ambientes em que parceiros, funcionários e consumidores geram conteúdo de texto. Isso inclui salas de chat, fóruns de discussão, chatbots, catálogos de comércio eletrônico e documentos. 

A resposta de serviço inclui as informações a seguir:

- Conteúdo ofensivo: correspondência baseada em termos com lista interna de termos ofensivos em vários idiomas
- Classificação: classificação assistida por computador em três categorias
- Dados pessoais
- Texto corrigido automaticamente
- Texto original
- Idioma

## <a name="profanity"></a>Conteúdo ofensivo

Se a API detectar termos ofensivos em qualquer um dos [idiomas com suporte](Text-Moderation-API-Languages.md), esses termos serão incluídos na resposta. A resposta também contém a localização (`Index`) no texto original. O `ListId` no JSON de exemplo a seguir refere-se aos termos encontrados nas [listas de termos personalizadas](try-terms-list-api.md), se disponíveis.

    "Terms": [
    {
        "Index": 118,
        "OriginalIndex": 118,
        "ListId": 0,
        "Term": "crap"
    }

> [!NOTE]
> Para o parâmetro **language**, atribua `eng` ou deixe vazio para ver a resposta de **classificação** assistida por computador (recurso de visualização). **Esse recurso dá suporte ao inglês apenas**.
>
> Para a detecção de **termos obscenos**, use o [código ISO 639-3](http://www-01.sil.org/iso639-3/codes.asp) dos idiomas com suporte listados neste artigo ou deixe vazio.

## <a name="classification"></a>classificação

O **recurso de classificação de texto** assistido por computador do Content Moderator dá suporte **somente em inglês** e ajuda a detectar conteúdo potencialmente indesejado. O conteúdo sinalizado pode ser avaliado como inadequado, dependendo do contexto. Ele transmite a probabilidade de cada categoria e pode recomendar uma análise humana. O recurso utiliza um modelo treinado para identificar uma possível linguagem abusiva, depreciativa ou discriminatória. Isso inclui gírias, palavras abreviadas, palavras ofensivas e intencionalmente incorretas para a análise. 

A extração a seguir na extração JSON mostra uma saída de exemplo:

    "Classification": {
        "ReviewRecommended": true,
        "Category1": {
            "Score": 1.5113095059859916E-06
            },
        "Category2": {
            "Score": 0.12747249007225037
            },
        "Category3": {
            "Score": 0.98799997568130493
        }
    }

### <a name="explanation"></a>Explicação

- `Category1` refere-se à presença potencial de linguagem que pode ser considerada sexualmente explícita ou para adulto em determinadas situações.
- `Category2` refere-se à presença potencial de linguagem que pode ser considerada sexualmente sugestiva ou para adulto em determinadas situações.
- `Category3` refere-se à potencial presença de linguagem que pode ser considerada ofensiva em determinadas situações.
- `Score` fica entre 0 e 1. Quanto maior a pontuação, mais o modelo irá prever que a categoria pode ser aplicável. Esse recurso se baseia em um modelo estatístico, em vez dos resultados codificados manualmente. É recomendável testar com seu próprio conteúdo para determinar como cada categoria se alinha com seus requisitos.
- `ReviewRecommended` é verdadeiro ou falso, dependendo dos limites internos da pontuação. Os clientes devem avaliar se é para usar esse valor ou escolher limites personalizados com base nas políticas de conteúdo.

## <a name="personal-data"></a>Dados pessoais

O recurso de dados pessoais detecta a presença potencial dessas informações:

- Endereço de email
- Endereço para correspondência
- Endereço IP
- Número de telefone dos EUA
- Número de telefone no Reino Unido
- CPF

O exemplo a seguir mostra uma resposta de exemplo:

```json
"PII":{ 
  "Email":[ 
    { 
      "Detected":"abcdef@abcd.com",
      "SubType":"Regular",
      "Text":"abcdef@abcd.com",
      "Index":32
    }
  ],
  "IPA":[ 
    { 
      "SubType":"IPV4",
      "Text":"255.255.255.255",
      "Index":72
    }
  ],
  "Phone":[ 
    { 
      "CountryCode":"US",
      "Text":"4255550111",
      "Index":56
    },
    { 
      "CountryCode":"US",
      "Text":"425 555 0111",
      "Index":212
    },
    { 
      "CountryCode":"UK",
      "Text":"+123 456 7890",
      "Index":208
    },
    { 
      "CountryCode":"UK",
      "Text":"0234 567 8901",
      "Index":228
    },
    { 
      "CountryCode":"UK",
      "Text":"0456 789 0123",
      "Index":245
    }
  ],
  "Address":[ 
    { 
      "Text":"1234 Main Boulevard, Panapolis WA 96555",
      "Index":89
    }
  ],
  "SSN":[ 
    { 
      "Text":"999999999",
      "Index":56
    },
    { 
      "Text":"999-99-9999",
      "Index":267
    }
  ]
}
```

## <a name="auto-correction"></a>Correção automática

Suponha que o texto de entrada seja (o 'lzay' e 'f0x' são intencionais):

    The qu!ck brown f0x jumps over the lzay dog.

Se você solicitar correção automática, a resposta conterá a versão corrigida do texto:

    The quick brown fox jumps over the lazy dog.

## <a name="creating-and-managing-your-custom-lists-of-terms"></a>Criar e gerenciar as listas de termos personalizadas

Embora a lista global de termos padrão funcione muito bem na maioria dos casos, convém filtrar os termos específicos das necessidades da sua empresa. Por exemplo, é possível querer filtrar qualquer marca competitiva das postagens dos usuários.

> [!NOTE]
> Há um limite máximo de **5 listas de termos** com cada lista para **não exceder 10.000 termos**.
>

O exemplo a seguir mostra a ID da lista correspondente:

    "Terms": [
    {
        "Index": 118,
        "OriginalIndex": 118,
        "ListId": 231.
        "Term": "crap"
    }

O Content Moderator fornece uma [API de Lista de Termos](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67f) com operações para gerenciar listas de termos personalizadas. Inicie com o [Console de API de Listas de Termos](try-terms-list-api.md) e use os exemplos de código de API REST. Consulte também o [Início rápido do .NET das Listas de Termos](term-lists-quickstart-dotnet.md), se você estiver familiarizado com Visual Studio e C#.

## <a name="next-steps"></a>Próximas etapas

Faça test drive do [Console de API de moderação de texto](try-text-api.md) e use os exemplos de código de API REST. Confira também a seção de moderação de texto do [.NET SDK quickstart](dotnet-sdk-quickstart.md) se você estiver familiarizado com visual studio e C#.

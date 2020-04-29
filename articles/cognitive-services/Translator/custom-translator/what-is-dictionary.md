---
title: O que é um dicionário? - Tradutor Personalizado
titleSuffix: Azure Cognitive Services
description: Um dicionário é um documento alinhado que especifica uma lista de frases ou sentenças (e suas traduções) que você sempre quer que o Microsoft Translator traduza da mesma maneira. Os dicionários às vezes também são chamados de glossários ou bases de termos.
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.date: 02/21/2019
ms.author: swmachan
ms.topic: conceptual
ms.openlocfilehash: 36b449c4c4ca30eb658c9519ce8e870a4f1fab32
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/29/2020
ms.locfileid: "71970734"
---
# <a name="what-is-a-dictionary"></a>O que é um dicionário?

Um dicionário é um par de documentos alinhados que especifica uma lista de frases ou sentenças e suas respectivas traduções. Use um dicionário no seu treinamento, quando quiser que o Microsoft Translator sempre traduza quaisquer instâncias da frase ou sentença de origem, usando a tradução que você forneceu no dicionário. Dicionários são às vezes chamados de glossários ou bases de termos. Você pode pensar no dicionário como uma força bruta “copiar e substituir” para todos os termos listados. Além disso, o serviço Tradutor personalizado da Microsoft cria e usa seus próprios dicionários de uso geral para melhorar a qualidade de sua tradução. No entanto, um dicionário fornecido pelo cliente usa o precedente e será pesquisado primeiro em palavras ou sentenças de pesquisa.

Os dicionários só funcionam para projetos em pares de idiomas que tenham um modelo de rede neural geral da Microsoft totalmente compatível por trás deles. [Exiba a lista completa de idiomas](https://docs.microsoft.com/azure/cognitive-services/translator/language-support#customization).

## <a name="phrase-dictionary"></a>Dicionário de frase
Quando você inclui um dicionário de frases no treinamento do seu modelo, qualquer palavra ou frase listada é traduzida da maneira que você especificou. O resto da frase é traduzido como de costume. Você pode usar um dicionário de frases para especificar frases que não devem ser traduzidas, fornecendo a mesma frase não traduzida no arquivo de origem e de destino no dicionário.

## <a name="sentence-dictionary"></a>Dicionário de frases
O dicionário de sentenças permite que você especifique uma tradução de destino exata para uma frase de origem. Para que uma correspondência de dicionário de sentença ocorra, toda a sentença enviada deve corresponder à entrada do dicionário de origem.  Se apenas uma parte da sentença corresponder, a entrada não será igual.  Quando uma correspondência é detectada, a entrada de destino do dicionário de frases será retornada.

## <a name="dictionary-only-trainings"></a>Treinamentos somente de dicionário
Você pode treinar um modelo usando apenas dados do dicionário. Para fazer isso, selecione apenas o documento do dicionário (ou vários documentos do dicionário) que você deseja incluir e toque em Criar modelo. Como esse é um treinamento somente de dicionário, não há um número mínimo de sentenças de treinamento necessárias. Seu modelo normalmente completará o treinamento muito mais rápido que um treinamento padrão.  Os modelos resultantes usarão os modelos de linha de base da Microsoft para tradução com a adição dos dicionários que você adicionou.  Você não obterá um relatório de teste.

>[!Note]
>O Custom Translator não faz a sentença alinhar os arquivos do dicionário, por isso é importante que haja um número igual de frases / frases de origem e de destino nos documentos do dicionário e que eles estejam precisamente alinhados.

## <a name="recommendations"></a>Recomendações

- Os dicionários não são um substituto para treinar um modelo usando dados de treinamento. É recomendável evitá-los e permitir que o sistema Aprenda com seus dados de treinamento. No entanto, quando frases ou substantivos compostos devem ser renderizados como estão, use um dicionário.
- O dicionário de frase deve ser usado com moderação. Então, lembre-se de que, quando uma frase dentro de uma sentença é substituída, o contexto dentro dessa sentença é perdido ou limitado para a tradução do restante da sentença. O resultado é que, embora a frase ou palavra dentro da frase seja traduzida de acordo com o dicionário fornecido, a qualidade geral da tradução da sentença geralmente será prejudicada.
- O dicionário de frases funciona bem para nomes compostos como nomes de produtos (“Microsoft SQL Server”), nomes próprios (“Cidade de Hamburgo”) ou recursos do produto (“tabela dinâmica”). Ele não funciona igualmente bem para verbos ou adjetivos, porque estes são tipicamente altamente flexionados na origem ou no idioma de destino. As práticas recomendadas são evitar entradas de dicionário de frase para qualquer coisa, exceto os substantivos compostos.
- Ao usar um dicionário de frases, as letras maiúsculas e as pontuações são importantes. As entradas de dicionário só corresponderão a palavras e frases na sentença de entrada que usam exatamente as mesmas maiúsculas e minúsculas especificadas no arquivo de dicionário de origem. Além disso, as traduções refletirão a capitalização e a pontuação fornecidas no arquivo do dicionário de destino. Por exemplo, se você tiver treinado um sistema em inglês para espanhol que usa um dicionário de frases que especifica "US" no arquivo de origem e "EE. UU ". no arquivo de destino. Quando você solicitar a tradução de uma frase que inclua a palavra "US" (não capitalizado), isso não corresponderia ao dicionário. No entanto, se você solicitar a tradução de uma frase que contenha a palavra "US" (em maiúscula), ela corresponderia ao dicionário e a tradução conteria "EE. UU ". Observe que a capitalização e a pontuação na tradução podem ser diferentes das especificadas no arquivo de destino do dicionário e podem ser diferentes das maiúsculas e minúsculas na origem. Ele segue as regras do idioma de destino.
- Se uma palavra aparecer mais de uma vez em um arquivo de dicionário, o sistema sempre usará a última entrada fornecida. Portanto, o dicionário não deve conter várias traduções da mesma palavra.

## <a name="next-steps"></a>Próximas etapas

- Leia sobre as diretrizes [sobre formatos de documentos](document-formats-naming-convention.md).

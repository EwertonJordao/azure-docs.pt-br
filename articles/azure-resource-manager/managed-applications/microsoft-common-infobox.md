---
title: Elemento De IU do InfoBox
description: Descreve o elemento Microsoft.Common.InfoBox UI para o portal Azure. Use para adicionar texto ou avisos ao implantar o aplicativo gerenciado.
author: tfitzmac
ms.topic: conceptual
ms.date: 06/15/2018
ms.author: tomfitz
ms.openlocfilehash: 6d1e4a84904ef7022d9ce85803bf10285bf0b8ac
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75652470"
---
# <a name="microsoftcommoninfobox-ui-element"></a>Elemento de interface do usuário Microsoft.Common.InfoBox

Um controle que adiciona uma caixa de informações. A caixa contém texto ou avisos importantes que ajudam os usuários a entender os valores que estão fornecendo. Ela também pode ter um link para um URI que ofereça mais informações.

## <a name="ui-sample"></a>Exemplo de interface do usuário

![Microsoft.Common.InfoBox](./media/managed-application-elements/microsoft.common.infobox.png)


## <a name="schema"></a>Esquema

```json
{
  "name": "text1",
  "type": "Microsoft.Common.InfoBox",
  "visible": true,
  "options": {
    "icon": "None",
    "text": "Nullam eros mi, mollis in sollicitudin non, tincidunt sed enim. Sed et felis metus, rhoncus ornare nibh. Ut at magna leo.",
    "uri": "https://www.microsoft.com"
  }
}
```

## <a name="sample-output"></a>Saída de exemplo

```json
"Nullam eros mi, mollis in sollicitudin non, tincidunt sed enim. Sed et felis metus, rhoncus ornare nibh. Ut at magna leo."
```

## <a name="remarks"></a>Comentários

* Para `icon`, use **Nenhum**, **Informações**, **Aviso**, ou **Erro**.
* A propriedade `uri` é opcional.

## <a name="next-steps"></a>Próximas etapas

* Para obter uma introdução à criação de definições de interface do usuário, consulte [Introdução ao CreateUiDefinition](create-uidefinition-overview.md).
* Para obter uma descrição das propriedades comuns em elementos de interface do usuário, consulte [Elementos de CreateUiDefinition](create-uidefinition-elements.md).

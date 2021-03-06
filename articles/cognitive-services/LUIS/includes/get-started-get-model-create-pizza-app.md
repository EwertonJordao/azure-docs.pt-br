---
title: incluir arquivo
description: incluir arquivo
services: cognitive-services
author: roy-har
manager: diberry
ms.service: cognitive-services
ms.date: 06/04/2020
ms.subservice: language-understanding
ms.topic: include
ms.custom: include file
ms.author: roy-har
ms.openlocfilehash: 9965e4c856fdef2af17b116264ad5344ebc97eb2
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/05/2020
ms.locfileid: "84466904"
---
Criar o aplicativo Pizza.

1. Selecione [pizza-app-for-luis-v6.json](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/luis/apps/pizza-app-for-luis-v6.json) para abrir a página do GitHub para o arquivo `pizza-app-for-luis.json`.
1. Clique com o botão direito do mouse (ou toque e segure por alguns segundos) o botão **Bruto** e selecione **Salvar link como** para salvar o `pizza-app-for-luis.json` em seu computador.
1. Entre no [portal do LUIS](https://www.luis.ai).
1. Selecione [Meus Aplicativos](https://www.luis.ai/applications).
1. Na página **Meus aplicativos**, selecione **+ Novo aplicativo de conversa**.
1. Selecione **Importar como JSON**.
1. Na caixa de diálogo **Importar novo aplicativo**, selecione o botão **Escolher arquivo**.
1. Selecione o arquivo `pizza-app-for-luis.json` que você baixou e, em seguida, selecione **Abrir**.
1. No campo **Nome** da caixa de diálogo **Importar novo aplicativo**, insira um nome para o aplicativo Pizza e, em seguida, selecione o botão **Concluído**.

O aplicativo será importado.

Se você vir uma caixa de diálogo que diz **Como criar um aplicativo LUIS eficaz**, feche-a.

## <a name="train-and-publish-the-pizza-app"></a>Treinar e publicar o aplicativo Pizza

Você deve ver a página **Intenções** com uma lista de intenções no aplicativo Pizza.

[!INCLUDE [How to train](howto-train.md)]

[!INCLUDE [How to publish](howto-publish.md)]

## <a name="add-an-authoring-resource-to-the-pizza-app"></a>Adicionar um recurso de criação ao aplicativo Pizza

1. Selecione **GERENCIAR**.
1. Selecione **Recursos do Azure**.
1. Selecione **Recurso de Criação**.
1. Selecione **Alterar recurso de criação**.

Se você tiver um recurso de criação, insira o **Nome do Locatário**, **Nome da Assinatura** e **nome do recurso LUIS** do recurso de criação.

Se você não tiver um recurso de criação:

1. Selecione **Criar recurso**.
1. Insira um **Nome do Locatário**, **Nome do Recurso**, **Nome da Assinatura** e **Nome do Grupo de Recursos do Azure**.

Agora, o aplicativo Pizza está pronto para o uso.

## <a name="record-the-access-values-for-your-pizza-app"></a>Registrar os valores de acesso para seu aplicativo Pizza

Para usar o novo aplicativo Pizza, você precisará da ID do aplicativo, da chave de criação e do ponto de extremidade de criação desse aplicativo.

Para encontrar esses valores:

1. Na página **Intenções**, selecione **GERENCIAR**.
1. Na página **Configurações de aplicativo**, registre a **ID do aplicativo**.
1. Selecione **Recursos do Azure**.
1. Selecione **Recurso de Criação**.
1. Na guia **Recurso de Criação**, registre a **Chave Primária**. Esse valor é a sua chave de criação.
1. Registre a **URL do ponto de extremidade**. Esse valor é o seu ponto de extremidade de criação.

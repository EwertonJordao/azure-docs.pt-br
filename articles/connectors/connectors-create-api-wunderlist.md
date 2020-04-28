---
title: Conectar-se ao Wunderlist de aplicativos lógicos do Azure
description: Automatize fluxos de trabalho e tarefas que monitoram e gerenciam listas, tarefas e lembretes, entre outros, em sua conta do Wunderlist usando os Aplicativos Lógicos do Azure
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 08/25/2018
tags: connectors
ms.openlocfilehash: 5ac13595bd77238aaede5fa3bdc3a35ef69e8504
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2020
ms.locfileid: "74789113"
---
# <a name="monitor-and-manage-wunderlist-by-using-azure-logic-apps"></a>Monitore e gerencie o Wunderlist usando os Aplicativos Lógicos do Azure

Com os Aplicativos Lógicos do Azure e o conector do Wunderlist, você pode criar fluxos de trabalho e tarefas automatizadas que monitoram e gerenciam listas de tarefas pendentes, tarefas, lembretes e mais em sua conta do Wunderlist, bem como outras ações, por exemplo:

* Monitorar quando novas tarefas são criadas, quando tarefas atingem o prazo e quando lembretes são gerados.
* Criar e gerenciar listas, observações, tarefas, subtarefas e muito mais.
* Configurar lembretes.
* Obter listas, tarefas, subtarefas, lembretes, arquivos, observações, comentários e muito mais.

O [Wunderlist](https://www.wunderlist.com/) é um serviço que ajuda você a planejar, gerenciar e concluir seus projetos, tarefas e listas de tarefas pendentes em qualquer dispositivo e qualquer lugar. Você pode usar gatilhos que obtêm respostas de sua conta do Wunderlist e disponibilizam a saída para outras ações. Você pode usar ações que executam tarefas com sua conta do Wunderlist. Você também pode fazer com que outras ações usem a saída das ações do Wunderlist. Por exemplo, quando novas tarefas atingem o prazo definido, você pode postar mensagens com o conector do Slack. Se você for novo em aplicativos lógicos, examine [o que são os aplicativos lógicos do Azure?](../logic-apps/logic-apps-overview.md)

## <a name="prerequisites"></a>Pré-requisitos

* Uma assinatura do Azure. Se você não tiver uma assinatura do Azure, [inscreva-se em uma conta gratuita do Azure](https://azure.microsoft.com/free/). 

* Suas credenciais de usuário e conta do Wunderlist

   Suas credenciais autorizam o aplicativo lógico a criar uma conexão e acessar sua conta do Wunderlist.

* Conhecimento básico sobre [como criar aplicativos lógicos](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* O aplicativo lógico em que você deseja acessar sua conta do Yammer. Para iniciar com um gatilho do Wunderlist, [crie um aplicativo lógico em branco](../logic-apps/quickstart-create-first-logic-app-workflow.md). Para usar uma ação do Wunderlist, inicie seu aplicativo lógico com outro gatilho, por exemplo, o gatilho de **Recorrência**.

## <a name="connect-to-wunderlist"></a>Conectar-se com o Wunderlist

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. Entre no [portal do Azure](https://portal.azure.com) e abra seu aplicativo lógico no Designer de Aplicativo Lógico, se ele ainda não estiver aberto.

1. Escolha um caminho: 

   * Para aplicativos lógicos em branco, na caixa de pesquisa, insira "wunderlist" como o filtro. 
   Na lista de gatilhos, selecione o gatilho desejado. 

     -ou-

   * Para aplicativos lógicos existentes: 
   
     * Na última etapa em que você deseja adicionar uma ação, escolha **Nova etapa**. 

       -ou-

     * Entre as etapas em que você deseja adicionar uma ação, mova o ponteiro sobre a seta entre as etapas. 
     Escolha o sinal de adição**+**() que aparece e, em seguida, selecione **Adicionar uma ação**.
     
       Na caixa de pesquisa, digite "wunderlist" como filtro. 
       Na lista de ações, selecione a ação desejada.

1. Se for solicitado que você entre no Wunderlist, entre agora para que possa permitir o acesso.

1. Forneça os detalhes necessários para o gatilho ou a ação selecionada e continue criando o fluxo de trabalho do aplicativo lógico.

## <a name="connector-reference"></a>Referência de conector

Para obter detalhes técnicos sobre gatilhos, ações e limites, que são explicados na descrição da OpenAPI do conector (anteriormente conhecido como Swagger), veja a [página de referência](/connectors/wunderlist/) do conector.

## <a name="get-support"></a>Obter suporte

* Em caso de dúvidas, visite o [Fórum dos Aplicativos Lógicos do Azure](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).
* Para enviar ou votar em ideias de recurso, visite o [site de comentários do usuário de Aplicativos Lógicos](https://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Próximas etapas

* Saiba mais sobre outros [conectores de Aplicativos Lógicos](../connectors/apis-list.md)
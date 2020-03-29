---
title: Conecte-se ao Salesforce a partir de aplicativos azure logic
description: Automatizar tarefas e fluxos de trabalho que monitoram, criam e gerenciam trabalhos e registros do Salesforce usando os Aplicativos Lógicos do Azure
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 08/24/2018
tags: connectors
ms.openlocfilehash: 000f4381ef2a7c0a2099a021b991087725ff2070
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74789283"
---
# <a name="monitor-create-and-manage-salesforce-resources-by-using-azure-logic-apps"></a>Monitorar, criar e gerenciar recursos do Salesforce usando os Aplicativos Lógicos do Azure

Com os Aplicativos Lógicos do Azure e o conector do Salesforce, você pode criar fluxos de trabalho e tarefas automatizadas para os recursos do Salesforce, tais como registros, trabalhos e objetos, por exemplo:

* Monitore quando registros são criados ou alterados. 
* Crie, obtenha e gerencie trabalhos e registros, incluindo as ações de inserção, atualização e exclusão.

Você pode usar gatilhos do Salesforce para obter respostas do Salesforce e disponibilizar a saída para outras ações. Você pode usar ações no Aplicativo Lógico para executar tarefas com recursos de Salesforce. Se você é novo em aplicativos lógicos, [revise o que é o Azure Logic Apps?](../logic-apps/logic-apps-overview.md)

## <a name="prerequisites"></a>Pré-requisitos

* Uma assinatura do Azure. Se você não tiver uma assinatura do Azure, [inscreva-se em uma conta gratuita do Azure](https://azure.microsoft.com/free/). 

* Uma [conta do Salesforce](https://salesforce.com/)

* Conhecimento básico sobre [como criar aplicativos lógicos](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* O aplicativo lógico no qual você deseja acessar a conta do Salesforce. Para iniciar com um gatilho do Salesforce, [crie um aplicativo lógico em branco](../logic-apps/quickstart-create-first-logic-app-workflow.md). Para usar uma ação do Salesforce, inicie seu aplicativo lógico com outro gatilho, por exemplo, o gatilho **Recorrência**.

## <a name="connect-to-salesforce"></a>Conectar-se ao Salesforce

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. Entre no [portal do Azure](https://portal.azure.com) e abra seu aplicativo lógico no Designer de Aplicativo Lógico, se ele ainda não estiver aberto.

1. Escolha um caminho: 

   * Para aplicativos lógicos em branco, na caixa de pesquisa, insira "salesforce" como o filtro. 
   Na lista de gatilhos, selecione o gatilho desejado. 

     -ou-

   * Para aplicativos lógicos existentes, na etapa em que deseja adicionar uma ação, escolha **Nova etapa**. Na caixa de pesquisa, insira "salesforce" como seu filtro. Na lista de ações, selecione a ação desejada.

1. Se for solicitado que você entre no Salesforce, inscreva-se agora e permita o acesso.

   Suas credenciais autorizam o aplicativo lógico a criar uma conexão com o Salesforce e acessar seus dados.

1. Forneça os detalhes necessários para o gatilho ou a ação selecionada e continue criando o fluxo de trabalho do aplicativo lógico.

## <a name="connector-reference"></a>Referência de conector

Para obter detalhes técnicos sobre gatilhos, ações e limites, que são explicados na descrição da OpenAPI do conector (anteriormente conhecido como Swagger), veja a [página de referência](/connectors/salesforce/) do conector.

## <a name="get-support"></a>Obter suporte

* Em caso de dúvidas, visite o [Fórum dos Aplicativos Lógicos do Azure](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).
* Para enviar ou votar em ideias de recurso, visite o [site de comentários do usuário de Aplicativos Lógicos](https://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Próximas etapas

* Saiba mais sobre outros [conectores de Aplicativos Lógicos](../connectors/apis-list.md)
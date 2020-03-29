---
ms.service: logic-apps
ms.topic: include
author: ecfan
ms.author: estfan
ms.date: 03/02/2018
ms.openlocfilehash: 1aabc1b33722c571c6519b6620fb62211a44c2e8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74789744"
---
1. No [portal Azure,](https://portal.azure.com)crie um aplicativo de lógica em branco. 

2. Mo Designer de Aplicativos Lógicos, insira "github" como filtro. 

3. Selecione o conector GitHub e o gatilho que você quer usar.

   ![Selecionar o conector GitHub e um gatilho](./media/connectors-create-api-github/github-connector.png)

   > [!NOTE]
   > Todos os fluxos de trabalho de aplicativo lógico devem iniciar com um gatilho. Você poderá selecionar ações somente quando a lógica de fluxo de trabalho iniciar com um gatilho. 

4. Se uma conexão não foi criada anteriormente, escolha **Entrar** para que você possa fornecer as credenciais do GitHub quando for solicitado.  

   ![Entrar com as credenciais do GitHub](./media/connectors-create-api-github/github-connector-sign-in-credentials.png)

   O aplicativo lógico usa essas credenciais para autorizar a conexão e o acesso a dados da sua conta do GitHub. 

5. Forneça o nome de usuário e senha do GitHub e confirme sua autorização.

   ![Fornecer credenciais e confirmar autorização](./media/connectors-create-api-github/github-connector-authorize.png)   

   Sua conexão é criada no Portal do Azure e está pronta para uso.

6. Continue definindo o fluxo de trabalho do aplicativo lógico.

   ![Adicionar mais ações ao fluxo de trabalho do aplicativo lógico](./media/connectors-create-api-github/github-connector-logic-app.png)


---
ms.service: logic-apps
ms.topic: include
author: ecfan
ms.author: estfan
ms.date: 11/03/2016
ms.openlocfilehash: 951ab2300aa4ffed2c5f1039ff993cd7f6af543f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74789680"
---
## <a name="prerequisites"></a>Pré-requisitos

* Uma conta do Azure; você pode criar uma [conta gratuita](https://azure.microsoft.com/free)
* Uma conta do [OneDrive](https://www.microsoft.com/store/apps/onedrive/9wzdncrfj1p3) 

Para poder usar sua conta do OneDrive em um aplicativo lógico, autorize-o a se conectar à sua conta do OneDrive.  Você pode fazer isso de forma fácil usando seu aplicativo lógico no portal do Azure. 

Aqui estão as etapas para autorizar seu aplicativo lógico a se conectar à sua conta do OneDrive:

1. Crie um aplicativo lógico. No designer de Aplicativos Lógicos, selecione **Mostrar APIs gerenciadas da Microsoft** na lista suspensa e então digite "onedrive" na caixa de pesquisa. Selecione um dos gatilhos ou ações:  
   ![](./media/connectors-create-api-onedrive/onedrive-1.png)
2. Se você não tiver criado quaisquer conexões OneDrive anteriormente, será solicitado a entrar usando suas credenciais do OneDrive:  
   ![](./media/connectors-create-api-onedrive/onedrive-2.png)
3. Selecione **Entrar** e insira seu nome de usuário e senha. Selecione **Entrar**:  
   ![](./media/connectors-create-api-onedrive/onedrive-3.png)   
   
    Essas credenciais serão usadas para autorizar seu aplicativo lógico a se conectar aos dados da sua conta do OneDrive e usá-los. 
4. Selecione **Sim** para autorizar o aplicativo lógico para usar sua conta do OneDrive:  
   ![](./media/connectors-create-api-onedrive/onedrive-4.png)   
5. Observe que a conexão foi criada. Agora, continue com as outras etapas no seu aplicativo lógico:  
   ![](./media/connectors-create-api-onedrive/onedrive-5.png)


---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 04/24/2019
ms.author: glenga
ms.openlocfilehash: 9c972508c98e87771154bd26cc166c6bea4201ee
ms.sourcegitcommit: 5397b08426da7f05d8aa2e5f465b71b97a75550b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/19/2020
ms.locfileid: "76279477"
---
## <a name="test"></a>Verificar a função no Azure

Você pode usar um navegador da Web para verificar a função implantada.  Copie a URL, incluindo a chave de função, para a barra de endereços do seu navegador da Web. Acrescente a cadeia de caracteres de consulta `&name=<yourname>` à URL antes de executar a solicitação.

![Usando um navegador da Web para chamar a função.](./media/functions-test-function-code/functions-azure-cli-function-test-browser.png)  

Ou você pode usar o cURL para verificar a função implantada. Usando a URL, incluindo a chave de função, que você copiou da etapa anterior, acrescente a cadeia de consulta `&name=<yourname>` à URL.

![usando o cURL para chamar a função no Azure.](./media/functions-test-function-code/functions-azure-cli-function-test-curl.png) 


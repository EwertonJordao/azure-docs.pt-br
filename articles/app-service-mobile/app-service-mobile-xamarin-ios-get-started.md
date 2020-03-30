---
title: Comece com os aplicativos Xamarin.iOS
description: Siga este tutorial para começar a usar os Aplicativos Móveis para desenvolvimento do Xamarin.iOS.
ms.assetid: 14428794-52ad-4b51-956c-deb296cafa34
ms.tgt_pltfrm: mobile-xamarin-ios
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 06/25/2019
ms.openlocfilehash: 6c35189e7c841fa2724f1cfe84afc689d5510676
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77458981"
---
# <a name="create-a-xamarinios-app"></a>Criar um aplicativo Xamarin.iOS
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a>Visão geral
Este tutorial mostra como adicionar um serviço de back-end baseado em nuvem a um aplicativo móvel Xamarin.iOS usando um back-end de Aplicativo Móvel do Azure.  Você cria um novo back-end do aplicativo móvel e um aplicativo Xamarin.iOS *Todo list* simples que armazena dados de aplicativo no Azure.

Concluir este tutorial é um pré-requisito para todos os outros tutoriais do Xamarin.iOS sobre como usar o recurso de Aplicativos Móveis no Serviço de Aplicativo do Azure.

## <a name="prerequisites"></a>Pré-requisitos
Para concluir este tutorial, você precisará dos seguintes pré-requisitos:

* Uma conta ativa do Azure. Caso você não tenha uma conta, inscreva-se para uma avaliação do Azure e obtenha até 10 aplicativos móveis gratuitos que você pode continuar a usar mesmo após o fim do seu período de avaliação. Para obter detalhes, consulte [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).
* Visual Studio para Mac. Ver [Configuração e instalar para Visual Studio para Mac](https://docs.microsoft.com/visualstudio/mac/installation?view=vsmac-2019)
* Um Mac com Xcode 9.0 ou posterior.
  
## <a name="create-an-azure-mobile-app-backend"></a>Criar um back-end de aplicativo móvel do Azure
[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

## <a name="create-a-database-connection-and-configure-the-client-and-server-project"></a>Crie uma conexão de banco de dados e configure o projeto cliente e servidor
[!INCLUDE [app-service-mobile-configure-new-backend](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="run-the-xamarinios-app"></a>Execute o aplicativo Xamarin.iOS
1. Abra o projeto Xamarin.iOS.

2. Acesse o [portal do Azure](https://portal.azure.com/) e navegue até o aplicativo móvel que você criou. Na `Overview` lâmina, procure a URL que é o ponto final público para o seu aplicativo móvel. Exemplo - o nome do site do meu nome https://test123.azurewebsites.netde aplicativo "test123" será .

3. Abra o `QSTodoService.cs` arquivo nesta pasta - xamarin.iOS/ZUMOAPPNAME. O nome do app é `ZUMOAPPNAME`.

4. Em `QSTodoService` classe, `ZUMOAPPURL` substitua a variável por ponto final público acima.

    `const string applicationURL = @"ZUMOAPPURL";`

    se torna
    
    `const string applicationURL = @"https://test123.azurewebsites.net";`
    
5. Pressione a tecla F5 para implantar e executar o aplicativo em um emulador de iPhone.

6. No aplicativo, digite texto significativo, como *Complete o tutorial* e clique no botão +.

    ![][10]

    Os dados da solicitação são inseridos na tabela TodoItem. Itens armazenados na tabela são retornados pelo back-end do aplicativo móvel e os dados aparecem na lista.

   > [!NOTE]
   > Você pode examinar o código que acessa o back-end do aplicativo móvel para consultar e inserir dados que estão localizados no arquivo ToDoActivity.cs C#.
   
<!-- Images. -->
[10]: ./media/app-service-mobile-xamarin-ios-get-started/mobile-quickstart-startup-ios.png

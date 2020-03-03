---
title: 'Tutorial: Compartilhar âncoras com o Azure Cosmos DB'
description: Neste tutorial, você aprenderá a compartilhar os identificadores de Âncoras Espaciais do Azure entre os dispositivos Android/iOS no Unity com um serviço de back-end e Azure Cosmos DB.
author: ramonarguelles
manager: vriveras
services: azure-spatial-anchors
ms.author: rgarcia
ms.date: 02/24/2019
ms.topic: tutorial
ms.service: azure-spatial-anchors
ms.openlocfilehash: 71b3027d86400d6921895f86e257ddff2961f91f
ms.sourcegitcommit: f15f548aaead27b76f64d73224e8f6a1a0fc2262
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/26/2020
ms.locfileid: "77615146"
---
# <a name="tutorial-sharing-azure-spatial-anchors-across-sessions-and-devices-with-an-azure-cosmos-db-back-end"></a>Tutorial: Compartilhar Âncoras Espaciais do Azure entre sessões e dispositivos com um back-end do Azure Cosmos DB

Este tutorial é uma continuação do [compartilhamento de Âncoras Espaciais do Azure entre sessões e dispositivos.](../../../articles/spatial-anchors/tutorials/tutorial-share-anchors-across-devices.md) Ele orientará você no processo de adicionar mais algumas funcionalidades para fazer o Azure Cosmos DB funcionar como um armazenamento de back-end ao comparar âncoras espaciais do azure entre sessões e dispositivos.

![Persistência de objetos que ilustram GIF](./media/persistence.gif)

Vale a pena lembrar que, embora você vá usar o Unity e o Azure Cosmos DB neste tutorial, será apenas um exemplo de como compartilhar identificadores de Âncoras Espaciais do Azure em outros dispositivos. Você poderá usar outras linguagens e tecnologias de back-end para alcançar a mesma meta. Além disso, o aplicativo Web do ASP.NET Core usado neste tutorial tem uma dependência do SDK 2.2 do .NET Core. Ele é executado bem em aplicativos Web para Windows, mas atualmente ele não será executado nos aplicativos Web para Linux.

## <a name="create-a-database-account"></a>Criar uma conta de banco de dados

Adicione um Banco de Dados do Azure Cosmos ao grupo de recursos criado anteriormente.

[!INCLUDE [cosmos-db-create-dbaccount-table](../../../includes/cosmos-db-create-dbaccount-table.md)]

Copie o `Connection String` porque você precisará.

## <a name="make-minor-changes-to-the-sharingservice-files"></a>Fazer alterações secundárias nos arquivos SharingService

No **Gerenciador de Soluções**, abra `SharingService\Startup.cs`.

Localize `#define INMEMORY_DEMO` na parte superior do arquivo e comente-a. Salve o arquivo.

No **Gerenciador de Soluções**, abra `SharingService\appsettings.json`.

Localize a propriedade `StorageConnectionString` e defina o valor para que seja o mesmo `Connection String` que você copiou na [etapa criar uma conta de banco de dados](#create-a-database-account). Salve o arquivo.

Você pode publicar o Serviço de Compartilhamento novamente e executar o aplicativo de exemplo.

## <a name="troubleshooting"></a>Solução de problemas

### <a name="unity-20193"></a>Unity 2019.3

Devido a alterações da falha, atualmente, não há suporte para o Unity 2019.3. Use o Unity 2019.1 ou 2019.2.

[!INCLUDE [Clean-up section](../../../includes/clean-up-section-portal.md)]

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você usou o Azure Cosmos DB para compartilhar identificadores de âncora entre dispositivos. Para saber mais sobre como usar Âncoras Espaciais do Azure em um novo aplicativo Unity HoloLens, passe para o próximo tutorial.

> [!div class="nextstepaction"]
> [Iniciar um novo aplicativo do Unity HoloLens](./tutorial-new-unity-hololens-app.md)

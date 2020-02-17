---
title: incluir arquivo
description: incluir arquivo
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: include
ms.date: 02/13/2020
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: bbdafde85097d0052edd5984b594fd37066dc1e6
ms.sourcegitcommit: 0eb0673e7dd9ca21525001a1cab6ad1c54f2e929
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/14/2020
ms.locfileid: "77279738"
---
Esta seção descreve como criar um hub IoT usando o [portal do Azure](https://portal.azure.com).

1. Entre no [portal do Azure](https://portal.azure.com).

1. Na página inicial do Azure, selecione o botão **Criar um recurso** e, em seguida, insira *Hub IoT* no campo **Pesquisar no Marketplace**.

1. Selecione **Hub IoT** nos resultados da pesquisa e, em seguida, selecione **Criar**.

1. Na guia **Básico**, preencha os campos da seguinte maneira:

   - **Assinatura**: Selecione a assinatura a ser usada para o hub.

   - **Grupo de Recursos**: Selecione um grupo de recursos ou crie um novo. Para criar um novo, selecione **Criar novo** e preencha o nome que deseja usar. Para usar um grupo de recursos existente, selecione esse grupo de recursos. Para obter mais informações, veja [Gerenciar grupos de recursos do Azure Resource Manager](../articles/azure-resource-manager/management/manage-resource-groups-portal.md).

   - **Região**: Selecione a região na qual você deseja que o hub esteja localizado. Selecione a localização mais próxima de você.

   - **Nome do Hub IoT**: Digite um nome para o seu hub. Esse nome deve ser globalmente exclusivo. Caso o nome inserido esteja disponível, uma marca de seleção verde será exibida.

   [!INCLUDE [iot-hub-pii-note-naming-hub](iot-hub-pii-note-naming-hub.md)]

   ![Criar um hub no portal do Azure](./media/iot-hub-include-create-hub/iot-hub-create-screen-basics.png)

1. Selecione **Avançar: Tamanho e escala** para continuar criando o hub IoT.

   ![Definir o tamanho e a escala para um novo hub usando o portal do Azure](./media/iot-hub-include-create-hub/iot-hub-create-screen-size-scale.png)

    Esta tela permite que você defina os seguintes valores:

    - **Tipo e escala de preço**: Seu nível selecionado. É possível escolher entre várias camadas, dependendo de quantos recursos você quer e quantas mensagens você envia por dia usando sua solução. A camada gratuita destina-se a testes e avaliação. Ela permite que 500 dispositivos sejam conectados ao hub e até 8 mil mensagens por dia. Cada assinatura do Azure pode criar um hub IoT na Camada gratuita.

    - **Unidades do Hub IoT**: O número de mensagens permitidas por unidade ao dia depende do tipo de preço do seu hub. Por exemplo, se você quiser que o hub dê suporte à entrada de 700 mil mensagens, escolha duas unidades do nível S1.
    Para obter detalhes sobre as outras opções da camada, consulte [Escolher a camada certa do Hub IoT](../articles/iot-hub/iot-hub-scaling.md).

    - **Central de Segurança do Azure**: Ative essa opção para adicionar uma camada extra de proteção contra ameaças à IoT e aos seus dispositivos. Ela não está disponível para hubs na camada gratuita. Para mais informações sobre esse recurso, veja [Central de Segurança do Azure para IoT](https://docs.microsoft.com/azure/asc-for-iot/).

    - **Configurações avançadas** > **Partições de dispositivo para nuvem**: Essa propriedade está relacionada a mensagens de dispositivo para nuvem para o número de leitores simultâneos das mensagens. A maioria dos hubs precisa apenas de quatro partições.

1. Para este artigo, aceite as escolhas padrão e, em seguida, selecione **Avançar: Marcas** para acessar a próxima tela.

    Marcas são pares nome/valor. Você pode atribuir a mesma marca a vários recursos e grupos de recursos para categorizá-los e consolidar a cobrança.

   ![Definir o tamanho e a escala para um novo hub usando o portal do Azure](./media/iot-hub-include-create-hub/iot-hub-create-tabs.png)

    Selecione **Avançar: Analisar + criar** para examinar suas escolhas. Você verá algo semelhante a esta tela.

   ![Examinar informações para criar o hub](./media/iot-hub-include-create-hub/iot-hub-create-review.png)

1. Selecione **Criar** para criar o hub. Criar o hub leva alguns minutos.

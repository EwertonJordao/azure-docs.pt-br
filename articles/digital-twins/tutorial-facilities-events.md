---
title: 'Tutorial: Capturar eventos de dispositivo de um espaço IoT – Gêmeos Digitais do Azure | Microsoft Docs'
description: Saiba como receber notificações de seus espaços integrando os Gêmeos Digitais do Azure ao Aplicativo Lógico do Azure usando as etapas deste tutorial.
services: digital-twins
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.custom: seodec18
ms.service: digital-twins
ms.topic: tutorial
ms.date: 01/10/2020
ms.openlocfilehash: 1cd617204bbc12a99b6ae9e3b55fbc59b0e0578a
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/24/2020
ms.locfileid: "75933756"
---
# <a name="tutorial-receive-notifications-from-your-azure-digital-twins-spaces-by-using-logic-apps"></a>Tutorial: Receber notificações de espaços dos Gêmeos Digitais do Azure usando Aplicativos Lógicos

Depois de implantar sua instância dos Gêmeos Digitais do Azure, provisionar seus espaços e implementar funções personalizadas para monitorar condições específicas, você poderá notificar o administrador do escritório por email quando as condições monitoradas ocorrerem.

No [primeiro tutorial](tutorial-facilities-setup.md), você configurou o grafo espacial de um prédio imaginário. Uma sala no prédio contém sensores de temperatura, dióxido de carbono e movimento. No [segundo tutorial](tutorial-facilities-udf.md), você provisionou o grafo e uma função definida pelo usuário para monitorar esses valores de sensor e disparar notificações quando a sala está vazia e a temperatura e o nível de dióxido de carbono estão em um patamar confortável. 

Este tutorial mostra como você pode integrar essas notificações aos Aplicativos Lógicos do Azure para enviar emails quando uma sala assim está disponível. Um administrador do escritório pode usar essas informações para tornar a reserva de salas de reunião pelos funcionários mais eficiente.

Neste tutorial, você aprenderá como:

> [!div class="checklist"]
> * Integrar eventos à Grade de Eventos do Azure.
> * Notificar eventos com o Aplicativo Lógico.

## <a name="prerequisites"></a>Pré-requisitos

Este tutorial pressupõe que você [configurou](tutorial-facilities-setup.md) e [provisionou](tutorial-facilities-udf.md) sua configuração dos Gêmeos Digitais do Azure. Antes de prosseguir, verifique se você tem:

- Uma [conta do Azure](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Uma instância de Gêmeos Digitais em execução.
- Os [exemplos de C# dos Gêmeos Digitais](https://github.com/Azure-Samples/digital-twins-samples-csharp) baixados e extraídos do seu computador de trabalho.
- [SDK do .NET Core versão 2.1.403 ou posterior](https://www.microsoft.com/net/download) no computador de desenvolvimento para executar o exemplo. Execute `dotnet --version` para verificar se a versão instalada é a correta.
- Uma conta do [Office 365](https://products.office.com/home) para enviar emails de notificação.

> [!TIP]
> Use um nome de instância dos Gêmeos Digitais exclusivo quando estiver provisionando uma nova instância.

## <a name="integrate-events-with-event-grid"></a>Integrar eventos à Grade de Eventos

Nesta seção, você configura uma [Grade de Eventos](../event-grid/overview.md) para coletar eventos da instância dos Gêmeos Digitais do Azure e os redireciona para um [manipulador de eventos](../event-grid/event-handlers.md) como o Aplicativo Lógico.

### <a name="create-an-event-grid-topic"></a>Criar um tópico de grade de eventos

Os [tópicos da grade de eventos](../event-grid/concepts.md#topics) fornecem uma interface para rotear os eventos gerados pela função definida pelo usuário. 

1. Entre no [portal do Azure](https://portal.azure.com).

1. No painel esquerdo, selecione **Criar um recurso**. 

1. Pesquise e selecione **Tópico da Grade de Eventos**. Selecione **Criar**.

1. Insira um **Nome** para o tópico da grade de eventos e escolha a **Assinatura**. Selecione o **Grupo de recursos** usado ou criado para sua instância dos Gêmeos Digitais e o **Local**. Selecione **Criar**. 

    [![Criar um tópico de grade de eventos](./media/tutorial-facilities-events/create-event-grid-topic.png)](./media/tutorial-facilities-events/create-event-grid-topic.png#lightbox)

1. Navegue até o tópico da grade de eventos a partir do grupo de recursos, selecione **Visão Geral** e copie o valor de **Ponto de Extremidade do Tópico** em um arquivo temporário. Você precisará dessa URL nas próximas seções. 

1. Selecione **Chaves de acesso** e copie a **Chave 1** e a **Chave 2** em um arquivo temporário. Você precisará desses valores para criar o ponto de extremidade na próxima seção.

    [![Chaves da Grade de Eventos](./media/tutorial-facilities-events/tutorial-event-grid-keys.png)](./media/tutorial-facilities-events/tutorial-event-grid-keys.png#lightbox)

### <a name="create-an-endpoint-for-the-event-grid-topic"></a>Criar um ponto de extremidade para o tópico da grade de eventos

1. Na janela de comando, verifique se você está na pasta **occupancy-quickstart\src** do exemplo dos Gêmeos Digitais.

1. Abra o arquivo **actions\createEndpoints.yaml** no editor do Visual Studio Code. Verifique se ele tem o seguinte conteúdo:

    ```yaml
    - type: EventGrid
      eventTypes:
      - SensorChange
      - SpaceChange
      - TopologyOperation
      - UdfCustom
      connectionString: <Primary connection string for your Event Grid>
      secondaryConnectionString: <Secondary connection string for your Event Grid>
      path: <Event Grid Topic Name without https:// and /api/events, e.g. eventgridname.region.eventgrid.azure.net>
    ```

1. Substitua o espaço reservado `<Primary connection string for your Event Grid>` pelo valor da **Chave 1**.

1. Substitua o espaço reservado `<Secondary connection string for your Event Grid>` pelo valor da **Chave 2**.

1. Substitua o espaço reservado do **caminho** pelo caminho do tópico da grade de eventos. Obtenha esse caminho removendo o **https://** e os caminhos de recurso à direita da URL do **Ponto de Extremidade do Tópico**. Ele deve ficar aproximadamente com este formato: *seuNomedaGradedeEvento.seuLocal.eventgrid.azure.net*.

    > [!IMPORTANT]
    > Insira todos os valores sem aspas. Verifique se há pelo menos um caractere de espaço após os dois-pontos no arquivo YAML. Você também pode validar o conteúdo do arquivo YAML usando qualquer validador YAML online, como [esta ferramenta](https://onlineyamltools.com/validate-yaml).

1. Salve e feche o arquivo. Na janela de comando, execute o comando a seguir e entre quando solicitado. 

    ```cmd/sh
    dotnet run CreateEndpoints
    ```

   Este comando cria o ponto de extremidade para a Grade de Eventos. 

   [![Pontos de extremidade da Grade de Eventos](./media/tutorial-facilities-events/dotnet-create-endpoints.png)](./media/tutorial-facilities-events/dotnet-create-endpoints.png#lightbox)

## <a name="notify-events-with-logic-apps"></a>Notificar eventos com o Aplicativo Lógico

Você pode usar o serviço [Aplicativos Lógicos do Azure](../logic-apps/logic-apps-overview.md) a fim de criar tarefas automatizadas para eventos recebidos de outros serviços. Nesta seção, você configura o Aplicativo Lógico para criar notificações por email para eventos roteados a partir de seus sensores espaciais, com a ajuda de um [tópico da grade de eventos](../event-grid/overview.md).

1. No painel esquerdo do [portal do Azure](https://portal.azure.com), selecione **Criar um recurso**.

1. Pesquise e selecione um novo recurso **Aplicativo Lógico**. Selecione **Criar**.

1. Insira um **Nome** para o recurso de aplicativo lógico e selecione a **Assinatura**, o **Grupo de recursos** e o **Local**. Selecione **Criar**.

    [![Criar um recurso dos Aplicativos Lógicos](./media/tutorial-facilities-events/tutorial-create-logic-app.png)](./media/tutorial-facilities-events/tutorial-create-logic-app.png#lightbox)

1. Abra o recurso dos Aplicativos Lógicos quando ele for implantado e, em seguida, abra o painel **Designer de Aplicativo Lógico**. 

1. Selecione o gatilho **Quando um evento de recurso da Grade de Eventos ocorrer**. Expanda a opção **Grade de Eventos do Azure** e entre no locatário com a sua conta do Azure quando solicitado. Selecione **Permitir acesso** para o recurso da Grade de Eventos, se solicitado. Selecione **Continuar**.

1. Na janela **Quando um evento de recurso ocorrer**: 
   
   a. Selecione a **Assinatura** que você usou para criar o tópico da grade de eventos.

   b. Selecione **Microsoft.EventGrid.Topics** como **Tipo de Recurso**.

   c. Selecione o recurso Grade de Eventos na lista suspensa do **Nome do Recurso**.

   [![Painel do Designer de Aplicativo Lógico](./media/tutorial-facilities-events/logic-app-resource-event.png)](./media/tutorial-facilities-events/logic-app-resource-event.png#lightbox)

1. Selecione o botão **Nova etapa**.

1. Na janela **Escolher uma ação**:

   a. Pesquise a frase **analisar json**e selecione a ação **Analisar JSON**.

   b. No campo **Conteúdo**, selecione **Corpo** na lista **Conteúdo dinâmico**.

   c. Selecione **Use o conteúdo de amostra para gerar o esquema**. Cole a carga JSON a seguir e selecione **Concluído**.

    ```JSON
    {
    "id": "32162f00-a8f1-4d37-aee2-9312aabba0fd",
    "subject": "UdfCustom",
    "data": {
      "TopologyObjectId": "20efd3a8-34cb-4d96-a502-e02bffdabb14",
      "ResourceType": "Space",
      "Payload": "\"Air quality is poor.\"",
      "CorrelationId": "32162f00-a8f1-4d37-aee2-9312aabba0fd"
    },
    "eventType": "UdfCustom",
    "eventTime": "0001-01-01T00:00:00Z",
    "dataVersion": "1.0",
    "metadataVersion": "1",
    "topic": "/subscriptions/a382ee71-b48e-4382-b6be-eec7540cf271/resourceGroups/HOL/providers/Microsoft.EventGrid/topics/DigitalTwinEventGrid"
    }
    ```

    Essa carga tem valores fictícios. O Aplicativo Lógico usa essa carga de exemplo para gerar um *esquema*.

    [![Janela de análise de JSON dos Aplicativos Lógicos para a Grade de Eventos](./media/tutorial-facilities-events/logic-app-parse-json.png)](./media/tutorial-facilities-events/logic-app-parse-json.png#lightbox)

1. Selecione o botão **Nova etapa**.

1. Na janela **Escolher uma ação**:

   a. Selecione **Controle > Condição** ou pesquise **Condição** na lista **Ações**. 

   b. Na primeira caixa de texto **Escolher um valor**, selecione **eventType** na lista **Conteúdo dinâmico** para a janela **Analisar JSON**.

   c. Na segunda caixa de texto **Escolher um valor**, insira `UdfCustom`.

   [![Condições selecionadas](./media/tutorial-facilities-events/tutorial-logic-app-condition.png)](./media/tutorial-facilities-events/tutorial-logic-app-condition.png#lightbox)

1. Na janela **Se verdadeiro**:

   a. Selecione **Adicionar uma ação** e selecione **Outlook para Office 365**.

   b. Na lista **Ações**, selecione **Enviar um email (V2)** . Selecione **Entrar** e use suas credenciais de conta de email. Selecione **Permitir acesso**, se solicitado.

   c. Na caixa **Para**, insira a ID do email para receber notificações. Em **Assunto**, digite o texto **Notificação dos Gêmeos Digitais sobre a má qualidade do ar no espaço**. Em seguida, selecione **TopologyObjectId** na lista **Conteúdo dinâmico** de **Parse JSON**.

   d. Em **Corpo** na mesma janela, insira um texto semelhante ao seguinte: **Má qualidade do ar detectada em uma sala; a temperatura precisa ser ajustada**. Você pode elaborar mais usando elementos da lista **Conteúdo dinâmico**.

   [![Seleções de "Enviar um email" dos Aplicativos Lógicos](./media/tutorial-facilities-events/tutorial-logic-app-send-email.png)](./media/tutorial-facilities-events/tutorial-logic-app-send-email.png#lightbox)

1. Selecione o botão **Salvar** na parte superior do painel **Designer de Aplicativo Lógico**.

1. Não deixe de simular dados de sensor navegando até a pasta **device-connectivity** do exemplo dos Gêmeos Digitais em uma janela de comando e executando `dotnet run`.

Em alguns minutos, você deve começar a receber notificações por email do recurso do Aplicativo Lógico. 

   [![Notificação por email](./media/tutorial-facilities-events/logic-app-notification.png)](./media/tutorial-facilities-events/logic-app-notification.png#lightbox)

Para interromper o recebimento desses emails, vá para o recurso do Aplicativo Lógico no portal e selecione o painel **Visão Geral**. Selecione **Desabilitar**.

## <a name="clean-up-resources"></a>Limpar os recursos

Se você quiser parar de explorar os Gêmeos Digitais do Azure neste momento, fique à vontade para excluir recursos criados neste tutorial:

1. No menu à esquerda no [portal do Azure](https://portal.azure.com), escolha **Todos os recursos**, marque o grupo de recursos dos Gêmeos Digitais e a opção **Excluir**.

    > [!TIP]
    > Se você teve problemas para excluir sua instância de Gêmeos Digitais, lançamos uma atualização de serviço com a correção. Tente novamente excluir a instância.

2. Se necessário, exclua os aplicativos de exemplo em seu computador de trabalho.

## <a name="next-steps"></a>Próximas etapas

Para aprender a visualizar os dados de sensor, analisar tendências e detectar anomalias, prossiga para o próximo tutorial:

> [!div class="nextstepaction"]
> [Tutorial: Visualizar e analisar eventos de espaços dos Gêmeos Digitais do Azure usando o Time Series Insights](tutorial-facilities-analyze.md)

Também é possível saber mais sobre grafos de inteligência espacial e modelos de objeto nos Gêmeos Digitais do Azure:

> [!div class="nextstepaction"]
> [Noções básicas sobre modelos de objeto e grafos de inteligência espacial dos Gêmeos Digitais](concepts-objectmodel-spatialgraph.md)

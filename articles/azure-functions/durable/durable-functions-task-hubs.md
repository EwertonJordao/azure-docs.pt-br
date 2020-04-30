---
title: Hubs de tarefas nas Funções Duráveis – Azure
description: Saiba o que é um hub de tarefas na extensão de Funções Duráveis do Azure Functions. Saiba como configurar os hubs de tarefas.
author: cgillum
ms.topic: conceptual
ms.date: 11/03/2019
ms.author: azfuncdf
ms.openlocfilehash: 427ab6c4e0e769ab881af0af3023d514c1b092c6
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81604606"
---
# <a name="task-hubs-in-durable-functions-azure-functions"></a>Hubs de tarefas nas Funções Duráveis (Azure Functions)

Um *hub de tarefas* nas [Funções Duráveis](durable-functions-overview.md) é um contêiner lógico dos recursos do Armazenamento do Azure usado para orquestrações. Funções de orquestrador e atividade só podem interagir entre si quando pertencem ao mesmo hub de tarefas.

Se vários aplicativos de funções compartilham uma conta de armazenamento, cada aplicativo de funções *deve* ser configurado com um nome de hub de tarefas separado. Uma conta de armazenamento pode conter vários hubs de tarefas. O diagrama a seguir ilustra um hub de tarefas por aplicativo de funções em contas de armazenamento compartilhadas e dedicadas.

![Diagrama mostrando contas de armazenamento compartilhadas e dedicadas.](./media/durable-functions-task-hubs/task-hubs-storage.png)

## <a name="azure-storage-resources"></a>Recursos de Armazenamento do Azure

Um hub de tarefas é composto pelos seguinte recursos de armazenamento:

* Uma ou mais filas de controle.
* Uma fila de item de trabalho.
* Uma tabela de histórico.
* Uma tabela de instâncias.
* Um contêiner de armazenamento que contém um ou mais blobs de concessão.
* Um contêiner de armazenamento que contém cargas de mensagens grandes, se aplicável.

Todos esses recursos são criados automaticamente na conta de armazenamento do Azure padrão quando as funções Orchestrator, Entity ou Activity são executadas ou estão agendadas para execução. O artigo [Desempenho e Escala](durable-functions-perf-and-scale.md) explica como esses recursos são usados.

## <a name="task-hub-names"></a>Nomes de hub de tarefas

Os hubs de tarefas são identificados por um nome que está de acordo com estas regras:

* Contém apenas caracteres alfanuméricos
* Começa com uma letra
* Tem um comprimento mínimo de 3 caracteres, o comprimento máximo de 45 caracteres

O nome do hub de tarefas é declarado no arquivo *host. JSON* , conforme mostrado no exemplo a seguir:

### <a name="hostjson-functions-20"></a>host. JSON (funções 2,0)

```json
{
  "version": "2.0",
  "extensions": {
    "durableTask": {
      "hubName": "MyTaskHub"
    }
  }
}
```

### <a name="hostjson-functions-1x"></a>host.json (funções 1.x)

```json
{
  "durableTask": {
    "hubName": "MyTaskHub"
  }
}
```

Os hubs de tarefas também podem ser configurados usando as configurações do aplicativo `host.json` , conforme mostrado no seguinte arquivo de exemplo:

### <a name="hostjson-functions-10"></a>host. JSON (funções 1,0)

```json
{
  "durableTask": {
    "hubName": "%MyTaskHub%"
  }
}
```

### <a name="hostjson-functions-20"></a>host. JSON (funções 2,0)

```json
{
  "version": "2.0",
  "extensions": {
    "durableTask": {
      "hubName": "%MyTaskHub%"
    }
  }
}
```

O nome do hub de tarefas será definido com o valor da configuração do aplicativo `MyTaskHub`. O seguinte `local.settings.json` demonstra como definir a `MyTaskHub`configurar como `samplehubname`:

```json
{
  "IsEncrypted": false,
  "Values": {
    "MyTaskHub" : "samplehubname"
  }
}
```

O código a seguir demonstra como escrever uma função que usa a [Associação de cliente de orquestração](durable-functions-bindings.md#orchestration-client) para trabalhar com um hub de tarefas que está configurado como uma configuração de aplicativo:

# <a name="c"></a>[C#](#tab/csharp)

```csharp
[FunctionName("HttpStart")]
public static async Task<HttpResponseMessage> Run(
    [HttpTrigger(AuthorizationLevel.Function, methods: "post", Route = "orchestrators/{functionName}")] HttpRequestMessage req,
    [OrchestrationClient(TaskHub = "%MyTaskHub%")] IDurableOrchestrationClient starter,
    string functionName,
    ILogger log)
{
    // Function input comes from the request content.
    dynamic eventData = await req.Content.ReadAsAsync<object>();
    string instanceId = await starter.StartNewAsync(functionName, eventData);

    log.LogInformation($"Started orchestration with ID = '{instanceId}'.");

    return starter.CreateCheckStatusResponse(req, instanceId);
}
```

> [!NOTE]
> O exemplo anterior de C# é para Durable Functions 2. x. Para Durable Functions 1. x, você deve usar `DurableOrchestrationContext` em vez `IDurableOrchestrationContext`de. Para obter mais informações sobre as diferenças entre versões, consulte o artigo [Durable Functions versões](durable-functions-versions.md) .

# <a name="javascript"></a>[JavaScript](#tab/javascript)

A propriedade do hub de tarefas no arquivo `function.json` é definida por meio da Configuração de Aplicativo:

```json
{
    "name": "input",
    "taskHub": "%MyTaskHub%",
    "type": "orchestrationClient",
    "direction": "in"
}
```

---

Nomes de hubs de tarefas devem começar com uma letra e devem ser compostos somente por letras e números. Se não for especificado, um nome de Hub de tarefas padrão será usado conforme mostrado na tabela a seguir:

| Versão de extensão durável | Nome do hub de tarefas padrão |
| - | - |
| 2. x | Quando implantado no Azure, o nome do hub de tarefas é derivado do nome do _aplicativo de funções_. Ao executar fora do Azure, o nome do hub de tarefas `TestHubName`padrão é. |
| 1.x | O nome do hub de tarefas padrão para todos `DurableFunctionsHub`os ambientes é. |

Para obter mais informações sobre as diferenças entre as versões de extensão, consulte o artigo [Durable Functions versões](durable-functions-versions.md) .

> [!NOTE]
> O nome é o que diferencia um hub de tarefas de outro quando há vários em uma conta de armazenamento compartilhada. Se você tiver vários aplicativos de funções compartilhando uma conta de armazenamento, será necessário configurar, explicitamente, nomes diferentes para cada hub de tarefas nos arquivos *host.json*. Caso contrário, os aplicativos de várias funções competirão entre si para mensagens, o que pode resultar em um `Pending` comportamento indefinido, incluindo orquestrações sendo inesperadas "presas" no estado ou. `Running`

## <a name="next-steps"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Saiba como lidar com o controle de versão de orquestração](durable-functions-versioning.md)

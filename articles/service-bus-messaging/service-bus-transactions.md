---
title: Visão geral do processamento de transações no barramento de serviço do Azure
description: Este artigo fornece uma visão geral do processamento de transações e do recurso enviar via no barramento de serviço do Azure.
ms.topic: article
ms.date: 06/23/2020
ms.custom: devx-track-csharp
ms.openlocfilehash: f51e570775fbce8a316d98b5198fa906173dc755
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/09/2020
ms.locfileid: "88999947"
---
# <a name="overview-of-service-bus-transaction-processing"></a>Visão geral do processamento de transações do Barramento de Serviço

Este artigo aborda as funcionalidades de transação do Barramento de Serviço do Microsoft Azure. Grande parte da discussão é ilustrada pelo [exemplo transações de AMQP com o barramento de serviço](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.Azure.ServiceBus/TransactionsAndSendVia/TransactionsAndSendVia/AMQPTransactionsSendVia). Este artigo é limitado a uma visão geral do processamento de transações e ao recurso *Enviar por* do Barramento de Serviço, enquanto a amostra Transações atômicas tem um escopo mais amplo e complexo.

## <a name="transactions-in-service-bus"></a>Transações no Barramento de Serviço

Uma *transação* agrupa duas ou mais operações em um *escopo de execução*. Por natureza, essa transação deve garantir que todas as operações que pertencem a determinado grupo de operações sejam concluídas com êxito ou com falha em conjunto. Nesse sentido, as transações agem como uma unidade, que, geralmente, é conhecida como *atomicidade*.

O Barramento de Serviço é um agente de mensagens transacionais e assegura a integridade transacional de todas as operações internas em seus repositórios de mensagens. Todas as transferências de mensagens no Barramento de Serviço, como a movimentação de mensagens para uma [fila de mensagens mortas](service-bus-dead-letter-queues.md) ou [encaminhamento automático](service-bus-auto-forwarding.md) de mensagens entre entidades, são transacionais. Assim, caso o Barramento de Serviço aceite uma mensagem, isso significa ela já foi armazenada e rotulada com um número de sequência. Daí em diante, todas as transferências de mensagens no Barramento de Serviço são operações coordenadas entre entidades e não resultarão em perda (origem com êxito e destino com falha) nem em duplicação (origem com falha e destino com êxito) da mensagem.

O Barramento de Serviço dá suporte a operações de agrupamento em uma única entidade de mensagens (fila, tópico e assinatura) no escopo de uma transação. Por exemplo, você pode enviar várias mensagens para uma fila em um escopo de transação, e as mensagens só serão confirmadas no log da fila quando a transação for concluída com êxito.

## <a name="operations-within-a-transaction-scope"></a>Operações em um escopo de transação

As operações que podem ser executadas em um escopo de transação são as seguintes:

* ** [QueueClient](/dotnet/api/microsoft.azure.servicebus.queueclient), [MessageSender](/dotnet/api/microsoft.azure.servicebus.core.messagesender), [TopicClient](/dotnet/api/microsoft.azure.servicebus.topicclient)**: `Send` , `SendAsync` , `SendBatch` ,`SendBatchAsync`
* **[BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage)**:,,,,,,,, `Complete` `CompleteAsync` `Abandon` `AbandonAsync` `Deadletter` `DeadletterAsync` `Defer` `DeferAsync` `RenewLock` , `RenewLockAsync` 

As operações de recebimento não são incluídas, pois presume-se que o aplicativo obtenha as mensagens usando o modo [ReceiveMode.PeekLock](/dotnet/api/microsoft.azure.servicebus.receivemode) em algum loop de recebimento ou com um retorno de chamada [OnMessage](/dotnet/api/microsoft.servicebus.messaging.queueclient.onmessage) e só então abre um escopo de transação para o processamento da mensagem.

Em seguida, a disposição da mensagem (conclusão, abandono, mensagens mortas, adiamento) ocorre no escopo do resultado geral da transação e depende dele.

## <a name="transfers-and-send-via"></a>Transferências e “enviar por”

Para habilitar a transferência transacional dos dados de uma fila para um processador e, em seguida, para outra fila, o Barramento de Serviço dá suporte a *transferências*. Em uma operação de transferência, um remetente primeiro envia uma mensagem para uma *fila de transferência*, e a fila de transferência move imediatamente a mensagem para a fila de destino pretendida usando a mesma implementação de transferência robusta da qual o recurso de avanço depende. A mensagem nunca é confirmada no log da fila de transferência de modo que fique visível para os consumidores da fila de transferência.

A potência dessa funcionalidade transacional se torna aparente quando a própria fila de transferência é a origem das mensagens de entrada do remetente. Em outras palavras, o Barramento de Serviço pode transferir a mensagem para a fila de destino por meio da fila de transferência, ao mesmo tempo que executa uma operação de conclusão (ou adiamento ou mensagens mortas) nas mensagens de entrada, tudo em uma única operação atômica. 

### <a name="see-it-in-code"></a>Ver em código

Para configurar essas transferências, você cria um remetente da mensagem que tem como alvo a fila de destino por meio da fila de transferência. Você também terá um receptor que fará o pull das mensagens dessa mesma fila. Por exemplo:

```csharp
var connection = new ServiceBusConnection(connectionString);

var sender = new MessageSender(connection, QueueName);
var receiver = new MessageReceiver(connection, QueueName);
```

Uma transação simples usa esses elementos, como no exemplo a seguir. Para fazer referência ao exemplo completo, consulte o [código-fonte no GitHub](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.Azure.ServiceBus/TransactionsAndSendVia/TransactionsAndSendVia/AMQPTransactionsSendVia):

```csharp
var receivedMessage = await receiver.ReceiveAsync();

using (var ts = new TransactionScope(TransactionScopeAsyncFlowOption.Enabled))
{
    try
    {
        // do some processing
        if (receivedMessage != null)
            await receiver.CompleteAsync(receivedMessage.SystemProperties.LockToken);

        var myMsgBody = new MyMessage
        {
            Name = "Some name",
            Address = "Some street address",
            ZipCode = "Some zip code"
        };

        // send message
        var message = myMsgBody.AsMessage();
        await sender.SendAsync(message).ConfigureAwait(false);
        Console.WriteLine("Message has been sent");

        // complete the transaction
        ts.Complete();
    }
    catch (Exception ex)
    {
        // This rolls back send and complete in case an exception happens
        ts.Dispose();
        Console.WriteLine(ex.ToString());
    }
}
```

## <a name="timeout"></a>Tempo limite
Uma transação atinge o tempo limite após 2 minutos. O temporizador de transação é iniciado quando a primeira operação na transação é iniciada. 

## <a name="next-steps"></a>Próximas etapas

Confira os artigos a seguir para obter mais informações sobre as filas do Barramento de Serviço:

* [Como usar filas do Barramento de Serviço](service-bus-dotnet-get-started-with-queues.md)
* [Encadeando entidades do Barramento de Serviço com o encaminhamento automático](service-bus-auto-forwarding.md)
* [Amostra de avanço automático](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/AutoForward)
* [Amostra de Transações atômicas com o Barramento de Serviço](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/AtomicTransactions)
* [Filas do Azure e filas do Barramento de Serviço – comparações](service-bus-azure-and-service-bus-queues-compared-contrasted.md)



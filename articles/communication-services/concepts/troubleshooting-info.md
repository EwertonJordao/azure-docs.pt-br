---
title: Solução de problemas nos Serviços de Comunicação do Azure
description: Saiba como reunir as informações necessárias para solucionar problemas de sua solução de Serviços de Comunicação.
author: manoskow
manager: jken
services: azure-communication-services
ms.author: manoskow
ms.date: 10/23/2020
ms.topic: overview
ms.service: azure-communication-services
ms.openlocfilehash: ff3e7fee87661fb89ba930b7368bd54e71ad57bf
ms.sourcegitcommit: 6a902230296a78da21fbc68c365698709c579093
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/05/2020
ms.locfileid: "93357616"
---
# <a name="troubleshooting-in-azure-communication-services"></a>Solução de problemas nos Serviços de Comunicação do Azure

Este documento ajudará você a solucionar problemas que podem ocorrer na solução de Serviços de Comunicação. Se estiver solucionando problemas de SMS, você poderá [habilitar o relatório de entrega com a Grade de Eventos](../quickstarts/telephony-sms/handle-sms-events.md) para capturar os detalhes da entrega do SMS.

## <a name="getting-help"></a>Obtendo ajuda

Incentivamos os desenvolvedores a enviar perguntas, sugerir recursos e relatar problemas como problemas no repositório de Serviços de Comunicação [GitHub](https://github.com/Azure/communication). Outros fóruns incluem:

* [P e R da Microsoft](https://docs.microsoft.com/answers/questions/topics/single/101418.html)
* [StackOverflow](https://stackoverflow.com/questions/tagged/azure+communication)

Dependendo do [plano de suporte](https://azure.microsoft.com/support/plans/) de assinatura do Azure, você pode enviar um tíquete de suporte diretamente por meio do [portal do Azure](https://azure.microsoft.com/support/create-ticket/).

Para ajudá-lo a solucionar determinados tipos de problemas, você pode ser solicitado a fornecer qualquer uma das seguintes informações:

* **ID MS-CV**: Essa ID é usada para solucionar problemas de chamadas e mensagens. 
* **ID da chamada**: Essa ID é usada para identificar chamadas de Serviços de Comunicação.
* **ID da mensagem do SMS**: Essa ID é usada para identificar mensagens SMS.
* **Logs de chamada**: Esses logs contêm informações detalhadas que podem ser usadas para solucionar problemas de chamada e de rede.


## <a name="access-your-ms-cv-id"></a>Acessar sua ID do MS-CV

A ID do MS-CV pode ser acessada configurando o diagnóstico na instância do objeto `clientOptions` ao inicializar suas bibliotecas de cliente. O diagnóstico pode ser configurado para qualquer uma das bibliotecas de cliente do Azure, incluindo chat, administração e chamada VoIP.

### <a name="client-options-example"></a>Exemplo das opções do cliente

Os trechos de código a seguir demonstram a configuração de diagnóstico. Quando as bibliotecas de cliente são usadas com o diagnóstico habilitado, os detalhes do diagnóstico serão emitidos para o ouvinte de evento configurado:

# <a name="c"></a>[C#](#tab/csharp)
``` 
// 1. Import Azure.Core.Diagnostics
using Azure.Core.Diagnostics;

// 2. Initialize an event source listener instance
using var listener = AzureEventSourceListener.CreateConsoleLogger();
Uri endpoint = new Uri("https://<RESOURCE-NAME>.communication.azure.net");
var (token, communicationUser) = await GetCommunicationUserAndToken();
CommunicationUserCredential communicationUserCredential = new CommunicationUserCredential(token);

// 3. Setup diagnostic settings
var clientOptions = new ChatClientOptions()
{
    Diagnostics =
    {
        LoggedHeaderNames = { "*" },
        LoggedQueryParameters = { "*" },
        IsLoggingContentEnabled = true,
    }
};

// 4. Initialize the ChatClient instance with the clientOptions 
ChatClient chatClient = new ChatClient(endpoint, communicationUserCredential, clientOptions);
ChatThreadClient chatThreadClient = await chatClient.CreateChatThreadAsync("Thread Topic", new[] { new ChatThreadMember(communicationUser) });
```

# <a name="python"></a>[Python](#tab/python)
``` 
from azure.communication.chat import ChatClient, CommunicationUserCredential
endpoint = "https://communication-services-sdk-live-tests-for-python.communication.azure.com"
chat_client = ChatClient(
    endpoint,
    CommunicationUserCredential(token),
    http_logging_policy=your_logging_policy)
```
---

## <a name="access-your-call-id"></a>Acessar sua ID de chamada

Ao fazer o arquivamento de uma solicitação de suporte por meio do portal do Azure relacionado à chamada de problemas, você pode ser solicitado a fornecer a ID da chamada à qual está se referindo. Isso pode ser acessado por meio da biblioteca de cliente de chamada:

# <a name="javascript"></a>[JavaScript](#tab/javascript)
```javascript
// `call` is an instance of a call created by `callAgent.call` or `callAgent.join` methods 
console.log(call.id)
```

# <a name="ios"></a>[iOS](#tab/ios)
```objc
// The `call id` property can be retrieved by calling the `call.getCallId()` method on a call object after a call ends 
// todo: the code snippet suggests it's a property while the comment suggests it's a method call
print(call.callId) 
```

# <a name="android"></a>[Android](#tab/android)
```java
// The `call id` property can be retrieved by calling the `call.getCallId()` method on a call object after a call ends
// `call` is an instance of a call created by `callAgent.call(…)` or `callAgent.join(…)` methods 
Log.d(call.getCallId()) 
```
---


## <a name="access-your-sms-message-id"></a>Acessar sua ID de mensagem de SMS

Para problemas de SMS, você pode coletar a ID da mensagem do objeto de resposta.

# <a name="net"></a>[.NET](#tab/dotnet)
```
// Instantiate the SMS client
const smsClient = new SmsClient(connectionString);
async function main() {
  const result = await smsClient.send({
    from: "+18445792722",
    to: ["+1972xxxxxxx"],
    message: "Hello World 👋🏻 via Sms"
  }, {
    enableDeliveryReport: true // Optional parameter
  });
console.log(result); // your message ID will be in the result
}
```
---

## <a name="enable-and-access-call-logs"></a>Habilitar e acessar os logs de chamadas




# <a name="javascript"></a>[JavaScript](#tab/javascript)

O seguinte código pode ser usado a fim de configurar `AzureLogger` para gerar logs no console usando a biblioteca de cliente JavaScript:

```javascript
import { AzureLogger } from '@azure/logger'; 

AzureLogger.verbose = (...args) => { console.info(...args); } 
AzureLogger.info = (...args) => { console.info(...args); } 
AzureLogger.warning = (...args) => { console.info(...args); } 
AzureLogger.error = (...args) => { console.info(...args); } 

callClient = new CallClient({logger: AzureLogger}); 
```

# <a name="ios"></a>[iOS](#tab/ios)

Ao desenvolver para iOS, os logs são armazenados em arquivos `.blog`. Observe que você não pode ver os logs diretamente, pois eles estão criptografados.

Eles podem acessados abrindo o Xcode. Vá para Windows > Dispositivos e Simuladores > Dispositivos. Selecione seu dispositivo. Em Aplicativos Instalados, selecione o aplicativo desejado e clique em "Baixar contêiner". 

Isso lhe proporcionará um arquivo `xcappdata`. Clique com o botão direito do mouse sobre esse arquivo e selecione "Mostrar conteúdo do pacote". Em seguida, você verá os arquivos `.blog` que podem ser anexados à solicitação de Suporte do Azure.

# <a name="android"></a>[Android](#tab/android)

Ao desenvolver para Android, os logs são armazenados em arquivos `.blog`. Observe que você não pode ver os logs diretamente, pois eles estão criptografados.

No Android Studio, navegue até o Explorador de Arquivos do Dispositivo selecionando Exibir > Janelas de Ferramentas > Explorador de Arquivos do Dispositivo tanto do simulador quanto do dispositivo. O arquivo `.blog` estará localizado no diretório do aplicativo, que deve ser semelhante a `/data/data/[app_name_space:com.contoso.com.acsquickstartapp]/files/acs_sdk.blog`. Você pode anexar esse arquivo à sua solicitação de suporte. 
   

---


## <a name="related-information"></a>Informações relacionadas
- [Configurar logs e diagnósticos](logging-and-diagnostics.md)
- [Métricas](metrics.md)

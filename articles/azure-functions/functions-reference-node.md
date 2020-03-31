---
title: Referência do desenvolvedor JavaScript para funções do Azure
description: Entenda como desenvolver funções usando JavaScript.
ms.assetid: 45dedd78-3ff9-411f-bb4b-16d29a11384c
ms.topic: reference
ms.date: 12/17/2019
ms.openlocfilehash: 345df8e1ea88caa6f8dbe941245c1f989c3e81c6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79276823"
---
# <a name="azure-functions-javascript-developer-guide"></a>Guia do desenvolvedor de JavaScript do Azure Functions

Este guia contém informações sobre as complexidades de escrever Azure Functions com JavaScript.

Uma função JavaScript é um `function` exportado que é executado quando acionado ([acionadores são configurados em function.json](functions-triggers-bindings.md)). O primeiro argumento passado para `context` cada função é um objeto, que é usado para receber e enviar dados vinculativos, registrar e se comunicar com o tempo de execução.

Este artigo pressupõe que você já tenha lido a [Referência do desenvolvedor do Azure Functions](functions-reference.md). Complete as funções rapidamente para criar sua primeira função, usando [o Visual Studio Code](functions-create-first-function-vs-code.md) ou no [portal](functions-create-first-azure-function.md).

Este artigo também suporta o [desenvolvimento de aplicativos TypeScript](#typescript).

## <a name="folder-structure"></a>Estrutura de pastas

A estrutura de pastas necessárias para um projeto JavaScript é semelhante à seguinte. Este padrão pode ser alterado. Para mais informações, consulte a seção [scriptArquivo](#using-scriptfile) abaixo.

```
FunctionsProject
 | - MyFirstFunction
 | | - index.js
 | | - function.json
 | - MySecondFunction
 | | - index.js
 | | - function.json
 | - SharedCode
 | | - myFirstHelperFunction.js
 | | - mySecondHelperFunction.js
 | - node_modules
 | - host.json
 | - package.json
 | - extensions.csproj
```

Na raiz do projeto, há um arquivo [host.json](functions-host-json.md) compartilhado que pode ser usado para configurar o aplicativo de funções. Cada função tem uma pasta com seu próprio arquivo de código (.js) e o arquivo de configuração de associação (function.json). O nome do diretório pai de `function.json` é sempre o nome da sua função.

As extensões de associação necessárias na [versão 2.x](functions-versions.md) do runtime do Functions são definidas no arquivo `extensions.csproj`, com os arquivos de biblioteca reais na pasta `bin`. Ao desenvolver localmente, você precisa [registrar as extensões de associação](./functions-bindings-register.md#extension-bundles). Ao desenvolver funções no portal do Azure, esse registro é feito para você.

## <a name="exporting-a-function"></a>Exportando uma função

As funções JavaScript devem [`module.exports`](https://nodejs.org/api/modules.html#modules_module_exports) ser [`exports`](https://nodejs.org/api/modules.html#modules_exports)exportadas via (ou ). Sua função exportada deve ser uma função JavaScript que é executada quando acionada.

Por padrão, o runtime de Funções procura sua função em `index.js`, onde `index.js` compartilha o mesmo diretório pai que o `function.json` correspondente. No caso padrão, sua função exportada deve ser a única exportação de seu arquivo ou a exportação denominada `run` ou `index`. Para configurar o local do arquivo e o nome de exportação da função, leia [Configurando o ponto de entrada da função](functions-reference-node.md#configure-function-entry-point) abaixo.

Sua função exportada é passada um número de argumentos na execução. O primeiro argumento é sempre um objeto `context`. Se sua função for síncrona (não retorna uma `context` Promessa), `context.done` você deve passar o objeto, pois a chamada é necessária para uso correto.

```javascript
// You should include context, other arguments are optional
module.exports = function(context, myTrigger, myInput, myOtherInput) {
    // function logic goes here :)
    context.done();
};
```

### <a name="exporting-an-async-function"></a>Exportando uma função assíncrona
Ao usar [`async function`](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Statements/async_function) a declaração ou simples Promessas [JavaScript](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise) na versão 2.x do tempo [`context.done`](#contextdone-method) de execução Functions, você não precisa chamar explicitamente a chamada de retorno para sinalizar que sua função foi concluída. Sua função é concluída quando a função assíncrona exportada / Promise é concluída. Para funções direcionadas à versão 1.x tempo [`context.done`](#contextdone-method) de execução, você ainda deve ligar quando seu código estiver sendo executado.

O exemplo a seguir é uma função simples que registra que foi acionada e imediatamente conclui a execução.

```javascript
module.exports = async function (context) {
    context.log('JavaScript trigger function processed a request.');
};
```

Ao exportar uma função assíncrona, você também pode configurar uma ligação de saída para obter o valor `return`. Isso é recomendado se você tiver apenas uma associação de saída.

Para atribuir uma saída usando `return`, altere o `name` propriedade a ser `$return` em `function.json`.

```json
{
  "type": "http",
  "direction": "out",
  "name": "$return"
}
```

Nesse caso, sua função deve se parecer com o seguinte exemplo:

```javascript
module.exports = async function (context, req) {
    context.log('JavaScript HTTP trigger function processed a request.');
    // You can call and await an async method here
    return {
        body: "Hello, world!"
    };
}
```

## <a name="bindings"></a>Associações 
Em JavaScript, [ligações](functions-triggers-bindings.md) são configuradas e definidas na função function.json. As funções interagem com as ligações de várias maneiras.

### <a name="inputs"></a>Entradas
As entradas são divididas em duas categorias no Azure Functions: uma é a entrada de gatilho e a outra é a entrada adicional. Trigger e outras ligações de entrada (ligações de `direction === "in"`) podem ser lidas por uma função de três maneiras:
 - **_ [Recomendado] _ Como parâmetros passados para sua função.** Eles são passados para a função na mesma ordem em que são definidos *function.json*. A `name` propriedade definida em *function.json* não precisa corresponder ao nome do seu parâmetro, embora devesse.
 
   ```javascript
   module.exports = async function(context, myTrigger, myInput, myOtherInput) { ... };
   ```
   
 - **Como os membros de [`context.bindings`](#contextbindings-property) objeto.** Observe que a propriedade `name` definida em * function.json * não precisa corresponder ao nome do seu parâmetro, embora deva....
 
   ```javascript
   module.exports = async function(context) { 
       context.log("This is myTrigger: " + context.bindings.myTrigger);
       context.log("This is myInput: " + context.bindings.myInput);
       context.log("This is myOtherInput: " + context.bindings.myOtherInput);
   };
   ```
   
 - **Como entradas usando o [`arguments`](https://msdn.microsoft.com/library/87dw3w1k.aspx)objeto JavaScript.** Isso é essencialmente o mesmo que passar entradas como parâmetros, mas permite que você manipule dinamicamente entradas.
 
   ```javascript
   module.exports = async function(context) { 
       context.log("This is myTrigger: " + arguments[1]);
       context.log("This is myInput: " + arguments[2]);
       context.log("This is myOtherInput: " + arguments[3]);
   };
   ```

### <a name="outputs"></a>outputs
As saídas (ligações de `direction === "out"`) podem ser gravadas por uma função de várias maneiras. Em todos os casos, a propriedade `name` da ligação, conforme definido em * function.json *, corresponde ao nome do membro do objeto gravado na sua função. 

Você pode atribuir dados a vinculações de saída de uma das seguintes maneiras (não combine esses métodos):

- **_ [Recomendado para várias saídas]_ Retornando um objeto.** Se você estiver usando uma função de retorno assincronia/Promessa, você pode retornar um objeto com dados de saída atribuídos. No exemplo abaixo, as ligações de saída são nomeadas "httpResponse" e "queueOutput" em *function.json*.

  ```javascript
  module.exports = async function(context) {
      let retMsg = 'Hello, world!';
      return {
          httpResponse: {
              body: retMsg
          },
          queueOutput: retMsg
      };
  };
  ```

  Se você estiver usando uma função síncrona, você pode retornar este objeto usando [`context.done`](#contextdone-method) (veja exemplo).
- **_[Recomendado para saída única]_ Retornando um valor diretamente e usando o nome de ligação $ return.** Isso funciona apenas para as funções de retorno assíncrono / Promessa. Veja o exemplo em [exportando uma função assíncrona](#exporting-an-async-function). 
- **Atribuindo valores a serem `context.bindings`** você pode atribuir valores diretamente para Context. Bindings.

  ```javascript
  module.exports = async function(context) {
      let retMsg = 'Hello, world!';
      context.bindings.httpResponse = {
          body: retMsg
      };
      context.bindings.queueOutput = retMsg;
      return;
  };
  ```

### <a name="bindings-data-type"></a>Tipo de dados de associações

Para definir o tipo de dados para uma associação de entrada, use a propriedade `dataType` na definição de associação. Por exemplo, para ler o conteúdo de uma solicitação HTTP em formato binário, use o tipo `binary`:

```json
{
    "type": "httpTrigger",
    "name": "req",
    "direction": "in",
    "dataType": "binary"
}
```

As opções para `dataType` são: `binary`, `stream` e `string`.

## <a name="context-object"></a>objeto de contexto
O runtime usa um objeto `context` para passar dados de/para sua função e permitir que você se comunique com o runtime. O objeto de contexto pode ser usado para ler e definir os dados de associações, gravar logs e usando o `context.done` retorno de chamada quando a função exportada é síncrona.

O `context` objeto é sempre o primeiro parâmetro para uma função. Deve ser incluído porque tem métodos importantes, como `context.done` e `context.log`. Você pode nomear o objeto de acordo com a sua preferência (por exemplo, `ctx` ou `c`).

```javascript
// You must include a context, but other arguments are optional
module.exports = function(ctx) {
    // function logic goes here :)
    ctx.done();
};
```

### <a name="contextbindings-property"></a>Propriedade context.bindings

```js
context.bindings
```

Retorna um objeto nomeado que é usado para ler ou atribuir dados de vinculação. Os dados de vinculação de entrada `context.bindings`e gatilho podem ser acessados lendo propriedades em . Os dados de vinculação de saída podem ser atribuídos adicionando dados a`context.bindings`

Por exemplo, as seguintes definições de ligação em sua função.json permitem acessar o conteúdo de uma fila de `context.bindings.myInput`e atribuir saídas a uma fila usando`context.bindings.myOutput`.

```json
{
    "type":"queue",
    "direction":"in",
    "name":"myInput"
    ...
},
{
    "type":"queue",
    "direction":"out",
    "name":"myOutput"
    ...
}
```

```javascript
// myInput contains the input data, which may have properties such as "name"
var author = context.bindings.myInput.name;
// Similarly, you can set your output data
context.bindings.myOutput = { 
        some_text: 'hello world', 
        a_number: 1 };
```

Você pode optar por definir os dados de ligação de saída usando o método `context.done` em vez do objeto `context.binding` (veja abaixo).

### <a name="contextbindingdata-property"></a>Propriedade context.bindingData

```js
context.bindingData
```

Retorna um objeto nomeado que contém dados de invocação de função e os metadados do gatilho (`invocationId`, `sys.methodName`, `sys.utcNow`, `sys.randGuid`). Para obter um exemplo de metadados de gatilho, confira este [exemplo de hubs de eventos](functions-bindings-event-hubs-trigger.md).

### <a name="contextdone-method"></a>Método context.done

```js
context.done([err],[propertyBag])
```

Permite que o runtime saiba que seu código foi concluído. Quando sua função [`async function`](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Statements/async_function) usa a declaração, `context.done()`você não precisa usar . O `context.done` retorno de chamada é chamado implicitamente. As funções assíncronas estão disponíveis no Nó 8 ou em uma versão posterior, que requer a versão 2.x do runtime de Funções.

Se sua função não for uma função assíncrona, **você deve ligar** `context.done` para informar o tempo de execução que sua função está completa. Os tempos de execução se está ausente.

O método `context.done` permite que você retorne um erro definido pelo usuário ao runtime e um objeto JSON que contém dados de associação de saída. As propriedades transmitidas para `context.done`sobrescrevem qualquer coisa definida no objeto`context.bindings`.

```javascript
// Even though we set myOutput to have:
//  -> text: 'hello world', number: 123
context.bindings.myOutput = { text: 'hello world', number: 123 };
// If we pass an object to the done function...
context.done(null, { myOutput: { text: 'hello there, world', noNumber: true }});
// the done method overwrites the myOutput binding to be: 
//  -> text: 'hello there, world', noNumber: true
```

### <a name="contextlog-method"></a>Método context.log  

```js
context.log(message)
```

Permite que você grave em logs de função de streaming no nível de rastreamento padrão. No `context.log`, há métodos de registro adicionais disponíveis para permitir que você grave logs de função em outros níveis de rastreamento:


| Método                 | Descrição                                |
| ---------------------- | ------------------------------------------ |
| **erro _(mensagem)_**   | Grava no registro em log no nível do erro, ou em um nível inferior.   |
| **warn(_message_)**    | Grava no registro em log no nível do aviso, ou em um nível inferior. |
| **info(_message_)**    | Grava no registro em log no nível da informação, ou em um nível inferior.    |
| **verbose(_message_)** | Grava no registro em log no nível detalhado.           |

O exemplo a seguir grava um log no nível de rastreamento de aviso:

```javascript
context.log.warn("Something has happened."); 
```

Você pode [configurar o limite do nível de rastreamento para registro em log](#configure-the-trace-level-for-console-logging) no arquivo host.json. Para obter mais informações sobre como gravar logs, confira [Gravando saídas de rastreamento](#writing-trace-output-to-the-console) abaixo.

Leia [Monitorado o Azure Functions](functions-monitoring.md) para saber mais sobre como exibir e consultar logs de função.

## <a name="writing-trace-output-to-the-console"></a>Gravar a saída de rastreamento no console 

No Functions, use os métodos `context.log` para gravar a saída de rastreamento no console. No Functions v2.x, as saídas de rastreio usando `console.log` são capturadas no nível do Aplicativo Function. Isso significa que `console.log` as saídas de não estão vinculadas a uma invocação de função específica e não são exibidas nos registros de uma função específica. Eles, no entanto, se propagam para o Application Insights. No Functions v1.x, não é possível usar `console.log` para gravar no console.

Quando você chama `context.log()`, sua mensagem é gravada no console no nível de rastreamento padrão, que é o nível de rastreamento de _informações_. O código a seguir grava no console no nível de rastreamento de informações:

```javascript
context.log({hello: 'world'});  
```

Esse código é equivalente ao código acima:

```javascript
context.log.info({hello: 'world'});  
```

Esse código grava no console no nível de erro:

```javascript
context.log.error("An error has occurred.");  
```

Como _erro_ é o nível de rastreamento mais alto, esse rastreamento é gravado na saída em todos os níveis de rastreamento enquanto o registro em log estiver habilitado.

Todos os métodos `context.log` dão suporte ao mesmo formato de parâmetro que o [método util.format](https://nodejs.org/api/util.html#util_util_format_format) de Node.js. Considere o código a seguir, que grava logs de função usando o nível de rastreamento padrão:

```javascript
context.log('Node.js HTTP trigger function processed a request. RequestUri=' + req.originalUrl);
context.log('Request Headers = ' + JSON.stringify(req.headers));
```

Você também pode escrever o mesmo código no formato a seguir:

```javascript
context.log('Node.js HTTP trigger function processed a request. RequestUri=%s', req.originalUrl);
context.log('Request Headers = ', JSON.stringify(req.headers));
```

### <a name="configure-the-trace-level-for-console-logging"></a>Configurar o nível de rastreamento para o registro em log no console

O Functions 1.x permite a definição do nível de rastreamento de limite para gravar no console, o que facilita o controle do modo de gravação dos rastreamentos no console da sua função. Para definir o limite para todos os rastreamentos gravados no console, use a propriedade `tracing.consoleLevel` no arquivo host.json. Essa configuração se aplica a todas as funções em seu aplicativo de função. O exemplo a seguir define o limite de rastreamento para habilitar o registro em log detalhado:

```json
{
    "tracing": {
        "consoleLevel": "verbose"
    }
}  
```

Os valores de **consoleLevel** correspondem aos nomes dos métodos `context.log`. Para desabilitar todo o registro em log do rastreamento no console, defina **consoleLevel** como _off_. Para obter mais informações, consulte a [referência para host.json](functions-host-json-v1.md).

## <a name="http-triggers-and-bindings"></a>Gatilhos e associações HTTP

HTTP e gatilhos de webhook e associações de saída HTTP usam objetos de solicitação e resposta para representar as mensagens HTTP.  

### <a name="request-object"></a>Objeto da solicitação

O objeto `context.req` (solicitação) tem as seguintes propriedades:

| Propriedade      | Descrição                                                    |
| ------------- | -------------------------------------------------------------- |
| _Corpo_        | Um objeto que contém o corpo da solicitação.               |
| _Cabeçalhos_     | Um objeto que contém os cabeçalhos da solicitação.                   |
| _Método_      | O método HTTP da solicitação.                                |
| _Url original_ | A URL da solicitação.                                        |
| _params_      | Um objeto que contém os parâmetros de roteamento da solicitação. |
| _Consulta_       | Um objeto que contém os parâmetros da consulta.                  |
| _rawBody_     | O corpo da mensagem como uma cadeia de caracteres.                           |


### <a name="response-object"></a>Objeto de resposta

O objeto `context.res` (resposta) tem as seguintes propriedades:

| Propriedade  | Descrição                                               |
| --------- | --------------------------------------------------------- |
| _Corpo_    | Um objeto que contém o corpo da resposta.         |
| _Cabeçalhos_ | Um objeto que contém os cabeçalhos da resposta.             |
| _isRaw_   | Indica que a formatação foi ignorada para a resposta.    |
| _status_  | O código de status HTTP da resposta.                     |
| _Cookies_ | Uma matriz de objetos de cookie HTTP definidos na resposta. Um objeto de `name`cookie `value`HTTP tem uma , `maxAge` `sameSite`e outras propriedades de cookies, como ou . |

### <a name="accessing-the-request-and-response"></a>Acessar a solicitação e a resposta 

Ao trabalhar com gatilhos HTTP, há várias maneiras de acessar os objetos de solicitação e resposta HTTP:

+ **Partir `req` e `res` propriedades no `context` objeto.** Dessa forma, você pode usar o padrão convencional para acessar os dados HTTP a partir do objeto de contexto, em vez de usar o padrão `context.bindings.name` completo. O exemplo a seguir mostra como acessar os objetos `req` e `res` no `context`:

    ```javascript
    // You can access your HTTP request off the context ...
    if(context.req.body.emoji === ':pizza:') context.log('Yay!');
    // and also set your HTTP response
    context.res = { status: 202, body: 'You successfully ordered more coffee!' }; 
    ```

+ **A partir das associações de entrada e saída nomeadas.** Dessa forma, o gatilho e as associações de HTTP funcionam da mesma forma que qualquer outra associação. O exemplo a seguir define o objeto de resposta usando uma associação chamada `response`: 

    ```json
    {
        "type": "http",
        "direction": "out",
        "name": "response"
    }
    ```
    ```javascript
    context.bindings.response = { status: 201, body: "Insert succeeded." };
    ```
+ **_[Somente Resposta]_ Chamando`context.res.send(body?: any)`.** Uma resposta HTTP é criada com a entrada `body` como o corpo da resposta. `context.done()` é chamado implicitamente.

+ **_[Somente Resposta]_ Chamando`context.done()`.** Um tipo especial de vinculação HTTP retorna `context.done()` a resposta que é passada para o método. A seguinte associação de saída HTTP define um parâmetro de saída `$return`:

    ```json
    {
      "type": "http",
      "direction": "out",
      "name": "$return"
    }
    ``` 
    ```javascript
     // Define a valid response object.
    res = { status: 201, body: "Insert succeeded." };
    context.done(null, res);   
    ```  

## <a name="scaling-and-concurrency"></a>Dimensionamento e concorrência

Por padrão, o Azure Functions monitora automaticamente a carga em seu aplicativo e cria instâncias de host adicionais para Node.js conforme necessário. As funções usam limiares incorporados (não configuráveis pelo usuário) para diferentes tipos de gatilho para decidir quando adicionar instâncias, como a idade das mensagens e o tamanho da fila para O QueueTrigger. Para obter mais informações, consulte [como funcionam os planos de consumo e premium.](functions-scale.md#how-the-consumption-and-premium-plans-work)

Esse comportamento de dimensionamento é suficiente para muitas aplicações node.js. Para aplicativos vinculados à CPU, você pode melhorar ainda mais o desempenho usando vários processos de trabalho de idioma.

Por padrão, cada instância de host de Funções tem um único processo de trabalhador de idiomas. Você pode aumentar o número de processos de trabalhadorpor host (até 10) usando a configuração de aplicativo [FUNCTIONS_WORKER_PROCESS_COUNT.](functions-app-settings.md#functions_worker_process_count) As funções do Azure, então, tentam distribuir uniformemente invocações de funções simultâneas entre esses trabalhadores. 

O FUNCTIONS_WORKER_PROCESS_COUNT se aplica a cada host que as Funções criam ao dimensionar seu aplicativo para atender à demanda. 

## <a name="node-version"></a>Versão do nó

A tabela a seguir mostra as versões node.js suportadas atualmente para cada versão principal do tempo de execução Functions, pelo sistema operacional:

| Versão do Functions | Versão nó (Windows) | Versão node (Linux) |
|---|---| --- |
| 1.x | 6.11.2 (bloqueada pelo runtime) | n/d |
| 2. x  | ~8<br/>~10 (recomendado)<br/>~12<sup>*</sup> | ~8 (recomendado)<br/>~10  |
| 3.x | ~10<br/>~12 (recomendado)  | ~10<br/>~12 (recomendado) |

<sup>*</sup>O nó ~12 é atualmente permitido na versão 2.x do tempo de execução Functions. No entanto, para obter o melhor desempenho, recomendamos o uso de funções em tempo de execução versão 3.x com node ~12. 

Veja versão atual que o runtime está usando verificando a configuração de aplicativo acima ou imprimindo `process.version` de qualquer função. Direcione a versão no Azure definindo a [configuração](functions-how-to-use-azure-function-app-settings.md#settings) do `~10`aplicativo WEBSITE_NODE_DEFAULT_VERSION para uma versão LTS suportada, como .

## <a name="dependency-management"></a>Gerenciamento de dependência
Para usar as bibliotecas da comunidade no código JavaScript, como é mostrado no exemplo abaixo, você precisa garantir que todas as dependências sejam instaladas no aplicativo de funções no Azure.

```javascript
// Import the underscore.js library
var _ = require('underscore');
var version = process.version; // version === 'v6.5.0'

module.exports = function(context) {
    // Using our imported underscore.js library
    var matched_names = _
        .where(context.bindings.myInput.names, {first: 'Carla'});
```

> [!NOTE]
> Você deve definir um arquivo `package.json` na raiz do seu aplicativo de função. A definição de arquivo permite que todas as funções no aplicativo compartilhem os mesmos pacotes armazenados em cache, o que oferece o melhor desempenho. Se houver conflitos de versão, você poderá resolver o conflito adicionando um arquivo `package.json` na pasta de uma função específica.  

Ao implantar os aplicativos de função do controle de origem, qualquer arquivo `package.json` presente em seu repositório acionará um `npm install` em sua pasta durante a implantação. Mas ao implantar via Portal ou CLI, você terá que instalar manualmente os pacotes.

Há duas maneiras de instalar pacotes no aplicativo de funções: 

### <a name="deploying-with-dependencies"></a>Implantando com dependências
1. Instale todos os pacotes necessários localmente executando `npm install`.

2. Implante o código e verifique se a pasta `node_modules` está incluída na implantação. 


### <a name="using-kudu"></a>Usando o Kudu
1. Ir para `https://<function_app_name>.scm.azurewebsites.net`.

2. Clique **em Debug Console** > **CMD**.

3. Acesse `D:\home\site\wwwroot`e arraste o arquivo package.json para a pasta **wwwroot** na metade superior da página.  
    Também há outras maneiras de carregar arquivos em seu aplicativo de função. Para saber mais, confira [Como atualizar os arquivos do aplicativo de função](functions-reference.md#fileupdate). 

4. Depois que o arquivo package.json é carregado, execute o comando `npm install` no **console de execução remota do Kudu**.  
    Essa ação baixa os pacotes indicados no arquivo package.json e reinicia o aplicativo de função.

## <a name="environment-variables"></a>Variáveis de ambiente

Em funções, [configurações do aplicativo](functions-app-settings.md), como conexão de serviço cadeias de caracteres, são expostas como variáveis de ambiente durante a execução. Você pode acessar essas `process.env`configurações usando , como mostrado `context.log()` aqui na `AzureWebJobsStorage` `WEBSITE_SITE_NAME` segunda e terceira chamadas para onde registramos as variáveis e ambiente:

```javascript
module.exports = async function (context, myTimer) {
    var timeStamp = new Date().toISOString();

    context.log('Node.js timer trigger function ran!', timeStamp);
    context.log("AzureWebJobsStorage: " + process.env["AzureWebJobsStorage"]);
    context.log("WEBSITE_SITE_NAME: " + process.env["WEBSITE_SITE_NAME"]);
};
```

[!INCLUDE [Function app settings](../../includes/functions-app-settings.md)]

Ao executar localmente, as configurações do aplicativo são lidos a partir de [Settings](functions-run-local.md#local-settings-file) arquivo de projeto.

## <a name="configure-function-entry-point"></a>Configurar o ponto de entrada de função

As propriedades `scriptFile` e `entryPoint` do `function.json` podem ser usadas para configurar o local e o nome da função exportada. Essas propriedades podem ser importantes quando seu JavaScript é transferido.

### <a name="using-scriptfile"></a>Usando o `scriptFile`

Por padrão, uma função JavaScript é executada do `index.js`, um arquivo que compartilha o mesmo diretório pai que seu `function.json` correspondente.

`scriptFile` pode ser usado para obter uma estrutura de pastas semelhante ao exemplo a seguir:

```
FunctionApp
 | - host.json
 | - myNodeFunction
 | | - function.json
 | - lib
 | | - sayHello.js
 | - node_modules
 | | - ... packages ...
 | - package.json
```

O `function.json` da `myNodeFunction` deve incluir uma propriedade `scriptFile` que aponte para o arquivo com a função exportada a ser executada.

```json
{
  "scriptFile": "../lib/sayHello.js",
  "bindings": [
    ...
  ]
}
```

### <a name="using-entrypoint"></a>Usando o `entryPoint`

No `scriptFile` (ou `index.js`), uma função precisa ser exportada usando `module.exports` para ser localizada e executada. Por padrão, a função que é executada quando disparada é a única exportação desse arquivo, a exportação denominada `run` ou a exportação denominada `index`.

Isso pode ser configurado usando `entryPoint` em `function.json`, como no exemplo a seguir:

```json
{
  "entryPoint": "logFoo",
  "bindings": [
    ...
  ]
}
```

No Functions v2.x, que suporta o parâmetro `this` nas funções do usuário, o código da função poderia ser como no seguinte exemplo:

```javascript
class MyObj {
    constructor() {
        this.foo = 1;
    };

    logFoo(context) { 
        context.log("Foo is " + this.foo); 
        context.done(); 
    }
}

const myObj = new MyObj();
module.exports = myObj;
```

Neste exemplo, é importante notar que, embora um objeto esteja sendo exportado, não há garantias de preservação do Estado entre as execuções.

## <a name="local-debugging"></a>Depuração local

Quando iniciado `--inspect` com o parâmetro, um processo Node.js ouve um cliente de depuração na porta especificada. Nas funções do Azure 2.x, você pode especificar argumentos para passar para o processo Node.js que executa seu código adicionando a variável ambiente ou configuração do `languageWorkers:node:arguments = <args>`aplicativo . 

Para depurar localmente, adicione `"languageWorkers:node:arguments": "--inspect=5858"` em `Values` seu arquivo [local.settings.json](https://docs.microsoft.com/azure/azure-functions/functions-run-local#local-settings-file) e anexe um depurador na porta 5858.

Ao depurar usando o `--inspect` CÓDIGO VS, o `port` parâmetro é adicionado automaticamente usando o valor no arquivo launch.json do projeto.

Na versão 1.x, a configuração `languageWorkers:node:arguments` não funcionará. A porta de depuração [`--nodeDebugPort`](https://docs.microsoft.com/azure/azure-functions/functions-run-local#start) pode ser selecionada com o parâmetro no Azure Functions Core Tools.

## <a name="typescript"></a>TypeScript

Quando você segmenta a versão 2.x do tempo de execução Functions, tanto [o Azure Functions for Visual Studio Code](functions-create-first-function-vs-code.md) quanto o [Azure Functions Core Tools](functions-run-local.md) permitem criar aplicativos de função usando um modelo que suporta projetos de aplicativos de função TypeScript. O modelo `package.json` `tsconfig.json` gera e projeta arquivos que facilitam a transcompilação, execução e publicação de funções JavaScript a partir do código TypeScript com essas ferramentas.

Um `.funcignore` arquivo gerado é usado para indicar quais arquivos são excluídos quando um projeto é publicado no Azure.  

Os arquivos TypeScript (.ts) são transcompilados em arquivos `dist` JavaScript (.js) no diretório de saída. Os modelos TypeScript usam `function.json` o [ `scriptFile` parâmetro](#using-scriptfile) para indicar a localização `dist` do arquivo .js correspondente na pasta. O local de saída é `outDir` definido pelo `tsconfig.json` modelo usando parâmetro no arquivo. Se você alterar esta configuração ou o nome da pasta, o tempo de execução não será capaz de encontrar o código para executar.

> [!NOTE]
> O suporte experimental para TypeScript existe a versão 1.x do tempo de execução Functions. A versão experimental transpile arquivos TypeScript em arquivos JavaScript quando a função é invocada. Na versão 2.x, este suporte experimental foi substituído pelo método orientado por ferramentas que faz a transpilação antes da inicialização do host e durante o processo de implantação.

A maneira como você desenvolve e implanta localmente a partir de um projeto TypeScript depende de sua ferramenta de desenvolvimento.

### <a name="visual-studio-code"></a>Visual Studio Code

A [extensão Azure Functions for Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions) permite que você desenvolva suas funções usando o TypeScript. O Core Tools é um requisito da extensão Funções do Azure.

Para criar um aplicativo de função TypeScript no Visual Studio Code, escolha `TypeScript` como seu idioma ao criar um aplicativo de função.

Quando você pressiona **F5** para executar o aplicativo localmente, a transpilação é feita antes que o host (func.exe) seja inicializado. 

Quando você implanta seu aplicativo de função no Azure usando o **aplicativo Implantar para funcionar...** a extensão Funções do Azure gera primeiro uma compilação pronta para produção de arquivos JavaScript a partir dos arquivos de origem TypeScript.

### <a name="azure-functions-core-tools"></a>Azure Functions Core Tools

Existem várias maneiras pelas quais um projeto TypeScript difere de um projeto JavaScript ao usar as Ferramentas Principais.

#### <a name="create-project"></a>Criar projeto

Para criar um projeto de aplicativo de função TypeScript usando o Core Tools, você deve especificar a opção de idioma TypeScript ao criar seu aplicativo de função. Você pode fazer isso de uma das seguintes maneiras:

- Execute `func init` o comando, selecione `node` como sua `typescript`pilha de idiomas e, em seguida, selecione .

- Execute o comando `func init --worker-runtime typescript`.

#### <a name="run-local"></a>Executar local

Para executar o código do aplicativo de função localmente `func host start`usando as Ferramentas Principais, use os seguintes comandos em vez de: 

```command
npm install
npm start
```

O `npm start` comando é equivalente aos seguintes comandos:

- `npm run build`
- `func extensions install`
- `tsc`
- `func start`

#### <a name="publish-to-azure"></a>Publicar no Azure

Antes de [`func azure functionapp publish`] usar o comando para implantar no Azure, você cria uma compilação pronta para produção de arquivos JavaScript a partir dos arquivos de origem TypeScript. 

Os seguintes comandos preparam e publicam seu projeto TypeScript usando ferramentas principais: 

```command
npm run build:production 
func azure functionapp publish <APP_NAME>
```

Neste comando, `<APP_NAME>` substitua-o pelo nome do seu aplicativo de função.

## <a name="considerations-for-javascript-functions"></a>Considerações para funções em JavaScript

Ao trabalhar com funções JavaScript, lembre-se das considerações nas seções a seguir.

### <a name="choose-single-vcpu-app-service-plans"></a>Escolher Planos do Serviço de Aplicativo de vCPU único

Ao criar um aplicativo de funções que usa o Plano do Serviço de Aplicativo, recomendamos que você selecione um plano de vCPU único em vez de um plano com vários vCPUs. Atualmente, o Functions executa funções em JavaScript com mais eficiência em VMs de vCPU único, e o uso de VMs maiores não produz os aprimoramentos de desempenho esperados. Quando necessário, você pode dimensionar manualmente adicionando mais instâncias de VM de um único vCPU, ou você pode habilitar a escala automática. Para saber mais, confira [Dimensionar a contagem de instâncias manual ou automaticamente](../monitoring-and-diagnostics/insights-how-to-scale.md?toc=%2fazure%2fapp-service%2ftoc.json).

### <a name="cold-start"></a>Inicialização a frio

No desenvolvimento de Azure Functions no modelo de hospedagem sem servidor, as inicializações a frio são uma realidade. *Partida a frio* refere-se ao fato de que, quando seu aplicativo de função é iniciado pela primeira vez após um período de inatividade, leva mais tempo para inicializar. Para funções JavaScript com árvores de dependência grandes em particular, o cold start pode ser significativo. Para acelerar o processo de inicialização a frio, [execute suas funções como um arquivo de pacote](run-functions-from-deployment-package.md) quando possível. Muitos métodos de implantação usam a execução do modelo de pacote por padrão, mas se você estiver experimentando grandes reinicializações a frio e não estiver sendo executado dessa maneira, essa alteração poderá oferecer uma melhoria significativa.

### <a name="connection-limits"></a>Limites de conexão

Quando você usa um cliente específico de serviço em um aplicativo de funções do Azure, não crie um novo cliente com cada invocação de função. Em vez disso, crie um único cliente estático no escopo global. Para obter mais informações, consulte [o gerenciamento de conexões em Funções Azure](manage-connections.md).

### <a name="use-async-and-await"></a>Usar `async` e`await`

Ao escrever Funções do Azure em JavaScript, você deve escrever código usando as `async` palavras-chave. `await` Escrever código `async` `await` usando e em `.then` vez `.catch` de retornos de chamadas ou e com Promessas ajuda a evitar dois problemas comuns:
 - Lançar exceções não capturadas que [travam o processo Node.js,](https://nodejs.org/api/process.html#process_warning_using_uncaughtexception_correctly)potencialmente afetando a execução de outras funções.
 - Comportamento inesperado, como a falta de registros do context.log, causada por chamadas assíncronas que não são devidamente aguardadas.

No exemplo abaixo, o método `fs.readFile` assíncrono é invocado com uma função de retorno de chamada de erro-primeiro como seu segundo parâmetro. Este código causa ambos os problemas mencionados acima. Uma exceção que não está explicitamente presa no escopo correto derrubou todo o processo (emitir #1). Ligar `context.done()` fora do escopo da função de retorno de chamada significa que a invocação da função pode terminar antes que o arquivo seja lido (emita #2). Neste exemplo, `context.done()` chamar resultados muito cedo em entradas de log ausentes começando com `Data from file:`.

```javascript
// NOT RECOMMENDED PATTERN
const fs = require('fs');

module.exports = function (context) {
    fs.readFile('./hello.txt', (err, data) => {
        if (err) {
            context.log.error('ERROR', err);
            // BUG #1: This will result in an uncaught exception that crashes the entire process
            throw err;
        }
        context.log(`Data from file: ${data}`);
        // context.done() should be called here
    });
    // BUG #2: Data is not guaranteed to be read before the Azure Function's invocation ends
    context.done();
}
```

O `async` uso `await` de palavras-chave ajuda a evitar esses dois erros. Você deve usar a função [`util.promisify`](https://nodejs.org/api/util.html#util_util_promisify_original) de utilidade Node.js para transformar funções no estilo de retorno de chamada em funções aguardadas.

No exemplo abaixo, quaisquer exceções não tratadas lançadas durante a execução da função apenas falham na invocação individual que levantou uma exceção. A `await` palavra-chave significa `readFileAsync` que `readFile` as etapas seguintes somente após a execução são concluídas. Com `async` `await`e , você também não `context.done()` precisa chamar a chamada de volta.

```javascript
// Recommended pattern
const fs = require('fs');
const util = require('util');
const readFileAsync = util.promisify(fs.readFile);

module.exports = async function (context) {
    let data;
    try {
        data = await readFileAsync('./hello.txt');
    } catch (err) {
        context.log.error('ERROR', err);
        // This rethrown exception will be handled by the Functions Runtime and will only fail the individual invocation
        throw err;
    }
    context.log(`Data from file: ${data}`);
}
```

## <a name="next-steps"></a>Próximas etapas

Para saber mais, consulte os recursos a seguir:

+ [Práticas recomendadas do Azure Functions](functions-best-practices.md)
+ [Referência do desenvolvedor do Azure Functions](functions-reference.md)
+ [Azure Funções gatilhos e vinculações](functions-triggers-bindings.md)

['func azure functionapp publicar']: functions-run-local.md#project-file-deployment

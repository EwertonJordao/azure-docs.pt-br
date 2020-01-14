---
title: 'Início Rápido: Biblioteca de clientes da Pesquisa Personalizada do Bing para Java'
description: Introdução à biblioteca de clientes da Pesquisa Personalizada do Bing para Java solicitando os resultados da pesquisa de sua instância da Pesquisa Personalizada do Bing neste início rápido.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: ''
ms.topic: quickstart
ms.date: 12/09/2019
ms.author: aahi
ms.openlocfilehash: 1060cceb9241d9c1c490f7e7f12490c734b0a78d
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75384546"
---
# <a name="quickstart-bing-custom-search-client-library-for-java"></a>Início Rápido: Biblioteca de clientes da Pesquisa Personalizada do Bing para Java

Introdução à biblioteca de clientes da Pesquisa Personalizada do Bing para Java. Siga essas etapas para instalar o pacote e testar o código de exemplo para tarefas básicas. A API de Pesquisa Personalizada do Bing permite que você crie experiências de pesquisa personalizada sem anúncios para os tópicos de seu interesse.

Use a biblioteca de clientes da Pesquisa Personalizada do Bing para Java:

* Localizar os resultados da pesquisa na Web, na sua instância da Pesquisa Personalizada do Bing. 

[Documentação de referência](https://docs.microsoft.com/java/api/overview/azure/cognitiveservices/client/bingcustomsearch?view=azure-java-stable) | [Código-fonte da biblioteca](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/cognitiveservices/Search.BingCustomSearch) | [Artefato (Maven)](https://search.maven.org/artifact/com.microsoft.azure.cognitiveservices/azure-cognitiveservices-customsearch/) | [Exemplos](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples)

## <a name="prerequisites"></a>Prerequisites

* Assinatura do Azure – [crie uma gratuitamente](https://azure.microsoft.com/free/).
* A versão atual do [JDK (Java Development Kit)](https://www.oracle.com/technetwork/java/javase/downloads/index.html).
* A [ferramenta de build Gradle](https://gradle.org/install/) ou outro gerenciador de dependência.
* Uma instância da Pesquisa Personalizada do Bing. Confira [Início Rápido: Criar sua primeira instância da Pesquisa Personalizada do Bing](quick-start.md) para obter mais informações.

## <a name="setting-up"></a>Configurando

### <a name="create-a-bing-custom-search-azure-resource"></a>Criar um recurso do Azure da Pesquisa Personalizada do Bing

Os Serviços Cognitivos do Azure são representados por recursos do Azure que você assina. Crie um recurso da Pesquisa Personalizada do Bing usando o [portal do Azure](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) ou a [CLI do Azure](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account-cli) em seu computador local. Também é possível:

* Obter uma [chave de avaliação](https://azure.microsoft.com/try/cognitive-services/#decision) válida por 7 dias gratuitamente. Depois de se inscrever, ela estará disponível no [site do Azure](https://azure.microsoft.com/try/cognitive-services/my-apis/).  
* Exibir esse recurso no [portal do Azure](https://portal.azure.com/).

Depois de obter uma chave do recurso ou da assinatura de avaliação, [crie uma variável de ambiente](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) para a chave, chamada `AZURE_BING_CUSTOM_SEARCH_API_KEY`.

### <a name="create-a-new-gradle-project"></a>Criar um novo projeto Gradle

> [!TIP]
> Se você não estiver usando o Gradle, poderá encontrar os detalhes da biblioteca de clientes e para outros gerenciadores de dependência no [Repositório Central do Maven](https://search.maven.org/artifact/com.microsoft.azure.cognitiveservices/azure-cognitiveservices-textanalytics/).

Em uma janela de console (como cmd, PowerShell ou Bash), crie um novo diretório para seu aplicativo e navegue até ele. 

```console
mkdir myapp && cd myapp
```

Execute o comando `gradle init` em seu diretório de trabalho. Esse comando cria arquivos de build essenciais para o Gradle, incluindo um arquivo *build.gradle.kts*, que é usado no runtime para configurar seu aplicativo.

```console
gradle init --type basic
```

Quando solicitado a escolher uma **DSL**, escolha **Kotlin**.

## <a name="install-the-client-library"></a>Instalar a biblioteca de clientes 

Localize o *build.gradle.kts* e abra-o com seu IDE ou editor de texto preferencial. Depois copie nessa configuração de build. Inclua a biblioteca de clientes em `dependencies`:

```kotlin
plugins {
    java
    application
}
application {
    mainClassName = "main.java.BingCustomSearchSample"
}
repositories {
    mavenCentral()
}
dependencies {
    compile("org.slf4j:slf4j-simple:1.7.25")
    compile("com.microsoft.azure.cognitiveservices:azure-cognitiveservices-customsearch:1.0.2")
}
```

Crie uma pasta para seu aplicativo de exemplo. Do diretório de trabalho, execute o seguinte comando:

```console
mkdir src/main/java
```

Navegue até a nova pasta e crie um arquivo chamado *BingCustomSearchSample.java*. Abra-o e adicione as seguintes instruções `import`:


[!code-java[import statements](~/cognitive-services-java-sdk-samples/Search/BingCustomSearch/src/main/java/BingCustomSearchSample.java?name=imports)]

Crie uma classe denominada `BingCustomSearchSample`

```java
public class BingCustomSearchSample {
}
```

Na classe, crie um método `main` e uma variável para a chave do recurso. Se você tiver criado a variável de ambiente depois de iniciar o aplicativo, feche e reabra o editor, o IDE ou o shell em execução para acessar a variável. Você definirá os métodos mais tarde.

[!code-java[main method](~/cognitive-services-java-sdk-samples/Search/BingCustomSearch/src/main/java/BingCustomSearchSample.java?name=main)]

## <a name="object-model"></a>Modelo de objeto

O cliente da Pesquisa Personalizada do Bing é um objeto [BingCustomSearchAPI](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.search.customsearch.bingcustomsearchapi?view=azure-java-stable) criado no método [authenticate()](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.search.customsearch.bingcustomsearchmanager.authenticate?view=azure-java-stable#com_microsoft_azure_cognitiveservices_search_customsearch_BingCustomSearchManager_authenticate_String_) do objeto [BingCustomSearchManager](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.search.customsearch.bingcustomsearchmanager?view=azure-java-stable). Você pode enviar uma solicitação de pesquisa usando o método [BingCustomInstances.search()](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.search.customsearch.bingcustominstances.search?view=azure-java-stable#com_microsoft_azure_cognitiveservices_search_customsearch_BingCustomInstances_search__) do cliente.

A resposta da API é um objeto [SearchResponse](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.search.customsearch.models.searchresponse?view=azure-java-stable) que contém informações sobre a consulta de pesquisa e os resultados da pesquisa.

## <a name="code-examples"></a>Exemplos de código

Esses snippets de códigos mostram como realizar as seguintes tarefas com a biblioteca de clientes da Pesquisa Personalizada do Bing para Java:

* [Autenticar o cliente](#authenticate-the-client)
* [Obter resultados da pesquisa na instância de pesquisa personalizada](#get-search-results-from-your-custom-search-instance)

## <a name="authenticate-the-client"></a>Autenticar o cliente

O método principal deve incluir um objeto [BingCustomSearchManager](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.search.customsearch.bingcustomsearchapi?view=azure-java-stable) que usa a chave e chama seu `authenticate()`.

```java
BingCustomSearchAPI client = BingCustomSearchManager.authenticate(subscriptionKey);
```

## <a name="get-search-results-from-your-custom-search-instance"></a>Obter resultados da pesquisa na instância de pesquisa personalizada

Use a função [BingCustomInstances.search()](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.search.customsearch.bingcustominstances.search?view=azure-java-stable#com_microsoft_azure_cognitiveservices_search_customsearch_BingCustomInstances_search__) do cliente para enviar uma consulta de pesquisa para sua instância personalizada. Defina o `withCustomConfig` para sua ID de configuração personalizada ou por padrão como `1`. Depois de obter uma resposta da API, verifique se foram encontrados resultados da pesquisa. Nesse caso, obtenha o primeiro resultado da pesquisa chamando a função `webPages().value().get()` da resposta e imprima o nome do resultado e a URL. 

[!code-java[call the custom search API](~/cognitive-services-java-sdk-samples/Search/BingCustomSearch/src/main/java/BingCustomSearchSample.java?name=runSample)]

## <a name="run-the-application"></a>Executar o aplicativo

Compile o aplicativo com o seguinte comando no diretório principal do projeto:

```console
gradle build
```

Execute o aplicativo com a meta `run`:

```console
gradle run
```

## <a name="clean-up-resources"></a>Limpar os recursos

Se quiser limpar e remover uma assinatura dos Serviços Cognitivos, você poderá excluir o recurso ou grupo de recursos. Excluir o grupo de recursos também exclui todos os recursos associados a ele.

* [Portal](../cognitive-services-apis-create-account.md#clean-up-resources)
* [CLI do Azure](../cognitive-services-apis-create-account-cli.md#clean-up-resources)

## <a name="next-steps"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Criar um aplicativo Web de Pesquisa Personalizada](./tutorials/custom-search-web-page.md)

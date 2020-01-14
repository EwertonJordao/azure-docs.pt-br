---
title: 'Início Rápido: Biblioteca de clientes do Personalizador para .NET'
titleSuffix: Azure Cognitive Services
description: Este guia de início rápido mostra como começar a trabalhar com a biblioteca de clientes do Personalizador para .NET usando um loop de aprendizado.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: quickstart
ms.date: 10/24/2019
ms.author: diberry
ms.openlocfilehash: c17bf54d89e3a98ca667eeba40f2d2b166550833
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75446383"
---
# <a name="quickstart-personalizer-client-library-for-net"></a>Início Rápido: Biblioteca de clientes do Personalizador para .NET

Exiba o conteúdo personalizado neste início rápido do C# com o serviço Personalizador.

Introdução à biblioteca de clientes do Personalizador para .NET. Siga essas etapas para instalar o pacote e testar o código de exemplo para tarefas básicas.

 * Classificar uma lista de ações para personalização.
 * Pontuação de recompensa do relatório indicando êxito da ação de classificação mais alta.

[Documentação de referência](https://docs.microsoft.com/dotnet/api/Microsoft.Azure.CognitiveServices.Personalizer?view=azure-dotnet-preview) | [Código-fonte da biblioteca](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/cognitiveservices/Personalizer) | [Pacote (NuGet)](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Personalizer/) | [Exemplos](https://github.com/Azure-Samples/cognitive-services-personalizer-samples)

## <a name="prerequisites"></a>Prerequisites

* Assinatura do Azure – [Criar uma gratuitamente](https://azure.microsoft.com/free/)
* A versão atual do [.NET Core](https://dotnet.microsoft.com/download/dotnet-core).

## <a name="using-this-quickstart"></a>Como usar este início rápido

Há várias etapas para usar este início rápido:

* No portal do Azure, crie um recurso do Personalizador
* No portal do Azure, para o recurso do Personalizador, na página **Configuração**, altere a frequência de atualização do modelo
* Em um editor de códigos, crie um arquivo de código e edite-o
* Na linha de comando ou no terminal, instale o SDK usando a linha de comando
* Na linha de comando ou no terminal, execute o arquivo de código

## <a name="create-a-personalizer-azure-resource"></a>Criar um recurso do Azure do Personalizador

Crie um recurso para o Personalizador usando o [portal do Azure](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) ou a [CLI do Azure](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account-cli) em seu computador local. Também é possível:

* Obter uma [chave de avaliação](https://azure.microsoft.com/try/cognitive-services) válida por 7 dias gratuitamente. Depois de se inscrever, ela estará disponível no [site do Azure](https://azure.microsoft.com/try/cognitive-services/my-apis/).  
* Exibir esse recurso no [portal do Azure](https://portal.azure.com/).

Depois de obter uma chave do recurso ou da assinatura de avaliação, crie duas [variáveis de ambiente](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication):

* `PERSONALIZER_RESOURCE_KEY` para a chave do recurso.
* `PERSONALIZER_RESOURCE_ENDPOINT` para o ponto de extremidade do recurso.

No portal do Azure, os valores da chave e do ponto de extremidade estão disponíveis na página **início rápido**.

## <a name="change-the-model-update-frequency"></a>Alterar a frequência de atualização do modelo

No portal do Azure, no recurso do Personalizador na página **Configuração**, altere a **Frequência de atualização de modelo** para 10 segundos. Essa duração curta treinará o serviço rapidamente, permitindo que você veja como a ação superior é alterada a cada iteração.

![Alterar a frequência de atualização do modelo](./media/settings/configure-model-update-frequency-settings.png)

Quando o loop do Personalizador é instanciado pela primeira vez, não há modelo, pois não houve nenhuma chamada à API de Recompensa com base na qual treinar. Chamadas de classificação retornarão probabilidades iguais para cada item. Seu aplicativo ainda deve classificar sempre o conteúdo usando a saída de RewardActionId.

## <a name="create-a-new-c-application"></a>Criar um aplicativo em C#

Crie um novo aplicativo .NET Core em seu IDE ou editor preferido. 

Em uma janela de console (como cmd, PowerShell ou Bash), use o comando `new` para criar um novo aplicativo do console com o nome `personalizer-quickstart`. Esse comando cria um projeto simples em C# do tipo "Olá, Mundo" com um arquivo de origem único: `Program.cs`. 

```console
dotnet new console -n personalizer-quickstart
```

Altere o diretório para a pasta do aplicativo recém-criado. É possível criar o aplicativo com:

```console
dotnet build
```

A saída de compilação não deve conter nenhum aviso ou erro. 

```console
...
Build succeeded.
 0 Warning(s)
 0 Error(s)
...
```

## <a name="install-the-sdk"></a>Instalar o SDK

Dentro do diretório do aplicativo, instale a biblioteca de clientes do Personalizador para .NET com o seguinte comando:

```console
dotnet add package Microsoft.Azure.CognitiveServices.Personalizer --version 0.8.0-preview
```

Se você estiver usando o IDE do Visual Studio, a biblioteca de clientes estará disponível como um pacote baixável do NuGet.

## <a name="object-model"></a>Modelo de objeto

O cliente do Personalizador é um objeto [PersonalizerClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.personalizer.personalizerclient?view=azure-dotnet) que se autentica no Azure usando Microsoft.Rest.ServiceClientCredentials, que contém sua chave.

Para solicitar uma classificação do conteúdo, crie um [RankRequest](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.personalizer.models.rankrequest?view=azure-dotnet-preview) e, em seguida, passe-o para o método [client.Rank](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.personalizer.personalizerclientextensions.rank?view=azure-dotnet-preview). O método Rank retorna um RankResponse, que contém o conteúdo classificado. 

Para enviar uma recompensa ao Personalizador, crie um [RewardRequest](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.personalizer.models.rewardrequest?view=azure-dotnet-preview) e, em seguida, passe-o para método [client.Reward](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.personalizer.personalizerclientextensions.reward?view=azure-dotnet-preview). 

Determinar a recompensa neste início rápido é trivial. Em um sistema de produção, a determinação do que afeta a [pontuação de recompensa](concept-rewards.md) e em que medida pode ser um processo complexo; você pode decidir alterá-lo ao longo do tempo. Essa decisão de design deve ser uma das principais decisões na arquitetura do Personalizador. 

## <a name="code-examples"></a>Exemplos de código

Esses snippets de código mostram como realizar as seguintes tarefas com a biblioteca de clientes do Personalizador para .NET:

* [Criar um cliente do Personalizador](#create-a-personalizer-client)
* [Solicitar uma classificação](#request-a-rank)
* [Enviar uma recompensa](#send-a-reward)

## <a name="add-the-dependencies"></a>Adicionar as dependências

No diretório do projeto, abra o arquivo **Program.cs** no IDE ou no editor de sua preferência. Substitua o código `using` existente pelas seguintes diretivas de `using`:

[!code-csharp[Using statements](~/samples-personalizer/quickstarts/csharp/PersonalizerExample/Program.cs?name=Dependencies)]

## <a name="add-personalizer-resource-information"></a>Adicionar informações de recursos do Personalizador

Na classe **Program**, crie variáveis para a chave e o ponto de extremidade do Azure extraídos das variáveis de ambiente, denominadas `PERSONALIZER_RESOURCE_KEY` e `PERSONALIZER_RESOURCE_ENDPOINT`. Se você tiver criado as variáveis de ambiente depois de iniciar o aplicativo, será necessário fechar e recarregar o editor, o IDE ou o shell em execução para acessar a variável. Os métodos serão criados posteriormente neste início rápido.

[!code-csharp[Create variables to hold the Personalizer resource key and endpoint values found in the Azure portal.](~/samples-personalizer/quickstarts/csharp/PersonalizerExample/Program.cs?name=classVariables)]

## <a name="create-a-personalizer-client"></a>Criar um cliente do Personalizador

Em seguida, crie um método para retornar um cliente do Personalizador. O parâmetro para o método é o `PERSONALIZER_RESOURCE_ENDPOINT` e a ApiKey é o `PERSONALIZER_RESOURCE_KEY`.

[!code-csharp[Create the Personalizer client](~/samples-personalizer/quickstarts/csharp/PersonalizerExample/Program.cs?name=authorization)]

## <a name="get-food-items-as-rankable-actions"></a>Obter itens de alimentos como ações classificáveis

As ações representam as opções de conteúdo que você deseja que o Personalizador classifique. Adicione os métodos a seguir à classe Program para representar o conjunto de ações a serem classificadas.

[!code-csharp[Food items as actions](~/samples-personalizer/quickstarts/csharp/PersonalizerExample/Program.cs?name=createAction)]

## <a name="get-user-preferences-for-context"></a>Obter preferências do usuário para o contexto

Adicione os seguintes métodos à classe Program para obter a entrada de um usuário da linha de comando para a hora do dia e a preferência de alimentos atual. Elas serão usadas como contexto ao classificar as ações.

[!code-csharp[Present time out day preference to the user](~/samples-personalizer/quickstarts/csharp/PersonalizerExample/Program.cs?name=createUserFeatureTimeOfDay)]

[!code-csharp[Present food taste preference to the user](~/samples-personalizer/quickstarts/csharp/PersonalizerExample/Program.cs?name=createUserFeatureTastePreference)]

Os dois métodos usam o método `GetKey` para ler a seleção do usuário na linha de comando. 

[!code-csharp[Read user's choice from the command line](~/samples-personalizer/quickstarts/csharp/PersonalizerExample/Program.cs?name=readCommandLine)]

## <a name="create-the-learning-loop"></a>Criar o loop de aprendizado

O loop de aprendizado do Personalizador é um ciclo de chamadas de classificação e de recompensa. Neste início rápido, cada chamada de classificação, para personalizar o conteúdo, é seguida por uma chamada de recompensa para informar ao Personalizador quão bem o serviço classificou o conteúdo. 

O código a seguir no método `main` do programa executa um loop em um ciclo de perguntar as preferências do usuário na linha de comando, enviar essas informações ao Personalizador para classificar, apresentar a seleção classificada ao cliente para escolher entre as que estão na lista e, em seguida, enviar uma recompensa para Personalizador, sinalizando quão bem o serviço classificou a seleção.

[!code-csharp[Learning loop](~/samples-personalizer/quickstarts/csharp/PersonalizerExample/Program.cs?name=mainLoop)]

Adicione os seguintes métodos, que [obtêm as opções de conteúdo](#get-food-items-as-rankable-actions), antes de executar o arquivo de código:

* `GetActions`
* `GetUsersTimeOfDay`
* `GetUsersTastePreference`
* `GetKey`

## <a name="request-a-rank"></a>Solicitar uma classificação

Para concluir a solicitação de classificação, o programa pergunta as preferências do usuário para criar um `currentContent` das opções de conteúdo. O processo pode criar conteúdo a ser excluído da classificação, mostrado como `excludeActions`. A solicitação de classificação precisa das ações, currentContext, excludeActions e uma ID exclusiva de evento de classificação (como um GUID), para receber a resposta classificada. 

Este início rápido tem recursos de contexto simples da hora do dia e da preferência de alimentos do usuário. Em sistemas de produção, determinar e [avaliar](concept-feature-evaluation.md) [ações e recursos](concepts-features.md) pode ser uma questão não trivial.  

[!code-csharp[The Personalizer learning loop ranks the request.](~/samples-personalizer/quickstarts/csharp/PersonalizerExample/Program.cs?name=rank)]

## <a name="send-a-reward"></a>Enviar uma recompensa

Para concluir a solicitação de recompensa, o programa obtém a seleção do usuário da linha de comando, atribui um valor numérico a cada seleção e, em seguida, envia uma ID exclusiva de evento de classificação e o valor numérico para o método de recompensa.

Este início rápido atribui um número simples como uma recompensa, um zero ou um 1. Em sistemas de produção, determinar quando e o que enviar para a chamada de [recompensa](concept-rewards.md) pode ser uma questão não trivial, dependendo de suas necessidades específicas. 

[!code-csharp[The Personalizer learning loop ranks the request.](~/samples-personalizer/quickstarts/csharp/PersonalizerExample/Program.cs?name=reward)]

## <a name="run-the-program"></a>Execute o programa

Execute o aplicativo em seu próprio diretório com o comando `run` de dotnet.

```console
dotnet run
```

![O programa de Início Rápido faz algumas perguntas para coletar as preferências do usuário, conhecidas como recursos e, em seguida, fornece a ação principal.](media/csharp-quickstart-commandline-feedback-loop/quickstart-program-feedback-loop-example.png)

O [código-fonte deste início rápido](https://github.com/Azure-Samples/cognitive-services-personalizer-samples/blob/master/quickstarts/csharp/PersonalizerExample/Program.cs) está disponível no repositório GitHub de exemplos do Personalizador.

## <a name="clean-up-resources"></a>Limpar os recursos

Se quiser limpar e remover uma assinatura dos Serviços Cognitivos, você poderá excluir o recurso ou grupo de recursos. Excluir o grupo de recursos também exclui todos os recursos associados a ele.

* [Portal](../cognitive-services-apis-create-account.md#clean-up-resources)
* [CLI do Azure](../cognitive-services-apis-create-account-cli.md#clean-up-resources)

## <a name="next-steps"></a>Próximas etapas

> [!div class="nextstepaction"]
>[Como funciona o Personalizador](how-personalizer-works.md)

* [O que é Personalizador?](what-is-personalizer.md)
* [Onde você pode usar o Personalizador?](where-can-you-use-personalizer.md)
* [Solução de problemas](troubleshooting.md)

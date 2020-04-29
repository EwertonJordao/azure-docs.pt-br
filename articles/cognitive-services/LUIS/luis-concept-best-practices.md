---
title: Práticas recomendadas para criar seu aplicativo LUIS
description: Conheça as práticas recomendadas para obter os melhores resultados do modelo do aplicativo LUIS.
ms.topic: conceptual
ms.date: 04/14/2020
ms.author: diberry
ms.openlocfilehash: 525d450084723a53ae090319d9ebf3f68d63beee
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/29/2020
ms.locfileid: "81382383"
---
# <a name="best-practices-for-building-a-language-understanding-luis-app"></a>Práticas recomendadas para a criação de um aplicativo de reconhecimento de linguagem (LUIS)
Use o processo de criação de aplicativos para criar seu aplicativo LUIS:

* Criar modelos de linguagem (intenções e entidades)
* Adicionar alguns exemplos de treinamento declarações (15-30 por tentativa)
* Publicar no ponto de extremidade
* Testar do ponto de extremidade

Depois que seu aplicativo for [publicado](luis-how-to-publish-app.md), use o ciclo de vida de desenvolvimento para adicionar recursos, publicar e testar do ponto de extremidade. Não comece o próximo ciclo de criação adicionando mais declarações de exemplo porque isso não permite que o LUIS Aprenda seu modelo com declarações de usuário do mundo real.

Não expanda o declarações até que o conjunto atual de declarações de exemplo e de ponto de extremidade esteja retornando pontuações de previsão alta de confiança. Melhore as pontuações usando o [aprendizado ativo](luis-concept-review-endpoint-utterances.md).




## <a name="do-and-dont"></a>O que fazer e o que não fazer
A lista a seguir inclui melhores práticas para aplicativos LUIS:

|O que fazer|O que não fazer|
|--|--|
|[Definir intenções distintas](#do-define-distinct-intents)<br>[Adicionar descritores a tentativas](#do-add-descriptors-to-intents) |[Adicionar muitos exemplos de declaração a intenções](#dont-add-many-example-utterances-to-intents)<br>[Usar algumas ou entidades simples](#dont-use-few-or-simple-entities) |
|[Localizar um ponto ideal entre muito genérico e muito específico para cada intenção](#do-find-sweet-spot-for-intents)|[Usar o LUIS como uma plataforma de treinamento](#dont-use-luis-as-a-training-platform)|
|[Crie seu aplicativo iterativamente com versões](#do-build-your-app-iteratively-with-versions)<br>[Criar entidades para a decomposição do modelo](#do-build-for-model-decomposition)|[Adicionar muitas declarações de exemplo do mesmo formato, ignorando os outros formatos](#dont-add-many-example-utterances-of-the-same-format-ignoring-other-formats)|
|[Adicionar padrões em iterações posteriores](#do-add-patterns-in-later-iterations)|[Misturar a definição de intenções e entidades](#dont-mix-the-definition-of-intents-and-entities)|
|[Equilibrar suas declarações em todas as intenções](#balance-your-utterances-across-all-intents) exceto a intenção Nenhum.<br>[Adicionar exemplos de enunciado à intenção None](#do-add-example-utterances-to-none-intent)|[Criar descritores com todos os valores possíveis](#dont-create-descriptors-with-all-the-possible-values)|
|[Aproveitar o recurso de sugestão para aprendizado ativo](#do-leverage-the-suggest-feature-for-active-learning)|[Adicionar muitos padrões](#dont-add-many-patterns)|
|[Monitorar o desempenho do seu aplicativo com testes em lotes](#do-monitor-the-performance-of-your-app)|[Treinar e publicar com cada declaração de exemplo única adicionada](#dont-train-and-publish-with-every-single-example-utterance)|

## <a name="do-define-distinct-intents"></a>Defina intenções distintas
Verifique se o vocabulário para cada intenção é apenas para essa intenção e não está se sobrepondo a uma intenção diferente. Por exemplo, se desejar ter um aplicativo que manipula organizações de viagem como voos por companhia aérea e hotéis, será possível optar por tê-las como áreas de assunto, intenções separadas ou a mesma intenção com entidades para dados específicos dentro da declaração.

Se o vocabulário entre duas intenções for o mesmo, combine a intenção e use entidades.

Considere os seguintes exemplos de declaração:

|Exemplo de enunciados|
|--|
|Reservar um voo|
|Reservar um hotel|

`Book a flight`e `Book a hotel` use o mesmo vocabulário de `book a `. Esse formato é o mesmo, portanto, deve ser a mesma intenção com as diferentes palavras `flight` de `hotel` e como entidades extraídas.

## <a name="do-add-descriptors-to-intents"></a>Adicionar descritores a tentativas

Os descritores ajudam a descrever os recursos para uma intenção. Um descritor pode ser uma lista de frases de palavras que são significativas para essa intenção ou uma entidade que é significativa para essa intenção.

## <a name="do-find-sweet-spot-for-intents"></a>Localizar ponto ideal para intenções
Use dados de previsão do LUIS para determinar se suas intenções estiverem se sobrepondo. Intenções sobrepostas confundem o LUIS. O resultado é que a principal intenção de pontuação está muito perto de outra intenção. Como o LUIS não usa o mesmo caminho exato por meio dos dados para treinamento a cada vez, uma intenção sobreposta tem uma chance de ser a primeira ou a segunda no treinamento. Convém que a pontuação da declaração para cada intenção esteja mais distante para que esse flip/flop não aconteça. A boa distinção para intenções deve resultar na principal intenção esperada toda vez.

<a name="#do-build-the-app-iteratively"></a>

## <a name="do-build-your-app-iteratively-with-versions"></a>Compilar seu aplicativo iterativamente com versões

Cada ciclo de criação deve estar dentro de uma nova [versão](luis-concept-version.md), clonada de uma versão existente.

## <a name="do-build-for-model-decomposition"></a>Fazer compilação para a decomposição do modelo

A decomposição do modelo tem um processo típico de:

* criar **intenção** com base nas intenções do usuário do aplicativo cliente
* Adicione o exemplo de 15-30 declarações com base na entrada do usuário do mundo real
* rotular o conceito de dados de nível superior no exemplo expressão
* dividir o conceito de dados em subcomponentes
* Adicionar descritores (recursos) a subcomponentes
* Adicionar descritores (recursos) à intenção

Depois de criar a intenção e adicionar o exemplo declarações, o exemplo a seguir descreve a decomposição da entidade.

Comece identificando os conceitos de dados completos que você deseja extrair em um expressão. Esta é sua entidade aprendida por computador. Em seguida, decompor a frase em suas partes. Isso inclui a identificação de subcomponentes (como entidades), juntamente com os descritores e restrições.

Por exemplo, se você quiser extrair um endereço, a entidade aprendida por máquina superior poderá ser chamada `Address`. Ao criar o endereço, identifique alguns de seus subcomponentes, como endereço, cidade, estado e CEP.

Continue decompondo esses elementos **restringindo** o CEP a uma expressão regular. Decompor o endereço em partes de um número de rua (usando um número predefinido), um nome de rua e um tipo de rua. O tipo de rua pode ser descrito com uma lista de **descritores** como Avenida, círculo, estrada e pista.

A API de criação v3 permite a decomposição do modelo.

## <a name="do-add-patterns-in-later-iterations"></a>Adicionar padrões em iterações posteriores

Você deve entender como o aplicativo se comporta antes de adicionar [padrões](luis-concept-patterns.md) porque os padrões são ponderados de forma muito maior do que o exemplo declarações e distorcerá a confiança.

Depois de entender como seu aplicativo se comporta, adicione padrões conforme eles se aplicam ao seu aplicativo. Você não precisa adicioná-los a cada [iteração](luis-concept-app-iteration.md).

Não há danos para adicioná-los no início do design do modelo, mas é mais fácil ver como cada padrão altera o modelo depois que o modelo é testado com declarações.

<a name="balance-your-utterances-across-all-intents"></a>

## <a name="do-balance-your-utterances-across-all-intents"></a>Equilibre seu declarações em todas as intenções

Para que o LUIS tenha previsões precisas, a quantidade de enunciados de exemplo em cada intenção (exceto para a intenção Nenhum), deve ser relativamente semelhante.

Se você tiver uma intenção com 100 declarações de exemplo e uma intenção com 20 declarações de exemplo, a intenção de expressão 100 terá uma taxa mais alta de previsão.

## <a name="do-add-example-utterances-to-none-intent"></a>Adicionar declarações de exemplo à intenção None

Essa é a intenção de fallback, indicando tudo fora do seu aplicativo. Adicione um exemplo de declaração à intenção None para todos os 10 exemplos de declarações no restante do seu aplicativo LUIS.

## <a name="do-leverage-the-suggest-feature-for-active-learning"></a>Aproveite o recurso de sugestão para aprendizado ativo

Use **Examinar declarações de ponto de extremidade** do [aprendizado ativo](luis-how-to-review-endpoint-utterances.md) regularmente, em vez de adicionar mais exemplos de declarações a intenções. Como o aplicativo está constantemente recebendo declarações de ponto de extremidade, essa lista está crescendo e mudando.

## <a name="do-monitor-the-performance-of-your-app"></a>Monitore o desempenho do seu aplicativo

Monitore a precisão da previsão usando um [conjunto de teste em lote](luis-concept-batch-test.md).

Mantenha um conjunto separado de declarações que não são usados como [exemplo declarações](luis-concept-utterance.md) ou Endpoint declarações. Continue melhorando o aplicativo para seu conjunto de testes. Adapte o conjunto de teste para refletir declarações de usuário real. Use este conjunto de teste para avaliar cada iteração ou versão do aplicativo.

## <a name="dont-add-many-example-utterances-to-intents"></a>Não adicione muitos exemplos de declaração a intenções

Depois que o aplicativo for publicado, adicione apenas declarações do aprendizado ativo no processo de ciclo de vida de desenvolvimento. Se as declarações forem semelhantes demais, adicione um padrão.

## <a name="dont-use-few-or-simple-entities"></a>Não use algumas ou entidades simples

As entidades são criadas para extração e previsão de dados. É importante que cada tentativa tenha entidades aprendidas por máquina que descrevam os dados na intenção. Isso ajuda a LUIS a prever a intenção, mesmo que o aplicativo cliente não precise usar a entidade extraída.

## <a name="dont-use-luis-as-a-training-platform"></a>Não use o LUIS como uma plataforma de treinamento

O LUIS é específico para o domínio de um modelo de linguagem. Ele não é destinado a trabalhar como uma plataforma de treinamento geral de idioma natural.

## <a name="dont-add-many-example-utterances-of-the-same-format-ignoring-other-formats"></a>Não adicionar muitas declarações de exemplo do mesmo formato, ignorando os outros formatos

O LUIS espera variações nas declarações de uma intenção. As declarações podem variar e ter o mesmo significado geral. As variações podem incluir o comprimento da declaração, a escolha de palavras e o posicionamento de palavras.

|Não use o mesmo formato|Use o formato variável|
|--|--|
|Comprar uma passagem para Seattle<br>Comprar uma passagem para Paris<br>Comprar uma passagem para Orlando|Comprar 1 passagem para Seattle<br>Reservar duas poltronas no voo para Paris na próxima segunda-feira<br>Eu gostaria de reservar três passagens para Orlando para a semana do saco cheio|

A segunda coluna usa diferentes verbos (comprar e reservar), diferentes quantidades (1, duas, 3) e diferentes organizações de palavras, mas todas têm a mesma intenção de comprar passagens de avião para viagem.

## <a name="dont-mix-the-definition-of-intents-and-entities"></a>Não misture a definição de intenções e entidades

Criar uma intenção para qualquer ação que seu bot usará. Use entidades como parâmetros que possibilitam a ação.

Para um bot que irá reservar vôos de companhia aérea, crie uma intenção **BookFlight** . Não crie uma intenção para toda companhia aérea ou para todo destino. Use essas partes de dados como [entidades](luis-concept-entity-types.md) e marque-as nos exemplos de declarações.

## <a name="dont-create-descriptors-with-all-the-possible-values"></a>Não crie descritores com todos os valores possíveis

Forneça alguns exemplos nas [listas de frases](luis-concept-feature.md) de descritor, mas não todas as palavras. O LUIS generaliza e leva em conta o contexto.

## <a name="dont-add-many-patterns"></a>Não adicione muitos padrões

Não adicione [padrões](luis-concept-patterns.md) demais. O LUIS destina-se a aprender rapidamente com menos exemplos. Não sobrecarregue o sistema desnecessariamente.

## <a name="dont-train-and-publish-with-every-single-example-utterance"></a>Não treine nem publique com toda declaração de exemplo única

Adicione 10 ou 15 declarações antes do treinamento e da publicação. Isso permite ver o impacto sobre a precisão da previsão. Adicionar uma única declaração pode não ter um impacto visível sobre a pontuação.

## <a name="next-steps"></a>Próximas etapas

* Saiba como [planejar seu aplicativo](luis-how-plan-your-app.md) no aplicativo LUIS.

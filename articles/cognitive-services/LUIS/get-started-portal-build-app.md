---
title: 'Início Rápido: Criar um aplicativo no portal do LUIS'
description: Neste início rápido, você criará as partes básicas de um aplicativo, intenções e entidades, bem como um teste com enunciado de exemplo no portal do LUIS.
ms.topic: quickstart
ms.date: 04/14/2020
ms.openlocfilehash: 2d601646c43c0f0d99dc6934cf1f1c960e0b0f79
ms.sourcegitcommit: ea006cd8e62888271b2601d5ed4ec78fb40e8427
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81382579"
---
# <a name="quickstart-create-a-new-app-in-the-luis-portal"></a>Início Rápido: Criar um aplicativo no portal do LUIS

Neste início rápido, você criará um aplicativo no portal do LUIS. Primeiro, crie as partes básicas de um aplicativo, **intenções** e **entidades**. Em seguida, teste o aplicativo fornecendo um enunciado de usuário de exemplo no painel de teste interativo para obter a intenção prevista.

[!INCLUDE [Sign in to LUIS](./includes/sign-in-process.md)]

## <a name="create-an-app"></a>Criar um aplicativo

1. Selecione **+ Novo aplicativo para conversa** na barra de ferramentas de contexto e, em seguida, selecione **Novo aplicativo para conversa**.

    > [!div class="mx-imgBorder"]
    > [![Criar aplicativo no portal do LUIS](./media/create-app-in-portal.png)](./media/create-app-in-portal.png#lightbox)

1. Na janela pop-up, configure o aplicativo com as configurações a seguir e, em seguida, selecione **Concluído**.

   |Nome da configuração| Valor | Finalidade|
   |--|--|--|
   |Nome|`myEnglishApp`|Nome exclusivo de aplicativo LUIS<br>obrigatório|
   |Cultura|**Inglês**|Linguagem de enunciados de usuários, **pt-br**<br>obrigatório|
   |Descrição (opcional)|`App made with LUIS Portal`|Descrição do aplicativo<br>opcional|
   |Recurso de previsão (opcional) |-  |Não selecione. O LUIS fornece uma chave de início para ser usada gratuitamente com a finalidade de criação e 1.000 solicitações de ponto de extremidade de previsão. |

   ![Inserir configurações do novo aplicativo](./media/get-started-portal-build-app/create-new-app-settings.png)

## <a name="create-intents"></a>Criar intenções

Depois de criar o aplicativo LUIS, você precisará criar intenções. Intenções são uma forma de categorizar o texto dos usuários. Por exemplo, um aplicativo de recursos humanos pode ter duas funções. Ajudar as pessoas a:

 1. Encontrar um emprego e se candidatar a ele
 1. Encontrar formulários para se candidatar a um emprego

As duas diferentes _intenções_ do aplicativo se alinham com as seguintes intenções:

|Intencional|Exemplo de texto do usuário<br>conhecido como um _enunciado_|
|--|--|
|ApplyForJob|`I want to apply for the new software engineering position in Cairo.`|
|FindForm|`Where is the job transfer form hrf-123456?`|

Para criar intenções, conclua as seguintes etapas:

1. Depois que o aplicativo é criado, você está na página **Intenções** da seção **Build**. Selecione **Criar**.

   [![Selecione Criar para criar uma intenção](./media/get-started-portal-build-app/create-new-intent-button.png)](./media/get-started-portal-build-app/create-new-intent-button.png#lightbox)

1. Insira o nome da intenção, `FindForm`, e, em seguida, selecione **Concluído**.

## <a name="add-an-example-utterance"></a>Adicionar uma expressão de exemplo

Você adicionará enunciados de exemplo depois de criar intenções. Enunciados de exemplo são um texto que um usuário insere em um chatbot ou em outro aplicativo cliente. Eles mapeiam a intenção de texto do usuário para uma intenção do LUIS.

Para a intenção `FindForm` deste aplicativo de exemplo, os enunciados de exemplo incluirão o número de formulário. O aplicativo cliente precisa do número de formulário para atender à solicitação do usuário, portanto, é importante incluí-lo no enunciado.

> [!div class="mx-imgBorder"]
> [![Inserir os exemplos de enunciado para a intenção do FindForm](./media/get-started-portal-build-app/add-example-utterance.png)](./media/get-started-portal-build-app/add-example-utterance.png#lightbox)

Adicione os 15 enunciados de exemplo a seguir à intenção `FindForm`.

|#|Exemplo de enunciados|
|--|--|
|1|Procurando por hrf-123456|
|2|Onde está o formulário de recursos humanos hrf-234591?|
|3|hrf-345623, onde ele está|
|4|É possível enviar-me o hrf-345794|
|5|Eu preciso do hrf-234695 para candidatar-me a uma vaga de emprego interna?|
|6|Meu gerente precisa saber que estou me candidatando a um emprego com hrf-234091|
|7|Para onde envio o hrf-234918? Eu obtenho uma resposta por email de que ele foi recebido?|
|8|hrf-234555|
|9|Quando o hrf-234987 foi atualizado?|
|10|Posso usar o formulário hrf-876345 me candidatar a vagas de engenharia|
|11|Uma nova versão do hrf-765234 foi enviada para minha solicitação em aberto?|
|12|Devo usar o hrf-234234 para vagas de emprego internacionais?|
|13|Erro de ortografia no hrf-234598|
|14|o hrf-234567 será editado para novos requisitos|
|15|hrf-123456, hrf-123123, hrf-234567|

Por design, esses enunciados de exemplo variam das seguintes maneiras:

* tamanho do enunciado
* [pontuação](luis-reference-application-settings.md#punctuation-normalization)
* escolha de palavras
* tempos verbais (é, foi, será)
* ordem das palavras


## <a name="create-a-regular-expression-entity"></a>Criar uma entidade de expressão regular

Para retornar o número de formulário na resposta de previsão do runtime, o formulário precisa ser marcado como uma entidade. Como o texto do número de formulário é altamente estruturado, você pode marcá-lo usando uma entidade de expressão regular. Crie a entidade com as seguintes etapas:

1. Selecione **Entidades** no menu à esquerda.

1. Selecione **Criar** na página **Entidades**.

1. Insira o nome `Human Resources Form Number`, selecione o tipo de entidade **Regex** e, em seguida, selecione **Próximo**.

   ![Criar entidade de expressão regular](./media/get-started-portal-build-app/create-regular-expression-entity.png)

1. Insira a expressão da **RegEx** (expressão regular), `hrf-[0-9]{6}`. Essa entrada corresponde aos caracteres do literal – `hrf-` – e permite exatamente seis dígitos, depois selecione **Criar**.

   ![Inserir expressão regular para entidade](./media/get-started-portal-build-app/create-regular-expression-entity-with-expression.png)


## <a name="add-example-utterances-to-the-none-intent"></a>Adicionar enunciados de exemplo à intenção None

A intenção **Nenhum** é a intenção de fallback e não deve ser deixada vazia. Essa intenção deve conter um enunciado para cada dez enunciados de exemplo que você adicionou para as outras intenções do aplicativo.

Os exemplos de enunciado da intenção **Nenhum** devem ficar fora do seu domínio do aplicativo cliente.

1. Selecione **Intenções** no menu à esquerda e, em seguida, selecione **Nenhum** na lista de intenções.

1. Adicione os quinze exemplos de enunciado a seguir à intenção:

   |Exemplos de enunciado da intenção Nenhum|
   |--|
   |Cachorros que latem demais são irritantes|
   |Peça uma pizza para mim|
   |Pinguins no oceano|

   Para este aplicativo, esses exemplos de enunciado estão fora do domínio. Se o domínio incluir animais, alimentos ou o oceano, você deverá usar outros exemplos de enunciado para a intenção **Nenhum**.

## <a name="train-the-app"></a>Treinar o aplicativo

[!INCLUDE [LUIS How to Train steps](includes/howto-train.md)]

## <a name="look-at-the-regular-expression-entity-in-the-example-utterances"></a>Examinar a entidade de expressão regular nos exemplos de enunciado

1. Verifique se a entidade é encontrada na intenção **FindForm** selecionando **Intenções** no menu à esquerda. Em seguida, selecione a intenção **FindForm**.

   A entidade é marcada onde ela aparece nos exemplos de enunciado. Caso deseje ver o texto original em vez do nome da entidade, ative/desative a **Exibição de Entidades** na barra de ferramentas.

   > [!div class="mx-imgBorder"]
   > [![Todos os exemplos de enunciado marcados com entidades](./media/get-started-portal-build-app/all-example-utterances-marked-with-entities.png)](./media/get-started-portal-build-app/all-example-utterances-marked-with-entities.png#lightbox)

## <a name="test-your-new-app-with-the-interactive-test-pane"></a>Testar seu novo aplicativo com o painel de teste interativo

Use o painel **Teste** interativo no portal do LUIS para validar se a entidade é extraída de novos enunciados que o aplicativo ainda não viu.

1. Selecione **Testar** no menu superior direito.

1. Adicione um novo enunciado e, em seguida, pressione Enter:

   ```Is there a form named hrf-234098```

    Selecione **Inspecionar** para ver as previsões de entidade.

   > [!div class="mx-imgBorder"]
   > ![Testar o novo enunciado no painel de teste](./media/get-started-portal-build-app/test-new-utterance.png)

   A primeira intenção prevista é corretamente **FindForm**, com uma confiança acima de 90% (0,977). A entidade **Número de Formulário de Recursos Humanos** é extraída com um valor igual a hrf-234098.

## <a name="clean-up-resources"></a>Limpar os recursos

Quando você concluir este início rápido e não estiver passando para o próximo, selecione **Meus aplicativos** no menu de navegação superior. Em seguida, marque a caixa de seleção à esquerda do aplicativo na lista e selecione **Excluir** na barra de ferramentas de contexto acima da lista.

## <a name="next-steps"></a>Próximas etapas

> [!div class="nextstepaction"]
> [2. Implantar um aplicativo](get-started-portal-deploy-app.md)

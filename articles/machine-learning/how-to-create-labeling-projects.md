---
title: Criar um projeto de rotulagem de dados
titleSuffix: Azure Machine Learning
description: Saiba como criar e executar projetos de rotulagem para marcar dados para o aprendizado de máquina.
author: lobrien
ms.author: laobri
ms.service: machine-learning
ms.topic: tutorial
ms.date: 11/04/2019
ms.openlocfilehash: e469837c8e374e62824bd8f7a7feb110ed1be9c9
ms.sourcegitcommit: 812bc3c318f513cefc5b767de8754a6da888befc
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/12/2020
ms.locfileid: "77153104"
---
# <a name="create-a-data-labeling-project-and-export-labels"></a>Criar um projeto de rotulagem de dados e exportar rótulos 

A rotulagem de um grande volume de dados em projetos de Machine Learning costuma ser um problema. Os projetos que têm um componente de pesquisa visual computacional, como classificação de imagem ou detecção de objetos, geralmente exigem rótulos para milhares de imagens.
 
O [Azure Machine Learning](https://ml.azure.com/) oferece um lugar central para criar, gerenciar e monitorar projetos de rotulagem. Use-o para coordenar dados, rótulos e membros da equipe, a fim de gerenciar tarefas de rotulagem com eficiência. O Machine Learning dá suporte à classificação de imagem (de vários rótulos ou multiclasse) e à identificação do objeto junto com caixas delimitadoras.

O Machine Learning acompanha o progresso e mantém a fila de tarefas de rotulagem incompletas. Os rotuladores não precisam ter uma conta do Azure para participar. Depois de serem autenticados com a sua conta Microsoft ou com o [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-whatis), eles poderão trabalhar com rotulagem à vontade, de acordo com o tempo que tiverem disponível para isso.

No Machine Learning, você inicia e interrompe o projeto, adiciona e remove pessoas e equipes e monitora o progresso. Você pode exportar os dados rotulados no formato COCO ou como um conjunto de dados do Azure Machine Learning.

> [!Important]
> Somente projetos de rotulagem de classificação de imagens e identificação de objetos são compatíveis no momento. Além disso, as imagens de dados devem estar disponíveis em um armazenamento de blobs do Azure. (Se você não tiver um armazenamento de dados existente, poderá carregar imagens durante a criação do projeto). 

Neste artigo, você aprenderá a:

> [!div class="checklist"]
> * Criar um projeto
> * Especificar os dados e a estrutura do projeto
> * Gerenciar as equipes e as pessoas que trabalham no projeto
> * Executar e monitorar o projeto
> * Exportar os rótulos


## <a name="prerequisites"></a>Pré-requisitos

* Os dados que deseja rotular, em arquivos locais ou no Armazenamento do Azure.
* O conjunto de rótulos que deseja aplicar.
* As instruções para rotulagem.
* Uma assinatura do Azure. Se você não tiver uma assinatura do Azure, crie uma [conta gratuita](https://aka.ms/AMLFree) antes de começar.
* Um Workspace do Machine Learning. Confira [Criar um Workspace do Azure Machine Learning](how-to-manage-workspace.md).

## <a name="create-a-labeling-project"></a>Criar um projeto de rotulagem

Os projetos de rotulagem são administrados no Azure Machine Learning. Use a página **Projetos de rotulagem** para gerenciar os projetos e as pessoas. Um projeto tem uma ou mais equipes atribuídas a ele e uma equipe tem uma ou mais pessoas atribuídas a ela.

Caso os seus dados já estejam no Armazenamento de Blobs do Azure, você deverá disponibilizá-los como um armazenamento de dados antes de criar o projeto de rotulagem. Para obter detalhes, confira [Criar e registrar armazenamentos de dados](https://docs.microsoft.com/azure/machine-learning/how-to-access-data#create-and-register-datastores).

Para criar um projeto, selecione **Adicionar projeto**. Dê ao projeto um nome apropriado e selecione **Tipo de tarefa de rotulagem**.

![Assistente de criação de projeto de rotulagem](./media/how-to-create-labeling-projects/labeling-creation-wizard.png)

* Escolha **Classificação de Imagem de Vários Rótulos** para os projetos quando desejar aplicar *um ou mais* rótulos de um conjunto de classes a uma imagem. Por exemplo, uma foto de um cachorro pode ser rotulada com *cachorro* e *dia*.
* Escolha **Classificação de Imagem Multiclasse** para os projetos quando só desejar aplicar uma *única classe* de um conjunto de classes a uma imagem.
* Escolha **Identificação do Objeto (Caixa Delimitadora)** para os projetos quando desejar atribuir uma classe e uma caixa delimitadora a cada objeto dentro de uma imagem.

Selecione **Avançar** quando estiver pronto para continuar.

## <a name="specify-the-data-to-label"></a>Especificar os dados a serem rotulados

Se já tiver criado um conjunto de dados que contenha seus dados, selecione-o na lista suspensa **Selecionar um conjunto de dados existente**. Ou, então, selecione **Criar um conjunto de dados** para usar um armazenamento de dados do Azure existente ou fazer upload de arquivos locais.

### <a name="create-a-dataset-from-an-azure-datastore"></a>Criar um conjunto de dados de um armazenamento de dados do Azure

Em muitos casos, é suficiente apenas fazer upload de arquivos locais. Mas o [Gerenciador de Armazenamento do Azure](https://azure.microsoft.com/features/storage-explorer/) fornece uma maneira mais rápida e robusta de transferir uma grande quantidade de dados. Recomendamos o Gerenciador de Armazenamento como o método padrão de mover arquivos.

Para criar um conjunto de dados com base nos dados que você já armazenou no Armazenamento de Blobs do Azure:

1. Selecione **Criar um conjunto de dados** > **Com base no armazenamento de dados**.
1. Atribua um **Nome** ao conjunto de dados.
1. Escolha **Arquivo** como o **Tipo de conjunto de dados**.  
1. Selecione o armazenamento de dados.
1. Se os dados estiverem em uma subpasta no Armazenamento de Blobs, escolha **Procurar** para selecionar o caminho.
    * Acrescente "/**" ao caminho para incluir todos os arquivos nas subpastas do caminho selecionado.
    * Acrescente "* */* .*" para incluir todos os dados no contêiner atual e nas subpastas.
1. Forneça uma descrição para o conjunto de dados.
1. Selecione **Avançar**.
1. Confirme os detalhes. Selecione **Voltar** para modificar as configurações ou **Criar** para criar o conjunto de dados.

### <a name="create-a-dataset-from-uploaded-data"></a>Criar um conjunto de dados com base nos dados carregados

Para fazer upload dos dados diretamente:

1. Selecione **Criar um conjunto de dados** > **Com base nos arquivos locais**.
1. Atribua um **Nome** ao conjunto de dados.
1. Escolha "Arquivo" como o **Tipo de conjunto de dados**.
1. *Opcional:* Selecione **Configurações avançadas** para personalizar o armazenamento de dados, o contêiner e o caminho para os dados.
1. Selecione **Procurar** para selecionar os arquivos locais a serem carregados.
1. Forneça uma descrição do conjunto de dados.
1. Selecione **Avançar**.
1. Confirme os detalhes. Selecione **Voltar** para modificar as configurações ou **Criar** para criar o conjunto de dados.

Os dados são carregados no armazenamento de blobs padrão ("workspaceblobstore") do Workspace do Machine Learning.

## <a name="specify-label-classes"></a>Especificar classes de rótulo

Na página **Classes de rótulo**, especifique o conjunto de classes para categorizar os dados. Faça isso com cuidado, pois a precisão e a velocidade dos rotuladores serão afetadas pela capacidade deles de fazer escolhas entre as classes. Por exemplo, em vez de escrever por extenso o gênero e a espécie de plantas ou animais, use um código de campo ou abrevie o gênero.

Insira um rótulo por linha. Use o botão **+** para adicionar uma nova linha. Se você tiver mais de três ou quatro rótulos, porém, menos de 10, o ideal será colocar números como prefixos para os nomes ("1: ", "2: "), de modo que os rotuladores possam usar as teclas numéricas para acelerar o trabalho.

## <a name="describe-the-labeling-task"></a>Descrever a tarefa de rotulagem

É importante explicar a tarefa de rotulagem de maneira clara. Na página **Instruções de rotulagem**, você pode adicionar um link para um site externo indicando as instruções de rotulagem. Mantenha as instruções orientadas a tarefas e apropriadas para o público-alvo. Considere estas perguntas:

* Quais são os rótulos que as pessoas verão e como elas escolherão entre eles? Há um texto de referência para consulta?
* O que elas deverão fazer se nenhum rótulo parecer apropriado?
* O que elas deverão fazer se vários rótulos parecerem apropriados?
* Qual limite de confiança elas devem aplicar a um rótulo? Você deseja que elas forneçam o "melhor palpite" se não tiverem certeza?
* O que elas deverão fazer com objetos de interesse parcialmente obstruídos ou sobrepostos?
* O que elas deverão fazer se um objeto de interesse for recortado pela borda da imagem?
* O que elas deverão fazer depois de enviarem um rótulo se acreditarem que cometeram um erro?

Com relação às caixas delimitadoras, entre as perguntas importantes se incluem:

* Como a caixa delimitadora é definida para essa tarefa? Ela deverá estar totalmente no interior do objeto ou deverá estar no exterior? Ela deverá ser cortada o mais próximo possível ou um espaço livre é aceitável?
* Qual nível de cuidado e de consistência você espera que os rotuladores apliquem na definição das caixas delimitadoras?

>[!NOTE]
> Observe que os rotuladores poderão selecionar os nove primeiros rótulos usando as teclas numéricas 1 a 9.

## <a name="initialize-the-labeling-project"></a>Inicializar o projeto de rotulagem

Depois que o projeto de rotulagem for inicializado, alguns aspectos do projeto serão imutáveis. Não será possível alterar o tipo de tarefa nem o conjunto de dados. Você *poderá* modificar os rótulos e a URL para a descrição da tarefa. Examine cuidadosamente as configurações antes de criar o projeto. Depois de enviar o projeto, você será direcionado novamente para a home page **Rotulagem de Dados**, que mostrará o projeto como **Inicializando**. Essa página não é atualizada automaticamente. Portanto, após uma pausa, atualize manualmente a página para ver o status do projeto como **Criado**.

## <a name="manage-teams-and-people"></a>Gerenciar equipes e pessoas

Por padrão, cada projeto de rotulagem criado recebe uma nova equipe, com você como membro. No entanto, as equipes também podem ser compartilhadas entre projetos. Além disso, os projetos podem ter mais de uma equipe. Para criar uma equipe, selecione **Adicionar equipe** na página **Equipes**.

Gerencie as pessoas na página **Pessoas**. Adicione e remova pessoas por endereço de email. Cada rotulador precisa se autenticar por meio da sua conta Microsoft ou do Azure Active Directory, se você usá-lo.  

Depois de adicionar uma pessoa, você poderá atribuir essa pessoa a uma ou mais equipes: Acesse a página **Equipes**, selecione a equipe e, em seguida, selecione **Atribuir pessoas** ou **Remover pessoas**.

Para enviar um email à equipe, selecione a equipe para exibir a página **Detalhes da equipe**. Nessa página, selecione **Enviar um email à equipe** para abrir um rascunho de email com os endereços de todos da equipe.

## <a name="run-and-monitor-the-project"></a>Executar e monitorar o projeto

Depois que você inicializar o projeto, o Azure começará a executá-lo. Selecione o projeto na página principal **Rotulagem de Dados** para acessar os **Detalhes do projeto**. A guia **Painel** mostra o progresso da tarefa de rotulagem.

Na guia **Dados**, você poderá ver o conjunto de dados e examinar os dados rotulados. Se você perceber que há dados rotulados incorretamente, selecione-os e escolha **Rejeitar**, o que removerá os rótulos e colocará os dados novamente na fila sem rótulo.

Use a guia **Equipe** para atribuir equipes ou cancelar a atribuição de equipes no projeto.

Para pausar ou reiniciar o projeto, selecione o botão **Pausar**/**Iniciar**. Você só pode rotular dados quando o projeto está em execução.

Você pode rotular dados diretamente da página **Detalhes do projeto** selecionando **Rotular dados**.

## <a name="add-labels-to-a-project"></a>Adicionar rótulos a um projeto

Durante o processo de rotulagem, você pode achar que rótulos adicionais são necessários para classificar suas imagens.  Por exemplo, você pode adicionar um rótulo "Desconhecido" ou "Outro" para indicar imagens confusas.

Use estas etapas para adicionar um ou mais rótulos a um projeto:

1. Selecione o projeto na página principal **Rotulagem de Dados**.
1. Na parte superior da página, selecione **Pausar** para interromper os rotuladores de suas atividades.
1. Selecione a guia **Detalhes**.
1. Na lista à esquerda, selecione **Classes de rótulo**.
1. No topo da lista, selecione **+ Adicionar rótulos** ![Adicionar um rótulo](media/how-to-create-labeling-projects/add-label.png)
1. No formulário, adicione o novo rótulo e escolha como continuar.  Como alterou os rótulos disponíveis para uma imagem, você escolhe como tratar os dados já rotulados:
    * Recomece, removendo todos os rótulos existentes.  Escolha esta opção se desejar começar a rotular desde o início com o novo conjunto completo de rótulos. 
    * Recomece, mantendo todos os rótulos existentes.  Escolha esta opção para marcar todos os dados como sem rótulo, mas mantenha os rótulos existentes como uma marca padrão para imagens que foram rotuladas anteriormente.
    * Continue, mantendo todos os rótulos existentes. Escolha esta opção para manter todos os dados já rotulados como estão, e comece a usar o novo rótulo para dados ainda não rotulados.
1. Modifique a página de instruções conforme necessário para os novos rótulos.
1. Depois de adicionar todos os novos rótulos, na parte superior da página, selecione **Iniciar** para reiniciar o projeto.  

## <a name="export-the-labels"></a>Exportar os rótulos

Você pode exportar os dados de rótulo para experimentação no Machine Learning a qualquer momento. Os rótulos de imagens podem ser exportados no [formato COCO](http://cocodataset.org/#format-data) ou como um conjunto de dados do Azure Machine Learning. Use o botão **Exportar** na página **Detalhes do projeto** do projeto de rotulagem.

O arquivo COCO é criado no armazenamento de blobs padrão do Workspace do Azure Machine Learning em uma pasta dentro de *export/coco*. Acesse o conjunto de dados exportado do Azure Machine Learning na seção **Conjuntos de dados** do Machine Learning. A página de detalhes do conjunto de dados também fornece um código de exemplo para acessar seus rótulos por meio do Python.

![Conjunto de dados exportado](./media/how-to-create-labeling-projects/exported-dataset.png)

## <a name="next-steps"></a>Próximas etapas

* Rotular imagens para [classificação de imagens ou detecção de objetos](how-to-label-images.md)
* Saiba mais sobre [o Azure Machine Learning e o Machine Learning Studio (clássico)](compare-azure-ml-to-studio-classic.md)

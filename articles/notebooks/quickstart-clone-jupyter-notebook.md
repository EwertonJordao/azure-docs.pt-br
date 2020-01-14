---
title: Clonar um Jupyter notebook do GitHub com a versão prévia do Azure Notebooks
description: Clone rapidamente um Jupyter notebook de um repositório GitHub e execute-o em sua conta do Azure Notebooks.
ms.topic: quickstart
ms.date: 12/04/2018
ms.openlocfilehash: 8aa88008ece170a5eed7ab491e3318aed5168923
ms.sourcegitcommit: f788bc6bc524516f186386376ca6651ce80f334d
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/03/2020
ms.locfileid: "75647094"
---
# <a name="quickstart-clone-a-notebook-in-azure-notebooks-preview"></a>Início Rápido: Clonar um notebook na versão prévia do Azure Notebooks

Muitos desenvolvedores e cientistas de dados armazenam seus notebooks em [repositórios GitHub](https://github.com), um serviço gratuito que fornece armazenamento e controle de versão para muitos tipos de projeto diferentes. O GitHub costuma ser usado como um meio de colaboração em Jupyter notebooks executados localmente. Nesses casos, cada colaborador mantém uma cópia local do repositório e executa os notebooks com base nessa cópia.

A clonagem cria uma cópia de um notebook do GitHub em sua conta do Azure Notebooks. Esse clone é independente de seu repositório original; as alterações são armazenadas apenas em sua conta do Azure Notebooks e não afetam o original. Como o clone está na nuvem, você pode compartilhar o projeto com outros colaboradores que não precisam fazer cópias locais, nem mesmo ter o Jupyter instalado em seus próprios computadores. Você também pode clonar um notebook simplesmente como um ponto de partida para um projeto próprio ou para obter arquivos de dados.

[!INCLUDE [notebooks-status](../../includes/notebooks-status.md)]

## <a name="clone-azure-cognitive-services-notebooks"></a>Clonar notebooks dos Serviços Cognitivos do Azure

1. Vá até [Azure Notebooks](https://notebooks.azure.com) e entre. (Para obter mais detalhes, consulte [Início Rápido – Entrar no Azure Notebooks](quickstart-sign-in-azure-notebooks.md)).

1. Em sua página de perfil público, selecione **Meus Projetos** na parte superior da página:

    ![Link Meus Projetos na parte superior da janela do navegador](media/quickstarts/my-projects-link.png)

1. Na página **Meus Projetos**, selecione o botão de seta para cima (atalho de teclado: U; o botão é exibido como **Carregar Repositório GitHub** quando a janela do navegador é grande o suficiente):

    ![Comando Carregar Repositório GitHub na página Meus Projetos](media/quickstarts/upload-github-repo-command.png)

1. No comando **Carregar Repositório GitHub** exibido, insira ou defina os seguintes detalhes e selecione **Importar**:

   - **Repositório GitHub**: Microsoft/cognitive-services-notebooks (esse nome clona os Jupyter notebooks para os Serviços Cognitivos do Azure em [https://github.com/Microsoft/cognitive-services-notebooks](https://github.com/Microsoft/cognitive-services-notebooks)).
   - **Clonar recursivamente**: (desmarcado)
   - **Nome do projeto:** : Clone dos Serviços Cognitivos
   - **ID do Projeto**: cognitive-services-clone
   - **Público**: (desmarcado)

     ![Pop-up de Carregar Repositório GitHub para coletar informações sobre o repositório](media/quickstarts/upload-github-repo-popup.png)

1. Seja paciente enquanto o processo for concluído; a clonagem de um repositório pode levar alguns minutos.

1. Depois que a clonagem for concluída, o Azure Notebooks levará você para o novo projeto, no qual você poderá ver as cópias de todos os arquivos.

    [![](media/quickstarts/completed-clone.png "View of a completed clone")](media/quickstarts/completed-clone.png#lightbox)

## <a name="share-a-notebook"></a>Compartilhar um notebook

1. Para compartilhar sua cópia do projeto clonado, use o controle **Compartilhar** ou obtenha um link, obtenha o código HTML ou Markdown que contém o link ou crie uma mensagem de email com o link:

    ![Comando de compartilhamento de projeto](media/quickstarts/share-project-command.png)

1. Como você desmarcou a opção **Pública** ao clonar o projeto, o clone é particular. Para tornar sua cópia pública, selecione **Configurações do Projeto**, defina a opção **Projeto público** no pop-up e, em seguida, selecione **Salvar**.

1. Selecione um notebook no projeto para executá-lo. Cada notebook no repositório dos Serviços Cognitivos do Azure, por exemplo, é seu próprio Início Rápido autossuficiente. A imagem abaixo mostra o resultado do uso do notebook BingImageSearchAPI, após a adição de uma chave de assinatura da API dos Serviços Cognitivos e a alteração do termo de pesquisa "cachorrinhos" para "coelhinhos":

    ![Executando um Jupyter notebook clonado no GitHub](media/quickstarts/clone-notebook-result.png)

1. Quando terminar de executar o notebook, selecione **Arquivo** > **Fechar e parar** para fechar o notebook e a janela do navegador.

1. Para compartilhar um notebook individual no projeto, clique com o botão direito do mouse no notebook e selecione **Copiar link** (atalho de teclado: y):

    ![Comando de menu de contexto para copiar um link para um notebook individual](media/quickstarts/copy-link-to-individual-notebook.png)

1. Para editar arquivos que não sejam notebooks, clique com o botão direito do mouse no projeto e selecione **Editar arquivo** (atalho de teclado: i). A ação padrão, **Executar** (atalho de teclado: r), apenas mostra o conteúdo do arquivo e não permite a edição.

## <a name="next-steps"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Tutorial: criar e executar um Jupyter Notebook para fazer uma regressão linear](tutorial-create-run-jupyter-notebook.md)

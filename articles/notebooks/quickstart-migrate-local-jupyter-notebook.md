---
title: Migrar um Jupyter notebook local para a versão prévia do Azure Notebooks
description: Transfira rapidamente um Jupyter notebook para a versão prévia do Azure Notebooks do computador local ou de uma URL da Web e, em seguida, compartilhe-o para colaboração.
ms.topic: quickstart
ms.date: 12/04/2018
ms.openlocfilehash: 9e5270c59a64f9510f9108bbe4d00b922178888c
ms.sourcegitcommit: f788bc6bc524516f186386376ca6651ce80f334d
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/03/2020
ms.locfileid: "75647043"
---
# <a name="quickstart-migrate-a-local-jupyter-notebook-in-azure-notebooks-preview"></a>Início Rápido: Migrar um Jupyter notebook local na versão prévia do Azure Notebooks

Os notebooks Jupyter que você criou localmente no seu próprio computador são acessíveis apenas para você. Você pode compartilhar seus arquivos por meio de uma variedade de meios, mas, em seguida, os destinatários têm sua própria cópia local do bloco de anotações e é difícil incorporar as alterações que podem fazer. Você também pode armazenar os blocos de anotações em um repositório compartilhado online, como o GitHub, mas isso ainda exige que cada colaborador tenha sua própria instalação do Jupyter local com a mesma configuração que o seu.

Ao migrar seus blocos de anotações locais ou baseada em repositório para o Azure Notebooks, armazena-as na nuvem da qual você pode instantaneamente compartilhar com seus colaboradores. Esses colaboradores precisam apenas de um navegador para exibir e executar o bloco de anotações e se eles [entrarem](quickstart-sign-in-azure-notebooks.md) para o Azure Notebooks eles também podem fazer alterações.

[!INCLUDE [notebooks-status](../../includes/notebooks-status.md)]

Este Início Rápido demonstra o processo de migração de um bloco de anotações do seu computador local ou outra URL de arquivo acessível. Para migrar os notebooks de um repositório GitHub, consulte o [Guia de Início Rápido: Clonar um notebook](quickstart-clone-jupyter-notebook.md).

## <a name="create-a-project-on-azure-notebooks"></a>Criar um projeto no Azure Notebooks

1. Vá até [Azure Notebooks](https://notebooks.azure.com) e entre. (Para obter mais detalhes, consulte [Início Rápido – Entrar no Azure Notebooks](quickstart-sign-in-azure-notebooks.md)).

1. Em sua página de perfil público, selecione **Meus Projetos** na parte superior da página:

    ![Link Meus Projetos na parte superior da janela do navegador](media/quickstarts/my-projects-link.png)

1. Na página **Meus Projetos**, selecione **+ Novo Projeto**(atalho de teclado: n); o botão poderá aparecer apenas como **+** , se a janela do navegador for estreita:

    ![Comando Novo Projeto na página Meus Projetos](media/quickstarts/new-project-command.png)

1. Na janela pop-up **Criar novo projeto** que aparece, insira os valores apropriados para o bloco de anotações que você está migrando nos campos **Nome do projeto** e **ID do projeto**, desmarque as opções para **Projeto público** e **Criar um README.md**, em seguida, selecione **Criar**.

## <a name="upload-the-local-notebook"></a>Carregar o notebook local

1. Na página do projeto, selecione **carregar** (que pode ser exibido com uma seta para cima apenas se a janela do navegador for pequena), em seguida, selecione 1. No popup que aparece, selecione **Do computador** se o seu notebook estiver localizado no sistema de arquivos local, ou **Da URL** se o bloco de anotações estiver localizado online:

    ![Comando para carregar um bloco de anotações de uma URL ou o computador local](media/quickstarts/upload-from-computer-url-command.png)

   (novamente, se o bloco de anotações estiver em um repositório do GitHub, siga as etapas no [Guia de Início Rápido: Clonar um notebook](quickstart-clone-jupyter-notebook.md) em vez disso).

   - Se estiver usando **Do computador**, arraste e solte seus arquivos *ipynb* para o pop-up ou selecione **Escolher arquivos**, em seguida, navegue e selecione os arquivos que você deseja importar. Depois selecionar **Carregar**. Os arquivos carregados recebem o mesmo nome que os arquivos locais. (você não precisa carregar o conteúdo de quaisquer pastas *.ipynb_checkpoints*.)

     ![Carregar do popup do computador](media/quickstarts/upload-from-computer-popup.png)

   - Se usar **Da URL**, insira o endereço de origem no campo **URL do Arquivo** e o nome do arquivo para atribuir ao bloco de anotações do projeto no campo **Nome do Arquivo**. Depois selecionar **Carregar**. Se você tiver vários arquivos com URLs separadas, use o comando **+ Adicionar arquivo** para verificar a primeira URL que você inseriu, após o qual o pop-up fornece novos campos para outro arquivo.

     ![Carregar de URL popup](media/quickstarts/upload-from-url-popup.png)

1. Abra e execute o notebook recentemente carregado para verificar seu conteúdo e a operação. Quando terminar, selecione **Arquivo** > **Halt e fechar** para fechar o notebook.

1. Para compartilhar um link para o notebook, clique no arquivo no projeto e selecione **Copiar Link** (atalho de teclado: y), em seguida, cole o link na mensagem apropriada. Como alternativa, você pode compartilhar o projeto como um todo usando o controle **Compartilhar** na página do projeto.

1. Para editar arquivos que não sejam notebooks, clique com o botão direito do mouse no projeto e selecione **Editar arquivo** (atalho de teclado: i). A ação padrão, **Executar** (atalho de teclado: r), apenas mostra o conteúdo do arquivo e não permite a edição.

## <a name="next-steps"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Tutorial: criar e executar um Jupyter Notebook para fazer uma regressão linear](tutorial-create-run-jupyter-notebook.md)

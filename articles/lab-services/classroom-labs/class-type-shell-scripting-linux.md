---
title: Configurar um laboratório de script do shell do Linux com o Azure Lab Services | Microsoft Docs
description: Saiba como configurar um laboratório para ensinar o script de Shell no Linux.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/30/2019
ms.author: spelluru
ms.openlocfilehash: 100a485588c77f6977001dae984b30ebcb1de557
ms.sourcegitcommit: 6e87ddc3cc961945c2269b4c0c6edd39ea6a5414
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/18/2020
ms.locfileid: "77443543"
---
# <a name="set-up-a-lab-to-teach-shell-scripting-on-linux"></a>Configurar um laboratório para ensinar scripts de Shell no Linux
Este artigo mostra como configurar um laboratório para ensinar o script de Shell no Linux. O script é uma parte útil da administração do sistema que permite aos administradores evitar tarefas repetitivas. Neste cenário de exemplo, a classe abrange scripts do Bash tradicionais e scripts avançados. Scripts avançados são scripts que combinam comandos do Bash e o Ruby. Essa abordagem permite que o Ruby passe os dados e que os comandos do Bash interajam com o shell. 

Os alunos que fazem essas aulas de script obtêm uma máquina virtual do Linux para aprender os conceitos básicos do Linux e também se familiarizarem com os scripts do shell Bash. A máquina virtual do Linux é fornecida com o acesso à Área de Trabalho Remota habilitado e com os editores de texto [gedit](https://help.gnome.org/users/gedit/stable/) e [Visual Studio Code](https://code.visualstudio.com/) instalados.

## <a name="lab-configuration"></a>Configuração do laboratório
Para configurar este laboratório, você precisa de uma assinatura do Azure para começar. Se você não tiver uma assinatura do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/) antes de começar. Depois de ter uma assinatura do Azure, você pode criar uma nova conta de laboratório no Azure Lab Services ou usar uma conta de laboratório existente. Consulte o tutorial a seguir para criar uma nova conta de laboratório: [tutorial para configurar uma conta de laboratório](tutorial-setup-lab-account.md).

Depois de criar a conta de laboratório, habilite as seguintes configurações na conta do laboratório: 

| Configuração de conta do laboratório | Instruções |
| ----------- | ------------ |  
| Imagens do Marketplace | Habilite a imagem do [Ubuntu Server 18, 4 LTS](https://azuremarketplace.microsoft.com/marketplace/apps/canonical.0001-com-ubuntu-server-bionic) para uso em sua conta de laboratório. Para obter mais informações, consulte [especificar imagens do Marketplace disponíveis para criadores de laboratório](specify-marketplace-images.md). | 

Siga [este tutorial](tutorial-setup-classroom-lab.md) para criar um novo laboratório e aplicar as seguintes configurações:

| Configurações do laboratório | Valor/instruções | 
| ------------ | ------------------ |
| Tamanho da VM (máquina virtual) | Pequena  |
| Imagem da VM | [Ubuntu Server 18, 4 LTS](https://azuremarketplace.microsoft.com/marketplace/apps/canonical.0001-com-ubuntu-server-bionic) |
| Habilitar conexão de área de trabalho remota | Habilitar. <p>Habilitar essa configuração permitirá que professores e alunos se conectem às suas VMs usando a área de trabalho remota (RDP). Para obter mais informações, consulte [habilitar a área de trabalho remota para máquinas virtuais Linux em um laboratório no Azure Lab Services](how-to-enable-remote-desktop-linux.md#connect-to-the-template-vm). </p>|


## <a name="install-desktop-and-xrdp"></a>Instalar o desktop e o xrdp
A imagem do [Ubuntu Server 18, 4 LTS](https://azuremarketplace.microsoft.com/marketplace/apps/canonical.0001-com-ubuntu-server-bionic) não tem o servidor de área de trabalho remota instalado por padrão. Siga as instruções no artigo [instalar e configurar área de trabalho remota para se conectar a uma VM do Linux no Azure](../../virtual-machines/linux/use-remote-desktop.md) para instalar os pacotes necessários no computador de modelo para se conectar via protocolo de área de trabalho remota.

## <a name="install-ruby"></a>Instalar Ruby
O Ruby é uma linguagem dinâmica de software livre que pode ser combinada com scripts bash. Esta seção mostra como usar `apt-get` para instalar a versão mais recente do [Ruby](https://www.ruby-lang.org/).

1. Instale as atualizações executando os seguintes comandos:

    ```bash
    sudo apt-get update 
    sudo apt-get upgrade 
    ```
2.  Instale o [Ruby](https://www.ruby-lang.org/).  O Ruby é uma linguagem dinâmica de software livre que pode ser combinada com scripts bash. 
    
    ```bash
    sudo apt-get install ruby-full
    ```

## <a name="install-development-tools"></a>Instalar ferramentas de desenvolvimento
Esta seção mostra como instalar alguns editores de texto. Gedit é o editor de texto padrão para o ambiente de desktop GNOME. Ele foi projetado como um editor de texto de uso geral. Visual Studio Code é um editor de texto que inclui suporte para depuração e integração de controle do código-fonte.

> [!NOTE]
> Há vários editores de texto diferentes disponíveis. Visual Studio Code e gedit são apenas dois exemplos.

1. Instale o [gedit](https://help.gnome.org/users/gedit/stable/).

    ```bash
    sudo apt-get install gedit
    ```
1. Instale o [Visual Studio Code](https://code.visualstudio.com/).  O Visual Studio Code pode ser instalado usando o snap Store.  Para obter opções de instalação alternativas, consulte [Visual Studio Code downloads alternativos](https://code.visualstudio.com/#alt-downloads).

    ```bash
    sudo snap install vscode --classic 
    ```

    O modelo agora está atualizado e tem a linguagem de programação e as ferramentas de desenvolvimento necessárias para concluir o laboratório. A imagem de modelo agora pode ser publicada no laboratório. Selecione o botão **publicar** na página modelo para publicar o modelo no laboratório.  

## <a name="cost"></a>Custo 
Se você quiser estimar o custo deste laboratório, poderá usar o exemplo a seguir:
 
Para uma classe de 25 alunos com 20 horas de tempo de classe agendada e 10 horas de cota para a casa ou as atribuições, o preço do laboratório seria: 

25 alunos * (20 + 10) horas * 20 unidades de laboratório * 0, 1 USD por hora = 150 USD

Para obter mais informações sobre os preços, encontre o seguinte documento: [Azure Lab Services preços](https://azure.microsoft.com/pricing/details/lab-services/).

## <a name="conclusion"></a>Conclusão
Este artigo descreveu as etapas para criar um laboratório para classes de script. Embora este artigo se concentre na configuração das ferramentas de script do Ruby no computador Linux, a mesma configuração pode ser usada para outras classes de script, como Python no Linux.

## <a name="next-steps"></a>Próximas etapas
As próximas etapas são comuns à configuração de qualquer laboratório:

- [Adicionar usuários](tutorial-setup-classroom-lab.md#add-users-to-the-lab)
- [Definir cota](how-to-configure-student-usage.md#set-quotas-for-users)
- [Definir um agendamento](tutorial-setup-classroom-lab.md#set-a-schedule-for-the-lab) 
- [Links de registro de email para alunos](how-to-configure-student-usage.md#send-invitations-to-users). 






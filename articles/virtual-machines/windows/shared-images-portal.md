---
title: Criar uma galeria de imagens compartilhadas do Azure usando o portal
description: Saiba como usar portal do Azure para criar e compartilhar imagens de máquinas virtuais.
author: cynthn
ms.service: virtual-machines-windows
ms.subservice: imaging
ms.topic: how-to
ms.workload: infrastructure
ms.date: 11/06/2019
ms.author: cynthn
ms.openlocfilehash: 1560a67d73b712ba1f295992fce4f7f1a2ae75bd
ms.sourcegitcommit: 086d7c0cf812de709f6848a645edaf97a7324360
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/23/2020
ms.locfileid: "82100695"
---
# <a name="create-an-azure-shared-image-gallery-using-the-portal"></a>Criar uma galeria de imagens compartilhadas do Azure usando o portal

Uma [Galeria de Imagens Compartilhadas](shared-image-galleries.md) simplifica o compartilhamento da imagem personalizada em sua organização. Imagens personalizadas são como imagens do marketplace, mas você mesmo as cria. As imagens personalizadas podem ser usadas para inicializar tarefas de implantação, como pré-carregamento de aplicativos, configurações de aplicativos e outras configurações do sistema operacional. 

A galeria de imagens compartilhadas permite compartilhar suas imagens da VM personalizadas com outras pessoas em sua organização, dentro ou entre regiões, em um locatário do AAD. Escolha quais imagens você deseja compartilhar, em quais regiões deseja torná-las disponíveis e com quem deseja compartilhá-las. Você pode criar várias galerias, de modo que pode agrupar logicamente as imagens compartilhadas. 

A galeria é um recurso de nível superior que fornece RBAC (controle de acesso completo baseado em função). As imagens podem ser atualizadas, e você pode optar por replicar cada versão da imagem para um conjunto diferente de regiões do Azure. A galeria funciona apenas com imagens gerenciadas.

O recurso Galeria de Imagens Compartilhadas tem vários tipos de recursos. Usaremos ou criaremos estes itens neste artigo:

| Recurso | Descrição|
|----------|------------|
| **Imagem gerenciada** | Uma imagem básica que pode ser usada sozinha ou usada para criar uma **versão de imagem** em uma galeria de imagens. As imagens gerenciadas são criadas a partir de VMs [generalizadas](shared-image-galleries.md#generalized-and-specialized-images) . Uma imagem gerenciada é um tipo especial de VHD que pode ser usado para criar várias VMs e agora pode ser usado para criar versões de imagem compartilhada. |
| **Instantâneo** | Uma cópia de um VHD que pode ser usado para criar uma **versão de imagem**. Os instantâneos podem ser obtidos de uma VM [especializada](shared-image-galleries.md#generalized-and-specialized-images) (uma que não tenha sido generalizada) em seguida, usados sozinhos ou com instantâneos de discos de dados, para criar uma versão de imagem especializada.
| **Galeria de imagens** | Como o Azure Marketplace, uma **galeria de imagens** é um repositório para gerenciar e compartilhar imagens, mas você controla quem tem acesso. |
| **Definição da imagem** | As imagens são definidas em uma galeria e contêm informações sobre a imagem e os requisitos para usá-la em sua organização. Você pode incluir informações como se a imagem é generalizada ou especializada, o sistema operacional, os requisitos mínimos e máximos de memória e notas de versão. É uma definição de um tipo de imagem. |
| **Versão da imagem** | Uma **versão da imagem** é usada para criar uma VM ao usar uma galeria. Você pode ter diversas versões de uma imagem conforme necessário para seu ambiente. Como uma imagem gerenciada, quando você usa uma **versão da imagem** para criar uma VM, a versão da imagem é usada para criar novos discos para a VM. Versões de imagem podem ser usadas várias vezes. |

<br>


> [!IMPORTANT]
> As imagens especializadas estão atualmente em visualização pública.
> Essa versão prévia é fornecida sem um contrato de nível de serviço e não é recomendada para cargas de trabalho de produção. Alguns recursos podem não ter suporte ou podem ter restrição de recursos. Para obter mais informações, consulte [Termos de Uso Complementares de Versões Prévias do Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>
> **Limitações de visualização conhecidas** As VMs só podem ser criadas a partir de imagens especializadas usando o portal ou a API. O não é um suporte de CLI ou PowerShell para a versão prévia.

## <a name="before-you-begin"></a>Antes de começar

Para concluir o exemplo neste artigo, você deve ter uma imagem gerenciada existente de uma VM generalizada ou um instantâneo de uma VM especializada. Você pode seguir [o tutorial: criar uma imagem personalizada de uma VM do Azure com Azure PowerShell](tutorial-custom-images.md) para criar uma imagem gerenciada ou [criar um instantâneo](snapshot-copy-managed-disk.md) para uma VM especializada. Para imagens gerenciadas e instantâneos, o tamanho do disco de dados não pode ser superior a 1 TB.

Ao trabalhar com este artigo, substitua o grupo de recursos e os nomes de VM quando for necessário.


[!INCLUDE [virtual-machines-common-shared-images-portal](../../../includes/virtual-machines-common-shared-images-portal.md)]
 
## <a name="create-vms"></a>Criar VMs

Agora você pode criar uma ou mais novas VMs. Este exemplo cria uma VM chamada *myVM*, no *MyResource*, no datacenter *leste dos EUA* .

1. Vá para a definição de imagem. Você pode usar o filtro de recursos para mostrar todas as definições de imagem disponíveis.
1. Na página da definição de imagem, selecione **criar VM** no menu na parte superior da página.
1. Para **grupo de recursos**, selecione **criar novo** e digite grupo de *recursos* para o nome.
1. Em **nome da máquina virtual**, digite *myVM*.
1. Em **Região**, selecione *Leste dos EUA*.
1. Para **as opções de disponibilidade**, deixe o padrão de *nenhuma redundância de infraestrutura necessária*.
1. O valor da **imagem** será preenchido automaticamente com a `latest` versão da imagem se você tiver iniciado na página para a definição da imagem.
1. Para **tamanho**, escolha um tamanho de VM na lista de tamanhos disponíveis e escolha **selecionar**.
1. Em **conta de administrador**, se a imagem tiver sido generalizada, você precisará fornecer um nome de usuário, como *azureuser* e uma senha. A senha deve ter no mínimo 12 caracteres e atender a [requisitos de complexidade definidos](faq.md#what-are-the-password-requirements-when-creating-a-vm). Se a imagem for especializada, os campos de nome de usuário e senha ficarão esmaecidos porque o nome de usuário e a senha da VM de origem são usados.
1. Se você quiser permitir o acesso remoto à VM, em **portas de entrada públicas**, escolha **permitir portas selecionadas** e, em seguida, selecione **RDP (3389)** na lista suspensa. Se você não quiser permitir o acesso remoto à VM, deixe **nenhuma** selecionada para **portas de entrada públicas**.
1. Quando tiver terminado, selecione o botão **revisar + criar** na parte inferior da página.
1. Depois que a VM passar na validação, selecione **criar** na parte inferior da página para iniciar a implantação.


## <a name="clean-up-resources"></a>Limpar os recursos

Quando o grupo de recursos, a máquina virtual e todos os recursos relacionados não forem mais necessários, exclua-os. Para fazer isso, selecione o grupo de recursos da máquina virtual, selecione **Excluir**, em seguida, confirme o nome do grupo de recursos para excluir.

Se você quiser excluir recursos individuais, será necessário excluí-los na ordem inversa. Por exemplo, para excluir uma definição de imagem, você precisa excluir todas as versões de imagem criadas por meio dessa imagem.

## <a name="next-steps"></a>Próximas etapas

Você também pode criar um recurso de Galeria de imagens compartilhadas usando modelos. Há vários Modelos de Início Rápido do Azure disponíveis: 

- [Criar uma Galeria de Imagens Compartilhadas](https://azure.microsoft.com/resources/templates/101-sig-create/)
- [Criar uma Definição de Imagem em uma Galeria de Imagens Compartilhadas](https://azure.microsoft.com/resources/templates/101-sig-image-definition-create/)
- [Criar uma Versão da Imagem em uma Galeria de Imagens Compartilhadas](https://azure.microsoft.com/resources/templates/101-sig-image-version-create/)
- [Criar uma VM por meio de uma Versão da Imagem](https://azure.microsoft.com/resources/templates/101-vm-from-sig/)

Para obter mais informações sobre galerias de imagens compartilhadas, confira a [Visão geral](shared-image-galleries.md). Se você enfrentar problemas, confira [Solução de problemas de galerias de imagens compartilhadas](troubleshooting-shared-images.md).


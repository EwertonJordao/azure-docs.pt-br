---
title: O que é a Área de Trabalho Virtual do Windows? - Azure
description: Uma visão geral da Área de Trabalho Virtual do Windows.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: overview
ms.date: 01/21/2020
ms.author: helohr
ms.openlocfilehash: 318997e2ebd7a423d7793a75575617d06ab842ac
ms.sourcegitcommit: 38b11501526a7997cfe1c7980d57e772b1f3169b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/22/2020
ms.locfileid: "76514265"
---
# <a name="what-is-windows-virtual-desktop"></a>O que é a Área de Trabalho Virtual do Windows? 

A Área de Trabalho Virtual do Windows é um serviço de virtualização de aplicativos e área de trabalho executado na nuvem.

Estas são as tarefas que você pode realizar ao executar a Área de Trabalho Virtual do Windows no Azure:

* Configurar uma implantação do Windows 10 de várias sessões que fornece uma escalabilidade completa do Windows 10
* Virtualizar o Office 365 ProPlus e otimizá-lo para execução em cenários virtuais de vários usuários
* Fornecer áreas de trabalho virtuais do Windows 7 com Atualizações de Segurança Estendida gratuitas
* Trazer seu RDS (Serviços de Área de Trabalho Remota) existente, além de aplicativos e áreas de trabalho do Windows Server para qualquer computador
* Virtualizar aplicativos e áreas de trabalho
* Gerenciar aplicativos e áreas de trabalho do Windows 10, Windows Server e Windows 7 com uma experiência unificada de gerenciamento

## <a name="introductory-video"></a>Vídeo de introdução

Saiba mais sobre a Área de Trabalho Virtual do Windows, por que ela é única e o que há de novo neste vídeo:

<br></br><iframe src="https://www.youtube.com/embed/NQFtI3JLtaU" width="640" height="320" allowFullScreen="true" frameBorder="0"></iframe>

Para obter mais vídeos sobre a Área de Trabalho Virtual do Windows, confira [nossa playlist](https://www.youtube.com/watch?v=NQFtI3JLtaU&list=PLXtHYVsvn_b8KAKw44YUpghpD6lg-EHev).

## <a name="key-capabilities"></a>Principais recursos

Com a Área de Trabalho Virtual do Windows, você pode configurar um ambiente escalonável e flexível:

* Crie um ambiente completo de virtualização de área de trabalho em sua assinatura do Azure sem precisar executar servidores de gateway adicionais.
* Publique a quantidade de pools de host que precisar para acomodar cargas de trabalho diversificadas.
* Traga sua própria imagem para cargas de trabalho de produção ou de teste por meio da Galeria do Azure.
* Reduza os custos com recursos em pool de várias sessões. Com a nova funcionalidade de várias sessões do Windows 10 Enterprise exclusiva da Área de Trabalho Virtual do Windows e da função RDSH (Host da Sessão da Área de Trabalho Remota) no Windows Server, você pode reduzir consideravelmente o número de máquinas virtuais e a sobrecarga do sistema operacional, enquanto ainda fornece os mesmos recursos aos usuários.
* Forneça uma propriedade individual por meio de áreas de trabalho pessoais (persistentes).

Você pode implantar e gerenciar áreas de trabalho virtuais:

* Use as interfaces REST e PowerShell da Área de Trabalho Virtual do Windows para configurar os pools de host, criar grupos de aplicativos, atribuir usuários e publicar recursos.
* Publique aplicativos remotos completos de área de trabalho ou individuais por meio de um único pool de hosts, crie grupos de aplicativos individuais para diferentes conjuntos de usuários ou, até mesmo, atribua usuários a vários grupos de aplicativos para reduzir o número de imagens.
* Enquanto gerencia seu ambiente, use o acesso delegado interno para atribuir funções e coletar diagnóstico para compreender os vários erros de configuração ou de usuário.
* Use o novo serviço de Diagnóstico para solucionar problemas de erros.
* Gerencie somente a imagem e as máquinas virtuais, não a infraestrutura. Você não precisa gerenciar pessoalmente as funções da Área de Trabalho Remota, como faz com os Serviços de Área de Trabalho Remota, apenas as máquinas virtuais em sua assinatura do Azure.

Você também pode atribuir e conectar os usuários às áreas de trabalho virtuais:

* Depois de atribuídos, os usuários podem iniciar qualquer cliente da Área de Trabalho Virtual do Windows para conectar os usuários às áreas de trabalho e aos aplicativos publicados do Windows. Conecte-se em qualquer dispositivo por meio de um aplicativo nativo em seu dispositivo ou no cliente Web HTML5 da Área de Trabalho Virtual do Windows.
* Estabeleça usuários com segurança por meio de conexões reversas com o serviço, para que você nunca precise deixar as portas de entrada abertas.

## <a name="requirements"></a>Requisitos

Há alguns itens necessários para configurar a Área de Trabalho Virtual do Windows e conectar com êxito os usuários aos aplicativos e às áreas de trabalho do Windows.

Planejamos adicionar suporte aos sistemas operacionais a seguir, então verifique se você tem as [licenças apropriadas](https://azure.microsoft.com/pricing/details/virtual-desktop/) para os usuários com base na área de trabalho e nos aplicativos que você pretende implantar:

|Sistema operacional|Licença necessária|
|---|---|
|Windows 10 Enterprise de várias sessões ou Windows 10 Enterprise|Microsoft 365 E3, E5, A3, A5, F1, Business<br>Windows E3, E5, A3, A5|
|Windows 7 Enterprise |Microsoft 365 E3, E5, A3, A5, F1, Business<br>Windows E3, E5, A3, A5|
|Windows Server 2012 R2, 2016, 2019|CAL (licença de acesso para cliente) do RDS com o Software Assurance|

A infraestrutura precisa dos seguintes itens para dar suporte à Área de Trabalho Virtual do Windows:

* Um [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/)
* Um Windows Server Active Directory em sincronia com o Azure Active Directory. Você pode configurar isso com um dos seguintes:
  * Azure AD Connect (para organizações híbridas)
  * Azure AD Domain Services (para organizações híbridas ou na nuvem)
* Uma assinatura do Azure que contém uma rede virtual que contenha o Windows Server Active Directory ou que esteja conectada a ele
  
As máquinas virtuais do Azure criadas para a Área de Trabalho Virtual do Windows precisam ser:

* [Ingressadas no domínio padrão](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-comparison) ou [Ingressadas no AD híbridas](https://docs.microsoft.com/azure/active-directory/devices/hybrid-azuread-join-plan). As máquinas virtuais não podem ser ingressadas no Azure AD.
* A execução de uma das seguintes [imagens do sistema operacional compatíveis](#supported-virtual-machine-os-images).

>[!NOTE]
>Caso precise de uma assinatura do Azure, [inscreva-se em uma avaliação gratuita de um mês](https://azure.microsoft.com/free/). Se estiver usando a versão de avaliação gratuita do Azure, use o Azure AD Domain Services para manter o Windows Server Active Directory em sincronia com o Azure Active Directory.

As máquinas virtuais do Azure criadas para a Área de Trabalho Virtual do Windows precisam ter acesso às seguintes URLs:

|Endereço|Porta de saída|Finalidade|
|---|---|---|
|*.wvd.microsoft.com|Porta TCP 443|Tráfego de serviço|
|*.blob.core.windows.net|Porta TCP 443|Agente, atualizações de pilha de SXS e tráfego de Agente|
|*.core.windows.net|Porta TCP 443|Tráfego de agente|
|*.servicebus.windows.net|Porta TCP 443|Tráfego de agente|
|prod.warmpath.msftcloudes.com|Porta TCP 443|Tráfego de agente|
|catalogartifact.azureedge.net|Porta TCP 443|Azure Marketplace|
|kms.core.windows.net|Porta TCP 1688|Ativação do Windows 10|

>[!IMPORTANT]
>Abrir essas URLs é essencial para uma implantação da Área de Trabalho Virtual do Windows confiável. Não há suporte ao bloqueio do acesso a essas URLs e isso afetará a funcionalidade do serviço. Essas URLs são correspondentes apenas aos sites e recursos da Área de Trabalho Virtual do Windows e não incluem URLs para outros serviços, como o Azure AD.

>[!NOTE]
>Você precisa usar o caractere curinga (*) para URLs que envolvem tráfego de serviço. Se preferir não usar * para o tráfego relacionado ao agente, encontre as URLs sem curinga da seguinte forma:
>
>1. Registre suas máquinas virtuais no pool de hosts da Área de Trabalho Virtual do Windows.
>2. Abra o **Visualizador de Eventos** e navegue até **Windows** > **Logs de Aplicativos** e procure a ID de Evento 3702.
>3. Inclua na lista de permissões as URLs encontradas com a ID de Evento 3702. As URLs com a ID de Evento 3702 são específicas da região. Você precisará repetir o processo de inclusão na lista de permissões com as URLs relevantes para cada região em que quiser implantar suas máquinas virtuais.

A Área de Trabalho Virtual do Windows é composta por áreas de trabalho e aplicativos do Windows que você fornece aos usuários e pela solução de gerenciamento, que é hospedada pela Microsoft como um serviço no Azure. As áreas de trabalho e os aplicativos podem ser implantados em VMs (máquinas virtuais) em qualquer região do Azure. A solução de gerenciamento e os dados dessas VMs residirão nos Estados Unidos. Isso poderá resultar na transferência de dados para os Estados Unidos.

Para otimizar o desempenho, verifique se a rede atende aos seguintes requisitos:

* A latência RTT (viagem de ida e volta) da rede do cliente para a região do Azure em que os pools de host foram implantados deve ser inferior a 150 ms.
* O tráfego de rede poderá fluir para fora das fronteiras do país/região quando as VMs que hospedam os aplicativos e as áreas de trabalho se conectarem ao serviço de gerenciamento.
* Para otimizar o desempenho da rede, recomendamos que as VMs do host da sessão sejam colocadas na mesma região do Azure do serviço de gerenciamento.

## <a name="supported-remote-desktop-clients"></a>Clientes compatíveis da Área de Trabalho Remota

Os seguintes clientes da Área de Trabalho Remota são compatíveis com a Área de Trabalho Virtual do Windows:

* [Windows](connect-windows-7-and-10.md)
* [Web](connect-web.md)
* [Mac](connect-macos.md)
* [iOS](connect-ios.md)
* [Android (versão prévia)](connect-android.md)

## <a name="supported-virtual-machine-os-images"></a>Imagens compatíveis do sistema operacional da máquina virtual

A Área de Trabalho Virtual do Windows é compatível com as seguintes imagens do sistema operacional x64:

* Windows 10 Enterprise de várias sessões, versão 1809 ou posterior
* Windows 10 Enterprise, versão 1809 ou posterior
* Windows 7 Enterprise
* Windows Server 2019
* Windows Server 2016
* Windows Server 2012 R2

A Área de Trabalho Virtual do Windows não é compatível com imagens do sistema operacional x86 (32 bits), Windows 10 Enterprise N ou Windows 10 Enterprise KN.

As opções de automação e implantação disponíveis dependem do sistema operacional e da versão escolhidos, conforme mostrado na tabela a seguir: 

|Sistema operacional|Galeria de Imagens do Azure|Implantação manual de VM|Integração de modelos do Azure Resource Manager|Provisionar pools de hosts no Azure Marketplace|Atualizações do Agente da Área de Trabalho Virtual do Windows|
|--------------------------------------|:------:|:------:|:------:|:------:|:------:|
|Várias sessões do Windows 10, versão 1903|Sim|Sim|Sim|Sim|Automático|
|Várias sessões do Windows 10, versão 1809|Sim|Sim|Não|Não|Automático|
|Windows 10 Enterprise, versão 1903|Sim|Sim|Sim|Sim|Automático|
|Windows 10 Enterprise, versão 1809|Sim|Sim|Não|Não|Automático|
|Windows 7 Enterprise|Sim|Sim|Não|Não|Manual|
|Windows Server 2019|Sim|Sim|Não|Não|Automático|
|Windows Server 2016|Sim|Sim|Sim|Sim|Automático|
|Windows Server 2012 R2|Sim|Sim|Não|Não|Automático|

## <a name="next-steps"></a>Próximas etapas

Para começar, você precisará criar um locatário. Para saber mais sobre como criar um locatário, continue para o tutorial de criação de locatário.

> [!div class="nextstepaction"]
> [Criar um locatário na Área de Trabalho Virtual do Windows](tenant-setup-azure-active-directory.md)

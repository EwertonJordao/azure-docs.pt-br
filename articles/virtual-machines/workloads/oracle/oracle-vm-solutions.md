---
title: Soluções Oracle em máquinas virtuais do Azure | Microsoft Docs
description: Saiba mais sobre as configurações com suporte e as limitações das imagens de máquina virtual do Oracle em Microsoft Azure.
services: virtual-machines-linux
documentationcenter: ''
author: BorisB2015
manager: gwallace
tags: azure-resource-management
ms.assetid: ''
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 05/23/2019
ms.author: borisb
ms.openlocfilehash: 0cee7c25960d567c75a14d8ad9ef95b3e7221862
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81683426"
---
# <a name="oracle-vm-images-and-their-deployment-on-microsoft-azure"></a>Imagens de VM Oracle e sua implantação no Microsoft Azure

Este artigo aborda informações sobre soluções Oracle baseadas em imagens de máquinas virtuais publicadas pela Oracle no Azure Marketplace. Se você estiver interessado em soluções de aplicativos entre nuvem com a infraestrutura de nuvem da Oracle, consulte [soluções de aplicativos Oracle integrando Microsoft Azure e infraestrutura de nuvem da Oracle](oracle-oci-overview.md).

Para obter uma lista de imagens disponíveis no momento, execute o seguinte comando:

```azurecli-interactive
az vm image list --publisher oracle -o table --all
```

A partir de maio de 2019, as seguintes imagens estão disponíveis:

```bash
Offer                   Publisher    Sku                     Urn                                                          Version
----------------------  -----------  ----------------------  -----------------------------------------------------------  -------------
Oracle-Database-Ee      Oracle       12.1.0.2                Oracle:Oracle-Database-Ee:12.1.0.2:12.1.20170220             12.1.20170220
Oracle-Database-Ee      Oracle       12.2.0.1                Oracle:Oracle-Database-Ee:12.2.0.1:12.2.20180725             12.2.20180725
Oracle-Database-Ee      Oracle       18.3.0.0                Oracle:Oracle-Database-Ee:18.3.0.0:18.3.20181213             18.3.20181213
Oracle-Database-Se      Oracle       12.1.0.2                Oracle:Oracle-Database-Se:12.1.0.2:12.1.20170220             12.1.20170220
Oracle-Database-Se      Oracle       12.2.0.1                Oracle:Oracle-Database-Se:12.2.0.1:12.2.20180725             12.2.20180725
Oracle-Database-Se      Oracle       18.3.0.0                Oracle:Oracle-Database-Se:18.3.0.0:18.3.20181213             18.3.20181213
Oracle-Linux            Oracle       6.10                    Oracle:Oracle-Linux:6.10:6.10.20190506                       6.10.20190506
Oracle-Linux            Oracle       6.8                     Oracle:Oracle-Linux:6.8:6.8.20190506                         6.8.20190506
Oracle-Linux            Oracle       6.9                     Oracle:Oracle-Linux:6.9:6.9.20190506                         6.9.20190506
Oracle-Linux            Oracle       7.3                     Oracle:Oracle-Linux:7.3:7.3.20190506                         7.3.20190506
Oracle-Linux            Oracle       7.4                     Oracle:Oracle-Linux:7.4:7.4.20190506                         7.4.20190506
Oracle-Linux            Oracle       7.5                     Oracle:Oracle-Linux:7.5:7.5.20181207                         7.5.20181207
Oracle-Linux            Oracle       7.5                     Oracle:Oracle-Linux:7.5:7.5.20190506                         7.5.20190506
Oracle-Linux            Oracle       7.6                     Oracle:Oracle-Linux:7.6:7.6.20181207                         7.6.20181207
Oracle-Linux            Oracle       7.6                     Oracle:Oracle-Linux:7.6:7.6.20190506                         7.6.20190506
Oracle-WebLogic-Server  Oracle       Oracle-WebLogic-Server  Oracle:Oracle-WebLogic-Server:Oracle-WebLogic-Server:12.1.2  12.1.2
```

Essas imagens são consideradas "Traga sua própria licença" e como tal, você só será cobrado pelos custos de computação, armazenamento e rede incorridos na execução de uma VM.  Presume-se que você esteja devidamente licenciado para usar o software da Oracle e que tem um contrato de suporte em vigor com a Oracle. A Oracle tem garantia de mobilidade de licença do local para o Azure. Consulte a observação [Oracle e Microsoft](https://www.oracle.com/technetwork/topics/cloud/faq-1963009.html) publicada para obter detalhes sobre a mobilidade da licença. 

Pessoas também podem optar por basear suas soluções em uma imagem personalizada, elas criam imagens do zero no Azure ou fazem upload da imagem personalizada de seu ambiente local.

## <a name="oracle-database-vm-images"></a>Imagens de VM de banco de dados Oracle
O Oracle dá suporte à execução de Oracle Database 12,1 e superior Standard e Enterprise Editions no Azure em imagens de máquina virtual com base em Oracle Linux.  Para obter o melhor desempenho para cargas de trabalho de produção de Oracle Database no Azure, certifique-se de dimensionar corretamente a imagem da VM e usar SSD Premium ou SSD Ultra Managed Disks. Para obter instruções sobre como obter rapidamente um Oracle Database em funcionamento no Azure usando a imagem de VM publicada do Oracle, [Experimente o guia de início rápido do Oracle Database](oracle-database-quick-create.md).

### <a name="attached-disk-configuration-options"></a>Opções de configuração de disco anexado

Os discos anexados se baseiam no serviço de armazenamento de Blobs do Azure. Cada disco padrão pode fornecer uma velocidade máxima teórica de aproximadamente 500 IOPS (operações de entrada/saída por segundo). Nossa oferta de disco premium é preferencial para cargas de trabalho de banco de dados de alto desempenho e pode alcançar até 5000 IOPS por disco. Você pode usar um único disco se atender às suas necessidades de desempenho. No entanto, você pode melhorar o desempenho de IOPS efetivo se usar vários discos anexados, espalhar dados de banco de dados entre eles e usar o Oracle Automatic Storage Management (ASM). Veja [Visão geral do armazenamento automático da Oracle](https://www.oracle.com/technetwork/database/index-100339.html) para obter mais informações específicas do Oracle ASM. Para obter um exemplo de como instalar e configurar o Oracle ASM em uma VM do Azure para Linux, consulte o tutorial [Instalando e Configurando o Oracle Automated Storage Management](configure-oracle-asm.md) .

### <a name="shared-storage-configuration-options"></a>Opções de configuração de armazenamento compartilhado

O Azure NetApp Files foi projetado para atender aos principais requisitos de execução de cargas de trabalho de alto desempenho, como bancos de dados na nuvem, e fornece;
- Serviço de armazenamento NFS compartilhado nativo do Azure para executar cargas de trabalho Oracle por meio de cliente NFS nativo de VM ou Oracle dNFS
- Níveis de desempenho escalonáveis que refletem o intervalo real de demandas de IOPS
- Baixa latência
- Alta disponibilidade, alta durabilidade e capacidade de gerenciamento em escala, normalmente exigidas por cargas de trabalho de missão crítica corporativas (como SAP e Oracle)
- Backup e recuperação rápidos e eficientes, para atingir os SLAs de RTO e RPO mais agressivos

Esses recursos são possíveis porque Azure NetApp Files se baseia no NetApp® ONTAP® todos os sistemas flash em execução no ambiente de data center do Azure – como um serviço nativo do Azure. O resultado é uma tecnologia de armazenamento de banco de dados ideal que pode ser provisionada e consumida da mesma forma que outras opções de armazenamento do Azure. Consulte a [documentação Azure NetApp files](https://docs.microsoft.com/azure/azure-netapp-files/) para obter mais informações sobre como implantar e acessar Azure NetApp files volumes NFS. Consulte o guia de práticas [recomendadas de implantação do Oracle no Azure usando Azure NetApp files](https://www.netapp.com/us/media/tr-4780.pdf) para obter recomendações de práticas recomendadas para operar um banco de dados Oracle no Azure NetApp files.


## <a name="licensing-oracle-database--software-on-azure"></a>Licenciamento Oracle Database software & no Azure
Microsoft Azure é um ambiente de nuvem autorizado para execução de Oracle Database. A tabela de fatores básicos do Oracle não é aplicável ao licenciar bancos de dados Oracle na nuvem. Em vez disso, ao usar VMs com a tecnologia Hyper-Threading habilitada para bancos de dados Enterprise Edition, conte duas vCPUs como equivalentes a uma licença de processador Oracle se o hyperthreading estiver habilitado (conforme indicado no documento de política). Os detalhes da política podem ser encontrados [aqui](http://www.oracle.com/us/corporate/pricing/cloud-licensing-070579.pdf).
Os bancos de dados Oracle geralmente exigem mais memória e e/s. Por esse motivo, as [VMs com otimização de memória](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-memory) são recomendadas para essas cargas de trabalho. Para otimizar suas cargas de trabalho ainda mais, as [VCPUs básicas restritas](https://docs.microsoft.com/azure/virtual-machines/linux/constrained-vcpu) são recomendadas para cargas de trabalho Oracle Database que exigem memória alta, armazenamento e largura de banda de e/s, mas não uma contagem de núcleos alta.

Ao migrar o software e as cargas de trabalho do Oracle do local para o Microsoft Azure, a Oracle fornece mobilidade de licenças conforme indicado nas [perguntas frequentes sobre o Oracle no Azure](https://www.oracle.com/cloud/technologies/oracle-azure-faq.html)


## <a name="oracle-real-application-cluster-oracle-rac"></a>Oracle Real Application Cluster (Oracle RAC)
Oracle RAC (Real Application Cluster) foi projetado para atenuar a falha de um único nó em uma configuração de cluster de vários nós local. Ele se baseia em duas tecnologias locais que não são nativas em ambientes de nuvem pública de hiperescala: multicast de rede e disco compartilhado. Se sua solução de banco de dados exigir o Oracle RAC no Azure, você precisará de terceiro software de terceiros para habilitar essas tecnologias. Para obter mais informações sobre o Oracle RAC, consulte a [página FlashGrid SkyCluster](https://www.flashgrid.io/oracle-rac-in-azure/).

## <a name="high-availability-and-disaster-recovery-considerations"></a>Considerações sobre alta disponibilidade e recuperação de desastres
Ao usar bancos de dados Oracle no Azure, você é responsável por implementar uma solução de alta disponibilidade e recuperação de desastres para evitar qualquer tempo de inatividade. 

A alta disponibilidade e a recuperação de desastres para Oracle Database Enterprise Edition (sem depender do Oracle RAC) podem ser obtidas no Azure usando [Data Guard, active Data Guard](https://www.oracle.com/database/technologies/high-availability/dataguard.html)ou [Oracle GoldenGate](https://www.oracle.com/technetwork/middleware/goldengate), com dois bancos de dados em duas máquinas virtuais separadas. As duas máquinas virtuais devem estar na mesma [rede virtual](https://azure.microsoft.com/documentation/services/virtual-network/) para garantir que podem se acessar mutuamente por um endereço IP privado persistente.  Além disso, é recomendável colocar as máquinas virtuais no mesmo conjunto de disponibilidade para permitir que o Azure as coloque em domínios de falha e domínios de atualização separados. Se você quiser ter redundância geográfica, configure os dois bancos de dados para replicar entre duas regiões diferentes e conecte as duas instâncias com um gateway de VPN.

O tutorial [implementar o Oracle Data Guard no Azure](configure-oracle-dataguard.md) orienta você pelo procedimento de configuração básica no Azure.  

Com o Oracle Data Guard, a alta disponibilidade pode ser obtida com um banco de dados primário em uma máquina virtual, um banco de dados secundário (em espera) em outra máquina virtual e replicação unidirecional entre eles. O resultado é o acesso de leitura à cópia do banco de dados. Com o Oracle GoldenGate, você pode configurar a replicação bidirecional entre dois bancos de dados. Para saber como configurar uma solução de alta disponibilidade para seus bancos de dados usando essas ferramentas, veja a documentação do [Active Data Guard](https://www.oracle.com/database/technologies/high-availability/dataguard.html) e do [GoldenGate](https://docs.oracle.com/goldengate/1212/gg-winux/index.html) no site da Oracle. Se precisar de acesso de leitura-gravação à cópia do banco de dados, você poderá usar o [Oracle Active Data Guard](https://www.oracle.com/uk/products/database/options/active-data-guard/overview/index.html).

O tutorial [implementar o Oracle GoldenGate no Azure](configure-oracle-golden-gate.md) orienta você pelo procedimento de configuração básica no Azure.

Além de ter uma solução de HA e DR arquitetada no Azure, você deve ter uma estratégia de backup em vigor para restaurar seu banco de dados. O tutorial [fazer backup e recuperar um Oracle Database](oracle-backup-recovery.md) orienta você pelo procedimento básico para estabelecer um backup consistente.


## <a name="support-for-jd-edwards"></a>Suporte para JD Edwards
De acordo com a nota de suporte da Oracle, a [ID do documento 2178595,1](https://support.oracle.com/epmos/faces/DocumentDisplay?_afrLoop=573435677515785&id=2178595.1&_afrWindowMode=0&_adf.ctrl-state=o852dw7d_4), JD Edwards EnterpriseOne versões 9,2 e superiores têm suporte em **qualquer oferta de nuvem pública** que atenda aos seus específicos `Minimum Technical Requirements` (MTR).  Você precisa criar imagens personalizadas que atendam às suas especificações MTR para o sistema operacional e à compatibilidade do aplicativo de software. 


## <a name="oracle-weblogic-server-virtual-machine-images"></a>Imagens de máquina virtual do Oracle WebLogic Server

* **Há suporte para clustering apenas na Enterprise Edition.** Você está licenciado para usar o clustering WebLogic somente ao usar o Enterprise Edition do Oracle WebLogic Server. Não use clustering com a edição Oracle WebLogic Server Standard.
* **O multicast de protocolo UDP não tem suporte.**  O Azure dá suporte a unicast UDP, mas não a multicast ou difusão. O Oracle WebLogic Server é capaz de contar com os recursos unicast UDP do Azure. Para obter os melhores resultados que dependem do UDP unicast, recomendamos que o tamanho do cluster WebLogic seja mantido estático ou mantido com no máximo 10 servidores gerenciados.
* **O Oracle WebLogic Server espera que as portas públicas e privadas sejam as mesmas para o acesso T3 (por exemplo, ao usar Enterprise JavaBeans).** Considere um cenário de várias camadas em que um aplicativo de camada de serviço (EJB) está em execução em um cluster do Oracle WebLogic Server que consiste em duas ou mais VMs, em uma rede virtual chamada *SLWLS*. A camada de cliente está em uma sub-rede diferente na mesma rede virtual, executando um programa Java simples tentando chamar o EJB na camada de serviço. Como é necessário balancear a carga da camada de serviço, um ponto de extremidade público com balanceamento de carga precisa ser criado para as máquinas virtuais no cluster do Oracle WebLogic Server. Se a porta privada que você especificar for diferente da porta pública (por exemplo, 7006:7008), ocorrerá um erro como o seguinte:

       [java] javax.naming.CommunicationException [Root exception is java.net.ConnectException: t3://example.cloudapp.net:7006:

       Bootstrap to: example.cloudapp.net/138.91.142.178:7006' over: 't3' got an error or timed out]

   Isso ocorre porque para qualquer acesso de T3 remoto, o Oracle WebLogic Server espera que a porta do balanceador de carga e a porta do servidor gerenciado WebLogic sejam as mesmas. No caso anterior, o cliente está acessando a porta 7006 (a porta do balanceador de carga) e o servidor gerenciado está escutando na 7008 (a porta privada). Essa restrição se aplica somente ao acesso T3, não HTTP.

   Para evitar esse problema, use uma das seguintes alternativas:

  * Use os mesmos números de porta pública e privada para pontos de extremidade com balanceamento de carga dedicados ao acesso T3.
  * Inclua o seguinte parâmetro JVM ao iniciar o Oracle WebLogic Server:

    ```
    -Dweblogic.rjvm.enableprotocolswitch=true
    ```

Para obter informações relacionadas, consulte o artigo KB **860340.1** em <https://support.oracle.com>.

* **Limitações de balanceamento de carga e clustering dinâmico.** Suponha que você deseja usar um cluster dinâmico no Oracle WebLogic Server e expô-lo por meio de um único ponto de extremidade público com balanceamento de carga no Azure. Isso pode ser feito desde que você use um número de porta fixo para cada um dos servidores gerenciados (não atribuídos dinamicamente de um intervalo) e não inicie servidores mais gerenciados do que os computadores que o administrador está controlando. Ou seja, não há mais de um servidor gerenciado por máquina virtual). Se sua configuração resultar em mais servidores Oracle WebLogic sendo iniciados do que há máquinas virtuais (ou seja, onde várias instâncias do Oracle WebLogic Server compartilham a mesma máquina virtual), não será possível que mais de uma dessas instâncias de servidores do Oracle WebLogic se associe a um determinado número de porta. Os outros nessa máquina virtual falham.

   Se você configurar o servidor de administração para atribuir automaticamente números de porta exclusivos para os servidores gerenciados, o balanceamento de carga não será possível porque o Azure não dá suporte ao mapeamento de uma única porta pública para várias portas privadas, como seria necessário para esta configuração.
* **Várias instâncias do Oracle WebLogic Server em uma máquina virtual.** Dependendo dos requisitos da sua implantação, você pode considerar a execução de várias instâncias do Oracle WebLogic Server na mesma máquina virtual, se a máquina virtual for grande o suficiente. Por exemplo, em uma máquina virtual de tamanho médio, que contém dois núcleos, você pode optar por executar duas instâncias do Oracle WebLogic Server. No entanto, ainda recomendamos que você evite introduzir pontos únicos de falha em sua arquitetura, o que seria o caso se você usava apenas uma máquina virtual que esteja executando várias instâncias do Oracle WebLogic Server. Usar pelo menos duas máquinas virtuais pode ser uma abordagem melhor, e cada máquina virtual poderia então executar várias instâncias do Oracle WebLogic Server. Cada instância do Oracle WebLogic Server ainda pode fazer parte do mesmo cluster. No entanto, no momento não é possível usar o Azure para balancear a carga de pontos de extremidade expostos por essas implantações do Oracle WebLogic Server na mesma máquina virtual, porque o Azure Load Balancer requer que os servidores com balanceamento de carga sejam distribuídos entre máquinas virtuais exclusivas.

## <a name="oracle-jdk-virtual-machine-images"></a>Imagens de máquina virtual no JDK do Oracle
* **Atualizações mais recentes do JDK 6 e 7.**  Embora seja recomendável usar a versão mais recente com suporte público do Java (atualmente, o Java 8), o Azure também disponibiliza imagens do JDK 6 e 7. Eles são voltados a aplicativos herdados que ainda não estão prontos para serem atualizado para o JDK 8. Enquanto atualizações para imagens do JDK anteriores podem não estar disponíveis para público em geral, devido à parceria da Microsoft com a Oracle, as imagens do JDK 6 e 7 fornecidas pelo Azure devem conter uma atualização mais recente, não pública, que normalmente é oferecida pela Oracle somente para um grupo selecionado de clientes com suporte da Oracle. As novas versões das imagens do JDK ficarão disponíveis com o passar do tempo, com versões atualizadas do JDK 6 e 7.

   O JDK disponível nas imagens do JDK 6 e 7 e as máquinas virtuais e imagens derivadas delas só podem ser usados no Azure.
* **JDK de 64 bits.**  As imagens de máquina virtual do Oracle WebLogic Server e as imagens de máquina virtual do Oracle JDK fornecidas pelo Azure contêm as versões de 64 bits do Windows Server e o JDK.

## <a name="next-steps"></a>Próximas etapas
Agora você tem uma visão geral das soluções atuais da Oracle com base em imagens de máquina virtual no Microsoft Azure. A próxima etapa é implantar seu primeiro banco de dados Oracle no Azure.

> [!div class="nextstepaction"]
> [Criar um banco de dados Oracle no Azure](oracle-database-quick-create.md)

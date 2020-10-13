---
title: Executar cargas de trabalho em VMs econômicas de baixa prioridade
description: Saiba como provisionar VMs de baixa prioridade para reduzir o custo das cargas de trabalho do Lote do Azure.
author: mscurrell
ms.topic: how-to
ms.date: 09/08/2020
ms.custom: seodec18
ms.openlocfilehash: bd5b73cf55110985a2e7eecbc161c77ca6d645cb
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/09/2020
ms.locfileid: "89568448"
---
# <a name="use-low-priority-vms-with-batch"></a>Usar VMs de baixa prioridade com o Lote

O Lote do Azure oferece VMs (máquinas virtuais) de baixa prioridade para reduzir o custo das cargas de trabalho no Lote. As VMs de baixa prioridade possibilitam novos tipos de cargas de trabalho do Lote, permitindo que uma grande capacidade de computação seja usada por um preço muito baixo.

VMs de baixa prioridade tiram proveito da capacidade excedente do Azure. Quando você especifica VMs de baixa prioridade em seus pools, o Lote do Azure pode usar esse excedente quando ele estiver disponível.

A desvantagem de usar VMs de baixa prioridade é que essas VMs nem sempre podem estar disponíveis para serem alocadas ou podem ser admitidas a qualquer momento, dependendo da capacidade disponível. Por esse motivo, as VMs de baixa prioridade são mais adequadas para determinados tipos de cargas de trabalho. Use VMs de baixa prioridade para cargas de trabalho de processamento assíncronas e em lote, em que o tempo para conclusão do trabalho é flexível e o trabalho é distribuído entre várias VMs.

VMs de prioridade baixa são oferecidas a um preço consideravelmente menor em comparação com VMs dedicadas. Para ver detalhes dos preços, consulte [Preços do Lote](https://azure.microsoft.com/pricing/details/batch/).

> [!NOTE]
> [As VMs Spot](https://azure.microsoft.com/pricing/spot/) já estão disponíveis para [VMs de instância única](../virtual-machines/spot-vms.md) e [conjuntos de dimensionamento de VM](../virtual-machine-scale-sets/use-spot.md). As VMs Spot representam uma evolução nas VMs de baixa prioridade, mas diferem no fato de que o preço pode variar e um preço máximo opcional pode ser definido em sua alocação.
>
> Os pools do Lote do Azure começarão a dar suporte às VMs Spot alguns meses depois delas ficarem disponíveis, com novas versões das [ferramentas e APIs do Lote](./batch-apis-tools.md). Quando o suporte à VM Spot estiver disponível, as VMs de baixa prioridade serão preteridas - elas continuarão a ter suporte com o uso das versões atuais das APIs e ferramentas por pelo menos 12 meses, para dar tempo suficiente para a migração para as VMs Spot. 
>
> As VMs Spot não terão suporte para pools de [Configuração de Serviço de Nuvem](/rest/api/batchservice/pool/add#cloudserviceconfiguration). Para usar VMs Spot, os pools de Serviço de Nuvem terão que ser migrados para pools de [Configuração de Máquina Virtual](/rest/api/batchservice/pool/add#virtualmachineconfiguration).

## <a name="use-cases-for-low-priority-vms"></a>Casos de uso para VMs de baixa prioridade

Dadas as características das VMs de baixa prioridade, quais cargas de trabalho podem e não podem usá-las? De modo geral, cargas de trabalho de processamento em lote são uma boa opção, uma vez que os trabalhos são divididos em várias tarefas paralelas ou há muitos trabalhos que são expandidos e distribuídos entre várias VMs.

-   Para maximizar o uso da capacidade excedente no Azure, trabalhos adequados podem ser escalados horizontalmente.

-   Ocasionalmente, as VMs podem não estar disponíveis ou podem sofrer preempção, o que leva a uma redução de capacidade para os trabalhos e pode levar a repetições e interrupções de tarefas. Portanto, os trabalhos devem ser flexíveis quanto ao tempo necessário para executá-los.

-   Trabalhos com tarefas mais longas podem ser mais afetados se forem interrompidos. Se tarefas de longa duração implementarem o uso de pontos de verificação para salvar o andamento conforme forem executadas, o impacto das interrupções será reduzido. Tarefas com tempos de execução menores tendem a funcionar melhor com VMs de baixa prioridade, pois o impacto da interrupção é muito menor.

-   Trabalhos de MPI de longa duração que utilizam várias VMs não são adequados para o uso de VMs de baixa prioridade, pois uma VM que admitiu preempção pode fazer com que todo o trabalho precise ser executado novamente.

Alguns exemplos de casos de uso de processamento em lote adequados ao uso de VMs de baixa prioridade são:

-   **Desenvolvimento e teste**: Em particular, se soluções de grande escala estiverem sendo desenvolvidas, economias significativas podem ser obtidas. Todos os tipos de testes podem ser beneficiados, mas testes de carga de larga escala e testes de regressão são ótimas opções.

-   **Complementação de capacidade sob demanda**: VMs de baixa prioridade podem ser usadas para complementar VMs dedicadas regulares – quando disponíveis, os trabalhos podem ser dimensionados e, portanto, concluídos mais rapidamente para reduzir o custo; quando não estiverem disponíveis, a linha de base de VMs dedicadas permanecerá disponível.

-   **Tempo de execução do trabalho flexível**: Se houver flexibilidade quanto ao tempo necessário para concluir os trabalhos, quedas potencias na capacidade poderão ser toleradas. No entanto, com a adição de VMs de baixa prioridade, frequentemente os trabalhos são executados mais rapidamente e por um custo mais baixo.

Pools do Lote podem ser configurados para usar as VMs de baixa prioridade de algumas maneiras, dependendo da flexibilidade no tempo de execução do trabalho:

-   Máquinas virtuais de baixa prioridade só podem ser usadas em um pool. Nesse caso, o Lote recupera qualquer capacidade com preempção quando estiver disponível. Essa configuração é a maneira mais econômica de executar trabalhos, pois somente VMs de baixa prioridade são usadas.

-   VMs de baixa prioridade podem ser usadas em conjunto com uma linha de base fixa de VMs dedicadas. O número fixo de VMs dedicadas garante que sempre haja alguma capacidade para manter um trabalho em andamento.

-   Pode haver uma combinação dinâmica de VMs dedicadas e de baixa prioridade, para que as VMs de baixa prioridade, mais econômicas, sejam usadas somente quando disponíveis e as VMs dedicadas, com preço total, sejam expandidas quando necessário. Essa configuração mantém uma quantidade mínima de capacidade disponível a fim de manter os trabalhos em andamento.

## <a name="batch-support-for-low-priority-vms"></a>Suporte do Lote para VMs de baixa prioridade

O Lote do Azure fornece vários recursos que tornam mais fácil consumir e se beneficiar de VMs de baixa prioridade:

-   Pools do Lote podem conter VMs dedicadas e VMs de baixa prioridade. O número de cada tipo de VM pode ser especificado quando o pool é criado e pode ser alterado a qualquer momento para um pool existente usando a operação de redimensionamento explícito ou o dimensionamento automático. O envio de trabalhos e tarefas pode permanecer inalterado, independentemente dos tipos de VM no pool. Também é possível configurar um pool a fim de usar completamente as VMs de baixa prioridade para executar trabalhos da forma mais econômica possível, mas use VMs dedicadas se a capacidade cair para baixo do limite mínimo, a fim de manter os trabalhos em execução.

-   Pools do Lote buscam automaticamente usar o número alvo de VMs de baixa prioridade. Se as VMs sofrerem preempção, o Lote tentará substituir a capacidade perdida e retornar ao valor alvo.

-   Quando as tarefas são interrompidas, o Lote esse status e as coloca automaticamente em fila para nova execução.

-   VMs de baixa prioridade têm uma cota de vCPU separada que difere das VMs dedicadas. 
    A cotação para VMs de baixa prioridade é maior do que a das VMs dedicadas, pois as VMs de baixa prioridade custam menos. Para saber mais, confira [Limites e cotas do serviço de Lote](batch-quota-limit.md#resource-quotas).    

> [!NOTE]
> Atualmente, as VMs de baixa prioridade não têm suporte para contas do Lote criadas no [modo de assinatura de usuário](accounts.md).

## <a name="create-and-update-pools"></a>Criar e atualizar pools

Um pool do Lote pode conter VMs dedicadas e de baixa prioridade (também conhecidas como nós de computação). É possível definir o número alvo de nós de computação para VMs dedicadas e de baixa prioridade. O número alvo de nós especifica o número de VMs que você deseja ter no pool.

Por exemplo, para criar um pool usando VMs do serviço de nuvem do Azure tendo como valor alvo 5 VMs dedicadas e 20 VMs de baixa prioridade:

```csharp
CloudPool pool = batchClient.PoolOperations.CreatePool(
    poolId: "cspool",
    targetDedicatedComputeNodes: 5,
    targetLowPriorityComputeNodes: 20,
    virtualMachineSize: "Standard_D2_v2",
    cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "5") // WS 2016
);
```

Para criar um pool usando máquinas virtuais do Azure (neste caso, VMs Linux) tendo como valor alvo 5 VMs dedicadas e 20 VMs de baixa prioridade:

```csharp
ImageReference imageRef = new ImageReference(
    publisher: "Canonical",
    offer: "UbuntuServer",
    sku: "16.04-LTS",
    version: "latest");

// Create the pool
VirtualMachineConfiguration virtualMachineConfiguration =
    new VirtualMachineConfiguration("batch.node.ubuntu 16.04", imageRef);

pool = batchClient.PoolOperations.CreatePool(
    poolId: "vmpool",
    targetDedicatedComputeNodes: 5,
    targetLowPriorityComputeNodes: 20,
    virtualMachineSize: "Standard_D2_v2",
    virtualMachineConfiguration: virtualMachineConfiguration);
```

É possível obter o número atual de nós para VMs dedicadas e de baixa prioridade:

```csharp
int? numDedicated = pool1.CurrentDedicatedComputeNodes;
int? numLowPri = pool1.CurrentLowPriorityComputeNodes;
```

Os nós do pool têm uma propriedade para indicar se o nó é uma VM dedicada ou de baixa prioridade:

```csharp
bool? isNodeDedicated = poolNode.IsDedicated;
```

Para pools de configuração de máquina virtual, quando um ou mais nós são preempçãos, uma operação listar nós no pool ainda retorna esses nós. O número atual de nós de baixa prioridade permanece inalterado, mas esses nós têm seu estado definido como **Admitiu preempção**. O Lote tenta localizar VMs substitutas e, se for bem-sucedido, os nós passarão pelos estados **Criando** e **Iniciando** antes de ficarem disponíveis para a execução da tarefa, assim como ocorreria com novos nós.

## <a name="scale-a-pool-containing-low-priority-vms"></a>Dimensionar um pool que contém VMs de baixa prioridade

Assim como acontece com pools que consistem somente em VMs dedicadas, é possível dimensionar um pool que contém VMs de baixa prioridade chamando o método Redimensionar ou usando o dimensionamento automático.

A operação de redimensionamento do pool usa um segundo parâmetro opcional que atualiza o valor de **targetLowPriorityNodes**:

```csharp
pool.Resize(targetDedicatedComputeNodes: 0, targetLowPriorityComputeNodes: 25);
```

A fórmula de dimensionamento automático do pool dá suporte a VMs de baixa prioridade, da seguinte maneira:

-   Você pode obter ou definir o valor da variável definida pelo serviço **$TargetLowPriorityNodes**.

-   Você pode obter o valor da variável definida pelo serviço **$CurrentLowPriorityNodes**.

-   Você pode obter o valor da variável definida pelo serviço **$PreemptedNodeCount**. 
    Essa variável retorna o número de nós com estado "admitiu preempção" e permite que você expanda ou reduza o número de nós dedicados, dependendo do número de nós que admitiram preempção e que estão indisponíveis.

## <a name="jobs-and-tasks"></a>Trabalhos e tarefas

Trabalhos e tarefas exigem pouca configuração para nós de baixa prioridade; o único suporte é o seguinte:

-   A propriedade JobManagerTask de um trabalho tem uma nova propriedade, **AllowLowPriorityNode**. 
    Quando essa propriedade tiver o valor "true", a tarefa do gerenciador de trabalho poderá ser agendada em um nó dedicado ou de baixa prioridade. Se essa propriedade tiver o valor false, a tarefa do gerenciador de trabalho será agendada apenas para um nó dedicado.

-   Um [variável de ambiente](batch-compute-node-environment-variables.md) está disponível para um aplicativo de tarefas para que ele possa determinar se ela está em execução em um nó de baixa prioridade ou dedicado. A variável de ambiente é AZ_BATCH_NODE_IS_DEDICATED.

## <a name="handling-preemption"></a>Tratamento de preempções

Ocasionalmente, as VMs podem sofrer preempção. Quando isso acontece, as tarefas que estavam em execução nas VMs de nó admitidas são recolocadas na fila e executadas novamente.

Para pools de configuração de máquina virtual, o lote também faz o seguinte:

-   As VMs que sofreram preempção têm seu estado atualizado para **Admitiu Preempção**.
-   A VM é excluída efetivamente, causando a perda de todos os dados armazenados localmente na VM.
-   O pool tentará continuamente alcançar o número alvo de nós de baixa prioridade disponíveis. Quando a capacidade de substituição for encontrada, os nós manterão suas IDs, mas serão reinicializados, passando pelos estados **Criando** e **Inicial** antes que fiquem disponíveis para o agendamento de tarefas.
-   Contagens de preempção estão disponíveis como uma métrica no Portal do Azure.

## <a name="metrics"></a>Métricas

Novas métricas estão disponíveis no [Portal do Azure](https://portal.azure.com) para nós de baixa prioridade. Essas métricas são:

- Contagem de nós de baixa prioridade
- Contagem de núcleos de baixa prioridade
- Contagem de nós com preempção

Para exibir as métricas no Portal do Azure:

1. Navegue até sua conta do Lote no portal e exiba as configurações de sua conta do Lote.
2. Selecione **Métricas** na seção **Monitoramento**.
3. Selecione as métricas desejadas na lista **Métricas Disponíveis**.

![Captura de tela mostrando a seleção de métrica para nós de baixa prioridade.](media/batch-low-pri-vms/low-pri-metrics.png)

## <a name="next-steps"></a>Próximas etapas

- Saiba mais sobre o [Fluxo de trabalho e os recursos primários do serviço de lote](batch-service-workflow-features.md) como os pools, nós, trabalhos e tarefas.
- Saiba mais sobre as [Ferramentas e APIs do Lote](batch-apis-tools.md) disponíveis para a criação de soluções do Lote.
- Comece a planejar a mudança das VMs de baixa prioridade para as VMs Spot. Se você usa VMs de baixa prioridade com pools de **configuração de Serviço de Nuvem**, planeje a mudança para pools de **configuração de Máquina Virtual**.

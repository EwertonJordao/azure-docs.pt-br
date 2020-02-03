---
title: Usar o dimensionador automático de cluster no AKS (Serviço de Kubernetes do Azure)
description: Saiba como usar o dimensionador automático de cluster para dimensionar automaticamente seu cluster e atender às demandas de aplicativo em um cluster do AKS (Serviço de Kubernetes do Azure).
services: container-service
author: mlearned
ms.service: container-service
ms.topic: article
ms.date: 07/18/2019
ms.author: mlearned
ms.openlocfilehash: 033cf88e29ba4a9f7ce9397fe216f7380e70be07
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76713390"
---
# <a name="automatically-scale-a-cluster-to-meet-application-demands-on-azure-kubernetes-service-aks"></a>Dimensionar automaticamente um cluster para atender às demandas de aplicativo no AKS (Serviço de Kubernetes do Azure)

Para se manter atualizado com as demandas de aplicativo no AKS (Serviço de Kubernetes do Azure), talvez você precise ajustar o número de nós que executam as cargas de trabalho. O componente de dimensionador automático de cluster pode inspecionar os pods no cluster que não podem ser agendados devido a restrições de recursos. Quando forem detectados problemas, o número de nós em um pool de nós será aumentado para atender à demanda do aplicativo. Os nós também são regularmente verificados quanto à falta de pods em execução, com o número de nós diminuído posteriormente conforme necessário. Essa capacidade de escalar ou reduzir verticalmente o número de nós no cluster do AKS permite a execução de um cluster eficiente e econômico.

Este artigo mostra como habilitar e gerenciar o dimensionador automático de cluster em um cluster do AKS. 

## <a name="before-you-begin"></a>Antes de começar

Este artigo requer que você esteja executando o CLI do Azure versão 2.0.76 ou posterior. Execute `az --version` para encontrar a versão. Se você precisa instalar ou atualizar, consulte [Instalar a CLI do Azure][azure-cli-install].

## <a name="limitations"></a>Limitações

As seguintes limitações se aplicam quando você cria e gerencia clusters AKS que usam o dimensionamento de clusters:

* O complemento de roteamento de aplicativo HTTP não pode ser usado.

## <a name="about-the-cluster-autoscaler"></a>Sobre o dimensionador automático de cluster

Para se ajustar às mudanças nas demandas de aplicativo, como entre a jornada de trabalho e a noite ou em um fim de semana, os clusters geralmente precisam de uma maneira de serem dimensionados automaticamente. Os clusters do AKS podem ser dimensionados de uma das duas maneiras:

* O **dimensionador automático de cluster** inspeciona os pods que não podem ser agendados em nós devido a restrições de recursos. Em seguida, o cluster aumenta automaticamente o número de nós.
* O **dimensionador automático de pod horizontal** usa o Servidor de Métricas em um cluster do Kubernetes para monitorar a demanda de recursos dos pods. Se um aplicativo precisar de mais recursos, o número de pods será aumentado automaticamente para atender à demanda.

![O dimensionador automático de cluster e o dimensionador automático de pod horizontal geralmente trabalham juntos para dar suporte às demandas necessárias de aplicativo](media/autoscaler/cluster-autoscaler.png)

Tanto o Pod horizontal como o dimensionamento automática de clusters e o autoescalar podem diminuir o número de pods e nós conforme necessário. O dimensionador automático de cluster diminui o número de nós quando a capacidade não é utilizada por um período. Os pods em um nó a serem removidos pelo dimensionador automático de cluster são agendados com segurança em outro lugar no cluster. O dimensionador automático de cluster poderá não conseguir fazer a redução vertical se os pods não puderem ser movidos, como nas seguintes situações:

* Um pod é criado diretamente e não é apoiado por um objeto de controlador, como uma implantação ou um conjunto de réplicas.
* Um PDB (orçamento de interrupção de pod) é muito restritivo e não permite que o número de pods fique abaixo de determinado limite.
* Um pod usa seletores de nó ou uma antiafinidade que não pode ser cumprida se ele está agendado em outro nó.

Para obter mais informações sobre como o dimensionador de cluster pode não ser capaz de reduzir verticalmente, consulte [quais tipos de pods podem impedir que o dimensionamento de um cluster remova um nó?][autoscaler-scaledown]

O dimensionador automático de cluster usa parâmetros de inicialização para itens como intervalos de tempo entre eventos de escala e limites de recursos. Esses parâmetros são definidos pela plataforma Azure e atualmente não são expostos para seu ajuste. Para obter mais informações sobre quais parâmetros são usados pelo dimensionador de cluster, consulte [quais são os parâmetros de dimensionamento de clusters?][autoscaler-parameters].

O cluster e os autodimensionadores de Pod horizontal podem trabalhar juntos e, muitas vezes, são implantados em um cluster. Quando combinados, o dimensionador automático de pod horizontal se concentra na execução do número de pods necessários para atender à demanda de aplicativo. O dimensionador automático de cluster se concentra na execução do número de nós necessários para dar suporte aos pods agendados.

> [!NOTE]
> O dimensionamento manual é desabilitado quando o dimensionador automático de cluster é usado. Deixe o dimensionador automático de cluster determinar o número de nós necessários. Caso deseje dimensionar o cluster manualmente, [desabilite o dimensionador automático de cluster](#disable-the-cluster-autoscaler).

## <a name="create-an-aks-cluster-and-enable-the-cluster-autoscaler"></a>Criar um cluster do AKS e habilitar o dimensionador automático de cluster

Se você precisar criar um cluster AKS, use o comando [AZ AKs Create][az-aks-create] . Para habilitar e configurar o cluster de dimensionamento autoescalar no pool de nós para o cluster, use o parâmetro *--Enable-cluster-AutoScaler* e especifique um nó *--min-Count* e *--Max-Count*.

> [!IMPORTANT]
> O dimensionamento automático do cluster é um componente de Kubernetes. Embora o cluster do AKS use um conjunto de dimensionamento de máquinas virtuais para os nós, não habilite ou edite manualmente as configurações de dimensionamento automático do conjunto de dimensionamento no portal do Azure ou usando a CLI do Azure. Permita que o dimensionador automático do cluster de Kubernetes gerencie as configurações de dimensionamento necessárias. Para obter mais informações, consulte posso [modificar os recursos do AKS no grupo de recursos do nó?](faq.md#can-i-modify-tags-and-other-properties-of-the-aks-resources-in-the-node-resource-group)

O exemplo a seguir cria um cluster AKS com um único pool de nós apoiado por um conjunto de dimensionamento de máquinas virtuais. Ele também habilita o dimensionador do cluster em conjunto de nós para o cluster e define um mínimo de *1* e o máximo de *3* nós:

```azurecli-interactive
# First create a resource group
az group create --name myResourceGroup --location eastus

# Now create the AKS cluster and enable the cluster autoscaler
az aks create \
  --resource-group myResourceGroup \
  --name myAKSCluster \
  --node-count 1 \
  --vm-set-type VirtualMachineScaleSets \
  --load-balancer-sku standard \
  --enable-cluster-autoscaler \
  --min-count 1 \
  --max-count 3
```

São necessários alguns minutos para criar o cluster e definir as configurações do dimensionador automático de cluster.

## <a name="change-the-cluster-autoscaler-settings"></a>Alterar as configurações do dimensionador automático de cluster

> [!IMPORTANT]
> Se você tiver vários pools de nós no cluster AKS, pule para a [seção dimensionamento automático com vários pools de agentes](#use-the-cluster-autoscaler-with-multiple-node-pools-enabled). Clusters com vários pools de agentes exigem o uso do conjunto de comandos `az aks nodepool` para alterar propriedades específicas do pool de nós em vez de `az aks`.

Na etapa anterior para criar um cluster AKS ou atualizar um pool de nós existente, a contagem de nós mínimos do cluster de dimensionamento foi definida como *1*e a contagem máxima de nós foi definida como *3*. Conforme as demandas de aplicativo mudam, você pode precisar ajustar a contagem de nós do dimensionador automático de cluster.

Para alterar a contagem de nós, use o comando [AZ AKs Update][az-aks-update] .

```azurecli-interactive
az aks update \
  --resource-group myResourceGroup \
  --name myAKSCluster \
  --update-cluster-autoscaler \
  --min-count 1 \
  --max-count 5
```

O exemplo acima atualiza o conjunto de dimensionamento de clusters em um único pool de nós em *myAKSCluster* para um mínimo de *1* e o máximo de *5* nós.

> [!NOTE]
> Você não pode definir uma contagem de nós mínima mais alta do que o atualmente definido para o pool de nós. Por exemplo, se atualmente você tiver uma contagem mínima definida como *1*, não poderá atualizar a contagem mínima para *3*.

Monitore o desempenho de seus aplicativos e serviços e ajuste as contagens de nós do dimensionador automático de cluster para que correspondam ao desempenho necessário.

## <a name="disable-the-cluster-autoscaler"></a>Desabilitar o dimensionador automático de cluster

Se você não quiser mais usar o dimensionamento automática do cluster, poderá desabilitá-lo usando o comando [AZ AKs Update][az-aks-update] , especificando o parâmetro *--Disable-cluster-AutoScaler* . Os nós não são removidos quando o dimensionador automático de cluster é desabilitado.

```azurecli-interactive
az aks update \
  --resource-group myResourceGroup \
  --name myAKSCluster \
  --disable-cluster-autoscaler
```

Você pode dimensionar manualmente o cluster depois de desabilitar o dimensionamento de cluster automaticamente usando o comando [AZ AKs Scale][az-aks-scale] . Se você usar a escalabilidade horizontal do pod, esse recurso continuará a ser executado com o dimensionamento em autoescala do cluster desabilitado, mas o pods poderá não ser agendado se todos os recursos do nó estiverem em uso.

## <a name="re-enable-a-disabled-cluster-autoscaler"></a>Reabilitar o dimensionador de cluster desabilitado

Se desejar reabilitar o dimensionador automática do cluster em um cluster existente, você poderá habilitá-lo novamente usando o comando [AZ AKs Update][az-aks-update] , especificando os parâmetros *--Enable-cluster-autoscaleer*, *--min-Count*e *--Max-Count* .

## <a name="retrieve-cluster-autoscaler-logs-and-status"></a>Recuperar status e logs de dimensionamento de clusters

Para diagnosticar e depurar eventos de autoescalar, os logs e o status podem ser recuperados do complemento de dimensionamento.

O AKS gerencia o cluster de dimensionamento em seu nome e o executa no plano de controle gerenciado. Os logs do nó mestre devem ser configurados para serem exibidos como resultado.

Para configurar os logs a serem enviados por push do cluster de dimensionamento para Log Analytics siga estas etapas.

1. Configurar uma regra para logs de diagnóstico para envio por push de cluster-logs de dimensionamento de AutoEscala para Log Analytics. [As instruções são detalhadas aqui](https://docs.microsoft.com/azure/aks/view-master-logs#enable-diagnostics-logs), certifique-se de marcar a caixa para `cluster-autoscaler` ao selecionar opções para "logs".
1. Clique na seção "logs" no cluster por meio do portal do Azure.
1. Insira a seguinte consulta de exemplo em Log Analytics:

```
AzureDiagnostics
| where Category == "cluster-autoscaler"
```

Você deve ver logs semelhantes aos seguintes retornados, desde que haja logs a serem recuperados.

![Logs de Log Analytics](media/autoscaler/autoscaler-logs.png)

O autodimensionador de cluster também irá gravar o status de integridade em um ConfigMap chamado `cluster-autoscaler-status`. Para recuperar esses logs, execute o comando `kubectl` a seguir. Um status de integridade será relatado para cada pool de nós configurado com o dimensionador de cluster.

```
kubectl get configmap -n kube-system cluster-autoscaler-status -o yaml
```

Para saber mais sobre o que é registrado a partir do dimensionador de escalabilidade, leia as perguntas frequentes no [projeto GitHub kubernetes/AutoScaler](https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md#ca-doesnt-work-but-it-used-to-work-yesterday-why).

## <a name="use-the-cluster-autoscaler-with-multiple-node-pools-enabled"></a>Usar o dimensionamento de autoescalar do cluster com vários pools de nós habilitados

O conjunto de dimensionamento de clusters pode ser usado junto com [vários pools de nós](use-multiple-node-pools.md) habilitados. Siga esse documento para saber como habilitar vários pools de nó e adicionar pools de nós adicionais a um cluster existente. Ao usar os dois recursos juntos, você habilita o dimensionador automático do cluster em cada pool de nós individual no cluster e pode passar regras exclusivas de dimensionamento automático para cada um deles.

O comando abaixo pressupõe que você seguiu as [instruções iniciais](#create-an-aks-cluster-and-enable-the-cluster-autoscaler) anteriormente neste documento e deseja atualizar a contagem máxima de um pool de nós existente de *3* para *5*. Use o comando [AZ AKs nodepool Update][az-aks-nodepool-update] para atualizar as configurações de um pool de nós existente.

```azurecli-interactive
az aks nodepool update \
  --resource-group myResourceGroup \
  --cluster-name myAKSCluster \
  --name nodepool1 \
  --update-cluster-autoscaler \
  --min-count 1 \
  --max-count 5
```

O dimensionador automática de cluster pode ser desabilitado com [AZ AKs nodepool Update][az-aks-nodepool-update] e passando o parâmetro `--disable-cluster-autoscaler`.

```azurecli-interactive
az aks nodepool update \
  --resource-group myResourceGroup \
  --cluster-name myAKSCluster \
  --name nodepool1 \
  --disable-cluster-autoscaler
```

Se desejar reabilitar o dimensionador automática do cluster em um cluster existente, você poderá habilitá-lo novamente usando o comando [AZ AKs nodepool Update][az-aks-nodepool-update] , especificando os parâmetros *--Enable-cluster-autoscaleer*, *--min-Count*e *--Max-Count* .

## <a name="next-steps"></a>{1&gt;{2&gt;Próximas etapas&lt;2}&lt;1}

Este artigo mostrou como dimensionar automaticamente o número de nós do AKS. Você também pode usar o dimensionador automático de pod horizontal para ajustar automaticamente o número de pods que executam o aplicativo. Para obter as etapas sobre como usar a escalabilidade horizontal Pod, consulte [dimensionar aplicativos em AKs][aks-scale-apps].

<!-- LINKS - internal -->
[aks-upgrade]: upgrade-cluster.md
[azure-cli-install]: /cli/azure/install-azure-cli
[az-aks-show]: /cli/azure/aks#az-aks-show
[az-extension-add]: /cli/azure/extension#az-extension-add
[aks-scale-apps]: tutorial-kubernetes-scale.md
[az-aks-create]: /cli/azure/aks#az-aks-create
[az-aks-scale]: /cli/azure/aks#az-aks-scale
[az-feature-register]: /cli/azure/feature#az-feature-register
[az-feature-list]: /cli/azure/feature#az-feature-list
[az-provider-register]: /cli/azure/provider#az-provider-register
[aks-support-policies]: support-policies.md
[aks-faq]: faq.md
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-extension-update]: /cli/azure/extension#az-extension-update

<!-- LINKS - external -->
[az-aks-update]: https://github.com/Azure/azure-cli-extensions/tree/master/src/aks-preview
[az-aks-nodepool-update]: https://github.com/Azure/azure-cli-extensions/tree/master/src/aks-preview#enable-cluster-auto-scaler-for-a-node-pool
[autoscaler-scaledown]: https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md#what-types-of-pods-can-prevent-ca-from-removing-a-node
[autoscaler-parameters]: https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md#what-are-the-parameters-to-ca

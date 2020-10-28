---
title: Como excluir um cluster HDInsight - Azure
description: Informações sobre as várias maneiras que você pode excluir um cluster do Azure HDInsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: how-to
ms.custom: H1Hack27Feb2017,hdinsightactive, devx-track-azurecli
ms.date: 11/29/2019
ms.openlocfilehash: 49a43f821a159af6944abb9509c24a8dd081be43
ms.sourcegitcommit: 8c7f47cc301ca07e7901d95b5fb81f08e6577550
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/27/2020
ms.locfileid: "92748808"
---
# <a name="delete-an-hdinsight-cluster-using-your-browser-powershell-or-the-azure-cli"></a>Excluir um cluster HDInsight usando o navegador, o PowerShell ou a CLI do Azure

A cobrança do cluster HDInsight começa quando um cluster é criado e para quando o cluster é excluído. A cobrança é proporcional por minuto, portanto, você sempre deve excluir o cluster quando ele não estiver mais em uso. Neste documento, você aprende a excluir um cluster usando o [portal do Azure](https://portal.azure.com), Azure PowerShell o [módulo Az](/powershell/azure/)e o [CLI do Azure](/cli/azure/).

> [!IMPORTANT]  
> A exclusão de um cluster HDInsight não exclui as contas do Armazenamento do Azure ou Data Lake Storage associadas ao cluster. Você pode reutilizar os dados armazenados nesses serviços no futuro.

## <a name="azure-portal"></a>Portal do Azure

1. Entre no [portal do Azure](https://portal.azure.com).

2. No menu à esquerda, navegue até **todos os serviços**  >  **Analytics**  >  **clusters HDInsight** de análise e selecione o cluster.

3. No modo de exibição padrão, selecione o ícone **excluir** . Siga o prompt para excluir o cluster.

    ![Botão excluir cluster do HDInsight](./media/hdinsight-delete-cluster/hdinsight-delete-cluster.png)

## <a name="azure-powershell"></a>Azure PowerShell

Substitua `CLUSTERNAME` pelo nome do seu cluster HDInsight no código a seguir. Em um prompt do PowerShell, digite o seguinte comando para excluir o cluster:

```powershell
Remove-AzHDInsightCluster -ClusterName CLUSTERNAME
```

## <a name="azure-cli"></a>CLI do Azure

Substitua `CLUSTERNAME` pelo nome do seu cluster HDInsight e `RESOURCEGROUP` pelo nome do seu grupo de recursos no código a seguir.  Em um prompt de comando, digite o seguinte para excluir o cluster:

```azurecli
az hdinsight delete --name CLUSTERNAME --resource-group RESOURCEGROUP
```

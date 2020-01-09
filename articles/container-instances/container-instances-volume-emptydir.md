---
title: Montar o volume emptyDir para o grupo de contêineres
description: Saiba como montar um volume emptyDir para compartilhar dados entre os contêineres em um grupo de contêineres em Instâncias de Contêiner do Azure
ms.topic: article
ms.date: 02/08/2018
ms.openlocfilehash: 955423b685ebb3979271c7c2dc7e835a16100c2b
ms.sourcegitcommit: ec2eacbe5d3ac7878515092290722c41143f151d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/31/2019
ms.locfileid: "75552450"
---
# <a name="mount-an-emptydir-volume-in-azure-container-instances"></a>Montar um volume emptyDir em Instâncias de Contêiner do Azure

Saiba como montar um volume *emptyDir* para compartilhar dados entre os contêineres em um grupo de contêineres em Instâncias de Contêiner do Azure. Use volumes *emptyDir* como caches efêmeros para suas cargas de trabalho em contêineres.

> [!NOTE]
> A montagem de um volume *emptyDir* está atualmente restrita a contêineres do Linux. Enquanto estamos trabalhando para trazer todos os recursos para contêineres do Windows, você pode encontrar as diferenças da plataforma atual na [visão geral](container-instances-overview.md#linux-and-windows-containers).

## <a name="emptydir-volume"></a>Volume emptyDir

O volume *emptyDir* fornece um diretório gravável acessível para cada contêiner em um grupo de contêineres. Os contêineres no grupo podem ler e gravar os mesmos arquivos no volume, e ele pode ser montado usando-se os mesmos caminhos ou caminhos diferentes em cada contêiner.

Alguns exemplos usam para um volume *emptyDir*:

* Espaço transitório
* Ponto de verificação durante tarefas de longa execução
* Armazenar dados recuperados por um contêiner secundário e fornecidos por um contêiner de aplicativos

Os dados em um volume *emptyDir* são mantidos mesmo após falhas de contêiner. Os contêineres reiniciados, entretanto, não têm a garantia de manter os dados em um volume *emptyDir*. Se você parar um grupo de contêineres, o volume *emptyDir* não será persistido.

## <a name="mount-an-emptydir-volume"></a>Montar um volume emptyDir

Para montar um volume emptyDir em uma instância de contêiner, você deve implantar usando um [Modelo do Azure Resource Manager](/azure/templates/microsoft.containerinstance/containergroups).

Primeiro, popule a matriz `volumes` na seção `properties` do grupo de contêineres do modelo. Em seguida, para cada contêiner do grupo de contêineres no qual você deseja montar o volume *emptyDir*, popule a matriz `volumeMounts` na seção `properties` da definição de contêiner.

Por exemplo, o modelo do Resource Manager a seguir cria um grupo de contêineres que consiste em dois contêineres, cada um montando o volume *emptyDir*:

<!-- https://github.com/Azure/azure-docs-json-samples/blob/master/container-instances/aci-deploy-volume-emptydir.json -->
[!code-json[volume-emptydir](~/azure-docs-json-samples/container-instances/aci-deploy-volume-emptydir.json)]

Para ver um exemplo de implantação de instância de contêiner com um modelo do Azure Resource Manager, consulte [Implantar grupos com vários contêineres em Instâncias de Contêiner do Azure](container-instances-multi-container-group.md).

## <a name="next-steps"></a>Próximos passos

Saiba como montar outros tipos de volume em Instâncias de Contêiner do Azure:

* [Montar um compartilhamento de arquivos do Azure em Instâncias de Contêiner do Azure](container-instances-volume-azure-files.md)
* [Montar um volume gitRepo em Instâncias de Contêiner do Azure](container-instances-volume-gitrepo.md)
* [Montar um volume secreto em Instâncias de Contêiner do Azure](container-instances-volume-secret.md)
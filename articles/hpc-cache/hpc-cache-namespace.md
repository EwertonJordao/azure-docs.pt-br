---
title: Criar um Azure HPC Cache
description: Como criar uma instância do Azure HPC Cache
author: ekpgh
ms.service: hpc-cache
ms.topic: conceptual
ms.date: 10/30/2019
ms.author: rohogue
ms.openlocfilehash: aaa939051a1aeafdb0650119772fc7214506aa8d
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2020
ms.locfileid: "73582189"
---
# <a name="plan-the-aggregated-namespace"></a>Planejar o namespace agregado

O cache HPC do Azure permite que os clientes acessem uma variedade de sistemas de armazenamento por meio de um namespace virtual que oculta os detalhes do sistema de armazenamento de back-end.

Ao adicionar um destino de armazenamento, você define o caminho do arquivo voltado para o cliente. Os computadores cliente montam esse caminho de arquivo e podem fazer solicitações de leitura de arquivo para o cache em vez de montar o sistema de armazenamento diretamente.

Como o cache do HPC do Azure gerencia esse sistema de arquivos virtual, você pode alterar o destino de armazenamento sem alterar o caminho voltado para o cliente. Por exemplo, você pode substituir um sistema de armazenamento de hardware pelo armazenamento em nuvem sem a necessidade de reescrever procedimentos voltados para o cliente.

## <a name="aggregated-namespace-example"></a>Exemplo de namespace agregado

Planeje o namespace agregado para que os computadores cliente possam acessar convenientemente as informações de que precisam e para que os administradores e engenheiros de fluxo de trabalho possam distinguir facilmente os caminhos.

Por exemplo, considere um sistema em que uma instância de cache do Azure HPC está sendo usada para processar dados armazenados no blob do Azure. A análise requer arquivos de modelo que são armazenados em um datacenter local.

Os dados do modelo são armazenados em um datacenter e as informações necessárias para esse trabalho são armazenadas nesses subdiretórios:

    /goldline/templates/acme2017/sku798
    /goldline/templates/acme2017/sku980 

O sistema de armazenamento do datacenter expõe essas exportações:

    /
    /goldline
    /goldline/templates

Os dados a serem analisados foram copiados para um contêiner de armazenamento de BLOBs do Azure denominado "SourceCollection" usando o [utilitário CLFSLoad](hpc-cache-ingest.md#pre-load-data-in-blob-storage-with-clfsload).

Para permitir acesso fácil por meio do cache, considere a criação de destinos de armazenamento com estes caminhos de namespace virtual:

| Sistema de armazenamento de back-end <br/> (Caminho do arquivo NFS ou contêiner de BLOB) | Caminho do namespace virtual |
|-----------------------------------------|------------------------|
| /goldline/templates/acme2017/sku798     | /templates/sku798      |
| /goldline/templates/acme2017/sku980     | /templates/sku980      |
| SourceCollection                        | /Source               |

Um destino de armazenamento NFS pode ter vários caminhos de namespace virtual, desde que cada um faça referência a um caminho de exportação exclusivo.

Como os caminhos de origem do NFS são subdiretórios da mesma exportação, você precisará definir vários caminhos de namespace do mesmo destino de armazenamento.

| Nome de host do destino de armazenamento  | Caminho de exportação do NFS      | Caminho do subdiretório | Caminho do namespace    |
|--------------------------|----------------------|-------------------|-------------------|
| *Endereço IP ou nome do host* | /goldline/templates  | acme2017/sku798   | /templates/sku798 |
| *Endereço IP ou nome do host* | /goldline/templates  | acme2017/sku980   | /templates/sku980 |

Um aplicativo cliente pode montar o cache e acessar facilmente os caminhos ``/source``de arquivo de namespace agregados, ``/templates/sku980`` ``/templates/sku798``e.

## <a name="next-steps"></a>Próximas etapas

Depois de decidir como configurar seu sistema de arquivos virtual, [crie destinos de armazenamento](hpc-cache-add-storage.md) para mapear seu armazenamento de back-end para os caminhos de arquivos virtuais voltados para o cliente.

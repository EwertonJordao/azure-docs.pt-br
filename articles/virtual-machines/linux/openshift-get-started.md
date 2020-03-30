---
title: OpenShift no Azure visão geral
description: Uma visão geral de OpenShift no Azure.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: haroldwongms
manager: mdotson
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/7/2019
ms.author: haroldw
ms.openlocfilehash: 021ebe010a27fa155de861121e1972466c800f4a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74035418"
---
# <a name="openshift-in-azure"></a>OpenShift no Azure

O OpenShift é uma plataforma de aplicativo de contêiner aberta e extensível que traz docker e Kubernetes para a empresa.  

O OpenShift inclui Kubernetes para gerenciamento e orquestração do contêiner. Ele adiciona ferramentas concentradas no desenvolvedor e em operações que permitem:

- Desenvolvimento rápido de aplicativos.
- Implantação e dimensionamento fáceis.
- Manutenção de ciclo de vida de longo prazo para equipes e aplicativos.

Existem várias versões do OpenShift disponíveis.  Dessas versões, apenas duas estão disponíveis hoje para os clientes implantarem no Azure: OpenShift Container Platform e OKD (anteriormente OpenShift Origin).

## <a name="azure-red-hat-openshift"></a>Red Hat OpenShift no Azure

O Microsoft Azure Red Hat OpenShift é uma oferta totalmente gerenciada do OpenShift em execução no Azure. Esse serviço é gerenciado e suportado pela Microsoft e Red Hat em conjunto. Para obter mais detalhes, consulte a documentação do [Azure Red Hat OpenShift Service.](https://docs.microsoft.com/azure/openshift/)

## <a name="openshift-container-platform"></a>Plataforma do Contêiner do OpenShift

Plataforma de contêiner é uma [versão comercial](https://www.openshift.com) pronta para empresa do e suportada pelo Red Hat. Com essa versão, o cliente adquire os direitos necessários para a Plataforma de Contêiner OpenShift e é responsável pela instalação e o gerenciamento de toda a infraestrutura.

Como os clientes "possuem" toda a plataforma, eles podem instalá-la em seu datacenter local ou em uma nuvem pública (como o Azure).

## <a name="okd"></a>OKD

O OKD é um projeto upstream de [software livre](https://www.okd.io/) do OpenShift que tem o suporte da comunidade. O OKD pode ser instalado em CentOS ou RHEL (Red Hat Enterprise Linux).

## <a name="next-steps"></a>Próximas etapas

- [Configurar pré-requisitos comuns para OpenShift no Azure](./openshift-container-platform-3x-prerequisites.md)
- [Implantar o OpenShift Container Platform no Azure](./openshift-container-platform-3x.md)
- [Implantar oferta de marketplace auto-gerenciada da plataforma de contêineres OpenShift](./openshift-container-platform-3x-marketplace-self-managed.md)
- [Implantar openshift no Azure Stack](./openshift-azure-stack.md)
- [Tarefas de pós-implantação](./openshift-container-platform-3x-post-deployment.md)
- [Solução de problemas de implantação do OpenShift](./openshift-container-platform-3x-troubleshooting.md)

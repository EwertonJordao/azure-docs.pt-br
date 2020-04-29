---
title: DNS personalizado da instância gerenciada
description: Este tópico descreve opções de configuração para um DNS personalizado com uma Instância Gerenciada do Banco de Dados SQL do Azure.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: sstein, bonova, carlrab
ms.date: 07/17/2019
ms.openlocfilehash: 9c98176413f858d236902c332553d47f749aa3c0
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80247071"
---
# <a name="configuring-a-custom-dns-for-azure-sql-database-managed-instance"></a>Configurar DNS personalizado para Instância Gerenciada do Banco de Dados SQL do Azure

Uma Instância Gerenciada do Banco de Dados SQL do Azure deve ser implantada em uma [VNet (rede virtual)](../virtual-network/virtual-networks-overview.md) do Azure. Há alguns cenários (por exemplo, db mail, servidores vinculados a outras instâncias do SQL em seu ambiente de nuvem ou híbrido) que exigem nomes de hosts privados para serem resolvidos da Instância Gerenciada. Nesse caso, você precisa configurar um DNS personalizado dentro do Azure. 

Como Instância Gerenciada usa o mesmo DNS para seus trabalhos internos, configure o servidor DNS personalizado para que ele possa resolver nomes de domínio públicos.

> [!IMPORTANT]
> Sempre use um FQDN (nome de domínio totalmente qualificado) para o servidor de email, a instância de SQL Server e para outros serviços, mesmo se eles estiverem dentro de sua zona DNS privada. Por exemplo, use `smtp.contoso.com` para seu servidor de email `smtp` porque o não resolverá corretamente. A criação de um servidor vinculado ou replicação que faz referência a VMs do SQL dentro da mesma rede virtual também requer um FQDN e um sufixo DNS padrão. Por exemplo, `SQLVM.internal.cloudapp.net`. Para obter mais informações, consulte [resolução de nomes que usa seu próprio servidor DNS](https://docs.microsoft.com/azure/virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances#name-resolution-that-uses-your-own-dns-server).

> [!IMPORTANT]
> A atualização de servidores DNS da rede virtual não afetará Instância Gerenciada imediatamente. Instância Gerenciada configuração de DNS é atualizada após a concessão do DHCP expirar ou após a atualização da plataforma, o que ocorrer primeiro. **Os usuários são aconselhados a definir sua configuração de DNS de rede virtual antes de criar o primeiro Instância Gerenciada.**

## <a name="next-steps"></a>Próximas etapas

- Para obter uma visão geral, consulte [o que é um instância gerenciada](sql-database-managed-instance.md)
- Para obter um tutorial mostrando como criar uma nova Instância Gerenciada, consulte [Criando uma Instância Gerenciada](sql-database-managed-instance-get-started.md).
- Para obter informações sobre como configurar sua rede virtual à Instância Gerenciada, consulte [Configuração da rede virtual de Instâncias Gerenciadas](sql-database-managed-instance-connectivity-architecture.md)

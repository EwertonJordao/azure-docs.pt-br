---
title: Controles de segurança
titleSuffix: Azure Storage
description: Exibir listas de verificação de controle de segurança para avaliar o armazenamento do Azure. As áreas cobertas são proteção de dados, rede, monitoramento e registro, identidade e configuração.
services: storage
author: msmbaldwin
ms.author: mbaldwin
ms.service: storage
ms.subservice: common
ms.topic: conceptual
ms.date: 03/11/2020
ms.openlocfilehash: 3b4d74e7ba869e0438a936817d824e841823d472
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91715711"
---
# <a name="security-controls-for-azure-storage"></a>Controles de segurança para o armazenamento do Azure

Este artigo documenta os controles de segurança incorporados ao armazenamento do Azure.

[!INCLUDE [Security controls Header](../../../includes/security-controls-header.md)]

## <a name="data-protection"></a>Proteção de dados

| Controle de segurança | Sim/Não | Observações |
|---|---|--|
| Criptografia no lado do servidor em repouso: chaves gerenciadas pela Microsoft | Sim |  |
| Criptografia no lado do servidor em repouso: chaves gerenciadas pelo cliente (BYOK) | Sim | Consulte [criptografia do serviço de armazenamento usando chaves gerenciadas pelo cliente no Azure Key Vault](storage-service-encryption-customer-managed-keys.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).|
| Criptografia em nível de coluna (serviços de dados do Azure)| N/D |  |
| Criptografia em trânsito (como criptografia de ExpressRoute, criptografia de VNet e criptografia de VNet-VNet)| Sim | Suporte a mecanismos HTTPS/TLS padrão.  Os usuários também podem criptografar dados antes de serem transmitidos para o serviço. |
| Chamadas criptografadas à API| Sim |  |

## <a name="network"></a>Rede

| Controle de segurança | Sim/Não | Observações |
|---|---|--|
| Suporte ao ponto de extremidade de serviço| Sim |  |
| Suporte a marcas de serviço| Sim | Consulte [visão geral das marcas de serviço do Azure](../../virtual-network/service-tags-overview.md) para obter mais informações sobre as marcas de serviço com suporte no armazenamento do Azure. |
| Suporte à injeção de VNet| N/D |  |
| Isolamento de rede e suporte a firewall| Sim | |
| Suporte a túnel forçado| N/D |  |

## <a name="monitoring--logging"></a>Monitorando & log

| Controle de segurança | Sim/Não | Observações|
|---|---|--|
| Suporte ao monitoramento do Azure (log Analytics, app insights, etc.)| Sim | Métricas do Azure Monitor|
| Registro e auditoria do plano de gerenciamento e controle | Sim | Log de Atividades do Azure |
| Log e auditoria do plano de dados| Sim | Azure Monitor logs de recursos |

## <a name="identity"></a>Identidade

| Controle de segurança | Sim/Não | Observações|
|---|---|--|
| Autenticação| Sim | Azure Active Directory, chave compartilhada, token de acesso compartilhado. |
| Autorização| Sim | Suporte à autorização por meio do RBAC do Azure, ACLs POSIX e tokens SAS |

## <a name="configuration-management"></a>Gerenciamento de configuração

| Controle de segurança | Sim/Não | Observações|
|---|---|--|
| Suporte ao gerenciamento de configuração (controle de versão de configuração, etc.)| Sim | Suporte ao controle de versão do provedor de recursos por meio de APIs de Azure Resource Manager |

## <a name="next-steps"></a>Próximas etapas

- Saiba mais sobre os [controles de segurança internos nos serviços do Azure](../../security/fundamentals/security-controls.md).
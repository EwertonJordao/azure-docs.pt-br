---
title: Recomendações de segurança para Azure Key Vault
description: Recomendações de segurança para Azure Key Vault. Implementar essas diretrizes ajudará você a atender suas obrigações de segurança, conforme descrito em nosso modelo de responsabilidade compartilhada
services: key-vault
author: barclayn
manager: rkarlin
ms.service: key-vault
ms.subservice: general
ms.topic: article
ms.date: 09/30/2019
ms.author: barclayn
ms.custom: security-recommendations
ms.openlocfilehash: 9fd41ae9b61a9c74fd5d99dd71199cfde06f5cb2
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/29/2020
ms.locfileid: "78192363"
---
# <a name="security-recommendations-for-azure-key-vault"></a>Recomendações de segurança para Azure Key Vault

Este artigo contém recomendações de segurança para Azure Key Vault. Implementar essas recomendações ajudará você a atender suas obrigações de segurança, conforme descrito em nosso modelo de responsabilidade compartilhada. Para obter mais informações sobre o que a Microsoft faz para atender às responsabilidades do provedor de serviços, leia [responsabilidades compartilhadas para computação em nuvem](https://gallery.technet.microsoft.com/Shared-Responsibilities-81d0ff91).

Algumas das recomendações incluídas neste artigo podem ser monitoradas automaticamente pela central de segurança do Azure. A central de segurança do Azure é a primeira linha de defesa na proteção de seus recursos no Azure. Ele analisa periodicamente o estado de segurança de seus recursos do Azure para identificar possíveis vulnerabilidades de segurança. Em seguida, ele fornece recomendações sobre como solucioná-los.

- Para obter mais informações sobre as recomendações da central de segurança do Azure, consulte [recomendações de segurança na central de segurança do Azure](../security-center/security-center-recommendations.md).
- Para obter informações sobre a central de segurança do Azure, consulte o [que é a central de segurança do Azure?](../security-center/security-center-intro.md)

## <a name="data-protection"></a>Proteção de dados

| Recomendação | Comentários | Central de Segurança |
|-|----|--|
|Habilitar exclusão reversível | A [exclusão reversível](key-vault-ovw-soft-delete.md) permite recuperar cofres excluídos e objetos de cofre |  - |
| Limitar o acesso aos dados do cofre  | Siga o princípio de privilégios mínimos e limite quais membros de sua organização têm acesso aos dados do cofre |  - |

## <a name="identity-and-access-management"></a>Gerenciamento de identidade e de acesso

| Recomendação | Comentários | Central de Segurança |
|-|----|--|
| Limitar o número de usuários com acesso de colaborador | Se um usuário tiver permissões de colaborador para um plano de gerenciamento do cofre de chaves, o usuário poderá conceder a si mesmo o acesso ao plano de dados definindo uma política de acesso de Key Vault. Você deve controlar rigidamente quem tem acesso de função de colaborador para seus cofres de chaves. Certifique-se de que apenas aqueles com a necessidade de acessar pessoas autorizadas possam acessar e gerenciar seus cofres. Você pode ler [o acesso seguro a um cofre de chaves](key-vault-secure-your-key-vault.md) | - |

## <a name="monitoring"></a>Monitoramento

| Recomendação | Comentários | Central de Segurança |
|-|----|--|
 Os logs de diagnóstico no Key Vault devem ser habilitados | Ativar os logs e mantenha-os por até um ano. Isso permite recriar trilhas de atividades para fins de investigação quando ocorre um incidente de segurança ou quando sua rede é comprometida. | [Sim](../security-center/security-center-identity-access.md) |
| Restringir quem pode acessar seus logs do cofre de chaves do Azure | [Os logs de Key Vault](key-vault-logging.md) salvam informações sobre as atividades executadas em seu cofre, como criação ou exclusão de cofres, chaves, segredos e podem ser usadas durante uma investigação |  - |

## <a name="networking"></a>Rede

| Recomendação | Comentários | Central de Segurança |
|-|----|--|
|Limitar a exposição da rede | O acesso à rede deve ser limitado às redes virtuais usadas por soluções que exigem acesso ao cofre. Examinar informações sobre [pontos de extremidade de serviço de rede virtual para Azure Key Vault](key-vault-overview-vnet-service-endpoints.md) | - |

## <a name="next-steps"></a>Próximas etapas

Verifique com seu provedor de aplicativos para ver se há requisitos de segurança adicionais. Para obter mais informações sobre como desenvolver aplicativos seguros, consulte a [documentação de desenvolvimento seguro](../security/fundamentals/abstract-develop-secure-apps.md).

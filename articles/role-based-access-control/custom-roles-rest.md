---
title: Criar ou atualizar funções personalizadas para recursos do Azure com a API REST
description: Saiba como listar, criar, atualizar ou excluir funções personalizadas com RBAC (controle de acesso baseado em função) para recursos do Azure usando a API REST.
services: active-directory
documentationcenter: na
author: rolyon
manager: mtillman
editor: ''
ms.assetid: 1f90228a-7aac-4ea7-ad82-b57d222ab128
ms.service: role-based-access-control
ms.workload: multiple
ms.tgt_pltfrm: rest-api
ms.devlang: na
ms.topic: conceptual
ms.date: 04/18/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 145bc45e1b7faeddc23cf5f0662337e15ab51c29
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79245688"
---
# <a name="create-or-update-custom-roles-for-azure-resources-using-the-rest-api"></a>Criar ou atualizar funções personalizadas para recursos do Azure usando a API REST

Se as [funções internas dos recursos do Azure](built-in-roles.md) não atenderem às necessidades específicas de sua organização, você poderá criar suas próprias funções personalizadas. Este artigo descreve como listar, criar, atualizar ou excluir funções personalizadas usando a API REST.

## <a name="list-custom-roles"></a>Listar funções personalizadas

Para listar todas as funções personalizadas em um diretório, use as [definições de função-lista](/rest/api/authorization/roledefinitions/list) de API REST.

1. Comece com a solicitação a seguir:

    ```http
    GET https://management.azure.com/providers/Microsoft.Authorization/roleDefinitions?api-version=2015-07-01&$filter={filter}
    ```

1. Substitua *{Filter}* pelo tipo de função.

    | Filtrar | DESCRIÇÃO |
    | --- | --- |
    | `$filter=type%20eq%20'CustomRole'` | Filtrar com base no tipo CustomRole |

## <a name="list-custom-roles-at-a-scope"></a>Listar funções personalizadas em um escopo

Para listar funções personalizadas em um escopo, use as [definições de função-lista](/rest/api/authorization/roledefinitions/list) de API REST.

1. Comece com a solicitação a seguir:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions?api-version=2015-07-01&$filter={filter}
    ```

1. Dentro da URI, substitua *{scope}* pelo escopo para o qual você deseja listar as funções.

    | Escopo | Type |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Subscription |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Resource group |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Recurso |

1. Substitua *{Filter}* pelo tipo de função.

    | Filtrar | DESCRIÇÃO |
    | --- | --- |
    | `$filter=type%20eq%20'CustomRole'` | Filtrar com base no tipo CustomRole |

## <a name="list-a-custom-role-definition-by-name"></a>Listar uma definição de função personalizada por nome

Para obter informações sobre uma função personalizada por seu nome de exibição, use as [definições de função-obter](/rest/api/authorization/roledefinitions/get) API REST.

1. Comece com a solicitação a seguir:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions?api-version=2015-07-01&$filter={filter}
    ```

1. Dentro da URI, substitua *{scope}* pelo escopo para o qual você deseja listar as funções.

    | Escopo | Type |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Subscription |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Resource group |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Recurso |

1. Substitua *{Filter}* pelo nome de exibição da função.

    | Filtrar | DESCRIÇÃO |
    | --- | --- |
    | `$filter=roleName%20eq%20'{roleDisplayName}'` | Use a forma codificada da URL do nome de exibição exato da função. Por exemplo, `$filter=roleName%20eq%20'Virtual%20Machine%20Contributor'` |

## <a name="list-a-custom-role-definition-by-id"></a>Listar uma definição de função personalizada por ID

Para obter informações sobre uma função personalizada por seu identificador exclusivo, use as [definições de função-obter](/rest/api/authorization/roledefinitions/get) API REST.

1. Use [Definições de Função - Lista](/rest/api/authorization/roledefinitions/list) API REST para obter o identificador GUID para a função.

1. Comece com a solicitação a seguir:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{roleDefinitionId}?api-version=2015-07-01
    ```

1. Dentro da URI, substitua *{scope}* pelo escopo para o qual você deseja listar as funções.

    | Escopo | Type |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Subscription |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Resource group |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Recurso |

1. Substitua *{roleDefinitionId}* pelo identificador GUID de definição da função.

## <a name="create-a-custom-role"></a>Criar uma função personalizada

Para criar uma função personalizada, use [Definições de Função - Criar ou Atualizar](/rest/api/authorization/roledefinitions/createorupdate) API REST. Para chamar essa API, você deve estar conectado com um usuário que recebe uma função que tem a permissão `Microsoft.Authorization/roleDefinitions/write` em todos os `assignableScopes`. Das funções internas, somente o [proprietário](built-in-roles.md#owner) e o [administrador de acesso do usuário](built-in-roles.md#user-access-administrator) incluem essa permissão.

1. Examine a lista de [operações do provedor de recursos](resource-provider-operations.md) que estão disponíveis para criar as permissões para a função personalizada.

1. Use uma ferramenta GUID para gerar um identificador exclusivo que será usado para o identificador de função personalizada. O identificador tem o formato: `00000000-0000-0000-0000-000000000000`

1. Comece com a solicitação e corpo a seguir:

    ```http
    PUT https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{roleDefinitionId}?api-version=2015-07-01
    ```

    ```json
    {
      "name": "{roleDefinitionId}",
      "properties": {
        "roleName": "",
        "description": "",
        "type": "CustomRole",
        "permissions": [
          {
            "actions": [
    
            ],
            "notActions": [
    
            ]
          }
        ],
        "assignableScopes": [
          "/subscriptions/{subscriptionId}"
        ]
      }
    }
    ```

1. Dentro da URI, substita *{scope}* pelo primeiro `assignableScopes` da função personalizada.

    | Escopo | Type |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Subscription |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Resource group |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Recurso |

1. Substitua *{roleDefinitionId}* pelo identificador GUID de definição da função personalizada.

1. Dentro do corpo de solicitação, na `assignableScopes` propriedade, substitua *{roleDefinitionId}* pelo identificador GUID.

1. Substitua *{subscriptionId}* pelo identificador da assinatura.

1. Em `actions` propriedade, adicione as operações que permitem que a função seja executada.

1. Em `notActions` propriedade, adicione as operações que são excluídas do permitidos `actions`.

1. Em `roleName` e `description` propriedades, especifique um nome exclusivo de função e uma descrição. Para obter informações sobre as propriedades, confira [Funções personalizadas](custom-roles.md).

    O exemplo a seguir mostra um exemplo de um corpo de solicitação:

    ```json
    {
      "name": "88888888-8888-8888-8888-888888888888",
      "properties": {
        "roleName": "Virtual Machine Operator",
        "description": "Can monitor and restart virtual machines.",
        "type": "CustomRole",
        "permissions": [
          {
            "actions": [
              "Microsoft.Storage/*/read",
              "Microsoft.Network/*/read",
              "Microsoft.Compute/*/read",
              "Microsoft.Compute/virtualMachines/start/action",
              "Microsoft.Compute/virtualMachines/restart/action",
              "Microsoft.Authorization/*/read",
              "Microsoft.ResourceHealth/availabilityStatuses/read",
              "Microsoft.Resources/subscriptions/resourceGroups/read",
              "Microsoft.Insights/alertRules/*",
              "Microsoft.Support/*"
            ],
            "notActions": []
          }
        ],
        "assignableScopes": [
          "/subscriptions/00000000-0000-0000-0000-000000000000"
        ]
      }
    }
    ```

## <a name="update-a-custom-role"></a>Atualizar uma função personalizada

Para atualizar uma função personalizada, use [Definições de Função - Criar ou Atualizar](/rest/api/authorization/roledefinitions/createorupdate) API REST. Para chamar essa API, você deve estar conectado com um usuário que recebe uma função que tem a permissão `Microsoft.Authorization/roleDefinitions/write` em todos os `assignableScopes`. Das funções internas, somente o [proprietário](built-in-roles.md#owner) e o [administrador de acesso do usuário](built-in-roles.md#user-access-administrator) incluem essa permissão.

1. Use [Definições de Função - Lista](/rest/api/authorization/roledefinitions/list) ou [Definições de Função - Obter](/rest/api/authorization/roledefinitions/get) API REST para obter informações sobre a função personalizada. Para obter mais informações, consulte a seção [listar funções personalizadas](#list-custom-roles) anteriores.

1. Comece com a solicitação a seguir:

    ```http
    PUT https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{roleDefinitionId}?api-version=2015-07-01
    ```

1. Dentro da URI, substita *{scope}* pelo primeiro `assignableScopes` da função personalizada.

    | Escopo | Type |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Subscription |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Resource group |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Recurso |

1. Substitua *{roleDefinitionId}* pelo identificador GUID de definição da função personalizada.

1. Com base nas informações sobre a função personalizada, crie um corpo de solicitação com o seguinte formato:

    ```json
    {
      "name": "{roleDefinitionId}",
      "properties": {
        "roleName": "",
        "description": "",
        "type": "CustomRole",
        "permissions": [
          {
            "actions": [
    
            ],
            "notActions": [
    
            ]
          }
        ],
        "assignableScopes": [
          "/subscriptions/{subscriptionId}"
        ]
      }
    }
    ```

1. Atualize o corpo de solicitação com as alterações que você deseja fazer a função personalizada.

    O exemplo a seguir mostra um exemplo de um corpo de solicitação com uma nova ação de configurações de diagnóstico adicionada:

    ```json
    {
      "name": "88888888-8888-8888-8888-888888888888",
      "properties": {
        "roleName": "Virtual Machine Operator",
        "description": "Can monitor and restart virtual machines.",
        "type": "CustomRole",
        "permissions": [
          {
            "actions": [
              "Microsoft.Storage/*/read",
              "Microsoft.Network/*/read",
              "Microsoft.Compute/*/read",
              "Microsoft.Compute/virtualMachines/start/action",
              "Microsoft.Compute/virtualMachines/restart/action",
              "Microsoft.Authorization/*/read",
              "Microsoft.ResourceHealth/availabilityStatuses/read",
              "Microsoft.Resources/subscriptions/resourceGroups/read",
              "Microsoft.Insights/alertRules/*",
              "Microsoft.Insights/diagnosticSettings/*",
              "Microsoft.Support/*"
            ],
            "notActions": []
          }
        ],
        "assignableScopes": [
          "/subscriptions/00000000-0000-0000-0000-000000000000"
        ]
      }
    }
    ```

## <a name="delete-a-custom-role"></a>Excluir uma função personalizada

Para excluir uma função personalizada, use [Definições de Função - Excluir](/rest/api/authorization/roledefinitions/delete) API REST. Para chamar essa API, você deve estar conectado com um usuário que recebe uma função que tem a permissão `Microsoft.Authorization/roleDefinitions/delete` em todos os `assignableScopes`. Das funções internas, somente o [proprietário](built-in-roles.md#owner) e o [administrador de acesso do usuário](built-in-roles.md#user-access-administrator) incluem essa permissão.

1. Use [Definições de Função - Lista](/rest/api/authorization/roledefinitions/list) ou [Definições de Função - Obter](/rest/api/authorization/roledefinitions/get) API REST para obter o identificador GUI da função personalizada. Para obter mais informações, consulte a seção [listar funções personalizadas](#list-custom-roles) anteriores.

1. Comece com a solicitação a seguir:

    ```http
    DELETE https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{roleDefinitionId}?api-version=2015-07-01
    ```

1. Dentro da URI, substitua *{scope}* pelo escopo para o qual você deseja excluir a função personalizada.

    | Escopo | Type |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Subscription |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Resource group |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Recurso |

1. Substitua *{roleDefinitionId}* pelo identificador GUID de definição da função personalizada.

## <a name="next-steps"></a>Próximas etapas

- [Funções personalizadas para recursos do Azure](custom-roles.md)
- [Gerenciar o acesso aos recursos do Azure usando o RBAC e a API REST](role-assignments-rest.md)
- [Referência de API REST do Azure](/rest/api/azure/)

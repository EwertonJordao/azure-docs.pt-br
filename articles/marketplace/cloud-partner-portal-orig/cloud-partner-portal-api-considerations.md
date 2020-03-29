---
title: Considerações da API | Mercado Azure
description: Problemas de controle de versão, tratamento de erro e autorização ao usar as APIs do Marketplace.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: dsindona
ms.openlocfilehash: 4e04f521ed2023dfb9cd562549cb2e1bcd319b8c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80288624"
---
# <a name="api-considerations"></a>Considerações sobre API


<a name="api-versioning"></a>Controle de versão de API
--------------

É possível que existam várias versões da API disponíveis ao mesmo tempo. Os clientes devem indicar qual versão querem chamar usando o parâmetro `api-version` como parte da cadeia de caracteres de consulta.

   `GET https://cloudpartner.azure.com/api/offerTypes?api-version=2017-10-31`

A resposta a uma solicitação com uma versão de API desconhecida ou inválida é um código HTTP 400. Esse erro retorna a coleção de versões de API conhecidas no corpo da resposta.

``` json
    {
        "error": { 
            "code":"InvalidAPIVersion",
            "message":"Invalid api version. Allowed values are [2016-08-01-preview]"
        }
    }
```            

<a name="errors"></a>Errors
------

A API responde a erros com os códigos de status HTTP correspondentes e, opcionalmente, informações adicionais na resposta serializada como JSON.
Quando você receber um erro, especialmente um erro de classe 400, não tente a solicitação novamente antes de corrigir a causa subjacente. Por exemplo, na resposta de exemplo acima, corrija o parâmetro de versão da API antes de reenviar a solicitação.

<a name="authorization-header"></a>Cabeçalho de autorização
--------------------

Para todas as APIs nessa referência, você deve passar o cabeçalho de autorização junto com o token de portador obtido do Azure AD (Azure Active Directory). Esse cabeçalho é necessário para receber uma resposta válida e, se não estiver presente, um erro `401 Unauthorized` será retornado. 

``` HTTP
  GET https://cloudpartner.azure.com/api/offerTypes?api-version=2016-08-01-preview

    Accept: application/json 
    Authorization: Bearer <YOUR_TOKEN> 
```

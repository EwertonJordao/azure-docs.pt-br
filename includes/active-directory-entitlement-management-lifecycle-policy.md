---
title: arquivo de inclusão
description: arquivo de inclusão
services: active-directory
author: msaburnley
ms.service: active-directory
ms.topic: include
ms.date: 10/15/2019
ms.author: ajburnle
ms.custom: include file
ms.openlocfilehash: e971af5d415ea788f97ab11b58541e629c7eb0ed
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/09/2020
ms.locfileid: "76694338"
---
## <a name="lifecycle"></a>Ciclo de vida

Na guia **ciclo de vida** , você especifica quando a atribuição de um usuário para o pacote de acesso expira. Você também pode especificar se os usuários podem estender suas atribuições.

1. Na seção **expiração** , defina as **atribuições de pacote de acesso expiram** como **em data**, **número de dias**ou **nunca**.

    Para **na data**, selecione uma data de validade no futuro.

    Para **número de dias**, especifique um número entre 0 e 3660 dias.

    Com base na sua seleção, a atribuição de um usuário ao pacote de acesso expira em uma determinada data, um determinado número de dias depois que eles são aprovados ou nunca.

1. Clique em **Mostrar configurações de expiração avançadas** para mostrar configurações adicionais.

    ![Pacotes de acesso-configurações de expiração do ciclo de vida](./media/active-directory-entitlement-management-lifecycle-policy/expiration.png)

1. Para permitir que o usuário Estenda suas atribuições, defina **permitir que os usuários estendam o acesso** para **Sim**.

    Se as extensões forem permitidas na política, o usuário receberá um email 14 dias e também um dia antes que sua atribuição de pacote de acesso seja definida para expirar solicitando que estendam a atribuição. Se o usuário enviar uma solicitação para estender o acesso, a data da extensão deverá ser em ou antes que as atribuições expirem conforme definido na política usada para conceder ao usuário acesso ao pacote de acesso. Por exemplo, se a política indicar que as atribuições estão definidas para expirar em 30 de junho, a extensão máxima que um usuário pode solicitar é 30 de junho.

    Se o acesso de um usuário for estendido, ele não poderá solicitar o pacote de acesso após a data de extensão especificada (data definida no fuso horário do usuário que criou a política).

1. Para exigir aprovação para conceder uma extensão, defina **exigir aprovação para conceder a extensão** para **Sim**.

    As mesmas configurações de aprovação que foram especificadas na guia **solicitações** serão usadas.

1. Clique em **Avançar** ou **Atualizar**.

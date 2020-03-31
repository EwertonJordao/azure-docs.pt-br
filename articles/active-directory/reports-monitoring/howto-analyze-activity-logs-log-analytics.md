---
title: Analisar registros de atividades usando logs do Monitor do Azure | Microsoft Docs
description: Saiba como analisar os registros de atividades do Azure Active Directory usando logs do Monitor do Azure
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: 4535ae65-8591-41ba-9a7d-b7f00c574426
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 04/18/2019
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2d6212692465270182db541889bed5f03a08a345
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74008281"
---
# <a name="analyze-azure-ad-activity-logs-with-azure-monitor-logs"></a>Analisar logs de atividade do Azure AD com os logs do Azure Monitor

Depois de [integrar os logs de atividades do Azure AD com os logs do Azure Monitor](howto-integrate-activity-logs-with-log-analytics.md), você pode usar o poder dos logs do Azure Monitor para obter insights sobre seu ambiente. Você também pode instalar as [visualizações de análise de log para logs de atividades do Azure AD](howto-install-use-log-analytics-views.md) para ter acesso a relatórios pré-construídos em torno de eventos de auditoria e login em seu ambiente.

Neste artigo, você aprenderá como analisar o logs de atividades do Azure AD no seu espaço de trabalho do Log Analytics. 

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="prerequisites"></a>Pré-requisitos 

Para acompanhar, você precisa:

* Um espaço de trabalho do Log Analytics em sua assinatura do Azure. Saiba como [criar um espaço de trabalho do Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-quick-create-workspace).
* Em primeiro lugar, conclua as etapas para [rotear os logs de atividades do Azure AD para seu espaço de trabalho do Log Analytics](howto-integrate-activity-logs-with-log-analytics.md).
*  [Acesso](https://docs.microsoft.com/azure/azure-monitor/platform/manage-access#manage-access-using-workspace-permissions) ao espaço de trabalho de análise de log
* As seguintes funções no Azure Active Directory (se você estiver acessando o Log Analytics através do portal do Azure Active Directory)
    - Administrador de Segurança
    - Leitor de segurança
    - Leitor de relatórios
    - Administrador global
    
## <a name="navigate-to-the-log-analytics-workspace"></a>Navegar para seu espaço de trabalho do Log Analytics

1. Faça login no [portal Azure](https://portal.azure.com). 

2. Selecione **Azure Active Directory** e, em seguida, selecione **Logs** na seção **Monitoramento** para abrir o espaço de trabalho do Log Analytics. O workspace será aberto com uma consulta padrão.

    ![Consulta padrão](./media/howto-analyze-activity-logs-log-analytics/defaultquery.png)


## <a name="view-the-schema-for-azure-ad-activity-logs"></a>Exibir o esquema para logs de atividade do Azure AD

É efetuado push dos logs para as tabelas **AuditLogs** e **SigninLogs** no workspace. Para exibir o esquema para estas tabelas:

1. Na visualização da consulta padrão na seção anterior, selecione **Esquema** e expanda o workspace. 

2. Expanda a seção **Gerenciamento de Log** e, em seguida, expanda **AuditLogs** ou **SignInLogs** para exibir o esquema de log.
    ![Registros de](./media/howto-analyze-activity-logs-log-analytics/auditlogschema.png) ![auditoria Registrações de login](./media/howto-analyze-activity-logs-log-analytics/signinlogschema.png)

## <a name="query-the-azure-ad-activity-logs"></a>Consulte os logs de atividade do Azure AD

Agora que você tem os logs em seu workspace, você pode executar consultas em relação a eles. Por exemplo, para obter os aplicativos principais usados na última semana, substitua a consulta padrão pelo seguinte e selecione **Executar**

```
SigninLogs 
| where CreatedDateTime >= ago(7d)
| summarize signInCount = count() by AppDisplayName 
| sort by signInCount desc 
```

Para obter os principais eventos de auditoria na última semana, use a seguinte consulta:

```
AuditLogs 
| where TimeGenerated >= ago(7d)
| summarize auditCount = count() by OperationName 
| sort by auditCount desc 
```
## <a name="alert-on-azure-ad-activity-log-data"></a>Alerta sobre dados de log de atividades do Azure AD

Você também pode configurar alertas em sua consulta. Por exemplo, para configurar um alerta quando mais de 10 aplicativos tiverem sido usados na última semana:

1. No workspace, selecione **Definir alerta** para abrir a página **Criar regra**.

    ![Definir alerta](./media/howto-analyze-activity-logs-log-analytics/setalert.png)

2. Selecione os **critérios de alerta** padrão criados no alerta e atualize o **Limite** na métrica padrão para 10.

    ![Critérios do alerta](./media/howto-analyze-activity-logs-log-analytics/alertcriteria.png)

3. Insira um nome e uma descrição para o alerta e escolha o nível de gravidade. Para nosso exemplo, podemos pode defini-lo como **Informativo**.

4. Selecione o **Grupo de Ações** que receberá um alerta quando ocorrer o sinal. Você pode optar por notificar a equipe por email ou mensagem de texto, ou pode automatizar a ação usando webhooks, o Azure Functions ou aplicativos lógicos. Saiba mais sobre [como criar e gerenciar grupos de alerta no portal do Azure](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-action-groups).

5. Depois de configurar o alerta, selecione **Criar alerta** para habilitá-lo. 

## <a name="install-and-use-pre-built-views-for-azure-ad-activity-logs"></a>Instalar e usar exibições predefinidas para logs de atividades do Azure AD

Você também pode baixar as exibições do Log Analytics predefinidas para logs de atividades do Azure AD. As exibições fornecem vários relatórios relacionados a cenários comuns que envolvem eventos de auditoria e entrada. Você também pode alertar sobre qualquer um dos dados fornecidos nos relatórios seguindo as etapas descritas na seção anterior.

* **Eventos de Provisionamento de Conta do Azure AD**: esta exibição mostra os relatórios relacionados à auditoria da atividade de provisionamento, como o número de novos usuários provisionados e falhas de provisionamento, número de usuários atualizados e falhas de atualização e número de usuários desprovisionados e falhas correspondentes.    
* **Eventos de Entradas**: esta exibição mostra os relatórios mais relevantes relacionados à atividade de entrada de monitoramento, como entradas por aplicativo, usuário, dispositivo, bem como exibição resumida acompanhando o número de entradas ao longo do tempo.
* **Consentimento dos Usuários para Execução**: esta exibição mostra os relatórios relacionados ao consentimento do usuário, como o consentimento dados por usuário, entradas por usuários que recebeu consentimento, bem como entradas por aplicativo para todos os aplicativos baseados em consentimento. 

Saiba como [instalar e usar as exibições do Log Analytics para logs de atividades do Azure AD](howto-install-use-log-analytics-views.md). 


## <a name="next-steps"></a>Próximas etapas

* [Introdução às consultas de log do Azure Monitor](https://docs.microsoft.com/azure/log-analytics/query-language/get-started-queries)
* [Criar e gerenciar grupos de alertas no portal do Azure](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-action-groups)
* [Instalar e usar os modos de exibição do Log Analytics do Azure Active Directory](howto-install-use-log-analytics-views.md)

---
title: Visão geral das consultas de log no Azure Monitor | Microsoft Docs
description: Responde a perguntas comuns relacionadas a consultas de log e você começa a usá-las.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 06/19/2019
ms.openlocfilehash: 7e8e2015e7d0798542ff0c80d8eb39461918ad67
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86539594"
---
# <a name="overview-of-log-queries-in-azure-monitor"></a>Visão geral das consultas de log no Azure Monitor
As consultas de log ajudam você a aproveitar totalmente o valor dos dados coletados nos [logs de Azure monitor](../platform/data-platform-logs.md). Uma linguagem de consulta eficiente permite unir dados de várias tabelas, agregar grandes conjuntos de dados e executar operações complexas com o mínimo de código. Praticamente qualquer pergunta pode ser respondida e a análise realizada desde que os dados de suporte tenham sido coletados e você entenda como construir a consulta correta.

Alguns recursos em Azure Monitor como [informações](../insights/insights-overview.md) e [soluções](../monitor-reference.md) processam dados de log sem expor você às consultas subjacentes. Para aproveitar totalmente outros recursos do Azure Monitor, você deve entender como as consultas são construídas e como usá-las para analisar interativamente os dados nos logs do Azure Monitor.

Use este artigo como um ponto de partida para aprender sobre consultas de log no Azure Monitor. Ele responde a perguntas comuns e fornece links para outras documentações que fornecem mais detalhes e lições.

## <a name="how-can-i-learn-how-to-write-queries"></a>Como posso aprender a escrever consultas?
Se você quiser ir diretamente para as coisas, poderá começar com os seguintes tutoriais:

- Comece a usar o [log Analytics no Azure monitor](get-started-portal.md).
- [Introdução às consultas de log no Azure monitor](get-started-queries.md).

Depois de obter as noções básicas, percorra várias lições usando seus próprios dados ou dados de nosso ambiente de demonstração, começando com: 

- [Trabalhe com cadeias de caracteres nas consultas de log no Azure Monitor](string-operations.md)
 
## <a name="what-language-do-log-queries-use"></a>Que linguagem as consultas de log usam?
Os logs de Azure Monitor baseiam-se no [Azure data Explorer](/azure/data-explorer)e as consultas de log são escritas usando a mesma linguagem de consulta KUSTO (KQL). Essa é uma linguagem avançada criada para ser fácil de ler e criar e você deve ser capaz de começar a usá-la com diretrizes mínimas.

Consulte a [documentação do Azure data Explorer KQL](/azure/kusto/query) para obter a documentação completa sobre o KQL e a referência em funções diferentes disponíveis.<br>
Consulte Introdução [às consultas de log no Azure monitor](get-started-queries.md) para obter uma rápida explicação do idioma usando dados de Logs de Azure monitor.
Veja [Azure monitor diferenças na linguagem de consulta de log](data-explorer-difference.md) para pequenas diferenças na versão do KQL usada pelo Azure monitor.

## <a name="what-data-is-available-to-log-queries"></a>Quais dados estão disponíveis para registrar consultas?
Todos os dados coletados em logs de Azure Monitor estão disponíveis para recuperar e analisar em consultas de log. Diferentes fontes de dados gravarão seus dados em tabelas diferentes, mas você pode incluir várias tabelas em uma única consulta para analisar dados em várias fontes. Ao criar uma consulta, você começa determinando quais tabelas têm os dados que está procurando e, portanto, deve ter pelo menos um entendimento básico de como os dados em logs de Azure Monitor são estruturados.

Consulte [fontes de logs de Azure monitor](../platform/data-platform-logs.md#sources-of-azure-monitor-logs)para obter uma lista de diferentes fontes de dados que preenchem Azure monitor logs.<br>
Consulte [estrutura de logs de Azure monitor](logs-structure.md) para obter uma explicação de como os dados são estruturados.

## <a name="what-does-a-log-query-look-like"></a>Como é a aparência de uma consulta de log?
Uma consulta pode ser tão simples quanto um único nome de tabela para recuperar todos os registros dessa tabela:

```Kusto
Syslog
```

Ou poderia filtrar registros específicos, resumi-los e visualizar os resultados em um gráfico:

```
SecurityEvent
| where TimeGenerated > ago(7d)
| where EventID == 4625
| summarize count() by Computer, bin(TimeGenerated, 1h)
| render timechart 
```

Para uma análise mais complexa, você pode recuperar dados de várias tabelas usando uma junção para analisar os resultados juntos.

```Kusto
app("ContosoRetailWeb").requests
| summarize count() by bin(timestamp,1hr)
| join kind= inner (Perf
    | summarize avg(CounterValue) 
      by bin(TimeGenerated,1hr))
on $left.timestamp == $right.TimeGenerated
```
Mesmo que você não esteja familiarizado com o KQL, você deve ser capaz de, pelo menos, descobrir a lógica básica que está sendo usada por essas consultas. Eles começam com o nome de uma tabela e, em seguida, adicionam vários comandos para filtrar e processar esses dados. Uma consulta pode usar qualquer número de comandos, e você pode escrever consultas mais complexas conforme se familiarizar com os diferentes comandos do KQL disponíveis.

Consulte Introdução [às consultas de log no Azure monitor](get-started-queries.md) para obter um tutorial sobre consultas de log que introduzem a linguagem e as funções comuns,.<br>


## <a name="what-is-log-analytics"></a>O que é o Log Analytics?
O Log Analytics é a principal ferramenta no portal do Azure para escrever consultas de log e analisar interativamente os resultados. Mesmo que uma consulta de log seja usada em outro lugar no Azure Monitor, você normalmente escreverá e testará a consulta primeiro usando o Log Analytics.

Você pode iniciar o Log Analytics de vários locais na portal do Azure. O escopo dos dados disponíveis para Log Analytics é determinado pelo modo como você o inicia. Consulte [escopo da consulta](scope.md) para obter mais detalhes.

- Selecione **logs** no menu **Azure monitor** ou no menu **log Analytics espaços de trabalho** .
- Selecione **logs** na página **visão geral** de um aplicativo Application insights.
- Selecione **logs** no menu de um recurso do Azure.

![Log Analytics](media/log-query-overview/log-analytics.png)

Confira [introdução ao log Analytics no Azure monitor](get-started-portal.md) para obter um tutorial explicativo de log Analytics que apresenta vários de seus recursos.

## <a name="where-else-are-log-queries-used"></a>Onde outras consultas de log são usadas?
Além de trabalhar interativamente com consultas de log e seus resultados em Log Analytics, as áreas em Azure Monitor em que você usará consultas incluem o seguinte:

- **Regras de alerta** As [Regras de alerta](../platform/alerts-overview.md) identificam proativamente os problemas dos dados no workspace.  Cada regra de alerta é baseada em uma pesquisa de logs que é executada automaticamente em intervalos regulares.  Os resultados são inspecionados para determinar se um alerta deve ser criado.
- **Painéis.** Você pode fixar os resultados da consulta em um [painel do Azure](../learn/tutorial-logs-dashboards.md), que permite visualizar os dados de log e de métrica em conjunto e, opcionalmente, compartilhar com outros usuários do Azure.
- **Modos de exibição.**  Você pode criar visualizações de dados a serem incluídas em painéis de usuários com [Designer de Exibição](../platform/view-designer.md).  As consultas de logs fornecem os dados usados por [blocos](../platform/view-designer-tiles.md) e [blocos de visualização](../platform/view-designer-parts.md) em cada exibição.  
- **Export.**  Ao importar dados de log do Azure Monitor para o Excel ou o [Power BI](../platform/powerbi.md), você cria uma consulta de logs para definir os dados a serem exportados.
- **PowerShell.** Você pode executar um script do PowerShell de uma linha de comando ou um runbook de automação do Azure que usa [Get-AzOperationalInsightsSearchResults](/powershell/module/az.operationalinsights/get-azoperationalinsightssearchresult) para recuperar dados de log de Azure monitor.  Esse cmdlet requer uma consulta para determinar os dados a serem recuperados.
- **API de Logs do Azure Monitor.**  A [API de Logs do Azure Monitor](https://dev.loganalytics.io) permite que qualquer cliente da API REST recupere dados de log do workspace.  A solicitação de API inclui uma consulta que é executada no Azure Monitor para determinar os dados a serem recuperados.


## <a name="next-steps"></a>Próximas etapas
- Percorra um [tutorial sobre como usar log Analytics no portal do Azure](get-started-portal.md).
- Percorra um [tutorial sobre como escrever consultas](get-started-queries.md).

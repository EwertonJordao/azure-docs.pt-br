---
title: Consulta de dados na visualização-Azure Time Series Insights | Microsoft Docs
description: Visão geral dos conceitos de consulta de dados e da API REST HTTP na visualização Azure Time Series Insights.
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.workload: big-data
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 02/07/2020
ms.custom: seodec18
ms.openlocfilehash: 898515f49672a19ed8bf1c62439128b6727afc73
ms.sourcegitcommit: cfbea479cc065c6343e10c8b5f09424e9809092e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/08/2020
ms.locfileid: "77087396"
---
# <a name="data-querying-in-azure-time-series-insights-preview"></a>Consulta de dados na visualização Azure Time Series Insights

A Versão Prévia do Azure Time Series Insights permite consultar dados em eventos e metadados armazenados no ambiente por meio de APIs de superfície públicas. Essas APIs também são usadas no [Explorer da Versão Prévia do Time Series Insights](./time-series-insights-update-explorer.md).

Três categorias principais de API estão disponíveis no Time Series Insights:

* **APIs de ambiente**: essas APIs habilitam consultas no próprio ambiente de time Series insights. Exemplos de consultas incluem a lista de ambientes a que o chamador tem acesso e metadados do ambiente.
* **APIs de consulta de modelo de série temporal (TSM-Q)** : habilita operações de criação, leitura, atualização e exclusão (CRUD) em metadados armazenados na parte do ambiente do modelo de série temporal. Exemplos incluem hierarquias, tipos e instâncias.
* **APIs de consulta de série temporal (TSQ)** : habilita a recuperação de dados de telemetria ou eventos conforme eles são gravados do provedor de origem ou pela redução dos dados usando funções escalares e de agregação armazenadas parte das variáveis. Essas APIs podem executar operações para transformar, combinar e aplicar cálculos em dados de série temporal.

Time Series Insights usa uma linguagem de expressão baseada em cadeia de caracteres avançada, [TSX (expressão de série temporal)](https://docs.microsoft.com/rest/api/time-series-insights/preview-tsx), para expressar cálculos.

## <a name="azure-time-series-insights-preview-core-apis"></a>Principais APIs da Versão Prévia do Azure Time Series Insights

Há suporte para as APIs principais a seguir.

[Visão geral de consulta de série temporal ![](media/v2-update-tsq/tsq.png)](media/v2-update-tsq/tsq.png#lightbox)

## <a name="environment-apis"></a>APIs de ambiente

As seguintes APIs de ambiente estão disponíveis:

* [Obter a API de ambientes](/rest/api/time-series-insights/management/environments/get): retorna a lista de ambientes aos quais o chamador está autorizado a acessar.
* [Obter API de disponibilidade de ambientes](/rest/api/time-series-insights/dataaccess(preview)/query/getavailability): retorna a distribuição da contagem de eventos sobre o `$ts`do carimbo de data/hora do evento. Essa API ajuda a determinar se há eventos no carimbo de data/hora retornando a contagem de eventos, se existir.
* [Obter API do esquema de evento](/rest/api/time-series-insights/dataaccess(preview)/query/geteventschema): retorna os metadados do esquema de evento para um determinado intervalo de pesquisa. Essa API ajuda a recuperar todos os metadados e as propriedades disponíveis no esquema para determinado período pesquisado.

## <a name="time-series-model-query-tsm-q-apis"></a>APIs TSM-Q (Consulta do modelo do Time Series)

As seguintes APIs de consulta de modelo de série temporal estão disponíveis. A maioria dessas APIs dá suporte à operação de execução em lote para habilitar operações CRUD de lote em várias entidades de modelo de série temporal:

* [API de configurações de modelo](https://docs.microsoft.com/rest/api/time-series-insights/preview-model#model-settings-api): habilita *Get* e *patch* no tipo padrão e o nome do modelo do ambiente.
* [API de tipos](https://docs.microsoft.com/rest/api/time-series-insights/preview-model#types-api): habilita CRUD em tipos de série temporal e suas variáveis associadas.
* [API de hierarquias](https://docs.microsoft.com/rest/api/time-series-insights/preview-model#hierarchies-api): habilita CRUD em hierarquias de série temporal e seus caminhos de campo associados.
* [API de instâncias](https://docs.microsoft.com/rest/api/time-series-insights/preview-model#instances-api): habilita CRUD em instâncias de série temporal e seus campos de instância associados. Além disso, a API de instâncias do oferece suporte às seguintes operações:
  * [Pesquisa](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/timeseriesinstances/search): recupera uma lista parcial de ocorrências na pesquisa de instâncias de série temporal com base em atributos de instância.
  * [Sugerir](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/timeseriesinstances/suggest): pesquisa e sugere uma lista parcial de ocorrências na pesquisa de instâncias de série temporal com base em atributos de instância.

## <a name="time-series-query-tsq-apis"></a>APIs TSQ (consulta de série temporal)

As seguintes APIs de consulta de série temporal estão disponíveis. Essas APIs estão disponíveis em todos os armazenamentos em várias camadas com suporte no Time Series Insights. Os parâmetros de URL de consulta são usados para especificar o [tipo de repositório](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/query/execute#uri-parameters) em que a consulta deve ser executada:

* [API Get Events](/rest/api/time-series-insights/dataaccess(preview)/query/execute#getevents): habilita a consulta e a recuperação de dados de time Series insights de eventos conforme eles são gravados em time Series insights do provedor de origem. Essa API permite a recuperação de eventos brutos para uma determinada ID de série temporal e intervalo de pesquisa. Essa API dá suporte à paginação para recuperar o conjunto de dados completo da entrada selecionada. 

* [Obter a API da série](/rest/api/time-series-insights/dataaccess(preview)/query/execute#getseries): habilita a consulta e a recuperação de dados de time Series insights de eventos capturados usando dados registrados na conexão. Os valores que são retornados se baseiam nas variáveis que foram definidas no modelo ou fornecidas internamente. Essa API dá suporte à paginação para recuperar o conjunto de dados completo da entrada selecionada. Essa API ajuda a definir propriedades ou colunas calculadas.

    >[!NOTE]
    > A cláusula Aggregation é ignorada, mesmo se estiver especificada em um modelo ou embutida.

  A API de obtenção de série retorna um valor de série temporal para cada variável e intervalo. Um valor de série temporal é um formato que Time Series Insights usa para o JSON de saída de uma consulta. Os valores retornados são com base na ID do Time Series e no conjunto de variáveis que foram fornecidos.

* [API da série de agregação](/rest/api/time-series-insights/dataaccess(preview)/query/execute#aggregatevariable): habilita a consulta e a recuperação de dados de time Series insights de eventos capturados por amostragem e agregação de dados gravados. Essa API dá suporte à execução de continuável usando [tokens de continuação](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/query/execute#queryresultpage).

  A API de agregação de série retorna um valor de série temporal para cada variável e intervalo. Os valores são com base na ID do Time Series e no conjunto de variáveis que foram fornecidos. A API de agregação de série consegue fazer uma redução usando as variáveis armazenadas no modelo do Time Series ou embutidas para agregar ou coletar exemplo dos dados.

## <a name="next-steps"></a>Próximas etapas

- Saiba mais sobre o [armazenamento e a entrada](./time-series-insights-update-storage-ingress.md) na versão prévia do Azure Time Series insights.
- Leia o artigo Time Series Insights Preview [Data Modeling](./time-series-insights-update-tsm.md) .
- Descubra [as práticas recomendadas para escolher uma ID de série temporal](./time-series-insights-update-how-to-id.md).

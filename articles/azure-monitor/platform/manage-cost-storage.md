---
title: Gerenciar o uso e os custos de logs de Azure Monitor
description: Saiba como alterar o plano de preços e gerenciar o volume de dados e a política de retenção para seu espaço de trabalho do Log Analytics no Azure Monitor.
services: azure-monitor
documentationcenter: azure-monitor
author: bwren
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 04/28/2020
ms.author: bwren
ms.subservice: ''
ms.openlocfilehash: 8904d584d453cb0945a11b08ad50688aeb1e1fc0
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82207319"
---
# <a name="manage-usage-and-costs-with-azure-monitor-logs"></a>Gerenciar o uso e os custos com logs de Azure Monitor

> [!NOTE]
> Este artigo descreve como entender e controlar seus custos para logs de Azure Monitor. Um artigo relacionado, [monitoramento de uso e custos estimados](https://docs.microsoft.com/azure/azure-monitor/platform/usage-estimated-costs) descreve como exibir o uso e os custos estimados em vários recursos de monitoramento do Azure para diferentes modelos de preços. Todos os preços e custos mostrados neste artigo são apenas para fins de exemplo. 

Os logs de Azure Monitor são projetados para dimensionar e dar suporte à coleta, indexação e armazenamento de grandes quantidades de dados por dia de qualquer fonte em sua empresa ou implantados no Azure.  Embora isso possa ser um driver primário para a organização, a eficiência de custo é, em última instância, o driver subjacente. Para esse fim, é importante entender que o custo de um espaço de trabalho Log Analytics não se baseia apenas no volume de dados coletados, mas também depende do plano selecionado e por quanto tempo você optou por armazenar os dados gerados de suas fontes conectadas.  

Neste artigo, examinaremos como você pode monitorar proativamente o crescimento do armazenamento e do volume de dados ingeridos e definir limites para controlar esses custos associados. 

## <a name="pricing-model"></a>Modelo de preços

O preço padrão para Log Analytics é um modelo **pago conforme o uso** com base no volume de dados ingerido e, opcionalmente, para a retenção de dados mais longa. O volume de dados é medido como o tamanho dos dados que serão armazenados. Cada espaço de trabalho Log Analytics é cobrado como um serviço separado e contribui para a fatura de sua assinatura do Azure. A quantidade de ingestão de dados pode ser considerável dependendo dos seguintes fatores: 

  - Número de soluções de gerenciamento habilitadas e suas configurações
  - Número de VMs monitoradas
  - Tipo de dados coletados de cada VM monitorada 
  
Além do modelo pago conforme o uso, Log Analytics tem camadas de **reserva de capacidade** que permitem que você economize até 25% em comparação com o preço pago conforme o uso do. O preço de reserva de capacidade permite que você compre uma reserva a partir de 100 GB/dia. Qualquer uso acima do nível de reserva será cobrado com a taxa paga conforme o uso. As camadas de reserva de capacidade têm um período de compromisso de 31 dias. Durante o período de compromisso, você pode alterar para uma camada de reserva de capacidade de nível superior (que reiniciará o período de compromisso de 31 dias), mas não poderá voltar para o pré-pago ou para uma camada de reserva de capacidade mais baixa até que o período de compromisso seja concluído. A cobrança para as camadas de reserva de capacidade é feita diariamente. [Saiba mais](https://azure.microsoft.com/pricing/details/monitor/) sobre o log Analytics preços pagos conforme o uso e de reserva de capacidade. 

Em todos os tipos de preço, o volume de dados é calculado a partir de uma representação de cadeia de caracteres dos dados conforme eles são preparados para serem armazenados. Várias [Propriedades comuns a todos os tipos de dados](https://docs.microsoft.com/azure/azure-monitor/platform/log-standard-properties) não são incluídas no cálculo do tamanho do evento, `_ResourceId`incluindo `_ItemId`, `_IsBillable` e `_BilledSize`.

Além disso, observe que algumas soluções, como a [central de segurança do Azure](https://azure.microsoft.com/pricing/details/security-center/), o [Azure Sentinel](https://azure.microsoft.com/pricing/details/azure-sentinel/) e o [Gerenciamento de configuração](https://azure.microsoft.com/pricing/details/automation/) têm seus próprios modelos de preços. 

### <a name="dedicated-clusters"></a>Clusters dedicados

Azure Monitor clusters dedicados de log são coleções de espaços de trabalho em um único cluster gerenciado do Azure Data Explorer (ADX) para dar suporte a cenários avançados, como [chaves gerenciadas pelo cliente](https://docs.microsoft.com/azure/azure-monitor/platform/customer-managed-keys).  Os clusters dedicados dão suporte a apenas um modelo de preços de reserva de capacidade a partir de 1000 GB/dia com um desconto de 25% em comparação com o preço pago conforme o uso. Qualquer uso acima do nível de reserva será cobrado com a taxa paga conforme o uso. A reserva de capacidade do cluster tem um período de compromisso de 31 dias após o aumento do nível de reserva. Durante o período de compromisso, o nível de reserva de capacidade não pode ser reduzido, mas pode ser aumentado a qualquer momento. Saiba mais sobre como [Criar clusters dedicados](https://docs.microsoft.com/azure/azure-monitor/platform/customer-managed-keys#create-cluster-resource) e [associar espaços de trabalho a ele](https://docs.microsoft.com/azure/azure-monitor/platform/customer-managed-keys#workspace-association-to-cluster-resource).  

Como a cobrança de dados ingeridos é feita no nível do cluster, os espaços de trabalho associados a um cluster não têm mais um tipo de preço. As quantidades de dados ingeridos de cada espaço de trabalho associado a um cluster são agregadas para calcular a fatura diária do cluster. Observe que as alocações por nó da central de segurança do Azure são aplicadas no nível do espaço de trabalho antes dessa agregação. A retenção de dados ainda é cobrada no nível do espaço de trabalho.  

## <a name="estimating-the-costs-to-manage-your-environment"></a>Estimando os custos para gerenciar seu ambiente 

Se você ainda não estiver usando logs de Azure Monitor, poderá usar a [calculadora de preços de Azure monitor](https://azure.microsoft.com/pricing/calculator/?service=monitor) para estimar o custo do uso de log Analytics. Comece inserindo "Azure Monitor" na caixa de pesquisa e clicando no bloco do Azure Monitor resultante. Role a página para baixo até Azure Monitor e selecione Log Analytics na lista suspensa tipo.  Aqui você pode inserir o número de VMs e os GB de dados que espera coletar de cada VM. Normalmente, 1 a 3 GB de dados de mês é ingerido de uma VM do Azure típica. Se já estiver avaliando os logs de Azure Monitor, você poderá usar suas estatísticas de dados do seu próprio ambiente. Veja abaixo como determinar o [número de VMs monitoradas](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#understanding-nodes-sending-data) e o [volume de dados que seu espaço de trabalho está ingerindo](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#understanding-ingested-data-volume). 

## <a name="understand-your-usage-and-estimate-costs"></a>Entenda seu uso e calcule os custos

Se você estiver usando logs de Azure Monitor agora, é fácil entender quais são os custos mais prováveis com base em padrões de uso recentes. Para fazer isso, use **log Analytics uso e custos estimados** para revisar e analisar o uso de dados. Isso mostra quantos dados são coletados por cada solução, quantos dados estão sendo retidos e uma estimativa dos custos com base na quantidade de dados ingeridos e qualquer retenção adicional além da quantidade incluída.

![Uso e custos estimados](media/manage-cost-storage/usage-estimated-cost-dashboard-01.png)

Para explorar seus dados com mais detalhes, clique no ícone no canto superior direito de qualquer um dos gráficos na página **Uso e custos estimados**. Agora você pode trabalhar com essa consulta para explorar mais detalhes do seu uso.  

![Exibir logs](media/manage-cost-storage/logs.png)

Da página**uso e custos estimados**, pode-se examinar o seu volume de dados para o mês. Isso inclui todos os dados recebidos e retidos no espaço de trabalho do Log Analytics.  Clique em **detalhes de uso** na parte superior da página para exibir o painel uso com informações sobre as tendências de volume de dados por fonte, computadores e oferta. Para exibir e definir um limite diário ou modificar o período de retenção, clique em **Gerenciamento de volume de dados**.
 
Os encargos do Log Analytics são adicionadas à sua fatura do Azure. É possível ver os detalhes da fatura do Azure na seção Cobrança do Portal do Azure ou no [Portal de Cobrança do Azure](https://account.windowsazure.com/Subscriptions).  

## <a name="viewing-log-analytics-usage-on-your-azure-bill"></a>Exibindo o uso de Log Analytics em sua fatura do Azure 

O Azure fornece uma grande funcionalidade útil no gerenciamento de [custos do Azure +](https://docs.microsoft.com/azure/cost-management/quick-acm-cost-analysis?toc=/azure/billing/TOC.json) Hub de cobrança. Por exemplo, a funcionalidade de "análise de custo" permite que você exiba seus gastos para os recursos do Azure. A adição de um filtro por tipo de recurso (para Microsoft. operationalinsights/Workspace para Log Analytics) permitirá que você acompanhe seus gastos.

Mais informações sobre seu uso podem ser obtidas [baixando seu uso do portal do Azure](https://docs.microsoft.com/azure/billing/billing-download-azure-invoice-daily-usage-date#download-usage-in-azure-portal). Na planilha baixada, você pode ver o uso por recurso do Azure (por exemplo, Log Analytics espaço de trabalho) por dia. Nesta planilha do Excel, o uso de seus espaços de trabalho do Log Analytics pode ser encontrado primeiro filtrando a coluna "categoria do medidor" para mostrar "insights e análises" (usado por alguns dos tipos de preço herdados) e "Log Analytics" e, em seguida, adicionar um filtro na coluna "ID da instância", que é "contém espaço de trabalho". O uso é mostrado na coluna "quantidade consumida" e a unidade de cada entrada é mostrada na coluna "unidade de medida".  Mais detalhes estão disponíveis para ajudá-lo a [entender sua fatura de Microsoft Azure](https://docs.microsoft.com/azure/billing/billing-understand-your-bill). 

## <a name="changing-pricing-tier"></a>Alterando o tipo de preço

Para alterar o tipo de preço Log Analytics do seu espaço de trabalho, 

1. No portal do Azure, abra **uso e custos estimados** em seu espaço de trabalho, em que você verá uma lista de cada tipo de preço disponível para esse espaço de trabalho.

2. Examine os custos estimados de cada tipo de preço. Essa estimativa se baseia nos últimos 31 dias de uso, portanto, essa estimativa de custo depende dos últimos 31 dias que representam o uso típico. No exemplo abaixo, você pode ver como, com base nos padrões de dados dos últimos 31 dias, esse espaço de trabalho custaria menos na camada pré-paga (#1) em comparação com a camada de reserva de capacidade de 100 GB/dia (#2).  

    ![Tipos de preço](media/manage-cost-storage/pricing-tier-estimated-costs.png)

3. Depois de examinar os custos estimados com base nos últimos 31 dias de uso, se você decidir alterar o tipo de preço, clique em **selecionar**.  

Você também pode [definir o tipo de preço por meio](https://docs.microsoft.com/azure/azure-monitor/platform/template-workspace-configuration#configure-a-log-analytics-workspace) de `sku` Azure Resource Manager usando`pricingTier` o parâmetro (no modelo Azure Resource Manager). 

## <a name="legacy-pricing-tiers"></a>Tipos de preço legados

As assinaturas que tinham um espaço de trabalho Log Analytics ou Application Insights recurso nele antes de 2 de abril de 2018 ou estão vinculadas a uma Enterprise Agreement iniciada antes de 1º de fevereiro de 2019, continuarão a ter acesso para usar os tipos de preço herdados: **gratuito**, **autônomo (por GB)** e **por nó (OMS)**.  Os espaços de trabalho no tipo de preço gratuito terão ingestão de dados diária limitada a 500 MB (exceto os tipos de dados de segurança coletados pela central de segurança do Azure) e a retenção de dados será limitada a 7 dias. O tipo de preço gratuito é destinado apenas para fins de avaliação. Os espaços de trabalho nos tipos de preço autônomos ou por nó têm retenção configurável do usuário de 30 a 730 dias.

O tipo de preço por nó cobra por VM monitorada (nó) em uma granularidade de hora. Para cada nó monitorado, o espaço de trabalho é alocado 500 MB de dados por dia que não é cobrado. Essa alocação é agregada no nível do espaço de trabalho. Os dados ingeridos acima da alocação diária de dados agregados são cobrados por GB como excedentes de dados. Observe que, em sua fatura, o serviço será **Insight and Analytics** para log Analytics uso se o espaço de trabalho estiver no tipo de preço por nó. 

> [!TIP]
> Se o seu espaço de trabalho tiver acesso ao tipo de preço **por nó** , mas você estiver se perguntando se ele seria menor em uma camada paga conforme o uso, você pode [usar a consulta abaixo](#evaluating-the-legacy-per-node-pricing-tier) para obter uma recomendação com facilidade. 

Os espaços de trabalho criados antes de abril de 2016 também podem acessar os tipos de preço Standard e **Premium** **padrão** que têm retenção de dados fixa de 30 a 365 dias, respectivamente. Novos espaços de trabalho não podem ser criados nos tipos de preço **Standard** ou **Premium** e, se um espaço de trabalho for movido para fora dessas camadas, ele não poderá ser movido de volta. 

Mais detalhes sobre as limitações do tipo de preço estão disponíveis [aqui](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#log-analytics-workspaces).

> [!NOTE]
> Para usar os direitos provenientes da aquisição de OMS E1 Suite, OMS E2 Suite OMS ou Complemento do OMS para System Center, escolha o tipo de preço *Por Nó* do Log Analytics.

## <a name="change-the-data-retention-period"></a>Alterar o período de retenção de dados

As etapas a seguir descrevem como configurar por quanto tempo os dados de log são mantidos no workspace. A retenção de dados pode ser configurada de 30 a 730 dias (2 anos) para todos os espaços de trabalho, a menos que eles estejam usando o tipo de preço gratuito herdado. 

### <a name="default-retention"></a>Retenção padrão

Para definir a retenção padrão para seu espaço de trabalho, 
 
1. No portal do Azure, no espaço de trabalho, selecione **uso e custos estimados** no painel esquerdo.
2. Na página **Uso e estimativa de custos**, clique em **Gerenciamento de volume de dados** na parte superior da página.
3. No painel, mova o controle deslizante para aumentar ou diminuir o número de dias e, em seguida, clique em **Salvar**.  Se você usar a camada *Gratuita*, não será possível modificar o período de retenção de dados, sendo necessário atualizar para a camada paga para controlar essa configuração.

    ![Alterar a configuração de retenção de dados do workspace](media/manage-cost-storage/manage-cost-change-retention-01.png)

Quando a retenção é reduzida, há um período de carência de vários dias antes que os dados mais antigos sejam removidos. 
    
A retenção também pode ser [definida por meio](https://docs.microsoft.com/azure/azure-monitor/platform/template-workspace-configuration#configure-a-log-analytics-workspace) de Azure Resource Manager `retentionInDays` usando o parâmetro. Além disso, se você definir a retenção de dados para 30 dias, poderá disparar uma limpeza imediata de dados mais antigos `immediatePurgeDataOn30Days` usando o parâmetro, o que pode ser útil para cenários relacionados à conformidade. Essa funcionalidade só é exposta por meio de Azure Resource Manager. 

Dois tipos de dados- `Usage` - `AzureActivity` e--são retidos por 90 dias por padrão, e não há nenhum custo para essa retenção de 90 dias. Esses tipos de dados também são gratuitos de encargos de ingestão de dados. 



### <a name="retention-by-data-type"></a>Retenção por tipo de dados

Também é possível especificar diferentes configurações de retenção para tipos de dados individuais de 30 a 730 dias (exceto para espaços de trabalho no tipo de preço gratuito herdado). Cada tipo de dados é um subrecurso do espaço de trabalho. Por exemplo, a tabela SecurityEvent pode ser endereçada em [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) como:

```
/subscriptions/00000000-0000-0000-0000-00000000000/resourceGroups/MyResourceGroupName/providers/Microsoft.OperationalInsights/workspaces/MyWorkspaceName/Tables/SecurityEvent
```

Observe que o tipo de dados (tabela) diferencia maiúsculas de minúsculas.  Para obter as configurações de retenção de tipo de dados atuais por um tipo de dados específico (neste exemplo SecurityEvent), use:

```JSON
    GET /subscriptions/00000000-0000-0000-0000-00000000000/resourceGroups/MyResourceGroupName/providers/Microsoft.OperationalInsights/workspaces/MyWorkspaceName/Tables/SecurityEvent?api-version=2017-04-26-preview
```

Para obter as configurações atuais de retenção de tipo de dados para todos os tipos de dados em seu espaço de trabalho, basta omitir o tipo de dados específico, por exemplo:

```JSON
    GET /subscriptions/00000000-0000-0000-0000-00000000000/resourceGroups/MyResourceGroupName/providers/Microsoft.OperationalInsights/workspaces/MyWorkspaceName/Tables?api-version=2017-04-26-preview
```

Para definir a retenção de um tipo de dados específico (neste exemplo, SecurityEvent) a 730 dias, faça

```JSON
    PUT /subscriptions/00000000-0000-0000-0000-00000000000/resourceGroups/MyResourceGroupName/providers/Microsoft.OperationalInsights/workspaces/MyWorkspaceName/Tables/SecurityEvent?api-version=2017-04-26-preview
    {
        "properties": 
        {
            "retentionInDays": 730
        }
    }
```

Os valores válidos `retentionInDays` para são de 30 a 730.

Os `Usage` tipos `AzureActivity` de dados e não podem ser definidos com retenção personalizada. Eles terão o máximo de retenção de espaço de trabalho padrão ou de 90 dias. 

Uma ótima ferramenta para se conectar diretamente ao Azure Resource Manager definir a retenção por tipo de dados é a ferramenta OSS [ARMclient](https://github.com/projectkudu/ARMClient).  Saiba mais sobre o ARMclient de artigos de [David Ebbo](http://blog.davidebbo.com/2015/01/azure-resource-manager-client.html) e [Daniel Bowbyes](https://blog.bowbyes.co.nz/2016/11/02/using-armclient-to-directly-access-azure-arm-rest-apis-and-list-arm-policy-details/).  Aqui está um exemplo usando ARMClient, definindo os dados de SecurityEvent para uma retenção de 730 dias:

```
armclient PUT /subscriptions/00000000-0000-0000-0000-00000000000/resourceGroups/MyResourceGroupName/providers/Microsoft.OperationalInsights/workspaces/MyWorkspaceName/Tables/SecurityEvent?api-version=2017-04-26-preview "{properties: {retentionInDays: 730}}"
```

> [!TIP]
> A configuração da retenção em tipos de dados individuais pode ser usada para reduzir os custos de retenção de dados.  Para os dados coletados a partir de outubro de 2019 (quando esse recurso foi lançado), a redução da retenção para alguns tipos de dados pode reduzir o custo de retenção ao longo do tempo.  Para os dados coletados anteriormente, a definição de uma retenção inferior para um tipo individual não afetará os custos de retenção.  

## <a name="manage-your-maximum-daily-data-volume"></a>Gerenciar seu volume máximo de dados diário

É possível configurar um limite diário e limitar a ingestão diária para o workspace, mas seja cuidadoso, pois sua meta não deve ser atingir o limite diário.  Caso contrário, você perderá os dados no restante do dia, o que pode afetar outros serviços e soluções do Azure cuja funcionalidade pode depender de dados atualizados no workspace.  Como resultado, sua capacidade de observar e receber alertas quando as condições de integridade dos recursos que dão suporte a serviços de TI forem afetadas.  O limite diário destina-se a ser usado como uma maneira de gerenciar o aumento inesperado no volume de dados de seus recursos gerenciados e permanecer dentro do limite ou quando você quiser limitar os encargos não planejados para seu espaço de trabalho.  

Logo após o limite diário ser atingido, a coleção de tipos de dados faturáveis é interrompida para o restante do dia. (A latência inerente à aplicação do limite diário pode significar que o limite não é aplicado exatamente ao nível de limite diário especificado.) Uma faixa de aviso aparece na parte superior da página do espaço de trabalho Log Analytics selecionado e um evento de operação é enviado para a tabela de *operação* em **LogManagement** categoria. A coleta de dados é retomada após o tempo de redefinição definido em *O limite diário será definido em*. É recomendável definir uma regra de alerta com base nesse evento de operação, configurada para notificar quando o limite de dados diários for alcançado. 

> [!WARNING]
> O limite diário não interrompe a coleta de dados da central de segurança do Azure, exceto para espaços de trabalho nos quais a central de segurança do Azure foi instalada antes de 19 de junho de 2017. 

### <a name="identify-what-daily-data-limit-to-define"></a>Identificar o limite diário de dados a definir

Analise [Uso do Log Analytics e custos estimados](usage-estimated-costs.md) para reconhecer a tendência de ingestão de dados e qual é o limite diário de volume a ser definido. Isso deve ser considerado com cuidado, já que você ganhou? t ser capaz de monitorar seus recursos depois que o limite for atingido. 

### <a name="set-the-daily-cap"></a>Definir o limite diário

As etapas a seguir descrevem como configurar um limite para gerenciar o volume de dados que Log Analytics espaço de trabalho será ingerido por dia.  

1. No workspace, selecione **Uso e custos estimados** no painel esquerdo.
2. Na página **Uso e custos estimados** para o workspace selecionado, clique em **Gerenciamento de volume de dados** na parte superior da página. 
3. O limite diário está **desativado** por padrão? Clique **em ativado** para habilitá-lo e defina o limite de volume de dados em GB/dia.

    ![Configuração do limite de dados do Log Analytics](media/manage-cost-storage/set-daily-volume-cap-01.png)

### <a name="alert-when-daily-cap-reached"></a>Alertar quando o limite diário for atingido

Embora uma indicação visual seja apresentada no Portal do Azure quando o limite de dados é alcançado, esse comportamento não alinha-se necessariamente à maneira como você gerencia problemas operacionais que exigem atenção imediata.  Para receber uma notificação de alerta, é possível criar uma nova regra de alerta no Azure Monitor.  Para saber mais, consulte [como criar, exibir e gerenciar alertas](alerts-metric.md).

Para começar, aqui estão as configurações recomendadas para o alerta:

- Destino: selecione o recurso do Log Analytics
- Critérios: 
   - Nome do sinal: pesquisa de registro personalizada
   - Consulta de pesquisa: operação | onde o Detalhe tem 'OverQuota'
   - Baseado em: número de resultados
   - Condição: maior do que
   - Limite: 0
   - Período: 5 (minutos)
   - Frequência: 5 (minutos)
- Nome da regra de alerta: limite de dados diários alcançado
- Gravidade: aviso (Grav 1)

Quando o alerta é definido e o limite é alcançado, um alerta é disparado e executa a resposta definida no Grupo de Ações. Ele pode notificar a equipe por email e mensagens de texto, ou automatizar ações usando webhooks, runbooks de Automação ou [integrando com uma solução ITSM externa](itsmc-overview.md#create-itsm-work-items-from-azure-alerts). 

## <a name="troubleshooting-why-usage-is-higher-than-expected"></a>Solução de problemas por uso acima do esperado

O uso aumenta devido a uma ou mais das seguintes causas:
- Mais nós do que o esperado enviando dados para Log Analytics espaço de trabalho
- Mais dados do que o esperado que está sendo enviado para Log Analytics espaço de trabalho (talvez devido ao início do uso de uma nova solução ou uma alteração de configuração para uma solução existente)

## <a name="understanding-nodes-sending-data"></a>Noções básicas sobre nós enviando dados

Para entender o número de nós que relatam pulsações do agente a cada dia no último mês, use

```kusto
Heartbeat 
| where TimeGenerated > startofday(ago(31d))
| summarize nodes = dcount(Computer) by bin(TimeGenerated, 1d)    
| render timechart
```
O obter uma contagem de nós enviando dados nas últimas 24 horas use a consulta: 

```kusto
union withsource = tt * 
| where TimeGenerated > ago(24h)
| extend computerName = tolower(tostring(split(Computer, '.')[0]))
| where computerName != ""
| summarize nodes = dcount(computerName)
```

Para obter uma lista de nós que enviam quaisquer dados (e a quantidade de dados enviados por cada um), a seguinte consulta pode ser usada:

```kusto
union withsource = tt * 
| where TimeGenerated > ago(24h)
| extend computerName = tolower(tostring(split(Computer, '.')[0]))
| where computerName != ""
| summarize TotalVolumeBytes=sum(_BilledSize) by computerName
```

> [!TIP]
> Use essas `union withsource = tt *` consultas com moderação, pois as verificações entre os tipos de dados são [intensivas em recursos](https://docs.microsoft.com/azure/azure-monitor/log-query/query-optimization#query-performance-pane) para serem executadas. Essa consulta substitui a maneira antiga de consultar informações por computador com o tipo de dados de uso.  

## <a name="understanding-ingested-data-volume"></a>Compreendendo o volume de dados ingeridos

Na página **Uso e custos estimados**, o gráfico *Ingestão de dados por solução* mostra o volume total de dados enviados e quanto está sendo enviado por cada solução. Isso permite determinar tendências, como se o uso geral de dados (ou uso por uma solução específica) está crescendo, permanecendo estável ou diminuindo. 

### <a name="data-volume-for-specific-events"></a>Volume de dados para eventos específicos

Para examinar o tamanho dos dados ingeridos de um determinado conjunto de eventos, você pode consultar a tabela específica (neste exemplo `Event`) e, em seguida, restringir a consulta aos eventos de interesse (neste exemplo, ID do evento 5145 ou 5156):

```kusto
Event
| where TimeGenerated > startofday(ago(31d)) and TimeGenerated < startofday(now()) 
| where EventID == 5145 or EventID == 5156
| where _IsBillable == true
| summarize count(), Bytes=sum(_BilledSize) by EventID, bin(TimeGenerated, 1d)
``` 

Observe que a cláusula `where IsBillable = true` filtra os tipos de dados de determinadas soluções para as quais não há nenhum encargo de ingestão. 

### <a name="data-volume-by-solution"></a>Volume de dados por solução

A consulta usada para exibir o volume de dados faturáveis por solução no último mês (exceto o último dia parcial) é:

```kusto
Usage 
| where TimeGenerated > ago(32d)
| where StartTime >= startofday(ago(31d)) and EndTime < startofday(now())
| where IsBillable == true
| summarize BillableDataGB = sum(Quantity) / 1000. by bin(StartTime, 1d), Solution | render barchart
```

A cláusula WITH `TimeGenerated` é apenas para garantir que a experiência de consulta no portal do Azure seja exibida fora do padrão 24 horas. Ao usar o tipo de dados de `StartTime` uso `EndTime` e representar os buckets de tempo para os quais os resultados são apresentados. 

### <a name="data-volume-by-type"></a>Volume de dados por tipo

Você pode analisar mais detalhadamente para ver as tendências de dados por tipo de dados:

```kusto
Usage 
| where TimeGenerated > ago(32d)
| where StartTime >= startofday(ago(31d)) and EndTime < startofday(now())
| where IsBillable == true
| summarize BillableDataGB = sum(Quantity) / 1000. by bin(StartTime, 1d), DataType | render barchart
```

Ou para ver uma tabela por solução e tipo para o último mês,

```kusto
Usage 
| where TimeGenerated > ago(32d)
| where StartTime >= startofday(ago(31d)) and EndTime < startofday(now())
| where IsBillable == true
| summarize BillableDataGB = sum(Quantity) by Solution, DataType
| sort by Solution asc, DataType asc
```

### <a name="data-volume-by-computer"></a>Volume de dados por computador

O `Usage` tipo de dados não inclui informações no nível de preenchimento. Para ver o **tamanho** dos dados ingeridos por computador, use `_BilledSize` a [Propriedade](log-standard-properties.md#_billedsize), que fornece o tamanho em bytes:

```kusto
union withsource = tt * 
| where TimeGenerated > ago(24h)
| where _IsBillable == true 
| extend computerName = tolower(tostring(split(Computer, '.')[0]))
| summarize BillableDataBytes = sum(_BilledSize) by  computerName | sort by Bytes nulls last
```

A `_IsBillable` [Propriedade](log-standard-properties.md#_isbillable) especifica se os dados ingeridos incorrerão em encargos.

Para ver a **contagem** de eventos faturáveis ingeridos por computador, use 

```kusto
union withsource = tt * 
| where TimeGenerated > ago(24h)
| where _IsBillable == true 
| extend computerName = tolower(tostring(split(Computer, '.')[0]))
| summarize eventCount = count() by computerName  | sort by eventCount nulls last
```

### <a name="data-volume-by-azure-resource-resource-group-or-subscription"></a>Volume de dados por recurso do Azure, grupo de recursos ou assinatura

Para dados de nós hospedados no Azure, você pode obter o **tamanho** dos dados ingeridos __por computador__, usar a [Propriedade](log-standard-properties.md#_resourceid)_ResourceId, que fornece o caminho completo para o recurso:

```kusto
union withsource = tt * 
| where TimeGenerated > ago(24h)
| where _IsBillable == true 
| summarize BillableDataBytes = sum(_BilledSize) by _ResourceId | sort by Bytes nulls last
```

Para dados de nós hospedados no Azure, você pode obter o **tamanho** dos dados ingeridos __por assinatura do Azure__, analisar a `_ResourceId` Propriedade como:

```kusto
union withsource = tt * 
| where TimeGenerated > ago(24h)
| where _IsBillable == true 
| parse tolower(_ResourceId) with "/subscriptions/" subscriptionId "/resourcegroups/" 
    resourceGroup "/providers/" provider "/" resourceType "/" resourceName   
| summarize BillableDataBytes = sum(_BilledSize) by subscriptionId | sort by Bytes nulls last
```

A `subscriptionId` alteração `resourceGroup` para mostrará o volume de dados ingeridos Faturável pelo grupo de recursos do Azure. 

> [!WARNING]
> Alguns dos campos do tipo de dados Uso, ainda no esquema, foram reprovados e seus valores não serão mais preenchidos. Estes são **Computador**, bem como campos relacionados à ingestão (**TotalBatches**, **BatchesWithinSla**, **BatchesOutsideSla**, **BatchesCapped** e **AverageProcessingTimeMs**.

### <a name="querying-for-common-data-types"></a>Consultando tipos de dados comuns

Para aprofundar a fonte de dados de um determinado tipo de dados, aqui estão algumas consultas de exemplo úteis:

+ Solução de **Segurança**
  - `SecurityEvent | summarize AggregatedValue = count() by EventID`
+ Solução do **Gerenciamento de log**
  - `Usage | where Solution == "LogManagement" and iff(isnotnull(toint(IsBillable)), IsBillable == true, IsBillable == "true") == true | summarize AggregatedValue = count() by DataType`
+ Tipo de dados de **Desempenho**
  - `Perf | summarize AggregatedValue = count() by CounterPath`
  - `Perf | summarize AggregatedValue = count() by CounterName`
+ Tipo de dados de **Evento**
  - `Event | summarize AggregatedValue = count() by EventID`
  - `Event | summarize AggregatedValue = count() by EventLog, EventLevelName`
+ Tipo de dados **Syslog**
  - `Syslog | summarize AggregatedValue = count() by Facility, SeverityLevel`
  - `Syslog | summarize AggregatedValue = count() by ProcessName`
+ Tipo de dados **AzureDiagnostics**
  - `AzureDiagnostics | summarize AggregatedValue = count() by ResourceProvider, ResourceId`

### <a name="tips-for-reducing-data-volume"></a>Dicas para reduzir o volume de dados

Algumas sugestões para reduzir o volume de logs coletados incluem:

| Origem do alto volume de dados | Como reduzir o volume de dados |
| -------------------------- | ------------------------- |
| Eventos de segurança            | Selecione [eventos de segurança mínima ou comuns](https://docs.microsoft.com/azure/security-center/security-center-enable-data-collection#data-collection-tier) <br> Alterar a política de auditoria de segurança para coletar somente eventos necessários. Em particular, examine a necessidade para coletar eventos para <br> - [auditoria de plataforma de filtragem](https://technet.microsoft.com/library/dd772749(WS.10).aspx) <br> - [auditoria de registro](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd941614(v%3dws.10))<br> - [auditoria de sistema de arquivos](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd772661(v%3dws.10))<br> - [auditoria de objeto kernel](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd941615(v%3dws.10))<br> - [auditoria de manipulação de identificador](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd772626(v%3dws.10))<br> - auditoria de armazenamento removível |
| Contadores de desempenho       | Altere a [configuração do contador de desempenho](data-sources-performance-counters.md) para: <br> - Reduzir a frequência de coleta <br> - Reduzir o número de contadores de desempenho |
| Logs de eventos                 | Altere a [configuração de log de eventos](data-sources-windows-events.md) para: <br> - Reduzir o número de logs de eventos coletados <br> - Coletar somente níveis de eventos necessários. Por exemplo, não colete eventos de nível *informações* |
| syslog                     | Altere a [configuração do syslog](data-sources-syslog.md) para: <br> - Reduzir o número de instalações coletadas <br> - Coletar somente níveis de eventos necessários. Por exemplo, não coletar eventos de nível *Informações* e *Depurar* |
| AzureDiagnostics           | Altere a coleção de logs do recurso para: <br> - Reduzir o número de logs de envio de recursos para o Log Analytics <br> - Coletar somente os logs necessários |
| Dados da solução de computadores que não precisam da solução | Use [direcionamento de solução](../insights/solution-targeting.md) para coletar dados somente dos grupos de computadores necessários. |

### <a name="getting-nodes-as-billed-in-the-per-node-pricing-tier"></a>Obtendo nós como cobrados no tipo de preço por nó

Para obter uma lista de computadores que serão cobrados como nós se o espaço de trabalho estiver no tipo de preço herdado por nó, procure os nós que estão enviando **tipos de dados cobrados** (alguns tipos de dados são gratuitos). Para fazer isso, use a `_IsBillable` [Propriedade](log-standard-properties.md#_isbillable) e use o campo mais à esquerda do nome de domínio totalmente qualificado. Isso retorna a contagem de computadores com dados cobrados por hora (que é a granularidade na qual os nós são contados e cobrados):

```kusto
union withsource = tt * 
| where _IsBillable == true 
| extend computerName = tolower(tostring(split(Computer, '.')[0]))
| where computerName != ""
| summarize billableNodes=dcount(computerName) by bin(TimeGenerated, 1h) | sort by TimeGenerated asc
```

### <a name="getting-security-and-automation-node-counts"></a>Obtendo contagens de nós de segurança e automação

Se você estiver no nível de preço "Por nó (OMS)", será cobrado com base no número de nós e soluções usados, o número de nós do Insights e do Analytics para os quais você está sendo faturado será mostrado na tabela do **Uso e custo estimado**.  

Para ver o número de nós de segurança distintos, você pode usar a consulta:

```kusto
union
(
    Heartbeat
    | where (Solutions has 'security' or Solutions has 'antimalware' or Solutions has 'securitycenter')
    | project Computer
),
(
    ProtectionStatus
    | where Computer !in~
    (
        (
            Heartbeat
            | project Computer
        )
    )
    | project Computer
)
| distinct Computer
| project lowComputer = tolower(Computer)
| distinct lowComputer
| count
```

Para ver o número de nós de automação distintos, use a consulta:

```kusto
 ConfigurationData 
 | where (ConfigDataType == "WindowsServices" or ConfigDataType == "Software" or ConfigDataType =="Daemons") 
 | extend lowComputer = tolower(Computer) | summarize by lowComputer 
 | join (
     Heartbeat 
       | where SCAgentChannel == "Direct"
       | extend lowComputer = tolower(Computer) | summarize by lowComputer, ComputerEnvironment
 ) on lowComputer
 | summarize count() by ComputerEnvironment | sort by ComputerEnvironment asc
```

## <a name="evaluating-the-legacy-per-node-pricing-tier"></a>Avaliando o tipo de preço herdado por nó

A decisão de saber se os espaços de trabalho com acesso ao tipo de preço herdado **por nó** estão melhor nessa camada ou em uma camada de **reserva de capacidade** **paga conforme o uso** ou em geral é difícil para os clientes avaliarem.  Isso envolve entender a compensação entre o custo fixo por nó monitorado no tipo de preço por nó e sua alocação de dados incluída de 500 MB/nó/dia e o custo de apenas pagar por dados ingeridos na camada pago pelo uso (por GB). 

Para facilitar essa avaliação, a consulta a seguir pode ser usada para fazer uma recomendação para o tipo de preço ideal com base nos padrões de uso de um espaço de trabalho.  Essa consulta examina os nós monitorados e os dados ingeridos em um espaço de trabalho nos últimos 7 dias e, para cada dia, avalia qual tipo de preço seria ideal. Para usar a consulta, você precisa especificar se o espaço de trabalho está usando a central de segurança `workspaceHasSecurityCenter` do `true` Azure `false`definindo como ou e, em seguida (opcionalmente) atualizando os preços por nó e por GB que seu Organizaiton recebe. 

```kusto
// Set these parameters before running query
let workspaceHasSecurityCenter = true;  // Specify if the workspace has Azure Security Center
let PerNodePrice = 15.; // Enter your montly price per monitored nodes
let PerGBPrice = 2.30; // Enter your price per GB 
// ---------------------------------------
let SecurityDataTypes=dynamic(["SecurityAlert", "SecurityBaseline", "SecurityBaselineSummary", "SecurityDetection", "SecurityEvent", "WindowsFirewall", "MaliciousIPCommunication", "LinuxAuditLog", "SysmonEvent", "ProtectionStatus", "WindowsEvent", "Update", "UpdateSummary"]);
union withsource = tt * 
| where TimeGenerated >= startofday(now(-7d)) and TimeGenerated < startofday(now())
| extend computerName = tolower(tostring(split(Computer, '.')[0]))
| where computerName != ""
| summarize nodesPerHour = dcount(computerName) by bin(TimeGenerated, 1h)  
| summarize nodesPerDay = sum(nodesPerHour)/24.  by day=bin(TimeGenerated, 1d)  
| join (
    Heartbeat 
    | where TimeGenerated >= startofday(now(-7d)) and TimeGenerated < startofday(now())
    | where Computer != ""
    | summarize ASCnodesPerHour = dcount(Computer) by bin(TimeGenerated, 1h) 
    | extend ASCnodesPerHour = iff(workspaceHasSecurityCenter, ASCnodesPerHour, 0)
    | summarize ASCnodesPerDay = sum(ASCnodesPerHour)/24.  by day=bin(TimeGenerated, 1d)   
) on day
| join (
    Usage 
    | where TimeGenerated > ago(8d)
    | where StartTime >= startofday(now(-7d)) and EndTime < startofday(now())
    | where IsBillable == true
    | extend NonSecurityData = iff(DataType !in (SecurityDataTypes), Quantity, 0.)
    | extend SecurityData = iff(DataType in (SecurityDataTypes), Quantity, 0.)
    | summarize DataGB=sum(Quantity)/1000., NonSecurityDataGB=sum(NonSecurityData)/1000., SecurityDataGB=sum(SecurityData)/1000. by day=bin(StartTime, 1d)  
) on day
| extend AvgGbPerNode =  NonSecurityDataGB / nodesPerDay
| extend PerGBDailyCost = iff(workspaceHasSecurityCenter,
             (NonSecurityDataGB + max_of(SecurityDataGB - 0.5*ASCnodesPerDay, 0.)) * PerGBPrice,
             DataGB * PerGBPrice)
| extend OverageGB = iff(workspaceHasSecurityCenter, 
             max_of(DataGB - 0.5*nodesPerDay - 0.5*ASCnodesPerDay, 0.), 
             max_of(DataGB - 0.5*nodesPerDay, 0.))
| extend PerNodeDailyCost = nodesPerDay * PerNodePrice / 31. + OverageGB * PerGBPrice
| extend Recommendation = iff(PerNodeDailyCost < PerGBDailyCost, "Per Node tier", 
             iff(NonSecurityDataGB > 85., "Capacity Reservation tier", "Pay-as-you-go (Per GB) tier"))
| project day, nodesPerDay, ASCnodesPerDay, NonSecurityDataGB, SecurityDataGB, OverageGB, AvgGbPerNode, PerGBDailyCost, PerNodeDailyCost, Recommendation | sort by day asc
| project day, Recommendation // Comment this line to see details
| sort by day asc
```

Essa consulta não é uma replicação exata de como o uso é calculado, mas funcionará para fornecer recomendações de tipo de preço na maioria dos casos.  

## <a name="create-an-alert-when-data-collection-is-high"></a>Criar um alerta quando a coleta de dados estiver alta

Este artigo descreve como criar um alerta quando:
- O volume de dados excede uma quantidade definida.
- A previsão do volume de dados excede uma quantidade definida.

Os alertas do Azure oferecem suporte a [alertas de log](alerts-unified-log.md) que usam consultas de pesquisa. 

A consulta abaixo tem um resultado quando há mais de 100 GB de dados coletados nas últimas 24 horas:

```kusto
union withsource = $table Usage 
| where QuantityUnit == "MBytes" and iff(isnotnull(toint(IsBillable)), IsBillable == true, IsBillable == "true") == true 
| extend Type = $table | summarize DataGB = sum((Quantity / 1000.)) by Type 
| where DataGB > 100
```

A consulta abaixo usa uma fórmula simples para prever quando mais de 100 GB de dados serão enviados em um mesmo dia: 

```kusto
union withsource = $table Usage 
| where QuantityUnit == "MBytes" and iff(isnotnull(toint(IsBillable)), IsBillable == true, IsBillable == "true") == true 
| extend Type = $table 
| summarize EstimatedGB = sum(((Quantity * 8) / 1000.)) by Type 
| where EstimatedGB > 100
```

Para receber um alerta em caso de volume de dados diferente, altere o número 100 nas consultas para o número de GB que deve disparar um alerta.

Use as etapas descritas em [criar um novo alerta de log](alerts-metric.md) para ser notificado quando a coleta de dados for maior que a esperada.

Ao criar o alerta para a primeira consulta, quando há mais de 100 GB de dados em 24 horas, defina:  

- **Definir condição de alerta** especifica seu espaço de trabalho do Log Analytics como o destino do recurso.
- **Critérios de alerta** especificam o seguinte:
   - **Nome do sinal** selecione **pesquisa de logs personalizada**
   - **Consulta de pesquisa** como `union withsource = $table Usage | where QuantityUnit == "MBytes" and iff(isnotnull(toint(IsBillable)), IsBillable == true, IsBillable == "true") == true | extend Type = $table | summarize DataGB = sum((Quantity / 1000.)) by Type | where DataGB > 100`
   - **Lógica de alerta** é **Baseada em** *número de resultados* e **Condição** é *maior* que um **Limite ** de *0*
   - **Período de tempo** de *1440* minutos e **Frequência de alerta** a cada *60* minutos uma vez que os dados de uso são atualizados somente uma vez por hora.
- **Definir os detalhes do alerta** especifica o seguinte:
   - O **Nome** como *Volume de dados maior que 100 GB em 24 horas*
   - A **Gravidade** como *Aviso*

Especifique um já existente ou crie um novo [Grupo de ação](action-groups.md) para que, quando o alerta de log corresponder aos critérios, você seja notificado.

Ao criar o alerta para a segunda consulta, quando existe a previsão de que haverá mais de 100 GB de dados em 24 horas, defina:

- **Definir condição de alerta** especifica seu espaço de trabalho do Log Analytics como o destino do recurso.
- **Critérios de alerta** especificam o seguinte:
   - **Nome do sinal** selecione **pesquisa de logs personalizada**
   - **Consulta de pesquisa** como `union withsource = $table Usage | where QuantityUnit == "MBytes" and iff(isnotnull(toint(IsBillable)), IsBillable == true, IsBillable == "true") == true | extend Type = $table | summarize EstimatedGB = sum(((Quantity * 8) / 1000.)) by Type | where EstimatedGB > 100`
   - **Lógica de alerta** é **Baseada em** *número de resultados* e **Condição** é *maior* que um **Limite ** de *0*
   - **Período de tempo** de *180* minutos e **Frequência de alerta** a cada *60* minutos uma vez que os dados de uso são atualizados somente uma vez por hora.
- **Definir os detalhes do alerta** especifica o seguinte:
   - O **Nome** como *Previsão de volume de dados maior que 100 GB em 24 horas*
   - A **Gravidade** como *Aviso*

Especifique um já existente ou crie um novo [Grupo de ação](action-groups.md) para que, quando o alerta de log corresponder aos critérios, você seja notificado.

Ao receber um alerta, use as etapas na seção a seguir para solucionar problemas relativos ao uso acima do esperado.

## <a name="data-transfer-charges-using-log-analytics"></a>Cobranças de transferência de dados usando Log Analytics

Enviar dados para Log Analytics pode incorrer em encargos de largura de banda de dados. Conforme descrito na [página de preços de largura de banda do Azure](https://azure.microsoft.com/pricing/details/bandwidth/), a transferência de dados entre os serviços do Azure localizados em duas regiões cobradas como transferência de dados de saída na taxa normal. A transferência de dados de entrada é gratuita. No entanto, esse encargo é muito pequeno (alguns%) em comparação com os custos de ingestão de dados Log Analytics. Consequentemente, controlar os custos de Log Analytics precisa se concentrar no volume de dados ingerido e temos diretrizes para ajudar a entender isso [aqui](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#understanding-ingested-data-volume).   


## <a name="troubleshooting-why-log-analytics-is-no-longer-collecting-data"></a>Solucionar problemas se o Log Analytics não está mais coletando dados

Se você estiver usando o tipo de preço gratuito herdado e tiver enviado mais de 500 MB de dados em um dia, a coleta de dados será interrompida pelo restante do dia. Alcançar o limite diário é um motivo comum para o Log Analytics parar de coletar dados ou para dados parecerem estar ausentes.  O Log Analytics cria um evento de tipo Operação quando a coleta de dados inicia e para. Execute a seguinte consulta na pesquisa para verificar se você está atingindo o limite diário e perdendo dados: 

```kusto
Operation | where OperationCategory == 'Data Collection Status'
```

Quando a coleta de dados é interrompida, o OperationStatus é **aviso**. Quando a coleta de dados é iniciada, o OperationStatus é **bem-sucedido**. A tabela a seguir descreve os motivos pelos quais a coleta de dados é interrompida e uma ação é sugerida para retomar a coleta de dados:  

|Motivo para a interrupção da coleta| Solução| 
|-----------------------|---------|
|Limite diário de tipo de preço gratuito herdado atingido |Aguarde até o dia seguinte para que a coleta seja reiniciada automaticamente ou altere para um tipo de preço pago.|
|O limite diário do seu workspace foi atingido|Aguarde para a coleta ser reiniciada automaticamente ou aumente o limite diário de volume de dados descrito em Gerenciar o volume máximo de dados diário. O tempo de redefinição de limite diário é exibido na página **Gerenciamento de volume de dados**. |
|Assinatura do Azure está em um estado suspenso devido a:<br> A avaliação gratuita terminou<br> O Azure Pass expirou<br> Limite de gastos mensal atingido (por exemplo, em uma assinatura do MSDN ou do Visual Studio)|Converter para uma assinatura paga<br> Remova o limite ou espere o limite ser redefinido|

Para ser notificado quando a coleta de dados for interrompida, use as etapas descritas em criar alerta de *limite de dados diários* para ser notificado quando a coleta de dados parar. Use as etapas descritas em [criar um grupo de ações](action-groups.md) para configurar uma ação de email, webhook ou runbook para a regra de alerta. 

## <a name="limits-summary"></a>Resumo de limites

Há alguns limites de Log Analytics adicionais, alguns dos quais dependem do tipo de preço Log Analytics. Eles estão documentados [aqui](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#log-analytics-workspaces).


## <a name="next-steps"></a>Próximas etapas

- Consulte [pesquisas de log em logs de Azure monitor](../log-query/log-query-overview.md) para saber como usar o idioma de pesquisa. Você pode usar consultas de pesquisa para executar outras análises nos dados de uso.
- Use as etapas descritas em [criar um alerta de log](alerts-metric.md) para ser notificado quando um critério de pesquisa for atendido.
- Use [direcionamento de solução](../insights/solution-targeting.md) para coletar dados somente dos grupos de computadores necessários.
- Para configurar uma política de coleta de eventos eficaz, confira a [Política de filtragem da Central de Segurança do Azure](../../security-center/security-center-enable-data-collection.md).
- Altere a [configuração do contador de desempenho](data-sources-performance-counters.md).
- Para modificar as configurações de coleta de eventos, examine a [configuração do log de eventos](data-sources-windows-events.md).
- Para modificar as configurações da coleção do syslog, examine [configuração do syslog](data-sources-syslog.md).

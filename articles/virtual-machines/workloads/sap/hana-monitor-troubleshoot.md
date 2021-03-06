---
title: Solução de problemas e monitoramento do HANA no SAP HANA no Azure (Instâncias Grandes) | Microsoft Docs
description: Solução de problemas e monitoramento do HANA no SAP HANA no Azure (Instâncias Grandes).
services: virtual-machines-linux
documentationcenter: ''
author: msjuergent
manager: bburns
editor: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/10/2018
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 047ea4d07f2b497ac8c7deb90c056d63976094f4
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/09/2020
ms.locfileid: "77617070"
---
# <a name="monitoring-and-troubleshooting-from-hana-side"></a>Monitoramento e solução de problemas no lado do HANA

Para analisar efetivamente os problemas relacionados ao SAP HANA no Azure (Instâncias Grandes), é útil restringir a causa raiz do problema. O SAP publicou uma grande quantidade de documentação para ajudar você.

Perguntas frequentes aplicáveis relacionadas ao desempenho do SAP HANA podem ser encontradas nas seguintes Notas SAP a seguir:

- [Nota SAP nº 2222200 – Perguntas frequentes: Rede SAP HANA](https://launchpad.support.sap.com/#/notes/2222200)
- [Nota SAP nº 2100040 – Perguntas frequentes: CPU SAP HANA](https://launchpad.support.sap.com/#/notes/0002100040)
- [Nota SAP nº 199997 – Perguntas frequentes: Memória SAP HANA](https://launchpad.support.sap.com/#/notes/2177064)
- [Nota SAP nº 200000 – Perguntas frequentes: Otimização de desempenho SAP HANA](https://launchpad.support.sap.com/#/notes/2000000)
- [Nota SAP nº 199930 – Perguntas frequentes: Análise de E/S SAP HANA](https://launchpad.support.sap.com/#/notes/1999930)
- [Nota da SAP #2177064 – Perguntas frequentes: Reinicialização e falhas do serviço SAP HANA](https://launchpad.support.sap.com/#/notes/2177064)

## <a name="sap-hana-alerts"></a>Alertas do SAP HANA

Como uma primeira etapa, verifique os logs de alerta atuais do SAP HANA. No SAP HANA Studio, acesse **console de administração: alertas: mostrar: todos os alertas**. Esta guia mostrará todos os alertas do SAP HANA para valores específicos (memória física livre, utilização de CPU etc.) que estão fora dos limites mínimo e máximo definidos. Por padrão, as verificações são atualizados automaticamente a cada 15 minutos.

![No SAP HANA Studio, acesse Console de Administração: Alertas: Mostrar: todos os alertas](./media/troubleshooting-monitoring/image1-show-alerts.png)

## <a name="cpu"></a>CPU

Para um alerta disparado devido à configuração incorreta do limite, uma resolução é redefinir para o valor padrão ou um valor de limite mais razoável.

![Redefinir para o valor padrão ou para um valor de limite mais razoável](./media/troubleshooting-monitoring/image2-cpu-utilization.png)

Os alertas a seguir podem indicar problemas de recursos de CPU:

- Uso de CPU Host (Alerta 5)
- Operação de ponto de salvamento mais recente (Alerta 28)
- Duração do ponto de salvamento (Alerta 54)

Talvez você perceba o alto consumo de CPU em seu banco de dados SAP HANA em um dos seguintes:

- O Alerta 5 (Uso de CPU Host) é emitido para o uso de CPU atual ou anterior
- O uso de CPU exibido na tela de visão geral

![Uso de CPU exibido na tela de visão geral](./media/troubleshooting-monitoring/image3-cpu-usage.png)

O grafo de Carregamento pode mostrar alto consumo de CPU ou alto consumo anterior:

![O grafo de Carregamento pode mostrar alto consumo de CPU ou alto consumo anterior](./media/troubleshooting-monitoring/image4-load-graph.png)

Um alerta disparado devido à alta utilização da CPU pode ser causado por vários motivos, incluindo, mas não se limitando a: execução de determinadas transações, carregamento de dados, trabalhos que não estão respondendo, instruções SQL de longa execução e desempenho de consulta inadequado (por exemplo, com BW em cubos HANA).

Consulte o site [Solução de problemas do SAP HANA: causas e soluções relacionadas à CPU](https://help.sap.com/saphelp_hanaplatform/helpdata/en/4f/bc915462db406aa2fe92b708b95189/content.htm?frameset=/en/db/6ca50424714af8b370960c04ce667b/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=46&amp;show_children=false) para obter etapas de solução de problemas detalhadas.

## <a name="operating-system"></a>Sistema operacional

Uma das verificações mais importantes para o SAP HANA no Linux é certificar-se de que Transparent Huge Pages estejam desabilitadas, consulte a [Nota SAP nº 2131662 – THP (Transparent Huge Pages) em servidores SAP HANA](https://launchpad.support.sap.com/#/notes/2131662).

- Você pode verificar se o Transparent Huge Pages está habilitado por meio do seguinte comando do Linux: **cat /sys/kernel/mm/transparent\_hugepage/enabled**
- Se _always_ estiver entre colchetes, conforme mostrado a seguir, significa que Transparent Huge Pages está habilitado: [always] madvise never; se _never_ estiver entre colchetes, conforme mostrado a seguir, significa que Transparent Huge Pages está desabilitado: always madvise [never]

O comando Linux a seguir não deve retornar nada: **rpm -qa | grep ulimit.** Se parecer que _ulimit_ está instalado, desinstale-o imediatamente.

## <a name="memory"></a>Memória

Você pode observar que a quantidade de memória alocada pelo banco de dados SAP HANA é maior do que o esperado. Os alertas a seguir indicam problemas com alto uso da memória:

- Uso de memória do host físico (Alerta 1)
- Uso de memória do servidor de nomes (Alerta 12)
- Uso total da memória das tabelas do Repositório de Colunas (Alerta 40)
- Uso de memória dos serviços (Alerta 43)
- Uso de memória do armazenamento principal das tabelas do Repositório de Colunas (Alerta 45)
- Arquivos de despejo em runtime (Alerta 46)

Consulte o site [Solução de problemas do SAP HANA: problemas de memória](https://help.sap.com/saphelp_hanaplatform/helpdata/en/db/6ca50424714af8b370960c04ce667b/content.htm?frameset=/en/59/5eaa513dde43758b51378ab3315ebb/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=26&amp;show_children=false) para obter etapas de solução de problemas detalhadas.

## <a name="network"></a>Rede

Consulte a [Nota SAP nº 2081065 – Solução de problemas de rede do SAP HANA](https://launchpad.support.sap.com/#/notes/2081065) e execute as etapas de solução de problemas de rede desta Nota SAP.

1. Análise do tempo de ida e volta entre o cliente e servidor.
  a. Execute o script SQL [_HANA\_Rede\_Clientes_](https://launchpad.support.sap.com/#/notes/1969700)_._
  
2. Análise da comunicação entre nós.
  a. Execute o script SQL [_HANA\_Rede\_Serviços_](https://launchpad.support.sap.com/#/notes/1969700)_._

3. Execute o comando Linux **ifconfig** (a saída mostrará se ocorre quaisquer perdas de pacote).
4. Execute o comando Linux **tcpdump**.

Além disso, use a ferramenta [IPERF](https://iperf.fr/) de código-fonte aberto (ou semelhante) para medir o desempenho real da rede do aplicativo.

Consulte o site [Solução de problemas do SAP HANA: problemas de desempenho e conectividade](https://help.sap.com/saphelp_hanaplatform/helpdata/en/a3/ccdff1aedc4720acb24ed8826938b6/content.htm?frameset=/en/dc/6ff98fa36541e997e4c719a632cbd8/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=142&amp;show_children=false) para obter etapas de solução de problemas detalhadas.

## <a name="storage"></a>Armazenamento

Do ponto de vista do usuário final, um aplicativo (ou o sistema como um todo) é executado de forma lenta, não responde ou pode até mesmo parecer parar de responder se houver problemas com O desempenho de e/s. Na guia **Volumes** no SAP HANA Studio, você pode ver os volumes conectados e quais volumes são usados por cada serviço.

![Na guia Volumes no SAP HANA Studio, você pode ver os volumes conectados e quais volumes são usados por cada serviço](./media/troubleshooting-monitoring/image5-volumes-tab-a.png)

Você pode ver detalhes dos volumes conectados na parte inferior da tela, como arquivos e estatísticas de E/S.

![Você pode ver detalhes dos volumes conectados na parte inferior da tela, como arquivos e estatísticas de E/S](./media/troubleshooting-monitoring/image6-volumes-tab-b.png)

Consulte os sites [Solução de problemas de SAP HANA: Principais causas e soluções relacionadas à E/S](https://help.sap.com/saphelp_hanaplatform/helpdata/en/dc/6ff98fa36541e997e4c719a632cbd8/content.htm?frameset=/en/47/4cb08a715c42fe9f7cc5efdc599959/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=55&amp;show_children=false) e [Solução de problemas de SAP HANA: Principais causas e soluções relacionadas ao disco](https://help.sap.com/saphelp_hanaplatform/helpdata/en/47/4cb08a715c42fe9f7cc5efdc599959/content.htm?frameset=/en/44/3e1db4f73d42da859008df4f69e37a/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=53&amp;show_children=false) para obter as etapas de solução de problemas detalhadas.

## <a name="diagnostic-tools"></a>Ferramentas de Diagnóstico

Execute uma Verificação de Integridade do SAP HANA por meio do HANA\_Configuration\_Minichecks. Essa ferramenta retorna possíveis problemas técnicos críticos que devem já ter sido gerados como alertas no SAP HANA Studio.

Consulte [Nota SAP nº1969700 – Coleta de instrução SQL para SAP HANA](https://launchpad.support.sap.com/#/notes/1969700) e baixe o arquivo SQL Statements.zip anexado à nota. Armazene esse arquivo .zip no disco rígido local.

No SAP HANA Studio, na guia **informações do sistema** , clique com o botão direito do mouse na coluna **nome** e selecione **importar instruções SQL**.

![No SAP HANA Studio, na guia Informações do Sistema, clique com o botão direito na coluna Nome e selecione Importar Instruções SQL](./media/troubleshooting-monitoring/image7-import-statements-a.png)

Selecione o arquivo SQL Statements.zip armazenado localmente, e uma pasta com instruções SQL correspondentes será importada. Neste ponto, as diversas verificações de diagnóstico podem ser executadas com essas instruções SQL.

Por exemplo, para testar os requisitos de largura de banda de replicação do sistema SAP HANA, clique com o botão direito na instrução **Largura de banda** em **Replicação: largura de banda** e selecione **Abrir** no Console do SQL.

A instrução SQL completa é aberta, permitindo que os parâmetros de entrada (seção de modificação) sejam alterados e, em seguida, executados.

![A instrução SQL completa é aberta, permitindo que os parâmetros de entrada (seção de modificação) sejam alterados e, em seguida, executados](./media/troubleshooting-monitoring/image8-import-statements-b.png)

Outro exemplo é clicar com o botão direito nas instruções em **Replicação: visão geral**. Selecione **executar** no menu de contexto:

![Outro exemplo é clicar com o botão direito nas instruções em Replicação: visão geral. Selecione Executar no menu de contexto](./media/troubleshooting-monitoring/image9-import-statements-c.png)

Isso resulta em informações que ajudam com a solução de problemas:

![Isso resultará em informações que ajudam com a solução de problemas](./media/troubleshooting-monitoring/image10-import-statements-d.png)

Faça o mesmo para as HANA\_Configuration\_Minichecks e verifique se há qualquer marca _X_ na coluna _C_ (Crítico).

Exemplos de saída:

**HANA\_Configuration\_MiniChecks\_Rev102.01+1** para verificações gerais do SAP HANA.

![HANA\_Configuration\_MiniChecks\_Rev102.01+1 para verificações gerais SAP HANA](./media/troubleshooting-monitoring/image11-configuration-minichecks.png)

**HANA\_Services\_Overview** para uma visão geral de quais serviços SAP HANA estão em execução no momento.

![HANA\_Services\_Overview para uma visão geral de quais serviços SAP HANA estão em execução no momento](./media/troubleshooting-monitoring/image12-services-overview.png)

**HANA\_Services\_Statistics** para informações de serviço do SAP HANA (CPU, memória etc.).

![HANA\_Services\_Statistics para informações de serviço do SAP HANA](./media/troubleshooting-monitoring/image13-services-statistics.png)

**HANA\_Configuration\_Overview\_Rev110+** para informações gerais sobre a instância do SAP HANA.

![HANA\_Configuration\_Overview\_Rev110+ para informações gerais sobre a instância do SAP HANA](./media/troubleshooting-monitoring/image14-configuration-overview.png)

**Hana \_ Parâmetros de configuração \_ \_ Rev70 +** para verificar parâmetros de SAP Hana.

![HANA\_Configuration\_Parameters\_Rev70+ para verificar parâmetros do SAP HANA](./media/troubleshooting-monitoring/image15-configuration-parameters.png)

**Próximas etapas**

- Veja [Configuração de alta disponibilidade no SUSE usando o STONITH](ha-setup-with-stonith.md).
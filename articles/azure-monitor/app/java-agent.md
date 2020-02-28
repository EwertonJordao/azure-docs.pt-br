---
title: Monitoramento de desempenho de aplicativos Web Java-insights de Aplicativo Azure
description: Desempenho e monitoramento de uso estendidos do seu site Java com o Application Insights.
ms.topic: conceptual
ms.date: 01/10/2019
ms.openlocfilehash: b29618179d22eac97a07bf41906465aba1fd7929
ms.sourcegitcommit: 747a20b40b12755faa0a69f0c373bd79349f39e3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/27/2020
ms.locfileid: "77657020"
---
# <a name="monitor-dependencies-caught-exceptions-and-method-execution-times-in-java-web-apps"></a>Monitorar dependências, exceções capturadas e tempos de execução de método em aplicativos Web Java


Se você tiver [instrumentado seu aplicativo Web Java com Application insights][java], poderá usar o agente Java para obter informações mais aprofundadas, sem nenhuma alteração de código:

* **Dependências:** dados sobre chamadas de seu aplicativo a outros componentes, incluindo:
  * As **chamadas http de saída** feitas via Apache HttpClient, OkHttp e `java.net.HttpURLConnection` são capturadas.
  * **Chamadas Redis** feitas por meio do cliente Jedis são capturadas.
  * **Consultas JDBC** – para MySQL e PostgreSQL, se a chamada demorar mais de 10 segundos, o agente relatará o plano de consulta.

* **Log de aplicativo:** Capturar e correlacionar os logs de aplicativo com solicitações HTTP e outras telemetrias
  * **Log4J 1,2**
  * **Log4j2**
  * **Logback**

* **Melhor nomenclatura de operação:** (usada para agregação de solicitações no Portal)
  * Baseado na **mola** `@RequestMapping`.
  * **JAX-RS** -baseado em `@Path`. 

Para usar o agente Java, instale-o no servidor. Seus aplicativos Web devem ser instrumentados com o [SDK do Java Application insights][java]. 

## <a name="install-the-application-insights-agent-for-java"></a>Instalar o agente do Application Insights para Java
1. No computador que está executando o servidor Java, [baixe o agente](https://github.com/Microsoft/ApplicationInsights-Java/releases/latest). Certifique-se de baixar a mesma versão do Agente Java que o núcleo e os pacotes da Web do SDK de Java do Application Insights.
2. Edite o script de inicialização do servidor de aplicativos e adicione o seguinte argumento JVM:
   
    `-javaagent:<full path to the agent JAR file>`
   
    Por exemplo, no Tomcat em um computador Linux:
   
    `export JAVA_OPTS="$JAVA_OPTS -javaagent:<full path to agent JAR file>"`
3. Reinicie o servidor de aplicativos.

## <a name="configure-the-agent"></a>Configurar o agente
Crie um arquivo chamado `AI-Agent.xml` e coloque-o na mesma pasta que o arquivo JAR do agente.

Defina o conteúdo do arquivo xml. Edite o exemplo a seguir para incluir ou omitir os recursos desejados.

```XML
<?xml version="1.0" encoding="utf-8"?>
<ApplicationInsightsAgent>
   <Instrumentation>
      <BuiltIn enabled="true">

         <!-- capture logging via Log4j 1.2, Log4j2, and Logback, default is true -->
         <Logging enabled="true" />

         <!-- capture outgoing HTTP calls performed through Apache HttpClient, OkHttp,
              and java.net.HttpURLConnection, default is true -->
         <HTTP enabled="true" />

         <!-- capture JDBC queries, default is true -->
         <JDBC enabled="true" />

         <!-- capture Redis calls, default is true -->
         <Jedis enabled="true" />

         <!-- capture query plans for JDBC queries that exceed this value (MySQL, PostgreSQL),
              default is 10000 milliseconds -->
         <MaxStatementQueryLimitInMS>1000</MaxStatementQueryLimitInMS>

      </BuiltIn>
   </Instrumentation>
</ApplicationInsightsAgent>
```

## <a name="additional-config-spring-boot"></a>Configuração adicional (Spring boot)

`java -javaagent:/path/to/agent.jar -jar path/to/TestApp.jar`

Para Azure App serviços, faça o seguinte:

* Selecione Configurações > Configurações do Aplicativo
* Em configurações do aplicativo, adicione um novo par de chave/valor:

Chave: `JAVA_OPTS` valor: `-javaagent:D:/home/site/wwwroot/applicationinsights-agent-2.5.0.jar`

Para obter a versão mais recente do agente Java, verifique as versões [aqui](https://github.com/Microsoft/ApplicationInsights-Java/releases
). 

O agente deve ser empacotado como um recurso em seu projeto, de modo que ele termine no diretório D:/Home/site/wwwroot/. Você pode confirmar que o agente está no diretório do serviço de aplicativo correto acessando **ferramentas de desenvolvimento** > **ferramentas avançadas** > **console de depuração** e examinando o conteúdo do diretório do site.    

* Salve as configurações e reinicie o aplicativo. (Essas etapas se aplicam somente aos serviços de aplicativo em execução no Windows.)

> [!NOTE]
> IA-Agent.xml e o arquivo jar do agente devem estar na mesma pasta. Frequentemente, eles são colocados juntos na pasta `/resources` do projeto.  

#### <a name="enable-w3c-distributed-tracing"></a>Habilite o rastreamento distribuído do W3C

Adicione o seguinte ao AI-Agent.xml:

```xml
<Instrumentation>
   <BuiltIn enabled="true">
      <HTTP enabled="true" W3C="true" enableW3CBackCompat="true"/>
   </BuiltIn>
</Instrumentation>
```

> [!NOTE]
> O modo de compatibilidade com versões anteriores está habilitado por padrão e o parâmetro enableW3CBackCompat é opcional e deve ser usado apenas quando você quiser desativá-lo. 

Idealmente, esse seria o caso quando todos os seus serviços fossem atualizados para a versão mais recente dos SDKs com suporte para o protocolo W3C. É altamente recomendável migrar para a versão mais recente dos SDKs do W3C com suporte assim que possível.

Verifique se **ambas as configurações de [entrada](correlation.md#enable-w3c-distributed-tracing-support-for-java-apps) e saídas (agente)** são exatamente as mesmas.

## <a name="view-the-data"></a>Exibir os dados
No recurso Application Insights, a dependência remota agregada e os tempos de execução [do método aparecem no bloco desempenho][metrics].

Para pesquisar instâncias individuais de dependência, exceção e relatórios de método, abra a [pesquisa][diagnostic].

[Diagnosticando problemas de dependência – Saiba mais](../../azure-monitor/app/asp-net-dependencies.md#diagnosis).

## <a name="questions-problems"></a>Perguntas? Problemas?
* Não há dados? [Definir exceções de firewall](../../azure-monitor/app/ip-addresses.md)
* [Solucionar problemas de Java](java-troubleshoot.md)

<!--Link references-->

[api]: ../../azure-monitor/app/api-custom-events-metrics.md
[apiexceptions]: ../../azure-monitor/app/api-custom-events-metrics.md#track-exception
[availability]: ../../azure-monitor/app/monitor-web-app-availability.md
[diagnostic]: ../../azure-monitor/app/diagnostic-search.md
[eclipse]: app-insights-java-eclipse.md
[java]: java-get-started.md
[javalogs]: java-trace-logs.md
[metrics]: ../../azure-monitor/app/metrics-explorer.md

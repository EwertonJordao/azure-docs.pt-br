---
title: Use AMQP com API de serviço de mensagem Java & ônibus de serviço Azure
description: Como usar o Java Message Service (JMS) com o Barramento de Serviço do Microsoft Azure e Advanced Message Queuing Protocol (AMQP) 1.0.
services: service-bus-messaging
documentationcenter: java
author: axisc
editor: spelluru
ms.assetid: be766f42-6fd1-410c-b275-8c400c811519
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 10/22/2019
ms.author: aschhab
ms.custom: seo-java-july2019, seo-java-august2019, seo-java-september2019
ms.openlocfilehash: cd06838abbb69af5684fdea18c42f6a8f95ffe2f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77371261"
---
# <a name="use-the-java-message-service-jms-with-azure-service-bus-and-amqp-10"></a>Use o Serviço de Mensagens Java (JMS) com ônibus de serviço Azure e AMQP 1.0
Este artigo explica como usar os recursos de mensagens do Azure Service Bus (filas e publicar/subscrever tópicos) de aplicativos Java usando o popular padrão DePI do Java Message Service (JMS). Há um [artigo complementar](service-bus-amqp-dotnet.md) que explica como fazer o mesmo usando a API azure Service Bus .NET. Você pode usar esses dois guias em conjunto para saber mais sobre mensagens em plataformas cruzadas usando o AMQP 1.0.

O AMQP 1.0 é um protocolo de mensagens eficiente, confiável e conectado que pode ser usado para criar aplicativos de mensagens robustos em plataformas cruzadas.

O suporte para AMQP 1.0 no Azure Service Bus significa que você pode usar os recursos de mensagens intermediadas de fila e publicação/assinatura de uma variedade de plataformas usando um protocolo binário eficiente. Além disso, você pode criar aplicativos formados por componentes criados com o uso de uma mistura de linguagens, estruturas e sistemas operacionais.

## <a name="get-started-with-service-bus"></a>Introdução ao Barramento de serviço
Este guia pressupõe que você já tem um espaço `basicqueue`de nome de Ônibus de Serviço contendo uma fila chamada . Se você não fizer isso, então você pode [criar o namespace e a fila](service-bus-create-namespace-portal.md) usando o portal [Azure](https://portal.azure.com). Para obter mais informações sobre como criar namespaces e filas do Barramento de Serviço, consulte [Introdução às filas do Barramento de Serviço](service-bus-dotnet-get-started-with-queues.md).

> [!NOTE]
> Filas e tópicos particionados também dão suporte ao AMQP. Para saber mais, confira [Entidades de mensagens particionadas](service-bus-partitioning.md) e [Suporte a AMQP 1.0 para filas e tópicos particionados do Barramento de Serviço](service-bus-partitioned-queues-and-topics-amqp-overview.md).
> 
> 

## <a name="downloading-the-amqp-10-jms-client-library"></a>Baixando a biblioteca do cliente do JMS do AMQP 1.0
Para obter informações sobre onde baixar a versão mais recente da biblioteca de clientes [https://qpid.apache.org/download.html](https://qpid.apache.org/download.html)Apache Qpid JMS AMQP 1.0, visite .

Você deve adicionar os seguintes quatro arquivos JAR do arquivamento de distribuição do Apache Qpid JMS do AMQP 1.0 ao CLASSPATH do Java ao criar e executar aplicativos do JMS com o Barramento de Serviço:

* geronimo-jms\_1.1\_spec-1.0.jar
* qpid-jms-client-[version].jar

> [!NOTE]
> Os nomes e versões do JMS JAR podem ter mudado. Para obter detalhes, consulte [Qpid JMS - AMQP 1.0](https://qpid.apache.org/maven.html#qpid-jms-amqp-10).

## <a name="coding-java-applications"></a>Codificando os aplicativos Java
### <a name="java-naming-and-directory-interface-jndi"></a>Java Naming and Directory Interface (JNDI)
O JMS usa a Java Naming and Directory Interface (JNDI) para criar uma separação entre nomes lógicos e físicos. Dois tipos de objetos JMS são resolvidos usando a JNDI: ConnectionFactory e Destino. A JNDI usa um modelo de provedor no qual você pode conectar diferentes serviços de diretório para lidar com tarefas de resolução de nome. A biblioteca Apache Qpid JMS AMQP 1.0 vem com um provedor JNDI baseado em arquivo de propriedade simples que é configurado usando um arquivo de propriedades do seguinte formato:

```TEXT
# servicebus.properties - sample JNDI configuration

# Register a ConnectionFactory in JNDI using the form:
# connectionfactory.[jndi_name] = [ConnectionURL]
connectionfactory.SBCF = amqps://[SASPolicyName]:[SASPolicyKey]@[namespace].servicebus.windows.net

# Register some queues in JNDI using the form
# queue.[jndi_name] = [physical_name]
# topic.[jndi_name] = [physical_name]
queue.QUEUE = queue1
```

#### <a name="setup-jndi-context-and-configure-the-connectionfactory"></a>Configurar o contexto JNDI e configurar o ConnectionFactory

O **ConnectionString** foi referenciado no disponível no 'Políticas de acesso compartilhado' no [portal Azure](https://portal.azure.com) em **Primary Connection String**
```java
// The connection string builder is the only part of the azure-servicebus SDK library
// we use in this JMS sample and for the purpose of robustly parsing the Service Bus 
// connection string. 
ConnectionStringBuilder csb = new ConnectionStringBuilder(connectionString);
        
// set up JNDI context
Hashtable<String, String> hashtable = new Hashtable<>();
hashtable.put("connectionfactory.SBCF", "amqps://" + csb.getEndpoint().getHost() + "?amqp.idleTimeout=120000&amqp.traceFrames=true");
hashtable.put("queue.QUEUE", "BasicQueue");
hashtable.put(Context.INITIAL_CONTEXT_FACTORY, "org.apache.qpid.jms.jndi.JmsInitialContextFactory");
Context context = new InitialContext(hashtable);

ConnectionFactory cf = (ConnectionFactory) context.lookup("SBCF");

// Look up queue
Destination queue = (Destination) context.lookup("QUEUE");
```

#### <a name="configure-producer-and-consumer-destination-queues"></a>Configurar as Filas de Destino do Produtor e do Consumidor
A entrada usada para definir um destino no provedor JNDI do arquivo de propriedades do Qpid tem o seguinte formato:

Para criar a fila de destino para o Produtor- 
```java
String queueName = "queueName";
Destination queue = (Destination) queueName;

ConnectionFactory cf = (ConnectionFactory) context.lookup("SBCF");
Connection connection - cf.createConnection(csb.getSasKeyName(), csb.getSasKey());

Session session = connection.createSession(false, Session.CLIENT_ACKNOWLEDGE);

// Create Producer
MessageProducer producer = session.createProducer(queue);
```

Para criar a fila de destino para o Consumidor- 
```java
String queueName = "queueName";
Destination queue = (Destination) queueName;

ConnectionFactory cf = (ConnectionFactory) context.lookup("SBCF");
Connection connection - cf.createConnection(csb.getSasKeyName(), csb.getSasKey());

Session session = connection.createSession(false, Session.CLIENT_ACKNOWLEDGE);

// Create Consumer
MessageConsumer consumer = session.createConsumer(queue);
```

### <a name="write-the-jms-application"></a>Escrever o aplicativo JMS
Não existem APIs ou opções especiais obrigatórias ao usar o JMS com o Service Bus. No entanto, existem algumas restrições que serão abordadas posteriormente. Da mesma forma que ocorre com qualquer aplicativo JMS, a primeiro item necessário é a configuração do ambiente JNDI, para ser capaz de resolver um **ConnectionFactory** e destinos.

#### <a name="configure-the-jndi-initialcontext"></a>Configurar o InitialContext de JNDI
O ambiente JNDI é configurado por meio da transmissão de uma tabela de hash com informações de configuração para o construtor da classe javax.naming.InitialContext. Os dois elementos necessários da tabela de hash são o nome da classe de Initial Context Factory e a URL do Provedor. O código a seguir mostra como configurar o ambiente JNDI para usar o Provedor JNDI com base em arquivo de propriedades do Qpid com um arquivo de propriedades chamado **servicebus.properties**.

```java
// set up JNDI context
Hashtable<String, String> hashtable = new Hashtable<>();
hashtable.put("connectionfactory.SBCF", "amqps://" + csb.getEndpoint().getHost() + \
"?amqp.idleTimeout=120000&amqp.traceFrames=true");
hashtable.put("queue.QUEUE", "BasicQueue");
hashtable.put(Context.INITIAL_CONTEXT_FACTORY, "org.apache.qpid.jms.jndi.JmsInitialContextFactory");
Context context = new InitialContext(hashtable);
``` 

### <a name="a-simple-jms-application-using-a-service-bus-queue"></a>Um aplicativo JMS simples que usa uma fila do Barramento de Serviço
O programa de exemplo a seguir envia TextMessages do JMS para uma fila do Service Bus com o nome lógico de JNDI da FILA e recebe as mensagens de volta.

Você pode acessar todas as informações de código-fonte e configuração do [Azure Service Bus Samples JMS Fila quickstart](https://github.com/Azure/azure-service-bus/tree/master/samples/Java/qpid-jms-client/JmsQueueQuickstart)

```java
// Copyright (c) Microsoft. All rights reserved.
// Licensed under the MIT license. See LICENSE file in the project root for full license information.

package com.microsoft.azure.servicebus.samples.jmsqueuequickstart;

import com.microsoft.azure.servicebus.primitives.ConnectionStringBuilder;
import org.apache.commons.cli.*;
import org.apache.log4j.*;

import javax.jms.*;
import javax.naming.Context;
import javax.naming.InitialContext;
import java.util.Hashtable;
import java.util.concurrent.atomic.AtomicInteger;
import java.util.function.Function;

/**
 * This sample demonstrates how to send messages from a JMS Queue producer into
 * an Azure Service Bus Queue, and receive them with a JMS message consumer.
 * JMS Queue. 
 */
public class JmsQueueQuickstart {

    // Number of messages to send
    private static int totalSend = 10;
    //Tracking counter for how many messages have been received; used as termination condition
    private static AtomicInteger totalReceived = new AtomicInteger(0);
    // log4j logger 
    private static Logger logger = Logger.getRootLogger();

    public void run(String connectionString) throws Exception {

        // The connection string builder is the only part of the azure-servicebus SDK library
        // we use in this JMS sample and for the purpose of robustly parsing the Service Bus 
        // connection string. 
        ConnectionStringBuilder csb = new ConnectionStringBuilder(connectionString);
        
        // set up JNDI context
        Hashtable<String, String> hashtable = new Hashtable<>();
        hashtable.put("connectionfactory.SBCF", "amqps://" + csb.getEndpoint().getHost() + "?amqp.idleTimeout=120000&amqp.traceFrames=true");
        hashtable.put("queue.QUEUE", "BasicQueue");
        hashtable.put(Context.INITIAL_CONTEXT_FACTORY, "org.apache.qpid.jms.jndi.JmsInitialContextFactory");
        Context context = new InitialContext(hashtable);
        ConnectionFactory cf = (ConnectionFactory) context.lookup("SBCF");
        
        // Look up queue
        Destination queue = (Destination) context.lookup("QUEUE");

        // we create a scope here so we can use the same set of local variables cleanly 
        // again to show the receive side separately with minimal clutter
        {
            // Create Connection
            Connection connection = cf.createConnection(csb.getSasKeyName(), csb.getSasKey());
            // Create Session, no transaction, client ack
            Session session = connection.createSession(false, Session.CLIENT_ACKNOWLEDGE);

            // Create producer
            MessageProducer producer = session.createProducer(queue);

            // Send messages
            for (int i = 0; i < totalSend; i++) {
                BytesMessage message = session.createBytesMessage();
                message.writeBytes(String.valueOf(i).getBytes());
                producer.send(message);
                System.out.printf("Sent message %d.\n", i + 1);
            }

            producer.close();
            session.close();
            connection.stop();
            connection.close();
        }

        {
            // Create Connection
            Connection connection = cf.createConnection(csb.getSasKeyName(), csb.getSasKey());
            connection.start();
            // Create Session, no transaction, client ack
            Session session = connection.createSession(false, Session.CLIENT_ACKNOWLEDGE);
            // Create consumer
            MessageConsumer consumer = session.createConsumer(queue);
            // create a listener callback to receive the messages
            consumer.setMessageListener(message -> {
                try {
                    // receives message is passed to callback
                    System.out.printf("Received message %d with sq#: %s\n",
                            totalReceived.incrementAndGet(), // increments the tracking counter
                            message.getJMSMessageID());
                    message.acknowledge();
                } catch (Exception e) {
                    logger.error(e);
                }
            });

            // wait on the main thread until all sent messages have been received
            while (totalReceived.get() < totalSend) {
                Thread.sleep(1000);
            }
            consumer.close();
            session.close();
            connection.stop();
            connection.close();
        }

        System.out.printf("Received all messages, exiting the sample.\n");
        System.out.printf("Closing queue client.\n");
    }

    public static void main(String[] args) {

        System.exit(runApp(args, (connectionString) -> {
            JmsQueueQuickstart app = new JmsQueueQuickstart();
            try {
                app.run(connectionString);
                return 0;
            } catch (Exception e) {
                System.out.printf("%s", e.toString());
                return 1;
            }
        }));
    }

    static final String SB_SAMPLES_CONNECTIONSTRING = "SB_SAMPLES_CONNECTIONSTRING";

    public static int runApp(String[] args, Function<String, Integer> run) {
        try {

            String connectionString = null;

            // parse connection string from command line
            Options options = new Options();
            options.addOption(new Option("c", true, "Connection string"));
            CommandLineParser clp = new DefaultParser();
            CommandLine cl = clp.parse(options, args);
            if (cl.getOptionValue("c") != null) {
                connectionString = cl.getOptionValue("c");
            }

            // get overrides from the environment
            String env = System.getenv(SB_SAMPLES_CONNECTIONSTRING);
            if (env != null) {
                connectionString = env;
            }

            if (connectionString == null) {
                HelpFormatter formatter = new HelpFormatter();
                formatter.printHelp("run jar with", "", options, "", true);
                return 2;
            }
            return run.apply(connectionString);
        } catch (Exception e) {
            System.out.printf("%s", e.toString());
            return 3;
        }
    }
}
```

### <a name="run-the-application"></a>Executar o aplicativo
Passe a **Cadeia de Conexão** das políticas de acesso compartilhado para executar o aplicativo.
Abaixo está a saída do formulário, executando o aplicativo:

```Output
> mvn clean package
>java -jar ./target/jmsqueuequickstart-1.0.0-jar-with-dependencies.jar -c "<CONNECTION_STRING>"

Sent message 1.
Sent message 2.
Sent message 3.
Sent message 4.
Sent message 5.
Sent message 6.
Sent message 7.
Sent message 8.
Sent message 9.
Sent message 10.
Received message 1 with sq#: ID:7f6a7659-bcdf-4af6-afc1-4011e2ddcb3c:1:1:1-1
Received message 2 with sq#: ID:7f6a7659-bcdf-4af6-afc1-4011e2ddcb3c:1:1:1-2
Received message 3 with sq#: ID:7f6a7659-bcdf-4af6-afc1-4011e2ddcb3c:1:1:1-3
Received message 4 with sq#: ID:7f6a7659-bcdf-4af6-afc1-4011e2ddcb3c:1:1:1-4
Received message 5 with sq#: ID:7f6a7659-bcdf-4af6-afc1-4011e2ddcb3c:1:1:1-5
Received message 6 with sq#: ID:7f6a7659-bcdf-4af6-afc1-4011e2ddcb3c:1:1:1-6
Received message 7 with sq#: ID:7f6a7659-bcdf-4af6-afc1-4011e2ddcb3c:1:1:1-7
Received message 8 with sq#: ID:7f6a7659-bcdf-4af6-afc1-4011e2ddcb3c:1:1:1-8
Received message 9 with sq#: ID:7f6a7659-bcdf-4af6-afc1-4011e2ddcb3c:1:1:1-9
Received message 10 with sq#: ID:7f6a7659-bcdf-4af6-afc1-4011e2ddcb3c:1:1:1-10
Received all messages, exiting the sample.
Closing queue client.

```

## <a name="amqp-disposition-and-service-bus-operation-mapping"></a>Disposição do Advanced Message Queuing Protocol e mapeamento da operação de Barramento de Serviço
Aqui está como uma disposição de AMQP se traduz em uma operação de Barramento de Serviço:

```Output
ACCEPTED = 1; -> Complete()
REJECTED = 2; -> DeadLetter()
RELEASED = 3; (just unlock the message in service bus, will then get redelivered)
MODIFIED_FAILED = 4; -> Abandon() which increases delivery count
MODIFIED_FAILED_UNDELIVERABLE = 5; -> Defer()
```

## <a name="jms-topics-vs-service-bus-topics"></a>JMS Topics vs. Service Bus Topics
Usando tópicos e assinaturas do Azure Service Bus através da API do Java Message Service (JMS) fornece recursos básicos de envio e recebimento. É uma escolha conveniente ao portar aplicativos de outros corretores de mensagens com APIs compatíveis com JMS, embora os tópicos do Service Bus diferem dos Tópicos JMS e exijam alguns ajustes. 

O Azure Service Bus direciona mensagens para assinaturas nomeadas, compartilhadas e duráveis que são gerenciadas através da interface de gerenciamento de recursos do Azure, das ferramentas de linha de comando do Azure ou através do portal Azure. Cada assinatura permite até 2000 regras de seleção, cada uma das quais pode ter uma condição de filtro e, para filtros SQL, também uma ação de transformação de metadados. Cada correspondência de condição de filtro seleciona a mensagem de entrada a ser copiada na assinatura.  

Receber mensagens de assinaturas é idêntico recebendo mensagens de filas. Cada assinatura tem uma fila de letras mortas associada e a capacidade de encaminhar automaticamente mensagens para outra fila ou tópicos. 

Os JMS Topics permitem que os clientes criem dinamicamente assinantes não duráveis e duráveis que permitem opcionalmente filtrar mensagens com seletores de mensagens. Essas entidades não compartilhadas não são suportadas pela Service Bus. A sintaxe de regra do filtro SQL para Service Bus é, no entanto, semelhante à sintaxe seletor de mensagens suportada pelo JMS. 

O lado editor do JMS Topic é compatível com o Service Bus, como mostrado nesta amostra, mas assinantes dinâmicos não são. As seguintes APIs JMS relacionadas à topologia não são suportadas com o Service Bus. 

## <a name="unsupported-features-and-restrictions"></a>Restrições e recursos não suportados
As restrições a seguir ocorrem durante o uso do JMS sobre o AMQP 1.0 com o Service Bus, ou seja:

* Apenas um **MessageProducer** ou **MessageConsumer** é permitido por **Sessão**. Se precisar criar vários **MessageProducers** ou **MessageConsumers** em um aplicativo, crie uma **Session** dedicada para cada um deles.
* Assinaturas de tópicos voláteis não são suportadas no momento.
* **Os Seletores de mensagens** não são suportados no momento.
* As transações distribuídas não são suportadas (mas as sessões transacionadas são suportadas).

Além disso, o Barramento de Serviço do Microsoft Azure divide o plano de controle do plano de dados e, portanto, não é compatível com várias funções de topologia dinâmica do JMS:

| Método sem suporte          | Substitua por                                                                             |
|-----------------------------|------------------------------------------------------------------------------------------|
| createDurableSubscriber     | criar uma assinatura de tópico portando o seletor de mensagem                                 |
| createDurableConsumer       | criar uma assinatura de tópico portando o seletor de mensagem                                 |
| createSharedConsumer        | Os tópicos do Barramento de Serviço sempre podem ser compartilhados, consulte acima                                       |
| createSharedDurableConsumer | Os tópicos do Barramento de Serviço sempre podem ser compartilhados, consulte acima                                       |
| createTemporaryTopic        | criar um tópico por meio do gerenciamento de API/ferramentas/portal com *AutoDeleteOnIdle* definido como um período de expiração |
| createTopic                 | criar um tópico por meio de ferramentas/API/portal de gerenciamento                                           |
| cancelar assinatura                 | excluir a API de gerenciamento do tópico/ferramentas/portal                                             |
| createBrowser               | sem suporte. Usar a funcionalidade de Peek() da API do Barramento de Serviço                         |
| createQueue                 | criar uma fila por meio de ferramentas/API/portal de gerenciamento                                           | 
| createTemporaryQueue        | criar uma fila por meio do gerenciamento de API/ferramentas/portal com *AutoDeleteOnIdle* definido como um período de expiração |
| receberNoWait               | use o método de recebimento () fornecido pelo Service Bus SDK e especifique um tempo de intervalo muito baixo ou zero |

## <a name="summary"></a>Resumo
Este guia de instruções explicou como usar os recursos do sistema de mensagens agenciado do Barramento de Serviço (tópicos sobre filas e publicação/assinatura) do Java usando a API popular JMS e o AMQP 1.0.

Você também pode usar o AMQP 1.0 do Service Bus de outras linguagens, incluindo .NET, C, Python e PHP. Os componentes criados com essas diferentes linguagens podem trocar mensagens de forma confiável e com total fidelidade usando o suporte do AMQP 1.0 no Service Bus.

## <a name="next-steps"></a>Próximas etapas
* [Suporte para o AMQP 1.0 no Barramento de Serviço do Azure](service-bus-amqp-overview.md)
* [Como usar o AMQP 1.0 com a API .NET do Barramento de Serviço](service-bus-dotnet-advanced-message-queuing.md)
* [Guia do Desenvolvedor do AMQP 1.0 do Barramento de Serviço](service-bus-amqp-dotnet.md)
* [Introdução às filas do Barramento de Serviço](service-bus-dotnet-get-started-with-queues.md)
* [Java Developer Center](https://azure.microsoft.com/develop/java/)


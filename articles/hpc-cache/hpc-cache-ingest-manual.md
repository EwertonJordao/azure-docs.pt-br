---
title: Azure HPC Cache dataing - cópia manual
description: Como usar comandos cp para mover dados para um alvo de armazenamento Blob no Cache Azure HPC
author: ekpgh
ms.service: hpc-cache
ms.topic: conceptual
ms.date: 10/30/2019
ms.author: rohogue
ms.openlocfilehash: fc397088e46f0d2b623080f3deed24c386e7d8b4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74168492"
---
# <a name="azure-hpc-cache-data-ingest---manual-copy-method"></a>Azure HPC Cache dataing - manual copy method

Este artigo fornece instruções detalhadas para copiar manualmente dados para um contêiner de armazenamento Blob para uso com cache Azure HPC. Ele usa operações paralelas multi-threaded para otimizar a velocidade da cópia.

Para saber mais sobre como mover dados para o armazenamento Blob para o cache Do Azure HPC, leia [Mover dados para o armazenamento Azure Blob](hpc-cache-ingest.md).

## <a name="simple-copy-example"></a>Exemplo de cópia simples

Você pode criar manualmente uma cópia com vários threads em um cliente executando mais de um comando de cópia ao mesmo tempo em segundo plano em relação a conjuntos predefinidos de arquivos ou caminhos.

O comando ``cp`` UNIX/Linux inclui o argumento ``-p`` para preservar a propriedade e os metadados mtime. Adicionar esse argumento aos comandos a seguir é opcional. (Adicionar o argumento aumenta o número de chamadas do sistema de arquivos enviadas do cliente para o sistema de arquivos de destino para modificação de metadados.)

Este exemplo simples copia dois arquivos em paralelo:

```bash
cp /mnt/source/file1 /mnt/destination1/ & cp /mnt/source/file2 /mnt/destination1/ &
```

Depois de emitir esse comando, o comando `jobs` mostrará se os dois threads estão em execução.

## <a name="copy-data-with-predictable-file-names"></a>Copiar dados com nomes de arquivos previsíveis

Se os nomes dos seus arquivos forem previsíveis, você pode usar expressões para criar segmentos de cópia paralela. 

Por exemplo, se o diretório contiver 1.000 arquivos numerados consecutivamente de `0001` a `1000`, você poderá usar as expressões a seguir para criar dez threads paralelos que copiam, cada um, 100 arquivos:

```bash
cp /mnt/source/file0* /mnt/destination1/ & \
cp /mnt/source/file1* /mnt/destination1/ & \
cp /mnt/source/file2* /mnt/destination1/ & \
cp /mnt/source/file3* /mnt/destination1/ & \
cp /mnt/source/file4* /mnt/destination1/ & \
cp /mnt/source/file5* /mnt/destination1/ & \
cp /mnt/source/file6* /mnt/destination1/ & \
cp /mnt/source/file7* /mnt/destination1/ & \
cp /mnt/source/file8* /mnt/destination1/ & \
cp /mnt/source/file9* /mnt/destination1/
```

## <a name="copy-data-with-unstructured-file-names"></a>Copiar dados com nomes de arquivos não estruturados

Se sua estrutura de nomeação de arquivos não for previsível, você pode agrupar arquivos por nomes de diretório. 

Este exemplo coleta diretórios inteiros para enviar a comandos ``cp`` executados como tarefas em segundo plano:

```bash
/root
|-/dir1
| |-/dir1a
| |-/dir1b
| |-/dir1c
   |-/dir1c1
|-/dir1d
```

Depois que os arquivos são coletados, você pode executar comandos de cópia paralela para copiar recursivamente as subpastas e todos os seus conteúdos:

```bash
cp /mnt/source/* /mnt/destination/
mkdir -p /mnt/destination/dir1 && cp /mnt/source/dir1/* mnt/destination/dir1/ & 
cp -R /mnt/source/dir1/dir1a /mnt/destination/dir1/ & 
cp -R /mnt/source/dir1/dir1b /mnt/destination/dir1/ & 
cp -R /mnt/source/dir1/dir1c /mnt/destination/dir1/ & # this command copies dir1c1 via recursion
cp -R /mnt/source/dir1/dir1d /mnt/destination/dir1/ &
```

## <a name="when-to-add-mount-points"></a>Quando adicionar pontos de montagem

Depois de ter threads paralelos suficientes indo contra um único ponto de montagem do sistema de arquivos de destino, haverá um ponto em que adicionar mais threads não dá mais throughput. (O throughput será medido em arquivos/segundo ou bytes/segundo, dependendo do seu tipo de dados.) Ou pior, o excesso de rosca às vezes pode causar uma degradação de throughput.  

Quando isso acontecer, você pode adicionar pontos de montagem do lado do cliente a outros endereços de montagem do Cache Azure HPC, usando o mesmo caminho de montagem do sistema de arquivos remoto:

```bash
10.1.0.100:/nfs on /mnt/sourcetype nfs (rw,vers=3,proto=tcp,addr=10.1.0.100)
10.1.1.101:/nfs on /mnt/destination1type nfs (rw,vers=3,proto=tcp,addr=10.1.1.101)
10.1.1.102:/nfs on /mnt/destination2type nfs (rw,vers=3,proto=tcp,addr=10.1.1.102)
10.1.1.103:/nfs on /mnt/destination3type nfs (rw,vers=3,proto=tcp,addr=10.1.1.103)
```

Adicionar pontos de montagem do lado do cliente permite que você divida os comandos de cópia para pontos de montagem `/mnt/destination[1-3]` adicionais, obtendo ainda mais paralelismo.  

Por exemplo, se os arquivos forem muito grandes, você poderá definir os comandos de cópia para usar caminhos de destino distintos, enviando mais comandos em paralelo do cliente que está executando a cópia.

```bash
cp /mnt/source/file0* /mnt/destination1/ & \
cp /mnt/source/file1* /mnt/destination2/ & \
cp /mnt/source/file2* /mnt/destination3/ & \
cp /mnt/source/file3* /mnt/destination1/ & \
cp /mnt/source/file4* /mnt/destination2/ & \
cp /mnt/source/file5* /mnt/destination3/ & \
cp /mnt/source/file6* /mnt/destination1/ & \
cp /mnt/source/file7* /mnt/destination2/ & \
cp /mnt/source/file8* /mnt/destination3/ & \
```

No exemplo acima, todos os três pontos de montagem de destino estão sendo direcionados pelos processos de cópia de arquivo do cliente.

## <a name="when-to-add-clients"></a>Quando adicionar clientes

Por fim, quando você tiver atingiu as funcionalidades do cliente, adicionar mais threads de cópia ou pontos de montagem não produzirá mais nenhum aumento de arquivos/s ou bytes/s. Nessa situação, você pode implantar outro cliente com o mesmo conjunto de pontos de montagem que vá executar os próprios conjuntos de processos de cópia de arquivo. 

Exemplo:

```bash
Client1: cp -R /mnt/source/dir1/dir1a /mnt/destination/dir1/ &
Client1: cp -R /mnt/source/dir2/dir2a /mnt/destination/dir2/ &
Client1: cp -R /mnt/source/dir3/dir3a /mnt/destination/dir3/ &

Client2: cp -R /mnt/source/dir1/dir1b /mnt/destination/dir1/ &
Client2: cp -R /mnt/source/dir2/dir2b /mnt/destination/dir2/ &
Client2: cp -R /mnt/source/dir3/dir3b /mnt/destination/dir3/ &

Client3: cp -R /mnt/source/dir1/dir1c /mnt/destination/dir1/ &
Client3: cp -R /mnt/source/dir2/dir2c /mnt/destination/dir2/ &
Client3: cp -R /mnt/source/dir3/dir3c /mnt/destination/dir3/ &

Client4: cp -R /mnt/source/dir1/dir1d /mnt/destination/dir1/ &
Client4: cp -R /mnt/source/dir2/dir2d /mnt/destination/dir2/ &
Client4: cp -R /mnt/source/dir3/dir3d /mnt/destination/dir3/ &
```

## <a name="create-file-manifests"></a>Criar manifestos de arquivista

Depois de entender as abordagens acima (vários threads de cópia por destino, vários destinos por cliente, vários clientes por sistema de arquivos de origem acessível à rede), considere esta recomendação: Construa manifestos de arquivos e, em seguida, use-os com cópia comandos em vários clientes.

Esse cenário usa o comando ``find`` do UNIX para criar manifestos de arquivos ou diretórios:

```bash
user@build:/mnt/source > find . -mindepth 4 -maxdepth 4 -type d
./atj5b55c53be6-01/support/gsi/2018-07-22T21:12:06EDT
./atj5b55c53be6-01/support/pcap/2018-07-23T01:34:57UTC
./atj5b55c53be6-01/support/trace/rolling
./atj5b55c53be6-03/support/gsi/2018-07-22T21:12:06EDT
./atj5b55c53be6-03/support/pcap/2018-07-23T01:34:57UTC
./atj5b55c53be6-03/support/trace/rolling
./atj5b55c53be6-02/support/gsi/2018-07-22T21:12:06EDT
./atj5b55c53be6-02/support/pcap/2018-07-23T01:34:57UTC
./atj5b55c53be6-02/support/trace/rolling
```

Redirecione esse resultado para um arquivo: `find . -mindepth 4 -maxdepth 4 -type d > /tmp/foo`

Em seguida, você pode iterar pelo manifesto, usando comandos BASH para contar arquivos e determinar os tamanhos dos subdiretórios:

```bash
ben@xlcycl1:/sps/internal/atj5b5ab44b7f > for i in $(cat /tmp/foo); do echo " `find ${i} |wc -l`    `du -sh ${i}`"; done
244    3.5M    ./atj5b5ab44b7f-02/support/gsi/2018-07-18T00:07:03EDT
9      172K    ./atj5b5ab44b7f-02/support/gsi/stats_2018-07-18T05:01:00UTC
124    5.8M    ./atj5b5ab44b7f-02/support/gsi/stats_2018-07-19T01:01:01UTC
152    15M     ./atj5b5ab44b7f-02/support/gsi/stats_2018-07-20T01:01:00UTC
131    13M     ./atj5b5ab44b7f-02/support/gsi/stats_2018-07-20T21:59:41UTC_partial
789    6.2M    ./atj5b5ab44b7f-02/support/gsi/2018-07-20T21:59:41UTC
134    12M     ./atj5b5ab44b7f-02/support/gsi/stats_2018-07-20T22:22:55UTC_hpccache_catchup
7      16K     ./atj5b5ab44b7f-02/support/pcap/2018-07-18T17:12:19UTC
8      83K     ./atj5b5ab44b7f-02/support/pcap/2018-07-18T17:17:17UTC
575    7.7M    ./atj5b5ab44b7f-02/support/cores/armada_main.2000.1531980253.gsi
33     4.4G    ./atj5b5ab44b7f-02/support/trace/rolling
281    6.6M    ./atj5b5ab44b7f-01/support/gsi/2018-07-18T00:07:03EDT
15     182K    ./atj5b5ab44b7f-01/support/gsi/stats_2018-07-18T05:01:00UTC
244    17M     ./atj5b5ab44b7f-01/support/gsi/stats_2018-07-19T01:01:01UTC
299    31M     ./atj5b5ab44b7f-01/support/gsi/stats_2018-07-20T01:01:00UTC
256    29M     ./atj5b5ab44b7f-01/support/gsi/stats_2018-07-20T21:59:41UTC_partial
889    7.7M    ./atj5b5ab44b7f-01/support/gsi/2018-07-20T21:59:41UTC
262    29M     ./atj5b5ab44b7f-01/support/gsi/stats_2018-07-20T22:22:55UTC_hpccache_catchup
11     248K    ./atj5b5ab44b7f-01/support/pcap/2018-07-18T17:12:19UTC
11     88K     ./atj5b5ab44b7f-01/support/pcap/2018-07-18T17:17:17UTC
645    11M     ./atj5b5ab44b7f-01/support/cores/armada_main.2019.1531980253.gsi
33     4.0G    ./atj5b5ab44b7f-01/support/trace/rolling
244    2.1M    ./atj5b5ab44b7f-03/support/gsi/2018-07-18T00:07:03EDT
9      158K    ./atj5b5ab44b7f-03/support/gsi/stats_2018-07-18T05:01:00UTC
124    5.3M    ./atj5b5ab44b7f-03/support/gsi/stats_2018-07-19T01:01:01UTC
152    15M     ./atj5b5ab44b7f-03/support/gsi/stats_2018-07-20T01:01:00UTC
131    12M     ./atj5b5ab44b7f-03/support/gsi/stats_2018-07-20T21:59:41UTC_partial
789    8.4M    ./atj5b5ab44b7f-03/support/gsi/2018-07-20T21:59:41UTC
134    14M     ./atj5b5ab44b7f-03/support/gsi/stats_2018-07-20T22:25:58UTC_hpccache_catchup
7      159K    ./atj5b5ab44b7f-03/support/pcap/2018-07-18T17:12:19UTC
7      157K    ./atj5b5ab44b7f-03/support/pcap/2018-07-18T17:17:17UTC
576    12M     ./atj5b5ab44b7f-03/support/cores/armada_main.2013.1531980253.gsi
33     2.8G    ./atj5b5ab44b7f-03/support/trace/rolling
```

Por fim, você deve criar os comandos de cópia de arquivo reais para os clientes.  

Se você tiver quatro clientes, use este comando:

```bash
for i in 1 2 3 4 ; do sed -n ${i}~4p /tmp/foo > /tmp/client${i}; done
```

Se você tiver cinco clientes, use algo parecido com isto:

```bash
for i in 1 2 3 4 5; do sed -n ${i}~5p /tmp/foo > /tmp/client${i}; done
```

E por seis... Extrapolar conforme necessário.

```bash
for i in 1 2 3 4 5 6; do sed -n ${i}~6p /tmp/foo > /tmp/client${i}; done
```

Você obterá *N* arquivos resultantes, um para cada um dos seus *N* clientes que têm os nomes de caminho para os diretórios de quatro níveis obtidos como parte da saída do comando `find`. 

Use cada arquivo para criar o comando de cópia:

```bash
for i in 1 2 3 4 5 6; do for j in $(cat /tmp/client${i}); do echo "cp -p -R /mnt/source/${j} /mnt/destination/${j}" >> /tmp/client${i}_copy_commands ; done; done
```

As opções acima gerará *N* arquivos, cada um com um comando de cópia por linha, que pode ser executado como um script BASH no cliente. 

A meta é executar vários threads desses scripts simultaneamente por cliente em paralelo em vários clientes.

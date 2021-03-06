---
title: Vários clusters HDInsight & uma conta de Azure Data Lake Storage
description: Saiba como usar mais de um cluster HDInsight com uma única conta do Data Lake Storage
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: how-to
ms.custom: hdinsightactive
ms.date: 12/18/2019
ms.openlocfilehash: 19c40f2a7609d556448641e78fdeffe83e8660b1
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/09/2020
ms.locfileid: "86083943"
---
# <a name="use-multiple-hdinsight-clusters-with-an-azure-data-lake-storage-account"></a>Usar múltiplos clusters HDInsight com uma conta do Azure Data Lake Storage

Começando com o HDInsight versão 3.5, você pode criar clusters HDInsight com contas de armazenamento do Azure Data Lake Storage como o sistema de arquivos padrão.
O Data Lake Storage dá suporte a armazenamento ilimitado, o que o torna ideal não apenas para hospedagem de grandes quantidades de dados, mas também para hospedar vários clusters HDInsight que compartilham uma única conta do Data Lake Storage. Para obter instruções sobre como criar um cluster HDInsight com Data Lake Storage como armazenamento, consulte [início rápido: configurar clusters no HDInsight](../storage/data-lake-storage/quickstart-create-connect-hdi-cluster.md).

Este artigo fornece recomendações para o administrador do Data Lake Storage configurar uma única conta compartilhada do Data Lake Storage que pode ser usada em vários clusters HDInsight **ativos**. Essas recomendações se aplicam à hospedagem de vários clusters Apache Hadoop seguros e também não seguros em uma conta compartilhada do Data Lake Storage.

## <a name="data-lake-storage-file-and-folder-level-acls"></a>ACLs no nível de pasta e de arquivo do Data Lake Storage

O restante deste artigo pressupõe que você tenha um bom conhecimento de ACLs no nível de arquivo e de pasta do Azure Data Lake Storage, o que é descrito em detalhes no [Controle de acesso no Azure Data Lake Storage](../data-lake-store/data-lake-store-access-control.md).

## <a name="data-lake-storage-setup-for-multiple-hdinsight-clusters"></a>Configuração do Data Lake Storage para vários clusters HDInsight

Vamos pegar uma hierarquia de pastas de dois níveis para explicar as recomendações para o uso de vários clusters HDInsight com uma conta de Data Lake Storage. Considere que você tem uma conta do Data Lake Storage com a estrutura de pasta **/clusters/finance**. Com essa estrutura, todos os clusters requeridos pela organização Finance podem usar /clusters/finance como o local de armazenamento. No futuro, se outra organização, digamos, Marketing, quiser criar clusters HDInsight que usem a mesma conta do Data Lake Storage, ela poderá criar clusters/marketing. Por enquanto, usaremos apenas **/clusters/finance**.

Para habilitar essa estrutura de pasta para ser efetivamente usada pelos clusters HDInsight, o administrador do Data Lake Storage deve atribuir permissões apropriadas, conforme descrito na tabela. As permissões mostradas na tabela correspondem a ACLs de acesso e ACLs não padrão.

|Pasta  |Permissões  |Usuário proprietário  |Grupo proprietário  | Usuário nomeado | Permissões de usuário nomeado | Grupo nomeado | Permissões de grupo nomeado |
|---------|---------|---------|---------|---------|---------|---------|---------|
|/ | rwxr-x--x  |administrador |administrador  |Entidade de serviço |--x  |FINGRP   |r-x         |
|/clusters | rwxr-x--x |administrador |administrador |Entidade de serviço |--x  |FINGRP |r-x         |
|/clusters/finance | rwxr-x--t |administrador |FINGRP  |Entidade de serviço |rwx  |-  |-     |

Na tabela,

- **Admin** é o criador e o administrador da conta do Data Lake Storage.
- **Entidade de serviço** é a entidade de serviço do Azure Active Directory (AAD) associada à conta.
- **FINGRP** é um grupo de usuários criado no AAD que contém os usuários da organização Finance.

Para obter instruções sobre como criar um aplicativo do AAD (que também cria uma entidade de serviço), consulte [Criar um aplicativo do AAD](../active-directory/develop/howto-create-service-principal-portal.md#register-an-application-with-azure-ad-and-create-a-service-principal). Para obter instruções sobre como criar um grupo de usuários no AAD, consulte [Gerenciamento de grupos no Azure Active Directory](../active-directory/fundamentals/active-directory-groups-create-azure-portal.md).

Veja alguns pontos principais a considerar.

- A estrutura de pastas dois níveis (**/clusters/finance/**) deve ser criada e provisionada com permissões apropriadas pelo administrador do Data Lake Storage **antes da** conta de armazenamento ser usada para clusters. Essa estrutura não é criada automaticamente durante a criação de clusters.
- O exemplo acima recomenda que se configure o grupo proprietário de **/clusters/finance** como **FINGRP** e se conceda acesso **r-x** a FINGRP para a hierarquia de pastas inteira, começando da raiz. Isso assegura que os membros de FINGRP possam navegar pela estrutura de pastas começando da raiz.
- No caso em que entidades de serviço do AAD diferentes podem criar clusters em **/clusters/finance**, o sticky bit (quando definido na pasta **finance**) assegura que as pastas criadas por uma entidade de serviço não possam ser excluídas pela outra.
- Depois que a estrutura de pastas e as permissões estiverem em vigor, o processo de criação de cluster do HDInsight criará um local de armazenamento específico do cluster em **/clusters/Finance/**. Por exemplo, o armazenamento de um cluster com o nome fincluster01 poderia ser **/clusters/finance/fincluster01**. A propriedade e as permissões para as pastas criadas pelo cluster HDInsight são mostrados na tabela apresentada aqui.

    |Pasta  |Permissões  |Usuário proprietário  |Grupo proprietário  | Usuário nomeado | Permissões de usuário nomeado | Grupo nomeado | Permissões de grupo nomeado |
    |---------|---------|---------|---------|---------|---------|---------|---------|
    |/clusters/finanace/ fincluster01 | rwxr-x---  |Entidade de Serviço |FINGRP  |- |-  |-   |-  |

## <a name="recommendations-for-job-input-and-output-data"></a>Recomendações para dados de entrada e saída do trabalho

É recomendável que os dados de entrada para um trabalho e as saídas de um trabalho sejam armazenados em uma pasta fora de **/clusters**. Isso garante que, mesmo que a pasta específica do cluster seja excluída para recuperar algum espaço de armazenamento, as entradas e saídas do trabalho ainda estarão disponíveis para uso futuro. Nesse caso, verifique se a hierarquia de pastas para armazenar as entradas e saídas de trabalho permite o nível de acesso apropriado para a entidade de serviço.

## <a name="limit-on-clusters-sharing-a-single-storage-account"></a>Limite de clusters compartilhando uma única conta de armazenamento

O limite no número de clusters que podem compartilhar uma única conta do Data Lake Storage depende da carga de trabalho sendo executada em um desses clusters. Ter clusters demais ou cargas de trabalho muito pesadas nos clusters que compartilham uma conta de armazenamento pode fazer com que a entrada/saída de conta de armazenamento seja limitada.

## <a name="support-for-default-acls"></a>Suporte para ACLs padrão

Ao criar uma entidade de serviço com acesso de usuário nomeado (conforme mostrado na tabela acima), é recomendável **não** adicionar o usuário nomeado com uma ACL padrão. O provisionamento de acesso de usuário nomeado usando ACLs padrão resulta na atribuição de 770 permissões para o usuário proprietário, o grupo proprietário e outros. Embora esse valor padrão de 770 não remova as permissões do usuário proprietário (7) ou do grupo proprietário (7), ele retira todas as permissões para outros (0). Isso resulta em um problema conhecido com um caso de uso específico, que é abordado em detalhes na seção [Problemas conhecidos e soluções alternativas](#known-issues-and-workarounds).

## <a name="known-issues-and-workarounds"></a>Problemas conhecidos e limitações

Esta seção lista os problemas conhecidos para usar o HDInsight com Data Lake Storage e suas soluções alternativas.

### <a name="publicly-visible-localized-apache-hadoop-yarn-resources"></a>Recursos do Apache Hadoop YARN localizados e visíveis publicamente

Quando uma nova conta do Azure Data Lake Storage é criada, o diretório raiz é provisionado automaticamente com os bits de permissão de ACL de acesso definidos como 770. O usuário proprietário da pasta raiz é definido como o usuário que criou a conta (o administrador do Data Lake Storage) e o grupo proprietário é definido como o grupo primário do usuário que criou a conta. Nenhum acesso é fornecido para "outros".

Sabe-se que essas configurações afetam um caso de uso específico do HDInsight capturado no [YARN 247](https://hwxmonarch.atlassian.net/browse/YARN-247). Envios de trabalho podem falhar com uma mensagem de erro semelhante a esta:

```output
Resource XXXX is not publicly accessible and as such cannot be part of the public cache.
```

Conforme declarado no JIRA YARN vinculado anteriormente, ao localizar recursos públicos, o localizador valida que todos os recursos solicitados estão realmente públicos verificando as permissões deles no sistema de arquivos remoto. Qualquer LocalResource que não caiba nessa condição será rejeitado para localização. Para verificar se há permissões, o acesso de leitura para o arquivo é incluído para "outros". Esse cenário não funciona pronto para uso ao hospedar clusters HDInsight em Azure Data Lake, já que Azure Data Lake nega todo o acesso a "outros" no nível da pasta raiz.

#### <a name="workaround"></a>Solução alternativa

Defina permissões de leitura-execução **outros** por meio da hierarquia, por exemplo, em **/**, **/clusters** e **/clusters/finance**, conforme mostrado na tabela acima.

## <a name="see-also"></a>Confira também

- [Início rápido: Configurar clusters no HDInsight](../storage/data-lake-storage/quickstart-create-connect-hdi-cluster.md)
- [Usar Gen2 de armazenamento do Azure Data Lake com clusters de HDInsight do Azure](hdinsight-hadoop-use-data-lake-storage-gen2.md)

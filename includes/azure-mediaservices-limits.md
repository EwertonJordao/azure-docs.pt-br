---
author: rothja
ms.service: cost-management-billing
ms.topic: include
ms.date: 11/09/2018
ms.author: jroth
ms.openlocfilehash: 2fe091a4ff0295ecadfd69ba3f2d4ca59e9612e5
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80334606"
---
>[!NOTE]
>Para recursos que não são corrigidos, abra um tíquete de suporte para solicitar um aumento nas cotas. Não crie outras contas dos serviços de mídia do Azure em uma tentativa de obter limites mais altos.

| Recurso | Limite | 
| --- | --- | 
| Contas dos serviços de mídia do Azure em uma única assinatura | 25 (fixo) |
| Unidades reservadas de mídia por conta de serviços de mídia |25 (S1)<br/>10 (S2, S3)<sup>1</sup> | 
| Trabalhos por conta dos Serviços de Mídia | 50.000<sup>2</sup> |
| Tarefas encadeadas por trabalho | 30 (fixo) |
| Ativos por conta de serviços de mídia | 1.000.000|
| Ativos por tarefa | 50 |
| Ativos por trabalho | 100 |
| Localizadores exclusivos associados a um ativo simultaneamente | 5<sup>4</sup> |
| Canais ao vivo por conta de serviços de mídia |5|
| Programas no estado interrompido por canal  |50|
| Programa em estado de execução por canal  |3|
| Pontos de extremidade de streaming que são interrompidos ou em execução por conta de serviços de mídia|2|
| Unidades de streaming por ponto de extremidade de streaming |10 |
| Contas de armazenamento | 1,000<sup>5</sup> (fixo) |
| Políticas | 1,000,000<sup>6</sup> |
| Tamanho do arquivo| Em alguns cenários, há um limite no tamanho máximo de arquivo com suporte para processamento nos serviços de mídia. <sup>7</sup> |

<sup>1</sup> Se você alterar o tipo, por exemplo, de S2 para S1, os limites de unidade reservados máximos serão redefinidos.

<sup>2</sup> Esse número inclui trabalhos enfileirados, concluídos, ativos e cancelados. Ele não inclui trabalhos excluídos. Você pode excluir trabalhos antigos usando **IJob. Delete** ou a solicitação HTTP **delete** .

A partir de 1º de abril de 2017, qualquer registro de trabalho em sua conta com mais de 90 dias será excluído automaticamente, juntamente com seus registros de tarefa associados. A exclusão automática ocorre mesmo que o número total de registros esteja abaixo da cota máxima. Para arquivar as informações de trabalho e tarefa, use o código descrito em [gerenciar ativos com o SDK do .net dos serviços de mídia](../articles/media-services/previous/media-services-dotnet-manage-entities.md).

<sup>3</sup> Quando você faz uma solicitação para listar entidades de trabalho, um máximo de 1.000 trabalhos é retornado por solicitação. Para manter o controle de todos os trabalhos enviados, use as consultas Top ou Skip conforme descrito em [Opções de consulta do sistema OData](/previous-versions/dynamicscrm-2015/developers-guide/gg309461(v=crm.7)).

<sup>4</sup> Os localizadores não foram projetados para gerenciar o controle de acesso por usuário. Para conceder direitos de acesso diferentes a usuários individuais, use soluções de DRM (gerenciamento de direitos digitais). Para obter mais informações, consulte [proteger seu conteúdo com os serviços de mídia do Azure](../articles/media-services/previous/media-services-content-protection-overview.md).

<sup>5</sup> As contas de armazenamento devem ser da mesma assinatura do Azure.

<sup>6</sup> Há um limite de 1 milhão políticas para diferentes políticas de serviços de mídia. Um exemplo é para a política do localizador ou ContentKeyAuthorizationPolicy. 

>[!NOTE]
> Se você sempre usar os mesmos dias e permissões de acesso, use a mesma ID de política. Para obter informações e um exemplo, consulte [gerenciar ativos com o SDK do .net dos serviços de mídia](../articles/media-services/previous/media-services-dotnet-manage-entities.md#limit-access-policies).

<sup>7</sup> O tamanho máximo com suporte para um único blob atualmente é de até 5 TB no armazenamento de BLOBs do Azure. Limites adicionais se aplicam aos serviços de mídia com base nos tamanhos de VM que são usados pelo serviço. O limite de tamanho se aplica aos arquivos que você carrega e também aos arquivos que são gerados como resultado do processamento de serviços de mídia (codificação ou análise). Se o arquivo de origem for maior que 260 GB, o trabalho provavelmente falhará. 

A tabela a seguir mostra os limites nas unidades reservadas de mídia S1, S2 e S3. Se o arquivo de origem for maior do que os limites definidos na tabela, o trabalho de codificação falhará. Se você codificar fontes de resolução de 4K de longa duração, será necessário usar unidades reservadas de mídia S3 para atingir o desempenho necessário. Se você tiver um conteúdo de 4K maior do que o limite de 260 GB nas unidades reservadas de mídia S3, abra um tíquete de suporte.

|Tipo de unidade reservada de mídia    |Tamanho máximo de entrada (GB)|
|---|---|
|S1 |    26|
|S2    | 60|
|S3    |260|

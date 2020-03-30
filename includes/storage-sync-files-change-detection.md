---
author: roygara
ms.service: storage
ms.topic: include
ms.date: 10/26/2018
ms.author: rogarana
ms.openlocfilehash: 55456a6be938411d3c08a0eaa8fdbfb0844e7129
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2020
ms.locfileid: "72391547"
---
As alterações feitas ao compartilhamento de Arquivos do Azure usando o Portal do Azure ou SMB não são detectadas e replicadas imediatamente como as alterações no Ponto de extremidade do servidor são. Os Arquivos do Azure ainda não têm notificações/diário de alteração, portanto, não há nenhuma maneira de iniciar automaticamente uma sessão de sincronização quando os arquivos são alterados. No Windows Server, a sincronização de Arquivos do Azure usa o [diário de USN do Windows](https://msdn.microsoft.com/library/windows/desktop/aa363798.aspx) para iniciar automaticamente uma sessão de sincronização quando os arquivos são alterados.

Para detectar alterações para o compartilhamento de arquivos do Azure, a sincronização de arquivos do Azure tem um trabalho agendado chamado um *alterar o trabalho de detecção*. Um trabalho de detecção de alteração enumera todos os arquivos no compartilhamento de arquivos e, em seguida, compara a versão de sincronização para esse arquivo. Quando o trabalho de detecção de alteração determina que os arquivos foram alterados, a sincronização de Arquivos do Azure inicia uma sessão de sincronização. O trabalho de detecção de alteração é iniciado a cada 24 horas. Como o trabalho de detecção de alteração funciona enumerando todos os arquivos no compartilhamento de Arquivos do Azure, a detecção de troca demora mais nos namespaces grandes do que namespaces menores. Para esses namespaces, pode levar mais de uma vez a cada 24 horas para determinar quais arquivos foram alterados.

Para sincronizar imediatamente os arquivos que são alterados no compartilhamento de arquivos do Azure, o **cmdlet Invoke-AzStorageSyncDetection** PowerShell pode ser usado para iniciar manualmente a detecção de alterações no compartilhamento de arquivos do Azure. Este cmdlet destina-se a cenários em que algum tipo de processo automatizado está fazendo alterações no compartilhamento de arquivos do Azure ou as alterações são feitas por um administrador (como mover arquivos e diretórios para o compartilhamento). Para alterações do usuário final, a recomendação é instalar o agente Azure File Sync em uma VM IaaS e fazer com que os usuários finais acessem o compartilhamento de arquivos através do VM IaaS. Desta forma, todas as alterações serão sincronizadas rapidamente com outros agentes sem a necessidade de usar o cmdlet Invoke-AzStorageSyncDetecção. Para saber mais, consulte a documentação [Invoke-AzStorageSyncChangeDetection.](https://docs.microsoft.com/powershell/module/az.storagesync/invoke-azstoragesyncchangedetection)

>[!NOTE]
>As alterações feitas em um compartilhamento de arquivos do Azure usando REST não atualizam o último tempo modificado do SMB e não serão vistas como uma alteração por sincronização.

Estamos explorando adicionar detecção de alteração para um compartilhamento de arquivos do Azure semelhante ao USN para volumes no Windows Server. Ajude-nos a priorizar esse recurso para desenvolvimento futuro votando no [UserVoice de Arquivos do Azure](https://feedback.azure.com/forums/217298-storage/category/180670-files).

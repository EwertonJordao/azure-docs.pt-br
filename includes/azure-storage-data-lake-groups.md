---
services: storage
author: normesta
ms.service: storage
ms.topic: include
ms.date: 09/29/2020
ms.author: normesta
ms.custom: include file
ms.openlocfilehash: 73a8f20122e21c4557289995c15b008401285011
ms.sourcegitcommit: 33368ca1684106cb0e215e3280b828b54f7e73e8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/16/2020
ms.locfileid: "92131642"
---
Sempre use os [grupos de segurança do Azure ad](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-manage-groups) como a entidade atribuída em uma entrada de ACL. Resista a oportunidade de atribuir diretamente a usuários individuais ou entidades de serviço. Usar essa estrutura permitirá que você adicione e remova usuários ou entidades de serviço sem a necessidade de reaplicar as ACLs para uma estrutura de diretório inteiro. Em vez disso, você pode apenas adicionar ou remover usuários e entidades de serviço do grupo de segurança apropriado do Azure AD. 

Há várias maneiras diferentes de configurar grupos. Por exemplo, imagine que você tenha um diretório chamado **/LogData** que contém dados de log gerados pelo servidor. Azure Data Factory (ADF) ingeri os dados nessa pasta. Usuários específicos da equipe de engenharia de serviço carregarão logs e gerenciarão outros usuários dessa pasta, e vários clusters do databricks analisarão os logs dessa pasta. 

Para habilitar essas atividades, você pode criar um `LogsWriter` grupo e um `LogsReader` grupo. Em seguida, você pode atribuir permissões da seguinte maneira:

- Adicione o `LogsWriter` grupo à ACL do diretório **/LogData** com `rwx` permissões.
- Adicione o `LogsReader` grupo à ACL do diretório **/LogData** com `r-x` permissões.
- Adicione o objeto de entidade de serviço ou Identidade de Serviço Gerenciada (MSI) para o ADF ao `LogsWriters` grupo.
- Adicione usuários na equipe de engenharia de serviço ao `LogsWriter` grupo.
- Adicione o objeto de entidade de serviço ou MSI para databricks ao `LogsReader` grupo.

Se um usuário na equipe de engenharia de serviço sair da empresa, você poderá apenas removê-las do `LogsWriter` grupo. Se você não adicionou esse usuário a um grupo, mas, em vez disso, adicionou uma entrada ACL dedicada para esse usuário, você precisaria remover essa entrada ACL do diretório **/LogData** Você também precisaria remover a entrada de todos os subdiretórios e arquivos na hierarquia de diretório inteira do diretório **/LogData** . 

Para criar um grupo e adicionar membros, consulte [criar um grupo básico e adicionar membros usando Azure Active Directory](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-groups-create-azure-portal).
---
title: Conectividade SSL-banco de dados do Azure para MySQL
description: Informações para configurar o Banco de Dados do Azure para MySQL e aplicativos associados a fim de usar as conexões SSL adequadamente
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 12/02/2019
ms.openlocfilehash: d7abed41f3d02382d690f9d7f2a088318632d531
ms.sourcegitcommit: 3eb0cc8091c8e4ae4d537051c3265b92427537fe
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/11/2020
ms.locfileid: "75902578"
---
# <a name="ssl-connectivity-in-azure-database-for-mysql"></a>Conectividade SSL no Banco de Dados do Azure para MySQL
O Banco de Dados do Azure para MySQL dá suporte à conexão de seu servidor de banco de dados com aplicativos cliente usando o protocolo SSL. Impor conexões SSL entre seu servidor de banco de dados e os aplicativos cliente ajuda a proteger contra ataques de "intermediários" criptografando o fluxo de dados entre o servidor e seu aplicativo.

## <a name="default-settings"></a>Configurações padrão
Por padrão, o serviço de banco de dados deve ser configurado para exigir conexões SSL ao se conectar ao MySQL.  Recomendamos evitar desabilitar a opção SSL sempre que possível. 

Ao provisionar um novo servidor de Banco de Dados do Azure para MySQL por meio do Portal e CLI do Azure, a imposição de conexões SSL está habilitada por padrão. 

Cadeias de conexão para várias linguagens de programação são mostradas no Portal do Azure. Essas cadeias de conexão incluem os parâmetros SSL necessários para conectar-se ao banco de dados. No Portal do Azure, selecione seu servidor. No cabeçalho **Configurações**, selecione as **Cadeias de conexão**. O parâmetro SSL varia de acordo com o conector, por exemplo "ssl=true" ou "sslmode=require" ou "sslmode=required" e outras variações.

> [!NOTE]
> Atualmente, a versão do TLS com suporte para o banco de dados do Azure para MySQL é TLS 1,0, TLS 1,1, TLS 1,2

Para saber como habilitar ou desabilitar a conexão SSL durante o desenvolvimento de aplicativos, consulte [Como configurar o SSL](howto-configure-ssl.md). 

## <a name="next-steps"></a>Próximos passos
[Bibliotecas de conexão para o Banco de Dados do Azure para MySQL](concepts-connection-libraries.md)

---
title: Conectar-se aos servidores dos Azure Analysis Services | Microsoft Docs
description: Saiba como se conectar e obter dados de um servidor do Azure Analysis Services.
author: minewiskan
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 03/30/2020
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 2875959a833134131fa6db6d51df0c2b47a39377
ms.sourcegitcommit: 27bbda320225c2c2a43ac370b604432679a6a7c0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80411307"
---
# <a name="connecting-to-servers"></a>Conectando-se a servidores

Este artigo descreve a conexão a um servidor usando aplicativos de modelagem e gerenciamento de dados como SQL Server Management Studio (SSMS) ou Visual Studio com projetos de Serviços de Análise, ou com aplicativos de relatórios de clientes como Microsoft Excel, Power BI Desktop ou aplicativos personalizados. Conexões ao Azure Analysis Services usam HTTPS.

## <a name="client-libraries"></a>Bibliotecas de cliente

[Obter as bibliotecas de Cliente mais recentes](analysis-services-data-providers.md)

Todas as conexões a um servidor, independentemente do tipo, exigem bibliotecas de cliente OLEDB, ADOMD.NET e AMO atualizadas para a conexão e interface com um servidor do Analysis Services. Para SSMS, Visual Studio, Excel 2016 e posterior, e Power BI, as bibliotecas de clientes mais recentes são instaladas ou atualizadas com lançamentos mensais. No entanto, em alguns casos, é possível que um aplicativo não tenha a mais recente. Por exemplo, quando políticas atrasam atualizações ou, as atualizações do Office 365 estão no Canal Adiado.

## <a name="server-name"></a>Nome do servidor

Quando você cria um servidor do Azure Analysis Services, é necessário especificar um nome exclusivo e a região na qual o servidor será criado. Ao especificar o nome do servidor em uma conexão, o esquema de nomenclatura do servidor será:

```
<protocol>://<region>/<servername>
```
 Quando o protocolo for a cadeia de caracteres **asazure**, a região será o URI de onde o servidor foi criado (por exemplo, westus.asazure.windows.net) e servername será o nome do seu servidor exclusivo dentro da região.

### <a name="get-the-server-name"></a>Obter o nome do servidor

No **Portal do Azure** > servidor > **Visão geral** > **Nome do servidor**, copie todo o nome do servidor. Se outros usuários em sua organização também forem se conectar a esse servidor, compartilhe o nome do servidor com eles. Ao especificar um nome de servidor, todo o caminho deve ser usado.

![Obter o nome do servidor no Azure](./media/analysis-services-deploy/aas-deploy-get-server-name.png)

> [!NOTE]
> O protocolo para a região leste dos EUA 2 é **aspaaseastus2**.

## <a name="connection-string"></a>Cadeia de conexão

Ao conectar-se ao Azure Analysis Services usando o Modelo de objeto de tabela, use os seguintes formatos de cadeia de conexão:

###### <a name="integrated-azure-active-directory-authentication"></a>Autenticação integrada do Azure Active Directory

A autenticação integrada seleciona o cache de credencial do Azure Active Directory, se estiver disponível. Caso contrário, a janela de logon do Azure será exibida.

```
"Provider=MSOLAP;Data Source=<Azure AS instance name>;"
```


###### <a name="azure-active-directory-authentication-with-username-and-password"></a>Autenticação do Azure Active Directory com nome de usuário e senha

```
"Provider=MSOLAP;Data Source=<Azure AS instance name>;User ID=<user name>;Password=<password>;Persist Security Info=True; Impersonation Level=Impersonate;";
```

###### <a name="windows-authentication-integrated-security"></a>Autenticação do Windows (segurança integrada)

Use a conta do Windows executando o processo atual.

```
"Provider=MSOLAP;Data Source=<Azure AS instance name>; Integrated Security=SSPI;Persist Security Info=True;"
```

## <a name="connect-using-an-odc-file"></a>Conectar usando um arquivo .odc

Com versões mais antigas do Excel, os usuários podem se conectar a um servidor do Azure Analysis Service usando um arquivo Office Data Connectionn (.odc). Para obter mais informações, consulte [Criar um arquivo Office Data Connection (.odc)](analysis-services-odc.md).


## <a name="next-steps"></a>Próximas etapas

[Conecte-se com o Excel](analysis-services-connect-excel.md)    
[Conecte-se com power bi](analysis-services-connect-pbi.md)   
[Gerenciar seu serviço](analysis-services-manage.md)   


---
title: Recursos, funções e controle de acesso no Azure Application Insights | Microsoft Docs
description: Proprietários, colaboradores e leitores de percepções de sua organização.
ms.service: azure-monitor
ms.subservice: application-insights
ms.topic: conceptual
author: mrbullwinkle
ms.author: mbullwin
ms.date: 02/14/2019
ms.openlocfilehash: 28eb7e490890b46a4025a6cf62e13b364bd7b6dd
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75432353"
---
# <a name="resources-roles-and-access-control-in-application-insights"></a>Recursos, funções e controle de acesso no Application Insights

Você pode controlar quem tem acesso de leitura e atualização aos seus dados no Azure [Application insights][start], usando o [controle de acesso baseado em função no Microsoft Azure](../../role-based-access-control/role-assignments-portal.md).

> [!IMPORTANT]
> Atribua acesso aos usuários no **grupo de recursos ou assinatura** ao qual o recurso do aplicativo pertence, não no próprio recurso. Atribua a função **Colaborador de componente do Application Insights** . Isso garante o controle uniforme de acesso a testes na Web e alertas, juntamente com o recurso do aplicativo. [Saiba mais](#access).


[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="resources-groups-and-subscriptions"></a>Recursos, grupos e assinaturas

Primeiro, algumas definições:

* **Recurso** ‒ uma instância de um serviço do Microsoft Azure. O recurso do Application Insights coleta, analisa e exibe os dados de telemetria enviados de seu aplicativo.  Outros tipos de recursos do Azure incluem aplicativos Web, bancos de dados e VMs.
  
    Para ver seus recursos, abra o [portal do Azure][portal], entre e clique em todos os recursos. Para encontrar um recurso, digite uma parte de seu nome no campo de filtro.
  
    ![Lista de recursos do Azure](./media/resources-roles-access-control/10-browse.png)

<a name="resource-group"></a>

* [**Grupo de recursos**][group] -cada recurso pertence a um grupo. Um grupo é uma maneira conveniente de gerenciar recursos relacionados, particularmente para controle de acesso. Por exemplo, em um grupo de recursos, você pode colocar um aplicativo Web, um recurso do Application Insights para monitorar o aplicativo e um Recurso de armazenamento para manter os dados exportados.

* [**Assinatura**](https://portal.azure.com) - para usar o Application Insights ou outros recursos do Azure, entre em uma assinatura do Azure. Cada grupo de recursos pertence a uma assinatura do Azure, em que você escolhe o pacote de preços e, se for uma assinatura de organização, escolhe os membros e suas permissões de acesso.
* [**Conta Microsoft**][account] -o nome de usuário e a senha que você usa para entrar em assinaturas do Microsoft Azure, Xbox Live, Outlook.com e outros serviços da Microsoft.

## <a name="access"></a> Controlar o acesso no grupo de recursos

É importante entender que, além do recurso criado para seu aplicativo, também há recursos ocultos separados para alertas e testes na Web. Eles são anexados ao mesmo [grupo de recursos](#resource-group) que o recurso de Application insights. Você também pode colocar outros serviços do Azure nele, como sites ou armazenamento.

Portanto, para controlar o acesso a esses recursos, é recomendável:

* Controlar o acesso no nível de **grupo de recursos ou assinatura** .
* Atribuir a função **Colaborador de componente do Application Insights** aos usuários. Isso permite editar testes na Web, alertas e recursos do Application Insights, sem fornecer acesso a qualquer outro serviço no grupo.

## <a name="to-provide-access-to-another-user"></a>Para fornecer acesso a outro usuário

Você deve ter direitos de Proprietário para a assinatura ou o grupo de recursos.

O usuário deve ter uma [conta da Microsoft][account]ou acesso à sua [conta organizacional da Microsoft](../../active-directory/fundamentals/sign-up-organization.md). Você pode fornecer acesso a indivíduos e também a grupos de usuários definidos no Azure Active Directory.

#### <a name="navigate-to-resource-group-or-directly-to-the-resource-itself"></a>Navegue até o grupo de recursos ou diretamente ao próprio recurso

Escolha **Controle de acesso (IAM)** no menu à esquerda.

![Botão de controle da captura de tela de acesso no portal do Azure](./media/resources-roles-access-control/0001-access-control.png)

Selecione **Adicionar atribuição de função**

![Captura de tela do menu de controle de acesso com o botão Adicionar destacado em vermelho](./media/resources-roles-access-control/0002-add.png)

A visualização **Adicionar permissões** é principalmente específica dos recursos do Application Insights. Se você visualizasse as permissões de controle de acesso de um nível superior, como grupos de recursos, veria funções adicionais não baseadas no Application Insights.

Para exibir informações sobre todas as funções internas do controle de acesso baseado em função do Azure, use o [conteúdo de referência oficial](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles).

![Lista de função de usuário de controle de captura de tela de acesso](./media/resources-roles-access-control/0003-user-roles.png)

#### <a name="select-a-role"></a>Selecione uma função

Quando aplicável, vincular a documentação de referência oficial associada.

| Função | No grupo de recursos |
| --- | --- |
| [Proprietário](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#owner) |Pode alterar qualquer item, incluindo o acesso do usuário. |
| [Colaborador](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#contributor) |Pode editar qualquer coisa, incluindo todos os recursos. |
| [Colaborador do Componente do Insights do Aplicativo](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#application-insights-component-contributor) |É possível editar recursos, testes da web e alertas do Application Insights. |
| [Leitor](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#reader) |Pode exibir, mas não alterar nada. |
| [Depurador de Instantâneos do Application Insights](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#application-insights-snapshot-debugger) | Concede ao usuário permissão para usar os recursos do Depurador de Captura Instantânea do Application Insights. Observe que essa função não está incluída nas funções Owner e Contributor. |
| Colaborador do Gerenciamento de Liberação do Azure Service Deploy | Função de contribuição para a implantação de serviços por meio do Azure Service Deploy. |
| [Limpador de Dados](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#data-purger) | Função especial para limpar os dados pessoais. Consulte nosso [orientação para os dados pessoais](https://docs.microsoft.com/azure/application-insights/app-insights-customer-data) para obter mais informações.   |
| Administrador de ExpressRoute | Pode excluir de criar e gerenciar as rotas expressas.|
| [Colaborador do Log Analytics](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#log-analytics-contributor) | O Colaborador do Log Analytics pode ler todos os dados de monitoramento e editar as configurações de monitoramento. A edição das configurações de monitoramento inclui a adição da extensão da VM às VMs, leitura das chaves da conta de armazenamento para poder configurar a coleção de logs do Armazenamento do Microsoft Azure, criação e configuração de contas de Automação, adição de soluções e configuração do diagnóstico do Azure em todos os recursos do Azure.  |
| [Leitor do Log Analytics](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#log-analytics-reader) | Um Leitor do Log Analytics pode exibir e pesquisar todos os dados de monitoramento além de exibir as configurações de monitoramento, incluindo a exibição da configuração do diagnóstico do Azure em todos os recursos do Azure. |
| masterreader | Permite que um usuário visualize tudo, mas não faça alterações. |
| [Colaborador de monitoramento](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#monitoring-contributor) | Pode ler todos os dados de monitoramento e atualizar as configurações de monitoramento. |
| [Publicador de Métricas de Monitoramento](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#monitoring-metrics-publisher) | Permite publicar métricas em relação aos recursos do Azure. |
| [Leitor de monitoramento](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#monitoring-reader) | Pode ler todos os dados de monitoramento. |
| Colaborador da Política de Recursos (Versão prévia) | Os usuários preenchidos da EA, com direitos para criar / modificar a política de recursos, criam tickets de suporte e leem recursos / hierarquias.  |
| [Administrador de Acesso do Usuário](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#user-access-administrator) | Permite que um usuário gerencie o acesso de outros usuários aos recursos do Azure.|
| [Colaborador do Site](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#website-contributor) | Permite gerenciar sites (não planos da web), mas não acessá-los.|

A 'edição' inclui a criação, a exclusão e a atualização:

* Implante
* Testes da Web
* Alertas
* Exportação contínua

#### <a name="select-the-user"></a>Selecione o usuário

Se o usuário desejado não estiver no diretório, você poderá convidar qualquer pessoa com uma conta da Microsoft.
(Se ela usa serviços como Outlook.com, OneDrive, Windows Phone ou XBox Live, já tem uma conta da Microsoft.)

## <a name="related-content"></a>Conteúdo relacionado

* [Controle de acesso baseado em função no Azure](../../role-based-access-control/role-assignments-portal.md)

## <a name="powershell-query-to-determine-role-membership"></a>Consulta do PowerShell para determinar a associação de função

Já que determinadas funções podem ser vinculadas a notificações e alertas de email, talvez seja útil gerar uma lista dos usuários que pertencem a uma certa função. Para ajudar a gerar esses tipos de listas, oferecemos os seguintes exemplos de consultas que podem ser ajustados para atender às suas necessidades específicas:

### <a name="query-entire-subscription-for-admin-roles--contributor-roles"></a>Consulte a assinatura inteira para encontrar funções de administrador + funções de colaborador

```powershell
(Get-AzRoleAssignment -IncludeClassicAdministrators | Where-Object {$_.RoleDefinitionName -in @('ServiceAdministrator', 'CoAdministrator', 'Owner', 'Contributor') } | Select -ExpandProperty SignInName | Sort-Object -Unique) -Join ", "
```

### <a name="query-within-the-context-of-a-specific-application-insights-resource-for-owners-and-contributors"></a>Consulte dentro do contexto de um recurso específico do Application Insights para encontrar os proprietários e colaboradores

```powershell
$resourceGroup = "RGNAME"
$resourceName = "AppInsightsName"
$resourceType = "microsoft.insights/components"
(Get-AzRoleAssignment -ResourceGroup $resourceGroup -ResourceType $resourceType -ResourceName $resourceName | Where-Object {$_.RoleDefinitionName -in @('Owner', 'Contributor') } | Select -ExpandProperty SignInName | Sort-Object -Unique) -Join ", "
```

### <a name="query-within-the-context-of-a-specific-resource-group-for-owners-and-contributors"></a>Consulte dentro do contexto de um grupo de recursos específico para encontrar os proprietários e colaboradores

```powershell
$resourceGroup = "RGNAME"
(Get-AzRoleAssignment -ResourceGroup $resourceGroup | Where-Object {$_.RoleDefinitionName -in @('Owner', 'Contributor') } | Select -ExpandProperty SignInName | Sort-Object -Unique) -Join ", "
```

<!--Link references-->

[account]: https://account.microsoft.com
[group]: ../../azure-resource-manager/management/overview.md
[portal]: https://portal.azure.com/
[start]: ../../azure-monitor/app/app-insights-overview.md

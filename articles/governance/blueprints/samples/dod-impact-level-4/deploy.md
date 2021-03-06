---
title: Modelo do blueprint do Nível de Impacto 4 do DoD
description: Etapas de implantação do modelo de blueprint do Nível de Impacto 4 do DoD, incluindo detalhes do parâmetro do artefato de blueprint.
ms.date: 09/17/2020
ms.topic: sample
ms.openlocfilehash: 7ab2e5967031b52bcad7c1b6f38b546cb8a7eb86
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/09/2020
ms.locfileid: "90978403"
---
# <a name="deploy-the-dod-impact-level-4-blueprint-sample"></a>Implantar o modelo de blueprint do Nível de Impacto 4 do DoD

Para implantar o modelo do blueprint do Departamento de Defesa do Azure Blueprints Nível de Impacto 4 do DoD, as seguintes etapas devem ser seguidas:

> [!div class="checklist"]
> - Criar um blueprint com base na amostra
> - Marcar sua cópia do exemplo como **Publicado**
> - Atribuir a sua cópia do blueprint a uma assinatura existente

Se você não tiver uma assinatura do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free) antes de começar.

## <a name="create-blueprint-from-sample"></a>Criar um blueprint com base na amostra

Primeiro, implemente a amostra de blueprint criando um blueprint no ambiente usando a amostra como ponto de partida.

1. Selecione **Todos os serviços** no painel esquerdo. Pesquise e selecione **Blueprints**.

1. Na página **Introdução** à esquerda, selecione o botão **Criar** em _Criar um blueprint_.

1. Encontre o modelo de blueprint **Nível de Impacto 4 do DoD** em _Outras Amostras_ e selecione **Usar esta amostra**.

1. Insira as informações _Básicas_ do exemplo de blueprint:

   - **Nome do blueprint**: Forneça um nome para a sua cópia do modelo de blueprint Nível de Impacto 4 do DoD.
   - **Localização da definição**: Use as reticências e selecione o grupo de gerenciamento em que deseja salvar a cópia da amostra.

1. Selecione a guia _Artefatos_ na parte superior da página ou clique em **Avançar: Artefatos** na parte inferior da página.

1. Examine a lista de artefatos que compõem o exemplo de blueprint. Muitos dos artefatos têm parâmetros que definiremos mais tarde. Selecione **Salvar Rascunho** quando terminar de examinar o exemplo de blueprint.

## <a name="publish-the-sample-copy"></a>Publicar a cópia do exemplo

Agora a cópia do exemplo de blueprint foi criada em seu ambiente. Ela é criada no modo **Rascunho** e deve ser **Publicada** antes de ser atribuída e implantada. A cópia da amostra de blueprint pode ser personalizada de acordo com seu ambiente e suas necessidades, mas essa modificação poderá desviá-lo dos controles Nível de Impacto 4 do DoD.

1. Selecione **Todos os serviços** no painel esquerdo. Pesquise e selecione **Blueprints**.

1. Selecione a página **Definições de Blueprint** à esquerda. Use os filtros para localizar a cópia da amostra de blueprint e, em seguida, selecione-a.

1. Selecione **Publicar blueprint** na parte superior da página. Na nova página à direita, informe a **Versão** da sua cópia da amostra de blueprint. Essa propriedade será útil se você fizer uma modificação mais tarde. Forneça **Notas de alterações**, como "Primeira versão publicada do modelo de blueprint do DoD IL4". Em seguida, selecione **Publicar** na parte inferior da página.

## <a name="assign-the-sample-copy"></a>Atribuir a cópia de exemplo

Quando a cópia do exemplo de blueprint for **Publicada** com êxito, ele poderá ser atribuído a uma assinatura do grupo de gerenciamento em que ele foi salvo. Esta é a etapa em que os parâmetros são fornecidos para tornar exclusiva cada implantação da cópia do exemplo de blueprint.

1. Selecione **Todos os serviços** no painel esquerdo. Pesquise e selecione **Blueprints**.

1. Selecione a página **Definições de Blueprint** à esquerda. Use os filtros para localizar a cópia da amostra de blueprint e, em seguida, selecione-a.

1. Selecione **Atribuir blueprint** na parte superior da página de definição de blueprint.

1. Forneça os valores de parâmetro para a atribuição de blueprint:

   - Noções básicas

     - **Assinaturas**: Selecione uma ou mais das assinaturas que estão no grupo de gerenciamento em que você salvou a cópia do exemplo de blueprint. Se você selecionar mais de uma assinatura, será criada uma atribuição para cada uma, usando os parâmetros inseridos.
     - **Nome da atribuição**: O nome é pré-preenchido para você com base no nome do blueprint.
       Altere-o conforme necessário ou mantenha-o como está.
     - **Localização**: Selecione uma região para a identidade gerenciada a ser criada. O Blueprint do Azure usa essa identidade gerenciada para implantar todos os artefatos no blueprint atribuído. Para saber mais, veja [identidades gerenciadas para recursos do Azure](../../../../active-directory/managed-identities-azure-resources/overview.md).
     - **Versão de definição de blueprint**: Escolha uma versão **Publicada** da cópia da amostra de blueprint.

   - Bloquear atribuição

     Selecione a configuração de bloqueio de blueprint para o seu ambiente. Para obter mais informações, consulte [bloqueio de recursos de projetos](../../concepts/resource-locking.md).

   - Identidade Gerenciada

     Deixe a opção de identidade gerenciada _atribuída ao sistema_ padrão.

   - Parâmetros do artefato

     Os parâmetros definidos nesta seção se aplicam ao artefato sob o qual ele está definido. Esses são [parâmetros dinâmicos](../../concepts/parameters.md#dynamic-parameters), pois são definidos durante a atribuição do blueprint. Para obter uma lista completa ou parâmetros de artefato e suas descrições, confira a [Tabela de parâmetros de artefato](#artifact-parameters-table).

1. Depois que todos os parâmetros forem inseridos, selecione **Atribuir** na parte inferior da página. A atribuição de blueprint é criada, e a implantação de artefato é iniciada. A implantação leva aproximadamente uma hora. Para verificar o status da implantação, abra a atribuição de blueprint.

> [!WARNING]
> As amostras internas de blueprint e o serviço Azure Blueprints são **gratuitos**. Os recursos do Azure são [precificados por produto](https://azure.microsoft.com/pricing/). Use a [Calculadora de Preços](https://azure.microsoft.com/pricing/calculator/) para estimar o custo da execução de recursos implantados por essa amostra de blueprint.

## <a name="artifact-parameters-table"></a>Tabela de parâmetros de artefato

A seguinte tabela fornece uma lista dos parâmetros de artefato de blueprint:

|Nome do artefato|Tipo de artefato|Nome do parâmetro|Descrição|
|-|-|-|-|
|Locais permitidos|Atribuição de política|Locais permitidos|Esta política permite que você restrinja os locais que sua organização pode especificar ao implantar recursos. Use para impor seus requisitos de conformidade geográfica.|
|Localizações permitidas para grupos de recursos|Atribuição de política |Locais permitidos|Esta política permite que você restrinja os locais em que sua organização pode criar grupos de recursos. Use para impor seus requisitos de conformidade geográfica.|
|Implantar Auditoria em servidores SQL|Atribuição de política|O valor em dias do período de retenção (0 é uma indicação de retenção ilimitada)|Dias de retenção (opcional, 180 dias se não for especificado)|
|Implantar Auditoria em servidores SQL|Atribuição de política|Nome do grupo de recursos da conta de armazenamento para auditoria do SQL Server|A auditoria grava eventos de banco de dados em um log de auditoria na sua conta de Armazenamento do Azure (uma conta de armazenamento será criada em cada região onde um SQL Server é criado e será compartilhada por todos os servidores na região). Importante: para um funcionamento adequado da Auditoria, não exclua nem renomeie o grupo de recursos nem as contas de armazenamento.|
|Implantar configurações de diagnóstico para Grupos de Segurança de Rede|Atribuição de política|Prefixo da conta de armazenamento para diagnóstico do grupo de segurança de rede|Este prefixo será combinado com o local de grupo de segurança de rede para formar o nome da conta de armazenamento criada.|
|Implantar configurações de diagnóstico para Grupos de Segurança de Rede|Atribuição de política|Nome do grupo de recursos da conta de armazenamento para diagnóstico do grupo de segurança de rede (precisa existir)|O nome do grupo de recursos em que a conta de armazenamento será criada. Esse grupo de recursos já precisa existir.|
|Implantar o agente do Log Analytics em conjuntos de dimensionamento de máquinas virtuais do Linux|Atribuição de política|Workspace do Log Analytics para conjuntos de dimensionamento de máquinas virtuais do Linux|Se este workspace estiver fora do escopo da atribuição, você deverá conceder permissões de 'Colaborador do Log Analytics' (ou semelhantes) à ID da entidade de segurança da atribuição da política.|
|Implantar o agente do Log Analytics em conjuntos de dimensionamento de máquinas virtuais do Linux|Atribuição de política|Opcional: Lista de imagens de VM compatíveis com o sistema operacional Linux a serem adicionadas ao escopo|Uma matriz vazia pode ser usada para indicar a inexistência de parâmetros opcionais: \[\]|
|Implantar o Agente do Log Analytics para VMs do Linux|Atribuição de política|Workspace do Log Analytics para VMs Linux|Se este workspace estiver fora do escopo da atribuição, você deverá conceder permissões de 'Colaborador do Log Analytics' (ou semelhantes) à ID da entidade de segurança da atribuição da política.|
|Implantar o Agente do Log Analytics para VMs do Linux|Atribuição de política|Opcional: Lista de imagens de VM compatíveis com o sistema operacional Linux a serem adicionadas ao escopo|Uma matriz vazia pode ser usada para indicar a inexistência de parâmetros opcionais: \[\]|
|Implantar o agente do Log Analytics em conjuntos de dimensionamento de máquinas virtuais do Windows|Atribuição de política|Workspace do Log Analytics para conjuntos de dimensionamento de máquinas virtuais do Windows|Se este workspace estiver fora do escopo da atribuição, você deverá conceder permissões de 'Colaborador do Log Analytics' (ou semelhantes) à ID da entidade de segurança da atribuição da política.|
|Implantar o agente do Log Analytics em conjuntos de dimensionamento de máquinas virtuais do Windows|Atribuição de política|Opcional: Lista de imagens de VM compatíveis com o sistema operacional Windows a serem adicionadas ao escopo|Uma matriz vazia pode ser usada para indicar a inexistência de parâmetros opcionais: \[\]|
|Implantar o Agente do Log Analytics para VMs do Windows|Atribuição de política|Workspace do Log Analytics para VMs do Windows|Se este workspace estiver fora do escopo da atribuição, você deverá conceder permissões de 'Colaborador do Log Analytics' (ou semelhantes) à ID da entidade de segurança da atribuição da política.|
|Implantar o Agente do Log Analytics para VMs do Windows|Atribuição de política|Opcional: Lista de imagens de VM compatíveis com o sistema operacional Windows a serem adicionadas ao escopo|Uma matriz vazia pode ser usada para indicar a inexistência de parâmetros opcionais: \[\]|
|\[Versão Prévia\]: Nível de Impacto 4 do DoD|Atribuição de política|Membros a serem incluídos no Grupo local de administradores|Uma lista separada por ponto e vírgula de membros que devem ser excluídos do grupo local de Administradores. Por exemplo: Administrador; myUser1; myUser2|
|\[Versão Prévia\]: Nível de Impacto 4 do DoD|Atribuição de política|Membros que devem ser excluídos do Grupo local de administradores|Uma lista separada por ponto e vírgula de membros que devem ser incluídos no grupo local de Administradores. Por exemplo: Administrador; myUser1; myUser2|
|\[Versão Prévia\]: Nível de Impacto 4 do DoD|Atribuição de política|Lista de tipos de recurso que devem ter os logs de diagnóstico habilitados|Lista de tipos de recurso a serem auditados se a configuração do log de diagnóstico não estiver habilitada. Os valores aceitáveis podem ser encontrados em [Esquemas de logs de diagnóstico do Azure Monitor](../../../../azure-monitor/platform/resource-logs-schema.md#service-specific-schemas).|
|\[Versão Prévia\]: Nível de Impacto 4 do DoD|Atribuição de política|ID do workspace do Log Analytics para a qual as VMs devem ser configuradas|Esta é a ID (GUID) do workspace do Log Analytics para a qual as VMs devem ser configuradas.|
|\[Versão Prévia\]: Nível de Impacto 4 do DoD|Atribuição de política|O backup com redundância geográfica de longo prazo deve ser habilitado para os Bancos de Dados SQL do Azure|As informações sobre os efeitos da política podem [Compreender os efeitos do Azure Policy](../../../policy/concepts/effects.md).|
|\[Versão Prévia\]: Nível de Impacto 4 do DoD|Atribuição de política|A avaliação de vulnerabilidades deve ser habilitada nas instâncias gerenciadas do SQL|As informações sobre os efeitos da política podem [Compreender os efeitos do Azure Policy](../../../policy/concepts/effects.md).|
|\[Versão Prévia\]: Nível de Impacto 4 do DoD|Atribuição de política|A avaliação da vulnerabilidade deve ser habilitada nos servidores SQL|As informações sobre os efeitos da política podem [Compreender os efeitos do Azure Policy](../../../policy/concepts/effects.md).|
|\[Versão Prévia\]: Nível de Impacto 4 do DoD|Atribuição de política|O armazenamento com redundância geográfica deve ser habilitado para Contas de Armazenamento|As informações sobre os efeitos da política podem [Compreender os efeitos do Azure Policy](../../../policy/concepts/effects.md).|
|\[Versão Prévia\]: Nível de Impacto 4 do DoD|Atribuição de política|O backup com redundância geográfica deve ser habilitado para o Banco de Dados do Azure para MySQL|As informações sobre os efeitos da política podem [Compreender os efeitos do Azure Policy](../../../policy/concepts/effects.md).|
|\[Versão Prévia\]: Nível de Impacto 4 do DoD|Atribuição de política|O backup com redundância geográfica deve ser habilitado para o Banco de Dados do Azure para PostgreSQL|As informações sobre os efeitos da política podem [Compreender os efeitos do Azure Policy](../../../policy/concepts/effects.md).|
|\[Versão Prévia\]: Nível de Impacto 4 do DoD|Atribuição de política|Aplicativo Web deve ser acessível somente por HTTPS|As informações sobre os efeitos da política podem [Compreender os efeitos do Azure Policy](../../../policy/concepts/effects.md).|
|\[Versão Prévia\]: Nível de Impacto 4 do DoD|Atribuição de política|O aplicativo de funções deve ser acessível apenas por HTTPS|As informações sobre os efeitos da política podem [Compreender os efeitos do Azure Policy](../../../policy/concepts/effects.md).|
|\[Versão Prévia\]: Nível de Impacto 4 do DoD|Atribuição de política|As contas externas com permissões de gravação devem ser removidas de sua assinatura|As informações sobre os efeitos da política podem [Compreender os efeitos do Azure Policy](../../../policy/concepts/effects.md).|
|\[Versão Prévia\]: Nível de Impacto 4 do DoD|Atribuição de política|As contas externas com permissões de leitura devem ser removidas de sua assinatura|As informações sobre os efeitos da política podem [Compreender os efeitos do Azure Policy](../../../policy/concepts/effects.md).|
|\[Versão Prévia\]: Nível de Impacto 4 do DoD|Atribuição de política|As contas externas com permissões de proprietário devem ser removidas de sua assinatura|As informações sobre os efeitos da política podem [Compreender os efeitos do Azure Policy](../../../policy/concepts/effects.md).|
|\[Versão Prévia\]: Nível de Impacto 4 do DoD|Atribuição de política|As contas preteridas com permissões de proprietário devem ser removidas de sua assinatura|As informações sobre os efeitos da política podem [Compreender os efeitos do Azure Policy](../../../policy/concepts/effects.md).|
|\[Versão Prévia\]: Nível de Impacto 4 do DoD|Atribuição de política|As contas preteridas devem ser removidas de sua assinatura|As informações sobre os efeitos da política podem [Compreender os efeitos do Azure Policy](../../../policy/concepts/effects.md).|
|\[Versão Prévia\]: Nível de Impacto 4 do DoD|Atribuição de política|O CORS não deve permitir que todos os recursos acessem seu aplicativo Web|As informações sobre os efeitos da política podem [Compreender os efeitos do Azure Policy](../../../policy/concepts/effects.md).|
|\[Versão Prévia\]: Nível de Impacto 4 do DoD|Atribuição de política|As atualizações do sistema nos conjuntos de dimensionamento de máquinas virtuais devem ser instaladas|As informações sobre os efeitos da política podem [Compreender os efeitos do Azure Policy](../../../policy/concepts/effects.md).|
|\[Versão Prévia\]: Nível de Impacto 4 do DoD|Atribuição de política|O MFA deve ser habilitado em contas com permissões de leitura em sua assinatura|As informações sobre os efeitos da política podem [Compreender os efeitos do Azure Policy](../../../policy/concepts/effects.md).|
|\[Versão Prévia\]: Nível de Impacto 4 do DoD|Atribuição de política|O MFA deve ser habilitado em contas com permissões de proprietário em sua assinatura|As informações sobre os efeitos da política podem [Compreender os efeitos do Azure Policy](../../../policy/concepts/effects.md).|
|\[Versão Prévia\]: Nível de Impacto 4 do DoD|Atribuição de política|O MFA deve ser habilitado em contas com permissões de gravação em sua assinatura|As informações sobre os efeitos da política podem [Compreender os efeitos do Azure Policy](../../../policy/concepts/effects.md).|

## <a name="next-steps"></a>Próximas etapas

Agora que você examinou as etapas para implantar a amostra do blueprint do Nível de Impacto 4 do DoD, leia os seguintes artigos para saber mais sobre o blueprint e o mapeamento de controle:

> [!div class="nextstepaction"]
> [Blueprint do Nível de Impacto 4 do DoD – Visão Geral](./index.md)
> [Blueprint do Nível de Impacto 4 do DoD – Mapeamento de controle](./control-mapping.md)

Outros artigos sobre blueprints e como usá-los:

- Saiba mais sobre o [ciclo de vida do blueprint](../../concepts/lifecycle.md).
- Saiba como usar [parâmetros estáticos e dinâmicos](../../concepts/parameters.md).
- Saiba como personalizar a [ordem de sequenciamento de blueprint](../../concepts/sequencing-order.md).
- Saiba como usar o [bloqueio de recurso de blueprint](../../concepts/resource-locking.md).
- Saiba como [atualizar atribuições existentes](../../how-to/update-existing-assignments.md).

---
title: Configurar uma instância e uma autenticação (portal)
titleSuffix: Azure Digital Twins
description: Consulte como configurar uma instância do serviço gêmeos do Azure digital usando o portal do Azure
author: baanders
ms.author: baanders
ms.date: 7/23/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: c67add18dc653cc033d0cf4990f9c44f07633ac2
ms.sourcegitcommit: 2e72661f4853cd42bb4f0b2ded4271b22dc10a52
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/14/2020
ms.locfileid: "92047396"
---
# <a name="set-up-an-azure-digital-twins-instance-and-authentication-portal"></a>Configurar uma instância e autenticação do gêmeos digital do Azure (Portal)

[!INCLUDE [digital-twins-setup-selector.md](../../includes/digital-twins-setup-selector.md)]

Este artigo aborda as etapas para **Configurar uma nova instância de gêmeos digital do Azure**, incluindo a criação da instância e a configuração da autenticação. Depois de concluir este artigo, você terá uma instância do gêmeos digital do Azure pronta para começar a programar.

Esta versão deste artigo percorre essas etapas manualmente, uma a uma, usando o portal do Azure. O portal do Azure é um console unificado baseado na Web que fornece uma alternativa para as ferramentas de linha de comando.
* Para percorrer essas etapas manualmente usando a CLI, consulte a versão da CLI deste artigo: [*como configurar uma instância e autenticação (CLI)*](how-to-set-up-instance-cli.md).
* Para executar uma configuração automatizada usando um exemplo de script de implantação, consulte a versão com script deste artigo: [*como: configurar uma instância e autenticação (script)*](how-to-set-up-instance-scripted.md).

[!INCLUDE [digital-twins-setup-steps-prereq.md](../../includes/digital-twins-setup-steps-prereq.md)]

## <a name="create-the-azure-digital-twins-instance"></a>Criar a instância de gêmeos digital do Azure

Nesta seção, você **criará uma nova instância do Azure digital gêmeos** usando o [portal do Azure](https://ms.portal.azure.com/). Navegue até o portal e faça logon com suas credenciais.

Uma vez no portal, comece selecionando _criar um recurso_ no menu Home Page de serviços do Azure.

:::image type="content" source= "media/how-to-set-up-instance/portal/create-resource.png" alt-text="Selecionando &quot;criar um recurso&quot; na home page da portal do Azure":::

Pesquise *gêmeos digital do Azure* na caixa de pesquisa e escolha o serviço **gêmeos (versão prévia) do Azure digital** dos resultados. Selecione o botão _criar_ para criar uma nova instância do serviço.

:::image type="content" source= "media/how-to-set-up-instance/portal/create-azure-digital-twins.png" alt-text="Selecionando &quot;criar um recurso&quot; na home page da portal do Azure":::

Na página *criar recurso* a seguir, preencha os valores fornecidos abaixo:
* **Assinatura**: a assinatura do Azure que você está usando
  - **Grupo de recursos**: um grupo de recursos no qual implantar a instância. Se você ainda não tiver um grupo de recursos existente em mente, poderá criar um aqui selecionando o link *criar novo* e inserindo um nome para um novo grupo de recursos
* **Local**: uma região do Azure digital gêmeos habilitada para a implantação. Para obter mais detalhes sobre o suporte regional, visite [*produtos do Azure disponíveis por região (Azure digital gêmeos)*](https://azure.microsoft.com/global-infrastructure/services/?products=digital-twins).
* **Nome do recurso**: um nome para sua instância de gêmeos digital do Azure. O nome da nova instância deve ser exclusivo na região da sua assinatura (ou seja, se sua assinatura tiver outra instância de gêmeos digital do Azure na região que já está usando o nome que você escolher, será solicitado que você escolha um nome diferente).

:::image type="content" source= "media/how-to-set-up-instance/portal/create-azure-digital-twins-2.png" alt-text="Selecionando &quot;criar um recurso&quot; na home page da portal do Azure":::

Quando terminar, selecione _revisar + criar_. Isso levará você a uma página de resumo, na qual você pode examinar os detalhes da instância inseridos e clicar em _criar_. 

### <a name="verify-success-and-collect-important-values"></a>Verificar o êxito e coletar valores importantes

Depois de *enviar por*Push, você pode exibir o status da implantação da sua instância em suas notificações do Azure ao longo da barra de ícones do Portal. A notificação indicará quando a implantação foi bem-sucedida e você poderá selecionar o botão _ir para o recurso_ para exibir a instância criada.

:::image type="content" source="media/how-to-set-up-instance/portal/notifications-deployment.png" alt-text="Selecionando &quot;criar um recurso&quot; na home page da portal do Azure":::

Como alternativa, se a implantação falhar, a notificação indicará o porquê. Observe o aviso da mensagem de erro e tente criar a instância novamente.

>[!TIP]
>Depois que sua instância for criada, você poderá retornar para sua página a qualquer momento pesquisando o nome da sua instância na barra de pesquisa portal do Azure.

Na página *visão geral* da instância, anote seu *nome*, *grupo de recursos*e *nome do host*. Esses são os valores importantes que podem ser necessários à medida que você continuar trabalhando com sua instância de gêmeos digital do Azure. Se outros usuários estiverem programando em relação à instância, você deverá compartilhar esses valores com eles.

:::image type="content" source="media/how-to-set-up-instance/portal/instance-important-values.png" alt-text="Selecionando &quot;criar um recurso&quot; na home page da portal do Azure":::

Agora você tem uma instância de gêmeos digital do Azure pronta para uso. Em seguida, você dará as permissões apropriadas de usuário do Azure para gerenciá-lo.

## <a name="set-up-user-access-permissions"></a>Configurar permissões de acesso do usuário

[!INCLUDE [digital-twins-setup-role-assignment.md](../../includes/digital-twins-setup-role-assignment.md)]

Primeiro, abra a página da instância do gêmeos digital do Azure no portal do Azure. No menu da instância, selecione *controle de acesso (iam)*. Selecione o botão  *Adicionar* em *Adicionar uma atribuição de função*.

:::image type="content" source="media/how-to-set-up-instance/portal/add-role-assignment-1.png" alt-text="Selecionando &quot;criar um recurso&quot; na home page da portal do Azure":::

Na página *Adicionar atribuição de função* a seguir, preencha os valores (deve ser concluído por um usuário com [permissões suficientes](#prerequisites-permission-requirements) na assinatura do Azure):
* **Função**: selecione *proprietário do gêmeos digital do Azure (versão prévia)* no menu suspenso
* **Atribuir acesso a**: selecione *usuário, grupo ou entidade de serviço do Azure ad* no menu suspenso
* **Selecione**: Procure o nome ou endereço de email do usuário a ser atribuído. Quando você seleciona o resultado, o usuário aparecerá em uma seção de *Membros selecionados* .

:::row:::
    :::column:::
        :::image type="content" source="media/how-to-set-up-instance/portal/add-role-assignment-2.png" alt-text="Selecionando &quot;criar um recurso&quot; na home page da portal do Azure":::
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

Quando tiver terminado de inserir os detalhes, clique no botão *salvar* .

### <a name="verify-success"></a>Verificar êxito

Você pode exibir a atribuição de função que configurou sob o *controle de acesso (iam) > atribuições de função*. O usuário deve aparecer na lista com uma função do proprietário do *gêmeos digital do Azure (versão prévia)*. 

:::image type="content" source="media/how-to-set-up-instance/portal/verify-role-assignment.png" alt-text="Selecionando &quot;criar um recurso&quot; na home page da portal do Azure":::

Agora você tem uma instância de gêmeos digital do Azure pronta para uso e atribuiu permissões para gerenciá-la. Em seguida, você configurará permissões para um aplicativo cliente acessá-lo.

## <a name="set-up-access-permissions-for-client-applications"></a>Configurar permissões de acesso para aplicativos cliente

[!INCLUDE [digital-twins-setup-app-registration.md](../../includes/digital-twins-setup-app-registration.md)]

Comece navegando até [Azure Active Directory](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) na portal do Azure (você pode usar este link ou encontrá-lo com a barra de pesquisa do Portal). Selecione *registros de aplicativo* no menu serviço e *+ novo registro*.

:::image type="content" source="media/how-to-set-up-instance/portal/new-registration.png" alt-text="Selecionando &quot;criar um recurso&quot; na home page da portal do Azure":::

Na página *registrar um aplicativo* a seguir, preencha os valores solicitados:
* **Nome**: um nome de exibição do aplicativo do Azure ad a ser associado ao registro
* **Tipos de conta com suporte**: selecione *contas neste diretório organizacional somente (somente diretório padrão-locatário único)*
* **URI de redirecionamento**: uma *URL de resposta do aplicativo do Azure ad* para o aplicativo do Azure AD. Adicione um URI de *cliente público/nativo (mobile & Desktop)* para `http://localhost` .

Quando tiver terminado, pressione o botão *registrar* .

:::image type="content" source="media/how-to-set-up-instance/portal/register-an-application.png" alt-text="Selecionando &quot;criar um recurso&quot; na home page da portal do Azure":::

Quando a configuração do registro for concluída, o portal irá redirecioná-lo para sua página de detalhes.

### <a name="provide-azure-digital-twins-api-permission"></a>Fornecer permissão de API de gêmeos digital do Azure

Em seguida, configure o registro de aplicativo que você criou com permissões de linha de base para as APIs do Azure digital gêmeos.

Na página do portal para o registro do aplicativo, selecione *permissões de API* no menu. Na página permissões a seguir, clique no botão *+ Adicionar uma permissão* .

:::image type="content" source="media/how-to-set-up-instance/portal/add-permission.png" alt-text="Selecionando &quot;criar um recurso&quot; na home page da portal do Azure":::

Na página *solicitar permissões de API* que segue, alterne para a guia *APIs que minha organização usa* e pesquise *gêmeos do Azure digital*. Selecione _**Azure digital gêmeos**_ nos resultados da pesquisa para continuar com a atribuição de permissões para as APIs do gêmeos digital do Azure.

:::image type="content" source="media/how-to-set-up-instance/portal/request-api-permissions-1.png" alt-text="Selecionando &quot;criar um recurso&quot; na home page da portal do Azure":::

>[!NOTE]
> Se sua assinatura ainda tiver uma instância de gêmeos digital do Azure existente da visualização pública anterior do serviço (antes de julho de 2020), você precisará pesquisar e selecionar o _**serviço de espaços inteligentes do Azure**_ em vez disso. Esse é um nome mais antigo para o mesmo conjunto de APIs (Observe que a *ID do aplicativo (cliente)* é a mesma que na captura de tela acima) e sua experiência não será alterada além desta etapa.
> :::image type="content" source="media/how-to-set-up-instance/portal/request-api-permissions-1-smart-spaces.png" alt-text="Selecionando &quot;criar um recurso&quot; na home page da portal do Azure":::

Em seguida, você selecionará quais permissões conceder para essas APIs. Expanda a permissão **Read (1)** e marque a caixa que diz *Read. Write* para conceder a esse aplicativo o leitor de registro e permissões de gravador.

:::image type="content" source="media/how-to-set-up-instance/portal/request-api-permissions-2.png" alt-text="Selecionando &quot;criar um recurso&quot; na home page da portal do Azure":::

Clique em *adicionar permissões* quando terminar.

### <a name="verify-success"></a>Verificar êxito

De volta à página *permissões de API* , verifique se agora há uma entrada para o Azure digital gêmeos que reflete as permissões de leitura/gravação:

:::image type="content" source="media/how-to-set-up-instance/portal/verify-api-permissions.png" alt-text="Selecionando &quot;criar um recurso&quot; na home page da portal do Azure":::

Você também pode verificar a conexão com o gêmeos digital do Azure dentro domanifest.jsdo registro do aplicativo * em*, que foi atualizado automaticamente com as informações do gêmeos digital do Azure quando você adicionou as permissões de API.

Para fazer isso, selecione *manifesto* no menu para exibir o código do manifesto do registro do aplicativo. Role até a parte inferior da janela de código e procure esses campos em `requiredResourceAccess` . Os valores devem corresponder aos da captura de tela abaixo:

:::image type="content" source="media/how-to-set-up-instance/portal/verify-manifest.png" alt-text="Selecionando &quot;criar um recurso&quot; na home page da portal do Azure":::

### <a name="collect-important-values"></a>Coletar valores importantes

Em seguida, selecione *visão geral* na barra de menus para ver os detalhes do registro do aplicativo:

:::image type="content" source="media/how-to-set-up-instance/portal/app-important-values.png" alt-text="Selecionando &quot;criar um recurso&quot; na home page da portal do Azure":::

Anote a ID do *aplicativo (cliente)* e a *ID do diretório (locatário)* mostradas **na página.** Esses valores serão necessários posteriormente para [autenticar um aplicativo cliente em relação às APIs do gêmeos digital do Azure](how-to-authenticate-client.md). Se você não for a pessoa que vai escrever código para tais aplicativos, deverá compartilhar esses valores com a pessoa que será.

### <a name="other-possible-steps-for-your-organization"></a>Outras etapas possíveis para sua organização

[!INCLUDE [digital-twins-setup-additional-requirements.md](../../includes/digital-twins-setup-additional-requirements.md)]

## <a name="next-steps"></a>Próximas etapas

Teste as chamadas de API REST individuais em sua instância usando os comandos da CLI do Azure digital gêmeos: 
* [referência de AZ DT](/cli/azure/ext/azure-iot/dt?preserve-view=true&view=azure-cli-latest)
* [*Como usar a CLI dos Gêmeos Digitais do Azure*](how-to-use-cli.md)

Ou então, consulte Como conectar seu aplicativo cliente à sua instância escrevendo o código de autenticação do aplicativo cliente:
* [*Como: escrever código de autenticação do aplicativo*](how-to-authenticate-client.md)
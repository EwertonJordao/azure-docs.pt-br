---
title: 'Tutorial: provisionamento de usuário – LinkedIn Sales Navigator, Azure AD'
description: Saiba como configurar o Azure Active Directory para provisionar e desprovisionar automaticamente contas de usuário no LinkedIn Sales Navigator.
services: active-directory
documentationcenter: ''
author: ArvindHarinder1
manager: CelesteDG
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/28/2019
ms.author: arvinh
ms.collection: M365-identity-device-management
ms.openlocfilehash: f789f82288c9820214c4ab32c271bb547945d4d1
ms.sourcegitcommit: db2d402883035150f4f89d94ef79219b1604c5ba
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/07/2020
ms.locfileid: "77057372"
---
# <a name="tutorial-configure-linkedin-sales-navigator-for-automatic-user-provisioning"></a>Tutorial: Configurar LinkedIn Sales Navigator para provisionamento automático de usuário

O objetivo deste tutorial é mostrar as etapas que precisam ser executadas no LinkedIn Sales Navigator e no Azure AD para provisionar e desprovisionar automaticamente as contas de usuário do Azure AD para o LinkedIn Sales Navigator.

## <a name="prerequisites"></a>Prerequisites

O cenário descrito neste tutorial pressupõe que você já tem os seguintes itens:

* Um locatário do Azure Active Directory
* Um locatário do LinkedIn Sales Navigator 
* Uma conta de administrador no LinkedIn Sales Navigator com acesso ao Centro de Contas do LinkedIn

> [!NOTE]
> O Azure Active Directory integra-se com o LinkedIn Sales Navigator usando o protocolo [SCIM](http://www.simplecloud.info/).

## <a name="assigning-users-to-linkedin-sales-navigator"></a>Atribuir usuários ao LinkedIn Sales Navigator

O Azure Active Directory usa um conceito chamado "atribuições" para determinar quais usuários devem receber acesso aos aplicativos selecionados. No contexto do provisionamento automático de conta de usuário, somente os usuários e grupos que foram "atribuídos" a um aplicativo no Azure AD serão sincronizados.

Antes de configurar e habilitar o serviço de provisionamento, você precisará decidir quais usuários e/ou grupos no Azure AD representam os usuários que precisam de acesso ao LinkedIn Sales Navigator. Depois de decidir, atribua esses usuários ao LinkedIn Sales Navigator seguindo estas instruções:

[Atribuir um usuário ou um grupo a um aplicativo empresarial](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-linkedin-sales-navigator"></a>Dicas importantes para atribuir usuários ao LinkedIn Sales Navigator

* Recomendamos a atribuição de um único usuário do Azure AD ao LinkedIn Sales Navigator para testar a configuração de provisionamento. Outros usuários e/ou grupos podem ser atribuídos mais tarde.

* Ao atribuir um usuário ao LinkedIn Sales Navigator, selecione a função **Usuário** na caixa de diálogo de atribuição. A função de "Acesso Padrão" não funciona para provisionamento.

## <a name="configuring-user-provisioning-to-linkedin-sales-navigator"></a>Como configurar o provisionamento de usuários no LinkedIn Sales Navigator

Esta seção orienta você pela conexão do Azure AD à API de provisionamento de conta de usuário do SCIM do LinkedIn Sales Navigator e também pela configuração do serviço de provisionamento, a fim de criar, atualizar e desabilitar contas de usuário atribuídas no LinkedIn Sales Navigator com base na atribuição de usuário e de grupo do Azure AD.

> [!TIP]
> Você também pode optar por habilitar o logon único baseado em SAML para o LinkedIn Sales Navigator seguindo as instruções fornecidas no [Portal do Azure](https://portal.azure.com). O logon único pode ser configurado independentemente do provisionamento automático, embora esses dois recursos sejam complementares.

### <a name="to-configure-automatic-user-account-provisioning-to-linkedin-sales-navigator-in-azure-ad"></a>Para configurar o provisionamento de conta de usuário automático para o LinkedIn Sales Navigator no Azure AD:

A primeira etapa é recuperar o token de acesso do LinkedIn. Se você for um administrador corporativo, você poderá provisionar um token de acesso automaticamente. No seu centro de contas, vá para **Configurações &gt; Configurações Globais** e abra o painel **Instalação do SCIM**.

> [!NOTE]
> Se você estiver acessando o centro de contas diretamente em vez de fazê-lo por meio de um link, você poderá alcançá-lo usando as etapas a seguir.

1. Entre no Centro de Contas.

2. Selecione **Administrador &gt; Configurações de Administrador**.

3. Clique em **Integrações Avançadas** na barra lateral esquerda. Você será direcionado para o Centro de Contas.

4. Clique em **+ Adicionar a nova configuração de SCIM** e siga o procedimento preenchendo cada campo.

    > [!NOTE]
    > Quando a atribuição automática de licenças não está habilitada, isso significa que somente os dados de usuário estão sincronizados.

    ![Provisionamento do LinkedIn Sales Navigator](./media/linkedinsalesnavigator-provisioning-tutorial/linkedin_1.PNG)

    > [!NOTE]
    > Quando a atribuição automática de licenças estiver habilitada, você precisa anotar o tipo de licença e a instância do aplicativo. Licenças são atribuídas por ordem de chegada, até que todas as licenças tenham sido utilizadas.

    ![Provisionamento do LinkedIn Sales Navigator](./media/linkedinsalesnavigator-provisioning-tutorial/linkedin_2.PNG)

5. Clique em **Gerar token**. Você deve ver o token de acesso ser exibido sob o campo **Token de acesso**.

6. Salve seu token de acesso para a área de transferência ou o computador antes de sair da página.

7. Em seguida, entre no [Portal do Azure](https://portal.azure.com) e navegue até a seção **Azure Active Directory > Aplicativos Empresariais > Todos os aplicativos**.

8. Se você já tiver configurado o LinkedIn Sales Navigator para logon único, pesquise por sua instância do LinkedIn Sales Navigator usando o campo de pesquisa. Caso contrário, selecione **Adicionar** e pesquise por **LinkedIn Sales Navigator** na galeria de aplicativos. Selecione o LinkedIn Sales Navigator nos resultados da pesquisa e adicione-o à lista de aplicativos.

9. Selecione sua instância do LinkedIn Sales Navigator e selecione a guia **Provisionamento**.

10. Defina o **Modo de Provisionamento** como **Automático**.

    ![Provisionamento do LinkedIn Sales Navigator](./media/linkedinsalesnavigator-provisioning-tutorial/linkedin_3.PNG)

11. Preencha os campos a seguir em **Credenciais de Administrador**:

    * No campo de **URL do locatário**, digite https://api.linkedin.com.

    * No campo **Segredo do Token**, insira o token de acesso gerado na etapa 1 e clique em **Testar Conexão**.

    * Você verá uma notificação de êxito no lado do superior direito do seu Portal.

12. Insira o endereço de email de uma pessoa ou grupo que deve receber notificações de erro de provisionamento no campo **Email de Notificação** e marque a caixa de seleção abaixo.

13. Clique em **Save** (Salvar).

14. Na seção **Mapeamentos de Atributo**, examine os atributos de usuário e grupo que serão sincronizados do Azure AD para o LinkedIn Sales Navigator. Observe que os atributos selecionados como propriedades **Correspondentes** serão usados para corresponder as contas de usuário e grupos no LinkedIn Sales Navigator para operações de atualização. Selecione o botão Salvar para confirmar as alterações.

    ![Provisionamento do LinkedIn Sales Navigator](./media/linkedinsalesnavigator-provisioning-tutorial/linkedin_4.PNG)

15. Para habilitar o serviço de provisionamento do Azure AD para o LinkedIn Sales Navigator, altere o **Status de Provisionamento** para **Ativado** na seção **Configurações**

16. Clique em **Save** (Salvar).

Isso iniciará a sincronização inicial de todos os usuários e/ou grupos atribuídos ao LinkedIn Sales Navigator na seção Usuários e Grupos. Observe que a sincronização inicial levará mais tempo do que as sincronizações subsequentes, que ocorrem aproximadamente a cada 40 minutos, desde que o serviço esteja em execução. Use a seção **Detalhes de Sincronização** para monitorar o progresso e siga os links para os logs de atividade de provisionamento, que descrevem todas as ações executadas pelo serviço de provisionamento em seu aplicativo LinkedIn Sales Navigator.

Para saber mais sobre como ler os logs de provisionamento do Azure AD, consulte [Relatórios sobre o provisionamento automático de contas de usuário](../app-provisioning/check-status-user-account-provisioning.md).

## <a name="additional-resources"></a>Recursos adicionais

* [Gerenciamento do provisionamento de conta de usuário para Aplicativos Empresariais](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [O que é o acesso a aplicativos e logon único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

---
title: Atualizar uma oferta existente do Marketplace comercial
description: Como fazer atualizações em uma oferta existente do Marketplace comercial, incluindo edição, exclusão de um rascunho, cancelamento de uma solicitação de publicação, parar de vender uma oferta ou um plano e sincronizar audiências privadas.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: how-to
author: keferna
ms.author: keferna
ms.date: 01/16/2020
ms.openlocfilehash: f83f5da03d2db5354b020ce7d0c3c8d70f1830a0
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/09/2020
ms.locfileid: "89650100"
---
# <a name="update-an-existing-offer-in-the-commercial-marketplace"></a>Atualizar uma oferta existente no Marketplace comercial

Você pode exibir suas ofertas existentes na guia **visão geral** do [portal do Marketplace comercial](https://partner.microsoft.com/dashboard/commercial-marketplace/offers) no Partner Center.

Para atualizar uma oferta existente que está atualmente ativa no mercado comercial:

1. Selecione o nome da oferta que você deseja atualizar. O status da oferta pode ser listado como **Visualização**, **dinâmico**, **publicação em andamento**, **rascunho**, **atenção necessária**ou **não disponível** (se você optou anteriormente por parar de vender a oferta). Depois de selecionada, a página **visão geral da oferta** para essa oferta será aberta.
2. Selecione **Atualizar** no cartão na página Visão geral da oferta ou no item de menu no painel de navegação esquerdo para a área que você deseja atualizar. Talvez você queira atualizar a **configuração da oferta**, **Propriedades**, **lista de ofertas**, **Visualização**, **configuração técnica**, **visão geral do plano**ou **Test Drive**.
3. Faça suas alterações e selecione **salvar rascunho**. Repita esse processo até que todas as suas alterações sejam concluídas.

## <a name="review-and-publish-an-updated-offer"></a>Examinar e publicar uma oferta atualizada

Quando estiver pronto para publicar sua oferta atualizada, selecione **revisar e publicar** em qualquer página. A página **revisar e publicar** será aberta. Nesta página, você pode:

- Consulte o status de conclusão das seções da oferta que você atualizou: 
    - **Alterações não publicadas**: a seção foi atualizada e está concluída. Todos os dados necessários foram fornecidos e não houve erros introduzidos nas atualizações.
    - **Incompleto**: as atualizações feitas na seção introduziram erros que precisam ser corrigidos ou exigem mais informações a serem fornecidas.
- Forneça informações adicionais à equipe de testes de certificação para garantir que os testes sejam tranqüilamente.
- Envie a oferta atualizada para publicação selecionando **publicar**.  Enviaremos um email para você quando uma versão de visualização da oferta atualizada estiver disponível para revisão e aprovação.

> [!IMPORTANT]
> Você deve revisar a versão prévia da oferta quando ela estiver disponível e selecionar **Go-Live** para publicar sua oferta atualizada em seu público-alvo (público ou privado).

## <a name="add-a-plan-to-an-existing-offer"></a>Adicionar um plano a uma oferta existente

Para adicionar um novo plano em uma oferta existente que você já publicou:

1. Com a página **visão geral da oferta** para sua oferta existente aberta, vá para a página **visão geral do plano** e, em seguida, selecione **criar novo plano**.
1. Crie um novo plano de acordo com as [diretrizes](./create-new-saas-offer.md#plan-overview) usando o **modelo de preços de planos existentes**.
1. Selecione **salvar rascunho** depois de alterar o nome do plano.
1. Selecione **publicar** quando estiver pronto para publicar suas atualizações. A página **[revisar e publicar](#review-and-publish-an-updated-offer)** é aberta e fornece um status de conclusão para suas atualizações.

## <a name="update-a-plan-within-an-existing-offer"></a>Atualizar um plano em uma oferta existente

Para fazer alterações em um plano dentro de uma oferta existente que você já publicou:

1. Com a página **visão geral da oferta** para sua oferta existente aberta, escolha o plano que você deseja alterar. Se o plano não estiver acessível na lista **visão geral do plano** , selecione **Ver todos os planos**.
1. Selecione o **nome**do plano, o **modelo de preços**ou a **disponibilidade**. *Atualmente, os planos estão disponíveis apenas em inglês (Estados Unidos)*.
1. Selecione **salvar rascunho** depois de fazer alterações no nome do plano, na descrição ou na disponibilidade do público.
1. Selecione **revisar e publicar** quando estiver pronto para publicar suas atualizações. A página **[revisar e publicar](#review-and-publish-an-updated-offer)** é aberta e fornece um status de conclusão para suas atualizações.
1. Envie o plano atualizado para publicação selecionando **publicar**. Enviaremos um email para você quando uma versão de visualização da oferta atualizada estiver disponível para revisão e aprovação.

## <a name="offer-a-virtual-machine-plan-at-a-new-price"></a>Oferecer um plano de máquina virtual a um novo preço

Depois que um plano de máquina virtual é publicado, seu preço não pode ser alterado. Para oferecer o mesmo plano a um preço diferente, você deve ocultar o plano e criar um novo com o preço atualizado. Primeiro, oculte o plano com o preço que você deseja alterar:

1. Com a página **visão geral da oferta** para sua oferta existente aberta, escolha o plano que você deseja alterar. Se o plano não estiver acessível na lista **visão geral do plano** , selecione **Ver todos os planos**.
1. Marque a caixa de seleção **ocultar plano** . Salve o rascunho antes de continuar.

Agora que você ocultou o plano com o preço antigo, crie uma cópia desse plano com o preço atualizado:

1. No Partner Center, volte para o **plano visão geral**.
2. Selecione **Criar novo plano**. Insira uma **ID de plano** e um **nome de plano**e, em seguida, selecione **criar**.
1. Para reutilizar a configuração técnica do plano que você ocultou, marque a caixa de seleção **reutilizar a configuração técnica** . Leia [visão geral do plano](azure-vm-create-offer.md#plan-overview) para saber mais.
    > [!IMPORTANT]
    > Se você selecionar **este plano reutiliza a configuração técnica de outro plano**, não será possível parar de vender o plano pai posteriormente. Não use essa opção se desejar parar de vender o plano pai.
3. Conclua todas as seções necessárias para o novo plano, incluindo o novo preço.
1. Selecione **Salvar rascunho**.
1. Depois de concluir todas as seções necessárias para o novo plano, selecione **revisar e publicar**. Isso enviará sua oferta para revisão e publicação. Leia [revisar e publicar uma oferta no Marketplace comercial](../review-publish-offer.md) para obter mais detalhes.

## <a name="compare-changes-to-commercial-marketplace-offers"></a>Comparar alterações a ofertas do Marketplace comercial

Você pode auditar as alterações feitas em uma oferta [publicada](#compare-changes-to-published-offer) ou de [Visualização](#compare-changes-to-a-preview-offer) antes de torná-las ao vivo usando **comparar**.

> [!NOTE]
> Uma oferta publicada é uma oferta que foi publicada com êxito no estado de visualização ou ao vivo.

Veja abaixo as informações gerais de auditoria:

- Você pode usar **comparar** a qualquer momento durante o processo de edição.
- Selecione um campo na página **comparar** para navegar até o valor que você deseja modificar.
- Se você estiver pronto para publicar suas atualizações, selecione **revisar e publicar**.
- Para ver os valores de todos os campos, até mesmo os campos não atualizados, selecione o filtro **todos os campos** . Você pode modificar filtros dentro desses campos selecionando **campos modificados**e, em seguida, selecionando um dos filtros abaixo:
    - O filtro de **valores removidos exibe os** campos que você publicou e agora está completamente removendo.
    - O filtro **valores adicionados exibe os** campos que você não publicou originalmente e que agora estão sendo adicionados.
    - O filtro de **valores editados** exibe os campos que foram publicados, mas agora você atualizou o conteúdo.

      >[!NOTE]
      > Se um desses filtros não estiver disponível, isso indica que você não fez uma atualização desse tipo.

- Para ver apenas os valores que não foram atualizados, selecione o filtro **campos inalterados** . Os valores de campo mostrados para a versão publicada e de rascunho serão os mesmos.

  ![Filtros para comparar atualizações à sua oferta publicada ou de visualização](./media/compare-changes-marketplace.png)

>[!NOTE]
> Atualmente, as páginas a seguir não dão suporte a **comparação**:
>- Público do revendedor CSP
>- Testar a configuração técnica
>- Lista do test drive do Marketplace
>- Venda conjunta
>- Arquivos complementares

### <a name="compare-changes-to-published-offer"></a>Comparar alterações à oferta publicada

Siga as instruções abaixo para comparar as alterações da oferta publicada:

1. Selecione **comparar** na barra de comandos da página. A página **comparar** fornece versões lado a lado das alterações salvas dessa oferta e da oferta do Marketplace publicada.
2. Quando você acessa **comparar** de uma página específica da oferta, o padrão exibe apenas as alterações feitas nessa página.
    - Se você quiser comparar as alterações em todas as páginas, altere a página de **selecionar uma página para comparar**.  
    - Se você quiser comparar as alterações à oferta em todas as páginas, selecione **todas as páginas**.

Como padrão, **Compare** contrasta suas novas alterações para a oferta do Live Marketplace.

Lembre-se de republicar sua oferta depois de fazer as atualizações para que as alterações entrem em vigor.

### <a name="compare-changes-to-a-preview-offer"></a>Comparar alterações a uma oferta de visualização

Se você tiver alterações na visualização que não estão dinâmicas, você poderá comparar novas alterações com a oferta de versão prévia do Marketplace.

Siga as instruções abaixo para comparar as novas alterações com sua oferta do Marketplace de visualização:

1. Selecione **comparar** na barra de comandos da página.
2. Selecione a lista suspensa **com** e altere-a da **oferta dinâmica** para a versão **prévia**.
3. A página **comparar** fornece versões lado a lado que mostram as alterações.

>[!NOTE]
>Se sua oferta ainda não tiver sido ativada, mas você tiver publicado para a versão prévia, você não tem a opção de comparar com uma oferta dinâmica.

Lembre-se de republicar sua oferta depois de fazer as atualizações para que as alterações entrem em vigor.

## <a name="delete-a-draft-offer"></a>Excluir uma oferta de rascunho

Você pode excluir uma oferta de rascunho (aquela que não foi publicada) selecionando **excluir rascunho** na página **visão geral da oferta** . Esta opção não estará disponível para você se você já tiver publicado a oferta.

Depois de confirmar que deseja excluir a oferta de rascunho, a oferta não estará mais visível ou acessível no Partner Center e a página **todas as ofertas** será aberta.

## <a name="delete-a-draft-plan"></a>Excluir um plano de rascunho

Para excluir um plano que não foi publicado, selecione **excluir rascunho** na página **visão geral do plano** . Essa opção não estará disponível se você tiver publicado a oferta anteriormente.

Depois de confirmar que deseja excluir o plano de rascunho, o plano não estará mais visível ou acessível no Partner Center.

## <a name="cancel-publishing"></a>Cancelar publicação

Para cancelar uma oferta com o status **publicação em andamento** :

1. Selecione o nome da oferta para abrir a página **visão geral da oferta** .
1. Selecione **Cancelar publicação** no canto superior direito da página.
1. Confirme que você deseja impedir que a oferta seja publicada.

Se você quiser publicar a oferta em um momento posterior, será necessário iniciar o processo de publicação.

> [!NOTE]
> Você pode interromper a publicação de uma oferta somente se a oferta ainda não tiver progredido para a etapa de aprovação do Publicador. Depois de selecionar o **Go Live** , você não terá a opção de cancelar a publicação mais.

## <a name="stop-selling-an-offer-or-plan"></a>Pare de vender uma oferta ou um plano

Por vários motivos, você pode decidir remover sua listagem de oferta do Microsoft Commercial Marketplace. A remoção da oferta garante que novos clientes não possam mais comprar ou implantar sua oferta, mas não têm impacto sobre os clientes existentes.

Para parar de vender uma oferta depois de publicá-la, selecione **parar de vender** na página **visão geral da oferta** .

Depois de confirmar que deseja parar de vender a oferta, em algumas horas ela não estará mais visível no mercado comercial e nenhum novo cliente poderá baixá-la.

Para parar de vender um plano, selecione **parar de vender** na página **visão geral do plano** . A opção de parar de vender um plano só estará disponível se você tiver mais de um plano na oferta. Você pode optar por parar de vender um plano sem afetar outros planos dentro de sua oferta. Depois de confirmar que deseja parar de vender o plano, você deve republicar a oferta para que a alteração entre em vigor. Depois que a oferta for republicada, o plano não estará mais visível no mercado comercial e nenhum novo cliente poderá baixá-lo.

Todos os clientes que já adquiriram a oferta ou o plano ainda podem usá-lo. Eles podem baixá-lo novamente, mas eles não receberão atualizações se você atualizar e publicar novamente a oferta ou o plano mais tarde.

Depois que sua solicitação para parar de vender a oferta/plano tiver sido concluída, você ainda a verá no portal do Marketplace comercial no Partner Center com um status **indisponível** .

Se você decidir listar ou vender esta oferta ou planejar novamente, siga as instruções para [atualizar uma oferta existente](#update-an-existing-offer-in-the-commercial-marketplace). Não se esqueça de que será necessário **publicar** a oferta ou o plano novamente após fazer as alterações.

## <a name="remove-offers-from-existing-customers"></a>Remover ofertas de clientes existentes

Para remover ofertas de clientes existentes, [registre uma solicitação de suporte](https://aka.ms/marketplacepublishersupport). Na lista tópico de suporte, selecione oferta de **Marketplace comercial**  >  **ou deslistação de aplicativos, remoção ou rescisão** e envie a solicitação. A equipe de suporte orientará você pelo processo de remoção da oferta.

## <a name="sync-private-plan-audiences"></a>Sincronizar audiências do plano privado

Se sua oferta incluir um ou mais planos configurados para serem disponibilizados somente para um público restrito privado, será possível atualizar somente o público que pode acessar esse plano privado sem publicar outras alterações na oferta. 

Para atualizar e sincronizar o público privado para seus planos:

- Modifique o público em um ou mais planos privados usando o botão + **Adicionar ID** ou **importar clientes (CSV)** e, em seguida, salve as alterações.
- Selecione **sincronizar público privado** na página **visão geral do plano** .

**Sincronizar público privado** publicará somente as alterações em seus públicos privados, sem publicar outras atualizações que você tenha feito na oferta de rascunho.

## <a name="next-steps"></a>Próximas etapas

- [Verificar o status de publicação da sua oferta do Marketplace comercial](./publishing-status.md)

---
title: O que são Reservas do Azure?
description: Saiba mais sobre as Reservas do Azure e sobre os preços para economizar nos custos de máquinas virtuais, de bancos de dados SQL, do Azure Cosmos DB e de outros recursos.
author: yashesvi
ms.reviewer: yashar
ms.service: cost-management-billing
ms.topic: conceptual
ms.date: 02/18/2020
ms.author: banders
ms.openlocfilehash: c6a8547235c302f52aacd0e6ae4a8fbf08b538b8
ms.sourcegitcommit: 6e87ddc3cc961945c2269b4c0c6edd39ea6a5414
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/18/2020
ms.locfileid: "77443628"
---
# <a name="what-are-azure-reservations"></a>O que são Reservas do Azure?

As Reservas do Azure ajudam você a economizar dinheiro ao se comprometer com um plano de um ano ou de três anos para uso de máquinas virtuais, do Armazenamento de Blobs do Azure ou do Azure Data Lake Storage Gen2, da capacidade de computação do Banco de Dados SQL, do Armazenamento em Disco do Azure, da taxa de transferência do Azure Cosmos DB ou de outros recursos do Azure. O compromisso permite que você obtenha um desconto nos recursos que usar. As reservas podem reduzir significativamente os custos de recursos em até 72% nos preços pagos conforme o uso. As reservas fornecem um desconto de cobrança e não afetam o estado de runtime dos recursos.

Você pode pagar por uma reserva de forma antecipada ou mensal. O custo total das reservas antecipadas e mensais é o mesmo e você não paga nenhuma taxa adicional quando opta por pagar mensalmente. O pagamento mensal está disponível para reservas do Azure, e não para produtos de terceiros.

Você pode comprar uma instância reservada no [portal do Azure](https://ms.portal.azure.com/?microsoft_azure_marketplace_ItemHideKey=Reservations&Microsoft_Azure_Reservations=true#blade/Microsoft_Azure_Reservations/ReservationsBrowseBlade).

## <a name="why-buy-a-reservation"></a>Por que comprar uma reserva?

Se você tem máquinas virtuais, dados de armazenamento de blobs, o Azure Cosmos DB ou bancos de dados SQL que usem uma capacidade ou taxa de transferência significativas ou que sejam executados por períodos longos, a compra de uma reserva oferece a opção mais econômica. Por exemplo, quando executa continuamente quatro instâncias de um serviço sem a reserva, você é cobrado com base em taxas pagas conforme o uso. Quando compra uma reserva para esses recursos, você obtém imediatamente o desconto de reserva. Os recursos não serão mais cobrados com base nas taxas pagas conforme o uso.

## <a name="charges-covered-by-reservation"></a>Preços cobertos pela reserva

Planos de serviço:

- **Instância de Máquina Virtual Reservada** – uma reserva apenas abrange os custos de computação de máquina virtual. Não cobre encargos adicionais de software, rede e armazenamento.
- **Capacidade reservada de Armazenamento do Azure**: uma reserva abrange a capacidade de contas de armazenamento padrão para armazenamento de blobs ou do Azure Data Lake Storage Gen2. A reserva não abrange a largura de banda nem as taxas de transação.
- **Reservas do Armazenamento em Disco do Azure** – uma reserva cobre apenas SSDs Premium de tamanho P30 ou superior. Ela não cobre nenhum outro tipo de disco ou tamanhos menores que P30.
- **Capacidade reservada do Azure Cosmos DB** – cobre a taxa de transferência provisionada para seus recursos. Ela não cobre encargos de armazenamento e rede.
- **vCore reservado do Banco de Dados SQL** – apenas os custos de computação são incluídos com uma reserva. A licença é cobrada separadamente.
- **SQL Data Warehouse** – uma reserva cobre o uso de cDWU. Ela não cobre encargos de armazenamento ou rede associados ao uso do SQL Data Warehouse.
- **Imposto de selo do Serviço de Aplicativo** – uma reserva cobre o uso do imposto de selo. Ela não se aplica a funções de trabalho, de forma que todos os outros recursos associados ao selo são cobrados separadamente.
- **Azure Databricks** – uma reserva abrange apenas o uso de DBU. Outros encargos, como computação, armazenamento e rede, são aplicados separadamente.
- **Banco de Dados do Azure para MySQL** – apenas os custos de computação são incluídos com uma reserva. Uma reserva não cobre custos de software, rede ou armazenamento associados à instância do servidor de Banco de Dados MySQL.
- **Banco de Dados do Azure para PostgreSQL** – apenas os custos de computação são incluídos com uma reserva. Uma reserva não cobre custos de software, rede ou armazenamento associados à instância dos servidores de Banco de Dados PostgreSQL.
- **Banco de Dados do Azure para MariaDB** – apenas os custos de computação são incluídos com uma reserva. Uma reserva não cobre custos de software, rede ou armazenamento associados à instância do servidor de banco de dados MariaDB.
- **Azure Data Explorer** – uma reserva abrange os encargos de marcação. Uma reserva não abrange os encargos de computação, rede ou armazenamento associados aos clusters.
- **Managed Disks SSD Premium** – uma reserva é feita para um SKU de disco especificado. 

Planos de software:

- **SUSE Linux** – uma reserva abrange os custos do plano de software. Os descontos aplicam-se apenas aos medidores do SUSE e não ao uso da máquina virtual.
- **Planos do Red Hat** – uma reserva abrange os custos do plano de software. Os descontos aplicam-se apenas aos medidores Red Hat e não ao uso da máquina virtual.
- **Solução VMware da CloudSimple no Azure** – uma reserva abrange os nós VMWare da CloudSimple. Os custos de software adicionais ainda se aplicam.
- **Red Hat OpenShift no Azure** – uma reserva se aplica aos custos do OpenShift, mas não aos custos de infraestrutura do Azure.

Para as máquinas virtuais do Windows e banco de Dados SQL, você pode cobrir os custos de licenciamento com o [Benefício Híbrido do Azure](https://azure.microsoft.com/pricing/hybrid-benefit/).

## <a name="whos-eligible-to-purchase-a-reservation"></a>Quem é elegível para comprar uma reserva?

Para comprar um plano, você deve ter uma função de proprietário de assinatura em uma assinatura Enterprise (MS-AZR-0017P ou MS-AZR-0148P) ou de Pagamento Conforme o Uso (MS-AZR-0003P ou MS-AZR-0023P) ou de Contrato de Cliente da Microsoft. Os provedores de solução na nuvem podem usar o portal do Azure ou o  [Partner Center](/partner-center/azure-reservations)  para comprar Reservas do Azure.

Clientes com EA (Contrato Enterprise) podem limitar as compras aos administradores de EA desabilitando a opção **Adicionar Instâncias Reservadas** no Portal do EA. Os administradores de EA precisam ser proprietários de pelo menos uma assinatura de EA para comprar uma reserva. A opção é útil para empresas que desejam que uma equipe centralizada compre reservas para diferentes centros de custo. Após a compra, as equipes centralizadas podem adicionar proprietários de centro de custo às reservas. Os proprietários podem, então, definir o escopo da reserva para suas assinaturas. A equipe central não precisa ter acesso de proprietário da assinatura no local em que a reserva é comprada.

Um desconto de reserva aplica-se somente aos recursos associados a assinaturas adquiridas por meio do Enterprise, do CSP (Provedor de Soluções de Nuvem), Contrato de Cliente da Microsoft e planos individuais com tarifas pagas conforme o uso.

## <a name="scope-reservations"></a>Escopo das reservas

Você pode definir o escopo de uma reserva como uma assinatura ou como grupos de recursos. Definir o escopo de uma reserva seleciona o local a que a economia da reserva se aplica. Quando você define o escopo da reserva como um grupo de recursos, os descontos da reserva aplicam-se somente ao grupo de recursos — e não à assinatura inteira.

### <a name="reservation-scoping-options"></a>Opções de escopo de reserva

Com o escopo do grupo de recursos, você tem três opções para definir o escopo de uma reserva, dependendo de suas necessidades:

- **Escopo de grupo de recursos único** — aplica o desconto de reserva apenas aos recursos correspondentes no grupo de recursos selecionado.
- **Escopo de assinatura única** — aplica o desconto de reserva apenas aos recursos correspondentes na assinatura selecionada.
- **Escopo compartilhado** — aplica o desconto de reserva aos recursos correspondentes em assinaturas qualificadas que estão no contexto de cobrança. Para clientes do Contrato Enterprise, o contexto de cobrança é o registro. Para clientes do Contrato de Cliente da Microsoft, o escopo do orçamento é o perfil de cobrança. Para assinaturas individuais com tarifas pagas conforme o uso, o escopo do orçamento são todas as assinaturas qualificadas criadas pelo administrador da conta.

Ao aplicar descontos de reserva ao seu uso, o Azure processa a reserva na seguinte ordem:

1. Reservas que estão no escopo de um grupo de recursos
2. Reservas de escopo único
3. Reservas de escopo compartilhado

Um único grupo de recursos pode obter descontos de reserva provenientes de várias reservas, dependendo de como você define o escopo de suas reservas.

### <a name="scope-a-reservation-to-a-resource-group"></a>Definir o escopo de uma reserva como um grupo de recursos

Você pode definir o escopo da reserva como um grupo de recursos quando adquire a reserva ou pode definir o escopo após a compra. Você precisa ser um proprietário da assinatura para definir o escopo de uma reserva como um grupo de recursos.

Para definir o escopo, vá até a página [Comprar reserva](https://ms.portal.azure.com/#blade/Microsoft\_Azure\_Reservations/CreateBlade/referrer/Browse\_AddCommand) no portal do Azure. Selecione o tipo de reserva que deseja comprar. No formulário de seleção **Selecione o produto que deseja comprar**, altere o valor do escopo para Grupo de recursos único. Em seguida, selecione um grupo de recursos.

![Exemplo mostrando a seleção de compra de reserva de VM](./media/save-compute-costs-reservations/select-product-to-purchase.png)

São mostradas recomendações de compra para o grupo de recursos na reserva da máquina virtual. As recomendações são calculadas analisando seu uso nos últimos 30 dias. Uma recomendação de compra é feita quando o custo da execução dos recursos com instâncias reservadas é mais barato do que o custo da execução dos recursos com tarifas pagas conforme o uso. Para saber mais sobre as recomendações de compra de reserva, confira [Obter recomendações de compra de instância reservada com base no padrão de uso](https://azure.microsoft.com/blog/get-usage-based-reserved-instance-recommendations).

Você sempre pode atualizar o escopo depois de comprar uma reserva. Para fazer isso, vá para a reserva, clique em **Configuração** e redefina o escopo da reserva. A redefinição do escopo de uma reserva não é uma transação comercial. O termo da reserva não é alterado. Para saber mais sobre como atualizar o escopo, confira [Atualizar o escopo depois de comprar uma reserva](manage-reserved-vm-instance.md#change-the-reservation-scope).

![Exemplo mostrando uma alteração de escopo de reserva](./media/save-compute-costs-reservations/rescope-reservation-resource-group.png)

### <a name="monitor-and-optimize-reservation-usage"></a>Monitorar e otimizar o uso da reserva

Você pode monitorar o uso da reserva de várias maneiras – no portal do Azure, usando APIs ou por meio dos dados de uso. Para ver todas as reservas a que tem acesso, vá até **Reservas** no portal do Azure. A grade de reservas mostra o percentual de utilização gravado pela última vez para a reserva. Clique na reserva para ver sua utilização de longo prazo.

Você também poderá obter utilização de reserva usando [APIs](reservation-apis.md#see-reservation-usage) e dos seus [dados de uso](understand-reserved-instance-usage-ea.md#common-cost-and-usage-tasks) se for um contrato Enterprise ou um cliente do Contrato de Cliente da Microsoft.

Se observar que a utilização da reserva com escopo do grupo de recursos está baixa, você poderá atualizar o escopo da reserva para uma assinatura única ou compartilhá-la no contexto da cobrança. Você também pode dividir a reserva e aplicar as reservas resultantes a grupos de recursos diferentes.

### <a name="other-considerations"></a>Outras considerações

Se você não tiver recursos correspondentes em um grupo de recursos, a reserva será subutilizada. A reserva não se aplica automaticamente a um grupo de recursos ou assinatura diferente em que há pouca utilização.

O escopo de uma reserva não será atualizado automaticamente se você mover o grupo de recursos de uma assinatura para outra. O escopo não será atualizado se você excluir o grupo de recursos. Você precisará [redefinir o escopo da reserva](manage-reserved-vm-instance.md#change-the-reservation-scope). Caso contrário, a reserva será subutilizada.

## <a name="discounted-subscription-and-offer-types"></a>Assinatura com desconto e tipos de oferta

Os descontos na reserva aplicam-se às seguintes assinaturas e tipos de oferta elegíveis.

- Enterprise Agreement (números da oferta: MS-AZR-0017P ou MS-AZR-0148P)
- Assinaturas do Contrato de Cliente da Microsoft.
- Planos individuais com tarifas de pagamento conforme o uso (números da oferta: MS-AZR-0003P ou MS-AZR-0023P)
- Assinaturas de CSP

Os recursos executados em uma assinatura com outros tipos de oferta não recebem o desconto de reserva.

## <a name="how-is-a-reservation-billed"></a>Como uma reserva é cobrada?

A reserva é cobrada no método de pagamento associado à assinatura. O custo de reserva é deduzido do saldo do seu compromisso monetário, se disponível. Quando o saldo do compromisso monetário não cobrir o custo da reserva, você será cobrado pelo excedente. Se você tiver uma assinatura de um plano individual com tarifas pagas conforme o uso, o cartão de crédito que você tem em sua conta será cobrado imediatamente para as compras antecipadas. Os pagamentos mensais aparecem em sua fatura e seu cartão de crédito é cobrado mensalmente. Quando for cobrado por fatura, você verá os encargos na sua próxima fatura.

## <a name="how-reservation-discount-is-applied"></a>Como o desconto de reserva é aplicado

O desconto de reserva se aplica para o uso de recursos que correspondem aos atributos que você seleciona ao comprar a reserva. Os atributos incluem o escopo em que as VMs, os banco de dados SQL, o Azure Cosmos DB ou outros recursos correspondentes são executados. Por exemplo, caso deseje obter um desconto de reserva para quatro máquinas virtuais Standard D2 na região Oeste dos EUA, selecione a assinatura na qual as VMs estão em execução.

Um desconto de reserva é do tipo "*usar ou perder*". Se você não tiver recursos correspondentes para nenhuma hora, perderá uma quantidade de reserva para essa hora. Não é possível postergar horas reservadas não utilizadas.

Quando você desliga um recurso, o desconto de reserva se aplica automaticamente a outro recurso correspondente no escopo especificado. Se nenhum recurso correspondente for encontrado no escopo especificado, as horas reservadas serão *perdidas*.

Por exemplo, você pode criar um recurso posteriormente e ter uma reserva correspondente que fica subutilizada. O desconto da reserva se aplica automaticamente ao novo recurso de correspondente.

Se as máquinas virtuais são executadas em diferentes assinaturas em sua conta ou registro, então, selecione o escopo como compartilhado. O escopo compartilhado permite que o desconto de reserva seja aplicado entre assinaturas. Você pode alterar o escopo depois de comprar uma reserva. Para obter mais informações, consulte [Gerenciar Reservas do Azure](manage-reserved-vm-instance.md).

Um desconto de reserva aplica-se apenas aos recursos associados aos tipos de assinatura Enterprise, Contrato de Cliente da Microsoft, CSP ou assinaturas com tarifas pagas conforme o uso. Os recursos executados em uma assinatura com outros tipos de oferta não recebem o desconto de reserva.

## <a name="when-the-reservation-term-expires"></a>Quando o prazo da reserva expira

No final do período de reserva, o desconto de cobrança expira e os recursos são cobrados pelo preço de pré-pagamento. Por padrão, as reservas não são definidas para renovação automática. Você pode optar por habilitar a renovação automática de uma reserva selecionando a opção nas configurações de renovação. Com a renovação automática, uma reserva substituta será adquirida após a expiração da reserva existente. Por padrão, a reserva substituta tem os mesmos atributos que a reserva que está vencendo. Opcionalmente, você pode alterar a frequência de cobrança, o prazo ou a quantidade nas configurações de renovação. Qualquer usuário com acesso de proprietário na reserva e na assinatura usada para cobrança pode configurar a renovação.  

## <a name="discount-applies-to-different-sizes"></a>O desconto se aplica a diferentes tamanhos

Quando você compra uma reserva, o desconto pode ser aplicado a outras instâncias com atributos que estão dentro do mesmo grupo de tamanho. Esse recurso é conhecido como flexibilidade de tamanho da instância. A flexibilidade da cobertura de desconto depende do tipo de reserva e dos atributos que você escolhe quando compra a reserva.

Planos de serviço:

- Instâncias de VM reservadas: Ao comprar a reserva, se você selecionar **Otimizado para flexibilidade de tamanho da instância**, a cobertura de desconto dependerá do tamanho da VM que você selecionar. A reserva pode ser aplicada aos tamanhos de VMs (máquinas virtuais) no mesmo grupo de série de tamanho. Para obter mais informações, confira [Flexibilidade de tamanho de máquina virtual com Instâncias de VM Reservadas](../../virtual-machines/windows/reserved-vm-instance-size-flexibility.md).
- Capacidade reservada do Armazenamento do Azure: você pode comprar capacidade reservada para contas de armazenamento padrão do Azure em unidades de 100 TiB ou 1 PiB por mês. A capacidade reservada do armazenamento do Azure está disponível em todas as regiões para qualquer camada de acesso (quente, fria ou arquivo) e para qualquer opção de replicação (LRS, GRS ou ZRS).
- Capacidade reservada do Banco de Dados SQL: A cobertura de desconto depende do nível de desempenho escolhido. Para obter mais informações, confira [Entender como um desconto de reserva do Azure é aplicado](understand-reservation-charges.md).
- Capacidade reservada do Azure Cosmos DB: A cobertura de desconto depende da taxa de transferência provisionada. Para obter mais informações, confira [Entender como um desconto de reserva do Azure Cosmos DB é aplicado](understand-cosmosdb-reservation-charges.md).

## <a name="reservation-notifications"></a>Notificações de reserva

Dependendo de como você paga por sua assinatura do Azure, enviamos emails com notificações de reserva aos seguintes usuários de sua organização. São enviadas notificações referentes a vários eventos, incluindo:

- Purchase
- Futura expiração da reserva
- Expiry
- Renovação
- Cancelamento
- Alteração de escopo

Para clientes com assinaturas EA:
- Uma notificação de compra é enviada ao comprador e aos contatos de notificação do EA.
- Outras notificações do ciclo de vida da reserva são enviadas somente para os contatos de notificação do EA.
- Usuários adicionados a uma reserva usando a permissão de RBAC (IAM) não recebem nenhuma notificação por email.

Para clientes com assinaturas individuais:
- O comprador recebe uma notificação de compra.
- No momento da compra, o proprietário da conta de cobrança da assinatura recebe uma notificação de compra.
- O proprietário da conta recebe todas as outras notificações.


## <a name="need-help-contact-us"></a>Precisa de ajuda? Entre em contato conosco.

Caso tenha dúvidas ou precise de ajuda, [crie uma solicitação de suporte](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Próximas etapas

- Saiba mais sobre as Reservas do Azure com os seguintes artigos:
    - [Gerenciar Reservas do Azure](manage-reserved-vm-instance.md)
    - [Noções básicas sobre o uso de reserva para sua assinatura com taxas pagas conforme o uso](understand-reserved-instance-usage.md)
    - [Entender o uso de reserva para seu registro de empresa](understand-reserved-instance-usage-ea.md)
    - [Custos de software do Windows não estão incluídos nas reservas](reserved-instance-windows-software-costs.md)
    - [Reservas do Azure no programa de CSP (Provedor de Soluções na Nuvem) do Partner Center](/partner-center/azure-reservations)

- Saiba mais sobre reservas para planos de serviço:
    - [Máquinas virtuais com Instâncias de VM Reservadas do Azure](../../virtual-machines/windows/prepay-reserved-vm-instances.md)
    - [Recursos do Azure Cosmos DB com capacidade reservada do Azure Cosmos DB](../../cosmos-db/cosmos-db-reserved-capacity.md)
    - [Recursos de computação de Banco de Dados SQL do Azure com capacidade reservada](../../sql-database/sql-database-reserved-capacity.md) Saiba mais sobre reservas para planos de software:
    - [Planos de software Red Hat das Reservas do Azure](../../virtual-machines/linux/prepay-rhel-software-charges.md)
    - [Planos de software SUSE das Reservas do Azure](../../virtual-machines/linux/prepay-suse-software-charges.md)

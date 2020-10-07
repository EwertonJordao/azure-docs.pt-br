---
title: 'Início Rápido: Criar um ponto de extremidade privado do Azure usando a CLI do Azure'
description: Saiba mais sobre o ponto de extremidade privado do Azure neste Início Rápido
services: private-link
author: malopMSFT
ms.service: private-link
ms.topic: quickstart
ms.date: 09/16/2019
ms.author: allensu
ms.custom: devx-track-azurecli
ms.openlocfilehash: e7c098ba06086781306960f76978aac9e4fa06bc
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/05/2020
ms.locfileid: "87502657"
---
# <a name="quickstart-create-a-private-endpoint-using-azure-cli"></a>Início Rápido: Criar um ponto de extremidade privado usando a CLI do Azure

O Ponto de Extremidade Privado é o bloco de construção fundamental para o Link Privado no Azure. Ele permite que os recursos do Azure, como VMs (máquinas virtuais), se comuniquem de forma privada com os Recursos de Link Privado. Neste Início Rápido, você aprenderá como criar uma VM em uma rede virtual, um servidor no Banco de Dados SQL com um Ponto de Extremidade Privado usando a CLI do Azure. Em seguida, você poderá acessar a VM e acessar com segurança o recurso de link privado (um servidor privado no Banco de Dados SQL neste exemplo).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Se você optar por instalar e usar a CLI do Azure localmente, este guia de início rápido exigirá a versão 2.0.28 ou posterior da CLI do Azure. Execute `az --version` para localizar a versão instalada. Para informações sobre como instalar ou atualizar, confira [Instalar a CLI do Azure](/cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Criar um grupo de recursos

Antes de criar qualquer recurso, você deve criar um grupo de recursos para hospedar a Rede Virtual. Crie um grupo de recursos com [az group create](/cli/azure/group). Este exemplo cria um grupo de recursos chamado *myResourceGroup* na localização *westcentralus*:

```azurecli-interactive
az group create --name myResourceGroup --location westcentralus
```

## <a name="create-a-virtual-network"></a>Criar uma rede virtual

Crie uma Rede Virtual com [az network vnet create](/cli/azure/network/vnet). O exemplo cria uma Rede Virtual padrão nomeada *myVirtualNetwork* com uma sub-rede nomeada *mySubnet*:

```azurecli-interactive
az network vnet create \
 --name myVirtualNetwork \
 --resource-group myResourceGroup \
 --subnet-name mySubnet
```

## <a name="disable-subnet-private-endpoint-policies"></a>Desabilitar políticas de ponto de extremidade privado de sub-rede

O Azure implanta recursos em uma sub-rede dentro de uma rede virtual. Portanto, você precisa criar ou atualizar a sub-rede para desativar as políticas de rede de ponto de extremidade privado. Atualize uma configuração de sub-rede denominada *mySubnet* com [az network vnet subnet update](https://docs.microsoft.com/cli/azure/network/vnet/subnet?view=azure-cli-latest#az-network-vnet-subnet-update):

```azurecli-interactive
az network vnet subnet update \
 --name mySubnet \
 --resource-group myResourceGroup \
 --vnet-name myVirtualNetwork \
 --disable-private-endpoint-network-policies true
```

## <a name="create-the-vm"></a>Criar a VM

Crie uma VM com az vm create. Quando solicitado, forneça uma senha a ser usada como credencial de entrada para a VM. Este exemplo cria uma VM chamada *myVm*:

```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVm \
  --image Win2019Datacenter
```

Anote o endereço IP da VM. Você usará esse endereço para conectar-se à VM pela Internet na próxima etapa.

## <a name="create-a-server-in-sql-database"></a>Criar um servidor no Banco de Dados SQL

Crie um servidor no Banco de Dados SQL com o comando az sql server create. Lembre-se de que o nome do seu servidor deve ser exclusivo no Azure, portanto substitua o valor do espaço reservado entre colchetes pelo seu próprio valor exclusivo:

```azurecli-interactive
# Create a server in the resource group
az sql server create \
    --name "myserver"\
    --resource-group myResourceGroup \
    --location WestUS \
    --admin-user "sqladmin" \
    --admin-password "CHANGE_PASSWORD_1"

# Create a database in the server with zone redundancy as false
az sql db create \
    --resource-group myResourceGroup  \
    --server myserver \
    --name mySampleDatabase \
    --sample-name AdventureWorksLT \
    --edition GeneralPurpose \
    --family Gen4 \
    --capacity 1
```

A ID do servidor é semelhante a ```/subscriptions/subscriptionId/resourceGroups/myResourceGroup/providers/Microsoft.Sql/servers/myserver.```. Você usará a ID do servidor na próxima etapa.

## <a name="create-the-private-endpoint"></a>Criar um Ponto de Extremidade Privado

Crie um ponto de extremidade privado para o SQL Server lógico em sua Rede Virtual:

```azurecli-interactive
az network private-endpoint create \  
    --name myPrivateEndpoint \  
    --resource-group myResourceGroup \  
    --vnet-name myVirtualNetwork  \  
    --subnet mySubnet \  
    --private-connection-resource-id "<server ID>" \  
    --group-ids sqlServer \  
    --connection-name myConnection  
 ```

## <a name="configure-the-private-dns-zone"></a>Configurar a Zona DNS Privada

Crie uma Zona DNS Privada para o domínio do Banco de Dados SQL, crie um link de associação com a Rede Virtual e crie um Grupo de Zona DNS para associar o ponto de extremidade privado à Zona DNS Privada. 

```azurecli-interactive
az network private-dns zone create --resource-group myResourceGroup \
   --name  "privatelink.database.windows.net"
az network private-dns link vnet create --resource-group myResourceGroup \
   --zone-name  "privatelink.database.windows.net"\
   --name MyDNSLink \
   --virtual-network myVirtualNetwork \
   --registration-enabled false
az network private-endpoint dns-zone-group create \
   --resource-group myResourceGroup \
   --endpoint-name myPrivateEndpoint \
   --name MyZoneGroup \
   --private-dns-zone "privatelink.database.windows.net" \
   --zone-name sql
```

## <a name="connect-to-a-vm-from-the-internet"></a>Conecte uma VM a partir da Internet

Conecte-se à VM *myVm* da Internet da seguinte forma:

1. Na barra de pesquisa do portal, insira *myVm*.

1. Selecione o botão **Conectar**. Depois de selecionar o botão **Conectar**, **Conectar-se à máquina virtual** abre.

1. Selecione **Baixar Arquivo RDP**. O Azure cria um arquivo *.rdp* (protocolo RDP) e ele é baixado no computador.

1. Abra o arquivo downloaded.rdp*.

    1. Se solicitado, selecione **Conectar**.

    1. Insira o nome de usuário e a senha que você especificou ao criar a VM.

        > [!NOTE]
        > Talvez seja necessário selecionar **Mais escolhas** > **Usar uma conta diferente** para especificar as credenciais inseridas durante a criação da VM.

1. Selecione **OK**.

1. Você pode receber um aviso do certificado durante o processo de logon. Se você receber um aviso de certificado, selecione **Sim** ou **Continuar**.

1. Depois que a área de trabalho da VM for exibida, minimize-a para voltar para sua área de trabalho local.  

## <a name="access-sql-database-privately-from-the-vm"></a>Acessar o Banco de Dados SQL no modo privado da VM

Nesta seção, você se conectará ao Banco de Dados SQL da VM usando o Ponto de Extremidade Privado.

1. Na Área de Trabalho Remota do *myVM*, abra o PowerShell.
2. Insira nslookup myserver.database.windows.net

   Você receberá uma mensagem semelhante a esta:

    ```
    Server:  UnKnown
    Address:  168.63.129.16
    Non-authoritative answer:
    Name:    myserver.privatelink.database.windows.net
    Address:  10.0.0.5
    Aliases:  myserver.database.windows.net
    ```

3. Instale o SQL Server Management Studio
4. Em Conectar-se ao servidor, insira ou selecione estas informações:

   - Tipo de servidor: Selecione Mecanismo de Banco de Dados.
   - Nome do servidor: Selecione myserver.database.windows.net
   - Nome de Usuário: Insira um nome de usuário fornecido durante a criação.
   - Senha: Insira uma senha fornecida durante a criação.
   - Lembrar senha: Selecione Sim.

5. Selecione **Conectar**.
6. Procure **Bancos de Dados** no menu à esquerda.
7. (Opcionalmente) Crie ou consulte informações de consulta no *mydatabase*
8. Feche a conexão da área de trabalho remota para *myVm*.

## <a name="clean-up-resources"></a>Limpar os recursos

Quando não for mais necessário, você poderá usar az group delete para remover o grupo de recursos e todos os recursos que ele contém:

```azurecli-interactive
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>Próximas etapas

Saiba mais sobre o [Link Privado do Azure](private-link-overview.md)

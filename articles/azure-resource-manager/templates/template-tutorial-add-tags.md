---
title: Tutorial – adicionar marcas a recursos no modelo
description: Adicione marcas a recursos que você implanta em seu modelo do Azure Resource Manager. As marcas permitem que você organize logicamente os recursos.
author: mumian
ms.date: 10/04/2019
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: d7853d388b9159328a515818697ba9ddd5b95637
ms.sourcegitcommit: 984c5b53851be35c7c3148dcd4dfd2a93cebe49f
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/28/2020
ms.locfileid: "76773190"
---
# <a name="tutorial-add-tags-in-your-resource-manager-template"></a>Tutorial: Adicionar marcas em seu modelo do Resource Manager

Neste tutorial, você aprenderá como adicionar marcas a recursos em seu modelo. As [marcas](../management/tag-resources.md) ajudam você a organizar logicamente seus recursos. Os valores de tag aparecem em relatórios de custo. Este tutorial leva **8 minutos** para ser concluído.

## <a name="prerequisites"></a>Prerequisites

Recomendamos que você conclua o [tutorial sobre Modelos de início rápido](template-tutorial-quickstart-template.md), mas isso não é obrigatório.

É necessário ter o Visual Studio Code com a extensão Ferramentas do Resource Manager e o Azure PowerShell ou a CLI do Azure. Para obter mais informações, confira [Ferramentas de modelo](template-tutorial-create-first-template.md#get-tools).

## <a name="review-template"></a>Examinar modelo

O modelo anterior implantou uma conta de armazenamento, um plano do Serviço de Aplicativo e um aplicativo Web.

:::code language="json" source="~/resourcemanager-templates/get-started-with-templates/quickstart-template/azuredeploy.json":::

Após implantar esses recursos, você pode precisar controlar custos e localizar recursos que pertencem a uma categoria. Você pode adicionar marcas para ajudar a resolver esses problemas.

## <a name="add-tags"></a>Adicionar marcas

Marque recursos para adicionar valores que ajudam você a identificar seu uso. Por exemplo, você pode adicionar marcas que listam o ambiente e o projeto. Você pode adicionar marcas que identificam um centro de custo ou a equipe que tem a propriedade do recurso. Adicione valores que façam sentido para sua organização.

O exemplo a seguir realça as alterações no modelo. Copie o arquivo inteiro e substitua o modelo pelo conteúdo.

:::code language="json" source="~/resourcemanager-templates/get-started-with-templates/add-tags/azuredeploy.json" range="1-118" highlight="46-52,64,78,102":::

## <a name="deploy-template"></a>Implantar modelo

É hora de implantar o modelo e examinar os resultados.

Caso você não tenha criado o grupo de recursos, confira [Criar grupo de recursos](template-tutorial-create-first-template.md#create-resource-group). O exemplo pressupõe que você tenha definido a variável **templateFile** como o caminho para o arquivo de modelo, conforme mostrado no [primeiro tutorial](template-tutorial-create-first-template.md#deploy-template).

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell
New-AzResourceGroupDeployment `
  -Name addtags `
  -ResourceGroupName myResourceGroup `
  -TemplateFile $templateFile `
  -storagePrefix "store" `
  -storageSKU Standard_LRS `
  -webAppName demoapp
```

# <a name="azure-clitabazure-cli"></a>[CLI do Azure](#tab/azure-cli)

```azurecli
az group deployment create \
  --name addtags \
  --resource-group myResourceGroup \
  --template-file $templateFile \
  --parameters storagePrefix=store storageSKU=Standard_LRS webAppName=demoapp
```

---

## <a name="verify-deployment"></a>Verificar implantação

Você pode verificar a implantação explorando o grupo de recursos no portal do Azure.

1. Entre no [portal do Azure](https://portal.azure.com).
1. No menu à esquerda, selecione **Grupos de recursos**.
1. Selecione o grupo de recursos no qual você fez a implantação.
1. Selecione um dos recursos, como o recurso da conta de armazenamento. Você verá que agora ele tem marcas.

   ![Mostrar marcas](./media/template-tutorial-add-tags/show-tags.png)

## <a name="clean-up-resources"></a>Limpar os recursos

Se você estiver passando para o próximo tutorial, não será necessário excluir o grupo de recursos.

Se estiver parando agora, o ideal será limpar os recursos implantados excluindo o grupo de recursos.

1. No portal do Azure, escolha **Grupos de recursos** do menu à esquerda.
2. No campo **Filtrar por nome**, insira o nome do grupo de recursos.
3. Selecione o nome do grupo de recursos.
4. Escolha **Excluir grupo de recursos** no menu superior.

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você adicionou marcas aos recursos. No próximo tutorial, você aprenderá a usar os arquivos de parâmetro para simplificar a passagem de valores para o modelo.

> [!div class="nextstepaction"]
> [Usar arquivo de parâmetro](template-tutorial-use-parameter-file.md)

---
title: Estender seu experimento com R
titleSuffix: ML Studio (classic) - Azure
description: Como estender a funcionalidade de Azure Machine Learning Studio (clássico) por meio da linguagem R usando o módulo executar script R.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: xiaoharper
ms.author: zhanxia
ms.custom: previous-author=heatherbshapiro, previous-ms.author=hshapiro
ms.date: 03/20/2017
ms.openlocfilehash: 6c5c09d0214b8ab0d427b15e5637adb5e7cbfdee
ms.sourcegitcommit: 812bc3c318f513cefc5b767de8754a6da888befc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/12/2020
ms.locfileid: "77153036"
---
# <a name="azure-machine-learning-studio-classic-extend-your-experiment-with-r"></a>Azure Machine Learning Studio (clássico): Estenda seu experimento com o R 
Você pode estender a funcionalidade do Azure Machine Learning Studio (clássico) por meio da linguagem R usando o módulo [Executar script r][execute-r-script] .

Esse módulo aceita vários conjuntos de dados de entrada e produz um único conjunto de dados como saída. Você pode digitar um script R no parâmetro de **script r** do módulo [Executar script r][execute-r-script] .

Você pode acessar cada porta de entrada do módulo usando código semelhante ao seguinte:

    dataset1 <- maml.mapInputPort(1)

## <a name="listing-all-currently-installed-packages"></a>Listando todos os pacotes instalados no momento
A lista de pacotes instalados pode mudar. Uma lista de pacotes instalados no momento pode ser encontrada em [pacotes de R com suporte pelo Azure Machine Learning Studio (clássico)](https://msdn.microsoft.com/library/azure/mt741980.aspx).

Você também pode obter a lista completa e atual dos pacotes instalados inserindo o seguinte código no módulo [Executar script R][execute-r-script] :

    out <- data.frame(installed.packages(,,,fields="Description"))
    maml.mapOutputPort("out")

Isso envia a lista de pacotes para a porta de saída do módulo [Executar script R][execute-r-script] .
Para exibir a lista de pacotes, conecte um módulo de conversão, como [converter em CSV][convert-to-csv] , para a saída esquerda do módulo [Executar script R][execute-r-script] , executar o experimento e, em seguida, clique na saída do módulo de conversão e selecione **baixar**. 

![Baixar saída do módulo “Converter para CSV”](./media/extend-your-experiment-with-r/download-package-list.png)


<!--
For convenience, here is the [current full list with version numbers in Excel format](https://az754797.vo.msecnd.net/docs/RPackages.xlsx).
-->

## <a name="importing-packages"></a>Importando pacotes
Você pode importar pacotes que ainda não estão instalados usando os seguintes comandos no módulo [Executar script R][execute-r-script] :

    install.packages("src/my_favorite_package.zip", lib = ".", repos = NULL, verbose = TRUE)
    success <- library("my_favorite_package", lib.loc = ".", logical.return = TRUE, verbose = TRUE)

onde o arquivo `my_favorite_package.zip` contém o pacote.




<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[convert-to-csv]: https://msdn.microsoft.com/library/azure/faa6ba63-383c-4086-ba58-7abf26b85814/

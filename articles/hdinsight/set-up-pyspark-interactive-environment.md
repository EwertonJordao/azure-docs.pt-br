---
title: Ambiente interativo PySpark com ferramentas Azure HDInsight
description: Saiba como usar as Ferramentas do Azure HDInsight para Visual Studio Code para criar e enviar consultas e scripts.
keywords: VSCode, Ferramentas do Azure HDInsight, Hive, Python, PySpark, Spark, HDInsight, Hadoop, LLAP, Hive Interativo, Consulta Interativa
author: jejiang
ms.author: jejiang
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 06/13/2019
ms.openlocfilehash: db2336fb79207ada24b71e0e64f0aaaab543e4da
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73241550"
---
# <a name="set-up-the-pyspark-interactive-environment-for-visual-studio-code"></a>Configurar o ambiente interativo do PySpark para o Visual Studio Code

As etapas a seguir mostram como configurar o ambiente interativo do PySpark no VS Code.

Usamos comando **python/pip** para criar o ambiente virtual em seu caminho de Página Inicial. Se você deseja usar outra versão, precisa alterar a versão padrão do comando **python/pip** manualmente. Para obter mais detalhes, confira [update-alternatives](https://linux.die.net/man/8/update-alternatives).

1. Instale [Python](https://www.python.org/downloads/) e [pip](https://pip.pypa.io/en/stable/installing/).

   + Instale [https://www.python.org/downloads/](https://www.python.org/downloads/)python a partir de .
   + Instale [https://pip.pypa.io/en/stable/installing](https://pip.pypa.io/en/stable/installing/) pip (se não estiver instalado a partir da instalação python).
   + Valide se o Python e o pip são instalados com sucesso usando os seguintes comandos. (Opcional)

        ![Verifique o comando da versão python pip](./media/set-up-pyspark-interactive-environment/check-python-pip-version.png)

     > [!NOTE]
     > Recomenda-se instalar manualmente o Python em vez de usar a versão padrão do macOS.

2. Instale **virtualenv** executando o comando a seguir.

   ```
   pip install virtualenv
   ```

## <a name="other-packages"></a>Outros pacotes

Se você encontrar uma mensagem de erro, instale os pacotes necessários executando os seguintes comandos:

   ![Instale o pacote libkrb5 para python](./media/set-up-pyspark-interactive-environment/install-libkrb5-package.png)

```
sudo apt-get install libkrb5-dev
```

```
sudo apt-get install python-dev
```

Reinicie o VS Code e volte para o editor de scripts que está executando o **HDInsight: PySpark Interactive**.

## <a name="next-steps"></a>Próximas etapas

### <a name="demo"></a>Demonstração
* HDInsight para VS Code: [Vídeo](https://go.microsoft.com/fwlink/?linkid=858706)

### <a name="tools-and-extensions"></a>Ferramentas e extensões
* [Use a ferramenta Azure HDInsight para o Visual Studio Code](hdinsight-for-vscode.md)
* [Use o Azure Toolkit for IntelliJ para criar e enviar aplicativos Scala do Apache Spark](spark/apache-spark-intellij-tool-plugin.md)
* [ Use o Azure Toolkit for IntelliJ para depurar os aplicativos do Apache Spark remotamente por meio do SSH ](spark/apache-spark-intellij-tool-debug-remotely-through-ssh.md)
* [Use o Azure Toolkit for IntelliJ para depurar aplicativos Apache Spark remotamente por meio de VPN](spark/apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Use as ferramentas do HDInsight no Azure Toolkit for Eclipse para criar aplicativos do Apache Spark](spark/apache-spark-eclipse-tool-plugin.md)
* [Use os blocos de anotações do Apache Zeppelin com um cluster do Apache Spark no HDInsight](spark/apache-spark-zeppelin-notebook.md)
* [Kernels disponíveis para o notebook do Jupyter no cluster do Apache Spark para HDInsight](spark/apache-spark-jupyter-notebook-kernels.md)
* [Usar pacotes externos com blocos de notas Jupyter](spark/apache-spark-jupyter-notebook-use-external-packages.md)
* [Instalar o Jupyter em seu computador e conectar-se a um cluster Spark do HDInsight](spark/apache-spark-jupyter-notebook-install-locally.md)
* [Visualize dados do Apache Hive com o Microsoft Power BI no Azure HDInsight](hadoop/apache-hadoop-connect-hive-power-bi.md)
* [Usar o Apache Zeppelin para executar consultas do Apache Hive no HDInsight do Azure](./interactive-query/hdinsight-connect-hive-zeppelin.md)

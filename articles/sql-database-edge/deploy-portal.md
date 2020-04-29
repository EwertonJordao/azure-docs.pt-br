---
title: Implantar a visualização de borda do banco de dados SQL usando o portal do Azure | Microsoft Docs
description: Saiba como implantar a borda do banco de dados SQL do Azure usando o portal do Azure
keywords: implantar borda do banco de dados SQL
services: sql-database-edge
ms.service: sql-database-edge
ms.topic: conceptual
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 11/04/2019
ms.openlocfilehash: 9da922de38d820864b3f83de80fe64eb3ac792e4
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80246714"
---
# <a name="deploy-azure-sql-database-edge-preview"></a>Implantar a visualização de borda do banco de dados SQL do Azure

Visualização de borda do banco de dados SQL do Azure é um mecanismo de banco de dados relacional otimizado para implantações de IoT e Azure IoT Edge. Ele fornece recursos para criar uma camada de processamento e armazenamento de dados de alto desempenho para aplicativos e soluções de IoT. Este guia de início rápido mostra como começar a criar um módulo de borda do banco de dados SQL do Azure por meio de Azure IoT Edge usando o portal do Azure.

## <a name="before-you-begin"></a>Antes de começar

* Se você não tiver uma assinatura do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/).
* Entre no [portal do Azure](https://portal.azure.com/).
* Envie uma solicitação [aqui](https://azure.microsoft.com/services/sql-database-edge/#contact)para que a assinatura seja habilitada para implantação da borda do banco de dados SQL.
* Crie um [Hub IOT do Azure](../iot-hub/iot-hub-create-through-portal.md).
* Registre um [dispositivo de IOT Edge da portal do Azure](../iot-edge/how-to-register-device-portal.md).
* Prepare o dispositivo de IoT Edge para [implantar IOT Edge módulo do portal do Azure](../iot-edge/how-to-deploy-modules-portal.md).

> [!NOTE]
> Para implantar uma VM Linux do Azure como um dispositivo IoT Edge, consulte este [Guia de início rápido](../iot-edge/quickstart-linux.md).

## <a name="deploy-sql-database-edge-module-from-azure-marketplace"></a>Implantar o módulo de borda do banco de dados SQL do Azure Marketplace

O Azure Marketplace é um marketplace online de aplicativos e serviços onde você pode procurar por uma ampla variedade de aplicativos e soluções corporativas que são certificadas e otimizadas para execução no Azure, incluindo [módulos do IoT Edge](https://azuremarketplace.microsoft.com/marketplace/apps/category/internet-of-things?page=1&subcategories=iot-edge-modules). A borda do banco de dados SQL do Azure pode ser implantada em um dispositivo de borda por meio do Marketplace.

1. Localize o módulo de borda do banco de dados SQL do Azure no Azure Marketplace.<br><br>

   ![Borda do banco de dados SQL no MarketPlace](media/deploy-portal/find-offer-marketplace.png)

2. Escolha o plano de software que melhor corresponda às suas necessidades e clique em **criar**. <br><br>

   ![Escolha o plano de software correto](media/deploy-portal/pick-correct-plan.png)

3. Na página dispositivos de destino para IoT Edge módulo, especifique os seguintes detalhes e clique em **criar**

   |**Campo**  |**Descrição**  |
   |---------|---------|
   |Assinatura  |  A assinatura do Azure sob a qual o Hub IoT foi criado |
   |Hub IoT   |  Nome do Hub IoT em que o dispositivo IoT Edge está registrado e, em seguida, selecione a opção "implantar em um dispositivo"|
   |Nome do dispositivo IoT Edge  |  Nome do dispositivo de IoT Edge onde a borda do banco de dados SQL será implantada |

4. Na página **definir módulos** , navegue até a seção sobre módulos de implantação e clique em **Configurar** no módulo borda do banco de dados SQL. 

5. No painel **IOT Edge módulos personalizados** , especifique os valores desejados para as variáveis de ambiente e/ou personalize as opções de criação e as propriedades desejadas para o módulo. Para obter uma lista completa de variáveis de ambiente com suporte, consulte [SQL Server variáveis de ambiente de contêiner](/sql/linux/sql-server-linux-configure-environment-variables/).

   |**Parâmetro**  |**Descrição**|
   |---------|---------|
   | Name | Nome do módulo. |
   |SA_PASSWORD  | Especifique uma senha forte para a conta de administrador de borda do banco de dados SQL. |
   |MSSQL_LCID   | Define a ID de idioma a ser usada para o SQL Server. Por exemplo, 1036 é francês. |
   |MSSQL_COLLATION | Define a ordenação padrão para o SQL Server. Essa configuração substitui o mapeamento padrão de ID de idioma (LCID) para Agrupamento. |

   > [!NOTE]
   > Não altere nem atualize o URI da **imagem** ou as configurações de **ACCEPT_EULA** no módulo.

6. No painel **IOT Edge módulos personalizados** , atualize o valor de opções de criação de contêiner desejadas para a **porta do host**. Se você precisar implantar mais de um módulo do SQL DB Edge, certifique-se de atualizar a opção montagens para criar um novo par de origem & destino para o volume persistente. Para obter mais informações sobre montagens e volume, consulte [usar volumes](https://docs.docker.com/storage/volumes/) na documentação do Docker. 

   ```json
       {
         "HostConfig": {
           "Binds": [
             "sqlvolume:/sqlvolume"
           ],
           "PortBindings": {
             "1433/tcp": [
               {
                 "HostPort": "1433"
               }
             ]
           },
           "Mounts": [
             {
               "Type": "volume",
               "Source": "sqlvolume",
               "Target": "/var/opt/mssql"
             }
           ]
         },
         "Env": [
           "MSSQL_AGENT_ENABLED=TRUE",
           "MSSQL_PID=Developer"
         ]
       }
   ```

7. No painel **IOT Edge módulos personalizados** , atualize as *propriedades desejadas do módulo Set* para incluir o local do pacote do SQL e as informações do trabalho do Stream Analytics. Esses dois campos são opcionais e devem ser usados se você quiser implantar o módulo de borda do banco de dados SQL com um banco de dados e um trabalho de streaming.

   ```json
       {
         "properties.desired":
         {
           "SqlPackage": "<Optional_DACPAC_ZIP_SAS_URL>",
           "ASAJobInfo": "<Optional_ASA_Job_ZIP_SAS_URL>"
         }
       }
   ```

8. No painel **IOT Edge módulos personalizados** , defina *política de reinicialização* para o status sempre e *desejado* como em execução.
9. No painel **IOT Edge módulos personalizados** , clique em **salvar**.
10. Na página **definir módulos** , clique em **Avançar**.
11. Em **especificar rota (opcional)** da página **definir módulos** , especifique as rotas para módulo para módulo ou módulo para IOT Edge comunicação de Hub consulte [implantar módulos e estabelecer rotas no IOT Edge](../iot-edge/module-composition.md).
12. Clique em **Avançar**.
13. Clique em **Enviar**.

Neste guia de início rápido, você implantou um módulo de borda do banco de dados SQL em um dispositivo IoT Edge.

## <a name="next-steps"></a>Próximas etapas

- [Machine Learning e inteligência artificial com ONNX na borda do banco de dados SQL](onnx-overview.md).
- Criando uma solução de IoT de ponta a ponta com a borda do banco de dados SQL usando IoT Edge.

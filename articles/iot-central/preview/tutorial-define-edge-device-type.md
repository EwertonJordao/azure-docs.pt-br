---
title: Tutorial – Definir um novo tipo de dispositivo Azure IoT Edge no Azure IoT Central
description: Este tutorial mostra a você, como construtor, como criar um dispositivo Azure IoT Edge em seu aplicativo do Azure IoT Central. Você define a telemetria, estado, propriedades e comandos para o seu tipo.
author: rangv
ms.author: rangv
ms.date: 12/09/2019
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: peterpr
ms.openlocfilehash: e5aa41023e1d7f84e8de095d13835d5ebdf66f57
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75434864"
---
# <a name="tutorial-define-a-new-azure-iot-edge-device-type-in-your-azure-iot-central-application-preview-features"></a>Tutorial: Definir um novo tipo de dispositivo Azure IoT Edge no aplicativo do Azure IoT Central (versão prévia dos recursos)

[!INCLUDE [iot-central-pnp-original](../../../includes/iot-central-pnp-original-note.md)]

Este tutorial mostra a você, como um construtor, como usar um modelo de dispositivo para definir um novo tipo de dispositivo Azure IoT Edge no seu aplicativo do Azure IoT Central. 

Para obter uma visão geral, confira [O que é o Azure IoT Central (versão prévia do recurso)?](overview-iot-central.md). 

O IoT Edge é composto por três componentes:
* Os **módulos do IoT Edge** são contêineres que executam serviços do Azure, serviços do parceiro ou o seu próprio código. Os módulos são implantados em dispositivos do IoT Edge e executados localmente nesses dispositivos.
* O **runtime do IoT Edge** é executado em cada dispositivo IoT Edge e gerencia os módulos implantados em cada dispositivo.
* Uma **interface baseada em nuvem** permite monitorar e gerenciar dispositivos do IoT Edge remotamente. O IoT Central é a interface de nuvem.

Um dispositivo **Azure IoT Edge** pode ser um dispositivo de gateway com dispositivos downstream conectados ao dispositivo IoT Edge. Este tutorial compartilha mais informações sobre os padrões de conectividade do dispositivo downstream.

Um **modelo de dispositivo** define as funcionalidades do seu dispositivo e dos módulos IoT Edge. As funcionalidades incluem telemetria que o módulo envia, as propriedades do módulo e os comandos aos quais um módulo responde.

Neste tutorial, você criará um modelo de dispositivo Sensor de Ambiente. Um dispositivo de sensor ambiental:

* Envia telemetria, como a temperatura.
* Responde às propriedades graváveis quando atualizado na nuvem, como o intervalo de envio de telemetria.
* Responde a comandos como a redefinição da temperatura.

Além disso, neste tutorial, você criará um modelo de dispositivo de Gateway de Ambiente. Um dispositivo de gateway ambiental:

* Envia telemetria, como a temperatura.
* Responde às propriedades graváveis quando atualizado na nuvem, como o intervalo de envio de telemetria.
* Responde a comandos como a redefinição da temperatura.
* Permite relações com outros modelos de funcionalidade do dispositivo.


Neste tutorial, você aprenderá como:

> [!div class="checklist"]
> * Criar um modelo de dispositivo Azure IoT Edge.
> * Carregar um manifesto de implantação.
> * Criar funcionalidades, incluindo telemetria, propriedades e comandos para cada módulo.
> * Definir uma visualização para a telemetria do módulo.
> * Adicionar relações aos modelos de dispositivo downstream.
> * Publicar seu modelo de dispositivo.

## <a name="prerequisites"></a>Prerequisites

Para concluir este tutorial, você precisa [criar um aplicativo do Azure IoT Central](quick-deploy-iot-central.md).


## <a name="downstream-device-relationships-with-a-gateway-and-modules"></a>Relações de dispositivo downstream com um gateway e módulos

Os dispositivos downstream podem se conectar a um dispositivo de gateway do IoT Edge por meio do módulo `$edgeHub`. Esse dispositivo IoT Edge se torna um gateway transparente neste cenário.

![Diagrama de gateway transparente](./media/tutorial-define-edge-device-type/gateway-transparent.png)

Os dispositivos downstream também podem se conectar a um dispositivo de gateway do IoT Edge por meio de um módulo personalizado. No cenário a seguir, os dispositivos downstream conectam-se por meio de um módulo personalizado Modbus.

![Diagrama da conexão de módulo personalizado](./media/tutorial-define-edge-device-type/gateway-module.png)

O diagrama a seguir mostra a conexão a um dispositivo de gateway do IoT Edge por meio de ambos os tipos de módulos (personalizado e `$edgeHub`).  

![Diagrama de conexão usando ambos os módulos de conexão](./media/tutorial-define-edge-device-type/gateway-module-transparent.png)

Por fim, os dispositivos downstream podem se conectar a um dispositivo de gateway do IoT Edge por meio de vários módulos personalizados. O diagrama a seguir mostra os dispositivos downstream que se conectam por meio de um módulo personalizado Modbus, um módulo BLE personalizado e o módulo `$edgeHub`. 

![Diagrama de conexão por meio de vários módulos personalizados](./media/tutorial-define-edge-device-type/gateway-module2-transparent.png)


## <a name="create-a-template"></a>Criar um modelo

Como um construtor, você pode criar e editar os modelos de dispositivo do IoT Edge no seu aplicativo. Depois de publicar um modelo de dispositivo, você pode conectar dispositivos reais que implementam o modelo de dispositivo.

### <a name="select-device-template-type"></a>Selecionar um tipo de modelo de dispositivo 

Para adicionar um novo modelo de dispositivo a seu aplicativo, selecione **Modelos de Dispositivo** no painel esquerdo.

![Captura de tela do Aplicativo de Visualização com Modelos de Dispositivo realçado](./media/tutorial-define-edge-device-type/edgedevicetemplate.png)

Selecione **+ Novo** para começar a criar um modelo de dispositivo.

![Captura de tela da página Modelos de dispositivo com Novo realçado](./media/tutorial-define-edge-device-type/edgedevicetemplatenew.png)

Na página **Selecionar tipo de modelo**, selecione **Azure IoT Edge** e selecione **Avançar: Personalizar**.

![Captura de tela da página Selecionar tipo de modelo](./media/tutorial-define-edge-device-type/selectiotedge.png)

### <a name="customize-device-template"></a>Personalizar modelo de dispositivo

No IoT Edge, é possível implantar e gerenciar a lógica de negócios na forma de módulos. Os módulos do IoT Edge são a menor unidade de computação gerenciada pelo IoT Edge e podem conter serviços do Azure (por exemplo, o Azure Stream Analytics) ou o código específico da sua própria solução. Para entender como os módulos são desenvolvidos, implantados e mantidos, confira [Módulos do IoT Edge](../../iot-edge/iot-edge-modules.md).

Em um alto nível, um manifesto de implantação é uma lista de módulos gêmeos que são configurados com suas propriedades desejadas. Um manifesto de implantação informa um dispositivo IoT Edge (ou um grupo de dispositivos), quais módulos instalar e como configurá-los. Manifestos de implantação incluem as propriedades desejadas para cada módulo gêmeo. Dispositivos do IoT Edge relatam de volta as propriedades relatadas para cada módulo.

Use o Visual Studio Code para criar um manifesto de implantação. Para saber mais, confira [Azure IoT Edge para Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge).

Aqui está um manifesto de implantação básico, com um módulo como um exemplo a ser usado neste tutorial. Copie o JSON a seguir e salve-o como um arquivo .json. 

   ```JSON
   {
     "modulesContent": {
       "$edgeAgent": {
         "properties.desired": {
           "schemaVersion": "1.0",
           "runtime": {
             "type": "docker",
             "settings": {
               "minDockerVersion": "v1.25",
               "loggingOptions": "",
               "registryCredentials": {}
             }
           },
           "systemModules": {
             "edgeAgent": {
               "type": "docker",
               "settings": {
                 "image": "mcr.microsoft.com/azureiotedge-agent:1.0",
                 "createOptions": "{}"
               }
             },
             "edgeHub": {
               "type": "docker",
               "status": "running",
               "restartPolicy": "always",
               "settings": {
                 "image": "mcr.microsoft.com/azureiotedge-hub:1.0",
                 "createOptions": "{}"
               }
             }
           },
           "modules": {
             "SimulatedTemperatureSensor": {
               "version": "1.0",
               "type": "docker",
               "status": "running",
               "restartPolicy": "always",
               "settings": {
                 "image": "mcr.microsoft.com/azureiotedge-simulated-temperature-sensor:1.0",
                 "createOptions": "{}"
               }
             }
           }
         }
       },
       "$edgeHub": {
         "properties.desired": {
           "schemaVersion": "1.0",
           "routes": {
               "route": "FROM /* INTO $upstream"
           },
           "storeAndForwardConfiguration": {
             "timeToLiveSecs": 7200
           }
         }
       },
       "SimulatedTemperatureSensor": {
         "properties.desired": {
              "SendData": true,
              "SendInterval": 10
         }
       }
     }
   }
   ```

#### <a name="upload-an-iot-edge-deployment-manifest"></a>Fazer upload de um manifesto de implantação do IoT Edge

Na página **Personalizar o dispositivo**, em **Fazer upload de um manifesto de implantação do Azure IoT Edge**, selecione **Procurar**. 

![Captura de tela da página Personalizar dispositivo com Procurar realçado](./media/tutorial-define-edge-device-type/edgedevicetemplateuploadmanifest.png)

Se planejar criar um modelo de dispositivo de gateway do IoT Edge, marque **Dispositivo de gateway com dispositivos downstream**.

![Captura de tela da página Personalizar dispositivo com Dispositivo de gateway com dispositivos downstream realçado](./media/tutorial-define-edge-device-type/gateway-upload.png)

Na caixa de diálogo de seleção de arquivo, selecione o arquivo de manifesto de implantação e selecione **Abrir**.

O IoT Edge valida o arquivo de manifesto de implantação em relação a um esquema. Se a validação for bem-sucedida, selecione **Examinar**.

![Captura de tela da página Personalizar dispositivo com Manifesto de Implantação e Examinar realçados](./media/tutorial-define-edge-device-type/deploymentmanifestvalidate.png)

O fluxograma a seguir mostra um ciclo de vida do manifesto de implantação no IoT Central.

![Fluxograma do ciclo de vida do manifesto de implantação](./media/tutorial-define-edge-device-type/dmflow.png)

Em seguida, você verá uma página de análise com detalhes do manifesto de implantação. Esta página mostra uma lista de módulos do manifesto de implantação. Neste tutorial, observe que o módulo `SimulatedTemperatureSensor` está listado. Selecione **Criar**.

![Captura de tela da página de análise com Módulo e Criar realçados](./media/tutorial-define-edge-device-type/edgedevicetemplatereview.png)

Se você tiver selecionado um dispositivo de gateway, verá a página de análise a seguir.

![Captura de tela da página Examinar com Azure IoT Edge Gateway realçado](./media/tutorial-define-edge-device-type/gateway-review.png)


Você cria um modelo de dispositivo com modelos de capacidade do módulo. Neste tutorial, você cria um modelo de dispositivo com o modelo de funcionalidade do módulo `SimulatedTemperatureSensor`. 

Altere o título do modelo de dispositivo para **Modelo de Dispositivo do Sensor de Ambiente**.

![Captura de tela do modelo de dispositivo com título atualizado realçado](./media/tutorial-define-edge-device-type/edgedevicetemplatelanding.png)

No dispositivo IoT Edge, modele o IoT Plug and Play da seguinte maneira:
* Cada modelo de dispositivo IoT Edge tem um modelo de funcionalidade de dispositivo.
* Para cada módulo personalizado listado no manifesto de implantação, um modelo de funcionalidade do módulo é gerado.
* Um relacionamento é estabelecida entre cada modelo de funcionalidade do módulo e um modelo de funcionalidade do dispositivo.
* Um modelo de funcionalidade do módulo implementa interfaces de módulo.
* Cada interface de módulo contém telemetria, propriedades e comandos.

![Diagrama de modelagem do IoT Edge](./media/tutorial-define-edge-device-type/edgemodelling.png)

#### <a name="add-capabilities-to-a-module-capability-model"></a>Adicionar funcionalidades a um modelo de capacidade do módulo

Aqui está uma saída de amostra do módulo `SimulatedTemperatureSensor`:
```json
{

    "machine": {

        "temperature": 75.0,
        "pressure": 40.2
    },
    "ambient": {
        "temperature": 23.0,
        "humidity": 30.0
    },
    "timeCreated": ""
}
```

Você pode adicionar recursos ao módulo `SimulatedTemperatureSensor`, que refletirá a saída anterior. 

1. Para gerenciar uma interface do modelo de funcionalidade do módulo `SimulatedTemperatureSensor`, selecione **Gerenciar** > **Adicionar Funcionalidade**. 

    ![Captura de tela do Modelo de Sensor de Ambiente com Adicionar Funcionalidade realçado](./media/tutorial-define-edge-device-type/edgetemplateaddcapability.png)
  
1. Adicione um computador como um tipo de objeto.
  
    ![Captura de tela da página Funcionalidades do Modelo do Sensor de Ambiente com Esquema realçado](./media/tutorial-define-edge-device-type/edgetemplatemachineobject.png)

1. Selecione **Definir**. Na caixa de diálogo que aparece, altere o nome do objeto para **Computador**. Crie propriedades de temperatura e pressão e selecione **Aplicar**.
  
    ![Captura de tela da caixa de diálogo de atributos com várias opções realçadas](./media/tutorial-define-edge-device-type/edgetemplatemachineattributes.png)
  
1. Adicione **ambiente** como um tipo de objeto.

1. Selecione **Definir**. Na caixa de diálogo que aparece, altere o nome do objeto para **ambiente**. Crie propriedades de temperatura e umidade e selecione **Aplicar**.
  
    ![Captura de tela da caixa de diálogo de atributos com várias opções realçadas](./media/tutorial-define-edge-device-type/edgetemplateambientattributes.png)

  
1. Adicione `timeCreated` como um tipo `DateTime` e selecione **Salvar**.
  
    ![Captura de tela do Modelo de Sensor de Ambiente, com Salvar realçado](./media/tutorial-define-edge-device-type/edgetemplateallattributes.png)


### <a name="add-relationships"></a>Adicionar relações

Se tiver selecionado o dispositivo IoT Edge como um dispositivo de gateway, poderá adicionar relações de downstream aos modelos de funcionalidade do dispositivo para os dispositivos que você deseja conectar ao dispositivo de gateway.
  
  ![Captura de tela do Modelo de Gateway de Ambiente com Adicionar Relacionamento realçado](./media/tutorial-define-edge-device-type/gateway-add-relationship.png)

Você pode adicionar um relacionamento em um dispositivo ou em um módulo.
  
  ![Captura de tela do Modelo de Gateway de Ambiente, com relacionamentos no nível do dispositivo e do módulo realçados](./media/tutorial-define-edge-device-type/gateway-relationship-types.png)


Você pode selecionar um modelo de funcionalidade de dispositivo downstream ou o símbolo de asterisco. 
  
  ![Captura de tela do Modelo de Gateway de Ambiente com Destino realçado](./media/tutorial-define-edge-device-type/gateway-downstream-rel.png)

  Para este tutorial, selecione o asterisco. Essa opção permite qualquer relacionamento downstream. Em seguida, selecione **Salvar**.

  ![Captura de tela do Modelo de Gateway de Ambiente com Destino realçado](./media/tutorial-define-edge-device-type/gateway-add-relationship-asterix.png)


### <a name="add-cloud-properties"></a>Adicionar propriedades da nuvem

Um modelo de dispositivo pode incluir propriedades de nuvem. As propriedades de nuvem existem somente no aplicativo IoT Central e nunca são enviadas nem recebidas de um dispositivo.

1. Selecione **Propriedades de Nuvem** >  **+ Adicionar Propriedade de Nuvem**. Use as informações na tabela a seguir para adicionar uma propriedade de nuvem ao seu modelo de dispositivo.

    | Nome de exibição      | Tipo semântico | Esquema |
    | ----------------- | ------------- | ------ |
    | Data do Último Serviço | Nenhum          | Data   |
    | Nome do cliente     | Nenhum          | String |

2. Clique em **Salvar**.

  
    ![Captura de tela do Modelo de Sensor de Ambiente, com Salvar realçado](./media/tutorial-define-edge-device-type/edgetemplatecloudproperties.png)

### <a name="add-customizations"></a>Adicionar personalizações

Use personalizações para modificar uma interface ou para adicionar recursos específicos do IoT Central a uma funcionalidade que não exige o controle de versão de seu modelo de funcionalidade do dispositivo. Você pode personalizar os campos quando o modelo de funcionalidade estiver em um rascunho ou em um estado publicado. Você só pode personalizar campos que não interrompem a compatibilidade de interface. Por exemplo, você pode:

- Personalizar o nome de exibição e as unidades de uma funcionalidade.
- Adicionar uma cor padrão a ser usada quando o valor for exibido em um gráfico.
- Especificar os valores iniciais, mínimos e máximos, para uma propriedade.

Você não pode personalizar o nome da funcionalidade ou o tipo de funcionalidade.

Quando tiver terminado de personalizar, selecione **Salvar**.
  
![Captura de tela da página Personalização do Modelo do Sensor de Ambiente](./media/tutorial-define-edge-device-type/edgetemplatecustomize.png)


### <a name="create-views"></a>Criar exibições

Como um construtor, você pode personalizar o aplicativo para exibir informações relevantes sobre o dispositivo de sensor ambiental para um operador. Suas personalizações habilitam o operador a gerenciar os dispositivos de sensor ambiental conectados ao aplicativo. Você pode criar dois tipos de modos de exibição para que um operador use para interagir com dispositivos:

* Formulários para exibir e editar propriedades de dispositivo e de nuvem.
* Painéis para visualizar dispositivos.

### <a name="configure-a-view-to-visualize-devices"></a>Configurar um modo de exibição para visualizar dispositivos

Um painel de dispositivos permite que um operador visualize um dispositivo usando gráficos e métricas. Como um construtor, você pode definir quais informações aparecem em um painel do dispositivo. Você pode definir vários painéis para os dispositivos. Para criar um painel para visualizar a telemetria do sensor ambiental, selecione **Exibições** > **Como Visualizar o Dispositivo**:

  
![Captura de tela da página Exibições do Modelo do Sensor de Ambiente com Como Visualizar o Dispositivo realçado](./media/tutorial-define-edge-device-type/visualizingthedevice.png)


Telemetria de Ambiente e Telemetria de Computador são objetos complexos. Para criar gráficos:

1. Arraste a **Telemetria de Ambiente** e selecione **Gráfico de linhas**. 
  
   ![Captura de tela do modelo de sensor de ambiente, com Telemetria de Ambiente e Gráfico de Linhas realçados](./media/tutorial-define-edge-device-type/sensorambientchart.png)

1. Selecione o ícone de configurar. Selecione **Temperatura** e **Umidade** para visualizar os dados e selecione **Atualizar configuração**. 
  
   ![Captura de tela do Modelo de Sensor de Ambiente, com várias opções realçadas](./media/tutorial-define-edge-device-type/sensorambienttelemetrychart.png)

1. Clique em **Salvar**.

Você pode adicionar mais blocos que mostram outras propriedades ou valores de telemetria. Você também pode adicionar texto estático, links e imagens. Para mover ou redimensionar um bloco no painel, mova o ponteiro do mouse sobre o bloco e solte-o para uma nova localização ou para redimensioná-lo.
  
![Captura de tela da exibição de Painel de Modelo de Sensor de Ambiente](./media/tutorial-define-edge-device-type/viewsdashboard.png)

### <a name="add-a-device-form"></a>Adicionar um formulário de dispositivo

Um formulário de dispositivo permite que um operador edite as propriedades de dispositivo graváveis e as propriedades de nuvem. Como um construtor, você pode definir vários formulários e escolher quais propriedades de dispositivo e de nuvem mostrar em cada formulário. Você também pode exibir propriedades de dispositivo somente leitura em um formulário.

Para criar um formulário para exibir e editar as propriedades do sensor ambiental:

1. No **Modelo de Sensor Ambiental**, acesse **Exibições**. Selecione o bloco **Editando dados de Dispositivo e de Nuvem** para adicionar uma nova exibição.
  
   ![Captura de tela da página Exibições de Modelo de Sensor Ambiental, com Edição de Dispositivo e Dados na nuvem realçados](./media/tutorial-define-edge-device-type/editingdeviceandclouddata.png)

1. Insira o nome do formulário **Propriedades do Sensor Ambiental**.

1. Arraste as propriedades da nuvem **Nome do cliente** e **Última data de serviço** até a seção existente no formulário.
  
   ![Captura de tela da página Exibições de Modelo do Sensor Ambiental, com várias opções realçadas](./media/tutorial-define-edge-device-type/views-properties.png)

1. Clique em **Salvar**.

## <a name="publish-a-device-template"></a>Publicar um modelo de dispositivo

Antes de criar um sensor ambiental simulado ou conectar um sensor ambiental real, você precisa publicar o modelo do dispositivo.

Para publicar um modelo de dispositivo:

1. Vá para seu modelo de dispositivo na página **Modelos de Dispositivo**.

2. Selecione **Publicar**.
  
    ![Captura de tela do Modelo de Sensor Ambiental com Publicar realçado](./media/tutorial-define-edge-device-type/edgetemplatepublish.png)

1. Na caixa de diálogo **Publicar um Modelo de Dispositivo**, escolha **Publicar**.
  
    ![Captura de tela da caixa de diálogo Publicar um Modelo de Dispositivo com Publicar realçado](./media/tutorial-define-edge-device-type/edgepublishtemplate.png)

Depois que um modelo de dispositivo é publicado, ele fica visível na página **Dispositivos** e para o operador. Em um modelo de dispositivo publicado, você não pode editar um modelo de funcionalidade do dispositivo sem criar uma nova versão. No entanto, você pode fazer atualizações nas propriedades de nuvem, nas personalizações e nos modos de exibição, em um modelo de dispositivo publicado. Essas atualizações não fazem com que uma nova versão seja criada. Depois de fazer qualquer alteração, selecione **Publicar** para enviar por push essas alterações para o operador.
  
![Captura de tela da lista Modelos de dispositivo de modelos publicados](./media/tutorial-define-edge-device-type/publishedtemplate.png)

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você aprendeu a:

* Criar uma borda como um modelo de dispositivo de folha.
* Gerar módulos de um manifesto de implantação carregado.
* Adicionar propriedades e telemetria de tipo complexo.
* Criar propriedades da nuvem.
* Criar personalizações.
* Definir uma visualização para a telemetria do dispositivo.
* Publicar seu modelo de dispositivo de borda.

Agora que você criou um modelo de dispositivo no aplicativo Azure IoT Central, é possível fazer isto a seguir:

> [!div class="nextstepaction"]
> [Conectar dispositivo de borda](./tutorial-add-edge-as-leaf-device.md)

---
title: Referência da biblioteca de cliente de funções definidas pelo usuário-Azure digital gêmeos | Microsoft Docs
description: Documentação de referência da biblioteca de cliente de funções definidas pelo usuário do Azure digital gêmeos.
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: article
ms.date: 01/17/2020
ms.custom: seodec18
ms.openlocfilehash: bd6095daca51ddca0cfb4b34ca86e763df9a3d02
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2020
ms.locfileid: "76276808"
---
# <a name="user-defined-functions-client-library-reference"></a>Biblioteca de clientes com funções definidas pelo usuário

Este documento fornece informações de referência para a biblioteca de clientes de funções definidas pelo usuário dos Gêmeos Digitais do Azure.

## <a name="helper-methods"></a>Métodos auxiliares

A biblioteca de clientes define os métodos auxiliares para as operações mais usadas.

### <a name="getspacemetadataid--space"></a>getSpaceMetadata(id) ⇒ `space`

Dado um identificador de espaço, essa função recupera o espaço do grafo.

**Tipo**: função global

| Parâmetro  | Type                | Descrição  |
| ---------- | ------------------- | ------------ |
| *id*  | `guid` | Identificador de espaço |

### <a name="getsensormetadataid--sensor"></a>getSensorMetadata(id) ⇒ `sensor`

Dado um identificador de sensor, essa função recupera o sensor do grafo.

**Tipo**: função global

| Parâmetro  | Type                | Descrição  |
| ---------- | ------------------- | ------------ |
| *id*  | `guid` | Identificador de sensor |

### <a name="getdevicemetadataid--device"></a>getDeviceMetadata(id) ⇒ `device`

Dado um identificador de dispositivo, essa função recupera o dispositivo do grafo.

**Tipo**: função global

| Parâmetro  | Type                | Descrição  |
| ------ | ------------------- | ------------ |
| *id* | `guid` | Identificador de dispositivo |

### <a name="getsensorvaluesensorid-datatype--value"></a>getSensorValue(sensorId, dataType) ⇒ `value`

Dado um identificador de sensor e o tipo de dados, essa função recupera o valor atual para esse sensor.

**Tipo**: função global

| Parâmetro  | Type                | Descrição  |
| ------ | ------------------- | ------------ |
| *sensorId*  | `guid` | Identificador de sensor |
| *dataType*  | `string` | Tipo de dados de sensor |

### <a name="getspacevaluespaceid-valuename--value"></a>getSpaceValue(spaceId, valueName) ⇒ `value`

Dado um identificador de espaço e o nome do valor, essa função recupera o valor atual para essa propriedade de espaço.

**Tipo**: função global

| Parâmetro  | Type                | Descrição  |
| ------ | ------------------- | ------------ |
| *spaceId*  | `guid` | Identificador de espaço |
| *valueName* | `string` | Nome da propriedade de espaço |

### <a name="getsensorhistoryvaluessensorid-datatype--value"></a>getSensorHistoryValues(sensorId, dataType) ⇒ `value[]`

Dado um identificador de sensor e o tipo de dados, essa função recupera os valores históricos desse sensor.

**Tipo**: função global

| Parâmetro  | Type                | Descrição  |
| ------ | ------------------- | ------------ |
| *sensorId* | `guid` | Identificador de sensor |
| *dataType* | `string` | Tipo de dados de sensor |

### <a name="getspacehistoryvaluesspaceid-datatype--value"></a>getSpaceHistoryValues(spaceId, dataType) ⇒ `value[]`

Dado um identificador de espaço e o nome do valor, essa função recupera os valores históricos dessa propriedade no espaço.

**Tipo**: função global

| Parâmetro  | Type                | Descrição  |
| ------ | ------------------- | ------------ |
| *spaceId* | `guid` | Identificador de espaço |
| *valueName* | `string` | Nome da propriedade de espaço |

### <a name="getspacechildspacesspaceid--space"></a>getSpaceChildSpaces(spaceId) ⇒ `space[]`

Dado um identificador de espaço, essa função recupera os espaços filhos para esse espaço pai.

**Tipo**: função global

| Parâmetro  | Type                | Descrição  |
| ------ | ------------------- | ------------ |
| *spaceId* | `guid` | Identificador de espaço |

### <a name="getspacechildsensorsspaceid--sensor"></a>getSpaceChildSensors(spaceId) ⇒ `sensor[]`

Dado um identificador de espaço, essa função recupera os sensores filhos para esse espaço pai.

**Tipo**: função global

| Parâmetro  | Type                | Descrição  |
| ------ | ------------------- | ------------ |
| *spaceId* | `guid` | Identificador de espaço |

### <a name="getspacechilddevicesspaceid--device"></a>getSpaceChildDevices(spaceId) ⇒ `device[]`

Dado um identificador de espaço, essa função recupera os dispositivos filhos para esse espaço pai.

**Tipo**: função global

| Parâmetro  | Type                | Descrição  |
| ------ | ------------------- | ------------ |
| *spaceId* | `guid` | Identificador de espaço |

### <a name="getdevicechildsensorsdeviceid--sensor"></a>getDeviceChildSensors(deviceId) ⇒ `sensor[]`

Dado um identificador de dispositivo, essa função recupera os sensores filhos desse dispositivo pai.

**Tipo**: função global

| Parâmetro  | Type                | Descrição  |
| ------ | ------------------- | ------------ |
| *deviceId* | `guid` | Identificador de dispositivo |

### <a name="getspaceparentspacechildspaceid--space"></a>getSpaceParentSpace(childSpaceId) ⇒ `space`

Dado um identificador de espaço, essa função recupera o espaço pai.

**Tipo**: função global

| Parâmetro  | Type                | Descrição  |
| ------ | ------------------- | ------------ |
| *childSpaceId* | `guid` | Identificador de espaço |

### <a name="getsensorparentspacechildsensorid--space"></a>getSensorParentSpace(childSensorId) ⇒ `space`

Dado um identificador de sensor, essa função recupera o espaço pai.

**Tipo**: função global

| Parâmetro  | Type                | Descrição  |
| ------ | ------------------- | ------------ |
| *childSensorId* | `guid` | Identificador de sensor |

### <a name="getdeviceparentspacechilddeviceid--space"></a>getDeviceParentSpace(childDeviceId) ⇒ `space`

Dado um identificador de dispositivo, essa função recupera o espaço pai.

**Tipo**: função global

| Parâmetro  | Type                | Descrição  |
| ------ | ------------------- | ------------ |
| *childDeviceId* | `guid` | Identificador de dispositivo |

### <a name="getsensorparentdevicechildsensorid--space"></a>getSensorParentDevice(childSensorId) ⇒ `space`

Dado um identificador de sensor, essa função recupera o dispositivo pai.

**Tipo**: função global

| Parâmetro  | Type                | Descrição  |
| ------ | ------------------- | ------------ |
| *childSensorId* | `guid` | Identificador de sensor |

### <a name="getspaceextendedpropertyspaceid-propertyname--extendedproperty"></a>getSpaceExtendedProperty(spaceId, propertyName) ⇒ `extendedProperty`

Dado um identificador de espaço, essa função recupera a propriedade e o valor do espaço.

**Tipo**: função global

| Parâmetro  | Type                | Descrição  |
| ------ | ------------------- | ------------ |
| *spaceId* | `guid` | Identificador de espaço |
| *propertyName* | `string` | Nome da propriedade de espaço |

### <a name="getsensorextendedpropertysensorid-propertyname--extendedproperty"></a>getSensorExtendedProperty(sensorId, propertyName) ⇒ `extendedProperty`

Dado um identificador de sensor, esta função recupera a propriedade e o valor do sensor.

**Tipo**: função global

| Parâmetro  | Type                | Descrição  |
| ------ | ------------------- | ------------ |
| *sensorId* | `guid` | Identificador de sensor |
| *propertyName* | `string` | Nome da propriedade do sensor |

### <a name="getdeviceextendedpropertydeviceid-propertyname--extendedproperty"></a>getDeviceExtendedProperty(deviceId, propertyName) ⇒ `extendedProperty`

Dado um identificador de dispositivo, essa função recupera a propriedade e o valor do dispositivo.

**Tipo**: função global

| Parâmetro  | Type                | Descrição  |
| ------ | ------------------- | ------------ |
| *deviceId* | `guid` | Identificador de dispositivo |
| *propertyName* | `string` | Nome da propriedade do dispositivo |

### <a name="setsensorvaluesensorid-datatype-value"></a>setSensorValue(sensorId, dataType, value)

Essa função define um valor no objeto sensor com o tipo de dados dado.

**Tipo**: função global

| Parâmetro  | Type                | Descrição  |
| ------ | ------------------- | ------------ |
| *sensorId* | `guid` | Identificador de sensor |
| *dataType*  | `string` | Tipo de dados de sensor |
| *value*  | `string` | Valor |

### <a name="setspacevaluespaceid-datatype-value"></a>setSpaceValue(spaceId, dataType, value)

Essa função define um valor no objeto de espaço com o tipo de dados fornecido.

**Tipo**: função global

| Parâmetro  | Type                | Descrição  |
| ------ | ------------------- | ------------ |
| *spaceId* | `guid` | Identificador de espaço |
| *dataType* | `string` | Tipo de dados |
| *value* | `string` | Valor |

### <a name="logmessage"></a>log(message)

Essa função registra a seguinte mensagem dentro da função definida pelo usuário.

**Tipo**: função global

| Parâmetro  | Type                | Descrição  |
| ------ | ------------------- | ------------ |
| *message* | `string` | Mensagem a ser registrada em log |

### <a name="sendnotificationtopologyobjectid-topologyobjecttype-payload"></a>sendNotification(topologyObjectId, topologyObjectType, payload)

Essa função envia uma notificação personalizada a ser despachada.

**Tipo**: função global

| Parâmetro  | Type                | Descrição  |
| ------ | ------------------- | ------------ |
| *topologyObjectId*  | `guid` | Identificador de objeto do grafo. Exemplos são espaço, sensor e ID do dispositivo.|
| *topologyObjectType*  | `string` | Exemplos são sensor e dispositivo.|
| *payload*  | `string` | A carga útil JSON a ser enviada com a notificação. |

## <a name="return-types"></a>Tipos de retorno

Os modelos de resposta retornados de métodos auxiliares de referência do cliente são descritos abaixo.

### <a name="space"></a>Space

```JSON
{
  "id": "00000000-0000-0000-0000-000000000000",
  "name": "Space",
  "friendlyName": "Conference Room",
  "typeId": 0,
  "parentSpaceId": "00000000-0000-0000-0000-000000000001",
  "subtypeId": 0
}
```

### <a name="space-methods"></a>Métodos de espaço

#### <a name="parent--space"></a>Parent() ⇒ `space`

Essa função retorna o espaço pai do espaço atual.

#### <a name="childsensors--sensor"></a>ChildSensors() ⇒ `sensor[]`

Essa função retorna os sensores filhos do espaço atual.

#### <a name="childdevices--device"></a>ChildDevices() ⇒ `device[]`

Essa função retorna os dispositivos filhos do espaço atual.

#### <a name="extendedpropertypropertyname--extendedproperty"></a>ExtendedProperty(propertyName) ⇒ `extendedProperty`

Essa função retorna a propriedade estendida e o valor para o espaço atual.

| Parâmetro  | Type                | Descrição  |
| ------ | ------------------- | ------------ |
| *propertyName* | `string` | Nome da propriedade estendida |

#### <a name="valuevaluename--value"></a>Value(valueName) ⇒ `value`

Essa função retorna o valor do espaço atual.

| Parâmetro  | Type                | Descrição  |
| ------ | ------------------- | ------------ |
| *valueName* | `string` | Nome do valor |

#### <a name="historyvaluename--value"></a>History(valueName) ⇒ `value[]`

Essa função retorna os valores históricos do espaço atual.

| Parâmetro  | Type                | Descrição  |
| ------ | ------------------- | ------------ |
| *valueName* | `string` | Nome do valor |

#### <a name="notifypayload"></a>Notify(payload)

Essa função envia uma notificação com a carga especificada.

| Parâmetro  | Type                | Descrição  |
| ------ | ------------------- | ------------ |
| *payload* | `string` | payload JSON a ser incluído na notificação |

### <a name="device"></a>Dispositivo

```JSON
{
  "id": "00000000-0000-0000-0000-000000000002",
  "name": "Device",
  "friendlyName": "Temperature Sensing Device",
  "description": "This device contains a sensor that captures temperature readings.",
  "type": "None",
  "subtype": "None",
  "typeId": 0,
  "subtypeId": 0,
  "hardwareId": "ABC123",
  "gatewayId": "ABC",
  "spaceId": "00000000-0000-0000-0000-000000000000"
}
```

### <a name="device-methods"></a>Métodos do dispositivo

#### <a name="parent--space"></a>Parent() ⇒ `space`

Essa função retorna o espaço pai do dispositivo atual.

#### <a name="childsensors--sensor"></a>ChildSensors() ⇒ `sensor[]`

Essa função retorna os sensores filhos do dispositivo atual.

#### <a name="extendedpropertypropertyname--extendedproperty"></a>ExtendedProperty(propertyName) ⇒ `extendedProperty`

Essa função retorna a propriedade estendida e o valor para o dispositivo atual.

| Parâmetro  | Type                | Descrição  |
| ------ | ------------------- | ------------ |
| *propertyName* | `string` | Nome da propriedade estendida |

#### <a name="notifypayload"></a>Notify(payload)

Essa função envia uma notificação com a carga especificada.

| Parâmetro  | Type                | Descrição  |
| ------ | ------------------- | ------------ |
| *payload* | `string` | payload JSON a ser incluído na notificação |

### <a name="sensor"></a>Sensor

```JSON
{
  "id": "00000000-0000-0000-0000-000000000003",
  "port": "30",
  "pollRate": 3600,
  "dataType": "Temperature",
  "dataSubtype": "None",
  "type": "Classic",
  "portType": "None",
  "dataUnitType": "FahrenheitTemperature",
  "spaceId": "00000000-0000-0000-0000-000000000000",
  "deviceId": "00000000-0000-0000-0000-000000000001",
  "portTypeId": 0,
  "dataUnitTypeId": 0,
  "dataTypeId": 0,
  "dataSubtypeId": 0,
  "typeId": 0  
}
```

### <a name="sensor-methods"></a>Métodos do sensor

#### <a name="space--space"></a>Space() ⇒ `space`

Essa função retorna o espaço pai do sensor atual.

#### <a name="device--device"></a>Device() ⇒ `device`

Essa função retorna o dispositivo pai do sensor atual.

#### <a name="extendedpropertypropertyname--extendedproperty"></a>ExtendedProperty(propertyName) ⇒ `extendedProperty`

Essa função retorna a propriedade estendida e o valor para o sensor atual.

| Parâmetro  | Type                | Descrição  |
| ------ | ------------------- | ------------ |
| *propertyName* | `string` | Nome da propriedade estendida |

#### <a name="value--value"></a>Value() ⇒ `value`

Essa função retorna o valor do sensor atual.

#### <a name="history--value"></a>History() ⇒ `value[]`

Esta função retorna os valores históricos do sensor atual.

#### <a name="notifypayload"></a>Notify(payload)

Essa função envia uma notificação com a carga especificada.

| Parâmetro  | Type                | Descrição  |
| ------ | ------------------- | ------------ |
| *payload* | `string` | payload JSON a ser incluído na notificação |

### <a name="value"></a>Valor

```JSON
{
  "dataType": "Temperature",
  "value": "70",
  "createdTime": ""
}
```

### <a name="extended-property"></a>Propriedade estendida

```JSON
{
  "name": "OccupancyStatus",
  "value": "Occupied"
}
```

## <a name="next-steps"></a>Próximas etapas

- Aprenda sobre as [funções definidas pelo usuário em Gêmeos Digitais do Azure](./concepts-user-defined-functions.md).

- Aprenda [como criar funções definidas pelo usuário](./how-to-user-defined-functions.md).

- Aprenda [como depurar funções definidas pelo usuário](./how-to-diagnose-user-defined-functions.md).

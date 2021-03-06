---
title: Variáveis de ambiente do Detector de Anomalias
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: mrbullwinkle
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 06/30/2020
ms.author: mbullwin
ms.openlocfilehash: 74e5373ebafbd39748f8b002069eaa6d732be069
ms.sourcegitcommit: 2c586a0fbec6968205f3dc2af20e89e01f1b74b5
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/14/2020
ms.locfileid: "92018417"
---
### <a name="create-an-environment-variable"></a>Criar uma variável de ambiente

>[!NOTE]
> Os pontos de extremidade para recursos que não são de avaliação criados após 1º de julho de 2019 usam o formato de subdomínio personalizado mostrado abaixo. Para saber mais e para obter uma lista completa de pontos de extremidade regionais, confira [Nomes de subdomínio personalizados para Serviços Cognitivos](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-custom-subdomains). 

Usando a chave e o ponto de extremidade do recurso criado, crie duas variáveis de ambiente para autenticação:

* `ANOMALY_DETECTOR_KEY` – a chave de recurso para autenticar as solicitações.
* `ANOMALY_DETECTOR_ENDPOINT` – o ponto de extremidade do recurso para enviar solicitações de API. Ele terá esta aparência: 
  * `https://<your-custom-subdomain>.api.cognitive.microsoft.com` 

Use as instruções para seu sistema operacional.

#### <a name="windows"></a>[Windows](#tab/windows)

```console
setx ANOMALY_DETECTOR_KEY <replace-with-your-anomaly-detector-key>
setx ANOMALY_DETECTOR_ENDPOINT <replace-with-your-anomaly-detector-endpoint>
```

Depois de adicionar a variável de ambiente, reinicie a janela do console.

#### <a name="linux"></a>[Linux](#tab/linux)

```bash
export ANOMALY_DETECTOR_KEY=<replace-with-your-anomaly-detector-key>
export ANOMALY_DETECTOR_ENDPOINT=<replace-with-your-anomaly-detector-endpoint>
```

Depois de adicionar a variável de ambiente, execute `source ~/.bashrc` a partir da janela de console para que as alterações entrem em vigor.

#### <a name="macos"></a>[macOS](#tab/unix)

Edite seu `.bash_profile` e adicione a variável de ambiente:

```bash
export ANOMALY_DETECTOR_KEY=<replace-with-your-anomaly-detector-key>
export ANOMALY_DETECTOR_ENDPOINT=<replace-with-your-anomaly-detector-endpoint>
```

Depois de adicionar a variável de ambiente, execute `source .bash_profile` a partir da janela de console para que as alterações entrem em vigor.

***
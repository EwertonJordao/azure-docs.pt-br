---
title: Soluções para erros comuns e problemas em cenários B2B
description: Encontre soluções para erros e problemas comuns ao solucionar problemas de cenários B2B em aplicativos lógicos do Azure
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, logicappspm
ms.topic: article
ms.date: 06/02/2017
ms.openlocfilehash: 38e281ce3d8117bff719b1bb572f09acbbb89669
ms.sourcegitcommit: ff9688050000593146b509a5da18fbf64e24fbeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/06/2020
ms.locfileid: "75666679"
---
# <a name="b2b-errors-and-solutions-for-azure-logic-apps"></a>Erros e soluções B2B para os Aplicativos Lógicos do Azure

Este artigo ajuda você a solucionar problemas de erros que podem ocorrer em cenários de Aplicativos Lógicos B2B e sugere ações adequadas para corrigir esses erros.

## <a name="agreement-resolution"></a>Resolução do contrato

### <a name="no-agreement-found"></a>Nenhum contrato encontrado 

|   |   |  
|---|---|
| Descrição do erro | Não foi encontrado nenhum contrato com os Parâmetros de Resolução de Contrato. | 
| Ação do usuário | O contrato deve ser adicionado à conta de integração com identidades comerciais acordadas. </br>As identidades comerciais devem corresponder às IDs de mensagens de entrada. |  
|   |   |

### <a name="no-agreement-found-with-identities"></a>Não foi encontrado nenhum contrato com identidades

|   |   | 
|---|---|
| Descrição do erro | Não foi encontrado nenhum contrato com as identidades: 'AS2Identity'::'Partner1' e 'AS2Identity'::'Partner3' | 
| Ação do usuário | AS2-From ou AS2-To inválido configurado para o contrato. </br>Corrigir os cabeçalhos "AS2-From" ou "AS2-To" da mensagem AS2 ou o contrato para corresponder as IDs do AS2 nos cabeçalhos de mensagem AS2 com as configurações de contrato. |
|   |   |     

## <a name="as2"></a>AS2

### <a name="missing-as2-message-headers"></a>Cabeçalhos de mensagem AS2 ausentes  

|   |   |  
|---|---|
| Descrição do erro | Cabeçalhos de AS2 inválidos. Um dos cabeçalhos "AS2-To" ou "AS2-From" está vazio. | 
| Ação do usuário | Foi recebida uma mensagem AS2 que não continha o cabeçalho AS2-From ou AS2-To ou ambos. </br> Verifique os cabeçalhos AS2-From e AS2-To da mensagem AS2 e corrija-os com base na configuração do contrato. |
|  |  | 

### <a name="missing-as2-message-body-and-headers"></a>Cabeçalhos e corpo da mensagem AS2 ausentes    

|   |   |  
|---|---|
| Descrição do erro | O conteúdo da solicitação é nulo ou vazio. | 
| Ação do usuário | Foi recebida uma mensagem AS2 que não continha o corpo da mensagem. |
|  |  | 

### <a name="as2-message-decryption-failure"></a>Falha na descriptografia mensagem AS2

|   |   | 
|---|---|
| Descrição do erro |  [processado/Erro: falha na descriptografia] | 
| Ação do usuário | Adicione @base64ToBinary a AS2Message antes de enviar ao parceiro. |
|||

Por exemplo:

```json
"HTTP": {
   "inputs": {
   "body": "@base64ToBinary(body('Encode_to_AS2_message')?['AS2Message']?['Content'])",
   "headers": "@body('Encode_to_AS2_message')?['AS2Message']?['OutboundHeaders']",
   "method": "POST",
   "uri": "xxxxx.xxx"
},
``` 

### <a name="mdn-decryption-failure"></a>Falha na descriptografia do MDN

|   |   | 
|---|---|
| Descrição do erro |  [processado/Erro: falha na descriptografia] | 
| Ação do usuário | Adicione @base64ToBinary ao MDN antes de enviar ao parceiro. | 
|||

Por exemplo:

```json
"Response": {
   "inputs": {
   "body": "@base64ToBinary(body('Decode_AS2_message')?['OutgoingMDN']?['Content'])",
   "headers": "@body('Decode_AS2_message')?['OutgoingMDN']?['OutboundHeaders']",
   "statusCode": 200
},               
``` 

### <a name="missing-signing-certificate"></a>Certificado de assinatura ausente

|   |   |  
|---|---|
| Descrição do erro| O certificado de autenticação não foi configurado para a parte do AS2. </br>AS2-From: partner1 AS2-To: partner2 | 
| Ação do usuário | Definir as configurações do contrato AS2 com o certificado correto para assinatura. |
|  |  | 

## <a name="x12-and-edifact"></a>X12 e EDIFACT

### <a name="leading-or-trailing-space-found"></a>Espaço à esquerda ou à direita encontrado    
    
|   |   | 
|---|---|
| Descrição do erro | Erro encontrado durante a análise. O conjunto de transações EDIFACT com a ID '123456' contido no intercâmbio (sem grupo) com a ID '987654', com a ID de remetente 'Partner1' e a ID do receptor 'Partner2', está sendo suspenso com os seguintes erros: <p>"Separador à direita/esquerda encontrado" |
| Ação do usuário | As configurações de contrato a serem configuradas para permitir o espaço à esquerda e à direita. </br>Edite as configurações do contrato para permitir o espaço à esquerda e à direita. |
|   |   |

![permitir espaço](./media/logic-apps-enterprise-integration-b2b-list-errors-solutions/leadingandtrailing.png)

### <a name="duplicate-check-has-enabled-in-the-agreement"></a>A verificação dupla foi habilitada no contrato

|   |   | 
|---|---| 
| Descrição do erro | Duplicar Número de Controle |
| Ação do usuário | Esse erro indica que a mensagem recebida tem números de controle duplicados. </br>Corrija o número de controle e reenvie a mensagem. |
|   |   |

### <a name="missing-schema-in-the-agreement"></a>Esquema ausente no contrato

|   |   | 
|---|---| 
| Descrição do erro | Erro encontrado durante a análise. O conjunto de transações do X12 com ID '564220001' contido no grupo funcional com ID '56422', em intercâmbio com a ID '000056422', com ID do remente '12345678' e ID do receptor '87654321', está sendo suspenso com os seguintes erros: <p>"A mensagem tem um tipo de documento desconhecido e não foi resolvida para nenhum dos esquemas existentes configurados no contrato" |
| Ação do usuário | Configure o esquema nas configurações do contrato.  |
|   |   |

### <a name="incorrect-schema-in-the-agreement"></a>Esquema incorreto no contrato

|   |   | 
|---|---| 
| Descrição do erro | A mensagem tem um tipo de documento desconhecido e não foi resolvida para nenhum dos esquemas existentes configurados no contrato. |
| Ação do usuário | Configure o esquema correto nas configurações do contrato. |
|   |   |

## <a name="flat-file"></a>Arquivo simples

### <a name="input-message-with-no-body"></a>Mensagem de entrada sem corpo

|   |   | 
|---|---|
| Descrição do erro | InvalidTemplate. Não é possível processar as expressões da linguagem do modelo nas entradas da ação “Flat_File_Decoding” na linha “1” e coluna “1902”: “A propriedade obrigatória “content” espera um valor, mas recebeu nulo. Caminho “”.”. |
| Ação do usuário | Esse erro indica que a mensagem de entrada não contém um corpo. |
|   |   | 


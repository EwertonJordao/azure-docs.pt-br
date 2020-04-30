---
title: O que há de novo Reconhecimento vocal (LUIS)
description: Este artigo é atualizado regularmente com notícias sobre os API de Reconhecimento Vocal de serviços cognitivas do Azure.
ms.topic: conceptual
ms.date: 02/11/2020
ms.openlocfilehash: 8e3b6f729f69866cdb797cdfd1887de4f5e2be05
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/29/2020
ms.locfileid: "82097720"
---
# <a name="whats-new-in-language-understanding"></a>O que há de novo no Reconhecimento vocal

Conheça o que há de novo no serviço. Esses itens incluem notas de versão, vídeos, Postagens de blog e outros tipos de informações. Marque esta página para manter-se atualizado quanto ao serviço.

## <a name="release-notes"></a>Notas de versão

### <a name="march-2020"></a>Março de 2020

* O TLS 1,2 agora é imposto para todas as solicitações HTTP para esse serviço. Para obter mais informações, consulte [segurança de serviços cognitivas do Azure](../cognitive-services-security.md).

### <a name="november-4-2019---ignite"></a>4 de novembro de 2019-Ignite

* Vídeo- [modelos NLU (reconhecimento vocal naturais avançados) usando o Luis e os serviços cognitivas do Azure | BRK2188](https://www.youtube.com/watch?v=JdJEV2jV0_Y)

* Produtividade do desenvolvedor aprimorada
    * Disponibilidade geral do nosso [ponto de extremidade de previsão v3](luis-migration-api-v3.md).
    * Capacidade de importar e exportar aplicativos com o formato. Lu ([LUDown](https://github.com/microsoft/botbuilder-tools/tree/master/packages/Ludown)). Isso abre o caminho para um processo de CI/CD eficaz.
* Expansão de idioma
    * [Árabe e híndi](luis-language-support.md) em visualização pública.
* Modelos predefinidos
    * Os [domínios predefinidos](luis-reference-prebuilt-domains.md) agora estão disponíveis para o público geral (GA)
    * Não há suporte para [entidades predefinidas](luis-reference-prebuilt-entities.md#japanese-entity-support) japonesas – idade, moeda, número e porcentagem na v3.
    * [Entidades predefinidas](luis-reference-prebuilt-entities.md#italian-entity-support) em italiano-idade, moeda, dimensão, número e resolução de porcentagem alteradas da v2.
* Experiência do usuário aprimorada no [portal do preview.Luis.ai](https://preview.luis.ai) – experiência de rotulação remodelada para habilitar a criação e a depuração de modelos complexos. Experimente os tutoriais do portal de visualização:
    * [Somente tentativas](tutorial-intents-only.md)
    * [Entidade aprendida por máquina decomposta](tutorial-machine-learned-entity.md)
* Recursos de reconhecimento de idioma avançado- [criando modelos de linguagem sofisticados](luis-concept-entity-types.md) com menos esforço.
* Defina os recursos de aprendizado de máquina no nível do modelo e habilite os modelos a serem usados como sinais para outros modelos, por exemplo, usando entidades como recursos para intenções e outras entidades.
* Novos [limites](luis-limits.md) expandidos-máximo mais alto para listas de frases e expressões totais, novo modelo como limites de recursos
* Extraia informações de texto no formato de estrutura de hierarquia profunda, tornando os aplicativos de conversa mais poderosos.

    ![imagem de entidade aprendida por computador](./media/whats-new/deep-entity-extraction-example.png)

### <a name="september-3-2019"></a>3 de setembro de 2019

* Recurso de criação do Azure – [migrar agora](luis-migration-authoring.md).
    * 500 aplicativos por recurso do Azure
    * 100 versões por aplicativo
* Suporte a turco para entidades predefinidas
* Suporte italiano para datetimeV2

### <a name="july-23-2019"></a>23 de julho de 2019

* Atualizar os [reconhecedores-texto](https://github.com/microsoft/Recognizers-Text/releases/tag/dotnet-v1.2.3) para 1.2.3
    * Reconhecedores de idade, temperatura, dimensão e moeda em italiano.
    * Melhoria no reconhecimento de feriados em inglês para calcular corretamente as datas baseadas em graças.
    * Melhorias no DateTime em francês para reduzir os falsos positivos de entidades não atualizadas e não de tempo.
    * Suporte para calendário/escola/ano fiscal e acrônimos em DateRange em inglês.
    * Reconhecimento de PhoneNumber aprimorado em chinês e japonês.
    * Suporte aprimorado para NumberRange em inglês.
    * Melhorias de desempenho.

### <a name="june-24-2019"></a>24 de junho de 2019

* [OrdinalV2 entidade predefinida](luis-reference-prebuilt-ordinal-v2.md) para dar suporte a pedidos como, por exemplo, avançar, anterior e último. Somente cultura em inglês.

### <a name="may-6-2019---build-conference"></a>6 de maio, 2019-conferência//Build

Os seguintes recursos foram lançados na conferência Build 2019:

* [Visualização do guia de migração da API v3](luis-migration-api-v3.md)
* [Painel analítico aprimorado](luis-how-to-use-dashboard.md)
* [Domínios predefinidos aprimorados](luis-reference-prebuilt-domains.md)
* [Entidades de lista dinâmica](schema-change-prediction-runtime.md#dynamic-lists-passed-in-at-prediction-time)
* [Entidades externas](schema-change-prediction-runtime.md#external-entities-passed-in-at-prediction-time)

## <a name="blogs"></a>Blogs

[Ambas as estruturas](https://blog.botframework.com/)

## <a name="videos"></a>vídeos

### <a name="2019-ignite-videos"></a>vídeos de 2019 Ignite

[Modelos NLU (Advanced natural Reconhecimento vocal) usando LUIS e serviços cognitivas do Azure | BRK2188](https://www.youtube.com/watch?v=JdJEV2jV0_Y)

### <a name="2019-build-videos"></a>vídeos de Build 2019

[Como usar o AI de conversa do Azure para dimensionar sua empresa para a próxima geração](https://www.youtube.com/watch?v=_k97jd-csuk&feature=youtu.be)

## <a name="service-updates"></a>Atualizações de serviço

[Comunicados de atualização do Azure para os Serviços Cognitivos](https://azure.microsoft.com/updates/?product=cognitive-services)

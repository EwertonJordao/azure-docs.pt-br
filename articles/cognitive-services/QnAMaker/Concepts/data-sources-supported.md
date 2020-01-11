---
title: Fontes de dados com suporte – QnA Maker
titleSuffix: Azure Cognitive Services
description: QnA Maker extrai automaticamente os pares de resposta de pergunta armazenados como páginas da Web, arquivos PDF ou arquivos de documento MS Word ou arquivos de conteúdo QnA estruturados.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 01/09/2020
ms.author: diberry
ms.openlocfilehash: 2978ffa68814d176ea1caf485e7e4f1ba72f2597
ms.sourcegitcommit: 12a26f6682bfd1e264268b5d866547358728cd9a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/10/2020
ms.locfileid: "75867569"
---
# <a name="data-sources-for-qna-maker-content"></a>Fontes de dados para conteúdo do QnA Maker

O QnA Maker extrai automaticamente pares de pergunta-resposta do conteúdo semiestruturado, como perguntas frequentes, manuais de produto, diretrizes, documentos de suporte e políticas armazenadas como arquivos de documentos do Microsoft Word, arquivos PDF ou páginas da Web. O conteúdo também pode ser adicionado à base de conhecimento dos arquivos de conteúdo QnA estruturados.

<a name="data-types"></a>

## <a name="file-and-url-data-types"></a>Tipos de dados de arquivo e URL

A tabela a seguir resume os tipos de conteúdo e formatos de arquivo com suporte no QnA Maker.

|Tipo de Fonte|Tipo de conteúdo| Exemplos|
|--|--|--|
|URL|Perguntas frequentes<br> (simples, com seções ou com uma página inicial de tópicos)<br>Páginas de suporte <br> (artigos de instrução de uma página, artigos de solução de problemas etc.)|[Perguntas frequentes simples](https://docs.microsoft.com/azure/cognitive-services/qnamaker/faqs), <br>[Perguntas frequentes com links](https://www.microsoft.com/en-us/software-download/faq),<br> [Perguntas frequentes com home page de tópicos](https://www.microsoft.com/Licensing/servicecenter/Help/Faq.aspx)<br>[Artigo de suporte](https://docs.microsoft.com/azure/cognitive-services/qnamaker/concepts/best-practices)|
|PDF/DOC|Perguntas frequentes,<br> Manual do Produto,<br> Folhetos,<br> Papel,<br> Política de folheto,<br> Guia de suporte,<br> QnA estruturado,<br> etc.|[QnA.doc estruturado](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/structured.docx),<br> [Exemplo produto Manual.pdf](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/product-manual.pdf),<br> [Exemplo semi-estruturado.doc](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/semi-structured.docx),<br> [Exemplo White Paper. pdf](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/white-paper.pdf),<br>[Exemplo de multi-Turn. docx](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/multi-turn.docx)|
|\* Excel|Arquivo QnA estruturado<br> (incluindo suporte RTF, HTML)|[Exemplo de QnA FAQ.xls](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/QnA%20Maker%20Sample%20FAQ.xlsx)|
|\* TXT/TSV|Arquivo QnA estruturado|[Exemplo de chit-chat.tsv](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/Scenario_Responses_Friendly.tsv)|

### <a name="import-and-export-knowledge-base"></a>Importar e exportar base de dados de conhecimento

Os **arquivos TSV e XLS**, das bases de dados de conhecimento exportadas, só podem ser usados pela importação dos arquivos da página **configurações** no portal de QnA Maker. Eles não podem ser usados como fontes de dados durante a criação da base de conhecimento ou no recurso **+ Adicionar arquivo** ou **+ Adicionar URL** na página **configurações** .

## <a name="data-source-locations"></a>Locais de origem de dados

Os locais de fonte de dados são **URLs ou arquivos públicos**, que não exigem autenticação.

Se você precisar de autenticação para sua fonte de dados, considere os seguintes métodos para obter esses dados em QnA Maker:

* [Baixar o arquivo manualmente](#download-file-from-authenticated-data-source-location) e importar para o QnA Maker
* Importar arquivo para o [local do SharePoint](#import-file-from-authenticated-sharepoint) autenticado

### <a name="download-file-from-authenticated-data-source-location"></a>Baixar arquivo do local da fonte de dados autenticada

Se você tiver um arquivo autenticado (não em um local do SharePoint autenticado) ou URL, uma opção alternativa é baixar o arquivo do site autenticado para o computador local e, em seguida, adicionar o arquivo do seu computador local à base de dados de conhecimento.

### <a name="import-file-from-authenticated-sharepoint"></a>Importar arquivo do SharePoint autenticado

Os [locais de fonte de dados do SharePoint](../How-to/add-sharepoint-datasources.md) têm permissão para fornecer **arquivos**autenticados. Os recursos do SharePoint devem ser arquivos, não páginas da Web. Se a URL terminar com uma extensão da Web, como **. ASPX**, ele não será importado para o QnA Maker do SharePoint.


## <a name="faq-urls"></a>URLs de perguntas frequentes

O QnA Maker pode dar suporte a páginas da Web de perguntas frequentes de três maneiras diferentes: páginas de Perguntas Frequentes Simples, páginas de Perguntas Frequentes com links, páginas de Perguntas Frequentes com uma Página Inicial de Tópicos.

### <a name="plain-faq-pages"></a>Páginas de perguntas frequentes simples

Este é o tipo mais comum de página de perguntas frequentes em que as respostas a seguem imediatamente às perguntas na mesma página.

Abaixo está um exemplo de uma página de perguntas frequentes simples:

![Exemplo de página de perguntas frequentes simples para uma base de dados de conhecimento](../media/qnamaker-concepts-datasources/plain-faq.png)


### <a name="faq-pages-with-links"></a>Páginas de perguntas frequentes com links

Nesse tipo de página de perguntas frequentes, as perguntas são agregadas e vinculadas a respostas em seções diferentes da mesma página ou em páginas diferentes.

Abaixo está um exemplo de uma página de perguntas frequentes com links nas seções que estão na mesma página:

 ![Exemplo de página de perguntas frequentes com link de seção para uma base de dados de conhecimento](../media/qnamaker-concepts-datasources/sectionlink-faq.png)


### <a name="faq-pages-with-a-topics-homepage"></a>Páginas de perguntas frequentes com uma página inicial de Tópicos

Esse tipo de perguntas frequentes tem uma página inicial com Tópicos, em que cada tópico é um link para as QnAs relevantes em uma página diferente. Aqui, o QnA Maker rastreia todas as páginas vinculadas para extrair as perguntas e respostas correspondentes.

Abaixo está um exemplo de uma página de perguntas frequentes em que uma página inicial de tópicos tem links para seções de perguntas frequentes em diferentes páginas.

 ![Exemplo de página de perguntas frequentes com link profundo para uma base de dados de conhecimento](../media/qnamaker-concepts-datasources/topics-faq.png)


### <a name="support-urls"></a>Suporte a Urls

O QnA Maker pode processar páginas da web de suporte semiestruturadas, como artigos da web que descrevem como executar uma tarefa específica, como diagnosticar e resolver um problema específico e quais são as práticas recomendadas para um determinado processo. A extração funciona melhor em documentos que têm uma estrutura clara com cabeçalhos hierárquicos.

> [!NOTE]
> Extração para artigos de suporte é um recurso novo e está nos estágios iniciais. Funciona melhor para páginas simples, que também são estruturadas e não contêm cabeçalhos/rodapés complexos.

![O QnA Maker dá suporte à extração de páginas web semi-estruturadas em que a estrutura limpa é apresentada com cabeçalhos hierárquicos](../media/qnamaker-concepts-datasources/support-web-pages-with-heirarchical-structure.png)


## <a name="pdf-doc-files"></a>Arquivos PDF/DOC

O QnA Maker pode processar o conteúdo semiestruturado em um arquivo PDF ou documento e convertê-la em QnAs. Um bom arquivo que pode ser extraído também é um arquivo em que o conteúdo está organizado em alguma forma estruturada e é representado em seções bem definidas. As seções adicionais podem ser divididas em subseções ou subtópicos. A extração funciona melhor em documentos que têm uma estrutura clara com cabeçalhos hierárquicos.

QnA Maker identifica seções e subseções e relações no arquivo com base em pistas visuais, como tamanho da fonte, estilo da fonte, numeração, cores, etc. Arquivos de documento ou PDF semiestruturados podem ser manuais, perguntas frequentes, diretrizes, políticas, folhetos, panfletos e muitos outros tipos de arquivos. Abaixo estão alguns tipos de exemplo desses arquivos.

### <a name="product-manuals"></a>Manuais de produtos

Normalmente, um manual é o material de diretrizes que acompanha um produto. Ele ajuda o usuário a configurar, usar, manter e solucionar problemas do produto. Quando o QnA Maker processa um manual, ele extrai os títulos e subtítulos como perguntas e o conteúdo subsequente como respostas. Veja um exemplo [aqui](https://download.microsoft.com/download/2/9/B/29B20383-302C-4517-A006-B0186F04BE28/surface-pro-4-user-guide-EN.pdf).

Abaixo está um exemplo de um manual com uma página de índice e conteúdo hierárquico

 ![Exemplo de manual de produto para uma base de dados de conhecimento](../media/qnamaker-concepts-datasources/product-manual.png)

> [!NOTE]
> A extração funciona melhor em manuais com uma tabela de conteúdo e/ou uma página de índice e uma estrutura clara com cabeçalhos hierárquicos.

### <a name="brochures-guidelines-papers-and-other-files"></a>Brochuras, diretrizes, documentos e outros arquivos

Muitos outros tipos de documentos também podem ser processados para gerar pares de QA, contanto que tenham uma estrutura e um layout claros. Eles incluem: folhetos, diretrizes, relatórios, White papers, documentos científicos, políticas, livros, etc. Veja um exemplo [aqui](https://qnamakerstore.blob.core.windows.net/qnamakerdata/docs/Manage%20Azure%20Blob%20Storage.docx).

Abaixo está um exemplo de um documento semiestruturado sem um índice:

 ![Documento semiestruturado de armazenamento de Blobs do Azure](../media/qnamaker-concepts-datasources/semi-structured-doc.png)

### <a name="structured-qna-document"></a>Documento de QnA Estruturado

O formato para Pergunta-Respostas estruturas em arquivos DOC é na forma de Perguntas e Respostas alternadas por linha, uma pergunta por linha seguida pela respectiva resposta na linha seguinte, conforme mostrado abaixo:

```text
Question1

Answer1

Question2

Answer2
```

Abaixo está um exemplo de um documento do word de QnA estruturado:

 ![Exemplo de documento do QnA estruturado em uma base de dados de conhecimento](../media/qnamaker-concepts-datasources/structured-qna-doc.png)

## <a name="structured-txt-tsv-and-xls-files"></a>Arquivos *TXT*, *TSV* e *XLS* Estruturados

QnAs na forma de arquivos *.txt*, *.tsv* ou *.xls* estruturados também podem ser carregadas para o QnA Maker para criar ou ampliar uma base de conhecimento.  Podem ser texto sem formatação ou ter conteúdo em RTF ou HTML.

| Pergunta  | Resposta  | Metadados (1 chave: 1 valor) |
|-----------|---------|-------------------------|
| Pergunta1 | Resposta1 | <code>Key1:Value1 &#124; Key2:Value2</code> |
| Pergunta2 | Resposta2 |      `Key:Value`           |

As colunas adicionais no arquivo de origem são ignoradas.

### <a name="example-of-structured-excel-file"></a>Exemplo de arquivo do Excel estruturado

Abaixo está um exemplo de arquivo *.xls* de QnA estruturada, com conteúdo HTML:

 ![Exemplo do QnA estruturado em uma base de dados de conhecimento](../media/qnamaker-concepts-datasources/structured-qna-xls.png)

### <a name="example-of-alternate-questions-for-single-answer-in-excel-file"></a>Exemplo de perguntas alternativas para uma única resposta no arquivo do Excel

Veja abaixo um exemplo de um arquivo QnA *. xls* estruturado, com várias perguntas alternativas para uma única resposta:

 ![Exemplo de perguntas alternativas para uma única resposta no arquivo do Excel](../media/qnamaker-concepts-datasources/xls-alternate-question-example.png)

Depois que o arquivo for importado, o par de perguntas e respostas estará na base de dados de conhecimento, conforme mostrado abaixo:

 ![Captura de tela de perguntas alternativas para uma única resposta importada na base de dados de conhecimento](../media/qnamaker-concepts-datasources/xls-alternate-question-example-after-import.png)

## <a name="structured-data-format-through-import"></a>Formato de dados estruturados por meio de importação

Importar uma base de dados de conhecimento substitui o conteúdo da base de dados de conhecimento existente. A importação requer um arquivo .tsv estruturado que contenha informações de fonte de dados. Essas informações ajudam o QnA Maker a agrupar os pares de resposta de pergunta e atribuí-los a uma fonte de dados específico.

| Pergunta  | Resposta  | Origem| Metadados (1 chave: 1 valor) |
|-----------|---------|----|---------------------|
| Pergunta1 | Resposta1 | Url1 | <code>Key1:Value1 &#124; Key2:Value2</code> |
| Pergunta2 | Resposta2 | Editorial|    `Key:Value`       |

## <a name="editorially-add-to-knowledge-base"></a>Adicionar de modo editorial à base de dados de conhecimento

Se não tiver conteúdo pré-existente para preencher a base de dados de conhecimento, você poderá adicionar QnAs editorialmente na Base de Dados de Conhecimento do QnA Maker. Saiba como atualizar sua base de dados de conhecimento [aqui](../How-To/edit-knowledge-base.md).

<a href="#formatting-considerations"></a>

## <a name="formatting-considerations"></a>Considerações de formatação

Depois de importar um arquivo ou URL, QnA Maker converte e armazena seu conteúdo no [formato de redução](https://en.wikipedia.org/wiki/Markdown). O processo de conversão adiciona novas linhas no texto, como `\n\n`. Um conhecimento do formato de redução ajuda você a entender o conteúdo convertido e gerenciar seu conteúdo da base de dados de conhecimento.

Se você adicionar ou editar seu conteúdo diretamente na sua base de dados de conhecimento, use a **formatação de redução** para criar conteúdo de Rich Text ou alterar o conteúdo do formato de redução que já está na resposta. QnA Maker dá suporte a grande parte do formato de redução para trazer recursos de Rich Text para seu conteúdo. No entanto, o aplicativo cliente, como um bot de chat, pode não dar suporte ao mesmo conjunto de formatos de redução. É importante testar a exibição de respostas do aplicativo cliente.

Consulte os exemplos de redução fornecidos na [referência de redução de QnA Maker](../reference-markdown-format.md) para obter mais informações.

## <a name="editing-your-knowledge-base-locally"></a>Como editar sua base de dados de conhecimento localmente

Após criar uma base de conhecimento, é recomendável que você faça as edições no texto da base de conhecimento no [portal QnA Maker](https://qnamaker.ai), em vez de exportar e reimportar por meio de arquivos locais. No entanto, pode haver ocasiões em que você precisa editar uma base de conhecimento localmente.

Exporte a base de conhecimento na página **Configurações** e, em seguida, edite-a com o Microsoft Excel. Se você optar por usar outro aplicativo para editar o arquivo TSV exportado, o aplicativo pode introduzir erros de sintaxe porque não é totalmente compatível com TSV. Em geral, os arquivos TSV do Microsoft Excel não introduzem nenhum erro de formatação.

Depois de concluir as edições, reimporte o arquivo TSV na página **Configurações**. Isso substitui totalmente a base de conhecimento atual pela base de conhecimento importada.

## <a name="testing-your-markdown"></a>Testar seu Markdown

Use o tutorial **[CommonMark](https://commonmark.org/help/tutorial/index.html)** para validar seu Markdown. O tutorial tem um recurso **Experimentar** para validação rápida de copiar/colar.

## <a name="version-control-for-data-in-your-knowledge-base"></a>Controle de versão para dados em sua base de conhecimento

O controle de versão para dados é fornecido por meio do [recurso de importação/exportação](development-lifecycle-knowledge-base.md#version-control-of-a-knowledge-base) na página **configurações** .

## <a name="next-steps"></a>Próximos passos

> [!div class="nextstepaction"]
> [Configurar um serviço do QnA Maker](../How-To/set-up-qnamaker-service-azure.md)

## <a name="see-also"></a>Consulte também

[Visão geral do QnA Maker](../Overview/overview.md)

---
title: Obtenha informações usando o centro de backup
description: Saiba como analisar tendências históricas e obter informações mais aprofundadas sobre seus backups com o centro de backup.
ms.topic: conceptual
ms.date: 09/01/2020
ms.openlocfilehash: 5964f285089feea721a0b452efed884e905b89cc
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/09/2020
ms.locfileid: "90993894"
---
# <a name="obtain-insights-using-backup-center"></a>Obtenha informações usando o centro de backup

Para analisar tendências históricas e obter informações mais aprofundadas sobre seus backups, o centro de backup fornece uma interface para [fazer backup de relatórios](configure-reports.md), que usa [logs de Azure monitor](https://docs.microsoft.com/azure/azure-monitor/platform/data-platform-logs) e [pastas de trabalho do Azure](https://docs.microsoft.com/azure/azure-monitor/platform/workbooks-overview). Os relatórios de backup oferecem os seguintes recursos:

- Alocação e previsão do armazenamento em nuvem consumido.

- Auditoria de backups e restaurações.

- Identificação das principais tendências em diferentes níveis de granularidade.

- Obter visibilidade e informações sobre as oportunidades de otimização de custos para seus backups.

## <a name="supported-scenarios"></a>Cenários com suporte

- Os relatórios de backup não estão disponíveis no momento para o backup do servidor do banco de dados do Azure para PostgreSQL.

- Consulte a [matriz de suporte](backup-center-support-matrix.md) para obter uma lista detalhada de cenários com e sem suporte.

## <a name="get-started"></a>Introdução

### <a name="configure-your-vaults-to-send-data-to-a-log-analytics-workspace"></a>Configurar seus cofres para enviar dados para um espaço de trabalho Log Analytics

[Saiba como definir as configurações de diagnóstico em escala para seus cofres](https://docs.microsoft.com/azure/backup/configure-reports#get-started)

### <a name="view-backup-reports-in-the-backup-center-portal"></a>Exibir relatórios de backup no portal do centro de backup

A seleção do item de menu **relatórios de backup** no centro de backup abre os relatórios. Escolha um ou mais espaços de trabalho do Log Analytics para exibir e analisar informações de chave em seus backups.

![Relatórios de backup no centro de backup](./media/backup-center-obtain-insights/backup-center-backup-reports.png)

A seguir estão as exibições disponíveis:

1. **Resumo** – Use esta guia para obter uma visão geral de alto nível do seu espaço de backup. [Saiba mais](https://docs.microsoft.com/azure/backup/configure-reports#summary)

1. **Itens de backup** -Use esta guia para ver informações e tendências sobre o armazenamento em nuvem consumidos em um nível de item de backup. [Saiba mais](https://docs.microsoft.com/azure/backup/configure-reports#backup-items)

1. **Uso** -Use essa guia para exibir os parâmetros de cobrança de chave para seus backups. [Saiba mais](https://docs.microsoft.com/azure/backup/configure-reports#usage)

1. **Trabalhos** – Use essa guia para exibir tendências de longa execução em trabalhos, como o número de trabalhos com falha por dia e as principais causas de falha do trabalho. [Saiba mais](https://docs.microsoft.com/azure/backup/configure-reports#jobs)

1. **Políticas** – Use essa guia para exibir informações sobre todas as suas políticas ativas, como o número de itens associados e o armazenamento em nuvem total consumido por itens de backup em uma determinada política. [Saiba mais](https://docs.microsoft.com/azure/backup/configure-reports#policies)

1. **Otimizar** – Use essa guia para obter visibilidade de possíveis oportunidades de otimização de custos para seus backups. [Saiba mais](https://docs.microsoft.com/azure/backup/configure-reports#optimize)

## <a name="next-steps"></a>Próximas etapas

- [Monitorar e operar backups](backup-center-monitor-operate.md)
- [Governar seu espaço de backup](backup-center-govern-environment.md)
- [Executar ações usando o centro de backup](backup-center-actions.md)

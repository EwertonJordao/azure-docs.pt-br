---
title: Limpar o trabalho do Azure Stream Analytics
description: Este artigo mostra diferentes métodos para excluir trabalhos do Azure Stream Analytics.
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: how-to
ms.date: 06/21/2019
ms.custom: seodec18
ms.openlocfilehash: f078ab217a5c4911fbd132338883504897db899f
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/09/2020
ms.locfileid: "86044593"
---
# <a name="stop-or-delete-your-azure-stream-analytics-job"></a>Parar ou excluir seu trabalho de Azure Stream Analytics

Azure Stream Analytics trabalhos podem ser facilmente interrompidos ou excluídos por meio do portal do Azure, Azure PowerShell, SDK do Azure para .net ou API REST. Um trabalho de Stream Analytics não pode ser recuperado depois de ser excluído.

>[!NOTE] 
>Quando o trabalho do Azure Stream Analytics é interrompido, os dados persistem somente no armazenamento de entrada e saída, como no Hubs de Eventos ou no Banco de Dados SQL do Azure. Se for necessário para remover dados do Azure, lembre-se de seguir o processo de remoção para os recursos de entrada e saída do seu trabalho do Stream Analytics.

## <a name="stop-a-job-in-azure-portal"></a>Parar um trabalho no portal do Azure

Quando você interrompe um trabalho, os recursos são desprovisionados e o processamento de eventos é interrompido. Os encargos relacionados a esse trabalho também são interrompidos. No entanto, todas as suas configurações são mantidas e você pode reiniciar o trabalho mais tarde 

1. Entre no [portal do Azure](https://portal.azure.com). 

2. Localize o trabalho em execução do Stream Analytics e selecione-o.

3. Na página do trabalho no Stream Analytics, selecione **Parar** para interrompê-lo. 

   ![Interromper um trabalho do Azure Stream Analytics](./media/stream-analytics-clean-up-your-job/stop-stream-analytics-job.png)


## <a name="delete-a-job-in-azure-portal"></a>Excluir um trabalho no portal do Azure

>[!WARNING] 
>Um trabalho de Stream Analytics não pode ser recuperado depois de ser excluído.

1. Entre no portal do Azure. 

2. Localize o trabalho existente do Stream Analytics e selecione-o.

3. Na página do trabalho no Stream Analytics, selecione **Excluir** para excluí-lo. 

   ![Excluir um trabalho do Azure Stream Analytics](./media/stream-analytics-clean-up-your-job/delete-stream-analytics-job.png)


## <a name="stop-or-delete-a-job-using-powershell"></a>Parar ou excluir um trabalho usando o PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Para interromper um trabalho usando o PowerShell, use o cmdlet [Stop-AzStreamAnalyticsJob](https://docs.microsoft.com/powershell/module/az.streamanalytics/stop-azstreamanalyticsjob) . Para excluir um trabalho usando o PowerShell, use o cmdlet [Remove-AzStreamAnalyticsJob](https://docs.microsoft.com/powershell/module/az.streamanalytics/Remove-azStreamAnalyticsJob) .

## <a name="stop-or-delete-a-job-using-azure-sdk-for-net"></a>Parar ou excluir um trabalho usando o SDK do Azure para .NET

Para interromper um trabalho usando o SDK do Azure para .NET, use o método [StreamingJobsOperationsExtensions.BeginStop](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.streamanalytics.streamingjobsoperationsextensions.beginstop?view=azure-dotnet). Para excluir um trabalho usando o SDK do Azure para .NET, use o método [StreamingJobsOperationsExtensions.BeginDelete](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.streamanalytics.streamingjobsoperationsextensions.begindelete?view=azure-dotnet).

## <a name="stop-or-delete-a-job-using-rest-api"></a>Parar ou excluir um trabalho usando a API REST

Para interromper um trabalho usando a API REST, confira o método [Parar](https://docs.microsoft.com/rest/api/streamanalytics/stream-analytics-job#stop). Para excluir um trabalho usando a API REST, confira o método [Excluir](https://docs.microsoft.com/rest/api/streamanalytics/stream-analytics-job#delete).

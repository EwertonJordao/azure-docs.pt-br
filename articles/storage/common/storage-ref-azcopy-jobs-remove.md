---
title: remover trabalhos do azcopy | Microsoft Docs
description: Este artigo fornece informações de referência para o comando azcopy Jobs remove.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 10/16/2019
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: ba403c8d823b7ead0414521ebd51dc6f6601ccd8
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/27/2020
ms.locfileid: "74034168"
---
# <a name="azcopy-jobs-remove"></a>azcopy jobs remove

Remova todos os arquivos associados à ID de trabalho fornecida.

> [!NOTE] 
> Você pode personalizar o local onde os arquivos de log e de plano são salvos. Consulte o comando [azcopy env](storage-ref-azcopy-env.md) para saber mais.

```
azcopy jobs remove [jobID] [flags]
```

## <a name="related-conceptual-articles"></a>Artigos conceituais relacionados

- [Introdução ao AzCopy](storage-use-azcopy-v10.md)
- [Transferir dados com o armazenamento de BLOBs e AzCopy](storage-use-azcopy-blobs.md)
- [Transferir dados com o AzCopy e o Armazenamento de Arquivos](storage-use-azcopy-files.md)
- [Configurar, otimizar e solucionar problemas do AzCopy](storage-use-azcopy-configure.md)

## <a name="examples"></a>Exemplos

```
  azcopy jobs rm e52247de-0323-b14d-4cc8-76e0be2e2d44
```

## <a name="options"></a>Opções

**-h,--ajuda**                Ajuda para remover.

## <a name="options-inherited-from-parent-commands"></a>Opções herdadas de comandos pai

**--Cap-Mbps UInt32**      Limita a taxa de transferência, em megabits por segundo. A taxa de transferência por minuto pode variar um pouco a partir do limite. Se essa opção for definida como zero ou for omitida, a taxa de transferência não será limitada.

**--** formato da cadeia de caracteres do tipo de saída da saída do comando. As opções incluem: Text, JSON. O valor padrão é ' Text '. (padrão "texto")

## <a name="see-also"></a>Confira também

- [azcopy jobs](storage-ref-azcopy-jobs.md)

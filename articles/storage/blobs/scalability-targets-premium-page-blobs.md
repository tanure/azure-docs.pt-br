---
title: Metas de escalabilidade para contas de armazenamento de blobs de páginas Premium
titleSuffix: Azure Storage
description: Uma conta de armazenamento de blob de página de desempenho Premium é otimizada para operações de leitura/gravação. Esse tipo de conta de armazenamento faz backup de um disco não gerenciado para uma máquina virtual do Azure.
services: storage
author: tamram
ms.service: storage
ms.topic: conceptual
ms.date: 12/18/2019
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: 2c238bf3911283db5d09fdd5679d784c5e5401f8
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/09/2020
ms.locfileid: "88654796"
---
# <a name="scalability-and-performance-targets-for-premium-page-blob-storage-accounts"></a>Escalabilidade e metas de desempenho para contas de armazenamento de blob de página Premium

[!INCLUDE [storage-scalability-intro-include](../../../includes/storage-scalability-intro-include.md)]

## <a name="scale-targets-for-premium-page-blob-accounts"></a>Destinos de escala para contas de blob de páginas Premium

Uma conta de armazenamento de blob de página de desempenho Premium é otimizada para operações de leitura/gravação. Esse tipo de conta de armazenamento faz backup de um disco não gerenciado para uma máquina virtual do Azure.

> [!NOTE]
> A Microsoft recomenda o uso de discos gerenciados com VMs (máquinas virtuais) do Azure, se possível. Para obter mais informações sobre discos gerenciados, consulte [visão geral de armazenamento em disco do Azure para VMs](../../virtual-machines/managed-disks-overview.md).

As contas de armazenamento de blobs de páginas Premium têm as seguintes metas de escalabilidade:

| Capacidade total da conta                            | Largura de banda total para uma conta de armazenamento com redundância local                     |
| ------------------------------------------------- | --------------------------------------------------------------------------- |
| Capacidade do disco: 4 TB (disco individual)/35 TB (total cumulativo de todos os discos) <br>Capacidade do instantâneo: 10 TB | Até 50 gigabits para entrada<sup>1</sup> + saída<sup>2</sup> |

<sup>1</sup> Todos os dados (solicitações) que são enviados para uma conta de armazenamento

<sup>2</sup> Todos os dados (respostas) que são recebidos de uma conta de armazenamento

Uma conta de blob de páginas Premium é uma conta de uso geral configurada para desempenho premium. As contas de armazenamento de uso geral v2 são recomendadas.

Se você estiver usando contas de armazenamento de blobs de páginas Premium para discos não gerenciados e seu aplicativo exceder as metas de escalabilidade de uma única conta de armazenamento, a Microsoft recomenda a migração para o Managed disks. Para obter mais informações sobre discos gerenciados, consulte [visão geral de armazenamento em disco do Azure para VMs](../../virtual-machines/managed-disks-overview.md).

Se você não puder migrar para o Managed disks, crie seu aplicativo para usar várias contas de armazenamento e particione seus dados entre essas contas de armazenamento. Por exemplo, se você quiser anexar discos de 51 TB em várias VMs, separe-as entre duas contas de armazenamento. O limite para uma conta de armazenamento único premium é de 35 TB. Certifique-se de que uma única conta de armazenamento de desempenho premium nunca tenha mais de 35 TB de discos provisionados.

## <a name="see-also"></a>Confira também

- [Metas de escalabilidade e desempenho das contas de Armazenamento Standard](../common/scalability-targets-standard-account.md)
- [Metas de escalabilidade para contas de armazenamento de blob de blocos Premium](../blobs/scalability-targets-premium-block-blobs.md)
- [Limites e cotas de assinatura do Azure](../../azure-resource-manager/management/azure-subscription-service-limits.md)

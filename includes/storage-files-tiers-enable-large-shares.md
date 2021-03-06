---
title: arquivo de inclusão
description: arquivo de inclusão
services: storage
author: roygara
ms.service: storage
ms.topic: include
ms.date: 12/27/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 327e86642041a607ada7c1173bb053b12a41832c
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/25/2020
ms.locfileid: "96011672"
---
Por padrão, os compartilhamentos de arquivo padrão podem abranger até 5 TiB, embora o limite de compartilhamento possa ser aumentado para 100 TiB. Para fazer isso, o recurso de *compartilhamento de arquivo grande* precisa ser habilitado no nível da conta de armazenamento. As contas de armazenamento Premium (contas de armazenamento *FileStorage*) não têm o sinalizador de recurso de compartilhamento de arquivo grande, pois todos os compartilhamentos de arquivo Premium já estão habilitados para provisionar até a capacidade total de 100 TiB.

Você só pode habilitar compartilhamentos de arquivo grande em contas de armazenamento padrão com redundância local ou de zona. Depois de habilitar o sinalizador de recurso de compartilhamento de arquivo grande, não será possível alterar o nível de redundância para armazenamento com redundância geográfica ou com redundância de zona geográfica.

Para habilitar compartilhamentos de arquivo grande em uma conta de armazenamento existente, navegue até a exibição **Configuração** no sumário da conta de armazenamento e defina o botão de alternância do compartilhamento de arquivo grande como habilitado:

![Uma captura de tela da opção habilitar o botão de alternância do compartilhamento de arquivo grande no portal do Azure](media/storage-files-tiers-enable-large-shares/enable-lfs-0.png)

Você também pode habilitar compartilhamentos de arquivo de 100 TiB por meio do cmdlet [`Set-AzStorageAccount`](/powershell/module/az.storage/set-azstorageaccount) do PowerShell e do comando [`az storage account update`](/cli/azure/storage/account#az-storage-account-update) da CLI do Azure. Para obter instruções detalhadas sobre como habilitar compartilhamentos de arquivo grande, confira [habilitar e criar compartilhamentos de arquivo grande](../articles/storage/files/storage-files-how-to-create-large-file-share.md).

Para saber mais sobre como criar compartilhamentos de arquivo em novas contas de armazenamento, confira [criar um compartilhamento de arquivo do Azure](../articles/storage/files/storage-how-to-create-file-share.md).
---
title: Baixar uma lista de grupos no portal de Azure Active Directory | Microsoft Docs
description: Baixe as propriedades do grupo em massa no centro de administração do Azure em Azure Active Directory.
services: active-directory
author: curtand
ms.author: curtand
manager: daveba
ms.date: 11/15/2020
ms.topic: how-to
ms.service: active-directory
ms.workload: identity
ms.custom: it-pro
ms.reviewer: jeffsta
ms.collection: M365-identity-device-management
ms.openlocfilehash: bd624abdcd358dfc1d26c3092e53691ad51cf75f
ms.sourcegitcommit: b8eba4e733ace4eb6d33cc2c59456f550218b234
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/23/2020
ms.locfileid: "95503558"
---
# <a name="bulk-download-a-list-of-groups-in-azure-active-directory"></a>Baixar em massa uma lista de grupos no Azure Active Directory

Usando o portal do Azure Active Directory (AD do Azure), você pode fazer o download em massa da lista de todos os grupos em sua organização para um arquivo CSV (valores separados por vírgula).

## <a name="to-download-a-list-of-groups"></a>Para baixar uma lista de grupos

1. Entre no [portal do Azure](https://portal.azure.com) com uma conta de administrador na organização.
1. No Azure AD, selecione **grupos**  >  **baixar grupos**.
1. Na página de **Download grupos** , selecione **Iniciar** para receber um arquivo CSV que lista seus grupos.

   ![O comando baixar grupos está na página todos os grupos](./media/groups-bulk-download/bulk-download.png)

## <a name="check-download-status"></a>Verificar status de download

É possível ver o status de todas as suas solicitações em massa pendentes na página **Resultados da operação em massa**.

[![Verifique o status na página resultados de operações em massa.](./media/groups-bulk-download/bulk-center.png)](./media/groups-bulk-download/bulk-center.png#lightbox)

## <a name="bulk-download-service-limits"></a>Limites do serviço de download em massa

Cada atividade em massa para baixar uma lista de grupos pode ser executada por até uma hora. Isso permite que você baixe uma lista de pelo menos 300.000 grupos.

## <a name="next-steps"></a>Próximas etapas

- [Remover membros do grupo em massa](groups-bulk-remove-members.md)
- [Baixar membros de um grupo](groups-bulk-download-members.md)

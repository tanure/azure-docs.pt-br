---
title: arquivo de inclusão
description: arquivo de inclusão
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 10/19/2018
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: fcfe05db6a9be1049ca5da06985f31135ac79f3b
ms.sourcegitcommit: c95e2d89a5a3cf5e2983ffcc206f056a7992df7d
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/24/2020
ms.locfileid: "95556562"
---
Você é cobrado pelo Armazenamento do Azure com base no seu uso da conta de armazenamento. Todos os objetos em uma conta de armazenamento são cobrados juntos como um grupo. 

Os custos de armazenamento são calculados de acordo com os seguintes fatores: 

* **Região** refere-se à região geográfica na qual sua conta está baseada.
* **Tipo de conta** refere-se ao tipo de conta de armazenamento que você está usando. 
* **Camada de acesso** refere-se ao padrão de uso de dados que você especificou para sua conta de uso geral v2 ou de armazenamento de blobs.
* A **capacidade** de armazenamento refere-se a quanto de sua alocação de conta de armazenamento você está usando para armazenar dados.
* A **replicação** determina quantas cópias dos seus dados serão mantidas de uma só vez e em quais locais.
* As **transações** referem-se a todas as operações de leitura e gravação no Armazenamento do Microsoft Azure.
* A **saída de dados** refere-se a dados transferidos para fora de uma região do Azure. Quando os dados em sua conta de armazenamento são acessados por um aplicativo que não está sendo executado na mesma região, você é cobrado pela saída de dados. Para obter informações sobre como usar grupos de recursos para agrupar seus dados e serviços na mesma região para limitar os encargos de saída, consulte [O que é um grupo de recursos do Azure?](/azure/cloud-adoption-framework/govern/resource-consistency/resource-access-management#what-is-an-azure-resource-group). 

A página [Preços de Armazenamento do Azure](https://azure.microsoft.com/pricing/details/storage/) fornece informações detalhadas de preços com base no tipo de conta, capacidade de armazenamento, replicação e transações. A página [Detalhes de preços de transferências de dados](https://azure.microsoft.com/pricing/details/data-transfers/) fornece informações detalhadas de preços para saída de dados. Você pode usar a [Calculadora de preços do Armazenamento do Azure](https://azure.microsoft.com/pricing/calculator/?scenario=data-management) para ajudar a estimar os custos.
---
title: Disponibilidade e redundância do Azure Key Vault – Azure Key Vault | Microsoft Docs
description: Saiba mais sobre a disponibilidade e a redundância do Azure Key Vault.
services: key-vault
author: ShaneBala-keyvault
manager: ravijan
ms.service: key-vault
ms.subservice: general
ms.topic: tutorial
ms.date: 08/28/2020
ms.author: sudbalas
ms.openlocfilehash: aea5f0428fe55c0dae3734e196008cbc26a974b9
ms.sourcegitcommit: 1d6ec4b6f60b7d9759269ce55b00c5ac5fb57d32
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/13/2020
ms.locfileid: "94576206"
---
# <a name="azure-key-vault-availability-and-redundancy"></a>Redundância e disponibilidade de Cofre de Chaves do Azure

O Cofre de Chaves do Azure tem várias camadas de redundância, a fim de garantir que seus segredos e chaves permaneçam disponíveis para seu aplicativo até mesmo se os componentes individuais do serviço falharem.

> [!NOTE]
> Este guia se aplica aos cofres. Os pools do HSM gerenciado usam um modelo diferente de alta disponibilidade e recuperação de desastre. Confira o [Guia de recuperação de desastre do HSM gerenciado](../managed-hsm/disaster-recovery-guide.md) para obter mais informações.

O conteúdo do seu cofre de chaves é replicado na região e em uma região secundária a, pelo menos, 150 milhas de distância, mas na mesma geografia, para manter alta durabilidade das chaves e dos segredos. Para obter detalhes sobre pares de regiões específicas, confira [Regiões emparelhadas do Azure](../../best-practices-availability-paired-regions.md). A exceção ao modelo de regiões emparelhadas é o Sul do Brasil, que permite apenas a opção de manter os dados residentes no Sul do Brasil. O Sul do Brasil usa o LRS (armazenamento com redundância local) para replicar seus dados três vezes na única localização/região.   

Se os componentes individuais dentro do serviço de cofre de chaves falharem, os componentes alternativos dentro da região interferirão para atender à sua solicitação, de modo a garantir que não haja degradação da funcionalidade. Você não precisa realizar nenhuma ação para iniciar esse processo, ele ocorre automaticamente e será transparente para você.

No eventual caso de uma região inteira do Azure ficar indisponível, as solicitações que você faz do Cofre de Chaves do Azure nessa região são roteadas automaticamente (*failover*) para uma região secundária. Quando a região primária estiver disponível novamente, as solicitações serão roteadas de volta (*failback*) para a região primária. Novamente, não é necessário executar nenhuma ação, pois isso acontecerá de modo automático.

Com esse design de alta disponibilidade, o Azure Key Vault não requer nenhum tempo de inatividade para atividades de manutenção.

Há algumas advertências que você deve conhecer:

* No caso de um failover de região, pode levar alguns minutos para o serviço executar failover. As solicitações feitas durante esse período antes do failover podem falhar.
* Se você estiver usando o link privado para se conectar ao cofre de chaves, poderá levar até 20 minutos para a conexão ser restabelecida no caso de um failover. 
* Durante o failover, o cofre de chaves estará no modo somente leitura. As solicitações permitidas nesse modo são:
  * Listar certificados
  * Obter certificados
  * Listar segredos
  * Obter segredos
  * Listar chaves
  * Obter (propriedades das) chaves
  * Encrypt
  * Descriptografar
  * Encapsular
  * Desencapsular
  * Verificar
  * Assinar
  * Backup

* Durante o failover, você não poderá fazer alterações nas propriedades do cofre de chaves. Você não poderá alterar a política de acesso ou as configurações e definições de firewall.

* Após o failback de um failover, todos os tipos de solicitação (ou seja, solicitações de leitura *e* de gravação) são disponibilizados.

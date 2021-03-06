---
title: Configuração de endereço IP de front-end do gateway Aplicativo Azure
description: Este artigo descreve como configurar o endereço IP de front-end do gateway de Aplicativo Azure.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: conceptual
ms.date: 09/09/2020
ms.author: surmb
ms.openlocfilehash: dc5efd6ad478710ba839634a49f041211756af71
ms.sourcegitcommit: 0ce1ccdb34ad60321a647c691b0cff3b9d7a39c8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/05/2020
ms.locfileid: "93397664"
---
# <a name="application-gateway-front-end-ip-address-configuration"></a>Configuração do endereço IP de front-end do gateway de aplicativo

Você pode configurar o gateway de aplicativo para ter um endereço IP público, um endereço IP privado ou ambos. Um endereço IP público é necessário quando você hospeda um back-end que os clientes devem acessar pela Internet por meio de um IP virtual (VIP) da Internet.

## <a name="public-and-private-ip-address-support"></a>Suporte a endereços IP públicos e privados

O Gateway de Aplicativo v2 atualmente não dá suporte apenas ao modo de IP privado. Ele dá suporte às seguintes combinações:

* Endereço IP privado e endereço IP público
* Somente endereço IP público

Para obter mais informações, consulte perguntas frequentes [sobre o gateway de aplicativo](application-gateway-faq.md#how-do-i-use-application-gateway-v2-with-only-private-frontend-ip-address).


Um endereço IP público não é necessário para um ponto de extremidade interno que não esteja exposto à Internet. Isso é conhecido como ponto de extremidade ILB ( *balanceador de carga interno* ) ou IP de front-end privado. Um ILB de gateway de aplicativo é útil para aplicativos de linha de negócios internos que não são expostos à Internet. Ele também é útil para serviços e camadas em um aplicativo de várias camadas dentro de um limite de segurança que não é exposto à Internet, mas que exigem distribuição de carga Round Robin, adesão de sessão ou terminação de TLS.

Há suporte para apenas um endereço IP público ou um endereço IP privado. Você escolhe o IP de front-end ao criar o gateway de aplicativo.

- Para um endereço IP público, você pode criar um novo endereço IP público ou usar um IP público existente no mesmo local que o gateway de aplicativo. Para obter mais informações, consulte [endereço IP público estático vs. dinâmico](./application-gateway-components.md#static-versus-dynamic-public-ip-address).

- Para um endereço IP privado, você pode especificar um endereço IP privado da sub-rede em que o gateway de aplicativo é criado. Se você não especificar um, um endereço IP arbitrário será selecionado automaticamente da sub-rede. O tipo de endereço IP que você selecionar (estático ou dinâmico) não poderá ser alterado posteriormente. Para obter mais informações, consulte [criar um gateway de aplicativo com um balanceador de carga interno](./application-gateway-ilb-arm.md).

Um endereço IP de front-end é associado a um *ouvinte* , que verifica as solicitações de entrada no IP de front-end.

## <a name="next-steps"></a>Próximas etapas

- [Saiba mais sobre a configuração do ouvinte](configuration-listeners.md)
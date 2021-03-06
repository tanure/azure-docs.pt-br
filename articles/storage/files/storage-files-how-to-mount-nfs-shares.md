---
title: Montar um compartilhamento de arquivos NFS do Azure-arquivos do Azure
description: Saiba como montar um compartilhamento do sistema de arquivos de rede.
author: roygara
ms.service: storage
ms.topic: how-to
ms.date: 09/15/2020
ms.author: rogarana
ms.subservice: files
ms.custom: references_regions
ms.openlocfilehash: 530ae82720e6b4eb6a3e4d1021c0b37b9f4dbf5c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/09/2020
ms.locfileid: "90707434"
---
# <a name="how-to-mount-an-nfs-file-share"></a>Como montar um compartilhamento de arquivos NFS

[Arquivos do Azure](storage-files-introduction.md) é o sistema de arquivos de nuvem de fácil acesso da Microsoft. Os compartilhamentos de arquivos do Azure podem ser montados em distribuições do Linux usando o protocolo SMB ou o protocolo NFS (sistema de arquivos de rede). Este artigo se concentra na montagem com NFS, para obter detalhes sobre como montar com o SMB, consulte [usar os arquivos do Azure com o Linux](storage-how-to-use-files-linux.md). Para obter detalhes sobre cada um dos protocolos disponíveis, consulte [protocolos de compartilhamento de arquivos do Azure](storage-files-compare-protocols.md).

## <a name="limitations"></a>Limitações

[!INCLUDE [files-nfs-limitations](../../../includes/files-nfs-limitations.md)]

### <a name="regional-availability"></a>Disponibilidade regional

[!INCLUDE [files-nfs-regional-availability](../../../includes/files-nfs-regional-availability.md)]

## <a name="prerequisites"></a>Pré-requisitos

- [Crie um compartilhamento NFS](storage-files-how-to-create-nfs-shares.md).

    > [!IMPORTANT]
    > Os compartilhamentos NFS só podem ser acessados de redes confiáveis. As conexões com o compartilhamento NFS devem se originar de uma das seguintes fontes:

- Use uma das seguintes soluções de rede:
    - [Crie um ponto de extremidade privado](storage-files-networking-endpoints.md#create-a-private-endpoint) (recomendado) ou [restrinja o acesso ao seu ponto de extremidade público](storage-files-networking-endpoints.md#restrict-public-endpoint-access).
    - [Configure uma VPN ponto a site (P2S) no Linux para uso com os arquivos do Azure](storage-files-configure-p2s-vpn-linux.md).
    - [Configure uma VPN site a site para uso com os arquivos do Azure](storage-files-configure-s2s-vpn.md).
    - Configure o [ExpressRoute](../../expressroute/expressroute-introduction.md).

## <a name="disable-secure-transfer"></a>Desabilitar transferência segura

1. Entre no portal do Azure e acesse a conta de armazenamento que contém o compartilhamento NFS que você criou.
1. Selecione **Configuração**.
1. Selecione **desabilitado** para **transferência segura necessária**.
1. Selecione **Salvar**.

    :::image type="content" source="media/storage-files-how-to-mount-nfs-shares/storage-account-disable-secure-transfer.png" alt-text="Captura de tela de configuração da conta de armazenamento com transferência segura desabilitada.":::

## <a name="mount-an-nfs-share"></a>Montar um compartilhamento NFS

1. Depois que o compartilhamento de arquivos for criado, selecione o compartilhamento e selecione **conectar do Linux**.
1. Insira o caminho de montagem que você deseja usar e copie o script.
1. Conecte-se ao seu cliente e use o script de montagem fornecido.

    :::image type="content" source="media/storage-files-how-to-create-mount-nfs-shares/mount-nfs-file-share-script.png" alt-text="Captura de tela de configuração da conta de armazenamento com transferência segura desabilitada.":::

Agora você montou seu compartilhamento NFS.

## <a name="next-steps"></a>Próximas etapas

- Saiba mais sobre os arquivos do Azure com nosso artigo, [planejando uma implantação de arquivos do Azure](storage-files-planning.md).
- Se você tiver problemas, consulte [solucionar problemas de compartilhamentos de arquivos NFS do Azure](storage-troubleshooting-files-nfs.md).
---
title: Usar o PowerShell para gerenciar dados em nuvens independentes do Azure
titleSuffix: Azure Storage
description: Gerenciamento de armazenamento na nuvem da China, na nuvem governamental e na nuvem alemã usando o Azure PowerShell.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 12/04/2019
ms.author: tamram
ms.subservice: common
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: e924a5f6c765b5b964fe3b1492393b063d9d23b4
ms.sourcegitcommit: 400f473e8aa6301539179d4b320ffbe7dfae42fe
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/28/2020
ms.locfileid: "92783565"
---
# <a name="managing-storage-in-the-azure-independent-clouds-using-powershell"></a>Gerenciamento do Armazenamento nas nuvens independentes do Azure usando o PowerShell

A maioria das pessoas usa a Nuvem Pública do Azure em suas implantações globais do Azure. Há também algumas implantações independentes do Microsoft Azure por motivos de soberania e assim por diante. Essas implantações independentes são chamadas de “ambientes”. A lista a seguir fornece detalhes sobre as nuvens independentes disponíveis no momento.

* [Nuvem do Azure Governamental](https://azure.microsoft.com/features/gov/)
* [Nuvem do Azure China 21Vianet operada pela 21Vianet na China](http://www.windowsazure.cn/)
* [Nuvem Alemã do Azure](../../germany/germany-welcome.md)

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="using-an-independent-cloud"></a>Utilização de uma nuvem independente

Para utilizar o Armazenamento do Azure em uma das nuvens independentes, você deve se conectar àquela nuvem em vez de se conectar à nuvem pública do Azure. Para usar uma das nuvens independentes em vez da nuvem pública do Azure:

* Especifique o *ambiente* com o qual deseja se conectar.
* Determine e use as regiões disponíveis.
* Use o sufixo do ponto de extremidade correto, que é diferente do da nuvem pública do Azure.

Estes exemplos exigem a versão 0.7 ou posterior do módulo do Azure PowerShell. Em uma janela do PowerShell, execute `Get-Module -ListAvailable Az` para localizar a versão. Se nada for listado ou você precisar atualizar, consulte [Instalar o módulo do Azure PowerShell](/powershell/azure/install-Az-ps).

## <a name="log-in-to-azure"></a>Fazer logon no Azure

Execute o cmdlet [Get-AzEnvironment](/powershell/module/az.accounts/get-azenvironment) para ver os ambientes do Azure disponíveis:

```powershell
Get-AzEnvironment
```

Entre em sua conta que tem acesso à nuvem com a qual você deseja se conectar e defina o ambiente. Este exemplo mostra como entrar em uma conta que usa a Nuvem do Azure Governamental.   

```powershell
Connect-AzAccount –Environment AzureUSGovernment
```

Para acessar a Nuvem da China, use o ambiente **AzureChinaCloud** . Para acessar a Nuvem alemã, use **AzureGermanCloud** .

Neste ponto, se precisar da lista de locais para criar uma conta de armazenamento ou outro recurso, você poderá consultar os locais disponíveis para a nuvem selecionada usando [Get-AzLocation](/powershell/module/az.resources/get-azlocation).

```powershell
Get-AzLocation | select Location, DisplayName
```

A tabela a seguir mostra os locais retornados para a Nuvem alemã.

|Location | Nome de exibição |
|----|----|
| `germanycentral` | Alemanha Central|
| `germanynortheast` | Nordeste da Alemanha |


## <a name="endpoint-suffix"></a>Sufixo de ponto de extremidade

O sufixo de ponto de extremidade para cada um desses ambientes é diferente do ponto de extremidade da nuvem pública do Azure. Por exemplo, o sufixo de ponto de extremidade do blob da nuvem pública do Azure é **blob.core.windows.net** . Para a nuvem do governo, o sufixo de ponto de extremidade do blob é **blob.core.usgovcloudapi.net** .

### <a name="get-endpoint-using-get-azenvironment"></a>Obter o ponto de extremidade usando Get-AzEnvironment

Recupere o sufixo de ponto de extremidade usando [Get-AzEnvironment](/powershell/module/az.accounts/get-azenvironment). O ponto de extremidade é a propriedade *StorageEndpointSuffix* do ambiente.

Os trechos de código a seguir mostram como recuperar o sufixo do ponto de extremidade. Todos esses comandos retornam algo como "core.cloudapp.net" ou "core.cloudapi.de", etc. Acrescente o sufixo ao serviço de armazenamento para acessar esse serviço. Por exemplo, “queue.core.cloudapi.de” acessará o serviço Fila na nuvem alemã.

Este snippet de código recupera todos os ambientes e o sufixo do ponto de extremidade para cada um.

```powershell
Get-AzEnvironment | select Name, StorageEndpointSuffix 
```

Esse comando retorna os seguintes resultados.

| Name| StorageEndpointSuffix|
|----|----|
| AzureChinaCloud | core.chinacloudapi.cn|
| AzureCloud | core.windows.net |
| AzureGermanCloud | core.cloudapi.de|
| AzureUSGovernment | core.usgovcloudapi.net |

Para recuperar todas as propriedades para o ambiente especificado, chame **Get-AzEnvironment** e especifique o nome da nuvem. Este snippet de código retorna uma lista de propriedades. Procure por **StorageEndpointSuffix** na lista. O exemplo a seguir é para a nuvem alemã.

```powershell
Get-AzEnvironment -Name AzureGermanCloud
```

Os resultados são semelhantes aos seguintes valores:

|Nome da propriedade|Valor|
|----|----|
| Nome | `AzureGermanCloud` |
| EnableAdfsAuthentication | `False` |
| ActiveDirectoryServiceEndpointResourceI | `http://management.core.cloudapi.de/` |
| GalleryURL | `https://gallery.cloudapi.de/` |
| ManagementPortalUrl | `https://portal.microsoftazure.de/` |
| ServiceManagementUrl | `https://manage.core.cloudapi.de/` |
| PublishSettingsFileUrl| `https://manage.microsoftazure.de/publishsettings/index` |
| ResourceManagerUrl | `http://management.microsoftazure.de/` |
| SqlDatabaseDnsSuffix | `.database.cloudapi.de` |
| **StorageEndpointSuffix** | `core.cloudapi.de` |
| ... | ... |

Para recuperar apenas a propriedade de sufixo de ponto de extremidade do armazenamento, recupere a nuvem específica e solicite somente aquela propriedade.

```powershell
$environment = Get-AzEnvironment -Name AzureGermanCloud
Write-Host "Storage EndPoint Suffix = " $environment.StorageEndpointSuffix
```

Esse comando retorna as informações a seguir:

`Storage Endpoint Suffix = core.cloudapi.de`

### <a name="get-endpoint-from-a-storage-account"></a>Obter o ponto de extremidade de uma conta de armazenamento

Você também pode examinar as propriedades de uma conta de armazenamento para recuperar os pontos de extremidade:

```powershell
# Get a reference to the storage account.
$resourceGroup = "myexistingresourcegroup"
$storageAccountName = "myexistingstorageaccount"
$storageAccount = Get-AzStorageAccount `
  -ResourceGroupName $resourceGroup `
  -Name $storageAccountName 
  # Output the endpoints.
Write-Host "blob endpoint = " $storageAccount.PrimaryEndPoints.Blob 
Write-Host "file endpoint = " $storageAccount.PrimaryEndPoints.File
Write-Host "queue endpoint = " $storageAccount.PrimaryEndPoints.Queue
Write-Host "table endpoint = " $storageAccount.PrimaryEndPoints.Table
```

Para uma conta de armazenamento na nuvem governamental, esse comando retorna a seguinte saída:

```
blob endpoint = http://myexistingstorageaccount.blob.core.usgovcloudapi.net/
file endpoint = http://myexistingstorageaccount.file.core.usgovcloudapi.net/
queue endpoint = http://myexistingstorageaccount.queue.core.usgovcloudapi.net/
table endpoint = http://myexistingstorageaccount.table.core.usgovcloudapi.net/
```

## <a name="after-setting-the-environment"></a>Após configurar o ambiente

Agora você pode usar o PowerShell para gerenciar suas contas de armazenamento e acessar dados de BLOB, fila, arquivo e tabela. Para obter mais informações, consulte [AZ. Storage](/powershell/module/az.storage).

## <a name="clean-up-resources"></a>Limpar os recursos

Se você criou um novo grupo de recursos e uma conta de armazenamento para este exercício, você pode remover ambos os ativos excluindo o grupo de recursos. A exclusão do grupo de recursos exclui todos os recursos contidos nele.

```powershell
Remove-AzResourceGroup -Name $resourceGroup
```

## <a name="next-steps"></a>Próximas etapas

* [Persistência de logons de usuário nas sessões do PowerShell](/powershell/azure/context-persistence)
* [Armazenamento do Azure governamental](../../azure-government/compare-azure-government-global-azure.md)
* [Guia do Desenvolvedor do Microsoft Azure Government](../../azure-government/documentation-government-developer-guide.md)
* [Notas do desenvolvedor para aplicativos da 21Vianet do Azure na China](https://msdn.microsoft.com/library/azure/dn578439.aspx)
* [Documentação do Azure Alemanha](../../germany/germany-welcome.md)
---
title: Testar uma imagem de máquina virtual do Azure para o Azure Marketplace
description: Saiba como testar e enviar uma oferta de máquina virtual do Azure no Azure Marketplace.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: how-to
author: github-2407
ms.author: krsh
ms.date: 10/15/2020
ms.openlocfilehash: 36eebb218ed2b2d9a48cf7d970896115af5cf6f8
ms.sourcegitcommit: 6906980890a8321dec78dd174e6a7eb5f5fcc029
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/22/2020
ms.locfileid: "92424863"
---
# <a name="test-a-virtual-machine-image"></a>Testar uma imagem de máquina virtual

Este artigo descreve como testar e enviar uma imagem de VM (máquina virtual) no Marketplace comercial para garantir que ela atenda aos requisitos mais recentes de publicação do Azure Marketplace.

Conclua estas etapas antes de enviar sua oferta de VM:

- Implantar uma VM do Azure usando sua imagem generalizada; consulte [criar uma máquina virtual usando uma base aprovada](azure-vm-create-using-approved-base.md) ou [criar uma máquina virtual usando sua própria imagem](azure-vm-create-using-own-image.md).
- Execute as validações.

## <a name="deploy-an-azure-vm-using-your-generalized-image"></a>Implante uma VM do Azure usando sua imagem generalizada

Esta seção descreve como implantar uma imagem de VHD (disco rígido virtual) generalizado para criar um novo recurso de VM do Azure. Para esse processo, usaremos o modelo de Azure Resource Manager fornecido e Azure PowerShell script.

### <a name="prepare-an-azure-resource-manager-template"></a>Preparar um modelo do Azure Resource Manager

Esta seção descreve como criar e implantar uma imagem de VM (máquina virtual) fornecida pelo usuário. Você pode fazer isso fornecendo imagens VHD do sistema operacional e do disco de dados de um disco rígido virtual implantado pelo Azure. Essas etapas implantam a VM usando o VHD generalizado.

1. Entre no [portal do Azure](https://portal.azure.com/).
2. Carregue o VHD do sistema operacional generalizado e os VHDs do disco de dados em sua conta de armazenamento do Azure.
3. Na home page, selecione **criar um recurso**, pesquise "implantação de modelo" e selecione **criar**.
4. Escolha **Criar seu modelo no editor**.

    :::image type="content" source="media/vm/template-deployment.png" alt-text="Mostra a seleção de um modelo.&quot;:::

5. Cole o modelo JSON a seguir no editor e selecione **salvar**.

```JSON
{
    &quot;$schema&quot;: &quot;https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#&quot;,
    &quot;contentVersion&quot;: &quot;1.0.0.0&quot;,
    &quot;parameters&quot;: {
        &quot;userStorageAccountName&quot;: {
            &quot;type&quot;: &quot;String&quot;
        },
        &quot;userStorageContainerName&quot;: {
            &quot;defaultValue&quot;: &quot;vhds&quot;,
            &quot;type&quot;: &quot;String&quot;
        },
        &quot;dnsNameForPublicIP&quot;: {
            &quot;type&quot;: &quot;String&quot;
        },
        &quot;adminUserName&quot;: {
            &quot;defaultValue&quot;: &quot;isv&quot;,
            &quot;type&quot;: &quot;String&quot;
        },
        &quot;adminPassword&quot;: {
            &quot;defaultValue&quot;: &quot;&quot;,
            &quot;type&quot;: &quot;SecureString&quot;
        },
        &quot;osType&quot;: {
            &quot;defaultValue&quot;: &quot;windows&quot;,
            &quot;allowedValues&quot;: [
                &quot;windows&quot;,
                &quot;linux&quot;
            ],
            &quot;type&quot;: &quot;String&quot;
        },
        &quot;subscriptionId&quot;: {
            &quot;type&quot;: &quot;String&quot;
        },
        &quot;location&quot;: {
            &quot;type&quot;: &quot;String&quot;
        },
        &quot;vmSize&quot;: {
            &quot;type&quot;: &quot;String&quot;
        },
        &quot;publicIPAddressName&quot;: {
            &quot;type&quot;: &quot;String&quot;
        },
        &quot;vmName&quot;: {
            &quot;type&quot;: &quot;String&quot;
        },
        &quot;virtualNetworkName&quot;: {
            &quot;type&quot;: &quot;String&quot;
        },
        &quot;nicName&quot;: {
            &quot;type&quot;: &quot;String&quot;
        },
        &quot;vhdUrl&quot;: {
            &quot;type&quot;: &quot;String&quot;,
            &quot;metadata&quot;: {
                &quot;description&quot;: &quot;VHD Url...&quot;
            }
        }
    },
    &quot;variables&quot;: {
        &quot;addressPrefix&quot;: &quot;10.0.0.0/16&quot;,
        &quot;subnet1Name&quot;: &quot;Subnet-1&quot;,
        &quot;subnet2Name&quot;: &quot;Subnet-2&quot;,
        &quot;subnet1Prefix&quot;: &quot;10.0.0.0/24&quot;,
        &quot;subnet2Prefix&quot;: &quot;10.0.1.0/24&quot;,
        &quot;publicIPAddressType&quot;: &quot;Dynamic&quot;,
        &quot;vnetID&quot;: &quot;[resourceId('Microsoft.Network/virtualNetworks',parameters('virtualNetworkName'))]&quot;,
        &quot;subnet1Ref&quot;: &quot;[concat(variables('vnetID'),'/subnets/',variables('subnet1Name'))]&quot;,
        &quot;hostDNSNameScriptArgument&quot;: &quot;[concat(parameters('dnsNameForPublicIP'),'.',parameters('location'),'.cloudapp.azure.com')]&quot;,
        &quot;osDiskVhdName&quot;: &quot;[concat('http://',parameters('userStorageAccountName'),'.blob.core.windows.net/',parameters('userStorageContainerName'),'/',parameters('vmName'),'osDisk.vhd')]&quot;
    },
    &quot;resources&quot;: [
        {
            &quot;type&quot;: &quot;Microsoft.Network/publicIPAddresses&quot;,
            &quot;apiVersion&quot;: &quot;2015-06-15&quot;,
            &quot;name&quot;: &quot;[parameters('publicIPAddressName')]&quot;,
            &quot;location&quot;: &quot;[parameters('location')]&quot;,
            &quot;properties&quot;: {
                &quot;publicIPAllocationMethod&quot;: &quot;[variables('publicIPAddressType')]&quot;,
                &quot;dnsSettings&quot;: {
                    &quot;domainNameLabel&quot;: &quot;[parameters('dnsNameForPublicIP')]&quot;
                }
            }
        },
        {
            &quot;type&quot;: &quot;Microsoft.Network/virtualNetworks&quot;,
            &quot;apiVersion&quot;: &quot;2015-06-15&quot;,
            &quot;name&quot;: &quot;[parameters('virtualNetworkName')]&quot;,
            &quot;location&quot;: &quot;[parameters('location')]&quot;,
            &quot;properties&quot;: {
                &quot;addressSpace&quot;: {
                    &quot;addressPrefixes&quot;: [
                        &quot;[variables('addressPrefix')]&quot;
                    ]
                },
                &quot;subnets&quot;: [
                    {
                        &quot;name&quot;: &quot;[variables('subnet1Name')]&quot;,
                        &quot;properties&quot;: {
                            &quot;addressPrefix&quot;: &quot;[variables('subnet1Prefix')]&quot;
                        }
                    },
                    {
                        &quot;name&quot;: &quot;[variables('subnet2Name')]&quot;,
                        &quot;properties&quot;: {
                            &quot;addressPrefix&quot;: &quot;[variables('subnet2Prefix')]&quot;
                        }
                    }
                ]
            }
        },
        {
            &quot;type&quot;: &quot;Microsoft.Network/networkInterfaces&quot;,
            &quot;apiVersion&quot;: &quot;2015-06-15&quot;,
            &quot;name&quot;: &quot;[parameters('nicName')]&quot;,
            &quot;location&quot;: &quot;[parameters('location')]&quot;,
            &quot;dependsOn&quot;: [
                &quot;[concat('Microsoft.Network/publicIPAddresses/', parameters('publicIPAddressName'))]&quot;,
                &quot;[concat('Microsoft.Network/virtualNetworks/', parameters('virtualNetworkName'))]&quot;
            ],
            &quot;properties&quot;: {
                &quot;ipConfigurations&quot;: [
                    {
                        &quot;name&quot;: &quot;ipconfig1&quot;,
                        &quot;properties&quot;: {
                            &quot;privateIPAllocationMethod&quot;: &quot;Dynamic&quot;,
                            &quot;publicIPAddress&quot;: {
                                &quot;id&quot;: &quot;[resourceId('Microsoft.Network/publicIPAddresses',parameters('publicIPAddressName'))]&quot;
                            },
                            &quot;subnet&quot;: {
                                &quot;id&quot;: &quot;[variables('subnet1Ref')]&quot;
                            }
                        }
                    }
                ]
            }
        },
        {
            &quot;type&quot;: &quot;Microsoft.Compute/virtualMachines&quot;,
            &quot;apiVersion&quot;: &quot;2015-06-15&quot;,
            &quot;name&quot;: &quot;[parameters('vmName')]&quot;,
            &quot;location&quot;: &quot;[parameters('location')]&quot;,
            &quot;dependsOn&quot;: [
                &quot;[concat('Microsoft.Network/networkInterfaces/', parameters('nicName'))]&quot;
            ],
            &quot;properties&quot;: {
                &quot;hardwareProfile&quot;: {
                    &quot;vmSize&quot;: &quot;[parameters('vmSize')]&quot;
                },
                &quot;osProfile&quot;: {
                    &quot;computername&quot;: &quot;[parameters('vmName')]&quot;,
                    &quot;adminUsername&quot;: &quot;[parameters('adminUsername')]&quot;,
                    &quot;adminPassword&quot;: &quot;[parameters('adminPassword')]&quot;
                },
                &quot;storageProfile&quot;: {
                    &quot;osDisk&quot;: {
                        &quot;name&quot;: &quot;[concat(parameters('vmName'),'-osDisk')]&quot;,
                        &quot;osType&quot;: &quot;[parameters('osType')]&quot;,
                        &quot;caching&quot;: &quot;ReadWrite&quot;,
                        &quot;image&quot;: {
                            &quot;uri&quot;: &quot;[parameters('vhdUrl')]&quot;
                        },
                        &quot;vhd&quot;: {
                            &quot;uri&quot;: &quot;[variables('osDiskVhdName')]&quot;
                        },
                        &quot;createOption&quot;: &quot;FromImage&quot;
                    }
                },
                &quot;networkProfile&quot;: {
                    &quot;networkInterfaces&quot;: [
                        {
                            &quot;id&quot;: &quot;[resourceId('Microsoft.Network/networkInterfaces',parameters('nicName'))]&quot;
                        }
                    ]
                }
            }
        },
        {
            &quot;type&quot;: &quot;Microsoft.Compute/virtualMachines/extensions&quot;,
            &quot;apiVersion&quot;: &quot;2015-06-15&quot;,
            &quot;name&quot;: &quot;[concat(parameters('vmName'),'/WinRMCustomScriptExtension')]&quot;,
            &quot;location&quot;: &quot;[parameters('location')]&quot;,
            &quot;dependsOn&quot;: [
                &quot;[concat('Microsoft.Compute/virtualMachines/', parameters('vmName'))]&quot;
            ],
            &quot;properties&quot;: {
                &quot;publisher&quot;: &quot;Microsoft.Compute&quot;,
                &quot;type&quot;: &quot;CustomScriptExtension&quot;,
                &quot;typeHandlerVersion&quot;: &quot;1.4&quot;,
                &quot;settings&quot;: {
                    &quot;fileUris&quot;: [
                        &quot;https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-winrm-windows/ConfigureWinRM.ps1&quot;,
                        &quot;https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-winrm-windows/makecert.exe&quot;,
                        &quot;https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-winrm-windows/winrmconf.cmd&quot;
                    ],
                    &quot;commandToExecute&quot;: &quot;[concat('powershell -ExecutionPolicy Unrestricted -file ConfigureWinRM.ps1 ',variables('hostDNSNameScriptArgument'))]"
                }
            }
        }
    ]
}
```

<br>

6. Forneça os valores de parâmetro para as páginas de propriedades Implantação Personalizada.

    | ResourceGroupName | O nome do grupo de recursos do Azure existente. Normalmente, usa o mesmo RG associado ao cofre de chaves. |
    | --- | --- |
    | TemplateFile | Nome do caminho completo para o arquivo VHDtoImage.json. |
    | userStorageAccountName | Nome da conta de armazenamento. |
    | dnsNameForPublicIP | Nome DNS para o IP público; deve estar em minúsculas. |
    | subscriptionId | O identificador da assinatura do Azure. |
    | Location | Local geográfico padrão do Azure para o grupo de recursos. |
    | vmName | Nome da máquina virtual. |
    | vhdUrl | Endereço Web do disco rígido virtual. |
    | vmSize | Tamanho da instância de máquina virtual. |
    | publicIPAddressName | Nome do endereço IP público. |
    | virtualNetworkName | Nome da rede virtual. |
    | nicName | Nome da placa de interface de rede para a rede virtual. |
    | adminUserName | Nome de usuário da conta de administrador. |
    | adminPassword | Senha do administrador. |


7. Depois de fornecer esses valores, selecione **Comprar**.

O Azure iniciará a implantação. Ele cria uma VM com o VHD não gerenciado especificado, no caminho de conta de armazenamento especificada. Você pode acompanhar o progresso no portal do Azure selecionando **Máquinas Virtuais** no lado esquerdo do portal. Quando a VM tiver sido criada, o status será alterado de Iniciando para Em execução.

#### <a name="for-generation-2-vm-deployment-use-this-template"></a>Para a implantação de VM de geração 2, use este modelo:

```JSON
{
   "$schema":"https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
   "contentVersion":"1.0.0.0",
   "parameters":{
      "userStorageAccountName":{
         "type":"String"
      },
      "userStorageContainerName":{
         "type":"String",
         "defaultValue":"vhds-86350-720-f4efbbb2-611b-4cd7-ad1e-afdgfuadfluad"
      },
      "dnsNameForPublicIP":{
         "type":"String"
      },
      "adminUserName":{
         "defaultValue":"isv",
         "type":"String"
      },
      "adminPassword":{
         "type":"securestring",
         "defaultValue":"Certcare@86350"
      },
      "osType":{
         "type":"string",
         "defaultValue":"linux",
         "allowedValues":[
            "windows",
            "linux"
         ]
      },
      "subscriptionId":{
         "type":"string"
      },
      "location":{
         "type":"string"
      },
      "vmSize":{
         "type":"string"
      },
      "publicIPAddressName":{
         "type":"string"
      },
      "vmName":{
         "type":"string"
      },
      "vNetNewOrExisting":{
         "defaultValue":"existing",
         "allowedValues":[
            "new",
            "existing"
         ],
         "type":"String",
         "metadata":{
            "description":"Specify whether to create a new or existing virtual network for the VM."
         }
      },
      "virtualNetworkName":{
         "type":"String",
         "defaultValue":""
      },
      "SubnetName":{
         "defaultValue":"subnet-5",
         "type":"String"
      },
      "SubnetPrefix":{
         "defaultValue":"10.0.5.0/24",
         "type":"String"
      },
      "nicName":{
         "type":"string"
      },
      "vhdUrl":{
         "type":"string",
         "metadata":{
            "description":"VHD Url..."
         }
      },
      "Datadisk1":{
         "type":"string",
         "metadata":{
            "description":"datadisk1 Url..."
         }
      },
      "Datadisk2":{
         "type":"string",
         "metadata":{
            "description":"datadisk2 Url..."
         }
      },
      "Datadisk3":{
         "type":"string",
         "metadata":{
            "description":"datadisk2 Url..."
         }
      }
   },
   "variables":{
      "addressPrefix":"10.0.0.0/16",
      "subnet1Name":"[parameters('SubnetName')]",
      "subnet1Prefix":"[parameters('SubnetPrefix')]",
      "publicIPAddressType":"Static",
      "vnetID":"[resourceId('Microsoft.Network/virtualNetworks',parameters('virtualNetworkName'))]",
      "subnet1Ref":"[concat(variables('vnetID'),'/subnets/',variables('subnet1Name'))]",
      "osDiskVhdName":"[concat('http://',parameters('userStorageAccountName'),'.blob.core.windows.net/',parameters('userStorageContainerName'),'/',parameters('vmName'),'osDisk.vhd')]",
      "dataDiskVhdName":"[concat('http://',parameters('userStorageAccountName'),'.blob.core.windows.net/',parameters('userStorageContainerName'),'/',parameters('vmName'),'datadisk')]",
      "imageName":"[concat(parameters('vmName'), '-image')]"
   },
   "resources":[
      {
         "apiVersion":"2015-05-01-preview",
         "type":"Microsoft.Network/publicIPAddresses",
         "name":"[parameters('publicIPAddressName')]",
         "location":"[parameters('location')]",
         "properties":{
            "publicIPAllocationMethod":"[variables('publicIPAddressType')]",
            "dnsSettings":{
               "domainNameLabel":"[parameters('dnsNameForPublicIP')]"
            }
         }
      },
      {
         "type":"Microsoft.Compute/images",
         "apiVersion":"2019-12-01",
         "name":"[variables('imageName')]",
         "location":"[parameters('location')]",
         "properties":{
            "storageProfile":{
               "osDisk":{
                  "osType":"[parameters('osType')]",
                  "osState":"Generalized",
                  "blobUri":"[parameters('vhdUrl')]",
                  "storageAccountType":"Standard_LRS"
               },
               "dataDisks":[
                  {
                     "lun":0,
                     "blobUri":"[parameters('Datadisk1')]",
                     "storageAccountType":"Standard_LRS"
                  },
                  {
                     "lun":1,
                     "blobUri":"[parameters('Datadisk2')]",
                     "storageAccountType":"Standard_LRS"
                  },
                  {
                     "lun":2,
                     "blobUri":"[parameters('Datadisk3')]",
                     "storageAccountType":"Standard_LRS"
                  }
               ]
            },
            "hyperVGeneration":"V2"
         }
      },
      {
         "apiVersion":"2015-05-01-preview",
         "type":"Microsoft.Network/virtualNetworks",
         "name":"[parameters('virtualNetworkName')]",
         "location":"[parameters('location')]",
         "properties":{
            "addressSpace":{
               "addressPrefixes":[
                  "[variables('addressPrefix')]"
               ]
            },
            "subnets":[
               {
                  "name":"[variables('subnet1Name')]",
                  "properties":{
                     "addressPrefix":"[variables('subnet1Prefix')]"
                  }
               }
            ]
         },
         "condition":"[equals(parameters('vNetNewOrExisting'), 'new')]"
      },
      {
         "apiVersion":"2016-09-01",
         "type":"Microsoft.Network/networkInterfaces",
         "name":"[parameters('nicName')]",
         "location":"[parameters('location')]",
         "dependsOn":[
            "[concat('Microsoft.Network/publicIPAddresses/', parameters('publicIPAddressName'))]",
            "[concat('Microsoft.Network/virtualNetworks/', parameters('virtualNetworkName'))]"
         ],
         "properties":{
            "ipConfigurations":[
               {
                  "name":"ipconfig1",
                  "properties":{
                     "privateIPAllocationMethod":"Dynamic",
                     "publicIPAddress":{
                        "id":"[resourceId('Microsoft.Network/publicIPAddresses',parameters('publicIPAddressName'))]"
                     },
                     "subnet":{
                        "id":"[variables('subnet1Ref')]"
                     }
                  }
               }
            ]
         }
      },
      {
         "apiVersion":"2019-12-01",
         "type":"Microsoft.Compute/virtualMachines",
         "name":"[parameters('vmName')]",
         "location":"[parameters('location')]",
         "dependsOn":[
            "[concat('Microsoft.Network/networkInterfaces/', parameters('nicName'))]",
            "[variables('imageName')]"
         ],
         "properties":{
            "hardwareProfile":{
               "vmSize":"[parameters('vmSize')]"
            },
            "osProfile":{
               "computername":"[parameters('vmName')]",
               "adminUsername":"[parameters('adminUsername')]",
               "adminPassword":"[parameters('adminPassword')]"
            },
            "storageProfile":{
               "imageReference":{
                  "id":"[resourceId('Microsoft.Compute/images', variables('imageName'))]"
               },
               "dataDisks":[
                  {
                     "name":"[concat(parameters('vmName'),'_DataDisk0')]",
                     "lun":0,
                     "createOption":"FromImage",
                     "managedDisk":{
                        "storageAccountType":"Standard_LRS"
                     }
                  },
                  {
                     "name":"[concat(parameters('vmName'),'_DataDisk1')]",
                     "lun":1,
                     "createOption":"FromImage",
                     "managedDisk":{
                        "storageAccountType":"Standard_LRS"
                     }
                  },
                  {
                     "name":"[concat(parameters('vmName'),'_DataDisk2')]",
                     "lun":2,
                     "createOption":"FromImage",
                     "managedDisk":{
                        "storageAccountType":"Standard_LRS"
                     }
                  }
               ],
               "osDisk":{
                  "name":"[concat(parameters('vmName'),'-OSDisk')]",
                  "createOption":"FromImage",
                  "managedDisk":{
                     "storageAccountType":"Standard_LRS"
                  }
               }
            },
            "networkProfile":{
               "networkInterfaces":[
                  {
                     "id":"[resourceId('Microsoft.Network/networkInterfaces',parameters('nicName'))]"
                  }
               ]
            }
         }
      }
   ]
}
```

### <a name="deploy-an-azure-vm-using-powershell"></a>Implantar uma VM do Azure usando o PowerShell

Copie e edite o script a seguir para fornecer valores para as variáveis `$storageaccount` e `$vhdUrl`. Executá-lo para criar um recurso de VM do Azure a partir de seu VHD generalizado existente.

```PowerShell
# storage account of existing generalized VHD
$storageaccount = "testwinrm11815"
# generalized VHD URL
$vhdUrl = "https://testwinrm11815.blob.core.windows.net/vhds/testvm1234562016651857.vhd"

echo "New-AzResourceGroupDeployment -Name "dplisvvm$postfix" -ResourceGroupName "$rgName" -TemplateFile "C:\certLocation\VHDtoImage.json" -userStorageAccountName "$storageaccount" -dnsNameForPublicIP "$vmName" -subscriptionId "$mysubid" -location "$location" -vmName "$vmName" -vaultName "$kvname" -vaultResourceGroup "$rgName" -certificateUrl
$objAzureKeyVaultSecret.Id -vhdUrl "$vhdUrl" -vmSize "Standard\_A2" -publicIPAddressName "myPublicIP1" -virtualNetworkName "myVNET1" -nicName "myNIC1" -adminUserName "isv" -adminPassword $pwd"

# deploying VM with existing VHD
New-AzResourceGroupDeployment -Name "dplisvvm$postfix" -ResourceGroupName "$rgName" -TemplateFile "C:\certLocation\VHDtoImage.json" -userStorageAccountName "$storageaccount" -dnsNameForPublicIP "$vmName" -subscriptionId "$mysubid" -location "$location" -vmName "$vmName" -vaultName "$kvname" -vaultResourceGroup "$rgName" -certificateUrl $objAzureKeyVaultSecret.Id -vhdUrl "$vhdUrl" -vmSize "Standard\_A2" -publicIPAddressName "myPublicIP1" -virtualNetworkName "myVNET1" -nicName "myNIC1" -adminUserName "isv" -adminPassword $pwd
```

## <a name="run-validations"></a>Executar as validações

Há duas maneiras de executar validações na imagem implantada.

### <a name="use-certification-test-tool-for-azure-certified"></a>Use a Ferramenta do teste de certificação para o Certificado Azure

#### <a name="download-and-run-the-certification-test-tool"></a>Baixar e executar a ferramenta de teste de certificação

A Ferramenta de Teste de Certificação para Certificado do Azure executa em uma máquina Windows local, mas testa uma VM do Linux ou do Windows baseado no Azure. Ela verifica se a imagem da VM do usuário pode ser usada com o Microsoft Azure, ou seja, se as diretrizes e os requisitos de preparação do VHD foram cumpridos.

1. Baixe e instale a [Ferramenta de Teste de Certificação para Certificado do Azure](https://www.microsoft.com/download/details.aspx?id=44299) mais recente.
2. Abra a ferramenta de certificação e, em seguida, clique em **Iniciar Novo Teste**.
3. Na tela Informações do Teste, insira um **Nome de Teste** para a execução de teste.
4. Selecione a plataforma para sua VM, **Windows Server** ou **Linux**. A escolha da plataforma impactará nas opções restantes.
5. Se a VM estiver usando esse serviço de banco de dados, marque a caixa de seleção **Testar para Banco de Dados SQL do Azure**.

#### <a name="connect-the-certification-tool-to-a-vm-image"></a>Conectar a ferramenta de certificação a uma imagem da VM

1. Selecione o modo Autenticação SSH: Autenticação de senha ou de arquivo de chave.
2. Se estiver usando a autenticação baseada em senha, insira valores para o **nome DNS da VM**, o **nome de usuário**e a **senha**. É possível alterar o número da Porta SSH padrão.

    :::image type="content" source="media/vm/azure-vm-cert-2.png" alt-text="Mostra a seleção de um modelo.&quot;:::

5. Cole o modelo JSON a seguir no editor e selecione **salvar**.

```JSON
{
    &quot;$schema&quot;: &quot;https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#&quot;,
    &quot;contentVersion&quot;: &quot;1.0.0.0&quot;,
    &quot;parameters&quot;: {
        &quot;userStorageAccountName&quot;: {
            &quot;type&quot;: &quot;String&quot;
        },
        &quot;userStorageContainerName&quot;: {
            &quot;defaultValue&quot;: &quot;vhds&quot;,
            &quot;type&quot;: &quot;String&quot;
        },
        &quot;dnsNameForPublicIP&quot;: {
            &quot;type&quot;: &quot;String&quot;
        },
        &quot;adminUserName&quot;: {
            &quot;defaultValue&quot;: &quot;isv&quot;,
            &quot;type&quot;: &quot;String&quot;
        },
        &quot;adminPassword&quot;: {
            &quot;defaultValue&quot;: &quot;&quot;,
            &quot;type&quot;: &quot;SecureString&quot;
        },
        &quot;osType&quot;: {
            &quot;defaultValue&quot;: &quot;windows&quot;,
            &quot;allowedValues&quot;: [
                &quot;windows&quot;,
                &quot;linux&quot;
            ],
            &quot;type&quot;: &quot;String&quot;
        },
        &quot;subscriptionId&quot;: {
            &quot;type&quot;: &quot;String&quot;
        },
        &quot;location&quot;: {
            &quot;type&quot;: &quot;String&quot;
        },
        &quot;vmSize&quot;: {
            &quot;type&quot;: &quot;String&quot;
        },
        &quot;publicIPAddressName&quot;: {
            &quot;type&quot;: &quot;String&quot;
        },
        &quot;vmName&quot;: {
            &quot;type&quot;: &quot;String&quot;
        },
        &quot;virtualNetworkName&quot;: {
            &quot;type&quot;: &quot;String&quot;
        },
        &quot;nicName&quot;: {
            &quot;type&quot;: &quot;String&quot;
        },
        &quot;vhdUrl&quot;: {
            &quot;type&quot;: &quot;String&quot;,
            &quot;metadata&quot;: {
                &quot;description&quot;: &quot;VHD Url...&quot;
            }
        }
    },
    &quot;variables&quot;: {
        &quot;addressPrefix&quot;: &quot;10.0.0.0/16&quot;,
        &quot;subnet1Name&quot;: &quot;Subnet-1&quot;,
        &quot;subnet2Name&quot;: &quot;Subnet-2&quot;,
        &quot;subnet1Prefix&quot;: &quot;10.0.0.0/24&quot;,
        &quot;subnet2Prefix&quot;: &quot;10.0.1.0/24&quot;,
        &quot;publicIPAddressType&quot;: &quot;Dynamic&quot;,
        &quot;vnetID&quot;: &quot;[resourceId('Microsoft.Network/virtualNetworks',parameters('virtualNetworkName'))]&quot;,
        &quot;subnet1Ref&quot;: &quot;[concat(variables('vnetID'),'/subnets/',variables('subnet1Name'))]&quot;,
        &quot;hostDNSNameScriptArgument&quot;: &quot;[concat(parameters('dnsNameForPublicIP'),'.',parameters('location'),'.cloudapp.azure.com')]&quot;,
        &quot;osDiskVhdName&quot;: &quot;[concat('http://',parameters('userStorageAccountName'),'.blob.core.windows.net/',parameters('userStorageContainerName'),'/',parameters('vmName'),'osDisk.vhd')]&quot;
    },
    &quot;resources&quot;: [
        {
            &quot;type&quot;: &quot;Microsoft.Network/publicIPAddresses&quot;,
            &quot;apiVersion&quot;: &quot;2015-06-15&quot;,
            &quot;name&quot;: &quot;[parameters('publicIPAddressName')]&quot;,
            &quot;location&quot;: &quot;[parameters('location')]&quot;,
            &quot;properties&quot;: {
                &quot;publicIPAllocationMethod&quot;: &quot;[variables('publicIPAddressType')]&quot;,
                &quot;dnsSettings&quot;: {
                    &quot;domainNameLabel&quot;: &quot;[parameters('dnsNameForPublicIP')]&quot;
                }
            }
        },
        {
            &quot;type&quot;: &quot;Microsoft.Network/virtualNetworks&quot;,
            &quot;apiVersion&quot;: &quot;2015-06-15&quot;,
            &quot;name&quot;: &quot;[parameters('virtualNetworkName')]&quot;,
            &quot;location&quot;: &quot;[parameters('location')]&quot;,
            &quot;properties&quot;: {
                &quot;addressSpace&quot;: {
                    &quot;addressPrefixes&quot;: [
                        &quot;[variables('addressPrefix')]&quot;
                    ]
                },
                &quot;subnets&quot;: [
                    {
                        &quot;name&quot;: &quot;[variables('subnet1Name')]&quot;,
                        &quot;properties&quot;: {
                            &quot;addressPrefix&quot;: &quot;[variables('subnet1Prefix')]&quot;
                        }
                    },
                    {
                        &quot;name&quot;: &quot;[variables('subnet2Name')]&quot;,
                        &quot;properties&quot;: {
                            &quot;addressPrefix&quot;: &quot;[variables('subnet2Prefix')]&quot;
                        }
                    }
                ]
            }
        },
        {
            &quot;type&quot;: &quot;Microsoft.Network/networkInterfaces&quot;,
            &quot;apiVersion&quot;: &quot;2015-06-15&quot;,
            &quot;name&quot;: &quot;[parameters('nicName')]&quot;,
            &quot;location&quot;: &quot;[parameters('location')]&quot;,
            &quot;dependsOn&quot;: [
                &quot;[concat('Microsoft.Network/publicIPAddresses/', parameters('publicIPAddressName'))]&quot;,
                &quot;[concat('Microsoft.Network/virtualNetworks/', parameters('virtualNetworkName'))]&quot;
            ],
            &quot;properties&quot;: {
                &quot;ipConfigurations&quot;: [
                    {
                        &quot;name&quot;: &quot;ipconfig1&quot;,
                        &quot;properties&quot;: {
                            &quot;privateIPAllocationMethod&quot;: &quot;Dynamic&quot;,
                            &quot;publicIPAddress&quot;: {
                                &quot;id&quot;: &quot;[resourceId('Microsoft.Network/publicIPAddresses',parameters('publicIPAddressName'))]&quot;
                            },
                            &quot;subnet&quot;: {
                                &quot;id&quot;: &quot;[variables('subnet1Ref')]&quot;
                            }
                        }
                    }
                ]
            }
        },
        {
            &quot;type&quot;: &quot;Microsoft.Compute/virtualMachines&quot;,
            &quot;apiVersion&quot;: &quot;2015-06-15&quot;,
            &quot;name&quot;: &quot;[parameters('vmName')]&quot;,
            &quot;location&quot;: &quot;[parameters('location')]&quot;,
            &quot;dependsOn&quot;: [
                &quot;[concat('Microsoft.Network/networkInterfaces/', parameters('nicName'))]&quot;
            ],
            &quot;properties&quot;: {
                &quot;hardwareProfile&quot;: {
                    &quot;vmSize&quot;: &quot;[parameters('vmSize')]&quot;
                },
                &quot;osProfile&quot;: {
                    &quot;computername&quot;: &quot;[parameters('vmName')]&quot;,
                    &quot;adminUsername&quot;: &quot;[parameters('adminUsername')]&quot;,
                    &quot;adminPassword&quot;: &quot;[parameters('adminPassword')]&quot;
                },
                &quot;storageProfile&quot;: {
                    &quot;osDisk&quot;: {
                        &quot;name&quot;: &quot;[concat(parameters('vmName'),'-osDisk')]&quot;,
                        &quot;osType&quot;: &quot;[parameters('osType')]&quot;,
                        &quot;caching&quot;: &quot;ReadWrite&quot;,
                        &quot;image&quot;: {
                            &quot;uri&quot;: &quot;[parameters('vhdUrl')]&quot;
                        },
                        &quot;vhd&quot;: {
                            &quot;uri&quot;: &quot;[variables('osDiskVhdName')]&quot;
                        },
                        &quot;createOption&quot;: &quot;FromImage&quot;
                    }
                },
                &quot;networkProfile&quot;: {
                    &quot;networkInterfaces&quot;: [
                        {
                            &quot;id&quot;: &quot;[resourceId('Microsoft.Network/networkInterfaces',parameters('nicName'))]&quot;
                        }
                    ]
                }
            }
        },
        {
            &quot;type&quot;: &quot;Microsoft.Compute/virtualMachines/extensions&quot;,
            &quot;apiVersion&quot;: &quot;2015-06-15&quot;,
            &quot;name&quot;: &quot;[concat(parameters('vmName'),'/WinRMCustomScriptExtension')]&quot;,
            &quot;location&quot;: &quot;[parameters('location')]&quot;,
            &quot;dependsOn&quot;: [
                &quot;[concat('Microsoft.Compute/virtualMachines/', parameters('vmName'))]&quot;
            ],
            &quot;properties&quot;: {
                &quot;publisher&quot;: &quot;Microsoft.Compute&quot;,
                &quot;type&quot;: &quot;CustomScriptExtension&quot;,
                &quot;typeHandlerVersion&quot;: &quot;1.4&quot;,
                &quot;settings&quot;: {
                    &quot;fileUris&quot;: [
                        &quot;https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-winrm-windows/ConfigureWinRM.ps1&quot;,
                        &quot;https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-winrm-windows/makecert.exe&quot;,
                        &quot;https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-winrm-windows/winrmconf.cmd&quot;
                    ],
                    &quot;commandToExecute&quot;: &quot;[concat('powershell -ExecutionPolicy Unrestricted -file ConfigureWinRM.ps1 ',variables('hostDNSNameScriptArgument'))]":::

3. Se estiver usando a autenticação baseada em arquivo, insira valores para o Nome DNS da VM, Nome de usuário e local da Chave Privada. Você também pode incluir uma frase secreta ou alterar o número da Porta SSH.
4. Insira o nome DNS totalmente qualificado da VM (por exemplo, MyVMName.Cloudapp.net).
5. Insira o **nome de usuário** e a **senha**.

    :::image type="content" source="media/vm/azure-vm-cert-4.png" alt-text="Mostra a seleção de um modelo.&quot;:::

5. Cole o modelo JSON a seguir no editor e selecione **salvar**.

```JSON
{
    &quot;$schema&quot;: &quot;https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#&quot;,
    &quot;contentVersion&quot;: &quot;1.0.0.0&quot;,
    &quot;parameters&quot;: {
        &quot;userStorageAccountName&quot;: {
            &quot;type&quot;: &quot;String&quot;
        },
        &quot;userStorageContainerName&quot;: {
            &quot;defaultValue&quot;: &quot;vhds&quot;,
            &quot;type&quot;: &quot;String&quot;
        },
        &quot;dnsNameForPublicIP&quot;: {
            &quot;type&quot;: &quot;String&quot;
        },
        &quot;adminUserName&quot;: {
            &quot;defaultValue&quot;: &quot;isv&quot;,
            &quot;type&quot;: &quot;String&quot;
        },
        &quot;adminPassword&quot;: {
            &quot;defaultValue&quot;: &quot;&quot;,
            &quot;type&quot;: &quot;SecureString&quot;
        },
        &quot;osType&quot;: {
            &quot;defaultValue&quot;: &quot;windows&quot;,
            &quot;allowedValues&quot;: [
                &quot;windows&quot;,
                &quot;linux&quot;
            ],
            &quot;type&quot;: &quot;String&quot;
        },
        &quot;subscriptionId&quot;: {
            &quot;type&quot;: &quot;String&quot;
        },
        &quot;location&quot;: {
            &quot;type&quot;: &quot;String&quot;
        },
        &quot;vmSize&quot;: {
            &quot;type&quot;: &quot;String&quot;
        },
        &quot;publicIPAddressName&quot;: {
            &quot;type&quot;: &quot;String&quot;
        },
        &quot;vmName&quot;: {
            &quot;type&quot;: &quot;String&quot;
        },
        &quot;virtualNetworkName&quot;: {
            &quot;type&quot;: &quot;String&quot;
        },
        &quot;nicName&quot;: {
            &quot;type&quot;: &quot;String&quot;
        },
        &quot;vhdUrl&quot;: {
            &quot;type&quot;: &quot;String&quot;,
            &quot;metadata&quot;: {
                &quot;description&quot;: &quot;VHD Url...&quot;
            }
        }
    },
    &quot;variables&quot;: {
        &quot;addressPrefix&quot;: &quot;10.0.0.0/16&quot;,
        &quot;subnet1Name&quot;: &quot;Subnet-1&quot;,
        &quot;subnet2Name&quot;: &quot;Subnet-2&quot;,
        &quot;subnet1Prefix&quot;: &quot;10.0.0.0/24&quot;,
        &quot;subnet2Prefix&quot;: &quot;10.0.1.0/24&quot;,
        &quot;publicIPAddressType&quot;: &quot;Dynamic&quot;,
        &quot;vnetID&quot;: &quot;[resourceId('Microsoft.Network/virtualNetworks',parameters('virtualNetworkName'))]&quot;,
        &quot;subnet1Ref&quot;: &quot;[concat(variables('vnetID'),'/subnets/',variables('subnet1Name'))]&quot;,
        &quot;hostDNSNameScriptArgument&quot;: &quot;[concat(parameters('dnsNameForPublicIP'),'.',parameters('location'),'.cloudapp.azure.com')]&quot;,
        &quot;osDiskVhdName&quot;: &quot;[concat('http://',parameters('userStorageAccountName'),'.blob.core.windows.net/',parameters('userStorageContainerName'),'/',parameters('vmName'),'osDisk.vhd')]&quot;
    },
    &quot;resources&quot;: [
        {
            &quot;type&quot;: &quot;Microsoft.Network/publicIPAddresses&quot;,
            &quot;apiVersion&quot;: &quot;2015-06-15&quot;,
            &quot;name&quot;: &quot;[parameters('publicIPAddressName')]&quot;,
            &quot;location&quot;: &quot;[parameters('location')]&quot;,
            &quot;properties&quot;: {
                &quot;publicIPAllocationMethod&quot;: &quot;[variables('publicIPAddressType')]&quot;,
                &quot;dnsSettings&quot;: {
                    &quot;domainNameLabel&quot;: &quot;[parameters('dnsNameForPublicIP')]&quot;
                }
            }
        },
        {
            &quot;type&quot;: &quot;Microsoft.Network/virtualNetworks&quot;,
            &quot;apiVersion&quot;: &quot;2015-06-15&quot;,
            &quot;name&quot;: &quot;[parameters('virtualNetworkName')]&quot;,
            &quot;location&quot;: &quot;[parameters('location')]&quot;,
            &quot;properties&quot;: {
                &quot;addressSpace&quot;: {
                    &quot;addressPrefixes&quot;: [
                        &quot;[variables('addressPrefix')]&quot;
                    ]
                },
                &quot;subnets&quot;: [
                    {
                        &quot;name&quot;: &quot;[variables('subnet1Name')]&quot;,
                        &quot;properties&quot;: {
                            &quot;addressPrefix&quot;: &quot;[variables('subnet1Prefix')]&quot;
                        }
                    },
                    {
                        &quot;name&quot;: &quot;[variables('subnet2Name')]&quot;,
                        &quot;properties&quot;: {
                            &quot;addressPrefix&quot;: &quot;[variables('subnet2Prefix')]&quot;
                        }
                    }
                ]
            }
        },
        {
            &quot;type&quot;: &quot;Microsoft.Network/networkInterfaces&quot;,
            &quot;apiVersion&quot;: &quot;2015-06-15&quot;,
            &quot;name&quot;: &quot;[parameters('nicName')]&quot;,
            &quot;location&quot;: &quot;[parameters('location')]&quot;,
            &quot;dependsOn&quot;: [
                &quot;[concat('Microsoft.Network/publicIPAddresses/', parameters('publicIPAddressName'))]&quot;,
                &quot;[concat('Microsoft.Network/virtualNetworks/', parameters('virtualNetworkName'))]&quot;
            ],
            &quot;properties&quot;: {
                &quot;ipConfigurations&quot;: [
                    {
                        &quot;name&quot;: &quot;ipconfig1&quot;,
                        &quot;properties&quot;: {
                            &quot;privateIPAllocationMethod&quot;: &quot;Dynamic&quot;,
                            &quot;publicIPAddress&quot;: {
                                &quot;id&quot;: &quot;[resourceId('Microsoft.Network/publicIPAddresses',parameters('publicIPAddressName'))]&quot;
                            },
                            &quot;subnet&quot;: {
                                &quot;id&quot;: &quot;[variables('subnet1Ref')]&quot;
                            }
                        }
                    }
                ]
            }
        },
        {
            &quot;type&quot;: &quot;Microsoft.Compute/virtualMachines&quot;,
            &quot;apiVersion&quot;: &quot;2015-06-15&quot;,
            &quot;name&quot;: &quot;[parameters('vmName')]&quot;,
            &quot;location&quot;: &quot;[parameters('location')]&quot;,
            &quot;dependsOn&quot;: [
                &quot;[concat('Microsoft.Network/networkInterfaces/', parameters('nicName'))]&quot;
            ],
            &quot;properties&quot;: {
                &quot;hardwareProfile&quot;: {
                    &quot;vmSize&quot;: &quot;[parameters('vmSize')]&quot;
                },
                &quot;osProfile&quot;: {
                    &quot;computername&quot;: &quot;[parameters('vmName')]&quot;,
                    &quot;adminUsername&quot;: &quot;[parameters('adminUsername')]&quot;,
                    &quot;adminPassword&quot;: &quot;[parameters('adminPassword')]&quot;
                },
                &quot;storageProfile&quot;: {
                    &quot;osDisk&quot;: {
                        &quot;name&quot;: &quot;[concat(parameters('vmName'),'-osDisk')]&quot;,
                        &quot;osType&quot;: &quot;[parameters('osType')]&quot;,
                        &quot;caching&quot;: &quot;ReadWrite&quot;,
                        &quot;image&quot;: {
                            &quot;uri&quot;: &quot;[parameters('vhdUrl')]&quot;
                        },
                        &quot;vhd&quot;: {
                            &quot;uri&quot;: &quot;[variables('osDiskVhdName')]&quot;
                        },
                        &quot;createOption&quot;: &quot;FromImage&quot;
                    }
                },
                &quot;networkProfile&quot;: {
                    &quot;networkInterfaces&quot;: [
                        {
                            &quot;id&quot;: &quot;[resourceId('Microsoft.Network/networkInterfaces',parameters('nicName'))]&quot;
                        }
                    ]
                }
            }
        },
        {
            &quot;type&quot;: &quot;Microsoft.Compute/virtualMachines/extensions&quot;,
            &quot;apiVersion&quot;: &quot;2015-06-15&quot;,
            &quot;name&quot;: &quot;[concat(parameters('vmName'),'/WinRMCustomScriptExtension')]&quot;,
            &quot;location&quot;: &quot;[parameters('location')]&quot;,
            &quot;dependsOn&quot;: [
                &quot;[concat('Microsoft.Compute/virtualMachines/', parameters('vmName'))]&quot;
            ],
            &quot;properties&quot;: {
                &quot;publisher&quot;: &quot;Microsoft.Compute&quot;,
                &quot;type&quot;: &quot;CustomScriptExtension&quot;,
                &quot;typeHandlerVersion&quot;: &quot;1.4&quot;,
                &quot;settings&quot;: {
                    &quot;fileUris&quot;: [
                        &quot;https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-winrm-windows/ConfigureWinRM.ps1&quot;,
                        &quot;https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-winrm-windows/makecert.exe&quot;,
                        &quot;https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-winrm-windows/winrmconf.cmd&quot;
                    ],
                    &quot;commandToExecute&quot;: &quot;[concat('powershell -ExecutionPolicy Unrestricted -file ConfigureWinRM.ps1 ',variables('hostDNSNameScriptArgument'))]":::

6. Selecione **Avançar**.

#### <a name="run-a-certification-test"></a>Executar um teste de certificação

Depois de obter os valores de parâmetro para sua imagem de VM na ferramenta de certificação, selecione testar conexão para criar uma conexão válida com sua VM. Após verificar a conexão, selecione **Avançar** para iniciar o teste. Quando o teste é concluído, os resultados são exibidos em uma tabela. A coluna status mostra (aprovação/falha/aviso) para cada teste. Se algum dos testes falhar, a imagem não será certificada. Nesse caso, revise os requisitos e as mensagens de falha, faça as alterações sugeridas e execute novamente o teste.

Após a conclusão do teste automatizado, forneça informações adicionais sobre a imagem da VM nas duas guias da tela Questionário, Avaliação geral e Personalização do kernel e, em seguida, selecione Próximo.

A última tela permite que você forneça mais informações, como informações de acesso SSH para uma imagem de VM do Linux e uma explicação para quaisquer avaliações com falha se você estiver procurando por exceções.

Por fim, selecione Gerar Relatório para baixar os resultados do teste e os arquivos de log dos casos de teste juntamente com as respostas ao questionário. 
> [!Note]
> Alguns Publicadores têm cenários em que as VMs precisam ser bloqueadas, pois têm software como firewalls instalados na VM. Nesse caso, baixe a [ferramenta de teste certificada](https://aka.ms/AzureCertificationTestTool) aqui e envie o relatório no [suporte](https://aka.ms/marketplacepublishersupport)do Partner Center.

## <a name="how-to-use-powershell-to-consume-the-self-test-api"></a>Como usar o PowerShell para consumir a API de Self-Test

### <a name="on-linux-os"></a>No SO Linux

Chame a API no PowerShell:

1. Use o comando Invoke-WebRequest para chamar a API.
2. O método é Post e o tipo de conteúdo é JSON, conforme mostrado no exemplo de código e na captura de tela a seguir.
3. Especifique parâmetros de corpo no formato JSON.

Este exemplo a seguir mostra uma chamada do PowerShell para a API:

```POWERSHELL
$accesstoken = "token"
$headers = @{ "Authorization" = "Bearer $accesstoken" }
$DNSName = "<Machine DNS Name>"
$UserName = "<User ID>"
$Password = "<Password>"
$OS = "Linux"
$PortNo = "22"
$CompanyName = "ABCD"
$AppID = "<Application ID>"
$TenantId = "<Tenant ID>"

$body = @{
   "DNSName" = $DNSName
   "UserName" = $UserName
   "Password" = $Password
   "OS" = $OS
   "PortNo" = $PortNo
   "CompanyName" = $CompanyName
   "AppID" = $AppID
   "TenantId" = $TenantId
} | ConvertTo-Json

$body

$uri = "URL"

$res = (Invoke-WebRequest -Method "Post" -Uri $uri -Body $body -ContentType "application/json" -Headers $headers).Content
```

<br>Aqui está um exemplo de como chamar a API no PowerShell:

[![Exemplo de tela para chamar a API no PowerShell.](media/vm/call-api-in-powershell.png)](media/vm/call-api-in-powershell.png#lightbox)

<br>Usando o exemplo anterior, você poderá recuperar o JSON e analisá-lo para obter os seguintes detalhes:

```PowerShell
$resVar = $res | ConvertFrom-Json
$actualresult = $resVar.Response | ConvertFrom-Json

Write-Host "OSName: $($actualresult.OSName)"
Write-Host "OSVersion: $($actualresult.OSVersion)"
Write-Host "Overall Test Result: $($actualresult.TestResult)"

For ($i = 0; $i -lt $actualresult.Tests.Length; $i++) {
   Write-Host "TestID: $($actualresult.Tests[$i].TestID)"
   Write-Host "TestCaseName: $($actualresult.Tests[$i].TestCaseName)"
   Write-Host "Description: $($actualresult.Tests[$i].Description)"
   Write-Host "Result: $($actualresult.Tests[$i].Result)"
   Write-Host "ActualValue: $($actualresult.Tests[$i].ActualValue)"
}
```

<br>Esta tela de exemplo, que mostra `$res.Content` , mostra detalhes dos resultados do teste no formato JSON:

[![Exemplo de tela para chamar a API no PowerShell com detalhes de resultados de teste.](media/vm/call-api-in-powershell-details.png)](media/vm/call-api-in-powershell-details.png#lightbox)

<br>Aqui está um exemplo de resultados de teste JSON exibidos em um visualizador JSON online (como [código Beautify](https://codebeautify.org/jsonviewer) ou [Visualizador JSON](https://jsonformatter.org/json-viewer)).

![Mais resultados de teste em um visualizador JSON online.](media/vm/test-results-json-viewer-1.png)

### <a name="on-windows-os"></a>No sistema operacional Windows

Chame a API no PowerShell:

1. Use o comando Invoke-WebRequest para chamar a API.
2. O método é post e o tipo de conteúdo é JSON, conforme mostrado no exemplo de código e na tela de exemplo a seguir.
3. Crie os parâmetros de corpo no formato JSON.

Este exemplo de código mostra uma chamada do PowerShell para a API:

```PowerShell
$accesstoken = "Get token for your Client AAD App"
$headers = @{ "Authorization" = "Bearer $accesstoken" }
$Body = @{ 
   "DNSName" = "XXXX.westus.cloudapp.azure.com"
   "UserName" = "XXX"
   "Password" = "XXX@123456"
   "OS" = "Windows"
   "PortNo" = "5986"
   "CompanyName" = "ABCD"
   "AppID" = "XXXX-XXXX-XXXX"
   "TenantId" = "XXXX-XXXX-XXXX"
} | ConvertTo-Json

$res = Invoke-WebRequest -Method "Post" -Uri $uri -Body $Body -ContentType "application/json" –Headers $headers;
$Content = $res | ConvertFrom-Json
```

Essas telas de exemplo mostram o exemplo para chamar a API no PowerShell:

**Com chave SSH**:

 :::image type="content" source="media/vm/call-api-with-ssh-key.png" alt-text="Mostra a seleção de um modelo.&quot;:::

5. Cole o modelo JSON a seguir no editor e selecione **salvar**.

```JSON
{
    &quot;$schema&quot;: &quot;https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#&quot;,
    &quot;contentVersion&quot;: &quot;1.0.0.0&quot;,
    &quot;parameters&quot;: {
        &quot;userStorageAccountName&quot;: {
            &quot;type&quot;: &quot;String&quot;
        },
        &quot;userStorageContainerName&quot;: {
            &quot;defaultValue&quot;: &quot;vhds&quot;,
            &quot;type&quot;: &quot;String&quot;
        },
        &quot;dnsNameForPublicIP&quot;: {
            &quot;type&quot;: &quot;String&quot;
        },
        &quot;adminUserName&quot;: {
            &quot;defaultValue&quot;: &quot;isv&quot;,
            &quot;type&quot;: &quot;String&quot;
        },
        &quot;adminPassword&quot;: {
            &quot;defaultValue&quot;: &quot;&quot;,
            &quot;type&quot;: &quot;SecureString&quot;
        },
        &quot;osType&quot;: {
            &quot;defaultValue&quot;: &quot;windows&quot;,
            &quot;allowedValues&quot;: [
                &quot;windows&quot;,
                &quot;linux&quot;
            ],
            &quot;type&quot;: &quot;String&quot;
        },
        &quot;subscriptionId&quot;: {
            &quot;type&quot;: &quot;String&quot;
        },
        &quot;location&quot;: {
            &quot;type&quot;: &quot;String&quot;
        },
        &quot;vmSize&quot;: {
            &quot;type&quot;: &quot;String&quot;
        },
        &quot;publicIPAddressName&quot;: {
            &quot;type&quot;: &quot;String&quot;
        },
        &quot;vmName&quot;: {
            &quot;type&quot;: &quot;String&quot;
        },
        &quot;virtualNetworkName&quot;: {
            &quot;type&quot;: &quot;String&quot;
        },
        &quot;nicName&quot;: {
            &quot;type&quot;: &quot;String&quot;
        },
        &quot;vhdUrl&quot;: {
            &quot;type&quot;: &quot;String&quot;,
            &quot;metadata&quot;: {
                &quot;description&quot;: &quot;VHD Url...&quot;
            }
        }
    },
    &quot;variables&quot;: {
        &quot;addressPrefix&quot;: &quot;10.0.0.0/16&quot;,
        &quot;subnet1Name&quot;: &quot;Subnet-1&quot;,
        &quot;subnet2Name&quot;: &quot;Subnet-2&quot;,
        &quot;subnet1Prefix&quot;: &quot;10.0.0.0/24&quot;,
        &quot;subnet2Prefix&quot;: &quot;10.0.1.0/24&quot;,
        &quot;publicIPAddressType&quot;: &quot;Dynamic&quot;,
        &quot;vnetID&quot;: &quot;[resourceId('Microsoft.Network/virtualNetworks',parameters('virtualNetworkName'))]&quot;,
        &quot;subnet1Ref&quot;: &quot;[concat(variables('vnetID'),'/subnets/',variables('subnet1Name'))]&quot;,
        &quot;hostDNSNameScriptArgument&quot;: &quot;[concat(parameters('dnsNameForPublicIP'),'.',parameters('location'),'.cloudapp.azure.com')]&quot;,
        &quot;osDiskVhdName&quot;: &quot;[concat('http://',parameters('userStorageAccountName'),'.blob.core.windows.net/',parameters('userStorageContainerName'),'/',parameters('vmName'),'osDisk.vhd')]&quot;
    },
    &quot;resources&quot;: [
        {
            &quot;type&quot;: &quot;Microsoft.Network/publicIPAddresses&quot;,
            &quot;apiVersion&quot;: &quot;2015-06-15&quot;,
            &quot;name&quot;: &quot;[parameters('publicIPAddressName')]&quot;,
            &quot;location&quot;: &quot;[parameters('location')]&quot;,
            &quot;properties&quot;: {
                &quot;publicIPAllocationMethod&quot;: &quot;[variables('publicIPAddressType')]&quot;,
                &quot;dnsSettings&quot;: {
                    &quot;domainNameLabel&quot;: &quot;[parameters('dnsNameForPublicIP')]&quot;
                }
            }
        },
        {
            &quot;type&quot;: &quot;Microsoft.Network/virtualNetworks&quot;,
            &quot;apiVersion&quot;: &quot;2015-06-15&quot;,
            &quot;name&quot;: &quot;[parameters('virtualNetworkName')]&quot;,
            &quot;location&quot;: &quot;[parameters('location')]&quot;,
            &quot;properties&quot;: {
                &quot;addressSpace&quot;: {
                    &quot;addressPrefixes&quot;: [
                        &quot;[variables('addressPrefix')]&quot;
                    ]
                },
                &quot;subnets&quot;: [
                    {
                        &quot;name&quot;: &quot;[variables('subnet1Name')]&quot;,
                        &quot;properties&quot;: {
                            &quot;addressPrefix&quot;: &quot;[variables('subnet1Prefix')]&quot;
                        }
                    },
                    {
                        &quot;name&quot;: &quot;[variables('subnet2Name')]&quot;,
                        &quot;properties&quot;: {
                            &quot;addressPrefix&quot;: &quot;[variables('subnet2Prefix')]&quot;
                        }
                    }
                ]
            }
        },
        {
            &quot;type&quot;: &quot;Microsoft.Network/networkInterfaces&quot;,
            &quot;apiVersion&quot;: &quot;2015-06-15&quot;,
            &quot;name&quot;: &quot;[parameters('nicName')]&quot;,
            &quot;location&quot;: &quot;[parameters('location')]&quot;,
            &quot;dependsOn&quot;: [
                &quot;[concat('Microsoft.Network/publicIPAddresses/', parameters('publicIPAddressName'))]&quot;,
                &quot;[concat('Microsoft.Network/virtualNetworks/', parameters('virtualNetworkName'))]&quot;
            ],
            &quot;properties&quot;: {
                &quot;ipConfigurations&quot;: [
                    {
                        &quot;name&quot;: &quot;ipconfig1&quot;,
                        &quot;properties&quot;: {
                            &quot;privateIPAllocationMethod&quot;: &quot;Dynamic&quot;,
                            &quot;publicIPAddress&quot;: {
                                &quot;id&quot;: &quot;[resourceId('Microsoft.Network/publicIPAddresses',parameters('publicIPAddressName'))]&quot;
                            },
                            &quot;subnet&quot;: {
                                &quot;id&quot;: &quot;[variables('subnet1Ref')]&quot;
                            }
                        }
                    }
                ]
            }
        },
        {
            &quot;type&quot;: &quot;Microsoft.Compute/virtualMachines&quot;,
            &quot;apiVersion&quot;: &quot;2015-06-15&quot;,
            &quot;name&quot;: &quot;[parameters('vmName')]&quot;,
            &quot;location&quot;: &quot;[parameters('location')]&quot;,
            &quot;dependsOn&quot;: [
                &quot;[concat('Microsoft.Network/networkInterfaces/', parameters('nicName'))]&quot;
            ],
            &quot;properties&quot;: {
                &quot;hardwareProfile&quot;: {
                    &quot;vmSize&quot;: &quot;[parameters('vmSize')]&quot;
                },
                &quot;osProfile&quot;: {
                    &quot;computername&quot;: &quot;[parameters('vmName')]&quot;,
                    &quot;adminUsername&quot;: &quot;[parameters('adminUsername')]&quot;,
                    &quot;adminPassword&quot;: &quot;[parameters('adminPassword')]&quot;
                },
                &quot;storageProfile&quot;: {
                    &quot;osDisk&quot;: {
                        &quot;name&quot;: &quot;[concat(parameters('vmName'),'-osDisk')]&quot;,
                        &quot;osType&quot;: &quot;[parameters('osType')]&quot;,
                        &quot;caching&quot;: &quot;ReadWrite&quot;,
                        &quot;image&quot;: {
                            &quot;uri&quot;: &quot;[parameters('vhdUrl')]&quot;
                        },
                        &quot;vhd&quot;: {
                            &quot;uri&quot;: &quot;[variables('osDiskVhdName')]&quot;
                        },
                        &quot;createOption&quot;: &quot;FromImage&quot;
                    }
                },
                &quot;networkProfile&quot;: {
                    &quot;networkInterfaces&quot;: [
                        {
                            &quot;id&quot;: &quot;[resourceId('Microsoft.Network/networkInterfaces',parameters('nicName'))]&quot;
                        }
                    ]
                }
            }
        },
        {
            &quot;type&quot;: &quot;Microsoft.Compute/virtualMachines/extensions&quot;,
            &quot;apiVersion&quot;: &quot;2015-06-15&quot;,
            &quot;name&quot;: &quot;[concat(parameters('vmName'),'/WinRMCustomScriptExtension')]&quot;,
            &quot;location&quot;: &quot;[parameters('location')]&quot;,
            &quot;dependsOn&quot;: [
                &quot;[concat('Microsoft.Compute/virtualMachines/', parameters('vmName'))]&quot;
            ],
            &quot;properties&quot;: {
                &quot;publisher&quot;: &quot;Microsoft.Compute&quot;,
                &quot;type&quot;: &quot;CustomScriptExtension&quot;,
                &quot;typeHandlerVersion&quot;: &quot;1.4&quot;,
                &quot;settings&quot;: {
                    &quot;fileUris&quot;: [
                        &quot;https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-winrm-windows/ConfigureWinRM.ps1&quot;,
                        &quot;https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-winrm-windows/makecert.exe&quot;,
                        &quot;https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-winrm-windows/winrmconf.cmd&quot;
                    ],
                    &quot;commandToExecute&quot;: &quot;[concat('powershell -ExecutionPolicy Unrestricted -file ConfigureWinRM.ps1 ',variables('hostDNSNameScriptArgument'))]":::

**Com a senha**:

 :::image type="content" source="media/vm/call-api-with-password.png" alt-text="Mostra a seleção de um modelo.&quot;:::

5. Cole o modelo JSON a seguir no editor e selecione **salvar**.

```JSON
{
    &quot;$schema&quot;: &quot;https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#&quot;,
    &quot;contentVersion&quot;: &quot;1.0.0.0&quot;,
    &quot;parameters&quot;: {
        &quot;userStorageAccountName&quot;: {
            &quot;type&quot;: &quot;String&quot;
        },
        &quot;userStorageContainerName&quot;: {
            &quot;defaultValue&quot;: &quot;vhds&quot;,
            &quot;type&quot;: &quot;String&quot;
        },
        &quot;dnsNameForPublicIP&quot;: {
            &quot;type&quot;: &quot;String&quot;
        },
        &quot;adminUserName&quot;: {
            &quot;defaultValue&quot;: &quot;isv&quot;,
            &quot;type&quot;: &quot;String&quot;
        },
        &quot;adminPassword&quot;: {
            &quot;defaultValue&quot;: &quot;&quot;,
            &quot;type&quot;: &quot;SecureString&quot;
        },
        &quot;osType&quot;: {
            &quot;defaultValue&quot;: &quot;windows&quot;,
            &quot;allowedValues&quot;: [
                &quot;windows&quot;,
                &quot;linux&quot;
            ],
            &quot;type&quot;: &quot;String&quot;
        },
        &quot;subscriptionId&quot;: {
            &quot;type&quot;: &quot;String&quot;
        },
        &quot;location&quot;: {
            &quot;type&quot;: &quot;String&quot;
        },
        &quot;vmSize&quot;: {
            &quot;type&quot;: &quot;String&quot;
        },
        &quot;publicIPAddressName&quot;: {
            &quot;type&quot;: &quot;String&quot;
        },
        &quot;vmName&quot;: {
            &quot;type&quot;: &quot;String&quot;
        },
        &quot;virtualNetworkName&quot;: {
            &quot;type&quot;: &quot;String&quot;
        },
        &quot;nicName&quot;: {
            &quot;type&quot;: &quot;String&quot;
        },
        &quot;vhdUrl&quot;: {
            &quot;type&quot;: &quot;String&quot;,
            &quot;metadata&quot;: {
                &quot;description&quot;: &quot;VHD Url...&quot;
            }
        }
    },
    &quot;variables&quot;: {
        &quot;addressPrefix&quot;: &quot;10.0.0.0/16&quot;,
        &quot;subnet1Name&quot;: &quot;Subnet-1&quot;,
        &quot;subnet2Name&quot;: &quot;Subnet-2&quot;,
        &quot;subnet1Prefix&quot;: &quot;10.0.0.0/24&quot;,
        &quot;subnet2Prefix&quot;: &quot;10.0.1.0/24&quot;,
        &quot;publicIPAddressType&quot;: &quot;Dynamic&quot;,
        &quot;vnetID&quot;: &quot;[resourceId('Microsoft.Network/virtualNetworks',parameters('virtualNetworkName'))]&quot;,
        &quot;subnet1Ref&quot;: &quot;[concat(variables('vnetID'),'/subnets/',variables('subnet1Name'))]&quot;,
        &quot;hostDNSNameScriptArgument&quot;: &quot;[concat(parameters('dnsNameForPublicIP'),'.',parameters('location'),'.cloudapp.azure.com')]&quot;,
        &quot;osDiskVhdName&quot;: &quot;[concat('http://',parameters('userStorageAccountName'),'.blob.core.windows.net/',parameters('userStorageContainerName'),'/',parameters('vmName'),'osDisk.vhd')]&quot;
    },
    &quot;resources&quot;: [
        {
            &quot;type&quot;: &quot;Microsoft.Network/publicIPAddresses&quot;,
            &quot;apiVersion&quot;: &quot;2015-06-15&quot;,
            &quot;name&quot;: &quot;[parameters('publicIPAddressName')]&quot;,
            &quot;location&quot;: &quot;[parameters('location')]&quot;,
            &quot;properties&quot;: {
                &quot;publicIPAllocationMethod&quot;: &quot;[variables('publicIPAddressType')]&quot;,
                &quot;dnsSettings&quot;: {
                    &quot;domainNameLabel&quot;: &quot;[parameters('dnsNameForPublicIP')]&quot;
                }
            }
        },
        {
            &quot;type&quot;: &quot;Microsoft.Network/virtualNetworks&quot;,
            &quot;apiVersion&quot;: &quot;2015-06-15&quot;,
            &quot;name&quot;: &quot;[parameters('virtualNetworkName')]&quot;,
            &quot;location&quot;: &quot;[parameters('location')]&quot;,
            &quot;properties&quot;: {
                &quot;addressSpace&quot;: {
                    &quot;addressPrefixes&quot;: [
                        &quot;[variables('addressPrefix')]&quot;
                    ]
                },
                &quot;subnets&quot;: [
                    {
                        &quot;name&quot;: &quot;[variables('subnet1Name')]&quot;,
                        &quot;properties&quot;: {
                            &quot;addressPrefix&quot;: &quot;[variables('subnet1Prefix')]&quot;
                        }
                    },
                    {
                        &quot;name&quot;: &quot;[variables('subnet2Name')]&quot;,
                        &quot;properties&quot;: {
                            &quot;addressPrefix&quot;: &quot;[variables('subnet2Prefix')]&quot;
                        }
                    }
                ]
            }
        },
        {
            &quot;type&quot;: &quot;Microsoft.Network/networkInterfaces&quot;,
            &quot;apiVersion&quot;: &quot;2015-06-15&quot;,
            &quot;name&quot;: &quot;[parameters('nicName')]&quot;,
            &quot;location&quot;: &quot;[parameters('location')]&quot;,
            &quot;dependsOn&quot;: [
                &quot;[concat('Microsoft.Network/publicIPAddresses/', parameters('publicIPAddressName'))]&quot;,
                &quot;[concat('Microsoft.Network/virtualNetworks/', parameters('virtualNetworkName'))]&quot;
            ],
            &quot;properties&quot;: {
                &quot;ipConfigurations&quot;: [
                    {
                        &quot;name&quot;: &quot;ipconfig1&quot;,
                        &quot;properties&quot;: {
                            &quot;privateIPAllocationMethod&quot;: &quot;Dynamic&quot;,
                            &quot;publicIPAddress&quot;: {
                                &quot;id&quot;: &quot;[resourceId('Microsoft.Network/publicIPAddresses',parameters('publicIPAddressName'))]&quot;
                            },
                            &quot;subnet&quot;: {
                                &quot;id&quot;: &quot;[variables('subnet1Ref')]&quot;
                            }
                        }
                    }
                ]
            }
        },
        {
            &quot;type&quot;: &quot;Microsoft.Compute/virtualMachines&quot;,
            &quot;apiVersion&quot;: &quot;2015-06-15&quot;,
            &quot;name&quot;: &quot;[parameters('vmName')]&quot;,
            &quot;location&quot;: &quot;[parameters('location')]&quot;,
            &quot;dependsOn&quot;: [
                &quot;[concat('Microsoft.Network/networkInterfaces/', parameters('nicName'))]&quot;
            ],
            &quot;properties&quot;: {
                &quot;hardwareProfile&quot;: {
                    &quot;vmSize&quot;: &quot;[parameters('vmSize')]&quot;
                },
                &quot;osProfile&quot;: {
                    &quot;computername&quot;: &quot;[parameters('vmName')]&quot;,
                    &quot;adminUsername&quot;: &quot;[parameters('adminUsername')]&quot;,
                    &quot;adminPassword&quot;: &quot;[parameters('adminPassword')]&quot;
                },
                &quot;storageProfile&quot;: {
                    &quot;osDisk&quot;: {
                        &quot;name&quot;: &quot;[concat(parameters('vmName'),'-osDisk')]&quot;,
                        &quot;osType&quot;: &quot;[parameters('osType')]&quot;,
                        &quot;caching&quot;: &quot;ReadWrite&quot;,
                        &quot;image&quot;: {
                            &quot;uri&quot;: &quot;[parameters('vhdUrl')]&quot;
                        },
                        &quot;vhd&quot;: {
                            &quot;uri&quot;: &quot;[variables('osDiskVhdName')]&quot;
                        },
                        &quot;createOption&quot;: &quot;FromImage&quot;
                    }
                },
                &quot;networkProfile&quot;: {
                    &quot;networkInterfaces&quot;: [
                        {
                            &quot;id&quot;: &quot;[resourceId('Microsoft.Network/networkInterfaces',parameters('nicName'))]&quot;
                        }
                    ]
                }
            }
        },
        {
            &quot;type&quot;: &quot;Microsoft.Compute/virtualMachines/extensions&quot;,
            &quot;apiVersion&quot;: &quot;2015-06-15&quot;,
            &quot;name&quot;: &quot;[concat(parameters('vmName'),'/WinRMCustomScriptExtension')]&quot;,
            &quot;location&quot;: &quot;[parameters('location')]&quot;,
            &quot;dependsOn&quot;: [
                &quot;[concat('Microsoft.Compute/virtualMachines/', parameters('vmName'))]&quot;
            ],
            &quot;properties&quot;: {
                &quot;publisher&quot;: &quot;Microsoft.Compute&quot;,
                &quot;type&quot;: &quot;CustomScriptExtension&quot;,
                &quot;typeHandlerVersion&quot;: &quot;1.4&quot;,
                &quot;settings&quot;: {
                    &quot;fileUris&quot;: [
                        &quot;https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-winrm-windows/ConfigureWinRM.ps1&quot;,
                        &quot;https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-winrm-windows/makecert.exe&quot;,
                        &quot;https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-winrm-windows/winrmconf.cmd&quot;
                    ],
                    &quot;commandToExecute&quot;: &quot;[concat('powershell -ExecutionPolicy Unrestricted -file ConfigureWinRM.ps1 ',variables('hostDNSNameScriptArgument'))]":::

<br>Usando o exemplo anterior, você poderá recuperar o JSON e analisá-lo para obter os seguintes detalhes:

```PowerShell
$resVar = $res | ConvertFrom-Json
$actualresult = $resVar.Response | ConvertFrom-Json

Write-Host "OSName: $($actualresult.OSName)"
Write-Host "OSVersion: $($actualresult.OSVersion)"
Write-Host "Overall Test Result: $($actualresult.TestResult)"

For ($i = 0; $i -lt $actualresult.Tests.Length; $i++) {
   Write-Host "TestID: $($actualresult.Tests[$i].TestID)"
   Write-Host "TestCaseName: $($actualresult.Tests[$i].TestCaseName)"
   Write-Host "Description: $($actualresult.Tests[$i].Description)"
   Write-Host "Result: $($actualresult.Tests[$i].Result)"
   Write-Host "ActualValue: $($actualresult.Tests[$i].ActualValue)"
}
```

<br>Essa tela, que mostra `$res.Content` , mostra os detalhes dos resultados do teste no formato JSON:

 :::image type="content" source="media/vm/test-results-json-format.png" alt-text="Mostra a seleção de um modelo.&quot;:::

5. Cole o modelo JSON a seguir no editor e selecione **salvar**.

```JSON
{
    &quot;$schema&quot;: &quot;https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#&quot;,
    &quot;contentVersion&quot;: &quot;1.0.0.0&quot;,
    &quot;parameters&quot;: {
        &quot;userStorageAccountName&quot;: {
            &quot;type&quot;: &quot;String&quot;
        },
        &quot;userStorageContainerName&quot;: {
            &quot;defaultValue&quot;: &quot;vhds&quot;,
            &quot;type&quot;: &quot;String&quot;
        },
        &quot;dnsNameForPublicIP&quot;: {
            &quot;type&quot;: &quot;String&quot;
        },
        &quot;adminUserName&quot;: {
            &quot;defaultValue&quot;: &quot;isv&quot;,
            &quot;type&quot;: &quot;String&quot;
        },
        &quot;adminPassword&quot;: {
            &quot;defaultValue&quot;: &quot;&quot;,
            &quot;type&quot;: &quot;SecureString&quot;
        },
        &quot;osType&quot;: {
            &quot;defaultValue&quot;: &quot;windows&quot;,
            &quot;allowedValues&quot;: [
                &quot;windows&quot;,
                &quot;linux&quot;
            ],
            &quot;type&quot;: &quot;String&quot;
        },
        &quot;subscriptionId&quot;: {
            &quot;type&quot;: &quot;String&quot;
        },
        &quot;location&quot;: {
            &quot;type&quot;: &quot;String&quot;
        },
        &quot;vmSize&quot;: {
            &quot;type&quot;: &quot;String&quot;
        },
        &quot;publicIPAddressName&quot;: {
            &quot;type&quot;: &quot;String&quot;
        },
        &quot;vmName&quot;: {
            &quot;type&quot;: &quot;String&quot;
        },
        &quot;virtualNetworkName&quot;: {
            &quot;type&quot;: &quot;String&quot;
        },
        &quot;nicName&quot;: {
            &quot;type&quot;: &quot;String&quot;
        },
        &quot;vhdUrl&quot;: {
            &quot;type&quot;: &quot;String&quot;,
            &quot;metadata&quot;: {
                &quot;description&quot;: &quot;VHD Url...&quot;
            }
        }
    },
    &quot;variables&quot;: {
        &quot;addressPrefix&quot;: &quot;10.0.0.0/16&quot;,
        &quot;subnet1Name&quot;: &quot;Subnet-1&quot;,
        &quot;subnet2Name&quot;: &quot;Subnet-2&quot;,
        &quot;subnet1Prefix&quot;: &quot;10.0.0.0/24&quot;,
        &quot;subnet2Prefix&quot;: &quot;10.0.1.0/24&quot;,
        &quot;publicIPAddressType&quot;: &quot;Dynamic&quot;,
        &quot;vnetID&quot;: &quot;[resourceId('Microsoft.Network/virtualNetworks',parameters('virtualNetworkName'))]&quot;,
        &quot;subnet1Ref&quot;: &quot;[concat(variables('vnetID'),'/subnets/',variables('subnet1Name'))]&quot;,
        &quot;hostDNSNameScriptArgument&quot;: &quot;[concat(parameters('dnsNameForPublicIP'),'.',parameters('location'),'.cloudapp.azure.com')]&quot;,
        &quot;osDiskVhdName&quot;: &quot;[concat('http://',parameters('userStorageAccountName'),'.blob.core.windows.net/',parameters('userStorageContainerName'),'/',parameters('vmName'),'osDisk.vhd')]&quot;
    },
    &quot;resources&quot;: [
        {
            &quot;type&quot;: &quot;Microsoft.Network/publicIPAddresses&quot;,
            &quot;apiVersion&quot;: &quot;2015-06-15&quot;,
            &quot;name&quot;: &quot;[parameters('publicIPAddressName')]&quot;,
            &quot;location&quot;: &quot;[parameters('location')]&quot;,
            &quot;properties&quot;: {
                &quot;publicIPAllocationMethod&quot;: &quot;[variables('publicIPAddressType')]&quot;,
                &quot;dnsSettings&quot;: {
                    &quot;domainNameLabel&quot;: &quot;[parameters('dnsNameForPublicIP')]&quot;
                }
            }
        },
        {
            &quot;type&quot;: &quot;Microsoft.Network/virtualNetworks&quot;,
            &quot;apiVersion&quot;: &quot;2015-06-15&quot;,
            &quot;name&quot;: &quot;[parameters('virtualNetworkName')]&quot;,
            &quot;location&quot;: &quot;[parameters('location')]&quot;,
            &quot;properties&quot;: {
                &quot;addressSpace&quot;: {
                    &quot;addressPrefixes&quot;: [
                        &quot;[variables('addressPrefix')]&quot;
                    ]
                },
                &quot;subnets&quot;: [
                    {
                        &quot;name&quot;: &quot;[variables('subnet1Name')]&quot;,
                        &quot;properties&quot;: {
                            &quot;addressPrefix&quot;: &quot;[variables('subnet1Prefix')]&quot;
                        }
                    },
                    {
                        &quot;name&quot;: &quot;[variables('subnet2Name')]&quot;,
                        &quot;properties&quot;: {
                            &quot;addressPrefix&quot;: &quot;[variables('subnet2Prefix')]&quot;
                        }
                    }
                ]
            }
        },
        {
            &quot;type&quot;: &quot;Microsoft.Network/networkInterfaces&quot;,
            &quot;apiVersion&quot;: &quot;2015-06-15&quot;,
            &quot;name&quot;: &quot;[parameters('nicName')]&quot;,
            &quot;location&quot;: &quot;[parameters('location')]&quot;,
            &quot;dependsOn&quot;: [
                &quot;[concat('Microsoft.Network/publicIPAddresses/', parameters('publicIPAddressName'))]&quot;,
                &quot;[concat('Microsoft.Network/virtualNetworks/', parameters('virtualNetworkName'))]&quot;
            ],
            &quot;properties&quot;: {
                &quot;ipConfigurations&quot;: [
                    {
                        &quot;name&quot;: &quot;ipconfig1&quot;,
                        &quot;properties&quot;: {
                            &quot;privateIPAllocationMethod&quot;: &quot;Dynamic&quot;,
                            &quot;publicIPAddress&quot;: {
                                &quot;id&quot;: &quot;[resourceId('Microsoft.Network/publicIPAddresses',parameters('publicIPAddressName'))]&quot;
                            },
                            &quot;subnet&quot;: {
                                &quot;id&quot;: &quot;[variables('subnet1Ref')]&quot;
                            }
                        }
                    }
                ]
            }
        },
        {
            &quot;type&quot;: &quot;Microsoft.Compute/virtualMachines&quot;,
            &quot;apiVersion&quot;: &quot;2015-06-15&quot;,
            &quot;name&quot;: &quot;[parameters('vmName')]&quot;,
            &quot;location&quot;: &quot;[parameters('location')]&quot;,
            &quot;dependsOn&quot;: [
                &quot;[concat('Microsoft.Network/networkInterfaces/', parameters('nicName'))]&quot;
            ],
            &quot;properties&quot;: {
                &quot;hardwareProfile&quot;: {
                    &quot;vmSize&quot;: &quot;[parameters('vmSize')]&quot;
                },
                &quot;osProfile&quot;: {
                    &quot;computername&quot;: &quot;[parameters('vmName')]&quot;,
                    &quot;adminUsername&quot;: &quot;[parameters('adminUsername')]&quot;,
                    &quot;adminPassword&quot;: &quot;[parameters('adminPassword')]&quot;
                },
                &quot;storageProfile&quot;: {
                    &quot;osDisk&quot;: {
                        &quot;name&quot;: &quot;[concat(parameters('vmName'),'-osDisk')]&quot;,
                        &quot;osType&quot;: &quot;[parameters('osType')]&quot;,
                        &quot;caching&quot;: &quot;ReadWrite&quot;,
                        &quot;image&quot;: {
                            &quot;uri&quot;: &quot;[parameters('vhdUrl')]&quot;
                        },
                        &quot;vhd&quot;: {
                            &quot;uri&quot;: &quot;[variables('osDiskVhdName')]&quot;
                        },
                        &quot;createOption&quot;: &quot;FromImage&quot;
                    }
                },
                &quot;networkProfile&quot;: {
                    &quot;networkInterfaces&quot;: [
                        {
                            &quot;id&quot;: &quot;[resourceId('Microsoft.Network/networkInterfaces',parameters('nicName'))]&quot;
                        }
                    ]
                }
            }
        },
        {
            &quot;type&quot;: &quot;Microsoft.Compute/virtualMachines/extensions&quot;,
            &quot;apiVersion&quot;: &quot;2015-06-15&quot;,
            &quot;name&quot;: &quot;[concat(parameters('vmName'),'/WinRMCustomScriptExtension')]&quot;,
            &quot;location&quot;: &quot;[parameters('location')]&quot;,
            &quot;dependsOn&quot;: [
                &quot;[concat('Microsoft.Compute/virtualMachines/', parameters('vmName'))]&quot;
            ],
            &quot;properties&quot;: {
                &quot;publisher&quot;: &quot;Microsoft.Compute&quot;,
                &quot;type&quot;: &quot;CustomScriptExtension&quot;,
                &quot;typeHandlerVersion&quot;: &quot;1.4&quot;,
                &quot;settings&quot;: {
                    &quot;fileUris&quot;: [
                        &quot;https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-winrm-windows/ConfigureWinRM.ps1&quot;,
                        &quot;https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-winrm-windows/makecert.exe&quot;,
                        &quot;https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-winrm-windows/winrmconf.cmd&quot;
                    ],
                    &quot;commandToExecute&quot;: &quot;[concat('powershell -ExecutionPolicy Unrestricted -file ConfigureWinRM.ps1 ',variables('hostDNSNameScriptArgument'))]":::

<br>Aqui está um exemplo de resultados de teste exibidos em um visualizador JSON online (como [código Beautify](https://codebeautify.org/jsonviewer) ou [Visualizador JSON](https://jsonformatter.org/json-viewer)).

![Resultados de teste em um visualizador JSON online.](media/vm/test-results-json-viewer-2.png)

## <a name="how-to-use-curl-to-consume-the-self-test-api-on-linux-os"></a>Como usar a rotação para consumir a API de Self-Test no sistema operacional Linux

Neste exemplo, a ondulação será usada para fazer uma chamada à API POST para Azure Active Directory e a VM Self-Host.

1. Solicitar um token do Azure AD para autenticar na VM de hospedagem interna

   Certifique-se de que os valores corretos sejam substituídos na solicitação de ondulação.

   ```
   curl --location --request POST 'https://login.microsoftonline.com/{TENANT_ID}/oauth2/token' \
   --header 'Content-Type: application/x-www-form-urlencoded' \
   --data-urlencode 'grant_type=client_credentials' \
   --data-urlencode 'client_id={CLIENT_ID} ' \
   --data-urlencode 'client_secret={CLIENT_SECRET}' \
   --data-urlencode 'resource=https://management.core.windows.net'
   ```
   Aqui está um exemplo da resposta da solicitação:
   ```JSON
   {
       "token_type": "Bearer",
       "expires_in": "86399",
       "ext_expires_in": "86399",
       "expires_on": "1599663998",
       "not_before": "1599577298",
       "resource": "https://management.core.windows.net",
       "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJS…"
   }
   ```

2. Enviar uma solicitação para a VM de teste automático

   Verifique se o token de portador e os parâmetros são substituídos pelos valores corretos.

   ```
   curl --location --request POST 'https://isvapp.azurewebsites.net/selftest-vm' \
   --header 'Content-Type: application/json' \
   --header 'Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJS…' \
   --data-raw '{
       "DNSName": "avvm1.eastus.cloudapp.azure.com",
       "UserName": "azureuser",
       "Password": "SECURE_PASSWORD_FOR_THE_SSH_INTO_VM",
       "OS": "Linux",
       "PortNo": "22",
       "CompanyName": "COMPANY_NAME",
       "AppId": "CLIENT_ID_SAME_AS_USED_FOR_AAD_TOKEN ",
       "TenantId": "TENANT_ID_SAME_AS_USED_FOR_AAD_TOKEN"
   }'
   ```

   Exemplo de resposta da chamada à API de VM de autoteste
   ```JSON
   {
       "TrackingId": "9bffc887-dd1d-40dd-a8a2-34cee4f4c4c3",
       "Response": "{\"SchemaVersion\":1,\"AppCertificationCategory\":\"Microsoft Single VM Certification\",\"ProviderID\":\"050DE427-2A99-46D4-817C-5354D3BF2AE8\",\"OSName\":\"Ubuntu 18.04\",\"OSDistro\":\"Ubuntu 18.04.5 LTS\",\"KernelVersion\":\"5.4.0-1023-azure\",\"KernelFullVersion\":\"Linux version 5.4.0-1023-azure (buildd@lgw01-amd64-053) (gcc version 7.5.0 (Ubuntu 7.5.0-3ubuntu1~18.04)) #23~18.04.1-Ubuntu SMP Thu Aug 20 14:46:48 UTC 2020\\n\",\"OSVersion\":\"18.04\",\"CreatedDate\":\"09/08/2020 01:13:47\",\"TestResult\":\"Pass\",\"APIVersion\":\"1.5\",\"Tests\":[{\"TestID\":\"48\",\"TestCaseName\":\"Bash History\",\"Description\":\"Bash history files should be cleared before creating the VM image.\",\"Result\":\"Passed\",\"ActualValue\":\"No file Exist\",\"RequiredValue\":\"1024\"},{\"TestID\":\"39\",\"TestCaseName\":\"Linux Agent Version\",\"Description\":\"Azure Linux Agent Version 2.2.41 and above should be installed.\",\"Result\":\"Passed\",\"ActualValue\":\"2.2.49\",\"RequiredValue\":\"2.2.41\"},{\"TestID\":\"40\",\"TestCaseName\":\"Required Kernel Parameters\",\"Description\":\"Verifies the following kernel parameters are set console=ttyS0, earlyprintk=ttyS0, rootdelay=300\",\"Result\":\"Warning\",\"ActualValue\":\"Missing Parameter: rootdelay=300\\r\\nMatched Parameter: console=ttyS0,earlyprintk=ttyS0\",\"RequiredValue\":\"console=ttyS0#earlyprintk=ttyS0#rootdelay=300\"},{\"TestID\":\"41\",\"TestCaseName\":\"Swap Partition on OS Disk\",\"Description\":\"Verifies that no Swap partitions are created on the OS disk.\",\"Result\":\"Passed\",\"ActualValue\":\"No. of Swap Partitions: 0\",\"RequiredValue\":\"swap\"},{\"TestID\":\"42\",\"TestCaseName\":\"Root Partition on OS Disk\",\"Description\":\"It is recommended that a single root partition is created for the OS disk.\",\"Result\":\"Passed\",\"ActualValue\":\"Root Partition: 1\",\"RequiredValue\":\"1\"},{\"TestID\":\"44\",\"TestCaseName\":\"OpenSSL Version\",\"Description\":\"OpenSSL Version should be >=0.9.8.\",\"Result\":\"Passed\",\"ActualValue\":\"1.1.1\",\"RequiredValue\":\"0.9.8\"},{\"TestID\":\"45\",\"TestCaseName\":\"Python Version\",\"Description\":\"Python version 2.6+ is highly recommended. \",\"Result\":\"Passed\",\"ActualValue\":\"2.7.17\",\"RequiredValue\":\"2.6\"},{\"TestID\":\"46\",\"TestCaseName\":\"Client Alive Interval\",\"Description\":\"It is recommended to set ClientAliveInterval to 180. On the application need, it can be set between 30 to 235. \\nIf you are enabling the SSH for your end users this value must be set as explained.\",\"Result\":\"Warning\",\"ActualValue\":\"120\",\"RequiredValue\":\"ClientAliveInterval 180\"},{\"TestID\":\"49\",\"TestCaseName\":\"OS Architecture\",\"Description\":\"Only 64-bit operating system should be supported.\",\"Result\":\"Passed\",\"ActualValue\":\"x86_64\\n\",\"RequiredValue\":\"x86_64,amd64\"},{\"TestID\":\"50\",\"TestCaseName\":\"Security threats\",\"Description\":\"Identifies OS with recent high profile vulnerability that may need patching.  Ignore warning if system was patched as appropriate.\",\"Result\":\"Passed\",\"ActualValue\":\"Ubuntu 18.04\",\"RequiredValue\":\"OS impacted by GHOSTS\"},{\"TestID\":\"51\",\"TestCaseName\":\"Auto Update\",\"Description\":\"Identifies if Linux Agent Auto Update is enabled or not.\",\"Result\":\"Passed\",\"ActualValue\":\"# AutoUpdate.Enabled=y\\n\",\"RequiredValue\":\"Yes\"},{\"TestID\":\"52\",\"TestCaseName\":\"SACK Vulnerability patch verification\",\"Description\":\"Checks if the running Kernel Version has SACK vulnerability patch.\",\"Result\":\"Passed\",\"ActualValue\":\"Ubuntu 18.04.5 LTS,Linux version 5.4.0-1023-azure (buildd@lgw01-amd64-053) (gcc version 7.5.0 (Ubuntu 7.5.0-3ubuntu1~18.04)) #23~18.04.1-Ubuntu SMP Thu Aug 20 14:46:48 UTC 2020\",\"RequiredValue\":\"Yes\"}]}"
   }
   ```

## <a name="next-steps"></a>Próximas etapas

- [Obter URI da SAS](azure-vm-get-sas-uri.md)

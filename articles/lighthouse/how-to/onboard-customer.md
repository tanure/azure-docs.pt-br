---
title: Integrar um cliente ao Azure Lighthouse
description: Saiba como integrar um cliente ao Azure Lighthouse, permitindo que seus recursos sejam acessados e gerenciados por meio de seu próprio locatário usando o gerenciamento de recursos delegado do Azure.
ms.date: 09/24/2020
ms.topic: how-to
ms.openlocfilehash: 43f28073c996167c82e241476020bdc341486b26
ms.sourcegitcommit: 10d00006fec1f4b69289ce18fdd0452c3458eca5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/21/2020
ms.locfileid: "95024289"
---
# <a name="onboard-a-customer-to-azure-lighthouse"></a>Integrar um cliente ao Azure Lighthouse

Este artigo explica como você, como um provedor de serviços, pode integrar um cliente ao Azure Lighthouse. Quando você faz isso, os recursos delegados do cliente (assinaturas e/ou grupos de recursos) podem ser acessados e gerenciados por meio de seu próprio locatário do Azure Active Directory (Azure AD) usando o [Gerenciamento de recursos delegado do Azure](../concepts/azure-delegated-resource-management.md).

> [!TIP]
> Embora possamos nos referimos a provedores de serviços e clientes neste tópico, as [empresas que gerenciam vários locatários](../concepts/enterprise.md) podem usar o mesmo processo para configurar o Azure Lighthouse e consolidar sua experiência de gerenciamento.

Você pode repetir o processo de integração para vários clientes. Quando um usuário com as permissões apropriadas entra no seu locatário de gerenciamento, esse usuário pode ser autorizado entre escopos de aluguel do cliente para executar operações de gerenciamento, sem precisar entrar em cada locatário individual do cliente.

Para acompanhar o impacto nas participações do cliente e receber reconhecimento, associe a ID do MPN (Microsoft Partner Network) a pelo menos uma conta de usuário que tenha acesso a cada uma das assinaturas integradas. Você precisará executar essa associação em seu locatário do provedor de serviços. É recomendável criar uma conta de entidade de serviço em seu locatário associado à ID do MPN e, em seguida, incluir essa entidade de serviço sempre que você carregar um cliente. Para obter mais informações, consulte [vincular sua ID de parceiro para habilitar o crédito ganho do parceiro em recursos delegados](partner-earned-credit.md).

> [!NOTE]
> Os clientes também podem ser integrados ao Azure Lighthouse quando compram uma oferta de serviço gerenciado (pública ou privada) que você [publica no Azure Marketplace](publish-managed-services-offers.md). Você também pode usar o processo de integração descrito aqui junto com as ofertas publicadas no Azure Marketplace.

O processo de integração requer que as ações sejam executadas dentro do locatário do provedor de serviços e do locatário do cliente. Todas essas etapas são descritas neste artigo.

## <a name="gather-tenant-and-subscription-details"></a>Reunir detalhes do locatário e da assinatura

Para integrar o locatário de um cliente, ele deve ter uma assinatura ativa do Azure. Você precisa saber do seguinte:

- A ID do locatário do provedor de serviços (em que você gerenciará os recursos do cliente)
- A ID do locatário do cliente (cujos recursos serão gerenciados pelo provedor de serviços)
- As IDs de cada assinatura específica no locatário do cliente que serão gerenciadas pelo provedor de serviços (ou que contém os grupos de recursos que o provedor de serviços vai gerenciar).

Se ainda não tiver esses valores de ID, você pode recuperá-los de uma das maneiras a seguir. Use esses valores exatos na implantação.

### <a name="azure-portal"></a>Portal do Azure

Você pode ver a sua ID de locatário passando o mouse sobre o nome da sua conta no lado superior direito do portal do Azure ou selecionando **Mudar diretório**. Para selecionar e copiar sua ID de locatário, pesquise "Azure Active Directory" no portal, selecione **Propriedades** e copie o valor mostrado no campo **ID de diretório**. Para localizar a ID de uma assinatura em um locatário de cliente, pesquise por "Assinaturas" e selecione a ID da assinatura apropriada.

### <a name="powershell"></a>PowerShell

```azurepowershell-interactive
# Log in first with Connect-AzAccount if you're not using Cloud Shell

Select-AzSubscription <subscriptionId>
```

### <a name="azure-cli"></a>CLI do Azure

```azurecli-interactive
# Log in first with az login if you're not using Cloud Shell

az account set --subscription <subscriptionId/name>
az account show
```

> [!NOTE]
> Ao integrar uma assinatura (ou um ou mais grupos de recursos dentro de uma assinatura) usando o processo descrito aqui, o provedor de recursos **Microsoft.ManagedServices** será registrado nessa assinatura.

## <a name="define-roles-and-permissions"></a>Definir funções e permissões do administrador

Como provedor de serviços, talvez você queira executar várias tarefas para um único cliente, o que exige acesso diferente para escopos diferentes. Você pode definir quantas autorizações forem necessárias para atribuir as [funções internas do Azure](../../role-based-access-control/built-in-roles.md) apropriadas aos usuários em seu locatário.

Para facilitar o gerenciamento, recomendamos o uso de grupos de usuários do Azure AD para cada função. Isso oferece a flexibilidade para adicionar ou remover usuários individuais do grupo que tem acesso, para que você não precise repetir o processo de integração para fazer alterações no usuário. Você pode atribuir funções a uma entidade de serviço, que pode ser útil para cenários de automação.

> [!IMPORTANT]
> Para adicionar permissões para um grupo do Azure AD, o **tipo de grupo** deve ser definido como **segurança**. Essa opção é selecionada quando o grupo é criado. Para obter mais informações, consulte [Criar um grupo básico e adicionar membros usando o Azure Active Directory](../../active-directory/fundamentals/active-directory-groups-create-azure-portal.md).

Ao definir suas autorizações, certifique-se de seguir o princípio de privilégios mínimos para que os usuários tenham apenas as permissões necessárias para concluir seu trabalho. Para obter diretrizes e informações sobre as funções com suporte, consulte [locatários, usuários e funções em cenários de Lighthouse do Azure](../concepts/tenants-users-roles.md).

Para definir autorizações, você precisará saber os valores de ID para cada usuário, grupo de usuários ou entidade de serviço no locatário do provedor de serviços ao qual você deseja conceder acesso. Você também precisa da ID de definição de função para cada função interna que deseja atribuir. Se você ainda não tiver essas informações, pode recuperá-las executando os comandos abaixo no locatário do provedor de serviços.

### <a name="powershell"></a>PowerShell

```azurepowershell-interactive
# Log in first with Connect-AzAccount if you're not using Cloud Shell

# To retrieve the objectId for an Azure AD group
(Get-AzADGroup -DisplayName '<yourGroupName>').id

# To retrieve the objectId for an Azure AD user
(Get-AzADUser -UserPrincipalName '<yourUPN>').id

# To retrieve the objectId for an SPN
(Get-AzADApplication -DisplayName '<appDisplayName>' | Get-AzADServicePrincipal).Id

# To retrieve role definition IDs
(Get-AzRoleDefinition -Name '<roleName>').id
```

### <a name="azure-cli"></a>CLI do Azure

```azurecli-interactive
# Log in first with az login if you're not using Cloud Shell

# To retrieve the objectId for an Azure AD group
az ad group list --query "[?displayName == '<yourGroupName>'].objectId" --output tsv

# To retrieve the objectId for an Azure AD user
az ad user show --id "<yourUPN>" --query "objectId" --output tsv

# To retrieve the objectId for an SPN
az ad sp list --query "[?displayName == '<spDisplayName>'].objectId" --output tsv

# To retrieve role definition IDs
az role definition list --name "<roleName>" | grep name
```

> [!TIP]
> É recomendável atribuir a [Função de Exclusão da Atribuição de Registro de Serviços Gerenciados](../../role-based-access-control/built-in-roles.md#managed-services-registration-assignment-delete-role) ao integrar um cliente, assim os usuários em seu locatário podem [remover o acesso à delegação](remove-delegation.md) mais tarde, se necessário. Se essa função não for atribuída, os recursos delegados só poderão ser removidos por um usuário no locatário do cliente.

## <a name="create-an-azure-resource-manager-template"></a>Criar um modelo do Azure Resource Manager

Para integrar seu cliente, você precisará criar um modelo do [Azure Resource Manager](../../azure-resource-manager/index.yml) para sua oferta com as seguintes informações. Os valores de **mspOfferName** e **mspOfferDescription** serão visíveis para o cliente na [página provedores de serviço](view-manage-service-providers.md) do portal do Azure.

|Campo  |Definição  |
|---------|---------|
|**mspOfferName**     |Um nome que descreve essa definição. Esse valor é exibido para o cliente como o título da oferta e deve ser um valor exclusivo.        |
|**mspOfferDescription**     |Uma breve descrição da sua oferta (por exemplo, "oferta de gerenciamento de VM da Contoso").      |
|**managedByTenantId**     |ID do locatário.          |
|**autorizações**     |Os valores de **principalId** dos usuários/grupos/SPNs do seu locatário, cada um com uma **principalIdDisplayName** para ajudar seu cliente a entender a finalidade da autorização, mapeados no valor interno de **roleDefinitionId** para especificar o nível de acesso.      |

O processo de integração requer um modelo do Azure Resource Manager (fornecido em nosso [repositório de exemplos](https://github.com/Azure/Azure-Lighthouse-samples/)) e um arquivo de parâmetros correspondentes que você modifica para corresponder à sua configuração e definir suas autorizações.

> [!IMPORTANT]
> O processo descrito aqui requer uma implantação separada para cada assinatura sendo integrada, mesmo se você estiver integrando assinaturas no mesmo locatário do cliente. Também serão necessárias implantações separadas se você estiver integrando vários grupos de recursos em assinaturas diferentes no mesmo locatário do cliente. No entanto, a integração de múltiplos grupos de recursos em uma única assinatura pode ser feita em uma implantação.
>
> Implantações separadas também são necessárias para várias ofertas que estão sendo aplicadas à mesma assinatura (ou grupos de recursos dentro de uma assinatura). Cada oferta aplicada deve usar um **mspOfferName** diferente.

O modelo escolhido dependerá se você está integrando uma assinatura inteira, um grupo de recursos ou múltiplos grupos de recursos em uma assinatura. Também fornecemos um modelo que pode ser usado por clientes que compraram uma oferta de serviço gerenciado que você publicou no Azure Marketplace, se você preferir integrar suas assinaturas dessa maneira.

|Para fazer essa integração  |use este modelo do Azure Resource Manager  |e altere esse arquivo de parâmetros |
|---------|---------|---------|
|Subscription   |[delegatedResourceManagement.json](https://github.com/Azure/Azure-Lighthouse-samples/blob/master/templates/delegated-resource-management/delegatedResourceManagement.json)  |[delegatedResourceManagement.parameters.json](https://github.com/Azure/Azure-Lighthouse-samples/blob/master/templates/delegated-resource-management/delegatedResourceManagement.parameters.json)    |
|Resource group   |[rgDelegatedResourceManagement.json](https://github.com/Azure/Azure-Lighthouse-samples/blob/master/templates/rg-delegated-resource-management/rgDelegatedResourceManagement.json)  |[rgDelegatedResourceManagement.parameters.json](https://github.com/Azure/Azure-Lighthouse-samples/blob/master/templates/rg-delegated-resource-management/rgDelegatedResourceManagement.parameters.json)    |
|Múltiplos grupos de recursos em uma assinatura   |[multipleRgDelegatedResourceManagement.json](https://github.com/Azure/Azure-Lighthouse-samples/blob/master/templates/rg-delegated-resource-management/multipleRgDelegatedResourceManagement.json)  |[multipleRgDelegatedResourceManagement.json](https://github.com/Azure/Azure-Lighthouse-samples/blob/master/templates/rg-delegated-resource-management/multipleRgDelegatedResourceManagement.parameters.json)    |
|Assinatura (ao usar uma oferta publicada no Azure Marketplace)   |[marketplaceDelegatedResourceManagement.json](https://github.com/Azure/Azure-Lighthouse-samples/blob/master/templates/marketplace-delegated-resource-management/marketplaceDelegatedResourceManagement.json)  |[marketplaceDelegatedResourceManagement.parameters.json](https://github.com/Azure/Azure-Lighthouse-samples/blob/master/templates/marketplace-delegated-resource-management/marketplaceDelegatedResourceManagement.parameters.json)    |

> [!TIP]
> Embora não seja possível carregar um grupo de gerenciamento inteiro em uma implantação, você pode [implantar uma política no nível do grupo de gerenciamento](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates/policy-delegate-management-groups). A política verificará se cada assinatura dentro do grupo de gerenciamento foi delegada ao locatário de gerenciamento especificado e, caso contrário, criará a atribuição com base nos valores que você fornecer.

O exemplo a seguir mostra um arquivo **delegatedResourceManagement.parameters.json** modificado que pode ser usado para integrar uma assinatura. Os arquivos de parâmetro do grupo de recursos, localizados na pasta [rg-delegated-resource-management](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates/rg-delegated-resource-management), são semelhantes, mas também incluem o parâmetro **rgName** para identificar os grupos de recursos específicos a integrar.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "mspOfferName": {
            "value": "Fabrikam Managed Services - Interstellar"
        },
        "mspOfferDescription": {
            "value": "Fabrikam Managed Services - Interstellar"
        },
        "managedByTenantId": {
            "value": "df4602a3-920c-435f-98c4-49ff031b9ef6"
        },
        "authorizations": {
            "value": [
                {
                    "principalId": "0019bcfb-6d35-48c1-a491-a701cf73b419",
                    "principalIdDisplayName": "Tier 1 Support",
                    "roleDefinitionId": "b24988ac-6180-42a0-ab88-20f7382dd24c"
                },
                {
                    "principalId": "0019bcfb-6d35-48c1-a491-a701cf73b419",
                    "principalIdDisplayName": "Tier 1 Support",
                    "roleDefinitionId": "36243c78-bf99-498c-9df9-86d9f8d28608"
                },
                {
                    "principalId": "0afd8497-7bff-4873-a7ff-b19a6b7b332c",
                    "principalIdDisplayName": "Tier 2 Support",
                    "roleDefinitionId": "acdd72a7-3385-48ef-bd42-f606fba81ae7"
                },
                {
                    "principalId": "9fe47fff-5655-4779-b726-2cf02b07c7c7",
                    "principalIdDisplayName": "Service Automation Account",
                    "roleDefinitionId": "b24988ac-6180-42a0-ab88-20f7382dd24c"
                },
                {
                    "principalId": "3kl47fff-5655-4779-b726-2cf02b05c7c4",
                    "principalIdDisplayName": "Policy Automation Account",
                    "roleDefinitionId": "18d7d88d-d35e-4fb5-a5c3-7773c20a72d9",
                    "delegatedRoleDefinitionIds": [
                        "b24988ac-6180-42a0-ab88-20f7382dd24c",
                        "92aaf0da-9dab-42b6-94a3-d43ce8d16293"
                    ]
                }
            ]
        }
    }
}
```

A última autorização no exemplo acima adiciona **principalId** com a função de Administrador de Acesso do Usuário (18d7d88d-d35e-4fb5-a5c3-7773c20a72d9). Ao atribuir essa função, você deve incluir a propriedade **delegatedRoleDefinitionIds** e uma ou mais funções internas. O usuário criado nessa autorização poderá atribuir essas funções internas a [identidades gerenciadas](../../active-directory/managed-identities-azure-resources/overview.md) no locatário do cliente, o que é necessário para [implantar políticas que podem ser corrigidas](deploy-policy-remediation.md).  O usuário também pode criar incidentes de suporte.  Nenhuma outra permissão normalmente associada à função Administrador de Acesso de Usuário será aplicada a esse usuário.

## <a name="deploy-the-azure-resource-manager-templates"></a>Implantar os modelos do Azure Resource Manager

Depois de atualizar o arquivo de parâmetros, um usuário no locatário do cliente deve implantar o modelo de Azure Resource Manager dentro de seu locatário. Uma implantação separada é necessária para cada assinatura que você deseja integrar (ou para cada assinatura que contém os grupos de recursos que você deseja carregar).

> [!IMPORTANT]
> Essa implantação deve ser feita por uma conta que não seja de convidado no locatário do cliente que tem a [função interna de proprietário](../../role-based-access-control/built-in-roles.md#owner) para a assinatura que está sendo integrada (ou que contém os grupos de recursos que estão sendo integrados). Para ver todos os usuários que podem delegar a assinatura, um usuário do locatário do cliente poderá selecionar a assinatura no portal do Azure, abrir o **IAM (Controle de acesso)** e [exibir todos os usuários com a função Proprietário](../../role-based-access-control/role-assignments-list-portal.md#list-owners-of-a-subscription). 
>
> Se a assinatura foi criada por meio do programa [CSP (provedor de soluções na nuvem)](../concepts/cloud-solution-provider.md), qualquer usuário que tenha a função de [Agente de administração](/partner-center/permissions-overview#manage-commercial-transactions-in-partner-center-azure-ad-and-csp-roles) no locatário do provedor de serviços pode executar a implantação.

A implantação pode ser feita no portal do Azure, usando o PowerShell ou usando CLI do Azure, conforme mostrado abaixo.

### <a name="azure-portal"></a>Portal do Azure

1. Em nosso [repositório GitHub](https://github.com/Azure/Azure-Lighthouse-samples/), selecione o botão **implantar no Azure** mostrado ao lado do modelo que você deseja usar. O modelo será aberto no portal do Azure.
1. Insira seus valores para **nome da oferta MSP**, **Descrição da oferta MSP**, **gerenciado por ID do locatário** e **autorizações**. Se preferir, você pode selecionar **Editar parâmetros** para inserir valores para `mspOfferName` , `mspOfferDescription` , `managedbyTenantId` e `authorizations` diretamente no arquivo de parâmetro. Certifique-se de atualizar esses valores em vez de usar os valores padrão do modelo.
1. Selecione **revisar e criar e**, em seguida, selecionar **criar**.

Após alguns minutos, você deverá ver uma notificação informando que a implantação foi concluída.

### <a name="powershell"></a>PowerShell

```azurepowershell-interactive
# Log in first with Connect-AzAccount if you're not using Cloud Shell

# Deploy Azure Resource Manager template using template and parameter file locally
New-AzSubscriptionDeployment -Name <deploymentName> `
                 -Location <AzureRegion> `
                 -TemplateFile <pathToTemplateFile> `
                 -TemplateParameterFile <pathToParameterFile> `
                 -Verbose

# Deploy Azure Resource Manager template that is located externally
New-AzSubscriptionDeployment -Name <deploymentName> `
                 -Location <AzureRegion> `
                 -TemplateUri <templateUri> `
                 -TemplateParameterUri <parameterUri> `
                 -Verbose
```

### <a name="azure-cli"></a>CLI do Azure

```azurecli-interactive
# Log in first with az login if you're not using Cloud Shell

# Deploy Azure Resource Manager template using template and parameter file locally
az deployment sub create --name <deploymentName> \
                         --location <AzureRegion> \
                         --template-file <pathToTemplateFile> \
                         --parameters <parameters/parameterFile> \
                         --verbose

# Deploy external Azure Resource Manager template, with local parameter file
az deployment sub create --name <deploymentName> \
                         --location <AzureRegion> \
                         --template-uri <templateUri> \
                         --parameters <parameterFile> \
                         --verbose
```

## <a name="confirm-successful-onboarding"></a>Confirmar integração bem-sucedida

Quando uma assinatura de cliente tiver sido integrada com êxito ao Lighthouse do Azure, os usuários no locatário do provedor de serviços poderão ver a assinatura e seus recursos (se tiverem recebido acesso a ele por meio do processo acima, seja individualmente ou como membro de um grupo do Azure AD com as permissões apropriadas). Para confirmar, verifique se a assinatura é exibida de uma das maneiras a seguir.  

### <a name="azure-portal"></a>Portal do Azure

No locatário do provedor de serviços:

1. Navegue até a [página Meus clientes](view-manage-customers.md).
2. Selecione **Clientes**.
3. Confirme que você consegue ver as assinaturas com o nome da oferta fornecido no modelo do Resource Manager.

> [!IMPORTANT]
> Para ver a assinatura delegada em [meus clientes](view-manage-customers.md), os usuários no locatário do provedor de serviços devem ter recebido a função [leitor](../../role-based-access-control/built-in-roles.md#reader) (ou outra função interna que inclua acesso de leitor) quando a assinatura foi integrada.

No locatário do cliente:

1. Navegue até a [página Provedores de serviços](view-manage-service-providers.md).
2. Selecione **Ofertas do provedor de serviços**.
3. Confirme que você consegue ver as assinaturas com o nome da oferta fornecido no modelo do Resource Manager.

> [!NOTE]
> Pode demorar alguns minutos depois da conclusão da implantação até que o portal do Azure reflita as atualizações.

### <a name="powershell"></a>PowerShell

```azurepowershell-interactive
# Log in first with Connect-AzAccount if you're not using Cloud Shell

Get-AzContext

# Confirm successful onboarding for Azure Lighthouse

Get-AzManagedServicesDefinition
Get-AzManagedServicesAssignment
```

### <a name="azure-cli"></a>CLI do Azure

```azurecli-interactive
# Log in first with az login if you're not using Cloud Shell

az account list
```

## <a name="next-steps"></a>Próximas etapas

- Saiba mais sobre as [experiências de gerenciamento entre locatários](../concepts/cross-tenant-management-experience.md).
- [Exiba e gerencie clientes](view-manage-customers.md) acessando **Meus clientes** no portal do Azure.
- Aprenda a [remover o acesso a uma delegação](remove-delegation.md) que foi integrada anteriormente.

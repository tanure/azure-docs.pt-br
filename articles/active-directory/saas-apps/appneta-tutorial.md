---
title: 'Tutorial: Integração do SSO (logon único) do Azure Active Directory ao AppNeta Performance Monitor | Microsoft Docs'
description: Saiba como configurar o logon único entre o Azure Active Directory e o AppNeta Performance Monitor.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 11/06/2019
ms.author: jeedes
ms.openlocfilehash: 43447d98cf4af6887e66dd0811464e34fc1ac717
ms.sourcegitcommit: 9b8425300745ffe8d9b7fbe3c04199550d30e003
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/23/2020
ms.locfileid: "92457882"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-appneta-performance-monitor"></a>Tutorial: Integração do SSO (logon único) do Azure Active Directory ao AppNeta Performance Monitor

Neste tutorial, você aprenderá a integrar o AppNeta Performance Monitor ao Azure AD (Azure Active Directory). Ao integrar o AppNeta Performance Monitor ao Azure AD, você pode:

* Controlar no Azure AD quem terá acesso ao AppNeta Performance Monitor.
* Permitir que os usuários façam logon automaticamente no AppNeta Performance Monitor usando suas contas do Azure AD.
* Gerenciar suas contas em um local central: o portal do Azure.

Para saber mais sobre a integração de aplicativos SaaS ao Azure AD, confira [O que é o acesso de aplicativos e o logon único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para começar, você precisará dos seguintes itens:

* Uma assinatura do Azure AD. Caso você não tenha uma assinatura, obtenha uma [conta gratuita](https://azure.microsoft.com/free/).
* Uma assinatura do AppNeta Performance Monitor habilitada para SSO (logon único).

## <a name="scenario-description"></a>Descrição do cenário

Neste tutorial, você configurará e testará o SSO do Azure AD em um ambiente de teste.

* O AppNeta Performance Monitor é compatível com o SSO iniciado por **SP**


* O AppNeta Performance Monitor é compatível com o provisionamento de usuário **Just-In-Time**

> [!NOTE]
> O identificador desse aplicativo é um valor de cadeia de caracteres fixo; portanto apenas uma instância pode ser configurada em um locatário.


## <a name="adding-appneta-performance-monitor-from-the-gallery"></a>Adicionar o AppNeta Performance Monitor a partir da galeria

Para configurar a integração do AppNeta Performance Monitor ao Azure AD, você precisará adicionar o AppNeta Performance Monitor da galeria à sua lista de aplicativos SaaS gerenciados.

1. Entre no [portal do Azure](https://portal.azure.com) usando uma conta corporativa ou de estudante ou uma conta pessoal da Microsoft.
1. No painel de navegação esquerdo, escolha o serviço **Azure Active Directory** .
1. Navegue até **Aplicativos Empresariais** e, em seguida, escolha **Todos os Aplicativos** .
1. Para adicionar um novo aplicativo, escolha **Novo aplicativo** .
1. Na seção **Adicionar da galeria** , digite **AppNeta Performance Monitor** na caixa de pesquisa.
1. Selecione **AppNeta Performance Monitor** no painel de resultados e, em seguida, adicione o aplicativo. Aguarde alguns segundos enquanto o aplicativo é adicionado ao seu locatário.


## <a name="configure-and-test-azure-ad-single-sign-on-for-appneta-performance-monitor"></a>Configurar e testar o logon único do Azure AD para o AppNeta Performance Monitor

Configure e teste o SSO do Azure AD com o AppNeta Performance Monitor usando um usuário de teste chamado **B.Fernandes** . Para que o SSO funcione, é necessário estabelecer uma relação de vínculo entre um usuário do Azure AD e o usuário relacionado do AppNeta Performance Monitor.

Para configurar e testar o SSO do Azure AD com o AppNeta Performance Monitor, conclua os seguintes blocos de construção:

1. **[Configurar o SSO do Azure AD](#configure-azure-ad-sso)** – para permitir que os usuários usem esse recurso.
    1. **[Criar um usuário de teste do Azure AD](#create-an-azure-ad-test-user)** para testar o logon único do Azure AD com B.Fernandes.
    1. **[Atribuir o usuário de teste do Azure AD](#assign-the-azure-ad-test-user)** – para permitir que B.Fernandes use o logon único do Azure AD.
1. **[Configurar o SSO do AppNeta Performance Monitor](#configure-appneta-performance-monitor-sso)** – para definir as configurações de logon único no lado do aplicativo.
    1. **[Criar um usuário de teste do AppNeta Performance Monitor](#create-appneta-performance-monitor-test-user)** – para ter um equivalente de B.Fernandes no AppNeta Performance Monitor que está vinculado à declaração do usuário no Azure AD.
1. **[Testar o SSO](#test-sso)** – para verificar se a configuração funciona.

## <a name="configure-azure-ad-sso"></a>Configurar o SSO do Azure AD

Siga estas etapas para habilitar o SSO do Azure AD no portal do Azure.

1. No [portal do Azure](https://portal.azure.com/), na página de integração de aplicativos do **AppNeta Performance Monitor** , localize a seção **Gerenciar** e selecione **Logon único** .
1. Na página **Selecionar um método de logon único** , escolha **SAML** .
1. Na página **Configurar o logon único com o SAML** , clique no ícone de edição/caneta da **Configuração Básica do SAML** para editar as configurações.

   ![Editar a Configuração Básica de SAML](common/edit-urls.png)

1. Na seção **Configuração Básica do SAML** , insira os valores para os seguintes campos:

    a. Na caixa de texto **URL de Logon** , digite uma URL usando o seguinte padrão: `https://<subdomain>.pm.appneta.com`

    b. Na caixa de texto **Identificador (ID da Entidade)** , digite um valor: `PingConnect`

    > [!NOTE]
    > O valor da URL de logon não é real. Atualize esse valor com a URL de Logon real. Entre em contato com a [equipe de suporte do AppNeta Performance Monitor](mailto:support@appneta.com) para obter esse valor. Você também pode consultar os padrões exibidos na seção **Configuração Básica de SAML** no portal do Azure.

1. O aplicativo AppNeta Performance Monitor espera as declarações do SAML em um formato específico, o que exige a adição de mapeamentos de atributo personalizados à configuração de atributos do token SAML. A captura de tela a seguir mostra a lista de atributos padrão.

    ![image](common/edit-attribute.png)

1. Além do indicado acima, o aplicativo AppNeta Performance Monitor espera que mais alguns atributos sejam transmitidos novamente na resposta SAML, os quais são mostrados abaixo. Esses atributos também são pré-populados, mas você pode examiná-los de acordo com seu requisito.

    | Nome | Atributo de Origem|
    | --------| ----------------|
    | firstName| user.givenname|
    | lastName| user.surname|
    | email| user.userprincipalname|
    | name| user.userprincipalname|
    | groups  | user.assignedroles |
    | phone| user.telephonenumber |
    | título| user.jobtitle|
    | | |

    > [!NOTE]
    > **grupos** se refere aos grupos de segurança no Appneta que são mapeados para uma **Função** no Azure AD. Consulte [ este documento ](../develop/active-directory-enterprise-app-role-management.md) que explica como criar funções personalizadas no Microsoft Azure AD.

    1. Clique em **Adicionar nova reivindicação** para abrir a caixa de diálogo **Gerenciar declarações de usuários** .

    1. Na caixa de texto **Nome** , digite o nome do atributo mostrado para essa linha.

    1. Deixe o **Namespace** em branco.

    1. Escolha Origem como **Atributo** .

    1. Na lista **Atributo de origem** , digite o valor do atributo mostrado para essa linha.

    1. Clique em **Ok**

    1. Clique em **Save** (Salvar).

1. Na página **Configurar o logon único com o SAML** , na seção **Certificado de Autenticação SAML** , localize **XML de Metadados de Federação** e selecione **Baixar** para baixar o certificado e salvá-lo no computador.

    ![O link de download do Certificado](common/metadataxml.png)

1. Na seção **Configurar o AppNeta Performance Monitor** , copie a URL apropriada conforme suas necessidades.

    ![Copiar URLs de configuração](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Criar um usuário de teste do Azure AD

Nesta seção, você criará um usuário de teste no portal do Azure chamado B.Fernandes.

1. No painel esquerdo do portal do Azure, escolha **Azure Active Directory** , **Usuários** e, em seguida, **Todos os usuários** .
1. Selecione **Novo usuário** na parte superior da tela.
1. Nas propriedades do **Usuário** , siga estas etapas:
   1. No campo **Nome** , insira `B.Simon`.  
   1. No campo **Nome de usuário** , insira username@companydomain.extension. Por exemplo, `B.Simon@contoso.com`.
   1. Marque a caixa de seleção **Mostrar senha** e, em seguida, anote o valor exibido na caixa **Senha** .
   1. Clique em **Criar** .

### <a name="assign-the-azure-ad-test-user"></a>Atribuir o usuário de teste do Azure AD

Nesta seção, você permitirá que B.Fernandes use o logon único do Azure concedendo acesso ao AppNeta Performance Monitor.

1. No portal do Azure, selecione **Aplicativos empresariais** e, em seguida, selecione **Todos os aplicativos** .
1. Na lista de aplicativos, selecione **o Monitor de desempenho AppNeta Performance Monitor** .
1. Na página de visão geral do aplicativo, localize a seção **Gerenciar** e escolha **Usuários e grupos** .

   ![O link “Usuários e grupos”](common/users-groups-blade.png)

1. Escolha **Adicionar usuário** e, em seguida, **Usuários e grupos** na caixa de diálogo **Adicionar Atribuição** .

    ![O link Adicionar Usuário](common/add-assign-user.png)

1. Na caixa de diálogo **Usuários e grupos** , selecione **B.Fernandes** na lista Usuários e clique no botão **Selecionar** na parte inferior da tela.
1. Se você estiver esperando um valor de função na declaração SAML, na caixa de diálogo **Selecionar Função** , escolha a função apropriada para o usuário da lista e, em seguida, clique no botão **Escolher** na parte inferior da tela.
1. Na caixa de diálogo **Adicionar atribuição** , clique no botão **Atribuir** .

## <a name="configure-appneta-performance-monitor-sso"></a>Configurar o SSO do AppNeta Performance Monitor

Para configurar o logon único no lado do **AppNeta Performance Monitor** , é necessário enviar o **XML de Metadados de Federação** baixado e as URLs apropriadas copiadas do portal do Azure para a [equipe de suporte do AppNeta Performance Monitor](mailto:support@appneta.com). Eles definem essa configuração para ter a conexão de SSO de SAML definida corretamente em ambos os lados.

### <a name="create-appneta-performance-monitor-test-user"></a>Criar um usuário de teste do AppNeta Performance Monitor

Nesta seção, um usuário chamado Brenda Fernandes será criado no AppNeta Performance Monitor. O AppNeta Performance Monitor é compatível com o provisionamento do usuário just-in-time, que está habilitado por padrão. Não há itens de ação para você nesta seção. Se um usuário ainda não existir no AppNeta Performance Monitor, um novo usuário será criado após a autenticação.

> [!Note]
> Se você precisar criar um usuário manualmente, contate a [equipe de suporte ao cliente do AppNeta Performance Monitor](mailto:support@appneta.com).

## <a name="test-sso"></a>Testar o SSO 

Nesta seção, você testará sua configuração de logon único do Azure AD usando o Painel de Acesso.

Ao clicar no bloco AppNeta Performance Monitor no Painel de Acesso, você deverá ser conectado automaticamente ao AppNeta Performance Monitor, para o qual configurou o SSO. Para saber mais sobre o Painel de Acesso, veja [Introdução ao Painel de Acesso](../user-help/my-apps-portal-end-user-access.md).

## <a name="additional-resources"></a>Recursos adicionais

- [ Lista de tutoriais sobre como integrar aplicativos SaaS com o Active Directory do Azure ](./tutorial-list.md)

- [O que é o acesso a aplicativos e logon único com o Azure Active Directory? ](../manage-apps/what-is-single-sign-on.md)

- [O que é o acesso condicional no Azure Active Directory?](../conditional-access/overview.md)

- [Experimentar o AppNeta Performance Monitor com o Azure AD](https://aad.portal.azure.com/)
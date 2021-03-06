---
title: 'Tutorial: Integração do Azure Active Directory ao Five9 Plus Adapter (CTI, Contact Center Agents) | Microsoft Docs'
description: Saiba como configurar o logon único entre o Azure Active Directory e o Five9 Plus Adapter (CTI, Contact Center Agents).
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 04/04/2019
ms.author: jeedes
ms.openlocfilehash: 68de5b11c131fe33252178ebecdeb9c3855fe239
ms.sourcegitcommit: 9b8425300745ffe8d9b7fbe3c04199550d30e003
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/23/2020
ms.locfileid: "92453424"
---
# <a name="tutorial-azure-active-directory-integration-with-five9-plus-adapter-cti-contact-center-agents"></a>Tutorial: Integração do Azure Active Directory ao Five9 Plus Adapter (CTI, Contact Center Agents)

Neste tutorial, você aprende a integrar o Five9 Plus Adapter (CTI, Contact Center Agents) ao Azure AD (Azure Active Directory).
A integração do Five9 Plus Adapter (CTI, Contact Center Agents) ao Azure AD oferece os seguintes benefícios:

* Você pode controlar no Azure AD quem tem acesso ao Five9 Plus Adapter (CTI, Contact Center Agents).
* Você pode permitir que os usuários sejam conectados automaticamente ao Five9 Plus Adapter (CTI, Contact Center Agents) (logon único) com suas contas do Azure AD.
* Você pode gerenciar suas contas em um único local central – o portal do Azure.

Para conhecer mais detalhadamente a integração de aplicativos de SaaS ao AD do Azure, consulte [O que é o acesso a aplicativos e logon único com o Active Directory do Azure](../manage-apps/what-is-single-sign-on.md).
Se você não tiver uma assinatura do Azure, [crie uma conta gratuita](https://azure.microsoft.com/free/) antes de começar.

## <a name="prerequisites"></a>Prerequisites

Para configurar a integração do Azure AD ao Five9 Plus Adapter (CTI, Contact Center Agents), você precisa dos seguintes itens:

* Uma assinatura do Azure AD. Se não tiver um ambiente do Azure AD, poderá obter uma [conta gratuita](https://azure.microsoft.com/free/).
* Assinatura habilitada para logon único do Five9 Plus Adapter (CTI, Contact Center Agents)

## <a name="scenario-description"></a>Descrição do cenário

Neste tutorial, você configurará e testará o logon único do Azure AD em um ambiente de teste.

* O Five9 Plus Adapter (CTI, Contact Center Agents) dá suporte ao SSO iniciado por **IdP**

## <a name="adding-five9-plus-adapter-cti-contact-center-agents-from-the-gallery"></a>Adicionando o Five9 Plus Adapter (CTI, Contact Center Agents) por meio da galeria

Para configurar a integração do Five9 Plus Adapter (CTI, Contact Center Agents) ao Azure AD, é necessário adicionar o Five9 Plus Adapter (CTI, Contact Center Agents) à lista de aplicativos SaaS gerenciados por meio da galeria.

**Para adicionar o Five9 Plus Adapter (CTI, Contact Center Agents) por meio da galeria, realize as seguintes etapas:**

1. No **[Portal do Azure](https://portal.azure.com)** , no painel navegação à esquerda, clique no ícone **Azure Active Directory** .

    ![O botão Azure Active Directory](common/select-azuread.png)

2. Navegue até **Aplicativos Empresariais** e, em seguida, selecione a opção **Todos os Aplicativos** .

    ![A folha Aplicativos empresariais](common/enterprise-applications.png)

3. Clique no botão **Novo aplicativo** na parte superior da caixa de diálogo para adicionar o novo aplicativo.

    ![O botão Novo aplicativo](common/add-new-app.png)

4. Na caixa de pesquisa, digite **Five9 Plus Adapter (CTI, Contact Center Agents)** , selecione **Five9 Plus Adapter (CTI, Contact Center Agents)** no painel de resultados e, em seguida, clique no botão **Adicionar** para adicionar o aplicativo.

     ![Five9 Plus Adapter (CTI, Contact Center Agents) na lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar logon único do Azure AD

Nesta seção, você configurará e testará o logon único do Azure AD com o Five9 Plus Adapter (CTI, Contact Center Agents), com base em um usuário de teste chamado **Brenda Fernandes** .
Para que o logon único funcione, é necessário estabelecer uma relação de vínculo entre um usuário do Azure AD e o usuário relacionado do Five9 Plus Adapter (CTI, Contact Center Agents).

Para configurar e testar o logon único do Azure AD com o Five9 Plus Adapter (CTI, Contact Center Agents), você precisa concluir os seguintes blocos de construção:

1. **[Configurar o logon único do Azure AD](#configure-azure-ad-single-sign-on)** – para habilitar seus usuários a usar esse recurso.
2. **[Configurar o logon único do Five9 Plus Adapter (CTI, Contact Center Agents)](#configure-five9-plus-adapter-cti-contact-center-agents-single-sign-on)** – para definir as configurações de logon único no lado do aplicativo.
3. **[Criar um usuário de teste do Azure AD](#create-an-azure-ad-test-user)** – para testar o logon único do Azure AD com Brenda Fernandes.
4. **[Atribuir o usuário de teste do Azure AD](#assign-the-azure-ad-test-user)** – para permitir que Brenda Fernandes use o logon único do Azure AD.
5. **[Criar um usuário de teste do Five9 Plus Adapter (CTI, Contact Center Agents)](#create-five9-plus-adapter-cti-contact-center-agents-test-user)** – para ter um equivalente de Brenda Fernandes no Five9 Plus Adapter (CTI, Contact Center Agents) que esteja vinculado à representação de usuário do Azure AD.
6. **[Teste o logon único](#test-single-sign-on)** – para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o logon único do Azure AD

Nesta seção, você habilitará o logon único do Azure AD no portal do Azure.

Para configurar o logon único do Azure AD com o Five9 Plus Adapter (CTI, Contact Center Agents), execute as seguintes etapas:

1. No [portal do Azure](https://portal.azure.com/), na página de integração de aplicativos do **Five9 Plus Adapter (CTI, Contact Center Agents)** , selecione **Logon único** .

    ![Link Configurar logon único](common/select-sso.png)

2. Na caixa de diálogo **Selecionar um método de logon único** , selecione o modo **SAML/WS-Fed** para habilitar o logon único.

    ![Modo de seleção de logon único](common/select-saml-option.png)

3. Na página **Definir logon único com SAML** , clique no ícone **Editar** para abrir a caixa de diálogo **Configuração básica do SAML** .

    ![Editar a Configuração Básica de SAML](common/edit-urls.png)

4. Na página **Configurar Logon Único com SAML** , execute as seguintes etapas:

    ![Informações de logon único de Domínio e URLs do Five9 Plus Adapter (CTI, Contact Center Agents)](common/idp-intiated.png)

    a. Na caixa de texto **Identificador** , digite uma URL usando o seguinte padrão:
    
    |    Ambiente      |       URL      |
    | :-- | :-- |
    | Para o “Five9 Plus Adapter for Microsoft Dynamics CRM” | `https://app.five9.com/appsvcs/saml/metadata/alias/msdc` |
    | Para o “Five9 Plus Adapter for Zendesk” | `https://app.five9.com/appsvcs/saml/metadata/alias/zd` |
    | Para o “Five9 Plus Adapter for Agent Desktop Toolkit” | `https://app.five9.com/appsvcs/saml/metadata/alias/adt` |

    b. Na caixa de texto **URL de Resposta** , digite uma URL usando o seguinte padrão:

    |      Ambiente     |      URL      |
    | :--                  | :--           |
    | Para o “Five9 Plus Adapter for Microsoft Dynamics CRM” | `https://app.five9.com/appsvcs/saml/SSO/alias/msdc` |
    | Para o “Five9 Plus Adapter for Zendesk” | `https://app.five9.com/appsvcs/saml/SSO/alias/zd` |
    | Para o “Five9 Plus Adapter for Agent Desktop Toolkit” | `https://app.five9.com/appsvcs/saml/SSO/alias/adt` |

6. Na página **Configurar logon único com SAML** , na seção **Certificado de Autenticação SAML** , clique em **Fazer o download** para fazer o download do **Certificado (Base64)** usando as opções fornecidas de acordo com seus requisitos e salve-o no computador.

    ![O link de download do Certificado](common/certificatebase64.png)

7. Na seção **Configurar o Five9 Plus Adapter (CTI, Contact Center Agents)** , copie as URLs apropriadas de acordo com suas necessidades.

    ![Copiar URLs de configuração](common/copy-configuration-urls.png)

    a. URL de logon

    b. Identificador do Azure AD

    c. URL de logoff

### <a name="configure-five9-plus-adapter-cti-contact-center-agents-single-sign-on"></a>Configurar o logon único do Five9 Plus Adapter (CTI, Contact Center Agents)

1. Para configurar o logon único no lado do **Five9 Plus Adapter (CTI, Contact Center Agents)** , é necessário enviar o **Certificado (Base64)** baixado e as URLs apropriadas copiadas para a [equipe de suporte do Five9 Plus Adapter (CTI, Contact Center Agents)](https://www.five9.com/about/contact). Além disso, para configurar o SSO adicional, siga as etapas abaixo de acordo com o adaptador:

    a. Guia do Administrador “Five9 Plus Adapter for Agent Desktop Toolkit”: [https://webapps.five9.com/assets/files/for_customers/documentation/integrations/agent-desktop-toolkit/plus-agent-desktop-toolkit-administrators-guide.pdf](https://webapps.five9.com/assets/files/for_customers/documentation/integrations/agent-desktop-toolkit/plus-agent-desktop-toolkit-administrators-guide.pdf)
    
    b. Guia do administrador “Five9 Plus Adapter for Microsoft Dynamics CRM”: [https://webapps.five9.com/assets/files/for_customers/documentation/integrations/microsoft/microsoft-administrators-guide.pdf](https://webapps.five9.com/assets/files/for_customers/documentation/integrations/microsoft/microsoft-administrators-guide.pdf)
    
    c. Guia do administrador“Five9 Plus Adapter for Zendesk”: [https://webapps.five9.com/assets/files/for_customers/documentation/integrations/zendesk/zendesk-plus-administrators-guide.pdf](https://webapps.five9.com/assets/files/for_customers/documentation/integrations/zendesk/zendesk-plus-administrators-guide.pdf)

### <a name="create-an-azure-ad-test-user"></a>Criar um usuário de teste do Azure AD 

O objetivo desta seção é criar um usuário de teste no Portal do Azure chamado Brenda Fernandes.

1. No Portal do Azure, no painel esquerdo, selecione **Azure Active Directory** , selecione **Usuários** e, em seguida, **Todos os usuários** .

    ![Os links “Usuários e grupos” e “Todos os usuários”](common/users.png)

2. Selecione **Novo usuário** na parte superior da tela.

    ![Botão Novo usuário](common/new-user.png)

3. Nas Propriedades do usuário, execute as etapas a seguir.

    ![A caixa de diálogo Usuário](common/user-properties.png)

    a. No campo **Nome** , insira **BrendaFernandes** .
  
    b. No campo **Nome de usuário** , digite `brittasimon@yourcompanydomain.extension`. Por exemplo, BrittaSimon@contoso.com

    c. Marque a caixa de seleção **Mostrar senha** e, em seguida, anote o valor exibido na caixa Senha.

    d. Clique em **Criar** .

### <a name="assign-the-azure-ad-test-user"></a>Atribuir o usuário de teste do Azure AD

Nesta seção, você permite que Brenda Fernandes use o logon único do Azure concedendo acesso ao Five9 Plus Adapter (CTI, Contact Center Agents).

1. No portal do Azure, selecione **Aplicativos Empresariais** , selecione **Todos os aplicativos** e, em seguida, **Five9 Plus Adapter (CTI, Contact Center Agents)** .

    ![Folha de aplicativos empresariais](common/enterprise-applications.png)

2. Na lista de aplicativos, selecione **Five9 Plus Adapter (CTI, Contact Center Agents)** .

    ![O link do Five9 Plus Adapter (CTI, Contact Center Agents) na lista Aplicativos](common/all-applications.png)

3. No menu à esquerda, selecione **Usuários e grupos** .

    ![O link “Usuários e grupos”](common/users-groups-blade.png)

4. Escolha o botão **Adicionar usuário** e, em seguida, escolha **Usuários e grupos** na caixa de diálogo **Adicionar Atribuição** .

    ![O painel Adicionar Atribuição](common/add-assign-user.png)

5. Na caixa de diálogo **Usuários e grupos** , escolha **Brenda Fernandes** na lista Usuários e clique no botão **Selecionar** na parte inferior da tela.

6. Se você estiver esperando um valor de função na declaração SAML, na caixa de diálogo **Selecionar função** , escolha a função de usuário apropriada na lista e clique no botão **Selecionar** na parte inferior da tela.

7. Na caixa de diálogo **Adicionar atribuição** , clique no botão **Atribuir** .

### <a name="create-five9-plus-adapter-cti-contact-center-agents-test-user"></a>Criar um usuário de teste do Five9 Plus Adapter (CTI, Contact Center Agents)

Nesta seção, você cria um usuário chamado Brenda Fernandes no Five9 Plus Adapter (CTI, Contact Center Agents). Trabalhe com a [equipe de suporte do Five9 Plus Adapter (CTI, Contact Center Agents)](https://www.five9.com/about/contact) para adicionar os usuários à plataforma Five9 Plus Adapter (CTI, Contact Center Agents). Os usuários devem ser criados e ativados antes de usar o logon único. 

### <a name="test-single-sign-on"></a>Testar logon único 

Nesta seção, você testará sua configuração de logon único do Azure AD usando o Painel de Acesso.

Ao clicar no bloco do Five9 Plus Adapter (CTI, Contact Center Agents) no Painel de Acesso, você deverá ser conectado automaticamente ao Five9 Plus Adapter (CTI, Contact Center Agents), para o qual você configurou o SSO. Para saber mais sobre o Painel de Acesso, veja [Introdução ao Painel de Acesso](../user-help/my-apps-portal-end-user-access.md).

## <a name="additional-resources"></a>Recursos adicionais

- [Lista de tutoriais sobre como integrar aplicativos SaaS com o Active Directory do Azure](./tutorial-list.md)

- [O que é o acesso a aplicativos e logon único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

- [O que é o acesso condicional no Azure Active Directory?](../conditional-access/overview.md)
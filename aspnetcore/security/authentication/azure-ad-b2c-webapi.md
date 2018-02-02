---
title: "Autenticação de nuvem em APIs com o Azure Active Directory B2C da web"
author: camsoper
description: "Saiba como configurar a autenticação do Azure Active Directory B2C com a API da Web do ASP.NET Core. Teste web API com carteiro autenticada."
ms.author: casoper
manager: wpickett
ms.date: 01/25/2018
ms.topic: tutorial
ms.technology: aspnet
ms.prod: asp.net-core
ms.custom: mvc
uid: security/authentication/azure-ad-b2c-webapi
ms.openlocfilehash: a63bfc26bb6b0f5ea1c64641d6f57a3555d7f401
ms.sourcegitcommit: f2a11a89037471a77ad68a67533754b7bb8303e2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/01/2018
---
# <a name="cloud-authentication-in-web-apis-with-azure-active-directory-b2c"></a>Autenticação de nuvem em APIs com o Azure Active Directory B2C da web

Por [Cam Soper](https://twitter.com/camsoper)

[B2C de diretório ativo do Azure](/azure/active-directory-b2c/active-directory-b2c-overview) (Azure AD B2C) é uma solução de gerenciamento de identidade de nuvem para aplicativos web e móveis. O serviço fornece autenticação para aplicativos hospedados na nuvem e local. Tipos de autenticação incluem incluem contas individuais, contas de rede social e federados contas corporativas. Além disso, o Azure AD B2C pode fornecer autenticação multifator com configuração mínima.

> [!TIP]
> Azure Active Directory (AD do Azure) do Azure AD B2C são ofertas de produtos separados. Um locatário do AD do Azure representa uma organização, enquanto um locatário Azure AD B2C representa uma coleção de identidades a serem usadas com aplicativos de terceira parte confiável. Para obter mais informações, consulte [do Azure AD B2C: perguntas frequentes (FAQ)](/azure/active-directory-b2c/active-directory-b2c-faqs).

Como as APIs da web não tem nenhuma interface de usuário, eles são não é possível redirecionar o usuário para um serviço de token seguro como o Azure AD B2C. Em vez disso, a API é passada um token de portador do aplicativo de chamada, que já foi autenticada do usuário com o Azure AD B2C. A API, em seguida, valida o token sem interação direta do usuário.

Neste tutorial, saiba como:

> [!div class="checklist"]
> * Crie um locatário do Azure Active Directory B2C.
> * Registre uma API da Web no B2C do Azure AD.
> * Use o Visual Studio para criar uma API da Web configurado para usar o locatário do Azure AD B2C para autenticação.
> * Configure políticas que controlam o comportamento do locatário do Azure AD B2C.
> * Use carteiro para simular um aplicativo web que apresenta uma caixa de diálogo de logon, recupera um token e usa para fazer uma solicitação em relação a API da web.

## <a name="prerequisites"></a>Pré-requisitos

A seguir é necessários para este passo a passo:

* [Assinatura do Microsoft Azure](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
* [Visual Studio de 2017](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs) (qualquer edição)
* [Postman](https://www.getpostman.com/postman)

## <a name="create-the-azure-active-directory-b2c-tenant"></a>Criar o locatário do Azure Active Directory B2C

Crie um locatário Azure AD B2C [conforme descrito na documentação do](/azure/active-directory-b2c/active-directory-b2c-get-started). Quando solicitado, associar o locatário com uma assinatura do Azure é opcional para este tutorial.

## <a name="configure-a-sign-up-or-sign-in-policy"></a>Configurar uma política de inscrever-se ou entrar

Use as etapas da documentação do Azure AD B2C [criar uma política de inscrever-se ou entrar](/azure/active-directory-b2c/active-directory-b2c-reference-policies#create-a-sign-up-or-sign-in-policy). Nomear a política **SiUpIn**.  Use os valores de exemplo fornecidos na documentação do **provedores de identidade**, **inscrição atributos**, e **declarações de aplicativo**. Usando o **executar agora** botão para testar a política conforme descrito na documentação é opcional.

## <a name="register-the-api-in-azure-ad-b2c"></a>Registrar a API no B2C do Azure AD

Registrar no locatário do Azure AD B2C recém-criado, usando a API [as etapas na documentação do](/azure/active-directory-b2c/active-directory-b2c-app-registration#register-a-web-api) sob o **registrar uma API da web** seção.

Use os seguintes valores:

| Configuração                       | Valor               | Observações                                                                                  |
|-------------------------------|---------------------|----------------------------------------------------------------------------------------|
| **Nome**                      | *&lt;Nome da API&gt;*  | Insira um **nome** para o aplicativo que descrevem seu aplicativo para os consumidores.                     |
| **Incluir o aplicativo web / da web API** | Sim                 |                                                                                        |
| **Permitir que o fluxo implícito**       | Sim                 |                                                                                        |
| **URL de resposta**                 | `https://localhost` | URLs de resposta são pontos de extremidade em que o Azure AD B2C retorna todos os tokens que solicita a seu aplicativo. |
| **URI da ID do aplicativo**                | *api*               | O URI não precisa ser resolvido para um endereço físico. Ele só precisa ser exclusivo.     |
| **Incluir o cliente nativo**     | Não                  |                                                                                        |

Depois que a API é registrada, é exibida a lista de aplicativos e APIs no locatário. Selecione a API que acabou de ser registrada. Selecione o **cópia** ícone à direita do **ID do aplicativo** campo para copiá-lo para a área de transferência. Selecione **publicado escopos** e verifique se o padrão *user_impersonation* escopo está presente.

## <a name="create-an-aspnet-core-app-in-visual-studio-2017"></a>Criar um aplicativo do ASP.NET Core no Visual Studio de 2017

O modelo de aplicativo de Web do Visual Studio pode ser configurado para usar o locatário do Azure AD B2C para autenticação.

No Visual Studio:

1. Crie um novo Aplicativo Web ASP.NET Core. 
2. Selecione **API da Web** da lista de modelos.
3. Selecione o **alterar autenticação** botão.
    
    ![Botão de alteração de autenticação](./azure-ad-b2c-webapi/change-auth-button.png)

4. No **alterar autenticação** caixa de diálogo, selecione **contas de usuário individuais**e, em seguida, selecione **conectar a um repositório de usuário existente na nuvem** na lista suspensa. 
    
    ![Caixa de diálogo Alterar autenticação](./azure-ad-b2c-webapi/change-auth-dialog.png)

5. Preencha o formulário com os seguintes valores:
    
    | Configuração                       | Valor                                                 |
    |-------------------------------|-------------------------------------------------------|
    | **Nome de domínio**               | *&lt;o nome de domínio do seu locatário B2C&gt;*          |
    | **ID do aplicativo**            | *&lt;Cole a ID do aplicativo da área de transferência&gt;* |
    | **Política de inscrever-se ou entrar** | `B2C_1_SiUpIn`                                        |
    
    Selecione **Okey** para fechar o **alterar autenticação** caixa de diálogo. Selecione **Okey** para criar o aplicativo web.

O Visual Studio cria a API da web com um controlador chamado *ValuesController.cs* que retorna valores embutidos para solicitações GET. A classe está decorada com o [autorizar atributo](xref:security/authorization/simple), portanto, todas as solicitações requerem autenticação.

## <a name="run-the-web-api"></a>Executar a API da web

No Visual Studio, execute a API. Visual Studio inicia um navegador apontado URL da raiz da API. Anote a URL na barra de endereços e deixe a API em execução em segundo plano.

> [!NOTE]
> Como não há nenhum controlador definido para a URL raiz, o navegador exibe um erro 404 de (página não encontrada). Esse comportamento é esperado.

## <a name="use-postman-to-get-a-token-and-test-the-api"></a>Use carteiro para obter um token e a API de teste

[Carteiro](https://getpostman.com/postman) é uma ferramenta para testar APIs da web. Para este tutorial, carteiro simula um aplicativo web que acessa a API da web em nome do usuário.

### <a name="register-postman-as-a-web-app"></a>Registrar carteiro como um aplicativo web

Como carteiro simula um aplicativo web que pode obter tokens do locatário do Azure AD B2C, ele deve ser registrado no locatário como um aplicativo web. Registrar carteiro usando [as etapas na documentação do](/azure/active-directory-b2c/active-directory-b2c-app-registration#register-a-web-app) sob o **registrar um aplicativo web** seção. Parar no **criar um segredo de cliente do aplicativo web** seção. Um segredo do cliente não é necessário para este tutorial. 

Use os seguintes valores:

| Configuração                       | Valor                            | Observações                           |
|-------------------------------|----------------------------------|---------------------------------|
| **Nome**                      | Carteiro                          |                                 |
| **Incluir o aplicativo web / da web API** | Sim                              |                                 |
| **Permitir que o fluxo implícito**       | Sim                              |                                 |
| **URL de resposta**                 | `https://getpostman.com/postman` |                                 |
| **URI da ID do aplicativo**                | *&lt;Deixe em branco&gt;*            | Não é necessário para este tutorial. |
| **Incluir o cliente nativo**     | Não                               |                                 |

O aplicativo web recém-registrados precisa de permissão para acessar a API da web em nome do usuário.  

1. Selecione **carteiro** na lista de aplicativos e, em seguida, selecione **acesso à API** no menu à esquerda.
2. Selecione **+ adicionar**.
3. No **selecione API** lista suspensa, selecione o nome da API web.
4. No **selecione escopos** suspenso, certifique-se de que todos os escopos são selecionados.
5. Selecione **Okey**.

Observe o ID do aplicativo do aplicativo carteiro, pois, é necessário para obter um token de portador.

### <a name="create-a-postman-request"></a>Criar uma solicitação de carteiro

Inicie o carteiro. Por padrão, exibe carteiro a **criar novo** caixa de diálogo no início. Se a caixa de diálogo não for exibida, selecione o **+ novo** botão no canto superior esquerdo.

Do **criar novo** caixa de diálogo:

1. Selecione **solicitação**.
    
    ![Botão de solicitação](./azure-ad-b2c-webapi/postman-create-new.png)

2. Digite *obter valores* no **nome da solicitação** caixa.
3. Selecione **+ Criar coleção** para criar uma nova coleção para armazenar a solicitação. Nome da coleção *tutoriais do ASP.NET Core* e, em seguida, selecione a marca de seleção.
    
    ![Criar uma nova coleção](./azure-ad-b2c-webapi/postman-create-collection.png)

4. Selecione o **Salvar para tutoriais do ASP.NET Core** botão.

### <a name="test-the-web-api-withoutauthentication"></a>Testar o withoutauthentication API da web

Para verificar que a API da web requer autenticação, primeiro verifique uma solicitação sem autenticação.

1. No **insira a URL de solicitação** , digite a URL para `ValuesController`. A URL é a mesma exibida no navegador com **api/valores** anexado. Um exemplo seria `https://localhost:44375/api/values`.
2. Selecione o **enviar** botão.
3. Observe o status da resposta é *401 não autorizado*.

    ![401 não autorizado a resposta](./azure-ad-b2c-webapi/postman-401-status.png)

### <a name="obtain-a-bearer-token"></a>Obter um token de portador

Para fazer uma solicitação autenticada para a API da web, é necessário um token de portador. Carteiro facilita a entrar no locatário do Azure AD B2C e obter um token.

1. No **autorização** guia o **tipo** lista suspensa, selecione **OAuth 2.0**. No **adicionar dados de autorização para** lista suspensa, selecione **cabeçalhos de solicitação**. Selecione **obter Token de acesso novo**.
    
    ![Guia de autorização com configurações](./azure-ad-b2c-webapi/postman-auth-tab.png)

2. Concluir o **obter novo TOKEN de acesso** caixa de diálogo da seguinte maneira:
    
    | Configuração                   | Valor                                                                                         | Observações                                                                                      |
    |---------------------------|-----------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------|
    | **Nome do token**            | *&lt;nome do token&gt;*                                                                          | Insira um nome descritivo para o token.                                                    |
    | **Tipo de concessão**            | Implícita                                                                                      |                                                                                            |
    | **URL de retorno de chamada**          | `https://getpostman.com/postman`                                                              |                                                                                            |
    | **URL de autenticação**              | `https://login.microsoftonline.com/<tenant domain name>/oauth2/v2.0/authorize?p=B2C_1_SiUpIn` | Substituir  *&lt;nome de domínio de locatário&gt;*  com o nome de domínio do locatário sem os colchetes angulares. |
    | **ID do cliente**             | *&lt;Insira o aplicativo de carteiro <b>ID do aplicativo</b>&gt;*                                       |                                                                                            |
    | **Segredo do cliente**         | *&lt;Deixe em branco&gt;*                                                                         |                                                                                            |
    | **Escopo**                 | `https://<tenant domain name>/api/user_impersonation openid offline_access`                   | Substituir  *&lt;nome de domínio de locatário&gt;*  com o nome de domínio do locatário sem os colchetes angulares. |
    | **Autenticação de cliente** | Enviar as credenciais do cliente no corpo                                                               |                                                                                            |
    
3. Selecione o **solicitação de Token** botão.

4. Carteiro abre uma nova janela que contém a caixa de diálogo de entrada do locatário do Azure AD B2C. Entrar com uma conta existente (se foi criado as políticas de teste) ou selecione **inscrever-se agora** para criar uma nova conta. O **esqueceu sua senha?** link é usado para redefinir uma senha esquecida.

5. Depois de entrar com êxito no, a janela será fechada e o **gerenciar TOKENS de acesso** caixa de diálogo é exibida. Role para baixo até a parte inferior e selecione o **uso Token** botão.
    
    ![Onde encontrar o botão "Token de uso"](./azure-ad-b2c-webapi/postman-access-token.png)

### <a name="test-the-web-api-with-authentication"></a>A API da web com autenticação de teste

Selecione o **enviar** botão para enviar a solicitação novamente. Neste momento, o status da resposta é *200 Okey* e a carga JSON é visível na resposta **corpo** guia.
    
![Status de êxito e de carga](./azure-ad-b2c-webapi/postman-success.png)

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você aprendeu como:

> [!div class="checklist"]
> * Crie um locatário do Azure Active Directory B2C.
> * Registre uma API da Web no B2C do Azure AD.
> * Use o Visual Studio para criar uma API da Web configurado para usar o locatário do Azure AD B2C para autenticação.
> * Configure políticas que controlam o comportamento do locatário do Azure AD B2C.
> * Use carteiro para simular um aplicativo web que apresenta uma caixa de diálogo de logon, recupera um token e usa para fazer uma solicitação em relação a API da web.

Continue a desenvolver sua API aprendendo para:

* [Proteger um ASP.NET Core aplicativo da web usando o Azure AD B2C](xref:security/authentication/azure-ad-b2c).
* [Chamar uma API da web do .NET a partir de um aplicativo web do .NET usando o Azure AD B2C](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-web-api-dotnet).
* [Personalizar a interface do usuário do Azure AD B2C](/azure/active-directory-b2c/active-directory-b2c-reference-ui-customization).
* [Configurar requisitos de complexidade de senha](/azure/active-directory-b2c/active-directory-b2c-reference-password-complexity).
* [Habilitar a autenticação multifator](/azure/active-directory-b2c/active-directory-b2c-reference-mfa).
* Configurar provedores de identidade adicional, como [Microsoft](/azure/active-directory-b2c/active-directory-b2c-setup-msa-app), [Facebook](/azure/active-directory-b2c/active-directory-b2c-setup-fb-app), [Google](/azure/active-directory-b2c/active-directory-b2c-setup-goog-app), [Amazon](/azure/active-directory-b2c/active-directory-b2c-setup-amzn-app), [do Twitter ](/azure/active-directory-b2c/active-directory-b2c-setup-twitter-app)e outros.
* [Use a API do Azure AD Graph](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-graph-dotnet) para recuperar informações de usuário adicionais, como membros do grupo, do locatário do Azure AD B2C.
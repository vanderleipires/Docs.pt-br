---
title: "Autenticação de nuvem com o Azure Active Directory B2C"
author: camsoper
description: "Saiba como configurar a autenticação do Azure Active Directory B2C com ASP.NET Core."
manager: wpickett
ms.date: 01/25/2018
ms.topic: tutorial
ms.technology: aspnet
ms.prod: asp.net-core
ms.custom: mvc
uid: security/authentication/azure-ad-b2c
ms.openlocfilehash: d60698b5798e837a5946dbe158a647aae9e149d4
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/30/2018
---
# <a name="cloud-authentication-with-azure-active-directory-b2c"></a>Autenticação de nuvem com o Azure Active Directory B2C

Por [Cam Soper](https://twitter.com/camsoper)

[B2C de diretório ativo do Azure](/azure/active-directory-b2c/active-directory-b2c-overview) (Azure AD B2C) é uma solução de gerenciamento de identidade de nuvem para aplicativos web e móveis. O serviço fornece autenticação para aplicativos hospedados na nuvem e local. Tipos de autenticação incluem incluem contas individuais, contas de rede social e federados contas corporativas. Além disso, o Azure AD B2C pode fornecer autenticação multifator com configuração mínima.

> [!TIP]
> Azure Active Directory (AD do Azure) do Azure AD B2C são ofertas de produtos separados. Um locatário do AD do Azure representa uma organização, enquanto um locatário Azure AD B2C representa uma coleção de identidades a serem usadas com aplicativos de terceira parte confiável. Para obter mais informações, consulte [do Azure AD B2C: perguntas frequentes (FAQ)](/azure/active-directory-b2c/active-directory-b2c-faqs).

Neste tutorial, saiba como:

> [!div class="checklist"]
> * Crie um locatário do Azure Active Directory B2C
> * Registrar um aplicativo no Azure B2C do AD
> * Usar o Visual Studio para criar um aplicativo web do ASP.NET Core configurado para usar o locatário do Azure AD B2C para autenticação
> * Configurar políticas que controlam o comportamento do locatário do Azure AD B2C

## <a name="prerequisites"></a>Pré-requisitos

A seguir é necessários para este passo a passo:

* [Assinatura do Microsoft Azure](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
* [Visual Studio de 2017](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs) (qualquer edição)

## <a name="create-the-azure-active-directory-b2c-tenant"></a>Criar o locatário do Azure Active Directory B2C

Crie um locatário do Azure Active Directory B2C [conforme descrito na documentação do](/azure/active-directory-b2c/active-directory-b2c-get-started). Quando solicitado, associar o locatário com uma assinatura do Azure é opcional para este tutorial.

## <a name="register-the-app-in-azure-ad-b2c"></a>Registrar o aplicativo em B2C do Azure AD

O locatário do Azure AD B2C recém-criado, registre seu aplicativo usando [as etapas na documentação do](/azure/active-directory-b2c/active-directory-b2c-app-registration#register-a-web-app) sob o **registrar um aplicativo web** seção. Parar no **criar um segredo de cliente do aplicativo web** seção. Um segredo do cliente não é necessário para este tutorial. 

Use os seguintes valores:

| Configuração                       | Valor                     | Observações                                                                                                                                                                                              |
|-------------------------------|---------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Nome**                      | *&lt;nome do aplicativo&gt;*        | Insira um **nome** para o aplicativo que descrevem seu aplicativo para os consumidores.                                                                                                                                 |
| **Incluir o aplicativo web / da web API** | Sim                       |                                                                                                                                                                                                    |
| **Permitir que o fluxo implícito**       | Sim                       |                                                                                                                                                                                                    |
| **URL de resposta**                 | `https://localhost:44300` | URLs de resposta são pontos de extremidade em que o Azure AD B2C retorna todos os tokens que solicita a seu aplicativo. O Visual Studio fornece a URL de resposta para usar. Por enquanto, digite `https://localhost:44300` para preencher o formulário. |
| **URI da ID do aplicativo**                | Deixe em branco               | Não é necessário para este tutorial.                                                                                                                                                                    |
| **Incluir o cliente nativo**     | Não                        |                                                                                                                                                                                                    |

> [!WARNING]
> Se a configuração de uma URL de resposta não localhost, esteja ciente do [restrições sobre o que é permitido na lista de URL de resposta](/azure/active-directory-b2c/active-directory-b2c-app-registration#choosing-a-web-app-or-api-reply-url). 

Depois que o aplicativo for registrado, é exibida a lista de aplicativos no locatário. Selecione o aplicativo que acabou de ser registrado. Selecione o **cópia** ícone à direita do **ID do aplicativo** campo para copiá-lo para a área de transferência.

Nada mais podem ser configurado no locatário do Azure AD B2C neste momento, mas deixam a janela do navegador aberta. Há mais de configuração depois que o aplicativo ASP.NET Core é criado.

## <a name="create-an-aspnet-core-app-in-visual-studio-2017"></a>Criar um aplicativo do ASP.NET Core no Visual Studio de 2017

O modelo de aplicativo de Web do Visual Studio pode ser configurado para usar o locatário do Azure AD B2C para autenticação.

No Visual Studio:

1. Crie um novo Aplicativo Web ASP.NET Core. 
2. Selecione **aplicativo Web** da lista de modelos.
3. Selecione o **alterar autenticação** botão.
    
    ![Botão de alteração de autenticação](./azure-ad-b2c/_static/changeauth.png)

4. No **alterar autenticação** caixa de diálogo, selecione **contas de usuário individuais**e, em seguida, selecione **conectar a um repositório de usuário existente na nuvem** na lista suspensa. 
    
    ![Caixa de diálogo Alterar autenticação](./azure-ad-b2c/_static/changeauthdialog.png)

5. Preencha o formulário com os seguintes valores:
    
    | Configuração                       | Valor                                                 |
    |-------------------------------|-------------------------------------------------------|
    | **Nome de domínio**               | *&lt;o nome de domínio do seu locatário B2C&gt;*          |
    | **ID do aplicativo**            | *&lt;Cole a ID do aplicativo da área de transferência&gt;* |
    | **Caminho de retorno de chamada**             | *&lt;Use o valor padrão&gt;*                       |
    | **Política de inscrever-se ou entrar** | `B2C_1_SiUpIn`                                        |
    | **Política de redefinição de senha**     | `B2C_1_SSPR`                                          |
    | **Editar política de perfil**       | *&lt;Deixe em branco&gt;*                                 |
    
    Selecione o **cópia** próximo ao link **URI Reply** para copiar o URI de resposta para a área de transferência. Selecione **Okey** para fechar o **alterar autenticação** caixa de diálogo. Selecione **Okey** para criar o aplicativo web.

## <a name="finish-the-b2c-app-registration"></a>Concluir o registro do aplicativo B2C

Retornar à janela do navegador com as propriedades do aplicativo B2C ainda abertas. Alterar temporárias **URL de resposta** especificado anteriormente para o valor copiado do Visual Studio. Selecione **salvar** na parte superior da janela.

> [!TIP]
> Se você não copiar a URL de resposta, use o endereço SSL da guia depuração nas propriedades do projeto web e acrescente o **CallbackPath** valor *appSettings. JSON*.

## <a name="configure-policies"></a>Configurar políticas

Use as etapas da documentação do Azure AD B2C [criar uma política de inscrever-se ou entrar](/azure/active-directory-b2c/active-directory-b2c-reference-policies#create-a-sign-up-or-sign-in-policy)e, em seguida, [criar uma política de redefinição de senha](/azure/active-directory-b2c/active-directory-b2c-reference-policies#create-a-password-reset-policy). Use os valores de exemplo fornecidos na documentação do **provedores de identidade**, **inscrição atributos**, e **declarações de aplicativo**. Usando o **executar agora** botão para testar as políticas, conforme descrito na documentação é opcional.

> [!WARNING]
> Verifique os nomes de política são exatamente conforme descrito na documentação, como as políticas foram usadas no **alterar autenticação** caixa de diálogo no Visual Studio. Os nomes de política podem ser verificados no *appSettings. JSON*.

## <a name="run-the-app"></a>Executar o aplicativo

No Visual Studio, pressione **F5** para compilar e executar o aplicativo. Depois que o aplicativo web é iniciado, selecione **entrar**.

![Entrar no aplicativo](./azure-ad-b2c/_static/signin.png)

O navegador é redirecionado para o locatário do Azure AD B2C. Entrar com uma conta existente (se foi criado as políticas de teste) ou selecione **inscrever-se agora** para criar uma nova conta. O **esqueceu sua senha?** link é usado para redefinir uma senha esquecida.

![Logon do AD B2C do Azure](./azure-ad-b2c/_static/b2csts.png)

Depois de entrar com êxito no, o navegador é redirecionado ao aplicativo web.

![Êxito](./azure-ad-b2c/_static/success.png)

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você aprendeu como:

> [!div class="checklist"]
> * Crie um locatário do Azure Active Directory B2C
> * Registrar um aplicativo no Azure B2C do AD
> * Usar o Visual Studio para criar um aplicativo de Web do ASP.NET Core configurado para usar o locatário do Azure AD B2C para autenticação
> * Configurar políticas que controlam o comportamento do locatário do Azure AD B2C

O aplicativo do ASP.NET Core já está configurado para usar o Azure AD B2C para autenticação, o [autorizar atributo](xref:security/authorization/simple) pode ser usado para proteger seu aplicativo. Continue a desenvolver seu aplicativo pelo aprendizado para:

* [Personalizar a interface do usuário do Azure AD B2C](/azure/active-directory-b2c/active-directory-b2c-reference-ui-customization).
* [Configurar requisitos de complexidade de senha](/azure/active-directory-b2c/active-directory-b2c-reference-password-complexity).
* [Habilitar a autenticação multifator](/azure/active-directory-b2c/active-directory-b2c-reference-mfa).
* Configurar provedores de identidade adicional, como [Microsoft](/azure/active-directory-b2c/active-directory-b2c-setup-msa-app), [Facebook](/azure/active-directory-b2c/active-directory-b2c-setup-fb-app), [Google](/azure/active-directory-b2c/active-directory-b2c-setup-goog-app), [Amazon](/azure/active-directory-b2c/active-directory-b2c-setup-amzn-app), [do Twitter ](/azure/active-directory-b2c/active-directory-b2c-setup-twitter-app)e outros.
* [Use a API do Azure AD Graph](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-graph-dotnet) para recuperar informações de usuário adicionais, como membros do grupo, do locatário do Azure AD B2C.
* [Proteger um ASP.NET Core API da web usando o Azure AD B2C](xref:security/authentication/azure-ad-b2c-api).
* [Chamar uma API da web do .NET a partir de um aplicativo web do .NET usando o Azure AD B2C](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-web-api-dotnet).

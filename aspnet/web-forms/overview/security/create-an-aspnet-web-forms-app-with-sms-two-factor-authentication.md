---
uid: web-forms/overview/security/create-an-aspnet-web-forms-app-with-sms-two-factor-authentication
title: Criar um ASP.NET Web Forms aplicativo com autenticação de dois fatores do SMS (c#) | Microsoft Docs
author: Erikre
description: Este tutorial mostra como criar um aplicativo de Web Forms do ASP.NET com autenticação de dois fatores. Este tutorial foi desenvolvido para complementar o tutorial intitulado Cr...
ms.author: riande
ms.date: 10/09/2014
ms.assetid: 716264ae-ab72-45de-bfc5-53a6237089cf
msc.legacyurl: /web-forms/overview/security/create-an-aspnet-web-forms-app-with-sms-two-factor-authentication
msc.type: authoredcontent
ms.openlocfilehash: 7ad3b7a453a40f2708902ae5b9e5cb75b931d54d
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41824974"
---
<a name="create-an-aspnet-web-forms-app-with-sms-two-factor-authentication-c"></a>Criar um ASP.NET Web Forms aplicativo com autenticação de dois fatores do SMS (c#)
====================
por [Erik Reitan](https://github.com/Erikre)

[Baixe o aplicativo de formulários da Web do ASP.NET com o Email e SMS de autenticação de dois fatores](https://code.msdn.microsoft.com/ASPNET-Web-Forms-App-with-5a0ff94e)

> Este tutorial mostra como criar um aplicativo de Web Forms do ASP.NET com autenticação de dois fatores. Este tutorial foi desenvolvido para complementar o tutorial intitulado [criar um aplicativo de Web Forms do ASP.NET seguro com o registro de usuário, redefinição de senha e de confirmação de email](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset.md). Além disso, este tutorial se baseia de Rick Anderson [tutorial MVC](../../../mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md).


## <a name="introduction"></a>Introdução

Este tutorial orienta você pelas etapas necessárias para criar um aplicativo ASP.NET Web Forms que dá suporte à autenticação de dois fatores usando o Visual Studio. Autenticação de dois fatores é uma etapa de autenticação de usuário adicionais. Essa etapa extra gera um número exclusivo de identificação pessoal (PIN) durante o logon. O PIN é normalmente enviado para o usuário como um email ou mensagem SMS. O usuário do seu aplicativo, em seguida, insere o PIN como uma medida de autenticação extra ao entrar.

### <a name="tutorial-tasks-and-information"></a>Tutoriais tarefas e informações:

- [Criar um aplicativo de formulários da Web do ASP.NET](#createWebForms)
- [Configurar a autenticação de dois fatores e SMS](#SMS)
- [Habilitar a autenticação de dois fatores para usuários registrados](#use2FA)
- [Recursos adicionais](#addRes)

<a id="createWebForms"></a>
## <a name="create-an-aspnet-web-forms-app"></a>Criar um aplicativo de formulários da Web do ASP.NET

Comece instalando e executando [Visual Studio Express 2013 para Web](https://go.microsoft.com/fwlink/?LinkId=299058) ou [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Instale [Visual Studio 2013 atualização 3](https://go.microsoft.com/fwlink/?LinkId=390465) ou superior também. Além disso, você precisará criar uma [Twilio](https://www.twilio.com/try-twilio) de conta, conforme explicado abaixo.

> [!NOTE]
> Importante: Você deve instalar [Visual Studio 2013 atualização 3](https://go.microsoft.com/fwlink/?LinkId=390465) ou superior para concluir este tutorial.


1. Criar um novo projeto (**arquivo**  - &gt; **novo projeto**) e selecione o **aplicativo Web ASP.NET** modelo juntamente com o .NET Framework a versão 4.5.2 do **novo projeto** caixa de diálogo.
2. Dos **novo projeto ASP.NET** caixa de diálogo, selecione o **Web Forms** modelo. Deixe a autenticação padrão como **contas de usuário individuais**. Em seguida, clique em **Okey** para criar o novo projeto.  
    ![Caixa de diálogo Novo projeto ASP.NET](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image1.png)
3. Habilite o Secure Sockets Layer (SSL) para o projeto. Siga as etapas disponíveis na **habilitar o SSL para o projeto** seção o [Introdução a série de tutoriais de Web Forms](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms).
4. No Visual Studio, abra o **Package Manager Console** (**ferramentas**  - &gt; **Gerenciador de pacotes NuGet**  - &gt; **Package Manager Console**) e digite o seguinte comando:  
    `Install-Package Twilio`

<a id="SMS"></a>
## <a name="setup-sms-and-two-factor-authentication"></a>Configurar a autenticação de dois fatores e SMS

Este tutorial usa o Twilio, mas você pode usar qualquer provedor SMS.

1. Criar uma [Twilio](https://www.twilio.com/try-twilio) conta.
2. Do **Dashboard** guia da sua conta do Twilio, copie o **SID de conta** e **Token de autenticação.** Você adicionará ao seu aplicativo mais tarde.
3. Dos **números** guia, copie seu Twilio **número de telefone** também.
4. Fazer o Twilio **SID da conta**, **Auth Token** e **número de telefone** disponíveis para o aplicativo. Para manter as coisas simples, você armazenará esses valores na *Web. config* arquivo. Quando você implanta no Azure, você pode armazenar os valores com segurança os **appSettings** seção no site da web na guia Configurar. Além disso, ao adicionar o número de telefone, use apenas números.   
   Observe que você também pode adicionar as credenciais do SendGrid. O SendGrid é um serviço de notificação de email. Para obter detalhes sobre como habilitar o SendGrid, consulte a seção de 'Gancho backup SendGrid' do tutorial intitulado [criar um aplicativo seguro do ASP.NET Web Forms com o registro de usuário, redefinição de senha e de confirmação de email.](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset.md)

    [!code-xml[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample1.xml?highlight=2,6-10)]

    > [!WARNING]
    > Security - nunca armazenar os dados confidenciais em seu código-fonte. Neste exemplo, a conta e as credenciais são armazenadas do **appSettings** seção o *Web. config* arquivo. No Azure, você pode armazenar com segurança esses valores sobre o **[configurar](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** guia no portal do Azure. Para obter informações relacionadas, consulte o tópico de Rick Anderson [práticas recomendadas para implantar senhas e outros dados confidenciais no ASP.NET e o Azure](https://go.microsoft.com/fwlink/?LinkId=513141).
5. Configurar o `SmsService` classe a *App\_Start\IdentityConfig.cs* realçado em amarelo de alterações de arquivo fazendo o seguinte: 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample2.cs?highlight=5-17)]
6. Adicione o seguinte `using` instruções ao início dos *IdentityConfig.cs* arquivo: 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample3.cs?highlight=1-4)]
7. Atualizar o *Account/Manage.aspx* arquivo removendo as linhas destacadas em amarelo:  

    [!code-aspx[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample4.aspx?highlight=38,53,57-60,63,66,70,73)]
8. No `Page_Load` manipulador do *Manage.aspx.cs* de lógica, remova a linha de código realçada em amarelo, de modo que ele apareça como segue: 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample5.cs?highlight=8)]
9. No codebehind da *conta*/*TwoFactorAuthenticationSignIn.aspx.cs*, atualize o `Page_Load` manipulador adicionando o seguinte código realçado em amarelo: 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample6.cs?highlight=3-4,13)]

   Ao tornar o código acima para alterar, DropDownList "Provedores de" que contém as opções de autenticação não será redefinido para o primeiro valor. Isso permitirá que o usuário com êxito, selecione todas as opções para usar quando autenticar, não apenas o primeiro.
10. Na **Gerenciador de soluções**, clique com botão direito *default. aspx* e selecione **definir como página inicial**.
11. Testando seu aplicativo, primeiro Compile o aplicativo (**Ctrl**+**Shift**+**B**) e, em seguida, execute o aplicativo (**F5**) e Selecione **registre** para criar uma nova conta de usuário ou selecione **faça logon no** se a conta de usuário já foi registrada.
12. Depois de você (como o usuário) tiver conectado, clique na ID do usuário (endereço de email) na barra de navegação para exibir o **Gerenciar conta** página (Manage.aspx).  
    ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image2.png)
13. Clique em **Add** lado **número de telefone** sobre o **Gerenciar conta** página.  
    ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image3.png)
14. Adicionar o número de telefone no qual você (como o usuário) gostaria de receber mensagens SMS (mensagens de texto) e clique no **enviar** botão.   
    ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image4.png)  
    Neste ponto, o aplicativo usará as credenciais do *Web. config* entrar em contato com o Twilio. Para o telefone associado com a conta de usuário receberá uma mensagem SMS (mensagem de texto). Você pode verificar que a mensagem do Twilio foi enviada pela exibição do painel do Twilio.
15. Em poucos segundos, o telefone associado com a conta de usuário receberá uma mensagem de texto que contém o código de verificação. Insira o código de verificação e pressione **enviar**.  
     ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image5.png)

<a id="use2FA"></a>
## <a name="enable-two-factor-authentication-for-a-registered-user"></a>Habilitar a autenticação de dois fatores para um usuário registrado

Neste ponto, você habilitou a autenticação de dois fatores para seu aplicativo. Para um usuário usar a autenticação de dois fatores, ele pode simplesmente mudar suas configurações usando a interface do usuário. 

1. Como um usuário de seu aplicativo você pode habilitar autenticação de dois fatores para sua conta específica clicando na ID do usuário (alias de email) na barra de navegação para exibir o **Gerenciar conta** página. Em seguida, clique no **habilitar** link para habilitar a autenticação de dois fatores para a conta.![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image6.png)
2. Fazer logoff e, em seguida, faça logon novamente. Se você tiver habilitado o email, você pode selecionar o SMS ou email para a autenticação de dois fatores. Se você ainda não tiver ativado o email, consulte o tutorial intitulado [criar um aplicativo seguro do ASP.NET Web Forms com o registro do usuário, confirmação por Email e redefinição de senha](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset.md).![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image7.png)
3. A página de autenticação de dois fatores é exibida em que você pode inserir o código (de SMS ou email).![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image8.png)  
 Clicar na **lembrar deste navegador** caixa de seleção serão isentos da necessidade de usar a autenticação de dois fatores para fazer logon ao usar o navegador e o dispositivo em que você tiver marcado a caixa. Desde que usuários mal-intencionados não podem obter acesso ao seu dispositivo, habilitando a autenticação de dois fatores e clicando na **lembrar deste navegador** lhe fornecerá acesso conveniente de senha uma única etapa, enquanto ainda retém strong proteção de autenticação de dois fatores para todo o acesso de dispositivos não confiáveis. Você pode fazer isso em qualquer dispositivo privado que você usa regularmente.

<a id="addRes"></a>
## <a name="additional-resources"></a>Recursos adicionais

- [Autenticação de dois fatores usando SMS e email com a Identidade do ASP.NET](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md)
- [Recursos recomendados de links para a identidade do ASP.NET](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [Implantar um aplicativo de formulários da Web do ASP.NET seguro com associação, OAuth e banco de dados SQL em um site do Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)
- [Série de tutoriais do ASP.NET Web Forms - adicionar um provedor de OAuth 2.0](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#OAuthWebForms)
- [Série de tutoriais de Web Forms do ASP.NET - habilitar o SSL para o projeto](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms)
- [Confirmação de conta e recuperação de senha com o ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [Criando o aplicativo no Facebook e conectar o aplicativo ao projeto](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)

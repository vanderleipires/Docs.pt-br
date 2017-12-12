---
uid: web-forms/overview/security/create-an-aspnet-web-forms-app-with-sms-two-factor-authentication
title: "Criar um Web ASP.NET Forms aplicativo com autenticação de dois fatores do SMS (c#) | Microsoft Docs"
author: Erikre
description: "Este tutorial mostra como criar um aplicativo de Web Forms do ASP.NET com autenticação de dois fatores. Este tutorial foi desenvolvido para complementar o tutorial intitulado Cr..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/09/2014
ms.topic: article
ms.assetid: 716264ae-ab72-45de-bfc5-53a6237089cf
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/security/create-an-aspnet-web-forms-app-with-sms-two-factor-authentication
msc.type: authoredcontent
ms.openlocfilehash: 92ab5f2d7a9a29089f3d340849e229d015613509
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="create-an-aspnet-web-forms-app-with-sms-two-factor-authentication-c"></a>Criar um Web ASP.NET Forms aplicativo com autenticação de dois fatores do SMS (c#)
====================
Por [Erik Reitan](https://github.com/Erikre)

[Baixe o aplicativo de formulários da Web do ASP.NET com Email e SMS de autenticação de dois fatores](https://code.msdn.microsoft.com/ASPNET-Web-Forms-App-with-5a0ff94e)

> Este tutorial mostra como criar um aplicativo de Web Forms do ASP.NET com autenticação de dois fatores. Este tutorial foi desenvolvido para complementar o tutorial intitulado [criar um aplicativo ASP.NET Web Forms seguro com o registro de usuário, redefinição de senha e de confirmação de email](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset.md). Além disso, este tutorial baseia de Rick Anderson [tutorial MVC](../../../mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md).


## <a name="introduction"></a>Introdução

Este tutorial orienta você pelas etapas necessárias para criar um aplicativo ASP.NET Web Forms que dá suporte à autenticação de dois fatores usando o Visual Studio. Autenticação de dois fatores é uma etapa de autenticação de usuário adicionais. Esta etapa extra gera um número exclusivo de identificação pessoal (PIN) durante o logon. O PIN geralmente é enviado ao usuário como um email ou mensagem SMS. O usuário do seu aplicativo, em seguida, insira o PIN como medida adicional de autenticação ao entrar no.

### <a name="tutorial-tasks-and-information"></a>Tutoriais tarefas e informações:

- [Criar um aplicativo de formulários da Web do ASP.NET](#createWebForms)
- [Instalação de SMS e autenticação de dois fatores](#SMS)
- [Habilitar a autenticação de dois fatores para usuário registrado](#use2FA)
- [Recursos adicionais](#addRes)

<a id="createWebForms"></a>
## <a name="create-an-aspnet-web-forms-app"></a>Criar um aplicativo de formulários da Web do ASP.NET

Comece instalando e executando [Visual Studio Express 2013 para Web](https://go.microsoft.com/fwlink/?LinkId=299058) ou [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Instalar [Visual Studio 2013 atualização 3](https://go.microsoft.com/fwlink/?LinkId=390465) ou superior também. Além disso, você precisará criar um [Twilio](https://www.twilio.com/try-twilio) conta, conforme explicado a seguir.

> [!NOTE]
> Importante: Você deve instalar [Visual Studio 2013 atualização 3](https://go.microsoft.com/fwlink/?LinkId=390465) ou superior para concluir este tutorial.


1. Criar um novo projeto (**arquivo**  - &gt; **novo projeto**) e selecione o **aplicativo Web ASP.NET** modelo junto com o .NET Framework versão 4.5.2 a partir de **novo projeto** caixa de diálogo.
2. Do **novo projeto ASP.NET** caixa de diálogo, selecione o **Web Forms** modelo. Deixe a autenticação padrão como **contas de usuário individuais**. Em seguida, clique em **Okey** para criar o novo projeto.  
    ![Caixa de diálogo Novo projeto ASP.NET](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image1.png)
3. Habilite o protocolo (SSL) para o projeto. Siga as etapas disponíveis no **habilitar SSL para o projeto** seção o [Introdução à série de tutoriais de Web Forms](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms).
4. No Visual Studio, abra o **Package Manager Console** (**ferramentas**  - &gt; **Gerenciador de pacote do NuGet**  - &gt; **Package Manager Console**) e digite o seguinte comando:  
    `Install-Package Twilio`

<a id="SMS"></a>
## <a name="setup-sms-and-two-factor-authentication"></a>Instalação de SMS e autenticação de dois fatores

Este tutorial usa Twilio, mas você pode usar qualquer provedor SMS.

1. Criar um [Twilio](https://www.twilio.com/try-twilio) conta.
2. Do **painel** guia da sua conta do Twilio, copie o **SID de conta** e **Token de autenticação.** Adicione-os ao seu aplicativo mais tarde.
3. Do **números** guia, copie o Twilio **número de telefone** também.
4. Verifique o Twilio **SID de conta**, **Token de autenticação** e **número de telefone** disponíveis para o aplicativo. Para manter as coisas simples, você armazenará esses valores no *Web. config* arquivo. Quando você implanta no Azure, você pode armazenar os valores de segurança no **appSettings** guia Configurar de seção no site da web. Além disso, quando adicionar o número de telefone, use apenas números.   
 Observe que você também pode adicionar as credenciais do SendGrid. SendGrid é um serviço de notificação de email. Para obter detalhes sobre como habilitar o SendGrid, consulte a seção de 'Gancho backup SendGrid' do tutorial intitulada [criar um aplicativo seguro do ASP.NET Web Forms com o registro de usuário, a redefinição de senha e de confirmação de email.](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset.md)

    [!code-xml[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample1.xml?highlight=2,6-10)]

    > [!WARNING]
    > Segurança - nunca armazenar os dados confidenciais em seu código-fonte. Neste exemplo, a conta e as credenciais são armazenadas no **appSettings** seção o *Web. config* arquivo. No Azure, você pode armazenar com segurança esses valores de  **[configurar](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)**  no portal do Azure. Para obter informações relacionadas, consulte o tópico de Rick Anderson [práticas recomendadas para a implantação de senhas e outros dados confidenciais em ASP.NET e o Azure](https://go.microsoft.com/fwlink/?LinkId=513141).
5. Configurar o `SmsService` classe no *aplicativo\_Start\IdentityConfig.cs* realçados em amarelo alterações em arquivos, fazendo o seguinte: 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample2.cs?highlight=5-17)]
6. Adicione o seguinte `using` instruções para o início do *IdentityConfig.cs* arquivo: 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample3.cs?highlight=1-4)]
7. Atualização de *Account/Manage.aspx* arquivo removendo as linhas destacadas em amarelo:  

    [!code-aspx[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample4.aspx?highlight=38,53,57-60,63,66,70,73)]
8. No `Page_Load` manipulador do *Manage.aspx.cs* por trás do código, descomente a linha de código realçada em amarelo para que ele apareça como segue: 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample5.cs?highlight=8)]
9. No code-behind de *conta*/*TwoFactorAuthenticationSignIn.aspx.cs*, atualize o `Page_Load` manipulador, adicionando o seguinte código realçado em amarelo: 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample6.cs?highlight=3-4,13)]

 Tornando o código acima alterar, DropDownList "Provedores" que contém as opções de autenticação não será redefinido para o primeiro valor. Isso permitirá que o usuário selecione com êxito todas as opções para usar quando autenticar, não apenas o primeiro.
10. Em **Solution Explorer**, clique com botão direito *Default.aspx* e selecione **definir como página inicial**.
11. Testando seu aplicativo, primeiro crie o aplicativo (**Ctrl**+**Shift**+**B**) e, em seguida, executar o aplicativo (**F5**) e Selecione **registrar** para criar uma nova conta de usuário ou selecione **login** se a conta de usuário já foi registrada.
12. Depois de você (como o usuário) tiver conectado, clique na ID de usuário (endereço de email) na barra de navegação para exibir o **Gerenciar conta** página (Manage.aspx).  
    ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image2.png)
13. Clique em **adicionar** lado **telefone** no **Gerenciar conta** página.  
    ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image3.png)
14. Adicionar o número de telefone onde você (como o usuário) gostaria de receber mensagens SMS (mensagens de texto) e clique no **enviar** botão.   
    ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image4.png)  
 Neste ponto, o aplicativo usará as credenciais do *Web. config* entrar em contato com o Twilio. Uma mensagem SMS (mensagem de texto) será enviada para o telefone associado à conta de usuário. Você pode verificar se a mensagem de Twilio foi enviada ao exibir o painel do Twilio.
15. Em alguns segundos, o telefone associado à conta de usuário será exibida uma mensagem de texto que contém o código de verificação. Insira o código de verificação e pressione **enviar**.  
     ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image5.png)

<a id="use2FA"></a>
## <a name="enable-two-factor-authentication-for-a-registered-user"></a>Habilitar a autenticação de dois fatores para um usuário registrado

Neste ponto, você tiver habilitado a autenticação de dois fatores para seu aplicativo. Para um usuário usar a autenticação de dois fatores, ele pode simplesmente mudar suas configurações usando a interface do usuário. 

1. Como um usuário do seu aplicativo você pode habilitar autenticação de dois fatores para sua conta específica clicando na ID de usuário (alias de email) na barra de navegação para exibir o **Gerenciar conta** página. Em seguida, clique no **habilitar** link para habilitar a autenticação de dois fatores para a conta.![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image6.png)
2. Fazer logoff e logon novamente. Se você tiver habilitado o email, você pode selecionar o SMS ou email para autenticação de dois fatores. Se você ainda não ativou o email, consulte o tutorial intitulado [criar um aplicativo seguro do ASP.NET Web Forms com o registro de usuário, Email de confirmação e de redefinição de senha](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset.md).![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image7.png)
3. A página de autenticação de dois fatores é exibida, onde você pode inserir o código (de SMS ou email).![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image8.png)  
 Clicar no **lembrar este navegador** caixa de seleção isentará a necessidade de usar a autenticação de dois fatores para fazer logon ao usar o navegador e o dispositivo em que você tiver marcado a caixa. Desde que usuários mal-intencionados não podem acessar seu dispositivo, habilitando a autenticação de dois fatores e clicando no **lembrar este navegador** fornece acesso conveniente de senha um etapa, enquanto ainda retém forte proteção de autenticação de dois fatores para todo o acesso de dispositivos não confiáveis. Você pode fazer isso em qualquer dispositivo privado que você usa regularmente.

<a id="addRes"></a>
## <a name="additional-resources"></a>Recursos adicionais

- [Autenticação de dois fatores usando SMS e email com a identidade do ASP.NET](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md)
- [Links para a identidade do ASP.NET recomendado recursos](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [Implantar um aplicativo de formulários da Web do ASP.NET seguro com associação, OAuth e o banco de dados SQL em um site do Azure](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)
- [Série de tutoriais do Web Forms do ASP.NET - adicionar um provedor do OAuth 2.0](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#OAuthWebForms)
- [Série de tutoriais de Web Forms do ASP.NET - habilitar SSL para o projeto](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms)
- [Confirmação de conta e senha de recuperação com a identidade do ASP.NET](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [Criando o aplicativo no Facebook e conectar o aplicativo ao projeto](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)

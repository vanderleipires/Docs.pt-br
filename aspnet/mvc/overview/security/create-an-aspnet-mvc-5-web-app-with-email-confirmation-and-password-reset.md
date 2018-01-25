---
uid: mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset
title: "Criar um aplicativo web seguro do ASP.NET MVC 5 com logon, email de redefinição de senha e de confirmação (c#) | Microsoft Docs"
author: Rick-Anderson
description: "Este tutorial mostra como criar um aplicativo da web ASP.NET MVC 5 com o email de confirmação e redefinição de senha usando o sistema de associação do ASP.NET Identity. Ca..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/26/2015
ms.topic: article
ms.assetid: d4911cb3-1afb-4805-b860-10818c4b1280
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset
msc.type: authoredcontent
ms.openlocfilehash: 5689031015279484cc616090a767a8c25eefa3c1
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="create-a-secure-aspnet-mvc-5-web-app-with-log-in-email-confirmation-and-password-reset-c"></a>Criar um aplicativo web seguro do ASP.NET MVC 5 com logon, email de redefinição de senha e de confirmação (c#)
====================
Por [Rick Anderson](https://github.com/Rick-Anderson)

> Este tutorial mostra como criar um aplicativo da web ASP.NET MVC 5 com o email de confirmação e redefinição de senha usando o sistema de associação do ASP.NET Identity. Você pode baixar o aplicativo concluído [aqui](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952). O download contém os auxiliares de depuração que permitem que você teste confirmação por email e SMS sem configurar um email ou o provedor de SMS.
> 
> Este tutorial foi escrito por [Rick Anderson](https://blogs.msdn.com/rickAndy) (Twitter: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) ).


<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a>Criar um aplicativo ASP.NET MVC

Comece instalando e executando [Visual Studio Express 2013 para Web](https://go.microsoft.com/fwlink/?LinkId=299058) ou [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Instalar [Visual Studio 2013 atualização 3](https://go.microsoft.com/fwlink/?LinkId=390465) ou superior.

> [!NOTE]
> Aviso: Você deve instalar [Visual Studio 2013 atualização 3](https://go.microsoft.com/fwlink/?LinkId=390465) ou superior para concluir este tutorial.


1. Criar um novo projeto da Web do ASP.NET e selecione o modelo MVC. Formulários da Web também oferece suporte a identidade do ASP.NET, você pode seguir etapas semelhantes em um aplicativo de formulários da web.  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image1.png)
2. Deixe a autenticação padrão como **contas de usuário individuais**. Se você quiser hospedar o aplicativo no Azure, deixe a caixa de seleção marcada. No tutorial posteriormente, implantará no Azure. Você pode [abrir uma conta do Azure gratuitamente](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).
3. Definir o [projeto para usar SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).
4. Executar o aplicativo, clique no **registrar** vincular e registrar um usuário. Neste ponto, a validação somente do email é com o [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) atributo.
5. No Gerenciador de servidores, navegue até **dados Connections\DefaultConnection\Tables\AspNetUsers**, clique com botão direito e selecione **abrir definição de tabela**.

    A imagem a seguir mostra o `AspNetUsers` esquema:

    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image2.png)
6. Clique com o botão direito no **AspNetUsers** de tabela e selecione **Mostrar dados da tabela**.  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image3.png)  
 Agora o email não foi confirmado.
7. Clique na linha e selecione Excluir. Você adicionará o email novamente na próxima etapa e enviar um email de confirmação.

## <a name="email-confirmation"></a>Email de confirmação

É uma prática recomendada para confirmar o email de um novo registro de usuário para verificar se eles não são representando outra pessoa (ou seja, eles ainda não registrados com outra pessoa email). Suponha que você tivesse um fórum de discussão, você deseja evitar `"bob@example.com"` de registrar como `"joe@contoso.com"`. Sem confirmação por email, `"joe@contoso.com"` foi possível obter indesejadas de seu aplicativo. Suponha que Bob acidentalmente registrado como `"bib@example.com"` e ainda não tenha notado, ele não serão capaz de usar a senha de recuperação porque o aplicativo não tiver seu email correto. Email de confirmação oferece apenas proteção limitada de robôs e não oferece proteção contra spam determinado, têm muitos aliases de email de trabalho, que eles podem usar para registrar.

Em geral você deseja impedir que novos usuários lançamento todos os dados para seu site da web antes de confirmar por email, uma mensagem de texto SMS ou outro mecanismo. <a id="build"></a>As seções a seguir, vamos habilitar confirmação por email e modifique o código para impedir que usuários recém-registrados logon até que o email foi confirmado.

<a id="SG"></a>
## <a name="hook-up-sendgrid"></a>Hook up SendGrid

Embora este tutorial mostra apenas como adicionar notificação por email por meio de [SendGrid](http://sendgrid.com/), você pode enviar emails usando SMTP e outros mecanismos (consulte [recursos adicionais](#addRes)).

1. No Console do Gerenciador de pacotes, digite o seguinte comando a seguir: 

    [!code-console[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample1.cmd)]
2. Vá para o [página de inscrição do Azure SendGrid](https://go.microsoft.com/fwlink/?linkid=271033&clcid=0x409) e registre-se gratuitamente conta do SendGrid. Adicione código semelhante à seguinte para configurar o SendGrid:

    [!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample2.cs?highlight=3,5)]

Você precisará adicionar que inclui o seguinte:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample3.cs)]

Para simplificar este exemplo, armazenamos as configurações de aplicativo no *Web. config* arquivo:

[!code-xml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample4.xml)]

> [!WARNING]
> Segurança - nunca armazenar os dados confidenciais em seu código-fonte. A conta e as credenciais são armazenadas na appSetting. No Azure, você pode armazenar com segurança esses valores de  **[configurar](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)**  no portal do Azure. Consulte [práticas recomendadas para a implantação de senhas e outros dados confidenciais em ASP.NET e o Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).


### <a name="enable-email-confirmation-in-the-account-controller"></a>Habilitar o email de confirmação no controlador de conta

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample5.cs?highlight=16-21)]

Verifique se o *Views\Account\ConfirmEmail.cshtml* arquivo tem a sintaxe do razor correto. (O @ caractere na primeira linha pode estar ausente. )

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample6.cshtml?highlight=1)]

Executar o aplicativo e clique no link de registro. Depois que você envia o formulário de registro, você está conectado.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image4.png)

Verifique sua conta de email e clique no link para confirmar seu email.

<a id="require"></a>
## <a name="require-email-confirmation-before-log-in"></a>Solicitar confirmação de email antes de login

Atualmente, depois que o usuário concluir o formulário de registro, elas são registradas. Você geralmente deseja confirmar seu email antes de registrá-los. A seção a seguir, vamos modificará o código para solicitar novos usuários para que um email confirmado antes que eles estejam conectados (autenticados). Atualização de `HttpPost Register` método com as seguintes alterações realçados:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample7.cs?highlight=14-15,23-30)]

Comentando o `SignInAsync` método, o usuário não será assinado pelo registro. O `TempData["ViewBagLink"] = callbackUrl;` linha pode ser usada para [depurar o aplicativo](#dbg) e testar o registro sem enviar email. `ViewBag.Message`é usado para exibir as instruções de confirmação. O [baixar exemplo](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952) contém código para testar a confirmação de email sem configurar o email e também pode ser usado para depurar o aplicativo.

Criar um `Views\Shared\Info.cshtml` de arquivo e adicione a seguinte marcação razor:

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample8.cshtml)]

Adicionar o [autorizar atributo](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.118).aspx) para o `Contact` método de ação do controlador Home. Você pode usar o clique no **contato** link para verificar se os usuários anônimos não têm acesso e os usuários autenticados têm acesso.

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample9.cs?highlight=1)]

Você também deve atualizar o `HttpPost Login` método de ação:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample10.cs?highlight=13-22)]

Atualização de *Views\Shared\Error.cshtml* para exibir a mensagem de erro:

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample11.cshtml?highlight=8-17)]

Excluir todas as contas no **AspNetUsers** tabela que contém o alias de email que você deseja testar. Execute o aplicativo e verifique se que você não pode efetuar logon até que você confirmar seu endereço de email. Depois de confirmar seu endereço de email, clique no **contato** link.

<a id="reset"></a>
## <a name="password-recoveryreset"></a>Redefinição da recuperação de senha

Remova os caracteres de comentário do `HttpPost ForgotPassword` método de ação no controlador de conta:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample12.cs?highlight=17-20)]

Remova os caracteres de comentário do `ForgotPassword` [ActionLink](https://msdn.microsoft.com/library/system.web.mvc.html.linkextensions.actionlink(v=vs.118).aspx) no *Views\Account\Login.cshtml* o arquivo de exibição razor:

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample13.cshtml?highlight=47-50)]

Página de logon agora mostrará um link para redefinir a senha.

<a id="rsend"></a>
## <a name="resend-email-confirmation-link"></a>Reenviar o link de confirmação de email

Depois que um usuário cria uma nova conta local, eles são enviados por email um link de confirmação são necessárias para usar antes que possa fazer logon. Se o usuário acidentalmente exclui o email de confirmação ou email nunca chega, eles precisarão o link de confirmação enviado novamente. As alterações de código a seguir mostram como fazer isso.

Adicione o seguinte método de auxiliar na parte inferior da *Controllers\AccountController.cs* arquivo:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample14.cs)]

O método Register para usar o novo auxiliar de atualização:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample15.cs?highlight=17)]

Atualizar o método de logon para enviar novamente a senha quando se a conta de usuário não foi confirmada:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample16.cs?highlight=20)]

<a id="combine"></a>
## <a name="combine-social-and-local-login-accounts"></a>Combinar as contas de logon local e social

Você pode combinar as contas locais e sociais clicando no link seu email. Na sequência a seguir  **RickAndMSFT@gmail.com**  é criado pela primeira vez como um logon local, mas você pode criar a conta como um log social primeiro e adicionar um logon local.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image5.png)

Clique no **gerenciar** link. Observe externo 0 (logons sociais) associada à conta.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image6.png)

Clique no link para outro log no serviço e aceitar as solicitações do aplicativo. As duas contas foram combinadas, você poderá fazer logon com a conta. Convém que os usuários adicionem contas locais no caso de seu log social no serviço de autenticação está inoperante ou mais provável que perdeu o acesso à sua conta social.

Na imagem a seguir, Tom é um logon social (que você pode ver o **logons externos: 1** mostradas na página).

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image7.png)

Clicando em **escolher uma senha** permite que você adicione um log local em associados com a mesma conta.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image8.png)

## <a name="email-confirmation-in-more-depth"></a>Confirmação de email com mais profundidade

O tutorial [confirmação de conta e senha de recuperação com a identidade do ASP.NET](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) entra neste tópico com mais detalhes.

<a id="dbg"></a>
## <a name="debugging-the-app"></a>Depurar o aplicativo

Se você não receber um email contendo o link:

- Verifique sua pasta Lixo eletrônico ou spam.
- Faça logon em sua conta do SendGrid e clique no [link da atividade de Email](https://sendgrid.com/logs/index).

Para testar o link de verificação sem email, baixe o [exemplo concluído](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952). O link de confirmação e códigos de confirmação serão exibidos na página.

<a id="addRes"></a>
## <a name="additional-resources"></a>Recursos adicionais

- [Links para a identidade do ASP.NET recomendado recursos](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [Conta de confirmação e de recuperação de senha com a identidade do ASP.NET](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) apresenta mais detalhes na confirmação da senha da conta e a recuperação.
- [Aplicativo do MVC 5 com o Facebook, Twitter, LinkedIn e logon do Google OAuth2](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) este tutorial mostra como escrever um aplicativo ASP.NET MVC 5 com autorização Facebook e Google OAuth 2. Ele também mostra como adicionar dados adicionais para o banco de dados de identidade.
- [Implantar um aplicativo ASP.NET MVC seguro com associação, OAuth e o banco de dados SQL do Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Este tutorial adiciona a implantação do Azure, como proteger seu aplicativo com as funções, como usar a API de associação para adicionar usuários e funções e recursos de segurança adicionais.
- [Criar um aplicativo do Google OAuth 2 e conectar o aplicativo ao projeto](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [Criando o aplicativo no Facebook e conectar o aplicativo ao projeto](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [Configurar o SSL no projeto](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)

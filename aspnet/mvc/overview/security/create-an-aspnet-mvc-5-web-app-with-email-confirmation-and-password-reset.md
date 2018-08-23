---
uid: mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset
title: Criar um aplicativo de web do ASP.NET MVC 5 seguro com logon, o email de confirmação e redefinição de senha (c#) | Microsoft Docs
author: Rick-Anderson
description: Este tutorial mostra como criar um aplicativo de web do ASP.NET MVC 5 com confirmação por email e redefinição de senha usando o sistema de associação do ASP.NET Identity. Ca...
ms.author: riande
ms.date: 03/26/2015
ms.assetid: d4911cb3-1afb-4805-b860-10818c4b1280
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset
msc.type: authoredcontent
ms.openlocfilehash: e15595cab2d1f51374d4577a67f0f190a531acb5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834493"
---
<a name="create-a-secure-aspnet-mvc-5-web-app-with-log-in-email-confirmation-and-password-reset-c"></a>Criar um aplicativo de web do ASP.NET MVC 5 seguro com logon, o email de confirmação e redefinição de senha (c#)
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

> Este tutorial mostra como criar um aplicativo de web do ASP.NET MVC 5 com confirmação por email e redefinição de senha usando o sistema de associação do ASP.NET Identity. Você pode baixar o aplicativo concluído [aqui](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952). O download contém os auxiliares de depuração que permitem que você teste confirmação por email e SMS sem configurar um email ou um provedor de SMS.
> 
> Este tutorial foi escrito por [Rick Anderson](https://blogs.msdn.com/rickAndy) (Twitter: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) ).


<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a>Criar um aplicativo ASP.NET MVC

Comece instalando e executando [Visual Studio Express 2013 para Web](https://go.microsoft.com/fwlink/?LinkId=299058) ou [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Instale [Visual Studio 2013 atualização 3](https://go.microsoft.com/fwlink/?LinkId=390465) ou superior.

> [!NOTE]
> Aviso: Você deve instalar [Visual Studio 2013 atualização 3](https://go.microsoft.com/fwlink/?LinkId=390465) ou superior para concluir este tutorial.


1. Crie um novo projeto da Web do ASP.NET e selecione o modelo MVC. Web Forms também oferece suporte a identidade do ASP.NET, você pode seguir etapas semelhantes em um aplicativo de formulários da web.  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image1.png)
2. Deixe a autenticação padrão como **contas de usuário individuais**. Se você quiser hospedar o aplicativo no Azure, deixe a caixa de seleção marcada. Posteriormente no tutorial, implantaremos para o Azure. Você pode [abrir uma conta do Azure gratuitamente](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).
3. Defina as [projeto para usar SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).
4. Execute o aplicativo, clique no **registrar** vincular e registrar um usuário. Neste ponto, a validação apenas no email é com o [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) atributo.
5. No Server Explorer, navegue até **dados Connections\DefaultConnection\Tables\AspNetUsers**, clique com o botão direito e selecione **abrir definição de tabela**.

    A imagem a seguir mostra o `AspNetUsers` esquema:

    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image2.png)
6. Clique com botão direito do **AspNetUsers** de tabela e selecione **Mostrar dados da tabela**.  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image3.png)  
 Neste ponto o email não foi confirmado.
7. Clique na linha e selecione Excluir. Você adicionará este email novamente na próxima etapa e enviar um email de confirmação.

## <a name="email-confirmation"></a>Email de confirmação

Ele é uma prática recomendada para confirmar o email de um novo registro de usuário para verificar se eles não são representando a outra pessoa (ou seja, eles ainda não registrados com o email de outra pessoa). Suponha que você tivesse um fórum de discussão, convém evitar `"bob@example.com"` de registrar como `"joe@contoso.com"`. Sem email de confirmação, `"joe@contoso.com"` poderia obter email indesejado do seu aplicativo. Suponha que Bob acidentalmente registrado como `"bib@example.com"` e ainda não tenha notado, ele não seria capaz de usar a senha de recuperação porque o aplicativo não tiver seu email correto. Email de confirmação fornece apenas proteção limitada de bots e não fornece proteção de determinado remetentes de spam, eles têm muitos aliases de email de trabalho, que eles podem usar para se registrar.

Você geralmente deseja impedir que usuários novos lançamentos de todos os dados para seu site da web antes de confirmar por email, uma mensagem de texto SMS ou outro mecanismo. <a id="build"></a>Nas seções a seguir, podemos habilitará o email de confirmação e modificar o código para impedir que usuários recém-registrados entrar até que o email foi confirmado.

<a id="SG"></a>
## <a name="hook-up-sendgrid"></a>Enganchar SendGrid

Embora este tutorial apenas mostra como adicionar notificação por email por meio [SendGrid](http://sendgrid.com/), você pode enviar emails usando SMTP e outros mecanismos (consulte [recursos adicionais](#addRes)).

1. No Console do Gerenciador de pacotes, digite o seguinte comando: 

    [!code-console[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample1.cmd)]
2. Vá para o [página de inscrição do Azure SendGrid](https://go.microsoft.com/fwlink/?linkid=271033&clcid=0x409) e registre-se para uma conta gratuita do SendGrid. Configure o SendGrid, adicionando código semelhante ao seguinte no *App_Start/IdentityConfig.cs*:

    [!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample2.cs?highlight=3,5)]

Você precisará adicionar o que inclui o seguinte:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample3.cs)]

Para simplificar este exemplo, vamos armazenar as configurações do aplicativo na *Web. config* arquivo:

[!code-xml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample4.xml)]

> [!WARNING]
> Security - nunca armazenar os dados confidenciais em seu código-fonte. A conta e as credenciais são armazenadas na appSetting. No Azure, você pode armazenar com segurança esses valores sobre o **[configurar](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** guia no portal do Azure. Ver [práticas recomendadas para implantar senhas e outros dados confidenciais no ASP.NET e o Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).


### <a name="enable-email-confirmation-in-the-account-controller"></a>Habilitar o email de confirmação no controlador da conta

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample5.cs?highlight=16-21)]

Verifique se o *Views\Account\ConfirmEmail.cshtml* arquivo tem a sintaxe do razor correto. (O caractere no primeiro @ linha pode estar ausente. )

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample6.cshtml?highlight=1)]

Execute o aplicativo e clique no link de registro. Depois de enviar o formulário de registro, você está conectado.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image4.png)

Verifique sua conta de email e clique no link para confirmar seu email.

<a id="require"></a>
## <a name="require-email-confirmation-before-log-in"></a>Exigir email de confirmação antes de entrar

No momento, depois que o usuário concluir o formulário de registro, elas são registradas no. Você geralmente deseja confirmar seu email antes do registro em log. A seção a seguir, modificaremos o código para exigir que novos usuários ter um email confirmado antes que eles estejam conectados (autenticado). Atualização de `HttpPost Register` método com as seguintes alterações realçadas:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample7.cs?highlight=14-15,23-30)]

Comentando o `SignInAsync` método, o usuário não será conectado pelo registro. O `TempData["ViewBagLink"] = callbackUrl;` linha pode ser usada para [depurar o aplicativo](#dbg) e testar o registro sem enviar email. `ViewBag.Message` é usado para exibir as instruções de confirmar. O [baixar exemplo](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952) contém código para testar o email de confirmação sem configurar o email e também pode ser usado para depurar o aplicativo.

Criar um `Views\Shared\Info.cshtml` arquivo e adicione a seguinte marcação razor:

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample8.cshtml)]

Adicione a [atributo Authorize](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.118).aspx) para o `Contact` método de ação do controlador Home. Você pode clicar na **entre em contato com** link para verificar se os usuários anônimos não têm acesso e os usuários autenticados têm acesso.

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample9.cs?highlight=1)]

Você também deve atualizar o `HttpPost Login` método de ação:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample10.cs?highlight=13-22)]

Atualizar o *Views\Shared\Error.cshtml* exibição para exibir a mensagem de erro:

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample11.cshtml?highlight=8-17)]

Excluir qualquer conta na **AspNetUsers** tabela que contém o alias de email que você deseja testar com. Execute o aplicativo e verifique se que você não pode fazer logon até que você confirmar seu endereço de email. Depois de confirmar seu endereço de email, clique no **entre em contato com** link.

<a id="reset"></a>
## <a name="password-recoveryreset"></a>Recuperação/redefinição de senha

Remova os caracteres de comentário do `HttpPost ForgotPassword` método de ação no controlador da conta:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample12.cs?highlight=17-20)]

Remova os caracteres de comentário do `ForgotPassword` [ActionLink](https://msdn.microsoft.com/library/system.web.mvc.html.linkextensions.actionlink(v=vs.118).aspx) no *Views\Account\Login.cshtml* arquivo de exibição do razor:

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample13.cshtml?highlight=47-50)]

A página de logon agora mostrará um link para redefinir a senha.

<a id="rsend"></a>
## <a name="resend-email-confirmation-link"></a>Reenviar link de confirmação de email

Depois que um usuário cria uma nova conta local, eles são enviados por email um link de confirmação que são obrigados a usar antes que eles podem fazer logon. Se o usuário acidentalmente exclui o email de confirmação, ou o email chega nunca, será necessário o link de confirmação enviado novamente. As alterações de código a seguir mostram como habilitar isso.

Adicione o seguinte método auxiliar na parte inferior do *Controllers\AccountController.cs* arquivo:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample14.cs)]

O método Register para usar o novo auxiliar de atualização:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample15.cs?highlight=17)]

Atualize o método de logon para reenviar a senha se a conta de usuário não foi confirmada:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample16.cs?highlight=20)]

<a id="combine"></a>
## <a name="combine-social-and-local-login-accounts"></a>Combine as contas de logon social e local

Você pode combinar as contas locais e sociais clicando no link seu email. Na sequência a seguir **RickAndMSFT@gmail.com** é criado como um logon local, mas você pode criar a conta como um logon social no primeiro e adicionar um logon local.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image5.png)

Clique no **gerenciar** link. Observe a **Logins externos: 0** associada com esta conta.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image6.png)

Clique no link para outro log no serviço e aceitar as solicitações do aplicativo. As duas contas foram combinadas, você poderá fazer logon com qualquer uma das contas. Convém que os usuários adicionem contas locais no caso de seu logon social no serviço de autenticação está inoperante ou, mais provavelmente eles tem perdido o acesso à sua conta social.

Na imagem a seguir, Tom é um logon social (que você pode ver o **logons externos: 1** mostradas na página).

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image7.png)

Clicar em **escolha uma senha** permite que você adicione um log local em associado com a mesma conta.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image8.png)

## <a name="email-confirmation-in-more-depth"></a>Email de confirmação com mais profundidade

Tutorial de minha [confirmação de conta e recuperação de senha com o ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) entra neste tópico com mais detalhes.

<a id="dbg"></a>
## <a name="debugging-the-app"></a>Depuração do aplicativo

Se você não receber um email contendo o link:

- Verifique a pasta Lixo eletrônico ou spam.
- Faça logon em sua conta do SendGrid e clique no [link da atividade de Email](https://sendgrid.com/logs/index).

Para testar o link de verificação sem email, baixe o [amostra concluída](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952). O link de confirmação e códigos de confirmação serão exibidos na página.

<a id="addRes"></a>
## <a name="additional-resources"></a>Recursos adicionais

- [Recursos recomendados de links para a identidade do ASP.NET](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [Confirmação de conta e recuperação de senha com o ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) apresenta mais detalhes na confirmação de conta e recuperação de senha.
- [Aplicativo do MVC 5 com o Facebook, Twitter, LinkedIn e Google OAuth2 Sign-on](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) este tutorial mostra como escrever um aplicativo ASP.NET MVC 5 com o Facebook e Google OAuth 2 autorização. Ele também mostra como adicionar dados adicionais para o banco de dados de identidade.
- [Implantar um aplicativo ASP.NET MVC seguro com associação, OAuth e banco de dados SQL no Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Este tutorial adiciona a implantação do Azure, como proteger seu aplicativo com as funções, como usar a API de associação para adicionar usuários e funções e recursos de segurança adicionais.
- [Criando um aplicativo do Google para o OAuth 2 e conectar o aplicativo ao projeto](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [Criando o aplicativo no Facebook e conectar o aplicativo ao projeto](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [Configurar o SSL no projeto](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)

---
uid: identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity
title: Conta de confirmação e de recuperação de senha com a identidade do ASP.NET (c#) | Microsoft Docs
author: HaoK
description: Antes de fazer este tutorial, a que você deve primeiro concluir criar um aplicativo de web seguro do ASP.NET MVC 5 com logon, redefinição de senha e de confirmação de email. Este tutorial...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/26/2015
ms.topic: article
ms.assetid: 8d54180d-f826-4df7-b503-7debf5ed9fb3
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 0167388cf6b488b72ca36f583a7794690dbf9900
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="account-confirmation-and-password-recovery-with-aspnet-identity-c"></a>Confirmação de conta e senha de recuperação com a identidade do ASP.NET (c#)
====================
por [Kung Hao](https://github.com/HaoK), [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://github.com/Rick-Anderson), [Suhas Joshi](https://github.com/suhasj)

> Antes de fazer este tutorial você deve concluir [criar um aplicativo web seguro do ASP.NET MVC 5 com logon, redefinição de senha e de confirmação de email](../../../mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md). Este tutorial contém mais detalhes e mostrará como configurar o email de confirmação de conta local e permitir que os usuários redefinir sua senha esquecida na identidade do ASP.NET. Este artigo foi escrito de Rick Anderson ([@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)), Pranav Rastogi ([@rustd](https://twitter.com/rustd)), Hao Kung e Suhas Joshi. O exemplo do NuGet foi escrito principalmente por Hao Kung.


Uma conta de usuário local requer que o usuário criar uma senha para a conta e senha é armazenada (com segurança) no aplicativo web. Identidade do ASP.NET também oferece suporte a contas sociais, que não exigem que o usuário criar uma senha para o aplicativo. [Contas sociais](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) usam um terceiro (como Google, Twitter, Facebook ou Microsoft) para autenticar usuários. Este tópico aborda os seguintes tópicos:

- [Criar um aplicativo ASP.NET MVC](#createMvc) e explorar os recursos de identidade do ASP.NET.
- [Compilando o exemplo de identidade](#build)
- [Configurar o email de confirmação](#email)

Novos usuários registrar seus alias de email, que cria uma conta local.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image1.png)

Clique no botão registrar envia um email de confirmação que contém um token de validação para o seu endereço de email.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image2.png)

O usuário é enviado um email com um token de confirmação para sua conta.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image3.png)

Clique no link confirma a conta.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image4.png)

<a id="passwordReset"></a>

## <a name="password-recoveryreset"></a>Redefinição da recuperação de senha

Usuários locais que esqueceram suas senhas podem ter um token de segurança enviado à sua conta de email, possibilitando a redefinir sua senha.  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image5.png)  
  
Assim, o usuário receberá um email com um link que permite que eles redefinir sua senha.  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image6.png)  
Clique no link vai levá-los para a página de redefinição.  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image7.png)  
  
Clique o **redefinir** botão Confirmar a senha foi redefinida.  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image8.png)

<a id="createMvc"></a>

## <a name="create-an-aspnet-web-app"></a>Criar um aplicativo da Web do ASP.NET

Comece instalando e executando [Visual Studio Express 2013 para Web](https://go.microsoft.com/fwlink/?LinkId=299058) ou [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Instalar o Visual Studio [2013 atualização 2](https://go.microsoft.com/fwlink/?LinkId=390521) ou superior.

> [!NOTE]
> Aviso: Você deve instalar o Visual Studio [2013 atualização 2](https://go.microsoft.com/fwlink/?LinkId=390521) para concluir este tutorial.


1. Criar um novo projeto da Web do ASP.NET e selecione o modelo MVC. Formulários da Web também oferece suporte a identidade do ASP.NET, você pode seguir etapas semelhantes em um aplicativo de formulários da web.
2. Deixe a autenticação padrão como **contas de usuário individuais**.
3. Executar o aplicativo, clique no **registrar** vincular e registrar um usuário. Neste ponto, a validação somente do email é com o [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) atributo.
4. No Gerenciador de servidores, navegue até **dados Connections\DefaultConnection\Tables\AspNetUsers**, clique com botão direito e selecione **abrir definição de tabela**.

    A imagem a seguir mostra o `AspNetUsers` esquema:

    ![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image9.png)
5. Clique com o botão direito no **AspNetUsers** de tabela e selecione **Mostrar dados da tabela**.  
  
    ![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image10.png)  
  
   Agora o email não foi confirmado.

O repositório de dados padrão para a identidade do ASP.NET é Entity Framework, mas você pode configurá-lo para usar outros repositórios de dados e adicionar outros campos. Consulte [recursos adicionais](#addRes) seção no final deste tutorial.

O [classe de inicialização OWIN](../../../aspnet/overview/owin-and-katana/owin-startup-class-detection.md) ( *Startup.cs* ) é chamado quando o aplicativo é iniciado e invoca o `ConfigureAuth` método *aplicativo\_Start\Startup.Auth.cs*, que configura o pipeline OWIN e inicializa a identidade do ASP.NET. Examine o `ConfigureAuth` método. Cada `CreatePerOwinContext` chamada registra um retorno de chamada (salvo no `OwinContext`) que será chamado uma vez por solicitação para criar uma instância do tipo especificado. Você pode definir um ponto de interrupção no construtor e `Create` método de cada tipo (`ApplicationDbContext, ApplicationUserManager`) e verifique se eles são chamados em cada solicitação. Uma instância de `ApplicationDbContext` e `ApplicationUserManager` é armazenada no contexto OWIN, que pode ser acessado em todo o aplicativo. Ganchos de identidade do ASP.NET no pipeline OWIN por meio do middleware do cookie. Para obter mais informações, consulte [por gerenciamento de vida útil de solicitação para a classe UserManager no ASP.NET Identity](https://blogs.msdn.com/b/webdev/archive/2014/02/12/per-request-lifetime-management-for-usermanager-class-in-asp-net-identity.aspx).

Quando você altera seu perfil de segurança, um novo carimbo de segurança é gerado e armazenado na `SecurityStamp` campo o *AspNetUsers* tabela. Observe que o `SecurityStamp` campo é diferente do cookie de segurança. O cookie de segurança não é armazenado no `AspNetUsers` tabela (ou em qualquer lugar no banco de dados de identidade). O token de cookie de segurança é autoassinado usando [DPAPI](https://msdn.microsoft.com/library/system.security.cryptography.protecteddata.aspx) e é criado com o `UserId, SecurityStamp` e informações de tempo de expiração.

O middleware de cookie verifica o cookie em cada solicitação. O `SecurityStampValidator` método o `Startup` classe atinge o banco de dados e verifica periodicamente, o carimbo de segurança conforme especificado com o `validateInterval`. Isso acontece apenas a cada 30 minutos (em nosso exemplo), a menos que você altere seu perfil de segurança. O intervalo de 30 minutos foi escolhido para minimizar as viagens ao banco de dados. Consulte meu [tutorial de autenticação de dois fatores](index.md) para obter mais detalhes.

Por que os comentários no código, o `UseCookieAuthentication` método dá suporte à autenticação de cookie. O `SecurityStamp` associado e campo de código fornece uma camada extra de segurança ao seu aplicativo, quando você alterar sua senha, será feito fora do navegador que você fez logon. O `SecurityStampValidator.OnValidateIdentity` método permite que o aplicativo validar o token de segurança quando o usuário fizer logon, que é usado quando você alterar uma senha ou usar o logon externo. Isso é necessário para garantir que todos os tokens (cookies) gerados com a senha antiga são invalidados. No projeto de exemplo, se você alterar a senha de usuários, em seguida, um novo token é gerada para o usuário, todos os tokens anteriores são invalidados e `SecurityStamp` campo é atualizado.

O sistema de identidade permitem que você configure seu aplicativo isso quando altera o perfil de segurança de usuários (por exemplo, quando o usuário altera sua senha ou as alterações associadas logon (por exemplo, Facebook, Google, conta da Microsoft, etc.), o usuário está conectado a todos instâncias do navegador. Por exemplo, a imagem abaixo mostra o [exemplo de saída única](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SingleSignOutSample/readme.txt) aplicativo, que permite que o usuário sair de todas as instâncias de navegador (nesse caso, Internet Explorer, Firefox e o Chrome) clicando em um botão. Como alternativa, o exemplo permite que você sair apenas uma instância de navegador específico.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image11.png)

O [exemplo de saída única](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SingleSignOutSample/readme.txt) aplicativo mostra como a identidade do ASP.NET permite que você gerar o token de segurança. Isso é necessário para garantir que todos os tokens (cookies) gerados com a senha antiga são invalidados. Este recurso fornece uma camada extra de segurança para seu aplicativo. Quando você alterar sua senha, desconectado onde você fez no aplicativo.

O *aplicativo\_Start\IdentityConfig.cs* arquivo contém o `ApplicationUserManager`, `EmailService` e `SmsService` classes. O `EmailService` e `SmsService` classes cada implementam o `IIdentityMessageService` interface, para que você tenha métodos comuns de cada classe para configurar o email e SMS. Embora este tutorial mostra apenas como adicionar notificação por email por meio de [SendGrid](http://sendgrid.com/), você pode enviar emails usando SMTP e outros mecanismos.

O `Startup` classe também contém placa clichê para adicionar logons sociais (Facebook, Twitter, etc.), consulte o tutorial [aplicativo do MVC 5 com o Facebook, Twitter, LinkedIn e logon do Google OAuth2](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) para obter mais informações.

Examine o `ApplicationUserManager` classe, que contém as informações de identidade de usuários e configura os seguintes recursos:

- Requisitos de força da senha.
- Bloqueio de usuário (tentativas e tempo).
- Autenticação de dois fatores (2FA). Abordaremos 2FA e SMS em outro tutorial.
- Conectar o email e serviços do SMS. (Abordaremos SMS em outro tutorial).

O `ApplicationUserManager` classe deriva genérica `UserManager<ApplicationUser>` classe. `ApplicationUser` deriva [IdentityUser](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework.identityuser.aspx). `IdentityUser` deriva genérica `IdentityUser` classe:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample1.cs)]

As propriedades acima coincidirem com as propriedades de `AspNetUsers` tabela, como mostrada acima.

Argumentos genéricos em `IUser` permitem que você derivar uma classe usando tipos diferentes para a chave primária. Consulte o [ChangePK](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/ChangePK/readme.txt) exemplo que mostra como alterar a chave primária de cadeia de caracteres para int ou GUID.

### <a name="applicationuser"></a>ApplicationUser

`ApplicationUser` (`public class ApplicationUserManager : UserManager<ApplicationUser>`) é definido em *Models\IdentityModels.cs* como:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample2.cs?highlight=8-9)]

O código realçado acima gera um [ClaimsIdentity](https://msdn.microsoft.com/library/system.security.claims.claimsidentity.aspx). Identidade do ASP.NET e autenticação de Cookie OWIN são baseados em declarações, portanto o framework requer o aplicativo para gerar um `ClaimsIdentity` para o usuário. `ClaimsIdentity` contém informações sobre todas as declarações para o usuário, como o nome do usuário, idade e as funções que o usuário pertence. Você também pode adicionar mais declarações para o usuário neste estágio.

O OWIN `AuthenticationManager.SignIn` método passa a `ClaimsIdentity` e assina o usuário:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample3.cs?highlight=4-6)]

[Aplicativo do MVC 5 com o Facebook, Twitter, LinkedIn e logon do Google OAuth2](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) mostra como você pode adicionar propriedades adicionais para o `ApplicationUser` classe.

## <a name="email-confirmation"></a>Email de confirmação

É uma boa ideia para confirmar o email de um novo usuário se registrar para verificar se eles não são representando outra pessoa (ou seja, eles ainda não registrados com outra pessoa email). Suponha que você tivesse um fórum de discussão, você deseja evitar `"bob@example.com"` de registrar como `"joe@contoso.com"`. Sem confirmação por email, `"joe@contoso.com"` foi possível obter indesejadas de seu aplicativo. Suponha que Bob acidentalmente registrado como `"bib@example.com"` e ainda não tenha notado, ele não serão capaz de usar a senha de recuperação porque o aplicativo não tiver seu email correto. Email de confirmação oferece apenas proteção limitada de robôs e não oferece proteção contra spam determinado, têm muitos aliases de email de trabalho, que eles podem usar para registrar. No exemplo abaixo, o usuário não poderá alterar sua senha até que sua conta foi confirmada (por eles clicando em um link de confirmação recebida na conta de email, eles registrados com.) Você pode aplicar o fluxo de trabalho a outros cenários, por exemplo, enviar um link para confirmar e redefinir a senha em novas contas criadas pelo administrador, enviar um email de usuário quando ela alterou seu perfil e assim por diante. Em geral você deseja impedir que novos usuários lançamento todos os dados para seu site da web antes de confirmar por email, uma mensagem de texto SMS ou outro mecanismo. <a id="build"></a>

## <a name="building-a-more-complete-sample"></a>Criando um exemplo mais completo

Nesta seção, você usará o NuGet para baixar um exemplo mais completo, com que trabalharemos.

1. Criar um novo ***vazio*** projeto da Web do ASP.NET.
2. No Console do Gerenciador de pacotes, digite os seguintes comandos a seguir: 

    [!code-console[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample4.cmd)]

   Neste tutorial, vamos usar [SendGrid](http://sendgrid.com/) para enviar email. O `Identity.Samples` pacote instala o código que trabalhará.
3. Definir o [projeto para usar SSL](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).
4. Criação de conta local de teste executando o aplicativo, clicando no **registrar** link e o formulário de registro de lançamento.
5. Clique no link de email de demonstração, que simula a confirmação de email.
6. Remover o código de confirmação de link de email de demonstração do exemplo (o `ViewBag.Link` código no controlador de conta. Consulte o `DisplayEmail` e `ForgotPasswordConfirmation` métodos de ação e modos de exibição do razor).

> [!NOTE]
> Aviso: Se você alterar as configurações de segurança neste exemplo, aplicativos de produção serão necessário passar por uma auditoria de segurança que explicitamente chama as alterações feitas.


## <a name="examine-the-code-in-appstartidentityconfigcs"></a>Examine o código no aplicativo\_Start\IdentityConfig.cs

O exemplo mostra como criar uma conta e adicioná-lo para o *Admin* função. Você deve substituir o email no exemplo de email que você usará para a conta de administrador. A maneira mais fácil no momento para criar uma conta de administrador é programaticamente o `Seed` método. Esperamos que tenha uma ferramenta no futuro que permitirá que você criar e administrar usuários e funções. O código de exemplo permitem criar e gerenciar usuários e funções, mas primeiro você deve ter uma conta de administradores para executar as funções e as páginas de administração de usuário. Neste exemplo, a conta de administrador é criada quando o banco de dados é propagado.

Alterar a senha e altere o nome para uma conta em que você pode receber notificações por email.

> [!WARNING]
> Segurança - nunca armazenar os dados confidenciais em seu código-fonte.

Conforme mencionado anteriormente, o `app.CreatePerOwinContext` chamada na classe de inicialização adiciona retornos de chamada para o `Create` método os banco de dados de aplicativo de conteúdo, manager e a função Gerenciador de classes de usuário. Chamadas de pipeline OWIN a `Create` método nessas classes para cada solicitação e armazena o contexto para cada classe. O controlador de conta expõe o gerente do usuário do contexto HTTP (que contém o contexto do OWIN):

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample5.cs)]

Quando um usuário registra uma conta local, o `HTTP Post Register` método é chamado:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample6.cs)]

O código acima usa os dados de modelo para criar uma nova conta de usuário usando o email e a senha digitada. Se o alias de email está no repositório de dados, falha de criação da conta e o formulário é exibido novamente. O `GenerateEmailConfirmationTokenAsync` método cria um token de confirmação segura e armazena-o no repositório de dados de identidade do ASP.NET. O [URL](https://msdn.microsoft.com/library/dd505232(v=vs.118).aspx) método cria um link que contém o `UserId` e token de confirmação. Esse link, em seguida, é enviado por email para o usuário, o usuário pode clicar no link em seu aplicativo de email para confirmar sua conta.

<a id="email"></a>

## <a name="set-up-email-confirmation"></a>Configurar o email de confirmação

Vá para o [página de inscrição do Azure SendGrid](https://azure.microsoft.com/gallery/store/sendgrid/sendgrid-azure/) e registre-se gratuitamente conta. Adicione código semelhante à seguinte para configurar o SendGrid:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample7.cs?highlight=5)]

> [!NOTE]
> Clientes de email com frequência aceitam somente mensagens de texto (sem HTML). Você deve fornecer a mensagem de texto e HTML. No exemplo do SendGrid acima, isso é feito com o `myMessage.Text` e `myMessage.Html` código mostrado acima.


O código a seguir mostra como enviar email usando o [MailMessage](https://msdn.microsoft.com/library/system.net.mail.mailmessage.aspx) classe onde `message.Body` retorna somente o link.

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample8.cs)]

> [!WARNING]
> Segurança - nunca armazenar os dados confidenciais em seu código-fonte. A conta e as credenciais são armazenadas na appSetting. No Azure, você pode armazenar com segurança esses valores de **[configurar](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** no portal do Azure. Consulte [práticas recomendadas para a implantação de senhas e outros dados confidenciais em ASP.NET e o Azure](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).


Insira suas credenciais SendGrid, executar o aplicativo, registrar com um alias de email pode clicar no link confirmar seu email. Para saber como fazer isso com seu [Outlook.com](http://outlook.com) contas de email, consulte de John Atten [c# configuração de SMTP para o Host de SMTP do Outlook.Com](http://typecastexception.com/post/2013/12/20/C-SMTP-Configuration-for-OutlookCom-SMTP-Host.aspx) e suas[ASP.NET 2.0 de identidade: configuração de backup de validação da conta Autorização de dois fatores e](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) postagens.

Depois que um usuário clica o **registrar** botão um email de confirmação que contém um token de validação é enviado para seu endereço de email.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image12.png)

O usuário é enviado um email com um token de confirmação para sua conta.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image13.png)

## <a name="examine-the-code"></a>Examine o código

O código a seguir mostra o método `POST ForgotPassword`.

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample9.cs)]

O método silenciosamente falhará se o email do usuário não foi confirmado. Se um erro foi lançado para um endereço de email inválido, usuários mal-intencionados podem usar essas informações para localizar a userId válido (alias de email) a ataques.

O código a seguir mostra o `ConfirmEmail` método no controlador de conta que é chamado quando o usuário clica no link de confirmação de email enviado a eles:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample10.cs)]

Depois que um token de senha tiver sido usado, são invalidados. A seguinte alteração de código no `Create` método (no *aplicativo\_Start\IdentityConfig.cs* arquivo) define os tokens para expirar em 3 horas.

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample11.cs?highlight=6-8)]

Com o código acima, a senha e os tokens de confirmação de email expirará em 3 horas. O padrão `TokenLifespan` é um dia.

O código a seguir mostra o método de confirmação de email:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample12.cs)]

 Para tornar seu aplicativo mais seguro, a identidade do ASP.NET oferece suporte a autenticação de dois fatores (2FA). Consulte [identidade do ASP.NET 2.0: Configurando a validação da conta e a autorização de dois fatores](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) por John Atten. Embora você possa definir o bloqueio de conta em falhas de tentativa de senha de logon, essa abordagem torna seu logon suscetíveis a [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) bloqueios. Recomendamos que você use o bloqueio de conta apenas com 2FA.  
<a id="addRes"></a>

## <a name="additional-resources"></a>Recursos adicionais

- [Visão geral de provedores de armazenamento personalizados para a Identidade do ASP.NET](../extensibility/overview-of-custom-storage-providers-for-aspnet-identity.md)
- [Aplicativo do MVC 5 com o Facebook, Twitter, LinkedIn e logon do Google OAuth2](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) também mostra como adicionar informações de perfil para a tabela de usuários.
- [ASP.NET MVC e identidade 2.0: Noções básicas](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx) por John Atten.
- [Introdução à Identidade do ASP.NET](../getting-started/introduction-to-aspnet-identity.md)
- [Anunciando RTM do ASP.NET Identity 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx) por Pranav Rastogi.

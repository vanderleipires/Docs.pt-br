---
uid: identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity
title: Autenticação de dois fatores usando SMS e email com o ASP.NET Identity | Microsoft Docs
author: HaoK
description: Este tutorial mostra como configurar a autenticação de dois fatores (2FA) usando o SMS e email. Este artigo foi escrito por Rick Anderson ( @RickAndMSFT ), por...
ms.author: riande
ms.date: 09/15/2015
ms.assetid: 053e23c4-13c9-40fa-87cb-3e9b0823b31e
msc.legacyurl: /identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 4b253923696e35e59c196578a232f53c11671d16
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577971"
---
<a name="two-factor-authentication-using-sms-and-email-with-aspnet-identity"></a>Autenticação de dois fatores usando SMS e email com o ASP.NET Identity
====================
por [Kung Hao](https://github.com/HaoK), [Pranav Rastogi](https://github.com/rustd), [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Suhas Joshi](https://github.com/suhasj)

> Este tutorial mostra como configurar a autenticação de dois fatores (2FA) usando o SMS e email.
> 
> Este artigo foi escrito por Rick Anderson ([@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)), Pranav Rastogi ([@rustd](https://twitter.com/rustd)), Hao Kung e Suhas Joshi. O exemplo do NuGet foi escrito principalmente por Hao Kung.


Este tópico aborda o seguinte:

- [Compilando o exemplo de identidade](#build)
- [Configurar o SMS para autenticação de dois fatores](#SMS)
- [Habilitar a autenticação de dois fatores](#enable2)
- [Como registrar um provedor de autenticação de dois fatores](#reg)
- [Combine as contas de logon social e local](#combine)
- [Bloqueio de conta contra ataques de força bruta](#lock)

<a id="build"></a>

## <a name="building-the-identity-sample"></a>Compilando o exemplo de identidade

Nesta seção, você usará o NuGet para baixar um exemplo, com que trabalharemos. Comece instalando e executando [Visual Studio Express 2013 para Web](https://go.microsoft.com/fwlink/?LinkId=299058) ou [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Instalar o Visual Studio [2013 atualização 2](https://go.microsoft.com/fwlink/?LinkId=390521) ou superior.

> [!NOTE]
> Aviso: Você deve instalar o Visual Studio [2013 atualização 2](https://go.microsoft.com/fwlink/?LinkId=390521) para concluir este tutorial.


1. Criar um novo ***vazio*** projeto Web do ASP.NET.
2. No Console do Gerenciador de pacotes, digite o seguinte os comandos a seguir:  
  
    `Install-Package SendGrid`  
    `Install-Package -Prerelease Microsoft.AspNet.Identity.Samples`  
  
   Neste tutorial, usaremos [SendGrid](http://sendgrid.com/) para enviar email e [Twilio](https://www.twilio.com/) ou [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) para mandar um texto sms. O `Identity.Samples` pacote instala o código que podemos trabalhará.
3. Defina as [projeto para usar SSL](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).
4. *Opcional*: siga as instruções em minha [tutorial de confirmação de Email](account-confirmation-and-password-recovery-with-aspnet-identity.md) interligar SendGrid e, em seguida, execute o aplicativo e registrar uma conta de email.
5. * Opcional: * Remova o código de confirmação de link de email de demonstração de exemplo (o `ViewBag.Link` código no controlador da conta. Consulte a `DisplayEmail` e `ForgotPasswordConfirmation` métodos de ação e as exibições do razor).
6. <em>Opcional: * Remova o `ViewBag.Status` código dos controladores de gerenciamento e a conta e do *Views\Account\VerifyCode.cshtml</em> e <em>Views\Manage\VerifyPhoneNumber.cshtml</em> exibições do razor. Como alternativa, você pode manter o `ViewBag.Status` exibição para testar como este aplicativo funciona localmente sem ter de ligar e enviar email e mensagens SMS.

> [!NOTE]
> Aviso: Se você alterar as configurações de segurança neste exemplo, aplicativos de produções precisará passar por uma auditoria de segurança que explicitamente chama as alterações feitas.


<a id="SMS"></a>

## <a name="set-up-sms-for-two-factor-authentication"></a>Configurar o SMS para autenticação de dois fatores

Este tutorial fornece instruções sobre como usar Twilio ou ASPSMS, mas você pode usar qualquer outro provedor SMS.

1. **Criando uma conta de usuário com um provedor de SMS**  
  
   Criar uma [Twilio](https://www.twilio.com/try-twilio) ou um [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) conta.
2. **Instalar pacotes adicionais ou adicionar referências de serviço**  
  
   Twilio:  
   No Console do Gerenciador de pacotes, digite o seguinte comando:  
    `Install-Package Twilio`  
  
   ASPSMS:  
   A seguinte referência de serviço precisa ser adicionado:  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image1.png)  
  
   Endereço:  
    `https://webservice.aspsms.com/aspsmsx2.asmx?WSDL`  
  
   Namespace:  
    `ASPSMSX2`
3. **Descobrir as credenciais do usuário do provedor de SMS**  
  
   Twilio:  
   Do **Dashboard** guia da sua conta do Twilio, copie o **SID de conta** e **token de autenticação**.  
  
   ASPSMS:  
   Em suas configurações de conta, navegue até **Userkey** e copie-o junto com seu autodefinido **senha**.  
  
   Mais tarde armazenaremos esses valores nas variáveis `SMSAccountIdentification` e `SMSAccountPassword` .
4. **Especificando SenderID / originador**  
  
   Twilio:  
   Dos **números** guia, copie o número de telefone do Twilio.  
  
   ASPSMS:  
   Dentro de **desbloquear originadores** Menu, desbloquear originadores de um ou mais, ou escolha um originador alfanumérico (não é suportado por todas as redes).  
  
   Será mais tarde armazenamos esse valor na variável `SMSAccountFrom` .
5. **Transferir as credenciais do provedor SMS para o aplicativo**  
  
   Disponibilize as credenciais e o número de telefone do remetente para o aplicativo:

    [!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample1.cs)]

    > [!WARNING]
    > Security - nunca armazenar os dados confidenciais em seu código-fonte. A conta e as credenciais são adicionadas ao código acima para manter o exemplo simples. Consulte de Jon Atten [ASP.NET MVC: manter privadas configurações fora do controle do código-fonte](http://typecastexception.com/post/2014/04/06/ASPNET-MVC-Keep-Private-Settings-Out-of-Source-Control.aspx).
6. **Implementação de transferência de dados para o provedor de SMS**  
  
   Configurar o `SmsService` classe os *App\_Start\IdentityConfig.cs* arquivo.  
  
   Dependendo do provedor de SMS usado ativar o **Twilio** ou o **ASPSMS** seção: 

    [!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample2.cs)]
7. Execute o aplicativo e faça logon com a conta que você registrou anteriormente.
8. Clique em sua ID de usuário, que ativa o `Index` método de ação `Manage` controlador.  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image2.png)
9. Clique em Adicionar.  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image3.png)
10. Em poucos segundos, você receberá uma mensagem de texto com o código de verificação. Insira-o e pressione **enviar**.  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image4.png)
11. O modo de gerenciar mostra que o número de telefone foi adicionado.  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image5.png)

### <a name="examine-the-code"></a>Examinar o código

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample3.cs?highlight=2)]

O `Index` método de ação `Manage` controlador define a mensagem de status com base em sua ação anterior e fornece links para alterar sua senha local ou adicionar uma conta local. O `Index` método também exibe o estado ou a 2FA 2FA habilitado, logons externos, número de telefone e lembre-se o método 2FA para este navegador (explicado posteriormente). Clicar em sua ID de usuário (email) na barra de título não passa de uma mensagem. Clicar na **número de telefone: Remova** vincular passes `Message=RemovePhoneSuccess` como uma cadeia de caracteres de consulta.  
  
`https://localhost:44300/Manage?Message=RemovePhoneSuccess`

[![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image6.png)]

O `AddPhoneNumber` método de ação exibe uma caixa de diálogo para inserir um número de telefone que possa receber mensagens SMS.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample4.cs)]

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image7.png)

Clicar na **enviar código de verificação** botão posta o número de telefone para o HTTP POST `AddPhoneNumber` método de ação.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample5.cs?highlight=12)]

O `GenerateChangePhoneNumberTokenAsync` método gera o token de segurança que será definido na mensagem SMS. Se o serviço SMS tiver sido configurado, o token é enviado como a cadeia de caracteres &quot;seu código de segurança é &lt;token&gt;&quot;. O `SmsService.SendAsync` método é chamado de forma assíncrona e, em seguida, o aplicativo é redirecionado para o `VerifyPhoneNumber` o método de ação (que exibe a caixa de diálogo a seguir), onde você pode inserir o código de verificação.

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image8.png)

Depois de inserir o código e clique em enviar, o código é postado para o HTTP POST `VerifyPhoneNumber` método de ação.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample6.cs)]

O `ChangePhoneNumberAsync` método verifica o código de segurança lançadas. Se o código estiver correto, o número de telefone é adicionado para o `PhoneNumber` campo do `AspNetUsers` tabela. Se essa chamada for bem-sucedida, o `SignInAsync` método é chamado:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample7.cs)]

O `isPersistent` parâmetro define se a sessão de autenticação é persistente entre várias solicitações.

Quando você altera seu perfil de segurança, um novo carimbo de segurança é gerado e armazenado na `SecurityStamp` campo do *AspNetUsers* tabela. Observe que o `SecurityStamp` campo é diferente do que o cookie de segurança. O cookie de segurança não é armazenado no `AspNetUsers` tabela (ou qualquer outro lugar no banco de dados de identidade). O token de cookie de segurança é autoassinado usando [DPAPI](https://msdn.microsoft.com/library/system.security.cryptography.protecteddata.aspx) e é criado com o `UserId, SecurityStamp` e informações de tempo de expiração.

O middleware de cookie verifica o cookie em cada solicitação. O `SecurityStampValidator` método na `Startup` classe atinge o BD e verifica periodicamente, o carimbo de segurança conforme especificado com o `validateInterval`. Isso ocorre apenas a cada 30 minutos (em nosso exemplo), a menos que você altere seu perfil de segurança. O intervalo de 30 minutos foi escolhido para minimizar as viagens ao banco de dados.

O `SignInAsync` método precisa ser chamado quando qualquer alteração é feita para o perfil de segurança. Quando o perfil de segurança é alterada, o banco de dados é as atualizações a `SecurityStamp` campo e sem chamar o `SignInAsync` método permaneceriam conectado *somente* até a próxima vez que o pipeline do OWIN acessa o banco de dados (o `validateInterval`). Você pode testar isso alterando os `SignInAsync` método para retornar imediatamente e definir o cookie `validateInterval` propriedade de 30 minutos para 5 segundos:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample8.cs?highlight=3)]

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample9.cs?highlight=20-21)]

Com as alterações de código acima, você pode alterar seu perfil de segurança (por exemplo, alterando o estado de **dois fatores ativados**) e você será desconectado em 5 segundos quando o `SecurityStampValidator.OnValidateIdentity` método falhar. Remova a linha de retornada no `SignInAsync` método, alterar a outro perfil de segurança e você será não ser desconectado. O `SignInAsync` método gera um novo cookie de segurança.

<a id="enable2"></a>

## <a name="enable-two-factor-authentication"></a>Habilitar a autenticação de dois fatores

No aplicativo de exemplo, você precisará usar a interface do usuário para habilitar a autenticação de dois fatores (2FA). Para habilitar a 2FA, clique em sua ID de usuário (alias de email) na barra de navegação.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image9.png)  
Clique em Habilitar 2FA.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image10.png) Faça logoff e, em seguida, fazer logon novamente. Se você tiver habilitado o email (consulte minha [tutorial anterior](account-confirmation-and-password-recovery-with-aspnet-identity.md)), você pode selecionar o SMS ou email para 2FA.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image11.png) A página de código de verificar é exibida, onde você pode inserir o código (de SMS ou email).![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image12.png) Clicar na **lembrar deste navegador** caixa de seleção serão isentos da necessidade de usar 2FA para fazer logon com esse computador e o navegador. Habilitar a 2FA e clicar na **lembrar deste navegador** lhe fornecerá 2FA forte proteção contra usuários mal-intencionados que tentam acessar sua conta, desde que não tiverem acesso ao seu computador. Você pode fazer isso em qualquer computador privada, que você usa regularmente. Definindo **lembrar deste navegador**, você obtém a segurança adicional de 2FA de computadores que você não usa regularmente, e você obtém a conveniência em não ter que passar por 2FA em seus próprios computadores. 

<a id="reg"></a>
## <a name="how-to-register-a-two-factor-authentication-provider"></a>Como registrar um provedor de autenticação de dois fatores

Quando você cria um novo projeto do MVC, o *IdentityConfig.cs* arquivo contém o código a seguir para registrar um provedor de autenticação de dois fatores:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample10.cs?highlight=22-35)]

## <a name="add-a-phone-number-for-2fa"></a>Adicionar um número de telefone para 2FA

O `AddPhoneNumber` método de ação a `Manage` controlador gera um token de segurança e envia-o para o telefone número que você forneceu.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample11.cs)]

Depois de enviar o token, ele redireciona para o `VerifyPhoneNumber` método de ação, onde você pode inserir o código para registrar o SMS 2fa. SMS 2FA não é usado até que você verificou que o número de telefone.

## <a name="enabling-2fa"></a>Habilitar a 2FA

O `EnableTFA` 2FA habilita a método de ação:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample12.cs)]

Observação o `SignInAsync` deve ser chamado como habilitar 2FA é uma alteração para o perfil de segurança. Quando 2FA estiver habilitado, o usuário precisará usar 2FA para entrar, usando as abordagens 2FA já tenham registrado (SMS e email no exemplo).

Você pode adicionar mais provedores de 2FA, como geradores de código QR ou você pode gravar sua própria (consulte [usando o Google Authenticator com o ASP.NET Identity](http://www.beabigrockstar.com/blog/using-google-authenticator-asp-net-identity/)).

> [!NOTE]
> Os códigos de 2FA são gerados usando [baseada em tempo algoritmo de senha de uso único](http://en.wikipedia.org/wiki/Time-based_One-time_Password_Algorithm) e códigos são válidos por seis minutos. Se você levar mais de seis minutos para inserir o código, você obterá uma mensagem de erro de código inválido.


<a id="combine"></a>

## <a name="combine-social-and-local-login-accounts"></a>Combine as contas de logon social e local

Você pode combinar as contas locais e sociais clicando no link seu email. Na sequência a seguir &quot; RickAndMSFT@gmail.com &quot; é criado como um logon local, mas você pode criar a conta como um logon social no primeiro e adicionar um logon local.

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image13.png)

Clique no **gerenciar** link. Observe externo 0 (logons sociais) associado a essa conta.

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image14.png)

Clique no link para outro log no serviço e aceitar as solicitações do aplicativo. As duas contas foram combinadas, você poderá fazer logon com qualquer uma das contas. Convém que os usuários adicionem contas locais no caso de seu logon social no serviço de autenticação está inoperante ou, mais provavelmente eles tem perdido o acesso à sua conta social.

Na imagem a seguir, Tom é um logon social (que você pode ver o **logons externos: 1** mostradas na página).

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image15.png)

Clicar em **escolha uma senha** permite que você adicione um log local em associado com a mesma conta.

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image16.png)

<a id="lock"></a>

## <a name="account-lockout-from-brute-force-attacks"></a>Bloqueio de conta contra ataques de força bruta

Você pode proteger as contas em seu aplicativo contra ataques de dicionário, permitindo o bloqueio do usuário. O código a seguir no `ApplicationUserManager Create` método configura o bloqueio:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample13.cs)]

O código acima permite que o bloqueio para apenas a autenticação de dois fatores. Embora você possa permitir bloqueio para logons, alterando `shouldLockout` como true na `Login` método do controlador de conta, é recomendável habilitar bloqueio para logons não porque torna a conta suscetíveis a [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) logon os ataques. No código de exemplo, o bloqueio está desabilitado para a conta de administrador criada a `ApplicationDbInitializer Seed` método:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample14.cs?highlight=19)]

## <a name="requiring-a-user-to-have-a-validated-email-account"></a>Exigir que o usuário tiver uma conta de email validada

O código a seguir exige que um usuário tenha uma conta de email validada antes de fazer logon:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample15.cs?highlight=8-17)]

## <a name="how-signinmanager-checks-for-2fa-requirement"></a>Como o SignInManager procura requisito 2FA

Logon local e para logon social verificação para ver se 2FA está habilitada. Se 2FA estiver habilitado, o `SignInManager` retorna o método de logon `SignInStatus.RequiresVerification`, e o usuário será redirecionado para o `SendCode` método de ação, em que eles terão de digitar o código para concluir o log em sequência. Se o usuário tiver RememberMe é definido no cookie de local de usuários, o `SignInManager` retornará `SignInStatus.Success` e eles não terão de passar por 2FA.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample16.cs?highlight=20-22)]

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample17.cs?highlight=10-11,17-18)]

O seguinte código mostra o `SendCode` método de ação. Um [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) é criado com todos os métodos de 2FA habilitados para o usuário. O [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) é passado para o [DropDownListFor](https://msdn.microsoft.com/library/system.web.ui.webcontrols.dropdownlist.aspx) auxiliar, que permite que o usuário selecionar a abordagem 2FA (normalmente o email e SMS).

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample18.cs)]

Depois que o usuário posta a abordagem 2FA, o `HTTP POST SendCode` método de ação é chamado, o `SignInManager` envia o código de 2FA e o usuário é redirecionado para o `VerifyCode` método de ação no qual eles podem digitar o código para concluir o logon.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample19.cs?highlight=3,13-14,18)]

## <a name="2fa-lockout"></a>2FA Lockout

Embora você possa definir o bloqueio de conta em falhas de tentativa de senha de logon, essa abordagem torna seu logon suscetíveis a [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) bloqueios. É recomendável que você use o bloqueio de conta apenas com 2FA. Quando o `ApplicationUserManager` é criado, o código de exemplo define bloqueio 2FA e `MaxFailedAccessAttemptsBeforeLockout` para cinco. Depois que um usuário fizer logon (por meio de conta local ou social), cada tentativa com falha no 2FA é armazenada e se o máximo de tentativas for atingido, o usuário está bloqueado por cinco minutos (você pode definir o bloqueio de tempo apenas com `DefaultAccountLockoutTimeSpan`).

<a id="addRes"></a>

## <a name="additional-resources"></a>Recursos adicionais

- [Recursos recomendados da identidade do ASP.NET](../getting-started/aspnet-identity-recommended-resources.md) então vincula uma lista completa de blogs, vídeos, tutoriais e excelente de identidade.
- [Aplicativo do MVC 5 com o Facebook, Twitter, LinkedIn e Google OAuth2 Sign-on](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) também mostra como adicionar informações de perfil para a tabela de usuários.
- [ASP.NET MVC e identidade 2.0: Noções básicas sobre os conceitos básicos](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx) por John Atten.
- [Confirmação de conta e recuperação de senha com o ASP.NET Identity](account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [Introdução à Identidade do ASP.NET](../getting-started/introduction-to-aspnet-identity.md)
- [Anunciando o RTM do ASP.NET Identity 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx) de Pranav Rastogi.
- [Identidade do ASP.NET 2.0: Configuração validação da conta e a autorização de dois fatores](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) por John Atten.

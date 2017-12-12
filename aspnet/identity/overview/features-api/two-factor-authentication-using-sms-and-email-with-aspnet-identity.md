---
uid: identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity
title: "Autenticação de dois fatores usando SMS e email com o ASP.NET Identity | Microsoft Docs"
author: HaoK
description: "Este tutorial mostrará como configurar a autenticação de dois fatores (2FA) usando o SMS e email. Este artigo foi escrito de Rick Anderson ( @RickAndMSFT ), por..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/15/2015
ms.topic: article
ms.assetid: 053e23c4-13c9-40fa-87cb-3e9b0823b31e
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: ecb1fc693063995a3a05a7af5db64554c9f595e2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="two-factor-authentication-using-sms-and-email-with-aspnet-identity"></a>Autenticação de dois fatores usando SMS e email com a identidade do ASP.NET
====================
por [Kung Hao](https://github.com/HaoK), [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://github.com/Rick-Anderson), [Suhas Joshi](https://github.com/suhasj)

> Este tutorial mostrará como configurar a autenticação de dois fatores (2FA) usando o SMS e email.
> 
> Este artigo foi escrito de Rick Anderson ([@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)), Pranav Rastogi ([@rustd](https://twitter.com/rustd)), Hao Kung e Suhas Joshi. O exemplo do NuGet foi escrito principalmente por Hao Kung.


Este tópico aborda os seguintes tópicos:

- [Compilando o exemplo de identidade](#build)
- [Configurar o SMS para autenticação de dois fatores](#SMS)
- [Habilitar a autenticação de dois fatores](#enable2)
- [Como registrar um provedor de autenticação de dois fatores](#reg)
- [Combinar as contas de logon local e social](#combine)
- [Bloqueio de conta contra ataques de força bruta](#lock)

<a id="build"></a>

## <a name="building-the-identity-sample"></a>Compilando o exemplo de identidade

Nesta seção, você usará o NuGet para baixar um exemplo com que trabalharemos. Comece instalando e executando [Visual Studio Express 2013 para Web](https://go.microsoft.com/fwlink/?LinkId=299058) ou [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Instalar o Visual Studio [2013 atualização 2](https://go.microsoft.com/fwlink/?LinkId=390521) ou superior.

> [!NOTE]
> Aviso: Você deve instalar o Visual Studio [2013 atualização 2](https://go.microsoft.com/fwlink/?LinkId=390521) para concluir este tutorial.


1. Criar um novo ***vazio*** projeto da Web do ASP.NET.
2. No Console do Gerenciador de pacotes, digite os seguintes comandos a seguir:  
  
    `Install-Package SendGrid`  
    `Install-Package -Prerelease Microsoft.AspNet.Identity.Samples`  
  
 Neste tutorial, vamos usar [SendGrid](http://sendgrid.com/) para enviar email e [Twilio](https://www.twilio.com/) ou [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) para textos do sms. O `Identity.Samples` pacote instala o código que trabalhará.
3. Definir o [projeto para usar SSL](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).
4. *Opcional*: siga as instruções no meu [tutorial de confirmação de Email](account-confirmation-and-password-recovery-with-aspnet-identity.md) ligar SendGrid e, em seguida, executar o aplicativo e registrar uma conta de email.
5. * Opcional: * Remova o código de confirmação de link de email de demonstração do exemplo (o `ViewBag.Link` código no controlador de conta. Consulte o `DisplayEmail` e `ForgotPasswordConfirmation` métodos de ação e modos de exibição do razor).
6. * Opcional: * Remova o `ViewBag.Status` código de controladores de gerenciamento e a conta no *Views\Account\VerifyCode.cshtml* e *Views\Manage\VerifyPhoneNumber.cshtml* modos de exibição do razor. Como alternativa, você pode manter o `ViewBag.Status` exibição para testar como este aplicativo funciona localmente sem a necessidade de conectar e enviar email e mensagens SMS.

> [!NOTE]
> Aviso: Se você alterar as configurações de segurança neste exemplo, aplicativos de produção serão necessário passar por uma auditoria de segurança que explicitamente chama as alterações feitas.


<a id="SMS"></a>

## <a name="set-up-sms-for-two-factor-authentication"></a>Configurar o SMS para autenticação de dois fatores

Este tutorial fornece instruções sobre como usar o Twilio ou ASPSMS, mas você pode usar qualquer outro provedor SMS.

1. **Criando uma conta de usuário com um provedor de SMS**  
  
 Criar um [Twilio](https://www.twilio.com/try-twilio) ou um [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) conta.
2. **Instalando pacotes adicionais ou adicionando referências de serviço**  
  
 Twilio:  
 No Console do Gerenciador de pacotes, digite o seguinte comando:  
    `Install-Package Twilio`  
  
 ASPSMS:  
 A referência de serviço a seguir precisa ser adicionado:  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image1.png)  
  
 endereço:  
    `https://webservice.aspsms.com/aspsmsx2.asmx?WSDL`  
  
 Namespace:  
    `ASPSMSX2`
3. **Descobrir as credenciais do usuário do provedor de SMS**  
  
 Twilio:  
 Do **painel** guia da sua conta do Twilio, copie o **SID de conta** e **token de autenticação**.  
  
 ASPSMS:  
 De suas configurações de conta, navegue até **chave de usuário confidenciais** e copiá-lo junto com seus Self definido **senha**.  
  
 Mais tarde armazenaremos esses valores nas variáveis de `SMSAccountIdentification` e `SMSAccountPassword` .
4. **Especificando SenderID / originador**  
  
 Twilio:  
 Do **números** guia, copie o seu número de telefone do Twilio.  
  
 ASPSMS:  
 Dentro de **desbloquear originadores** Menu desbloquear originadores de uma ou mais ou escolha um originador alfanumérico (não é suportado por todas as redes).  
  
 Posteriormente, armazenará esse valor na variável `SMSAccountFrom` .
5. **Transferindo as credenciais do provedor SMS em aplicativo**  
  
 Verifique as credenciais e o número de telefone do remetente disponíveis para o aplicativo:

    [!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample1.cs)]

    > [!WARNING]
    > Segurança - nunca armazenar os dados confidenciais em seu código-fonte. A conta e as credenciais são adicionadas ao código acima para manter o exemplo simples. Consulte de Jon Atten [ASP.NET MVC: manter particulares configurações fora do controle do código-fonte](http://typecastexception.com/post/2014/04/06/ASPNET-MVC-Keep-Private-Settings-Out-of-Source-Control.aspx).
6. **Implementação de transferência de dados para o provedor de SMS**  
  
 Configurar o `SmsService` classe no *aplicativo\_Start\IdentityConfig.cs* arquivo.  
  
 Dependendo do provedor SMS usado ativar o **Twilio** ou **ASPSMS** seção: 

    [!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample2.cs)]
7. Executar o aplicativo e faça logon usando a conta que você registrou anteriormente.
8. Clique em sua ID de usuário, que ativa o `Index` método de ação `Manage` controlador.  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image2.png)
9. Clique em Adicionar.  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image3.png)
10. Em alguns segundos, você receberá uma mensagem de texto com o código de verificação. Insira-o e pressione **enviar**.  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image4.png)
11. O modo de gerenciar mostra que o número de telefone foi adicionado.  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image5.png)

### <a name="examine-the-code"></a>Examine o código

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample3.cs?highlight=2)]

O `Index` método de ação `Manage` controlador define a mensagem de status com base em sua ação anterior e fornece links para alterar sua senha local ou adicionar uma conta local. O `Index` método também exibe o estado ou a 2FA 2FA habilitado, logons externos, número de telefone e lembre-se o método 2FA para este navegador (explicado posteriormente). Clicando em sua ID de usuário (email) na barra de título não passa uma mensagem. Clicar no **telefone: remover** link passa `Message=RemovePhoneSuccess` como uma cadeia de caracteres de consulta.  
  
`https://localhost:44300/Manage?Message=RemovePhoneSuccess`

[![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image6.png)]

O `AddPhoneNumber` método de ação exibe uma caixa de diálogo para inserir um número de telefone que pode receber mensagens SMS.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample4.cs)]

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image7.png)

Clicar no **enviar o código de verificação** botão envia o número de telefone para o HTTP POST `AddPhoneNumber` método de ação.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample5.cs?highlight=12)]

O `GenerateChangePhoneNumberTokenAsync` método gera o token de segurança que será definido na mensagem SMS. Se o serviço SMS tiver sido configurado, o token é enviado como a cadeia de caracteres &quot;seu código de segurança é &lt;token&gt;&quot;. O `SmsService.SendAsync` método é chamado de forma assíncrona, em seguida, o aplicativo é redirecionado para o `VerifyPhoneNumber` método de ação (que exibe a caixa de diálogo a seguir), onde você pode inserir o código de verificação.

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image8.png)

Depois que você insira o código e clique em enviar, o código é postado para o HTTP POST `VerifyPhoneNumber` método de ação.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample6.cs)]

O `ChangePhoneNumberAsync` método verifica o código de segurança lançadas. Se o código está correto, o número de telefone é adicionado para o `PhoneNumber` campo o `AspNetUsers` tabela. Se essa chamada for bem-sucedida, o `SignInAsync` método é chamado:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample7.cs)]

O `isPersistent` parâmetro define se a sessão de autenticação é persistente entre várias solicitações.

Quando você altera seu perfil de segurança, um novo carimbo de segurança é gerado e armazenado na `SecurityStamp` campo o *AspNetUsers* tabela. Observe que o `SecurityStamp` campo é diferente do cookie de segurança. O cookie de segurança não é armazenado no `AspNetUsers` tabela (ou em qualquer lugar no banco de dados de identidade). O token de cookie de segurança é autoassinado usando [DPAPI](https://msdn.microsoft.com/en-us/library/system.security.cryptography.protecteddata.aspx) e é criado com o `UserId, SecurityStamp` e informações de tempo de expiração.

O middleware de cookie verifica o cookie em cada solicitação. O `SecurityStampValidator` método o `Startup` classe atinge o banco de dados e verifica periodicamente, o carimbo de segurança conforme especificado com o `validateInterval`. Isso acontece apenas a cada 30 minutos (em nosso exemplo), a menos que você altere seu perfil de segurança. O intervalo de 30 minutos foi escolhido para minimizar as viagens ao banco de dados.

O `SignInAsync` método precisa ser chamado quando nenhuma alteração é feita para o perfil de segurança. Quando o perfil de segurança for alterada, o banco de dados é as atualizações de `SecurityStamp` campo e sem chamar o `SignInAsync` método seria permanecer conectado *somente* até a próxima vez que o pipeline OWIN atinge o banco de dados (o `validateInterval`). Você pode testar isso alterando o `SignInAsync` método para retornar imediatamente e definindo o cookie `validateInterval` propriedade de 30 minutos para 5 segundos:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample8.cs?highlight=3)]

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample9.cs?highlight=20-21)]

Com as alterações de código acima, você pode alterar seu perfil de segurança (por exemplo, alterando o estado do **dois fatores ativados**) e você será desconectado em 5 segundos quando o `SecurityStampValidator.OnValidateIdentity` método falhar. Remova a linha de retorno no `SignInAsync` método, alterar a outro perfil de segurança e você não será registrado-out. O `SignInAsync` método gera um novo cookie de segurança.

<a id="enable2"></a>

## <a name="enable-two-factor-authentication"></a>Habilitar a autenticação de dois fatores

No aplicativo de exemplo, você precisa usar a interface do usuário para habilitar a autenticação de dois fatores (2FA). Para habilitar a 2FA, clique em sua ID de usuário (alias de email) na barra de navegação.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image9.png)  
Clique em Habilitar 2FA.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image10.png) Faça logoff e logon novamente. Se você tiver habilitado o email (consulte meu [tutorial anterior](account-confirmation-and-password-recovery-with-aspnet-identity.md)), você pode selecionar o email para 2FA ou SMS.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image11.png) A página de código verificar é exibida, onde você pode inserir o código (de SMS ou email).![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image12.png) Clicar no **lembrar este navegador** caixa de seleção isentará a necessidade de usar 2FA para fazer logon com esse computador e o navegador. Habilitando 2FA e clicando no **lembrar este navegador** fornecerá 2FA forte proteção contra usuários mal-intencionados que tentar acessar sua conta, desde que eles não têm acesso a seu computador. Você pode fazer isso em qualquer computador privada que você usa regularmente. Definindo **lembrar este navegador**, obter a segurança adicional de 2FA de computadores que você não usa regularmente e obter a conveniência de não ter percorrer 2FA em seus próprios computadores. 

<a id="reg"></a>
## <a name="how-to-register-a-two-factor-authentication-provider"></a>Como registrar um provedor de autenticação de dois fatores

Quando você cria um novo projeto MVC, a *IdentityConfig.cs* arquivo contém o código a seguir para registrar um provedor de autenticação de dois fatores:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample10.cs?highlight=22-35)]

## <a name="add-a-phone-number-for-2fa"></a>Adicionar um número de telefone para 2FA

O `AddPhoneNumber` método de ação de `Manage` controlador gera um token de segurança e envia-o para o telefone número que você forneceu.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample11.cs)]

Depois de enviar o token, ele redireciona para o `VerifyPhoneNumber` método de ação, onde você pode inserir o código para registrar o SMS para 2FA. 2FA SMS não é usado até que você verificou se o número de telefone.

## <a name="enabling-2fa"></a>Habilitando 2FA

O `EnableTFA` método de ação permite 2FA:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample12.cs)]

Observe o `SignInAsync` deve ser chamado porque habilitar 2FA é uma alteração para o perfil de segurança. Quando 2FA estiver habilitado, o usuário precisará usar 2FA para logon, usando as abordagens 2FA eles registraram (SMS e email no exemplo).

Você pode adicionar mais provedores de 2FA como geradores de código QR ou você pode escrever sua propriedade (consulte [usando o Google Authenticator com a identidade do ASP.NET](http://www.beabigrockstar.com/blog/using-google-authenticator-asp-net-identity/)).

> [!NOTE]
> Os códigos de 2FA são gerados usando [baseada em tempo algoritmo de senha de uso único](http://en.wikipedia.org/wiki/Time-based_One-time_Password_Algorithm) e são válidos por seis minutos. Se você levar mais de seis minutos para inserir o código, você obterá uma mensagem de erro de código inválido.


<a id="combine"></a>

## <a name="combine-social-and-local-login-accounts"></a>Combinar as contas de logon local e social

Você pode combinar as contas locais e sociais clicando no link seu email. Na sequência a seguir &quot; RickAndMSFT@gmail.com &quot; é criado pela primeira vez como um logon local, mas você pode criar a conta como um log social primeiro e adicionar um logon local.

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image13.png)

Clique no **gerenciar** link. Observe externo 0 (logons sociais) associada à conta.

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image14.png)

Clique no link para outro log no serviço e aceitar as solicitações do aplicativo. As duas contas foram combinadas, você poderá fazer logon com a conta. Convém que os usuários adicionem contas locais no caso de seu log social no serviço de autenticação está inoperante ou mais provável que perdeu o acesso à sua conta social.

Na imagem a seguir, Tom é um logon social (que você pode ver o **logons externos: 1** mostradas na página).

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image15.png)

Clicando em **escolher uma senha** permite que você adicione um log local em associados com a mesma conta.

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image16.png)

<a id="lock"></a>

## <a name="account-lockout-from-brute-force-attacks"></a>Bloqueio de conta contra ataques de força bruta

Você pode proteger as contas em seu aplicativo contra ataques de dicionário, permitindo o bloqueio do usuário. O código a seguir no `ApplicationUserManager Create` método configura o bloqueio:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample13.cs)]

O código acima permite que o bloqueio para somente a autenticação de dois fatores. Enquanto você pode habilitar o bloqueio para logons alterando `shouldLockout` como verdadeiro no `Login` método do controlador de conta, é recomendável não habilitar bloqueio para logons porque ele faz com que a conta suscetíveis a [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) logon ataques. No código de exemplo, o bloqueio está desabilitado para a conta de administrador criada no `ApplicationDbInitializer Seed` método:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample14.cs?highlight=19)]

## <a name="requiring-a-user-to-have-a-validated-email-account"></a>Exigir que o usuário tenha uma conta de email validado

O código a seguir requer que um usuário tenha uma conta de email validadas antes de fazer logon:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample15.cs?highlight=8-17)]

## <a name="how-signinmanager-checks-for-2fa-requirement"></a>Como o SignInManager procura 2FA requisito

Logon local e para log social na verificação para ver se 2FA está habilitada. Se 2FA estiver habilitado, o `SignInManager` retorna do método de logon `SignInStatus.RequiresVerification`, e o usuário será redirecionado para o `SendCode` método de ação, onde eles terão de digitar o código para concluir o logon na sequência. Se o usuário tiver RememberMe é definido no cookie de local de usuários, o `SignInManager` retornará `SignInStatus.Success` e não será necessário passar por 2FA.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample16.cs?highlight=20-22)]

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample17.cs?highlight=10-11,17-18)]

O código a seguir mostra o `SendCode` método de ação. Um [SelectListItem](https://msdn.microsoft.com/en-us/library/system.web.mvc.selectlistitem.aspx) é criado com todos os métodos de 2FA habilitados para o usuário. O [SelectListItem](https://msdn.microsoft.com/en-us/library/system.web.mvc.selectlistitem.aspx) é passado para o [DropDownListFor](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.dropdownlist.aspx) auxiliar, que permite ao usuário selecionar a abordagem 2FA (normalmente, email e SMS).

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample18.cs)]

Depois que o usuário envia a abordagem 2FA, o `HTTP POST SendCode` é chamado de método de ação, o `SignInManager` envia o código de 2FA e o usuário é redirecionado para o `VerifyCode` método de ação podem digitar o código para concluir o logon.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample19.cs?highlight=3,13-14,18)]

## <a name="2fa-lockout"></a>Bloqueio de 2FA

Embora você possa definir o bloqueio de conta em falhas de tentativa de senha de logon, essa abordagem torna seu logon suscetíveis a [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) bloqueios. Recomendamos que você use o bloqueio de conta apenas com 2FA. Quando o `ApplicationUserManager` é criado, o código de exemplo define 2FA bloqueio e `MaxFailedAccessAttemptsBeforeLockout` a cinco. Depois que um usuário fizer logon (por meio de conta local ou social), cada tentativa falha de 2FA é armazenada e se o número máximo de tentativas for atingido, o usuário é bloqueado por cinco minutos (você pode definir o bloqueio de tempo com `DefaultAccountLockoutTimeSpan`).

<a id="addRes"></a>

## <a name="additional-resources"></a>Recursos adicionais

- [Identidade do ASP.NET recomendado recursos](../getting-started/aspnet-identity-recommended-resources.md) assim vincula uma lista completa de blogs de identidade, vídeos, tutoriais e ótimo.
- [Aplicativo do MVC 5 com o Facebook, Twitter, LinkedIn e logon do Google OAuth2](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) também mostra como adicionar informações de perfil para a tabela de usuários.
- [ASP.NET MVC e identidade 2.0: Noções básicas](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx) por John Atten.
- [Confirmação de conta e senha de recuperação com a identidade do ASP.NET](account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [Introdução a identidade do ASP.NET](../getting-started/introduction-to-aspnet-identity.md)
- [Anunciando RTM do ASP.NET Identity 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx) por Pranav Rastogi.
- [Identidade do ASP.NET 2.0: Configurando a validação da conta e a autorização de dois fatores](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) por John Atten.

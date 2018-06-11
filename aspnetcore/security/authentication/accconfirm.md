---
title: Confirmação de conta e de recuperação de senha no ASP.NET Core
author: rick-anderson
description: Saiba como criar um aplicativo do ASP.NET Core com redefinição de senha e de confirmação de email.
manager: wpickett
ms.author: riande
ms.date: 2/11/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/accconfirm
ms.openlocfilehash: d7c1aea2b533fc614eb25c537b72bea773e76077
ms.sourcegitcommit: 63fb07fb3f71b32daf2c9466e132f2e7cc617163
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/10/2018
ms.locfileid: "35252276"
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a>Confirmação de conta e de recuperação de senha no ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT) e [Joe Audette](https://twitter.com/joeaudette)

Este tutorial mostra como criar um aplicativo do ASP.NET Core com redefinição de senha e de confirmação de email. Este tutorial é **não** um tópico de início. Você deve estar familiarizado com:

* [ASP.NET Core](xref:tutorials/first-mvc-app/start-mvc)
* [Autenticação](xref:security/authentication/index)
* [Confirmação de conta e recuperação de senha](xref:security/authentication/accconfirm)
* [Entity Framework Core](xref:data/ef-mvc/intro)

Consulte [este arquivo PDF](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) para as versões do ASP.NET Core MVC 1.1 e 2. x.

## <a name="prerequisites"></a>Pré-requisitos

[!INCLUDE [](~/includes/net-core-prereqs.md)]

## <a name="create-a-new-aspnet-core-project-with-the-net-core-cli"></a>Criar um novo projeto do ASP.NET Core com o .NET Core CLI

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

::: moniker range=">= aspnetcore-2.1"

```console
dotnet new webapp --auth Individual -o WebPWrecover
cd WebPWrecover
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
dotnet new razor --auth Individual -o WebPWrecover
cd WebPWrecover
```

::: moniker-end

* `--auth Individual` Especifica o modelo de projeto de contas de usuário individuais.
* No Windows, adicione o `-uld` opção. Especifica que o LocalDB deve ser usado em vez do SQLite.
* Execute `new mvc --help` para obter ajuda sobre este comando.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Se você estiver usando a CLI ou SQLite, execute o seguinte em uma janela de comando:

```console
dotnet new mvc --auth Individual
```

* `--auth Individual` Especifica o modelo de projeto de contas de usuário individuais.
* No Windows, adicione o `-uld` opção. Especifica que o LocalDB deve ser usado em vez do SQLite.
* Execute `new mvc --help` para obter ajuda sobre este comando.

---

Como alternativa, você pode criar um novo projeto ASP.NET Core com o Visual Studio:

* No Visual Studio, crie um novo **aplicativo Web** projeto.
* Selecione **Core ASP.NET 2.0**. **.NET core** está selecionado na imagem a seguir, mas você pode selecionar **do .NET Framework**.
* Selecione **alterar autenticação** e definido como **contas de usuário individuais**.
* Mantenha o padrão **no aplicativo de contas de usuário do repositório**.

![Nova caixa de diálogo do projeto mostrando "Opção de contas de usuário individuais" selecionada](accconfirm/_static/2.png)

## <a name="test-new-user-registration"></a>Testar o novo registro de usuário

Executar o aplicativo, selecione o **registrar** vincular e registrar um usuário. Siga as instruções para executar migrações de Entity Framework Core. Neste ponto, a validação somente do email é com o [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) atributo. Depois de enviar o registro, você está conectado no aplicativo. Posteriormente no tutorial, o código foi atualizado para que novos usuários não podem fazer logon até que o email foi validado.

## <a name="view-the-identity-database"></a>Exibir o banco de dados de identidade

Consulte [trabalhar com SQLite em um projeto MVC do ASP.NET Core](xref:tutorials/first-mvc-app-xplat/working-with-sql) para obter instruções sobre como exibir o banco de dados SQLite.

Para o Visual Studio:

* Do **exibição** menu, selecione **Pesquisador de objetos do SQL Server** (SSOX).
* Navegue até **(localdb) MSSQLLocalDB (SQL Server 13)**. Clique duas vezes em **dbo. AspNetUsers** > **exibir dados**:

![Menu de contexto no AspNetUsers tabela no Pesquisador de objetos do SQL Server](accconfirm/_static/ssox.png)

Observe a tabela `EmailConfirmed` campo é `False`.

Você talvez queira usar o email novamente na próxima etapa, quando o aplicativo envia um email de confirmação. Clique na linha e selecione **excluir**. Excluir o alias de email torna mais fácil nas etapas a seguir.

---

## <a name="require-https"></a>Exigir HTTPS

Consulte [exigir HTTPS](xref:security/enforcing-ssl).

<a name="prevent-login-at-registration"></a>
## <a name="require-email-confirmation"></a>Solicitar confirmação de email

É uma prática recomendada para confirmar o email de um novo registro de usuário. Ajuda de confirmação para verificar se eles não estiver representando alguém de email (ou seja, eles ainda não registrados com outra pessoa email). Suponha que você tivesse um fórum de discussão, e quiser impedir "yli@example.com"do registro como"nolivetto@contoso.com". Sem confirmação por email, "nolivetto@contoso.com" pode receber email indesejado de seu aplicativo. Suponha que o usuário registrado acidentalmente como "ylo@example.com" e ainda não tenha percebido a digitação incorreta da "yli". Elas não serão capazes de usar a recuperação de senha porque o aplicativo não tiver seu email correto. Email de confirmação oferece apenas proteção limitada de robôs. Email de confirmação não fornece proteção contra usuários mal-intencionados com várias contas de email.

Geralmente você deseja impedir que novos usuários incluam dados em seu site até que eles tenham um email confirmado.

Atualização `ConfigureServices` para exigir um email confirmado:

[!code-csharp[](accconfirm/sample/WebPWrecover/Startup.cs?name=snippet1&highlight=12-17)]

`config.SignIn.RequireConfirmedEmail = true;` impede que usuários registrados fazer logon até que o email foi confirmado.

### <a name="configure-email-provider"></a>Configurar o provedor de email

Neste tutorial, SendGrid é usada para enviar email. Você precisa de uma conta do SendGrid e a chave para enviar email. Você pode usar outros provedores de email. ASP.NET Core 2. x inclui `System.Net.Mail`, que permite enviar email de seu aplicativo. É recomendável que usar o SendGrid ou outro serviço de email para enviar email. O SMTP é difícil proteger e configurado corretamente.

O [padrão de opções](xref:fundamentals/configuration/options) é usado para acessar as configurações de conta e a chave de usuário. Para obter mais informações, consulte [configuração](xref:fundamentals/configuration/index).

Crie uma classe para obter a chave de email seguro. Para este exemplo, o `AuthMessageSenderOptions` classe é criada no *Services/AuthMessageSenderOptions.cs* arquivo:

[!code-csharp[](accconfirm/sample/WebPWrecover/Services/AuthMessageSenderOptions.cs?name=snippet1)]

Definir o `SendGridUser` e `SendGridKey` com o [ferramenta Gerenciador de segredo](xref:security/app-secrets). Por exemplo:

```console
C:\WebAppl\src\WebApp1>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

No Windows, o segredo Manager armazena pares de chaves/valor em uma *secrets.json* arquivo o `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` directory.

O conteúdo do *secrets.json* arquivo não são criptografados. O *secrets.json* arquivo é mostrado a seguir (o `SendGridKey` valor foi removido.)

 ```json
  {
    "SendGridUser": "RickAndMSFT",
    "SendGridKey": "<key removed>"
  }
  ```

### <a name="configure-startup-to-use-authmessagesenderoptions"></a>Configurar a inicialização para usar AuthMessageSenderOptions

Adicionar `AuthMessageSenderOptions` ao contêiner de serviço no final o `ConfigureServices` método o *Startup.cs* arquivo:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](accconfirm/sample/WebPWrecover/Startup.cs?name=snippet2&highlight=28)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=26)]

---

### <a name="configure-the-authmessagesender-class"></a>Configurar a classe AuthMessageSender

Este tutorial mostra como adicionar notificações de email por meio de [SendGrid](https://sendgrid.com/), mas você pode enviar emails usando SMTP e outros mecanismos.

Instalar o `SendGrid` pacote do NuGet:

* Na linha de comando:

    `dotnet add package SendGrid`

* No Console do Gerenciador de pacotes, digite o seguinte comando:

  `Install-Package SendGrid`

Consulte [comece com SendGrid gratuitamente](https://sendgrid.com/free/) para registrar-se para uma conta gratuita do SendGrid.

#### <a name="configure-sendgrid"></a>Configurar o SendGrid

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

Para configurar o SendGrid, adicione o código semelhante ao seguinte no *Services/EmailSender.cs*:

[!code-csharp[](accconfirm/sample/WebPWrecover/Services/EmailSender.cs)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

* Adicione código *Services/MessageServices.cs* semelhante à seguinte para configurar o SendGrid:

[!code-csharp[](accconfirm/sample/WebApp1/Services/MessageServices.cs)]

---

## <a name="enable-account-confirmation-and-password-recovery"></a>Habilitar a recuperação de confirmação e a senha da conta

O modelo tem o código de recuperação de confirmação e a senha da conta. Localizar o `OnPostAsync` método *Pages/Account/Register.cshtml.cs*.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

Impedi que usuários recém-registrados sendo registrados automaticamente pelo comentar a seguinte linha:

```csharp
await _signInManager.SignInAsync(user, isPersistent: false);
```

O método complete é mostrado com a linha alterada realçada:

[!code-csharp[](accconfirm/sample/WebPWrecover/Pages/Account/Register.cshtml.cs?highlight=16&name=snippet_Register)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

Para habilitar a confirmação de conta, descomente o código a seguir:

[!code-csharp[](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=16-25&name=snippet_Register)]

**Observação:** o código está impedindo que um usuário registrado recentemente que estão sendo registrados automaticamente pelo comentar a seguinte linha:

```csharp
//await _signInManager.SignInAsync(user, isPersistent: false);
```

Habilitar a recuperação de senha por uncommenting o código de `ForgotPassword` ação de *Controllers/AccountController.cs*:

[!code-csharp[](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=17-23&name=snippet_ForgotPassword)]

Remova o elemento de formulário em *Views/Account/ForgotPassword.cshtml*. Talvez você queira remover o `<p> For more information on how to enable reset password ... </p>` elemento, que contém um link para este artigo.

[!code-cshtml[](accconfirm/sample/WebApp1/Views/Account/ForgotPassword.cshtml?highlight=7-10,12,28)]

---

## <a name="register-confirm-email-and-reset-password"></a>Registrar, confirme o email e redefinição de senha

Executar o aplicativo web e testar o fluxo de recuperação de senha e confirmação de conta.

* Execute o aplicativo e registrar um novo usuário

  ![Registrar conta exibição do aplicativo Web](accconfirm/_static/loginaccconfirm1.png)

* Verifique seu email para o link de confirmação de conta. Consulte [depurar email](#debug) se você não receber o email.
* Clique no link para confirmar seu email.
* Faça logon com seu email e senha.
* Fazer logoff.

### <a name="view-the-manage-page"></a>Exibir a página de gerenciamento

Selecione o nome de usuário no navegador: ![janela do navegador com o nome de usuário](accconfirm/_static/un.png)

Talvez seja necessário expandir a barra de navegação para ver o nome de usuário.

![barra de navegação](accconfirm/_static/x.png)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

A página de gerenciamento é exibida com o **perfil** guia selecionada. O **Email** mostra uma caixa de seleção que indica o email foi confirmada.

![página Gerenciar](accconfirm/_static/rick2.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Isto é mencionado no tutorial posteriormente.
![página Gerenciar](accconfirm/_static/rick2.png)

---

### <a name="test-password-reset"></a>Redefinição de senha do teste

* Se você estiver conectado, selecione **Logout**.
* Selecione o **login** link e selecione o **esqueceu sua senha?** link.
* Insira o email que é usado para registrar a conta.
* É enviado um email com um link para redefinir sua senha. Verifique seu email e clique no link para redefinir sua senha. Depois que sua senha foi redefinida com êxito, você pode fazer logon com seu email e a nova senha.

<a name="debug"></a>

### <a name="debug-email"></a>Depurar email

Se você não pode receber email de trabalho:

* Criar um [aplicativo de console para enviar email](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html).
* Examine o [atividade Email](https://sendgrid.com/docs/User_Guide/email_activity.html) página.
* Verifique a pasta de spam.
* Tente outro alias de email em um provedor de email diferente (Microsoft, Yahoo, Gmail, etc.)
* Tente enviar para contas de email diferente.

**Uma prática recomendada de segurança** é **não** use segredos de produção no desenvolvimento e teste. Se você publicar o aplicativo no Azure, você pode definir os segredos do SendGrid como configurações de aplicativo no portal do aplicativo Web do Azure. O sistema de configuração é configurado para ler as chaves de variáveis de ambiente.

## <a name="combine-social-and-local-login-accounts"></a>Combinar as contas de logon local e social

Para concluir esta seção, você deve primeiro habilitar um provedor de autenticação externa. Consulte [Facebook, Google e a autenticação do provedor externo](xref:security/authentication/social/index).

Você pode combinar as contas locais e sociais clicando no link seu email. Na sequência a seguir, "RickAndMSFT@gmail.com" é criado como um logon local; no entanto, você pode criar a conta como um logon social primeiro, depois de adicionar um logon local.

![Aplicativo Web: RickAndMSFT@gmail.com usuário autenticado](accconfirm/_static/rick.png)

Clique no **gerenciar** link. Observe externo 0 (logons sociais) associada à conta.

![Gerenciar o modo de exibição](accconfirm/_static/manage.png)

Clique no link para outro serviço de logon e aceitar as solicitações do aplicativo. Na imagem a seguir, o Facebook é o provedor de autenticação externa:

![Gerenciar o modo de exibição de logons externos listando Facebook](accconfirm/_static/fb.png)

As duas contas foram combinadas. É possível fazer logon com a conta. Convém que os usuários adicionem contas locais, caso seu serviço de autenticação de logon social está inoperante ou mais provável perdeu o acesso à sua conta social.

## <a name="enable-account-confirmation-after-a-site-has-users"></a>Habilitar confirmação de conta após um site tem usuários

Habilitar confirmação de conta em um site com usuários bloqueia todos os usuários existentes. Os usuários estão bloqueados porque suas contas não são confirmadas. Para contornar o bloqueio de usuário existente, use uma das seguintes abordagens:

* Atualize o banco de dados para marcar todos os usuários existentes como sendo confirmada.
* Confirme se os usuários existentes. Por exemplo, envio em lote-emails com links de confirmação.

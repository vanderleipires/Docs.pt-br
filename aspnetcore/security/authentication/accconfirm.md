---
title: "Confirmação de conta e senha de recuperação no núcleo do ASP.NET"
author: rick-anderson
description: "Mostra como criar um aplicativo do ASP.NET Core com redefinição de senha e de confirmação de email."
ms.author: riande
manager: wpickett
ms.date: 12/1/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/accconfirm
ms.openlocfilehash: bc9febc41d0637be9f83a02799d360489f257849
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a>Confirmação de conta e de recuperação de senha no núcleo do ASP.NET

Por [Rick Anderson](https://twitter.com/RickAndMSFT) e [Joe Audette](https://twitter.com/joeaudette) 

Este tutorial mostra como criar um aplicativo do ASP.NET Core com redefinição de senha e de confirmação de email.

## <a name="create-a-new-aspnet-core-project"></a>Criar um novo projeto ASP.NET Core

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Essa etapa se aplica ao Visual Studio no Windows. Consulte a próxima seção para obter instruções de CLI.

O tutorial requer o Visual Studio 2017 Preview 2 ou posterior.

* No Visual Studio, crie um novo projeto de aplicativo Web.
* Selecione **Core ASP.NET 2.0**. A imagem a seguir mostram **.NET Core** selecionado, mas você pode selecionar **do .NET Framework**.
* Selecione **alterar autenticação** e definido como **contas de usuário individuais**.
* Mantenha o padrão **no aplicativo de contas de usuário do repositório**.

![Nova caixa de diálogo do projeto mostrando "Opção de contas de usuário individuais" selecionada](accconfirm/_static/2.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

O tutorial requer o Visual Studio 2017 ou posterior.

* No Visual Studio, crie um novo projeto de aplicativo Web.
* Selecione **alterar autenticação** e definido como **contas de usuário individuais**.

![Nova caixa de diálogo do projeto mostrando "Opção de contas de usuário individuais" selecionada](accconfirm/_static/indiv.png)

---

### <a name="net-core-cli-project-creation-for-macos-and-linux"></a>Criação do projeto .NET core CLI para macOS e Linux

Se você estiver usando a CLI ou SQLite, execute o seguinte em uma janela de comando:

```console
dotnet new mvc --auth Individual
```

* `--auth Individual`Especifica o modelo de contas de usuário individuais.
* No Windows, adicione o `-uld` opção. O `-uld` opção cria uma cadeia de caracteres de conexão do LocalDB em vez de um banco de dados SQLite.
* Execute `new mvc --help` para obter ajuda sobre este comando.

## <a name="test-new-user-registration"></a>Testar o novo registro de usuário

Executar o aplicativo, selecione o **registrar** vincular e registrar um usuário. Siga as instruções para executar migrações de Entity Framework Core. Neste ponto, a validação somente do email é com o [[EmailAddress]](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) atributo. Depois de enviar o registro, você está conectado no aplicativo. Posteriormente no tutorial, vamos alterar isso para que novos usuários não podem fazer logon até que o email foi validado.

## <a name="view-the-identity-database"></a>Exibir o banco de dados de identidade

# <a name="sql-servertabsql-server"></a>[SQL Server](#tab/sql-server)

* Do **exibição** menu, selecione **Pesquisador de objetos do SQL Server** (SSOX). 
* Navegue até **(localdb) MSSQLLocalDB (SQL Server 13)**. Clique duas vezes em **dbo. AspNetUsers** > **exibir dados**:

![Menu de contexto no AspNetUsers tabela no Pesquisador de objetos do SQL Server](accconfirm/_static/ssox.png)

Observe o `EmailConfirmed` campo é `False`.

Você talvez queira usar o email novamente na próxima etapa, quando o aplicativo envia um email de confirmação. Clique na linha e selecione **excluir**. Excluir o email alias agora facilitará nas etapas a seguir.

# <a name="sqlitetabsqlite"></a>[SQLite](#tab/sqlite)

Consulte [trabalhando com SQLite em um projeto MVC do ASP.NET Core](xref:tutorials/first-mvc-app-xplat/working-with-sql) para obter instruções sobre como exibir o banco de dados SQLite. 

---

## <a name="require-ssl-and-setup-iis-express-for-ssl"></a>Exigir SSL e configurar o IIS Express para SSL

Consulte [impondo SSL](xref:security/enforcing-ssl).

<a name="prevent-login-at-registration"></a>
## <a name="require-email-confirmation"></a>Solicitar confirmação de email

É uma prática recomendada para confirmar o email de um novo registro de usuário para verificar se eles não estiver representando outra pessoa (ou seja, eles ainda não registrados com outra pessoa email). Suponha que você tivesse um fórum de discussão, e quiser impedir "yli@example.com"do registro como"nolivetto@contoso.com." Sem confirmação por email, "nolivetto@contoso.com" poderia obter indesejadas de seu aplicativo. Suponha que o usuário registrado acidentalmente como "ylo@example.com" e ainda não tenha notado o erro de "yli", elas não serão capazes de usar a recuperação de senha porque o aplicativo não tiver seu email correto. Email de confirmação oferece apenas proteção limitada de robôs e não oferece proteção contra spam determinado que têm muitos aliases de email de trabalho, que eles podem usar para registrar.

Em geral você deseja impedir que novos usuários lançamento todos os dados para seu site da web para que eles tenham um email confirmado. 

Atualização `ConfigureServices` para exigir um email confirmado:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](accconfirm/sample/WebPW/Startup.cs?name=snippet1&highlight=6-9)]


# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=13-16)]

---

 
```csharp
config.SignIn.RequireConfirmedEmail = true;
```
A linha precedente impede que usuários registrados que está sendo conectado até que o email foi confirmado. No entanto, essa linha não impede que novos usuários que está sendo conectado depois que eles se registrar. O código padrão registra em um usuário depois que eles se registrar. Depois que eles fazer logoff, eles não será capazes de fazer logon novamente até que eles se registrar. Posteriormente no tutorial, vamos alterar o usuário de código registrado para recentemente são **não** conectado.

### <a name="configure-email-provider"></a>Configurar o provedor de email

Neste tutorial, SendGrid é usada para enviar email. Você precisa de uma conta do SendGrid e a chave para enviar email. Você pode usar outros provedores de email. ASP.NET Core 2. x inclui `System.Net.Mail`, que permite enviar email de seu aplicativo. É recomendável que usar o SendGrid ou outro serviço de email para enviar email.

O [padrão de opções](xref:fundamentals/configuration/options) é usado para acessar as configurações de conta e a chave de usuário. Para obter mais informações, consulte [configuração](xref:fundamentals/configuration/index).

Crie uma classe para obter a chave de email seguro. Para este exemplo, o `AuthMessageSenderOptions` classe é criada no *Services/AuthMessageSenderOptions.cs* arquivo.

[!code-csharp[Main](accconfirm/sample/WebApp1/Services/AuthMessageSenderOptions.cs?name=snippet1)]

Definir o `SendGridUser` e `SendGridKey` com o [ferramenta Gerenciador de segredo](../app-secrets.md). Por exemplo:

```none
C:\WebAppl\src\WebApp1>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

No Windows, o segredo Manager armazena seus pares de chaves/valor em uma *secrets.json* arquivo no diretório %APPDATA%/Microsoft/UserSecrets/ < WebAppName userSecretsId >.

O conteúdo do *secrets.json* arquivo não são criptografados. O *secrets.json* arquivo é mostrado a seguir (o `SendGridKey` valor foi removido.)

  ```json
  {
    "SendGridUser": "RickAndMSFT",
    "SendGridKey": "<key removed>"
  }
  ```

### <a name="configure-startup-to-use-authmessagesenderoptions"></a>Configurar a inicialização para usar AuthMessageSenderOptions

Adicionar `AuthMessageSenderOptions` ao contêiner de serviço no final o `ConfigureServices` método o *Startup.cs* arquivo:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](accconfirm/sample/WebPW/Startup.cs?name=snippet1&highlight=18)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)
[!code-csharp[Main](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=26)]

---

### <a name="configure-the-authmessagesender-class"></a>Configurar a classe AuthMessageSender

Este tutorial mostra como adicionar notificações de email por meio de [SendGrid](https://sendgrid.com/), mas você pode enviar emails usando SMTP e outros mecanismos.

* Instalar o `SendGrid` pacote NuGet. No Console do Gerenciador de pacotes, digite o seguinte comando a seguir:

  `Install-Package SendGrid`

* Consulte [comece com SendGrid gratuitamente](https://sendgrid.com/free/) para registrar-se para uma conta gratuita do SendGrid.

#### <a name="configure-sendgrid"></a>Configurar o SendGrid

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

* Adicione código *Services/EmailSender.cs* semelhante à seguinte para configurar o SendGrid:

[!code-csharp[Main](accconfirm/sample/WebPW/Services/EmailSender.cs)]


# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)
* Adicione código *Services/MessageServices.cs* semelhante à seguinte para configurar o SendGrid:

[!code-csharp[Main](accconfirm/sample/WebApp1/Services/MessageServices.cs)]

---

## <a name="enable-account-confirmation-and-password-recovery"></a>Habilitar a recuperação de confirmação e a senha da conta

O modelo tem o código de recuperação de confirmação e a senha da conta. Localizar o `[HttpPost] Register` método o *AccountController.cs* arquivo.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Impedi que usuários recém-registrados sendo registrados automaticamente pelo comentar a seguinte linha:

```csharp 
await _signInManager.SignInAsync(user, isPersistent: false);
```

O método complete é mostrado com a linha alterada realçada:

[!code-csharp[Main](accconfirm/sample/WebPW/Controllers/AccountController.cs?highlight=19&name=snippet_Register)]

Observação: O código anterior falhará se você implementar `IEmailSender` e enviar um email de texto sem formatação. Consulte [esse problema](https://github.com/aspnet/Home/issues/2152) para obter mais informações e uma solução alternativa.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Descomente o código para habilitar a confirmação da conta.

[!code-csharp[Main](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=16-25&name=snippet_Register)]

Observação: É estiver também impedindo que um usuário registrado recentemente sendo registrados automaticamente pelo comentar a seguinte linha:

```csharp 
//await _signInManager.SignInAsync(user, isPersistent: false);
```

Habilitar a recuperação de senha por uncommenting o código de `ForgotPassword` ação no *Controllers/AccountController.cs* arquivo.

[!code-csharp[Main](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=17-23&name=snippet_ForgotPassword)]

Remova o elemento de formulário em *Views/Account/ForgotPassword.cshtml*. Talvez você queira remover o `<p> For more information on how to enable reset password ... </p>` elemento que contém um link para este artigo.

[!code-html[Main](accconfirm/sample/WebApp1/Views/Account/ForgotPassword.cshtml?highlight=7-10,12,28)]

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

![navbar](accconfirm/_static/x.png)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

A página de gerenciamento é exibida com o **perfil** guia selecionada. O **Email** mostra uma caixa de seleção que indica o email foi confirmada. 

![página Gerenciar](accconfirm/_static/rick2.png)


# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Falaremos sobre esta página no tutorial posteriormente.
![página Gerenciar](accconfirm/_static/rick2.png)

---

### <a name="test-password-reset"></a>Redefinição de senha do teste

* Se você estiver conectado, selecione **Logout**.  
* Selecione o **login** link e selecione o **esqueceu sua senha?** link.
* Insira o email que é usado para registrar a conta.
* Será enviado um email com um link para redefinir sua senha. Verifique seu email e clique no link para redefinir sua senha.  Depois que sua senha foi redefinida com êxito, você pode fazer logon com seu email e a nova senha.

<a name="debug"></a>

### <a name="debug-email"></a>Depurar email

Se você não pode receber email de trabalho:

* Examine o [atividade Email](https://sendgrid.com/docs/User_Guide/email_activity.html) página.
* Verifique a pasta de spam.
* Tente outro alias de email em um provedor de email diferente (Microsoft, Yahoo, Gmail, etc.)
* Criar um [aplicativo de console para enviar email](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html).
* Tente enviar para contas de email diferente.

**Observação:** uma prática recomendada é não usar segredos de produção no desenvolvimento e teste. Se você publicar o aplicativo no Azure, você pode definir os segredos do SendGrid como configurações de aplicativo no portal do aplicativo Web do Azure. O sistema de configuração está configurado para ler as chaves de variáveis de ambiente.

## <a name="prevent-login-at-registration"></a>Impedir que o logon no momento do registro

Com os modelos atuais, depois que o usuário concluir o formulário de registro, estão conectados (autenticados). Você geralmente deseja confirmar seu email antes de registrá-los. A seção a seguir, vamos modificar o código para exigir que novos usuários possuem um email confirmado antes que ele estão conectados. Atualização de `[HttpPost] Login` ação no *AccountController.cs* arquivo com as seguintes alterações realçados.

[!code-csharp[Main](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=11-21&name=snippet_Login)]

**Observação:** uma prática recomendada é não usar segredos de produção no desenvolvimento e teste. Se você publicar o aplicativo no Azure, você pode definir os segredos do SendGrid como configurações de aplicativo no portal do aplicativo Web do Azure. O sistema de configuração está configurado para ler as chaves de variáveis de ambiente.


## <a name="combine-social-and-local-login-accounts"></a>Combinar as contas de logon local e social

Observação: Esta seção se aplica apenas ao ASP.NET Core 1. x. Para o ASP.NET Core 2. x, consulte [isso](https://github.com/aspnet/Docs/issues/3753) problema.

Para concluir esta seção, você deve primeiro habilitar um provedor de autenticação externa. Consulte [habilitando a autenticação usando o Facebook, Google e outros provedores externos](social/index.md).

Você pode combinar as contas locais e sociais clicando no link seu email. Na sequência a seguir, "RickAndMSFT@gmail.com" é criado como um logon local; no entanto, você pode criar a conta como um logon social primeiro, depois de adicionar um logon local.

![Aplicativo Web: RickAndMSFT@gmail.com usuário autenticado](accconfirm/_static/rick.png)

Clique no **gerenciar** link. Observe externo 0 (logons sociais) associada à conta.

![Gerenciar o modo de exibição](accconfirm/_static/manage.png)

Clique no link para outro serviço de logon e aceitar as solicitações do aplicativo. Na imagem abaixo, o Facebook é o provedor de autenticação externa:

![Gerenciar o modo de exibição de logons externos listando Facebook](accconfirm/_static/fb.png)

As duas contas foram combinadas. Você poderá fazer logon com a conta. Convém que os usuários adicionem contas locais no caso de seu log social no serviço de autenticação está inoperante ou mais provável perdeu o acesso à sua conta social.

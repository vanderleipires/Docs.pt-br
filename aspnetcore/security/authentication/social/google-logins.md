---
title: "Configuração de logon externo do Google no núcleo do ASP.NET"
author: rick-anderson
description: "Este tutorial demonstra a integração da autenticação de usuário de conta do Google em um aplicativo existente do ASP.NET Core."
manager: wpickett
ms.author: riande
ms.date: 08/02/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/google-logins
ms.openlocfilehash: 1ca63593a7cf2b0eff1e52c0beda7ef2b826d474
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/30/2018
---
# <a name="configuring-google-authentication-in-aspnet-core"></a>Configurando a autenticação do Google no núcleo do ASP.NET

Por [Valeriy Novytskyy](https://github.com/01binary) e [Rick Anderson](https://twitter.com/RickAndMSFT)

Este tutorial mostra como habilitar os usuários entrar com sua conta Google + usando um projeto do ASP.NET Core 2.0 de exemplo criado no [página anterior](index.md). Vamos começar seguindo o [oficiais etapas](https://developers.google.com/identity/sign-in/web/devconsole-project) para criar um novo aplicativo no Console de API do Google.

## <a name="create-the-app-in-google-api-console"></a>Criar o aplicativo no Console de API do Google

* Navegue até [https://console.developers.google.com/projectselector/apis/library](https://console.developers.google.com/projectselector/apis/library) e entrar. Se você ainda não tiver uma conta do Google, use **mais opções** > **[criar conta](https://accounts.google.com/SignUpWithoutGmail?service=cloudconsole&continue=https%3A%2F%2Fconsole.developers.google.com%2Fprojectselector%2Fapis%2Flibrary&ltmpl=api)**  link para criar um:

![Console de API do Google](index/_static/GoogleConsoleLogin.png)

* Você será redirecionado para **biblioteca API Manager** página:

![Página Gerenciador de API de biblioteca](index/_static/GoogleConsoleSwitchboard.png)

* Toque em **criar** e insira seu **nome do projeto**:

![Caixa de diálogo Novo Projeto](index/_static/GoogleConsoleNewProj.png)

* Após aceitar a caixa de diálogo, você será redirecionado para a página de biblioteca, permitindo que você escolha os recursos para seu novo aplicativo. Localizar **API do Google +** na lista e clique em seu link para adicionar o recurso de API:

![Página Gerenciador de API de biblioteca](index/_static/GoogleConsoleChooseApi.png)

* A página para a API recém-adicionado é exibida. Toque em **habilitar** para adicionar o Google + sinal de recurso ao seu aplicativo:

![Página Gerenciador de API do Google + API](index/_static/GoogleConsoleEnableApi.png)

* Depois de habilitar a API, toque em **criar credenciais** para configurar os segredos:

![Página Gerenciador de API do Google + API](index/_static/GoogleConsoleGoCredentials.png)

* Escolha:
   * **API do Google +**
   * **Servidor Web (por exemplo, node.js, Tomcat)**, e
   * **Dados de usuário**:

![Página de credenciais de Gerenciador de API: descobrir quais tipos de credenciais que você precisa de painel](index/_static/GoogleConsoleChooseCred.png)

* Toque em **as credenciais que eu preciso?** que leva você para a segunda etapa de configuração de aplicativo, **criar uma ID de cliente OAuth 2.0**:

![Página de credenciais de Gerenciador de API: Crie uma ID de cliente OAuth 2.0](index/_static/GoogleConsoleCreateClient.png)

* Porque estamos criando um projeto do Google + com apenas um recurso (entrada), podemos inserir a mesma **nome** para a ID do cliente OAuth 2.0 é usado para o projeto.

* Insira o URI de desenvolvimento com */signin-google* acrescentados no **autorizados redirecionar URIs** campo (por exemplo: `https://localhost:44320/signin-google`). A autenticação do Google configurada mais tarde neste tutorial automaticamente manipulará as solicitações no */signin-google* rota para implementar o fluxo do OAuth.

* Pressione TAB para adicionar o **autorizados redirecionar URIs** entrada.

* Toque em **criar ID do cliente**, que leva você para a terceira etapa, **configurar a tela de consentimento de OAuth 2.0**:

![Página de credenciais de Gerenciador de API: configurar a tela de consentimento de OAuth 2.0](index/_static/GoogleConsoleAddCred.png)

* Insira seu voltado ao público **endereço de Email** e **nome do produto** mostrado para seu aplicativo quando Google + solicita ao usuário para entrar. Opções adicionais estão disponíveis em **mais opções de personalização**.

* Toque em **continuar** para prosseguir para a última etapa, **baixar credenciais**:

![Página de credenciais de Gerenciador de API: baixar credenciais](index/_static/GoogleConsoleFinish.png)

* Toque em **baixar** para salvar um arquivo JSON com segredos do aplicativo, e **feito** para concluir a criação do novo aplicativo.

* Ao implantar o site será necessário rever o **Google Console** e registre uma nova url pública.

## <a name="store-google-clientid-and-clientsecret"></a>Loja Google ClientID e ClientSecret

Vincular as configurações confidenciais com o Google `Client ID` e `Client Secret` para sua configuração de aplicativo usando o [Manager segredo](../../app-secrets.md). Para os fins deste tutorial, nomeie os tokens `Authentication:Google:ClientId` e `Authentication:Google:ClientSecret`.

Os valores para esses tokens podem ser encontrados no arquivo JSON baixado na etapa anterior em `web.client_id` e `web.client_secret`.

## <a name="configure-google-authentication"></a>Configurar a autenticação do Google

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Adicione o serviço do Google no `ConfigureServices` método *Startup.cs* arquivo:

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

services.AddAuthentication().AddGoogle(googleOptions =>
{
    googleOptions.ClientId = Configuration["Authentication:Google:ClientId"];
    googleOptions.ClientSecret = Configuration["Authentication:Google:ClientSecret"];
});
```

[!INCLUDE[default settings configuration](includes/default-settings.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

O modelo de projeto usado neste tutorial garante que [Microsoft.AspNetCore.Authentication.Google](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Google) pacote está instalado.

 * Para instalar este pacote com 2017 do Visual Studio, clique com botão direito no projeto e selecione **gerenciar pacotes NuGet**.
 * Para instalar o .NET Core CLI, execute o seguinte no diretório do projeto:

   `dotnet add package Microsoft.AspNetCore.Authentication.Google`

Adicionar o middleware do Google no `Configure` método *Startup.cs* arquivo:

```csharp
app.UseGoogleAuthentication(new GoogleOptions()
{
    ClientId = Configuration["Authentication:Google:ClientId"],
    ClientSecret = Configuration["Authentication:Google:ClientSecret"]
});
```

---

Consulte o [GoogleOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.googleoptions) referência de API para obter mais informações sobre opções de configuração com suporte pela autenticação do Google. Isso pode ser usado para solicitar informações diferentes sobre o usuário.

## <a name="sign-in-with-google"></a>Entrar com o Google

Execute o aplicativo e clique em **login**. Uma opção para entrar com o Google aparece:

![Aplicativo Web em execução no Microsoft Edge: usuário não autenticado](index/_static/DoneGoogle.png)

Quando você clica no Google, você é redirecionado para o Google para autenticação:

![Diálogo de autenticação do Google](index/_static/GoogleLogin.png)

Depois de inserir suas credenciais do Google, em seguida, você será redirecionado para o site onde você pode definir seu email.

Agora você está conectado usando suas credenciais do Google:

![Aplicativo Web em execução no Microsoft Edge: usuário autenticado](index/_static/Done.png)

## <a name="troubleshooting"></a>Solução de problemas

* Se você receber um `403 (Forbidden)` página de erro do seu próprio aplicativo quando em execução no modo de desenvolvimento (ou interrupção no depurador com o mesmo erro), certifique-se de que **API do Google +** foi habilitada no **API do Gerenciador de biblioteca** seguindo as etapas listadas [anteriores nesta página](#create-the-app-in-google-api-console). Se a entrada não funciona e não estiver obtendo erros, alterne para o modo de desenvolvimento para facilitar o problema depurar.
* **ASP.NET Core 2. x somente:** identidade se não estiver configurada, chamando `services.AddIdentity` na `ConfigureServices`, tentar autenticar resultará em *ArgumentException: A opção 'SignInScheme' deve ser fornecida*. O modelo de projeto usado neste tutorial garante que isso é feito.
* Se o banco de dados do site não tiver sido criado, aplicando a migração inicial, você obterá *uma operação de banco de dados falhou ao processar a solicitação* erro. Toque em **aplicar migrações** para criar o banco de dados e a atualização para continuar após o erro.

## <a name="next-steps"></a>Próximas etapas

* Este artigo mostrou como você pode autenticar com o Google. Você pode seguir uma abordagem semelhante para autenticar com outros provedores listados no [página anterior](index.md).

* Depois de publicar seu site da web para o aplicativo web do Azure, você deve redefinir o `ClientSecret` no Console de API do Google.

* Definir o `Authentication:Google:ClientId` e `Authentication:Google:ClientSecret` como configurações de aplicativo no portal do Azure. O sistema de configuração é configurado para ler as chaves de variáveis de ambiente.

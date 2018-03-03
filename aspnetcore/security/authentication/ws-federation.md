---
title: "Autenticar usuários com o WS-Federation no núcleo do ASP.NET"
author: chlowell
description: Este tutorial demonstra como usar o WS-Federation em um aplicativo do ASP.NET Core.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 02/27/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/ws-federation
ms.openlocfilehash: 0532f866e9c58b2e45623f522f62438e15017e54
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/02/2018
---
# <a name="authenticate-users-with-ws-federation-in-aspnet-core"></a>Autenticar usuários com o WS-Federation no núcleo do ASP.NET

Este tutorial demonstra como habilitar usuários entrar com um provedor de autenticação do WS-Federation como o Active Directory Federation Services (ADFS) ou [Active Directory do Azure](/azure/active-directory/) (AAD). Ele usa o aplicativo de exemplo do ASP.NET Core 2.0 descrito em [Facebook, Google e a autenticação do provedor externo](xref:security/authentication/social/index).

Para aplicativos do ASP.NET Core 2.0, o suporte do WS-Federation é fornecido por [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation). Esse componente é movido de [Microsoft.Owin.Security.WsFederation](https://www.nuget.org/packages/Microsoft.Owin.Security.WsFederation) e compartilha muitas das mecânica do componente. No entanto, os componentes são diferentes de duas maneiras importantes.

Por padrão, o middleware novo:

* Não permitir logons não solicitados. Esse recurso do protocolo WS-Federation é vulnerável a ataques XSRF. No entanto, ela pode ser habilitada com o `AllowUnsolicitedLogins` opção.
* Não verifica cada postagem de formulário para mensagens de entrada. Somente as solicitações para o `CallbackPath` são verificados quanto à entrada-ins `CallbackPath` padrão é `/signin-wsfed` , mas pode ser alterado. Esse caminho pode ser compartilhado com outros provedores de autenticação, permitindo que o `SkipUnrecognizedRequests` opção.

## <a name="register-the-app-with-active-directory"></a>Registrar o aplicativo com o Active Directory

### <a name="active-directory-federation-services"></a>Serviços de Federação do Active Directory

* Abra o servidor **terceira parte confiável Assistente para adicionar confiança** no console de gerenciamento do AD FS:

![Adicionar Assistente de confiança de terceira parte confiável: Bem-vindo](ws-federation/_static/AdfsAddTrust.png)

* Escolha esta opção para inserir os dados manualmente:

![Adicionar Assistente de terceira parte confiável: Selecionar fonte de dados](ws-federation/_static/AdfsSelectDataSource.png)

* Insira um nome para exibição para a terceira parte confiável. O nome não é importante para o aplicativo ASP.NET Core.

* [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) não tem suporte para criptografia de token, portanto, não configurar um certificado de criptografia do token:

![Adicionar Assistente de terceira parte confiável: Configurar o certificado](ws-federation/_static/AdfsConfigureCert.png)

* Habilite o suporte para o protocolo WS-Federation Passive, usando a URL do aplicativo. Verifique se que a porta está correta para o aplicativo:

![Adicionar Assistente de terceira parte confiável: Configurar a URL](ws-federation/_static/AdfsConfigureUrl.png)

> [!NOTE]
> Isso deve ser uma URL HTTPS. O IIS Express pode fornecer um certificado autoassinado ao hospedar o aplicativo durante o desenvolvimento. Kestrel requer configuração manual do certificado. Consulte o [documentação Kestrel](xref:fundamentals/servers/kestrel) para obter mais detalhes.

* Clique em **próximo** através do restante do assistente e **fechar** no final.

* A identidade do ASP.NET Core requer um **ID de nome** de declaração. Adicione um do **editar regras de declaração** caixa de diálogo:

![Editar regras de declaração](ws-federation/_static/EditClaimRules.png)

* No **Adicionar Assistente de regra de declaração de transformação**, deixe o padrão **enviar atributos LDAP como declarações** modelo selecionado e clique em **próximo**. Adicionar um mapeamento de regra a **nome de conta SAM** atributo LDAP para o **ID de nome** declaração de saída:

![Adicionar Assistente de regra de declaração de transformação: Configurar a regra de declaração](ws-federation/_static/AddTransformClaimRule.png)

* Clique em **concluir** > **Okey** no **editar regras de declaração** janela.

### <a name="azure-active-directory"></a>Azure Active Directory

* Navegue até a folha de registros de aplicativo do locatário do AAD. Clique em **novo registro de aplicativo**:

![Active Directory do Azure: Registros de aplicativo](ws-federation/_static/AadNewAppRegistration.png)

* Insira um nome para o registro do aplicativo. Isso não é importante para o aplicativo ASP.NET Core.
* Insira a URL do aplicativo escuta em que o **URL de logon**:

![Active Directory do Azure: Crie um registro de aplicativo](ws-federation/_static/AadCreateAppRegistration.png)

* Clique em **pontos de extremidade** e observe o **documento de metadados de Federação** URL. Este é o middleware de WS-Federation `MetadataAddress`:

![Active Directory do Azure: os pontos de extremidade](ws-federation/_static/AadFederationMetadataDocument.png)

* Navegue até o novo registro de aplicativo. Clique em **configurações** > **propriedades** e anote o **URI da ID do aplicativo**. Este é o middleware de WS-Federation `Wtrealm`:

![Active Directory do Azure: Propriedades de registro de aplicativo](ws-federation/_static/AadAppIdUri.png)

## <a name="add-ws-federation-as-an-external-login-provider-for-aspnet-core-identity"></a>Adicionar o WS-Federation como um provedor de logon externo para a identidade do ASP.NET Core

* Adicione uma dependência no [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) ao projeto.
* Adicionar o WS-Federation para o `Configure` método *Startup.cs*:

    ```csharp
    services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

    services.AddAuthentication()
        .AddWsFederation(options =>
        {
            // MetadataAddress represents the Active Directory instance used to authenticate users.
            options.MetadataAddress = "https://<ADFS FQDN or AAD tenant>/FederationMetadata/2007-06/FederationMetadata.xml";

            // Wtrealm is the app's identifier in the Active Directory instance.
            // For ADFS, use the relying party's identifier, its WS-Federation Passive protocol URL:
            options.Wtrealm = "https://localhost:44307/";

            // For AAD, use the App ID URI from the app registration's Properties blade:
            options.Wtrealm = "https://wsfedsample.onmicrosoft.com/bf0e7e6d-056e-4e37-b9a6-2c36797b9f01";
        });

    services.AddMvc()
     // ...
    ```

[!INCLUDE[default settings configuration](social/includes/default-settings.md)]

### <a name="log-in-with-ws-federation"></a>Faça logon com o WS-Federation

Navegue até o aplicativo e clique no **login** link no cabeçalho de navegação. Há uma opção para fazer logon com WsFederation: ![página de logon](ws-federation/_static/WsFederationButton.png)

Com o ADFS como o provedor, o botão redireciona para uma página de entrada do AD FS: ![página de entrada do AD FS](ws-federation/_static/AdfsLoginPage.png)

Com o Azure Active Directory como o provedor, o botão redireciona para uma página de entrada do AAD: ![página de entrada do AAD](ws-federation/_static/AadSignIn.png)

Um bem-sucedida entrar para um novo usuário redireciona para a página de registro de usuário do aplicativo: ![página de registro](ws-federation/_static/Register.png)

## <a name="use-ws-federation-without-aspnet-core-identity"></a>Use o WS-Federation sem identidade do ASP.NET Core

O middleware de WS-Federation pode ser usado sem identidade. Por exemplo:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddAuthentication(sharedOptions =>
    {
        sharedOptions.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
        sharedOptions.DefaultSignInScheme = CookieAuthenticationDefaults.AuthenticationScheme;
        sharedOptions.DefaultChallengeScheme = WsFederationDefaults.AuthenticationScheme;
    })
    .AddWsFederation(options =>
    {
        options.Wtrealm = Configuration["wsfed:realm"];
        options.MetadataAddress = Configuration["wsfed:metadata"];
    })
    .AddCookie();
}

public void Configure(IApplicationBuilder app)
{
    app.UseAuthentication();
        // …
}
```

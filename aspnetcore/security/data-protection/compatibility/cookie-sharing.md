---
title: Compartilhamento de cookies entre aplicativos
author: rick-anderson
description: "Este documento explica como a pilha de proteção de dados oferece suporte ao compartilhamento de cookies de autenticação entre o ASP.NET 4. x e aplicativos ASP.NET Core."
keywords: ASP.NET Core,ASP.NET,cookies,Interop,cookie compartilhamento
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 9a7aae45-8e21-4c54-950c-3c29df6c1082
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/compatibility/cookie-sharing
ms.openlocfilehash: e92cc81f9362787b7b4bfb44ba26b82182826a59
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
# <a name="sharing-cookies-between-applications"></a>Compartilhamento de cookies entre aplicativos

Sites da Web geralmente consistem em muitos aplicativos web individuais, todo trabalho juntos harmoniously. Se quiser que um desenvolvedor de aplicativos proporcionar uma boa experiência de logon único, eles geralmente precisará todos os aplicativos web diferente dentro do site para compartilhar os tíquetes de autenticação entre si.

Para dar suporte a esse cenário, a pilha de proteção de dados permite o compartilhamento de autenticação de cookie Katana e permissões de autenticação de cookie do ASP.NET Core.

## <a name="sharing-authentication-cookies-between-applications"></a>Cookies de autenticação de compartilhamento entre aplicativos

Para compartilhar os cookies de autenticação entre dois aplicativos diferentes do ASP.NET Core, configure cada aplicativo que deve compartilhar cookies da seguinte maneira.

No seu configurar método, use o CookieAuthenticationOptions para configurar o serviço de proteção de dados para cookies e o AuthenticationScheme para coincidir com o ASP.NET 4. x.

Se você estiver usando a identidade:

```csharp
app.AddIdentity<ApplicationUser, IdentityRole>(options =>
{
    options.Cookies.ApplicationCookie.AuthenticationScheme = "ApplicationCookie";
    var protectionProvider = DataProtectionProvider.Create(new DirectoryInfo(@"c:\shared-auth-ticket-keys\"));
    options.Cookies.ApplicationCookie.DataProtectionProvider = protectionProvider;
    options.Cookies.ApplicationCookie.TicketDataFormat = new TicketDataFormat(protectionProvider.CreateProtector("Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationMiddleware", "Cookies", "v2"));
});
```

Se você estiver usando cookies diretamente:

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    DataProtectionProvider = DataProtectionProvider.Create(new DirectoryInfo(@"c:\shared-auth-ticket-keys\"))
});
```
   
O `DataProtectionProvider` requer o `Microsoft.AspNetCore.DataProtection.Extensions` pacote NuGet.

Quando usado dessa maneira, o DirectoryInfo deve apontar para um local de armazenamento de chaves, especificamente, separe os cookies de autenticação. O middleware de autenticação de cookie usarão a implementação fornecida explicitamente de DataProtectionProvider, que agora é isolado do sistema de proteção de dados usado por outras partes do aplicativo. O nome do aplicativo será ignorado (intencionalmente caso, já que você está tentando obter vários aplicativos compartilhem cargas).

>[!WARNING]
>Considere configurar o DataProtectionProvider, que as chaves são criptografadas em repouso, como no exemplo abaixo.
>
>
>  ```csharp
>  app.UseCookieAuthentication(new CookieAuthenticationOptions
>  {
>      DataProtectionProvider = DataProtectionProvider.Create(
>          new DirectoryInfo(@"c:\shared-auth-ticket-keys\"),
>          configure =>
>          {
>              configure.ProtectKeysWithCertificate("thumbprint");
>          })
>  });
>  ```

## <a name="sharing-authentication-cookies-between-aspnet-4x-and-aspnet-core-applications"></a>Compartilhamento de cookies de autenticação entre o ASP.NET 4. x e aplicativos ASP.NET Core

Aplicativos do ASP.NET 4. x que usam Katana middleware de autenticação de cookie podem ser configurados para gerar os cookies de autenticação que são compatíveis com o middleware de autenticação de cookie do ASP.NET Core. Isso permite atualizar aplicativos individuais de um grande site por etapas enquanto ainda fornecem um logon único a suave experiência em todo o site.

>[!TIP]
> Você pode saber se o seu aplicativo usa o middleware de autenticação de cookie Katana pela existência de uma chamada para UseCookieAuthentication em Startup.Auth.cs do seu projeto. Projetos de aplicativo web do ASP.NET 4. x criadas com o Visual Studio 2013 e depois, usar o middleware de autenticação de cookie Katana por padrão.

> [!NOTE]
> O aplicativo do ASP.NET 4. x deve ter como destino do .NET Framework 4.5.1 ou superior, caso contrário, os pacotes do NuGet necessário será instalado.

Para compartilhar os cookies de autenticação entre aplicativos ASP.NET 4. x e os aplicativos do ASP.NET Core, configure o aplicativo do ASP.NET Core, conforme indicado acima e configurar aplicativos ASP.NET 4. x, seguindo as etapas abaixo.

1.  Instale o pacote Microsoft.Owin.Security.Interop em cada um dos aplicativos ASP.NET 4. x.

2.   No Startup.Auth.cs, localize a chamada para UseCookieAuthentication, que geralmente se parecerá com o seguinte.

    ```csharp
    app.UseCookieAuthentication(new CookieAuthenticationOptions
    {
        // ...
    });
    ```
    
3.  Modifique a chamada para UseCookieAuthentication da seguinte maneira, alterando o CookieName para corresponder ao nome usado pelo middleware de autenticação de cookie ASP.NET Core, fornecendo uma instância de um DataProtectionProvider que foi inicializado para um local de armazenamento de chaves, e Defina CookieManager como ChunkingCookieManager de interoperabilidade para que o formato das partes seja compatível.

    ```csharp
    app.UseCookieAuthentication(new CookieAuthenticationOptions
    {
        AuthenticationType = DefaultAuthenticationTypes.ApplicationCookie,
        CookieName = ".AspNetCore.Cookies",
        // CookieName = ".AspNetCore.ApplicationCookie", (if you're using identity)
        // CookiePath = "...", (if necessary)
        // ...
        TicketDataFormat = new AspNetTicketDataFormat(
            new DataProtectorShim(
                DataProtectionProvider.Create(new DirectoryInfo(@"c:\shared-auth-ticket-keys\"))
                .CreateProtector("Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationMiddleware",
                "Cookies", "v2"))),
        CookieManager = new ChunkingCookieManager()
    });
    ```
    O DirectoryInfo deve apontar para o mesmo local de armazenamento que você apontada seu aplicativo ASP.NET Core e deve ser configurados usando as mesmas configurações.

O ASP.NET 4. x e aplicativos ASP.NET Core agora estão configurados para compartilhar os cookies de autenticação.

> [!NOTE]
> Você precisará certificar-se de que o sistema de identidade para cada aplicativo é apontado para o mesmo banco de dados do usuário. Caso contrário, o sistema de identidade produzirá falhas em tempo de execução quando ele tenta corresponder as informações no cookie de autenticação com as informações no banco de dados.

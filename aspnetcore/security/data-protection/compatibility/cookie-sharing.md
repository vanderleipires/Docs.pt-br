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
# <a name="sharing-cookies-between-applications"></a><span data-ttu-id="5acd2-104">Compartilhamento de cookies entre aplicativos</span><span class="sxs-lookup"><span data-stu-id="5acd2-104">Sharing cookies between applications</span></span>

<span data-ttu-id="5acd2-105">Sites da Web geralmente consistem em muitos aplicativos web individuais, todo trabalho juntos harmoniously.</span><span class="sxs-lookup"><span data-stu-id="5acd2-105">Web sites commonly consist of many individual web applications, all working together harmoniously.</span></span> <span data-ttu-id="5acd2-106">Se quiser que um desenvolvedor de aplicativos proporcionar uma boa experiência de logon único, eles geralmente precisará todos os aplicativos web diferente dentro do site para compartilhar os tíquetes de autenticação entre si.</span><span class="sxs-lookup"><span data-stu-id="5acd2-106">If an application developer wants to provide a good single-sign-on experience, they'll often need all of the different web applications within the site to share authentication tickets between each other.</span></span>

<span data-ttu-id="5acd2-107">Para dar suporte a esse cenário, a pilha de proteção de dados permite o compartilhamento de autenticação de cookie Katana e permissões de autenticação de cookie do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5acd2-107">To support this scenario, the data protection stack allows sharing Katana cookie authentication and ASP.NET Core cookie authentication tickets.</span></span>

## <a name="sharing-authentication-cookies-between-applications"></a><span data-ttu-id="5acd2-108">Cookies de autenticação de compartilhamento entre aplicativos</span><span class="sxs-lookup"><span data-stu-id="5acd2-108">Sharing authentication cookies between applications</span></span>

<span data-ttu-id="5acd2-109">Para compartilhar os cookies de autenticação entre dois aplicativos diferentes do ASP.NET Core, configure cada aplicativo que deve compartilhar cookies da seguinte maneira.</span><span class="sxs-lookup"><span data-stu-id="5acd2-109">To share authentication cookies between two different ASP.NET Core applications, configure each application that should share cookies as follows.</span></span>

<span data-ttu-id="5acd2-110">No seu configurar método, use o CookieAuthenticationOptions para configurar o serviço de proteção de dados para cookies e o AuthenticationScheme para coincidir com o ASP.NET 4. x.</span><span class="sxs-lookup"><span data-stu-id="5acd2-110">In your configure method, use the CookieAuthenticationOptions to set up the data protection service for cookies and the AuthenticationScheme to match ASP.NET 4.x.</span></span>

<span data-ttu-id="5acd2-111">Se você estiver usando a identidade:</span><span class="sxs-lookup"><span data-stu-id="5acd2-111">If you're using identity:</span></span>

```csharp
app.AddIdentity<ApplicationUser, IdentityRole>(options =>
{
    options.Cookies.ApplicationCookie.AuthenticationScheme = "ApplicationCookie";
    var protectionProvider = DataProtectionProvider.Create(new DirectoryInfo(@"c:\shared-auth-ticket-keys\"));
    options.Cookies.ApplicationCookie.DataProtectionProvider = protectionProvider;
    options.Cookies.ApplicationCookie.TicketDataFormat = new TicketDataFormat(protectionProvider.CreateProtector("Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationMiddleware", "Cookies", "v2"));
});
```

<span data-ttu-id="5acd2-112">Se você estiver usando cookies diretamente:</span><span class="sxs-lookup"><span data-stu-id="5acd2-112">If you're using cookies directly:</span></span>

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    DataProtectionProvider = DataProtectionProvider.Create(new DirectoryInfo(@"c:\shared-auth-ticket-keys\"))
});
```
   
<span data-ttu-id="5acd2-113">O `DataProtectionProvider` requer o `Microsoft.AspNetCore.DataProtection.Extensions` pacote NuGet.</span><span class="sxs-lookup"><span data-stu-id="5acd2-113">The `DataProtectionProvider` requires the `Microsoft.AspNetCore.DataProtection.Extensions` NuGet package.</span></span>

<span data-ttu-id="5acd2-114">Quando usado dessa maneira, o DirectoryInfo deve apontar para um local de armazenamento de chaves, especificamente, separe os cookies de autenticação.</span><span class="sxs-lookup"><span data-stu-id="5acd2-114">When used in this manner, the DirectoryInfo should point to a key storage location specifically set aside for authentication cookies.</span></span> <span data-ttu-id="5acd2-115">O middleware de autenticação de cookie usarão a implementação fornecida explicitamente de DataProtectionProvider, que agora é isolado do sistema de proteção de dados usado por outras partes do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="5acd2-115">The cookie authentication middleware will use the explicitly provided implementation of the DataProtectionProvider, which is now isolated from the data protection system used by other parts of the application.</span></span> <span data-ttu-id="5acd2-116">O nome do aplicativo será ignorado (intencionalmente caso, já que você está tentando obter vários aplicativos compartilhem cargas).</span><span class="sxs-lookup"><span data-stu-id="5acd2-116">The application name is ignored (intentionally so, since you're trying to get multiple applications to share payloads).</span></span>

>[!WARNING]
><span data-ttu-id="5acd2-117">Considere configurar o DataProtectionProvider, que as chaves são criptografadas em repouso, como no exemplo abaixo.</span><span class="sxs-lookup"><span data-stu-id="5acd2-117">You should consider configuring the DataProtectionProvider such that keys are encrypted at rest, as in the below example.</span></span>
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

## <a name="sharing-authentication-cookies-between-aspnet-4x-and-aspnet-core-applications"></a><span data-ttu-id="5acd2-118">Compartilhamento de cookies de autenticação entre o ASP.NET 4. x e aplicativos ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5acd2-118">Sharing authentication cookies between ASP.NET 4.x and ASP.NET Core applications</span></span>

<span data-ttu-id="5acd2-119">Aplicativos do ASP.NET 4. x que usam Katana middleware de autenticação de cookie podem ser configurados para gerar os cookies de autenticação que são compatíveis com o middleware de autenticação de cookie do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5acd2-119">ASP.NET 4.x applications which use Katana cookie authentication middleware can be configured to generate authentication cookies which are compatible with the ASP.NET Core cookie authentication middleware.</span></span> <span data-ttu-id="5acd2-120">Isso permite atualizar aplicativos individuais de um grande site por etapas enquanto ainda fornecem um logon único a suave experiência em todo o site.</span><span class="sxs-lookup"><span data-stu-id="5acd2-120">This allows upgrading a large site's individual applications piecemeal while still providing a smooth single sign on experience across the site.</span></span>

>[!TIP]
> <span data-ttu-id="5acd2-121">Você pode saber se o seu aplicativo usa o middleware de autenticação de cookie Katana pela existência de uma chamada para UseCookieAuthentication em Startup.Auth.cs do seu projeto.</span><span class="sxs-lookup"><span data-stu-id="5acd2-121">You can tell if your existing application uses Katana cookie authentication middleware by the existence of a call to UseCookieAuthentication in your project's Startup.Auth.cs.</span></span> <span data-ttu-id="5acd2-122">Projetos de aplicativo web do ASP.NET 4. x criadas com o Visual Studio 2013 e depois, usar o middleware de autenticação de cookie Katana por padrão.</span><span class="sxs-lookup"><span data-stu-id="5acd2-122">ASP.NET 4.x web application projects created with Visual Studio 2013 and later use the Katana cookie authentication middleware by default.</span></span>

> [!NOTE]
> <span data-ttu-id="5acd2-123">O aplicativo do ASP.NET 4. x deve ter como destino do .NET Framework 4.5.1 ou superior, caso contrário, os pacotes do NuGet necessário será instalado.</span><span class="sxs-lookup"><span data-stu-id="5acd2-123">Your ASP.NET 4.x application must target .NET Framework 4.5.1 or higher, otherwise the necessary NuGet packages will fail to install.</span></span>

<span data-ttu-id="5acd2-124">Para compartilhar os cookies de autenticação entre aplicativos ASP.NET 4. x e os aplicativos do ASP.NET Core, configure o aplicativo do ASP.NET Core, conforme indicado acima e configurar aplicativos ASP.NET 4. x, seguindo as etapas abaixo.</span><span class="sxs-lookup"><span data-stu-id="5acd2-124">To share authentication cookies between your ASP.NET 4.x applications and your ASP.NET Core applications, configure the ASP.NET Core application as stated above, then configure your ASP.NET 4.x applications by following the steps below.</span></span>

1.  <span data-ttu-id="5acd2-125">Instale o pacote Microsoft.Owin.Security.Interop em cada um dos aplicativos ASP.NET 4. x.</span><span class="sxs-lookup"><span data-stu-id="5acd2-125">Install the package Microsoft.Owin.Security.Interop into each of your ASP.NET 4.x applications.</span></span>

2.   <span data-ttu-id="5acd2-126">No Startup.Auth.cs, localize a chamada para UseCookieAuthentication, que geralmente se parecerá com o seguinte.</span><span class="sxs-lookup"><span data-stu-id="5acd2-126">In Startup.Auth.cs, locate the call to UseCookieAuthentication, which will generally look like the following.</span></span>

    ```csharp
    app.UseCookieAuthentication(new CookieAuthenticationOptions
    {
        // ...
    });
    ```
    
3.  <span data-ttu-id="5acd2-127">Modifique a chamada para UseCookieAuthentication da seguinte maneira, alterando o CookieName para corresponder ao nome usado pelo middleware de autenticação de cookie ASP.NET Core, fornecendo uma instância de um DataProtectionProvider que foi inicializado para um local de armazenamento de chaves, e Defina CookieManager como ChunkingCookieManager de interoperabilidade para que o formato das partes seja compatível.</span><span class="sxs-lookup"><span data-stu-id="5acd2-127">Modify the call to UseCookieAuthentication as follows, changing the CookieName to match the name used by the ASP.NET Core cookie authentication middleware, providing an instance of a DataProtectionProvider that has been initialized to a key storage location, and set CookieManager to interop ChunkingCookieManager so the chunking format is compatible.</span></span>

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
    <span data-ttu-id="5acd2-128">O DirectoryInfo deve apontar para o mesmo local de armazenamento que você apontada seu aplicativo ASP.NET Core e deve ser configurados usando as mesmas configurações.</span><span class="sxs-lookup"><span data-stu-id="5acd2-128">The DirectoryInfo has to point to the same storage location that you pointed your ASP.NET Core application to and should be configured using the same settings.</span></span>

<span data-ttu-id="5acd2-129">O ASP.NET 4. x e aplicativos ASP.NET Core agora estão configurados para compartilhar os cookies de autenticação.</span><span class="sxs-lookup"><span data-stu-id="5acd2-129">The ASP.NET 4.x and ASP.NET Core applications are now configured to share authentication cookies.</span></span>

> [!NOTE]
> <span data-ttu-id="5acd2-130">Você precisará certificar-se de que o sistema de identidade para cada aplicativo é apontado para o mesmo banco de dados do usuário.</span><span class="sxs-lookup"><span data-stu-id="5acd2-130">You'll need to make sure that the identity system for each application is pointed at the same user database.</span></span> <span data-ttu-id="5acd2-131">Caso contrário, o sistema de identidade produzirá falhas em tempo de execução quando ele tenta corresponder as informações no cookie de autenticação com as informações no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="5acd2-131">Otherwise the identity system will produce failures at runtime when it tries to match the information in the authentication cookie against the information in its database.</span></span>

---
title: "Configurar a autenticação do Windows no núcleo do ASP.NET"
author: ardalis
description: "Este artigo descreve como configurar a autenticação do Windows no ASP.NET Core, usando o IIS, IIS Express, o HTTP. sys e WebListener."
keywords: "ASP.NET Core, a autenticação do Windows, o atributo de autorização, o atributo de AllowAnonymous"
ms.author: riande
manager: wpickett
ms.date: 10/24/2017
ms.topic: article
ms.assetid: cf119f21-1a2b-49a2-b052-548ccb66ee83
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/windowsauth
ms.openlocfilehash: 703f924d049a267cb8bee22e63e6669b13099c53
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
# <a name="configure-windows-authentication-in-an-aspnet-core-app"></a><span data-ttu-id="4189d-104">Configurar a autenticação do Windows em um aplicativo do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4189d-104">Configure Windows authentication in an ASP.NET Core app</span></span>

<span data-ttu-id="4189d-105">Por [Steve Smith](https://ardalis.com) e [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="4189d-105">By [Steve Smith](https://ardalis.com) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="4189d-106">Autenticação do Windows pode ser configurada para aplicativos ASP.NET Core hospedados no IIS, [HTTP.sys](xref:fundamentals/servers/httpsys), ou [WebListener](xref:fundamentals/servers/weblistener).</span><span class="sxs-lookup"><span data-stu-id="4189d-106">Windows authentication can be configured for ASP.NET Core apps hosted with IIS, [HTTP.sys](xref:fundamentals/servers/httpsys), or [WebListener](xref:fundamentals/servers/weblistener).</span></span>

## <a name="what-is-windows-authentication"></a><span data-ttu-id="4189d-107">O que é autenticação do Windows?</span><span class="sxs-lookup"><span data-stu-id="4189d-107">What is Windows authentication?</span></span>

<span data-ttu-id="4189d-108">Autenticação do Windows se baseia no sistema operacional para autenticar os usuários de aplicativos do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4189d-108">Windows authentication relies on the operating system to authenticate users of ASP.NET Core apps.</span></span> <span data-ttu-id="4189d-109">Você pode usar a autenticação do Windows quando o servidor é executado em uma rede corporativa usando identidades do domínio do Active Directory ou outras contas do Windows para identificar os usuários.</span><span class="sxs-lookup"><span data-stu-id="4189d-109">You can use Windows authentication when your server runs on a corporate network using Active Directory domain identities or other Windows accounts to identify users.</span></span> <span data-ttu-id="4189d-110">Autenticação do Windows é mais adequada para ambientes de intranet em que os usuários, os aplicativos cliente e servidores web pertencem ao mesmo domínio do Windows.</span><span class="sxs-lookup"><span data-stu-id="4189d-110">Windows authentication is best suited to intranet environments in which users, client applications, and web servers belong to the same Windows domain.</span></span>

<span data-ttu-id="4189d-111">[Saiba mais sobre a autenticação do Windows e instalá-lo para o IIS](https://docs.microsoft.com/iis/configuration/system.webServer/security/authentication/windowsAuthentication/).</span><span class="sxs-lookup"><span data-stu-id="4189d-111">[Learn more about Windows authentication and installing it for IIS](https://docs.microsoft.com/iis/configuration/system.webServer/security/authentication/windowsAuthentication/).</span></span>

## <a name="enable-windows-authentication-in-an-aspnet-core-app"></a><span data-ttu-id="4189d-112">Habilitar a autenticação do Windows em um aplicativo do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4189d-112">Enable Windows authentication in an ASP.NET Core app</span></span>

<span data-ttu-id="4189d-113">O modelo de aplicativo de Web do Visual Studio pode ser configurado para dar suporte à autenticação do Windows.</span><span class="sxs-lookup"><span data-stu-id="4189d-113">The Visual Studio Web Application template can be configured to support Windows authentication.</span></span>

### <a name="use-the-windows-authentication-app-template"></a><span data-ttu-id="4189d-114">Use o modelo de aplicativo de autenticação do Windows</span><span class="sxs-lookup"><span data-stu-id="4189d-114">Use the Windows authentication app template</span></span>

<span data-ttu-id="4189d-115">No Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="4189d-115">In Visual Studio:</span></span>
1. <span data-ttu-id="4189d-116">Crie um novo Aplicativo Web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4189d-116">Create a new ASP.NET Core Web Application.</span></span> 
1. <span data-ttu-id="4189d-117">Selecione o aplicativo Web da lista de modelos.</span><span class="sxs-lookup"><span data-stu-id="4189d-117">Select Web Application from the list of templates.</span></span>
1. <span data-ttu-id="4189d-118">Selecione o **alterar autenticação** botão e selecione **autenticação do Windows**.</span><span class="sxs-lookup"><span data-stu-id="4189d-118">Select the **Change Authentication** button and select **Windows Authentication**.</span></span> 

<span data-ttu-id="4189d-119">Execute o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="4189d-119">Run the app.</span></span> <span data-ttu-id="4189d-120">O nome de usuário aparece no canto superior direito do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="4189d-120">The username appears in the top right of the app.</span></span>

![Tela de navegador de autenticação do Windows](windowsauth/_static/browser-screenshot.png)

<span data-ttu-id="4189d-122">Para o trabalho de desenvolvimento usando o IIS Express, o modelo fornece todas as configurações necessárias para usar a autenticação do Windows.</span><span class="sxs-lookup"><span data-stu-id="4189d-122">For development work using IIS Express, the template provides all the configuration necessary to use Windows authentication.</span></span> <span data-ttu-id="4189d-123">A seção a seguir mostra como configurar manualmente um aplicativo ASP.NET Core para autenticação do Windows.</span><span class="sxs-lookup"><span data-stu-id="4189d-123">The following section shows how to manually configure an ASP.NET Core app for Windows authentication.</span></span>

### <a name="visual-studio-settings-for-windows-and-anonymous-authentication"></a><span data-ttu-id="4189d-124">Configurações do Visual Studio para Windows e autenticação anônima</span><span class="sxs-lookup"><span data-stu-id="4189d-124">Visual Studio settings for Windows and anonymous authentication</span></span>

<span data-ttu-id="4189d-125">O projeto do Visual Studio **propriedades** da página **depurar** guia fornece caixas de seleção para autenticação anônima e autenticação do Windows.</span><span class="sxs-lookup"><span data-stu-id="4189d-125">The Visual Studio project **Properties** page's **Debug** tab provides check boxes for Windows authentication and anonymous authentication.</span></span>

![Tela de navegador de autenticação do Windows](windowsauth/_static/vs-auth-property-menu.png)

<span data-ttu-id="4189d-127">Como alternativa, essas duas propriedades podem ser definidas no *launchSettings.json* arquivo:</span><span class="sxs-lookup"><span data-stu-id="4189d-127">Alternatively, these two properties can be configured in the *launchSettings.json* file:</span></span>

[!code-json[](windowsauth/sample/launchSettings.json?highlight=3-4)]

## <a name="enable-windows-authentication-with-iis"></a><span data-ttu-id="4189d-128">Habilitar a autenticação do Windows com o IIS</span><span class="sxs-lookup"><span data-stu-id="4189d-128">Enable Windows authentication with IIS</span></span>

<span data-ttu-id="4189d-129">O IIS usa o [ASP.NET Core módulo](xref:fundamentals/servers/aspnet-core-module) (ANCM) para hospedar aplicativos ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4189d-129">IIS uses the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) (ANCM) to host ASP.NET Core apps.</span></span> <span data-ttu-id="4189d-130">A ANCM fluxos autenticação do Windows para o IIS por padrão.</span><span class="sxs-lookup"><span data-stu-id="4189d-130">The ANCM flows Windows authentication to IIS by default.</span></span> <span data-ttu-id="4189d-131">Configuração da autenticação do Windows é executada no IIS, não o projeto de aplicativo.</span><span class="sxs-lookup"><span data-stu-id="4189d-131">Configuration of Windows authentication is done within IIS, not the application project.</span></span> <span data-ttu-id="4189d-132">As seções a seguir mostram como usar o Gerenciador do IIS para configurar um aplicativo ASP.NET Core para usar a autenticação do Windows.</span><span class="sxs-lookup"><span data-stu-id="4189d-132">The following sections show how to use IIS Manager to configure an ASP.NET Core app to use Windows authentication.</span></span>

### <a name="create-a-new-iis-site"></a><span data-ttu-id="4189d-133">Criar um novo site do IIS</span><span class="sxs-lookup"><span data-stu-id="4189d-133">Create a new IIS site</span></span>

<span data-ttu-id="4189d-134">Especifique um nome e uma pasta e permitir que ele criar um novo pool de aplicativos.</span><span class="sxs-lookup"><span data-stu-id="4189d-134">Specify a name and folder and allow it to create a new application pool.</span></span>

### <a name="customize-authentication"></a><span data-ttu-id="4189d-135">Personalizar a autenticação</span><span class="sxs-lookup"><span data-stu-id="4189d-135">Customize authentication</span></span>

<span data-ttu-id="4189d-136">Abra o menu de autenticação para o site.</span><span class="sxs-lookup"><span data-stu-id="4189d-136">Open the Authentication menu for the site.</span></span>

![Menu de autenticação do IIS](windowsauth/_static/iis-authentication-menu.png)

<span data-ttu-id="4189d-138">Desabilitar a autenticação anônima e habilitar a autenticação do Windows.</span><span class="sxs-lookup"><span data-stu-id="4189d-138">Disable Anonymous Authentication and enable Windows Authentication.</span></span>

![Configurações de autenticação do IIS](windowsauth/_static/iis-auth-settings.png)

### <a name="publish-your-project-to-the-iis-site-folder"></a><span data-ttu-id="4189d-140">Publicar seu projeto para a pasta de site do IIS</span><span class="sxs-lookup"><span data-stu-id="4189d-140">Publish your project to the IIS site folder</span></span>

<span data-ttu-id="4189d-141">Usando o Visual Studio ou o .NET Core CLI, publica o aplicativo para a pasta de destino.</span><span class="sxs-lookup"><span data-stu-id="4189d-141">Using Visual Studio or the .NET Core CLI, publish the app to the destination folder.</span></span>

![Caixa de diálogo de publicação do Visual Studio](windowsauth/_static/vs-publish-app.png)

<span data-ttu-id="4189d-143">Saiba mais sobre [publicar no IIS](xref:publishing/iis).</span><span class="sxs-lookup"><span data-stu-id="4189d-143">Learn more about [publishing to IIS](xref:publishing/iis).</span></span>

<span data-ttu-id="4189d-144">Inicie o aplicativo para verificar a autenticação do Windows está funcionando.</span><span class="sxs-lookup"><span data-stu-id="4189d-144">Launch the app to verify Windows authentication is working.</span></span>

## <a name="enable-windows-authentication-with-httpsys-or-weblistener"></a><span data-ttu-id="4189d-145">Habilitar a autenticação do Windows com o HTTP.sys ou WebListener</span><span class="sxs-lookup"><span data-stu-id="4189d-145">Enable Windows authentication with HTTP.sys or WebListener</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4189d-146">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4189d-146">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="4189d-147">Embora Kestrel não dá suporte a autenticação do Windows, você pode usar [HTTP.sys](xref:fundamentals/servers/httpsys) para dar suporte a cenários de hospedagem interna no Windows.</span><span class="sxs-lookup"><span data-stu-id="4189d-147">Although Kestrel doesn't support Windows authentication, you can use [HTTP.sys](xref:fundamentals/servers/httpsys) to support self-hosted scenarios on Windows.</span></span> <span data-ttu-id="4189d-148">O exemplo a seguir configura o host de web do aplicativo para usar o HTTP. sys com autenticação do Windows:</span><span class="sxs-lookup"><span data-stu-id="4189d-148">The following example configures the app's web host to use HTTP.sys with Windows authentication:</span></span>

[!code-csharp[](windowsauth/sample/Program2x.cs?highlight=9-14)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4189d-149">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4189d-149">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="4189d-150">Embora Kestrel não dá suporte a autenticação do Windows, você pode usar [WebListener](xref:fundamentals/servers/weblistener) para dar suporte a cenários de hospedagem interna no Windows.</span><span class="sxs-lookup"><span data-stu-id="4189d-150">Although Kestrel doesn't support Windows authentication, you can use [WebListener](xref:fundamentals/servers/weblistener) to support self-hosted scenarios on Windows.</span></span> <span data-ttu-id="4189d-151">O exemplo a seguir configura o host de web do aplicativo para usar WebListener com autenticação do Windows:</span><span class="sxs-lookup"><span data-stu-id="4189d-151">The following example configures the app's web host to use WebListener with Windows authentication:</span></span>

[!code-csharp[](windowsauth/sample/Program1x.cs?highlight=6-11)]

---

## <a name="work-with-windows-authentication"></a><span data-ttu-id="4189d-152">Trabalhar com a autenticação do Windows</span><span class="sxs-lookup"><span data-stu-id="4189d-152">Work with Windows authentication</span></span>

<span data-ttu-id="4189d-153">O estado de configuração do acesso anônimo determina o modo no qual o `[Authorize]` e `[AllowAnonymous]` os atributos são usados no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="4189d-153">The configuration state of anonymous access determines the way in which the `[Authorize]` and `[AllowAnonymous]` attributes are used in the app.</span></span> <span data-ttu-id="4189d-154">As seções a seguir explicam como lidar com os estados de configuração permitidas e não permitidas do acesso anônimo.</span><span class="sxs-lookup"><span data-stu-id="4189d-154">The following two sections explain how to handle the disallowed and allowed configuration states of anonymous access.</span></span>

### <a name="disallow-anonymous-access"></a><span data-ttu-id="4189d-155">Não permitir acesso anônimo</span><span class="sxs-lookup"><span data-stu-id="4189d-155">Disallow anonymous access</span></span>

<span data-ttu-id="4189d-156">Quando a autenticação do Windows está habilitada e o acesso anônimo é desabilitado, o `[Authorize]` e `[AllowAnonymous]` atributos não têm nenhum efeito.</span><span class="sxs-lookup"><span data-stu-id="4189d-156">When Windows authentication is enabled and anonymous access is disabled, the `[Authorize]` and `[AllowAnonymous]` attributes have no effect.</span></span> <span data-ttu-id="4189d-157">Se o site do IIS (ou servidor HTTP. sys ou WebListener) é configurado para não permitir acesso anônimo, a solicitação nunca atinge seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="4189d-157">If the IIS site (or HTTP.sys or WebListener server) is configured to disallow anonymous access, the request never reaches your app.</span></span> <span data-ttu-id="4189d-158">Por esse motivo, o `[AllowAnonymous]` atributo não é aplicável.</span><span class="sxs-lookup"><span data-stu-id="4189d-158">For this reason, the `[AllowAnonymous]` attribute isn't applicable.</span></span>

### <a name="allow-anonymous-access"></a><span data-ttu-id="4189d-159">Permitir o acesso anônimo</span><span class="sxs-lookup"><span data-stu-id="4189d-159">Allow anonymous access</span></span>

<span data-ttu-id="4189d-160">Quando a autenticação do Windows e o acesso anônimo estão habilitados, use o `[Authorize]` e `[AllowAnonymous]` atributos.</span><span class="sxs-lookup"><span data-stu-id="4189d-160">When both Windows authentication and anonymous access are enabled, use the `[Authorize]` and `[AllowAnonymous]` attributes.</span></span> <span data-ttu-id="4189d-161">O `[Authorize]` atributo permite proteger partes do aplicativo que realmente exigem a autenticação do Windows.</span><span class="sxs-lookup"><span data-stu-id="4189d-161">The `[Authorize]` attribute allows you to secure pieces of the app which truly do require Windows authentication.</span></span> <span data-ttu-id="4189d-162">O `[AllowAnonymous]` atributo substituições `[Authorize]` atributo uso em aplicativos que permitem o acesso anônimo.</span><span class="sxs-lookup"><span data-stu-id="4189d-162">The `[AllowAnonymous]` attribute overrides `[Authorize]` attribute usage within apps which allow anonymous access.</span></span> <span data-ttu-id="4189d-163">Consulte [autorização simples](xref:security/authorization/simple) para obter detalhes de uso do atributo.</span><span class="sxs-lookup"><span data-stu-id="4189d-163">See [Simple Authorization](xref:security/authorization/simple) for attribute usage details.</span></span>

<span data-ttu-id="4189d-164">No núcleo do ASP.NET 2. x, o `[Authorize]` atributo requer configuração adicional no *Startup.cs* desafiar solicitações anônimas para a autenticação do Windows.</span><span class="sxs-lookup"><span data-stu-id="4189d-164">In ASP.NET Core 2.x, the `[Authorize]` attribute requires additional configuration in *Startup.cs* to challenge anonymous requests for Windows authentication.</span></span> <span data-ttu-id="4189d-165">A configuração recomendada varia ligeiramente com base no servidor web que está sendo usado.</span><span class="sxs-lookup"><span data-stu-id="4189d-165">The recommended configuration varies slightly based on the web server being used.</span></span>

#### <a name="iis"></a><span data-ttu-id="4189d-166">IIS</span><span class="sxs-lookup"><span data-stu-id="4189d-166">IIS</span></span>

<span data-ttu-id="4189d-167">Se usar o IIS, adicione o seguinte para o `ConfigureServices` método:</span><span class="sxs-lookup"><span data-stu-id="4189d-167">If using IIS, add the following to the `ConfigureServices` method:</span></span> 

```csharp
// IISDefaults requires the following import:
// using Microsoft.AspNetCore.Server.IISIntegration;
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

#### <a name="httpsys"></a><span data-ttu-id="4189d-168">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="4189d-168">HTTP.sys</span></span>

<span data-ttu-id="4189d-169">Se usar o HTTP. sys, adicione o seguinte para o `ConfigureServices` método:</span><span class="sxs-lookup"><span data-stu-id="4189d-169">If using HTTP.sys, add the following to the `ConfigureServices` method:</span></span>

```csharp
// HttpSysDefaults requires the following import:
// using Microsoft.AspNetCore.Server.HttpSys;
services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);
```

### <a name="impersonation"></a><span data-ttu-id="4189d-170">Representação</span><span class="sxs-lookup"><span data-stu-id="4189d-170">Impersonation</span></span>

<span data-ttu-id="4189d-171">ASP.NET Core não implementa a representação.</span><span class="sxs-lookup"><span data-stu-id="4189d-171">ASP.NET Core doesn't implement impersonation.</span></span> <span data-ttu-id="4189d-172">Os aplicativos executados com a identidade de aplicativo para todas as solicitações, usando a identidade de processo ou pool de aplicativo.</span><span class="sxs-lookup"><span data-stu-id="4189d-172">Apps run with the application identity for all requests, using app pool or process identity.</span></span> <span data-ttu-id="4189d-173">Se você precisar executar explicitamente uma ação em nome do usuário, use `WindowsIdentity.RunImpersonated`.</span><span class="sxs-lookup"><span data-stu-id="4189d-173">If you need to explicitly perform an action on behalf of a user, use `WindowsIdentity.RunImpersonated`.</span></span> <span data-ttu-id="4189d-174">Executar uma única ação neste contexto e, em seguida, feche o contexto.</span><span class="sxs-lookup"><span data-stu-id="4189d-174">Run a single action in this context and then close the context.</span></span>

[!code-csharp[](windowsauth/sample/Startup.cs?name=snippet_Impersonate&highlight=10-18)]

<span data-ttu-id="4189d-175">Observe que `RunImpersonated` não dá suporte a operações assíncronas e não deve ser usado para cenários complexos.</span><span class="sxs-lookup"><span data-stu-id="4189d-175">Note that `RunImpersonated` doesn't support asynchronous operations and shouldn't be used for complex scenarios.</span></span> <span data-ttu-id="4189d-176">Por exemplo, solicitações inteiras ou cadeias de middleware de encapsulamento não é suportado ou recomendado.</span><span class="sxs-lookup"><span data-stu-id="4189d-176">For example, wrapping entire requests or middleware chains isn't supported or recommended.</span></span>

---

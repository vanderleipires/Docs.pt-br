---
title: "Configurar a autenticação do Windows no núcleo do ASP.NET"
author: ardalis
description: "Este artigo descreve como configurar a autenticação do Windows no ASP.NET Core, usando o IIS, IIS Express, o HTTP. sys e WebListener."
manager: wpickett
ms.author: riande
ms.date: 10/24/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/windowsauth
ms.openlocfilehash: c229537e7f533eea2173dbc51b8d0d0e097d434a
ms.sourcegitcommit: 49fb3b7669b504d35edad34db8285e56b958a9fc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/23/2018
---
# <a name="configure-windows-authentication-in-an-aspnet-core-app"></a><span data-ttu-id="90eee-103">Configurar a autenticação do Windows em um aplicativo do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="90eee-103">Configure Windows authentication in an ASP.NET Core app</span></span>

<span data-ttu-id="90eee-104">Por [Steve Smith](https://ardalis.com) e [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="90eee-104">By [Steve Smith](https://ardalis.com) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="90eee-105">Autenticação do Windows pode ser configurada para aplicativos ASP.NET Core hospedados no IIS, [HTTP.sys](xref:fundamentals/servers/httpsys), ou [WebListener](xref:fundamentals/servers/weblistener).</span><span class="sxs-lookup"><span data-stu-id="90eee-105">Windows authentication can be configured for ASP.NET Core apps hosted with IIS, [HTTP.sys](xref:fundamentals/servers/httpsys), or [WebListener](xref:fundamentals/servers/weblistener).</span></span>

## <a name="what-is-windows-authentication"></a><span data-ttu-id="90eee-106">O que é autenticação do Windows?</span><span class="sxs-lookup"><span data-stu-id="90eee-106">What is Windows authentication?</span></span>

<span data-ttu-id="90eee-107">Autenticação do Windows se baseia no sistema operacional para autenticar os usuários de aplicativos do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="90eee-107">Windows authentication relies on the operating system to authenticate users of ASP.NET Core apps.</span></span> <span data-ttu-id="90eee-108">Você pode usar a autenticação do Windows quando o servidor é executado em uma rede corporativa usando identidades do domínio do Active Directory ou outras contas do Windows para identificar os usuários.</span><span class="sxs-lookup"><span data-stu-id="90eee-108">You can use Windows authentication when your server runs on a corporate network using Active Directory domain identities or other Windows accounts to identify users.</span></span> <span data-ttu-id="90eee-109">Autenticação do Windows é mais adequada para ambientes de intranet em que os usuários, os aplicativos cliente e servidores web pertencem ao mesmo domínio do Windows.</span><span class="sxs-lookup"><span data-stu-id="90eee-109">Windows authentication is best suited to intranet environments in which users, client applications, and web servers belong to the same Windows domain.</span></span>

<span data-ttu-id="90eee-110">[Saiba mais sobre a autenticação do Windows e instalá-lo para o IIS](https://docs.microsoft.com/iis/configuration/system.webServer/security/authentication/windowsAuthentication/).</span><span class="sxs-lookup"><span data-stu-id="90eee-110">[Learn more about Windows authentication and installing it for IIS](https://docs.microsoft.com/iis/configuration/system.webServer/security/authentication/windowsAuthentication/).</span></span>

## <a name="enable-windows-authentication-in-an-aspnet-core-app"></a><span data-ttu-id="90eee-111">Habilitar a autenticação do Windows em um aplicativo do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="90eee-111">Enable Windows authentication in an ASP.NET Core app</span></span>

<span data-ttu-id="90eee-112">O modelo de aplicativo de Web do Visual Studio pode ser configurado para dar suporte à autenticação do Windows.</span><span class="sxs-lookup"><span data-stu-id="90eee-112">The Visual Studio Web Application template can be configured to support Windows authentication.</span></span>

### <a name="use-the-windows-authentication-app-template"></a><span data-ttu-id="90eee-113">Use o modelo de aplicativo de autenticação do Windows</span><span class="sxs-lookup"><span data-stu-id="90eee-113">Use the Windows authentication app template</span></span>

<span data-ttu-id="90eee-114">No Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="90eee-114">In Visual Studio:</span></span>
1. <span data-ttu-id="90eee-115">Crie um novo Aplicativo Web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="90eee-115">Create a new ASP.NET Core Web Application.</span></span> 
1. <span data-ttu-id="90eee-116">Selecione o aplicativo Web da lista de modelos.</span><span class="sxs-lookup"><span data-stu-id="90eee-116">Select Web Application from the list of templates.</span></span>
1. <span data-ttu-id="90eee-117">Selecione o **alterar autenticação** botão e selecione **autenticação do Windows**.</span><span class="sxs-lookup"><span data-stu-id="90eee-117">Select the **Change Authentication** button and select **Windows Authentication**.</span></span> 

<span data-ttu-id="90eee-118">Execute o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="90eee-118">Run the app.</span></span> <span data-ttu-id="90eee-119">O nome de usuário aparece no canto superior direito do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="90eee-119">The username appears in the top right of the app.</span></span>

![Tela de navegador de autenticação do Windows](windowsauth/_static/browser-screenshot.png)

<span data-ttu-id="90eee-121">Para o trabalho de desenvolvimento usando o IIS Express, o modelo fornece todas as configurações necessárias para usar a autenticação do Windows.</span><span class="sxs-lookup"><span data-stu-id="90eee-121">For development work using IIS Express, the template provides all the configuration necessary to use Windows authentication.</span></span> <span data-ttu-id="90eee-122">A seção a seguir mostra como configurar manualmente um aplicativo ASP.NET Core para autenticação do Windows.</span><span class="sxs-lookup"><span data-stu-id="90eee-122">The following section shows how to manually configure an ASP.NET Core app for Windows authentication.</span></span>

### <a name="visual-studio-settings-for-windows-and-anonymous-authentication"></a><span data-ttu-id="90eee-123">Configurações do Visual Studio para Windows e autenticação anônima</span><span class="sxs-lookup"><span data-stu-id="90eee-123">Visual Studio settings for Windows and anonymous authentication</span></span>

<span data-ttu-id="90eee-124">O projeto do Visual Studio **propriedades** da página **depurar** guia fornece caixas de seleção para autenticação anônima e autenticação do Windows.</span><span class="sxs-lookup"><span data-stu-id="90eee-124">The Visual Studio project **Properties** page's **Debug** tab provides check boxes for Windows authentication and anonymous authentication.</span></span>

![Tela de navegador de autenticação do Windows](windowsauth/_static/vs-auth-property-menu.png)

<span data-ttu-id="90eee-126">Como alternativa, essas duas propriedades podem ser definidas no *launchSettings.json* arquivo:</span><span class="sxs-lookup"><span data-stu-id="90eee-126">Alternatively, these two properties can be configured in the *launchSettings.json* file:</span></span>

[!code-json[](windowsauth/sample/launchSettings.json?highlight=3-4)]

## <a name="enable-windows-authentication-with-iis"></a><span data-ttu-id="90eee-127">Habilitar a autenticação do Windows com o IIS</span><span class="sxs-lookup"><span data-stu-id="90eee-127">Enable Windows authentication with IIS</span></span>

<span data-ttu-id="90eee-128">O IIS usa o [ASP.NET Core módulo](xref:fundamentals/servers/aspnet-core-module) (ANCM) para hospedar aplicativos ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="90eee-128">IIS uses the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) (ANCM) to host ASP.NET Core apps.</span></span> <span data-ttu-id="90eee-129">A ANCM fluxos autenticação do Windows para o IIS por padrão.</span><span class="sxs-lookup"><span data-stu-id="90eee-129">The ANCM flows Windows authentication to IIS by default.</span></span> <span data-ttu-id="90eee-130">Configuração da autenticação do Windows é executada no IIS, não o projeto de aplicativo.</span><span class="sxs-lookup"><span data-stu-id="90eee-130">Configuration of Windows authentication is done within IIS, not the application project.</span></span> <span data-ttu-id="90eee-131">As seções a seguir mostram como usar o Gerenciador do IIS para configurar um aplicativo ASP.NET Core para usar a autenticação do Windows.</span><span class="sxs-lookup"><span data-stu-id="90eee-131">The following sections show how to use IIS Manager to configure an ASP.NET Core app to use Windows authentication.</span></span>

### <a name="create-a-new-iis-site"></a><span data-ttu-id="90eee-132">Criar um novo site do IIS</span><span class="sxs-lookup"><span data-stu-id="90eee-132">Create a new IIS site</span></span>

<span data-ttu-id="90eee-133">Especifique um nome e uma pasta e permitir que ele criar um novo pool de aplicativos.</span><span class="sxs-lookup"><span data-stu-id="90eee-133">Specify a name and folder and allow it to create a new application pool.</span></span>

### <a name="customize-authentication"></a><span data-ttu-id="90eee-134">Personalizar a autenticação</span><span class="sxs-lookup"><span data-stu-id="90eee-134">Customize authentication</span></span>

<span data-ttu-id="90eee-135">Abra o menu de autenticação para o site.</span><span class="sxs-lookup"><span data-stu-id="90eee-135">Open the Authentication menu for the site.</span></span>

![Menu de autenticação do IIS](windowsauth/_static/iis-authentication-menu.png)

<span data-ttu-id="90eee-137">Desabilitar a autenticação anônima e habilitar a autenticação do Windows.</span><span class="sxs-lookup"><span data-stu-id="90eee-137">Disable Anonymous Authentication and enable Windows Authentication.</span></span>

![Configurações de autenticação do IIS](windowsauth/_static/iis-auth-settings.png)

### <a name="publish-your-project-to-the-iis-site-folder"></a><span data-ttu-id="90eee-139">Publicar seu projeto para a pasta de site do IIS</span><span class="sxs-lookup"><span data-stu-id="90eee-139">Publish your project to the IIS site folder</span></span>

<span data-ttu-id="90eee-140">Usando o Visual Studio ou o .NET Core CLI, publica o aplicativo para a pasta de destino.</span><span class="sxs-lookup"><span data-stu-id="90eee-140">Using Visual Studio or the .NET Core CLI, publish the app to the destination folder.</span></span>

![Caixa de diálogo de publicação do Visual Studio](windowsauth/_static/vs-publish-app.png)

<span data-ttu-id="90eee-142">Saiba mais sobre [publicar no IIS](xref:host-and-deploy/iis/index).</span><span class="sxs-lookup"><span data-stu-id="90eee-142">Learn more about [publishing to IIS](xref:host-and-deploy/iis/index).</span></span>

<span data-ttu-id="90eee-143">Inicie o aplicativo para verificar a autenticação do Windows está funcionando.</span><span class="sxs-lookup"><span data-stu-id="90eee-143">Launch the app to verify Windows authentication is working.</span></span>

## <a name="enable-windows-authentication-with-httpsys-or-weblistener"></a><span data-ttu-id="90eee-144">Habilitar a autenticação do Windows com o HTTP.sys ou WebListener</span><span class="sxs-lookup"><span data-stu-id="90eee-144">Enable Windows authentication with HTTP.sys or WebListener</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="90eee-145">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="90eee-145">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="90eee-146">Embora Kestrel não dá suporte a autenticação do Windows, você pode usar [HTTP.sys](xref:fundamentals/servers/httpsys) para dar suporte a cenários de hospedagem interna no Windows.</span><span class="sxs-lookup"><span data-stu-id="90eee-146">Although Kestrel doesn't support Windows authentication, you can use [HTTP.sys](xref:fundamentals/servers/httpsys) to support self-hosted scenarios on Windows.</span></span> <span data-ttu-id="90eee-147">O exemplo a seguir configura o host de web do aplicativo para usar o HTTP. sys com autenticação do Windows:</span><span class="sxs-lookup"><span data-stu-id="90eee-147">The following example configures the app's web host to use HTTP.sys with Windows authentication:</span></span>

[!code-csharp[](windowsauth/sample/Program2x.cs?highlight=9-14)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="90eee-148">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="90eee-148">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="90eee-149">Embora Kestrel não dá suporte a autenticação do Windows, você pode usar [WebListener](xref:fundamentals/servers/weblistener) para dar suporte a cenários de hospedagem interna no Windows.</span><span class="sxs-lookup"><span data-stu-id="90eee-149">Although Kestrel doesn't support Windows authentication, you can use [WebListener](xref:fundamentals/servers/weblistener) to support self-hosted scenarios on Windows.</span></span> <span data-ttu-id="90eee-150">O exemplo a seguir configura o host de web do aplicativo para usar WebListener com autenticação do Windows:</span><span class="sxs-lookup"><span data-stu-id="90eee-150">The following example configures the app's web host to use WebListener with Windows authentication:</span></span>

[!code-csharp[](windowsauth/sample/Program1x.cs?highlight=6-11)]

---

## <a name="work-with-windows-authentication"></a><span data-ttu-id="90eee-151">Trabalhar com a autenticação do Windows</span><span class="sxs-lookup"><span data-stu-id="90eee-151">Work with Windows authentication</span></span>

<span data-ttu-id="90eee-152">O estado de configuração do acesso anônimo determina o modo no qual o `[Authorize]` e `[AllowAnonymous]` os atributos são usados no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="90eee-152">The configuration state of anonymous access determines the way in which the `[Authorize]` and `[AllowAnonymous]` attributes are used in the app.</span></span> <span data-ttu-id="90eee-153">As seções a seguir explicam como lidar com os estados de configuração permitidas e não permitidas do acesso anônimo.</span><span class="sxs-lookup"><span data-stu-id="90eee-153">The following two sections explain how to handle the disallowed and allowed configuration states of anonymous access.</span></span>

### <a name="disallow-anonymous-access"></a><span data-ttu-id="90eee-154">Não permitir acesso anônimo</span><span class="sxs-lookup"><span data-stu-id="90eee-154">Disallow anonymous access</span></span>

<span data-ttu-id="90eee-155">Quando a autenticação do Windows está habilitada e o acesso anônimo é desabilitado, o `[Authorize]` e `[AllowAnonymous]` atributos não têm nenhum efeito.</span><span class="sxs-lookup"><span data-stu-id="90eee-155">When Windows authentication is enabled and anonymous access is disabled, the `[Authorize]` and `[AllowAnonymous]` attributes have no effect.</span></span> <span data-ttu-id="90eee-156">Se o site do IIS (ou servidor HTTP. sys ou WebListener) é configurado para não permitir acesso anônimo, a solicitação nunca atinge seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="90eee-156">If the IIS site (or HTTP.sys or WebListener server) is configured to disallow anonymous access, the request never reaches your app.</span></span> <span data-ttu-id="90eee-157">Por esse motivo, o `[AllowAnonymous]` atributo não é aplicável.</span><span class="sxs-lookup"><span data-stu-id="90eee-157">For this reason, the `[AllowAnonymous]` attribute isn't applicable.</span></span>

### <a name="allow-anonymous-access"></a><span data-ttu-id="90eee-158">Permitir o acesso anônimo</span><span class="sxs-lookup"><span data-stu-id="90eee-158">Allow anonymous access</span></span>

<span data-ttu-id="90eee-159">Quando a autenticação do Windows e o acesso anônimo estão habilitados, use o `[Authorize]` e `[AllowAnonymous]` atributos.</span><span class="sxs-lookup"><span data-stu-id="90eee-159">When both Windows authentication and anonymous access are enabled, use the `[Authorize]` and `[AllowAnonymous]` attributes.</span></span> <span data-ttu-id="90eee-160">O `[Authorize]` atributo permite proteger partes do aplicativo que realmente exigem a autenticação do Windows.</span><span class="sxs-lookup"><span data-stu-id="90eee-160">The `[Authorize]` attribute allows you to secure pieces of the app which truly do require Windows authentication.</span></span> <span data-ttu-id="90eee-161">O `[AllowAnonymous]` atributo substituições `[Authorize]` atributo uso em aplicativos que permitem o acesso anônimo.</span><span class="sxs-lookup"><span data-stu-id="90eee-161">The `[AllowAnonymous]` attribute overrides `[Authorize]` attribute usage within apps which allow anonymous access.</span></span> <span data-ttu-id="90eee-162">Consulte [autorização simples](xref:security/authorization/simple) para obter detalhes de uso do atributo.</span><span class="sxs-lookup"><span data-stu-id="90eee-162">See [Simple Authorization](xref:security/authorization/simple) for attribute usage details.</span></span>

<span data-ttu-id="90eee-163">No núcleo do ASP.NET 2. x, o `[Authorize]` atributo requer configuração adicional no *Startup.cs* desafiar solicitações anônimas para a autenticação do Windows.</span><span class="sxs-lookup"><span data-stu-id="90eee-163">In ASP.NET Core 2.x, the `[Authorize]` attribute requires additional configuration in *Startup.cs* to challenge anonymous requests for Windows authentication.</span></span> <span data-ttu-id="90eee-164">A configuração recomendada varia ligeiramente com base no servidor web que está sendo usado.</span><span class="sxs-lookup"><span data-stu-id="90eee-164">The recommended configuration varies slightly based on the web server being used.</span></span>

> [!NOTE]
> <span data-ttu-id="90eee-165">Por padrão, os usuários que não têm autorização para acessar uma página são apresentados com um documento em branco.</span><span class="sxs-lookup"><span data-stu-id="90eee-165">By default, users who lack authorization to access a page are presented with a blank document.</span></span> <span data-ttu-id="90eee-166">O [StatusCodePages middleware](xref:fundamentals/error-handling#configuring-status-code-pages) podem ser configurados para fornecer aos usuários uma experiência melhor de "Acesso negado".</span><span class="sxs-lookup"><span data-stu-id="90eee-166">The [StatusCodePages middleware](xref:fundamentals/error-handling#configuring-status-code-pages) can be configured to provide users with a better "Access Denied" experience.</span></span>

#### <a name="iis"></a><span data-ttu-id="90eee-167">IIS</span><span class="sxs-lookup"><span data-stu-id="90eee-167">IIS</span></span>

<span data-ttu-id="90eee-168">Se usar o IIS, adicione o seguinte para o `ConfigureServices` método:</span><span class="sxs-lookup"><span data-stu-id="90eee-168">If using IIS, add the following to the `ConfigureServices` method:</span></span> 

```csharp
// IISDefaults requires the following import:
// using Microsoft.AspNetCore.Server.IISIntegration;
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

#### <a name="httpsys"></a><span data-ttu-id="90eee-169">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="90eee-169">HTTP.sys</span></span>

<span data-ttu-id="90eee-170">Se usar o HTTP. sys, adicione o seguinte para o `ConfigureServices` método:</span><span class="sxs-lookup"><span data-stu-id="90eee-170">If using HTTP.sys, add the following to the `ConfigureServices` method:</span></span>

```csharp
// HttpSysDefaults requires the following import:
// using Microsoft.AspNetCore.Server.HttpSys;
services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);
```

### <a name="impersonation"></a><span data-ttu-id="90eee-171">Representação</span><span class="sxs-lookup"><span data-stu-id="90eee-171">Impersonation</span></span>

<span data-ttu-id="90eee-172">ASP.NET Core não implementa a representação.</span><span class="sxs-lookup"><span data-stu-id="90eee-172">ASP.NET Core doesn't implement impersonation.</span></span> <span data-ttu-id="90eee-173">Os aplicativos executados com a identidade de aplicativo para todas as solicitações, usando a identidade de processo ou pool de aplicativo.</span><span class="sxs-lookup"><span data-stu-id="90eee-173">Apps run with the application identity for all requests, using app pool or process identity.</span></span> <span data-ttu-id="90eee-174">Se você precisar executar explicitamente uma ação em nome do usuário, use `WindowsIdentity.RunImpersonated`.</span><span class="sxs-lookup"><span data-stu-id="90eee-174">If you need to explicitly perform an action on behalf of a user, use `WindowsIdentity.RunImpersonated`.</span></span> <span data-ttu-id="90eee-175">Executar uma única ação neste contexto e, em seguida, feche o contexto.</span><span class="sxs-lookup"><span data-stu-id="90eee-175">Run a single action in this context and then close the context.</span></span>

[!code-csharp[](windowsauth/sample/Startup.cs?name=snippet_Impersonate&highlight=10-18)]

<span data-ttu-id="90eee-176">Observe que `RunImpersonated` não dá suporte a operações assíncronas e não deve ser usado para cenários complexos.</span><span class="sxs-lookup"><span data-stu-id="90eee-176">Note that `RunImpersonated` doesn't support asynchronous operations and shouldn't be used for complex scenarios.</span></span> <span data-ttu-id="90eee-177">Por exemplo, solicitações inteiras ou cadeias de middleware de encapsulamento não é suportado ou recomendado.</span><span class="sxs-lookup"><span data-stu-id="90eee-177">For example, wrapping entire requests or middleware chains isn't supported or recommended.</span></span>

---

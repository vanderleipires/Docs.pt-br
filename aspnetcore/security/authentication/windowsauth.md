---
title: "Configurar a autenticação do Windows no núcleo do ASP.NET"
author: ardalis
description: "Como configurar a autenticação do Windows no núcleo do ASP.NET"
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 07/05/2017
ms.topic: article
ms.assetid: cf119f21-1a2b-49a2-b052-548ccb66ee83
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/windowsauth
ms.openlocfilehash: f724584b43eb2be105cc8a207d5c7b6fec558881
ms.sourcegitcommit: 67f54fabbfa4e3942f5bfe1f8a7fdfe4a7a75358
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/19/2017
---
# <a name="configure-windows-authentication-in-aspnet-core"></a><span data-ttu-id="41308-104">Configurar a autenticação do Windows no núcleo do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="41308-104">Configure Windows Authentication in ASP.NET Core</span></span>

<span data-ttu-id="41308-105">Por [Steve Smith](https://ardalis.com)</span><span class="sxs-lookup"><span data-stu-id="41308-105">By [Steve Smith](https://ardalis.com)</span></span>

<span data-ttu-id="41308-106">Autenticação do Windows pode ser configurada para aplicativos ASP.NET Core hospedados com o IIS ou WebListener.</span><span class="sxs-lookup"><span data-stu-id="41308-106">Windows authentication can be configured for ASP.NET Core apps hosted with IIS or WebListener.</span></span>

## <a name="what-is-windows-authentication"></a><span data-ttu-id="41308-107">O que é autenticação do Windows</span><span class="sxs-lookup"><span data-stu-id="41308-107">What is Windows authentication</span></span>

<span data-ttu-id="41308-108">Autenticação do Windows se baseia no sistema operacional para autenticar os usuários de aplicativos do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="41308-108">Windows authentication relies on the operating system to authenticate users of ASP.NET Core apps.</span></span> <span data-ttu-id="41308-109">Você pode usar a autenticação do Windows quando o servidor é executado em uma rede corporativa usando identidades do domínio do Active Directory ou outras contas do Windows para identificar os usuários.</span><span class="sxs-lookup"><span data-stu-id="41308-109">You can use Windows authentication when your server runs on a corporate network using Active Directory domain identities or other Windows accounts to identify users.</span></span> <span data-ttu-id="41308-110">Autenticação do Windows é uma forma segura de autenticação melhor adequada para ambientes de intranet onde os usuários, os aplicativos cliente e servidores web pertencem ao mesmo domínio do Windows.</span><span class="sxs-lookup"><span data-stu-id="41308-110">Windows authentication is a secure form of authentication best suited to intranet environments where users, client applications, and web servers belong to the same Windows domain.</span></span>

<span data-ttu-id="41308-111">[Saiba mais sobre a autenticação do Windows e instalá-lo para o IIS](https://docs.microsoft.com/iis/configuration/system.webServer/security/authentication/windowsAuthentication/).</span><span class="sxs-lookup"><span data-stu-id="41308-111">[Learn more about Windows Authentication and installing it for IIS](https://docs.microsoft.com/iis/configuration/system.webServer/security/authentication/windowsAuthentication/).</span></span>

## <a name="enabling-windows-authentication-in-an-aspnet-core-application"></a><span data-ttu-id="41308-112">Habilitar a autenticação do Windows em um aplicativo do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="41308-112">Enabling Windows authentication in an ASP.NET Core application</span></span>

<span data-ttu-id="41308-113">O modelo de aplicativo de Web do Visual Studio pode ser configurado para dar suporte à autenticação do Windows.</span><span class="sxs-lookup"><span data-stu-id="41308-113">The Visual Studio Web Application template can be configured to support Windows authentication.</span></span>

### <a name="using-the-windows-authentication-app-template"></a><span data-ttu-id="41308-114">Usando o modelo de aplicativo de autenticação do Windows</span><span class="sxs-lookup"><span data-stu-id="41308-114">Using the Windows authentication app template</span></span>

<span data-ttu-id="41308-115">No Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="41308-115">In Visual Studio:</span></span>
* <span data-ttu-id="41308-116">Crie um novo Aplicativo Web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="41308-116">Create a new ASP.NET Core Web Application.</span></span> 
* <span data-ttu-id="41308-117">Selecione o aplicativo Web da lista de modelos.</span><span class="sxs-lookup"><span data-stu-id="41308-117">Select Web Application from the list of templates.</span></span>
* <span data-ttu-id="41308-118">Selecione o botão Alterar autenticação e selecione **autenticação do Windows**.</span><span class="sxs-lookup"><span data-stu-id="41308-118">Select the Change Authentication button and select **Windows Authentication**.</span></span> 

<span data-ttu-id="41308-119">Execute o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="41308-119">Run the app.</span></span> <span data-ttu-id="41308-120">O nome de usuário aparece no canto superior direito do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="41308-120">The username appears in the top right of the app.</span></span>

![Tela de navegador de autenticação do Windows](windowsauth/_static/browser-screenshot.png)

<span data-ttu-id="41308-122">Para o trabalho de desenvolvimento usando o IIS Express, o modelo fornece todas as configurações necessárias para usar a autenticação do Windows.</span><span class="sxs-lookup"><span data-stu-id="41308-122">For development work using IIS Express, the template provides all the configuration necessary to use Windows authentication.</span></span> <span data-ttu-id="41308-123">A próxima seção mostra como configurar um aplicativo ASP.NET Core manualmente para a autenticação do Windows.</span><span class="sxs-lookup"><span data-stu-id="41308-123">The next section shows how to configure an ASP.NET Core app manually for Windows authentication.</span></span>

### <a name="visual-studio-settings-for-windows-and-anonymous-authentication"></a><span data-ttu-id="41308-124">Configurações do Visual Studio para Windows e autenticação anônima</span><span class="sxs-lookup"><span data-stu-id="41308-124">Visual Studio settings for Windows and anonymous authentication</span></span>

<span data-ttu-id="41308-125">Página de propriedades do Visual Studio, a guia depurar fornece caixas de seleção para autenticação anônima e autenticação do Windows.</span><span class="sxs-lookup"><span data-stu-id="41308-125">The Visual Studio properties page, debug tab provides check boxes for Windows authentication and anonymous authentication.</span></span>

![Tela de navegador de autenticação do Windows](windowsauth/_static/vs-auth-property-menu.png)

<span data-ttu-id="41308-127">Você também pode configurar essas propriedades no `launchSettings.json` arquivo:</span><span class="sxs-lookup"><span data-stu-id="41308-127">You can also configure these properties in the `launchSettings.json` file:</span></span>

```json
{
  "iisSettings": {
    "windowsAuthentication": true,
    "anonymousAuthentication": false,
    "iisExpress": {
      "applicationUrl": "http://localhost:52171/",
      "sslPort": 0
    }
  } // additional options trimmed
}
```

## <a name="enabling-windows-authentication-with-iis"></a><span data-ttu-id="41308-128">Habilitar a autenticação do Windows com o IIS</span><span class="sxs-lookup"><span data-stu-id="41308-128">Enabling Windows Authentication with IIS</span></span>

<span data-ttu-id="41308-129">O IIS usa o [ASP.NET Core módulo](xref:fundamentals/servers/aspnet-core-module) (ANCM) para hospedar aplicativos ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="41308-129">IIS uses the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) (ANCM) to host ASP.NET Core apps.</span></span> <span data-ttu-id="41308-130">A ANCM fluxos autenticação do Windows para o IIS por padrão.</span><span class="sxs-lookup"><span data-stu-id="41308-130">The ANCM flows Windows authentication to IIS by default.</span></span> <span data-ttu-id="41308-131">Configuração da autenticação do Windows é executada no IIS, não o projeto de aplicativo.</span><span class="sxs-lookup"><span data-stu-id="41308-131">Configuration of Windows authentication is done within IIS, not the application project.</span></span> <span data-ttu-id="41308-132">As seções a seguir mostram como usar o Gerenciador do IIS para configurar um aplicativo ASP.NET Core para usar a autenticação do Windows:</span><span class="sxs-lookup"><span data-stu-id="41308-132">The following sections show how to use IIS Manager to configure an ASP.NET Core app to use Windows authentication:</span></span>

### <a name="create-a-new-iis-site"></a><span data-ttu-id="41308-133">Criar um novo site do IIS</span><span class="sxs-lookup"><span data-stu-id="41308-133">Create a new IIS site</span></span>

<span data-ttu-id="41308-134">Especifique um nome e uma pasta e permitir que ele criar um novo pool de aplicativos.</span><span class="sxs-lookup"><span data-stu-id="41308-134">Specify a name and folder and allow it to create a new application pool.</span></span>

### <a name="customize-authentication"></a><span data-ttu-id="41308-135">Personalizar a autenticação</span><span class="sxs-lookup"><span data-stu-id="41308-135">Customize Authentication</span></span>

<span data-ttu-id="41308-136">Abra o menu de autenticação para o site.</span><span class="sxs-lookup"><span data-stu-id="41308-136">Open the Authentication menu for the site.</span></span>

![Menu de autenticação do IIS](windowsauth/_static/iis-authentication-menu.png)

<span data-ttu-id="41308-138">Desabilitar a autenticação anônima e habilitar a autenticação do Windows.</span><span class="sxs-lookup"><span data-stu-id="41308-138">Disable Anonymous Authentication and enable Windows Authentication.</span></span>

![Configurações de autenticação do IIS](windowsauth/_static/iis-auth-settings.png)

### <a name="publish-your-project-to-the-iis-site-folder"></a><span data-ttu-id="41308-140">Publicar seu projeto para a pasta de site do IIS</span><span class="sxs-lookup"><span data-stu-id="41308-140">Publish your project to the IIS site folder</span></span>

<span data-ttu-id="41308-141">Usando o Visual Studio ou o .NET Core CLI, *publicar* seu aplicativo para a pasta de destino.</span><span class="sxs-lookup"><span data-stu-id="41308-141">Using Visual Studio or the .NET Core CLI, *publish* your app to the destination folder.</span></span>

![Caixa de diálogo de publicação do Visual Studio](windowsauth/_static/vs-publish-app.png)

<span data-ttu-id="41308-143">Saiba mais sobre [publicar no IIS](xref:publishing/iis).</span><span class="sxs-lookup"><span data-stu-id="41308-143">Learn more about [publishing to IIS](xref:publishing/iis).</span></span>

<span data-ttu-id="41308-144">Inicie o aplicativo para verificar a autenticação do Windows está funcionando.</span><span class="sxs-lookup"><span data-stu-id="41308-144">Launch the app to verify Windows authentication is working.</span></span>

## <a name="enabling-windows-authentication-with-weblistener"></a><span data-ttu-id="41308-145">Habilitar a autenticação do Windows com WebListener</span><span class="sxs-lookup"><span data-stu-id="41308-145">Enabling Windows authentication with WebListener</span></span>

<span data-ttu-id="41308-146">Embora Kestrel não dá suporte a autenticação do Windows, você pode usar [WebListener](xref:fundamentals/servers/weblistener) para dar suporte a cenários de hospedagem interna no Windows.</span><span class="sxs-lookup"><span data-stu-id="41308-146">Although Kestrel doesn't support Windows authentication, you can use [WebListener](xref:fundamentals/servers/weblistener) to support self-hosted scenarios on Windows.</span></span> <span data-ttu-id="41308-147">O exemplo a seguir configura o host de web do aplicativo para usar WebListener com autenticação do Windows:</span><span class="sxs-lookup"><span data-stu-id="41308-147">The following example configures the app's web host to use WebListener with Windows authentication:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        var host = new WebHostBuilder()
            .UseWebListener(options =>
            {
                options.ListenerSettings.Authentication.Schemes = 
                    AuthenticationSchemes.Negotiate | AuthenticationSchemes.NTLM;
                options.ListenerSettings.Authentication.AllowAnonymous = false;
            })
            .UseContentRoot(Directory.GetCurrentDirectory())
            .UseStartup<Startup>()
            .Build();

        host.Run();
    }
}
```

## <a name="working-with-windows-authentication"></a><span data-ttu-id="41308-148">Trabalhando com autenticação do Windows</span><span class="sxs-lookup"><span data-stu-id="41308-148">Working with Windows authentication</span></span>

<span data-ttu-id="41308-149">Se seu aplicativo usa autenticação do Windows e acesso anônimo, você pode usar o ``[Authorize]`` e ``[AllowAnonymous]`` atributos.</span><span class="sxs-lookup"><span data-stu-id="41308-149">If your app uses Windows authentication and anonymous access, you can use the ``[Authorize]`` and ``[AllowAnonymous]`` attributes.</span></span> <span data-ttu-id="41308-150">Aplicativos que não têm anônima habilitado não exigem ``[Authorize]``; o aplicativo é tratado como exigir autenticação, solicitações anônimas são rejeitadas.</span><span class="sxs-lookup"><span data-stu-id="41308-150">Apps that do not have anonymous enabled do not require ``[Authorize]``; the  app is treated as requiring authentication, anonymous requests are rejected.</span></span> <span data-ttu-id="41308-151">Observe que, se o site do IIS é configurado **não** para permitir acesso anônimo, o ``[AllowAnonymous]`` atributo faz **não** permitir solicitações anônimas.</span><span class="sxs-lookup"><span data-stu-id="41308-151">Note, if the IIS site is configured **not** to allow anonymous access, the ``[AllowAnonymous]`` attribute does **not** allow anonymous requests.</span></span> <span data-ttu-id="41308-152">O ``[AllowAnonymous]`` atributo substituições ``[Authorize]`` atributo uso em aplicativos que permitem o acesso anônimo.</span><span class="sxs-lookup"><span data-stu-id="41308-152">The ``[AllowAnonymous]`` attribute overrides ``[Authorize]`` attribute usage within apps that allow anonymous access.</span></span>

### <a name="impersonation"></a><span data-ttu-id="41308-153">Representação</span><span class="sxs-lookup"><span data-stu-id="41308-153">Impersonation</span></span>

<span data-ttu-id="41308-154">ASP.NET Core não implementa a representação.</span><span class="sxs-lookup"><span data-stu-id="41308-154">ASP.NET Core does not implement impersonation.</span></span> <span data-ttu-id="41308-155">Os aplicativos executados com a identidade de aplicativo para todas as solicitações, usando a identidade de processo ou pool de aplicativo.</span><span class="sxs-lookup"><span data-stu-id="41308-155">Apps run with the application identity for all requests, using app pool or process identity.</span></span> <span data-ttu-id="41308-156">Se você precisar executar explicitamente uma ação em nome do usuário, use ``WindowsIdentity.RunImpersonated``.</span><span class="sxs-lookup"><span data-stu-id="41308-156">If you need to explicitly perform an action on behalf of a user, use ``WindowsIdentity.RunImpersonated``.</span></span> <span data-ttu-id="41308-157">Executar uma única ação neste contexto e, em seguida, feche o contexto.</span><span class="sxs-lookup"><span data-stu-id="41308-157">Run a single action in this context and then close the context.</span></span> <span data-ttu-id="41308-158">Observe que ``RunImpersonated`` não oferece suporte a assíncrona e não deve ser usado para cenários complexos.</span><span class="sxs-lookup"><span data-stu-id="41308-158">Note that ``RunImpersonated`` does not support async and should not be used for complex scenarios.</span></span> <span data-ttu-id="41308-159">Por exemplo, solicitações de inteiras ou cadeias de middleware de encapsulamento não é suportado nem recomendado.</span><span class="sxs-lookup"><span data-stu-id="41308-159">For example, wrapping entire requests or middleware chains is not supported or recommended.</span></span>

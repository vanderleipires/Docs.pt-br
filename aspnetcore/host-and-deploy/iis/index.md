---
title: Hospedar o ASP.NET Core no Windows com o IIS
author: guardrex
description: "Saiba como hospedar aplicativos ASP.NET Core no Windows Server IIS (Serviços de Informações da Internet)."
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 03/13/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: host-and-deploy/iis/index
ms.openlocfilehash: 18c7448ad79891d04eca1e939a0aeeabe417bde8
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
# <a name="host-aspnet-core-on-windows-with-iis"></a><span data-ttu-id="140e8-103">Hospedar o ASP.NET Core no Windows com o IIS</span><span class="sxs-lookup"><span data-stu-id="140e8-103">Host ASP.NET Core on Windows with IIS</span></span>

<span data-ttu-id="140e8-104">Por [Luke Latham](https://github.com/guardrex) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="140e8-104">By [Luke Latham](https://github.com/guardrex) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

## <a name="supported-operating-systems"></a><span data-ttu-id="140e8-105">Sistemas operacionais com suporte</span><span class="sxs-lookup"><span data-stu-id="140e8-105">Supported operating systems</span></span>

<span data-ttu-id="140e8-106">Há suporte para os seguintes sistemas operacionais:</span><span class="sxs-lookup"><span data-stu-id="140e8-106">The following operating systems are supported:</span></span>

* <span data-ttu-id="140e8-107">Windows 7 e mais recente</span><span class="sxs-lookup"><span data-stu-id="140e8-107">Windows 7 and newer</span></span>
* <span data-ttu-id="140e8-108">Windows Server 2008 R2 e mais recente&#8224;</span><span class="sxs-lookup"><span data-stu-id="140e8-108">Windows Server 2008 R2 and newer&#8224;</span></span>

<span data-ttu-id="140e8-109">&#8224;Conceitualmente, a configuração do IIS descrita neste documento também se aplica à hospedagem de aplicativos ASP.NET Core no IIS do Nano Server, mas consulte [ASP.NET Core com o IIS no Nano Server](xref:tutorials/nano-server) para obter instruções específicas.</span><span class="sxs-lookup"><span data-stu-id="140e8-109">&#8224;Conceptually, the IIS configuration described in this document also applies to hosting ASP.NET Core apps on Nano Server IIS, but refer to [ASP.NET Core with IIS on Nano Server](xref:tutorials/nano-server) for specific instructions.</span></span>

<span data-ttu-id="140e8-110">O [servidor HTTP.sys](xref:fundamentals/servers/httpsys) (anteriormente chamado de [WebListener](xref:fundamentals/servers/weblistener)) não funcionará em uma configuração de proxy reverso com IIS.</span><span class="sxs-lookup"><span data-stu-id="140e8-110">[HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener)) won't work in a reverse proxy configuration with IIS.</span></span> <span data-ttu-id="140e8-111">Use o [servidor Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="140e8-111">Use the [Kestrel server](xref:fundamentals/servers/kestrel).</span></span>

## <a name="iis-configuration"></a><span data-ttu-id="140e8-112">Configuração do IIS</span><span class="sxs-lookup"><span data-stu-id="140e8-112">IIS configuration</span></span>

<span data-ttu-id="140e8-113">Habilite a função **Servidor Web (IIS)** e estabeleça serviços de função.</span><span class="sxs-lookup"><span data-stu-id="140e8-113">Enable the **Web Server (IIS)** role and establish role services.</span></span>

### <a name="windows-desktop-operating-systems"></a><span data-ttu-id="140e8-114">Sistemas operacionais de área de trabalho do Windows</span><span class="sxs-lookup"><span data-stu-id="140e8-114">Windows desktop operating systems</span></span>

<span data-ttu-id="140e8-115">Navegue para **Painel de Controle** > **Programas** > **Programas e Recursos** > **Ativar ou desativar recursos do Windows** (lado esquerdo da tela).</span><span class="sxs-lookup"><span data-stu-id="140e8-115">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span> <span data-ttu-id="140e8-116">Abra o grupo de **Serviços de Informações da Internet** e **Ferramentas de Gerenciamento da Web**.</span><span class="sxs-lookup"><span data-stu-id="140e8-116">Open the group for **Internet Information Services** and **Web Management Tools**.</span></span> <span data-ttu-id="140e8-117">Marque a caixa de **Console de Gerenciamento do IIS**.</span><span class="sxs-lookup"><span data-stu-id="140e8-117">Check the box for **IIS Management Console**.</span></span> <span data-ttu-id="140e8-118">Marque a caixa de **Serviços na World Wide Web**.</span><span class="sxs-lookup"><span data-stu-id="140e8-118">Check the box for **World Wide Web Services**.</span></span> <span data-ttu-id="140e8-119">Aceite os recursos padrão dos **Serviços na World Wide Web** ou personalize os recursos do IIS.</span><span class="sxs-lookup"><span data-stu-id="140e8-119">Accept the default features for **World Wide Web Services** or customize the IIS features.</span></span>

![O Console de Gerenciamento do IIS e os Serviços na World Wide Web estão selecionados em Recursos do Windows.](index/_static/windows-features-win10.png)

### <a name="windows-server-operating-systems"></a><span data-ttu-id="140e8-121">Sistemas operacionais do Windows Server</span><span class="sxs-lookup"><span data-stu-id="140e8-121">Windows Server operating systems</span></span>

<span data-ttu-id="140e8-122">Para sistemas operacionais de servidor, use o assistente para **Adicionar Funções e Recursos** por meio do menu **Gerenciar** ou do link no **Gerenciador do Servidor**.</span><span class="sxs-lookup"><span data-stu-id="140e8-122">For server operating systems, use the **Add Roles and Features** wizard via the **Manage** menu or the link in **Server Manager**.</span></span> <span data-ttu-id="140e8-123">Na etapa **Funções de Servidor**, marque a caixa de **Servidor Web (IIS)**.</span><span class="sxs-lookup"><span data-stu-id="140e8-123">On the **Server Roles** step, check the box for **Web Server (IIS)**.</span></span>

![A função de Servidor Web IIS é selecionada na etapa Selecionar funções de servidor.](index/_static/server-roles-ws2016.png)

<span data-ttu-id="140e8-125">Na etapa **Serviços de função**, selecione os serviços de função do IIS desejados ou aceite os serviços de função padrão fornecidos.</span><span class="sxs-lookup"><span data-stu-id="140e8-125">On the **Role services** step, select the IIS role services desired or accept the default role services provided.</span></span>

![Os serviços de função padrão são selecionados na etapa Selecionar serviços de função.](index/_static/role-services-ws2016.png)

<span data-ttu-id="140e8-127">Continue para a etapa **Confirmação** para instalar os serviços e a função de servidor Web.</span><span class="sxs-lookup"><span data-stu-id="140e8-127">Proceed through the **Confirmation** step to install the web server role and services.</span></span> <span data-ttu-id="140e8-128">A reinicialização de um servidor/IIS não é necessária após a instalação da função do Servidor Web (IIS).</span><span class="sxs-lookup"><span data-stu-id="140e8-128">A server/IIS restart isn't required after installing the Web Server (IIS) role.</span></span>

## <a name="install-the-net-core-windows-server-hosting-bundle"></a><span data-ttu-id="140e8-129">Instalar o pacote de hospedagem do Windows Server do .NET Core</span><span class="sxs-lookup"><span data-stu-id="140e8-129">Install the .NET Core Windows Server Hosting bundle</span></span>

1. <span data-ttu-id="140e8-130">Instale o [pacote de hospedagem do Windows Server do .NET Core](https://aka.ms/dotnetcore-2-windowshosting) no sistema de hospedagem.</span><span class="sxs-lookup"><span data-stu-id="140e8-130">Install the [.NET Core Windows Server Hosting bundle](https://aka.ms/dotnetcore-2-windowshosting) on the hosting system.</span></span> <span data-ttu-id="140e8-131">O pacote instala o Tempo de Execução .NET Core, a Biblioteca do .NET Core e o [Módulo do ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="140e8-131">The bundle installs the .NET Core Runtime, .NET Core Library, and the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="140e8-132">O módulo cria o proxy reverso entre o IIS e o servidor Kestrel.</span><span class="sxs-lookup"><span data-stu-id="140e8-132">The module creates the reverse proxy between IIS and the Kestrel server.</span></span> <span data-ttu-id="140e8-133">Se o sistema não tiver uma conexão com a Internet, obtenha e instale os [Pacotes redistribuíveis do Microsoft Visual C++ 2015](https://www.microsoft.com/download/details.aspx?id=53840) antes de instalar o pacote de hospedagem do Windows Server do .NET Core.</span><span class="sxs-lookup"><span data-stu-id="140e8-133">If the system doesn't have an Internet connection, obtain and install the [Microsoft Visual C++ 2015 Redistributable](https://www.microsoft.com/download/details.aspx?id=53840) before installing the .NET Core Windows Server Hosting bundle.</span></span>

2. <span data-ttu-id="140e8-134">Reinicie o sistema ou execute **net stop was /y** seguido por **net start w3svc** em um prompt de comando para acompanhar uma alteração no PATH do sistema.</span><span class="sxs-lookup"><span data-stu-id="140e8-134">Restart the system or execute **net stop was /y** followed by **net start w3svc** from a command prompt to pick up a change to the system PATH.</span></span>

> [!NOTE]
> <span data-ttu-id="140e8-135">Para obter informações sobre a Configuração Compartilhada do IIS, consulte [Módulo do ASP.NET Core com a Configuração Compartilhada do IIS](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration).</span><span class="sxs-lookup"><span data-stu-id="140e8-135">For information on IIS Shared Configuration, see [ASP.NET Core Module with IIS Shared Configuration](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration).</span></span>

## <a name="install-web-deploy-when-publishing-with-visual-studio"></a><span data-ttu-id="140e8-136">Instalar a Implantação da Web durante a publicação com o Visual Studio</span><span class="sxs-lookup"><span data-stu-id="140e8-136">Install Web Deploy when publishing with Visual Studio</span></span>

<span data-ttu-id="140e8-137">Ao implantar aplicativos para servidores com [Implantação da Web](/iis/publish/using-web-deploy/introduction-to-web-deploy), instale a versão mais recente da Implantação da Web no servidor.</span><span class="sxs-lookup"><span data-stu-id="140e8-137">When deploying apps to servers with [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy), install the latest version of Web Deploy on the server.</span></span> <span data-ttu-id="140e8-138">Para instalar a Implantação da Web, use o [WebPI (Web Platform Installer)](https://www.microsoft.com/web/downloads/platform.aspx) ou obtenha um instalador diretamente no [Centro de Download da Microsoft](https://www.microsoft.com/download/details.aspx?id=43717).</span><span class="sxs-lookup"><span data-stu-id="140e8-138">To install Web Deploy, use the [Web Platform Installer (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) or obtain an installer directly from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=43717).</span></span> <span data-ttu-id="140e8-139">O método preferencial é usar o WebPI.</span><span class="sxs-lookup"><span data-stu-id="140e8-139">The preferred method is to use WebPI.</span></span> <span data-ttu-id="140e8-140">O WebPI oferece uma instalação autônoma e uma configuração para provedores de hospedagem.</span><span class="sxs-lookup"><span data-stu-id="140e8-140">WebPI offers a standalone setup and a configuration for hosting providers.</span></span>

## <a name="application-configuration"></a><span data-ttu-id="140e8-141">Configuração do aplicativo</span><span class="sxs-lookup"><span data-stu-id="140e8-141">Application configuration</span></span>

### <a name="enabling-the-iisintegration-components"></a><span data-ttu-id="140e8-142">Habilitando os componentes de IISIntegration</span><span class="sxs-lookup"><span data-stu-id="140e8-142">Enabling the IISIntegration components</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="140e8-143">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="140e8-143">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="140e8-144">Um típico *Program.cs* chama [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) para começar a configurar um host.</span><span class="sxs-lookup"><span data-stu-id="140e8-144">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to begin setting up a host.</span></span> <span data-ttu-id="140e8-145">`CreateDefaultBuilder` configura [Kestrel](xref:fundamentals/servers/kestrel) como o servidor Web e habilita a integração IIS configurando o caminho base e a porta para o [módulo do ASP.NET Core](xref:fundamentals/servers/aspnet-core-module):</span><span class="sxs-lookup"><span data-stu-id="140e8-145">`CreateDefaultBuilder` configures [Kestrel](xref:fundamentals/servers/kestrel) as the web server and enables IIS integration by configuring the base path and port for the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module):</span></span>

```csharp
public static IWebHost BuildWebHost(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="140e8-146">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="140e8-146">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="140e8-147">Inclua uma dependência no pacote [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) nas dependências do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="140e8-147">Include a dependency on the [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) package in the app's dependencies.</span></span> <span data-ttu-id="140e8-148">Incorpore o middleware Integração do IIS no aplicativo adicionando o método de extensão *UseIISIntegration* ao *WebHostBuilder*:</span><span class="sxs-lookup"><span data-stu-id="140e8-148">Incorporate IIS Integration middleware into the app by adding the *UseIISIntegration* extension method to *WebHostBuilder*:</span></span>

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .UseIISIntegration()
    ...
```

<span data-ttu-id="140e8-149">Ambos `UseKestrel` e `UseIISIntegration` são necessários.</span><span class="sxs-lookup"><span data-stu-id="140e8-149">Both `UseKestrel` and `UseIISIntegration` are required.</span></span> <span data-ttu-id="140e8-150">A chamada do código a `UseIISIntegration` não afeta a portabilidade do código.</span><span class="sxs-lookup"><span data-stu-id="140e8-150">Code calling `UseIISIntegration` doesn't affect code portability.</span></span> <span data-ttu-id="140e8-151">Se o aplicativo não é executado por trás do IIS (por exemplo, o aplicativo é executado diretamente em Kestrel), `UseIISIntegration` fica sem operações.</span><span class="sxs-lookup"><span data-stu-id="140e8-151">If the app isn't run behind IIS (for example, the app is run directly on Kestrel), `UseIISIntegration` no-ops.</span></span>

---

<span data-ttu-id="140e8-152">Para obter mais informações sobre hospedagem, consulte [Hospedagem em ASP.NET Core](xref:fundamentals/hosting).</span><span class="sxs-lookup"><span data-stu-id="140e8-152">For more information on hosting, see [Hosting in ASP.NET Core](xref:fundamentals/hosting).</span></span>

### <a name="iis-options"></a><span data-ttu-id="140e8-153">Opções do IIS</span><span class="sxs-lookup"><span data-stu-id="140e8-153">IIS options</span></span>

<span data-ttu-id="140e8-154">Para configurar opções do IIS, inclua uma configuração de serviço para `IISOptions` em `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="140e8-154">To configure IIS options, include a service configuration for `IISOptions` in `ConfigureServices`:</span></span>

```csharp
services.Configure<IISOptions>(options => 
{
    ...
});
```

| <span data-ttu-id="140e8-155">Opção</span><span class="sxs-lookup"><span data-stu-id="140e8-155">Option</span></span>                         | <span data-ttu-id="140e8-156">Padrão</span><span class="sxs-lookup"><span data-stu-id="140e8-156">Default</span></span> | <span data-ttu-id="140e8-157">Configuração</span><span class="sxs-lookup"><span data-stu-id="140e8-157">Setting</span></span> |
| ------------------------------ | ------- | ------- |
| `AutomaticAuthentication`      | `true`  | <span data-ttu-id="140e8-158">Se `true`, o middleware de autenticação configurará o `HttpContext.User` e responderá a desafios genéricos.</span><span class="sxs-lookup"><span data-stu-id="140e8-158">If `true`, the authentication middleware sets the `HttpContext.User` and responds to generic challenges.</span></span> <span data-ttu-id="140e8-159">Se `false`, o middleware de autenticação fornecerá apenas uma identidade (`HttpContext.User`) e responderá a desafios quando explicitamente solicitado pelo `AuthenticationScheme`.</span><span class="sxs-lookup"><span data-stu-id="140e8-159">If `false`, the authentication middleware only provides an identity (`HttpContext.User`) and responds to challenges when explicitly requested by the `AuthenticationScheme`.</span></span> <span data-ttu-id="140e8-160">A autenticação do Windows deve estar habilitada no IIS para que o `AutomaticAuthentication` funcione.</span><span class="sxs-lookup"><span data-stu-id="140e8-160">Windows Authentication must be enabled in IIS for `AutomaticAuthentication` to function.</span></span> |
| `AuthenticationDisplayName`    | `null`  | <span data-ttu-id="140e8-161">Configura o nome de exibição mostrado aos usuários em páginas de logon.</span><span class="sxs-lookup"><span data-stu-id="140e8-161">Sets the display name shown to users on login pages.</span></span> |
| `ForwardClientCertificate`     | `true`  | <span data-ttu-id="140e8-162">Se `true` e o cabeçalho da solicitação `MS-ASPNETCORE-CLIENTCERT` estiverem presentes, o `HttpContext.Connection.ClientCertificate` será populado.</span><span class="sxs-lookup"><span data-stu-id="140e8-162">If `true` and the `MS-ASPNETCORE-CLIENTCERT` request header is present, the `HttpContext.Connection.ClientCertificate` is populated.</span></span> |

### <a name="webconfig"></a><span data-ttu-id="140e8-163">web.config</span><span class="sxs-lookup"><span data-stu-id="140e8-163">web.config</span></span>

<span data-ttu-id="140e8-164">O objetivo principal do arquivo *web.config* é configurar o [módulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="140e8-164">The *web.config* file's primary purpose is to configure the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="140e8-165">Como opção, ele pode fornecer definições de configuração IIS adicionais.</span><span class="sxs-lookup"><span data-stu-id="140e8-165">It may optionally provide additional IIS configuration settings.</span></span> <span data-ttu-id="140e8-166">A criação, transformação e publicação do *web.config* é gerenciada pelo SDK Web do .NET Core (`Microsoft.NET.Sdk.Web`).</span><span class="sxs-lookup"><span data-stu-id="140e8-166">Creating, transforming, and publishing *web.config* is handled by the .NET Core Web SDK (`Microsoft.NET.Sdk.Web`).</span></span> <span data-ttu-id="140e8-167">O SDK é definido na parte superior do arquivo de projeto, `<Project Sdk="Microsoft.NET.Sdk.Web">`.</span><span class="sxs-lookup"><span data-stu-id="140e8-167">The SDK is set  at the top of the project file, `<Project Sdk="Microsoft.NET.Sdk.Web">`.</span></span> <span data-ttu-id="140e8-168">Para impedir que o SDK se transforme no arquivo *web.config*, adicione a propriedade **\<IsTransformWebConfigDisabled>** ao arquivo de projeto com a configuração `true`:</span><span class="sxs-lookup"><span data-stu-id="140e8-168">To prevent the SDK from transforming the *web.config* file, add the **\<IsTransformWebConfigDisabled>** property to the project file with a setting of `true`:</span></span>

```xml
<PropertyGroup>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

<span data-ttu-id="140e8-169">Se um arquivo *web.config* estiver no projeto, ele será transformado com o *processPath* e os *arguments* corretos para configurar o [Módulo do ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) e será movido para o [resultado publicado](xref:host-and-deploy/directory-structure).</span><span class="sxs-lookup"><span data-stu-id="140e8-169">If a *web.config* file is in the project, it's transformed with the correct *processPath* and *arguments* to configure the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and moved to [published output](xref:host-and-deploy/directory-structure).</span></span> <span data-ttu-id="140e8-170">A transformação não altera as definições de configuração do IIS no arquivo.</span><span class="sxs-lookup"><span data-stu-id="140e8-170">The transformation doesn't modify IIS configuration settings in the file.</span></span>

### <a name="webconfig-location"></a><span data-ttu-id="140e8-171">local de web.config</span><span class="sxs-lookup"><span data-stu-id="140e8-171">web.config location</span></span>

<span data-ttu-id="140e8-172">Os aplicativos .NET Core são hospedados por meio de um proxy reverso entre o IIS e o servidor Kestrel.</span><span class="sxs-lookup"><span data-stu-id="140e8-172">.NET Core apps are hosted via a reverse proxy between IIS and the Kestrel server.</span></span> <span data-ttu-id="140e8-173">Para criar um proxy reverso, o arquivo *web.config* deve estar presente no caminho raiz do conteúdo (geralmente, o aplicativo base do caminho) do aplicativo implantado, que é o caminho físico do site fornecido ao IIS.</span><span class="sxs-lookup"><span data-stu-id="140e8-173">In order to create the reverse proxy, the *web.config* file must be present at the content root path (typically the app base path) of the deployed app, which is the website physical path provided to IIS.</span></span> <span data-ttu-id="140e8-174">O arquivo *web.config* é necessário na raiz do aplicativo para habilitar a publicação de vários aplicativos usando a Implantação da Web.</span><span class="sxs-lookup"><span data-stu-id="140e8-174">The *web.config* file is required at the root of the app to enable the publishing of multiple apps using Web Deploy.</span></span>

<span data-ttu-id="140e8-175">Existem arquivos confidenciais no caminho físico do aplicativo, incluindo subpastas, como *\<nome_do_assembly>.runtimeconfig.json*, *\<nome_do_assembly>.xml* (comentários da Documentação XML) e *\<nome_do_assembly>.deps.json*.</span><span class="sxs-lookup"><span data-stu-id="140e8-175">Sensitive files exist on the app's physical path, including subfolders, such as *\<assembly_name>.runtimeconfig.json*, *\<assembly_name>.xml* (XML Documentation comments), and *\<assembly_name>.deps.json*.</span></span> <span data-ttu-id="140e8-176">Quando o arquivo *web.config* está presente e configura o site, o IIS impede que esses arquivos confidenciais sejam servidos.</span><span class="sxs-lookup"><span data-stu-id="140e8-176">When the *web.config* file is present and configures the site, IIS prevents these sensitive files from being served.</span></span> <span data-ttu-id="140e8-177">**Portanto, é importante que o arquivo *web.config* não seja renomeado ou removido acidentalmente da implantação.**</span><span class="sxs-lookup"><span data-stu-id="140e8-177">**Therefore, it's important that the *web.config* file isn't accidently renamed or removed from the deployment.**</span></span>

## <a name="create-the-iis-website"></a><span data-ttu-id="140e8-178">Criar o site do IIS</span><span class="sxs-lookup"><span data-stu-id="140e8-178">Create the IIS Website</span></span>

1. <span data-ttu-id="140e8-179">No sistema do IIS de destino, crie uma pasta para conter as pastas e os arquivos publicados do aplicativo, que são descritos em [Estrutura de diretório](xref:host-and-deploy/directory-structure).</span><span class="sxs-lookup"><span data-stu-id="140e8-179">On the target IIS system, create a folder to contain the app's published folders and files, which are described in [Directory Structure](xref:host-and-deploy/directory-structure).</span></span>

2. <span data-ttu-id="140e8-180">Na pasta, crie uma pasta *logs* para armazenar os logs do stdout quando o log do stdout estiver habilitado.</span><span class="sxs-lookup"><span data-stu-id="140e8-180">Within the folder, create a *logs* folder to hold stdout logs when stdout logging is enabled.</span></span> <span data-ttu-id="140e8-181">Se o aplicativo for implantado com uma pasta *logs* no conteúdo, ignore esta etapa.</span><span class="sxs-lookup"><span data-stu-id="140e8-181">If the app is deployed with a *logs* folder in the payload, skip this step.</span></span> <span data-ttu-id="140e8-182">Para obter instruções sobre como fazer com que o MSBuild crie a pasta *logs*, consulte o tópico [Estrutura de diretórios](xref:host-and-deploy/directory-structure).</span><span class="sxs-lookup"><span data-stu-id="140e8-182">For instructions on how to make MSBuild create the *logs* folder, see the [Directory structure](xref:host-and-deploy/directory-structure) topic.</span></span>

3. <span data-ttu-id="140e8-183">Em **Gerenciador do IIS**, crie um novo site.</span><span class="sxs-lookup"><span data-stu-id="140e8-183">In **IIS Manager**, create a new website.</span></span> <span data-ttu-id="140e8-184">Forneça um **Nome do site** e defina o **Caminho físico** como a pasta de implantação do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="140e8-184">Provide a **Site name** and set the **Physical path** to the app's deployment folder.</span></span> <span data-ttu-id="140e8-185">Forneça a configuração **Associação** e crie o site.</span><span class="sxs-lookup"><span data-stu-id="140e8-185">Provide the **Binding** configuration and create the website.</span></span>

4. <span data-ttu-id="140e8-186">Defina o pool de aplicativos como **Sem Código Gerenciado**.</span><span class="sxs-lookup"><span data-stu-id="140e8-186">Set the application pool to **No Managed Code**.</span></span> <span data-ttu-id="140e8-187">O ASP.NET Core é executado em um processo separado e gerencia o tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="140e8-187">ASP.NET Core runs in a separate process and manages the runtime.</span></span>

5. <span data-ttu-id="140e8-188">Abra a janela **Adicionar Site**.</span><span class="sxs-lookup"><span data-stu-id="140e8-188">Open the **Add Website** window.</span></span>

   ![Selecione 'Adicionar Site' no menu contextual de Sites.](index/_static/add-website-context-menu-ws2016.png)

6. <span data-ttu-id="140e8-190">Configure o site.</span><span class="sxs-lookup"><span data-stu-id="140e8-190">Configure the website.</span></span>

   ![Forneça o Nome do site, o caminho físico e o Nome do host na etapa Adicionar Site.](index/_static/add-website-ws2016.png)

7. <span data-ttu-id="140e8-192">No painel **Pools de Aplicativos**, abra a janela **Editar pool de aplicativos** clicando com o botão direito do mouse no pool de aplicativos do site da Web e selecionando **Configurações Básicas...** no menu pop-up.</span><span class="sxs-lookup"><span data-stu-id="140e8-192">In the **Application Pools** panel, open the **Edit Application Pool** window by right-clicking on the website's app pool and selecting **Basic Settings...** from the popup menu.</span></span>

   ![Selecione Configurações Básicas no menu contextual do Pool de Aplicativos.](index/_static/apppools-basic-settings-ws2016.png)

8. <span data-ttu-id="140e8-194">Defina a **versão do CLR do .NET** como **Sem Código Gerenciado**.</span><span class="sxs-lookup"><span data-stu-id="140e8-194">Set the **.NET CLR version** to **No Managed Code**.</span></span>

   ![Defina Sem Código Gerenciado para a versão do CLR do .NET.](index/_static/edit-apppool-ws2016.png)
     
    <span data-ttu-id="140e8-196">Observação: a configuração da **versão do CLR do .NET** como **Sem Código Gerenciado** é opcional.</span><span class="sxs-lookup"><span data-stu-id="140e8-196">Note: Setting the **.NET CLR version** to **No Managed Code** is optional.</span></span> <span data-ttu-id="140e8-197">O ASP.NET Core não depende do carregamento do CLR de área de trabalho.</span><span class="sxs-lookup"><span data-stu-id="140e8-197">ASP.NET Core doesn't rely on loading the desktop CLR.</span></span>

9. <span data-ttu-id="140e8-198">Confirme se a identidade do modelo de processo tem as permissões apropriadas.</span><span class="sxs-lookup"><span data-stu-id="140e8-198">Confirm the process model identity has the proper permissions.</span></span>

   <span data-ttu-id="140e8-199">Se você alterar a identidade padrão do pool de aplicativos (**Modelo de Processo** > **Identidade**) em **ApplicationPoolIdentity** para outra, verifique se a nova identidade tem as permissões necessárias para acessar a pasta do aplicativo, o banco de dados e outros recursos necessários.</span><span class="sxs-lookup"><span data-stu-id="140e8-199">If the default identity of the app pool (**Process Model** > **Identity**) is changed from **ApplicationPoolIdentity** to another identity, verify that the new identity has the required permissions to access the app's folder, database, and other required resources.</span></span>
   
## <a name="deploy-the-app"></a><span data-ttu-id="140e8-200">Implantar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="140e8-200">Deploy the app</span></span>

<span data-ttu-id="140e8-201">Implante o aplicativo na pasta criada no sistema do IIS de destino.</span><span class="sxs-lookup"><span data-stu-id="140e8-201">Deploy the app to the folder created on the target IIS system.</span></span> <span data-ttu-id="140e8-202">A [Implantação da Web](/iis/publish/using-web-deploy/introduction-to-web-deploy) é o mecanismo recomendado para implantação.</span><span class="sxs-lookup"><span data-stu-id="140e8-202">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) is the recommended mechanism for deployment.</span></span>

<span data-ttu-id="140e8-203">Confirme se o aplicativo publicado para implantação não está em execução.</span><span class="sxs-lookup"><span data-stu-id="140e8-203">Confirm that the published app for deployment isn't running.</span></span> <span data-ttu-id="140e8-204">Os arquivos da pasta *publish* são bloqueados quando o aplicativo está em execução.</span><span class="sxs-lookup"><span data-stu-id="140e8-204">Files in the *publish* folder are locked when the app is running.</span></span> <span data-ttu-id="140e8-205">Arquivos bloqueados não podem ser substituídos.</span><span class="sxs-lookup"><span data-stu-id="140e8-205">Locked files can't be overwritten.</span></span> <span data-ttu-id="140e8-206">Para liberar os arquivos bloqueados em uma implantação, interrompa o pool de aplicativos:</span><span class="sxs-lookup"><span data-stu-id="140e8-206">To release locked files in a deployment, stop the app pool:</span></span>

* <span data-ttu-id="140e8-207">Manualmente no Gerenciador do IIS no servidor.</span><span class="sxs-lookup"><span data-stu-id="140e8-207">Manually in the IIS Manager on the server.</span></span>
* <span data-ttu-id="140e8-208">Usar a Implantação da Web e referenciar `Microsoft.NET.Sdk.Web` no arquivo de projeto.</span><span class="sxs-lookup"><span data-stu-id="140e8-208">Using Web Deploy and referencing `Microsoft.NET.Sdk.Web` in the project file.</span></span> <span data-ttu-id="140e8-209">Um arquivo *app_offline.htm* é colocado na raiz do diretório de aplicativo da Web.</span><span class="sxs-lookup"><span data-stu-id="140e8-209">An *app_offline.htm* file is placed at the root of the web app directory.</span></span> <span data-ttu-id="140e8-210">Quando o arquivo estiver presente, o módulo do ASP.NET Core apenas desligará o aplicativo e servirá o arquivo *app_offline.htm* durante a implantação.</span><span class="sxs-lookup"><span data-stu-id="140e8-210">When the file is present, the ASP.NET Core Module gracefully shuts down the app and serves the *app_offline.htm* file during the deployment.</span></span> <span data-ttu-id="140e8-211">Para obter mais informações, consulte [Referência de configuração do módulo do ASP.NET Core](xref:host-and-deploy/aspnet-core-module#appofflinehtm).</span><span class="sxs-lookup"><span data-stu-id="140e8-211">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#appofflinehtm).</span></span>
* <span data-ttu-id="140e8-212">Use o PowerShell para interromper e reiniciar o pool de aplicativos (requer o PowerShell 5 ou posterior):</span><span class="sxs-lookup"><span data-stu-id="140e8-212">Use PowerShell to stop and restart the app pool (requires PowerShell 5 or later):</span></span>
  ```PowerShell
  $webAppPoolName = 'APP_POOL_NAME'

  # Stop the AppPool
  if((Get-WebAppPoolState $webAppPoolName).Value -ne 'Stopped') {
      Stop-WebAppPool -Name $webAppPoolName
      while((Get-WebAppPoolState $webAppPoolName).Value -ne 'Stopped') {
          Start-Sleep -s 1
      }
      Write-Host `-AppPool Stopped
  }

  # Provide script commands here to deploy the app

  # Restart the AppPool
  if((Get-WebAppPoolState $webAppPoolName).Value -ne 'Started') {
      Start-WebAppPool -Name $webAppPoolName
      while((Get-WebAppPoolState $webAppPoolName).Value -ne 'Started') {
          Start-Sleep -s 1
      }
      Write-Host `-AppPool Started
  }
  ```

### <a name="web-deploy-with-visual-studio"></a><span data-ttu-id="140e8-213">Implantação da Web com o Visual Studio</span><span class="sxs-lookup"><span data-stu-id="140e8-213">Web Deploy with Visual Studio</span></span>

<span data-ttu-id="140e8-214">Consulte o tópico [Perfis de publicação do Visual Studio para implantação de aplicativos ASP.NET Core](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles) para saber como criar um perfil de publicação para uso com a Implantação da Web.</span><span class="sxs-lookup"><span data-stu-id="140e8-214">See the [Visual Studio publish profiles for ASP.NET Core app deployment](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles) topic to learn how to create a publish profile for use with Web Deploy.</span></span> <span data-ttu-id="140e8-215">Se o provedor de hospedagem fornecer um Perfil de Publicação ou o suporte para a criação de um, baixe o perfil e importe-o usando a caixa de diálogo **Publicar** do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="140e8-215">If the hosting provider supplies a Publish Profile or support for creating one, download their profile and import it using the Visual Studio **Publish** dialog.</span></span>

![Página de caixa de diálogo Publicar](index/_static/pub-dialog.png)

### <a name="web-deploy-outside-of-visual-studio"></a><span data-ttu-id="140e8-217">Implantação da Web fora do Visual Studio</span><span class="sxs-lookup"><span data-stu-id="140e8-217">Web Deploy outside of Visual Studio</span></span>

<span data-ttu-id="140e8-218">Também é possível usar a [Implantação da Web](/iis/publish/using-web-deploy/introduction-to-web-deploy) fora do Visual Studio na linha de comando.</span><span class="sxs-lookup"><span data-stu-id="140e8-218">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) can also be used outside of Visual Studio from the command line.</span></span> <span data-ttu-id="140e8-219">Para obter mais informações, consulte [Ferramenta de Implantação da Web](/iis/publish/using-web-deploy/use-the-web-deployment-tool).</span><span class="sxs-lookup"><span data-stu-id="140e8-219">For more information, see [Web Deployment Tool](/iis/publish/using-web-deploy/use-the-web-deployment-tool).</span></span>

### <a name="alternatives-to-web-deploy"></a><span data-ttu-id="140e8-220">Alternativas à Implantação da Web</span><span class="sxs-lookup"><span data-stu-id="140e8-220">Alternatives to Web Deploy</span></span>

<span data-ttu-id="140e8-221">Use qualquer um dos vários métodos para mover o aplicativo para o sistema host, como Xcopy, Robocopy ou PowerShell.</span><span class="sxs-lookup"><span data-stu-id="140e8-221">Use any of several methods to move the app to the hosting system, such as Xcopy, Robocopy, or PowerShell.</span></span> <span data-ttu-id="140e8-222">Os usuários do Visual Studio podem usar as [Amostras de Publicação](https://github.com/aspnet/vsweb-publish/blob/master/samples/samples.md).</span><span class="sxs-lookup"><span data-stu-id="140e8-222">Visual Studio users may use the [Publish Samples](https://github.com/aspnet/vsweb-publish/blob/master/samples/samples.md).</span></span>

## <a name="browse-the-website"></a><span data-ttu-id="140e8-223">Navegar no site</span><span class="sxs-lookup"><span data-stu-id="140e8-223">Browse the website</span></span>

![O navegador Microsoft Edge carregou a página de inicialização do IIS.](index/_static/browsewebsite.png)

## <a name="data-protection"></a><span data-ttu-id="140e8-225">Proteção de dados</span><span class="sxs-lookup"><span data-stu-id="140e8-225">Data protection</span></span>

<span data-ttu-id="140e8-226">A Proteção de Dados é usada por vários middlewares ASP.NET, incluindo aqueles usados na autenticação.</span><span class="sxs-lookup"><span data-stu-id="140e8-226">Data Protection is used by several ASP.NET middlewares, including those used in authentication.</span></span> <span data-ttu-id="140e8-227">Mesmo se APIs de proteção de dados não são chamadas do código do usuário, a proteção de dados deve ser configurada com um script de implantação ou no código do usuário para criar um repositório de chaves persistente.</span><span class="sxs-lookup"><span data-stu-id="140e8-227">Even if Data Protection APIs aren't called from user's code, Data Protection should be configured with a deployment script or in user code to create a persistent key store.</span></span> <span data-ttu-id="140e8-228">Se a proteção de dados não estiver configurada, as chaves serão mantidas na memória e descartadas quando o aplicativo for reiniciado.</span><span class="sxs-lookup"><span data-stu-id="140e8-228">If data protection isn't configured, the keys are held in memory and discarded when the app restarts.</span></span>

<span data-ttu-id="140e8-229">Se o token de autenticação for armazenado na memória, quando o aplicativo for reiniciado:</span><span class="sxs-lookup"><span data-stu-id="140e8-229">If the keyring is stored in memory when the app restarts:</span></span>

* <span data-ttu-id="140e8-230">Todos os tokens de autenticação de formulários serão inválidos.</span><span class="sxs-lookup"><span data-stu-id="140e8-230">All forms authentication tokens are invalidated.</span></span> 
* <span data-ttu-id="140e8-231">Os usuários precisam entrar novamente na próxima solicitação deles.</span><span class="sxs-lookup"><span data-stu-id="140e8-231">Users are required to sign in again on their next request.</span></span> 
* <span data-ttu-id="140e8-232">Todos os dados protegidos com o token de autenticação não podem mais ser descriptografados.</span><span class="sxs-lookup"><span data-stu-id="140e8-232">Any data protected with the keyring can no longer be decrypted.</span></span>

<span data-ttu-id="140e8-233">Para configurar a Proteção de dados no IIS, use **uma** das seguintes abordagens:</span><span class="sxs-lookup"><span data-stu-id="140e8-233">To configure Data Protection under IIS, use **one** of the following approaches:</span></span>

### <a name="create-a-data-protection-registry-hive"></a><span data-ttu-id="140e8-234">Criar um Hive do Registro de Proteção de Dados</span><span class="sxs-lookup"><span data-stu-id="140e8-234">Create a Data Protection Registry Hive</span></span>

<span data-ttu-id="140e8-235">As chaves de Proteção de Dados usadas pelos aplicativos ASP.NET são armazenadas em hives do Registro externos aos aplicativos.</span><span class="sxs-lookup"><span data-stu-id="140e8-235">Data Protection keys used by ASP.NET apps are stored in registry hives external to the apps.</span></span> <span data-ttu-id="140e8-236">Para persistir as chaves de determinado aplicativo, crie um hive do registro para o pool do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="140e8-236">To persist the keys for a given app, create a registry hive for the app pool.</span></span>

<span data-ttu-id="140e8-237">Para instalações autônomas do IIS não Web Farm, você pode usar o [script Provision-AutoGenKeys.ps1 de Proteção de Dados do PowerShell](https://github.com/aspnet/DataProtection/blob/dev/Provision-AutoGenKeys.ps1) para cada pool de aplicativos usado com um aplicativo ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="140e8-237">For standalone, non-webfarm IIS installations, the [Data Protection Provision-AutoGenKeys.ps1 PowerShell script](https://github.com/aspnet/DataProtection/blob/dev/Provision-AutoGenKeys.ps1) can be used for each app pool used with an ASP.NET Core app.</span></span> <span data-ttu-id="140e8-238">Esse script cria uma chave do registro especial no registro HKLM que tem a ACL acessível apenas para a conta do processo de trabalho.</span><span class="sxs-lookup"><span data-stu-id="140e8-238">This script creates a special registry key in the HKLM registry that's ACLed only to the worker process account.</span></span> <span data-ttu-id="140e8-239">As chaves são criptografadas em repouso usando a DPAPI com uma chave para todo o computador.</span><span class="sxs-lookup"><span data-stu-id="140e8-239">Keys are encrypted at rest using DPAPI with a machine-wide key.</span></span>

<span data-ttu-id="140e8-240">Em cenários de web farm, um aplicativo pode ser configurado para usar um caminho UNC para armazenar seu token de autenticação de proteção de dados.</span><span class="sxs-lookup"><span data-stu-id="140e8-240">In web farm scenarios, an app can be configured to use a UNC path to store its data protection keyring.</span></span> <span data-ttu-id="140e8-241">Por padrão, as chaves de proteção de dados não são criptografadas.</span><span class="sxs-lookup"><span data-stu-id="140e8-241">By default, the data protection keys are not encrypted.</span></span> <span data-ttu-id="140e8-242">Garanta que as permissões de arquivo de um compartilhamento como esse são limitadas à conta do Windows na qual o aplicativo é executado.</span><span class="sxs-lookup"><span data-stu-id="140e8-242">Ensure that the file permissions for such a share are limited to the Windows account the app runs as.</span></span> <span data-ttu-id="140e8-243">Além disso, um certificado X509 pode ser usado para proteger chaves em repouso.</span><span class="sxs-lookup"><span data-stu-id="140e8-243">In addition, an X509 certificate can be used to protect keys at rest.</span></span> <span data-ttu-id="140e8-244">Considere um mecanismo para permitir aos usuários carregar certificados: coloque os certificados no repositório de certificados confiáveis do usuário e certifique-se de que eles estejam disponíveis em todos os computadores nos quais o aplicativo do usuário é executado.</span><span class="sxs-lookup"><span data-stu-id="140e8-244">Consider a mechanism to allow users to upload certificates: Place certificates into the user's trusted certificate store and ensure they're available on all machines where the user's app runs.</span></span> <span data-ttu-id="140e8-245">Consulte [Configurando a Proteção de Dados](xref:security/data-protection/configuration/overview) para obter detalhes.</span><span class="sxs-lookup"><span data-stu-id="140e8-245">See [Configuring Data Protection](xref:security/data-protection/configuration/overview) for details.</span></span>

### <a name="configure-the-iis-application-pool-to-load-the-user-profile"></a><span data-ttu-id="140e8-246">Configurar o Pool de Aplicativos do IIS para carregar o perfil do usuário</span><span class="sxs-lookup"><span data-stu-id="140e8-246">Configure the IIS Application Pool to load the user profile</span></span>

<span data-ttu-id="140e8-247">Essa configuração está na seção **Modelo de processo** nas **Configurações avançadas** do pool de aplicativos.</span><span class="sxs-lookup"><span data-stu-id="140e8-247">This setting is in the **Process Model** section under the **Advanced Settings** for the app pool.</span></span> <span data-ttu-id="140e8-248">Defina Carregar perfil do usuário como `True`.</span><span class="sxs-lookup"><span data-stu-id="140e8-248">Set Load User Profile to `True`.</span></span> <span data-ttu-id="140e8-249">Isso armazena as chaves no diretório do perfil do usuário e as protege usando a DPAPI com uma chave específica à conta de usuário usada para o pool de aplicativos.</span><span class="sxs-lookup"><span data-stu-id="140e8-249">This stores keys under the user profile directory and protects them using DPAPI with a key specific to the user account used for the app pool.</span></span>

### <a name="use-the-file-system-as-a-key-ring-store"></a><span data-ttu-id="140e8-250">Usar o sistema de arquivos como um repositório de tokens de autenticação</span><span class="sxs-lookup"><span data-stu-id="140e8-250">Use the file system as a key ring store</span></span>

<span data-ttu-id="140e8-251">Ajuste o código do aplicativo para [usar o sistema de arquivos como um repositório de tokens de autenticação](xref:security/data-protection/configuration/overview).</span><span class="sxs-lookup"><span data-stu-id="140e8-251">Adjust the app code to [use the file system as a key ring store](xref:security/data-protection/configuration/overview).</span></span> <span data-ttu-id="140e8-252">Use um certificado X509 para proteger o token de autenticação e verifique se ele é um certificado confiável.</span><span class="sxs-lookup"><span data-stu-id="140e8-252">Use an X509 certificate to protect the keyring and ensure the certificate is a trusted certificate.</span></span> <span data-ttu-id="140e8-253">Se ele for um certificado autoassinado, você deverá colocá-lo no repositório Raiz confiável.</span><span class="sxs-lookup"><span data-stu-id="140e8-253">If it's a self signed certificate, place the certificate in the Trusted Root store.</span></span>

<span data-ttu-id="140e8-254">Ao usar o IIS em uma web farm:</span><span class="sxs-lookup"><span data-stu-id="140e8-254">When using IIS in a web farm:</span></span>

* <span data-ttu-id="140e8-255">Use um compartilhamento de arquivos que pode ser acessado por todos os computadores.</span><span class="sxs-lookup"><span data-stu-id="140e8-255">Use a file share that all machines can access.</span></span>
* <span data-ttu-id="140e8-256">Implante um certificado X509 em cada computador.</span><span class="sxs-lookup"><span data-stu-id="140e8-256">Deploy an X509 certificate to each machine.</span></span> <span data-ttu-id="140e8-257">Configure a [proteção de dados no código](xref:security/data-protection/configuration/overview).</span><span class="sxs-lookup"><span data-stu-id="140e8-257">Configure [data protection in code](xref:security/data-protection/configuration/overview).</span></span>

### <a name="set-a-machine-wide-policy-for-data-protection"></a><span data-ttu-id="140e8-258">Definir uma política para todo o computador para proteção de dados</span><span class="sxs-lookup"><span data-stu-id="140e8-258">Set a machine-wide policy for data protection</span></span>

<span data-ttu-id="140e8-259">O sistema de proteção de dados tem suporte limitado para a configuração da [política de todo o computador](xref:security/data-protection/configuration/machine-wide-policy) padrão para todos os aplicativos que consomem as APIs de proteção de dados.</span><span class="sxs-lookup"><span data-stu-id="140e8-259">The data protection system has limited support for setting a default [machine-wide policy](xref:security/data-protection/configuration/machine-wide-policy) for all apps that consume the Data Protection APIs.</span></span> <span data-ttu-id="140e8-260">Consulte a documentação de [proteção de dados](xref:security/data-protection/index) para obter mais detalhes.</span><span class="sxs-lookup"><span data-stu-id="140e8-260">See the [data protection](xref:security/data-protection/index) documentation for more details.</span></span>

## <a name="configuration-of-sub-applications"></a><span data-ttu-id="140e8-261">Configuração de subaplicativos</span><span class="sxs-lookup"><span data-stu-id="140e8-261">Configuration of sub-applications</span></span>

<span data-ttu-id="140e8-262">Os subaplicativos adicionados no aplicativo raiz não devem incluir o Módulo do ASP.NET Core como um manipulador.</span><span class="sxs-lookup"><span data-stu-id="140e8-262">Sub-apps added under the root app shouldn't include the ASP.NET Core Module as a handler.</span></span> <span data-ttu-id="140e8-263">Se o módulo for adicionado como um manipulador em um arquivo *web.config* de um subaplicativo, quando tentar procurar o subaplicativo, você receberá um 500.19 (Erro interno do servidor) que referenciará o arquivo de configuração com falha.</span><span class="sxs-lookup"><span data-stu-id="140e8-263">If the module is added as a handler in a sub-app's *web.config* file, a 500.19 (Internal Server Error) referencing the faulty config file is received when attempting to browse the sub-app.</span></span> <span data-ttu-id="140e8-264">O seguinte exemplo mostra o conteúdo de um arquivo *web.config* publicado de um subaplicativo ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="140e8-264">The following example shows the contents of a published *web.config* file for an ASP.NET Core sub-app:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <aspNetCore processPath="dotnet" 
      arguments=".\<assembly_name>.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

<span data-ttu-id="140e8-265">Ao hospedar um subaplicativo não ASP.NET Core abaixo de um aplicativo ASP.NET Core, remova explicitamente o manipulador herdado no arquivo *web.config* do subaplicativo:</span><span class="sxs-lookup"><span data-stu-id="140e8-265">When hosting a non-ASP.NET Core sub-app underneath an ASP.NET Core app, explicitly remove the inherited handler in the sub-app *web.config* file:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <remove name="aspNetCore"/>
    </handlers>
    <aspNetCore processPath="dotnet" 
      arguments=".\<assembly_name>.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

<span data-ttu-id="140e8-266">Para obter mais informações sobre como configurar o Módulo do ASP.NET Core, consulte o tópico [Introdução ao Módulo do ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) e a [Referência de configuração do Módulo do ASP.NET Core](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="140e8-266">For more information on configuring the ASP.NET Core Module, see the [Introduction to ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) topic and the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

## <a name="configuration-of-iis-with-webconfig"></a><span data-ttu-id="140e8-267">Configuração do IIS com web.config</span><span class="sxs-lookup"><span data-stu-id="140e8-267">Configuration of IIS with web.config</span></span>

<span data-ttu-id="140e8-268">A configuração do IIS é influenciada pela seção **\<system.webServer>** de *web.config* para os recursos do IIS que se aplicam a uma configuração de proxy reverso.</span><span class="sxs-lookup"><span data-stu-id="140e8-268">IIS configuration is influenced by the **\<system.webServer>** section of *web.config* for those IIS features that apply to a reverse proxy configuration.</span></span> <span data-ttu-id="140e8-269">Se o IIS é configurado no nível do sistema para usar a compactação dinâmica, essa configuração pode ser desabilitada para um aplicativo com o elemento **\<urlCompression>** no arquivo *web.config* do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="140e8-269">If IIS is configured at the system level to use dynamic compression, that setting can be disabled for an app with the **\<urlCompression>** element in the app's *web.config* file.</span></span> <span data-ttu-id="140e8-270">Para obter mais informações, consulte a [referência de configuração do \<system.webServer>](https://docs.microsoft.com/iis/configuration/system.webServer/), a [referência de configuração do Módulo do ASP.NET Core](xref:host-and-deploy/aspnet-core-module) e [Usando os Módulos do IIS com o ASP.NET Core](xref:host-and-deploy/iis/modules).</span><span class="sxs-lookup"><span data-stu-id="140e8-270">For more information, see the [configuration reference for \<system.webServer>](https://docs.microsoft.com/iis/configuration/system.webServer/), [ASP.NET Core Module Configuration Reference](xref:host-and-deploy/aspnet-core-module) and [Using IIS Modules with ASP.NET Core](xref:host-and-deploy/iis/modules).</span></span> <span data-ttu-id="140e8-271">Se precisar definir variáveis de ambiente para aplicativos individuais executados em pools de aplicativos isolados (compatível no IIS 10.0+), consulte a seção *comando AppCmd.exe* do tópico [Variáveis de ambiente \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) na documentação de referência do IIS.</span><span class="sxs-lookup"><span data-stu-id="140e8-271">If there's a need to set environment variables for individual apps running in isolated app pools (supported on IIS 10.0+), see the *AppCmd.exe command* section of the [Environment Variables \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic in the IIS reference documentation.</span></span>

## <a name="configuration-sections-of-webconfig"></a><span data-ttu-id="140e8-272">Seções de configuração de web.config</span><span class="sxs-lookup"><span data-stu-id="140e8-272">Configuration sections of web.config</span></span>

<span data-ttu-id="140e8-273">Seções de configuração de aplicativos ASP.NET Framework em *web.config* não são usados por aplicativos ASP.NET Core para configuração:</span><span class="sxs-lookup"><span data-stu-id="140e8-273">Configruation sections of ASP.NET Framework apps in *web.config* aren't used by ASP.NET Core apps for configuration:</span></span>

* <span data-ttu-id="140e8-274">**\<system.web>**</span><span class="sxs-lookup"><span data-stu-id="140e8-274">**\<system.web>**</span></span>
* <span data-ttu-id="140e8-275">**\<appSettings>**</span><span class="sxs-lookup"><span data-stu-id="140e8-275">**\<appSettings>**</span></span>
* <span data-ttu-id="140e8-276">**\<connectionStrings>**</span><span class="sxs-lookup"><span data-stu-id="140e8-276">**\<connectionStrings>**</span></span>
* <span data-ttu-id="140e8-277">**\<location>**</span><span class="sxs-lookup"><span data-stu-id="140e8-277">**\<location>**</span></span>

<span data-ttu-id="140e8-278">Aplicativos ASP.NET Core são configurados para usar outros provedores de configuração.</span><span class="sxs-lookup"><span data-stu-id="140e8-278">ASP.NET Core apps are configured using other configuration providers.</span></span> <span data-ttu-id="140e8-279">Para obter mais informações, consulte [Configuração](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="140e8-279">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

## <a name="application-pools"></a><span data-ttu-id="140e8-280">Pools de aplicativos</span><span class="sxs-lookup"><span data-stu-id="140e8-280">Application Pools</span></span>

<span data-ttu-id="140e8-281">Ao hospedar vários sites em um único sistema, isole os aplicativos entre si, executando cada aplicativo em seu próprio pool de aplicativos.</span><span class="sxs-lookup"><span data-stu-id="140e8-281">When hosting multiple websites on a single system, isolate the apps from each other by running each app in its own app pool.</span></span> <span data-ttu-id="140e8-282">A caixa de diálogo **Adicionar Site** do IIS usa como padrão essa configuração.</span><span class="sxs-lookup"><span data-stu-id="140e8-282">The IIS **Add Website** dialog defaults to this configuration.</span></span> <span data-ttu-id="140e8-283">Quando você fornece um **Nome do site**, o texto é transferido automaticamente para a caixa de texto **Pool de aplicativos**.</span><span class="sxs-lookup"><span data-stu-id="140e8-283">When **Site name** is provided, the text is automatically transferred to the **Application pool** textbox.</span></span> <span data-ttu-id="140e8-284">Um novo pool de aplicativos é criado usando o nome do site quando você adicionar o site.</span><span class="sxs-lookup"><span data-stu-id="140e8-284">A new app pool is created using the site name when the website is added.</span></span>

## <a name="application-pool-identity"></a><span data-ttu-id="140e8-285">Identidade do pool de aplicativos</span><span class="sxs-lookup"><span data-stu-id="140e8-285">Application Pool Identity</span></span>

<span data-ttu-id="140e8-286">Uma conta de identidade do pool de aplicativos permite executar um aplicativo em uma conta exclusiva sem a necessidade de criar e gerenciar domínios ou contas locais.</span><span class="sxs-lookup"><span data-stu-id="140e8-286">An app pool identity account allows an app to run under a unique account without having to create and manage domains or local accounts.</span></span> <span data-ttu-id="140e8-287">No IIS 8.0+, o WAS (Processo de trabalho do administrador) do IIS cria uma conta virtual com o nome do novo pool de aplicativos e executa os processos de trabalho do pool de aplicativos nesta conta por padrão.</span><span class="sxs-lookup"><span data-stu-id="140e8-287">On IIS 8.0+, the IIS Admin Worker Process (WAS) creates a virtual account with the name of the new app pool and runs the app pool's worker processes under this account by default.</span></span> <span data-ttu-id="140e8-288">No Console de Gerenciamento do IIS, em **Configurações avançadas** do pool de aplicativos, verifique se a **Identidade** é definida para usar **ApplicationPoolIdentity**:</span><span class="sxs-lookup"><span data-stu-id="140e8-288">In the IIS Management Console under **Advanced Settings** for the app pool, ensure that the **Identity** is set to use **ApplicationPoolIdentity**:</span></span>

![Caixa de diálogo Configurações avançadas do pool de aplicativos](index/_static/apppool-identity.png)

<span data-ttu-id="140e8-290">O processo de gerenciamento do IIS cria um identificador seguro com o nome do pool de aplicativos no Sistema de segurança do Windows.</span><span class="sxs-lookup"><span data-stu-id="140e8-290">The IIS management process creates a secure identifier with the name of the app pool in the Windows Security System.</span></span> <span data-ttu-id="140e8-291">Os recursos podem ser protegidos usando essa identidade; no entanto, essa identidade não é uma conta de usuário real e não será mostrada no Console de Gerenciamento de usuário do Windows.</span><span class="sxs-lookup"><span data-stu-id="140e8-291">Resources can be secured by using this identity; however, this identity isn't a real user account and won't show up in the Windows User Management Console.</span></span>

<span data-ttu-id="140e8-292">Se o processo de trabalho do IIS requerer acesso elevado ao aplicativo, modifique a ACL (lista de controle de acesso) do diretório que contém o aplicativo:</span><span class="sxs-lookup"><span data-stu-id="140e8-292">If the IIS worker process requires elevated access to the app, modify the Access Control List (ACL) for the directory containing the app:</span></span>

1. <span data-ttu-id="140e8-293">Abra o Windows Explorer e navegue para o diretório.</span><span class="sxs-lookup"><span data-stu-id="140e8-293">Open Windows Explorer and navigate to the directory.</span></span>

2. <span data-ttu-id="140e8-294">Clique com o botão direito do mouse no diretório e selecione **Propriedades**.</span><span class="sxs-lookup"><span data-stu-id="140e8-294">Right-click on the directory and select **Properties**.</span></span>

3. <span data-ttu-id="140e8-295">Na guia **Segurança**, selecione o botão **Editar** e, em seguida, no botão **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="140e8-295">Under the **Security** tab, select the **Edit** button and then the **Add** button.</span></span>

4. <span data-ttu-id="140e8-296">Clique no botão **Locais** e verifique se o sistema está selecionado.</span><span class="sxs-lookup"><span data-stu-id="140e8-296">Select the **Locations** button and make sure the system is selected.</span></span>

5. <span data-ttu-id="140e8-297">Insira **IIS AppPool\DefaultAppPool** na caixa de texto **Inserir os nomes de objeto a serem selecionados**.</span><span class="sxs-lookup"><span data-stu-id="140e8-297">Enter **IIS AppPool\DefaultAppPool** in **Enter the object names to select** textbox.</span></span>

   ![Caixa de diálogo Selecionar usuários ou grupos da pasta do aplicativo](index/_static/select-users-or-groups-1.png)

6. <span data-ttu-id="140e8-299">Selecione o botão **Verificar Nomes**.</span><span class="sxs-lookup"><span data-stu-id="140e8-299">Select the **Check Names** button.</span></span> <span data-ttu-id="140e8-300">Selecione **OK**.</span><span class="sxs-lookup"><span data-stu-id="140e8-300">Select **OK**.</span></span>

   ![Caixa de diálogo Selecionar usuários ou grupos da pasta do aplicativo](index/_static/select-users-or-groups-2.png)

<span data-ttu-id="140e8-302">Isso também pode ser feito por meio de um prompt de comando usando a ferramenta **ICACLS**:</span><span class="sxs-lookup"><span data-stu-id="140e8-302">This can also be accomplished via a command prompt using the **ICACLS** tool:</span></span>

```console
ICACLS C:\sites\MyWebApp /grant "IIS AppPool\DefaultAppPool":F
```

## <a name="additional-resources"></a><span data-ttu-id="140e8-303">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="140e8-303">Additional resources</span></span>

* [<span data-ttu-id="140e8-304">Solucionar problemas do ASP.NET Core no IIS</span><span class="sxs-lookup"><span data-stu-id="140e8-304">Troubleshoot ASP.NET Core on IIS</span></span>](xref:host-and-deploy/iis/troubleshoot)
* [<span data-ttu-id="140e8-305">Referência de erros comuns para o Serviço de Aplicativo do Azure e o IIS com o ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="140e8-305">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)
* [<span data-ttu-id="140e8-306">Introdução ao Módulo do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="140e8-306">Introduction to ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* [<span data-ttu-id="140e8-307">Referência de configuração do Módulo do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="140e8-307">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
* [<span data-ttu-id="140e8-308">Usando Módulos do IIS com o ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="140e8-308">Using IIS Modules with ASP.NET Core</span></span>](xref:host-and-deploy/iis/modules)
* [<span data-ttu-id="140e8-309">Introdução ao ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="140e8-309">Introduction to ASP.NET Core</span></span>](../index.md)
* [<span data-ttu-id="140e8-310">O site oficial da IIS da Microsoft</span><span class="sxs-lookup"><span data-stu-id="140e8-310">The Official Microsoft IIS Site</span></span>](https://www.iis.net/)
* [<span data-ttu-id="140e8-311">Biblioteca Microsoft TechNet: Windows Server</span><span class="sxs-lookup"><span data-stu-id="140e8-311">Microsoft TechNet Library: Windows Server</span></span>](https://docs.microsoft.com/windows-server/windows-server-versions)

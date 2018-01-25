---
title: "Trabalhando com vários ambientes no núcleo do ASP.NET"
author: rick-anderson
description: "Saiba como o ASP.NET Core fornece suporte para controlar o comportamento do aplicativo em vários ambientes."
ms.author: riande
manager: wpickett
ms.date: 12/25/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/environments
ms.openlocfilehash: 60a1543ce11d08490e6df0eb84f980672ecfe672
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
# <a name="working-with-multiple-environments"></a><span data-ttu-id="7de6d-103">Trabalhando com vários ambientes</span><span class="sxs-lookup"><span data-stu-id="7de6d-103">Working with multiple environments</span></span>

<span data-ttu-id="7de6d-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="7de6d-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="7de6d-105">ASP.NET Core fornece suporte para a configuração de comportamento do aplicativo em tempo de execução com variáveis de ambiente.</span><span class="sxs-lookup"><span data-stu-id="7de6d-105">ASP.NET Core provides support for setting application behavior at runtime with environment variables.</span></span>

<span data-ttu-id="7de6d-106">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="7de6d-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="environments"></a><span data-ttu-id="7de6d-107">Ambientes</span><span class="sxs-lookup"><span data-stu-id="7de6d-107">Environments</span></span>

<span data-ttu-id="7de6d-108">ASP.NET Core lê a variável de ambiente `ASPNETCORE_ENVIRONMENT` na inicialização do aplicativo e armazena esse valor em [IHostingEnvironment.EnvironmentName](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName).</span><span class="sxs-lookup"><span data-stu-id="7de6d-108">ASP.NET Core reads the environment variable `ASPNETCORE_ENVIRONMENT` at application startup and stores that value in [IHostingEnvironment.EnvironmentName](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName).</span></span> <span data-ttu-id="7de6d-109">`ASPNETCORE_ENVIRONMENT`pode ser definida como qualquer valor, mas [três valores](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname?view=aspnetcore-2.0) com suporte do framework: [desenvolvimento](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development?view=aspnetcore-2.0), [preparo](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging?view=aspnetcore-2.0), e [produção](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="7de6d-109">`ASPNETCORE_ENVIRONMENT` can be set to any value, but [three values](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname?view=aspnetcore-2.0) are supported by the framework: [Development](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development?view=aspnetcore-2.0), [Staging](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging?view=aspnetcore-2.0), and [Production](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production?view=aspnetcore-2.0).</span></span> <span data-ttu-id="7de6d-110">Se `ASPNETCORE_ENVIRONMENT` não está definida, o padrão será `Production`.</span><span class="sxs-lookup"><span data-stu-id="7de6d-110">If `ASPNETCORE_ENVIRONMENT` isn't set, it will default to `Production`.</span></span>

[!code-csharp[Main](environments/sample/WebApp1/Startup.cs?name=snippet)]

<span data-ttu-id="7de6d-111">O código anterior:</span><span class="sxs-lookup"><span data-stu-id="7de6d-111">The preceding code:</span></span>

* <span data-ttu-id="7de6d-112">Chamadas [UseDeveloperExceptionPage](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_DeveloperExceptionPageExtensions_UseDeveloperExceptionPage_Microsoft_AspNetCore_Builder_IApplicationBuilder_) e [UseBrowserLink](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_BrowserLinkExtensions_UseBrowserLink_Microsoft_AspNetCore_Builder_IApplicationBuilder_) quando `ASPNETCORE_ENVIRONMENT` é definido como `Development`.</span><span class="sxs-lookup"><span data-stu-id="7de6d-112">Calls [UseDeveloperExceptionPage](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_DeveloperExceptionPageExtensions_UseDeveloperExceptionPage_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [UseBrowserLink](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_BrowserLinkExtensions_UseBrowserLink_Microsoft_AspNetCore_Builder_IApplicationBuilder_) when `ASPNETCORE_ENVIRONMENT` is set to `Development`.</span></span>
* <span data-ttu-id="7de6d-113">Chamadas [UseExceptionHandler](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ExceptionHandlerExtensions_UseExceptionHandler_Microsoft_AspNetCore_Builder_IApplicationBuilder_) quando o valor de `ASPNETCORE_ENVIRONMENT` é definido um dos seguintes:</span><span class="sxs-lookup"><span data-stu-id="7de6d-113">Calls [UseExceptionHandler](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ExceptionHandlerExtensions_UseExceptionHandler_Microsoft_AspNetCore_Builder_IApplicationBuilder_) when the value of `ASPNETCORE_ENVIRONMENT` is set one of the following:</span></span>

    * `Staging`
    * `Production`
    * `Staging_2`

<span data-ttu-id="7de6d-114">O [auxiliar de marca de ambiente ](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) usa o valor de `IHostingEnvironment.EnvironmentName` para incluir ou excluir marcação no elemento:</span><span class="sxs-lookup"><span data-stu-id="7de6d-114">The [Environment Tag Helper ](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) uses the value of `IHostingEnvironment.EnvironmentName` to include or exclude markup in the element:</span></span>

[!code-html[Main](environments/sample/WebApp1/Pages/About.cshtml)]

<span data-ttu-id="7de6d-115">Observação: No Windows e macOS, valores e variáveis de ambiente não são diferencia maiusculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="7de6d-115">Note: On Windows and macOS, environment variables and values are not case sensitive.</span></span> <span data-ttu-id="7de6d-116">Variáveis de ambiente do Linux e os valores são **diferencia maiusculas de minúsculas** por padrão.</span><span class="sxs-lookup"><span data-stu-id="7de6d-116">Linux environment variables and values are **case sensitive** by default.</span></span>

### <a name="development"></a><span data-ttu-id="7de6d-117">Desenvolvimento</span><span class="sxs-lookup"><span data-stu-id="7de6d-117">Development</span></span>

<span data-ttu-id="7de6d-118">O ambiente de desenvolvimento pode habilitar recursos que não devem ser expostos na produção.</span><span class="sxs-lookup"><span data-stu-id="7de6d-118">The development environment can enable features that shouldn't be exposed in production.</span></span> <span data-ttu-id="7de6d-119">Por exemplo, os modelos do ASP.NET Core habilitam o [página de exceção de desenvolvedor](xref:fundamentals/error-handling#the-developer-exception-page) no ambiente de desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="7de6d-119">For example, the ASP.NET Core templates enable the [developer exception page](xref:fundamentals/error-handling#the-developer-exception-page) in the development environment.</span></span>

<span data-ttu-id="7de6d-120">O ambiente de desenvolvimento do computador local pode ser definido na *Properties\launchSettings.json* arquivo do projeto.</span><span class="sxs-lookup"><span data-stu-id="7de6d-120">The environment for local machine development can be set in the *Properties\launchSettings.json* file of the project.</span></span> <span data-ttu-id="7de6d-121">Definir valores de ambiente em *launchSettings.json* substituir valores definidos no ambiente do sistema.</span><span class="sxs-lookup"><span data-stu-id="7de6d-121">Environment values set in *launchSettings.json* override values set in the system environment.</span></span>

<span data-ttu-id="7de6d-122">O XML a seguir mostra três perfis de um *launchSettings.json* arquivo:</span><span class="sxs-lookup"><span data-stu-id="7de6d-122">The following XML shows three profiles from a *launchSettings.json* file:</span></span>

[!code-xml[Main](environments/sample/WebApp1/Properties/launchSettings.json?highlight=10,11,18,26)]

<span data-ttu-id="7de6d-123">Quando o aplicativo é iniciado com `dotnet run`, o primeiro perfil com `"commandName": "Project"` será usado.</span><span class="sxs-lookup"><span data-stu-id="7de6d-123">When the application is launched with `dotnet run`, the first profile with `"commandName": "Project"` will be used.</span></span> <span data-ttu-id="7de6d-124">O valor de `commandName` Especifica o servidor web para iniciar.</span><span class="sxs-lookup"><span data-stu-id="7de6d-124">The value of `commandName` specifies the web server to launch.</span></span> <span data-ttu-id="7de6d-125">`commandName`pode ser um destes:</span><span class="sxs-lookup"><span data-stu-id="7de6d-125">`commandName` can be one of :</span></span>

* <span data-ttu-id="7de6d-126">IIS Express</span><span class="sxs-lookup"><span data-stu-id="7de6d-126">IIS Express</span></span>
* <span data-ttu-id="7de6d-127">IIS</span><span class="sxs-lookup"><span data-stu-id="7de6d-127">IIS</span></span>
* <span data-ttu-id="7de6d-128">Projeto (o que inicia Kestrel)</span><span class="sxs-lookup"><span data-stu-id="7de6d-128">Project (which launches Kestrel)</span></span>

<span data-ttu-id="7de6d-129">Quando um aplicativo é iniciado com `dotnet run`:</span><span class="sxs-lookup"><span data-stu-id="7de6d-129">When an app is launched with `dotnet run`:</span></span>

* <span data-ttu-id="7de6d-130">*launchSettings.json* é lido se disponível.</span><span class="sxs-lookup"><span data-stu-id="7de6d-130">*launchSettings.json* is read if available.</span></span> <span data-ttu-id="7de6d-131">`environmentVariables`as configurações no *launchSettings.json* substituir variáveis de ambiente.</span><span class="sxs-lookup"><span data-stu-id="7de6d-131">`environmentVariables` settings in *launchSettings.json* override environment variables.</span></span>
* <span data-ttu-id="7de6d-132">O ambiente de hospedagem é exibido.</span><span class="sxs-lookup"><span data-stu-id="7de6d-132">The hosting environment is displayed.</span></span>


<span data-ttu-id="7de6d-133">A saída a seguir mostra um aplicativo iniciado com `dotnet run`:</span><span class="sxs-lookup"><span data-stu-id="7de6d-133">The following output shows an app started with `dotnet run`:</span></span>
```bash
PS C:\Webs\WebApp1> dotnet run
Using launch settings from C:\Webs\WebApp1\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Webs\WebApp1
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="7de6d-134">O Visual Studio **depurar** guia fornece uma interface gráfica do usuário para editar o *launchSettings.json* arquivo:</span><span class="sxs-lookup"><span data-stu-id="7de6d-134">The Visual Studio **Debug** tab provides a GUI to edit the *launchSettings.json* file:</span></span>

![Variáveis de ambiente de configuração de propriedades do projeto](environments/_static/project-properties-debug.png)

<span data-ttu-id="7de6d-136">As alterações feitas aos perfis de projeto não podem ser aplicadas até que o servidor web seja reiniciado.</span><span class="sxs-lookup"><span data-stu-id="7de6d-136">Changes made to project profiles may not take effect until the web server is restarted.</span></span> <span data-ttu-id="7de6d-137">Kestrel deve ser reiniciado antes de ele detectará as alterações feitas em seu ambiente.</span><span class="sxs-lookup"><span data-stu-id="7de6d-137">Kestrel must be restarted before it will detect changes made to its environment.</span></span>

>[!WARNING]
> <span data-ttu-id="7de6d-138">*launchSettings.json* não deve armazenar segredos.</span><span class="sxs-lookup"><span data-stu-id="7de6d-138">*launchSettings.json* shouldn't store secrets.</span></span> <span data-ttu-id="7de6d-139">O [ferramenta Gerenciador de segredo](xref:security/app-secrets) pode ser usado para armazenar segredos de desenvolvimento local.</span><span class="sxs-lookup"><span data-stu-id="7de6d-139">The [Secret Manager tool](xref:security/app-secrets) can be used to store secrets for local development.</span></span>

### <a name="production"></a><span data-ttu-id="7de6d-140">Produção</span><span class="sxs-lookup"><span data-stu-id="7de6d-140">Production</span></span>

<span data-ttu-id="7de6d-141">O ambiente de produção deve ser configurado para maximizar a segurança, desempenho e eficiência do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="7de6d-141">The production environment should be configured to maximize security, performance, and application robustness.</span></span> <span data-ttu-id="7de6d-142">Algumas configurações comuns que pode ter um ambiente de produção que seriam diferentes de desenvolvimento incluem:</span><span class="sxs-lookup"><span data-stu-id="7de6d-142">Some common settings that a production environment might have that would differ from development include:</span></span>

* <span data-ttu-id="7de6d-143">Armazenamento em cache.</span><span class="sxs-lookup"><span data-stu-id="7de6d-143">Caching.</span></span>
* <span data-ttu-id="7de6d-144">Recursos do cliente são agrupados, minimizados e potencialmente atendidos a partir de uma CDN.</span><span class="sxs-lookup"><span data-stu-id="7de6d-144">Client-side resources are bundled, minified, and potentially served from a CDN.</span></span>
* <span data-ttu-id="7de6d-145">Páginas de erro de diagnóstico desabilitadas.</span><span class="sxs-lookup"><span data-stu-id="7de6d-145">Diagnostic error pages disabled.</span></span>
* <span data-ttu-id="7de6d-146">Páginas de erro amigável habilitadas.</span><span class="sxs-lookup"><span data-stu-id="7de6d-146">Friendly error pages enabled.</span></span>
* <span data-ttu-id="7de6d-147">Log de produção e o monitoramento habilitado.</span><span class="sxs-lookup"><span data-stu-id="7de6d-147">Production logging and monitoring enabled.</span></span> <span data-ttu-id="7de6d-148">Por exemplo, [Application Insights](https://azure.microsoft.com/documentation/articles/app-insights-asp-net-five/).</span><span class="sxs-lookup"><span data-stu-id="7de6d-148">For example, [Application Insights](https://azure.microsoft.com/documentation/articles/app-insights-asp-net-five/).</span></span>

## <a name="setting-the-environment"></a><span data-ttu-id="7de6d-149">Configurando o ambiente</span><span class="sxs-lookup"><span data-stu-id="7de6d-149">Setting the environment</span></span>

<span data-ttu-id="7de6d-150">Geralmente é útil definir um ambiente específico para teste.</span><span class="sxs-lookup"><span data-stu-id="7de6d-150">It's often useful to set a specific environment for testing.</span></span> <span data-ttu-id="7de6d-151">Se o ambiente não for definido, o padrão será `Production` que desativa a maioria dos recursos de depuração.</span><span class="sxs-lookup"><span data-stu-id="7de6d-151">If the environment isn't set, it will default to `Production` which disables most debugging features.</span></span>

<span data-ttu-id="7de6d-152">O método para definir o ambiente depende do sistema operacional.</span><span class="sxs-lookup"><span data-stu-id="7de6d-152">The method for setting the environment depends on the operating system.</span></span>

### <a name="azure"></a><span data-ttu-id="7de6d-153">Azure</span><span class="sxs-lookup"><span data-stu-id="7de6d-153">Azure</span></span>

<span data-ttu-id="7de6d-154">Para o serviço de aplicativo do Azure:</span><span class="sxs-lookup"><span data-stu-id="7de6d-154">For Azure app service:</span></span>

* <span data-ttu-id="7de6d-155">Selecione o **configurações de aplicativo** folha.</span><span class="sxs-lookup"><span data-stu-id="7de6d-155">Select the **Application settings** blade.</span></span>
* <span data-ttu-id="7de6d-156">Adicionar a chave e valor em **configurações do aplicativo**.</span><span class="sxs-lookup"><span data-stu-id="7de6d-156">Add the key and value in **App settings**.</span></span>


### <a name="windows"></a><span data-ttu-id="7de6d-157">Windows</span><span class="sxs-lookup"><span data-stu-id="7de6d-157">Windows</span></span>
<span data-ttu-id="7de6d-158">Para definir o `ASPNETCORE_ENVIRONMENT` para a sessão atual, se o aplicativo é iniciado usando `dotnet run`, os comandos a seguir são usados</span><span class="sxs-lookup"><span data-stu-id="7de6d-158">To set the `ASPNETCORE_ENVIRONMENT` for the current session, if the app is started using `dotnet run`, the following commands are used</span></span>

<span data-ttu-id="7de6d-159">**Linha de comando**</span><span class="sxs-lookup"><span data-stu-id="7de6d-159">**Command line**</span></span>
```
set ASPNETCORE_ENVIRONMENT=Development
```
<span data-ttu-id="7de6d-160">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="7de6d-160">**PowerShell**</span></span>
```
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

<span data-ttu-id="7de6d-161">Esses comandos têm efeito somente para a janela atual.</span><span class="sxs-lookup"><span data-stu-id="7de6d-161">These commands take effect only for the current window.</span></span> <span data-ttu-id="7de6d-162">Quando a janela for fechada, a configuração ASPNETCORE_ENVIRONMENT é revertida para a configuração padrão ou o valor de máquina.</span><span class="sxs-lookup"><span data-stu-id="7de6d-162">When the window is closed, the ASPNETCORE_ENVIRONMENT setting reverts to the default setting or machine value.</span></span> <span data-ttu-id="7de6d-163">Para definir o valor global em janelas abertas do **painel de controle** > **sistema** > **configurações avançadas do sistema** e adicionar ou editar o `ASPNETCORE_ENVIRONMENT` valor.</span><span class="sxs-lookup"><span data-stu-id="7de6d-163">In order to set the value globally on Windows open the **Control Panel** > **System** > **Advanced system settings** and add or edit the `ASPNETCORE_ENVIRONMENT` value.</span></span>

![Propriedades avançadas do sistema](environments/_static/systemsetting_environment.png)

![Variável de ambiente do ASPNET Core](environments/_static/windows_aspnetcore_environment.png)


<span data-ttu-id="7de6d-166">**web.config**</span><span class="sxs-lookup"><span data-stu-id="7de6d-166">**web.config**</span></span>

<span data-ttu-id="7de6d-167">Consulte o *definir variáveis de ambiente* seção o [referência de configuração do módulo do ASP.NET Core](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) tópico.</span><span class="sxs-lookup"><span data-stu-id="7de6d-167">See the *Setting environment variables* section of the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) topic.</span></span>

<span data-ttu-id="7de6d-168">**Por Pool de aplicativos do IIS**</span><span class="sxs-lookup"><span data-stu-id="7de6d-168">**Per IIS Application Pool**</span></span>

<span data-ttu-id="7de6d-169">Para definir variáveis de ambiente para aplicativos individuais executados em Pools de aplicativos isolados (com suporte no IIS 10.0 +), consulte o *comando AppCmd.exe* seção o [variáveis de ambiente \< environmentVariables >](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) tópico.</span><span class="sxs-lookup"><span data-stu-id="7de6d-169">To set environment variables for individual apps running in isolated Application Pools (supported on IIS 10.0+), see the *AppCmd.exe command* section of the [Environment Variables \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic.</span></span>

### <a name="macos"></a><span data-ttu-id="7de6d-170">macOS</span><span class="sxs-lookup"><span data-stu-id="7de6d-170">macOS</span></span>
<span data-ttu-id="7de6d-171">Configurar o ambiente atual para macOS pode ser feito na linha ao executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="7de6d-171">Setting the current environment for macOS can be done in-line when running the application;</span></span>

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```
<span data-ttu-id="7de6d-172">ou usar `export` para defini-lo antes de executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="7de6d-172">or using `export` to set it prior to running the app.</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```
<span data-ttu-id="7de6d-173">Variáveis de ambiente de nível de máquina são definidas no *. bashrc* ou *. bash_profile* arquivo.</span><span class="sxs-lookup"><span data-stu-id="7de6d-173">Machine level environment variables are set in the *.bashrc* or *.bash_profile* file.</span></span> <span data-ttu-id="7de6d-174">Editar o arquivo usando qualquer editor de texto e adicione a seguinte instrução.</span><span class="sxs-lookup"><span data-stu-id="7de6d-174">Edit the file using any text editor and add the following statment.</span></span>

```
export ASPNETCORE_ENVIRONMENT=Development
```

### <a name="linux"></a><span data-ttu-id="7de6d-175">Linux</span><span class="sxs-lookup"><span data-stu-id="7de6d-175">Linux</span></span>
<span data-ttu-id="7de6d-176">Para distribuições de Linux, use o `export` para sessão com base em configurações de variável de comando na linha de comando e *bash_profile* arquivo de configurações de ambiente de nível de máquina.</span><span class="sxs-lookup"><span data-stu-id="7de6d-176">For Linux distros, use the `export` command at the command line for session based variable settings and *bash_profile* file for machine level environment settings.</span></span>

### <a name="configuration-by-environment"></a><span data-ttu-id="7de6d-177">Configuração por ambiente</span><span class="sxs-lookup"><span data-stu-id="7de6d-177">Configuration by environment</span></span>

<span data-ttu-id="7de6d-178">Consulte [configuração pelo ambiente](xref:fundamentals/configuration/index#configuration-by-environment) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="7de6d-178">See [Configuration by environment](xref:fundamentals/configuration/index#configuration-by-environment) for more information.</span></span>

<a name="startup-conventions"></a>
## <a name="environment-based-startup-class-and-methods"></a><span data-ttu-id="7de6d-179">Ambiente com base em métodos e classe de inicialização</span><span class="sxs-lookup"><span data-stu-id="7de6d-179">Environment based Startup class and methods</span></span>

<span data-ttu-id="7de6d-180">Quando um aplicativo do ASP.NET Core é iniciado, o [classe inicialização](xref:fundamentals/startup) inicializa o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="7de6d-180">When an ASP.NET Core app starts, the [Startup class](xref:fundamentals/startup) bootstraps the app.</span></span> <span data-ttu-id="7de6d-181">Se uma classe `Startup{EnvironmentName}` existe, se a classe será chamada para que `EnvironmentName`:</span><span class="sxs-lookup"><span data-stu-id="7de6d-181">If a class `Startup{EnvironmentName}` exists, that class will be called for that `EnvironmentName`:</span></span>

[!code-csharp[Main](environments/sample/WebApp1/StartupDev.cs?name=snippet&highlight=1)]

<span data-ttu-id="7de6d-182">Observação: Ao chamar [WebHostBuilder.UseStartup<TStartup> ](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) substitui seções de configuração.</span><span class="sxs-lookup"><span data-stu-id="7de6d-182">Note: Calling [WebHostBuilder.UseStartup<TStartup>](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) overrides configuration sections.</span></span>

<span data-ttu-id="7de6d-183">[Configurar](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_StartupBase_Configure_Microsoft_AspNetCore_Builder_IApplicationBuilder_) e [ConfigureServices](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices?view=aspnetcore-2.0) suporte a versões específicas do ambiente do formulário `Configure{EnvironmentName}` e `Configure{EnvironmentName}Services`:</span><span class="sxs-lookup"><span data-stu-id="7de6d-183">[Configure](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_StartupBase_Configure_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [ConfigureServices](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices?view=aspnetcore-2.0) support environment specific versions of the form `Configure{EnvironmentName}` and `Configure{EnvironmentName}Services`:</span></span>

[!code-csharp[Main](environments/sample/WebApp1/Startup.cs?name=snippet_all&highlight=15,37)]

## <a name="additional-resources"></a><span data-ttu-id="7de6d-184">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="7de6d-184">Additional Resources</span></span>

* [<span data-ttu-id="7de6d-185">Inicialização de aplicativos</span><span class="sxs-lookup"><span data-stu-id="7de6d-185">Application startup</span></span>](xref:fundamentals/startup)
* [<span data-ttu-id="7de6d-186">Configuração</span><span class="sxs-lookup"><span data-stu-id="7de6d-186">Configuration</span></span>](xref:fundamentals/configuration/index)
* [<span data-ttu-id="7de6d-187">IHostingEnvironment.EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="7de6d-187">IHostingEnvironment.EnvironmentName</span></span>](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName)

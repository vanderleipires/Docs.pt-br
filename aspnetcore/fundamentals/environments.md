---
title: "Trabalhando com vários ambientes no ASP.NET Core"
author: rick-anderson
description: "Saiba como o ASP.NET Core fornece suporte para controlar o comportamento do aplicativo em vários ambientes."
manager: wpickett
ms.author: riande
ms.date: 12/25/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/environments
ms.openlocfilehash: b40ee9b1c6feae4942f05d22dab776d3cf6c26a0
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/30/2018
---
# <a name="working-with-multiple-environments"></a><span data-ttu-id="393bb-103">Trabalhando com vários ambientes</span><span class="sxs-lookup"><span data-stu-id="393bb-103">Working with multiple environments</span></span>

<span data-ttu-id="393bb-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="393bb-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="393bb-105">O ASP.NET Core fornece suporte para a configuração do comportamento do aplicativo em tempo de execução com variáveis de ambiente.</span><span class="sxs-lookup"><span data-stu-id="393bb-105">ASP.NET Core provides support for setting application behavior at runtime with environment variables.</span></span>

<span data-ttu-id="393bb-106">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="393bb-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="environments"></a><span data-ttu-id="393bb-107">Ambientes</span><span class="sxs-lookup"><span data-stu-id="393bb-107">Environments</span></span>

<span data-ttu-id="393bb-108">O ASP.NET Core lê a variável de ambiente `ASPNETCORE_ENVIRONMENT` na inicialização do aplicativo e armazena esse valor em [IHostingEnvironment.EnvironmentName](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName).</span><span class="sxs-lookup"><span data-stu-id="393bb-108">ASP.NET Core reads the environment variable `ASPNETCORE_ENVIRONMENT` at application startup and stores that value in [IHostingEnvironment.EnvironmentName](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName).</span></span> <span data-ttu-id="393bb-109">`ASPNETCORE_ENVIRONMENT` pode ser definido com qualquer valor, mas há suporte para [três valores](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname?view=aspnetcore-2.0) na estrutura: [Desenvolvimento](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development?view=aspnetcore-2.0), [Preparo](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging?view=aspnetcore-2.0) e [Produção](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="393bb-109">`ASPNETCORE_ENVIRONMENT` can be set to any value, but [three values](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname?view=aspnetcore-2.0) are supported by the framework: [Development](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development?view=aspnetcore-2.0), [Staging](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging?view=aspnetcore-2.0), and [Production](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production?view=aspnetcore-2.0).</span></span> <span data-ttu-id="393bb-110">Se `ASPNETCORE_ENVIRONMENT` não estiver definido, ele usará `Production` como padrão.</span><span class="sxs-lookup"><span data-stu-id="393bb-110">If `ASPNETCORE_ENVIRONMENT` isn't set, it will default to `Production`.</span></span>

[!code-csharp[Main](environments/sample/WebApp1/Startup.cs?name=snippet)]

<span data-ttu-id="393bb-111">O código anterior:</span><span class="sxs-lookup"><span data-stu-id="393bb-111">The preceding code:</span></span>

* <span data-ttu-id="393bb-112">Chama [UseDeveloperExceptionPage](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_DeveloperExceptionPageExtensions_UseDeveloperExceptionPage_Microsoft_AspNetCore_Builder_IApplicationBuilder_) e [UseBrowserLink](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_BrowserLinkExtensions_UseBrowserLink_Microsoft_AspNetCore_Builder_IApplicationBuilder_) quando `ASPNETCORE_ENVIRONMENT` é definido como `Development`.</span><span class="sxs-lookup"><span data-stu-id="393bb-112">Calls [UseDeveloperExceptionPage](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_DeveloperExceptionPageExtensions_UseDeveloperExceptionPage_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [UseBrowserLink](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_BrowserLinkExtensions_UseBrowserLink_Microsoft_AspNetCore_Builder_IApplicationBuilder_) when `ASPNETCORE_ENVIRONMENT` is set to `Development`.</span></span>
* <span data-ttu-id="393bb-113">Chama [UseExceptionHandler](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ExceptionHandlerExtensions_UseExceptionHandler_Microsoft_AspNetCore_Builder_IApplicationBuilder_) quando o valor de `ASPNETCORE_ENVIRONMENT` é definido com um dos seguintes:</span><span class="sxs-lookup"><span data-stu-id="393bb-113">Calls [UseExceptionHandler](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ExceptionHandlerExtensions_UseExceptionHandler_Microsoft_AspNetCore_Builder_IApplicationBuilder_) when the value of `ASPNETCORE_ENVIRONMENT` is set one of the following:</span></span>

    * `Staging`
    * `Production`
    * `Staging_2`

<span data-ttu-id="393bb-114">O [Auxiliar de Marca de Ambiente ](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) usa o valor de `IHostingEnvironment.EnvironmentName` para incluir ou excluir a marcação no elemento:</span><span class="sxs-lookup"><span data-stu-id="393bb-114">The [Environment Tag Helper ](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) uses the value of `IHostingEnvironment.EnvironmentName` to include or exclude markup in the element:</span></span>

[!code-html[Main](environments/sample/WebApp1/Pages/About.cshtml)]

<span data-ttu-id="393bb-115">Observação: no Windows e macOS, valores e variáveis de ambiente não diferenciam maiúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="393bb-115">Note: On Windows and macOS, environment variables and values are not case sensitive.</span></span> <span data-ttu-id="393bb-116">Valores e variáveis de ambiente do Linux **diferenciam maiúsculas de minúsculas** por padrão.</span><span class="sxs-lookup"><span data-stu-id="393bb-116">Linux environment variables and values are **case sensitive** by default.</span></span>

### <a name="development"></a><span data-ttu-id="393bb-117">Desenvolvimento</span><span class="sxs-lookup"><span data-stu-id="393bb-117">Development</span></span>

<span data-ttu-id="393bb-118">O ambiente de desenvolvimento pode habilitar recursos que não devem ser expostos em produção.</span><span class="sxs-lookup"><span data-stu-id="393bb-118">The development environment can enable features that shouldn't be exposed in production.</span></span> <span data-ttu-id="393bb-119">Por exemplo, os modelos do ASP.NET Core habilitam a [página de exceção do desenvolvedor](xref:fundamentals/error-handling#the-developer-exception-page) no ambiente de desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="393bb-119">For example, the ASP.NET Core templates enable the [developer exception page](xref:fundamentals/error-handling#the-developer-exception-page) in the development environment.</span></span>

<span data-ttu-id="393bb-120">O ambiente de desenvolvimento do computador local pode ser definido no arquivo *Properties\launchSettings.json* do projeto.</span><span class="sxs-lookup"><span data-stu-id="393bb-120">The environment for local machine development can be set in the *Properties\launchSettings.json* file of the project.</span></span> <span data-ttu-id="393bb-121">Os valores de ambiente definidos em *launchSettings.json* substituem os valores definidos no ambiente do sistema.</span><span class="sxs-lookup"><span data-stu-id="393bb-121">Environment values set in *launchSettings.json* override values set in the system environment.</span></span>

<span data-ttu-id="393bb-122">O seguinte JSON mostra três perfis de um arquivo *launchSettings.json*:</span><span class="sxs-lookup"><span data-stu-id="393bb-122">The following JSON shows three profiles from a *launchSettings.json* file:</span></span>

[!code-json[Main](environments/sample/WebApp1/Properties/launchSettings.json?highlight=10,11,18,26)]

<span data-ttu-id="393bb-123">Quando o aplicativo for iniciado com `dotnet run`, o primeiro perfil com `"commandName": "Project"` será usado.</span><span class="sxs-lookup"><span data-stu-id="393bb-123">When the application is launched with `dotnet run`, the first profile with `"commandName": "Project"` will be used.</span></span> <span data-ttu-id="393bb-124">O valor de `commandName` especifica o servidor Web a ser iniciado.</span><span class="sxs-lookup"><span data-stu-id="393bb-124">The value of `commandName` specifies the web server to launch.</span></span> <span data-ttu-id="393bb-125">`commandName` pode ser um destes:</span><span class="sxs-lookup"><span data-stu-id="393bb-125">`commandName` can be one of :</span></span>

* <span data-ttu-id="393bb-126">IIS Express</span><span class="sxs-lookup"><span data-stu-id="393bb-126">IIS Express</span></span>
* <span data-ttu-id="393bb-127">IIS</span><span class="sxs-lookup"><span data-stu-id="393bb-127">IIS</span></span>
* <span data-ttu-id="393bb-128">Projeto (que inicia o Kestrel)</span><span class="sxs-lookup"><span data-stu-id="393bb-128">Project (which launches Kestrel)</span></span>

<span data-ttu-id="393bb-129">Quando um aplicativo é iniciado com `dotnet run`:</span><span class="sxs-lookup"><span data-stu-id="393bb-129">When an app is launched with `dotnet run`:</span></span>

* <span data-ttu-id="393bb-130">*launchSettings.json* é lido, se está disponível.</span><span class="sxs-lookup"><span data-stu-id="393bb-130">*launchSettings.json* is read if available.</span></span> <span data-ttu-id="393bb-131">As configurações de `environmentVariables` em *launchSettings.json* substituem as variáveis de ambiente.</span><span class="sxs-lookup"><span data-stu-id="393bb-131">`environmentVariables` settings in *launchSettings.json* override environment variables.</span></span>
* <span data-ttu-id="393bb-132">O ambiente de hospedagem é exibido.</span><span class="sxs-lookup"><span data-stu-id="393bb-132">The hosting environment is displayed.</span></span>


<span data-ttu-id="393bb-133">O seguinte resultado mostra um aplicativo iniciado com `dotnet run`:</span><span class="sxs-lookup"><span data-stu-id="393bb-133">The following output shows an app started with `dotnet run`:</span></span>
```bash
PS C:\Webs\WebApp1> dotnet run
Using launch settings from C:\Webs\WebApp1\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Webs\WebApp1
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="393bb-134">A guia **Depurar** do Visual Studio fornece uma GUI para editar o arquivo *launchSettings.json*:</span><span class="sxs-lookup"><span data-stu-id="393bb-134">The Visual Studio **Debug** tab provides a GUI to edit the *launchSettings.json* file:</span></span>

![Configurando variáveis de ambiente das propriedades do projeto](environments/_static/project-properties-debug.png)

<span data-ttu-id="393bb-136">As alterações feitas nos perfis do projeto poderão não ter efeito até que o servidor Web seja reiniciado.</span><span class="sxs-lookup"><span data-stu-id="393bb-136">Changes made to project profiles may not take effect until the web server is restarted.</span></span> <span data-ttu-id="393bb-137">O Kestrel precisa ser reiniciado antes de detectar as alterações feitas em seu ambiente.</span><span class="sxs-lookup"><span data-stu-id="393bb-137">Kestrel must be restarted before it will detect changes made to its environment.</span></span>

>[!WARNING]
> <span data-ttu-id="393bb-138">*launchSettings.json* não deve armazenar segredos.</span><span class="sxs-lookup"><span data-stu-id="393bb-138">*launchSettings.json* shouldn't store secrets.</span></span> <span data-ttu-id="393bb-139">A [ferramenta Secret Manager](xref:security/app-secrets) pode ser usado para armazenar segredos de desenvolvimento local.</span><span class="sxs-lookup"><span data-stu-id="393bb-139">The [Secret Manager tool](xref:security/app-secrets) can be used to store secrets for local development.</span></span>

### <a name="production"></a><span data-ttu-id="393bb-140">Produção</span><span class="sxs-lookup"><span data-stu-id="393bb-140">Production</span></span>

<span data-ttu-id="393bb-141">O ambiente de produção deve ser configurado para maximizar a segurança, o desempenho e a robustez do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="393bb-141">The production environment should be configured to maximize security, performance, and application robustness.</span></span> <span data-ttu-id="393bb-142">Algumas configurações comuns que um ambiente de produção pode ter que são diferentes do desenvolvimento incluem:</span><span class="sxs-lookup"><span data-stu-id="393bb-142">Some common settings that a production environment might have that would differ from development include:</span></span>

* <span data-ttu-id="393bb-143">Cache.</span><span class="sxs-lookup"><span data-stu-id="393bb-143">Caching.</span></span>
* <span data-ttu-id="393bb-144">Recursos do lado do cliente são agrupados, minimizados e potencialmente atendidos por meio de uma CDN.</span><span class="sxs-lookup"><span data-stu-id="393bb-144">Client-side resources are bundled, minified, and potentially served from a CDN.</span></span>
* <span data-ttu-id="393bb-145">Páginas de erro de diagnóstico desabilitadas.</span><span class="sxs-lookup"><span data-stu-id="393bb-145">Diagnostic error pages disabled.</span></span>
* <span data-ttu-id="393bb-146">Páginas de erro amigáveis habilitadas.</span><span class="sxs-lookup"><span data-stu-id="393bb-146">Friendly error pages enabled.</span></span>
* <span data-ttu-id="393bb-147">Log de produção e monitoramento habilitados.</span><span class="sxs-lookup"><span data-stu-id="393bb-147">Production logging and monitoring enabled.</span></span> <span data-ttu-id="393bb-148">Por exemplo, [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span><span class="sxs-lookup"><span data-stu-id="393bb-148">For example, [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="setting-the-environment"></a><span data-ttu-id="393bb-149">Configurando o ambiente</span><span class="sxs-lookup"><span data-stu-id="393bb-149">Setting the environment</span></span>

<span data-ttu-id="393bb-150">Geralmente, é útil definir um ambiente específico para teste.</span><span class="sxs-lookup"><span data-stu-id="393bb-150">It's often useful to set a specific environment for testing.</span></span> <span data-ttu-id="393bb-151">Se o ambiente não for definido, ele usará `Production` como padrão, o que desabilita a maioria dos recursos de depuração.</span><span class="sxs-lookup"><span data-stu-id="393bb-151">If the environment isn't set, it will default to `Production` which disables most debugging features.</span></span>

<span data-ttu-id="393bb-152">O método para configurar o ambiente depende do sistema operacional.</span><span class="sxs-lookup"><span data-stu-id="393bb-152">The method for setting the environment depends on the operating system.</span></span>

### <a name="azure"></a><span data-ttu-id="393bb-153">Azure</span><span class="sxs-lookup"><span data-stu-id="393bb-153">Azure</span></span>

<span data-ttu-id="393bb-154">Para o Serviço de Aplicativo do Azure:</span><span class="sxs-lookup"><span data-stu-id="393bb-154">For Azure app service:</span></span>

* <span data-ttu-id="393bb-155">Selecione a folha **Configurações de Aplicativo**.</span><span class="sxs-lookup"><span data-stu-id="393bb-155">Select the **Application settings** blade.</span></span>
* <span data-ttu-id="393bb-156">Adicione a chave e o valor em **Configurações de aplicativo**.</span><span class="sxs-lookup"><span data-stu-id="393bb-156">Add the key and value in **App settings**.</span></span>


### <a name="windows"></a><span data-ttu-id="393bb-157">Windows</span><span class="sxs-lookup"><span data-stu-id="393bb-157">Windows</span></span>
<span data-ttu-id="393bb-158">Para definir o `ASPNETCORE_ENVIRONMENT` para a sessão atual, se o aplicativo é iniciado com `dotnet run`, os seguintes comandos são usados</span><span class="sxs-lookup"><span data-stu-id="393bb-158">To set the `ASPNETCORE_ENVIRONMENT` for the current session, if the app is started using `dotnet run`, the following commands are used</span></span>

<span data-ttu-id="393bb-159">**Linha de comando**</span><span class="sxs-lookup"><span data-stu-id="393bb-159">**Command line**</span></span>
```
set ASPNETCORE_ENVIRONMENT=Development
```
<span data-ttu-id="393bb-160">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="393bb-160">**PowerShell**</span></span>
```
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

<span data-ttu-id="393bb-161">Esses comandos têm efeito somente para a janela atual.</span><span class="sxs-lookup"><span data-stu-id="393bb-161">These commands take effect only for the current window.</span></span> <span data-ttu-id="393bb-162">Quando a janela é fechada, a configuração ASPNETCORE_ENVIRONMENT é revertida para a configuração padrão ou o valor do computador.</span><span class="sxs-lookup"><span data-stu-id="393bb-162">When the window is closed, the ASPNETCORE_ENVIRONMENT setting reverts to the default setting or machine value.</span></span> <span data-ttu-id="393bb-163">Para definir o valor globalmente no Windows, abra o **Painel de Controle** > **Sistema** > **Configurações avançadas do sistema** e adicione ou edite o valor `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="393bb-163">In order to set the value globally on Windows open the **Control Panel** > **System** > **Advanced system settings** and add or edit the `ASPNETCORE_ENVIRONMENT` value.</span></span>

![Propriedades avançadas do sistema](environments/_static/systemsetting_environment.png)

![Variável de ambiente do ASP.NET Core](environments/_static/windows_aspnetcore_environment.png)


<span data-ttu-id="393bb-166">**web.config**</span><span class="sxs-lookup"><span data-stu-id="393bb-166">**web.config**</span></span>

<span data-ttu-id="393bb-167">Consulte a seção *Configurando variáveis de ambiente* do tópico [Referência de configuração do Módulo do ASP.NET Core](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span><span class="sxs-lookup"><span data-stu-id="393bb-167">See the *Setting environment variables* section of the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) topic.</span></span>

<span data-ttu-id="393bb-168">**Por pool de aplicativos do IIS**</span><span class="sxs-lookup"><span data-stu-id="393bb-168">**Per IIS Application Pool**</span></span>

<span data-ttu-id="393bb-169">Para definir variáveis de ambiente para aplicativos individuais executados em Pools de Aplicativos isolados (compatíveis com o IIS 10.0+), consulte a seção *Comando AppCmd.exe* do tópico [Variáveis de ambiente \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe).</span><span class="sxs-lookup"><span data-stu-id="393bb-169">To set environment variables for individual apps running in isolated Application Pools (supported on IIS 10.0+), see the *AppCmd.exe command* section of the [Environment Variables \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic.</span></span>

### <a name="macos"></a><span data-ttu-id="393bb-170">macOS</span><span class="sxs-lookup"><span data-stu-id="393bb-170">macOS</span></span>
<span data-ttu-id="393bb-171">A configuração do ambiente atual para o macOS pode ser feita em linha durante a execução do aplicativo</span><span class="sxs-lookup"><span data-stu-id="393bb-171">Setting the current environment for macOS can be done in-line when running the application;</span></span>

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```
<span data-ttu-id="393bb-172">ou usando `export` para configurá-lo antes da execução do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="393bb-172">or using `export` to set it prior to running the app.</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```
<span data-ttu-id="393bb-173">As variáveis de ambiente no nível do computador são definidas no arquivo *.bashrc* ou *.bash_profile*.</span><span class="sxs-lookup"><span data-stu-id="393bb-173">Machine level environment variables are set in the *.bashrc* or *.bash_profile* file.</span></span> <span data-ttu-id="393bb-174">Edite o arquivo usando qualquer editor de texto e adicione a instrução a seguir.</span><span class="sxs-lookup"><span data-stu-id="393bb-174">Edit the file using any text editor and add the following statment.</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

### <a name="linux"></a><span data-ttu-id="393bb-175">Linux</span><span class="sxs-lookup"><span data-stu-id="393bb-175">Linux</span></span>
<span data-ttu-id="393bb-176">Para distribuições Linux, use o comando `export` na linha de comando para as configurações de variável baseadas na sessão e o arquivo *bash_profile* para as configurações de ambiente no nível do computador.</span><span class="sxs-lookup"><span data-stu-id="393bb-176">For Linux distros, use the `export` command at the command line for session based variable settings and *bash_profile* file for machine level environment settings.</span></span>

### <a name="configuration-by-environment"></a><span data-ttu-id="393bb-177">Configuração por ambiente</span><span class="sxs-lookup"><span data-stu-id="393bb-177">Configuration by environment</span></span>

<span data-ttu-id="393bb-178">Consulte [Configuração por ambiente](xref:fundamentals/configuration/index#configuration-by-environment) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="393bb-178">See [Configuration by environment](xref:fundamentals/configuration/index#configuration-by-environment) for more information.</span></span>

<a name="startup-conventions"></a>
## <a name="environment-based-startup-class-and-methods"></a><span data-ttu-id="393bb-179">Classe Startup e métodos baseados no ambiente</span><span class="sxs-lookup"><span data-stu-id="393bb-179">Environment based Startup class and methods</span></span>

<span data-ttu-id="393bb-180">Quando um aplicativo ASP.NET Core é iniciado, a [classe Startup](xref:fundamentals/startup) inicia o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="393bb-180">When an ASP.NET Core app starts, the [Startup class](xref:fundamentals/startup) bootstraps the app.</span></span> <span data-ttu-id="393bb-181">Se houver uma classe `Startup{EnvironmentName}`, essa classe será chamada para esse `EnvironmentName`:</span><span class="sxs-lookup"><span data-stu-id="393bb-181">If a class `Startup{EnvironmentName}` exists, that class will be called for that `EnvironmentName`:</span></span>

[!code-csharp[Main](environments/sample/WebApp1/StartupDev.cs?name=snippet&highlight=1)]

<span data-ttu-id="393bb-182">Observação: a chamada a [WebHostBuilder.UseStartup<TStartup> ](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) substitui as seções de configuração.</span><span class="sxs-lookup"><span data-stu-id="393bb-182">Note: Calling [WebHostBuilder.UseStartup<TStartup>](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) overrides configuration sections.</span></span>

<span data-ttu-id="393bb-183">[Configure](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_StartupBase_Configure_Microsoft_AspNetCore_Builder_IApplicationBuilder_) e [ConfigureServices](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices?view=aspnetcore-2.0) são compatíveis com versões específicas ao ambiente do formato `Configure{EnvironmentName}` e `Configure{EnvironmentName}Services`:</span><span class="sxs-lookup"><span data-stu-id="393bb-183">[Configure](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_StartupBase_Configure_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [ConfigureServices](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices?view=aspnetcore-2.0) support environment specific versions of the form `Configure{EnvironmentName}` and `Configure{EnvironmentName}Services`:</span></span>

[!code-csharp[Main](environments/sample/WebApp1/Startup.cs?name=snippet_all&highlight=15,37)]

## <a name="additional-resources"></a><span data-ttu-id="393bb-184">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="393bb-184">Additional resources</span></span>

* [<span data-ttu-id="393bb-185">Inicialização de aplicativos</span><span class="sxs-lookup"><span data-stu-id="393bb-185">Application startup</span></span>](xref:fundamentals/startup)
* [<span data-ttu-id="393bb-186">Configuração</span><span class="sxs-lookup"><span data-stu-id="393bb-186">Configuration</span></span>](xref:fundamentals/configuration/index)
* [<span data-ttu-id="393bb-187">IHostingEnvironment.EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="393bb-187">IHostingEnvironment.EnvironmentName</span></span>](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName)

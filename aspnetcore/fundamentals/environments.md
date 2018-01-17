---
title: "Trabalhando com vários ambientes no núcleo do ASP.NET"
author: rick-anderson
description: "Saiba como o ASP.NET Core fornece suporte para controlar o comportamento do aplicativo em vários ambientes."
keywords: "Núcleo do ASP.NET, configurações de ambiente, ASPNETCORE_ENVIRONMENT"
ms.author: riande
manager: wpickett
ms.date: 12/25/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/environments
ms.openlocfilehash: 784d176145c3e4e44ddc0ea06b6702f70cd4b08c
ms.sourcegitcommit: 87168cdc409e7a7257f92a0f48f9c5ab320b5b28
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/17/2018
---
# <a name="working-with-multiple-environments"></a><span data-ttu-id="7a286-104">Trabalhando com vários ambientes</span><span class="sxs-lookup"><span data-stu-id="7a286-104">Working with multiple environments</span></span>

<span data-ttu-id="7a286-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="7a286-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="7a286-106">ASP.NET Core fornece suporte para a configuração de comportamento do aplicativo em tempo de execução com variáveis de ambiente.</span><span class="sxs-lookup"><span data-stu-id="7a286-106">ASP.NET Core provides support for setting application behavior at runtime with environment variables.</span></span>

<span data-ttu-id="7a286-107">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="7a286-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="environments"></a><span data-ttu-id="7a286-108">Ambientes</span><span class="sxs-lookup"><span data-stu-id="7a286-108">Environments</span></span>

<span data-ttu-id="7a286-109">ASP.NET Core lê a variável de ambiente `ASPNETCORE_ENVIRONMENT` na inicialização do aplicativo e armazena esse valor em [IHostingEnvironment.EnvironmentName](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName).</span><span class="sxs-lookup"><span data-stu-id="7a286-109">ASP.NET Core reads the environment variable `ASPNETCORE_ENVIRONMENT` at application startup and stores that value in [IHostingEnvironment.EnvironmentName](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName).</span></span> <span data-ttu-id="7a286-110">`ASPNETCORE_ENVIRONMENT`pode ser definida como qualquer valor, mas [três valores](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname?view=aspnetcore-2.0) com suporte do framework: [desenvolvimento](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development?view=aspnetcore-2.0), [preparo](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging?view=aspnetcore-2.0), e [produção](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="7a286-110">`ASPNETCORE_ENVIRONMENT` can be set to any value, but [three values](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname?view=aspnetcore-2.0) are supported by the framework: [Development](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development?view=aspnetcore-2.0), [Staging](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging?view=aspnetcore-2.0), and [Production](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production?view=aspnetcore-2.0).</span></span> <span data-ttu-id="7a286-111">Se `ASPNETCORE_ENVIRONMENT` não está definida, o padrão será `Production`.</span><span class="sxs-lookup"><span data-stu-id="7a286-111">If `ASPNETCORE_ENVIRONMENT` is not set, it will default to `Production`.</span></span>

[!code-csharp[Main](environments/sample/WebApp1/Startup.cs?name=snippet)]

<span data-ttu-id="7a286-112">O código anterior:</span><span class="sxs-lookup"><span data-stu-id="7a286-112">The preceding code:</span></span>

* <span data-ttu-id="7a286-113">Chamadas [UseDeveloperExceptionPage](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_DeveloperExceptionPageExtensions_UseDeveloperExceptionPage_Microsoft_AspNetCore_Builder_IApplicationBuilder_) e [UseBrowserLink](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_BrowserLinkExtensions_UseBrowserLink_Microsoft_AspNetCore_Builder_IApplicationBuilder_) quando `ASPNETCORE_ENVIRONMENT` é definido como `Development`.</span><span class="sxs-lookup"><span data-stu-id="7a286-113">Calls [UseDeveloperExceptionPage](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_DeveloperExceptionPageExtensions_UseDeveloperExceptionPage_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [UseBrowserLink](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_BrowserLinkExtensions_UseBrowserLink_Microsoft_AspNetCore_Builder_IApplicationBuilder_) when `ASPNETCORE_ENVIRONMENT` is set to `Development`.</span></span>
* <span data-ttu-id="7a286-114">Chamadas [UseExceptionHandler](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ExceptionHandlerExtensions_UseExceptionHandler_Microsoft_AspNetCore_Builder_IApplicationBuilder_) quando o valor de `ASPNETCORE_ENVIRONMENT` é definido um dos seguintes:</span><span class="sxs-lookup"><span data-stu-id="7a286-114">Calls [UseExceptionHandler](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ExceptionHandlerExtensions_UseExceptionHandler_Microsoft_AspNetCore_Builder_IApplicationBuilder_) when the value of `ASPNETCORE_ENVIRONMENT` is set one of the following:</span></span>

    * `Staging`
    * `Production`
    * `Staging_2`

<span data-ttu-id="7a286-115">O [auxiliar de marca de ambiente ](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) usa o valor de `IHostingEnvironment.EnvironmentName` para incluir ou excluir marcação no elemento:</span><span class="sxs-lookup"><span data-stu-id="7a286-115">The [Environment Tag Helper ](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) uses the value of `IHostingEnvironment.EnvironmentName` to include or exclude markup in the element:</span></span>

[!code-html[Main](environments/sample/WebApp1/Pages/About.cshtml)]

<span data-ttu-id="7a286-116">Observação: No Windows e macOS, valores e variáveis de ambiente não são diferencia maiusculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="7a286-116">Note: On Windows and macOS, environment variables and values are not case sensitive.</span></span> <span data-ttu-id="7a286-117">Variáveis de ambiente do Linux e os valores são **diferencia maiusculas de minúsculas** por padrão.</span><span class="sxs-lookup"><span data-stu-id="7a286-117">Linux environment variables and values are **case sensitive** by default.</span></span>

### <a name="development"></a><span data-ttu-id="7a286-118">Desenvolvimento</span><span class="sxs-lookup"><span data-stu-id="7a286-118">Development</span></span>

<span data-ttu-id="7a286-119">O ambiente de desenvolvimento pode habilitar recursos que não devem ser expostos na produção.</span><span class="sxs-lookup"><span data-stu-id="7a286-119">The development environment can enable features that should not be exposed in production.</span></span> <span data-ttu-id="7a286-120">Por exemplo, os modelos do ASP.NET Core habilitam o [página de exceção de desenvolvedor](xref:fundamentals/error-handling#the-developer-exception-page) no ambiente de desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="7a286-120">For example, the ASP.NET Core templates enable the [developer exception page](xref:fundamentals/error-handling#the-developer-exception-page) in the development environment.</span></span>

<span data-ttu-id="7a286-121">O ambiente de desenvolvimento do computador local pode ser definido na *Properties\launchSettings.json* arquivo do projeto.</span><span class="sxs-lookup"><span data-stu-id="7a286-121">The environment for local machine development can be set in the *Properties\launchSettings.json* file of the project.</span></span> <span data-ttu-id="7a286-122">Definir valores de ambiente em *launchSettings.json* substituir valores definidos no ambiente do sistema.</span><span class="sxs-lookup"><span data-stu-id="7a286-122">Environment values set in *launchSettings.json* override values set in the system environment.</span></span>

<span data-ttu-id="7a286-123">O XML a seguir mostra três perfis de um *launchSettings.json* arquivo:</span><span class="sxs-lookup"><span data-stu-id="7a286-123">The following XML shows three profiles from a *launchSettings.json* file:</span></span>

[!code-xml[Main](environments/sample/WebApp1/Properties/launchSettings.json?highlight=10,11,18,26)]

<span data-ttu-id="7a286-124">Quando o aplicativo é iniciado com `dotnet run`, o primeiro perfil com `"commandName": "Project"` será usado.</span><span class="sxs-lookup"><span data-stu-id="7a286-124">When the application is launched with `dotnet run`, the first profile with `"commandName": "Project"` will be used.</span></span> <span data-ttu-id="7a286-125">O valor de `commandName` Especifica o servidor web para iniciar.</span><span class="sxs-lookup"><span data-stu-id="7a286-125">The value of `commandName` specifies the web server to launch.</span></span> <span data-ttu-id="7a286-126">`commandName`pode ser um destes:</span><span class="sxs-lookup"><span data-stu-id="7a286-126">`commandName` can be one of :</span></span>

* <span data-ttu-id="7a286-127">IIS Express</span><span class="sxs-lookup"><span data-stu-id="7a286-127">IIS Express</span></span>
* <span data-ttu-id="7a286-128">IIS</span><span class="sxs-lookup"><span data-stu-id="7a286-128">IIS</span></span>
* <span data-ttu-id="7a286-129">Projeto (o que inicia Kestrel)</span><span class="sxs-lookup"><span data-stu-id="7a286-129">Project (which launches Kestrel)</span></span>

<span data-ttu-id="7a286-130">Quando um aplicativo é iniciado com `dotnet run`:</span><span class="sxs-lookup"><span data-stu-id="7a286-130">When an app is launched with `dotnet run`:</span></span>

* <span data-ttu-id="7a286-131">*launchSettings.json* é lido se disponível.</span><span class="sxs-lookup"><span data-stu-id="7a286-131">*launchSettings.json* is read if available.</span></span> <span data-ttu-id="7a286-132">`environmentVariables`as configurações no *launchSettings.json* substituir variáveis de ambiente.</span><span class="sxs-lookup"><span data-stu-id="7a286-132">`environmentVariables` settings in *launchSettings.json* override environment variables.</span></span>
* <span data-ttu-id="7a286-133">O ambiente de hospedagem é exibido.</span><span class="sxs-lookup"><span data-stu-id="7a286-133">The hosting environment is displayed.</span></span>


<span data-ttu-id="7a286-134">A saída a seguir mostra um aplicativo iniciado com `dotnet run`:</span><span class="sxs-lookup"><span data-stu-id="7a286-134">The following output shows an app started with `dotnet run`:</span></span>
```bash
PS C:\Webs\WebApp1> dotnet run
Using launch settings from C:\Webs\WebApp1\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Webs\WebApp1
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="7a286-135">O Visual Studio **depurar** guia fornece uma interface gráfica do usuário para editar o *launchSettings.json* arquivo:</span><span class="sxs-lookup"><span data-stu-id="7a286-135">The Visual Studio **Debug** tab provides a GUI to edit the *launchSettings.json* file:</span></span>

![Variáveis de ambiente de configuração de propriedades do projeto](environments/_static/project-properties-debug.png)

<span data-ttu-id="7a286-137">As alterações feitas aos perfis de projeto não podem ser aplicadas até que o servidor web seja reiniciado.</span><span class="sxs-lookup"><span data-stu-id="7a286-137">Changes made to project profiles may not take effect until the web server is restarted.</span></span> <span data-ttu-id="7a286-138">Kestrel deve ser reiniciado antes de ele detectará as alterações feitas em seu ambiente.</span><span class="sxs-lookup"><span data-stu-id="7a286-138">Kestrel must be restarted before it will detect changes made to its environment.</span></span>

>[!WARNING]
> <span data-ttu-id="7a286-139">*launchSettings.json* não deve armazenar segredos.</span><span class="sxs-lookup"><span data-stu-id="7a286-139">*launchSettings.json* should not store secrets.</span></span> <span data-ttu-id="7a286-140">O [ferramenta Gerenciador de segredo](xref:security/app-secrets) pode ser usado para armazenar segredos de desenvolvimento local.</span><span class="sxs-lookup"><span data-stu-id="7a286-140">The [Secret Manager tool](xref:security/app-secrets) can be used to store secrets for local development.</span></span>

### <a name="production"></a><span data-ttu-id="7a286-141">Produção</span><span class="sxs-lookup"><span data-stu-id="7a286-141">Production</span></span>

<span data-ttu-id="7a286-142">O ambiente de produção deve ser configurado para maximizar a segurança, desempenho e eficiência do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="7a286-142">The production environment should be configured to maximize security, performance, and application robustness.</span></span> <span data-ttu-id="7a286-143">Algumas configurações comuns que pode ter um ambiente de produção que seriam diferentes de desenvolvimento incluem:</span><span class="sxs-lookup"><span data-stu-id="7a286-143">Some common settings that a production environment might have that would differ from development include:</span></span>

* <span data-ttu-id="7a286-144">Armazenamento em cache.</span><span class="sxs-lookup"><span data-stu-id="7a286-144">Caching.</span></span>
* <span data-ttu-id="7a286-145">Recursos do cliente são agrupados, minimizados e potencialmente atendidos a partir de uma CDN.</span><span class="sxs-lookup"><span data-stu-id="7a286-145">Client-side resources are bundled, minified, and potentially served from a CDN.</span></span>
* <span data-ttu-id="7a286-146">Páginas de erro de diagnóstico desabilitadas.</span><span class="sxs-lookup"><span data-stu-id="7a286-146">Diagnostic error pages disabled.</span></span>
* <span data-ttu-id="7a286-147">Páginas de erro amigável habilitadas.</span><span class="sxs-lookup"><span data-stu-id="7a286-147">Friendly error pages enabled.</span></span>
* <span data-ttu-id="7a286-148">Log de produção e o monitoramento habilitado.</span><span class="sxs-lookup"><span data-stu-id="7a286-148">Production logging and monitoring enabled.</span></span> <span data-ttu-id="7a286-149">Por exemplo, [Application Insights](https://azure.microsoft.com/documentation/articles/app-insights-asp-net-five/).</span><span class="sxs-lookup"><span data-stu-id="7a286-149">For example, [Application Insights](https://azure.microsoft.com/documentation/articles/app-insights-asp-net-five/).</span></span>

## <a name="setting-the-environment"></a><span data-ttu-id="7a286-150">Configurando o ambiente</span><span class="sxs-lookup"><span data-stu-id="7a286-150">Setting the environment</span></span>

<span data-ttu-id="7a286-151">Geralmente é útil definir um ambiente específico para teste.</span><span class="sxs-lookup"><span data-stu-id="7a286-151">It's often useful to set a specific environment for testing.</span></span> <span data-ttu-id="7a286-152">Se o ambiente não for definido, o padrão será `Production` que desativa a maioria dos recursos de depuração.</span><span class="sxs-lookup"><span data-stu-id="7a286-152">If the environment is not set, it will default to `Production` which disables most debugging features.</span></span>

<span data-ttu-id="7a286-153">O método para definir o ambiente depende do sistema operacional.</span><span class="sxs-lookup"><span data-stu-id="7a286-153">The method for setting the environment depends on the operating system.</span></span>

### <a name="azure"></a><span data-ttu-id="7a286-154">Azure</span><span class="sxs-lookup"><span data-stu-id="7a286-154">Azure</span></span>

<span data-ttu-id="7a286-155">Para o serviço de aplicativo do Azure:</span><span class="sxs-lookup"><span data-stu-id="7a286-155">For Azure app service:</span></span>

* <span data-ttu-id="7a286-156">Selecione o **configurações de aplicativo** folha.</span><span class="sxs-lookup"><span data-stu-id="7a286-156">Select the **Application settings** blade.</span></span>
* <span data-ttu-id="7a286-157">Adicionar a chave e valor em **configurações do aplicativo**.</span><span class="sxs-lookup"><span data-stu-id="7a286-157">Add the key and value in **App settings**.</span></span>


### <a name="windows"></a><span data-ttu-id="7a286-158">Windows</span><span class="sxs-lookup"><span data-stu-id="7a286-158">Windows</span></span>
<span data-ttu-id="7a286-159">Para definir o `ASPNETCORE_ENVIRONMENT` para a sessão atual, se o aplicativo é iniciado usando `dotnet run`, os comandos a seguir são usados</span><span class="sxs-lookup"><span data-stu-id="7a286-159">To set the `ASPNETCORE_ENVIRONMENT` for the current session, if the app is started using `dotnet run`, the following commands are used</span></span>

<span data-ttu-id="7a286-160">**Linha de comando**</span><span class="sxs-lookup"><span data-stu-id="7a286-160">**Command line**</span></span>
```
set ASPNETCORE_ENVIRONMENT=Development
```
<span data-ttu-id="7a286-161">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="7a286-161">**PowerShell**</span></span>
```
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

<span data-ttu-id="7a286-162">Esses comandos têm efeito somente para a janela atual.</span><span class="sxs-lookup"><span data-stu-id="7a286-162">These commands take effect only for the current window.</span></span> <span data-ttu-id="7a286-163">Quando a janela for fechada, a configuração ASPNETCORE_ENVIRONMENT é revertida para a configuração padrão ou o valor de máquina.</span><span class="sxs-lookup"><span data-stu-id="7a286-163">When the window is closed, the ASPNETCORE_ENVIRONMENT setting reverts to the default setting or machine value.</span></span> <span data-ttu-id="7a286-164">Para definir o valor global em janelas abertas do **painel de controle** > **sistema** > **configurações avançadas do sistema** e adicionar ou editar o `ASPNETCORE_ENVIRONMENT` valor.</span><span class="sxs-lookup"><span data-stu-id="7a286-164">In order to set the value globally on Windows open the **Control Panel** > **System** > **Advanced system settings** and add or edit the `ASPNETCORE_ENVIRONMENT` value.</span></span>

![Propriedades avançadas do sistema](environments/_static/systemsetting_environment.png)

![Variável de ambiente do ASPNET Core](environments/_static/windows_aspnetcore_environment.png)


<span data-ttu-id="7a286-167">**web.config**</span><span class="sxs-lookup"><span data-stu-id="7a286-167">**web.config**</span></span>

<span data-ttu-id="7a286-168">Consulte o *definir variáveis de ambiente* seção o [referência de configuração do módulo do ASP.NET Core](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) tópico.</span><span class="sxs-lookup"><span data-stu-id="7a286-168">See the *Setting environment variables* section of the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) topic.</span></span>

<span data-ttu-id="7a286-169">**Por Pool de aplicativos do IIS**</span><span class="sxs-lookup"><span data-stu-id="7a286-169">**Per IIS Application Pool**</span></span>

<span data-ttu-id="7a286-170">Para definir variáveis de ambiente para aplicativos individuais executados em Pools de aplicativos isolados (com suporte no IIS 10.0 +), consulte o *comando AppCmd.exe* seção o [variáveis de ambiente \< environmentVariables >](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) tópico.</span><span class="sxs-lookup"><span data-stu-id="7a286-170">To set environment variables for individual apps running in isolated Application Pools (supported on IIS 10.0+), see the *AppCmd.exe command* section of the [Environment Variables \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic.</span></span>

### <a name="macos"></a><span data-ttu-id="7a286-171">macOS</span><span class="sxs-lookup"><span data-stu-id="7a286-171">macOS</span></span>
<span data-ttu-id="7a286-172">Configurar o ambiente atual para macOS pode ser feito na linha ao executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="7a286-172">Setting the current environment for macOS can be done in-line when running the application;</span></span>

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```
<span data-ttu-id="7a286-173">ou usar `export` para defini-lo antes de executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="7a286-173">or using `export` to set it prior to running the app.</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```
<span data-ttu-id="7a286-174">Variáveis de ambiente de nível de máquina são definidas no *. bashrc* ou *. bash_profile* arquivo.</span><span class="sxs-lookup"><span data-stu-id="7a286-174">Machine level environment variables are set in the *.bashrc* or *.bash_profile* file.</span></span> <span data-ttu-id="7a286-175">Editar o arquivo usando qualquer editor de texto e adicione a seguinte instrução.</span><span class="sxs-lookup"><span data-stu-id="7a286-175">Edit the file using any text editor and add the following statment.</span></span>

```
export ASPNETCORE_ENVIRONMENT=Development
```

### <a name="linux"></a><span data-ttu-id="7a286-176">Linux</span><span class="sxs-lookup"><span data-stu-id="7a286-176">Linux</span></span>
<span data-ttu-id="7a286-177">Para distribuições de Linux, use o `export` para sessão com base em configurações de variável de comando na linha de comando e *bash_profile* arquivo de configurações de ambiente de nível de máquina.</span><span class="sxs-lookup"><span data-stu-id="7a286-177">For Linux distros, use the `export` command at the command line for session based variable settings and *bash_profile* file for machine level environment settings.</span></span>

### <a name="configuration-by-environment"></a><span data-ttu-id="7a286-178">Configuração do ambiente</span><span class="sxs-lookup"><span data-stu-id="7a286-178">Configuration by environment</span></span>

<span data-ttu-id="7a286-179">Consulte [configuração pelo ambiente](xref:fundamentals/configuration/index#configuration-by-environment) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="7a286-179">See [Configuration by environment](xref:fundamentals/configuration/index#configuration-by-environment) for more information.</span></span>

<a name="startup-conventions"></a>
## <a name="environment-based-startup-class-and-methods"></a><span data-ttu-id="7a286-180">Ambiente com base em métodos e classe de inicialização</span><span class="sxs-lookup"><span data-stu-id="7a286-180">Environment based Startup class and methods</span></span>

<span data-ttu-id="7a286-181">Quando um aplicativo do ASP.NET Core é iniciado, o [classe inicialização](xref:fundamentals/startup) inicializa o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="7a286-181">When an ASP.NET Core app starts, the [Startup class](xref:fundamentals/startup) bootstraps the app.</span></span> <span data-ttu-id="7a286-182">Se uma classe `Startup{EnvironmentName}` existe, se a classe será chamada para que `EnvironmentName`:</span><span class="sxs-lookup"><span data-stu-id="7a286-182">If a class `Startup{EnvironmentName}` exists, that class will be called for that `EnvironmentName`:</span></span>

[!code-csharp[Main](environments/sample/WebApp1/StartupDev.cs?name=snippet&highlight=1)]

<span data-ttu-id="7a286-183">Observação: Ao chamar [WebHostBuilder.UseStartup<TStartup> ](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) substitui seções de configuração.</span><span class="sxs-lookup"><span data-stu-id="7a286-183">Note: Calling [WebHostBuilder.UseStartup<TStartup>](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) overrides configuration sections.</span></span>

<span data-ttu-id="7a286-184">[Configurar](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_StartupBase_Configure_Microsoft_AspNetCore_Builder_IApplicationBuilder_) e [ConfigureServices](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices?view=aspnetcore-2.0) suporte a versões específicas do ambiente do formulário `Configure{EnvironmentName}` e `Configure{EnvironmentName}Services`:</span><span class="sxs-lookup"><span data-stu-id="7a286-184">[Configure](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_StartupBase_Configure_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [ConfigureServices](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices?view=aspnetcore-2.0) support environment specific versions of the form `Configure{EnvironmentName}` and `Configure{EnvironmentName}Services`:</span></span>

[!code-csharp[Main](environments/sample/WebApp1/Startup.cs?name=snippet_all&highlight=15,37)]

## <a name="additional-resources"></a><span data-ttu-id="7a286-185">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="7a286-185">Additional Resources</span></span>

* [<span data-ttu-id="7a286-186">Inicialização de aplicativos</span><span class="sxs-lookup"><span data-stu-id="7a286-186">Application startup</span></span>](xref:fundamentals/startup)
* [<span data-ttu-id="7a286-187">Configuração</span><span class="sxs-lookup"><span data-stu-id="7a286-187">Configuration</span></span>](xref:fundamentals/configuration/index)
* [<span data-ttu-id="7a286-188">IHostingEnvironment.EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="7a286-188">IHostingEnvironment.EnvironmentName</span></span>](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName)

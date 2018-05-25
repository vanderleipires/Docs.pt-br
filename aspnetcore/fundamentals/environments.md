---
title: Usar vários ambientes no ASP.NET Core
author: rick-anderson
description: Saiba como o ASP.NET Core fornece suporte para controlar o comportamento do aplicativo em vários ambientes.
manager: wpickett
ms.author: riande
ms.date: 12/25/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/environments
ms.openlocfilehash: 2c8441db527203aeea516073dae3bc335c335565
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/07/2018
---
# <a name="use-multiple-environments-in-aspnet-core"></a><span data-ttu-id="23101-103">Usar vários ambientes no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="23101-103">Use multiple environments in ASP.NET Core</span></span>

<span data-ttu-id="23101-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="23101-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="23101-105">O ASP.NET Core fornece suporte para a configuração do comportamento do aplicativo em tempo de execução com variáveis de ambiente.</span><span class="sxs-lookup"><span data-stu-id="23101-105">ASP.NET Core provides support for setting application behavior at runtime with environment variables.</span></span>

<span data-ttu-id="23101-106">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="23101-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="environments"></a><span data-ttu-id="23101-107">Ambientes</span><span class="sxs-lookup"><span data-stu-id="23101-107">Environments</span></span>

<span data-ttu-id="23101-108">O ASP.NET Core lê a variável de ambiente `ASPNETCORE_ENVIRONMENT` na inicialização do aplicativo e armazena esse valor em [IHostingEnvironment.EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName).</span><span class="sxs-lookup"><span data-stu-id="23101-108">ASP.NET Core reads the environment variable `ASPNETCORE_ENVIRONMENT` at application startup and stores that value in [IHostingEnvironment.EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName).</span></span> <span data-ttu-id="23101-109">`ASPNETCORE_ENVIRONMENT` pode ser definido com qualquer valor, mas há suporte para [três valores](/dotnet/api/microsoft.aspnetcore.hosting.environmentname?view=aspnetcore-2.0) na estrutura: [Desenvolvimento](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development?view=aspnetcore-2.0), [Preparo](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging?view=aspnetcore-2.0) e [Produção](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="23101-109">`ASPNETCORE_ENVIRONMENT` can be set to any value, but [three values](/dotnet/api/microsoft.aspnetcore.hosting.environmentname?view=aspnetcore-2.0) are supported by the framework: [Development](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development?view=aspnetcore-2.0), [Staging](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging?view=aspnetcore-2.0), and [Production](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production?view=aspnetcore-2.0).</span></span> <span data-ttu-id="23101-110">Se `ASPNETCORE_ENVIRONMENT` não estiver definido, ele usará `Production` como padrão.</span><span class="sxs-lookup"><span data-stu-id="23101-110">If `ASPNETCORE_ENVIRONMENT` isn't set, it will default to `Production`.</span></span>

[!code-csharp[](environments/sample/WebApp1/Startup.cs?name=snippet)]

<span data-ttu-id="23101-111">O código anterior:</span><span class="sxs-lookup"><span data-stu-id="23101-111">The preceding code:</span></span>

* <span data-ttu-id="23101-112">Chama [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_DeveloperExceptionPageExtensions_UseDeveloperExceptionPage_Microsoft_AspNetCore_Builder_IApplicationBuilder_) e [UseBrowserLink](/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_BrowserLinkExtensions_UseBrowserLink_Microsoft_AspNetCore_Builder_IApplicationBuilder_) quando `ASPNETCORE_ENVIRONMENT` é definido como `Development`.</span><span class="sxs-lookup"><span data-stu-id="23101-112">Calls [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_DeveloperExceptionPageExtensions_UseDeveloperExceptionPage_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [UseBrowserLink](/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_BrowserLinkExtensions_UseBrowserLink_Microsoft_AspNetCore_Builder_IApplicationBuilder_) when `ASPNETCORE_ENVIRONMENT` is set to `Development`.</span></span>
* <span data-ttu-id="23101-113">Chama [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ExceptionHandlerExtensions_UseExceptionHandler_Microsoft_AspNetCore_Builder_IApplicationBuilder_) quando o valor de `ASPNETCORE_ENVIRONMENT` é definido com um dos seguintes:</span><span class="sxs-lookup"><span data-stu-id="23101-113">Calls [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ExceptionHandlerExtensions_UseExceptionHandler_Microsoft_AspNetCore_Builder_IApplicationBuilder_) when the value of `ASPNETCORE_ENVIRONMENT` is set one of the following:</span></span>

    * `Staging`
    * `Production`
    * `Staging_2`

<span data-ttu-id="23101-114">O [Auxiliar de Marca de Ambiente ](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) usa o valor de `IHostingEnvironment.EnvironmentName` para incluir ou excluir a marcação no elemento:</span><span class="sxs-lookup"><span data-stu-id="23101-114">The [Environment Tag Helper ](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) uses the value of `IHostingEnvironment.EnvironmentName` to include or exclude markup in the element:</span></span>

[!code-html[](environments/sample/WebApp1/Pages/About.cshtml)]

<span data-ttu-id="23101-115">Observação: no Windows e macOS, valores e variáveis de ambiente não diferenciam maiúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="23101-115">Note: On Windows and macOS, environment variables and values are not case sensitive.</span></span> <span data-ttu-id="23101-116">Valores e variáveis de ambiente do Linux **diferenciam maiúsculas de minúsculas** por padrão.</span><span class="sxs-lookup"><span data-stu-id="23101-116">Linux environment variables and values are **case sensitive** by default.</span></span>

### <a name="development"></a><span data-ttu-id="23101-117">Desenvolvimento</span><span class="sxs-lookup"><span data-stu-id="23101-117">Development</span></span>

<span data-ttu-id="23101-118">O ambiente de desenvolvimento pode habilitar recursos que não devem ser expostos em produção.</span><span class="sxs-lookup"><span data-stu-id="23101-118">The development environment can enable features that shouldn't be exposed in production.</span></span> <span data-ttu-id="23101-119">Por exemplo, os modelos do ASP.NET Core habilitam a [página de exceção do desenvolvedor](xref:fundamentals/error-handling#the-developer-exception-page) no ambiente de desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="23101-119">For example, the ASP.NET Core templates enable the [developer exception page](xref:fundamentals/error-handling#the-developer-exception-page) in the development environment.</span></span>

<span data-ttu-id="23101-120">O ambiente de desenvolvimento do computador local pode ser definido no arquivo *Properties\launchSettings.json* do projeto.</span><span class="sxs-lookup"><span data-stu-id="23101-120">The environment for local machine development can be set in the *Properties\launchSettings.json* file of the project.</span></span> <span data-ttu-id="23101-121">Os valores de ambiente definidos em *launchSettings.json* substituem os valores definidos no ambiente do sistema.</span><span class="sxs-lookup"><span data-stu-id="23101-121">Environment values set in *launchSettings.json* override values set in the system environment.</span></span>

<span data-ttu-id="23101-122">O seguinte JSON mostra três perfis de um arquivo *launchSettings.json*:</span><span class="sxs-lookup"><span data-stu-id="23101-122">The following JSON shows three profiles from a *launchSettings.json* file:</span></span>

[!code-json[](environments/sample/WebApp1/Properties/launchSettings.json?highlight=10,11,18,26)]

::: moniker range=">= aspnetcore-2.1"
> [!NOTE]
> <span data-ttu-id="23101-123">A propriedade `applicationUrl` no *launchSettings.json* pode especificar uma lista de URLs de servidores.</span><span class="sxs-lookup"><span data-stu-id="23101-123">The `applicationUrl` property in *launchSettings.json* can specify a list of server URLs.</span></span> <span data-ttu-id="23101-124">Use um ponto e vírgula entre as URLs na lista:</span><span class="sxs-lookup"><span data-stu-id="23101-124">Use a semicolon between the URLs in the list:</span></span>
>
> ```json
> "WebApplication1": {
>    "commandName": "Project",
>    "launchBrowser": true,
>    "applicationUrl": "https://localhost:5001;http://localhost:5000",
>    "environmentVariables": {
>      "ASPNETCORE_ENVIRONMENT": "Development"
>    }
> }
> ```
::: moniker-end

<span data-ttu-id="23101-125">Quando o aplicativo for iniciado com [dotnet run](/dotnet/core/tools/dotnet-run), o primeiro perfil com `"commandName": "Project"` será usado.</span><span class="sxs-lookup"><span data-stu-id="23101-125">When the application is launched with [dotnet run](/dotnet/core/tools/dotnet-run), the first profile with `"commandName": "Project"` will be used.</span></span> <span data-ttu-id="23101-126">O valor de `commandName` especifica o servidor Web a ser iniciado.</span><span class="sxs-lookup"><span data-stu-id="23101-126">The value of `commandName` specifies the web server to launch.</span></span> <span data-ttu-id="23101-127">`commandName` pode ser um destes:</span><span class="sxs-lookup"><span data-stu-id="23101-127">`commandName` can be one of :</span></span>

* <span data-ttu-id="23101-128">IIS Express</span><span class="sxs-lookup"><span data-stu-id="23101-128">IIS Express</span></span>
* <span data-ttu-id="23101-129">IIS</span><span class="sxs-lookup"><span data-stu-id="23101-129">IIS</span></span>
* <span data-ttu-id="23101-130">Projeto (que inicia o Kestrel)</span><span class="sxs-lookup"><span data-stu-id="23101-130">Project (which launches Kestrel)</span></span>

<span data-ttu-id="23101-131">Quando um aplicativo for iniciado com [dotnet run](/dotnet/core/tools/dotnet-run):</span><span class="sxs-lookup"><span data-stu-id="23101-131">When an app is launched with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

* <span data-ttu-id="23101-132">*launchSettings.json* é lido, se está disponível.</span><span class="sxs-lookup"><span data-stu-id="23101-132">*launchSettings.json* is read if available.</span></span> <span data-ttu-id="23101-133">As configurações de `environmentVariables` em *launchSettings.json* substituem as variáveis de ambiente.</span><span class="sxs-lookup"><span data-stu-id="23101-133">`environmentVariables` settings in *launchSettings.json* override environment variables.</span></span>
* <span data-ttu-id="23101-134">O ambiente de hospedagem é exibido.</span><span class="sxs-lookup"><span data-stu-id="23101-134">The hosting environment is displayed.</span></span>


<span data-ttu-id="23101-135">A saída a seguir mostra um aplicativo iniciado com [dotnet run](/dotnet/core/tools/dotnet-run):</span><span class="sxs-lookup"><span data-stu-id="23101-135">The following output shows an app started with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>
```bash
PS C:\Webs\WebApp1> dotnet run
Using launch settings from C:\Webs\WebApp1\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Webs\WebApp1
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="23101-136">A guia **Depurar** do Visual Studio fornece uma GUI para editar o arquivo *launchSettings.json*:</span><span class="sxs-lookup"><span data-stu-id="23101-136">The Visual Studio **Debug** tab provides a GUI to edit the *launchSettings.json* file:</span></span>

![Configurando variáveis de ambiente das propriedades do projeto](environments/_static/project-properties-debug.png)

<span data-ttu-id="23101-138">As alterações feitas nos perfis do projeto poderão não ter efeito até que o servidor Web seja reiniciado.</span><span class="sxs-lookup"><span data-stu-id="23101-138">Changes made to project profiles may not take effect until the web server is restarted.</span></span> <span data-ttu-id="23101-139">O Kestrel precisa ser reiniciado antes de detectar as alterações feitas em seu ambiente.</span><span class="sxs-lookup"><span data-stu-id="23101-139">Kestrel must be restarted before it will detect changes made to its environment.</span></span>

>[!WARNING]
> <span data-ttu-id="23101-140">*launchSettings.json* não deve armazenar segredos.</span><span class="sxs-lookup"><span data-stu-id="23101-140">*launchSettings.json* shouldn't store secrets.</span></span> <span data-ttu-id="23101-141">A [ferramenta Secret Manager](xref:security/app-secrets) pode ser usado para armazenar segredos de desenvolvimento local.</span><span class="sxs-lookup"><span data-stu-id="23101-141">The [Secret Manager tool](xref:security/app-secrets) can be used to store secrets for local development.</span></span>

### <a name="production"></a><span data-ttu-id="23101-142">Produção</span><span class="sxs-lookup"><span data-stu-id="23101-142">Production</span></span>

<span data-ttu-id="23101-143">O ambiente de produção deve ser configurado para maximizar a segurança, o desempenho e a robustez do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="23101-143">The production environment should be configured to maximize security, performance, and application robustness.</span></span> <span data-ttu-id="23101-144">Algumas configurações comuns que são diferentes do desenvolvimento incluem:</span><span class="sxs-lookup"><span data-stu-id="23101-144">Some common settings that differ from development include:</span></span>

* <span data-ttu-id="23101-145">Cache.</span><span class="sxs-lookup"><span data-stu-id="23101-145">Caching.</span></span>
* <span data-ttu-id="23101-146">Recursos do lado do cliente são agrupados, minimizados e potencialmente atendidos por meio de uma CDN.</span><span class="sxs-lookup"><span data-stu-id="23101-146">Client-side resources are bundled, minified, and potentially served from a CDN.</span></span>
* <span data-ttu-id="23101-147">Páginas de erro de diagnóstico desabilitadas.</span><span class="sxs-lookup"><span data-stu-id="23101-147">Diagnostic error pages disabled.</span></span>
* <span data-ttu-id="23101-148">Páginas de erro amigáveis habilitadas.</span><span class="sxs-lookup"><span data-stu-id="23101-148">Friendly error pages enabled.</span></span>
* <span data-ttu-id="23101-149">Log de produção e monitoramento habilitados.</span><span class="sxs-lookup"><span data-stu-id="23101-149">Production logging and monitoring enabled.</span></span> <span data-ttu-id="23101-150">Por exemplo, [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span><span class="sxs-lookup"><span data-stu-id="23101-150">For example, [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="setting-the-environment"></a><span data-ttu-id="23101-151">Configurando o ambiente</span><span class="sxs-lookup"><span data-stu-id="23101-151">Setting the environment</span></span>

<span data-ttu-id="23101-152">Geralmente, é útil definir um ambiente específico para teste.</span><span class="sxs-lookup"><span data-stu-id="23101-152">It's often useful to set a specific environment for testing.</span></span> <span data-ttu-id="23101-153">Se o ambiente não for definido, ele usará `Production` como padrão, o que desabilita a maioria dos recursos de depuração.</span><span class="sxs-lookup"><span data-stu-id="23101-153">If the environment isn't set, it will default to `Production` which disables most debugging features.</span></span>

<span data-ttu-id="23101-154">O método para configurar o ambiente depende do sistema operacional.</span><span class="sxs-lookup"><span data-stu-id="23101-154">The method for setting the environment depends on the operating system.</span></span>

### <a name="azure"></a><span data-ttu-id="23101-155">Azure</span><span class="sxs-lookup"><span data-stu-id="23101-155">Azure</span></span>

<span data-ttu-id="23101-156">Para o Serviço de Aplicativo do Azure:</span><span class="sxs-lookup"><span data-stu-id="23101-156">For Azure app service:</span></span>

* <span data-ttu-id="23101-157">Selecione a folha **Configurações de Aplicativo**.</span><span class="sxs-lookup"><span data-stu-id="23101-157">Select the **Application settings** blade.</span></span>
* <span data-ttu-id="23101-158">Adicione a chave e o valor em **Configurações de aplicativo**.</span><span class="sxs-lookup"><span data-stu-id="23101-158">Add the key and value in **App settings**.</span></span>


### <a name="windows"></a><span data-ttu-id="23101-159">Windows</span><span class="sxs-lookup"><span data-stu-id="23101-159">Windows</span></span>
<span data-ttu-id="23101-160">Para definir o `ASPNETCORE_ENVIRONMENT` para a sessão atual, se o aplicativo for iniciado usando [dotnet run](/dotnet/core/tools/dotnet-run), os comandos a seguir serão usados</span><span class="sxs-lookup"><span data-stu-id="23101-160">To set the `ASPNETCORE_ENVIRONMENT` for the current session, if the app is started using [dotnet run](/dotnet/core/tools/dotnet-run), the following commands are used</span></span>

<span data-ttu-id="23101-161">**Linha de comando**</span><span class="sxs-lookup"><span data-stu-id="23101-161">**Command line**</span></span>
```
set ASPNETCORE_ENVIRONMENT=Development
```
<span data-ttu-id="23101-162">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="23101-162">**PowerShell**</span></span>
```
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

<span data-ttu-id="23101-163">Esses comandos têm efeito somente para a janela atual.</span><span class="sxs-lookup"><span data-stu-id="23101-163">These commands take effect only for the current window.</span></span> <span data-ttu-id="23101-164">Quando a janela é fechada, a configuração ASPNETCORE_ENVIRONMENT é revertida para a configuração padrão ou o valor do computador.</span><span class="sxs-lookup"><span data-stu-id="23101-164">When the window is closed, the ASPNETCORE_ENVIRONMENT setting reverts to the default setting or machine value.</span></span> <span data-ttu-id="23101-165">Para definir o valor globalmente no Windows, abra o **Painel de Controle** > **Sistema** > **Configurações avançadas do sistema** e adicione ou edite o valor `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="23101-165">In order to set the value globally on Windows open the **Control Panel** > **System** > **Advanced system settings** and add or edit the `ASPNETCORE_ENVIRONMENT` value.</span></span>

![Propriedades avançadas do sistema](environments/_static/systemsetting_environment.png)

![Variável de ambiente do ASP.NET Core](environments/_static/windows_aspnetcore_environment.png)


<span data-ttu-id="23101-168">**web.config**</span><span class="sxs-lookup"><span data-stu-id="23101-168">**web.config**</span></span>

<span data-ttu-id="23101-169">Consulte a seção *Configurando variáveis de ambiente* do tópico [Referência de configuração do Módulo do ASP.NET Core](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span><span class="sxs-lookup"><span data-stu-id="23101-169">See the *Setting environment variables* section of the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) topic.</span></span>

<span data-ttu-id="23101-170">**Por pool de aplicativos do IIS**</span><span class="sxs-lookup"><span data-stu-id="23101-170">**Per IIS Application Pool**</span></span>

<span data-ttu-id="23101-171">Para definir variáveis de ambiente para aplicativos individuais executados em Pools de Aplicativos isolados (compatíveis com o IIS 10.0+), consulte a seção *Comando AppCmd.exe* do tópico [Variáveis de ambiente \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe).</span><span class="sxs-lookup"><span data-stu-id="23101-171">To set environment variables for individual apps running in isolated Application Pools (supported on IIS 10.0+), see the *AppCmd.exe command* section of the [Environment Variables \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic.</span></span>

### <a name="macos"></a><span data-ttu-id="23101-172">macOS</span><span class="sxs-lookup"><span data-stu-id="23101-172">macOS</span></span>
<span data-ttu-id="23101-173">A configuração do ambiente atual para o macOS pode ser feita em linha durante a execução do aplicativo</span><span class="sxs-lookup"><span data-stu-id="23101-173">Setting the current environment for macOS can be done in-line when running the application;</span></span>

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```
<span data-ttu-id="23101-174">ou usando `export` para configurá-lo antes da execução do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="23101-174">or using `export` to set it prior to running the app.</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```
<span data-ttu-id="23101-175">As variáveis de ambiente no nível do computador são definidas no arquivo *.bashrc* ou *.bash_profile*.</span><span class="sxs-lookup"><span data-stu-id="23101-175">Machine level environment variables are set in the *.bashrc* or *.bash_profile* file.</span></span> <span data-ttu-id="23101-176">Edite o arquivo usando qualquer editor de texto e adicione a instrução a seguir.</span><span class="sxs-lookup"><span data-stu-id="23101-176">Edit the file using any text editor and add the following statment.</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

### <a name="linux"></a><span data-ttu-id="23101-177">Linux</span><span class="sxs-lookup"><span data-stu-id="23101-177">Linux</span></span>
<span data-ttu-id="23101-178">Para distribuições Linux, use o comando `export` na linha de comando para as configurações de variável baseadas na sessão e o arquivo *bash_profile* para as configurações de ambiente no nível do computador.</span><span class="sxs-lookup"><span data-stu-id="23101-178">For Linux distros, use the `export` command at the command line for session based variable settings and *bash_profile* file for machine level environment settings.</span></span>

### <a name="configuration-by-environment"></a><span data-ttu-id="23101-179">Configuração por ambiente</span><span class="sxs-lookup"><span data-stu-id="23101-179">Configuration by environment</span></span>

<span data-ttu-id="23101-180">Consulte [Configuração por ambiente](xref:fundamentals/configuration/index#configuration-by-environment) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="23101-180">See [Configuration by environment](xref:fundamentals/configuration/index#configuration-by-environment) for more information.</span></span>

<a name="startup-conventions"></a>
## <a name="environment-based-startup-class-and-methods"></a><span data-ttu-id="23101-181">Classe Startup e métodos baseados no ambiente</span><span class="sxs-lookup"><span data-stu-id="23101-181">Environment based Startup class and methods</span></span>

<span data-ttu-id="23101-182">Quando um aplicativo ASP.NET Core é iniciado, a [classe Startup](xref:fundamentals/startup) inicia o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="23101-182">When an ASP.NET Core app starts, the [Startup class](xref:fundamentals/startup) bootstraps the app.</span></span> <span data-ttu-id="23101-183">Se houver uma classe `Startup{EnvironmentName}`, essa classe será chamada para esse `EnvironmentName`:</span><span class="sxs-lookup"><span data-stu-id="23101-183">If a class `Startup{EnvironmentName}` exists, that class will be called for that `EnvironmentName`:</span></span>

[!code-csharp[](environments/sample/WebApp1/StartupDev.cs?name=snippet&highlight=1)]

<span data-ttu-id="23101-184">Observação: a chamada a [WebHostBuilder.UseStartup<TStartup> ](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) substitui as seções de configuração.</span><span class="sxs-lookup"><span data-stu-id="23101-184">Note: Calling [WebHostBuilder.UseStartup<TStartup>](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) overrides configuration sections.</span></span>

<span data-ttu-id="23101-185">[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_StartupBase_Configure_Microsoft_AspNetCore_Builder_IApplicationBuilder_) e [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices?view=aspnetcore-2.0) são compatíveis com versões específicas ao ambiente do formato `Configure{EnvironmentName}` e `Configure{EnvironmentName}Services`:</span><span class="sxs-lookup"><span data-stu-id="23101-185">[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_StartupBase_Configure_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices?view=aspnetcore-2.0) support environment specific versions of the form `Configure{EnvironmentName}` and `Configure{EnvironmentName}Services`:</span></span>

[!code-csharp[](environments/sample/WebApp1/Startup.cs?name=snippet_all&highlight=15,37)]

## <a name="additional-resources"></a><span data-ttu-id="23101-186">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="23101-186">Additional resources</span></span>

* [<span data-ttu-id="23101-187">Inicialização de aplicativos</span><span class="sxs-lookup"><span data-stu-id="23101-187">Application startup</span></span>](xref:fundamentals/startup)
* [<span data-ttu-id="23101-188">Configuração</span><span class="sxs-lookup"><span data-stu-id="23101-188">Configuration</span></span>](xref:fundamentals/configuration/index)
* [<span data-ttu-id="23101-189">IHostingEnvironment.EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="23101-189">IHostingEnvironment.EnvironmentName</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName)

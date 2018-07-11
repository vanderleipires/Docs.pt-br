---
title: Usar vários ambientes no ASP.NET Core
author: rick-anderson
description: Aprenda a controlar o comportamento do aplicativo em vários ambientes em aplicativos do ASP.NET Core.
ms.author: riande
ms.date: 07/03/2018
uid: fundamentals/environments
ms.openlocfilehash: 8983a0ce81beb16d68c799d30bfbfce6e7b693b1
ms.sourcegitcommit: 18339e3cb5a891a3ca36d8146fa83cf91c32e707
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37433942"
---
# <a name="use-multiple-environments-in-aspnet-core"></a><span data-ttu-id="24f50-103">Usar vários ambientes no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="24f50-103">Use multiple environments in ASP.NET Core</span></span>

<span data-ttu-id="24f50-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="24f50-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="24f50-105">O ASP.NET Core configura o comportamento do aplicativo com base no ambiente de tempo de execução usando uma variável de ambiente.</span><span class="sxs-lookup"><span data-stu-id="24f50-105">ASP.NET Core configures app behavior based on the runtime environment using an environment variable.</span></span>

<span data-ttu-id="24f50-106">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="24f50-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="environments"></a><span data-ttu-id="24f50-107">Ambientes</span><span class="sxs-lookup"><span data-stu-id="24f50-107">Environments</span></span>

<span data-ttu-id="24f50-108">O ASP.NET Core lê a variável de ambiente `ASPNETCORE_ENVIRONMENT` na inicialização do aplicativo e armazena o valor em [IHostingEnvironment.EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname).</span><span class="sxs-lookup"><span data-stu-id="24f50-108">ASP.NET Core reads the environment variable `ASPNETCORE_ENVIRONMENT` at app startup and stores the value in [IHostingEnvironment.EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname).</span></span> <span data-ttu-id="24f50-109">Você pode definir `ASPNETCORE_ENVIRONMENT` como qualquer valor, mas há suporte para [três valores](/dotnet/api/microsoft.aspnetcore.hosting.environmentname) na estrutura: [Desenvolvimento](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development), [Preparo](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging) e [Produção](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production).</span><span class="sxs-lookup"><span data-stu-id="24f50-109">You can set `ASPNETCORE_ENVIRONMENT` to any value, but [three values](/dotnet/api/microsoft.aspnetcore.hosting.environmentname) are supported by the framework: [Development](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development), [Staging](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging), and [Production](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production).</span></span> <span data-ttu-id="24f50-110">Se `ASPNETCORE_ENVIRONMENT` não estiver definido, o padrão será `Production`.</span><span class="sxs-lookup"><span data-stu-id="24f50-110">If `ASPNETCORE_ENVIRONMENT` isn't set, it defaults to `Production`.</span></span>

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet)]

<span data-ttu-id="24f50-111">O código anterior:</span><span class="sxs-lookup"><span data-stu-id="24f50-111">The preceding code:</span></span>

* <span data-ttu-id="24f50-112">Chama [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) e [UseBrowserLink](/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink) quando `ASPNETCORE_ENVIRONMENT` é definido como `Development`.</span><span class="sxs-lookup"><span data-stu-id="24f50-112">Calls [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) and [UseBrowserLink](/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink) when `ASPNETCORE_ENVIRONMENT` is set to `Development`.</span></span>
* <span data-ttu-id="24f50-113">Chama [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) quando o valor de `ASPNETCORE_ENVIRONMENT` é definido com um dos seguintes:</span><span class="sxs-lookup"><span data-stu-id="24f50-113">Calls [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) when the value of `ASPNETCORE_ENVIRONMENT` is set one of the following:</span></span>

    * `Staging`
    * `Production`
    * `Staging_2`

<span data-ttu-id="24f50-114">O [Auxiliar de Marcação de Ambiente](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) usa o valor de `IHostingEnvironment.EnvironmentName` para incluir ou excluir a marcação no elemento:</span><span class="sxs-lookup"><span data-stu-id="24f50-114">The [Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) uses the value of `IHostingEnvironment.EnvironmentName` to include or exclude markup in the element:</span></span>

[!code-cshtml[](environments/sample-snapshot/EnvironmentsSample/Pages/About.cshtml)]

<span data-ttu-id="24f50-115">No Windows e no macOS, valores e variáveis de ambiente não diferenciam maiúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="24f50-115">On Windows and macOS, environment variables and values aren't case sensitive.</span></span> <span data-ttu-id="24f50-116">Valores e variáveis de ambiente do Linux **diferenciam maiúsculas de minúsculas** por padrão.</span><span class="sxs-lookup"><span data-stu-id="24f50-116">Linux environment variables and values are **case sensitive** by default.</span></span>

### <a name="development"></a><span data-ttu-id="24f50-117">Desenvolvimento</span><span class="sxs-lookup"><span data-stu-id="24f50-117">Development</span></span>

<span data-ttu-id="24f50-118">O ambiente de desenvolvimento pode habilitar recursos que não devem ser expostos em produção.</span><span class="sxs-lookup"><span data-stu-id="24f50-118">The development environment can enable features that shouldn't be exposed in production.</span></span> <span data-ttu-id="24f50-119">Por exemplo, os modelos do ASP.NET Core habilitam a [página de exceção do desenvolvedor](xref:fundamentals/error-handling#the-developer-exception-page) no ambiente de desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="24f50-119">For example, the ASP.NET Core templates enable the [developer exception page](xref:fundamentals/error-handling#the-developer-exception-page) in the development environment.</span></span>

<span data-ttu-id="24f50-120">O ambiente de desenvolvimento do computador local pode ser definido no arquivo *Properties\launchSettings.json* do projeto.</span><span class="sxs-lookup"><span data-stu-id="24f50-120">The environment for local machine development can be set in the *Properties\launchSettings.json* file of the project.</span></span> <span data-ttu-id="24f50-121">Os valores de ambiente definidos em *launchSettings.json* substituem os valores definidos no ambiente do sistema.</span><span class="sxs-lookup"><span data-stu-id="24f50-121">Environment values set in *launchSettings.json* override values set in the system environment.</span></span>

<span data-ttu-id="24f50-122">O seguinte JSON mostra três perfis de um arquivo *launchSettings.json*:</span><span class="sxs-lookup"><span data-stu-id="24f50-122">The following JSON shows three profiles from a *launchSettings.json* file:</span></span>

```json
{
  "iisSettings": {
    "windowsAuthentication": false,
    "anonymousAuthentication": true,
    "iisExpress": {
      "applicationUrl": "http://localhost:54339/",
      "sslPort": 0
    }
  },
  "profiles": {
    "IIS Express": {
      "commandName": "IISExpress",
      "launchBrowser": true,
      "environmentVariables": {
        "ASPNETCORE_My_Environment": "1",
        "ASPNETCORE_DETAILEDERRORS": "1",
        "ASPNETCORE_ENVIRONMENT": "Staging"
      }
    },
    "EnvironmentsSample": {
      "commandName": "Project",
      "launchBrowser": true,
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Staging"
      },
      "applicationUrl": "http://localhost:54340/"
    },
    "Kestrel Staging": {
      "commandName": "Project",
      "launchBrowser": true,
      "environmentVariables": {
        "ASPNETCORE_My_Environment": "1",
        "ASPNETCORE_DETAILEDERRORS": "1",
        "ASPNETCORE_ENVIRONMENT": "Staging"
      },
      "applicationUrl": "http://localhost:51997/"
    }
  }
}
```

::: moniker range=">= aspnetcore-2.1"

> [!NOTE]
> <span data-ttu-id="24f50-123">A propriedade `applicationUrl` no *launchSettings.json* pode especificar uma lista de URLs de servidores.</span><span class="sxs-lookup"><span data-stu-id="24f50-123">The `applicationUrl` property in *launchSettings.json* can specify a list of server URLs.</span></span> <span data-ttu-id="24f50-124">Use um ponto e vírgula entre as URLs na lista:</span><span class="sxs-lookup"><span data-stu-id="24f50-124">Use a semicolon between the URLs in the list:</span></span>
>
> ```json
> "EnvironmentsSample": {
>    "commandName": "Project",
>    "launchBrowser": true,
>    "applicationUrl": "https://localhost:5001;http://localhost:5000",
>    "environmentVariables": {
>      "ASPNETCORE_ENVIRONMENT": "Development"
>    }
> }
> ```

::: moniker-end

<span data-ttu-id="24f50-125">Quando o aplicativo é inicializado com [dotnet run](/dotnet/core/tools/dotnet-run), o primeiro perfil com `"commandName": "Project"` é usado.</span><span class="sxs-lookup"><span data-stu-id="24f50-125">When the app is launched with [dotnet run](/dotnet/core/tools/dotnet-run), the first profile with `"commandName": "Project"` is used.</span></span> <span data-ttu-id="24f50-126">O valor de `commandName` especifica o servidor Web a ser iniciado.</span><span class="sxs-lookup"><span data-stu-id="24f50-126">The value of `commandName` specifies the web server to launch.</span></span> <span data-ttu-id="24f50-127">`commandName` pode ser qualquer um dos seguintes:</span><span class="sxs-lookup"><span data-stu-id="24f50-127">`commandName` can be any one of the following:</span></span>

* <span data-ttu-id="24f50-128">IIS Express</span><span class="sxs-lookup"><span data-stu-id="24f50-128">IIS Express</span></span>
* <span data-ttu-id="24f50-129">IIS</span><span class="sxs-lookup"><span data-stu-id="24f50-129">IIS</span></span>
* <span data-ttu-id="24f50-130">Projeto (que inicia o Kestrel)</span><span class="sxs-lookup"><span data-stu-id="24f50-130">Project (which launches Kestrel)</span></span>

<span data-ttu-id="24f50-131">Quando um aplicativo for iniciado com [dotnet run](/dotnet/core/tools/dotnet-run):</span><span class="sxs-lookup"><span data-stu-id="24f50-131">When an app is launched with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

* <span data-ttu-id="24f50-132">*launchSettings.json* é lido, se está disponível.</span><span class="sxs-lookup"><span data-stu-id="24f50-132">*launchSettings.json* is read if available.</span></span> <span data-ttu-id="24f50-133">As configurações de `environmentVariables` em *launchSettings.json* substituem as variáveis de ambiente.</span><span class="sxs-lookup"><span data-stu-id="24f50-133">`environmentVariables` settings in *launchSettings.json* override environment variables.</span></span>
* <span data-ttu-id="24f50-134">O ambiente de hospedagem é exibido.</span><span class="sxs-lookup"><span data-stu-id="24f50-134">The hosting environment is displayed.</span></span>

<span data-ttu-id="24f50-135">A saída a seguir mostra um aplicativo iniciado com [dotnet run](/dotnet/core/tools/dotnet-run):</span><span class="sxs-lookup"><span data-stu-id="24f50-135">The following output shows an app started with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

```bash
PS C:\Websites\EnvironmentsSample> dotnet run
Using launch settings from C:\Websites\EnvironmentsSample\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Websites\EnvironmentsSample
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="24f50-136">A guia **Depurar** das propriedades do projeto do Visual Studio fornece uma GUI para editar o arquivo *launchSettings.json*:</span><span class="sxs-lookup"><span data-stu-id="24f50-136">The Visual Studio project properties **Debug** tab provides a GUI to edit the *launchSettings.json* file:</span></span>

![Configurando variáveis de ambiente das propriedades do projeto](environments/_static/project-properties-debug.png)

<span data-ttu-id="24f50-138">As alterações feitas nos perfis do projeto poderão não ter efeito até que o servidor Web seja reiniciado.</span><span class="sxs-lookup"><span data-stu-id="24f50-138">Changes made to project profiles may not take effect until the web server is restarted.</span></span> <span data-ttu-id="24f50-139">O Kestrel precisa ser reiniciado antes de detectar as alterações feitas ao seu ambiente.</span><span class="sxs-lookup"><span data-stu-id="24f50-139">Kestrel must be restarted before it can detect changes made to its environment.</span></span>

> [!WARNING]
> <span data-ttu-id="24f50-140">*launchSettings.json* não deve armazenar segredos.</span><span class="sxs-lookup"><span data-stu-id="24f50-140">*launchSettings.json* shouldn't store secrets.</span></span> <span data-ttu-id="24f50-141">A [ferramenta Secret Manager](xref:security/app-secrets) pode ser usado para armazenar segredos de desenvolvimento local.</span><span class="sxs-lookup"><span data-stu-id="24f50-141">The [Secret Manager tool](xref:security/app-secrets) can be used to store secrets for local development.</span></span>

<span data-ttu-id="24f50-142">Ao usar [Visual Studio Code](https://code.visualstudio.com/), variáveis de ambiente podem ser definidas no arquivo *.vscode/launch.json*.</span><span class="sxs-lookup"><span data-stu-id="24f50-142">When using [Visual Studio Code](https://code.visualstudio.com/), environment variables can be set in the *.vscode/launch.json* file.</span></span> <span data-ttu-id="24f50-143">O exemplo a seguir define o ambiente como `Development`:</span><span class="sxs-lookup"><span data-stu-id="24f50-143">The following example sets the environment to `Development`:</span></span>

```json
{
   "version": "0.2.0",
   "configurations": [
        {
            "name": ".NET Core Launch (web)",

            ... additional VS Code configuration settings ...

            "env": {
                "ASPNETCORE_ENVIRONMENT": "Development"
            }
        }
    ]
}
```

<span data-ttu-id="24f50-144">Um arquivo *.vscode/launch.json* do projeto não é lido ao iniciar o aplicativo com `dotnet run` da mesma maneira que *Properties/launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="24f50-144">A *.vscode/launch.json* file in the project isn't read when starting the app with `dotnet run` in the same way as *Properties/launchSettings.json*.</span></span> <span data-ttu-id="24f50-145">Ao inicializar um aplicativo em desenvolvimento que não tem um arquivo *launchSettings.json*, defina o ambiente com uma variável de ambiente ou um argumento de linha de comando para o comando `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="24f50-145">When launching an app in development that doesn't have a *launchSettings.json* file, either set the environment with an environment variable or a command-line argument to the `dotnet run` command.</span></span>

### <a name="production"></a><span data-ttu-id="24f50-146">Produção</span><span class="sxs-lookup"><span data-stu-id="24f50-146">Production</span></span>

<span data-ttu-id="24f50-147">O ambiente de produção deve ser configurado para maximizar a segurança, o desempenho e a robustez do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="24f50-147">The production environment should be configured to maximize security, performance, and app robustness.</span></span> <span data-ttu-id="24f50-148">Algumas configurações comuns que são diferentes do desenvolvimento incluem:</span><span class="sxs-lookup"><span data-stu-id="24f50-148">Some common settings that differ from development include:</span></span>

* <span data-ttu-id="24f50-149">Cache.</span><span class="sxs-lookup"><span data-stu-id="24f50-149">Caching.</span></span>
* <span data-ttu-id="24f50-150">Recursos do lado do cliente são agrupados, minimizados e potencialmente atendidos por meio de uma CDN.</span><span class="sxs-lookup"><span data-stu-id="24f50-150">Client-side resources are bundled, minified, and potentially served from a CDN.</span></span>
* <span data-ttu-id="24f50-151">Páginas de erro de diagnóstico desabilitadas.</span><span class="sxs-lookup"><span data-stu-id="24f50-151">Diagnostic error pages disabled.</span></span>
* <span data-ttu-id="24f50-152">Páginas de erro amigáveis habilitadas.</span><span class="sxs-lookup"><span data-stu-id="24f50-152">Friendly error pages enabled.</span></span>
* <span data-ttu-id="24f50-153">Log de produção e monitoramento habilitados.</span><span class="sxs-lookup"><span data-stu-id="24f50-153">Production logging and monitoring enabled.</span></span> <span data-ttu-id="24f50-154">Por exemplo, [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span><span class="sxs-lookup"><span data-stu-id="24f50-154">For example, [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="set-the-environment"></a><span data-ttu-id="24f50-155">Definir o ambiente</span><span class="sxs-lookup"><span data-stu-id="24f50-155">Set the environment</span></span>

<span data-ttu-id="24f50-156">Geralmente, é útil definir um ambiente específico para teste.</span><span class="sxs-lookup"><span data-stu-id="24f50-156">It's often useful to set a specific environment for testing.</span></span> <span data-ttu-id="24f50-157">Se o ambiente não for definido, ele usará `Production` como padrão, o que desabilitará a maioria dos recursos de depuração.</span><span class="sxs-lookup"><span data-stu-id="24f50-157">If the environment isn't set, it defaults to `Production`, which disables most debugging features.</span></span> <span data-ttu-id="24f50-158">O método para configurar o ambiente depende do sistema operacional.</span><span class="sxs-lookup"><span data-stu-id="24f50-158">The method for setting the environment depends on the operating system.</span></span>

### <a name="azure-app-service"></a><span data-ttu-id="24f50-159">Serviço de Aplicativo do Azure</span><span class="sxs-lookup"><span data-stu-id="24f50-159">Azure App Service</span></span>

<span data-ttu-id="24f50-160">Para definir o ambiente no [Serviço de Aplicativo do Azure](https://azure.microsoft.com/services/app-service/), execute as seguintes etapas:</span><span class="sxs-lookup"><span data-stu-id="24f50-160">To set the environment in [Azure App Service](https://azure.microsoft.com/services/app-service/), perform the following steps:</span></span>

1. <span data-ttu-id="24f50-161">Selecione o aplicativo na folha **Serviços de Aplicativos**.</span><span class="sxs-lookup"><span data-stu-id="24f50-161">Select the app from the **App Services** blade.</span></span>
1. <span data-ttu-id="24f50-162">No grupo **CONFIGURAÇÕES**, selecione a folha **Configurações do aplicativo**.</span><span class="sxs-lookup"><span data-stu-id="24f50-162">In the **SETTINGS** group, select the **Application settings** blade.</span></span>
1. <span data-ttu-id="24f50-163">Na área **Configurações do aplicativo**, selecione **Adicionar nova configuração**.</span><span class="sxs-lookup"><span data-stu-id="24f50-163">In the **Application settings** area, select **Add new setting**.</span></span>
1. <span data-ttu-id="24f50-164">Para **Inserir um nome**, forneça `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="24f50-164">For **Enter a name**, provide `ASPNETCORE_ENVIRONMENT`.</span></span> <span data-ttu-id="24f50-165">Para **Inserir um valor**, fornecer o ambiente (por exemplo, `Staging`).</span><span class="sxs-lookup"><span data-stu-id="24f50-165">For **Enter a value**, provide the environment (for example, `Staging`).</span></span>
1. <span data-ttu-id="24f50-166">Marque a caixa de seleção **Configuração do Slot** se desejar que a configuração do ambiente permaneça no slot atual quando os slots de implantação forem trocados.</span><span class="sxs-lookup"><span data-stu-id="24f50-166">Select the **Slot Setting** check box if you wish the environment setting to remain with the current slot when deployment slots are swapped.</span></span> <span data-ttu-id="24f50-167">Para obter mais informações, veja [Documentação do Azure: que configurações são trocadas?](/azure/app-service/web-sites-staged-publishing).</span><span class="sxs-lookup"><span data-stu-id="24f50-167">For more information, see [Azure Documentation: Which settings are swapped?](/azure/app-service/web-sites-staged-publishing).</span></span>
1. <span data-ttu-id="24f50-168">Selecione **Salvar** na parte superior da folha.</span><span class="sxs-lookup"><span data-stu-id="24f50-168">Select **Save** at the top of the blade.</span></span>

<span data-ttu-id="24f50-169">O Serviço de Aplicativo do Azure reinicia automaticamente o aplicativo após uma configuração de aplicativo (variável de ambiente) ser adicionada, alterada ou excluída no portal do Azure.</span><span class="sxs-lookup"><span data-stu-id="24f50-169">Azure App Service automatically restarts the app after an app setting (environment variable) is added, changed, or deleted in the Azure portal.</span></span>

### <a name="windows"></a><span data-ttu-id="24f50-170">Windows</span><span class="sxs-lookup"><span data-stu-id="24f50-170">Windows</span></span>

<span data-ttu-id="24f50-171">Para definir o `ASPNETCORE_ENVIRONMENT` para a sessão atual quando o aplicativo for iniciado usando [dotnet run](/dotnet/core/tools/dotnet-run), os comandos a seguir serão usados:</span><span class="sxs-lookup"><span data-stu-id="24f50-171">To set the `ASPNETCORE_ENVIRONMENT` for the current session when the app is started using [dotnet run](/dotnet/core/tools/dotnet-run), the following commands are used:</span></span>

<span data-ttu-id="24f50-172">**Prompt de comando**</span><span class="sxs-lookup"><span data-stu-id="24f50-172">**Command prompt**</span></span>

```console
set ASPNETCORE_ENVIRONMENT=Development
```

<span data-ttu-id="24f50-173">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="24f50-173">**PowerShell**</span></span>

```powershell
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

<span data-ttu-id="24f50-174">Esses comandos somente têm efeito para a janela atual.</span><span class="sxs-lookup"><span data-stu-id="24f50-174">These commands only take effect for the current window.</span></span> <span data-ttu-id="24f50-175">Quando a janela é fechada, a configuração `ASPNETCORE_ENVIRONMENT` é revertida para a configuração padrão ou o valor de computador.</span><span class="sxs-lookup"><span data-stu-id="24f50-175">When the window is closed, the `ASPNETCORE_ENVIRONMENT` setting reverts to the default setting or machine value.</span></span> <span data-ttu-id="24f50-176">Para definir o valor globalmente no Windows, abra **Painel de Controle** > **Sistema** > **Configurações avançadas do sistema** e adicione ou edite o valor `ASPNETCORE_ENVIRONMENT`:</span><span class="sxs-lookup"><span data-stu-id="24f50-176">To set the value globally in Windows, open the **Control Panel** > **System** > **Advanced system settings** and add or edit the `ASPNETCORE_ENVIRONMENT` value:</span></span>

![Propriedades avançadas do sistema](environments/_static/systemsetting_environment.png)

![Variável de ambiente do ASP.NET Core](environments/_static/windows_aspnetcore_environment.png)

<span data-ttu-id="24f50-179">**web.config**</span><span class="sxs-lookup"><span data-stu-id="24f50-179">**web.config**</span></span>

<span data-ttu-id="24f50-180">Consulte a seção *Configurando variáveis de ambiente* do tópico [Referência de configuração do Módulo do ASP.NET Core](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span><span class="sxs-lookup"><span data-stu-id="24f50-180">See the *Setting environment variables* section of the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) topic.</span></span>

<span data-ttu-id="24f50-181">**Por pool de aplicativos do IIS**</span><span class="sxs-lookup"><span data-stu-id="24f50-181">**Per IIS Application Pool**</span></span>

<span data-ttu-id="24f50-182">Para definir variáveis de ambiente para aplicativos individuais executados em Pools de Aplicativos isolados (compatíveis com o IIS 10.0+), confira a seção *Comando AppCmd.exe* do tópico [Variáveis de ambiente &lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe).</span><span class="sxs-lookup"><span data-stu-id="24f50-182">To set environment variables for individual apps running in isolated Application Pools (supported on IIS 10.0+), see the *AppCmd.exe command* section of the [Environment Variables &lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic.</span></span>

### <a name="macos"></a><span data-ttu-id="24f50-183">macOS</span><span class="sxs-lookup"><span data-stu-id="24f50-183">macOS</span></span>

<span data-ttu-id="24f50-184">A configuração do ambiente atual para macOS pode ser feita em linha ao executar o aplicativo:</span><span class="sxs-lookup"><span data-stu-id="24f50-184">Setting the current environment for macOS can be performed in-line when running the app:</span></span>

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```

<span data-ttu-id="24f50-185">Como alternativa, defina o ambiente com `export` antes de executar o aplicativo:</span><span class="sxs-lookup"><span data-stu-id="24f50-185">Alternatively, set the environment with `export` prior to running the app:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

<span data-ttu-id="24f50-186">As variáveis de ambiente no nível do computador são definidas no arquivo *.bashrc* ou *.bash_profile*.</span><span class="sxs-lookup"><span data-stu-id="24f50-186">Machine-level environment variables are set in the *.bashrc* or *.bash_profile* file.</span></span> <span data-ttu-id="24f50-187">Edite o arquivo usando qualquer editor de texto.</span><span class="sxs-lookup"><span data-stu-id="24f50-187">Edit the file using any text editor.</span></span> <span data-ttu-id="24f50-188">Adicione a seguinte instrução:</span><span class="sxs-lookup"><span data-stu-id="24f50-188">Add the following statement:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

### <a name="linux"></a><span data-ttu-id="24f50-189">Linux</span><span class="sxs-lookup"><span data-stu-id="24f50-189">Linux</span></span>

<span data-ttu-id="24f50-190">Para distribuições Linux, use o comando `export` no prompt de comando para as configurações de variável baseadas na sessão e o arquivo *bash_profile* para as configurações de ambiente no nível do computador.</span><span class="sxs-lookup"><span data-stu-id="24f50-190">For Linux distros, use the `export` command at a command prompt for session-based variable settings and *bash_profile* file for machine-level environment settings.</span></span>

### <a name="configuration-by-environment"></a><span data-ttu-id="24f50-191">Configuração por ambiente</span><span class="sxs-lookup"><span data-stu-id="24f50-191">Configuration by environment</span></span>

<span data-ttu-id="24f50-192">Consulte [Configuração por ambiente](xref:fundamentals/configuration/index#configuration-by-environment) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="24f50-192">See [Configuration by environment](xref:fundamentals/configuration/index#configuration-by-environment) for more information.</span></span>

## <a name="environment-based-startup-class-and-methods"></a><span data-ttu-id="24f50-193">Métodos e classe Startup baseados no ambiente</span><span class="sxs-lookup"><span data-stu-id="24f50-193">Environment-based Startup class and methods</span></span>

<span data-ttu-id="24f50-194">Quando um aplicativo ASP.NET Core é iniciado, a [classe Startup](xref:fundamentals/startup) inicia o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="24f50-194">When an ASP.NET Core app starts, the [Startup class](xref:fundamentals/startup) bootstraps the app.</span></span> <span data-ttu-id="24f50-195">Se uma classe `Startup{EnvironmentName}` existir, a classe será chamada para aquele `EnvironmentName`:</span><span class="sxs-lookup"><span data-stu-id="24f50-195">If a `Startup{EnvironmentName}` class exists, the class is called for that `EnvironmentName`:</span></span>

[!code-csharp[](environments/sample/EnvironmentsSample/StartupDev.cs?name=snippet&highlight=1)]

<span data-ttu-id="24f50-196">[WebHostBuilder.UseStartup&lt;TStartup&gt;](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) substitui as seções de configuração.</span><span class="sxs-lookup"><span data-stu-id="24f50-196">[WebHostBuilder.UseStartup&lt;TStartup&gt;](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) overrides configuration sections.</span></span>

<span data-ttu-id="24f50-197">[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) e [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) são compatíveis com versões específicas do ambiente dos formatos `Configure{EnvironmentName}` e `Configure{EnvironmentName}Services`:</span><span class="sxs-lookup"><span data-stu-id="24f50-197">[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) support environment-specific versions of the form `Configure{EnvironmentName}` and `Configure{EnvironmentName}Services`:</span></span>

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet_all&highlight=15,51)]

## <a name="additional-resources"></a><span data-ttu-id="24f50-198">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="24f50-198">Additional resources</span></span>

* <xref:fundamentals/startup>
* <xref:fundamentals/configuration/index>
* [<span data-ttu-id="24f50-199">IHostingEnvironment.EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="24f50-199">IHostingEnvironment.EnvironmentName</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname)

---
title: Referência de configuração do Módulo do ASP.NET Core
author: guardrex
description: Saiba como configurar o módulo do ASP.NET Core para hospedar aplicativos do ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 11/12/2018
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: 32fbf2b19da2d088847279f447f9a72cedcf8085
ms.sourcegitcommit: 408921a932448f66cb46fd53c307a864f5323fe5
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/12/2018
ms.locfileid: "51570172"
---
# <a name="aspnet-core-module-configuration-reference"></a><span data-ttu-id="1cb6e-103">Referência de configuração do Módulo do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1cb6e-103">ASP.NET Core Module configuration reference</span></span>

<span data-ttu-id="1cb6e-104">Por [Luke Latham](https://github.com/guardrex), [Rick Anderson](https://twitter.com/RickAndMSFT), [Sourabh Shirhatti](https://twitter.com/sshirhatti) e [Justin Kotalik](https://github.com/jkotalik)</span><span class="sxs-lookup"><span data-stu-id="1cb6e-104">By [Luke Latham](https://github.com/guardrex), [Rick Anderson](https://twitter.com/RickAndMSFT), [Sourabh Shirhatti](https://twitter.com/sshirhatti), and [Justin Kotalik](https://github.com/jkotalik)</span></span>

<span data-ttu-id="1cb6e-105">Este documento fornece instruções sobre como configurar o Módulo do ASP.NET Core para hospedar aplicativos do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-105">This document provides instructions on how to configure the ASP.NET Core Module for hosting ASP.NET Core apps.</span></span> <span data-ttu-id="1cb6e-106">Para obter uma introdução ao Módulo do ASP.NET Core e instruções de instalação, veja a [visão geral do Módulo do ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="1cb6e-106">For an introduction to the ASP.NET Core Module and installation instructions, see the [ASP.NET Core Module overview](xref:fundamentals/servers/aspnet-core-module).</span></span>

::: moniker range=">= aspnetcore-2.2"

## <a name="hosting-model"></a><span data-ttu-id="1cb6e-107">Modelo de hospedagem</span><span class="sxs-lookup"><span data-stu-id="1cb6e-107">Hosting model</span></span>

<span data-ttu-id="1cb6e-108">Para aplicativos em execução no .NET Core 2.2 ou posterior, o módulo dá suporte a um modelo de hospedagem em processo para melhorar o desempenho em comparação com hospedagem do proxy reverso (fora de processo).</span><span class="sxs-lookup"><span data-stu-id="1cb6e-108">For apps running on .NET Core 2.2 or later, the module supports an in-process hosting model for improved performance compared to reverse-proxy (out-of-process) hosting.</span></span> <span data-ttu-id="1cb6e-109">Para obter mais informações, consulte <xref:fundamentals/servers/aspnet-core-module#aspnet-core-module-description>.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-109">For more information, see <xref:fundamentals/servers/aspnet-core-module#aspnet-core-module-description>.</span></span>

<span data-ttu-id="1cb6e-110">A hospedagem em processo é uma aceitação para os aplicativos existentes, mas o padrão dos modelos [dotnet new](/dotnet/core/tools/dotnet-new) é o modelo de hospedagem em processo para todos os cenários do IIS e IIS Express.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-110">In-procsess hosting is opt-in for existing apps, but [dotnet new](/dotnet/core/tools/dotnet-new) templates default to the in-process hosting model for all IIS and IIS Express scenarios.</span></span>

<span data-ttu-id="1cb6e-111">Para configurar um aplicativo para hospedagem em processo, adicione a propriedade `<AspNetCoreHostingModel>` ao arquivo de projeto do aplicativo (por exemplo, *MyApp.csproj*) com um valor de `inprocess` (a hospedagem de fora do processo é definida com `outofprocess`):</span><span class="sxs-lookup"><span data-stu-id="1cb6e-111">To configure an app for in-process hosting, add the `<AspNetCoreHostingModel>` property to the app's project file (for example, *MyApp.csproj*) with a value of `inprocess` (out-of-process hosting is set with `outofprocess`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>inprocess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="1cb6e-112">As seguintes características se aplicam ao hospedar em processo:</span><span class="sxs-lookup"><span data-stu-id="1cb6e-112">The following characteristics apply when hosting in-process:</span></span>

* <span data-ttu-id="1cb6e-113">O [servidor Kestrel](xref:fundamentals/servers/kestrel) não é usado.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-113">The [Kestrel server](xref:fundamentals/servers/kestrel) isn't used.</span></span> <span data-ttu-id="1cb6e-114">Uma implementação personalizada do <xref:Microsoft.AspNetCore.Hosting.Server.IServer>, `IISHttpServer` funciona como o servidor do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-114">A custom <xref:Microsoft.AspNetCore.Hosting.Server.IServer> implementation, `IISHttpServer` acts as the app's server.</span></span>

* <span data-ttu-id="1cb6e-115">O [requestTimeout atributo](#attributes-of-the-aspnetcore-element) não se aplica à hospedagem em processo.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-115">The [requestTimeout attribute](#attributes-of-the-aspnetcore-element) doesn't apply to in-process hosting.</span></span>

* <span data-ttu-id="1cb6e-116">Não há suporte para o compartilhamento do pool de aplicativos entre aplicativos.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-116">Sharing an app pool among apps isn't supported.</span></span> <span data-ttu-id="1cb6e-117">Use um pool de aplicativos por aplicativo.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-117">Use one app pool per app.</span></span>

* <span data-ttu-id="1cb6e-118">Ao usar a [Implantação da Web](/iis/publish/using-web-deploy/introduction-to-web-deploy) ou inserir manualmente um [arquivo app_offline.htm na implantação](xref:host-and-deploy/iis/index#locked-deployment-files), o aplicativo talvez não seja desligado imediatamente se houver uma conexão aberta.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-118">When using [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) or manually placing an [app_offline.htm file in the deployment](xref:host-and-deploy/iis/index#locked-deployment-files), the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="1cb6e-119">Por exemplo, uma conexão websocket pode atrasar o desligamento do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-119">For example, a websocket connection may delay app shut down.</span></span>

* <span data-ttu-id="1cb6e-120">A arquitetura (número de bit) do aplicativo e o tempo de execução instalado (x64 ou x86) devem corresponder à arquitetura do pool de aplicativos.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-120">The architecture (bitness) of the app and installed runtime (x64 or x86) must match the architecture of the app pool.</span></span>

* <span data-ttu-id="1cb6e-121">Se a configuração manual do host do aplicativo com `WebHostBuilder` (não usando [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host)) e o aplicativo não forem executados diretamente no servidor Kestrel (auto-hospedado), chame `UseKestrel` antes de chamar `UseIISIntegration`.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-121">If setting up the app's host manually with `WebHostBuilder` (not using [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host)) and the app is ever run directly on the Kestrel server (self-hosted), call `UseKestrel` before calling `UseIISIntegration`.</span></span> <span data-ttu-id="1cb6e-122">Se a ordem for invertida, a inicialização do host falhará.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-122">If the order is reversed, the host fails to start.</span></span>

* <span data-ttu-id="1cb6e-123">As desconexões do cliente são detectadas.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-123">Client disconnects are detected.</span></span> <span data-ttu-id="1cb6e-124">O token de cancelamento [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) é cancelado quando o cliente se desconecta.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-124">The [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) cancellation token is cancelled when the client disconnects.</span></span>

* <span data-ttu-id="1cb6e-125">`Directory.GetCurrentDirectory()` retorna o diretório de trabalho do processo iniciado pelo IIS em vez do diretório do aplicativo (por exemplo, *C:\Windows\System32\inetsrv* para *w3wp.exe*).</span><span class="sxs-lookup"><span data-stu-id="1cb6e-125">`Directory.GetCurrentDirectory()` returns the worker directory of the process started by IIS rather than the application directory (for example, *C:\Windows\System32\inetsrv* for *w3wp.exe*).</span></span>

### <a name="hosting-model-changes"></a><span data-ttu-id="1cb6e-126">Alterações no modelo de hospedagem</span><span class="sxs-lookup"><span data-stu-id="1cb6e-126">Hosting model changes</span></span>

<span data-ttu-id="1cb6e-127">Se a configuração `hostingModel` for alterada no arquivo *web.config* (explicado na seção [Configuração a Web.config](#configuration-with-webconfig)), o módulo reciclará o processo de trabalho do IIS.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-127">If the `hostingModel` setting is changed in the *web.config* file (explained in the [Configuration with web.config](#configuration-with-webconfig) section), the module recycles the worker process for IIS.</span></span>

<span data-ttu-id="1cb6e-128">Para o IIS Express, o módulo não recicla o processo de trabalho, mas em vez disso, dispara um desligamento normal do processo atual do IIS Express.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-128">For IIS Express, the module doesn't recycle the worker process but instead triggers a graceful shutdown of the current IIS Express process.</span></span> <span data-ttu-id="1cb6e-129">A próxima solicitação para o aplicativo gera um novo processo do IIS Express.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-129">The next request to the app spawns a new IIS Express process.</span></span>

### <a name="process-name"></a><span data-ttu-id="1cb6e-130">Nome do processo</span><span class="sxs-lookup"><span data-stu-id="1cb6e-130">Process name</span></span>

<span data-ttu-id="1cb6e-131">`Process.GetCurrentProcess().ProcessName` relata `w3wp`/`iisexpress` (em processo) ou `dotnet` (fora do processo).</span><span class="sxs-lookup"><span data-stu-id="1cb6e-131">`Process.GetCurrentProcess().ProcessName` reports `w3wp`/`iisexpress` (in-process) or `dotnet` (out-of-process).</span></span>

::: moniker-end

## <a name="configuration-with-webconfig"></a><span data-ttu-id="1cb6e-132">Configuração com web.config</span><span class="sxs-lookup"><span data-stu-id="1cb6e-132">Configuration with web.config</span></span>

<span data-ttu-id="1cb6e-133">O Módulo do ASP.NET Core está configurado com a seção `aspNetCore` do nó `system.webServer` no arquivo *web.config* do site.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-133">The ASP.NET Core Module is configured with the `aspNetCore` section of the `system.webServer` node in the site's *web.config* file.</span></span>

<span data-ttu-id="1cb6e-134">O seguinte arquivo *web.config* é publicado para uma [implantação dependente de estrutura](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) e configura o Módulo do ASP.NET Core para manipular solicitações de site:</span><span class="sxs-lookup"><span data-stu-id="1cb6e-134">The following *web.config* file is published for a [framework-dependent deployment](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) and configures the ASP.NET Core Module to handle site requests:</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <location path="." inheritInChildApplications="false">
    <system.webServer>
      <handlers>
        <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModuleV2" resourceType="Unspecified" />
      </handlers>
      <aspNetCore processPath="dotnet" 
                  arguments=".\MyApp.dll" 
                  stdoutLogEnabled="false" 
                  stdoutLogFile=".\logs\stdout" 
                  hostingModel="inprocess" />
    </system.webServer>
  </location>
</configuration>
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath="dotnet" 
                arguments=".\MyApp.dll" 
                stdoutLogEnabled="false" 
                stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

::: moniker-end

<span data-ttu-id="1cb6e-135">O seguinte *web.config* é publicado para uma [implantação autossuficiente](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span><span class="sxs-lookup"><span data-stu-id="1cb6e-135">The following *web.config* is published for a [self-contained deployment](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <location path="." inheritInChildApplications="false">
    <system.webServer>
      <handlers>
        <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModuleV2" resourceType="Unspecified" />
      </handlers>
      <aspNetCore processPath=".\MyApp.exe" 
                  stdoutLogEnabled="false" 
                  stdoutLogFile=".\logs\stdout" 
                  hostingModel="inprocess" />
    </system.webServer>
  </location>
</configuration>
```

<span data-ttu-id="1cb6e-136">A propriedade <xref:System.Configuration.SectionInformation.InheritInChildApplications*> é definida como `false` para indicar que as configurações especificadas no elemento [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) não são herdadas por aplicativos que residem em um subdiretório do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-136">The <xref:System.Configuration.SectionInformation.InheritInChildApplications*> property is set to `false` to indicate that the settings specified within the [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) element aren't inherited by apps that reside in a subdirectory of the app.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath=".\MyApp.exe" 
                stdoutLogEnabled="false" 
                stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

::: moniker-end

<span data-ttu-id="1cb6e-137">Quando um aplicativo é implantado no [Serviço de Aplicativo do Azure](https://azure.microsoft.com/services/app-service/), o caminho `stdoutLogFile` é definido para `\\?\%home%\LogFiles\stdout`.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-137">When an app is deployed to [Azure App Service](https://azure.microsoft.com/services/app-service/), the `stdoutLogFile` path is set to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="1cb6e-138">O caminho salva logs de stdout para a pasta *LogFiles*, que é um local criado automaticamente pelo serviço.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-138">The path saves stdout logs to the *LogFiles* folder, which is a location automatically created by the service.</span></span>

<span data-ttu-id="1cb6e-139">Confira [Configuração de subaplicativos](xref:host-and-deploy/iis/index#sub-application-configuration) para uma nota importante relativa à configuração de arquivos *web.config* em subaplicativos.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-139">See [Sub-application configuration](xref:host-and-deploy/iis/index#sub-application-configuration) for an important note pertaining to the configuration of *web.config* files in sub-apps.</span></span>

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="1cb6e-140">Atributos do elemento aspNetCore</span><span class="sxs-lookup"><span data-stu-id="1cb6e-140">Attributes of the aspNetCore element</span></span>

::: moniker range=">= aspnetcore-2.2"

| <span data-ttu-id="1cb6e-141">Atributo</span><span class="sxs-lookup"><span data-stu-id="1cb6e-141">Attribute</span></span> | <span data-ttu-id="1cb6e-142">Descrição</span><span class="sxs-lookup"><span data-stu-id="1cb6e-142">Description</span></span> | <span data-ttu-id="1cb6e-143">Padrão</span><span class="sxs-lookup"><span data-stu-id="1cb6e-143">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="1cb6e-144">Atributo de cadeia de caracteres opcional.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-144">Optional string attribute.</span></span></p><p><span data-ttu-id="1cb6e-145">Argumentos para o executável especificado em **processPath**.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-145">Arguments to the executable specified in **processPath**.</span></span></p> | |
| `disableStartUpErrorPage` | <p><span data-ttu-id="1cb6e-146">Atributo booliano opcional.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-146">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="1cb6e-147">Se for true, a página **502.5 – Falha do Processo** será suprimida e a página de código de status 502, configurada no *web.config*, terá precedência.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-147">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="1cb6e-148">Atributo booliano opcional.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-148">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="1cb6e-149">Se for true, o token será encaminhado para o processo filho escutando em %ASPNETCORE_PORT% como um cabeçalho 'MS-ASPNETCORE-WINAUTHTOKEN' por solicitação.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-149">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="1cb6e-150">É responsabilidade desse processo chamar CloseHandle nesse token por solicitação.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-150">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `hostingModel` | <p><span data-ttu-id="1cb6e-151">Atributo de cadeia de caracteres opcional.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-151">Optional string attribute.</span></span></p><p><span data-ttu-id="1cb6e-152">Especifica o modelo de hospedagem como em processo (`inprocess`) ou de fora do processo (`outofprocess`).</span><span class="sxs-lookup"><span data-stu-id="1cb6e-152">Specifies the hosting model as in-process (`inprocess`) or out-of-process (`outofprocess`).</span></span></p> | `outofprocess` |
| `processesPerApplication` | <p><span data-ttu-id="1cb6e-153">Atributo inteiro opcional.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-153">Optional integer attribute.</span></span></p><p><span data-ttu-id="1cb6e-154">Especifica o número de instâncias do processo especificado na configuração **processPath** que pode ser ativada por aplicativo.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-154">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="1cb6e-155">&dagger;Para hospedagem em processo, o valor está limitado a `1`.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-155">&dagger;For in-process hosting, the value is limited to `1`.</span></span></p> | <span data-ttu-id="1cb6e-156">Padrão: `1`</span><span class="sxs-lookup"><span data-stu-id="1cb6e-156">Default: `1`</span></span><br><span data-ttu-id="1cb6e-157">Mín.: `1`</span><span class="sxs-lookup"><span data-stu-id="1cb6e-157">Min: `1`</span></span><br><span data-ttu-id="1cb6e-158">Máx.: `100`&dagger;</span><span class="sxs-lookup"><span data-stu-id="1cb6e-158">Max: `100`&dagger;</span></span> |
| `processPath` | <p><span data-ttu-id="1cb6e-159">Atributo de cadeia de caracteres obrigatório.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-159">Required string attribute.</span></span></p><p><span data-ttu-id="1cb6e-160">Caminho para o executável que inicia um processo que escuta solicitações HTTP.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-160">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="1cb6e-161">Caminhos relativos são compatíveis.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-161">Relative paths are supported.</span></span> <span data-ttu-id="1cb6e-162">Se o caminho começa com `.`, o caminho é considerado relativo à raiz do site.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-162">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="1cb6e-163">Atributo inteiro opcional.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-163">Optional integer attribute.</span></span></p><p><span data-ttu-id="1cb6e-164">Especifica o número de vezes que o processo especificado em **processPath** pode falhar por minuto.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-164">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="1cb6e-165">Se esse limite for excedido, o módulo interromperá a inicialização do processo pelo restante do minuto.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-165">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p><p><span data-ttu-id="1cb6e-166">Sem suporte com hospedagem padrão.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-166">Not supported with in-process hosting.</span></span></p> | <span data-ttu-id="1cb6e-167">Padrão: `10`</span><span class="sxs-lookup"><span data-stu-id="1cb6e-167">Default: `10`</span></span><br><span data-ttu-id="1cb6e-168">Mín.: `0`</span><span class="sxs-lookup"><span data-stu-id="1cb6e-168">Min: `0`</span></span><br><span data-ttu-id="1cb6e-169">Máx.: `100`</span><span class="sxs-lookup"><span data-stu-id="1cb6e-169">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="1cb6e-170">Atributo de intervalo de tempo opcional.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-170">Optional timespan attribute.</span></span></p><p><span data-ttu-id="1cb6e-171">Especifica a duração para a qual o Módulo do ASP.NET Core aguarda uma resposta do processo que escuta em %ASPNETCORE_PORT%.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-171">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="1cb6e-172">Em versões do Módulo do ASP.NET Core que acompanham a versão do ASP.NET Core 2.1 ou posterior, o `requestTimeout` é especificado em horas, minutos e segundos.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-172">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p><p><span data-ttu-id="1cb6e-173">Não se aplica à hospedagem em processo.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-173">Doesn't apply to in-process hosting.</span></span> <span data-ttu-id="1cb6e-174">Para a hospedagem em processo, o módulo aguarda o aplicativo processar a solicitação.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-174">For in-process hosting, the module waits for the app to process the request.</span></span></p> | <span data-ttu-id="1cb6e-175">Padrão: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="1cb6e-175">Default: `00:02:00`</span></span><br><span data-ttu-id="1cb6e-176">Mín.: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="1cb6e-176">Min: `00:00:00`</span></span><br><span data-ttu-id="1cb6e-177">Máx.: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="1cb6e-177">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="1cb6e-178">Atributo inteiro opcional.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-178">Optional integer attribute.</span></span></p><p><span data-ttu-id="1cb6e-179">Duração em segundos que o módulo espera para o executável desligar normalmente quando o arquivo *app_offline.htm* é detectado.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-179">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="1cb6e-180">Padrão: `10`</span><span class="sxs-lookup"><span data-stu-id="1cb6e-180">Default: `10`</span></span><br><span data-ttu-id="1cb6e-181">Mín.: `0`</span><span class="sxs-lookup"><span data-stu-id="1cb6e-181">Min: `0`</span></span><br><span data-ttu-id="1cb6e-182">Máx.: `600`</span><span class="sxs-lookup"><span data-stu-id="1cb6e-182">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="1cb6e-183">Atributo inteiro opcional.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-183">Optional integer attribute.</span></span></p><p><span data-ttu-id="1cb6e-184">Duração em segundos que o módulo espera para o arquivo executável iniciar um processo escutando na porta.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-184">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="1cb6e-185">Se esse tempo limite é excedido, o módulo encerra o processo.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-185">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="1cb6e-186">O módulo tentará reiniciar o processo quando ele receber uma nova solicitação e continuará a tentar reiniciar o processo em solicitações subsequentes de entrada, a menos que o aplicativo falhe em iniciar um número de vezes igual a **rapidFailsPerMinute** no último minuto sem interrupção.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-186">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="1cb6e-187">Um valor de 0 (zero) **não** é considerado um tempo limite infinito.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-187">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="1cb6e-188">Padrão: `120`</span><span class="sxs-lookup"><span data-stu-id="1cb6e-188">Default: `120`</span></span><br><span data-ttu-id="1cb6e-189">Mín.: `0`</span><span class="sxs-lookup"><span data-stu-id="1cb6e-189">Min: `0`</span></span><br><span data-ttu-id="1cb6e-190">Máx.: `3600`</span><span class="sxs-lookup"><span data-stu-id="1cb6e-190">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="1cb6e-191">Atributo booliano opcional.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-191">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="1cb6e-192">Se for true, **stdout** e **stderr** para o processo especificado em **processPath** serão redirecionados para o arquivo especificado em **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-192">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="1cb6e-193">Atributo de cadeia de caracteres opcional.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-193">Optional string attribute.</span></span></p><p><span data-ttu-id="1cb6e-194">Especifica o caminho relativo ou absoluto para o qual **stdout** e **stderr** do processo especificado em **processPath** são registrados em log.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-194">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="1cb6e-195">Os caminhos relativos são relativos à raiz do site.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-195">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="1cb6e-196">Qualquer caminho começando com `.` é relativo à raiz do site e todos os outros caminhos são tratados como caminhos absolutos.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-196">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="1cb6e-197">As pastas fornecidas no caminho devem existir para que o módulo crie o arquivo de log.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-197">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="1cb6e-198">Usando delimitadores de sublinhado, um carimbo de data/hora, uma ID de processo e a extensão de arquivo (*.log*) são adicionados ao último segmento do caminho **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-198">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="1cb6e-199">Se `.\logs\stdout` é fornecido como um valor, um log de exemplo stdout é salvo como *stdout_20180205194132_1934.log* na pasta *logs* quando salvos em 5/2/2018, às 19:41:32, com uma ID de processo de 1934.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-199">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="= aspnetcore-2.1"

| <span data-ttu-id="1cb6e-200">Atributo</span><span class="sxs-lookup"><span data-stu-id="1cb6e-200">Attribute</span></span> | <span data-ttu-id="1cb6e-201">Descrição</span><span class="sxs-lookup"><span data-stu-id="1cb6e-201">Description</span></span> | <span data-ttu-id="1cb6e-202">Padrão</span><span class="sxs-lookup"><span data-stu-id="1cb6e-202">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="1cb6e-203">Atributo de cadeia de caracteres opcional.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-203">Optional string attribute.</span></span></p><p><span data-ttu-id="1cb6e-204">Argumentos para o executável especificado em **processPath**.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-204">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="1cb6e-205">Atributo booliano opcional.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-205">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="1cb6e-206">Se for true, a página **502.5 – Falha do Processo** será suprimida e a página de código de status 502, configurada no *web.config*, terá precedência.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-206">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="1cb6e-207">Atributo booliano opcional.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-207">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="1cb6e-208">Se for true, o token será encaminhado para o processo filho escutando em %ASPNETCORE_PORT% como um cabeçalho 'MS-ASPNETCORE-WINAUTHTOKEN' por solicitação.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-208">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="1cb6e-209">É responsabilidade desse processo chamar CloseHandle nesse token por solicitação.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-209">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="1cb6e-210">Atributo inteiro opcional.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-210">Optional integer attribute.</span></span></p><p><span data-ttu-id="1cb6e-211">Especifica o número de instâncias do processo especificado na configuração **processPath** que pode ser ativada por aplicativo.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-211">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p> | <span data-ttu-id="1cb6e-212">Padrão: `1`</span><span class="sxs-lookup"><span data-stu-id="1cb6e-212">Default: `1`</span></span><br><span data-ttu-id="1cb6e-213">Mín.: `1`</span><span class="sxs-lookup"><span data-stu-id="1cb6e-213">Min: `1`</span></span><br><span data-ttu-id="1cb6e-214">Máx.: `100`</span><span class="sxs-lookup"><span data-stu-id="1cb6e-214">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="1cb6e-215">Atributo de cadeia de caracteres obrigatório.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-215">Required string attribute.</span></span></p><p><span data-ttu-id="1cb6e-216">Caminho para o executável que inicia um processo que escuta solicitações HTTP.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-216">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="1cb6e-217">Caminhos relativos são compatíveis.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-217">Relative paths are supported.</span></span> <span data-ttu-id="1cb6e-218">Se o caminho começa com `.`, o caminho é considerado relativo à raiz do site.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-218">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="1cb6e-219">Atributo inteiro opcional.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-219">Optional integer attribute.</span></span></p><p><span data-ttu-id="1cb6e-220">Especifica o número de vezes que o processo especificado em **processPath** pode falhar por minuto.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-220">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="1cb6e-221">Se esse limite for excedido, o módulo interromperá a inicialização do processo pelo restante do minuto.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-221">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="1cb6e-222">Padrão: `10`</span><span class="sxs-lookup"><span data-stu-id="1cb6e-222">Default: `10`</span></span><br><span data-ttu-id="1cb6e-223">Mín.: `0`</span><span class="sxs-lookup"><span data-stu-id="1cb6e-223">Min: `0`</span></span><br><span data-ttu-id="1cb6e-224">Máx.: `100`</span><span class="sxs-lookup"><span data-stu-id="1cb6e-224">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="1cb6e-225">Atributo de intervalo de tempo opcional.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-225">Optional timespan attribute.</span></span></p><p><span data-ttu-id="1cb6e-226">Especifica a duração para a qual o Módulo do ASP.NET Core aguarda uma resposta do processo que escuta em %ASPNETCORE_PORT%.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-226">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="1cb6e-227">Em versões do Módulo do ASP.NET Core que acompanham a versão do ASP.NET Core 2.1 ou posterior, o `requestTimeout` é especificado em horas, minutos e segundos.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-227">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p> | <span data-ttu-id="1cb6e-228">Padrão: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="1cb6e-228">Default: `00:02:00`</span></span><br><span data-ttu-id="1cb6e-229">Mín.: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="1cb6e-229">Min: `00:00:00`</span></span><br><span data-ttu-id="1cb6e-230">Máx.: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="1cb6e-230">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="1cb6e-231">Atributo inteiro opcional.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-231">Optional integer attribute.</span></span></p><p><span data-ttu-id="1cb6e-232">Duração em segundos que o módulo espera para o executável desligar normalmente quando o arquivo *app_offline.htm* é detectado.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-232">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="1cb6e-233">Padrão: `10`</span><span class="sxs-lookup"><span data-stu-id="1cb6e-233">Default: `10`</span></span><br><span data-ttu-id="1cb6e-234">Mín.: `0`</span><span class="sxs-lookup"><span data-stu-id="1cb6e-234">Min: `0`</span></span><br><span data-ttu-id="1cb6e-235">Máx.: `600`</span><span class="sxs-lookup"><span data-stu-id="1cb6e-235">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="1cb6e-236">Atributo inteiro opcional.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-236">Optional integer attribute.</span></span></p><p><span data-ttu-id="1cb6e-237">Duração em segundos que o módulo espera para o arquivo executável iniciar um processo escutando na porta.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-237">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="1cb6e-238">Se esse tempo limite é excedido, o módulo encerra o processo.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-238">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="1cb6e-239">O módulo tentará reiniciar o processo quando ele receber uma nova solicitação e continuará a tentar reiniciar o processo em solicitações subsequentes de entrada, a menos que o aplicativo falhe em iniciar um número de vezes igual a **rapidFailsPerMinute** no último minuto sem interrupção.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-239">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="1cb6e-240">Um valor de 0 (zero) **não** é considerado um tempo limite infinito.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-240">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="1cb6e-241">Padrão: `120`</span><span class="sxs-lookup"><span data-stu-id="1cb6e-241">Default: `120`</span></span><br><span data-ttu-id="1cb6e-242">Mín.: `0`</span><span class="sxs-lookup"><span data-stu-id="1cb6e-242">Min: `0`</span></span><br><span data-ttu-id="1cb6e-243">Máx.: `3600`</span><span class="sxs-lookup"><span data-stu-id="1cb6e-243">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="1cb6e-244">Atributo booliano opcional.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-244">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="1cb6e-245">Se for true, **stdout** e **stderr** para o processo especificado em **processPath** serão redirecionados para o arquivo especificado em **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-245">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="1cb6e-246">Atributo de cadeia de caracteres opcional.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-246">Optional string attribute.</span></span></p><p><span data-ttu-id="1cb6e-247">Especifica o caminho relativo ou absoluto para o qual **stdout** e **stderr** do processo especificado em **processPath** são registrados em log.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-247">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="1cb6e-248">Os caminhos relativos são relativos à raiz do site.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-248">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="1cb6e-249">Qualquer caminho começando com `.` é relativo à raiz do site e todos os outros caminhos são tratados como caminhos absolutos.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-249">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="1cb6e-250">As pastas fornecidas no caminho devem existir para que o módulo crie o arquivo de log.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-250">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="1cb6e-251">Usando delimitadores de sublinhado, um carimbo de data/hora, uma ID de processo e a extensão de arquivo (*.log*) são adicionados ao último segmento do caminho **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-251">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="1cb6e-252">Se `.\logs\stdout` é fornecido como um valor, um log de exemplo stdout é salvo como *stdout_20180205194132_1934.log* na pasta *logs* quando salvos em 5/2/2018, às 19:41:32, com uma ID de processo de 1934.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-252">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

| <span data-ttu-id="1cb6e-253">Atributo</span><span class="sxs-lookup"><span data-stu-id="1cb6e-253">Attribute</span></span> | <span data-ttu-id="1cb6e-254">Descrição</span><span class="sxs-lookup"><span data-stu-id="1cb6e-254">Description</span></span> | <span data-ttu-id="1cb6e-255">Padrão</span><span class="sxs-lookup"><span data-stu-id="1cb6e-255">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="1cb6e-256">Atributo de cadeia de caracteres opcional.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-256">Optional string attribute.</span></span></p><p><span data-ttu-id="1cb6e-257">Argumentos para o executável especificado em **processPath**.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-257">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="1cb6e-258">Atributo booliano opcional.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-258">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="1cb6e-259">Se for true, a página **502.5 – Falha do Processo** será suprimida e a página de código de status 502, configurada no *web.config*, terá precedência.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-259">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="1cb6e-260">Atributo booliano opcional.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-260">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="1cb6e-261">Se for true, o token será encaminhado para o processo filho escutando em %ASPNETCORE_PORT% como um cabeçalho 'MS-ASPNETCORE-WINAUTHTOKEN' por solicitação.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-261">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="1cb6e-262">É responsabilidade desse processo chamar CloseHandle nesse token por solicitação.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-262">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="1cb6e-263">Atributo inteiro opcional.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-263">Optional integer attribute.</span></span></p><p><span data-ttu-id="1cb6e-264">Especifica o número de instâncias do processo especificado na configuração **processPath** que pode ser ativada por aplicativo.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-264">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p> | <span data-ttu-id="1cb6e-265">Padrão: `1`</span><span class="sxs-lookup"><span data-stu-id="1cb6e-265">Default: `1`</span></span><br><span data-ttu-id="1cb6e-266">Mín.: `1`</span><span class="sxs-lookup"><span data-stu-id="1cb6e-266">Min: `1`</span></span><br><span data-ttu-id="1cb6e-267">Máx.: `100`</span><span class="sxs-lookup"><span data-stu-id="1cb6e-267">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="1cb6e-268">Atributo de cadeia de caracteres obrigatório.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-268">Required string attribute.</span></span></p><p><span data-ttu-id="1cb6e-269">Caminho para o executável que inicia um processo que escuta solicitações HTTP.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-269">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="1cb6e-270">Caminhos relativos são compatíveis.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-270">Relative paths are supported.</span></span> <span data-ttu-id="1cb6e-271">Se o caminho começa com `.`, o caminho é considerado relativo à raiz do site.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-271">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="1cb6e-272">Atributo inteiro opcional.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-272">Optional integer attribute.</span></span></p><p><span data-ttu-id="1cb6e-273">Especifica o número de vezes que o processo especificado em **processPath** pode falhar por minuto.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-273">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="1cb6e-274">Se esse limite for excedido, o módulo interromperá a inicialização do processo pelo restante do minuto.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-274">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="1cb6e-275">Padrão: `10`</span><span class="sxs-lookup"><span data-stu-id="1cb6e-275">Default: `10`</span></span><br><span data-ttu-id="1cb6e-276">Mín.: `0`</span><span class="sxs-lookup"><span data-stu-id="1cb6e-276">Min: `0`</span></span><br><span data-ttu-id="1cb6e-277">Máx.: `100`</span><span class="sxs-lookup"><span data-stu-id="1cb6e-277">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="1cb6e-278">Atributo de intervalo de tempo opcional.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-278">Optional timespan attribute.</span></span></p><p><span data-ttu-id="1cb6e-279">Especifica a duração para a qual o Módulo do ASP.NET Core aguarda uma resposta do processo que escuta em %ASPNETCORE_PORT%.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-279">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="1cb6e-280">Em versões do Módulo ASP.NET Core que acompanham a versão do ASP.NET Core 2.0 ou anterior, o `requestTimeout` deve ser especificado somente em minutos inteiros, caso contrário, ele assume o valor padrão de 2 minutos.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-280">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.0 or earlier, the `requestTimeout` must be specified in whole minutes only, otherwise it defaults to 2 minutes.</span></span></p> | <span data-ttu-id="1cb6e-281">Padrão: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="1cb6e-281">Default: `00:02:00`</span></span><br><span data-ttu-id="1cb6e-282">Mín.: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="1cb6e-282">Min: `00:00:00`</span></span><br><span data-ttu-id="1cb6e-283">Máx.: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="1cb6e-283">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="1cb6e-284">Atributo inteiro opcional.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-284">Optional integer attribute.</span></span></p><p><span data-ttu-id="1cb6e-285">Duração em segundos que o módulo espera para o executável desligar normalmente quando o arquivo *app_offline.htm* é detectado.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-285">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="1cb6e-286">Padrão: `10`</span><span class="sxs-lookup"><span data-stu-id="1cb6e-286">Default: `10`</span></span><br><span data-ttu-id="1cb6e-287">Mín.: `0`</span><span class="sxs-lookup"><span data-stu-id="1cb6e-287">Min: `0`</span></span><br><span data-ttu-id="1cb6e-288">Máx.: `600`</span><span class="sxs-lookup"><span data-stu-id="1cb6e-288">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="1cb6e-289">Atributo inteiro opcional.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-289">Optional integer attribute.</span></span></p><p><span data-ttu-id="1cb6e-290">Duração em segundos que o módulo espera para o arquivo executável iniciar um processo escutando na porta.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-290">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="1cb6e-291">Se esse tempo limite é excedido, o módulo encerra o processo.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-291">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="1cb6e-292">O módulo tentará reiniciar o processo quando ele receber uma nova solicitação e continuará a tentar reiniciar o processo em solicitações subsequentes de entrada, a menos que o aplicativo falhe em iniciar um número de vezes igual a **rapidFailsPerMinute** no último minuto sem interrupção.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-292">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p> | <span data-ttu-id="1cb6e-293">Padrão: `120`</span><span class="sxs-lookup"><span data-stu-id="1cb6e-293">Default: `120`</span></span><br><span data-ttu-id="1cb6e-294">Mín.: `0`</span><span class="sxs-lookup"><span data-stu-id="1cb6e-294">Min: `0`</span></span><br><span data-ttu-id="1cb6e-295">Máx.: `3600`</span><span class="sxs-lookup"><span data-stu-id="1cb6e-295">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="1cb6e-296">Atributo booliano opcional.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-296">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="1cb6e-297">Se for true, **stdout** e **stderr** para o processo especificado em **processPath** serão redirecionados para o arquivo especificado em **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-297">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="1cb6e-298">Atributo de cadeia de caracteres opcional.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-298">Optional string attribute.</span></span></p><p><span data-ttu-id="1cb6e-299">Especifica o caminho relativo ou absoluto para o qual **stdout** e **stderr** do processo especificado em **processPath** são registrados em log.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-299">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="1cb6e-300">Os caminhos relativos são relativos à raiz do site.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-300">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="1cb6e-301">Qualquer caminho começando com `.` é relativo à raiz do site e todos os outros caminhos são tratados como caminhos absolutos.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-301">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="1cb6e-302">As pastas fornecidas no caminho devem existir para que o módulo crie o arquivo de log.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-302">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="1cb6e-303">Usando delimitadores de sublinhado, um carimbo de data/hora, uma ID de processo e a extensão de arquivo (*.log*) são adicionados ao último segmento do caminho **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-303">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="1cb6e-304">Se `.\logs\stdout` é fornecido como um valor, um log de exemplo stdout é salvo como *stdout_20180205194132_1934.log* na pasta *logs* quando salvos em 5/2/2018, às 19:41:32, com uma ID de processo de 1934.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-304">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

### <a name="setting-environment-variables"></a><span data-ttu-id="1cb6e-305">Definindo variáveis de ambiente</span><span class="sxs-lookup"><span data-stu-id="1cb6e-305">Setting environment variables</span></span>

<span data-ttu-id="1cb6e-306">Variáveis de ambiente podem ser especificadas para o processo no atributo `processPath`.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-306">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="1cb6e-307">Especificar uma variável de ambiente com o elemento filho `environmentVariable` de um elemento de coleção `environmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-307">Specify an environment variable with the `environmentVariable` child element of an `environmentVariables` collection element.</span></span> <span data-ttu-id="1cb6e-308">Variáveis de ambiente definidas nesta seção têm precedência sobre variáveis de ambiente do sistema.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-308">Environment variables set in this section take precedence over system environment variables.</span></span>

<span data-ttu-id="1cb6e-309">O exemplo a seguir define duas variáveis de ambiente.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-309">The following example sets two environment variables.</span></span> <span data-ttu-id="1cb6e-310">`ASPNETCORE_ENVIRONMENT` configura o ambiente do aplicativo para `Development`.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-310">`ASPNETCORE_ENVIRONMENT` configures the app's environment to `Development`.</span></span> <span data-ttu-id="1cb6e-311">Um desenvolvedor pode definir esse valor temporariamente no arquivo *web.config* para forçar o carregamento da [Página de Exceções do Desenvolvedor](xref:fundamentals/error-handling) ao depurar uma exceção de aplicativo.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-311">A developer may temporarily set this value in the *web.config* file in order to force the [Developer Exception Page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="1cb6e-312">`CONFIG_DIR` é um exemplo de uma variável de ambiente definida pelo usuário, em que o desenvolvedor escreveu código que lê o valor de inicialização para formar um caminho no qual carregar o arquivo de configuração do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-312">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that reads the value on startup to form a path for loading the app's configuration file.</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile="\\?\%home%\LogFiles\stdout"
      hostingModel="inprocess">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
    <environmentVariable name="CONFIG_DIR" value="f:\application_config" />
  </environmentVariables>
</aspNetCore>
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile="\\?\%home%\LogFiles\stdout">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
    <environmentVariable name="CONFIG_DIR" value="f:\application_config" />
  </environmentVariables>
</aspNetCore>
```

::: moniker-end

> [!WARNING]
> <span data-ttu-id="1cb6e-313">Defina a variável de ambiente apenas `ASPNETCORE_ENVIRONMENT` para `Development` em servidores de preparo e de teste que não estão acessíveis a redes não confiáveis, tais como a Internet.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-313">Only set the `ASPNETCORE_ENVIRONMENT` environment variable to `Development` on staging and testing servers that aren't accessible to untrusted networks, such as the Internet.</span></span>

## <a name="appofflinehtm"></a><span data-ttu-id="1cb6e-314">app_offline.htm</span><span class="sxs-lookup"><span data-stu-id="1cb6e-314">app_offline.htm</span></span>

<span data-ttu-id="1cb6e-315">Se um arquivo com o nome *app_offline.htm* é detectado no diretório raiz de um aplicativo, o Módulo do ASP.NET Core tenta desligar normalmente o aplicativo e parar o processamento de solicitações de entrada.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-315">If a file with the name *app_offline.htm* is detected in the root directory of an app, the ASP.NET Core Module attempts to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="1cb6e-316">Se o aplicativo ainda está em execução após o número de segundos definido em `shutdownTimeLimit`, o Módulo do ASP.NET Core encerra o processo em execução.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-316">If the app is still running after the number of seconds defined in `shutdownTimeLimit`, the ASP.NET Core Module kills the running process.</span></span>

<span data-ttu-id="1cb6e-317">Enquanto o arquivo *app_offline.htm* estiver presente, o Módulo do ASP.NET Core responderá às solicitações enviando o conteúdo do arquivo *app_offline.htm*.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-317">While the *app_offline.htm* file is present, the ASP.NET Core Module responds to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="1cb6e-318">Quando o arquivo *app_offline.htm* é removido, a próxima solicitação inicia o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-318">When the *app_offline.htm* file is removed, the next request starts the app.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="1cb6e-319">Ao usar o modelo de hospedagem de fora do processo, talvez o aplicativo não desligue imediatamente se houver uma conexão aberta.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-319">When using the out-of-process hosting model, the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="1cb6e-320">Por exemplo, uma conexão websocket pode atrasar o desligamento do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-320">For example, a websocket connection may delay app shut down.</span></span>

::: moniker-end

## <a name="start-up-error-page"></a><span data-ttu-id="1cb6e-321">Página de erro de inicialização</span><span class="sxs-lookup"><span data-stu-id="1cb6e-321">Start-up error page</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="1cb6e-322">A hospedagem em processo e fora do processo produzem páginas de erro personalizadas quando falham ao iniciar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-322">Both in-process and out-of-process hosting produce custom error pages when they fail to start the app.</span></span>

<span data-ttu-id="1cb6e-323">Se o Módulo do ASP.NET Core falhar ao encontrar o manipulador de solicitação em processo ou fora do processo, uma página de código de status *500.0 – falha ao carregar manipulador no processo/fora do processo* será exibida.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-323">If the ASP.NET Core Module fails to find either the in-process or out-of-process request handler, a *500.0 - In-Process/Out-Of-Process Handler Load Failure* status code page appears.</span></span>

<span data-ttu-id="1cb6e-324">No caso da hospedagem em processo, se o módulo do ASP.NET Core falhar ao iniciar o aplicativo, uma página de código de status *500.30 – falha ao iniciar* será exibida.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-324">For in-process hosting if the ASP.NET Core Module fails to start the app, a *500.30 - Start Failure* status code page appears.</span></span>

<span data-ttu-id="1cb6e-325">Para hospedagem fora do processo, se o Módulo do ASP.NET Core falhar ao iniciar o processo de back-end ou se o processo de back-end iniciar, mas falhar ao escutar na porta configurada, uma página de código de status *502.5 – falha no processo* será exibida.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-325">For out-of-process hosting if the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span>

<span data-ttu-id="1cb6e-326">Para suprimir essa página e reverter para a página de código de status 5xx padrão do IIS, use o atributo `disableStartUpErrorPage`.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-326">To suppress this page and revert to the default IIS 5xx status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="1cb6e-327">Para obter mais informações sobre como configurar mensagens de erro personalizadas, veja [Erros HTTP \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="1cb6e-327">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="1cb6e-328">Se o Módulo do ASP.NET Core falhar ao iniciar o processo de back-end ou se o processo de back-end iniciar, mas falhar ao escutar na porta configurada, uma página de código de status *502.5 – falha no processo* será exibida.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-328">If the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span> <span data-ttu-id="1cb6e-329">Para omitir esta página e reverter para a página de código de status 502 padrão do IIS, use o atributo `disableStartUpErrorPage`.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-329">To suppress this page and revert to the default IIS 502 status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="1cb6e-330">Para obter mais informações sobre como configurar mensagens de erro personalizadas, veja [Erros HTTP \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="1cb6e-330">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

![Página de código de status 502.5 – Falha do Processo](aspnet-core-module/_static/ANCM-502_5.png)

::: moniker-end

## <a name="log-creation-and-redirection"></a><span data-ttu-id="1cb6e-332">Criação de log e redirecionamento</span><span class="sxs-lookup"><span data-stu-id="1cb6e-332">Log creation and redirection</span></span>

<span data-ttu-id="1cb6e-333">O Módulo do ASP.NET Core redireciona as saídas de console stdout e stderr para o disco se os atributos `stdoutLogEnabled` e `stdoutLogFile` do elemento `aspNetCore` forem definidos.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-333">The ASP.NET Core Module redirects stdout and stderr console output to disk if the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element are set.</span></span> <span data-ttu-id="1cb6e-334">As pastas no caminho `stdoutLogFile` devem existir para que o módulo crie o arquivo de log.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-334">Any folders in the `stdoutLogFile` path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="1cb6e-335">O pool de aplicativos deve ter acesso de gravação ao local em que os logs foram gravados (use `IIS AppPool\<app_pool_name>` para fornecer permissão de gravação).</span><span class="sxs-lookup"><span data-stu-id="1cb6e-335">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="1cb6e-336">Logs não sofrem rotação, a menos que ocorra a reciclagem/reinicialização do processo.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-336">Logs aren't rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="1cb6e-337">É responsabilidade do hoster limitar o espaço em disco consumido pelos logs.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-337">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span>

<span data-ttu-id="1cb6e-338">Usar o log de stdout é recomendado apenas para solucionar problemas de inicialização do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-338">Using the stdout log is only recommended for troubleshooting app startup issues.</span></span> <span data-ttu-id="1cb6e-339">Não use o log de stdout para fins gerais de registro em log do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-339">Don't use the stdout log for general app logging purposes.</span></span> <span data-ttu-id="1cb6e-340">Para registro em log de rotina em um aplicativo ASP.NET Core, use uma biblioteca de registro em log que limita o tamanho do arquivo de log e realiza a rotação de logs.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-340">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="1cb6e-341">Para obter mais informações, veja [provedores de log de terceiros](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="1cb6e-341">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

<span data-ttu-id="1cb6e-342">Uma extensão de arquivo e um carimbo de data/hora são adicionados automaticamente quando o arquivo de log é criado.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-342">A timestamp and file extension are added automatically when the log file is created.</span></span> <span data-ttu-id="1cb6e-343">O nome do arquivo de log é composto por meio do acréscimo do carimbo de data/hora, da ID do processo e da extensão de arquivo (*.log*) para o último segmento do caminho `stdoutLogFile` (normalmente *stdout*), delimitados por sublinhados.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-343">The log file name is composed by appending the timestamp, process ID, and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="1cb6e-344">Se o caminho `stdoutLogFile` termina com *stdout*, um log para um aplicativo com um PID de 1934, criado em 5/2/2018 às 19:42:32, tem o nome de arquivo *stdout_20180205194132_1934.log*.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-344">If the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 1934 created on 2/5/2018 at 19:42:32 has the file name *stdout_20180205194132_1934.log*.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="1cb6e-345">Se `stdoutLogEnabled` for falso, os erros que ocorrerem na inicialização do aplicativo serão capturados e emitidos no log de eventos até 30 KB.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-345">If `stdoutLogEnabled` is false, errors that occur on app startup are captured and emitted to the event log up to 30 KB.</span></span> <span data-ttu-id="1cb6e-346">Após a inicialização, todos os logs adicionais são descartados.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-346">After startup, all additional logs are discarded.</span></span>

::: moniker-end

<span data-ttu-id="1cb6e-347">O elemento `aspNetCore` de exemplo a seguir configura o registro em log de stdout para um aplicativo hospedado no Serviço de Aplicativo do Azure.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-347">The following sample `aspNetCore` element configures stdout logging for an app hosted in Azure App Service.</span></span> <span data-ttu-id="1cb6e-348">Um caminho local ou um caminho de compartilhamento de rede é aceitável para o registro em log local.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-348">A local path or network share path is acceptable for local logging.</span></span> <span data-ttu-id="1cb6e-349">Confirme se a identidade do usuário AppPool tem permissão para gravar no caminho fornecido.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-349">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="inprocess">
</aspNetCore>
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout">
</aspNetCore>
```

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

## <a name="enhanced-diagnostic-logs"></a><span data-ttu-id="1cb6e-350">Logs de diagnóstico avançados</span><span class="sxs-lookup"><span data-stu-id="1cb6e-350">Enhanced diagnostic logs</span></span>

<span data-ttu-id="1cb6e-351">O Módulo do ASP.NET Core pode ser configurado para fornecer logs de diagnóstico avançados.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-351">The ASP.NET Core Module provides is configurable to provide enhanced diagnostics logs.</span></span> <span data-ttu-id="1cb6e-352">Adicione o elemento `<handlerSettings>` ao elemento `<aspNetCore>` no *web.config*. A definição de `debugLevel` como `TRACE` expõe uma fidelidade maior de informações de diagnóstico:</span><span class="sxs-lookup"><span data-stu-id="1cb6e-352">Add the `<handlerSettings>` element to the `<aspNetCore>` element in *web.config*. Setting the `debugLevel` to `TRACE` exposes a higher fidelity of diagnostic information:</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="false"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="inprocess">
  <handlerSettings>
    <handlerSetting name="debugFile" value="aspnetcore-debug.log" />
    <handlerSetting name="debugLevel" value="FILE,TRACE" />
  </handlerSettings>
</aspNetCore>
```

<span data-ttu-id="1cb6e-353">Os valores do nível de depuração (`debugLevel`) podem incluir o nível e a localização.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-353">Debug level (`debugLevel`) values can include both the level and the location.</span></span>

<span data-ttu-id="1cb6e-354">Níveis (na ordem do menos para o mais detalhado):</span><span class="sxs-lookup"><span data-stu-id="1cb6e-354">Levels (in order from least to most verbose):</span></span>

* <span data-ttu-id="1cb6e-355">ERROR</span><span class="sxs-lookup"><span data-stu-id="1cb6e-355">ERROR</span></span>
* <span data-ttu-id="1cb6e-356">WARNING</span><span class="sxs-lookup"><span data-stu-id="1cb6e-356">WARNING</span></span>
* <span data-ttu-id="1cb6e-357">INFO</span><span class="sxs-lookup"><span data-stu-id="1cb6e-357">INFO</span></span>
* <span data-ttu-id="1cb6e-358">TRACE</span><span class="sxs-lookup"><span data-stu-id="1cb6e-358">TRACE</span></span>

<span data-ttu-id="1cb6e-359">Locais (vários locais são permitidos):</span><span class="sxs-lookup"><span data-stu-id="1cb6e-359">Locations (multiple locations are permitted):</span></span>

* <span data-ttu-id="1cb6e-360">CONSOLE</span><span class="sxs-lookup"><span data-stu-id="1cb6e-360">CONSOLE</span></span>
* <span data-ttu-id="1cb6e-361">EVENTLOG</span><span class="sxs-lookup"><span data-stu-id="1cb6e-361">EVENTLOG</span></span>
* <span data-ttu-id="1cb6e-362">FILE</span><span class="sxs-lookup"><span data-stu-id="1cb6e-362">FILE</span></span>

<span data-ttu-id="1cb6e-363">As configurações do manipulador também podem ser fornecidas por meio de variáveis de ambiente:</span><span class="sxs-lookup"><span data-stu-id="1cb6e-363">The handler settings can also be provided via environment variables:</span></span>

* <span data-ttu-id="1cb6e-364">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; caminho para o arquivo de log de depuração.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-364">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Path to the debug log file.</span></span> <span data-ttu-id="1cb6e-365">(Padrão: *aspnetcore-debug.log*)</span><span class="sxs-lookup"><span data-stu-id="1cb6e-365">(Default: *aspnetcore-debug.log*)</span></span>
* <span data-ttu-id="1cb6e-366">`ASPNETCORE_MODULE_DEBUG` &ndash; configuração do nível de depuração.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-366">`ASPNETCORE_MODULE_DEBUG` &ndash; Debug level setting.</span></span>

> [!WARNING]
> <span data-ttu-id="1cb6e-367">**Não** deixe o log de depuração habilitado na implantação por mais tempo que o necessário para solucionar um problema.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-367">Do **not** leave debug logging enabled in the deployment for longer than required to troubleshoot an issue.</span></span> <span data-ttu-id="1cb6e-368">O tamanho do log não é limitado.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-368">The size of the log isn't limited.</span></span> <span data-ttu-id="1cb6e-369">Deixar o log de depuração habilitado pode esgotar o espaço em disco disponível e causar falha no servidor ou no serviço de aplicativo.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-369">Leaving the debug log enabled can exhaust the available disk space and crash the server or app service.</span></span>

::: moniker-end

<span data-ttu-id="1cb6e-370">Veja [Configuração com web.config](#configuration-with-webconfig) para obter um exemplo do elemento `aspNetCore` no arquivo *web.config*.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-370">See [Configuration with web.config](#configuration-with-webconfig) for an example of the `aspNetCore` element in the *web.config* file.</span></span>

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a><span data-ttu-id="1cb6e-371">A configuração de proxy usa o protocolo HTTP e um token de emparelhamento</span><span class="sxs-lookup"><span data-stu-id="1cb6e-371">Proxy configuration uses HTTP protocol and a pairing token</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="1cb6e-372">*Só se aplica à hospedagem de fora do processo.*</span><span class="sxs-lookup"><span data-stu-id="1cb6e-372">*Only applies to out-of-process hosting.*</span></span>

::: moniker-end

<span data-ttu-id="1cb6e-373">O proxy criado entre o Módulo do ASP.NET Core e o Kestrel usa o protocolo HTTP.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-373">The proxy created between the ASP.NET Core Module and Kestrel uses the HTTP protocol.</span></span> <span data-ttu-id="1cb6e-374">O uso de HTTP é uma otimização de desempenho na qual o tráfego entre o módulo e o Kestrel ocorre em um endereço de loopback fora do adaptador de rede.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-374">Using HTTP is a performance optimization, where the traffic between the module and Kestrel takes place on a loopback address off of the network interface.</span></span> <span data-ttu-id="1cb6e-375">Não há nenhum risco de interceptação do tráfego entre o módulo e o Kestrel em um local fora do servidor.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-375">There's no risk of eavesdropping the traffic between the module and Kestrel from a location off of the server.</span></span>

<span data-ttu-id="1cb6e-376">Um token de emparelhamento é usado para assegurar que as solicitações recebidas pelo Kestrel foram transmitidas por proxy pelo IIS e que não são provenientes de outra origem.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-376">A pairing token is used to guarantee that the requests received by Kestrel were proxied by IIS and didn't come from some other source.</span></span> <span data-ttu-id="1cb6e-377">O token de emparelhamento é criado e definido em uma variável de ambiente (`ASPNETCORE_TOKEN`) pelo módulo.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-377">The pairing token is created and set into an environment variable (`ASPNETCORE_TOKEN`) by the module.</span></span> <span data-ttu-id="1cb6e-378">O token de emparelhamento também é definido em um cabeçalho (`MSAspNetCoreToken`) em cada solicitação com proxy.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-378">The pairing token is also set into a header (`MSAspNetCoreToken`) on every proxied request.</span></span> <span data-ttu-id="1cb6e-379">O Middleware do IIS verifica cada solicitação recebida para confirmar se o valor de cabeçalho do token de emparelhamento corresponde ao valor da variável de ambiente.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-379">IIS Middleware checks each request it receives to confirm that the pairing token header value matches the environment variable value.</span></span> <span data-ttu-id="1cb6e-380">Se os valores do token forem incompatíveis, a solicitação será registrada em log e rejeitada.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-380">If the token values are mismatched, the request is logged and rejected.</span></span> <span data-ttu-id="1cb6e-381">A variável de ambiente do token de emparelhamento e o tráfego entre o módulo e o Kestrel não são acessíveis em um local fora do servidor.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-381">The pairing token environment variable and the traffic between the module and Kestrel aren't accessible from a location off of the server.</span></span> <span data-ttu-id="1cb6e-382">Sem saber o valor do token de emparelhamento, um invasor não pode enviar solicitações que ignoram a verificação no Middleware do IIS.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-382">Without knowing the pairing token value, an attacker can't submit requests that bypass the check in the IIS Middleware.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="1cb6e-383">Módulo do ASP.NET Core com uma configuração do IIS compartilhada</span><span class="sxs-lookup"><span data-stu-id="1cb6e-383">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="1cb6e-384">O instalador do módulo do ASP.NET Core é executado com os privilégios da conta de **SISTEMA**.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-384">The ASP.NET Core Module installer runs with the privileges of the **SYSTEM** account.</span></span> <span data-ttu-id="1cb6e-385">Já que a conta de sistema local não tem permissão para modificar o caminho do compartilhamento usado pela configuração compartilhada de IIS, o instalador experimenta um erro de acesso negado ao tentar definir as configurações de módulo em *applicationHost.config* no compartilhamento.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-385">Because the local system account doesn't have modify permission for the share path used by the IIS Shared Configuration, the installer hits an access denied error when attempting to configure the module settings in *applicationHost.config* on the share.</span></span> <span data-ttu-id="1cb6e-386">Ao usar uma configuração compartilhada de IIS, siga estas etapas:</span><span class="sxs-lookup"><span data-stu-id="1cb6e-386">When using an IIS Shared Configuration, follow these steps:</span></span>

1. <span data-ttu-id="1cb6e-387">Desabilite a configuração compartilhada de IIS.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-387">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="1cb6e-388">Execute o instalador.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-388">Run the installer.</span></span>
1. <span data-ttu-id="1cb6e-389">Exportar o arquivo *applicationHost.config* atualizado para o compartilhamento.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-389">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="1cb6e-390">Reabilite a Configuração Compartilhada do IIS.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-390">Re-enable the IIS Shared Configuration.</span></span>

## <a name="module-version-and-hosting-bundle-installer-logs"></a><span data-ttu-id="1cb6e-391">Versão do módulo e logs do instalador do pacote de hospedagem</span><span class="sxs-lookup"><span data-stu-id="1cb6e-391">Module version and Hosting Bundle installer logs</span></span>

<span data-ttu-id="1cb6e-392">Para determinar a versão do Módulo do ASP.NET Core instalado:</span><span class="sxs-lookup"><span data-stu-id="1cb6e-392">To determine the version of the installed ASP.NET Core Module:</span></span>

1. <span data-ttu-id="1cb6e-393">No sistema de hospedagem, navegue até *%windir%\System32\inetsrv*.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-393">On the hosting system, navigate to *%windir%\System32\inetsrv*.</span></span>
1. <span data-ttu-id="1cb6e-394">Localize o arquivo *aspnetcore.dll*.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-394">Locate the *aspnetcore.dll* file.</span></span>
1. <span data-ttu-id="1cb6e-395">Clique com o botão direito do mouse no arquivo e selecione **Propriedades** no menu contextual.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-395">Right-click the file and select **Properties** from the contextual menu.</span></span>
1. <span data-ttu-id="1cb6e-396">Selecione a guia **Detalhes**. A **Versão do arquivo** e a **Versão do produto** representam a versão instalada do módulo.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-396">Select the **Details** tab. The **File version** and **Product version** represent the installed version of the module.</span></span>

<span data-ttu-id="1cb6e-397">Os logs de instalador do pacote de hospedagem para o módulo são encontrados em *C:\\Usuários\\%UserName%\\AppData\\Local\\Temp*. O arquivo é nomeado *dd_DotNetCoreWinSvrHosting__\<carimbo de data/hora>_000_AspNetCoreModule_x64.log*.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-397">The Hosting Bundle installer logs for the module are found at *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. The file is named *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="1cb6e-398">Locais dos arquivos de módulo, de esquema e de configuração</span><span class="sxs-lookup"><span data-stu-id="1cb6e-398">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="1cb6e-399">Módulo</span><span class="sxs-lookup"><span data-stu-id="1cb6e-399">Module</span></span>

<span data-ttu-id="1cb6e-400">**IIS (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="1cb6e-400">**IIS (x86/amd64):**</span></span>

   * <span data-ttu-id="1cb6e-401">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="1cb6e-401">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

   * <span data-ttu-id="1cb6e-402">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="1cb6e-402">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="1cb6e-403">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="1cb6e-403">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

   * <span data-ttu-id="1cb6e-404">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="1cb6e-404">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

::: moniker-end

<span data-ttu-id="1cb6e-405">**IIS Express (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="1cb6e-405">**IIS Express (x86/amd64):**</span></span>

   * <span data-ttu-id="1cb6e-406">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="1cb6e-406">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

   * <span data-ttu-id="1cb6e-407">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="1cb6e-407">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="1cb6e-408">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="1cb6e-408">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

   * <span data-ttu-id="1cb6e-409">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="1cb6e-409">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

::: moniker-end

### <a name="schema"></a><span data-ttu-id="1cb6e-410">Esquema</span><span class="sxs-lookup"><span data-stu-id="1cb6e-410">Schema</span></span>

<span data-ttu-id="1cb6e-411">**IIS**</span><span class="sxs-lookup"><span data-stu-id="1cb6e-411">**IIS**</span></span>

   * <span data-ttu-id="1cb6e-412">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="1cb6e-412">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="1cb6e-413">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="1cb6e-413">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span></span>

::: moniker-end
<span data-ttu-id="1cb6e-414">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="1cb6e-414">**IIS Express**</span></span>

   * <span data-ttu-id="1cb6e-415">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="1cb6e-415">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="1cb6e-416">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="1cb6e-416">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span></span>

::: moniker-end

### <a name="configuration"></a><span data-ttu-id="1cb6e-417">Configuração</span><span class="sxs-lookup"><span data-stu-id="1cb6e-417">Configuration</span></span>

<span data-ttu-id="1cb6e-418">**IIS**</span><span class="sxs-lookup"><span data-stu-id="1cb6e-418">**IIS**</span></span>

   * <span data-ttu-id="1cb6e-419">%windir%\System32\inetsrv\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="1cb6e-419">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="1cb6e-420">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="1cb6e-420">**IIS Express**</span></span>

   * <span data-ttu-id="1cb6e-421">%ProgramFiles%\IIS Express\config\templates\PersonalWebServer\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="1cb6e-421">%ProgramFiles%\IIS Express\config\templates\PersonalWebServer\applicationHost.config</span></span>

<span data-ttu-id="1cb6e-422">Os arquivos podem ser encontrados pesquisando por *aspnetcore* no arquivo *applicationHost.config*.</span><span class="sxs-lookup"><span data-stu-id="1cb6e-422">The files can be found by searching for *aspnetcore* in the *applicationHost.config* file.</span></span>

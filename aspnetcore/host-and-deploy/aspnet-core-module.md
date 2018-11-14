---
title: Referência de configuração do Módulo do ASP.NET Core
author: guardrex
description: Saiba como configurar o módulo do ASP.NET Core para hospedar aplicativos do ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 09/21/2018
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: ca86b1548c7c28a64fd391617b2e8290c1c264cf
ms.sourcegitcommit: 09affee3d234cb27ea6fe33bc113b79e68900d22
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/06/2018
ms.locfileid: "51191354"
---
# <a name="aspnet-core-module-configuration-reference"></a><span data-ttu-id="1387f-103">Referência de configuração do Módulo do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1387f-103">ASP.NET Core Module configuration reference</span></span>

<span data-ttu-id="1387f-104">Por [Luke Latham](https://github.com/guardrex), [Rick Anderson](https://twitter.com/RickAndMSFT), [Sourabh Shirhatti](https://twitter.com/sshirhatti) e [Justin Kotalik](https://github.com/jkotalik)</span><span class="sxs-lookup"><span data-stu-id="1387f-104">By [Luke Latham](https://github.com/guardrex), [Rick Anderson](https://twitter.com/RickAndMSFT), [Sourabh Shirhatti](https://twitter.com/sshirhatti), and [Justin Kotalik](https://github.com/jkotalik)</span></span>

<span data-ttu-id="1387f-105">Este documento fornece instruções sobre como configurar o Módulo do ASP.NET Core para hospedar aplicativos do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1387f-105">This document provides instructions on how to configure the ASP.NET Core Module for hosting ASP.NET Core apps.</span></span> <span data-ttu-id="1387f-106">Para obter uma introdução ao Módulo do ASP.NET Core e instruções de instalação, veja a [visão geral do Módulo do ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="1387f-106">For an introduction to the ASP.NET Core Module and installation instructions, see the [ASP.NET Core Module overview](xref:fundamentals/servers/aspnet-core-module).</span></span>

::: moniker range=">= aspnetcore-2.2"

## <a name="hosting-model"></a><span data-ttu-id="1387f-107">Modelo de hospedagem</span><span class="sxs-lookup"><span data-stu-id="1387f-107">Hosting model</span></span>

<span data-ttu-id="1387f-108">Para aplicativos em execução no .NET Core 2.2 ou posterior, o módulo dá suporte a um modelo de hospedagem em processo para melhorar o desempenho em comparação com hospedagem do proxy reverso (fora de processo).</span><span class="sxs-lookup"><span data-stu-id="1387f-108">For apps running on .NET Core 2.2 or later, the module supports an in-process hosting model for improved performance compared to reverse-proxy (out-of-process) hosting.</span></span> <span data-ttu-id="1387f-109">Para obter mais informações, consulte <xref:fundamentals/servers/aspnet-core-module#aspnet-core-module-description>.</span><span class="sxs-lookup"><span data-stu-id="1387f-109">For more information, see <xref:fundamentals/servers/aspnet-core-module#aspnet-core-module-description>.</span></span>

<span data-ttu-id="1387f-110">A hospedagem em processo é uma aceitação para os aplicativos existentes, mas o padrão dos modelos [dotnet new](/dotnet/core/tools/dotnet-new) é o modelo de hospedagem em processo para todos os cenários do IIS e IIS Express.</span><span class="sxs-lookup"><span data-stu-id="1387f-110">In-procsess hosting is opt-in for existing apps, but [dotnet new](/dotnet/core/tools/dotnet-new) templates default to the in-process hosting model for all IIS and IIS Express scenarios.</span></span>

<span data-ttu-id="1387f-111">Para configurar um aplicativo para hospedagem em processo, adicione a propriedade `<AspNetCoreHostingModel>` ao arquivo de projeto do aplicativo (por exemplo, *MyApp.csproj*) com um valor de `inprocess` (a hospedagem de fora do processo é definida com `outofprocess`):</span><span class="sxs-lookup"><span data-stu-id="1387f-111">To configure an app for in-process hosting, add the `<AspNetCoreHostingModel>` property to the app's project file (for example, *MyApp.csproj*) with a value of `inprocess` (out-of-process hosting is set with `outofprocess`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>inprocess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="1387f-112">As seguintes características se aplicam ao hospedar em processo:</span><span class="sxs-lookup"><span data-stu-id="1387f-112">The following characteristics apply when hosting in-process:</span></span>

* <span data-ttu-id="1387f-113">O [servidor Kestrel](xref:fundamentals/servers/kestrel) não é usado.</span><span class="sxs-lookup"><span data-stu-id="1387f-113">The [Kestrel server](xref:fundamentals/servers/kestrel) isn't used.</span></span> <span data-ttu-id="1387f-114">Uma implementação personalizada do <xref:Microsoft.AspNetCore.Hosting.Server.IServer>, `IISHttpServer` funciona como o servidor do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="1387f-114">A custom <xref:Microsoft.AspNetCore.Hosting.Server.IServer> implementation, `IISHttpServer` acts as the app's server.</span></span>

* <span data-ttu-id="1387f-115">O [requestTimeout atributo](#attributes-of-the-aspnetcore-element) não se aplica à hospedagem em processo.</span><span class="sxs-lookup"><span data-stu-id="1387f-115">The [requestTimeout attribute](#attributes-of-the-aspnetcore-element) doesn't apply to in-process hosting.</span></span>

* <span data-ttu-id="1387f-116">Não há suporte para o compartilhamento do pool de aplicativos entre aplicativos.</span><span class="sxs-lookup"><span data-stu-id="1387f-116">Sharing an app pool among apps isn't supported.</span></span> <span data-ttu-id="1387f-117">Use um pool de aplicativos por aplicativo.</span><span class="sxs-lookup"><span data-stu-id="1387f-117">Use one app pool per app.</span></span>

* <span data-ttu-id="1387f-118">Ao usar a [Implantação da Web](/iis/publish/using-web-deploy/introduction-to-web-deploy) ou inserir manualmente um [arquivo app_offline.htm na implantação](xref:host-and-deploy/iis/index#locked-deployment-files), o aplicativo talvez não seja desligado imediatamente se houver uma conexão aberta.</span><span class="sxs-lookup"><span data-stu-id="1387f-118">When using [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) or manually placing an [app_offline.htm file in the deployment](xref:host-and-deploy/iis/index#locked-deployment-files), the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="1387f-119">Por exemplo, uma conexão websocket pode atrasar o desligamento do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="1387f-119">For example, a websocket connection may delay app shut down.</span></span>

* <span data-ttu-id="1387f-120">A arquitetura (número de bit) do aplicativo e o tempo de execução instalado (x64 ou x86) devem corresponder à arquitetura do pool de aplicativos.</span><span class="sxs-lookup"><span data-stu-id="1387f-120">The architecture (bitness) of the app and installed runtime (x64 or x86) must match the architecture of the app pool.</span></span>

* <span data-ttu-id="1387f-121">Se a configuração manual do host do aplicativo com `WebHostBuilder` (não usando [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host)) e o aplicativo não forem executados diretamente no servidor Kestrel (auto-hospedado), chame `UseKestrel` antes de chamar `UseIISIntegration`.</span><span class="sxs-lookup"><span data-stu-id="1387f-121">If setting up the app's host manually with `WebHostBuilder` (not using [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host)) and the app is ever run directly on the Kestrel server (self-hosted), call `UseKestrel` before calling `UseIISIntegration`.</span></span> <span data-ttu-id="1387f-122">Se a ordem for invertida, a inicialização do host falhará.</span><span class="sxs-lookup"><span data-stu-id="1387f-122">If the order is reversed, the host fails to start.</span></span>

* <span data-ttu-id="1387f-123">As desconexões do cliente são detectadas.</span><span class="sxs-lookup"><span data-stu-id="1387f-123">Client disconnects are detected.</span></span> <span data-ttu-id="1387f-124">O token de cancelamento [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) é cancelado quando o cliente se desconecta.</span><span class="sxs-lookup"><span data-stu-id="1387f-124">The [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) cancellation token is cancelled when the client disconnects.</span></span>

* <span data-ttu-id="1387f-125">`Directory.GetCurrentDirectory()` retorna o diretório de trabalho do processo iniciado pelo IIS em vez do diretório do aplicativo (por exemplo, *C:\Windows\System32\inetsrv* para *w3wp.exe*).</span><span class="sxs-lookup"><span data-stu-id="1387f-125">`Directory.GetCurrentDirectory()` returns the worker directory of the process started by IIS rather than the application directory (for example, *C:\Windows\System32\inetsrv* for *w3wp.exe*).</span></span>

### <a name="hosting-model-changes"></a><span data-ttu-id="1387f-126">Alterações no modelo de hospedagem</span><span class="sxs-lookup"><span data-stu-id="1387f-126">Hosting model changes</span></span>

<span data-ttu-id="1387f-127">Se a configuração `hostingModel` for alterada no arquivo *web.config* (explicado na seção [Configuração a Web.config](#configuration-with-webconfig)), o módulo reciclará o processo de trabalho do IIS.</span><span class="sxs-lookup"><span data-stu-id="1387f-127">If the `hostingModel` setting is changed in the *web.config* file (explained in the [Configuration with web.config](#configuration-with-webconfig) section), the module recycles the worker process for IIS.</span></span>

<span data-ttu-id="1387f-128">Para o IIS Express, o módulo não recicla o processo de trabalho, mas em vez disso, dispara um desligamento normal do processo atual do IIS Express.</span><span class="sxs-lookup"><span data-stu-id="1387f-128">For IIS Express, the module doesn't recycle the worker process but instead triggers a graceful shutdown of the current IIS Express process.</span></span> <span data-ttu-id="1387f-129">A próxima solicitação para o aplicativo gera um novo processo do IIS Express.</span><span class="sxs-lookup"><span data-stu-id="1387f-129">The next request to the app spawns a new IIS Express process.</span></span>

### <a name="process-name"></a><span data-ttu-id="1387f-130">Nome do processo</span><span class="sxs-lookup"><span data-stu-id="1387f-130">Process name</span></span>

<span data-ttu-id="1387f-131">`Process.GetCurrentProcess().ProcessName` relata `w3wp`/`iisexpress` (em processo) ou `dotnet` (fora do processo).</span><span class="sxs-lookup"><span data-stu-id="1387f-131">`Process.GetCurrentProcess().ProcessName` reports `w3wp`/`iisexpress` (in-process) or `dotnet` (out-of-process).</span></span>

::: moniker-end

## <a name="configuration-with-webconfig"></a><span data-ttu-id="1387f-132">Configuração com web.config</span><span class="sxs-lookup"><span data-stu-id="1387f-132">Configuration with web.config</span></span>

<span data-ttu-id="1387f-133">O Módulo do ASP.NET Core está configurado com a seção `aspNetCore` do nó `system.webServer` no arquivo *web.config* do site.</span><span class="sxs-lookup"><span data-stu-id="1387f-133">The ASP.NET Core Module is configured with the `aspNetCore` section of the `system.webServer` node in the site's *web.config* file.</span></span>

<span data-ttu-id="1387f-134">O seguinte arquivo *web.config* é publicado para uma [implantação dependente de estrutura](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) e configura o Módulo do ASP.NET Core para manipular solicitações de site:</span><span class="sxs-lookup"><span data-stu-id="1387f-134">The following *web.config* file is published for a [framework-dependent deployment](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) and configures the ASP.NET Core Module to handle site requests:</span></span>

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

<span data-ttu-id="1387f-135">O seguinte *web.config* é publicado para uma [implantação autossuficiente](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span><span class="sxs-lookup"><span data-stu-id="1387f-135">The following *web.config* is published for a [self-contained deployment](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span></span>

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

<span data-ttu-id="1387f-136">Quando um aplicativo é implantado no [Serviço de Aplicativo do Azure](https://azure.microsoft.com/services/app-service/), o caminho `stdoutLogFile` é definido para `\\?\%home%\LogFiles\stdout`.</span><span class="sxs-lookup"><span data-stu-id="1387f-136">When an app is deployed to [Azure App Service](https://azure.microsoft.com/services/app-service/), the `stdoutLogFile` path is set to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="1387f-137">O caminho salva logs de stdout para a pasta *LogFiles*, que é um local criado automaticamente pelo serviço.</span><span class="sxs-lookup"><span data-stu-id="1387f-137">The path saves stdout logs to the *LogFiles* folder, which is a location automatically created by the service.</span></span>

<span data-ttu-id="1387f-138">Confira [Configuração de subaplicativos](xref:host-and-deploy/iis/index#sub-application-configuration) para uma nota importante relativa à configuração de arquivos *web.config* em subaplicativos.</span><span class="sxs-lookup"><span data-stu-id="1387f-138">See [Sub-application configuration](xref:host-and-deploy/iis/index#sub-application-configuration) for an important note pertaining to the configuration of *web.config* files in sub-apps.</span></span>

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="1387f-139">Atributos do elemento aspNetCore</span><span class="sxs-lookup"><span data-stu-id="1387f-139">Attributes of the aspNetCore element</span></span>

::: moniker range=">= aspnetcore-2.2"

| <span data-ttu-id="1387f-140">Atributo</span><span class="sxs-lookup"><span data-stu-id="1387f-140">Attribute</span></span> | <span data-ttu-id="1387f-141">Descrição</span><span class="sxs-lookup"><span data-stu-id="1387f-141">Description</span></span> | <span data-ttu-id="1387f-142">Padrão</span><span class="sxs-lookup"><span data-stu-id="1387f-142">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="1387f-143">Atributo de cadeia de caracteres opcional.</span><span class="sxs-lookup"><span data-stu-id="1387f-143">Optional string attribute.</span></span></p><p><span data-ttu-id="1387f-144">Argumentos para o executável especificado em **processPath**.</span><span class="sxs-lookup"><span data-stu-id="1387f-144">Arguments to the executable specified in **processPath**.</span></span></p> | |
| `disableStartUpErrorPage` | <p><span data-ttu-id="1387f-145">Atributo booliano opcional.</span><span class="sxs-lookup"><span data-stu-id="1387f-145">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="1387f-146">Se for true, a página **502.5 – Falha do Processo** será suprimida e a página de código de status 502, configurada no *web.config*, terá precedência.</span><span class="sxs-lookup"><span data-stu-id="1387f-146">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="1387f-147">Atributo booliano opcional.</span><span class="sxs-lookup"><span data-stu-id="1387f-147">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="1387f-148">Se for true, o token será encaminhado para o processo filho escutando em %ASPNETCORE_PORT% como um cabeçalho 'MS-ASPNETCORE-WINAUTHTOKEN' por solicitação.</span><span class="sxs-lookup"><span data-stu-id="1387f-148">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="1387f-149">É responsabilidade desse processo chamar CloseHandle nesse token por solicitação.</span><span class="sxs-lookup"><span data-stu-id="1387f-149">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `hostingModel` | <p><span data-ttu-id="1387f-150">Atributo de cadeia de caracteres opcional.</span><span class="sxs-lookup"><span data-stu-id="1387f-150">Optional string attribute.</span></span></p><p><span data-ttu-id="1387f-151">Especifica o modelo de hospedagem como em processo (`inprocess`) ou de fora do processo (`outofprocess`).</span><span class="sxs-lookup"><span data-stu-id="1387f-151">Specifies the hosting model as in-process (`inprocess`) or out-of-process (`outofprocess`).</span></span></p> | `outofprocess` |
| `processesPerApplication` | <p><span data-ttu-id="1387f-152">Atributo inteiro opcional.</span><span class="sxs-lookup"><span data-stu-id="1387f-152">Optional integer attribute.</span></span></p><p><span data-ttu-id="1387f-153">Especifica o número de instâncias do processo especificado na configuração **processPath** que pode ser ativada por aplicativo.</span><span class="sxs-lookup"><span data-stu-id="1387f-153">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="1387f-154">&dagger;Para hospedagem em processo, o valor está limitado a `1`.</span><span class="sxs-lookup"><span data-stu-id="1387f-154">&dagger;For in-process hosting, the value is limited to `1`.</span></span></p> | <span data-ttu-id="1387f-155">Padrão: `1`</span><span class="sxs-lookup"><span data-stu-id="1387f-155">Default: `1`</span></span><br><span data-ttu-id="1387f-156">Mín.: `1`</span><span class="sxs-lookup"><span data-stu-id="1387f-156">Min: `1`</span></span><br><span data-ttu-id="1387f-157">Máx.: `100`&dagger;</span><span class="sxs-lookup"><span data-stu-id="1387f-157">Max: `100`&dagger;</span></span> |
| `processPath` | <p><span data-ttu-id="1387f-158">Atributo de cadeia de caracteres obrigatório.</span><span class="sxs-lookup"><span data-stu-id="1387f-158">Required string attribute.</span></span></p><p><span data-ttu-id="1387f-159">Caminho para o executável que inicia um processo que escuta solicitações HTTP.</span><span class="sxs-lookup"><span data-stu-id="1387f-159">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="1387f-160">Caminhos relativos são compatíveis.</span><span class="sxs-lookup"><span data-stu-id="1387f-160">Relative paths are supported.</span></span> <span data-ttu-id="1387f-161">Se o caminho começa com `.`, o caminho é considerado relativo à raiz do site.</span><span class="sxs-lookup"><span data-stu-id="1387f-161">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="1387f-162">Atributo inteiro opcional.</span><span class="sxs-lookup"><span data-stu-id="1387f-162">Optional integer attribute.</span></span></p><p><span data-ttu-id="1387f-163">Especifica o número de vezes que o processo especificado em **processPath** pode falhar por minuto.</span><span class="sxs-lookup"><span data-stu-id="1387f-163">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="1387f-164">Se esse limite for excedido, o módulo interromperá a inicialização do processo pelo restante do minuto.</span><span class="sxs-lookup"><span data-stu-id="1387f-164">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p><p><span data-ttu-id="1387f-165">Sem suporte com hospedagem padrão.</span><span class="sxs-lookup"><span data-stu-id="1387f-165">Not supported with in-process hosting.</span></span></p> | <span data-ttu-id="1387f-166">Padrão: `10`</span><span class="sxs-lookup"><span data-stu-id="1387f-166">Default: `10`</span></span><br><span data-ttu-id="1387f-167">Mín.: `0`</span><span class="sxs-lookup"><span data-stu-id="1387f-167">Min: `0`</span></span><br><span data-ttu-id="1387f-168">Máx.: `100`</span><span class="sxs-lookup"><span data-stu-id="1387f-168">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="1387f-169">Atributo de intervalo de tempo opcional.</span><span class="sxs-lookup"><span data-stu-id="1387f-169">Optional timespan attribute.</span></span></p><p><span data-ttu-id="1387f-170">Especifica a duração para a qual o Módulo do ASP.NET Core aguarda uma resposta do processo que escuta em %ASPNETCORE_PORT%.</span><span class="sxs-lookup"><span data-stu-id="1387f-170">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="1387f-171">Em versões do Módulo do ASP.NET Core que acompanham a versão do ASP.NET Core 2.1 ou posterior, o `requestTimeout` é especificado em horas, minutos e segundos.</span><span class="sxs-lookup"><span data-stu-id="1387f-171">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p><p><span data-ttu-id="1387f-172">Não se aplica à hospedagem em processo.</span><span class="sxs-lookup"><span data-stu-id="1387f-172">Doesn't apply to in-process hosting.</span></span> <span data-ttu-id="1387f-173">Para a hospedagem em processo, o módulo aguarda o aplicativo processar a solicitação.</span><span class="sxs-lookup"><span data-stu-id="1387f-173">For in-process hosting, the module waits for the app to process the request.</span></span></p> | <span data-ttu-id="1387f-174">Padrão: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="1387f-174">Default: `00:02:00`</span></span><br><span data-ttu-id="1387f-175">Mín.: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="1387f-175">Min: `00:00:00`</span></span><br><span data-ttu-id="1387f-176">Máx.: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="1387f-176">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="1387f-177">Atributo inteiro opcional.</span><span class="sxs-lookup"><span data-stu-id="1387f-177">Optional integer attribute.</span></span></p><p><span data-ttu-id="1387f-178">Duração em segundos que o módulo espera para o executável desligar normalmente quando o arquivo *app_offline.htm* é detectado.</span><span class="sxs-lookup"><span data-stu-id="1387f-178">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="1387f-179">Padrão: `10`</span><span class="sxs-lookup"><span data-stu-id="1387f-179">Default: `10`</span></span><br><span data-ttu-id="1387f-180">Mín.: `0`</span><span class="sxs-lookup"><span data-stu-id="1387f-180">Min: `0`</span></span><br><span data-ttu-id="1387f-181">Máx.: `600`</span><span class="sxs-lookup"><span data-stu-id="1387f-181">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="1387f-182">Atributo inteiro opcional.</span><span class="sxs-lookup"><span data-stu-id="1387f-182">Optional integer attribute.</span></span></p><p><span data-ttu-id="1387f-183">Duração em segundos que o módulo espera para o arquivo executável iniciar um processo escutando na porta.</span><span class="sxs-lookup"><span data-stu-id="1387f-183">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="1387f-184">Se esse tempo limite é excedido, o módulo encerra o processo.</span><span class="sxs-lookup"><span data-stu-id="1387f-184">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="1387f-185">O módulo tentará reiniciar o processo quando ele receber uma nova solicitação e continuará a tentar reiniciar o processo em solicitações subsequentes de entrada, a menos que o aplicativo falhe em iniciar um número de vezes igual a **rapidFailsPerMinute** no último minuto sem interrupção.</span><span class="sxs-lookup"><span data-stu-id="1387f-185">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="1387f-186">Um valor de 0 (zero) **não** é considerado um tempo limite infinito.</span><span class="sxs-lookup"><span data-stu-id="1387f-186">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="1387f-187">Padrão: `120`</span><span class="sxs-lookup"><span data-stu-id="1387f-187">Default: `120`</span></span><br><span data-ttu-id="1387f-188">Mín.: `0`</span><span class="sxs-lookup"><span data-stu-id="1387f-188">Min: `0`</span></span><br><span data-ttu-id="1387f-189">Máx.: `3600`</span><span class="sxs-lookup"><span data-stu-id="1387f-189">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="1387f-190">Atributo booliano opcional.</span><span class="sxs-lookup"><span data-stu-id="1387f-190">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="1387f-191">Se for true, **stdout** e **stderr** para o processo especificado em **processPath** serão redirecionados para o arquivo especificado em **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="1387f-191">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="1387f-192">Atributo de cadeia de caracteres opcional.</span><span class="sxs-lookup"><span data-stu-id="1387f-192">Optional string attribute.</span></span></p><p><span data-ttu-id="1387f-193">Especifica o caminho relativo ou absoluto para o qual **stdout** e **stderr** do processo especificado em **processPath** são registrados em log.</span><span class="sxs-lookup"><span data-stu-id="1387f-193">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="1387f-194">Os caminhos relativos são relativos à raiz do site.</span><span class="sxs-lookup"><span data-stu-id="1387f-194">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="1387f-195">Qualquer caminho começando com `.` é relativo à raiz do site e todos os outros caminhos são tratados como caminhos absolutos.</span><span class="sxs-lookup"><span data-stu-id="1387f-195">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="1387f-196">As pastas fornecidas no caminho devem existir para que o módulo crie o arquivo de log.</span><span class="sxs-lookup"><span data-stu-id="1387f-196">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="1387f-197">Usando delimitadores de sublinhado, um carimbo de data/hora, uma ID de processo e a extensão de arquivo (*.log*) são adicionados ao último segmento do caminho **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="1387f-197">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="1387f-198">Se `.\logs\stdout` é fornecido como um valor, um log de exemplo stdout é salvo como *stdout_20180205194132_1934.log* na pasta *logs* quando salvos em 5/2/2018, às 19:41:32, com uma ID de processo de 1934.</span><span class="sxs-lookup"><span data-stu-id="1387f-198">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="= aspnetcore-2.1"

| <span data-ttu-id="1387f-199">Atributo</span><span class="sxs-lookup"><span data-stu-id="1387f-199">Attribute</span></span> | <span data-ttu-id="1387f-200">Descrição</span><span class="sxs-lookup"><span data-stu-id="1387f-200">Description</span></span> | <span data-ttu-id="1387f-201">Padrão</span><span class="sxs-lookup"><span data-stu-id="1387f-201">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="1387f-202">Atributo de cadeia de caracteres opcional.</span><span class="sxs-lookup"><span data-stu-id="1387f-202">Optional string attribute.</span></span></p><p><span data-ttu-id="1387f-203">Argumentos para o executável especificado em **processPath**.</span><span class="sxs-lookup"><span data-stu-id="1387f-203">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="1387f-204">Atributo booliano opcional.</span><span class="sxs-lookup"><span data-stu-id="1387f-204">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="1387f-205">Se for true, a página **502.5 – Falha do Processo** será suprimida e a página de código de status 502, configurada no *web.config*, terá precedência.</span><span class="sxs-lookup"><span data-stu-id="1387f-205">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="1387f-206">Atributo booliano opcional.</span><span class="sxs-lookup"><span data-stu-id="1387f-206">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="1387f-207">Se for true, o token será encaminhado para o processo filho escutando em %ASPNETCORE_PORT% como um cabeçalho 'MS-ASPNETCORE-WINAUTHTOKEN' por solicitação.</span><span class="sxs-lookup"><span data-stu-id="1387f-207">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="1387f-208">É responsabilidade desse processo chamar CloseHandle nesse token por solicitação.</span><span class="sxs-lookup"><span data-stu-id="1387f-208">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="1387f-209">Atributo inteiro opcional.</span><span class="sxs-lookup"><span data-stu-id="1387f-209">Optional integer attribute.</span></span></p><p><span data-ttu-id="1387f-210">Especifica o número de instâncias do processo especificado na configuração **processPath** que pode ser ativada por aplicativo.</span><span class="sxs-lookup"><span data-stu-id="1387f-210">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p> | <span data-ttu-id="1387f-211">Padrão: `1`</span><span class="sxs-lookup"><span data-stu-id="1387f-211">Default: `1`</span></span><br><span data-ttu-id="1387f-212">Mín.: `1`</span><span class="sxs-lookup"><span data-stu-id="1387f-212">Min: `1`</span></span><br><span data-ttu-id="1387f-213">Máx.: `100`</span><span class="sxs-lookup"><span data-stu-id="1387f-213">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="1387f-214">Atributo de cadeia de caracteres obrigatório.</span><span class="sxs-lookup"><span data-stu-id="1387f-214">Required string attribute.</span></span></p><p><span data-ttu-id="1387f-215">Caminho para o executável que inicia um processo que escuta solicitações HTTP.</span><span class="sxs-lookup"><span data-stu-id="1387f-215">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="1387f-216">Caminhos relativos são compatíveis.</span><span class="sxs-lookup"><span data-stu-id="1387f-216">Relative paths are supported.</span></span> <span data-ttu-id="1387f-217">Se o caminho começa com `.`, o caminho é considerado relativo à raiz do site.</span><span class="sxs-lookup"><span data-stu-id="1387f-217">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="1387f-218">Atributo inteiro opcional.</span><span class="sxs-lookup"><span data-stu-id="1387f-218">Optional integer attribute.</span></span></p><p><span data-ttu-id="1387f-219">Especifica o número de vezes que o processo especificado em **processPath** pode falhar por minuto.</span><span class="sxs-lookup"><span data-stu-id="1387f-219">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="1387f-220">Se esse limite for excedido, o módulo interromperá a inicialização do processo pelo restante do minuto.</span><span class="sxs-lookup"><span data-stu-id="1387f-220">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="1387f-221">Padrão: `10`</span><span class="sxs-lookup"><span data-stu-id="1387f-221">Default: `10`</span></span><br><span data-ttu-id="1387f-222">Mín.: `0`</span><span class="sxs-lookup"><span data-stu-id="1387f-222">Min: `0`</span></span><br><span data-ttu-id="1387f-223">Máx.: `100`</span><span class="sxs-lookup"><span data-stu-id="1387f-223">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="1387f-224">Atributo de intervalo de tempo opcional.</span><span class="sxs-lookup"><span data-stu-id="1387f-224">Optional timespan attribute.</span></span></p><p><span data-ttu-id="1387f-225">Especifica a duração para a qual o Módulo do ASP.NET Core aguarda uma resposta do processo que escuta em %ASPNETCORE_PORT%.</span><span class="sxs-lookup"><span data-stu-id="1387f-225">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="1387f-226">Em versões do Módulo do ASP.NET Core que acompanham a versão do ASP.NET Core 2.1 ou posterior, o `requestTimeout` é especificado em horas, minutos e segundos.</span><span class="sxs-lookup"><span data-stu-id="1387f-226">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p> | <span data-ttu-id="1387f-227">Padrão: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="1387f-227">Default: `00:02:00`</span></span><br><span data-ttu-id="1387f-228">Mín.: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="1387f-228">Min: `00:00:00`</span></span><br><span data-ttu-id="1387f-229">Máx.: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="1387f-229">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="1387f-230">Atributo inteiro opcional.</span><span class="sxs-lookup"><span data-stu-id="1387f-230">Optional integer attribute.</span></span></p><p><span data-ttu-id="1387f-231">Duração em segundos que o módulo espera para o executável desligar normalmente quando o arquivo *app_offline.htm* é detectado.</span><span class="sxs-lookup"><span data-stu-id="1387f-231">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="1387f-232">Padrão: `10`</span><span class="sxs-lookup"><span data-stu-id="1387f-232">Default: `10`</span></span><br><span data-ttu-id="1387f-233">Mín.: `0`</span><span class="sxs-lookup"><span data-stu-id="1387f-233">Min: `0`</span></span><br><span data-ttu-id="1387f-234">Máx.: `600`</span><span class="sxs-lookup"><span data-stu-id="1387f-234">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="1387f-235">Atributo inteiro opcional.</span><span class="sxs-lookup"><span data-stu-id="1387f-235">Optional integer attribute.</span></span></p><p><span data-ttu-id="1387f-236">Duração em segundos que o módulo espera para o arquivo executável iniciar um processo escutando na porta.</span><span class="sxs-lookup"><span data-stu-id="1387f-236">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="1387f-237">Se esse tempo limite é excedido, o módulo encerra o processo.</span><span class="sxs-lookup"><span data-stu-id="1387f-237">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="1387f-238">O módulo tentará reiniciar o processo quando ele receber uma nova solicitação e continuará a tentar reiniciar o processo em solicitações subsequentes de entrada, a menos que o aplicativo falhe em iniciar um número de vezes igual a **rapidFailsPerMinute** no último minuto sem interrupção.</span><span class="sxs-lookup"><span data-stu-id="1387f-238">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="1387f-239">Um valor de 0 (zero) **não** é considerado um tempo limite infinito.</span><span class="sxs-lookup"><span data-stu-id="1387f-239">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="1387f-240">Padrão: `120`</span><span class="sxs-lookup"><span data-stu-id="1387f-240">Default: `120`</span></span><br><span data-ttu-id="1387f-241">Mín.: `0`</span><span class="sxs-lookup"><span data-stu-id="1387f-241">Min: `0`</span></span><br><span data-ttu-id="1387f-242">Máx.: `3600`</span><span class="sxs-lookup"><span data-stu-id="1387f-242">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="1387f-243">Atributo booliano opcional.</span><span class="sxs-lookup"><span data-stu-id="1387f-243">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="1387f-244">Se for true, **stdout** e **stderr** para o processo especificado em **processPath** serão redirecionados para o arquivo especificado em **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="1387f-244">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="1387f-245">Atributo de cadeia de caracteres opcional.</span><span class="sxs-lookup"><span data-stu-id="1387f-245">Optional string attribute.</span></span></p><p><span data-ttu-id="1387f-246">Especifica o caminho relativo ou absoluto para o qual **stdout** e **stderr** do processo especificado em **processPath** são registrados em log.</span><span class="sxs-lookup"><span data-stu-id="1387f-246">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="1387f-247">Os caminhos relativos são relativos à raiz do site.</span><span class="sxs-lookup"><span data-stu-id="1387f-247">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="1387f-248">Qualquer caminho começando com `.` é relativo à raiz do site e todos os outros caminhos são tratados como caminhos absolutos.</span><span class="sxs-lookup"><span data-stu-id="1387f-248">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="1387f-249">As pastas fornecidas no caminho devem existir para que o módulo crie o arquivo de log.</span><span class="sxs-lookup"><span data-stu-id="1387f-249">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="1387f-250">Usando delimitadores de sublinhado, um carimbo de data/hora, uma ID de processo e a extensão de arquivo (*.log*) são adicionados ao último segmento do caminho **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="1387f-250">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="1387f-251">Se `.\logs\stdout` é fornecido como um valor, um log de exemplo stdout é salvo como *stdout_20180205194132_1934.log* na pasta *logs* quando salvos em 5/2/2018, às 19:41:32, com uma ID de processo de 1934.</span><span class="sxs-lookup"><span data-stu-id="1387f-251">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

| <span data-ttu-id="1387f-252">Atributo</span><span class="sxs-lookup"><span data-stu-id="1387f-252">Attribute</span></span> | <span data-ttu-id="1387f-253">Descrição</span><span class="sxs-lookup"><span data-stu-id="1387f-253">Description</span></span> | <span data-ttu-id="1387f-254">Padrão</span><span class="sxs-lookup"><span data-stu-id="1387f-254">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="1387f-255">Atributo de cadeia de caracteres opcional.</span><span class="sxs-lookup"><span data-stu-id="1387f-255">Optional string attribute.</span></span></p><p><span data-ttu-id="1387f-256">Argumentos para o executável especificado em **processPath**.</span><span class="sxs-lookup"><span data-stu-id="1387f-256">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="1387f-257">Atributo booliano opcional.</span><span class="sxs-lookup"><span data-stu-id="1387f-257">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="1387f-258">Se for true, a página **502.5 – Falha do Processo** será suprimida e a página de código de status 502, configurada no *web.config*, terá precedência.</span><span class="sxs-lookup"><span data-stu-id="1387f-258">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="1387f-259">Atributo booliano opcional.</span><span class="sxs-lookup"><span data-stu-id="1387f-259">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="1387f-260">Se for true, o token será encaminhado para o processo filho escutando em %ASPNETCORE_PORT% como um cabeçalho 'MS-ASPNETCORE-WINAUTHTOKEN' por solicitação.</span><span class="sxs-lookup"><span data-stu-id="1387f-260">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="1387f-261">É responsabilidade desse processo chamar CloseHandle nesse token por solicitação.</span><span class="sxs-lookup"><span data-stu-id="1387f-261">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="1387f-262">Atributo inteiro opcional.</span><span class="sxs-lookup"><span data-stu-id="1387f-262">Optional integer attribute.</span></span></p><p><span data-ttu-id="1387f-263">Especifica o número de instâncias do processo especificado na configuração **processPath** que pode ser ativada por aplicativo.</span><span class="sxs-lookup"><span data-stu-id="1387f-263">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p> | <span data-ttu-id="1387f-264">Padrão: `1`</span><span class="sxs-lookup"><span data-stu-id="1387f-264">Default: `1`</span></span><br><span data-ttu-id="1387f-265">Mín.: `1`</span><span class="sxs-lookup"><span data-stu-id="1387f-265">Min: `1`</span></span><br><span data-ttu-id="1387f-266">Máx.: `100`</span><span class="sxs-lookup"><span data-stu-id="1387f-266">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="1387f-267">Atributo de cadeia de caracteres obrigatório.</span><span class="sxs-lookup"><span data-stu-id="1387f-267">Required string attribute.</span></span></p><p><span data-ttu-id="1387f-268">Caminho para o executável que inicia um processo que escuta solicitações HTTP.</span><span class="sxs-lookup"><span data-stu-id="1387f-268">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="1387f-269">Caminhos relativos são compatíveis.</span><span class="sxs-lookup"><span data-stu-id="1387f-269">Relative paths are supported.</span></span> <span data-ttu-id="1387f-270">Se o caminho começa com `.`, o caminho é considerado relativo à raiz do site.</span><span class="sxs-lookup"><span data-stu-id="1387f-270">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="1387f-271">Atributo inteiro opcional.</span><span class="sxs-lookup"><span data-stu-id="1387f-271">Optional integer attribute.</span></span></p><p><span data-ttu-id="1387f-272">Especifica o número de vezes que o processo especificado em **processPath** pode falhar por minuto.</span><span class="sxs-lookup"><span data-stu-id="1387f-272">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="1387f-273">Se esse limite for excedido, o módulo interromperá a inicialização do processo pelo restante do minuto.</span><span class="sxs-lookup"><span data-stu-id="1387f-273">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="1387f-274">Padrão: `10`</span><span class="sxs-lookup"><span data-stu-id="1387f-274">Default: `10`</span></span><br><span data-ttu-id="1387f-275">Mín.: `0`</span><span class="sxs-lookup"><span data-stu-id="1387f-275">Min: `0`</span></span><br><span data-ttu-id="1387f-276">Máx.: `100`</span><span class="sxs-lookup"><span data-stu-id="1387f-276">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="1387f-277">Atributo de intervalo de tempo opcional.</span><span class="sxs-lookup"><span data-stu-id="1387f-277">Optional timespan attribute.</span></span></p><p><span data-ttu-id="1387f-278">Especifica a duração para a qual o Módulo do ASP.NET Core aguarda uma resposta do processo que escuta em %ASPNETCORE_PORT%.</span><span class="sxs-lookup"><span data-stu-id="1387f-278">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="1387f-279">Em versões do Módulo ASP.NET Core que acompanham a versão do ASP.NET Core 2.0 ou anterior, o `requestTimeout` deve ser especificado somente em minutos inteiros, caso contrário, ele assume o valor padrão de 2 minutos.</span><span class="sxs-lookup"><span data-stu-id="1387f-279">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.0 or earlier, the `requestTimeout` must be specified in whole minutes only, otherwise it defaults to 2 minutes.</span></span></p> | <span data-ttu-id="1387f-280">Padrão: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="1387f-280">Default: `00:02:00`</span></span><br><span data-ttu-id="1387f-281">Mín.: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="1387f-281">Min: `00:00:00`</span></span><br><span data-ttu-id="1387f-282">Máx.: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="1387f-282">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="1387f-283">Atributo inteiro opcional.</span><span class="sxs-lookup"><span data-stu-id="1387f-283">Optional integer attribute.</span></span></p><p><span data-ttu-id="1387f-284">Duração em segundos que o módulo espera para o executável desligar normalmente quando o arquivo *app_offline.htm* é detectado.</span><span class="sxs-lookup"><span data-stu-id="1387f-284">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="1387f-285">Padrão: `10`</span><span class="sxs-lookup"><span data-stu-id="1387f-285">Default: `10`</span></span><br><span data-ttu-id="1387f-286">Mín.: `0`</span><span class="sxs-lookup"><span data-stu-id="1387f-286">Min: `0`</span></span><br><span data-ttu-id="1387f-287">Máx.: `600`</span><span class="sxs-lookup"><span data-stu-id="1387f-287">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="1387f-288">Atributo inteiro opcional.</span><span class="sxs-lookup"><span data-stu-id="1387f-288">Optional integer attribute.</span></span></p><p><span data-ttu-id="1387f-289">Duração em segundos que o módulo espera para o arquivo executável iniciar um processo escutando na porta.</span><span class="sxs-lookup"><span data-stu-id="1387f-289">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="1387f-290">Se esse tempo limite é excedido, o módulo encerra o processo.</span><span class="sxs-lookup"><span data-stu-id="1387f-290">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="1387f-291">O módulo tentará reiniciar o processo quando ele receber uma nova solicitação e continuará a tentar reiniciar o processo em solicitações subsequentes de entrada, a menos que o aplicativo falhe em iniciar um número de vezes igual a **rapidFailsPerMinute** no último minuto sem interrupção.</span><span class="sxs-lookup"><span data-stu-id="1387f-291">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p> | <span data-ttu-id="1387f-292">Padrão: `120`</span><span class="sxs-lookup"><span data-stu-id="1387f-292">Default: `120`</span></span><br><span data-ttu-id="1387f-293">Mín.: `0`</span><span class="sxs-lookup"><span data-stu-id="1387f-293">Min: `0`</span></span><br><span data-ttu-id="1387f-294">Máx.: `3600`</span><span class="sxs-lookup"><span data-stu-id="1387f-294">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="1387f-295">Atributo booliano opcional.</span><span class="sxs-lookup"><span data-stu-id="1387f-295">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="1387f-296">Se for true, **stdout** e **stderr** para o processo especificado em **processPath** serão redirecionados para o arquivo especificado em **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="1387f-296">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="1387f-297">Atributo de cadeia de caracteres opcional.</span><span class="sxs-lookup"><span data-stu-id="1387f-297">Optional string attribute.</span></span></p><p><span data-ttu-id="1387f-298">Especifica o caminho relativo ou absoluto para o qual **stdout** e **stderr** do processo especificado em **processPath** são registrados em log.</span><span class="sxs-lookup"><span data-stu-id="1387f-298">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="1387f-299">Os caminhos relativos são relativos à raiz do site.</span><span class="sxs-lookup"><span data-stu-id="1387f-299">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="1387f-300">Qualquer caminho começando com `.` é relativo à raiz do site e todos os outros caminhos são tratados como caminhos absolutos.</span><span class="sxs-lookup"><span data-stu-id="1387f-300">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="1387f-301">As pastas fornecidas no caminho devem existir para que o módulo crie o arquivo de log.</span><span class="sxs-lookup"><span data-stu-id="1387f-301">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="1387f-302">Usando delimitadores de sublinhado, um carimbo de data/hora, uma ID de processo e a extensão de arquivo (*.log*) são adicionados ao último segmento do caminho **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="1387f-302">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="1387f-303">Se `.\logs\stdout` é fornecido como um valor, um log de exemplo stdout é salvo como *stdout_20180205194132_1934.log* na pasta *logs* quando salvos em 5/2/2018, às 19:41:32, com uma ID de processo de 1934.</span><span class="sxs-lookup"><span data-stu-id="1387f-303">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

### <a name="setting-environment-variables"></a><span data-ttu-id="1387f-304">Definindo variáveis de ambiente</span><span class="sxs-lookup"><span data-stu-id="1387f-304">Setting environment variables</span></span>

<span data-ttu-id="1387f-305">Variáveis de ambiente podem ser especificadas para o processo no atributo `processPath`.</span><span class="sxs-lookup"><span data-stu-id="1387f-305">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="1387f-306">Especificar uma variável de ambiente com o elemento filho `environmentVariable` de um elemento de coleção `environmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="1387f-306">Specify an environment variable with the `environmentVariable` child element of an `environmentVariables` collection element.</span></span> <span data-ttu-id="1387f-307">Variáveis de ambiente definidas nesta seção têm precedência sobre variáveis de ambiente do sistema.</span><span class="sxs-lookup"><span data-stu-id="1387f-307">Environment variables set in this section take precedence over system environment variables.</span></span>

<span data-ttu-id="1387f-308">O exemplo a seguir define duas variáveis de ambiente.</span><span class="sxs-lookup"><span data-stu-id="1387f-308">The following example sets two environment variables.</span></span> <span data-ttu-id="1387f-309">`ASPNETCORE_ENVIRONMENT` configura o ambiente do aplicativo para `Development`.</span><span class="sxs-lookup"><span data-stu-id="1387f-309">`ASPNETCORE_ENVIRONMENT` configures the app's environment to `Development`.</span></span> <span data-ttu-id="1387f-310">Um desenvolvedor pode definir esse valor temporariamente no arquivo *web.config* para forçar o carregamento da [Página de Exceções do Desenvolvedor](xref:fundamentals/error-handling) ao depurar uma exceção de aplicativo.</span><span class="sxs-lookup"><span data-stu-id="1387f-310">A developer may temporarily set this value in the *web.config* file in order to force the [Developer Exception Page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="1387f-311">`CONFIG_DIR` é um exemplo de uma variável de ambiente definida pelo usuário, em que o desenvolvedor escreveu código que lê o valor de inicialização para formar um caminho no qual carregar o arquivo de configuração do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="1387f-311">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that reads the value on startup to form a path for loading the app's configuration file.</span></span>

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
> <span data-ttu-id="1387f-312">Defina a variável de ambiente apenas `ASPNETCORE_ENVIRONMENT` para `Development` em servidores de preparo e de teste que não estão acessíveis a redes não confiáveis, tais como a Internet.</span><span class="sxs-lookup"><span data-stu-id="1387f-312">Only set the `ASPNETCORE_ENVIRONMENT` environment variable to `Development` on staging and testing servers that aren't accessible to untrusted networks, such as the Internet.</span></span>

## <a name="appofflinehtm"></a><span data-ttu-id="1387f-313">app_offline.htm</span><span class="sxs-lookup"><span data-stu-id="1387f-313">app_offline.htm</span></span>

<span data-ttu-id="1387f-314">Se um arquivo com o nome *app_offline.htm* é detectado no diretório raiz de um aplicativo, o Módulo do ASP.NET Core tenta desligar normalmente o aplicativo e parar o processamento de solicitações de entrada.</span><span class="sxs-lookup"><span data-stu-id="1387f-314">If a file with the name *app_offline.htm* is detected in the root directory of an app, the ASP.NET Core Module attempts to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="1387f-315">Se o aplicativo ainda está em execução após o número de segundos definido em `shutdownTimeLimit`, o Módulo do ASP.NET Core encerra o processo em execução.</span><span class="sxs-lookup"><span data-stu-id="1387f-315">If the app is still running after the number of seconds defined in `shutdownTimeLimit`, the ASP.NET Core Module kills the running process.</span></span>

<span data-ttu-id="1387f-316">Enquanto o arquivo *app_offline.htm* estiver presente, o Módulo do ASP.NET Core responderá às solicitações enviando o conteúdo do arquivo *app_offline.htm*.</span><span class="sxs-lookup"><span data-stu-id="1387f-316">While the *app_offline.htm* file is present, the ASP.NET Core Module responds to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="1387f-317">Quando o arquivo *app_offline.htm* é removido, a próxima solicitação inicia o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="1387f-317">When the *app_offline.htm* file is removed, the next request starts the app.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="1387f-318">Ao usar o modelo de hospedagem de fora do processo, talvez o aplicativo não desligue imediatamente se houver uma conexão aberta.</span><span class="sxs-lookup"><span data-stu-id="1387f-318">When using the out-of-process hosting model, the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="1387f-319">Por exemplo, uma conexão websocket pode atrasar o desligamento do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="1387f-319">For example, a websocket connection may delay app shut down.</span></span>

::: moniker-end

## <a name="start-up-error-page"></a><span data-ttu-id="1387f-320">Página de erro de inicialização</span><span class="sxs-lookup"><span data-stu-id="1387f-320">Start-up error page</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="1387f-321">A hospedagem em processo e fora do processo produzem páginas de erro personalizadas quando falham ao iniciar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="1387f-321">Both in-process and out-of-process hosting produce custom error pages when they fail to start the app.</span></span>

<span data-ttu-id="1387f-322">Se o Módulo do ASP.NET Core falhar ao encontrar o manipulador de solicitação em processo ou fora do processo, uma página de código de status *500.0 – falha ao carregar manipulador no processo/fora do processo* será exibida.</span><span class="sxs-lookup"><span data-stu-id="1387f-322">If the ASP.NET Core Module fails to find either the in-process or out-of-process request handler, a *500.0 - In-Process/Out-Of-Process Handler Load Failure* status code page appears.</span></span>

<span data-ttu-id="1387f-323">No caso da hospedagem em processo, se o módulo do ASP.NET Core falhar ao iniciar o aplicativo, uma página de código de status *500.30 – falha ao iniciar* será exibida.</span><span class="sxs-lookup"><span data-stu-id="1387f-323">For in-process hosting if the ASP.NET Core Module fails to start the app, a *500.30 - Start Failure* status code page appears.</span></span>

<span data-ttu-id="1387f-324">Para hospedagem fora do processo, se o Módulo do ASP.NET Core falhar ao iniciar o processo de back-end ou se o processo de back-end iniciar, mas falhar ao escutar na porta configurada, uma página de código de status *502.5 – falha no processo* será exibida.</span><span class="sxs-lookup"><span data-stu-id="1387f-324">For out-of-process hosting if the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span>

<span data-ttu-id="1387f-325">Para suprimir essa página e reverter para a página de código de status 5xx padrão do IIS, use o atributo `disableStartUpErrorPage`.</span><span class="sxs-lookup"><span data-stu-id="1387f-325">To suppress this page and revert to the default IIS 5xx status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="1387f-326">Para obter mais informações sobre como configurar mensagens de erro personalizadas, veja [Erros HTTP &lt;httpErrors&gt;](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="1387f-326">For more information on configuring custom error messages, see [HTTP Errors &lt;httpErrors&gt;](/iis/configuration/system.webServer/httpErrors/).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="1387f-327">Se o Módulo do ASP.NET Core falhar ao iniciar o processo de back-end ou se o processo de back-end iniciar, mas falhar ao escutar na porta configurada, uma página de código de status *502.5 – falha no processo* será exibida.</span><span class="sxs-lookup"><span data-stu-id="1387f-327">If the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span> <span data-ttu-id="1387f-328">Para omitir esta página e reverter para a página de código de status 502 padrão do IIS, use o atributo `disableStartUpErrorPage`.</span><span class="sxs-lookup"><span data-stu-id="1387f-328">To suppress this page and revert to the default IIS 502 status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="1387f-329">Para obter mais informações sobre como configurar mensagens de erro personalizadas, veja [Erros HTTP &lt;httpErrors&gt;](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="1387f-329">For more information on configuring custom error messages, see [HTTP Errors &lt;httpErrors&gt;](/iis/configuration/system.webServer/httpErrors/).</span></span>

![Página de código de status 502.5 – Falha do Processo](aspnet-core-module/_static/ANCM-502_5.png)

::: moniker-end

## <a name="log-creation-and-redirection"></a><span data-ttu-id="1387f-331">Criação de log e redirecionamento</span><span class="sxs-lookup"><span data-stu-id="1387f-331">Log creation and redirection</span></span>

<span data-ttu-id="1387f-332">O Módulo do ASP.NET Core redireciona as saídas de console stdout e stderr para o disco se os atributos `stdoutLogEnabled` e `stdoutLogFile` do elemento `aspNetCore` forem definidos.</span><span class="sxs-lookup"><span data-stu-id="1387f-332">The ASP.NET Core Module redirects stdout and stderr console output to disk if the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element are set.</span></span> <span data-ttu-id="1387f-333">As pastas no caminho `stdoutLogFile` devem existir para que o módulo crie o arquivo de log.</span><span class="sxs-lookup"><span data-stu-id="1387f-333">Any folders in the `stdoutLogFile` path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="1387f-334">O pool de aplicativos deve ter acesso de gravação ao local em que os logs foram gravados (use `IIS AppPool\<app_pool_name>` para fornecer permissão de gravação).</span><span class="sxs-lookup"><span data-stu-id="1387f-334">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="1387f-335">Logs não sofrem rotação, a menos que ocorra a reciclagem/reinicialização do processo.</span><span class="sxs-lookup"><span data-stu-id="1387f-335">Logs aren't rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="1387f-336">É responsabilidade do hoster limitar o espaço em disco consumido pelos logs.</span><span class="sxs-lookup"><span data-stu-id="1387f-336">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span>

<span data-ttu-id="1387f-337">Usar o log de stdout é recomendado apenas para solucionar problemas de inicialização do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="1387f-337">Using the stdout log is only recommended for troubleshooting app startup issues.</span></span> <span data-ttu-id="1387f-338">Não use o log de stdout para fins gerais de registro em log do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="1387f-338">Don't use the stdout log for general app logging purposes.</span></span> <span data-ttu-id="1387f-339">Para registro em log de rotina em um aplicativo ASP.NET Core, use uma biblioteca de registro em log que limita o tamanho do arquivo de log e realiza a rotação de logs.</span><span class="sxs-lookup"><span data-stu-id="1387f-339">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="1387f-340">Para obter mais informações, veja [provedores de log de terceiros](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="1387f-340">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

<span data-ttu-id="1387f-341">Uma extensão de arquivo e um carimbo de data/hora são adicionados automaticamente quando o arquivo de log é criado.</span><span class="sxs-lookup"><span data-stu-id="1387f-341">A timestamp and file extension are added automatically when the log file is created.</span></span> <span data-ttu-id="1387f-342">O nome do arquivo de log é composto por meio do acréscimo do carimbo de data/hora, da ID do processo e da extensão de arquivo (*.log*) para o último segmento do caminho `stdoutLogFile` (normalmente *stdout*), delimitados por sublinhados.</span><span class="sxs-lookup"><span data-stu-id="1387f-342">The log file name is composed by appending the timestamp, process ID, and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="1387f-343">Se o caminho `stdoutLogFile` termina com *stdout*, um log para um aplicativo com um PID de 1934, criado em 5/2/2018 às 19:42:32, tem o nome de arquivo *stdout_20180205194132_1934.log*.</span><span class="sxs-lookup"><span data-stu-id="1387f-343">If the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 1934 created on 2/5/2018 at 19:42:32 has the file name *stdout_20180205194132_1934.log*.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="1387f-344">Se `stdoutLogEnabled` for falso, os erros que ocorrerem na inicialização do aplicativo serão capturados e emitidos no log de eventos até 30 KB.</span><span class="sxs-lookup"><span data-stu-id="1387f-344">If `stdoutLogEnabled` is false, errors that occur on app startup are captured and emitted to the event log up to 30 KB.</span></span> <span data-ttu-id="1387f-345">Após a inicialização, todos os logs adicionais são descartados.</span><span class="sxs-lookup"><span data-stu-id="1387f-345">After startup, all additional logs are discarded.</span></span>

::: moniker-end

<span data-ttu-id="1387f-346">O elemento `aspNetCore` de exemplo a seguir configura o registro em log de stdout para um aplicativo hospedado no Serviço de Aplicativo do Azure.</span><span class="sxs-lookup"><span data-stu-id="1387f-346">The following sample `aspNetCore` element configures stdout logging for an app hosted in Azure App Service.</span></span> <span data-ttu-id="1387f-347">Um caminho local ou um caminho de compartilhamento de rede é aceitável para o registro em log local.</span><span class="sxs-lookup"><span data-stu-id="1387f-347">A local path or network share path is acceptable for local logging.</span></span> <span data-ttu-id="1387f-348">Confirme se a identidade do usuário AppPool tem permissão para gravar no caminho fornecido.</span><span class="sxs-lookup"><span data-stu-id="1387f-348">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

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

## <a name="enhanced-diagnostic-logs"></a><span data-ttu-id="1387f-349">Logs de diagnóstico avançados</span><span class="sxs-lookup"><span data-stu-id="1387f-349">Enhanced diagnostic logs</span></span>

<span data-ttu-id="1387f-350">O Módulo do ASP.NET Core pode ser configurado para fornecer logs de diagnóstico avançados.</span><span class="sxs-lookup"><span data-stu-id="1387f-350">The ASP.NET Core Module provides is configurable to provide enhanced diagnostics logs.</span></span> <span data-ttu-id="1387f-351">Adicione o elemento `<handlerSettings>` ao elemento `<aspNetCore>` no *web.config*. A definição de `debugLevel` como `TRACE` expõe uma fidelidade maior de informações de diagnóstico:</span><span class="sxs-lookup"><span data-stu-id="1387f-351">Add the `<handlerSettings>` element to the `<aspNetCore>` element in *web.config*. Setting the `debugLevel` to `TRACE` exposes a higher fidelity of diagnostic information:</span></span>

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

<span data-ttu-id="1387f-352">Os valores do nível de depuração (`debugLevel`) podem incluir o nível e a localização.</span><span class="sxs-lookup"><span data-stu-id="1387f-352">Debug level (`debugLevel`) values can include both the level and the location.</span></span>

<span data-ttu-id="1387f-353">Níveis (na ordem do menos para o mais detalhado):</span><span class="sxs-lookup"><span data-stu-id="1387f-353">Levels (in order from least to most verbose):</span></span>

* <span data-ttu-id="1387f-354">ERROR</span><span class="sxs-lookup"><span data-stu-id="1387f-354">ERROR</span></span>
* <span data-ttu-id="1387f-355">WARNING</span><span class="sxs-lookup"><span data-stu-id="1387f-355">WARNING</span></span>
* <span data-ttu-id="1387f-356">INFO</span><span class="sxs-lookup"><span data-stu-id="1387f-356">INFO</span></span>
* <span data-ttu-id="1387f-357">TRACE</span><span class="sxs-lookup"><span data-stu-id="1387f-357">TRACE</span></span>

<span data-ttu-id="1387f-358">Locais (vários locais são permitidos):</span><span class="sxs-lookup"><span data-stu-id="1387f-358">Locations (multiple locations are permitted):</span></span>

* <span data-ttu-id="1387f-359">CONSOLE</span><span class="sxs-lookup"><span data-stu-id="1387f-359">CONSOLE</span></span>
* <span data-ttu-id="1387f-360">EVENTLOG</span><span class="sxs-lookup"><span data-stu-id="1387f-360">EVENTLOG</span></span>
* <span data-ttu-id="1387f-361">FILE</span><span class="sxs-lookup"><span data-stu-id="1387f-361">FILE</span></span>

<span data-ttu-id="1387f-362">As configurações do manipulador também podem ser fornecidas por meio de variáveis de ambiente:</span><span class="sxs-lookup"><span data-stu-id="1387f-362">The handler settings can also be provided via environment variables:</span></span>

* <span data-ttu-id="1387f-363">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; caminho para o arquivo de log de depuração.</span><span class="sxs-lookup"><span data-stu-id="1387f-363">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Path to the debug log file.</span></span> <span data-ttu-id="1387f-364">(Padrão: *aspnetcore-debug.log*)</span><span class="sxs-lookup"><span data-stu-id="1387f-364">(Default: *aspnetcore-debug.log*)</span></span>
* <span data-ttu-id="1387f-365">`ASPNETCORE_MODULE_DEBUG` &ndash; configuração do nível de depuração.</span><span class="sxs-lookup"><span data-stu-id="1387f-365">`ASPNETCORE_MODULE_DEBUG` &ndash; Debug level setting.</span></span>

> [!WARNING]
> <span data-ttu-id="1387f-366">**Não** deixe o log de depuração habilitado na implantação por mais tempo que o necessário para solucionar um problema.</span><span class="sxs-lookup"><span data-stu-id="1387f-366">Do **not** leave debug logging enabled in the deployment for longer than required to troubleshoot an issue.</span></span> <span data-ttu-id="1387f-367">O tamanho do log não é limitado.</span><span class="sxs-lookup"><span data-stu-id="1387f-367">The size of the log isn't limited.</span></span> <span data-ttu-id="1387f-368">Deixar o log de depuração habilitado pode esgotar o espaço em disco disponível e causar falha no servidor ou no serviço de aplicativo.</span><span class="sxs-lookup"><span data-stu-id="1387f-368">Leaving the debug log enabled can exhaust the available disk space and crash the server or app service.</span></span>

::: moniker-end

<span data-ttu-id="1387f-369">Veja [Configuração com web.config](#configuration-with-webconfig) para obter um exemplo do elemento `aspNetCore` no arquivo *web.config*.</span><span class="sxs-lookup"><span data-stu-id="1387f-369">See [Configuration with web.config](#configuration-with-webconfig) for an example of the `aspNetCore` element in the *web.config* file.</span></span>

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a><span data-ttu-id="1387f-370">A configuração de proxy usa o protocolo HTTP e um token de emparelhamento</span><span class="sxs-lookup"><span data-stu-id="1387f-370">Proxy configuration uses HTTP protocol and a pairing token</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="1387f-371">*Só se aplica à hospedagem de fora do processo.*</span><span class="sxs-lookup"><span data-stu-id="1387f-371">*Only applies to out-of-process hosting.*</span></span>

::: moniker-end

<span data-ttu-id="1387f-372">O proxy criado entre o Módulo do ASP.NET Core e o Kestrel usa o protocolo HTTP.</span><span class="sxs-lookup"><span data-stu-id="1387f-372">The proxy created between the ASP.NET Core Module and Kestrel uses the HTTP protocol.</span></span> <span data-ttu-id="1387f-373">O uso de HTTP é uma otimização de desempenho na qual o tráfego entre o módulo e o Kestrel ocorre em um endereço de loopback fora do adaptador de rede.</span><span class="sxs-lookup"><span data-stu-id="1387f-373">Using HTTP is a performance optimization, where the traffic between the module and Kestrel takes place on a loopback address off of the network interface.</span></span> <span data-ttu-id="1387f-374">Não há nenhum risco de interceptação do tráfego entre o módulo e o Kestrel em um local fora do servidor.</span><span class="sxs-lookup"><span data-stu-id="1387f-374">There's no risk of eavesdropping the traffic between the module and Kestrel from a location off of the server.</span></span>

<span data-ttu-id="1387f-375">Um token de emparelhamento é usado para assegurar que as solicitações recebidas pelo Kestrel foram transmitidas por proxy pelo IIS e que não são provenientes de outra origem.</span><span class="sxs-lookup"><span data-stu-id="1387f-375">A pairing token is used to guarantee that the requests received by Kestrel were proxied by IIS and didn't come from some other source.</span></span> <span data-ttu-id="1387f-376">O token de emparelhamento é criado e definido em uma variável de ambiente (`ASPNETCORE_TOKEN`) pelo módulo.</span><span class="sxs-lookup"><span data-stu-id="1387f-376">The pairing token is created and set into an environment variable (`ASPNETCORE_TOKEN`) by the module.</span></span> <span data-ttu-id="1387f-377">O token de emparelhamento também é definido em um cabeçalho (`MSAspNetCoreToken`) em cada solicitação com proxy.</span><span class="sxs-lookup"><span data-stu-id="1387f-377">The pairing token is also set into a header (`MSAspNetCoreToken`) on every proxied request.</span></span> <span data-ttu-id="1387f-378">O Middleware do IIS verifica cada solicitação recebida para confirmar se o valor de cabeçalho do token de emparelhamento corresponde ao valor da variável de ambiente.</span><span class="sxs-lookup"><span data-stu-id="1387f-378">IIS Middleware checks each request it receives to confirm that the pairing token header value matches the environment variable value.</span></span> <span data-ttu-id="1387f-379">Se os valores do token forem incompatíveis, a solicitação será registrada em log e rejeitada.</span><span class="sxs-lookup"><span data-stu-id="1387f-379">If the token values are mismatched, the request is logged and rejected.</span></span> <span data-ttu-id="1387f-380">A variável de ambiente do token de emparelhamento e o tráfego entre o módulo e o Kestrel não são acessíveis em um local fora do servidor.</span><span class="sxs-lookup"><span data-stu-id="1387f-380">The pairing token environment variable and the traffic between the module and Kestrel aren't accessible from a location off of the server.</span></span> <span data-ttu-id="1387f-381">Sem saber o valor do token de emparelhamento, um invasor não pode enviar solicitações que ignoram a verificação no Middleware do IIS.</span><span class="sxs-lookup"><span data-stu-id="1387f-381">Without knowing the pairing token value, an attacker can't submit requests that bypass the check in the IIS Middleware.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="1387f-382">Módulo do ASP.NET Core com uma configuração do IIS compartilhada</span><span class="sxs-lookup"><span data-stu-id="1387f-382">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="1387f-383">O instalador do módulo do ASP.NET Core é executado com os privilégios da conta de **SISTEMA**.</span><span class="sxs-lookup"><span data-stu-id="1387f-383">The ASP.NET Core Module installer runs with the privileges of the **SYSTEM** account.</span></span> <span data-ttu-id="1387f-384">Já que a conta de sistema local não tem permissão para modificar o caminho do compartilhamento usado pela configuração compartilhada de IIS, o instalador experimenta um erro de acesso negado ao tentar definir as configurações de módulo em *applicationHost.config* no compartilhamento.</span><span class="sxs-lookup"><span data-stu-id="1387f-384">Because the local system account doesn't have modify permission for the share path used by the IIS Shared Configuration, the installer hits an access denied error when attempting to configure the module settings in *applicationHost.config* on the share.</span></span> <span data-ttu-id="1387f-385">Ao usar uma configuração compartilhada de IIS, siga estas etapas:</span><span class="sxs-lookup"><span data-stu-id="1387f-385">When using an IIS Shared Configuration, follow these steps:</span></span>

1. <span data-ttu-id="1387f-386">Desabilite a configuração compartilhada de IIS.</span><span class="sxs-lookup"><span data-stu-id="1387f-386">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="1387f-387">Execute o instalador.</span><span class="sxs-lookup"><span data-stu-id="1387f-387">Run the installer.</span></span>
1. <span data-ttu-id="1387f-388">Exportar o arquivo *applicationHost.config* atualizado para o compartilhamento.</span><span class="sxs-lookup"><span data-stu-id="1387f-388">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="1387f-389">Reabilite a Configuração Compartilhada do IIS.</span><span class="sxs-lookup"><span data-stu-id="1387f-389">Re-enable the IIS Shared Configuration.</span></span>

## <a name="module-version-and-hosting-bundle-installer-logs"></a><span data-ttu-id="1387f-390">Versão do módulo e logs do instalador do pacote de hospedagem</span><span class="sxs-lookup"><span data-stu-id="1387f-390">Module version and Hosting Bundle installer logs</span></span>

<span data-ttu-id="1387f-391">Para determinar a versão do Módulo do ASP.NET Core instalado:</span><span class="sxs-lookup"><span data-stu-id="1387f-391">To determine the version of the installed ASP.NET Core Module:</span></span>

1. <span data-ttu-id="1387f-392">No sistema de hospedagem, navegue até *%windir%\System32\inetsrv*.</span><span class="sxs-lookup"><span data-stu-id="1387f-392">On the hosting system, navigate to *%windir%\System32\inetsrv*.</span></span>
1. <span data-ttu-id="1387f-393">Localize o arquivo *aspnetcore.dll*.</span><span class="sxs-lookup"><span data-stu-id="1387f-393">Locate the *aspnetcore.dll* file.</span></span>
1. <span data-ttu-id="1387f-394">Clique com o botão direito do mouse no arquivo e selecione **Propriedades** no menu contextual.</span><span class="sxs-lookup"><span data-stu-id="1387f-394">Right-click the file and select **Properties** from the contextual menu.</span></span>
1. <span data-ttu-id="1387f-395">Selecione a guia **Detalhes**. A **Versão do arquivo** e a **Versão do produto** representam a versão instalada do módulo.</span><span class="sxs-lookup"><span data-stu-id="1387f-395">Select the **Details** tab. The **File version** and **Product version** represent the installed version of the module.</span></span>

<span data-ttu-id="1387f-396">Os logs de instalador do pacote de hospedagem para o módulo são encontrados em *C:\\Usuários\\%UserName%\\AppData\\Local\\Temp*. O arquivo é nomeado *dd_DotNetCoreWinSvrHosting__\<carimbo de data/hora>_000_AspNetCoreModule_x64.log*.</span><span class="sxs-lookup"><span data-stu-id="1387f-396">The Hosting Bundle installer logs for the module are found at *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. The file is named *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="1387f-397">Locais dos arquivos de módulo, de esquema e de configuração</span><span class="sxs-lookup"><span data-stu-id="1387f-397">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="1387f-398">Módulo</span><span class="sxs-lookup"><span data-stu-id="1387f-398">Module</span></span>

<span data-ttu-id="1387f-399">**IIS (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="1387f-399">**IIS (x86/amd64):**</span></span>

   * <span data-ttu-id="1387f-400">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="1387f-400">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

   * <span data-ttu-id="1387f-401">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="1387f-401">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="1387f-402">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="1387f-402">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

   * <span data-ttu-id="1387f-403">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="1387f-403">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

::: moniker-end

<span data-ttu-id="1387f-404">**IIS Express (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="1387f-404">**IIS Express (x86/amd64):**</span></span>

   * <span data-ttu-id="1387f-405">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="1387f-405">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

   * <span data-ttu-id="1387f-406">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="1387f-406">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="1387f-407">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="1387f-407">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

   * <span data-ttu-id="1387f-408">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="1387f-408">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

::: moniker-end

### <a name="schema"></a><span data-ttu-id="1387f-409">Esquema</span><span class="sxs-lookup"><span data-stu-id="1387f-409">Schema</span></span>

<span data-ttu-id="1387f-410">**IIS**</span><span class="sxs-lookup"><span data-stu-id="1387f-410">**IIS**</span></span>

   * <span data-ttu-id="1387f-411">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="1387f-411">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="1387f-412">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="1387f-412">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span></span>

::: moniker-end
<span data-ttu-id="1387f-413">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="1387f-413">**IIS Express**</span></span>

   * <span data-ttu-id="1387f-414">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="1387f-414">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="1387f-415">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="1387f-415">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span></span>

::: moniker-end

### <a name="configuration"></a><span data-ttu-id="1387f-416">Configuração</span><span class="sxs-lookup"><span data-stu-id="1387f-416">Configuration</span></span>

<span data-ttu-id="1387f-417">**IIS**</span><span class="sxs-lookup"><span data-stu-id="1387f-417">**IIS**</span></span>

   * <span data-ttu-id="1387f-418">%windir%\System32\inetsrv\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="1387f-418">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="1387f-419">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="1387f-419">**IIS Express**</span></span>

   * <span data-ttu-id="1387f-420">%ProgramFiles%\IIS Express\config\templates\PersonalWebServer\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="1387f-420">%ProgramFiles%\IIS Express\config\templates\PersonalWebServer\applicationHost.config</span></span>

<span data-ttu-id="1387f-421">Os arquivos podem ser encontrados pesquisando por *aspnetcore* no arquivo *applicationHost.config*.</span><span class="sxs-lookup"><span data-stu-id="1387f-421">The files can be found by searching for *aspnetcore* in the *applicationHost.config* file.</span></span>
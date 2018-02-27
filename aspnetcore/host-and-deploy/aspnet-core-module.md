---
title: "Referência de configuração de módulo principal do ASP.NET"
author: guardrex
description: "Saiba como configurar o módulo do núcleo do ASP.NET para hospedar aplicativos ASP.NET Core."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 02/15/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: c01abed767a226eae68725c1c53d922eac2f705e
ms.sourcegitcommit: 49fb3b7669b504d35edad34db8285e56b958a9fc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/23/2018
---
# <a name="aspnet-core-module-configuration-reference"></a><span data-ttu-id="c3971-103">Referência de configuração de módulo principal do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="c3971-103">ASP.NET Core Module configuration reference</span></span>

<span data-ttu-id="c3971-104">Por [Luke Latham](https://github.com/guardrex), [Rick Anderson](https://twitter.com/RickAndMSFT), e [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="c3971-104">By [Luke Latham](https://github.com/guardrex), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="c3971-105">Este documento fornece instruções sobre como configurar o módulo do núcleo do ASP.NET para hospedar aplicativos ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c3971-105">This document provides instructions on how to configure the ASP.NET Core Module for hosting ASP.NET Core apps.</span></span> <span data-ttu-id="c3971-106">Para obter uma introdução ao ASP.NET Core Module e instruções de instalação, consulte o [visão geral do módulo do ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="c3971-106">For an introduction to the ASP.NET Core Module and installation instructions, see the [ASP.NET Core Module overview](xref:fundamentals/servers/aspnet-core-module).</span></span>

## <a name="configuration-with-webconfig"></a><span data-ttu-id="c3971-107">Configuração com a Web. config</span><span class="sxs-lookup"><span data-stu-id="c3971-107">Configuration with web.config</span></span>

<span data-ttu-id="c3971-108">O módulo de núcleo do ASP.NET está configurado com o `aspNetCore` seção o `system.webServer` nó do site *Web. config* arquivo.</span><span class="sxs-lookup"><span data-stu-id="c3971-108">The ASP.NET Core Module is configured with the `aspNetCore` section of the `system.webServer` node in the site's *web.config* file.</span></span>

<span data-ttu-id="c3971-109">O seguinte *Web. config* arquivo é publicado para uma [implantação dependentes de framework](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) e configura o módulo de núcleo do ASP.NET para manipular solicitações de site:</span><span class="sxs-lookup"><span data-stu-id="c3971-109">The following *web.config* file is published for a [framework-dependent deployment](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) and configures the ASP.NET Core Module to handle site requests:</span></span>

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

<span data-ttu-id="c3971-110">O seguinte *Web. config* é publicado para uma [implantação autossuficiente](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span><span class="sxs-lookup"><span data-stu-id="c3971-110">The following *web.config* is published for a [self-contained deployment](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span></span>

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

<span data-ttu-id="c3971-111">Quando um aplicativo é implantado em [do serviço de aplicativo do Azure](https://azure.microsoft.com/services/app-service/), o `stdoutLogFile` caminho é definido como `\\?\%home%\LogFiles\stdout`.</span><span class="sxs-lookup"><span data-stu-id="c3971-111">When an app is deployed to [Azure App Service](https://azure.microsoft.com/services/app-service/), the `stdoutLogFile` path is set to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="c3971-112">O caminho salva stdout logs para o *LogFiles* pasta, que é um local automaticamente criada pelo serviço.</span><span class="sxs-lookup"><span data-stu-id="c3971-112">The path saves stdout logs to the *LogFiles* folder, which is a location automatically created by the service.</span></span>

<span data-ttu-id="c3971-113">Consulte [configuração abaixo aplicativo](xref:host-and-deploy/iis/index#sub-application-configuration) para uma nota importante relativas à configuração do *Web. config* arquivos em subpastas de aplicativos.</span><span class="sxs-lookup"><span data-stu-id="c3971-113">See [Sub-application configuration](xref:host-and-deploy/iis/index#sub-application-configuration) for an important note pertaining to the configuration of *web.config* files in sub-apps.</span></span>

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="c3971-114">Atributos do elemento aspNetCore</span><span class="sxs-lookup"><span data-stu-id="c3971-114">Attributes of the aspNetCore element</span></span>

| <span data-ttu-id="c3971-115">Atributo</span><span class="sxs-lookup"><span data-stu-id="c3971-115">Attribute</span></span> | <span data-ttu-id="c3971-116">Descrição</span><span class="sxs-lookup"><span data-stu-id="c3971-116">Description</span></span> | <span data-ttu-id="c3971-117">Padrão</span><span class="sxs-lookup"><span data-stu-id="c3971-117">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="c3971-118">Atributo de cadeia de caracteres opcional.</span><span class="sxs-lookup"><span data-stu-id="c3971-118">Optional string attribute.</span></span></p><p><span data-ttu-id="c3971-119">Argumentos para o executável especificado em **processPath**.</span><span class="sxs-lookup"><span data-stu-id="c3971-119">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <span data-ttu-id="c3971-120">verdadeiro ou falso.</span><span class="sxs-lookup"><span data-stu-id="c3971-120">true or false.</span></span></p><p><span data-ttu-id="c3971-121">Se for true, o **502.5 - falha do processo** página é suprimida, e a página de código de 502 status configuradas no *Web. config* terá precedência.</span><span class="sxs-lookup"><span data-stu-id="c3971-121">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <span data-ttu-id="c3971-122">verdadeiro ou falso.</span><span class="sxs-lookup"><span data-stu-id="c3971-122">true or false.</span></span></p><p><span data-ttu-id="c3971-123">Se for true, o token é encaminhado para o processo filho escuta em % ASPNETCORE_PORT % como um cabeçalho 'MS-ASPNETCORE-WINAUTHTOKEN' por solicitação.</span><span class="sxs-lookup"><span data-stu-id="c3971-123">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="c3971-124">É responsabilidade do processo para chamar CloseHandle neste token por solicitação.</span><span class="sxs-lookup"><span data-stu-id="c3971-124">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processPath` | <p><span data-ttu-id="c3971-125">Atributo de cadeia de caracteres obrigatória.</span><span class="sxs-lookup"><span data-stu-id="c3971-125">Required string attribute.</span></span></p><p><span data-ttu-id="c3971-126">Caminho para o executável que inicia um processo que escuta solicitações HTTP.</span><span class="sxs-lookup"><span data-stu-id="c3971-126">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="c3971-127">Há suporte para caminhos relativos.</span><span class="sxs-lookup"><span data-stu-id="c3971-127">Relative paths are supported.</span></span> <span data-ttu-id="c3971-128">Se o caminho começa com `.`, o caminho é considerado em relação à raiz do site.</span><span class="sxs-lookup"><span data-stu-id="c3971-128">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="c3971-129">Atributo inteiro opcional.</span><span class="sxs-lookup"><span data-stu-id="c3971-129">Optional integer attribute.</span></span></p><p><span data-ttu-id="c3971-130">Especifica o número de vezes que o processo especificado na **processPath** pode falhar por minuto.</span><span class="sxs-lookup"><span data-stu-id="c3971-130">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="c3971-131">Se esse limite for excedido, o módulo para iniciar o processo para o restante do minuto.</span><span class="sxs-lookup"><span data-stu-id="c3971-131">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | `10` |
| `requestTimeout` | <p><span data-ttu-id="c3971-132">Atributo timespan opcional.</span><span class="sxs-lookup"><span data-stu-id="c3971-132">Optional timespan attribute.</span></span></p><p><span data-ttu-id="c3971-133">Especifica a duração para a qual o módulo do ASP.NET Core aguarda uma resposta do processo que escuta em % ASPNETCORE_PORT %.</span><span class="sxs-lookup"><span data-stu-id="c3971-133">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="c3971-134">O `requestTimeout` deve ser especificado em minutos inteiros, caso contrário, o padrão é 2 minutos.</span><span class="sxs-lookup"><span data-stu-id="c3971-134">The `requestTimeout` must be specified in whole minutes only, otherwise it defaults to 2 minutes.</span></span></p> | `00:02:00` |
| `shutdownTimeLimit` | <p><span data-ttu-id="c3971-135">Atributo inteiro opcional.</span><span class="sxs-lookup"><span data-stu-id="c3971-135">Optional integer attribute.</span></span></p><p><span data-ttu-id="c3971-136">Duração em segundos que o módulo espera para o executável para desligar normalmente quando o *app_offline.htm* arquivo é detectado.</span><span class="sxs-lookup"><span data-stu-id="c3971-136">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | `10` |
| `startupTimeLimit` | <p><span data-ttu-id="c3971-137">Atributo inteiro opcional.</span><span class="sxs-lookup"><span data-stu-id="c3971-137">Optional integer attribute.</span></span></p><p><span data-ttu-id="c3971-138">Duração em segundos que o módulo espera para o arquivo executável iniciar um processo que escuta na porta.</span><span class="sxs-lookup"><span data-stu-id="c3971-138">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="c3971-139">Se esse tempo limite for excedido, o módulo elimina o processo.</span><span class="sxs-lookup"><span data-stu-id="c3971-139">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="c3971-140">O módulo tentará reiniciar o processo quando ele recebe uma nova solicitação e continuará a tentar reiniciar o processo em solicitações subsequentes de entrada, a menos que o aplicativo não pode ser iniciado **rapidFailsPerMinute** vezes nos últimos minuto sem interrupção.</span><span class="sxs-lookup"><span data-stu-id="c3971-140">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p> | `120` |
| `stdoutLogEnabled` | <p><span data-ttu-id="c3971-141">Atributo booliano opcional.</span><span class="sxs-lookup"><span data-stu-id="c3971-141">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="c3971-142">Se true, **stdout** e **stderr** para o processo especificado na **processPath** são redirecionadas para o arquivo especificado em **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="c3971-142">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="c3971-143">Atributo de cadeia de caracteres opcional.</span><span class="sxs-lookup"><span data-stu-id="c3971-143">Optional string attribute.</span></span></p><p><span data-ttu-id="c3971-144">Especifica o caminho relativo ou absoluto para o qual **stdout** e **stderr** do processo especificado na **processPath** são registrados em log.</span><span class="sxs-lookup"><span data-stu-id="c3971-144">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="c3971-145">Caminhos relativos são relativo à raiz do site.</span><span class="sxs-lookup"><span data-stu-id="c3971-145">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="c3971-146">Qualquer caminho começando com `.` estão em relação ao site raiz e todos os outros caminhos são tratados como caminhos absolutos.</span><span class="sxs-lookup"><span data-stu-id="c3971-146">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="c3971-147">As pastas fornecidas no caminho devem existir para que o módulo criar o arquivo de log.</span><span class="sxs-lookup"><span data-stu-id="c3971-147">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="c3971-148">Usando os delimitadores de sublinhado, um carimbo de hora, ID de processo e extensão de arquivo (*. log*) são adicionados para o último segmento do **stdoutLogFile** caminho.</span><span class="sxs-lookup"><span data-stu-id="c3971-148">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="c3971-149">Se `.\logs\stdout` é fornecido como um valor, um log de exemplo stdout é salvo como *stdout_20180205194132_1934.log* no *logs* pasta quando salvos em 5/2/2018 às 19:41:32 com uma ID de processo de 1934.</span><span class="sxs-lookup"><span data-stu-id="c3971-149">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

### <a name="setting-environment-variables"></a><span data-ttu-id="c3971-150">Definir variáveis de ambiente</span><span class="sxs-lookup"><span data-stu-id="c3971-150">Setting environment variables</span></span>

<span data-ttu-id="c3971-151">Variáveis de ambiente podem ser especificadas para o processo no `processPath` atributo.</span><span class="sxs-lookup"><span data-stu-id="c3971-151">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="c3971-152">Especificar uma variável de ambiente com o `environmentVariable` elemento filho de um `environmentVariables` elemento da coleção.</span><span class="sxs-lookup"><span data-stu-id="c3971-152">Specify an environment variable with the `environmentVariable` child element of an `environmentVariables` collection element.</span></span> <span data-ttu-id="c3971-153">Variáveis de ambiente definidas nesta seção têm precedência sobre o sistema, variáveis de ambiente.</span><span class="sxs-lookup"><span data-stu-id="c3971-153">Environment variables set in this section take precedence over system environment variables.</span></span>

<span data-ttu-id="c3971-154">O exemplo a seguir define duas variáveis de ambiente.</span><span class="sxs-lookup"><span data-stu-id="c3971-154">The following example sets two environment variables.</span></span> <span data-ttu-id="c3971-155">`ASPNETCORE_ENVIRONMENT` configura o ambiente do aplicativo para `Development`.</span><span class="sxs-lookup"><span data-stu-id="c3971-155">`ASPNETCORE_ENVIRONMENT` configures the app's environment to `Development`.</span></span> <span data-ttu-id="c3971-156">Um desenvolvedor pode definir esse valor temporariamente *Web. config* arquivo para forçar o [página de exceção de desenvolvedor](xref:fundamentals/error-handling) carregar ao depurar uma exceção de aplicativo.</span><span class="sxs-lookup"><span data-stu-id="c3971-156">A developer may temporarily set this value in the *web.config* file in order to force the [Developer Exception Page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="c3971-157">`CONFIG_DIR` é um exemplo de uma variável de ambiente definidas pelo usuário, em que o desenvolvedor tenha gravado código que lê o valor de inicialização para formar um caminho para carregar o arquivo de configuração do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="c3971-157">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that reads the value on startup to form a path for loading the app's configuration file.</span></span>

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

> [!WARNING]
> <span data-ttu-id="c3971-158">Apenas definir o `ASPNETCORE_ENVIRONMENT` envirnonment variável `Development` no preparo e teste de servidores que não estão acessíveis a redes não confiáveis, como a Internet.</span><span class="sxs-lookup"><span data-stu-id="c3971-158">Only set the `ASPNETCORE_ENVIRONMENT` envirnonment variable to `Development` on staging and testing servers that aren't accessible to untrusted networks, such as the Internet.</span></span>

## <a name="appofflinehtm"></a><span data-ttu-id="c3971-159">app_offline.htm</span><span class="sxs-lookup"><span data-stu-id="c3971-159">app_offline.htm</span></span>

<span data-ttu-id="c3971-160">Se um arquivo com o nome *app_offline.htm* é detectado no diretório raiz de um aplicativo, o módulo do ASP.NET Core tenta desligar normalmente o aplicativo e parar o processamento de solicitações de entrada.</span><span class="sxs-lookup"><span data-stu-id="c3971-160">If a file with the name *app_offline.htm* is detected in the root directory of an app, the ASP.NET Core Module attempts to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="c3971-161">Se o aplicativo ainda está em execução após o número de segundos definido no `shutdownTimeLimit`, o módulo do ASP.NET Core elimina o processo em execução.</span><span class="sxs-lookup"><span data-stu-id="c3971-161">If the app is still running after the number of seconds defined in `shutdownTimeLimit`, the ASP.NET Core Module kills the running process.</span></span>

<span data-ttu-id="c3971-162">Enquanto o *app_offline.htm* arquivo estiver presente, o módulo do ASP.NET Core responde às solicitações enviando o conteúdo de *app_offline.htm* arquivo.</span><span class="sxs-lookup"><span data-stu-id="c3971-162">While the *app_offline.htm* file is present, the ASP.NET Core Module responds to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="c3971-163">Quando o *app_offline.htm* arquivo é removido, a próxima solicitação inicia o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="c3971-163">When the *app_offline.htm* file is removed, the next request starts the app.</span></span>

## <a name="start-up-error-page"></a><span data-ttu-id="c3971-164">Página de erro de inicialização</span><span class="sxs-lookup"><span data-stu-id="c3971-164">Start-up error page</span></span>

<span data-ttu-id="c3971-165">Se o módulo do ASP.NET Core falhar ao iniciar o processo de back-end ou o início do processo de back-end, mas não escutar na porta configurada, um *502.5 falha no processo* página de código de status será exibida.</span><span class="sxs-lookup"><span data-stu-id="c3971-165">If the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 Process Failure* status code page appears.</span></span> <span data-ttu-id="c3971-166">Para omitir esta página e reverter para a página de código de status de IIS 502 padrão, use o `disableStartUpErrorPage` atributo.</span><span class="sxs-lookup"><span data-stu-id="c3971-166">To suppress this page and revert to the default IIS 502 status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="c3971-167">Para obter mais informações sobre como configurar mensagens de erro personalizadas, consulte [erros HTTP `<httpErrors>` ](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="c3971-167">For more information on configuring custom error messages, see [HTTP Errors `<httpErrors>`](/iis/configuration/system.webServer/httpErrors/).</span></span>

![502.5 página de código de Status de falha do processo](aspnet-core-module/_static/ANCM-502_5.png)

## <a name="log-creation-and-redirection"></a><span data-ttu-id="c3971-169">Criação de log e de redirecionamento</span><span class="sxs-lookup"><span data-stu-id="c3971-169">Log creation and redirection</span></span>

<span data-ttu-id="c3971-170">O módulo do ASP.NET Core redireciona `stdout` e `stderr` logs no disco se o `stdoutLogEnabled` e `stdoutLogFile` atributos do `aspNetCore` elemento são definidas.</span><span class="sxs-lookup"><span data-stu-id="c3971-170">The ASP.NET Core Module redirects `stdout` and `stderr` logs to disk if the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element are set.</span></span> <span data-ttu-id="c3971-171">As pastas no `stdoutLogFile` caminho deve existir para que o módulo criar o arquivo de log.</span><span class="sxs-lookup"><span data-stu-id="c3971-171">Any folders in the `stdoutLogFile` path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="c3971-172">Uma extensão de arquivo e o carimbo de hora são adicionados automaticamente quando o arquivo de log é criado.</span><span class="sxs-lookup"><span data-stu-id="c3971-172">A timestamp and file extension are added automatically when the log file is created.</span></span> <span data-ttu-id="c3971-173">Logs não são girados, a menos que ocorra a reciclagem de processo/reinício.</span><span class="sxs-lookup"><span data-stu-id="c3971-173">Logs aren't rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="c3971-174">É responsabilidade do hoster para limitar o espaço em disco que consomem os logs.</span><span class="sxs-lookup"><span data-stu-id="c3971-174">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span> <span data-ttu-id="c3971-175">Usando o `stdout` log é recomendado apenas para solucionar problemas de inicialização do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="c3971-175">Using the `stdout` log is only recommended for troubleshooting app startup issues.</span></span> <span data-ttu-id="c3971-176">Não use o log de stdout para fins de registro do aplicativo geral.</span><span class="sxs-lookup"><span data-stu-id="c3971-176">Don't use the stdout log for general app logging purposes.</span></span> <span data-ttu-id="c3971-177">Para log de rotina no aplicativo do ASP.NET Core, use uma biblioteca de registro em log que limita o tamanho do arquivo de log e gira logs.</span><span class="sxs-lookup"><span data-stu-id="c3971-177">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="c3971-178">Para obter mais informações, consulte [provedores de log de terceiros](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="c3971-178">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

<span data-ttu-id="c3971-179">O nome do arquivo de log é composto por meio do acréscimo de carimbo de hora, a ID do processo e a extensão de arquivo (*. log*) para o último segmento do `stdoutLogFile` caminho (normalmente *stdout*) delimitados por sublinhados.</span><span class="sxs-lookup"><span data-stu-id="c3971-179">The log file name is composed by appending the timestamp, process ID, and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="c3971-180">Se o `stdoutLogFile` caminho termina com *stdout*, um log para um aplicativo com um PID de 1934 criada em 5/2/2018 às 19:42:32 tem o nome de arquivo *stdout_20180205194132_1934.log*.</span><span class="sxs-lookup"><span data-stu-id="c3971-180">If the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 1934 created on 2/5/2018 at 19:42:32 has the file name *stdout_20180205194132_1934.log*.</span></span>

<span data-ttu-id="c3971-181">O exemplo a seguir `aspNetCore` elemento configura `stdout` log para um aplicativo hospedado no serviço de aplicativo do Azure.</span><span class="sxs-lookup"><span data-stu-id="c3971-181">The following sample `aspNetCore` element configures `stdout` logging for an app hosted in Azure App Service.</span></span> <span data-ttu-id="c3971-182">Um caminho local ou um caminho de compartilhamento de rede é aceitável para o registro local.</span><span class="sxs-lookup"><span data-stu-id="c3971-182">A local path or network share path is acceptable for local logging.</span></span> <span data-ttu-id="c3971-183">Confirme se a identidade do usuário AppPool tem permissão para gravar o caminho fornecido.</span><span class="sxs-lookup"><span data-stu-id="c3971-183">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout">
</aspNetCore>
```

<span data-ttu-id="c3971-184">Consulte [configuração com a Web. config](#configuration-with-webconfig) para obter um exemplo de `aspNetCore` elemento no *Web. config* arquivo.</span><span class="sxs-lookup"><span data-stu-id="c3971-184">See [Configuration with web.config](#configuration-with-webconfig) for an example of the `aspNetCore` element in the *web.config* file.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="c3971-185">Configuração do módulo principal do ASP.NET com um IIS compartilhada</span><span class="sxs-lookup"><span data-stu-id="c3971-185">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="c3971-186">O instalador do módulo de núcleo do ASP.NET é executado com os privilégios do **sistema** conta.</span><span class="sxs-lookup"><span data-stu-id="c3971-186">The ASP.NET Core Module installer runs with the privileges of the **SYSTEM** account.</span></span> <span data-ttu-id="c3971-187">Como a conta sistema local não ter modificar permissões para o caminho do compartilhamento usado por configuração compartilhada de IIS, o instalador atinge um erro de acesso negado ao tentar definir as configurações de módulo em *applicationHost. config* no compartilhamento.</span><span class="sxs-lookup"><span data-stu-id="c3971-187">Because the local system account doesn't have modify permission for the share path used by the IIS Shared Configuration, the installer hits an access denied error when attempting to configure the module settings in *applicationHost.config* on the share.</span></span> <span data-ttu-id="c3971-188">Ao usar uma configuração compartilhada de IIS, siga estas etapas:</span><span class="sxs-lookup"><span data-stu-id="c3971-188">When using an IIS Shared Configuration, follow these steps:</span></span>

1. <span data-ttu-id="c3971-189">Desabilite a configuração compartilhada de IIS.</span><span class="sxs-lookup"><span data-stu-id="c3971-189">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="c3971-190">Execute o instalador.</span><span class="sxs-lookup"><span data-stu-id="c3971-190">Run the installer.</span></span>
1. <span data-ttu-id="c3971-191">Exportar atualizado *applicationHost. config* arquivo para o compartilhamento.</span><span class="sxs-lookup"><span data-stu-id="c3971-191">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="c3971-192">Habilite novamente a configuração compartilhada de IIS.</span><span class="sxs-lookup"><span data-stu-id="c3971-192">Re-enable the IIS Shared Configuration.</span></span>

## <a name="module-version-and-hosting-bundle-installer-logs"></a><span data-ttu-id="c3971-193">Versão do módulo e a hospedagem de logs do instalador do pacote</span><span class="sxs-lookup"><span data-stu-id="c3971-193">Module version and hosting bundle installer logs</span></span>

<span data-ttu-id="c3971-194">Para determinar a versão do ASP.NET Core módulo instalado:</span><span class="sxs-lookup"><span data-stu-id="c3971-194">To determine the version of the installed ASP.NET Core Module:</span></span>

1. <span data-ttu-id="c3971-195">No sistema de hospedagem, navegue até *%windir%\system32\inetsrv*.</span><span class="sxs-lookup"><span data-stu-id="c3971-195">On the hosting system, navigate to *%windir%\System32\inetsrv*.</span></span>
1. <span data-ttu-id="c3971-196">Localize o *aspnetcore.dll* arquivo.</span><span class="sxs-lookup"><span data-stu-id="c3971-196">Locate the *aspnetcore.dll* file.</span></span>
1. <span data-ttu-id="c3971-197">Clique no arquivo e selecione **propriedades** no menu contextual.</span><span class="sxs-lookup"><span data-stu-id="c3971-197">Right-click the file and select **Properties** from the contextual menu.</span></span>
1. <span data-ttu-id="c3971-198">Selecione o **detalhes** guia. O **versão do arquivo** e **versão do produto** representar a versão instalada do módulo.</span><span class="sxs-lookup"><span data-stu-id="c3971-198">Select the **Details** tab. The **File version** and **Product version** represent the installed version of the module.</span></span>

<span data-ttu-id="c3971-199">Os logs de instalador de pacote de hospedagem do Windows Server para o módulo são encontrados no *c:\\usuários\\% UserName %\\AppData\\Local\\Temp*. O arquivo é nomeado *dd_DotNetCoreWinSvrHosting__\<timestamp > _000_AspNetCoreModule_x64.log*.</span><span class="sxs-lookup"><span data-stu-id="c3971-199">The Windows Server Hosting bundle installer logs for the module are found at *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. The file is named *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="c3971-200">Locais de arquivo do módulo, o esquema e a configuração</span><span class="sxs-lookup"><span data-stu-id="c3971-200">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="c3971-201">Módulo</span><span class="sxs-lookup"><span data-stu-id="c3971-201">Module</span></span>

<span data-ttu-id="c3971-202">**IIS (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="c3971-202">**IIS (x86/amd64):**</span></span>

   * <span data-ttu-id="c3971-203">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="c3971-203">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

   * <span data-ttu-id="c3971-204">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="c3971-204">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

<span data-ttu-id="c3971-205">**O IIS Express (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="c3971-205">**IIS Express (x86/amd64):**</span></span>

   * <span data-ttu-id="c3971-206">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="c3971-206">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

   * <span data-ttu-id="c3971-207">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="c3971-207">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

### <a name="schema"></a><span data-ttu-id="c3971-208">Esquema</span><span class="sxs-lookup"><span data-stu-id="c3971-208">Schema</span></span>

<span data-ttu-id="c3971-209">**IIS**</span><span class="sxs-lookup"><span data-stu-id="c3971-209">**IIS**</span></span>

   * <span data-ttu-id="c3971-210">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="c3971-210">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

<span data-ttu-id="c3971-211">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="c3971-211">**IIS Express**</span></span>

   * <span data-ttu-id="c3971-212">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="c3971-212">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

### <a name="configuration"></a><span data-ttu-id="c3971-213">Configuração</span><span class="sxs-lookup"><span data-stu-id="c3971-213">Configuration</span></span>

<span data-ttu-id="c3971-214">**IIS**</span><span class="sxs-lookup"><span data-stu-id="c3971-214">**IIS**</span></span>

   * <span data-ttu-id="c3971-215">%windir%\System32\inetsrv\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="c3971-215">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="c3971-216">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="c3971-216">**IIS Express**</span></span>

   * <span data-ttu-id="c3971-217">.vs\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="c3971-217">.vs\config\applicationHost.config</span></span>

<span data-ttu-id="c3971-218">Os arquivos podem ser encontrados pesquisando *aspnetcore.dll* no *applicationHost. config* arquivo.</span><span class="sxs-lookup"><span data-stu-id="c3971-218">The files can be found by searching for *aspnetcore.dll* in the *applicationHost.config* file.</span></span> <span data-ttu-id="c3971-219">Para o IIS Express, o *applicationHost. config* arquivo não existe por padrão.</span><span class="sxs-lookup"><span data-stu-id="c3971-219">For IIS Express, the *applicationHost.config* file won't exist by default.</span></span> <span data-ttu-id="c3971-220">O arquivo é criado no  *\<application_root >\\VS\\config* ao iniciar qualquer projeto de aplicativo web na solução do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c3971-220">The file is created at *\<application_root>\\.vs\\config* when starting any web app project in the Visual Studio solution.</span></span>

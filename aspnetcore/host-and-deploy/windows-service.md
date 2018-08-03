---
title: Hospedar o ASP.NET Core em um serviço Windows
author: guardrex
description: Saiba como hospedar um aplicativo ASP.NET Core em um serviço Windows.
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/04/2018
uid: host-and-deploy/windows-service
ms.openlocfilehash: 4aded0b87ca14a5c09844cc378efb1ac0c12a289
ms.sourcegitcommit: 927e510d68f269d8335b5a7c8592621219a90965
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39342150"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="887e8-103">Hospedar o ASP.NET Core em um serviço Windows</span><span class="sxs-lookup"><span data-stu-id="887e8-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="887e8-104">Por [Luke Latham](https://github.com/guardrex) e [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="887e8-104">By [Luke Latham](https://github.com/guardrex) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="887e8-105">Um aplicativo ASP.NET Core pode ser hospedado no Windows sem usar o IIS como um [Serviço Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span><span class="sxs-lookup"><span data-stu-id="887e8-105">An ASP.NET Core app can be hosted on Windows without using IIS as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span></span> <span data-ttu-id="887e8-106">Quando hospedado como um serviço Windows, o aplicativo pode iniciar automaticamente após reinicializações e falhas, sem exigir intervenção humana.</span><span class="sxs-lookup"><span data-stu-id="887e8-106">When hosted as a Windows Service, the app can automatically start after reboots and crashes without requiring human intervention.</span></span>

<span data-ttu-id="887e8-107">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="887e8-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="get-started"></a><span data-ttu-id="887e8-108">Introdução</span><span class="sxs-lookup"><span data-stu-id="887e8-108">Get started</span></span>

<span data-ttu-id="887e8-109">As seguintes alterações mínimas são necessárias para configurar um projeto existente do ASP.NET Core para ser executado em um serviço:</span><span class="sxs-lookup"><span data-stu-id="887e8-109">The following minimum changes are required to set up an existing ASP.NET Core project to run in a service:</span></span>

1. <span data-ttu-id="887e8-110">No arquivo de projeto:</span><span class="sxs-lookup"><span data-stu-id="887e8-110">In the project file:</span></span>

   1. <span data-ttu-id="887e8-111">Confirme a presença do identificador de tempo de execução ou adicione-o ao **\<PropertyGroup>** que contém a estrutura de destino:</span><span class="sxs-lookup"><span data-stu-id="887e8-111">Confirm the presence of the runtime identifier or add it to the **\<PropertyGroup>** that contains the target framework:</span></span>

      ::: moniker range=">= aspnetcore-2.1"

      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp2.1</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```

      ::: moniker-end

      ::: moniker range="= aspnetcore-2.0"

      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp2.0</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```

      ::: moniker-end

      ::: moniker range="< aspnetcore-2.0"

      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp1.1</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```

      ::: moniker-end

   1. <span data-ttu-id="887e8-112">Adicionar uma referência de pacote para [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span><span class="sxs-lookup"><span data-stu-id="887e8-112">Add a package reference for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span></span>

1. <span data-ttu-id="887e8-113">Faça as seguintes alterações em `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="887e8-113">Make the following changes in `Program.Main`:</span></span>

   * <span data-ttu-id="887e8-114">Chame [host. RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice), em vez de `host.Run`.</span><span class="sxs-lookup"><span data-stu-id="887e8-114">Call [host.RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) instead of `host.Run`.</span></span>

   * <span data-ttu-id="887e8-115">Chame [UseContentRoot](xref:fundamentals/host/web-host#content-root) e use um caminho para o local publicado do aplicativo, em vez de `Directory.GetCurrentDirectory()`.</span><span class="sxs-lookup"><span data-stu-id="887e8-115">Call [UseContentRoot](xref:fundamentals/host/web-host#content-root) and use a path to the app's published location instead of `Directory.GetCurrentDirectory()`.</span></span>

     ::: moniker range=">= aspnetcore-2.0"

     [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOnly&highlight=8-9,12)]

     ::: moniker-end

     ::: moniker range="< aspnetcore-2.0"

     [!code-csharp[](windows-service/samples_snapshot/1.x/AspNetCoreService/Program.cs?name=ServiceOnly&highlight=3-4,8,13)]

     ::: moniker-end

1. <span data-ttu-id="887e8-116">Publique o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="887e8-116">Publish the app.</span></span> <span data-ttu-id="887e8-117">Use [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) ou um [perfil de publicação do Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles).</span><span class="sxs-lookup"><span data-stu-id="887e8-117">Use [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) or a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles).</span></span> <span data-ttu-id="887e8-118">Ao usar o Visual Studio, selecione **FolderProfile**.</span><span class="sxs-lookup"><span data-stu-id="887e8-118">When using a Visual Studio, select the **FolderProfile**.</span></span>

   <span data-ttu-id="887e8-119">Para publicar o aplicativo de exemplo da linha de comando, execute o seguinte comando em uma janela do console da pasta de projeto:</span><span class="sxs-lookup"><span data-stu-id="887e8-119">To publish the sample app from the command line, run the following command in a console window from the project folder:</span></span>

   ```console
   dotnet publish --configuration Release
   ```

1. <span data-ttu-id="887e8-120">Use a ferramenta de linha de comando [sc.exe](https://technet.microsoft.com/library/bb490995) para criar o serviço.</span><span class="sxs-lookup"><span data-stu-id="887e8-120">Use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create the service.</span></span> <span data-ttu-id="887e8-121">O valor `binPath` é o caminho para o executável do aplicativo, que inclui o nome do arquivo executável.</span><span class="sxs-lookup"><span data-stu-id="887e8-121">The `binPath` value is the path to the app's executable, which includes the executable file name.</span></span> <span data-ttu-id="887e8-122">**O espaço entre o sinal de igual e o caractere de aspas no início do caminho é necessário.**</span><span class="sxs-lookup"><span data-stu-id="887e8-122">**The space between the equal sign and the quote character at the start of the path is required.**</span></span>

   ```console
   sc create <SERVICE_NAME> binPath= "<PATH_TO_SERVICE_EXECUTABLE>"
   ```

   <span data-ttu-id="887e8-123">Para um serviço publicado na pasta do projeto, use o caminho para a pasta *publish* para criar o serviço.</span><span class="sxs-lookup"><span data-stu-id="887e8-123">For a service published in the project folder, use the path to the *publish* folder to create the service.</span></span> <span data-ttu-id="887e8-124">No exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="887e8-124">In the following example:</span></span>

   * <span data-ttu-id="887e8-125">O projeto está na pasta `c:\my_services\AspNetCoreService`.</span><span class="sxs-lookup"><span data-stu-id="887e8-125">The project resides in the `c:\my_services\AspNetCoreService` folder.</span></span>
   * <span data-ttu-id="887e8-126">O projeto é publicado na configuração `Release`.</span><span class="sxs-lookup"><span data-stu-id="887e8-126">The project is published in `Release` configuration.</span></span>
   * <span data-ttu-id="887e8-127">O TFM (Moniker da Estrutura de Destino) é `netcoreapp2.1`.</span><span class="sxs-lookup"><span data-stu-id="887e8-127">The Target Framework Moniker (TFM) is `netcoreapp2.1`.</span></span>
   * <span data-ttu-id="887e8-128">O RID (Identificador de Tempo de Execução) é `win7-x64`.</span><span class="sxs-lookup"><span data-stu-id="887e8-128">The Runtime Identifer (RID) is `win7-x64`.</span></span>
   * <span data-ttu-id="887e8-129">O nome do executável de aplicativo é *AspNetCoreService.exe*.</span><span class="sxs-lookup"><span data-stu-id="887e8-129">The app executable is named *AspNetCoreService.exe*.</span></span>
   * <span data-ttu-id="887e8-130">O nome do serviço é **MyService**.</span><span class="sxs-lookup"><span data-stu-id="887e8-130">The service is named **MyService**.</span></span>

   <span data-ttu-id="887e8-131">Exemplo:</span><span class="sxs-lookup"><span data-stu-id="887e8-131">Example:</span></span>

   ```console
   sc create MyService binPath= "c:\my_services\AspNetCoreService\bin\Release\netcoreapp2.1\win7-x64\publish\AspNetCoreService.exe"
   ```
   
   > [!IMPORTANT]
   > <span data-ttu-id="887e8-132">Verifique se existe o espaço entre o argumento `binPath=` e seu valor.</span><span class="sxs-lookup"><span data-stu-id="887e8-132">Make sure the space is present between the `binPath=` argument and its value.</span></span>
   
   <span data-ttu-id="887e8-133">Para publicar e iniciar o serviço de uma pasta diferente:</span><span class="sxs-lookup"><span data-stu-id="887e8-133">To publish and start the service from a different folder:</span></span>
   
      1. <span data-ttu-id="887e8-134">Use a opção [--output &lt;OUTPUT_DIRECTORY&gt;](/dotnet/core/tools/dotnet-publish#options) no comando `dotnet publish`.</span><span class="sxs-lookup"><span data-stu-id="887e8-134">Use the [--output &lt;OUTPUT_DIRECTORY&gt;](/dotnet/core/tools/dotnet-publish#options) option on the `dotnet publish` command.</span></span> <span data-ttu-id="887e8-135">Se você usar o Visual Studio, configure o **Local de Destino** na página de propriedades da publicação **FolderProfile** antes de selecionar o botão **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="887e8-135">If using Visual Studio, configure the **Target Location** in the **FolderProfile** publish property page before selecting the **Publish** button.</span></span>
   1. <span data-ttu-id="887e8-136">Crie o serviço com o comando `sc.exe` usando o caminho da pasta de saída.</span><span class="sxs-lookup"><span data-stu-id="887e8-136">Create the service with the `sc.exe` command using the output folder path.</span></span> <span data-ttu-id="887e8-137">Inclua o nome do executável do serviço no caminho fornecido para `binPath`.</span><span class="sxs-lookup"><span data-stu-id="887e8-137">Include the name of the service's executable in the path provided to `binPath`.</span></span>

1. <span data-ttu-id="887e8-138">Inicie o serviço com o comando `sc start <SERVICE_NAME>`.</span><span class="sxs-lookup"><span data-stu-id="887e8-138">Start the service with the `sc start <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="887e8-139">Para iniciar o serviço de aplicativo de exemplo, use o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="887e8-139">To start the sample app service, use the following command:</span></span>

   ```console
   sc start MyService
   ```

   <span data-ttu-id="887e8-140">O comando leva alguns segundos para iniciar o serviço.</span><span class="sxs-lookup"><span data-stu-id="887e8-140">The command takes a few seconds to start the service.</span></span>

1. <span data-ttu-id="887e8-141">O comando `sc query <SERVICE_NAME>` pode ser usado para verificar o status do serviço para determinar seu status:</span><span class="sxs-lookup"><span data-stu-id="887e8-141">The `sc query <SERVICE_NAME>` command can be used to check the status of the service to determine its status:</span></span>

   * `START_PENDING`
   * `RUNNING`
   * `STOP_PENDING`
   * `STOPPED`

   <span data-ttu-id="887e8-142">Use o seguinte comando para verificar o status do serviço de aplicativo de exemplo:</span><span class="sxs-lookup"><span data-stu-id="887e8-142">Use the following command to check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

1. <span data-ttu-id="887e8-143">Quando o serviço estiver no estado `RUNNING` e se o serviço for um aplicativo Web, procure o aplicativo em seu caminho (por padrão, `http://localhost:5000`, que redireciona para `https://localhost:5001` ao usar [Middleware de Redirecionamento HTTPS](xref:security/enforcing-ssl)).</span><span class="sxs-lookup"><span data-stu-id="887e8-143">When the service is in the `RUNNING` state and if the service is a web app, browse the app at its path (by default, `http://localhost:5000`, which redirects to `https://localhost:5001` when using [HTTPS Redirection Middleware](xref:security/enforcing-ssl)).</span></span>

   <span data-ttu-id="887e8-144">Para o serviço de aplicativo de exemplo, procure o aplicativo em `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="887e8-144">For the sample app service, browse the app at `http://localhost:5000`.</span></span>

1. <span data-ttu-id="887e8-145">Interrompa o serviço com o comando `sc stop <SERVICE_NAME>`.</span><span class="sxs-lookup"><span data-stu-id="887e8-145">Stop the service with the `sc stop <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="887e8-146">O comando a seguir interrompe o serviço de aplicativo de exemplo:</span><span class="sxs-lookup"><span data-stu-id="887e8-146">The following command stops the sample app service:</span></span>

   ```console
   sc stop MyService
   ```

1. <span data-ttu-id="887e8-147">Após um pequeno atraso para interromper um serviço, desinstale o serviço com o comando `sc delete <SERVICE_NAME>`.</span><span class="sxs-lookup"><span data-stu-id="887e8-147">After a short delay to stop a service, uninstall the service with the `sc delete <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="887e8-148">Verifique o status do serviço de aplicativo de exemplo:</span><span class="sxs-lookup"><span data-stu-id="887e8-148">Check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

   <span data-ttu-id="887e8-149">Quando o serviço de aplicativo de exemplo estiver no estado `STOPPED`, use o seguinte comando para desinstalar o serviço de aplicativo de exemplo:</span><span class="sxs-lookup"><span data-stu-id="887e8-149">When the sample app service is in the `STOPPED` state, use the following command to uninstall the sample app service:</span></span>

   ```console
   sc delete MyService
   ```

## <a name="provide-a-way-to-run-outside-of-a-service"></a><span data-ttu-id="887e8-150">Fornecer uma maneira de executar fora de um serviço</span><span class="sxs-lookup"><span data-stu-id="887e8-150">Provide a way to run outside of a service</span></span>

<span data-ttu-id="887e8-151">É mais fácil testar e depurar ao executar fora de um serviço, então é comum adicionar código que chama `RunAsService` apenas em determinadas condições.</span><span class="sxs-lookup"><span data-stu-id="887e8-151">It's easier to test and debug when running outside of a service, so it's customary to add code that calls `RunAsService` only under certain conditions.</span></span> <span data-ttu-id="887e8-152">Por exemplo, o aplicativo pode ser executado como um aplicativo de console com um argumento de linha de comando `--console` ou se o depurador está anexado:</span><span class="sxs-lookup"><span data-stu-id="887e8-152">For example, the app can run as a console app with a `--console` command-line argument or if the debugger is attached:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOrConsole)]

<span data-ttu-id="887e8-153">Porque a configuração do ASP.NET Core requer pares nome-valor para argumentos de linha de comando, a opção `--console` é removida antes que os argumentos sejam passados para [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span><span class="sxs-lookup"><span data-stu-id="887e8-153">Because ASP.NET Core configuration requires name-value pairs for command-line arguments, the `--console` switch is removed before the arguments are passed to [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span></span>

> [!NOTE]
> <span data-ttu-id="887e8-154">`isService` não é passado do `Main` para o `CreateWebHostBuilder`, porque a assinatura do `CreateWebHostBuilder` deve ser `CreateWebHostBuilder(string[])` para que o [teste de integração](xref:test/integration-tests) funcione corretamente.</span><span class="sxs-lookup"><span data-stu-id="887e8-154">`isService` isn't passed from `Main` into `CreateWebHostBuilder` because the signature of `CreateWebHostBuilder` must be `CreateWebHostBuilder(string[])` in order for [integration testing](xref:test/integration-tests) to work properly.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](windows-service/samples_snapshot/1.x/AspNetCoreService/Program.cs?name=ServiceOrConsole)]

::: moniker-end

## <a name="handle-stopping-and-starting-events"></a><span data-ttu-id="887e8-155">Manipular eventos de início e de parada</span><span class="sxs-lookup"><span data-stu-id="887e8-155">Handle stopping and starting events</span></span>

<span data-ttu-id="887e8-156">Para tratar eventos [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted) e [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping), faça as seguintes alterações adicionais:</span><span class="sxs-lookup"><span data-stu-id="887e8-156">To handle [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted), and [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping) events, make the following additional changes:</span></span>

1. <span data-ttu-id="887e8-157">Crie uma classe que derive de [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice):</span><span class="sxs-lookup"><span data-stu-id="887e8-157">Create a class that derives from [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice):</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=NoLogging)]

2. <span data-ttu-id="887e8-158">Crie um método de extensão para [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) que passe o `WebHostService` personalizado para [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run):</span><span class="sxs-lookup"><span data-stu-id="887e8-158">Create an extension method for [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) that passes the custom `WebHostService` to [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run):</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="887e8-159">Em `Program.Main`, chame o novo método de extensão, `RunAsCustomService`, em vez de [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice):</span><span class="sxs-lookup"><span data-stu-id="887e8-159">In `Program.Main`, call the new extension method, `RunAsCustomService`, instead of [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice):</span></span>

   ::: moniker range=">= aspnetcore-2.0"

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=HandleStopStart&highlight=14)]

   > [!NOTE]
   > <span data-ttu-id="887e8-160">`isService` não é passado do `Main` para o `CreateWebHostBuilder`, porque a assinatura do `CreateWebHostBuilder` deve ser `CreateWebHostBuilder(string[])` para que o [teste de integração](xref:test/integration-tests) funcione corretamente.</span><span class="sxs-lookup"><span data-stu-id="887e8-160">`isService` isn't passed from `Main` into `CreateWebHostBuilder` because the signature of `CreateWebHostBuilder` must be `CreateWebHostBuilder(string[])` in order for [integration testing](xref:test/integration-tests) to work properly.</span></span>

   ::: moniker-end

   ::: moniker range="< aspnetcore-2.0"

   [!code-csharp[](windows-service/samples_snapshot/1.x/AspNetCoreService/Program.cs?name=HandleStopStart&highlight=27)]

   ::: moniker-end

<span data-ttu-id="887e8-161">Se o código `WebHostService` personalizado exigir um serviço de injeção de dependência (como um agente), obtenha-o da propriedade [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services):</span><span class="sxs-lookup"><span data-stu-id="887e8-161">If the custom `WebHostService` code requires a service from dependency injection (such as a logger), obtain it from the [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services) property:</span></span>

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=Logging&highlight=7-8)]

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="887e8-162">Servidor proxy e cenários de balanceador de carga</span><span class="sxs-lookup"><span data-stu-id="887e8-162">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="887e8-163">Serviços que interagem com solicitações da Internet ou de uma rede corporativa e estão atrás de um proxy ou de um balanceador de carga podem exigir configuração adicional.</span><span class="sxs-lookup"><span data-stu-id="887e8-163">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="887e8-164">Para obter mais informações, consulte <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="887e8-164">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="887e8-165">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="887e8-165">Additional resources</span></span>

* <span data-ttu-id="887e8-166">[Configuração do ponto de extremidade Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) (inclui a configuração HTTPS e suporte à SNI)</span><span class="sxs-lookup"><span data-stu-id="887e8-166">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/web-host>

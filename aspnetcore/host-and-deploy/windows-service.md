---
title: Hospedar o ASP.NET Core em um serviço Windows
author: guardrex
description: Saiba como hospedar um aplicativo ASP.NET Core em um serviço Windows.
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/04/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/windows-service
ms.openlocfilehash: 5eba685bbe55d43bb063a01798bc691a1ba0d6fc
ms.sourcegitcommit: 4e3497bda0c3e5011ffba3717eb61a1d46c61c15
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/14/2018
ms.locfileid: "35613080"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="0abed-103">Hospedar o ASP.NET Core em um serviço Windows</span><span class="sxs-lookup"><span data-stu-id="0abed-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="0abed-104">Por [Luke Latham](https://github.com/guardrex) e [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="0abed-104">By [Luke Latham](https://github.com/guardrex) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="0abed-105">Um aplicativo ASP.NET Core pode ser hospedado no Windows sem usar o IIS como um [Serviço Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span><span class="sxs-lookup"><span data-stu-id="0abed-105">An ASP.NET Core app can be hosted on Windows without using IIS as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span></span> <span data-ttu-id="0abed-106">Quando hospedado como um serviço Windows, o aplicativo pode iniciar automaticamente após reinicializações e falhas, sem exigir intervenção humana.</span><span class="sxs-lookup"><span data-stu-id="0abed-106">When hosted as a Windows Service, the app can automatically start after reboots and crashes without requiring human intervention.</span></span>

<span data-ttu-id="0abed-107">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="0abed-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="get-started"></a><span data-ttu-id="0abed-108">Introdução</span><span class="sxs-lookup"><span data-stu-id="0abed-108">Get started</span></span>

<span data-ttu-id="0abed-109">As seguintes alterações mínimas são necessárias para configurar um projeto existente do ASP.NET Core para ser executado em um serviço:</span><span class="sxs-lookup"><span data-stu-id="0abed-109">The following minimum changes are required to set up an existing ASP.NET Core project to run in a service:</span></span>

1. <span data-ttu-id="0abed-110">No arquivo de projeto:</span><span class="sxs-lookup"><span data-stu-id="0abed-110">In the project file:</span></span>

   1. <span data-ttu-id="0abed-111">Confirme a presença do identificador de tempo de execução ou adicione-o ao **\<PropertyGroup>** que contém a estrutura de destino:</span><span class="sxs-lookup"><span data-stu-id="0abed-111">Confirm the presence of the runtime identifier or add it to the **\<PropertyGroup>** that contains the target framework:</span></span>
      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp2.1</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```
   1. <span data-ttu-id="0abed-112">Adicionar uma referência de pacote para [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span><span class="sxs-lookup"><span data-stu-id="0abed-112">Add a package reference for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span></span>

1. <span data-ttu-id="0abed-113">Faça as seguintes alterações em `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="0abed-113">Make the following changes in `Program.Main`:</span></span>

   * <span data-ttu-id="0abed-114">Chame [host. RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice), em vez de `host.Run`.</span><span class="sxs-lookup"><span data-stu-id="0abed-114">Call [host.RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) instead of `host.Run`.</span></span>

   * <span data-ttu-id="0abed-115">Se o código chama `UseContentRoot`, use um caminho para o local publicado do aplicativo, em vez de `Directory.GetCurrentDirectory()`.</span><span class="sxs-lookup"><span data-stu-id="0abed-115">If the code calls `UseContentRoot`, use a path to the app's published location instead of `Directory.GetCurrentDirectory()`.</span></span>

     ::: moniker range=">= aspnetcore-2.0"

     <span data-ttu-id="0abed-116">[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,7,11)]</span><span class="sxs-lookup"><span data-stu-id="0abed-116">[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,7,11)]</span></span>

     ::: moniker-end

     ::: moniker range="< aspnetcore-2.0"

     <span data-ttu-id="0abed-117">[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOnly&highlight=3-4,8,13)]</span><span class="sxs-lookup"><span data-stu-id="0abed-117">[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOnly&highlight=3-4,8,13)]</span></span>

     ::: moniker-end

1. <span data-ttu-id="0abed-118">Publique o aplicativo em uma pasta.</span><span class="sxs-lookup"><span data-stu-id="0abed-118">Publish the app to a folder.</span></span> <span data-ttu-id="0abed-119">Use [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) ou um [perfil de publicação do Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles) que publica para uma pasta.</span><span class="sxs-lookup"><span data-stu-id="0abed-119">Use [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) or a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles) that publishes to a folder.</span></span>

   <span data-ttu-id="0abed-120">Para publicar o aplicativo de exemplo da linha de comando, execute o seguinte comando em uma janela do console da pasta de projeto:</span><span class="sxs-lookup"><span data-stu-id="0abed-120">To publish the sample app from the command line, run the following command in a console window from the project folder:</span></span>

   ```console
   dotnet publish --configuration Release --output c:\svc
   ```

1. <span data-ttu-id="0abed-121">Use a ferramenta de linha de comando [sc.exe](https://technet.microsoft.com/library/bb490995) para criar o serviço (`sc create <SERVICE_NAME> binPath= "<PATH_TO_SERVICE_EXECUTABLE>"`).</span><span class="sxs-lookup"><span data-stu-id="0abed-121">Use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create the service (`sc create <SERVICE_NAME> binPath= "<PATH_TO_SERVICE_EXECUTABLE>"`).</span></span> <span data-ttu-id="0abed-122">O valor `binPath` é o caminho para o executável do aplicativo, que inclui o nome do arquivo executável.</span><span class="sxs-lookup"><span data-stu-id="0abed-122">The `binPath` value is the path to the app's executable, which includes the executable file name.</span></span> <span data-ttu-id="0abed-123">**O espaço entre o sinal de igual e o caractere de aspas que inicia o caminho é necessário.**</span><span class="sxs-lookup"><span data-stu-id="0abed-123">**The space between the equal sign and the quote character that starts the path is required.**</span></span>

   <span data-ttu-id="0abed-124">Para o aplicativo de exemplo e o comando que se segue, o serviço é:</span><span class="sxs-lookup"><span data-stu-id="0abed-124">For the sample app and command that follows, the service is:</span></span>

   * <span data-ttu-id="0abed-125">Chamado **MyService**.</span><span class="sxs-lookup"><span data-stu-id="0abed-125">Named **MyService**.</span></span>
   * <span data-ttu-id="0abed-126">Publicado na pasta *c:\\svc*.</span><span class="sxs-lookup"><span data-stu-id="0abed-126">Published to *c:\\svc* folder.</span></span>
   * <span data-ttu-id="0abed-127">Tem um aplicativo executável chamado *AspNetCoreService.exe*.</span><span class="sxs-lookup"><span data-stu-id="0abed-127">Has an app executable named *AspNetCoreService.exe*.</span></span>

   <span data-ttu-id="0abed-128">Abra um shell de comando com privilégios administrativos e execute o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="0abed-128">Open a command shell with administrative privileges and run the following command:</span></span>

   ```console
   sc create MyService binPath= "c:\svc\aspnetcoreservice.exe"
   ```

   <span data-ttu-id="0abed-129">**Verifique se existe o espaço entre o argumento `binPath=` e seu valor.**</span><span class="sxs-lookup"><span data-stu-id="0abed-129">**Make sure the space is present between the `binPath=` argument and its value.**</span></span>

1. <span data-ttu-id="0abed-130">Inicie o serviço com o comando `sc start <SERVICE_NAME>`.</span><span class="sxs-lookup"><span data-stu-id="0abed-130">Start the service with the `sc start <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="0abed-131">Para iniciar o serviço de aplicativo de exemplo, use o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="0abed-131">To start the sample app service, use the following command:</span></span>

   ```console
   sc start MyService
   ```

   <span data-ttu-id="0abed-132">O comando leva alguns segundos para iniciar o serviço.</span><span class="sxs-lookup"><span data-stu-id="0abed-132">The command takes a few seconds to start the service.</span></span>

1. <span data-ttu-id="0abed-133">O comando `sc query <SERVICE_NAME>` pode ser usado para verificar o status do serviço para determinar seu status:</span><span class="sxs-lookup"><span data-stu-id="0abed-133">The `sc query <SERVICE_NAME>` command can be used to check the status of the service to determine its status:</span></span>

   * `START_PENDING`
   * `RUNNING`
   * `STOP_PENDING`
   * `STOPPED`

   <span data-ttu-id="0abed-134">Use o seguinte comando para verificar o status do serviço de aplicativo de exemplo:</span><span class="sxs-lookup"><span data-stu-id="0abed-134">Use the following command to check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

1. <span data-ttu-id="0abed-135">Quando o serviço estiver no estado `RUNNING` e se o serviço for um aplicativo Web, procure o aplicativo em seu caminho (por padrão, `http://localhost:5000`, que redireciona para `https://localhost:5001` ao usar [Middleware de Redirecionamento HTTPS](xref:security/enforcing-ssl)).</span><span class="sxs-lookup"><span data-stu-id="0abed-135">When the service is in the `RUNNING` state and if the service is a web app, browse the app at its path (by default, `http://localhost:5000`, which redirects to `https://localhost:5001` when using [HTTPS Redirection Middleware](xref:security/enforcing-ssl)).</span></span>

   <span data-ttu-id="0abed-136">Para o serviço de aplicativo de exemplo, procure o aplicativo em `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="0abed-136">For the sample app service, browse the app at `http://localhost:5000`.</span></span>

1. <span data-ttu-id="0abed-137">Interrompa o serviço com o comando `sc stop <SERVICE_NAME>`.</span><span class="sxs-lookup"><span data-stu-id="0abed-137">Stop the service with the `sc stop <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="0abed-138">O comando a seguir interrompe o serviço de aplicativo de exemplo:</span><span class="sxs-lookup"><span data-stu-id="0abed-138">The following command stops the sample app service:</span></span>

   ```console
   sc stop MyService
   ```

1. <span data-ttu-id="0abed-139">Após um pequeno atraso para interromper um serviço, desinstale o serviço com o comando `sc delete <SERVICE_NAME>`.</span><span class="sxs-lookup"><span data-stu-id="0abed-139">After a short delay to stop a service, uninstall the service with the `sc delete <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="0abed-140">Verifique o status do serviço de aplicativo de exemplo:</span><span class="sxs-lookup"><span data-stu-id="0abed-140">Check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

   <span data-ttu-id="0abed-141">Quando o serviço de aplicativo de exemplo estiver no estado `STOPPED`, use o seguinte comando para desinstalar o serviço de aplicativo de exemplo:</span><span class="sxs-lookup"><span data-stu-id="0abed-141">When the sample app service is in the `STOPPED` state, use the following command to uninstall the sample app service:</span></span>

   ```console
   sc delete MyService
   ```

## <a name="provide-a-way-to-run-outside-of-a-service"></a><span data-ttu-id="0abed-142">Fornecer uma maneira de executar fora de um serviço</span><span class="sxs-lookup"><span data-stu-id="0abed-142">Provide a way to run outside of a service</span></span>

<span data-ttu-id="0abed-143">É mais fácil testar e depurar ao executar fora de um serviço, então é comum adicionar código que chama `RunAsService` apenas em determinadas condições.</span><span class="sxs-lookup"><span data-stu-id="0abed-143">It's easier to test and debug when running outside of a service, so it's customary to add code that calls `RunAsService` only under certain conditions.</span></span> <span data-ttu-id="0abed-144">Por exemplo, o aplicativo pode ser executado como um aplicativo de console com um argumento de linha de comando `--console` ou se o depurador está anexado:</span><span class="sxs-lookup"><span data-stu-id="0abed-144">For example, the app can run as a console app with a `--console` command-line argument or if the debugger is attached:</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="0abed-145">[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]</span><span class="sxs-lookup"><span data-stu-id="0abed-145">[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]</span></span>

<span data-ttu-id="0abed-146">Porque a configuração do ASP.NET Core requer pares nome-valor para argumentos de linha de comando, a opção `--console` é removida antes que os argumentos sejam passados para [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span><span class="sxs-lookup"><span data-stu-id="0abed-146">Because ASP.NET Core configuration requires name-value pairs for command-line arguments, the `--console` switch is removed before the arguments are passed to [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="0abed-147">[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOrConsole)]</span><span class="sxs-lookup"><span data-stu-id="0abed-147">[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOrConsole)]</span></span>

::: moniker-end

## <a name="handle-stopping-and-starting-events"></a><span data-ttu-id="0abed-148">Manipular eventos de início e de parada</span><span class="sxs-lookup"><span data-stu-id="0abed-148">Handle stopping and starting events</span></span>

<span data-ttu-id="0abed-149">Para tratar eventos [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted) e [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping), faça as seguintes alterações adicionais:</span><span class="sxs-lookup"><span data-stu-id="0abed-149">To handle [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted), and [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping) events, make the following additional changes:</span></span>

1. <span data-ttu-id="0abed-150">Crie uma classe que derive de [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice):</span><span class="sxs-lookup"><span data-stu-id="0abed-150">Create a class that derives from [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice):</span></span>

   [!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=NoLogging)]

2. <span data-ttu-id="0abed-151">Crie um método de extensão para [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) que passe o `WebHostService` personalizado para [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run):</span><span class="sxs-lookup"><span data-stu-id="0abed-151">Create an extension method for [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) that passes the custom `WebHostService` to [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run):</span></span>

   [!code-csharp[](windows-service/sample/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="0abed-152">Em `Program.Main`, chame o novo método de extensão, `RunAsCustomService`, em vez de [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice):</span><span class="sxs-lookup"><span data-stu-id="0abed-152">In `Program.Main`, call the new extension method, `RunAsCustomService`, instead of [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice):</span></span>

   ::: moniker range=">= aspnetcore-2.0"

   <span data-ttu-id="0abed-153">[!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=27)]</span><span class="sxs-lookup"><span data-stu-id="0abed-153">[!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=27)]</span></span>

   ::: moniker-end

   ::: moniker range="< aspnetcore-2.0"

   <span data-ttu-id="0abed-154">[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=HandleStopStart&highlight=27)]</span><span class="sxs-lookup"><span data-stu-id="0abed-154">[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=HandleStopStart&highlight=27)]</span></span>

   ::: moniker-end

<span data-ttu-id="0abed-155">Se o código `WebHostService` personalizado exigir um serviço de injeção de dependência (como um agente), obtenha-o da propriedade [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services):</span><span class="sxs-lookup"><span data-stu-id="0abed-155">If the custom `WebHostService` code requires a service from dependency injection (such as a logger), obtain it from the [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services) property:</span></span>

[!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=Logging&highlight=7)]

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="0abed-156">Servidor proxy e cenários de balanceador de carga</span><span class="sxs-lookup"><span data-stu-id="0abed-156">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="0abed-157">Serviços que interagem com solicitações da Internet ou de uma rede corporativa e estão atrás de um proxy ou de um balanceador de carga podem exigir configuração adicional.</span><span class="sxs-lookup"><span data-stu-id="0abed-157">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="0abed-158">Para obter mais informações, veja [Configurar o ASP.NET Core para trabalhar com servidores proxy e balanceadores de carga](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="0abed-158">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

## <a name="kestrel-endpoint-configuration"></a><span data-ttu-id="0abed-159">Configuração de ponto de extremidade do Kestrel</span><span class="sxs-lookup"><span data-stu-id="0abed-159">Kestrel endpoint configuration</span></span>

<span data-ttu-id="0abed-160">Para obter informações sobre a configuração de ponto de extremidade Kestrel, incluindo configuração do HTTPS e suporte a SNI, veja [configuração de ponto de extremidade Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="0abed-160">For information on Kestrel endpoint configuration, including HTTPS configuration and SNI support, see [Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).</span></span>

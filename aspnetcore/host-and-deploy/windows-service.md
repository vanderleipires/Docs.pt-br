---
title: Hospedar o ASP.NET Core em um serviço Windows
author: guardrex
description: Saiba como hospedar um aplicativo ASP.NET Core em um serviço Windows.
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/25/2018
uid: host-and-deploy/windows-service
ms.openlocfilehash: 7f19db0a1d12b904daff989bc969daf8d2302bfa
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/15/2018
ms.locfileid: "49325777"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="c8dd9-103">Hospedar o ASP.NET Core em um serviço Windows</span><span class="sxs-lookup"><span data-stu-id="c8dd9-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="c8dd9-104">Por [Luke Latham](https://github.com/guardrex) e [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="c8dd9-104">By [Luke Latham](https://github.com/guardrex) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="c8dd9-105">Um aplicativo ASP.NET Core pode ser hospedado no Windows sem usar o IIS como um [Serviço Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span><span class="sxs-lookup"><span data-stu-id="c8dd9-105">An ASP.NET Core app can be hosted on Windows without using IIS as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span></span> <span data-ttu-id="c8dd9-106">Quando hospedado como um Serviço Windows, o aplicativo é iniciado automaticamente após a reinicialização.</span><span class="sxs-lookup"><span data-stu-id="c8dd9-106">When hosted as a Windows Service, the app automatically starts after reboots.</span></span>

<span data-ttu-id="c8dd9-107">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c8dd9-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="convert-a-project-into-a-windows-service"></a><span data-ttu-id="c8dd9-108">Converter um projeto em um serviço Windows</span><span class="sxs-lookup"><span data-stu-id="c8dd9-108">Convert a project into a Windows Service</span></span>

<span data-ttu-id="c8dd9-109">As alterações mínimas a seguir são necessárias para configurar um projeto existente do ASP.NET Core a ser executado como um serviço:</span><span class="sxs-lookup"><span data-stu-id="c8dd9-109">The following minimum changes are required to set up an existing ASP.NET Core project to run as a service:</span></span>

1. <span data-ttu-id="c8dd9-110">No arquivo de projeto:</span><span class="sxs-lookup"><span data-stu-id="c8dd9-110">In the project file:</span></span>

   * <span data-ttu-id="c8dd9-111">Confirme a presença de um [RID (Identificador de Tempo de Execução)](/dotnet/core/rid-catalog) do Windows ou adicione-a ao `<PropertyGroup>` que contém a estrutura de destino:</span><span class="sxs-lookup"><span data-stu-id="c8dd9-111">Confirm the presence of a Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) or add it to the `<PropertyGroup>` that contains the target framework:</span></span>

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

      <span data-ttu-id="c8dd9-112">Para publicar para vários RIDs:</span><span class="sxs-lookup"><span data-stu-id="c8dd9-112">To publish for multiple RIDs:</span></span>

      * <span data-ttu-id="c8dd9-113">Forneça os RIDs em uma lista delimitada por ponto e vírgula.</span><span class="sxs-lookup"><span data-stu-id="c8dd9-113">Provide the RIDs in a semicolon-delimited list.</span></span>
      * <span data-ttu-id="c8dd9-114">Use o nome da propriedade `<RuntimeIdentifiers>` (plural).</span><span class="sxs-lookup"><span data-stu-id="c8dd9-114">Use the property name `<RuntimeIdentifiers>` (plural).</span></span>

      <span data-ttu-id="c8dd9-115">Para obter mais informações, consulte [Catálogo de RID do .NET Core](/dotnet/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="c8dd9-115">For more information, see [.NET Core RID Catalog](/dotnet/core/rid-catalog).</span></span>

   * <span data-ttu-id="c8dd9-116">Adicionar uma referência de pacote para [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices).</span><span class="sxs-lookup"><span data-stu-id="c8dd9-116">Add a package reference for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices).</span></span>

1. <span data-ttu-id="c8dd9-117">Faça as seguintes alterações em `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="c8dd9-117">Make the following changes in `Program.Main`:</span></span>

   * <span data-ttu-id="c8dd9-118">Chame [host. RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice), em vez de `host.Run`.</span><span class="sxs-lookup"><span data-stu-id="c8dd9-118">Call [host.RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) instead of `host.Run`.</span></span>

   * <span data-ttu-id="c8dd9-119">Chame [UseContentRoot](xref:fundamentals/host/web-host#content-root) e use um caminho para o local publicado do aplicativo, em vez de `Directory.GetCurrentDirectory()`.</span><span class="sxs-lookup"><span data-stu-id="c8dd9-119">Call [UseContentRoot](xref:fundamentals/host/web-host#content-root) and use a path to the app's published location instead of `Directory.GetCurrentDirectory()`.</span></span>

     ::: moniker range=">= aspnetcore-2.0"

     [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOnly&highlight=8-9,16)]

     ::: moniker-end

     ::: moniker range="< aspnetcore-2.0"

     [!code-csharp[](windows-service/samples_snapshot/1.x/AspNetCoreService/Program.cs?name=ServiceOnly&highlight=3-4,8,13)]

     ::: moniker-end

1. <span data-ttu-id="c8dd9-120">Publique o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="c8dd9-120">Publish the app.</span></span> <span data-ttu-id="c8dd9-121">Use [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) ou um [perfil de publicação do Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles).</span><span class="sxs-lookup"><span data-stu-id="c8dd9-121">Use [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) or a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles).</span></span> <span data-ttu-id="c8dd9-122">Ao usar o Visual Studio, selecione **FolderProfile**.</span><span class="sxs-lookup"><span data-stu-id="c8dd9-122">When using a Visual Studio, select the **FolderProfile**.</span></span>

   <span data-ttu-id="c8dd9-123">Para publicar o aplicativo de exemplo usando as ferramentas da CLI (interface de linha de comando), execute o comando [dotnet publish](/dotnet/core/tools/dotnet-publish) em um prompt de comando da pasta do projeto.</span><span class="sxs-lookup"><span data-stu-id="c8dd9-123">To publish the sample app using command-line interface (CLI) tools, run the [dotnet publish](/dotnet/core/tools/dotnet-publish) command at a command prompt from the project folder.</span></span> <span data-ttu-id="c8dd9-124">O RID deve ser especificado na propriedade `<RuntimeIdenfifier>` (ou `<RuntimeIdentifiers>`) do arquivo de projeto.</span><span class="sxs-lookup"><span data-stu-id="c8dd9-124">The RID must be specified in the `<RuntimeIdenfifier>` (or `<RuntimeIdentifiers>`) property of the project file.</span></span> <span data-ttu-id="c8dd9-125">No exemplo a seguir, o aplicativo é publicado na Configuração de versão para o tempo de execução `win7-x64`:</span><span class="sxs-lookup"><span data-stu-id="c8dd9-125">In the following example, the app is published in Release configuration for the `win7-x64` runtime:</span></span>

   ```console
   dotnet publish --configuration Release --runtime win7-x64
   ```

1. <span data-ttu-id="c8dd9-126">Use a ferramenta de linha de comando [sc.exe](https://technet.microsoft.com/library/bb490995) para criar o serviço.</span><span class="sxs-lookup"><span data-stu-id="c8dd9-126">Use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create the service.</span></span> <span data-ttu-id="c8dd9-127">O valor `binPath` é o caminho para o executável do aplicativo, que inclui o nome do arquivo executável.</span><span class="sxs-lookup"><span data-stu-id="c8dd9-127">The `binPath` value is the path to the app's executable, which includes the executable file name.</span></span> <span data-ttu-id="c8dd9-128">**O espaço entre o sinal de igual e o caractere de aspas no início do caminho é necessário.**</span><span class="sxs-lookup"><span data-stu-id="c8dd9-128">**The space between the equal sign and the quote character at the start of the path is required.**</span></span>

   ```console
   sc create <SERVICE_NAME> binPath= "<PATH_TO_SERVICE_EXECUTABLE>"
   ```

   <span data-ttu-id="c8dd9-129">Para um serviço publicado na pasta do projeto, use o caminho para a pasta *publish* para criar o serviço.</span><span class="sxs-lookup"><span data-stu-id="c8dd9-129">For a service published in the project folder, use the path to the *publish* folder to create the service.</span></span> <span data-ttu-id="c8dd9-130">No exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="c8dd9-130">In the following example:</span></span>

   * <span data-ttu-id="c8dd9-131">O projeto reside na pasta *c:\\my_services\\AspNetCoreService*.</span><span class="sxs-lookup"><span data-stu-id="c8dd9-131">The project resides in the *c:\\my_services\\AspNetCoreService* folder.</span></span>
   * <span data-ttu-id="c8dd9-132">O projeto é publicado na configuração `Release`.</span><span class="sxs-lookup"><span data-stu-id="c8dd9-132">The project is published in `Release` configuration.</span></span>
   * <span data-ttu-id="c8dd9-133">O TFM (Moniker da Estrutura de Destino) é `netcoreapp2.1`.</span><span class="sxs-lookup"><span data-stu-id="c8dd9-133">The Target Framework Moniker (TFM) is `netcoreapp2.1`.</span></span>
   * <span data-ttu-id="c8dd9-134">O RID (Identificador de Tempo de Execução) é `win7-x64`.</span><span class="sxs-lookup"><span data-stu-id="c8dd9-134">The Runtime Identifer (RID) is `win7-x64`.</span></span>
   * <span data-ttu-id="c8dd9-135">O nome do executável de aplicativo é *AspNetCoreService.exe*.</span><span class="sxs-lookup"><span data-stu-id="c8dd9-135">The app executable is named *AspNetCoreService.exe*.</span></span>
   * <span data-ttu-id="c8dd9-136">O nome do serviço é **MyService**.</span><span class="sxs-lookup"><span data-stu-id="c8dd9-136">The service is named **MyService**.</span></span>

   <span data-ttu-id="c8dd9-137">Exemplo:</span><span class="sxs-lookup"><span data-stu-id="c8dd9-137">Example:</span></span>

   ```console
   sc create MyService binPath= "c:\my_services\AspNetCoreService\bin\Release\netcoreapp2.1\win7-x64\publish\AspNetCoreService.exe"
   ```

   > [!IMPORTANT]
   > <span data-ttu-id="c8dd9-138">Verifique se existe o espaço entre o argumento `binPath=` e seu valor.</span><span class="sxs-lookup"><span data-stu-id="c8dd9-138">Make sure the space is present between the `binPath=` argument and its value.</span></span>

   <span data-ttu-id="c8dd9-139">Para publicar e iniciar o serviço de uma pasta diferente:</span><span class="sxs-lookup"><span data-stu-id="c8dd9-139">To publish and start the service from a different folder:</span></span>

      * <span data-ttu-id="c8dd9-140">Use a opção [--output &lt;OUTPUT_DIRECTORY&gt;](/dotnet/core/tools/dotnet-publish#options) no comando `dotnet publish`.</span><span class="sxs-lookup"><span data-stu-id="c8dd9-140">Use the [--output &lt;OUTPUT_DIRECTORY&gt;](/dotnet/core/tools/dotnet-publish#options) option on the `dotnet publish` command.</span></span> <span data-ttu-id="c8dd9-141">Se você usar o Visual Studio, configure o **Local de Destino** na página de propriedades da publicação **FolderProfile** antes de selecionar o botão **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="c8dd9-141">If using Visual Studio, configure the **Target Location** in the **FolderProfile** publish property page before selecting the **Publish** button.</span></span>
      * <span data-ttu-id="c8dd9-142">Crie o serviço com o comando `sc.exe` usando o caminho da pasta de saída.</span><span class="sxs-lookup"><span data-stu-id="c8dd9-142">Create the service with the `sc.exe` command using the output folder path.</span></span> <span data-ttu-id="c8dd9-143">Inclua o nome do executável do serviço no caminho fornecido para `binPath`.</span><span class="sxs-lookup"><span data-stu-id="c8dd9-143">Include the name of the service's executable in the path provided to `binPath`.</span></span>

1. <span data-ttu-id="c8dd9-144">Inicie o serviço com o comando `sc start <SERVICE_NAME>`.</span><span class="sxs-lookup"><span data-stu-id="c8dd9-144">Start the service with the `sc start <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="c8dd9-145">Para iniciar o serviço de aplicativo de exemplo, use o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="c8dd9-145">To start the sample app service, use the following command:</span></span>

   ```console
   sc start MyService
   ```

   <span data-ttu-id="c8dd9-146">O comando leva alguns segundos para iniciar o serviço.</span><span class="sxs-lookup"><span data-stu-id="c8dd9-146">The command takes a few seconds to start the service.</span></span>

1. <span data-ttu-id="c8dd9-147">Para verificar o status do serviço, use o comando `sc query <SERVICE_NAME>`.</span><span class="sxs-lookup"><span data-stu-id="c8dd9-147">To check the status of the service, use the `sc query <SERVICE_NAME>` command.</span></span> <span data-ttu-id="c8dd9-148">O status é relatado como um dos seguintes valores:</span><span class="sxs-lookup"><span data-stu-id="c8dd9-148">The status is reported as one of the following values:</span></span>

   * `START_PENDING`
   * `RUNNING`
   * `STOP_PENDING`
   * `STOPPED`

   <span data-ttu-id="c8dd9-149">Use o seguinte comando para verificar o status do serviço de aplicativo de exemplo:</span><span class="sxs-lookup"><span data-stu-id="c8dd9-149">Use the following command to check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

1. <span data-ttu-id="c8dd9-150">Quando o serviço estiver no estado `RUNNING` e se o serviço for um aplicativo Web, procure o aplicativo em seu caminho (por padrão, `http://localhost:5000`, que redireciona para `https://localhost:5001` ao usar [Middleware de Redirecionamento HTTPS](xref:security/enforcing-ssl)).</span><span class="sxs-lookup"><span data-stu-id="c8dd9-150">When the service is in the `RUNNING` state and if the service is a web app, browse the app at its path (by default, `http://localhost:5000`, which redirects to `https://localhost:5001` when using [HTTPS Redirection Middleware](xref:security/enforcing-ssl)).</span></span>

   <span data-ttu-id="c8dd9-151">Para o serviço de aplicativo de exemplo, procure o aplicativo em `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="c8dd9-151">For the sample app service, browse the app at `http://localhost:5000`.</span></span>

1. <span data-ttu-id="c8dd9-152">Interrompa o serviço com o comando `sc stop <SERVICE_NAME>`.</span><span class="sxs-lookup"><span data-stu-id="c8dd9-152">Stop the service with the `sc stop <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="c8dd9-153">O comando a seguir interrompe o serviço de aplicativo de exemplo:</span><span class="sxs-lookup"><span data-stu-id="c8dd9-153">The following command stops the sample app service:</span></span>

   ```console
   sc stop MyService
   ```

1. <span data-ttu-id="c8dd9-154">Após um pequeno atraso para interromper um serviço, desinstale o serviço com o comando `sc delete <SERVICE_NAME>`.</span><span class="sxs-lookup"><span data-stu-id="c8dd9-154">After a short delay to stop a service, uninstall the service with the `sc delete <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="c8dd9-155">Verifique o status do serviço de aplicativo de exemplo:</span><span class="sxs-lookup"><span data-stu-id="c8dd9-155">Check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

   <span data-ttu-id="c8dd9-156">Quando o serviço de aplicativo de exemplo estiver no estado `STOPPED`, use o seguinte comando para desinstalar o serviço de aplicativo de exemplo:</span><span class="sxs-lookup"><span data-stu-id="c8dd9-156">When the sample app service is in the `STOPPED` state, use the following command to uninstall the sample app service:</span></span>

   ```console
   sc delete MyService
   ```

## <a name="run-the-app-outside-of-a-service"></a><span data-ttu-id="c8dd9-157">Execute o aplicativo fora de um serviço</span><span class="sxs-lookup"><span data-stu-id="c8dd9-157">Run the app outside of a service</span></span>

<span data-ttu-id="c8dd9-158">É mais fácil testar e depurar ao executar fora de um serviço, então é comum adicionar código que chama `RunAsService` apenas em determinadas condições.</span><span class="sxs-lookup"><span data-stu-id="c8dd9-158">It's easier to test and debug when running outside of a service, so it's customary to add code that calls `RunAsService` only under certain conditions.</span></span> <span data-ttu-id="c8dd9-159">Por exemplo, o aplicativo pode ser executado como um aplicativo de console com um argumento de linha de comando `--console` ou se o depurador está anexado:</span><span class="sxs-lookup"><span data-stu-id="c8dd9-159">For example, the app can run as a console app with a `--console` command-line argument or if the debugger is attached:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOrConsole)]

<span data-ttu-id="c8dd9-160">Porque a configuração do ASP.NET Core requer pares nome-valor para argumentos de linha de comando, a opção `--console` é removida antes que os argumentos sejam passados para [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span><span class="sxs-lookup"><span data-stu-id="c8dd9-160">Because ASP.NET Core configuration requires name-value pairs for command-line arguments, the `--console` switch is removed before the arguments are passed to [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span></span>

> [!NOTE]
> <span data-ttu-id="c8dd9-161">`isService` não é passado do `Main` para o `CreateWebHostBuilder`, porque a assinatura do `CreateWebHostBuilder` deve ser `CreateWebHostBuilder(string[])` para que o [teste de integração](xref:test/integration-tests) funcione corretamente.</span><span class="sxs-lookup"><span data-stu-id="c8dd9-161">`isService` isn't passed from `Main` into `CreateWebHostBuilder` because the signature of `CreateWebHostBuilder` must be `CreateWebHostBuilder(string[])` in order for [integration testing](xref:test/integration-tests) to work properly.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](windows-service/samples_snapshot/1.x/AspNetCoreService/Program.cs?name=ServiceOrConsole)]

::: moniker-end

## <a name="handle-stopping-and-starting-events"></a><span data-ttu-id="c8dd9-162">Manipular eventos de início e de parada</span><span class="sxs-lookup"><span data-stu-id="c8dd9-162">Handle stopping and starting events</span></span>

<span data-ttu-id="c8dd9-163">Para tratar eventos [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted) e [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping), faça as seguintes alterações adicionais:</span><span class="sxs-lookup"><span data-stu-id="c8dd9-163">To handle [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted), and [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping) events, make the following additional changes:</span></span>

1. <span data-ttu-id="c8dd9-164">Crie uma classe que derive de [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice):</span><span class="sxs-lookup"><span data-stu-id="c8dd9-164">Create a class that derives from [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice):</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=NoLogging)]

2. <span data-ttu-id="c8dd9-165">Crie um método de extensão para [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) que passe o `WebHostService` personalizado para [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run):</span><span class="sxs-lookup"><span data-stu-id="c8dd9-165">Create an extension method for [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) that passes the custom `WebHostService` to [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run):</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="c8dd9-166">Em `Program.Main`, chame o novo método de extensão, `RunAsCustomService`, em vez de [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice):</span><span class="sxs-lookup"><span data-stu-id="c8dd9-166">In `Program.Main`, call the new extension method, `RunAsCustomService`, instead of [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice):</span></span>

   ::: moniker range=">= aspnetcore-2.0"

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=HandleStopStart&highlight=17)]

   > [!NOTE]
   > <span data-ttu-id="c8dd9-167">`isService` não é passado do `Main` para o `CreateWebHostBuilder`, porque a assinatura do `CreateWebHostBuilder` deve ser `CreateWebHostBuilder(string[])` para que o [teste de integração](xref:test/integration-tests) funcione corretamente.</span><span class="sxs-lookup"><span data-stu-id="c8dd9-167">`isService` isn't passed from `Main` into `CreateWebHostBuilder` because the signature of `CreateWebHostBuilder` must be `CreateWebHostBuilder(string[])` in order for [integration testing](xref:test/integration-tests) to work properly.</span></span>

   ::: moniker-end

   ::: moniker range="< aspnetcore-2.0"

   [!code-csharp[](windows-service/samples_snapshot/1.x/AspNetCoreService/Program.cs?name=HandleStopStart&highlight=27)]

   ::: moniker-end

<span data-ttu-id="c8dd9-168">Se o código `WebHostService` personalizado exigir um serviço de injeção de dependência (como um agente), obtenha-o da propriedade [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services):</span><span class="sxs-lookup"><span data-stu-id="c8dd9-168">If the custom `WebHostService` code requires a service from dependency injection (such as a logger), obtain it from the [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services) property:</span></span>

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=Logging&highlight=7-8)]

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="c8dd9-169">Servidor proxy e cenários de balanceador de carga</span><span class="sxs-lookup"><span data-stu-id="c8dd9-169">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="c8dd9-170">Serviços que interagem com solicitações da Internet ou de uma rede corporativa e estão atrás de um proxy ou de um balanceador de carga podem exigir configuração adicional.</span><span class="sxs-lookup"><span data-stu-id="c8dd9-170">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="c8dd9-171">Para obter mais informações, consulte <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="c8dd9-171">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

## <a name="configure-https"></a><span data-ttu-id="c8dd9-172">Configurar o HTTPS</span><span class="sxs-lookup"><span data-stu-id="c8dd9-172">Configure HTTPS</span></span>

<span data-ttu-id="c8dd9-173">Especifique uma [configuração do ponto de extremidade HTTPS do servidor Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="c8dd9-173">Specify a [Kestrel server HTTPS endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).</span></span>

## <a name="current-directory-and-content-root"></a><span data-ttu-id="c8dd9-174">Diretório atual e a raiz do conteúdo</span><span class="sxs-lookup"><span data-stu-id="c8dd9-174">Current directory and content root</span></span>

<span data-ttu-id="c8dd9-175">O diretório de trabalho atual retornado ao chamar `Directory.GetCurrentDirectory()` de um serviço Windows é a pasta *C:\\WINDOWS\\system32*.</span><span class="sxs-lookup"><span data-stu-id="c8dd9-175">The current working directory returned by calling `Directory.GetCurrentDirectory()` for a Windows Service is the *C:\\WINDOWS\\system32* folder.</span></span> <span data-ttu-id="c8dd9-176">A pasta *system32* não é um local adequado para armazenar os arquivos de um serviço (por exemplo, os arquivos de configurações).</span><span class="sxs-lookup"><span data-stu-id="c8dd9-176">The *system32* folder isn't a suitable location to store a service's files (for example, settings files).</span></span> <span data-ttu-id="c8dd9-177">Use uma das seguintes abordagens para manter e acessar os arquivos de ativos e de configurações de um serviço com [FileConfigurationExtensions.SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath) ao usar um [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder):</span><span class="sxs-lookup"><span data-stu-id="c8dd9-177">Use one of the following approaches to maintain and access a service's assets and settings files with [FileConfigurationExtensions.SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath) when using an [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder):</span></span>

* <span data-ttu-id="c8dd9-178">Use o caminho raiz do conteúdo.</span><span class="sxs-lookup"><span data-stu-id="c8dd9-178">Use the content root path.</span></span> <span data-ttu-id="c8dd9-179">O `IHostingEnvironment.ContentRootPath` é o mesmo caminho fornecido para o argumento `binPath` quando o serviço é criado.</span><span class="sxs-lookup"><span data-stu-id="c8dd9-179">The `IHostingEnvironment.ContentRootPath` is the same path provided to the `binPath` argument when the service is created.</span></span> <span data-ttu-id="c8dd9-180">Em vez de usar `Directory.GetCurrentDirectory()` para criar caminhos para arquivos de configurações, use o caminho raiz do conteúdo e mantenha os arquivos na raiz do conteúdo do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="c8dd9-180">Instead of using `Directory.GetCurrentDirectory()` to create paths to settings files, use the content root path and maintain the files in the app's content root.</span></span>
* <span data-ttu-id="c8dd9-181">Armazene os arquivos em um local adequado no disco.</span><span class="sxs-lookup"><span data-stu-id="c8dd9-181">Store the files in a suitable location on disk.</span></span> <span data-ttu-id="c8dd9-182">Especifique um caminho absoluto com `SetBasePath` para a pasta que contém os arquivos.</span><span class="sxs-lookup"><span data-stu-id="c8dd9-182">Specify an absolute path with `SetBasePath` to the folder containing the files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c8dd9-183">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="c8dd9-183">Additional resources</span></span>

* <span data-ttu-id="c8dd9-184">[Configuração do ponto de extremidade Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) (inclui a configuração HTTPS e suporte à SNI)</span><span class="sxs-lookup"><span data-stu-id="c8dd9-184">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/web-host>

---
title: Host ASP.NET Core em um serviço do Windows
author: tdykstra
description: Saiba como hospedar um aplicativo ASP.NET Core em um serviço do Windows.
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 01/30/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/windows-service
ms.openlocfilehash: b0b27f274de1ca88b20bf582127132527b553ce0
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="56e2d-103">Host ASP.NET Core em um serviço do Windows</span><span class="sxs-lookup"><span data-stu-id="56e2d-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="56e2d-104">Por [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="56e2d-104">By [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="56e2d-105">A maneira recomendada para hospedar um aplicativo ASP.NET Core no Windows sem usar o IIS é executá-lo uma [serviço Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span><span class="sxs-lookup"><span data-stu-id="56e2d-105">The recommended way to host an ASP.NET Core app on Windows without using IIS is to run it in a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span></span> <span data-ttu-id="56e2d-106">Quando hospedado como um serviço do Windows, o aplicativo pode automaticamente iniciar após reinicializar e falhas sem exigir intervenção humana.</span><span class="sxs-lookup"><span data-stu-id="56e2d-106">When hosted as a Windows Service, the app can automatically start after reboots and crashes without requiring human intervention.</span></span>

<span data-ttu-id="56e2d-107">[Exibir ou baixar um código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="56e2d-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span> <span data-ttu-id="56e2d-108">Para obter instruções sobre como executar o aplicativo de exemplo, consulte o exemplo *README.md* arquivo.</span><span class="sxs-lookup"><span data-stu-id="56e2d-108">For instructions on how to run the sample app, see the sample's *README.md* file.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="56e2d-109">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="56e2d-109">Prerequisites</span></span>

* <span data-ttu-id="56e2d-110">O aplicativo deve ser executado em tempo de execução do .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="56e2d-110">The app must run on the .NET Framework runtime.</span></span> <span data-ttu-id="56e2d-111">No *. csproj* de arquivos, especifique os valores apropriados para [TargetFramework](/nuget/schema/target-frameworks) e [RuntimeIdentifier](/dotnet/articles/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="56e2d-111">In the *.csproj* file, specify appropriate values for [TargetFramework](/nuget/schema/target-frameworks) and [RuntimeIdentifier](/dotnet/articles/core/rid-catalog).</span></span> <span data-ttu-id="56e2d-112">Veja um exemplo:</span><span class="sxs-lookup"><span data-stu-id="56e2d-112">Here's an example:</span></span>

  [!code-xml[](windows-service/sample/AspNetCoreService.csproj?range=3-6)]

  <span data-ttu-id="56e2d-113">Ao criar um projeto no Visual Studio, use o **aplicativo do ASP.NET Core (.NET Framework)** modelo.</span><span class="sxs-lookup"><span data-stu-id="56e2d-113">When creating a project in Visual Studio, use the **ASP.NET Core Application (.NET Framework)** template.</span></span>

* <span data-ttu-id="56e2d-114">Se o aplicativo recebe solicitações de Internet (não apenas a partir de uma rede interna), ele deve usar o [HTTP.sys](xref:fundamentals/servers/httpsys) servidor web (anteriormente conhecida como [WebListener](xref:fundamentals/servers/weblistener) para aplicativos do ASP.NET Core 1. x) em vez de [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="56e2d-114">If the app receives requests from the Internet (not just from an internal network), it must use the [HTTP.sys](xref:fundamentals/servers/httpsys) web server (formerly known as [WebListener](xref:fundamentals/servers/weblistener) for ASP.NET Core 1.x apps) rather than [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="56e2d-115">O IIS é recomendado para uso como um servidor proxy reverso com Kestrel para implantações de borda.</span><span class="sxs-lookup"><span data-stu-id="56e2d-115">IIS is recommended for use as a reverse proxy server with Kestrel for edge deployments.</span></span> <span data-ttu-id="56e2d-116">Para obter mais informações, consulte [Quando usar Kestrel com um proxy reverso](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span><span class="sxs-lookup"><span data-stu-id="56e2d-116">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

## <a name="get-started"></a><span data-ttu-id="56e2d-117">Introdução</span><span class="sxs-lookup"><span data-stu-id="56e2d-117">Get started</span></span>

<span data-ttu-id="56e2d-118">Esta seção explica as alterações mínimas necessárias para configurar um projeto existente do ASP.NET Core para executar em um serviço.</span><span class="sxs-lookup"><span data-stu-id="56e2d-118">This section explains the minimum changes required to set up an existing ASP.NET Core project to run in a service.</span></span>

1. <span data-ttu-id="56e2d-119">Instale o pacote NuGet [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span><span class="sxs-lookup"><span data-stu-id="56e2d-119">Install the NuGet package [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span></span>

2. <span data-ttu-id="56e2d-120">Faça as seguintes alterações em `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="56e2d-120">Make the following changes in `Program.Main`:</span></span>

   * <span data-ttu-id="56e2d-121">Chamar `host.RunAsService` em vez de `host.Run`.</span><span class="sxs-lookup"><span data-stu-id="56e2d-121">Call `host.RunAsService` instead of `host.Run`.</span></span>

   * <span data-ttu-id="56e2d-122">Se o código chama `UseContentRoot`, use um caminho para o local de publicação em vez de `Directory.GetCurrentDirectory()`.</span><span class="sxs-lookup"><span data-stu-id="56e2d-122">If the code calls `UseContentRoot`, use a path to the publish location instead of `Directory.GetCurrentDirectory()`.</span></span>

   #### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="56e2d-123">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="56e2d-123">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)
   [!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,7,12)]

   #### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="56e2d-124">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="56e2d-124">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)
   [!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOnly&highlight=3-4,8,14)]

   * * *

3. <span data-ttu-id="56e2d-125">Publica o aplicativo em uma pasta.</span><span class="sxs-lookup"><span data-stu-id="56e2d-125">Publish the app to a folder.</span></span> <span data-ttu-id="56e2d-126">Use [dotnet publicar](/dotnet/articles/core/tools/dotnet-publish) ou um [perfil de publicação do Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles) que publica uma pasta.</span><span class="sxs-lookup"><span data-stu-id="56e2d-126">Use [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) or a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles) that publishes to a folder.</span></span>

4. <span data-ttu-id="56e2d-127">Teste ao criar e iniciar o serviço.</span><span class="sxs-lookup"><span data-stu-id="56e2d-127">Test by creating and starting the service.</span></span>

   <span data-ttu-id="56e2d-128">Abra um shell de comando com privilégios administrativos para usar o [sc.exe](https://technet.microsoft.com/library/bb490995) ferramenta de linha de comando para criar e iniciar um serviço.</span><span class="sxs-lookup"><span data-stu-id="56e2d-128">Open a command shell with administrative privileges to use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create and start a service.</span></span> <span data-ttu-id="56e2d-129">Se o serviço é chamado MyService, publicados `c:\svc`, e denominado AspNetCoreService, os comandos são:</span><span class="sxs-lookup"><span data-stu-id="56e2d-129">If the service is named MyService, published to `c:\svc`, and named AspNetCoreService, the commands are:</span></span>

   ```console
   sc create MyService binPath="c:\svc\aspnetcoreservice.exe"
   sc start MyService
   ```

   <span data-ttu-id="56e2d-130">O `binPath` valor é o caminho para o executável do aplicativo, que inclui o nome do arquivo executável.</span><span class="sxs-lookup"><span data-stu-id="56e2d-130">The `binPath` value is the path to the app's executable, which includes the executable file name.</span></span>

   ![Janela de console criar e iniciar o exemplo](windows-service/_static/create-start.png)

   <span data-ttu-id="56e2d-132">Quando terminar desses comandos, navegue até o mesmo caminho que durante a execução como um aplicativo de console (por padrão, `http://localhost:5000`):</span><span class="sxs-lookup"><span data-stu-id="56e2d-132">When these commands finish, browse to the same path as when running as a console app (by default, `http://localhost:5000`):</span></span>

   ![Executando em um serviço](windows-service/_static/running-in-service.png)

## <a name="provide-a-way-to-run-outside-of-a-service"></a><span data-ttu-id="56e2d-134">Fornecem uma maneira de executar fora de um serviço</span><span class="sxs-lookup"><span data-stu-id="56e2d-134">Provide a way to run outside of a service</span></span>

<span data-ttu-id="56e2d-135">É mais fácil de testar e depurar quando em execução fora de um serviço, portanto, é comum para adicionar o código que chama `RunAsService` apenas em determinadas condições.</span><span class="sxs-lookup"><span data-stu-id="56e2d-135">It's easier to test and debug when running outside of a service, so it's customary to add code that calls `RunAsService` only under certain conditions.</span></span> <span data-ttu-id="56e2d-136">Por exemplo, o aplicativo pode ser executado como um aplicativo de console com um `--console` argumento de linha de comando ou se o depurador é anexado:</span><span class="sxs-lookup"><span data-stu-id="56e2d-136">For example, the app can run as a console app with a `--console` command-line argument or if the debugger is attached:</span></span>

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="56e2d-137">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="56e2d-137">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)
[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="56e2d-138">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="56e2d-138">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)
[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOrConsole)]

* * *
## <a name="handle-stopping-and-starting-events"></a><span data-ttu-id="56e2d-139">Tratar parar e iniciar eventos</span><span class="sxs-lookup"><span data-stu-id="56e2d-139">Handle stopping and starting events</span></span>

<span data-ttu-id="56e2d-140">Para tratar `OnStarting`, `OnStarted`, e `OnStopping` eventos, faça as seguintes alterações adicionais:</span><span class="sxs-lookup"><span data-stu-id="56e2d-140">To handle `OnStarting`, `OnStarted`, and `OnStopping` events, make the following additional changes:</span></span>

1. <span data-ttu-id="56e2d-141">Criar uma classe que deriva de `WebHostService`:</span><span class="sxs-lookup"><span data-stu-id="56e2d-141">Create a class that derives from `WebHostService`:</span></span>

   [!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=NoLogging)]

2. <span data-ttu-id="56e2d-142">Criar um método de extensão para `IWebHost` que passa personalizado `WebHostService` para `ServiceBase.Run`:</span><span class="sxs-lookup"><span data-stu-id="56e2d-142">Create an extension method for `IWebHost` that passes the custom `WebHostService` to `ServiceBase.Run`:</span></span>

   [!code-csharp[](windows-service/sample/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="56e2d-143">Em `Program.Main`, chame o novo método de extensão, `RunAsCustomService`, em vez de `RunAsService`:</span><span class="sxs-lookup"><span data-stu-id="56e2d-143">In `Program.Main`, call the new extension method, `RunAsCustomService`, instead of `RunAsService`:</span></span>

   #### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="56e2d-144">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="56e2d-144">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)
   [!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=24)]

   #### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="56e2d-145">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="56e2d-145">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)
   [!code-csharp[](windows-service/sample_snapshot/Program.cs?name=HandleStopStart&highlight=26)]

   * * *
<span data-ttu-id="56e2d-146">Se personalizado `WebHostService` código requer que um serviço de injeção de dependência (como um agente de log), ele obtido o `Services` propriedade `IWebHost`:</span><span class="sxs-lookup"><span data-stu-id="56e2d-146">If the custom `WebHostService` code requires a service from dependency injection (such as a logger), obtain it from the `Services` property of `IWebHost`:</span></span>

[!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=Logging&highlight=7)]

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="56e2d-147">Servidor proxy e cenários de Balanceador de carga</span><span class="sxs-lookup"><span data-stu-id="56e2d-147">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="56e2d-148">Serviços que interagem com solicitações da Internet ou de uma rede corporativa e estiverem atrás de um proxy ou balanceador de carga podem exigir configuração adicional.</span><span class="sxs-lookup"><span data-stu-id="56e2d-148">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="56e2d-149">Para obter mais informações, consulte [configurar o ASP.NET Core para trabalhar com servidores proxy e balanceadores de carga](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="56e2d-149">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

## <a name="acknowledgments"></a><span data-ttu-id="56e2d-150">Agradecimentos</span><span class="sxs-lookup"><span data-stu-id="56e2d-150">Acknowledgments</span></span>

<span data-ttu-id="56e2d-151">Este artigo foi escrito com a Ajuda das fontes publicados:</span><span class="sxs-lookup"><span data-stu-id="56e2d-151">This article was written with the help of published sources:</span></span>

* [<span data-ttu-id="56e2d-152">Hospedagem ASP.NET Core como serviço do Windows</span><span class="sxs-lookup"><span data-stu-id="56e2d-152">Hosting ASP.NET Core as Windows service</span></span>](https://stackoverflow.com/questions/37346383/hosting-asp-net-core-as-windows-service/37464074)
* [<span data-ttu-id="56e2d-153">Como hospedar o ASP.NET Core em um serviço do Windows</span><span class="sxs-lookup"><span data-stu-id="56e2d-153">How to host your ASP.NET Core in a Windows Service</span></span>](https://dotnetthoughts.net/how-to-host-your-aspnet-core-in-a-windows-service/)

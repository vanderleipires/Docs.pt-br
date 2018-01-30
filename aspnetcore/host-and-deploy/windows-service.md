---
title: "Host em um serviço do Windows"
author: tdykstra
description: "Saiba como hospedar um aplicativo ASP.NET Core em um serviço do Windows."
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 03/30/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/windows-service
ms.openlocfilehash: beda34dbd613f6ffe0afa207ab57dd6ebbc489ee
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/30/2018
---
# <a name="host-an-aspnet-core-app-in-a-windows-service"></a><span data-ttu-id="8e554-103">Hospedar um aplicativo ASP.NET Core em um serviço do Windows</span><span class="sxs-lookup"><span data-stu-id="8e554-103">Host an ASP.NET Core app in a Windows Service</span></span>

<span data-ttu-id="8e554-104">Por [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="8e554-104">By [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="8e554-105">A maneira recomendada para hospedar um aplicativo ASP.NET Core no Windows sem usar o IIS é executá-lo uma [serviço Windows](https://docs.microsoft.com/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span><span class="sxs-lookup"><span data-stu-id="8e554-105">The recommended way to host an ASP.NET Core app on Windows without using IIS is to run it in a [Windows Service](https://docs.microsoft.com/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span></span> <span data-ttu-id="8e554-106">Dessa forma ele pode iniciar automaticamente após a reinicialização e falhas, sem esperar que alguém fazer logon.</span><span class="sxs-lookup"><span data-stu-id="8e554-106">That way it can automatically start after reboots and crashes, without waiting for someone to log in.</span></span>

<span data-ttu-id="8e554-107">[Exibir ou baixar o código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="8e554-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span> <span data-ttu-id="8e554-108">Consulte o [próximas etapas](#next-steps) seção para obter instruções sobre como executá-lo.</span><span class="sxs-lookup"><span data-stu-id="8e554-108">See the [Next Steps](#next-steps) section for instructions on how to run it.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8e554-109">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="8e554-109">Prerequisites</span></span>

* <span data-ttu-id="8e554-110">O aplicativo deve ser executado em tempo de execução do .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="8e554-110">The app must run on the .NET Framework runtime.</span></span>  <span data-ttu-id="8e554-111">No *. csproj* de arquivos, especifique os valores apropriados para [TargetFramework](https://docs.microsoft.com/nuget/schema/target-frameworks) e [RuntimeIdentifier](https://docs.microsoft.com/dotnet/articles/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="8e554-111">In the *.csproj* file, specify appropriate values for [TargetFramework](https://docs.microsoft.com/nuget/schema/target-frameworks) and [RuntimeIdentifier](https://docs.microsoft.com/dotnet/articles/core/rid-catalog).</span></span> <span data-ttu-id="8e554-112">Veja um exemplo:</span><span class="sxs-lookup"><span data-stu-id="8e554-112">Here's an example:</span></span>

  [!code-xml[](windows-service/sample/AspNetCoreService.csproj?range=3-6)]

  <span data-ttu-id="8e554-113">Ao criar um projeto no Visual Studio, use o **aplicativo do ASP.NET Core (.NET Framework)** modelo.</span><span class="sxs-lookup"><span data-stu-id="8e554-113">When creating a project in Visual Studio, use the **ASP.NET Core Application (.NET Framework)** template.</span></span>

* <span data-ttu-id="8e554-114">Se o aplicativo recebe solicitações de Internet (não apenas a partir de uma rede interna), ele deve usar o [WebListener](xref:fundamentals/servers/weblistener) servidor web em vez de [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="8e554-114">If the app receives requests from the Internet (not just from an internal network), it must use the [WebListener](xref:fundamentals/servers/weblistener) web server rather than [Kestrel](xref:fundamentals/servers/kestrel).</span></span>  <span data-ttu-id="8e554-115">Kestrel deve ser usado com o IIS para implantações de borda.</span><span class="sxs-lookup"><span data-stu-id="8e554-115">Kestrel must be used with IIS for edge deployments.</span></span>  <span data-ttu-id="8e554-116">Para obter mais informações, consulte [Quando usar Kestrel com um proxy reverso](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span><span class="sxs-lookup"><span data-stu-id="8e554-116">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

## <a name="getting-started"></a><span data-ttu-id="8e554-117">Introdução</span><span class="sxs-lookup"><span data-stu-id="8e554-117">Getting started</span></span>

<span data-ttu-id="8e554-118">Esta seção explica as alterações mínimas necessárias para configurar um projeto existente do ASP.NET Core para executar em um serviço.</span><span class="sxs-lookup"><span data-stu-id="8e554-118">This section explains the minimum changes required to set up an existing ASP.NET Core project to run in a service.</span></span>

* <span data-ttu-id="8e554-119">Instale o pacote NuGet [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span><span class="sxs-lookup"><span data-stu-id="8e554-119">Install the NuGet package [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span></span>

* <span data-ttu-id="8e554-120">Faça as seguintes alterações em `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="8e554-120">Make the following changes in `Program.Main`:</span></span>
  
  * <span data-ttu-id="8e554-121">Chamar `host.RunAsService` em vez de `host.Run`.</span><span class="sxs-lookup"><span data-stu-id="8e554-121">Call `host.RunAsService` instead of `host.Run`.</span></span>
  
  * <span data-ttu-id="8e554-122">Se o código chama `UseContentRoot`, use um caminho para o local de publicação em vez de`Directory.GetCurrentDirectory()`</span><span class="sxs-lookup"><span data-stu-id="8e554-122">If the code calls `UseContentRoot`, use a path to the publish location instead of `Directory.GetCurrentDirectory()`</span></span> 
  
  [!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,8,14)]

* <span data-ttu-id="8e554-123">Publica o aplicativo em uma pasta.</span><span class="sxs-lookup"><span data-stu-id="8e554-123">Publish the application to a folder.</span></span>

  <span data-ttu-id="8e554-124">Use [dotnet publicar](https://docs.microsoft.com/dotnet/articles/core/tools/dotnet-publish) ou um [perfil de publicação do Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles) que publica uma pasta.</span><span class="sxs-lookup"><span data-stu-id="8e554-124">Use [dotnet publish](https://docs.microsoft.com/dotnet/articles/core/tools/dotnet-publish) or a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles) that publishes to a folder.</span></span>

* <span data-ttu-id="8e554-125">Teste ao criar e iniciar o serviço.</span><span class="sxs-lookup"><span data-stu-id="8e554-125">Test by creating and starting the service.</span></span>

  <span data-ttu-id="8e554-126">Abra uma janela de prompt de comando de administrador para usar o [sc.exe](https://technet.microsoft.com/library/bb490995) ferramenta de linha de comando para criar e iniciar um serviço.</span><span class="sxs-lookup"><span data-stu-id="8e554-126">Open an administrator command prompt window to use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create and start a service.</span></span>  
  
  <span data-ttu-id="8e554-127">Se o serviço é chamado MyService, publicar o aplicativo para `c:\svc`e o aplicativo em si é denominado AspNetCoreService, os comandos teria esta aparência:</span><span class="sxs-lookup"><span data-stu-id="8e554-127">If the service is named MyService, publish the app to `c:\svc`, and the app itself is named AspNetCoreService, the commands would look like this:</span></span>

  ```console
  sc create MyService binPath="C:\Svc\AspNetCoreService.exe"
  sc start MyService
  ```

  <span data-ttu-id="8e554-128">O `binPath` valor é o caminho para o executável do aplicativo, incluindo o nome do arquivo executável em si.</span><span class="sxs-lookup"><span data-stu-id="8e554-128">The `binPath` value is the path to the app's executable, including the executable filename itself.</span></span>

  ![Janela de console criar e iniciar o exemplo](windows-service/_static/create-start.png)

  <span data-ttu-id="8e554-130">Quando terminar desses comandos, navegue até o mesmo caminho que durante a execução como um aplicativo de console (por padrão, `http://localhost:5000`)</span><span class="sxs-lookup"><span data-stu-id="8e554-130">When these commands finish, browse to the same path as when running as a console app (by default, `http://localhost:5000`)</span></span>

  ![Executando em um serviço](windows-service/_static/running-in-service.png)


## <a name="provide-a-way-to-run-outside-of-a-service"></a><span data-ttu-id="8e554-132">Fornecem uma maneira de executar fora de um serviço</span><span class="sxs-lookup"><span data-stu-id="8e554-132">Provide a way to run outside of a service</span></span>

<span data-ttu-id="8e554-133">É mais fácil de testar e depurar quando em execução fora de um serviço, portanto, é comum para adicionar o código que chama `host.RunAsService` apenas em determinadas condições.</span><span class="sxs-lookup"><span data-stu-id="8e554-133">It's easier to test and debug when running outside of a service, so it's customary to add code that calls `host.RunAsService` only under certain conditions.</span></span>  <span data-ttu-id="8e554-134">Por exemplo, o aplicativo pode ser executado como um aplicativo de console com um `--console` argumento de linha de comando ou se o depurador é anexado.</span><span class="sxs-lookup"><span data-stu-id="8e554-134">For example, the app can run as a console app with a `--console` command-line argument or if the debugger is attached.</span></span>

[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]

## <a name="handle-stopping-and-starting-events"></a><span data-ttu-id="8e554-135">Tratar parar e iniciar eventos</span><span class="sxs-lookup"><span data-stu-id="8e554-135">Handle stopping and starting events</span></span>

<span data-ttu-id="8e554-136">Para tratar `OnStarting`, `OnStarted`, e `OnStopping` eventos, faça as seguintes alterações adicionais:</span><span class="sxs-lookup"><span data-stu-id="8e554-136">To handle `OnStarting`, `OnStarted`, and `OnStopping` events, make the following additional changes:</span></span>

* <span data-ttu-id="8e554-137">Crie uma classe que deriva de `WebHostService`.</span><span class="sxs-lookup"><span data-stu-id="8e554-137">Create a class that derives from `WebHostService`.</span></span>

  [!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=NoLogging)]

* <span data-ttu-id="8e554-138">Criar um método de extensão para `IWebHost` que passa personalizado `WebHostService` para `ServiceBase.Run`.</span><span class="sxs-lookup"><span data-stu-id="8e554-138">Create an extension method for `IWebHost` that passes the custom `WebHostService` to `ServiceBase.Run`.</span></span>

  [!code-csharp[](windows-service/sample/WebHostServiceExtensions.cs?name=ExtensionsClass)]

* <span data-ttu-id="8e554-139">Em `Program.Main` alteração chamar o novo método de extensão, em vez de `host.RunAsService`.</span><span class="sxs-lookup"><span data-stu-id="8e554-139">In `Program.Main` change call the new extension method instead of `host.RunAsService`.</span></span>

  [!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=26)]

<span data-ttu-id="8e554-140">Se personalizado `WebHostService` código precisa obter um serviço de injeção de dependência (como um agente de log), obtenha-o do `Services` propriedade `IWebHost`.</span><span class="sxs-lookup"><span data-stu-id="8e554-140">If the custom `WebHostService` code needs to get a service from dependency injection (such as a logger), get it from the `Services` property of `IWebHost`.</span></span>

[!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=Logging&highlight=7)]

## <a name="next-steps"></a><span data-ttu-id="8e554-141">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="8e554-141">Next steps</span></span>

<span data-ttu-id="8e554-142">O [aplicativo de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) que acompanha este artigo é um aplicativo da web MVC simples que tenha sido modificado, conforme mostrado no anterior exemplos de código.</span><span class="sxs-lookup"><span data-stu-id="8e554-142">The [sample application](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) that accompanies this article is a simple MVC web app that has been modified as shown in preceding code examples.</span></span>  <span data-ttu-id="8e554-143">Para executá-lo em um serviço, siga as etapas a seguir:</span><span class="sxs-lookup"><span data-stu-id="8e554-143">To run it in a service, do the following steps:</span></span>

* <span data-ttu-id="8e554-144">Publicar *c:\svc*.</span><span class="sxs-lookup"><span data-stu-id="8e554-144">Publish to *c:\svc*.</span></span>

* <span data-ttu-id="8e554-145">Abra uma janela de administrador.</span><span class="sxs-lookup"><span data-stu-id="8e554-145">Open an administrator window.</span></span>

* <span data-ttu-id="8e554-146">Digite os seguintes comandos:</span><span class="sxs-lookup"><span data-stu-id="8e554-146">Enter the following commands:</span></span>

  ```console
  sc create MyService binPath="c:\svc\aspnetcoreservice.exe"
  sc start MyService
  ```

  * <span data-ttu-id="8e554-147">Em um navegador, vá para http://localhost:5000/ para verificar se ele está em execução.</span><span class="sxs-lookup"><span data-stu-id="8e554-147">In a browser, go to http://localhost:5000 to verify that it's running.</span></span>

<span data-ttu-id="8e554-148">Se o aplicativo não for iniciado como o esperado quando em execução em um serviço, uma maneira rápida para disponibilizar as mensagens de erro é adicionar um provedor de log, como o [provedor de log de eventos do Windows](xref:fundamentals/logging/index#eventlog).</span><span class="sxs-lookup"><span data-stu-id="8e554-148">If the app doesn't start up as expected when running in a service, a quick way to make error messages accessible is to add a logging provider such as the [Windows EventLog provider](xref:fundamentals/logging/index#eventlog).</span></span>

## <a name="acknowledgments"></a><span data-ttu-id="8e554-149">Confirmações</span><span class="sxs-lookup"><span data-stu-id="8e554-149">Acknowledgments</span></span>

<span data-ttu-id="8e554-150">Este artigo foi escrito com a Ajuda de fontes que já foram publicadas.</span><span class="sxs-lookup"><span data-stu-id="8e554-150">This article was written with the help of sources that were already published.</span></span> <span data-ttu-id="8e554-151">A primeira e mais úteis, eles foram estes:</span><span class="sxs-lookup"><span data-stu-id="8e554-151">The earliest and most useful of them were these:</span></span>

* [<span data-ttu-id="8e554-152">Hospedagem ASP.NET Core como serviço do Windows</span><span class="sxs-lookup"><span data-stu-id="8e554-152">Hosting ASP.NET Core as Windows service</span></span>](https://stackoverflow.com/questions/37346383/hosting-asp-net-core-as-windows-service/37464074)
* [<span data-ttu-id="8e554-153">Como hospedar o ASP.NET Core em um serviço do Windows</span><span class="sxs-lookup"><span data-stu-id="8e554-153">How to host your ASP.NET Core in a Windows Service</span></span>](https://dotnetthoughts.net/how-to-host-your-aspnet-core-in-a-windows-service/)

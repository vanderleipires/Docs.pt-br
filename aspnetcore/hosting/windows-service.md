---
title: "Host em um serviço do Windows"
author: tdykstra
description: "Saiba como hospedar um aplicativo ASP.NET Core em um serviço do Windows."
keywords: "Hospedagem ASP.NET Core, o serviço do Windows"
ms.author: tdykstra
manager: wpickett
ms.date: 03/30/2017
ms.topic: article
ms.assetid: d9a65066-d7cb-47df-b046-64629c4d2c6f
ms.technology: aspnet
ms.prod: aspnet-core
uid: hosting/windows-service
ms.openlocfilehash: 1b3cdc18ded89ebdf7b7afa9f43af9669748eff4
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/11/2017
---
# <a name="host-an-aspnet-core-app-in-a-windows-service"></a><span data-ttu-id="f474b-104">Hospedar um aplicativo ASP.NET Core em um serviço do Windows</span><span class="sxs-lookup"><span data-stu-id="f474b-104">Host an ASP.NET Core app in a Windows Service</span></span>

<span data-ttu-id="f474b-105">Por [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="f474b-105">By [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="f474b-106">É a maneira recomendada para hospedar um aplicativo ASP.NET Core no Windows quando você não usar o IIS para executá-lo uma [serviço Windows](https://msdn.microsoft.com/library/d56de412).</span><span class="sxs-lookup"><span data-stu-id="f474b-106">The recommended way to host an ASP.NET Core app on Windows when you don't use IIS is to run it in a [Windows Service](https://msdn.microsoft.com/library/d56de412).</span></span> <span data-ttu-id="f474b-107">Dessa forma ele pode iniciar automaticamente após a reinicialização e falhas, sem esperar que alguém fazer logon.</span><span class="sxs-lookup"><span data-stu-id="f474b-107">That way it can automatically start after reboots and crashes, without waiting for someone to log in.</span></span>

<span data-ttu-id="f474b-108">[Exibir ou baixar o código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/hosting/windows-service/sample) consulte o [próximas etapas](#next-steps) seção para obter instruções sobre como executá-lo.</span><span class="sxs-lookup"><span data-stu-id="f474b-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/hosting/windows-service/sample) See the [Next Steps](#next-steps) section for instructions on how to run it.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f474b-109">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="f474b-109">Prerequisites</span></span>

* <span data-ttu-id="f474b-110">O aplicativo deve ser executado em runtime do .NET framework.</span><span class="sxs-lookup"><span data-stu-id="f474b-110">The app must run on the .NET framework runtime.</span></span>  <span data-ttu-id="f474b-111">No *. csproj* de arquivos, especifique os valores apropriados para [TargetFramework](https://docs.microsoft.com/nuget/schema/target-frameworks) e [RuntimeIdentifier](https://docs.microsoft.com/dotnet/articles/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="f474b-111">In the *.csproj* file, specify appropriate values for [TargetFramework](https://docs.microsoft.com/nuget/schema/target-frameworks) and [RuntimeIdentifier](https://docs.microsoft.com/dotnet/articles/core/rid-catalog).</span></span> <span data-ttu-id="f474b-112">Veja um exemplo:</span><span class="sxs-lookup"><span data-stu-id="f474b-112">Here's an example:</span></span>

  [!code-xml[](windows-service/sample/AspNetCoreService.csproj?range=3-6)]

  <span data-ttu-id="f474b-113">Ao criar um projeto no Visual Studio, use o **aplicativo do ASP.NET Core (.NET Framework)** modelo.</span><span class="sxs-lookup"><span data-stu-id="f474b-113">When creating a project in Visual Studio, use the **ASP.NET Core Application (.NET Framework)** template.</span></span>

* <span data-ttu-id="f474b-114">Se o aplicativo receberá solicitações da internet (não apenas a partir de uma rede interna), ele deve usar o [WebListener](xref:fundamentals/servers/weblistener) servidor web em vez de [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="f474b-114">If the app will get requests from the internet (not just from an internal network), it must use the [WebListener](xref:fundamentals/servers/weblistener) web server rather than [Kestrel](xref:fundamentals/servers/kestrel).</span></span>  <span data-ttu-id="f474b-115">Kestrel deve ser usado com o IIS para implantações de borda.</span><span class="sxs-lookup"><span data-stu-id="f474b-115">Kestrel must be used with IIS for edge deployments.</span></span>  <span data-ttu-id="f474b-116">Para obter mais informações, consulte [quando usar Kestrel com um proxy reverso](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span><span class="sxs-lookup"><span data-stu-id="f474b-116">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

## <a name="getting-started"></a><span data-ttu-id="f474b-117">Introdução</span><span class="sxs-lookup"><span data-stu-id="f474b-117">Getting started</span></span>

<span data-ttu-id="f474b-118">Esta seção explica as alterações mínimas necessárias para configurar um projeto existente do ASP.NET Core para executar em um serviço.</span><span class="sxs-lookup"><span data-stu-id="f474b-118">This section explains the minimum changes required to set up an existing ASP.NET Core project to run in a service.</span></span>

* <span data-ttu-id="f474b-119">Instale o pacote NuGet [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span><span class="sxs-lookup"><span data-stu-id="f474b-119">Install the NuGet package [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span></span>

* <span data-ttu-id="f474b-120">Faça as seguintes alterações em `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="f474b-120">Make the following changes in `Program.Main`:</span></span>
  
  * <span data-ttu-id="f474b-121">Chamar `host.RunAsService` em vez de `host.Run`.</span><span class="sxs-lookup"><span data-stu-id="f474b-121">Call `host.RunAsService` instead of `host.Run`.</span></span>
  
  * <span data-ttu-id="f474b-122">Se seu código chama `UseContentRoot`, use um caminho para o local de publicação em vez de`Directory.GetCurrentDirectory()`</span><span class="sxs-lookup"><span data-stu-id="f474b-122">If your code calls `UseContentRoot`, use a path to the publish location instead of `Directory.GetCurrentDirectory()`</span></span> 
  
  [!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,8,14)]

* <span data-ttu-id="f474b-123">Publica o aplicativo em uma pasta.</span><span class="sxs-lookup"><span data-stu-id="f474b-123">Publish the application to a folder.</span></span>

  <span data-ttu-id="f474b-124">Use [dotnet publicar](https://docs.microsoft.com/dotnet/articles/core/tools/dotnet-publish) ou um [perfil de publicação do Visual Studio](xref:publishing/web-publishing-vs) que publica uma pasta.</span><span class="sxs-lookup"><span data-stu-id="f474b-124">Use [dotnet publish](https://docs.microsoft.com/dotnet/articles/core/tools/dotnet-publish) or a [Visual Studio publish profile](xref:publishing/web-publishing-vs) that publishes to a folder.</span></span>

* <span data-ttu-id="f474b-125">Teste ao criar e iniciar o serviço.</span><span class="sxs-lookup"><span data-stu-id="f474b-125">Test by creating and starting the service.</span></span>

  <span data-ttu-id="f474b-126">Abra uma janela de prompt de comando de administrador para usar o [sc.exe](https://technet.microsoft.com/library/bb490995) ferramenta de linha de comando para criar e iniciar um serviço.</span><span class="sxs-lookup"><span data-stu-id="f474b-126">Open an administrator command prompt window to use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create and start a service.</span></span>  
  
  <span data-ttu-id="f474b-127">Se você nomear o serviço MyService, você publica seu aplicativo para `c:\svc`e o aplicativo em si é denominado AspNetCoreService, os comandos teria esta aparência:</span><span class="sxs-lookup"><span data-stu-id="f474b-127">If you name your service MyService, you publish your app to `c:\svc`, and the app itself is named AspNetCoreService, the commands would look like this:</span></span>

  ```console
  sc create MyService binPath="C:\Svc\AspNetCoreService.exe"
  sc start MyService
  ```
  <span data-ttu-id="f474b-128">O `binPath` valor é o caminho para o executável do aplicativo, incluindo o nome do arquivo executável em si.</span><span class="sxs-lookup"><span data-stu-id="f474b-128">The `binPath` value is the path to your app's executable, including the executable filename itself.</span></span>

  ![Janela de console criar e iniciar o exemplo](windows-service/_static/create-start.png)

  <span data-ttu-id="f474b-130">Quando terminar desses comandos, você pode navegar para o mesmo caminho que quando você executa como um aplicativo de console (por padrão, `http://localhost:5000`)</span><span class="sxs-lookup"><span data-stu-id="f474b-130">When these commands finish, you can browse to the same path as when you run as a console app (by default, `http://localhost:5000`)</span></span>

  ![Executando em um serviço](windows-service/_static/running-in-service.png)


## <a name="provide-a-way-to-run-outside-of-a-service"></a><span data-ttu-id="f474b-132">Fornecem uma maneira de executar fora de um serviço</span><span class="sxs-lookup"><span data-stu-id="f474b-132">Provide a way to run outside of a service</span></span>

<span data-ttu-id="f474b-133">É mais fácil de testar e depurar quando você estiver executando o fora de um serviço, portanto, é comum para adicionar o código que chama `host.RunAsService` apenas em determinadas condições.</span><span class="sxs-lookup"><span data-stu-id="f474b-133">It's easier to test and debug when you're running outside of a service, so it's customary to add code that calls `host.RunAsService` only under certain conditions.</span></span>  <span data-ttu-id="f474b-134">Por exemplo, você pode executar como um aplicativo de console se você receber um `--console` argumento de linha de comando ou se o depurador é anexado.</span><span class="sxs-lookup"><span data-stu-id="f474b-134">For example, you could run as a console app if you get a `--console` command-line argument or if the debugger is attached.</span></span>

[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]

## <a name="handle-stopping-and-starting-events"></a><span data-ttu-id="f474b-135">Tratar parar e iniciar eventos</span><span class="sxs-lookup"><span data-stu-id="f474b-135">Handle stopping and starting events</span></span>

<span data-ttu-id="f474b-136">Se você desejar tratar `OnStarting`, `OnStarted`, e `OnStopping` eventos, faça as seguintes alterações adicionais:</span><span class="sxs-lookup"><span data-stu-id="f474b-136">If you want to handle `OnStarting`, `OnStarted`, and `OnStopping` events, make the following additional changes:</span></span>

* <span data-ttu-id="f474b-137">Crie uma classe que deriva de `WebHostService`.</span><span class="sxs-lookup"><span data-stu-id="f474b-137">Create a class that derives from `WebHostService`.</span></span>

  [!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=NoLogging)]

* <span data-ttu-id="f474b-138">Criar um método de extensão para `IWebHost` que passa personalizados `WebHostService` para `ServiceBase.Run`.</span><span class="sxs-lookup"><span data-stu-id="f474b-138">Create an extension method for `IWebHost` that passes your custom `WebHostService` to `ServiceBase.Run`.</span></span>

  [!code-csharp[](windows-service/sample/WebHostServiceExtensions.cs?name=ExtensionsClass)]

* <span data-ttu-id="f474b-139">Em `Program.Main` alteração chamar o novo método de extensão, em vez de `host.RunAsService`.</span><span class="sxs-lookup"><span data-stu-id="f474b-139">In `Program.Main` change call the new extension method instead of `host.RunAsService`.</span></span>

  [!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=26)]

<span data-ttu-id="f474b-140">Se seu personalizado `WebHostService` código precisa obter um serviço de injeção de dependência (como um agente de log), você poderá obtê-lo do `Services` propriedade de `IWebHost`.</span><span class="sxs-lookup"><span data-stu-id="f474b-140">If your custom `WebHostService` code needs to get a service from dependency injection (such as a logger), you can get it from the `Services` property of `IWebHost`.</span></span>

[!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=Logging&highlight=7)]

## <a name="next-steps"></a><span data-ttu-id="f474b-141">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="f474b-141">Next steps</span></span>

<span data-ttu-id="f474b-142">O [aplicativo de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/hosting/windows-service/sample) que acompanha este artigo é um aplicativo da web MVC simples que tenha sido modificado, conforme mostrado no anterior exemplos de código.</span><span class="sxs-lookup"><span data-stu-id="f474b-142">The [sample application](https://github.com/aspnet/Docs/tree/master/aspnetcore/hosting/windows-service/sample) that accompanies this article is a simple MVC web app that has been modified as shown in preceding code examples.</span></span>  <span data-ttu-id="f474b-143">Para executá-lo em um serviço, siga as etapas a seguir:</span><span class="sxs-lookup"><span data-stu-id="f474b-143">To run it in a service, do the following steps:</span></span>

* <span data-ttu-id="f474b-144">Publicar *c:\svc*.</span><span class="sxs-lookup"><span data-stu-id="f474b-144">Publish to *c:\svc*.</span></span>

* <span data-ttu-id="f474b-145">Abra uma janela de administrador.</span><span class="sxs-lookup"><span data-stu-id="f474b-145">Open an administrator window.</span></span>

* <span data-ttu-id="f474b-146">Digite os seguintes comandos:</span><span class="sxs-lookup"><span data-stu-id="f474b-146">Enter the following commands:</span></span>

  ```console
  sc create MyService binPath="c:\svc\aspnetcoreservice.exe"
  sc start MyService
  ```

  * <span data-ttu-id="f474b-147">Em um navegador, vá para http://localhost:5000/ para verificar se ele está em execução.</span><span class="sxs-lookup"><span data-stu-id="f474b-147">In a browser, go to http://localhost:5000 to verify that it's running.</span></span>

<span data-ttu-id="f474b-148">Se o aplicativo não for iniciado como o esperado quando em execução em um serviço, uma maneira rápida para disponibilizar as mensagens de erro é adicionar um provedor de log, como o [provedor de log de eventos do Windows](xref:fundamentals/logging#eventlog).</span><span class="sxs-lookup"><span data-stu-id="f474b-148">If the app doesn't start up as expected when running in a service, a quick way to make error messages accessible is to add a logging provider such as the [Windows EventLog provider](xref:fundamentals/logging#eventlog).</span></span>

## <a name="acknowledgments"></a><span data-ttu-id="f474b-149">Confirmações</span><span class="sxs-lookup"><span data-stu-id="f474b-149">Acknowledgments</span></span>

<span data-ttu-id="f474b-150">Este artigo foi escrito com a Ajuda de fontes que já foram publicadas.</span><span class="sxs-lookup"><span data-stu-id="f474b-150">This article was written with the help of sources that were already published.</span></span> <span data-ttu-id="f474b-151">A primeira e mais úteis, eles foram estes:</span><span class="sxs-lookup"><span data-stu-id="f474b-151">The earliest and most useful of them were these:</span></span>

* [<span data-ttu-id="f474b-152">Hospedagem ASP.NET Core como serviço do Windows</span><span class="sxs-lookup"><span data-stu-id="f474b-152">Hosting ASP.NET Core as Windows service</span></span>](http://stackoverflow.com/questions/37346383/hosting-asp-net-core-as-windows-service/37464074#37464074)
* [<span data-ttu-id="f474b-153">Como hospedar o ASP.NET Core em um serviço do Windows</span><span class="sxs-lookup"><span data-stu-id="f474b-153">How to host your ASP.NET Core in a Windows Service</span></span>](http://dotnetthoughts.net/how-to-host-your-aspnet-core-in-a-windows-service/)

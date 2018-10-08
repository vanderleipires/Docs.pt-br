---
title: Tarefas em segundo plano com serviços hospedados no ASP.NET Core
author: guardrex
description: Aprenda a implementar tarefas em segundo plano com serviços hospedados no ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/15/2018
uid: fundamentals/host/hosted-services
ms.openlocfilehash: 8c6a4a039fdc2cbe097d3439b3d79b9228d458b1
ms.sourcegitcommit: 599ebae5c2d6fcb22dfa6ae7d1f4bdfcacb79af4
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/26/2018
ms.locfileid: "47210971"
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a><span data-ttu-id="1a09b-103">Tarefas em segundo plano com serviços hospedados no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1a09b-103">Background tasks with hosted services in ASP.NET Core</span></span>

<span data-ttu-id="1a09b-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="1a09b-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="1a09b-105">No ASP.NET Core, as tarefas em segundo plano podem ser implementadas como *serviços hospedados*.</span><span class="sxs-lookup"><span data-stu-id="1a09b-105">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="1a09b-106">Um serviço hospedado é uma classe com lógica de tarefa em segundo plano que implementa a interface <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="1a09b-106">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="1a09b-107">Este tópico fornece três exemplos de serviço hospedado:</span><span class="sxs-lookup"><span data-stu-id="1a09b-107">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="1a09b-108">Tarefa em segundo plano que é executada com um temporizador.</span><span class="sxs-lookup"><span data-stu-id="1a09b-108">Background task that runs on a timer.</span></span>
* <span data-ttu-id="1a09b-109">Serviço hospedado que ativa um serviço com escopo.</span><span class="sxs-lookup"><span data-stu-id="1a09b-109">Hosted service that activates a scoped service.</span></span> <span data-ttu-id="1a09b-110">O serviço com escopo pode usar a injeção de dependência.</span><span class="sxs-lookup"><span data-stu-id="1a09b-110">The scoped service can use dependency injection.</span></span>
* <span data-ttu-id="1a09b-111">Tarefas em segundo plano na fila que são executadas sequencialmente.</span><span class="sxs-lookup"><span data-stu-id="1a09b-111">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="1a09b-112">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="1a09b-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="1a09b-113">Este aplicativo de exemplo é fornecido em duas versões:</span><span class="sxs-lookup"><span data-stu-id="1a09b-113">The sample app is provided in two versions:</span></span>

* <span data-ttu-id="1a09b-114">Host da Web &ndash; O Host da Web é útil para hospedar aplicativos web.</span><span class="sxs-lookup"><span data-stu-id="1a09b-114">Web Host &ndash; The Web Host is useful for hosting web apps.</span></span> <span data-ttu-id="1a09b-115">O código de exemplo mostrado neste tópico é da versão do host da web do exemplo.</span><span class="sxs-lookup"><span data-stu-id="1a09b-115">The example code shown in this topic is from the Web Host version of the sample.</span></span> <span data-ttu-id="1a09b-116">Para obter mais informações, consulte o tópico [Host da Web](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="1a09b-116">For more information, see the [Web Host](xref:fundamentals/host/web-host) topic.</span></span>
* <span data-ttu-id="1a09b-117">Host Genérico &ndash; O Host Genérico é novo no ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="1a09b-117">Generic Host &ndash; The Generic Host is new in ASP.NET Core 2.1.</span></span> <span data-ttu-id="1a09b-118">Para obter mais informações, confira o tópico [Host Genérico](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="1a09b-118">For more information, see the [Generic Host](xref:fundamentals/host/generic-host) topic.</span></span>

## <a name="package"></a><span data-ttu-id="1a09b-119">Pacote</span><span class="sxs-lookup"><span data-stu-id="1a09b-119">Package</span></span>

<span data-ttu-id="1a09b-120">Referencie o [metapacote Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) ou adicione uma referência de pacote ao pacote [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting).</span><span class="sxs-lookup"><span data-stu-id="1a09b-120">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) package.</span></span>

## <a name="ihostedservice-interface"></a><span data-ttu-id="1a09b-121">Interface IHostedService</span><span class="sxs-lookup"><span data-stu-id="1a09b-121">IHostedService interface</span></span>

<span data-ttu-id="1a09b-122">Os serviços hospedados implementam a interface <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="1a09b-122">Hosted services implement the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="1a09b-123">A interface define dois métodos para objetos que são gerenciados pelo host:</span><span class="sxs-lookup"><span data-stu-id="1a09b-123">The interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="1a09b-124">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) - `StartAsync` contém a lógica para iniciar a tarefa em segundo plano.</span><span class="sxs-lookup"><span data-stu-id="1a09b-124">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) - `StartAsync` contains the logic to start the background task.</span></span> <span data-ttu-id="1a09b-125">Ao usar o [host Web](xref:fundamentals/host/web-host), `StartAsync` é chamado depois que o servidor é iniciado e [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) é disparado.</span><span class="sxs-lookup"><span data-stu-id="1a09b-125">When using the [Web Host](xref:fundamentals/host/web-host), `StartAsync` is called after the server has started and [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) is triggered.</span></span> <span data-ttu-id="1a09b-126">Ao usar o [host genérico](xref:fundamentals/host/generic-host), `StartAsync` é chamado antes de `ApplicationStarted` ser disparado.</span><span class="sxs-lookup"><span data-stu-id="1a09b-126">When using the [Generic Host](xref:fundamentals/host/generic-host), `StartAsync` is called before `ApplicationStarted` is triggered.</span></span>

* <span data-ttu-id="1a09b-127">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) – é disparado quando o host está executando um desligamento normal.</span><span class="sxs-lookup"><span data-stu-id="1a09b-127">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) - Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="1a09b-128">`StopAsync` contém a lógica para encerrar a tarefa em segundo plano e descartar todos os recursos não gerenciados.</span><span class="sxs-lookup"><span data-stu-id="1a09b-128">`StopAsync` contains the logic to end the background task and dispose of any unmanaged resources.</span></span> <span data-ttu-id="1a09b-129">Se o aplicativo for desligado inesperadamente (por exemplo, em uma falha do processo do aplicativo), `StopAsync` não poderá ser chamado.</span><span class="sxs-lookup"><span data-stu-id="1a09b-129">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span>

<span data-ttu-id="1a09b-130">O serviço hospedado é ativado uma única vez na inicialização do aplicativo e desligado normalmente durante o desligamento do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="1a09b-130">The hosted service is activated once at app startup and gracefully shutdown at app shutdown.</span></span> <span data-ttu-id="1a09b-131">Quando <xref:System.IDisposable> é implementado, os recursos podem ser descartados quando o contêiner de serviço é descartado.</span><span class="sxs-lookup"><span data-stu-id="1a09b-131">When <xref:System.IDisposable> is implemented, resources can be disposed when the service container is disposed.</span></span> <span data-ttu-id="1a09b-132">Se um erro for gerado durante a execução da tarefa em segundo plano, `Dispose` deverá ser chamado mesmo se `StopAsync` não for chamado.</span><span class="sxs-lookup"><span data-stu-id="1a09b-132">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="1a09b-133">Tarefas em segundo plano temporizadas</span><span class="sxs-lookup"><span data-stu-id="1a09b-133">Timed background tasks</span></span>

<span data-ttu-id="1a09b-134">Uma tarefa em segundo plano temporizada usa a classe [System.Threading.Timer](xref:System.Threading.Timer).</span><span class="sxs-lookup"><span data-stu-id="1a09b-134">A timed background task makes use of the [System.Threading.Timer](xref:System.Threading.Timer) class.</span></span> <span data-ttu-id="1a09b-135">O temporizador dispara o método `DoWork` da tarefa.</span><span class="sxs-lookup"><span data-stu-id="1a09b-135">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="1a09b-136">O temporizador é desabilitado em `StopAsync` e descartado quando o contêiner de serviço é descartado em `Dispose`:</span><span class="sxs-lookup"><span data-stu-id="1a09b-136">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

<span data-ttu-id="1a09b-137">O serviço está registrado em `Startup.ConfigureServices` com o método de extensão `AddHostedService`:</span><span class="sxs-lookup"><span data-stu-id="1a09b-137">The service is registered in `Startup.ConfigureServices` with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="1a09b-138">Consumindo um serviço com escopo em uma tarefa em segundo plano</span><span class="sxs-lookup"><span data-stu-id="1a09b-138">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="1a09b-139">Para usar os serviços com escopo dentro de um `IHostedService`, crie um escopo.</span><span class="sxs-lookup"><span data-stu-id="1a09b-139">To use scoped services within an `IHostedService`, create a scope.</span></span> <span data-ttu-id="1a09b-140">Por padrão, nenhum escopo é criado para um serviço hospedado.</span><span class="sxs-lookup"><span data-stu-id="1a09b-140">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="1a09b-141">O serviço da tarefa em segundo plano com escopo contém a lógica da tarefa em segundo plano.</span><span class="sxs-lookup"><span data-stu-id="1a09b-141">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="1a09b-142">No exemplo a seguir, um <xref:Microsoft.Extensions.Logging.ILogger> é injetado no serviço:</span><span class="sxs-lookup"><span data-stu-id="1a09b-142">In the following example, an <xref:Microsoft.Extensions.Logging.ILogger> is injected into the service:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="1a09b-143">O serviço hospedado cria um escopo para resolver o serviço da tarefa em segundo plano com escopo para chamar seu método `DoWork`:</span><span class="sxs-lookup"><span data-stu-id="1a09b-143">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

<span data-ttu-id="1a09b-144">Os serviços são registrados em `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="1a09b-144">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="1a09b-145">A implementação `IHostedService` é registrada com o método de extensão `AddHostedService`:</span><span class="sxs-lookup"><span data-stu-id="1a09b-145">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="1a09b-146">Tarefas em segundo plano na fila</span><span class="sxs-lookup"><span data-stu-id="1a09b-146">Queued background tasks</span></span>

<span data-ttu-id="1a09b-147">Uma fila de tarefas em segundo plano é baseada no <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> do .NET 4.x ([provisoriamente agendado para ser integrado ao ASP.NET Core 3.0](https://github.com/aspnet/Hosting/issues/1280)):</span><span class="sxs-lookup"><span data-stu-id="1a09b-147">A background task queue is based on the .NET 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([tentatively scheduled to be built-in for ASP.NET Core 3.0](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="1a09b-148">No `QueueHostedService`, as tarefas em segundo plano na fila são removidas da fila e executadas como um <xref:Microsoft.Extensions.Hosting.BackgroundService>, que é uma classe base para implementar um `IHostedService` de longa execução:</span><span class="sxs-lookup"><span data-stu-id="1a09b-148">In `QueueHostedService`, background tasks in the queue are dequeued and executed as a <xref:Microsoft.Extensions.Hosting.BackgroundService>, which is a base class for implementing a long running `IHostedService`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/QueuedHostedService.cs?name=snippet1&highlight=16,20)]

<span data-ttu-id="1a09b-149">Os serviços são registrados em `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="1a09b-149">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="1a09b-150">A implementação `IHostedService` é registrada com o método de extensão `AddHostedService`:</span><span class="sxs-lookup"><span data-stu-id="1a09b-150">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet3)]

<span data-ttu-id="1a09b-151">Na classe de modelo de página de índice, a `IBackgroundTaskQueue` é injetada no construtor e atribuída à `Queue`:</span><span class="sxs-lookup"><span data-stu-id="1a09b-151">In the Index page model class, the `IBackgroundTaskQueue` is injected into the constructor and assigned to `Queue`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="1a09b-152">Quando o botão **Adicionar Tarefa** é selecionado na página de índice, o método `OnPostAddTask` é executado.</span><span class="sxs-lookup"><span data-stu-id="1a09b-152">When the **Add Task** button is selected on the Index page, the `OnPostAddTask` method is executed.</span></span> <span data-ttu-id="1a09b-153">O `QueueBackgroundWorkItem` é chamado para enfileirar o item de trabalho:</span><span class="sxs-lookup"><span data-stu-id="1a09b-153">`QueueBackgroundWorkItem` is called to enqueue the work item:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Pages/Index.cshtml.cs?name=snippet2)]

## <a name="additional-resources"></a><span data-ttu-id="1a09b-154">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="1a09b-154">Additional resources</span></span>

* [<span data-ttu-id="1a09b-155">Implementar tarefas em segundo plano em microsserviços com IHostedService e a classe BackgroundService</span><span class="sxs-lookup"><span data-stu-id="1a09b-155">Implement background tasks in microservices with IHostedService and the BackgroundService class</span></span>](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* [<span data-ttu-id="1a09b-156">System.Threading.Timer</span><span class="sxs-lookup"><span data-stu-id="1a09b-156">System.Threading.Timer</span></span>](xref:System.Threading.Timer)

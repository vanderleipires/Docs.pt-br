---
title: Tarefas em segundo plano com serviços hospedados no ASP.NET Core
author: guardrex
description: Aprenda a implementar tarefas em segundo plano com serviços hospedados no ASP.NET Core.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 02/15/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/host/hosted-services
ms.openlocfilehash: cc39d125b639719599eca68d627fda014fb107e0
ms.sourcegitcommit: 466300d32f8c33e64ee1b419a2cbffe702863cdf
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2018
ms.locfileid: "34555294"
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a><span data-ttu-id="49ab4-103">Tarefas em segundo plano com serviços hospedados no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="49ab4-103">Background tasks with hosted services in ASP.NET Core</span></span>

<span data-ttu-id="49ab4-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="49ab4-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="49ab4-105">No ASP.NET Core, as tarefas em segundo plano podem ser implementadas como *serviços hospedados*.</span><span class="sxs-lookup"><span data-stu-id="49ab4-105">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="49ab4-106">Um serviço hospedado é uma classe com lógica de tarefa em segundo plano que implementa a interface [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice).</span><span class="sxs-lookup"><span data-stu-id="49ab4-106">A hosted service is a class with background task logic that implements the [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interface.</span></span> <span data-ttu-id="49ab4-107">Este tópico fornece três exemplos de serviço hospedado:</span><span class="sxs-lookup"><span data-stu-id="49ab4-107">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="49ab4-108">Tarefa em segundo plano que é executada com um temporizador.</span><span class="sxs-lookup"><span data-stu-id="49ab4-108">Background task that runs on a timer.</span></span>
* <span data-ttu-id="49ab4-109">Serviço hospedado que ativa um serviço com escopo.</span><span class="sxs-lookup"><span data-stu-id="49ab4-109">Hosted service that activates a scoped service.</span></span> <span data-ttu-id="49ab4-110">O serviço com escopo pode usar a injeção de dependência.</span><span class="sxs-lookup"><span data-stu-id="49ab4-110">The scoped service can use dependency injection.</span></span>
* <span data-ttu-id="49ab4-111">Tarefas em segundo plano na fila que são executadas sequencialmente.</span><span class="sxs-lookup"><span data-stu-id="49ab4-111">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="49ab4-112">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="49ab4-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="49ab4-113">Este aplicativo de exemplo é fornecido em duas versões:</span><span class="sxs-lookup"><span data-stu-id="49ab4-113">The sample app is provided in two versions:</span></span>

* <span data-ttu-id="49ab4-114">Host da Web &ndash; O Host da Web é útil para hospedar aplicativos web.</span><span class="sxs-lookup"><span data-stu-id="49ab4-114">Web Host &ndash; The Web Host is useful for hosting web apps.</span></span> <span data-ttu-id="49ab4-115">O código de exemplo mostrado neste tópico é da versão do host da web do exemplo.</span><span class="sxs-lookup"><span data-stu-id="49ab4-115">The example code shown in this topic is from the Web Host version of the sample.</span></span> <span data-ttu-id="49ab4-116">Para obter mais informações, consulte o tópico [Host da Web](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="49ab4-116">For more information, see the [Web Host](xref:fundamentals/host/web-host) topic.</span></span>
* <span data-ttu-id="49ab4-117">Host Genérico &ndash; O Host Genérico é novo no ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="49ab4-117">Generic Host &ndash; The Generic Host is new in ASP.NET Core 2.1.</span></span> <span data-ttu-id="49ab4-118">Para obter mais informações, confira o tópico [Host Genérico](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="49ab4-118">For more information, see the [Generic Host](xref:fundamentals/host/generic-host) topic.</span></span>

::: moniker-end

## <a name="ihostedservice-interface"></a><span data-ttu-id="49ab4-119">Interface IHostedService</span><span class="sxs-lookup"><span data-stu-id="49ab4-119">IHostedService interface</span></span>

<span data-ttu-id="49ab4-120">Os serviços hospedados implementam a interface [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice).</span><span class="sxs-lookup"><span data-stu-id="49ab4-120">Hosted services implement the [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interface.</span></span> <span data-ttu-id="49ab4-121">A interface define dois métodos para objetos que são gerenciados pelo host:</span><span class="sxs-lookup"><span data-stu-id="49ab4-121">The interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="49ab4-122">[StartAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) – chamado depois que o servidor é iniciado e [IApplicationLifetime.ApplicationStarted](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstarted) é disparado.</span><span class="sxs-lookup"><span data-stu-id="49ab4-122">[StartAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) - Called after the server has started and [IApplicationLifetime.ApplicationStarted](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstarted) is triggered.</span></span> <span data-ttu-id="49ab4-123">`StartAsync` contém a lógica para iniciar a tarefa em segundo plano.</span><span class="sxs-lookup"><span data-stu-id="49ab4-123">`StartAsync` contains the logic to start the background task.</span></span>

* <span data-ttu-id="49ab4-124">[StopAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) – é disparado quando o host está executando um desligamento normal.</span><span class="sxs-lookup"><span data-stu-id="49ab4-124">[StopAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) - Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="49ab4-125">`StopAsync` contém a lógica para encerrar a tarefa em segundo plano e descartar todos os recursos não gerenciados.</span><span class="sxs-lookup"><span data-stu-id="49ab4-125">`StopAsync` contains the logic to end the background task and dispose of any unmanaged resources.</span></span> <span data-ttu-id="49ab4-126">Se o aplicativo for desligado inesperadamente (por exemplo, em uma falha do processo do aplicativo), `StopAsync` não poderá ser chamado.</span><span class="sxs-lookup"><span data-stu-id="49ab4-126">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span>

<span data-ttu-id="49ab4-127">O serviço hospedado é um singleton que é ativado uma única vez na inicialização do aplicativo e desligado normalmente durante o desligamento do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="49ab4-127">The hosted service is a singleton that's activated once at app startup and gracefully shutdown at app shutdown.</span></span> <span data-ttu-id="49ab4-128">Quando [IDisposable](/dotnet/api/system.idisposable) está implementado, os recursos podem ser descartados quando o contêiner de serviço é descartado.</span><span class="sxs-lookup"><span data-stu-id="49ab4-128">When [IDisposable](/dotnet/api/system.idisposable) is implemented, resources can be disposed when the service container is disposed.</span></span> <span data-ttu-id="49ab4-129">Se um erro for gerado durante a execução da tarefa em segundo plano, `Dispose` deverá ser chamado mesmo se `StopAsync` não for chamado.</span><span class="sxs-lookup"><span data-stu-id="49ab4-129">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="49ab4-130">Tarefas em segundo plano temporizadas</span><span class="sxs-lookup"><span data-stu-id="49ab4-130">Timed background tasks</span></span>

<span data-ttu-id="49ab4-131">Uma tarefa em segundo plano temporizada usa a classe [System.Threading.Timer](/dotnet/api/system.threading.timer).</span><span class="sxs-lookup"><span data-stu-id="49ab4-131">A timed background task makes use of the [System.Threading.Timer](/dotnet/api/system.threading.timer) class.</span></span> <span data-ttu-id="49ab4-132">O temporizador dispara o método `DoWork` da tarefa.</span><span class="sxs-lookup"><span data-stu-id="49ab4-132">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="49ab4-133">O temporizador é desabilitado em `StopAsync` e descartado quando o contêiner de serviço é descartado em `Dispose`:</span><span class="sxs-lookup"><span data-stu-id="49ab4-133">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

<span data-ttu-id="49ab4-134">O serviço é registrado em `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="49ab4-134">The service is registered in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="49ab4-135">Consumindo um serviço com escopo em uma tarefa em segundo plano</span><span class="sxs-lookup"><span data-stu-id="49ab4-135">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="49ab4-136">Para usar os serviços com escopo dentro de um `IHostedService`, crie um escopo.</span><span class="sxs-lookup"><span data-stu-id="49ab4-136">To use scoped services within an `IHostedService`, create a scope.</span></span> <span data-ttu-id="49ab4-137">Por padrão, nenhum escopo é criado para um serviço hospedado.</span><span class="sxs-lookup"><span data-stu-id="49ab4-137">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="49ab4-138">O serviço da tarefa em segundo plano com escopo contém a lógica da tarefa em segundo plano.</span><span class="sxs-lookup"><span data-stu-id="49ab4-138">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="49ab4-139">No exemplo a seguir, [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) é injetado no serviço:</span><span class="sxs-lookup"><span data-stu-id="49ab4-139">In the following example, [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) is injected into the service:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="49ab4-140">O serviço hospedado cria um escopo para resolver o serviço da tarefa em segundo plano com escopo para chamar seu método `DoWork`:</span><span class="sxs-lookup"><span data-stu-id="49ab4-140">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

<span data-ttu-id="49ab4-141">Os serviços são registrados em `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="49ab4-141">The services are registered in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="49ab4-142">Tarefas em segundo plano na fila</span><span class="sxs-lookup"><span data-stu-id="49ab4-142">Queued background tasks</span></span>

<span data-ttu-id="49ab4-143">Uma fila de tarefas em segundo plano é baseada no [QueueBackgroundWorkItem](/dotnet/api/system.web.hosting.hostingenvironment.queuebackgroundworkitem) do .NET 4.x ([provisoriamente agendado para ser interno no ASP.NET Core 2.2](https://github.com/aspnet/Hosting/issues/1280)):</span><span class="sxs-lookup"><span data-stu-id="49ab4-143">A background task queue is based on the .NET 4.x [QueueBackgroundWorkItem](/dotnet/api/system.web.hosting.hostingenvironment.queuebackgroundworkitem) ([tentatively scheduled to be built-in for ASP.NET Core 2.2](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="49ab4-144">No `QueueHostedService`, as tarefas em segundo plano (`workItem`) na fila são removidas da fila e executadas:</span><span class="sxs-lookup"><span data-stu-id="49ab4-144">In `QueueHostedService`, background tasks (`workItem`) in the queue are dequeued and executed:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/QueuedHostedService.cs?name=snippet1&highlight=30-31,35)]

<span data-ttu-id="49ab4-145">Os serviços são registrados em `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="49ab4-145">The services are registered in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet3)]

<span data-ttu-id="49ab4-146">Na classe de modelo de página de índice, a `IBackgroundTaskQueue` é injetada no construtor e atribuída à `Queue`:</span><span class="sxs-lookup"><span data-stu-id="49ab4-146">In the Index page model class, the `IBackgroundTaskQueue` is injected into the constructor and assigned to `Queue`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="49ab4-147">Quando o botão **Adicionar Tarefa** é selecionado na página de índice, o método `OnPostAddTask` é executado.</span><span class="sxs-lookup"><span data-stu-id="49ab4-147">When the **Add Task** button is selected on the Index page, the `OnPostAddTask` method is executed.</span></span> <span data-ttu-id="49ab4-148">O `QueueBackgroundWorkItem` é chamado para enfileirar o item de trabalho:</span><span class="sxs-lookup"><span data-stu-id="49ab4-148">`QueueBackgroundWorkItem` is called to enqueue the work item:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Pages/Index.cshtml.cs?name=snippet2)]

## <a name="additional-resources"></a><span data-ttu-id="49ab4-149">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="49ab4-149">Additional resources</span></span>

* [<span data-ttu-id="49ab4-150">Implementar tarefas em segundo plano em microsserviços com IHostedService e a classe BackgroundService</span><span class="sxs-lookup"><span data-stu-id="49ab4-150">Implement background tasks in microservices with IHostedService and the BackgroundService class</span></span>](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* [<span data-ttu-id="49ab4-151">System.Threading.Timer</span><span class="sxs-lookup"><span data-stu-id="49ab4-151">System.Threading.Timer</span></span>](/dotnet/api/system.threading.timer)

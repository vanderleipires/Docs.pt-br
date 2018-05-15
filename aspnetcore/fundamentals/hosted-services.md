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
uid: fundamentals/hosted-services
ms.openlocfilehash: 89e595fb6ef38745d7377fdaaf1780c64320efe3
ms.sourcegitcommit: d43c84c4c80527c85e49d53691b293669557a79d
ms.contentlocale: pt-BR
ms.lasthandoff: 02/20/2018
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a><span data-ttu-id="04191-103">Tarefas em segundo plano com serviços hospedados no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="04191-103">Background tasks with hosted services in ASP.NET Core</span></span>

<span data-ttu-id="04191-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="04191-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="04191-105">No ASP.NET Core, as tarefas em segundo plano podem ser implementadas como *serviços hospedados*.</span><span class="sxs-lookup"><span data-stu-id="04191-105">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="04191-106">Um serviço hospedado é uma classe com lógica de tarefa em segundo plano que implementa a interface [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice).</span><span class="sxs-lookup"><span data-stu-id="04191-106">A hosted service is a class with background task logic that implements the [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interface.</span></span> <span data-ttu-id="04191-107">Este tópico fornece três exemplos de serviço hospedado:</span><span class="sxs-lookup"><span data-stu-id="04191-107">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="04191-108">Tarefa em segundo plano que é executada com um temporizador.</span><span class="sxs-lookup"><span data-stu-id="04191-108">Background task that runs on a timer.</span></span>
* <span data-ttu-id="04191-109">Serviço hospedado que ativa um serviço com escopo.</span><span class="sxs-lookup"><span data-stu-id="04191-109">Hosted service that activates a scoped service.</span></span> <span data-ttu-id="04191-110">O serviço com escopo pode usar a injeção de dependência.</span><span class="sxs-lookup"><span data-stu-id="04191-110">The scoped service can use dependency injection.</span></span>
* <span data-ttu-id="04191-111">Tarefas em segundo plano na fila que são executadas sequencialmente.</span><span class="sxs-lookup"><span data-stu-id="04191-111">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="04191-112">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/hosted-services/samples/2.x) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="04191-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/hosted-services/samples/2.x) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="ihostedservice-interface"></a><span data-ttu-id="04191-113">Interface IHostedService</span><span class="sxs-lookup"><span data-stu-id="04191-113">IHostedService interface</span></span>

<span data-ttu-id="04191-114">Os serviços hospedados implementam a interface [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice).</span><span class="sxs-lookup"><span data-stu-id="04191-114">Hosted services implement the [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interface.</span></span> <span data-ttu-id="04191-115">A interface define dois métodos para objetos que são gerenciados pelo host:</span><span class="sxs-lookup"><span data-stu-id="04191-115">The interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="04191-116">[StartAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) – chamado depois que o servidor é iniciado e [IApplicationLifetime.ApplicationStarted](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstarted) é disparado.</span><span class="sxs-lookup"><span data-stu-id="04191-116">[StartAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) - Called after the server has started and [IApplicationLifetime.ApplicationStarted](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstarted) is triggered.</span></span> <span data-ttu-id="04191-117">`StartAsync` contém a lógica para iniciar a tarefa em segundo plano.</span><span class="sxs-lookup"><span data-stu-id="04191-117">`StartAsync` contains the logic to start the background task.</span></span>

* <span data-ttu-id="04191-118">[StopAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) – é disparado quando o host está executando um desligamento normal.</span><span class="sxs-lookup"><span data-stu-id="04191-118">[StopAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) - Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="04191-119">`StopAsync` contém a lógica para encerrar a tarefa em segundo plano e descartar todos os recursos não gerenciados.</span><span class="sxs-lookup"><span data-stu-id="04191-119">`StopAsync` contains the logic to end the background task and dispose of any unmanaged resources.</span></span> <span data-ttu-id="04191-120">Se o aplicativo for desligado inesperadamente (por exemplo, em uma falha do processo do aplicativo), `StopAsync` não poderá ser chamado.</span><span class="sxs-lookup"><span data-stu-id="04191-120">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span>

<span data-ttu-id="04191-121">O serviço hospedado é um singleton que é ativado uma única vez na inicialização do aplicativo e desligado normalmente durante o desligamento do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="04191-121">The hosted service is a singleton that's activated once at app startup and gracefully shutdown at app shutdown.</span></span> <span data-ttu-id="04191-122">Quando [IDisposable](/dotnet/api/system.idisposable) está implementado, os recursos podem ser descartados quando o contêiner de serviço é descartado.</span><span class="sxs-lookup"><span data-stu-id="04191-122">When [IDisposable](/dotnet/api/system.idisposable) is implemented, resources can be disposed when the service container is disposed.</span></span> <span data-ttu-id="04191-123">Se um erro for gerado durante a execução da tarefa em segundo plano, `Dispose` deverá ser chamado mesmo se `StopAsync` não for chamado.</span><span class="sxs-lookup"><span data-stu-id="04191-123">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="04191-124">Tarefas em segundo plano temporizadas</span><span class="sxs-lookup"><span data-stu-id="04191-124">Timed background tasks</span></span>

<span data-ttu-id="04191-125">Uma tarefa em segundo plano temporizada usa a classe [System.Threading.Timer](/dotnet/api/system.threading.timer).</span><span class="sxs-lookup"><span data-stu-id="04191-125">A timed background task makes use of the [System.Threading.Timer](/dotnet/api/system.threading.timer) class.</span></span> <span data-ttu-id="04191-126">O temporizador dispara o método `DoWork` da tarefa.</span><span class="sxs-lookup"><span data-stu-id="04191-126">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="04191-127">O temporizador é desabilitado em `StopAsync` e descartado quando o contêiner de serviço é descartado em `Dispose`:</span><span class="sxs-lookup"><span data-stu-id="04191-127">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

<span data-ttu-id="04191-128">O serviço é registrado em `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="04191-128">The service is registered in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/Startup.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="04191-129">Consumindo um serviço com escopo em uma tarefa em segundo plano</span><span class="sxs-lookup"><span data-stu-id="04191-129">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="04191-130">Para usar os serviços com escopo dentro de um `IHostedService`, crie um escopo.</span><span class="sxs-lookup"><span data-stu-id="04191-130">To use scoped services within an `IHostedService`, create a scope.</span></span> <span data-ttu-id="04191-131">Por padrão, nenhum escopo é criado para um serviço hospedado.</span><span class="sxs-lookup"><span data-stu-id="04191-131">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="04191-132">O serviço da tarefa em segundo plano com escopo contém a lógica da tarefa em segundo plano.</span><span class="sxs-lookup"><span data-stu-id="04191-132">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="04191-133">No exemplo a seguir, [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) é injetado no serviço:</span><span class="sxs-lookup"><span data-stu-id="04191-133">In the following example, [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) is injected into the service:</span></span>

[!code-csharp[](hosted-services/samples/2.x/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="04191-134">O serviço hospedado cria um escopo para resolver o serviço da tarefa em segundo plano com escopo para chamar seu método `DoWork`:</span><span class="sxs-lookup"><span data-stu-id="04191-134">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

<span data-ttu-id="04191-135">Os serviços são registrados em `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="04191-135">The services are registered in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/Startup.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="04191-136">Tarefas em segundo plano na fila</span><span class="sxs-lookup"><span data-stu-id="04191-136">Queued background tasks</span></span>

<span data-ttu-id="04191-137">Uma fila de tarefas em segundo plano é baseada no [QueueBackgroundWorkItem](/dotnet/api/system.web.hosting.hostingenvironment.queuebackgroundworkitem) do .NET 4.x ([provisoriamente agendado para ser interno no ASP.NET Core 2.2](https://github.com/aspnet/Hosting/issues/1280)):</span><span class="sxs-lookup"><span data-stu-id="04191-137">A background task queue is based on the .NET 4.x [QueueBackgroundWorkItem](/dotnet/api/system.web.hosting.hostingenvironment.queuebackgroundworkitem) ([tentatively scheduled to be built-in for ASP.NET Core 2.2](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/2.x/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="04191-138">No `QueueHostedService`, as tarefas em segundo plano (`workItem`) na fila são removidas da fila e executadas:</span><span class="sxs-lookup"><span data-stu-id="04191-138">In `QueueHostedService`, background tasks (`workItem`) in the queue are dequeued and executed:</span></span>

[!code-csharp[](hosted-services/samples/2.x/Services/QueuedHostedService.cs?name=snippet1&highlight=30-31,35)]

<span data-ttu-id="04191-139">Os serviços são registrados em `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="04191-139">The services are registered in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/Startup.cs?name=snippet3)]

<span data-ttu-id="04191-140">Na classe de modelo de página de índice, a `IBackgroundTaskQueue` é injetada no construtor e atribuída à `Queue`:</span><span class="sxs-lookup"><span data-stu-id="04191-140">In the Index page model class, the `IBackgroundTaskQueue` is injected into the constructor and assigned to `Queue`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="04191-141">Quando o botão **Adicionar Tarefa** é selecionado na página de índice, o método `OnPostAddTask` é executado.</span><span class="sxs-lookup"><span data-stu-id="04191-141">When the **Add Task** button is selected on the Index page, the `OnPostAddTask` method is executed.</span></span> <span data-ttu-id="04191-142">O `QueueBackgroundWorkItem` é chamado para enfileirar o item de trabalho:</span><span class="sxs-lookup"><span data-stu-id="04191-142">`QueueBackgroundWorkItem` is called to enqueue the work item:</span></span>

[!code-csharp[](hosted-services/samples/2.x/Pages/Index.cshtml.cs?name=snippet2)]

## <a name="additional-resources"></a><span data-ttu-id="04191-143">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="04191-143">Additional resources</span></span>

* [<span data-ttu-id="04191-144">Implementar tarefas em segundo plano em microsserviços com IHostedService e a classe BackgroundService</span><span class="sxs-lookup"><span data-stu-id="04191-144">Implement background tasks in microservices with IHostedService and the BackgroundService class</span></span>](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* [<span data-ttu-id="04191-145">System.Threading.Timer</span><span class="sxs-lookup"><span data-stu-id="04191-145">System.Threading.Timer</span></span>](/dotnet/api/system.threading.timer)

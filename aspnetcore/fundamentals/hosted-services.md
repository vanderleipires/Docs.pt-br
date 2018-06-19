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
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/20/2018
ms.locfileid: "29374685"
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a>Tarefas em segundo plano com serviços hospedados no ASP.NET Core

Por [Luke Latham](https://github.com/guardrex)

No ASP.NET Core, as tarefas em segundo plano podem ser implementadas como *serviços hospedados*. Um serviço hospedado é uma classe com lógica de tarefa em segundo plano que implementa a interface [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice). Este tópico fornece três exemplos de serviço hospedado:

* Tarefa em segundo plano que é executada com um temporizador.
* Serviço hospedado que ativa um serviço com escopo. O serviço com escopo pode usar a injeção de dependência.
* Tarefas em segundo plano na fila que são executadas sequencialmente.

[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/hosted-services/samples/2.x) ([como baixar](xref:tutorials/index#how-to-download-a-sample))

## <a name="ihostedservice-interface"></a>Interface IHostedService

Os serviços hospedados implementam a interface [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice). A interface define dois métodos para objetos que são gerenciados pelo host:

* [StartAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) – chamado depois que o servidor é iniciado e [IApplicationLifetime.ApplicationStarted](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstarted) é disparado. `StartAsync` contém a lógica para iniciar a tarefa em segundo plano.

* [StopAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) – é disparado quando o host está executando um desligamento normal. `StopAsync` contém a lógica para encerrar a tarefa em segundo plano e descartar todos os recursos não gerenciados. Se o aplicativo for desligado inesperadamente (por exemplo, em uma falha do processo do aplicativo), `StopAsync` não poderá ser chamado.

O serviço hospedado é um singleton que é ativado uma única vez na inicialização do aplicativo e desligado normalmente durante o desligamento do aplicativo. Quando [IDisposable](/dotnet/api/system.idisposable) está implementado, os recursos podem ser descartados quando o contêiner de serviço é descartado. Se um erro for gerado durante a execução da tarefa em segundo plano, `Dispose` deverá ser chamado mesmo se `StopAsync` não for chamado.

## <a name="timed-background-tasks"></a>Tarefas em segundo plano temporizadas

Uma tarefa em segundo plano temporizada usa a classe [System.Threading.Timer](/dotnet/api/system.threading.timer). O temporizador dispara o método `DoWork` da tarefa. O temporizador é desabilitado em `StopAsync` e descartado quando o contêiner de serviço é descartado em `Dispose`:

[!code-csharp[](hosted-services/samples/2.x/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

O serviço é registrado em `Startup.ConfigureServices`:

[!code-csharp[](hosted-services/samples/2.x/Startup.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a>Consumindo um serviço com escopo em uma tarefa em segundo plano

Para usar os serviços com escopo dentro de um `IHostedService`, crie um escopo. Por padrão, nenhum escopo é criado para um serviço hospedado.

O serviço da tarefa em segundo plano com escopo contém a lógica da tarefa em segundo plano. No exemplo a seguir, [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) é injetado no serviço:

[!code-csharp[](hosted-services/samples/2.x/Services/ScopedProcessingService.cs?name=snippet1)]

O serviço hospedado cria um escopo para resolver o serviço da tarefa em segundo plano com escopo para chamar seu método `DoWork`:

[!code-csharp[](hosted-services/samples/2.x/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

Os serviços são registrados em `Startup.ConfigureServices`:

[!code-csharp[](hosted-services/samples/2.x/Startup.cs?name=snippet2)]

## <a name="queued-background-tasks"></a>Tarefas em segundo plano na fila

Uma fila de tarefas em segundo plano é baseada no [QueueBackgroundWorkItem](/dotnet/api/system.web.hosting.hostingenvironment.queuebackgroundworkitem) do .NET 4.x ([provisoriamente agendado para ser interno no ASP.NET Core 2.2](https://github.com/aspnet/Hosting/issues/1280)):

[!code-csharp[](hosted-services/samples/2.x/Services/BackgroundTaskQueue.cs?name=snippet1)]

No `QueueHostedService`, as tarefas em segundo plano (`workItem`) na fila são removidas da fila e executadas:

[!code-csharp[](hosted-services/samples/2.x/Services/QueuedHostedService.cs?name=snippet1&highlight=30-31,35)]

Os serviços são registrados em `Startup.ConfigureServices`:

[!code-csharp[](hosted-services/samples/2.x/Startup.cs?name=snippet3)]

Na classe de modelo de página de índice, a `IBackgroundTaskQueue` é injetada no construtor e atribuída à `Queue`:

[!code-csharp[](hosted-services/samples/2.x/Pages/Index.cshtml.cs?name=snippet1)]

Quando o botão **Adicionar Tarefa** é selecionado na página de índice, o método `OnPostAddTask` é executado. O `QueueBackgroundWorkItem` é chamado para enfileirar o item de trabalho:

[!code-csharp[](hosted-services/samples/2.x/Pages/Index.cshtml.cs?name=snippet2)]

## <a name="additional-resources"></a>Recursos adicionais

* [Implementar tarefas em segundo plano em microsserviços com IHostedService e a classe BackgroundService](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* [System.Threading.Timer](/dotnet/api/system.threading.timer)

---
title: "Registro em log no núcleo do ASP.NET"
author: ardalis
description: "Apresenta a estrutura de registro do ASP.NET Core. Inclui uma seção para cada provedor de logs interno e links para alguns provedores de terceiros populares."
keywords: Os escopos do ASP.NET Core, registro em log, provedores de log, Microsoft.Extensions.Logging, ILogger, ILoggerFactory, LogLevel, WithFilter, TraceSource, log de eventos, EventSource,
ms.author: tdykstra
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: ac27ac68-d76a-4f8e-b8ab-ea045803e5f2
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/logging
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 9e5c97ce868e281310aa75c16e73298e2aaa0d9d
ms.sourcegitcommit: bd05f7ea8f87ad076ef6e8b704698ebcba5ca80c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/25/2017
---
# <a name="introduction-to-logging-in-aspnet-core"></a><span data-ttu-id="9f6a8-105">Introdução ao registro em log no núcleo do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="9f6a8-105">Introduction to Logging in ASP.NET Core</span></span>

<span data-ttu-id="9f6a8-106">Por [Steve Smith](http://ardalis.com) e [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="9f6a8-106">By [Steve Smith](http://ardalis.com) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="9f6a8-107">ASP.NET Core dá suporte a uma API de registro em log que funciona com uma variedade de provedores de log.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-107">ASP.NET Core supports a logging API that works with a variety of logging providers.</span></span> <span data-ttu-id="9f6a8-108">Provedores internos permitem que você envie logs para um ou mais destinos, e você pode conectar uma estrutura de log de terceiros.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-108">Built-in providers let you send logs to one or more destinations, and you can plug in a third-party logging framework.</span></span> <span data-ttu-id="9f6a8-109">Este artigo mostra como usar a API de registro em log internos e provedores em seu código.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-109">This article shows how to use the built-in logging API and providers in your code.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9f6a8-110">ASP.NET Core 2. x</span><span class="sxs-lookup"><span data-stu-id="9f6a8-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[<span data-ttu-id="9f6a8-111">Exibir ou baixar o código de exemplo</span><span class="sxs-lookup"><span data-stu-id="9f6a8-111">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/sample2)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9f6a8-112">ASP.NET Core 1. x</span><span class="sxs-lookup"><span data-stu-id="9f6a8-112">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[<span data-ttu-id="9f6a8-113">Exibir ou baixar o código de exemplo</span><span class="sxs-lookup"><span data-stu-id="9f6a8-113">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/sample)

---

## <a name="how-to-create-logs"></a><span data-ttu-id="9f6a8-114">Como criar logs</span><span class="sxs-lookup"><span data-stu-id="9f6a8-114">How to create logs</span></span>

<span data-ttu-id="9f6a8-115">Para criar logs, obter um `ILogger` de objeto o [injeção de dependência](dependency-injection.md) contêiner:</span><span class="sxs-lookup"><span data-stu-id="9f6a8-115">To create logs, get an `ILogger` object from the [dependency injection](dependency-injection.md) container:</span></span>

[!code-csharp[](logging/sample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

<span data-ttu-id="9f6a8-116">Em seguida, chame métodos de registro em log nesse objeto de agente de log:</span><span class="sxs-lookup"><span data-stu-id="9f6a8-116">Then call logging methods on that logger object:</span></span>

[!code-csharp[](logging/sample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="9f6a8-117">Este exemplo cria logs com o `TodoController` classe como o *categoria*.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-117">This example creates logs with the `TodoController` class as the *category*.</span></span>  <span data-ttu-id="9f6a8-118">Categorias são explicadas [posteriormente neste artigo](#log-category).</span><span class="sxs-lookup"><span data-stu-id="9f6a8-118">Categories are explained [later in this article](#log-category).</span></span>

<span data-ttu-id="9f6a8-119">ASP.NET Core não fornece async métodos do agente porque o log deve ser tão rápido que ela não vale a pena o custo do uso de async.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-119">ASP.NET Core does not provide async logger methods because logging should be so fast that it isn't worth the cost of using async.</span></span> <span data-ttu-id="9f6a8-120">Se você estiver em uma situação em que não é true, considere alterar a maneira de que fazer logon.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-120">If you're in a situation where that's not true, consider changing the way you log.</span></span>  <span data-ttu-id="9f6a8-121">Se o repositório de dados estiver lento, gravar as mensagens de log em um repositório rápido primeiro e movê-los para um repositório lenta mais tarde.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-121">If your data store is slow, write the log messages to a fast store first, then move them to a slow store later.</span></span> <span data-ttu-id="9f6a8-122">Por exemplo, faça uma fila de mensagens que é lida e persistida para armazenamento lento por outro processo.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-122">For example, log to a message queue that is read and persisted to slow storage by another process.</span></span>

## <a name="how-to-add-providers"></a><span data-ttu-id="9f6a8-123">Como adicionar provedores</span><span class="sxs-lookup"><span data-stu-id="9f6a8-123">How to add providers</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9f6a8-124">ASP.NET Core 2. x</span><span class="sxs-lookup"><span data-stu-id="9f6a8-124">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="9f6a8-125">Um provedor de log leva as mensagens que você criar com um `ILogger` do objeto, exibe e armazena-os.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-125">A logging provider takes the messages that you create with an `ILogger` object and displays or stores them.</span></span> <span data-ttu-id="9f6a8-126">Por exemplo, o provedor de Console exibe mensagens no console, e o provedor de serviço de aplicativo do Azure pode armazená-los no armazenamento de BLOBs do Azure.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-126">For example, the Console provider displays messages on the console, and the Azure App Service provider can store them in Azure blob storage.</span></span>

<span data-ttu-id="9f6a8-127">Para usar um provedor, chamar o provedor `Add<ProviderName>` método de extensão no *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="9f6a8-127">To use a provider, call the provider's `Add<ProviderName>` extension method in *Program.cs*:</span></span>

[!code-csharp[](logging/sample2/Program.cs?name=snippet_ExpandDefault&highlight=16,17)]

<span data-ttu-id="9f6a8-128">O modelo de projeto padrão define o registro em log da maneira que você vê-lo no código anterior, mas o `ConfigureLogging` chamada é feita pelo `CreateDefaultBuilder` método.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-128">The default project template sets up logging the way you see it in the preceding code, but the `ConfigureLogging` call is done by the `CreateDefaultBuilder` method.</span></span> <span data-ttu-id="9f6a8-129">Aqui está o código em *Program.cs* que são criadas por modelos de projeto:</span><span class="sxs-lookup"><span data-stu-id="9f6a8-129">Here's the code in *Program.cs* that is created by project templates:</span></span>

[!code-csharp[](logging/sample2/Program.cs?name=snippet_TemplateCode&highlight=7)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9f6a8-130">ASP.NET Core 1. x</span><span class="sxs-lookup"><span data-stu-id="9f6a8-130">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="9f6a8-131">Um provedor de log leva as mensagens que você criar com um `ILogger` do objeto, exibe e armazena-os.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-131">A logging provider takes the messages that you create with an `ILogger` object and displays or stores them.</span></span> <span data-ttu-id="9f6a8-132">Por exemplo, o provedor de Console exibe mensagens no console, e o provedor de serviço de aplicativo do Azure pode armazená-los no armazenamento de BLOBs do Azure.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-132">For example, the Console provider displays messages on the console, and the Azure App Service provider can store them in Azure blob storage.</span></span>

<span data-ttu-id="9f6a8-133">Para usar um provedor, instale o pacote do NuGet e chame o método de extensão do provedor em uma instância do `ILoggerFactory`, conforme mostrado no exemplo a seguir.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-133">To use a provider, install its NuGet package and call the provider's extension method on an instance of `ILoggerFactory`, as shown in the following example.</span></span>

[!code-csharp[](logging/sample//Startup.cs?name=snippet_AddConsoleAndDebug&highlight=3,5-7)]

<span data-ttu-id="9f6a8-134">ASP.NET Core [injeção de dependência](dependency-injection.md) (DI) fornece o `ILoggerFactory` instância.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-134">ASP.NET Core [dependency injection](dependency-injection.md) (DI) provides the `ILoggerFactory` instance.</span></span> <span data-ttu-id="9f6a8-135">O `AddConsole` e `AddDebug` métodos de extensão são definidos no [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) e [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/) pacotes.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-135">The `AddConsole` and `AddDebug` extension methods are defined in the [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) and [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/) packages.</span></span> <span data-ttu-id="9f6a8-136">Cada método de extensão chama o `ILoggerFactory.AddProvider` método, passando uma instância do provedor.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-136">Each extension method calls the `ILoggerFactory.AddProvider` method, passing in an instance of the provider.</span></span> 

> [!NOTE]
> <span data-ttu-id="9f6a8-137">O aplicativo de exemplo para este artigo adiciona provedores de log no `Configure` método o `Startup` classe.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-137">The sample application for this article adds logging providers in the `Configure` method of the `Startup` class.</span></span> <span data-ttu-id="9f6a8-138">Se você quiser obter saída de log do código que é executado anteriormente, adicionar provedores de log no `Startup` em vez disso, o construtor de classe.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-138">If you want to get log output from code that executes earlier, add logging providers in the `Startup` class constructor instead.</span></span> 

---

<span data-ttu-id="9f6a8-139">Você encontrará informações sobre cada [provedor de logs interno](#built-in-logging-providers) e links para [provedores de log de terceiros](#third-party-logging-providers) posteriormente no artigo.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-139">You'll find information about each [built-in logging provider](#built-in-logging-providers) and links to [third-party logging providers](#third-party-logging-providers) later in the article.</span></span>

## <a name="sample-logging-output"></a><span data-ttu-id="9f6a8-140">Exemplo de saída de log</span><span class="sxs-lookup"><span data-stu-id="9f6a8-140">Sample logging output</span></span>

<span data-ttu-id="9f6a8-141">Com o código de exemplo mostrado na seção anterior, você verá logs no console quando executado na linha de comando.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-141">With the sample code shown in the preceding section, you'll see logs in the console when you run from the command line.</span></span> <span data-ttu-id="9f6a8-142">Aqui está um exemplo de saída do console:</span><span class="sxs-lookup"><span data-stu-id="9f6a8-142">Here's an example of console output:</span></span>

```console
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[1]
      Request starting HTTP/1.1 GET http://localhost:5000/api/todo/0
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[1]
      Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
info: TodoApi.Controllers.TodoController[1002]
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      GetById(0) NOT FOUND
info: Microsoft.AspNetCore.Mvc.StatusCodeResult[1]
      Executing HttpStatusCodeResult, setting HTTP status code 404
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[2]
      Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 42.9286ms
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[2]
      Request finished in 148.889ms 404
```
 
<span data-ttu-id="9f6a8-143">Esses logs criados acessando `http://localhost:5000/api/todo/0`, que dispara a execução de ambos `ILogger` chamadas mostradas na seção anterior.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-143">These logs were created by going to `http://localhost:5000/api/todo/0`, which triggers execution of both `ILogger` calls shown in the preceding section.</span></span>

<span data-ttu-id="9f6a8-144">Aqui está um exemplo de como os mesmos logs que aparecem na janela de depuração quando você executar o aplicativo de exemplo no Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="9f6a8-144">Here's an example of the same logs as they appear in the Debug window when you run the sample application in Visual Studio:</span></span>

```console
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request starting HTTP/1.1 GET http://localhost:53104/api/todo/0  
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
TodoApi.Controllers.TodoController:Information: Getting item 0
TodoApi.Controllers.TodoController:Warning: GetById(0) NOT FOUND
Microsoft.AspNetCore.Mvc.StatusCodeResult:Information: Executing HttpStatusCodeResult, setting HTTP status code 404
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 152.5657ms
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request finished in 316.3195ms 404 
```

<span data-ttu-id="9f6a8-145">Os logs criados pelo `ILogger` chamadas mostradas na seção anterior começam com "TodoApi.Controllers.TodoController".</span><span class="sxs-lookup"><span data-stu-id="9f6a8-145">The logs that were created by the `ILogger` calls shown in the preceding section begin with "TodoApi.Controllers.TodoController".</span></span> <span data-ttu-id="9f6a8-146">Os logs que começam com categorias de "Microsoft" são do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-146">The logs that begin with "Microsoft" categories are from ASP.NET Core.</span></span> <span data-ttu-id="9f6a8-147">ASP.NET Core em si e o código do aplicativo estão usando a mesma API de registro em log e os mesmos provedores de log.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-147">ASP.NET Core itself and your application code are using the same logging API and the same logging providers.</span></span>

<span data-ttu-id="9f6a8-148">O restante deste artigo explica alguns detalhes e as opções de log.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-148">The remainder of this article explains some details and options for logging.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="9f6a8-149">Pacotes NuGet</span><span class="sxs-lookup"><span data-stu-id="9f6a8-149">NuGet packages</span></span>

<span data-ttu-id="9f6a8-150">O `ILogger` e `ILoggerFactory` interfaces estão em [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), e implementações padrão para que eles estão em [Microsoft.Extensions.Logging](https://www.nuget.org/packages/Microsoft.Extensions.Logging/).</span><span class="sxs-lookup"><span data-stu-id="9f6a8-150">The `ILogger` and `ILoggerFactory` interfaces are in [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), and default implementations for them are in [Microsoft.Extensions.Logging](https://www.nuget.org/packages/Microsoft.Extensions.Logging/).</span></span>

## <a name="log-category"></a><span data-ttu-id="9f6a8-151">Categoria de log</span><span class="sxs-lookup"><span data-stu-id="9f6a8-151">Log category</span></span>

<span data-ttu-id="9f6a8-152">Um *categoria* é incluído com cada log que você criar.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-152">A *category* is included with each log that you create.</span></span>  <span data-ttu-id="9f6a8-153">Especificar a categoria quando você cria um `ILogger` objeto.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-153">You specify the category when you create an `ILogger` object.</span></span> <span data-ttu-id="9f6a8-154">A categoria pode ser qualquer cadeia de caracteres, mas uma convenção é usar o nome totalmente qualificado da classe da qual os logs são gravados.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-154">The category may be any string, but a convention is to use the fully qualified name of the class from which the logs are written.</span></span>  <span data-ttu-id="9f6a8-155">Por exemplo: "TodoApi.Controllers.TodoController".</span><span class="sxs-lookup"><span data-stu-id="9f6a8-155">For example: "TodoApi.Controllers.TodoController".</span></span>

<span data-ttu-id="9f6a8-156">Você pode especificar a categoria como uma cadeia de caracteres ou usar um método de extensão que é derivado da categoria do tipo.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-156">You can specify the category as a string or use an extension method that derives the category from the type.</span></span> <span data-ttu-id="9f6a8-157">Para especificar a categoria como uma cadeia de caracteres, chamar `CreateLogger` em um `ILoggerFactory` de instância, conforme mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-157">To specify the category as a string, call `CreateLogger` on an `ILoggerFactory` instance, as shown below.</span></span>

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

<span data-ttu-id="9f6a8-158">Na maioria das vezes, é mais fácil de usar `ILogger<T>`, conforme mostrado no exemplo a seguir.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-158">Most of the time, it will be easier to use  `ILogger<T>`, as in the following example.</span></span>

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

<span data-ttu-id="9f6a8-159">Isso é equivalente a chamar `CreateLogger` com o nome de tipo totalmente qualificado do `T`.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-159">This is equivalent to calling `CreateLogger` with the fully qualified type name of `T`.</span></span>

## <a name="log-level"></a><span data-ttu-id="9f6a8-160">Nível de log</span><span class="sxs-lookup"><span data-stu-id="9f6a8-160">Log level</span></span>

<span data-ttu-id="9f6a8-161">Cada vez que você gravar um log, especifique seu [LogLevel](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel).</span><span class="sxs-lookup"><span data-stu-id="9f6a8-161">Each time you write a log, you specify its [LogLevel](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel).</span></span> <span data-ttu-id="9f6a8-162">O nível de log indica o grau de severidade ou importância.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-162">The log level indicates the degree of severity or importance.</span></span>  <span data-ttu-id="9f6a8-163">Por exemplo, você pode escrever um `Information` registrar quando um método é finalizado normalmente, um `Warning` quando um método retorna o código de retorno 404 e um log `Error` log quando você captura uma exceção inesperada.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-163">For example, you might write an `Information` log when a method ends normally, a `Warning` log when a method returns a 404 return code, and an `Error` log when you catch an unexpected exception.</span></span>

<span data-ttu-id="9f6a8-164">No seguinte exemplo de código, os nomes dos métodos (por exemplo, `LogWarning`) Especifique o nível de log.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-164">In the following code example, the names of the methods (for example, `LogWarning`) specify the log level.</span></span>  <span data-ttu-id="9f6a8-165">O primeiro parâmetro é o [ID de evento de Log](#log-event-id) (explicado mais adiante neste artigo).</span><span class="sxs-lookup"><span data-stu-id="9f6a8-165">The first parameter is the [Log event ID](#log-event-id) (explained later in this article).</span></span>  <span data-ttu-id="9f6a8-166">Os parâmetros restantes construir uma cadeia de caracteres de mensagem de log.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-166">The remaining parameters construct a log message string.</span></span>

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="9f6a8-167">Métodos de log que incluem o nível no nome do método estão [métodos de extensão para ILogger](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loggerextensions).</span><span class="sxs-lookup"><span data-stu-id="9f6a8-167">Log methods that include the level in the method name are [extension methods for ILogger](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loggerextensions).</span></span> <span data-ttu-id="9f6a8-168">Nos bastidores, chamam esses métodos um `Log` método que utiliza um `LogLevel` parâmetro.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-168">Behind the scenes, these methods call a `Log` method that takes a `LogLevel` parameter.</span></span> <span data-ttu-id="9f6a8-169">Você pode chamar o `Log` método diretamente em vez de um desses métodos de extensão, mas a sintaxe é relativamente complicado.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-169">You can call the `Log` method directly rather than one of these extension methods, but the syntax is relatively complicated.</span></span> <span data-ttu-id="9f6a8-170">Para obter mais informações, consulte o [ILogger interface](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.ilogger) e [código-fonte extensões do agente de log](https://github.com/aspnet/Logging/blob/master/src/Microsoft.Extensions.Logging.Abstractions/LoggerExtensions.cs).</span><span class="sxs-lookup"><span data-stu-id="9f6a8-170">For more information, see the [ILogger interface](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.ilogger) and the [logger extensions source code](https://github.com/aspnet/Logging/blob/master/src/Microsoft.Extensions.Logging.Abstractions/LoggerExtensions.cs).</span></span>

<span data-ttu-id="9f6a8-171">ASP.NET Core define os seguintes [níveis de log](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel), ordenados aqui de severidade mais alta para menos.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-171">ASP.NET Core defines the following [log levels](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel), ordered here from least to highest severity.</span></span>

* <span data-ttu-id="9f6a8-172">Rastreamento = 0</span><span class="sxs-lookup"><span data-stu-id="9f6a8-172">Trace = 0</span></span>

  <span data-ttu-id="9f6a8-173">Para obter informações valiosas somente para um desenvolvedor de depurar um problema.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-173">For information that is valuable only to a developer debugging an issue.</span></span> <span data-ttu-id="9f6a8-174">Essas mensagens podem conter dados confidenciais de aplicativos e portanto não devem ser habilitadas em um ambiente de produção.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-174">These messages may contain sensitive application data and so should not be enabled in a production environment.</span></span> <span data-ttu-id="9f6a8-175">*Desabilitado por padrão.*</span><span class="sxs-lookup"><span data-stu-id="9f6a8-175">*Disabled by default.*</span></span> <span data-ttu-id="9f6a8-176">Exemplo: `Credentials: {"User":"someuser", "Password":"P@ssword"}`</span><span class="sxs-lookup"><span data-stu-id="9f6a8-176">Example: `Credentials: {"User":"someuser", "Password":"P@ssword"}`</span></span>

* <span data-ttu-id="9f6a8-177">Depurar = 1</span><span class="sxs-lookup"><span data-stu-id="9f6a8-177">Debug = 1</span></span>

  <span data-ttu-id="9f6a8-178">Para obter informações que não tem utilidade de curto prazo durante o desenvolvimento e depuração.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-178">For information that has short-term usefulness during development and debugging.</span></span> <span data-ttu-id="9f6a8-179">Exemplo: `Entering method Configure with flag set to true.` você normalmente não permite `Debug` nível registra em produção, a menos que você estiver solucionando problemas, devido ao alto volume de logs.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-179">Example: `Entering method Configure with flag set to true.` You typically would not enable `Debug` level logs in production unless you are troubleshooting, due to the high volume of logs.</span></span>

* <span data-ttu-id="9f6a8-180">Informações = 2</span><span class="sxs-lookup"><span data-stu-id="9f6a8-180">Information = 2</span></span>

  <span data-ttu-id="9f6a8-181">Para rastrear o fluxo geral do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-181">For tracking the general flow of the application.</span></span> <span data-ttu-id="9f6a8-182">Esses logs normalmente têm algum valor de longo prazo.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-182">These logs typically have some long-term value.</span></span> <span data-ttu-id="9f6a8-183">Exemplo: `Request received for path /api/todo`</span><span class="sxs-lookup"><span data-stu-id="9f6a8-183">Example: `Request received for path /api/todo`</span></span>

* <span data-ttu-id="9f6a8-184">Aviso = 3</span><span class="sxs-lookup"><span data-stu-id="9f6a8-184">Warning = 3</span></span>

  <span data-ttu-id="9f6a8-185">Para eventos anormais ou inesperados no fluxo do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-185">For abnormal or unexpected events in the application flow.</span></span> <span data-ttu-id="9f6a8-186">Eles podem incluir erros ou outras condições que fazem com que o aplicativo pare, mas que talvez precise ser investigado.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-186">These may include errors or other conditions that do not cause the application to stop, but which may need to be investigated.</span></span> <span data-ttu-id="9f6a8-187">Exceções manipuladas estão um local comum para usar o `Warning` nível de log.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-187">Handled exceptions are a common place to use the `Warning` log level.</span></span> <span data-ttu-id="9f6a8-188">Exemplo: `FileNotFoundException for file quotes.txt.`</span><span class="sxs-lookup"><span data-stu-id="9f6a8-188">Example: `FileNotFoundException for file quotes.txt.`</span></span>

* <span data-ttu-id="9f6a8-189">Erro = 4</span><span class="sxs-lookup"><span data-stu-id="9f6a8-189">Error = 4</span></span>

  <span data-ttu-id="9f6a8-190">Para erros e exceções que não podem ser manipuladas.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-190">For errors and exceptions that cannot be handled.</span></span> <span data-ttu-id="9f6a8-191">Essas mensagens indicam uma falha na operação (como a solicitação HTTP atual) ou a atividade atual, não é uma falha de todo o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-191">These messages indicate a failure in the current activity or operation (such as the current HTTP request), not an application-wide failure.</span></span> <span data-ttu-id="9f6a8-192">Exemplo de mensagem de log:`Cannot insert record due to duplicate key violation.`</span><span class="sxs-lookup"><span data-stu-id="9f6a8-192">Example log message: `Cannot insert record due to duplicate key violation.`</span></span>

* <span data-ttu-id="9f6a8-193">Crítico = 5</span><span class="sxs-lookup"><span data-stu-id="9f6a8-193">Critical = 5</span></span>

  <span data-ttu-id="9f6a8-194">Falhas que exigem atenção imediata.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-194">For failures that require immediate attention.</span></span> <span data-ttu-id="9f6a8-195">Exemplos: dados cenários de perda, espaço em disco insuficiente.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-195">Examples: data loss scenarios, out of disk space.</span></span>

<span data-ttu-id="9f6a8-196">Você pode usar o nível de log para controlar quanto saída de log é gravada em uma mídia de armazenamento específico ou exibir a janela.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-196">You can use the log level to control how much log output is written to a particular storage medium or display window.</span></span> <span data-ttu-id="9f6a8-197">Por exemplo, em produção, você pode querer todos os logs de `Information` nível e para baixo para ir para um repositório de dados de volume e todos os logs de `Warning` nível e superior para ir para um repositório de dados de valor.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-197">For example, in production you might want all logs of `Information` level and lower to go to a volume data store, and all logs of `Warning` level and higher to go to a value data store.</span></span> <span data-ttu-id="9f6a8-198">Durante o desenvolvimento, você normalmente pode enviar logs de `Warning` ou severidade mais alta para o console.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-198">During development, you might normally send logs of `Warning` or higher severity to the console.</span></span> <span data-ttu-id="9f6a8-199">Em seguida, quando você precisar solucionar problemas, você pode adicionar `Debug` nível.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-199">Then when you need to troubleshoot, you can add `Debug` level.</span></span> <span data-ttu-id="9f6a8-200">O [filtragem de Log](#log-filtering) seção mais adiante neste artigo explica como controlar os níveis de log que trata de um provedor.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-200">The [Log filtering](#log-filtering) section later in this article explains how to control which log levels a provider handles.</span></span>

<span data-ttu-id="9f6a8-201">Grava a estrutura do ASP.NET Core `Debug` nível logs de eventos do framework.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-201">The ASP.NET Core framework writes `Debug` level logs for framework events.</span></span> <span data-ttu-id="9f6a8-202">Os exemplos de log anteriormente neste artigo abaixo de logs excluído `Information` nível, portanto, não há `Debug` logs níveis foram mostrados.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-202">The log examples earlier in this article excluded logs below `Information` level, so no `Debug` level logs were shown.</span></span> <span data-ttu-id="9f6a8-203">Aqui está um exemplo de logs do console, se você executar o aplicativo de exemplo configurado para mostrar `Debug` e logs mais alto para o provedor de console.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-203">Here's an example of console logs if you run the sample application configured to show `Debug` and higher logs for the console provider.</span></span>

```console
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[1]
      Request starting HTTP/1.1 GET http://localhost:62555/api/todo/0
dbug: Microsoft.AspNetCore.Routing.Tree.TreeRouter[1]
      Request successfully matched the route with name 'GetTodo' and template 'api/Todo/{id}'.
dbug: Microsoft.AspNetCore.Mvc.Internal.ActionSelector[2]
      Action 'TodoApi.Controllers.TodoController.Update (TodoApi)' with id '089d59b6-92ec-472d-b552-cc613dfd625d' did not match the constraint 'Microsoft.AspNetCore.Mvc.Internal.HttpMethodActionConstraint'
dbug: Microsoft.AspNetCore.Mvc.Internal.ActionSelector[2]
      Action 'TodoApi.Controllers.TodoController.Delete (TodoApi)' with id 'f3476abe-4bd9-4ad3-9261-3ead09607366' did not match the constraint 'Microsoft.AspNetCore.Mvc.Internal.HttpMethodActionConstraint'
dbug: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[1]
      Executing action TodoApi.Controllers.TodoController.GetById (TodoApi)
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[1]
      Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
info: TodoApi.Controllers.TodoController[1002]
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      GetById(0) NOT FOUND
dbug: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[2]
      Executed action method TodoApi.Controllers.TodoController.GetById (TodoApi), returned result Microsoft.AspNetCore.Mvc.NotFoundResult.
info: Microsoft.AspNetCore.Mvc.StatusCodeResult[1]
      Executing HttpStatusCodeResult, setting HTTP status code 404
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[2]
      Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 0.8788ms
dbug: Microsoft.AspNetCore.Server.Kestrel[9]
      Connection id "0HL6L7NEFF2QD" completed keep alive response.
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[2]
      Request finished in 2.7286ms 404
```

## <a name="log-event-id"></a><span data-ttu-id="9f6a8-204">ID de evento de log</span><span class="sxs-lookup"><span data-stu-id="9f6a8-204">Log event ID</span></span>

<span data-ttu-id="9f6a8-205">Cada vez que você grava um log, você pode especificar um *ID do evento*.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-205">Each time you write a log, you can specify an *event ID*.</span></span> <span data-ttu-id="9f6a8-206">O aplicativo de exemplo faz isso usando um definidos localmente `LoggingEvents` classe:</span><span class="sxs-lookup"><span data-stu-id="9f6a8-206">The sample app does this by using a locally-defined `LoggingEvents` class:</span></span>

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](logging/sample//Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

<span data-ttu-id="9f6a8-207">Uma ID de evento é um valor inteiro que você pode usar para associar um conjunto de eventos registrados entre si.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-207">An event ID is an integer value that you can use to associate a set of logged events with one another.</span></span> <span data-ttu-id="9f6a8-208">Por exemplo, um log para adicionar um item a um carrinho de compras pode ser a identificação de evento 1000 e um log para concluir uma compra pode ser a identificação de evento 1001.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-208">For instance, a log for adding an item to a shopping cart could be event ID 1000 and a log for completing a purchase could be event ID 1001.</span></span>

<span data-ttu-id="9f6a8-209">Na saída de log, a ID do evento pode ser armazenada em um campo ou incluída na mensagem de texto, dependendo do provedor.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-209">In logging output, the event ID may be stored in a field or included in the text message, depending on the provider.</span></span>  <span data-ttu-id="9f6a8-210">O provedor de depuração não mostra as identificações de evento, mas o provedor do console mostra-los entre colchetes após a categoria:</span><span class="sxs-lookup"><span data-stu-id="9f6a8-210">The Debug provider doesn't show event IDs, but the console provider shows them in brackets after the category:</span></span>

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-format-string"></a><span data-ttu-id="9f6a8-211">Cadeia de formato de mensagem de log</span><span class="sxs-lookup"><span data-stu-id="9f6a8-211">Log message format string</span></span>

<span data-ttu-id="9f6a8-212">Cada vez que você gravar um log, em que você fornecer uma mensagem de texto.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-212">Each time you write a log, you provide a text message.</span></span> <span data-ttu-id="9f6a8-213">A cadeia de caracteres de mensagem pode conter espaços reservados nomeados para o argumento de valores são inseridos, como no exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="9f6a8-213">The message string can contain named placeholders into which argument values are placed, as in the following example:</span></span>

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="9f6a8-214">A ordem dos espaços reservados, não seus nomes, determina quais parâmetros são usados para eles.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-214">The order of placeholders, not their names, determines which parameters are used for them.</span></span> <span data-ttu-id="9f6a8-215">Por exemplo, se você tiver o seguinte código:</span><span class="sxs-lookup"><span data-stu-id="9f6a8-215">For example, if you have the following code:</span></span>

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

<span data-ttu-id="9f6a8-216">A mensagem de log resultante teria esta aparência:</span><span class="sxs-lookup"><span data-stu-id="9f6a8-216">The resulting log message would look like this:</span></span>

```
Parameter values: parm1, parm2
```

<span data-ttu-id="9f6a8-217">Estrutura de registros de mensagens formatação dessa maneira para possibilitar que os provedores de log implementar [log semântico, também conhecido como registro em log estruturado](http://programmers.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="9f6a8-217">The logging framework does message formatting in this way to make it possible for logging providers to implement [semantic logging, also known as structured logging](http://programmers.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span> <span data-ttu-id="9f6a8-218">Como os próprios argumentos são passados para o sistema de log, não apenas a mensagem formatada cadeia de caracteres, provedores de log podem armazenar os valores de parâmetro como campos além de cadeia de caracteres de mensagem.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-218">Because the arguments themselves are passed to the logging system, not just the formatted message string, logging providers can store the parameter values as fields in addition to the message string.</span></span> <span data-ttu-id="9f6a8-219">Por exemplo, se você estiver direcionando o log de saída para o armazenamento de tabela do Azure e sua chamada de método do agente de log tem esta aparência:</span><span class="sxs-lookup"><span data-stu-id="9f6a8-219">For example, if you are directing your log output to Azure Table Storage, and your logger method call looks like this:</span></span>

```csharp
_logger.LogInformation("Getting item {ID} at {RequestTime}", id, DateTime.Now);
```

<span data-ttu-id="9f6a8-220">Cada entidade de tabela do Azure pode ter `ID` e `RequestTime` propriedades, que seriam simplificam as consultas em dados de log.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-220">Each Azure Table entity could have `ID` and `RequestTime` properties, which would simplify queries on log data.</span></span> <span data-ttu-id="9f6a8-221">Você pode encontrar todos os logs em um determinado `RequestTime` intervalo, sem a necessidade de analisar o tempo limite da mensagem de texto.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-221">You could find all logs within a particular `RequestTime` range, without having to parse the time out of the text message.</span></span>

## <a name="logging-exceptions"></a><span data-ttu-id="9f6a8-222">Exceções de registro em log</span><span class="sxs-lookup"><span data-stu-id="9f6a8-222">Logging exceptions</span></span>

<span data-ttu-id="9f6a8-223">Os métodos de agente de log têm sobrecargas que permitem que você passar uma exceção, como no exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="9f6a8-223">The logger methods have overloads that let you pass in an exception, as in the following example:</span></span>

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

<span data-ttu-id="9f6a8-224">Provedores diferentes lidar com as informações de exceção de maneiras diferentes.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-224">Different providers handle the exception information in different ways.</span></span> <span data-ttu-id="9f6a8-225">Aqui está um exemplo da saída do provedor de depuração de código mostrado acima.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-225">Here's an example of Debug provider output from the code shown above.</span></span>

```
TodoApi.Controllers.TodoController:Warning: GetById(036dd898-fb01-47e8-9a65-f92eb73cf924) NOT FOUND

System.Exception: Item not found exception.
 at TodoApi.Controllers.TodoController.GetById(String id) in C:\logging\sample\src\TodoApi\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a><span data-ttu-id="9f6a8-226">Filtragem de log</span><span class="sxs-lookup"><span data-stu-id="9f6a8-226">Log filtering</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9f6a8-227">ASP.NET Core 2. x</span><span class="sxs-lookup"><span data-stu-id="9f6a8-227">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="9f6a8-228">Você pode especificar um nível de log mínimo para um provedor específico e uma categoria ou para todos os provedores ou todas as categorias.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-228">You can specify a minimum log level for a specific provider and category or for all providers or all categories.</span></span>  <span data-ttu-id="9f6a8-229">Logs abaixo do nível mínimo não são passados para esse provedor, para que eles não obter exibidos ou armazenados.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-229">Any logs below the minimum level aren't passed to that provider, so they don't get displayed or stored.</span></span> 

<span data-ttu-id="9f6a8-230">Se você desejar suprimir todos os logs, você pode especificar `LogLevel.None` como o nível de log mínimo.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-230">If you want to suppress all logs, you can specify `LogLevel.None` as the minimum log level.</span></span> <span data-ttu-id="9f6a8-231">O valor inteiro do `LogLevel.None` é 6, que é maior do que `LogLevel.Critical` (5).</span><span class="sxs-lookup"><span data-stu-id="9f6a8-231">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

<span data-ttu-id="9f6a8-232">**Criar regras de filtro na configuração**</span><span class="sxs-lookup"><span data-stu-id="9f6a8-232">**Create filter rules in configuration**</span></span>

<span data-ttu-id="9f6a8-233">Os modelos de projeto criam código que chama `CreateDefaultBuilder` para configurar o log para os provedores de Console e de depuração.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-233">The project templates create code that calls `CreateDefaultBuilder` to set up logging for the Console and Debug providers.</span></span> <span data-ttu-id="9f6a8-234">O `CreateDefaultBuilder` método também define o registro em log para procurar a configuração em um `Logging` seção, usando código semelhante ao seguinte:</span><span class="sxs-lookup"><span data-stu-id="9f6a8-234">The `CreateDefaultBuilder` method also sets up logging to look for configuration in a `Logging` section, using code like the following:</span></span>

[!code-csharp[](logging/sample2/Program.cs?name=snippet_ExpandDefault&highlight=15)]

<span data-ttu-id="9f6a8-235">Os dados de configuração especificam os níveis de log mínimo pelo provedor e a categoria, como no exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="9f6a8-235">The configuration data specifies minimum log levels by provider and category, as in the following example:</span></span>

[!code-json[](logging/sample2/AppSettings.json)]

<span data-ttu-id="9f6a8-236">Este JSON cria seis regras de filtro, um para o provedor de depuração, quatro para o provedor de Console e que se aplica a todos os provedores.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-236">This JSON creates six filter rules, one for the Debug provider, four for the Console provider, and one that applies to all providers.</span></span> <span data-ttu-id="9f6a8-237">Você verá posteriormente como apenas uma dessas regras é escolhida para cada provedor quando um `ILogger` objeto é criado.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-237">You'll see later how just one of these rules is chosen for each provider when an `ILogger` object is created.</span></span>

<span data-ttu-id="9f6a8-238">**Regras de filtro no código**</span><span class="sxs-lookup"><span data-stu-id="9f6a8-238">**Filter rules in code**</span></span>

<span data-ttu-id="9f6a8-239">Você pode registrar as regras de filtro no código, conforme mostrado no exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="9f6a8-239">You can register filter rules in code, as shown in the following example:</span></span>

[!code-csharp[](logging/sample2/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

<span data-ttu-id="9f6a8-240">O segundo `AddFilter` Especifica o provedor de depuração usando seu nome de tipo.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-240">The second `AddFilter` specifies the Debug provider by using its type name.</span></span> <span data-ttu-id="9f6a8-241">A primeira `AddFilter` se aplica a todos os provedores porque ela não especifica um tipo de provedor.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-241">The first `AddFilter` applies to all providers because it doesn't specify a provider type.</span></span>

<span data-ttu-id="9f6a8-242">**Como filtrar as regras são aplicadas**</span><span class="sxs-lookup"><span data-stu-id="9f6a8-242">**How filtering rules are applied**</span></span>

<span data-ttu-id="9f6a8-243">Os dados de configuração e o `AddFilter` código mostrado nos exemplos anteriores criar as regras mostradas na tabela a seguir.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-243">The configuration data and the `AddFilter` code shown in the preceding examples create the rules shown in the following table.</span></span> <span data-ttu-id="9f6a8-244">Primeiro seis vêm de exemplo de configuração e os dois últimos vêm do exemplo de código.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-244">The first six come from the configuration example and the last two come from the code example.</span></span>

<span data-ttu-id="9f6a8-245">Número</span><span class="sxs-lookup"><span data-stu-id="9f6a8-245">Number</span></span>|<span data-ttu-id="9f6a8-246">Provider</span><span class="sxs-lookup"><span data-stu-id="9f6a8-246">Provider</span></span>|<span data-ttu-id="9f6a8-247">Categorias que começam com</span><span class="sxs-lookup"><span data-stu-id="9f6a8-247">Categories that begin with</span></span>|<span data-ttu-id="9f6a8-248">Nível de log mínimo</span><span class="sxs-lookup"><span data-stu-id="9f6a8-248">Minimum log level</span></span>|
------|--------|--------------------------|-----------------|
<span data-ttu-id="9f6a8-249">1</span><span class="sxs-lookup"><span data-stu-id="9f6a8-249">1</span></span>|<span data-ttu-id="9f6a8-250">Depurar</span><span class="sxs-lookup"><span data-stu-id="9f6a8-250">Debug</span></span>|<span data-ttu-id="9f6a8-251">Todas as categorias</span><span class="sxs-lookup"><span data-stu-id="9f6a8-251">All categories</span></span>|<span data-ttu-id="9f6a8-252">Informações</span><span class="sxs-lookup"><span data-stu-id="9f6a8-252">Information</span></span>|
<span data-ttu-id="9f6a8-253">2</span><span class="sxs-lookup"><span data-stu-id="9f6a8-253">2</span></span>|<span data-ttu-id="9f6a8-254">Console</span><span class="sxs-lookup"><span data-stu-id="9f6a8-254">Console</span></span>|<span data-ttu-id="9f6a8-255">Microsoft.AspNetCore.Mvc.Razor.Internal</span><span class="sxs-lookup"><span data-stu-id="9f6a8-255">Microsoft.AspNetCore.Mvc.Razor.Internal</span></span>|<span data-ttu-id="9f6a8-256">Aviso</span><span class="sxs-lookup"><span data-stu-id="9f6a8-256">Warning</span></span>|
<span data-ttu-id="9f6a8-257">3</span><span class="sxs-lookup"><span data-stu-id="9f6a8-257">3</span></span>|<span data-ttu-id="9f6a8-258">Console</span><span class="sxs-lookup"><span data-stu-id="9f6a8-258">Console</span></span>|<span data-ttu-id="9f6a8-259">Microsoft.AspNetCore.Mvc.Razor.Razor</span><span class="sxs-lookup"><span data-stu-id="9f6a8-259">Microsoft.AspNetCore.Mvc.Razor.Razor</span></span>|<span data-ttu-id="9f6a8-260">Depurar</span><span class="sxs-lookup"><span data-stu-id="9f6a8-260">Debug</span></span>|
<span data-ttu-id="9f6a8-261">4</span><span class="sxs-lookup"><span data-stu-id="9f6a8-261">4</span></span>|<span data-ttu-id="9f6a8-262">Console</span><span class="sxs-lookup"><span data-stu-id="9f6a8-262">Console</span></span>|<span data-ttu-id="9f6a8-263">Microsoft.AspNetCore.Mvc.Razor</span><span class="sxs-lookup"><span data-stu-id="9f6a8-263">Microsoft.AspNetCore.Mvc.Razor</span></span>|<span data-ttu-id="9f6a8-264">Erro</span><span class="sxs-lookup"><span data-stu-id="9f6a8-264">Error</span></span>|
<span data-ttu-id="9f6a8-265">5</span><span class="sxs-lookup"><span data-stu-id="9f6a8-265">5</span></span>|<span data-ttu-id="9f6a8-266">Console</span><span class="sxs-lookup"><span data-stu-id="9f6a8-266">Console</span></span>|<span data-ttu-id="9f6a8-267">Todas as categorias</span><span class="sxs-lookup"><span data-stu-id="9f6a8-267">All categories</span></span>|<span data-ttu-id="9f6a8-268">Informações</span><span class="sxs-lookup"><span data-stu-id="9f6a8-268">Information</span></span>|
<span data-ttu-id="9f6a8-269">6</span><span class="sxs-lookup"><span data-stu-id="9f6a8-269">6</span></span>|<span data-ttu-id="9f6a8-270">Todos os provedores</span><span class="sxs-lookup"><span data-stu-id="9f6a8-270">All providers</span></span>|<span data-ttu-id="9f6a8-271">Todas as categorias</span><span class="sxs-lookup"><span data-stu-id="9f6a8-271">All categories</span></span>|<span data-ttu-id="9f6a8-272">Aviso</span><span class="sxs-lookup"><span data-stu-id="9f6a8-272">Warning</span></span>
<span data-ttu-id="9f6a8-273">7</span><span class="sxs-lookup"><span data-stu-id="9f6a8-273">7</span></span>|<span data-ttu-id="9f6a8-274">Todos os provedores</span><span class="sxs-lookup"><span data-stu-id="9f6a8-274">All providers</span></span>|<span data-ttu-id="9f6a8-275">Sistema</span><span class="sxs-lookup"><span data-stu-id="9f6a8-275">System</span></span>|<span data-ttu-id="9f6a8-276">Depurar</span><span class="sxs-lookup"><span data-stu-id="9f6a8-276">Debug</span></span>
<span data-ttu-id="9f6a8-277">8</span><span class="sxs-lookup"><span data-stu-id="9f6a8-277">8</span></span>|<span data-ttu-id="9f6a8-278">Depurar</span><span class="sxs-lookup"><span data-stu-id="9f6a8-278">Debug</span></span>|<span data-ttu-id="9f6a8-279">Microsoft</span><span class="sxs-lookup"><span data-stu-id="9f6a8-279">Microsoft</span></span>|<span data-ttu-id="9f6a8-280">rastreamento</span><span class="sxs-lookup"><span data-stu-id="9f6a8-280">Trace</span></span>

<span data-ttu-id="9f6a8-281">Quando você cria um `ILogger` objeto gravar logs com o `ILoggerFactory` objeto seleciona uma única regra por provedor para aplicar a esse agente.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-281">When you create an `ILogger` object to write logs with, the `ILoggerFactory` object selects a single rule per provider to apply to that logger.</span></span> <span data-ttu-id="9f6a8-282">Todas as mensagens gravadas pelo `ILogger` objeto são filtrados com base nas regras selecionadas.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-282">All messages written by that `ILogger` object are filtered based on the selected rules.</span></span> <span data-ttu-id="9f6a8-283">A regra mais específica possível para cada par de categoria e o provedor é selecionada de regras disponíveis.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-283">The most specific rule possible for each provider and category pair is selected from the available rules.</span></span>

<span data-ttu-id="9f6a8-284">O seguinte algoritmo é usado para cada provedor quando um `ILogger` é criado para uma determinada categoria:</span><span class="sxs-lookup"><span data-stu-id="9f6a8-284">The following algorithm is used for each provider when an `ILogger` is created for a given category:</span></span>

* <span data-ttu-id="9f6a8-285">Selecione todas as regras que correspondem ao provedor ou seu alias.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-285">Select all rules that match the provider or its alias.</span></span>  <span data-ttu-id="9f6a8-286">Se nenhum for encontrado, selecione todas as regras com um provedor vazio.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-286">If none are found, select all rules with an empty provider.</span></span>
* <span data-ttu-id="9f6a8-287">Do resultado da etapa anterior, selecione as regras com maior correspondência de prefixo de categoria.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-287">From the result of the preceding step, select rules with longest matching category prefix.</span></span> <span data-ttu-id="9f6a8-288">Se nenhum for encontrado, selecione todas as regras que não especificar uma categoria.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-288">If none are found, select all rules that don't specify a category.</span></span>
* <span data-ttu-id="9f6a8-289">Se várias regras forem selecionadas terão o **último** um.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-289">If multiple rules are selected take the **last** one.</span></span>
* <span data-ttu-id="9f6a8-290">Se nenhuma regra estiver selecionada, use `MinimumLevel`.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-290">If no rules are selected, use `MinimumLevel`.</span></span>
 
<span data-ttu-id="9f6a8-291">Por exemplo, suponha que você tem a lista anterior de regras e criar um `ILogger` objeto para a categoria "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":</span><span class="sxs-lookup"><span data-stu-id="9f6a8-291">For example, suppose you have the preceding list of rules and you create an `ILogger` object for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":</span></span>

* <span data-ttu-id="9f6a8-292">Para o provedor de depuração, 1, 6 e 8 as regras se aplicam.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-292">For the Debug provider, rules 1, 6, and 8 apply.</span></span> <span data-ttu-id="9f6a8-293">Regra de 8 é o mais específica, para que seja selecionado.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-293">Rule 8 is most specific, so that's the one selected.</span></span>
* <span data-ttu-id="9f6a8-294">Para o provedor de Console, 3, 4, 5 e 6 de regras se aplicam.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-294">For the Console provider, rules 3, 4, 5, and 6 apply.</span></span> <span data-ttu-id="9f6a8-295">A regra 3 é o mais específica.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-295">Rule 3 is most specific.</span></span>

<span data-ttu-id="9f6a8-296">Quando você cria logs com um `ILogger` para a categoria "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine", os logs de `Trace` nível e acima para ir para o provedor de depuração e logs de `Debug` nível e acima para ir para o provedor de Console.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-296">When you create logs with an `ILogger` for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine", logs of `Trace` level and above will go to the Debug provider, and logs of `Debug` level and above will go to the Console provider.</span></span>

<span data-ttu-id="9f6a8-297">**Aliases de provedor**</span><span class="sxs-lookup"><span data-stu-id="9f6a8-297">**Provider aliases**</span></span>

<span data-ttu-id="9f6a8-298">Você pode usar o nome do tipo para especificar um provedor de configuração, mas cada provedor define um menor *alias* que é mais fácil de usar.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-298">You can use the type name to specify a provider in configuration, but each provider defines a shorter *alias* that is easier to use.</span></span> <span data-ttu-id="9f6a8-299">Para os provedores internos, use os seguintes aliases:</span><span class="sxs-lookup"><span data-stu-id="9f6a8-299">For the built-in providers, use the following aliases:</span></span>

- <span data-ttu-id="9f6a8-300">Console</span><span class="sxs-lookup"><span data-stu-id="9f6a8-300">Console</span></span>
- <span data-ttu-id="9f6a8-301">Depurar</span><span class="sxs-lookup"><span data-stu-id="9f6a8-301">Debug</span></span>
- <span data-ttu-id="9f6a8-302">Log de eventos</span><span class="sxs-lookup"><span data-stu-id="9f6a8-302">EventLog</span></span>
- <span data-ttu-id="9f6a8-303">AzureAppServices</span><span class="sxs-lookup"><span data-stu-id="9f6a8-303">AzureAppServices</span></span>
- <span data-ttu-id="9f6a8-304">TraceSource</span><span class="sxs-lookup"><span data-stu-id="9f6a8-304">TraceSource</span></span>
- <span data-ttu-id="9f6a8-305">EventSource</span><span class="sxs-lookup"><span data-stu-id="9f6a8-305">EventSource</span></span>

<span data-ttu-id="9f6a8-306">**Nível mínimo de padrão**</span><span class="sxs-lookup"><span data-stu-id="9f6a8-306">**Default minimum level**</span></span>

<span data-ttu-id="9f6a8-307">Há uma configuração de nível mínimo que entra em vigor somente se nenhuma regra de código ou de configuração aplicáveis para um determinado provedor e uma categoria.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-307">There is a minimum level setting that takes effect only if no rules from configuration or code apply for a given provider and category.</span></span> <span data-ttu-id="9f6a8-308">O exemplo a seguir mostra como definir o nível mínimo:</span><span class="sxs-lookup"><span data-stu-id="9f6a8-308">The following example shows how to set the minimum level:</span></span>

[!code-csharp[](logging/sample2/Program.cs?name=snippet_MinLevel&highlight=3)]

<span data-ttu-id="9f6a8-309">Se você não definir explicitamente o nível mínimo, o valor padrão é `Information`, o que significa que `Trace` e `Debug` logs são ignorados.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-309">IF you don't explicitly set the minimum level, the default value is `Information`, which means that `Trace` and `Debug` logs are ignored.</span></span>

<span data-ttu-id="9f6a8-310">**Funções de filtro**</span><span class="sxs-lookup"><span data-stu-id="9f6a8-310">**Filter functions**</span></span>

<span data-ttu-id="9f6a8-311">Você pode escrever código em uma função de filtro para aplicar regras de filtragem.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-311">You can write code in a filter function to apply filtering rules.</span></span> <span data-ttu-id="9f6a8-312">Uma função de filtro é invocada para todos os provedores e as categorias que não têm regras atribuídas a eles por configuração ou código.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-312">A filter function is invoked for all providers and categories that do not have rules assigned to them by configuration or code.</span></span> <span data-ttu-id="9f6a8-313">O código na função tem acesso para o tipo de provedor, a categoria e o nível de log para decidir se uma mensagem deve ser registrada.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-313">Code in the function has access to the provider type, category, and log level to decide whether or not a message should be logged.</span></span> <span data-ttu-id="9f6a8-314">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="9f6a8-314">For example:</span></span>

[!code-csharp[](logging/sample2/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9f6a8-315">ASP.NET Core 1. x</span><span class="sxs-lookup"><span data-stu-id="9f6a8-315">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="9f6a8-316">Alguns provedores de log permitem especificar quando os logs devem ser gravados em uma mídia de armazenamento ou ignorados com base no nível de log e categoria.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-316">Some logging providers let you specify when logs should be written to a storage medium or ignored based on log level and category.</span></span>

<span data-ttu-id="9f6a8-317">O `AddConsole` e `AddDebug` métodos de extensão fornecem sobrecargas que permitem que você passe em critérios de filtragem.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-317">The `AddConsole` and `AddDebug` extension methods provide overloads that let you pass in filtering criteria.</span></span> <span data-ttu-id="9f6a8-318">O código de exemplo a seguir faz com que o provedor de console ignorar logs abaixo `Warning` nível, enquanto o provedor de depuração ignora os logs que cria a estrutura.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-318">The following sample code causes the console provider to ignore logs below `Warning` level, while the Debug provider ignores logs that the framework creates.</span></span>

[!code-csharp[](logging/sample/Startup.cs?name=snippet_AddConsoleAndDebugWithFilter&highlight=6-7)]

<span data-ttu-id="9f6a8-319">O `AddEventLog` método tem uma sobrecarga que usa um `EventLogSettings` instância, que pode conter uma função de filtragem no seu `Filter` propriedade.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-319">The `AddEventLog` method has an overload that takes an `EventLogSettings` instance, which may contain a filtering function in its `Filter` property.</span></span> <span data-ttu-id="9f6a8-320">O provedor TraceSource não fornece qualquer uma dessas sobrecargas, porque seu nível de log e outros parâmetros são baseados no `SourceSwitch` e `TraceListener` usa.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-320">The TraceSource provider does not provide any of those overloads, since its logging level and other parameters are based on the  `SourceSwitch` and `TraceListener` it uses.</span></span>

<span data-ttu-id="9f6a8-321">Você pode definir regras de filtragem para todos os provedores registrados com um `ILoggerFactory` instância usando o `WithFilter` método de extensão.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-321">You can set filtering rules for all providers that are registered with an `ILoggerFactory` instance by using the `WithFilter` extension method.</span></span> <span data-ttu-id="9f6a8-322">O exemplo a seguir limita os logs de framework (categoria começa com "Microsoft" ou "Sistema"), os avisos ao mesmo tempo, permitindo que o log de aplicativo no nível de depuração.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-322">The example below limits framework logs (category begins with "Microsoft" or "System") to warnings while letting the app log at debug level.</span></span>

[!code-csharp[](logging/sample/Startup.cs?name=snippet_FactoryFilter&highlight=6-11)]

<span data-ttu-id="9f6a8-323">Se você quiser usar a filtragem para impedir que todos os logs sejam gravados para uma determinada categoria, você pode especificar `LogLevel.None` como o nível de log mínimo dessa categoria.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-323">If you want to use filtering to prevent all logs from being written for a particular category, you can specify `LogLevel.None` as the minimum log level for that category.</span></span> <span data-ttu-id="9f6a8-324">O valor inteiro do `LogLevel.None` é 6, que é maior do que `LogLevel.Critical` (5).</span><span class="sxs-lookup"><span data-stu-id="9f6a8-324">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

<span data-ttu-id="9f6a8-325">O `WithFilter` fornecido pelo método de extensão de [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) pacote NuGet.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-325">The `WithFilter` extension method is provided by the [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) NuGet package.</span></span> <span data-ttu-id="9f6a8-326">O método retorna um novo `ILoggerFactory` instância que filtra as mensagens de log passadas para todos os provedores de agente registrados com ele.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-326">The method returns a new `ILoggerFactory` instance that will filter the log messages passed to all logger providers registered with it.</span></span> <span data-ttu-id="9f6a8-327">Ele não afeta nenhum outro `ILoggerFactory` instâncias, incluindo original `ILoggerFactory` instância.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-327">It does not affect any other `ILoggerFactory` instances, including the original `ILoggerFactory` instance.</span></span>

---

## <a name="log-scopes"></a><span data-ttu-id="9f6a8-328">Escopos de log</span><span class="sxs-lookup"><span data-stu-id="9f6a8-328">Log scopes</span></span>

<span data-ttu-id="9f6a8-329">Você pode agrupar um conjunto de operações lógicas dentro de um *escopo* para anexar os mesmos dados para cada log que é criado como parte desse conjunto.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-329">You can group a set of logical operations within a *scope* in order to attach the same data to each log that is created as part of that set.</span></span>  <span data-ttu-id="9f6a8-330">Por exemplo, convém cada log criado como parte do processamento de uma transação para incluir a ID da transação.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-330">For example, you might want every log created as part of processing a transaction to include the transaction ID.</span></span>

<span data-ttu-id="9f6a8-331">Um escopo é um `IDisposable` tipo retornado pelo `ILogger.BeginScope<TState>` método e permanece assim até que seja descartada.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-331">A scope is an `IDisposable` type that is returned by the `ILogger.BeginScope<TState>` method and lasts until it is disposed.</span></span> <span data-ttu-id="9f6a8-332">Use um escopo por encapsulamento chama seu agente de log em um `using` bloquear, conforme mostrado aqui:</span><span class="sxs-lookup"><span data-stu-id="9f6a8-332">You use a scope by wrapping your logger calls in a `using` block, as shown here:</span></span>

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

<span data-ttu-id="9f6a8-333">O código a seguir habilita os escopos para o provedor de console:</span><span class="sxs-lookup"><span data-stu-id="9f6a8-333">The following code enables scopes for the console provider:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9f6a8-334">ASP.NET Core 2. x</span><span class="sxs-lookup"><span data-stu-id="9f6a8-334">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="9f6a8-335">Em *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="9f6a8-335">In *Program.cs*:</span></span>

[!code-csharp[](logging/sample2/Program.cs?name=snippet_Scopes&highlight=4)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9f6a8-336">ASP.NET Core 1. x</span><span class="sxs-lookup"><span data-stu-id="9f6a8-336">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="9f6a8-337">Em *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="9f6a8-337">In *Startup.cs*:</span></span>

[!code-csharp[](logging/sample/Startup.cs?name=snippet_Scopes&highlight=6)]

---

<span data-ttu-id="9f6a8-338">Cada mensagem de log inclui as informações com escopo definido:</span><span class="sxs-lookup"><span data-stu-id="9f6a8-338">Each log message includes the scoped information:</span></span>

```
info: TodoApi.Controllers.TodoController[1002]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      GetById(0) NOT FOUND
```

## <a name="built-in-logging-providers"></a><span data-ttu-id="9f6a8-339">Provedores de registro em log internos</span><span class="sxs-lookup"><span data-stu-id="9f6a8-339">Built-in logging providers</span></span>

<span data-ttu-id="9f6a8-340">ASP.NET Core vem com os seguintes provedores:</span><span class="sxs-lookup"><span data-stu-id="9f6a8-340">ASP.NET Core ships the following providers:</span></span>

* [<span data-ttu-id="9f6a8-341">Console</span><span class="sxs-lookup"><span data-stu-id="9f6a8-341">Console</span></span>](#console)
* [<span data-ttu-id="9f6a8-342">Depurar</span><span class="sxs-lookup"><span data-stu-id="9f6a8-342">Debug</span></span>](#debug)
* [<span data-ttu-id="9f6a8-343">EventSource</span><span class="sxs-lookup"><span data-stu-id="9f6a8-343">EventSource</span></span>](#eventsource)
* [<span data-ttu-id="9f6a8-344">EventLog</span><span class="sxs-lookup"><span data-stu-id="9f6a8-344">EventLog</span></span>](#eventlog)
* [<span data-ttu-id="9f6a8-345">TraceSource</span><span class="sxs-lookup"><span data-stu-id="9f6a8-345">TraceSource</span></span>](#tracesource)
* [<span data-ttu-id="9f6a8-346">Serviço de Aplicativo do Azure</span><span class="sxs-lookup"><span data-stu-id="9f6a8-346">Azure App Service</span></span>](#appservice)

<a id="console"></a>
### <a name="the-console-provider"></a><span data-ttu-id="9f6a8-347">O provedor de console</span><span class="sxs-lookup"><span data-stu-id="9f6a8-347">The console provider</span></span>

<span data-ttu-id="9f6a8-348">O [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) pacote provedor envia a saída de log para o console.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-348">The [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) provider package sends log output to the console.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9f6a8-349">ASP.NET Core 2. x</span><span class="sxs-lookup"><span data-stu-id="9f6a8-349">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddConsole()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9f6a8-350">ASP.NET Core 1. x</span><span class="sxs-lookup"><span data-stu-id="9f6a8-350">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddConsole()
```

<span data-ttu-id="9f6a8-351">[Sobrecargas de AddConsole](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.consoleloggerextensions) permitem que você transmitir um um nível de log mínimo, uma função de filtro e um valor booleano que indica se há suporte para escopos.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-351">[AddConsole overloads](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.consoleloggerextensions) let you pass in an a minimum log level, a filter function, and a boolean that indicates whether scopes are supported.</span></span>  <span data-ttu-id="9f6a8-352">Outra opção é passar um `IConfiguration` objeto, que pode especificar níveis de log e suporte de escopos.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-352">Another option is to pass in an `IConfiguration` object, which can specify scopes support and logging levels.</span></span> 

<span data-ttu-id="9f6a8-353">Se você estiver considerando o provedor de console para uso em produção, lembre-se de que ele tem um impacto significativo no desempenho.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-353">If you are considering the console provider for use in production, be aware that it has a significant impact on performance.</span></span>

<span data-ttu-id="9f6a8-354">Quando você cria um novo projeto no Visual Studio, o `AddConsole` método tem esta aparência:</span><span class="sxs-lookup"><span data-stu-id="9f6a8-354">When you create a new project in Visual Studio, the `AddConsole` method looks like this:</span></span>

```csharp
loggerFactory.AddConsole(Configuration.GetSection("Logging"));
```

<span data-ttu-id="9f6a8-355">Esse código se refere a `Logging` seção o *appSettings. JSON* arquivo:</span><span class="sxs-lookup"><span data-stu-id="9f6a8-355">This code refers to the `Logging` section of the *appSettings.json* file:</span></span>

[!code-json[](logging/sample//appsettings.json)]

<span data-ttu-id="9f6a8-356">As configurações mostradas limite framework logs avisos ao mesmo tempo, permitindo que o aplicativo para fazer logon no nível de depuração, conforme explicado no [filtragem de Log](#log-filtering) seção.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-356">The settings shown limit framework logs to warnings while allowing the app to log at debug level, as explained in the [Log filtering](#log-filtering) section.</span></span> <span data-ttu-id="9f6a8-357">Para obter mais informações, consulte [configuração](configuration.md).</span><span class="sxs-lookup"><span data-stu-id="9f6a8-357">For more information, see [Configuration](configuration.md).</span></span>

---

<a id="debug"></a>
### <a name="the-debug-provider"></a><span data-ttu-id="9f6a8-358">O provedor de depuração</span><span class="sxs-lookup"><span data-stu-id="9f6a8-358">The Debug provider</span></span>

<span data-ttu-id="9f6a8-359">O [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) pacote provedor grava a saída de log usando o [Debug](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.debug#System_Diagnostics_Debug) classe (`Debug.WriteLine` chamadas de método).</span><span class="sxs-lookup"><span data-stu-id="9f6a8-359">The [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) provider package writes log output by using the [System.Diagnostics.Debug](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.debug#System_Diagnostics_Debug) class (`Debug.WriteLine` method calls).</span></span>

<span data-ttu-id="9f6a8-360">No Linux, esse provedor grava logs no *logs /var/log/message*.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-360">On Linux, this provider writes logs to */var/log/message*.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9f6a8-361">ASP.NET Core 2. x</span><span class="sxs-lookup"><span data-stu-id="9f6a8-361">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddDebug()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9f6a8-362">ASP.NET Core 1. x</span><span class="sxs-lookup"><span data-stu-id="9f6a8-362">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddDebug()
```

<span data-ttu-id="9f6a8-363">[Sobrecargas de AddDebug](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.debugloggerfactoryextensions) permitem que você passa um nível de log mínimo ou uma função de filtro.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-363">[AddDebug overloads](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.debugloggerfactoryextensions) let you pass in a minimum log level or a filter function.</span></span>

---

<a id="eventsource"></a>
### <a name="the-eventsource-provider"></a><span data-ttu-id="9f6a8-364">O provedor de EventSource</span><span class="sxs-lookup"><span data-stu-id="9f6a8-364">The EventSource provider</span></span>

<span data-ttu-id="9f6a8-365">Para aplicativos que se destinam a ASP.NET Core 1.1.0 ou superior, o [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) pacote provedor pode implementar o rastreamento de eventos.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-365">For apps that target ASP.NET Core 1.1.0 or higher, the [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) provider package can implement event tracing.</span></span> <span data-ttu-id="9f6a8-366">No Windows, ele usa [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span><span class="sxs-lookup"><span data-stu-id="9f6a8-366">On Windows, it uses [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span></span> <span data-ttu-id="9f6a8-367">O provedor é entre plataformas, mas não existem ferramentas de coleta e a exibição para Linux ou macOS em nenhum evento.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-367">The provider is cross-platform, but there are no event collection and display tools yet for Linux or macOS.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9f6a8-368">ASP.NET Core 2. x</span><span class="sxs-lookup"><span data-stu-id="9f6a8-368">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddEventSourceLogger()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9f6a8-369">ASP.NET Core 1. x</span><span class="sxs-lookup"><span data-stu-id="9f6a8-369">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddEventSourceLogger()
```

---

<span data-ttu-id="9f6a8-370">Uma boa maneira de coletar e exibir logs é usar o [PerfView utilitário](https://www.microsoft.com/download/details.aspx?id=28567).</span><span class="sxs-lookup"><span data-stu-id="9f6a8-370">A good way to collect and view logs is to use the [PerfView utility](https://www.microsoft.com/download/details.aspx?id=28567).</span></span> <span data-ttu-id="9f6a8-371">Há outras ferramentas para exibir os logs do ETW, mas PerfView proporciona a melhor experiência para trabalhar com os eventos ETW emitidos pelo ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-371">There are other tools for viewing ETW logs, but PerfView provides the best experience for working with the ETW events emitted by ASP.NET.</span></span> 

<span data-ttu-id="9f6a8-372">Para configurar o PerfView para coletar eventos registrados por esse provedor, adicione a cadeia de caracteres `*Microsoft-Extensions-Logging` para o **provedores adicionais** lista.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-372">To configure PerfView for collecting events logged by this provider, add the string `*Microsoft-Extensions-Logging` to the **Additional Providers** list.</span></span> <span data-ttu-id="9f6a8-373">(Não perca o asterisco no início da cadeia de caracteres).</span><span class="sxs-lookup"><span data-stu-id="9f6a8-373">(Don't miss the asterisk at the start of the string.)</span></span>

![Outros provedores de Perfview](logging/_static/perfview-additional-providers.png)

<span data-ttu-id="9f6a8-375">Capturando eventos em Nano Server requer alguma configuração adicional:</span><span class="sxs-lookup"><span data-stu-id="9f6a8-375">Capturing events on Nano Server requires some additional setup:</span></span>

* <span data-ttu-id="9f6a8-376">Conecte-se de comunicação remota do PowerShell para o Nano Server:</span><span class="sxs-lookup"><span data-stu-id="9f6a8-376">Connect PowerShell remoting to the Nano Server:</span></span>

  ```powershell
  Enter-PSSession [name]
  ```

* <span data-ttu-id="9f6a8-377">Crie uma sessão do ETW:</span><span class="sxs-lookup"><span data-stu-id="9f6a8-377">Create an ETW session:</span></span>

  ```powershell
  New-EtwTraceSession -Name "MyAppTrace" -LocalFilePath C:\trace.etl
  ```

* <span data-ttu-id="9f6a8-378">Adicionar provedores do ETW para [CLR](https://msdn.microsoft.com/library/ff357718), ASP.NET Core e outros conforme necessário.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-378">Add ETW providers for [CLR](https://msdn.microsoft.com/library/ff357718), ASP.NET Core, and others as needed.</span></span> <span data-ttu-id="9f6a8-379">O provedor ASP.NET Core GUID é `3ac73b97-af73-50e9-0822-5da4367920d0`.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-379">The ASP.NET Core provider GUID is `3ac73b97-af73-50e9-0822-5da4367920d0`.</span></span> 

  ```powershell
  Add-EtwTraceProvider -Guid "{e13c0d23-ccbc-4e12-931b-d9cc2eee27e4}" -SessionName MyAppTrace
  Add-EtwTraceProvider -Guid "{3ac73b97-af73-50e9-0822-5da4367920d0}" -SessionName MyAppTrace
  ```

* <span data-ttu-id="9f6a8-380">Executar o site e fazer quaisquer ações que você deseja informações de rastreamento.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-380">Run the site and do whatever actions you want tracing information for.</span></span>

* <span data-ttu-id="9f6a8-381">Pare a sessão de rastreamento quando tiver terminado:</span><span class="sxs-lookup"><span data-stu-id="9f6a8-381">Stop the tracing session when you're finished:</span></span>

  ```powershell
  Stop-EtwTraceSession -Name "MyAppTrace"
  ```

<span data-ttu-id="9f6a8-382">Resultante *C:\trace.etl* arquivo pode ser analisado com PerfView como em outras edições do Windows.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-382">The resulting *C:\trace.etl* file can be analyzed with PerfView as on other editions of Windows.</span></span>

<a id="eventlog"></a>
### <a name="the-windows-eventlog-provider"></a><span data-ttu-id="9f6a8-383">O provedor de log de eventos do Windows</span><span class="sxs-lookup"><span data-stu-id="9f6a8-383">The Windows EventLog provider</span></span>

<span data-ttu-id="9f6a8-384">O [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) pacote provedor envia a saída do log no Log de eventos do Windows.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-384">The [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) provider package sends log output to the Windows Event Log.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9f6a8-385">ASP.NET Core 2. x</span><span class="sxs-lookup"><span data-stu-id="9f6a8-385">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddEventLog()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9f6a8-386">ASP.NET Core 1. x</span><span class="sxs-lookup"><span data-stu-id="9f6a8-386">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddEventLog()
```

<span data-ttu-id="9f6a8-387">[Sobrecargas de AddEventLog](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.eventloggerfactoryextensions) permitem que você transmitir `EventLogSettings` ou um nível de log mínimo.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-387">[AddEventLog overloads](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.eventloggerfactoryextensions) let you pass in `EventLogSettings` or a minimum log level.</span></span>

---

<a id="tracesource"></a>
### <a name="the-tracesource-provider"></a><span data-ttu-id="9f6a8-388">O provedor de TraceSource</span><span class="sxs-lookup"><span data-stu-id="9f6a8-388">The TraceSource provider</span></span>

<span data-ttu-id="9f6a8-389">O [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) provedor pacote usa a [System.Diagnostics.TraceSource](https://msdn.microsoft.com/library/system.diagnostics.tracesource.aspx) bibliotecas e provedores.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-389">The [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) provider package uses the [System.Diagnostics.TraceSource](https://msdn.microsoft.com/library/system.diagnostics.tracesource.aspx) libraries and providers.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9f6a8-390">ASP.NET Core 2. x</span><span class="sxs-lookup"><span data-stu-id="9f6a8-390">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddTraceSource(sourceSwitchName);
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9f6a8-391">ASP.NET Core 1. x</span><span class="sxs-lookup"><span data-stu-id="9f6a8-391">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddTraceSource(sourceSwitchName);
```

---

<span data-ttu-id="9f6a8-392">[Sobrecargas de AddTraceSource](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.tracesourcefactoryextensions) permitem que você passa uma alternância de origem e um ouvinte de rastreamento.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-392">[AddTraceSource overloads](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.tracesourcefactoryextensions) let you pass in a source switch and a trace listener.</span></span>

<span data-ttu-id="9f6a8-393">Para usar esse provedor, um aplicativo deve ser executado no .NET Framework (em vez de .NET Core).</span><span class="sxs-lookup"><span data-stu-id="9f6a8-393">To use this provider, an application has to run on the .NET Framework (rather than .NET Core).</span></span> <span data-ttu-id="9f6a8-394">O provedor permite rotear mensagens para uma variedade de [ouvintes](https://msdn.microsoft.com/library/4y5y10s7), como o [TextWriterTraceListener](https://msdn.microsoft.com/library/system.diagnostics.textwritertracelistener) usados no aplicativo de exemplo.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-394">The provider lets you route messages to a variety of [listeners](https://msdn.microsoft.com/library/4y5y10s7), such as the [TextWriterTraceListener](https://msdn.microsoft.com/library/system.diagnostics.textwritertracelistener) used in the sample application.</span></span>

<span data-ttu-id="9f6a8-395">O exemplo a seguir configura um `TraceSource` provedor que registra `Warning` e superior mensagens na janela de console.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-395">The following example configures a `TraceSource` provider that logs `Warning` and higher messages to the console window.</span></span>

[!code-csharp[](logging/sample/Startup.cs?name=snippet_TraceSource&highlight=9-12)]

<a id="appservice"></a>
### <a name="the-azure-app-service-provider"></a><span data-ttu-id="9f6a8-396">O provedor de serviço de aplicativo do Azure</span><span class="sxs-lookup"><span data-stu-id="9f6a8-396">The Azure App Service provider</span></span>

<span data-ttu-id="9f6a8-397">O [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) pacote provedor grava logs para arquivos de texto no sistema de arquivos do aplicativo do serviço de aplicativo do Azure e ao [armazenamento de blob](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) em uma conta de armazenamento do Azure.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-397">The [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) provider package writes logs to text files in an Azure App Service app's file system and to [blob storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) in an Azure Storage account.</span></span> <span data-ttu-id="9f6a8-398">O provedor está disponível apenas para aplicativos que se destinam a ASP.NET Core 1.1.0 ou superior.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-398">The provider is available only for apps that target ASP.NET Core 1.1.0 or higher.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9f6a8-399">ASP.NET Core 2. x</span><span class="sxs-lookup"><span data-stu-id="9f6a8-399">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

> [!NOTE]
> <span data-ttu-id="9f6a8-400">Núcleo do ASP.NET 2.0 está em visualização.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-400">ASP.NET Core 2.0 is in preview.</span></span>  <span data-ttu-id="9f6a8-401">Os aplicativos criados com a versão de visualização mais recente podem não ser executado quando implantado em um serviço de aplicativo do Azure.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-401">Apps created with the latest preview release may not run when deployed to Azure App Service.</span></span> <span data-ttu-id="9f6a8-402">Quando o ASP.NET Core 2.0 foi lançado, o serviço de aplicativo do Azure executará 2.0 aplicativos e o serviço de aplicativo do Azure provedor funcionará conforme o indicado aqui.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-402">When ASP.NET Core 2.0 is released, Azure App Service will run 2.0 apps, and the Azure App Service provider will work as indicated here.</span></span>

<span data-ttu-id="9f6a8-403">Você não precisa instalar o pacote de provedor ou a chamada a `AddAzureWebAppDiagnostics` método de extensão.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-403">You don't have to install the provider package or call the `AddAzureWebAppDiagnostics` extension method.</span></span>  <span data-ttu-id="9f6a8-404">O provedor está automaticamente disponível para seu aplicativo quando você implanta o aplicativo do serviço de aplicativo do Azure.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-404">The provider is automatically available to your app when you deploy the app to Azure App Service.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9f6a8-405">ASP.NET Core 1. x</span><span class="sxs-lookup"><span data-stu-id="9f6a8-405">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddAzureWebAppDiagnostics();
```

<span data-ttu-id="9f6a8-406">Um `AddAzureWebAppDiagnostics` sobrecarga permite que você passe [AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs), com a qual você pode substituir as configurações padrão como o modelo de saída de log, o nome de blob e o limite de tamanho de arquivo.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-406">An `AddAzureWebAppDiagnostics` overload lets you pass in [AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs), with which you can override default settings such as the logging output template, blob name, and file size limit.</span></span> <span data-ttu-id="9f6a8-407">(*Modelo saída* é uma cadeia de caracteres de formato de mensagem que é aplicada a todos os logs, na parte superior que você fornece ao chamar um `ILogger` método.)</span><span class="sxs-lookup"><span data-stu-id="9f6a8-407">(*Output template* is a message format string that is applied to all logs, on top of the one that you provide when you call an `ILogger` method.)</span></span>  

---

<span data-ttu-id="9f6a8-408">Quando você implanta um aplicativo de serviço de aplicativo, seu aplicativo respeita as configurações a [Logs de diagnóstico](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag) seção o **do serviço de aplicativo** página do portal do Azure.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-408">When you deploy to an App Service app, your application honors the settings in the [Diagnostic Logs](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag) section of the **App Service** page of the Azure portal.</span></span> <span data-ttu-id="9f6a8-409">Quando você alterar essas configurações, as alterações entram em vigor imediatamente sem exigir que você reinicie o aplicativo ou reimplantar o código para ele.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-409">When you change those settings, the changes take effect immediately without requiring that you restart the app or redeploy code to it.</span></span> 

![Configurações de log do Azure](logging/_static/azure-logging-settings.png)

<span data-ttu-id="9f6a8-411">O local padrão para arquivos de log é o *unidade d:\\inicial\\LogFiles\\aplicativo* pasta e o nome de arquivo padrão é *yyyymmdd.txt diagnóstico*.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-411">The default location for log files is in the *D:\\home\\LogFiles\\Application* folder, and the default file name is *diagnostics-yyyymmdd.txt*.</span></span> <span data-ttu-id="9f6a8-412">O limite de tamanho de arquivo padrão é 10 MB, e o número de máximo padrão de arquivos mantidos é 2.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-412">The default file size limit is 10 MB, and the default maximum number of files retained is 2.</span></span> <span data-ttu-id="9f6a8-413">O nome de blob padrão é *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-413">The default blob name is *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*.</span></span> <span data-ttu-id="9f6a8-414">Para obter mais informações sobre o comportamento padrão, consulte [AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs).</span><span class="sxs-lookup"><span data-stu-id="9f6a8-414">For more information about default behavior, see [AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs).</span></span>

<span data-ttu-id="9f6a8-415">O provedor funciona somente quando o projeto é executado no ambiente do Azure.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-415">The provider only works when your project runs in the Azure environment.</span></span>  <span data-ttu-id="9f6a8-416">Ele não tem nenhum efeito quando é executado localmente &mdash; não gravará arquivos locais ou armazenamento de desenvolvimento local para blobs.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-416">It has no effect when you run locally &mdash; it does not write to local files or local development storage for blobs.</span></span>

## <a name="third-party-logging-providers"></a><span data-ttu-id="9f6a8-417">Provedores de log de terceiros</span><span class="sxs-lookup"><span data-stu-id="9f6a8-417">Third-party logging providers</span></span>

<span data-ttu-id="9f6a8-418">Aqui estão algumas estruturas de registro em log de terceiros que funcionam com o ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="9f6a8-418">Here are some third-party logging frameworks that work with ASP.NET Core:</span></span>

* <span data-ttu-id="9f6a8-419">[ELMAH.IO](https://github.com/elmahio/Elmah.Io.Extensions.Logging) -provedor para o serviço Elmah.Io</span><span class="sxs-lookup"><span data-stu-id="9f6a8-419">[elmah.io](https://github.com/elmahio/Elmah.Io.Extensions.Logging) - provider for the Elmah.Io service</span></span>

* <span data-ttu-id="9f6a8-420">[JSNLog](http://jsnlog.com) -registra em log as exceções de JavaScript e outros eventos do lado do cliente no registro do servidor.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-420">[JSNLog](http://jsnlog.com) - logs JavaScript exceptions and other client-side events in your server-side log.</span></span>

* <span data-ttu-id="9f6a8-421">[Loggr](https://github.com/imobile3/Loggr.Extensions.Logging) -provedor para o serviço Loggr</span><span class="sxs-lookup"><span data-stu-id="9f6a8-421">[Loggr](https://github.com/imobile3/Loggr.Extensions.Logging) - provider for the Loggr service</span></span>

* <span data-ttu-id="9f6a8-422">[NLog](https://github.com/NLog/NLog.Extensions.Logging) -provedor para a biblioteca de NLog</span><span class="sxs-lookup"><span data-stu-id="9f6a8-422">[NLog](https://github.com/NLog/NLog.Extensions.Logging) - provider for the NLog library</span></span>

* <span data-ttu-id="9f6a8-423">[Serilog](https://github.com/serilog/serilog-framework-logging) -provedor para a biblioteca de Serilog</span><span class="sxs-lookup"><span data-stu-id="9f6a8-423">[Serilog](https://github.com/serilog/serilog-framework-logging) - provider for the Serilog library</span></span>

<span data-ttu-id="9f6a8-424">Algumas estruturas de terceiros podem fazer [log semântico, também conhecido como registro em log estruturado](http://programmers.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="9f6a8-424">Some third-party frameworks can do [semantic logging, also known as structured logging](http://programmers.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="9f6a8-425">Usando uma estrutura de terceiros é semelhante ao uso de um dos provedores internos: adicionar um pacote NuGet ao seu projeto e chamar um método de extensão em `ILoggerFactory`.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-425">Using a third-party framework is similar to using one of the built-in providers: add a NuGet package to your project and call an extension method on `ILoggerFactory`.</span></span> <span data-ttu-id="9f6a8-426">Para obter mais informações, consulte a documentação de cada estrutura.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-426">For more information, see each framework's documentation.</span></span>

<span data-ttu-id="9f6a8-427">Você pode criar seus próprios provedores personalizados, para dar suporte a outras estruturas de registro em log ou seus próprios requisitos de registro em log.</span><span class="sxs-lookup"><span data-stu-id="9f6a8-427">You can create your own custom providers as well, to support other logging frameworks or your own logging requirements.</span></span>

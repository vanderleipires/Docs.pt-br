---
title: Registro em log no ASP.NET Core
author: ardalis
description: Saiba mais sobre a estrutura de registros no ASP.NET Core. Descubra os provedores de log internos e saiba mais sobre os provedores de terceiros populares.
ms.author: tdykstra
manager: wpickett
ms.date: 12/15/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/logging/index
ms.openlocfilehash: af8364c584b686fd5c0fe30a89e241d9d08a30c0
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
# <a name="introduction-to-logging-in-aspnet-core"></a><span data-ttu-id="f6f44-104">Introdução ao registro em log no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f6f44-104">Introduction to logging in ASP.NET Core</span></span>

<span data-ttu-id="f6f44-105">Por [Steve Smith](https://ardalis.com/) e [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="f6f44-105">By [Steve Smith](https://ardalis.com/) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="f6f44-106">O ASP.NET Core dá suporte a uma API de registro em log que funciona com uma variedade de provedores de logs.</span><span class="sxs-lookup"><span data-stu-id="f6f44-106">ASP.NET Core supports a logging API that works with a variety of logging providers.</span></span> <span data-ttu-id="f6f44-107">Os provedores internos permitem que você envie logs para um ou mais destinos e você pode se conectar a uma estrutura de registros de terceiros.</span><span class="sxs-lookup"><span data-stu-id="f6f44-107">Built-in providers let you send logs to one or more destinations, and you can plug in a third-party logging framework.</span></span> <span data-ttu-id="f6f44-108">Este artigo mostra como usar a API de registro em log e os provedores internos em seu código.</span><span class="sxs-lookup"><span data-stu-id="f6f44-108">This article shows how to use the built-in logging API and providers in your code.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f6f44-109">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f6f44-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="f6f44-110">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample2) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f6f44-110">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample2) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f6f44-111">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f6f44-111">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="f6f44-112">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f6f44-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

---

## <a name="how-to-create-logs"></a><span data-ttu-id="f6f44-113">Como criar logs</span><span class="sxs-lookup"><span data-stu-id="f6f44-113">How to create logs</span></span>

<span data-ttu-id="f6f44-114">Para criar logs, obtenha um objeto `ILogger` do contêiner de [injeção de dependência](xref:fundamentals/dependency-injection):</span><span class="sxs-lookup"><span data-stu-id="f6f44-114">To create logs, get an `ILogger` object from the [dependency injection](xref:fundamentals/dependency-injection) container:</span></span>

[!code-csharp[](index/sample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

<span data-ttu-id="f6f44-115">Em seguida, chame métodos de registro em log nesse objeto de agente:</span><span class="sxs-lookup"><span data-stu-id="f6f44-115">Then call logging methods on that logger object:</span></span>

[!code-csharp[](index/sample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="f6f44-116">Este exemplo cria logs com a classe `TodoController` como a *categoria*.</span><span class="sxs-lookup"><span data-stu-id="f6f44-116">This example creates logs with the `TodoController` class as the *category*.</span></span> <span data-ttu-id="f6f44-117">As categorias serão explicadas [posteriormente neste artigo](#log-category).</span><span class="sxs-lookup"><span data-stu-id="f6f44-117">Categories are explained [later in this article](#log-category).</span></span>

<span data-ttu-id="f6f44-118">O ASP.NET Core não fornece métodos de agente assíncronos porque o registro em log deve ser tão rápido que não valeria a pena usar assíncronos.</span><span class="sxs-lookup"><span data-stu-id="f6f44-118">ASP.NET Core doesn't provide async logger methods because logging should be so fast that it isn't worth the cost of using async.</span></span> <span data-ttu-id="f6f44-119">Se você estiver em uma situação em que isso não é verdadeiro, considere alterar a maneira como você faz logs.</span><span class="sxs-lookup"><span data-stu-id="f6f44-119">If you're in a situation where that's not true, consider changing the way you log.</span></span> <span data-ttu-id="f6f44-120">Se seu armazenamento de dados é lento, grave as mensagens de log em um repositório rápido primeiro e, depois, mova-as para um repositório lento.</span><span class="sxs-lookup"><span data-stu-id="f6f44-120">If your data store is slow, write the log messages to a fast store first, then move them to a slow store later.</span></span> <span data-ttu-id="f6f44-121">Por exemplo, registre em uma fila de mensagens que seja lida e persistida em um armazenamento lento por outro processo.</span><span class="sxs-lookup"><span data-stu-id="f6f44-121">For example, log to a message queue that's read and persisted to slow storage by another process.</span></span>

## <a name="how-to-add-providers"></a><span data-ttu-id="f6f44-122">Como adicionar provedores</span><span class="sxs-lookup"><span data-stu-id="f6f44-122">How to add providers</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f6f44-123">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f6f44-123">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="f6f44-124">Um provedor de logs obtém as mensagens que você cria com um objeto `ILogger` e as exibe ou armazena.</span><span class="sxs-lookup"><span data-stu-id="f6f44-124">A logging provider takes the messages that you create with an `ILogger` object and displays or stores them.</span></span> <span data-ttu-id="f6f44-125">Por exemplo, o provedor Console exibe as mensagens no console e o provedor do Serviço de Aplicativo do Azure pode armazená-las no armazenamento de blobs do Azure.</span><span class="sxs-lookup"><span data-stu-id="f6f44-125">For example, the Console provider displays messages on the console, and the Azure App Service provider can store them in Azure blob storage.</span></span>

<span data-ttu-id="f6f44-126">Para usar um provedor, chame o método de extensão `Add<ProviderName>` do provedor no *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="f6f44-126">To use a provider, call the provider's `Add<ProviderName>` extension method in *Program.cs*:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_ExpandDefault&highlight=16,17)]

<span data-ttu-id="f6f44-127">O modelo de projeto padrão permite o registro em log com o método [CreateDefaultBuilder](https://docs.microsoft.com/ dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder?view=aspnetcore-2.0#Microsoft_AspNetCore_WebHost_CreateDefaultBuilder_System_String___):</span><span class="sxs-lookup"><span data-stu-id="f6f44-127">The default project template enables logging with the [CreateDefaultBuilder](https://docs.microsoft.com/ dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder?view=aspnetcore-2.0#Microsoft_AspNetCore_WebHost_CreateDefaultBuilder_System_String___) method:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_TemplateCode&highlight=7)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f6f44-128">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f6f44-128">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="f6f44-129">Um provedor de logs obtém as mensagens que você cria com um objeto `ILogger` e as exibe ou armazena.</span><span class="sxs-lookup"><span data-stu-id="f6f44-129">A logging provider takes the messages that you create with an `ILogger` object and displays or stores them.</span></span> <span data-ttu-id="f6f44-130">Por exemplo, o provedor Console exibe as mensagens no console e o provedor do Serviço de Aplicativo do Azure pode armazená-las no armazenamento de blobs do Azure.</span><span class="sxs-lookup"><span data-stu-id="f6f44-130">For example, the Console provider displays messages on the console, and the Azure App Service provider can store them in Azure blob storage.</span></span>

<span data-ttu-id="f6f44-131">Para usar um provedor, instale o pacote NuGet e chame o método de extensão do provedor em uma instância de `ILoggerFactory`, conforme mostrado no exemplo a seguir.</span><span class="sxs-lookup"><span data-stu-id="f6f44-131">To use a provider, install its NuGet package and call the provider's extension method on an instance of `ILoggerFactory`, as shown in the following example.</span></span>

[!code-csharp[](index/sample//Startup.cs?name=snippet_AddConsoleAndDebug&highlight=3,5-7)]

<span data-ttu-id="f6f44-132">A [DI](xref:fundamentals/dependency-injection) (injeção de dependência) do ASP.NET Core fornece a instância de `ILoggerFactory`.</span><span class="sxs-lookup"><span data-stu-id="f6f44-132">ASP.NET Core [dependency injection](xref:fundamentals/dependency-injection) (DI) provides the `ILoggerFactory` instance.</span></span> <span data-ttu-id="f6f44-133">Os métodos de extensão `AddConsole` e `AddDebug` são definidos nos pacotes [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) e [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/).</span><span class="sxs-lookup"><span data-stu-id="f6f44-133">The `AddConsole` and `AddDebug` extension methods are defined in the [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) and [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/) packages.</span></span> <span data-ttu-id="f6f44-134">Cada método de extensão chama o método `ILoggerFactory.AddProvider`, passando uma instância do provedor.</span><span class="sxs-lookup"><span data-stu-id="f6f44-134">Each extension method calls the `ILoggerFactory.AddProvider` method, passing in an instance of the provider.</span></span> 

> [!NOTE]
> <span data-ttu-id="f6f44-135">O aplicativo de exemplo deste artigo adiciona provedores de log no método `Configure` da classe `Startup`.</span><span class="sxs-lookup"><span data-stu-id="f6f44-135">The sample application for this article adds logging providers in the `Configure` method of the `Startup` class.</span></span> <span data-ttu-id="f6f44-136">Se você quiser obter saída de log de código que é executado anteriormente, adicione provedores de log no construtor de classe `Startup`.</span><span class="sxs-lookup"><span data-stu-id="f6f44-136">If you want to get log output from code that executes earlier, add logging providers in the `Startup` class constructor instead.</span></span> 

---

<span data-ttu-id="f6f44-137">Você encontrará informações sobre cada [provedor de log interno](#built-in-logging-providers) e links para [provedores de log de terceiros](#third-party-logging-providers) mais adiante no artigo.</span><span class="sxs-lookup"><span data-stu-id="f6f44-137">You'll find information about each [built-in logging provider](#built-in-logging-providers) and links to [third-party logging providers](#third-party-logging-providers) later in the article.</span></span>

## <a name="sample-logging-output"></a><span data-ttu-id="f6f44-138">Exemplo de saída de registro em log</span><span class="sxs-lookup"><span data-stu-id="f6f44-138">Sample logging output</span></span>

<span data-ttu-id="f6f44-139">Com o código de exemplo mostrado na seção anterior, você verá logs no console ao executar através da linha de comando.</span><span class="sxs-lookup"><span data-stu-id="f6f44-139">With the sample code shown in the preceding section, you'll see logs in the console when you run from the command line.</span></span> <span data-ttu-id="f6f44-140">Aqui está um exemplo da saída do console:</span><span class="sxs-lookup"><span data-stu-id="f6f44-140">Here's an example of console output:</span></span>

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
 
<span data-ttu-id="f6f44-141">Esses logs foram criados acessando `http://localhost:5000/api/todo/0`, que dispara a execução das duas chamadas `ILogger` mostradas na seção anterior.</span><span class="sxs-lookup"><span data-stu-id="f6f44-141">These logs were created by going to `http://localhost:5000/api/todo/0`, which triggers execution of both `ILogger` calls shown in the preceding section.</span></span>

<span data-ttu-id="f6f44-142">Aqui está um exemplo de como os mesmos logs aparecem na janela Depuração quando você executa o aplicativo de exemplo no Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="f6f44-142">Here's an example of the same logs as they appear in the Debug window when you run the sample application in Visual Studio:</span></span>

```console
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request starting HTTP/1.1 GET http://localhost:53104/api/todo/0  
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
TodoApi.Controllers.TodoController:Information: Getting item 0
TodoApi.Controllers.TodoController:Warning: GetById(0) NOT FOUND
Microsoft.AspNetCore.Mvc.StatusCodeResult:Information: Executing HttpStatusCodeResult, setting HTTP status code 404
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 152.5657ms
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request finished in 316.3195ms 404 
```

<span data-ttu-id="f6f44-143">Os logs criados pelas chamadas `ILogger` mostradas na seção anterior começam com "TodoApi.Controllers.TodoController".</span><span class="sxs-lookup"><span data-stu-id="f6f44-143">The logs that were created by the `ILogger` calls shown in the preceding section begin with "TodoApi.Controllers.TodoController".</span></span> <span data-ttu-id="f6f44-144">Os logs que começam com categorias "Microsoft" são do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f6f44-144">The logs that begin with "Microsoft" categories are from ASP.NET Core.</span></span> <span data-ttu-id="f6f44-145">O próprio ASP.NET Core e o código do aplicativo estão usando a mesma API de registro em log e os mesmos provedores de log.</span><span class="sxs-lookup"><span data-stu-id="f6f44-145">ASP.NET Core itself and your application code are using the same logging API and the same logging providers.</span></span>

<span data-ttu-id="f6f44-146">O restante deste artigo explica alguns detalhes e opções para registro em log.</span><span class="sxs-lookup"><span data-stu-id="f6f44-146">The remainder of this article explains some details and options for logging.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="f6f44-147">Pacotes NuGet</span><span class="sxs-lookup"><span data-stu-id="f6f44-147">NuGet packages</span></span>

<span data-ttu-id="f6f44-148">As interfaces `ILogger` e `ILoggerFactory` estão em [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/) e as implementações padrão para elas estão em [Microsoft.Extensions.Logging](https://www.nuget.org/packages/Microsoft.Extensions.Logging/).</span><span class="sxs-lookup"><span data-stu-id="f6f44-148">The `ILogger` and `ILoggerFactory` interfaces are in [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), and default implementations for them are in [Microsoft.Extensions.Logging](https://www.nuget.org/packages/Microsoft.Extensions.Logging/).</span></span>

## <a name="log-category"></a><span data-ttu-id="f6f44-149">Categoria de log</span><span class="sxs-lookup"><span data-stu-id="f6f44-149">Log category</span></span>

<span data-ttu-id="f6f44-150">Um *categoria* é incluída com cada log que você cria.</span><span class="sxs-lookup"><span data-stu-id="f6f44-150">A *category* is included with each log that you create.</span></span> <span data-ttu-id="f6f44-151">Você especifica a categoria ao criar um objeto `ILogger`.</span><span class="sxs-lookup"><span data-stu-id="f6f44-151">You specify the category when you create an `ILogger` object.</span></span> <span data-ttu-id="f6f44-152">A categoria pode ser qualquer cadeia de caracteres, mas uma convenção é usar o nome totalmente qualificado da classe da qual os logs são gravados.</span><span class="sxs-lookup"><span data-stu-id="f6f44-152">The category may be any string, but a convention is to use the fully qualified name of the class from which the logs are written.</span></span> <span data-ttu-id="f6f44-153">Por exemplo: "TodoApi.Controllers.TodoController".</span><span class="sxs-lookup"><span data-stu-id="f6f44-153">For example: "TodoApi.Controllers.TodoController".</span></span>

<span data-ttu-id="f6f44-154">Você pode especificar a categoria como uma cadeia de caracteres ou usar um método de extensão que deriva a categoria do tipo.</span><span class="sxs-lookup"><span data-stu-id="f6f44-154">You can specify the category as a string or use an extension method that derives the category from the type.</span></span> <span data-ttu-id="f6f44-155">Para especificar a categoria como uma cadeia de caracteres, chame `CreateLogger` em uma instância de `ILoggerFactory`, conforme mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="f6f44-155">To specify the category as a string, call `CreateLogger` on an `ILoggerFactory` instance, as shown below.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

<span data-ttu-id="f6f44-156">Na maioria das vezes será mais fácil usar `ILogger<T>`, conforme mostrado no exemplo a seguir.</span><span class="sxs-lookup"><span data-stu-id="f6f44-156">Most of the time, it will be easier to use `ILogger<T>`, as in the following example.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

<span data-ttu-id="f6f44-157">Isso é equivalente a chamar `CreateLogger` com o nome de tipo totalmente qualificado de `T`.</span><span class="sxs-lookup"><span data-stu-id="f6f44-157">This is equivalent to calling `CreateLogger` with the fully qualified type name of `T`.</span></span>

## <a name="log-level"></a><span data-ttu-id="f6f44-158">Nível de log</span><span class="sxs-lookup"><span data-stu-id="f6f44-158">Log level</span></span>

<span data-ttu-id="f6f44-159">Sempre que você grava um log, você especifica o [LogLevel](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel).</span><span class="sxs-lookup"><span data-stu-id="f6f44-159">Each time you write a log, you specify its [LogLevel](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel).</span></span> <span data-ttu-id="f6f44-160">O nível de log indica o grau de gravidade ou importância.</span><span class="sxs-lookup"><span data-stu-id="f6f44-160">The log level indicates the degree of severity or importance.</span></span> <span data-ttu-id="f6f44-161">Por exemplo, você pode gravar um log `Information` quando um método é finalizado normalmente, um log `Warning` quando um método retorna um código de retorno 404 e um log `Error` ao capturar uma exceção inesperada.</span><span class="sxs-lookup"><span data-stu-id="f6f44-161">For example, you might write an `Information` log when a method ends normally, a `Warning` log when a method returns a 404 return code, and an `Error` log when you catch an unexpected exception.</span></span>

<span data-ttu-id="f6f44-162">No exemplo de código a seguir, os nomes dos métodos (por exemplo, `LogWarning`) especificam o nível de log.</span><span class="sxs-lookup"><span data-stu-id="f6f44-162">In the following code example, the names of the methods (for example, `LogWarning`) specify the log level.</span></span> <span data-ttu-id="f6f44-163">O primeiro parâmetro é a [ID de evento de log](#log-event-id).</span><span class="sxs-lookup"><span data-stu-id="f6f44-163">The first parameter is the [Log event ID](#log-event-id).</span></span> <span data-ttu-id="f6f44-164">O segundo parâmetro é um [modelo de mensagem](#log-message-template) com espaços reservados para valores de argumento fornecidos pelos parâmetros de método restantes.</span><span class="sxs-lookup"><span data-stu-id="f6f44-164">The second parameter is a [message template](#log-message-template) with placeholders for argument values provided by the remaining method parameters.</span></span> <span data-ttu-id="f6f44-165">Os parâmetros de método serão explicados com mais detalhes posteriormente neste artigo.</span><span class="sxs-lookup"><span data-stu-id="f6f44-165">The method parameters are explained in more detail later in this article.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="f6f44-166">Os métodos de log que incluem o nível no nome do método são [métodos de extensão para ILogger](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loggerextensions).</span><span class="sxs-lookup"><span data-stu-id="f6f44-166">Log methods that include the level in the method name are [extension methods for ILogger](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loggerextensions).</span></span> <span data-ttu-id="f6f44-167">Nos bastidores, esses métodos chamam um método `Log` que recebe um parâmetro `LogLevel`.</span><span class="sxs-lookup"><span data-stu-id="f6f44-167">Behind the scenes, these methods call a `Log` method that takes a `LogLevel` parameter.</span></span> <span data-ttu-id="f6f44-168">Você pode chamar o método `Log` diretamente em vez de um desses métodos de extensão, mas a sintaxe é relativamente complicada.</span><span class="sxs-lookup"><span data-stu-id="f6f44-168">You can call the `Log` method directly rather than one of these extension methods, but the syntax is relatively complicated.</span></span> <span data-ttu-id="f6f44-169">Para obter mais informações, consulte a [interface ILogger](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.ilogger) e o [código-fonte de extensões de agente](https://github.com/aspnet/Logging/blob/master/src/Microsoft.Extensions.Logging.Abstractions/LoggerExtensions.cs).</span><span class="sxs-lookup"><span data-stu-id="f6f44-169">For more information, see the [ILogger interface](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.ilogger) and the [logger extensions source code](https://github.com/aspnet/Logging/blob/master/src/Microsoft.Extensions.Logging.Abstractions/LoggerExtensions.cs).</span></span>

<span data-ttu-id="f6f44-170">O ASP.NET Core define os seguintes [níveis de log](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel), ordenados aqui da menor para a maior gravidade.</span><span class="sxs-lookup"><span data-stu-id="f6f44-170">ASP.NET Core defines the following [log levels](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel), ordered here from least to highest severity.</span></span>

* <span data-ttu-id="f6f44-171">Trace = 0</span><span class="sxs-lookup"><span data-stu-id="f6f44-171">Trace = 0</span></span>

  <span data-ttu-id="f6f44-172">Para obter informações valiosas somente para um desenvolvedor que esteja depurando um problema.</span><span class="sxs-lookup"><span data-stu-id="f6f44-172">For information that's valuable only to a developer debugging an issue.</span></span> <span data-ttu-id="f6f44-173">Essas mensagens podem conter dados confidenciais de aplicativos e, portanto, não devem ser habilitadas em um ambiente de produção.</span><span class="sxs-lookup"><span data-stu-id="f6f44-173">These messages may contain sensitive application data and so shouldn't be enabled in a production environment.</span></span> <span data-ttu-id="f6f44-174">*Desabilitado por padrão.*</span><span class="sxs-lookup"><span data-stu-id="f6f44-174">*Disabled by default.*</span></span> <span data-ttu-id="f6f44-175">Exemplo: `Credentials: {"User":"someuser", "Password":"P@ssword"}`</span><span class="sxs-lookup"><span data-stu-id="f6f44-175">Example: `Credentials: {"User":"someuser", "Password":"P@ssword"}`</span></span>

* <span data-ttu-id="f6f44-176">Debug = 1</span><span class="sxs-lookup"><span data-stu-id="f6f44-176">Debug = 1</span></span>

  <span data-ttu-id="f6f44-177">Para informações que tenham utilidade de curto prazo durante o desenvolvimento e a depuração.</span><span class="sxs-lookup"><span data-stu-id="f6f44-177">For information that has short-term usefulness during development and debugging.</span></span> <span data-ttu-id="f6f44-178">Exemplo: `Entering method Configure with flag set to true.` Você normalmente não habilitaria logs de nível `Debug` em produção, a menos que estivesse solucionando problemas, devido ao alto volume de logs.</span><span class="sxs-lookup"><span data-stu-id="f6f44-178">Example: `Entering method Configure with flag set to true.` You typically wouldn't enable `Debug` level logs in production unless you are troubleshooting, due to the high volume of logs.</span></span>

* <span data-ttu-id="f6f44-179">Information = 2</span><span class="sxs-lookup"><span data-stu-id="f6f44-179">Information = 2</span></span>

  <span data-ttu-id="f6f44-180">Para rastrear o fluxo geral do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="f6f44-180">For tracking the general flow of the application.</span></span> <span data-ttu-id="f6f44-181">Esses logs normalmente têm algum valor a longo prazo.</span><span class="sxs-lookup"><span data-stu-id="f6f44-181">These logs typically have some long-term value.</span></span> <span data-ttu-id="f6f44-182">Exemplo: `Request received for path /api/todo`</span><span class="sxs-lookup"><span data-stu-id="f6f44-182">Example: `Request received for path /api/todo`</span></span>

* <span data-ttu-id="f6f44-183">Warning = 3</span><span class="sxs-lookup"><span data-stu-id="f6f44-183">Warning = 3</span></span>

  <span data-ttu-id="f6f44-184">Para eventos anormais ou inesperados no fluxo de aplicativo.</span><span class="sxs-lookup"><span data-stu-id="f6f44-184">For abnormal or unexpected events in the application flow.</span></span> <span data-ttu-id="f6f44-185">Eles podem incluir erros ou outras condições que não fazem com que o aplicativo pare, mas que talvez precisem ser investigados.</span><span class="sxs-lookup"><span data-stu-id="f6f44-185">These may include errors or other conditions that don't cause the application to stop, but which may need to be investigated.</span></span> <span data-ttu-id="f6f44-186">Exceções manipuladas são um local comum para usar o nível de log `Warning`.</span><span class="sxs-lookup"><span data-stu-id="f6f44-186">Handled exceptions are a common place to use the `Warning` log level.</span></span> <span data-ttu-id="f6f44-187">Exemplo: `FileNotFoundException for file quotes.txt.`</span><span class="sxs-lookup"><span data-stu-id="f6f44-187">Example: `FileNotFoundException for file quotes.txt.`</span></span>

* <span data-ttu-id="f6f44-188">Error = 4</span><span class="sxs-lookup"><span data-stu-id="f6f44-188">Error = 4</span></span>

  <span data-ttu-id="f6f44-189">Para erros e exceções que não podem ser manipulados.</span><span class="sxs-lookup"><span data-stu-id="f6f44-189">For errors and exceptions that cannot be handled.</span></span> <span data-ttu-id="f6f44-190">Essas mensagens indicam uma falha na atividade ou na operação atual (como a solicitação HTTP atual) e não uma falha em todo o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="f6f44-190">These messages indicate a failure in the current activity or operation (such as the current HTTP request), not an application-wide failure.</span></span> <span data-ttu-id="f6f44-191">Mensagem de log de exemplo:`Cannot insert record due to duplicate key violation.`</span><span class="sxs-lookup"><span data-stu-id="f6f44-191">Example log message: `Cannot insert record due to duplicate key violation.`</span></span>

* <span data-ttu-id="f6f44-192">Critical = 5</span><span class="sxs-lookup"><span data-stu-id="f6f44-192">Critical = 5</span></span>

  <span data-ttu-id="f6f44-193">Para falhas que exigem atenção imediata.</span><span class="sxs-lookup"><span data-stu-id="f6f44-193">For failures that require immediate attention.</span></span> <span data-ttu-id="f6f44-194">Exemplos: cenários de perda de dados, espaço em disco insuficiente.</span><span class="sxs-lookup"><span data-stu-id="f6f44-194">Examples: data loss scenarios, out of disk space.</span></span>

<span data-ttu-id="f6f44-195">Você pode usar o nível de log para controlar a quantidade de saída de log que é gravada em uma mídia de armazenamento específica ou em uma janela de exibição.</span><span class="sxs-lookup"><span data-stu-id="f6f44-195">You can use the log level to control how much log output is written to a particular storage medium or display window.</span></span> <span data-ttu-id="f6f44-196">Por exemplo, em produção, você pode desejar que todos os logs de nível `Information` e inferior vão para um armazenamento de dados de volume e que todos os logs de nível `Warning` e superior vão para um armazenamento de dados de valor.</span><span class="sxs-lookup"><span data-stu-id="f6f44-196">For example, in production you might want all logs of `Information` level and lower to go to a volume data store, and all logs of `Warning` level and higher to go to a value data store.</span></span> <span data-ttu-id="f6f44-197">Durante o desenvolvimento, você normalmente poderia enviar os logs de gravidade `Warning` ou mais alta para o console.</span><span class="sxs-lookup"><span data-stu-id="f6f44-197">During development, you might normally send logs of `Warning` or higher severity to the console.</span></span> <span data-ttu-id="f6f44-198">Então, quando precisar solucionar problemas, você poderá adicionar o nível `Debug`.</span><span class="sxs-lookup"><span data-stu-id="f6f44-198">Then when you need to troubleshoot, you can add `Debug` level.</span></span> <span data-ttu-id="f6f44-199">A seção [Filtragem de log](#log-filtering) mais adiante neste artigo explicará como controlar os níveis de log que um provedor manipula.</span><span class="sxs-lookup"><span data-stu-id="f6f44-199">The [Log filtering](#log-filtering) section later in this article explains how to control which log levels a provider handles.</span></span>

<span data-ttu-id="f6f44-200">A estrutura do ASP.NET Core grava logs de nível `Debug` para eventos de estrutura.</span><span class="sxs-lookup"><span data-stu-id="f6f44-200">The ASP.NET Core framework writes `Debug` level logs for framework events.</span></span> <span data-ttu-id="f6f44-201">Os exemplos de log anteriores neste artigo excluíram logs abaixo do nível `Information`, portanto, os logs de nível `Debug` não foram mostrados.</span><span class="sxs-lookup"><span data-stu-id="f6f44-201">The log examples earlier in this article excluded logs below `Information` level, so no `Debug` level logs were shown.</span></span> <span data-ttu-id="f6f44-202">Aqui está um exemplo de logs do console, caso você execute o aplicativo de exemplo configurado para mostrar logs de nível `Debug` e superiores para o provedor de console.</span><span class="sxs-lookup"><span data-stu-id="f6f44-202">Here's an example of console logs if you run the sample application configured to show `Debug` and higher logs for the console provider.</span></span>

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

## <a name="log-event-id"></a><span data-ttu-id="f6f44-203">ID de evento de log</span><span class="sxs-lookup"><span data-stu-id="f6f44-203">Log event ID</span></span>

<span data-ttu-id="f6f44-204">Sempre que você grava um log, você pode especificar uma *ID de evento*.</span><span class="sxs-lookup"><span data-stu-id="f6f44-204">Each time you write a log, you can specify an *event ID*.</span></span> <span data-ttu-id="f6f44-205">O aplicativo de exemplo faz isso usando uma classe `LoggingEvents` definida localmente:</span><span class="sxs-lookup"><span data-stu-id="f6f44-205">The sample app does this by using a locally-defined `LoggingEvents` class:</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/sample//Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

<span data-ttu-id="f6f44-206">Uma ID de evento é um valor inteiro que pode ser usado para associar um conjunto de eventos registrados.</span><span class="sxs-lookup"><span data-stu-id="f6f44-206">An event ID is an integer value that you can use to associate a set of logged events with one another.</span></span> <span data-ttu-id="f6f44-207">Por exemplo, um log para adicionar um item a um carrinho de compras pode ser ter a ID de evento 1000 e um log para concluir uma compra pode ter a ID de evento 1001.</span><span class="sxs-lookup"><span data-stu-id="f6f44-207">For instance, a log for adding an item to a shopping cart could be event ID 1000 and a log for completing a purchase could be event ID 1001.</span></span>

<span data-ttu-id="f6f44-208">Na saída do registro em log, a ID do evento pode ser armazenada em um campo ou incluída na mensagem de texto, dependendo do provedor.</span><span class="sxs-lookup"><span data-stu-id="f6f44-208">In logging output, the event ID may be stored in a field or included in the text message, depending on the provider.</span></span> <span data-ttu-id="f6f44-209">O provedor Depuração não mostra as IDs de evento, mas o provedor do console sim, entre colchetes depois da categoria:</span><span class="sxs-lookup"><span data-stu-id="f6f44-209">The Debug provider doesn't show event IDs, but the console provider shows them in brackets after the category:</span></span>

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-template"></a><span data-ttu-id="f6f44-210">Modelo de mensagem de log</span><span class="sxs-lookup"><span data-stu-id="f6f44-210">Log message template</span></span>

<span data-ttu-id="f6f44-211">Sempre que você grava uma mensagem de log, você fornece um modelo de mensagem.</span><span class="sxs-lookup"><span data-stu-id="f6f44-211">Each time you write a log message, you provide a message template.</span></span> <span data-ttu-id="f6f44-212">O modelo de mensagem pode ser uma cadeia de caracteres ou pode conter espaços reservados nomeados, nos quais valores de argumento serão colocados.</span><span class="sxs-lookup"><span data-stu-id="f6f44-212">The message template can be a string or it can contain named placeholders into which argument values are placed.</span></span> <span data-ttu-id="f6f44-213">O modelo não é uma cadeia de caracteres de formatação e os espaços reservados devem ser nomeados, não numerados.</span><span class="sxs-lookup"><span data-stu-id="f6f44-213">The template isn't a format string, and placeholders should be named, not numbered.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="f6f44-214">A ordem dos espaços reservados e não de seus nomes, determina quais parâmetros serão usados para fornecer seus valores.</span><span class="sxs-lookup"><span data-stu-id="f6f44-214">The order of placeholders, not their names, determines which parameters are used to provide their values.</span></span> <span data-ttu-id="f6f44-215">Se você tiver o seguinte código:</span><span class="sxs-lookup"><span data-stu-id="f6f44-215">If you have the following code:</span></span>

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

<span data-ttu-id="f6f44-216">A mensagem de log resultante terá esta aparência:</span><span class="sxs-lookup"><span data-stu-id="f6f44-216">The resulting log message looks like this:</span></span>

```
Parameter values: parm1, parm2
```

<span data-ttu-id="f6f44-217">A estrutura de registros realiza a formatação de mensagens dessa maneira para possibilitar que os provedores de log implementem o [registro em log semântico, também conhecido como registro em log estruturado](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="f6f44-217">The logging framework does message formatting in this way to make it possible for logging providers to implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span> <span data-ttu-id="f6f44-218">Como os próprios argumentos são passados para o sistema de registro em log, não apenas o modelo de mensagem formatada, mas os provedores de log, também poderão armazenar os valores de parâmetro como campos, além do modelo de mensagem.</span><span class="sxs-lookup"><span data-stu-id="f6f44-218">Because the arguments themselves are passed to the logging system, not just the formatted message template, logging providers can store the parameter values as fields in addition to the message template.</span></span> <span data-ttu-id="f6f44-219">Se você estiver direcionando a saída de log para o Armazenamento de Tabelas do Azure e sua chamada de método do agente tiver esta aparência:</span><span class="sxs-lookup"><span data-stu-id="f6f44-219">If you're directing your log output to Azure Table Storage and your logger method call looks like this:</span></span>

```csharp
_logger.LogInformation("Getting item {ID} at {RequestTime}", id, DateTime.Now);
```

<span data-ttu-id="f6f44-220">Cada entidade da Tabela do Azure poderá ter propriedades `ID` e `RequestTime`, o que simplificará as consultas nos dados de log.</span><span class="sxs-lookup"><span data-stu-id="f6f44-220">Each Azure Table entity can have `ID` and `RequestTime` properties, which simplifies queries on log data.</span></span> <span data-ttu-id="f6f44-221">Você poderá encontrar todos os logs em um determinado intervalo de `RequestTime` sem a necessidade de analisar o tempo limite da mensagem de texto.</span><span class="sxs-lookup"><span data-stu-id="f6f44-221">You can find all logs within a particular `RequestTime` range without the need to parse the time out of the text message.</span></span>

## <a name="logging-exceptions"></a><span data-ttu-id="f6f44-222">Exceções de registro em log</span><span class="sxs-lookup"><span data-stu-id="f6f44-222">Logging exceptions</span></span>

<span data-ttu-id="f6f44-223">Os métodos de agente têm sobrecargas que permitem que você passe uma exceção, como no exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="f6f44-223">The logger methods have overloads that let you pass in an exception, as in the following example:</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

<span data-ttu-id="f6f44-224">Provedores diferentes manipulam as informações de exceção de maneiras diferentes.</span><span class="sxs-lookup"><span data-stu-id="f6f44-224">Different providers handle the exception information in different ways.</span></span> <span data-ttu-id="f6f44-225">Aqui está um exemplo da saída do provedor Depuração do código mostrado acima.</span><span class="sxs-lookup"><span data-stu-id="f6f44-225">Here's an example of Debug provider output from the code shown above.</span></span>

```
TodoApi.Controllers.TodoController:Warning: GetById(036dd898-fb01-47e8-9a65-f92eb73cf924) NOT FOUND

System.Exception: Item not found exception.
 at TodoApi.Controllers.TodoController.GetById(String id) in C:\logging\sample\src\TodoApi\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a><span data-ttu-id="f6f44-226">Filtragem de log</span><span class="sxs-lookup"><span data-stu-id="f6f44-226">Log filtering</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f6f44-227">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f6f44-227">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="f6f44-228">Você pode especificar um nível de log mínimo para um provedor e uma categoria específicos ou para todos os provedores ou todas as categorias.</span><span class="sxs-lookup"><span data-stu-id="f6f44-228">You can specify a minimum log level for a specific provider and category or for all providers or all categories.</span></span> <span data-ttu-id="f6f44-229">Os logs abaixo do nível mínimo não serão passados para esse provedor, para que não sejam exibidos ou armazenados.</span><span class="sxs-lookup"><span data-stu-id="f6f44-229">Any logs below the minimum level aren't passed to that provider, so they don't get displayed or stored.</span></span> 

<span data-ttu-id="f6f44-230">Se quiser suprimir todos os logs, você poderá especificar `LogLevel.None` como o nível de log mínimo.</span><span class="sxs-lookup"><span data-stu-id="f6f44-230">If you want to suppress all logs, you can specify `LogLevel.None` as the minimum log level.</span></span> <span data-ttu-id="f6f44-231">O valor inteiro de `LogLevel.None` é 6, que é maior do que `LogLevel.Critical` (5).</span><span class="sxs-lookup"><span data-stu-id="f6f44-231">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

<span data-ttu-id="f6f44-232">**Criar regras de filtro na configuração**</span><span class="sxs-lookup"><span data-stu-id="f6f44-232">**Create filter rules in configuration**</span></span>

<span data-ttu-id="f6f44-233">Os modelos de projeto criam código que chama `CreateDefaultBuilder` para configurar o registro em log para os provedores Console e Depuração.</span><span class="sxs-lookup"><span data-stu-id="f6f44-233">The project templates create code that calls `CreateDefaultBuilder` to set up logging for the Console and Debug providers.</span></span> <span data-ttu-id="f6f44-234">O método `CreateDefaultBuilder` também configura o registro em log para procurar a configuração em uma seção `Logging`, usando código semelhante ao seguinte:</span><span class="sxs-lookup"><span data-stu-id="f6f44-234">The `CreateDefaultBuilder` method also sets up logging to look for configuration in a `Logging` section, using code like the following:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_ExpandDefault&highlight=15)]

<span data-ttu-id="f6f44-235">Os dados de configuração especificam níveis de log mínimo por provedor e por categoria, como no exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="f6f44-235">The configuration data specifies minimum log levels by provider and category, as in the following example:</span></span>

[!code-json[](index/sample2/appsettings.json)]

<span data-ttu-id="f6f44-236">Este JSON cria seis regras de filtro, uma para o provedor Depuração, quatro para o provedor Console e uma que se aplica a todos os provedores.</span><span class="sxs-lookup"><span data-stu-id="f6f44-236">This JSON creates six filter rules, one for the Debug provider, four for the Console provider, and one that applies to all providers.</span></span> <span data-ttu-id="f6f44-237">Você verá posteriormente como apenas uma dessas regras é escolhida para cada provedor quando um objeto `ILogger` é criado.</span><span class="sxs-lookup"><span data-stu-id="f6f44-237">You'll see later how just one of these rules is chosen for each provider when an `ILogger` object is created.</span></span>

<span data-ttu-id="f6f44-238">**Regras de filtro no código**</span><span class="sxs-lookup"><span data-stu-id="f6f44-238">**Filter rules in code**</span></span>

<span data-ttu-id="f6f44-239">Você pode registrar regras de filtro no código, conforme mostrado no exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="f6f44-239">You can register filter rules in code, as shown in the following example:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

<span data-ttu-id="f6f44-240">O segundo `AddFilter` especifica o provedor Depuração usando seu nome de tipo.</span><span class="sxs-lookup"><span data-stu-id="f6f44-240">The second `AddFilter` specifies the Debug provider by using its type name.</span></span> <span data-ttu-id="f6f44-241">O primeiro `AddFilter` se aplica a todos os provedores porque ele não especifica um tipo de provedor.</span><span class="sxs-lookup"><span data-stu-id="f6f44-241">The first `AddFilter` applies to all providers because it doesn't specify a provider type.</span></span>

<span data-ttu-id="f6f44-242">**Como as regras de filtragem são aplicadas**</span><span class="sxs-lookup"><span data-stu-id="f6f44-242">**How filtering rules are applied**</span></span>

<span data-ttu-id="f6f44-243">Os dados de configuração e o código `AddFilter`, mostrados nos exemplos anteriores, criam as regras mostradas na tabela a seguir.</span><span class="sxs-lookup"><span data-stu-id="f6f44-243">The configuration data and the `AddFilter` code shown in the preceding examples create the rules shown in the following table.</span></span> <span data-ttu-id="f6f44-244">As primeiras seis vêm do exemplo de configuração e as últimas duas vêm do exemplo de código.</span><span class="sxs-lookup"><span data-stu-id="f6f44-244">The first six come from the configuration example and the last two come from the code example.</span></span>

| <span data-ttu-id="f6f44-245">Número</span><span class="sxs-lookup"><span data-stu-id="f6f44-245">Number</span></span> | <span data-ttu-id="f6f44-246">Provider</span><span class="sxs-lookup"><span data-stu-id="f6f44-246">Provider</span></span>      | <span data-ttu-id="f6f44-247">Categorias que começam com...</span><span class="sxs-lookup"><span data-stu-id="f6f44-247">Categories that begin with ...</span></span>          | <span data-ttu-id="f6f44-248">Nível de log mínimo</span><span class="sxs-lookup"><span data-stu-id="f6f44-248">Minimum log level</span></span> |
| :----: | ------------- | --------------------------------------- | ----------------- |
| <span data-ttu-id="f6f44-249">1</span><span class="sxs-lookup"><span data-stu-id="f6f44-249">1</span></span>      | <span data-ttu-id="f6f44-250">Depurar</span><span class="sxs-lookup"><span data-stu-id="f6f44-250">Debug</span></span>         | <span data-ttu-id="f6f44-251">Todas as categorias</span><span class="sxs-lookup"><span data-stu-id="f6f44-251">All categories</span></span>                          | <span data-ttu-id="f6f44-252">Informações</span><span class="sxs-lookup"><span data-stu-id="f6f44-252">Information</span></span>       |
| <span data-ttu-id="f6f44-253">2</span><span class="sxs-lookup"><span data-stu-id="f6f44-253">2</span></span>      | <span data-ttu-id="f6f44-254">Console</span><span class="sxs-lookup"><span data-stu-id="f6f44-254">Console</span></span>       | <span data-ttu-id="f6f44-255">Microsoft.AspNetCore.Mvc.Razor.Internal</span><span class="sxs-lookup"><span data-stu-id="f6f44-255">Microsoft.AspNetCore.Mvc.Razor.Internal</span></span> | <span data-ttu-id="f6f44-256">Aviso</span><span class="sxs-lookup"><span data-stu-id="f6f44-256">Warning</span></span>           |
| <span data-ttu-id="f6f44-257">3</span><span class="sxs-lookup"><span data-stu-id="f6f44-257">3</span></span>      | <span data-ttu-id="f6f44-258">Console</span><span class="sxs-lookup"><span data-stu-id="f6f44-258">Console</span></span>       | <span data-ttu-id="f6f44-259">Microsoft.AspNetCore.Mvc.Razor.Razor</span><span class="sxs-lookup"><span data-stu-id="f6f44-259">Microsoft.AspNetCore.Mvc.Razor.Razor</span></span>    | <span data-ttu-id="f6f44-260">Depurar</span><span class="sxs-lookup"><span data-stu-id="f6f44-260">Debug</span></span>             |
| <span data-ttu-id="f6f44-261">4</span><span class="sxs-lookup"><span data-stu-id="f6f44-261">4</span></span>      | <span data-ttu-id="f6f44-262">Console</span><span class="sxs-lookup"><span data-stu-id="f6f44-262">Console</span></span>       | <span data-ttu-id="f6f44-263">Microsoft.AspNetCore.Mvc.Razor</span><span class="sxs-lookup"><span data-stu-id="f6f44-263">Microsoft.AspNetCore.Mvc.Razor</span></span>          | <span data-ttu-id="f6f44-264">Erro</span><span class="sxs-lookup"><span data-stu-id="f6f44-264">Error</span></span>             |
| <span data-ttu-id="f6f44-265">5</span><span class="sxs-lookup"><span data-stu-id="f6f44-265">5</span></span>      | <span data-ttu-id="f6f44-266">Console</span><span class="sxs-lookup"><span data-stu-id="f6f44-266">Console</span></span>       | <span data-ttu-id="f6f44-267">Todas as categorias</span><span class="sxs-lookup"><span data-stu-id="f6f44-267">All categories</span></span>                          | <span data-ttu-id="f6f44-268">Informações</span><span class="sxs-lookup"><span data-stu-id="f6f44-268">Information</span></span>       |
| <span data-ttu-id="f6f44-269">6</span><span class="sxs-lookup"><span data-stu-id="f6f44-269">6</span></span>      | <span data-ttu-id="f6f44-270">Todos os provedores</span><span class="sxs-lookup"><span data-stu-id="f6f44-270">All providers</span></span> | <span data-ttu-id="f6f44-271">Todas as categorias</span><span class="sxs-lookup"><span data-stu-id="f6f44-271">All categories</span></span>                          | <span data-ttu-id="f6f44-272">Depurar</span><span class="sxs-lookup"><span data-stu-id="f6f44-272">Debug</span></span>             |
| <span data-ttu-id="f6f44-273">7</span><span class="sxs-lookup"><span data-stu-id="f6f44-273">7</span></span>      | <span data-ttu-id="f6f44-274">Todos os provedores</span><span class="sxs-lookup"><span data-stu-id="f6f44-274">All providers</span></span> | <span data-ttu-id="f6f44-275">Sistema</span><span class="sxs-lookup"><span data-stu-id="f6f44-275">System</span></span>                                  | <span data-ttu-id="f6f44-276">Depurar</span><span class="sxs-lookup"><span data-stu-id="f6f44-276">Debug</span></span>             |
| <span data-ttu-id="f6f44-277">8</span><span class="sxs-lookup"><span data-stu-id="f6f44-277">8</span></span>      | <span data-ttu-id="f6f44-278">Depurar</span><span class="sxs-lookup"><span data-stu-id="f6f44-278">Debug</span></span>         | <span data-ttu-id="f6f44-279">Microsoft</span><span class="sxs-lookup"><span data-stu-id="f6f44-279">Microsoft</span></span>                               | <span data-ttu-id="f6f44-280">Rastrear</span><span class="sxs-lookup"><span data-stu-id="f6f44-280">Trace</span></span>             |

<span data-ttu-id="f6f44-281">Quando você cria um objeto `ILogger` para usar para gravar logs, o objeto `ILoggerFactory` seleciona uma única regra por provedor para aplicar a esse agente.</span><span class="sxs-lookup"><span data-stu-id="f6f44-281">When you create an `ILogger` object to write logs with, the `ILoggerFactory` object selects a single rule per provider to apply to that logger.</span></span> <span data-ttu-id="f6f44-282">Todas as mensagens gravadas pelo objeto `ILogger` são filtradas com base nas regras selecionadas.</span><span class="sxs-lookup"><span data-stu-id="f6f44-282">All messages written by that `ILogger` object are filtered based on the selected rules.</span></span> <span data-ttu-id="f6f44-283">A regra mais específica possível para cada par de categoria e provedor é selecionada dentre as regras disponíveis.</span><span class="sxs-lookup"><span data-stu-id="f6f44-283">The most specific rule possible for each provider and category pair is selected from the available rules.</span></span>

<span data-ttu-id="f6f44-284">O algoritmo a seguir é usado para cada provedor quando um `ILogger` é criado para uma determinada categoria:</span><span class="sxs-lookup"><span data-stu-id="f6f44-284">The following algorithm is used for each provider when an `ILogger` is created for a given category:</span></span>

* <span data-ttu-id="f6f44-285">Selecione todas as regras que correspondem ao provedor ou seu alias.</span><span class="sxs-lookup"><span data-stu-id="f6f44-285">Select all rules that match the provider or its alias.</span></span> <span data-ttu-id="f6f44-286">Se nenhuma for encontrada, selecione todas as regras com um provedor vazio.</span><span class="sxs-lookup"><span data-stu-id="f6f44-286">If none are found, select all rules with an empty provider.</span></span>
* <span data-ttu-id="f6f44-287">Do resultado da etapa anterior, selecione as regras com o prefixo de categoria de maior correspondência.</span><span class="sxs-lookup"><span data-stu-id="f6f44-287">From the result of the preceding step, select rules with longest matching category prefix.</span></span> <span data-ttu-id="f6f44-288">Se nenhuma for encontrada, selecione todas as regras que não especificam uma categoria.</span><span class="sxs-lookup"><span data-stu-id="f6f44-288">If none are found, select all rules that don't specify a category.</span></span>
* <span data-ttu-id="f6f44-289">Se várias regras forem selecionadas use a **última**.</span><span class="sxs-lookup"><span data-stu-id="f6f44-289">If multiple rules are selected take the **last** one.</span></span>
* <span data-ttu-id="f6f44-290">Se nenhuma regra for selecionada, use `MinimumLevel`.</span><span class="sxs-lookup"><span data-stu-id="f6f44-290">If no rules are selected, use `MinimumLevel`.</span></span>
 
<span data-ttu-id="f6f44-291">Por exemplo, suponha que você tem a lista anterior de regras e cria um objeto `ILogger` para a categoria "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":</span><span class="sxs-lookup"><span data-stu-id="f6f44-291">For example, suppose you have the preceding list of rules and you create an `ILogger` object for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":</span></span>

* <span data-ttu-id="f6f44-292">Para o provedor Depuração as regras 1, 6 e 8 se aplicam.</span><span class="sxs-lookup"><span data-stu-id="f6f44-292">For the Debug provider, rules 1, 6, and 8 apply.</span></span> <span data-ttu-id="f6f44-293">A regra 8 é mais específica, portanto é a que será selecionada.</span><span class="sxs-lookup"><span data-stu-id="f6f44-293">Rule 8 is most specific, so that's the one selected.</span></span>
* <span data-ttu-id="f6f44-294">Para o provedor Console as regras 3, 4, 5 e 6 se aplicam.</span><span class="sxs-lookup"><span data-stu-id="f6f44-294">For the Console provider, rules 3, 4, 5, and 6 apply.</span></span> <span data-ttu-id="f6f44-295">A regra 3 é a mais específica.</span><span class="sxs-lookup"><span data-stu-id="f6f44-295">Rule 3 is most specific.</span></span>

<span data-ttu-id="f6f44-296">Quando você cria logs com um `ILogger` para a categoria "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine", os logs de nível `Trace` e superiores vão para o provedor Depuração e os logs de nível `Debug` e superiores vão para o provedor Console.</span><span class="sxs-lookup"><span data-stu-id="f6f44-296">When you create logs with an `ILogger` for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine", logs of `Trace` level and above will go to the Debug provider, and logs of `Debug` level and above will go to the Console provider.</span></span>

<span data-ttu-id="f6f44-297">**Aliases de provedor**</span><span class="sxs-lookup"><span data-stu-id="f6f44-297">**Provider aliases**</span></span>

<span data-ttu-id="f6f44-298">Você pode usar o nome do tipo para especificar um provedor na configuração, mas cada provedor define um *alias* menor que é mais fácil de usar.</span><span class="sxs-lookup"><span data-stu-id="f6f44-298">You can use the type name to specify a provider in configuration, but each provider defines a shorter *alias* that's easier to use.</span></span> <span data-ttu-id="f6f44-299">Para os provedores internos, use os seguintes aliases:</span><span class="sxs-lookup"><span data-stu-id="f6f44-299">For the built-in providers, use the following aliases:</span></span>

- <span data-ttu-id="f6f44-300">Console</span><span class="sxs-lookup"><span data-stu-id="f6f44-300">Console</span></span>
- <span data-ttu-id="f6f44-301">Depurar</span><span class="sxs-lookup"><span data-stu-id="f6f44-301">Debug</span></span>
- <span data-ttu-id="f6f44-302">EventLog</span><span class="sxs-lookup"><span data-stu-id="f6f44-302">EventLog</span></span>
- <span data-ttu-id="f6f44-303">AzureAppServices</span><span class="sxs-lookup"><span data-stu-id="f6f44-303">AzureAppServices</span></span>
- <span data-ttu-id="f6f44-304">TraceSource</span><span class="sxs-lookup"><span data-stu-id="f6f44-304">TraceSource</span></span>
- <span data-ttu-id="f6f44-305">EventSource</span><span class="sxs-lookup"><span data-stu-id="f6f44-305">EventSource</span></span>

<span data-ttu-id="f6f44-306">**Nível mínimo padrão**</span><span class="sxs-lookup"><span data-stu-id="f6f44-306">**Default minimum level**</span></span>

<span data-ttu-id="f6f44-307">Há uma configuração de nível mínimo que entra em vigor somente se nenhuma regra de código ou de configuração se aplicar a um provedor e uma categoria determinados.</span><span class="sxs-lookup"><span data-stu-id="f6f44-307">There's a minimum level setting that takes effect only if no rules from configuration or code apply for a given provider and category.</span></span> <span data-ttu-id="f6f44-308">O exemplo a seguir mostra como definir o nível mínimo:</span><span class="sxs-lookup"><span data-stu-id="f6f44-308">The following example shows how to set the minimum level:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_MinLevel&highlight=3)]

<span data-ttu-id="f6f44-309">Se você não definir explicitamente o nível mínimo, o valor padrão será `Information`, o que significa que logs `Trace` e `Debug` serão ignorados.</span><span class="sxs-lookup"><span data-stu-id="f6f44-309">If you don't explicitly set the minimum level, the default value is `Information`, which means that `Trace` and `Debug` logs are ignored.</span></span>

<span data-ttu-id="f6f44-310">**Funções de filtro**</span><span class="sxs-lookup"><span data-stu-id="f6f44-310">**Filter functions**</span></span>

<span data-ttu-id="f6f44-311">Você pode escrever código em uma função de filtro para aplicar regras de filtragem.</span><span class="sxs-lookup"><span data-stu-id="f6f44-311">You can write code in a filter function to apply filtering rules.</span></span> <span data-ttu-id="f6f44-312">Uma função de filtro é invocada para todos os provedores e categorias que não têm regras atribuídas a eles por configuração ou código.</span><span class="sxs-lookup"><span data-stu-id="f6f44-312">A filter function is invoked for all providers and categories that don't have rules assigned to them by configuration or code.</span></span> <span data-ttu-id="f6f44-313">O código na função tem acesso ao tipo de provedor, à categoria e ao nível de log para decidir se uma mensagem deve ser registrada.</span><span class="sxs-lookup"><span data-stu-id="f6f44-313">Code in the function has access to the provider type, category, and log level to decide whether or not a message should be logged.</span></span> <span data-ttu-id="f6f44-314">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="f6f44-314">For example:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f6f44-315">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f6f44-315">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="f6f44-316">Alguns provedores de log permitem especificar quando os logs devem ser gravados em uma mídia de armazenamento ou ignorados, com base no nível de log e na categoria.</span><span class="sxs-lookup"><span data-stu-id="f6f44-316">Some logging providers let you specify when logs should be written to a storage medium or ignored based on log level and category.</span></span>

<span data-ttu-id="f6f44-317">Os métodos de extensão `AddConsole` e `AddDebug` fornecem sobrecargas que permitem que você passe critérios de filtragem.</span><span class="sxs-lookup"><span data-stu-id="f6f44-317">The `AddConsole` and `AddDebug` extension methods provide overloads that let you pass in filtering criteria.</span></span> <span data-ttu-id="f6f44-318">O código de exemplo a seguir faz com que o provedor de console ignore os logs abaixo do nível `Warning`, enquanto o provedor Depuração ignora os logs criados pela estrutura.</span><span class="sxs-lookup"><span data-stu-id="f6f44-318">The following sample code causes the console provider to ignore logs below `Warning` level, while the Debug provider ignores logs that the framework creates.</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_AddConsoleAndDebugWithFilter&highlight=6-7)]

<span data-ttu-id="f6f44-319">O método `AddEventLog` tem uma sobrecarga que recebe uma instância `EventLogSettings`, que pode conter uma função de filtragem em sua propriedade `Filter`.</span><span class="sxs-lookup"><span data-stu-id="f6f44-319">The `AddEventLog` method has an overload that takes an `EventLogSettings` instance, which may contain a filtering function in its `Filter` property.</span></span> <span data-ttu-id="f6f44-320">O provedor TraceSource não fornece nenhuma dessas sobrecargas, porque seu nível de registro em log e outros parâmetros são baseados no `SourceSwitch` e no `TraceListener` que ele usa.</span><span class="sxs-lookup"><span data-stu-id="f6f44-320">The TraceSource provider doesn't provide any of those overloads, since its logging level and other parameters are based on the `SourceSwitch` and `TraceListener` it uses.</span></span>

<span data-ttu-id="f6f44-321">Você pode definir regras de filtragem para todos os provedores registrados com uma instância `ILoggerFactory` usando o método de extensão `WithFilter`.</span><span class="sxs-lookup"><span data-stu-id="f6f44-321">You can set filtering rules for all providers that are registered with an `ILoggerFactory` instance by using the `WithFilter` extension method.</span></span> <span data-ttu-id="f6f44-322">O exemplo abaixo limita os logs de estrutura (categoria começa com "Microsoft" ou "Sistema") a avisos, permitindo que aplicativo registre no nível de depuração.</span><span class="sxs-lookup"><span data-stu-id="f6f44-322">The example below limits framework logs (category begins with "Microsoft" or "System") to warnings while letting the app log at debug level.</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_FactoryFilter&highlight=6-11)]

<span data-ttu-id="f6f44-323">Se quiser usar a filtragem para impedir que todos os logs de uma determinada categoria sejam gravados, você poderá especificar `LogLevel.None` como o nível de log mínimo para essa categoria.</span><span class="sxs-lookup"><span data-stu-id="f6f44-323">If you want to use filtering to prevent all logs from being written for a particular category, you can specify `LogLevel.None` as the minimum log level for that category.</span></span> <span data-ttu-id="f6f44-324">O valor inteiro de `LogLevel.None` é 6, que é maior do que `LogLevel.Critical` (5).</span><span class="sxs-lookup"><span data-stu-id="f6f44-324">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

<span data-ttu-id="f6f44-325">O método de extensão `WithFilter` é fornecido pelo pacote NuGet [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter).</span><span class="sxs-lookup"><span data-stu-id="f6f44-325">The `WithFilter` extension method is provided by the [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) NuGet package.</span></span> <span data-ttu-id="f6f44-326">O método retorna um nova instância `ILoggerFactory`, que filtrará as mensagens de log passadas para todos os provedores de agente registrados com ela.</span><span class="sxs-lookup"><span data-stu-id="f6f44-326">The method returns a new `ILoggerFactory` instance that will filter the log messages passed to all logger providers registered with it.</span></span> <span data-ttu-id="f6f44-327">Isso não afeta nenhuma outra instância de `ILoggerFactory`, incluindo a instância de `ILoggerFactory` original.</span><span class="sxs-lookup"><span data-stu-id="f6f44-327">It doesn't affect any other `ILoggerFactory` instances, including the original `ILoggerFactory` instance.</span></span>

---

## <a name="log-scopes"></a><span data-ttu-id="f6f44-328">Escopos de log</span><span class="sxs-lookup"><span data-stu-id="f6f44-328">Log scopes</span></span>

<span data-ttu-id="f6f44-329">Você pode agrupar um conjunto de operações lógicas dentro de um *escopo* para anexar os mesmos dados a cada log que é criado como parte desse conjunto.</span><span class="sxs-lookup"><span data-stu-id="f6f44-329">You can group a set of logical operations within a *scope* in order to attach the same data to each log that's created as part of that set.</span></span> <span data-ttu-id="f6f44-330">Por exemplo, é conveniente que cada log criado como parte do processamento de uma transação inclua a ID da transação.</span><span class="sxs-lookup"><span data-stu-id="f6f44-330">For example, you might want every log created as part of processing a transaction to include the transaction ID.</span></span>

<span data-ttu-id="f6f44-331">Um escopo é um tipo `IDisposable` retornado pelo método `ILogger.BeginScope<TState>` e que dura até que seja descartado.</span><span class="sxs-lookup"><span data-stu-id="f6f44-331">A scope is an `IDisposable` type that's returned by the `ILogger.BeginScope<TState>` method and lasts until it's disposed.</span></span> <span data-ttu-id="f6f44-332">Um escopo é usado pelo encapsulamento das chama de agente em um bloco `using`, conforme mostrado aqui:</span><span class="sxs-lookup"><span data-stu-id="f6f44-332">You use a scope by wrapping your logger calls in a `using` block, as shown here:</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

<span data-ttu-id="f6f44-333">O código a seguir habilita os escopos para o provedor de console:</span><span class="sxs-lookup"><span data-stu-id="f6f44-333">The following code enables scopes for the console provider:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f6f44-334">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f6f44-334">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="f6f44-335">Em *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="f6f44-335">In *Program.cs*:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> <span data-ttu-id="f6f44-336">A configuração da opção de agente de console `IncludeScopes` é necessária para habilitar o registro em log baseado em escopo.</span><span class="sxs-lookup"><span data-stu-id="f6f44-336">Configuring the `IncludeScopes` console logger option is required to enable scope-based logging.</span></span> <span data-ttu-id="f6f44-337">A configuração de `IncludeScopes` usando arquivos de configuração *appsettings* estará disponível com o lançamento do ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="f6f44-337">Configuration of `IncludeScopes` using *appsettings* configuration files will be available with the release of ASP.NET Core 2.1.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f6f44-338">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f6f44-338">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="f6f44-339">Em *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="f6f44-339">In *Startup.cs*:</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_Scopes&highlight=6)]

---

<span data-ttu-id="f6f44-340">Cada mensagem de log inclui as informações com escopo definido:</span><span class="sxs-lookup"><span data-stu-id="f6f44-340">Each log message includes the scoped information:</span></span>

```
info: TodoApi.Controllers.TodoController[1002]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      GetById(0) NOT FOUND
```

## <a name="built-in-logging-providers"></a><span data-ttu-id="f6f44-341">Provedores de log internos</span><span class="sxs-lookup"><span data-stu-id="f6f44-341">Built-in logging providers</span></span>

<span data-ttu-id="f6f44-342">O ASP.NET Core vem com os seguintes provedores:</span><span class="sxs-lookup"><span data-stu-id="f6f44-342">ASP.NET Core ships the following providers:</span></span>

* [<span data-ttu-id="f6f44-343">Console</span><span class="sxs-lookup"><span data-stu-id="f6f44-343">Console</span></span>](#console)
* [<span data-ttu-id="f6f44-344">Depurar</span><span class="sxs-lookup"><span data-stu-id="f6f44-344">Debug</span></span>](#debug)
* [<span data-ttu-id="f6f44-345">EventSource</span><span class="sxs-lookup"><span data-stu-id="f6f44-345">EventSource</span></span>](#eventsource)
* [<span data-ttu-id="f6f44-346">EventLog</span><span class="sxs-lookup"><span data-stu-id="f6f44-346">EventLog</span></span>](#eventlog)
* [<span data-ttu-id="f6f44-347">TraceSource</span><span class="sxs-lookup"><span data-stu-id="f6f44-347">TraceSource</span></span>](#tracesource)
* [<span data-ttu-id="f6f44-348">Serviço de Aplicativo do Azure</span><span class="sxs-lookup"><span data-stu-id="f6f44-348">Azure App Service</span></span>](#appservice)

<a id="console"></a>
### <a name="the-console-provider"></a><span data-ttu-id="f6f44-349">O provedor de console</span><span class="sxs-lookup"><span data-stu-id="f6f44-349">The console provider</span></span>

<span data-ttu-id="f6f44-350">O pacote de provedor [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) envia a saída de log para o console.</span><span class="sxs-lookup"><span data-stu-id="f6f44-350">The [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) provider package sends log output to the console.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f6f44-351">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f6f44-351">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddConsole()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f6f44-352">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f6f44-352">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddConsole()
```

<span data-ttu-id="f6f44-353">As [sobrecargas do AddConsole](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.consoleloggerextensions) permitem que você passe um nível de log mínimo, uma função de filtro e um valor booliano que indica se escopos são compatíveis.</span><span class="sxs-lookup"><span data-stu-id="f6f44-353">[AddConsole overloads](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.consoleloggerextensions) let you pass in an a minimum log level, a filter function, and a boolean that indicates whether scopes are supported.</span></span> <span data-ttu-id="f6f44-354">Outra opção é passar um objeto `IConfiguration`, que pode especificar suporte de escopos e níveis de log.</span><span class="sxs-lookup"><span data-stu-id="f6f44-354">Another option is to pass in an `IConfiguration` object, which can specify scopes support and logging levels.</span></span> 

<span data-ttu-id="f6f44-355">Se você estiver considerando o provedor de console para uso em produção, lembre-se de que ele tem um impacto significativo no desempenho.</span><span class="sxs-lookup"><span data-stu-id="f6f44-355">If you are considering the console provider for use in production, be aware that it has a significant impact on performance.</span></span>

<span data-ttu-id="f6f44-356">Quando você cria um novo projeto no Visual Studio, o método `AddConsole` tem essa aparência:</span><span class="sxs-lookup"><span data-stu-id="f6f44-356">When you create a new project in Visual Studio, the `AddConsole` method looks like this:</span></span>

```csharp
loggerFactory.AddConsole(Configuration.GetSection("Logging"));
```

<span data-ttu-id="f6f44-357">Esse código se refere à seção `Logging` do arquivo *appSettings.json*:</span><span class="sxs-lookup"><span data-stu-id="f6f44-357">This code refers to the `Logging` section of the *appSettings.json* file:</span></span>

[!code-json[](index/sample//appsettings.json)]

<span data-ttu-id="f6f44-358">As configurações mostradas limitam os logs de estrutura a avisos, permitindo que o aplicativo para faça registros no nível de depuração, conforme explicado na [Filtragem de log](#log-filtering) seção.</span><span class="sxs-lookup"><span data-stu-id="f6f44-358">The settings shown limit framework logs to warnings while allowing the app to log at debug level, as explained in the [Log filtering](#log-filtering) section.</span></span> <span data-ttu-id="f6f44-359">Para obter mais informações, consulte [Configuração](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="f6f44-359">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

---

<a id="debug"></a>
### <a name="the-debug-provider"></a><span data-ttu-id="f6f44-360">O provedor Depuração</span><span class="sxs-lookup"><span data-stu-id="f6f44-360">The Debug provider</span></span>

<span data-ttu-id="f6f44-361">O pacote de provedor [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) grava a saída de log usando a classe [System.Diagnostics.Debug](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.debug#System_Diagnostics_Debug) (chamadas de método `Debug.WriteLine`).</span><span class="sxs-lookup"><span data-stu-id="f6f44-361">The [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) provider package writes log output by using the [System.Diagnostics.Debug](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.debug#System_Diagnostics_Debug) class (`Debug.WriteLine` method calls).</span></span>

<span data-ttu-id="f6f44-362">No Linux, esse provedor grava logs em */var/log/message*.</span><span class="sxs-lookup"><span data-stu-id="f6f44-362">On Linux, this provider writes logs to */var/log/message*.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f6f44-363">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f6f44-363">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddDebug()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f6f44-364">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f6f44-364">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddDebug()
```

<span data-ttu-id="f6f44-365">As [sobrecargas de AddDebug](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.debugloggerfactoryextensions) permitem que você passe um nível de log mínimo ou uma função de filtro.</span><span class="sxs-lookup"><span data-stu-id="f6f44-365">[AddDebug overloads](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.debugloggerfactoryextensions) let you pass in a minimum log level or a filter function.</span></span>

---

<a id="eventsource"></a>
### <a name="the-eventsource-provider"></a><span data-ttu-id="f6f44-366">O provedor EventSource</span><span class="sxs-lookup"><span data-stu-id="f6f44-366">The EventSource provider</span></span>

<span data-ttu-id="f6f44-367">Para aplicativos que se destinam ao ASP.NET Core 1.1.0 ou superior, o pacote de provedor [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) pode implementar o rastreamento de eventos.</span><span class="sxs-lookup"><span data-stu-id="f6f44-367">For apps that target ASP.NET Core 1.1.0 or higher, the [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) provider package can implement event tracing.</span></span> <span data-ttu-id="f6f44-368">No Windows, ele usa [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span><span class="sxs-lookup"><span data-stu-id="f6f44-368">On Windows, it uses [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span></span> <span data-ttu-id="f6f44-369">O provedor é multiplataforma, mas ainda não há ferramentas de coleta e exibição de eventos para Linux ou macOS.</span><span class="sxs-lookup"><span data-stu-id="f6f44-369">The provider is cross-platform, but there are no event collection and display tools yet for Linux or macOS.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f6f44-370">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f6f44-370">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddEventSourceLogger()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f6f44-371">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f6f44-371">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddEventSourceLogger()
```

---

<span data-ttu-id="f6f44-372">Uma boa maneira de coletar e exibir logs é usar o [utilitário PerfView](https://www.microsoft.com/download/details.aspx?id=28567).</span><span class="sxs-lookup"><span data-stu-id="f6f44-372">A good way to collect and view logs is to use the [PerfView utility](https://www.microsoft.com/download/details.aspx?id=28567).</span></span> <span data-ttu-id="f6f44-373">Há outras ferramentas para exibir os logs do ETW, mas o PerfView proporciona a melhor experiência para trabalhar com os eventos de ETW emitidos pelo ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="f6f44-373">There are other tools for viewing ETW logs, but PerfView provides the best experience for working with the ETW events emitted by ASP.NET.</span></span> 

<span data-ttu-id="f6f44-374">Para configurar o PerfView para coletar eventos registrados por esse provedor, adicione a cadeia de caracteres `*Microsoft-Extensions-Logging` à lista **Provedores Adicionais**.</span><span class="sxs-lookup"><span data-stu-id="f6f44-374">To configure PerfView for collecting events logged by this provider, add the string `*Microsoft-Extensions-Logging` to the **Additional Providers** list.</span></span> <span data-ttu-id="f6f44-375">(Não se esqueça do asterisco no início da cadeia de caracteres).</span><span class="sxs-lookup"><span data-stu-id="f6f44-375">(Don't miss the asterisk at the start of the string.)</span></span>

![Outros provedores de Perfview](index/_static/perfview-additional-providers.png)

<span data-ttu-id="f6f44-377">A captura de eventos no Nano Server demanda algumas configurações adicionais:</span><span class="sxs-lookup"><span data-stu-id="f6f44-377">Capturing events on Nano Server requires some additional setup:</span></span>

* <span data-ttu-id="f6f44-378">Conecte a comunicação remota do PowerShell ao Nano Server:</span><span class="sxs-lookup"><span data-stu-id="f6f44-378">Connect PowerShell remoting to the Nano Server:</span></span>

  ```powershell
  Enter-PSSession [name]
  ```

* <span data-ttu-id="f6f44-379">Crie uma sessão de ETW:</span><span class="sxs-lookup"><span data-stu-id="f6f44-379">Create an ETW session:</span></span>

  ```powershell
  New-EtwTraceSession -Name "MyAppTrace" -LocalFilePath C:\trace.etl
  ```

* <span data-ttu-id="f6f44-380">Adicione provedores de ETW ao [CLR](https://docs.microsoft.com/dotnet/framework/performance/clr-etw-providers), ao ASP.NET Core e a outros, conforme necessário.</span><span class="sxs-lookup"><span data-stu-id="f6f44-380">Add ETW providers for [CLR](https://docs.microsoft.com/dotnet/framework/performance/clr-etw-providers), ASP.NET Core, and others as needed.</span></span> <span data-ttu-id="f6f44-381">O GUID de provedor do ASP.NET Core é `3ac73b97-af73-50e9-0822-5da4367920d0`.</span><span class="sxs-lookup"><span data-stu-id="f6f44-381">The ASP.NET Core provider GUID is `3ac73b97-af73-50e9-0822-5da4367920d0`.</span></span> 

  ```powershell
  Add-EtwTraceProvider -Guid "{e13c0d23-ccbc-4e12-931b-d9cc2eee27e4}" -SessionName MyAppTrace
  Add-EtwTraceProvider -Guid "{3ac73b97-af73-50e9-0822-5da4367920d0}" -SessionName MyAppTrace
  ```

* <span data-ttu-id="f6f44-382">Execute o site e realize as ações para as quais você deseja obter informações de rastreamento.</span><span class="sxs-lookup"><span data-stu-id="f6f44-382">Run the site and do whatever actions you want tracing information for.</span></span>

* <span data-ttu-id="f6f44-383">Interrompa a sessão de rastreamento quando tiver terminado:</span><span class="sxs-lookup"><span data-stu-id="f6f44-383">Stop the tracing session when you're finished:</span></span>

  ```powershell
  Stop-EtwTraceSession -Name "MyAppTrace"
  ```

<span data-ttu-id="f6f44-384">O arquivo resultante *C:\trace.etl* pode ser analisado com PerfView, como em outras edições do Windows.</span><span class="sxs-lookup"><span data-stu-id="f6f44-384">The resulting *C:\trace.etl* file can be analyzed with PerfView as on other editions of Windows.</span></span>

<a id="eventlog"></a>
### <a name="the-windows-eventlog-provider"></a><span data-ttu-id="f6f44-385">O provedor EventLog do Windows</span><span class="sxs-lookup"><span data-stu-id="f6f44-385">The Windows EventLog provider</span></span>

<span data-ttu-id="f6f44-386">O pacote de provedor [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) envia a saída de log para o Log de Eventos do Windows.</span><span class="sxs-lookup"><span data-stu-id="f6f44-386">The [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) provider package sends log output to the Windows Event Log.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f6f44-387">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f6f44-387">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddEventLog()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f6f44-388">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f6f44-388">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddEventLog()
```

<span data-ttu-id="f6f44-389">As [sobrecargas de AddEventLog](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.eventloggerfactoryextensions) permitem que você passe `EventLogSettings` ou um nível de log mínimo.</span><span class="sxs-lookup"><span data-stu-id="f6f44-389">[AddEventLog overloads](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.eventloggerfactoryextensions) let you pass in `EventLogSettings` or a minimum log level.</span></span>

---

<a id="tracesource"></a>
### <a name="the-tracesource-provider"></a><span data-ttu-id="f6f44-390">O provedor TraceSource</span><span class="sxs-lookup"><span data-stu-id="f6f44-390">The TraceSource provider</span></span>

<span data-ttu-id="f6f44-391">O pacote de provedor [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) usa as bibliotecas e provedores de [System.Diagnostics.TraceSource](https://docs.microsoft.com/dotnet/api/system.diagnostics.tracesource).</span><span class="sxs-lookup"><span data-stu-id="f6f44-391">The [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) provider package uses the [System.Diagnostics.TraceSource](https://docs.microsoft.com/dotnet/api/system.diagnostics.tracesource) libraries and providers.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f6f44-392">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f6f44-392">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddTraceSource(sourceSwitchName);
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f6f44-393">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f6f44-393">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddTraceSource(sourceSwitchName);
```

---

<span data-ttu-id="f6f44-394">As [sobrecargas de AddTraceSource](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.tracesourcefactoryextensions) permitem que você passe um comutador de fonte e um ouvinte de rastreamento.</span><span class="sxs-lookup"><span data-stu-id="f6f44-394">[AddTraceSource overloads](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.tracesourcefactoryextensions) let you pass in a source switch and a trace listener.</span></span>

<span data-ttu-id="f6f44-395">Para usar esse provedor, o aplicativo deve ser executado no .NET Framework (em vez do .NET Core).</span><span class="sxs-lookup"><span data-stu-id="f6f44-395">To use this provider, an application has to run on the .NET Framework (rather than .NET Core).</span></span> <span data-ttu-id="f6f44-396">O provedor permite rotear mensagens a uma variedade de [ouvintes](https://docs.microsoft.com/dotnet/framework/debug-trace-profile/trace-listeners), como o [TextWriterTraceListener](https://docs.microsoft.com/dotnet/api/system.diagnostics.textwritertracelistenerr) usado no aplicativo de exemplo.</span><span class="sxs-lookup"><span data-stu-id="f6f44-396">The provider lets you route messages to a variety of [listeners](https://docs.microsoft.com/dotnet/framework/debug-trace-profile/trace-listeners), such as the [TextWriterTraceListener](https://docs.microsoft.com/dotnet/api/system.diagnostics.textwritertracelistenerr) used in the sample application.</span></span>

<span data-ttu-id="f6f44-397">O exemplo a seguir configura um provedor `TraceSource` que registra mensagens `Warning` e superiores na janela de console.</span><span class="sxs-lookup"><span data-stu-id="f6f44-397">The following example configures a `TraceSource` provider that logs `Warning` and higher messages to the console window.</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_TraceSource&highlight=9-12)]

<a id="appservice"></a>
### <a name="the-azure-app-service-provider"></a><span data-ttu-id="f6f44-398">O provedor do Serviço de Aplicativo do Azure</span><span class="sxs-lookup"><span data-stu-id="f6f44-398">The Azure App Service provider</span></span>

<span data-ttu-id="f6f44-399">O pacote de provedor [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) grava logs em arquivos de texto no sistema de arquivos de um aplicativo do Serviço de Aplicativo do Azure e no [armazenamento de blobs](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) em uma conta de Armazenamento do Azure.</span><span class="sxs-lookup"><span data-stu-id="f6f44-399">The [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) provider package writes logs to text files in an Azure App Service app's file system and to [blob storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) in an Azure Storage account.</span></span> <span data-ttu-id="f6f44-400">O provedor está disponível somente para aplicativos que se destinam ao ASP.NET Core 1.1.0 ou superior.</span><span class="sxs-lookup"><span data-stu-id="f6f44-400">The provider is available only for apps that target ASP.NET Core 1.1.0 or higher.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f6f44-401">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f6f44-401">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="f6f44-402">Se o destino for o .NET Core, você não precisará instalar o pacote de provedor ou chamar explicitamente `AddAzureWebAppDiagnostics`.</span><span class="sxs-lookup"><span data-stu-id="f6f44-402">If targeting .NET Core, you don't have to install the provider package or explicitly call `AddAzureWebAppDiagnostics`.</span></span> <span data-ttu-id="f6f44-403">O provedor fica automaticamente disponível para o aplicativo quando você implanta o aplicativo do Serviço de Aplicativo do Azure.</span><span class="sxs-lookup"><span data-stu-id="f6f44-403">The provider is automatically available to your app when you deploy the app to Azure App Service.</span></span>

<span data-ttu-id="f6f44-404">Se o destino for o .NET Framework, adicione o pacote de provedor ao seu projeto e invoque `AddAzureWebAppDiagnostics`:</span><span class="sxs-lookup"><span data-stu-id="f6f44-404">If targeting .NET Framework, add the provider package to your project and invoke `AddAzureWebAppDiagnostics`:</span></span>

```csharp
logging.AddAzureWebAppDiagnostics();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f6f44-405">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f6f44-405">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddAzureWebAppDiagnostics();
```

<span data-ttu-id="f6f44-406">Uma sobrecarga `AddAzureWebAppDiagnostics` permite que você passe [AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs) com a qual você pode substituir as configurações padrão, como o modelo de saída de registro em log, o nome do blob e o limite do tamanho do arquivo.</span><span class="sxs-lookup"><span data-stu-id="f6f44-406">An `AddAzureWebAppDiagnostics` overload lets you pass in [AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs) with which you can override default settings such as the logging output template, blob name, and file size limit.</span></span> <span data-ttu-id="f6f44-407">(O *modelo Output* é um modelo de mensagem que é aplicado a todos os logs, sobre aquele que você fornece ao chamar um método `ILogger`).</span><span class="sxs-lookup"><span data-stu-id="f6f44-407">(*Output template* is a message template that's applied to all logs on top of the one that you provide when you call an `ILogger` method.)</span></span>

---

<span data-ttu-id="f6f44-408">Ao implantar um aplicativo do Serviço de Aplicativo, seu aplicativo respeita as configurações na seção [Logs de Diagnóstico](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag) da página **Serviço de Aplicativo** do Portal do Azure.</span><span class="sxs-lookup"><span data-stu-id="f6f44-408">When you deploy to an App Service app, your application honors the settings in the [Diagnostic Logs](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag) section of the **App Service** page of the Azure portal.</span></span> <span data-ttu-id="f6f44-409">Ao alterar essas configurações, elas entram em vigor imediatamente, sem exigir que você reinicie o aplicativo ou reimplante o código nele.</span><span class="sxs-lookup"><span data-stu-id="f6f44-409">When you change those settings, the changes take effect immediately without requiring that you restart the app or redeploy code to it.</span></span> 

![Configurações de registro em log do Azure](index/_static/azure-logging-settings.png)

<span data-ttu-id="f6f44-411">O local padrão para arquivos de log é na pasta *D:\\home\\LogFiles\\Application* e o nome de arquivo padrão é *diagnostics-aaaammdd.txt*.</span><span class="sxs-lookup"><span data-stu-id="f6f44-411">The default location for log files is in the *D:\\home\\LogFiles\\Application* folder, and the default file name is *diagnostics-yyyymmdd.txt*.</span></span> <span data-ttu-id="f6f44-412">O limite padrão de tamanho do arquivo é 10 MB e o número padrão máximo de arquivos mantidos é 2.</span><span class="sxs-lookup"><span data-stu-id="f6f44-412">The default file size limit is 10 MB, and the default maximum number of files retained is 2.</span></span> <span data-ttu-id="f6f44-413">O nome de blob padrão é *{app-name}{timestamp}/aaaa/mm/dd/hh/{guid}-applicationLog.txt*.</span><span class="sxs-lookup"><span data-stu-id="f6f44-413">The default blob name is *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*.</span></span> <span data-ttu-id="f6f44-414">Para obter mais informações sobre o comportamento padrão, consulte [AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs).</span><span class="sxs-lookup"><span data-stu-id="f6f44-414">For more information about default behavior, see [AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs).</span></span>

<span data-ttu-id="f6f44-415">O provedor funciona somente quando o projeto é executado no ambiente do Azure.</span><span class="sxs-lookup"><span data-stu-id="f6f44-415">The provider only works when your project runs in the Azure environment.</span></span> <span data-ttu-id="f6f44-416">Ele não tem nenhum efeito quando é executado localmente &mdash; ele não grava em arquivos locais ou no armazenamento de desenvolvimento local para blobs.</span><span class="sxs-lookup"><span data-stu-id="f6f44-416">It has no effect when you run locally &mdash; it doesn't write to local files or local development storage for blobs.</span></span>

## <a name="third-party-logging-providers"></a><span data-ttu-id="f6f44-417">Provedores de log de terceiros</span><span class="sxs-lookup"><span data-stu-id="f6f44-417">Third-party logging providers</span></span>

<span data-ttu-id="f6f44-418">Aqui estão algumas estruturas de registros de terceiros que funcionam com o ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="f6f44-418">Here are some third-party logging frameworks that work with ASP.NET Core:</span></span>

* <span data-ttu-id="f6f44-419">[elmah.io](https://github.com/elmahio/Elmah.Io.Extensions.Logging) – provedor para o serviço Elmah.Io</span><span class="sxs-lookup"><span data-stu-id="f6f44-419">[elmah.io](https://github.com/elmahio/Elmah.Io.Extensions.Logging) - provider for the Elmah.Io service</span></span>

* <span data-ttu-id="f6f44-420">[JSNLog](http://jsnlog.com) – registra as exceções de JavaScript e outros eventos do lado do cliente no registro do lado do servidor.</span><span class="sxs-lookup"><span data-stu-id="f6f44-420">[JSNLog](http://jsnlog.com) - logs JavaScript exceptions and other client-side events in your server-side log.</span></span>

* <span data-ttu-id="f6f44-421">[Loggr](https://github.com/imobile3/Loggr.Extensions.Logging) – provedor para o serviço Loggr</span><span class="sxs-lookup"><span data-stu-id="f6f44-421">[Loggr](https://github.com/imobile3/Loggr.Extensions.Logging) - provider for the Loggr service</span></span>

* <span data-ttu-id="f6f44-422">[NLog](https://github.com/NLog/NLog.Extensions.Logging) – provedor para a biblioteca NLog</span><span class="sxs-lookup"><span data-stu-id="f6f44-422">[NLog](https://github.com/NLog/NLog.Extensions.Logging) - provider for the NLog library</span></span>

* <span data-ttu-id="f6f44-423">[Serilog](https://github.com/serilog/serilog-extensions-logging) – provedor para a biblioteca Serilog</span><span class="sxs-lookup"><span data-stu-id="f6f44-423">[Serilog](https://github.com/serilog/serilog-extensions-logging) - provider for the Serilog library</span></span>

<span data-ttu-id="f6f44-424">Algumas estruturas de terceiros podem fazer o [log semântico, também conhecido como registro em log estruturado](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="f6f44-424">Some third-party frameworks can do [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="f6f44-425">O uso de uma estrutura de terceiros é semelhante ao uso de um dos provedores internos: adicione um pacote NuGet ao seu projeto e chame um método de extensão em `ILoggerFactory`.</span><span class="sxs-lookup"><span data-stu-id="f6f44-425">Using a third-party framework is similar to using one of the built-in providers: add a NuGet package to your project and call an extension method on `ILoggerFactory`.</span></span> <span data-ttu-id="f6f44-426">Para obter mais informações, consulte a documentação de cada estrutura.</span><span class="sxs-lookup"><span data-stu-id="f6f44-426">For more information, see each framework's documentation.</span></span>

<span data-ttu-id="f6f44-427">Você também pode criar seus próprios provedores personalizados para dar suporte a outras estruturas de registros ou a seus próprios requisitos de registro em log.</span><span class="sxs-lookup"><span data-stu-id="f6f44-427">You can create your own custom providers as well, to support other logging frameworks or your own logging requirements.</span></span>

## <a name="azure-log-streaming"></a><span data-ttu-id="f6f44-428">Fluxo de log do Azure</span><span class="sxs-lookup"><span data-stu-id="f6f44-428">Azure log streaming</span></span>

<span data-ttu-id="f6f44-429">O fluxo de log do Azure permite que você exiba a atividade de log em tempo real:</span><span class="sxs-lookup"><span data-stu-id="f6f44-429">Azure log streaming enables you to view log activity in real time from:</span></span> 

* <span data-ttu-id="f6f44-430">Do servidor de aplicativos</span><span class="sxs-lookup"><span data-stu-id="f6f44-430">The application server</span></span> 
* <span data-ttu-id="f6f44-431">Do servidor Web</span><span class="sxs-lookup"><span data-stu-id="f6f44-431">The web server</span></span>
* <span data-ttu-id="f6f44-432">De uma solicitação de rastreio com falha</span><span class="sxs-lookup"><span data-stu-id="f6f44-432">Failed request tracing</span></span> 

<span data-ttu-id="f6f44-433">Para configurar o fluxo de log do Azure:</span><span class="sxs-lookup"><span data-stu-id="f6f44-433">To configure Azure log streaming:</span></span> 

* <span data-ttu-id="f6f44-434">Navegue até a página **Logs de Diagnóstico** da página do portal do seu aplicativo</span><span class="sxs-lookup"><span data-stu-id="f6f44-434">Navigate to the **Diagnostics Logs** page from your application's portal page</span></span>
* <span data-ttu-id="f6f44-435">Defina o **Log de aplicativo (Sistema de Arquivos)** como ativado.</span><span class="sxs-lookup"><span data-stu-id="f6f44-435">Set **Application Logging (Filesystem)** to on.</span></span> 

![Página de logs de diagnóstico do Portal do Azure](index/_static/azure-diagnostic-logs.png)

<span data-ttu-id="f6f44-437">Navegue até a página **Fluxo de Log** para exibir as mensagens de aplicativo.</span><span class="sxs-lookup"><span data-stu-id="f6f44-437">Navigate to the **Log Streaming** page to view application messages.</span></span> <span data-ttu-id="f6f44-438">Elas são registradas pelo aplicativo por meio da interface `ILogger`.</span><span class="sxs-lookup"><span data-stu-id="f6f44-438">They're logged by application through the `ILogger` interface.</span></span> 

![Fluxo de log do aplicativo do Portal do Azure](index/_static/azure-log-streaming.png)


## <a name="see-also"></a><span data-ttu-id="f6f44-440">Consulte também</span><span class="sxs-lookup"><span data-stu-id="f6f44-440">See also</span></span>

[<span data-ttu-id="f6f44-441">Registro em log de alto desempenho com LoggerMessage</span><span class="sxs-lookup"><span data-stu-id="f6f44-441">High-performance logging with LoggerMessage</span></span>](xref:fundamentals/logging/loggermessage)

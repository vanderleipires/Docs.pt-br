---
title: Registro em log no ASP.NET Core
author: ardalis
description: Saiba mais sobre a estrutura de registros no ASP.NET Core. Descubra os provedores de log internos e saiba mais sobre os provedores de terceiros populares.
ms.author: tdykstra
ms.date: 12/15/2017
uid: fundamentals/logging/index
ms.openlocfilehash: 969ad303c3fee06aa40d43140153ffbf58b735db
ms.sourcegitcommit: 2941e24d7f3fd3d5e88d27e5f852aaedd564deda
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37126281"
---
# <a name="logging-in-aspnet-core"></a><span data-ttu-id="40e64-104">Registro em log no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="40e64-104">Logging in ASP.NET Core</span></span>

<span data-ttu-id="40e64-105">Por [Steve Smith](https://ardalis.com/) e [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="40e64-105">By [Steve Smith](https://ardalis.com/) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="40e64-106">O ASP.NET Core dá suporte a uma API de registro em log que funciona com uma variedade de provedores de logs.</span><span class="sxs-lookup"><span data-stu-id="40e64-106">ASP.NET Core supports a logging API that works with a variety of logging providers.</span></span> <span data-ttu-id="40e64-107">Os provedores internos permitem que você envie logs para um ou mais destinos e você pode se conectar a uma estrutura de registros de terceiros.</span><span class="sxs-lookup"><span data-stu-id="40e64-107">Built-in providers let you send logs to one or more destinations, and you can plug in a third-party logging framework.</span></span> <span data-ttu-id="40e64-108">Este artigo mostra como usar a API de registro em log e os provedores internos em seu código.</span><span class="sxs-lookup"><span data-stu-id="40e64-108">This article shows how to use the built-in logging API and providers in your code.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="40e64-109">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample2) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="40e64-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample2) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="40e64-110">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="40e64-110">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

::: moniker-end

## <a name="how-to-create-logs"></a><span data-ttu-id="40e64-111">Como criar logs</span><span class="sxs-lookup"><span data-stu-id="40e64-111">How to create logs</span></span>

<span data-ttu-id="40e64-112">Para criar logs, implemente um objeto [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) do contêiner de [injeção de dependência](xref:fundamentals/dependency-injection):</span><span class="sxs-lookup"><span data-stu-id="40e64-112">To create logs, implement an [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) object from the [dependency injection](xref:fundamentals/dependency-injection) container:</span></span>

[!code-csharp[](index/sample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

<span data-ttu-id="40e64-113">Em seguida, chame métodos de registro em log nesse objeto de agente:</span><span class="sxs-lookup"><span data-stu-id="40e64-113">Then call logging methods on that logger object:</span></span>

[!code-csharp[](index/sample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="40e64-114">Este exemplo cria logs com a classe `TodoController` como a *categoria*.</span><span class="sxs-lookup"><span data-stu-id="40e64-114">This example creates logs with the `TodoController` class as the *category*.</span></span> <span data-ttu-id="40e64-115">As categorias serão explicadas [posteriormente neste artigo](#log-category).</span><span class="sxs-lookup"><span data-stu-id="40e64-115">Categories are explained [later in this article](#log-category).</span></span>

<span data-ttu-id="40e64-116">O ASP.NET Core não fornece métodos de agente assíncronos porque o registro em log deve ser tão rápido que não valeria a pena usar assíncronos.</span><span class="sxs-lookup"><span data-stu-id="40e64-116">ASP.NET Core doesn't provide async logger methods because logging should be so fast that it isn't worth the cost of using async.</span></span> <span data-ttu-id="40e64-117">Se você estiver em uma situação em que isso não é verdadeiro, considere alterar a maneira como você faz logs.</span><span class="sxs-lookup"><span data-stu-id="40e64-117">If you're in a situation where that's not true, consider changing the way you log.</span></span> <span data-ttu-id="40e64-118">Se seu armazenamento de dados é lento, grave as mensagens de log em um repositório rápido primeiro e, depois, mova-as para um repositório lento.</span><span class="sxs-lookup"><span data-stu-id="40e64-118">If your data store is slow, write the log messages to a fast store first, then move them to a slow store later.</span></span> <span data-ttu-id="40e64-119">Por exemplo, registre em uma fila de mensagens que seja lida e persistida em um armazenamento lento por outro processo.</span><span class="sxs-lookup"><span data-stu-id="40e64-119">For example, log to a message queue that's read and persisted to slow storage by another process.</span></span>

## <a name="how-to-add-providers"></a><span data-ttu-id="40e64-120">Como adicionar provedores</span><span class="sxs-lookup"><span data-stu-id="40e64-120">How to add providers</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="40e64-121">Um provedor de logs obtém as mensagens que você cria com um objeto `ILogger` e as exibe ou armazena.</span><span class="sxs-lookup"><span data-stu-id="40e64-121">A logging provider takes the messages that you create with an `ILogger` object and displays or stores them.</span></span> <span data-ttu-id="40e64-122">Por exemplo, o provedor Console exibe as mensagens no console e o provedor do Serviço de Aplicativo do Azure pode armazená-las no armazenamento de blobs do Azure.</span><span class="sxs-lookup"><span data-stu-id="40e64-122">For example, the Console provider displays messages on the console, and the Azure App Service provider can store them in Azure blob storage.</span></span>

<span data-ttu-id="40e64-123">Para usar um provedor, chame o método de extensão `Add<ProviderName>` do provedor no *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="40e64-123">To use a provider, call the provider's `Add<ProviderName>` extension method in *Program.cs*:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_ExpandDefault&highlight=16,17)]

<span data-ttu-id="40e64-124">O modelo de projeto padrão permite o registro em log com o método [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder?view=aspnetcore-2.0#Microsoft_AspNetCore_WebHost_CreateDefaultBuilder_System_String___):</span><span class="sxs-lookup"><span data-stu-id="40e64-124">The default project template enables logging with the [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder?view=aspnetcore-2.0#Microsoft_AspNetCore_WebHost_CreateDefaultBuilder_System_String___) method:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_TemplateCode&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="40e64-125">Um provedor de logs obtém as mensagens que você cria com um objeto `ILogger` e as exibe ou armazena.</span><span class="sxs-lookup"><span data-stu-id="40e64-125">A logging provider takes the messages that you create with an `ILogger` object and displays or stores them.</span></span> <span data-ttu-id="40e64-126">Por exemplo, o provedor Console exibe as mensagens no console e o provedor do Serviço de Aplicativo do Azure pode armazená-las no armazenamento de blobs do Azure.</span><span class="sxs-lookup"><span data-stu-id="40e64-126">For example, the Console provider displays messages on the console, and the Azure App Service provider can store them in Azure blob storage.</span></span>

<span data-ttu-id="40e64-127">Para usar um provedor, instale o pacote NuGet e chame o método de extensão do provedor em uma instância do [ILoggerFactory](/dotnet/api/microsoft.extensions.logging.iloggerfactory), conforme mostrado no exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="40e64-127">To use a provider, install its NuGet package and call the provider's extension method on an instance of [ILoggerFactory](/dotnet/api/microsoft.extensions.logging.iloggerfactory), as shown in the following example:</span></span>

[!code-csharp[](index/sample//Startup.cs?name=snippet_AddConsoleAndDebug&highlight=3,5-7)]

<span data-ttu-id="40e64-128">A [DI](xref:fundamentals/dependency-injection) (injeção de dependência) do ASP.NET Core fornece a instância de `ILoggerFactory`.</span><span class="sxs-lookup"><span data-stu-id="40e64-128">ASP.NET Core [dependency injection](xref:fundamentals/dependency-injection) (DI) provides the `ILoggerFactory` instance.</span></span> <span data-ttu-id="40e64-129">Os métodos de extensão `AddConsole` e `AddDebug` são definidos nos pacotes [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) e [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/).</span><span class="sxs-lookup"><span data-stu-id="40e64-129">The `AddConsole` and `AddDebug` extension methods are defined in the [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) and [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/) packages.</span></span> <span data-ttu-id="40e64-130">Cada método de extensão chama o método `ILoggerFactory.AddProvider`, passando uma instância do provedor.</span><span class="sxs-lookup"><span data-stu-id="40e64-130">Each extension method calls the `ILoggerFactory.AddProvider` method, passing in an instance of the provider.</span></span> 

> [!NOTE]
> <span data-ttu-id="40e64-131">O [aplicativo de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample) adiciona provedores de log no método `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="40e64-131">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample) adds logging providers in the `Startup.Configure` method.</span></span> <span data-ttu-id="40e64-132">Se você quiser obter saída de log do código que é executado anteriormente, adicione provedores de log no construtor de classe `Startup`.</span><span class="sxs-lookup"><span data-stu-id="40e64-132">If you want to obtain log output from code that executes earlier, add logging providers in the `Startup` class constructor.</span></span>

::: moniker-end

<span data-ttu-id="40e64-133">Você encontrará informações sobre cada [provedor de log interno](#built-in-logging-providers) e links para [provedores de log de terceiros](#third-party-logging-providers) mais adiante no artigo.</span><span class="sxs-lookup"><span data-stu-id="40e64-133">You'll find information about each [built-in logging provider](#built-in-logging-providers) and links to [third-party logging providers](#third-party-logging-providers) later in the article.</span></span>

## <a name="settings-file-configuration"></a><span data-ttu-id="40e64-134">Definições do arquivo de configurações</span><span class="sxs-lookup"><span data-stu-id="40e64-134">Settings file configuration</span></span>

<span data-ttu-id="40e64-135">Cada um dos exemplos anteriores na seção [Como adicionar provedores](#how-to-add-providers) carrega a configuração do provedor de logs da seção `Logging` dos arquivos de configurações do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="40e64-135">Each of the preceding examples in the [How to add providers](#how-to-add-providers) section loads logging provider configuration from the `Logging` section of app settings files.</span></span> <span data-ttu-id="40e64-136">O exemplo a seguir mostra o conteúdo de um típico arquivo *appsettings.Development.json*:</span><span class="sxs-lookup"><span data-stu-id="40e64-136">The following example shows the contents of a typical *appsettings.Development.json* file:</span></span>

::: moniker range=">= aspnetcore-2.1"

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Debug",
      "System": "Information",
      "Microsoft": "Information"
    },
    "Console":
    {
      "IncludeScopes": "true"
    }
  }
}
```

<span data-ttu-id="40e64-137">Chaves `LogLevel` representam nomes de log.</span><span class="sxs-lookup"><span data-stu-id="40e64-137">`LogLevel` keys represent log names.</span></span> <span data-ttu-id="40e64-138">A chave `Default` aplica-se a logs não listados de forma explícita.</span><span class="sxs-lookup"><span data-stu-id="40e64-138">The `Default` key applies to logs not explicitly listed.</span></span> <span data-ttu-id="40e64-139">O valor representa o [nível de log](#log-level) aplicado ao log fornecido.</span><span class="sxs-lookup"><span data-stu-id="40e64-139">The value represents the [log level](#log-level) applied to the given log.</span></span> <span data-ttu-id="40e64-140">Chaves de log que definem `IncludeScopes` (`Console` no exemplo) especificam se os [escopos de log](#log-scopes) estão habilitados para o log indicado.</span><span class="sxs-lookup"><span data-stu-id="40e64-140">Log keys that set `IncludeScopes` (`Console` in the example), specify if [log scopes](#log-scopes) are enabled for the indicated log.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.1"

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Debug",
      "System": "Information",
      "Microsoft": "Information"
    }
  }
}
```

<span data-ttu-id="40e64-141">Chaves `LogLevel` representam nomes de log.</span><span class="sxs-lookup"><span data-stu-id="40e64-141">`LogLevel` keys represent log names.</span></span> <span data-ttu-id="40e64-142">A chave `Default` aplica-se a logs não listados de forma explícita.</span><span class="sxs-lookup"><span data-stu-id="40e64-142">The `Default` key applies to logs not explicitly listed.</span></span> <span data-ttu-id="40e64-143">O valor representa o [nível de log](#log-level) aplicado ao log fornecido.</span><span class="sxs-lookup"><span data-stu-id="40e64-143">The value represents the [log level](#log-level) applied to the given log.</span></span>

::: moniker-end

## <a name="sample-logging-output"></a><span data-ttu-id="40e64-144">Exemplo de saída de registro em log</span><span class="sxs-lookup"><span data-stu-id="40e64-144">Sample logging output</span></span>

<span data-ttu-id="40e64-145">Com o código de exemplo mostrado na seção anterior, você verá logs no console ao executar através da linha de comando.</span><span class="sxs-lookup"><span data-stu-id="40e64-145">With the sample code shown in the preceding section, you'll see logs in the console when you run from the command line.</span></span> <span data-ttu-id="40e64-146">Aqui está um exemplo da saída do console:</span><span class="sxs-lookup"><span data-stu-id="40e64-146">Here's an example of console output:</span></span>

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

<span data-ttu-id="40e64-147">Esses logs foram criados acessando `http://localhost:5000/api/todo/0`, que dispara a execução das duas chamadas `ILogger` mostradas na seção anterior.</span><span class="sxs-lookup"><span data-stu-id="40e64-147">These logs were created by going to `http://localhost:5000/api/todo/0`, which triggers execution of both `ILogger` calls shown in the preceding section.</span></span>

<span data-ttu-id="40e64-148">Aqui está um exemplo de como os mesmos logs aparecem na janela Depuração quando você executa o aplicativo de exemplo no Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="40e64-148">Here's an example of the same logs as they appear in the Debug window when you run the sample application in Visual Studio:</span></span>

```console
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request starting HTTP/1.1 GET http://localhost:53104/api/todo/0  
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
TodoApi.Controllers.TodoController:Information: Getting item 0
TodoApi.Controllers.TodoController:Warning: GetById(0) NOT FOUND
Microsoft.AspNetCore.Mvc.StatusCodeResult:Information: Executing HttpStatusCodeResult, setting HTTP status code 404
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 152.5657ms
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request finished in 316.3195ms 404 
```

<span data-ttu-id="40e64-149">Os logs criados pelas chamadas `ILogger` mostradas na seção anterior começam com "TodoApi.Controllers.TodoController".</span><span class="sxs-lookup"><span data-stu-id="40e64-149">The logs that were created by the `ILogger` calls shown in the preceding section begin with "TodoApi.Controllers.TodoController".</span></span> <span data-ttu-id="40e64-150">Os logs que começam com categorias "Microsoft" são do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="40e64-150">The logs that begin with "Microsoft" categories are from ASP.NET Core.</span></span> <span data-ttu-id="40e64-151">O próprio ASP.NET Core e o código do aplicativo estão usando a mesma API de registro em log e os mesmos provedores de log.</span><span class="sxs-lookup"><span data-stu-id="40e64-151">ASP.NET Core itself and your application code are using the same logging API and the same logging providers.</span></span>

<span data-ttu-id="40e64-152">O restante deste artigo explica alguns detalhes e opções para registro em log.</span><span class="sxs-lookup"><span data-stu-id="40e64-152">The remainder of this article explains some details and options for logging.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="40e64-153">Pacotes NuGet</span><span class="sxs-lookup"><span data-stu-id="40e64-153">NuGet packages</span></span>

<span data-ttu-id="40e64-154">As interfaces `ILogger` e `ILoggerFactory` estão em [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/) e as implementações padrão para elas estão em [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).</span><span class="sxs-lookup"><span data-stu-id="40e64-154">The `ILogger` and `ILoggerFactory` interfaces are in [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), and default implementations for them are in [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).</span></span>

## <a name="log-category"></a><span data-ttu-id="40e64-155">Categoria de log</span><span class="sxs-lookup"><span data-stu-id="40e64-155">Log category</span></span>

<span data-ttu-id="40e64-156">Um *categoria* é incluída com cada log que você cria.</span><span class="sxs-lookup"><span data-stu-id="40e64-156">A *category* is included with each log that you create.</span></span> <span data-ttu-id="40e64-157">Você especifica a categoria ao criar um objeto `ILogger`.</span><span class="sxs-lookup"><span data-stu-id="40e64-157">You specify the category when you create an `ILogger` object.</span></span> <span data-ttu-id="40e64-158">A categoria pode ser qualquer cadeia de caracteres, mas uma convenção é usar o nome totalmente qualificado da classe da qual os logs são gravados.</span><span class="sxs-lookup"><span data-stu-id="40e64-158">The category may be any string, but a convention is to use the fully qualified name of the class from which the logs are written.</span></span> <span data-ttu-id="40e64-159">Por exemplo: "TodoApi.Controllers.TodoController".</span><span class="sxs-lookup"><span data-stu-id="40e64-159">For example: "TodoApi.Controllers.TodoController".</span></span>

<span data-ttu-id="40e64-160">Você pode especificar a categoria como uma cadeia de caracteres ou usar um método de extensão que deriva a categoria do tipo.</span><span class="sxs-lookup"><span data-stu-id="40e64-160">You can specify the category as a string or use an extension method that derives the category from the type.</span></span> <span data-ttu-id="40e64-161">Para especificar a categoria como uma cadeia de caracteres, chame `CreateLogger` em uma instância de `ILoggerFactory`, conforme mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="40e64-161">To specify the category as a string, call `CreateLogger` on an `ILoggerFactory` instance, as shown below.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

<span data-ttu-id="40e64-162">Na maioria das vezes será mais fácil usar `ILogger<T>`, conforme mostrado no exemplo a seguir.</span><span class="sxs-lookup"><span data-stu-id="40e64-162">Most of the time, it will be easier to use `ILogger<T>`, as in the following example.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

<span data-ttu-id="40e64-163">Isso é equivalente a chamar `CreateLogger` com o nome de tipo totalmente qualificado de `T`.</span><span class="sxs-lookup"><span data-stu-id="40e64-163">This is equivalent to calling `CreateLogger` with the fully qualified type name of `T`.</span></span>

## <a name="log-level"></a><span data-ttu-id="40e64-164">Nível de log</span><span class="sxs-lookup"><span data-stu-id="40e64-164">Log level</span></span>

<span data-ttu-id="40e64-165">Sempre que você grava um log, você especifica o [LogLevel](/dotnet/api/microsoft.extensions.logging.logLevel).</span><span class="sxs-lookup"><span data-stu-id="40e64-165">Each time you write a log, you specify its [LogLevel](/dotnet/api/microsoft.extensions.logging.logLevel).</span></span> <span data-ttu-id="40e64-166">O nível de log indica o grau de gravidade ou importância.</span><span class="sxs-lookup"><span data-stu-id="40e64-166">The log level indicates the degree of severity or importance.</span></span> <span data-ttu-id="40e64-167">Por exemplo, você pode gravar um log `Information` quando um método é finalizado normalmente, um log `Warning` quando um método retorna um código de retorno 404 e um log `Error` ao capturar uma exceção inesperada.</span><span class="sxs-lookup"><span data-stu-id="40e64-167">For example, you might write an `Information` log when a method ends normally, a `Warning` log when a method returns a 404 return code, and an `Error` log when you catch an unexpected exception.</span></span>

<span data-ttu-id="40e64-168">No exemplo de código a seguir, os nomes dos métodos (por exemplo, `LogWarning`) especificam o nível de log.</span><span class="sxs-lookup"><span data-stu-id="40e64-168">In the following code example, the names of the methods (for example, `LogWarning`) specify the log level.</span></span> <span data-ttu-id="40e64-169">O primeiro parâmetro é a [ID de evento de log](#log-event-id).</span><span class="sxs-lookup"><span data-stu-id="40e64-169">The first parameter is the [Log event ID](#log-event-id).</span></span> <span data-ttu-id="40e64-170">O segundo parâmetro é um [modelo de mensagem](#log-message-template) com espaços reservados para valores de argumento fornecidos pelos parâmetros de método restantes.</span><span class="sxs-lookup"><span data-stu-id="40e64-170">The second parameter is a [message template](#log-message-template) with placeholders for argument values provided by the remaining method parameters.</span></span> <span data-ttu-id="40e64-171">Os parâmetros de método serão explicados com mais detalhes posteriormente neste artigo.</span><span class="sxs-lookup"><span data-stu-id="40e64-171">The method parameters are explained in more detail later in this article.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="40e64-172">Os métodos de log que incluem o nível no nome do método são [métodos de extensão para ILogger](/dotnet/api/microsoft.extensions.logging.loggerextensions).</span><span class="sxs-lookup"><span data-stu-id="40e64-172">Log methods that include the level in the method name are [extension methods for ILogger](/dotnet/api/microsoft.extensions.logging.loggerextensions).</span></span> <span data-ttu-id="40e64-173">Nos bastidores, esses métodos chamam um método `Log` que recebe um parâmetro `LogLevel`.</span><span class="sxs-lookup"><span data-stu-id="40e64-173">Behind the scenes, these methods call a `Log` method that takes a `LogLevel` parameter.</span></span> <span data-ttu-id="40e64-174">Você pode chamar o método `Log` diretamente em vez de um desses métodos de extensão, mas a sintaxe é relativamente complicada.</span><span class="sxs-lookup"><span data-stu-id="40e64-174">You can call the `Log` method directly rather than one of these extension methods, but the syntax is relatively complicated.</span></span> <span data-ttu-id="40e64-175">Para obter mais informações, consulte a [interface ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) e o [código-fonte de extensões de agente](https://github.com/aspnet/Logging/blob/master/src/Microsoft.Extensions.Logging.Abstractions/LoggerExtensions.cs).</span><span class="sxs-lookup"><span data-stu-id="40e64-175">For more information, see the [ILogger interface](/dotnet/api/microsoft.extensions.logging.ilogger) and the [logger extensions source code](https://github.com/aspnet/Logging/blob/master/src/Microsoft.Extensions.Logging.Abstractions/LoggerExtensions.cs).</span></span>

<span data-ttu-id="40e64-176">O ASP.NET Core define os seguintes [níveis de log](/dotnet/api/microsoft.extensions.logging.loglevel), ordenados aqui da menor para a maior gravidade.</span><span class="sxs-lookup"><span data-stu-id="40e64-176">ASP.NET Core defines the following [log levels](/dotnet/api/microsoft.extensions.logging.loglevel), ordered here from least to highest severity.</span></span>

* <span data-ttu-id="40e64-177">Trace = 0</span><span class="sxs-lookup"><span data-stu-id="40e64-177">Trace = 0</span></span>

  <span data-ttu-id="40e64-178">Para obter informações valiosas somente para um desenvolvedor que esteja depurando um problema.</span><span class="sxs-lookup"><span data-stu-id="40e64-178">For information that's valuable only to a developer debugging an issue.</span></span> <span data-ttu-id="40e64-179">Essas mensagens podem conter dados confidenciais de aplicativos e, portanto, não devem ser habilitadas em um ambiente de produção.</span><span class="sxs-lookup"><span data-stu-id="40e64-179">These messages may contain sensitive application data and so shouldn't be enabled in a production environment.</span></span> <span data-ttu-id="40e64-180">*Desabilitado por padrão.*</span><span class="sxs-lookup"><span data-stu-id="40e64-180">*Disabled by default.*</span></span> <span data-ttu-id="40e64-181">Exemplo: `Credentials: {"User":"someuser", "Password":"P@ssword"}`</span><span class="sxs-lookup"><span data-stu-id="40e64-181">Example: `Credentials: {"User":"someuser", "Password":"P@ssword"}`</span></span>

* <span data-ttu-id="40e64-182">Debug = 1</span><span class="sxs-lookup"><span data-stu-id="40e64-182">Debug = 1</span></span>

  <span data-ttu-id="40e64-183">Para informações que tenham utilidade de curto prazo durante o desenvolvimento e a depuração.</span><span class="sxs-lookup"><span data-stu-id="40e64-183">For information that has short-term usefulness during development and debugging.</span></span> <span data-ttu-id="40e64-184">Exemplo: `Entering method Configure with flag set to true.` Você normalmente não habilitaria logs de nível `Debug` em produção, a menos que estivesse solucionando problemas, devido ao alto volume de logs.</span><span class="sxs-lookup"><span data-stu-id="40e64-184">Example: `Entering method Configure with flag set to true.` You typically wouldn't enable `Debug` level logs in production unless you are troubleshooting, due to the high volume of logs.</span></span>

* <span data-ttu-id="40e64-185">Information = 2</span><span class="sxs-lookup"><span data-stu-id="40e64-185">Information = 2</span></span>

  <span data-ttu-id="40e64-186">Para rastrear o fluxo geral do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="40e64-186">For tracking the general flow of the application.</span></span> <span data-ttu-id="40e64-187">Esses logs normalmente têm algum valor a longo prazo.</span><span class="sxs-lookup"><span data-stu-id="40e64-187">These logs typically have some long-term value.</span></span> <span data-ttu-id="40e64-188">Exemplo: `Request received for path /api/todo`</span><span class="sxs-lookup"><span data-stu-id="40e64-188">Example: `Request received for path /api/todo`</span></span>

* <span data-ttu-id="40e64-189">Warning = 3</span><span class="sxs-lookup"><span data-stu-id="40e64-189">Warning = 3</span></span>

  <span data-ttu-id="40e64-190">Para eventos anormais ou inesperados no fluxo de aplicativo.</span><span class="sxs-lookup"><span data-stu-id="40e64-190">For abnormal or unexpected events in the application flow.</span></span> <span data-ttu-id="40e64-191">Eles podem incluir erros ou outras condições que não fazem com que o aplicativo pare, mas que talvez precisem ser investigados.</span><span class="sxs-lookup"><span data-stu-id="40e64-191">These may include errors or other conditions that don't cause the application to stop, but which may need to be investigated.</span></span> <span data-ttu-id="40e64-192">Exceções manipuladas são um local comum para usar o nível de log `Warning`.</span><span class="sxs-lookup"><span data-stu-id="40e64-192">Handled exceptions are a common place to use the `Warning` log level.</span></span> <span data-ttu-id="40e64-193">Exemplo: `FileNotFoundException for file quotes.txt.`</span><span class="sxs-lookup"><span data-stu-id="40e64-193">Example: `FileNotFoundException for file quotes.txt.`</span></span>

* <span data-ttu-id="40e64-194">Error = 4</span><span class="sxs-lookup"><span data-stu-id="40e64-194">Error = 4</span></span>

  <span data-ttu-id="40e64-195">Para erros e exceções que não podem ser manipulados.</span><span class="sxs-lookup"><span data-stu-id="40e64-195">For errors and exceptions that cannot be handled.</span></span> <span data-ttu-id="40e64-196">Essas mensagens indicam uma falha na atividade ou na operação atual (como a solicitação HTTP atual) e não uma falha em todo o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="40e64-196">These messages indicate a failure in the current activity or operation (such as the current HTTP request), not an application-wide failure.</span></span> <span data-ttu-id="40e64-197">Mensagem de log de exemplo:`Cannot insert record due to duplicate key violation.`</span><span class="sxs-lookup"><span data-stu-id="40e64-197">Example log message: `Cannot insert record due to duplicate key violation.`</span></span>

* <span data-ttu-id="40e64-198">Critical = 5</span><span class="sxs-lookup"><span data-stu-id="40e64-198">Critical = 5</span></span>

  <span data-ttu-id="40e64-199">Para falhas que exigem atenção imediata.</span><span class="sxs-lookup"><span data-stu-id="40e64-199">For failures that require immediate attention.</span></span> <span data-ttu-id="40e64-200">Exemplos: cenários de perda de dados, espaço em disco insuficiente.</span><span class="sxs-lookup"><span data-stu-id="40e64-200">Examples: data loss scenarios, out of disk space.</span></span>

<span data-ttu-id="40e64-201">Você pode usar o nível de log para controlar a quantidade de saída de log que é gravada em uma mídia de armazenamento específica ou em uma janela de exibição.</span><span class="sxs-lookup"><span data-stu-id="40e64-201">You can use the log level to control how much log output is written to a particular storage medium or display window.</span></span> <span data-ttu-id="40e64-202">Por exemplo, em produção, você pode desejar que todos os logs de nível `Information` e inferior vão para um armazenamento de dados de volume e que todos os logs de nível `Warning` e superior vão para um armazenamento de dados de valor.</span><span class="sxs-lookup"><span data-stu-id="40e64-202">For example, in production you might want all logs of `Information` level and lower to go to a volume data store, and all logs of `Warning` level and higher to go to a value data store.</span></span> <span data-ttu-id="40e64-203">Durante o desenvolvimento, você normalmente poderia enviar os logs de gravidade `Warning` ou mais alta para o console.</span><span class="sxs-lookup"><span data-stu-id="40e64-203">During development, you might normally send logs of `Warning` or higher severity to the console.</span></span> <span data-ttu-id="40e64-204">Então, quando precisar solucionar problemas, você poderá adicionar o nível `Debug`.</span><span class="sxs-lookup"><span data-stu-id="40e64-204">Then when you need to troubleshoot, you can add `Debug` level.</span></span> <span data-ttu-id="40e64-205">A seção [Filtragem de log](#log-filtering) mais adiante neste artigo explicará como controlar os níveis de log que um provedor manipula.</span><span class="sxs-lookup"><span data-stu-id="40e64-205">The [Log filtering](#log-filtering) section later in this article explains how to control which log levels a provider handles.</span></span>

<span data-ttu-id="40e64-206">A estrutura do ASP.NET Core grava logs de nível `Debug` para eventos de estrutura.</span><span class="sxs-lookup"><span data-stu-id="40e64-206">The ASP.NET Core framework writes `Debug` level logs for framework events.</span></span> <span data-ttu-id="40e64-207">Os exemplos de log anteriores neste artigo excluíram logs abaixo do nível `Information`, portanto, os logs de nível `Debug` não foram mostrados.</span><span class="sxs-lookup"><span data-stu-id="40e64-207">The log examples earlier in this article excluded logs below `Information` level, so no `Debug` level logs were shown.</span></span> <span data-ttu-id="40e64-208">Aqui está um exemplo de logs do console, caso você execute o aplicativo de exemplo configurado para mostrar logs de nível `Debug` e superiores para o provedor de console.</span><span class="sxs-lookup"><span data-stu-id="40e64-208">Here's an example of console logs if you run the sample application configured to show `Debug` and higher logs for the console provider.</span></span>

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

## <a name="log-event-id"></a><span data-ttu-id="40e64-209">ID de evento de log</span><span class="sxs-lookup"><span data-stu-id="40e64-209">Log event ID</span></span>

<span data-ttu-id="40e64-210">Sempre que você grava um log, você pode especificar uma *ID de evento*.</span><span class="sxs-lookup"><span data-stu-id="40e64-210">Each time you write a log, you can specify an *event ID*.</span></span> <span data-ttu-id="40e64-211">O aplicativo de exemplo faz isso usando uma classe `LoggingEvents` definida localmente:</span><span class="sxs-lookup"><span data-stu-id="40e64-211">The sample app does this by using a locally-defined `LoggingEvents` class:</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/sample//Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

<span data-ttu-id="40e64-212">Uma ID de evento é um valor inteiro que pode ser usado para associar um conjunto de eventos registrados.</span><span class="sxs-lookup"><span data-stu-id="40e64-212">An event ID is an integer value that you can use to associate a set of logged events with one another.</span></span> <span data-ttu-id="40e64-213">Por exemplo, um log para adicionar um item a um carrinho de compras pode ser ter a ID de evento 1000 e um log para concluir uma compra pode ter a ID de evento 1001.</span><span class="sxs-lookup"><span data-stu-id="40e64-213">For instance, a log for adding an item to a shopping cart could be event ID 1000 and a log for completing a purchase could be event ID 1001.</span></span>

<span data-ttu-id="40e64-214">Na saída do registro em log, a ID do evento pode ser armazenada em um campo ou incluída na mensagem de texto, dependendo do provedor.</span><span class="sxs-lookup"><span data-stu-id="40e64-214">In logging output, the event ID may be stored in a field or included in the text message, depending on the provider.</span></span> <span data-ttu-id="40e64-215">O provedor Depuração não mostra as IDs de evento, mas o provedor do console sim, entre colchetes depois da categoria:</span><span class="sxs-lookup"><span data-stu-id="40e64-215">The Debug provider doesn't show event IDs, but the console provider shows them in brackets after the category:</span></span>

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-template"></a><span data-ttu-id="40e64-216">Modelo de mensagem de log</span><span class="sxs-lookup"><span data-stu-id="40e64-216">Log message template</span></span>

<span data-ttu-id="40e64-217">Sempre que você grava uma mensagem de log, você fornece um modelo de mensagem.</span><span class="sxs-lookup"><span data-stu-id="40e64-217">Each time you write a log message, you provide a message template.</span></span> <span data-ttu-id="40e64-218">O modelo de mensagem pode ser uma cadeia de caracteres ou pode conter espaços reservados nomeados, nos quais valores de argumento serão colocados.</span><span class="sxs-lookup"><span data-stu-id="40e64-218">The message template can be a string or it can contain named placeholders into which argument values are placed.</span></span> <span data-ttu-id="40e64-219">O modelo não é uma cadeia de caracteres de formatação e os espaços reservados devem ser nomeados, não numerados.</span><span class="sxs-lookup"><span data-stu-id="40e64-219">The template isn't a format string, and placeholders should be named, not numbered.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="40e64-220">A ordem dos espaços reservados e não de seus nomes, determina quais parâmetros serão usados para fornecer seus valores.</span><span class="sxs-lookup"><span data-stu-id="40e64-220">The order of placeholders, not their names, determines which parameters are used to provide their values.</span></span> <span data-ttu-id="40e64-221">Se você tiver o seguinte código:</span><span class="sxs-lookup"><span data-stu-id="40e64-221">If you have the following code:</span></span>

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

<span data-ttu-id="40e64-222">A mensagem de log resultante terá esta aparência:</span><span class="sxs-lookup"><span data-stu-id="40e64-222">The resulting log message looks like this:</span></span>

```
Parameter values: parm1, parm2
```

<span data-ttu-id="40e64-223">A estrutura de registros realiza a formatação de mensagens dessa maneira para possibilitar que os provedores de log implementem o [registro em log semântico, também conhecido como registro em log estruturado](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="40e64-223">The logging framework does message formatting in this way to make it possible for logging providers to implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span> <span data-ttu-id="40e64-224">Como os próprios argumentos são passados para o sistema de registro em log, não apenas o modelo de mensagem formatada, mas os provedores de log, também poderão armazenar os valores de parâmetro como campos, além do modelo de mensagem.</span><span class="sxs-lookup"><span data-stu-id="40e64-224">Because the arguments themselves are passed to the logging system, not just the formatted message template, logging providers can store the parameter values as fields in addition to the message template.</span></span> <span data-ttu-id="40e64-225">Se você estiver direcionando a saída de log para o Armazenamento de Tabelas do Azure e sua chamada de método do agente tiver esta aparência:</span><span class="sxs-lookup"><span data-stu-id="40e64-225">If you're directing your log output to Azure Table Storage and your logger method call looks like this:</span></span>

```csharp
_logger.LogInformation("Getting item {ID} at {RequestTime}", id, DateTime.Now);
```

<span data-ttu-id="40e64-226">Cada entidade da Tabela do Azure poderá ter propriedades `ID` e `RequestTime`, o que simplificará as consultas nos dados de log.</span><span class="sxs-lookup"><span data-stu-id="40e64-226">Each Azure Table entity can have `ID` and `RequestTime` properties, which simplifies queries on log data.</span></span> <span data-ttu-id="40e64-227">Você poderá encontrar todos os logs em um determinado intervalo de `RequestTime` sem a necessidade de analisar o tempo limite da mensagem de texto.</span><span class="sxs-lookup"><span data-stu-id="40e64-227">You can find all logs within a particular `RequestTime` range without the need to parse the time out of the text message.</span></span>

## <a name="logging-exceptions"></a><span data-ttu-id="40e64-228">Exceções de registro em log</span><span class="sxs-lookup"><span data-stu-id="40e64-228">Logging exceptions</span></span>

<span data-ttu-id="40e64-229">Os métodos de agente têm sobrecargas que permitem que você passe uma exceção, como no exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="40e64-229">The logger methods have overloads that let you pass in an exception, as in the following example:</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

<span data-ttu-id="40e64-230">Provedores diferentes manipulam as informações de exceção de maneiras diferentes.</span><span class="sxs-lookup"><span data-stu-id="40e64-230">Different providers handle the exception information in different ways.</span></span> <span data-ttu-id="40e64-231">Aqui está um exemplo da saída do provedor Depuração do código mostrado acima.</span><span class="sxs-lookup"><span data-stu-id="40e64-231">Here's an example of Debug provider output from the code shown above.</span></span>

```
TodoApi.Controllers.TodoController:Warning: GetById(036dd898-fb01-47e8-9a65-f92eb73cf924) NOT FOUND

System.Exception: Item not found exception.
 at TodoApi.Controllers.TodoController.GetById(String id) in C:\logging\sample\src\TodoApi\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a><span data-ttu-id="40e64-232">Filtragem de log</span><span class="sxs-lookup"><span data-stu-id="40e64-232">Log filtering</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="40e64-233">Você pode especificar um nível de log mínimo para um provedor e uma categoria específicos ou para todos os provedores ou todas as categorias.</span><span class="sxs-lookup"><span data-stu-id="40e64-233">You can specify a minimum log level for a specific provider and category or for all providers or all categories.</span></span> <span data-ttu-id="40e64-234">Os logs abaixo do nível mínimo não serão passados para esse provedor, para que não sejam exibidos ou armazenados.</span><span class="sxs-lookup"><span data-stu-id="40e64-234">Any logs below the minimum level aren't passed to that provider, so they don't get displayed or stored.</span></span> 

<span data-ttu-id="40e64-235">Se quiser suprimir todos os logs, você poderá especificar `LogLevel.None` como o nível de log mínimo.</span><span class="sxs-lookup"><span data-stu-id="40e64-235">If you want to suppress all logs, you can specify `LogLevel.None` as the minimum log level.</span></span> <span data-ttu-id="40e64-236">O valor inteiro de `LogLevel.None` é 6, que é maior do que `LogLevel.Critical` (5).</span><span class="sxs-lookup"><span data-stu-id="40e64-236">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

<span data-ttu-id="40e64-237">**Criar regras de filtro na configuração**</span><span class="sxs-lookup"><span data-stu-id="40e64-237">**Create filter rules in configuration**</span></span>

<span data-ttu-id="40e64-238">Os modelos de projeto criam código que chama `CreateDefaultBuilder` para configurar o registro em log para os provedores Console e Depuração.</span><span class="sxs-lookup"><span data-stu-id="40e64-238">The project templates create code that calls `CreateDefaultBuilder` to set up logging for the Console and Debug providers.</span></span> <span data-ttu-id="40e64-239">O método `CreateDefaultBuilder` também configura o registro em log para procurar a configuração em uma seção `Logging`, usando código semelhante ao seguinte:</span><span class="sxs-lookup"><span data-stu-id="40e64-239">The `CreateDefaultBuilder` method also sets up logging to look for configuration in a `Logging` section, using code like the following:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_ExpandDefault&highlight=15)]

<span data-ttu-id="40e64-240">Os dados de configuração especificam níveis de log mínimo por provedor e por categoria, como no exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="40e64-240">The configuration data specifies minimum log levels by provider and category, as in the following example:</span></span>

[!code-json[](index/sample2/appsettings.json)]

<span data-ttu-id="40e64-241">Este JSON cria seis regras de filtro, uma para o provedor Depuração, quatro para o provedor Console e uma que se aplica a todos os provedores.</span><span class="sxs-lookup"><span data-stu-id="40e64-241">This JSON creates six filter rules, one for the Debug provider, four for the Console provider, and one that applies to all providers.</span></span> <span data-ttu-id="40e64-242">Você verá posteriormente como apenas uma dessas regras é escolhida para cada provedor quando um objeto `ILogger` é criado.</span><span class="sxs-lookup"><span data-stu-id="40e64-242">You'll see later how just one of these rules is chosen for each provider when an `ILogger` object is created.</span></span>

<span data-ttu-id="40e64-243">**Regras de filtro no código**</span><span class="sxs-lookup"><span data-stu-id="40e64-243">**Filter rules in code**</span></span>

<span data-ttu-id="40e64-244">Você pode registrar regras de filtro no código, conforme mostrado no exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="40e64-244">You can register filter rules in code, as shown in the following example:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

<span data-ttu-id="40e64-245">O segundo `AddFilter` especifica o provedor Depuração usando seu nome de tipo.</span><span class="sxs-lookup"><span data-stu-id="40e64-245">The second `AddFilter` specifies the Debug provider by using its type name.</span></span> <span data-ttu-id="40e64-246">O primeiro `AddFilter` se aplica a todos os provedores porque ele não especifica um tipo de provedor.</span><span class="sxs-lookup"><span data-stu-id="40e64-246">The first `AddFilter` applies to all providers because it doesn't specify a provider type.</span></span>

<span data-ttu-id="40e64-247">**Como as regras de filtragem são aplicadas**</span><span class="sxs-lookup"><span data-stu-id="40e64-247">**How filtering rules are applied**</span></span>

<span data-ttu-id="40e64-248">Os dados de configuração e o código `AddFilter`, mostrados nos exemplos anteriores, criam as regras mostradas na tabela a seguir.</span><span class="sxs-lookup"><span data-stu-id="40e64-248">The configuration data and the `AddFilter` code shown in the preceding examples create the rules shown in the following table.</span></span> <span data-ttu-id="40e64-249">As primeiras seis vêm do exemplo de configuração e as últimas duas vêm do exemplo de código.</span><span class="sxs-lookup"><span data-stu-id="40e64-249">The first six come from the configuration example and the last two come from the code example.</span></span>

| <span data-ttu-id="40e64-250">Número</span><span class="sxs-lookup"><span data-stu-id="40e64-250">Number</span></span> | <span data-ttu-id="40e64-251">Provider</span><span class="sxs-lookup"><span data-stu-id="40e64-251">Provider</span></span>      | <span data-ttu-id="40e64-252">Categorias que começam com...</span><span class="sxs-lookup"><span data-stu-id="40e64-252">Categories that begin with ...</span></span>          | <span data-ttu-id="40e64-253">Nível de log mínimo</span><span class="sxs-lookup"><span data-stu-id="40e64-253">Minimum log level</span></span> |
| :----: | ------------- | --------------------------------------- | ----------------- |
| <span data-ttu-id="40e64-254">1</span><span class="sxs-lookup"><span data-stu-id="40e64-254">1</span></span>      | <span data-ttu-id="40e64-255">Depurar</span><span class="sxs-lookup"><span data-stu-id="40e64-255">Debug</span></span>         | <span data-ttu-id="40e64-256">Todas as categorias</span><span class="sxs-lookup"><span data-stu-id="40e64-256">All categories</span></span>                          | <span data-ttu-id="40e64-257">Informações</span><span class="sxs-lookup"><span data-stu-id="40e64-257">Information</span></span>       |
| <span data-ttu-id="40e64-258">2</span><span class="sxs-lookup"><span data-stu-id="40e64-258">2</span></span>      | <span data-ttu-id="40e64-259">Console</span><span class="sxs-lookup"><span data-stu-id="40e64-259">Console</span></span>       | <span data-ttu-id="40e64-260">Microsoft.AspNetCore.Mvc.Razor.Internal</span><span class="sxs-lookup"><span data-stu-id="40e64-260">Microsoft.AspNetCore.Mvc.Razor.Internal</span></span> | <span data-ttu-id="40e64-261">Aviso</span><span class="sxs-lookup"><span data-stu-id="40e64-261">Warning</span></span>           |
| <span data-ttu-id="40e64-262">3</span><span class="sxs-lookup"><span data-stu-id="40e64-262">3</span></span>      | <span data-ttu-id="40e64-263">Console</span><span class="sxs-lookup"><span data-stu-id="40e64-263">Console</span></span>       | <span data-ttu-id="40e64-264">Microsoft.AspNetCore.Mvc.Razor.Razor</span><span class="sxs-lookup"><span data-stu-id="40e64-264">Microsoft.AspNetCore.Mvc.Razor.Razor</span></span>    | <span data-ttu-id="40e64-265">Depurar</span><span class="sxs-lookup"><span data-stu-id="40e64-265">Debug</span></span>             |
| <span data-ttu-id="40e64-266">4</span><span class="sxs-lookup"><span data-stu-id="40e64-266">4</span></span>      | <span data-ttu-id="40e64-267">Console</span><span class="sxs-lookup"><span data-stu-id="40e64-267">Console</span></span>       | <span data-ttu-id="40e64-268">Microsoft.AspNetCore.Mvc.Razor</span><span class="sxs-lookup"><span data-stu-id="40e64-268">Microsoft.AspNetCore.Mvc.Razor</span></span>          | <span data-ttu-id="40e64-269">Erro</span><span class="sxs-lookup"><span data-stu-id="40e64-269">Error</span></span>             |
| <span data-ttu-id="40e64-270">5</span><span class="sxs-lookup"><span data-stu-id="40e64-270">5</span></span>      | <span data-ttu-id="40e64-271">Console</span><span class="sxs-lookup"><span data-stu-id="40e64-271">Console</span></span>       | <span data-ttu-id="40e64-272">Todas as categorias</span><span class="sxs-lookup"><span data-stu-id="40e64-272">All categories</span></span>                          | <span data-ttu-id="40e64-273">Informações</span><span class="sxs-lookup"><span data-stu-id="40e64-273">Information</span></span>       |
| <span data-ttu-id="40e64-274">6</span><span class="sxs-lookup"><span data-stu-id="40e64-274">6</span></span>      | <span data-ttu-id="40e64-275">Todos os provedores</span><span class="sxs-lookup"><span data-stu-id="40e64-275">All providers</span></span> | <span data-ttu-id="40e64-276">Todas as categorias</span><span class="sxs-lookup"><span data-stu-id="40e64-276">All categories</span></span>                          | <span data-ttu-id="40e64-277">Depurar</span><span class="sxs-lookup"><span data-stu-id="40e64-277">Debug</span></span>             |
| <span data-ttu-id="40e64-278">7</span><span class="sxs-lookup"><span data-stu-id="40e64-278">7</span></span>      | <span data-ttu-id="40e64-279">Todos os provedores</span><span class="sxs-lookup"><span data-stu-id="40e64-279">All providers</span></span> | <span data-ttu-id="40e64-280">Sistema</span><span class="sxs-lookup"><span data-stu-id="40e64-280">System</span></span>                                  | <span data-ttu-id="40e64-281">Depurar</span><span class="sxs-lookup"><span data-stu-id="40e64-281">Debug</span></span>             |
| <span data-ttu-id="40e64-282">8</span><span class="sxs-lookup"><span data-stu-id="40e64-282">8</span></span>      | <span data-ttu-id="40e64-283">Depurar</span><span class="sxs-lookup"><span data-stu-id="40e64-283">Debug</span></span>         | <span data-ttu-id="40e64-284">Microsoft</span><span class="sxs-lookup"><span data-stu-id="40e64-284">Microsoft</span></span>                               | <span data-ttu-id="40e64-285">Rastrear</span><span class="sxs-lookup"><span data-stu-id="40e64-285">Trace</span></span>             |

<span data-ttu-id="40e64-286">Quando você cria um objeto `ILogger` para usar para gravar logs, o objeto `ILoggerFactory` seleciona uma única regra por provedor para aplicar a esse agente.</span><span class="sxs-lookup"><span data-stu-id="40e64-286">When you create an `ILogger` object to write logs with, the `ILoggerFactory` object selects a single rule per provider to apply to that logger.</span></span> <span data-ttu-id="40e64-287">Todas as mensagens gravadas pelo objeto `ILogger` são filtradas com base nas regras selecionadas.</span><span class="sxs-lookup"><span data-stu-id="40e64-287">All messages written by that `ILogger` object are filtered based on the selected rules.</span></span> <span data-ttu-id="40e64-288">A regra mais específica possível para cada par de categoria e provedor é selecionada dentre as regras disponíveis.</span><span class="sxs-lookup"><span data-stu-id="40e64-288">The most specific rule possible for each provider and category pair is selected from the available rules.</span></span>

<span data-ttu-id="40e64-289">O algoritmo a seguir é usado para cada provedor quando um `ILogger` é criado para uma determinada categoria:</span><span class="sxs-lookup"><span data-stu-id="40e64-289">The following algorithm is used for each provider when an `ILogger` is created for a given category:</span></span>

* <span data-ttu-id="40e64-290">Selecione todas as regras que correspondem ao provedor ou seu alias.</span><span class="sxs-lookup"><span data-stu-id="40e64-290">Select all rules that match the provider or its alias.</span></span> <span data-ttu-id="40e64-291">Se nenhuma for encontrada, selecione todas as regras com um provedor vazio.</span><span class="sxs-lookup"><span data-stu-id="40e64-291">If none are found, select all rules with an empty provider.</span></span>
* <span data-ttu-id="40e64-292">Do resultado da etapa anterior, selecione as regras com o prefixo de categoria de maior correspondência.</span><span class="sxs-lookup"><span data-stu-id="40e64-292">From the result of the preceding step, select rules with longest matching category prefix.</span></span> <span data-ttu-id="40e64-293">Se nenhuma for encontrada, selecione todas as regras que não especificam uma categoria.</span><span class="sxs-lookup"><span data-stu-id="40e64-293">If none are found, select all rules that don't specify a category.</span></span>
* <span data-ttu-id="40e64-294">Se várias regras forem selecionadas use a **última**.</span><span class="sxs-lookup"><span data-stu-id="40e64-294">If multiple rules are selected take the **last** one.</span></span>
* <span data-ttu-id="40e64-295">Se nenhuma regra for selecionada, use `MinimumLevel`.</span><span class="sxs-lookup"><span data-stu-id="40e64-295">If no rules are selected, use `MinimumLevel`.</span></span>

<span data-ttu-id="40e64-296">Por exemplo, suponha que você tem a lista anterior de regras e cria um objeto `ILogger` para a categoria "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":</span><span class="sxs-lookup"><span data-stu-id="40e64-296">For example, suppose you have the preceding list of rules and you create an `ILogger` object for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":</span></span>

* <span data-ttu-id="40e64-297">Para o provedor Depuração as regras 1, 6 e 8 se aplicam.</span><span class="sxs-lookup"><span data-stu-id="40e64-297">For the Debug provider, rules 1, 6, and 8 apply.</span></span> <span data-ttu-id="40e64-298">A regra 8 é mais específica, portanto é a que será selecionada.</span><span class="sxs-lookup"><span data-stu-id="40e64-298">Rule 8 is most specific, so that's the one selected.</span></span>
* <span data-ttu-id="40e64-299">Para o provedor Console as regras 3, 4, 5 e 6 se aplicam.</span><span class="sxs-lookup"><span data-stu-id="40e64-299">For the Console provider, rules 3, 4, 5, and 6 apply.</span></span> <span data-ttu-id="40e64-300">A regra 3 é a mais específica.</span><span class="sxs-lookup"><span data-stu-id="40e64-300">Rule 3 is most specific.</span></span>

<span data-ttu-id="40e64-301">Quando você cria logs com um `ILogger` para a categoria "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine", os logs de nível `Trace` e superiores vão para o provedor Depuração e os logs de nível `Debug` e superiores vão para o provedor Console.</span><span class="sxs-lookup"><span data-stu-id="40e64-301">When you create logs with an `ILogger` for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine", logs of `Trace` level and above will go to the Debug provider, and logs of `Debug` level and above will go to the Console provider.</span></span>

<span data-ttu-id="40e64-302">**Aliases de provedor**</span><span class="sxs-lookup"><span data-stu-id="40e64-302">**Provider aliases**</span></span>

<span data-ttu-id="40e64-303">Você pode usar o nome do tipo para especificar um provedor na configuração, mas cada provedor define um *alias* menor que é mais fácil de usar.</span><span class="sxs-lookup"><span data-stu-id="40e64-303">You can use the type name to specify a provider in configuration, but each provider defines a shorter *alias* that's easier to use.</span></span> <span data-ttu-id="40e64-304">Para os provedores internos, use os seguintes aliases:</span><span class="sxs-lookup"><span data-stu-id="40e64-304">For the built-in providers, use the following aliases:</span></span>

- <span data-ttu-id="40e64-305">Console</span><span class="sxs-lookup"><span data-stu-id="40e64-305">Console</span></span>
- <span data-ttu-id="40e64-306">Depurar</span><span class="sxs-lookup"><span data-stu-id="40e64-306">Debug</span></span>
- <span data-ttu-id="40e64-307">EventLog</span><span class="sxs-lookup"><span data-stu-id="40e64-307">EventLog</span></span>
- <span data-ttu-id="40e64-308">AzureAppServices</span><span class="sxs-lookup"><span data-stu-id="40e64-308">AzureAppServices</span></span>
- <span data-ttu-id="40e64-309">TraceSource</span><span class="sxs-lookup"><span data-stu-id="40e64-309">TraceSource</span></span>
- <span data-ttu-id="40e64-310">EventSource</span><span class="sxs-lookup"><span data-stu-id="40e64-310">EventSource</span></span>

<span data-ttu-id="40e64-311">**Nível mínimo padrão**</span><span class="sxs-lookup"><span data-stu-id="40e64-311">**Default minimum level**</span></span>

<span data-ttu-id="40e64-312">Há uma configuração de nível mínimo que entra em vigor somente se nenhuma regra de código ou de configuração se aplicar a um provedor e uma categoria determinados.</span><span class="sxs-lookup"><span data-stu-id="40e64-312">There's a minimum level setting that takes effect only if no rules from configuration or code apply for a given provider and category.</span></span> <span data-ttu-id="40e64-313">O exemplo a seguir mostra como definir o nível mínimo:</span><span class="sxs-lookup"><span data-stu-id="40e64-313">The following example shows how to set the minimum level:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_MinLevel&highlight=3)]

<span data-ttu-id="40e64-314">Se você não definir explicitamente o nível mínimo, o valor padrão será `Information`, o que significa que logs `Trace` e `Debug` serão ignorados.</span><span class="sxs-lookup"><span data-stu-id="40e64-314">If you don't explicitly set the minimum level, the default value is `Information`, which means that `Trace` and `Debug` logs are ignored.</span></span>

<span data-ttu-id="40e64-315">**Funções de filtro**</span><span class="sxs-lookup"><span data-stu-id="40e64-315">**Filter functions**</span></span>

<span data-ttu-id="40e64-316">Você pode escrever código em uma função de filtro para aplicar regras de filtragem.</span><span class="sxs-lookup"><span data-stu-id="40e64-316">You can write code in a filter function to apply filtering rules.</span></span> <span data-ttu-id="40e64-317">Uma função de filtro é invocada para todos os provedores e categorias que não têm regras atribuídas a eles por configuração ou código.</span><span class="sxs-lookup"><span data-stu-id="40e64-317">A filter function is invoked for all providers and categories that don't have rules assigned to them by configuration or code.</span></span> <span data-ttu-id="40e64-318">O código na função tem acesso ao tipo de provedor, à categoria e ao nível de log para decidir se uma mensagem deve ser registrada.</span><span class="sxs-lookup"><span data-stu-id="40e64-318">Code in the function has access to the provider type, category, and log level to decide whether or not a message should be logged.</span></span> <span data-ttu-id="40e64-319">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="40e64-319">For example:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="40e64-320">Alguns provedores de log permitem especificar quando os logs devem ser gravados em uma mídia de armazenamento ou ignorados, com base no nível de log e na categoria.</span><span class="sxs-lookup"><span data-stu-id="40e64-320">Some logging providers let you specify when logs should be written to a storage medium or ignored based on log level and category.</span></span>

<span data-ttu-id="40e64-321">Os métodos de extensão `AddConsole` e `AddDebug` fornecem sobrecargas que permitem que você passe critérios de filtragem.</span><span class="sxs-lookup"><span data-stu-id="40e64-321">The `AddConsole` and `AddDebug` extension methods provide overloads that let you pass in filtering criteria.</span></span> <span data-ttu-id="40e64-322">O código de exemplo a seguir faz com que o provedor de console ignore os logs abaixo do nível `Warning`, enquanto o provedor Depuração ignora os logs criados pela estrutura.</span><span class="sxs-lookup"><span data-stu-id="40e64-322">The following sample code causes the console provider to ignore logs below `Warning` level, while the Debug provider ignores logs that the framework creates.</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_AddConsoleAndDebugWithFilter&highlight=6-7)]

<span data-ttu-id="40e64-323">O método `AddEventLog` tem uma sobrecarga que recebe uma instância `EventLogSettings`, que pode conter uma função de filtragem em sua propriedade `Filter`.</span><span class="sxs-lookup"><span data-stu-id="40e64-323">The `AddEventLog` method has an overload that takes an `EventLogSettings` instance, which may contain a filtering function in its `Filter` property.</span></span> <span data-ttu-id="40e64-324">O provedor TraceSource não fornece nenhuma dessas sobrecargas, porque seu nível de registro em log e outros parâmetros são baseados no `SourceSwitch` e no `TraceListener` que ele usa.</span><span class="sxs-lookup"><span data-stu-id="40e64-324">The TraceSource provider doesn't provide any of those overloads, since its logging level and other parameters are based on the `SourceSwitch` and `TraceListener` it uses.</span></span>

<span data-ttu-id="40e64-325">Você pode definir regras de filtragem para todos os provedores registrados com uma instância `ILoggerFactory` usando o método de extensão `WithFilter`.</span><span class="sxs-lookup"><span data-stu-id="40e64-325">You can set filtering rules for all providers that are registered with an `ILoggerFactory` instance by using the `WithFilter` extension method.</span></span> <span data-ttu-id="40e64-326">O exemplo abaixo limita os logs de estrutura (categoria começa com "Microsoft" ou "Sistema") a avisos, permitindo que aplicativo registre no nível de depuração.</span><span class="sxs-lookup"><span data-stu-id="40e64-326">The example below limits framework logs (category begins with "Microsoft" or "System") to warnings while letting the app log at debug level.</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_FactoryFilter&highlight=6-11)]

<span data-ttu-id="40e64-327">Se quiser usar a filtragem para impedir que todos os logs de uma determinada categoria sejam gravados, você poderá especificar `LogLevel.None` como o nível de log mínimo para essa categoria.</span><span class="sxs-lookup"><span data-stu-id="40e64-327">If you want to use filtering to prevent all logs from being written for a particular category, you can specify `LogLevel.None` as the minimum log level for that category.</span></span> <span data-ttu-id="40e64-328">O valor inteiro de `LogLevel.None` é 6, que é maior do que `LogLevel.Critical` (5).</span><span class="sxs-lookup"><span data-stu-id="40e64-328">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

<span data-ttu-id="40e64-329">O método de extensão `WithFilter` é fornecido pelo pacote NuGet [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter).</span><span class="sxs-lookup"><span data-stu-id="40e64-329">The `WithFilter` extension method is provided by the [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) NuGet package.</span></span> <span data-ttu-id="40e64-330">O método retorna um nova instância `ILoggerFactory`, que filtrará as mensagens de log passadas para todos os provedores de agente registrados com ela.</span><span class="sxs-lookup"><span data-stu-id="40e64-330">The method returns a new `ILoggerFactory` instance that will filter the log messages passed to all logger providers registered with it.</span></span> <span data-ttu-id="40e64-331">Isso não afeta nenhuma outra instância de `ILoggerFactory`, incluindo a instância de `ILoggerFactory` original.</span><span class="sxs-lookup"><span data-stu-id="40e64-331">It doesn't affect any other `ILoggerFactory` instances, including the original `ILoggerFactory` instance.</span></span>

::: moniker-end

## <a name="log-scopes"></a><span data-ttu-id="40e64-332">Escopos de log</span><span class="sxs-lookup"><span data-stu-id="40e64-332">Log scopes</span></span>

<span data-ttu-id="40e64-333">Você pode agrupar um conjunto de operações lógicas dentro de um *escopo* para anexar os mesmos dados a cada log que é criado como parte desse conjunto.</span><span class="sxs-lookup"><span data-stu-id="40e64-333">You can group a set of logical operations within a *scope* in order to attach the same data to each log that's created as part of that set.</span></span> <span data-ttu-id="40e64-334">Por exemplo, é conveniente que cada log criado como parte do processamento de uma transação inclua a ID da transação.</span><span class="sxs-lookup"><span data-stu-id="40e64-334">For example, you might want every log created as part of processing a transaction to include the transaction ID.</span></span>

<span data-ttu-id="40e64-335">Um escopo é um tipo `IDisposable` retornado pelo método [ILogger.BeginScope&lt;TState&gt;](/dotnet/api/microsoft.extensions.logging.ilogger.beginscope) e que dura até que seja descartado.</span><span class="sxs-lookup"><span data-stu-id="40e64-335">A scope is an `IDisposable` type that's returned by the [ILogger.BeginScope&lt;TState&gt;](/dotnet/api/microsoft.extensions.logging.ilogger.beginscope) method and lasts until it's disposed.</span></span> <span data-ttu-id="40e64-336">Um escopo é usado pelo encapsulamento das chama de agente em um bloco `using`, conforme mostrado aqui:</span><span class="sxs-lookup"><span data-stu-id="40e64-336">You use a scope by wrapping your logger calls in a `using` block, as shown here:</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

<span data-ttu-id="40e64-337">O código a seguir habilita os escopos para o provedor de console:</span><span class="sxs-lookup"><span data-stu-id="40e64-337">The following code enables scopes for the console provider:</span></span>

::: moniker range="> aspnetcore-2.0"

<span data-ttu-id="40e64-338">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="40e64-338">*Program.cs*:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> <span data-ttu-id="40e64-339">A configuração da opção de agente de console `IncludeScopes` é necessária para habilitar o registro em log baseado em escopo.</span><span class="sxs-lookup"><span data-stu-id="40e64-339">Configuring the `IncludeScopes` console logger option is required to enable scope-based logging.</span></span>
>
> <span data-ttu-id="40e64-340">`IncludeScopes` pode ser configurado por meio de arquivos de configuração *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="40e64-340">`IncludeScopes` can be configured via *appsettings* configuration files.</span></span> <span data-ttu-id="40e64-341">Para obter mais informações, veja a seção [Definição do arquivo de configurações](#settings-file-configuration).</span><span class="sxs-lookup"><span data-stu-id="40e64-341">For more information, see the [Settings file configuration](#settings-file-configuration) section.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="40e64-342">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="40e64-342">*Program.cs*:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> <span data-ttu-id="40e64-343">A configuração da opção de agente de console `IncludeScopes` é necessária para habilitar o registro em log baseado em escopo.</span><span class="sxs-lookup"><span data-stu-id="40e64-343">Configuring the `IncludeScopes` console logger option is required to enable scope-based logging.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="40e64-344">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="40e64-344">*Startup.cs*:</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_Scopes&highlight=6)]

::: moniker-end

<span data-ttu-id="40e64-345">Cada mensagem de log inclui as informações com escopo definido:</span><span class="sxs-lookup"><span data-stu-id="40e64-345">Each log message includes the scoped information:</span></span>

```
info: TodoApi.Controllers.TodoController[1002]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      GetById(0) NOT FOUND
```

## <a name="built-in-logging-providers"></a><span data-ttu-id="40e64-346">Provedores de log internos</span><span class="sxs-lookup"><span data-stu-id="40e64-346">Built-in logging providers</span></span>

<span data-ttu-id="40e64-347">O ASP.NET Core vem com os seguintes provedores:</span><span class="sxs-lookup"><span data-stu-id="40e64-347">ASP.NET Core ships the following providers:</span></span>

* [<span data-ttu-id="40e64-348">Console</span><span class="sxs-lookup"><span data-stu-id="40e64-348">Console</span></span>](#console-provider)
* [<span data-ttu-id="40e64-349">Depurar</span><span class="sxs-lookup"><span data-stu-id="40e64-349">Debug</span></span>](#debug-provider)
* [<span data-ttu-id="40e64-350">EventSource</span><span class="sxs-lookup"><span data-stu-id="40e64-350">EventSource</span></span>](#eventsource-provider)
* [<span data-ttu-id="40e64-351">EventLog</span><span class="sxs-lookup"><span data-stu-id="40e64-351">EventLog</span></span>](#windows-eventlog-provider)
* [<span data-ttu-id="40e64-352">TraceSource</span><span class="sxs-lookup"><span data-stu-id="40e64-352">TraceSource</span></span>](#tracesource-provider)
* [<span data-ttu-id="40e64-353">Serviço de Aplicativo do Azure</span><span class="sxs-lookup"><span data-stu-id="40e64-353">Azure App Service</span></span>](#azure-app-service-provider)

### <a name="console-provider"></a><span data-ttu-id="40e64-354">Provedor do console</span><span class="sxs-lookup"><span data-stu-id="40e64-354">Console provider</span></span>

<span data-ttu-id="40e64-355">O pacote de provedor [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) envia a saída de log para o console.</span><span class="sxs-lookup"><span data-stu-id="40e64-355">The [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) provider package sends log output to the console.</span></span> 

::: moniker range=">= aspnetcore-2.0"


```csharp
logging.AddConsole()
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddConsole()
```

<span data-ttu-id="40e64-356">As [sobrecargas do AddConsole](/dotnet/api/microsoft.extensions.logging.consoleloggerextensions) permitem que você passe um nível de log mínimo, uma função de filtro e um valor booliano que indica se escopos são compatíveis.</span><span class="sxs-lookup"><span data-stu-id="40e64-356">[AddConsole overloads](/dotnet/api/microsoft.extensions.logging.consoleloggerextensions) let you pass in an a minimum log level, a filter function, and a boolean that indicates whether scopes are supported.</span></span> <span data-ttu-id="40e64-357">Outra opção é passar um objeto `IConfiguration`, que pode especificar suporte de escopos e níveis de log.</span><span class="sxs-lookup"><span data-stu-id="40e64-357">Another option is to pass in an `IConfiguration` object, which can specify scopes support and logging levels.</span></span> 

<span data-ttu-id="40e64-358">Se você estiver considerando o provedor de console para uso em produção, lembre-se de que ele tem um impacto significativo no desempenho.</span><span class="sxs-lookup"><span data-stu-id="40e64-358">If you are considering the console provider for use in production, be aware that it has a significant impact on performance.</span></span>

<span data-ttu-id="40e64-359">Quando você cria um novo projeto no Visual Studio, o método `AddConsole` tem essa aparência:</span><span class="sxs-lookup"><span data-stu-id="40e64-359">When you create a new project in Visual Studio, the `AddConsole` method looks like this:</span></span>

```csharp
loggerFactory.AddConsole(Configuration.GetSection("Logging"));
```

<span data-ttu-id="40e64-360">Esse código se refere à seção `Logging` do arquivo *appSettings.json*:</span><span class="sxs-lookup"><span data-stu-id="40e64-360">This code refers to the `Logging` section of the *appSettings.json* file:</span></span>

[!code-json[](index/sample//appsettings.json)]

<span data-ttu-id="40e64-361">As configurações mostradas limitam os logs de estrutura a avisos, permitindo que o aplicativo para faça registros no nível de depuração, conforme explicado na [Filtragem de log](#log-filtering) seção.</span><span class="sxs-lookup"><span data-stu-id="40e64-361">The settings shown limit framework logs to warnings while allowing the app to log at debug level, as explained in the [Log filtering](#log-filtering) section.</span></span> <span data-ttu-id="40e64-362">Para obter mais informações, consulte [Configuração](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="40e64-362">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

::: moniker-end

### <a name="debug-provider"></a><span data-ttu-id="40e64-363">Depurar provedor</span><span class="sxs-lookup"><span data-stu-id="40e64-363">Debug provider</span></span>

<span data-ttu-id="40e64-364">O pacote de provedor [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) grava a saída de log usando a classe [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) (chamadas de método `Debug.WriteLine`).</span><span class="sxs-lookup"><span data-stu-id="40e64-364">The [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) provider package writes log output by using the [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) class (`Debug.WriteLine` method calls).</span></span>

<span data-ttu-id="40e64-365">No Linux, esse provedor grava logs em */var/log/message*.</span><span class="sxs-lookup"><span data-stu-id="40e64-365">On Linux, this provider writes logs to */var/log/message*.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddDebug()
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddDebug()
```

<span data-ttu-id="40e64-366">As [sobrecargas de AddDebug](/dotnet/api/microsoft.extensions.logging.debugloggerfactoryextensions) permitem que você passe um nível de log mínimo ou uma função de filtro.</span><span class="sxs-lookup"><span data-stu-id="40e64-366">[AddDebug overloads](/dotnet/api/microsoft.extensions.logging.debugloggerfactoryextensions) let you pass in a minimum log level or a filter function.</span></span>

::: moniker-end

### <a name="eventsource-provider"></a><span data-ttu-id="40e64-367">Provedor EventSource</span><span class="sxs-lookup"><span data-stu-id="40e64-367">EventSource provider</span></span>

<span data-ttu-id="40e64-368">Para aplicativos que se destinam ao ASP.NET Core 1.1.0 ou superior, o pacote de provedor [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) pode implementar o rastreamento de eventos.</span><span class="sxs-lookup"><span data-stu-id="40e64-368">For apps that target ASP.NET Core 1.1.0 or higher, the [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) provider package can implement event tracing.</span></span> <span data-ttu-id="40e64-369">No Windows, ele usa [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span><span class="sxs-lookup"><span data-stu-id="40e64-369">On Windows, it uses [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span></span> <span data-ttu-id="40e64-370">O provedor é multiplataforma, mas ainda não há ferramentas de coleta e exibição de eventos para Linux ou macOS.</span><span class="sxs-lookup"><span data-stu-id="40e64-370">The provider is cross-platform, but there are no event collection and display tools yet for Linux or macOS.</span></span> 

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddEventSourceLogger()
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddEventSourceLogger()
```

::: moniker-end

<span data-ttu-id="40e64-371">Uma boa maneira de coletar e exibir logs é usar o [utilitário PerfView](https://github.com/Microsoft/perfview).</span><span class="sxs-lookup"><span data-stu-id="40e64-371">A good way to collect and view logs is to use the [PerfView utility](https://github.com/Microsoft/perfview).</span></span> <span data-ttu-id="40e64-372">Há outras ferramentas para exibir os logs do ETW, mas o PerfView proporciona a melhor experiência para trabalhar com os eventos de ETW emitidos pelo ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="40e64-372">There are other tools for viewing ETW logs, but PerfView provides the best experience for working with the ETW events emitted by ASP.NET.</span></span> 

<span data-ttu-id="40e64-373">Para configurar o PerfView para coletar eventos registrados por esse provedor, adicione a cadeia de caracteres `*Microsoft-Extensions-Logging` à lista **Provedores Adicionais**.</span><span class="sxs-lookup"><span data-stu-id="40e64-373">To configure PerfView for collecting events logged by this provider, add the string `*Microsoft-Extensions-Logging` to the **Additional Providers** list.</span></span> <span data-ttu-id="40e64-374">(Não se esqueça do asterisco no início da cadeia de caracteres).</span><span class="sxs-lookup"><span data-stu-id="40e64-374">(Don't miss the asterisk at the start of the string.)</span></span>

![Outros provedores de Perfview](index/_static/perfview-additional-providers.png)

### <a name="windows-eventlog-provider"></a><span data-ttu-id="40e64-376">Provedor EventLog do Windows</span><span class="sxs-lookup"><span data-stu-id="40e64-376">Windows EventLog provider</span></span>

<span data-ttu-id="40e64-377">O pacote de provedor [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) envia a saída de log para o Log de Eventos do Windows.</span><span class="sxs-lookup"><span data-stu-id="40e64-377">The [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) provider package sends log output to the Windows Event Log.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddEventLog()
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddEventLog()
```

<span data-ttu-id="40e64-378">As [sobrecargas de AddEventLog](/dotnet/api/microsoft.extensions.logging.eventloggerfactoryextensions) permitem que você passe `EventLogSettings` ou um nível de log mínimo.</span><span class="sxs-lookup"><span data-stu-id="40e64-378">[AddEventLog overloads](/dotnet/api/microsoft.extensions.logging.eventloggerfactoryextensions) let you pass in `EventLogSettings` or a minimum log level.</span></span>

::: moniker-end

### <a name="tracesource-provider"></a><span data-ttu-id="40e64-379">Provedor TraceSource</span><span class="sxs-lookup"><span data-stu-id="40e64-379">TraceSource provider</span></span>

<span data-ttu-id="40e64-380">O pacote de provedor [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) usa as bibliotecas e provedores de [System.Diagnostics.TraceSource](/dotnet/api/system.diagnostics.tracesource).</span><span class="sxs-lookup"><span data-stu-id="40e64-380">The [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) provider package uses the [System.Diagnostics.TraceSource](/dotnet/api/system.diagnostics.tracesource) libraries and providers.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddTraceSource(sourceSwitchName);
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddTraceSource(sourceSwitchName);
```

::: moniker-end

<span data-ttu-id="40e64-381">As [sobrecargas de AddTraceSource](/dotnet/api/microsoft.extensions.logging.tracesourcefactoryextensions) permitem que você passe um comutador de fonte e um ouvinte de rastreamento.</span><span class="sxs-lookup"><span data-stu-id="40e64-381">[AddTraceSource overloads](/dotnet/api/microsoft.extensions.logging.tracesourcefactoryextensions) let you pass in a source switch and a trace listener.</span></span>

<span data-ttu-id="40e64-382">Para usar esse provedor, o aplicativo deve ser executado no .NET Framework (em vez do .NET Core).</span><span class="sxs-lookup"><span data-stu-id="40e64-382">To use this provider, an application has to run on the .NET Framework (rather than .NET Core).</span></span> <span data-ttu-id="40e64-383">O provedor permite rotear mensagens a uma variedade de [ouvintes](/dotnet/framework/debug-trace-profile/trace-listeners), como o [TextWriterTraceListener](/dotnet/api/system.diagnostics.textwritertracelistenerr) usado no aplicativo de exemplo.</span><span class="sxs-lookup"><span data-stu-id="40e64-383">The provider lets you route messages to a variety of [listeners](/dotnet/framework/debug-trace-profile/trace-listeners), such as the [TextWriterTraceListener](/dotnet/api/system.diagnostics.textwritertracelistenerr) used in the sample application.</span></span>

<span data-ttu-id="40e64-384">O exemplo a seguir configura um provedor `TraceSource` que registra mensagens `Warning` e superiores na janela de console.</span><span class="sxs-lookup"><span data-stu-id="40e64-384">The following example configures a `TraceSource` provider that logs `Warning` and higher messages to the console window.</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_TraceSource&highlight=9-12)]

### <a name="azure-app-service-provider"></a><span data-ttu-id="40e64-385">Provedor do Serviço de Aplicativo do Azure</span><span class="sxs-lookup"><span data-stu-id="40e64-385">Azure App Service provider</span></span>

<span data-ttu-id="40e64-386">O pacote de provedor [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) grava logs em arquivos de texto no sistema de arquivos de um aplicativo do Serviço de Aplicativo do Azure e no [armazenamento de blobs](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) em uma conta de Armazenamento do Azure.</span><span class="sxs-lookup"><span data-stu-id="40e64-386">The [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) provider package writes logs to text files in an Azure App Service app's file system and to [blob storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) in an Azure Storage account.</span></span> <span data-ttu-id="40e64-387">O provedor está disponível somente para aplicativos que se destinam ao ASP.NET Core 1.1 ou superior.</span><span class="sxs-lookup"><span data-stu-id="40e64-387">The provider is available only for apps that target ASP.NET Core 1.1 or later.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="40e64-388">Se o destino for o .NET Core, não instale o pacote de provedor ou chame explicitamente [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics).</span><span class="sxs-lookup"><span data-stu-id="40e64-388">If targeting .NET Core, don't install the provider package or explicitly call [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics).</span></span> <span data-ttu-id="40e64-389">O provedor fica automaticamente disponível para o aplicativo quando o aplicativo for implantado no Serviço de Aplicativo do Azure.</span><span class="sxs-lookup"><span data-stu-id="40e64-389">The provider is automatically available to the app when the app is deployed to Azure App Service.</span></span>

<span data-ttu-id="40e64-390">Se o destino for o .NET Framework, adicione o pacote de provedor ao projeto e invoque `AddAzureWebAppDiagnostics`:</span><span class="sxs-lookup"><span data-stu-id="40e64-390">If targeting .NET Framework, add the provider package to the project and invoke `AddAzureWebAppDiagnostics`:</span></span>

```csharp
logging.AddAzureWebAppDiagnostics();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddAzureWebAppDiagnostics();
```

<span data-ttu-id="40e64-391">Uma sobrecarga do [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) permite que você passe no [AzureAppServicesDiagnosticsSettings](/dotnet/api/microsoft.extensions.logging.azureappservices.azureappservicesdiagnosticssettings) com a qual você pode substituir as configurações padrão, como o modelo de saída de registro em log, o nome do blob e o limite do tamanho do arquivo.</span><span class="sxs-lookup"><span data-stu-id="40e64-391">An [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) overload lets you pass in [AzureAppServicesDiagnosticsSettings](/dotnet/api/microsoft.extensions.logging.azureappservices.azureappservicesdiagnosticssettings) with which you can override default settings such as the logging output template, blob name, and file size limit.</span></span> <span data-ttu-id="40e64-392">(O *modelo Output* é um modelo de mensagem que é aplicado a todos os logs, sobre aquele que você fornece ao chamar um método `ILogger`).</span><span class="sxs-lookup"><span data-stu-id="40e64-392">(*Output template* is a message template that's applied to all logs on top of the one that you provide when you call an `ILogger` method.)</span></span>

::: moniker-end

<span data-ttu-id="40e64-393">Ao implantar um aplicativo do Serviço de Aplicativo, o aplicativo respeitará as configurações na seção [Logs de Diagnóstico](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag) da página **Serviço de Aplicativo** do Portal do Azure.</span><span class="sxs-lookup"><span data-stu-id="40e64-393">When you deploy to an App Service app, the app honors the settings in the [Diagnostic Logs](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag) section of the **App Service** page of the Azure portal.</span></span> <span data-ttu-id="40e64-394">Quando essas configurações são atualizadas, as alterações entram em vigor imediatamente sem a necessidade de uma reinicialização ou reimplantação do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="40e64-394">When these settings are updated, the changes take effect immediately without requiring a restart or redeployment of the app.</span></span>

![Configurações de registro em log do Azure](index/_static/azure-logging-settings.png)

<span data-ttu-id="40e64-396">O local padrão para arquivos de log é na pasta *D:\\home\\LogFiles\\Application* e o nome de arquivo padrão é *diagnostics-aaaammdd.txt*.</span><span class="sxs-lookup"><span data-stu-id="40e64-396">The default location for log files is in the *D:\\home\\LogFiles\\Application* folder, and the default file name is *diagnostics-yyyymmdd.txt*.</span></span> <span data-ttu-id="40e64-397">O limite padrão de tamanho do arquivo é 10 MB e o número padrão máximo de arquivos mantidos é 2.</span><span class="sxs-lookup"><span data-stu-id="40e64-397">The default file size limit is 10 MB, and the default maximum number of files retained is 2.</span></span> <span data-ttu-id="40e64-398">O nome de blob padrão é *{app-name}{timestamp}/aaaa/mm/dd/hh/{guid}-applicationLog.txt*.</span><span class="sxs-lookup"><span data-stu-id="40e64-398">The default blob name is *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*.</span></span> <span data-ttu-id="40e64-399">Para obter mais informações sobre o comportamento padrão, consulte [AzureAppServicesDiagnosticsSettings](/dotnet/api/microsoft.extensions.logging.azureappservices.azureappservicesdiagnosticssettings).</span><span class="sxs-lookup"><span data-stu-id="40e64-399">For more information about default behavior, see [AzureAppServicesDiagnosticsSettings](/dotnet/api/microsoft.extensions.logging.azureappservices.azureappservicesdiagnosticssettings).</span></span>

<span data-ttu-id="40e64-400">O provedor funciona somente quando o projeto é executado no ambiente do Azure.</span><span class="sxs-lookup"><span data-stu-id="40e64-400">The provider only works when the project runs in the Azure environment.</span></span> <span data-ttu-id="40e64-401">Ele não tem nenhum efeito quando o projeto é executado localmente&mdash;ele não grava em arquivos locais ou no armazenamento de desenvolvimento local para blobs.</span><span class="sxs-lookup"><span data-stu-id="40e64-401">It has no effect when the project is run locally&mdash;it doesn't write to local files or local development storage for blobs.</span></span>

## <a name="third-party-logging-providers"></a><span data-ttu-id="40e64-402">Provedores de log de terceiros</span><span class="sxs-lookup"><span data-stu-id="40e64-402">Third-party logging providers</span></span>

<span data-ttu-id="40e64-403">Estruturas de log de terceiros que funcionam com o ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="40e64-403">Third-party logging frameworks that work with ASP.NET Core:</span></span>

* <span data-ttu-id="40e64-404">[elmah.io](https://elmah.io/) ([repositório GitHub](https://github.com/elmahio/Elmah.Io.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="40e64-404">[elmah.io](https://elmah.io/) ([GitHub repo](https://github.com/elmahio/Elmah.Io.Extensions.Logging))</span></span>
* <span data-ttu-id="40e64-405">[Gelf](http://docs.graylog.org/en/2.3/pages/gelf.html) ([repositório do GitHub](https://github.com/mattwcole/gelf-extensions-logging))</span><span class="sxs-lookup"><span data-stu-id="40e64-405">[Gelf](http://docs.graylog.org/en/2.3/pages/gelf.html) ([GitHub repo](https://github.com/mattwcole/gelf-extensions-logging))</span></span>
* <span data-ttu-id="40e64-406">[JSNLog](http://jsnlog.com/) ([repositório GitHub](https://github.com/mperdeck/jsnlog))</span><span class="sxs-lookup"><span data-stu-id="40e64-406">[JSNLog](http://jsnlog.com/) ([GitHub repo](https://github.com/mperdeck/jsnlog))</span></span>
* <span data-ttu-id="40e64-407">[Loggr](http://loggr.net/) ([repositório GitHub](https://github.com/imobile3/Loggr.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="40e64-407">[Loggr](http://loggr.net/) ([GitHub repo](https://github.com/imobile3/Loggr.Extensions.Logging))</span></span>
* <span data-ttu-id="40e64-408">[NLog](http://nlog-project.org/) ([repositório GitHub](https://github.com/NLog/NLog.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="40e64-408">[NLog](http://nlog-project.org/) ([GitHub repo](https://github.com/NLog/NLog.Extensions.Logging))</span></span>
* <span data-ttu-id="40e64-409">[Serilog](https://serilog.net/) ([repositório GitHub](https://github.com/serilog/serilog-extensions-logging))</span><span class="sxs-lookup"><span data-stu-id="40e64-409">[Serilog](https://serilog.net/) ([GitHub repo](https://github.com/serilog/serilog-extensions-logging))</span></span>

<span data-ttu-id="40e64-410">Algumas estruturas de terceiros podem fazer o [log semântico, também conhecido como registro em log estruturado](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="40e64-410">Some third-party frameworks can perform [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="40e64-411">Usar uma estrutura de terceiros é semelhante ao uso de um dos provedores internos:</span><span class="sxs-lookup"><span data-stu-id="40e64-411">Using a third-party framework is similar to using one of the built-in providers:</span></span>

1. <span data-ttu-id="40e64-412">Adicione um pacote NuGet ao projeto.</span><span class="sxs-lookup"><span data-stu-id="40e64-412">Add a NuGet package to your project.</span></span>
1. <span data-ttu-id="40e64-413">Chame um método de extensão em `ILoggerFactory`.</span><span class="sxs-lookup"><span data-stu-id="40e64-413">Call an extension method on `ILoggerFactory`.</span></span>

<span data-ttu-id="40e64-414">Para obter mais informações, consulte a documentação de cada estrutura.</span><span class="sxs-lookup"><span data-stu-id="40e64-414">For more information, see each framework's documentation.</span></span>

## <a name="azure-log-streaming"></a><span data-ttu-id="40e64-415">Fluxo de log do Azure</span><span class="sxs-lookup"><span data-stu-id="40e64-415">Azure log streaming</span></span>

<span data-ttu-id="40e64-416">O fluxo de log do Azure permite que você exiba a atividade de log em tempo real:</span><span class="sxs-lookup"><span data-stu-id="40e64-416">Azure log streaming enables you to view log activity in real time from:</span></span> 

* <span data-ttu-id="40e64-417">Do servidor de aplicativos</span><span class="sxs-lookup"><span data-stu-id="40e64-417">The application server</span></span>
* <span data-ttu-id="40e64-418">Do servidor Web</span><span class="sxs-lookup"><span data-stu-id="40e64-418">The web server</span></span>
* <span data-ttu-id="40e64-419">De uma solicitação de rastreio com falha</span><span class="sxs-lookup"><span data-stu-id="40e64-419">Failed request tracing</span></span>

<span data-ttu-id="40e64-420">Para configurar o fluxo de log do Azure:</span><span class="sxs-lookup"><span data-stu-id="40e64-420">To configure Azure log streaming:</span></span>

* <span data-ttu-id="40e64-421">Navegue até a página **Logs de Diagnóstico** da página do portal do seu aplicativo</span><span class="sxs-lookup"><span data-stu-id="40e64-421">Navigate to the **Diagnostics Logs** page from your application's portal page</span></span>
* <span data-ttu-id="40e64-422">Defina o **Log de aplicativo (Sistema de Arquivos)** como ativado.</span><span class="sxs-lookup"><span data-stu-id="40e64-422">Set **Application Logging (Filesystem)** to on.</span></span>

![Página de logs de diagnóstico do Portal do Azure](index/_static/azure-diagnostic-logs.png)

<span data-ttu-id="40e64-424">Navegue até a página **Fluxo de Log** para exibir as mensagens de aplicativo.</span><span class="sxs-lookup"><span data-stu-id="40e64-424">Navigate to the **Log Streaming** page to view application messages.</span></span> <span data-ttu-id="40e64-425">Elas são registradas pelo aplicativo por meio da interface `ILogger`.</span><span class="sxs-lookup"><span data-stu-id="40e64-425">They're logged by application through the `ILogger` interface.</span></span>

![Fluxo de log do aplicativo do Portal do Azure](index/_static/azure-log-streaming.png)

## <a name="azure-application-insights-trace-logging"></a><span data-ttu-id="40e64-427">Log de rastreamento do Azure Application Insights</span><span class="sxs-lookup"><span data-stu-id="40e64-427">Azure Application Insights trace logging</span></span>

<span data-ttu-id="40e64-428">O SDK do [Application Insights](https://azure.microsoft.com/services/application-insights/) é capaz de coletar telemetria de rastreamento de logs gerados por meio de infraestrutura de log do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="40e64-428">The [Application Insights](https://azure.microsoft.com/services/application-insights/) SDK is capable of collecting trace telemetry from logs generated via the ASP.NET Core logging infrastructure.</span></span> <span data-ttu-id="40e64-429">Para obter mais informações, veja [Wiki do Microsoft/ApplicationInsights-aspnetcore: Log](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Logging).</span><span class="sxs-lookup"><span data-stu-id="40e64-429">For more information, see [Microsoft/ApplicationInsights-aspnetcore Wiki: Logging](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Logging).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="40e64-430">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="40e64-430">Additional resources</span></span>

[<span data-ttu-id="40e64-431">Registro em log de alto desempenho com LoggerMessage</span><span class="sxs-lookup"><span data-stu-id="40e64-431">High-performance logging with LoggerMessage</span></span>](xref:fundamentals/logging/loggermessage)

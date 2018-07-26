---
title: Registro em log no ASP.NET Core
author: ardalis
description: Saiba mais sobre a estrutura de registros no ASP.NET Core. Descubra os provedores de log internos e saiba mais sobre os provedores de terceiros populares.
ms.author: tdykstra
ms.date: 07/24/2018
uid: fundamentals/logging/index
ms.openlocfilehash: 0181566aeab1fa055435ac90887c019eef52878c
ms.sourcegitcommit: b4c7b1a4c48dec0865f27874275c73da1f75e918
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2018
ms.locfileid: "39228631"
---
# <a name="logging-in-aspnet-core"></a><span data-ttu-id="37edc-104">Registro em log no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="37edc-104">Logging in ASP.NET Core</span></span>

<span data-ttu-id="37edc-105">Por [Steve Smith](https://ardalis.com/) e [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="37edc-105">By [Steve Smith](https://ardalis.com/) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="37edc-106">O ASP.NET Core dá suporte a uma API de registro em log que funciona com uma variedade de provedores de logs.</span><span class="sxs-lookup"><span data-stu-id="37edc-106">ASP.NET Core supports a logging API that works with a variety of logging providers.</span></span> <span data-ttu-id="37edc-107">Os provedores internos permitem que você envie logs para um ou mais destinos e você pode se conectar a uma estrutura de registros de terceiros.</span><span class="sxs-lookup"><span data-stu-id="37edc-107">Built-in providers let you send logs to one or more destinations, and you can plug in a third-party logging framework.</span></span> <span data-ttu-id="37edc-108">Este artigo mostra como usar a API de registro em log e os provedores internos em seu código.</span><span class="sxs-lookup"><span data-stu-id="37edc-108">This article shows how to use the built-in logging API and providers in your code.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="37edc-109">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample2) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="37edc-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample2) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="37edc-110">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="37edc-110">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

::: moniker-end

<span data-ttu-id="37edc-111">Para obter informações sobre o registro stdout ao hospedar com IIS, confira <xref:host-and-deploy/iis/troubleshoot#aspnet-core-module-stdout-log>.</span><span class="sxs-lookup"><span data-stu-id="37edc-111">For information on stdout logging when hosting with IIS, see <xref:host-and-deploy/iis/troubleshoot#aspnet-core-module-stdout-log>.</span></span> <span data-ttu-id="37edc-112">Para obter informações sobre o registro stdout com o Serviço de Aplicativo do Azure, confira <xref:host-and-deploy/azure-apps/troubleshoot#aspnet-core-module-stdout-log>.</span><span class="sxs-lookup"><span data-stu-id="37edc-112">For information on stdout logging with Azure App Service, see <xref:host-and-deploy/azure-apps/troubleshoot#aspnet-core-module-stdout-log>.</span></span>

## <a name="how-to-create-logs"></a><span data-ttu-id="37edc-113">Como criar logs</span><span class="sxs-lookup"><span data-stu-id="37edc-113">How to create logs</span></span>

<span data-ttu-id="37edc-114">Para criar logs, implemente um objeto [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) do contêiner de [injeção de dependência](xref:fundamentals/dependency-injection):</span><span class="sxs-lookup"><span data-stu-id="37edc-114">To create logs, implement an [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) object from the [dependency injection](xref:fundamentals/dependency-injection) container:</span></span>

[!code-csharp[](index/sample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

<span data-ttu-id="37edc-115">Em seguida, chame métodos de registro em log nesse objeto de agente:</span><span class="sxs-lookup"><span data-stu-id="37edc-115">Then call logging methods on that logger object:</span></span>

[!code-csharp[](index/sample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="37edc-116">Este exemplo cria logs com a classe `TodoController` como a *categoria*.</span><span class="sxs-lookup"><span data-stu-id="37edc-116">This example creates logs with the `TodoController` class as the *category*.</span></span> <span data-ttu-id="37edc-117">As categorias serão explicadas [posteriormente neste artigo](#log-category).</span><span class="sxs-lookup"><span data-stu-id="37edc-117">Categories are explained [later in this article](#log-category).</span></span>

<span data-ttu-id="37edc-118">O ASP.NET Core não fornece métodos de agente assíncronos porque o registro em log deve ser tão rápido que não valeria a pena usar assíncronos.</span><span class="sxs-lookup"><span data-stu-id="37edc-118">ASP.NET Core doesn't provide async logger methods because logging should be so fast that it isn't worth the cost of using async.</span></span> <span data-ttu-id="37edc-119">Se você estiver em uma situação em que isso não é verdadeiro, considere alterar a maneira como você faz logs.</span><span class="sxs-lookup"><span data-stu-id="37edc-119">If you're in a situation where that's not true, consider changing the way you log.</span></span> <span data-ttu-id="37edc-120">Se seu armazenamento de dados é lento, grave as mensagens de log em um repositório rápido primeiro e, depois, mova-as para um repositório lento.</span><span class="sxs-lookup"><span data-stu-id="37edc-120">If your data store is slow, write the log messages to a fast store first, then move them to a slow store later.</span></span> <span data-ttu-id="37edc-121">Por exemplo, registre em uma fila de mensagens que seja lida e persistida em um armazenamento lento por outro processo.</span><span class="sxs-lookup"><span data-stu-id="37edc-121">For example, log to a message queue that's read and persisted to slow storage by another process.</span></span>

## <a name="how-to-add-providers"></a><span data-ttu-id="37edc-122">Como adicionar provedores</span><span class="sxs-lookup"><span data-stu-id="37edc-122">How to add providers</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="37edc-123">Um provedor de logs obtém as mensagens que você cria com um objeto `ILogger` e as exibe ou armazena.</span><span class="sxs-lookup"><span data-stu-id="37edc-123">A logging provider takes the messages that you create with an `ILogger` object and displays or stores them.</span></span> <span data-ttu-id="37edc-124">Por exemplo, o provedor Console exibe as mensagens no console e o provedor do Serviço de Aplicativo do Azure pode armazená-las no armazenamento de blobs do Azure.</span><span class="sxs-lookup"><span data-stu-id="37edc-124">For example, the Console provider displays messages on the console, and the Azure App Service provider can store them in Azure blob storage.</span></span>

<span data-ttu-id="37edc-125">Para usar um provedor, chame o método de extensão `Add<ProviderName>` do provedor no *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="37edc-125">To use a provider, call the provider's `Add<ProviderName>` extension method in *Program.cs*:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_ExpandDefault&highlight=16,17)]

<span data-ttu-id="37edc-126">O modelo de projeto padrão permite o registro em log com o método [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder?view=aspnetcore-2.0#Microsoft_AspNetCore_WebHost_CreateDefaultBuilder_System_String___):</span><span class="sxs-lookup"><span data-stu-id="37edc-126">The default project template enables logging with the [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder?view=aspnetcore-2.0#Microsoft_AspNetCore_WebHost_CreateDefaultBuilder_System_String___) method:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_TemplateCode&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="37edc-127">Um provedor de logs obtém as mensagens que você cria com um objeto `ILogger` e as exibe ou armazena.</span><span class="sxs-lookup"><span data-stu-id="37edc-127">A logging provider takes the messages that you create with an `ILogger` object and displays or stores them.</span></span> <span data-ttu-id="37edc-128">Por exemplo, o provedor Console exibe as mensagens no console e o provedor do Serviço de Aplicativo do Azure pode armazená-las no armazenamento de blobs do Azure.</span><span class="sxs-lookup"><span data-stu-id="37edc-128">For example, the Console provider displays messages on the console, and the Azure App Service provider can store them in Azure blob storage.</span></span>

<span data-ttu-id="37edc-129">Para usar um provedor, instale o pacote NuGet e chame o método de extensão do provedor em uma instância do [ILoggerFactory](/dotnet/api/microsoft.extensions.logging.iloggerfactory), conforme mostrado no exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="37edc-129">To use a provider, install its NuGet package and call the provider's extension method on an instance of [ILoggerFactory](/dotnet/api/microsoft.extensions.logging.iloggerfactory), as shown in the following example:</span></span>

[!code-csharp[](index/sample//Startup.cs?name=snippet_AddConsoleAndDebug&highlight=3,5-7)]

<span data-ttu-id="37edc-130">A [DI](xref:fundamentals/dependency-injection) (injeção de dependência) do ASP.NET Core fornece a instância de `ILoggerFactory`.</span><span class="sxs-lookup"><span data-stu-id="37edc-130">ASP.NET Core [dependency injection](xref:fundamentals/dependency-injection) (DI) provides the `ILoggerFactory` instance.</span></span> <span data-ttu-id="37edc-131">Os métodos de extensão `AddConsole` e `AddDebug` são definidos nos pacotes [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) e [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/).</span><span class="sxs-lookup"><span data-stu-id="37edc-131">The `AddConsole` and `AddDebug` extension methods are defined in the [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) and [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/) packages.</span></span> <span data-ttu-id="37edc-132">Cada método de extensão chama o método `ILoggerFactory.AddProvider`, passando uma instância do provedor.</span><span class="sxs-lookup"><span data-stu-id="37edc-132">Each extension method calls the `ILoggerFactory.AddProvider` method, passing in an instance of the provider.</span></span>

> [!NOTE]
> <span data-ttu-id="37edc-133">O [aplicativo de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample) adiciona provedores de log no método `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="37edc-133">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample) adds logging providers in the `Startup.Configure` method.</span></span> <span data-ttu-id="37edc-134">Se você quiser obter saída de log do código que é executado anteriormente, adicione provedores de log no construtor de classe `Startup`.</span><span class="sxs-lookup"><span data-stu-id="37edc-134">If you want to obtain log output from code that executes earlier, add logging providers in the `Startup` class constructor.</span></span>

::: moniker-end

<span data-ttu-id="37edc-135">Você encontrará informações sobre cada [provedor de log interno](#built-in-logging-providers) e links para [provedores de log de terceiros](#third-party-logging-providers) mais adiante no artigo.</span><span class="sxs-lookup"><span data-stu-id="37edc-135">You'll find information about each [built-in logging provider](#built-in-logging-providers) and links to [third-party logging providers](#third-party-logging-providers) later in the article.</span></span>

## <a name="settings-file-configuration"></a><span data-ttu-id="37edc-136">Definições do arquivo de configurações</span><span class="sxs-lookup"><span data-stu-id="37edc-136">Settings file configuration</span></span>

<span data-ttu-id="37edc-137">Cada um dos exemplos anteriores na seção [Como adicionar provedores](#how-to-add-providers) carrega a configuração do provedor de logs da seção `Logging` dos arquivos de configurações do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="37edc-137">Each of the preceding examples in the [How to add providers](#how-to-add-providers) section loads logging provider configuration from the `Logging` section of app settings files.</span></span> <span data-ttu-id="37edc-138">O exemplo a seguir mostra o conteúdo de um típico arquivo *appsettings.Development.json*:</span><span class="sxs-lookup"><span data-stu-id="37edc-138">The following example shows the contents of a typical *appsettings.Development.json* file:</span></span>

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

<span data-ttu-id="37edc-139">Chaves `LogLevel` representam nomes de log.</span><span class="sxs-lookup"><span data-stu-id="37edc-139">`LogLevel` keys represent log names.</span></span> <span data-ttu-id="37edc-140">A chave `Default` aplica-se a logs não listados de forma explícita.</span><span class="sxs-lookup"><span data-stu-id="37edc-140">The `Default` key applies to logs not explicitly listed.</span></span> <span data-ttu-id="37edc-141">O valor representa o [nível de log](#log-level) aplicado ao log fornecido.</span><span class="sxs-lookup"><span data-stu-id="37edc-141">The value represents the [log level](#log-level) applied to the given log.</span></span> <span data-ttu-id="37edc-142">Chaves de log que definem `IncludeScopes` (`Console` no exemplo) especificam se os [escopos de log](#log-scopes) estão habilitados para o log indicado.</span><span class="sxs-lookup"><span data-stu-id="37edc-142">Log keys that set `IncludeScopes` (`Console` in the example), specify if [log scopes](#log-scopes) are enabled for the indicated log.</span></span>

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

<span data-ttu-id="37edc-143">Chaves `LogLevel` representam nomes de log.</span><span class="sxs-lookup"><span data-stu-id="37edc-143">`LogLevel` keys represent log names.</span></span> <span data-ttu-id="37edc-144">A chave `Default` aplica-se a logs não listados de forma explícita.</span><span class="sxs-lookup"><span data-stu-id="37edc-144">The `Default` key applies to logs not explicitly listed.</span></span> <span data-ttu-id="37edc-145">O valor representa o [nível de log](#log-level) aplicado ao log fornecido.</span><span class="sxs-lookup"><span data-stu-id="37edc-145">The value represents the [log level](#log-level) applied to the given log.</span></span>

::: moniker-end

## <a name="sample-logging-output"></a><span data-ttu-id="37edc-146">Exemplo de saída de registro em log</span><span class="sxs-lookup"><span data-stu-id="37edc-146">Sample logging output</span></span>

<span data-ttu-id="37edc-147">Com o código de exemplo mostrado na seção anterior, você verá logs no console ao executar através da linha de comando.</span><span class="sxs-lookup"><span data-stu-id="37edc-147">With the sample code shown in the preceding section, you'll see logs in the console when you run from the command line.</span></span> <span data-ttu-id="37edc-148">Aqui está um exemplo da saída do console:</span><span class="sxs-lookup"><span data-stu-id="37edc-148">Here's an example of console output:</span></span>

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

<span data-ttu-id="37edc-149">Esses logs foram criados acessando `http://localhost:5000/api/todo/0`, que dispara a execução das duas chamadas `ILogger` mostradas na seção anterior.</span><span class="sxs-lookup"><span data-stu-id="37edc-149">These logs were created by going to `http://localhost:5000/api/todo/0`, which triggers execution of both `ILogger` calls shown in the preceding section.</span></span>

<span data-ttu-id="37edc-150">Aqui está um exemplo de como os mesmos logs aparecem na janela Depuração quando você executa o aplicativo de exemplo no Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="37edc-150">Here's an example of the same logs as they appear in the Debug window when you run the sample application in Visual Studio:</span></span>

```console
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request starting HTTP/1.1 GET http://localhost:53104/api/todo/0  
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
TodoApi.Controllers.TodoController:Information: Getting item 0
TodoApi.Controllers.TodoController:Warning: GetById(0) NOT FOUND
Microsoft.AspNetCore.Mvc.StatusCodeResult:Information: Executing HttpStatusCodeResult, setting HTTP status code 404
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 152.5657ms
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request finished in 316.3195ms 404
```

<span data-ttu-id="37edc-151">Os logs criados pelas chamadas `ILogger` mostradas na seção anterior começam com "TodoApi.Controllers.TodoController".</span><span class="sxs-lookup"><span data-stu-id="37edc-151">The logs that were created by the `ILogger` calls shown in the preceding section begin with "TodoApi.Controllers.TodoController".</span></span> <span data-ttu-id="37edc-152">Os logs que começam com categorias "Microsoft" são do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="37edc-152">The logs that begin with "Microsoft" categories are from ASP.NET Core.</span></span> <span data-ttu-id="37edc-153">O próprio ASP.NET Core e o código do aplicativo estão usando a mesma API de registro em log e os mesmos provedores de log.</span><span class="sxs-lookup"><span data-stu-id="37edc-153">ASP.NET Core itself and your application code are using the same logging API and the same logging providers.</span></span>

<span data-ttu-id="37edc-154">O restante deste artigo explica alguns detalhes e opções para registro em log.</span><span class="sxs-lookup"><span data-stu-id="37edc-154">The remainder of this article explains some details and options for logging.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="37edc-155">Pacotes NuGet</span><span class="sxs-lookup"><span data-stu-id="37edc-155">NuGet packages</span></span>

<span data-ttu-id="37edc-156">As interfaces `ILogger` e `ILoggerFactory` estão em [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/) e as implementações padrão para elas estão em [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).</span><span class="sxs-lookup"><span data-stu-id="37edc-156">The `ILogger` and `ILoggerFactory` interfaces are in [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), and default implementations for them are in [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).</span></span>

## <a name="log-category"></a><span data-ttu-id="37edc-157">Categoria de log</span><span class="sxs-lookup"><span data-stu-id="37edc-157">Log category</span></span>

<span data-ttu-id="37edc-158">Um *categoria* é incluída com cada log que você cria.</span><span class="sxs-lookup"><span data-stu-id="37edc-158">A *category* is included with each log that you create.</span></span> <span data-ttu-id="37edc-159">Você especifica a categoria ao criar um objeto `ILogger`.</span><span class="sxs-lookup"><span data-stu-id="37edc-159">You specify the category when you create an `ILogger` object.</span></span> <span data-ttu-id="37edc-160">A categoria pode ser qualquer cadeia de caracteres, mas uma convenção é usar o nome totalmente qualificado da classe da qual os logs são gravados.</span><span class="sxs-lookup"><span data-stu-id="37edc-160">The category may be any string, but a convention is to use the fully qualified name of the class from which the logs are written.</span></span> <span data-ttu-id="37edc-161">Por exemplo: "TodoApi.Controllers.TodoController".</span><span class="sxs-lookup"><span data-stu-id="37edc-161">For example: "TodoApi.Controllers.TodoController".</span></span>

<span data-ttu-id="37edc-162">Você pode especificar a categoria como uma cadeia de caracteres ou usar um método de extensão que deriva a categoria do tipo.</span><span class="sxs-lookup"><span data-stu-id="37edc-162">You can specify the category as a string or use an extension method that derives the category from the type.</span></span> <span data-ttu-id="37edc-163">Para especificar a categoria como uma cadeia de caracteres, chame `CreateLogger` em uma instância de `ILoggerFactory`, conforme mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="37edc-163">To specify the category as a string, call `CreateLogger` on an `ILoggerFactory` instance, as shown below.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

<span data-ttu-id="37edc-164">Na maioria das vezes será mais fácil usar `ILogger<T>`, conforme mostrado no exemplo a seguir.</span><span class="sxs-lookup"><span data-stu-id="37edc-164">Most of the time, it will be easier to use `ILogger<T>`, as in the following example.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

<span data-ttu-id="37edc-165">Isso é equivalente a chamar `CreateLogger` com o nome de tipo totalmente qualificado de `T`.</span><span class="sxs-lookup"><span data-stu-id="37edc-165">This is equivalent to calling `CreateLogger` with the fully qualified type name of `T`.</span></span>

## <a name="log-level"></a><span data-ttu-id="37edc-166">Nível de log</span><span class="sxs-lookup"><span data-stu-id="37edc-166">Log level</span></span>

<span data-ttu-id="37edc-167">Sempre que você grava um log, você especifica o [LogLevel](/dotnet/api/microsoft.extensions.logging.logLevel).</span><span class="sxs-lookup"><span data-stu-id="37edc-167">Each time you write a log, you specify its [LogLevel](/dotnet/api/microsoft.extensions.logging.logLevel).</span></span> <span data-ttu-id="37edc-168">O nível de log indica o grau de gravidade ou importância.</span><span class="sxs-lookup"><span data-stu-id="37edc-168">The log level indicates the degree of severity or importance.</span></span> <span data-ttu-id="37edc-169">Por exemplo, você pode gravar um log `Information` quando um método é finalizado normalmente, um log `Warning` quando um método retorna um código de retorno 404 e um log `Error` ao capturar uma exceção inesperada.</span><span class="sxs-lookup"><span data-stu-id="37edc-169">For example, you might write an `Information` log when a method ends normally, a `Warning` log when a method returns a 404 return code, and an `Error` log when you catch an unexpected exception.</span></span>

<span data-ttu-id="37edc-170">No exemplo de código a seguir, os nomes dos métodos (por exemplo, `LogWarning`) especificam o nível de log.</span><span class="sxs-lookup"><span data-stu-id="37edc-170">In the following code example, the names of the methods (for example, `LogWarning`) specify the log level.</span></span> <span data-ttu-id="37edc-171">O primeiro parâmetro é a [ID de evento de log](#log-event-id).</span><span class="sxs-lookup"><span data-stu-id="37edc-171">The first parameter is the [Log event ID](#log-event-id).</span></span> <span data-ttu-id="37edc-172">O segundo parâmetro é um [modelo de mensagem](#log-message-template) com espaços reservados para valores de argumento fornecidos pelos parâmetros de método restantes.</span><span class="sxs-lookup"><span data-stu-id="37edc-172">The second parameter is a [message template](#log-message-template) with placeholders for argument values provided by the remaining method parameters.</span></span> <span data-ttu-id="37edc-173">Os parâmetros de método serão explicados com mais detalhes posteriormente neste artigo.</span><span class="sxs-lookup"><span data-stu-id="37edc-173">The method parameters are explained in more detail later in this article.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="37edc-174">Os métodos de log que incluem o nível no nome do método são [métodos de extensão para ILogger](/dotnet/api/microsoft.extensions.logging.loggerextensions).</span><span class="sxs-lookup"><span data-stu-id="37edc-174">Log methods that include the level in the method name are [extension methods for ILogger](/dotnet/api/microsoft.extensions.logging.loggerextensions).</span></span> <span data-ttu-id="37edc-175">Nos bastidores, esses métodos chamam um método `Log` que recebe um parâmetro `LogLevel`.</span><span class="sxs-lookup"><span data-stu-id="37edc-175">Behind the scenes, these methods call a `Log` method that takes a `LogLevel` parameter.</span></span> <span data-ttu-id="37edc-176">Você pode chamar o método `Log` diretamente em vez de um desses métodos de extensão, mas a sintaxe é relativamente complicada.</span><span class="sxs-lookup"><span data-stu-id="37edc-176">You can call the `Log` method directly rather than one of these extension methods, but the syntax is relatively complicated.</span></span> <span data-ttu-id="37edc-177">Para obter mais informações, consulte a [interface ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) e o [código-fonte de extensões de agente](https://github.com/aspnet/Logging/blob/master/src/Microsoft.Extensions.Logging.Abstractions/LoggerExtensions.cs).</span><span class="sxs-lookup"><span data-stu-id="37edc-177">For more information, see the [ILogger interface](/dotnet/api/microsoft.extensions.logging.ilogger) and the [logger extensions source code](https://github.com/aspnet/Logging/blob/master/src/Microsoft.Extensions.Logging.Abstractions/LoggerExtensions.cs).</span></span>

<span data-ttu-id="37edc-178">O ASP.NET Core define os seguintes [níveis de log](/dotnet/api/microsoft.extensions.logging.loglevel), ordenados aqui da menor para a maior gravidade.</span><span class="sxs-lookup"><span data-stu-id="37edc-178">ASP.NET Core defines the following [log levels](/dotnet/api/microsoft.extensions.logging.loglevel), ordered here from least to highest severity.</span></span>

* <span data-ttu-id="37edc-179">Trace = 0</span><span class="sxs-lookup"><span data-stu-id="37edc-179">Trace = 0</span></span>

  <span data-ttu-id="37edc-180">Para obter informações valiosas somente para um desenvolvedor que esteja depurando um problema.</span><span class="sxs-lookup"><span data-stu-id="37edc-180">For information that's valuable only to a developer debugging an issue.</span></span> <span data-ttu-id="37edc-181">Essas mensagens podem conter dados confidenciais de aplicativos e, portanto, não devem ser habilitadas em um ambiente de produção.</span><span class="sxs-lookup"><span data-stu-id="37edc-181">These messages may contain sensitive application data and so shouldn't be enabled in a production environment.</span></span> <span data-ttu-id="37edc-182">*Desabilitado por padrão.*</span><span class="sxs-lookup"><span data-stu-id="37edc-182">*Disabled by default.*</span></span> <span data-ttu-id="37edc-183">Exemplo: `Credentials: {"User":"someuser", "Password":"P@ssword"}`</span><span class="sxs-lookup"><span data-stu-id="37edc-183">Example: `Credentials: {"User":"someuser", "Password":"P@ssword"}`</span></span>

* <span data-ttu-id="37edc-184">Debug = 1</span><span class="sxs-lookup"><span data-stu-id="37edc-184">Debug = 1</span></span>

  <span data-ttu-id="37edc-185">Para informações que tenham utilidade de curto prazo durante o desenvolvimento e a depuração.</span><span class="sxs-lookup"><span data-stu-id="37edc-185">For information that has short-term usefulness during development and debugging.</span></span> <span data-ttu-id="37edc-186">Exemplo: `Entering method Configure with flag set to true.` Você normalmente não habilitaria logs de nível `Debug` em produção, a menos que estivesse solucionando problemas, devido ao alto volume de logs.</span><span class="sxs-lookup"><span data-stu-id="37edc-186">Example: `Entering method Configure with flag set to true.` You typically wouldn't enable `Debug` level logs in production unless you are troubleshooting, due to the high volume of logs.</span></span>

* <span data-ttu-id="37edc-187">Information = 2</span><span class="sxs-lookup"><span data-stu-id="37edc-187">Information = 2</span></span>

  <span data-ttu-id="37edc-188">Para rastrear o fluxo geral do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="37edc-188">For tracking the general flow of the application.</span></span> <span data-ttu-id="37edc-189">Esses logs normalmente têm algum valor a longo prazo.</span><span class="sxs-lookup"><span data-stu-id="37edc-189">These logs typically have some long-term value.</span></span> <span data-ttu-id="37edc-190">Exemplo: `Request received for path /api/todo`</span><span class="sxs-lookup"><span data-stu-id="37edc-190">Example: `Request received for path /api/todo`</span></span>

* <span data-ttu-id="37edc-191">Warning = 3</span><span class="sxs-lookup"><span data-stu-id="37edc-191">Warning = 3</span></span>

  <span data-ttu-id="37edc-192">Para eventos anormais ou inesperados no fluxo de aplicativo.</span><span class="sxs-lookup"><span data-stu-id="37edc-192">For abnormal or unexpected events in the application flow.</span></span> <span data-ttu-id="37edc-193">Eles podem incluir erros ou outras condições que não fazem com que o aplicativo pare, mas que talvez precisem ser investigados.</span><span class="sxs-lookup"><span data-stu-id="37edc-193">These may include errors or other conditions that don't cause the application to stop, but which may need to be investigated.</span></span> <span data-ttu-id="37edc-194">Exceções manipuladas são um local comum para usar o nível de log `Warning`.</span><span class="sxs-lookup"><span data-stu-id="37edc-194">Handled exceptions are a common place to use the `Warning` log level.</span></span> <span data-ttu-id="37edc-195">Exemplo: `FileNotFoundException for file quotes.txt.`</span><span class="sxs-lookup"><span data-stu-id="37edc-195">Example: `FileNotFoundException for file quotes.txt.`</span></span>

* <span data-ttu-id="37edc-196">Error = 4</span><span class="sxs-lookup"><span data-stu-id="37edc-196">Error = 4</span></span>

  <span data-ttu-id="37edc-197">Para erros e exceções que não podem ser manipulados.</span><span class="sxs-lookup"><span data-stu-id="37edc-197">For errors and exceptions that cannot be handled.</span></span> <span data-ttu-id="37edc-198">Essas mensagens indicam uma falha na atividade ou na operação atual (como a solicitação HTTP atual) e não uma falha em todo o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="37edc-198">These messages indicate a failure in the current activity or operation (such as the current HTTP request), not an application-wide failure.</span></span> <span data-ttu-id="37edc-199">Mensagem de log de exemplo:`Cannot insert record due to duplicate key violation.`</span><span class="sxs-lookup"><span data-stu-id="37edc-199">Example log message: `Cannot insert record due to duplicate key violation.`</span></span>

* <span data-ttu-id="37edc-200">Critical = 5</span><span class="sxs-lookup"><span data-stu-id="37edc-200">Critical = 5</span></span>

  <span data-ttu-id="37edc-201">Para falhas que exigem atenção imediata.</span><span class="sxs-lookup"><span data-stu-id="37edc-201">For failures that require immediate attention.</span></span> <span data-ttu-id="37edc-202">Exemplos: cenários de perda de dados, espaço em disco insuficiente.</span><span class="sxs-lookup"><span data-stu-id="37edc-202">Examples: data loss scenarios, out of disk space.</span></span>

<span data-ttu-id="37edc-203">Você pode usar o nível de log para controlar a quantidade de saída de log que é gravada em uma mídia de armazenamento específica ou em uma janela de exibição.</span><span class="sxs-lookup"><span data-stu-id="37edc-203">You can use the log level to control how much log output is written to a particular storage medium or display window.</span></span> <span data-ttu-id="37edc-204">Por exemplo, em produção, você pode desejar que todos os logs de nível `Information` e inferior vão para um armazenamento de dados de volume e que todos os logs de nível `Warning` e superior vão para um armazenamento de dados de valor.</span><span class="sxs-lookup"><span data-stu-id="37edc-204">For example, in production you might want all logs of `Information` level and lower to go to a volume data store, and all logs of `Warning` level and higher to go to a value data store.</span></span> <span data-ttu-id="37edc-205">Durante o desenvolvimento, você normalmente poderia enviar os logs de gravidade `Warning` ou mais alta para o console.</span><span class="sxs-lookup"><span data-stu-id="37edc-205">During development, you might normally send logs of `Warning` or higher severity to the console.</span></span> <span data-ttu-id="37edc-206">Então, quando precisar solucionar problemas, você poderá adicionar o nível `Debug`.</span><span class="sxs-lookup"><span data-stu-id="37edc-206">Then when you need to troubleshoot, you can add `Debug` level.</span></span> <span data-ttu-id="37edc-207">A seção [Filtragem de log](#log-filtering) mais adiante neste artigo explicará como controlar os níveis de log que um provedor manipula.</span><span class="sxs-lookup"><span data-stu-id="37edc-207">The [Log filtering](#log-filtering) section later in this article explains how to control which log levels a provider handles.</span></span>

<span data-ttu-id="37edc-208">A estrutura do ASP.NET Core grava logs de nível `Debug` para eventos de estrutura.</span><span class="sxs-lookup"><span data-stu-id="37edc-208">The ASP.NET Core framework writes `Debug` level logs for framework events.</span></span> <span data-ttu-id="37edc-209">Os exemplos de log anteriores neste artigo excluíram logs abaixo do nível `Information`, portanto, os logs de nível `Debug` não foram mostrados.</span><span class="sxs-lookup"><span data-stu-id="37edc-209">The log examples earlier in this article excluded logs below `Information` level, so no `Debug` level logs were shown.</span></span> <span data-ttu-id="37edc-210">Aqui está um exemplo de logs do console, caso você execute o aplicativo de exemplo configurado para mostrar logs de nível `Debug` e superiores para o provedor de console.</span><span class="sxs-lookup"><span data-stu-id="37edc-210">Here's an example of console logs if you run the sample application configured to show `Debug` and higher logs for the console provider.</span></span>

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

## <a name="log-event-id"></a><span data-ttu-id="37edc-211">ID de evento de log</span><span class="sxs-lookup"><span data-stu-id="37edc-211">Log event ID</span></span>

<span data-ttu-id="37edc-212">Sempre que você grava um log, você pode especificar uma *ID de evento*.</span><span class="sxs-lookup"><span data-stu-id="37edc-212">Each time you write a log, you can specify an *event ID*.</span></span> <span data-ttu-id="37edc-213">O aplicativo de exemplo faz isso usando uma classe `LoggingEvents` definida localmente:</span><span class="sxs-lookup"><span data-stu-id="37edc-213">The sample app does this by using a locally-defined `LoggingEvents` class:</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/sample//Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

<span data-ttu-id="37edc-214">Uma ID de evento é um valor inteiro que pode ser usado para associar um conjunto de eventos registrados.</span><span class="sxs-lookup"><span data-stu-id="37edc-214">An event ID is an integer value that you can use to associate a set of logged events with one another.</span></span> <span data-ttu-id="37edc-215">Por exemplo, um log para adicionar um item a um carrinho de compras pode ser ter a ID de evento 1000 e um log para concluir uma compra pode ter a ID de evento 1001.</span><span class="sxs-lookup"><span data-stu-id="37edc-215">For instance, a log for adding an item to a shopping cart could be event ID 1000 and a log for completing a purchase could be event ID 1001.</span></span>

<span data-ttu-id="37edc-216">Na saída do registro em log, a ID do evento pode ser armazenada em um campo ou incluída na mensagem de texto, dependendo do provedor.</span><span class="sxs-lookup"><span data-stu-id="37edc-216">In logging output, the event ID may be stored in a field or included in the text message, depending on the provider.</span></span> <span data-ttu-id="37edc-217">O provedor Depuração não mostra as IDs de evento, mas o provedor do console sim, entre colchetes depois da categoria:</span><span class="sxs-lookup"><span data-stu-id="37edc-217">The Debug provider doesn't show event IDs, but the console provider shows them in brackets after the category:</span></span>

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-template"></a><span data-ttu-id="37edc-218">Modelo de mensagem de log</span><span class="sxs-lookup"><span data-stu-id="37edc-218">Log message template</span></span>

<span data-ttu-id="37edc-219">Sempre que você grava uma mensagem de log, você fornece um modelo de mensagem.</span><span class="sxs-lookup"><span data-stu-id="37edc-219">Each time you write a log message, you provide a message template.</span></span> <span data-ttu-id="37edc-220">O modelo de mensagem pode ser uma cadeia de caracteres ou pode conter espaços reservados nomeados, nos quais valores de argumento serão colocados.</span><span class="sxs-lookup"><span data-stu-id="37edc-220">The message template can be a string or it can contain named placeholders into which argument values are placed.</span></span> <span data-ttu-id="37edc-221">O modelo não é uma cadeia de caracteres de formatação e os espaços reservados devem ser nomeados, não numerados.</span><span class="sxs-lookup"><span data-stu-id="37edc-221">The template isn't a format string, and placeholders should be named, not numbered.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="37edc-222">A ordem dos espaços reservados e não de seus nomes, determina quais parâmetros serão usados para fornecer seus valores.</span><span class="sxs-lookup"><span data-stu-id="37edc-222">The order of placeholders, not their names, determines which parameters are used to provide their values.</span></span> <span data-ttu-id="37edc-223">Se você tiver o seguinte código:</span><span class="sxs-lookup"><span data-stu-id="37edc-223">If you have the following code:</span></span>

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

<span data-ttu-id="37edc-224">A mensagem de log resultante terá esta aparência:</span><span class="sxs-lookup"><span data-stu-id="37edc-224">The resulting log message looks like this:</span></span>

```
Parameter values: parm1, parm2
```

<span data-ttu-id="37edc-225">A estrutura de registros realiza a formatação de mensagens dessa maneira para possibilitar que os provedores de log implementem o [registro em log semântico, também conhecido como registro em log estruturado](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="37edc-225">The logging framework does message formatting in this way to make it possible for logging providers to implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span> <span data-ttu-id="37edc-226">Como os próprios argumentos são passados para o sistema de registro em log, não apenas o modelo de mensagem formatada, mas os provedores de log, também poderão armazenar os valores de parâmetro como campos, além do modelo de mensagem.</span><span class="sxs-lookup"><span data-stu-id="37edc-226">Because the arguments themselves are passed to the logging system, not just the formatted message template, logging providers can store the parameter values as fields in addition to the message template.</span></span> <span data-ttu-id="37edc-227">Se você estiver direcionando a saída de log para o Armazenamento de Tabelas do Azure e sua chamada de método do agente tiver esta aparência:</span><span class="sxs-lookup"><span data-stu-id="37edc-227">If you're directing your log output to Azure Table Storage and your logger method call looks like this:</span></span>

```csharp
_logger.LogInformation("Getting item {ID} at {RequestTime}", id, DateTime.Now);
```

<span data-ttu-id="37edc-228">Cada entidade da Tabela do Azure poderá ter propriedades `ID` e `RequestTime`, o que simplificará as consultas nos dados de log.</span><span class="sxs-lookup"><span data-stu-id="37edc-228">Each Azure Table entity can have `ID` and `RequestTime` properties, which simplifies queries on log data.</span></span> <span data-ttu-id="37edc-229">Você poderá encontrar todos os logs em um determinado intervalo de `RequestTime` sem a necessidade de analisar o tempo limite da mensagem de texto.</span><span class="sxs-lookup"><span data-stu-id="37edc-229">You can find all logs within a particular `RequestTime` range without the need to parse the time out of the text message.</span></span>

## <a name="logging-exceptions"></a><span data-ttu-id="37edc-230">Exceções de registro em log</span><span class="sxs-lookup"><span data-stu-id="37edc-230">Logging exceptions</span></span>

<span data-ttu-id="37edc-231">Os métodos de agente têm sobrecargas que permitem que você passe uma exceção, como no exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="37edc-231">The logger methods have overloads that let you pass in an exception, as in the following example:</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

<span data-ttu-id="37edc-232">Provedores diferentes manipulam as informações de exceção de maneiras diferentes.</span><span class="sxs-lookup"><span data-stu-id="37edc-232">Different providers handle the exception information in different ways.</span></span> <span data-ttu-id="37edc-233">Aqui está um exemplo da saída do provedor Depuração do código mostrado acima.</span><span class="sxs-lookup"><span data-stu-id="37edc-233">Here's an example of Debug provider output from the code shown above.</span></span>

```
TodoApi.Controllers.TodoController:Warning: GetById(036dd898-fb01-47e8-9a65-f92eb73cf924) NOT FOUND

System.Exception: Item not found exception.
 at TodoApi.Controllers.TodoController.GetById(String id) in C:\logging\sample\src\TodoApi\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a><span data-ttu-id="37edc-234">Filtragem de log</span><span class="sxs-lookup"><span data-stu-id="37edc-234">Log filtering</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="37edc-235">Você pode especificar um nível de log mínimo para um provedor e uma categoria específicos ou para todos os provedores ou todas as categorias.</span><span class="sxs-lookup"><span data-stu-id="37edc-235">You can specify a minimum log level for a specific provider and category or for all providers or all categories.</span></span> <span data-ttu-id="37edc-236">Os logs abaixo do nível mínimo não serão passados para esse provedor, para que não sejam exibidos ou armazenados.</span><span class="sxs-lookup"><span data-stu-id="37edc-236">Any logs below the minimum level aren't passed to that provider, so they don't get displayed or stored.</span></span>

<span data-ttu-id="37edc-237">Se quiser suprimir todos os logs, você poderá especificar `LogLevel.None` como o nível de log mínimo.</span><span class="sxs-lookup"><span data-stu-id="37edc-237">If you want to suppress all logs, you can specify `LogLevel.None` as the minimum log level.</span></span> <span data-ttu-id="37edc-238">O valor inteiro de `LogLevel.None` é 6, que é maior do que `LogLevel.Critical` (5).</span><span class="sxs-lookup"><span data-stu-id="37edc-238">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

### <a name="create-filter-rules-in-configuration"></a><span data-ttu-id="37edc-239">Criar regras de filtro na configuração</span><span class="sxs-lookup"><span data-stu-id="37edc-239">Create filter rules in configuration</span></span>

<span data-ttu-id="37edc-240">Os modelos de projeto criam código que chama `CreateDefaultBuilder` para configurar o registro em log para os provedores Console e Depuração.</span><span class="sxs-lookup"><span data-stu-id="37edc-240">The project templates create code that calls `CreateDefaultBuilder` to set up logging for the Console and Debug providers.</span></span> <span data-ttu-id="37edc-241">O método `CreateDefaultBuilder` também configura o registro em log para procurar a configuração em uma seção `Logging`, usando código semelhante ao seguinte:</span><span class="sxs-lookup"><span data-stu-id="37edc-241">The `CreateDefaultBuilder` method also sets up logging to look for configuration in a `Logging` section, using code like the following:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_ExpandDefault&highlight=15)]

<span data-ttu-id="37edc-242">Os dados de configuração especificam níveis de log mínimo por provedor e por categoria, como no exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="37edc-242">The configuration data specifies minimum log levels by provider and category, as in the following example:</span></span>

[!code-json[](index/sample2/appsettings.json)]

<span data-ttu-id="37edc-243">Este JSON cria seis regras de filtro, uma para o provedor Depuração, quatro para o provedor Console e uma que se aplica a todos os provedores.</span><span class="sxs-lookup"><span data-stu-id="37edc-243">This JSON creates six filter rules, one for the Debug provider, four for the Console provider, and one that applies to all providers.</span></span> <span data-ttu-id="37edc-244">Você verá posteriormente como apenas uma dessas regras é escolhida para cada provedor quando um objeto `ILogger` é criado.</span><span class="sxs-lookup"><span data-stu-id="37edc-244">You'll see later how just one of these rules is chosen for each provider when an `ILogger` object is created.</span></span>

### <a name="filter-rules-in-code"></a><span data-ttu-id="37edc-245">Regras de filtro no código</span><span class="sxs-lookup"><span data-stu-id="37edc-245">Filter rules in code</span></span>

<span data-ttu-id="37edc-246">Você pode registrar regras de filtro no código, conforme mostrado no exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="37edc-246">You can register filter rules in code, as shown in the following example:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

<span data-ttu-id="37edc-247">O segundo `AddFilter` especifica o provedor Depuração usando seu nome de tipo.</span><span class="sxs-lookup"><span data-stu-id="37edc-247">The second `AddFilter` specifies the Debug provider by using its type name.</span></span> <span data-ttu-id="37edc-248">O primeiro `AddFilter` se aplica a todos os provedores porque ele não especifica um tipo de provedor.</span><span class="sxs-lookup"><span data-stu-id="37edc-248">The first `AddFilter` applies to all providers because it doesn't specify a provider type.</span></span>

### <a name="how-filtering-rules-are-applied"></a><span data-ttu-id="37edc-249">Como as regras de filtragem são aplicadas</span><span class="sxs-lookup"><span data-stu-id="37edc-249">How filtering rules are applied</span></span>

<span data-ttu-id="37edc-250">Os dados de configuração e o código `AddFilter`, mostrados nos exemplos anteriores, criam as regras mostradas na tabela a seguir.</span><span class="sxs-lookup"><span data-stu-id="37edc-250">The configuration data and the `AddFilter` code shown in the preceding examples create the rules shown in the following table.</span></span> <span data-ttu-id="37edc-251">As primeiras seis vêm do exemplo de configuração e as últimas duas vêm do exemplo de código.</span><span class="sxs-lookup"><span data-stu-id="37edc-251">The first six come from the configuration example and the last two come from the code example.</span></span>

| <span data-ttu-id="37edc-252">Número</span><span class="sxs-lookup"><span data-stu-id="37edc-252">Number</span></span> | <span data-ttu-id="37edc-253">Provider</span><span class="sxs-lookup"><span data-stu-id="37edc-253">Provider</span></span>      | <span data-ttu-id="37edc-254">Categorias que começam com...</span><span class="sxs-lookup"><span data-stu-id="37edc-254">Categories that begin with ...</span></span>          | <span data-ttu-id="37edc-255">Nível de log mínimo</span><span class="sxs-lookup"><span data-stu-id="37edc-255">Minimum log level</span></span> |
| :----: | ------------- | --------------------------------------- | ----------------- |
| <span data-ttu-id="37edc-256">1</span><span class="sxs-lookup"><span data-stu-id="37edc-256">1</span></span>      | <span data-ttu-id="37edc-257">Depurar</span><span class="sxs-lookup"><span data-stu-id="37edc-257">Debug</span></span>         | <span data-ttu-id="37edc-258">Todas as categorias</span><span class="sxs-lookup"><span data-stu-id="37edc-258">All categories</span></span>                          | <span data-ttu-id="37edc-259">Informações</span><span class="sxs-lookup"><span data-stu-id="37edc-259">Information</span></span>       |
| <span data-ttu-id="37edc-260">2</span><span class="sxs-lookup"><span data-stu-id="37edc-260">2</span></span>      | <span data-ttu-id="37edc-261">Console</span><span class="sxs-lookup"><span data-stu-id="37edc-261">Console</span></span>       | <span data-ttu-id="37edc-262">Microsoft.AspNetCore.Mvc.Razor.Internal</span><span class="sxs-lookup"><span data-stu-id="37edc-262">Microsoft.AspNetCore.Mvc.Razor.Internal</span></span> | <span data-ttu-id="37edc-263">Aviso</span><span class="sxs-lookup"><span data-stu-id="37edc-263">Warning</span></span>           |
| <span data-ttu-id="37edc-264">3</span><span class="sxs-lookup"><span data-stu-id="37edc-264">3</span></span>      | <span data-ttu-id="37edc-265">Console</span><span class="sxs-lookup"><span data-stu-id="37edc-265">Console</span></span>       | <span data-ttu-id="37edc-266">Microsoft.AspNetCore.Mvc.Razor.Razor</span><span class="sxs-lookup"><span data-stu-id="37edc-266">Microsoft.AspNetCore.Mvc.Razor.Razor</span></span>    | <span data-ttu-id="37edc-267">Depurar</span><span class="sxs-lookup"><span data-stu-id="37edc-267">Debug</span></span>             |
| <span data-ttu-id="37edc-268">4</span><span class="sxs-lookup"><span data-stu-id="37edc-268">4</span></span>      | <span data-ttu-id="37edc-269">Console</span><span class="sxs-lookup"><span data-stu-id="37edc-269">Console</span></span>       | <span data-ttu-id="37edc-270">Microsoft.AspNetCore.Mvc.Razor</span><span class="sxs-lookup"><span data-stu-id="37edc-270">Microsoft.AspNetCore.Mvc.Razor</span></span>          | <span data-ttu-id="37edc-271">Erro</span><span class="sxs-lookup"><span data-stu-id="37edc-271">Error</span></span>             |
| <span data-ttu-id="37edc-272">5</span><span class="sxs-lookup"><span data-stu-id="37edc-272">5</span></span>      | <span data-ttu-id="37edc-273">Console</span><span class="sxs-lookup"><span data-stu-id="37edc-273">Console</span></span>       | <span data-ttu-id="37edc-274">Todas as categorias</span><span class="sxs-lookup"><span data-stu-id="37edc-274">All categories</span></span>                          | <span data-ttu-id="37edc-275">Informações</span><span class="sxs-lookup"><span data-stu-id="37edc-275">Information</span></span>       |
| <span data-ttu-id="37edc-276">6</span><span class="sxs-lookup"><span data-stu-id="37edc-276">6</span></span>      | <span data-ttu-id="37edc-277">Todos os provedores</span><span class="sxs-lookup"><span data-stu-id="37edc-277">All providers</span></span> | <span data-ttu-id="37edc-278">Todas as categorias</span><span class="sxs-lookup"><span data-stu-id="37edc-278">All categories</span></span>                          | <span data-ttu-id="37edc-279">Depurar</span><span class="sxs-lookup"><span data-stu-id="37edc-279">Debug</span></span>             |
| <span data-ttu-id="37edc-280">7</span><span class="sxs-lookup"><span data-stu-id="37edc-280">7</span></span>      | <span data-ttu-id="37edc-281">Todos os provedores</span><span class="sxs-lookup"><span data-stu-id="37edc-281">All providers</span></span> | <span data-ttu-id="37edc-282">Sistema</span><span class="sxs-lookup"><span data-stu-id="37edc-282">System</span></span>                                  | <span data-ttu-id="37edc-283">Depurar</span><span class="sxs-lookup"><span data-stu-id="37edc-283">Debug</span></span>             |
| <span data-ttu-id="37edc-284">8</span><span class="sxs-lookup"><span data-stu-id="37edc-284">8</span></span>      | <span data-ttu-id="37edc-285">Depurar</span><span class="sxs-lookup"><span data-stu-id="37edc-285">Debug</span></span>         | <span data-ttu-id="37edc-286">Microsoft</span><span class="sxs-lookup"><span data-stu-id="37edc-286">Microsoft</span></span>                               | <span data-ttu-id="37edc-287">Rastrear</span><span class="sxs-lookup"><span data-stu-id="37edc-287">Trace</span></span>             |

<span data-ttu-id="37edc-288">Quando você cria um objeto `ILogger` para usar para gravar logs, o objeto `ILoggerFactory` seleciona uma única regra por provedor para aplicar a esse agente.</span><span class="sxs-lookup"><span data-stu-id="37edc-288">When you create an `ILogger` object to write logs with, the `ILoggerFactory` object selects a single rule per provider to apply to that logger.</span></span> <span data-ttu-id="37edc-289">Todas as mensagens gravadas pelo objeto `ILogger` são filtradas com base nas regras selecionadas.</span><span class="sxs-lookup"><span data-stu-id="37edc-289">All messages written by that `ILogger` object are filtered based on the selected rules.</span></span> <span data-ttu-id="37edc-290">A regra mais específica possível para cada par de categoria e provedor é selecionada dentre as regras disponíveis.</span><span class="sxs-lookup"><span data-stu-id="37edc-290">The most specific rule possible for each provider and category pair is selected from the available rules.</span></span>

<span data-ttu-id="37edc-291">O algoritmo a seguir é usado para cada provedor quando um `ILogger` é criado para uma determinada categoria:</span><span class="sxs-lookup"><span data-stu-id="37edc-291">The following algorithm is used for each provider when an `ILogger` is created for a given category:</span></span>

* <span data-ttu-id="37edc-292">Selecione todas as regras que correspondem ao provedor ou seu alias.</span><span class="sxs-lookup"><span data-stu-id="37edc-292">Select all rules that match the provider or its alias.</span></span> <span data-ttu-id="37edc-293">Se nenhuma for encontrada, selecione todas as regras com um provedor vazio.</span><span class="sxs-lookup"><span data-stu-id="37edc-293">If none are found, select all rules with an empty provider.</span></span>
* <span data-ttu-id="37edc-294">Do resultado da etapa anterior, selecione as regras com o prefixo de categoria de maior correspondência.</span><span class="sxs-lookup"><span data-stu-id="37edc-294">From the result of the preceding step, select rules with longest matching category prefix.</span></span> <span data-ttu-id="37edc-295">Se nenhuma for encontrada, selecione todas as regras que não especificam uma categoria.</span><span class="sxs-lookup"><span data-stu-id="37edc-295">If none are found, select all rules that don't specify a category.</span></span>
* <span data-ttu-id="37edc-296">Se várias regras forem selecionadas use a **última**.</span><span class="sxs-lookup"><span data-stu-id="37edc-296">If multiple rules are selected take the **last** one.</span></span>
* <span data-ttu-id="37edc-297">Se nenhuma regra for selecionada, use `MinimumLevel`.</span><span class="sxs-lookup"><span data-stu-id="37edc-297">If no rules are selected, use `MinimumLevel`.</span></span>

<span data-ttu-id="37edc-298">Por exemplo, suponha que você tem a lista anterior de regras e cria um objeto `ILogger` para a categoria "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":</span><span class="sxs-lookup"><span data-stu-id="37edc-298">For example, suppose you have the preceding list of rules and you create an `ILogger` object for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":</span></span>

* <span data-ttu-id="37edc-299">Para o provedor Depuração as regras 1, 6 e 8 se aplicam.</span><span class="sxs-lookup"><span data-stu-id="37edc-299">For the Debug provider, rules 1, 6, and 8 apply.</span></span> <span data-ttu-id="37edc-300">A regra 8 é mais específica, portanto é a que será selecionada.</span><span class="sxs-lookup"><span data-stu-id="37edc-300">Rule 8 is most specific, so that's the one selected.</span></span>
* <span data-ttu-id="37edc-301">Para o provedor Console as regras 3, 4, 5 e 6 se aplicam.</span><span class="sxs-lookup"><span data-stu-id="37edc-301">For the Console provider, rules 3, 4, 5, and 6 apply.</span></span> <span data-ttu-id="37edc-302">A regra 3 é a mais específica.</span><span class="sxs-lookup"><span data-stu-id="37edc-302">Rule 3 is most specific.</span></span>

<span data-ttu-id="37edc-303">Quando você cria logs com um `ILogger` para a categoria "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine", os logs de nível `Trace` e superiores vão para o provedor Depuração e os logs de nível `Debug` e superiores vão para o provedor Console.</span><span class="sxs-lookup"><span data-stu-id="37edc-303">When you create logs with an `ILogger` for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine", logs of `Trace` level and above will go to the Debug provider, and logs of `Debug` level and above will go to the Console provider.</span></span>

### <a name="provider-aliases"></a><span data-ttu-id="37edc-304">Aliases de provedor</span><span class="sxs-lookup"><span data-stu-id="37edc-304">Provider aliases</span></span>

<span data-ttu-id="37edc-305">Você pode usar o nome do tipo para especificar um provedor na configuração, mas cada provedor define um *alias* menor que é mais fácil de usar.</span><span class="sxs-lookup"><span data-stu-id="37edc-305">You can use the type name to specify a provider in configuration, but each provider defines a shorter *alias* that's easier to use.</span></span> <span data-ttu-id="37edc-306">Para os provedores internos, use os seguintes aliases:</span><span class="sxs-lookup"><span data-stu-id="37edc-306">For the built-in providers, use the following aliases:</span></span>

* <span data-ttu-id="37edc-307">Console</span><span class="sxs-lookup"><span data-stu-id="37edc-307">Console</span></span>
* <span data-ttu-id="37edc-308">Depurar</span><span class="sxs-lookup"><span data-stu-id="37edc-308">Debug</span></span>
* <span data-ttu-id="37edc-309">EventLog</span><span class="sxs-lookup"><span data-stu-id="37edc-309">EventLog</span></span>
* <span data-ttu-id="37edc-310">AzureAppServices</span><span class="sxs-lookup"><span data-stu-id="37edc-310">AzureAppServices</span></span>
* <span data-ttu-id="37edc-311">TraceSource</span><span class="sxs-lookup"><span data-stu-id="37edc-311">TraceSource</span></span>
* <span data-ttu-id="37edc-312">EventSource</span><span class="sxs-lookup"><span data-stu-id="37edc-312">EventSource</span></span>

### <a name="default-minimum-level"></a><span data-ttu-id="37edc-313">Nível mínimo padrão</span><span class="sxs-lookup"><span data-stu-id="37edc-313">Default minimum level</span></span>

<span data-ttu-id="37edc-314">Há uma configuração de nível mínimo que entra em vigor somente se nenhuma regra de código ou de configuração se aplicar a um provedor e uma categoria determinados.</span><span class="sxs-lookup"><span data-stu-id="37edc-314">There's a minimum level setting that takes effect only if no rules from configuration or code apply for a given provider and category.</span></span> <span data-ttu-id="37edc-315">O exemplo a seguir mostra como definir o nível mínimo:</span><span class="sxs-lookup"><span data-stu-id="37edc-315">The following example shows how to set the minimum level:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_MinLevel&highlight=3)]

<span data-ttu-id="37edc-316">Se você não definir explicitamente o nível mínimo, o valor padrão será `Information`, o que significa que logs `Trace` e `Debug` serão ignorados.</span><span class="sxs-lookup"><span data-stu-id="37edc-316">If you don't explicitly set the minimum level, the default value is `Information`, which means that `Trace` and `Debug` logs are ignored.</span></span>

### <a name="filter-functions"></a><span data-ttu-id="37edc-317">Funções de filtro</span><span class="sxs-lookup"><span data-stu-id="37edc-317">Filter functions</span></span>

<span data-ttu-id="37edc-318">Você pode escrever código em uma função de filtro para aplicar regras de filtragem.</span><span class="sxs-lookup"><span data-stu-id="37edc-318">You can write code in a filter function to apply filtering rules.</span></span> <span data-ttu-id="37edc-319">Uma função de filtro é invocada para todos os provedores e categorias que não têm regras atribuídas a eles por configuração ou código.</span><span class="sxs-lookup"><span data-stu-id="37edc-319">A filter function is invoked for all providers and categories that don't have rules assigned to them by configuration or code.</span></span> <span data-ttu-id="37edc-320">O código na função tem acesso ao tipo de provedor, à categoria e ao nível de log para decidir se uma mensagem deve ser registrada.</span><span class="sxs-lookup"><span data-stu-id="37edc-320">Code in the function has access to the provider type, category, and log level to decide whether or not a message should be logged.</span></span> <span data-ttu-id="37edc-321">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="37edc-321">For example:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="37edc-322">Alguns provedores de log permitem especificar quando os logs devem ser gravados em uma mídia de armazenamento ou ignorados, com base no nível de log e na categoria.</span><span class="sxs-lookup"><span data-stu-id="37edc-322">Some logging providers let you specify when logs should be written to a storage medium or ignored based on log level and category.</span></span>

<span data-ttu-id="37edc-323">Os métodos de extensão `AddConsole` e `AddDebug` fornecem sobrecargas que permitem que você passe critérios de filtragem.</span><span class="sxs-lookup"><span data-stu-id="37edc-323">The `AddConsole` and `AddDebug` extension methods provide overloads that let you pass in filtering criteria.</span></span> <span data-ttu-id="37edc-324">O código de exemplo a seguir faz com que o provedor de console ignore os logs abaixo do nível `Warning`, enquanto o provedor Depuração ignora os logs criados pela estrutura.</span><span class="sxs-lookup"><span data-stu-id="37edc-324">The following sample code causes the console provider to ignore logs below `Warning` level, while the Debug provider ignores logs that the framework creates.</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_AddConsoleAndDebugWithFilter&highlight=6-7)]

<span data-ttu-id="37edc-325">O método `AddEventLog` tem uma sobrecarga que recebe uma instância `EventLogSettings`, que pode conter uma função de filtragem em sua propriedade `Filter`.</span><span class="sxs-lookup"><span data-stu-id="37edc-325">The `AddEventLog` method has an overload that takes an `EventLogSettings` instance, which may contain a filtering function in its `Filter` property.</span></span> <span data-ttu-id="37edc-326">O provedor TraceSource não fornece nenhuma dessas sobrecargas, porque seu nível de registro em log e outros parâmetros são baseados no `SourceSwitch` e no `TraceListener` que ele usa.</span><span class="sxs-lookup"><span data-stu-id="37edc-326">The TraceSource provider doesn't provide any of those overloads, since its logging level and other parameters are based on the `SourceSwitch` and `TraceListener` it uses.</span></span>

<span data-ttu-id="37edc-327">Você pode definir regras de filtragem para todos os provedores registrados com uma instância `ILoggerFactory` usando o método de extensão `WithFilter`.</span><span class="sxs-lookup"><span data-stu-id="37edc-327">You can set filtering rules for all providers that are registered with an `ILoggerFactory` instance by using the `WithFilter` extension method.</span></span> <span data-ttu-id="37edc-328">O exemplo abaixo limita os logs de estrutura (categoria começa com "Microsoft" ou "Sistema") a avisos, permitindo que aplicativo registre no nível de depuração.</span><span class="sxs-lookup"><span data-stu-id="37edc-328">The example below limits framework logs (category begins with "Microsoft" or "System") to warnings while letting the app log at debug level.</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_FactoryFilter&highlight=6-11)]

<span data-ttu-id="37edc-329">Se quiser usar a filtragem para impedir que todos os logs de uma determinada categoria sejam gravados, você poderá especificar `LogLevel.None` como o nível de log mínimo para essa categoria.</span><span class="sxs-lookup"><span data-stu-id="37edc-329">If you want to use filtering to prevent all logs from being written for a particular category, you can specify `LogLevel.None` as the minimum log level for that category.</span></span> <span data-ttu-id="37edc-330">O valor inteiro de `LogLevel.None` é 6, que é maior do que `LogLevel.Critical` (5).</span><span class="sxs-lookup"><span data-stu-id="37edc-330">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

<span data-ttu-id="37edc-331">O método de extensão `WithFilter` é fornecido pelo pacote NuGet [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter).</span><span class="sxs-lookup"><span data-stu-id="37edc-331">The `WithFilter` extension method is provided by the [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) NuGet package.</span></span> <span data-ttu-id="37edc-332">O método retorna um nova instância `ILoggerFactory`, que filtrará as mensagens de log passadas para todos os provedores de agente registrados com ela.</span><span class="sxs-lookup"><span data-stu-id="37edc-332">The method returns a new `ILoggerFactory` instance that will filter the log messages passed to all logger providers registered with it.</span></span> <span data-ttu-id="37edc-333">Isso não afeta nenhuma outra instância de `ILoggerFactory`, incluindo a instância de `ILoggerFactory` original.</span><span class="sxs-lookup"><span data-stu-id="37edc-333">It doesn't affect any other `ILoggerFactory` instances, including the original `ILoggerFactory` instance.</span></span>

::: moniker-end

## <a name="log-scopes"></a><span data-ttu-id="37edc-334">Escopos de log</span><span class="sxs-lookup"><span data-stu-id="37edc-334">Log scopes</span></span>

<span data-ttu-id="37edc-335">Você pode agrupar um conjunto de operações lógicas dentro de um *escopo* para anexar os mesmos dados a cada log que é criado como parte desse conjunto.</span><span class="sxs-lookup"><span data-stu-id="37edc-335">You can group a set of logical operations within a *scope* in order to attach the same data to each log that's created as part of that set.</span></span> <span data-ttu-id="37edc-336">Por exemplo, é conveniente que cada log criado como parte do processamento de uma transação inclua a ID da transação.</span><span class="sxs-lookup"><span data-stu-id="37edc-336">For example, you might want every log created as part of processing a transaction to include the transaction ID.</span></span>

<span data-ttu-id="37edc-337">Um escopo é um tipo `IDisposable` retornado pelo método [ILogger.BeginScope&lt;TState&gt;](/dotnet/api/microsoft.extensions.logging.ilogger.beginscope) e que dura até que seja descartado.</span><span class="sxs-lookup"><span data-stu-id="37edc-337">A scope is an `IDisposable` type that's returned by the [ILogger.BeginScope&lt;TState&gt;](/dotnet/api/microsoft.extensions.logging.ilogger.beginscope) method and lasts until it's disposed.</span></span> <span data-ttu-id="37edc-338">Um escopo é usado pelo encapsulamento das chama de agente em um bloco `using`, conforme mostrado aqui:</span><span class="sxs-lookup"><span data-stu-id="37edc-338">You use a scope by wrapping your logger calls in a `using` block, as shown here:</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

<span data-ttu-id="37edc-339">O código a seguir habilita os escopos para o provedor de console:</span><span class="sxs-lookup"><span data-stu-id="37edc-339">The following code enables scopes for the console provider:</span></span>

::: moniker range="> aspnetcore-2.0"

<span data-ttu-id="37edc-340">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="37edc-340">*Program.cs*:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> <span data-ttu-id="37edc-341">A configuração da opção de agente de console `IncludeScopes` é necessária para habilitar o registro em log baseado em escopo.</span><span class="sxs-lookup"><span data-stu-id="37edc-341">Configuring the `IncludeScopes` console logger option is required to enable scope-based logging.</span></span>
>
> <span data-ttu-id="37edc-342">`IncludeScopes` pode ser configurado por meio de arquivos de configuração *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="37edc-342">`IncludeScopes` can be configured via *appsettings* configuration files.</span></span> <span data-ttu-id="37edc-343">Para obter mais informações, veja a seção [Definição do arquivo de configurações](#settings-file-configuration).</span><span class="sxs-lookup"><span data-stu-id="37edc-343">For more information, see the [Settings file configuration](#settings-file-configuration) section.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="37edc-344">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="37edc-344">*Program.cs*:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> <span data-ttu-id="37edc-345">A configuração da opção de agente de console `IncludeScopes` é necessária para habilitar o registro em log baseado em escopo.</span><span class="sxs-lookup"><span data-stu-id="37edc-345">Configuring the `IncludeScopes` console logger option is required to enable scope-based logging.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="37edc-346">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="37edc-346">*Startup.cs*:</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_Scopes&highlight=6)]

::: moniker-end

<span data-ttu-id="37edc-347">Cada mensagem de log inclui as informações com escopo definido:</span><span class="sxs-lookup"><span data-stu-id="37edc-347">Each log message includes the scoped information:</span></span>

```
info: TodoApi.Controllers.TodoController[1002]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      GetById(0) NOT FOUND
```

## <a name="built-in-logging-providers"></a><span data-ttu-id="37edc-348">Provedores de log internos</span><span class="sxs-lookup"><span data-stu-id="37edc-348">Built-in logging providers</span></span>

<span data-ttu-id="37edc-349">O ASP.NET Core vem com os seguintes provedores:</span><span class="sxs-lookup"><span data-stu-id="37edc-349">ASP.NET Core ships the following providers:</span></span>

* [<span data-ttu-id="37edc-350">Console</span><span class="sxs-lookup"><span data-stu-id="37edc-350">Console</span></span>](#console-provider)
* [<span data-ttu-id="37edc-351">Depurar</span><span class="sxs-lookup"><span data-stu-id="37edc-351">Debug</span></span>](#debug-provider)
* [<span data-ttu-id="37edc-352">EventSource</span><span class="sxs-lookup"><span data-stu-id="37edc-352">EventSource</span></span>](#eventsource-provider)
* [<span data-ttu-id="37edc-353">EventLog</span><span class="sxs-lookup"><span data-stu-id="37edc-353">EventLog</span></span>](#windows-eventlog-provider)
* [<span data-ttu-id="37edc-354">TraceSource</span><span class="sxs-lookup"><span data-stu-id="37edc-354">TraceSource</span></span>](#tracesource-provider)
* [<span data-ttu-id="37edc-355">Serviço de Aplicativo do Azure</span><span class="sxs-lookup"><span data-stu-id="37edc-355">Azure App Service</span></span>](#azure-app-service-provider)

### <a name="console-provider"></a><span data-ttu-id="37edc-356">Provedor do console</span><span class="sxs-lookup"><span data-stu-id="37edc-356">Console provider</span></span>

<span data-ttu-id="37edc-357">O pacote de provedor [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) envia a saída de log para o console.</span><span class="sxs-lookup"><span data-stu-id="37edc-357">The [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) provider package sends log output to the console.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddConsole();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddConsole();
```

<span data-ttu-id="37edc-358">As [sobrecargas do AddConsole](/dotnet/api/microsoft.extensions.logging.consoleloggerextensions) permitem que você passe um nível de log mínimo, uma função de filtro e um valor booliano que indica se escopos são compatíveis.</span><span class="sxs-lookup"><span data-stu-id="37edc-358">[AddConsole overloads](/dotnet/api/microsoft.extensions.logging.consoleloggerextensions) let you pass in an a minimum log level, a filter function, and a boolean that indicates whether scopes are supported.</span></span> <span data-ttu-id="37edc-359">Outra opção é passar um objeto `IConfiguration`, que pode especificar suporte de escopos e níveis de log.</span><span class="sxs-lookup"><span data-stu-id="37edc-359">Another option is to pass in an `IConfiguration` object, which can specify scopes support and logging levels.</span></span>

<span data-ttu-id="37edc-360">Se você estiver considerando o provedor de console para uso em produção, lembre-se de que ele tem um impacto significativo no desempenho.</span><span class="sxs-lookup"><span data-stu-id="37edc-360">If you are considering the console provider for use in production, be aware that it has a significant impact on performance.</span></span>

<span data-ttu-id="37edc-361">Quando você cria um novo projeto no Visual Studio, o método `AddConsole` tem essa aparência:</span><span class="sxs-lookup"><span data-stu-id="37edc-361">When you create a new project in Visual Studio, the `AddConsole` method looks like this:</span></span>

```csharp
loggerFactory.AddConsole(Configuration.GetSection("Logging"));
```

<span data-ttu-id="37edc-362">Esse código se refere à seção `Logging` do arquivo *appSettings.json*:</span><span class="sxs-lookup"><span data-stu-id="37edc-362">This code refers to the `Logging` section of the *appSettings.json* file:</span></span>

[!code-json[](index/sample//appsettings.json)]

<span data-ttu-id="37edc-363">As configurações mostradas limitam os logs de estrutura a avisos, permitindo que o aplicativo para faça registros no nível de depuração, conforme explicado na [Filtragem de log](#log-filtering) seção.</span><span class="sxs-lookup"><span data-stu-id="37edc-363">The settings shown limit framework logs to warnings while allowing the app to log at debug level, as explained in the [Log filtering](#log-filtering) section.</span></span> <span data-ttu-id="37edc-364">Para obter mais informações, consulte [Configuração](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="37edc-364">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

::: moniker-end

### <a name="debug-provider"></a><span data-ttu-id="37edc-365">Depurar provedor</span><span class="sxs-lookup"><span data-stu-id="37edc-365">Debug provider</span></span>

<span data-ttu-id="37edc-366">O pacote de provedor [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) grava a saída de log usando a classe [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) (chamadas de método `Debug.WriteLine`).</span><span class="sxs-lookup"><span data-stu-id="37edc-366">The [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) provider package writes log output by using the [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) class (`Debug.WriteLine` method calls).</span></span>

<span data-ttu-id="37edc-367">No Linux, esse provedor grava logs em */var/log/message*.</span><span class="sxs-lookup"><span data-stu-id="37edc-367">On Linux, this provider writes logs to */var/log/message*.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddDebug();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddDebug();
```

<span data-ttu-id="37edc-368">As [sobrecargas de AddDebug](/dotnet/api/microsoft.extensions.logging.debugloggerfactoryextensions) permitem que você passe um nível de log mínimo ou uma função de filtro.</span><span class="sxs-lookup"><span data-stu-id="37edc-368">[AddDebug overloads](/dotnet/api/microsoft.extensions.logging.debugloggerfactoryextensions) let you pass in a minimum log level or a filter function.</span></span>

::: moniker-end

### <a name="eventsource-provider"></a><span data-ttu-id="37edc-369">Provedor EventSource</span><span class="sxs-lookup"><span data-stu-id="37edc-369">EventSource provider</span></span>

<span data-ttu-id="37edc-370">Para aplicativos que se destinam ao ASP.NET Core 1.1.0 ou posterior, o pacote de provedor [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) pode implementar o rastreamento de eventos.</span><span class="sxs-lookup"><span data-stu-id="37edc-370">For apps that target ASP.NET Core 1.1.0 or later, the [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) provider package can implement event tracing.</span></span> <span data-ttu-id="37edc-371">No Windows, ele usa [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span><span class="sxs-lookup"><span data-stu-id="37edc-371">On Windows, it uses [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span></span> <span data-ttu-id="37edc-372">O provedor é multiplataforma, mas ainda não há ferramentas de coleta e exibição de eventos para Linux ou macOS.</span><span class="sxs-lookup"><span data-stu-id="37edc-372">The provider is cross-platform, but there are no event collection and display tools yet for Linux or macOS.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddEventSourceLogger();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddEventSourceLogger();
```

::: moniker-end

<span data-ttu-id="37edc-373">Uma boa maneira de coletar e exibir logs é usar o [utilitário PerfView](https://github.com/Microsoft/perfview).</span><span class="sxs-lookup"><span data-stu-id="37edc-373">A good way to collect and view logs is to use the [PerfView utility](https://github.com/Microsoft/perfview).</span></span> <span data-ttu-id="37edc-374">Há outras ferramentas para exibir os logs do ETW, mas o PerfView proporciona a melhor experiência para trabalhar com os eventos de ETW emitidos pelo ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="37edc-374">There are other tools for viewing ETW logs, but PerfView provides the best experience for working with the ETW events emitted by ASP.NET.</span></span>

<span data-ttu-id="37edc-375">Para configurar o PerfView para coletar eventos registrados por esse provedor, adicione a cadeia de caracteres `*Microsoft-Extensions-Logging` à lista **Provedores Adicionais**.</span><span class="sxs-lookup"><span data-stu-id="37edc-375">To configure PerfView for collecting events logged by this provider, add the string `*Microsoft-Extensions-Logging` to the **Additional Providers** list.</span></span> <span data-ttu-id="37edc-376">(Não se esqueça do asterisco no início da cadeia de caracteres).</span><span class="sxs-lookup"><span data-stu-id="37edc-376">(Don't miss the asterisk at the start of the string.)</span></span>

![Outros provedores de Perfview](index/_static/perfview-additional-providers.png)

### <a name="windows-eventlog-provider"></a><span data-ttu-id="37edc-378">Provedor EventLog do Windows</span><span class="sxs-lookup"><span data-stu-id="37edc-378">Windows EventLog provider</span></span>

<span data-ttu-id="37edc-379">O pacote de provedor [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) envia a saída de log para o Log de Eventos do Windows.</span><span class="sxs-lookup"><span data-stu-id="37edc-379">The [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) provider package sends log output to the Windows Event Log.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddEventLog();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddEventLog();
```

<span data-ttu-id="37edc-380">As [sobrecargas de AddEventLog](/dotnet/api/microsoft.extensions.logging.eventloggerfactoryextensions) permitem que você passe `EventLogSettings` ou um nível de log mínimo.</span><span class="sxs-lookup"><span data-stu-id="37edc-380">[AddEventLog overloads](/dotnet/api/microsoft.extensions.logging.eventloggerfactoryextensions) let you pass in `EventLogSettings` or a minimum log level.</span></span>

::: moniker-end

### <a name="tracesource-provider"></a><span data-ttu-id="37edc-381">Provedor TraceSource</span><span class="sxs-lookup"><span data-stu-id="37edc-381">TraceSource provider</span></span>

<span data-ttu-id="37edc-382">O pacote de provedor [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) usa as bibliotecas e provedores de [System.Diagnostics.TraceSource](/dotnet/api/system.diagnostics.tracesource).</span><span class="sxs-lookup"><span data-stu-id="37edc-382">The [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) provider package uses the [System.Diagnostics.TraceSource](/dotnet/api/system.diagnostics.tracesource) libraries and providers.</span></span>

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

<span data-ttu-id="37edc-383">As [sobrecargas de AddTraceSource](/dotnet/api/microsoft.extensions.logging.tracesourcefactoryextensions) permitem que você passe um comutador de fonte e um ouvinte de rastreamento.</span><span class="sxs-lookup"><span data-stu-id="37edc-383">[AddTraceSource overloads](/dotnet/api/microsoft.extensions.logging.tracesourcefactoryextensions) let you pass in a source switch and a trace listener.</span></span>

<span data-ttu-id="37edc-384">Para usar esse provedor, o aplicativo deve ser executado no .NET Framework (em vez do .NET Core).</span><span class="sxs-lookup"><span data-stu-id="37edc-384">To use this provider, an application has to run on the .NET Framework (rather than .NET Core).</span></span> <span data-ttu-id="37edc-385">O provedor permite rotear mensagens a uma variedade de [ouvintes](/dotnet/framework/debug-trace-profile/trace-listeners), como o [TextWriterTraceListener](/dotnet/api/system.diagnostics.textwritertracelistenerr) usado no aplicativo de exemplo.</span><span class="sxs-lookup"><span data-stu-id="37edc-385">The provider lets you route messages to a variety of [listeners](/dotnet/framework/debug-trace-profile/trace-listeners), such as the [TextWriterTraceListener](/dotnet/api/system.diagnostics.textwritertracelistenerr) used in the sample application.</span></span>

<span data-ttu-id="37edc-386">O exemplo a seguir configura um provedor `TraceSource` que registra mensagens `Warning` e superiores na janela de console.</span><span class="sxs-lookup"><span data-stu-id="37edc-386">The following example configures a `TraceSource` provider that logs `Warning` and higher messages to the console window.</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_TraceSource&highlight=9-12)]

### <a name="azure-app-service-provider"></a><span data-ttu-id="37edc-387">Provedor do Serviço de Aplicativo do Azure</span><span class="sxs-lookup"><span data-stu-id="37edc-387">Azure App Service provider</span></span>

<span data-ttu-id="37edc-388">O pacote de provedor [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) grava logs em arquivos de texto no sistema de arquivos de um aplicativo do Serviço de Aplicativo do Azure e no [armazenamento de blobs](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) em uma conta de Armazenamento do Azure.</span><span class="sxs-lookup"><span data-stu-id="37edc-388">The [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) provider package writes logs to text files in an Azure App Service app's file system and to [blob storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) in an Azure Storage account.</span></span> <span data-ttu-id="37edc-389">O pacote de provedor está disponível para aplicativos destinados ao .NET Core 1.1 ou posterior.</span><span class="sxs-lookup"><span data-stu-id="37edc-389">The provider package is available for apps targeting .NET Core 1.1 or later.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="37edc-390">Se você estiver direcionando para o .NET Core, observe os seguintes pontos:</span><span class="sxs-lookup"><span data-stu-id="37edc-390">If targeting .NET Core, note the following points:</span></span>

* <span data-ttu-id="37edc-391">O pacote do provedor está incluído no [metapacote Microsoft.AspNetCore.All](xref:fundamentals/metapackage) do ASP.NET Core, mas não no [metapacote Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="37edc-391">The provider package is included in the ASP.NET Core [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) but not in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>
* <span data-ttu-id="37edc-392">Não chame explicitamente [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics).</span><span class="sxs-lookup"><span data-stu-id="37edc-392">Don't explicitly call [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics).</span></span> <span data-ttu-id="37edc-393">Quando o aplicativo for implantado no Serviço de Aplicativo do Azure, o provedor ficará automaticamente disponível para ele.</span><span class="sxs-lookup"><span data-stu-id="37edc-393">The provider is automatically made available to the app when the app is deployed to Azure App Service.</span></span>

<span data-ttu-id="37edc-394">Se você estiver direcionando para o .NET Framework ou referenciando o metapacote `Microsoft.AspNetCore.App`, adicione o pacote do provedor ao projeto.</span><span class="sxs-lookup"><span data-stu-id="37edc-394">If targeting .NET Framework or referencing the `Microsoft.AspNetCore.App` metapackage, add the provider package to the project.</span></span> <span data-ttu-id="37edc-395">Invocar `AddAzureWebAppDiagnostics` em uma instância do [ILoggerFactory](/dotnet/api/microsoft.extensions.logging.iloggerfactory):</span><span class="sxs-lookup"><span data-stu-id="37edc-395">Invoke `AddAzureWebAppDiagnostics` on an [ILoggerFactory](/dotnet/api/microsoft.extensions.logging.iloggerfactory) instance:</span></span>

```csharp
logging.AddAzureWebAppDiagnostics();
```

::: moniker-end

::: moniker range="= aspnetcore-1.1"

```csharp
loggerFactory.AddAzureWebAppDiagnostics();
```

::: moniker-end

<span data-ttu-id="37edc-396">Uma sobrecarga do [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) permite que você passe [AzureAppServicesDiagnosticsSettings](/dotnet/api/microsoft.extensions.logging.azureappservices.azureappservicesdiagnosticssettings) que pode ser usado para substituir as configurações padrão, como o modelo de saída de registro em log, o nome do blob e o limite do tamanho do arquivo.</span><span class="sxs-lookup"><span data-stu-id="37edc-396">An [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) overload lets you pass in [AzureAppServicesDiagnosticsSettings](/dotnet/api/microsoft.extensions.logging.azureappservices.azureappservicesdiagnosticssettings) with which you can override default settings, such as the logging output template, blob name, and file size limit.</span></span> <span data-ttu-id="37edc-397">(O *modelo Output* é um modelo de mensagem que é aplicado a todos os logs, sobre aquele que você fornece ao chamar um método `ILogger`).</span><span class="sxs-lookup"><span data-stu-id="37edc-397">(*Output template* is a message template that's applied to all logs on top of the one that you provide when you call an `ILogger` method.)</span></span>

<span data-ttu-id="37edc-398">Ao implantar um aplicativo do Serviço de Aplicativo, o aplicativo respeitará as configurações na seção [Logs de Diagnóstico](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag) da página **Serviço de Aplicativo** do Portal do Azure.</span><span class="sxs-lookup"><span data-stu-id="37edc-398">When you deploy to an App Service app, the app honors the settings in the [Diagnostic Logs](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag) section of the **App Service** page of the Azure portal.</span></span> <span data-ttu-id="37edc-399">Quando essas configurações são atualizadas, as alterações entram em vigor imediatamente sem a necessidade de uma reinicialização ou reimplantação do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="37edc-399">When these settings are updated, the changes take effect immediately without requiring a restart or redeployment of the app.</span></span>

![Configurações de registro em log do Azure](index/_static/azure-logging-settings.png)

<span data-ttu-id="37edc-401">O local padrão para arquivos de log é na pasta *D:\\home\\LogFiles\\Application* e o nome de arquivo padrão é *diagnostics-aaaammdd.txt*.</span><span class="sxs-lookup"><span data-stu-id="37edc-401">The default location for log files is in the *D:\\home\\LogFiles\\Application* folder, and the default file name is *diagnostics-yyyymmdd.txt*.</span></span> <span data-ttu-id="37edc-402">O limite padrão de tamanho do arquivo é 10 MB e o número padrão máximo de arquivos mantidos é 2.</span><span class="sxs-lookup"><span data-stu-id="37edc-402">The default file size limit is 10 MB, and the default maximum number of files retained is 2.</span></span> <span data-ttu-id="37edc-403">O nome de blob padrão é *{app-name}{timestamp}/aaaa/mm/dd/hh/{guid}-applicationLog.txt*.</span><span class="sxs-lookup"><span data-stu-id="37edc-403">The default blob name is *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*.</span></span> <span data-ttu-id="37edc-404">Para obter mais informações sobre o comportamento padrão, consulte [AzureAppServicesDiagnosticsSettings](/dotnet/api/microsoft.extensions.logging.azureappservices.azureappservicesdiagnosticssettings).</span><span class="sxs-lookup"><span data-stu-id="37edc-404">For more information about default behavior, see [AzureAppServicesDiagnosticsSettings](/dotnet/api/microsoft.extensions.logging.azureappservices.azureappservicesdiagnosticssettings).</span></span>

<span data-ttu-id="37edc-405">O provedor funciona somente quando o projeto é executado no ambiente do Azure.</span><span class="sxs-lookup"><span data-stu-id="37edc-405">The provider only works when the project runs in the Azure environment.</span></span> <span data-ttu-id="37edc-406">Ele não tem nenhum efeito quando o projeto é executado localmente&mdash;ele não grava em arquivos locais ou no armazenamento de desenvolvimento local para blobs.</span><span class="sxs-lookup"><span data-stu-id="37edc-406">It has no effect when the project is run locally&mdash;it doesn't write to local files or local development storage for blobs.</span></span>

## <a name="third-party-logging-providers"></a><span data-ttu-id="37edc-407">Provedores de log de terceiros</span><span class="sxs-lookup"><span data-stu-id="37edc-407">Third-party logging providers</span></span>

<span data-ttu-id="37edc-408">Estruturas de log de terceiros que funcionam com o ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="37edc-408">Third-party logging frameworks that work with ASP.NET Core:</span></span>

* <span data-ttu-id="37edc-409">[elmah.io](https://elmah.io/) ([repositório GitHub](https://github.com/elmahio/Elmah.Io.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="37edc-409">[elmah.io](https://elmah.io/) ([GitHub repo](https://github.com/elmahio/Elmah.Io.Extensions.Logging))</span></span>
* <span data-ttu-id="37edc-410">[Gelf](http://docs.graylog.org/en/2.3/pages/gelf.html) ([repositório do GitHub](https://github.com/mattwcole/gelf-extensions-logging))</span><span class="sxs-lookup"><span data-stu-id="37edc-410">[Gelf](http://docs.graylog.org/en/2.3/pages/gelf.html) ([GitHub repo](https://github.com/mattwcole/gelf-extensions-logging))</span></span>
* <span data-ttu-id="37edc-411">[JSNLog](http://jsnlog.com/) ([repositório GitHub](https://github.com/mperdeck/jsnlog))</span><span class="sxs-lookup"><span data-stu-id="37edc-411">[JSNLog](http://jsnlog.com/) ([GitHub repo](https://github.com/mperdeck/jsnlog))</span></span>
* <span data-ttu-id="37edc-412">[Loggr](http://loggr.net/) ([repositório GitHub](https://github.com/imobile3/Loggr.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="37edc-412">[Loggr](http://loggr.net/) ([GitHub repo](https://github.com/imobile3/Loggr.Extensions.Logging))</span></span>
* <span data-ttu-id="37edc-413">[NLog](http://nlog-project.org/) ([repositório GitHub](https://github.com/NLog/NLog.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="37edc-413">[NLog](http://nlog-project.org/) ([GitHub repo](https://github.com/NLog/NLog.Extensions.Logging))</span></span>
* <span data-ttu-id="37edc-414">[Serilog](https://serilog.net/) ([repositório GitHub](https://github.com/serilog/serilog-extensions-logging))</span><span class="sxs-lookup"><span data-stu-id="37edc-414">[Serilog](https://serilog.net/) ([GitHub repo](https://github.com/serilog/serilog-extensions-logging))</span></span>

<span data-ttu-id="37edc-415">Algumas estruturas de terceiros podem fazer o [log semântico, também conhecido como registro em log estruturado](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="37edc-415">Some third-party frameworks can perform [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="37edc-416">Usar uma estrutura de terceiros é semelhante ao uso de um dos provedores internos:</span><span class="sxs-lookup"><span data-stu-id="37edc-416">Using a third-party framework is similar to using one of the built-in providers:</span></span>

1. <span data-ttu-id="37edc-417">Adicione um pacote NuGet ao projeto.</span><span class="sxs-lookup"><span data-stu-id="37edc-417">Add a NuGet package to your project.</span></span>
1. <span data-ttu-id="37edc-418">Chame um método de extensão em `ILoggerFactory`.</span><span class="sxs-lookup"><span data-stu-id="37edc-418">Call an extension method on `ILoggerFactory`.</span></span>

<span data-ttu-id="37edc-419">Para obter mais informações, consulte a documentação de cada estrutura.</span><span class="sxs-lookup"><span data-stu-id="37edc-419">For more information, see each framework's documentation.</span></span>

## <a name="azure-log-streaming"></a><span data-ttu-id="37edc-420">Fluxo de log do Azure</span><span class="sxs-lookup"><span data-stu-id="37edc-420">Azure log streaming</span></span>

<span data-ttu-id="37edc-421">O fluxo de log do Azure permite que você exiba a atividade de log em tempo real:</span><span class="sxs-lookup"><span data-stu-id="37edc-421">Azure log streaming enables you to view log activity in real time from:</span></span>

* <span data-ttu-id="37edc-422">Do servidor de aplicativos</span><span class="sxs-lookup"><span data-stu-id="37edc-422">The application server</span></span>
* <span data-ttu-id="37edc-423">Do servidor Web</span><span class="sxs-lookup"><span data-stu-id="37edc-423">The web server</span></span>
* <span data-ttu-id="37edc-424">De uma solicitação de rastreio com falha</span><span class="sxs-lookup"><span data-stu-id="37edc-424">Failed request tracing</span></span>

<span data-ttu-id="37edc-425">Para configurar o fluxo de log do Azure:</span><span class="sxs-lookup"><span data-stu-id="37edc-425">To configure Azure log streaming:</span></span>

* <span data-ttu-id="37edc-426">Navegue até a página **Logs de Diagnóstico** da página do portal do seu aplicativo</span><span class="sxs-lookup"><span data-stu-id="37edc-426">Navigate to the **Diagnostics Logs** page from your application's portal page</span></span>
* <span data-ttu-id="37edc-427">Defina o **Log de aplicativo (Sistema de Arquivos)** como ativado.</span><span class="sxs-lookup"><span data-stu-id="37edc-427">Set **Application Logging (Filesystem)** to on.</span></span>

![Página de logs de diagnóstico do Portal do Azure](index/_static/azure-diagnostic-logs.png)

<span data-ttu-id="37edc-429">Navegue até a página **Fluxo de Log** para exibir as mensagens de aplicativo.</span><span class="sxs-lookup"><span data-stu-id="37edc-429">Navigate to the **Log Streaming** page to view application messages.</span></span> <span data-ttu-id="37edc-430">Elas são registradas pelo aplicativo por meio da interface `ILogger`.</span><span class="sxs-lookup"><span data-stu-id="37edc-430">They're logged by application through the `ILogger` interface.</span></span>

![Fluxo de log do aplicativo do Portal do Azure](index/_static/azure-log-streaming.png)

## <a name="azure-application-insights-trace-logging"></a><span data-ttu-id="37edc-432">Log de rastreamento do Azure Application Insights</span><span class="sxs-lookup"><span data-stu-id="37edc-432">Azure Application Insights trace logging</span></span>

<span data-ttu-id="37edc-433">O SDK do [Application Insights](https://azure.microsoft.com/services/application-insights/) é capaz de coletar telemetria de rastreamento de logs gerados por meio de infraestrutura de log do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="37edc-433">The [Application Insights](https://azure.microsoft.com/services/application-insights/) SDK is capable of collecting trace telemetry from logs generated via the ASP.NET Core logging infrastructure.</span></span> <span data-ttu-id="37edc-434">Para obter mais informações, veja [Wiki do Microsoft/ApplicationInsights-aspnetcore: Log](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Logging).</span><span class="sxs-lookup"><span data-stu-id="37edc-434">For more information, see [Microsoft/ApplicationInsights-aspnetcore Wiki: Logging](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Logging).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="37edc-435">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="37edc-435">Additional resources</span></span>

* <xref:fundamentals/logging/loggermessage>

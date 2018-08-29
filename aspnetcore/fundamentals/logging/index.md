---
title: Registro em log no ASP.NET Core
author: ardalis
description: Saiba mais sobre a estrutura de registros no ASP.NET Core. Descubra os provedores de log internos e saiba mais sobre os provedores de terceiros populares.
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/27/2018
uid: fundamentals/logging/index
ms.openlocfilehash: c6e9aae06df6ebec373b1296f86e37380bf08b15
ms.sourcegitcommit: 847cc1de5526ff42a7303491e6336c2dbdb45de4
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "43055755"
---
# <a name="logging-in-aspnet-core"></a><span data-ttu-id="7984a-104">Registro em log no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7984a-104">Logging in ASP.NET Core</span></span>

<span data-ttu-id="7984a-105">Por [Steve Smith](https://ardalis.com/) e [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="7984a-105">By [Steve Smith](https://ardalis.com/) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="7984a-106">O ASP.NET Core dá suporte a uma API de registro em log que funciona com uma variedade de provedores de logs.</span><span class="sxs-lookup"><span data-stu-id="7984a-106">ASP.NET Core supports a logging API that works with a variety of logging providers.</span></span> <span data-ttu-id="7984a-107">Os provedores internos permitem que você envie logs para um ou mais destinos e você pode se conectar a uma estrutura de registros de terceiros.</span><span class="sxs-lookup"><span data-stu-id="7984a-107">Built-in providers let you send logs to one or more destinations, and you can plug in a third-party logging framework.</span></span> <span data-ttu-id="7984a-108">Este artigo mostra como usar a API de registro em log e os provedores internos em seu código.</span><span class="sxs-lookup"><span data-stu-id="7984a-108">This article shows how to use the built-in logging API and providers in your code.</span></span>

<span data-ttu-id="7984a-109">Para obter informações sobre o registro stdout ao hospedar com IIS, confira <xref:host-and-deploy/iis/troubleshoot#aspnet-core-module-stdout-log>.</span><span class="sxs-lookup"><span data-stu-id="7984a-109">For information on stdout logging when hosting with IIS, see <xref:host-and-deploy/iis/troubleshoot#aspnet-core-module-stdout-log>.</span></span> <span data-ttu-id="7984a-110">Para obter informações sobre o registro stdout com o Serviço de Aplicativo do Azure, confira <xref:host-and-deploy/azure-apps/troubleshoot#aspnet-core-module-stdout-log>.</span><span class="sxs-lookup"><span data-stu-id="7984a-110">For information on stdout logging with Azure App Service, see <xref:host-and-deploy/azure-apps/troubleshoot#aspnet-core-module-stdout-log>.</span></span>

<span data-ttu-id="7984a-111">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/samples) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="7984a-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="how-to-create-logs"></a><span data-ttu-id="7984a-112">Como criar logs</span><span class="sxs-lookup"><span data-stu-id="7984a-112">How to create logs</span></span>

<span data-ttu-id="7984a-113">Para criar logs, implemente um objeto [ILogger&lt;TCategoryName&gt;](/dotnet/api/microsoft.extensions.logging.ilogger-1) do contêiner de [injeção de dependência](xref:fundamentals/dependency-injection):</span><span class="sxs-lookup"><span data-stu-id="7984a-113">To create logs, implement an [ILogger&lt;TCategoryName&gt;](/dotnet/api/microsoft.extensions.logging.ilogger-1) object from the [dependency injection](xref:fundamentals/dependency-injection) container:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

::: moniker-end

<span data-ttu-id="7984a-114">Em seguida, chame métodos de registro em log nesse objeto de agente:</span><span class="sxs-lookup"><span data-stu-id="7984a-114">Then call logging methods on that logger object:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

<span data-ttu-id="7984a-115">Este exemplo cria logs com a classe `TodoController` como a *categoria*.</span><span class="sxs-lookup"><span data-stu-id="7984a-115">This example creates logs with the `TodoController` class as the *category*.</span></span> <span data-ttu-id="7984a-116">As categorias serão explicadas [posteriormente neste artigo](#log-category).</span><span class="sxs-lookup"><span data-stu-id="7984a-116">Categories are explained [later in this article](#log-category).</span></span>

<span data-ttu-id="7984a-117">O ASP.NET Core não fornece métodos de agente assíncronos porque o registro em log deve ser tão rápido que não valeria a pena usar assíncronos.</span><span class="sxs-lookup"><span data-stu-id="7984a-117">ASP.NET Core doesn't provide async logger methods because logging should be so fast that it isn't worth the cost of using async.</span></span> <span data-ttu-id="7984a-118">Se você estiver em uma situação em que isso não é verdadeiro, considere alterar a maneira como você faz logs.</span><span class="sxs-lookup"><span data-stu-id="7984a-118">If you're in a situation where that's not true, consider changing the way you log.</span></span> <span data-ttu-id="7984a-119">Se seu armazenamento de dados é lento, grave as mensagens de log em um repositório rápido primeiro e, depois, mova-as para um repositório lento.</span><span class="sxs-lookup"><span data-stu-id="7984a-119">If your data store is slow, write the log messages to a fast store first, then move them to a slow store later.</span></span> <span data-ttu-id="7984a-120">Por exemplo, registre em uma fila de mensagens que seja lida e persistida em um armazenamento lento por outro processo.</span><span class="sxs-lookup"><span data-stu-id="7984a-120">For example, log to a message queue that's read and persisted to slow storage by another process.</span></span>

## <a name="how-to-add-providers"></a><span data-ttu-id="7984a-121">Como adicionar provedores</span><span class="sxs-lookup"><span data-stu-id="7984a-121">How to add providers</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="7984a-122">Um provedor de logs obtém as mensagens que você cria com um objeto `ILogger` e as exibe ou armazena.</span><span class="sxs-lookup"><span data-stu-id="7984a-122">A logging provider takes the messages that you create with an `ILogger` object and displays or stores them.</span></span> <span data-ttu-id="7984a-123">Por exemplo, o provedor Console exibe as mensagens no console e o provedor do Serviço de Aplicativo do Azure pode armazená-las no armazenamento de blobs do Azure.</span><span class="sxs-lookup"><span data-stu-id="7984a-123">For example, the Console provider displays messages on the console, and the Azure App Service provider can store them in Azure blob storage.</span></span>

<span data-ttu-id="7984a-124">Para usar um provedor, chame o método de extensão `Add<ProviderName>` do provedor no *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="7984a-124">To use a provider, call the provider's `Add<ProviderName>` extension method in *Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_ExpandDefault&highlight=16,17)]

<span data-ttu-id="7984a-125">O modelo do projeto padrão habilita os provedores de log de depuração e console com uma chamada para o método de extensão [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) no *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="7984a-125">The default project template enables the Console and Debug logging providers with a call to the [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) extension method in *Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_TemplateCode&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="7984a-126">Um provedor de logs obtém as mensagens que você cria com um objeto `ILogger` e as exibe ou armazena.</span><span class="sxs-lookup"><span data-stu-id="7984a-126">A logging provider takes the messages that you create with an `ILogger` object and displays or stores them.</span></span> <span data-ttu-id="7984a-127">Por exemplo, o provedor Console exibe as mensagens no console e o provedor do Serviço de Aplicativo do Azure pode armazená-las no armazenamento de blobs do Azure.</span><span class="sxs-lookup"><span data-stu-id="7984a-127">For example, the Console provider displays messages on the console, and the Azure App Service provider can store them in Azure blob storage.</span></span>

<span data-ttu-id="7984a-128">Para usar um provedor, instale o pacote NuGet e chame o método de extensão do provedor em uma instância do [ILoggerFactory](/dotnet/api/microsoft.extensions.logging.iloggerfactory), conforme mostrado no exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="7984a-128">To use a provider, install its NuGet package and call the provider's extension method on an instance of [ILoggerFactory](/dotnet/api/microsoft.extensions.logging.iloggerfactory), as shown in the following example:</span></span>

[!code-csharp[](index/samples/1.x/TodoApiSample//Startup.cs?name=snippet_AddConsoleAndDebug&highlight=3,5-7)]

<span data-ttu-id="7984a-129">A [DI](xref:fundamentals/dependency-injection) (injeção de dependência) do ASP.NET Core fornece a instância de `ILoggerFactory`.</span><span class="sxs-lookup"><span data-stu-id="7984a-129">ASP.NET Core [dependency injection](xref:fundamentals/dependency-injection) (DI) provides the `ILoggerFactory` instance.</span></span> <span data-ttu-id="7984a-130">Os métodos de extensão `AddConsole` e `AddDebug` são definidos nos pacotes [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) e [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/).</span><span class="sxs-lookup"><span data-stu-id="7984a-130">The `AddConsole` and `AddDebug` extension methods are defined in the [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) and [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/) packages.</span></span> <span data-ttu-id="7984a-131">Cada método de extensão chama o método `ILoggerFactory.AddProvider`, passando uma instância do provedor.</span><span class="sxs-lookup"><span data-stu-id="7984a-131">Each extension method calls the `ILoggerFactory.AddProvider` method, passing in an instance of the provider.</span></span>

> [!NOTE]
> <span data-ttu-id="7984a-132">O [aplicativo de exemplo 1.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/samples/1.x) adiciona provedores de log no método `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="7984a-132">The [1.x sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/samples/1.x) adds logging providers in the `Startup.Configure` method.</span></span> <span data-ttu-id="7984a-133">Se você quiser obter saída de log do código que é executado anteriormente, adicione provedores de log no construtor de classe `Startup`.</span><span class="sxs-lookup"><span data-stu-id="7984a-133">If you want to obtain log output from code that executes earlier, add logging providers in the `Startup` class constructor.</span></span>

::: moniker-end

<span data-ttu-id="7984a-134">Saiba mais sobre os [provedores de log internos](#built-in-logging-providers) e encontre links para [provedores de log de terceiros](#third-party-logging-providers) mais adiante no artigo.</span><span class="sxs-lookup"><span data-stu-id="7984a-134">Learn more about the [built-in logging providers](#built-in-logging-providers) and find links to [third-party logging providers](#third-party-logging-providers) later in the article.</span></span>

## <a name="configuration"></a><span data-ttu-id="7984a-135">Configuração</span><span class="sxs-lookup"><span data-stu-id="7984a-135">Configuration</span></span>

<span data-ttu-id="7984a-136">A configuração do provedor de logs é fornecida por um ou mais provedores de sincronização:</span><span class="sxs-lookup"><span data-stu-id="7984a-136">Logging provider configuration is provided by one or more configuration providers:</span></span>

* <span data-ttu-id="7984a-137">Formatos de arquivo (INI, JSON e XML).</span><span class="sxs-lookup"><span data-stu-id="7984a-137">File formats (INI, JSON, and XML).</span></span>
* <span data-ttu-id="7984a-138">Argumentos de linha de comando.</span><span class="sxs-lookup"><span data-stu-id="7984a-138">Command-line arguments.</span></span>
* <span data-ttu-id="7984a-139">Variáveis de ambiente.</span><span class="sxs-lookup"><span data-stu-id="7984a-139">Environment variables.</span></span>
* <span data-ttu-id="7984a-140">Objetos do .NET na memória.</span><span class="sxs-lookup"><span data-stu-id="7984a-140">In-memory .NET objects.</span></span>
* <span data-ttu-id="7984a-141">O armazenamento do [Secret Manager](xref:security/app-secrets) não criptografado.</span><span class="sxs-lookup"><span data-stu-id="7984a-141">The unencrypted [Secret Manager](xref:security/app-secrets) storage.</span></span>
* <span data-ttu-id="7984a-142">Um repositório de usuário criptografado, como o [Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="7984a-142">An encrypted user store, such as [Azure Key Vault](xref:security/key-vault-configuration).</span></span>
* <span data-ttu-id="7984a-143">Provedores personalizados (instalados ou criados).</span><span class="sxs-lookup"><span data-stu-id="7984a-143">Custom providers (installed or created).</span></span>

<span data-ttu-id="7984a-144">Por exemplo, a configuração de log geralmente é fornecida pela seção `Logging` dos arquivos de configurações do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="7984a-144">For example, logging configuration is commonly provided by the `Logging` section of app settings files.</span></span> <span data-ttu-id="7984a-145">O exemplo a seguir mostra o conteúdo de um típico arquivo *appsettings.Development.json*:</span><span class="sxs-lookup"><span data-stu-id="7984a-145">The following example shows the contents of a typical *appsettings.Development.json* file:</span></span>

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

<span data-ttu-id="7984a-146">Chaves `LogLevel` representam nomes de log.</span><span class="sxs-lookup"><span data-stu-id="7984a-146">`LogLevel` keys represent log names.</span></span> <span data-ttu-id="7984a-147">A chave `Default` aplica-se a logs não listados de forma explícita.</span><span class="sxs-lookup"><span data-stu-id="7984a-147">The `Default` key applies to logs not explicitly listed.</span></span> <span data-ttu-id="7984a-148">O valor representa o [nível de log](#log-level) aplicado ao log fornecido.</span><span class="sxs-lookup"><span data-stu-id="7984a-148">The value represents the [log level](#log-level) applied to the given log.</span></span> <span data-ttu-id="7984a-149">Chaves de log que definem `IncludeScopes` (`Console` no exemplo) especificam se os [escopos de log](#log-scopes) estão habilitados para o log indicado.</span><span class="sxs-lookup"><span data-stu-id="7984a-149">Log keys that set `IncludeScopes` (`Console` in the example), specify if [log scopes](#log-scopes) are enabled for the indicated log.</span></span>

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

<span data-ttu-id="7984a-150">Chaves `LogLevel` representam nomes de log.</span><span class="sxs-lookup"><span data-stu-id="7984a-150">`LogLevel` keys represent log names.</span></span> <span data-ttu-id="7984a-151">A chave `Default` aplica-se a logs não listados de forma explícita.</span><span class="sxs-lookup"><span data-stu-id="7984a-151">The `Default` key applies to logs not explicitly listed.</span></span> <span data-ttu-id="7984a-152">O valor representa o [nível de log](#log-level) aplicado ao log fornecido.</span><span class="sxs-lookup"><span data-stu-id="7984a-152">The value represents the [log level](#log-level) applied to the given log.</span></span>

::: moniker-end

<span data-ttu-id="7984a-153">Saiba mais sobre como implementar provedores de configuração em <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="7984a-153">For information on implementing configuration providers, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="sample-logging-output"></a><span data-ttu-id="7984a-154">Exemplo de saída de registro em log</span><span class="sxs-lookup"><span data-stu-id="7984a-154">Sample logging output</span></span>

<span data-ttu-id="7984a-155">Com o código de exemplo mostrado na seção anterior, você verá logs no console ao executar através da linha de comando.</span><span class="sxs-lookup"><span data-stu-id="7984a-155">With the sample code shown in the preceding section, you'll see logs in the console when you run from the command line.</span></span> <span data-ttu-id="7984a-156">Aqui está um exemplo da saída do console:</span><span class="sxs-lookup"><span data-stu-id="7984a-156">Here's an example of console output:</span></span>

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

<span data-ttu-id="7984a-157">Esses logs foram criados acessando `http://localhost:5000/api/todo/0`, que dispara a execução das duas chamadas `ILogger` mostradas na seção anterior.</span><span class="sxs-lookup"><span data-stu-id="7984a-157">These logs were created by going to `http://localhost:5000/api/todo/0`, which triggers execution of both `ILogger` calls shown in the preceding section.</span></span>

<span data-ttu-id="7984a-158">Aqui está um exemplo de como os mesmos logs aparecem na janela Depuração quando você executa o aplicativo de exemplo no Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="7984a-158">Here's an example of the same logs as they appear in the Debug window when you run the sample application in Visual Studio:</span></span>

```console
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request starting HTTP/1.1 GET http://localhost:53104/api/todo/0  
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
TodoApi.Controllers.TodoController:Information: Getting item 0
TodoApi.Controllers.TodoController:Warning: GetById(0) NOT FOUND
Microsoft.AspNetCore.Mvc.StatusCodeResult:Information: Executing HttpStatusCodeResult, setting HTTP status code 404
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 152.5657ms
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request finished in 316.3195ms 404
```

<span data-ttu-id="7984a-159">Os logs criados pelas chamadas `ILogger` mostradas na seção anterior começam com "TodoApi.Controllers.TodoController".</span><span class="sxs-lookup"><span data-stu-id="7984a-159">The logs that were created by the `ILogger` calls shown in the preceding section begin with "TodoApi.Controllers.TodoController".</span></span> <span data-ttu-id="7984a-160">Os logs que começam com categorias "Microsoft" são do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7984a-160">The logs that begin with "Microsoft" categories are from ASP.NET Core.</span></span> <span data-ttu-id="7984a-161">O próprio ASP.NET Core e o código do aplicativo estão usando a mesma API de registro em log e os mesmos provedores de log.</span><span class="sxs-lookup"><span data-stu-id="7984a-161">ASP.NET Core itself and your application code are using the same logging API and the same logging providers.</span></span>

<span data-ttu-id="7984a-162">O restante deste artigo explica alguns detalhes e opções para registro em log.</span><span class="sxs-lookup"><span data-stu-id="7984a-162">The remainder of this article explains some details and options for logging.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="7984a-163">Pacotes NuGet</span><span class="sxs-lookup"><span data-stu-id="7984a-163">NuGet packages</span></span>

<span data-ttu-id="7984a-164">As interfaces `ILogger` e `ILoggerFactory` estão em [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/) e as implementações padrão para elas estão em [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).</span><span class="sxs-lookup"><span data-stu-id="7984a-164">The `ILogger` and `ILoggerFactory` interfaces are in [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), and default implementations for them are in [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).</span></span>

## <a name="log-category"></a><span data-ttu-id="7984a-165">Categoria de log</span><span class="sxs-lookup"><span data-stu-id="7984a-165">Log category</span></span>

<span data-ttu-id="7984a-166">Um *categoria* é incluída com cada log que você cria.</span><span class="sxs-lookup"><span data-stu-id="7984a-166">A *category* is included with each log that you create.</span></span> <span data-ttu-id="7984a-167">Você especifica a categoria ao criar um objeto `ILogger`.</span><span class="sxs-lookup"><span data-stu-id="7984a-167">You specify the category when you create an `ILogger` object.</span></span> <span data-ttu-id="7984a-168">A categoria pode ser qualquer cadeia de caracteres, mas uma convenção é usar o nome totalmente qualificado da classe da qual os logs são gravados.</span><span class="sxs-lookup"><span data-stu-id="7984a-168">The category may be any string, but a convention is to use the fully qualified name of the class from which the logs are written.</span></span> <span data-ttu-id="7984a-169">Por exemplo: "TodoApi.Controllers.TodoController".</span><span class="sxs-lookup"><span data-stu-id="7984a-169">For example: "TodoApi.Controllers.TodoController".</span></span>

<span data-ttu-id="7984a-170">Você pode especificar a categoria como uma cadeia de caracteres ou usar um método de extensão que deriva a categoria do tipo.</span><span class="sxs-lookup"><span data-stu-id="7984a-170">You can specify the category as a string or use an extension method that derives the category from the type.</span></span> <span data-ttu-id="7984a-171">Para especificar a categoria como uma cadeia de caracteres, chame `CreateLogger` em uma instância de `ILoggerFactory`, conforme mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="7984a-171">To specify the category as a string, call `CreateLogger` on an `ILoggerFactory` instance, as shown below.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

::: moniker-end

<span data-ttu-id="7984a-172">Na maioria das vezes será mais fácil usar `ILogger<T>`, conforme mostrado no exemplo a seguir.</span><span class="sxs-lookup"><span data-stu-id="7984a-172">Most of the time, it will be easier to use `ILogger<T>`, as in the following example.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

::: moniker-end

<span data-ttu-id="7984a-173">Isso é equivalente a chamar `CreateLogger` com o nome de tipo totalmente qualificado de `T`.</span><span class="sxs-lookup"><span data-stu-id="7984a-173">This is equivalent to calling `CreateLogger` with the fully qualified type name of `T`.</span></span>

## <a name="log-level"></a><span data-ttu-id="7984a-174">Nível de log</span><span class="sxs-lookup"><span data-stu-id="7984a-174">Log level</span></span>

<span data-ttu-id="7984a-175">Sempre que você grava um log, você especifica o [LogLevel](/dotnet/api/microsoft.extensions.logging.logLevel).</span><span class="sxs-lookup"><span data-stu-id="7984a-175">Each time you write a log, you specify its [LogLevel](/dotnet/api/microsoft.extensions.logging.logLevel).</span></span> <span data-ttu-id="7984a-176">O nível de log indica o grau de gravidade ou importância.</span><span class="sxs-lookup"><span data-stu-id="7984a-176">The log level indicates the degree of severity or importance.</span></span> <span data-ttu-id="7984a-177">Por exemplo, você pode gravar um log `Information` quando um método é finalizado normalmente, um log `Warning` quando um método retorna um código de retorno 404 e um log `Error` ao capturar uma exceção inesperada.</span><span class="sxs-lookup"><span data-stu-id="7984a-177">For example, you might write an `Information` log when a method ends normally, a `Warning` log when a method returns a 404 return code, and an `Error` log when you catch an unexpected exception.</span></span>

<span data-ttu-id="7984a-178">No exemplo de código a seguir, os nomes dos métodos (por exemplo, `LogWarning`) especificam o nível de log.</span><span class="sxs-lookup"><span data-stu-id="7984a-178">In the following code example, the names of the methods (for example, `LogWarning`) specify the log level.</span></span> <span data-ttu-id="7984a-179">O primeiro parâmetro é a [ID de evento de log](#log-event-id).</span><span class="sxs-lookup"><span data-stu-id="7984a-179">The first parameter is the [Log event ID](#log-event-id).</span></span> <span data-ttu-id="7984a-180">O segundo parâmetro é um [modelo de mensagem](#log-message-template) com espaços reservados para valores de argumento fornecidos pelos parâmetros de método restantes.</span><span class="sxs-lookup"><span data-stu-id="7984a-180">The second parameter is a [message template](#log-message-template) with placeholders for argument values provided by the remaining method parameters.</span></span> <span data-ttu-id="7984a-181">Os parâmetros de método serão explicados com mais detalhes posteriormente neste artigo.</span><span class="sxs-lookup"><span data-stu-id="7984a-181">The method parameters are explained in more detail later in this article.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

<span data-ttu-id="7984a-182">Os métodos de log que incluem o nível no nome do método são [métodos de extensão para ILogger](/dotnet/api/microsoft.extensions.logging.loggerextensions).</span><span class="sxs-lookup"><span data-stu-id="7984a-182">Log methods that include the level in the method name are [extension methods for ILogger](/dotnet/api/microsoft.extensions.logging.loggerextensions).</span></span> <span data-ttu-id="7984a-183">Nos bastidores, esses métodos chamam um método `Log` que recebe um parâmetro `LogLevel`.</span><span class="sxs-lookup"><span data-stu-id="7984a-183">Behind the scenes, these methods call a `Log` method that takes a `LogLevel` parameter.</span></span> <span data-ttu-id="7984a-184">Você pode chamar o método `Log` diretamente em vez de um desses métodos de extensão, mas a sintaxe é relativamente complicada.</span><span class="sxs-lookup"><span data-stu-id="7984a-184">You can call the `Log` method directly rather than one of these extension methods, but the syntax is relatively complicated.</span></span> <span data-ttu-id="7984a-185">Para obter mais informações, consulte a [interface ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) e o [código-fonte de extensões de agente](https://github.com/aspnet/Logging/blob/master/src/Microsoft.Extensions.Logging.Abstractions/LoggerExtensions.cs).</span><span class="sxs-lookup"><span data-stu-id="7984a-185">For more information, see the [ILogger interface](/dotnet/api/microsoft.extensions.logging.ilogger) and the [logger extensions source code](https://github.com/aspnet/Logging/blob/master/src/Microsoft.Extensions.Logging.Abstractions/LoggerExtensions.cs).</span></span>

<span data-ttu-id="7984a-186">O ASP.NET Core define os seguintes [níveis de log](/dotnet/api/microsoft.extensions.logging.loglevel), ordenados aqui da menor para a maior gravidade.</span><span class="sxs-lookup"><span data-stu-id="7984a-186">ASP.NET Core defines the following [log levels](/dotnet/api/microsoft.extensions.logging.loglevel), ordered here from least to highest severity.</span></span>

* <span data-ttu-id="7984a-187">Trace = 0</span><span class="sxs-lookup"><span data-stu-id="7984a-187">Trace = 0</span></span>

  <span data-ttu-id="7984a-188">Para obter informações valiosas somente para um desenvolvedor que esteja depurando um problema.</span><span class="sxs-lookup"><span data-stu-id="7984a-188">For information that's valuable only to a developer debugging an issue.</span></span> <span data-ttu-id="7984a-189">Essas mensagens podem conter dados confidenciais de aplicativos e, portanto, não devem ser habilitadas em um ambiente de produção.</span><span class="sxs-lookup"><span data-stu-id="7984a-189">These messages may contain sensitive application data and so shouldn't be enabled in a production environment.</span></span> <span data-ttu-id="7984a-190">*Desabilitado por padrão.*</span><span class="sxs-lookup"><span data-stu-id="7984a-190">*Disabled by default.*</span></span> <span data-ttu-id="7984a-191">Exemplo: `Credentials: {"User":"someuser", "Password":"P@ssword"}`</span><span class="sxs-lookup"><span data-stu-id="7984a-191">Example: `Credentials: {"User":"someuser", "Password":"P@ssword"}`</span></span>

* <span data-ttu-id="7984a-192">Debug = 1</span><span class="sxs-lookup"><span data-stu-id="7984a-192">Debug = 1</span></span>

  <span data-ttu-id="7984a-193">Para informações que tenham utilidade de curto prazo durante o desenvolvimento e a depuração.</span><span class="sxs-lookup"><span data-stu-id="7984a-193">For information that has short-term usefulness during development and debugging.</span></span> <span data-ttu-id="7984a-194">Exemplo: `Entering method Configure with flag set to true.` Você normalmente não habilitaria logs de nível `Debug` em produção, a menos que estivesse solucionando problemas, devido ao alto volume de logs.</span><span class="sxs-lookup"><span data-stu-id="7984a-194">Example: `Entering method Configure with flag set to true.` You typically wouldn't enable `Debug` level logs in production unless you are troubleshooting, due to the high volume of logs.</span></span>

* <span data-ttu-id="7984a-195">Information = 2</span><span class="sxs-lookup"><span data-stu-id="7984a-195">Information = 2</span></span>

  <span data-ttu-id="7984a-196">Para rastrear o fluxo geral do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="7984a-196">For tracking the general flow of the application.</span></span> <span data-ttu-id="7984a-197">Esses logs normalmente têm algum valor a longo prazo.</span><span class="sxs-lookup"><span data-stu-id="7984a-197">These logs typically have some long-term value.</span></span> <span data-ttu-id="7984a-198">Exemplo: `Request received for path /api/todo`</span><span class="sxs-lookup"><span data-stu-id="7984a-198">Example: `Request received for path /api/todo`</span></span>

* <span data-ttu-id="7984a-199">Warning = 3</span><span class="sxs-lookup"><span data-stu-id="7984a-199">Warning = 3</span></span>

  <span data-ttu-id="7984a-200">Para eventos anormais ou inesperados no fluxo de aplicativo.</span><span class="sxs-lookup"><span data-stu-id="7984a-200">For abnormal or unexpected events in the application flow.</span></span> <span data-ttu-id="7984a-201">Eles podem incluir erros ou outras condições que não fazem com que o aplicativo pare, mas que talvez precisem ser investigados.</span><span class="sxs-lookup"><span data-stu-id="7984a-201">These may include errors or other conditions that don't cause the application to stop, but which may need to be investigated.</span></span> <span data-ttu-id="7984a-202">Exceções manipuladas são um local comum para usar o nível de log `Warning`.</span><span class="sxs-lookup"><span data-stu-id="7984a-202">Handled exceptions are a common place to use the `Warning` log level.</span></span> <span data-ttu-id="7984a-203">Exemplo: `FileNotFoundException for file quotes.txt.`</span><span class="sxs-lookup"><span data-stu-id="7984a-203">Example: `FileNotFoundException for file quotes.txt.`</span></span>

* <span data-ttu-id="7984a-204">Error = 4</span><span class="sxs-lookup"><span data-stu-id="7984a-204">Error = 4</span></span>

  <span data-ttu-id="7984a-205">Para erros e exceções que não podem ser manipulados.</span><span class="sxs-lookup"><span data-stu-id="7984a-205">For errors and exceptions that cannot be handled.</span></span> <span data-ttu-id="7984a-206">Essas mensagens indicam uma falha na atividade ou na operação atual (como a solicitação HTTP atual) e não uma falha em todo o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="7984a-206">These messages indicate a failure in the current activity or operation (such as the current HTTP request), not an application-wide failure.</span></span> <span data-ttu-id="7984a-207">Mensagem de log de exemplo:`Cannot insert record due to duplicate key violation.`</span><span class="sxs-lookup"><span data-stu-id="7984a-207">Example log message: `Cannot insert record due to duplicate key violation.`</span></span>

* <span data-ttu-id="7984a-208">Critical = 5</span><span class="sxs-lookup"><span data-stu-id="7984a-208">Critical = 5</span></span>

  <span data-ttu-id="7984a-209">Para falhas que exigem atenção imediata.</span><span class="sxs-lookup"><span data-stu-id="7984a-209">For failures that require immediate attention.</span></span> <span data-ttu-id="7984a-210">Exemplos: cenários de perda de dados, espaço em disco insuficiente.</span><span class="sxs-lookup"><span data-stu-id="7984a-210">Examples: data loss scenarios, out of disk space.</span></span>

<span data-ttu-id="7984a-211">Você pode usar o nível de log para controlar a quantidade de saída de log que é gravada em uma mídia de armazenamento específica ou em uma janela de exibição.</span><span class="sxs-lookup"><span data-stu-id="7984a-211">You can use the log level to control how much log output is written to a particular storage medium or display window.</span></span> <span data-ttu-id="7984a-212">Por exemplo, em produção, você pode desejar que todos os logs de nível `Information` e inferior vão para um armazenamento de dados de volume e que todos os logs de nível `Warning` e superior vão para um armazenamento de dados de valor.</span><span class="sxs-lookup"><span data-stu-id="7984a-212">For example, in production you might want all logs of `Information` level and lower to go to a volume data store, and all logs of `Warning` level and higher to go to a value data store.</span></span> <span data-ttu-id="7984a-213">Durante o desenvolvimento, você normalmente poderia enviar os logs de gravidade `Warning` ou mais alta para o console.</span><span class="sxs-lookup"><span data-stu-id="7984a-213">During development, you might normally send logs of `Warning` or higher severity to the console.</span></span> <span data-ttu-id="7984a-214">Então, quando precisar solucionar problemas, você poderá adicionar o nível `Debug`.</span><span class="sxs-lookup"><span data-stu-id="7984a-214">Then when you need to troubleshoot, you can add `Debug` level.</span></span> <span data-ttu-id="7984a-215">A seção [Filtragem de log](#log-filtering) mais adiante neste artigo explicará como controlar os níveis de log que um provedor manipula.</span><span class="sxs-lookup"><span data-stu-id="7984a-215">The [Log filtering](#log-filtering) section later in this article explains how to control which log levels a provider handles.</span></span>

<span data-ttu-id="7984a-216">A estrutura do ASP.NET Core grava logs de nível `Debug` para eventos de estrutura.</span><span class="sxs-lookup"><span data-stu-id="7984a-216">The ASP.NET Core framework writes `Debug` level logs for framework events.</span></span> <span data-ttu-id="7984a-217">Os exemplos de log anteriores neste artigo excluíram logs abaixo do nível `Information`, portanto, os logs de nível `Debug` não foram mostrados.</span><span class="sxs-lookup"><span data-stu-id="7984a-217">The log examples earlier in this article excluded logs below `Information` level, so no `Debug` level logs were shown.</span></span> <span data-ttu-id="7984a-218">Aqui está um exemplo de logs do console, caso você execute o aplicativo de exemplo configurado para mostrar logs de nível `Debug` e superiores para o provedor de console.</span><span class="sxs-lookup"><span data-stu-id="7984a-218">Here's an example of console logs if you run the sample application configured to show `Debug` and higher logs for the console provider.</span></span>

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

## <a name="log-event-id"></a><span data-ttu-id="7984a-219">ID de evento de log</span><span class="sxs-lookup"><span data-stu-id="7984a-219">Log event ID</span></span>

<span data-ttu-id="7984a-220">Sempre que você grava um log, você pode especificar uma *ID de evento*.</span><span class="sxs-lookup"><span data-stu-id="7984a-220">Each time you write a log, you can specify an *event ID*.</span></span> <span data-ttu-id="7984a-221">O aplicativo de exemplo faz isso usando uma classe `LoggingEvents` definida localmente:</span><span class="sxs-lookup"><span data-stu-id="7984a-221">The sample app does this by using a locally-defined `LoggingEvents` class:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/samples/2.x/TodoApiSample/Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/samples/1.x/TodoApiSample/Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

::: moniker-end

<span data-ttu-id="7984a-222">Uma ID de evento é um valor inteiro que pode ser usado para associar um conjunto de eventos registrados.</span><span class="sxs-lookup"><span data-stu-id="7984a-222">An event ID is an integer value that you can use to associate a set of logged events with one another.</span></span> <span data-ttu-id="7984a-223">Por exemplo, um log para adicionar um item a um carrinho de compras pode ser ter a ID de evento 1000 e um log para concluir uma compra pode ter a ID de evento 1001.</span><span class="sxs-lookup"><span data-stu-id="7984a-223">For instance, a log for adding an item to a shopping cart could be event ID 1000 and a log for completing a purchase could be event ID 1001.</span></span>

<span data-ttu-id="7984a-224">Na saída do registro em log, a ID do evento pode ser armazenada em um campo ou incluída na mensagem de texto, dependendo do provedor.</span><span class="sxs-lookup"><span data-stu-id="7984a-224">In logging output, the event ID may be stored in a field or included in the text message, depending on the provider.</span></span> <span data-ttu-id="7984a-225">O provedor Depuração não mostra as IDs de evento, mas o provedor do console sim, entre colchetes depois da categoria:</span><span class="sxs-lookup"><span data-stu-id="7984a-225">The Debug provider doesn't show event IDs, but the console provider shows them in brackets after the category:</span></span>

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-template"></a><span data-ttu-id="7984a-226">Modelo de mensagem de log</span><span class="sxs-lookup"><span data-stu-id="7984a-226">Log message template</span></span>

<span data-ttu-id="7984a-227">Sempre que você grava uma mensagem de log, você fornece um modelo de mensagem.</span><span class="sxs-lookup"><span data-stu-id="7984a-227">Each time you write a log message, you provide a message template.</span></span> <span data-ttu-id="7984a-228">O modelo de mensagem pode ser uma cadeia de caracteres ou pode conter espaços reservados nomeados, nos quais valores de argumento serão colocados.</span><span class="sxs-lookup"><span data-stu-id="7984a-228">The message template can be a string or it can contain named placeholders into which argument values are placed.</span></span> <span data-ttu-id="7984a-229">O modelo não é uma cadeia de caracteres de formatação e os espaços reservados devem ser nomeados, não numerados.</span><span class="sxs-lookup"><span data-stu-id="7984a-229">The template isn't a format string, and placeholders should be named, not numbered.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

<span data-ttu-id="7984a-230">A ordem dos espaços reservados e não de seus nomes, determina quais parâmetros serão usados para fornecer seus valores.</span><span class="sxs-lookup"><span data-stu-id="7984a-230">The order of placeholders, not their names, determines which parameters are used to provide their values.</span></span> <span data-ttu-id="7984a-231">Se você tiver o seguinte código:</span><span class="sxs-lookup"><span data-stu-id="7984a-231">If you have the following code:</span></span>

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

<span data-ttu-id="7984a-232">A mensagem de log resultante terá esta aparência:</span><span class="sxs-lookup"><span data-stu-id="7984a-232">The resulting log message looks like this:</span></span>

```
Parameter values: parm1, parm2
```

<span data-ttu-id="7984a-233">A estrutura de registros realiza a formatação de mensagens dessa maneira para possibilitar que os provedores de log implementem o [registro em log semântico, também conhecido como registro em log estruturado](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="7984a-233">The logging framework does message formatting in this way to make it possible for logging providers to implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span> <span data-ttu-id="7984a-234">Como os próprios argumentos são passados para o sistema de registro em log, não apenas o modelo de mensagem formatada, mas os provedores de log, também poderão armazenar os valores de parâmetro como campos, além do modelo de mensagem.</span><span class="sxs-lookup"><span data-stu-id="7984a-234">Because the arguments themselves are passed to the logging system, not just the formatted message template, logging providers can store the parameter values as fields in addition to the message template.</span></span> <span data-ttu-id="7984a-235">Se você estiver direcionando a saída de log para o Armazenamento de Tabelas do Azure e sua chamada de método do agente tiver esta aparência:</span><span class="sxs-lookup"><span data-stu-id="7984a-235">If you're directing your log output to Azure Table Storage and your logger method call looks like this:</span></span>

```csharp
_logger.LogInformation("Getting item {ID} at {RequestTime}", id, DateTime.Now);
```

<span data-ttu-id="7984a-236">Cada entidade da Tabela do Azure poderá ter propriedades `ID` e `RequestTime`, o que simplificará as consultas nos dados de log.</span><span class="sxs-lookup"><span data-stu-id="7984a-236">Each Azure Table entity can have `ID` and `RequestTime` properties, which simplifies queries on log data.</span></span> <span data-ttu-id="7984a-237">Você poderá encontrar todos os logs em um determinado intervalo de `RequestTime` sem a necessidade de analisar o tempo limite da mensagem de texto.</span><span class="sxs-lookup"><span data-stu-id="7984a-237">You can find all logs within a particular `RequestTime` range without the need to parse the time out of the text message.</span></span>

## <a name="logging-exceptions"></a><span data-ttu-id="7984a-238">Exceções de registro em log</span><span class="sxs-lookup"><span data-stu-id="7984a-238">Logging exceptions</span></span>

<span data-ttu-id="7984a-239">Os métodos de agente têm sobrecargas que permitem que você passe uma exceção, como no exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="7984a-239">The logger methods have overloads that let you pass in an exception, as in the following example:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

::: moniker-end

<span data-ttu-id="7984a-240">Provedores diferentes manipulam as informações de exceção de maneiras diferentes.</span><span class="sxs-lookup"><span data-stu-id="7984a-240">Different providers handle the exception information in different ways.</span></span> <span data-ttu-id="7984a-241">Aqui está um exemplo da saída do provedor Depuração do código mostrado acima.</span><span class="sxs-lookup"><span data-stu-id="7984a-241">Here's an example of Debug provider output from the code shown above.</span></span>

```
TodoApi.Controllers.TodoController:Warning: GetById(036dd898-fb01-47e8-9a65-f92eb73cf924) NOT FOUND

System.Exception: Item not found exception.
 at TodoApi.Controllers.TodoController.GetById(String id) in C:\logging\sample\src\TodoApi\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a><span data-ttu-id="7984a-242">Filtragem de log</span><span class="sxs-lookup"><span data-stu-id="7984a-242">Log filtering</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="7984a-243">Você pode especificar um nível de log mínimo para um provedor e uma categoria específicos ou para todos os provedores ou todas as categorias.</span><span class="sxs-lookup"><span data-stu-id="7984a-243">You can specify a minimum log level for a specific provider and category or for all providers or all categories.</span></span> <span data-ttu-id="7984a-244">Os logs abaixo do nível mínimo não serão passados para esse provedor, para que não sejam exibidos ou armazenados.</span><span class="sxs-lookup"><span data-stu-id="7984a-244">Any logs below the minimum level aren't passed to that provider, so they don't get displayed or stored.</span></span>

<span data-ttu-id="7984a-245">Se quiser suprimir todos os logs, você poderá especificar `LogLevel.None` como o nível de log mínimo.</span><span class="sxs-lookup"><span data-stu-id="7984a-245">If you want to suppress all logs, you can specify `LogLevel.None` as the minimum log level.</span></span> <span data-ttu-id="7984a-246">O valor inteiro de `LogLevel.None` é 6, que é maior do que `LogLevel.Critical` (5).</span><span class="sxs-lookup"><span data-stu-id="7984a-246">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

### <a name="create-filter-rules-in-configuration"></a><span data-ttu-id="7984a-247">Criar regras de filtro na configuração</span><span class="sxs-lookup"><span data-stu-id="7984a-247">Create filter rules in configuration</span></span>

<span data-ttu-id="7984a-248">Os modelos de projeto criam código que chama `CreateDefaultBuilder` para configurar o registro em log para os provedores Console e Depuração.</span><span class="sxs-lookup"><span data-stu-id="7984a-248">The project templates create code that calls `CreateDefaultBuilder` to set up logging for the Console and Debug providers.</span></span> <span data-ttu-id="7984a-249">O método `CreateDefaultBuilder` também configura o registro em log para procurar a configuração em uma seção `Logging`, usando código semelhante ao seguinte:</span><span class="sxs-lookup"><span data-stu-id="7984a-249">The `CreateDefaultBuilder` method also sets up logging to look for configuration in a `Logging` section, using code like the following:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_ExpandDefault&highlight=15)]

<span data-ttu-id="7984a-250">Os dados de configuração especificam níveis de log mínimo por provedor e por categoria, como no exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="7984a-250">The configuration data specifies minimum log levels by provider and category, as in the following example:</span></span>

[!code-json[](index/samples/2.x/TodoApiSample/appsettings.json)]

<span data-ttu-id="7984a-251">Este JSON cria seis regras de filtro, uma para o provedor Depuração, quatro para o provedor Console e uma que se aplica a todos os provedores.</span><span class="sxs-lookup"><span data-stu-id="7984a-251">This JSON creates six filter rules, one for the Debug provider, four for the Console provider, and one that applies to all providers.</span></span> <span data-ttu-id="7984a-252">Você verá posteriormente como apenas uma dessas regras é escolhida para cada provedor quando um objeto `ILogger` é criado.</span><span class="sxs-lookup"><span data-stu-id="7984a-252">You'll see later how just one of these rules is chosen for each provider when an `ILogger` object is created.</span></span>

### <a name="filter-rules-in-code"></a><span data-ttu-id="7984a-253">Regras de filtro no código</span><span class="sxs-lookup"><span data-stu-id="7984a-253">Filter rules in code</span></span>

<span data-ttu-id="7984a-254">Você pode registrar regras de filtro no código, conforme mostrado no exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="7984a-254">You can register filter rules in code, as shown in the following example:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

<span data-ttu-id="7984a-255">O segundo `AddFilter` especifica o provedor Depuração usando seu nome de tipo.</span><span class="sxs-lookup"><span data-stu-id="7984a-255">The second `AddFilter` specifies the Debug provider by using its type name.</span></span> <span data-ttu-id="7984a-256">O primeiro `AddFilter` se aplica a todos os provedores porque ele não especifica um tipo de provedor.</span><span class="sxs-lookup"><span data-stu-id="7984a-256">The first `AddFilter` applies to all providers because it doesn't specify a provider type.</span></span>

### <a name="how-filtering-rules-are-applied"></a><span data-ttu-id="7984a-257">Como as regras de filtragem são aplicadas</span><span class="sxs-lookup"><span data-stu-id="7984a-257">How filtering rules are applied</span></span>

<span data-ttu-id="7984a-258">Os dados de configuração e o código `AddFilter`, mostrados nos exemplos anteriores, criam as regras mostradas na tabela a seguir.</span><span class="sxs-lookup"><span data-stu-id="7984a-258">The configuration data and the `AddFilter` code shown in the preceding examples create the rules shown in the following table.</span></span> <span data-ttu-id="7984a-259">As primeiras seis vêm do exemplo de configuração e as últimas duas vêm do exemplo de código.</span><span class="sxs-lookup"><span data-stu-id="7984a-259">The first six come from the configuration example and the last two come from the code example.</span></span>

| <span data-ttu-id="7984a-260">Número</span><span class="sxs-lookup"><span data-stu-id="7984a-260">Number</span></span> | <span data-ttu-id="7984a-261">Provider</span><span class="sxs-lookup"><span data-stu-id="7984a-261">Provider</span></span>      | <span data-ttu-id="7984a-262">Categorias que começam com...</span><span class="sxs-lookup"><span data-stu-id="7984a-262">Categories that begin with ...</span></span>          | <span data-ttu-id="7984a-263">Nível de log mínimo</span><span class="sxs-lookup"><span data-stu-id="7984a-263">Minimum log level</span></span> |
| :----: | ------------- | --------------------------------------- | ----------------- |
| <span data-ttu-id="7984a-264">1</span><span class="sxs-lookup"><span data-stu-id="7984a-264">1</span></span>      | <span data-ttu-id="7984a-265">Depurar</span><span class="sxs-lookup"><span data-stu-id="7984a-265">Debug</span></span>         | <span data-ttu-id="7984a-266">Todas as categorias</span><span class="sxs-lookup"><span data-stu-id="7984a-266">All categories</span></span>                          | <span data-ttu-id="7984a-267">Informações</span><span class="sxs-lookup"><span data-stu-id="7984a-267">Information</span></span>       |
| <span data-ttu-id="7984a-268">2</span><span class="sxs-lookup"><span data-stu-id="7984a-268">2</span></span>      | <span data-ttu-id="7984a-269">Console</span><span class="sxs-lookup"><span data-stu-id="7984a-269">Console</span></span>       | <span data-ttu-id="7984a-270">Microsoft.AspNetCore.Mvc.Razor.Internal</span><span class="sxs-lookup"><span data-stu-id="7984a-270">Microsoft.AspNetCore.Mvc.Razor.Internal</span></span> | <span data-ttu-id="7984a-271">Aviso</span><span class="sxs-lookup"><span data-stu-id="7984a-271">Warning</span></span>           |
| <span data-ttu-id="7984a-272">3</span><span class="sxs-lookup"><span data-stu-id="7984a-272">3</span></span>      | <span data-ttu-id="7984a-273">Console</span><span class="sxs-lookup"><span data-stu-id="7984a-273">Console</span></span>       | <span data-ttu-id="7984a-274">Microsoft.AspNetCore.Mvc.Razor.Razor</span><span class="sxs-lookup"><span data-stu-id="7984a-274">Microsoft.AspNetCore.Mvc.Razor.Razor</span></span>    | <span data-ttu-id="7984a-275">Depurar</span><span class="sxs-lookup"><span data-stu-id="7984a-275">Debug</span></span>             |
| <span data-ttu-id="7984a-276">4</span><span class="sxs-lookup"><span data-stu-id="7984a-276">4</span></span>      | <span data-ttu-id="7984a-277">Console</span><span class="sxs-lookup"><span data-stu-id="7984a-277">Console</span></span>       | <span data-ttu-id="7984a-278">Microsoft.AspNetCore.Mvc.Razor</span><span class="sxs-lookup"><span data-stu-id="7984a-278">Microsoft.AspNetCore.Mvc.Razor</span></span>          | <span data-ttu-id="7984a-279">Erro</span><span class="sxs-lookup"><span data-stu-id="7984a-279">Error</span></span>             |
| <span data-ttu-id="7984a-280">5</span><span class="sxs-lookup"><span data-stu-id="7984a-280">5</span></span>      | <span data-ttu-id="7984a-281">Console</span><span class="sxs-lookup"><span data-stu-id="7984a-281">Console</span></span>       | <span data-ttu-id="7984a-282">Todas as categorias</span><span class="sxs-lookup"><span data-stu-id="7984a-282">All categories</span></span>                          | <span data-ttu-id="7984a-283">Informações</span><span class="sxs-lookup"><span data-stu-id="7984a-283">Information</span></span>       |
| <span data-ttu-id="7984a-284">6</span><span class="sxs-lookup"><span data-stu-id="7984a-284">6</span></span>      | <span data-ttu-id="7984a-285">Todos os provedores</span><span class="sxs-lookup"><span data-stu-id="7984a-285">All providers</span></span> | <span data-ttu-id="7984a-286">Todas as categorias</span><span class="sxs-lookup"><span data-stu-id="7984a-286">All categories</span></span>                          | <span data-ttu-id="7984a-287">Depurar</span><span class="sxs-lookup"><span data-stu-id="7984a-287">Debug</span></span>             |
| <span data-ttu-id="7984a-288">7</span><span class="sxs-lookup"><span data-stu-id="7984a-288">7</span></span>      | <span data-ttu-id="7984a-289">Todos os provedores</span><span class="sxs-lookup"><span data-stu-id="7984a-289">All providers</span></span> | <span data-ttu-id="7984a-290">Sistema</span><span class="sxs-lookup"><span data-stu-id="7984a-290">System</span></span>                                  | <span data-ttu-id="7984a-291">Depurar</span><span class="sxs-lookup"><span data-stu-id="7984a-291">Debug</span></span>             |
| <span data-ttu-id="7984a-292">8</span><span class="sxs-lookup"><span data-stu-id="7984a-292">8</span></span>      | <span data-ttu-id="7984a-293">Depurar</span><span class="sxs-lookup"><span data-stu-id="7984a-293">Debug</span></span>         | <span data-ttu-id="7984a-294">Microsoft</span><span class="sxs-lookup"><span data-stu-id="7984a-294">Microsoft</span></span>                               | <span data-ttu-id="7984a-295">Rastrear</span><span class="sxs-lookup"><span data-stu-id="7984a-295">Trace</span></span>             |

<span data-ttu-id="7984a-296">Quando você cria um objeto `ILogger` para usar para gravar logs, o objeto `ILoggerFactory` seleciona uma única regra por provedor para aplicar a esse agente.</span><span class="sxs-lookup"><span data-stu-id="7984a-296">When you create an `ILogger` object to write logs with, the `ILoggerFactory` object selects a single rule per provider to apply to that logger.</span></span> <span data-ttu-id="7984a-297">Todas as mensagens gravadas pelo objeto `ILogger` são filtradas com base nas regras selecionadas.</span><span class="sxs-lookup"><span data-stu-id="7984a-297">All messages written by that `ILogger` object are filtered based on the selected rules.</span></span> <span data-ttu-id="7984a-298">A regra mais específica possível para cada par de categoria e provedor é selecionada dentre as regras disponíveis.</span><span class="sxs-lookup"><span data-stu-id="7984a-298">The most specific rule possible for each provider and category pair is selected from the available rules.</span></span>

<span data-ttu-id="7984a-299">O algoritmo a seguir é usado para cada provedor quando um `ILogger` é criado para uma determinada categoria:</span><span class="sxs-lookup"><span data-stu-id="7984a-299">The following algorithm is used for each provider when an `ILogger` is created for a given category:</span></span>

* <span data-ttu-id="7984a-300">Selecione todas as regras que correspondem ao provedor ou seu alias.</span><span class="sxs-lookup"><span data-stu-id="7984a-300">Select all rules that match the provider or its alias.</span></span> <span data-ttu-id="7984a-301">Se nenhuma for encontrada, selecione todas as regras com um provedor vazio.</span><span class="sxs-lookup"><span data-stu-id="7984a-301">If none are found, select all rules with an empty provider.</span></span>
* <span data-ttu-id="7984a-302">Do resultado da etapa anterior, selecione as regras com o prefixo de categoria de maior correspondência.</span><span class="sxs-lookup"><span data-stu-id="7984a-302">From the result of the preceding step, select rules with longest matching category prefix.</span></span> <span data-ttu-id="7984a-303">Se nenhuma for encontrada, selecione todas as regras que não especificam uma categoria.</span><span class="sxs-lookup"><span data-stu-id="7984a-303">If none are found, select all rules that don't specify a category.</span></span>
* <span data-ttu-id="7984a-304">Se várias regras forem selecionadas use a **última**.</span><span class="sxs-lookup"><span data-stu-id="7984a-304">If multiple rules are selected take the **last** one.</span></span>
* <span data-ttu-id="7984a-305">Se nenhuma regra for selecionada, use `MinimumLevel`.</span><span class="sxs-lookup"><span data-stu-id="7984a-305">If no rules are selected, use `MinimumLevel`.</span></span>

<span data-ttu-id="7984a-306">Por exemplo, suponha que você tem a lista anterior de regras e cria um objeto `ILogger` para a categoria "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":</span><span class="sxs-lookup"><span data-stu-id="7984a-306">For example, suppose you have the preceding list of rules and you create an `ILogger` object for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":</span></span>

* <span data-ttu-id="7984a-307">Para o provedor Depuração as regras 1, 6 e 8 se aplicam.</span><span class="sxs-lookup"><span data-stu-id="7984a-307">For the Debug provider, rules 1, 6, and 8 apply.</span></span> <span data-ttu-id="7984a-308">A regra 8 é mais específica, portanto é a que será selecionada.</span><span class="sxs-lookup"><span data-stu-id="7984a-308">Rule 8 is most specific, so that's the one selected.</span></span>
* <span data-ttu-id="7984a-309">Para o provedor Console as regras 3, 4, 5 e 6 se aplicam.</span><span class="sxs-lookup"><span data-stu-id="7984a-309">For the Console provider, rules 3, 4, 5, and 6 apply.</span></span> <span data-ttu-id="7984a-310">A regra 3 é a mais específica.</span><span class="sxs-lookup"><span data-stu-id="7984a-310">Rule 3 is most specific.</span></span>

<span data-ttu-id="7984a-311">Quando você cria logs com um `ILogger` para a categoria "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine", os logs de nível `Trace` e superiores vão para o provedor Depuração e os logs de nível `Debug` e superiores vão para o provedor Console.</span><span class="sxs-lookup"><span data-stu-id="7984a-311">When you create logs with an `ILogger` for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine", logs of `Trace` level and above will go to the Debug provider, and logs of `Debug` level and above will go to the Console provider.</span></span>

### <a name="provider-aliases"></a><span data-ttu-id="7984a-312">Aliases de provedor</span><span class="sxs-lookup"><span data-stu-id="7984a-312">Provider aliases</span></span>

<span data-ttu-id="7984a-313">Você pode usar o nome do tipo para especificar um provedor na configuração, mas cada provedor define um *alias* menor que é mais fácil de usar.</span><span class="sxs-lookup"><span data-stu-id="7984a-313">You can use the type name to specify a provider in configuration, but each provider defines a shorter *alias* that's easier to use.</span></span> <span data-ttu-id="7984a-314">Para os provedores internos, use os seguintes aliases:</span><span class="sxs-lookup"><span data-stu-id="7984a-314">For the built-in providers, use the following aliases:</span></span>

* <span data-ttu-id="7984a-315">Console</span><span class="sxs-lookup"><span data-stu-id="7984a-315">Console</span></span>
* <span data-ttu-id="7984a-316">Depurar</span><span class="sxs-lookup"><span data-stu-id="7984a-316">Debug</span></span>
* <span data-ttu-id="7984a-317">EventLog</span><span class="sxs-lookup"><span data-stu-id="7984a-317">EventLog</span></span>
* <span data-ttu-id="7984a-318">AzureAppServices</span><span class="sxs-lookup"><span data-stu-id="7984a-318">AzureAppServices</span></span>
* <span data-ttu-id="7984a-319">TraceSource</span><span class="sxs-lookup"><span data-stu-id="7984a-319">TraceSource</span></span>
* <span data-ttu-id="7984a-320">EventSource</span><span class="sxs-lookup"><span data-stu-id="7984a-320">EventSource</span></span>

### <a name="default-minimum-level"></a><span data-ttu-id="7984a-321">Nível mínimo padrão</span><span class="sxs-lookup"><span data-stu-id="7984a-321">Default minimum level</span></span>

<span data-ttu-id="7984a-322">Há uma configuração de nível mínimo que entra em vigor somente se nenhuma regra de código ou de configuração se aplicar a um provedor e uma categoria determinados.</span><span class="sxs-lookup"><span data-stu-id="7984a-322">There's a minimum level setting that takes effect only if no rules from configuration or code apply for a given provider and category.</span></span> <span data-ttu-id="7984a-323">O exemplo a seguir mostra como definir o nível mínimo:</span><span class="sxs-lookup"><span data-stu-id="7984a-323">The following example shows how to set the minimum level:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_MinLevel&highlight=3)]

<span data-ttu-id="7984a-324">Se você não definir explicitamente o nível mínimo, o valor padrão será `Information`, o que significa que logs `Trace` e `Debug` serão ignorados.</span><span class="sxs-lookup"><span data-stu-id="7984a-324">If you don't explicitly set the minimum level, the default value is `Information`, which means that `Trace` and `Debug` logs are ignored.</span></span>

### <a name="filter-functions"></a><span data-ttu-id="7984a-325">Funções de filtro</span><span class="sxs-lookup"><span data-stu-id="7984a-325">Filter functions</span></span>

<span data-ttu-id="7984a-326">Você pode escrever código em uma função de filtro para aplicar regras de filtragem.</span><span class="sxs-lookup"><span data-stu-id="7984a-326">You can write code in a filter function to apply filtering rules.</span></span> <span data-ttu-id="7984a-327">Uma função de filtro é invocada para todos os provedores e categorias que não têm regras atribuídas a eles por configuração ou código.</span><span class="sxs-lookup"><span data-stu-id="7984a-327">A filter function is invoked for all providers and categories that don't have rules assigned to them by configuration or code.</span></span> <span data-ttu-id="7984a-328">O código na função tem acesso ao tipo de provedor, à categoria e ao nível de log para decidir se uma mensagem deve ser registrada.</span><span class="sxs-lookup"><span data-stu-id="7984a-328">Code in the function has access to the provider type, category, and log level to decide whether or not a message should be logged.</span></span> <span data-ttu-id="7984a-329">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="7984a-329">For example:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="7984a-330">Alguns provedores de log permitem especificar quando os logs devem ser gravados em uma mídia de armazenamento ou ignorados, com base no nível de log e na categoria.</span><span class="sxs-lookup"><span data-stu-id="7984a-330">Some logging providers let you specify when logs should be written to a storage medium or ignored based on log level and category.</span></span>

<span data-ttu-id="7984a-331">Os métodos de extensão `AddConsole` e `AddDebug` fornecem sobrecargas que permitem que você passe critérios de filtragem.</span><span class="sxs-lookup"><span data-stu-id="7984a-331">The `AddConsole` and `AddDebug` extension methods provide overloads that let you pass in filtering criteria.</span></span> <span data-ttu-id="7984a-332">O código de exemplo a seguir faz com que o provedor de console ignore os logs abaixo do nível `Warning`, enquanto o provedor Depuração ignora os logs criados pela estrutura.</span><span class="sxs-lookup"><span data-stu-id="7984a-332">The following sample code causes the console provider to ignore logs below `Warning` level, while the Debug provider ignores logs that the framework creates.</span></span>

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_AddConsoleAndDebugWithFilter&highlight=6-7)]

<span data-ttu-id="7984a-333">O método `AddEventLog` tem uma sobrecarga que recebe uma instância `EventLogSettings`, que pode conter uma função de filtragem em sua propriedade `Filter`.</span><span class="sxs-lookup"><span data-stu-id="7984a-333">The `AddEventLog` method has an overload that takes an `EventLogSettings` instance, which may contain a filtering function in its `Filter` property.</span></span> <span data-ttu-id="7984a-334">O provedor TraceSource não fornece nenhuma dessas sobrecargas, porque seu nível de registro em log e outros parâmetros são baseados no `SourceSwitch` e no `TraceListener` que ele usa.</span><span class="sxs-lookup"><span data-stu-id="7984a-334">The TraceSource provider doesn't provide any of those overloads, since its logging level and other parameters are based on the `SourceSwitch` and `TraceListener` it uses.</span></span>

<span data-ttu-id="7984a-335">Você pode definir regras de filtragem para todos os provedores registrados com uma instância `ILoggerFactory` usando o método de extensão `WithFilter`.</span><span class="sxs-lookup"><span data-stu-id="7984a-335">You can set filtering rules for all providers that are registered with an `ILoggerFactory` instance by using the `WithFilter` extension method.</span></span> <span data-ttu-id="7984a-336">O exemplo abaixo limita os logs de estrutura (categoria começa com "Microsoft" ou "Sistema") a avisos, permitindo que aplicativo registre no nível de depuração.</span><span class="sxs-lookup"><span data-stu-id="7984a-336">The example below limits framework logs (category begins with "Microsoft" or "System") to warnings while letting the app log at debug level.</span></span>

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_FactoryFilter&highlight=6-11)]

<span data-ttu-id="7984a-337">Se quiser usar a filtragem para impedir que todos os logs de uma determinada categoria sejam gravados, você poderá especificar `LogLevel.None` como o nível de log mínimo para essa categoria.</span><span class="sxs-lookup"><span data-stu-id="7984a-337">If you want to use filtering to prevent all logs from being written for a particular category, you can specify `LogLevel.None` as the minimum log level for that category.</span></span> <span data-ttu-id="7984a-338">O valor inteiro de `LogLevel.None` é 6, que é maior do que `LogLevel.Critical` (5).</span><span class="sxs-lookup"><span data-stu-id="7984a-338">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

<span data-ttu-id="7984a-339">O método de extensão `WithFilter` é fornecido pelo pacote NuGet [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter).</span><span class="sxs-lookup"><span data-stu-id="7984a-339">The `WithFilter` extension method is provided by the [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) NuGet package.</span></span> <span data-ttu-id="7984a-340">O método retorna um nova instância `ILoggerFactory`, que filtrará as mensagens de log passadas para todos os provedores de agente registrados com ela.</span><span class="sxs-lookup"><span data-stu-id="7984a-340">The method returns a new `ILoggerFactory` instance that will filter the log messages passed to all logger providers registered with it.</span></span> <span data-ttu-id="7984a-341">Isso não afeta nenhuma outra instância de `ILoggerFactory`, incluindo a instância de `ILoggerFactory` original.</span><span class="sxs-lookup"><span data-stu-id="7984a-341">It doesn't affect any other `ILoggerFactory` instances, including the original `ILoggerFactory` instance.</span></span>

::: moniker-end

## <a name="log-scopes"></a><span data-ttu-id="7984a-342">Escopos de log</span><span class="sxs-lookup"><span data-stu-id="7984a-342">Log scopes</span></span>

<span data-ttu-id="7984a-343">Você pode agrupar um conjunto de operações lógicas dentro de um *escopo* para anexar os mesmos dados a cada log que é criado como parte desse conjunto.</span><span class="sxs-lookup"><span data-stu-id="7984a-343">You can group a set of logical operations within a *scope* in order to attach the same data to each log that's created as part of that set.</span></span> <span data-ttu-id="7984a-344">Por exemplo, é conveniente que cada log criado como parte do processamento de uma transação inclua a ID da transação.</span><span class="sxs-lookup"><span data-stu-id="7984a-344">For example, you might want every log created as part of processing a transaction to include the transaction ID.</span></span>

<span data-ttu-id="7984a-345">Um escopo é um tipo `IDisposable` retornado pelo método [ILogger.BeginScope&lt;TState&gt;](/dotnet/api/microsoft.extensions.logging.ilogger.beginscope) e que dura até que seja descartado.</span><span class="sxs-lookup"><span data-stu-id="7984a-345">A scope is an `IDisposable` type that's returned by the [ILogger.BeginScope&lt;TState&gt;](/dotnet/api/microsoft.extensions.logging.ilogger.beginscope) method and lasts until it's disposed.</span></span> <span data-ttu-id="7984a-346">Um escopo é usado pelo encapsulamento das chama de agente em um bloco `using`, conforme mostrado aqui:</span><span class="sxs-lookup"><span data-stu-id="7984a-346">You use a scope by wrapping your logger calls in a `using` block, as shown here:</span></span>

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

<span data-ttu-id="7984a-347">O código a seguir habilita os escopos para o provedor de console:</span><span class="sxs-lookup"><span data-stu-id="7984a-347">The following code enables scopes for the console provider:</span></span>

::: moniker range="> aspnetcore-2.0"

<span data-ttu-id="7984a-348">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="7984a-348">*Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> <span data-ttu-id="7984a-349">A configuração da opção de agente de console `IncludeScopes` é necessária para habilitar o registro em log baseado em escopo.</span><span class="sxs-lookup"><span data-stu-id="7984a-349">Configuring the `IncludeScopes` console logger option is required to enable scope-based logging.</span></span>
>
> <span data-ttu-id="7984a-350">Saiba mais sobre como configurar na seção [Configuração](#configuration).</span><span class="sxs-lookup"><span data-stu-id="7984a-350">For information on configuration, see the [Configuration](#configuration) section.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="7984a-351">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="7984a-351">*Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> <span data-ttu-id="7984a-352">A configuração da opção de agente de console `IncludeScopes` é necessária para habilitar o registro em log baseado em escopo.</span><span class="sxs-lookup"><span data-stu-id="7984a-352">Configuring the `IncludeScopes` console logger option is required to enable scope-based logging.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="7984a-353">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="7984a-353">*Startup.cs*:</span></span>

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_Scopes&highlight=6)]

::: moniker-end

<span data-ttu-id="7984a-354">Cada mensagem de log inclui as informações com escopo definido:</span><span class="sxs-lookup"><span data-stu-id="7984a-354">Each log message includes the scoped information:</span></span>

```
info: TodoApi.Controllers.TodoController[1002]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      GetById(0) NOT FOUND
```

## <a name="built-in-logging-providers"></a><span data-ttu-id="7984a-355">Provedores de log internos</span><span class="sxs-lookup"><span data-stu-id="7984a-355">Built-in logging providers</span></span>

<span data-ttu-id="7984a-356">O ASP.NET Core vem com os seguintes provedores:</span><span class="sxs-lookup"><span data-stu-id="7984a-356">ASP.NET Core ships the following providers:</span></span>

* [<span data-ttu-id="7984a-357">Console</span><span class="sxs-lookup"><span data-stu-id="7984a-357">Console</span></span>](#console-provider)
* [<span data-ttu-id="7984a-358">Depurar</span><span class="sxs-lookup"><span data-stu-id="7984a-358">Debug</span></span>](#debug-provider)
* [<span data-ttu-id="7984a-359">EventSource</span><span class="sxs-lookup"><span data-stu-id="7984a-359">EventSource</span></span>](#eventsource-provider)
* [<span data-ttu-id="7984a-360">EventLog</span><span class="sxs-lookup"><span data-stu-id="7984a-360">EventLog</span></span>](#windows-eventlog-provider)
* [<span data-ttu-id="7984a-361">TraceSource</span><span class="sxs-lookup"><span data-stu-id="7984a-361">TraceSource</span></span>](#tracesource-provider)
* [<span data-ttu-id="7984a-362">Serviço de Aplicativo do Azure</span><span class="sxs-lookup"><span data-stu-id="7984a-362">Azure App Service</span></span>](#azure-app-service-provider)

### <a name="console-provider"></a><span data-ttu-id="7984a-363">Provedor do console</span><span class="sxs-lookup"><span data-stu-id="7984a-363">Console provider</span></span>

<span data-ttu-id="7984a-364">O pacote de provedor [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) envia a saída de log para o console.</span><span class="sxs-lookup"><span data-stu-id="7984a-364">The [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) provider package sends log output to the console.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddConsole();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddConsole();
```

<span data-ttu-id="7984a-365">As [sobrecargas do AddConsole](/dotnet/api/microsoft.extensions.logging.consoleloggerextensions) permitem que você passe um nível de log mínimo, uma função de filtro e um valor booliano que indica se escopos são compatíveis.</span><span class="sxs-lookup"><span data-stu-id="7984a-365">[AddConsole overloads](/dotnet/api/microsoft.extensions.logging.consoleloggerextensions) let you pass in an a minimum log level, a filter function, and a boolean that indicates whether scopes are supported.</span></span> <span data-ttu-id="7984a-366">Outra opção é passar um objeto `IConfiguration`, que pode especificar suporte de escopos e níveis de log.</span><span class="sxs-lookup"><span data-stu-id="7984a-366">Another option is to pass in an `IConfiguration` object, which can specify scopes support and logging levels.</span></span>

<span data-ttu-id="7984a-367">Se você estiver considerando o provedor de console para uso em produção, lembre-se de que ele tem um impacto significativo no desempenho.</span><span class="sxs-lookup"><span data-stu-id="7984a-367">If you are considering the console provider for use in production, be aware that it has a significant impact on performance.</span></span>

<span data-ttu-id="7984a-368">Quando você cria um novo projeto no Visual Studio, o método `AddConsole` tem essa aparência:</span><span class="sxs-lookup"><span data-stu-id="7984a-368">When you create a new project in Visual Studio, the `AddConsole` method looks like this:</span></span>

```csharp
loggerFactory.AddConsole(Configuration.GetSection("Logging"));
```

<span data-ttu-id="7984a-369">Esse código se refere à seção `Logging` do arquivo *appSettings.json*:</span><span class="sxs-lookup"><span data-stu-id="7984a-369">This code refers to the `Logging` section of the *appSettings.json* file:</span></span>

[!code-json[](index/samples/1.x/TodoApiSample//appsettings.json)]

<span data-ttu-id="7984a-370">As configurações mostradas limitam os logs de estrutura a avisos, permitindo que o aplicativo para faça registros no nível de depuração, conforme explicado na [Filtragem de log](#log-filtering) seção.</span><span class="sxs-lookup"><span data-stu-id="7984a-370">The settings shown limit framework logs to warnings while allowing the app to log at debug level, as explained in the [Log filtering](#log-filtering) section.</span></span> <span data-ttu-id="7984a-371">Para obter mais informações, consulte [Configuração](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="7984a-371">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

::: moniker-end

### <a name="debug-provider"></a><span data-ttu-id="7984a-372">Depurar provedor</span><span class="sxs-lookup"><span data-stu-id="7984a-372">Debug provider</span></span>

<span data-ttu-id="7984a-373">O pacote de provedor [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) grava a saída de log usando a classe [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) (chamadas de método `Debug.WriteLine`).</span><span class="sxs-lookup"><span data-stu-id="7984a-373">The [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) provider package writes log output by using the [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) class (`Debug.WriteLine` method calls).</span></span>

<span data-ttu-id="7984a-374">No Linux, esse provedor grava logs em */var/log/message*.</span><span class="sxs-lookup"><span data-stu-id="7984a-374">On Linux, this provider writes logs to */var/log/message*.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddDebug();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddDebug();
```

<span data-ttu-id="7984a-375">As [sobrecargas de AddDebug](/dotnet/api/microsoft.extensions.logging.debugloggerfactoryextensions) permitem que você passe um nível de log mínimo ou uma função de filtro.</span><span class="sxs-lookup"><span data-stu-id="7984a-375">[AddDebug overloads](/dotnet/api/microsoft.extensions.logging.debugloggerfactoryextensions) let you pass in a minimum log level or a filter function.</span></span>

::: moniker-end

### <a name="eventsource-provider"></a><span data-ttu-id="7984a-376">Provedor EventSource</span><span class="sxs-lookup"><span data-stu-id="7984a-376">EventSource provider</span></span>

<span data-ttu-id="7984a-377">Para aplicativos que se destinam ao ASP.NET Core 1.1.0 ou posterior, o pacote de provedor [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) pode implementar o rastreamento de eventos.</span><span class="sxs-lookup"><span data-stu-id="7984a-377">For apps that target ASP.NET Core 1.1.0 or later, the [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) provider package can implement event tracing.</span></span> <span data-ttu-id="7984a-378">No Windows, ele usa [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span><span class="sxs-lookup"><span data-stu-id="7984a-378">On Windows, it uses [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span></span> <span data-ttu-id="7984a-379">O provedor é multiplataforma, mas ainda não há ferramentas de coleta e exibição de eventos para Linux ou macOS.</span><span class="sxs-lookup"><span data-stu-id="7984a-379">The provider is cross-platform, but there are no event collection and display tools yet for Linux or macOS.</span></span>

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

<span data-ttu-id="7984a-380">Uma boa maneira de coletar e exibir logs é usar o [utilitário PerfView](https://github.com/Microsoft/perfview).</span><span class="sxs-lookup"><span data-stu-id="7984a-380">A good way to collect and view logs is to use the [PerfView utility](https://github.com/Microsoft/perfview).</span></span> <span data-ttu-id="7984a-381">Há outras ferramentas para exibir os logs do ETW, mas o PerfView proporciona a melhor experiência para trabalhar com os eventos de ETW emitidos pelo ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="7984a-381">There are other tools for viewing ETW logs, but PerfView provides the best experience for working with the ETW events emitted by ASP.NET.</span></span>

<span data-ttu-id="7984a-382">Para configurar o PerfView para coletar eventos registrados por esse provedor, adicione a cadeia de caracteres `*Microsoft-Extensions-Logging` à lista **Provedores Adicionais**.</span><span class="sxs-lookup"><span data-stu-id="7984a-382">To configure PerfView for collecting events logged by this provider, add the string `*Microsoft-Extensions-Logging` to the **Additional Providers** list.</span></span> <span data-ttu-id="7984a-383">(Não se esqueça do asterisco no início da cadeia de caracteres).</span><span class="sxs-lookup"><span data-stu-id="7984a-383">(Don't miss the asterisk at the start of the string.)</span></span>

![Outros provedores de Perfview](index/_static/perfview-additional-providers.png)

### <a name="windows-eventlog-provider"></a><span data-ttu-id="7984a-385">Provedor EventLog do Windows</span><span class="sxs-lookup"><span data-stu-id="7984a-385">Windows EventLog provider</span></span>

<span data-ttu-id="7984a-386">O pacote de provedor [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) envia a saída de log para o Log de Eventos do Windows.</span><span class="sxs-lookup"><span data-stu-id="7984a-386">The [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) provider package sends log output to the Windows Event Log.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddEventLog();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddEventLog();
```

<span data-ttu-id="7984a-387">As [sobrecargas de AddEventLog](/dotnet/api/microsoft.extensions.logging.eventloggerfactoryextensions) permitem que você passe `EventLogSettings` ou um nível de log mínimo.</span><span class="sxs-lookup"><span data-stu-id="7984a-387">[AddEventLog overloads](/dotnet/api/microsoft.extensions.logging.eventloggerfactoryextensions) let you pass in `EventLogSettings` or a minimum log level.</span></span>

::: moniker-end

### <a name="tracesource-provider"></a><span data-ttu-id="7984a-388">Provedor TraceSource</span><span class="sxs-lookup"><span data-stu-id="7984a-388">TraceSource provider</span></span>

<span data-ttu-id="7984a-389">O pacote de provedor [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) usa as bibliotecas e provedores de [System.Diagnostics.TraceSource](/dotnet/api/system.diagnostics.tracesource).</span><span class="sxs-lookup"><span data-stu-id="7984a-389">The [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) provider package uses the [System.Diagnostics.TraceSource](/dotnet/api/system.diagnostics.tracesource) libraries and providers.</span></span>

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

<span data-ttu-id="7984a-390">As [sobrecargas de AddTraceSource](/dotnet/api/microsoft.extensions.logging.tracesourcefactoryextensions) permitem que você passe um comutador de fonte e um ouvinte de rastreamento.</span><span class="sxs-lookup"><span data-stu-id="7984a-390">[AddTraceSource overloads](/dotnet/api/microsoft.extensions.logging.tracesourcefactoryextensions) let you pass in a source switch and a trace listener.</span></span>

<span data-ttu-id="7984a-391">Para usar esse provedor, o aplicativo deve ser executado no .NET Framework (em vez do .NET Core).</span><span class="sxs-lookup"><span data-stu-id="7984a-391">To use this provider, an application has to run on the .NET Framework (rather than .NET Core).</span></span> <span data-ttu-id="7984a-392">O provedor permite rotear mensagens a uma variedade de [ouvintes](/dotnet/framework/debug-trace-profile/trace-listeners), como o [TextWriterTraceListener](/dotnet/api/system.diagnostics.textwritertracelistenerr) usado no aplicativo de exemplo.</span><span class="sxs-lookup"><span data-stu-id="7984a-392">The provider lets you route messages to a variety of [listeners](/dotnet/framework/debug-trace-profile/trace-listeners), such as the [TextWriterTraceListener](/dotnet/api/system.diagnostics.textwritertracelistenerr) used in the sample application.</span></span>

<span data-ttu-id="7984a-393">O exemplo a seguir configura um provedor `TraceSource` que registra mensagens `Warning` e superiores na janela de console.</span><span class="sxs-lookup"><span data-stu-id="7984a-393">The following example configures a `TraceSource` provider that logs `Warning` and higher messages to the console window.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Startup.cs?name=snippet_TraceSource&highlight=9-12)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_TraceSource&highlight=9-12)]

::: moniker-end

### <a name="azure-app-service-provider"></a><span data-ttu-id="7984a-394">Provedor do Serviço de Aplicativo do Azure</span><span class="sxs-lookup"><span data-stu-id="7984a-394">Azure App Service provider</span></span>

<span data-ttu-id="7984a-395">O pacote de provedor [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) grava logs em arquivos de texto no sistema de arquivos de um aplicativo do Serviço de Aplicativo do Azure e no [armazenamento de blobs](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) em uma conta de Armazenamento do Azure.</span><span class="sxs-lookup"><span data-stu-id="7984a-395">The [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) provider package writes logs to text files in an Azure App Service app's file system and to [blob storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) in an Azure Storage account.</span></span> <span data-ttu-id="7984a-396">O pacote de provedor está disponível para aplicativos destinados ao .NET Core 1.1 ou posterior.</span><span class="sxs-lookup"><span data-stu-id="7984a-396">The provider package is available for apps targeting .NET Core 1.1 or later.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="7984a-397">Se você estiver direcionando para o .NET Core, observe os seguintes pontos:</span><span class="sxs-lookup"><span data-stu-id="7984a-397">If targeting .NET Core, note the following points:</span></span>

* <span data-ttu-id="7984a-398">O pacote do provedor está incluído no [metapacote Microsoft.AspNetCore.All](xref:fundamentals/metapackage) do ASP.NET Core, mas não no [metapacote Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="7984a-398">The provider package is included in the ASP.NET Core [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) but not in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>
* <span data-ttu-id="7984a-399">Não chame explicitamente [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics).</span><span class="sxs-lookup"><span data-stu-id="7984a-399">Don't explicitly call [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics).</span></span> <span data-ttu-id="7984a-400">Quando o aplicativo for implantado no Serviço de Aplicativo do Azure, o provedor ficará automaticamente disponível para ele.</span><span class="sxs-lookup"><span data-stu-id="7984a-400">The provider is automatically made available to the app when the app is deployed to Azure App Service.</span></span>

<span data-ttu-id="7984a-401">Se você estiver direcionando para o .NET Framework ou referenciando o metapacote `Microsoft.AspNetCore.App`, adicione o pacote do provedor ao projeto.</span><span class="sxs-lookup"><span data-stu-id="7984a-401">If targeting .NET Framework or referencing the `Microsoft.AspNetCore.App` metapackage, add the provider package to the project.</span></span> <span data-ttu-id="7984a-402">Invocar `AddAzureWebAppDiagnostics` em uma instância do [ILoggerFactory](/dotnet/api/microsoft.extensions.logging.iloggerfactory):</span><span class="sxs-lookup"><span data-stu-id="7984a-402">Invoke `AddAzureWebAppDiagnostics` on an [ILoggerFactory](/dotnet/api/microsoft.extensions.logging.iloggerfactory) instance:</span></span>

```csharp
logging.AddAzureWebAppDiagnostics();
```

::: moniker-end

::: moniker range="= aspnetcore-1.1"

```csharp
loggerFactory.AddAzureWebAppDiagnostics();
```

::: moniker-end

<span data-ttu-id="7984a-403">Uma sobrecarga do [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) permite que você passe [AzureAppServicesDiagnosticsSettings](/dotnet/api/microsoft.extensions.logging.azureappservices.azureappservicesdiagnosticssettings) que pode ser usado para substituir as configurações padrão, como o modelo de saída de registro em log, o nome do blob e o limite do tamanho do arquivo.</span><span class="sxs-lookup"><span data-stu-id="7984a-403">An [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) overload lets you pass in [AzureAppServicesDiagnosticsSettings](/dotnet/api/microsoft.extensions.logging.azureappservices.azureappservicesdiagnosticssettings) with which you can override default settings, such as the logging output template, blob name, and file size limit.</span></span> <span data-ttu-id="7984a-404">(O *modelo Output* é um modelo de mensagem que é aplicado a todos os logs, sobre aquele que você fornece ao chamar um método `ILogger`).</span><span class="sxs-lookup"><span data-stu-id="7984a-404">(*Output template* is a message template that's applied to all logs on top of the one that you provide when you call an `ILogger` method.)</span></span>

<span data-ttu-id="7984a-405">Ao implantar um aplicativo do Serviço de Aplicativo, o aplicativo respeitará as configurações na seção [Logs de Diagnóstico](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag) da página **Serviço de Aplicativo** do Portal do Azure.</span><span class="sxs-lookup"><span data-stu-id="7984a-405">When you deploy to an App Service app, the app honors the settings in the [Diagnostic Logs](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag) section of the **App Service** page of the Azure portal.</span></span> <span data-ttu-id="7984a-406">Quando essas configurações são atualizadas, as alterações entram em vigor imediatamente sem a necessidade de uma reinicialização ou reimplantação do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="7984a-406">When these settings are updated, the changes take effect immediately without requiring a restart or redeployment of the app.</span></span>

![Configurações de registro em log do Azure](index/_static/azure-logging-settings.png)

<span data-ttu-id="7984a-408">O local padrão para arquivos de log é na pasta *D:\\home\\LogFiles\\Application* e o nome de arquivo padrão é *diagnostics-aaaammdd.txt*.</span><span class="sxs-lookup"><span data-stu-id="7984a-408">The default location for log files is in the *D:\\home\\LogFiles\\Application* folder, and the default file name is *diagnostics-yyyymmdd.txt*.</span></span> <span data-ttu-id="7984a-409">O limite padrão de tamanho do arquivo é 10 MB e o número padrão máximo de arquivos mantidos é 2.</span><span class="sxs-lookup"><span data-stu-id="7984a-409">The default file size limit is 10 MB, and the default maximum number of files retained is 2.</span></span> <span data-ttu-id="7984a-410">O nome de blob padrão é *{app-name}{timestamp}/aaaa/mm/dd/hh/{guid}-applicationLog.txt*.</span><span class="sxs-lookup"><span data-stu-id="7984a-410">The default blob name is *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*.</span></span> <span data-ttu-id="7984a-411">Para obter mais informações sobre o comportamento padrão, consulte [AzureAppServicesDiagnosticsSettings](/dotnet/api/microsoft.extensions.logging.azureappservices.azureappservicesdiagnosticssettings).</span><span class="sxs-lookup"><span data-stu-id="7984a-411">For more information about default behavior, see [AzureAppServicesDiagnosticsSettings](/dotnet/api/microsoft.extensions.logging.azureappservices.azureappservicesdiagnosticssettings).</span></span>

<span data-ttu-id="7984a-412">O provedor funciona somente quando o projeto é executado no ambiente do Azure.</span><span class="sxs-lookup"><span data-stu-id="7984a-412">The provider only works when the project runs in the Azure environment.</span></span> <span data-ttu-id="7984a-413">Ele não tem nenhum efeito quando o projeto é executado localmente&mdash;ele não grava em arquivos locais ou no armazenamento de desenvolvimento local para blobs.</span><span class="sxs-lookup"><span data-stu-id="7984a-413">It has no effect when the project is run locally&mdash;it doesn't write to local files or local development storage for blobs.</span></span>

## <a name="third-party-logging-providers"></a><span data-ttu-id="7984a-414">Provedores de log de terceiros</span><span class="sxs-lookup"><span data-stu-id="7984a-414">Third-party logging providers</span></span>

<span data-ttu-id="7984a-415">Estruturas de log de terceiros que funcionam com o ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="7984a-415">Third-party logging frameworks that work with ASP.NET Core:</span></span>

* <span data-ttu-id="7984a-416">[elmah.io](https://elmah.io/) ([repositório GitHub](https://github.com/elmahio/Elmah.Io.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="7984a-416">[elmah.io](https://elmah.io/) ([GitHub repo](https://github.com/elmahio/Elmah.Io.Extensions.Logging))</span></span>
* <span data-ttu-id="7984a-417">[Gelf](http://docs.graylog.org/en/2.3/pages/gelf.html) ([repositório do GitHub](https://github.com/mattwcole/gelf-extensions-logging))</span><span class="sxs-lookup"><span data-stu-id="7984a-417">[Gelf](http://docs.graylog.org/en/2.3/pages/gelf.html) ([GitHub repo](https://github.com/mattwcole/gelf-extensions-logging))</span></span>
* <span data-ttu-id="7984a-418">[JSNLog](http://jsnlog.com/) ([repositório GitHub](https://github.com/mperdeck/jsnlog))</span><span class="sxs-lookup"><span data-stu-id="7984a-418">[JSNLog](http://jsnlog.com/) ([GitHub repo](https://github.com/mperdeck/jsnlog))</span></span>
* <span data-ttu-id="7984a-419">[KissLog.net](https://kisslog.net/) ([Repositório do GitHub](https://github.com/catalingavan/KissLog-net))</span><span class="sxs-lookup"><span data-stu-id="7984a-419">[KissLog.net](https://kisslog.net/) ([GitHub repo](https://github.com/catalingavan/KissLog-net))</span></span>
* <span data-ttu-id="7984a-420">[Loggr](http://loggr.net/) ([repositório GitHub](https://github.com/imobile3/Loggr.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="7984a-420">[Loggr](http://loggr.net/) ([GitHub repo](https://github.com/imobile3/Loggr.Extensions.Logging))</span></span>
* <span data-ttu-id="7984a-421">[NLog](http://nlog-project.org/) ([repositório GitHub](https://github.com/NLog/NLog.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="7984a-421">[NLog](http://nlog-project.org/) ([GitHub repo](https://github.com/NLog/NLog.Extensions.Logging))</span></span>
* <span data-ttu-id="7984a-422">[Serilog](https://serilog.net/) ([repositório GitHub](https://github.com/serilog/serilog-extensions-logging))</span><span class="sxs-lookup"><span data-stu-id="7984a-422">[Serilog](https://serilog.net/) ([GitHub repo](https://github.com/serilog/serilog-extensions-logging))</span></span>

<span data-ttu-id="7984a-423">Algumas estruturas de terceiros podem fazer o [log semântico, também conhecido como registro em log estruturado](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="7984a-423">Some third-party frameworks can perform [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="7984a-424">Usar uma estrutura de terceiros é semelhante ao uso de um dos provedores internos:</span><span class="sxs-lookup"><span data-stu-id="7984a-424">Using a third-party framework is similar to using one of the built-in providers:</span></span>

1. <span data-ttu-id="7984a-425">Adicione um pacote NuGet ao projeto.</span><span class="sxs-lookup"><span data-stu-id="7984a-425">Add a NuGet package to your project.</span></span>
1. <span data-ttu-id="7984a-426">Chame um método de extensão em `ILoggerFactory`.</span><span class="sxs-lookup"><span data-stu-id="7984a-426">Call an extension method on `ILoggerFactory`.</span></span>

<span data-ttu-id="7984a-427">Para obter mais informações, consulte a documentação de cada estrutura.</span><span class="sxs-lookup"><span data-stu-id="7984a-427">For more information, see each framework's documentation.</span></span>

## <a name="azure-log-streaming"></a><span data-ttu-id="7984a-428">Fluxo de log do Azure</span><span class="sxs-lookup"><span data-stu-id="7984a-428">Azure log streaming</span></span>

<span data-ttu-id="7984a-429">O fluxo de log do Azure permite que você exiba a atividade de log em tempo real:</span><span class="sxs-lookup"><span data-stu-id="7984a-429">Azure log streaming enables you to view log activity in real time from:</span></span>

* <span data-ttu-id="7984a-430">Do servidor de aplicativos</span><span class="sxs-lookup"><span data-stu-id="7984a-430">The application server</span></span>
* <span data-ttu-id="7984a-431">Do servidor Web</span><span class="sxs-lookup"><span data-stu-id="7984a-431">The web server</span></span>
* <span data-ttu-id="7984a-432">De uma solicitação de rastreio com falha</span><span class="sxs-lookup"><span data-stu-id="7984a-432">Failed request tracing</span></span>

<span data-ttu-id="7984a-433">Para configurar o fluxo de log do Azure:</span><span class="sxs-lookup"><span data-stu-id="7984a-433">To configure Azure log streaming:</span></span>

* <span data-ttu-id="7984a-434">Navegue até a página **Logs de Diagnóstico** da página do portal do seu aplicativo</span><span class="sxs-lookup"><span data-stu-id="7984a-434">Navigate to the **Diagnostics Logs** page from your application's portal page</span></span>
* <span data-ttu-id="7984a-435">Defina o **Log de aplicativo (Sistema de Arquivos)** como ativado.</span><span class="sxs-lookup"><span data-stu-id="7984a-435">Set **Application Logging (Filesystem)** to on.</span></span>

![Página de logs de diagnóstico do Portal do Azure](index/_static/azure-diagnostic-logs.png)

<span data-ttu-id="7984a-437">Navegue até a página **Fluxo de Log** para exibir as mensagens de aplicativo.</span><span class="sxs-lookup"><span data-stu-id="7984a-437">Navigate to the **Log Streaming** page to view application messages.</span></span> <span data-ttu-id="7984a-438">Elas são registradas pelo aplicativo por meio da interface `ILogger`.</span><span class="sxs-lookup"><span data-stu-id="7984a-438">They're logged by application through the `ILogger` interface.</span></span>

![Fluxo de log do aplicativo do Portal do Azure](index/_static/azure-log-streaming.png)

## <a name="azure-application-insights-trace-logging"></a><span data-ttu-id="7984a-440">Log de rastreamento do Azure Application Insights</span><span class="sxs-lookup"><span data-stu-id="7984a-440">Azure Application Insights trace logging</span></span>

<span data-ttu-id="7984a-441">O SDK do [Application Insights](https://azure.microsoft.com/services/application-insights/) é capaz de coletar telemetria de rastreamento de logs gerados por meio de infraestrutura de log do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7984a-441">The [Application Insights](https://azure.microsoft.com/services/application-insights/) SDK is capable of collecting trace telemetry from logs generated via the ASP.NET Core logging infrastructure.</span></span> <span data-ttu-id="7984a-442">Para obter mais informações, veja [Application Insights para ASP.NET Core](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net-core) e [Wiki do Microsoft/ApplicationInsights-aspnetcore: registro em log](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Logging).</span><span class="sxs-lookup"><span data-stu-id="7984a-442">For more information, see [Application Insights for ASP.NET Core](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net-core) and [Microsoft/ApplicationInsights-aspnetcore Wiki: Logging](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Logging).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7984a-443">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="7984a-443">Additional resources</span></span>

* <xref:fundamentals/logging/loggermessage>

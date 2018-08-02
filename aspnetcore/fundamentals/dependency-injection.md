---
title: Injeção de dependência no ASP.NET Core
author: guardrex
description: Saiba como o ASP.NET Core implementa a injeção de dependência e como usá-la.
ms.author: riande
ms.custom: mvc
ms.date: 07/02/2018
uid: fundamentals/dependency-injection
ms.openlocfilehash: 861370dc689e2420838f639ea0b1fb8f73927e16
ms.sourcegitcommit: 927e510d68f269d8335b5a7c8592621219a90965
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39342413"
---
# <a name="dependency-injection-in-aspnet-core"></a><span data-ttu-id="22cfe-103">Injeção de dependência no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="22cfe-103">Dependency injection in ASP.NET Core</span></span>

<span data-ttu-id="22cfe-104">Por [Steve Smith](https://ardalis.com/), [Scott Addie](https://scottaddie.com) e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="22cfe-104">By [Steve Smith](https://ardalis.com/), [Scott Addie](https://scottaddie.com), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="22cfe-105">O ASP.NET Core dá suporte ao padrão de design de software de DI (injeção de dependência), que é uma técnica para conseguir [IoC (inversão de controle)](https://deviq.com/inversion-of-control/) entre classes e suas dependências.</span><span class="sxs-lookup"><span data-stu-id="22cfe-105">ASP.NET Core supports the dependency injection (DI) software design pattern, which is a technique for achieving [Inversion of Control (IoC)](https://deviq.com/inversion-of-control/) between classes and their dependencies.</span></span>

<span data-ttu-id="22cfe-106">Para obter mais informações específicas sobre injeção de dependência em controladores de MVC, consulte <xref:mvc/controllers/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="22cfe-106">For more information specific to dependency injection within MVC controllers, see <xref:mvc/controllers/dependency-injection>.</span></span>

<span data-ttu-id="22cfe-107">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="22cfe-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="overview-of-dependency-injection"></a><span data-ttu-id="22cfe-108">Visão geral da injeção de dependência</span><span class="sxs-lookup"><span data-stu-id="22cfe-108">Overview of dependency injection</span></span>

<span data-ttu-id="22cfe-109">Uma *dependência* é qualquer objeto exigido por outro objeto.</span><span class="sxs-lookup"><span data-stu-id="22cfe-109">A *dependency* is any object that another object requires.</span></span> <span data-ttu-id="22cfe-110">Examine a classe `MyDependency` a seguir com um método `WriteMessage` do qual outras classes em um aplicativo dependem:</span><span class="sxs-lookup"><span data-stu-id="22cfe-110">Examine the following `MyDependency` class with a `WriteMessage` method that other classes in an app depend upon:</span></span>

```csharp
public class MyDependency
{
    public MyDependency()
    {
    }

    public Task WriteMessage(string message)
    {
        Console.WriteLine(
            $"MyDependency.WriteMessage called. Message: {message}");

        return Task.FromResult(0);
    }
}
```

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="22cfe-111">Uma instância da classe `MyDependency` pode ser criada para tornar o método `WriteMessage` disponível para uma classe.</span><span class="sxs-lookup"><span data-stu-id="22cfe-111">An instance of the `MyDependency` class can be created to make the `WriteMessage` method available to a class.</span></span> <span data-ttu-id="22cfe-112">A classe `MyDependency` é uma dependência da classe `IndexModel`:</span><span class="sxs-lookup"><span data-stu-id="22cfe-112">The `MyDependency` class is a dependency of the `IndexModel` class:</span></span>

```csharp
public class IndexModel : PageModel
{
    MyDependency _dependency = new MyDependency();

    public async Task OnGetAsync()
    {
        await _myDependency.WriteMessage(
            "IndexModel.OnGetAsync created this message.");
    }
}
```

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="22cfe-113">Uma instância da classe `MyDependency` pode ser criada para tornar o método `WriteMessage` disponível para uma classe.</span><span class="sxs-lookup"><span data-stu-id="22cfe-113">An instance of the `MyDependency` class can be created to make the `WriteMessage` method available to a class.</span></span> <span data-ttu-id="22cfe-114">A classe `MyDependency` é uma dependência da classe `HomeController`:</span><span class="sxs-lookup"><span data-stu-id="22cfe-114">The `MyDependency` class is a dependency of the `HomeController` class:</span></span>

```csharp
public class HomeController : Controller
{
    MyDependency _dependency = new MyDependency();

    public async Task<IActionResult> Index()
    {
        await _myDependency.WriteMessage(
            "HomeController.Index created this message.");

        return View();
    }
}
```

::: moniker-end

<span data-ttu-id="22cfe-115">A classe cria e depende diretamente da instância `MyDependency`.</span><span class="sxs-lookup"><span data-stu-id="22cfe-115">The class creates and directly depends on the `MyDependency` instance.</span></span> <span data-ttu-id="22cfe-116">As dependências de código (como no exemplo anterior) são problemáticas e devem ser evitadas por estes motivos:</span><span class="sxs-lookup"><span data-stu-id="22cfe-116">Code dependencies (such as the previous example) are problematic and should be avoided for the following reasons:</span></span>

* <span data-ttu-id="22cfe-117">Para substituir `MyDependency` por uma implementação diferente, a classe deve ser modificada.</span><span class="sxs-lookup"><span data-stu-id="22cfe-117">To replace `MyDependency` with a different implementation, the class must be modified.</span></span>
* <span data-ttu-id="22cfe-118">Se `MyDependency` tiver dependências, elas deverão ser configuradas pela classe.</span><span class="sxs-lookup"><span data-stu-id="22cfe-118">If `MyDependency` has dependencies, they must be configured by the class.</span></span> <span data-ttu-id="22cfe-119">Em um projeto grande com várias classes dependendo da `MyDependency`, o código de configuração fica pulverizado por todo o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="22cfe-119">In a large project with multiple classes depending on `MyDependency`, the configuration code becomes scattered across the app.</span></span>
* <span data-ttu-id="22cfe-120">É difícil testar a unidade dessa implementação.</span><span class="sxs-lookup"><span data-stu-id="22cfe-120">This implementation is difficult to unit test.</span></span> <span data-ttu-id="22cfe-121">O aplicativo deve usar uma simulação ou stub da classe `MyDependency`, o que não é possível com essa abordagem.</span><span class="sxs-lookup"><span data-stu-id="22cfe-121">The app should use a mock or stub `MyDependency` class, which isn't possible with this approach.</span></span>

<span data-ttu-id="22cfe-122">Injeção de dependência trata desses problemas da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="22cfe-122">Dependency injection addresses these problems through:</span></span>

* <span data-ttu-id="22cfe-123">Usando uma interface para abstrair a implementação da dependência.</span><span class="sxs-lookup"><span data-stu-id="22cfe-123">The use of an interface to abstract the dependency implementation.</span></span>
* <span data-ttu-id="22cfe-124">Registrando a dependência em um contêiner de serviço.</span><span class="sxs-lookup"><span data-stu-id="22cfe-124">Registration of the dependency in a service container.</span></span> <span data-ttu-id="22cfe-125">O ASP.NET Core fornece um contêiner de serviço interno, o [IServiceProvider](/dotnet/api/system.iserviceprovider).</span><span class="sxs-lookup"><span data-stu-id="22cfe-125">ASP.NET Core provides a built-in service container, [IServiceProvider](/dotnet/api/system.iserviceprovider).</span></span> <span data-ttu-id="22cfe-126">Os serviços são registrados no método `Startup.ConfigureServices` do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="22cfe-126">Services are registered in the app's `Startup.ConfigureServices` method.</span></span>
* <span data-ttu-id="22cfe-127">*Injeção* do serviço no construtor da classe na qual ele é usado.</span><span class="sxs-lookup"><span data-stu-id="22cfe-127">*Injection* of the service into the constructor of the class where it's used.</span></span> <span data-ttu-id="22cfe-128">A estrutura assume a responsabilidade de criar uma instância da dependência e de descartá-la quando não for mais necessária.</span><span class="sxs-lookup"><span data-stu-id="22cfe-128">The framework takes on the responsibility of creating an instance of the dependency and disposing of it when it's no longer needed.</span></span>

<span data-ttu-id="22cfe-129">No [exemplo de aplicativo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples), a interface `IMyDependency` define um método que o serviço fornece ao aplicativo:</span><span class="sxs-lookup"><span data-stu-id="22cfe-129">In the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples), the `IMyDependency` interface defines a method that the service provides to the app:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Interfaces/IMyDependency.cs?name=snippet1)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Interfaces/IMyDependency.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="22cfe-130">Essa interface é implementada por um tipo concreto, `MyDependency`:</span><span class="sxs-lookup"><span data-stu-id="22cfe-130">This interface is implemented by a concrete type, `MyDependency`:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Services/MyDependency.cs?name=snippet1)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Services/MyDependency.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="22cfe-131">`MyDependency` solicita um [ILogger&lt;TCategoryName&gt; ](/dotnet/api/microsoft.extensions.logging.ilogger-1) em seu construtor.</span><span class="sxs-lookup"><span data-stu-id="22cfe-131">`MyDependency` requests an [ILogger&lt;TCategoryName&gt;](/dotnet/api/microsoft.extensions.logging.ilogger-1) in its constructor.</span></span> <span data-ttu-id="22cfe-132">Não é incomum usar a injeção de dependência de uma maneira encadeada.</span><span class="sxs-lookup"><span data-stu-id="22cfe-132">It's not unusual to use dependency injection in a chained fashion.</span></span> <span data-ttu-id="22cfe-133">Por sua vez, cada dependência solicitada solicita suas próprias dependências.</span><span class="sxs-lookup"><span data-stu-id="22cfe-133">Each requested dependency in turn requests its own dependencies.</span></span> <span data-ttu-id="22cfe-134">O contêiner resolve as dependências no grafo e retorna o serviço totalmente resolvido.</span><span class="sxs-lookup"><span data-stu-id="22cfe-134">The container resolves the dependencies in the graph and returns the fully resolved service.</span></span> <span data-ttu-id="22cfe-135">O conjunto de dependências que precisa ser resolvido normalmente é chamado de *árvore de dependência*, *grafo de dependência* ou *grafo de objeto*.</span><span class="sxs-lookup"><span data-stu-id="22cfe-135">The collective set of dependencies that must be resolved is typically referred to as a *dependency tree*, *dependency graph*, or *object graph*.</span></span>

<span data-ttu-id="22cfe-136">`IMyDependency` e `ILogger<TCategoryName>` devem ser registrados no contêiner do serviço.</span><span class="sxs-lookup"><span data-stu-id="22cfe-136">`IMyDependency` and `ILogger<TCategoryName>` must be registered in the service container.</span></span> <span data-ttu-id="22cfe-137">`IMyDependency` está registrado em `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="22cfe-137">`IMyDependency` is registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="22cfe-138">`ILogger<TCategoryName>` é registrado pela infraestrutura de abstrações de registro em log, portanto, é um [serviço fornecido por estrutura](#framework-provided-services) registrado por padrão pela estrutura.</span><span class="sxs-lookup"><span data-stu-id="22cfe-138">`ILogger<TCategoryName>` is registered by the logging abstractions infrastructure, so it's a [framework-provided service](#framework-provided-services) registered by default by the framework.</span></span>

<span data-ttu-id="22cfe-139">No exemplo de aplicativo, o serviço `IMyDependency` está registrado com o tipo concreto `MyDependency`.</span><span class="sxs-lookup"><span data-stu-id="22cfe-139">In the sample app, the `IMyDependency` service is registered with the concrete type `MyDependency`.</span></span> <span data-ttu-id="22cfe-140">O registro define o escopo do tempo de vida do serviço para o tempo de vida de uma única solicitação.</span><span class="sxs-lookup"><span data-stu-id="22cfe-140">The registration scopes the service lifetime to the lifetime of a single request.</span></span> <span data-ttu-id="22cfe-141">Descreveremos posteriormente neste tópico os [tempos de vida do serviço](#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="22cfe-141">[Service lifetimes](#service-lifetimes) are described later in this topic.</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=11)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=5)]

::: moniker-end

> [!NOTE]
> <span data-ttu-id="22cfe-142">Cada método de extensão `services.Add<ServiceName>` adiciona (e possivelmente configura) serviços.</span><span class="sxs-lookup"><span data-stu-id="22cfe-142">Each `services.Add<ServiceName>` extension method adds (and potentially configures) services.</span></span> <span data-ttu-id="22cfe-143">Por exemplo, `services.AddMvc()` adiciona os serviços exigidos pelo MVC e por Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="22cfe-143">For example, `services.AddMvc()` adds the services Razor Pages and MVC require.</span></span> <span data-ttu-id="22cfe-144">Recomendamos que os aplicativos sigam essa convenção.</span><span class="sxs-lookup"><span data-stu-id="22cfe-144">We recommended that apps follow this convention.</span></span> <span data-ttu-id="22cfe-145">Coloque os métodos de extensão no namespace [Microsoft.Extensions.DependencyInjection](/dotnet/api/microsoft.extensions.dependencyinjection) para encapsular grupos de registros de serviço.</span><span class="sxs-lookup"><span data-stu-id="22cfe-145">Place extension methods in the [Microsoft.Extensions.DependencyInjection](/dotnet/api/microsoft.extensions.dependencyinjection) namespace to encapsulate groups of service registrations.</span></span>

<span data-ttu-id="22cfe-146">Se o construtor do serviço exigir um primitivo, como um `string`, o primitivo poderá ser injetado usando a [configuração](xref:fundamentals/configuration/index) ou o [padrão de opções](xref:fundamentals/configuration/options):</span><span class="sxs-lookup"><span data-stu-id="22cfe-146">If the service's constructor requires a primitive, such as a `string`, the primitive can be injected by using [configuration](xref:fundamentals/configuration/index) or the [options pattern](xref:fundamentals/configuration/options):</span></span>

```csharp
public class MyDependency : IMyDependency
{
    public MyDependency(IConfiguration config)
    {
        var myStringValue = config["MyStringKey"];

        // Use myStringValue
    }

    ...
}
```

<span data-ttu-id="22cfe-147">Uma instância do serviço é solicitada por meio do construtor de uma classe, onde o serviço é usado e atribuído a um campo particular.</span><span class="sxs-lookup"><span data-stu-id="22cfe-147">An instance of the service is requested via the constructor of a class where the service is used and assigned to a private field.</span></span> <span data-ttu-id="22cfe-148">O campo é usado para acessar o serviço conforme o necessário na classe.</span><span class="sxs-lookup"><span data-stu-id="22cfe-148">The field is used to access the service as necessary throughout the class.</span></span>

<span data-ttu-id="22cfe-149">No exemplo de aplicativo, a instância `IMyDependency` é solicitada e usada para chamar o método `WriteMessage` do serviço:</span><span class="sxs-lookup"><span data-stu-id="22cfe-149">In the sample app, the `IMyDependency` instance is requested and used to call the service's `WriteMessage` method:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3,6,13,29-30)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Controllers/MyDependencyController.cs?name=snippet1&highlight=3,5-8,13-14)]

::: moniker-end

## <a name="framework-provided-services"></a><span data-ttu-id="22cfe-150">Serviços fornecidos pela estrutura</span><span class="sxs-lookup"><span data-stu-id="22cfe-150">Framework-provided services</span></span>

<span data-ttu-id="22cfe-151">O método `Startup.ConfigureServices` é responsável por definir os serviços usados pelo aplicativo, incluindo recursos de plataforma como o Entity Framework Core e o ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="22cfe-151">The `Startup.ConfigureServices` method is responsible for defining the services the app uses, including platform features, such as Entity Framework Core and ASP.NET Core MVC.</span></span> <span data-ttu-id="22cfe-152">Inicialmente, a `IServiceCollection` fornecida ao `ConfigureServices` tem os seguintes serviços definidos (dependendo de [como o host foi configurado](xref:fundamentals/host/index)):</span><span class="sxs-lookup"><span data-stu-id="22cfe-152">Initially, the `IServiceCollection` provided to `ConfigureServices` has the following services defined (depending on [how the host was configured](xref:fundamentals/host/index)):</span></span>

| <span data-ttu-id="22cfe-153">Tipo de serviço</span><span class="sxs-lookup"><span data-stu-id="22cfe-153">Service Type</span></span> | <span data-ttu-id="22cfe-154">Tempo de vida</span><span class="sxs-lookup"><span data-stu-id="22cfe-154">Lifetime</span></span> |
| ------------ | -------- |
| [<span data-ttu-id="22cfe-155">Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory</span><span class="sxs-lookup"><span data-stu-id="22cfe-155">Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.builder.iapplicationbuilderfactory) | <span data-ttu-id="22cfe-156">Transitório</span><span class="sxs-lookup"><span data-stu-id="22cfe-156">Transient</span></span> |
| [<span data-ttu-id="22cfe-157">Microsoft.AspNetCore.Hosting.IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="22cfe-157">Microsoft.AspNetCore.Hosting.IApplicationLifetime</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) | <span data-ttu-id="22cfe-158">Singleton</span><span class="sxs-lookup"><span data-stu-id="22cfe-158">Singleton</span></span> |
| [<span data-ttu-id="22cfe-159">Microsoft.AspNetCore.Hosting.IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="22cfe-159">Microsoft.AspNetCore.Hosting.IHostingEnvironment</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) | <span data-ttu-id="22cfe-160">Singleton</span><span class="sxs-lookup"><span data-stu-id="22cfe-160">Singleton</span></span> |
| [<span data-ttu-id="22cfe-161">Microsoft.AspNetCore.Hosting.IStartup</span><span class="sxs-lookup"><span data-stu-id="22cfe-161">Microsoft.AspNetCore.Hosting.IStartup</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.istartup) | <span data-ttu-id="22cfe-162">Singleton</span><span class="sxs-lookup"><span data-stu-id="22cfe-162">Singleton</span></span> |
| [<span data-ttu-id="22cfe-163">Microsoft.AspNetCore.Hosting.IStartupFilter</span><span class="sxs-lookup"><span data-stu-id="22cfe-163">Microsoft.AspNetCore.Hosting.IStartupFilter</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter) | <span data-ttu-id="22cfe-164">Transitório</span><span class="sxs-lookup"><span data-stu-id="22cfe-164">Transient</span></span> |
| [<span data-ttu-id="22cfe-165">Microsoft.AspNetCore.Hosting.Server.IServer</span><span class="sxs-lookup"><span data-stu-id="22cfe-165">Microsoft.AspNetCore.Hosting.Server.IServer</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.server.iserver) | <span data-ttu-id="22cfe-166">Singleton</span><span class="sxs-lookup"><span data-stu-id="22cfe-166">Singleton</span></span> |
| [<span data-ttu-id="22cfe-167">Microsoft.AspNetCore.Http.IHttpContextFactory</span><span class="sxs-lookup"><span data-stu-id="22cfe-167">Microsoft.AspNetCore.Http.IHttpContextFactory</span></span>](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextfactory) | <span data-ttu-id="22cfe-168">Transitório</span><span class="sxs-lookup"><span data-stu-id="22cfe-168">Transient</span></span> |
| [<span data-ttu-id="22cfe-169">Microsoft.Extensions.Logging.ILogger&lt;T&gt;</span><span class="sxs-lookup"><span data-stu-id="22cfe-169">Microsoft.Extensions.Logging.ILogger&lt;T&gt;</span></span>](/dotnet/api/microsoft.extensions.logging.ilogger) | <span data-ttu-id="22cfe-170">Singleton</span><span class="sxs-lookup"><span data-stu-id="22cfe-170">Singleton</span></span> |
| [<span data-ttu-id="22cfe-171">Microsoft.Extensions.Logging.ILoggerFactory</span><span class="sxs-lookup"><span data-stu-id="22cfe-171">Microsoft.Extensions.Logging.ILoggerFactory</span></span>](/dotnet/api/microsoft.extensions.logging.iloggerfactory) | <span data-ttu-id="22cfe-172">Singleton</span><span class="sxs-lookup"><span data-stu-id="22cfe-172">Singleton</span></span> |
| [<span data-ttu-id="22cfe-173">Microsoft.Extensions.ObjectPool.ObjectPoolProvider</span><span class="sxs-lookup"><span data-stu-id="22cfe-173">Microsoft.Extensions.ObjectPool.ObjectPoolProvider</span></span>](/dotnet/api/microsoft.extensions.objectpool.objectpoolprovider) | <span data-ttu-id="22cfe-174">Singleton</span><span class="sxs-lookup"><span data-stu-id="22cfe-174">Singleton</span></span> |
| [<span data-ttu-id="22cfe-175">Microsoft.Extensions.Options.IConfigureOptions&lt;T&gt;</span><span class="sxs-lookup"><span data-stu-id="22cfe-175">Microsoft.Extensions.Options.IConfigureOptions&lt;T&gt;</span></span>](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) | <span data-ttu-id="22cfe-176">Transitório</span><span class="sxs-lookup"><span data-stu-id="22cfe-176">Transient</span></span> |
| [<span data-ttu-id="22cfe-177">Microsoft.Extensions.Options.IOptions&lt;T&gt;</span><span class="sxs-lookup"><span data-stu-id="22cfe-177">Microsoft.Extensions.Options.IOptions&lt;T&gt;</span></span>](/dotnet/api/microsoft.extensions.options.ioptions-1) | <span data-ttu-id="22cfe-178">Singleton</span><span class="sxs-lookup"><span data-stu-id="22cfe-178">Singleton</span></span> |
| [<span data-ttu-id="22cfe-179">System.Diagnostics.DiagnosticSource</span><span class="sxs-lookup"><span data-stu-id="22cfe-179">System.Diagnostics.DiagnosticSource</span></span>](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.diagnosticsource) | <span data-ttu-id="22cfe-180">Singleton</span><span class="sxs-lookup"><span data-stu-id="22cfe-180">Singleton</span></span> |
| [<span data-ttu-id="22cfe-181">System.Diagnostics.DiagnosticListener</span><span class="sxs-lookup"><span data-stu-id="22cfe-181">System.Diagnostics.DiagnosticListener</span></span>](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.diagnosticlistener) | <span data-ttu-id="22cfe-182">Singleton</span><span class="sxs-lookup"><span data-stu-id="22cfe-182">Singleton</span></span> |

<span data-ttu-id="22cfe-183">Quando um método de extensão de coleta do serviço estiver disponível para registrar um serviço (e seus serviços dependentes, se for necessário), a convenção é usar um único método de extensão `Add<ServiceName>` para registrar todos os serviços exigidos por esse serviço.</span><span class="sxs-lookup"><span data-stu-id="22cfe-183">When a service collection extension method is available to register a service (and its dependent services, if required), the convention is to use a single `Add<ServiceName>` extension method to register all of the services required by that service.</span></span> <span data-ttu-id="22cfe-184">O código a seguir é um exemplo de como adicionar outros serviços ao contêiner usando os métodos de extensão [AddDbContext](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext), [AddIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.addidentity) e [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc):</span><span class="sxs-lookup"><span data-stu-id="22cfe-184">The following code is an example of how to add additional services to the container using the extension methods [AddDbContext](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext), [AddIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.addidentity), and [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("DefaultConnection")));

    services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

    services.AddMvc();
}
```

<span data-ttu-id="22cfe-185">Para saber mais, confira a [Classe ServiceCollection](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollection) na documentação da API.</span><span class="sxs-lookup"><span data-stu-id="22cfe-185">For more information, see the [ServiceCollection Class](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollection) in the API documentation.</span></span>

## <a name="service-lifetimes"></a><span data-ttu-id="22cfe-186">Tempos de vida do serviço</span><span class="sxs-lookup"><span data-stu-id="22cfe-186">Service lifetimes</span></span>

<span data-ttu-id="22cfe-187">Escolha um tempo de vida apropriado para cada serviço registrado.</span><span class="sxs-lookup"><span data-stu-id="22cfe-187">Choose an appropriate lifetime for each registered service.</span></span> <span data-ttu-id="22cfe-188">Os serviços do ASP.NET Core podem ser configurados com os seguintes tempos de vida:</span><span class="sxs-lookup"><span data-stu-id="22cfe-188">ASP.NET Core services can be configured with the following lifetimes:</span></span>

<span data-ttu-id="22cfe-189">**Transitório**</span><span class="sxs-lookup"><span data-stu-id="22cfe-189">**Transient**</span></span>

<span data-ttu-id="22cfe-190">Os serviços com tempo de vida transitório são criados sempre que são solicitados.</span><span class="sxs-lookup"><span data-stu-id="22cfe-190">Transient lifetime services are created each time they're requested.</span></span> <span data-ttu-id="22cfe-191">Esse tempo de vida funciona melhor para serviços leves e sem estado.</span><span class="sxs-lookup"><span data-stu-id="22cfe-191">This lifetime works best for lightweight, stateless services.</span></span>

<span data-ttu-id="22cfe-192">**Com escopo**</span><span class="sxs-lookup"><span data-stu-id="22cfe-192">**Scoped**</span></span>

<span data-ttu-id="22cfe-193">Os serviços com tempo de vida com escopo são criados uma vez por solicitação.</span><span class="sxs-lookup"><span data-stu-id="22cfe-193">Scoped lifetime services are created once per request.</span></span>

> [!WARNING]
> <span data-ttu-id="22cfe-194">Ao usar um serviço com escopo em um middleware, injete o serviço no método `Invoke` ou `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="22cfe-194">When using a scoped service in a middleware, inject the service into the `Invoke` or `InvokeAsync` method.</span></span> <span data-ttu-id="22cfe-195">Não injete por meio de injeção de construtor porque isso força o serviço a se comportar como um singleton.</span><span class="sxs-lookup"><span data-stu-id="22cfe-195">Don't inject via constructor injection because it forces the service to behave like a singleton.</span></span> <span data-ttu-id="22cfe-196">Para obter mais informações, consulte <xref:fundamentals/middleware/index>.</span><span class="sxs-lookup"><span data-stu-id="22cfe-196">For more information, see <xref:fundamentals/middleware/index>.</span></span>

<span data-ttu-id="22cfe-197">**Singleton**</span><span class="sxs-lookup"><span data-stu-id="22cfe-197">**Singleton**</span></span>

<span data-ttu-id="22cfe-198">Serviços de tempo de vida singleton são criados na primeira solicitação (ou quando `ConfigureServices` é executado e uma instância é especificada com o registro do serviço).</span><span class="sxs-lookup"><span data-stu-id="22cfe-198">Singleton lifetime services are created the first time they're requested (or when `ConfigureServices` is run and an instance is specified with the service registration).</span></span> <span data-ttu-id="22cfe-199">Cada solicitação subsequente usa a mesma instância.</span><span class="sxs-lookup"><span data-stu-id="22cfe-199">Every subsequent request uses the same instance.</span></span> <span data-ttu-id="22cfe-200">Se o aplicativo exigir um comportamento de singleton, recomendamos permitir que o contêiner do serviço gerencie o tempo de vida do serviço.</span><span class="sxs-lookup"><span data-stu-id="22cfe-200">If the app requires singleton behavior, allowing the service container to manage the service's lifetime is recommended.</span></span> <span data-ttu-id="22cfe-201">Não implemente o padrão de design singleton e forneça o código de usuário para gerenciar o tempo de vida do objeto na classe.</span><span class="sxs-lookup"><span data-stu-id="22cfe-201">Don't implement the singleton design pattern and provide user code to manage the object's lifetime in the class.</span></span>

> [!WARNING]
> <span data-ttu-id="22cfe-202">É perigoso resolver um serviço com escopo de um singleton.</span><span class="sxs-lookup"><span data-stu-id="22cfe-202">It's dangerous to resolve a scoped service from a singleton.</span></span> <span data-ttu-id="22cfe-203">Pode fazer com que o serviço tenha um estado incorreto durante o processamento das solicitações seguintes.</span><span class="sxs-lookup"><span data-stu-id="22cfe-203">It may cause the service to have incorrect state when processing subsequent requests.</span></span>

### <a name="constructor-injection-behavior"></a><span data-ttu-id="22cfe-204">Comportamento da injeção de construtor</span><span class="sxs-lookup"><span data-stu-id="22cfe-204">Constructor injection behavior</span></span>

<span data-ttu-id="22cfe-205">Os serviços podem ser resolvidos por dois mecanismos:</span><span class="sxs-lookup"><span data-stu-id="22cfe-205">Services can be resolved by two mechanisms:</span></span>

* `IServiceProvider`
* <span data-ttu-id="22cfe-206">[ActivatorUtilities](/dotnet/api/microsoft.extensions.dependencyinjection.activatorutilities) &ndash; Permite a criação de objetos sem registro do serviço no contêiner de injeção de dependência.</span><span class="sxs-lookup"><span data-stu-id="22cfe-206">[ActivatorUtilities](/dotnet/api/microsoft.extensions.dependencyinjection.activatorutilities) &ndash; Permits object creation without service registration in the dependency injection container.</span></span> <span data-ttu-id="22cfe-207">`ActivatorUtilities` é usado com abstrações voltadas ao usuário, como Auxiliares de Marca, controladores de MVC, Hubs de SignalR e associadores de modelo.</span><span class="sxs-lookup"><span data-stu-id="22cfe-207">`ActivatorUtilities` is used with user-facing abstractions, such as Tag Helpers, MVC controllers, SignalR Hubs, and model binders.</span></span>

<span data-ttu-id="22cfe-208">Os construtores podem aceitar argumentos que não são fornecidos pela injeção de dependência, mas que precisam atribuir valores padrão.</span><span class="sxs-lookup"><span data-stu-id="22cfe-208">Constructors can accept arguments that aren't provided by dependency injection, but the arguments must assign default values.</span></span>

<span data-ttu-id="22cfe-209">Quando os serviços são resolvidos por `IServiceProvider` ou `ActivatorUtilities`, a injeção do construtor exige um construtor *público*.</span><span class="sxs-lookup"><span data-stu-id="22cfe-209">When services are resolved by `IServiceProvider` or `ActivatorUtilities`, constructor injection requires a *public* constructor.</span></span>

<span data-ttu-id="22cfe-210">Quando os serviços são resolvidos por `ActivatorUtilities`, a injeção de construtor exige a existência apenas de um construtor aplicável.</span><span class="sxs-lookup"><span data-stu-id="22cfe-210">When services are resolved by `ActivatorUtilities`, constructor injection requires that only one applicable constructor exist.</span></span> <span data-ttu-id="22cfe-211">Há suporte para sobrecargas de construtor, mas somente uma sobrecarga pode existir, cujos argumentos podem ser todos atendidos pela injeção de dependência.</span><span class="sxs-lookup"><span data-stu-id="22cfe-211">Constructor overloads are supported, but only one overload can exist whose arguments can all be fulfilled by dependency injection.</span></span>

## <a name="entity-framework-contexts"></a><span data-ttu-id="22cfe-212">Contextos de Entity Framework</span><span class="sxs-lookup"><span data-stu-id="22cfe-212">Entity Framework contexts</span></span>

<span data-ttu-id="22cfe-213">Os contextos do Entity Framework devem ser adicionados ao contêiner de serviços usando o tempo de vida com escopo.</span><span class="sxs-lookup"><span data-stu-id="22cfe-213">Entity Framework contexts should be added to the service container using the scoped lifetime.</span></span> <span data-ttu-id="22cfe-214">Isso é tratado automaticamente com uma chamada para o método [AddDbContext](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) ao registrar o contexto do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="22cfe-214">This is handled automatically with a call to the [AddDbContext](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) method when registering the database context.</span></span> <span data-ttu-id="22cfe-215">Os serviços que usam o contexto de banco de dados também devem usar o tempo de vida com escopo.</span><span class="sxs-lookup"><span data-stu-id="22cfe-215">Services that use the database context should also use the scoped lifetime.</span></span>

## <a name="lifetime-and-registration-options"></a><span data-ttu-id="22cfe-216">Opções de tempo de vida e de registro</span><span class="sxs-lookup"><span data-stu-id="22cfe-216">Lifetime and registration options</span></span>

<span data-ttu-id="22cfe-217">Para demonstrar a diferença entre as opções de tempo de vida e de registro, considere as interfaces a seguir, que representam tarefas como uma operação com um identificador exclusivo, `OperationId`.</span><span class="sxs-lookup"><span data-stu-id="22cfe-217">To demonstrate the difference between the lifetime and registration options, consider the following interfaces that represent tasks as an operation with a unique identifier, `OperationId`.</span></span> <span data-ttu-id="22cfe-218">Dependendo de como o tempo de vida de um serviço de operações é configurado para as interfaces a seguir, o contêiner fornece a mesma instância do serviço, ou outra diferente, quando recebe uma solicitação de uma classe:</span><span class="sxs-lookup"><span data-stu-id="22cfe-218">Depending on how the lifetime of an operations service is configured for the following interfaces, the container provides either the same or a different instance of the service when requested by a class:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Interfaces/IOperation.cs?name=snippet1)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Interfaces/IOperation.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="22cfe-219">As interfaces são implementadas na classe `Operation`.</span><span class="sxs-lookup"><span data-stu-id="22cfe-219">The interfaces are implemented in the `Operation` class.</span></span> <span data-ttu-id="22cfe-220">O construtor `Operation` gera um GUID, caso um não seja fornecido:</span><span class="sxs-lookup"><span data-stu-id="22cfe-220">The `Operation` constructor generates a GUID if one isn't supplied:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Models/Operation.cs?name=snippet1)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Models/Operation.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="22cfe-221">Há um `OperationService` registrado que depende de cada um dos outros `Operation` tipos.</span><span class="sxs-lookup"><span data-stu-id="22cfe-221">An `OperationService` is registered that depends on each of the other `Operation` types.</span></span> <span data-ttu-id="22cfe-222">Quando `OperationService` é solicitado por meio da injeção de dependência, ele recebe a uma nova instância de cada serviço, ou uma instância existente com base no tempo de vida do serviço dependente.</span><span class="sxs-lookup"><span data-stu-id="22cfe-222">When `OperationService` is requested via dependency injection, it receives either a new instance of each service or an existing instance based on the lifetime of the dependent service.</span></span>

* <span data-ttu-id="22cfe-223">Se os serviços temporários forem criados mediante solicitação, o `OperationsId` do serviço `IOperationTransient` será diferente do `OperationsId` do `OperationService`.</span><span class="sxs-lookup"><span data-stu-id="22cfe-223">If transient services are created when requested, the `OperationsId` of the `IOperationTransient` service is different than the `OperationsId` of the `OperationService`.</span></span> <span data-ttu-id="22cfe-224">`OperationService` recebe uma nova instância da classe `IOperationTransient`.</span><span class="sxs-lookup"><span data-stu-id="22cfe-224">`OperationService` receives a new instance of the `IOperationTransient` class.</span></span> <span data-ttu-id="22cfe-225">A nova instância produz outro `OperationsId`.</span><span class="sxs-lookup"><span data-stu-id="22cfe-225">The new instance yields a different `OperationsId`.</span></span>
* <span data-ttu-id="22cfe-226">Se os serviços com escopo forem criados por solicitação, o `OperationsId` do serviço `IOperationScoped` será o mesmo de `OperationService` dentro de uma solicitação.</span><span class="sxs-lookup"><span data-stu-id="22cfe-226">If scoped services are created per request, the `OperationsId` of the `IOperationScoped` service is the same as that of `OperationService` within a request.</span></span> <span data-ttu-id="22cfe-227">Em todas as solicitações, os dois serviços compartilham um valor de `OperationsId` diferente.</span><span class="sxs-lookup"><span data-stu-id="22cfe-227">Across requests, both services share a different `OperationsId` value.</span></span>
* <span data-ttu-id="22cfe-228">Se os serviços singleton e de instância singleton forem criados uma vez e usados em todas as solicitações e em todos os serviços, o `OperationsId` será constante entre todas as solicitações de serviço.</span><span class="sxs-lookup"><span data-stu-id="22cfe-228">If singleton and singleton-instance services are created once and used across all requests and all services, the `OperationsId` is constant across all service requests.</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Services/OperationService.cs?name=snippet1)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Services/OperationService.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="22cfe-229">Em `Startup.ConfigureServices`, cada tipo é adicionado ao contêiner de acordo com seu tempo de vida nomeado:</span><span class="sxs-lookup"><span data-stu-id="22cfe-229">In `Startup.ConfigureServices`, each type is added to the container according to its named lifetime:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=12-15,18)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=6-9,12)]

::: moniker-end

<span data-ttu-id="22cfe-230">O serviço `IOperationSingletonInstance` está usando uma instância específica com uma ID conhecida de `Guid.Empty`.</span><span class="sxs-lookup"><span data-stu-id="22cfe-230">The `IOperationSingletonInstance` service is using a specific instance with a known ID of `Guid.Empty`.</span></span> <span data-ttu-id="22cfe-231">Fica claro quando esse tipo está em uso (seu GUID contém apenas zeros).</span><span class="sxs-lookup"><span data-stu-id="22cfe-231">It's clear when this type is in use (its GUID is all zeroes).</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="22cfe-232">O exemplo de aplicativo demonstra os tempos de vida do objeto dentro e entre solicitações individuais.</span><span class="sxs-lookup"><span data-stu-id="22cfe-232">The sample app demonstrates object lifetimes within and between individual requests.</span></span> <span data-ttu-id="22cfe-233">O exemplo de aplicativo `IndexModel` solicita cada tipo `IOperation` e o `OperationService`.</span><span class="sxs-lookup"><span data-stu-id="22cfe-233">The sample app's `IndexModel` requests each kind of `IOperation` type and the `OperationService`.</span></span> <span data-ttu-id="22cfe-234">Depois, a página exibe todos os valores de `OperationId` da classe de modelo de página e do serviço por meio de atribuições de propriedade:</span><span class="sxs-lookup"><span data-stu-id="22cfe-234">The page then displays all of the page model class's and service's `OperationId` values through property assignments:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=7-11,14-18,21-25)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="22cfe-235">O exemplo de aplicativo demonstra os tempos de vida do objeto dentro e entre solicitações individuais.</span><span class="sxs-lookup"><span data-stu-id="22cfe-235">The sample app demonstrates object lifetimes within and between individual requests.</span></span> <span data-ttu-id="22cfe-236">O exemplo de aplicativo inclui um `OperationsController` que solicita cada tipo de `IOperation` e o `OperationService`.</span><span class="sxs-lookup"><span data-stu-id="22cfe-236">The sample app includes an `OperationsController` that requests each kind of `IOperation` type and the `OperationService`.</span></span> <span data-ttu-id="22cfe-237">A ação `Index` define os serviços no `ViewBag` para exibição dos valores `OperationId` do serviço:</span><span class="sxs-lookup"><span data-stu-id="22cfe-237">The `Index` action sets the services into the `ViewBag` for display of the service's `OperationId` values:</span></span>

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Controllers/OperationsController.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="22cfe-238">As duas saídas a seguir mostram os resultados das duas solicitações:</span><span class="sxs-lookup"><span data-stu-id="22cfe-238">Two following output shows the results of two requests:</span></span>

<span data-ttu-id="22cfe-239">**Primeira solicitação:**</span><span class="sxs-lookup"><span data-stu-id="22cfe-239">**First request:**</span></span>

<span data-ttu-id="22cfe-240">Operações do controlador:</span><span class="sxs-lookup"><span data-stu-id="22cfe-240">Controller operations:</span></span>

<span data-ttu-id="22cfe-241">Transitória: d233e165-f417-469b-a866-1cf1935d2518</span><span class="sxs-lookup"><span data-stu-id="22cfe-241">Transient: d233e165-f417-469b-a866-1cf1935d2518</span></span>  
<span data-ttu-id="22cfe-242">Com escopo: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span><span class="sxs-lookup"><span data-stu-id="22cfe-242">Scoped: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span></span>  
<span data-ttu-id="22cfe-243">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="22cfe-243">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="22cfe-244">Instância: 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="22cfe-244">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="22cfe-245">`OperationService` operações:</span><span class="sxs-lookup"><span data-stu-id="22cfe-245">`OperationService` operations:</span></span>

<span data-ttu-id="22cfe-246">Transitória: c6b049eb-1318-4e31-90f1-eb2dd849ff64</span><span class="sxs-lookup"><span data-stu-id="22cfe-246">Transient: c6b049eb-1318-4e31-90f1-eb2dd849ff64</span></span>  
<span data-ttu-id="22cfe-247">Com escopo: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span><span class="sxs-lookup"><span data-stu-id="22cfe-247">Scoped: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span></span>  
<span data-ttu-id="22cfe-248">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="22cfe-248">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="22cfe-249">Instância: 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="22cfe-249">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="22cfe-250">**Segunda solicitação:**</span><span class="sxs-lookup"><span data-stu-id="22cfe-250">**Second request:**</span></span>

<span data-ttu-id="22cfe-251">Operações do controlador:</span><span class="sxs-lookup"><span data-stu-id="22cfe-251">Controller operations:</span></span>

<span data-ttu-id="22cfe-252">Transitória: b63bd538-0a37-4ff1-90ba-081c5138dda0</span><span class="sxs-lookup"><span data-stu-id="22cfe-252">Transient: b63bd538-0a37-4ff1-90ba-081c5138dda0</span></span>  
<span data-ttu-id="22cfe-253">Com escopo: 31e820c5-4834-4d22-83fc-a60118acb9f4</span><span class="sxs-lookup"><span data-stu-id="22cfe-253">Scoped: 31e820c5-4834-4d22-83fc-a60118acb9f4</span></span>  
<span data-ttu-id="22cfe-254">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="22cfe-254">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="22cfe-255">Instância: 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="22cfe-255">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="22cfe-256">`OperationService` operações:</span><span class="sxs-lookup"><span data-stu-id="22cfe-256">`OperationService` operations:</span></span>

<span data-ttu-id="22cfe-257">Transitória: c4cbacb8-36a2-436d-81c8-8c1b78808aaf</span><span class="sxs-lookup"><span data-stu-id="22cfe-257">Transient: c4cbacb8-36a2-436d-81c8-8c1b78808aaf</span></span>  
<span data-ttu-id="22cfe-258">Com escopo: 31e820c5-4834-4d22-83fc-a60118acb9f4</span><span class="sxs-lookup"><span data-stu-id="22cfe-258">Scoped: 31e820c5-4834-4d22-83fc-a60118acb9f4</span></span>  
<span data-ttu-id="22cfe-259">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="22cfe-259">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="22cfe-260">Instância: 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="22cfe-260">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="22cfe-261">Observe qual dos valores de `OperationId` varia em uma solicitação, e entre as solicitações:</span><span class="sxs-lookup"><span data-stu-id="22cfe-261">Observe which of the `OperationId` values vary within a request and between requests:</span></span>

* <span data-ttu-id="22cfe-262">Os objetos *transitórios* sempre são diferentes.</span><span class="sxs-lookup"><span data-stu-id="22cfe-262">*Transient* objects are always different.</span></span> <span data-ttu-id="22cfe-263">Observe que o valor `OperationId` transitório da primeira e da segunda solicitações é diferente para as duas operações `OperationService` e em todas as solicitações.</span><span class="sxs-lookup"><span data-stu-id="22cfe-263">Note that the transient `OperationId` value for both the first and second requests are different for both `OperationService` operations and across requests.</span></span> <span data-ttu-id="22cfe-264">Uma nova instância é fornecida para cada solicitação e serviço.</span><span class="sxs-lookup"><span data-stu-id="22cfe-264">A new instance is provided to each service and request.</span></span>
* <span data-ttu-id="22cfe-265">Os objetos *com escopo* são os mesmos em uma solicitação, mas diferentes entre solicitações.</span><span class="sxs-lookup"><span data-stu-id="22cfe-265">*Scoped* objects are the same within a request but different across requests.</span></span>
* <span data-ttu-id="22cfe-266">Os pbjetos *singleton* são os mesmos para cada objeto e solicitação, independentemente de uma instância `Operation` ser fornecida em `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="22cfe-266">*Singleton* objects are the same for every object and every request regardless of whether an `Operation` instance is provided in `ConfigureServices`.</span></span>

## <a name="call-services-from-main"></a><span data-ttu-id="22cfe-267">Chamar os serviços desde o principal</span><span class="sxs-lookup"><span data-stu-id="22cfe-267">Call services from main</span></span>

<span data-ttu-id="22cfe-268">Crie um [IServiceScope](/dotnet/api/microsoft.extensions.dependencyinjection.iservicescope) com [IServiceScopeFactory.CreateScope](/dotnet/api/microsoft.extensions.dependencyinjection.iservicescopefactory.createscope) para resolver um serviço com escopo dentro do escopo do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="22cfe-268">Create an [IServiceScope](/dotnet/api/microsoft.extensions.dependencyinjection.iservicescope) with [IServiceScopeFactory.CreateScope](/dotnet/api/microsoft.extensions.dependencyinjection.iservicescopefactory.createscope) to resolve a scoped service within the app's scope.</span></span> <span data-ttu-id="22cfe-269">Essa abordagem é útil para acessar um serviço com escopo durante a inicialização para executar tarefas de inicialização.</span><span class="sxs-lookup"><span data-stu-id="22cfe-269">This approach is useful to access a scoped service at startup to run initialization tasks.</span></span> <span data-ttu-id="22cfe-270">O exemplo a seguir mostra como obter um contexto para o `MyScopedService` em `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="22cfe-270">The following example shows how to obtain a context for the `MyScopedService` in `Program.Main`:</span></span>

```csharp
public static void Main(string[] args)
{
    var host = CreateWebHostBuilder(args).Build();

    using (var serviceScope = host.Services.CreateScope())
    {
        var services = serviceScope.ServiceProvider;

        try
        {
            var serviceContext = services.GetRequiredService<MyScopedService>();
            // Use the context here
        }
        catch (Exception ex)
        {
            var logger = services.GetRequiredService<ILogger<Program>>();
            logger.LogError(ex, "An error occurred.");
        }
    }

    host.Run();
}
```

## <a name="scope-validation"></a><span data-ttu-id="22cfe-271">Validação de escopo</span><span class="sxs-lookup"><span data-stu-id="22cfe-271">Scope validation</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="22cfe-272">Quando o aplicativo está em execução no ambiente de desenvolvimento, o provedor de serviço padrão executa verificações para saber se:</span><span class="sxs-lookup"><span data-stu-id="22cfe-272">When the app is running in the Development environment, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="22cfe-273">Os serviços com escopo não são resolvidos direta ou indiretamente pelo provedor de serviço raiz.</span><span class="sxs-lookup"><span data-stu-id="22cfe-273">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="22cfe-274">Os serviços com escopo não são injetados direta ou indiretamente em singletons.</span><span class="sxs-lookup"><span data-stu-id="22cfe-274">Scoped services aren't directly or indirectly injected into singletons.</span></span>

::: moniker-end

<span data-ttu-id="22cfe-275">O provedor de serviços raiz é criado quando [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) é chamado.</span><span class="sxs-lookup"><span data-stu-id="22cfe-275">The root service provider is created when [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) is called.</span></span> <span data-ttu-id="22cfe-276">O tempo de vida do provedor de serviço raiz corresponde ao tempo de vida do aplicativo/servidor quando o provedor começa com o aplicativo e é descartado quando o aplicativo é desligado.</span><span class="sxs-lookup"><span data-stu-id="22cfe-276">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="22cfe-277">Os serviços com escopo são descartados pelo contêiner que os criou.</span><span class="sxs-lookup"><span data-stu-id="22cfe-277">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="22cfe-278">Se um serviço com escopo é criado no contêiner raiz, o tempo de vida do serviço é promovido efetivamente para singleton, porque ele só é descartado pelo contêiner raiz quando o aplicativo/servidor é desligado.</span><span class="sxs-lookup"><span data-stu-id="22cfe-278">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="22cfe-279">A validação dos escopos de serviço detecta essas situações quando `BuildServiceProvider` é chamado.</span><span class="sxs-lookup"><span data-stu-id="22cfe-279">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="22cfe-280">Para obter mais informações, consulte <xref:fundamentals/host/web-host#scope-validation>.</span><span class="sxs-lookup"><span data-stu-id="22cfe-280">For more information, see <xref:fundamentals/host/web-host#scope-validation>.</span></span>

## <a name="request-services"></a><span data-ttu-id="22cfe-281">Serviços de solicitação</span><span class="sxs-lookup"><span data-stu-id="22cfe-281">Request Services</span></span>

<span data-ttu-id="22cfe-282">Os serviços disponíveis em uma solicitação do ASP.NET de `HttpContext` são expostos por meio da coleção [HttpContext.RequestServices](/dotnet/api/microsoft.aspnetcore.http.httpcontext.requestservices).</span><span class="sxs-lookup"><span data-stu-id="22cfe-282">The services available within an ASP.NET Core request from `HttpContext` are exposed through the [HttpContext.RequestServices](/dotnet/api/microsoft.aspnetcore.http.httpcontext.requestservices) collection.</span></span>

<span data-ttu-id="22cfe-283">Os Serviços de Solicitação representam os serviços configurados e solicitados como parte do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="22cfe-283">Request Services represent the services configured and requested as part of the app.</span></span> <span data-ttu-id="22cfe-284">Quando os objetos especificam dependências, elas são atendidas pelos tipos encontrados em `RequestServices`, não em `ApplicationServices`.</span><span class="sxs-lookup"><span data-stu-id="22cfe-284">When the objects specify dependencies, these are satisfied by the types found in `RequestServices`, not `ApplicationServices`.</span></span>

<span data-ttu-id="22cfe-285">Em geral, o aplicativo não deve usar essas propriedades diretamente.</span><span class="sxs-lookup"><span data-stu-id="22cfe-285">Generally, the app shouldn't use these properties directly.</span></span> <span data-ttu-id="22cfe-286">Em vez disso, solicite os tipos exigidos pelas classes por meio de construtores de classe e permita que a estrutura injete as dependências.</span><span class="sxs-lookup"><span data-stu-id="22cfe-286">Instead, request the types that classes require via class constructors and allow the framework inject the dependencies.</span></span> <span data-ttu-id="22cfe-287">Isso resulta em classes que são mais fáceis de testar (confira os tópicos [Testar e depurar](xref:test/index)).</span><span class="sxs-lookup"><span data-stu-id="22cfe-287">This yields classes that are easier to test (see the [Test and debug](xref:test/index) topics).</span></span>

> [!NOTE]
> <span data-ttu-id="22cfe-288">Prefira solicitar dependências como parâmetros de construtor para acessar a coleção `RequestServices`.</span><span class="sxs-lookup"><span data-stu-id="22cfe-288">Prefer requesting dependencies as constructor parameters to accessing the `RequestServices` collection.</span></span>

## <a name="design-services-for-dependency-injection"></a><span data-ttu-id="22cfe-289">Projetar serviços para injeção de dependência</span><span class="sxs-lookup"><span data-stu-id="22cfe-289">Design services for dependency injection</span></span>

<span data-ttu-id="22cfe-290">As melhores práticas são:</span><span class="sxs-lookup"><span data-stu-id="22cfe-290">Best practices are to:</span></span>

* <span data-ttu-id="22cfe-291">Projetar serviços para usar a injeção de dependência a fim de obter suas dependências.</span><span class="sxs-lookup"><span data-stu-id="22cfe-291">Design services to use dependency injection to obtain their dependencies.</span></span>
* <span data-ttu-id="22cfe-292">Evite chamadas de método estático com estado (uma prática conhecida como [adesão estática](https://deviq.com/static-cling/)).</span><span class="sxs-lookup"><span data-stu-id="22cfe-292">Avoid stateful, static method calls (a practice known as [static cling](https://deviq.com/static-cling/)).</span></span>
* <span data-ttu-id="22cfe-293">Evite a instanciação direta das classes dependentes em serviços.</span><span class="sxs-lookup"><span data-stu-id="22cfe-293">Avoid direct instantiation of dependent classes within services.</span></span> <span data-ttu-id="22cfe-294">A instanciação direta acopla o código a uma implementação específica.</span><span class="sxs-lookup"><span data-stu-id="22cfe-294">Direct instantiation couples the code to a particular implementation.</span></span>

<span data-ttu-id="22cfe-295">Seguindo os [Princípios SOLID de Design Orientado a Objeto](https://deviq.com/solid/), as classes de aplicativo naturalmente tendem a ser pequenas, bem fatoradas e facilmente testadas.</span><span class="sxs-lookup"><span data-stu-id="22cfe-295">By following the [SOLID Principles of Object Oriented Design](https://deviq.com/solid/), app classes naturally tend to be small, well-factored, and easily tested.</span></span>

<span data-ttu-id="22cfe-296">Se parecer que uma classe tem muitas dependências injetadas, normalmente é um sinal de que a classe tem muitas responsabilidades e está violando o [Princípio da Responsabilidade Única (SRP)](https://deviq.com/single-responsibility-principle/).</span><span class="sxs-lookup"><span data-stu-id="22cfe-296">If a class seems to have too many injected dependencies, this is generally a sign that the class has too many responsibilities and is violating the [Single Responsibility Principle (SRP)](https://deviq.com/single-responsibility-principle/).</span></span> <span data-ttu-id="22cfe-297">Tente refatorar a classe movendo algumas de suas responsabilidades para uma nova classe.</span><span class="sxs-lookup"><span data-stu-id="22cfe-297">Attempt to refactor the class by moving some of its responsibilities into a new class.</span></span> <span data-ttu-id="22cfe-298">Lembre-se de que as classes de modelo de página de Razor Pages e as classes do controlador de MVC devem se concentrar em questões de interface do usuário.</span><span class="sxs-lookup"><span data-stu-id="22cfe-298">Keep in mind that Razor Pages page model classes and MVC controller classes should focus on UI concerns.</span></span> <span data-ttu-id="22cfe-299">Os detalhes de implementação de acesso aos dados e as regras de negócios devem ser mantidos em classes apropriadas a esses [interesses separados](https://deviq.com/separation-of-concerns/).</span><span class="sxs-lookup"><span data-stu-id="22cfe-299">Business rules and data access implementation details should be kept in classes appropriate to these [separate concerns](https://deviq.com/separation-of-concerns/).</span></span>

### <a name="disposal-of-services"></a><span data-ttu-id="22cfe-300">Descarte de serviços</span><span class="sxs-lookup"><span data-stu-id="22cfe-300">Disposal of services</span></span>

<span data-ttu-id="22cfe-301">O contêiner chama `Dispose` para os tipos `IDisposable` criados por ele.</span><span class="sxs-lookup"><span data-stu-id="22cfe-301">The container calls `Dispose` for the `IDisposable` types it creates.</span></span> <span data-ttu-id="22cfe-302">Se uma instância for adicionada ao contêiner pelo código do usuário, ele não será descartado automaticamente.</span><span class="sxs-lookup"><span data-stu-id="22cfe-302">If an instance is added to the container by user code, it isn't disposed automatically.</span></span>

```csharp
// Services that implement IDisposable:
public class Service1 : IDisposable {}
public class Service2 : IDisposable {}
public class Service3 : IDisposable {}

public interface ISomeService {}
public class SomeServiceImplementation : ISomeService, IDisposable {}

public void ConfigureServices(IServiceCollection services)
{
    // The container creates the following instances and disposes them automatically:
    services.AddScoped<Service1>();
    services.AddSingleton<Service2>();
    services.AddSingleton<ISomeService>(sp => new SomeServiceImplementation());

    // The container doesn't create the following instances, so it doesn't dispose of
    // the instances automatically:
    services.AddSingleton<Service3>(new Service3());
    services.AddSingleton(new Service3());
}
```

::: moniker range="= aspnetcore-1.0"

> [!NOTE]
> <span data-ttu-id="22cfe-303">No ASP.NET Core 1.0, as chamadas do contêiner descartam *todos* os objetos `IDisposable`, incluindo aqueles que ele não criou.</span><span class="sxs-lookup"><span data-stu-id="22cfe-303">In ASP.NET Core 1.0, the container calls dispose on *all* `IDisposable` objects, including those it didn't create.</span></span>

::: moniker-end

## <a name="default-service-container-replacement"></a><span data-ttu-id="22cfe-304">Substituição do contêiner de serviço padrão</span><span class="sxs-lookup"><span data-stu-id="22cfe-304">Default service container replacement</span></span>

<span data-ttu-id="22cfe-305">O contêiner de serviços interno serve para atender às necessidades básicas da estrutura, e a maioria dos aplicativos de consumidor tem base nele.</span><span class="sxs-lookup"><span data-stu-id="22cfe-305">The built-in service container is meant to serve the basic needs of the framework and most consumer apps built on it.</span></span> <span data-ttu-id="22cfe-306">No entanto, os desenvolvedores podem substituir o contêiner interno por seu contêiner preferencial.</span><span class="sxs-lookup"><span data-stu-id="22cfe-306">However, developers can replace the built-in container with their preferred container.</span></span> <span data-ttu-id="22cfe-307">O método `Startup.ConfigureServices` normalmente retorna `void`.</span><span class="sxs-lookup"><span data-stu-id="22cfe-307">The `Startup.ConfigureServices` method typically returns `void`.</span></span> <span data-ttu-id="22cfe-308">Se a assinatura do método for alterada para retornar um [IServiceProvider](/dotnet/api/system.iserviceprovider), um contêiner diferente poderá ser configurado e retornado.</span><span class="sxs-lookup"><span data-stu-id="22cfe-308">If the method's signature is changed to return an [IServiceProvider](/dotnet/api/system.iserviceprovider), a different container can be configured and returned.</span></span> <span data-ttu-id="22cfe-309">Há vários contêineres IoC disponíveis para o .NET.</span><span class="sxs-lookup"><span data-stu-id="22cfe-309">There are many IoC containers available for .NET.</span></span> <span data-ttu-id="22cfe-310">No exemplo a seguir, o contêiner [Autofac](https://autofac.org/) é usado:</span><span class="sxs-lookup"><span data-stu-id="22cfe-310">In the following example, the [Autofac](https://autofac.org/) container is used:</span></span>

1. <span data-ttu-id="22cfe-311">Instale os pacotes de contêiner apropriados:</span><span class="sxs-lookup"><span data-stu-id="22cfe-311">Install the appropriate container package(s):</span></span>

    * [<span data-ttu-id="22cfe-312">Autofac</span><span class="sxs-lookup"><span data-stu-id="22cfe-312">Autofac</span></span>](https://www.nuget.org/packages/Autofac/)
    * [<span data-ttu-id="22cfe-313">Autofac.Extensions.DependencyInjection</span><span class="sxs-lookup"><span data-stu-id="22cfe-313">Autofac.Extensions.DependencyInjection</span></span>](https://www.nuget.org/packages/Autofac.Extensions.DependencyInjection/)

2. <span data-ttu-id="22cfe-314">Configure o contêiner no `Startup.ConfigureServices` e retorne um `IServiceProvider`:</span><span class="sxs-lookup"><span data-stu-id="22cfe-314">Configure the container in `Startup.ConfigureServices` and return an `IServiceProvider`:</span></span>

    ```csharp
    public IServiceProvider ConfigureServices(IServiceCollection services)
    {
        services.AddMvc();
        // Add other framework services

        // Add Autofac
        var containerBuilder = new ContainerBuilder();
        containerBuilder.RegisterModule<DefaultModule>();
        containerBuilder.Populate(services);
        var container = containerBuilder.Build();
        return new AutofacServiceProvider(container);
    }
    ```

    <span data-ttu-id="22cfe-315">Para usar um contêiner de terceiros, `Startup.ConfigureServices` deve retornar `IServiceProvider`.</span><span class="sxs-lookup"><span data-stu-id="22cfe-315">To use a 3rd party container, `Startup.ConfigureServices` must return `IServiceProvider`.</span></span>

3. <span data-ttu-id="22cfe-316">Configure o Autofac em `DefaultModule`:</span><span class="sxs-lookup"><span data-stu-id="22cfe-316">Configure Autofac in `DefaultModule`:</span></span>

    ```csharp
    public class DefaultModule : Module
    {
        protected override void Load(ContainerBuilder builder)
        {
            builder.RegisterType<CharacterRepository>().As<ICharacterRepository>();
        }
    }
    ```

<span data-ttu-id="22cfe-317">Em tempo de execução, o Autofac é usado para resolver tipos e injetar dependências.</span><span class="sxs-lookup"><span data-stu-id="22cfe-317">At runtime, Autofac is used to resolve types and inject dependencies.</span></span> <span data-ttu-id="22cfe-318">Para saber mais sobre como usar o Autofac com o ASP.NET Core, consulte a [documentação do Autofac](https://docs.autofac.org/en/latest/integration/aspnetcore.html).</span><span class="sxs-lookup"><span data-stu-id="22cfe-318">To learn more about using Autofac with ASP.NET Core, see the [Autofac documentation](https://docs.autofac.org/en/latest/integration/aspnetcore.html).</span></span>

### <a name="thread-safety"></a><span data-ttu-id="22cfe-319">Acesso thread-safe</span><span class="sxs-lookup"><span data-stu-id="22cfe-319">Thread safety</span></span>

<span data-ttu-id="22cfe-320">Os serviços Singleton precisam ser thread-safe.</span><span class="sxs-lookup"><span data-stu-id="22cfe-320">Singleton services need to be thread safe.</span></span> <span data-ttu-id="22cfe-321">Se um serviço singleton tiver uma dependência em um serviço transitório, o serviço transitório também precisará ser thread-safe, dependendo de como ele é usado pelo singleton.</span><span class="sxs-lookup"><span data-stu-id="22cfe-321">If a singleton service has a dependency on a transient service, the transient service may also need to be thread safe depending how it's used by the singleton.</span></span>

<span data-ttu-id="22cfe-322">O método de fábrica de um único serviço, como o segundo argumento para [AddSingleton&lt;TService&gt;(IServiceCollection, Func&lt;IServiceProvider,TService&gt;)](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions.addsingleton#Microsoft_Extensions_DependencyInjection_ServiceCollectionServiceExtensions_AddSingleton__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Func_System_IServiceProvider___0__), não precisa ser thread-safe.</span><span class="sxs-lookup"><span data-stu-id="22cfe-322">The factory method of single service, such as the second argument to [AddSingleton&lt;TService&gt;(IServiceCollection, Func&lt;IServiceProvider,TService&gt;)](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions.addsingleton#Microsoft_Extensions_DependencyInjection_ServiceCollectionServiceExtensions_AddSingleton__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Func_System_IServiceProvider___0__), doesn't need to be thread-safe.</span></span> <span data-ttu-id="22cfe-323">Como um construtor do tipo (`static`), ele tem garantia de ser chamado uma vez por um único thread.</span><span class="sxs-lookup"><span data-stu-id="22cfe-323">Like a type (`static`) constructor, it's guaranteed to be called once by a single thread.</span></span>

## <a name="recommendations"></a><span data-ttu-id="22cfe-324">Recomendações</span><span class="sxs-lookup"><span data-stu-id="22cfe-324">Recommendations</span></span>

<span data-ttu-id="22cfe-325">Ao trabalhar com a injeção de dependência, lembre-se das seguintes recomendações:</span><span class="sxs-lookup"><span data-stu-id="22cfe-325">When working with dependency injection, keep the following recommendations in mind:</span></span>

* <span data-ttu-id="22cfe-326">Evite armazenar dados e a configuração diretamente no contêiner do serviço.</span><span class="sxs-lookup"><span data-stu-id="22cfe-326">Avoid storing data and configuration directly in the service container.</span></span> <span data-ttu-id="22cfe-327">Por exemplo, o carrinho de compras de um usuário normalmente não deve ser adicionado ao contêiner do serviço.</span><span class="sxs-lookup"><span data-stu-id="22cfe-327">For example, a user's shopping cart shouldn't typically be added to the service container.</span></span> <span data-ttu-id="22cfe-328">A configuração deve usar o [padrão de opções](xref:fundamentals/configuration/options).</span><span class="sxs-lookup"><span data-stu-id="22cfe-328">Configuration should use the [options pattern](xref:fundamentals/configuration/options).</span></span> <span data-ttu-id="22cfe-329">Da mesma forma, evite objetos de "suporte de dados" que existem somente para permitir o acesso a outro objeto.</span><span class="sxs-lookup"><span data-stu-id="22cfe-329">Similarly, avoid "data holder" objects that only exist to allow access to some other object.</span></span> <span data-ttu-id="22cfe-330">É melhor solicitar o item real por meio da injeção de dependência, se for possível.</span><span class="sxs-lookup"><span data-stu-id="22cfe-330">It's better to request the actual item via dependency injection, if possible.</span></span>

* <span data-ttu-id="22cfe-331">Evite o acesso estático aos serviços (por exemplo, digitando estaticamente [IApplicationBuilder.ApplicationServices](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.applicationservices) para usar em outro lugar).</span><span class="sxs-lookup"><span data-stu-id="22cfe-331">Avoid static access to services (for example, statically-typing [IApplicationBuilder.ApplicationServices](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.applicationservices) for use elsewhere).</span></span>

* <span data-ttu-id="22cfe-332">Evite usar o padrão de localizador do serviço (por exemplo, [IServiceProvider.GetService](/dotnet/api/system.iserviceprovider.getservice)).</span><span class="sxs-lookup"><span data-stu-id="22cfe-332">Avoid using the service locator pattern (for example, [IServiceProvider.GetService](/dotnet/api/system.iserviceprovider.getservice)).</span></span>

* <span data-ttu-id="22cfe-333">Evite o acesso estático a `HttpContext` (por exemplo, [IHttpContextAccessor.HttpContext](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor.httpcontext)).</span><span class="sxs-lookup"><span data-stu-id="22cfe-333">Avoid static access to `HttpContext` (for example, [IHttpContextAccessor.HttpContext](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor.httpcontext)).</span></span>

<span data-ttu-id="22cfe-334">Como todos os conjuntos de recomendações, talvez você encontre situações em que é necessário ignorar uma recomendação.</span><span class="sxs-lookup"><span data-stu-id="22cfe-334">Like all sets of recommendations, you may encounter situations where ignoring a recommendation is required.</span></span> <span data-ttu-id="22cfe-335">As exceções são raras &mdash;principalmente casos especiais dentro da própria estrutura.</span><span class="sxs-lookup"><span data-stu-id="22cfe-335">Exceptions are rare&mdash;mostly special cases within the framework itself.</span></span>

<span data-ttu-id="22cfe-336">A injeção de dependência é uma *alternativa* aos padrões de acesso a objeto estático/global.</span><span class="sxs-lookup"><span data-stu-id="22cfe-336">Dependency injection is an *alternative* to static/global object access patterns.</span></span> <span data-ttu-id="22cfe-337">Talvez você não perceba os benefícios da injeção de dependência se combiná-la com o acesso a objeto estático.</span><span class="sxs-lookup"><span data-stu-id="22cfe-337">You may not be able to realize the benefits of dependency injection if you mix it with static object access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="22cfe-338">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="22cfe-338">Additional resources</span></span>

* <xref:mvc/views/dependency-injection>
* <xref:mvc/controllers/dependency-injection>
* <xref:security/authorization/dependencyinjection>
* <xref:fundamentals/repository-pattern>
* <xref:fundamentals/startup>
* <xref:test/index>
* <xref:fundamentals/middleware/extensibility>
* [<span data-ttu-id="22cfe-339">Como escrever um código limpo no ASP.NET Core com injeção de dependência (MSDN)</span><span class="sxs-lookup"><span data-stu-id="22cfe-339">Writing Clean Code in ASP.NET Core with Dependency Injection (MSDN)</span></span>](https://msdn.microsoft.com/magazine/mt703433.aspx)
* [<span data-ttu-id="22cfe-340">Design de aplicativo gerenciado por contêiner, prelúdio: a que local o contêiner pertence?</span><span class="sxs-lookup"><span data-stu-id="22cfe-340">Container-Managed Application Design, Prelude: Where does the Container Belong?</span></span>](https://blogs.msdn.microsoft.com/nblumhardt/2008/12/26/container-managed-application-design-prelude-where-does-the-container-belong/)
* [<span data-ttu-id="22cfe-341">Princípio de Dependências Explícitas</span><span class="sxs-lookup"><span data-stu-id="22cfe-341">Explicit Dependencies Principle</span></span>](https://deviq.com/explicit-dependencies-principle/)
* [<span data-ttu-id="22cfe-342">Inversão de Contêineres de Controle e o padrão de Injeção de Dependência (Martin Fowler)</span><span class="sxs-lookup"><span data-stu-id="22cfe-342">Inversion of Control Containers and the Dependency Injection Pattern (Martin Fowler)</span></span>](https://www.martinfowler.com/articles/injection.html)
* [<span data-ttu-id="22cfe-343">New is Glue ("colando" o código em uma implementação específica)</span><span class="sxs-lookup"><span data-stu-id="22cfe-343">New is Glue ("gluing" code to a particular implementation)</span></span>](https://ardalis.com/new-is-glue)

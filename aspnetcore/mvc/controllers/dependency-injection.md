---
title: "Injeção de dependência nos controladores"
author: ardalis
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: bc8b4ba3-e9ba-48fd-b1eb-cd48ff6bc7a1
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/controllers/dependency-injection
ms.openlocfilehash: b83bd4a24ccf7e90e9df06d6a8e229a2d5c6699a
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/12/2017
---
# <a name="dependency-injection-into-controllers"></a><span data-ttu-id="8ec7b-103">Injeção de dependência nos controladores</span><span class="sxs-lookup"><span data-stu-id="8ec7b-103">Dependency injection into controllers</span></span>

<a name=dependency-injection-controllers></a>

<span data-ttu-id="8ec7b-104">Por [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="8ec7b-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="8ec7b-105">Controladores MVC do ASP.NET Core devem solicitar suas dependências explicitamente por meio de seus construtores.</span><span class="sxs-lookup"><span data-stu-id="8ec7b-105">ASP.NET Core MVC controllers should request their dependencies explicitly via their constructors.</span></span> <span data-ttu-id="8ec7b-106">Em algumas instâncias, ações do controlador individuais podem exigir um serviço e não pode fazer sentido para solicitar ao nível de controlador.</span><span class="sxs-lookup"><span data-stu-id="8ec7b-106">In some instances, individual controller actions may require a service, and it may not make sense to request at the controller level.</span></span> <span data-ttu-id="8ec7b-107">Nesse caso, você também pode optar por injetar um serviço como um parâmetro no método de ação.</span><span class="sxs-lookup"><span data-stu-id="8ec7b-107">In this case, you can also choose to inject a service as a parameter on the action method.</span></span>

[<span data-ttu-id="8ec7b-108">Exibir ou baixar o código de exemplo</span><span class="sxs-lookup"><span data-stu-id="8ec7b-108">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/dependency-injection/sample)

## <a name="dependency-injection"></a><span data-ttu-id="8ec7b-109">Injeção de dependência</span><span class="sxs-lookup"><span data-stu-id="8ec7b-109">Dependency Injection</span></span>

<span data-ttu-id="8ec7b-110">Injeção de dependência é uma técnica que segue o [princípio de inversão de dependência](http://deviq.com/dependency-inversion-principle/), permitindo que aplicativos de ser composto de módulos acoplados de forma flexível.</span><span class="sxs-lookup"><span data-stu-id="8ec7b-110">Dependency injection is a technique that follows the [Dependency Inversion Principle](http://deviq.com/dependency-inversion-principle/), allowing for applications to be composed of loosely coupled modules.</span></span> <span data-ttu-id="8ec7b-111">ASP.NET Core tem suporte interno para [injeção de dependência](../../fundamentals/dependency-injection.md), que torna os aplicativos mais fácil de testar e manter.</span><span class="sxs-lookup"><span data-stu-id="8ec7b-111">ASP.NET Core has built-in support for [dependency injection](../../fundamentals/dependency-injection.md), which makes applications easier to test and maintain.</span></span>

## <a name="constructor-injection"></a><span data-ttu-id="8ec7b-112">Injeção de construtor</span><span class="sxs-lookup"><span data-stu-id="8ec7b-112">Constructor Injection</span></span>

<span data-ttu-id="8ec7b-113">Estende o suporte interno do ASP.NET Core injeção de dependência baseado no construtor para controladores MVC.</span><span class="sxs-lookup"><span data-stu-id="8ec7b-113">ASP.NET Core's built-in support for constructor-based dependency injection extends to MVC controllers.</span></span> <span data-ttu-id="8ec7b-114">Simplesmente adicionando um tipo de serviço para o seu controlador como um parâmetro de construtor, o ASP.NET Core tentará resolver daquele tipo usando seu contêiner de serviço.</span><span class="sxs-lookup"><span data-stu-id="8ec7b-114">By simply adding a service type to your controller as a constructor parameter, ASP.NET Core will attempt to resolve that type using its built in service container.</span></span> <span data-ttu-id="8ec7b-115">Os serviços são normalmente, mas não sempre, definidos usando interfaces.</span><span class="sxs-lookup"><span data-stu-id="8ec7b-115">Services are typically, but not always, defined using interfaces.</span></span> <span data-ttu-id="8ec7b-116">Por exemplo, se seu aplicativo tiver a lógica de negócios que depende do tempo atual, você pode injetar um serviço que recupera o tempo (em vez de embutir em código ele), que permite que os testes passar em implementações que usam um período de tempo.</span><span class="sxs-lookup"><span data-stu-id="8ec7b-116">For example, if your application has business logic that depends on the current time, you can inject a service that retrieves the time (rather than hard-coding it), which would allow your tests to pass in implementations that use a set time.</span></span>

<span data-ttu-id="8ec7b-117">[!code-csharp[Main](dependency-injection/sample/src/ControllerDI/Interfaces/IDateTime.cs)]</span><span class="sxs-lookup"><span data-stu-id="8ec7b-117">[!code-csharp[Main](dependency-injection/sample/src/ControllerDI/Interfaces/IDateTime.cs)]</span></span>


<span data-ttu-id="8ec7b-118">Implementando uma interface como esta para que ele usa o relógio do sistema em tempo de execução é simples:</span><span class="sxs-lookup"><span data-stu-id="8ec7b-118">Implementing an interface like this one so that it uses the system clock at runtime is trivial:</span></span>

<span data-ttu-id="8ec7b-119">[!code-csharp[Main](dependency-injection/sample/src/ControllerDI/Services/SystemDateTime.cs)]</span><span class="sxs-lookup"><span data-stu-id="8ec7b-119">[!code-csharp[Main](dependency-injection/sample/src/ControllerDI/Services/SystemDateTime.cs)]</span></span>


<span data-ttu-id="8ec7b-120">Com isso em vigor, podemos usar o serviço em nosso controlador.</span><span class="sxs-lookup"><span data-stu-id="8ec7b-120">With this in place, we can use the service in our controller.</span></span> <span data-ttu-id="8ec7b-121">Nesse caso, adicionamos alguma lógica para o `HomeController` `Index` método para exibir uma saudação ao usuário com base na hora do dia.</span><span class="sxs-lookup"><span data-stu-id="8ec7b-121">In this case, we have added some logic to the `HomeController` `Index` method to display a greeting to the user based on the time of day.</span></span>

<span data-ttu-id="8ec7b-122">[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Controllers/HomeController.cs?highlight=8,10,12,17,18,19,20,21,22,23,24,25,26,27,28,29,30&range=1-31,51-52)]</span><span class="sxs-lookup"><span data-stu-id="8ec7b-122">[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Controllers/HomeController.cs?highlight=8,10,12,17,18,19,20,21,22,23,24,25,26,27,28,29,30&range=1-31,51-52)]</span></span>

<span data-ttu-id="8ec7b-123">Se executarmos o aplicativo agora, vamos provavelmente encontrará um erro:</span><span class="sxs-lookup"><span data-stu-id="8ec7b-123">If we run the application now, we will most likely encounter an error:</span></span>

<!-- literal_block {"ids": [], "xml:space": "preserve"} -->

```
An unhandled exception occurred while processing the request.

InvalidOperationException: Unable to resolve service for type 'ControllerDI.Interfaces.IDateTime' while attempting to activate 'ControllerDI.Controllers.HomeController'.
Microsoft.Extensions.DependencyInjection.ActivatorUtilities.GetService(IServiceProvider sp, Type type, Type requiredBy, Boolean isDefaultParameterRequired)
```

<span data-ttu-id="8ec7b-124">Esse erro ocorre quando não configuramos um serviço no `ConfigureServices` método em nosso `Startup` classe.</span><span class="sxs-lookup"><span data-stu-id="8ec7b-124">This error occurs when we have not configured a service in the `ConfigureServices` method in our `Startup` class.</span></span> <span data-ttu-id="8ec7b-125">Para especificar que as solicitações de `IDateTime` devem ser resolvidos usando uma instância de `SystemDateTime`, adicione a linha realçada na lista abaixo para sua `ConfigureServices` método:</span><span class="sxs-lookup"><span data-stu-id="8ec7b-125">To specify that requests for `IDateTime` should be resolved using an instance of `SystemDateTime`, add the highlighted line in the listing below to your `ConfigureServices` method:</span></span>

<span data-ttu-id="8ec7b-126">[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Startup.cs?highlight=4&range=26-27,42-44)]</span><span class="sxs-lookup"><span data-stu-id="8ec7b-126">[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Startup.cs?highlight=4&range=26-27,42-44)]</span></span>

> [!NOTE]
> <span data-ttu-id="8ec7b-127">Esse serviço específico pode ser implementado usando qualquer uma das várias opções diferentes de tempo de vida (`Transient`, `Scoped`, ou `Singleton`).</span><span class="sxs-lookup"><span data-stu-id="8ec7b-127">This particular service could be implemented using any of several different lifetime options (`Transient`, `Scoped`, or `Singleton`).</span></span> <span data-ttu-id="8ec7b-128">Consulte [injeção de dependência](../../fundamentals/dependency-injection.md) para entender como cada uma dessas opções de escopo afetará o comportamento do seu serviço.</span><span class="sxs-lookup"><span data-stu-id="8ec7b-128">See [Dependency Injection](../../fundamentals/dependency-injection.md) to understand how each of these scope options will affect the behavior of your service.</span></span>

<span data-ttu-id="8ec7b-129">Depois que o serviço tiver sido configurado, executando o aplicativo e navegar para a home page devem exibir a mensagem de tempo conforme o esperado:</span><span class="sxs-lookup"><span data-stu-id="8ec7b-129">Once the service has been configured, running the application and navigating to the home page should display the time-based message as expected:</span></span>

![Saudação do servidor](dependency-injection/_static/server-greeting.png)

>[!TIP]
> <span data-ttu-id="8ec7b-131">Consulte [lógica do controlador de teste](testing.md) para saber como solicitar explicitamente dependências [http://deviq.com/explicit-dependencies-principle/](http://deviq.com/explicit-dependencies-principle/) em controladores facilita o código de teste.</span><span class="sxs-lookup"><span data-stu-id="8ec7b-131">See [Testing Controller Logic](testing.md) to learn how to explicitly request dependencies [http://deviq.com/explicit-dependencies-principle/](http://deviq.com/explicit-dependencies-principle/) in controllers makes code easier to test.</span></span>

<span data-ttu-id="8ec7b-132">Injeção de dependência interna do ASP.NET Core dá suporte a apenas um único construtor para classes de solicitação de serviços.</span><span class="sxs-lookup"><span data-stu-id="8ec7b-132">ASP.NET Core's built-in dependency injection supports having only a single constructor for classes requesting services.</span></span> <span data-ttu-id="8ec7b-133">Se você tiver mais de um construtor, você pode receber uma exceção informando:</span><span class="sxs-lookup"><span data-stu-id="8ec7b-133">If you have more than one constructor, you may get an exception stating:</span></span>

<!-- literal_block {"ids": [], "xml:space": "preserve"} -->

```
An unhandled exception occurred while processing the request.

InvalidOperationException: Multiple constructors accepting all given argument types have been found in type 'ControllerDI.Controllers.HomeController'. There should only be one applicable constructor.
Microsoft.Extensions.DependencyInjection.ActivatorUtilities.FindApplicableConstructor(Type instanceType, Type[] argumentTypes, ConstructorInfo& matchingConstructor, Nullable`1[]& parameterMap)
```

<span data-ttu-id="8ec7b-134">Como a mensagem de erro afirma, você pode corrigir esse problema ter apenas um único construtor.</span><span class="sxs-lookup"><span data-stu-id="8ec7b-134">As the error message states, you can correct this problem having just a single constructor.</span></span> <span data-ttu-id="8ec7b-135">Você também pode [substituir o suporte de injeção de dependência padrão com uma implementação de terceiros](../../fundamentals/dependency-injection.md#replacing-the-default-services-container), muitos dos quais oferecem suporte a vários construtores.</span><span class="sxs-lookup"><span data-stu-id="8ec7b-135">You can also [replace the default dependency injection support with a third party implementation](../../fundamentals/dependency-injection.md#replacing-the-default-services-container), many of which support multiple constructors.</span></span>

## <a name="action-injection-with-fromservices"></a><span data-ttu-id="8ec7b-136">Injeção de ação com FromServices</span><span class="sxs-lookup"><span data-stu-id="8ec7b-136">Action Injection with FromServices</span></span>

<span data-ttu-id="8ec7b-137">Às vezes, você não precisa de um serviço para mais de uma ação em seu controlador.</span><span class="sxs-lookup"><span data-stu-id="8ec7b-137">Sometimes you don't need a service for more than one action within your controller.</span></span> <span data-ttu-id="8ec7b-138">Nesse caso, talvez faça sentido para injetar o serviço como um parâmetro para o método de ação.</span><span class="sxs-lookup"><span data-stu-id="8ec7b-138">In this case, it may make sense to inject the service as a parameter to the action method.</span></span> <span data-ttu-id="8ec7b-139">Isso é feito marcando o parâmetro com o atributo `[FromServices]` conforme mostrado aqui:</span><span class="sxs-lookup"><span data-stu-id="8ec7b-139">This is done by marking the parameter with the attribute `[FromServices]` as shown here:</span></span>

<span data-ttu-id="8ec7b-140">[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Controllers/HomeController.cs?highlight=1&range=33-38)]</span><span class="sxs-lookup"><span data-stu-id="8ec7b-140">[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Controllers/HomeController.cs?highlight=1&range=33-38)]</span></span>

## <a name="accessing-settings-from-a-controller"></a><span data-ttu-id="8ec7b-141">Acessando configurações de um controlador</span><span class="sxs-lookup"><span data-stu-id="8ec7b-141">Accessing Settings from a Controller</span></span>

<span data-ttu-id="8ec7b-142">Acessando configurações de aplicativo ou configuração de dentro de um controlador é um padrão comum.</span><span class="sxs-lookup"><span data-stu-id="8ec7b-142">Accessing application or configuration settings from within a controller is a common pattern.</span></span> <span data-ttu-id="8ec7b-143">Esse acesso deve usar o padrão de opções descrito na [configuração](../../fundamentals/configuration.md).</span><span class="sxs-lookup"><span data-stu-id="8ec7b-143">This access should use the Options pattern described in [configuration](../../fundamentals/configuration.md).</span></span> <span data-ttu-id="8ec7b-144">Em geral não solicite configurações diretamente do seu controlador usando a injeção de dependência.</span><span class="sxs-lookup"><span data-stu-id="8ec7b-144">You generally should not request settings directly from your controller using dependency injection.</span></span> <span data-ttu-id="8ec7b-145">Uma abordagem melhor é a solicitação de um `IOptions<T>` instância, onde `T` é a classe de configuração que você precisa.</span><span class="sxs-lookup"><span data-stu-id="8ec7b-145">A better approach is to request an `IOptions<T>` instance, where `T` is the configuration class you need.</span></span>

<span data-ttu-id="8ec7b-146">Para trabalhar com o padrão de opções, você precisa criar uma classe que representa as opções, como este:</span><span class="sxs-lookup"><span data-stu-id="8ec7b-146">To work with the options pattern, you need to create a class that represents the options, such as this one:</span></span>

<span data-ttu-id="8ec7b-147">[!code-csharp[Main](dependency-injection/sample/src/ControllerDI/Model/SampleWebSettings.cs)]</span><span class="sxs-lookup"><span data-stu-id="8ec7b-147">[!code-csharp[Main](dependency-injection/sample/src/ControllerDI/Model/SampleWebSettings.cs)]</span></span>

<span data-ttu-id="8ec7b-148">Em seguida, você precisa configurar o aplicativo para usar o modelo de opções e adicione sua classe de configuração para a coleção de serviços no `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="8ec7b-148">Then you need to configure the application to use the options model and add your configuration class to the services collection in `ConfigureServices`:</span></span>

<span data-ttu-id="8ec7b-149">[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Startup.cs?highlight=3,4,5,6,9,16,19&range=14-44)]</span><span class="sxs-lookup"><span data-stu-id="8ec7b-149">[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Startup.cs?highlight=3,4,5,6,9,16,19&range=14-44)]</span></span>

> [!NOTE]
> <span data-ttu-id="8ec7b-150">Na lista acima, estamos configurando o aplicativo para ler as configurações de um arquivo no formato JSON.</span><span class="sxs-lookup"><span data-stu-id="8ec7b-150">In the above listing, we are configuring the application to read the settings from a JSON-formatted file.</span></span> <span data-ttu-id="8ec7b-151">Você também pode configurar as configurações inteiramente no código, como é mostrado no código comentado acima.</span><span class="sxs-lookup"><span data-stu-id="8ec7b-151">You can also configure the settings entirely in code, as is shown in the commented code above.</span></span> <span data-ttu-id="8ec7b-152">Consulte [configuração](../../fundamentals/configuration.md) para outras opções de configuração.</span><span class="sxs-lookup"><span data-stu-id="8ec7b-152">See [Configuration](../../fundamentals/configuration.md) for further configuration options.</span></span>

<span data-ttu-id="8ec7b-153">Depois que você tiver especificado um objeto de configuração fortemente tipado (nesse caso, `SampleWebSettings`) e ele foi adicionado à coleção de serviços, você pode solicitá-las de qualquer método de ação ou controlador solicitando uma instância de `IOptions<T>` (nesse caso, `IOptions<SampleWebSettings>`) .</span><span class="sxs-lookup"><span data-stu-id="8ec7b-153">Once you've specified a strongly-typed configuration object (in this case, `SampleWebSettings`) and added it to the services collection, you can request it from any Controller or Action method by requesting an instance of `IOptions<T>` (in this case, `IOptions<SampleWebSettings>`).</span></span> <span data-ttu-id="8ec7b-154">O código a seguir mostra como um solicita as configurações de um controlador:</span><span class="sxs-lookup"><span data-stu-id="8ec7b-154">The following code shows how one would request the settings from a controller:</span></span>

<span data-ttu-id="8ec7b-155">[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Controllers/SettingsController.cs?highlight=3,5,7&range=7-22)]</span><span class="sxs-lookup"><span data-stu-id="8ec7b-155">[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Controllers/SettingsController.cs?highlight=3,5,7&range=7-22)]</span></span>

<span data-ttu-id="8ec7b-156">Seguindo o padrão de opções permite que as configurações e seja dissociado uns dos outros e garante que o controlador é após [separação de preocupações](http://deviq.com/separation-of-concerns/), pois ele não precisa saber como e onde encontrar as configurações informações.</span><span class="sxs-lookup"><span data-stu-id="8ec7b-156">Following the Options pattern allows settings and configuration to be decoupled from one another, and ensures the controller is following [separation of concerns](http://deviq.com/separation-of-concerns/), since it doesn't need to know how or where to find the settings information.</span></span> <span data-ttu-id="8ec7b-157">Ele também facilita o controlador de teste de unidade [lógica do controlador de teste](testing.md), porque não há nenhum [adesivos](http://deviq.com/static-cling/) ou instanciação direta de classes de configuração dentro da classe do controlador.</span><span class="sxs-lookup"><span data-stu-id="8ec7b-157">It also makes the controller easier to unit test [Testing Controller Logic](testing.md), since there is no [static cling](http://deviq.com/static-cling/) or direct instantiation of settings classes within the controller class.</span></span>

---
uid: web-api/overview/advanced/dependency-injection
title: "Injeção de dependência no ASP.NET Web API 2 | Microsoft Docs"
author: MikeWasson
description: "Este tutorial mostra como injetar dependências em seu controlador API Web do ASP.NET. Versões de software usadas no tutorial da Web API 2 Unity Application Block..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: e3d3e7ba-87f0-4032-bdd3-31f3c1aa9d9c
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 7f64cc83e36c80b0ffd53edfc629557c0847b200
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="dependency-injection-in-aspnet-web-api-2"></a><span data-ttu-id="2e79d-104">Injeção de dependência no ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="2e79d-104">Dependency Injection in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="2e79d-105">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="2e79d-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="2e79d-106">Baixe o projeto concluído</span><span class="sxs-lookup"><span data-stu-id="2e79d-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-468ee148)

> <span data-ttu-id="2e79d-107">Este tutorial mostra como injetar dependências em seu controlador API Web do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="2e79d-107">This tutorial shows how to inject dependencies into your ASP.NET Web API controller.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="2e79d-108">Versões de software usadas no tutorial</span><span class="sxs-lookup"><span data-stu-id="2e79d-108">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="2e79d-109">Web API 2</span><span class="sxs-lookup"><span data-stu-id="2e79d-109">Web API 2</span></span>
> - [<span data-ttu-id="2e79d-110">Bloco de aplicativos do Unity</span><span class="sxs-lookup"><span data-stu-id="2e79d-110">Unity Application Block</span></span>](https://www.nuget.org/packages/Unity/)
> - <span data-ttu-id="2e79d-111">Entity Framework 6 (versão 5 também funciona)</span><span class="sxs-lookup"><span data-stu-id="2e79d-111">Entity Framework 6 (version 5 also works)</span></span>


## <a name="what-is-dependency-injection"></a><span data-ttu-id="2e79d-112">O que é a injeção de dependência?</span><span class="sxs-lookup"><span data-stu-id="2e79d-112">What is Dependency Injection?</span></span>

<span data-ttu-id="2e79d-113">Um *dependência* é qualquer objeto que requer a outro objeto.</span><span class="sxs-lookup"><span data-stu-id="2e79d-113">A *dependency* is any object that another object requires.</span></span> <span data-ttu-id="2e79d-114">Por exemplo, é comum para definir um [repositório](http://martinfowler.com/eaaCatalog/repository.html) que controla o acesso a dados.</span><span class="sxs-lookup"><span data-stu-id="2e79d-114">For example, it's common to define a [repository](http://martinfowler.com/eaaCatalog/repository.html) that handles data access.</span></span> <span data-ttu-id="2e79d-115">Vamos ilustrar um exemplo.</span><span class="sxs-lookup"><span data-stu-id="2e79d-115">Let's illustrate with an example.</span></span> <span data-ttu-id="2e79d-116">Primeiro, vamos definir um modelo de domínio:</span><span class="sxs-lookup"><span data-stu-id="2e79d-116">First, we'll define a domain model:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

<span data-ttu-id="2e79d-117">Aqui está uma classe simples do repositório que armazena itens em um banco de dados usando o Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="2e79d-117">Here is a simple repository class that stores items in a database, using Entity Framework.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

<span data-ttu-id="2e79d-118">Agora vamos definir um controlador de API da Web que oferece suporte a solicitações GET para `Product` entidades.</span><span class="sxs-lookup"><span data-stu-id="2e79d-118">Now let's define a Web API controller that supports GET requests for `Product` entities.</span></span> <span data-ttu-id="2e79d-119">(Estou deixando POST e outros métodos para simplificar.) Aqui está a primeira tentativa:</span><span class="sxs-lookup"><span data-stu-id="2e79d-119">(I'm leaving out POST and other methods for simplicity.) Here is a first attempt:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

<span data-ttu-id="2e79d-120">Observe que a classe do controlador depende `ProductRepository`, e estão permitindo que o controlador de criar o `ProductRepository` instância.</span><span class="sxs-lookup"><span data-stu-id="2e79d-120">Notice that the controller class depends on `ProductRepository`, and we are letting the controller create the `ProductRepository` instance.</span></span> <span data-ttu-id="2e79d-121">No entanto, é uma boa ideia para codificar a dependência dessa forma, por vários motivos.</span><span class="sxs-lookup"><span data-stu-id="2e79d-121">However, it's a bad idea to hard code the dependency in this way, for several reasons.</span></span>

- <span data-ttu-id="2e79d-122">Se você quiser substituir `ProductRepository` com uma implementação diferente, você também precisará modificar a classe do controlador.</span><span class="sxs-lookup"><span data-stu-id="2e79d-122">If you want to replace `ProductRepository` with a different implementation, you also need to modify the controller class.</span></span>
- <span data-ttu-id="2e79d-123">Se o `ProductRepository` tem dependências, você deve configurá-los dentro do controlador.</span><span class="sxs-lookup"><span data-stu-id="2e79d-123">If the `ProductRepository` has dependencies, you must configure these inside the controller.</span></span> <span data-ttu-id="2e79d-124">Para um projeto grande com vários controladores, o código de configuração se torna espalhado por seu projeto.</span><span class="sxs-lookup"><span data-stu-id="2e79d-124">For a large project with multiple controllers, your configuration code becomes scattered across your project.</span></span>
- <span data-ttu-id="2e79d-125">É difícil para teste de unidade, porque o controlador é codificado para consultar o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="2e79d-125">It is hard to unit test, because the controller is hard-coded to query the database.</span></span> <span data-ttu-id="2e79d-126">Para um teste de unidade, você deve usar um repositório de simulação ou stub, que não é possível com o design de currect.</span><span class="sxs-lookup"><span data-stu-id="2e79d-126">For a unit test, you should use a mock or stub repository, which is not possible with the currect design.</span></span>

<span data-ttu-id="2e79d-127">Podemos resolver esses problemas por *injetando* repositório no controlador.</span><span class="sxs-lookup"><span data-stu-id="2e79d-127">We can address these problems by *injecting* the repository into the controller.</span></span> <span data-ttu-id="2e79d-128">Primeiro, refatorar o `ProductRepository` classe em uma interface:</span><span class="sxs-lookup"><span data-stu-id="2e79d-128">First, refactor the `ProductRepository` class into an interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

<span data-ttu-id="2e79d-129">Em seguida, forneça o `IProductRepository` como um parâmetro de construtor:</span><span class="sxs-lookup"><span data-stu-id="2e79d-129">Then provide the `IProductRepository` as a constructor parameter:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

<span data-ttu-id="2e79d-130">Este exemplo usa [injeção de construtor](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span><span class="sxs-lookup"><span data-stu-id="2e79d-130">This example uses [constructor injection](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span></span> <span data-ttu-id="2e79d-131">Você também pode usar *injeção de setter*, onde você pode definir a dependência por meio de uma propriedade ou método de setter.</span><span class="sxs-lookup"><span data-stu-id="2e79d-131">You can also use *setter injection*, where you set the dependency through a setter method or property.</span></span>

<span data-ttu-id="2e79d-132">Mas agora há um problema, pois o seu aplicativo não cria o controlador diretamente.</span><span class="sxs-lookup"><span data-stu-id="2e79d-132">But now there is a problem, because your application doesn't create the controller directly.</span></span> <span data-ttu-id="2e79d-133">API da Web cria o controlador quando ele encaminha a solicitação e a API da Web não sabe nada sobre `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="2e79d-133">Web API creates the controller when it routes the request, and Web API doesn't know anything about `IProductRepository`.</span></span> <span data-ttu-id="2e79d-134">Isso é aqui que entra o resolvedor de dependência de API da Web.</span><span class="sxs-lookup"><span data-stu-id="2e79d-134">This is where the Web API dependency resolver comes in.</span></span>

## <a name="the-web-api-dependency-resolver"></a><span data-ttu-id="2e79d-135">O resolvedor de dependência de API da Web</span><span class="sxs-lookup"><span data-stu-id="2e79d-135">The Web API Dependency Resolver</span></span>

<span data-ttu-id="2e79d-136">API da Web define o **IDependencyResolver** interface para resolver as dependências.</span><span class="sxs-lookup"><span data-stu-id="2e79d-136">Web API defines the **IDependencyResolver** interface for resolving dependencies.</span></span> <span data-ttu-id="2e79d-137">Aqui está a definição da interface:</span><span class="sxs-lookup"><span data-stu-id="2e79d-137">Here is the definition of the interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

<span data-ttu-id="2e79d-138">O **IDependencyScope** interface tem dois métodos:</span><span class="sxs-lookup"><span data-stu-id="2e79d-138">The **IDependencyScope** interface has two methods:</span></span>

- <span data-ttu-id="2e79d-139">**GetService** cria uma instância de um tipo.</span><span class="sxs-lookup"><span data-stu-id="2e79d-139">**GetService** creates one instance of a type.</span></span>
- <span data-ttu-id="2e79d-140">**GetServices** cria uma coleção de objetos de um tipo especificado.</span><span class="sxs-lookup"><span data-stu-id="2e79d-140">**GetServices** creates a collection of objects of a specified type.</span></span>

<span data-ttu-id="2e79d-141">O **IDependencyResolver** método herda **IDependencyScope** e adiciona o **BeginScope** método.</span><span class="sxs-lookup"><span data-stu-id="2e79d-141">The **IDependencyResolver** method inherits **IDependencyScope** and adds the **BeginScope** method.</span></span> <span data-ttu-id="2e79d-142">Vou falar sobre escopos posteriormente neste tutorial.</span><span class="sxs-lookup"><span data-stu-id="2e79d-142">I'll talk about scopes later in this tutorial.</span></span>

<span data-ttu-id="2e79d-143">Quando a API da Web cria uma instância de controlador, ele primeiro chama **IDependencyResolver.GetService**, passando o tipo de controlador.</span><span class="sxs-lookup"><span data-stu-id="2e79d-143">When Web API creates a controller instance, it first calls **IDependencyResolver.GetService**, passing in the controller type.</span></span> <span data-ttu-id="2e79d-144">Você pode usar esse gancho de extensibilidade para criar o controlador, resolver quaisquer dependências.</span><span class="sxs-lookup"><span data-stu-id="2e79d-144">You can use this extensibility hook to create the controller, resolving any dependencies.</span></span> <span data-ttu-id="2e79d-145">Se **GetService** retorna null, a API da Web procura um construtor sem parâmetros na classe do controlador.</span><span class="sxs-lookup"><span data-stu-id="2e79d-145">If **GetService** returns null, Web API looks for a parameterless constructor on the controller class.</span></span>

## <a name="dependency-resolution-with-the-unity-container"></a><span data-ttu-id="2e79d-146">Resolução de dependência com o contêiner do Unity</span><span class="sxs-lookup"><span data-stu-id="2e79d-146">Dependency Resolution with the Unity Container</span></span>

<span data-ttu-id="2e79d-147">Embora você poderia escrever uma completa **IDependencyResolver** implementação do zero, a interface é realmente criada para agir como ponte entre a API da Web e os contêineres de IoC existentes.</span><span class="sxs-lookup"><span data-stu-id="2e79d-147">Although you could write a complete **IDependencyResolver** implementation from scratch, the interface is really designed to act as bridge between Web API and existing IoC containers.</span></span>

<span data-ttu-id="2e79d-148">Um contêiner IoC é um componente de software que é responsável por gerenciar as dependências.</span><span class="sxs-lookup"><span data-stu-id="2e79d-148">An IoC container is a software component that is responsible for managing dependencies.</span></span> <span data-ttu-id="2e79d-149">Você registrar tipos com o contêiner e, em seguida, usa o contêiner para criar objetos.</span><span class="sxs-lookup"><span data-stu-id="2e79d-149">You register types with the container, and then use the container to create objects.</span></span> <span data-ttu-id="2e79d-150">O contêiner automaticamente descobre as relações de dependência.</span><span class="sxs-lookup"><span data-stu-id="2e79d-150">The container automatically figures out the dependency relations.</span></span> <span data-ttu-id="2e79d-151">Vários contêineres IoC também permitem que você controle como escopo e tempo de vida do objeto.</span><span class="sxs-lookup"><span data-stu-id="2e79d-151">Many IoC containers also allow you to control things like object lifetime and scope.</span></span>

> [!NOTE]
> <span data-ttu-id="2e79d-152">"IoC" significa "inversão de controle", que é um padrão geral em que uma estrutura chama o código do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="2e79d-152">"IoC" stands for "inversion of control", which is a general pattern where a framework calls into application code.</span></span> <span data-ttu-id="2e79d-153">Um contêiner IoC constrói os objetos para você, o que "inverte" o fluxo normal de controle.</span><span class="sxs-lookup"><span data-stu-id="2e79d-153">An IoC container constructs your objects for you, which "inverts" the usual flow of control.</span></span>


<span data-ttu-id="2e79d-154">Para este tutorial, vamos usar [Unity](https://msdn.microsoft.com/library/ff647202.aspx) da Microsoft Patterns &amp; práticas recomendadas.</span><span class="sxs-lookup"><span data-stu-id="2e79d-154">For this tutorial, we'll use [Unity](https://msdn.microsoft.com/library/ff647202.aspx) from Microsoft Patterns &amp; Practices.</span></span> <span data-ttu-id="2e79d-155">(Outras bibliotecas populares incluem [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Ninject](http://www.ninject.org/), e [StructureMap ](http://docs.structuremap.net/).) Você pode usar o NuGet Package Manager para instalar o Unity.</span><span class="sxs-lookup"><span data-stu-id="2e79d-155">(Other popular libraries include [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Ninject](http://www.ninject.org/), and [StructureMap](http://docs.structuremap.net/).) You can use NuGet Package Manager to install Unity.</span></span> <span data-ttu-id="2e79d-156">Do **ferramentas** menu do Visual Studio, selecione **Gerenciador de biblioteca de pacote**, em seguida, selecione **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="2e79d-156">From the **Tools** menu in Visual Studio, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="2e79d-157">Na janela do Console do Gerenciador de pacotes, digite o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="2e79d-157">In the Package Manager Console window, type the following command:</span></span>

[!code-console[Main](dependency-injection/samples/sample7.cmd)]

<span data-ttu-id="2e79d-158">Aqui está uma implementação de **IDependencyResolver** que encapsula um contêiner do Unity.</span><span class="sxs-lookup"><span data-stu-id="2e79d-158">Here is an implementation of **IDependencyResolver** that wraps a Unity container.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

> [!NOTE]
> <span data-ttu-id="2e79d-159">Se o **GetService** método não é possível resolver um tipo, ele deverá retornar **nulo**.</span><span class="sxs-lookup"><span data-stu-id="2e79d-159">If the **GetService** method cannot resolve a type, it should return **null**.</span></span> <span data-ttu-id="2e79d-160">Se o **GetServices** método não é possível resolver um tipo, ele deverá retornar um objeto de coleção vazia.</span><span class="sxs-lookup"><span data-stu-id="2e79d-160">If the **GetServices** method cannot resolve a type, it should return an empty collection object.</span></span> <span data-ttu-id="2e79d-161">Não gera exceções para tipos desconhecidos.</span><span class="sxs-lookup"><span data-stu-id="2e79d-161">Don't throw exceptions for unknown types.</span></span>


## <a name="configuring-the-dependency-resolver"></a><span data-ttu-id="2e79d-162">Configurando o resolvedor de dependência</span><span class="sxs-lookup"><span data-stu-id="2e79d-162">Configuring the Dependency Resolver</span></span>

<span data-ttu-id="2e79d-163">Definir o resolvedor de dependência no **DependencyResolver** propriedade global **HttpConfiguration** objeto.</span><span class="sxs-lookup"><span data-stu-id="2e79d-163">Set the dependency resolver on the **DependencyResolver** property of the global **HttpConfiguration** object.</span></span>

<span data-ttu-id="2e79d-164">O código a seguir registra o `IProductRepository` interface com Unity e, em seguida, cria um `UnityResolver`.</span><span class="sxs-lookup"><span data-stu-id="2e79d-164">The following code registers the `IProductRepository` interface with Unity and then creates a `UnityResolver`.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

## <a name="dependency-scope-and-controller-lifetime"></a><span data-ttu-id="2e79d-165">Escopo de dependência e tempo de vida do controlador</span><span class="sxs-lookup"><span data-stu-id="2e79d-165">Dependency Scope and Controller Lifetime</span></span>

<span data-ttu-id="2e79d-166">Os controladores são criados por solicitação.</span><span class="sxs-lookup"><span data-stu-id="2e79d-166">Controllers are created per request.</span></span> <span data-ttu-id="2e79d-167">Para gerenciar o tempo de vida do objeto, **IDependencyResolver** usa o conceito de um *escopo*.</span><span class="sxs-lookup"><span data-stu-id="2e79d-167">To manage object lifetimes, **IDependencyResolver** uses the concept of a *scope*.</span></span>

<span data-ttu-id="2e79d-168">O resolvedor de dependência anexado para o **HttpConfiguration** objeto tem escopo global.</span><span class="sxs-lookup"><span data-stu-id="2e79d-168">The dependency resolver attached to the **HttpConfiguration** object has global scope.</span></span> <span data-ttu-id="2e79d-169">Quando a API da Web cria um controlador, ele chama **BeginScope**.</span><span class="sxs-lookup"><span data-stu-id="2e79d-169">When Web API creates a controller, it calls **BeginScope**.</span></span> <span data-ttu-id="2e79d-170">Este método retorna um **IDependencyScope** que representa um escopo filho.</span><span class="sxs-lookup"><span data-stu-id="2e79d-170">This method returns an **IDependencyScope** that represents a child scope.</span></span>

<span data-ttu-id="2e79d-171">Em seguida, chama uma API da Web **GetService** no escopo filho para criar o controlador.</span><span class="sxs-lookup"><span data-stu-id="2e79d-171">Web API then calls **GetService** on the child scope to create the controller.</span></span> <span data-ttu-id="2e79d-172">Quando a solicitação for concluída, a API da Web chama **Dispose** no escopo filho.</span><span class="sxs-lookup"><span data-stu-id="2e79d-172">When request is complete, Web API calls **Dispose** on the child scope.</span></span> <span data-ttu-id="2e79d-173">Use o **Dispose** método descarte as dependências do controlador.</span><span class="sxs-lookup"><span data-stu-id="2e79d-173">Use the **Dispose** method to dispose of the controller's dependencies.</span></span>

<span data-ttu-id="2e79d-174">Como você implementa **BeginScope** depende do contêiner IoC.</span><span class="sxs-lookup"><span data-stu-id="2e79d-174">How you implement **BeginScope** depends on the IoC container.</span></span> <span data-ttu-id="2e79d-175">Para Unity escopo corresponde a um contêiner filho:</span><span class="sxs-lookup"><span data-stu-id="2e79d-175">For Unity, scope corresponds to a child container:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample10.cs)]

<span data-ttu-id="2e79d-176">Maioria dos contêineres IoC têm equivalentes semelhantes.</span><span class="sxs-lookup"><span data-stu-id="2e79d-176">Most IoC containers have similar equivalents.</span></span>

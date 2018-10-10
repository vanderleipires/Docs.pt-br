---
uid: web-api/overview/advanced/dependency-injection
title: Injeção de dependência no ASP.NET Web API 2 | Microsoft Docs
author: MikeWasson
description: Este tutorial mostra como injetar dependências em seu controlador de API Web do ASP.NET. Versões de software usadas no tutorial Web API 2 Unity Application Block...
ms.author: riande
ms.date: 01/20/2014
ms.assetid: e3d3e7ba-87f0-4032-bdd3-31f3c1aa9d9c
msc.legacyurl: /web-api/overview/advanced/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: c58b06af0044144cf28cc36c16a41672aa1f6eb3
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/10/2018
ms.locfileid: "48911259"
---
<a name="dependency-injection-in-aspnet-web-api-2"></a><span data-ttu-id="feaef-104">Injeção de dependência no ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="feaef-104">Dependency Injection in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="feaef-105">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="feaef-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="feaef-106">Baixe o projeto concluído</span><span class="sxs-lookup"><span data-stu-id="feaef-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-468ee148)

> <span data-ttu-id="feaef-107">Este tutorial mostra como injetar dependências em seu controlador de API Web do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="feaef-107">This tutorial shows how to inject dependencies into your ASP.NET Web API controller.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="feaef-108">Versões de software usadas no tutorial</span><span class="sxs-lookup"><span data-stu-id="feaef-108">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="feaef-109">API Web 2</span><span class="sxs-lookup"><span data-stu-id="feaef-109">Web API 2</span></span>
> - [<span data-ttu-id="feaef-110">Unity Application Block</span><span class="sxs-lookup"><span data-stu-id="feaef-110">Unity Application Block</span></span>](https://www.nuget.org/packages/Unity/)
> - <span data-ttu-id="feaef-111">Entity Framework 6 (versão 5 também funciona)</span><span class="sxs-lookup"><span data-stu-id="feaef-111">Entity Framework 6 (version 5 also works)</span></span>


## <a name="what-is-dependency-injection"></a><span data-ttu-id="feaef-112">O que é a injeção de dependência?</span><span class="sxs-lookup"><span data-stu-id="feaef-112">What is Dependency Injection?</span></span>

<span data-ttu-id="feaef-113">Uma *dependência* é qualquer objeto exigido por outro objeto.</span><span class="sxs-lookup"><span data-stu-id="feaef-113">A *dependency* is any object that another object requires.</span></span> <span data-ttu-id="feaef-114">Por exemplo, é comum para definir um [repositório](http://martinfowler.com/eaaCatalog/repository.html) que manipula o acesso a dados.</span><span class="sxs-lookup"><span data-stu-id="feaef-114">For example, it's common to define a [repository](http://martinfowler.com/eaaCatalog/repository.html) that handles data access.</span></span> <span data-ttu-id="feaef-115">Vamos ilustrar um exemplo.</span><span class="sxs-lookup"><span data-stu-id="feaef-115">Let's illustrate with an example.</span></span> <span data-ttu-id="feaef-116">Primeiro, vamos definir um modelo de domínio:</span><span class="sxs-lookup"><span data-stu-id="feaef-116">First, we'll define a domain model:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

<span data-ttu-id="feaef-117">Aqui está uma classe de repositório simples que armazena itens em um banco de dados, usando o Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="feaef-117">Here is a simple repository class that stores items in a database, using Entity Framework.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

<span data-ttu-id="feaef-118">Agora vamos definir um controlador de API da Web que dá suporte a solicitações GET para `Product` entidades.</span><span class="sxs-lookup"><span data-stu-id="feaef-118">Now let's define a Web API controller that supports GET requests for `Product` entities.</span></span> <span data-ttu-id="feaef-119">(Estou deixando de fora POST e outros métodos para manter a simplicidade.) Aqui está uma primeira tentativa:</span><span class="sxs-lookup"><span data-stu-id="feaef-119">(I'm leaving out POST and other methods for simplicity.) Here is a first attempt:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

<span data-ttu-id="feaef-120">Observe que a classe de controlador depende `ProductRepository`, e podemos está permitindo que o controlador de criar o `ProductRepository` instância.</span><span class="sxs-lookup"><span data-stu-id="feaef-120">Notice that the controller class depends on `ProductRepository`, and we are letting the controller create the `ProductRepository` instance.</span></span> <span data-ttu-id="feaef-121">No entanto, é uma boa ideia para codificar a dependência dessa forma, por vários motivos.</span><span class="sxs-lookup"><span data-stu-id="feaef-121">However, it's a bad idea to hard code the dependency in this way, for several reasons.</span></span>

- <span data-ttu-id="feaef-122">Se você quiser substituir `ProductRepository` com uma implementação diferente, você também precisará modificar a classe do controlador.</span><span class="sxs-lookup"><span data-stu-id="feaef-122">If you want to replace `ProductRepository` with a different implementation, you also need to modify the controller class.</span></span>
- <span data-ttu-id="feaef-123">Se o `ProductRepository` tem dependências, você deve configurá-los dentro do controlador.</span><span class="sxs-lookup"><span data-stu-id="feaef-123">If the `ProductRepository` has dependencies, you must configure these inside the controller.</span></span> <span data-ttu-id="feaef-124">Para um projeto grande com vários controladores, seu código de configuração se torna espalhado em seu projeto.</span><span class="sxs-lookup"><span data-stu-id="feaef-124">For a large project with multiple controllers, your configuration code becomes scattered across your project.</span></span>
- <span data-ttu-id="feaef-125">É difícil para o teste de unidade, porque o controlador é embutido em código para consultar o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="feaef-125">It is hard to unit test, because the controller is hard-coded to query the database.</span></span> <span data-ttu-id="feaef-126">Para um teste de unidade, você deve usar um repositório de simulação ou stub, que não é possível com o design de currect.</span><span class="sxs-lookup"><span data-stu-id="feaef-126">For a unit test, you should use a mock or stub repository, which is not possible with the currect design.</span></span>

<span data-ttu-id="feaef-127">Podemos pode resolver esses problemas por *injetando* o repositório no controlador.</span><span class="sxs-lookup"><span data-stu-id="feaef-127">We can address these problems by *injecting* the repository into the controller.</span></span> <span data-ttu-id="feaef-128">Primeiro, refatorar o `ProductRepository` classe em uma interface:</span><span class="sxs-lookup"><span data-stu-id="feaef-128">First, refactor the `ProductRepository` class into an interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

<span data-ttu-id="feaef-129">Em seguida, forneça o `IProductRepository` como um parâmetro de construtor:</span><span class="sxs-lookup"><span data-stu-id="feaef-129">Then provide the `IProductRepository` as a constructor parameter:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

<span data-ttu-id="feaef-130">Este exemplo usa [injeção de construtor](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span><span class="sxs-lookup"><span data-stu-id="feaef-130">This example uses [constructor injection](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span></span> <span data-ttu-id="feaef-131">Você também pode usar *injeção de setter*, em que você definir a dependência por meio de um método de setter ou propriedade.</span><span class="sxs-lookup"><span data-stu-id="feaef-131">You can also use *setter injection*, where you set the dependency through a setter method or property.</span></span>

<span data-ttu-id="feaef-132">Mas, agora há um problema, pois seu aplicativo não cria o controlador diretamente.</span><span class="sxs-lookup"><span data-stu-id="feaef-132">But now there is a problem, because your application doesn't create the controller directly.</span></span> <span data-ttu-id="feaef-133">API da Web cria o controlador quando ele encaminha a solicitação e a API da Web não sabe nada sobre `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="feaef-133">Web API creates the controller when it routes the request, and Web API doesn't know anything about `IProductRepository`.</span></span> <span data-ttu-id="feaef-134">Isso é onde entra o resolvedor de dependência de API da Web.</span><span class="sxs-lookup"><span data-stu-id="feaef-134">This is where the Web API dependency resolver comes in.</span></span>

## <a name="the-web-api-dependency-resolver"></a><span data-ttu-id="feaef-135">O resolvedor de dependência de API da Web</span><span class="sxs-lookup"><span data-stu-id="feaef-135">The Web API Dependency Resolver</span></span>

<span data-ttu-id="feaef-136">API Web define o **IDependencyResolver** interface para resolver as dependências.</span><span class="sxs-lookup"><span data-stu-id="feaef-136">Web API defines the **IDependencyResolver** interface for resolving dependencies.</span></span> <span data-ttu-id="feaef-137">Aqui está a definição da interface:</span><span class="sxs-lookup"><span data-stu-id="feaef-137">Here is the definition of the interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

<span data-ttu-id="feaef-138">O **IDependencyScope** interface tem dois métodos:</span><span class="sxs-lookup"><span data-stu-id="feaef-138">The **IDependencyScope** interface has two methods:</span></span>

- <span data-ttu-id="feaef-139">**GetService** cria uma instância de um tipo.</span><span class="sxs-lookup"><span data-stu-id="feaef-139">**GetService** creates one instance of a type.</span></span>
- <span data-ttu-id="feaef-140">**GetServices** cria uma coleção de objetos de um tipo especificado.</span><span class="sxs-lookup"><span data-stu-id="feaef-140">**GetServices** creates a collection of objects of a specified type.</span></span>

<span data-ttu-id="feaef-141">O **IDependencyResolver** método herda **IDependencyScope** e adiciona os **BeginScope** método.</span><span class="sxs-lookup"><span data-stu-id="feaef-141">The **IDependencyResolver** method inherits **IDependencyScope** and adds the **BeginScope** method.</span></span> <span data-ttu-id="feaef-142">Vou falar sobre escopos posteriormente no tutorial.</span><span class="sxs-lookup"><span data-stu-id="feaef-142">I'll talk about scopes later in this tutorial.</span></span>

<span data-ttu-id="feaef-143">Quando a API da Web cria uma instância de controlador, ela primeiro chama **IDependencyResolver.GetService**, passando o tipo de controlador.</span><span class="sxs-lookup"><span data-stu-id="feaef-143">When Web API creates a controller instance, it first calls **IDependencyResolver.GetService**, passing in the controller type.</span></span> <span data-ttu-id="feaef-144">Você pode usar esse gancho de extensibilidade para criar o controlador, a resolução de todas as dependências.</span><span class="sxs-lookup"><span data-stu-id="feaef-144">You can use this extensibility hook to create the controller, resolving any dependencies.</span></span> <span data-ttu-id="feaef-145">Se **GetService** retorna null, a API da Web procura um construtor sem parâmetros na classe do controlador.</span><span class="sxs-lookup"><span data-stu-id="feaef-145">If **GetService** returns null, Web API looks for a parameterless constructor on the controller class.</span></span>

## <a name="dependency-resolution-with-the-unity-container"></a><span data-ttu-id="feaef-146">Resolução de dependência com o contêiner do Unity</span><span class="sxs-lookup"><span data-stu-id="feaef-146">Dependency Resolution with the Unity Container</span></span>

<span data-ttu-id="feaef-147">Embora você poderia escrever uma completa **IDependencyResolver** implementação do zero, a interface é realmente projetada para atuar como ponte entre a API da Web e contêineres de IoC existentes.</span><span class="sxs-lookup"><span data-stu-id="feaef-147">Although you could write a complete **IDependencyResolver** implementation from scratch, the interface is really designed to act as bridge between Web API and existing IoC containers.</span></span>

<span data-ttu-id="feaef-148">Um contêiner IoC é um componente de software que é responsável por gerenciar as dependências.</span><span class="sxs-lookup"><span data-stu-id="feaef-148">An IoC container is a software component that is responsible for managing dependencies.</span></span> <span data-ttu-id="feaef-149">Você registra tipos com o contêiner e, em seguida, usa o contêiner para criar objetos.</span><span class="sxs-lookup"><span data-stu-id="feaef-149">You register types with the container, and then use the container to create objects.</span></span> <span data-ttu-id="feaef-150">O contêiner automaticamente detecta as relações de dependência.</span><span class="sxs-lookup"><span data-stu-id="feaef-150">The container automatically figures out the dependency relations.</span></span> <span data-ttu-id="feaef-151">Vários contêineres IoC também permitem controlar coisas como o tempo de vida do objeto e o escopo.</span><span class="sxs-lookup"><span data-stu-id="feaef-151">Many IoC containers also allow you to control things like object lifetime and scope.</span></span>

> [!NOTE]
> <span data-ttu-id="feaef-152">"IoC" significa "inversão de controle", que é um padrão geral em que uma estrutura chama o código do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="feaef-152">"IoC" stands for "inversion of control", which is a general pattern where a framework calls into application code.</span></span> <span data-ttu-id="feaef-153">Um contêiner IoC constrói seus objetos para você, o que "inverte" o fluxo normal de controle.</span><span class="sxs-lookup"><span data-stu-id="feaef-153">An IoC container constructs your objects for you, which "inverts" the usual flow of control.</span></span>


<span data-ttu-id="feaef-154">Para este tutorial, usaremos [Unity](https://msdn.microsoft.com/library/ff647202.aspx) da Microsoft Patterns &amp; práticas.</span><span class="sxs-lookup"><span data-stu-id="feaef-154">For this tutorial, we'll use [Unity](https://msdn.microsoft.com/library/ff647202.aspx) from Microsoft Patterns &amp; Practices.</span></span> <span data-ttu-id="feaef-155">(Outras bibliotecas populares incluem [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Ninject](http://www.ninject.org/), e [StructureMap ](http://docs.structuremap.net/).) Você pode usar o Gerenciador de pacotes NuGet para instalar o Unity.</span><span class="sxs-lookup"><span data-stu-id="feaef-155">(Other popular libraries include [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Ninject](http://www.ninject.org/), and [StructureMap](http://docs.structuremap.net/).) You can use NuGet Package Manager to install Unity.</span></span> <span data-ttu-id="feaef-156">Dos **ferramentas** menu no Visual Studio, selecione **Gerenciador de pacotes NuGet**, em seguida, selecione **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="feaef-156">From the **Tools** menu in Visual Studio, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="feaef-157">Na janela do Console do Gerenciador de pacotes, digite o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="feaef-157">In the Package Manager Console window, type the following command:</span></span>

[!code-console[Main](dependency-injection/samples/sample7.cmd)]

<span data-ttu-id="feaef-158">Aqui está uma implementação de **IDependencyResolver** que encapsula um contêiner do Unity.</span><span class="sxs-lookup"><span data-stu-id="feaef-158">Here is an implementation of **IDependencyResolver** that wraps a Unity container.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

> [!NOTE]
> <span data-ttu-id="feaef-159">Se o **GetService** método não é possível resolver um tipo, ele deverá retornar **nulo**.</span><span class="sxs-lookup"><span data-stu-id="feaef-159">If the **GetService** method cannot resolve a type, it should return **null**.</span></span> <span data-ttu-id="feaef-160">Se o **GetServices** método não é possível resolver um tipo, ele deverá retornar um objeto de coleção vazia.</span><span class="sxs-lookup"><span data-stu-id="feaef-160">If the **GetServices** method cannot resolve a type, it should return an empty collection object.</span></span> <span data-ttu-id="feaef-161">Não lançam exceções para tipos desconhecidos.</span><span class="sxs-lookup"><span data-stu-id="feaef-161">Don't throw exceptions for unknown types.</span></span>


## <a name="configuring-the-dependency-resolver"></a><span data-ttu-id="feaef-162">Configurando o resolvedor de dependência</span><span class="sxs-lookup"><span data-stu-id="feaef-162">Configuring the Dependency Resolver</span></span>

<span data-ttu-id="feaef-163">Definir o resolvedor de dependência na **DependencyResolver** propriedade global **HttpConfiguration** objeto.</span><span class="sxs-lookup"><span data-stu-id="feaef-163">Set the dependency resolver on the **DependencyResolver** property of the global **HttpConfiguration** object.</span></span>

<span data-ttu-id="feaef-164">O código a seguir registra o `IProductRepository` da interface com o Unity e, em seguida, cria um `UnityResolver`.</span><span class="sxs-lookup"><span data-stu-id="feaef-164">The following code registers the `IProductRepository` interface with Unity and then creates a `UnityResolver`.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

## <a name="dependency-scope-and-controller-lifetime"></a><span data-ttu-id="feaef-165">Escopo de dependência e tempo de vida do controlador</span><span class="sxs-lookup"><span data-stu-id="feaef-165">Dependency Scope and Controller Lifetime</span></span>

<span data-ttu-id="feaef-166">Controladores são criados por solicitação.</span><span class="sxs-lookup"><span data-stu-id="feaef-166">Controllers are created per request.</span></span> <span data-ttu-id="feaef-167">Para gerenciar o tempo de vida do objeto, **IDependencyResolver** usa o conceito de uma *escopo*.</span><span class="sxs-lookup"><span data-stu-id="feaef-167">To manage object lifetimes, **IDependencyResolver** uses the concept of a *scope*.</span></span>

<span data-ttu-id="feaef-168">O resolvedor de dependência anexada para o **HttpConfiguration** objeto tem escopo global.</span><span class="sxs-lookup"><span data-stu-id="feaef-168">The dependency resolver attached to the **HttpConfiguration** object has global scope.</span></span> <span data-ttu-id="feaef-169">Quando a API da Web cria um controlador, ele chama **BeginScope**.</span><span class="sxs-lookup"><span data-stu-id="feaef-169">When Web API creates a controller, it calls **BeginScope**.</span></span> <span data-ttu-id="feaef-170">Esse método retorna um **IDependencyScope** que representa um escopo filho.</span><span class="sxs-lookup"><span data-stu-id="feaef-170">This method returns an **IDependencyScope** that represents a child scope.</span></span>

<span data-ttu-id="feaef-171">Em seguida, chama uma API Web **GetService** no escopo filho para criar o controlador.</span><span class="sxs-lookup"><span data-stu-id="feaef-171">Web API then calls **GetService** on the child scope to create the controller.</span></span> <span data-ttu-id="feaef-172">Quando a solicitação for concluída, a API da Web chama **Dispose** no escopo filho.</span><span class="sxs-lookup"><span data-stu-id="feaef-172">When request is complete, Web API calls **Dispose** on the child scope.</span></span> <span data-ttu-id="feaef-173">Use o **Dispose** método descartar as dependências do controlador.</span><span class="sxs-lookup"><span data-stu-id="feaef-173">Use the **Dispose** method to dispose of the controller's dependencies.</span></span>

<span data-ttu-id="feaef-174">Como você implementar **BeginScope** depende do contêiner de IoC.</span><span class="sxs-lookup"><span data-stu-id="feaef-174">How you implement **BeginScope** depends on the IoC container.</span></span> <span data-ttu-id="feaef-175">Para o Unity, o escopo corresponde a um contêiner filho:</span><span class="sxs-lookup"><span data-stu-id="feaef-175">For Unity, scope corresponds to a child container:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample10.cs)]

<span data-ttu-id="feaef-176">A maioria dos contêineres de IoC têm equivalentes semelhantes.</span><span class="sxs-lookup"><span data-stu-id="feaef-176">Most IoC containers have similar equivalents.</span></span>

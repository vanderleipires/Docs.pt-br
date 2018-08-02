---
title: Padrão de repositório com o ASP.NET Core
author: ardalis
description: Saiba como implementar o padrão de design de aplicativo do repositório em um aplicativo ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 07/02/2018
uid: fundamentals/repository-pattern
ms.openlocfilehash: d64c3d090f62c580ebc05a6bbc2744a4508bb9bc
ms.sourcegitcommit: 927e510d68f269d8335b5a7c8592621219a90965
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39342560"
---
# <a name="repository-pattern-with-aspnet-core"></a><span data-ttu-id="ed44b-103">Padrão de repositório com o ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ed44b-103">Repository pattern with ASP.NET Core</span></span>

<span data-ttu-id="ed44b-104">Por [Steve Smith](https://ardalis.com/), [Scott Addie](https://scottaddie.com) e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="ed44b-104">By [Steve Smith](https://ardalis.com/), [Scott Addie](https://scottaddie.com), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="ed44b-105">O *padrão de repositório* é um padrão de design que isola o acesso a dados por trás de abstrações de interface.</span><span class="sxs-lookup"><span data-stu-id="ed44b-105">The *repository pattern* is a design pattern that isolates data access behind interface abstractions.</span></span> <span data-ttu-id="ed44b-106">A conexão com o banco de dados e a manipulação de objetos de armazenamento de dados é realizada por meio de métodos fornecidos pela implementação da interface.</span><span class="sxs-lookup"><span data-stu-id="ed44b-106">Connecting to the database and manipulating data storage objects is performed through methods provided by the interface's implementation.</span></span> <span data-ttu-id="ed44b-107">Consequentemente, não é necessário chamar um código para lidar com questões de banco de dados, como conexões, comandos e leitores.</span><span class="sxs-lookup"><span data-stu-id="ed44b-107">Consequently, there's no need for calling code to deal with database concerns, such as connections, commands, and readers.</span></span>

<span data-ttu-id="ed44b-108">A implementação do padrão de repositório com o ASP.NET Core confere estes benefícios:</span><span class="sxs-lookup"><span data-stu-id="ed44b-108">Implementation of the repository pattern with ASP.NET Core has the following benefits:</span></span>

* <span data-ttu-id="ed44b-109">A organização do aplicativo é menos complexa, sem interdependência direta entre as camadas de acesso de dados e de negócios.</span><span class="sxs-lookup"><span data-stu-id="ed44b-109">Organization of the app is less complex with no direct interdependency between the business and data access layers.</span></span>
* <span data-ttu-id="ed44b-110">É mais fácil de reutilizar o código de acesso de banco de dados, pois ele é gerenciado centralmente por um ou mais repositórios.</span><span class="sxs-lookup"><span data-stu-id="ed44b-110">It's easier to reuse database access code because the code is centrally managed by one or more repositories.</span></span>
* <span data-ttu-id="ed44b-111">É possível testar a unidade do domínio de negócios de forma independente na camada de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="ed44b-111">The business domain can be independently unit tested from the database layer.</span></span>

<span data-ttu-id="ed44b-112">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/repository-pattern/samples) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ed44b-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/repository-pattern/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="ed44b-113">O [exemplo de aplicativo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/repository-pattern/samples) usa o padrão de repositório para inicializar e exibir uma lista de nomes de caracteres de filme.</span><span class="sxs-lookup"><span data-stu-id="ed44b-113">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/repository-pattern/samples) uses the repository pattern to initialize and display a list of movie character names.</span></span> <span data-ttu-id="ed44b-114">O aplicativo usa [Entity Framework Core](/ef/core/) e uma classe `ApplicationDbContext` para persistência de dados, mas a infraestrutura do banco de dados não fica aparente onde os dados são acessados.</span><span class="sxs-lookup"><span data-stu-id="ed44b-114">The app uses [Entity Framework Core](/ef/core/) and an `ApplicationDbContext` class for its data persistence, but the database infrastructure isn't apparent where the data is accessed.</span></span> <span data-ttu-id="ed44b-115">Os objetos de banco de dados e de acesso aos dados são abstraídos por trás de um [repositório](https://martinfowler.com/eaaCatalog/repository.html).</span><span class="sxs-lookup"><span data-stu-id="ed44b-115">Data access and database objects are abstracted behind a [repository](https://martinfowler.com/eaaCatalog/repository.html).</span></span>

## <a name="repository-interface"></a><span data-ttu-id="ed44b-116">Interface de repositório</span><span class="sxs-lookup"><span data-stu-id="ed44b-116">Repository interface</span></span>

<span data-ttu-id="ed44b-117">Uma interface de repositório define as propriedades e métodos para implementação.</span><span class="sxs-lookup"><span data-stu-id="ed44b-117">A repository interface defines the properties and methods for implementation.</span></span> <span data-ttu-id="ed44b-118">No aplicativo de exemplo, a interface do repositório para dados de caracteres de filme é `ICharacterRepository`.</span><span class="sxs-lookup"><span data-stu-id="ed44b-118">In the sample app, the repository interface for movie character data is `ICharacterRepository`.</span></span> <span data-ttu-id="ed44b-119">`ICharacterRepository` define os métodos `ListAll` e `Add` necessários para trabalhar com instâncias de `Character` no aplicativo:</span><span class="sxs-lookup"><span data-stu-id="ed44b-119">`ICharacterRepository` defines the `ListAll` and `Add` methods required to work with `Character` instances in the app:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Interfaces/ICharacterRepository.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Interfaces/ICharacterRepository.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="ed44b-120">`Character` é definido como:</span><span class="sxs-lookup"><span data-stu-id="ed44b-120">`Character` is defined as:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Models/Character.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Models/Character.cs?name=snippet1)]

::: moniker-end

## <a name="repository-concrete-type"></a><span data-ttu-id="ed44b-121">Tipo concreto de repositório</span><span class="sxs-lookup"><span data-stu-id="ed44b-121">Repository concrete type</span></span>

<span data-ttu-id="ed44b-122">A interface é implementada por um tipo concreto.</span><span class="sxs-lookup"><span data-stu-id="ed44b-122">The interface is implemented by a concrete type.</span></span> <span data-ttu-id="ed44b-123">No aplicativo de exemplo, `CharacterRepository` gerencia o contexto de banco de dados e implementa os métodos `ListAll` e `Add`:</span><span class="sxs-lookup"><span data-stu-id="ed44b-123">In the sample app, `CharacterRepository` manages the database context and implements the `ListAll` and `Add` methods:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Models/CharacterRepository.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Models/CharacterRepository.cs?name=snippet1)]

::: moniker-end

## <a name="register-the-repository-service"></a><span data-ttu-id="ed44b-124">Registrar o serviço de repositório</span><span class="sxs-lookup"><span data-stu-id="ed44b-124">Register the repository service</span></span>

<span data-ttu-id="ed44b-125">Os contextos do repositório e do banco de dados são registrados com o contêiner de serviço em `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="ed44b-125">The repository and database context are registered with the service container in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="ed44b-126">No exemplo de aplicativo, `ApplicationDbContext` é configurado com a chamada para o método de extensão [AddDbContext](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext).</span><span class="sxs-lookup"><span data-stu-id="ed44b-126">In the sample app, `ApplicationDbContext` is configured with the call to the extension method [AddDbContext](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext).</span></span> <span data-ttu-id="ed44b-127">`ICharacterRepository` é registrado como um serviço com escopo:</span><span class="sxs-lookup"><span data-stu-id="ed44b-127">`ICharacterRepository` is registered as a scoped service:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Startup.cs?name=snippet1&highlight=4-6,18)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Startup.cs?name=snippet1&highlight=4-6,12)]

::: moniker-end

## <a name="inject-an-instance-of-the-repository"></a><span data-ttu-id="ed44b-128">Injetar uma instância do repositório</span><span class="sxs-lookup"><span data-stu-id="ed44b-128">Inject an instance of the repository</span></span>

<span data-ttu-id="ed44b-129">Em uma classe na qual o acesso ao banco de dados é exigido, uma instância do repositório é solicitada por meio do construtor e atribuída a um campo particular para uso por métodos da classe.</span><span class="sxs-lookup"><span data-stu-id="ed44b-129">In a class where database access is required, an instance of the repository is requested via the constructor and assigned to a private field for use by class methods.</span></span> <span data-ttu-id="ed44b-130">No exemplo de aplicativo, `ICharacterRepository` é usado para:</span><span class="sxs-lookup"><span data-stu-id="ed44b-130">In the sample app, `ICharacterRepository` is used to:</span></span>

* <span data-ttu-id="ed44b-131">Preencha o banco de dados se não houver caracteres.</span><span class="sxs-lookup"><span data-stu-id="ed44b-131">Populate the database if no characters exist.</span></span>
* <span data-ttu-id="ed44b-132">Obtenha uma lista dos caracteres para exibição.</span><span class="sxs-lookup"><span data-stu-id="ed44b-132">Obtain a list of the characters for display.</span></span>

<span data-ttu-id="ed44b-133">Observe como o código de chamada interage apenas com a implementação da interface, `CharacterRepository`.</span><span class="sxs-lookup"><span data-stu-id="ed44b-133">Notice how the calling code only interacts with the interface's implementation, `CharacterRepository`.</span></span> <span data-ttu-id="ed44b-134">O código chamador não usa o `ApplicationDbContext` diretamente:</span><span class="sxs-lookup"><span data-stu-id="ed44b-134">Calling code doesn't use the `ApplicationDbContext` directly:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Pages/Index.cshtml.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Controllers/HomeController.cs?name=snippet1)]

::: moniker-end

## <a name="generic-repository-interface"></a><span data-ttu-id="ed44b-135">Interface genérica do repositório</span><span class="sxs-lookup"><span data-stu-id="ed44b-135">Generic repository interface</span></span>

<span data-ttu-id="ed44b-136">Este tópico e seu exemplo de aplicativo demonstram a implementação mais simples do padrão de repositório, em que um repositório é criado para cada objeto de negócios.</span><span class="sxs-lookup"><span data-stu-id="ed44b-136">This topic and its sample app demonstrate the simplest implementation of the repository pattern, where one repository is created for each business object.</span></span> <span data-ttu-id="ed44b-137">Se o aplicativo ultrapassar alguns objetos, uma *interface genérica de repositório* pode reduzir a quantidade de código necessária para implementar o padrão de repositório.</span><span class="sxs-lookup"><span data-stu-id="ed44b-137">If the app grows beyond a few objects, a *generic repository interface* may reduce the amount of code required to implement the repository pattern.</span></span> <span data-ttu-id="ed44b-138">Para saber mais, confira [DevIQ: padrão de repositório: interface genérica de repositório](http://deviq.com/repository-pattern/).</span><span class="sxs-lookup"><span data-stu-id="ed44b-138">For more information, see [DevIQ: Repository Pattern: Generic Repository Interface](http://deviq.com/repository-pattern/).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ed44b-139">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="ed44b-139">Additional resources</span></span>

* [<span data-ttu-id="ed44b-140">DevIQ: padrão de repositório</span><span class="sxs-lookup"><span data-stu-id="ed44b-140">DevIQ: Repository Pattern</span></span>](https://deviq.com/repository-pattern/)
* [<span data-ttu-id="ed44b-141">Injeção de dependência</span><span class="sxs-lookup"><span data-stu-id="ed44b-141">Dependency injection</span></span>](xref:fundamentals/dependency-injection)
* [<span data-ttu-id="ed44b-142">Injeção de dependência em exibições</span><span class="sxs-lookup"><span data-stu-id="ed44b-142">Dependency injection into views</span></span>](xref:mvc/views/dependency-injection)
* [<span data-ttu-id="ed44b-143">Injeção de dependência em controladores</span><span class="sxs-lookup"><span data-stu-id="ed44b-143">Dependency injection into controllers</span></span>](xref:mvc/controllers/dependency-injection)
* [<span data-ttu-id="ed44b-144">Injeção de dependência em manipuladores de requisitos</span><span class="sxs-lookup"><span data-stu-id="ed44b-144">Dependency injection in requirement handlers</span></span>](xref:security/authorization/dependencyinjection)
* [<span data-ttu-id="ed44b-145">Inversão de Contêineres de Controle e o padrão de Injeção de Dependência</span><span class="sxs-lookup"><span data-stu-id="ed44b-145">Inversion of Control Containers and the Dependency Injection Pattern</span></span>](https://www.martinfowler.com/articles/injection.html)

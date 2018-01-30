---
title: "Introdução ao ASP.NET Core e o Entity Framework 6"
author: tdykstra
description: Este artigo mostra como usar o Entity Framework 6 em um aplicativo do ASP.NET Core.
manager: wpickett
ms.author: tdykstra
ms.date: 02/24/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: data/entity-framework-6
ms.openlocfilehash: 7407fe8a976978d7d5077d5e5ac6cc264565621d
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/30/2018
---
# <a name="getting-started-with-aspnet-core-and-entity-framework-6"></a><span data-ttu-id="0143e-103">Introdução ao ASP.NET Core e o Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="0143e-103">Getting started with ASP.NET Core and Entity Framework 6</span></span>

<span data-ttu-id="0143e-104">Por [Paweł Grudzień](https://github.com/pgrudzien12), [Damien Pontifex](https://github.com/DamienPontifex), e [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="0143e-104">By [Paweł Grudzień](https://github.com/pgrudzien12), [Damien Pontifex](https://github.com/DamienPontifex), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="0143e-105">Este artigo mostra como usar o Entity Framework 6 em um aplicativo do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0143e-105">This article shows how to use Entity Framework 6 in an ASP.NET Core application.</span></span>

## <a name="overview"></a><span data-ttu-id="0143e-106">Visão geral</span><span class="sxs-lookup"><span data-stu-id="0143e-106">Overview</span></span>

<span data-ttu-id="0143e-107">Para usar o Entity Framework 6, seu projeto deve compilar no .NET Framework, como Entity Framework 6 não dá suporte ao .NET Core.</span><span class="sxs-lookup"><span data-stu-id="0143e-107">To use Entity Framework 6, your project has to compile against .NET Framework, as Entity Framework 6 doesn't support .NET Core.</span></span> <span data-ttu-id="0143e-108">Se você precisar de recursos de plataforma cruzada, será necessário atualizar para [Entity Framework Core](https://docs.microsoft.com/ef/).</span><span class="sxs-lookup"><span data-stu-id="0143e-108">If you need cross-platform features you will need to upgrade to [Entity Framework Core](https://docs.microsoft.com/ef/).</span></span>

<span data-ttu-id="0143e-109">A maneira recomendada para usar o Entity Framework 6 em um aplicativo do ASP.NET Core é colocar o contexto de EF6 e classes de modelo em uma biblioteca de classes do projeto que tem como destino framework completo.</span><span class="sxs-lookup"><span data-stu-id="0143e-109">The recommended way to use Entity Framework 6 in an ASP.NET Core application is to put the EF6 context and model classes in a class library project that targets the full framework.</span></span> <span data-ttu-id="0143e-110">Adicione uma referência para a biblioteca de classes do projeto ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0143e-110">Add a reference to the class library from the ASP.NET Core project.</span></span> <span data-ttu-id="0143e-111">Consulte o exemplo [solução do Visual Studio com projetos EF6 e ASP.NET Core](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/).</span><span class="sxs-lookup"><span data-stu-id="0143e-111">See the sample [Visual Studio solution with EF6 and ASP.NET Core projects](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/).</span></span>

<span data-ttu-id="0143e-112">Não é possível colocar um contexto EF6 em um projeto do ASP.NET Core porque projetos .NET Core não dão suporte a todas as funcionalidades que EF6 comandos como *Enable-Migrations* exigem.</span><span class="sxs-lookup"><span data-stu-id="0143e-112">You can't put an EF6 context in an ASP.NET Core project because .NET Core projects don't support all of the functionality that EF6 commands such as *Enable-Migrations* require.</span></span>

<span data-ttu-id="0143e-113">Independentemente do tipo de projeto em que você localize seu contexto EF6, somente ferramentas de linha de comando EF6 trabalhar com um contexto de EF6.</span><span class="sxs-lookup"><span data-stu-id="0143e-113">Regardless of project type in which you locate your EF6 context, only EF6 command-line tools work with an EF6 context.</span></span> <span data-ttu-id="0143e-114">Por exemplo, `Scaffold-DbContext` só está disponível no Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="0143e-114">For example, `Scaffold-DbContext` is only available in Entity Framework Core.</span></span> <span data-ttu-id="0143e-115">Se você precisar a engenharia reversa de um banco de dados em um modelo de EF6, consulte [Code First para um banco de dados](https://msdn.microsoft.com/jj200620).</span><span class="sxs-lookup"><span data-stu-id="0143e-115">If you need to do reverse engineering of a database into an EF6 model, see [Code First to an Existing Database](https://msdn.microsoft.com/jj200620).</span></span>

## <a name="reference-full-framework-and-ef6-in-the-aspnet-core-project"></a><span data-ttu-id="0143e-116">Estrutura completa de referência e EF6 no projeto do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0143e-116">Reference full framework and EF6 in the ASP.NET Core project</span></span>

<span data-ttu-id="0143e-117">Seu projeto do ASP.NET Core precisa referenciar EF6 e do .NET framework.</span><span class="sxs-lookup"><span data-stu-id="0143e-117">Your ASP.NET Core project needs to reference .NET framework and EF6.</span></span> <span data-ttu-id="0143e-118">Por exemplo, o *. csproj* arquivo do projeto ASP.NET Core será semelhante ao exemplo a seguir (somente as partes relevantes do arquivo são mostradas).</span><span class="sxs-lookup"><span data-stu-id="0143e-118">For example, the *.csproj* file of your ASP.NET Core project will look similar to the following example (only relevant parts of the file are shown).</span></span>

[!code-xml[](entity-framework-6/sample/MVCCore/MVCCore.csproj?range=3-9&highlight=2)]

<span data-ttu-id="0143e-119">Ao criar um novo projeto, use o **aplicativo Web do ASP.NET Core (.NET Framework)** modelo.</span><span class="sxs-lookup"><span data-stu-id="0143e-119">When creating a new project, use the **ASP.NET Core Web Application (.NET Framework)** template.</span></span>

## <a name="handle-connection-strings"></a><span data-ttu-id="0143e-120">Cadeias de caracteres do identificador de conexão</span><span class="sxs-lookup"><span data-stu-id="0143e-120">Handle connection strings</span></span>

<span data-ttu-id="0143e-121">As ferramentas de linha de comando EF6 que você usará no projeto de biblioteca de classe EF6 requerem um construtor padrão para que possam instanciar o contexto.</span><span class="sxs-lookup"><span data-stu-id="0143e-121">The EF6 command-line tools that you'll use in the EF6 class library project require a default constructor so they can instantiate the context.</span></span> <span data-ttu-id="0143e-122">Mas, você provavelmente desejará especificar a cadeia de caracteres de conexão para usar no projeto ASP.NET Core, caso em que o construtor de contexto deve ter um parâmetro que permite que você passe a cadeia de caracteres de conexão.</span><span class="sxs-lookup"><span data-stu-id="0143e-122">But you'll probably want to specify the connection string to use in the ASP.NET Core project, in which case your context constructor must have a parameter that lets you pass in the connection string.</span></span> <span data-ttu-id="0143e-123">Aqui está um exemplo.</span><span class="sxs-lookup"><span data-stu-id="0143e-123">Here's an example.</span></span>

[!code-csharp[](entity-framework-6/sample/EF6/SchoolContext.cs?name=snippet_Constructor)]

<span data-ttu-id="0143e-124">Como seu contexto EF6 não tem um construtor sem parâmetros, seu projeto EF6 precisa fornecer uma implementação de [IDbContextFactory](https://msdn.microsoft.com/library/hh506876).</span><span class="sxs-lookup"><span data-stu-id="0143e-124">Since your EF6 context doesn't have a parameterless constructor, your EF6 project has to provide an implementation of [IDbContextFactory](https://msdn.microsoft.com/library/hh506876).</span></span> <span data-ttu-id="0143e-125">As ferramentas de linha de comando EF6 encontrará e usar essa implementação, portanto, pode criar uma instância do contexto.</span><span class="sxs-lookup"><span data-stu-id="0143e-125">The EF6 command-line tools will find and use that implementation so they can instantiate the context.</span></span> <span data-ttu-id="0143e-126">Aqui está um exemplo.</span><span class="sxs-lookup"><span data-stu-id="0143e-126">Here's an example.</span></span>

[!code-csharp[](entity-framework-6/sample/EF6/SchoolContextFactory.cs?name=snippet_IDbContextFactory)]

<span data-ttu-id="0143e-127">Nesse código de exemplo, o `IDbContextFactory` implementação passa em uma cadeia de caracteres de conexão embutidas.</span><span class="sxs-lookup"><span data-stu-id="0143e-127">In this sample code, the `IDbContextFactory` implementation passes in a hard-coded connection string.</span></span> <span data-ttu-id="0143e-128">Esta é a cadeia de conexão que irá usar as ferramentas de linha de comando.</span><span class="sxs-lookup"><span data-stu-id="0143e-128">This is the connection string that the command-line tools will use.</span></span> <span data-ttu-id="0143e-129">Você vai querer implementar uma estratégia para garantir que a biblioteca de classe usa a mesma cadeia de conexão que usa o aplicativo de chamada.</span><span class="sxs-lookup"><span data-stu-id="0143e-129">You'll want to implement a strategy to ensure that the class library uses the same connection string that the calling application uses.</span></span> <span data-ttu-id="0143e-130">Por exemplo, você pode obter o valor de uma variável de ambiente em ambos os projetos.</span><span class="sxs-lookup"><span data-stu-id="0143e-130">For example, you could get the value from an environment variable in both projects.</span></span>

## <a name="set-up-dependency-injection-in-the-aspnet-core-project"></a><span data-ttu-id="0143e-131">Configurar a injeção de dependência no projeto do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0143e-131">Set up dependency injection in the ASP.NET Core project</span></span>

<span data-ttu-id="0143e-132">O projeto de núcleo *Startup.cs* arquivo, configurar o contexto de EF6 para injeção de dependência (DI) `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="0143e-132">In the Core project's *Startup.cs* file, set up the EF6 context for dependency injection (DI) in `ConfigureServices`.</span></span> <span data-ttu-id="0143e-133">Os objetos de contexto EF devem ser delimitados para um tempo de vida de cada solicitação.</span><span class="sxs-lookup"><span data-stu-id="0143e-133">EF context objects should be scoped for a per-request lifetime.</span></span>

[!code-csharp[](entity-framework-6/sample/MVCCore/Startup.cs?name=snippet_ConfigureServices&highlight=5)]

<span data-ttu-id="0143e-134">Em seguida, você pode obter uma instância do contexto em seus controladores de usando a injeção de dependência.</span><span class="sxs-lookup"><span data-stu-id="0143e-134">You can then get an instance of the context in your controllers by using DI.</span></span> <span data-ttu-id="0143e-135">O código é semelhante ao que você escreve para um contexto de EF principais:</span><span class="sxs-lookup"><span data-stu-id="0143e-135">The code is similar to what you'd write for an EF Core context:</span></span>

[!code-csharp[](entity-framework-6/sample/MVCCore/Controllers/StudentsController.cs?name=snippet_ContextInController)]

## <a name="sample-application"></a><span data-ttu-id="0143e-136">Aplicativo de exemplo</span><span class="sxs-lookup"><span data-stu-id="0143e-136">Sample application</span></span>

<span data-ttu-id="0143e-137">Para um aplicativo de exemplo de funcionamento, consulte o [solução do Visual Studio de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/) que acompanha este artigo.</span><span class="sxs-lookup"><span data-stu-id="0143e-137">For a working sample application, see the [sample Visual Studio solution](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/) that accompanies this article.</span></span>

<span data-ttu-id="0143e-138">Este exemplo pode ser criado do zero pelas seguintes etapas no Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="0143e-138">This sample can be created from scratch by the following steps in Visual Studio:</span></span>

* <span data-ttu-id="0143e-139">Crie uma solução.</span><span class="sxs-lookup"><span data-stu-id="0143e-139">Create a solution.</span></span>

* <span data-ttu-id="0143e-140">**Adicionar novo projeto > Web > aplicativo Web do ASP.NET Core (.NET Framework)**</span><span class="sxs-lookup"><span data-stu-id="0143e-140">**Add New Project > Web > ASP.NET Core Web Application (.NET Framework)**</span></span>

* <span data-ttu-id="0143e-141">**Adicionar novo projeto > área de trabalho clássica do Windows > (.NET Framework) da biblioteca de classe**</span><span class="sxs-lookup"><span data-stu-id="0143e-141">**Add New Project > Windows Classic Desktop > Class Library (.NET Framework)**</span></span>

* <span data-ttu-id="0143e-142">Em **Package Manager Console** (PMC) para ambos os projetos, execute o comando `Install-Package Entityframework`.</span><span class="sxs-lookup"><span data-stu-id="0143e-142">In **Package Manager Console** (PMC) for both projects, run the command `Install-Package Entityframework`.</span></span>

* <span data-ttu-id="0143e-143">No projeto de biblioteca de classe, crie classes de modelo de dados e uma classe de contexto e uma implementação de `IDbContextFactory`.</span><span class="sxs-lookup"><span data-stu-id="0143e-143">In the class library project, create data model classes and a context class, and an implementation of `IDbContextFactory`.</span></span>

* <span data-ttu-id="0143e-144">Em PMC para o projeto de biblioteca de classe, execute os comandos `Enable-Migrations` e `Add-Migration Initial`.</span><span class="sxs-lookup"><span data-stu-id="0143e-144">In PMC for the class library project, run the commands `Enable-Migrations` and `Add-Migration Initial`.</span></span> <span data-ttu-id="0143e-145">Se você tiver definido o projeto do ASP.NET Core como projeto de inicialização, adicionar `-StartupProjectName EF6` a esses comandos.</span><span class="sxs-lookup"><span data-stu-id="0143e-145">If you have set the ASP.NET Core project as the startup project, add `-StartupProjectName EF6` to these commands.</span></span>

* <span data-ttu-id="0143e-146">No projeto principal, adicione uma referência ao projeto de biblioteca de classe.</span><span class="sxs-lookup"><span data-stu-id="0143e-146">In the Core project, add a project reference to the class library project.</span></span>

* <span data-ttu-id="0143e-147">No projeto principal, em *Startup.cs*, registrar o contexto para injeção de dependência.</span><span class="sxs-lookup"><span data-stu-id="0143e-147">In the Core project, in *Startup.cs*, register the context for DI.</span></span>

* <span data-ttu-id="0143e-148">No projeto principal, em *appSettings. JSON*, adicione a cadeia de caracteres de conexão.</span><span class="sxs-lookup"><span data-stu-id="0143e-148">In the Core project, in *appsettings.json*, add the connection string.</span></span>

* <span data-ttu-id="0143e-149">No projeto principal, adicione um controlador e as exibições para verificar que você pode ler e gravar dados.</span><span class="sxs-lookup"><span data-stu-id="0143e-149">In the Core project, add a controller and view(s) to verify that you can read and write data.</span></span> <span data-ttu-id="0143e-150">(Observe que a estrutura MVC do ASP.NET Core não funcionará com o contexto de EF6 referenciado da biblioteca de classe.)</span><span class="sxs-lookup"><span data-stu-id="0143e-150">(Note that ASP.NET Core MVC scaffolding won't work with the EF6 context referenced from the class library.)</span></span>

## <a name="summary"></a><span data-ttu-id="0143e-151">Resumo</span><span class="sxs-lookup"><span data-stu-id="0143e-151">Summary</span></span>

<span data-ttu-id="0143e-152">Este artigo fornece orientações básicas para usar o Entity Framework 6 em um aplicativo do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0143e-152">This article has provided basic guidance for using Entity Framework 6 in an ASP.NET Core application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0143e-153">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="0143e-153">Additional resources</span></span>

* [<span data-ttu-id="0143e-154">Entity Framework - configuração baseada em código</span><span class="sxs-lookup"><span data-stu-id="0143e-154">Entity Framework - Code-Based Configuration</span></span>](https://msdn.microsoft.com/data/jj680699.aspx)

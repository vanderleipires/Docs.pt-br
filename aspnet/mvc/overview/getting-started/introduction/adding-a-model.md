---
uid: mvc/overview/getting-started/introduction/adding-a-model
title: Adicionando um modelo | Microsoft Docs
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: 276552b5-f349-4fcf-8f40-6d042f7aa88e
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: 79f136257119a8600a65e8d7c5f6e99cb9abceae
ms.sourcegitcommit: d8aa1d314891e981460b5e5c912afb730adbb3ad
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/05/2018
---
<a name="adding-a-model"></a><span data-ttu-id="d8777-102">Adicionando um modelo</span><span class="sxs-lookup"><span data-stu-id="d8777-102">Adding a Model</span></span>
====================
<span data-ttu-id="d8777-103">Por [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="d8777-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[!INCLUDE[Tutorial Note](sample/code-location.md)]

<span data-ttu-id="d8777-104">Nesta seção, você adicionará algumas classes de gerenciamento de filmes em um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="d8777-104">In this section you'll add some classes for managing movies in a database.</span></span> <span data-ttu-id="d8777-105">Essas classes será o &quot;modelo&quot; parte do aplicativo ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="d8777-105">These classes will be the &quot;model&quot; part of the ASP.NET MVC app.</span></span>

<span data-ttu-id="d8777-106">Você usará uma tecnologia de acesso a dados do .NET Framework conhecida como o [do Entity Framework](https://docs.microsoft.com/ef/) para definir e trabalhar com essas classes de modelo.</span><span class="sxs-lookup"><span data-stu-id="d8777-106">You'll use a .NET Framework data-access technology known as the [Entity Framework](https://docs.microsoft.com/ef/) to define and work with these model classes.</span></span> <span data-ttu-id="d8777-107">O Entity Framework (também conhecido como EF) oferece suporte a um paradigma de desenvolvimento chamado *Code First*.</span><span class="sxs-lookup"><span data-stu-id="d8777-107">The Entity Framework (often referred to as EF) supports a development paradigm called *Code First*.</span></span> <span data-ttu-id="d8777-108">Código primeiro permite que você crie objetos de modelo, escrevendo classes simples.</span><span class="sxs-lookup"><span data-stu-id="d8777-108">Code First allows you to create model objects by writing simple classes.</span></span> <span data-ttu-id="d8777-109">(Eles também são conhecidas como POCO classes, de &quot;objetos CLR simples antigo.&quot;) Em seguida, você pode ter o banco de dados criado dinamicamente de suas classes, que permite que um fluxo de trabalho de desenvolvimento muito claro e rápida.</span><span class="sxs-lookup"><span data-stu-id="d8777-109">(These are also known as POCO classes, from &quot;plain-old CLR objects.&quot;) You can then have the database created on the fly from your classes, which enables a very clean and rapid development workflow.</span></span> <span data-ttu-id="d8777-110">Se for necessário para criar o banco de dados pela primeira vez, você ainda pode seguir este tutorial para aprender sobre o desenvolvimento de aplicativo MVC e EF.</span><span class="sxs-lookup"><span data-stu-id="d8777-110">If you are required to create the database first, you can still follow this tutorial to learn about MVC and EF app development.</span></span> <span data-ttu-id="d8777-111">Você pode seguir Tom Fizmakens [ASP.NET Scaffolding](xref:visual-studio/overview/2013/aspnet-scaffolding-overview) tutorial, que abrange a primeira abordagem de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="d8777-111">You can then follow Tom Fizmakens [ASP.NET Scaffolding](xref:visual-studio/overview/2013/aspnet-scaffolding-overview) tutorial, which covers the database first approach.</span></span>

## <a name="adding-model-classes"></a><span data-ttu-id="d8777-112">Adicionando Classes de modelo</span><span class="sxs-lookup"><span data-stu-id="d8777-112">Adding Model Classes</span></span>

<span data-ttu-id="d8777-113">Em **Solution Explorer**, clique com botão direito do *modelos* pasta, selecione **adicionar**e, em seguida, selecione **classe**.</span><span class="sxs-lookup"><span data-stu-id="d8777-113">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-model/_static/image1.png)

<span data-ttu-id="d8777-114">Insira o *classe* nome &quot;filme&quot;.</span><span class="sxs-lookup"><span data-stu-id="d8777-114">Enter the *class* name &quot;Movie&quot;.</span></span>

<span data-ttu-id="d8777-115">Adicione as seguintes cinco propriedades para o `Movie` classe:</span><span class="sxs-lookup"><span data-stu-id="d8777-115">Add the following five properties to the `Movie` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

<span data-ttu-id="d8777-116">Usaremos a `Movie` classe para representar filmes em um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="d8777-116">We'll use the `Movie` class to represent movies in a database.</span></span> <span data-ttu-id="d8777-117">Cada instância de um `Movie` correspondam a uma linha em uma tabela de banco de dados e cada propriedade do objeto de `Movie` classe será mapeado para uma coluna na tabela.</span><span class="sxs-lookup"><span data-stu-id="d8777-117">Each instance of a `Movie` object will correspond to a row within a database table, and each property of the `Movie` class will map to a column in the table.</span></span>

<span data-ttu-id="d8777-118">Observação: Para usar System.Data.Entity e a classe relacionada, você precisa instalar o [pacote do NuGet do Entity Framework](https://www.nuget.org/packages/EntityFramework/).</span><span class="sxs-lookup"><span data-stu-id="d8777-118">Note: In order to use System.Data.Entity, and the related class, you need to install the [Entity Framework NuGet Package](https://www.nuget.org/packages/EntityFramework/).</span></span> <span data-ttu-id="d8777-119">Siga o link para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="d8777-119">Follow the link for further instructions.</span></span>

<span data-ttu-id="d8777-120">No mesmo arquivo, adicione o seguinte `MovieDBContext` classe:</span><span class="sxs-lookup"><span data-stu-id="d8777-120">In the same file, add the following `MovieDBContext` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample2.cs?highlight=2,15-18)]

<span data-ttu-id="d8777-121">O `MovieDBContext` classe representa o contexto de banco de dados do filme do Entity Framework, que manipula a busca, armazenar e atualizar `Movie` instâncias em um banco de dados de classe.</span><span class="sxs-lookup"><span data-stu-id="d8777-121">The `MovieDBContext` class represents the Entity Framework movie database context, which handles fetching, storing, and updating `Movie` class instances in a database.</span></span> <span data-ttu-id="d8777-122">O `MovieDBContext` deriva o `DbContext` fornecido pelo Entity Framework de classe base.</span><span class="sxs-lookup"><span data-stu-id="d8777-122">The `MovieDBContext` derives from the `DbContext` base class provided by the Entity Framework.</span></span>

<span data-ttu-id="d8777-123">Para poder referenciar `DbContext` e `DbSet`, você precisa adicionar o seguinte `using` instrução na parte superior do arquivo:</span><span class="sxs-lookup"><span data-stu-id="d8777-123">In order to be able to reference `DbContext` and `DbSet`, you need to add the following `using` statement at the top of the file:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

<span data-ttu-id="d8777-124">Você pode fazer isso manualmente adicionando o uso instrução ou passe o mouse sobre as linhas curvadas vermelhas, clique em `Show potential fixes` e clique em`using System.Data.Entity;`</span><span class="sxs-lookup"><span data-stu-id="d8777-124">You can do this by manually adding the using statement, or you can hover over the red squiggly lines, click `Show potential fixes` and click `using System.Data.Entity;`</span></span>

![](adding-a-model/_static/image2.png)

<span data-ttu-id="d8777-125">Observação: Vários não utilizados `using` instruções foram removidas.</span><span class="sxs-lookup"><span data-stu-id="d8777-125">Note: Several unused `using` statements have been removed.</span></span> <span data-ttu-id="d8777-126">O Visual Studio mostrará dependências não usadas em cinza.</span><span class="sxs-lookup"><span data-stu-id="d8777-126">Visual Studio will show unused dependencies as gray.</span></span> <span data-ttu-id="d8777-127">Você pode remover dependências não usadas ao passar sobre as dependências cinzas, clique em `Show potential fixes` e clique em **remover usos não utilizados.**</span><span class="sxs-lookup"><span data-stu-id="d8777-127">You can remove unused dependencies by hovering over the gray dependencies, click `Show potential fixes` and click **Remove Unused Usings.**</span></span>

![](adding-a-model/_static/image3.png)

<span data-ttu-id="d8777-128">Por fim, adicionamos um modelo (M no MVC).</span><span class="sxs-lookup"><span data-stu-id="d8777-128">We've finally added a model (the M in MVC).</span></span> <span data-ttu-id="d8777-129">Na próxima seção, você trabalhará com a cadeia de caracteres de conexão do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="d8777-129">In the next section you'll work with the database connection string.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="d8777-130">[Anterior](adding-a-view.md)
[Próximo](creating-a-connection-string.md)</span><span class="sxs-lookup"><span data-stu-id="d8777-130">[Previous](adding-a-view.md)
[Next](creating-a-connection-string.md)</span></span>

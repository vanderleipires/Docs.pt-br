---
uid: mvc/overview/getting-started/introduction/adding-a-model
title: Adicionando um modelo | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 05/28/2015
ms.assetid: 276552b5-f349-4fcf-8f40-6d042f7aa88e
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: 3b9d84ae757eac6cc2a1ae8f7857244adff50d7c
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/04/2018
ms.locfileid: "48578270"
---
<a name="adding-a-model"></a><span data-ttu-id="aec89-102">Adicionando um modelo</span><span class="sxs-lookup"><span data-stu-id="aec89-102">Adding a Model</span></span>
====================
<span data-ttu-id="aec89-103">por [Rick Anderson]((https://twitter.com/RickAndMSFT))</span><span class="sxs-lookup"><span data-stu-id="aec89-103">by [Rick Anderson]((https://twitter.com/RickAndMSFT))</span></span>

[!INCLUDE [Tutorial Note](sample/code-location.md)]

<span data-ttu-id="aec89-104">Nesta seção, você adicionará algumas classes para o gerenciamento de filmes em um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="aec89-104">In this section you'll add some classes for managing movies in a database.</span></span> <span data-ttu-id="aec89-105">Essas classes serão a &quot;modelo&quot; faz parte do aplicativo ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="aec89-105">These classes will be the &quot;model&quot; part of the ASP.NET MVC app.</span></span>

<span data-ttu-id="aec89-106">Você usará uma tecnologia de acesso a dados do .NET Framework conhecida como o [Entity Framework](https://docs.microsoft.com/ef/) para definir e trabalhar com essas classes de modelo.</span><span class="sxs-lookup"><span data-stu-id="aec89-106">You'll use a .NET Framework data-access technology known as the [Entity Framework](https://docs.microsoft.com/ef/) to define and work with these model classes.</span></span> <span data-ttu-id="aec89-107">A Entity Framework (também conhecido como EF) oferece suporte a um paradigma de desenvolvimento chamado *Code First*.</span><span class="sxs-lookup"><span data-stu-id="aec89-107">The Entity Framework (often referred to as EF) supports a development paradigm called *Code First*.</span></span> <span data-ttu-id="aec89-108">Código primeiro permite que você crie objetos de modelo ao escrever classes simples.</span><span class="sxs-lookup"><span data-stu-id="aec89-108">Code First allows you to create model objects by writing simple classes.</span></span> <span data-ttu-id="aec89-109">(Esses também são conhecidas como classes POCO, do &quot;objetos CLR de BOM e velho.&quot;) Em seguida, você pode ter o banco de dados criado em tempo real de suas classes, que permite que um fluxo de trabalho de desenvolvimento muito claro e rápido.</span><span class="sxs-lookup"><span data-stu-id="aec89-109">(These are also known as POCO classes, from &quot;plain-old CLR objects.&quot;) You can then have the database created on the fly from your classes, which enables a very clean and rapid development workflow.</span></span> <span data-ttu-id="aec89-110">Se você precisar criar o banco de dados pela primeira vez, você ainda pode seguir este tutorial para saber mais sobre o desenvolvimento de aplicativo do MVC e ao EF.</span><span class="sxs-lookup"><span data-stu-id="aec89-110">If you are required to create the database first, you can still follow this tutorial to learn about MVC and EF app development.</span></span> <span data-ttu-id="aec89-111">Em seguida, você pode seguir Tom Fizmakens [Scaffolding do ASP.NET](xref:visual-studio/overview/2013/aspnet-scaffolding-overview) tutorial, que abrange a primeira abordagem de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="aec89-111">You can then follow Tom Fizmakens [ASP.NET Scaffolding](xref:visual-studio/overview/2013/aspnet-scaffolding-overview) tutorial, which covers the database first approach.</span></span>

## <a name="adding-model-classes"></a><span data-ttu-id="aec89-112">Adicionando Classes de modelo</span><span class="sxs-lookup"><span data-stu-id="aec89-112">Adding Model Classes</span></span>

<span data-ttu-id="aec89-113">Na **Gerenciador de soluções**, clique com botão direito do *modelos* pasta, selecione **Add**e, em seguida, selecione **classe**.</span><span class="sxs-lookup"><span data-stu-id="aec89-113">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-model/_static/image1.png)

<span data-ttu-id="aec89-114">Insira o *classe* nome &quot;filme&quot;.</span><span class="sxs-lookup"><span data-stu-id="aec89-114">Enter the *class* name &quot;Movie&quot;.</span></span>

<span data-ttu-id="aec89-115">Adicione as seguintes propriedades de cinco para o `Movie` classe:</span><span class="sxs-lookup"><span data-stu-id="aec89-115">Add the following five properties to the `Movie` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

<span data-ttu-id="aec89-116">Vamos usar o `Movie` classe para representar os filmes em um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="aec89-116">We'll use the `Movie` class to represent movies in a database.</span></span> <span data-ttu-id="aec89-117">Cada instância de um `Movie` objeto corresponderá a uma linha em uma tabela de banco de dados e cada propriedade do `Movie` classe será mapeado para uma coluna na tabela.</span><span class="sxs-lookup"><span data-stu-id="aec89-117">Each instance of a `Movie` object will correspond to a row within a database table, and each property of the `Movie` class will map to a column in the table.</span></span>

<span data-ttu-id="aec89-118">Observação: Para usar Entity e a classe relacionada, você precisa instalar o [pacote do NuGet do Entity Framework](https://www.nuget.org/packages/EntityFramework/).</span><span class="sxs-lookup"><span data-stu-id="aec89-118">Note: In order to use System.Data.Entity, and the related class, you need to install the [Entity Framework NuGet Package](https://www.nuget.org/packages/EntityFramework/).</span></span> <span data-ttu-id="aec89-119">Siga o link para obter mais instruções.</span><span class="sxs-lookup"><span data-stu-id="aec89-119">Follow the link for further instructions.</span></span>

<span data-ttu-id="aec89-120">No mesmo arquivo, adicione o seguinte `MovieDBContext` classe:</span><span class="sxs-lookup"><span data-stu-id="aec89-120">In the same file, add the following `MovieDBContext` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample2.cs?highlight=2,15-18)]

<span data-ttu-id="aec89-121">O `MovieDBContext` classe representa o contexto de banco de dados de filme do Entity Framework, que lida com a busca, armazenar e atualizar `Movie` instâncias em um banco de dados de classe.</span><span class="sxs-lookup"><span data-stu-id="aec89-121">The `MovieDBContext` class represents the Entity Framework movie database context, which handles fetching, storing, and updating `Movie` class instances in a database.</span></span> <span data-ttu-id="aec89-122">O `MovieDBContext` deriva o `DbContext` fornecido pela estrutura de entidades de classe base.</span><span class="sxs-lookup"><span data-stu-id="aec89-122">The `MovieDBContext` derives from the `DbContext` base class provided by the Entity Framework.</span></span>

<span data-ttu-id="aec89-123">Para poder referenciar `DbContext` e `DbSet`, você precisará adicionar o seguinte `using` instrução na parte superior do arquivo:</span><span class="sxs-lookup"><span data-stu-id="aec89-123">In order to be able to reference `DbContext` and `DbSet`, you need to add the following `using` statement at the top of the file:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

<span data-ttu-id="aec89-124">Você pode fazer isso adicionando manualmente o usando instrução, ou você pode passar o mouse sobre as linhas curvadas vermelhas, clique em `Show potential fixes` e clique em `using System.Data.Entity;`</span><span class="sxs-lookup"><span data-stu-id="aec89-124">You can do this by manually adding the using statement, or you can hover over the red squiggly lines, click `Show potential fixes` and click `using System.Data.Entity;`</span></span>

![](adding-a-model/_static/image2.png)

<span data-ttu-id="aec89-125">Observação: Várias não utilizados `using` instruções foram removidas.</span><span class="sxs-lookup"><span data-stu-id="aec89-125">Note: Several unused `using` statements have been removed.</span></span> <span data-ttu-id="aec89-126">Visual Studio mostrará dependências não utilizadas como cinza.</span><span class="sxs-lookup"><span data-stu-id="aec89-126">Visual Studio will show unused dependencies as gray.</span></span> <span data-ttu-id="aec89-127">Você pode remover as dependências não utilizadas focalizando as dependências de cinza, clique em `Show potential fixes` e clique em **remover usos não utilizados.**</span><span class="sxs-lookup"><span data-stu-id="aec89-127">You can remove unused dependencies by hovering over the gray dependencies, click `Show potential fixes` and click **Remove Unused Usings.**</span></span>

![](adding-a-model/_static/image3.png)

<span data-ttu-id="aec89-128">Por fim, adicionamos um modelo (M no MVC).</span><span class="sxs-lookup"><span data-stu-id="aec89-128">We've finally added a model (the M in MVC).</span></span> <span data-ttu-id="aec89-129">Na próxima seção, você trabalhará com a cadeia de caracteres de conexão de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="aec89-129">In the next section you'll work with the database connection string.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="aec89-130">[Anterior](adding-a-view.md)
> [Próximo](creating-a-connection-string.md)</span><span class="sxs-lookup"><span data-stu-id="aec89-130">[Previous](adding-a-view.md)
[Next](creating-a-connection-string.md)</span></span>

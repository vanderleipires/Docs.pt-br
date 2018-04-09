---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-model
title: Adicionando um modelo (c#) | Microsoft Docs
author: Rick-Anderson
description: 'Observação: Uma versão atualizada deste tutorial está disponível aqui que usa o ASP.NET MVC 5 e Visual Studio 2013. É mais seguro e muito mais simples de seguir e demonstração...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 42355b95-5f1f-413e-8d16-14cdfaaefcd8
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: a7f9206ddd43b4a3b4f96240ee48b9414450da22
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="adding-a-model-c"></a><span data-ttu-id="1be57-104">Adicionando um modelo (c#)</span><span class="sxs-lookup"><span data-stu-id="1be57-104">Adding a Model (C#)</span></span>
====================
<span data-ttu-id="1be57-105">por [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="1be57-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="1be57-106">Este tutorial ensina as Noções básicas de criação de um aplicativo Web do ASP.NET MVC usando o Microsoft Visual Web Developer 2010 Express Service Pack 1, que é uma versão gratuita do Microsoft Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1be57-106">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="1be57-107">Antes de começar, verifique se que você instalou os pré-requisitos listados abaixo.</span><span class="sxs-lookup"><span data-stu-id="1be57-107">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="1be57-108">Você pode instalar todos eles clicando no link a seguir: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="1be57-108">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="1be57-109">Como alternativa, você pode instalar individualmente os pré-requisitos usando os links a seguir:</span><span class="sxs-lookup"><span data-stu-id="1be57-109">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="1be57-110">Pré-requisitos de Visual Studio Web Developer Express SP1</span><span class="sxs-lookup"><span data-stu-id="1be57-110">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="1be57-111">Atualização de ferramentas do ASP.NET MVC 3</span><span class="sxs-lookup"><span data-stu-id="1be57-111">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="1be57-112">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(tempo de execução + ferramentas de suportam)</span><span class="sxs-lookup"><span data-stu-id="1be57-112">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="1be57-113">Se você estiver usando o Visual Studio 2010 em vez do Visual Web Developer 2010, instale os pré-requisitos clicando no link a seguir: [pré-requisitos do Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="1be57-113">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="1be57-114">Um projeto do Visual Web Developer ao código-fonte c# está disponível para acompanhar este tópico.</span><span class="sxs-lookup"><span data-stu-id="1be57-114">A Visual Web Developer project with C# source code is available to accompany this topic.</span></span> <span data-ttu-id="1be57-115">[Baixe a versão c#](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span><span class="sxs-lookup"><span data-stu-id="1be57-115">[Download the C# version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="1be57-116">Se você preferir o Visual Basic, alterne para o [versão do Visual Basic](../vb/adding-a-model.md) deste tutorial.</span><span class="sxs-lookup"><span data-stu-id="1be57-116">If you prefer Visual Basic, switch to the [Visual Basic version](../vb/adding-a-model.md) of this tutorial.</span></span>


## <a name="adding-a-model"></a><span data-ttu-id="1be57-117">Adicionando um modelo</span><span class="sxs-lookup"><span data-stu-id="1be57-117">Adding a Model</span></span>

<span data-ttu-id="1be57-118">Nesta seção, você adicionará algumas classes de gerenciamento de filmes em um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="1be57-118">In this section you'll add some classes for managing movies in a database.</span></span> <span data-ttu-id="1be57-119">Essas classes será a parte "modelo" do aplicativo ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="1be57-119">These classes will be the "model" part of the ASP.NET MVC application.</span></span>

<span data-ttu-id="1be57-120">Você usará uma tecnologia de acesso a dados do .NET Framework conhecida como o Entity Framework para definir e trabalhar com essas classes de modelo.</span><span class="sxs-lookup"><span data-stu-id="1be57-120">You'll use a .NET Framework data-access technology known as the Entity Framework to define and work with these model classes.</span></span> <span data-ttu-id="1be57-121">O Entity Framework (também conhecido como EF) oferece suporte a um paradigma de desenvolvimento chamado *Code First*.</span><span class="sxs-lookup"><span data-stu-id="1be57-121">The Entity Framework (often referred to as EF) supports a development paradigm called *Code First*.</span></span> <span data-ttu-id="1be57-122">Código primeiro permite que você crie objetos de modelo, escrevendo classes simples.</span><span class="sxs-lookup"><span data-stu-id="1be57-122">Code First allows you to create model objects by writing simple classes.</span></span> <span data-ttu-id="1be57-123">(Esses são também conhecidos como classes POCO, de "objetos CLR plain old".) Em seguida, você pode ter o banco de dados criado dinamicamente de suas classes, que permite que um fluxo de trabalho de desenvolvimento muito claro e rápida.</span><span class="sxs-lookup"><span data-stu-id="1be57-123">(These are also known as POCO classes, from "plain-old CLR objects.") You can then have the database created on the fly from your classes, which enables a very clean and rapid development workflow.</span></span>

## <a name="adding-model-classes"></a><span data-ttu-id="1be57-124">Adicionando Classes de modelo</span><span class="sxs-lookup"><span data-stu-id="1be57-124">Adding Model Classes</span></span>

<span data-ttu-id="1be57-125">Em **Solution Explorer**, clique com botão direito do *modelos* pasta, selecione **adicionar**e, em seguida, selecione **classe**.</span><span class="sxs-lookup"><span data-stu-id="1be57-125">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-model/_static/image1.png)

<span data-ttu-id="1be57-126">Nome do *classe* "Filme".</span><span class="sxs-lookup"><span data-stu-id="1be57-126">Name the *class* "Movie".</span></span>

<span data-ttu-id="1be57-127">[![CreateMovieClass](adding-a-model/_static/image3.png)](adding-a-model/_static/image2.png)</span><span class="sxs-lookup"><span data-stu-id="1be57-127">[![CreateMovieClass](adding-a-model/_static/image3.png)](adding-a-model/_static/image2.png)</span></span>

<span data-ttu-id="1be57-128">Adicione as seguintes cinco propriedades para o `Movie` classe:</span><span class="sxs-lookup"><span data-stu-id="1be57-128">Add the following five properties to the `Movie` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

<span data-ttu-id="1be57-129">Usaremos a `Movie` classe para representar filmes em um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="1be57-129">We'll use the `Movie` class to represent movies in a database.</span></span> <span data-ttu-id="1be57-130">Cada instância de um `Movie` correspondam a uma linha em uma tabela de banco de dados e cada propriedade do objeto de `Movie` classe será mapeado para uma coluna na tabela.</span><span class="sxs-lookup"><span data-stu-id="1be57-130">Each instance of a `Movie` object will correspond to a row within a database table, and each property of the `Movie` class will map to a column in the table.</span></span>

<span data-ttu-id="1be57-131">No mesmo arquivo, adicione o seguinte `MovieDBContext` classe:</span><span class="sxs-lookup"><span data-stu-id="1be57-131">In the same file, add the following `MovieDBContext` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample2.cs)]

<span data-ttu-id="1be57-132">O `MovieDBContext` classe representa o contexto de banco de dados do filme do Entity Framework, que manipula a busca, armazenar e atualizar `Movie` instâncias em um banco de dados de classe.</span><span class="sxs-lookup"><span data-stu-id="1be57-132">The `MovieDBContext` class represents the Entity Framework movie database context, which handles fetching, storing, and updating `Movie` class instances in a database.</span></span> <span data-ttu-id="1be57-133">O `MovieDBContext` deriva o `DbContext` fornecido pelo Entity Framework de classe base.</span><span class="sxs-lookup"><span data-stu-id="1be57-133">The `MovieDBContext` derives from the `DbContext` base class provided by the Entity Framework.</span></span> <span data-ttu-id="1be57-134">Para obter mais informações sobre `DbContext` e `DbSet`, consulte [melhorias de produtividade para Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).</span><span class="sxs-lookup"><span data-stu-id="1be57-134">For more information about `DbContext` and `DbSet`, see [Productivity Improvements for the Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).</span></span>

<span data-ttu-id="1be57-135">Para poder referenciar `DbContext` e `DbSet`, você precisa adicionar o seguinte `using` instrução na parte superior do arquivo:</span><span class="sxs-lookup"><span data-stu-id="1be57-135">In order to be able to reference `DbContext` and `DbSet`, you need to add the following `using` statement at the top of the file:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

<span data-ttu-id="1be57-136">Completo *Movie.cs* arquivo é mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="1be57-136">The complete *Movie.cs* file is shown below.</span></span>

[!code-csharp[Main](adding-a-model/samples/sample4.cs)]

## <a name="creating-a-connection-string-and-working-with-sql-server-compact"></a><span data-ttu-id="1be57-137">Criando uma cadeia de Conexão e trabalhar com o SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="1be57-137">Creating a Connection String and Working with SQL Server Compact</span></span>

<span data-ttu-id="1be57-138">O `MovieDBContext` classe criada lida com a tarefa de conectar-se ao banco de dados e o mapeamento `Movie` objetos registros do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="1be57-138">The `MovieDBContext` class you created handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="1be57-139">Uma pergunta, que você pode perguntar, porém, é como especificar o banco de dados que ele se conectará.</span><span class="sxs-lookup"><span data-stu-id="1be57-139">One question you might ask, though, is how to specify which database it will connect to.</span></span> <span data-ttu-id="1be57-140">Você fará isso adicionando informações de conexão no *Web. config* arquivo do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="1be57-140">You'll do that by adding connection information in the *Web.config* file of the application.</span></span>

<span data-ttu-id="1be57-141">Abra a raiz do aplicativo *Web. config* arquivo.</span><span class="sxs-lookup"><span data-stu-id="1be57-141">Open the application root *Web.config* file.</span></span> <span data-ttu-id="1be57-142">(Não o *Web. config* arquivo o *exibições* pasta.) A imagem a seguir mostrar ambos *Web. config* arquivos; abrir o *Web. config* arquivo dentro de um círculo em vermelho.</span><span class="sxs-lookup"><span data-stu-id="1be57-142">(Not the *Web.config* file in the *Views* folder.) The image below show both *Web.config* files; open the *Web.config* file circled in red.</span></span>

![](adding-a-model/_static/image4.png)

### 

<span data-ttu-id="1be57-143">Adicione a seguinte cadeia de conexão para o `<connectionStrings>` elemento o *Web. config* arquivo.</span><span class="sxs-lookup"><span data-stu-id="1be57-143">Add the following connection string to the `<connectionStrings>` element in the *Web.config* file.</span></span>

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

<span data-ttu-id="1be57-144">O exemplo a seguir mostra uma parte do *Web. config* arquivo com a cadeia de caracteres de conexão nova adicionada:</span><span class="sxs-lookup"><span data-stu-id="1be57-144">The following example shows a portion of the *Web.config* file with the new connection string added:</span></span>

[!code-xml[Main](adding-a-model/samples/sample6.xml)]

<span data-ttu-id="1be57-145">Essa quantidade pequena de código e o XML é tudo o que você precisa gravar para representar e armazenar os dados do filme em um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="1be57-145">This small amount of code and XML is everything you need to write in order to represent and store the movie data in a database.</span></span>

<span data-ttu-id="1be57-146">Em seguida, você criará um novo `MoviesController` classe que você pode usar para exibir os dados do filme e permitir que os usuários criem novas listagens de filme.</span><span class="sxs-lookup"><span data-stu-id="1be57-146">Next, you'll build a new `MoviesController` class that you can use to display the movie data and allow users to create new movie listings.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="1be57-147">[Anterior](adding-a-view.md)
> [Próximo](accessing-your-models-data-from-a-controller.md)</span><span class="sxs-lookup"><span data-stu-id="1be57-147">[Previous](adding-a-view.md)
[Next](accessing-your-models-data-from-a-controller.md)</span></span>

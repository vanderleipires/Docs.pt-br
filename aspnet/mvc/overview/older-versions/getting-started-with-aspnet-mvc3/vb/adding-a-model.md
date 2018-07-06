---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-model
title: Adicionando um modelo (VB) | Microsoft Docs
author: Rick-Anderson
description: Este tutorial ensinará os conceitos básicos da criação de um aplicativo Web ASP.NET MVC usando o Microsoft Visual Web Developer 2010 Express Service Pack 1, que é...
ms.author: aspnetcontent
ms.date: 01/12/2011
ms.assetid: b3aa7720-5c78-4ca2-baef-9a52234fb7ce
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: 138d21d5e33384fbc0dd99c33a9e43a95976ed06
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37827800"
---
<a name="adding-a-model-vb"></a><span data-ttu-id="e6cc7-103">Adicionando um modelo (VB)</span><span class="sxs-lookup"><span data-stu-id="e6cc7-103">Adding a Model (VB)</span></span>
====================
<span data-ttu-id="e6cc7-104">por [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="e6cc7-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="e6cc7-105">Este tutorial ensinará os conceitos básicos da criação de um aplicativo Web ASP.NET MVC usando o Microsoft Visual Web Developer 2010 Express Service Pack 1, que é uma versão gratuita do Microsoft Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e6cc7-105">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="e6cc7-106">Antes de começar, verifique se que você instalou os pré-requisitos listados abaixo.</span><span class="sxs-lookup"><span data-stu-id="e6cc7-106">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="e6cc7-107">Você pode instalar todos eles clicando no link a seguir: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="e6cc7-107">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="e6cc7-108">Como alternativa, você pode instalar individualmente os pré-requisitos usando os links a seguir:</span><span class="sxs-lookup"><span data-stu-id="e6cc7-108">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="e6cc7-109">Pré-requisitos de Visual Studio Web Developer Express SP1</span><span class="sxs-lookup"><span data-stu-id="e6cc7-109">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="e6cc7-110">Atualização de ferramentas do ASP.NET MVC 3</span><span class="sxs-lookup"><span data-stu-id="e6cc7-110">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="e6cc7-111">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(tempo de execução de ferramentas de suporte +)</span><span class="sxs-lookup"><span data-stu-id="e6cc7-111">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="e6cc7-112">Se você estiver usando o Visual Studio 2010, em vez do Visual Web Developer 2010, instale os pré-requisitos, clicando no link a seguir: [pré-requisitos do Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="e6cc7-112">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="e6cc7-113">Um projeto do Visual Web Developer com código-fonte VB.NET está disponível para acompanhar este tópico.</span><span class="sxs-lookup"><span data-stu-id="e6cc7-113">A Visual Web Developer project with VB.NET source code is available to accompany this topic.</span></span> <span data-ttu-id="e6cc7-114">[Baixe a versão do VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span><span class="sxs-lookup"><span data-stu-id="e6cc7-114">[Download the VB.NET version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="e6cc7-115">Se você preferir o c#, alterne para o [c# versão](../cs/adding-a-model.md) deste tutorial.</span><span class="sxs-lookup"><span data-stu-id="e6cc7-115">If you prefer C#, switch to the [C# version](../cs/adding-a-model.md) of this tutorial.</span></span>


## <a name="adding-a-model"></a><span data-ttu-id="e6cc7-116">Adicionando um modelo</span><span class="sxs-lookup"><span data-stu-id="e6cc7-116">Adding a Model</span></span>

<span data-ttu-id="e6cc7-117">Nesta seção, você adicionará algumas classes para o gerenciamento de filmes em um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="e6cc7-117">In this section you'll add some classes for managing movies in a database.</span></span> <span data-ttu-id="e6cc7-118">Essas classes será a parte de "modelo" do aplicativo ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="e6cc7-118">These classes will be the "model" part of the ASP.NET MVC application.</span></span>

<span data-ttu-id="e6cc7-119">Você usará uma tecnologia de acesso a dados do .NET Framework conhecida como o Entity Framework para definir e trabalhar com essas classes de modelo.</span><span class="sxs-lookup"><span data-stu-id="e6cc7-119">You'll use a .NET Framework data-access technology known as the Entity Framework to define and work with these model classes.</span></span> <span data-ttu-id="e6cc7-120">A Entity Framework (também conhecido como EF) oferece suporte a um paradigma de desenvolvimento chamado *Code First*.</span><span class="sxs-lookup"><span data-stu-id="e6cc7-120">The Entity Framework (often referred to as EF) supports a development paradigm called *Code First*.</span></span> <span data-ttu-id="e6cc7-121">Código primeiro permite que você crie objetos de modelo ao escrever classes simples.</span><span class="sxs-lookup"><span data-stu-id="e6cc7-121">Code First allows you to create model objects by writing simple classes.</span></span> <span data-ttu-id="e6cc7-122">(Esses são também conhecidas como classes POCO, de "objetos CLR do BOM e velho".) Em seguida, você pode ter o banco de dados criado em tempo real de suas classes, que permite que um fluxo de trabalho de desenvolvimento muito claro e rápido.</span><span class="sxs-lookup"><span data-stu-id="e6cc7-122">(These are also known as POCO classes, from "plain-old CLR objects.") You can then have the database created on the fly from your classes, which enables a very clean and rapid development workflow.</span></span>

## <a name="adding-model-classes"></a><span data-ttu-id="e6cc7-123">Adicionando Classes de modelo</span><span class="sxs-lookup"><span data-stu-id="e6cc7-123">Adding Model Classes</span></span>

<span data-ttu-id="e6cc7-124">Na **Gerenciador de soluções**, clique com botão direito do *modelos* pasta, selecione **Add**e, em seguida, selecione **classe**.</span><span class="sxs-lookup"><span data-stu-id="e6cc7-124">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-model/_static/image1.png)

<span data-ttu-id="e6cc7-125">Nomeie a classe "Filme".</span><span class="sxs-lookup"><span data-stu-id="e6cc7-125">Name the class "Movie".</span></span>

<span data-ttu-id="e6cc7-126">Adicione as seguintes propriedades de cinco para o `Movie` classe:</span><span class="sxs-lookup"><span data-stu-id="e6cc7-126">Add the following five properties to the `Movie` class:</span></span>

[!code-vb[Main](adding-a-model/samples/sample1.vb)]

<span data-ttu-id="e6cc7-127">Vamos usar o `Movie` classe para representar os filmes em um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="e6cc7-127">We'll use the `Movie` class to represent movies in a database.</span></span> <span data-ttu-id="e6cc7-128">Cada instância de um `Movie` objeto corresponderá a uma linha em uma tabela de banco de dados e cada propriedade do `Movie` classe será mapeado para uma coluna na tabela.</span><span class="sxs-lookup"><span data-stu-id="e6cc7-128">Each instance of a `Movie` object will correspond to a row within a database table, and each property of the `Movie` class will map to a column in the table.</span></span>

<span data-ttu-id="e6cc7-129">No mesmo arquivo, adicione o seguinte `MovieDBContext` classe:</span><span class="sxs-lookup"><span data-stu-id="e6cc7-129">In the same file, add the following `MovieDBContext` class:</span></span>

[!code-vb[Main](adding-a-model/samples/sample2.vb)]

<span data-ttu-id="e6cc7-130">O `MovieDBContext` classe representa o contexto de banco de dados de filme do Entity Framework, que lida com a busca, armazenar e atualizar `Movie` instâncias em um banco de dados de classe.</span><span class="sxs-lookup"><span data-stu-id="e6cc7-130">The `MovieDBContext` class represents the Entity Framework movie database context, which handles fetching, storing, and updating `Movie` class instances in a database.</span></span> <span data-ttu-id="e6cc7-131">O `MovieDBContext` deriva o `DbContext` fornecido pela estrutura de entidades de classe base.</span><span class="sxs-lookup"><span data-stu-id="e6cc7-131">The `MovieDBContext` derives from the `DbContext` base class provided by the Entity Framework.</span></span> <span data-ttu-id="e6cc7-132">Para obter mais informações sobre `DbContext` e `DbSet`, consulte [melhorias de produtividade para o Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).</span><span class="sxs-lookup"><span data-stu-id="e6cc7-132">For more information about `DbContext` and `DbSet`, see [Productivity Improvements for the Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).</span></span>

<span data-ttu-id="e6cc7-133">Para poder referenciar `DbContext` e `DbSet`, você precisará adicionar o seguinte `imports` instrução na parte superior do arquivo:</span><span class="sxs-lookup"><span data-stu-id="e6cc7-133">In order to be able to reference `DbContext` and `DbSet`, you need to add the following `imports` statement at the top of the file:</span></span>

[!code-vb[Main](adding-a-model/samples/sample3.vb)]

<span data-ttu-id="e6cc7-134">A conclusão *Movie.vb* arquivo é mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="e6cc7-134">The complete *Movie.vb* file is shown below.</span></span>

[!code-vb[Main](adding-a-model/samples/sample4.vb)]

## <a name="creating-a-connection-string-and-working-with-sql-server-compact"></a><span data-ttu-id="e6cc7-135">Criando uma cadeia de Conexão e trabalhar com o SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="e6cc7-135">Creating a Connection String and Working with SQL Server Compact</span></span>

<span data-ttu-id="e6cc7-136">O `MovieDBContext` classe criada por você lida com a tarefa de conectar-se ao banco de dados e mapeamento `Movie` objetos para os registros de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="e6cc7-136">The `MovieDBContext` class you created handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="e6cc7-137">Uma pergunta que você pode fazer, no entanto, é como especificar qual banco de dados que ele se conectará.</span><span class="sxs-lookup"><span data-stu-id="e6cc7-137">One question you might ask, though, is how to specify which database it will connect to.</span></span> <span data-ttu-id="e6cc7-138">Você terá de fazer isso adicionando as informações de conexão na *Web. config* arquivo do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="e6cc7-138">You'll do that by adding connection information in the *Web.config* file of the application.</span></span>

<span data-ttu-id="e6cc7-139">Abra a raiz do aplicativo *Web. config* arquivo.</span><span class="sxs-lookup"><span data-stu-id="e6cc7-139">Open the application root *Web.config* file.</span></span> <span data-ttu-id="e6cc7-140">(Não o *Web. config* arquivo na *modos de exibição* pasta.) A imagem a seguir mostrar ambos *Web. config* arquivos; abrir os *Web. config* arquivo circulados em vermelho.</span><span class="sxs-lookup"><span data-stu-id="e6cc7-140">(Not the *Web.config* file in the *Views* folder.) The image below show both *Web.config* files; open the *Web.config* file circled in red.</span></span>

![](adding-a-model/_static/image2.png)

## 

<span data-ttu-id="e6cc7-141">Adicione a seguinte cadeia de conexão para o `<connectionStrings>` elemento na *Web. config* arquivo.</span><span class="sxs-lookup"><span data-stu-id="e6cc7-141">Add the following connection string to the `<connectionStrings>` element in the *Web.config* file.</span></span>

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

<span data-ttu-id="e6cc7-142">O exemplo a seguir mostra uma parte do *Web. config* arquivo com a cadeia de caracteres de conexão nova adicionada:</span><span class="sxs-lookup"><span data-stu-id="e6cc7-142">The following example shows a portion of the *Web.config* file with the new connection string added:</span></span>

[!code-xml[Main](adding-a-model/samples/sample6.xml)]

<span data-ttu-id="e6cc7-143">Essa pequena quantidade de código e o XML é tudo o que precisa ser escrita para representar e armazenar os dados de filmes em um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="e6cc7-143">This small amount of code and XML is everything you need to write in order to represent and store the movie data in a database.</span></span>

<span data-ttu-id="e6cc7-144">Em seguida, você criará um novo `MoviesController` classe que você pode usar para exibir os dados de filme e permitir que usuários criem novas listagens de filme.</span><span class="sxs-lookup"><span data-stu-id="e6cc7-144">Next, you'll build a new `MoviesController` class that you can use to display the movie data and allow users to create new movie listings.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e6cc7-145">[Anterior](adding-a-view.md)
> [Próximo](accessing-your-models-data-from-a-controller.md)</span><span class="sxs-lookup"><span data-stu-id="e6cc7-145">[Previous](adding-a-view.md)
[Next](accessing-your-models-data-from-a-controller.md)</span></span>

---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3
title: Introdução ao ASP.NET MVC 3 (c#) | Microsoft Docs
author: Rick-Anderson
description: Este tutorial ensina as Noções básicas de criação de um aplicativo Web do ASP.NET MVC usando o Microsoft Visual Web Developer 2010 Express Service Pack 1, que é...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 86a80b35-88bd-4b7c-bd58-f6e7997197d4
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3
msc.type: authoredcontent
ms.openlocfilehash: 9b965df6175051a084de35627160161c116be42d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868923"
---
<a name="intro-to-aspnet-mvc-3-c"></a><span data-ttu-id="f4beb-103">Introdução ao ASP.NET MVC 3 (c#)</span><span class="sxs-lookup"><span data-stu-id="f4beb-103">Intro to ASP.NET MVC 3 (C#)</span></span>
====================
<span data-ttu-id="f4beb-104">por [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="f4beb-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> > [!NOTE]
> > <span data-ttu-id="f4beb-105">Uma versão atualizada deste tutorial está disponível [aqui](../../../getting-started/introduction/getting-started.md) que usa o ASP.NET MVC 5 e Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="f4beb-105">An updated version of this tutorial is available [here](../../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="f4beb-106">É muito mais simples a seguir, mais segura e demonstra mais recursos.</span><span class="sxs-lookup"><span data-stu-id="f4beb-106">It's more secure, much simpler to follow and demonstrates more features.</span></span>
> 
> 
> <span data-ttu-id="f4beb-107">Este tutorial ensina as Noções básicas de criação de um aplicativo Web do ASP.NET MVC usando o Microsoft Visual Web Developer 2010 Express Service Pack 1, que é uma versão gratuita do Microsoft Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f4beb-107">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="f4beb-108">Antes de começar, verifique se que você instalou os pré-requisitos listados abaixo.</span><span class="sxs-lookup"><span data-stu-id="f4beb-108">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="f4beb-109">Você pode instalar todos eles clicando no link a seguir: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="f4beb-109">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="f4beb-110">Como alternativa, você pode instalar individualmente os pré-requisitos usando os links a seguir:</span><span class="sxs-lookup"><span data-stu-id="f4beb-110">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="f4beb-111">Pré-requisitos de Visual Studio Web Developer Express SP1</span><span class="sxs-lookup"><span data-stu-id="f4beb-111">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="f4beb-112">Atualização de ferramentas do ASP.NET MVC 3</span><span class="sxs-lookup"><span data-stu-id="f4beb-112">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="f4beb-113">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(tempo de execução + ferramentas de suportam)</span><span class="sxs-lookup"><span data-stu-id="f4beb-113">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="f4beb-114">Se você estiver usando o Visual Studio 2010 em vez do Visual Web Developer 2010, instale os pré-requisitos clicando no link a seguir: [pré-requisitos do Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="f4beb-114">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="f4beb-115">Um projeto do Visual Web Developer ao código-fonte c# está disponível para acompanhar este tópico.</span><span class="sxs-lookup"><span data-stu-id="f4beb-115">A Visual Web Developer project with C# source code is available to accompany this topic.</span></span> <span data-ttu-id="f4beb-116">[Baixe a versão c#](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span><span class="sxs-lookup"><span data-stu-id="f4beb-116">[Download the C# version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="f4beb-117">Se você preferir o Visual Basic, alterne para o [versão do Visual Basic](../vb/intro-to-aspnet-mvc-3.md) deste tutorial.</span><span class="sxs-lookup"><span data-stu-id="f4beb-117">If you prefer Visual Basic, switch to the [Visual Basic version](../vb/intro-to-aspnet-mvc-3.md) of this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="f4beb-118">O que você vai criar</span><span class="sxs-lookup"><span data-stu-id="f4beb-118">What You'll Build</span></span>

<span data-ttu-id="f4beb-119">Você implementará um simples aplicativo de listagem de filme que dá suporte à criação, edição e listando filmes de um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="f4beb-119">You'll implement a simple movie-listing application that supports creating, editing, and listing movies from a database.</span></span> <span data-ttu-id="f4beb-120">Abaixo estão as duas capturas de tela do aplicativo que você criará.</span><span class="sxs-lookup"><span data-stu-id="f4beb-120">Below are two screenshots of the application you'll build.</span></span> <span data-ttu-id="f4beb-121">Ele inclui uma página que exibe uma lista de filmes de um banco de dados:</span><span class="sxs-lookup"><span data-stu-id="f4beb-121">It includes a page that displays a list of movies from a database:</span></span>

![MoviesWithVariousSm](intro-to-aspnet-mvc-3/_static/image1.png)

<span data-ttu-id="f4beb-123">O aplicativo também permite adicionar, editar e excluir filmes, assim como para ver detalhes sobre os problemas individuais.</span><span class="sxs-lookup"><span data-stu-id="f4beb-123">The application also lets you add, edit, and delete movies, as well as see details about individual ones.</span></span> <span data-ttu-id="f4beb-124">Todos os cenários de entrada de dados incluem validação para garantir que os dados armazenados no banco de dados estão corretos.</span><span class="sxs-lookup"><span data-stu-id="f4beb-124">All data-entry scenarios include validation to ensure that the data stored in the database is correct.</span></span>

![](intro-to-aspnet-mvc-3/_static/image2.png)

## <a name="skills-youll-learn"></a><span data-ttu-id="f4beb-125">Você vai aprender as habilidades</span><span class="sxs-lookup"><span data-stu-id="f4beb-125">Skills You'll Learn</span></span>

<span data-ttu-id="f4beb-126">Aqui está o que você aprenderá:</span><span class="sxs-lookup"><span data-stu-id="f4beb-126">Here's what you'll learn:</span></span>

- <span data-ttu-id="f4beb-127">Como criar um novo projeto ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="f4beb-127">How to create a new ASP.NET MVC project.</span></span>
- <span data-ttu-id="f4beb-128">Como criar o ASP.NET MVC controladores e exibições.</span><span class="sxs-lookup"><span data-stu-id="f4beb-128">How to create ASP.NET MVC controllers and views.</span></span>
- <span data-ttu-id="f4beb-129">Como criar um novo banco de dados usando o paradigma do Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="f4beb-129">How to create a new database using the Entity Framework Code First paradigm.</span></span>
- <span data-ttu-id="f4beb-130">Como recuperar e exibir dados.</span><span class="sxs-lookup"><span data-stu-id="f4beb-130">How to retrieve and display data.</span></span>
- <span data-ttu-id="f4beb-131">Como editar os dados e habilitar a validação de dados.</span><span class="sxs-lookup"><span data-stu-id="f4beb-131">How to edit data and enable data validation.</span></span>

## <a name="getting-started"></a><span data-ttu-id="f4beb-132">Guia de Introdução</span><span class="sxs-lookup"><span data-stu-id="f4beb-132">Getting Started</span></span>

<span data-ttu-id="f4beb-133">Inicie executando o Visual Web Developer 2010 Express ("Visual Web Developer" em resumo) e selecione **novo projeto** do **iniciar** página.</span><span class="sxs-lookup"><span data-stu-id="f4beb-133">Start by running Visual Web Developer 2010 Express ("Visual Web Developer" for short) and select **New Project** from the **Start** page.</span></span>

<span data-ttu-id="f4beb-134">O Visual Web Developer é um IDE ou ambiente de desenvolvimento integrado.</span><span class="sxs-lookup"><span data-stu-id="f4beb-134">Visual Web Developer is an IDE, or integrated development environment.</span></span> <span data-ttu-id="f4beb-135">Como usar o Microsoft Word para escrever documentos, você usará um IDE para criar aplicativos.</span><span class="sxs-lookup"><span data-stu-id="f4beb-135">Just like you use Microsoft Word to write documents, you'll use an IDE to create applications.</span></span> <span data-ttu-id="f4beb-136">No Visual Web Developer, há uma barra de ferramentas na parte superior, mostrando várias opções disponíveis para você.</span><span class="sxs-lookup"><span data-stu-id="f4beb-136">In Visual Web Developer there's a toolbar along the top showing various options available to you.</span></span> <span data-ttu-id="f4beb-137">Há também um menu que oferece uma maneira de realizar tarefas no IDE.</span><span class="sxs-lookup"><span data-stu-id="f4beb-137">There's also a menu that provides another way to perform tasks in the IDE.</span></span> <span data-ttu-id="f4beb-138">(Por exemplo, em vez de selecionar **novo projeto** do **iniciar** página, você pode usar o menu e selecione **arquivo** &gt; **denovoprojeto**.)</span><span class="sxs-lookup"><span data-stu-id="f4beb-138">(For example, instead of selecting **New Project** from the **Start** page, you can use the menu and select **File** &gt; **New Project**.)</span></span>

[![](intro-to-aspnet-mvc-3/_static/image4.png)](intro-to-aspnet-mvc-3/_static/image3.png)

## <a name="creating-your-first-application"></a><span data-ttu-id="f4beb-139">Criando seu primeiro aplicativo</span><span class="sxs-lookup"><span data-stu-id="f4beb-139">Creating Your First Application</span></span>

<span data-ttu-id="f4beb-140">Você pode criar aplicativos usando o Visual Basic ou Visual c# como linguagem de programação.</span><span class="sxs-lookup"><span data-stu-id="f4beb-140">You can create applications using either Visual Basic or Visual C# as the programming language.</span></span> <span data-ttu-id="f4beb-141">Selecione o Visual C# à esquerda e, em seguida, selecione **aplicativo Web do ASP.NET MVC 3**.</span><span class="sxs-lookup"><span data-stu-id="f4beb-141">Select Visual C# on the left and then select **ASP.NET MVC 3 Web Application**.</span></span> <span data-ttu-id="f4beb-142">Nomeie o projeto "MvcMovie" e, em seguida, clique em **Okey**.</span><span class="sxs-lookup"><span data-stu-id="f4beb-142">Name your project "MvcMovie" and then click **OK**.</span></span> <span data-ttu-id="f4beb-143">(Se você preferir o Visual Basic, alterne para o [versão do Visual Basic](../vb/intro-to-aspnet-mvc-3.md) deste tutorial.)</span><span class="sxs-lookup"><span data-stu-id="f4beb-143">(If you prefer Visual Basic, switch to the [Visual Basic version](../vb/intro-to-aspnet-mvc-3.md) of this tutorial.)</span></span>

![](intro-to-aspnet-mvc-3/_static/image5.png)

<span data-ttu-id="f4beb-144">No **novo projeto do ASP.NET MVC 3** caixa de diálogo, selecione **aplicativo de Internet**.</span><span class="sxs-lookup"><span data-stu-id="f4beb-144">In the **New ASP.NET MVC 3 Project** dialog box, select **Internet Application**.</span></span> <span data-ttu-id="f4beb-145">Verificar **HTML5 Use marcação** e deixe **Razor** como o mecanismo de exibição padrão.</span><span class="sxs-lookup"><span data-stu-id="f4beb-145">Check **Use HTML5 markup** and leave **Razor** as the default view engine.</span></span>

![](intro-to-aspnet-mvc-3/_static/image6.png)

<span data-ttu-id="f4beb-146">Clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="f4beb-146">Click **OK**.</span></span> <span data-ttu-id="f4beb-147">O Visual Web Developer usado um modelo padrão para o projeto ASP.NET MVC que você acabou de criar, para que você tenha um aplicativo em execução no momento sem fazer nada!</span><span class="sxs-lookup"><span data-stu-id="f4beb-147">Visual Web Developer used a default template for the ASP.NET MVC project you just created, so you have a working application right now without doing anything!</span></span> <span data-ttu-id="f4beb-148">Este é um simples "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="f4beb-148">This is a simple "Hello World!"</span></span> <span data-ttu-id="f4beb-149">projeto e ele á um bom lugar para iniciar seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="f4beb-149">project, and it's a good place to start your application.</span></span>

[![](intro-to-aspnet-mvc-3/_static/image8.png)](intro-to-aspnet-mvc-3/_static/image7.png)

<span data-ttu-id="f4beb-150">No menu **Depuração**, selecione **Iniciar Depuração**.</span><span class="sxs-lookup"><span data-stu-id="f4beb-150">From the **Debug** menu, select **Start Debugging**.</span></span>

![](intro-to-aspnet-mvc-3/_static/image9.png)

<span data-ttu-id="f4beb-151">Observe que o atalho de teclado para iniciar a depuração F5.</span><span class="sxs-lookup"><span data-stu-id="f4beb-151">Notice that the keyboard shortcut to start debugging is F5.</span></span>

<span data-ttu-id="f4beb-152">F5 faz com que o Visual Web Developer iniciar um servidor de desenvolvimento web e executar seu aplicativo da web.</span><span class="sxs-lookup"><span data-stu-id="f4beb-152">F5 causes Visual Web Developer to start a development web server and run your web application.</span></span> <span data-ttu-id="f4beb-153">Visual Web Developer inicia um navegador e abre a página inicial do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="f4beb-153">Visual Web Developer then launches a browser and opens the application's home page.</span></span> <span data-ttu-id="f4beb-154">Observe que a barra de endereço do navegador informa `localhost` e não algo como `example.com`.</span><span class="sxs-lookup"><span data-stu-id="f4beb-154">Notice that the address bar of the browser says `localhost` and not something like `example.com`.</span></span> <span data-ttu-id="f4beb-155">Isso ocorre porque `localhost` sempre aponta para o seu próprio computador local, que nesse caso, é executado o aplicativo que você acabou de criar.</span><span class="sxs-lookup"><span data-stu-id="f4beb-155">That's because `localhost` always points to your own local computer, which in this case is running the application you just built.</span></span> <span data-ttu-id="f4beb-156">Quando o Visual Web Developer executa um projeto da web, uma porta aleatória é usada para o servidor web.</span><span class="sxs-lookup"><span data-stu-id="f4beb-156">When Visual Web Developer runs a web project, a random port is used for the web server.</span></span> <span data-ttu-id="f4beb-157">Na imagem abaixo, o número da porta aleatória é 43246.</span><span class="sxs-lookup"><span data-stu-id="f4beb-157">In the image below, the random port number is 43246.</span></span> <span data-ttu-id="f4beb-158">Quando você executa o aplicativo, você provavelmente verá um número de porta diferente.</span><span class="sxs-lookup"><span data-stu-id="f4beb-158">When you run the application, you'll probably see a different port number.</span></span>

![](intro-to-aspnet-mvc-3/_static/image10.png)

<span data-ttu-id="f4beb-159">Imediato desse modelo padrão fornece duas páginas para visitar e uma página de logon básica.</span><span class="sxs-lookup"><span data-stu-id="f4beb-159">Right out of the box this default template gives you two pages to visit and a basic login page.</span></span> <span data-ttu-id="f4beb-160">A próxima etapa é alterar como este aplicativo funciona e aprender um pouco sobre o ASP.NET MVC no processo.</span><span class="sxs-lookup"><span data-stu-id="f4beb-160">The next step is to change how this application works and learn a little bit about ASP.NET MVC in the process.</span></span> <span data-ttu-id="f4beb-161">Feche seu navegador e vamos alterar o código.</span><span class="sxs-lookup"><span data-stu-id="f4beb-161">Close your browser and let's change some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="f4beb-162">Avançar</span><span class="sxs-lookup"><span data-stu-id="f4beb-162">Next</span></span>](adding-a-controller.md)

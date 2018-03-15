---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
title: "Guia de Introdução ao 4.5 Web Forms do ASP.NET e Visual Studio 2013 | Microsoft Docs"
author: Erikre
description: "Esta série de tutoriais passo a passo ensinam os fundamentos da compilação de um aplicativo de Web Forms do ASP.NET usando o ASP.NET 4.5 e o Microsoft Visual Studio Express..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 9b96eaa1-8ef0-4338-a2e8-e0f970bfaf68
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
msc.type: authoredcontent
ms.openlocfilehash: 6ae398f94c0ac3872601bdc8fd935f6d285793db
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/15/2018
---
<a name="getting-started-with-aspnet-45-web-forms-and-visual-studio-2013"></a><span data-ttu-id="7f6df-103">Guia de Introdução ao 4.5 Web Forms do ASP.NET e Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="7f6df-103">Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013</span></span>
====================
<span data-ttu-id="7f6df-104">por [Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="7f6df-104">by [Erik Reitan](https://github.com/Erikre)</span></span>

<span data-ttu-id="7f6df-105">[Baixe o projeto de exemplo do Wingtip Toys (c#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) ou [baixar livro eletrônico (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span><span class="sxs-lookup"><span data-stu-id="7f6df-105">[Download Wingtip Toys Sample Project (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) or [Download E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span></span>

> <span data-ttu-id="7f6df-106">Esta série de tutoriais passo a passo ensinam os fundamentos da compilação de um aplicativo de Web Forms do ASP.NET usando o ASP.NET 4.5 e o Microsoft Visual Studio Express 2013 para Web.</span><span class="sxs-lookup"><span data-stu-id="7f6df-106">This step-by-step tutorial series will teach you the basics of building an ASP.NET Web Forms application using ASP.NET 4.5 and Microsoft Visual Studio Express 2013 for Web.</span></span> [<span data-ttu-id="7f6df-107">Teste de formulários da Web do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="7f6df-107">ASP.NET Web Forms Quiz</span></span>](http://quizapp.cloudapp.net/?quiz=ASP.NET)  
> <span data-ttu-id="7f6df-108">Testar seu conhecimento e reforçar os conceitos-chave executando o teste do ASP.NET Web Forms.</span><span class="sxs-lookup"><span data-stu-id="7f6df-108">Test your knowledge and reinforce key concepts by taking the ASP.NET Web Forms Quiz.</span></span> <span data-ttu-id="7f6df-109">Este teste foi projetado especificamente do conteúdo contido nesta série tutorial.</span><span class="sxs-lookup"><span data-stu-id="7f6df-109">This quiz was specifically designed from content contained in this tutorial series.</span></span> <span data-ttu-id="7f6df-110">Cada questão no teste fornece uma explicação junto com links para diretrizes adicionais.</span><span class="sxs-lookup"><span data-stu-id="7f6df-110">Each question in the quiz provides an explanation along with links to additional guidance.</span></span>


## <a name="introduction"></a><span data-ttu-id="7f6df-111">Introdução</span><span class="sxs-lookup"><span data-stu-id="7f6df-111">Introduction</span></span>

<span data-ttu-id="7f6df-112">Esta série de tutoriais orienta você pelas etapas necessárias para criar um aplicativo de Web Forms do ASP.NET usando o Visual Studio Express 2013 para Web e ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="7f6df-112">This series of tutorials guides you through the steps required to create an ASP.NET Web Forms application using Visual Studio Express 2013 for Web and ASP.NET 4.5.</span></span>

<span data-ttu-id="7f6df-113">O aplicativo que você criará é nomeado **Wingtip Toys**.</span><span class="sxs-lookup"><span data-stu-id="7f6df-113">The application you'll create is named **Wingtip Toys**.</span></span> <span data-ttu-id="7f6df-114">É um exemplo simplificado de um site de repositório front que vende itens online.</span><span class="sxs-lookup"><span data-stu-id="7f6df-114">It's a simplified example of a store front web site that sells items online.</span></span> <span data-ttu-id="7f6df-115">Esta série de tutoriais destaca os novos recursos disponíveis no ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="7f6df-115">This tutorial series highlights new features available in ASP.NET 4.5.</span></span>

<span data-ttu-id="7f6df-116">Comentários são bem-vindos e faremos todos os esforços para atualizar esta série de tutoriais com base em suas sugestões.</span><span class="sxs-lookup"><span data-stu-id="7f6df-116">Comments are welcome, and we'll make every effort to update this tutorial series based on your suggestions.</span></span>

### <a name="download-completed-project"></a><span data-ttu-id="7f6df-117">Projeto de download foi concluída</span><span class="sxs-lookup"><span data-stu-id="7f6df-117">Download completed project</span></span>

<span data-ttu-id="7f6df-118">Você pode baixar um projeto c# que contém o tutorial concluído.</span><span class="sxs-lookup"><span data-stu-id="7f6df-118">You can download a C# project that contains the completed tutorial.</span></span>

- <span data-ttu-id="7f6df-119">[Introdução ao 4.5 Web Forms do ASP.NET e Visual Studio 2013 – Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (c#)</span><span class="sxs-lookup"><span data-stu-id="7f6df-119">[Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span></span>

### <a name="review-the-content-by-taking-the-related-aspnet-web-forms-quiz"></a><span data-ttu-id="7f6df-120">Revise o conteúdo fazendo o teste de Web Forms do ASP.NET relacionado</span><span class="sxs-lookup"><span data-stu-id="7f6df-120">Review the content by taking the related ASP.NET Web Forms quiz</span></span>

<span data-ttu-id="7f6df-121">Depois de concluir este tutorial, testar seu conhecimento e reforçar os principais conceitos colocando o [ASP.NET Web Forms teste](http://quizapp.cloudapp.net/?quiz=ASP.NET&cdn_id=2014-01-17-001).</span><span class="sxs-lookup"><span data-stu-id="7f6df-121">After you complete this tutorial, test your knowledge and reinforce key concepts by taking the [ASP.NET Web Forms Quiz](http://quizapp.cloudapp.net/?quiz=ASP.NET&cdn_id=2014-01-17-001).</span></span> <span data-ttu-id="7f6df-122">Este teste foi projetado especificamente do conteúdo contido nesta série tutorial.</span><span class="sxs-lookup"><span data-stu-id="7f6df-122">This quiz was specifically designed from content contained in this tutorial series.</span></span> <span data-ttu-id="7f6df-123">Cada questão no teste fornece uma explicação junto com links para diretrizes adicionais.</span><span class="sxs-lookup"><span data-stu-id="7f6df-123">Each question in the quiz provides an explanation along with links to additional guidance.</span></span>

- [<span data-ttu-id="7f6df-124">Teste de formulários da Web do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="7f6df-124">ASP.NET Web Forms Quiz</span></span>](http://quizapp.cloudapp.net/?quiz=ASP.NET)

### <a name="audience"></a><span data-ttu-id="7f6df-125">Público-alvo</span><span class="sxs-lookup"><span data-stu-id="7f6df-125">Audience</span></span>

<span data-ttu-id="7f6df-126">O público-alvo da série tutorial é desenvolvedores experientes que forem novos no Web Forms do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="7f6df-126">The intended audience of this tutorial series is experienced developers who are new to ASP.NET Web Forms.</span></span> <span data-ttu-id="7f6df-127">Um desenvolvedor interessado nesta série de tutoriais deve ter as seguintes capacidades:</span><span class="sxs-lookup"><span data-stu-id="7f6df-127">A developer interested in this tutorial series should have the following skills:</span></span>

- <span data-ttu-id="7f6df-128">Familiarizados com um objeto orientado a linguagem de programação (OOP)</span><span class="sxs-lookup"><span data-stu-id="7f6df-128">Familiar with an object oriented programming (OOP) language</span></span>
- <span data-ttu-id="7f6df-129">Familiarizado com conceitos de desenvolvimento da Web (HTML, CSS, JavaScript)</span><span class="sxs-lookup"><span data-stu-id="7f6df-129">Familiar with Web development concepts (HTML, CSS, JavaScript)</span></span>
- <span data-ttu-id="7f6df-130">Conhecer os conceitos de banco de dados relacional</span><span class="sxs-lookup"><span data-stu-id="7f6df-130">Familiar with relational database concepts</span></span>
- <span data-ttu-id="7f6df-131">Conhecer os conceitos de arquitetura de n camadas</span><span class="sxs-lookup"><span data-stu-id="7f6df-131">Familiar with n-tier architecture concepts</span></span>

<span data-ttu-id="7f6df-132">Se você estiver interessado em examinar as áreas listadas acima, considere a possibilidade de revisar o conteúdo a seguir:</span><span class="sxs-lookup"><span data-stu-id="7f6df-132">If you are interested in reviewing the areas listed above, consider reviewing the following content:</span></span>

- [<span data-ttu-id="7f6df-133">Guia de Introdução ao Visual c#</span><span class="sxs-lookup"><span data-stu-id="7f6df-133">Getting Started with Visual C#</span></span>](https://msdn.microsoft.com/library/a72418yk.aspx)
- <span data-ttu-id="7f6df-134">[Desenvolvimento na Web](https://msdn.microsoft.com/beginner/bb308760.aspx), [HTML, CSS, JavaScript, SQL, PHP, JQuery](http://w3schools.com/)</span><span class="sxs-lookup"><span data-stu-id="7f6df-134">[Web Development](https://msdn.microsoft.com/beginner/bb308760.aspx), [HTML, CSS, JavaScript, SQL, PHP, JQuery](http://w3schools.com/)</span></span>
- [<span data-ttu-id="7f6df-135">Banco de dados relacional</span><span class="sxs-lookup"><span data-stu-id="7f6df-135">Relational database</span></span>](http://en.wikipedia.org/wiki/Relational_database)
- [<span data-ttu-id="7f6df-136">Arquitetura de várias camadas</span><span class="sxs-lookup"><span data-stu-id="7f6df-136">Multitier architecture</span></span>](http://en.wikipedia.org/wiki/Multitier_architecture)

### <a name="application-features"></a><span data-ttu-id="7f6df-137">Recursos de aplicativo</span><span class="sxs-lookup"><span data-stu-id="7f6df-137">Application Features</span></span>

<span data-ttu-id="7f6df-138">Os recursos de formulário da Web ASP.NET apresentados nesta série incluem:</span><span class="sxs-lookup"><span data-stu-id="7f6df-138">The ASP.NET Web Form features presented in this series include:</span></span>

- <span data-ttu-id="7f6df-139">O projeto de aplicativo da Web (não o projeto de Site da Web)</span><span class="sxs-lookup"><span data-stu-id="7f6df-139">The Web Application Project (not Web Site Project)</span></span>
- <span data-ttu-id="7f6df-140">Web Forms</span><span class="sxs-lookup"><span data-stu-id="7f6df-140">Web Forms</span></span>
- <span data-ttu-id="7f6df-141">Páginas mestras, configuração</span><span class="sxs-lookup"><span data-stu-id="7f6df-141">Master Pages, Configuration</span></span>
- <span data-ttu-id="7f6df-142">inicialização</span><span class="sxs-lookup"><span data-stu-id="7f6df-142">Bootstrap</span></span>
- <span data-ttu-id="7f6df-143">Entity Framework Code primeiro, o LocalDB</span><span class="sxs-lookup"><span data-stu-id="7f6df-143">Entity Framework Code First, LocalDB</span></span>
- <span data-ttu-id="7f6df-144">Validação de solicitação</span><span class="sxs-lookup"><span data-stu-id="7f6df-144">Request Validation</span></span>
- <span data-ttu-id="7f6df-145">Fortemente tipado a controles de dados, modelo de associação, as anotações de dados e provedores de valor</span><span class="sxs-lookup"><span data-stu-id="7f6df-145">Strongly Typed Data Controls, Model Binding, Data Annotations, and Value Providers</span></span>
- <span data-ttu-id="7f6df-146">SSL e OAuth</span><span class="sxs-lookup"><span data-stu-id="7f6df-146">SSL and OAuth</span></span>
- <span data-ttu-id="7f6df-147">Identidade do ASP.NET, configuração e autorização</span><span class="sxs-lookup"><span data-stu-id="7f6df-147">ASP.NET Identity, Configuration, and Authorization</span></span>
- <span data-ttu-id="7f6df-148">Validação discreta</span><span class="sxs-lookup"><span data-stu-id="7f6df-148">Unobtrusive Validation</span></span>
- <span data-ttu-id="7f6df-149">Roteamento</span><span class="sxs-lookup"><span data-stu-id="7f6df-149">Routing</span></span>
- <span data-ttu-id="7f6df-150">Tratamento de erros do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="7f6df-150">ASP.NET Error Handling</span></span>

### <a name="application-scenarios-and-tasks"></a><span data-ttu-id="7f6df-151">Tarefas e cenários de aplicativos</span><span class="sxs-lookup"><span data-stu-id="7f6df-151">Application Scenarios and Tasks</span></span>

<span data-ttu-id="7f6df-152">Tarefas demonstradas nesta série:</span><span class="sxs-lookup"><span data-stu-id="7f6df-152">Tasks demonstrated in this series include:</span></span>

- <span data-ttu-id="7f6df-153">Criando, revisando e executando o novo projeto</span><span class="sxs-lookup"><span data-stu-id="7f6df-153">Creating, reviewing and running the new project</span></span>
- <span data-ttu-id="7f6df-154">Criando a estrutura de banco de dados</span><span class="sxs-lookup"><span data-stu-id="7f6df-154">Creating the database structure</span></span>
- <span data-ttu-id="7f6df-155">Inicializando e a propagação do banco de dados</span><span class="sxs-lookup"><span data-stu-id="7f6df-155">Initializing and seeding the database</span></span>
- <span data-ttu-id="7f6df-156">Personalizando a interface do usuário usando estilos, gráficos e uma página mestra</span><span class="sxs-lookup"><span data-stu-id="7f6df-156">Customizing the UI using styles, graphics and a master page</span></span>
- <span data-ttu-id="7f6df-157">Adição de páginas e navegação</span><span class="sxs-lookup"><span data-stu-id="7f6df-157">Adding pages and navigation</span></span>
- <span data-ttu-id="7f6df-158">Exibindo detalhes do menu e os dados de produto</span><span class="sxs-lookup"><span data-stu-id="7f6df-158">Displaying menu details and product data</span></span>
- <span data-ttu-id="7f6df-159">Criando um carrinho de compras</span><span class="sxs-lookup"><span data-stu-id="7f6df-159">Creating a shopping cart</span></span>
- <span data-ttu-id="7f6df-160">Suporte a SSL adicionando e OAuth</span><span class="sxs-lookup"><span data-stu-id="7f6df-160">Adding SSL and OAuth support</span></span>
- <span data-ttu-id="7f6df-161">Adicionando um método de pagamento</span><span class="sxs-lookup"><span data-stu-id="7f6df-161">Adding a payment method</span></span>
- <span data-ttu-id="7f6df-162">Incluindo uma função de administrador e um usuário para o aplicativo</span><span class="sxs-lookup"><span data-stu-id="7f6df-162">Including an administrator role and a user to the application</span></span>
- <span data-ttu-id="7f6df-163">Restringir o acesso a páginas específicas e pasta</span><span class="sxs-lookup"><span data-stu-id="7f6df-163">Restricting access to specific pages and folder</span></span>
- <span data-ttu-id="7f6df-164">Carregando um arquivo para o aplicativo web</span><span class="sxs-lookup"><span data-stu-id="7f6df-164">Uploading a file to the web application</span></span>
- <span data-ttu-id="7f6df-165">Implementação de validação de entrada</span><span class="sxs-lookup"><span data-stu-id="7f6df-165">Implementing input validation</span></span>
- <span data-ttu-id="7f6df-166">Registrando as rotas para o aplicativo web</span><span class="sxs-lookup"><span data-stu-id="7f6df-166">Registering routes for the web application</span></span>
- <span data-ttu-id="7f6df-167">Implementando o tratamento de erros e log de erros</span><span class="sxs-lookup"><span data-stu-id="7f6df-167">Implementing error handling and error logging</span></span>

## <a name="overview"></a><span data-ttu-id="7f6df-168">Visão geral</span><span class="sxs-lookup"><span data-stu-id="7f6df-168">Overview</span></span>

<span data-ttu-id="7f6df-169">Se você for novo no Web Forms do ASP.NET mas tem familiaridade com conceitos de programação, você tem o direito tutorial.</span><span class="sxs-lookup"><span data-stu-id="7f6df-169">If you are new to ASP.NET Web Forms but have familiarity with programming concepts, you have the right tutorial.</span></span> <span data-ttu-id="7f6df-170">Se você já estiver familiarizado com o Web Forms do ASP.NET, você pode aproveitar essa série de tutoriais pelos novos recursos disponíveis no ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="7f6df-170">If you are already familiar with ASP.NET Web Forms, you can benefit from this tutorial series by the new features available in ASP.NET 4.5.</span></span> <span data-ttu-id="7f6df-171">Se você estiver familiarizado com conceitos de programação e Web Forms do ASP.NET, consulte os tutoriais adicionais fornecidos no Web Forms [Introdução](../../../index.md) seção no site da Web do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="7f6df-171">If you are unfamiliar with programming concepts and ASP.NET Web Forms, see the additional tutorials provided in the Web Forms [Getting Started](../../../index.md) section on the ASP.NET Web site.</span></span>

<span data-ttu-id="7f6df-172">Específico **mais recente** ASP.NET 4.5 recursos fornecido nesta Web Forms série de tutoriais incluem o seguinte:</span><span class="sxs-lookup"><span data-stu-id="7f6df-172">The specific **latest** ASP.NET 4.5 features provided in this Web Forms tutorial series include the following:</span></span>

- <span data-ttu-id="7f6df-173">Uma interface do usuário simple para a criação de projetos que oferecem [suporte para várias estruturas ASP.NET](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (Web Forms, MVC e API da Web).</span><span class="sxs-lookup"><span data-stu-id="7f6df-173">A simple UI for creating projects that offer [support for multiple ASP.NET frameworks](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (Web Forms, MVC, and Web API).</span></span>
- <span data-ttu-id="7f6df-174">[Inicialização](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), uma estrutura de layout e temas que fornece recursos de design e temas responsivos.</span><span class="sxs-lookup"><span data-stu-id="7f6df-174">[Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), a layout and theming framework that provides responsive design and theming capabilities.</span></span>
- <span data-ttu-id="7f6df-175">[Identidade do ASP.NET](../../../../identity/index.md), um novo sistema de associação do ASP.NET que funciona da mesma em todas as estruturas do ASP.NET e funciona com o software que não seja o IIS de hospedagem na web.</span><span class="sxs-lookup"><span data-stu-id="7f6df-175">[ASP.NET Identity](../../../../identity/index.md), a new ASP.NET membership system that works the same in all ASP.NET frameworks and works with web hosting software other than IIS.</span></span>
- <span data-ttu-id="7f6df-176">[Entity Framework 6](https://msdn.microsoft.com/data/ef.aspx), objetos de uma atualização para o Entity Framework que permite recuperar e manipular dados como fortemente tipados, acessar os dados de forma assíncrona, tratar falhas transitórias de conexão e instruções SQL de log.</span><span class="sxs-lookup"><span data-stu-id="7f6df-176">[Entity Framework 6](https://msdn.microsoft.com/data/ef.aspx), an update to the Entity Framework which allows you retrieve and manipulate data as strongly typed objects, access data asynchronously, handle transient connection faults, and log SQL statements.</span></span>

<span data-ttu-id="7f6df-177">Para obter uma lista completa dos recursos do ASP.NET 4.5, consulte [ASP.NET e Web Tools para notas de versão do Visual Studio 2013](../../../../visual-studio/overview/2013/release-notes.md).</span><span class="sxs-lookup"><span data-stu-id="7f6df-177">For a complete list of ASP.NET 4.5 features, see [ASP.NET and Web Tools for Visual Studio 2013 Release Notes](../../../../visual-studio/overview/2013/release-notes.md).</span></span>

### <a name="the-wingtip-toys-sample-application"></a><span data-ttu-id="7f6df-178">O aplicativo de exemplo do Wingtip Toys</span><span class="sxs-lookup"><span data-stu-id="7f6df-178">The Wingtip Toys Sample Application</span></span>

<span data-ttu-id="7f6df-179">As capturas de tela a seguir fornecem uma visão rápida do aplicativo de formulários da Web do ASP.NET que você criará nesta série tutorial.</span><span class="sxs-lookup"><span data-stu-id="7f6df-179">The following screen shots provide a quick view of the ASP.NET Web forms application that you will create in this tutorial series.</span></span> <span data-ttu-id="7f6df-180">Quando você executa o aplicativo do Visual Studio Express 2013 para Web, você verá a página inicial.</span><span class="sxs-lookup"><span data-stu-id="7f6df-180">When you run the application from Visual Studio Express 2013 for Web, you will see the following web Home page.</span></span>

![Wingtip Toys - página padrão](introduction-and-overview/_static/image1.png)

<span data-ttu-id="7f6df-182">Você pode registrar como um novo usuário, ou faça logon como um usuário existente.</span><span class="sxs-lookup"><span data-stu-id="7f6df-182">You can register as a new user, or log in as an existing user.</span></span> <span data-ttu-id="7f6df-183">Navegação é fornecida na parte superior de cada categoria de produto, recuperando os produtos disponíveis do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="7f6df-183">Navigation is provided at the top for each product category by retrieving the available products from the database.</span></span>

<span data-ttu-id="7f6df-184">Selecionando o link de produtos, você poderá ver uma lista de todos os produtos disponíveis.</span><span class="sxs-lookup"><span data-stu-id="7f6df-184">By selecting the Products link, you will be able to see a list of all available products.</span></span>

![Wingtip Toys - produtos](introduction-and-overview/_static/image2.png)

<span data-ttu-id="7f6df-186">Você também pode ver detalhes do produto individual selecionando qualquer um dos produtos listados.</span><span class="sxs-lookup"><span data-stu-id="7f6df-186">You can also see individual product details by selecting any of the listed products.</span></span>

![Wingtip Toys - detalhes do produto](introduction-and-overview/_static/image3.png)

<span data-ttu-id="7f6df-188">Como um usuário, você pode registrar e faça logon usando a funcionalidade padrão do modelo de Web Forms.</span><span class="sxs-lookup"><span data-stu-id="7f6df-188">As a user, you can register and log in using the default functionality of the Web Forms template.</span></span> <span data-ttu-id="7f6df-189">Este tutorial também explica como fazer logon usando uma conta existente do Gmail.</span><span class="sxs-lookup"><span data-stu-id="7f6df-189">This tutorial also explains how to login using an existing Gmail account.</span></span> <span data-ttu-id="7f6df-190">Além disso, você pode fazer logon como administrador para adicionar e remover produtos do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="7f6df-190">Additionally, you can login as the administrator to add and remove products from the database.</span></span>

![Wingtip Toys - login](introduction-and-overview/_static/image4.png)

<span data-ttu-id="7f6df-192">Depois de fazer logon como um usuário, você pode adicionar produtos ao carrinho de compras e check-out com o PayPal.</span><span class="sxs-lookup"><span data-stu-id="7f6df-192">Once you have logged in as a user, you can add products to the shopping cart and checkout with PayPal.</span></span> <span data-ttu-id="7f6df-193">Observe que este aplicativo de exemplo é projetado para funcionar com a área restrita do desenvolvedor do PayPal.</span><span class="sxs-lookup"><span data-stu-id="7f6df-193">Note that this sample application is designed to function with PayPal's developer sandbox.</span></span> <span data-ttu-id="7f6df-194">Nenhuma transação de dinheiro real ocorrerá.</span><span class="sxs-lookup"><span data-stu-id="7f6df-194">No actual money transaction will take place.</span></span>

![Wingtip Toys - carrinho de compras](introduction-and-overview/_static/image5.png)

<span data-ttu-id="7f6df-196">PayPal confirmar sua conta, a ordem e a informações de pagamento.</span><span class="sxs-lookup"><span data-stu-id="7f6df-196">PayPal will confirm your account, order, and payment information.</span></span>

![Wingtip Toys - PayPal](introduction-and-overview/_static/image6.png)

<span data-ttu-id="7f6df-198">Após o retorno do PayPal, você pode revisar e concluir o pedido.</span><span class="sxs-lookup"><span data-stu-id="7f6df-198">After returning from PayPal, you can review and complete your order.</span></span>

![Wingtip Toys - revisão de ordem](introduction-and-overview/_static/image7.png)

## <a name="prerequisites"></a><span data-ttu-id="7f6df-200">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="7f6df-200">Prerequisites</span></span>

<span data-ttu-id="7f6df-201">Antes de começar, certifique-se de que você tenha o seguinte software instalado em seu computador:</span><span class="sxs-lookup"><span data-stu-id="7f6df-201">Before you start, make sure that you have the following software installed on your computer:</span></span>

- <span data-ttu-id="7f6df-202">[Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) ou [Microsoft Visual Studio Express 2013 para Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span><span class="sxs-lookup"><span data-stu-id="7f6df-202">[Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) or [Microsoft Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span></span> <span data-ttu-id="7f6df-203">O .NET Framework é instalado automaticamente.</span><span class="sxs-lookup"><span data-stu-id="7f6df-203">The .NET Framework is installed automatically.</span></span>

<span data-ttu-id="7f6df-204">Esta série de tutoriais usa o Microsoft Visual Studio Express 2013 para Web.</span><span class="sxs-lookup"><span data-stu-id="7f6df-204">This tutorial series uses Microsoft Visual Studio Express 2013 for Web.</span></span> <span data-ttu-id="7f6df-205">Você pode usar o Microsoft Visual Studio Express 2013 para Web ou Microsoft Visual Studio 2013 para concluir este tutorial série.</span><span class="sxs-lookup"><span data-stu-id="7f6df-205">You can use either Microsoft Visual Studio Express 2013 for Web or Microsoft Visual Studio 2013 to complete this tutorial series.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="7f6df-206">Microsoft Visual Studio 2013 e Microsoft Visual Studio Express 2013 para Web serão ser conhecida como Visual Studio em toda a série de tutoriais.</span><span class="sxs-lookup"><span data-stu-id="7f6df-206">Microsoft Visual Studio 2013 and Microsoft Visual Studio Express 2013 for Web will often be referred to as Visual Studio throughout this tutorial series.</span></span>


<span data-ttu-id="7f6df-207">Se você já tiver uma versão do Visual Studio instalada, o processo de instalação irá instalar Visual Studio 2013 ou o Microsoft Visual Studio Express 2013 para Web próximo à versão existente.</span><span class="sxs-lookup"><span data-stu-id="7f6df-207">If you already have a Visual Studio version installed, the installation process will install Visual Studio 2013 or Microsoft Visual Studio Express 2013 for Web next to the existing version.</span></span> <span data-ttu-id="7f6df-208">Sites criados em versões anteriores podem ser abertos no Visual Studio 2013 e continuam a abrir em versões anteriores.</span><span class="sxs-lookup"><span data-stu-id="7f6df-208">Sites that you created in earlier versions can be opened in Visual Studio 2013 and continue to open in previous versions.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="7f6df-209">Este passo a passo pressupõe que você selecionou o *desenvolvimento Web* conjunto de configurações na primeira vez que você iniciar o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7f6df-209">This walkthrough assumes that you selected the *Web Development* collection of settings the first time that you started Visual Studio.</span></span> <span data-ttu-id="7f6df-210">Para obter mais informações, consulte [como: selecionar configurações de ambiente de desenvolvimento da Web](https://msdn.microsoft.com/library/ff521558.aspx).</span><span class="sxs-lookup"><span data-stu-id="7f6df-210">For more information, see [How to: Select Web Development Environment Settings](https://msdn.microsoft.com/library/ff521558.aspx).</span></span>


## <a name="download-the-sample-application"></a><span data-ttu-id="7f6df-211">Baixe o aplicativo de exemplo</span><span class="sxs-lookup"><span data-stu-id="7f6df-211">Download the Sample Application</span></span>

<span data-ttu-id="7f6df-212">Depois de instalar os pré-requisitos, você está pronto para começar a criar o novo projeto da Web que é apresentado nesta série tutorial.</span><span class="sxs-lookup"><span data-stu-id="7f6df-212">After installing the prerequisites, you are ready to begin creating the new Web project that is presented in this tutorial series.</span></span> <span data-ttu-id="7f6df-213">Se você quiser **opcionalmente** executar o aplicativo de exemplo que cria esta série de tutoriais, você pode baixá-lo do site de exemplos do MSDN.</span><span class="sxs-lookup"><span data-stu-id="7f6df-213">If you would like to **optionally** run the sample application that this tutorial series creates, you can download it from the MSDN Samples site.</span></span> <span data-ttu-id="7f6df-214">Este download contém o seguinte:</span><span class="sxs-lookup"><span data-stu-id="7f6df-214">This download contains the following:</span></span>

- <span data-ttu-id="7f6df-215">O aplicativo de exemplo no *WingtipToys* pasta.</span><span class="sxs-lookup"><span data-stu-id="7f6df-215">The sample application in the *WingtipToys* folder.</span></span>
- <span data-ttu-id="7f6df-216">Os recursos usados para criar o aplicativo de exemplo no *WingtipToys ativos* pasta o *WingtipToys* pasta.</span><span class="sxs-lookup"><span data-stu-id="7f6df-216">The resources used to create the sample application in the *WingtipToys-Assets* folder in the *WingtipToys* folder.</span></span>

#### <a name="download-the-file-from-msdn-samples-site"></a><span data-ttu-id="7f6df-217">Baixe o arquivo do site de exemplos do MSDN:</span><span class="sxs-lookup"><span data-stu-id="7f6df-217">Download the file from MSDN Samples site:</span></span>

<span data-ttu-id="7f6df-218">[Introdução ao 4.5 Web Forms do ASP.NET e Visual Studio 2013 – Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (c#)</span><span class="sxs-lookup"><span data-stu-id="7f6df-218">[Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span></span> 

<span data-ttu-id="7f6df-219">O download é um *. zip* arquivo.</span><span class="sxs-lookup"><span data-stu-id="7f6df-219">The download is a *.zip* file.</span></span> <span data-ttu-id="7f6df-220">Para ver o projeto completo que cria esta série de tutoriais, encontre e selecione o *c#*pasta o *. zip* arquivo.</span><span class="sxs-lookup"><span data-stu-id="7f6df-220">To see the completed project that this tutorial series creates, find and select the *C#*folder in the *.zip* file.</span></span> <span data-ttu-id="7f6df-221">Salve o *c#* folderto a pasta que você pode usar para trabalhar com projetos do Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="7f6df-221">Save the *C#* folderto the folder you use to work with Visual Studio 2013 projects.</span></span> <span data-ttu-id="7f6df-222">Por padrão, a pasta de projetos do Visual Studio 2013 é o seguinte:</span><span class="sxs-lookup"><span data-stu-id="7f6df-222">By default, the Visual Studio 2013 projects folder is the following:</span></span>

<span data-ttu-id="7f6df-223">**C:\Users\*****&lt;username&gt;* \Documents\Visual 2013\Projects Studio**</span><span class="sxs-lookup"><span data-stu-id="7f6df-223">**C:\Users\*****&lt;username&gt;*****\Documents\Visual Studio 2013\Projects**</span></span>

<span data-ttu-id="7f6df-224">Renomear o ***c#*** pasta ***WingtipToys***.</span><span class="sxs-lookup"><span data-stu-id="7f6df-224">Rename the ***C#*** folder to ***WingtipToys***.</span></span>

> [!NOTE]
> <span data-ttu-id="7f6df-225">Se você já tiver uma pasta chamada *WingtipToys* na sua pasta de projetos, renomeie temporariamente essa pasta existente antes de renomear o *c#* pasta *WingtipToys*.</span><span class="sxs-lookup"><span data-stu-id="7f6df-225">If you already have a folder named *WingtipToys* in your Projects folder, temporarily rename that existing folder before renaming the *C#* folder to *WingtipToys*.</span></span>


<span data-ttu-id="7f6df-226">Para executar o projeto concluído, abra o *WingtipToys* pasta e clique duas vezes o *WingtipToys.sln* arquivo.</span><span class="sxs-lookup"><span data-stu-id="7f6df-226">To run the completed project, open the *WingtipToys* folder and double-click the *WingtipToys.sln* file.</span></span> <span data-ttu-id="7f6df-227">Visual Studio 2013 abrirá o projeto.</span><span class="sxs-lookup"><span data-stu-id="7f6df-227">Visual Studio 2013 will open the project.</span></span> <span data-ttu-id="7f6df-228">Em seguida, clique com botão direito do *Default.aspx* arquivo na janela do Gerenciador de soluções e clique em Exibir no navegador, no menu de atalho.</span><span class="sxs-lookup"><span data-stu-id="7f6df-228">Next, right-click the *Default.aspx* file in the Solution Explorer window and click View In Browser from the right-click menu.</span></span>

### <a name="tutorial-support-and-comments"></a><span data-ttu-id="7f6df-229">Comentários e suporte de tutorial</span><span class="sxs-lookup"><span data-stu-id="7f6df-229">Tutorial Support and Comments</span></span>

<span data-ttu-id="7f6df-230">Use a seção de p e r incluída com o [Introdução ao Web Forms do ASP.NET 4.5 e o Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) exemplo (c#) para qualquer pergunta ou comentário.</span><span class="sxs-lookup"><span data-stu-id="7f6df-230">Use the Q AND A section included with the [Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) sample for any questions or comments.</span></span>

<span data-ttu-id="7f6df-231">Comentários sobre esta série de tutoriais são bem-vindos e quando esta série de tutoriais é atualizado todos os esforços serão feito para correções de conta ou sugestões de melhorias que são fornecidas nos comentários tutoriais.</span><span class="sxs-lookup"><span data-stu-id="7f6df-231">Comments on this tutorial series are welcome, and when this tutorial series is updated every effort will be made to take into account corrections or suggestions for improvements that are provided in the tutorial comments.</span></span>

<span data-ttu-id="7f6df-232">Quando ocorrer um erro durante o desenvolvimento, ou se o site da Web não funcionar corretamente, as mensagens de erro podem fornecer pistas complexas para a origem do problema ou não podem explicar como corrigi-lo.</span><span class="sxs-lookup"><span data-stu-id="7f6df-232">When an error happens during development, or if the Web site does not run correctly, the error messages may give complex clues to the source of the problem or might not explain how to fix it.</span></span> <span data-ttu-id="7f6df-233">Para ajudá-lo com alguns cenários comuns de problema, você também pode usar o [ASP.NET fóruns](https://forums.asp.net/) ou na seção de p e r incluídas com o [Introdução ao Web Forms do ASP.NET 4.5 e o Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) ( C#) exemplo.</span><span class="sxs-lookup"><span data-stu-id="7f6df-233">To help you with some common problem scenarios, you can also use the [ASP.NET forums](https://forums.asp.net/) or the Q AND A section included with the [Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) sample.</span></span> <span data-ttu-id="7f6df-234">Se você receber uma mensagem de erro ou algo não funciona ao percorrer os tutoriais, certifique-se de verificar os locais acima.</span><span class="sxs-lookup"><span data-stu-id="7f6df-234">If you get an error message or something doesn't work as you go through the tutorials, be sure to check the above locations.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="7f6df-235">Avançar</span><span class="sxs-lookup"><span data-stu-id="7f6df-235">Next</span></span>](create-the-project.md)

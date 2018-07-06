---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
title: Introdução ao ASP.NET 4.5 do Web Forms e Visual Studio 2013 | Microsoft Docs
author: Erikre
description: Esta série de tutoriais passo a passo ensina as Noções básicas de criação de um aplicativo de Web Forms do ASP.NET usando o ASP.NET 4.5 e o Microsoft Visual Studio Express...
ms.author: aspnetcontent
ms.date: 09/08/2014
ms.assetid: 9b96eaa1-8ef0-4338-a2e8-e0f970bfaf68
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
msc.type: authoredcontent
ms.openlocfilehash: cda0c6239e2f10186641ab315837440d83b2fda6
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37841390"
---
<a name="getting-started-with-aspnet-45-web-forms-and-visual-studio-2013"></a><span data-ttu-id="34400-103">Introdução ao ASP.NET 4.5 do Web Forms e Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="34400-103">Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013</span></span>
====================
<span data-ttu-id="34400-104">por [Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="34400-104">by [Erik Reitan](https://github.com/Erikre)</span></span>

<span data-ttu-id="34400-105">[Baixe o projeto de exemplo do Wingtip Toys (c#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) ou [Baixe o livro eletrônico (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span><span class="sxs-lookup"><span data-stu-id="34400-105">[Download Wingtip Toys Sample Project (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) or [Download E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span></span>

> <span data-ttu-id="34400-106">Esta série de tutoriais passo a passo ensina os conceitos básicos da criação de um aplicativo de Web Forms do ASP.NET usando o ASP.NET 4.5 e do Microsoft Visual Studio Express 2013 para Web.</span><span class="sxs-lookup"><span data-stu-id="34400-106">This step-by-step tutorial series will teach you the basics of building an ASP.NET Web Forms application using ASP.NET 4.5 and Microsoft Visual Studio Express 2013 for Web.</span></span> [<span data-ttu-id="34400-107">ASP.NET Web Forms do teste</span><span class="sxs-lookup"><span data-stu-id="34400-107">ASP.NET Web Forms Quiz</span></span>](http://quizapp.cloudapp.net/?quiz=ASP.NET)  
> <span data-ttu-id="34400-108">Teste seu conhecimento e reforçar os conceitos-chave executando o teste do ASP.NET Web Forms.</span><span class="sxs-lookup"><span data-stu-id="34400-108">Test your knowledge and reinforce key concepts by taking the ASP.NET Web Forms Quiz.</span></span> <span data-ttu-id="34400-109">Este teste foi desenvolvido especificamente desde o conteúdo contido nessa série de tutoriais.</span><span class="sxs-lookup"><span data-stu-id="34400-109">This quiz was specifically designed from content contained in this tutorial series.</span></span> <span data-ttu-id="34400-110">Cada pergunta em teste fornece uma explicação juntamente com links para orientações adicionais.</span><span class="sxs-lookup"><span data-stu-id="34400-110">Each question in the quiz provides an explanation along with links to additional guidance.</span></span>


## <a name="introduction"></a><span data-ttu-id="34400-111">Introdução</span><span class="sxs-lookup"><span data-stu-id="34400-111">Introduction</span></span>

<span data-ttu-id="34400-112">Esta série de tutoriais orienta você pelas etapas necessárias para criar um aplicativo Web Forms do ASP.NET usando o Visual Studio Express 2013 para Web e ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="34400-112">This series of tutorials guides you through the steps required to create an ASP.NET Web Forms application using Visual Studio Express 2013 for Web and ASP.NET 4.5.</span></span>

<span data-ttu-id="34400-113">O aplicativo, você cria é denominado **Wingtip Toys**.</span><span class="sxs-lookup"><span data-stu-id="34400-113">The application you'll create is named **Wingtip Toys**.</span></span> <span data-ttu-id="34400-114">É um exemplo simplificado de um site de armazenamento de front-que vende itens online.</span><span class="sxs-lookup"><span data-stu-id="34400-114">It's a simplified example of a store front web site that sells items online.</span></span> <span data-ttu-id="34400-115">Esta série de tutoriais destaca os novos recursos disponíveis no ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="34400-115">This tutorial series highlights new features available in ASP.NET 4.5.</span></span>

<span data-ttu-id="34400-116">Comentários são bem-vindos e podemos fazer todos os esforços para atualizar esta série de tutoriais com base em suas sugestões.</span><span class="sxs-lookup"><span data-stu-id="34400-116">Comments are welcome, and we'll make every effort to update this tutorial series based on your suggestions.</span></span>

### <a name="download-completed-project"></a><span data-ttu-id="34400-117">Projeto de download concluído</span><span class="sxs-lookup"><span data-stu-id="34400-117">Download completed project</span></span>

<span data-ttu-id="34400-118">Você pode baixar um projeto c# que contém o tutorial concluído.</span><span class="sxs-lookup"><span data-stu-id="34400-118">You can download a C# project that contains the completed tutorial.</span></span>

- <span data-ttu-id="34400-119">[Introdução ao ASP.NET 4.5 do Web Forms e Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (c#)</span><span class="sxs-lookup"><span data-stu-id="34400-119">[Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span></span>

### <a name="review-the-content-by-taking-the-related-aspnet-web-forms-quiz"></a><span data-ttu-id="34400-120">Revise o conteúdo, fazendo o teste de Web Forms do ASP.NET relacionado</span><span class="sxs-lookup"><span data-stu-id="34400-120">Review the content by taking the related ASP.NET Web Forms quiz</span></span>

<span data-ttu-id="34400-121">Depois de concluir este tutorial, teste seu conhecimento e reforçar os conceitos-chave por meio de [teste do ASP.NET Web Forms](http://quizapp.cloudapp.net/?quiz=ASP.NET&cdn_id=2014-01-17-001).</span><span class="sxs-lookup"><span data-stu-id="34400-121">After you complete this tutorial, test your knowledge and reinforce key concepts by taking the [ASP.NET Web Forms Quiz](http://quizapp.cloudapp.net/?quiz=ASP.NET&cdn_id=2014-01-17-001).</span></span> <span data-ttu-id="34400-122">Este teste foi desenvolvido especificamente desde o conteúdo contido nessa série de tutoriais.</span><span class="sxs-lookup"><span data-stu-id="34400-122">This quiz was specifically designed from content contained in this tutorial series.</span></span> <span data-ttu-id="34400-123">Cada pergunta em teste fornece uma explicação juntamente com links para orientações adicionais.</span><span class="sxs-lookup"><span data-stu-id="34400-123">Each question in the quiz provides an explanation along with links to additional guidance.</span></span>

- [<span data-ttu-id="34400-124">ASP.NET Web Forms do teste</span><span class="sxs-lookup"><span data-stu-id="34400-124">ASP.NET Web Forms Quiz</span></span>](http://quizapp.cloudapp.net/?quiz=ASP.NET)

### <a name="audience"></a><span data-ttu-id="34400-125">Público-alvo</span><span class="sxs-lookup"><span data-stu-id="34400-125">Audience</span></span>

<span data-ttu-id="34400-126">O público-alvo desta série de tutoriais é os desenvolvedores experientes novatos no Web Forms do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="34400-126">The intended audience of this tutorial series is experienced developers who are new to ASP.NET Web Forms.</span></span> <span data-ttu-id="34400-127">Um desenvolvedor interessado nessa série de tutoriais deve ter as seguintes capacidades:</span><span class="sxs-lookup"><span data-stu-id="34400-127">A developer interested in this tutorial series should have the following skills:</span></span>

- <span data-ttu-id="34400-128">Esteja familiarizado com um objeto orientado a linguagem de programação (OOP)</span><span class="sxs-lookup"><span data-stu-id="34400-128">Familiar with an object oriented programming (OOP) language</span></span>
- <span data-ttu-id="34400-129">Esteja familiarizado com conceitos de desenvolvimento da Web (HTML, CSS e JavaScript)</span><span class="sxs-lookup"><span data-stu-id="34400-129">Familiar with Web development concepts (HTML, CSS, JavaScript)</span></span>
- <span data-ttu-id="34400-130">Conheça os conceitos de banco de dados relacional</span><span class="sxs-lookup"><span data-stu-id="34400-130">Familiar with relational database concepts</span></span>
- <span data-ttu-id="34400-131">Conheça os conceitos de arquitetura de n camadas</span><span class="sxs-lookup"><span data-stu-id="34400-131">Familiar with n-tier architecture concepts</span></span>

<span data-ttu-id="34400-132">Se você estiver interessado em analisar as áreas listadas acima, considere a possibilidade de revisar o conteúdo a seguir:</span><span class="sxs-lookup"><span data-stu-id="34400-132">If you are interested in reviewing the areas listed above, consider reviewing the following content:</span></span>

- [<span data-ttu-id="34400-133">Introdução ao Visual c#</span><span class="sxs-lookup"><span data-stu-id="34400-133">Getting Started with Visual C#</span></span>](https://msdn.microsoft.com/library/a72418yk.aspx)
- <span data-ttu-id="34400-134">[Desenvolvimento na Web](https://msdn.microsoft.com/beginner/bb308760.aspx), [HTML, CSS, JavaScript, SQL, PHP, JQuery](http://w3schools.com/)</span><span class="sxs-lookup"><span data-stu-id="34400-134">[Web Development](https://msdn.microsoft.com/beginner/bb308760.aspx), [HTML, CSS, JavaScript, SQL, PHP, JQuery](http://w3schools.com/)</span></span>
- [<span data-ttu-id="34400-135">Banco de dados relacional</span><span class="sxs-lookup"><span data-stu-id="34400-135">Relational database</span></span>](http://en.wikipedia.org/wiki/Relational_database)
- [<span data-ttu-id="34400-136">Arquitetura de várias camadas</span><span class="sxs-lookup"><span data-stu-id="34400-136">Multitier architecture</span></span>](http://en.wikipedia.org/wiki/Multitier_architecture)

### <a name="application-features"></a><span data-ttu-id="34400-137">Recursos do aplicativo</span><span class="sxs-lookup"><span data-stu-id="34400-137">Application Features</span></span>

<span data-ttu-id="34400-138">Os recursos do Web Form do ASP.NET apresentados nesta série incluem:</span><span class="sxs-lookup"><span data-stu-id="34400-138">The ASP.NET Web Form features presented in this series include:</span></span>

- <span data-ttu-id="34400-139">O projeto de aplicativo Web (não o projeto de Site da Web)</span><span class="sxs-lookup"><span data-stu-id="34400-139">The Web Application Project (not Web Site Project)</span></span>
- <span data-ttu-id="34400-140">Web Forms</span><span class="sxs-lookup"><span data-stu-id="34400-140">Web Forms</span></span>
- <span data-ttu-id="34400-141">Páginas mestras, configuração</span><span class="sxs-lookup"><span data-stu-id="34400-141">Master Pages, Configuration</span></span>
- <span data-ttu-id="34400-142">Bootstrap</span><span class="sxs-lookup"><span data-stu-id="34400-142">Bootstrap</span></span>
- <span data-ttu-id="34400-143">Entity Framework Code First, LocalDB</span><span class="sxs-lookup"><span data-stu-id="34400-143">Entity Framework Code First, LocalDB</span></span>
- <span data-ttu-id="34400-144">Validação de solicitação</span><span class="sxs-lookup"><span data-stu-id="34400-144">Request Validation</span></span>
- <span data-ttu-id="34400-145">Fortemente tipado a controles de dados, da associação, anotações de dados de modelo e provedores de valor</span><span class="sxs-lookup"><span data-stu-id="34400-145">Strongly Typed Data Controls, Model Binding, Data Annotations, and Value Providers</span></span>
- <span data-ttu-id="34400-146">SSL e OAuth</span><span class="sxs-lookup"><span data-stu-id="34400-146">SSL and OAuth</span></span>
- <span data-ttu-id="34400-147">O ASP.NET Identity, configuração e autorização</span><span class="sxs-lookup"><span data-stu-id="34400-147">ASP.NET Identity, Configuration, and Authorization</span></span>
- <span data-ttu-id="34400-148">Validação não invasiva</span><span class="sxs-lookup"><span data-stu-id="34400-148">Unobtrusive Validation</span></span>
- <span data-ttu-id="34400-149">Roteamento</span><span class="sxs-lookup"><span data-stu-id="34400-149">Routing</span></span>
- <span data-ttu-id="34400-150">Tratamento de erros do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="34400-150">ASP.NET Error Handling</span></span>

### <a name="application-scenarios-and-tasks"></a><span data-ttu-id="34400-151">Tarefas e cenários de aplicativo</span><span class="sxs-lookup"><span data-stu-id="34400-151">Application Scenarios and Tasks</span></span>

<span data-ttu-id="34400-152">Demonstrado nesta série de tarefas incluem:</span><span class="sxs-lookup"><span data-stu-id="34400-152">Tasks demonstrated in this series include:</span></span>

- <span data-ttu-id="34400-153">Criação, a revisão e executar o novo projeto</span><span class="sxs-lookup"><span data-stu-id="34400-153">Creating, reviewing and running the new project</span></span>
- <span data-ttu-id="34400-154">Criando a estrutura de banco de dados</span><span class="sxs-lookup"><span data-stu-id="34400-154">Creating the database structure</span></span>
- <span data-ttu-id="34400-155">Inicializando e a propagação do banco de dados</span><span class="sxs-lookup"><span data-stu-id="34400-155">Initializing and seeding the database</span></span>
- <span data-ttu-id="34400-156">Personalizando a interface do usuário usando estilos, gráficos e uma página mestra</span><span class="sxs-lookup"><span data-stu-id="34400-156">Customizing the UI using styles, graphics and a master page</span></span>
- <span data-ttu-id="34400-157">Adicionar páginas e navegação</span><span class="sxs-lookup"><span data-stu-id="34400-157">Adding pages and navigation</span></span>
- <span data-ttu-id="34400-158">Exibindo detalhes de menu e os dados de produto</span><span class="sxs-lookup"><span data-stu-id="34400-158">Displaying menu details and product data</span></span>
- <span data-ttu-id="34400-159">Criando um carrinho de compras</span><span class="sxs-lookup"><span data-stu-id="34400-159">Creating a shopping cart</span></span>
- <span data-ttu-id="34400-160">Suporte a adição de SSL e OAuth</span><span class="sxs-lookup"><span data-stu-id="34400-160">Adding SSL and OAuth support</span></span>
- <span data-ttu-id="34400-161">Adicionando um método de pagamento</span><span class="sxs-lookup"><span data-stu-id="34400-161">Adding a payment method</span></span>
- <span data-ttu-id="34400-162">Incluindo uma função de administrador e um usuário para o aplicativo</span><span class="sxs-lookup"><span data-stu-id="34400-162">Including an administrator role and a user to the application</span></span>
- <span data-ttu-id="34400-163">Restringir o acesso a páginas específicas e pasta</span><span class="sxs-lookup"><span data-stu-id="34400-163">Restricting access to specific pages and folder</span></span>
- <span data-ttu-id="34400-164">Carregar um arquivo para o aplicativo web</span><span class="sxs-lookup"><span data-stu-id="34400-164">Uploading a file to the web application</span></span>
- <span data-ttu-id="34400-165">Implementar validação de entrada</span><span class="sxs-lookup"><span data-stu-id="34400-165">Implementing input validation</span></span>
- <span data-ttu-id="34400-166">Registrando as rotas para o aplicativo web</span><span class="sxs-lookup"><span data-stu-id="34400-166">Registering routes for the web application</span></span>
- <span data-ttu-id="34400-167">Implementando o tratamento de erros e log de erros</span><span class="sxs-lookup"><span data-stu-id="34400-167">Implementing error handling and error logging</span></span>

## <a name="overview"></a><span data-ttu-id="34400-168">Visão geral</span><span class="sxs-lookup"><span data-stu-id="34400-168">Overview</span></span>

<span data-ttu-id="34400-169">Se você for novo no Web Forms do ASP.NET, mas têm familiaridade com conceitos de programação, você tem o direito tutorial.</span><span class="sxs-lookup"><span data-stu-id="34400-169">If you are new to ASP.NET Web Forms but have familiarity with programming concepts, you have the right tutorial.</span></span> <span data-ttu-id="34400-170">Se você já estiver familiarizado com o ASP.NET Web Forms, você pode aproveitar esta série de tutoriais, os novos recursos disponíveis no ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="34400-170">If you are already familiar with ASP.NET Web Forms, you can benefit from this tutorial series by the new features available in ASP.NET 4.5.</span></span> <span data-ttu-id="34400-171">Se você estiver familiarizado com conceitos de programação e Web Forms do ASP.NET, consulte os tutoriais adicionais fornecidos no Web Forms [guia de Introdução](../../../index.md) seção no site da Web do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="34400-171">If you are unfamiliar with programming concepts and ASP.NET Web Forms, see the additional tutorials provided in the Web Forms [Getting Started](../../../index.md) section on the ASP.NET Web site.</span></span>

<span data-ttu-id="34400-172">O específico **mais recente** ASP.NET 4.5 recursos fornecidos no Web Forms do série de tutoriais incluem o seguinte:</span><span class="sxs-lookup"><span data-stu-id="34400-172">The specific **latest** ASP.NET 4.5 features provided in this Web Forms tutorial series include the following:</span></span>

- <span data-ttu-id="34400-173">Uma interface do usuário simple para a criação de projetos que oferecem [dar suporte a várias estruturas de ASP.NET](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (Web Forms, MVC e API da Web).</span><span class="sxs-lookup"><span data-stu-id="34400-173">A simple UI for creating projects that offer [support for multiple ASP.NET frameworks](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (Web Forms, MVC, and Web API).</span></span>
- <span data-ttu-id="34400-174">[O Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), uma estrutura de layout e temas que fornece recursos dinâmicos de design e temas.</span><span class="sxs-lookup"><span data-stu-id="34400-174">[Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), a layout and theming framework that provides responsive design and theming capabilities.</span></span>
- <span data-ttu-id="34400-175">[O ASP.NET Identity](../../../../identity/index.md), um novo sistema de associação do ASP.NET que funciona da mesma em todas as estruturas do ASP.NET e funciona com o software que não seja o IIS de hospedagem na web.</span><span class="sxs-lookup"><span data-stu-id="34400-175">[ASP.NET Identity](../../../../identity/index.md), a new ASP.NET membership system that works the same in all ASP.NET frameworks and works with web hosting software other than IIS.</span></span>
- <span data-ttu-id="34400-176">[Entity Framework 6](https://msdn.microsoft.com/data/ef.aspx), uma atualização para o Entity Framework que permite recuperar e manipular dados tão fortemente tipada de objetos, acessar os dados de forma assíncrona, tratar falhas transitórias de conexão e as instruções SQL de log.</span><span class="sxs-lookup"><span data-stu-id="34400-176">[Entity Framework 6](https://msdn.microsoft.com/data/ef.aspx), an update to the Entity Framework which allows you retrieve and manipulate data as strongly typed objects, access data asynchronously, handle transient connection faults, and log SQL statements.</span></span>

<span data-ttu-id="34400-177">Para obter uma lista completa dos recursos do ASP.NET 4.5, consulte [ASP.NET e Web Tools para notas de versão do Visual Studio 2013](../../../../visual-studio/overview/2013/release-notes.md).</span><span class="sxs-lookup"><span data-stu-id="34400-177">For a complete list of ASP.NET 4.5 features, see [ASP.NET and Web Tools for Visual Studio 2013 Release Notes](../../../../visual-studio/overview/2013/release-notes.md).</span></span>

### <a name="the-wingtip-toys-sample-application"></a><span data-ttu-id="34400-178">O aplicativo de exemplo do Wingtip Toys</span><span class="sxs-lookup"><span data-stu-id="34400-178">The Wingtip Toys Sample Application</span></span>

<span data-ttu-id="34400-179">As capturas de tela a seguir fornecem uma visão rápida do aplicativo ASP.NET Web forms que serão criados nessa série de tutoriais.</span><span class="sxs-lookup"><span data-stu-id="34400-179">The following screen shots provide a quick view of the ASP.NET Web forms application that you will create in this tutorial series.</span></span> <span data-ttu-id="34400-180">Quando você executa o aplicativo do Visual Studio Express 2013 para Web, você verá a seguinte página inicial da web.</span><span class="sxs-lookup"><span data-stu-id="34400-180">When you run the application from Visual Studio Express 2013 for Web, you will see the following web Home page.</span></span>

![A Wingtip Toys - página padrão](introduction-and-overview/_static/image1.png)

<span data-ttu-id="34400-182">Você pode registrar como um novo usuário, ou faça logon como um usuário existente.</span><span class="sxs-lookup"><span data-stu-id="34400-182">You can register as a new user, or log in as an existing user.</span></span> <span data-ttu-id="34400-183">Na parte superior, a navegação é fornecida para cada categoria de produto, recuperando os produtos disponíveis do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="34400-183">Navigation is provided at the top for each product category by retrieving the available products from the database.</span></span>

<span data-ttu-id="34400-184">Selecionando o link de produtos, você poderá ver uma lista de todos os produtos disponíveis.</span><span class="sxs-lookup"><span data-stu-id="34400-184">By selecting the Products link, you will be able to see a list of all available products.</span></span>

![A Wingtip Toys - produtos](introduction-and-overview/_static/image2.png)

<span data-ttu-id="34400-186">Você também pode ver os detalhes do produto individuais selecionando qualquer um dos produtos listados.</span><span class="sxs-lookup"><span data-stu-id="34400-186">You can also see individual product details by selecting any of the listed products.</span></span>

![A Wingtip Toys - detalhes do produto](introduction-and-overview/_static/image3.png)

<span data-ttu-id="34400-188">Como um usuário, você pode registrar e faça logon usando a funcionalidade padrão do modelo de formulários da Web.</span><span class="sxs-lookup"><span data-stu-id="34400-188">As a user, you can register and log in using the default functionality of the Web Forms template.</span></span> <span data-ttu-id="34400-189">Este tutorial também explica como fazer logon usando uma conta existente do Gmail.</span><span class="sxs-lookup"><span data-stu-id="34400-189">This tutorial also explains how to login using an existing Gmail account.</span></span> <span data-ttu-id="34400-190">Além disso, você pode fazer logon como administrador para adicionar e remover os produtos de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="34400-190">Additionally, you can login as the administrator to add and remove products from the database.</span></span>

![A Wingtip Toys - faça logon no](introduction-and-overview/_static/image4.png)

<span data-ttu-id="34400-192">Depois de fazer logon como um usuário, você pode adicionar produtos ao carrinho de compras e check-out com o PayPal.</span><span class="sxs-lookup"><span data-stu-id="34400-192">Once you have logged in as a user, you can add products to the shopping cart and checkout with PayPal.</span></span> <span data-ttu-id="34400-193">Observe que este aplicativo de exemplo é projetado para funcionar com a área restrita do desenvolvedor do PayPal.</span><span class="sxs-lookup"><span data-stu-id="34400-193">Note that this sample application is designed to function with PayPal's developer sandbox.</span></span> <span data-ttu-id="34400-194">Nenhuma transação de dinheiro real ocorrerá.</span><span class="sxs-lookup"><span data-stu-id="34400-194">No actual money transaction will take place.</span></span>

![A Wingtip Toys - carrinho de compras](introduction-and-overview/_static/image5.png)

<span data-ttu-id="34400-196">PayPal confirmará a sua conta, ordem e informações de pagamento.</span><span class="sxs-lookup"><span data-stu-id="34400-196">PayPal will confirm your account, order, and payment information.</span></span>

![A Wingtip Toys - PayPal](introduction-and-overview/_static/image6.png)

<span data-ttu-id="34400-198">Depois de retornar do PayPal, você pode revisar e concluir o pedido.</span><span class="sxs-lookup"><span data-stu-id="34400-198">After returning from PayPal, you can review and complete your order.</span></span>

![A Wingtip Toys - revisão de ordem](introduction-and-overview/_static/image7.png)

## <a name="prerequisites"></a><span data-ttu-id="34400-200">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="34400-200">Prerequisites</span></span>

<span data-ttu-id="34400-201">Antes de começar, certifique-se de que você tenha o seguinte software instalado em seu computador:</span><span class="sxs-lookup"><span data-stu-id="34400-201">Before you start, make sure that you have the following software installed on your computer:</span></span>

- <span data-ttu-id="34400-202">[Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) ou [Microsoft Visual Studio Express 2013 para Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span><span class="sxs-lookup"><span data-stu-id="34400-202">[Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) or [Microsoft Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span></span> <span data-ttu-id="34400-203">O .NET Framework é instalado automaticamente.</span><span class="sxs-lookup"><span data-stu-id="34400-203">The .NET Framework is installed automatically.</span></span>

<span data-ttu-id="34400-204">Esta série de tutoriais usa o Microsoft Visual Studio Express 2013 para Web.</span><span class="sxs-lookup"><span data-stu-id="34400-204">This tutorial series uses Microsoft Visual Studio Express 2013 for Web.</span></span> <span data-ttu-id="34400-205">Você pode usar o Microsoft Visual Studio Express 2013 para Web ou Microsoft Visual Studio 2013 para concluir esta série de tutoriais.</span><span class="sxs-lookup"><span data-stu-id="34400-205">You can use either Microsoft Visual Studio Express 2013 for Web or Microsoft Visual Studio 2013 to complete this tutorial series.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="34400-206">Microsoft Visual Studio 2013 e Microsoft Visual Studio Express 2013 para Web serão ser conhecido como Visual Studio em toda esta série de tutoriais.</span><span class="sxs-lookup"><span data-stu-id="34400-206">Microsoft Visual Studio 2013 and Microsoft Visual Studio Express 2013 for Web will often be referred to as Visual Studio throughout this tutorial series.</span></span>


<span data-ttu-id="34400-207">Se você já tiver uma versão do Visual Studio instalada, o processo de instalação instalará o Visual Studio 2013 ou Microsoft Visual Studio Express 2013 para Web ao lado da versão existente.</span><span class="sxs-lookup"><span data-stu-id="34400-207">If you already have a Visual Studio version installed, the installation process will install Visual Studio 2013 or Microsoft Visual Studio Express 2013 for Web next to the existing version.</span></span> <span data-ttu-id="34400-208">Sites criados em versões anteriores podem ser abertos no Visual Studio 2013 e continuam a abrir nas versões anteriores.</span><span class="sxs-lookup"><span data-stu-id="34400-208">Sites that you created in earlier versions can be opened in Visual Studio 2013 and continue to open in previous versions.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="34400-209">Este passo a passo pressupõe que você selecionou a *desenvolvimento Web* coleção de configurações na primeira vez que você iniciou o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="34400-209">This walkthrough assumes that you selected the *Web Development* collection of settings the first time that you started Visual Studio.</span></span> <span data-ttu-id="34400-210">Para obter mais informações, consulte [como: selecionar configurações de ambiente de desenvolvimento da Web](https://msdn.microsoft.com/library/ff521558.aspx).</span><span class="sxs-lookup"><span data-stu-id="34400-210">For more information, see [How to: Select Web Development Environment Settings](https://msdn.microsoft.com/library/ff521558.aspx).</span></span>


## <a name="download-the-sample-application"></a><span data-ttu-id="34400-211">Baixe o aplicativo de exemplo</span><span class="sxs-lookup"><span data-stu-id="34400-211">Download the Sample Application</span></span>

<span data-ttu-id="34400-212">Depois de instalar os pré-requisitos, você está pronto para começar a criar o novo projeto Web que é apresentado nessa série de tutoriais.</span><span class="sxs-lookup"><span data-stu-id="34400-212">After installing the prerequisites, you are ready to begin creating the new Web project that is presented in this tutorial series.</span></span> <span data-ttu-id="34400-213">Se você quiser **opcionalmente** executar o aplicativo de exemplo que cria esta série de tutoriais, você pode baixá-lo do site de exemplos do MSDN.</span><span class="sxs-lookup"><span data-stu-id="34400-213">If you would like to **optionally** run the sample application that this tutorial series creates, you can download it from the MSDN Samples site.</span></span> <span data-ttu-id="34400-214">Este download contém o seguinte:</span><span class="sxs-lookup"><span data-stu-id="34400-214">This download contains the following:</span></span>

- <span data-ttu-id="34400-215">O aplicativo de exemplo na *WingtipToys* pasta.</span><span class="sxs-lookup"><span data-stu-id="34400-215">The sample application in the *WingtipToys* folder.</span></span>
- <span data-ttu-id="34400-216">Os recursos usados para criar o aplicativo de exemplo na *ativos WingtipToys* pasta na *WingtipToys* pasta.</span><span class="sxs-lookup"><span data-stu-id="34400-216">The resources used to create the sample application in the *WingtipToys-Assets* folder in the *WingtipToys* folder.</span></span>

#### <a name="download-the-file-from-msdn-samples-site"></a><span data-ttu-id="34400-217">Baixe o arquivo do site de exemplos do MSDN:</span><span class="sxs-lookup"><span data-stu-id="34400-217">Download the file from MSDN Samples site:</span></span>

<span data-ttu-id="34400-218">[Introdução ao ASP.NET 4.5 do Web Forms e Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (c#)</span><span class="sxs-lookup"><span data-stu-id="34400-218">[Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span></span> 

<span data-ttu-id="34400-219">O download é um <em>. zip</em> arquivo.</span><span class="sxs-lookup"><span data-stu-id="34400-219">The download is a <em>.zip</em> file.</span></span> <span data-ttu-id="34400-220">Para ver o projeto completo que cria esta série de tutoriais, encontre e selecione os <em>c#</em>pasta o <em>. zip</em> arquivo.</span><span class="sxs-lookup"><span data-stu-id="34400-220">To see the completed project that this tutorial series creates, find and select the <em>C#</em>folder in the <em>.zip</em> file.</span></span> <span data-ttu-id="34400-221">Salvar a <em>c#</em> folderto a pasta que você pode usar para trabalhar com projetos do Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="34400-221">Save the <em>C#</em> folderto the folder you use to work with Visual Studio 2013 projects.</span></span> <span data-ttu-id="34400-222">Por padrão, a pasta de projetos do Visual Studio 2013 é o seguinte:</span><span class="sxs-lookup"><span data-stu-id="34400-222">By default, the Visual Studio 2013 projects folder is the following:</span></span>

<span data-ttu-id="34400-223"><strong>C:\Users&#92;</strong><strong><em>&lt;username&gt;</em></strong><strong>\Documents\Visual Studio 2013\Projects</strong></span><span class="sxs-lookup"><span data-stu-id="34400-223"><strong>C:\Users&#92;</strong><strong><em>&lt;username&gt;</em></strong><strong>\Documents\Visual Studio 2013\Projects</strong></span></span>

<span data-ttu-id="34400-224">Renomeie o ***c#*** pasta a ser ***WingtipToys***.</span><span class="sxs-lookup"><span data-stu-id="34400-224">Rename the ***C#*** folder to ***WingtipToys***.</span></span>

> [!NOTE]
> <span data-ttu-id="34400-225">Se você já tiver uma pasta chamada *WingtipToys* na sua pasta de projetos, renomeie temporariamente essa pasta existente antes de renomear o *c#* pasta a ser *WingtipToys*.</span><span class="sxs-lookup"><span data-stu-id="34400-225">If you already have a folder named *WingtipToys* in your Projects folder, temporarily rename that existing folder before renaming the *C#* folder to *WingtipToys*.</span></span>


<span data-ttu-id="34400-226">Para executar o projeto concluído, abra o *WingtipToys* pasta e clique duas vezes o *WingtipToys.sln* arquivo.</span><span class="sxs-lookup"><span data-stu-id="34400-226">To run the completed project, open the *WingtipToys* folder and double-click the *WingtipToys.sln* file.</span></span> <span data-ttu-id="34400-227">Visual Studio 2013 abrirá o projeto.</span><span class="sxs-lookup"><span data-stu-id="34400-227">Visual Studio 2013 will open the project.</span></span> <span data-ttu-id="34400-228">Em seguida, clique com botão direito do *default. aspx* do arquivo na janela do Gerenciador de soluções e clique em Exibir no navegador do menu de atalho.</span><span class="sxs-lookup"><span data-stu-id="34400-228">Next, right-click the *Default.aspx* file in the Solution Explorer window and click View In Browser from the right-click menu.</span></span>

### <a name="tutorial-support-and-comments"></a><span data-ttu-id="34400-229">Comentários e suporte de tutorial</span><span class="sxs-lookup"><span data-stu-id="34400-229">Tutorial Support and Comments</span></span>

<span data-ttu-id="34400-230">Use a seção de p e r incluída com o [Introdução ao Web Forms do ASP.NET 4.5 e Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (c#) exemplo tiver perguntas ou comentários.</span><span class="sxs-lookup"><span data-stu-id="34400-230">Use the Q AND A section included with the [Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) sample for any questions or comments.</span></span>

<span data-ttu-id="34400-231">Comentários sobre esta série de tutoriais são boas-vindos e quando esta série de tutoriais é atualizada todos os esforços serão feito para correções de conta ou sugestões de melhorias que são fornecidas nos comentários tutoriais.</span><span class="sxs-lookup"><span data-stu-id="34400-231">Comments on this tutorial series are welcome, and when this tutorial series is updated every effort will be made to take into account corrections or suggestions for improvements that are provided in the tutorial comments.</span></span>

<span data-ttu-id="34400-232">Quando ocorrer um erro durante o desenvolvimento, ou se o site da Web não é executado corretamente, as mensagens de erro podem fornecer pistas complexas para a origem do problema ou não podem explicar como corrigi-lo.</span><span class="sxs-lookup"><span data-stu-id="34400-232">When an error happens during development, or if the Web site does not run correctly, the error messages may give complex clues to the source of the problem or might not explain how to fix it.</span></span> <span data-ttu-id="34400-233">Para ajudar você com alguns cenários comuns de problema, você também pode usar o [fóruns do ASP.NET](https://forums.asp.net/) ou na seção de p e r incluídas com o [Introdução ao Web Forms do ASP.NET 4.5 e Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) ( C#) exemplo.</span><span class="sxs-lookup"><span data-stu-id="34400-233">To help you with some common problem scenarios, you can also use the [ASP.NET forums](https://forums.asp.net/) or the Q AND A section included with the [Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) sample.</span></span> <span data-ttu-id="34400-234">Se você receber uma mensagem de erro ou se algo não funciona ao percorrer os tutoriais, certifique-se de verificar os locais acima.</span><span class="sxs-lookup"><span data-stu-id="34400-234">If you get an error message or something doesn't work as you go through the tutorials, be sure to check the above locations.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="34400-235">Avançar</span><span class="sxs-lookup"><span data-stu-id="34400-235">Next</span></span>](create-the-project.md)

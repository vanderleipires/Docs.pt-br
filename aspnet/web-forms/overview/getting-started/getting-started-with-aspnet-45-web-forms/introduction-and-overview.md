---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
title: Introdução ao ASP.NET 4.5 do Web Forms e Visual Studio 2013 | Microsoft Docs
author: Erikre
description: Esta série de tutoriais passo a passo ensina as Noções básicas de criação de um aplicativo de Web Forms do ASP.NET usando o ASP.NET 4.5 e o Microsoft Visual Studio Express...
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 9b96eaa1-8ef0-4338-a2e8-e0f970bfaf68
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
msc.type: authoredcontent
ms.openlocfilehash: b9ed6ce4ac13f047f53c56e183433cbd038ea15c
ms.sourcegitcommit: e9b99854b0a8021dafabee0db5e1338067f250a9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2018
ms.locfileid: "52450678"
---
<a name="getting-started-with-aspnet-45-web-forms-and-visual-studio-2013"></a><span data-ttu-id="eb7eb-103">Introdução ao ASP.NET 4.5 do Web Forms e Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="eb7eb-103">Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013</span></span>
====================
<span data-ttu-id="eb7eb-104">por [Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="eb7eb-104">by [Erik Reitan](https://github.com/Erikre)</span></span>

<span data-ttu-id="eb7eb-105">[Baixe o projeto de exemplo do Wingtip Toys (c#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) ou [Baixe o livro eletrônico (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span><span class="sxs-lookup"><span data-stu-id="eb7eb-105">[Download Wingtip Toys Sample Project (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) or [Download E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span></span>

<span data-ttu-id="eb7eb-106">Esta série de tutoriais passo a passo ensina os conceitos básicos da criação de um aplicativo de Web Forms do ASP.NET usando o ASP.NET 4.5 e do Microsoft Visual Studio Express 2013 para Web.</span><span class="sxs-lookup"><span data-stu-id="eb7eb-106">This step-by-step tutorial series will teach you the basics of building an ASP.NET Web Forms application using ASP.NET 4.5 and Microsoft Visual Studio Express 2013 for Web.</span></span> 

## <a name="introduction"></a><span data-ttu-id="eb7eb-107">Introdução</span><span class="sxs-lookup"><span data-stu-id="eb7eb-107">Introduction</span></span>

<span data-ttu-id="eb7eb-108">Esta série de tutoriais orienta você pelas etapas necessárias para criar um aplicativo Web Forms do ASP.NET usando o Visual Studio Express 2013 para Web e ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="eb7eb-108">This series of tutorials guides you through the steps required to create an ASP.NET Web Forms application using Visual Studio Express 2013 for Web and ASP.NET 4.5.</span></span>

<span data-ttu-id="eb7eb-109">O aplicativo, você cria é denominado **Wingtip Toys**.</span><span class="sxs-lookup"><span data-stu-id="eb7eb-109">The application you'll create is named **Wingtip Toys**.</span></span> <span data-ttu-id="eb7eb-110">É um exemplo simplificado de um site de armazenamento de front-que vende itens online.</span><span class="sxs-lookup"><span data-stu-id="eb7eb-110">It's a simplified example of a store front web site that sells items online.</span></span> <span data-ttu-id="eb7eb-111">Esta série de tutoriais destaca os novos recursos disponíveis no ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="eb7eb-111">This tutorial series highlights new features available in ASP.NET 4.5.</span></span>

<span data-ttu-id="eb7eb-112">Comentários são bem-vindos e podemos fazer todos os esforços para atualizar esta série de tutoriais com base em suas sugestões.</span><span class="sxs-lookup"><span data-stu-id="eb7eb-112">Comments are welcome, and we'll make every effort to update this tutorial series based on your suggestions.</span></span>

### <a name="download-completed-project"></a><span data-ttu-id="eb7eb-113">Projeto de download concluído</span><span class="sxs-lookup"><span data-stu-id="eb7eb-113">Download completed project</span></span>

<span data-ttu-id="eb7eb-114">Você pode baixar um projeto c# que contém o tutorial concluído.</span><span class="sxs-lookup"><span data-stu-id="eb7eb-114">You can download a C# project that contains the completed tutorial.</span></span>

- <span data-ttu-id="eb7eb-115">[Introdução ao ASP.NET 4.5 do Web Forms e Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (c#)</span><span class="sxs-lookup"><span data-stu-id="eb7eb-115">[Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span></span>

### <a name="review-the-content-by-taking-the-related-aspnet-web-forms-quiz"></a><span data-ttu-id="eb7eb-116">Revise o conteúdo, fazendo o teste de Web Forms do ASP.NET relacionado</span><span class="sxs-lookup"><span data-stu-id="eb7eb-116">Review the content by taking the related ASP.NET Web Forms quiz</span></span>

<span data-ttu-id="eb7eb-117">Depois de concluir este tutorial, teste seu conhecimento e reforçar os conceitos-chave por meio de [teste do ASP.NET Web Forms](http://quizapp.cloudapp.net/?quiz=ASP.NET&cdn_id=2014-01-17-001).</span><span class="sxs-lookup"><span data-stu-id="eb7eb-117">After you complete this tutorial, test your knowledge and reinforce key concepts by taking the [ASP.NET Web Forms Quiz](http://quizapp.cloudapp.net/?quiz=ASP.NET&cdn_id=2014-01-17-001).</span></span> <span data-ttu-id="eb7eb-118">Este teste foi desenvolvido especificamente desde o conteúdo contido nessa série de tutoriais.</span><span class="sxs-lookup"><span data-stu-id="eb7eb-118">This quiz was specifically designed from content contained in this tutorial series.</span></span> <span data-ttu-id="eb7eb-119">Cada pergunta em teste fornece uma explicação juntamente com links para orientações adicionais.</span><span class="sxs-lookup"><span data-stu-id="eb7eb-119">Each question in the quiz provides an explanation along with links to additional guidance.</span></span>

- [<span data-ttu-id="eb7eb-120">ASP.NET Web Forms do teste</span><span class="sxs-lookup"><span data-stu-id="eb7eb-120">ASP.NET Web Forms Quiz</span></span>](http://quizapp.cloudapp.net/?quiz=ASP.NET)

### <a name="audience"></a><span data-ttu-id="eb7eb-121">Público-alvo</span><span class="sxs-lookup"><span data-stu-id="eb7eb-121">Audience</span></span>

<span data-ttu-id="eb7eb-122">O público-alvo desta série de tutoriais é os desenvolvedores experientes novatos no Web Forms do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="eb7eb-122">The intended audience of this tutorial series is experienced developers who are new to ASP.NET Web Forms.</span></span> <span data-ttu-id="eb7eb-123">Um desenvolvedor interessado nessa série de tutoriais deve ter as seguintes capacidades:</span><span class="sxs-lookup"><span data-stu-id="eb7eb-123">A developer interested in this tutorial series should have the following skills:</span></span>

- <span data-ttu-id="eb7eb-124">Esteja familiarizado com um objeto orientado a linguagem de programação (OOP)</span><span class="sxs-lookup"><span data-stu-id="eb7eb-124">Familiar with an object oriented programming (OOP) language</span></span>
- <span data-ttu-id="eb7eb-125">Esteja familiarizado com conceitos de desenvolvimento da Web (HTML, CSS e JavaScript)</span><span class="sxs-lookup"><span data-stu-id="eb7eb-125">Familiar with Web development concepts (HTML, CSS, JavaScript)</span></span>
- <span data-ttu-id="eb7eb-126">Conheça os conceitos de banco de dados relacional</span><span class="sxs-lookup"><span data-stu-id="eb7eb-126">Familiar with relational database concepts</span></span>
- <span data-ttu-id="eb7eb-127">Conheça os conceitos de arquitetura de n camadas</span><span class="sxs-lookup"><span data-stu-id="eb7eb-127">Familiar with n-tier architecture concepts</span></span>

<span data-ttu-id="eb7eb-128">Se você estiver interessado em analisar as áreas listadas acima, considere a possibilidade de revisar o conteúdo a seguir:</span><span class="sxs-lookup"><span data-stu-id="eb7eb-128">If you are interested in reviewing the areas listed above, consider reviewing the following content:</span></span>

- [<span data-ttu-id="eb7eb-129">Introdução ao Visual c#</span><span class="sxs-lookup"><span data-stu-id="eb7eb-129">Getting Started with Visual C#</span></span>](https://msdn.microsoft.com/library/a72418yk.aspx)
- <span data-ttu-id="eb7eb-130">[Desenvolvimento na Web](https://msdn.microsoft.com/beginner/bb308760.aspx), [HTML, CSS, JavaScript, SQL, PHP, JQuery](http://w3schools.com/)</span><span class="sxs-lookup"><span data-stu-id="eb7eb-130">[Web Development](https://msdn.microsoft.com/beginner/bb308760.aspx), [HTML, CSS, JavaScript, SQL, PHP, JQuery](http://w3schools.com/)</span></span>
- [<span data-ttu-id="eb7eb-131">Banco de dados relacional</span><span class="sxs-lookup"><span data-stu-id="eb7eb-131">Relational database</span></span>](http://en.wikipedia.org/wiki/Relational_database)
- [<span data-ttu-id="eb7eb-132">Arquitetura de várias camadas</span><span class="sxs-lookup"><span data-stu-id="eb7eb-132">Multitier architecture</span></span>](http://en.wikipedia.org/wiki/Multitier_architecture)

### <a name="application-features"></a><span data-ttu-id="eb7eb-133">Recursos do aplicativo</span><span class="sxs-lookup"><span data-stu-id="eb7eb-133">Application Features</span></span>

<span data-ttu-id="eb7eb-134">Os recursos do Web Form do ASP.NET apresentados nesta série incluem:</span><span class="sxs-lookup"><span data-stu-id="eb7eb-134">The ASP.NET Web Form features presented in this series include:</span></span>

- <span data-ttu-id="eb7eb-135">O projeto de aplicativo Web (não o projeto de Site da Web)</span><span class="sxs-lookup"><span data-stu-id="eb7eb-135">The Web Application Project (not Web Site Project)</span></span>
- <span data-ttu-id="eb7eb-136">Web Forms</span><span class="sxs-lookup"><span data-stu-id="eb7eb-136">Web Forms</span></span>
- <span data-ttu-id="eb7eb-137">Páginas mestras, configuração</span><span class="sxs-lookup"><span data-stu-id="eb7eb-137">Master Pages, Configuration</span></span>
- <span data-ttu-id="eb7eb-138">Bootstrap</span><span class="sxs-lookup"><span data-stu-id="eb7eb-138">Bootstrap</span></span>
- <span data-ttu-id="eb7eb-139">Entity Framework Code First, LocalDB</span><span class="sxs-lookup"><span data-stu-id="eb7eb-139">Entity Framework Code First, LocalDB</span></span>
- <span data-ttu-id="eb7eb-140">Validação de solicitação</span><span class="sxs-lookup"><span data-stu-id="eb7eb-140">Request Validation</span></span>
- <span data-ttu-id="eb7eb-141">Fortemente tipado a controles de dados, da associação, anotações de dados de modelo e provedores de valor</span><span class="sxs-lookup"><span data-stu-id="eb7eb-141">Strongly Typed Data Controls, Model Binding, Data Annotations, and Value Providers</span></span>
- <span data-ttu-id="eb7eb-142">SSL e OAuth</span><span class="sxs-lookup"><span data-stu-id="eb7eb-142">SSL and OAuth</span></span>
- <span data-ttu-id="eb7eb-143">O ASP.NET Identity, configuração e autorização</span><span class="sxs-lookup"><span data-stu-id="eb7eb-143">ASP.NET Identity, Configuration, and Authorization</span></span>
- <span data-ttu-id="eb7eb-144">Validação não invasiva</span><span class="sxs-lookup"><span data-stu-id="eb7eb-144">Unobtrusive Validation</span></span>
- <span data-ttu-id="eb7eb-145">Roteamento</span><span class="sxs-lookup"><span data-stu-id="eb7eb-145">Routing</span></span>
- <span data-ttu-id="eb7eb-146">Tratamento de erros do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="eb7eb-146">ASP.NET Error Handling</span></span>

### <a name="application-scenarios-and-tasks"></a><span data-ttu-id="eb7eb-147">Tarefas e cenários de aplicativo</span><span class="sxs-lookup"><span data-stu-id="eb7eb-147">Application Scenarios and Tasks</span></span>

<span data-ttu-id="eb7eb-148">Demonstrado nesta série de tarefas incluem:</span><span class="sxs-lookup"><span data-stu-id="eb7eb-148">Tasks demonstrated in this series include:</span></span>

- <span data-ttu-id="eb7eb-149">Criação, a revisão e executar o novo projeto</span><span class="sxs-lookup"><span data-stu-id="eb7eb-149">Creating, reviewing and running the new project</span></span>
- <span data-ttu-id="eb7eb-150">Criando a estrutura de banco de dados</span><span class="sxs-lookup"><span data-stu-id="eb7eb-150">Creating the database structure</span></span>
- <span data-ttu-id="eb7eb-151">Inicializando e a propagação do banco de dados</span><span class="sxs-lookup"><span data-stu-id="eb7eb-151">Initializing and seeding the database</span></span>
- <span data-ttu-id="eb7eb-152">Personalizando a interface do usuário usando estilos, gráficos e uma página mestra</span><span class="sxs-lookup"><span data-stu-id="eb7eb-152">Customizing the UI using styles, graphics and a master page</span></span>
- <span data-ttu-id="eb7eb-153">Adicionar páginas e navegação</span><span class="sxs-lookup"><span data-stu-id="eb7eb-153">Adding pages and navigation</span></span>
- <span data-ttu-id="eb7eb-154">Exibindo detalhes de menu e os dados de produto</span><span class="sxs-lookup"><span data-stu-id="eb7eb-154">Displaying menu details and product data</span></span>
- <span data-ttu-id="eb7eb-155">Criando um carrinho de compras</span><span class="sxs-lookup"><span data-stu-id="eb7eb-155">Creating a shopping cart</span></span>
- <span data-ttu-id="eb7eb-156">Suporte a adição de SSL e OAuth</span><span class="sxs-lookup"><span data-stu-id="eb7eb-156">Adding SSL and OAuth support</span></span>
- <span data-ttu-id="eb7eb-157">Adicionando um método de pagamento</span><span class="sxs-lookup"><span data-stu-id="eb7eb-157">Adding a payment method</span></span>
- <span data-ttu-id="eb7eb-158">Incluindo uma função de administrador e um usuário para o aplicativo</span><span class="sxs-lookup"><span data-stu-id="eb7eb-158">Including an administrator role and a user to the application</span></span>
- <span data-ttu-id="eb7eb-159">Restringir o acesso a páginas específicas e pasta</span><span class="sxs-lookup"><span data-stu-id="eb7eb-159">Restricting access to specific pages and folder</span></span>
- <span data-ttu-id="eb7eb-160">Carregar um arquivo para o aplicativo web</span><span class="sxs-lookup"><span data-stu-id="eb7eb-160">Uploading a file to the web application</span></span>
- <span data-ttu-id="eb7eb-161">Implementar validação de entrada</span><span class="sxs-lookup"><span data-stu-id="eb7eb-161">Implementing input validation</span></span>
- <span data-ttu-id="eb7eb-162">Registrando as rotas para o aplicativo web</span><span class="sxs-lookup"><span data-stu-id="eb7eb-162">Registering routes for the web application</span></span>
- <span data-ttu-id="eb7eb-163">Implementando o tratamento de erros e log de erros</span><span class="sxs-lookup"><span data-stu-id="eb7eb-163">Implementing error handling and error logging</span></span>

## <a name="overview"></a><span data-ttu-id="eb7eb-164">Visão geral</span><span class="sxs-lookup"><span data-stu-id="eb7eb-164">Overview</span></span>

<span data-ttu-id="eb7eb-165">Se você for novo no Web Forms do ASP.NET, mas têm familiaridade com conceitos de programação, você tem o direito tutorial.</span><span class="sxs-lookup"><span data-stu-id="eb7eb-165">If you are new to ASP.NET Web Forms but have familiarity with programming concepts, you have the right tutorial.</span></span> <span data-ttu-id="eb7eb-166">Se você já estiver familiarizado com o ASP.NET Web Forms, você pode aproveitar esta série de tutoriais, os novos recursos disponíveis no ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="eb7eb-166">If you are already familiar with ASP.NET Web Forms, you can benefit from this tutorial series by the new features available in ASP.NET 4.5.</span></span> <span data-ttu-id="eb7eb-167">Se você estiver familiarizado com conceitos de programação e Web Forms do ASP.NET, consulte os tutoriais adicionais fornecidos no Web Forms [guia de Introdução](../../../index.md) seção no site da Web do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="eb7eb-167">If you are unfamiliar with programming concepts and ASP.NET Web Forms, see the additional tutorials provided in the Web Forms [Getting Started](../../../index.md) section on the ASP.NET Web site.</span></span>

<span data-ttu-id="eb7eb-168">O específico **mais recente** ASP.NET 4.5 recursos fornecidos no Web Forms do série de tutoriais incluem o seguinte:</span><span class="sxs-lookup"><span data-stu-id="eb7eb-168">The specific **latest** ASP.NET 4.5 features provided in this Web Forms tutorial series include the following:</span></span>

- <span data-ttu-id="eb7eb-169">Uma interface do usuário simple para a criação de projetos que oferecem [dar suporte a várias estruturas de ASP.NET](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (Web Forms, MVC e API da Web).</span><span class="sxs-lookup"><span data-stu-id="eb7eb-169">A simple UI for creating projects that offer [support for multiple ASP.NET frameworks](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (Web Forms, MVC, and Web API).</span></span>
- <span data-ttu-id="eb7eb-170">[O Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), uma estrutura de layout e temas que fornece recursos dinâmicos de design e temas.</span><span class="sxs-lookup"><span data-stu-id="eb7eb-170">[Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), a layout and theming framework that provides responsive design and theming capabilities.</span></span>
- <span data-ttu-id="eb7eb-171">[O ASP.NET Identity](../../../../identity/index.md), um novo sistema de associação do ASP.NET que funciona da mesma em todas as estruturas do ASP.NET e funciona com o software que não seja o IIS de hospedagem na web.</span><span class="sxs-lookup"><span data-stu-id="eb7eb-171">[ASP.NET Identity](../../../../identity/index.md), a new ASP.NET membership system that works the same in all ASP.NET frameworks and works with web hosting software other than IIS.</span></span>
- <span data-ttu-id="eb7eb-172">[Entity Framework 6](https://msdn.microsoft.com/data/ef.aspx), uma atualização para o Entity Framework que permite recuperar e manipular dados tão fortemente tipada de objetos, acessar os dados de forma assíncrona, tratar falhas transitórias de conexão e as instruções SQL de log.</span><span class="sxs-lookup"><span data-stu-id="eb7eb-172">[Entity Framework 6](https://msdn.microsoft.com/data/ef.aspx), an update to the Entity Framework which allows you retrieve and manipulate data as strongly typed objects, access data asynchronously, handle transient connection faults, and log SQL statements.</span></span>

<span data-ttu-id="eb7eb-173">Para obter uma lista completa dos recursos do ASP.NET 4.5, consulte [ASP.NET e Web Tools para notas de versão do Visual Studio 2013](../../../../visual-studio/overview/2013/release-notes.md).</span><span class="sxs-lookup"><span data-stu-id="eb7eb-173">For a complete list of ASP.NET 4.5 features, see [ASP.NET and Web Tools for Visual Studio 2013 Release Notes](../../../../visual-studio/overview/2013/release-notes.md).</span></span>

### <a name="the-wingtip-toys-sample-application"></a><span data-ttu-id="eb7eb-174">O aplicativo de exemplo do Wingtip Toys</span><span class="sxs-lookup"><span data-stu-id="eb7eb-174">The Wingtip Toys Sample Application</span></span>

<span data-ttu-id="eb7eb-175">As capturas de tela a seguir fornecem uma visão rápida do aplicativo ASP.NET Web forms que serão criados nessa série de tutoriais.</span><span class="sxs-lookup"><span data-stu-id="eb7eb-175">The following screen shots provide a quick view of the ASP.NET Web forms application that you will create in this tutorial series.</span></span> <span data-ttu-id="eb7eb-176">Quando você executa o aplicativo do Visual Studio Express 2013 para Web, você verá a seguinte página inicial da web.</span><span class="sxs-lookup"><span data-stu-id="eb7eb-176">When you run the application from Visual Studio Express 2013 for Web, you will see the following web Home page.</span></span>

![A Wingtip Toys - página padrão](introduction-and-overview/_static/image1.png)

<span data-ttu-id="eb7eb-178">Você pode registrar como um novo usuário, ou faça logon como um usuário existente.</span><span class="sxs-lookup"><span data-stu-id="eb7eb-178">You can register as a new user, or log in as an existing user.</span></span> <span data-ttu-id="eb7eb-179">Na parte superior, a navegação é fornecida para cada categoria de produto, recuperando os produtos disponíveis do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="eb7eb-179">Navigation is provided at the top for each product category by retrieving the available products from the database.</span></span>

<span data-ttu-id="eb7eb-180">Selecionando o link de produtos, você poderá ver uma lista de todos os produtos disponíveis.</span><span class="sxs-lookup"><span data-stu-id="eb7eb-180">By selecting the Products link, you will be able to see a list of all available products.</span></span>

![A Wingtip Toys - produtos](introduction-and-overview/_static/image2.png)

<span data-ttu-id="eb7eb-182">Você também pode ver os detalhes do produto individuais selecionando qualquer um dos produtos listados.</span><span class="sxs-lookup"><span data-stu-id="eb7eb-182">You can also see individual product details by selecting any of the listed products.</span></span>

![A Wingtip Toys - detalhes do produto](introduction-and-overview/_static/image3.png)

<span data-ttu-id="eb7eb-184">Como um usuário, você pode registrar e faça logon usando a funcionalidade padrão do modelo de formulários da Web.</span><span class="sxs-lookup"><span data-stu-id="eb7eb-184">As a user, you can register and log in using the default functionality of the Web Forms template.</span></span> <span data-ttu-id="eb7eb-185">Este tutorial também explica como fazer logon usando uma conta existente do Gmail.</span><span class="sxs-lookup"><span data-stu-id="eb7eb-185">This tutorial also explains how to login using an existing Gmail account.</span></span> <span data-ttu-id="eb7eb-186">Além disso, você pode fazer logon como administrador para adicionar e remover os produtos de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="eb7eb-186">Additionally, you can login as the administrator to add and remove products from the database.</span></span>

![A Wingtip Toys - faça logon no](introduction-and-overview/_static/image4.png)

<span data-ttu-id="eb7eb-188">Depois de fazer logon como um usuário, você pode adicionar produtos ao carrinho de compras e check-out com o PayPal.</span><span class="sxs-lookup"><span data-stu-id="eb7eb-188">Once you have logged in as a user, you can add products to the shopping cart and checkout with PayPal.</span></span> <span data-ttu-id="eb7eb-189">Observe que este aplicativo de exemplo é projetado para funcionar com a área restrita do desenvolvedor do PayPal.</span><span class="sxs-lookup"><span data-stu-id="eb7eb-189">Note that this sample application is designed to function with PayPal's developer sandbox.</span></span> <span data-ttu-id="eb7eb-190">Nenhuma transação de dinheiro real ocorrerá.</span><span class="sxs-lookup"><span data-stu-id="eb7eb-190">No actual money transaction will take place.</span></span>

![A Wingtip Toys - carrinho de compras](introduction-and-overview/_static/image5.png)

<span data-ttu-id="eb7eb-192">PayPal confirmará a sua conta, ordem e informações de pagamento.</span><span class="sxs-lookup"><span data-stu-id="eb7eb-192">PayPal will confirm your account, order, and payment information.</span></span>

![A Wingtip Toys - PayPal](introduction-and-overview/_static/image6.png)

<span data-ttu-id="eb7eb-194">Depois de retornar do PayPal, você pode revisar e concluir o pedido.</span><span class="sxs-lookup"><span data-stu-id="eb7eb-194">After returning from PayPal, you can review and complete your order.</span></span>

![A Wingtip Toys - revisão de ordem](introduction-and-overview/_static/image7.png)

## <a name="prerequisites"></a><span data-ttu-id="eb7eb-196">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="eb7eb-196">Prerequisites</span></span>

<span data-ttu-id="eb7eb-197">Antes de começar, certifique-se de que você tenha o seguinte software instalado em seu computador:</span><span class="sxs-lookup"><span data-stu-id="eb7eb-197">Before you start, make sure that you have the following software installed on your computer:</span></span>

- <span data-ttu-id="eb7eb-198">[Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) ou [Microsoft Visual Studio Express 2013 para Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span><span class="sxs-lookup"><span data-stu-id="eb7eb-198">[Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) or [Microsoft Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span></span> <span data-ttu-id="eb7eb-199">O .NET Framework é instalado automaticamente.</span><span class="sxs-lookup"><span data-stu-id="eb7eb-199">The .NET Framework is installed automatically.</span></span>

<span data-ttu-id="eb7eb-200">Esta série de tutoriais usa o Microsoft Visual Studio Express 2013 para Web.</span><span class="sxs-lookup"><span data-stu-id="eb7eb-200">This tutorial series uses Microsoft Visual Studio Express 2013 for Web.</span></span> <span data-ttu-id="eb7eb-201">Você pode usar o Microsoft Visual Studio Express 2013 para Web ou Microsoft Visual Studio 2013 para concluir esta série de tutoriais.</span><span class="sxs-lookup"><span data-stu-id="eb7eb-201">You can use either Microsoft Visual Studio Express 2013 for Web or Microsoft Visual Studio 2013 to complete this tutorial series.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="eb7eb-202">Microsoft Visual Studio 2013 e Microsoft Visual Studio Express 2013 para Web serão ser conhecido como Visual Studio em toda esta série de tutoriais.</span><span class="sxs-lookup"><span data-stu-id="eb7eb-202">Microsoft Visual Studio 2013 and Microsoft Visual Studio Express 2013 for Web will often be referred to as Visual Studio throughout this tutorial series.</span></span>


<span data-ttu-id="eb7eb-203">Se você já tiver uma versão do Visual Studio instalada, o processo de instalação instalará o Visual Studio 2013 ou Microsoft Visual Studio Express 2013 para Web ao lado da versão existente.</span><span class="sxs-lookup"><span data-stu-id="eb7eb-203">If you already have a Visual Studio version installed, the installation process will install Visual Studio 2013 or Microsoft Visual Studio Express 2013 for Web next to the existing version.</span></span> <span data-ttu-id="eb7eb-204">Sites criados em versões anteriores podem ser abertos no Visual Studio 2013 e continuam a abrir nas versões anteriores.</span><span class="sxs-lookup"><span data-stu-id="eb7eb-204">Sites that you created in earlier versions can be opened in Visual Studio 2013 and continue to open in previous versions.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="eb7eb-205">Este passo a passo pressupõe que você selecionou a *desenvolvimento Web* coleção de configurações na primeira vez que você iniciou o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="eb7eb-205">This walkthrough assumes that you selected the *Web Development* collection of settings the first time that you started Visual Studio.</span></span> <span data-ttu-id="eb7eb-206">Para obter mais informações, consulte [como: selecionar configurações de ambiente de desenvolvimento da Web](https://msdn.microsoft.com/library/ff521558.aspx).</span><span class="sxs-lookup"><span data-stu-id="eb7eb-206">For more information, see [How to: Select Web Development Environment Settings](https://msdn.microsoft.com/library/ff521558.aspx).</span></span>


## <a name="download-the-sample-application"></a><span data-ttu-id="eb7eb-207">Baixe o aplicativo de exemplo</span><span class="sxs-lookup"><span data-stu-id="eb7eb-207">Download the Sample Application</span></span>

<span data-ttu-id="eb7eb-208">Depois de instalar os pré-requisitos, você está pronto para começar a criar o novo projeto Web que é apresentado nessa série de tutoriais.</span><span class="sxs-lookup"><span data-stu-id="eb7eb-208">After installing the prerequisites, you are ready to begin creating the new Web project that is presented in this tutorial series.</span></span> <span data-ttu-id="eb7eb-209">Se você quiser **opcionalmente** executar o aplicativo de exemplo que cria esta série de tutoriais, você pode baixá-lo do site de exemplos do MSDN.</span><span class="sxs-lookup"><span data-stu-id="eb7eb-209">If you would like to **optionally** run the sample application that this tutorial series creates, you can download it from the MSDN Samples site.</span></span> <span data-ttu-id="eb7eb-210">Este download contém o seguinte:</span><span class="sxs-lookup"><span data-stu-id="eb7eb-210">This download contains the following:</span></span>

- <span data-ttu-id="eb7eb-211">O aplicativo de exemplo na *WingtipToys* pasta.</span><span class="sxs-lookup"><span data-stu-id="eb7eb-211">The sample application in the *WingtipToys* folder.</span></span>
- <span data-ttu-id="eb7eb-212">Os recursos usados para criar o aplicativo de exemplo na *ativos WingtipToys* pasta na *WingtipToys* pasta.</span><span class="sxs-lookup"><span data-stu-id="eb7eb-212">The resources used to create the sample application in the *WingtipToys-Assets* folder in the *WingtipToys* folder.</span></span>

#### <a name="download-the-file-from-msdn-samples-site"></a><span data-ttu-id="eb7eb-213">Baixe o arquivo do site de exemplos do MSDN:</span><span class="sxs-lookup"><span data-stu-id="eb7eb-213">Download the file from MSDN Samples site:</span></span>

<span data-ttu-id="eb7eb-214">[Introdução ao ASP.NET 4.5 do Web Forms e Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (c#)</span><span class="sxs-lookup"><span data-stu-id="eb7eb-214">[Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span></span> 

<span data-ttu-id="eb7eb-215">O download é um <em>. zip</em> arquivo.</span><span class="sxs-lookup"><span data-stu-id="eb7eb-215">The download is a <em>.zip</em> file.</span></span> <span data-ttu-id="eb7eb-216">Para ver o projeto completo que cria esta série de tutoriais, encontre e selecione os <em>c#</em>pasta o <em>. zip</em> arquivo.</span><span class="sxs-lookup"><span data-stu-id="eb7eb-216">To see the completed project that this tutorial series creates, find and select the <em>C#</em>folder in the <em>.zip</em> file.</span></span> <span data-ttu-id="eb7eb-217">Salvar a <em>c#</em> folderto a pasta que você pode usar para trabalhar com projetos do Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="eb7eb-217">Save the <em>C#</em> folderto the folder you use to work with Visual Studio 2013 projects.</span></span> <span data-ttu-id="eb7eb-218">Por padrão, a pasta de projetos do Visual Studio 2013 é o seguinte:</span><span class="sxs-lookup"><span data-stu-id="eb7eb-218">By default, the Visual Studio 2013 projects folder is the following:</span></span>

<span data-ttu-id="eb7eb-219"><strong>C:\Users&#92;</strong><strong><em>&lt;username&gt;</em></strong><strong>\Documents\Visual Studio 2013\Projects</strong></span><span class="sxs-lookup"><span data-stu-id="eb7eb-219"><strong>C:\Users&#92;</strong><strong><em>&lt;username&gt;</em></strong><strong>\Documents\Visual Studio 2013\Projects</strong></span></span>

<span data-ttu-id="eb7eb-220">Renomeie o ***c#*** pasta a ser ***WingtipToys***.</span><span class="sxs-lookup"><span data-stu-id="eb7eb-220">Rename the ***C#*** folder to ***WingtipToys***.</span></span>

> [!NOTE]
> <span data-ttu-id="eb7eb-221">Se você já tiver uma pasta chamada *WingtipToys* na sua pasta de projetos, renomeie temporariamente essa pasta existente antes de renomear o *c#* pasta a ser *WingtipToys*.</span><span class="sxs-lookup"><span data-stu-id="eb7eb-221">If you already have a folder named *WingtipToys* in your Projects folder, temporarily rename that existing folder before renaming the *C#* folder to *WingtipToys*.</span></span>


<span data-ttu-id="eb7eb-222">Para executar o projeto concluído, abra o *WingtipToys* pasta e clique duas vezes o *WingtipToys.sln* arquivo.</span><span class="sxs-lookup"><span data-stu-id="eb7eb-222">To run the completed project, open the *WingtipToys* folder and double-click the *WingtipToys.sln* file.</span></span> <span data-ttu-id="eb7eb-223">Visual Studio 2013 abrirá o projeto.</span><span class="sxs-lookup"><span data-stu-id="eb7eb-223">Visual Studio 2013 will open the project.</span></span> <span data-ttu-id="eb7eb-224">Em seguida, clique com botão direito do *default. aspx* do arquivo na janela do Gerenciador de soluções e clique em Exibir no navegador do menu de atalho.</span><span class="sxs-lookup"><span data-stu-id="eb7eb-224">Next, right-click the *Default.aspx* file in the Solution Explorer window and click View In Browser from the right-click menu.</span></span>

### <a name="tutorial-support-and-comments"></a><span data-ttu-id="eb7eb-225">Comentários e suporte de tutorial</span><span class="sxs-lookup"><span data-stu-id="eb7eb-225">Tutorial Support and Comments</span></span>

<span data-ttu-id="eb7eb-226">Use a seção de p e r incluída com o [Introdução ao Web Forms do ASP.NET 4.5 e Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (c#) exemplo tiver perguntas ou comentários.</span><span class="sxs-lookup"><span data-stu-id="eb7eb-226">Use the Q AND A section included with the [Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) sample for any questions or comments.</span></span>

<span data-ttu-id="eb7eb-227">Comentários sobre esta série de tutoriais são boas-vindos e quando esta série de tutoriais é atualizada todos os esforços serão feito para correções de conta ou sugestões de melhorias que são fornecidas nos comentários tutoriais.</span><span class="sxs-lookup"><span data-stu-id="eb7eb-227">Comments on this tutorial series are welcome, and when this tutorial series is updated every effort will be made to take into account corrections or suggestions for improvements that are provided in the tutorial comments.</span></span>

<span data-ttu-id="eb7eb-228">Quando ocorrer um erro durante o desenvolvimento, ou se o site da Web não é executado corretamente, as mensagens de erro podem fornecer pistas complexas para a origem do problema ou não podem explicar como corrigi-lo.</span><span class="sxs-lookup"><span data-stu-id="eb7eb-228">When an error happens during development, or if the Web site does not run correctly, the error messages may give complex clues to the source of the problem or might not explain how to fix it.</span></span> <span data-ttu-id="eb7eb-229">Para ajudar você com alguns cenários comuns de problema, você também pode usar o [fóruns do ASP.NET](https://forums.asp.net/) ou na seção de p e r incluídas com o [Introdução ao Web Forms do ASP.NET 4.5 e Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) ( C#) exemplo.</span><span class="sxs-lookup"><span data-stu-id="eb7eb-229">To help you with some common problem scenarios, you can also use the [ASP.NET forums](https://forums.asp.net/) or the Q AND A section included with the [Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) sample.</span></span> <span data-ttu-id="eb7eb-230">Se você receber uma mensagem de erro ou se algo não funciona ao percorrer os tutoriais, certifique-se de verificar os locais acima.</span><span class="sxs-lookup"><span data-stu-id="eb7eb-230">If you get an error message or something doesn't work as you go through the tutorials, be sure to check the above locations.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="eb7eb-231">Avançar</span><span class="sxs-lookup"><span data-stu-id="eb7eb-231">Next</span></span>](create-the-project.md)

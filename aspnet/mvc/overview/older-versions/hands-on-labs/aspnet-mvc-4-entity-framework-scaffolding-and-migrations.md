---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
title: "Estrutura do ASP.NET MVC 4 Entity Framework e migrações | Microsoft Docs"
author: rick-anderson
description: "Se você estiver familiarizado com os métodos de controlador do ASP.NET MVC 4 ou concluiu a &quot;auxiliares, formulários e validação&quot; laboratório prático, você deve estar ciente..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 093c1362-f10b-407c-a708-be370f4b62b0
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
msc.type: authoredcontent
ms.openlocfilehash: 15db1589eb90739458b430c35cea38e93e3dec5a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-mvc-4-entity-framework-scaffolding-and-migrations"></a><span data-ttu-id="17452-103">As migrações e estrutura do ASP.NET MVC 4 Entity Framework</span><span class="sxs-lookup"><span data-stu-id="17452-103">ASP.NET MVC 4 Entity Framework Scaffolding and Migrations</span></span>
====================
<span data-ttu-id="17452-104">por [Web Camps Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="17452-104">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

> <span data-ttu-id="17452-105">Se você estiver familiarizado com os métodos de controlador do ASP.NET MVC 4 ou concluiu a &quot;auxiliares, formulários e validação&quot; laboratório prático, você deve estar ciente de que muitos a lógica para criar, atualizar, lista e remover qualquer entidade de dados é repetido entre o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="17452-105">If you are familiar with ASP.NET MVC 4 controller methods, or have completed the &quot;Helpers, Forms and Validation&quot; Hands-On lab, you should be aware that many of the logic to create, update, list and remove any data entity it is repeated among the application.</span></span> <span data-ttu-id="17452-106">Para não mencionar que, se seu modelo tiver várias classes para manipular, será provável gastam um tempo considerável gravar os métodos de ação POST e GET para cada operação de entidade, bem como cada um dos modos de exibição.</span><span class="sxs-lookup"><span data-stu-id="17452-106">Not to mention that, if your model has several classes to manipulate, you will be likely to spend a considerable time writing the POST and GET action methods for each entity operation, as well as each of the views.</span></span>
> 
> <span data-ttu-id="17452-107">Neste laboratório, você aprenderá como usar o ASP.NET MVC 4 de scaffolding para gerar automaticamente a linha de base de CRUD seu aplicativo (criar, ler, atualizar e excluir).</span><span class="sxs-lookup"><span data-stu-id="17452-107">In this lab you will learn how to use the ASP.NET MVC 4 scaffolding to automatically generate the baseline of your application's CRUD (Create, Read, Update and Delete).</span></span> <span data-ttu-id="17452-108">A partir de uma classe de modelo simples e, sem escrever uma única linha de código, você criará um controlador que contém todas as operações CRUD, bem como as todas as necessárias exibições.</span><span class="sxs-lookup"><span data-stu-id="17452-108">Starting from a simple model class, and, without writing a single line of code, you will create a controller that will contain all the CRUD operations, as well as the all the necessary views.</span></span> <span data-ttu-id="17452-109">Após criar e executar a solução mais simples, você terá o banco de dados de aplicativo gerado, juntamente com a lógica MVC e modos de exibição para a manipulação de dados.</span><span class="sxs-lookup"><span data-stu-id="17452-109">After building and running the simple solution, you will have the application database generated, together with the MVC logic and views for data manipulation.</span></span>
> 
> <span data-ttu-id="17452-110">Além disso, você aprenderá como é fácil usar o Entity Framework migrações para executar atualizações de modelo em todo o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="17452-110">In addition, you will learn how easy it is to use Entity Framework Migrations to perform model updates throughout your entire application.</span></span> <span data-ttu-id="17452-111">Migrações do Entity Framework permitem que você modificar o banco de dados depois que o modelo foi alterado com etapas simples.</span><span class="sxs-lookup"><span data-stu-id="17452-111">Entity Framework Migrations will let you modify your database after the model has changed with simple steps.</span></span> <span data-ttu-id="17452-112">Com todos esses em mente, você poderá criar e manter os aplicativos web com mais eficiência, aproveitando os recursos mais recentes do ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="17452-112">With all these in mind, you will be able to build and maintain web applications more efficiently, taking advantage of the latest features of ASP.NET MVC 4.</span></span>


<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="17452-113">Objetivos</span><span class="sxs-lookup"><span data-stu-id="17452-113">Objectives</span></span>

<span data-ttu-id="17452-114">Neste laboratório prático, você aprenderá como:</span><span class="sxs-lookup"><span data-stu-id="17452-114">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="17452-115">Use ASP.NET scaffolding para operações CRUD de controladores.</span><span class="sxs-lookup"><span data-stu-id="17452-115">Use ASP.NET scaffolding for CRUD operations in controllers.</span></span>
- <span data-ttu-id="17452-116">Altere o modelo de banco de dados usando o Entity Framework migrações.</span><span class="sxs-lookup"><span data-stu-id="17452-116">Change the database model using Entity Framework Migrations.</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="17452-117">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="17452-117">Prerequisites</span></span>

<span data-ttu-id="17452-118">Você deve ter os seguintes itens para concluir este laboratório:</span><span class="sxs-lookup"><span data-stu-id="17452-118">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="17452-119">[Microsoft Visual Studio Express 2012 para Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) ou superior (leitura [Apêndice A](#AppendixA) para obter instruções sobre como instalá-lo).</span><span class="sxs-lookup"><span data-stu-id="17452-119">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="17452-120">Configuração</span><span class="sxs-lookup"><span data-stu-id="17452-120">Setup</span></span>

<span data-ttu-id="17452-121">**Instalando os trechos de código**</span><span class="sxs-lookup"><span data-stu-id="17452-121">**Installing Code Snippets**</span></span>

<span data-ttu-id="17452-122">Para sua conveniência, boa parte do código que você estiver gerenciando ao longo deste laboratório está disponível como trechos de código do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="17452-122">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="17452-123">Para instalar os trechos de código executar **.\Source\Setup\CodeSnippets.vsi** arquivo.</span><span class="sxs-lookup"><span data-stu-id="17452-123">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="17452-124">Se você não estiver familiarizado com os trechos de código do Visual Studio e aprender como usá-los, consulte o Apêndice deste documento &quot; [trechos de código usando do apêndice b:](#AppendixB)&quot;.</span><span class="sxs-lookup"><span data-stu-id="17452-124">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix B: Using Code Snippets](#AppendixB)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="17452-125">Exercícios</span><span class="sxs-lookup"><span data-stu-id="17452-125">Exercises</span></span>

<span data-ttu-id="17452-126">O exercício a seguir compõem esse laboratório prático:</span><span class="sxs-lookup"><span data-stu-id="17452-126">The following exercise make up this Hands-On Lab:</span></span>

1. [<span data-ttu-id="17452-127">Usando o ASP.NET MVC 4 de Scaffolding com migrações do Entity Framework</span><span class="sxs-lookup"><span data-stu-id="17452-127">Using ASP.NET MVC 4 Scaffolding with Entity Framework Migrations</span></span>](#Exercise1)

> [!NOTE]
> <span data-ttu-id="17452-128">Este exercício é acompanhado por um **final** pasta que contém a solução resultante, você deve obter depois de concluir o exercício.</span><span class="sxs-lookup"><span data-stu-id="17452-128">This exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercise.</span></span> <span data-ttu-id="17452-129">Você pode usar essa solução como um guia se você precisar de ajuda adicional, trabalhando no exercício.</span><span class="sxs-lookup"><span data-stu-id="17452-129">You can use this solution as a guide if you need additional help working through the exercise.</span></span>


<span data-ttu-id="17452-130">Tempo estimado para concluir este laboratório: **30 minutos**</span><span class="sxs-lookup"><span data-stu-id="17452-130">Estimated time to complete this lab: **30 minutes**</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Using_ASPNET_MVC_4_Scaffolding_with_Entity_Framework_Migrations"></a>
### <a name="exercise-1-using-aspnet-mvc-4-scaffolding-with-entity-framework-migrations"></a><span data-ttu-id="17452-131">Exercício 1: Usando o ASP.NET MVC 4 de Scaffolding com migrações do Entity Framework</span><span class="sxs-lookup"><span data-stu-id="17452-131">Exercise 1: Using ASP.NET MVC 4 Scaffolding with Entity Framework Migrations</span></span>

<span data-ttu-id="17452-132">Estrutura do ASP.NET MVC fornece uma maneira rápida para gerar as operações CRUD de uma maneira padronizada, criando a lógica necessária que permite que seu aplicativo interaja com a camada de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="17452-132">ASP.NET MVC scaffolding provides a quick way to generate the CRUD operations in a standardized way, creating the necessary logic that lets your application interact with the database layer.</span></span>

<span data-ttu-id="17452-133">Neste exercício, você aprenderá como usar scaffolding do ASP.NET MVC 4 com o código primeiro para criar os métodos CRUD.</span><span class="sxs-lookup"><span data-stu-id="17452-133">In this exercise, you will learn how to use ASP.NET MVC 4 scaffolding with code first to create the CRUD methods.</span></span> <span data-ttu-id="17452-134">Em seguida, você aprenderá como atualizar seu modelo aplicando as alterações no banco de dados usando o Entity Framework migrações.</span><span class="sxs-lookup"><span data-stu-id="17452-134">Then, you will learn how to update your model applying the changes in the database by using Entity Framework Migrations.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1-_Creating_a_new_ASPNET_MVC_4_project_using_Scaffolding"></a>
#### <a name="task-1--creating-a-new-aspnet-mvc-4-project-using-scaffolding"></a><span data-ttu-id="17452-135">Tarefa 1-criar um novo do ASP.NET MVC 4 projeto usando o Scaffolding</span><span class="sxs-lookup"><span data-stu-id="17452-135">Task 1- Creating a new ASP.NET MVC 4 project using Scaffolding</span></span>

1. <span data-ttu-id="17452-136">Se não estiver aberto, inicie **Visual Studio 2012**.</span><span class="sxs-lookup"><span data-stu-id="17452-136">If not already open, start **Visual Studio 2012**.</span></span>
2. <span data-ttu-id="17452-137">Selecione **arquivo | Novo projeto**.</span><span class="sxs-lookup"><span data-stu-id="17452-137">Select **File | New Project**.</span></span> <span data-ttu-id="17452-138">No novo projeto em caixa de diálogo, o **Visual c# | Web** seção, selecione **aplicativo Web do ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="17452-138">In the New Project dialog, under the **Visual C# | Web** section, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="17452-139">Nomeie o projeto para **MVC4andEFMigrations** e defina o local para **Source\Ex1 UsingMVC4ScaffoldingEFMigrations** pasta deste laboratório.</span><span class="sxs-lookup"><span data-stu-id="17452-139">Name the project to **MVC4andEFMigrations** and set the location to **Source\Ex1-UsingMVC4ScaffoldingEFMigrations** folder of this lab.</span></span> <span data-ttu-id="17452-140">Definir o **nome da solução** para **começar** e certifique-se de **criar diretório para solução** é verificada.</span><span class="sxs-lookup"><span data-stu-id="17452-140">Set the **Solution name** to **Begin** and ensure **Create directory for solution** is checked.</span></span> <span data-ttu-id="17452-141">Clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="17452-141">Click **OK**.</span></span>

    <span data-ttu-id="17452-142">![Nova caixa de diálogo do projeto ASP.NET MVC 4](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image1.png "nova caixa de diálogo do projeto ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="17452-142">![New ASP.NET MVC 4 Project Dialog Box](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image1.png "New ASP.NET MVC 4 Project Dialog Box")</span></span>

    <span data-ttu-id="17452-143">*Nova caixa de diálogo do projeto ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="17452-143">*New ASP.NET MVC 4 Project Dialog Box*</span></span>
3. <span data-ttu-id="17452-144">No **novo projeto ASP.NET MVC 4** select da caixa de diálogo de **aplicativo de Internet** modelo e certifique-se de que **Razor** é selecionado **domecanismodeexibição**.</span><span class="sxs-lookup"><span data-stu-id="17452-144">In the **New ASP.NET MVC 4 Project** dialog box select the **Internet Application** template, and make sure that **Razor** is the selected **View engine**.</span></span> <span data-ttu-id="17452-145">Clique em **Okey** para criar o projeto.</span><span class="sxs-lookup"><span data-stu-id="17452-145">Click **OK** to create the project.</span></span>

    <span data-ttu-id="17452-146">![Novo aplicativo de Internet do ASP.NET MVC 4](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image2.png "novo aplicativo de Internet do ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="17452-146">![New ASP.NET MVC 4 Internet Application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image2.png "New ASP.NET MVC 4 Internet Application")</span></span>

    <span data-ttu-id="17452-147">*Novo aplicativo de Internet do ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="17452-147">*New ASP.NET MVC 4 Internet Application*</span></span>
4. <span data-ttu-id="17452-148">No Gerenciador de soluções, clique com botão direito **modelos** e selecione **adicionar | Classe** para criar uma classe simples de pessoa (POCO).</span><span class="sxs-lookup"><span data-stu-id="17452-148">In the Solution Explorer, right-click **Models** and select **Add | Class** to create a simple class person (POCO).</span></span> <span data-ttu-id="17452-149">Nomeie- **pessoa** e clique em **Okey**.</span><span class="sxs-lookup"><span data-stu-id="17452-149">Name it **Person** and click **OK**.</span></span>
5. <span data-ttu-id="17452-150">Abra a classe de pessoa e insira as propriedades a seguir.</span><span class="sxs-lookup"><span data-stu-id="17452-150">Open the Person class and insert the following properties.</span></span>

    <span data-ttu-id="17452-151">(Código de trecho - *ASP.NET MVC 4 e o Entity Framework migrações - Ex1 pessoa propriedades*)</span><span class="sxs-lookup"><span data-stu-id="17452-151">(Code Snippet - *ASP.NET MVC 4 and Entity Framework Migrations - Ex1 Person Properties*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample1.cs)]
6. <span data-ttu-id="17452-152">Clique em **compilar | Criar solução** para salvar as alterações e compilar o projeto.</span><span class="sxs-lookup"><span data-stu-id="17452-152">Click **Build | Build Solution** to save the changes and build the project.</span></span>

    <span data-ttu-id="17452-153">![Criando o aplicativo](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image3.png "criação do aplicativo")</span><span class="sxs-lookup"><span data-stu-id="17452-153">![Building the application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image3.png "Building the application")</span></span>

    <span data-ttu-id="17452-154">*Compilando o aplicativo*</span><span class="sxs-lookup"><span data-stu-id="17452-154">*Building the Application*</span></span>
7. <span data-ttu-id="17452-155">No Gerenciador de soluções, clique na pasta controladores e selecione **adicionar | Controlador**.</span><span class="sxs-lookup"><span data-stu-id="17452-155">In the Solution Explorer, right-click the controllers folder and select **Add | Controller**.</span></span>
8. <span data-ttu-id="17452-156">Nome do controlador *PersonController* e conclua o **opções de Scaffolding** com os seguintes valores.</span><span class="sxs-lookup"><span data-stu-id="17452-156">Name the controller *PersonController* and complete the **Scaffolding options** with the following values.</span></span>

    1. <span data-ttu-id="17452-157">No **modelo** lista suspensa, selecione o **controlador MVC com ações de leitura/gravação e modos de exibição, usando o Entity Framework** opção.</span><span class="sxs-lookup"><span data-stu-id="17452-157">In the **Template** drop-down list, select the **MVC controller with read/write actions and views, using Entity Framework** option.</span></span>
    2. <span data-ttu-id="17452-158">No **classe modelo** lista suspensa, selecione o **pessoa** classe.</span><span class="sxs-lookup"><span data-stu-id="17452-158">In the **Model class** drop-down list, select the **Person** class.</span></span>
    3. <span data-ttu-id="17452-159">No **classe de contexto de dados** lista, selecione  **&lt;novo contexto de dados... &gt;**.</span><span class="sxs-lookup"><span data-stu-id="17452-159">In the **Data Context class** list, select **&lt;New data context...&gt;**.</span></span> <span data-ttu-id="17452-160">Escolher qualquer nome e clique em **Okey**.</span><span class="sxs-lookup"><span data-stu-id="17452-160">Choose any name and click **OK**.</span></span>
    4. <span data-ttu-id="17452-161">No **exibições** suspensa lista, certifique-se de que **Razor** está selecionado.</span><span class="sxs-lookup"><span data-stu-id="17452-161">In the **Views** drop-down list, make sure that **Razor** is selected.</span></span>

    <span data-ttu-id="17452-162">![Adição do controlador de pessoa com scaffolding](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image4.png "adição do controlador de pessoa com scaffolding")</span><span class="sxs-lookup"><span data-stu-id="17452-162">![Adding the Person controller with scaffolding](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image4.png "Adding the Person controller with scaffolding")</span></span>

    <span data-ttu-id="17452-163">*Adição do controlador de pessoa com scaffolding*</span><span class="sxs-lookup"><span data-stu-id="17452-163">*Adding the Person controller with scaffolding*</span></span>
9. <span data-ttu-id="17452-164">Clique em **adicionar** para criar o novo controlador de pessoa com scaffolding.</span><span class="sxs-lookup"><span data-stu-id="17452-164">Click **Add** to create the new controller for Person with scaffolding.</span></span> <span data-ttu-id="17452-165">Você gerou as ações do controlador, bem como os modos de exibição.</span><span class="sxs-lookup"><span data-stu-id="17452-165">You have now generated the controller actions as well as the views.</span></span>

    <span data-ttu-id="17452-166">![Depois de criar o controlador de pessoa com scaffolding](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image5.png "depois de criar o controlador de pessoa com scaffolding")</span><span class="sxs-lookup"><span data-stu-id="17452-166">![After creating the Person controller with scaffolding](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image5.png "After creating the Person controller with scaffolding")</span></span>

    <span data-ttu-id="17452-167">*Depois de criar o controlador de pessoa com scaffolding*</span><span class="sxs-lookup"><span data-stu-id="17452-167">*After creating the Person controller with scaffolding*</span></span>
10. <span data-ttu-id="17452-168">Abra **PersonController** classe.</span><span class="sxs-lookup"><span data-stu-id="17452-168">Open **PersonController** class.</span></span> <span data-ttu-id="17452-169">Observe que os métodos de ação CRUD completos foi gerados automaticamente.</span><span class="sxs-lookup"><span data-stu-id="17452-169">Notice that the full CRUD action methods have been generated automatically.</span></span>

    <span data-ttu-id="17452-170">![No controlador de pessoa](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image6.png "controlador dentro a pessoa")</span><span class="sxs-lookup"><span data-stu-id="17452-170">![Inside the Person controller](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image6.png "Inside the Person controller")</span></span>

    <span data-ttu-id="17452-171">*No controlador de pessoa*</span><span class="sxs-lookup"><span data-stu-id="17452-171">*Inside the Person controller*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2-_Running_the_application"></a>
#### <a name="task-2--running-the-application"></a><span data-ttu-id="17452-172">Tarefa 2-executar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="17452-172">Task 2- Running the application</span></span>

<span data-ttu-id="17452-173">Neste ponto, o banco de dados ainda não foi criado.</span><span class="sxs-lookup"><span data-stu-id="17452-173">At this point, the database is not yet created.</span></span> <span data-ttu-id="17452-174">Nesta tarefa, você irá executar o aplicativo pela primeira vez e testar as operações CRUD.</span><span class="sxs-lookup"><span data-stu-id="17452-174">In this task, you will run the application for the first time and test the CRUD operations.</span></span> <span data-ttu-id="17452-175">O banco de dados será criado dinamicamente com o Code First.</span><span class="sxs-lookup"><span data-stu-id="17452-175">The database will be created on the fly with Code First.</span></span>

1. <span data-ttu-id="17452-176">Pressione **F5** para executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="17452-176">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="17452-177">No navegador, adicionar **/Person** para a URL para abrir a página da pessoa.</span><span class="sxs-lookup"><span data-stu-id="17452-177">In the browser, add **/Person** to the URL to open the Person page.</span></span>

    <span data-ttu-id="17452-178">![Primeira execução do aplicativo](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image7.png "primeira execução do aplicativo")</span><span class="sxs-lookup"><span data-stu-id="17452-178">![Application first run](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image7.png "Application first run")</span></span>

    <span data-ttu-id="17452-179">*O aplicativo: primeiro execute*</span><span class="sxs-lookup"><span data-stu-id="17452-179">*Application: first run*</span></span>
3. <span data-ttu-id="17452-180">Agora você explorar as páginas de pessoa e testar as operações CRUD.</span><span class="sxs-lookup"><span data-stu-id="17452-180">You will now explore the Person pages and test the CRUD operations.</span></span>

    1. <span data-ttu-id="17452-181">Clique em **criar novo** para adicionar uma nova pessoa.</span><span class="sxs-lookup"><span data-stu-id="17452-181">Click **Create New** to add a new person.</span></span> <span data-ttu-id="17452-182">Insira um nome e um sobrenome e clique em **criar**.</span><span class="sxs-lookup"><span data-stu-id="17452-182">Enter a first name and a last name and click **Create**.</span></span>

        <span data-ttu-id="17452-183">![Adicionando uma nova pessoa](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image8.png "adicionando uma nova pessoa")</span><span class="sxs-lookup"><span data-stu-id="17452-183">![Adding a new person](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image8.png "Adding a new person")</span></span>

        <span data-ttu-id="17452-184">*Adicionando uma nova pessoa*</span><span class="sxs-lookup"><span data-stu-id="17452-184">*Adding a new person*</span></span>
    2. <span data-ttu-id="17452-185">Na lista da pessoa, você pode excluir, editar ou adicionar itens.</span><span class="sxs-lookup"><span data-stu-id="17452-185">In the person's list, you can delete, edit or add items.</span></span>

        <span data-ttu-id="17452-186">![lista de pessoas](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image9.png "lista de pessoas")</span><span class="sxs-lookup"><span data-stu-id="17452-186">![person list](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image9.png "person list")</span></span>

        <span data-ttu-id="17452-187">*Lista de pessoas*</span><span class="sxs-lookup"><span data-stu-id="17452-187">*Person list*</span></span>
    3. <span data-ttu-id="17452-188">Clique em **detalhes** para abrir os detalhes da pessoa.</span><span class="sxs-lookup"><span data-stu-id="17452-188">Click **Details** to open the person's details.</span></span>

        <span data-ttu-id="17452-189">![Detalhes da pessoa](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image10.png "detalhes da pessoa")</span><span class="sxs-lookup"><span data-stu-id="17452-189">![Person's details](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image10.png "Person's details")</span></span>

        <span data-ttu-id="17452-190">*Detalhes da pessoa*</span><span class="sxs-lookup"><span data-stu-id="17452-190">*Person's details*</span></span>
4. <span data-ttu-id="17452-191">Feche o navegador e retorne ao Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="17452-191">Close the browser and return to Visual Studio.</span></span> <span data-ttu-id="17452-192">Observe que você criou o CRUD inteira para essa entidade em seu aplicativo - do modelo para os modos de exibição - sem a necessidade de escrever uma única linha de código!</span><span class="sxs-lookup"><span data-stu-id="17452-192">Notice that you have created the whole CRUD for the person entity throughout your application -from the model to the views- without having to write a single line of code!</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3-_Updating_the_database_using_Entity_Framework_Migrations"></a>
#### <a name="task-3--updating-the-database-using-entity-framework-migrations"></a><span data-ttu-id="17452-193">Tarefa 3-atualizando o banco de dados usando o Entity Framework migrações</span><span class="sxs-lookup"><span data-stu-id="17452-193">Task 3- Updating the database using Entity Framework Migrations</span></span>

<span data-ttu-id="17452-194">Nesta tarefa, você atualizará o banco de dados usando o Entity Framework migrações.</span><span class="sxs-lookup"><span data-stu-id="17452-194">In this task you will update the database using Entity Framework Migrations.</span></span> <span data-ttu-id="17452-195">Você descobrirá como é fácil alterar o modelo e refletir as alterações nos bancos de dados usando o recurso de migrações do Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="17452-195">You will discover how easy it is to change the model and reflect the changes in your databases by using the Entity Framework Migrations feature.</span></span>

1. <span data-ttu-id="17452-196">Abra o Console do Gerenciador de pacotes.</span><span class="sxs-lookup"><span data-stu-id="17452-196">Open the Package Manager Console.</span></span> <span data-ttu-id="17452-197">Selecione **ferramentas | Gerenciador de biblioteca de pacote | Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="17452-197">Select **Tools | Library Package Manager | Package Manager Console**.</span></span>
2. <span data-ttu-id="17452-198">No Console do Gerenciador de pacotes, digite o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="17452-198">In the Package Manager Console, enter the following command:</span></span>

    <span data-ttu-id="17452-199">PMC</span><span class="sxs-lookup"><span data-stu-id="17452-199">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample2.ps1)]

    <span data-ttu-id="17452-200">![Habilitar migrações](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image11.png "habilitar migrações")</span><span class="sxs-lookup"><span data-stu-id="17452-200">![Enabling Migrations](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image11.png "Enabling Migrations")</span></span>

    <span data-ttu-id="17452-201">*Habilitar migrações*</span><span class="sxs-lookup"><span data-stu-id="17452-201">*Enabling migrations*</span></span>

    <span data-ttu-id="17452-202">O comando de permitir a migração cria o **migrações** pasta, que contém um script para inicializar o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="17452-202">The Enable-Migration command creates the **Migrations** folder, which contains a script to initialize the database.</span></span>

    <span data-ttu-id="17452-203">![Pasta Migrations](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image12.png "pasta Migrations")</span><span class="sxs-lookup"><span data-stu-id="17452-203">![Migrations folder](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image12.png "Migrations folder")</span></span>

    <span data-ttu-id="17452-204">*Pasta de migrações*</span><span class="sxs-lookup"><span data-stu-id="17452-204">*Migrations folder*</span></span>
3. <span data-ttu-id="17452-205">Abra o **Configuration.cs** arquivo na pasta de migrações.</span><span class="sxs-lookup"><span data-stu-id="17452-205">Open the **Configuration.cs** file in the Migrations folder.</span></span> <span data-ttu-id="17452-206">Localize o construtor da classe e altere o **AutomaticMigrationsEnabled** valor *true*.</span><span class="sxs-lookup"><span data-stu-id="17452-206">Locate the class constructor and change the **AutomaticMigrationsEnabled** value to *true*.</span></span>


    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample3.cs)]
4. <span data-ttu-id="17452-207">Abra a classe Person e adicione um atributo de nome do meio da pessoa.</span><span class="sxs-lookup"><span data-stu-id="17452-207">Open the Person class and add an attribute for the person's middle name.</span></span> <span data-ttu-id="17452-208">Com esse novo atributo, você está alterando o modelo.</span><span class="sxs-lookup"><span data-stu-id="17452-208">With this new attribute, you are changing the model.</span></span>


    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample4.cs)]
5. <span data-ttu-id="17452-209">Selecione **compilar | Criar solução** no menu compilar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="17452-209">Select **Build | Build Solution** on the menu to build the application.</span></span>

    <span data-ttu-id="17452-210">![Criando o aplicativo](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image13.png "criação do aplicativo")</span><span class="sxs-lookup"><span data-stu-id="17452-210">![Building the application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image13.png "Building the application")</span></span>

    <span data-ttu-id="17452-211">*Compilando o aplicativo*</span><span class="sxs-lookup"><span data-stu-id="17452-211">*Building the application*</span></span>
6. <span data-ttu-id="17452-212">No Console do Gerenciador de pacotes, digite o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="17452-212">In the Package Manager Console, enter the following command:</span></span>

    <span data-ttu-id="17452-213">PMC</span><span class="sxs-lookup"><span data-stu-id="17452-213">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample5.ps1)]

    <span data-ttu-id="17452-214">Este comando vai procurar alterações nos objetos de dados e, em seguida, ele adicionará os comandos necessários para modificar o banco de dados adequadamente.</span><span class="sxs-lookup"><span data-stu-id="17452-214">This command will look for changes in the data objects, and then, it will add the necessary commands to modify the database accordingly.</span></span>

    <span data-ttu-id="17452-215">![Adicionando um nome do meio](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image14.png "adicionando um nome do meio")</span><span class="sxs-lookup"><span data-stu-id="17452-215">![Adding a middle name](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image14.png "Adding a middle name")</span></span>

    <span data-ttu-id="17452-216">*Adicionando um nome do meio*</span><span class="sxs-lookup"><span data-stu-id="17452-216">*Adding a middle name*</span></span>
7. <span data-ttu-id="17452-217">(Opcional) Você pode executar o seguinte comando para gerar um script SQL com a atualização diferencial.</span><span class="sxs-lookup"><span data-stu-id="17452-217">(Optional) You can run the following command to generate a SQL script with the differential update.</span></span> <span data-ttu-id="17452-218">Isso permitirá que você atualizar o banco de dados manualmente (nesse caso não é necessário), ou aplicar as alterações em outros bancos de dados:</span><span class="sxs-lookup"><span data-stu-id="17452-218">This will let you update the database manually (In this case it's not necessary), or apply the changes in other databases:</span></span>

    <span data-ttu-id="17452-219">PMC</span><span class="sxs-lookup"><span data-stu-id="17452-219">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample6.ps1)]

    <span data-ttu-id="17452-220">![Gerar um script SQL](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image15.png "gerar um script SQL")</span><span class="sxs-lookup"><span data-stu-id="17452-220">![Generating a SQL script](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image15.png "Generating a SQL script")</span></span>

    <span data-ttu-id="17452-221">*Gerar um script SQL*</span><span class="sxs-lookup"><span data-stu-id="17452-221">*Generating a SQL script*</span></span>

    <span data-ttu-id="17452-222">![Atualização do Script SQL](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image16.png "atualização do Script SQL")</span><span class="sxs-lookup"><span data-stu-id="17452-222">![SQL Script update](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image16.png "SQL Script update")</span></span>

    <span data-ttu-id="17452-223">*Atualização do Script SQL*</span><span class="sxs-lookup"><span data-stu-id="17452-223">*SQL Script update*</span></span>
8. <span data-ttu-id="17452-224">No Console do Gerenciador de pacotes, digite o seguinte comando para atualizar o banco de dados:</span><span class="sxs-lookup"><span data-stu-id="17452-224">In the Package Manager Console, enter the following command to update the database:</span></span>

    <span data-ttu-id="17452-225">PMC</span><span class="sxs-lookup"><span data-stu-id="17452-225">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample7.ps1)]

    <span data-ttu-id="17452-226">![Atualizando o banco de dados](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image17.png "Atualizar banco de dados")</span><span class="sxs-lookup"><span data-stu-id="17452-226">![Updating the Database](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image17.png "Updating the Database")</span></span>

    <span data-ttu-id="17452-227">*Atualizando o banco de dados*</span><span class="sxs-lookup"><span data-stu-id="17452-227">*Updating the Database*</span></span>

    <span data-ttu-id="17452-228">Isso adicionará o **MiddleName** coluna o **pessoas** tabela para coincidir com a definição atual do **pessoa** classe.</span><span class="sxs-lookup"><span data-stu-id="17452-228">This will add the **MiddleName** column in the **People** table to match the current definition of the **Person** class.</span></span>
9. <span data-ttu-id="17452-229">Depois que o banco de dados é atualizado, clique na pasta de controlador e selecione **adicionar | Controlador** para adicionar o controlador de pessoa novamente (completa com os mesmos valores).</span><span class="sxs-lookup"><span data-stu-id="17452-229">Once the database is updated, right-click the Controller folder and select **Add | Controller** to add the Person controller again (Complete with the same values).</span></span> <span data-ttu-id="17452-230">Isso atualizará os métodos existentes e as exibições para adicionar o novo atributo.</span><span class="sxs-lookup"><span data-stu-id="17452-230">This will update the existing methods and views adding the new attribute.</span></span>

    <span data-ttu-id="17452-231">![Adicionando uma atualização do controlador](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image18.png "adicionando uma atualização do controlador")</span><span class="sxs-lookup"><span data-stu-id="17452-231">![Adding a controller update](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image18.png "Adding a controller update")</span></span>

    <span data-ttu-id="17452-232">*Atualizar o controlador*</span><span class="sxs-lookup"><span data-stu-id="17452-232">*Updating the controller*</span></span>
10. <span data-ttu-id="17452-233">Clique em **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="17452-233">Click **Add**.</span></span> <span data-ttu-id="17452-234">Em seguida, selecione os valores **PersonController.cs substituir** e **substituir associados exibições** e clique em **Okey**.</span><span class="sxs-lookup"><span data-stu-id="17452-234">Then, select the values **Overwrite PersonController.cs** and the **Overwrite associated views** and click **OK**.</span></span>

    ![Adicionando uma substituição do controlador](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image19.png)

    <span data-ttu-id="17452-236">*Atualizar o controlador*</span><span class="sxs-lookup"><span data-stu-id="17452-236">*Updating the controller*</span></span>

<a id="Ex1Task4"></a>

<a id="Task4-_Running_the_application"></a>
#### <a name="task4--running-the-application"></a><span data-ttu-id="17452-237">Task4 - executando o aplicativo</span><span class="sxs-lookup"><span data-stu-id="17452-237">Task4- Running the application</span></span>

1. <span data-ttu-id="17452-238">Pressione **F5** para executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="17452-238">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="17452-239">Abra **/Person**.</span><span class="sxs-lookup"><span data-stu-id="17452-239">Open **/Person**.</span></span> <span data-ttu-id="17452-240">Observe que os dados foi preservados, enquanto a coluna de nome do meio foi adicionada.</span><span class="sxs-lookup"><span data-stu-id="17452-240">Notice that the data was preserved, while the middle name column was added.</span></span>

    <span data-ttu-id="17452-241">![Nome do meio adicionado](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image20.png "segundo nome adicionado")</span><span class="sxs-lookup"><span data-stu-id="17452-241">![Middle Name added](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image20.png "Middle Name added")</span></span>

    <span data-ttu-id="17452-242">*Nome do meio adicionado*</span><span class="sxs-lookup"><span data-stu-id="17452-242">*Middle Name added*</span></span>
3. <span data-ttu-id="17452-243">Se você clicar em **editar**, você poderá adicionar um segundo nome do usuário atual.</span><span class="sxs-lookup"><span data-stu-id="17452-243">If you click **Edit**, you will be able to add a middle name to the current person.</span></span>

    <span data-ttu-id="17452-244">![Nome do meio edição](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image21.png "edição do nome do meio")</span><span class="sxs-lookup"><span data-stu-id="17452-244">![Middle Name edition](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image21.png "Middle Name edition")</span></span>

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="17452-245">Resumo</span><span class="sxs-lookup"><span data-stu-id="17452-245">Summary</span></span>

<span data-ttu-id="17452-246">Este laboratório prático, você aprendeu etapas simples para criar operações CRUD com ASP.NET MVC 4 Scaffolding usando qualquer classe de modelo.</span><span class="sxs-lookup"><span data-stu-id="17452-246">In this Hands-On lab, you have learned simple steps to create CRUD operations with ASP.NET MVC 4 Scaffolding using any model class.</span></span> <span data-ttu-id="17452-247">Em seguida, você aprendeu como executar uma atualização de ponta a ponta em seu aplicativo - do banco de dados para os modos de exibição - usando o Entity Framework migrações.</span><span class="sxs-lookup"><span data-stu-id="17452-247">Then, you have learned how to perform an end to end update in your application -from the database to the views- by using Entity Framework Migrations.</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="17452-248">Apêndice a: instalar o Visual Studio Express 2012 para Web</span><span class="sxs-lookup"><span data-stu-id="17452-248">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="17452-249">Você pode instalar **Microsoft Visual Studio Express 2012 para Web** ou outro &quot;Express&quot; versão usando o  **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="17452-249">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="17452-250">As instruções a seguir guiá-lo pelas etapas necessárias para instalar o *Visual studio Express 2012 para Web* usando *Microsoft Web Platform Installer*.</span><span class="sxs-lookup"><span data-stu-id="17452-250">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="17452-251">Vá para [ [https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="17452-251">Go to [[https://go.microsoft.com/? linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="17452-252">Como alternativa, se você já tiver instalado o Web Platform Installer, você pode abri-la e procure o produto &quot; *Visual Studio Express 2012 para Web com o SDK do Windows Azure*&quot;.</span><span class="sxs-lookup"><span data-stu-id="17452-252">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;*Visual Studio Express 2012 for Web with Windows Azure SDK*&quot;.</span></span>
2. <span data-ttu-id="17452-253">Clique em **instalar agora**.</span><span class="sxs-lookup"><span data-stu-id="17452-253">Click on **Install Now**.</span></span> <span data-ttu-id="17452-254">Se você não tem **Web Platform Installer** você será redirecionado para baixar e instalá-lo primeiro.</span><span class="sxs-lookup"><span data-stu-id="17452-254">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="17452-255">Uma vez **Web Platform Installer** é aberto, clique em **instalar** para iniciar a instalação.</span><span class="sxs-lookup"><span data-stu-id="17452-255">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="17452-256">![Instalar Visual Studio Express](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image22.png "instalar Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="17452-256">![Install Visual Studio Express](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image22.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="17452-257">*Instalar Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="17452-257">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="17452-258">Leia os termos e licenças de todos os produtos e clique em **aceito** para continuar.</span><span class="sxs-lookup"><span data-stu-id="17452-258">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Aceitar os termos de licença](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image23.png)

    <span data-ttu-id="17452-260">*Aceitar os termos de licença*</span><span class="sxs-lookup"><span data-stu-id="17452-260">*Accepting the license terms*</span></span>
5. <span data-ttu-id="17452-261">Aguarde até que o processo de download e instalação seja concluída.</span><span class="sxs-lookup"><span data-stu-id="17452-261">Wait until the downloading and installation process completes.</span></span>

    ![Progresso da instalação](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image24.png)

    <span data-ttu-id="17452-263">*Progresso da instalação*</span><span class="sxs-lookup"><span data-stu-id="17452-263">*Installation progress*</span></span>
6. <span data-ttu-id="17452-264">Quando a instalação for concluída, clique em **concluir**.</span><span class="sxs-lookup"><span data-stu-id="17452-264">When the installation completes, click **Finish**.</span></span>

    ![Instalação concluída](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image25.png)

    <span data-ttu-id="17452-266">*Instalação concluída*</span><span class="sxs-lookup"><span data-stu-id="17452-266">*Installation completed*</span></span>
7. <span data-ttu-id="17452-267">Clique em **saída** para fechar o Web Platform Installer.</span><span class="sxs-lookup"><span data-stu-id="17452-267">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="17452-268">Para abrir o Visual Studio Express para Web, vá para o **iniciar** tela e comece a escrever &quot; **VS Express**&quot;, em seguida, clique no **VS Express para Web** lado a lado.</span><span class="sxs-lookup"><span data-stu-id="17452-268">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express para o bloco de Web](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image26.png)

    <span data-ttu-id="17452-270">*VS Express para o bloco de Web*</span><span class="sxs-lookup"><span data-stu-id="17452-270">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a><span data-ttu-id="17452-271">Apêndice b: usar trechos de código</span><span class="sxs-lookup"><span data-stu-id="17452-271">Appendix B: Using Code Snippets</span></span>

<span data-ttu-id="17452-272">Com trechos de código, você tem todo o código que é necessário ao seu alcance.</span><span class="sxs-lookup"><span data-stu-id="17452-272">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="17452-273">O documento de laboratório informará exatamente quando você pode usá-los, conforme mostrado na figura a seguir.</span><span class="sxs-lookup"><span data-stu-id="17452-273">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="17452-274">![Usando trechos de código do Visual Studio para inserir o código no seu projeto](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image27.png "trechos de código com o Visual Studio para inserir código em seu projeto")</span><span class="sxs-lookup"><span data-stu-id="17452-274">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image27.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="17452-275">*Usando trechos de código do Visual Studio para inserir código em seu projeto*</span><span class="sxs-lookup"><span data-stu-id="17452-275">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="17452-276">***Para adicionar um trecho de código usando o teclado (apenas c#)***</span><span class="sxs-lookup"><span data-stu-id="17452-276">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="17452-277">Posicione o cursor onde você deseja inserir o código.</span><span class="sxs-lookup"><span data-stu-id="17452-277">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="17452-278">Comece a digitar o nome do trecho (sem espaços ou hifens).</span><span class="sxs-lookup"><span data-stu-id="17452-278">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="17452-279">Observe como IntelliSense exibe os nomes dos trechos de código correspondentes.</span><span class="sxs-lookup"><span data-stu-id="17452-279">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="17452-280">Selecione o trecho correto (ou continue digitando até que o nome do trecho de código inteiro é selecionado).</span><span class="sxs-lookup"><span data-stu-id="17452-280">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="17452-281">Pressione a tecla Tab duas vezes para inserir o trecho de código no local do cursor.</span><span class="sxs-lookup"><span data-stu-id="17452-281">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="17452-282">![Comece a digitar o nome do fragmento](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image28.png "comece a digitar o nome do trecho de código")</span><span class="sxs-lookup"><span data-stu-id="17452-282">![Start typing the snippet name](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image28.png "Start typing the snippet name")</span></span>

<span data-ttu-id="17452-283">*Comece a digitar o nome do trecho de código*</span><span class="sxs-lookup"><span data-stu-id="17452-283">*Start typing the snippet name*</span></span>

<span data-ttu-id="17452-284">![Pressione Tab para selecionar o trecho de código realçado](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image29.png "pressione Tab para selecionar o trecho de código realçado")</span><span class="sxs-lookup"><span data-stu-id="17452-284">![Press Tab to select the highlighted snippet](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image29.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="17452-285">*Pressione Tab para selecionar o trecho de código realçado*</span><span class="sxs-lookup"><span data-stu-id="17452-285">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="17452-286">![Pressione Tab novamente e o trecho expandirá](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image30.png "expandirá pressione Tab novamente e o trecho de código")</span><span class="sxs-lookup"><span data-stu-id="17452-286">![Press Tab again and the snippet will expand](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image30.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="17452-287">*Pressione Tab novamente e o trecho de código serão expandida*</span><span class="sxs-lookup"><span data-stu-id="17452-287">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="17452-288">***Para adicionar um trecho de código usando o mouse (c#, Visual Basic e XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="17452-288">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="17452-289">Clique com botão direito em que você deseja inserir o trecho de código.</span><span class="sxs-lookup"><span data-stu-id="17452-289">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="17452-290">Selecione **Inserir trecho** seguido por **Meus trechos de código**.</span><span class="sxs-lookup"><span data-stu-id="17452-290">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="17452-291">Selecione o trecho relevante na lista, clicando nele.</span><span class="sxs-lookup"><span data-stu-id="17452-291">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="17452-292">![Com o botão direito em que você deseja inserir o trecho de código e selecione Inserir trecho](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image31.png "com o botão direito em que você deseja inserir o trecho de código e selecione Inserir trecho")</span><span class="sxs-lookup"><span data-stu-id="17452-292">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image31.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="17452-293">*Clique com botão direito em que você deseja inserir o trecho de código e selecione Inserir trecho*</span><span class="sxs-lookup"><span data-stu-id="17452-293">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="17452-294">![Selecione o trecho relevante na lista, clicando nele](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image32.png "escolher o trecho relevante na lista, clicando nele")</span><span class="sxs-lookup"><span data-stu-id="17452-294">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image32.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="17452-295">*Selecione o trecho relevante na lista, clicando nele*</span><span class="sxs-lookup"><span data-stu-id="17452-295">*Pick the relevant snippet from the list, by clicking on it*</span></span>

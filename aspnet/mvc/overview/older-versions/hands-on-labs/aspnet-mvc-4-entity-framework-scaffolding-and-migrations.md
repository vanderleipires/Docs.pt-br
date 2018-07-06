---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
title: Migrações e Scaffolding do Entity Framework do ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: Se você estiver familiarizado com os métodos do controlador ASP.NET MVC 4 ou tiver concluído a &quot;auxiliares, formulários e validação&quot; laboratório prático, você deve estar ciente...
ms.author: aspnetcontent
ms.date: 02/18/2013
ms.assetid: 093c1362-f10b-407c-a708-be370f4b62b0
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
msc.type: authoredcontent
ms.openlocfilehash: 31f593f294c4865f621a8556cb43d0d9c42f2660
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37814110"
---
# <a name="aspnet-mvc-4-entity-framework-scaffolding-and-migrations"></a><span data-ttu-id="2182d-103">Migrações e Scaffolding do Entity Framework do ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="2182d-103">ASP.NET MVC 4 Entity Framework Scaffolding and Migrations</span></span>

<span data-ttu-id="2182d-104">por [Web Camps equipe](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="2182d-104">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="2182d-105">Baixe o Kit de treinamento do Web Camps</span><span class="sxs-lookup"><span data-stu-id="2182d-105">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="2182d-106">Se você estiver familiarizado com os métodos do controlador ASP.NET MVC 4 ou tiver concluído a &quot;auxiliares, formulários e validação&quot; laboratório prático, você deve conhecer muitos da lógica para criar, atualizar, listar e remover qualquer entidade de dados que ele é repetido entre o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="2182d-106">If you are familiar with ASP.NET MVC 4 controller methods, or have completed the &quot;Helpers, Forms and Validation&quot; Hands-On lab, you should be aware that many of the logic to create, update, list and remove any data entity it is repeated among the application.</span></span> <span data-ttu-id="2182d-107">Para não mencionar que, se seu modelo tiver várias classes para manipular, será provável gastar um tempo considerável, escrever os métodos de ação POST e GET para cada operação de entidade, bem como cada um dos modos de exibição.</span><span class="sxs-lookup"><span data-stu-id="2182d-107">Not to mention that, if your model has several classes to manipulate, you will be likely to spend a considerable time writing the POST and GET action methods for each entity operation, as well as each of the views.</span></span>

<span data-ttu-id="2182d-108">Neste laboratório, você aprenderá como usar o scaffolding do ASP.NET MVC 4 para gerar automaticamente a linha de base de CRUD seu aplicativo (criar, ler, atualizar e excluir).</span><span class="sxs-lookup"><span data-stu-id="2182d-108">In this lab you will learn how to use the ASP.NET MVC 4 scaffolding to automatically generate the baseline of your application's CRUD (Create, Read, Update and Delete).</span></span> <span data-ttu-id="2182d-109">A partir de uma classe de modelo simples e, sem escrever uma única linha de código, você criará um controlador que irá conter todas as operações de CRUD, bem como as todas as necessárias exibições.</span><span class="sxs-lookup"><span data-stu-id="2182d-109">Starting from a simple model class, and, without writing a single line of code, you will create a controller that will contain all the CRUD operations, as well as the all the necessary views.</span></span> <span data-ttu-id="2182d-110">Depois de criar e executar a solução mais simples, você terá o banco de dados do aplicativo gerado, juntamente com a lógica MVC e os modos de exibição para manipulação de dados.</span><span class="sxs-lookup"><span data-stu-id="2182d-110">After building and running the simple solution, you will have the application database generated, together with the MVC logic and views for data manipulation.</span></span>

<span data-ttu-id="2182d-111">Além disso, você aprenderá como é fácil usar migrações do Entity Framework para executar atualizações de modelo ao longo de todo o seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="2182d-111">In addition, you will learn how easy it is to use Entity Framework Migrations to perform model updates throughout your entire application.</span></span> <span data-ttu-id="2182d-112">Migrações do Entity Framework permitem que você modifique seu banco de dados depois que o modelo foi alterado com etapas simples.</span><span class="sxs-lookup"><span data-stu-id="2182d-112">Entity Framework Migrations will let you modify your database after the model has changed with simple steps.</span></span> <span data-ttu-id="2182d-113">Com todos eles em mente, você poderá criar e manter aplicativos da web com mais eficiência, aproveitando os recursos mais recentes do ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="2182d-113">With all these in mind, you will be able to build and maintain web applications more efficiently, taking advantage of the latest features of ASP.NET MVC 4.</span></span>

> [!NOTE]
> <span data-ttu-id="2182d-114">Todo o código de exemplo e trechos de código são incluídos no Web Camps treinamento Kit, disponível no site da [versões Web/Microsoft-WebCampTrainingKit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="2182d-114">All sample code and snippets are included in the Web Camps Training Kit, available from at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="2182d-115">O projeto específico para este laboratório está disponível em [migrações e Scaffolding do ASP.NET MVC 4 Entity Framework](https://github.com/Microsoft-Web/HOL-EntityFrameworkScaffoldingAndMigrations).</span><span class="sxs-lookup"><span data-stu-id="2182d-115">The project specific to this lab is available at [ASP.NET MVC 4 Entity Framework Scaffolding and Migrations](https://github.com/Microsoft-Web/HOL-EntityFrameworkScaffoldingAndMigrations).</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="2182d-116">Objetivos</span><span class="sxs-lookup"><span data-stu-id="2182d-116">Objectives</span></span>

<span data-ttu-id="2182d-117">Neste laboratório prático, você aprenderá como:</span><span class="sxs-lookup"><span data-stu-id="2182d-117">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="2182d-118">Use o scaffolding do ASP.NET para operações CRUD em controladores.</span><span class="sxs-lookup"><span data-stu-id="2182d-118">Use ASP.NET scaffolding for CRUD operations in controllers.</span></span>
- <span data-ttu-id="2182d-119">Altere o modelo de banco de dados usando migrações do Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="2182d-119">Change the database model using Entity Framework Migrations.</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="2182d-120">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="2182d-120">Prerequisites</span></span>

<span data-ttu-id="2182d-121">Você deve ter os seguintes itens para concluir este laboratório:</span><span class="sxs-lookup"><span data-stu-id="2182d-121">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="2182d-122">[Microsoft Visual Studio Express 2012 para Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) ou superior (leia [Apêndice A](#AppendixA) para obter instruções sobre como instalá-lo).</span><span class="sxs-lookup"><span data-stu-id="2182d-122">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="2182d-123">Configuração</span><span class="sxs-lookup"><span data-stu-id="2182d-123">Setup</span></span>

<span data-ttu-id="2182d-124">**Instalando os trechos de código**</span><span class="sxs-lookup"><span data-stu-id="2182d-124">**Installing Code Snippets**</span></span>

<span data-ttu-id="2182d-125">Para sua conveniência, grande parte do código que você estiver gerenciando ao longo deste laboratório está disponível como trechos de código do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2182d-125">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="2182d-126">Para instalar os trechos de código executados **.\Source\Setup\CodeSnippets.vsi** arquivo.</span><span class="sxs-lookup"><span data-stu-id="2182d-126">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="2182d-127">Se você não estiver familiarizado com os trechos de código do Visual Studio e aprender como usá-los, consulte o Apêndice deste documento &quot; [apêndice b: usando o Code Snippets](#AppendixB)&quot;.</span><span class="sxs-lookup"><span data-stu-id="2182d-127">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix B: Using Code Snippets](#AppendixB)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="2182d-128">Exercícios</span><span class="sxs-lookup"><span data-stu-id="2182d-128">Exercises</span></span>

<span data-ttu-id="2182d-129">O exercício a seguir fazem parte deste laboratório prático:</span><span class="sxs-lookup"><span data-stu-id="2182d-129">The following exercise make up this Hands-On Lab:</span></span>

1. [<span data-ttu-id="2182d-130">Usando o Scaffolding do ASP.NET MVC 4 com migrações do Entity Framework</span><span class="sxs-lookup"><span data-stu-id="2182d-130">Using ASP.NET MVC 4 Scaffolding with Entity Framework Migrations</span></span>](#Exercise1)

> [!NOTE]
> <span data-ttu-id="2182d-131">Este exercício é acompanhado por um **final** pasta que contém a solução resultante, você deve obter depois de concluir o exercício.</span><span class="sxs-lookup"><span data-stu-id="2182d-131">This exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercise.</span></span> <span data-ttu-id="2182d-132">Você pode usar essa solução como um guia se você precisar de ajuda adicional, trabalhar com o exercício.</span><span class="sxs-lookup"><span data-stu-id="2182d-132">You can use this solution as a guide if you need additional help working through the exercise.</span></span>


<span data-ttu-id="2182d-133">Tempo estimado para concluir este laboratório: **30 minutos**</span><span class="sxs-lookup"><span data-stu-id="2182d-133">Estimated time to complete this lab: **30 minutes**</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Using_ASPNET_MVC_4_Scaffolding_with_Entity_Framework_Migrations"></a>
### <a name="exercise-1-using-aspnet-mvc-4-scaffolding-with-entity-framework-migrations"></a><span data-ttu-id="2182d-134">Exercício 1: Usando o Scaffolding do ASP.NET MVC 4 com migrações do Entity Framework</span><span class="sxs-lookup"><span data-stu-id="2182d-134">Exercise 1: Using ASP.NET MVC 4 Scaffolding with Entity Framework Migrations</span></span>

<span data-ttu-id="2182d-135">O scaffolding do ASP.NET MVC fornece uma maneira rápida para gerar as operações CRUD de uma maneira padronizada, criando a lógica necessária que permite que seu aplicativo interaja com a camada de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="2182d-135">ASP.NET MVC scaffolding provides a quick way to generate the CRUD operations in a standardized way, creating the necessary logic that lets your application interact with the database layer.</span></span>

<span data-ttu-id="2182d-136">Neste exercício, você aprenderá a usar scaffolding do ASP.NET MVC 4 com o código primeiro para criar os métodos CRUD.</span><span class="sxs-lookup"><span data-stu-id="2182d-136">In this exercise, you will learn how to use ASP.NET MVC 4 scaffolding with code first to create the CRUD methods.</span></span> <span data-ttu-id="2182d-137">Em seguida, você aprenderá a atualizar o modelo de aplicação das alterações no banco de dados por meio de migrações do Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="2182d-137">Then, you will learn how to update your model applying the changes in the database by using Entity Framework Migrations.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1-_Creating_a_new_ASPNET_MVC_4_project_using_Scaffolding"></a>
#### <a name="task-1--creating-a-new-aspnet-mvc-4-project-using-scaffolding"></a><span data-ttu-id="2182d-138">Projeto da tarefa 1-Criando um novo do ASP.NET MVC 4 usando o Scaffolding</span><span class="sxs-lookup"><span data-stu-id="2182d-138">Task 1- Creating a new ASP.NET MVC 4 project using Scaffolding</span></span>

1. <span data-ttu-id="2182d-139">Se não estiver aberto, inicie **Visual Studio 2012**.</span><span class="sxs-lookup"><span data-stu-id="2182d-139">If not already open, start **Visual Studio 2012**.</span></span>
2. <span data-ttu-id="2182d-140">Selecione **arquivo | Novo projeto**.</span><span class="sxs-lookup"><span data-stu-id="2182d-140">Select **File | New Project**.</span></span> <span data-ttu-id="2182d-141">No novo projeto na caixa de diálogo, o **Visual c# | Web** seção, selecione **aplicativo Web do ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="2182d-141">In the New Project dialog, under the **Visual C# | Web** section, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="2182d-142">Nomeie o projeto para **MVC4andEFMigrations** e defina o local como **Source\Ex1 UsingMVC4ScaffoldingEFMigrations** pasta deste laboratório.</span><span class="sxs-lookup"><span data-stu-id="2182d-142">Name the project to **MVC4andEFMigrations** and set the location to **Source\Ex1-UsingMVC4ScaffoldingEFMigrations** folder of this lab.</span></span> <span data-ttu-id="2182d-143">Definir a **nome da solução** para **começar** e verifique se **criar diretório para solução** é verificada.</span><span class="sxs-lookup"><span data-stu-id="2182d-143">Set the **Solution name** to **Begin** and ensure **Create directory for solution** is checked.</span></span> <span data-ttu-id="2182d-144">Clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="2182d-144">Click **OK**.</span></span>

    <span data-ttu-id="2182d-145">![Caixa de diálogo Novo projeto ASP.NET MVC 4](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image1.png "caixa de diálogo Novo projeto ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="2182d-145">![New ASP.NET MVC 4 Project Dialog Box](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image1.png "New ASP.NET MVC 4 Project Dialog Box")</span></span>

    <span data-ttu-id="2182d-146">*Caixa de diálogo Novo projeto ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="2182d-146">*New ASP.NET MVC 4 Project Dialog Box*</span></span>
3. <span data-ttu-id="2182d-147">No **novo projeto ASP.NET MVC 4** caixa de diálogo, selecione o **aplicativo de Internet** modelo e certifique-se de que **Razor** está selecionado **domecanismodeexibição**.</span><span class="sxs-lookup"><span data-stu-id="2182d-147">In the **New ASP.NET MVC 4 Project** dialog box select the **Internet Application** template, and make sure that **Razor** is the selected **View engine**.</span></span> <span data-ttu-id="2182d-148">Clique em **OK** para criar o projeto.</span><span class="sxs-lookup"><span data-stu-id="2182d-148">Click **OK** to create the project.</span></span>

    <span data-ttu-id="2182d-149">![Novo aplicativo de Internet do ASP.NET MVC 4](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image2.png "novo aplicativo de Internet do ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="2182d-149">![New ASP.NET MVC 4 Internet Application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image2.png "New ASP.NET MVC 4 Internet Application")</span></span>

    <span data-ttu-id="2182d-150">*Novo aplicativo de Internet do ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="2182d-150">*New ASP.NET MVC 4 Internet Application*</span></span>
4. <span data-ttu-id="2182d-151">No Gerenciador de soluções, clique com botão direito **modelos** e selecione **adicionar | Classe** para criar uma pessoa de classe simples (POCO).</span><span class="sxs-lookup"><span data-stu-id="2182d-151">In the Solution Explorer, right-click **Models** and select **Add | Class** to create a simple class person (POCO).</span></span> <span data-ttu-id="2182d-152">Denomine **pessoa** e clique em **Okey**.</span><span class="sxs-lookup"><span data-stu-id="2182d-152">Name it **Person** and click **OK**.</span></span>
5. <span data-ttu-id="2182d-153">Abra a classe Person e insira as propriedades a seguir.</span><span class="sxs-lookup"><span data-stu-id="2182d-153">Open the Person class and insert the following properties.</span></span>

    <span data-ttu-id="2182d-154">(Código de trecho de código – *ASP.NET MVC 4 e migrações do Entity Framework - propriedades de pessoas Ex1*)</span><span class="sxs-lookup"><span data-stu-id="2182d-154">(Code Snippet - *ASP.NET MVC 4 and Entity Framework Migrations - Ex1 Person Properties*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample1.cs)]
6. <span data-ttu-id="2182d-155">Clique em **compilar | Compilar solução** para salvar as alterações e compile o projeto.</span><span class="sxs-lookup"><span data-stu-id="2182d-155">Click **Build | Build Solution** to save the changes and build the project.</span></span>

    <span data-ttu-id="2182d-156">![Compilando o aplicativo](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image3.png "Compilando o aplicativo")</span><span class="sxs-lookup"><span data-stu-id="2182d-156">![Building the application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image3.png "Building the application")</span></span>

    <span data-ttu-id="2182d-157">*Compilando o aplicativo*</span><span class="sxs-lookup"><span data-stu-id="2182d-157">*Building the Application*</span></span>
7. <span data-ttu-id="2182d-158">No Gerenciador de soluções, clique com botão direito na pasta controladores e selecione **adicionar | Controlador**.</span><span class="sxs-lookup"><span data-stu-id="2182d-158">In the Solution Explorer, right-click the controllers folder and select **Add | Controller**.</span></span>
8. <span data-ttu-id="2182d-159">Nomeie o controlador *PersonController* e conclua a **opções de Scaffolding** com os seguintes valores.</span><span class="sxs-lookup"><span data-stu-id="2182d-159">Name the controller *PersonController* and complete the **Scaffolding options** with the following values.</span></span>

   1. <span data-ttu-id="2182d-160">No **modelo** lista suspensa, selecione o **controlador MVC com ações de leitura/gravação e modos de exibição, usando o Entity Framework** opção.</span><span class="sxs-lookup"><span data-stu-id="2182d-160">In the **Template** drop-down list, select the **MVC controller with read/write actions and views, using Entity Framework** option.</span></span>
   2. <span data-ttu-id="2182d-161">No **classe de modelo** lista suspensa, selecione o **pessoa** classe.</span><span class="sxs-lookup"><span data-stu-id="2182d-161">In the **Model class** drop-down list, select the **Person** class.</span></span>
   3. <span data-ttu-id="2182d-162">No **classe de contexto de dados** lista, selecione  **&lt;novo contexto de dados... &gt;**.</span><span class="sxs-lookup"><span data-stu-id="2182d-162">In the **Data Context class** list, select **&lt;New data context...&gt;**.</span></span> <span data-ttu-id="2182d-163">Escolha qualquer nome e clique em **Okey**.</span><span class="sxs-lookup"><span data-stu-id="2182d-163">Choose any name and click **OK**.</span></span>
   4. <span data-ttu-id="2182d-164">No **modos de exibição** suspensa lista, certifique-se de que **Razor** está selecionado.</span><span class="sxs-lookup"><span data-stu-id="2182d-164">In the **Views** drop-down list, make sure that **Razor** is selected.</span></span>

      <span data-ttu-id="2182d-165">![Adicionando o controlador de pessoa com scaffolding](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image4.png "adicionando o controlador de pessoa com scaffolding")</span><span class="sxs-lookup"><span data-stu-id="2182d-165">![Adding the Person controller with scaffolding](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image4.png "Adding the Person controller with scaffolding")</span></span>

      <span data-ttu-id="2182d-166">*Adicionando o controlador de pessoa com scaffolding*</span><span class="sxs-lookup"><span data-stu-id="2182d-166">*Adding the Person controller with scaffolding*</span></span>
9. <span data-ttu-id="2182d-167">Clique em **adicionar** para criar o novo controlador de pessoa com scaffolding.</span><span class="sxs-lookup"><span data-stu-id="2182d-167">Click **Add** to create the new controller for Person with scaffolding.</span></span> <span data-ttu-id="2182d-168">Agora, você tiver gerado as ações do controlador, bem como os modos de exibição.</span><span class="sxs-lookup"><span data-stu-id="2182d-168">You have now generated the controller actions as well as the views.</span></span>

    <span data-ttu-id="2182d-169">![Depois de criar o controlador de pessoa com scaffolding](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image5.png "depois de criar o controlador de pessoa com scaffolding")</span><span class="sxs-lookup"><span data-stu-id="2182d-169">![After creating the Person controller with scaffolding](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image5.png "After creating the Person controller with scaffolding")</span></span>

    <span data-ttu-id="2182d-170">*Depois de criar o controlador de pessoa com scaffolding*</span><span class="sxs-lookup"><span data-stu-id="2182d-170">*After creating the Person controller with scaffolding*</span></span>
10. <span data-ttu-id="2182d-171">Abra **PersonController** classe.</span><span class="sxs-lookup"><span data-stu-id="2182d-171">Open **PersonController** class.</span></span> <span data-ttu-id="2182d-172">Observe que os métodos de ação CRUD completos foi gerados automaticamente.</span><span class="sxs-lookup"><span data-stu-id="2182d-172">Notice that the full CRUD action methods have been generated automatically.</span></span>

   <span data-ttu-id="2182d-173">![Dentro do controlador de pessoa](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image6.png "controlador dentro a pessoa")</span><span class="sxs-lookup"><span data-stu-id="2182d-173">![Inside the Person controller](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image6.png "Inside the Person controller")</span></span>

   <span data-ttu-id="2182d-174">*Dentro do controlador de pessoa*</span><span class="sxs-lookup"><span data-stu-id="2182d-174">*Inside the Person controller*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2-_Running_the_application"></a>
#### <a name="task-2--running-the-application"></a><span data-ttu-id="2182d-175">Tarefa 2-executar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="2182d-175">Task 2- Running the application</span></span>

<span data-ttu-id="2182d-176">Neste ponto, o banco de dados ainda não foi criado.</span><span class="sxs-lookup"><span data-stu-id="2182d-176">At this point, the database is not yet created.</span></span> <span data-ttu-id="2182d-177">Nesta tarefa, você irá executar o aplicativo pela primeira vez e testar as operações CRUD.</span><span class="sxs-lookup"><span data-stu-id="2182d-177">In this task, you will run the application for the first time and test the CRUD operations.</span></span> <span data-ttu-id="2182d-178">O banco de dados será criado em tempo real com o Code First.</span><span class="sxs-lookup"><span data-stu-id="2182d-178">The database will be created on the fly with Code First.</span></span>

1. <span data-ttu-id="2182d-179">Pressione **F5** para executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="2182d-179">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="2182d-180">No navegador, adicione **/Person** para a URL para abrir a página de pessoa.</span><span class="sxs-lookup"><span data-stu-id="2182d-180">In the browser, add **/Person** to the URL to open the Person page.</span></span>

    <span data-ttu-id="2182d-181">![Primeira execução do aplicativo](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image7.png "primeira execução do aplicativo")</span><span class="sxs-lookup"><span data-stu-id="2182d-181">![Application first run](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image7.png "Application first run")</span></span>

    <span data-ttu-id="2182d-182">*O aplicativo: primeiro execute*</span><span class="sxs-lookup"><span data-stu-id="2182d-182">*Application: first run*</span></span>
3. <span data-ttu-id="2182d-183">Agora você explorar as páginas de pessoa e testar as operações CRUD.</span><span class="sxs-lookup"><span data-stu-id="2182d-183">You will now explore the Person pages and test the CRUD operations.</span></span>

    1. <span data-ttu-id="2182d-184">Clique em **criar novo** para adicionar uma nova pessoa.</span><span class="sxs-lookup"><span data-stu-id="2182d-184">Click **Create New** to add a new person.</span></span> <span data-ttu-id="2182d-185">Insira um nome e um sobrenome e clique em **criar**.</span><span class="sxs-lookup"><span data-stu-id="2182d-185">Enter a first name and a last name and click **Create**.</span></span>

        <span data-ttu-id="2182d-186">![Adicionando uma nova pessoa](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image8.png "adicionando uma nova pessoa")</span><span class="sxs-lookup"><span data-stu-id="2182d-186">![Adding a new person](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image8.png "Adding a new person")</span></span>

        <span data-ttu-id="2182d-187">*Adicionando uma nova pessoa*</span><span class="sxs-lookup"><span data-stu-id="2182d-187">*Adding a new person*</span></span>
    2. <span data-ttu-id="2182d-188">Na lista da pessoa, você pode excluir, editar ou adicionar itens.</span><span class="sxs-lookup"><span data-stu-id="2182d-188">In the person's list, you can delete, edit or add items.</span></span>

        <span data-ttu-id="2182d-189">![lista de pessoas](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image9.png "lista de pessoas")</span><span class="sxs-lookup"><span data-stu-id="2182d-189">![person list](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image9.png "person list")</span></span>

        <span data-ttu-id="2182d-190">*Lista de pessoas*</span><span class="sxs-lookup"><span data-stu-id="2182d-190">*Person list*</span></span>
    3. <span data-ttu-id="2182d-191">Clique em **detalhes** para abrir os detalhes da pessoa.</span><span class="sxs-lookup"><span data-stu-id="2182d-191">Click **Details** to open the person's details.</span></span>

        <span data-ttu-id="2182d-192">![Detalhes da pessoa](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image10.png "detalhes da pessoa")</span><span class="sxs-lookup"><span data-stu-id="2182d-192">![Person's details](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image10.png "Person's details")</span></span>

        <span data-ttu-id="2182d-193">*Detalhes da pessoa*</span><span class="sxs-lookup"><span data-stu-id="2182d-193">*Person's details*</span></span>
4. <span data-ttu-id="2182d-194">Feche o navegador e retorne ao Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2182d-194">Close the browser and return to Visual Studio.</span></span> <span data-ttu-id="2182d-195">Observe que você tenha criado o CRUD todo entidade person em todo o aplicativo - do modelo para os modos de exibição - sem escrever uma única linha de código!</span><span class="sxs-lookup"><span data-stu-id="2182d-195">Notice that you have created the whole CRUD for the person entity throughout your application -from the model to the views- without having to write a single line of code!</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3-_Updating_the_database_using_Entity_Framework_Migrations"></a>
#### <a name="task-3--updating-the-database-using-entity-framework-migrations"></a><span data-ttu-id="2182d-196">Tarefa 3-atualizando o banco de dados usando migrações do Entity Framework</span><span class="sxs-lookup"><span data-stu-id="2182d-196">Task 3- Updating the database using Entity Framework Migrations</span></span>

<span data-ttu-id="2182d-197">Nesta tarefa, você atualizará o banco de dados usando migrações do Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="2182d-197">In this task you will update the database using Entity Framework Migrations.</span></span> <span data-ttu-id="2182d-198">Você descobrirá como é fácil alterar o modelo e refletir as alterações nos bancos de dados usando o recurso de migrações do Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="2182d-198">You will discover how easy it is to change the model and reflect the changes in your databases by using the Entity Framework Migrations feature.</span></span>

1. <span data-ttu-id="2182d-199">Abra o Console do Gerenciador de pacotes.</span><span class="sxs-lookup"><span data-stu-id="2182d-199">Open the Package Manager Console.</span></span> <span data-ttu-id="2182d-200">Selecione **ferramentas | Gerenciador de pacotes de biblioteca | Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="2182d-200">Select **Tools | Library Package Manager | Package Manager Console**.</span></span>
2. <span data-ttu-id="2182d-201">No Console do Gerenciador de pacotes, digite o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="2182d-201">In the Package Manager Console, enter the following command:</span></span>

    <span data-ttu-id="2182d-202">PMC</span><span class="sxs-lookup"><span data-stu-id="2182d-202">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample2.ps1)]

    <span data-ttu-id="2182d-203">![Habilitar migrações](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image11.png "habilitar migrações")</span><span class="sxs-lookup"><span data-stu-id="2182d-203">![Enabling Migrations](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image11.png "Enabling Migrations")</span></span>

    <span data-ttu-id="2182d-204">*Habilitar migrações*</span><span class="sxs-lookup"><span data-stu-id="2182d-204">*Enabling migrations*</span></span>

    <span data-ttu-id="2182d-205">O comando Enable-migração cria o **migrações** pasta que contém um script para inicializar o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="2182d-205">The Enable-Migration command creates the **Migrations** folder, which contains a script to initialize the database.</span></span>

    <span data-ttu-id="2182d-206">![Pasta Migrations](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image12.png "pasta Migrations")</span><span class="sxs-lookup"><span data-stu-id="2182d-206">![Migrations folder](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image12.png "Migrations folder")</span></span>

    <span data-ttu-id="2182d-207">*Pasta Migrations*</span><span class="sxs-lookup"><span data-stu-id="2182d-207">*Migrations folder*</span></span>
3. <span data-ttu-id="2182d-208">Abra o **Configuration.cs** arquivo à pasta Migrations.</span><span class="sxs-lookup"><span data-stu-id="2182d-208">Open the **Configuration.cs** file in the Migrations folder.</span></span> <span data-ttu-id="2182d-209">Localize o construtor de classe e altere o **AutomaticMigrationsEnabled** valor para *verdadeiro*.</span><span class="sxs-lookup"><span data-stu-id="2182d-209">Locate the class constructor and change the **AutomaticMigrationsEnabled** value to *true*.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample3.cs)]
4. <span data-ttu-id="2182d-210">Abra a classe Person e adicione um atributo para o nome da pessoa intermediária.</span><span class="sxs-lookup"><span data-stu-id="2182d-210">Open the Person class and add an attribute for the person's middle name.</span></span> <span data-ttu-id="2182d-211">Com esse novo atributo, você está alterando o modelo.</span><span class="sxs-lookup"><span data-stu-id="2182d-211">With this new attribute, you are changing the model.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample4.cs)]
5. <span data-ttu-id="2182d-212">Selecione **compilar | Compilar solução** no menu compilar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="2182d-212">Select **Build | Build Solution** on the menu to build the application.</span></span>

    <span data-ttu-id="2182d-213">![Compilando o aplicativo](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image13.png "Compilando o aplicativo")</span><span class="sxs-lookup"><span data-stu-id="2182d-213">![Building the application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image13.png "Building the application")</span></span>

    <span data-ttu-id="2182d-214">*Compilando o aplicativo*</span><span class="sxs-lookup"><span data-stu-id="2182d-214">*Building the application*</span></span>
6. <span data-ttu-id="2182d-215">No Console do Gerenciador de pacotes, digite o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="2182d-215">In the Package Manager Console, enter the following command:</span></span>

    <span data-ttu-id="2182d-216">PMC</span><span class="sxs-lookup"><span data-stu-id="2182d-216">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample5.ps1)]

    <span data-ttu-id="2182d-217">Esse comando irá procurar alterações nos objetos de dados e, em seguida, ele adicionará os comandos necessários para modificar o banco de dados de acordo.</span><span class="sxs-lookup"><span data-stu-id="2182d-217">This command will look for changes in the data objects, and then, it will add the necessary commands to modify the database accordingly.</span></span>

    <span data-ttu-id="2182d-218">![Adicionando um nome do meio](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image14.png "adicionando um nome do meio")</span><span class="sxs-lookup"><span data-stu-id="2182d-218">![Adding a middle name](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image14.png "Adding a middle name")</span></span>

    <span data-ttu-id="2182d-219">*Adicionando um nome do meio*</span><span class="sxs-lookup"><span data-stu-id="2182d-219">*Adding a middle name*</span></span>
7. <span data-ttu-id="2182d-220">(Opcional) Você pode executar o seguinte comando para gerar um script SQL com a atualização de diferencial.</span><span class="sxs-lookup"><span data-stu-id="2182d-220">(Optional) You can run the following command to generate a SQL script with the differential update.</span></span> <span data-ttu-id="2182d-221">Isso permitirá que você atualizar o banco de dados manualmente (nesse caso, não é necessário), ou aplicar as alterações em outros bancos de dados:</span><span class="sxs-lookup"><span data-stu-id="2182d-221">This will let you update the database manually (In this case it's not necessary), or apply the changes in other databases:</span></span>

    <span data-ttu-id="2182d-222">PMC</span><span class="sxs-lookup"><span data-stu-id="2182d-222">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample6.ps1)]

    <span data-ttu-id="2182d-223">![Gerar um script SQL](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image15.png "gerando um script SQL")</span><span class="sxs-lookup"><span data-stu-id="2182d-223">![Generating a SQL script](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image15.png "Generating a SQL script")</span></span>

    <span data-ttu-id="2182d-224">*Gerar um script SQL*</span><span class="sxs-lookup"><span data-stu-id="2182d-224">*Generating a SQL script*</span></span>

    <span data-ttu-id="2182d-225">![Atualização de Script SQL](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image16.png "atualização de Script SQL")</span><span class="sxs-lookup"><span data-stu-id="2182d-225">![SQL Script update](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image16.png "SQL Script update")</span></span>

    <span data-ttu-id="2182d-226">*Atualização de Script SQL*</span><span class="sxs-lookup"><span data-stu-id="2182d-226">*SQL Script update*</span></span>
8. <span data-ttu-id="2182d-227">No Console do Gerenciador de pacotes, digite o seguinte comando para atualizar o banco de dados:</span><span class="sxs-lookup"><span data-stu-id="2182d-227">In the Package Manager Console, enter the following command to update the database:</span></span>

    <span data-ttu-id="2182d-228">PMC</span><span class="sxs-lookup"><span data-stu-id="2182d-228">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample7.ps1)]

    <span data-ttu-id="2182d-229">![Atualizando o banco de dados](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image17.png "atualizando o banco de dados")</span><span class="sxs-lookup"><span data-stu-id="2182d-229">![Updating the Database](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image17.png "Updating the Database")</span></span>

    <span data-ttu-id="2182d-230">*Atualizando o banco de dados*</span><span class="sxs-lookup"><span data-stu-id="2182d-230">*Updating the Database*</span></span>

    <span data-ttu-id="2182d-231">Isso adicionará a **MiddleName** coluna o **pessoas** tabela para coincidir com a definição atual do **pessoa** classe.</span><span class="sxs-lookup"><span data-stu-id="2182d-231">This will add the **MiddleName** column in the **People** table to match the current definition of the **Person** class.</span></span>
9. <span data-ttu-id="2182d-232">Depois que o banco de dados for atualizado, clique com botão direito na pasta do controlador e selecione **adicionar | Controlador** para adicionar o controlador de pessoa novamente (completa com os mesmos valores).</span><span class="sxs-lookup"><span data-stu-id="2182d-232">Once the database is updated, right-click the Controller folder and select **Add | Controller** to add the Person controller again (Complete with the same values).</span></span> <span data-ttu-id="2182d-233">Isso atualizará os métodos existentes e exibições para adicionar o novo atributo.</span><span class="sxs-lookup"><span data-stu-id="2182d-233">This will update the existing methods and views adding the new attribute.</span></span>

    <span data-ttu-id="2182d-234">![Adição de uma atualização do controlador](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image18.png "adicionando uma atualização do controlador")</span><span class="sxs-lookup"><span data-stu-id="2182d-234">![Adding a controller update](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image18.png "Adding a controller update")</span></span>

    <span data-ttu-id="2182d-235">*Atualizar o controlador*</span><span class="sxs-lookup"><span data-stu-id="2182d-235">*Updating the controller*</span></span>
10. <span data-ttu-id="2182d-236">Clique em **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="2182d-236">Click **Add**.</span></span> <span data-ttu-id="2182d-237">Em seguida, selecione os valores **substituir PersonController.cs** e o **substituir associados a exibições** e clique em **Okey**.</span><span class="sxs-lookup"><span data-stu-id="2182d-237">Then, select the values **Overwrite PersonController.cs** and the **Overwrite associated views** and click **OK**.</span></span>

   ![Adicionando uma substituição de controlador](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image19.png)

   <span data-ttu-id="2182d-239">*Atualizar o controlador*</span><span class="sxs-lookup"><span data-stu-id="2182d-239">*Updating the controller*</span></span>

<a id="Ex1Task4"></a>

<a id="Task4-_Running_the_application"></a>
#### <a name="task4--running-the-application"></a><span data-ttu-id="2182d-240">Task4 - executando o aplicativo</span><span class="sxs-lookup"><span data-stu-id="2182d-240">Task4- Running the application</span></span>

1. <span data-ttu-id="2182d-241">Pressione **F5** para executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="2182d-241">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="2182d-242">Abra **/Person**.</span><span class="sxs-lookup"><span data-stu-id="2182d-242">Open **/Person**.</span></span> <span data-ttu-id="2182d-243">Observe que os dados foi preservados, enquanto a coluna de nome do meio foi adicionada.</span><span class="sxs-lookup"><span data-stu-id="2182d-243">Notice that the data was preserved, while the middle name column was added.</span></span>

    <span data-ttu-id="2182d-244">![Nome do meio adicionado](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image20.png "sobrenome adicionado")</span><span class="sxs-lookup"><span data-stu-id="2182d-244">![Middle Name added](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image20.png "Middle Name added")</span></span>

    <span data-ttu-id="2182d-245">*Nome do meio adicionado*</span><span class="sxs-lookup"><span data-stu-id="2182d-245">*Middle Name added*</span></span>
3. <span data-ttu-id="2182d-246">Se você clicar **editar**, você poderá adicionar um nome do meio para a pessoa atual.</span><span class="sxs-lookup"><span data-stu-id="2182d-246">If you click **Edit**, you will be able to add a middle name to the current person.</span></span>

    <span data-ttu-id="2182d-247">![Edição do nome do meio](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image21.png "edição do nome do meio")</span><span class="sxs-lookup"><span data-stu-id="2182d-247">![Middle Name edition](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image21.png "Middle Name edition")</span></span>

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="2182d-248">Resumo</span><span class="sxs-lookup"><span data-stu-id="2182d-248">Summary</span></span>

<span data-ttu-id="2182d-249">Neste laboratório prático, você aprendeu as etapas simples para criar operações CRUD com Scaffolding do ASP.NET MVC 4 usando qualquer classe de modelo.</span><span class="sxs-lookup"><span data-stu-id="2182d-249">In this Hands-On lab, you have learned simple steps to create CRUD operations with ASP.NET MVC 4 Scaffolding using any model class.</span></span> <span data-ttu-id="2182d-250">Em seguida, você aprendeu a executar uma atualização de ponta a ponta em seu aplicativo - do banco de dados para os modos de exibição - por meio de migrações do Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="2182d-250">Then, you have learned how to perform an end to end update in your application -from the database to the views- by using Entity Framework Migrations.</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="2182d-251">Apêndice a: instalação do Visual Studio Express 2012 para Web</span><span class="sxs-lookup"><span data-stu-id="2182d-251">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="2182d-252">Você pode instalar **Microsoft Visual Studio Express 2012 para Web** ou outra &quot;Express&quot; versão usando o **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="2182d-252">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="2182d-253">As instruções a seguir guia você pelas etapas necessárias para instalar *Visual studio Express 2012 para Web* usando *Microsoft Web Platform Installer*.</span><span class="sxs-lookup"><span data-stu-id="2182d-253">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="2182d-254">Vá para [ [ https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="2182d-254">Go to [[https://go.microsoft.com/? linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="2182d-255">Como alternativa, se você já tiver instalado o Web Platform Installer, você pode abri-lo e pesquisar o produto &quot; <em>Visual Studio Express 2012 para Web com o SDK do Windows Azure</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="2182d-255">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="2182d-256">Clique em **instalar agora**.</span><span class="sxs-lookup"><span data-stu-id="2182d-256">Click on **Install Now**.</span></span> <span data-ttu-id="2182d-257">Se você não tiver **Web Platform Installer** você será redirecionado para baixar e instalá-lo primeiro.</span><span class="sxs-lookup"><span data-stu-id="2182d-257">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="2182d-258">Uma vez **Web Platform Installer** é aberto, clique em **instalar** para iniciar a instalação.</span><span class="sxs-lookup"><span data-stu-id="2182d-258">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="2182d-259">![Instalar o Visual Studio Express](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image22.png "instalar o Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="2182d-259">![Install Visual Studio Express](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image22.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="2182d-260">*Instalar o Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="2182d-260">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="2182d-261">Leia os termos e licenças de todos os produtos e clique em **aceito** para continuar.</span><span class="sxs-lookup"><span data-stu-id="2182d-261">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Aceitar os termos de licença](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image23.png)

    <span data-ttu-id="2182d-263">*Aceitar os termos de licença*</span><span class="sxs-lookup"><span data-stu-id="2182d-263">*Accepting the license terms*</span></span>
5. <span data-ttu-id="2182d-264">Aguarde até que o processo de download e instalação for concluído.</span><span class="sxs-lookup"><span data-stu-id="2182d-264">Wait until the downloading and installation process completes.</span></span>

    ![Progresso da instalação](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image24.png)

    <span data-ttu-id="2182d-266">*Progresso da instalação*</span><span class="sxs-lookup"><span data-stu-id="2182d-266">*Installation progress*</span></span>
6. <span data-ttu-id="2182d-267">Quando a instalação for concluída, clique em **concluir**.</span><span class="sxs-lookup"><span data-stu-id="2182d-267">When the installation completes, click **Finish**.</span></span>

    ![Instalação concluída](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image25.png)

    <span data-ttu-id="2182d-269">*Instalação concluída*</span><span class="sxs-lookup"><span data-stu-id="2182d-269">*Installation completed*</span></span>
7. <span data-ttu-id="2182d-270">Clique em **Exit** para fechar o Web Platform Installer.</span><span class="sxs-lookup"><span data-stu-id="2182d-270">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="2182d-271">Para abrir o Visual Studio Express para Web, vá para o **iniciar** de tela e comece a escrever &quot; **VS Express**&quot;, em seguida, clique no **VS Express para Web** lado a lado.</span><span class="sxs-lookup"><span data-stu-id="2182d-271">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express para o bloco da Web](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image26.png)

    <span data-ttu-id="2182d-273">*VS Express para o bloco da Web*</span><span class="sxs-lookup"><span data-stu-id="2182d-273">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a><span data-ttu-id="2182d-274">Apêndice b: usar trechos de código</span><span class="sxs-lookup"><span data-stu-id="2182d-274">Appendix B: Using Code Snippets</span></span>

<span data-ttu-id="2182d-275">Com trechos de código, você tem todo o código que necessário ao seu alcance.</span><span class="sxs-lookup"><span data-stu-id="2182d-275">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="2182d-276">O documento de laboratório lhe dirá exatamente quando você pode usá-los, conforme mostrado na figura a seguir.</span><span class="sxs-lookup"><span data-stu-id="2182d-276">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="2182d-277">![Usar trechos de código do Visual Studio para inserir código em seu projeto](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image27.png "trechos de código usando o Visual Studio para inserir código em seu projeto")</span><span class="sxs-lookup"><span data-stu-id="2182d-277">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image27.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="2182d-278">*Usar trechos de código do Visual Studio para inserir código em seu projeto*</span><span class="sxs-lookup"><span data-stu-id="2182d-278">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="2182d-279">***Para adicionar um trecho de código usando o teclado (somente c#)***</span><span class="sxs-lookup"><span data-stu-id="2182d-279">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="2182d-280">Coloque o cursor onde você deseja inserir o código.</span><span class="sxs-lookup"><span data-stu-id="2182d-280">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="2182d-281">Comece a digitar o nome do trecho de código (sem espaços ou hifens).</span><span class="sxs-lookup"><span data-stu-id="2182d-281">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="2182d-282">Assista como o IntelliSense exibe os nomes dos trechos de código correspondentes.</span><span class="sxs-lookup"><span data-stu-id="2182d-282">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="2182d-283">Selecione o trecho de código correto (ou continue digitando até que o nome do trecho toda está selecionado).</span><span class="sxs-lookup"><span data-stu-id="2182d-283">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="2182d-284">Pressione a tecla Tab duas vezes para inserir o trecho de código no local do cursor.</span><span class="sxs-lookup"><span data-stu-id="2182d-284">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="2182d-285">![Comece a digitar o nome do trecho](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image28.png "comece a digitar o nome do trecho de código")</span><span class="sxs-lookup"><span data-stu-id="2182d-285">![Start typing the snippet name](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image28.png "Start typing the snippet name")</span></span>

<span data-ttu-id="2182d-286">*Comece a digitar o nome do trecho de código*</span><span class="sxs-lookup"><span data-stu-id="2182d-286">*Start typing the snippet name*</span></span>

<span data-ttu-id="2182d-287">![Pressione Tab para selecionar o trecho de código realçado](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image29.png "pressione Tab para selecionar o trecho de código realçado")</span><span class="sxs-lookup"><span data-stu-id="2182d-287">![Press Tab to select the highlighted snippet](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image29.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="2182d-288">*Pressione Tab para selecionar o trecho de código realçado*</span><span class="sxs-lookup"><span data-stu-id="2182d-288">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="2182d-289">![Pressione Tab novamente e o trecho de código serão expandido](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image30.png "pressione Tab novamente e o trecho de código serão expandido")</span><span class="sxs-lookup"><span data-stu-id="2182d-289">![Press Tab again and the snippet will expand](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image30.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="2182d-290">*Pressione Tab novamente e o trecho de código serão expandido*</span><span class="sxs-lookup"><span data-stu-id="2182d-290">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="2182d-291">***Para adicionar um trecho de código usando o mouse (c#, Visual Basic e XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="2182d-291">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="2182d-292">Clique com botão direito no qual você deseja inserir o trecho de código.</span><span class="sxs-lookup"><span data-stu-id="2182d-292">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="2182d-293">Selecione **Inserir trecho** seguido **Meus trechos de código**.</span><span class="sxs-lookup"><span data-stu-id="2182d-293">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="2182d-294">Selecione o trecho relevante na lista, clicando nele.</span><span class="sxs-lookup"><span data-stu-id="2182d-294">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="2182d-295">![Clique com botão direito no qual você deseja inserir o trecho de código e selecione Insert Snippet](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image31.png "botão direito do mouse em que você deseja inserir o trecho de código e selecione Insert Snippet")</span><span class="sxs-lookup"><span data-stu-id="2182d-295">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image31.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="2182d-296">*Clique com botão direito no qual você deseja inserir o trecho de código e selecione Insert Snippet*</span><span class="sxs-lookup"><span data-stu-id="2182d-296">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="2182d-297">![Escolher o trecho relevante na lista, clicando nele](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image32.png "escolher o trecho relevante na lista, clicando nele")</span><span class="sxs-lookup"><span data-stu-id="2182d-297">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image32.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="2182d-298">*Escolher o trecho relevante na lista, clicando nele*</span><span class="sxs-lookup"><span data-stu-id="2182d-298">*Pick the relevant snippet from the list, by clicking on it*</span></span>

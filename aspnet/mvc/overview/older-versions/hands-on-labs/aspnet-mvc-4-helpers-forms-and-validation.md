---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
title: "Auxiliares do ASP.NET MVC 4, formulários e validação | Microsoft Docs"
author: rick-anderson
description: "No ASP.NET MVC 4 modelos e dados de laboratório prático de acesso, você foi carregar e exibir dados do banco de dados. Neste laboratório prático, você adicionará à..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 187ee9cd-bc70-479b-bfed-f568b8da96eb
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
msc.type: authoredcontent
ms.openlocfilehash: 243db3708ac4311d423c4c137f503f072f5553e6
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/02/2018
---
# <a name="aspnet-mvc-4-helpers-forms-and-validation"></a><span data-ttu-id="903ee-104">Validação, formulários e auxiliares do ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="903ee-104">ASP.NET MVC 4 Helpers, Forms and Validation</span></span>

<span data-ttu-id="903ee-105">Por [Web Camps Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="903ee-105">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="903ee-106">Baixar o Kit de treinamento de Camps de Web</span><span class="sxs-lookup"><span data-stu-id="903ee-106">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="903ee-107">Em **acesso a dados e modelos do ASP.NET MVC 4** laboratório prático, você esteve carregar e exibir dados do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="903ee-107">In **ASP.NET MVC 4 Models and Data Access** Hands-on Lab, you have been loading and displaying data from the database.</span></span> <span data-ttu-id="903ee-108">Neste laboratório prático, você adicionará ao **repositório de música** aplicativo a capacidade de editar esses dados.</span><span class="sxs-lookup"><span data-stu-id="903ee-108">In this Hands-on Lab, you will add to the **Music Store** application the ability to edit that data.</span></span>

<span data-ttu-id="903ee-109">Com esse objetivo em mente, você criará primeiro o controlador que dará suporte as ações de criação, leitura, atualização e exclusão (CRUD) de álbuns.</span><span class="sxs-lookup"><span data-stu-id="903ee-109">With that goal in mind, you will first create the controller that will support the Create, Read, Update and Delete (CRUD) actions of albums.</span></span> <span data-ttu-id="903ee-110">Você irá gerar um modelo de exibição de índice que aproveita o recurso de scaffolding de ASP.NET MVC para exibir as propriedades dos álbuns em uma tabela HTML.</span><span class="sxs-lookup"><span data-stu-id="903ee-110">You will generate an Index View template taking advantage of ASP.NET MVC's scaffolding feature to display the albums' properties in an HTML table.</span></span> <span data-ttu-id="903ee-111">Para aprimorar a exibição, você adicionará um auxiliar HTML personalizado que truncará descrições longo.</span><span class="sxs-lookup"><span data-stu-id="903ee-111">To enhance that view, you will add a custom HTML helper that will truncate long descriptions.</span></span>

<span data-ttu-id="903ee-112">Posteriormente, você adicionará a editar e criar exibições que permitem que você alteram álbuns no banco de dados, com a Ajuda de elementos de formulário, como listas suspensas.</span><span class="sxs-lookup"><span data-stu-id="903ee-112">Afterwards, you will add the Edit and Create Views that will let you alter the albums in the database, with the help of form elements like dropdowns.</span></span>

<span data-ttu-id="903ee-113">Por fim, você permitirá que os usuários excluir um álbum e também você impedirá de inserção de dados errados, validando a entrada.</span><span class="sxs-lookup"><span data-stu-id="903ee-113">Lastly, you will let users delete an album and also you will prevent them from entering wrong data by validating their input.</span></span>

<span data-ttu-id="903ee-114">Este laboratório prático supõe que você tenha um conhecimento básico de **ASP.NET MVC**.</span><span class="sxs-lookup"><span data-stu-id="903ee-114">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC**.</span></span> <span data-ttu-id="903ee-115">Se você não usou **ASP.NET MVC** antes, é recomendável que você passe **conceitos básicos do ASP.NET MVC** laboratório prático.</span><span class="sxs-lookup"><span data-stu-id="903ee-115">If you have not used **ASP.NET MVC** before, we recommend you to go over **ASP.NET MVC Fundamentals** Hands-on Lab.</span></span>

<span data-ttu-id="903ee-116">Este laboratório orienta os aperfeiçoamentos e novos recursos descritos anteriormente, aplicando alterações secundárias a um aplicativo da Web de exemplo fornecido na pasta de origem.</span><span class="sxs-lookup"><span data-stu-id="903ee-116">This lab walks you through the enhancements and new features previously described by applying minor changes to a sample Web application provided in the Source folder.</span></span>

> [!NOTE]
> <span data-ttu-id="903ee-117">Todo o código de exemplo e trechos de código são incluídos no Web Camps treinamento Kit, disponíveis em [versões Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="903ee-117">All sample code and snippets are included in the Web Camps Training Kit, available at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="903ee-118">O projeto específico para este laboratório está disponível em [auxiliares do ASP.NET MVC 4, formulários e validação](https://github.com/Microsoft-Web/HOL-MVC4HelpersFormsAndValidation).</span><span class="sxs-lookup"><span data-stu-id="903ee-118">The project specific to this lab is available at [ASP.NET MVC 4 Helpers, Forms and Validation](https://github.com/Microsoft-Web/HOL-MVC4HelpersFormsAndValidation).</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="903ee-119">Objetivos</span><span class="sxs-lookup"><span data-stu-id="903ee-119">Objectives</span></span>

<span data-ttu-id="903ee-120">Neste laboratório prático, você aprenderá como:</span><span class="sxs-lookup"><span data-stu-id="903ee-120">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="903ee-121">Criar um controlador para dar suporte a operações CRUD</span><span class="sxs-lookup"><span data-stu-id="903ee-121">Create a controller to support CRUD operations</span></span>
- <span data-ttu-id="903ee-122">Gerar uma exibição de índice para exibir as propriedades de entidade em uma tabela HTML</span><span class="sxs-lookup"><span data-stu-id="903ee-122">Generate an Index View to display entity properties in an HTML table</span></span>
- <span data-ttu-id="903ee-123">Adicionar um auxiliar HTML personalizado</span><span class="sxs-lookup"><span data-stu-id="903ee-123">Add a custom HTML helper</span></span>
- <span data-ttu-id="903ee-124">Criar e personalizar uma exibição editar</span><span class="sxs-lookup"><span data-stu-id="903ee-124">Create and customize an Edit View</span></span>
- <span data-ttu-id="903ee-125">Diferenciar entre os métodos de ação reagem a HTTP GET ou chamadas HTTP POST</span><span class="sxs-lookup"><span data-stu-id="903ee-125">Differentiate between action methods that react to either HTTP-GET or HTTP-POST calls</span></span>
- <span data-ttu-id="903ee-126">Adicionar e personalizar uma criar modo de exibição</span><span class="sxs-lookup"><span data-stu-id="903ee-126">Add and customize a Create View</span></span>
- <span data-ttu-id="903ee-127">Lidar com a exclusão de uma entidade</span><span class="sxs-lookup"><span data-stu-id="903ee-127">Handle the deletion of an entity</span></span>
- <span data-ttu-id="903ee-128">Validar a entrada do usuário</span><span class="sxs-lookup"><span data-stu-id="903ee-128">Validate user input</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="903ee-129">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="903ee-129">Prerequisites</span></span>

<span data-ttu-id="903ee-130">Você deve ter os seguintes itens para concluir este laboratório:</span><span class="sxs-lookup"><span data-stu-id="903ee-130">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="903ee-131">[Microsoft Visual Studio Express 2012 para Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) ou superior (leitura [Apêndice A](#AppendixA) para obter instruções sobre como instalá-lo).</span><span class="sxs-lookup"><span data-stu-id="903ee-131">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="903ee-132">Configuração</span><span class="sxs-lookup"><span data-stu-id="903ee-132">Setup</span></span>

<span data-ttu-id="903ee-133">**Instalando os trechos de código**</span><span class="sxs-lookup"><span data-stu-id="903ee-133">**Installing Code Snippets**</span></span>

<span data-ttu-id="903ee-134">Para sua conveniência, boa parte do código que você estiver gerenciando ao longo deste laboratório está disponível como trechos de código do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="903ee-134">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="903ee-135">Para instalar os trechos de código executar **.\Source\Setup\CodeSnippets.vsi** arquivo.</span><span class="sxs-lookup"><span data-stu-id="903ee-135">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="903ee-136">Se você não estiver familiarizado com os trechos de código do Visual Studio e aprender como usá-los, consulte o Apêndice deste documento &quot; [trechos de código usando do apêndice b:](#AppendixB)&quot;.</span><span class="sxs-lookup"><span data-stu-id="903ee-136">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix B: Using Code Snippets](#AppendixB)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="903ee-137">Exercícios</span><span class="sxs-lookup"><span data-stu-id="903ee-137">Exercises</span></span>

<span data-ttu-id="903ee-138">Os exercícios a seguir compõem esse laboratório prático:</span><span class="sxs-lookup"><span data-stu-id="903ee-138">The following exercises make up this Hands-On Lab:</span></span>

1. [<span data-ttu-id="903ee-139">Criando o controlador do Gerenciador de armazenamento e a exibição do índice</span><span class="sxs-lookup"><span data-stu-id="903ee-139">Creating the Store Manager controller and its Index view</span></span>](#Exercise1)
2. [<span data-ttu-id="903ee-140">Adicionando um auxiliar de HTML</span><span class="sxs-lookup"><span data-stu-id="903ee-140">Adding an HTML Helper</span></span>](#Exercise2)
3. [<span data-ttu-id="903ee-141">Criar a exibição de edição</span><span class="sxs-lookup"><span data-stu-id="903ee-141">Creating the Edit View</span></span>](#Exercise3)
4. [<span data-ttu-id="903ee-142">Adicionar um modo de criação</span><span class="sxs-lookup"><span data-stu-id="903ee-142">Adding a Create View</span></span>](#Exercise4)
5. [<span data-ttu-id="903ee-143">Tratamento de exclusão</span><span class="sxs-lookup"><span data-stu-id="903ee-143">Handling Deletion</span></span>](#Exercise5)
6. [<span data-ttu-id="903ee-144">Adicionando uma Validação</span><span class="sxs-lookup"><span data-stu-id="903ee-144">Adding Validation</span></span>](#Exercise6)
7. [<span data-ttu-id="903ee-145">Usando jQuery discreto no lado do cliente</span><span class="sxs-lookup"><span data-stu-id="903ee-145">Using Unobtrusive jQuery at Client Side</span></span>](#Exercise7)

> [!NOTE]
> <span data-ttu-id="903ee-146">Cada exercício é acompanhado por um **final** pasta que contém a solução resultante, você deve obter depois de concluir os exercícios.</span><span class="sxs-lookup"><span data-stu-id="903ee-146">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="903ee-147">Você pode usar essa solução como um guia se você precisar trabalhar com os exercícios de ajuda adicional.</span><span class="sxs-lookup"><span data-stu-id="903ee-147">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="903ee-148">Tempo estimado para concluir este laboratório: **60 minutos**</span><span class="sxs-lookup"><span data-stu-id="903ee-148">Estimated time to complete this lab: **60 minutes**</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_the_Store_Manager_controller_and_its_Index_view"></a>
### <a name="exercise-1-creating-the-store-manager-controller-and-its-index-view"></a><span data-ttu-id="903ee-149">Exercício 1: Criando o controlador do Gerenciador de armazenamento e a exibição do índice</span><span class="sxs-lookup"><span data-stu-id="903ee-149">Exercise 1: Creating the Store Manager controller and its Index view</span></span>

<span data-ttu-id="903ee-150">Neste exercício, você aprenderá como criar um novo controlador para dar suporte a operações CRUD, personalizar seu método de ação de índice para retornar uma lista de álbuns de banco de dados e, finalmente, gerar um modelo de exibição de índice que aproveitam a estrutura de ASP.NET MVC recurso para exibir as propriedades dos álbuns em uma tabela HTML.</span><span class="sxs-lookup"><span data-stu-id="903ee-150">In this exercise, you will learn how to create a new controller to support CRUD operations, customize its Index action method to return a list of albums from the database and finally generating an Index View template taking advantage of ASP.NET MVC's scaffolding feature to display the albums' properties in an HTML table.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_StoreManagerController"></a>
#### <a name="task-1---creating-the-storemanagercontroller"></a><span data-ttu-id="903ee-151">Tarefa 1: Criando o StoreManagerController</span><span class="sxs-lookup"><span data-stu-id="903ee-151">Task 1 - Creating the StoreManagerController</span></span>

<span data-ttu-id="903ee-152">Nesta tarefa, você criará um novo controlador chamado **StoreManagerController** para dar suporte a operações CRUD.</span><span class="sxs-lookup"><span data-stu-id="903ee-152">In this task, you will create a new controller called **StoreManagerController** to support CRUD operations.</span></span>

1. <span data-ttu-id="903ee-153">Abra o **começar** solução localizado em **fonte/Ex1-CreatingTheStoreManagerController/Begin/** pasta.</span><span class="sxs-lookup"><span data-stu-id="903ee-153">Open the **Begin** solution located at **Source/Ex1-CreatingTheStoreManagerController/Begin/** folder.</span></span>

    1. <span data-ttu-id="903ee-154">Você precisará baixar alguns pacotes do NuGet ausentes antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="903ee-154">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="903ee-155">Para fazer isso, clique o **projeto** menu e selecione **gerenciar pacotes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="903ee-155">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="903ee-156">No **gerenciar pacotes NuGet** caixa de diálogo, clique em **restaurar** para baixar os pacotes ausentes.</span><span class="sxs-lookup"><span data-stu-id="903ee-156">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="903ee-157">Por fim, compile a solução clicando **criar** | **compilar solução**.</span><span class="sxs-lookup"><span data-stu-id="903ee-157">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="903ee-158">Uma das vantagens de usar NuGet é que você não precisa enviar todas as bibliotecas no seu projeto, reduzindo o tamanho do projeto.</span><span class="sxs-lookup"><span data-stu-id="903ee-158">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="903ee-159">Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez que você executar o projeto.</span><span class="sxs-lookup"><span data-stu-id="903ee-159">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="903ee-160">É por isso você terá que executar estas etapas depois de abrir uma solução existente neste laboratório.</span><span class="sxs-lookup"><span data-stu-id="903ee-160">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="903ee-161">Adicione um novo controlador.</span><span class="sxs-lookup"><span data-stu-id="903ee-161">Add a new controller.</span></span> <span data-ttu-id="903ee-162">Para fazer isso, clique com botão direito do **controladores** pasta no Gerenciador de soluções, selecione **adicionar** e, em seguida, o **controlador** comando.</span><span class="sxs-lookup"><span data-stu-id="903ee-162">To do this, right-click the **Controllers** folder within the Solution Explorer, select **Add** and then the **Controller** command.</span></span> <span data-ttu-id="903ee-163">Alterar o **controlador** **nome** para **StoreManagerController** e verifique se a opção **controlador MVC com ações de leitura/gravação vazias**está selecionado.</span><span class="sxs-lookup"><span data-stu-id="903ee-163">Change the **Controller** **Name** to **StoreManagerController** and make sure the option **MVC controller with empty read/write actions** is selected.</span></span> <span data-ttu-id="903ee-164">Clique em **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="903ee-164">Click **Add**.</span></span>

    <span data-ttu-id="903ee-165">![Caixa de diálogo Adicionar controlador](aspnet-mvc-4-helpers-forms-and-validation/_static/image1.png "caixa de diálogo Adicionar controlador")</span><span class="sxs-lookup"><span data-stu-id="903ee-165">![Add controller dialog](aspnet-mvc-4-helpers-forms-and-validation/_static/image1.png "Add controller dialog")</span></span>

    <span data-ttu-id="903ee-166">*Adicionar caixa de diálogo do controlador*</span><span class="sxs-lookup"><span data-stu-id="903ee-166">*Add Controller Dialog*</span></span>

    <span data-ttu-id="903ee-167">Uma nova classe de controlador é gerada.</span><span class="sxs-lookup"><span data-stu-id="903ee-167">A new Controller class is generated.</span></span> <span data-ttu-id="903ee-168">Como indicado para adicionar ações de leitura/gravação, métodos de stub para aqueles, ações CRUD comuns são criadas com comentários TODO preenchidos, solicitando para incluir a lógica específica do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="903ee-168">Since you indicated to add actions for read/write, stub methods for those, common CRUD actions are created with TODO comments filled in, prompting to include the application specific logic.</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Customizing_the_StoreManager_Index"></a>
#### <a name="task-2---customizing-the-storemanager-index"></a><span data-ttu-id="903ee-169">Tarefa 2: personalizar o índice StoreManager</span><span class="sxs-lookup"><span data-stu-id="903ee-169">Task 2 - Customizing the StoreManager Index</span></span>

<span data-ttu-id="903ee-170">Nesta tarefa, você personalizará o método de ação do índice StoreManager para retornar uma exibição com a lista de álbuns do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="903ee-170">In this task, you will customize the StoreManager Index action method to return a View with the list of albums from the database.</span></span>

1. <span data-ttu-id="903ee-171">Na classe StoreManagerController, adicione o seguinte *usando* diretivas.</span><span class="sxs-lookup"><span data-stu-id="903ee-171">In the StoreManagerController class, add the following *using* directives.</span></span>

    <span data-ttu-id="903ee-172">(Código de trecho - *validação - Ex1, formulários e auxiliares do ASP.NET MVC 4 usando MvcMusicStore*)</span><span class="sxs-lookup"><span data-stu-id="903ee-172">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex1 using MvcMusicStore*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample1.cs)]
2. <span data-ttu-id="903ee-173">Adicione um campo para o **StoreManagerController** para manter uma instância do **MusicStoreEntities.**</span><span class="sxs-lookup"><span data-stu-id="903ee-173">Add a field to the **StoreManagerController** to hold an instance of **MusicStoreEntities.**</span></span>

    <span data-ttu-id="903ee-174">(Código de trecho - *auxiliares do ASP.NET MVC 4, formulários e validação - Ex1 MusicStoreEntities*)</span><span class="sxs-lookup"><span data-stu-id="903ee-174">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex1 MusicStoreEntities*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample2.cs)]
3. <span data-ttu-id="903ee-175">Implemente a ação de índice StoreManagerController para retornar uma exibição com a lista de álbuns.</span><span class="sxs-lookup"><span data-stu-id="903ee-175">Implement the StoreManagerController Index action to return a View with the list of albums.</span></span>

    <span data-ttu-id="903ee-176">A lógica de ação do controlador será muito semelhante à ação de índice do StoreController gravada anteriormente.</span><span class="sxs-lookup"><span data-stu-id="903ee-176">The Controller action logic will be very similar to the StoreController's Index action written earlier.</span></span> <span data-ttu-id="903ee-177">Use o LINQ para recuperar todos os álbuns, inclusive informações de gênero e artista para exibição.</span><span class="sxs-lookup"><span data-stu-id="903ee-177">Use LINQ to retrieve all albums, including Genre and Artist information for display.</span></span>

    <span data-ttu-id="903ee-178">(Código de trecho - *auxiliares do ASP.NET MVC 4, formulários e validação - Ex1 StoreManagerController índice*)</span><span class="sxs-lookup"><span data-stu-id="903ee-178">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex1 StoreManagerController Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample3.cs)]

<a id="Ex1Task3"></a>

<a id="strongTask_3_-_Creating_the_Index_Viewstrong"></a>
#### <a name="task-3---creating-the-index-view"></a><span data-ttu-id="903ee-179">Tarefa 3: criar a exibição do índice</span><span class="sxs-lookup"><span data-stu-id="903ee-179">Task 3 - Creating the Index View</span></span>

<span data-ttu-id="903ee-180">Nesta tarefa, você irá criar o modelo de exibição do índice para exibir a lista de álbuns retornado pelo **StoreManager** controlador.</span><span class="sxs-lookup"><span data-stu-id="903ee-180">In this task, you will create the Index View template to display the list of albums returned by the **StoreManager** Controller.</span></span>

1. <span data-ttu-id="903ee-181">Antes de criar o novo modelo de exibição, você deve compilar o projeto para que o **caixa de diálogo Adicionar modo de exibição** conhece o **álbum** classe a ser usada.</span><span class="sxs-lookup"><span data-stu-id="903ee-181">Before creating the new View template, you should build the project so that the **Add View Dialog** knows about the **Album** class to use.</span></span> <span data-ttu-id="903ee-182">Selecione **compilar | Criar MvcMusicStore** para compilar o projeto.</span><span class="sxs-lookup"><span data-stu-id="903ee-182">Select **Build | Build MvcMusicStore** to build the project.</span></span>
2. <span data-ttu-id="903ee-183">Clique dentro do **índice** método de ação e selecione **adicionar exibição**.</span><span class="sxs-lookup"><span data-stu-id="903ee-183">Right-click inside the **Index** action method and select **Add View**.</span></span> <span data-ttu-id="903ee-184">Isso abrirá o **adicionar exibição** caixa de diálogo.</span><span class="sxs-lookup"><span data-stu-id="903ee-184">This will bring up the **Add View** dialog.</span></span>

    <span data-ttu-id="903ee-185">![Adicionar exibição](aspnet-mvc-4-helpers-forms-and-validation/_static/image2.png "adicionar modo de exibição")</span><span class="sxs-lookup"><span data-stu-id="903ee-185">![Add view](aspnet-mvc-4-helpers-forms-and-validation/_static/image2.png "Add view")</span></span>

    <span data-ttu-id="903ee-186">*Adicionando uma exibição de dentro do método de índice*</span><span class="sxs-lookup"><span data-stu-id="903ee-186">*Adding a View from within the Index method*</span></span>
3. <span data-ttu-id="903ee-187">Na caixa de diálogo Adicionar modo de exibição, verifique se o nome de exibição é **índice**.</span><span class="sxs-lookup"><span data-stu-id="903ee-187">In the Add View dialog, verify that the View Name is **Index**.</span></span> <span data-ttu-id="903ee-188">Selecione o **criar uma exibição fortemente tipada** opção e selecione **álbum (MvcMusicStore.Models)** do **classe modelo** lista suspensa.</span><span class="sxs-lookup"><span data-stu-id="903ee-188">Select the **Create a strongly-typed view** option, and select **Album (MvcMusicStore.Models)** from the **Model class** drop-down.</span></span> <span data-ttu-id="903ee-189">Selecione **lista** do **modelo Scaffold** lista suspensa.</span><span class="sxs-lookup"><span data-stu-id="903ee-189">Select **List** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="903ee-190">Deixe o **mecanismo de exibição** para **Razor** e os outros campos com padrão de valor e, em seguida, clique em **adicionar**.</span><span class="sxs-lookup"><span data-stu-id="903ee-190">Leave the **View engine** to **Razor** and the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="903ee-191">![Adicionando uma exibição de índice](aspnet-mvc-4-helpers-forms-and-validation/_static/image3.png "adicionando uma exibição de índice")</span><span class="sxs-lookup"><span data-stu-id="903ee-191">![Adding an index view](aspnet-mvc-4-helpers-forms-and-validation/_static/image3.png "Adding an index view")</span></span>

    <span data-ttu-id="903ee-192">*Adicionando uma exibição de índice*</span><span class="sxs-lookup"><span data-stu-id="903ee-192">*Adding an Index View*</span></span>

<a id="Ex1Task4"></a>

<a id="Task_4_-_Customizing_the_scaffold_of_the_Index_View"></a>
#### <a name="task-4---customizing-the-scaffold-of-the-index-view"></a><span data-ttu-id="903ee-193">Tarefa 4 - personalizando scaffold da exibição de índice</span><span class="sxs-lookup"><span data-stu-id="903ee-193">Task 4 - Customizing the scaffold of the Index View</span></span>

<span data-ttu-id="903ee-194">Nesta tarefa, você ajustará o modelo de exibição simple criado com o recurso de estrutura ASP.NET MVC para que ele exiba os campos desejados.</span><span class="sxs-lookup"><span data-stu-id="903ee-194">In this task, you will adjust the simple View template created with ASP.NET MVC scaffolding feature to have it display the fields you want.</span></span>

> [!NOTE]
> <span data-ttu-id="903ee-195">O **scaffolding** suporte dentro de ASP.NET MVC gera um modelo de exibição simple que lista todos os campos no modelo álbum.</span><span class="sxs-lookup"><span data-stu-id="903ee-195">The **scaffolding** support within ASP.NET MVC generates a simple View template which lists all fields in the Album model.</span></span> <span data-ttu-id="903ee-196">**Estrutura** fornece uma maneira rápida para começar em uma exibição fortemente tipada: em vez de gravar o modelo de exibição manualmente, estrutura rapidamente gera um modelo padrão e, em seguida, você pode modificar o código gerado.</span><span class="sxs-lookup"><span data-stu-id="903ee-196">**Scaffolding** provides a quick way to get started on a strongly typed view: rather than having to write the View template manually, scaffolding quickly generates a default template and then you can modify the generated code.</span></span>


1. <span data-ttu-id="903ee-197">Examine o código criado.</span><span class="sxs-lookup"><span data-stu-id="903ee-197">Review the code created.</span></span> <span data-ttu-id="903ee-198">Lista de campos gerada fará parte dos seguintes HTML de tabela que **Scaffolding** está usando para exibir dados tabulares.</span><span class="sxs-lookup"><span data-stu-id="903ee-198">The generated list of fields will be part of the following HTML table that **Scaffolding** is using for displaying tabular data.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample4.cshtml)]
2. <span data-ttu-id="903ee-199">Substitua o  **&lt;tabela&gt;**  código com o código a seguir para exibir apenas o **gênero**, **artista**, **álbum**, e **preço** campos.</span><span class="sxs-lookup"><span data-stu-id="903ee-199">Replace the **&lt;table&gt;** code with the following code to display only the **Genre**, **Artist**, **Album Title**, and **Price** fields.</span></span> <span data-ttu-id="903ee-200">Isso exclui o **AlbumId** e **álbum arte URL** colunas.</span><span class="sxs-lookup"><span data-stu-id="903ee-200">This deletes the **AlbumId** and **Album Art URL** columns.</span></span> <span data-ttu-id="903ee-201">Além disso, ele altera GenreId e ArtistId colunas para exibir suas propriedades de classe vinculados de **Artist.Name** e **Genre.Name**e remove o **detalhes** link.</span><span class="sxs-lookup"><span data-stu-id="903ee-201">Also, it changes GenreId and ArtistId columns to display their linked class properties of **Artist.Name** and **Genre.Name**, and removes the **Details** link.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample5.cshtml)]
3. <span data-ttu-id="903ee-202">Altere as descrições a seguir.</span><span class="sxs-lookup"><span data-stu-id="903ee-202">Change the following descriptions.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample6.cshtml)]

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="903ee-203">Tarefa 5: executando o aplicativo</span><span class="sxs-lookup"><span data-stu-id="903ee-203">Task 5 - Running the Application</span></span>

<span data-ttu-id="903ee-204">Nesta tarefa, você testará que o **StoreManager** **índice** modelo de exibição exibe uma lista de álbuns de acordo com o design das etapas anteriores.</span><span class="sxs-lookup"><span data-stu-id="903ee-204">In this task, you will test that the **StoreManager** **Index** View template displays a list of albums according to the design of the previous steps.</span></span>

1. <span data-ttu-id="903ee-205">Pressione **F5** para executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="903ee-205">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="903ee-206">O projeto é iniciado na Home page.</span><span class="sxs-lookup"><span data-stu-id="903ee-206">The project starts in the Home page.</span></span> <span data-ttu-id="903ee-207">Altere a URL para **/StoreManager** para verificar se uma lista de álbuns é exibida, mostrando seus **título**, **artista** e **gênero**.</span><span class="sxs-lookup"><span data-stu-id="903ee-207">Change the URL to **/StoreManager** to verify that a list of albums is displayed, showing their **Title**, **Artist** and **Genre**.</span></span>

    <span data-ttu-id="903ee-208">![Navegando pela lista de álbuns](aspnet-mvc-4-helpers-forms-and-validation/_static/image4.png "navegando pela lista de álbuns")</span><span class="sxs-lookup"><span data-stu-id="903ee-208">![Browsing the list of albums](aspnet-mvc-4-helpers-forms-and-validation/_static/image4.png "Browsing the list of albums")</span></span>

    <span data-ttu-id="903ee-209">*Navegando pela lista de álbuns*</span><span class="sxs-lookup"><span data-stu-id="903ee-209">*Browsing the list of albums*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Adding_an_HTML_Helper"></a>
### <a name="exercise-2-adding-an-html-helper"></a><span data-ttu-id="903ee-210">Exercício 2: Adicionando um auxiliar de HTML</span><span class="sxs-lookup"><span data-stu-id="903ee-210">Exercise 2: Adding an HTML Helper</span></span>

<span data-ttu-id="903ee-211">A página de índice StoreManager tem um problema potencial: propriedades do título e nome de artista podem ambos ser grande o suficiente para livrar-se a formatação de tabela.</span><span class="sxs-lookup"><span data-stu-id="903ee-211">The StoreManager Index page has one potential issue: Title and Artist Name properties can both be long enough to throw off the table formatting.</span></span> <span data-ttu-id="903ee-212">Neste exercício, você aprenderá como adicionar um auxiliar HTML personalizado para truncar o texto.</span><span class="sxs-lookup"><span data-stu-id="903ee-212">In this exercise you will learn how to add a custom HTML helper to truncate that text.</span></span>

<span data-ttu-id="903ee-213">Na figura a seguir, você pode ver como o formato é modificado devido ao tamanho do texto quando você usa um tamanho pequeno de navegador.</span><span class="sxs-lookup"><span data-stu-id="903ee-213">In the following figure, you can see how the format is modified because of the length of the text when you use a small browser size.</span></span>

<span data-ttu-id="903ee-214">![Navegando pela lista de álbuns com texto truncado não](aspnet-mvc-4-helpers-forms-and-validation/_static/image5.png "navegando pela lista de álbuns com texto não truncado")</span><span class="sxs-lookup"><span data-stu-id="903ee-214">![Browsing the list of Albums with not truncated text](aspnet-mvc-4-helpers-forms-and-validation/_static/image5.png "Browsing the list of Albums with not truncated text")</span></span>

<span data-ttu-id="903ee-215">*Navegando pela lista de álbuns com texto não truncado*</span><span class="sxs-lookup"><span data-stu-id="903ee-215">*Browsing the list of Albums with not truncated text*</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Extending_the_HTML_Helper"></a>
#### <a name="task-1---extending-the-html-helper"></a><span data-ttu-id="903ee-216">Tarefa 1 - estendendo o auxiliar HTML</span><span class="sxs-lookup"><span data-stu-id="903ee-216">Task 1 - Extending the HTML Helper</span></span>

<span data-ttu-id="903ee-217">Nesta tarefa, você adicionará um novo método **Truncate** para o **HTML** objeto exposto em modos de exibição do ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="903ee-217">In this task, you will add a new method **Truncate** to the **HTML** object exposed within ASP.NET MVC Views.</span></span> <span data-ttu-id="903ee-218">Para fazer isso, você irá implementar um **método de extensão** o interna **System.Web.Mvc.HtmlHelper** classe fornecida pelo ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="903ee-218">To do this, you will implement an **extension method** to the built-in **System.Web.Mvc.HtmlHelper** class provided by ASP.NET MVC.</span></span>

> [!NOTE]
> <span data-ttu-id="903ee-219">Para saber mais sobre **métodos de extensão**, visite este artigo do msdn.</span><span class="sxs-lookup"><span data-stu-id="903ee-219">To read more about **Extension Methods**, please visit this msdn article.</span></span> <span data-ttu-id="903ee-220">[https://msdn.microsoft.com/library/bb383977.aspx](https://msdn.microsoft.com/library/bb383977.aspx).</span><span class="sxs-lookup"><span data-stu-id="903ee-220">[https://msdn.microsoft.com/library/bb383977.aspx](https://msdn.microsoft.com/library/bb383977.aspx).</span></span>


1. <span data-ttu-id="903ee-221">Abra o **começar** solução localizado em **fonte/o Ex2-AddingAnHTMLHelper/Begin/** pasta.</span><span class="sxs-lookup"><span data-stu-id="903ee-221">Open the **Begin** solution located at **Source/Ex2-AddingAnHTMLHelper/Begin/** folder.</span></span> <span data-ttu-id="903ee-222">Caso contrário, você pode continuar usando o **final** solução obtido executando o exercício anterior.</span><span class="sxs-lookup"><span data-stu-id="903ee-222">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="903ee-223">Se você abriu fornecido **começar** solução, você precisará baixar alguns pacotes do NuGet ausentes antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="903ee-223">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="903ee-224">Para fazer isso, clique o **projeto** menu e selecione **gerenciar pacotes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="903ee-224">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="903ee-225">No **gerenciar pacotes NuGet** caixa de diálogo, clique em **restaurar** para baixar os pacotes ausentes.</span><span class="sxs-lookup"><span data-stu-id="903ee-225">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="903ee-226">Por fim, compile a solução clicando **criar** | **compilar solução**.</span><span class="sxs-lookup"><span data-stu-id="903ee-226">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="903ee-227">Uma das vantagens de usar NuGet é que você não precisa enviar todas as bibliotecas no seu projeto, reduzindo o tamanho do projeto.</span><span class="sxs-lookup"><span data-stu-id="903ee-227">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="903ee-228">Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez que você executar o projeto.</span><span class="sxs-lookup"><span data-stu-id="903ee-228">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="903ee-229">É por isso você terá que executar estas etapas depois de abrir uma solução existente neste laboratório.</span><span class="sxs-lookup"><span data-stu-id="903ee-229">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="903ee-230">Abra o modo de exibição do StoreManager índice.</span><span class="sxs-lookup"><span data-stu-id="903ee-230">Open StoreManager's Index View.</span></span> <span data-ttu-id="903ee-231">Para fazer isso, no Gerenciador de soluções, expanda o **exibições** pasta, em seguida, o **StoreManager** e abra o **cshtml** arquivo.</span><span class="sxs-lookup"><span data-stu-id="903ee-231">To do this, in the Solution Explorer expand the **Views** folder, then the **StoreManager** and open the **Index.cshtml** file.</span></span>
3. <span data-ttu-id="903ee-232">Adicione o seguinte código abaixo o  **@model**  diretiva para definir o **Truncate** método auxiliar.</span><span class="sxs-lookup"><span data-stu-id="903ee-232">Add the following code below the **@model** directive to define the **Truncate** helper method.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample7.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Truncating_Text_in_the_Page"></a>
#### <a name="task-2---truncating-text-in-the-page"></a><span data-ttu-id="903ee-233">Tarefa 2 - truncando texto na página</span><span class="sxs-lookup"><span data-stu-id="903ee-233">Task 2 - Truncating Text in the Page</span></span>

<span data-ttu-id="903ee-234">Nesta tarefa, você usará o **Truncate** método truncar o texto no modelo de exibição.</span><span class="sxs-lookup"><span data-stu-id="903ee-234">In this task, you will use the **Truncate** method to truncate the text in the View template.</span></span>

1. <span data-ttu-id="903ee-235">Abra o modo de exibição do StoreManager índice.</span><span class="sxs-lookup"><span data-stu-id="903ee-235">Open StoreManager's Index View.</span></span> <span data-ttu-id="903ee-236">Para fazer isso, no Gerenciador de soluções, expanda o **exibições** pasta, em seguida, o **StoreManager** e abra o **cshtml** arquivo.</span><span class="sxs-lookup"><span data-stu-id="903ee-236">To do this, in the Solution Explorer expand the **Views** folder, then the **StoreManager** and open the **Index.cshtml** file.</span></span>
2. <span data-ttu-id="903ee-237">Substitua as linhas que mostram o **nome artista** e do álbum **título**.</span><span class="sxs-lookup"><span data-stu-id="903ee-237">Replace the lines that show the **Artist Name** and Album's **Title**.</span></span> <span data-ttu-id="903ee-238">Para fazer isso, substitua as linhas a seguir.</span><span class="sxs-lookup"><span data-stu-id="903ee-238">To do this, replace the following lines.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample8.cshtml)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="903ee-239">Tarefa 3: executando o aplicativo</span><span class="sxs-lookup"><span data-stu-id="903ee-239">Task 3 - Running the Application</span></span>

<span data-ttu-id="903ee-240">Nesta tarefa, você testará que o **StoreManager** **índice** exibir modelo trunca o título e o nome do artista do álbum.</span><span class="sxs-lookup"><span data-stu-id="903ee-240">In this task, you will test that the **StoreManager** **Index** View template truncates the Album's Title and Artist Name.</span></span>

1. <span data-ttu-id="903ee-241">Pressione **F5** para executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="903ee-241">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="903ee-242">O projeto é iniciado na Home page.</span><span class="sxs-lookup"><span data-stu-id="903ee-242">The project starts in the Home page.</span></span> <span data-ttu-id="903ee-243">Altere a URL para **/StoreManager** para verificar essa longa textos no **título** e **artista** coluna são truncados.</span><span class="sxs-lookup"><span data-stu-id="903ee-243">Change the URL to **/StoreManager** to verify that long texts in the **Title** and **Artist** column are truncated.</span></span>

    <span data-ttu-id="903ee-244">![Truncado nomes títulos e artistas](aspnet-mvc-4-helpers-forms-and-validation/_static/image6.png "truncado artistas e títulos de nomes")</span><span class="sxs-lookup"><span data-stu-id="903ee-244">![Truncated titles and artists names](aspnet-mvc-4-helpers-forms-and-validation/_static/image6.png "Truncated titles and artists names")</span></span>

    <span data-ttu-id="903ee-245">*Títulos truncados e nomes de artista*</span><span class="sxs-lookup"><span data-stu-id="903ee-245">*Truncated Titles and Artist Names*</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Creating_the_Edit_View"></a>
### <a name="exercise-3-creating-the-edit-view"></a><span data-ttu-id="903ee-246">Exercício 3: Criar a exibição de edição</span><span class="sxs-lookup"><span data-stu-id="903ee-246">Exercise 3: Creating the Edit View</span></span>

<span data-ttu-id="903ee-247">Neste exercício, você aprenderá como criar um formulário para permitir que os gerentes de loja editar um álbum.</span><span class="sxs-lookup"><span data-stu-id="903ee-247">In this exercise, you will learn how to create a form to allow store managers to edit an Album.</span></span> <span data-ttu-id="903ee-248">Eles procurará o **/StoreManager/Edit/id** URL (**id** sendo a id exclusiva do álbum para editar), tornando uma chamada HTTP GET para o servidor.</span><span class="sxs-lookup"><span data-stu-id="903ee-248">They will browse the **/StoreManager/Edit/id** URL (**id** being the unique id of the album to edit), thus making an HTTP-GET call to the server.</span></span>

<span data-ttu-id="903ee-249">O método de ação do controlador Editar irá recuperar o álbum apropriado do banco de dados, crie um **StoreManagerViewModel** objeto para encapsular o (junto com uma lista de artistas e gêneros) e passá-lo de um modelo de exibição para renderizar a página HTML para o usuário.</span><span class="sxs-lookup"><span data-stu-id="903ee-249">The Controller Edit action method will retrieve the appropriate Album from the database, create a **StoreManagerViewModel** object to encapsulate it (along with a list of Artists and Genres), and then pass it off to a View template to render the HTML page back to the user.</span></span> <span data-ttu-id="903ee-250">Esta página conterá uma  **&lt;formulário&gt;**  elemento com caixas de texto e listas suspensas para editar as propriedades de álbum.</span><span class="sxs-lookup"><span data-stu-id="903ee-250">This page will contain a **&lt;form&gt;** element with textboxes and dropdowns for editing the Album properties.</span></span>

<span data-ttu-id="903ee-251">Depois que o usuário atualiza os valores de formulário álbum e clica o **salvar** botão, as alterações são enviadas por meio de um POST HTTP de retorno de chamada para **/StoreManager/Edit/id**. Embora a URL permanece o mesmo que a última chamada, ASP.NET MVC identifica que esse tempo é um HTTP POST e, portanto, executa um método de ação de edição diferente (uma decorado com **[HttpPost]**).</span><span class="sxs-lookup"><span data-stu-id="903ee-251">Once the user updates the Album form values and clicks the **Save** button, the changes are submitted via an HTTP-POST call back to **/StoreManager/Edit/id**. Although the URL remains the same as in the last call, ASP.NET MVC identifies that this time it is an HTTP-POST and therefore executes a different Edit action method (one decorated with **[HttpPost]**).</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Edit_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-edit-action-method"></a><span data-ttu-id="903ee-252">Tarefa 1 - Implementando o método de ação de edição de HTTP GET</span><span class="sxs-lookup"><span data-stu-id="903ee-252">Task 1 - Implementing the HTTP-GET Edit Action Method</span></span>

<span data-ttu-id="903ee-253">Nesta tarefa, você implementar a versão de HTTP GET do método de ação de edição para recuperar o álbum apropriado do banco de dados, bem como uma lista de todos os gêneros e artistas.</span><span class="sxs-lookup"><span data-stu-id="903ee-253">In this task, you will implement the HTTP-GET version of the Edit action method to retrieve the appropriate Album from the database, as well as a list of all Genres and Artists.</span></span> <span data-ttu-id="903ee-254">Ele será empacotar esses dados para o **StoreManagerViewModel** objeto definido na última etapa, que, em seguida, será passada para um modelo de exibição para renderizar a resposta com.</span><span class="sxs-lookup"><span data-stu-id="903ee-254">It will package this data up into the **StoreManagerViewModel** object defined in the last step, which will then be passed to a View template to render the response with.</span></span>

1. <span data-ttu-id="903ee-255">Abra o **começar** solução localizado em **fonte/Ex3-CreatingTheEditView/Begin/** pasta.</span><span class="sxs-lookup"><span data-stu-id="903ee-255">Open the **Begin** solution located at **Source/Ex3-CreatingTheEditView/Begin/** folder.</span></span> <span data-ttu-id="903ee-256">Caso contrário, você pode continuar usando o **final** solução obtido executando o exercício anterior.</span><span class="sxs-lookup"><span data-stu-id="903ee-256">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="903ee-257">Se você abriu fornecido **começar** solução, você precisará baixar alguns pacotes do NuGet ausentes antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="903ee-257">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="903ee-258">Para fazer isso, clique o **projeto** menu e selecione **gerenciar pacotes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="903ee-258">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="903ee-259">No **gerenciar pacotes NuGet** caixa de diálogo, clique em **restaurar** para baixar os pacotes ausentes.</span><span class="sxs-lookup"><span data-stu-id="903ee-259">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="903ee-260">Por fim, compile a solução clicando **criar** | **compilar solução**.</span><span class="sxs-lookup"><span data-stu-id="903ee-260">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="903ee-261">Uma das vantagens de usar NuGet é que você não precisa enviar todas as bibliotecas no seu projeto, reduzindo o tamanho do projeto.</span><span class="sxs-lookup"><span data-stu-id="903ee-261">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="903ee-262">Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez que você executar o projeto.</span><span class="sxs-lookup"><span data-stu-id="903ee-262">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="903ee-263">É por isso você terá que executar estas etapas depois de abrir uma solução existente neste laboratório.</span><span class="sxs-lookup"><span data-stu-id="903ee-263">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="903ee-264">Abra o **StoreManagerController** classe.</span><span class="sxs-lookup"><span data-stu-id="903ee-264">Open the **StoreManagerController** class.</span></span> <span data-ttu-id="903ee-265">Para fazer isso, expanda o **controladores** pasta e clique duas vezes em **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="903ee-265">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
3. <span data-ttu-id="903ee-266">Substitua o **HTTP GET editar** método de ação com o código a seguir para recuperar as **álbum** , bem como a **gêneros** e **artistas**lista.</span><span class="sxs-lookup"><span data-stu-id="903ee-266">Replace the **HTTP-GET Edit** action method with the following code to retrieve the appropriate **Album** as well as the **Genres** and **Artists** lists.</span></span>

    <span data-ttu-id="903ee-267">(Código de trecho - *auxiliares do ASP.NET MVC 4, formulários e validação - Ex3 StoreManagerController HTTP GET Editar ação*)</span><span class="sxs-lookup"><span data-stu-id="903ee-267">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex3 StoreManagerController HTTP-GET Edit action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample9.cs)]

    > [!NOTE]
    > <span data-ttu-id="903ee-268">Você está usando **System.Web.Mvc** **SelectList** para artistas e gêneros em vez do **Generic** lista.</span><span class="sxs-lookup"><span data-stu-id="903ee-268">You are using **System.Web.Mvc** **SelectList** for Artists and Genres instead of the **System.Collections.Generic** List.</span></span>
    > 
    > <span data-ttu-id="903ee-269">**SelectList** é uma maneira de limpeza para popular menus suspensos HTML e gerenciar itens como a seleção atual.</span><span class="sxs-lookup"><span data-stu-id="903ee-269">**SelectList** is a cleaner way to populate HTML dropdowns and manage things like current selection.</span></span> <span data-ttu-id="903ee-270">Criando e a configuração mais recente desses objetos ViewModel na ação do controlador fará o cenário de formulário Editar limpeza.</span><span class="sxs-lookup"><span data-stu-id="903ee-270">Instantiating and later setting up these ViewModel objects in the controller action will make the Edit form scenario cleaner.</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Creating_the_Edit_View"></a>
#### <a name="task-2---creating-the-edit-view"></a><span data-ttu-id="903ee-271">Tarefa 2: criar o modo de exibição de edição</span><span class="sxs-lookup"><span data-stu-id="903ee-271">Task 2 - Creating the Edit View</span></span>

<span data-ttu-id="903ee-272">Nesta tarefa, você criará um modelo de exibição editar posteriormente exibirá as propriedades de álbum.</span><span class="sxs-lookup"><span data-stu-id="903ee-272">In this task, you will create an Edit View template that will later display the album properties.</span></span>

1. <span data-ttu-id="903ee-273">Crie a exibição de edição.</span><span class="sxs-lookup"><span data-stu-id="903ee-273">Create the Edit View.</span></span> <span data-ttu-id="903ee-274">Para fazer isso, clique dentro do **editar** método de ação e selecione **adicionar exibição**.</span><span class="sxs-lookup"><span data-stu-id="903ee-274">To do this, right-click inside the **Edit** action method and select **Add View**.</span></span>
2. <span data-ttu-id="903ee-275">Na caixa de diálogo Adicionar modo de exibição, verifique se o nome de exibição é **editar**.</span><span class="sxs-lookup"><span data-stu-id="903ee-275">In the Add View dialog, verify that the View Name is **Edit**.</span></span> <span data-ttu-id="903ee-276">Verifique o **criar uma exibição fortemente tipada** caixa de seleção e selecione **álbum (MvcMusicStore.Models)** do **exibir dados de classe** lista suspensa.</span><span class="sxs-lookup"><span data-stu-id="903ee-276">Check the **Create a strongly-typed view** checkbox and select **Album (MvcMusicStore.Models)** from the **View data class** drop-down.</span></span> <span data-ttu-id="903ee-277">Selecione **editar** do **modelo Scaffold** lista suspensa.</span><span class="sxs-lookup"><span data-stu-id="903ee-277">Select **Edit** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="903ee-278">Deixe os outros campos com seus valores padrão e, em seguida, clique em **adicionar**.</span><span class="sxs-lookup"><span data-stu-id="903ee-278">Leave the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="903ee-279">![Adicionando uma exibição de edição](aspnet-mvc-4-helpers-forms-and-validation/_static/image7.png "adicionando uma exibição de edição")</span><span class="sxs-lookup"><span data-stu-id="903ee-279">![Adding an Edit view](aspnet-mvc-4-helpers-forms-and-validation/_static/image7.png "Adding an Edit view")</span></span>

    <span data-ttu-id="903ee-280">*Adicionando uma exibição de edição*</span><span class="sxs-lookup"><span data-stu-id="903ee-280">*Adding an Edit view*</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="903ee-281">Tarefa 3: executando o aplicativo</span><span class="sxs-lookup"><span data-stu-id="903ee-281">Task 3 - Running the Application</span></span>

<span data-ttu-id="903ee-282">Nesta tarefa, você testará que o **StoreManager** **editar** página de exibição exibe valores das propriedades para o álbum passado como parâmetro.</span><span class="sxs-lookup"><span data-stu-id="903ee-282">In this task, you will test that the **StoreManager** **Edit** View page displays the properties' values for the album passed as parameter.</span></span>

1. <span data-ttu-id="903ee-283">Pressione **F5** para executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="903ee-283">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="903ee-284">O projeto é iniciado na Home page.</span><span class="sxs-lookup"><span data-stu-id="903ee-284">The project starts in the Home page.</span></span> <span data-ttu-id="903ee-285">Altere a URL para **/StoreManager/Edit/1** para verificar se os valores das propriedades para o álbum passados são exibidos.</span><span class="sxs-lookup"><span data-stu-id="903ee-285">Change the URL to **/StoreManager/Edit/1** to verify that the properties' values for the album passed are displayed.</span></span>

    <span data-ttu-id="903ee-286">![Pesquisa no modo de exibição de edição do álbum](aspnet-mvc-4-helpers-forms-and-validation/_static/image8.png "Editar exibição do álbum de navegação")</span><span class="sxs-lookup"><span data-stu-id="903ee-286">![Browsing Album's Edit View](aspnet-mvc-4-helpers-forms-and-validation/_static/image8.png "Browsing Album's Edit View")</span></span>

    <span data-ttu-id="903ee-287">*Exibição de edição do álbum de navegação*</span><span class="sxs-lookup"><span data-stu-id="903ee-287">*Browsing Album's Edit view*</span></span>

<a id="Ex3Task4"></a>

<a id="Task_4_-_Implementing_drop-downs_on_the_Album_Editor_Template"></a>
#### <a name="task-4---implementing-drop-downs-on-the-album-editor-template"></a><span data-ttu-id="903ee-288">Tarefa 4 - implementar listas suspensas no modelo álbum Editor</span><span class="sxs-lookup"><span data-stu-id="903ee-288">Task 4 - Implementing drop-downs on the Album Editor Template</span></span>

<span data-ttu-id="903ee-289">Nesta tarefa, você irá adicionar listas suspensas para o modelo de exibição criado na última tarefa, para que o usuário pode selecionar em uma lista de artistas e gêneros.</span><span class="sxs-lookup"><span data-stu-id="903ee-289">In this task, you will add drop-downs to the View template created in the last task, so that the user can select from a list of Artists and Genres.</span></span>

1. <span data-ttu-id="903ee-290">Substituir todos os **álbum** fieldset código com o seguinte:</span><span class="sxs-lookup"><span data-stu-id="903ee-290">Replace all the **Album** fieldset code with the following:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample10.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="903ee-291">Um **Html.DropDownList** auxiliar foi adicionado ao renderizar listas suspensas para escolher artistas e gêneros.</span><span class="sxs-lookup"><span data-stu-id="903ee-291">An **Html.DropDownList** helper has been added to render drop-downs for choosing Artists and Genres.</span></span> <span data-ttu-id="903ee-292">Os parâmetros passados para **Html.DropDownList** são:</span><span class="sxs-lookup"><span data-stu-id="903ee-292">The parameters passed to **Html.DropDownList** are:</span></span>
    > 
    > 1. <span data-ttu-id="903ee-293">O nome do campo de formulário (**&quot;ArtistId&quot;**).</span><span class="sxs-lookup"><span data-stu-id="903ee-293">The name of the form field (**&quot;ArtistId&quot;**).</span></span>
    > 2. <span data-ttu-id="903ee-294">O **SelectList** de valores na lista suspensa.</span><span class="sxs-lookup"><span data-stu-id="903ee-294">The **SelectList** of values for the drop-down.</span></span>

<a id="Ex3Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="903ee-295">Tarefa 5: executando o aplicativo</span><span class="sxs-lookup"><span data-stu-id="903ee-295">Task 5 - Running the Application</span></span>

<span data-ttu-id="903ee-296">Nesta tarefa, você testará que o **StoreManager** **editar** página de exibição exibe listas suspensas, em vez de campos de texto artista e ID de gênero.</span><span class="sxs-lookup"><span data-stu-id="903ee-296">In this task, you will test that the **StoreManager** **Edit** View page displays drop-downs instead of Artist and Genre ID text fields.</span></span>

1. <span data-ttu-id="903ee-297">Pressione **F5** para executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="903ee-297">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="903ee-298">O projeto é iniciado na Home page.</span><span class="sxs-lookup"><span data-stu-id="903ee-298">The project starts in the Home page.</span></span> <span data-ttu-id="903ee-299">Altere a URL para **/StoreManager/Edit/1** para verificar se ele exibe listas suspensas, em vez de campos de texto artista e ID de gênero.</span><span class="sxs-lookup"><span data-stu-id="903ee-299">Change the URL to **/StoreManager/Edit/1** to verify that it displays drop-downs instead of Artist and Genre ID text fields.</span></span>

    <span data-ttu-id="903ee-300">![Navegação Editar exibição do álbum com menus suspensos](aspnet-mvc-4-helpers-forms-and-validation/_static/image9.png "Editar exibição da navegação álbum com menus suspensos")</span><span class="sxs-lookup"><span data-stu-id="903ee-300">![Browsing Album's Edit View with drop downs](aspnet-mvc-4-helpers-forms-and-validation/_static/image9.png "Browsing Album's Edit View with drop downs")</span></span>

    <span data-ttu-id="903ee-301">*Exibição de edição do álbum, dessa vez com menus suspensos de navegação*</span><span class="sxs-lookup"><span data-stu-id="903ee-301">*Browsing Album's Edit view, this time with dropdowns*</span></span>

<a id="Ex3Task6"></a>

<a id="Task_6_-_Implementing_the_HTTP-POST_Edit_action_method"></a>
#### <a name="task-6---implementing-the-http-post-edit-action-method"></a><span data-ttu-id="903ee-302">Tarefa 6: implementar o método de ação HTTP POST editar</span><span class="sxs-lookup"><span data-stu-id="903ee-302">Task 6 - Implementing the HTTP-POST Edit action method</span></span>

<span data-ttu-id="903ee-303">Agora que a editar exibe conforme o esperado, você precisa implementar o método HTTP POST Editar ação para salvar as alterações feitas ao álbum.</span><span class="sxs-lookup"><span data-stu-id="903ee-303">Now that the Edit View displays as expected, you need to implement the HTTP-POST Edit Action method to save the changes made to the Album.</span></span>

1. <span data-ttu-id="903ee-304">Feche o navegador se necessário, para retornar à janela do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="903ee-304">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="903ee-305">Abra **StoreManagerController** do **controladores** pasta.</span><span class="sxs-lookup"><span data-stu-id="903ee-305">Open **StoreManagerController** from the **Controllers** folder.</span></span>
2. <span data-ttu-id="903ee-306">Substituir **HTTP POST editar** código do método de ação com os seguintes (Observe que o método que deve ser substituído versão sobrecarregada que recebe dois parâmetros):</span><span class="sxs-lookup"><span data-stu-id="903ee-306">Replace **HTTP-POST Edit** action method code with the following (note that the method that must be replaced is overloaded version that receives two parameters):</span></span>

    <span data-ttu-id="903ee-307">(Código de trecho - *auxiliares do ASP.NET MVC 4, formulários e validação - Ex3 StoreManagerController HTTP POST Editar ação*)</span><span class="sxs-lookup"><span data-stu-id="903ee-307">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex3 StoreManagerController HTTP-POST Edit action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample11.cs)]

    > [!NOTE]
    > <span data-ttu-id="903ee-308">Esse método será executado quando o usuário clica o **salvar** botão do modo de exibição e executa um POST HTTP dos valores de formulário para o servidor para mantê-las no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="903ee-308">This method will be executed when the user clicks the **Save** button of the View and performs an HTTP-POST of the form values back to the server to persist them in the database.</span></span> <span data-ttu-id="903ee-309">O decorador **[HttpPost]** indica que o método deve ser usado para os cenários de HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="903ee-309">The decorator **[HttpPost]** indicates that the method should be used for those HTTP-POST scenarios.</span></span> <span data-ttu-id="903ee-310">O método usa um **álbum** objeto.</span><span class="sxs-lookup"><span data-stu-id="903ee-310">The method takes an **Album** object.</span></span> <span data-ttu-id="903ee-311">ASP.NET MVC criará automaticamente o objeto álbum do postados &lt;formulário&gt; valores.</span><span class="sxs-lookup"><span data-stu-id="903ee-311">ASP.NET MVC will automatically create the Album object from the posted &lt;form&gt; values.</span></span>
    > 
    > <span data-ttu-id="903ee-312">O método executará estas etapas:</span><span class="sxs-lookup"><span data-stu-id="903ee-312">The method will perform these steps:</span></span>
    > 
    > 1. <span data-ttu-id="903ee-313">Se o modelo é válido:</span><span class="sxs-lookup"><span data-stu-id="903ee-313">If model is valid:</span></span>
    > 
    >     1. <span data-ttu-id="903ee-314">Atualize a entrada de álbum no contexto para marcá-la como um objeto modificado.</span><span class="sxs-lookup"><span data-stu-id="903ee-314">Update the album entry in the context to mark it as a modified object.</span></span>
    >     2. <span data-ttu-id="903ee-315">Salvar as alterações e redirecionar para o modo de exibição do índice.</span><span class="sxs-lookup"><span data-stu-id="903ee-315">Save the changes and redirect to the index view.</span></span>
    > 2. <span data-ttu-id="903ee-316">Se o modelo não é válido, ele preencherá ViewBag com o **GenreId** e **ArtistId**, ele retornará o modo de exibição com o objeto álbum recebido para permitir que o usuário execute qualquer atualização necessária.</span><span class="sxs-lookup"><span data-stu-id="903ee-316">If the model is not valid, it will populate the ViewBag with the **GenreId** and **ArtistId**, then it will return the view with the received Album object to allow the user perform any required update.</span></span>

<a id="Ex3Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a><span data-ttu-id="903ee-317">Tarefa 7: executando o aplicativo</span><span class="sxs-lookup"><span data-stu-id="903ee-317">Task 7 - Running the Application</span></span>

<span data-ttu-id="903ee-318">Nesta tarefa, você testará que o **StoreManager editar** página de exibição de salvar os dados atualizados do álbum no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="903ee-318">In this task, you will test that the **StoreManager Edit** View page actually saves the updated Album data in the database.</span></span>

1. <span data-ttu-id="903ee-319">Pressione **F5** para executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="903ee-319">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="903ee-320">O projeto é iniciado na Home page.</span><span class="sxs-lookup"><span data-stu-id="903ee-320">The project starts in the Home page.</span></span> <span data-ttu-id="903ee-321">Altere a URL para **/StoreManager/Edit/1**.</span><span class="sxs-lookup"><span data-stu-id="903ee-321">Change the URL to **/StoreManager/Edit/1**.</span></span> <span data-ttu-id="903ee-322">Altere o título do álbum para **carga** e clique em **salvar**.</span><span class="sxs-lookup"><span data-stu-id="903ee-322">Change the Album title to **Load** and click on **Save**.</span></span> <span data-ttu-id="903ee-323">Verifique se o título do álbum realmente alterado na lista de álbuns.</span><span class="sxs-lookup"><span data-stu-id="903ee-323">Verify that album's title actually changed in the list of albums.</span></span>

    <span data-ttu-id="903ee-324">![Atualizando um álbum](aspnet-mvc-4-helpers-forms-and-validation/_static/image10.png "atualizando um álbum")</span><span class="sxs-lookup"><span data-stu-id="903ee-324">![Updating an album](aspnet-mvc-4-helpers-forms-and-validation/_static/image10.png "Updating an album")</span></span>

    <span data-ttu-id="903ee-325">*Atualizando um álbum*</span><span class="sxs-lookup"><span data-stu-id="903ee-325">*Updating an Album*</span></span>

<a id="Exercise4"></a>

<a id="Exercise_4_Adding_a_Create_View"></a>
### <a name="exercise-4-adding-a-create-view"></a><span data-ttu-id="903ee-326">Exercício 4: Adicionar um modo de criação</span><span class="sxs-lookup"><span data-stu-id="903ee-326">Exercise 4: Adding a Create View</span></span>

<span data-ttu-id="903ee-327">Agora que o **StoreManagerController** oferece suporte a **editar** capacidade neste exercício, você aprenderá a adicionar um modelo de Create View para permitem armazenar gerenciadores de adicionar novos álbuns ao aplicativo.</span><span class="sxs-lookup"><span data-stu-id="903ee-327">Now that the **StoreManagerController** supports the **Edit** ability, in this exercise you will learn how to add a Create View template to let store managers add new Albums to the application.</span></span>

<span data-ttu-id="903ee-328">Como você fez com a funcionalidade de edição, implementará o cenário de criar usando dois métodos separados dentro de **StoreManagerController** classe:</span><span class="sxs-lookup"><span data-stu-id="903ee-328">Like you did with the Edit functionality, you will implement the Create scenario using two separate methods within the **StoreManagerController** class:</span></span>

1. <span data-ttu-id="903ee-329">Um método de ação exibirá um formulário vazio quando gerenciadores de armazenamento primeiro visite o **/StoreManager/criar** URL.</span><span class="sxs-lookup"><span data-stu-id="903ee-329">One action method will display an empty form when store managers first visit the **/StoreManager/Create** URL.</span></span>
2. <span data-ttu-id="903ee-330">Um segundo método de ação tratará o cenário em que o Gerenciador de armazenamento clica o **salvar** botão no formulário e envia os valores de volta para o **/StoreManager/criar** URL como um POST HTTP.</span><span class="sxs-lookup"><span data-stu-id="903ee-330">A second action method will handle the scenario where the store manager clicks the **Save** button within the form and submits the values back to the **/StoreManager/Create** URL as an HTTP-POST.</span></span>

<a id="Ex4Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Create_action_method"></a>
#### <a name="task-1---implementing-the-http-get-create-action-method"></a><span data-ttu-id="903ee-331">Tarefa 1 - Implementando o método de ação Criar HTTP GET</span><span class="sxs-lookup"><span data-stu-id="903ee-331">Task 1 - Implementing the HTTP-GET Create action method</span></span>

<span data-ttu-id="903ee-332">Nesta tarefa, você irá implementar a versão de HTTP GET do método de ação de criação para recuperar uma lista de todos os gêneros e artistas, empacote esses dados em um **StoreManagerViewModel** objeto, que, em seguida, passará para um modelo de exibição.</span><span class="sxs-lookup"><span data-stu-id="903ee-332">In this task, you will implement the HTTP-GET version of the Create action method to retrieve a list of all Genres and Artists, package this data up into a **StoreManagerViewModel** object, which will then be passed to a View template.</span></span>

1. <span data-ttu-id="903ee-333">Abra o **começar** solução localizado em **fonte/Ex4-AddingACreateView/Begin/** pasta.</span><span class="sxs-lookup"><span data-stu-id="903ee-333">Open the **Begin** solution located at **Source/Ex4-AddingACreateView/Begin/** folder.</span></span> <span data-ttu-id="903ee-334">Caso contrário, você pode continuar usando o **final** solução obtido executando o exercício anterior.</span><span class="sxs-lookup"><span data-stu-id="903ee-334">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="903ee-335">Se você abriu fornecido **começar** solução, você precisará baixar alguns pacotes do NuGet ausentes antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="903ee-335">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="903ee-336">Para fazer isso, clique o **projeto** menu e selecione **gerenciar pacotes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="903ee-336">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="903ee-337">No **gerenciar pacotes NuGet** caixa de diálogo, clique em **restaurar** para baixar os pacotes ausentes.</span><span class="sxs-lookup"><span data-stu-id="903ee-337">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="903ee-338">Por fim, compile a solução clicando **criar** | **compilar solução**.</span><span class="sxs-lookup"><span data-stu-id="903ee-338">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="903ee-339">Uma das vantagens de usar NuGet é que você não precisa enviar todas as bibliotecas no seu projeto, reduzindo o tamanho do projeto.</span><span class="sxs-lookup"><span data-stu-id="903ee-339">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="903ee-340">Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez que você executar o projeto.</span><span class="sxs-lookup"><span data-stu-id="903ee-340">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="903ee-341">É por isso você terá que executar estas etapas depois de abrir uma solução existente neste laboratório.</span><span class="sxs-lookup"><span data-stu-id="903ee-341">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="903ee-342">Abra **StoreManagerController** classe.</span><span class="sxs-lookup"><span data-stu-id="903ee-342">Open **StoreManagerController** class.</span></span> <span data-ttu-id="903ee-343">Para fazer isso, expanda o **controladores** pasta e clique duas vezes em **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="903ee-343">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
3. <span data-ttu-id="903ee-344">Substitua o **criar** código do método de ação com o seguinte:</span><span class="sxs-lookup"><span data-stu-id="903ee-344">Replace the **Create** action method code with the following:</span></span>

    <span data-ttu-id="903ee-345">(Código de trecho - *auxiliares do ASP.NET MVC 4, formulários e validação - Ex4 StoreManagerController HTTP GET Criar ação*)</span><span class="sxs-lookup"><span data-stu-id="903ee-345">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex4 StoreManagerController HTTP-GET Create action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample12.cs)]

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_the_Create_View"></a>
#### <a name="task-2---adding-the-create-view"></a><span data-ttu-id="903ee-346">Tarefa 2: adicionando Create View</span><span class="sxs-lookup"><span data-stu-id="903ee-346">Task 2 - Adding the Create View</span></span>

<span data-ttu-id="903ee-347">Nesta tarefa, você adicionará o modelo Criar modo de exibição que exibirá um novo formulário álbum (vazio).</span><span class="sxs-lookup"><span data-stu-id="903ee-347">In this task, you will add the Create View template that will display a new (empty) Album form.</span></span>

1. <span data-ttu-id="903ee-348">Clique dentro do **criar** método de ação e selecione **adicionar exibição**.</span><span class="sxs-lookup"><span data-stu-id="903ee-348">Right-click inside the **Create** action method and select **Add View**.</span></span> <span data-ttu-id="903ee-349">Isso abrirá a caixa de diálogo Adicionar modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="903ee-349">This will bring up the Add View dialog.</span></span>
2. <span data-ttu-id="903ee-350">Na caixa de diálogo Adicionar modo de exibição, verifique se o nome de exibição é **criar**.</span><span class="sxs-lookup"><span data-stu-id="903ee-350">In the Add View dialog, verify that the View Name is **Create**.</span></span> <span data-ttu-id="903ee-351">Selecione o **criar uma exibição fortemente tipada** opção e selecione **álbum (MvcMusicStore.Models)** do **classe modelo** suspenso e **criar** do **modelo Scaffold** lista suspensa.</span><span class="sxs-lookup"><span data-stu-id="903ee-351">Select the **Create a strongly-typed view** option and select **Album (MvcMusicStore.Models)** from the **Model class** drop-down and **Create** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="903ee-352">Deixe os outros campos com seus valores padrão e, em seguida, clique em **adicionar**.</span><span class="sxs-lookup"><span data-stu-id="903ee-352">Leave the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="903ee-353">![Adicionar um modo de criar](aspnet-mvc-4-helpers-forms-and-validation/_static/image11.png "adicionando-a-criar-view.png")</span><span class="sxs-lookup"><span data-stu-id="903ee-353">![Adding a create view](aspnet-mvc-4-helpers-forms-and-validation/_static/image11.png "adding-a-create-view.png")</span></span>

    <span data-ttu-id="903ee-354">*Adicionando o Create View*</span><span class="sxs-lookup"><span data-stu-id="903ee-354">*Adding the Create View*</span></span>
3. <span data-ttu-id="903ee-355">Atualização de **GenreId** e **ArtistId** campos para usar uma lista suspensa, conforme mostrado abaixo:</span><span class="sxs-lookup"><span data-stu-id="903ee-355">Update the **GenreId** and **ArtistId** fields to use a drop-down list as shown below:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample13.cshtml)]

<a id="Ex4Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="903ee-356">Tarefa 3: executando o aplicativo</span><span class="sxs-lookup"><span data-stu-id="903ee-356">Task 3 - Running the Application</span></span>

<span data-ttu-id="903ee-357">Nesta tarefa, você testará que o **StoreManager** **criar** página de exibição exibe um formulário álbum vazio.</span><span class="sxs-lookup"><span data-stu-id="903ee-357">In this task, you will test that the **StoreManager** **Create** View page displays an empty Album form.</span></span>

1. <span data-ttu-id="903ee-358">Pressione **F5** para executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="903ee-358">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="903ee-359">O projeto é iniciado na Home page.</span><span class="sxs-lookup"><span data-stu-id="903ee-359">The project starts in the Home page.</span></span> <span data-ttu-id="903ee-360">Altere a URL para **StoreManager/criar**.</span><span class="sxs-lookup"><span data-stu-id="903ee-360">Change the URL to **/StoreManager/Create**.</span></span> <span data-ttu-id="903ee-361">Verifique se um formulário vazio é exibido para preencher as novas propriedades de álbum.</span><span class="sxs-lookup"><span data-stu-id="903ee-361">Verify that an empty form is displayed for filling the new Album properties.</span></span>

    <span data-ttu-id="903ee-362">![Criar modo de exibição com um formulário vazio](aspnet-mvc-4-helpers-forms-and-validation/_static/image12.png "Create View com um formulário vazio")</span><span class="sxs-lookup"><span data-stu-id="903ee-362">![Create View with an empty form](aspnet-mvc-4-helpers-forms-and-validation/_static/image12.png "Create View with an empty form")</span></span>

    <span data-ttu-id="903ee-363">*Criar modo de exibição com um formulário vazio*</span><span class="sxs-lookup"><span data-stu-id="903ee-363">*Create View with an empty form*</span></span>

<a id="Ex4Task4"></a>

<a id="Task_4_-_Implementing_the_HTTP-POST_Create_Action_Method"></a>
#### <a name="task-4---implementing-the-http-post-create-action-method"></a><span data-ttu-id="903ee-364">Tarefa 4 - implementar o HTTP POST criar método de ação</span><span class="sxs-lookup"><span data-stu-id="903ee-364">Task 4 - Implementing the HTTP-POST Create Action Method</span></span>

<span data-ttu-id="903ee-365">Nesta tarefa, você irá implementar a versão HTTP POST do método de ação de criação que será chamado quando um usuário clica o **salvar** botão.</span><span class="sxs-lookup"><span data-stu-id="903ee-365">In this task, you will implement the HTTP-POST version of the Create action method that will be invoked when a user clicks the **Save** button.</span></span> <span data-ttu-id="903ee-366">O método deve salvar o novo álbum no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="903ee-366">The method should save the new album in the database.</span></span>

1. <span data-ttu-id="903ee-367">Feche o navegador se necessário, para retornar à janela do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="903ee-367">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="903ee-368">Abra **StoreManagerController** classe.</span><span class="sxs-lookup"><span data-stu-id="903ee-368">Open **StoreManagerController** class.</span></span> <span data-ttu-id="903ee-369">Para fazer isso, expanda o **controladores** pasta e clique duas vezes em **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="903ee-369">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
2. <span data-ttu-id="903ee-370">Substituir **HTTP POST criar** código do método de ação com o seguinte:</span><span class="sxs-lookup"><span data-stu-id="903ee-370">Replace **HTTP-POST Create** action method code with the following:</span></span>

    <span data-ttu-id="903ee-371">(Código de trecho - *auxiliares do ASP.NET MVC 4, formulários e validação - Ex4 StoreManagerController HTTP POST Criar ação*)</span><span class="sxs-lookup"><span data-stu-id="903ee-371">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex4 StoreManagerController HTTP- POST Create action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample14.cs)]

    > [!NOTE]
    > <span data-ttu-id="903ee-372">Ação de criação é muito semelhante ao método de ação de edição anterior, mas em vez de configurar o objeto como modificado, ele está sendo adicionado ao contexto.</span><span class="sxs-lookup"><span data-stu-id="903ee-372">The Create action is pretty similar to the previous Edit action method but instead of setting the object as modified, it is being added to the context.</span></span>

<a id="Ex4Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="903ee-373">Tarefa 5: executando o aplicativo</span><span class="sxs-lookup"><span data-stu-id="903ee-373">Task 5 - Running the Application</span></span>

<span data-ttu-id="903ee-374">Nesta tarefa, você testará que o **StoreManager criar** página de exibição permite que você crie um novo álbum e, em seguida, redireciona para a exibição do índice StoreManager.</span><span class="sxs-lookup"><span data-stu-id="903ee-374">In this task, you will test that the **StoreManager Create** View page lets you create a new Album and then redirects to the StoreManager Index View.</span></span>

1. <span data-ttu-id="903ee-375">Pressione **F5** para executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="903ee-375">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="903ee-376">O projeto é iniciado na Home page.</span><span class="sxs-lookup"><span data-stu-id="903ee-376">The project starts in the Home page.</span></span> <span data-ttu-id="903ee-377">Altere a URL para **StoreManager/criar**.</span><span class="sxs-lookup"><span data-stu-id="903ee-377">Change the URL to **/StoreManager/Create**.</span></span> <span data-ttu-id="903ee-378">Preencha todos os campos de formulário com dados para um novo álbum, como a mostrada na figura a seguir:</span><span class="sxs-lookup"><span data-stu-id="903ee-378">Fill all the form fields with data for a new Album, like the one in the following figure:</span></span>

    <span data-ttu-id="903ee-379">![Criando um álbum](aspnet-mvc-4-helpers-forms-and-validation/_static/image13.png "criando um álbum")</span><span class="sxs-lookup"><span data-stu-id="903ee-379">![Creating an Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image13.png "Creating an Album")</span></span>

    <span data-ttu-id="903ee-380">*Criando um álbum*</span><span class="sxs-lookup"><span data-stu-id="903ee-380">*Creating an Album*</span></span>
3. <span data-ttu-id="903ee-381">Verifique se que você é redirecionado para a exibição do índice StoreManager que inclui o novo álbum que acabou de criar.</span><span class="sxs-lookup"><span data-stu-id="903ee-381">Verify that you get redirected to the StoreManager Index View that includes the new Album just created.</span></span>

    <span data-ttu-id="903ee-382">![Novo álbum criado](aspnet-mvc-4-helpers-forms-and-validation/_static/image14.png "novo álbum criado")</span><span class="sxs-lookup"><span data-stu-id="903ee-382">![New Album Created](aspnet-mvc-4-helpers-forms-and-validation/_static/image14.png "New Album Created")</span></span>

    <span data-ttu-id="903ee-383">*Novo álbum criado*</span><span class="sxs-lookup"><span data-stu-id="903ee-383">*New Album Created*</span></span>

<a id="Exercise5"></a>

<a id="Exercise_5_Handling_Deletion"></a>
### <a name="exercise-5-handling-deletion"></a><span data-ttu-id="903ee-384">Exercício 5: Exclusão de tratamento</span><span class="sxs-lookup"><span data-stu-id="903ee-384">Exercise 5: Handling Deletion</span></span>

<span data-ttu-id="903ee-385">A capacidade de excluir álbuns ainda não foi implementada.</span><span class="sxs-lookup"><span data-stu-id="903ee-385">The ability to delete albums is not yet implemented.</span></span> <span data-ttu-id="903ee-386">Isso é o que este exercício sobre.</span><span class="sxs-lookup"><span data-stu-id="903ee-386">This is what this exercise will be about.</span></span> <span data-ttu-id="903ee-387">Como antes, você implementar o cenário de exclusão usando dois métodos separados dentro de **StoreManagerController** classe:</span><span class="sxs-lookup"><span data-stu-id="903ee-387">Like before, you will implement the Delete scenario using two separate methods within the **StoreManagerController** class:</span></span>

1. <span data-ttu-id="903ee-388">Um método de ação exibirá um formulário de confirmação</span><span class="sxs-lookup"><span data-stu-id="903ee-388">One action method will display a confirmation form</span></span>
2. <span data-ttu-id="903ee-389">Um segundo método de ação tratará o envio do formulário</span><span class="sxs-lookup"><span data-stu-id="903ee-389">A second action method will handle the form submission</span></span>

<a id="Ex5Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Delete_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-delete-action-method"></a><span data-ttu-id="903ee-390">Tarefa 1 - Implementando o método de ação de exclusão de HTTP GET</span><span class="sxs-lookup"><span data-stu-id="903ee-390">Task 1 - Implementing the HTTP-GET Delete Action Method</span></span>

<span data-ttu-id="903ee-391">Nesta tarefa, você implementar a versão de HTTP GET do método de ação de exclusão para recuperar informações do álbum.</span><span class="sxs-lookup"><span data-stu-id="903ee-391">In this task, you will implement the HTTP-GET version of the Delete action method to retrieve the album's information.</span></span>

1. <span data-ttu-id="903ee-392">Abra o **começar** solução localizado em **fonte/Ex5-HandlingDeletion/Begin/** pasta.</span><span class="sxs-lookup"><span data-stu-id="903ee-392">Open the **Begin** solution located at **Source/Ex5-HandlingDeletion/Begin/** folder.</span></span> <span data-ttu-id="903ee-393">Caso contrário, você pode continuar usando o **final** solução obtido executando o exercício anterior.</span><span class="sxs-lookup"><span data-stu-id="903ee-393">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="903ee-394">Se você abriu fornecido **começar** solução, você precisará baixar alguns pacotes do NuGet ausentes antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="903ee-394">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="903ee-395">Para fazer isso, clique o **projeto** menu e selecione **gerenciar pacotes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="903ee-395">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="903ee-396">No **gerenciar pacotes NuGet** caixa de diálogo, clique em **restaurar** para baixar os pacotes ausentes.</span><span class="sxs-lookup"><span data-stu-id="903ee-396">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="903ee-397">Por fim, compile a solução clicando **criar** | **compilar solução**.</span><span class="sxs-lookup"><span data-stu-id="903ee-397">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="903ee-398">Uma das vantagens de usar NuGet é que você não precisa enviar todas as bibliotecas no seu projeto, reduzindo o tamanho do projeto.</span><span class="sxs-lookup"><span data-stu-id="903ee-398">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="903ee-399">Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez que você executar o projeto.</span><span class="sxs-lookup"><span data-stu-id="903ee-399">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="903ee-400">É por isso você terá que executar estas etapas depois de abrir uma solução existente neste laboratório.</span><span class="sxs-lookup"><span data-stu-id="903ee-400">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="903ee-401">Abra **StoreManagerController** classe.</span><span class="sxs-lookup"><span data-stu-id="903ee-401">Open **StoreManagerController** class.</span></span> <span data-ttu-id="903ee-402">Para fazer isso, expanda o **controladores** pasta e clique duas vezes em **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="903ee-402">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
3. <span data-ttu-id="903ee-403">A ação de controlador de exclusão é exatamente o mesmo que a ação de controlador de detalhes da loja anterior: ele consulta o **álbum** o objeto de banco de dados usando o **id** fornecidos na URL e retorna o apropriado **exibição**.</span><span class="sxs-lookup"><span data-stu-id="903ee-403">The Delete controller action is exactly the same as the previous Store Details controller action: it queries the **album** object from the database using the **id** provided in the URL and returns the appropriate **View**.</span></span> <span data-ttu-id="903ee-404">Para fazer isso, substitua o HTTP GET **excluir** código do método de ação com o seguinte:</span><span class="sxs-lookup"><span data-stu-id="903ee-404">To do this, replace the HTTP-GET **Delete** action method code with the following:</span></span>

    <span data-ttu-id="903ee-405">(Código de trecho - *validação - ação HTTP GET exclusão Ex5 tratamento Delete, formulários e auxiliares do ASP.NET MVC 4*)</span><span class="sxs-lookup"><span data-stu-id="903ee-405">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex5 Handling Deletion HTTP-GET Delete action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample15.cs)]
4. <span data-ttu-id="903ee-406">Clique dentro do **excluir** método de ação e selecione **adicionar exibição**.</span><span class="sxs-lookup"><span data-stu-id="903ee-406">Right-click inside the **Delete** action method and select **Add View**.</span></span> <span data-ttu-id="903ee-407">Isso abrirá a caixa de diálogo Adicionar modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="903ee-407">This will bring up the Add View dialog.</span></span>
5. <span data-ttu-id="903ee-408">Na caixa de diálogo Adicionar modo de exibição, verifique se o nome de exibição é **excluir**.</span><span class="sxs-lookup"><span data-stu-id="903ee-408">In the Add View dialog, verify that the View name is **Delete**.</span></span> <span data-ttu-id="903ee-409">Selecione o **criar uma exibição fortemente tipada** opção e selecione **álbum (MvcMusicStore.Models)** do **classe modelo** lista suspensa.</span><span class="sxs-lookup"><span data-stu-id="903ee-409">Select the **Create a strongly-typed view** option and select **Album (MvcMusicStore.Models)** from the **Model class** drop-down.</span></span> <span data-ttu-id="903ee-410">Selecione **excluir** do **modelo Scaffold** lista suspensa.</span><span class="sxs-lookup"><span data-stu-id="903ee-410">Select **Delete** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="903ee-411">Deixe os outros campos com seus valores padrão e, em seguida, clique em **adicionar**.</span><span class="sxs-lookup"><span data-stu-id="903ee-411">Leave the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="903ee-412">![Adicionando uma exibição de exclusão](aspnet-mvc-4-helpers-forms-and-validation/_static/image15.png "adicionando uma exibição de exclusão")</span><span class="sxs-lookup"><span data-stu-id="903ee-412">![Adding a Delete View](aspnet-mvc-4-helpers-forms-and-validation/_static/image15.png "Adding a Delete View")</span></span>

    <span data-ttu-id="903ee-413">*Adicionando uma exibição de exclusão*</span><span class="sxs-lookup"><span data-stu-id="903ee-413">*Adding a Delete View*</span></span>
6. <span data-ttu-id="903ee-414">O modelo de exclusão mostra todos os campos do modelo.</span><span class="sxs-lookup"><span data-stu-id="903ee-414">The Delete template shows all the fields from the model.</span></span> <span data-ttu-id="903ee-415">Você mostrará apenas o título do álbum.</span><span class="sxs-lookup"><span data-stu-id="903ee-415">You will show only the album's title.</span></span> <span data-ttu-id="903ee-416">Para fazer isso, substitua o conteúdo do modo de exibição com o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="903ee-416">To do this, replace the content of the view with the following code:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample16.cshtml)]

<a id="Ex05Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="903ee-417">Tarefa 2: executando o aplicativo</span><span class="sxs-lookup"><span data-stu-id="903ee-417">Task 2 - Running the Application</span></span>

<span data-ttu-id="903ee-418">Nesta tarefa, você testará que o **StoreManager** **excluir** página de exibição exibe um formulário de exclusão de confirmação.</span><span class="sxs-lookup"><span data-stu-id="903ee-418">In this task, you will test that the **StoreManager** **Delete** View page displays a confirmation deletion form.</span></span>

1. <span data-ttu-id="903ee-419">Pressione **F5** para executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="903ee-419">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="903ee-420">O projeto é iniciado na Home page.</span><span class="sxs-lookup"><span data-stu-id="903ee-420">The project starts in the Home page.</span></span> <span data-ttu-id="903ee-421">Altere a URL para **/StoreManager**.</span><span class="sxs-lookup"><span data-stu-id="903ee-421">Change the URL to **/StoreManager**.</span></span> <span data-ttu-id="903ee-422">Selecione um álbum excluir clicando **excluir** e verifique se o novo modo de exibição é carregado.</span><span class="sxs-lookup"><span data-stu-id="903ee-422">Select one album to delete by clicking **Delete** and verify that the new view is uploaded.</span></span>

    <span data-ttu-id="903ee-423">![Excluindo um álbum](aspnet-mvc-4-helpers-forms-and-validation/_static/image16.png "excluindo um álbum")</span><span class="sxs-lookup"><span data-stu-id="903ee-423">![Deleting an Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image16.png "Deleting an Album")</span></span>

    <span data-ttu-id="903ee-424">*Excluir um álbum*</span><span class="sxs-lookup"><span data-stu-id="903ee-424">*Deleting an Album*</span></span>

<a id="Ex05Task3"></a>

<a id="Task_3-_Implementing_the_HTTP-POST_Delete_Action_Method"></a>
#### <a name="task-3--implementing-the-http-post-delete-action-method"></a><span data-ttu-id="903ee-425">Tarefa 3-implementar o método de ação de exclusão de HTTP POST</span><span class="sxs-lookup"><span data-stu-id="903ee-425">Task 3- Implementing the HTTP-POST Delete Action Method</span></span>

<span data-ttu-id="903ee-426">Nesta tarefa, você irá implementar a versão HTTP POST do método de ação de exclusão que será chamado quando um usuário clica o **excluir** botão.</span><span class="sxs-lookup"><span data-stu-id="903ee-426">In this task, you will implement the HTTP-POST version of the Delete action method that will be invoked when a user clicks the **Delete** button.</span></span> <span data-ttu-id="903ee-427">O método deve excluir o álbum no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="903ee-427">The method should delete the album in the database.</span></span>

1. <span data-ttu-id="903ee-428">Feche o navegador se necessário, para retornar à janela do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="903ee-428">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="903ee-429">Abra **StoreManagerController** classe.</span><span class="sxs-lookup"><span data-stu-id="903ee-429">Open **StoreManagerController** class.</span></span> <span data-ttu-id="903ee-430">Para fazer isso, expanda o **controladores** pasta e clique duas vezes em **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="903ee-430">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
2. <span data-ttu-id="903ee-431">Substituir **HTTP POST excluir** código do método de ação com o seguinte:</span><span class="sxs-lookup"><span data-stu-id="903ee-431">Replace **HTTP-POST Delete** action method code with the following:</span></span>

    <span data-ttu-id="903ee-432">(Código de trecho - *auxiliares do ASP.NET MVC 4, formulários e validação - ação HTTP POST exclusão Ex5 tratamento excluir*)</span><span class="sxs-lookup"><span data-stu-id="903ee-432">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex5 Handling Deletion HTTP-POST Delete action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample17.cs)]

<a id="Ex5Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="903ee-433">Tarefa 4 - executando o aplicativo</span><span class="sxs-lookup"><span data-stu-id="903ee-433">Task 4 - Running the Application</span></span>

<span data-ttu-id="903ee-434">Nesta tarefa, você testará que o **StoreManager excluir** página de exibição permite que você exclua um álbum e, em seguida, redireciona para a exibição do índice StoreManager.</span><span class="sxs-lookup"><span data-stu-id="903ee-434">In this task, you will test that the **StoreManager Delete** View page lets you delete an Album and then redirects to the StoreManager Index View.</span></span>

1. <span data-ttu-id="903ee-435">Pressione **F5** para executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="903ee-435">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="903ee-436">O projeto é iniciado na Home page.</span><span class="sxs-lookup"><span data-stu-id="903ee-436">The project starts in the Home page.</span></span> <span data-ttu-id="903ee-437">Altere a URL para **/StoreManager**.</span><span class="sxs-lookup"><span data-stu-id="903ee-437">Change the URL to **/StoreManager**.</span></span> <span data-ttu-id="903ee-438">Selecione um álbum excluir clicando **excluir.**</span><span class="sxs-lookup"><span data-stu-id="903ee-438">Select one album to delete by clicking **Delete.**</span></span> <span data-ttu-id="903ee-439">Confirme a exclusão clicando **excluir** botão:</span><span class="sxs-lookup"><span data-stu-id="903ee-439">Confirm the deletion by clicking **Delete** button:</span></span>

    <span data-ttu-id="903ee-440">![Excluindo um álbum](aspnet-mvc-4-helpers-forms-and-validation/_static/image17.png "excluindo um álbum")</span><span class="sxs-lookup"><span data-stu-id="903ee-440">![Deleting an Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image17.png "Deleting an Album")</span></span>

    <span data-ttu-id="903ee-441">*Excluir um álbum*</span><span class="sxs-lookup"><span data-stu-id="903ee-441">*Deleting an Album*</span></span>
3. <span data-ttu-id="903ee-442">Verifique se o álbum foi excluído porque ele não aparece no **índice** página.</span><span class="sxs-lookup"><span data-stu-id="903ee-442">Verify that the album was deleted since it does not appear in the **Index** page.</span></span>

<a id="Exercise6"></a>

<a id="Exercise_6_Adding_Validation"></a>
### <a name="exercise-6-adding-validation"></a><span data-ttu-id="903ee-443">Exercício 6: Adicionando validação</span><span class="sxs-lookup"><span data-stu-id="903ee-443">Exercise 6: Adding Validation</span></span>

<span data-ttu-id="903ee-444">Atualmente, as formas de criar e editar, que você tem em vigor não executar qualquer tipo de validação.</span><span class="sxs-lookup"><span data-stu-id="903ee-444">Currently, the Create and Edit forms you have in place do not perform any kind of validation.</span></span> <span data-ttu-id="903ee-445">Se o usuário deixar um campo obrigatório em branco ou letras de tipo no campo preço, o primeiro erro que será exibida será do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="903ee-445">If the user leaves a required field blank or type letters in the price field, the first error you will get will be from the database.</span></span>

<span data-ttu-id="903ee-446">Você pode adicionar validação para o aplicativo com a adição de anotações de dados à sua classe de modelo.</span><span class="sxs-lookup"><span data-stu-id="903ee-446">You can add validation to the application by adding Data Annotations to your model class.</span></span> <span data-ttu-id="903ee-447">As anotações de dados permitem que descreve as regras a que serem aplicadas às suas propriedades de modelo e ASP.NET MVC cuidará de imposição e exibindo a mensagem apropriada a usuários.</span><span class="sxs-lookup"><span data-stu-id="903ee-447">Data Annotations allow describing the rules you want applied to your model properties, and ASP.NET MVC will take care of enforcing and displaying appropriate message to users.</span></span>

<a id="Ex06Task1"></a>

<a id="Task_1_-_Adding_Data_Annotations"></a>
#### <a name="task-1---adding-data-annotations"></a><span data-ttu-id="903ee-448">Tarefa 1: adicionar anotações de dados</span><span class="sxs-lookup"><span data-stu-id="903ee-448">Task 1 - Adding Data Annotations</span></span>

<span data-ttu-id="903ee-449">Nesta tarefa, você irá adicionar anotações de dados para o modelo de álbum que fará com que a página criar e editar exibir mensagens de validação quando apropriado.</span><span class="sxs-lookup"><span data-stu-id="903ee-449">In this task, you will add Data Annotations to the Album Model that will make the Create and Edit page display validation messages when appropriate.</span></span>

<span data-ttu-id="903ee-450">Para uma classe simple do modelo, adicionar uma anotação de dados é tratado apenas adicionando uma **usando** instrução para **System.ComponentModel.DataAnnotation**, em seguida, colocar um **[obrigatório]**atributo as propriedades adequadas.</span><span class="sxs-lookup"><span data-stu-id="903ee-450">For a simple Model class, adding a Data Annotation is just handled by adding a **using** statement for **System.ComponentModel.DataAnnotation**, then placing a **[Required]** attribute on the appropriate properties.</span></span> <span data-ttu-id="903ee-451">O exemplo a seguir faria o **nome** um campo obrigatório no modo de exibição da propriedade.</span><span class="sxs-lookup"><span data-stu-id="903ee-451">The following example would make the **Name** property a required field in the View.</span></span>

[!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample18.cs)]

<span data-ttu-id="903ee-452">Isso é um pouco mais complexo em casos como esse aplicativo onde o modelo de dados de entidade é gerado.</span><span class="sxs-lookup"><span data-stu-id="903ee-452">This is a little more complex in cases like this application where the Entity Data Model is generated.</span></span> <span data-ttu-id="903ee-453">Se você adicionou anotações de dados diretamente para as classes de modelo, ele seriam substituídos se você atualizar o modelo do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="903ee-453">If you added Data Annotations directly to the model classes, they would be overwritten if you update the model from the database.</span></span> <span data-ttu-id="903ee-454">Em vez disso, você pode fazer uso de classes parciais de metadados que exista para manter as anotações e estão associadas com o modelo de classes usando o **[MetadataType]** atributo.</span><span class="sxs-lookup"><span data-stu-id="903ee-454">Instead, you can make use of metadata partial classes which will exist to hold the annotations and are associated with the model classes using the **[MetadataType]** attribute.</span></span>

1. <span data-ttu-id="903ee-455">Abra o **começar** solução localizado em **fonte/Ex6-AddingValidation/Begin/** pasta.</span><span class="sxs-lookup"><span data-stu-id="903ee-455">Open the **Begin** solution located at **Source/Ex6-AddingValidation/Begin/** folder.</span></span> <span data-ttu-id="903ee-456">Caso contrário, você pode continuar usando o **final** solução obtido executando o exercício anterior.</span><span class="sxs-lookup"><span data-stu-id="903ee-456">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="903ee-457">Se você abriu fornecido **começar** solução, você precisará baixar alguns pacotes do NuGet ausentes antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="903ee-457">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="903ee-458">Para fazer isso, clique o **projeto** menu e selecione **gerenciar pacotes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="903ee-458">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="903ee-459">No **gerenciar pacotes NuGet** caixa de diálogo, clique em **restaurar** para baixar os pacotes ausentes.</span><span class="sxs-lookup"><span data-stu-id="903ee-459">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="903ee-460">Por fim, compile a solução clicando **criar** | **compilar solução**.</span><span class="sxs-lookup"><span data-stu-id="903ee-460">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="903ee-461">Uma das vantagens de usar NuGet é que você não precisa enviar todas as bibliotecas no seu projeto, reduzindo o tamanho do projeto.</span><span class="sxs-lookup"><span data-stu-id="903ee-461">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="903ee-462">Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez que você executar o projeto.</span><span class="sxs-lookup"><span data-stu-id="903ee-462">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="903ee-463">É por isso você terá que executar estas etapas depois de abrir uma solução existente neste laboratório.</span><span class="sxs-lookup"><span data-stu-id="903ee-463">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="903ee-464">Abra o **Album.cs** do **modelos** pasta.</span><span class="sxs-lookup"><span data-stu-id="903ee-464">Open the **Album.cs** from the **Models** folder.</span></span>
3. <span data-ttu-id="903ee-465">Substituir **Album.cs** de conteúdo com o código realçado, para que ele se parece com o seguinte:</span><span class="sxs-lookup"><span data-stu-id="903ee-465">Replace **Album.cs** content with the highlighted code, so that it looks like the following:</span></span>

    > [!NOTE]
    > <span data-ttu-id="903ee-466">A linha **[DisplayFormat(ConvertEmptyStringToNull=false)]** indica que as cadeias de caracteres vazias do modelo não ser convertidas em nulo quando o campo de dados é atualizado na fonte de dados.</span><span class="sxs-lookup"><span data-stu-id="903ee-466">The line **[DisplayFormat(ConvertEmptyStringToNull=false)]** indicates that empty strings from the model won't be converted to null when the data field is updated in the data source.</span></span> <span data-ttu-id="903ee-467">Essa configuração evitará uma exceção quando o Entity Framework atribui valores nulos para o modelo antes de anotação de dados valida os campos.</span><span class="sxs-lookup"><span data-stu-id="903ee-467">This setting will avoid an exception when the Entity Framework assigns null values to the model before Data Annotation validates the fields.</span></span>

    <span data-ttu-id="903ee-468">(Código de trecho - *auxiliares do ASP.NET MVC 4, formulários e validação - classe parcial de metadados de álbum Ex6*)</span><span class="sxs-lookup"><span data-stu-id="903ee-468">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex6 Album metadata partial class*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample19.cs)]

    > [!NOTE]
    > <span data-ttu-id="903ee-469">Isso **álbum** classe parcial tem um **MetadataType** atributo que aponta para o **AlbumMetaData** classe para as anotações de dados.</span><span class="sxs-lookup"><span data-stu-id="903ee-469">This **Album** partial class has a **MetadataType** attribute which points to the **AlbumMetaData** class for the Data Annotations.</span></span> <span data-ttu-id="903ee-470">Estes são alguns dos atributos de anotação de dados que você está usando para anotar o modelo álbum:</span><span class="sxs-lookup"><span data-stu-id="903ee-470">These are some of the Data Annotation attributes you are using to annotate the Album model:</span></span>
    > 
    > - <span data-ttu-id="903ee-471">Exigido - indica que a propriedade é um campo obrigatório</span><span class="sxs-lookup"><span data-stu-id="903ee-471">Required - Indicates that the property is a required field</span></span>
    > - <span data-ttu-id="903ee-472">DisplayName - define o texto a ser usada em campos de formulário e mensagens de validação</span><span class="sxs-lookup"><span data-stu-id="903ee-472">DisplayName - Defines the text to be used on form fields and validation messages</span></span>
    > - <span data-ttu-id="903ee-473">DisplayFormat - Especifica como os campos de dados são exibidos e formatados.</span><span class="sxs-lookup"><span data-stu-id="903ee-473">DisplayFormat - Specifies how data fields are displayed and formatted.</span></span>
    > - <span data-ttu-id="903ee-474">StringLength - define um comprimento máximo de um campo de cadeia de caracteres</span><span class="sxs-lookup"><span data-stu-id="903ee-474">StringLength - Defines a maximum length for a string field</span></span>
    > - <span data-ttu-id="903ee-475">Intervalo - retorna um valor mínimo e máximo para um campo numérico</span><span class="sxs-lookup"><span data-stu-id="903ee-475">Range - Gives a maximum and minimum value for a numeric field</span></span>
    > - <span data-ttu-id="903ee-476">ScaffoldColumn - permite ocultar campos de formulários do editor</span><span class="sxs-lookup"><span data-stu-id="903ee-476">ScaffoldColumn - Allows hiding fields from editor forms</span></span>

<a id="Ex06Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="903ee-477">Tarefa 2: executando o aplicativo</span><span class="sxs-lookup"><span data-stu-id="903ee-477">Task 2 - Running the Application</span></span>

<span data-ttu-id="903ee-478">Nesta tarefa, você testará a criar e editar páginas validam campos, usando os nomes de exibição escolhidos na última tarefa.</span><span class="sxs-lookup"><span data-stu-id="903ee-478">In this task, you will test that the Create and Edit pages validate fields, using the display names chosen in the last task.</span></span>

1. <span data-ttu-id="903ee-479">Pressione **F5** para executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="903ee-479">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="903ee-480">O projeto é iniciado na Home page.</span><span class="sxs-lookup"><span data-stu-id="903ee-480">The project starts in the Home page.</span></span> <span data-ttu-id="903ee-481">Altere a URL para **StoreManager/criar**.</span><span class="sxs-lookup"><span data-stu-id="903ee-481">Change the URL to **/StoreManager/Create**.</span></span> <span data-ttu-id="903ee-482">Verificar se os nomes de exibição coincidem com aqueles na classe parcial (como **álbum arte URL** em vez de **AlbumArtUrl**)</span><span class="sxs-lookup"><span data-stu-id="903ee-482">Verify that the display names match the ones in the partial class (like **Album Art URL** instead of **AlbumArtUrl**)</span></span>
3. <span data-ttu-id="903ee-483">Clique em **criar**, sem preencher o formulário.</span><span class="sxs-lookup"><span data-stu-id="903ee-483">Click **Create**, without filling the form.</span></span> <span data-ttu-id="903ee-484">Verifique se que você obtém as mensagens de validação correspondente.</span><span class="sxs-lookup"><span data-stu-id="903ee-484">Verify that you get the corresponding validation messages.</span></span>

    <span data-ttu-id="903ee-485">![Validar os campos na página criar](aspnet-mvc-4-helpers-forms-and-validation/_static/image18.png "validado campos na página de criação")</span><span class="sxs-lookup"><span data-stu-id="903ee-485">![Validated fields in the Create page](aspnet-mvc-4-helpers-forms-and-validation/_static/image18.png "Validated fields in the Create page")</span></span>

    <span data-ttu-id="903ee-486">*Validado campos na página de criação*</span><span class="sxs-lookup"><span data-stu-id="903ee-486">*Validated fields in the Create page*</span></span>
4. <span data-ttu-id="903ee-487">Você pode verificar se o mesmo ocorre com o **editar** página.</span><span class="sxs-lookup"><span data-stu-id="903ee-487">You can verify that the same occurs with the **Edit** page.</span></span> <span data-ttu-id="903ee-488">Altere a URL para **/StoreManager/Edit/1** e verificar se os nomes de exibição coincidem com aqueles na classe parcial (como **álbum arte URL** em vez de **AlbumArtUrl**).</span><span class="sxs-lookup"><span data-stu-id="903ee-488">Change the URL to **/StoreManager/Edit/1** and verify that the display names match the ones in the partial class (like **Album Art URL** instead of **AlbumArtUrl**).</span></span> <span data-ttu-id="903ee-489">Vazio a **título** e **preço** campos e clique em **salvar**.</span><span class="sxs-lookup"><span data-stu-id="903ee-489">Empty the **Title** and **Price** fields and click **Save**.</span></span> <span data-ttu-id="903ee-490">Verifique se que você obtém as mensagens de validação correspondente.</span><span class="sxs-lookup"><span data-stu-id="903ee-490">Verify that you get the corresponding validation messages.</span></span>

    ![Validado campos na página de edição](aspnet-mvc-4-helpers-forms-and-validation/_static/image19.png)

    <span data-ttu-id="903ee-492">Validado campos na página de edição</span><span class="sxs-lookup"><span data-stu-id="903ee-492">*Validated fields in the Edit page*</span></span>

<a id="Exercise7"></a>

<a id="Exercise_7_Using_Unobtrusive_jQuery_at_Client_Side"></a>
### <a name="exercise-7-using-unobtrusive-jquery-at-client-side"></a><span data-ttu-id="903ee-493">Exercício 7: Usando jQuery discreto no lado do cliente</span><span class="sxs-lookup"><span data-stu-id="903ee-493">Exercise 7: Using Unobtrusive jQuery at Client Side</span></span>

<span data-ttu-id="903ee-494">Neste exercício, você aprenderá como habilitar a validação jQuery MVC 4 discreto no lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="903ee-494">In this exercise, you will learn how to enable MVC 4 Unobtrusive jQuery validation at client side.</span></span>

> [!NOTE]
> <span data-ttu-id="903ee-495">O jQuery discreto usa prefixo dados ajax JavaScript para invocar métodos de ação no servidor em vez de scripts de cliente forma intrusiva emissão embutido.</span><span class="sxs-lookup"><span data-stu-id="903ee-495">The Unobtrusive jQuery uses data-ajax prefix JavaScript to invoke action methods on the server rather than intrusively emitting inline client scripts.</span></span>


<a id="Ex7Task1"></a>

<a id="Task_1_-_Running_the_Application_before_Enabling_Unobtrusive_jQuery"></a>
#### <a name="task-1---running-the-application-before-enabling-unobtrusive-jquery"></a><span data-ttu-id="903ee-496">Tarefa 1: executar o aplicativo antes de habilitar discreto jQuery</span><span class="sxs-lookup"><span data-stu-id="903ee-496">Task 1 - Running the Application before Enabling Unobtrusive jQuery</span></span>

<span data-ttu-id="903ee-497">Nesta tarefa, você executará o aplicativo antes de incluir jQuery para comparar os dois modelos de validação.</span><span class="sxs-lookup"><span data-stu-id="903ee-497">In this task, you will run the application before including jQuery in order to compare both validation models.</span></span>

1. <span data-ttu-id="903ee-498">Abra o **começar** solução localizado em **fonte/Ex7-UnobtrusivejQueryValidation/Begin/** pasta.</span><span class="sxs-lookup"><span data-stu-id="903ee-498">Open the **Begin** solution located at **Source/Ex7-UnobtrusivejQueryValidation/Begin/** folder.</span></span> <span data-ttu-id="903ee-499">Caso contrário, você pode continuar usando o **final** solução obtido executando o exercício anterior.</span><span class="sxs-lookup"><span data-stu-id="903ee-499">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="903ee-500">Se você abriu fornecido **começar** solução, você precisará baixar alguns pacotes do NuGet ausentes antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="903ee-500">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="903ee-501">Para fazer isso, clique o **projeto** menu e selecione **gerenciar pacotes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="903ee-501">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="903ee-502">No **gerenciar pacotes NuGet** caixa de diálogo, clique em **restaurar** para baixar os pacotes ausentes.</span><span class="sxs-lookup"><span data-stu-id="903ee-502">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="903ee-503">Por fim, compile a solução clicando **criar** | **compilar solução**.</span><span class="sxs-lookup"><span data-stu-id="903ee-503">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="903ee-504">Uma das vantagens de usar NuGet é que você não precisa enviar todas as bibliotecas no seu projeto, reduzindo o tamanho do projeto.</span><span class="sxs-lookup"><span data-stu-id="903ee-504">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="903ee-505">Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez que você executar o projeto.</span><span class="sxs-lookup"><span data-stu-id="903ee-505">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="903ee-506">É por isso você terá que executar estas etapas depois de abrir uma solução existente neste laboratório.</span><span class="sxs-lookup"><span data-stu-id="903ee-506">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="903ee-507">Pressione **F5** para executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="903ee-507">Press **F5** to run the application.</span></span>
3. <span data-ttu-id="903ee-508">O projeto é iniciado na Home page.</span><span class="sxs-lookup"><span data-stu-id="903ee-508">The project starts in the Home page.</span></span> <span data-ttu-id="903ee-509">Procurar **/StoreManager/criar** e clique em **criar** sem preencher o formulário para verificar se você receber mensagens de validação:</span><span class="sxs-lookup"><span data-stu-id="903ee-509">Browse **/StoreManager/Create** and click **Create** without filling the form to verify that you get validation messages:</span></span>

    <span data-ttu-id="903ee-510">![Validação de cliente desabilitada](aspnet-mvc-4-helpers-forms-and-validation/_static/image20.png "desabilitada a validação do cliente")</span><span class="sxs-lookup"><span data-stu-id="903ee-510">![Client validation disabled](aspnet-mvc-4-helpers-forms-and-validation/_static/image20.png "Client validation disabled")</span></span>

    <span data-ttu-id="903ee-511">*Validação de cliente desabilitada*</span><span class="sxs-lookup"><span data-stu-id="903ee-511">*Client validation disabled*</span></span>
4. <span data-ttu-id="903ee-512">No navegador, abra o código-fonte HTML:</span><span class="sxs-lookup"><span data-stu-id="903ee-512">In the browser, open the HTML source code:</span></span>

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample20.html)]

<a id="Ex7Task2"></a>

<a id="Task_2_-_Enabling_Unobtrusive_Client_Validation"></a>
#### <a name="task-2---enabling-unobtrusive-client-validation"></a><span data-ttu-id="903ee-513">Tarefa 2 - Habilitar validação de cliente discreto</span><span class="sxs-lookup"><span data-stu-id="903ee-513">Task 2 - Enabling Unobtrusive Client Validation</span></span>

<span data-ttu-id="903ee-514">Nesta tarefa, você habilitará o jQuery **validação do cliente discreto** de **Web. config** arquivo, que é definido como false em todos os novos projetos do ASP.NET MVC 4 por padrão.</span><span class="sxs-lookup"><span data-stu-id="903ee-514">In this task, you will enable jQuery **unobtrusive client validation** from **Web.config** file, which is by default set to false in all new ASP.NET MVC 4 projects.</span></span> <span data-ttu-id="903ee-515">Além disso, você irá adicionar que referências para tornar jQuery trabalho discreto validação do cliente de scripts necessários.</span><span class="sxs-lookup"><span data-stu-id="903ee-515">Additionally, you will add the necessary scripts references to make jQuery Unobtrusive Client Validation work.</span></span>

1. <span data-ttu-id="903ee-516">Abra **Web. config** arquivo na raiz do projeto e certifique-se de que o **ClientValidationEnabled** e **UnobtrusiveJavaScriptEnabled** valores de chaves são definidos como **true**.</span><span class="sxs-lookup"><span data-stu-id="903ee-516">Open **Web.Config** file at project root, and make sure that the **ClientValidationEnabled** and **UnobtrusiveJavaScriptEnabled** keys values are set to **true**.</span></span>

    [!code-xml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample21.xml)]

    > [!NOTE]
    > <span data-ttu-id="903ee-517">Você também pode habilitar a validação do cliente pelo código em Global.asax.cs para obter os mesmos resultados:</span><span class="sxs-lookup"><span data-stu-id="903ee-517">You can also enable client validation by code at Global.asax.cs to get the same results:</span></span>
    > 
    > <span data-ttu-id="903ee-518">**HtmlHelper.ClientValidationEnabled = true;**</span><span class="sxs-lookup"><span data-stu-id="903ee-518">**HtmlHelper.ClientValidationEnabled = true;**</span></span>
    > 
    > <span data-ttu-id="903ee-519">Além disso, você pode atribuir o atributo ClientValidationEnabled em qualquer controlador de ter um comportamento personalizado.</span><span class="sxs-lookup"><span data-stu-id="903ee-519">Additionally, you can assign ClientValidationEnabled attribute into any controller to have a custom behavior.</span></span>
2. <span data-ttu-id="903ee-520">Abra **Create.cshtml** em **Views\StoreManager**.</span><span class="sxs-lookup"><span data-stu-id="903ee-520">Open **Create.cshtml** at **Views\StoreManager**.</span></span>
3. <span data-ttu-id="903ee-521">Verifique se os seguintes arquivos de script, **jquery.validate** e **jquery.validate.unobtrusive**, são referenciados na através do modo de exibição de &quot; **~/bundles/jqueryval** &quot; pacote.</span><span class="sxs-lookup"><span data-stu-id="903ee-521">Make sure the following script files, **jquery.validate** and **jquery.validate.unobtrusive**, are referenced in the view throught the &quot;**~/bundles/jqueryval**&quot; bundle.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample22.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="903ee-522">Todas essas bibliotecas jQuery são incluídas em novos projetos do MVC 4.</span><span class="sxs-lookup"><span data-stu-id="903ee-522">All these jQuery libraries are included in MVC 4 new projects.</span></span> <span data-ttu-id="903ee-523">Você pode encontrar mais bibliotecas no **/Scripts** pasta do projeto.</span><span class="sxs-lookup"><span data-stu-id="903ee-523">You can find more libraries in the **/Scripts** folder of you project.</span></span>
    > 
    > <span data-ttu-id="903ee-524">Para fazer essa validação bibliotecas de trabalho, você precisa adicionar uma referência à biblioteca do framework jQuery.</span><span class="sxs-lookup"><span data-stu-id="903ee-524">In order to make this validation libraries work, you need to add a reference to the jQuery framework library.</span></span> <span data-ttu-id="903ee-525">Desde que esta referência já foi adicionada a  **\_cshtml** arquivo, não é necessário adicioná-lo neste modo de exibição específico.</span><span class="sxs-lookup"><span data-stu-id="903ee-525">Since this reference is already added in the **\_Layout.cshtml** file, you do not need to add it in this particular view.</span></span>

<a id="Ex7Task3"></a>

<a id="Task_3_-_Running_the_Application_Using_Unobtrusive_jQuery_Validation"></a>
#### <a name="task-3---running-the-application-using-unobtrusive-jquery-validation"></a><span data-ttu-id="903ee-526">Tarefa 3: executando o aplicativo usando discreto jQuery validação</span><span class="sxs-lookup"><span data-stu-id="903ee-526">Task 3 - Running the Application Using Unobtrusive jQuery Validation</span></span>

<span data-ttu-id="903ee-527">Nesta tarefa, você testará que o **StoreManager** criar exibição de modelo executa a validação do lado do cliente usando bibliotecas jQuery quando o usuário cria um novo álbum.</span><span class="sxs-lookup"><span data-stu-id="903ee-527">In this task, you will test that the **StoreManager** create view template performs client side validation using jQuery libraries when the user creates a new album.</span></span>

1. <span data-ttu-id="903ee-528">Pressione **F5** para executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="903ee-528">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="903ee-529">O projeto é iniciado na Home page.</span><span class="sxs-lookup"><span data-stu-id="903ee-529">The project starts in the Home page.</span></span> <span data-ttu-id="903ee-530">Procurar **/StoreManager/criar** e clique em **criar** sem preencher o formulário para verificar se você receber mensagens de validação:</span><span class="sxs-lookup"><span data-stu-id="903ee-530">Browse **/StoreManager/Create** and click **Create** without filling the form to verify that you get validation messages:</span></span>

    <span data-ttu-id="903ee-531">![Validação do cliente com o jQuery habilitado](aspnet-mvc-4-helpers-forms-and-validation/_static/image21.png "validação do cliente com o jQuery habilitado")</span><span class="sxs-lookup"><span data-stu-id="903ee-531">![Client validation with jQuery enabled](aspnet-mvc-4-helpers-forms-and-validation/_static/image21.png "Client validation with jQuery enabled")</span></span>

    <span data-ttu-id="903ee-532">*Validação do cliente com o jQuery habilitado*</span><span class="sxs-lookup"><span data-stu-id="903ee-532">*Client validation with jQuery enabled*</span></span>
3. <span data-ttu-id="903ee-533">No navegador, abra o código-fonte para criar modo de exibição:</span><span class="sxs-lookup"><span data-stu-id="903ee-533">In the browser, open the source code for Create view:</span></span>

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample23.html)]

    > [!NOTE]
    > <span data-ttu-id="903ee-534">Para cada regra de validação de cliente jQuery discreto adiciona um atributo com dados-val -*rulename*=&quot;*mensagem*&quot;.</span><span class="sxs-lookup"><span data-stu-id="903ee-534">For each client validation rule, Unobtrusive jQuery adds an attribute with data-val-*rulename*=&quot;*message*&quot;.</span></span> <span data-ttu-id="903ee-535">Abaixo está uma lista de marcas que Unobtrusive jQuery insere o campo de entrada html para executar a validação do cliente:</span><span class="sxs-lookup"><span data-stu-id="903ee-535">Below is a list of tags that Unobtrusive jQuery inserts into the html input field to perform client validation:</span></span>
    > 
    > - <span data-ttu-id="903ee-536">Valor de dados</span><span class="sxs-lookup"><span data-stu-id="903ee-536">Data-val</span></span>
    > - <span data-ttu-id="903ee-537">Número de valor de dados</span><span class="sxs-lookup"><span data-stu-id="903ee-537">Data-val-number</span></span>
    > - <span data-ttu-id="903ee-538">Intervalo de valor de dados</span><span class="sxs-lookup"><span data-stu-id="903ee-538">Data-val-range</span></span>
    > - <span data-ttu-id="903ee-539">Data-val-intervalo-min / dados val-intervalo máximo</span><span class="sxs-lookup"><span data-stu-id="903ee-539">Data-val-range-min / Data-val-range-max</span></span>
    > - <span data-ttu-id="903ee-540">Dados val necessários</span><span class="sxs-lookup"><span data-stu-id="903ee-540">Data-val-required</span></span>
    > - <span data-ttu-id="903ee-541">Data-val-length</span><span class="sxs-lookup"><span data-stu-id="903ee-541">Data-val-length</span></span>
    > - <span data-ttu-id="903ee-542">Data-val-tamanho-max / dados val-comprimento mínimo</span><span class="sxs-lookup"><span data-stu-id="903ee-542">Data-val-length-max / Data-val-length-min</span></span>
    > 
    > <span data-ttu-id="903ee-543">Todos os valores de dados são preenchidos com modelo **anotação de dados**.</span><span class="sxs-lookup"><span data-stu-id="903ee-543">All the data values are filled with model **Data Annotation**.</span></span> <span data-ttu-id="903ee-544">Em seguida, toda a lógica que funciona no lado do servidor pode ser executada no lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="903ee-544">Then, all the logic that works at server side can be run at client side.</span></span> <span data-ttu-id="903ee-545">Por exemplo, o atributo preço tem a anotação de dados a seguir no modelo:</span><span class="sxs-lookup"><span data-stu-id="903ee-545">For example, Price attribute has the following data annotation in the model:</span></span>
    > 
    > [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample24.cs)]
    > 
    > <span data-ttu-id="903ee-546">Depois de usar jQuery discreto, o código gerado é:</span><span class="sxs-lookup"><span data-stu-id="903ee-546">After using Unobtrusive jQuery, the generated code is:</span></span>
    >  
    > [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample25.html)]

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="903ee-547">Resumo</span><span class="sxs-lookup"><span data-stu-id="903ee-547">Summary</span></span>

<span data-ttu-id="903ee-548">Ao concluir este laboratório prático, você aprendeu como habilitar usuários alterar os dados armazenados no banco de dados com o uso dos seguintes:</span><span class="sxs-lookup"><span data-stu-id="903ee-548">By completing this Hands-On Lab you have learned how to enable users to change the data stored in the database with the use of the following:</span></span>

- <span data-ttu-id="903ee-549">Ações do controlador como índice, criar, editar, excluir</span><span class="sxs-lookup"><span data-stu-id="903ee-549">Controller actions like Index, Create, Edit, Delete</span></span>
- <span data-ttu-id="903ee-550">Recurso de scaffolding de ASP.NET MVC para exibir as propriedades em uma tabela HTML</span><span class="sxs-lookup"><span data-stu-id="903ee-550">ASP.NET MVC's scaffolding feature for displaying properties in an HTML table</span></span>
- <span data-ttu-id="903ee-551">Experiência de auxiliares HTML personalizados para melhorar o usuário</span><span class="sxs-lookup"><span data-stu-id="903ee-551">Custom HTML helpers to improve user experience</span></span>
- <span data-ttu-id="903ee-552">Métodos de ação reagem a HTTP GET ou chamadas HTTP POST</span><span class="sxs-lookup"><span data-stu-id="903ee-552">Action methods that react to either HTTP-GET or HTTP-POST calls</span></span>
- <span data-ttu-id="903ee-553">Um modelo do editor de compartilhado para modelos de exibição semelhantes de como criar e editar</span><span class="sxs-lookup"><span data-stu-id="903ee-553">A shared editor template for similar View templates like Create and Edit</span></span>
- <span data-ttu-id="903ee-554">Elementos de formulário, como listas suspensas</span><span class="sxs-lookup"><span data-stu-id="903ee-554">Form elements like drop-downs</span></span>
- <span data-ttu-id="903ee-555">Anotações de dados para a validação de modelo</span><span class="sxs-lookup"><span data-stu-id="903ee-555">Data annotations for Model validation</span></span>
- <span data-ttu-id="903ee-556">Validação do lado do cliente usando a biblioteca discreto jQuery</span><span class="sxs-lookup"><span data-stu-id="903ee-556">Client Side Validation using jQuery Unobtrusive library</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="903ee-557">Apêndice a: instalar o Visual Studio Express 2012 para Web</span><span class="sxs-lookup"><span data-stu-id="903ee-557">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="903ee-558">Você pode instalar **Microsoft Visual Studio Express 2012 para Web** ou outro &quot;Express&quot; versão usando o  **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="903ee-558">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="903ee-559">As instruções a seguir guiá-lo pelas etapas necessárias para instalar o *Visual studio Express 2012 para Web* usando *Microsoft Web Platform Installer*.</span><span class="sxs-lookup"><span data-stu-id="903ee-559">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="903ee-560">Vá para [ [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="903ee-560">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="903ee-561">Como alternativa, se você já tiver instalado o Web Platform Installer, você pode abri-la e procure o produto &quot; *Visual Studio Express 2012 para Web com o SDK do Windows Azure*&quot;.</span><span class="sxs-lookup"><span data-stu-id="903ee-561">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;*Visual Studio Express 2012 for Web with Windows Azure SDK*&quot;.</span></span>
2. <span data-ttu-id="903ee-562">Clique em **instalar agora**.</span><span class="sxs-lookup"><span data-stu-id="903ee-562">Click on **Install Now**.</span></span> <span data-ttu-id="903ee-563">Se você não tem **Web Platform Installer** você será redirecionado para baixar e instalá-lo primeiro.</span><span class="sxs-lookup"><span data-stu-id="903ee-563">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="903ee-564">Uma vez **Web Platform Installer** é aberto, clique em **instalar** para iniciar a instalação.</span><span class="sxs-lookup"><span data-stu-id="903ee-564">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="903ee-565">![Instalar Visual Studio Express](aspnet-mvc-4-helpers-forms-and-validation/_static/image22.png "instalar Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="903ee-565">![Install Visual Studio Express](aspnet-mvc-4-helpers-forms-and-validation/_static/image22.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="903ee-566">*Instalar Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="903ee-566">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="903ee-567">Leia os termos e licenças de todos os produtos e clique em **aceito** para continuar.</span><span class="sxs-lookup"><span data-stu-id="903ee-567">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Aceitar os termos de licença](aspnet-mvc-4-helpers-forms-and-validation/_static/image23.png)

    <span data-ttu-id="903ee-569">*Aceitar os termos de licença*</span><span class="sxs-lookup"><span data-stu-id="903ee-569">*Accepting the license terms*</span></span>
5. <span data-ttu-id="903ee-570">Aguarde até que o processo de download e instalação seja concluída.</span><span class="sxs-lookup"><span data-stu-id="903ee-570">Wait until the downloading and installation process completes.</span></span>

    ![Progresso da instalação](aspnet-mvc-4-helpers-forms-and-validation/_static/image24.png)

    <span data-ttu-id="903ee-572">*Progresso da instalação*</span><span class="sxs-lookup"><span data-stu-id="903ee-572">*Installation progress*</span></span>
6. <span data-ttu-id="903ee-573">Quando a instalação for concluída, clique em **concluir**.</span><span class="sxs-lookup"><span data-stu-id="903ee-573">When the installation completes, click **Finish**.</span></span>

    ![Instalação concluída](aspnet-mvc-4-helpers-forms-and-validation/_static/image25.png)

    <span data-ttu-id="903ee-575">*Instalação concluída*</span><span class="sxs-lookup"><span data-stu-id="903ee-575">*Installation completed*</span></span>
7. <span data-ttu-id="903ee-576">Clique em **saída** para fechar o Web Platform Installer.</span><span class="sxs-lookup"><span data-stu-id="903ee-576">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="903ee-577">Para abrir o Visual Studio Express para Web, vá para o **iniciar** tela e comece a escrever &quot; **VS Express**&quot;, em seguida, clique no **VS Express para Web** lado a lado.</span><span class="sxs-lookup"><span data-stu-id="903ee-577">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express para o bloco de Web](aspnet-mvc-4-helpers-forms-and-validation/_static/image26.png)

    <span data-ttu-id="903ee-579">*VS Express para o bloco de Web*</span><span class="sxs-lookup"><span data-stu-id="903ee-579">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a><span data-ttu-id="903ee-580">Apêndice b: usar trechos de código</span><span class="sxs-lookup"><span data-stu-id="903ee-580">Appendix B: Using Code Snippets</span></span>

<span data-ttu-id="903ee-581">Com trechos de código, você tem todo o código que é necessário ao seu alcance.</span><span class="sxs-lookup"><span data-stu-id="903ee-581">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="903ee-582">O documento de laboratório informará exatamente quando você pode usá-los, conforme mostrado na figura a seguir.</span><span class="sxs-lookup"><span data-stu-id="903ee-582">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="903ee-583">![Usando trechos de código do Visual Studio para inserir o código no seu projeto](aspnet-mvc-4-helpers-forms-and-validation/_static/image27.png "trechos de código com o Visual Studio para inserir código em seu projeto")</span><span class="sxs-lookup"><span data-stu-id="903ee-583">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-helpers-forms-and-validation/_static/image27.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="903ee-584">*Usando trechos de código do Visual Studio para inserir código em seu projeto*</span><span class="sxs-lookup"><span data-stu-id="903ee-584">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="903ee-585">***Para adicionar um trecho de código usando o teclado (apenas c#)***</span><span class="sxs-lookup"><span data-stu-id="903ee-585">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="903ee-586">Posicione o cursor onde você deseja inserir o código.</span><span class="sxs-lookup"><span data-stu-id="903ee-586">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="903ee-587">Comece a digitar o nome do trecho (sem espaços ou hifens).</span><span class="sxs-lookup"><span data-stu-id="903ee-587">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="903ee-588">Observe como IntelliSense exibe os nomes dos trechos de código correspondentes.</span><span class="sxs-lookup"><span data-stu-id="903ee-588">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="903ee-589">Selecione o trecho correto (ou continue digitando até que o nome do trecho de código inteiro é selecionado).</span><span class="sxs-lookup"><span data-stu-id="903ee-589">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="903ee-590">Pressione a tecla Tab duas vezes para inserir o trecho de código no local do cursor.</span><span class="sxs-lookup"><span data-stu-id="903ee-590">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="903ee-591">![Comece a digitar o nome do fragmento](aspnet-mvc-4-helpers-forms-and-validation/_static/image28.png "comece a digitar o nome do trecho de código")</span><span class="sxs-lookup"><span data-stu-id="903ee-591">![Start typing the snippet name](aspnet-mvc-4-helpers-forms-and-validation/_static/image28.png "Start typing the snippet name")</span></span>

<span data-ttu-id="903ee-592">*Comece a digitar o nome do trecho de código*</span><span class="sxs-lookup"><span data-stu-id="903ee-592">*Start typing the snippet name*</span></span>

<span data-ttu-id="903ee-593">![Pressione Tab para selecionar o trecho de código realçado](aspnet-mvc-4-helpers-forms-and-validation/_static/image29.png "pressione Tab para selecionar o trecho de código realçado")</span><span class="sxs-lookup"><span data-stu-id="903ee-593">![Press Tab to select the highlighted snippet](aspnet-mvc-4-helpers-forms-and-validation/_static/image29.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="903ee-594">*Pressione Tab para selecionar o trecho de código realçado*</span><span class="sxs-lookup"><span data-stu-id="903ee-594">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="903ee-595">![Pressione Tab novamente e o trecho expandirá](aspnet-mvc-4-helpers-forms-and-validation/_static/image30.png "expandirá pressione Tab novamente e o trecho de código")</span><span class="sxs-lookup"><span data-stu-id="903ee-595">![Press Tab again and the snippet will expand](aspnet-mvc-4-helpers-forms-and-validation/_static/image30.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="903ee-596">*Pressione Tab novamente e o trecho de código serão expandida*</span><span class="sxs-lookup"><span data-stu-id="903ee-596">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="903ee-597">***Para adicionar um trecho de código usando o mouse (c#, Visual Basic e XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="903ee-597">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="903ee-598">Clique com botão direito em que você deseja inserir o trecho de código.</span><span class="sxs-lookup"><span data-stu-id="903ee-598">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="903ee-599">Selecione **Inserir trecho** seguido por **Meus trechos de código**.</span><span class="sxs-lookup"><span data-stu-id="903ee-599">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="903ee-600">Selecione o trecho relevante na lista, clicando nele.</span><span class="sxs-lookup"><span data-stu-id="903ee-600">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="903ee-601">![Com o botão direito em que você deseja inserir o trecho de código e selecione Inserir trecho](aspnet-mvc-4-helpers-forms-and-validation/_static/image31.png "com o botão direito em que você deseja inserir o trecho de código e selecione Inserir trecho")</span><span class="sxs-lookup"><span data-stu-id="903ee-601">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-helpers-forms-and-validation/_static/image31.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="903ee-602">*Clique com botão direito em que você deseja inserir o trecho de código e selecione Inserir trecho*</span><span class="sxs-lookup"><span data-stu-id="903ee-602">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="903ee-603">![Selecione o trecho relevante na lista, clicando nele](aspnet-mvc-4-helpers-forms-and-validation/_static/image32.png "escolher o trecho relevante na lista, clicando nele")</span><span class="sxs-lookup"><span data-stu-id="903ee-603">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-helpers-forms-and-validation/_static/image32.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="903ee-604">*Selecione o trecho relevante na lista, clicando nele*</span><span class="sxs-lookup"><span data-stu-id="903ee-604">*Pick the relevant snippet from the list, by clicking on it*</span></span>

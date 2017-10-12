---
title: Criar uma API Web com o ASP.NET Core e o Visual Studio para Windows
author: rick-anderson
description: Compilar uma API Web com o ASP.NET Core MVC e o Visual Studio para Windows
keywords: "ASP.NET Core, WebAPI, API Web, REST, HTTP, Serviço, Serviço HTTP"
ms.author: riande
manager: wpickett
ms.date: 08/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-web-api
ms.openlocfilehash: 617b11cd7652e393c06446c62138802e4a4e90df
ms.sourcegitcommit: 67f54fabbfa4e3942f5bfe1f8a7fdfe4a7a75358
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/19/2017
---
#<a name="create-a-web-api-with-aspnet-core-and-visual-studio-for-windows"></a><span data-ttu-id="abdce-104">Criar uma API Web com o ASP.NET Core e o Visual Studio para Windows</span><span class="sxs-lookup"><span data-stu-id="abdce-104">Create a web API with ASP.NET Core and Visual Studio for Windows</span></span>

<span data-ttu-id="abdce-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT) e [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="abdce-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="abdce-106">Neste tutorial, você compilará uma API Web para gerenciar uma lista de itens de “tarefas pendentes”.</span><span class="sxs-lookup"><span data-stu-id="abdce-106">In this tutorial, you’ll build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="abdce-107">Você não compilará uma interface do usuário.</span><span class="sxs-lookup"><span data-stu-id="abdce-107">You won’t build a UI.</span></span>

<span data-ttu-id="abdce-108">Há três versões deste tutorial:</span><span class="sxs-lookup"><span data-stu-id="abdce-108">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="abdce-109">Windows: API Web com o Visual Studio para Windows (este tutorial)</span><span class="sxs-lookup"><span data-stu-id="abdce-109">Windows: Web API with Visual Studio for Windows (This tutorial)</span></span>
* <span data-ttu-id="abdce-110">macOS: [API Web com o Visual Studio para Mac](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="abdce-110">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="abdce-111">macOS, Linux, Windows: [API Web com o Visual Studio Code](xref:tutorials/web-api-vsc)</span><span class="sxs-lookup"><span data-stu-id="abdce-111">macOS, Linux, Windows: [Web API with Visual Studio Code](xref:tutorials/web-api-vsc)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[intro to web API](../includes/webApi/intro.md)]

## <a name="prerequisites"></a><span data-ttu-id="abdce-112">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="abdce-112">Prerequisites</span></span>

[!INCLUDE[install 2.0](../includes/install2.0.md)]

<span data-ttu-id="abdce-113">Consulte [este PDF](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-web-api/_static/_webAPI.pdf) para o ASP.NET Core versão 1.1.</span><span class="sxs-lookup"><span data-stu-id="abdce-113">See [this PDF](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-web-api/_static/_webAPI.pdf) for the ASP.NET Core 1.1 version.</span></span>

## <a name="create-the-project"></a><span data-ttu-id="abdce-114">Criar o projeto</span><span class="sxs-lookup"><span data-stu-id="abdce-114">Create the project</span></span>

<span data-ttu-id="abdce-115">No Visual Studio, selecione o menu **Arquivo**, > **Novo** > **Projeto**.</span><span class="sxs-lookup"><span data-stu-id="abdce-115">From Visual Studio, select **File** menu, > **New** > **Project**.</span></span>

<span data-ttu-id="abdce-116">Selecione o modelo de projeto **Aplicativo Web ASP.NET Core (.NET Core)**.</span><span class="sxs-lookup"><span data-stu-id="abdce-116">Select the **ASP.NET Core Web Application (.NET Core)** project template.</span></span> <span data-ttu-id="abdce-117">Nomeie o projeto `TodoApi` e selecione **OK**.</span><span class="sxs-lookup"><span data-stu-id="abdce-117">Name the project `TodoApi` and select **OK**.</span></span>

![Caixa de diálogo Novo projeto](first-web-api/_static/new-project.png)

<span data-ttu-id="abdce-119">Na caixa de diálogo **Novo aplicativo Web ASP.NET Core – TodoApi**, selecione o modelo **API Web**.</span><span class="sxs-lookup"><span data-stu-id="abdce-119">In the **New ASP.NET Core Web Application - TodoApi** dialog, select the **Web API** template.</span></span> <span data-ttu-id="abdce-120">Selecione **OK**.</span><span class="sxs-lookup"><span data-stu-id="abdce-120">Select **OK**.</span></span> <span data-ttu-id="abdce-121">**Não** selecione **Habilitar Suporte ao Docker**.</span><span class="sxs-lookup"><span data-stu-id="abdce-121">Do **not** select **Enable Docker Support**.</span></span>

![Caixa de diálogo Novo Aplicativo Web ASP.NET com modelo de projeto de API Web selecionado de Modelos do ASP.NET Core](first-web-api/_static/web-api-project.png)

### <a name="launch-the-app"></a><span data-ttu-id="abdce-123">Iniciar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="abdce-123">Launch the app</span></span>

<span data-ttu-id="abdce-124">No Visual Studio, pressione CTRL + F5 para iniciar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="abdce-124">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="abdce-125">O Visual Studio inicia um navegador e navega para `http://localhost:port/api/values`, em que *porta* é um número da porta escolhido aleatoriamente.</span><span class="sxs-lookup"><span data-stu-id="abdce-125">Visual Studio launches a browser and navigates to `http://localhost:port/api/values`, where *port* is a randomly-chosen port number.</span></span> <span data-ttu-id="abdce-126">Chrome, Microsoft Edge e Firefox exibem o seguinte:</span><span class="sxs-lookup"><span data-stu-id="abdce-126">Chrome, Edge, and Firefox display the following:</span></span>

```
["value1","value2"]
``` 

### <a name="add-a-model-class"></a><span data-ttu-id="abdce-127">Adicionar uma classe de modelo</span><span class="sxs-lookup"><span data-stu-id="abdce-127">Add a model class</span></span>

<span data-ttu-id="abdce-128">Um modelo é um objeto que representa os dados em seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="abdce-128">A model is an object that represents the data in your application.</span></span> <span data-ttu-id="abdce-129">Nesse caso, o único modelo é um item de tarefa pendente.</span><span class="sxs-lookup"><span data-stu-id="abdce-129">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="abdce-130">Adicione uma pasta denominada "Modelos".</span><span class="sxs-lookup"><span data-stu-id="abdce-130">Add a folder named "Models".</span></span> <span data-ttu-id="abdce-131">No Gerenciador de Soluções, clique com o botão direito do mouse no projeto.</span><span class="sxs-lookup"><span data-stu-id="abdce-131">In Solution Explorer, right-click the project.</span></span> <span data-ttu-id="abdce-132">Selecione **Adicionar** > **Nova Pasta**.</span><span class="sxs-lookup"><span data-stu-id="abdce-132">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="abdce-133">Nomeie a pasta como *Modelos*.</span><span class="sxs-lookup"><span data-stu-id="abdce-133">Name the folder *Models*.</span></span>

<span data-ttu-id="abdce-134">Observação: as classes de modelo podem ser colocadas em qualquer lugar no seu projeto, mas a pasta *Modelos* é usada por convenção.</span><span class="sxs-lookup"><span data-stu-id="abdce-134">Note: The model classes go anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="abdce-135">Adicione uma classe `TodoItem`.</span><span class="sxs-lookup"><span data-stu-id="abdce-135">Add a `TodoItem` class.</span></span> <span data-ttu-id="abdce-136">Clique com o botão direito do mouse na pasta *Modelos* e selecione **Adicionar** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="abdce-136">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="abdce-137">Nomeie a classe `TodoItem` e, em seguida, selecione **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="abdce-137">Name the class `TodoItem` and select **Add**.</span></span>

<span data-ttu-id="abdce-138">Substitua o código gerado pelo mostrado a seguir:</span><span class="sxs-lookup"><span data-stu-id="abdce-138">Replace the generated code with the following:</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="abdce-139">O banco de dados gera o `Id` quando um `TodoItem` é criado.</span><span class="sxs-lookup"><span data-stu-id="abdce-139">The database generates the `Id` when a `TodoItem` is created.</span></span>

### <a name="create-the-database-context"></a><span data-ttu-id="abdce-140">Criar o contexto de banco de dados</span><span class="sxs-lookup"><span data-stu-id="abdce-140">Create the database context</span></span>

<span data-ttu-id="abdce-141">O *contexto de banco de dados* é a classe principal que coordena a funcionalidade do Entity Framework para um determinado modelo de dados.</span><span class="sxs-lookup"><span data-stu-id="abdce-141">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="abdce-142">Essa classe é criada derivando-a da classe `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="abdce-142">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="abdce-143">Adicione uma classe `TodoContext`.</span><span class="sxs-lookup"><span data-stu-id="abdce-143">Add a `TodoContext` class.</span></span> <span data-ttu-id="abdce-144">Clique com o botão direito do mouse na pasta *Modelos* e selecione **Adicionar** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="abdce-144">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="abdce-145">Nomeie a classe `TodoContext` e, em seguida, selecione **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="abdce-145">Name the class `TodoContext` and select **Add**.</span></span>

<span data-ttu-id="abdce-146">Substitua o código gerado pelo mostrado a seguir:</span><span class="sxs-lookup"><span data-stu-id="abdce-146">Replace the generated code with the following:</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

### <a name="add-a-controller"></a><span data-ttu-id="abdce-147">Adicionar um controlador</span><span class="sxs-lookup"><span data-stu-id="abdce-147">Add a controller</span></span>

<span data-ttu-id="abdce-148">No Gerenciador de Soluções, clique com o botão direito do mouse na pasta *Controladores*.</span><span class="sxs-lookup"><span data-stu-id="abdce-148">In Solution Explorer, right-click the *Controllers* folder.</span></span> <span data-ttu-id="abdce-149">Selecione **Adicionar** > **Novo Item**.</span><span class="sxs-lookup"><span data-stu-id="abdce-149">Select **Add** > **New Item**.</span></span> <span data-ttu-id="abdce-150">Na caixa de diálogo **Adicionar Novo Item**, selecione o modelo **Classe do Controlador de API Web**.</span><span class="sxs-lookup"><span data-stu-id="abdce-150">In the **Add New Item** dialog, select the **Web  API Controller Class** template.</span></span> <span data-ttu-id="abdce-151">Nomeie a classe `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="abdce-151">Name the class `TodoController`.</span></span>

![Caixa de diálogo Adicionar Novo Item com o controlador na caixa de pesquisa e o controlador da API Web selecionados](first-web-api/_static/new_controller.png)

<span data-ttu-id="abdce-153">Substitua o código gerado pelo mostrado a seguir:</span><span class="sxs-lookup"><span data-stu-id="abdce-153">Replace the generated code with the following:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]
  
### <a name="launch-the-app"></a><span data-ttu-id="abdce-154">Iniciar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="abdce-154">Launch the app</span></span>

<span data-ttu-id="abdce-155">No Visual Studio, pressione CTRL + F5 para iniciar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="abdce-155">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="abdce-156">O Visual Studio inicia um navegador e navega para `http://localhost:port/api/values`, em que *porta* é um número da porta escolhido aleatoriamente.</span><span class="sxs-lookup"><span data-stu-id="abdce-156">Visual Studio launches a browser and navigates to `http://localhost:port/api/values`, where *port* is a randomly chosen port number.</span></span> <span data-ttu-id="abdce-157">Se você estiver usando o Chrome, o Microsoft Edge ou o Firefox, os dados serão exibidos.</span><span class="sxs-lookup"><span data-stu-id="abdce-157">If you're using Chrome, Edge or Firefox, the data will be displayed.</span></span> <span data-ttu-id="abdce-158">Se você estiver usando o IE, ele solicitará que você abra ou salve o arquivo *values.json*.</span><span class="sxs-lookup"><span data-stu-id="abdce-158">If you're using IE, IE will prompt to you open or save the *values.json* file.</span></span> <span data-ttu-id="abdce-159">Navegue até o controlador `Todo` que acabamos de criar em `http://localhost:port/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="abdce-159">Navigate to the `Todo` controller we just created `http://localhost:port/api/todo`.</span></span>

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]


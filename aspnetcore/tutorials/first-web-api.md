---
title: Criar uma API Web com o ASP.NET Core e o Visual Studio para Windows
author: rick-anderson
description: Compilar uma API Web com o ASP.NET Core MVC e o Visual Studio para Windows
ms.author: riande
ms.custom: mvc
ms.date: 05/17/2018
uid: tutorials/first-web-api
ms.openlocfilehash: 3da22cbbbe0db7771656997a13587521e182fb2a
ms.sourcegitcommit: 79b756ea03eae77a716f500ef88253ee9b1464d2
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/25/2018
ms.locfileid: "36277394"
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-for-windows"></a><span data-ttu-id="d3828-103">Criar uma API Web com o ASP.NET Core e o Visual Studio para Windows</span><span class="sxs-lookup"><span data-stu-id="d3828-103">Create a Web API with ASP.NET Core and Visual Studio for Windows</span></span>

<span data-ttu-id="d3828-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT) e [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="d3828-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="d3828-105">Este tutorial compilará uma API Web para gerenciar uma lista de itens de “tarefas pendentes”.</span><span class="sxs-lookup"><span data-stu-id="d3828-105">This tutorial builds a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="d3828-106">Uma interface do usuário (UI) não será criada.</span><span class="sxs-lookup"><span data-stu-id="d3828-106">A user interface (UI) isn't created.</span></span>

<span data-ttu-id="d3828-107">Há três versões deste tutorial:</span><span class="sxs-lookup"><span data-stu-id="d3828-107">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="d3828-108">Windows: API Web com o Visual Studio para Windows (este tutorial)</span><span class="sxs-lookup"><span data-stu-id="d3828-108">Windows: Web API with Visual Studio for Windows (This tutorial)</span></span>
* <span data-ttu-id="d3828-109">macOS: [API Web com o Visual Studio para Mac](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="d3828-109">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="d3828-110">macOS, Linux, Windows: [API Web com o Visual Studio Code](xref:tutorials/web-api-vsc)</span><span class="sxs-lookup"><span data-stu-id="d3828-110">macOS, Linux, Windows: [Web API with Visual Studio Code](xref:tutorials/web-api-vsc)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[intro to web API](../includes/webApi/intro.md)]

## <a name="prerequisites"></a><span data-ttu-id="d3828-111">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="d3828-111">Prerequisites</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-windows.md)]

## <a name="create-the-project"></a><span data-ttu-id="d3828-112">Criar o projeto</span><span class="sxs-lookup"><span data-stu-id="d3828-112">Create the project</span></span>

<span data-ttu-id="d3828-113">Siga estas etapas no Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="d3828-113">Follow these steps in Visual Studio:</span></span>

* <span data-ttu-id="d3828-114">No menu **Arquivo**, selecione **Novo** > **Projeto**.</span><span class="sxs-lookup"><span data-stu-id="d3828-114">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="d3828-115">Selecione o modelo **Aplicativo Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="d3828-115">Select the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="d3828-116">Nomeie o projeto como *TodoApi* e clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="d3828-116">Name the project *TodoApi* and click **OK**.</span></span>
* <span data-ttu-id="d3828-117">Na caixa de diálogo **Novo aplicativo Web ASP.NET Core – TodoApi** e escolha a versão do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d3828-117">In the **New ASP.NET Core Web Application - TodoApi** dialog, choose the ASP.NET Core version.</span></span> <span data-ttu-id="d3828-118">Selecione o modelo **API** e clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="d3828-118">Select the **API** template and click **OK**.</span></span> <span data-ttu-id="d3828-119">**Não** selecione **Habilitar Suporte ao Docker**.</span><span class="sxs-lookup"><span data-stu-id="d3828-119">Do **not** select **Enable Docker Support**.</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="d3828-120">Iniciar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="d3828-120">Launch the app</span></span>

<span data-ttu-id="d3828-121">No Visual Studio, pressione CTRL + F5 para iniciar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="d3828-121">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="d3828-122">O Visual Studio inicia um navegador e navega para `http://localhost:<port>/api/values`, em que `<port>` é um número de porta escolhido aleatoriamente.</span><span class="sxs-lookup"><span data-stu-id="d3828-122">Visual Studio launches a browser and navigates to `http://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="d3828-123">O Chrome, Microsoft Edge e Firefox exibem a seguinte saída:</span><span class="sxs-lookup"><span data-stu-id="d3828-123">Chrome, Microsoft Edge, and Firefox display the following output:</span></span>

```json
["value1","value2"]
```

<span data-ttu-id="d3828-124">Se usar o Internet Explorer, você deverá salvar um arquivo *values.json*.</span><span class="sxs-lookup"><span data-stu-id="d3828-124">If using Internet Explorer, you'll be prompted to save a *values.json* file.</span></span>

### <a name="add-a-model-class"></a><span data-ttu-id="d3828-125">Adicionar uma classe de modelo</span><span class="sxs-lookup"><span data-stu-id="d3828-125">Add a model class</span></span>

<span data-ttu-id="d3828-126">Um modelo é um objeto que representa os dados no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="d3828-126">A model is an object representing the data in the app.</span></span> <span data-ttu-id="d3828-127">Nesse caso, o único modelo é um item de tarefas pendentes.</span><span class="sxs-lookup"><span data-stu-id="d3828-127">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="d3828-128">No Gerenciador de Soluções, clique com o botão direito do mouse no projeto.</span><span class="sxs-lookup"><span data-stu-id="d3828-128">In Solution Explorer, right-click the project.</span></span> <span data-ttu-id="d3828-129">Selecione **Adicionar** > **Nova Pasta**.</span><span class="sxs-lookup"><span data-stu-id="d3828-129">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="d3828-130">Nomeie a pasta *Models*.</span><span class="sxs-lookup"><span data-stu-id="d3828-130">Name the folder *Models*.</span></span>

> [!NOTE]
> <span data-ttu-id="d3828-131">As classes de modelo podem ficar em qualquer lugar no projeto.</span><span class="sxs-lookup"><span data-stu-id="d3828-131">The model classes can go anywhere in the project.</span></span> <span data-ttu-id="d3828-132">A pasta *Modelos* é usada por convenção para as classes de modelo.</span><span class="sxs-lookup"><span data-stu-id="d3828-132">The *Models* folder is used by convention for model classes.</span></span>

<span data-ttu-id="d3828-133">No Gerenciador de Soluções, clique com o botão direito do mouse na pasta *Modelos* e selecione **Adicionar** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="d3828-133">In Solution Explorer, right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="d3828-134">Nomeie a classe como *TodoItem* e clique em **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="d3828-134">Name the class *TodoItem* and click **Add**.</span></span>

<span data-ttu-id="d3828-135">Atualize a classe `TodoItem` com o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="d3828-135">Update the `TodoItem` class with the following code:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="d3828-136">O banco de dados gera o `Id` quando um `TodoItem` é criado.</span><span class="sxs-lookup"><span data-stu-id="d3828-136">The database generates the `Id` when a `TodoItem` is created.</span></span>

### <a name="create-the-database-context"></a><span data-ttu-id="d3828-137">Criar o contexto de banco de dados</span><span class="sxs-lookup"><span data-stu-id="d3828-137">Create the database context</span></span>

<span data-ttu-id="d3828-138">O *contexto de banco de dados* é a classe principal que coordena a funcionalidade do Entity Framework para um determinado modelo de dados.</span><span class="sxs-lookup"><span data-stu-id="d3828-138">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="d3828-139">Essa classe é criada derivando-a da classe `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="d3828-139">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="d3828-140">No Gerenciador de Soluções, clique com o botão direito do mouse na pasta *Modelos* e selecione **Adicionar** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="d3828-140">In Solution Explorer, right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="d3828-141">Nomeie a classe como *TodoContext* e clique em **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="d3828-141">Name the class *TodoContext* and click **Add**.</span></span>

<span data-ttu-id="d3828-142">Substitua a classe pelo código a seguir:</span><span class="sxs-lookup"><span data-stu-id="d3828-142">Replace the class with the following code:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

### <a name="add-a-controller"></a><span data-ttu-id="d3828-143">Adicionar um controlador</span><span class="sxs-lookup"><span data-stu-id="d3828-143">Add a controller</span></span>

<span data-ttu-id="d3828-144">No Gerenciador de Soluções, clique com o botão direito do mouse na pasta *Controladores*.</span><span class="sxs-lookup"><span data-stu-id="d3828-144">In Solution Explorer, right-click the *Controllers* folder.</span></span> <span data-ttu-id="d3828-145">Selecione **Adicionar** > **Novo Item**.</span><span class="sxs-lookup"><span data-stu-id="d3828-145">Select **Add** > **New Item**.</span></span> <span data-ttu-id="d3828-146">Na caixa de diálogo **Adicionar Novo Item**, selecione o modelo **Classe do Controlador de API**.</span><span class="sxs-lookup"><span data-stu-id="d3828-146">In the **Add New Item** dialog, select the **API Controller Class** template.</span></span> <span data-ttu-id="d3828-147">Nomeie a classe como *TodoController* e clique em **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="d3828-147">Name the class *TodoController*, and click **Add**.</span></span>

![Caixa de diálogo Adicionar Novo Item com o controlador na caixa de pesquisa e o controlador da API Web selecionados](first-web-api/_static/new_controller.png)

<span data-ttu-id="d3828-149">Substitua a classe pelo código a seguir:</span><span class="sxs-lookup"><span data-stu-id="d3828-149">Replace the class with the following code:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="d3828-150">Iniciar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="d3828-150">Launch the app</span></span>

<span data-ttu-id="d3828-151">No Visual Studio, pressione CTRL + F5 para iniciar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="d3828-151">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="d3828-152">O Visual Studio inicia um navegador e navega para `http://localhost:<port>/api/values`, em que `<port>` é um número de porta escolhido aleatoriamente.</span><span class="sxs-lookup"><span data-stu-id="d3828-152">Visual Studio launches a browser and navigates to `http://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="d3828-153">Navegue até o controlador `Todo` no `http://localhost:<port>/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="d3828-153">Navigate to the `Todo` controller at `http://localhost:<port>/api/todo`.</span></span>

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]

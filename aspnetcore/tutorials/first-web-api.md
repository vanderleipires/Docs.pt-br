---
title: Criar uma API Web com o ASP.NET Core e o Visual Studio
author: rick-anderson
description: Criar uma API Web com o ASP.NET Core MVC e o Visual Studio para Windows
ms.author: riande
ms.custom: mvc
ms.date: 05/17/2018
uid: tutorials/first-web-api
ms.openlocfilehash: eb23d5376742e04712f714263815582fddc5d18e
ms.sourcegitcommit: e9b99854b0a8021dafabee0db5e1338067f250a9
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2018
ms.locfileid: "52450691"
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio"></a><span data-ttu-id="4bb49-103">Criar uma API Web com o ASP.NET Core e o Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4bb49-103">Create a Web API with ASP.NET Core and Visual Studio</span></span>

<span data-ttu-id="4bb49-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT) e [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="4bb49-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="4bb49-105">Este tutorial compilará uma API Web para gerenciar uma lista de itens de “tarefas pendentes”.</span><span class="sxs-lookup"><span data-stu-id="4bb49-105">This tutorial builds a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="4bb49-106">Uma interface do usuário (UI) não será criada.</span><span class="sxs-lookup"><span data-stu-id="4bb49-106">A user interface (UI) isn't created.</span></span>

<span data-ttu-id="4bb49-107">Há três versões deste tutorial:</span><span class="sxs-lookup"><span data-stu-id="4bb49-107">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="4bb49-108">Windows: API Web com o Visual Studio para Windows (este tutorial, confira a [versão em vídeo](https://www.youtube.com/watch?v=TTkhEyGBfAk))</span><span class="sxs-lookup"><span data-stu-id="4bb49-108">Windows: Web API with Visual Studio on Windows (This tutorial, see the [video version](https://www.youtube.com/watch?v=TTkhEyGBfAk))</span></span>
* <span data-ttu-id="4bb49-109">macOS: [API Web com o Visual Studio para Mac](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="4bb49-109">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="4bb49-110">macOS, Linux, Windows: [API Web com o Visual Studio Code](xref:tutorials/web-api-vsc)</span><span class="sxs-lookup"><span data-stu-id="4bb49-110">macOS, Linux, Windows: [Web API with Visual Studio Code](xref:tutorials/web-api-vsc)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

> [!NOTE]
> <span data-ttu-id="4bb49-111">Estamos testando a usabilidade de uma nova estrutura proposta para o sumário do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4bb49-111">We’re testing the usability of a proposed new structure for the ASP.NET Core table of contents.</span></span>  <span data-ttu-id="4bb49-112">Se você tiver alguns minutos para experimentar um exercício de localização de sete tópicos diferentes no sumário atual ou proposto, [clique aqui para participar do estudo](https://dpk4xbh5.optimalworkshop.com/treejack/rps16hd5).</span><span class="sxs-lookup"><span data-stu-id="4bb49-112">If you have a few minutes to try an exercise of finding 7 different topics in the current or proposed table of contents, please [click here to participate in the study](https://dpk4xbh5.optimalworkshop.com/treejack/rps16hd5).</span></span>

[!INCLUDE[intro to web API](../includes/webApi/intro.md)]

## <a name="prerequisites"></a><span data-ttu-id="4bb49-113">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="4bb49-113">Prerequisites</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-windows.md)]

## <a name="create-the-project"></a><span data-ttu-id="4bb49-114">Criar o projeto</span><span class="sxs-lookup"><span data-stu-id="4bb49-114">Create the project</span></span>

<span data-ttu-id="4bb49-115">Siga estas etapas no Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="4bb49-115">Follow these steps in Visual Studio:</span></span>

* <span data-ttu-id="4bb49-116">No menu **Arquivo**, selecione **Novo** > **Projeto**.</span><span class="sxs-lookup"><span data-stu-id="4bb49-116">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="4bb49-117">Selecione o modelo **Aplicativo Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="4bb49-117">Select the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="4bb49-118">Nomeie o projeto como *TodoApi* e clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="4bb49-118">Name the project *TodoApi* and click **OK**.</span></span>
* <span data-ttu-id="4bb49-119">Na caixa de diálogo **Novo aplicativo Web ASP.NET Core – TodoApi** e escolha a versão do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4bb49-119">In the **New ASP.NET Core Web Application - TodoApi** dialog, choose the ASP.NET Core version.</span></span> <span data-ttu-id="4bb49-120">Selecione o modelo **API** e clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="4bb49-120">Select the **API** template and click **OK**.</span></span> <span data-ttu-id="4bb49-121">**Não** selecione **Habilitar Suporte ao Docker**.</span><span class="sxs-lookup"><span data-stu-id="4bb49-121">Do **not** select **Enable Docker Support**.</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="4bb49-122">Iniciar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="4bb49-122">Launch the app</span></span>

<span data-ttu-id="4bb49-123">No Visual Studio, pressione CTRL + F5 para iniciar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="4bb49-123">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="4bb49-124">O Visual Studio inicia um navegador e navega para `http://localhost:<port>/api/values`, em que `<port>` é um número de porta escolhido aleatoriamente.</span><span class="sxs-lookup"><span data-stu-id="4bb49-124">Visual Studio launches a browser and navigates to `http://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="4bb49-125">O Chrome, Microsoft Edge e Firefox exibem a seguinte saída:</span><span class="sxs-lookup"><span data-stu-id="4bb49-125">Chrome, Microsoft Edge, and Firefox display the following output:</span></span>

```json
["value1","value2"]
```

<span data-ttu-id="4bb49-126">Se usar o Internet Explorer, você deverá salvar um arquivo *values.json*.</span><span class="sxs-lookup"><span data-stu-id="4bb49-126">If using Internet Explorer, you'll be prompted to save a *values.json* file.</span></span>

### <a name="add-a-model-class"></a><span data-ttu-id="4bb49-127">Adicionar uma classe de modelo</span><span class="sxs-lookup"><span data-stu-id="4bb49-127">Add a model class</span></span>

<span data-ttu-id="4bb49-128">Um modelo é um objeto que representa os dados no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="4bb49-128">A model is an object representing the data in the app.</span></span> <span data-ttu-id="4bb49-129">Nesse caso, o único modelo é um item de tarefas pendentes.</span><span class="sxs-lookup"><span data-stu-id="4bb49-129">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="4bb49-130">No Gerenciador de Soluções, clique com o botão direito do mouse no projeto.</span><span class="sxs-lookup"><span data-stu-id="4bb49-130">In Solution Explorer, right-click the project.</span></span> <span data-ttu-id="4bb49-131">Selecione **Adicionar** > **Nova Pasta**.</span><span class="sxs-lookup"><span data-stu-id="4bb49-131">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="4bb49-132">Nomeie a pasta *Models*.</span><span class="sxs-lookup"><span data-stu-id="4bb49-132">Name the folder *Models*.</span></span>

> [!NOTE]
> <span data-ttu-id="4bb49-133">As classes de modelo podem ficar em qualquer lugar no projeto.</span><span class="sxs-lookup"><span data-stu-id="4bb49-133">The model classes can go anywhere in the project.</span></span> <span data-ttu-id="4bb49-134">A pasta *Modelos* é usada por convenção para as classes de modelo.</span><span class="sxs-lookup"><span data-stu-id="4bb49-134">The *Models* folder is used by convention for model classes.</span></span>

<span data-ttu-id="4bb49-135">No Gerenciador de Soluções, clique com o botão direito do mouse na pasta *Modelos* e selecione **Adicionar** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="4bb49-135">In Solution Explorer, right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="4bb49-136">Nomeie a classe como *TodoItem* e clique em **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="4bb49-136">Name the class *TodoItem* and click **Add**.</span></span>

<span data-ttu-id="4bb49-137">Atualize a classe `TodoItem` com o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="4bb49-137">Update the `TodoItem` class with the following code:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="4bb49-138">O banco de dados gera o `Id` quando um `TodoItem` é criado.</span><span class="sxs-lookup"><span data-stu-id="4bb49-138">The database generates the `Id` when a `TodoItem` is created.</span></span>

### <a name="create-the-database-context"></a><span data-ttu-id="4bb49-139">Criar o contexto de banco de dados</span><span class="sxs-lookup"><span data-stu-id="4bb49-139">Create the database context</span></span>

<span data-ttu-id="4bb49-140">O *contexto de banco de dados* é a classe principal que coordena a funcionalidade do Entity Framework para um determinado modelo de dados.</span><span class="sxs-lookup"><span data-stu-id="4bb49-140">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="4bb49-141">Essa classe é criada derivando-a da classe `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="4bb49-141">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="4bb49-142">No Gerenciador de Soluções, clique com o botão direito do mouse na pasta *Modelos* e selecione **Adicionar** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="4bb49-142">In Solution Explorer, right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="4bb49-143">Nomeie a classe como *TodoContext* e clique em **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="4bb49-143">Name the class *TodoContext* and click **Add**.</span></span>

<span data-ttu-id="4bb49-144">Substitua a classe pelo código a seguir:</span><span class="sxs-lookup"><span data-stu-id="4bb49-144">Replace the class with the following code:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

### <a name="add-a-controller"></a><span data-ttu-id="4bb49-145">Adicionar um controlador</span><span class="sxs-lookup"><span data-stu-id="4bb49-145">Add a controller</span></span>

<span data-ttu-id="4bb49-146">No Gerenciador de Soluções, clique com o botão direito do mouse na pasta *Controladores*.</span><span class="sxs-lookup"><span data-stu-id="4bb49-146">In Solution Explorer, right-click the *Controllers* folder.</span></span> <span data-ttu-id="4bb49-147">Selecione **Adicionar** > **Novo Item**.</span><span class="sxs-lookup"><span data-stu-id="4bb49-147">Select **Add** > **New Item**.</span></span> <span data-ttu-id="4bb49-148">Na caixa de diálogo **Adicionar Novo Item**, selecione o modelo **Classe do Controlador de API**.</span><span class="sxs-lookup"><span data-stu-id="4bb49-148">In the **Add New Item** dialog, select the **API Controller Class** template.</span></span> <span data-ttu-id="4bb49-149">Nomeie a classe como *TodoController* e clique em **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="4bb49-149">Name the class *TodoController*, and click **Add**.</span></span>

![Caixa de diálogo Adicionar Novo Item com o controlador na caixa de pesquisa e o controlador da API Web selecionados](first-web-api/_static/new_controller.png)

<span data-ttu-id="4bb49-151">Substitua a classe pelo código a seguir:</span><span class="sxs-lookup"><span data-stu-id="4bb49-151">Replace the class with the following code:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="4bb49-152">Iniciar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="4bb49-152">Launch the app</span></span>

<span data-ttu-id="4bb49-153">No Visual Studio, pressione CTRL + F5 para iniciar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="4bb49-153">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="4bb49-154">O Visual Studio inicia um navegador e navega para `http://localhost:<port>/api/values`, em que `<port>` é um número de porta escolhido aleatoriamente.</span><span class="sxs-lookup"><span data-stu-id="4bb49-154">Visual Studio launches a browser and navigates to `http://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="4bb49-155">Navegue até o controlador `Todo` no `http://localhost:<port>/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="4bb49-155">Navigate to the `Todo` controller at `http://localhost:<port>/api/todo`.</span></span>

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]

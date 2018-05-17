---
title: Criar uma API Web com o ASP.NET Core e o Visual Studio Code
author: rick-anderson
description: Compilar uma API Web no macOS, Linux ou Windows com o ASP.NET Core MVC e o Visual Studio Code
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/08/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/web-api-vsc
ms.openlocfilehash: 9fac4d7b3f687881eafbd63ee71f99bff3b27183
ms.sourcegitcommit: c867d7427bd4a88a78b2322e156367733b532730
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/09/2018
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-code"></a><span data-ttu-id="a96da-103">Criar uma API Web com o ASP.NET Core e o Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a96da-103">Create a Web API with ASP.NET Core and Visual Studio Code</span></span>

<span data-ttu-id="a96da-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT) e [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="a96da-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="a96da-105">Neste tutorial, crie uma API Web para gerenciar uma lista de itens de "tarefas pendentes".</span><span class="sxs-lookup"><span data-stu-id="a96da-105">In this tutorial, build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="a96da-106">Uma interface do usuário não é construída.</span><span class="sxs-lookup"><span data-stu-id="a96da-106">A UI isn't constructed.</span></span>

<span data-ttu-id="a96da-107">Há três versões deste tutorial:</span><span class="sxs-lookup"><span data-stu-id="a96da-107">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="a96da-108">macOS, Linux, Windows: API Web com o Visual Studio Code (Este tutorial)</span><span class="sxs-lookup"><span data-stu-id="a96da-108">macOS, Linux, Windows: Web API with Visual Studio Code (This tutorial)</span></span>
* <span data-ttu-id="a96da-109">macOS: [API Web com o Visual Studio para Mac](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="a96da-109">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="a96da-110">Windows: [API Web com o Visual Studio para Windows](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="a96da-110">Windows: [Web API with Visual Studio for Windows](xref:tutorials/first-web-api)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

## <a name="prerequisites"></a><span data-ttu-id="a96da-111">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="a96da-111">Prerequisites</span></span>

[!INCLUDE[prerequisites](~/includes/net-core-prereqs-vscode.md)]

## <a name="create-the-project"></a><span data-ttu-id="a96da-112">Criar o projeto</span><span class="sxs-lookup"><span data-stu-id="a96da-112">Create the project</span></span>

<span data-ttu-id="a96da-113">Em um console, execute os seguintes comandos:</span><span class="sxs-lookup"><span data-stu-id="a96da-113">From a console, run the following commands:</span></span>

```console
dotnet new webapi -o TodoApi
code TodoApi
```

<span data-ttu-id="a96da-114">A pasta *TodoApi* é aberta no VS Code (Visual Studio Code).</span><span class="sxs-lookup"><span data-stu-id="a96da-114">The *TodoApi* folder opens in Visual Studio Code (VS Code).</span></span> <span data-ttu-id="a96da-115">Selecione o arquivo *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="a96da-115">Select the *Startup.cs* file.</span></span>

* <span data-ttu-id="a96da-116">Selecione **Sim** para a mensagem de **Aviso** “Os ativos necessários para compilar e depurar estão ausentes em 'TodoApi'.</span><span class="sxs-lookup"><span data-stu-id="a96da-116">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'TodoApi'.</span></span> <span data-ttu-id="a96da-117">Deseja adicioná-los?”</span><span class="sxs-lookup"><span data-stu-id="a96da-117">Add them?"</span></span>
* <span data-ttu-id="a96da-118">Selecione **Restaurar** para a mensagem **Informações** “Há dependências não resolvidas”.</span><span class="sxs-lookup"><span data-stu-id="a96da-118">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

<!-- uid: tutorials/first-mvc-app-xplat/start-mvc uses the pic below. If you change it, make sure it's consistent -->

![VS Code com o Aviso – Os ativos necessários para compilar e depurar estão ausentes em “TodoApi”.](web-api-vsc/_static/vsc_restore.png)

<span data-ttu-id="a96da-122">Pressione **Depurar** (F5) para compilar e executar o programa.</span><span class="sxs-lookup"><span data-stu-id="a96da-122">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="a96da-123">Em um navegador, navegue até http://localhost:5000/api/values.</span><span class="sxs-lookup"><span data-stu-id="a96da-123">In a browser, navigate to http://localhost:5000/api/values.</span></span> <span data-ttu-id="a96da-124">É exibida a saída a seguir:</span><span class="sxs-lookup"><span data-stu-id="a96da-124">The following output is displayed:</span></span>

```json
["value1","value2"]
```

<span data-ttu-id="a96da-125">Consulte [Ajuda do Visual Studio Code](#visual-studio-code-help) para obter dicas sobre como usar o VS Code.</span><span class="sxs-lookup"><span data-stu-id="a96da-125">See [Visual Studio Code help](#visual-studio-code-help) for tips on using VS Code.</span></span>

## <a name="add-support-for-entity-framework-core"></a><span data-ttu-id="a96da-126">Adicionar suporte ao Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="a96da-126">Add support for Entity Framework Core</span></span>

:::moniker range="<= aspnetcore-2.0"
<span data-ttu-id="a96da-127">A criação de um novo projeto no ASP.NET Core 2.0 adiciona a referência de pacote [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) ao arquivo *TodoApi.csproj*:</span><span class="sxs-lookup"><span data-stu-id="a96da-127">Creating a new project in ASP.NET Core 2.0 adds the [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) package reference to the *TodoApi.csproj* file:</span></span>

<span data-ttu-id="a96da-128">[!code-xml[](first-web-api/samples/2.0/TodoApi/TodoApi.csproj?name=snippet_Metapackage&highlight=2)]</span><span class="sxs-lookup"><span data-stu-id="a96da-128">[!code-xml[](first-web-api/samples/2.0/TodoApi/TodoApi.csproj?name=snippet_Metapackage&highlight=2)]</span></span>
:::moniker-end
:::moniker range=">= aspnetcore-2.1"
<span data-ttu-id="a96da-129">A criação de um novo projeto no ASP.NET Core 2.1 ou posteriores adiciona a referência de pacote [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.App) ao arquivo *TodoApi.csproj*:</span><span class="sxs-lookup"><span data-stu-id="a96da-129">Creating a new project in ASP.NET Core 2.1 or later adds the [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App) package reference to the *TodoApi.csproj* file:</span></span>

<span data-ttu-id="a96da-130">[!code-xml[](first-web-api/samples/2.1/TodoApi/TodoApi.csproj?name=snippet_Metapackage&highlight=2)]</span><span class="sxs-lookup"><span data-stu-id="a96da-130">[!code-xml[](first-web-api/samples/2.1/TodoApi/TodoApi.csproj?name=snippet_Metapackage&highlight=2)]</span></span>
:::moniker-end

<span data-ttu-id="a96da-131">Não é necessário instalar o provedor de banco de dados [Entity Framework Core InMemory](/ef/core/providers/in-memory/) separadamente.</span><span class="sxs-lookup"><span data-stu-id="a96da-131">There's no need to install the [Entity Framework Core InMemory](/ef/core/providers/in-memory/) database provider separately.</span></span> <span data-ttu-id="a96da-132">Este provedor de banco de dados permite que o Entity Framework Core seja usado com um banco de dados em memória.</span><span class="sxs-lookup"><span data-stu-id="a96da-132">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span>

## <a name="add-a-model-class"></a><span data-ttu-id="a96da-133">Adicionar uma classe de modelo</span><span class="sxs-lookup"><span data-stu-id="a96da-133">Add a model class</span></span>

<span data-ttu-id="a96da-134">Um modelo é um objeto que representa os dados em seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="a96da-134">A model is an object representing the data in your app.</span></span> <span data-ttu-id="a96da-135">Nesse caso, o único modelo é um item de tarefas pendentes.</span><span class="sxs-lookup"><span data-stu-id="a96da-135">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="a96da-136">Adicione uma pasta chamada *Models*.</span><span class="sxs-lookup"><span data-stu-id="a96da-136">Add a folder named *Models*.</span></span> <span data-ttu-id="a96da-137">Você pode colocar as classes de modelo em qualquer lugar no projeto, mas a pasta *Models* é usada por convenção.</span><span class="sxs-lookup"><span data-stu-id="a96da-137">You can put model classes anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="a96da-138">Adicione uma classe `TodoItem` com o seguinte código:</span><span class="sxs-lookup"><span data-stu-id="a96da-138">Add a `TodoItem` class with the following code:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="a96da-139">O banco de dados gera o `Id` quando um `TodoItem` é criado.</span><span class="sxs-lookup"><span data-stu-id="a96da-139">The database generates the `Id` when a `TodoItem` is created.</span></span>

## <a name="create-the-database-context"></a><span data-ttu-id="a96da-140">Criar o contexto de banco de dados</span><span class="sxs-lookup"><span data-stu-id="a96da-140">Create the database context</span></span>

<span data-ttu-id="a96da-141">O *contexto de banco de dados* é a classe principal que coordena a funcionalidade do Entity Framework para determinado modelo de dados.</span><span class="sxs-lookup"><span data-stu-id="a96da-141">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="a96da-142">Você cria essa classe derivando-a da classe `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="a96da-142">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="a96da-143">Adicione uma classe `TodoContext` à pasta *Models*:</span><span class="sxs-lookup"><span data-stu-id="a96da-143">Add a `TodoContext` class in the *Models* folder:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a><span data-ttu-id="a96da-144">Adicionar um controlador</span><span class="sxs-lookup"><span data-stu-id="a96da-144">Add a controller</span></span>

<span data-ttu-id="a96da-145">Na pasta *Controllers*, crie uma classe chamada `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="a96da-145">In the *Controllers* folder, create a class named `TodoController`.</span></span> <span data-ttu-id="a96da-146">Substitua seu conteúdo pelo código a seguir:</span><span class="sxs-lookup"><span data-stu-id="a96da-146">Replace its contents with the following code:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="a96da-147">Iniciar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="a96da-147">Launch the app</span></span>

<span data-ttu-id="a96da-148">No VS Code, pressione F5 para iniciar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="a96da-148">In VS Code, press F5 to launch the app.</span></span> <span data-ttu-id="a96da-149">Navegue até http://localhost:5000/api/todo (o controlador `Todo` que nós criamos).</span><span class="sxs-lookup"><span data-stu-id="a96da-149">Navigate to http://localhost:5000/api/todo (the `Todo` controller we created).</span></span>

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

## <a name="visual-studio-code-help"></a><span data-ttu-id="a96da-150">Ajuda do Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a96da-150">Visual Studio Code help</span></span>

* [<span data-ttu-id="a96da-151">Introdução</span><span class="sxs-lookup"><span data-stu-id="a96da-151">Getting started</span></span>](https://code.visualstudio.com/docs)
* [<span data-ttu-id="a96da-152">Depuração</span><span class="sxs-lookup"><span data-stu-id="a96da-152">Debugging</span></span>](https://code.visualstudio.com/docs/editor/debugging)
* [<span data-ttu-id="a96da-153">Terminal integrado</span><span class="sxs-lookup"><span data-stu-id="a96da-153">Integrated terminal</span></span>](https://code.visualstudio.com/docs/editor/integrated-terminal)
* [<span data-ttu-id="a96da-154">Atalhos de teclado</span><span class="sxs-lookup"><span data-stu-id="a96da-154">Keyboard shortcuts</span></span>](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  * [<span data-ttu-id="a96da-155">Atalhos de teclado do macOS</span><span class="sxs-lookup"><span data-stu-id="a96da-155">macOS keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  * [<span data-ttu-id="a96da-156">Atalhos de teclado do Linux</span><span class="sxs-lookup"><span data-stu-id="a96da-156">Linux keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  * [<span data-ttu-id="a96da-157">Atalhos de teclado do Windows</span><span class="sxs-lookup"><span data-stu-id="a96da-157">Windows keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

[!INCLUDE[next steps](../includes/webApi/next.md)]

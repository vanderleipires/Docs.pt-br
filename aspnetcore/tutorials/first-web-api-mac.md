---
title: Criar uma API Web com o ASP.NET Core e o Visual Studio para Mac
author: rick-anderson
description: Criar uma API Web com o ASP.NET Core MVC e o Visual Studio para Mac
keywords: "ASP.NET Core, WebAPI, API Web, REST, mac, macOS, HTTP, Serviço, Serviço HTTP"
ms.author: riande
manager: wpickett
ms.date: 5/24/2017
ms.topic: get-started-article
ms.assetid: 830b4af5-ed14-1638-7734-764a6f13a8f6
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-web-api-mac
ms.openlocfilehash: 08619d3b4ab2d6fdb04794dcbafac0b696dd8504
ms.sourcegitcommit: 3273675dad5ac3e1dc1c589938b73db3f7d6660a
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/28/2017
---
# <a name="create-a-web-api-with-aspnet-core-mvc-and-visual-studio-for-mac"></a><span data-ttu-id="f98d9-104">Criar uma API Web com o ASP.NET Core MVC e o Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="f98d9-104">Create a Web API with ASP.NET Core MVC and Visual Studio for Mac</span></span>

<span data-ttu-id="f98d9-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT) e [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="f98d9-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="f98d9-106">Neste tutorial, você criará uma API Web para gerenciar uma lista de itens "pendentes".</span><span class="sxs-lookup"><span data-stu-id="f98d9-106">In this tutorial, you’ll build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="f98d9-107">Você não criará uma interface do usuário.</span><span class="sxs-lookup"><span data-stu-id="f98d9-107">You won’t build a UI.</span></span>

<span data-ttu-id="f98d9-108">Há três versões deste tutorial:</span><span class="sxs-lookup"><span data-stu-id="f98d9-108">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="f98d9-109">macOS: API Web com o Visual Studio para Mac (este tutorial)</span><span class="sxs-lookup"><span data-stu-id="f98d9-109">macOS: Web API with Visual Studio for Mac (This tutorial)</span></span>
* <span data-ttu-id="f98d9-110">Windows: [API Web com o Visual Studio para Windows](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="f98d9-110">Windows: [Web API with Visual Studio for Windows](xref:tutorials/first-web-api)</span></span>
* <span data-ttu-id="f98d9-111">macOS, Linux, Windows: [API Web com o Visual Studio Code](xref:tutorials/web-api-vsc)</span><span class="sxs-lookup"><span data-stu-id="f98d9-111">macOS, Linux, Windows: [Web API with Visual Studio Code](xref:tutorials/web-api-vsc)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

* <span data-ttu-id="f98d9-112">Consulte [Introdução ao ASP.NET Core MVC no Mac ou Linux](xref:tutorials/first-mvc-app-xplat/index) para obter um exemplo que usa um banco de dados persistente.</span><span class="sxs-lookup"><span data-stu-id="f98d9-112">See [Introduction to ASP.NET Core MVC on Mac or Linux](xref:tutorials/first-mvc-app-xplat/index) for an example that uses a persistent database.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f98d9-113">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="f98d9-113">Prerequisites</span></span>

<span data-ttu-id="f98d9-114">Instale o seguinte:</span><span class="sxs-lookup"><span data-stu-id="f98d9-114">Install the following:</span></span>

- [<span data-ttu-id="f98d9-115">SDK do .NET Core</span><span class="sxs-lookup"><span data-stu-id="f98d9-115">.NET Core SDK</span></span>](https://www.microsoft.com/net/core#macos)  
- [<span data-ttu-id="f98d9-116">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="f98d9-116">Visual Studio for Mac</span></span>](https://www.visualstudio.com/vs/visual-studio-mac/)

## <a name="create-the-project"></a><span data-ttu-id="f98d9-117">Criar o projeto</span><span class="sxs-lookup"><span data-stu-id="f98d9-117">Create the project</span></span>

<span data-ttu-id="f98d9-118">No Visual Studio, selecione **Arquivo > Nova Solução**.</span><span class="sxs-lookup"><span data-stu-id="f98d9-118">From Visual Studio, select **File > New Solution**.</span></span>

![Nova Solução do macOS](first-web-api-mac/_static/sln.png)

<span data-ttu-id="f98d9-120">Selecione **Aplicativo .NET Core > API Web ASP.NET Core > Avançar**.</span><span class="sxs-lookup"><span data-stu-id="f98d9-120">Select **.NET Core App >  ASP.NET Core Web API > Next**.</span></span>

![Caixa de diálogo Novo Projeto do macOS](first-web-api-mac/_static/1.png)

<span data-ttu-id="f98d9-122">Digite **TodoApi** para o **Nome do Projeto** e, em seguida, selecione Criar.</span><span class="sxs-lookup"><span data-stu-id="f98d9-122">Enter **TodoApi** for the **Project Name**, and then select Create.</span></span>

![caixa de diálogo de configuração](first-web-api-mac/_static/2.png)

### <a name="launch-the-app"></a><span data-ttu-id="f98d9-124">Iniciar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="f98d9-124">Launch the app</span></span>

<span data-ttu-id="f98d9-125">No Visual Studio, selecione **Executar > Iniciar Com Depuração** para iniciar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="f98d9-125">In Visual Studio, select **Run > Start With Debugging** to launch the app.</span></span> <span data-ttu-id="f98d9-126">O Visual Studio inicia um navegador e navega para `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="f98d9-126">Visual Studio launches a browser and navigates to `http://localhost:5000`.</span></span> <span data-ttu-id="f98d9-127">Você obtém um erro de HTTP 404 (Não Encontrado).</span><span class="sxs-lookup"><span data-stu-id="f98d9-127">You get an HTTP 404 (Not Found) error.</span></span>  <span data-ttu-id="f98d9-128">Altere a URL para `http://localhost:port/api/values`.</span><span class="sxs-lookup"><span data-stu-id="f98d9-128">Change the URL to `http://localhost:port/api/values`.</span></span> <span data-ttu-id="f98d9-129">Os dados de `ValuesController` serão exibidos:</span><span class="sxs-lookup"><span data-stu-id="f98d9-129">The `ValuesController` data will be displayed:</span></span>

```
["value1","value2"]
```

### <a name="add-support-for-entity-framework-core"></a><span data-ttu-id="f98d9-130">Adicionar suporte ao Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="f98d9-130">Add support for Entity Framework Core</span></span>

<span data-ttu-id="f98d9-131">Instalar o provedor de banco de dados [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/).</span><span class="sxs-lookup"><span data-stu-id="f98d9-131">Install the [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) database provider.</span></span> <span data-ttu-id="f98d9-132">Este provedor de banco de dados permite que o Entity Framework Core seja usado com um banco de dados em memória.</span><span class="sxs-lookup"><span data-stu-id="f98d9-132">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span>

* <span data-ttu-id="f98d9-133">No menu **Projeto**, selecione **Adicionar pacotes do NuGet**.</span><span class="sxs-lookup"><span data-stu-id="f98d9-133">From the **Project** menu, select **Add NuGet Packages**.</span></span> 

  *  <span data-ttu-id="f98d9-134">Como alternativa, clique com o botão direito do mouse em **Dependências** e, em seguida, selecione **Adicionar Pacotes**.</span><span class="sxs-lookup"><span data-stu-id="f98d9-134">Alternately, you can right-click **Dependencies**, and then select **Add Packages**.</span></span>

* <span data-ttu-id="f98d9-135">Digite `EntityFrameworkCore.InMemory` na caixa de pesquisa.</span><span class="sxs-lookup"><span data-stu-id="f98d9-135">Enter `EntityFrameworkCore.InMemory` in the search box.</span></span>
* <span data-ttu-id="f98d9-136">Selecione `Microsoft.EntityFrameworkCore.InMemory` e, em seguida, selecione **Adicionar Pacote**.</span><span class="sxs-lookup"><span data-stu-id="f98d9-136">Select `Microsoft.EntityFrameworkCore.InMemory`, and then select **Add Package**.</span></span>

### <a name="add-a-model-class"></a><span data-ttu-id="f98d9-137">Adicionar uma classe de modelo</span><span class="sxs-lookup"><span data-stu-id="f98d9-137">Add a model class</span></span>

<span data-ttu-id="f98d9-138">Um modelo é um objeto que representa os dados em seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="f98d9-138">A model is an object that represents the data in your application.</span></span> <span data-ttu-id="f98d9-139">Nesse caso, o único modelo é um item de tarefa pendente.</span><span class="sxs-lookup"><span data-stu-id="f98d9-139">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="f98d9-140">Adicione uma pasta denominada *Modelos*.</span><span class="sxs-lookup"><span data-stu-id="f98d9-140">Add a folder named *Models*.</span></span> <span data-ttu-id="f98d9-141">No Gerenciador de Soluções, clique com o botão direito do mouse no projeto.</span><span class="sxs-lookup"><span data-stu-id="f98d9-141">In Solution Explorer, right-click the project.</span></span> <span data-ttu-id="f98d9-142">Selecione **Adicionar** > **Nova Pasta**.</span><span class="sxs-lookup"><span data-stu-id="f98d9-142">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="f98d9-143">Nomeie a pasta como *Modelos*.</span><span class="sxs-lookup"><span data-stu-id="f98d9-143">Name the folder *Models*.</span></span>

![nova pasta](first-web-api-mac/_static/folder.png)

<span data-ttu-id="f98d9-145">Observação: Você pode colocar as classes de modelo em qualquer lugar no seu projeto, mas a pasta *Modelos* é usada por convenção.</span><span class="sxs-lookup"><span data-stu-id="f98d9-145">Note: You can put model classes anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="f98d9-146">Adicione uma classe `TodoItem`.</span><span class="sxs-lookup"><span data-stu-id="f98d9-146">Add a `TodoItem` class.</span></span> <span data-ttu-id="f98d9-147">Clique com o botão direito do mouse na pasta *Modelos* e selecione **Adicionar > Novo Arquivo > Geral > Classe Vazia**.</span><span class="sxs-lookup"><span data-stu-id="f98d9-147">Right-click the *Models* folder and select **Add > New File > General > Empty Class**.</span></span> <span data-ttu-id="f98d9-148">Nomeie a classe `TodoItem` e, em seguida, selecione **Novo**.</span><span class="sxs-lookup"><span data-stu-id="f98d9-148">Name the class `TodoItem`, and then select **New**.</span></span>

<span data-ttu-id="f98d9-149">Substitua o código gerado por:</span><span class="sxs-lookup"><span data-stu-id="f98d9-149">Replace the generated code with:</span></span>

<span data-ttu-id="f98d9-150">[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]</span><span class="sxs-lookup"><span data-stu-id="f98d9-150">[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]</span></span>

<span data-ttu-id="f98d9-151">O banco de dados gera o `Id` quando um `TodoItem` é criado.</span><span class="sxs-lookup"><span data-stu-id="f98d9-151">The database generates the `Id` when a `TodoItem` is created.</span></span>

### <a name="create-the-database-context"></a><span data-ttu-id="f98d9-152">Criar o contexto do banco de dados</span><span class="sxs-lookup"><span data-stu-id="f98d9-152">Create the database context</span></span>

<span data-ttu-id="f98d9-153">O *contexto de banco de dados* é a classe principal que coordena a funcionalidade do Entity Framework para um determinado modelo de dados.</span><span class="sxs-lookup"><span data-stu-id="f98d9-153">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="f98d9-154">Você cria essa classe derivando-a da classe `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="f98d9-154">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="f98d9-155">Adicione uma classe denominada `TodoContext` à pasta *Modelos*.</span><span class="sxs-lookup"><span data-stu-id="f98d9-155">Add a `TodoContext` class to the *Models* folder.</span></span>

<span data-ttu-id="f98d9-156">[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]</span><span class="sxs-lookup"><span data-stu-id="f98d9-156">[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]</span></span>

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a><span data-ttu-id="f98d9-157">Adicionar um controlador</span><span class="sxs-lookup"><span data-stu-id="f98d9-157">Add a controller</span></span>

<span data-ttu-id="f98d9-158">No Gerenciador de Soluções, na pasta *Controladores*, adicione a classe `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="f98d9-158">In Solution Explorer, in the *Controllers* folder, add the class `TodoController`.</span></span>

<span data-ttu-id="f98d9-159">Substitua o código gerado pelo código a seguir (e adicione chaves de fechamento):</span><span class="sxs-lookup"><span data-stu-id="f98d9-159">Replace the generated code with the following (and add closing braces):</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="f98d9-160">Iniciar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="f98d9-160">Launch the app</span></span>

<span data-ttu-id="f98d9-161">No Visual Studio, selecione **Executar > Iniciar Com Depuração** para iniciar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="f98d9-161">In Visual Studio, select **Run > Start With Debugging** to launch the app.</span></span> <span data-ttu-id="f98d9-162">O Visual Studio inicia um navegador e navega para `http://localhost:port`, em que *porta* é um número da porta escolhido aleatoriamente.</span><span class="sxs-lookup"><span data-stu-id="f98d9-162">Visual Studio launches a browser and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span> <span data-ttu-id="f98d9-163">Você obtém um erro de HTTP 404 (Não Encontrado).</span><span class="sxs-lookup"><span data-stu-id="f98d9-163">You get an HTTP 404 (Not Found) error.</span></span>  <span data-ttu-id="f98d9-164">Altere a URL para `http://localhost:port/api/values`.</span><span class="sxs-lookup"><span data-stu-id="f98d9-164">Change the URL to `http://localhost:port/api/values`.</span></span> <span data-ttu-id="f98d9-165">Os dados de `ValuesController` serão exibidos:</span><span class="sxs-lookup"><span data-stu-id="f98d9-165">The `ValuesController` data will be displayed:</span></span>

```
["value1","value2"]
```

<span data-ttu-id="f98d9-166">Navegue até o controlador `Todo` no `http://localhost:port/api/todo`:</span><span class="sxs-lookup"><span data-stu-id="f98d9-166">Navigate to the `Todo` controller at`http://localhost:port/api/todo`:</span></span>

```
[{"key":1,"name":"Item1","isComplete":false}]
```

## <a name="implement-the-other-crud-operations"></a><span data-ttu-id="f98d9-167">Implementar as outras operações de CRUD</span><span class="sxs-lookup"><span data-stu-id="f98d9-167">Implement the other CRUD operations</span></span>

<span data-ttu-id="f98d9-168">Adicionaremos os métodos `Create`, `Update` e `Delete` para o controlador.</span><span class="sxs-lookup"><span data-stu-id="f98d9-168">We'll add `Create`, `Update`, and `Delete` methods to the controller.</span></span> <span data-ttu-id="f98d9-169">Essas são variações em um tema, então apenas mostrarei o código e realçarei as principais diferenças.</span><span class="sxs-lookup"><span data-stu-id="f98d9-169">These are variations on a theme, so I'll just show the code and highlight the main differences.</span></span> <span data-ttu-id="f98d9-170">Compile o projeto depois de adicionar ou alterar código.</span><span class="sxs-lookup"><span data-stu-id="f98d9-170">Build the project after adding or changing code.</span></span>

### <a name="create"></a><span data-ttu-id="f98d9-171">Create</span><span class="sxs-lookup"><span data-stu-id="f98d9-171">Create</span></span>

<span data-ttu-id="f98d9-172">[!code-csharp[Main](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]</span><span class="sxs-lookup"><span data-stu-id="f98d9-172">[!code-csharp[Main](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]</span></span>

<span data-ttu-id="f98d9-173">Esse é um método HTTP POST, indicado pelo atributo [`[HttpPost]`](https://docs.asp.net/projects/api/en/latest/autoapi/Microsoft/AspNetCore/Mvc/HttpPostAttribute/index.html).</span><span class="sxs-lookup"><span data-stu-id="f98d9-173">This is an HTTP POST method, indicated by the [`[HttpPost]`](https://docs.asp.net/projects/api/en/latest/autoapi/Microsoft/AspNetCore/Mvc/HttpPostAttribute/index.html) attribute.</span></span> <span data-ttu-id="f98d9-174">O atributo [`[FromBody]`](https://docs.asp.net/projects/api/en/latest/autoapi/Microsoft/AspNetCore/Mvc/FromBodyAttribute/index.html) informa ao MVC para obter o valor do item de tarefa pendente do corpo da solicitação HTTP.</span><span class="sxs-lookup"><span data-stu-id="f98d9-174">The [`[FromBody]`](https://docs.asp.net/projects/api/en/latest/autoapi/Microsoft/AspNetCore/Mvc/FromBodyAttribute/index.html) attribute tells MVC to get the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="f98d9-175">O método `CreatedAtRoute` retorna uma resposta 201, que é a resposta padrão para um método HTTP POST que cria um novo recurso no servidor.</span><span class="sxs-lookup"><span data-stu-id="f98d9-175">The `CreatedAtRoute` method returns a 201 response, which is the standard response for an HTTP POST method that creates a new resource on the server.</span></span> <span data-ttu-id="f98d9-176">O `CreatedAtRoute` também adiciona um cabeçalho Local à resposta.</span><span class="sxs-lookup"><span data-stu-id="f98d9-176">`CreatedAtRoute` also adds a Location header to the response.</span></span> <span data-ttu-id="f98d9-177">O cabeçalho Local especifica o URI do item de tarefa pendente recém-criado.</span><span class="sxs-lookup"><span data-stu-id="f98d9-177">The Location header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="f98d9-178">Veja [10.2.2 201 Criado](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="f98d9-178">See [10.2.2 201 Created](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>

### <a name="use-postman-to-send-a-create-request"></a><span data-ttu-id="f98d9-179">Use o Postman para enviar uma solicitação de criação</span><span class="sxs-lookup"><span data-stu-id="f98d9-179">Use Postman to send a Create request</span></span>

* <span data-ttu-id="f98d9-180">Inicie o aplicativo (**Executar > Iniciar Com Depuração**).</span><span class="sxs-lookup"><span data-stu-id="f98d9-180">Start the app (**Run > Start With Debugging**).</span></span>
* <span data-ttu-id="f98d9-181">Inicie o Postman.</span><span class="sxs-lookup"><span data-stu-id="f98d9-181">Start Postman.</span></span>

![Console do Postman](first-web-api/_static/pmc.png)

* <span data-ttu-id="f98d9-183">Defina o método HTTP para `POST`</span><span class="sxs-lookup"><span data-stu-id="f98d9-183">Set the HTTP method to `POST`</span></span>
* <span data-ttu-id="f98d9-184">Selecione o botão de opção **Corpo**</span><span class="sxs-lookup"><span data-stu-id="f98d9-184">Select the **Body** radio button</span></span>
* <span data-ttu-id="f98d9-185">Selecione o botão de opção **bruto**</span><span class="sxs-lookup"><span data-stu-id="f98d9-185">Select the **raw** radio button</span></span>
* <span data-ttu-id="f98d9-186">Defina o tipo como JSON</span><span class="sxs-lookup"><span data-stu-id="f98d9-186">Set the type to JSON</span></span>
* <span data-ttu-id="f98d9-187">No editor de chave-valor, insira um item de tarefa pendente, tal como</span><span class="sxs-lookup"><span data-stu-id="f98d9-187">In the key-value editor, enter a Todo item such as</span></span>

```json
{
    "name":"walk dog",
    "isComplete":true
}
```

* <span data-ttu-id="f98d9-188">Selecione **Enviar**</span><span class="sxs-lookup"><span data-stu-id="f98d9-188">Select **Send**</span></span>

* <span data-ttu-id="f98d9-189">Selecione a guia Cabeçalhos no painel inferior e copie o cabeçalho **Local**:</span><span class="sxs-lookup"><span data-stu-id="f98d9-189">Select the Headers tab in the lower pane and copy the **Location** header:</span></span>

![Guia Cabeçalhos do console do Postman](first-web-api/_static/pmget.png)

<span data-ttu-id="f98d9-191">Você pode usar o URI do cabeçalho de local para acessar o recurso que você acabou de criar.</span><span class="sxs-lookup"><span data-stu-id="f98d9-191">You can use the Location header URI to access the resource you just created.</span></span> <span data-ttu-id="f98d9-192">Lembre-se de que o método `GetById` criou a rota nomeada `"GetTodo"`:</span><span class="sxs-lookup"><span data-stu-id="f98d9-192">Recall the `GetById` method created the `"GetTodo"` named route:</span></span>

```csharp
[HttpGet("{id}", Name = "GetTodo")]
public IActionResult GetById(string id)
```

### <a name="update"></a><span data-ttu-id="f98d9-193">Atualização</span><span class="sxs-lookup"><span data-stu-id="f98d9-193">Update</span></span>

<span data-ttu-id="f98d9-194">[!code-csharp[Main](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]</span><span class="sxs-lookup"><span data-stu-id="f98d9-194">[!code-csharp[Main](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]</span></span>

<span data-ttu-id="f98d9-195">`Update` é semelhante a `Create`, mas usa HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="f98d9-195">`Update` is similar to `Create`, but uses HTTP PUT.</span></span> <span data-ttu-id="f98d9-196">A resposta é [204 (sem conteúdo)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="f98d9-196">The response is [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="f98d9-197">Acordo com a especificação HTTP, uma solicitação PUT requer que o cliente envie a entidade atualizada inteira, não apenas os deltas.</span><span class="sxs-lookup"><span data-stu-id="f98d9-197">According to the HTTP spec, a PUT request requires the client to send the entire updated entity, not just the deltas.</span></span> <span data-ttu-id="f98d9-198">Para dar suporte a atualizações parciais, use HTTP PATCH.</span><span class="sxs-lookup"><span data-stu-id="f98d9-198">To support partial updates, use HTTP PATCH.</span></span>

```json
{
  "key": 1,
  "name": "walk dog",
  "isComplete": true
}
```

![Console do Postman mostrando a resposta 204 (sem conteúdo)](first-web-api/_static/pmcput.png)

### <a name="delete"></a><span data-ttu-id="f98d9-200">Excluir</span><span class="sxs-lookup"><span data-stu-id="f98d9-200">Delete</span></span>

<span data-ttu-id="f98d9-201">[!code-csharp[Main](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]</span><span class="sxs-lookup"><span data-stu-id="f98d9-201">[!code-csharp[Main](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]</span></span>

<span data-ttu-id="f98d9-202">A resposta é [204 (sem conteúdo)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="f98d9-202">The response is [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

![Console do Postman mostrando a resposta 204 (sem conteúdo)](first-web-api/_static/pmd.png)

## <a name="next-steps"></a><span data-ttu-id="f98d9-204">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="f98d9-204">Next steps</span></span>

* [<span data-ttu-id="f98d9-205">Roteamento para Ações do Controlador</span><span class="sxs-lookup"><span data-stu-id="f98d9-205">Routing to Controller Actions</span></span>](xref:mvc/controllers/routing)
* <span data-ttu-id="f98d9-206">Para obter informações sobre como implantar sua API, consulte [Publicação e implantação](../publishing/index.md).</span><span class="sxs-lookup"><span data-stu-id="f98d9-206">For information about deploying your API, see [Publishing and Deployment](../publishing/index.md).</span></span>
* [<span data-ttu-id="f98d9-207">Exibir ou baixar o código de exemplo</span><span class="sxs-lookup"><span data-stu-id="f98d9-207">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample)
* [<span data-ttu-id="f98d9-208">Postman</span><span class="sxs-lookup"><span data-stu-id="f98d9-208">Postman</span></span>](https://www.getpostman.com/)
* [<span data-ttu-id="f98d9-209">Fiddler</span><span class="sxs-lookup"><span data-stu-id="f98d9-209">Fiddler</span></span>](http://www.fiddler2.com/fiddler2/)

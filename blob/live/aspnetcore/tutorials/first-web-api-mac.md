---
title: Criar uma API Web com o ASP.NET Core e o Visual Studio para Mac
description: Criar uma API Web com o ASP.NET Core MVC e o Visual Studio para Mac
author: rick-anderson
ms.author: riande
ms.date: 09/15/2017
ms.topic: get-started-article
ms.prod: asp.net-core
uid: tutorials/first-web-api-mac
helpviewer_heywords: ASP.NET Core, WebAPI, Web API, REST, mac, macOS, HTTP, Service, HTTP Service
ms.technology: aspnet
manager: wpickett
ms.openlocfilehash: 4f2643a91e1523008b68df670a9734e3d4dea5a8
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/19/2018
---
# <a name="create-a-web-api-with-aspnet-core-mvc-and-visual-studio-for-mac"></a><span data-ttu-id="ee122-103">Criar uma API Web com o ASP.NET Core MVC e o Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="ee122-103">Create a Web API with ASP.NET Core MVC and Visual Studio for Mac</span></span>

<span data-ttu-id="ee122-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT) e [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="ee122-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="ee122-105">Neste tutorial, você compilará uma API Web para gerenciar uma lista de itens de “tarefas pendentes”.</span><span class="sxs-lookup"><span data-stu-id="ee122-105">In this tutorial, you’ll build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="ee122-106">Você não compilará uma interface do usuário.</span><span class="sxs-lookup"><span data-stu-id="ee122-106">You won’t build a UI.</span></span>

<span data-ttu-id="ee122-107">Há três versões deste tutorial:</span><span class="sxs-lookup"><span data-stu-id="ee122-107">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="ee122-108">macOS: API Web com o Visual Studio para Mac (este tutorial)</span><span class="sxs-lookup"><span data-stu-id="ee122-108">macOS: Web API with Visual Studio for Mac (This tutorial)</span></span>
* <span data-ttu-id="ee122-109">Windows: [API Web com o Visual Studio para Windows](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="ee122-109">Windows: [Web API with Visual Studio for Windows](xref:tutorials/first-web-api)</span></span>
* <span data-ttu-id="ee122-110">macOS, Linux, Windows: [API Web com o Visual Studio Code](xref:tutorials/web-api-vsc)</span><span class="sxs-lookup"><span data-stu-id="ee122-110">macOS, Linux, Windows: [Web API with Visual Studio Code](xref:tutorials/web-api-vsc)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

* <span data-ttu-id="ee122-111">Consulte [Introdução ao ASP.NET Core MVC no Mac ou Linux](xref:tutorials/first-mvc-app-xplat/index) para obter um exemplo que usa um banco de dados persistente.</span><span class="sxs-lookup"><span data-stu-id="ee122-111">See [Introduction to ASP.NET Core MVC on Mac or Linux](xref:tutorials/first-mvc-app-xplat/index) for an example that uses a persistent database.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ee122-112">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="ee122-112">Prerequisites</span></span>

<span data-ttu-id="ee122-113">Instale o seguinte:</span><span class="sxs-lookup"><span data-stu-id="ee122-113">Install the following:</span></span>

- <span data-ttu-id="ee122-114">[SDK do .NET Core 2.0.0](https://www.microsoft.com/net/core) ou posterior</span><span class="sxs-lookup"><span data-stu-id="ee122-114">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later</span></span>
- [<span data-ttu-id="ee122-115">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="ee122-115">Visual Studio for Mac</span></span>](https://www.visualstudio.com/vs/visual-studio-mac/)

## <a name="create-the-project"></a><span data-ttu-id="ee122-116">Criar o projeto</span><span class="sxs-lookup"><span data-stu-id="ee122-116">Create the project</span></span>

<span data-ttu-id="ee122-117">No Visual Studio, selecione **Arquivo > Nova Solução**.</span><span class="sxs-lookup"><span data-stu-id="ee122-117">From Visual Studio, select **File > New Solution**.</span></span>

![Nova Solução do macOS](first-web-api-mac/_static/sln.png)

<span data-ttu-id="ee122-119">Selecione **Aplicativo .NET Core > API Web ASP.NET Core > Avançar**.</span><span class="sxs-lookup"><span data-stu-id="ee122-119">Select **.NET Core App >  ASP.NET Core Web API > Next**.</span></span>

![Caixa de diálogo Novo Projeto do macOS](first-web-api-mac/_static/1.png)

<span data-ttu-id="ee122-121">Digite **TodoApi** para o **Nome do Projeto** e, em seguida, selecione Criar.</span><span class="sxs-lookup"><span data-stu-id="ee122-121">Enter **TodoApi** for the **Project Name**, and then select Create.</span></span>

![caixa de diálogo de configuração](first-web-api-mac/_static/2.png)

### <a name="launch-the-app"></a><span data-ttu-id="ee122-123">Iniciar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="ee122-123">Launch the app</span></span>

<span data-ttu-id="ee122-124">No Visual Studio, selecione **Executar > Iniciar Com Depuração** para iniciar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="ee122-124">In Visual Studio, select **Run > Start With Debugging** to launch the app.</span></span> <span data-ttu-id="ee122-125">O Visual Studio inicia um navegador e navega para `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="ee122-125">Visual Studio launches a browser and navigates to `http://localhost:5000`.</span></span> <span data-ttu-id="ee122-126">Você obtém um erro de HTTP 404 (Não Encontrado).</span><span class="sxs-lookup"><span data-stu-id="ee122-126">You get an HTTP 404 (Not Found) error.</span></span>  <span data-ttu-id="ee122-127">Altere a URL para `http://localhost:port/api/values`.</span><span class="sxs-lookup"><span data-stu-id="ee122-127">Change the URL to `http://localhost:port/api/values`.</span></span> <span data-ttu-id="ee122-128">Os dados de `ValuesController` serão exibidos:</span><span class="sxs-lookup"><span data-stu-id="ee122-128">The `ValuesController` data will be displayed:</span></span>

```
["value1","value2"]
```

### <a name="add-support-for-entity-framework-core"></a><span data-ttu-id="ee122-129">Adicionar suporte ao Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="ee122-129">Add support for Entity Framework Core</span></span>

<span data-ttu-id="ee122-130">Instalar o provedor de banco de dados [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/).</span><span class="sxs-lookup"><span data-stu-id="ee122-130">Install the [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) database provider.</span></span> <span data-ttu-id="ee122-131">Este provedor de banco de dados permite que o Entity Framework Core seja usado com um banco de dados em memória.</span><span class="sxs-lookup"><span data-stu-id="ee122-131">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span>

* <span data-ttu-id="ee122-132">No menu **Projeto**, selecione **Adicionar pacotes do NuGet**.</span><span class="sxs-lookup"><span data-stu-id="ee122-132">From the **Project** menu, select **Add NuGet Packages**.</span></span> 

  *  <span data-ttu-id="ee122-133">Como alternativa, clique com o botão direito do mouse em **Dependências** e, em seguida, selecione **Adicionar Pacotes**.</span><span class="sxs-lookup"><span data-stu-id="ee122-133">Alternately, you can right-click **Dependencies**, and then select **Add Packages**.</span></span>

* <span data-ttu-id="ee122-134">Digite `EntityFrameworkCore.InMemory` na caixa de pesquisa.</span><span class="sxs-lookup"><span data-stu-id="ee122-134">Enter `EntityFrameworkCore.InMemory` in the search box.</span></span>
* <span data-ttu-id="ee122-135">Selecione `Microsoft.EntityFrameworkCore.InMemory` e, em seguida, selecione **Adicionar Pacote**.</span><span class="sxs-lookup"><span data-stu-id="ee122-135">Select `Microsoft.EntityFrameworkCore.InMemory`, and then select **Add Package**.</span></span>

### <a name="add-a-model-class"></a><span data-ttu-id="ee122-136">Adicionar uma classe de modelo</span><span class="sxs-lookup"><span data-stu-id="ee122-136">Add a model class</span></span>

<span data-ttu-id="ee122-137">Um modelo é um objeto que representa os dados em seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="ee122-137">A model is an object that represents the data in your application.</span></span> <span data-ttu-id="ee122-138">Nesse caso, o único modelo é um item de tarefas pendentes.</span><span class="sxs-lookup"><span data-stu-id="ee122-138">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="ee122-139">Adicione uma pasta chamada *Models*.</span><span class="sxs-lookup"><span data-stu-id="ee122-139">Add a folder named *Models*.</span></span> <span data-ttu-id="ee122-140">No Gerenciador de Soluções, clique com o botão direito do mouse no projeto.</span><span class="sxs-lookup"><span data-stu-id="ee122-140">In Solution Explorer, right-click the project.</span></span> <span data-ttu-id="ee122-141">Selecione **Adicionar** > **Nova Pasta**.</span><span class="sxs-lookup"><span data-stu-id="ee122-141">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="ee122-142">Nomeie a pasta como *Modelos*.</span><span class="sxs-lookup"><span data-stu-id="ee122-142">Name the folder *Models*.</span></span>

![nova pasta](first-web-api-mac/_static/folder.png)

<span data-ttu-id="ee122-144">Observação: Você pode colocar as classes de modelo em qualquer lugar no seu projeto, mas a pasta *Modelos* é usada por convenção.</span><span class="sxs-lookup"><span data-stu-id="ee122-144">Note: You can put model classes anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="ee122-145">Adicione uma classe `TodoItem`.</span><span class="sxs-lookup"><span data-stu-id="ee122-145">Add a `TodoItem` class.</span></span> <span data-ttu-id="ee122-146">Clique com o botão direito do mouse na pasta *Modelos* e selecione **Adicionar > Novo Arquivo > Geral > Classe Vazia**.</span><span class="sxs-lookup"><span data-stu-id="ee122-146">Right-click the *Models* folder and select **Add > New File > General > Empty Class**.</span></span> <span data-ttu-id="ee122-147">Nomeie a classe `TodoItem` e, em seguida, selecione **Novo**.</span><span class="sxs-lookup"><span data-stu-id="ee122-147">Name the class `TodoItem`, and then select **New**.</span></span>

<span data-ttu-id="ee122-148">Substitua o código gerado por:</span><span class="sxs-lookup"><span data-stu-id="ee122-148">Replace the generated code with:</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="ee122-149">O banco de dados gera o `Id` quando um `TodoItem` é criado.</span><span class="sxs-lookup"><span data-stu-id="ee122-149">The database generates the `Id` when a `TodoItem` is created.</span></span>

### <a name="create-the-database-context"></a><span data-ttu-id="ee122-150">Criar o contexto de banco de dados</span><span class="sxs-lookup"><span data-stu-id="ee122-150">Create the database context</span></span>

<span data-ttu-id="ee122-151">O *contexto de banco de dados* é a classe principal que coordena a funcionalidade do Entity Framework para determinado modelo de dados.</span><span class="sxs-lookup"><span data-stu-id="ee122-151">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="ee122-152">Você cria essa classe derivando-a da classe `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="ee122-152">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="ee122-153">Adicione uma classe denominada `TodoContext` à pasta *Modelos*.</span><span class="sxs-lookup"><span data-stu-id="ee122-153">Add a `TodoContext` class to the *Models* folder.</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a><span data-ttu-id="ee122-154">Adicionar um controlador</span><span class="sxs-lookup"><span data-stu-id="ee122-154">Add a controller</span></span>

<span data-ttu-id="ee122-155">No Gerenciador de Soluções, na pasta *Controladores*, adicione a classe `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="ee122-155">In Solution Explorer, in the *Controllers* folder, add the class `TodoController`.</span></span>

<span data-ttu-id="ee122-156">Substitua o código gerado pelo código a seguir (e adicione chaves de fechamento):</span><span class="sxs-lookup"><span data-stu-id="ee122-156">Replace the generated code with the following (and add closing braces):</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="ee122-157">Iniciar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="ee122-157">Launch the app</span></span>

<span data-ttu-id="ee122-158">No Visual Studio, selecione **Executar > Iniciar Com Depuração** para iniciar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="ee122-158">In Visual Studio, select **Run > Start With Debugging** to launch the app.</span></span> <span data-ttu-id="ee122-159">O Visual Studio inicia um navegador e navega para `http://localhost:port`, em que *porta* é um número da porta escolhido aleatoriamente.</span><span class="sxs-lookup"><span data-stu-id="ee122-159">Visual Studio launches a browser and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span> <span data-ttu-id="ee122-160">Você obtém um erro de HTTP 404 (Não Encontrado).</span><span class="sxs-lookup"><span data-stu-id="ee122-160">You get an HTTP 404 (Not Found) error.</span></span>  <span data-ttu-id="ee122-161">Altere a URL para `http://localhost:port/api/values`.</span><span class="sxs-lookup"><span data-stu-id="ee122-161">Change the URL to `http://localhost:port/api/values`.</span></span> <span data-ttu-id="ee122-162">Os dados de `ValuesController` serão exibidos:</span><span class="sxs-lookup"><span data-stu-id="ee122-162">The `ValuesController` data will be displayed:</span></span>

```
["value1","value2"]
```

<span data-ttu-id="ee122-163">Navegue até o controlador `Todo` no `http://localhost:port/api/todo`:</span><span class="sxs-lookup"><span data-stu-id="ee122-163">Navigate to the `Todo` controller at`http://localhost:port/api/todo`:</span></span>

```
[{"key":1,"name":"Item1","isComplete":false}]
```

## <a name="implement-the-other-crud-operations"></a><span data-ttu-id="ee122-164">Implementar as outras operações de CRUD</span><span class="sxs-lookup"><span data-stu-id="ee122-164">Implement the other CRUD operations</span></span>

<span data-ttu-id="ee122-165">Adicionaremos os métodos `Create`, `Update` e `Delete` ao controlador.</span><span class="sxs-lookup"><span data-stu-id="ee122-165">We'll add `Create`, `Update`, and `Delete` methods to the controller.</span></span> <span data-ttu-id="ee122-166">Essas são variações de um mesmo tema e, portanto, mostrarei apenas o código e realçarei as principais diferenças.</span><span class="sxs-lookup"><span data-stu-id="ee122-166">These are variations on a theme, so I'll just show the code and highlight the main differences.</span></span> <span data-ttu-id="ee122-167">Compile o projeto depois de adicionar ou alterar o código.</span><span class="sxs-lookup"><span data-stu-id="ee122-167">Build the project after adding or changing code.</span></span>

### <a name="create"></a><span data-ttu-id="ee122-168">Create</span><span class="sxs-lookup"><span data-stu-id="ee122-168">Create</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="ee122-169">Este é um método HTTP POST, indicado pelo atributo [`[HttpPost]`](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute).</span><span class="sxs-lookup"><span data-stu-id="ee122-169">This is an HTTP POST method, indicated by the [`[HttpPost]`](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="ee122-170">O atributo [`[FromBody]`](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) informa ao MVC para obter o valor do item de tarefas pendentes do corpo da solicitação HTTP.</span><span class="sxs-lookup"><span data-stu-id="ee122-170">The [`[FromBody]`](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) attribute tells MVC to get the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="ee122-171">O método `CreatedAtRoute` retorna uma resposta 201, que é a resposta padrão para um método HTTP POST que cria um novo recurso no servidor.</span><span class="sxs-lookup"><span data-stu-id="ee122-171">The `CreatedAtRoute` method returns a 201 response, which is the standard response for an HTTP POST method that creates a new resource on the server.</span></span> <span data-ttu-id="ee122-172">`CreatedAtRoute` também adiciona um cabeçalho Local à resposta.</span><span class="sxs-lookup"><span data-stu-id="ee122-172">`CreatedAtRoute` also adds a Location header to the response.</span></span> <span data-ttu-id="ee122-173">O cabeçalho Location especifica o URI do item de tarefas pendentes recém-criado.</span><span class="sxs-lookup"><span data-stu-id="ee122-173">The Location header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="ee122-174">Consulte [10.2.2 201 criado](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="ee122-174">See [10.2.2 201 Created](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>

### <a name="use-postman-to-send-a-create-request"></a><span data-ttu-id="ee122-175">Use o Postman para enviar uma solicitação de criação</span><span class="sxs-lookup"><span data-stu-id="ee122-175">Use Postman to send a Create request</span></span>

* <span data-ttu-id="ee122-176">Inicie o aplicativo (**Executar > Iniciar Com Depuração**).</span><span class="sxs-lookup"><span data-stu-id="ee122-176">Start the app (**Run > Start With Debugging**).</span></span>
* <span data-ttu-id="ee122-177">Inicie o Postman.</span><span class="sxs-lookup"><span data-stu-id="ee122-177">Start Postman.</span></span>

![Console do Postman](first-web-api/_static/pmc.png)

* <span data-ttu-id="ee122-179">Defina o método HTTP como `POST`</span><span class="sxs-lookup"><span data-stu-id="ee122-179">Set the HTTP method to `POST`</span></span>
* <span data-ttu-id="ee122-180">Selecione o botão de opção **Corpo**</span><span class="sxs-lookup"><span data-stu-id="ee122-180">Select the **Body** radio button</span></span>
* <span data-ttu-id="ee122-181">Selecione o botão de opção **bruto**</span><span class="sxs-lookup"><span data-stu-id="ee122-181">Select the **raw** radio button</span></span>
* <span data-ttu-id="ee122-182">Definir o tipo como JSON</span><span class="sxs-lookup"><span data-stu-id="ee122-182">Set the type to JSON</span></span>
* <span data-ttu-id="ee122-183">No editor de chave-valor, insira um item de tarefas pendentes, como</span><span class="sxs-lookup"><span data-stu-id="ee122-183">In the key-value editor, enter a Todo item such as</span></span>

```json
{
    "name":"walk dog",
    "isComplete":true
}
```

* <span data-ttu-id="ee122-184">Selecione **Enviar**</span><span class="sxs-lookup"><span data-stu-id="ee122-184">Select **Send**</span></span>

* <span data-ttu-id="ee122-185">Selecione a guia Cabeçalhos no painel inferior e copie o cabeçalho **Location**:</span><span class="sxs-lookup"><span data-stu-id="ee122-185">Select the Headers tab in the lower pane and copy the **Location** header:</span></span>

![Guia Cabeçalhos do console do Postman](first-web-api/_static/pmget.png)

<span data-ttu-id="ee122-187">Use o URI do cabeçalho Location para acessar o recurso que você acabou de criar.</span><span class="sxs-lookup"><span data-stu-id="ee122-187">You can use the Location header URI to access the resource you just created.</span></span> <span data-ttu-id="ee122-188">Lembre-se de que o método `GetById` criou a rota nomeada `"GetTodo"`:</span><span class="sxs-lookup"><span data-stu-id="ee122-188">Recall the `GetById` method created the `"GetTodo"` named route:</span></span>

```csharp
[HttpGet("{id}", Name = "GetTodo")]
public IActionResult GetById(string id)
```

### <a name="update"></a><span data-ttu-id="ee122-189">Atualização</span><span class="sxs-lookup"><span data-stu-id="ee122-189">Update</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

<span data-ttu-id="ee122-190">`Update` é semelhante a `Create`, mas usa HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="ee122-190">`Update` is similar to `Create`, but uses HTTP PUT.</span></span> <span data-ttu-id="ee122-191">A resposta é [204 (Sem conteúdo)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="ee122-191">The response is [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="ee122-192">De acordo com a especificação HTTP, uma solicitação PUT exige que o cliente envie a entidade atualizada inteira, não apenas os deltas.</span><span class="sxs-lookup"><span data-stu-id="ee122-192">According to the HTTP spec, a PUT request requires the client to send the entire updated entity, not just the deltas.</span></span> <span data-ttu-id="ee122-193">Para dar suporte a atualizações parciais, use HTTP PATCH.</span><span class="sxs-lookup"><span data-stu-id="ee122-193">To support partial updates, use HTTP PATCH.</span></span>

```json
{
  "key": 1,
  "name": "walk dog",
  "isComplete": true
}
```

![Console do Postman mostrando a resposta 204 (Sem conteúdo)](first-web-api/_static/pmcput.png)

### <a name="delete"></a><span data-ttu-id="ee122-195">Excluir</span><span class="sxs-lookup"><span data-stu-id="ee122-195">Delete</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="ee122-196">A resposta é [204 (Sem conteúdo)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="ee122-196">The response is [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

![Console do Postman mostrando a resposta 204 (Sem conteúdo)](first-web-api/_static/pmd.png)

## <a name="next-steps"></a><span data-ttu-id="ee122-198">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="ee122-198">Next steps</span></span>

* [<span data-ttu-id="ee122-199">Roteamento para ações do controlador</span><span class="sxs-lookup"><span data-stu-id="ee122-199">Routing to Controller Actions</span></span>](xref:mvc/controllers/routing)
* <span data-ttu-id="ee122-200">Para obter informações sobre como implantar a API, consulte [Host e implantação](xref:host-and-deploy/index).</span><span class="sxs-lookup"><span data-stu-id="ee122-200">For information about deploying your API, see [Host and deploy](xref:host-and-deploy/index).</span></span>
* <span data-ttu-id="ee122-201">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ee122-201">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>
* [<span data-ttu-id="ee122-202">Postman</span><span class="sxs-lookup"><span data-stu-id="ee122-202">Postman</span></span>](https://www.getpostman.com/)
* [<span data-ttu-id="ee122-203">Fiddler</span><span class="sxs-lookup"><span data-stu-id="ee122-203">Fiddler</span></span>](https://www.telerik.com/download/fiddler)

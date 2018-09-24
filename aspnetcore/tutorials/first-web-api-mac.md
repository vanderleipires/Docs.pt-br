---
title: Criar uma API Web com o ASP.NET Core e o Visual Studio para Mac
author: rick-anderson
description: Criar uma API Web com o ASP.NET Core MVC e o Visual Studio para Mac
ms.author: riande
ms.custom: mvc
ms.date: 05/08/2018
uid: tutorials/first-web-api-mac
ms.openlocfilehash: 40f9bd9c57b97826edfddeb00cb4fb38a026d46e
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46011619"
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-for-mac"></a><span data-ttu-id="71ac4-103">Criar uma API Web com o ASP.NET Core e o Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="71ac4-103">Create a Web API with ASP.NET Core and Visual Studio for Mac</span></span>

<span data-ttu-id="71ac4-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT) e [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="71ac4-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="71ac4-105">Neste tutorial, crie uma API Web para gerenciar uma lista de itens de "tarefas pendentes".</span><span class="sxs-lookup"><span data-stu-id="71ac4-105">In this tutorial, build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="71ac4-106">A interface do usuário não é construída.</span><span class="sxs-lookup"><span data-stu-id="71ac4-106">The UI isn't constructed.</span></span>

<span data-ttu-id="71ac4-107">Há três versões deste tutorial:</span><span class="sxs-lookup"><span data-stu-id="71ac4-107">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="71ac4-108">macOS: API Web com o Visual Studio para Mac (este tutorial)</span><span class="sxs-lookup"><span data-stu-id="71ac4-108">macOS: Web API with Visual Studio for Mac (This tutorial)</span></span>
* <span data-ttu-id="71ac4-109">Windows: [API Web com o Visual Studio para Windows](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="71ac4-109">Windows: [Web API with Visual Studio for Windows](xref:tutorials/first-web-api)</span></span>
* <span data-ttu-id="71ac4-110">macOS, Linux, Windows: [API Web com o Visual Studio Code](xref:tutorials/web-api-vsc)</span><span class="sxs-lookup"><span data-stu-id="71ac4-110">macOS, Linux, Windows: [Web API with Visual Studio Code](xref:tutorials/web-api-vsc)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

<span data-ttu-id="71ac4-111">Confira [Introdução ao ASP.NET Core MVC no Mac ou Linux](xref:tutorials/first-mvc-app-xplat/index) para obter um exemplo que usa um banco de dados persistente.</span><span class="sxs-lookup"><span data-stu-id="71ac4-111">See [Introduction to ASP.NET Core MVC on macOS or Linux](xref:tutorials/first-mvc-app-xplat/index) for an example that uses a persistent database.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="71ac4-112">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="71ac4-112">Prerequisites</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-macos.md)]

## <a name="create-the-project"></a><span data-ttu-id="71ac4-113">Criar o projeto</span><span class="sxs-lookup"><span data-stu-id="71ac4-113">Create the project</span></span>

<span data-ttu-id="71ac4-114">No Visual Studio, selecione **Arquivo** > **Nova Solução**.</span><span class="sxs-lookup"><span data-stu-id="71ac4-114">From Visual Studio, select **File** > **New Solution**.</span></span>

![Nova solução do macOS](first-web-api-mac/_static/sln.png)

<span data-ttu-id="71ac4-116">Selecione **Aplicativo .NET Core** > **API Web ASP.NET Core** > **Avançar**.</span><span class="sxs-lookup"><span data-stu-id="71ac4-116">Select **.NET Core App** > **ASP.NET Core Web API** > **Next**.</span></span>

![Caixa de diálogo Novo projeto do macOS](first-web-api-mac/_static/1.png)

<span data-ttu-id="71ac4-118">Digite *TodoApi* para o **Nome do Projeto** e, em seguida, clique em **Criar**.</span><span class="sxs-lookup"><span data-stu-id="71ac4-118">Enter *TodoApi* for the **Project Name**, and then click **Create**.</span></span>

![caixa de diálogo de configuração](first-web-api-mac/_static/2.png)

### <a name="launch-the-app"></a><span data-ttu-id="71ac4-120">Iniciar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="71ac4-120">Launch the app</span></span>

<span data-ttu-id="71ac4-121">No Visual Studio, selecione **Executar** > **Iniciar com Depuração** para iniciar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="71ac4-121">In Visual Studio, select **Run** > **Start With Debugging** to launch the app.</span></span> <span data-ttu-id="71ac4-122">O Visual Studio inicia um navegador e navega para `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="71ac4-122">Visual Studio launches a browser and navigates to `http://localhost:5000`.</span></span> <span data-ttu-id="71ac4-123">Você obtém um erro de HTTP 404 (Não Encontrado).</span><span class="sxs-lookup"><span data-stu-id="71ac4-123">You get an HTTP 404 (Not Found) error.</span></span> <span data-ttu-id="71ac4-124">Altere a URL para `http://localhost:<port>/api/values`.</span><span class="sxs-lookup"><span data-stu-id="71ac4-124">Change the URL to `http://localhost:<port>/api/values`.</span></span> <span data-ttu-id="71ac4-125">Os dados do `ValuesController` são exibidos:</span><span class="sxs-lookup"><span data-stu-id="71ac4-125">The `ValuesController` data is displayed:</span></span>

```json
["value1","value2"]
```

### <a name="add-support-for-entity-framework-core"></a><span data-ttu-id="71ac4-126">Adicionar suporte ao Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="71ac4-126">Add support for Entity Framework Core</span></span>

<span data-ttu-id="71ac4-127">Instalar o provedor de banco de dados [Entity Framework Core InMemory](/ef/core/providers/in-memory/).</span><span class="sxs-lookup"><span data-stu-id="71ac4-127">Install the [Entity Framework Core InMemory](/ef/core/providers/in-memory/) database provider.</span></span> <span data-ttu-id="71ac4-128">Este provedor de banco de dados permite que o Entity Framework Core seja usado com um banco de dados em memória.</span><span class="sxs-lookup"><span data-stu-id="71ac4-128">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span>

* <span data-ttu-id="71ac4-129">No menu **Projeto**, selecione **Adicionar pacotes do NuGet**.</span><span class="sxs-lookup"><span data-stu-id="71ac4-129">From the **Project** menu, select **Add NuGet Packages**.</span></span>

  * <span data-ttu-id="71ac4-130">Como alternativa, clique com o botão direito do mouse em **Dependências** e, em seguida, selecione **Adicionar Pacotes**.</span><span class="sxs-lookup"><span data-stu-id="71ac4-130">Alternatively, you can right-click **Dependencies**, and then select **Add Packages**.</span></span>

* <span data-ttu-id="71ac4-131">Digite `EntityFrameworkCore.InMemory` na caixa de pesquisa.</span><span class="sxs-lookup"><span data-stu-id="71ac4-131">Enter `EntityFrameworkCore.InMemory` in the search box.</span></span>
* <span data-ttu-id="71ac4-132">Selecione `Microsoft.EntityFrameworkCore.InMemory` e, em seguida, selecione **Adicionar Pacote**.</span><span class="sxs-lookup"><span data-stu-id="71ac4-132">Select `Microsoft.EntityFrameworkCore.InMemory`, and then select **Add Package**.</span></span>

### <a name="add-a-model-class"></a><span data-ttu-id="71ac4-133">Adicionar uma classe de modelo</span><span class="sxs-lookup"><span data-stu-id="71ac4-133">Add a model class</span></span>

<span data-ttu-id="71ac4-134">Um modelo é um objeto que representa os dados em seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="71ac4-134">A model is an object representing the data in your app.</span></span> <span data-ttu-id="71ac4-135">Nesse caso, o único modelo é um item de tarefas pendentes.</span><span class="sxs-lookup"><span data-stu-id="71ac4-135">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="71ac4-136">No Gerenciador de Soluções, clique com o botão direito do mouse no projeto.</span><span class="sxs-lookup"><span data-stu-id="71ac4-136">In Solution Explorer, right-click the project.</span></span> <span data-ttu-id="71ac4-137">Selecione **Adicionar** > **Nova Pasta**.</span><span class="sxs-lookup"><span data-stu-id="71ac4-137">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="71ac4-138">Nomeie a pasta como *Modelos*.</span><span class="sxs-lookup"><span data-stu-id="71ac4-138">Name the folder *Models*.</span></span>

![nova pasta](first-web-api-mac/_static/folder.png)

> [!NOTE]
> <span data-ttu-id="71ac4-140">Você pode colocar as classes de modelo em qualquer lugar no projeto, mas a pasta *Models* é usada por convenção.</span><span class="sxs-lookup"><span data-stu-id="71ac4-140">You can put model classes anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="71ac4-141">Clique com o botão direito do mouse na pasta *Modelos* e selecione **Adicionar** > **Novo Arquivo** > **Geral** > **Classe Vazia**.</span><span class="sxs-lookup"><span data-stu-id="71ac4-141">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span> <span data-ttu-id="71ac4-142">Nomeie a classe como *TodoItem* e, em seguida, clique em **Novo**.</span><span class="sxs-lookup"><span data-stu-id="71ac4-142">Name the class *TodoItem*, and then click **New**.</span></span>

<span data-ttu-id="71ac4-143">Substitua o código gerado por:</span><span class="sxs-lookup"><span data-stu-id="71ac4-143">Replace the generated code with:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="71ac4-144">O banco de dados gera o `Id` quando um `TodoItem` é criado.</span><span class="sxs-lookup"><span data-stu-id="71ac4-144">The database generates the `Id` when a `TodoItem` is created.</span></span>

### <a name="create-the-database-context"></a><span data-ttu-id="71ac4-145">Criar o contexto de banco de dados</span><span class="sxs-lookup"><span data-stu-id="71ac4-145">Create the database context</span></span>

<span data-ttu-id="71ac4-146">O *contexto de banco de dados* é a classe principal que coordena a funcionalidade do Entity Framework para determinado modelo de dados.</span><span class="sxs-lookup"><span data-stu-id="71ac4-146">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="71ac4-147">Você cria essa classe derivando-a da classe `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="71ac4-147">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="71ac4-148">Adicione uma classe denominada `TodoContext` à pasta *Modelos*.</span><span class="sxs-lookup"><span data-stu-id="71ac4-148">Add a `TodoContext` class to the *Models* folder.</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a><span data-ttu-id="71ac4-149">Adicionar um controlador</span><span class="sxs-lookup"><span data-stu-id="71ac4-149">Add a controller</span></span>

<span data-ttu-id="71ac4-150">No Gerenciador de Soluções, na pasta *Controladores*, adicione a classe `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="71ac4-150">In Solution Explorer, in the *Controllers* folder, add the class `TodoController`.</span></span>

<span data-ttu-id="71ac4-151">Substitua o código gerado pelo seguinte:</span><span class="sxs-lookup"><span data-stu-id="71ac4-151">Replace the generated code with the following:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="71ac4-152">Iniciar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="71ac4-152">Launch the app</span></span>

<span data-ttu-id="71ac4-153">No Visual Studio, selecione **Executar** > **Iniciar com Depuração** para iniciar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="71ac4-153">In Visual Studio, select **Run** > **Start With Debugging** to launch the app.</span></span> <span data-ttu-id="71ac4-154">O Visual Studio inicia um navegador e navega para `http://localhost:<port>`, em que `<port>` é um número de porta escolhido aleatoriamente.</span><span class="sxs-lookup"><span data-stu-id="71ac4-154">Visual Studio launches a browser and navigates to `http://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="71ac4-155">Você obtém um erro de HTTP 404 (Não Encontrado).</span><span class="sxs-lookup"><span data-stu-id="71ac4-155">You get an HTTP 404 (Not Found) error.</span></span> <span data-ttu-id="71ac4-156">Altere a URL para `http://localhost:<port>/api/values`.</span><span class="sxs-lookup"><span data-stu-id="71ac4-156">Change the URL to `http://localhost:<port>/api/values`.</span></span> <span data-ttu-id="71ac4-157">Os dados do `ValuesController` são exibidos:</span><span class="sxs-lookup"><span data-stu-id="71ac4-157">The `ValuesController` data is displayed:</span></span>

```json
["value1","value2"]
```

<span data-ttu-id="71ac4-158">Navegue até o controlador `Todo` no `http://localhost:<port>/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="71ac4-158">Navigate to the `Todo` controller at `http://localhost:<port>/api/todo`.</span></span> <span data-ttu-id="71ac4-159">O seguinte JSON é retornado:</span><span class="sxs-lookup"><span data-stu-id="71ac4-159">The following JSON is returned:</span></span>

```json
[{"key":1,"name":"Item1","isComplete":false}]
```

## <a name="implement-the-other-crud-operations"></a><span data-ttu-id="71ac4-160">Implementar as outras operações CRUD</span><span class="sxs-lookup"><span data-stu-id="71ac4-160">Implement the other CRUD operations</span></span>

<span data-ttu-id="71ac4-161">Adicionaremos os métodos `Create`, `Update` e `Delete` ao controlador.</span><span class="sxs-lookup"><span data-stu-id="71ac4-161">We'll add `Create`, `Update`, and `Delete` methods to the controller.</span></span> <span data-ttu-id="71ac4-162">Esses métodos são variações de um mesmo tema e, portanto, mostrarei apenas o código e realçarei as principais diferenças.</span><span class="sxs-lookup"><span data-stu-id="71ac4-162">These methods are variations on a theme, so I'll just show the code and highlight the main differences.</span></span> <span data-ttu-id="71ac4-163">Compile o projeto depois de adicionar ou alterar o código.</span><span class="sxs-lookup"><span data-stu-id="71ac4-163">Build the project after adding or changing code.</span></span>

### <a name="create"></a><span data-ttu-id="71ac4-164">Create</span><span class="sxs-lookup"><span data-stu-id="71ac4-164">Create</span></span>

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="71ac4-165">O método anterior responde a um HTTP POST, conforme indicado pelo atributo [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute).</span><span class="sxs-lookup"><span data-stu-id="71ac4-165">The preceding method responds to an HTTP POST, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="71ac4-166">O atributo [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) informa ao MVC que ele deve obter o valor do item pendente no corpo da solicitação HTTP.</span><span class="sxs-lookup"><span data-stu-id="71ac4-166">The [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) attribute tells MVC to get the value of the to-do item from the body of the HTTP request.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="71ac4-167">O método anterior responde a um HTTP POST, conforme indicado pelo atributo [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute).</span><span class="sxs-lookup"><span data-stu-id="71ac4-167">The preceding method responds to an HTTP POST, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="71ac4-168">O MVC obtém o valor do item pendente no corpo da solicitação HTTP.</span><span class="sxs-lookup"><span data-stu-id="71ac4-168">MVC gets the value of the to-do item from the body of the HTTP request.</span></span>

::: moniker-end

<span data-ttu-id="71ac4-169">O método `CreatedAtRoute` retorna uma resposta 201.</span><span class="sxs-lookup"><span data-stu-id="71ac4-169">The `CreatedAtRoute` method returns a 201 response.</span></span> <span data-ttu-id="71ac4-170">Essa é a resposta padrão para um método HTTP POST que cria um novo recurso no servidor.</span><span class="sxs-lookup"><span data-stu-id="71ac4-170">It's the standard response for an HTTP POST method that creates a new resource on the server.</span></span> <span data-ttu-id="71ac4-171">`CreatedAtRoute` também adiciona um cabeçalho Local à resposta.</span><span class="sxs-lookup"><span data-stu-id="71ac4-171">`CreatedAtRoute` also adds a Location header to the response.</span></span> <span data-ttu-id="71ac4-172">O cabeçalho Location especifica o URI do item de tarefas pendentes recém-criado.</span><span class="sxs-lookup"><span data-stu-id="71ac4-172">The Location header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="71ac4-173">Consulte [10.2.2 201 criado](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="71ac4-173">See [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>

### <a name="use-postman-to-send-a-create-request"></a><span data-ttu-id="71ac4-174">Usar o Postman para enviar uma solicitação Create</span><span class="sxs-lookup"><span data-stu-id="71ac4-174">Use Postman to send a Create request</span></span>

* <span data-ttu-id="71ac4-175">Inicie o aplicativo (**Executar** > **Iniciar com Depuração**).</span><span class="sxs-lookup"><span data-stu-id="71ac4-175">Start the app (**Run** > **Start With Debugging**).</span></span>
* <span data-ttu-id="71ac4-176">Abra o Postman.</span><span class="sxs-lookup"><span data-stu-id="71ac4-176">Open Postman.</span></span>

![Console do Postman](first-web-api/_static/pmc.png)

* <span data-ttu-id="71ac4-178">Atualize o número da porta na URL do localhost.</span><span class="sxs-lookup"><span data-stu-id="71ac4-178">Update the port number in the localhost URL.</span></span>
* <span data-ttu-id="71ac4-179">Defina o método HTTP para *POST*.</span><span class="sxs-lookup"><span data-stu-id="71ac4-179">Set the HTTP method to *POST*.</span></span>
* <span data-ttu-id="71ac4-180">Clique na guia **Corpo**.</span><span class="sxs-lookup"><span data-stu-id="71ac4-180">Click the **Body** tab.</span></span>
* <span data-ttu-id="71ac4-181">Selecione o botão de opção **bruto**.</span><span class="sxs-lookup"><span data-stu-id="71ac4-181">Select the **raw** radio button.</span></span>
* <span data-ttu-id="71ac4-182">Defina o tipo como *JSON (aplicativo/json)*.</span><span class="sxs-lookup"><span data-stu-id="71ac4-182">Set the type to *JSON (application/json)*.</span></span>
* <span data-ttu-id="71ac4-183">Insira um corpo de solicitação com um item pendente que se assemelhe ao JSON a seguir:</span><span class="sxs-lookup"><span data-stu-id="71ac4-183">Enter a request body with a to-do item resembling the following JSON:</span></span>

```json
{
  "name":"walk dog",
  "isComplete":true
}
```

* <span data-ttu-id="71ac4-184">Clique no botão **Enviar**.</span><span class="sxs-lookup"><span data-stu-id="71ac4-184">Click the **Send** button.</span></span>

::: moniker range=">= aspnetcore-2.1"

> [!TIP]
> <span data-ttu-id="71ac4-185">Se nenhuma resposta for exibida depois de clicar em **Enviar**, desabilite a opção **Verificação de certificação SSL**.</span><span class="sxs-lookup"><span data-stu-id="71ac4-185">If no response displays after clicking **Send**, disable the **SSL certification verification** option.</span></span> <span data-ttu-id="71ac4-186">Isso é encontrado em **Arquivo** > **Configurações**.</span><span class="sxs-lookup"><span data-stu-id="71ac4-186">This is found under **File** > **Settings**.</span></span> <span data-ttu-id="71ac4-187">Clique no botão **Enviar** novamente depois de desabilitar a configuração.</span><span class="sxs-lookup"><span data-stu-id="71ac4-187">Click the **Send** button again after disabling the setting.</span></span>

::: moniker-end

<span data-ttu-id="71ac4-188">Clique na guia **Cabeçalhos** no painel **Resposta** e copie o valor do cabeçalho **Local**:</span><span class="sxs-lookup"><span data-stu-id="71ac4-188">Click the **Headers** tab in the **Response** pane and copy the **Location** header value:</span></span>

![Guia Cabeçalhos do console do Postman](first-web-api/_static/pmc2.png)

<span data-ttu-id="71ac4-190">É possível usar o URI do cabeçalho Local para acessar o recurso que você criou.</span><span class="sxs-lookup"><span data-stu-id="71ac4-190">You can use the Location header URI to access the resource you created.</span></span> <span data-ttu-id="71ac4-191">O método `Create` retorna [CreatedAtRoute](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdatroute#Microsoft_AspNetCore_Mvc_ControllerBase_CreatedAtRoute_System_String_System_Object_System_Object_).</span><span class="sxs-lookup"><span data-stu-id="71ac4-191">The `Create` method returns [CreatedAtRoute](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdatroute#Microsoft_AspNetCore_Mvc_ControllerBase_CreatedAtRoute_System_String_System_Object_System_Object_).</span></span> <span data-ttu-id="71ac4-192">O primeiro parâmetro passado para `CreatedAtRoute` representa a rota nomeada a ser usada para gerar a URL.</span><span class="sxs-lookup"><span data-stu-id="71ac4-192">The first parameter passed to `CreatedAtRoute` represents the named route to use for generating the URL.</span></span> <span data-ttu-id="71ac4-193">Lembre-se de que o método `GetById` criou a rota nomeada `"GetTodo"`:</span><span class="sxs-lookup"><span data-stu-id="71ac4-193">Recall that the `GetById` method created the `"GetTodo"` named route:</span></span>

```csharp
[HttpGet("{id}", Name = "GetTodo")]
```

### <a name="update"></a><span data-ttu-id="71ac4-194">Atualização</span><span class="sxs-lookup"><span data-stu-id="71ac4-194">Update</span></span>

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

::: moniker-end

<span data-ttu-id="71ac4-195">`Update` é semelhante a `Create`, mas usa HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="71ac4-195">`Update` is similar to `Create`, but uses HTTP PUT.</span></span> <span data-ttu-id="71ac4-196">A resposta é [204 (Sem conteúdo)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="71ac4-196">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="71ac4-197">De acordo com a especificação HTTP, uma solicitação PUT exige que o cliente envie a entidade atualizada inteira, não apenas os deltas.</span><span class="sxs-lookup"><span data-stu-id="71ac4-197">According to the HTTP spec, a PUT request requires the client to send the entire updated entity, not just the deltas.</span></span> <span data-ttu-id="71ac4-198">Para dar suporte a atualizações parciais, use HTTP PATCH.</span><span class="sxs-lookup"><span data-stu-id="71ac4-198">To support partial updates, use HTTP PATCH.</span></span>

```json
{
  "key": 1,
  "name": "walk dog",
  "isComplete": true
}
```

![Console do Postman mostrando a resposta 204 (Sem conteúdo)](first-web-api/_static/pmcput.png)

### <a name="delete"></a><span data-ttu-id="71ac4-200">Excluir</span><span class="sxs-lookup"><span data-stu-id="71ac4-200">Delete</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="71ac4-201">A resposta é [204 (Sem conteúdo)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="71ac4-201">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

![Console do Postman mostrando a resposta 204 (Sem conteúdo)](first-web-api/_static/pmd.png)

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]

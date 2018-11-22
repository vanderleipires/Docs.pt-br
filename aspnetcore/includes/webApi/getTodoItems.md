::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

<span data-ttu-id="e6537-101">O código anterior define uma classe de controlador de API sem métodos.</span><span class="sxs-lookup"><span data-stu-id="e6537-101">The preceding code defines an API controller class without methods.</span></span> <span data-ttu-id="e6537-102">Nas próximas seções, os métodos serão adicionados para implementar a API.</span><span class="sxs-lookup"><span data-stu-id="e6537-102">In the next sections, methods are added to implement the API.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

<span data-ttu-id="e6537-103">O código anterior:</span><span class="sxs-lookup"><span data-stu-id="e6537-103">The preceding code:</span></span>

* <span data-ttu-id="e6537-104">Define uma classe de controlador de API sem métodos.</span><span class="sxs-lookup"><span data-stu-id="e6537-104">Defines an API controller class without methods.</span></span>
* <span data-ttu-id="e6537-105">Cria um novo item de Tarefas pendentes quando `TodoItems` está vazio.</span><span class="sxs-lookup"><span data-stu-id="e6537-105">Creates a new Todo item when `TodoItems` is empty.</span></span> <span data-ttu-id="e6537-106">Você não poderá excluir todos os itens de Tarefas pendentes porque o construtor cria um novo se `TodoItems` estiver vazio.</span><span class="sxs-lookup"><span data-stu-id="e6537-106">You won't be able to delete all the Todo items because the constructor creates a new one if `TodoItems` is empty.</span></span>

<span data-ttu-id="e6537-107">Nas próximas seções, os métodos serão adicionados para implementar a API.</span><span class="sxs-lookup"><span data-stu-id="e6537-107">In the next sections, methods are added to implement the API.</span></span> <span data-ttu-id="e6537-108">A classe é anotada com um atributo `[ApiController]` para habilitar alguns recursos convenientes.</span><span class="sxs-lookup"><span data-stu-id="e6537-108">The class is annotated with an `[ApiController]` attribute to enable some convenient features.</span></span> <span data-ttu-id="e6537-109">Para obter informações sobre os recursos habilitados pelo atributo, confira [Anotação com o atributo ApiController](xref:web-api/index#annotation-with-apicontroller-attribute).</span><span class="sxs-lookup"><span data-stu-id="e6537-109">For information on features enabled by the attribute, see [Annotation with ApiController attribute](xref:web-api/index#annotation-with-apicontroller-attribute).</span></span>

::: moniker-end

<span data-ttu-id="e6537-110">O construtor do controlador usa a [Injeção de Dependência](xref:fundamentals/dependency-injection) para injetar o contexto de banco de dados (`TodoContext`) no controlador.</span><span class="sxs-lookup"><span data-stu-id="e6537-110">The controller's constructor uses [Dependency Injection](xref:fundamentals/dependency-injection) to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="e6537-111">O contexto de banco de dados é usado em cada um dos métodos [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) no controlador.</span><span class="sxs-lookup"><span data-stu-id="e6537-111">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span> <span data-ttu-id="e6537-112">O construtor adiciona um item no banco de dados em memória, caso ele não exista.</span><span class="sxs-lookup"><span data-stu-id="e6537-112">The constructor adds an item to the in-memory database if one doesn't exist.</span></span>

## <a name="get-to-do-items"></a><span data-ttu-id="e6537-113">Obter itens pendentes</span><span class="sxs-lookup"><span data-stu-id="e6537-113">Get to-do items</span></span>

<span data-ttu-id="e6537-114">Para obter os itens pendentes, adicione os seguintes métodos à classe `TodoController`:</span><span class="sxs-lookup"><span data-stu-id="e6537-114">To get to-do items, add the following methods to the `TodoController` class:</span></span>

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

::: moniker-end

<span data-ttu-id="e6537-115">Esses métodos de implementam os dois métodos GET:</span><span class="sxs-lookup"><span data-stu-id="e6537-115">These methods implement the two GET methods:</span></span>

* `GET /api/todo`
* `GET /api/todo/{id}`

<span data-ttu-id="e6537-116">Aqui está um exemplo de resposta HTTP para o método `GetAll`:</span><span class="sxs-lookup"><span data-stu-id="e6537-116">Here's a sample HTTP response for the `GetAll` method:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

<span data-ttu-id="e6537-117">Mais adiante no tutorial, mostrarei como a resposta HTTP pode ser visualizada com o [Postman](https://www.getpostman.com/) ou o [curl](https://curl.haxx.se/docs/manpage.html).</span><span class="sxs-lookup"><span data-stu-id="e6537-117">Later in the tutorial, I'll show how the HTTP response can be viewed with [Postman](https://www.getpostman.com/) or [curl](https://curl.haxx.se/docs/manpage.html).</span></span>

### <a name="routing-and-url-paths"></a><span data-ttu-id="e6537-118">Roteamento e caminhos de URL</span><span class="sxs-lookup"><span data-stu-id="e6537-118">Routing and URL paths</span></span>

<span data-ttu-id="e6537-119">O atributo `[HttpGet]` indica um método que responde a uma solicitação HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="e6537-119">The `[HttpGet]` attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="e6537-120">O caminho da URL de cada método é construído da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="e6537-120">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="e6537-121">Use a cadeia de caracteres de modelo no atributo `Route` do controlador:</span><span class="sxs-lookup"><span data-stu-id="e6537-121">Take the template string in the controller's `Route` attribute:</span></span>

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

::: moniker-end

* <span data-ttu-id="e6537-122">Substitua `[controller]` pelo nome do controlador, que é o nome de classe do controlador menos o sufixo "Controlador" por convenção.</span><span class="sxs-lookup"><span data-stu-id="e6537-122">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="e6537-123">Para esta amostra, o nome de classe do controlador é **Todo**Controller e o nome da raiz é “todo”.</span><span class="sxs-lookup"><span data-stu-id="e6537-123">For this sample, the controller class name is **Todo**Controller and the root name is "todo".</span></span> <span data-ttu-id="e6537-124">O [roteamento](xref:mvc/controllers/routing) do ASP.NET Core não diferencia maiúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="e6537-124">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="e6537-125">Se o atributo `[HttpGet]` tiver um modelo de rota (como `[HttpGet("/products")]`), acrescente isso ao caminho.</span><span class="sxs-lookup"><span data-stu-id="e6537-125">If the `[HttpGet]` attribute has a route template (such as `[HttpGet("/products")]`, append that to the path.</span></span> <span data-ttu-id="e6537-126">Esta amostra não usa um modelo.</span><span class="sxs-lookup"><span data-stu-id="e6537-126">This sample doesn't use a template.</span></span> <span data-ttu-id="e6537-127">Para obter mais informações, confira [Roteamento de atributo com atributos Http[Verb]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="e6537-127">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="e6537-128">No método `GetById` a seguir, `"{id}"` é uma variável de espaço reservado para o identificador exclusivo do item pendente.</span><span class="sxs-lookup"><span data-stu-id="e6537-128">In the following `GetById` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="e6537-129">Quando `GetById` é invocado, ele atribui o valor de `"{id}"` na URL ao parâmetro `id` do método.</span><span class="sxs-lookup"><span data-stu-id="e6537-129">When `GetById` is invoked, it assigns the value of `"{id}"` in the URL to the method's `id` parameter.</span></span>

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

::: moniker-end

<span data-ttu-id="e6537-130">`Name = "GetTodo"` cria uma rota nomeada.</span><span class="sxs-lookup"><span data-stu-id="e6537-130">`Name = "GetTodo"` creates a named route.</span></span> <span data-ttu-id="e6537-131">Rotas nomeadas:</span><span class="sxs-lookup"><span data-stu-id="e6537-131">Named routes:</span></span>

* <span data-ttu-id="e6537-132">permitem que o aplicativo crie um link HTTP usando o nome da rota.</span><span class="sxs-lookup"><span data-stu-id="e6537-132">Enable the app to create an HTTP link using the route name.</span></span>
* <span data-ttu-id="e6537-133">Serão explicadas posteriormente no tutorial.</span><span class="sxs-lookup"><span data-stu-id="e6537-133">Are explained later in the tutorial.</span></span>

### <a name="return-values"></a><span data-ttu-id="e6537-134">Valores de retorno</span><span class="sxs-lookup"><span data-stu-id="e6537-134">Return values</span></span>

<span data-ttu-id="e6537-135">O método `GetAll` retorna uma coleção de objetos `TodoItem`.</span><span class="sxs-lookup"><span data-stu-id="e6537-135">The `GetAll` method returns a collection of `TodoItem` objects.</span></span> <span data-ttu-id="e6537-136">O MVC serializa automaticamente o objeto em [JSON](https://www.json.org/) e grava o JSON no corpo da mensagem de resposta.</span><span class="sxs-lookup"><span data-stu-id="e6537-136">MVC automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="e6537-137">O código de resposta para esse método é 200, supondo que não haja nenhuma exceção sem tratamento.</span><span class="sxs-lookup"><span data-stu-id="e6537-137">The response code for this method is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="e6537-138">As exceções sem tratamento são convertidas em erros 5xx.</span><span class="sxs-lookup"><span data-stu-id="e6537-138">Unhandled exceptions are translated into 5xx errors.</span></span>

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="e6537-139">Por outro lado, o método `GetById` retorna o tipo [IActionResult type](xref:web-api/action-return-types#iactionresult-type) mais geral, que representa uma ampla variedade de tipos de retorno.</span><span class="sxs-lookup"><span data-stu-id="e6537-139">In contrast, the `GetById` method returns the more general [IActionResult type](xref:web-api/action-return-types#iactionresult-type), which represents a wide range of return types.</span></span> <span data-ttu-id="e6537-140">`GetById` tem dois tipos retornados diferentes:</span><span class="sxs-lookup"><span data-stu-id="e6537-140">`GetById` has two different return types:</span></span>

* <span data-ttu-id="e6537-141">Se nenhum item corresponder à ID solicitada, o método retornará um erro 404.</span><span class="sxs-lookup"><span data-stu-id="e6537-141">If no item matches the requested ID, the method returns a 404 error.</span></span> <span data-ttu-id="e6537-142">O retorno [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) retorna uma resposta HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="e6537-142">Returning [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) returns an HTTP 404 response.</span></span>
* <span data-ttu-id="e6537-143">Caso contrário, o método retornará 200 com um corpo de resposta JSON.</span><span class="sxs-lookup"><span data-stu-id="e6537-143">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="e6537-144">O retorno [OK](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) resulta em uma resposta HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="e6537-144">Returning [Ok](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) results in an HTTP 200 response.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="e6537-145">Por outro lado, o método `GetById` retorna o tipo [ActionResult\<T>](xref:web-api/action-return-types#actionresultt-type) mais geral, que representa uma ampla variedade de tipos de retorno.</span><span class="sxs-lookup"><span data-stu-id="e6537-145">In contrast, the `GetById` method returns the [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type), which represents a wide range of return types.</span></span> <span data-ttu-id="e6537-146">`GetById` tem dois tipos retornados diferentes:</span><span class="sxs-lookup"><span data-stu-id="e6537-146">`GetById` has two different return types:</span></span>

* <span data-ttu-id="e6537-147">Se nenhum item corresponder à ID solicitada, o método retornará um erro 404.</span><span class="sxs-lookup"><span data-stu-id="e6537-147">If no item matches the requested ID, the method returns a 404 error.</span></span> <span data-ttu-id="e6537-148">O retorno [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) retorna uma resposta HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="e6537-148">Returning [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) returns an HTTP 404 response.</span></span>
* <span data-ttu-id="e6537-149">Caso contrário, o método retornará 200 com um corpo de resposta JSON.</span><span class="sxs-lookup"><span data-stu-id="e6537-149">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="e6537-150">Retornar `item` resulta em uma resposta HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="e6537-150">Returning `item` results in an HTTP 200 response.</span></span>

::: moniker-end

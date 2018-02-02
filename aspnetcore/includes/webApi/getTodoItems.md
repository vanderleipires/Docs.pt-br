[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

<span data-ttu-id="8fa17-101">O código anterior:</span><span class="sxs-lookup"><span data-stu-id="8fa17-101">The preceding code:</span></span>

* <span data-ttu-id="8fa17-102">Define uma classe de controlador vazia.</span><span class="sxs-lookup"><span data-stu-id="8fa17-102">Defines an empty controller class.</span></span> <span data-ttu-id="8fa17-103">Nas próximas seções, os métodos serão adicionados para implementar a API.</span><span class="sxs-lookup"><span data-stu-id="8fa17-103">In the next sections, methods are added to implement the API.</span></span>
* <span data-ttu-id="8fa17-104">O construtor usa a [Injeção de Dependência](xref:fundamentals/dependency-injection) para injetar o contexto de banco de dados (`TodoContext `) no controlador.</span><span class="sxs-lookup"><span data-stu-id="8fa17-104">The constructor uses [Dependency Injection](xref:fundamentals/dependency-injection) to inject the database context (`TodoContext `) into the controller.</span></span> <span data-ttu-id="8fa17-105">O contexto de banco de dados é usado em cada um dos métodos [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) no controlador.</span><span class="sxs-lookup"><span data-stu-id="8fa17-105">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>
* <span data-ttu-id="8fa17-106">O construtor adiciona um item no banco de dados em memória, caso ele não exista.</span><span class="sxs-lookup"><span data-stu-id="8fa17-106">The constructor adds an item to the in-memory database if one doesn't exist.</span></span>

## <a name="getting-to-do-items"></a><span data-ttu-id="8fa17-107">Obtendo itens de tarefas pendentes</span><span class="sxs-lookup"><span data-stu-id="8fa17-107">Getting to-do items</span></span>

<span data-ttu-id="8fa17-108">Para obter os itens de tarefas pendentes, adicione os métodos a seguir à classe `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="8fa17-108">To get to-do items, add the following methods to the `TodoController` class.</span></span>

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

<span data-ttu-id="8fa17-109">Esses métodos de implementam os dois métodos GET:</span><span class="sxs-lookup"><span data-stu-id="8fa17-109">These methods implement the two GET methods:</span></span>

* `GET /api/todo`
* `GET /api/todo/{id}`

<span data-ttu-id="8fa17-110">Esta é uma resposta HTTP de exemplo para o método `GetAll`:</span><span class="sxs-lookup"><span data-stu-id="8fa17-110">Here is an example HTTP response for the `GetAll` method:</span></span>

```
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
   ```

<span data-ttu-id="8fa17-111">Mais adiante no tutorial, mostrarei como a resposta HTTP pode ser visualizada com o [Postman](https://www.getpostman.com/) ou o [curl](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/curl.1.html).</span><span class="sxs-lookup"><span data-stu-id="8fa17-111">Later in the tutorial I'll show how the HTTP response can be viewed with [Postman](https://www.getpostman.com/) or [curl](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/curl.1.html).</span></span>

### <a name="routing-and-url-paths"></a><span data-ttu-id="8fa17-112">Roteamento e caminhos de URL</span><span class="sxs-lookup"><span data-stu-id="8fa17-112">Routing and URL paths</span></span>

<span data-ttu-id="8fa17-113">O atributo `[HttpGet]` especifica um método HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="8fa17-113">The `[HttpGet]` attribute specifies an HTTP GET method.</span></span> <span data-ttu-id="8fa17-114">O caminho da URL de cada método é construído da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="8fa17-114">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="8fa17-115">Use a cadeia de caracteres de modelo no atributo `Route` do controlador:</span><span class="sxs-lookup"><span data-stu-id="8fa17-115">Take the template string in the controller's `Route` attribute:</span></span>

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

* <span data-ttu-id="8fa17-116">Substitua `[controller]` com o nome do controlador, que é o nome de classe do controlador menos o sufixo "Controlador".</span><span class="sxs-lookup"><span data-stu-id="8fa17-116">Replaces `[controller]` with the name of the controller, which is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="8fa17-117">Para esta amostra, o nome de classe do controlador é **Todo**Controller e o nome da raiz é “todo”.</span><span class="sxs-lookup"><span data-stu-id="8fa17-117">For this sample, the controller class name is **Todo**Controller and the root name is "todo".</span></span> <span data-ttu-id="8fa17-118">O [roteamento](xref:mvc/controllers/routing) do ASP.NET Core não diferencia maiúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="8fa17-118">ASP.NET Core [routing](xref:mvc/controllers/routing) isn't case sensitive.</span></span>
* <span data-ttu-id="8fa17-119">Se o atributo `[HttpGet]` tiver um modelo de rota (como `[HttpGet("/products")]`), acrescente isso ao caminho.</span><span class="sxs-lookup"><span data-stu-id="8fa17-119">If the `[HttpGet]` attribute has a route template (such as `[HttpGet("/products")]`, append that to the path.</span></span> <span data-ttu-id="8fa17-120">Esta amostra não usa um modelo.</span><span class="sxs-lookup"><span data-stu-id="8fa17-120">This sample doesn't use a template.</span></span> <span data-ttu-id="8fa17-121">Consulte [Roteamento de atributo com atributos Http[verb]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="8fa17-121">See [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes) for more information.</span></span>

<span data-ttu-id="8fa17-122">No método `GetById`:</span><span class="sxs-lookup"><span data-stu-id="8fa17-122">In the `GetById` method:</span></span>

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

<span data-ttu-id="8fa17-123">`"{id}"` é uma variável de espaço reservado para a ID do item `todo`.</span><span class="sxs-lookup"><span data-stu-id="8fa17-123">`"{id}"` is a placeholder variable for the ID of the `todo` item.</span></span> <span data-ttu-id="8fa17-124">Quando `GetById` é invocado, ele atribui o valor “{id}” na URL ao parâmetro `id` do método.</span><span class="sxs-lookup"><span data-stu-id="8fa17-124">When `GetById` is invoked, it assigns the value of "{id}" in the URL to the method's `id` parameter.</span></span>

<span data-ttu-id="8fa17-125">`Name = "GetTodo"` cria uma rota nomeada.</span><span class="sxs-lookup"><span data-stu-id="8fa17-125">`Name = "GetTodo"` creates a named route.</span></span> <span data-ttu-id="8fa17-126">Rotas nomeadas:</span><span class="sxs-lookup"><span data-stu-id="8fa17-126">Named routes:</span></span>

* <span data-ttu-id="8fa17-127">permitem que o aplicativo crie um link HTTP usando o nome da rota.</span><span class="sxs-lookup"><span data-stu-id="8fa17-127">Enable the app to create an HTTP link using the route name.</span></span>
* <span data-ttu-id="8fa17-128">Serão explicadas posteriormente no tutorial.</span><span class="sxs-lookup"><span data-stu-id="8fa17-128">Are explained later in the tutorial.</span></span>

### <a name="return-values"></a><span data-ttu-id="8fa17-129">Valores de retorno</span><span class="sxs-lookup"><span data-stu-id="8fa17-129">Return values</span></span>

<span data-ttu-id="8fa17-130">O método `GetAll` retorna um `IEnumerable`.</span><span class="sxs-lookup"><span data-stu-id="8fa17-130">The `GetAll` method returns an `IEnumerable`.</span></span> <span data-ttu-id="8fa17-131">O MVC serializa automaticamente o objeto em [JSON](http://www.json.org/) e grava o JSON no corpo da mensagem de resposta.</span><span class="sxs-lookup"><span data-stu-id="8fa17-131">MVC automatically serializes the object to [JSON](http://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="8fa17-132">O código de resposta para esse método é 200, supondo que não haja nenhuma exceção sem tratamento.</span><span class="sxs-lookup"><span data-stu-id="8fa17-132">The response code for this method is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="8fa17-133">(Exceções sem tratamento são convertidas em erros 5xx.)</span><span class="sxs-lookup"><span data-stu-id="8fa17-133">(Unhandled exceptions are translated into 5xx errors.)</span></span>

<span data-ttu-id="8fa17-134">Por outro lado, o método `GetById` retorna o tipo `IActionResult` mais geral, que representa uma ampla variedade de tipos de retorno.</span><span class="sxs-lookup"><span data-stu-id="8fa17-134">In contrast, the `GetById` method returns the more general `IActionResult` type, which represents a wide range of return types.</span></span> <span data-ttu-id="8fa17-135">`GetById` tem dois tipos retornados diferentes:</span><span class="sxs-lookup"><span data-stu-id="8fa17-135">`GetById` has two different return types:</span></span>

* <span data-ttu-id="8fa17-136">Se nenhum item corresponder à ID solicitada, o método retornará um erro 404.</span><span class="sxs-lookup"><span data-stu-id="8fa17-136">If no item matches the requested ID, the method returns a 404 error.</span></span> <span data-ttu-id="8fa17-137">Retornar `NotFound` retorna uma resposta HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="8fa17-137">Returning `NotFound` returns an HTTP 404 response.</span></span>

* <span data-ttu-id="8fa17-138">Caso contrário, o método retornará 200 com um corpo de resposta JSON.</span><span class="sxs-lookup"><span data-stu-id="8fa17-138">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="8fa17-139">Retornar `ObjectResult` retorna uma resposta HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="8fa17-139">Returning `ObjectResult` returns an HTTP 200 response.</span></span>

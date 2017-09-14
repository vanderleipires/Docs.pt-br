<span data-ttu-id="ef15e-101">[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]</span><span class="sxs-lookup"><span data-stu-id="ef15e-101">[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]</span></span>

<span data-ttu-id="ef15e-102">O código anterior:</span><span class="sxs-lookup"><span data-stu-id="ef15e-102">The preceding code:</span></span>

* <span data-ttu-id="ef15e-103">Define uma classe de controlador vazia.</span><span class="sxs-lookup"><span data-stu-id="ef15e-103">Defines an empty controller class.</span></span> <span data-ttu-id="ef15e-104">Nas próximas seções, adicionaremos métodos para implementar a API.</span><span class="sxs-lookup"><span data-stu-id="ef15e-104">In the next sections, we'll add methods to implement the API.</span></span>
* <span data-ttu-id="ef15e-105">O construtor usa a [Injeção de Dependência](xref:fundamentals/dependency-injection) para injetar o contexto de banco de dados (`TodoContext `) no controlador.</span><span class="sxs-lookup"><span data-stu-id="ef15e-105">The constructor uses [Dependency Injection](xref:fundamentals/dependency-injection) to inject the database context (`TodoContext `) into the controller.</span></span> <span data-ttu-id="ef15e-106">O contexto de banco de dados é usado em cada um dos métodos [CRUD](https://en.wikipedia.org/wiki/Create,_read,_update_and_delete) no controlador.</span><span class="sxs-lookup"><span data-stu-id="ef15e-106">The database context is used in each of the [CRUD](https://en.wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>
* <span data-ttu-id="ef15e-107">O construtor adiciona um item no banco de dados em memória, caso ele não exista.</span><span class="sxs-lookup"><span data-stu-id="ef15e-107">The constructor adds an item to the in-memory database if one doesn't exist.</span></span>

## <a name="getting-to-do-items"></a><span data-ttu-id="ef15e-108">Obtendo itens de tarefas pendentes</span><span class="sxs-lookup"><span data-stu-id="ef15e-108">Getting to-do items</span></span>

<span data-ttu-id="ef15e-109">Para obter os itens de tarefas pendentes, adicione os métodos a seguir à classe `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="ef15e-109">To get to-do items, add the following methods to the `TodoController` class.</span></span>

<span data-ttu-id="ef15e-110">[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]</span><span class="sxs-lookup"><span data-stu-id="ef15e-110">[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]</span></span>

<span data-ttu-id="ef15e-111">Esses métodos de implementam os dois métodos GET:</span><span class="sxs-lookup"><span data-stu-id="ef15e-111">These methods implement the two GET methods:</span></span>

* `GET /api/todo`
* `GET /api/todo/{id}`

<span data-ttu-id="ef15e-112">Esta é uma resposta HTTP de exemplo para o método `GetAll`:</span><span class="sxs-lookup"><span data-stu-id="ef15e-112">Here is an example HTTP response for the `GetAll` method:</span></span>

```
HTTP/1.1 200 OK
   Content-Type: application/json; charset=utf-8
   Server: Microsoft-IIS/10.0
   Date: Thu, 18 Jun 2015 20:51:10 GMT
   Content-Length: 82

   [{"Key":"1", "Name":"Item1","IsComplete":false}]
   ```

<span data-ttu-id="ef15e-113">Mais adiante no tutorial, mostrarei como você pode exibir a resposta HTTP usando o [Postman](https://www.getpostman.com/) ou o [curl](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/curl.1.html).</span><span class="sxs-lookup"><span data-stu-id="ef15e-113">Later in the tutorial I'll show how you can view the HTTP response using [Postman](https://www.getpostman.com/) or [curl](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/curl.1.html).</span></span>

### <a name="routing-and-url-paths"></a><span data-ttu-id="ef15e-114">Roteamento e caminhos de URL</span><span class="sxs-lookup"><span data-stu-id="ef15e-114">Routing and URL paths</span></span>

<span data-ttu-id="ef15e-115">O atributo `[HttpGet]` especifica um método HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="ef15e-115">The `[HttpGet]` attribute specifies an HTTP GET method.</span></span> <span data-ttu-id="ef15e-116">O caminho da URL de cada método é construído da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="ef15e-116">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="ef15e-117">Use a cadeia de caracteres de modelo no atributo de rota do controlador:</span><span class="sxs-lookup"><span data-stu-id="ef15e-117">Take the template string in the controller’s route attribute:</span></span>

<span data-ttu-id="ef15e-118">[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="ef15e-118">[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]</span></span>

* <span data-ttu-id="ef15e-119">Substitua “[Controller]” pelo nome do controlador, que é o nome de classe do controlador menos o sufixo “Controller”.</span><span class="sxs-lookup"><span data-stu-id="ef15e-119">Replace "[Controller]" with the name of the controller, which is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="ef15e-120">Para esta amostra, o nome de classe do controlador é **Todo**Controller e o nome da raiz é “todo”.</span><span class="sxs-lookup"><span data-stu-id="ef15e-120">For this sample, the controller class name is **Todo**Controller and the root name is "todo".</span></span> <span data-ttu-id="ef15e-121">O [roteamento](xref:mvc/controllers/routing) do ASP.NET Core não diferencia maiúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="ef15e-121">ASP.NET Core [routing](xref:mvc/controllers/routing) is not case sensitive.</span></span>
* <span data-ttu-id="ef15e-122">Se o atributo `[HttpGet]` tiver um modelo de rota (como `[HttpGet("/products")]`), acrescente isso ao caminho.</span><span class="sxs-lookup"><span data-stu-id="ef15e-122">If the `[HttpGet]` attribute has a route template (such as `[HttpGet("/products")]`, append that to the path.</span></span> <span data-ttu-id="ef15e-123">Esta amostra não usa um modelo.</span><span class="sxs-lookup"><span data-stu-id="ef15e-123">This sample doesn't use a template.</span></span> <span data-ttu-id="ef15e-124">Consulte [Roteamento de atributo com atributos Http[verb]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="ef15e-124">See [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes) for more information.</span></span>

<span data-ttu-id="ef15e-125">No método `GetById`:</span><span class="sxs-lookup"><span data-stu-id="ef15e-125">In the `GetById` method:</span></span>

```csharp
[HttpGet("{id}", Name = "GetTodo")]
public IActionResult GetById(long id)
```

<span data-ttu-id="ef15e-126">`"{id}"` é uma variável de espaço reservado para a ID do item `todo`.</span><span class="sxs-lookup"><span data-stu-id="ef15e-126">`"{id}"` is a placeholder variable for the ID of the `todo` item.</span></span> <span data-ttu-id="ef15e-127">Quando `GetById` é invocado, ele atribui o valor “{id}” na URL ao parâmetro `id` do método.</span><span class="sxs-lookup"><span data-stu-id="ef15e-127">When `GetById` is invoked, it assigns the value of "{id}" in the URL to the method's `id` parameter.</span></span>

<span data-ttu-id="ef15e-128">`Name = "GetTodo"` cria uma rota nomeada e permite vincular a essa rota em uma Resposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="ef15e-128">`Name = "GetTodo"` creates a named route and allows you to link to this route in an HTTP Response.</span></span> <span data-ttu-id="ef15e-129">Explicarei isso com um exemplo mais adiante.</span><span class="sxs-lookup"><span data-stu-id="ef15e-129">I'll explain it with an example later.</span></span> <span data-ttu-id="ef15e-130">Consulte [Roteamento para ações do controlador](xref:mvc/controllers/routing) para obter informações detalhadas.</span><span class="sxs-lookup"><span data-stu-id="ef15e-130">See [Routing to Controller Actions](xref:mvc/controllers/routing) for detailed information.</span></span>

### <a name="return-values"></a><span data-ttu-id="ef15e-131">Valores de retorno</span><span class="sxs-lookup"><span data-stu-id="ef15e-131">Return values</span></span>

<span data-ttu-id="ef15e-132">O método `GetAll` retorna um `IEnumerable`.</span><span class="sxs-lookup"><span data-stu-id="ef15e-132">The `GetAll` method returns an `IEnumerable`.</span></span> <span data-ttu-id="ef15e-133">O MVC serializa automaticamente o objeto em [JSON](http://www.json.org/) e grava o JSON no corpo da mensagem de resposta.</span><span class="sxs-lookup"><span data-stu-id="ef15e-133">MVC automatically serializes the object to [JSON](http://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="ef15e-134">O código de resposta para esse método é 200, supondo que não haja nenhuma exceção sem tratamento.</span><span class="sxs-lookup"><span data-stu-id="ef15e-134">The response code for this method is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="ef15e-135">(Exceções sem tratamento são convertidas em erros 5xx.)</span><span class="sxs-lookup"><span data-stu-id="ef15e-135">(Unhandled exceptions are translated into 5xx errors.)</span></span>

<span data-ttu-id="ef15e-136">Por outro lado, o método `GetById` retorna o tipo `IActionResult` mais geral, que representa uma ampla variedade de tipos de retorno.</span><span class="sxs-lookup"><span data-stu-id="ef15e-136">In contrast, the `GetById` method returns the more general `IActionResult` type, which represents a wide range of return types.</span></span> <span data-ttu-id="ef15e-137">`GetById` tem dois tipos retornados diferentes:</span><span class="sxs-lookup"><span data-stu-id="ef15e-137">`GetById` has two different return types:</span></span>

* <span data-ttu-id="ef15e-138">Se nenhum item corresponder à ID solicitada, o método retornará um erro 404.</span><span class="sxs-lookup"><span data-stu-id="ef15e-138">If no item matches the requested ID, the method returns a 404 error.</span></span>  <span data-ttu-id="ef15e-139">Isso é feito com o retorno de `NotFound`.</span><span class="sxs-lookup"><span data-stu-id="ef15e-139">This is done by returning `NotFound`.</span></span>

* <span data-ttu-id="ef15e-140">Caso contrário, o método retornará 200 com um corpo de resposta JSON.</span><span class="sxs-lookup"><span data-stu-id="ef15e-140">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="ef15e-141">Isso é feito com o retorno de um `ObjectResult`</span><span class="sxs-lookup"><span data-stu-id="ef15e-141">This is done by returning an `ObjectResult`</span></span>

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

O código anterior define uma classe de controlador de API sem métodos. Nas próximas seções, os métodos serão adicionados para implementar a API.
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

O código anterior define uma classe de controlador de API sem métodos. Nas próximas seções, os métodos serão adicionados para implementar a API. A classe é anotada com um atributo `[ApiController]` para habilitar alguns recursos convenientes. Para obter informações sobre os recursos habilitados pelo atributo, confira [Anotar classe com o ApiControllerAttribute](xref:web-api/index#annotate-class-with-apicontrollerattribute).
::: moniker-end

O construtor do controlador usa a [Injeção de Dependência](xref:fundamentals/dependency-injection) para injetar o contexto de banco de dados (`TodoContext`) no controlador. O contexto de banco de dados é usado em cada um dos métodos [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) no controlador. O construtor adiciona um item no banco de dados em memória, caso ele não exista.

## <a name="get-to-do-items"></a>Obter itens pendentes

Para obter os itens pendentes, adicione os seguintes métodos à classe `TodoController`:

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]
::: moniker-end

Esses métodos de implementam os dois métodos GET:

* `GET /api/todo`
* `GET /api/todo/{id}`

Aqui está um exemplo de resposta HTTP para o método `GetAll`:

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

Mais adiante no tutorial, mostrarei como a resposta HTTP pode ser visualizada com o [Postman](https://www.getpostman.com/) ou o [curl](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/curl.1.html).

### <a name="routing-and-url-paths"></a>Roteamento e caminhos de URL

O atributo `[HttpGet]` indica um método que responde a uma solicitação HTTP GET. O caminho da URL de cada método é construído da seguinte maneira:

* Use a cadeia de caracteres de modelo no atributo `Route` do controlador:

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]
::: moniker-end

* Substitua `[controller]` com o nome do controlador, que é o nome de classe do controlador menos o sufixo "Controlador". Para esta amostra, o nome de classe do controlador é **Todo**Controller e o nome da raiz é “todo”. O [roteamento](xref:mvc/controllers/routing) do ASP.NET Core não diferencia maiúsculas de minúsculas.
* Se o atributo `[HttpGet]` tiver um modelo de rota (como `[HttpGet("/products")]`), acrescente isso ao caminho. Esta amostra não usa um modelo. Para obter mais informações, confira [Roteamento de atributo com atributos Http[Verb]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).

No método `GetById` a seguir, `"{id}"` é uma variável de espaço reservado para o identificador exclusivo do item pendente. Quando `GetById` é invocado, ele atribui o valor de `"{id}"` na URL ao parâmetro `id` do método.

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]
::: moniker-end

`Name = "GetTodo"` cria uma rota nomeada. Rotas nomeadas:

* permitem que o aplicativo crie um link HTTP usando o nome da rota.
* Serão explicadas posteriormente no tutorial.

### <a name="return-values"></a>Valores de retorno

O método `GetAll` retorna uma coleção de objetos `TodoItem`. O MVC serializa automaticamente o objeto em [JSON](https://www.json.org/) e grava o JSON no corpo da mensagem de resposta. O código de resposta para esse método é 200, supondo que não haja nenhuma exceção sem tratamento. As exceções sem tratamento são convertidas em erros 5xx.

::: moniker range="<= aspnetcore-2.0"
Por outro lado, o método `GetById` retorna o tipo [IActionResult type](xref:web-api/action-return-types#iactionresult-type) mais geral, que representa uma ampla variedade de tipos de retorno. `GetById` tem dois tipos retornados diferentes:

* Se nenhum item corresponder à ID solicitada, o método retornará um erro 404. O retorno [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) retorna uma resposta HTTP 404.
* Caso contrário, o método retornará 200 com um corpo de resposta JSON. O retorno [OK](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) resulta em uma resposta HTTP 200.
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
Por outro lado, o método `GetById` retorna o tipo [ActionResult\<T>](xref:web-api/action-return-types#actionresultt-type) mais geral, que representa uma ampla variedade de tipos de retorno. `GetById` tem dois tipos retornados diferentes:

* Se nenhum item corresponder à ID solicitada, o método retornará um erro 404. O retorno [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) retorna uma resposta HTTP 404.
* Caso contrário, o método retornará 200 com um corpo de resposta JSON. Retornar `item` resulta em uma resposta HTTP 200.
::: moniker-end

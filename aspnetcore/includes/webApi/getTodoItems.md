[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

O código anterior:

* Define uma classe de controlador vazia. Nas próximas seções, os métodos serão adicionados para implementar a API.
* O construtor usa a [Injeção de Dependência](xref:fundamentals/dependency-injection) para injetar o contexto de banco de dados (`TodoContext `) no controlador. O contexto de banco de dados é usado em cada um dos métodos [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) no controlador.
* O construtor adiciona um item no banco de dados em memória, caso ele não exista.

## <a name="getting-to-do-items"></a>Obtendo itens de tarefas pendentes

Para obter os itens de tarefas pendentes, adicione os métodos a seguir à classe `TodoController`.

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

Esses métodos de implementam os dois métodos GET:

* `GET /api/todo`
* `GET /api/todo/{id}`

Esta é uma resposta HTTP de exemplo para o método `GetAll`:

```
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

O atributo `[HttpGet]` especifica um método HTTP GET. O caminho da URL de cada método é construído da seguinte maneira:

* Use a cadeia de caracteres de modelo no atributo `Route` do controlador:

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

* Substitua “[Controller]” pelo nome do controlador, que é o nome de classe do controlador menos o sufixo “Controller”. Para esta amostra, o nome de classe do controlador é **Todo**Controller e o nome da raiz é “todo”. O [roteamento](xref:mvc/controllers/routing) do ASP.NET Core não diferencia maiúsculas de minúsculas.
* Se o atributo `[HttpGet]` tiver um modelo de rota (como `[HttpGet("/products")]`), acrescente isso ao caminho. Esta amostra não usa um modelo. Consulte [Roteamento de atributo com atributos Http[verb]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes) para obter mais informações.

No método `GetById`:

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

`"{id}"` é uma variável de espaço reservado para a ID do item `todo`. Quando `GetById` é invocado, ele atribui o valor “{id}” na URL ao parâmetro `id` do método.

`Name = "GetTodo"` cria uma rota nomeada. Rotas nomeadas:

* permitem que o aplicativo crie um link HTTP usando o nome da rota.
* Serão explicadas posteriormente no tutorial.

### <a name="return-values"></a>Valores de retorno

O método `GetAll` retorna um `IEnumerable`. O MVC serializa automaticamente o objeto em [JSON](http://www.json.org/) e grava o JSON no corpo da mensagem de resposta. O código de resposta para esse método é 200, supondo que não haja nenhuma exceção sem tratamento. (Exceções sem tratamento são convertidas em erros 5xx.)

Por outro lado, o método `GetById` retorna o tipo `IActionResult` mais geral, que representa uma ampla variedade de tipos de retorno. `GetById` tem dois tipos retornados diferentes:

* Se nenhum item corresponder à ID solicitada, o método retornará um erro 404. Retornar `NotFound` retorna uma resposta HTTP 404.

* Caso contrário, o método retornará 200 com um corpo de resposta JSON. Retornar `ObjectResult` retorna uma resposta HTTP 200.

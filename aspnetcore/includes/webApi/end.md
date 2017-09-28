## <a name="implement-the-other-crud-operations"></a>Implementar as outras operações CRUD

Adicionaremos os métodos `Create`, `Update` e `Delete` ao controlador. Essas são variações de um mesmo tema e, portanto, mostrarei apenas o código e realçarei as principais diferenças. Compile o projeto depois de adicionar ou alterar o código.

### <a name="create"></a>Create

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

Este é um método HTTP POST, indicado pelo atributo [`[HttpPost]`](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute). O atributo [`[FromBody]`](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) informa ao MVC para obter o valor do item de tarefas pendentes do corpo da solicitação HTTP.

O método `CreatedAtRoute` retorna uma resposta 201, que é a resposta padrão para um método HTTP POST que cria um novo recurso no servidor. `CreatedAtRoute` também adiciona um cabeçalho Local à resposta. O cabeçalho Location especifica o URI do item de tarefas pendentes recém-criado. Consulte [10.2.2 201 criado](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).

### <a name="use-postman-to-send-a-create-request"></a>Usar o Postman para enviar uma solicitação Create

![Console do Postman](../../tutorials/first-web-api/_static/pmc.png)

* Defina o método HTTP como `POST`
* Selecione o botão de opção **Corpo**
* Selecione o botão de opção **bruto**
* Definir o tipo como JSON
* No editor de chave-valor, insira um item de tarefas pendentes, como 

```json
{
    "name":"walk dog",
    "isComplete":true
}
```

* Selecione **Enviar**

* Selecione a guia Cabeçalhos no painel inferior e copie o cabeçalho **Location**:

![Guia Cabeçalhos do console do Postman](../../tutorials/first-web-api/_static/pmget.png)

Use o URI do cabeçalho Location para acessar o recurso que você acabou de criar. Lembre-se de que o método `GetById` criou a rota nomeada `"GetTodo"`:

```csharp
[HttpGet("{id}", Name = "GetTodo")]
public IActionResult GetById(long id)
```

### <a name="update"></a>Atualização

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

`Update` é semelhante a `Create`, mas usa HTTP PUT. A resposta é [204 (Sem conteúdo)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html). De acordo com a especificação HTTP, uma solicitação PUT exige que o cliente envie a entidade atualizada inteira, não apenas os deltas. Para dar suporte a atualizações parciais, use HTTP PATCH.

![Console do Postman mostrando a resposta 204 (Sem conteúdo)](../../tutorials/first-web-api/_static/pmcput.png)

### <a name="delete"></a>Excluir

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

A resposta é [204 (Sem conteúdo)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).

![Console do Postman mostrando a resposta 204 (Sem conteúdo)](../../tutorials/first-web-api/_static/pmd.png)

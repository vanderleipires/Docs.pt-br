## <a name="implement-the-other-crud-operations"></a>Implementar as outras operações CRUD

Nas próximas seções, os métodos `Create`, `Update` e `Delete` serão adicionados ao controlador.

### <a name="create"></a>Create

Adicione o seguinte método `Create`:

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

O código anterior é um método HTTP POST, conforme indicado pelo atributo [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute). O atributo [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) informa ao MVC que ele deve obter o valor do item pendente no corpo da solicitação HTTP.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

O código anterior é um método HTTP POST, conforme indicado pelo atributo [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute). O MVC obtém o valor do item pendente no corpo da solicitação HTTP.

::: moniker-end

O método `CreatedAtRoute`:

* Retorna uma resposta 201. HTTP 201 é a resposta padrão para um método HTTP POST que cria um novo recurso no servidor.
* Adiciona um cabeçalho Local à resposta. O cabeçalho Location especifica o URI do item de tarefas pendentes recém-criado. Consulte [10.2.2 201 criado](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).
* Usa a rota chamada "GetTodo" para criar a URL. A rota chamada "GetTodo" é definida em `GetById`:

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

::: moniker-end

### <a name="use-postman-to-send-a-create-request"></a>Usar o Postman para enviar uma solicitação Create

* Inicie o aplicativo.
* Abra o Postman.

![Console do Postman](../../tutorials/first-web-api/_static/pmc.png)

* Atualize o número da porta na URL do localhost.
* Defina o método HTTP para *POST*.
* Clique na guia **Corpo**.
* Selecione o botão de opção **bruto**.
* Defina o tipo como *JSON (aplicativo/json)*.
* Insira um corpo de solicitação com um item pendente que se assemelhe ao JSON a seguir:

```json
{
  "name":"walk dog",
  "isComplete":true
}
```

* Clique no botão **Enviar**.

::: moniker range=">= aspnetcore-2.1"

> [!TIP]
> Se nenhuma resposta for exibida depois de clicar em **Enviar**, desabilite a opção **Verificação de certificação SSL**. Isso é encontrado em **Arquivo** > **Configurações**. Clique no botão **Enviar** novamente depois de desabilitar a configuração.

::: moniker-end

Clique na guia **Cabeçalhos** no painel **Resposta** e copie o valor do cabeçalho **Local**:

![Guia Cabeçalhos do console do Postman](../../tutorials/first-web-api/_static/pmc2.png)

O URI do cabeçalho Local pode ser usado para acessar o novo item.

### <a name="update"></a>Atualização

Adicione o seguinte método `Update`:

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

::: moniker-end

`Update` é semelhante a `Create`, exceto pelo uso de HTTP PUT. A resposta é [204 (Sem conteúdo)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html). De acordo com a especificação de HTTP, uma solicitação PUT requer que o cliente envie a entidade atualizada inteira, não apenas os deltas. Para dar suporte a atualizações parciais, use HTTP PATCH.

Use o Postman para atualizar o nome do item pendende para "walk cat":

![Console do Postman mostrando a resposta 204 (Sem conteúdo)](../../tutorials/first-web-api/_static/pmcput.png)

### <a name="delete"></a>Excluir

Adicione o seguinte método `Delete`:

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

A resposta `Delete` é [204 (Sem conteúdo)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).

Use Postman para excluir o item pendente:

![Console do Postman mostrando a resposta 204 (Sem conteúdo)](../../tutorials/first-web-api/_static/pmd.png)

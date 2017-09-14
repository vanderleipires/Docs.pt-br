## <a name="implement-the-other-crud-operations"></a><span data-ttu-id="ca25c-101">Implementar as outras operações CRUD</span><span class="sxs-lookup"><span data-stu-id="ca25c-101">Implement the other CRUD operations</span></span>

<span data-ttu-id="ca25c-102">Adicionaremos os métodos `Create`, `Update` e `Delete` ao controlador.</span><span class="sxs-lookup"><span data-stu-id="ca25c-102">We'll add `Create`, `Update`, and `Delete` methods to the controller.</span></span> <span data-ttu-id="ca25c-103">Essas são variações de um mesmo tema e, portanto, mostrarei apenas o código e realçarei as principais diferenças.</span><span class="sxs-lookup"><span data-stu-id="ca25c-103">These are variations on a theme, so I'll just show the code and highlight the main differences.</span></span> <span data-ttu-id="ca25c-104">Compile o projeto depois de adicionar ou alterar o código.</span><span class="sxs-lookup"><span data-stu-id="ca25c-104">Build the project after adding or changing code.</span></span>

### <a name="create"></a><span data-ttu-id="ca25c-105">Create</span><span class="sxs-lookup"><span data-stu-id="ca25c-105">Create</span></span>

<span data-ttu-id="ca25c-106">[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]</span><span class="sxs-lookup"><span data-stu-id="ca25c-106">[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]</span></span>

<span data-ttu-id="ca25c-107">Este é um método HTTP POST, indicado pelo atributo [`[HttpPost]`](https://docs.asp.net/projects/api/en/latest/autoapi/Microsoft/AspNetCore/Mvc/HttpPostAttribute/index.html).</span><span class="sxs-lookup"><span data-stu-id="ca25c-107">This is an HTTP POST method, indicated by the [`[HttpPost]`](https://docs.asp.net/projects/api/en/latest/autoapi/Microsoft/AspNetCore/Mvc/HttpPostAttribute/index.html) attribute.</span></span> <span data-ttu-id="ca25c-108">O atributo [`[FromBody]`](https://docs.asp.net/projects/api/en/latest/autoapi/Microsoft/AspNetCore/Mvc/FromBodyAttribute/index.html) informa ao MVC para obter o valor do item de tarefas pendentes do corpo da solicitação HTTP.</span><span class="sxs-lookup"><span data-stu-id="ca25c-108">The [`[FromBody]`](https://docs.asp.net/projects/api/en/latest/autoapi/Microsoft/AspNetCore/Mvc/FromBodyAttribute/index.html) attribute tells MVC to get the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="ca25c-109">O método `CreatedAtRoute` retorna uma resposta 201, que é a resposta padrão para um método HTTP POST que cria um novo recurso no servidor.</span><span class="sxs-lookup"><span data-stu-id="ca25c-109">The `CreatedAtRoute` method returns a 201 response, which is the standard response for an HTTP POST method that creates a new resource on the server.</span></span> <span data-ttu-id="ca25c-110">`CreatedAtRoute` também adiciona um cabeçalho Local à resposta.</span><span class="sxs-lookup"><span data-stu-id="ca25c-110">`CreatedAtRoute` also adds a Location header to the response.</span></span> <span data-ttu-id="ca25c-111">O cabeçalho Location especifica o URI do item de tarefas pendentes recém-criado.</span><span class="sxs-lookup"><span data-stu-id="ca25c-111">The Location header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="ca25c-112">Consulte [10.2.2 201 criado](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="ca25c-112">See [10.2.2 201 Created](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>

### <a name="use-postman-to-send-a-create-request"></a><span data-ttu-id="ca25c-113">Usar o Postman para enviar uma solicitação Create</span><span class="sxs-lookup"><span data-stu-id="ca25c-113">Use Postman to send a Create request</span></span>

![Console do Postman](../../tutorials/first-web-api/_static/pmc.png)

* <span data-ttu-id="ca25c-115">Defina o método HTTP como `POST`</span><span class="sxs-lookup"><span data-stu-id="ca25c-115">Set the HTTP method to `POST`</span></span>
* <span data-ttu-id="ca25c-116">Selecione o botão de opção **Corpo**</span><span class="sxs-lookup"><span data-stu-id="ca25c-116">Select the **Body** radio button</span></span>
* <span data-ttu-id="ca25c-117">Selecione o botão de opção **bruto**</span><span class="sxs-lookup"><span data-stu-id="ca25c-117">Select the **raw** radio button</span></span>
* <span data-ttu-id="ca25c-118">Definir o tipo como JSON</span><span class="sxs-lookup"><span data-stu-id="ca25c-118">Set the type to JSON</span></span>
* <span data-ttu-id="ca25c-119">No editor de chave-valor, insira um item de tarefas pendentes, como</span><span class="sxs-lookup"><span data-stu-id="ca25c-119">In the key-value editor, enter a Todo item such as</span></span> 

```json
{
    "name":"walk dog",
    "isComplete":true
}
```

* <span data-ttu-id="ca25c-120">Selecione **Enviar**</span><span class="sxs-lookup"><span data-stu-id="ca25c-120">Select **Send**</span></span>

* <span data-ttu-id="ca25c-121">Selecione a guia Cabeçalhos no painel inferior e copie o cabeçalho **Location**:</span><span class="sxs-lookup"><span data-stu-id="ca25c-121">Select the Headers tab in the lower pane and copy the **Location** header:</span></span>

![Guia Cabeçalhos do console do Postman](../../tutorials/first-web-api/_static/pmget.png)

<span data-ttu-id="ca25c-123">Use o URI do cabeçalho Location para acessar o recurso que você acabou de criar.</span><span class="sxs-lookup"><span data-stu-id="ca25c-123">You can use the Location header URI to access the resource you just created.</span></span> <span data-ttu-id="ca25c-124">Lembre-se de que o método `GetById` criou a rota nomeada `"GetTodo"`:</span><span class="sxs-lookup"><span data-stu-id="ca25c-124">Recall the `GetById` method created the `"GetTodo"` named route:</span></span>

```csharp
[HttpGet("{id}", Name = "GetTodo")]
public IActionResult GetById(long id)
```

### <a name="update"></a><span data-ttu-id="ca25c-125">Atualização</span><span class="sxs-lookup"><span data-stu-id="ca25c-125">Update</span></span>

<span data-ttu-id="ca25c-126">[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]</span><span class="sxs-lookup"><span data-stu-id="ca25c-126">[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]</span></span>

<span data-ttu-id="ca25c-127">`Update` é semelhante a `Create`, mas usa HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="ca25c-127">`Update` is similar to `Create`, but uses HTTP PUT.</span></span> <span data-ttu-id="ca25c-128">A resposta é [204 (Sem conteúdo)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="ca25c-128">The response is [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="ca25c-129">De acordo com a especificação HTTP, uma solicitação PUT exige que o cliente envie a entidade atualizada inteira, não apenas os deltas.</span><span class="sxs-lookup"><span data-stu-id="ca25c-129">According to the HTTP spec, a PUT request requires the client to send the entire updated entity, not just the deltas.</span></span> <span data-ttu-id="ca25c-130">Para dar suporte a atualizações parciais, use HTTP PATCH.</span><span class="sxs-lookup"><span data-stu-id="ca25c-130">To support partial updates, use HTTP PATCH.</span></span>

![Console do Postman mostrando a resposta 204 (Sem conteúdo)](../../tutorials/first-web-api/_static/pmcput.png)

### <a name="delete"></a><span data-ttu-id="ca25c-132">Excluir</span><span class="sxs-lookup"><span data-stu-id="ca25c-132">Delete</span></span>

<span data-ttu-id="ca25c-133">[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]</span><span class="sxs-lookup"><span data-stu-id="ca25c-133">[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]</span></span>

<span data-ttu-id="ca25c-134">A resposta é [204 (Sem conteúdo)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="ca25c-134">The response is [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

![Console do Postman mostrando a resposta 204 (Sem conteúdo)](../../tutorials/first-web-api/_static/pmd.png)

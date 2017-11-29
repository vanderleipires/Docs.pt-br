## <a name="implement-the-other-crud-operations"></a><span data-ttu-id="d0747-101">Implementar as outras operações CRUD</span><span class="sxs-lookup"><span data-stu-id="d0747-101">Implement the other CRUD operations</span></span>

<span data-ttu-id="d0747-102">Nas próximas seções, os métodos `Create`, `Update` e `Delete` serão adicionados ao controlador.</span><span class="sxs-lookup"><span data-stu-id="d0747-102">In the following sections, `Create`, `Update`, and `Delete` methods are added to the controller.</span></span>

### <a name="create"></a><span data-ttu-id="d0747-103">Create</span><span class="sxs-lookup"><span data-stu-id="d0747-103">Create</span></span>

<span data-ttu-id="d0747-104">Adicione o seguinte método `Create`.</span><span class="sxs-lookup"><span data-stu-id="d0747-104">Add the following `Create` method.</span></span>

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="d0747-105">O código anterior é um método HTTP POST, indicado pelo atributo [`[HttpPost]`](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute).</span><span class="sxs-lookup"><span data-stu-id="d0747-105">The preceding code is an HTTP POST method, indicated by the [`[HttpPost]`](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="d0747-106">O atributo [`[FromBody]`](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) informa ao MVC para obter o valor do item de tarefas pendentes do corpo da solicitação HTTP.</span><span class="sxs-lookup"><span data-stu-id="d0747-106">The [`[FromBody]`](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) attribute tells MVC to get the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="d0747-107">O método `CreatedAtRoute`:</span><span class="sxs-lookup"><span data-stu-id="d0747-107">The `CreatedAtRoute` method:</span></span>

* <span data-ttu-id="d0747-108">Retorna uma resposta 201.</span><span class="sxs-lookup"><span data-stu-id="d0747-108">Returns a 201 response.</span></span> <span data-ttu-id="d0747-109">HTTP 201 é a resposta padrão para um método HTTP POST que cria um novo recurso no servidor.</span><span class="sxs-lookup"><span data-stu-id="d0747-109">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="d0747-110">Adiciona um cabeçalho Local à resposta.</span><span class="sxs-lookup"><span data-stu-id="d0747-110">Adds a Location header to the response.</span></span> <span data-ttu-id="d0747-111">O cabeçalho Location especifica o URI do item de tarefas pendentes recém-criado.</span><span class="sxs-lookup"><span data-stu-id="d0747-111">The Location header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="d0747-112">Consulte [10.2.2 201 criado](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="d0747-112">See [10.2.2 201 Created](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="d0747-113">Usa a rota chamada "GetTodo" para criar a URL.</span><span class="sxs-lookup"><span data-stu-id="d0747-113">Uses the "GetTodo" named route to create the URL.</span></span> <span data-ttu-id="d0747-114">A rota chamada "GetTodo" é definida em `GetById`:</span><span class="sxs-lookup"><span data-stu-id="d0747-114">The "GetTodo" named route is defined in `GetById`:</span></span>

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

### <a name="use-postman-to-send-a-create-request"></a><span data-ttu-id="d0747-115">Usar o Postman para enviar uma solicitação Create</span><span class="sxs-lookup"><span data-stu-id="d0747-115">Use Postman to send a Create request</span></span>

![Console do Postman](../../tutorials/first-web-api/_static/pmc.png)

* <span data-ttu-id="d0747-117">Defina o método HTTP como `POST`</span><span class="sxs-lookup"><span data-stu-id="d0747-117">Set the HTTP method to `POST`</span></span>
* <span data-ttu-id="d0747-118">Selecione o botão de opção **Corpo**</span><span class="sxs-lookup"><span data-stu-id="d0747-118">Select the **Body** radio button</span></span>
* <span data-ttu-id="d0747-119">Selecione o botão de opção **bruto**</span><span class="sxs-lookup"><span data-stu-id="d0747-119">Select the **raw** radio button</span></span>
* <span data-ttu-id="d0747-120">Definir o tipo como JSON</span><span class="sxs-lookup"><span data-stu-id="d0747-120">Set the type to JSON</span></span>
* <span data-ttu-id="d0747-121">No editor de chave-valor, insira um item de tarefas pendentes, como</span><span class="sxs-lookup"><span data-stu-id="d0747-121">In the key-value editor, enter a Todo item such as</span></span>

```json
{
    "name":"walk dog",
    "isComplete":true
}
```

* <span data-ttu-id="d0747-122">Selecione **Enviar**</span><span class="sxs-lookup"><span data-stu-id="d0747-122">Select **Send**</span></span>
* <span data-ttu-id="d0747-123">Selecione a guia Cabeçalhos no painel inferior e copie o cabeçalho **Location**:</span><span class="sxs-lookup"><span data-stu-id="d0747-123">Select the Headers tab in the lower pane and copy the **Location** header:</span></span>

![Guia Cabeçalhos do console do Postman](../../tutorials/first-web-api/_static/pmget.png)

<span data-ttu-id="d0747-125">O URI do cabeçalho Local pode ser usado para acessar o novo item.</span><span class="sxs-lookup"><span data-stu-id="d0747-125">The Location header URI can be used to access the new item.</span></span>

### <a name="update"></a><span data-ttu-id="d0747-126">Atualização</span><span class="sxs-lookup"><span data-stu-id="d0747-126">Update</span></span>

<span data-ttu-id="d0747-127">Adicione o seguinte método `Update`:</span><span class="sxs-lookup"><span data-stu-id="d0747-127">Add the following `Update` method:</span></span>

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

<span data-ttu-id="d0747-128">`Update` é semelhante a `Create`, mas usa HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="d0747-128">`Update` is similar to `Create`, but uses HTTP PUT.</span></span> <span data-ttu-id="d0747-129">A resposta é [204 (Sem conteúdo)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="d0747-129">The response is [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="d0747-130">De acordo com a especificação HTTP, uma solicitação PUT exige que o cliente envie a entidade atualizada inteira, não apenas os deltas.</span><span class="sxs-lookup"><span data-stu-id="d0747-130">According to the HTTP spec, a PUT request requires the client to send the entire updated entity, not just the deltas.</span></span> <span data-ttu-id="d0747-131">Para dar suporte a atualizações parciais, use HTTP PATCH.</span><span class="sxs-lookup"><span data-stu-id="d0747-131">To support partial updates, use HTTP PATCH.</span></span>

![Console do Postman mostrando a resposta 204 (Sem conteúdo)](../../tutorials/first-web-api/_static/pmcput.png)

### <a name="delete"></a><span data-ttu-id="d0747-133">Excluir</span><span class="sxs-lookup"><span data-stu-id="d0747-133">Delete</span></span>

<span data-ttu-id="d0747-134">Adicione o seguinte método `Delete`:</span><span class="sxs-lookup"><span data-stu-id="d0747-134">Add the following `Delete` method:</span></span>

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="d0747-135">A resposta `Delete` é [204 (Sem conteúdo)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="d0747-135">The `Delete` response is [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

<span data-ttu-id="d0747-136">Teste `Delete`:</span><span class="sxs-lookup"><span data-stu-id="d0747-136">Test `Delete`:</span></span> 

![Console do Postman mostrando a resposta 204 (Sem conteúdo)](../../tutorials/first-web-api/_static/pmd.png)

## <a name="implement-the-other-crud-operations"></a><span data-ttu-id="5a55e-101">Implementar as outras operações CRUD</span><span class="sxs-lookup"><span data-stu-id="5a55e-101">Implement the other CRUD operations</span></span>

<span data-ttu-id="5a55e-102">Nas próximas seções, os métodos `Create`, `Update` e `Delete` serão adicionados ao controlador.</span><span class="sxs-lookup"><span data-stu-id="5a55e-102">In the following sections, `Create`, `Update`, and `Delete` methods are added to the controller.</span></span>

### <a name="create"></a><span data-ttu-id="5a55e-103">Create</span><span class="sxs-lookup"><span data-stu-id="5a55e-103">Create</span></span>

<span data-ttu-id="5a55e-104">Adicione o seguinte método `Create`:</span><span class="sxs-lookup"><span data-stu-id="5a55e-104">Add the following `Create` method:</span></span>

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="5a55e-105">O código anterior é um método HTTP POST, conforme indicado pelo atributo [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute).</span><span class="sxs-lookup"><span data-stu-id="5a55e-105">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="5a55e-106">O atributo [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) informa ao MVC que ele deve obter o valor do item pendente no corpo da solicitação HTTP.</span><span class="sxs-lookup"><span data-stu-id="5a55e-106">The [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) attribute tells MVC to get the value of the to-do item from the body of the HTTP request.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="5a55e-107">O código anterior é um método HTTP POST, conforme indicado pelo atributo [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute).</span><span class="sxs-lookup"><span data-stu-id="5a55e-107">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="5a55e-108">O MVC obtém o valor do item pendente no corpo da solicitação HTTP.</span><span class="sxs-lookup"><span data-stu-id="5a55e-108">MVC gets the value of the to-do item from the body of the HTTP request.</span></span>

::: moniker-end

<span data-ttu-id="5a55e-109">O método `CreatedAtRoute`:</span><span class="sxs-lookup"><span data-stu-id="5a55e-109">The `CreatedAtRoute` method:</span></span>

* <span data-ttu-id="5a55e-110">Retorna uma resposta 201.</span><span class="sxs-lookup"><span data-stu-id="5a55e-110">Returns a 201 response.</span></span> <span data-ttu-id="5a55e-111">HTTP 201 é a resposta padrão para um método HTTP POST que cria um novo recurso no servidor.</span><span class="sxs-lookup"><span data-stu-id="5a55e-111">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="5a55e-112">Adiciona um cabeçalho Local à resposta.</span><span class="sxs-lookup"><span data-stu-id="5a55e-112">Adds a Location header to the response.</span></span> <span data-ttu-id="5a55e-113">O cabeçalho Location especifica o URI do item de tarefas pendentes recém-criado.</span><span class="sxs-lookup"><span data-stu-id="5a55e-113">The Location header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="5a55e-114">Consulte [10.2.2 201 criado](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="5a55e-114">See [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="5a55e-115">Usa a rota chamada "GetTodo" para criar a URL.</span><span class="sxs-lookup"><span data-stu-id="5a55e-115">Uses the "GetTodo" named route to create the URL.</span></span> <span data-ttu-id="5a55e-116">A rota chamada "GetTodo" é definida em `GetById`:</span><span class="sxs-lookup"><span data-stu-id="5a55e-116">The "GetTodo" named route is defined in `GetById`:</span></span>

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

::: moniker-end

### <a name="use-postman-to-send-a-create-request"></a><span data-ttu-id="5a55e-117">Usar o Postman para enviar uma solicitação Create</span><span class="sxs-lookup"><span data-stu-id="5a55e-117">Use Postman to send a Create request</span></span>

* <span data-ttu-id="5a55e-118">Inicie o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="5a55e-118">Start the app.</span></span>
* <span data-ttu-id="5a55e-119">Abra o Postman.</span><span class="sxs-lookup"><span data-stu-id="5a55e-119">Open Postman.</span></span>

![Console do Postman](../../tutorials/first-web-api/_static/pmc.png)

* <span data-ttu-id="5a55e-121">Atualize o número da porta na URL do localhost.</span><span class="sxs-lookup"><span data-stu-id="5a55e-121">Update the port number in the localhost URL.</span></span>
* <span data-ttu-id="5a55e-122">Defina o método HTTP para *POST*.</span><span class="sxs-lookup"><span data-stu-id="5a55e-122">Set the HTTP method to *POST*.</span></span>
* <span data-ttu-id="5a55e-123">Clique na guia **Corpo**.</span><span class="sxs-lookup"><span data-stu-id="5a55e-123">Click the **Body** tab.</span></span>
* <span data-ttu-id="5a55e-124">Selecione o botão de opção **bruto**.</span><span class="sxs-lookup"><span data-stu-id="5a55e-124">Select the **raw** radio button.</span></span>
* <span data-ttu-id="5a55e-125">Defina o tipo como *JSON (aplicativo/json)*.</span><span class="sxs-lookup"><span data-stu-id="5a55e-125">Set the type to *JSON (application/json)*.</span></span>
* <span data-ttu-id="5a55e-126">Insira um corpo de solicitação com um item pendente que se assemelhe ao JSON a seguir:</span><span class="sxs-lookup"><span data-stu-id="5a55e-126">Enter a request body with a to-do item resembling the following JSON:</span></span>

```json
{
  "name":"walk dog",
  "isComplete":true
}
```

* <span data-ttu-id="5a55e-127">Clique no botão **Enviar**.</span><span class="sxs-lookup"><span data-stu-id="5a55e-127">Click the **Send** button.</span></span>

::: moniker range=">= aspnetcore-2.1"

> [!TIP]
> <span data-ttu-id="5a55e-128">Se nenhuma resposta for exibida depois de clicar em **Enviar**, desabilite a opção **Verificação de certificação SSL**.</span><span class="sxs-lookup"><span data-stu-id="5a55e-128">If no response displays after clicking **Send**, disable the **SSL certification verification** option.</span></span> <span data-ttu-id="5a55e-129">Isso é encontrado em **Arquivo** > **Configurações**.</span><span class="sxs-lookup"><span data-stu-id="5a55e-129">This is found under **File** > **Settings**.</span></span> <span data-ttu-id="5a55e-130">Clique no botão **Enviar** novamente depois de desabilitar a configuração.</span><span class="sxs-lookup"><span data-stu-id="5a55e-130">Click the **Send** button again after disabling the setting.</span></span>

::: moniker-end

<span data-ttu-id="5a55e-131">Clique na guia **Cabeçalhos** no painel **Resposta** e copie o valor do cabeçalho **Local**:</span><span class="sxs-lookup"><span data-stu-id="5a55e-131">Click the **Headers** tab in the **Response** pane and copy the **Location** header value:</span></span>

![Guia Cabeçalhos do console do Postman](../../tutorials/first-web-api/_static/pmc2.png)

<span data-ttu-id="5a55e-133">O URI do cabeçalho Local pode ser usado para acessar o novo item.</span><span class="sxs-lookup"><span data-stu-id="5a55e-133">The Location header URI can be used to access the new item.</span></span>

### <a name="update"></a><span data-ttu-id="5a55e-134">Atualização</span><span class="sxs-lookup"><span data-stu-id="5a55e-134">Update</span></span>

<span data-ttu-id="5a55e-135">Adicione o seguinte método `Update`:</span><span class="sxs-lookup"><span data-stu-id="5a55e-135">Add the following `Update` method:</span></span>

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

::: moniker-end

<span data-ttu-id="5a55e-136">`Update` é semelhante a `Create`, exceto pelo uso de HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="5a55e-136">`Update` is similar to `Create`, except it uses HTTP PUT.</span></span> <span data-ttu-id="5a55e-137">A resposta é [204 (Sem conteúdo)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="5a55e-137">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="5a55e-138">De acordo com a especificação de HTTP, uma solicitação PUT requer que o cliente envie a entidade atualizada inteira, não apenas os deltas.</span><span class="sxs-lookup"><span data-stu-id="5a55e-138">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the deltas.</span></span> <span data-ttu-id="5a55e-139">Para dar suporte a atualizações parciais, use HTTP PATCH.</span><span class="sxs-lookup"><span data-stu-id="5a55e-139">To support partial updates, use HTTP PATCH.</span></span>

<span data-ttu-id="5a55e-140">Use o Postman para atualizar o nome do item pendende para "walk cat":</span><span class="sxs-lookup"><span data-stu-id="5a55e-140">Use Postman to update the to-do item's name to "walk cat":</span></span>

![Console do Postman mostrando a resposta 204 (Sem conteúdo)](../../tutorials/first-web-api/_static/pmcput.png)

### <a name="delete"></a><span data-ttu-id="5a55e-142">Excluir</span><span class="sxs-lookup"><span data-stu-id="5a55e-142">Delete</span></span>

<span data-ttu-id="5a55e-143">Adicione o seguinte método `Delete`:</span><span class="sxs-lookup"><span data-stu-id="5a55e-143">Add the following `Delete` method:</span></span>

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="5a55e-144">A resposta `Delete` é [204 (Sem conteúdo)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="5a55e-144">The `Delete` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

<span data-ttu-id="5a55e-145">Use Postman para excluir o item pendente:</span><span class="sxs-lookup"><span data-stu-id="5a55e-145">Use Postman to delete the to-do item:</span></span>

![Console do Postman mostrando a resposta 204 (Sem conteúdo)](../../tutorials/first-web-api/_static/pmd.png)

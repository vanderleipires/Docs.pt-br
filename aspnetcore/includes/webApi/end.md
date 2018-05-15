## <a name="implement-the-other-crud-operations"></a><span data-ttu-id="aa9de-101">Implementar as outras operações CRUD</span><span class="sxs-lookup"><span data-stu-id="aa9de-101">Implement the other CRUD operations</span></span>

<span data-ttu-id="aa9de-102">Nas próximas seções, os métodos `Create`, `Update` e `Delete` serão adicionados ao controlador.</span><span class="sxs-lookup"><span data-stu-id="aa9de-102">In the following sections, `Create`, `Update`, and `Delete` methods are added to the controller.</span></span>

### <a name="create"></a><span data-ttu-id="aa9de-103">Create</span><span class="sxs-lookup"><span data-stu-id="aa9de-103">Create</span></span>

<span data-ttu-id="aa9de-104">Adicione o seguinte método `Create`:</span><span class="sxs-lookup"><span data-stu-id="aa9de-104">Add the following `Create` method:</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="aa9de-105">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]</span><span class="sxs-lookup"><span data-stu-id="aa9de-105">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]</span></span>

<span data-ttu-id="aa9de-106">O código anterior é um método HTTP POST, conforme indicado pelo atributo [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute).</span><span class="sxs-lookup"><span data-stu-id="aa9de-106">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="aa9de-107">O atributo [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) informa ao MVC que ele deve obter o valor do item pendente no corpo da solicitação HTTP.</span><span class="sxs-lookup"><span data-stu-id="aa9de-107">The [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) attribute tells MVC to get the value of the to-do item from the body of the HTTP request.</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="aa9de-108">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]</span><span class="sxs-lookup"><span data-stu-id="aa9de-108">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]</span></span>

<span data-ttu-id="aa9de-109">O código anterior é um método HTTP POST, conforme indicado pelo atributo [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute).</span><span class="sxs-lookup"><span data-stu-id="aa9de-109">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="aa9de-110">O MVC obtém o valor do item pendente no corpo da solicitação HTTP.</span><span class="sxs-lookup"><span data-stu-id="aa9de-110">MVC gets the value of the to-do item from the body of the HTTP request.</span></span>
::: moniker-end

<span data-ttu-id="aa9de-111">O método `CreatedAtRoute`:</span><span class="sxs-lookup"><span data-stu-id="aa9de-111">The `CreatedAtRoute` method:</span></span>

* <span data-ttu-id="aa9de-112">Retorna uma resposta 201.</span><span class="sxs-lookup"><span data-stu-id="aa9de-112">Returns a 201 response.</span></span> <span data-ttu-id="aa9de-113">HTTP 201 é a resposta padrão para um método HTTP POST que cria um novo recurso no servidor.</span><span class="sxs-lookup"><span data-stu-id="aa9de-113">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="aa9de-114">Adiciona um cabeçalho Local à resposta.</span><span class="sxs-lookup"><span data-stu-id="aa9de-114">Adds a Location header to the response.</span></span> <span data-ttu-id="aa9de-115">O cabeçalho Location especifica o URI do item de tarefas pendentes recém-criado.</span><span class="sxs-lookup"><span data-stu-id="aa9de-115">The Location header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="aa9de-116">Consulte [10.2.2 201 criado](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="aa9de-116">See [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="aa9de-117">Usa a rota chamada "GetTodo" para criar a URL.</span><span class="sxs-lookup"><span data-stu-id="aa9de-117">Uses the "GetTodo" named route to create the URL.</span></span> <span data-ttu-id="aa9de-118">A rota chamada "GetTodo" é definida em `GetById`:</span><span class="sxs-lookup"><span data-stu-id="aa9de-118">The "GetTodo" named route is defined in `GetById`:</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="aa9de-119">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]</span><span class="sxs-lookup"><span data-stu-id="aa9de-119">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="aa9de-120">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]</span><span class="sxs-lookup"><span data-stu-id="aa9de-120">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]</span></span>
::: moniker-end

### <a name="use-postman-to-send-a-create-request"></a><span data-ttu-id="aa9de-121">Usar o Postman para enviar uma solicitação Create</span><span class="sxs-lookup"><span data-stu-id="aa9de-121">Use Postman to send a Create request</span></span>

* <span data-ttu-id="aa9de-122">Inicie o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="aa9de-122">Start the app.</span></span>
* <span data-ttu-id="aa9de-123">Abra o Postman.</span><span class="sxs-lookup"><span data-stu-id="aa9de-123">Open Postman.</span></span>

![Console do Postman](../../tutorials/first-web-api/_static/pmc.png)

* <span data-ttu-id="aa9de-125">Atualize o número da porta na URL do localhost.</span><span class="sxs-lookup"><span data-stu-id="aa9de-125">Update the port number in the localhost URL.</span></span>
* <span data-ttu-id="aa9de-126">Defina o método HTTP para *POST*.</span><span class="sxs-lookup"><span data-stu-id="aa9de-126">Set the HTTP method to *POST*.</span></span>
* <span data-ttu-id="aa9de-127">Clique na guia **Corpo**.</span><span class="sxs-lookup"><span data-stu-id="aa9de-127">Click the **Body** tab.</span></span>
* <span data-ttu-id="aa9de-128">Selecione o botão de opção **bruto**.</span><span class="sxs-lookup"><span data-stu-id="aa9de-128">Select the **raw** radio button.</span></span>
* <span data-ttu-id="aa9de-129">Defina o tipo como *JSON (aplicativo/json)*.</span><span class="sxs-lookup"><span data-stu-id="aa9de-129">Set the type to *JSON (application/json)*.</span></span>
* <span data-ttu-id="aa9de-130">Insira um corpo de solicitação com um item pendente que se assemelhe ao JSON a seguir:</span><span class="sxs-lookup"><span data-stu-id="aa9de-130">Enter a request body with a to-do item resembling the following JSON:</span></span>

```json
{
  "name":"walk dog",
  "isComplete":true
}
```

* <span data-ttu-id="aa9de-131">Clique no botão **Enviar**.</span><span class="sxs-lookup"><span data-stu-id="aa9de-131">Click the **Send** button.</span></span>

::: moniker range=">= aspnetcore-2.1"
> [!TIP]
> <span data-ttu-id="aa9de-132">Se nenhuma resposta for exibida depois de clicar em **Enviar**, desabilite a opção **Verificação de certificação SSL**.</span><span class="sxs-lookup"><span data-stu-id="aa9de-132">If no response displays after clicking **Send**, disable the **SSL certification verification** option.</span></span> <span data-ttu-id="aa9de-133">Isso é encontrado em **Arquivo** > **Configurações**.</span><span class="sxs-lookup"><span data-stu-id="aa9de-133">This is found under **File** > **Settings**.</span></span> <span data-ttu-id="aa9de-134">Clique no botão **Enviar** novamente depois de desabilitar a configuração.</span><span class="sxs-lookup"><span data-stu-id="aa9de-134">Click the **Send** button again after disabling the setting.</span></span>
::: moniker-end

<span data-ttu-id="aa9de-135">Clique na guia **Cabeçalhos** no painel **Resposta** e copie o valor do cabeçalho **Local**:</span><span class="sxs-lookup"><span data-stu-id="aa9de-135">Click the **Headers** tab in the **Response** pane and copy the **Location** header value:</span></span>

![Guia Cabeçalhos do console do Postman](../../tutorials/first-web-api/_static/pmc2.png)

<span data-ttu-id="aa9de-137">O URI do cabeçalho Local pode ser usado para acessar o novo item.</span><span class="sxs-lookup"><span data-stu-id="aa9de-137">The Location header URI can be used to access the new item.</span></span>

### <a name="update"></a><span data-ttu-id="aa9de-138">Atualização</span><span class="sxs-lookup"><span data-stu-id="aa9de-138">Update</span></span>

<span data-ttu-id="aa9de-139">Adicione o seguinte método `Update`:</span><span class="sxs-lookup"><span data-stu-id="aa9de-139">Add the following `Update` method:</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="aa9de-140">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]</span><span class="sxs-lookup"><span data-stu-id="aa9de-140">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="aa9de-141">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]</span><span class="sxs-lookup"><span data-stu-id="aa9de-141">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]</span></span>
::: moniker-end

<span data-ttu-id="aa9de-142">`Update` é semelhante a `Create`, exceto pelo uso de HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="aa9de-142">`Update` is similar to `Create`, except it uses HTTP PUT.</span></span> <span data-ttu-id="aa9de-143">A resposta é [204 (Sem conteúdo)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="aa9de-143">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="aa9de-144">De acordo com a especificação de HTTP, uma solicitação PUT requer que o cliente envie a entidade atualizada inteira, não apenas os deltas.</span><span class="sxs-lookup"><span data-stu-id="aa9de-144">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the deltas.</span></span> <span data-ttu-id="aa9de-145">Para dar suporte a atualizações parciais, use HTTP PATCH.</span><span class="sxs-lookup"><span data-stu-id="aa9de-145">To support partial updates, use HTTP PATCH.</span></span>

<span data-ttu-id="aa9de-146">Use o Postman para atualizar o nome do item pendende para "walk cat":</span><span class="sxs-lookup"><span data-stu-id="aa9de-146">Use Postman to update the to-do item's name to "walk cat":</span></span>

![Console do Postman mostrando a resposta 204 (Sem conteúdo)](../../tutorials/first-web-api/_static/pmcput.png)

### <a name="delete"></a><span data-ttu-id="aa9de-148">Excluir</span><span class="sxs-lookup"><span data-stu-id="aa9de-148">Delete</span></span>

<span data-ttu-id="aa9de-149">Adicione o seguinte método `Delete`:</span><span class="sxs-lookup"><span data-stu-id="aa9de-149">Add the following `Delete` method:</span></span>

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="aa9de-150">A resposta `Delete` é [204 (Sem conteúdo)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="aa9de-150">The `Delete` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

<span data-ttu-id="aa9de-151">Use Postman para excluir o item pendente:</span><span class="sxs-lookup"><span data-stu-id="aa9de-151">Use Postman to delete the to-do item:</span></span>

![Console do Postman mostrando a resposta 204 (Sem conteúdo)](../../tutorials/first-web-api/_static/pmd.png)

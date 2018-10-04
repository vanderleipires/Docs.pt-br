---
uid: web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
title: Teste de unidade controladores no ASP.NET Web API 2 | Microsoft Docs
author: MikeWasson
description: Este tópico descreve algumas técnicas específicas para controladores de teste na Web API 2 de unidade. Antes de ler este tópico, você talvez queira ler o tutorial de unidade...
ms.author: riande
ms.date: 06/11/2014
ms.assetid: 43a6cce7-a3ef-42aa-ad06-90d36d49f098
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: e1bb1aa120ced95db7674eae1831f2a2c7356fc0
ms.sourcegitcommit: 7890dfb5a8f8c07d813f166d3ab0c263f893d0c6
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/04/2018
ms.locfileid: "48794818"
---
<a name="unit-testing-controllers-in-aspnet-web-api-2"></a><span data-ttu-id="36903-104">Teste de unidade controladores no ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="36903-104">Unit Testing Controllers in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="36903-105">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="36903-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="36903-106">Este tópico descreve algumas técnicas específicas para controladores de teste na Web API 2 de unidade.</span><span class="sxs-lookup"><span data-stu-id="36903-106">This topic describes some specific techniques for unit testing controllers in Web API 2.</span></span> <span data-ttu-id="36903-107">Antes de ler este tópico, você talvez queira ler o tutorial [unidade de teste de API Web ASP.NET 2](unit-testing-with-aspnet-web-api.md), que mostra como adicionar um projeto de teste de unidade à sua solução.</span><span class="sxs-lookup"><span data-stu-id="36903-107">Before reading this topic, you might want to read the tutorial [Unit Testing ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), which shows how to add a unit-test project to your solution.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="36903-108">Versões de software usadas no tutorial</span><span class="sxs-lookup"><span data-stu-id="36903-108">Software versions used in the tutorial</span></span>
>
> - [<span data-ttu-id="36903-109">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="36903-109">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
> - <span data-ttu-id="36903-110">API Web 2</span><span class="sxs-lookup"><span data-stu-id="36903-110">Web API 2</span></span>
> - <span data-ttu-id="36903-111">[Moq](https://github.com/Moq) 4.5.30</span><span class="sxs-lookup"><span data-stu-id="36903-111">[Moq](https://github.com/Moq) 4.5.30</span></span>

> [!NOTE]
> <span data-ttu-id="36903-112">Eu usei o Moq, mas a mesma ideia se aplica a qualquer estrutura de simulação.</span><span class="sxs-lookup"><span data-stu-id="36903-112">I used Moq, but the same idea applies to any mocking framework.</span></span> <span data-ttu-id="36903-113">Moq 4.5.30 (e posterior) dá suporte ao Visual Studio 2017, Roslyn e .NET 4.5 e versões posteriores.</span><span class="sxs-lookup"><span data-stu-id="36903-113">Moq 4.5.30 (and later) supports Visual Studio 2017, Roslyn and .NET 4.5 and later versions.</span></span>

<span data-ttu-id="36903-114">É um padrão comum em testes de unidade &quot;organizar act-declaração&quot;:</span><span class="sxs-lookup"><span data-stu-id="36903-114">A common pattern in unit tests is &quot;arrange-act-assert&quot;:</span></span>

- <span data-ttu-id="36903-115">Organizar: Configure todos os pré-requisitos para o teste seja executado.</span><span class="sxs-lookup"><span data-stu-id="36903-115">Arrange: Set up any prerequisites for the test to run.</span></span>
- <span data-ttu-id="36903-116">O ACT: Execute o teste.</span><span class="sxs-lookup"><span data-stu-id="36903-116">Act: Perform the test.</span></span>
- <span data-ttu-id="36903-117">Assert: Verifique se o teste foi bem-sucedido.</span><span class="sxs-lookup"><span data-stu-id="36903-117">Assert: Verify that the test succeeded.</span></span>

<span data-ttu-id="36903-118">Na etapa Arranjar, você geralmente usar simulação ou objetos stub.</span><span class="sxs-lookup"><span data-stu-id="36903-118">In the arrange step, you will often use mock or stub objects.</span></span> <span data-ttu-id="36903-119">Que minimiza o número de dependências, portanto, o teste é voltada para o teste uma coisa.</span><span class="sxs-lookup"><span data-stu-id="36903-119">That minimizes the number of dependencies, so the test is focused on testing one thing.</span></span>

<span data-ttu-id="36903-120">Aqui estão algumas coisas que você deve teste de unidade em seus controladores de API da Web:</span><span class="sxs-lookup"><span data-stu-id="36903-120">Here are some things that you should unit test in your Web API controllers:</span></span>

- <span data-ttu-id="36903-121">A ação retorna o tipo correto da resposta.</span><span class="sxs-lookup"><span data-stu-id="36903-121">The action returns the correct type of response.</span></span>
- <span data-ttu-id="36903-122">Parâmetros inválidos retornam a resposta de corrigir o erro.</span><span class="sxs-lookup"><span data-stu-id="36903-122">Invalid parameters return the correct error response.</span></span>
- <span data-ttu-id="36903-123">A ação chama o método correto na camada de repositório ou serviço.</span><span class="sxs-lookup"><span data-stu-id="36903-123">The action calls the correct method on the repository or service layer.</span></span>
- <span data-ttu-id="36903-124">Se a resposta inclui um modelo de domínio, verifique se o tipo de modelo.</span><span class="sxs-lookup"><span data-stu-id="36903-124">If the response includes a domain model, verify the model type.</span></span>

<span data-ttu-id="36903-125">Estas são algumas das coisas gerais para testar, mas as especificações dependem de sua implementação do controlador.</span><span class="sxs-lookup"><span data-stu-id="36903-125">These are some of the general things to test, but the specifics depend on your controller implementation.</span></span> <span data-ttu-id="36903-126">Em particular, ele faz uma grande diferença se ações do controlador retornam **HttpResponseMessage** ou **IHttpActionResult**.</span><span class="sxs-lookup"><span data-stu-id="36903-126">In particular, it makes a big difference whether your controller actions return **HttpResponseMessage** or **IHttpActionResult**.</span></span> <span data-ttu-id="36903-127">Para obter mais informações sobre esses tipos de resultado, consulte [resultados da ação na Api Web 2](../getting-started-with-aspnet-web-api/action-results.md).</span><span class="sxs-lookup"><span data-stu-id="36903-127">For more information about these result types, see [Action Results in Web Api 2](../getting-started-with-aspnet-web-api/action-results.md).</span></span>

## <a name="testing-actions-that-return-httpresponsemessage"></a><span data-ttu-id="36903-128">Ações que retornam HttpResponseMessage de testes</span><span class="sxs-lookup"><span data-stu-id="36903-128">Testing Actions that Return HttpResponseMessage</span></span>

<span data-ttu-id="36903-129">Aqui está um exemplo de um controlador cujo retorno de ações **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="36903-129">Here is an example of a controller whose actions return **HttpResponseMessage**.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample1.cs)]

<span data-ttu-id="36903-130">Observe que o controlador usa a injeção de dependência para injetar um `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="36903-130">Notice the controller uses dependency injection to inject an `IProductRepository`.</span></span> <span data-ttu-id="36903-131">Isso torna o controlador mais testável, porque você pode injetar um repositório fictício.</span><span class="sxs-lookup"><span data-stu-id="36903-131">That makes the controller more testable, because you can inject a mock repository.</span></span> <span data-ttu-id="36903-132">O teste de unidade a seguir verifica se o `Get` método gravações um `Product` no corpo da resposta.</span><span class="sxs-lookup"><span data-stu-id="36903-132">The following unit test verifies that the `Get` method writes a `Product` to the response body.</span></span> <span data-ttu-id="36903-133">Suponha que `repository` é uma simulação `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="36903-133">Assume that `repository` is a mock `IProductRepository`.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample2.cs)]

<span data-ttu-id="36903-134">É importante definir **solicitar** e **configuração** no controlador.</span><span class="sxs-lookup"><span data-stu-id="36903-134">It's important to set **Request** and **Configuration** on the controller.</span></span> <span data-ttu-id="36903-135">Caso contrário, o teste falhará com um **ArgumentNullException** ou **InvalidOperationException**.</span><span class="sxs-lookup"><span data-stu-id="36903-135">Otherwise, the test will fail with an **ArgumentNullException** or **InvalidOperationException**.</span></span>

## <a name="testing-link-generation"></a><span data-ttu-id="36903-136">Teste de geração de Link</span><span class="sxs-lookup"><span data-stu-id="36903-136">Testing Link Generation</span></span>

<span data-ttu-id="36903-137">O `Post` chamadas de método **UrlHelper.Link** criar links na resposta.</span><span class="sxs-lookup"><span data-stu-id="36903-137">The `Post` method calls **UrlHelper.Link** to create links in the response.</span></span> <span data-ttu-id="36903-138">Isso requer um pouco mais de configuração no teste de unidade:</span><span class="sxs-lookup"><span data-stu-id="36903-138">This requires a little more setup in the unit test:</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample3.cs)]

<span data-ttu-id="36903-139">O **UrlHelper** classe precisa os dados de rota e de URL de solicitação, para que o teste precisa definir valores para eles.</span><span class="sxs-lookup"><span data-stu-id="36903-139">The **UrlHelper** class needs the request URL and route data, so the test has to set values for these.</span></span> <span data-ttu-id="36903-140">Outra opção é simulação ou stub **UrlHelper**.</span><span class="sxs-lookup"><span data-stu-id="36903-140">Another option is mock or stub **UrlHelper**.</span></span> <span data-ttu-id="36903-141">Com essa abordagem, você substitua o valor padrão de [ApiController.Url](https://msdn.microsoft.com/library/system.web.http.apicontroller.url.aspx) com uma versão de simulação ou stub que retorna um valor fixo.</span><span class="sxs-lookup"><span data-stu-id="36903-141">With this approach, you replace the default value of [ApiController.Url](https://msdn.microsoft.com/library/system.web.http.apicontroller.url.aspx) with a mock or stub version that returns a fixed value.</span></span>

<span data-ttu-id="36903-142">Vamos reescrever o teste usando o [Moq](https://github.com/Moq) framework.</span><span class="sxs-lookup"><span data-stu-id="36903-142">Let's rewrite the test using the [Moq](https://github.com/Moq) framework.</span></span> <span data-ttu-id="36903-143">Instalar o `Moq` pacote do NuGet no projeto de teste.</span><span class="sxs-lookup"><span data-stu-id="36903-143">Install the `Moq` NuGet package in the test project.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample4.cs)]

<span data-ttu-id="36903-144">Nesta versão, você não precisa configurar quaisquer dados de rota, porque a simulação **UrlHelper** retorna uma cadeia de caracteres constante.</span><span class="sxs-lookup"><span data-stu-id="36903-144">In this version, you don't need to set up any route data, because the mock **UrlHelper** returns a constant string.</span></span>


## <a name="testing-actions-that-return-ihttpactionresult"></a><span data-ttu-id="36903-145">Ações que retornam IHttpActionResult de testes</span><span class="sxs-lookup"><span data-stu-id="36903-145">Testing Actions that Return IHttpActionResult</span></span>

<span data-ttu-id="36903-146">Na API Web 2, uma ação do controlador pode retornar **IHttpActionResult**, que é análogo à **ActionResult** no ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="36903-146">In Web API 2, a controller action can return **IHttpActionResult**, which is analogous to **ActionResult** in ASP.NET MVC.</span></span> <span data-ttu-id="36903-147">O **IHttpActionResult** interface define um padrão de comando para criar respostas HTTP.</span><span class="sxs-lookup"><span data-stu-id="36903-147">The **IHttpActionResult** interface defines a command pattern for creating HTTP responses.</span></span> <span data-ttu-id="36903-148">Em vez de criar a resposta diretamente, o controlador retorna um **IHttpActionResult**.</span><span class="sxs-lookup"><span data-stu-id="36903-148">Instead of creating the response directly, the controller returns an **IHttpActionResult**.</span></span> <span data-ttu-id="36903-149">Posteriormente, o pipeline chama o **IHttpActionResult** para criar a resposta.</span><span class="sxs-lookup"><span data-stu-id="36903-149">Later, the pipeline invokes the **IHttpActionResult** to create the response.</span></span> <span data-ttu-id="36903-150">Essa abordagem torna mais fácil escrever testes de unidade, porque você pode ignorar muito o que é necessária para instalação **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="36903-150">This approach makes it easier to write unit tests, because you can skip a lot of the setup that is needed for **HttpResponseMessage**.</span></span>

<span data-ttu-id="36903-151">Aqui está um exemplo controller cujo retorno de ações **IHttpActionResult**.</span><span class="sxs-lookup"><span data-stu-id="36903-151">Here is an example controller whose actions return **IHttpActionResult**.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample5.cs)]

<span data-ttu-id="36903-152">Este exemplo mostra alguns padrões comuns de uso **IHttpActionResult**.</span><span class="sxs-lookup"><span data-stu-id="36903-152">This example shows some common patterns using **IHttpActionResult**.</span></span> <span data-ttu-id="36903-153">Vamos ver como a unidade de testá-los.</span><span class="sxs-lookup"><span data-stu-id="36903-153">Let's see how to unit test them.</span></span>

### <a name="action-returns-200-ok-with-a-response-body"></a><span data-ttu-id="36903-154">Ação retorna 200 (Okey) com um corpo de resposta</span><span class="sxs-lookup"><span data-stu-id="36903-154">Action returns 200 (OK) with a response body</span></span>

<span data-ttu-id="36903-155">O `Get` chamadas de método `Ok(product)` se o produto for encontrado.</span><span class="sxs-lookup"><span data-stu-id="36903-155">The `Get` method calls `Ok(product)` if the product is found.</span></span> <span data-ttu-id="36903-156">No teste de unidade, verifique se o tipo de retorno será **OkNegotiatedContentResult** e o produto retornado tem a ID da direita.</span><span class="sxs-lookup"><span data-stu-id="36903-156">In the unit test, make sure the return type is **OkNegotiatedContentResult** and the returned product has the right ID.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample6.cs)]

<span data-ttu-id="36903-157">Observe que o teste de unidade não é o resultado da ação executada.</span><span class="sxs-lookup"><span data-stu-id="36903-157">Notice that the unit test doesn't execute the action result.</span></span> <span data-ttu-id="36903-158">Você pode supor que o resultado da ação cria a resposta HTTP corretamente.</span><span class="sxs-lookup"><span data-stu-id="36903-158">You can assume the action result creates the HTTP response correctly.</span></span> <span data-ttu-id="36903-159">(É por isso que a estrutura da API Web tem seus próprios testes de unidade!)</span><span class="sxs-lookup"><span data-stu-id="36903-159">(That's why the Web API framework has its own unit tests!)</span></span>

### <a name="action-returns-404-not-found"></a><span data-ttu-id="36903-160">Ação retorna 404 (não encontrado)</span><span class="sxs-lookup"><span data-stu-id="36903-160">Action returns 404 (Not Found)</span></span>

<span data-ttu-id="36903-161">O `Get` chamadas de método `NotFound()` se o produto não for encontrado.</span><span class="sxs-lookup"><span data-stu-id="36903-161">The `Get` method calls `NotFound()` if the product is not found.</span></span> <span data-ttu-id="36903-162">Nesse caso, o teste de unidade apenas verifica se o tipo de retorno será **NotFoundResult**.</span><span class="sxs-lookup"><span data-stu-id="36903-162">For this case, the unit test just checks if the return type is **NotFoundResult**.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample7.cs)]

### <a name="action-returns-200-ok-with-no-response-body"></a><span data-ttu-id="36903-163">Ação retorna 200 (Okey) sem o corpo da resposta</span><span class="sxs-lookup"><span data-stu-id="36903-163">Action returns 200 (OK) with no response body</span></span>

<span data-ttu-id="36903-164">O `Delete` chamadas de método `Ok()` para retornar uma resposta HTTP 200 vazia.</span><span class="sxs-lookup"><span data-stu-id="36903-164">The `Delete` method calls `Ok()` to return an empty HTTP 200 response.</span></span> <span data-ttu-id="36903-165">Como no exemplo anterior, o teste de unidade verifica o tipo de retorno, neste caso **OkResult**.</span><span class="sxs-lookup"><span data-stu-id="36903-165">Like the previous example, the unit test checks the return type, in this case **OkResult**.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample8.cs)]

### <a name="action-returns-201-created-with-a-location-header"></a><span data-ttu-id="36903-166">Ação retorna 201 (criado) com um cabeçalho de localização</span><span class="sxs-lookup"><span data-stu-id="36903-166">Action returns 201 (Created) with a Location header</span></span>

<span data-ttu-id="36903-167">O `Post` chamadas de método `CreatedAtRoute` para retornar uma resposta HTTP 201 com um URI no cabeçalho Location.</span><span class="sxs-lookup"><span data-stu-id="36903-167">The `Post` method calls `CreatedAtRoute` to return an HTTP 201 response with a URI in the Location header.</span></span> <span data-ttu-id="36903-168">No teste de unidade, verifique se que a ação define os valores corretos de roteamentos.</span><span class="sxs-lookup"><span data-stu-id="36903-168">In the unit test, verify that the action sets the correct routing values.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample9.cs)]

### <a name="action-returns-another-2xx-with-a-response-body"></a><span data-ttu-id="36903-169">Ação retorna outro 2xx com um corpo de resposta</span><span class="sxs-lookup"><span data-stu-id="36903-169">Action returns another 2xx with a response body</span></span>

<span data-ttu-id="36903-170">O `Put` chamadas de método `Content` para retornar uma resposta de HTTP 202 (aceito) com um corpo de resposta.</span><span class="sxs-lookup"><span data-stu-id="36903-170">The `Put` method calls `Content` to return an HTTP 202 (Accepted) response with a response body.</span></span> <span data-ttu-id="36903-171">Nesse caso, é semelhante ao retornar 200 (Okey), mas o teste de unidade também deve verificar o código de status.</span><span class="sxs-lookup"><span data-stu-id="36903-171">This case is similar to returning 200 (OK), but the unit test should also check the status code.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample10.cs)]

## <a name="additional-resources"></a><span data-ttu-id="36903-172">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="36903-172">Additional Resources</span></span>

- [<span data-ttu-id="36903-173">Simulação do Entity Framework quando a API Web ASP.NET 2 de teste de unidade</span><span class="sxs-lookup"><span data-stu-id="36903-173">Mocking Entity Framework when Unit Testing ASP.NET Web API 2</span></span>](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md)
- <span data-ttu-id="36903-174">[Escrever testes para um serviço de API Web ASP.NET](https://blogs.msdn.com/b/youssefm/archive/2013/01/28/writing-tests-for-an-asp-net-webapi-service.aspx) (postagem de blog de Youssef Moussaoui).</span><span class="sxs-lookup"><span data-stu-id="36903-174">[Writing tests for an ASP.NET Web API service](https://blogs.msdn.com/b/youssefm/archive/2013/01/28/writing-tests-for-an-asp-net-webapi-service.aspx) (blog post by Youssef Moussaoui).</span></span>
- [<span data-ttu-id="36903-175">API da Web do ASP.NET com depurador de rota de depuração</span><span class="sxs-lookup"><span data-stu-id="36903-175">Debugging ASP.NET Web API with Route Debugger</span></span>](https://blogs.msdn.com/b/webdev/archive/2013/04/04/debugging-asp-net-web-api-with-route-debugger.aspx)

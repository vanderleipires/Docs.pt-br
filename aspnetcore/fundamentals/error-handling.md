---
title: Tratamento de erro no ASP.NET Core
author: ardalis
description: Descubra como tratar erros em aplicativos ASP.NET Core.
manager: wpickett
ms.author: tdykstra
ms.custom: H1Hack27Feb2017
ms.date: 11/30/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/error-handling
ms.openlocfilehash: 5b0cda7b79b8a9523d1ba6a9b321d22d3ccc753a
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/30/2018
---
# <a name="introduction-to-error-handling-in-aspnet-core"></a><span data-ttu-id="69789-103">Introdução ao tratamento de erro no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="69789-103">Introduction to Error Handling in ASP.NET Core</span></span>

<span data-ttu-id="69789-104">Por [Steve Smith](https://ardalis.com/) e [Tom Dykstra](https://github.com/tdykstra/)</span><span class="sxs-lookup"><span data-stu-id="69789-104">By [Steve Smith](https://ardalis.com/) and [Tom Dykstra](https://github.com/tdykstra/)</span></span>

<span data-ttu-id="69789-105">Este artigo apresenta abordagens comuns para o tratamento de erros em aplicativos ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="69789-105">This article covers common appoaches to handling errors in ASP.NET Core apps.</span></span>

<span data-ttu-id="69789-106">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="69789-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="the-developer-exception-page"></a><span data-ttu-id="69789-107">A página de exceção do desenvolvedor</span><span class="sxs-lookup"><span data-stu-id="69789-107">The developer exception page</span></span>

<span data-ttu-id="69789-108">Para configurar um aplicativo para exibir uma página que mostra informações detalhadas sobre exceções, instale o pacote NuGet `Microsoft.AspNetCore.Diagnostics` e adicione uma linha ao [método Configure na classe Startup](startup.md):</span><span class="sxs-lookup"><span data-stu-id="69789-108">To configure an app to display a page that shows detailed information about exceptions, install the `Microsoft.AspNetCore.Diagnostics` NuGet package and add a line to the [Configure method in the Startup class](startup.md):</span></span>

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=7)]

<span data-ttu-id="69789-109">Coloque `UseDeveloperExceptionPage` antes de qualquer middleware no qual você deseja capturar exceções, como `app.UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="69789-109">Put `UseDeveloperExceptionPage` before any middleware you want to catch exceptions in, such as `app.UseMvc`.</span></span>

>[!WARNING]
> <span data-ttu-id="69789-110">Habilite a página de exceção do desenvolvedor **somente quando o aplicativo estiver em execução no ambiente de Desenvolvimento**.</span><span class="sxs-lookup"><span data-stu-id="69789-110">Enable the developer exception page **only when the app is running in the Development environment**.</span></span> <span data-ttu-id="69789-111">Não é recomendável compartilhar informações de exceção detalhadas publicamente quando o aplicativo é executado em produção.</span><span class="sxs-lookup"><span data-stu-id="69789-111">You don't want to share detailed exception information publicly when the app runs in production.</span></span> <span data-ttu-id="69789-112">[Saiba mais sobre como configurar ambientes](environments.md).</span><span class="sxs-lookup"><span data-stu-id="69789-112">[Learn more about configuring environments](environments.md).</span></span>

<span data-ttu-id="69789-113">Para ver a página de exceção do desenvolvedor, execute o aplicativo de exemplo com o ambiente definido como `Development` e adicione `?throw=true` à URL base do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="69789-113">To see the developer exception page, run the sample application with the environment set to `Development`, and add `?throw=true` to the base URL of the app.</span></span> <span data-ttu-id="69789-114">A página inclui várias guias com informações sobre a exceção e a solicitação.</span><span class="sxs-lookup"><span data-stu-id="69789-114">The page includes several tabs with information about the exception and the request.</span></span> <span data-ttu-id="69789-115">A primeira guia inclui um rastreamento de pilha.</span><span class="sxs-lookup"><span data-stu-id="69789-115">The first tab includes a stack trace.</span></span> 

![Rastreamento de pilha](error-handling/_static/developer-exception-page.png)

<span data-ttu-id="69789-117">A próxima guia mostra os parâmetros da cadeia de caracteres de consulta, se houver.</span><span class="sxs-lookup"><span data-stu-id="69789-117">The next tab shows the query string parameters, if any.</span></span>

![Parâmetros de cadeia de caracteres de consulta](error-handling/_static/developer-exception-page-query.png)

<span data-ttu-id="69789-119">Essa solicitação não tinha nenhum cookie, mas se tivesse, eles seriam exibidos na guia **Cookies**. Veja os cabeçalhos que foram passados na última guia.</span><span class="sxs-lookup"><span data-stu-id="69789-119">This request didn't have any cookies, but if it did, they would appear on the **Cookies** tab. You can see the headers that were passed in the last tab.</span></span>

![Cabeçalhos](error-handling/_static/developer-exception-page-headers.png)

## <a name="configuring-a-custom-exception-handling-page"></a><span data-ttu-id="69789-121">Configurando uma página de tratamento de exceção personalizada</span><span class="sxs-lookup"><span data-stu-id="69789-121">Configuring a custom exception handling page</span></span>

<span data-ttu-id="69789-122">É uma boa ideia configurar uma página de manipulador de exceção a ser usada quando o aplicativo não está sendo executado no ambiente `Development`.</span><span class="sxs-lookup"><span data-stu-id="69789-122">It's a good idea to configure an exception handler page to use when the app isn't running in the `Development` environment.</span></span>

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=11)]

<span data-ttu-id="69789-123">Em um aplicativo MVC, não decore o método de ação do manipulador de erro de forma explícita com atributos de método HTTP, como `HttpGet`.</span><span class="sxs-lookup"><span data-stu-id="69789-123">In an MVC app, don't explicitly decorate the error handler action method with HTTP method attributes, such as `HttpGet`.</span></span> <span data-ttu-id="69789-124">O uso de verbos explícitos pode impedir que algumas solicitações sejam recebidas pelo método.</span><span class="sxs-lookup"><span data-stu-id="69789-124">Using explicit verbs could prevent some requests from reaching the method.</span></span>

```csharp
[Route("/Error")]
public IActionResult Index()
{
    // Handle error here
}
```

## <a name="configuring-status-code-pages"></a><span data-ttu-id="69789-125">Configurando páginas de código de status</span><span class="sxs-lookup"><span data-stu-id="69789-125">Configuring status code pages</span></span>

<span data-ttu-id="69789-126">Por padrão, o aplicativo não fornecerá uma página de código de status detalhada para códigos de status HTTP como 500 (Erro Interno do Servidor) ou 404 (Não Encontrado).</span><span class="sxs-lookup"><span data-stu-id="69789-126">By default, your app won't provide a rich status code page for HTTP status codes such as 500 (Internal Server Error) or 404 (Not Found).</span></span> <span data-ttu-id="69789-127">Configure o `StatusCodePagesMiddleware` adicionando uma linha ao método `Configure`:</span><span class="sxs-lookup"><span data-stu-id="69789-127">You can configure the `StatusCodePagesMiddleware` by adding a line to the `Configure` method:</span></span>

```csharp
app.UseStatusCodePages();
```

<span data-ttu-id="69789-128">Por padrão, esse middleware adiciona manipuladores simples e somente texto a códigos de status comuns, como 404:</span><span class="sxs-lookup"><span data-stu-id="69789-128">By default, this middleware adds simple, text-only handlers for common status codes, such as 404:</span></span>

![Página 404](error-handling/_static/default-404-status-code.png)

<span data-ttu-id="69789-130">O middleware dá suporte a vários métodos de extensão diferentes.</span><span class="sxs-lookup"><span data-stu-id="69789-130">The middleware supports several different extension methods.</span></span> <span data-ttu-id="69789-131">Um usa uma expressão lambda e o outro usa uma cadeia de caracteres de formato e um tipo de conteúdo.</span><span class="sxs-lookup"><span data-stu-id="69789-131">One takes a lambda expression, another takes a content type and format string.</span></span>

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_StatusCodePages)]

```csharp
app.UseStatusCodePages("text/plain", "Status code page, status code: {0}");
```

<span data-ttu-id="69789-132">Também há métodos de extensão de redirecionamento.</span><span class="sxs-lookup"><span data-stu-id="69789-132">There are also redirect extension methods.</span></span> <span data-ttu-id="69789-133">Um envia um código de status 302 para o cliente e o outro retorna o código de status original para o cliente, mas também executa o manipulador para a URL de redirecionamento.</span><span class="sxs-lookup"><span data-stu-id="69789-133">One sends a 302 status code to the client, and one returns the original status code to the client but also executes the handler for the redirect URL.</span></span>

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

```csharp
app.UseStatusCodePagesWithReExecute("/error/{0}");
```

<span data-ttu-id="69789-134">Se precisar desabilitar páginas de código de status para determinadas solicitações, faça isso:</span><span class="sxs-lookup"><span data-stu-id="69789-134">If you need to disable status code pages for certain requests, you can do so:</span></span>

```csharp
var statusCodePagesFeature = context.Features.Get<IStatusCodePagesFeature>();
if (statusCodePagesFeature != null)
{
  statusCodePagesFeature.Enabled = false;
}
```

## <a name="exception-handling-code"></a><span data-ttu-id="69789-135">Código de tratamento de exceção</span><span class="sxs-lookup"><span data-stu-id="69789-135">Exception-handling code</span></span>

<span data-ttu-id="69789-136">Código em páginas de tratamento de exceção pode gerar exceções.</span><span class="sxs-lookup"><span data-stu-id="69789-136">Code in exception handling pages can throw exceptions.</span></span> <span data-ttu-id="69789-137">Geralmente, é uma boa ideia que páginas de erro de produção sejam compostas por conteúdo puramente estático.</span><span class="sxs-lookup"><span data-stu-id="69789-137">It's often a good idea for production error pages to consist of purely static content.</span></span>

<span data-ttu-id="69789-138">Além disso, esteja ciente de que, depois que os cabeçalhos de uma resposta forem enviados, você não poderá alterar o código de status da resposta e nenhuma página de exceção ou manipulador poderá ser executado.</span><span class="sxs-lookup"><span data-stu-id="69789-138">Also, be aware that once the headers for a response have been sent, you can't change the response's status code, nor can any exception pages or handlers run.</span></span> <span data-ttu-id="69789-139">A resposta deve ser concluída ou a conexão será anulada.</span><span class="sxs-lookup"><span data-stu-id="69789-139">The response must be completed or the connection aborted.</span></span>

## <a name="server-exception-handling"></a><span data-ttu-id="69789-140">Tratamento de exceções do servidor</span><span class="sxs-lookup"><span data-stu-id="69789-140">Server exception handling</span></span>

<span data-ttu-id="69789-141">Além da lógica de tratamento de exceção no aplicativo, o [servidor](servers/index.md) que hospeda o aplicativo executa uma parte do tratamento de exceção.</span><span class="sxs-lookup"><span data-stu-id="69789-141">In addition to the exception handling logic in your app, the [server](servers/index.md) hosting your app performs some exception handling.</span></span> <span data-ttu-id="69789-142">Se o servidor capturar uma exceção antes de os cabeçalhos serem enviados, o servidor enviará uma resposta 500 Erro Interno do Servidor sem corpo.</span><span class="sxs-lookup"><span data-stu-id="69789-142">If the server catches an exception before the headers are sent, the server sends a 500 Internal Server Error response with no body.</span></span> <span data-ttu-id="69789-143">Se o servidor capturar uma exceção depois que os cabeçalhos forem enviados, o servidor fechará a conexão.</span><span class="sxs-lookup"><span data-stu-id="69789-143">If the server catches an exception after the headers have been sent, the server closes the connection.</span></span> <span data-ttu-id="69789-144">As solicitações que não são manipuladas pelo aplicativo são manipuladas pelo servidor.</span><span class="sxs-lookup"><span data-stu-id="69789-144">Requests that aren't handled by your app are handled by the server.</span></span> <span data-ttu-id="69789-145">Qualquer exceção ocorrida é tratada pelo tratamento de exceção do servidor.</span><span class="sxs-lookup"><span data-stu-id="69789-145">Any exception that occurs is handled by the server's exception handling.</span></span> <span data-ttu-id="69789-146">As páginas de erro personalizadas, o middleware de tratamento de exceção ou os filtros configurados não afetam esse comportamento.</span><span class="sxs-lookup"><span data-stu-id="69789-146">Any configured custom error pages or exception handling middleware or filters don't affect this behavior.</span></span>

## <a name="startup-exception-handling"></a><span data-ttu-id="69789-147">Tratamento de exceção na inicialização</span><span class="sxs-lookup"><span data-stu-id="69789-147">Startup exception handling</span></span>

<span data-ttu-id="69789-148">Apenas a camada de hospedagem pode tratar exceções que ocorrem durante a inicialização do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="69789-148">Only the hosting layer can handle exceptions that take place during app startup.</span></span> <span data-ttu-id="69789-149">[Configure como o host se comporta em resposta a erros durante a inicialização](hosting.md#detailed-errors) usando `captureStartupErrors` e a chave `detailedErrors`.</span><span class="sxs-lookup"><span data-stu-id="69789-149">You can [configure how the host behaves in response to errors during startup](hosting.md#detailed-errors) using `captureStartupErrors` and the `detailedErrors` key.</span></span>

<span data-ttu-id="69789-150">A hospedagem apenas poderá mostrar uma página de erro para um erro de inicialização capturado se o erro ocorrer após a associação de endereço do host/porta.</span><span class="sxs-lookup"><span data-stu-id="69789-150">Hosting can only show an error page for a captured startup error if the error occurs after host address/port binding.</span></span> <span data-ttu-id="69789-151">Se uma associação falha por algum motivo, a camada de hospedagem registra uma exceção crítica em log, o processo dotnet falha e nenhuma página de erro é exibida.</span><span class="sxs-lookup"><span data-stu-id="69789-151">If any binding fails for any reason, the hosting layer logs a critical exception, the dotnet process crashes, and no error page is displayed.</span></span>

## <a name="aspnet-mvc-error-handling"></a><span data-ttu-id="69789-152">Tratamento de erro do ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="69789-152">ASP.NET MVC error handling</span></span>

<span data-ttu-id="69789-153">Os aplicativos [MVC](xref:mvc/overview) contêm algumas opções adicionais para o tratamento de erros, como configurar filtros de exceção e executar a validação do modelo.</span><span class="sxs-lookup"><span data-stu-id="69789-153">[MVC](xref:mvc/overview) apps have some additional options for handling errors, such as configuring exception filters and performing model validation.</span></span>

### <a name="exception-filters"></a><span data-ttu-id="69789-154">Filtros de exceção</span><span class="sxs-lookup"><span data-stu-id="69789-154">Exception Filters</span></span>

<span data-ttu-id="69789-155">Os filtros de exceção podem ser configurados globalmente ou por controlador ou por ação em um aplicativo MVC.</span><span class="sxs-lookup"><span data-stu-id="69789-155">Exception filters can be configured globally or on a per-controller or per-action basis in an MVC app.</span></span> <span data-ttu-id="69789-156">Esses filtros tratam qualquer exceção sem tratamento ocorrida durante a execução de uma ação do controlador ou de outro filtro e, caso contrário, não são chamadas.</span><span class="sxs-lookup"><span data-stu-id="69789-156">These filters handle any unhandled exception that occurs during the execution of a controller action or another filter, and are not called otherwise.</span></span> <span data-ttu-id="69789-157">Saiba mais sobre filtros de exceção em [Filtros](../mvc/controllers/filters.md).</span><span class="sxs-lookup"><span data-stu-id="69789-157">Learn more about exception filters in [Filters](../mvc/controllers/filters.md).</span></span>

>[!TIP]
> <span data-ttu-id="69789-158">Filtros de exceção são bons para interceptar exceções que ocorrem em ações do MVC, mas não são tão flexíveis quanto o middleware de tratamento de erro.</span><span class="sxs-lookup"><span data-stu-id="69789-158">Exception filters are good for trapping exceptions that occur within MVC actions, but they're not as flexible as error handling middleware.</span></span> <span data-ttu-id="69789-159">Dê preferência ao uso de middleware para o caso geral e use filtros apenas quando precisar fazer o tratamento de erro de modo *diferente*, dependendo da ação do MVC escolhida.</span><span class="sxs-lookup"><span data-stu-id="69789-159">Prefer middleware for the general case, and use filters only where you need to do error handling *differently* based on which MVC action was chosen.</span></span>

### <a name="handling-model-state-errors"></a><span data-ttu-id="69789-160">Tratando erros do estado do modelo</span><span class="sxs-lookup"><span data-stu-id="69789-160">Handling Model State Errors</span></span>

<span data-ttu-id="69789-161">A [validação do modelo](../mvc/models/validation.md) ocorre antes da invocação de cada ação do controlador e é responsabilidade do método de ação inspecionar `ModelState.IsValid` e responder de forma adequada.</span><span class="sxs-lookup"><span data-stu-id="69789-161">[Model validation](../mvc/models/validation.md) occurs prior to invoking each controller action, and it's the action method's responsibility to inspect `ModelState.IsValid` and react appropriately.</span></span>

<span data-ttu-id="69789-162">Alguns aplicativos optarão por seguir uma convenção padrão para lidar com erros de validação do modelo, caso em que um [filtro](../mvc/controllers/filters.md) pode ser um local adequado para implementar uma política como essa.</span><span class="sxs-lookup"><span data-stu-id="69789-162">Some apps will choose to follow a standard convention for dealing with model validation errors, in which case a [filter](../mvc/controllers/filters.md) may be an appropriate place to implement such a policy.</span></span> <span data-ttu-id="69789-163">É necessário testar o comportamento das ações com estados de modelo inválidos.</span><span class="sxs-lookup"><span data-stu-id="69789-163">You should test how your actions behave with invalid model states.</span></span> <span data-ttu-id="69789-164">Saiba mais em [Testando a lógica do controlador](../mvc/controllers/testing.md).</span><span class="sxs-lookup"><span data-stu-id="69789-164">Learn more in [Testing controller logic](../mvc/controllers/testing.md).</span></span>




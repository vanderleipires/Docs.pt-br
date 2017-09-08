---
title: "Tratamento de erro no núcleo do ASP.NET"
author: ardalis
description: Explica como manipular erros em aplicativos do ASP.NET Core
keywords: "ASP.NET Core, manipulação de exceções, tratamento de erros"
ms.author: tdykstra
manager: wpickett
ms.date: 11/30/2016
ms.topic: article
ms.assetid: 4db51023-c8a6-4119-bbbe-3917e272c260
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/error-handling
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 5898892c63e978adfabf9939394fef4ea1848d49
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/11/2017
---
# <a name="introduction-to-error-handling-in-aspnet-core"></a><span data-ttu-id="cea1e-104">Introdução ao ASP.NET Core de tratamento de erros</span><span class="sxs-lookup"><span data-stu-id="cea1e-104">Introduction to Error Handling in ASP.NET Core</span></span>

<span data-ttu-id="cea1e-105">Por [Steve Smith](http://ardalis.com) e [Tom Dykstra](https://github.com/tdykstra/)</span><span class="sxs-lookup"><span data-stu-id="cea1e-105">By [Steve Smith](http://ardalis.com) and [Tom Dykstra](https://github.com/tdykstra/)</span></span>

<span data-ttu-id="cea1e-106">Este artigo aborda appoaches comuns para tratamento de erros em aplicativos do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="cea1e-106">This article covers common appoaches to handling errors in ASP.NET Core apps.</span></span>

[<span data-ttu-id="cea1e-107">Exibir ou baixar o código de exemplo</span><span class="sxs-lookup"><span data-stu-id="cea1e-107">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/sample)

## <a name="the-developer-exception-page"></a><span data-ttu-id="cea1e-108">A página de exceção do desenvolvedor</span><span class="sxs-lookup"><span data-stu-id="cea1e-108">The developer exception page</span></span>

<span data-ttu-id="cea1e-109">Para configurar um aplicativo para exibir uma página que mostra informações detalhadas sobre exceções, instale o `Microsoft.AspNetCore.Diagnostics` NuGet do pacote e adicione uma linha para o [configurar método na classe de inicialização](startup.md):</span><span class="sxs-lookup"><span data-stu-id="cea1e-109">To configure an app to display a page that shows detailed information about exceptions, install the `Microsoft.AspNetCore.Diagnostics` NuGet package and add a line to the [Configure method in the Startup class](startup.md):</span></span>

<span data-ttu-id="cea1e-110">[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=7)]</span><span class="sxs-lookup"><span data-stu-id="cea1e-110">[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=7)]</span></span>

<span data-ttu-id="cea1e-111">Colocar `UseDeveloperExceptionPage` antes que qualquer middleware que você deseja capturar exceções, como `app.UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="cea1e-111">Put `UseDeveloperExceptionPage` before any middleware you want to catch exceptions in, such as `app.UseMvc`.</span></span>

>[!WARNING]
> <span data-ttu-id="cea1e-112">Habilitar a página de exceção de desenvolvedor **somente quando o aplicativo estiver em execução no ambiente de desenvolvimento**.</span><span class="sxs-lookup"><span data-stu-id="cea1e-112">Enable the developer exception page **only when the app is running in the Development environment**.</span></span> <span data-ttu-id="cea1e-113">Você não deseja compartilhar informações de exceção detalhadas publicamente quando o aplicativo for executado em produção.</span><span class="sxs-lookup"><span data-stu-id="cea1e-113">You don't want to share detailed exception information publicly when the app runs in production.</span></span> <span data-ttu-id="cea1e-114">[Saiba mais sobre como configurar ambientes](environments.md).</span><span class="sxs-lookup"><span data-stu-id="cea1e-114">[Learn more about configuring environments](environments.md).</span></span>

<span data-ttu-id="cea1e-115">Para ver a página de exceção de desenvolvedor, execute o aplicativo de exemplo com o ambiente definido como `Development`e adicione `?throw=true` para a URL base do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="cea1e-115">To see the developer exception page, run the sample application with the environment set to `Development`, and add `?throw=true` to the base URL of the app.</span></span> <span data-ttu-id="cea1e-116">A página inclui várias guias com informações sobre a exceção e a solicitação.</span><span class="sxs-lookup"><span data-stu-id="cea1e-116">The page includes several tabs with information about the exception and the request.</span></span> <span data-ttu-id="cea1e-117">A primeira guia inclui um rastreamento de pilha.</span><span class="sxs-lookup"><span data-stu-id="cea1e-117">The first tab includes a stack trace.</span></span> 

![Rastreamento de pilha](error-handling/_static/developer-exception-page.png)

<span data-ttu-id="cea1e-119">A próxima guia mostra a consulta parâmetros de cadeia de caracteres, se houver.</span><span class="sxs-lookup"><span data-stu-id="cea1e-119">The next tab shows the query string parameters, if any.</span></span>

![Parâmetros de cadeia de caracteres de consulta](error-handling/_static/developer-exception-page-query.png)

<span data-ttu-id="cea1e-121">Esta solicitação não tinha os cookies, mas se tivesse, apareceriam no **Cookies** guia.</span><span class="sxs-lookup"><span data-stu-id="cea1e-121">This request didn't have any cookies, but if it did, they would appear on the **Cookies** tab.</span></span> <span data-ttu-id="cea1e-122">Você pode ver os cabeçalhos que foram passados a última guia.</span><span class="sxs-lookup"><span data-stu-id="cea1e-122">You can see the headers that were passed in the last tab.</span></span>

![cabeçalhos](error-handling/_static/developer-exception-page-headers.png)

## <a name="configuring-a-custom-exception-handling-page"></a><span data-ttu-id="cea1e-124">Configurando uma página de tratamento de exceções personalizadas</span><span class="sxs-lookup"><span data-stu-id="cea1e-124">Configuring a custom exception handling page</span></span>

<span data-ttu-id="cea1e-125">É uma boa ideia para configurar uma página de manipulador de exceção a ser usado quando o aplicativo não está executando o `Development` ambiente.</span><span class="sxs-lookup"><span data-stu-id="cea1e-125">It's a good idea to configure an exception handler page to use when the app is not running in the `Development` environment.</span></span>

<span data-ttu-id="cea1e-126">[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=11)]</span><span class="sxs-lookup"><span data-stu-id="cea1e-126">[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=11)]</span></span>

<span data-ttu-id="cea1e-127">Em um aplicativo MVC, não explicitamente decorar o método de ação do manipulador de erro com os atributos de método HTTP, como `HttpGet`.</span><span class="sxs-lookup"><span data-stu-id="cea1e-127">In an MVC app, don't explicitly decorate the error handler action method with HTTP method attributes, such as `HttpGet`.</span></span> <span data-ttu-id="cea1e-128">Usando verbos explícitos pode impedir que algumas solicitações atinjam o método.</span><span class="sxs-lookup"><span data-stu-id="cea1e-128">Using explicit verbs could prevent some requests from reaching the method.</span></span>

```csharp
[Route("/Error")]
public IActionResult Index()
{
    // Handle error here
}
```

## <a name="configuring-status-code-pages"></a><span data-ttu-id="cea1e-129">Configurar páginas de código de status</span><span class="sxs-lookup"><span data-stu-id="cea1e-129">Configuring status code pages</span></span>

<span data-ttu-id="cea1e-130">Por padrão, seu aplicativo não fornecerá uma página de código de status detalhadas para códigos de status HTTP como 500 (erro interno do servidor) ou 404 (não encontrado).</span><span class="sxs-lookup"><span data-stu-id="cea1e-130">By default, your app will not provide a rich status code page for HTTP status codes such as 500 (Internal Server Error) or 404 (Not Found).</span></span> <span data-ttu-id="cea1e-131">Você pode configurar o `StatusCodePagesMiddleware` , adicionando uma linha para o `Configure` método:</span><span class="sxs-lookup"><span data-stu-id="cea1e-131">You can configure the `StatusCodePagesMiddleware` by adding a line to the `Configure` method:</span></span>

```csharp
app.UseStatusCodePages();
```

<span data-ttu-id="cea1e-132">Por padrão, este middleware adiciona manipuladores simples, somente de texto para os códigos de status comuns, como 404:</span><span class="sxs-lookup"><span data-stu-id="cea1e-132">By default, this middleware adds simple, text-only handlers for common status codes, such as 404:</span></span>

![página 404](error-handling/_static/default-404-status-code.png)

<span data-ttu-id="cea1e-134">O middleware oferece suporte a vários métodos de extensão diferente.</span><span class="sxs-lookup"><span data-stu-id="cea1e-134">The middleware supports several different extension methods.</span></span> <span data-ttu-id="cea1e-135">Um usa uma expressão lambda, outra usa uma cadeia de caracteres de formato e de tipo de conteúdo.</span><span class="sxs-lookup"><span data-stu-id="cea1e-135">One takes a lambda expression, another takes a content type and format string.</span></span>

<span data-ttu-id="cea1e-136">[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_StatusCodePages)]</span><span class="sxs-lookup"><span data-stu-id="cea1e-136">[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_StatusCodePages)]</span></span>

```csharp
app.UseStatusCodePages("text/plain", "Status code page, status code: {0}");
```

<span data-ttu-id="cea1e-137">Também há métodos de extensão de redirecionamento.</span><span class="sxs-lookup"><span data-stu-id="cea1e-137">There are also redirect extension methods.</span></span> <span data-ttu-id="cea1e-138">Um envia um código de 302 status para o cliente e um retorna o código de status original para o cliente, mas também executa o manipulador para a URL de redirecionamento.</span><span class="sxs-lookup"><span data-stu-id="cea1e-138">One sends a 302 status code to the client, and one returns the original status code to the client but also executes the handler for the redirect URL.</span></span>

<span data-ttu-id="cea1e-139">[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]</span><span class="sxs-lookup"><span data-stu-id="cea1e-139">[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]</span></span>

```csharp
app.UseStatusCodePagesWithReExecute("/error/{0}");
```

<span data-ttu-id="cea1e-140">Se você precisar desativar páginas de código de status para determinadas solicitações, você pode fazer isso:</span><span class="sxs-lookup"><span data-stu-id="cea1e-140">If you need to disable status code pages for certain requests, you can do so:</span></span>

```csharp
var statusCodePagesFeature = context.Features.Get<IStatusCodePagesFeature>();
if (statusCodePagesFeature != null)
{
  statusCodePagesFeature.Enabled = false;
}
```

## <a name="exception-handling-code"></a><span data-ttu-id="cea1e-141">Código de tratamento de exceção</span><span class="sxs-lookup"><span data-stu-id="cea1e-141">Exception-handling code</span></span>

<span data-ttu-id="cea1e-142">Código em páginas de tratamento de exceção pode gerar exceções.</span><span class="sxs-lookup"><span data-stu-id="cea1e-142">Code in exception handling pages can throw exceptions.</span></span> <span data-ttu-id="cea1e-143">Geralmente é uma boa ideia de páginas de erro de produção para consistem em conteúdo puramente estático.</span><span class="sxs-lookup"><span data-stu-id="cea1e-143">It's often a good idea for production error pages to consist of purely static content.</span></span>

<span data-ttu-id="cea1e-144">Além disso, lembre-se que, depois que os cabeçalhos de resposta forem enviados, não é possível alterar o código de status da resposta, nem podem executar de nenhuma página de exceção ou manipuladores.</span><span class="sxs-lookup"><span data-stu-id="cea1e-144">Also, be aware that once the headers for a response have been sent, you can't change the response's status code, nor can any exception pages or handlers run.</span></span> <span data-ttu-id="cea1e-145">A resposta deve ser concluída ou a conexão anulada.</span><span class="sxs-lookup"><span data-stu-id="cea1e-145">The response must be completed or the connection aborted.</span></span>

## <a name="server-exception-handling"></a><span data-ttu-id="cea1e-146">Tratamento de exceção de servidor</span><span class="sxs-lookup"><span data-stu-id="cea1e-146">Server exception handling</span></span>

<span data-ttu-id="cea1e-147">Além da lógica em seu aplicativo de tratamento de exceção de [server](servers/index.md) hospedar seu aplicativo irá realizar algumas tratamento de exceção.</span><span class="sxs-lookup"><span data-stu-id="cea1e-147">In addition to the exception handling logic in your app, the [server](servers/index.md) hosting your app will perform some exception handling.</span></span> <span data-ttu-id="cea1e-148">Se o servidor detecta uma exceção antes dos cabeçalhos enviados, ele enviará uma resposta de erro de servidor interno 500 sem corpo.</span><span class="sxs-lookup"><span data-stu-id="cea1e-148">If the server catches an exception before the headers have been sent it sends a 500 Internal Server Error response with no body.</span></span> <span data-ttu-id="cea1e-149">Se ele captura uma exceção após terem sido enviados os cabeçalhos, ele fecha a conexão.</span><span class="sxs-lookup"><span data-stu-id="cea1e-149">If it catches an exception after the headers have been sent, it closes the connection.</span></span> <span data-ttu-id="cea1e-150">Solicitações que não são manipuladas pelo seu aplicativo serão manipuladas pelo servidor e qualquer exceção que ocorre será tratada por exceção do servidor tratamento.</span><span class="sxs-lookup"><span data-stu-id="cea1e-150">Requests that are not handled by your app will be handled by the server, and any exception that occurs will be handled by the server's exception handling.</span></span> <span data-ttu-id="cea1e-151">Qualquer páginas de erro personalizadas ou middleware ou filtros que você configurou para seu aplicativo de tratamento de exceções não afetam esse comportamento.</span><span class="sxs-lookup"><span data-stu-id="cea1e-151">Any custom error pages or exception handling middleware or filters you have configured for your app will not affect this behavior.</span></span>

## <a name="startup-exception-handling"></a><span data-ttu-id="cea1e-152">Tratamento de exceções de inicialização</span><span class="sxs-lookup"><span data-stu-id="cea1e-152">Startup exception handling</span></span>

<span data-ttu-id="cea1e-153">Apenas a camada de hospedagem pode lidar com exceções que ocorrem durante a inicialização do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="cea1e-153">Only the hosting layer can handle exceptions that take place during app startup.</span></span> <span data-ttu-id="cea1e-154">Exceções que ocorrem durante a inicialização do aplicativo podem afetar o comportamento do servidor.</span><span class="sxs-lookup"><span data-stu-id="cea1e-154">Exceptions that occur during app startup can impact server behavior.</span></span> <span data-ttu-id="cea1e-155">Por exemplo, se ocorrer uma exceção antes de chamar `KestrelServerOptions.UseHttps`, a camada de hospedagem captura a exceção, o servidor é iniciado e exibe uma página de erro na porta não SSL.</span><span class="sxs-lookup"><span data-stu-id="cea1e-155">For example, if an exception happens before you call `KestrelServerOptions.UseHttps`, the hosting layer catches the exception, starts the server, and displays an error page on the non-SSL port.</span></span> <span data-ttu-id="cea1e-156">Se uma exceção ocorrer após a execução da linha, a página de erro é atendida por HTTPS.</span><span class="sxs-lookup"><span data-stu-id="cea1e-156">If an exception happens after that line executes, the error page is served over HTTPS instead.</span></span>

<span data-ttu-id="cea1e-157">Você pode [configurar como o host se comportará em resposta a erros durante a inicialização](hosting.md#configuring-a-host) usando `CaptureStartupErrors` e `detailedErrors` chave.</span><span class="sxs-lookup"><span data-stu-id="cea1e-157">You can [configure how the host will behave in response to errors during startup](hosting.md#configuring-a-host) using `CaptureStartupErrors` and the `detailedErrors` key.</span></span>

## <a name="aspnet-mvc-error-handling"></a><span data-ttu-id="cea1e-158">Tratamento de erros do ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="cea1e-158">ASP.NET MVC error handling</span></span>

<span data-ttu-id="cea1e-159">[MVC](../mvc/index.md) aplicativos tem algumas opções adicionais de manipulação de erros, como configurar filtros de exceção e executar a validação de modelo.</span><span class="sxs-lookup"><span data-stu-id="cea1e-159">[MVC](../mvc/index.md) apps have some additional options for handling errors, such as configuring exception filters and performing model validation.</span></span>

### <a name="exception-filters"></a><span data-ttu-id="cea1e-160">Filtros de exceção</span><span class="sxs-lookup"><span data-stu-id="cea1e-160">Exception Filters</span></span>

<span data-ttu-id="cea1e-161">Filtros de exceção podem ser configurados globalmente ou em uma base por controlador ou por ação em um aplicativo MVC.</span><span class="sxs-lookup"><span data-stu-id="cea1e-161">Exception filters can be configured globally or on a per-controller or per-action basis in an MVC app.</span></span> <span data-ttu-id="cea1e-162">Esses filtros lidar com qualquer exceção não tratada ocorre durante a execução de uma ação do controlador ou outro filtro e não são chamados caso contrário.</span><span class="sxs-lookup"><span data-stu-id="cea1e-162">These filters handle any unhandled exception that occurs during the execution of a controller action or another filter, and are not called otherwise.</span></span> <span data-ttu-id="cea1e-163">Saiba mais sobre filtros de exceção do [filtros](../mvc/controllers/filters.md).</span><span class="sxs-lookup"><span data-stu-id="cea1e-163">Learn more about exception filters in [Filters](../mvc/controllers/filters.md).</span></span>

>[!TIP]
> <span data-ttu-id="cea1e-164">Filtros de exceção são bons para interceptar exceções que ocorrem em ações do MVC, mas eles não são mais flexíveis middleware de tratamento de erros.</span><span class="sxs-lookup"><span data-stu-id="cea1e-164">Exception filters are good for trapping exceptions that occur within MVC actions, but they're not as flexible as error handling middleware.</span></span> <span data-ttu-id="cea1e-165">Preferir middleware para o caso geral e usar filtros apenas em que você precisa fazer o tratamento de erros *diferentemente* com base em qual ação MVC foi escolhida.</span><span class="sxs-lookup"><span data-stu-id="cea1e-165">Prefer middleware for the general case, and use filters only where you need to do error handling *differently* based on which MVC action was chosen.</span></span>

### <a name="handling-model-state-errors"></a><span data-ttu-id="cea1e-166">Estado do modelo de tratamento de erros</span><span class="sxs-lookup"><span data-stu-id="cea1e-166">Handling Model State Errors</span></span>

<span data-ttu-id="cea1e-167">[Validação de modelo](../mvc/models/validation.md) ocorre antes de cada ação de controlador que está sendo invocada, e é responsabilidade do método de ação para inspecionar `ModelState.IsValid` e reagir de forma adequada.</span><span class="sxs-lookup"><span data-stu-id="cea1e-167">[Model validation](../mvc/models/validation.md) occurs prior to each controller action being invoked, and it is the action method’s responsibility to inspect `ModelState.IsValid` and react appropriately.</span></span>

<span data-ttu-id="cea1e-168">Alguns aplicativos escolherá a seguir uma convenção padrão para lidar com erros de validação de modelo, caso em que um [filtro](../mvc/controllers/filters.md) pode ser um local adequado para implementar essa política.</span><span class="sxs-lookup"><span data-stu-id="cea1e-168">Some apps will choose to follow a standard convention for dealing with model validation errors, in which case a [filter](../mvc/controllers/filters.md) may be an appropriate place to implement such a policy.</span></span> <span data-ttu-id="cea1e-169">Você deve testar o comportam de suas ações com os estados de modelo inválido.</span><span class="sxs-lookup"><span data-stu-id="cea1e-169">You should test how your actions behave with invalid model states.</span></span> <span data-ttu-id="cea1e-170">Saiba mais em [lógica do controlador de teste](../mvc/controllers/testing.md).</span><span class="sxs-lookup"><span data-stu-id="cea1e-170">Learn more in [Testing controller logic](../mvc/controllers/testing.md).</span></span>




---
title: Tratamento de erros em ASP.NET Core
author: ardalis
description: Descobrir como tratar erros em aplicativos do ASP.NET Core.
keywords: "ASP.NET Core, tratamento de erros, manipulação de exceção"
ms.author: tdykstra
manager: wpickett
ms.date: 11/30/2016
ms.topic: article
ms.assetid: 4db51023-c8a6-4119-bbbe-3917e272c260
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/error-handling
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: de2ba0ff9ad17c198c06b510ecfb49f808721bdf
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
# <a name="introduction-to-error-handling-in-aspnet-core"></a><span data-ttu-id="dafbd-104">Introdução ao ASP.NET Core de tratamento de erros</span><span class="sxs-lookup"><span data-stu-id="dafbd-104">Introduction to Error Handling in ASP.NET Core</span></span>

<span data-ttu-id="dafbd-105">Por [Steve Smith](https://ardalis.com/) e [Tom Dykstra](https://github.com/tdykstra/)</span><span class="sxs-lookup"><span data-stu-id="dafbd-105">By [Steve Smith](https://ardalis.com/) and [Tom Dykstra](https://github.com/tdykstra/)</span></span>

<span data-ttu-id="dafbd-106">Este artigo aborda appoaches comuns para tratamento de erros em aplicativos do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="dafbd-106">This article covers common appoaches to handling errors in ASP.NET Core apps.</span></span>

<span data-ttu-id="dafbd-107">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="dafbd-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="the-developer-exception-page"></a><span data-ttu-id="dafbd-108">A página de exceção do desenvolvedor</span><span class="sxs-lookup"><span data-stu-id="dafbd-108">The developer exception page</span></span>

<span data-ttu-id="dafbd-109">Para configurar um aplicativo para exibir uma página que mostra informações detalhadas sobre exceções, instale o `Microsoft.AspNetCore.Diagnostics` NuGet do pacote e adicione uma linha para o [configurar método na classe de inicialização](startup.md):</span><span class="sxs-lookup"><span data-stu-id="dafbd-109">To configure an app to display a page that shows detailed information about exceptions, install the `Microsoft.AspNetCore.Diagnostics` NuGet package and add a line to the [Configure method in the Startup class](startup.md):</span></span>

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=7)]

<span data-ttu-id="dafbd-110">Colocar `UseDeveloperExceptionPage` antes que qualquer middleware que você deseja capturar exceções, como `app.UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="dafbd-110">Put `UseDeveloperExceptionPage` before any middleware you want to catch exceptions in, such as `app.UseMvc`.</span></span>

>[!WARNING]
> <span data-ttu-id="dafbd-111">Habilitar a página de exceção de desenvolvedor **somente quando o aplicativo estiver em execução no ambiente de desenvolvimento**.</span><span class="sxs-lookup"><span data-stu-id="dafbd-111">Enable the developer exception page **only when the app is running in the Development environment**.</span></span> <span data-ttu-id="dafbd-112">Você não deseja compartilhar informações de exceção detalhadas publicamente quando o aplicativo for executado em produção.</span><span class="sxs-lookup"><span data-stu-id="dafbd-112">You don't want to share detailed exception information publicly when the app runs in production.</span></span> <span data-ttu-id="dafbd-113">[Saiba mais sobre como configurar ambientes](environments.md).</span><span class="sxs-lookup"><span data-stu-id="dafbd-113">[Learn more about configuring environments](environments.md).</span></span>

<span data-ttu-id="dafbd-114">Para ver a página de exceção de desenvolvedor, execute o aplicativo de exemplo com o ambiente definido como `Development`e adicione `?throw=true` para a URL base do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="dafbd-114">To see the developer exception page, run the sample application with the environment set to `Development`, and add `?throw=true` to the base URL of the app.</span></span> <span data-ttu-id="dafbd-115">A página inclui várias guias com informações sobre a exceção e a solicitação.</span><span class="sxs-lookup"><span data-stu-id="dafbd-115">The page includes several tabs with information about the exception and the request.</span></span> <span data-ttu-id="dafbd-116">A primeira guia inclui um rastreamento de pilha.</span><span class="sxs-lookup"><span data-stu-id="dafbd-116">The first tab includes a stack trace.</span></span> 

![Rastreamento de pilha](error-handling/_static/developer-exception-page.png)

<span data-ttu-id="dafbd-118">A próxima guia mostra a consulta parâmetros de cadeia de caracteres, se houver.</span><span class="sxs-lookup"><span data-stu-id="dafbd-118">The next tab shows the query string parameters, if any.</span></span>

![Parâmetros de cadeia de caracteres de consulta](error-handling/_static/developer-exception-page-query.png)

<span data-ttu-id="dafbd-120">Esta solicitação não tinha os cookies, mas se tivesse, apareceriam no **Cookies** guia. Você pode ver os cabeçalhos que foram passados a última guia.</span><span class="sxs-lookup"><span data-stu-id="dafbd-120">This request didn't have any cookies, but if it did, they would appear on the **Cookies** tab. You can see the headers that were passed in the last tab.</span></span>

![cabeçalhos](error-handling/_static/developer-exception-page-headers.png)

## <a name="configuring-a-custom-exception-handling-page"></a><span data-ttu-id="dafbd-122">Configurando uma página de tratamento de exceções personalizadas</span><span class="sxs-lookup"><span data-stu-id="dafbd-122">Configuring a custom exception handling page</span></span>

<span data-ttu-id="dafbd-123">É uma boa ideia para configurar uma página de manipulador de exceção a ser usado quando o aplicativo não está executando o `Development` ambiente.</span><span class="sxs-lookup"><span data-stu-id="dafbd-123">It's a good idea to configure an exception handler page to use when the app is not running in the `Development` environment.</span></span>

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=11)]

<span data-ttu-id="dafbd-124">Em um aplicativo MVC, não explicitamente decorar o método de ação do manipulador de erro com os atributos de método HTTP, como `HttpGet`.</span><span class="sxs-lookup"><span data-stu-id="dafbd-124">In an MVC app, don't explicitly decorate the error handler action method with HTTP method attributes, such as `HttpGet`.</span></span> <span data-ttu-id="dafbd-125">Usando verbos explícitos pode impedir que algumas solicitações atinjam o método.</span><span class="sxs-lookup"><span data-stu-id="dafbd-125">Using explicit verbs could prevent some requests from reaching the method.</span></span>

```csharp
[Route("/Error")]
public IActionResult Index()
{
    // Handle error here
}
```

## <a name="configuring-status-code-pages"></a><span data-ttu-id="dafbd-126">Configurar páginas de código de status</span><span class="sxs-lookup"><span data-stu-id="dafbd-126">Configuring status code pages</span></span>

<span data-ttu-id="dafbd-127">Por padrão, seu aplicativo não fornecerá uma página de código de status detalhadas para códigos de status HTTP como 500 (erro interno do servidor) ou 404 (não encontrado).</span><span class="sxs-lookup"><span data-stu-id="dafbd-127">By default, your app will not provide a rich status code page for HTTP status codes such as 500 (Internal Server Error) or 404 (Not Found).</span></span> <span data-ttu-id="dafbd-128">Você pode configurar o `StatusCodePagesMiddleware` , adicionando uma linha para o `Configure` método:</span><span class="sxs-lookup"><span data-stu-id="dafbd-128">You can configure the `StatusCodePagesMiddleware` by adding a line to the `Configure` method:</span></span>

```csharp
app.UseStatusCodePages();
```

<span data-ttu-id="dafbd-129">Por padrão, este middleware adiciona manipuladores simples, somente de texto para os códigos de status comuns, como 404:</span><span class="sxs-lookup"><span data-stu-id="dafbd-129">By default, this middleware adds simple, text-only handlers for common status codes, such as 404:</span></span>

![página 404](error-handling/_static/default-404-status-code.png)

<span data-ttu-id="dafbd-131">O middleware oferece suporte a vários métodos de extensão diferente.</span><span class="sxs-lookup"><span data-stu-id="dafbd-131">The middleware supports several different extension methods.</span></span> <span data-ttu-id="dafbd-132">Um usa uma expressão lambda, outra usa uma cadeia de caracteres de formato e de tipo de conteúdo.</span><span class="sxs-lookup"><span data-stu-id="dafbd-132">One takes a lambda expression, another takes a content type and format string.</span></span>

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_StatusCodePages)]

```csharp
app.UseStatusCodePages("text/plain", "Status code page, status code: {0}");
```

<span data-ttu-id="dafbd-133">Também há métodos de extensão de redirecionamento.</span><span class="sxs-lookup"><span data-stu-id="dafbd-133">There are also redirect extension methods.</span></span> <span data-ttu-id="dafbd-134">Um envia um código de 302 status para o cliente e um retorna o código de status original para o cliente, mas também executa o manipulador para a URL de redirecionamento.</span><span class="sxs-lookup"><span data-stu-id="dafbd-134">One sends a 302 status code to the client, and one returns the original status code to the client but also executes the handler for the redirect URL.</span></span>

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

```csharp
app.UseStatusCodePagesWithReExecute("/error/{0}");
```

<span data-ttu-id="dafbd-135">Se você precisar desativar páginas de código de status para determinadas solicitações, você pode fazer isso:</span><span class="sxs-lookup"><span data-stu-id="dafbd-135">If you need to disable status code pages for certain requests, you can do so:</span></span>

```csharp
var statusCodePagesFeature = context.Features.Get<IStatusCodePagesFeature>();
if (statusCodePagesFeature != null)
{
  statusCodePagesFeature.Enabled = false;
}
```

## <a name="exception-handling-code"></a><span data-ttu-id="dafbd-136">Código de tratamento de exceção</span><span class="sxs-lookup"><span data-stu-id="dafbd-136">Exception-handling code</span></span>

<span data-ttu-id="dafbd-137">Código em páginas de tratamento de exceção pode gerar exceções.</span><span class="sxs-lookup"><span data-stu-id="dafbd-137">Code in exception handling pages can throw exceptions.</span></span> <span data-ttu-id="dafbd-138">Geralmente é uma boa ideia de páginas de erro de produção para consistem em conteúdo puramente estático.</span><span class="sxs-lookup"><span data-stu-id="dafbd-138">It's often a good idea for production error pages to consist of purely static content.</span></span>

<span data-ttu-id="dafbd-139">Além disso, lembre-se que, depois que os cabeçalhos de resposta forem enviados, não é possível alterar o código de status da resposta, nem podem executar de nenhuma página de exceção ou manipuladores.</span><span class="sxs-lookup"><span data-stu-id="dafbd-139">Also, be aware that once the headers for a response have been sent, you can't change the response's status code, nor can any exception pages or handlers run.</span></span> <span data-ttu-id="dafbd-140">A resposta deve ser concluída ou a conexão anulada.</span><span class="sxs-lookup"><span data-stu-id="dafbd-140">The response must be completed or the connection aborted.</span></span>

## <a name="server-exception-handling"></a><span data-ttu-id="dafbd-141">Tratamento de exceção de servidor</span><span class="sxs-lookup"><span data-stu-id="dafbd-141">Server exception handling</span></span>

<span data-ttu-id="dafbd-142">Além da lógica em seu aplicativo de tratamento de exceção de [server](servers/index.md) hospedar seu aplicativo executa algumas tratamento de exceção.</span><span class="sxs-lookup"><span data-stu-id="dafbd-142">In addition to the exception handling logic in your app, the [server](servers/index.md) hosting your app performs some exception handling.</span></span> <span data-ttu-id="dafbd-143">Se o servidor detecta uma exceção antes de serem enviados os cabeçalhos, o servidor envia uma resposta de erro de servidor interno 500 sem corpo.</span><span class="sxs-lookup"><span data-stu-id="dafbd-143">If the server catches an exception before the headers are sent, the server sends a 500 Internal Server Error response with no body.</span></span> <span data-ttu-id="dafbd-144">Se o servidor detecta uma exceção após terem sido enviados os cabeçalhos, o servidor fecha a conexão.</span><span class="sxs-lookup"><span data-stu-id="dafbd-144">If the server catches an exception after the headers have been sent, the server closes the connection.</span></span> <span data-ttu-id="dafbd-145">Solicitações que não são manipuladas pelo seu aplicativo são manipuladas pelo servidor.</span><span class="sxs-lookup"><span data-stu-id="dafbd-145">Requests that aren't handled by your app are handled by the server.</span></span> <span data-ttu-id="dafbd-146">Qualquer exceção que ocorre é tratada pela exceção do servidor tratamento.</span><span class="sxs-lookup"><span data-stu-id="dafbd-146">Any exception that occurs is handled by the server's exception handling.</span></span> <span data-ttu-id="dafbd-147">Algum configurado páginas de erro personalizadas ou middleware de tratamento de exceção ou filtros não afetam esse comportamento.</span><span class="sxs-lookup"><span data-stu-id="dafbd-147">Any configured custom error pages or exception handling middleware or filters don't affect this behavior.</span></span>

## <a name="startup-exception-handling"></a><span data-ttu-id="dafbd-148">Tratamento de exceções de inicialização</span><span class="sxs-lookup"><span data-stu-id="dafbd-148">Startup exception handling</span></span>

<span data-ttu-id="dafbd-149">Apenas a camada de hospedagem pode lidar com exceções que ocorrem durante a inicialização do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="dafbd-149">Only the hosting layer can handle exceptions that take place during app startup.</span></span> <span data-ttu-id="dafbd-150">Você pode [configurar como o host se comporta em resposta a erros durante a inicialização](hosting.md#detailed-errors) usando `captureStartupErrors` e `detailedErrors` chave.</span><span class="sxs-lookup"><span data-stu-id="dafbd-150">You can [configure how the host behaves in response to errors during startup](hosting.md#detailed-errors) using `captureStartupErrors` and the `detailedErrors` key.</span></span>

<span data-ttu-id="dafbd-151">Hospedagem só pode mostrar uma página de erro para um erro de inicialização capturada se o erro ocorrer após o endereço de host/associação de porta.</span><span class="sxs-lookup"><span data-stu-id="dafbd-151">Hosting can only show an error page for a captured startup error if the error occurs after host address/port binding.</span></span> <span data-ttu-id="dafbd-152">Se qualquer associação falhar por algum motivo, a camada de hospedagem registra uma exceção crítica, o travamento do processo dotnet, e nenhuma página de erro é exibida.</span><span class="sxs-lookup"><span data-stu-id="dafbd-152">If any binding fails for any reason, the hosting layer logs a critical exception, the dotnet process crashes, and no error page is displayed.</span></span>

## <a name="aspnet-mvc-error-handling"></a><span data-ttu-id="dafbd-153">Tratamento de erros do ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="dafbd-153">ASP.NET MVC error handling</span></span>

<span data-ttu-id="dafbd-154">[MVC](../mvc/index.md) aplicativos tem algumas opções adicionais de manipulação de erros, como configurar filtros de exceção e executar a validação de modelo.</span><span class="sxs-lookup"><span data-stu-id="dafbd-154">[MVC](../mvc/index.md) apps have some additional options for handling errors, such as configuring exception filters and performing model validation.</span></span>

### <a name="exception-filters"></a><span data-ttu-id="dafbd-155">Filtros de exceção</span><span class="sxs-lookup"><span data-stu-id="dafbd-155">Exception Filters</span></span>

<span data-ttu-id="dafbd-156">Filtros de exceção podem ser configurados globalmente ou em uma base por controlador ou por ação em um aplicativo MVC.</span><span class="sxs-lookup"><span data-stu-id="dafbd-156">Exception filters can be configured globally or on a per-controller or per-action basis in an MVC app.</span></span> <span data-ttu-id="dafbd-157">Esses filtros lidar com qualquer exceção não tratada ocorre durante a execução de uma ação do controlador ou outro filtro e não são chamados caso contrário.</span><span class="sxs-lookup"><span data-stu-id="dafbd-157">These filters handle any unhandled exception that occurs during the execution of a controller action or another filter, and are not called otherwise.</span></span> <span data-ttu-id="dafbd-158">Saiba mais sobre filtros de exceção do [filtros](../mvc/controllers/filters.md).</span><span class="sxs-lookup"><span data-stu-id="dafbd-158">Learn more about exception filters in [Filters](../mvc/controllers/filters.md).</span></span>

>[!TIP]
> <span data-ttu-id="dafbd-159">Filtros de exceção são bons para interceptar exceções que ocorrem em ações do MVC, mas eles não são mais flexíveis middleware de tratamento de erros.</span><span class="sxs-lookup"><span data-stu-id="dafbd-159">Exception filters are good for trapping exceptions that occur within MVC actions, but they're not as flexible as error handling middleware.</span></span> <span data-ttu-id="dafbd-160">Preferir middleware para o caso geral e usar filtros apenas em que você precisa fazer o tratamento de erros *diferentemente* com base em qual ação MVC foi escolhida.</span><span class="sxs-lookup"><span data-stu-id="dafbd-160">Prefer middleware for the general case, and use filters only where you need to do error handling *differently* based on which MVC action was chosen.</span></span>

### <a name="handling-model-state-errors"></a><span data-ttu-id="dafbd-161">Estado do modelo de tratamento de erros</span><span class="sxs-lookup"><span data-stu-id="dafbd-161">Handling Model State Errors</span></span>

<span data-ttu-id="dafbd-162">[Validação de modelo](../mvc/models/validation.md) ocorre antes de cada ação de controlador que está sendo invocada, e é responsabilidade do método de ação para inspecionar `ModelState.IsValid` e reagir de forma adequada.</span><span class="sxs-lookup"><span data-stu-id="dafbd-162">[Model validation](../mvc/models/validation.md) occurs prior to each controller action being invoked, and it is the action method’s responsibility to inspect `ModelState.IsValid` and react appropriately.</span></span>

<span data-ttu-id="dafbd-163">Alguns aplicativos escolherá a seguir uma convenção padrão para lidar com erros de validação de modelo, caso em que um [filtro](../mvc/controllers/filters.md) pode ser um local adequado para implementar essa política.</span><span class="sxs-lookup"><span data-stu-id="dafbd-163">Some apps will choose to follow a standard convention for dealing with model validation errors, in which case a [filter](../mvc/controllers/filters.md) may be an appropriate place to implement such a policy.</span></span> <span data-ttu-id="dafbd-164">Você deve testar o comportam de suas ações com os estados de modelo inválido.</span><span class="sxs-lookup"><span data-stu-id="dafbd-164">You should test how your actions behave with invalid model states.</span></span> <span data-ttu-id="dafbd-165">Saiba mais em [lógica do controlador de teste](../mvc/controllers/testing.md).</span><span class="sxs-lookup"><span data-stu-id="dafbd-165">Learn more in [Testing controller logic](../mvc/controllers/testing.md).</span></span>




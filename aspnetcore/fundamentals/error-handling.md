---
title: Tratar erros no ASP.NET Core
author: ardalis
description: Descubra como tratar erros em aplicativos ASP.NET Core.
ms.author: tdykstra
ms.custom: H1Hack27Feb2017
ms.date: 11/30/2016
uid: fundamentals/error-handling
ms.openlocfilehash: 2fe46ecc32d61a7fafb2ad6e2a35456476608251
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36273703"
---
# <a name="handle-errors-in-aspnet-core"></a><span data-ttu-id="d6220-103">Tratar erros no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d6220-103">Handle errors in ASP.NET Core</span></span>

<span data-ttu-id="d6220-104">Por [Steve Smith](https://ardalis.com/) e [Tom Dykstra](https://github.com/tdykstra/)</span><span class="sxs-lookup"><span data-stu-id="d6220-104">By [Steve Smith](https://ardalis.com/) and [Tom Dykstra](https://github.com/tdykstra/)</span></span>

<span data-ttu-id="d6220-105">Este artigo apresenta abordagens comuns para o tratamento de erros em aplicativos ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d6220-105">This article covers common appoaches to handling errors in ASP.NET Core apps.</span></span>

<span data-ttu-id="d6220-106">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d6220-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="the-developer-exception-page"></a><span data-ttu-id="d6220-107">A página de exceção do desenvolvedor</span><span class="sxs-lookup"><span data-stu-id="d6220-107">The developer exception page</span></span>

<span data-ttu-id="d6220-108">Para configurar um aplicativo para exibir uma página que mostra informações detalhadas sobre exceções, instale o pacote NuGet `Microsoft.AspNetCore.Diagnostics` e adicione uma linha ao [método Configure na classe Startup](xref:fundamentals/startup):</span><span class="sxs-lookup"><span data-stu-id="d6220-108">To configure an app to display a page that shows detailed information about exceptions, install the `Microsoft.AspNetCore.Diagnostics` NuGet package and add a line to the [Configure method in the Startup class](xref:fundamentals/startup):</span></span>

[!code-csharp[](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=7)]

<span data-ttu-id="d6220-109">Coloque `UseDeveloperExceptionPage` antes de qualquer middleware no qual você deseja capturar exceções, como `app.UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="d6220-109">Put `UseDeveloperExceptionPage` before any middleware you want to catch exceptions in, such as `app.UseMvc`.</span></span>

>[!WARNING]
> <span data-ttu-id="d6220-110">Habilite a página de exceção do desenvolvedor **somente quando o aplicativo estiver em execução no ambiente de Desenvolvimento**.</span><span class="sxs-lookup"><span data-stu-id="d6220-110">Enable the developer exception page **only when the app is running in the Development environment**.</span></span> <span data-ttu-id="d6220-111">Não é recomendável compartilhar informações de exceção detalhadas publicamente quando o aplicativo é executado em produção.</span><span class="sxs-lookup"><span data-stu-id="d6220-111">You don't want to share detailed exception information publicly when the app runs in production.</span></span> <span data-ttu-id="d6220-112">[Saiba mais sobre como configurar ambientes](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="d6220-112">[Learn more about configuring environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="d6220-113">Para ver a página de exceção do desenvolvedor, execute o aplicativo de exemplo com o ambiente definido como `Development` e adicione `?throw=true` à URL base do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="d6220-113">To see the developer exception page, run the sample application with the environment set to `Development`, and add `?throw=true` to the base URL of the app.</span></span> <span data-ttu-id="d6220-114">A página inclui várias guias com informações sobre a exceção e a solicitação.</span><span class="sxs-lookup"><span data-stu-id="d6220-114">The page includes several tabs with information about the exception and the request.</span></span> <span data-ttu-id="d6220-115">A primeira guia inclui um rastreamento de pilha.</span><span class="sxs-lookup"><span data-stu-id="d6220-115">The first tab includes a stack trace.</span></span> 

![Rastreamento de pilha](error-handling/_static/developer-exception-page.png)

<span data-ttu-id="d6220-117">A próxima guia mostra os parâmetros da cadeia de caracteres de consulta, se houver.</span><span class="sxs-lookup"><span data-stu-id="d6220-117">The next tab shows the query string parameters, if any.</span></span>

![Parâmetros de cadeia de caracteres de consulta](error-handling/_static/developer-exception-page-query.png)

<span data-ttu-id="d6220-119">Essa solicitação não tinha nenhum cookie, mas se tivesse, eles seriam exibidos na guia **Cookies**. Veja os cabeçalhos que foram passados na última guia.</span><span class="sxs-lookup"><span data-stu-id="d6220-119">This request didn't have any cookies, but if it did, they would appear on the **Cookies** tab. You can see the headers that were passed in the last tab.</span></span>

![Cabeçalhos](error-handling/_static/developer-exception-page-headers.png)

## <a name="configuring-a-custom-exception-handling-page"></a><span data-ttu-id="d6220-121">Configurando uma página de tratamento de exceção personalizada</span><span class="sxs-lookup"><span data-stu-id="d6220-121">Configuring a custom exception handling page</span></span>

<span data-ttu-id="d6220-122">Configure uma página do manipulador de exceção para usar quando o aplicativo não estiver em execução no ambiente `Development`.</span><span class="sxs-lookup"><span data-stu-id="d6220-122">Configure an exception handler page to use when the app isn't running in the `Development` environment.</span></span>

[!code-csharp[](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=11)]

<span data-ttu-id="d6220-123">Em um aplicativo Razor Pages, o modelo [dotnet novo](/dotnet/core/tools/dotnet-new) do Razor Pages fornece uma de erro e a classe de modelo de página `ErrorModel` na pasta *Páginas*.</span><span class="sxs-lookup"><span data-stu-id="d6220-123">In a Razor Pages app, the [dotnet new](/dotnet/core/tools/dotnet-new) Razor Pages template provides an Error page and `ErrorModel` page model class in the *Pages* folder.</span></span>

<span data-ttu-id="d6220-124">Em um aplicativo MVC, não decore o método de ação do manipulador de erro com atributos de método HTTP, como `HttpGet`.</span><span class="sxs-lookup"><span data-stu-id="d6220-124">In an MVC app, don't decorate the error handler action method with HTTP method attributes, such as `HttpGet`.</span></span> <span data-ttu-id="d6220-125">Verbos explícitos impedem algumas solicitações de chegar ao método.</span><span class="sxs-lookup"><span data-stu-id="d6220-125">Explicit verbs prevent some requests from reaching the method.</span></span> <span data-ttu-id="d6220-126">Permita acesso anônimo ao método para que os usuários não autenticados possam capazes receber a exibição de erro.</span><span class="sxs-lookup"><span data-stu-id="d6220-126">Allow anonymous access to the method so that unauthenticated users are able to receive the error view.</span></span>

<span data-ttu-id="d6220-127">Por exemplo, o seguinte método de manipulador de erro é fornecido pelo modelo MVC [dotnet novo](/dotnet/core/tools/dotnet-new) e aparece no controlador Home:</span><span class="sxs-lookup"><span data-stu-id="d6220-127">For example, the following error handler method is provided by the [dotnet new](/dotnet/core/tools/dotnet-new) MVC template and appears in the Home controller:</span></span>

```csharp
[AllowAnonymous]
public IActionResult Error()
{
    return View(new ErrorViewModel { RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier });
}
```

## <a name="configuring-status-code-pages"></a><span data-ttu-id="d6220-128">Configurando páginas de código de status</span><span class="sxs-lookup"><span data-stu-id="d6220-128">Configuring status code pages</span></span>

<span data-ttu-id="d6220-129">Por padrão, o aplicativo não fornece uma página de código de status detalhada para códigos de status HTTP, como *404 Não Encontrado*.</span><span class="sxs-lookup"><span data-stu-id="d6220-129">By default, an app doesn't provide a rich status code page for HTTP status codes, such as *404 Not Found*.</span></span> <span data-ttu-id="d6220-130">Para fornecer páginas de código de status, configure o middleware de páginas de código de status, adicionando uma linha ao método `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="d6220-130">To provide status code pages, configure the Status Code Pages Middleware by adding a line to the `Startup.Configure` method:</span></span>

```csharp
app.UseStatusCodePages();
```

<span data-ttu-id="d6220-131">Por padrão, o middleware de páginas de código de status adiciona manipuladores simples, somente de texto, para os códigos de status comuns, como o 404:</span><span class="sxs-lookup"><span data-stu-id="d6220-131">By default, Status Code Pages Middleware adds simple, text-only handlers for common status codes, such as 404:</span></span>

![Página 404](error-handling/_static/default-404-status-code.png)

<span data-ttu-id="d6220-133">O middleware permite vários métodos de extensão.</span><span class="sxs-lookup"><span data-stu-id="d6220-133">The middleware supports several extension methods.</span></span> <span data-ttu-id="d6220-134">Um método usa uma expressão lambda:</span><span class="sxs-lookup"><span data-stu-id="d6220-134">One method takes a lambda expression:</span></span>

[!code-csharp[](error-handling/sample/Startup.cs?name=snippet_StatusCodePages)]

<span data-ttu-id="d6220-135">Outro método usa uma cadeia de caracteres de formato e de tipo de conteúdo:</span><span class="sxs-lookup"><span data-stu-id="d6220-135">Another method takes a content type and format string:</span></span>

```csharp
app.UseStatusCodePages("text/plain", "Status code page, status code: {0}");
```

<span data-ttu-id="d6220-136">Há também métodos de extensão de redirecionamento e de nova execução.</span><span class="sxs-lookup"><span data-stu-id="d6220-136">There are also redirect and re-execute extension methods.</span></span> <span data-ttu-id="d6220-137">O método de redirecionamento envia um código de status 302 para o cliente:</span><span class="sxs-lookup"><span data-stu-id="d6220-137">The redirect method sends a 302 status code to the client:</span></span>

[!code-csharp[](error-handling/sample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

<span data-ttu-id="d6220-138">O método de nova execução retorna o código de status original para o cliente, mas também executa o manipulador para a URL de redirecionamento:</span><span class="sxs-lookup"><span data-stu-id="d6220-138">The re-execute method returns the original status code to the client but also executes the handler for the redirect URL:</span></span>

```csharp
app.UseStatusCodePagesWithReExecute("/error/{0}");
```

<span data-ttu-id="d6220-139">As páginas de código de status podem ser desabilitadas para solicitações específicas em um método de manipulador de Páginas Razor ou em um controlador do MVC.</span><span class="sxs-lookup"><span data-stu-id="d6220-139">Status code pages can be disabled for specific requests in a Razor Pages handler method or in an MVC controller.</span></span> <span data-ttu-id="d6220-140">Para desabilitar as páginas de código de status, tente recuperar o [IStatusCodePagesFeature](/dotnet/api/microsoft.aspnetcore.diagnostics.istatuscodepagesfeature) da coleção [HttpContext.Features](/dotnet/api/microsoft.aspnetcore.http.httpcontext.features) da solicitação e desabilitar o recurso se ele estiver disponível:</span><span class="sxs-lookup"><span data-stu-id="d6220-140">To disable status code pages, attempt to retrieve the [IStatusCodePagesFeature](/dotnet/api/microsoft.aspnetcore.diagnostics.istatuscodepagesfeature) from the request's [HttpContext.Features](/dotnet/api/microsoft.aspnetcore.http.httpcontext.features) collection and disable the feature if it's available:</span></span>

```csharp
var statusCodePagesFeature = HttpContext.Features.Get<IStatusCodePagesFeature>();

if (statusCodePagesFeature != null)
{
    statusCodePagesFeature.Enabled = false;
}
```

<span data-ttu-id="d6220-141">Se você está usando uma sobrecarga `UseStatusCodePages*` que aponta para um ponto de extremidade dentro do aplicativo, crie um modo de exibição do MVC ou Razor Page para o ponto de extremidade.</span><span class="sxs-lookup"><span data-stu-id="d6220-141">If using a `UseStatusCodePages*` overload that points to an endpoint within the app, create an MVC view or Razor Page for the endpoint.</span></span> <span data-ttu-id="d6220-142">Por exemplo, o modelo [dotnet novo](/dotnet/core/tools/dotnet-new) para um aplicativo Razor Pages produz a seguinte página e classe de modelo de página:</span><span class="sxs-lookup"><span data-stu-id="d6220-142">For example, the [dotnet new](/dotnet/core/tools/dotnet-new) template for a Razor Pages app produces the following page and page model class:</span></span>

<span data-ttu-id="d6220-143">*Error.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="d6220-143">*Error.cshtml*:</span></span>

```cshtml
@page
@model ErrorModel
@{
    ViewData["Title"] = "Error";
}

<h1 class="text-danger">Error.</h1>
<h2 class="text-danger">An error occurred while processing your request.</h2>

@if (Model.ShowRequestId)
{
    <p>
        <strong>Request ID:</strong> <code>@Model.RequestId</code>
    </p>
}

<h3>Development Mode</h3>
<p>
    Swapping to <strong>Development</strong> environment will display more detailed information about the error that occurred.
</p>
<p>
    <strong>Development environment should not be enabled in deployed applications</strong>, as it can result in sensitive information from exceptions being displayed to end users. For local debugging, development environment can be enabled by setting the <strong>ASPNETCORE_ENVIRONMENT</strong> environment variable to <strong>Development</strong>, and restarting the application.
</p>
```

<span data-ttu-id="d6220-144">*Error.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="d6220-144">*Error.cshtml.cs*:</span></span>

```csharp
public class ErrorModel : PageModel
{
    public string RequestId { get; set; }

    public bool ShowRequestId => !string.IsNullOrEmpty(RequestId);

    [ResponseCache(Duration = 0, Location = ResponseCacheLocation.None, NoStore = true)]
    public void OnGet()
    {
        RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier;
    }
}
```

## <a name="exception-handling-code"></a><span data-ttu-id="d6220-145">Código de tratamento de exceção</span><span class="sxs-lookup"><span data-stu-id="d6220-145">Exception-handling code</span></span>

<span data-ttu-id="d6220-146">Código em páginas de tratamento de exceção pode gerar exceções.</span><span class="sxs-lookup"><span data-stu-id="d6220-146">Code in exception handling pages can throw exceptions.</span></span> <span data-ttu-id="d6220-147">Geralmente, é uma boa ideia que páginas de erro de produção sejam compostas por conteúdo puramente estático.</span><span class="sxs-lookup"><span data-stu-id="d6220-147">It's often a good idea for production error pages to consist of purely static content.</span></span>

<span data-ttu-id="d6220-148">Além disso, esteja ciente de que, depois que os cabeçalhos de uma resposta forem enviados, você não poderá alterar o código de status da resposta e nenhuma página de exceção ou manipulador poderá ser executado.</span><span class="sxs-lookup"><span data-stu-id="d6220-148">Also, be aware that once the headers for a response have been sent, you can't change the response's status code, nor can any exception pages or handlers run.</span></span> <span data-ttu-id="d6220-149">A resposta deve ser concluída ou a conexão será anulada.</span><span class="sxs-lookup"><span data-stu-id="d6220-149">The response must be completed or the connection aborted.</span></span>

## <a name="server-exception-handling"></a><span data-ttu-id="d6220-150">Tratamento de exceções do servidor</span><span class="sxs-lookup"><span data-stu-id="d6220-150">Server exception handling</span></span>

<span data-ttu-id="d6220-151">Além da lógica de tratamento de exceção no aplicativo, o [servidor](xref:fundamentals/servers/index) que hospeda o aplicativo executa uma parte do tratamento de exceção.</span><span class="sxs-lookup"><span data-stu-id="d6220-151">In addition to the exception handling logic in your app, the [server](xref:fundamentals/servers/index) hosting your app performs some exception handling.</span></span> <span data-ttu-id="d6220-152">Se o servidor capturar uma exceção antes que os cabeçalhos sejam enviados, o servidor enviará uma resposta *500 Erro Interno do Servidor* sem corpo.</span><span class="sxs-lookup"><span data-stu-id="d6220-152">If the server catches an exception before the headers are sent, the server sends a *500 Internal Server Error* response with no body.</span></span> <span data-ttu-id="d6220-153">Se o servidor capturar uma exceção depois que os cabeçalhos forem enviados, o servidor fechará a conexão.</span><span class="sxs-lookup"><span data-stu-id="d6220-153">If the server catches an exception after the headers have been sent, the server closes the connection.</span></span> <span data-ttu-id="d6220-154">As solicitações que não são manipuladas pelo aplicativo são manipuladas pelo servidor.</span><span class="sxs-lookup"><span data-stu-id="d6220-154">Requests that aren't handled by your app are handled by the server.</span></span> <span data-ttu-id="d6220-155">Qualquer exceção ocorrida é tratada pelo tratamento de exceção do servidor.</span><span class="sxs-lookup"><span data-stu-id="d6220-155">Any exception that occurs is handled by the server's exception handling.</span></span> <span data-ttu-id="d6220-156">As páginas de erro personalizadas, o middleware de tratamento de exceção ou os filtros configurados não afetam esse comportamento.</span><span class="sxs-lookup"><span data-stu-id="d6220-156">Any configured custom error pages or exception handling middleware or filters don't affect this behavior.</span></span>

## <a name="startup-exception-handling"></a><span data-ttu-id="d6220-157">Tratamento de exceção na inicialização</span><span class="sxs-lookup"><span data-stu-id="d6220-157">Startup exception handling</span></span>

<span data-ttu-id="d6220-158">Apenas a camada de hospedagem pode tratar exceções que ocorrem durante a inicialização do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="d6220-158">Only the hosting layer can handle exceptions that take place during app startup.</span></span> <span data-ttu-id="d6220-159">Usando o [Host da Web](xref:fundamentals/host/web-host), você pode [configurar como o host se comporta em resposta a erros durante a inicialização](xref:fundamentals/host/web-host#detailed-errors) usando as chaves `captureStartupErrors` e `detailedErrors`.</span><span class="sxs-lookup"><span data-stu-id="d6220-159">Using the [Web Host](xref:fundamentals/host/web-host), you can [configure how the host behaves in response to errors during startup](xref:fundamentals/host/web-host#detailed-errors) with the `captureStartupErrors` and `detailedErrors` keys.</span></span>

<span data-ttu-id="d6220-160">A hospedagem apenas poderá mostrar uma página de erro para um erro de inicialização capturado se o erro ocorrer após a associação de endereço do host/porta.</span><span class="sxs-lookup"><span data-stu-id="d6220-160">Hosting can only show an error page for a captured startup error if the error occurs after host address/port binding.</span></span> <span data-ttu-id="d6220-161">Se alguma associação falhar por algum motivo, a camada de hospedagem registrará uma exceção crítica em log, o processo do dotnet falhará e nenhuma página de erro será exibida quando o aplicativo estiver sendo executado no servidor [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="d6220-161">If any binding fails for any reason, the hosting layer logs a critical exception, the dotnet process crashes, and no error page is displayed when the app is running on the [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span>

<span data-ttu-id="d6220-162">Quando executado no [IIS](/iis) ou no [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), um *502.5 Falha no Processo* será retornado pelo [Módulo do ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) se o processo não puder ser iniciado.</span><span class="sxs-lookup"><span data-stu-id="d6220-162">When running on [IIS](/iis) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), a *502.5 Process Failure* is returned by the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) if the process can't be started.</span></span> <span data-ttu-id="d6220-163">Siga os avisos de solução de problemas no tópico [Solucionar problemas do ASP.NET Core no IIS](xref:host-and-deploy/iis/troubleshoot).</span><span class="sxs-lookup"><span data-stu-id="d6220-163">Follow the troubleshooting advice in the [Troubleshoot ASP.NET Core on IIS](xref:host-and-deploy/iis/troubleshoot) topic.</span></span>

## <a name="aspnet-mvc-error-handling"></a><span data-ttu-id="d6220-164">Tratamento de erro do ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="d6220-164">ASP.NET MVC error handling</span></span>

<span data-ttu-id="d6220-165">Os aplicativos [MVC](xref:mvc/overview) contêm algumas opções adicionais para o tratamento de erros, como configurar filtros de exceção e executar a validação do modelo.</span><span class="sxs-lookup"><span data-stu-id="d6220-165">[MVC](xref:mvc/overview) apps have some additional options for handling errors, such as configuring exception filters and performing model validation.</span></span>

### <a name="exception-filters"></a><span data-ttu-id="d6220-166">Filtros de exceção</span><span class="sxs-lookup"><span data-stu-id="d6220-166">Exception Filters</span></span>

<span data-ttu-id="d6220-167">Os filtros de exceção podem ser configurados globalmente ou por controlador ou por ação em um aplicativo MVC.</span><span class="sxs-lookup"><span data-stu-id="d6220-167">Exception filters can be configured globally or on a per-controller or per-action basis in an MVC app.</span></span> <span data-ttu-id="d6220-168">Esses filtros tratam qualquer exceção sem tratamento ocorrida durante a execução de uma ação do controlador ou de outro filtro e, caso contrário, não são chamadas.</span><span class="sxs-lookup"><span data-stu-id="d6220-168">These filters handle any unhandled exception that occurs during the execution of a controller action or another filter, and are not called otherwise.</span></span> <span data-ttu-id="d6220-169">Saiba mais sobre filtros de exceção em [Filtros](xref:mvc/controllers/filters).</span><span class="sxs-lookup"><span data-stu-id="d6220-169">Learn more about exception filters in [Filters](xref:mvc/controllers/filters).</span></span>

> [!TIP]
> <span data-ttu-id="d6220-170">Filtros de exceção são bons para interceptar exceções que ocorrem em ações do MVC, mas não são tão flexíveis quanto o middleware de tratamento de erro.</span><span class="sxs-lookup"><span data-stu-id="d6220-170">Exception filters are good for trapping exceptions that occur within MVC actions, but they're not as flexible as error handling middleware.</span></span> <span data-ttu-id="d6220-171">Dê preferência ao uso de middleware para o caso geral e use filtros apenas quando precisar fazer o tratamento de erro de modo *diferente*, dependendo da ação do MVC escolhida.</span><span class="sxs-lookup"><span data-stu-id="d6220-171">Prefer middleware for the general case, and use filters only where you need to do error handling *differently* based on which MVC action was chosen.</span></span>

### <a name="handling-model-state-errors"></a><span data-ttu-id="d6220-172">Tratando erros do estado do modelo</span><span class="sxs-lookup"><span data-stu-id="d6220-172">Handling Model State Errors</span></span>

<span data-ttu-id="d6220-173">A [validação do modelo](xref:mvc/models/validation) ocorre antes da invocação de cada ação do controlador e é responsabilidade do método de ação inspecionar `ModelState.IsValid` e responder de forma adequada.</span><span class="sxs-lookup"><span data-stu-id="d6220-173">[Model validation](xref:mvc/models/validation) occurs prior to invoking each controller action, and it's the action method's responsibility to inspect `ModelState.IsValid` and react appropriately.</span></span>

<span data-ttu-id="d6220-174">Alguns aplicativos optarão por seguir uma convenção padrão para lidar com erros de validação do modelo, caso em que um [filtro](xref:mvc/controllers/filters) pode ser um local adequado para implementar uma política como essa.</span><span class="sxs-lookup"><span data-stu-id="d6220-174">Some apps will choose to follow a standard convention for dealing with model validation errors, in which case a [filter](xref:mvc/controllers/filters) may be an appropriate place to implement such a policy.</span></span> <span data-ttu-id="d6220-175">É necessário testar o comportamento das ações com estados de modelo inválidos.</span><span class="sxs-lookup"><span data-stu-id="d6220-175">You should test how your actions behave with invalid model states.</span></span> <span data-ttu-id="d6220-176">Saiba mais em [Testar a lógica do controlador](xref:mvc/controllers/testing).</span><span class="sxs-lookup"><span data-stu-id="d6220-176">Learn more in [Test controller logic](xref:mvc/controllers/testing).</span></span>

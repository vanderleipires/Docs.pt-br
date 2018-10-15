---
title: Impor HTTPS no ASP.NET Core
author: rick-anderson
description: Aprenda a exigir HTTPS/TLS em um aplicativo web ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 10/11/2018
uid: security/enforcing-ssl
ms.openlocfilehash: b4c058d3b4f00276043d9520d756e62ed8cac5d9
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/15/2018
ms.locfileid: "49325595"
---
# <a name="enforce-https-in-aspnet-core"></a><span data-ttu-id="8e6f3-103">Impor HTTPS no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8e6f3-103">Enforce HTTPS in ASP.NET Core</span></span>

<span data-ttu-id="8e6f3-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="8e6f3-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="8e6f3-105">Este documento demonstra como:</span><span class="sxs-lookup"><span data-stu-id="8e6f3-105">This document shows how to:</span></span>

* <span data-ttu-id="8e6f3-106">Exigir HTTPS para todas as solicitações.</span><span class="sxs-lookup"><span data-stu-id="8e6f3-106">Require HTTPS for all requests.</span></span>
* <span data-ttu-id="8e6f3-107">Redirecione todas as solicitações HTTP para HTTPS.</span><span class="sxs-lookup"><span data-stu-id="8e6f3-107">Redirect all HTTP requests to HTTPS.</span></span>

<span data-ttu-id="8e6f3-108">Nenhuma API pode impedir que um cliente envie dados confidenciais na primeira solicitação.</span><span class="sxs-lookup"><span data-stu-id="8e6f3-108">No API can prevent a client from sending sensitive data on the first request.</span></span>

> [!WARNING]
> <span data-ttu-id="8e6f3-109">Fazer **não** usar [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) em APIs da Web que recebe informações confidenciais.</span><span class="sxs-lookup"><span data-stu-id="8e6f3-109">Do **not** use [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="8e6f3-110">`RequireHttpsAttribute` usa códigos de status HTTP para redirecionar navegadores de HTTP para HTTPS.</span><span class="sxs-lookup"><span data-stu-id="8e6f3-110">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="8e6f3-111">Os clientes da API não podem compreender ou obedecem redirecionamentos de HTTP para HTTPS.</span><span class="sxs-lookup"><span data-stu-id="8e6f3-111">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="8e6f3-112">Esses clientes podem enviar informações por meio de HTTP.</span><span class="sxs-lookup"><span data-stu-id="8e6f3-112">Such clients may send information over HTTP.</span></span> <span data-ttu-id="8e6f3-113">As APIs da Web deverá:</span><span class="sxs-lookup"><span data-stu-id="8e6f3-113">Web APIs should either:</span></span>
>
> * <span data-ttu-id="8e6f3-114">Não realizar a escuta em HTTP.</span><span class="sxs-lookup"><span data-stu-id="8e6f3-114">Not listen on HTTP.</span></span>
> * <span data-ttu-id="8e6f3-115">Feche a conexão com o código de status 400 (solicitação incorreta) e não atender à solicitação.</span><span class="sxs-lookup"><span data-stu-id="8e6f3-115">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

<a name="require"></a>

## <a name="require-https"></a><span data-ttu-id="8e6f3-116">Exigir HTTPS</span><span class="sxs-lookup"><span data-stu-id="8e6f3-116">Require HTTPS</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="8e6f3-117">É recomendável toda a produção de ASP.NET Core, chamada de aplicativos web:</span><span class="sxs-lookup"><span data-stu-id="8e6f3-117">We recommend all production ASP.NET Core web apps call:</span></span>

* <span data-ttu-id="8e6f3-118">O Middleware de redirecionamento de HTTPS ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) para redirecionar todas as solicitações HTTP para HTTPS.</span><span class="sxs-lookup"><span data-stu-id="8e6f3-118">The HTTPS Redirection Middleware ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) to redirect all HTTP requests to HTTPS.</span></span>
* <span data-ttu-id="8e6f3-119">[UseHsts](#hsts), protocolo de segurança de transporte estrito HTTP (HSTS).</span><span class="sxs-lookup"><span data-stu-id="8e6f3-119">[UseHsts](#hsts), HTTP Strict Transport Security Protocol (HSTS).</span></span>

### <a name="usehttpsredirection"></a><span data-ttu-id="8e6f3-120">UseHttpsRedirection</span><span class="sxs-lookup"><span data-stu-id="8e6f3-120">UseHttpsRedirection</span></span>

<span data-ttu-id="8e6f3-121">O código a seguir chama `UseHttpsRedirection` no `Startup` classe:</span><span class="sxs-lookup"><span data-stu-id="8e6f3-121">The following code calls `UseHttpsRedirection` in the `Startup` class:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]

<span data-ttu-id="8e6f3-122">O código realçado anterior:</span><span class="sxs-lookup"><span data-stu-id="8e6f3-122">The preceding highlighted code:</span></span>

* <span data-ttu-id="8e6f3-123">Usa o padrão [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) ([Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect)).</span><span class="sxs-lookup"><span data-stu-id="8e6f3-123">Uses the default [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) ([Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect)).</span></span>
* <span data-ttu-id="8e6f3-124">Usa o padrão [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (null), a menos que substituído pela `ASPNETCORE_HTTPS_PORT` variável de ambiente ou [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature).</span><span class="sxs-lookup"><span data-stu-id="8e6f3-124">Uses the default [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (null) unless overridden by the `ASPNETCORE_HTTPS_PORT` environment variable or [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature).</span></span>

<span data-ttu-id="8e6f3-125">É recomendável usar redirecionamentos temporários em vez de redirecionamentos permanentes, como cache de link pode causar um comportamento instável em ambientes de desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="8e6f3-125">We recommend using temporary redirects rather than permanent redirects, as link caching can cause unstable behavior in development environments.</span></span> <span data-ttu-id="8e6f3-126">É recomendável usar [HSTS](#hsts) para sinalizar para os clientes que proteger somente o recurso deve ser redirecionada para o aplicativo (somente em produção).</span><span class="sxs-lookup"><span data-stu-id="8e6f3-126">We recommend using [HSTS](#hsts) to signal to clients that only secure resource requests should be sent to the app (only in production).</span></span>

> [!WARNING]
> <span data-ttu-id="8e6f3-127">Uma porta deve estar disponível para o middleware redirecionar para HTTPS.</span><span class="sxs-lookup"><span data-stu-id="8e6f3-127">A port must be available for the middleware to redirect to HTTPS.</span></span> <span data-ttu-id="8e6f3-128">Se nenhuma porta estiver disponível, o redirecionamento de HTTPS não ocorre.</span><span class="sxs-lookup"><span data-stu-id="8e6f3-128">If no port is available, redirection to HTTPS doesn't occur.</span></span> <span data-ttu-id="8e6f3-129">A porta HTTPS pode ser especificada usando qualquer uma das seguintes abordagens:</span><span class="sxs-lookup"><span data-stu-id="8e6f3-129">The HTTPS port can be specified using any of the following approaches:</span></span>
>
> * <span data-ttu-id="8e6f3-130">Definir `HttpsRedirectionOptions.HttpsPort`.</span><span class="sxs-lookup"><span data-stu-id="8e6f3-130">Set `HttpsRedirectionOptions.HttpsPort`.</span></span>
> * <span data-ttu-id="8e6f3-131">Definir a variável de ambiente `ASPNETCORE_HTTPS_PORT`.</span><span class="sxs-lookup"><span data-stu-id="8e6f3-131">Set the `ASPNETCORE_HTTPS_PORT` environment variable.</span></span>
> * <span data-ttu-id="8e6f3-132">No desenvolvimento, defina uma URL HTTPS *launchsettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="8e6f3-132">In development, set an HTTPS URL in *launchsettings.json*.</span></span>
> * <span data-ttu-id="8e6f3-133">Configurar um ponto de extremidade de URL HTTPS para [Kestrel](xref:fundamentals/servers/kestrel) ou [HTTP. sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="8e6f3-133">Configure an HTTPS URL endpoint for [Kestrel](xref:fundamentals/servers/kestrel) or [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>
>
> <span data-ttu-id="8e6f3-134">Quando o Kestrel ou HTTP. sys é usado como um servidor de borda para o público, o Kestrel ou HTTP. sys deve ser configurado para escutar em ambos:</span><span class="sxs-lookup"><span data-stu-id="8e6f3-134">When Kestrel or HTTP.sys is used as a public-facing edge server, Kestrel or HTTP.sys must be configured to listen on both:</span></span>
>
> * <span data-ttu-id="8e6f3-135">A porta segura em que o cliente é redirecionado (normalmente, 443 em 5001 no desenvolvimento e produção).</span><span class="sxs-lookup"><span data-stu-id="8e6f3-135">The secure port where the client is redirected (typically, 443 in production and 5001 in development).</span></span>
> * <span data-ttu-id="8e6f3-136">A porta não segura (normalmente, 80 na produção) e 5000 no desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="8e6f3-136">The insecure port (typically, 80 in production and 5000 in development).</span></span>
>
> <span data-ttu-id="8e6f3-137">A porta não segura deve estar acessível pelo cliente para que o aplicativo para receber uma solicitação não segura e redirecioná-la para a porta segura.</span><span class="sxs-lookup"><span data-stu-id="8e6f3-137">The insecure port must be accessible by the client in order for the app to receive an insecure request and redirect it to the secure port.</span></span>
>
> <span data-ttu-id="8e6f3-138">Qualquer firewall entre o cliente e o servidor também deve ter as portas abertas para o tráfego.</span><span class="sxs-lookup"><span data-stu-id="8e6f3-138">Any firewall between the client and server must also have the ports open for traffic.</span></span>
>
> <span data-ttu-id="8e6f3-139">Para obter mais informações, consulte [configuração de ponto de extremidade do Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) ou <xref:fundamentals/servers/httpsys>.</span><span class="sxs-lookup"><span data-stu-id="8e6f3-139">For more information, see [Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) or <xref:fundamentals/servers/httpsys>.</span></span>

<span data-ttu-id="8e6f3-140">O seguinte realçado código chama [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) para configurar as opções de middleware:</span><span class="sxs-lookup"><span data-stu-id="8e6f3-140">The following highlighted code calls [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) to configure middleware options:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]

<span data-ttu-id="8e6f3-141">Chamando `AddHttpsRedirection` só é necessário alterar os valores de `HttpsPort` ou `RedirectStatusCode`.</span><span class="sxs-lookup"><span data-stu-id="8e6f3-141">Calling `AddHttpsRedirection` is only necessary to change the values of `HttpsPort` or `RedirectStatusCode`.</span></span>

<span data-ttu-id="8e6f3-142">O código realçado anterior:</span><span class="sxs-lookup"><span data-stu-id="8e6f3-142">The preceding highlighted code:</span></span>

* <span data-ttu-id="8e6f3-143">Conjuntos [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) à [Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect), que é o valor padrão.</span><span class="sxs-lookup"><span data-stu-id="8e6f3-143">Sets [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) to [Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect), which is the default value.</span></span> <span data-ttu-id="8e6f3-144">Use os campos do [StatusCodes](/dotnet/api/microsoft.aspnetcore.http.statuscodes) classe atribuições para `RedirectStatusCode`.</span><span class="sxs-lookup"><span data-stu-id="8e6f3-144">Use the fields of the [StatusCodes](/dotnet/api/microsoft.aspnetcore.http.statuscodes) class for assignments to `RedirectStatusCode`.</span></span>
* <span data-ttu-id="8e6f3-145">Define a porta HTTPS para 5001.</span><span class="sxs-lookup"><span data-stu-id="8e6f3-145">Sets the HTTPS port to 5001.</span></span> <span data-ttu-id="8e6f3-146">O valor padrão é 443.</span><span class="sxs-lookup"><span data-stu-id="8e6f3-146">The default value is 443.</span></span>

<span data-ttu-id="8e6f3-147">Os seguintes mecanismos de definir a porta automaticamente:</span><span class="sxs-lookup"><span data-stu-id="8e6f3-147">The following mechanisms set the port automatically:</span></span>

* <span data-ttu-id="8e6f3-148">O middleware pode descobrir as portas por meio [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) quando as condições a seguir se aplicam:</span><span class="sxs-lookup"><span data-stu-id="8e6f3-148">The middleware can discover the ports via [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) when the following conditions apply:</span></span>

  * <span data-ttu-id="8e6f3-149">O kestrel ou HTTP. sys é usada diretamente com pontos de extremidade HTTPS (também se aplica a execução do aplicativo com o depurador do Visual Studio Code).</span><span class="sxs-lookup"><span data-stu-id="8e6f3-149">Kestrel or HTTP.sys is used directly with HTTPS endpoints (also applies to running the app with Visual Studio Code's debugger).</span></span>
  * <span data-ttu-id="8e6f3-150">Somente **uma porta HTTPS** é usado pelo aplicativo.</span><span class="sxs-lookup"><span data-stu-id="8e6f3-150">Only **one HTTPS port** is used by the app.</span></span>

* <span data-ttu-id="8e6f3-151">O Visual Studio é usado:</span><span class="sxs-lookup"><span data-stu-id="8e6f3-151">Visual Studio is used:</span></span>

  * <span data-ttu-id="8e6f3-152">O IIS Express tem habilitadas para HTTPS.</span><span class="sxs-lookup"><span data-stu-id="8e6f3-152">IIS Express has HTTPS enabled.</span></span>
  * <span data-ttu-id="8e6f3-153">*launchsettings. JSON* define o `sslPort` para o IIS Express.</span><span class="sxs-lookup"><span data-stu-id="8e6f3-153">*launchSettings.json* sets the `sslPort` for IIS Express.</span></span>

> [!NOTE]
> <span data-ttu-id="8e6f3-154">Quando um aplicativo é executado atrás de um proxy reverso (por exemplo, IIS, IIS Express), `IServerAddressesFeature` não está disponível.</span><span class="sxs-lookup"><span data-stu-id="8e6f3-154">When an app is run behind a reverse proxy (for example, IIS, IIS Express), `IServerAddressesFeature` isn't available.</span></span> <span data-ttu-id="8e6f3-155">A porta deve ser configurada manualmente.</span><span class="sxs-lookup"><span data-stu-id="8e6f3-155">The port must be manually configured.</span></span> <span data-ttu-id="8e6f3-156">Quando a porta não for definida, as solicitações não são redirecionadas.</span><span class="sxs-lookup"><span data-stu-id="8e6f3-156">When the port isn't set, requests aren't redirected.</span></span>

<span data-ttu-id="8e6f3-157">A porta pode ser configurada definindo a [definição de configuração do Host da Web https_port](xref:fundamentals/host/web-host#https-port):</span><span class="sxs-lookup"><span data-stu-id="8e6f3-157">The port can be configured by setting the [https_port Web Host configuration setting](xref:fundamentals/host/web-host#https-port):</span></span>

<span data-ttu-id="8e6f3-158">**Chave**: https_port</span><span class="sxs-lookup"><span data-stu-id="8e6f3-158">**Key**: https_port</span></span>  
<span data-ttu-id="8e6f3-159">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="8e6f3-159">**Type**: *string*</span></span>  
<span data-ttu-id="8e6f3-160">**Padrão**: um valor padrão não está definido.</span><span class="sxs-lookup"><span data-stu-id="8e6f3-160">**Default**: A default value isn't set.</span></span>  
<span data-ttu-id="8e6f3-161">**Definido usando**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="8e6f3-161">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="8e6f3-162">**Variável de ambiente**: `<PREFIX_>HTTPS_PORT` (o prefixo é `ASPNETCORE_` ao usar o Host da Web.)</span><span class="sxs-lookup"><span data-stu-id="8e6f3-162">**Environment variable**: `<PREFIX_>HTTPS_PORT` (The prefix is `ASPNETCORE_` when using the Web Host.)</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("https_port", "8080")
```

> [!NOTE]
> <span data-ttu-id="8e6f3-163">A porta pode ser configurada indiretamente, definindo a URL com o `ASPNETCORE_URLS` variável de ambiente.</span><span class="sxs-lookup"><span data-stu-id="8e6f3-163">The port can be configured indirectly by setting the URL with the `ASPNETCORE_URLS` environment variable.</span></span> <span data-ttu-id="8e6f3-164">A variável de ambiente configura o servidor e, em seguida, o middleware indiretamente descobre a porta HTTPS por meio de `IServerAddressesFeature`.</span><span class="sxs-lookup"><span data-stu-id="8e6f3-164">The environment variable configures the server, and then the middleware indirectly discovers the HTTPS port via `IServerAddressesFeature`.</span></span>

<span data-ttu-id="8e6f3-165">Se nenhuma porta for definida:</span><span class="sxs-lookup"><span data-stu-id="8e6f3-165">If no port is set:</span></span>

* <span data-ttu-id="8e6f3-166">As solicitações não são redirecionadas.</span><span class="sxs-lookup"><span data-stu-id="8e6f3-166">Requests aren't redirected.</span></span>
* <span data-ttu-id="8e6f3-167">O middleware registra o aviso "Falha ao determinar a porta https para redirecionamento."</span><span class="sxs-lookup"><span data-stu-id="8e6f3-167">The middleware logs the warning "Failed to determine the https port for redirect."</span></span>

> [!NOTE]
> <span data-ttu-id="8e6f3-168">Uma alternativa ao uso de Middleware de redirecionamento de HTTPS (`UseHttpsRedirection`) é usar o Middleware de reconfiguração de URL (`AddRedirectToHttps`).</span><span class="sxs-lookup"><span data-stu-id="8e6f3-168">An alternative to using HTTPS Redirection Middleware (`UseHttpsRedirection`) is to use URL Rewriting Middleware (`AddRedirectToHttps`).</span></span> <span data-ttu-id="8e6f3-169">`AddRedirectToHttps` também pode definir o código de status e a porta quando o redirecionamento é executado.</span><span class="sxs-lookup"><span data-stu-id="8e6f3-169">`AddRedirectToHttps` can also set the status code and port when the redirect is executed.</span></span> <span data-ttu-id="8e6f3-170">Para obter mais informações, consulte [Middleware de reconfiguração de URL](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="8e6f3-170">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>
>
> <span data-ttu-id="8e6f3-171">Ao redirecionar para HTTPS sem a necessidade de regras de redirecionamento adicional, é recomendável usar o Middleware de redirecionamento de HTTPS (`UseHttpsRedirection`) descrito neste tópico.</span><span class="sxs-lookup"><span data-stu-id="8e6f3-171">When redirecting to HTTPS without the requirement for additional redirect rules, we recommend using HTTPS Redirection Middleware (`UseHttpsRedirection`) described in this topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="8e6f3-172">O [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) é usada para exigir HTTPS.</span><span class="sxs-lookup"><span data-stu-id="8e6f3-172">The [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require HTTPS.</span></span> <span data-ttu-id="8e6f3-173">`[RequireHttpsAttribute]` pode decorar controladores ou métodos, ou podem ser aplicadas globalmente.</span><span class="sxs-lookup"><span data-stu-id="8e6f3-173">`[RequireHttpsAttribute]` can decorate controllers or methods, or can be applied globally.</span></span> <span data-ttu-id="8e6f3-174">Para aplicar o atributo globalmente, adicione o seguinte código ao `ConfigureServices` em `Startup`:</span><span class="sxs-lookup"><span data-stu-id="8e6f3-174">To apply the attribute globally, add the following code to `ConfigureServices` in `Startup`:</span></span>

[!code-csharp[](~/security/authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

<span data-ttu-id="8e6f3-175">O código realçado anterior requer que todas as solicitações usem `HTTPS`; portanto, as solicitações HTTP são ignoradas.</span><span class="sxs-lookup"><span data-stu-id="8e6f3-175">The preceding highlighted code requires all requests use `HTTPS`; therefore, HTTP requests are ignored.</span></span> <span data-ttu-id="8e6f3-176">O seguinte código realçado redireciona todas as solicitações HTTP para HTTPS:</span><span class="sxs-lookup"><span data-stu-id="8e6f3-176">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

<span data-ttu-id="8e6f3-177">Para obter mais informações, consulte [Middleware de reconfiguração de URL](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="8e6f3-177">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="8e6f3-178">O middleware também permite que o aplicativo para definir o código de status ou o código de status e a porta quando o redirecionamento é executado.</span><span class="sxs-lookup"><span data-stu-id="8e6f3-178">The middleware also permits the app to set the status code or the status code and the port when the redirect is executed.</span></span>

<span data-ttu-id="8e6f3-179">Exigir HTTPS globalmente (`options.Filters.Add(new RequireHttpsAttribute());`) é uma prática recomendada de segurança.</span><span class="sxs-lookup"><span data-stu-id="8e6f3-179">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="8e6f3-180">Aplicando o `[RequireHttps]` atributo a todas as páginas do Razor/controladores não é considerado tão seguro quanto exigir HTTPS globalmente.</span><span class="sxs-lookup"><span data-stu-id="8e6f3-180">Applying the `[RequireHttps]` attribute to all controllers/Razor Pages isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="8e6f3-181">Você não pode garantir o `[RequireHttps]` atributo é aplicado quando novos controladores e páginas Razor são adicionadas.</span><span class="sxs-lookup"><span data-stu-id="8e6f3-181">You can't guarantee the `[RequireHttps]` attribute is applied when new controllers and Razor Pages are added.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="hsts"></a>

## <a name="http-strict-transport-security-protocol-hsts"></a><span data-ttu-id="8e6f3-182">Protocolo de segurança de transporte estrito HTTP (HSTS)</span><span class="sxs-lookup"><span data-stu-id="8e6f3-182">HTTP Strict Transport Security Protocol (HSTS)</span></span>

<span data-ttu-id="8e6f3-183">Por [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [segurança de transporte estrito HTTP (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) é um aprimoramento de segurança opcional é especificado por um aplicativo web com o uso de um cabeçalho de resposta.</span><span class="sxs-lookup"><span data-stu-id="8e6f3-183">Per [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) is an opt-in security enhancement that's specified by a web app through the use of a response header.</span></span> <span data-ttu-id="8e6f3-184">Quando um [navegador que ofereça suporte a HSTS](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support) recebe esse cabeçalho:</span><span class="sxs-lookup"><span data-stu-id="8e6f3-184">When a [browser that supports HSTS](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support) receives this header:</span></span>

* <span data-ttu-id="8e6f3-185">O navegador armazena a configuração para o domínio que evita o envio de qualquer comunicação por HTTP.</span><span class="sxs-lookup"><span data-stu-id="8e6f3-185">The browser stores configuration for the domain that prevents sending any communication over HTTP.</span></span> <span data-ttu-id="8e6f3-186">O navegador força todas as comunicações via HTTPS.</span><span class="sxs-lookup"><span data-stu-id="8e6f3-186">The browser forces all communication over HTTPS.</span></span>
* <span data-ttu-id="8e6f3-187">O navegador impede que o usuário usando os certificados não confiáveis ou é inválidos.</span><span class="sxs-lookup"><span data-stu-id="8e6f3-187">The browser prevents the user from using untrusted or invalid certificates.</span></span> <span data-ttu-id="8e6f3-188">O navegador desativa prompts que permitem que um usuário para confiar temporariamente esses certificados.</span><span class="sxs-lookup"><span data-stu-id="8e6f3-188">The browser disables prompts that allow a user to temporarily trust such a certificate.</span></span>

<span data-ttu-id="8e6f3-189">Como HSTS é imposta pelo cliente, ele tem algumas limitações:</span><span class="sxs-lookup"><span data-stu-id="8e6f3-189">Because HSTS is enforced by the client it has some limitations:</span></span>

* <span data-ttu-id="8e6f3-190">O cliente deve oferecer suporte a HSTS.</span><span class="sxs-lookup"><span data-stu-id="8e6f3-190">The client must support HSTS.</span></span>
* <span data-ttu-id="8e6f3-191">HSTS requer pelo menos uma solicitação HTTPS bem-sucedida para estabelecer a política HSTS.</span><span class="sxs-lookup"><span data-stu-id="8e6f3-191">HSTS requires at least one successful HTTPS request to establish the HSTS policy.</span></span>
* <span data-ttu-id="8e6f3-192">O aplicativo deve verificar todas as solicitações HTTP e redirecionar ou rejeitar a solicitação HTTP.</span><span class="sxs-lookup"><span data-stu-id="8e6f3-192">The application must check every HTTP request and redirect or reject the HTTP request.</span></span>

<span data-ttu-id="8e6f3-193">ASP.NET Core 2.1 ou posterior implementa HSTS com o `UseHsts` método de extensão.</span><span class="sxs-lookup"><span data-stu-id="8e6f3-193">ASP.NET Core 2.1 or later implements HSTS with the `UseHsts` extension method.</span></span> <span data-ttu-id="8e6f3-194">O código a seguir chama `UseHsts` quando o aplicativo não está no [modo de desenvolvimento](xref:fundamentals/environments):</span><span class="sxs-lookup"><span data-stu-id="8e6f3-194">The following code calls `UseHsts` when the app isn't in [development mode](xref:fundamentals/environments):</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]

<span data-ttu-id="8e6f3-195">`UseHsts` não é recomendado em desenvolvimento porque as configurações de HSTS são altamente armazenáveis em cache por navegadores.</span><span class="sxs-lookup"><span data-stu-id="8e6f3-195">`UseHsts` isn't recommended in development because the HSTS settings are highly cacheable by browsers.</span></span> <span data-ttu-id="8e6f3-196">Por padrão, `UseHsts` exclui o endereço de loopback local.</span><span class="sxs-lookup"><span data-stu-id="8e6f3-196">By default, `UseHsts` excludes the local loopback address.</span></span>

<span data-ttu-id="8e6f3-197">Para ambientes de produção implementando HTTPS pela primeira vez, defina o valor inicial de HSTS para um valor pequeno.</span><span class="sxs-lookup"><span data-stu-id="8e6f3-197">For production environments implementing HTTPS for the first time, set the initial HSTS value to a small value.</span></span> <span data-ttu-id="8e6f3-198">Defina o valor de horas como não mais do que um único dia caso você precise reverter a infraestrutura HTTPS para HTTP.</span><span class="sxs-lookup"><span data-stu-id="8e6f3-198">Set the value from hours to no more than a single day in case you need to revert the HTTPS infrastructure to HTTP.</span></span> <span data-ttu-id="8e6f3-199">Depois que você estiver confiante em sustentabilidade da configuração do HTTPS, aumente o valor de idade máxima HSTS; um valor comumente usado é um ano.</span><span class="sxs-lookup"><span data-stu-id="8e6f3-199">After you're confident in the sustainability of the HTTPS configuration, increase the HSTS max-age value; a commonly used value is one year.</span></span> 

<span data-ttu-id="8e6f3-200">O código a seguir:</span><span class="sxs-lookup"><span data-stu-id="8e6f3-200">The following code:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]

* <span data-ttu-id="8e6f3-201">Define o parâmetro de pré-carregamento do cabeçalho de segurança de transporte estrito.</span><span class="sxs-lookup"><span data-stu-id="8e6f3-201">Sets the preload parameter of the Strict-Transport-Security header.</span></span> <span data-ttu-id="8e6f3-202">Pré-carregamento não faz parte dos [especificação RFC HSTS](https://tools.ietf.org/html/rfc6797), mas é compatível com navegadores da web para pré-carregar sites HSTS na nova instalação.</span><span class="sxs-lookup"><span data-stu-id="8e6f3-202">Preload isn't part of the [RFC HSTS specification](https://tools.ietf.org/html/rfc6797), but is supported by web browsers to preload HSTS sites on fresh install.</span></span> <span data-ttu-id="8e6f3-203">Veja [https://hstspreload.org/](https://hstspreload.org/) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="8e6f3-203">See [https://hstspreload.org/](https://hstspreload.org/) for more information.</span></span>
* <span data-ttu-id="8e6f3-204">Habilita [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), que se aplica a política HSTS para hospedar subdomínios.</span><span class="sxs-lookup"><span data-stu-id="8e6f3-204">Enables [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), which applies the HSTS policy to Host subdomains.</span></span>
* <span data-ttu-id="8e6f3-205">Define explicitamente o parâmetro de idade máxima do cabeçalho de segurança de transporte estrito para 60 dias.</span><span class="sxs-lookup"><span data-stu-id="8e6f3-205">Explicitly sets the max-age parameter of the Strict-Transport-Security header to 60 days.</span></span> <span data-ttu-id="8e6f3-206">Se não for definido, o padrão é 30 dias.</span><span class="sxs-lookup"><span data-stu-id="8e6f3-206">If not set, defaults to 30 days.</span></span> <span data-ttu-id="8e6f3-207">Consulte a [max-age diretiva](https://tools.ietf.org/html/rfc6797#section-6.1.1) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="8e6f3-207">See the [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) for more information.</span></span>
* <span data-ttu-id="8e6f3-208">Adiciona `example.com` à lista de hosts a serem excluídos.</span><span class="sxs-lookup"><span data-stu-id="8e6f3-208">Adds `example.com` to the list of hosts to exclude.</span></span>

<span data-ttu-id="8e6f3-209">`UseHsts` Exclui os seguintes hosts de loopback:</span><span class="sxs-lookup"><span data-stu-id="8e6f3-209">`UseHsts` excludes the following loopback hosts:</span></span>

* <span data-ttu-id="8e6f3-210">`localhost` : O endereço de loopback do IPv4.</span><span class="sxs-lookup"><span data-stu-id="8e6f3-210">`localhost` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="8e6f3-211">`127.0.0.1` : O endereço de loopback do IPv4.</span><span class="sxs-lookup"><span data-stu-id="8e6f3-211">`127.0.0.1` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="8e6f3-212">`[::1]` : O endereço de loopback do IPv6.</span><span class="sxs-lookup"><span data-stu-id="8e6f3-212">`[::1]` : The IPv6 loopback address.</span></span>

<span data-ttu-id="8e6f3-213">O exemplo anterior mostra como adicionar outros hosts.</span><span class="sxs-lookup"><span data-stu-id="8e6f3-213">The preceding example shows how to add additional hosts.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="https"></a>

## <a name="opt-out-of-httpshsts-on-project-creation"></a><span data-ttu-id="8e6f3-214">Recusa de HTTPS/HSTS na criação do projeto</span><span class="sxs-lookup"><span data-stu-id="8e6f3-214">Opt-out of HTTPS/HSTS on project creation</span></span>

<span data-ttu-id="8e6f3-215">Em alguns cenários de serviço de back-end em que a segurança de conexão é manipulada na borda da rede para o público, configurando a segurança de conexão em cada nó não é necessária.</span><span class="sxs-lookup"><span data-stu-id="8e6f3-215">In some backend service scenarios where connection security is handled at the public-facing edge of the network, configuring connection security at each node isn't required.</span></span> <span data-ttu-id="8e6f3-216">Aplicativos gerados a partir de modelos no Visual Studio ou na Web a [dotnet new](/dotnet/core/tools/dotnet-new) comando enable [redirecionamento a HTTPS](#require) e [HSTS](#hsts).</span><span class="sxs-lookup"><span data-stu-id="8e6f3-216">Web apps generated from the templates in Visual Studio or from the [dotnet new](/dotnet/core/tools/dotnet-new) command enable [HTTPS redirection](#require) and [HSTS](#hsts).</span></span> <span data-ttu-id="8e6f3-217">Para implantações que não exigem esses cenários, você pode recusar HTTPS/HSTS quando o aplicativo é criado a partir do modelo.</span><span class="sxs-lookup"><span data-stu-id="8e6f3-217">For deployments that don't require these scenarios, you can opt-out of HTTPS/HSTS when the app is created from the template.</span></span>

<span data-ttu-id="8e6f3-218">A recusa de HTTPS/HSTS:</span><span class="sxs-lookup"><span data-stu-id="8e6f3-218">To opt-out of HTTPS/HSTS:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8e6f3-219">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8e6f3-219">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="8e6f3-220">Desmarque a **configurar para HTTPS** caixa de seleção.</span><span class="sxs-lookup"><span data-stu-id="8e6f3-220">Uncheck the **Configure for HTTPS** checkbox.</span></span>

![Diagrama de entidade](enforcing-ssl/_static/out.png)

#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="8e6f3-222">CLI do .NET Core</span><span class="sxs-lookup"><span data-stu-id="8e6f3-222">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="8e6f3-223">Use a opção `--no-https`.</span><span class="sxs-lookup"><span data-stu-id="8e6f3-223">Use the `--no-https` option.</span></span> <span data-ttu-id="8e6f3-224">Por exemplo</span><span class="sxs-lookup"><span data-stu-id="8e6f3-224">For example</span></span>

```console
dotnet new webapp --no-https
```

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="how-to-set-up-a-developer-certificate-for-docker"></a><span data-ttu-id="8e6f3-225">Como configurar um certificado de desenvolvedor para o Docker</span><span class="sxs-lookup"><span data-stu-id="8e6f3-225">How to set up a developer certificate for Docker</span></span>

<span data-ttu-id="8e6f3-226">Ver [esse problema de GitHub](https://github.com/aspnet/Docs/issues/6199).</span><span class="sxs-lookup"><span data-stu-id="8e6f3-226">See [this GitHub issue](https://github.com/aspnet/Docs/issues/6199).</span></span>

::: moniker-end

## <a name="additional-information"></a><span data-ttu-id="8e6f3-227">Informações adicionais</span><span class="sxs-lookup"><span data-stu-id="8e6f3-227">Additional information</span></span>

* [<span data-ttu-id="8e6f3-228">Suporte a navegador HSTS OWASP</span><span class="sxs-lookup"><span data-stu-id="8e6f3-228">OWASP HSTS browser support</span></span>](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support)

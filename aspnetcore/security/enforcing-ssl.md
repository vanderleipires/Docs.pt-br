---
title: Impor HTTPS no ASP.NET Core
author: rick-anderson
description: Mostra como exigir HTTPS/TLS em um ASP.NET Core em aplicativo web.
ms.author: riande
ms.date: 2/9/2018
uid: security/enforcing-ssl
ms.openlocfilehash: 838cd00545f36736461616f806942249aaf6eee0
ms.sourcegitcommit: 4cd8dce371d63a66d780e4af1baab2bcf9d61b24
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/06/2018
ms.locfileid: "43893172"
---
# <a name="enforce-https-in-aspnet-core"></a><span data-ttu-id="3054d-103">Impor HTTPS no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3054d-103">Enforce HTTPS in ASP.NET Core</span></span>

<span data-ttu-id="3054d-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="3054d-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="3054d-105">Este documento demonstra como:</span><span class="sxs-lookup"><span data-stu-id="3054d-105">This document shows how to:</span></span>

* <span data-ttu-id="3054d-106">Exigir HTTPS para todas as solicitações.</span><span class="sxs-lookup"><span data-stu-id="3054d-106">Require HTTPS for all requests.</span></span>
* <span data-ttu-id="3054d-107">Redirecione todas as solicitações HTTP para HTTPS.</span><span class="sxs-lookup"><span data-stu-id="3054d-107">Redirect all HTTP requests to HTTPS.</span></span>

<span data-ttu-id="3054d-108">Nenhuma API pode impedir que um cliente envie dados confidenciais na primeira solicitação.</span><span class="sxs-lookup"><span data-stu-id="3054d-108">No API can prevent a client from sending sensitive data on the first request.</span></span>

> [!WARNING]
> <span data-ttu-id="3054d-109">Fazer **não** usar [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) em APIs da Web que recebe informações confidenciais.</span><span class="sxs-lookup"><span data-stu-id="3054d-109">Do **not** use [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="3054d-110">`RequireHttpsAttribute` usa códigos de status HTTP para redirecionar navegadores de HTTP para HTTPS.</span><span class="sxs-lookup"><span data-stu-id="3054d-110">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="3054d-111">Os clientes da API não podem compreender ou obedecem redirecionamentos de HTTP para HTTPS.</span><span class="sxs-lookup"><span data-stu-id="3054d-111">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="3054d-112">Esses clientes podem enviar informações por meio de HTTP.</span><span class="sxs-lookup"><span data-stu-id="3054d-112">Such clients may send information over HTTP.</span></span> <span data-ttu-id="3054d-113">As APIs da Web deverá:</span><span class="sxs-lookup"><span data-stu-id="3054d-113">Web APIs should either:</span></span>
>
> * <span data-ttu-id="3054d-114">Não realizar a escuta em HTTP.</span><span class="sxs-lookup"><span data-stu-id="3054d-114">Not listen on HTTP.</span></span>
> * <span data-ttu-id="3054d-115">Feche a conexão com o código de status 400 (solicitação incorreta) e não atender à solicitação.</span><span class="sxs-lookup"><span data-stu-id="3054d-115">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

<a name="require"></a>
## <a name="require-https"></a><span data-ttu-id="3054d-116">Exigir HTTPS</span><span class="sxs-lookup"><span data-stu-id="3054d-116">Require HTTPS</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="3054d-117">É recomendável toda a produção de ASP.NET Core, chamada de aplicativos web:</span><span class="sxs-lookup"><span data-stu-id="3054d-117">We recommend all production ASP.NET Core web apps call:</span></span>

* <span data-ttu-id="3054d-118">O Middleware de redirecionamento de HTTPS ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) para redirecionar todas as solicitações HTTP para HTTPS.</span><span class="sxs-lookup"><span data-stu-id="3054d-118">The HTTPS Redirection Middleware ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) to redirect all HTTP requests to HTTPS.</span></span>
* <span data-ttu-id="3054d-119">[UseHsts](#hsts), protocolo de segurança de transporte estrito HTTP (HSTS).</span><span class="sxs-lookup"><span data-stu-id="3054d-119">[UseHsts](#hsts), HTTP Strict Transport Security Protocol (HSTS).</span></span>

### <a name="usehttpsredirection"></a><span data-ttu-id="3054d-120">UseHttpsRedirection</span><span class="sxs-lookup"><span data-stu-id="3054d-120">UseHttpsRedirection</span></span>

<span data-ttu-id="3054d-121">O código a seguir chama `UseHttpsRedirection` no `Startup` classe:</span><span class="sxs-lookup"><span data-stu-id="3054d-121">The following code calls `UseHttpsRedirection` in the `Startup` class:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]

<span data-ttu-id="3054d-122">O código realçado anterior:</span><span class="sxs-lookup"><span data-stu-id="3054d-122">The preceding highlighted code:</span></span>

* <span data-ttu-id="3054d-123">Usa o padrão [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) (`Status307TemporaryRedirect`).</span><span class="sxs-lookup"><span data-stu-id="3054d-123">Uses the default [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) (`Status307TemporaryRedirect`).</span></span>
* <span data-ttu-id="3054d-124">Usa o padrão [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (null), a menos que substituído pela `ASPNETCORE_HTTPS_PORT` variável de ambiente ou [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature).</span><span class="sxs-lookup"><span data-stu-id="3054d-124">Uses the default [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (null) unless overridden by the `ASPNETCORE_HTTPS_PORT` environment variable or [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature).</span></span>

> [!WARNING] 
><span data-ttu-id="3054d-125">Uma porta deve estar disponível para o middleware redirecionar para HTTPS.</span><span class="sxs-lookup"><span data-stu-id="3054d-125">A port must be available for the middleware to redirect to HTTPS.</span></span> <span data-ttu-id="3054d-126">Se nenhuma porta estiver disponível, o redirecionamento de HTTPS não ocorrerá.</span><span class="sxs-lookup"><span data-stu-id="3054d-126">If no port is available, redirection to HTTPS does not occur.</span></span> <span data-ttu-id="3054d-127">A porta HTTPS pode ser especificada por qualquer um dos seguinte configuração:</span><span class="sxs-lookup"><span data-stu-id="3054d-127">The HTTPS port can be specified by any of the following setting:</span></span>
> 
>* `HttpsRedirectionOptions.HttpsPort` 
>* <span data-ttu-id="3054d-128">O `ASPNETCORE_HTTPS_PORT` variável de ambiente.</span><span class="sxs-lookup"><span data-stu-id="3054d-128">The `ASPNETCORE_HTTPS_PORT` environment variable.</span></span> 
>* <span data-ttu-id="3054d-129">No desenvolvimento, uma url HTTPS no *launchsettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="3054d-129">In development, an HTTPS url in *launchsettings.json*.</span></span> 
>* <span data-ttu-id="3054d-130">Uma url HTTPS configurada diretamente no Kestrel ou HttpSys.</span><span class="sxs-lookup"><span data-stu-id="3054d-130">An HTTPS url configured directly on Kestrel or HttpSys.</span></span> 

<span data-ttu-id="3054d-131">O seguinte realçado código chama [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) para configurar as opções de middleware:</span><span class="sxs-lookup"><span data-stu-id="3054d-131">The following highlighted code calls [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) to configure middleware options:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]

<span data-ttu-id="3054d-132">Chamando `AddHttpsRedirection` só é necessário alterar os valores de ` HttpsPort` ou ` RedirectStatusCode`;</span><span class="sxs-lookup"><span data-stu-id="3054d-132">Calling `AddHttpsRedirection` is only necessary to change the values of ` HttpsPort` or ` RedirectStatusCode`;</span></span>

<span data-ttu-id="3054d-133">O código realçado anterior:</span><span class="sxs-lookup"><span data-stu-id="3054d-133">The preceding highlighted code:</span></span>

* <span data-ttu-id="3054d-134">Conjuntos [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) para `Status307TemporaryRedirect`, que é o valor padrão.</span><span class="sxs-lookup"><span data-stu-id="3054d-134">Sets [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) to `Status307TemporaryRedirect`, which is the default value.</span></span>
* <span data-ttu-id="3054d-135">Define a porta HTTPS para 5001.</span><span class="sxs-lookup"><span data-stu-id="3054d-135">Sets the HTTPS port to 5001.</span></span> <span data-ttu-id="3054d-136">O valor padrão é 443.</span><span class="sxs-lookup"><span data-stu-id="3054d-136">The default value is 443.</span></span>

<span data-ttu-id="3054d-137">Os seguintes mecanismos de definir a porta automaticamente:</span><span class="sxs-lookup"><span data-stu-id="3054d-137">The following mechanisms set the port automatically:</span></span>

* <span data-ttu-id="3054d-138">O middleware pode descobrir as portas por meio [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) quando as condições a seguir se aplicam:</span><span class="sxs-lookup"><span data-stu-id="3054d-138">The middleware can discover the ports via [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) when the following conditions apply:</span></span>

   * <span data-ttu-id="3054d-139">O kestrel ou HTTP. sys é usada diretamente com pontos de extremidade HTTPS (também se aplica a execução do aplicativo com o depurador do Visual Studio Code).</span><span class="sxs-lookup"><span data-stu-id="3054d-139">Kestrel or HTTP.sys is used directly with HTTPS endpoints (also applies to running the app with Visual Studio Code's debugger).</span></span>
   * <span data-ttu-id="3054d-140">Somente **uma porta HTTPS** é usado pelo aplicativo.</span><span class="sxs-lookup"><span data-stu-id="3054d-140">Only **one HTTPS port** is used by the app.</span></span>

* <span data-ttu-id="3054d-141">O Visual Studio é usado:</span><span class="sxs-lookup"><span data-stu-id="3054d-141">Visual Studio is used:</span></span>
   * <span data-ttu-id="3054d-142">O IIS Express tem habilitadas para HTTPS.</span><span class="sxs-lookup"><span data-stu-id="3054d-142">IIS Express has HTTPS enabled.</span></span>
   * <span data-ttu-id="3054d-143">*launchsettings. JSON* define o `sslPort` para o IIS Express.</span><span class="sxs-lookup"><span data-stu-id="3054d-143">*launchSettings.json* sets the `sslPort` for IIS Express.</span></span>

> [!NOTE]
> <span data-ttu-id="3054d-144">Quando um aplicativo é executado atrás de um proxy reverso (por exemplo, IIS, IIS Express), `IServerAddressesFeature` não está disponível.</span><span class="sxs-lookup"><span data-stu-id="3054d-144">When an app is run behind a reverse proxy (for example, IIS, IIS Express), `IServerAddressesFeature` isn't available.</span></span> <span data-ttu-id="3054d-145">A porta deve ser configurada manualmente.</span><span class="sxs-lookup"><span data-stu-id="3054d-145">The port must be manually configured.</span></span> <span data-ttu-id="3054d-146">Quando a porta não for definida, as solicitações não são redirecionadas.</span><span class="sxs-lookup"><span data-stu-id="3054d-146">When the port isn't set, requests aren't redirected.</span></span>

<span data-ttu-id="3054d-147">A porta pode ser configurada definindo a [definição de configuração do Host da Web https_port](xref:fundamentals/host/web-host#https-port):</span><span class="sxs-lookup"><span data-stu-id="3054d-147">The port can be configured by setting the [https_port Web Host configuration setting](xref:fundamentals/host/web-host#https-port):</span></span>

<span data-ttu-id="3054d-148">**Chave**: https_port</span><span class="sxs-lookup"><span data-stu-id="3054d-148">**Key**: https_port</span></span>  
<span data-ttu-id="3054d-149">**Tipo**: *string*</span><span class="sxs-lookup"><span data-stu-id="3054d-149">**Type**: *string*</span></span>  
<span data-ttu-id="3054d-150">**Padrão**: um valor padrão não está definido.</span><span class="sxs-lookup"><span data-stu-id="3054d-150">**Default**: A default value isn't set.</span></span>  
<span data-ttu-id="3054d-151">**Definido usando**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="3054d-151">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="3054d-152">**Variável de ambiente**: `<PREFIX_>HTTPS_PORT` (o prefixo é `ASPNETCORE_` ao usar o Host da Web.)</span><span class="sxs-lookup"><span data-stu-id="3054d-152">**Environment variable**: `<PREFIX_>HTTPS_PORT` (The prefix is `ASPNETCORE_` when using the Web Host.)</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("https_port", "8080")
```

> [!NOTE]
> <span data-ttu-id="3054d-153">A porta pode ser configurada indiretamente, definindo a URL com o `ASPNETCORE_URLS` variável de ambiente.</span><span class="sxs-lookup"><span data-stu-id="3054d-153">The port can be configured indirectly by setting the URL with the `ASPNETCORE_URLS` environment variable.</span></span> <span data-ttu-id="3054d-154">A variável de ambiente configura o servidor e, em seguida, o middleware indiretamente descobre a porta HTTPS por meio de `IServerAddressesFeature`.</span><span class="sxs-lookup"><span data-stu-id="3054d-154">The environment variable configures the server, and then the middleware indirectly discovers the HTTPS port via `IServerAddressesFeature`.</span></span>

<span data-ttu-id="3054d-155">Se nenhuma porta for definida:</span><span class="sxs-lookup"><span data-stu-id="3054d-155">If no port is set:</span></span>

* <span data-ttu-id="3054d-156">As solicitações não são redirecionadas.</span><span class="sxs-lookup"><span data-stu-id="3054d-156">Requests aren't redirected.</span></span>
* <span data-ttu-id="3054d-157">O middleware registra o aviso "Falha ao determinar a porta https para redirecionamento."</span><span class="sxs-lookup"><span data-stu-id="3054d-157">The middleware logs the warning "Failed to determine the https port for redirect."</span></span>

> [!NOTE]
> <span data-ttu-id="3054d-158">Uma alternativa ao uso de Middleware de redirecionamento de HTTPS (`UseHttpsRedirection`) é usar o Middleware de reconfiguração de URL (`AddRedirectToHttps`).</span><span class="sxs-lookup"><span data-stu-id="3054d-158">An alternative to using HTTPS Redirection Middleware (`UseHttpsRedirection`) is to use URL Rewriting Middleware (`AddRedirectToHttps`).</span></span> <span data-ttu-id="3054d-159">`AddRedirectToHttps` também pode definir o código de status e a porta quando o redirecionamento é executado.</span><span class="sxs-lookup"><span data-stu-id="3054d-159">`AddRedirectToHttps` can also set the status code and port when the redirect is executed.</span></span> <span data-ttu-id="3054d-160">Para obter mais informações, consulte [Middleware de reconfiguração de URL](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="3054d-160">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>
>
> <span data-ttu-id="3054d-161">Ao redirecionar para HTTPS sem a necessidade de regras de redirecionamento adicional, é recomendável usar o Middleware de redirecionamento de HTTPS (`UseHttpsRedirection`) descrito neste tópico.</span><span class="sxs-lookup"><span data-stu-id="3054d-161">When redirecting to HTTPS without the requirement for additional redirect rules, we recommend using HTTPS Redirection Middleware (`UseHttpsRedirection`) described in this topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="3054d-162">O [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) é usada para exigir HTTPS.</span><span class="sxs-lookup"><span data-stu-id="3054d-162">The [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require HTTPS.</span></span> <span data-ttu-id="3054d-163">`[RequireHttpsAttribute]` pode decorar controladores ou métodos, ou podem ser aplicadas globalmente.</span><span class="sxs-lookup"><span data-stu-id="3054d-163">`[RequireHttpsAttribute]` can decorate controllers or methods, or can be applied globally.</span></span> <span data-ttu-id="3054d-164">Para aplicar o atributo globalmente, adicione o seguinte código ao `ConfigureServices` em `Startup`:</span><span class="sxs-lookup"><span data-stu-id="3054d-164">To apply the attribute globally, add the following code to `ConfigureServices` in `Startup`:</span></span>

[!code-csharp[](~/security/authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

<span data-ttu-id="3054d-165">O código realçado anterior requer que todas as solicitações usem `HTTPS`; portanto, as solicitações HTTP são ignoradas.</span><span class="sxs-lookup"><span data-stu-id="3054d-165">The preceding highlighted code requires all requests use `HTTPS`; therefore, HTTP requests are ignored.</span></span> <span data-ttu-id="3054d-166">O seguinte código realçado redireciona todas as solicitações HTTP para HTTPS:</span><span class="sxs-lookup"><span data-stu-id="3054d-166">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

<span data-ttu-id="3054d-167">Para obter mais informações, consulte [Middleware de reconfiguração de URL](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="3054d-167">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="3054d-168">O middleware também permite que o aplicativo para definir o código de status ou o código de status e a porta quando o redirecionamento é executado.</span><span class="sxs-lookup"><span data-stu-id="3054d-168">The middleware also permits the app to set the status code or the status code and the port when the redirect is executed.</span></span>

<span data-ttu-id="3054d-169">Exigir HTTPS globalmente (`options.Filters.Add(new RequireHttpsAttribute());`) é uma prática recomendada de segurança.</span><span class="sxs-lookup"><span data-stu-id="3054d-169">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="3054d-170">Aplicando o `[RequireHttps]` atributo a todas as páginas do Razor/controladores não é considerado tão seguro quanto exigir HTTPS globalmente.</span><span class="sxs-lookup"><span data-stu-id="3054d-170">Applying the `[RequireHttps]` attribute to all controllers/Razor Pages isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="3054d-171">Você não pode garantir o `[RequireHttps]` atributo é aplicado quando novos controladores e páginas Razor são adicionadas.</span><span class="sxs-lookup"><span data-stu-id="3054d-171">You can't guarantee the `[RequireHttps]` attribute is applied when new controllers and Razor Pages are added.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="hsts"></a>
## <a name="http-strict-transport-security-protocol-hsts"></a><span data-ttu-id="3054d-172">Protocolo de segurança de transporte estrito HTTP (HSTS)</span><span class="sxs-lookup"><span data-stu-id="3054d-172">HTTP Strict Transport Security Protocol (HSTS)</span></span>

<span data-ttu-id="3054d-173">Por [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [segurança de transporte estrito HTTP (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) é um aprimoramento de segurança opcional é especificado por um aplicativo web com o uso de um cabeçalho de resposta.</span><span class="sxs-lookup"><span data-stu-id="3054d-173">Per [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) is an opt-in security enhancement that's specified by a web app through the use of a response header.</span></span> <span data-ttu-id="3054d-174">Quando um [navegador que ofereça suporte a HSTS](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support) recebe esse cabeçalho:</span><span class="sxs-lookup"><span data-stu-id="3054d-174">When a [browser that supports HSTS](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support) receives this header:</span></span>

* <span data-ttu-id="3054d-175">O navegador armazena a configuração para o domínio que evita o envio de qualquer comunicação por HTTP.</span><span class="sxs-lookup"><span data-stu-id="3054d-175">The browser stores configuration for the domain that prevents sending any communication over HTTP.</span></span> <span data-ttu-id="3054d-176">O navegador força todas as comunicações via HTTPS.</span><span class="sxs-lookup"><span data-stu-id="3054d-176">The browser forces all communication over HTTPS.</span></span>
* <span data-ttu-id="3054d-177">O navegador impede que o usuário usando os certificados não confiáveis ou é inválidos.</span><span class="sxs-lookup"><span data-stu-id="3054d-177">The browser prevents the user from using untrusted or invalid certificates.</span></span> <span data-ttu-id="3054d-178">O navegador desativa prompts que permitem que um usuário para confiar temporariamente esses certificados.</span><span class="sxs-lookup"><span data-stu-id="3054d-178">The browser disables prompts that allow a user to temporarily trust such a certificate.</span></span>

<span data-ttu-id="3054d-179">Como HSTS é imposta pelo cliente, ele tem algumas limitações:</span><span class="sxs-lookup"><span data-stu-id="3054d-179">Because HSTS is enforced by the client it has some limitations:</span></span>

* <span data-ttu-id="3054d-180">O cliente deve oferecer suporte a HSTS.</span><span class="sxs-lookup"><span data-stu-id="3054d-180">The client must support HSTS.</span></span>
* <span data-ttu-id="3054d-181">HSTS requer pelo menos uma solicitação HTTPS bem-sucedida para estabelecer a política HSTS.</span><span class="sxs-lookup"><span data-stu-id="3054d-181">HSTS requires at least one successful HTTPS request to establish the HSTS policy.</span></span>
* <span data-ttu-id="3054d-182">O aplicativo deve verificar todas as solicitações HTTP e redirecionar ou rejeitar a solicitação HTTP.</span><span class="sxs-lookup"><span data-stu-id="3054d-182">The application must check every HTTP request and redirect or reject the HTTP request.</span></span>

<span data-ttu-id="3054d-183">ASP.NET Core 2.1 ou posterior implementa HSTS com o `UseHsts` método de extensão.</span><span class="sxs-lookup"><span data-stu-id="3054d-183">ASP.NET Core 2.1 or later implements HSTS with the `UseHsts` extension method.</span></span> <span data-ttu-id="3054d-184">O código a seguir chama `UseHsts` quando o aplicativo não está no [modo de desenvolvimento](xref:fundamentals/environments):</span><span class="sxs-lookup"><span data-stu-id="3054d-184">The following code calls `UseHsts` when the app isn't in [development mode](xref:fundamentals/environments):</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]

<span data-ttu-id="3054d-185">`UseHsts` não é recomendado em desenvolvimento porque as configurações de HSTS são altamente armazenáveis em cache por navegadores.</span><span class="sxs-lookup"><span data-stu-id="3054d-185">`UseHsts` isn't recommended in development because the HSTS settings are highly cacheable by browsers.</span></span> <span data-ttu-id="3054d-186">Por padrão, `UseHsts` exclui o endereço de loopback local.</span><span class="sxs-lookup"><span data-stu-id="3054d-186">By default, `UseHsts` excludes the local loopback address.</span></span>

<span data-ttu-id="3054d-187">Para ambientes de produção implementando HTTPS pela primeira vez, defina o valor inicial de HSTS para um valor pequeno.</span><span class="sxs-lookup"><span data-stu-id="3054d-187">For production environments implementing HTTPS for the first time, set the initial HSTS value to a small value.</span></span> <span data-ttu-id="3054d-188">Defina o valor de horas como não mais do que um único dia caso você precise reverter a infraestrutura HTTPS para HTTP.</span><span class="sxs-lookup"><span data-stu-id="3054d-188">Set the value from hours to no more than a single day in case you need to revert the HTTPS infrastructure to HTTP.</span></span> <span data-ttu-id="3054d-189">Depois que você estiver confiante em sustentabilidade da configuração do HTTPS, aumente o valor de idade máxima HSTS; um valor comumente usado é um ano.</span><span class="sxs-lookup"><span data-stu-id="3054d-189">After you're confident in the sustainability of the HTTPS configuration, increase the HSTS max-age value; a commonly used value is one year.</span></span> 

<span data-ttu-id="3054d-190">O código a seguir:</span><span class="sxs-lookup"><span data-stu-id="3054d-190">The following code:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]

* <span data-ttu-id="3054d-191">Define o parâmetro de pré-carregamento do cabeçalho de segurança de transporte estrito.</span><span class="sxs-lookup"><span data-stu-id="3054d-191">Sets the preload parameter of the Strict-Transport-Security header.</span></span> <span data-ttu-id="3054d-192">Pré-carregamento não faz parte dos [especificação RFC HSTS](https://tools.ietf.org/html/rfc6797), mas é compatível com navegadores da web para pré-carregar sites HSTS na nova instalação.</span><span class="sxs-lookup"><span data-stu-id="3054d-192">Preload isn't part of the [RFC HSTS specification](https://tools.ietf.org/html/rfc6797), but is supported by web browsers to preload HSTS sites on fresh install.</span></span> <span data-ttu-id="3054d-193">Veja [https://hstspreload.org/](https://hstspreload.org/) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="3054d-193">See [https://hstspreload.org/](https://hstspreload.org/) for more information.</span></span>
* <span data-ttu-id="3054d-194">Habilita [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), que se aplica a política HSTS para hospedar subdomínios.</span><span class="sxs-lookup"><span data-stu-id="3054d-194">Enables [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), which applies the HSTS policy to Host subdomains.</span></span> 
* <span data-ttu-id="3054d-195">Define explicitamente o parâmetro de idade máxima do cabeçalho de segurança de transporte estrito para 60 dias.</span><span class="sxs-lookup"><span data-stu-id="3054d-195">Explicitly sets the max-age parameter of the Strict-Transport-Security header to 60 days.</span></span> <span data-ttu-id="3054d-196">Se não for definido, o padrão é 30 dias.</span><span class="sxs-lookup"><span data-stu-id="3054d-196">If not set, defaults to 30 days.</span></span> <span data-ttu-id="3054d-197">Consulte a [max-age diretiva](https://tools.ietf.org/html/rfc6797#section-6.1.1) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="3054d-197">See the [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) for more information.</span></span>
* <span data-ttu-id="3054d-198">Adiciona `example.com` à lista de hosts a serem excluídos.</span><span class="sxs-lookup"><span data-stu-id="3054d-198">Adds `example.com` to the list of hosts to exclude.</span></span>

<span data-ttu-id="3054d-199">`UseHsts` Exclui os seguintes hosts de loopback:</span><span class="sxs-lookup"><span data-stu-id="3054d-199">`UseHsts` excludes the following loopback hosts:</span></span>

* <span data-ttu-id="3054d-200">`localhost` : O endereço de loopback do IPv4.</span><span class="sxs-lookup"><span data-stu-id="3054d-200">`localhost` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="3054d-201">`127.0.0.1` : O endereço de loopback do IPv4.</span><span class="sxs-lookup"><span data-stu-id="3054d-201">`127.0.0.1` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="3054d-202">`[::1]` : O endereço de loopback do IPv6.</span><span class="sxs-lookup"><span data-stu-id="3054d-202">`[::1]` : The IPv6 loopback address.</span></span>

<span data-ttu-id="3054d-203">O exemplo anterior mostra como adicionar outros hosts.</span><span class="sxs-lookup"><span data-stu-id="3054d-203">The preceding example shows how to add additional hosts.</span></span>
::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="https"></a>
## <a name="opt-out-of-https-on-project-creation"></a><span data-ttu-id="3054d-204">Recusa de HTTPS na criação do projeto</span><span class="sxs-lookup"><span data-stu-id="3054d-204">Opt-out of HTTPS on project creation</span></span>

<span data-ttu-id="3054d-205">Os modelos de aplicativos do ASP.NET Core web 2.1 ou posterior (do Visual Studio ou a linha de comando do dotnet) habilitam [redirecionamento a HTTPS](#require) e [HSTS](#hsts).</span><span class="sxs-lookup"><span data-stu-id="3054d-205">The ASP.NET Core 2.1 or later web application templates (from Visual Studio or the dotnet command line) enable [HTTPS redirection](#require) and [HSTS](#hsts).</span></span> <span data-ttu-id="3054d-206">Para implantações que não exijam HTTPS, você pode recusar de HTTPS.</span><span class="sxs-lookup"><span data-stu-id="3054d-206">For deployments that don't require HTTPS, you can opt-out of HTTPS.</span></span> <span data-ttu-id="3054d-207">Por exemplo, alguns serviços de back-end em que HTTPS está sendo manipulado externamente na borda, usando HTTPS em cada nó não é necessária.</span><span class="sxs-lookup"><span data-stu-id="3054d-207">For example, some backend services where HTTPS is being handled externally at the edge, using HTTPS at each node isn't needed.</span></span>

<span data-ttu-id="3054d-208">A recusa de HTTPS:</span><span class="sxs-lookup"><span data-stu-id="3054d-208">To opt-out of HTTPS:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3054d-209">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3054d-209">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="3054d-210">Desmarque a **configurar para HTTPS** caixa de seleção.</span><span class="sxs-lookup"><span data-stu-id="3054d-210">Uncheck the **Configure for HTTPS** checkbox.</span></span>

![Diagrama de entidade](enforcing-ssl/_static/out.png)

#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="3054d-212">CLI do .NET Core</span><span class="sxs-lookup"><span data-stu-id="3054d-212">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="3054d-213">Use a opção `--no-https`.</span><span class="sxs-lookup"><span data-stu-id="3054d-213">Use the `--no-https` option.</span></span> <span data-ttu-id="3054d-214">Por exemplo</span><span class="sxs-lookup"><span data-stu-id="3054d-214">For example</span></span>

```console
dotnet new webapp --no-https
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="how-to-set-up-a-developer-certificate-for-docker"></a><span data-ttu-id="3054d-215">Como configurar um certificado de desenvolvedor para o Docker</span><span class="sxs-lookup"><span data-stu-id="3054d-215">How to set up a developer certificate for Docker</span></span>

<span data-ttu-id="3054d-216">Ver [esse problema de GitHub](https://github.com/aspnet/Docs/issues/6199).</span><span class="sxs-lookup"><span data-stu-id="3054d-216">See [this GitHub issue](https://github.com/aspnet/Docs/issues/6199).</span></span>

::: moniker-end

## <a name="additional-information"></a><span data-ttu-id="3054d-217">Informações adicionais</span><span class="sxs-lookup"><span data-stu-id="3054d-217">Additional information</span></span>

* [<span data-ttu-id="3054d-218">Suporte a navegador HSTS OWASP</span><span class="sxs-lookup"><span data-stu-id="3054d-218">OWASP HSTS browser support</span></span>](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support)

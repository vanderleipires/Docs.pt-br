---
title: Impor HTTPS no ASP.NET Core
author: rick-anderson
description: Mostra como exigir HTTPS/TLS em um ASP.NET Core em aplicativo web.
ms.author: riande
ms.date: 2/9/2018
uid: security/enforcing-ssl
ms.openlocfilehash: c3d92994c0331b1408e246953454910ca1f4dc43
ms.sourcegitcommit: c8e62aa766641aa55105f7db79cdf2b27a6e5977
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39254825"
---
# <a name="enforce-https-in-aspnet-core"></a><span data-ttu-id="90b11-103">Impor HTTPS no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="90b11-103">Enforce HTTPS in ASP.NET Core</span></span>

<span data-ttu-id="90b11-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="90b11-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="90b11-105">Este documento demonstra como:</span><span class="sxs-lookup"><span data-stu-id="90b11-105">This document shows how to:</span></span>

* <span data-ttu-id="90b11-106">Exigir HTTPS para todas as solicitações.</span><span class="sxs-lookup"><span data-stu-id="90b11-106">Require HTTPS for all requests.</span></span>
* <span data-ttu-id="90b11-107">Redirecione todas as solicitações HTTP para HTTPS.</span><span class="sxs-lookup"><span data-stu-id="90b11-107">Redirect all HTTP requests to HTTPS.</span></span>

> [!WARNING]
> <span data-ttu-id="90b11-108">Fazer **não** usar [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) em APIs da Web que recebe informações confidenciais.</span><span class="sxs-lookup"><span data-stu-id="90b11-108">Do **not** use [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="90b11-109">`RequireHttpsAttribute` usa códigos de status HTTP para redirecionar navegadores de HTTP para HTTPS.</span><span class="sxs-lookup"><span data-stu-id="90b11-109">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="90b11-110">Os clientes da API não podem compreender ou obedecem redirecionamentos de HTTP para HTTPS.</span><span class="sxs-lookup"><span data-stu-id="90b11-110">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="90b11-111">Esses clientes podem enviar informações por meio de HTTP.</span><span class="sxs-lookup"><span data-stu-id="90b11-111">Such clients may send information over HTTP.</span></span> <span data-ttu-id="90b11-112">As APIs da Web deverá:</span><span class="sxs-lookup"><span data-stu-id="90b11-112">Web APIs should either:</span></span>
>
> * <span data-ttu-id="90b11-113">Não realizar a escuta em HTTP.</span><span class="sxs-lookup"><span data-stu-id="90b11-113">Not listen on HTTP.</span></span>
> * <span data-ttu-id="90b11-114">Feche a conexão com o código de status 400 (solicitação incorreta) e não atender à solicitação.</span><span class="sxs-lookup"><span data-stu-id="90b11-114">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

<a name="require"></a>
## <a name="require-https"></a><span data-ttu-id="90b11-115">Exigir HTTPS</span><span class="sxs-lookup"><span data-stu-id="90b11-115">Require HTTPS</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="90b11-116">É recomendável que todos os aplicativos do ASP.NET Core web chamar Middleware de redirecionamento de HTTPS ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) para redirecionar todas as solicitações HTTP para HTTPS.</span><span class="sxs-lookup"><span data-stu-id="90b11-116">We recommend all ASP.NET Core web apps call HTTPS Redirection Middleware ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) to redirect all HTTP requests to HTTPS.</span></span>

<span data-ttu-id="90b11-117">O código a seguir chama `UseHttpsRedirection` no `Startup` classe:</span><span class="sxs-lookup"><span data-stu-id="90b11-117">The following code calls `UseHttpsRedirection` in the `Startup` class:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]

<span data-ttu-id="90b11-118">O código realçado anterior:</span><span class="sxs-lookup"><span data-stu-id="90b11-118">The preceding highlighted code:</span></span>

* <span data-ttu-id="90b11-119">Usa o padrão [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) (`Status307TemporaryRedirect`).</span><span class="sxs-lookup"><span data-stu-id="90b11-119">Uses the default [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) (`Status307TemporaryRedirect`).</span></span> <span data-ttu-id="90b11-120">Aplicativos de produção devem chamar [UseHsts](#hsts).</span><span class="sxs-lookup"><span data-stu-id="90b11-120">Production apps should call [UseHsts](#hsts).</span></span>
* <span data-ttu-id="90b11-121">Usa o padrão [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (443).</span><span class="sxs-lookup"><span data-stu-id="90b11-121">Uses the default [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (443).</span></span>

<span data-ttu-id="90b11-122">O código a seguir chama [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) para configurar as opções de middleware:</span><span class="sxs-lookup"><span data-stu-id="90b11-122">The following code calls [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) to configure middleware options:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]

<span data-ttu-id="90b11-123">O código realçado anterior:</span><span class="sxs-lookup"><span data-stu-id="90b11-123">The preceding highlighted code:</span></span>

* <span data-ttu-id="90b11-124">Conjuntos [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) para `Status307TemporaryRedirect`, que é o valor padrão.</span><span class="sxs-lookup"><span data-stu-id="90b11-124">Sets [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) to `Status307TemporaryRedirect`, which is the default value.</span></span> <span data-ttu-id="90b11-125">Aplicativos de produção devem chamar [UseHsts](#hsts).</span><span class="sxs-lookup"><span data-stu-id="90b11-125">Production apps should call [UseHsts](#hsts).</span></span>
* <span data-ttu-id="90b11-126">Define a porta HTTPS para 5001.</span><span class="sxs-lookup"><span data-stu-id="90b11-126">Sets the HTTPS port to 5001.</span></span> <span data-ttu-id="90b11-127">O valor padrão é 443.</span><span class="sxs-lookup"><span data-stu-id="90b11-127">The default value is 443.</span></span>

<span data-ttu-id="90b11-128">Os seguintes mecanismos de definir a porta automaticamente:</span><span class="sxs-lookup"><span data-stu-id="90b11-128">The following mechanisms set the port automatically:</span></span>

* <span data-ttu-id="90b11-129">O middleware pode descobrir as portas por meio [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) quando as condições a seguir se aplicam:</span><span class="sxs-lookup"><span data-stu-id="90b11-129">The middleware can discover the ports via [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) when the following conditions apply:</span></span>
  - <span data-ttu-id="90b11-130">O kestrel ou HTTP. sys é usada diretamente com pontos de extremidade HTTPS (também se aplica a execução do aplicativo com o depurador do Visual Studio Code).</span><span class="sxs-lookup"><span data-stu-id="90b11-130">Kestrel or HTTP.sys is used directly with HTTPS endpoints (also applies to running the app with Visual Studio Code's debugger).</span></span>
  - <span data-ttu-id="90b11-131">Somente **uma porta HTTPS** é usado pelo aplicativo.</span><span class="sxs-lookup"><span data-stu-id="90b11-131">Only **one HTTPS port** is used by the app.</span></span>
* <span data-ttu-id="90b11-132">O Visual Studio é usado:</span><span class="sxs-lookup"><span data-stu-id="90b11-132">Visual Studio is used:</span></span>
  - <span data-ttu-id="90b11-133">O IIS Express tem habilitadas para HTTPS.</span><span class="sxs-lookup"><span data-stu-id="90b11-133">IIS Express has HTTPS enabled.</span></span>
  - <span data-ttu-id="90b11-134">*launchsettings. JSON* define o `sslPort` para o IIS Express.</span><span class="sxs-lookup"><span data-stu-id="90b11-134">*launchSettings.json* sets the `sslPort` for IIS Express.</span></span>

> [!NOTE]
> <span data-ttu-id="90b11-135">Quando um aplicativo é executado atrás de um proxy reverso (por exemplo, IIS, IIS Express), `IServerAddressesFeature` não está disponível.</span><span class="sxs-lookup"><span data-stu-id="90b11-135">When an app is run behind a reverse proxy (for example, IIS, IIS Express), `IServerAddressesFeature` isn't available.</span></span> <span data-ttu-id="90b11-136">A porta deve ser configurada manualmente.</span><span class="sxs-lookup"><span data-stu-id="90b11-136">The port must be manually configured.</span></span> <span data-ttu-id="90b11-137">Quando a porta não for definida, as solicitações não são redirecionadas.</span><span class="sxs-lookup"><span data-stu-id="90b11-137">When the port isn't set, requests aren't redirected.</span></span>

<span data-ttu-id="90b11-138">A porta pode ser configurada definindo a:</span><span class="sxs-lookup"><span data-stu-id="90b11-138">The port can be configured by setting the:</span></span>

* <span data-ttu-id="90b11-139">A variável de ambiente `ASPNETCORE_HTTPS_PORT`.</span><span class="sxs-lookup"><span data-stu-id="90b11-139">`ASPNETCORE_HTTPS_PORT` environment variable.</span></span>
* <span data-ttu-id="90b11-140">`http_port` chave de configuração de host (por exemplo, via *hostsettings.json* ou um argumento de linha de comando).</span><span class="sxs-lookup"><span data-stu-id="90b11-140">`http_port` host configuration key (for example, via *hostsettings.json* or a command line argument).</span></span>
* <span data-ttu-id="90b11-141">[HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport).</span><span class="sxs-lookup"><span data-stu-id="90b11-141">[HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport).</span></span> <span data-ttu-id="90b11-142">Consulte o exemplo anterior que mostra como definir a porta para 5001.</span><span class="sxs-lookup"><span data-stu-id="90b11-142">See the preceding example that shows how to set the port to 5001.</span></span>

> [!NOTE]
> <span data-ttu-id="90b11-143">A porta pode ser configurada indiretamente, definindo a URL com o `ASPNETCORE_URLS` variável de ambiente.</span><span class="sxs-lookup"><span data-stu-id="90b11-143">The port can be configured indirectly by setting the URL with the `ASPNETCORE_URLS` environment variable.</span></span> <span data-ttu-id="90b11-144">A variável de ambiente configura o servidor e, em seguida, o middleware indiretamente descobre a porta HTTPS por meio de `IServerAddressesFeature`.</span><span class="sxs-lookup"><span data-stu-id="90b11-144">The environment variable configures the server, and then the middleware indirectly discovers the HTTPS port via `IServerAddressesFeature`.</span></span>

<span data-ttu-id="90b11-145">Se nenhuma porta for definida:</span><span class="sxs-lookup"><span data-stu-id="90b11-145">If no port is set:</span></span>

* <span data-ttu-id="90b11-146">As solicitações não são redirecionadas.</span><span class="sxs-lookup"><span data-stu-id="90b11-146">Requests aren't redirected.</span></span>
* <span data-ttu-id="90b11-147">O middleware registra um aviso.</span><span class="sxs-lookup"><span data-stu-id="90b11-147">The middleware logs a warning.</span></span>

> [!NOTE]
> <span data-ttu-id="90b11-148">Uma alternativa ao uso de Middleware de redirecionamento de HTTPS (`UseHttpsRedirection`) é usar o Middleware de reconfiguração de URL (`AddRedirectToHttps`).</span><span class="sxs-lookup"><span data-stu-id="90b11-148">An alternative to using HTTPS Redirection Middleware (`UseHttpsRedirection`) is to use URL Rewriting Middleware (`AddRedirectToHttps`).</span></span> <span data-ttu-id="90b11-149">`AddRedirectToHttps` também pode definir o código de status e a porta quando o redirecionamento é executado.</span><span class="sxs-lookup"><span data-stu-id="90b11-149">`AddRedirectToHttps` can also set the status code and port when the redirect is executed.</span></span> <span data-ttu-id="90b11-150">Para obter mais informações, consulte [Middleware de reconfiguração de URL](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="90b11-150">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>
>
> <span data-ttu-id="90b11-151">Ao redirecionar para HTTPS sem a necessidade de regras de redirecionamento adicional, é recomendável usar o Middleware de redirecionamento de HTTPS (`UseHttpsRedirection`) descrito neste tópico.</span><span class="sxs-lookup"><span data-stu-id="90b11-151">When redirecting to HTTPS without the requirement for additional redirect rules, we recommend using HTTPS Redirection Middleware (`UseHttpsRedirection`) described in this topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="90b11-152">O [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) é usada para exigir HTTPS.</span><span class="sxs-lookup"><span data-stu-id="90b11-152">The [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require HTTPS.</span></span> <span data-ttu-id="90b11-153">`[RequireHttpsAttribute]` pode decorar controladores ou métodos, ou podem ser aplicadas globalmente.</span><span class="sxs-lookup"><span data-stu-id="90b11-153">`[RequireHttpsAttribute]` can decorate controllers or methods, or can be applied globally.</span></span> <span data-ttu-id="90b11-154">Para aplicar o atributo globalmente, adicione o seguinte código ao `ConfigureServices` em `Startup`:</span><span class="sxs-lookup"><span data-stu-id="90b11-154">To apply the attribute globally, add the following code to `ConfigureServices` in `Startup`:</span></span>

[!code-csharp[](~/security/authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

<span data-ttu-id="90b11-155">O código realçado anterior requer que todas as solicitações usem `HTTPS`; portanto, as solicitações HTTP são ignoradas.</span><span class="sxs-lookup"><span data-stu-id="90b11-155">The preceding highlighted code requires all requests use `HTTPS`; therefore, HTTP requests are ignored.</span></span> <span data-ttu-id="90b11-156">O seguinte código realçado redireciona todas as solicitações HTTP para HTTPS:</span><span class="sxs-lookup"><span data-stu-id="90b11-156">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

<span data-ttu-id="90b11-157">Para obter mais informações, consulte [Middleware de reconfiguração de URL](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="90b11-157">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="90b11-158">O middleware também permite que o aplicativo para definir o código de status ou o código de status e a porta quando o redirecionamento é executado.</span><span class="sxs-lookup"><span data-stu-id="90b11-158">The middleware also permits the app to set the status code or the status code and the port when the redirect is executed.</span></span>

<span data-ttu-id="90b11-159">Exigir HTTPS globalmente (`options.Filters.Add(new RequireHttpsAttribute());`) é uma prática recomendada de segurança.</span><span class="sxs-lookup"><span data-stu-id="90b11-159">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="90b11-160">Aplicando o `[RequireHttps]` atributo a todas as páginas do Razor/controladores não é considerado tão seguro quanto exigir HTTPS globalmente.</span><span class="sxs-lookup"><span data-stu-id="90b11-160">Applying the `[RequireHttps]` attribute to all controllers/Razor Pages isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="90b11-161">Você não pode garantir o `[RequireHttps]` atributo é aplicado quando novos controladores e páginas Razor são adicionadas.</span><span class="sxs-lookup"><span data-stu-id="90b11-161">You can't guarantee the `[RequireHttps]` attribute is applied when new controllers and Razor Pages are added.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="hsts"></a>
## <a name="http-strict-transport-security-protocol-hsts"></a><span data-ttu-id="90b11-162">Protocolo de segurança de transporte estrito HTTP (HSTS)</span><span class="sxs-lookup"><span data-stu-id="90b11-162">HTTP Strict Transport Security Protocol (HSTS)</span></span>

<span data-ttu-id="90b11-163">Por [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [segurança de transporte estrito HTTP (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) é um aprimoramento de segurança opcional que é especificado por um aplicativo web com o uso de um cabeçalho de resposta especial.</span><span class="sxs-lookup"><span data-stu-id="90b11-163">Per [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) is an opt-in security enhancement that is specified by a web application through the use of a special response header.</span></span> <span data-ttu-id="90b11-164">Depois que um navegador com suporte recebe esse cabeçalho nesse navegador impedirá todas as comunicações sejam enviadas pelo HTTP para o domínio especificado e envia todas as comunicações via HTTPS.</span><span class="sxs-lookup"><span data-stu-id="90b11-164">Once a supported browser receives this header that browser will prevent any communications from being sent over HTTP to the specified domain and will instead send all communications over HTTPS.</span></span> <span data-ttu-id="90b11-165">Ele também evita via HTTPS em prompts em navegadores.</span><span class="sxs-lookup"><span data-stu-id="90b11-165">It also prevents HTTPS click through prompts on browsers.</span></span>

<span data-ttu-id="90b11-166">ASP.NET Core 2.1 ou posterior implementa HSTS com o `UseHsts` método de extensão.</span><span class="sxs-lookup"><span data-stu-id="90b11-166">ASP.NET Core 2.1 or later implements HSTS with the `UseHsts` extension method.</span></span> <span data-ttu-id="90b11-167">O código a seguir chama `UseHsts` quando o aplicativo não está no [modo de desenvolvimento](xref:fundamentals/environments):</span><span class="sxs-lookup"><span data-stu-id="90b11-167">The following code calls `UseHsts` when the app isn't in [development mode](xref:fundamentals/environments):</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]

<span data-ttu-id="90b11-168">`UseHsts` não é recomendado em desenvolvimento porque o cabeçalho HSTS é altamente armazenável em cache por navegadores.</span><span class="sxs-lookup"><span data-stu-id="90b11-168">`UseHsts` isn't recommended in development because the HSTS header is highly cacheable by browsers.</span></span> <span data-ttu-id="90b11-169">Por padrão, `UseHsts` exclui o endereço de loopback local.</span><span class="sxs-lookup"><span data-stu-id="90b11-169">By default, `UseHsts` excludes the local loopback address.</span></span>

<span data-ttu-id="90b11-170">O código a seguir:</span><span class="sxs-lookup"><span data-stu-id="90b11-170">The following code:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]

* <span data-ttu-id="90b11-171">Define o parâmetro de pré-carregamento do cabeçalho de segurança de transporte estrito.</span><span class="sxs-lookup"><span data-stu-id="90b11-171">Sets the preload parameter of the Strict-Transport-Security header.</span></span> <span data-ttu-id="90b11-172">Preload não é parte do [especificação RFC HSTS](https://tools.ietf.org/html/rfc6797), mas é compatível com navegadores da web para pré-carregar sites HSTS na nova instalação.</span><span class="sxs-lookup"><span data-stu-id="90b11-172">Preload is not part of the [RFC HSTS specification](https://tools.ietf.org/html/rfc6797), but is supported by web browsers to preload HSTS sites on fresh install.</span></span> <span data-ttu-id="90b11-173">Veja [https://hstspreload.org/](https://hstspreload.org/) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="90b11-173">See [https://hstspreload.org/](https://hstspreload.org/) for more information.</span></span>
* <span data-ttu-id="90b11-174">Habilita [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), que se aplica a política HSTS para hospedar subdomínios.</span><span class="sxs-lookup"><span data-stu-id="90b11-174">Enables [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), which applies the HSTS policy to Host subdomains.</span></span> 
* <span data-ttu-id="90b11-175">Define explicitamente o parâmetro de idade máxima do cabeçalho de segurança de transporte estrito para 60 dias.</span><span class="sxs-lookup"><span data-stu-id="90b11-175">Explicitly sets the max-age parameter of the Strict-Transport-Security header to to 60 days.</span></span> <span data-ttu-id="90b11-176">Se não for definido, o padrão é 30 dias.</span><span class="sxs-lookup"><span data-stu-id="90b11-176">If not set, defaults to 30 days.</span></span> <span data-ttu-id="90b11-177">Consulte a [max-age diretiva](https://tools.ietf.org/html/rfc6797#section-6.1.1) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="90b11-177">See the [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) for more information.</span></span>
* <span data-ttu-id="90b11-178">Adiciona `example.com` à lista de hosts a serem excluídos.</span><span class="sxs-lookup"><span data-stu-id="90b11-178">Adds `example.com` to the list of hosts to exclude.</span></span>

<span data-ttu-id="90b11-179">`UseHsts` Exclui os seguintes hosts de loopback:</span><span class="sxs-lookup"><span data-stu-id="90b11-179">`UseHsts` excludes the following loopback hosts:</span></span>

* <span data-ttu-id="90b11-180">`localhost` : O endereço de loopback do IPv4.</span><span class="sxs-lookup"><span data-stu-id="90b11-180">`localhost` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="90b11-181">`127.0.0.1` : O endereço de loopback do IPv4.</span><span class="sxs-lookup"><span data-stu-id="90b11-181">`127.0.0.1` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="90b11-182">`[::1]` : O endereço de loopback do IPv6.</span><span class="sxs-lookup"><span data-stu-id="90b11-182">`[::1]` : The IPv6 loopback address.</span></span>

<span data-ttu-id="90b11-183">O exemplo anterior mostra como adicionar outros hosts.</span><span class="sxs-lookup"><span data-stu-id="90b11-183">The preceding example shows how to add additional hosts.</span></span>
::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="https"></a>
## <a name="opt-out-of-https-on-project-creation"></a><span data-ttu-id="90b11-184">Recusa de HTTPS na criação do projeto</span><span class="sxs-lookup"><span data-stu-id="90b11-184">Opt-out of HTTPS on project creation</span></span>

<span data-ttu-id="90b11-185">Os modelos de aplicativos do ASP.NET Core web 2.1 ou posterior (do Visual Studio ou a linha de comando do dotnet) habilitam [redirecionamento a HTTPS](#require) e [HSTS](#hsts).</span><span class="sxs-lookup"><span data-stu-id="90b11-185">The ASP.NET Core 2.1 or later web application templates (from Visual Studio or the dotnet command line) enable [HTTPS redirection](#require) and [HSTS](#hsts).</span></span> <span data-ttu-id="90b11-186">Para implantações que não exijam HTTPS, você pode recusar de HTTPS.</span><span class="sxs-lookup"><span data-stu-id="90b11-186">For deployments that don't require HTTPS, you can opt-out of HTTPS.</span></span> <span data-ttu-id="90b11-187">Por exemplo, alguns serviços de back-end em que HTTPS está sendo manipulado externamente na borda, usando HTTPS em cada nó não é necessária.</span><span class="sxs-lookup"><span data-stu-id="90b11-187">For example, some backend services where HTTPS is being handled externally at the edge, using HTTPS at each node is not needed.</span></span>

<span data-ttu-id="90b11-188">A recusa de HTTPS:</span><span class="sxs-lookup"><span data-stu-id="90b11-188">To opt-out of HTTPS:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="90b11-189">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="90b11-189">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="90b11-190">Desmarque a **configurar para HTTPS** caixa de seleção.</span><span class="sxs-lookup"><span data-stu-id="90b11-190">Uncheck the **Configure for HTTPS** checkbox.</span></span>

![Diagrama de entidade](enforcing-ssl/_static/out.png)

#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="90b11-192">CLI do .NET Core</span><span class="sxs-lookup"><span data-stu-id="90b11-192">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="90b11-193">Use a opção `--no-https`.</span><span class="sxs-lookup"><span data-stu-id="90b11-193">Use the `--no-https` option.</span></span> <span data-ttu-id="90b11-194">Por exemplo</span><span class="sxs-lookup"><span data-stu-id="90b11-194">For example</span></span>

```console
dotnet new webapp --no-https
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="how-to-setup-a-developer-certificate-for-docker"></a><span data-ttu-id="90b11-195">Como configurar um certificado de desenvolvedor para o Docker</span><span class="sxs-lookup"><span data-stu-id="90b11-195">How to setup a developer certificate for Docker</span></span>

<span data-ttu-id="90b11-196">Ver [esse problema de GitHub](https://github.com/aspnet/Docs/issues/6199).</span><span class="sxs-lookup"><span data-stu-id="90b11-196">See [this GitHub issue](https://github.com/aspnet/Docs/issues/6199).</span></span>

::: moniker-end

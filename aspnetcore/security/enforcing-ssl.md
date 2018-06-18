---
title: Impor HTTPS no núcleo do ASP.NET
author: rick-anderson
description: Mostra como exigir HTTPS/TLS em um núcleo de ASP.NET aplicativo web.
manager: wpickett
ms.author: riande
ms.date: 2/9/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/enforcing-ssl
ms.openlocfilehash: f49a7846149385125390285e2f1332d8e40642c0
ms.sourcegitcommit: 9a35906446af7ffd4ccfc18daec38874b5abbef7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/18/2018
ms.locfileid: "35725930"
---
# <a name="enforce-https-in-aspnet-core"></a><span data-ttu-id="ffa7f-103">Impor HTTPS no núcleo do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="ffa7f-103">Enforce HTTPS in ASP.NET Core</span></span>

<span data-ttu-id="ffa7f-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ffa7f-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="ffa7f-105">Este documento demonstra como:</span><span class="sxs-lookup"><span data-stu-id="ffa7f-105">This document shows how to:</span></span>

* <span data-ttu-id="ffa7f-106">Exigir HTTPS para todas as solicitações.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-106">Require HTTPS for all requests.</span></span>
* <span data-ttu-id="ffa7f-107">Redirecione todas as solicitações HTTP para HTTPS.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-107">Redirect all HTTP requests to HTTPS.</span></span>

> [!WARNING]
> <span data-ttu-id="ffa7f-108">Fazer **não** usar [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) em APIs da Web que recebe informações confidenciais.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-108">Do **not** use [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="ffa7f-109">`RequireHttpsAttribute` usa códigos de status HTTP para redirecionar navegadores de HTTP para HTTPS.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-109">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="ffa7f-110">Clientes de API podem não entender ou obedecer redirecionamentos de HTTP para HTTPS.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-110">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="ffa7f-111">Esses clientes podem enviar informações sobre HTTP.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-111">Such clients may send information over HTTP.</span></span> <span data-ttu-id="ffa7f-112">APIs da Web deverá:</span><span class="sxs-lookup"><span data-stu-id="ffa7f-112">Web APIs should either:</span></span>
>
> * <span data-ttu-id="ffa7f-113">Não escuta no HTTP.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-113">Not listen on HTTP.</span></span>
> * <span data-ttu-id="ffa7f-114">Feche a conexão com o código de status 400 (solicitação incorreta) e não atender à solicitação.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-114">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

<a name="require"></a>
## <a name="require-https"></a><span data-ttu-id="ffa7f-115">Exigir HTTPS</span><span class="sxs-lookup"><span data-stu-id="ffa7f-115">Require HTTPS</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="ffa7f-116">É recomendável que todos os aplicativos web do ASP.NET Core chamar Middleware de redirecionamento de HTTPS ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) para redirecionar todas as solicitações HTTP para HTTPS.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-116">We recommend all ASP.NET Core web apps call HTTPS Redirection Middleware ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) to redirect all HTTP requests to HTTPS.</span></span>

<span data-ttu-id="ffa7f-117">O código a seguir chama `UseHttpsRedirection` no `Startup` classe:</span><span class="sxs-lookup"><span data-stu-id="ffa7f-117">The following code calls `UseHttpsRedirection` in the `Startup` class:</span></span>

<span data-ttu-id="ffa7f-118">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]</span><span class="sxs-lookup"><span data-stu-id="ffa7f-118">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]</span></span>

<span data-ttu-id="ffa7f-119">O código a seguir chama [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) para configurar as opções de middleware:</span><span class="sxs-lookup"><span data-stu-id="ffa7f-119">The following code calls [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) to configure middleware options:</span></span>

<span data-ttu-id="ffa7f-120">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]</span><span class="sxs-lookup"><span data-stu-id="ffa7f-120">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]</span></span>

<span data-ttu-id="ffa7f-121">O código realçado acima:</span><span class="sxs-lookup"><span data-stu-id="ffa7f-121">The preceding highlighted code:</span></span>

* <span data-ttu-id="ffa7f-122">Conjuntos de [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) para `Status307TemporaryRedirect`, que é o valor padrão.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-122">Sets [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) to `Status307TemporaryRedirect`, which is the default value.</span></span> <span data-ttu-id="ffa7f-123">Aplicativos de produção devem chamar [UseHsts](#hsts).</span><span class="sxs-lookup"><span data-stu-id="ffa7f-123">Production apps should call [UseHsts](#hsts).</span></span>
* <span data-ttu-id="ffa7f-124">Define a porta HTTPS para 5001.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-124">Sets the HTTPS port to 5001.</span></span> <span data-ttu-id="ffa7f-125">O valor padrão é 443.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-125">The default value is 443.</span></span>

<span data-ttu-id="ffa7f-126">Os seguintes mecanismos defina a porta automaticamente:</span><span class="sxs-lookup"><span data-stu-id="ffa7f-126">The following mechanisms set the port automatically:</span></span>

* <span data-ttu-id="ffa7f-127">O middleware pode descobrir as portas por meio de [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) quando as seguintes condições se aplicam:</span><span class="sxs-lookup"><span data-stu-id="ffa7f-127">The middleware can discover the ports via [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) when the following conditions apply:</span></span>
  - <span data-ttu-id="ffa7f-128">Kestrel ou HTTP. sys é usado diretamente com pontos de extremidade HTTPS (também se aplica ao executar o aplicativo com o depurador do Visual Studio Code).</span><span class="sxs-lookup"><span data-stu-id="ffa7f-128">Kestrel or HTTP.sys is used directly with HTTPS endpoints (also applies to running the app with Visual Studio Code's debugger).</span></span>
  - <span data-ttu-id="ffa7f-129">Somente **uma porta HTTPS** é usada pelo aplicativo.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-129">Only **one HTTPS port** is used by the app.</span></span>
* <span data-ttu-id="ffa7f-130">O Visual Studio é usado:</span><span class="sxs-lookup"><span data-stu-id="ffa7f-130">Visual Studio is used:</span></span>
  - <span data-ttu-id="ffa7f-131">O IIS Express tem habilitadas para HTTPS.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-131">IIS Express has HTTPS enabled.</span></span>
  - <span data-ttu-id="ffa7f-132">*launchSettings.json* define o `sslPort` para IIS Express.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-132">*launchSettings.json* sets the `sslPort` for IIS Express.</span></span>

> [!NOTE]
> <span data-ttu-id="ffa7f-133">Quando um aplicativo é executado por trás de um proxy reverso (por exemplo, IIS, IIS Express), `IServerAddressesFeature` não está disponível.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-133">When an app is run behind a reverse proxy (for example, IIS, IIS Express), `IServerAddressesFeature` isn't available.</span></span> <span data-ttu-id="ffa7f-134">A porta deve ser configurada manualmente.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-134">The port must be manually configured.</span></span> <span data-ttu-id="ffa7f-135">Quando a porta não for definida, as solicitações não são redirecionadas.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-135">When the port isn't set, requests aren't redirected.</span></span>

<span data-ttu-id="ffa7f-136">A porta pode ser configurada definindo o:</span><span class="sxs-lookup"><span data-stu-id="ffa7f-136">The port can be configured by setting the:</span></span>

* <span data-ttu-id="ffa7f-137">A variável de ambiente `ASPNETCORE_HTTPS_PORT`.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-137">`ASPNETCORE_HTTPS_PORT` environment variable.</span></span>
* <span data-ttu-id="ffa7f-138">`http_port` chave de configuração de host (por exemplo, via *hostsettings.json* ou um argumento de linha de comando).</span><span class="sxs-lookup"><span data-stu-id="ffa7f-138">`http_port` host configuration key (for example, via *hostsettings.json* or a command line argument).</span></span>
* <span data-ttu-id="ffa7f-139">[HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport).</span><span class="sxs-lookup"><span data-stu-id="ffa7f-139">[HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport).</span></span> <span data-ttu-id="ffa7f-140">Consulte o exemplo anterior que mostra como definir a porta como 5001.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-140">See the preceding example that shows how to set the port to 5001.</span></span>

> [!NOTE]
> <span data-ttu-id="ffa7f-141">A porta pode ser configurada indiretamente definindo a URL com o `ASPNETCORE_URLS` variável de ambiente.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-141">The port can be configured indirectly by setting the URL with the `ASPNETCORE_URLS` environment variable.</span></span> <span data-ttu-id="ffa7f-142">A variável de ambiente configura o servidor e, em seguida, o middleware indiretamente descobre a porta HTTPS por meio de `IServerAddressesFeature`.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-142">The environment variable configures the server, and then the middleware indirectly discovers the HTTPS port via `IServerAddressesFeature`.</span></span>

<span data-ttu-id="ffa7f-143">Se a porta não está definida:</span><span class="sxs-lookup"><span data-stu-id="ffa7f-143">If no port is set:</span></span>

* <span data-ttu-id="ffa7f-144">As solicitações não são redirecionadas.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-144">Requests aren't redirected.</span></span>
* <span data-ttu-id="ffa7f-145">O middleware registra um aviso.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-145">The middleware logs a warning.</span></span>

> [!NOTE]
> <span data-ttu-id="ffa7f-146">Uma alternativa ao uso de Middleware de redirecionamento de HTTPS (`UseHttpsRedirection`) é usar o Middleware de regravação de URL (`AddRedirectToHttps`).</span><span class="sxs-lookup"><span data-stu-id="ffa7f-146">An alternative to using HTTPS Redirection Middleware (`UseHttpsRedirection`) is to use URL Rewriting Middleware (`AddRedirectToHttps`).</span></span> <span data-ttu-id="ffa7f-147">`AddRedirectToHttps` também pode definir o código de status e a porta quando o redirecionamento é executado.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-147">`AddRedirectToHttps` can also set the status code and port when the redirect is executed.</span></span> <span data-ttu-id="ffa7f-148">Para obter mais informações, consulte [Middleware de regravação de URL](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="ffa7f-148">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>
>
> <span data-ttu-id="ffa7f-149">Ao redirecionar para HTTPS sem a necessidade de regras de redirecionamento adicionais, é recomendável usar HTTPS redirecionamento Middleware (`UseHttpsRedirection`) descrito neste tópico.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-149">When redirecting to HTTPS without the requirement for additional redirect rules, we recommend using HTTPS Redirection Middleware (`UseHttpsRedirection`) described in this topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="ffa7f-150">O [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) é usada para exigir HTTPS.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-150">The [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require HTTPS.</span></span> <span data-ttu-id="ffa7f-151">`[RequireHttpsAttribute]` pode decorar controladores ou métodos, ou podem ser aplicadas globalmente.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-151">`[RequireHttpsAttribute]` can decorate controllers or methods, or can be applied globally.</span></span> <span data-ttu-id="ffa7f-152">Para aplicar o atributo global, adicione o seguinte código ao `ConfigureServices` em `Startup`:</span><span class="sxs-lookup"><span data-stu-id="ffa7f-152">To apply the attribute globally, add the following code to `ConfigureServices` in `Startup`:</span></span>

<span data-ttu-id="ffa7f-153">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]</span><span class="sxs-lookup"><span data-stu-id="ffa7f-153">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]</span></span>

<span data-ttu-id="ffa7f-154">O código realçado anterior requer todas as solicitações usar `HTTPS`; portanto, as solicitações HTTP são ignoradas.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-154">The preceding highlighted code requires all requests use `HTTPS`; therefore, HTTP requests are ignored.</span></span> <span data-ttu-id="ffa7f-155">O seguinte código realçado redireciona todas as solicitações HTTP para HTTPS:</span><span class="sxs-lookup"><span data-stu-id="ffa7f-155">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

<span data-ttu-id="ffa7f-156">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]</span><span class="sxs-lookup"><span data-stu-id="ffa7f-156">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]</span></span>

<span data-ttu-id="ffa7f-157">Para obter mais informações, consulte [Middleware de regravação de URL](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="ffa7f-157">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="ffa7f-158">O middleware também permite que o aplicativo para definir o código de status ou o código de status e a porta quando o redirecionamento é executado.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-158">The middleware also permits the app to set the status code or the status code and the port when the redirect is executed.</span></span>

<span data-ttu-id="ffa7f-159">Exigir HTTPS globalmente (`options.Filters.Add(new RequireHttpsAttribute());`) é uma prática recomendada de segurança.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-159">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="ffa7f-160">Aplicar o `[RequireHttps]` atributo para todas as páginas Razor/controladores não é considerado mais seguro que exijam HTTPS globalmente.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-160">Applying the `[RequireHttps]` attribute to all controllers/Razor Pages isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="ffa7f-161">Você não pode garantir a `[RequireHttps]` atributo é aplicado quando forem adicionadas novos controladores e páginas Razor.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-161">You can't guarantee the `[RequireHttps]` attribute is applied when new controllers and Razor Pages are added.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="hsts"></a>
## <a name="http-strict-transport-security-protocol-hsts"></a><span data-ttu-id="ffa7f-162">Protocolo de segurança de transporte estrito de HTTP (HSTS)</span><span class="sxs-lookup"><span data-stu-id="ffa7f-162">HTTP Strict Transport Security Protocol (HSTS)</span></span>

<span data-ttu-id="ffa7f-163">Por [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [segurança de transporte estrito HTTP (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) é um aprimoramento de segurança de aceitação é especificado por um aplicativo web com o uso de um cabeçalho de resposta especial.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-163">Per [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) is an opt-in security enhancement that is specified by a web application through the use of a special response header.</span></span> <span data-ttu-id="ffa7f-164">Depois que um navegador com suporte recebe esse cabeçalho navegador impedirá todas as comunicações do que está sendo enviada via HTTP no domínio especificado e enviará, em vez disso, todas as comunicações via HTTPS.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-164">Once a supported browser receives this header that browser will prevent any communications from being sent over HTTP to the specified domain and will instead send all communications over HTTPS.</span></span> <span data-ttu-id="ffa7f-165">Ele também evita avisos em navegadores de cliques HTTPS.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-165">It also prevents HTTPS click through prompts on browsers.</span></span>

<span data-ttu-id="ffa7f-166">2.1 ou posterior do ASP.NET Core implementa HSTS com o `UseHsts` método de extensão.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-166">ASP.NET Core 2.1 or later implements HSTS with the `UseHsts` extension method.</span></span> <span data-ttu-id="ffa7f-167">O código a seguir chama `UseHsts` quando o aplicativo não está em [modo de desenvolvimento](xref:fundamentals/environments):</span><span class="sxs-lookup"><span data-stu-id="ffa7f-167">The following code calls `UseHsts` when the app isn't in [development mode](xref:fundamentals/environments):</span></span>

<span data-ttu-id="ffa7f-168">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]</span><span class="sxs-lookup"><span data-stu-id="ffa7f-168">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]</span></span>

<span data-ttu-id="ffa7f-169">`UseHsts` não é recomendado em desenvolvimento porque o cabeçalho HSTS é altamente armazenável em cache por navegadores.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-169">`UseHsts` isn't recommended in development because the HSTS header is highly cacheable by browsers.</span></span> <span data-ttu-id="ffa7f-170">Por padrão, `UseHsts` exclui o endereço de loopback local.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-170">By default, `UseHsts` excludes the local loopback address.</span></span>

<span data-ttu-id="ffa7f-171">O código a seguir:</span><span class="sxs-lookup"><span data-stu-id="ffa7f-171">The following code:</span></span>

<span data-ttu-id="ffa7f-172">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]</span><span class="sxs-lookup"><span data-stu-id="ffa7f-172">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]</span></span>

* <span data-ttu-id="ffa7f-173">Define o parâmetro de pré-carregamento do cabeçalho de segurança de transporte Strict.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-173">Sets the preload parameter of the Strict-Transport-Security header.</span></span> <span data-ttu-id="ffa7f-174">Pré-carregamento não é parte do [especificação RFC HSTS](https://tools.ietf.org/html/rfc6797), mas é compatível com navegadores da web para pré-carregar sites HSTS nova instalação.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-174">Preload is not part of the [RFC HSTS specification](https://tools.ietf.org/html/rfc6797), but is supported by web browsers to preload HSTS sites on fresh install.</span></span> <span data-ttu-id="ffa7f-175">Veja [https://hstspreload.org/](https://hstspreload.org/) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-175">See [https://hstspreload.org/](https://hstspreload.org/) for more information.</span></span>
* <span data-ttu-id="ffa7f-176">Permite [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), que se aplica a política HSTS a subdomínios de Host.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-176">Enables [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), which applies the HSTS policy to Host subdomains.</span></span> 
* <span data-ttu-id="ffa7f-177">Define explicitamente o parâmetro de idade máxima do cabeçalho de segurança de transporte Strict para 60 dias.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-177">Explicitly sets the max-age parameter of the Strict-Transport-Security header to to 60 days.</span></span> <span data-ttu-id="ffa7f-178">Se não for definido, o padrão é 30 dias.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-178">If not set, defaults to 30 days.</span></span> <span data-ttu-id="ffa7f-179">Consulte o [max-age diretiva](https://tools.ietf.org/html/rfc6797#section-6.1.1) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-179">See the [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) for more information.</span></span>
* <span data-ttu-id="ffa7f-180">Adiciona `example.com` à lista de hosts a serem excluídos.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-180">Adds `example.com` to the list of hosts to exclude.</span></span>

<span data-ttu-id="ffa7f-181">`UseHsts` Exclui os seguintes hosts de loopback:</span><span class="sxs-lookup"><span data-stu-id="ffa7f-181">`UseHsts` excludes the following loopback hosts:</span></span>

* <span data-ttu-id="ffa7f-182">`localhost` : O endereço de loopback do IPv4.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-182">`localhost` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="ffa7f-183">`127.0.0.1` : O endereço de loopback do IPv4.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-183">`127.0.0.1` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="ffa7f-184">`[::1]` : O endereço de loopback de IPv6.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-184">`[::1]` : The IPv6 loopback address.</span></span>

<span data-ttu-id="ffa7f-185">O exemplo anterior mostra como adicionar outros hosts.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-185">The preceding example shows how to add additional hosts.</span></span>
::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="https"></a>
## <a name="opt-out-of-https-on-project-creation"></a><span data-ttu-id="ffa7f-186">Recusar HTTPS na criação do projeto</span><span class="sxs-lookup"><span data-stu-id="ffa7f-186">Opt-out of HTTPS on project creation</span></span>

<span data-ttu-id="ffa7f-187">Os modelos de aplicativos do ASP.NET Core web 2.1 ou posterior (do Visual Studio ou a linha de comando dotnet) habilitam [redirecionamento HTTPS](#require) e [HSTS](#hsts).</span><span class="sxs-lookup"><span data-stu-id="ffa7f-187">The ASP.NET Core 2.1 or later web application templates (from Visual Studio or the dotnet command line) enable [HTTPS redirection](#require) and [HSTS](#hsts).</span></span> <span data-ttu-id="ffa7f-188">Para implantações que não exijam HTTPS, você pode recusar o HTTPS.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-188">For deployments that don't require HTTPS, you can opt-out of HTTPS.</span></span> <span data-ttu-id="ffa7f-189">Por exemplo, alguns serviços de back-end onde HTTPS está sendo tratado externamente na borda, usando HTTPS em cada nó não é necessária.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-189">For example, some backend services where HTTPS is being handled externally at the edge, using HTTPS at each node is not needed.</span></span>

<span data-ttu-id="ffa7f-190">Para recusar o HTTPS:</span><span class="sxs-lookup"><span data-stu-id="ffa7f-190">To opt-out of HTTPS:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ffa7f-191">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ffa7f-191">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="ffa7f-192">Desmarque o **configurar para HTTPS** caixa de seleção.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-192">Uncheck the **Configure for HTTPS** checkbox.</span></span>

![Diagrama de entidade](enforcing-ssl/_static/out.png)

#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="ffa7f-194">CLI do .NET Core</span><span class="sxs-lookup"><span data-stu-id="ffa7f-194">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="ffa7f-195">Use a opção `--no-https`.</span><span class="sxs-lookup"><span data-stu-id="ffa7f-195">Use the `--no-https` option.</span></span> <span data-ttu-id="ffa7f-196">Por exemplo</span><span class="sxs-lookup"><span data-stu-id="ffa7f-196">For example</span></span>

```console
dotnet new webapp --no-https
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="how-to-setup-a-developer-certificate-for-docker"></a><span data-ttu-id="ffa7f-198">Como configurar um certificado do desenvolvedor para Docker</span><span class="sxs-lookup"><span data-stu-id="ffa7f-198">How to setup a developer certificate for Docker</span></span>

<span data-ttu-id="ffa7f-199">Consulte [esse problema do GitHub](https://github.com/aspnet/Docs/issues/6199).</span><span class="sxs-lookup"><span data-stu-id="ffa7f-199">See [this GitHub issue](https://github.com/aspnet/Docs/issues/6199).</span></span>

::: moniker-end

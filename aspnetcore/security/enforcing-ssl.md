---
title: Impor HTTPS do núcleo do ASP.NET
author: rick-anderson
description: Mostra como exigir HTTPS/TLS em um núcleo de ASP.NET aplicativo web.
manager: wpickett
ms.author: riande
ms.date: 2/9/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/enforcing-ssl
ms.openlocfilehash: 0509bebe430c6ba213031a2cb7cb91bb7a39566d
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/18/2018
---
# <a name="enforce-https-in-an-aspnet-core"></a><span data-ttu-id="88779-103">Impor HTTPS do núcleo do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="88779-103">Enforce HTTPS in an ASP.NET Core</span></span>

<span data-ttu-id="88779-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="88779-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="88779-105">Este documento demonstra como:</span><span class="sxs-lookup"><span data-stu-id="88779-105">This document shows how to:</span></span>

- <span data-ttu-id="88779-106">Exigir HTTPS para todas as solicitações.</span><span class="sxs-lookup"><span data-stu-id="88779-106">Require HTTPS for all requests.</span></span>
- <span data-ttu-id="88779-107">Redirecione todas as solicitações HTTP para HTTPS.</span><span class="sxs-lookup"><span data-stu-id="88779-107">Redirect all HTTP requests to HTTPS.</span></span>

> [!WARNING]
> <span data-ttu-id="88779-108">Fazer **não** usar `RequireHttpsAttribute` em APIs da Web que recebe informações confidenciais.</span><span class="sxs-lookup"><span data-stu-id="88779-108">Do **not** use `RequireHttpsAttribute` on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="88779-109">`RequireHttpsAttribute` usa códigos de status HTTP para redirecionar navegadores de HTTP para HTTPS.</span><span class="sxs-lookup"><span data-stu-id="88779-109">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="88779-110">Clientes de API podem não entender ou obedecer redirecionamentos de HTTP para HTTPS.</span><span class="sxs-lookup"><span data-stu-id="88779-110">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="88779-111">Esses clientes podem enviar informações sobre HTTP.</span><span class="sxs-lookup"><span data-stu-id="88779-111">Such clients may send information over HTTP.</span></span> <span data-ttu-id="88779-112">APIs da Web deverá:</span><span class="sxs-lookup"><span data-stu-id="88779-112">Web APIs should either:</span></span>
>
>* <span data-ttu-id="88779-113">Não escuta no HTTP.</span><span class="sxs-lookup"><span data-stu-id="88779-113">Not listen on HTTP.</span></span>
>* <span data-ttu-id="88779-114">Feche a conexão com o código de status 400 (solicitação incorreta) e não atender à solicitação.</span><span class="sxs-lookup"><span data-stu-id="88779-114">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

<a name="require"></a>
## <a name="require-https"></a><span data-ttu-id="88779-115">Exigir HTTPS</span><span class="sxs-lookup"><span data-stu-id="88779-115">Require HTTPS</span></span>

::: moniker range=">= aspnetcore-2.1"

[!INCLUDE[](~/includes/2.1.md)]

<span data-ttu-id="88779-117">É recomendável ASP.NET Core todas as chamadas de aplicativos web `UseHttpsRedirection` para redirecionar todas as solicitações HTTP para HTTPS.</span><span class="sxs-lookup"><span data-stu-id="88779-117">We recommend all ASP.NET Core web apps call `UseHttpsRedirection` to redirect all HTTP requests to HTTPS.</span></span> <span data-ttu-id="88779-118">Se `UseHsts` é chamado no aplicativo, ele deve ser chamado antes de `UseHttpsRedirection`.</span><span class="sxs-lookup"><span data-stu-id="88779-118">If `UseHsts` is called in the app, it must be called before `UseHttpsRedirection`.</span></span>

<span data-ttu-id="88779-119">O código a seguir chama `UseHttpsRedirection` no `Startup` classe:</span><span class="sxs-lookup"><span data-stu-id="88779-119">The following code calls `UseHttpsRedirection` in the `Startup` class:</span></span>

<span data-ttu-id="88779-120">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]</span><span class="sxs-lookup"><span data-stu-id="88779-120">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]</span></span>


<span data-ttu-id="88779-121">O código a seguir:</span><span class="sxs-lookup"><span data-stu-id="88779-121">The following code:</span></span>

<span data-ttu-id="88779-122">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]</span><span class="sxs-lookup"><span data-stu-id="88779-122">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]</span></span>

* <span data-ttu-id="88779-123">Conjuntos de `RedirectStatusCode`.</span><span class="sxs-lookup"><span data-stu-id="88779-123">Sets `RedirectStatusCode`.</span></span>
* <span data-ttu-id="88779-124">Define a porta HTTPS para 5001.</span><span class="sxs-lookup"><span data-stu-id="88779-124">Sets the HTTPS port to 5001.</span></span>

::: moniker-end


::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="88779-125">O [RequireHttpsAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.RequireHttpsAttribute) é usada para exigir HTTPS.</span><span class="sxs-lookup"><span data-stu-id="88779-125">The [RequireHttpsAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.RequireHttpsAttribute) is used to require HTTPS.</span></span> <span data-ttu-id="88779-126">`[RequireHttpsAttribute]` pode decorar controladores ou métodos, ou podem ser aplicadas globalmente.</span><span class="sxs-lookup"><span data-stu-id="88779-126">`[RequireHttpsAttribute]` can decorate controllers or methods, or can be applied globally.</span></span> <span data-ttu-id="88779-127">Para aplicar o atributo global, adicione o seguinte código ao `ConfigureServices` em `Startup`:</span><span class="sxs-lookup"><span data-stu-id="88779-127">To apply the attribute globally, add the following code to `ConfigureServices` in `Startup`:</span></span>

<span data-ttu-id="88779-128">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]</span><span class="sxs-lookup"><span data-stu-id="88779-128">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]</span></span>

<span data-ttu-id="88779-129">O código realçado anterior requer todas as solicitações usar `HTTPS`; portanto, as solicitações HTTP são ignoradas.</span><span class="sxs-lookup"><span data-stu-id="88779-129">The preceding highlighted code requires all requests use `HTTPS`; therefore, HTTP requests are ignored.</span></span> <span data-ttu-id="88779-130">O seguinte código realçado redireciona todas as solicitações HTTP para HTTPS:</span><span class="sxs-lookup"><span data-stu-id="88779-130">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

<span data-ttu-id="88779-131">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]</span><span class="sxs-lookup"><span data-stu-id="88779-131">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]</span></span>

<span data-ttu-id="88779-132">Para obter mais informações, consulte [Middleware de regravação de URL](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="88779-132">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>

<span data-ttu-id="88779-133">Exigir HTTPS globalmente (`options.Filters.Add(new RequireHttpsAttribute());`) é uma prática recomendada de segurança.</span><span class="sxs-lookup"><span data-stu-id="88779-133">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="88779-134">Aplicar o `[RequireHttps]` atributo para todas as páginas Razor/controladores não é considerado mais seguro que exijam HTTPS globalmente.</span><span class="sxs-lookup"><span data-stu-id="88779-134">Applying the `[RequireHttps]` attribute to all controllers/Razor Pages isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="88779-135">Você não pode garantir a `[RequireHttps]` atributo é aplicado quando forem adicionadas novos controladores e páginas Razor.</span><span class="sxs-lookup"><span data-stu-id="88779-135">You can't guarantee the `[RequireHttps]` attribute is applied when new controllers and Razor Pages are added.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"
<a name="hsts"></a>
## <a name="http-strict-transport-security-protocol-hsts"></a><span data-ttu-id="88779-136">Protocolo de segurança de transporte estrito de HTTP (HSTS)</span><span class="sxs-lookup"><span data-stu-id="88779-136">HTTP Strict Transport Security Protocol (HSTS)</span></span>

<span data-ttu-id="88779-137">Por [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [segurança de transporte estrito HTTP (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) é um aprimoramento de segurança de aceitação é especificado por um aplicativo web com o uso de um cabeçalho de resposta especial.</span><span class="sxs-lookup"><span data-stu-id="88779-137">Per [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) is an opt-in security enhancement that is specified by a web application through the use of a special response header.</span></span> <span data-ttu-id="88779-138">Depois que um navegador com suporte recebe esse cabeçalho navegador impedirá todas as comunicações do que está sendo enviada via HTTP no domínio especificado e enviará, em vez disso, todas as comunicações via HTTPS.</span><span class="sxs-lookup"><span data-stu-id="88779-138">Once a supported browser receives this header that browser will prevent any communications from being sent over HTTP to the specified domain and will instead send all communications over HTTPS.</span></span> <span data-ttu-id="88779-139">Ele também evita avisos em navegadores de cliques HTTPS.</span><span class="sxs-lookup"><span data-stu-id="88779-139">It also prevents HTTPS click through prompts on browsers.</span></span>

<span data-ttu-id="88779-140">ASP.NET Core preview1 2.1 ou posterior implementa HSTS com o `UseHsts` método de extensão.</span><span class="sxs-lookup"><span data-stu-id="88779-140">ASP.NET Core 2.1 preview1 or later implements HSTS with the `UseHsts` extension method.</span></span> <span data-ttu-id="88779-141">O código a seguir chama `UseHsts` quando o aplicativo não está em [modo de desenvolvimento](xref:fundamentals/environments):</span><span class="sxs-lookup"><span data-stu-id="88779-141">The following code calls `UseHsts` when the app isn't in [development mode](xref:fundamentals/environments):</span></span>

<span data-ttu-id="88779-142">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]</span><span class="sxs-lookup"><span data-stu-id="88779-142">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]</span></span>

<span data-ttu-id="88779-143">`UseHsts` não é recomendável em desenvolvimento porque o cabeçalho HSTS é altamente armazenável por navegadores.</span><span class="sxs-lookup"><span data-stu-id="88779-143">`UseHsts` is not recommend in development because the HSTS header is highly cachable by browsers.</span></span> <span data-ttu-id="88779-144">Por padrão, UseHsts exclui o endereço de loopback local.</span><span class="sxs-lookup"><span data-stu-id="88779-144">By default, UseHsts excludes the local loopback address.</span></span>

<span data-ttu-id="88779-145">O código a seguir:</span><span class="sxs-lookup"><span data-stu-id="88779-145">The following code:</span></span>

<span data-ttu-id="88779-146">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]</span><span class="sxs-lookup"><span data-stu-id="88779-146">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]</span></span>

* <span data-ttu-id="88779-147">Define o parâmetro de pré-carregamento do cabeçalho de segurança de transporte Strict.</span><span class="sxs-lookup"><span data-stu-id="88779-147">Sets the preload parameter of the Strict-Transport-Security header.</span></span> <span data-ttu-id="88779-148">Pré-carregamento não é parte do [especificação RFC HSTS](https://tools.ietf.org/html/rfc6797), mas é compatível com navegadores da web para pré-carregar sites HSTS nova instalação.</span><span class="sxs-lookup"><span data-stu-id="88779-148">Preload is not part of the [RFC HSTS specification](https://tools.ietf.org/html/rfc6797), but is supported by web browsers to preload HSTS sites on fresh install.</span></span> <span data-ttu-id="88779-149">Veja [https://hstspreload.org/](https://hstspreload.org/) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="88779-149">See [https://hstspreload.org/](https://hstspreload.org/) for more information.</span></span>
* <span data-ttu-id="88779-150">Permite [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), que se aplica a política HSTS a subdomínios de Host.</span><span class="sxs-lookup"><span data-stu-id="88779-150">Enables [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), which applies the HSTS policy to Host subdomains.</span></span> 
* <span data-ttu-id="88779-151">Define explicitamente o parâmetro de idade máxima do cabeçalho de segurança de transporte Strict para 60 dias.</span><span class="sxs-lookup"><span data-stu-id="88779-151">Explicitly sets the max-age parameter of the Strict-Transport-Security header to to 60 days.</span></span> <span data-ttu-id="88779-152">Se não for definido, o padrão é 30 dias.</span><span class="sxs-lookup"><span data-stu-id="88779-152">If not set, defaults to 30 days.</span></span> <span data-ttu-id="88779-153">Consulte o [max-age diretiva](https://tools.ietf.org/html/rfc6797#section-6.1.1) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="88779-153">See the [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) for more information.</span></span>
* <span data-ttu-id="88779-154">Adiciona `example.com` à lista de hosts a serem excluídos.</span><span class="sxs-lookup"><span data-stu-id="88779-154">Adds `example.com` to the list of hosts to exclude.</span></span>

<span data-ttu-id="88779-155">`UseHsts` Exclui os seguintes hosts de loopback:</span><span class="sxs-lookup"><span data-stu-id="88779-155">`UseHsts` excludes the following loopback hosts:</span></span>

* <span data-ttu-id="88779-156">`localhost` : O endereço de loopback do IPv4.</span><span class="sxs-lookup"><span data-stu-id="88779-156">`localhost` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="88779-157">`127.0.0.1` : O endereço de loopback do IPv4.</span><span class="sxs-lookup"><span data-stu-id="88779-157">`127.0.0.1` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="88779-158">`[::1]` : O endereço de loopback de IPv6.</span><span class="sxs-lookup"><span data-stu-id="88779-158">`[::1]` : The IPv6 loopback address.</span></span>

<span data-ttu-id="88779-159">O exemplo anterior mostra como adicionar outros hosts.</span><span class="sxs-lookup"><span data-stu-id="88779-159">The preceding example shows how to add additional hosts.</span></span>
::: moniker-end


::: moniker range=">= aspnetcore-2.1"
<a name="https"></a>
## <a name="opt-out-of-https-on-project-creation"></a><span data-ttu-id="88779-160">Recusar HTTPS na criação do projeto</span><span class="sxs-lookup"><span data-stu-id="88779-160">Opt-out of HTTPS on project creation</span></span>

<span data-ttu-id="88779-161">Habilitam o ASP.NET Core 2.1 e posteriores modelos de aplicativo da web (do Visual Studio ou a linha de comando dotnet) [redirecionamento HTTPS](#require) e [HSTS](#hsts).</span><span class="sxs-lookup"><span data-stu-id="88779-161">The ASP.NET Core 2.1 and later web application templates (from Visual Studio or the dotnet command line) enable [HTTPS redirection](#require) and [HSTS](#hsts).</span></span> <span data-ttu-id="88779-162">Para implantações que não exijam HTTPS, você pode recusar o HTTPS.</span><span class="sxs-lookup"><span data-stu-id="88779-162">For deployments that don't require HTTPS, you can opt-out of HTTPS.</span></span> <span data-ttu-id="88779-163">Por exemplo, alguns serviços de back-end onde HTTPS está sendo tratado externamente na borda, usando HTTPS em cada nó não é necessária.</span><span class="sxs-lookup"><span data-stu-id="88779-163">For example, some backend services where HTTPS is being handled externally at the edge, using HTTPS at each node is not needed.</span></span>

<span data-ttu-id="88779-164">Para recusar o HTTPS:</span><span class="sxs-lookup"><span data-stu-id="88779-164">To opt-out of HTTPS:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="88779-165">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="88779-165">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="88779-166">Desmarque o **configurar para HTTPS** caixa de seleção.</span><span class="sxs-lookup"><span data-stu-id="88779-166">Uncheck the **Configure for HTTPS** checkbox.</span></span>

![Diagrama de entidade](enforcing-ssl/_static/out.png)


#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="88779-168">CLI do .NET Core</span><span class="sxs-lookup"><span data-stu-id="88779-168">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="88779-169">Use a opção `--no-https`.</span><span class="sxs-lookup"><span data-stu-id="88779-169">Use the `--no-https` option.</span></span> <span data-ttu-id="88779-170">Por exemplo</span><span class="sxs-lookup"><span data-stu-id="88779-170">For example</span></span>

```cli
dotnet new razor --no-https
```

------

::: moniker-end
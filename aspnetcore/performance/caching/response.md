---
title: O cache de resposta no núcleo do ASP.NET
author: rick-anderson
description: Saiba como usar a resposta em cache para reduzir os requisitos de largura de banda e melhorar o desempenho de aplicativos do ASP.NET Core.
ms.author: riande
ms.date: 09/20/2017
uid: performance/caching/response
ms.openlocfilehash: c53ae3f6ab8d26588533772dd4fdacb36ec12059
ms.sourcegitcommit: 931b6a2d7eb28a0f1295e8a95690b8c4c5f58477
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/28/2018
ms.locfileid: "37077758"
---
# <a name="response-caching-in-aspnet-core"></a><span data-ttu-id="c9c80-103">O cache de resposta no núcleo do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="c9c80-103">Response caching in ASP.NET Core</span></span>

<span data-ttu-id="c9c80-104">Por [John Luo](https://github.com/JunTaoLuo), [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="c9c80-104">By [John Luo](https://github.com/JunTaoLuo), [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), and [Luke Latham](https://github.com/guardrex)</span></span>

> [!NOTE]
> <span data-ttu-id="c9c80-105">O cache de resposta em páginas Razor está disponível no ASP.NET Core 2.1 ou posterior.</span><span class="sxs-lookup"><span data-stu-id="c9c80-105">Response caching in Razor Pages is available in ASP.NET Core 2.1 or later.</span></span>

<span data-ttu-id="c9c80-106">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/response/samples) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c9c80-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/response/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="c9c80-107">O cache de resposta reduz o número de solicitações de que um cliente ou um proxy faz a um servidor web.</span><span class="sxs-lookup"><span data-stu-id="c9c80-107">Response caching reduces the number of requests a client or proxy makes to a web server.</span></span> <span data-ttu-id="c9c80-108">O cache de resposta também reduz a quantidade de trabalho do servidor web executa para gerar uma resposta.</span><span class="sxs-lookup"><span data-stu-id="c9c80-108">Response caching also reduces the amount of work the web server performs to generate a response.</span></span> <span data-ttu-id="c9c80-109">O cache de resposta é controlado por cabeçalhos que especifique como deseja middleware para respostas de cache de cliente e proxy.</span><span class="sxs-lookup"><span data-stu-id="c9c80-109">Response caching is controlled by headers that specify how you want client, proxy, and middleware to cache responses.</span></span>

<span data-ttu-id="c9c80-110">O servidor web pode armazenar em cache respostas quando você adiciona [Middleware de cache de resposta](xref:performance/caching/middleware).</span><span class="sxs-lookup"><span data-stu-id="c9c80-110">The web server can cache responses when you add [Response Caching Middleware](xref:performance/caching/middleware).</span></span>

## <a name="http-based-response-caching"></a><span data-ttu-id="c9c80-111">O cache de resposta baseado em HTTP</span><span class="sxs-lookup"><span data-stu-id="c9c80-111">HTTP-based response caching</span></span>

<span data-ttu-id="c9c80-112">O [especificação HTTP 1.1 cache](https://tools.ietf.org/html/rfc7234) descreve o comportamento do caches de Internet.</span><span class="sxs-lookup"><span data-stu-id="c9c80-112">The [HTTP 1.1 Caching specification](https://tools.ietf.org/html/rfc7234) describes how Internet caches should behave.</span></span> <span data-ttu-id="c9c80-113">O cabeçalho HTTP principal usado para armazenar em cache é [Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2), que é usado para especificar o cache *diretivas*.</span><span class="sxs-lookup"><span data-stu-id="c9c80-113">The primary HTTP header used for caching is [Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2), which is used to specify cache *directives*.</span></span> <span data-ttu-id="c9c80-114">As diretivas de controlam o comportamento de cache conforme solicitações passarão de clientes para servidores e respostas passarão de servidores de volta aos clientes.</span><span class="sxs-lookup"><span data-stu-id="c9c80-114">The directives control caching behavior as requests make their way from clients to servers and as reponses make their way from servers back to clients.</span></span> <span data-ttu-id="c9c80-115">Solicitações e respostas movem através de servidores proxy e servidores proxy também devem estar em conformidade com a especificação de cache do HTTP 1.1.</span><span class="sxs-lookup"><span data-stu-id="c9c80-115">Requests and responses move through proxy servers, and proxy servers must also conform to the HTTP 1.1 Caching specification.</span></span>

<span data-ttu-id="c9c80-116">Comuns `Cache-Control` diretivas são mostradas na tabela a seguir.</span><span class="sxs-lookup"><span data-stu-id="c9c80-116">Common `Cache-Control` directives are shown in the following table.</span></span>

| <span data-ttu-id="c9c80-117">Diretiva</span><span class="sxs-lookup"><span data-stu-id="c9c80-117">Directive</span></span>                                                       | <span data-ttu-id="c9c80-118">Ação</span><span class="sxs-lookup"><span data-stu-id="c9c80-118">Action</span></span> |
| --------------------------------------------------------------- | ------ |
| [<span data-ttu-id="c9c80-119">public</span><span class="sxs-lookup"><span data-stu-id="c9c80-119">public</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.2.5)   | <span data-ttu-id="c9c80-120">Um cache pode armazenar a resposta.</span><span class="sxs-lookup"><span data-stu-id="c9c80-120">A cache may store the response.</span></span> |
| [<span data-ttu-id="c9c80-121">private</span><span class="sxs-lookup"><span data-stu-id="c9c80-121">private</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.2.6)  | <span data-ttu-id="c9c80-122">A resposta não deve ser armazenada por um cache compartilhado.</span><span class="sxs-lookup"><span data-stu-id="c9c80-122">The response must not be stored by a shared cache.</span></span> <span data-ttu-id="c9c80-123">Um cache privado pode armazenar e reutilizar a resposta.</span><span class="sxs-lookup"><span data-stu-id="c9c80-123">A private cache may store and reuse the response.</span></span> |
| [<span data-ttu-id="c9c80-124">max-age</span><span class="sxs-lookup"><span data-stu-id="c9c80-124">max-age</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.1.1)  | <span data-ttu-id="c9c80-125">O cliente não aceitará uma resposta cuja idade seja maior que o número especificado de segundos.</span><span class="sxs-lookup"><span data-stu-id="c9c80-125">The client won't accept a response whose age is greater than the specified number of seconds.</span></span> <span data-ttu-id="c9c80-126">Exemplos: `max-age=60` (60 segundos), `max-age=2592000` (mês)</span><span class="sxs-lookup"><span data-stu-id="c9c80-126">Examples: `max-age=60` (60 seconds), `max-age=2592000` (1 month)</span></span> |
| [<span data-ttu-id="c9c80-127">no-cache</span><span class="sxs-lookup"><span data-stu-id="c9c80-127">no-cache</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.1.4) | <span data-ttu-id="c9c80-128">**Em solicitações**: um cache não deve usar uma resposta armazenada para satisfazer a solicitação.</span><span class="sxs-lookup"><span data-stu-id="c9c80-128">**On requests**: A cache must not use a stored response to satisfy the request.</span></span> <span data-ttu-id="c9c80-129">Observação: O servidor de origem gera novamente a resposta para o cliente e o middleware atualiza a resposta armazenada em seu cache.</span><span class="sxs-lookup"><span data-stu-id="c9c80-129">Note: The origin server re-generates the response for the client, and the middleware updates the stored response in its cache.</span></span><br><br><span data-ttu-id="c9c80-130">**As respostas**: A resposta não deve ser usada para uma solicitação subsequente sem validação no servidor de origem.</span><span class="sxs-lookup"><span data-stu-id="c9c80-130">**On responses**: The response must not be used for a subsequent request without validation on the origin server.</span></span> |
| [<span data-ttu-id="c9c80-131">no-store</span><span class="sxs-lookup"><span data-stu-id="c9c80-131">no-store</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.1.5) | <span data-ttu-id="c9c80-132">**Em solicitações**: um cache não deve armazenar a solicitação.</span><span class="sxs-lookup"><span data-stu-id="c9c80-132">**On requests**: A cache must not store the request.</span></span><br><br><span data-ttu-id="c9c80-133">**As respostas**: um cache não deve armazenar qualquer parte da resposta.</span><span class="sxs-lookup"><span data-stu-id="c9c80-133">**On responses**: A cache must not store any part of the response.</span></span> |

<span data-ttu-id="c9c80-134">Outros cabeçalhos de cache que desempenham uma função no cache são mostrados na tabela a seguir.</span><span class="sxs-lookup"><span data-stu-id="c9c80-134">Other cache headers that play a role in caching are shown in the following table.</span></span>

| <span data-ttu-id="c9c80-135">Cabeçalho</span><span class="sxs-lookup"><span data-stu-id="c9c80-135">Header</span></span>                                                     | <span data-ttu-id="c9c80-136">Função</span><span class="sxs-lookup"><span data-stu-id="c9c80-136">Function</span></span> |
| ---------------------------------------------------------- | -------- |
| [<span data-ttu-id="c9c80-137">Idade</span><span class="sxs-lookup"><span data-stu-id="c9c80-137">Age</span></span>](https://tools.ietf.org/html/rfc7234#section-5.1)     | <span data-ttu-id="c9c80-138">Uma estimativa da quantidade de tempo em segundos desde que a resposta foi gerada ou validada com êxito no servidor de origem.</span><span class="sxs-lookup"><span data-stu-id="c9c80-138">An estimate of the amount of time in seconds since the response was generated or successfully validated at the origin server.</span></span> |
| [<span data-ttu-id="c9c80-139">Expirar</span><span class="sxs-lookup"><span data-stu-id="c9c80-139">Expires</span></span>](https://tools.ietf.org/html/rfc7234#section-5.3) | <span data-ttu-id="c9c80-140">A data/hora após o qual a resposta é considerada obsoleta.</span><span class="sxs-lookup"><span data-stu-id="c9c80-140">The date/time after which the response is considered stale.</span></span> |
| [<span data-ttu-id="c9c80-141">Pragma</span><span class="sxs-lookup"><span data-stu-id="c9c80-141">Pragma</span></span>](https://tools.ietf.org/html/rfc7234#section-5.4)  | <span data-ttu-id="c9c80-142">Existe para versões anteriores a compatibilidade com HTTP/1.0 armazena em cache para a configuração `no-cache` comportamento.</span><span class="sxs-lookup"><span data-stu-id="c9c80-142">Exists for backwards compatibility with HTTP/1.0 caches for setting `no-cache` behavior.</span></span> <span data-ttu-id="c9c80-143">Se o `Cache-Control` cabeçalho estiver presente, o `Pragma` cabeçalho será ignorado.</span><span class="sxs-lookup"><span data-stu-id="c9c80-143">If the `Cache-Control` header is present, the `Pragma` header is ignored.</span></span> |
| [<span data-ttu-id="c9c80-144">Variar</span><span class="sxs-lookup"><span data-stu-id="c9c80-144">Vary</span></span>](https://tools.ietf.org/html/rfc7231#section-7.1.4)  | <span data-ttu-id="c9c80-145">Especifica que uma resposta em cache não deve ser enviada a menos que todos os do `Vary` correspondem a campos de cabeçalho na solicitação original da resposta em cache e a nova solicitação.</span><span class="sxs-lookup"><span data-stu-id="c9c80-145">Specifies that a cached response must not be sent unless all of the `Vary` header fields match in both the cached response's original request and the new request.</span></span> |

## <a name="http-based-caching-respects-request-cache-control-directives"></a><span data-ttu-id="c9c80-146">Aspectos de cache com base em HTTP solicitar as diretivas de controle de Cache</span><span class="sxs-lookup"><span data-stu-id="c9c80-146">HTTP-based caching respects request Cache-Control directives</span></span>

<span data-ttu-id="c9c80-147">O [especificação de cache do HTTP 1.1 para o cabeçalho Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2) requer um cache cumprir válido `Cache-Control` cabeçalho enviado pelo cliente.</span><span class="sxs-lookup"><span data-stu-id="c9c80-147">The [HTTP 1.1 Caching specification for the Cache-Control header](https://tools.ietf.org/html/rfc7234#section-5.2) requires a cache to honor a valid `Cache-Control` header sent by the client.</span></span> <span data-ttu-id="c9c80-148">Um cliente pode fazer solicitações com uma `no-cache` valor de cabeçalho e forçar o servidor para gerar uma nova resposta para cada solicitação.</span><span class="sxs-lookup"><span data-stu-id="c9c80-148">A client can make requests with a `no-cache` header value and force the server to generate a new response for every request.</span></span>

<span data-ttu-id="c9c80-149">Sempre para respeitar cliente `Cache-Control` cabeçalhos de solicitação faz sentido se você considerar o objetivo do cache de HTTP.</span><span class="sxs-lookup"><span data-stu-id="c9c80-149">Always honoring client `Cache-Control` request headers makes sense if you consider the goal of HTTP caching.</span></span> <span data-ttu-id="c9c80-150">Sob a especificação oficial, cache destina-se para reduzir a sobrecarga de rede e latência de atender solicitações em uma rede de clientes, proxies e servidores.</span><span class="sxs-lookup"><span data-stu-id="c9c80-150">Under the official specification, caching is meant to reduce the latency and network overhead of satisfying requests across a network of clients, proxies, and servers.</span></span> <span data-ttu-id="c9c80-151">Não necessariamente é uma maneira de controlar a carga em um servidor de origem.</span><span class="sxs-lookup"><span data-stu-id="c9c80-151">It isn't necessarily a way to control the load on an origin server.</span></span>

<span data-ttu-id="c9c80-152">Não há nenhum atual desenvolvedor controle sobre o comportamento de cache ao usar o [Middleware de cache de resposta](xref:performance/caching/middleware) porque o middleware cumpre o oficial da especificação de cache.</span><span class="sxs-lookup"><span data-stu-id="c9c80-152">There's no current developer control over this caching behavior when using the [Response Caching Middleware](xref:performance/caching/middleware) because the middleware adheres to the official caching specification.</span></span> <span data-ttu-id="c9c80-153">[Aperfeiçoamentos futuros para o middleware](https://github.com/aspnet/ResponseCaching/issues/96) permitirá a configurar o middleware para ignorar uma solicitação `Cache-Control` cabeçalho ao decidir servir uma resposta em cache.</span><span class="sxs-lookup"><span data-stu-id="c9c80-153">[Future enhancements to the middleware](https://github.com/aspnet/ResponseCaching/issues/96) will permit configuring the middleware to ignore a request's `Cache-Control` header when deciding to serve a cached response.</span></span> <span data-ttu-id="c9c80-154">Isso oferecerá uma oportunidade para controlar melhor a carga no servidor quando você usar o middleware.</span><span class="sxs-lookup"><span data-stu-id="c9c80-154">This will offer you an opportunity to better control the load on your server when you use the middleware.</span></span>

## <a name="other-caching-technology-in-aspnet-core"></a><span data-ttu-id="c9c80-155">Outras tecnologias de cache no núcleo do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="c9c80-155">Other caching technology in ASP.NET Core</span></span>

### <a name="in-memory-caching"></a><span data-ttu-id="c9c80-156">O armazenamento em cache na memória</span><span class="sxs-lookup"><span data-stu-id="c9c80-156">In-memory caching</span></span>

<span data-ttu-id="c9c80-157">O armazenamento em cache na memória usa a memória do servidor para armazenar dados em cache.</span><span class="sxs-lookup"><span data-stu-id="c9c80-157">In-memory caching uses server memory to store cached data.</span></span> <span data-ttu-id="c9c80-158">Esse tipo de cache é adequado para um único servidor ou vários servidores usando *sessões Autoadesivas*.</span><span class="sxs-lookup"><span data-stu-id="c9c80-158">This type of caching is suitable for a single server or multiple servers using *sticky sessions*.</span></span> <span data-ttu-id="c9c80-159">Sessões Autoadesivas significa que as solicitações de um cliente sempre são roteadas para o mesmo servidor para processamento.</span><span class="sxs-lookup"><span data-stu-id="c9c80-159">Sticky sessions means that the requests from a client are always routed to the same server for processing.</span></span>

<span data-ttu-id="c9c80-160">Para obter mais informações, consulte [Cache na memória](xref:performance/caching/memory).</span><span class="sxs-lookup"><span data-stu-id="c9c80-160">For more information, see [Cache in-memory](xref:performance/caching/memory).</span></span>

### <a name="distributed-cache"></a><span data-ttu-id="c9c80-161">Cache distribuído</span><span class="sxs-lookup"><span data-stu-id="c9c80-161">Distributed Cache</span></span>

<span data-ttu-id="c9c80-162">Use um cache distribuído para armazenar dados na memória quando o aplicativo é hospedado em um farm de servidor ou de nuvem.</span><span class="sxs-lookup"><span data-stu-id="c9c80-162">Use a distributed cache to store data in memory when the app is hosted in a cloud or server farm.</span></span> <span data-ttu-id="c9c80-163">O cache é compartilhado entre os servidores que processam solicitações.</span><span class="sxs-lookup"><span data-stu-id="c9c80-163">The cache is shared across the servers that process requests.</span></span> <span data-ttu-id="c9c80-164">Um cliente pode enviar uma solicitação que é tratada por qualquer servidor no grupo, se os dados armazenados em cache para o cliente estão disponíveis.</span><span class="sxs-lookup"><span data-stu-id="c9c80-164">A client can submit a request that's handled by any server in the group if cached data for the client is available.</span></span> <span data-ttu-id="c9c80-165">ASP.NET Core oferece o SQL Server e os caches Redis distribuído.</span><span class="sxs-lookup"><span data-stu-id="c9c80-165">ASP.NET Core offers SQL Server and Redis distributed caches.</span></span>

<span data-ttu-id="c9c80-166">Para obter mais informações, consulte [trabalhar com um cache distribuído](xref:performance/caching/distributed).</span><span class="sxs-lookup"><span data-stu-id="c9c80-166">For more information, see [Work with a distributed cache](xref:performance/caching/distributed).</span></span>

### <a name="cache-tag-helper"></a><span data-ttu-id="c9c80-167">Auxiliar de marca de cache</span><span class="sxs-lookup"><span data-stu-id="c9c80-167">Cache Tag Helper</span></span>

<span data-ttu-id="c9c80-168">Você pode armazenar em cache o conteúdo de um modo de exibição do MVC ou Razor de página com o auxiliar de marca de Cache.</span><span class="sxs-lookup"><span data-stu-id="c9c80-168">You can cache the content from an MVC view or Razor Page with the Cache Tag Helper.</span></span> <span data-ttu-id="c9c80-169">O auxiliar de marca de Cache usa o cache de memória para armazenar dados.</span><span class="sxs-lookup"><span data-stu-id="c9c80-169">The Cache Tag Helper uses in-memory caching to store data.</span></span>

<span data-ttu-id="c9c80-170">Para obter mais informações, consulte [auxiliar de marca de Cache no ASP.NET MVC de núcleo](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="c9c80-170">For more information, see [Cache Tag Helper in ASP.NET Core MVC](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper).</span></span>

### <a name="distributed-cache-tag-helper"></a><span data-ttu-id="c9c80-171">Auxiliar de Marca de Cache Distribuído</span><span class="sxs-lookup"><span data-stu-id="c9c80-171">Distributed Cache Tag Helper</span></span>

<span data-ttu-id="c9c80-172">Você pode armazenar em cache o conteúdo de um modo de exibição do MVC ou página de Razor em nuvem distribuído ou cenários de farm da web com o auxiliar de marca de Cache distribuído.</span><span class="sxs-lookup"><span data-stu-id="c9c80-172">You can cache the content from an MVC view or Razor Page in distributed cloud or web farm scenarios with the Distributed Cache Tag Helper.</span></span> <span data-ttu-id="c9c80-173">O auxiliar de marca de Cache distribuído usa o SQL Server ou Redis para armazenar dados.</span><span class="sxs-lookup"><span data-stu-id="c9c80-173">The Distributed Cache Tag Helper uses SQL Server or Redis to store data.</span></span>

<span data-ttu-id="c9c80-174">Para obter mais informações, consulte [auxiliar de marca de Cache distribuído](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="c9c80-174">For more information, see [Distributed Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper).</span></span>

## <a name="responsecache-attribute"></a><span data-ttu-id="c9c80-175">Atributo ResponseCache</span><span class="sxs-lookup"><span data-stu-id="c9c80-175">ResponseCache attribute</span></span>

<span data-ttu-id="c9c80-176">O [ResponseCacheAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) Especifica os parâmetros necessários para definir os cabeçalhos apropriados no cache de resposta.</span><span class="sxs-lookup"><span data-stu-id="c9c80-176">The [ResponseCacheAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) specifies the parameters necessary for setting appropriate headers in response caching.</span></span>

> [!WARNING]
> <span data-ttu-id="c9c80-177">Desabilite o cache de conteúdo que contém informações para clientes autenticados.</span><span class="sxs-lookup"><span data-stu-id="c9c80-177">Disable caching for content that contains information for authenticated clients.</span></span> <span data-ttu-id="c9c80-178">O cache deve ser habilitado apenas para o conteúdo que não são alterados com base na identidade do usuário ou se um usuário está conectado.</span><span class="sxs-lookup"><span data-stu-id="c9c80-178">Caching should only be enabled for content that doesn't change based on a user's identity or whether a user is signed in.</span></span>

<span data-ttu-id="c9c80-179">[VaryByQueryKeys](/dotnet/api/microsoft.aspnetcore.mvc.responsecacheattribute.varybyquerykeys) a resposta armazenada varia de acordo com os valores de determinada lista de chaves de consulta.</span><span class="sxs-lookup"><span data-stu-id="c9c80-179">[VaryByQueryKeys](/dotnet/api/microsoft.aspnetcore.mvc.responsecacheattribute.varybyquerykeys) varies the stored response by the values of the given list of query keys.</span></span> <span data-ttu-id="c9c80-180">Quando um único valor de `*` é fornecido, o middleware varia respostas por todos os parâmetros de cadeia de caracteres de consulta de solicitação.</span><span class="sxs-lookup"><span data-stu-id="c9c80-180">When a single value of `*` is provided, the middleware varies responses by all request query string parameters.</span></span> <span data-ttu-id="c9c80-181">`VaryByQueryKeys` exige o ASP.NET Core 1.1 ou posterior.</span><span class="sxs-lookup"><span data-stu-id="c9c80-181">`VaryByQueryKeys` requires ASP.NET Core 1.1 or later.</span></span>

<span data-ttu-id="c9c80-182">O Middleware de cache de resposta deve ser habilitado para definir o `VaryByQueryKeys` propriedade; caso contrário, uma exceção de tempo de execução é gerada.</span><span class="sxs-lookup"><span data-stu-id="c9c80-182">The Response Caching Middleware must be enabled to set the `VaryByQueryKeys` property; otherwise, a runtime exception is thrown.</span></span> <span data-ttu-id="c9c80-183">Não existe um cabeçalho HTTP correspondente para o `VaryByQueryKeys` propriedade.</span><span class="sxs-lookup"><span data-stu-id="c9c80-183">There isn't a corresponding HTTP header for the `VaryByQueryKeys` property.</span></span> <span data-ttu-id="c9c80-184">A propriedade é um recurso HTTP manipulado pelo Middleware de armazenamento em cache a resposta.</span><span class="sxs-lookup"><span data-stu-id="c9c80-184">The property is an HTTP feature handled by the Response Caching Middleware.</span></span> <span data-ttu-id="c9c80-185">Para o middleware servir uma resposta em cache, a cadeia de caracteres de consulta e o valor de cadeia de caracteres de consulta devem corresponder uma solicitação anterior.</span><span class="sxs-lookup"><span data-stu-id="c9c80-185">For the middleware to serve a cached response, the query string and query string value must match a previous request.</span></span> <span data-ttu-id="c9c80-186">Por exemplo, considere a sequência de solicitações e resultados mostrados na tabela a seguir.</span><span class="sxs-lookup"><span data-stu-id="c9c80-186">For example, consider the sequence of requests and results shown in the following table.</span></span>

| <span data-ttu-id="c9c80-187">Solicitação</span><span class="sxs-lookup"><span data-stu-id="c9c80-187">Request</span></span>                          | <span data-ttu-id="c9c80-188">Resultado</span><span class="sxs-lookup"><span data-stu-id="c9c80-188">Result</span></span>                   |
| -------------------------------- | ------------------------ |
| `http://example.com?key1=value1` | <span data-ttu-id="c9c80-189">Retornado do servidor</span><span class="sxs-lookup"><span data-stu-id="c9c80-189">Returned from server</span></span>     |
| `http://example.com?key1=value1` | <span data-ttu-id="c9c80-190">Retornado de middleware</span><span class="sxs-lookup"><span data-stu-id="c9c80-190">Returned from middleware</span></span> |
| `http://example.com?key1=value2` | <span data-ttu-id="c9c80-191">Retornado do servidor</span><span class="sxs-lookup"><span data-stu-id="c9c80-191">Returned from server</span></span>     |

<span data-ttu-id="c9c80-192">A primeira solicitação é retornada pelo servidor e armazenados em cache no middleware.</span><span class="sxs-lookup"><span data-stu-id="c9c80-192">The first request is returned by the server and cached in middleware.</span></span> <span data-ttu-id="c9c80-193">A segunda solicitação é retornada pelo middleware porque a cadeia de caracteres de consulta corresponde a solicitação anterior.</span><span class="sxs-lookup"><span data-stu-id="c9c80-193">The second request is returned by middleware because the query string matches the previous request.</span></span> <span data-ttu-id="c9c80-194">A terceira solicitação não está no cache de middleware porque o valor de cadeia de caracteres de consulta não corresponde a uma solicitação anterior.</span><span class="sxs-lookup"><span data-stu-id="c9c80-194">The third request isn't in the middleware cache because the query string value doesn't match a previous request.</span></span> 

<span data-ttu-id="c9c80-195">O `ResponseCacheAttribute` é usado para configurar e criar (por meio de `IFilterFactory`) um [ResponseCacheFilter](/dotnet/api/microsoft.aspnetcore.mvc.internal.responsecachefilter).</span><span class="sxs-lookup"><span data-stu-id="c9c80-195">The `ResponseCacheAttribute` is used to configure and create (via `IFilterFactory`) a [ResponseCacheFilter](/dotnet/api/microsoft.aspnetcore.mvc.internal.responsecachefilter).</span></span> <span data-ttu-id="c9c80-196">O `ResponseCacheFilter` realiza o trabalho de atualização de cabeçalhos HTTP apropriados e os recursos da resposta.</span><span class="sxs-lookup"><span data-stu-id="c9c80-196">The `ResponseCacheFilter` performs the work of updating the appropriate HTTP headers and features of the response.</span></span> <span data-ttu-id="c9c80-197">O filtro:</span><span class="sxs-lookup"><span data-stu-id="c9c80-197">The filter:</span></span>

* <span data-ttu-id="c9c80-198">Remove qualquer cabeçalho existente para `Vary`, `Cache-Control`, e `Pragma`.</span><span class="sxs-lookup"><span data-stu-id="c9c80-198">Removes any existing headers for `Vary`, `Cache-Control`, and `Pragma`.</span></span> 
* <span data-ttu-id="c9c80-199">Grava os cabeçalhos apropriados com base nas propriedades definidas no `ResponseCacheAttribute`.</span><span class="sxs-lookup"><span data-stu-id="c9c80-199">Writes out the appropriate headers based on the properties set in the `ResponseCacheAttribute`.</span></span> 
* <span data-ttu-id="c9c80-200">Atualiza a resposta de armazenamento em cache o recurso HTTP se `VaryByQueryKeys` está definido.</span><span class="sxs-lookup"><span data-stu-id="c9c80-200">Updates the response caching HTTP feature if `VaryByQueryKeys` is set.</span></span>

### <a name="vary"></a><span data-ttu-id="c9c80-201">Variar</span><span class="sxs-lookup"><span data-stu-id="c9c80-201">Vary</span></span>

<span data-ttu-id="c9c80-202">Esse cabeçalho é apenas gravado quando o `VaryByHeader` está definida.</span><span class="sxs-lookup"><span data-stu-id="c9c80-202">This header is only written when the `VaryByHeader` property is set.</span></span> <span data-ttu-id="c9c80-203">Ele é definido como o `Vary` valor da propriedade.</span><span class="sxs-lookup"><span data-stu-id="c9c80-203">It's set to the `Vary` property's value.</span></span> <span data-ttu-id="c9c80-204">O exemplo a seguir usa o `VaryByHeader` propriedade:</span><span class="sxs-lookup"><span data-stu-id="c9c80-204">The following sample uses the `VaryByHeader` property:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_VaryByHeader&highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_VaryByHeader&highlight=1)]

::: moniker-end

<span data-ttu-id="c9c80-205">Você pode exibir os cabeçalhos de resposta com as ferramentas de rede do seu navegador.</span><span class="sxs-lookup"><span data-stu-id="c9c80-205">You can view the response headers with your browser's network tools.</span></span> <span data-ttu-id="c9c80-206">A imagem a seguir mostra o F12 borda que saída o **rede** guia quando o `About2` método de ação é atualizado:</span><span class="sxs-lookup"><span data-stu-id="c9c80-206">The following image shows the Edge F12 output on the **Network** tab when the `About2` action method is refreshed:</span></span>

![Na guia rede de borda F12 saída quando o método de ação About2 é chamado](response/_static/vary.png)

### <a name="nostore-and-locationnone"></a><span data-ttu-id="c9c80-208">NoStore e Location.None</span><span class="sxs-lookup"><span data-stu-id="c9c80-208">NoStore and Location.None</span></span>

<span data-ttu-id="c9c80-209">`NoStore` substitui a maioria das outras propriedades.</span><span class="sxs-lookup"><span data-stu-id="c9c80-209">`NoStore` overrides most of the other properties.</span></span> <span data-ttu-id="c9c80-210">Quando essa propriedade é definida como `true`, o `Cache-Control` cabeçalho é definido como `no-store`.</span><span class="sxs-lookup"><span data-stu-id="c9c80-210">When this property is set to `true`, the `Cache-Control` header is set to `no-store`.</span></span> <span data-ttu-id="c9c80-211">Se `Location` é definido como `None`:</span><span class="sxs-lookup"><span data-stu-id="c9c80-211">If `Location` is set to `None`:</span></span>

* <span data-ttu-id="c9c80-212">`Cache-Control` é definido como `no-store,no-cache`.</span><span class="sxs-lookup"><span data-stu-id="c9c80-212">`Cache-Control` is set to `no-store,no-cache`.</span></span>
* <span data-ttu-id="c9c80-213">`Pragma` é definido como `no-cache`.</span><span class="sxs-lookup"><span data-stu-id="c9c80-213">`Pragma` is set to `no-cache`.</span></span>

<span data-ttu-id="c9c80-214">Se `NoStore` é `false` e `Location` é `None`, `Cache-Control` e `Pragma` são definidos como `no-cache`.</span><span class="sxs-lookup"><span data-stu-id="c9c80-214">If `NoStore` is `false` and `Location` is `None`, `Cache-Control` and `Pragma` are set to `no-cache`.</span></span>

<span data-ttu-id="c9c80-215">Você normalmente define `NoStore` para `true` nas páginas de erro.</span><span class="sxs-lookup"><span data-stu-id="c9c80-215">You typically set `NoStore` to `true` on error pages.</span></span> <span data-ttu-id="c9c80-216">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="c9c80-216">For example:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet1&highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet1&highlight=1)]

::: moniker-end

<span data-ttu-id="c9c80-217">Isso resulta nos seguintes cabeçalhos:</span><span class="sxs-lookup"><span data-stu-id="c9c80-217">This results in the following headers:</span></span>

```
Cache-Control: no-store,no-cache
Pragma: no-cache
```

### <a name="location-and-duration"></a><span data-ttu-id="c9c80-218">Local e duração</span><span class="sxs-lookup"><span data-stu-id="c9c80-218">Location and Duration</span></span>

<span data-ttu-id="c9c80-219">Para habilitar o cache, `Duration` deve ser definido como um valor positivo e `Location` devem ser `Any` (o padrão) ou `Client`.</span><span class="sxs-lookup"><span data-stu-id="c9c80-219">To enable caching, `Duration` must be set to a positive value and `Location` must be either `Any` (the default) or `Client`.</span></span> <span data-ttu-id="c9c80-220">Nesse caso, o `Cache-Control` cabeçalho é definido como o valor do local seguido de `max-age` da resposta.</span><span class="sxs-lookup"><span data-stu-id="c9c80-220">In this case, the `Cache-Control` header is set to the location value followed by the `max-age` of the response.</span></span>

> [!NOTE]
> <span data-ttu-id="c9c80-221">`Location`do opções de `Any` e `Client` se traduz em `Cache-Control` valores de cabeçalho de `public` e `private`, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="c9c80-221">`Location`'s options of `Any` and `Client` translate into `Cache-Control` header values of `public` and `private`, respectively.</span></span> <span data-ttu-id="c9c80-222">Conforme observado anteriormente, definindo `Location` para `None` define `Cache-Control` e `Pragma` cabeçalhos para `no-cache`.</span><span class="sxs-lookup"><span data-stu-id="c9c80-222">As noted previously, setting `Location` to `None` sets both `Cache-Control` and `Pragma` headers to `no-cache`.</span></span>

<span data-ttu-id="c9c80-223">Abaixo está um exemplo que mostra os cabeçalhos produzido definindo `Duration` e deixar o padrão `Location` valor:</span><span class="sxs-lookup"><span data-stu-id="c9c80-223">Below is an example showing the headers produced by setting `Duration` and leaving the default `Location` value:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_duration&highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_duration&highlight=1)]

::: moniker-end

<span data-ttu-id="c9c80-224">Isso gera o cabeçalho a seguir:</span><span class="sxs-lookup"><span data-stu-id="c9c80-224">This produces the following header:</span></span>

```
Cache-Control: public,max-age=60
```

### <a name="cache-profiles"></a><span data-ttu-id="c9c80-225">Perfis de cache</span><span class="sxs-lookup"><span data-stu-id="c9c80-225">Cache profiles</span></span>

<span data-ttu-id="c9c80-226">Em vez de duplicar `ResponseCache` configurações em muitos atributos de ação do controlador, perfis de cache podem ser configuradas como opções ao configurar MVC no `ConfigureServices` método `Startup`.</span><span class="sxs-lookup"><span data-stu-id="c9c80-226">Instead of duplicating `ResponseCache` settings on many controller action attributes, cache profiles can be configured as options when setting up MVC in the `ConfigureServices` method in `Startup`.</span></span> <span data-ttu-id="c9c80-227">Valores encontrados em um perfil de cache de referência são usados como padrões pelo `ResponseCache` de atributos e são substituídos por qualquer propriedade especificada no atributo.</span><span class="sxs-lookup"><span data-stu-id="c9c80-227">Values found in a referenced cache profile are used as the defaults by the `ResponseCache` attribute and are overridden by any properties specified on the attribute.</span></span>

<span data-ttu-id="c9c80-228">Configurando um perfil de cache:</span><span class="sxs-lookup"><span data-stu-id="c9c80-228">Setting up a cache profile:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Startup.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Startup.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="c9c80-229">Um perfil de cache de referência:</span><span class="sxs-lookup"><span data-stu-id="c9c80-229">Referencing a cache profile:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_controller&highlight=1,4)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_controller&highlight=1,4)]

::: moniker-end

<span data-ttu-id="c9c80-230">O `ResponseCache` atributo pode ser aplicado a ações (métodos) e controladores (classes).</span><span class="sxs-lookup"><span data-stu-id="c9c80-230">The `ResponseCache` attribute can be applied both to actions (methods) and controllers (classes).</span></span> <span data-ttu-id="c9c80-231">Atributos de nível de método substituem as configurações especificadas em atributos de nível de classe.</span><span class="sxs-lookup"><span data-stu-id="c9c80-231">Method-level attributes override the settings specified in class-level attributes.</span></span>

<span data-ttu-id="c9c80-232">No exemplo acima, um atributo de nível de classe especifica uma duração de 30 segundos, enquanto um atributo de nível de método faz referência a um perfil de cache com uma duração definida como 60 segundos.</span><span class="sxs-lookup"><span data-stu-id="c9c80-232">In the above example, a class-level attribute specifies a duration of 30 seconds, while a method-level attribute references a cache profile with a duration set to 60 seconds.</span></span>

<span data-ttu-id="c9c80-233">O cabeçalho resultante:</span><span class="sxs-lookup"><span data-stu-id="c9c80-233">The resulting header:</span></span>

```
Cache-Control: public,max-age=60
```

## <a name="additional-resources"></a><span data-ttu-id="c9c80-234">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="c9c80-234">Additional resources</span></span>

* [<span data-ttu-id="c9c80-235">Armazenar respostas em Caches</span><span class="sxs-lookup"><span data-stu-id="c9c80-235">Storing Responses in Caches</span></span>](https://tools.ietf.org/html/rfc7234#section-3)
* [<span data-ttu-id="c9c80-236">Cache-Control</span><span class="sxs-lookup"><span data-stu-id="c9c80-236">Cache-Control</span></span>](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)
* [<span data-ttu-id="c9c80-237">Cache na memória</span><span class="sxs-lookup"><span data-stu-id="c9c80-237">Cache in-memory</span></span>](xref:performance/caching/memory)
* [<span data-ttu-id="c9c80-238">Trabalhar com um cache distribuído</span><span class="sxs-lookup"><span data-stu-id="c9c80-238">Work with a distributed cache</span></span>](xref:performance/caching/distributed)
* [<span data-ttu-id="c9c80-239">Detectar alterações com tokens de alteração</span><span class="sxs-lookup"><span data-stu-id="c9c80-239">Detect changes with change tokens</span></span>](xref:fundamentals/primitives/change-tokens)
* [<span data-ttu-id="c9c80-240">Middleware de Cache de Resposta</span><span class="sxs-lookup"><span data-stu-id="c9c80-240">Response Caching Middleware</span></span>](xref:performance/caching/middleware)
* [<span data-ttu-id="c9c80-241">Auxiliar de marca de cache</span><span class="sxs-lookup"><span data-stu-id="c9c80-241">Cache Tag Helper</span></span>](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [<span data-ttu-id="c9c80-242">Auxiliar de marca de cache distribuído</span><span class="sxs-lookup"><span data-stu-id="c9c80-242">Distributed Cache Tag Helper</span></span>](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)

---
title: "Estado de sessão e de aplicativo no núcleo do ASP.NET"
author: rick-anderson
description: "Abordagens para preservar de aplicativo e o estado do usuário (sessão) entre as solicitações."
keywords: "Postagem do ASP.NET Core, o estado do aplicativo, estado de sessão, querystring,"
ms.author: riande
manager: wpickett
ms.date: 11/27/2017
ms.topic: article
ms.assetid: 18cda488-0769-4cb9-82f6-4c6685f2045d
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/app-state
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 35b34f1a40e431e59e6b9c1d9bfb4ce3fced35e6
ms.sourcegitcommit: 8f42ab93402c1b8044815e1e48d0bb84c81f8b59
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/29/2017
---
# <a name="introduction-to-session-and-application-state-in-aspnet-core"></a><span data-ttu-id="1d09b-104">Introdução ao estado de sessão e de aplicativo no núcleo do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="1d09b-104">Introduction to session and application state in ASP.NET Core</span></span>

<span data-ttu-id="1d09b-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), e [Diana LaRose](https://github.com/DianaLaRose)</span><span class="sxs-lookup"><span data-stu-id="1d09b-105">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), and [Diana LaRose](https://github.com/DianaLaRose)</span></span>

<span data-ttu-id="1d09b-106">HTTP é um protocolo sem monitoração de estado.</span><span class="sxs-lookup"><span data-stu-id="1d09b-106">HTTP is a stateless protocol.</span></span> <span data-ttu-id="1d09b-107">Um servidor web trata cada solicitação HTTP como uma solicitação independente e não manter valores de usuário de solicitações anteriores.</span><span class="sxs-lookup"><span data-stu-id="1d09b-107">A web server treats each HTTP request as an independent request and does not retain user values from previous requests.</span></span> <span data-ttu-id="1d09b-108">Este artigo aborda diferentes maneiras de preservar o estado da sessão entre as solicitações e aplicativo.</span><span class="sxs-lookup"><span data-stu-id="1d09b-108">This article discusses different ways to preserve application and session state between requests.</span></span> 

## <a name="session-state"></a><span data-ttu-id="1d09b-109">Estado da sessão</span><span class="sxs-lookup"><span data-stu-id="1d09b-109">Session state</span></span>

<span data-ttu-id="1d09b-110">O estado de sessão é um recurso do ASP.NET Core que você pode usar para salvar e armazenar dados de usuário enquanto o usuário navega seu aplicativo Web.</span><span class="sxs-lookup"><span data-stu-id="1d09b-110">Session state is a feature in ASP.NET Core that you can use to save and store user data while the user browses your web app.</span></span> <span data-ttu-id="1d09b-111">Estado de sessão consiste em uma tabela de hash ou dicionário no servidor, persiste dados em solicitações de um navegador.</span><span class="sxs-lookup"><span data-stu-id="1d09b-111">Consisting of a dictionary or hash table on the server, session state persists data across requests from a browser.</span></span> <span data-ttu-id="1d09b-112">Os dados da sessão são apoiados por um cache.</span><span class="sxs-lookup"><span data-stu-id="1d09b-112">The session data is backed by a cache.</span></span>

<span data-ttu-id="1d09b-113">ASP.NET Core mantém o estado de sessão, fornecendo um cookie que contém a ID de sessão, que é enviada para o servidor com cada solicitação de cliente.</span><span class="sxs-lookup"><span data-stu-id="1d09b-113">ASP.NET Core maintains session state by giving the client a cookie that contains the session ID, which is sent to the server with each request.</span></span> <span data-ttu-id="1d09b-114">O servidor usa a ID de sessão para buscar os dados da sessão.</span><span class="sxs-lookup"><span data-stu-id="1d09b-114">The server uses the session ID to fetch the session data.</span></span> <span data-ttu-id="1d09b-115">Como o cookie de sessão é específico para o navegador, você não pode compartilhar sessões entre navegadores.</span><span class="sxs-lookup"><span data-stu-id="1d09b-115">Because the session cookie is specific to the browser, you cannot share sessions across browsers.</span></span> <span data-ttu-id="1d09b-116">Cookies de sessão são excluídos somente quando termina a sessão do navegador.</span><span class="sxs-lookup"><span data-stu-id="1d09b-116">Session cookies are deleted only when the browser session ends.</span></span> <span data-ttu-id="1d09b-117">Se um cookie for recebido de uma sessão expirada, uma nova sessão que usa o mesmo cookie de sessão é criada.</span><span class="sxs-lookup"><span data-stu-id="1d09b-117">If a cookie is received for an expired session, a new session that uses the same session cookie  is created.</span></span> 

<span data-ttu-id="1d09b-118">O servidor mantém uma sessão por um tempo limitado, após a última solicitação.</span><span class="sxs-lookup"><span data-stu-id="1d09b-118">The server retains a session for a limited time after the last request.</span></span> <span data-ttu-id="1d09b-119">Você pode definir o tempo limite da sessão ou use o valor padrão de 20 minutos.</span><span class="sxs-lookup"><span data-stu-id="1d09b-119">You can either set the session timeout or use the default value of 20 minutes.</span></span> <span data-ttu-id="1d09b-120">Estado da sessão é ideal para armazenar dados de usuário que são específico para uma sessão específica, mas não precisam ser mantidos permanentemente.</span><span class="sxs-lookup"><span data-stu-id="1d09b-120">Session state is ideal for storing user data that is specific to a particular session but doesn’t need to be persisted permanently.</span></span> <span data-ttu-id="1d09b-121">Dados são excluídos do armazenamento de backup ou quando você chamar `Session.Clear` ou quando a sessão expira no repositório de dados.</span><span class="sxs-lookup"><span data-stu-id="1d09b-121">Data is deleted from the backing store either when you call `Session.Clear` or when the session expires in the data store.</span></span> <span data-ttu-id="1d09b-122">O servidor não sabe quando o navegador for fechado ou quando o cookie de sessão é excluído.</span><span class="sxs-lookup"><span data-stu-id="1d09b-122">The server does not know when the browser is closed or when the session cookie is deleted.</span></span>

> [!WARNING]
> <span data-ttu-id="1d09b-123">Não armazene dados confidenciais na sessão.</span><span class="sxs-lookup"><span data-stu-id="1d09b-123">Do not store sensitive data in session.</span></span> <span data-ttu-id="1d09b-124">O cliente não pode fechar o navegador e limpar o cookie de sessão (e alguns navegadores atividade cookies de sessão em windows).</span><span class="sxs-lookup"><span data-stu-id="1d09b-124">The client might not close the browser and clear the session cookie (and some browsers keep session cookies alive across windows).</span></span> <span data-ttu-id="1d09b-125">Além disso, uma sessão não pode ser restrita a um único usuário; o próximo usuário pode continuar com a mesma sessão.</span><span class="sxs-lookup"><span data-stu-id="1d09b-125">Also, a session might not be restricted to a single user; the next user might continue with the same session.</span></span>

<span data-ttu-id="1d09b-126">O provedor de sessão na memória armazena dados de sessão no servidor local.</span><span class="sxs-lookup"><span data-stu-id="1d09b-126">The in-memory session provider stores session data on the local server.</span></span> <span data-ttu-id="1d09b-127">Se você planeja executar seu aplicativo web em um farm de servidores, você deve usar sessões Autoadesivas para ligar cada sessão para um servidor específico.</span><span class="sxs-lookup"><span data-stu-id="1d09b-127">If you plan to run your web app on a server farm, you must use sticky sessions to tie each session to a specific server.</span></span> <span data-ttu-id="1d09b-128">A plataforma Windows Azure Web Sites padrão sessões Autoadesivas (Application Request Routing ou ARR).</span><span class="sxs-lookup"><span data-stu-id="1d09b-128">The Windows Azure Web Sites platform defaults to sticky sessions (Application Request Routing or ARR).</span></span> <span data-ttu-id="1d09b-129">Entretanto, sessões Autoadesivas podem afetar a escalabilidade e complicar as atualizações de aplicativo web.</span><span class="sxs-lookup"><span data-stu-id="1d09b-129">However, sticky sessions can affect scalability and complicate web app updates.</span></span> <span data-ttu-id="1d09b-130">Uma opção melhor é usar o Redis ou distribuídas do SQL Server armazena em cache, que não exigem sessões Autoadesivas.</span><span class="sxs-lookup"><span data-stu-id="1d09b-130">A better option is to use the Redis or SQL Server distributed caches, which don't require sticky sessions.</span></span> <span data-ttu-id="1d09b-131">Para obter mais informações, consulte [trabalhando com um Cache distribuído](xref:performance/caching/distributed).</span><span class="sxs-lookup"><span data-stu-id="1d09b-131">For more information, see [Working with a Distributed Cache](xref:performance/caching/distributed).</span></span> <span data-ttu-id="1d09b-132">Para obter detalhes sobre como configurar provedores de serviços, consulte [sessão Configurando](#configuring-session) posteriormente neste artigo.</span><span class="sxs-lookup"><span data-stu-id="1d09b-132">For details on setting up service providers, see [Configuring Session](#configuring-session) later in this article.</span></span>

<a name="temp"></a>
## <a name="tempdata"></a><span data-ttu-id="1d09b-133">TempData</span><span class="sxs-lookup"><span data-stu-id="1d09b-133">TempData</span></span>

<span data-ttu-id="1d09b-134">ASP.NET MVC de núcleo expõe o [TempData](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) propriedade em uma [controlador](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.controller?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="1d09b-134">ASP.NET Core MVC exposes the [TempData](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) property on a [controller](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.controller?view=aspnetcore-2.0).</span></span> <span data-ttu-id="1d09b-135">Essa propriedade armazena dados até eles serem lidos.</span><span class="sxs-lookup"><span data-stu-id="1d09b-135">This property stores data until it is read.</span></span> <span data-ttu-id="1d09b-136">Os métodos `Keep` e `Peek` podem ser usados para examinar os dados sem exclusão.</span><span class="sxs-lookup"><span data-stu-id="1d09b-136">The `Keep` and `Peek` methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="1d09b-137">`TempData`é particularmente útil para redirecionamento, quando dados são necessários para mais de uma única solicitação.</span><span class="sxs-lookup"><span data-stu-id="1d09b-137">`TempData` is particularly useful for redirection, when data is needed for more than a single request.</span></span> <span data-ttu-id="1d09b-138">`TempData`é implementado por provedores de TempData, por exemplo, usar cookies ou estado de sessão.</span><span class="sxs-lookup"><span data-stu-id="1d09b-138">`TempData` is implemented by TempData providers, for example, using either cookies or session state.</span></span>

<a name="tempdata-providers"></a>
### <a name="tempdata-providers"></a><span data-ttu-id="1d09b-139">Provedores de TempData</span><span class="sxs-lookup"><span data-stu-id="1d09b-139">TempData providers</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="1d09b-140">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="1d09b-140">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="1d09b-141">No ASP.NET Core 2.0 e posterior, o provedor de TempData baseada em cookie é usado por padrão para armazenar TempData em cookies.</span><span class="sxs-lookup"><span data-stu-id="1d09b-141">In ASP.NET Core 2.0 and later, the cookie-based TempData provider is used by default to store TempData in cookies.</span></span>

<span data-ttu-id="1d09b-142">Os dados do cookie são codificados com o [Base64UrlTextEncoder](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.webutilities.base64urltextencoder?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="1d09b-142">The cookie data is encoded with the [Base64UrlTextEncoder](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.webutilities.base64urltextencoder?view=aspnetcore-2.0).</span></span> <span data-ttu-id="1d09b-143">Porque o cookie é criptografado e em partes, o cookie único tamanho limite encontrado no núcleo do ASP.NET 1. x não se aplica.</span><span class="sxs-lookup"><span data-stu-id="1d09b-143">Because the cookie is encrypted and chunked, the single cookie size limit found in ASP.NET Core 1.x does not apply.</span></span> <span data-ttu-id="1d09b-144">Os dados do cookie não são compactados porque a compactação de dados criptografados pode levar a problemas de segurança, como o [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) e [violação](https://wikipedia.org/wiki/BREACH_(security_exploit)) ataques.</span><span class="sxs-lookup"><span data-stu-id="1d09b-144">The cookie data is not compressed because compressing encrypted data can lead to security problems such as the [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) and [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)) attacks.</span></span> <span data-ttu-id="1d09b-145">Para obter mais informações sobre o provedor de TempData baseada em cookie, consulte [CookieTempDataProvider](https://github.com/aspnet/Mvc/blob/dev/src/Microsoft.AspNetCore.Mvc.ViewFeatures/ViewFeatures/CookieTempDataProvider.cs).</span><span class="sxs-lookup"><span data-stu-id="1d09b-145">For more information on the cookie-based TempData provider, see [CookieTempDataProvider](https://github.com/aspnet/Mvc/blob/dev/src/Microsoft.AspNetCore.Mvc.ViewFeatures/ViewFeatures/CookieTempDataProvider.cs).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="1d09b-146">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="1d09b-146">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="1d09b-147">No ASP.NET Core 1.0 e 1.1, o provedor de TempData de estado de sessão é o padrão.</span><span class="sxs-lookup"><span data-stu-id="1d09b-147">In ASP.NET Core 1.0 and 1.1, the session state TempData provider is the default.</span></span>

--------------

<a name="choose-temp"></a>
### <a name="choosing-a-tempdata-provider"></a><span data-ttu-id="1d09b-148">Escolha de um provedor de TempData</span><span class="sxs-lookup"><span data-stu-id="1d09b-148">Choosing a TempData provider</span></span>

<span data-ttu-id="1d09b-149">Escolha de um provedor de TempData envolve várias considerações, como:</span><span class="sxs-lookup"><span data-stu-id="1d09b-149">Choosing a TempData provider involves several considerations, such as:</span></span>

1. <span data-ttu-id="1d09b-150">O aplicativo já usa o estado da sessão para outras finalidades?</span><span class="sxs-lookup"><span data-stu-id="1d09b-150">Does the application already use session state for other purposes?</span></span> <span data-ttu-id="1d09b-151">Nesse caso, usar o provedor de TempData de estado de sessão tem sem custo adicional para o aplicativo (além do tamanho dos dados).</span><span class="sxs-lookup"><span data-stu-id="1d09b-151">If so, using the session state TempData provider has no additional cost to the application (aside from the size of the data).</span></span>
2. <span data-ttu-id="1d09b-152">O aplicativo usa TempData somente com moderação, para quantidades relativamente pequenas de dados (até 500 bytes)?</span><span class="sxs-lookup"><span data-stu-id="1d09b-152">Does the application use TempData only sparingly, for relatively small amounts of data (up to 500 bytes)?</span></span> <span data-ttu-id="1d09b-153">Se assim, o provedor de TempData cookie adicionará um pequeno custo para cada solicitação que transporta TempData.</span><span class="sxs-lookup"><span data-stu-id="1d09b-153">If so, the cookie TempData provider will add a small cost to each request that carries TempData.</span></span> <span data-ttu-id="1d09b-154">Caso contrário, o provedor de TempData de estado de sessão pode ser útil evitar o ciclo uma grande quantidade de dados em cada solicitação até que o TempData é consumido.</span><span class="sxs-lookup"><span data-stu-id="1d09b-154">If not, the session state TempData provider can be beneficial to avoid round-tripping a large amount of data in each request until the TempData is consumed.</span></span>
3. <span data-ttu-id="1d09b-155">O aplicativo é executado em um farm da web (vários servidores)?</span><span class="sxs-lookup"><span data-stu-id="1d09b-155">Does the application run in a web farm (multiple servers)?</span></span> <span data-ttu-id="1d09b-156">Nesse caso, não há nenhuma configuração adicional necessária para usar o provedor de TempData do cookie.</span><span class="sxs-lookup"><span data-stu-id="1d09b-156">If so, there is no additional configuration needed to use the cookie TempData provider.</span></span>

> [!NOTE]
> <span data-ttu-id="1d09b-157">A maioria dos clientes da web (como navegadores da web) impõem limites sobre o tamanho máximo de cada cookie, o número total de cookies ou ambos.</span><span class="sxs-lookup"><span data-stu-id="1d09b-157">Most web clients (such as web browsers) enforce limits on the maximum size of each cookie, the total number of cookies, or both.</span></span> <span data-ttu-id="1d09b-158">Portanto, ao usar o provedor de TempData cookies, verifique se que o aplicativo não excede esses limites.</span><span class="sxs-lookup"><span data-stu-id="1d09b-158">Therefore, when using the cookie TempData provider, verify the app won't exceed these limits.</span></span> <span data-ttu-id="1d09b-159">Considere o tamanho total dos dados, considerando as sobrecargas de criptografia e agrupamento.</span><span class="sxs-lookup"><span data-stu-id="1d09b-159">Consider the total size of the data, accounting for the overheads of encryption and chunking.</span></span>

<a name="config-temp"></a>
### <a name="configure-the-tempdata-provider"></a><span data-ttu-id="1d09b-160">Configurar o provedor de TempData</span><span class="sxs-lookup"><span data-stu-id="1d09b-160">Configure the TempData provider</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="1d09b-161">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="1d09b-161">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="1d09b-162">O provedor de TempData baseada em cookie é habilitado por padrão.</span><span class="sxs-lookup"><span data-stu-id="1d09b-162">The cookie-based TempData provider is enabled by default.</span></span> <span data-ttu-id="1d09b-163">O seguinte `Startup` código da classe configura o provedor de TempData baseadas em sessão:</span><span class="sxs-lookup"><span data-stu-id="1d09b-163">The following `Startup` class code configures the session-based TempData provider:</span></span>

[!code-csharp[](app-state/sample/src/WebAppSessionDotNetCore2.0App/StartupTempDataSession.cs?name=snippet_TempDataSession&highlight=4,6,11)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="1d09b-164">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="1d09b-164">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="1d09b-165">O seguinte `Startup` código da classe configura o provedor de TempData baseadas em sessão:</span><span class="sxs-lookup"><span data-stu-id="1d09b-165">The following `Startup` class code configures the session-based TempData provider:</span></span>

[!code-csharp[](app-state/sample/src/WebAppSession/StartupTempDataSession.cs?name=snippet_TempDataSession&highlight=4,9)]

---

<span data-ttu-id="1d09b-166">A ordem é essencial para componentes de middleware.</span><span class="sxs-lookup"><span data-stu-id="1d09b-166">Ordering is critical for middleware components.</span></span> <span data-ttu-id="1d09b-167">No exemplo anterior, uma exceção do tipo `InvalidOperationException` ocorre quando `UseSession` é invocado após `UseMvcWithDefaultRoute`.</span><span class="sxs-lookup"><span data-stu-id="1d09b-167">In the preceding example, an exception of type `InvalidOperationException` occurs when `UseSession` is invoked after `UseMvcWithDefaultRoute`.</span></span> <span data-ttu-id="1d09b-168">Consulte [ordenação de Middleware](xref:fundamentals/middleware#ordering) para obter mais detalhes.</span><span class="sxs-lookup"><span data-stu-id="1d09b-168">See [Middleware Ordering](xref:fundamentals/middleware#ordering) for more detail.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1d09b-169">Se o destino do .NET Framework e usando o provedor baseadas em sessão, adicione o [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session) pacote NuGet ao seu projeto.</span><span class="sxs-lookup"><span data-stu-id="1d09b-169">If targeting .NET Framework and using the session-based provider, add the [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session) NuGet package to your project.</span></span>

## <a name="query-strings"></a><span data-ttu-id="1d09b-170">Cadeias de caracteres de consulta</span><span class="sxs-lookup"><span data-stu-id="1d09b-170">Query strings</span></span>

<span data-ttu-id="1d09b-171">Você pode passar uma quantidade limitada de dados de uma solicitação para outro, adicionando-a cadeia de caracteres de consulta da nova solicitação.</span><span class="sxs-lookup"><span data-stu-id="1d09b-171">You can pass a limited amount of data from one request to another by adding it to the new request’s query string.</span></span> <span data-ttu-id="1d09b-172">Isso é útil para capturar o estado de uma maneira persistente que permite que os links com estado inserido deve ser compartilhado por email ou por redes sociais.</span><span class="sxs-lookup"><span data-stu-id="1d09b-172">This is useful for capturing state in a persistent manner that allows links with embedded state to be shared through email or social networks.</span></span> <span data-ttu-id="1d09b-173">No entanto, por esse motivo, você nunca deve usar cadeias de caracteres de consulta para dados confidenciais.</span><span class="sxs-lookup"><span data-stu-id="1d09b-173">However, for this reason, you should never use query strings for sensitive data.</span></span> <span data-ttu-id="1d09b-174">Além de ser facilmente compartilhados, inclusive dados em cadeias de caracteres de consulta pode criar oportunidades de [falsificação de solicitação entre sites (CSRF)](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) ataques, que podem enganar os usuários a visitar sites mal-intencionados enquanto autenticado.</span><span class="sxs-lookup"><span data-stu-id="1d09b-174">In addition to being easily shared, including data in query strings can create opportunities for [Cross-Site Request Forgery (CSRF)](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) attacks, which can trick users into visiting malicious sites while authenticated.</span></span> <span data-ttu-id="1d09b-175">Os invasores podem roubar dados do usuário de seu aplicativo ou executar ações mal-intencionadas em nome do usuário.</span><span class="sxs-lookup"><span data-stu-id="1d09b-175">Attackers can then steal user data from your app or take malicious actions on behalf of the user.</span></span> <span data-ttu-id="1d09b-176">Qualquer estado preservado application ou session deve proteger contra ataques CSRF.</span><span class="sxs-lookup"><span data-stu-id="1d09b-176">Any preserved application or session state must protect against CSRF attacks.</span></span> <span data-ttu-id="1d09b-177">Para obter mais informações sobre ataques CSRF, consulte [ataques impedindo intersite solicitar CSRF (falsificação XSRF /) no núcleo do ASP.NET](../security/anti-request-forgery.md).</span><span class="sxs-lookup"><span data-stu-id="1d09b-177">For more information on CSRF attacks, see [Preventing Cross-Site Request Forgery (XSRF/CSRF) Attacks in ASP.NET Core](../security/anti-request-forgery.md).</span></span>

## <a name="post-data-and-hidden-fields"></a><span data-ttu-id="1d09b-178">Dados de postagem e campos ocultos</span><span class="sxs-lookup"><span data-stu-id="1d09b-178">Post data and hidden fields</span></span>

<span data-ttu-id="1d09b-179">Dados podem ser salvos em campos de formulário oculto e lançados novamente na próxima solicitação.</span><span class="sxs-lookup"><span data-stu-id="1d09b-179">Data can be saved in hidden form fields and posted back on the next request.</span></span> <span data-ttu-id="1d09b-180">Isso é comum em formulários de várias páginas.</span><span class="sxs-lookup"><span data-stu-id="1d09b-180">This is common in multi-page forms.</span></span> <span data-ttu-id="1d09b-181">No entanto, como o cliente pode potencialmente adulterar os dados, o servidor deve sempre validá-lo novamente.</span><span class="sxs-lookup"><span data-stu-id="1d09b-181">However, because the client can potentially tamper with the data, the server must always revalidate it.</span></span> 

## <a name="cookies"></a><span data-ttu-id="1d09b-182">Cookies</span><span class="sxs-lookup"><span data-stu-id="1d09b-182">Cookies</span></span>

<span data-ttu-id="1d09b-183">Cookies fornecem uma maneira de armazenar dados específicos do usuário em aplicativos da web.</span><span class="sxs-lookup"><span data-stu-id="1d09b-183">Cookies provide a way to store user-specific data in web applications.</span></span> <span data-ttu-id="1d09b-184">Porque os cookies são enviados com cada solicitação, seu tamanho deve ser reduzido ao mínimo.</span><span class="sxs-lookup"><span data-stu-id="1d09b-184">Because cookies are sent with every request, their size should be kept to a minimum.</span></span> <span data-ttu-id="1d09b-185">Idealmente, apenas um identificador deve ser armazenado em um cookie com os dados reais armazenados no servidor.</span><span class="sxs-lookup"><span data-stu-id="1d09b-185">Ideally, only an identifier should be stored in a cookie with the actual data stored on the server.</span></span> <span data-ttu-id="1d09b-186">A maioria dos navegadores restringir cookies 4096 bytes.</span><span class="sxs-lookup"><span data-stu-id="1d09b-186">Most browsers restrict cookies to 4096 bytes.</span></span> <span data-ttu-id="1d09b-187">Além disso, somente um número limitado dos cookies está disponível para cada domínio.</span><span class="sxs-lookup"><span data-stu-id="1d09b-187">In addition, only a limited number of cookies are available for each domain.</span></span>  

<span data-ttu-id="1d09b-188">Como os cookies estão sujeitos a falsificações, eles devem ser validados no servidor.</span><span class="sxs-lookup"><span data-stu-id="1d09b-188">Because cookies are subject to tampering, they must be validated on the server.</span></span> <span data-ttu-id="1d09b-189">Embora a durabilidade do cookie em um cliente esteja sujeito a expiração e a intervenção do usuário, elas geralmente são a forma mais durável de persistência de dados no cliente.</span><span class="sxs-lookup"><span data-stu-id="1d09b-189">Although the durability of the cookie on a client is subject to user intervention and expiration, they are generally the most durable form of data persistence on the client.</span></span>

<span data-ttu-id="1d09b-190">Cookies são usados para personalização, onde o conteúdo é personalizado para um usuário conhecido.</span><span class="sxs-lookup"><span data-stu-id="1d09b-190">Cookies are often used for personalization, where content is customized for a known user.</span></span> <span data-ttu-id="1d09b-191">Como o usuário é identificado apenas e não autenticado na maioria dos casos, você normalmente pode proteger um cookie ao armazenar o nome de usuário, nome da conta ou uma ID de usuário exclusiva (como um GUID) no cookie.</span><span class="sxs-lookup"><span data-stu-id="1d09b-191">Because the user is only identified and not authenticated in most cases, you can typically secure a cookie by storing the user name, account name, or a unique user ID (such as a GUID) in the cookie.</span></span> <span data-ttu-id="1d09b-192">Você pode usar o cookie para acessar a infraestrutura de personalização de usuário de um site.</span><span class="sxs-lookup"><span data-stu-id="1d09b-192">You can then use the cookie to access the user personalization infrastructure of a site.</span></span>

## <a name="httpcontextitems"></a><span data-ttu-id="1d09b-193">HttpContext</span><span class="sxs-lookup"><span data-stu-id="1d09b-193">HttpContext.Items</span></span>

<span data-ttu-id="1d09b-194">O `Items` coleção é um bom local para armazenar dados que é necessário somente durante processamento de uma determinada solicitação.</span><span class="sxs-lookup"><span data-stu-id="1d09b-194">The `Items` collection is a good location to store data that is needed only while processing one particular request.</span></span> <span data-ttu-id="1d09b-195">O conteúdo da coleção é descartado após cada solicitação.</span><span class="sxs-lookup"><span data-stu-id="1d09b-195">The collection's contents are discarded after each request.</span></span> <span data-ttu-id="1d09b-196">O `Items` coleção melhor é usada como uma maneira de componentes ou middleware para comunicar-se quando eles operam em pontos diferentes durante uma solicitação e não têm nenhuma maneira direta para passar parâmetros.</span><span class="sxs-lookup"><span data-stu-id="1d09b-196">The `Items` collection is best used as a way for components or middleware to communicate when they operate at different points in time during a request and have no direct way to pass parameters.</span></span> <span data-ttu-id="1d09b-197">Para obter mais informações, consulte [trabalhando com HttpContext](#working-with-httpcontextitems), mais adiante neste artigo.</span><span class="sxs-lookup"><span data-stu-id="1d09b-197">For more information, see [Working with HttpContext.Items](#working-with-httpcontextitems), later in this article.</span></span>

## <a name="cache"></a><span data-ttu-id="1d09b-198">Cache</span><span class="sxs-lookup"><span data-stu-id="1d09b-198">Cache</span></span>

<span data-ttu-id="1d09b-199">O cache é uma maneira eficiente de armazenar e recuperar dados.</span><span class="sxs-lookup"><span data-stu-id="1d09b-199">Caching is an efficient way to store and retrieve data.</span></span> <span data-ttu-id="1d09b-200">Você pode controlar o tempo de vida de itens em cache com base na hora e outras considerações.</span><span class="sxs-lookup"><span data-stu-id="1d09b-200">You can control the lifetime of cached items based on time and other considerations.</span></span> <span data-ttu-id="1d09b-201">Saiba mais sobre [cache](../performance/caching/index.md).</span><span class="sxs-lookup"><span data-stu-id="1d09b-201">Learn more about [Caching](../performance/caching/index.md).</span></span>

<a name="session"></a>
## <a name="working-with-session-state"></a><span data-ttu-id="1d09b-202">Trabalhando com o estado de sessão</span><span class="sxs-lookup"><span data-stu-id="1d09b-202">Working with Session State</span></span>

### <a name="configuring-session"></a><span data-ttu-id="1d09b-203">Configuração de sessão</span><span class="sxs-lookup"><span data-stu-id="1d09b-203">Configuring Session</span></span>

<span data-ttu-id="1d09b-204">O `Microsoft.AspNetCore.Session` pacote fornece middleware para gerenciar o estado da sessão.</span><span class="sxs-lookup"><span data-stu-id="1d09b-204">The `Microsoft.AspNetCore.Session` package provides middleware for managing session state.</span></span> <span data-ttu-id="1d09b-205">Para habilitar o middleware de sessão, `Startup` deve conter:</span><span class="sxs-lookup"><span data-stu-id="1d09b-205">To enable the session middleware, `Startup` must contain:</span></span>

- <span data-ttu-id="1d09b-206">Qualquer uma da [IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache) caches de memória.</span><span class="sxs-lookup"><span data-stu-id="1d09b-206">Any of the [IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache) memory caches.</span></span> <span data-ttu-id="1d09b-207">O `IDistributedCache` implementação é usada como um repositório de backup para a sessão.</span><span class="sxs-lookup"><span data-stu-id="1d09b-207">The `IDistributedCache` implementation is used as a backing store for session.</span></span>
- <span data-ttu-id="1d09b-208">[AddSession](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions#Microsoft_Extensions_DependencyInjection_SessionServiceCollectionExtensions_AddSession_Microsoft_Extensions_DependencyInjection_IServiceCollection_) chamar, que requer o pacote do NuGet "Microsoft.AspNetCore.Session".</span><span class="sxs-lookup"><span data-stu-id="1d09b-208">[AddSession](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions#Microsoft_Extensions_DependencyInjection_SessionServiceCollectionExtensions_AddSession_Microsoft_Extensions_DependencyInjection_IServiceCollection_) call, which requires NuGet package "Microsoft.AspNetCore.Session".</span></span>
- <span data-ttu-id="1d09b-209">[UseSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions#methods_) chamar.</span><span class="sxs-lookup"><span data-stu-id="1d09b-209">[UseSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions#methods_) call.</span></span>

<span data-ttu-id="1d09b-210">O código a seguir mostra como configurar o provedor de sessão na memória.</span><span class="sxs-lookup"><span data-stu-id="1d09b-210">The following code shows how to set up the in-memory session provider.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="1d09b-211">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="1d09b-211">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](app-state/sample/src/WebAppSessionDotNetCore2.0App/Startup.cs?highlight=11-19,24)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="1d09b-212">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="1d09b-212">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](app-state/sample/src/WebAppSession/Startup.cs?highlight=11-19,24)]

---

<span data-ttu-id="1d09b-213">Você pode fazer referência a sessão de `HttpContext` quando ele é instalado e configurado.</span><span class="sxs-lookup"><span data-stu-id="1d09b-213">You can reference Session from `HttpContext` once it is installed and configured.</span></span>

<span data-ttu-id="1d09b-214">Se você tentar acessar `Session` antes de `UseSession` tiver sido chamado, a exceção `InvalidOperationException: Session has not been configured for this application or request` é lançada.</span><span class="sxs-lookup"><span data-stu-id="1d09b-214">If you try to access `Session` before `UseSession` has been called, the exception `InvalidOperationException: Session has not been configured for this application or request` is thrown.</span></span>

<span data-ttu-id="1d09b-215">Se você tentar criar um novo `Session` (ou seja, nenhum cookie de sessão foi criado) depois que você já começou a gravar o `Response` fluxo, a exceção `InvalidOperationException: The session cannot be established after the response has started` é lançada.</span><span class="sxs-lookup"><span data-stu-id="1d09b-215">If you try to create a new `Session` (that is, no session cookie has been created) after you have already begun writing to the `Response` stream, the exception `InvalidOperationException: The session cannot be established after the response has started` is thrown.</span></span> <span data-ttu-id="1d09b-216">A exceção pode ser encontrada no log do servidor web; ele não será exibido no navegador.</span><span class="sxs-lookup"><span data-stu-id="1d09b-216">The exception can be found in the web server log; it will not be displayed in the browser.</span></span>

### <a name="loading-session-asynchronously"></a><span data-ttu-id="1d09b-217">Carregamento de sessão de forma assíncrona</span><span class="sxs-lookup"><span data-stu-id="1d09b-217">Loading Session asynchronously</span></span> 

<span data-ttu-id="1d09b-218">O provedor de sessão padrão no ASP.NET Core carrega o registro da sessão de subjacente [IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache) repositório de forma assíncrona somente se o [ISession.LoadAsync](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession#Microsoft_AspNetCore_Http_ISession_LoadAsync) método for chamado explicitamente antes  o `TryGetValue`, `Set`, ou `Remove` métodos.</span><span class="sxs-lookup"><span data-stu-id="1d09b-218">The default session provider in ASP.NET Core loads the session record from the underlying [IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache) store asynchronously only if the [ISession.LoadAsync](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession#Microsoft_AspNetCore_Http_ISession_LoadAsync) method is explicitly called before  the `TryGetValue`, `Set`, or `Remove` methods.</span></span> <span data-ttu-id="1d09b-219">Se `LoadAsync` não for chamado pela primeira vez, a base de registro de sessão é carregado de forma síncrona, que pode afetar a capacidade de dimensionar do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="1d09b-219">If `LoadAsync` is not called first, the underlying session record is loaded synchronously, which could potentially impact the ability of the app to scale.</span></span>

<span data-ttu-id="1d09b-220">Para que aplicativos impor esse padrão, encapsule o [DistributedSessionStore](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsessionstore) e [DistributedSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsession) implementações com versões que geram uma exceção se o `LoadAsync` método não é chamado antes de `TryGetValue`, `Set`, ou `Remove`.</span><span class="sxs-lookup"><span data-stu-id="1d09b-220">To have applications enforce this pattern, wrap the [DistributedSessionStore](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsessionstore) and [DistributedSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsession) implementations with versions that throw an exception if the `LoadAsync` method is not called before `TryGetValue`, `Set`, or `Remove`.</span></span> <span data-ttu-id="1d09b-221">Registre as versões encapsuladas no contêiner de serviços.</span><span class="sxs-lookup"><span data-stu-id="1d09b-221">Register the wrapped versions in the services container.</span></span>

### <a name="implementation-details"></a><span data-ttu-id="1d09b-222">Detalhes de implementação</span><span class="sxs-lookup"><span data-stu-id="1d09b-222">Implementation Details</span></span>

<span data-ttu-id="1d09b-223">Sessão usa um cookie para controlar e identificar as solicitações de um navegador único.</span><span class="sxs-lookup"><span data-stu-id="1d09b-223">Session uses a cookie to track and identify requests from a single browser.</span></span> <span data-ttu-id="1d09b-224">Por padrão, esse cookie é denominado ". AspNet.Session"e usa um caminho de"/".</span><span class="sxs-lookup"><span data-stu-id="1d09b-224">By default, this cookie is named ".AspNet.Session", and it uses a path of "/".</span></span> <span data-ttu-id="1d09b-225">Como o padrão de cookie não especificar um domínio, ele não ficam disponíveis para o script do lado do cliente na página (como `CookieHttpOnly` padrão é `true`).</span><span class="sxs-lookup"><span data-stu-id="1d09b-225">Because the cookie default does not specify a domain, it is not made available to the client-side script on the page (because `CookieHttpOnly` defaults to `true`).</span></span>

<span data-ttu-id="1d09b-226">Para substituir os padrões de sessão, use `SessionOptions`:</span><span class="sxs-lookup"><span data-stu-id="1d09b-226">To override session defaults, use `SessionOptions`:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="1d09b-227">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="1d09b-227">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](app-state/sample/src/WebAppSessionDotNetCore2.0App/StartupCopy.cs?name=snippet1&highlight=8-12)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="1d09b-228">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="1d09b-228">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](app-state/sample/src/WebAppSession/StartupCopy.cs?name=snippet1&highlight=8-12)]

---

<span data-ttu-id="1d09b-229">O servidor usa o `IdleTimeout` propriedade para determinar quanto tempo uma sessão pode ficar ociosa antes de seu conteúdo está sendo abandonado.</span><span class="sxs-lookup"><span data-stu-id="1d09b-229">The server uses the `IdleTimeout` property to determine how long a session can be idle before its contents are abandoned.</span></span> <span data-ttu-id="1d09b-230">Essa propriedade é independente da expiração do cookie.</span><span class="sxs-lookup"><span data-stu-id="1d09b-230">This property is independent of the cookie expiration.</span></span> <span data-ttu-id="1d09b-231">Cada solicitação que passa por meio do middleware de sessão (lida ou gravada para) redefine o tempo limite.</span><span class="sxs-lookup"><span data-stu-id="1d09b-231">Each request that passes through the Session middleware (read from or written to) resets the timeout.</span></span>

<span data-ttu-id="1d09b-232">Porque `Session` é *não bloqueio*, se duas solicitações tentam modificar o conteúdo da sessão, o último deles substitui o primeiro.</span><span class="sxs-lookup"><span data-stu-id="1d09b-232">Because `Session` is *non-locking*, if two requests both attempt to modify the contents of session, the last one overrides the first.</span></span> <span data-ttu-id="1d09b-233">`Session`é implementado como um *sessão coerente*, que significa que todo o conteúdo é armazenado juntos.</span><span class="sxs-lookup"><span data-stu-id="1d09b-233">`Session` is implemented as a *coherent session*, which means that all the contents are stored together.</span></span> <span data-ttu-id="1d09b-234">Duas solicitações que estão modificando diferentes partes da sessão (chaves diferentes) ainda podem afetar uns aos outros.</span><span class="sxs-lookup"><span data-stu-id="1d09b-234">Two requests that are modifying different parts of the session (different keys) might still impact each other.</span></span>

### <a name="setting-and-getting-session-values"></a><span data-ttu-id="1d09b-235">Configuração e obter valores de sessão</span><span class="sxs-lookup"><span data-stu-id="1d09b-235">Setting and getting Session values</span></span>

<span data-ttu-id="1d09b-236">Sessão é acessada por meio de `Session` propriedade `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="1d09b-236">Session is accessed through the `Session` property on `HttpContext`.</span></span> <span data-ttu-id="1d09b-237">Esta propriedade é um [ISession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession) implementação.</span><span class="sxs-lookup"><span data-stu-id="1d09b-237">This property is an [ISession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession) implementation.</span></span>

<span data-ttu-id="1d09b-238">O exemplo a seguir mostra a configuração e a obtenção de um inteiro e uma cadeia de caracteres:</span><span class="sxs-lookup"><span data-stu-id="1d09b-238">The following example shows setting and getting an int and a string:</span></span>

[!code-csharp[Main](app-state/sample/src/WebAppSession/Controllers/HomeController.cs?range=8-27,49)]

<span data-ttu-id="1d09b-239">Se você adicionar os seguintes métodos de extensão, você pode definir e obter objetos serializáveis a sessão:</span><span class="sxs-lookup"><span data-stu-id="1d09b-239">If you add the following extension methods, you can set and get serializable objects to Session:</span></span>

[!code-csharp[Main](app-state/sample/src/WebAppSession/Extensions/SessionExtensions.cs)]

<span data-ttu-id="1d09b-240">O exemplo a seguir mostra como definir e obter um objeto serializável:</span><span class="sxs-lookup"><span data-stu-id="1d09b-240">The following sample shows how to set and get a serializable object:</span></span>

[!code-csharp[Main](app-state/sample/src/WebAppSession/Controllers/HomeController.cs?name=snippet2)]


## <a name="working-with-httpcontextitems"></a><span data-ttu-id="1d09b-241">Trabalhando com HttpContext</span><span class="sxs-lookup"><span data-stu-id="1d09b-241">Working with HttpContext.Items</span></span>

<span data-ttu-id="1d09b-242">O `HttpContext` abstração oferece suporte para uma coleção de dicionário do tipo `IDictionary<object, object>`, chamado `Items`.</span><span class="sxs-lookup"><span data-stu-id="1d09b-242">The `HttpContext` abstraction provides support for a dictionary collection of type `IDictionary<object, object>`, called `Items`.</span></span> <span data-ttu-id="1d09b-243">Essa coleção está disponível desde o início de uma *HttpRequest* e serão descartados no final de cada solicitação.</span><span class="sxs-lookup"><span data-stu-id="1d09b-243">This collection is available from the start of an *HttpRequest* and is discarded at the end of each request.</span></span> <span data-ttu-id="1d09b-244">Você pode acessá-lo, atribuindo um valor a uma entrada de chave, ou solicitando o valor para uma determinada chave.</span><span class="sxs-lookup"><span data-stu-id="1d09b-244">You can access it by  assigning a value to a keyed entry, or by requesting the value for a particular key.</span></span>

<span data-ttu-id="1d09b-245">No exemplo abaixo, [Middleware](middleware.md) adiciona `isVerified` para o `Items` coleção.</span><span class="sxs-lookup"><span data-stu-id="1d09b-245">In the sample below, [Middleware](middleware.md) adds `isVerified` to the `Items` collection.</span></span>

```csharp
app.Use(async (context, next) =>
{
    // perform some verification
    context.Items["isVerified"] = true;
    await next.Invoke();
});
```

<span data-ttu-id="1d09b-246">Mais tarde no pipeline, outro middleware pode acessá-lo:</span><span class="sxs-lookup"><span data-stu-id="1d09b-246">Later in the pipeline, another middleware could access it:</span></span>

```csharp
app.Run(async (context) =>
{
    await context.Response.WriteAsync("Verified request? " + 
        context.Items["isVerified"]);
});
```

<span data-ttu-id="1d09b-247">Para o middleware que será usado apenas por um único aplicativo, `string` chaves são aceitáveis.</span><span class="sxs-lookup"><span data-stu-id="1d09b-247">For middleware that will only be used by a single app, `string` keys are acceptable.</span></span> <span data-ttu-id="1d09b-248">No entanto, o middleware que será compartilhado entre aplicativos deve usar chaves de objeto exclusivo para evitar qualquer possibilidade de colisões de chaves.</span><span class="sxs-lookup"><span data-stu-id="1d09b-248">However, middleware that will be shared between applications should use unique object keys to avoid any chance of key collisions.</span></span> <span data-ttu-id="1d09b-249">Se você estiver desenvolvendo o middleware que deve trabalhar em vários aplicativos, use uma chave de objeto exclusivo definida em sua classe de middleware conforme mostrado abaixo:</span><span class="sxs-lookup"><span data-stu-id="1d09b-249">If you are developing middleware that must work across multiple applications, use a unique object key defined in your middleware class as shown below:</span></span>

```csharp
public class SampleMiddleware
{
    public static readonly object SampleKey = new Object();

    public async Task Invoke(HttpContext httpContext)
    {
        httpContext.Items[SampleKey] = "some value";
        // additional code omitted
    }
}
```

<span data-ttu-id="1d09b-250">Outro código pode acessar o valor armazenado em `HttpContext.Items` usando a chave exposta pela classe middleware:</span><span class="sxs-lookup"><span data-stu-id="1d09b-250">Other code can access the value stored in `HttpContext.Items` using the key exposed by the middleware class:</span></span>

```csharp
public class HomeController : Controller
{
    public IActionResult Index()
    {
        string value = HttpContext.Items[SampleMiddleware.SampleKey];
    }
}
```

<span data-ttu-id="1d09b-251">Essa abordagem também tem a vantagem de eliminar a repetição de "magic cadeias de caracteres" em vários locais no código.</span><span class="sxs-lookup"><span data-stu-id="1d09b-251">This approach also has the advantage of eliminating repetition of "magic strings" in multiple places in the code.</span></span>

<a name="appstate-errors"></a>

## <a name="application-state-data"></a><span data-ttu-id="1d09b-252">Dados de estado do aplicativo</span><span class="sxs-lookup"><span data-stu-id="1d09b-252">Application state data</span></span>

<span data-ttu-id="1d09b-253">Use [injeção de dependência](xref:fundamentals/dependency-injection) para tornar dados disponíveis a todos os usuários:</span><span class="sxs-lookup"><span data-stu-id="1d09b-253">Use [Dependency Injection](xref:fundamentals/dependency-injection) to make data available to all users:</span></span>

1. <span data-ttu-id="1d09b-254">Definir um serviço que contém os dados (por exemplo, uma classe denominada `MyAppData`).</span><span class="sxs-lookup"><span data-stu-id="1d09b-254">Define a service containing the data (for example, a class named `MyAppData`).</span></span>

```csharp
public class MyAppData
{
    // Declare properties/methods/etc.
} 
```
2. <span data-ttu-id="1d09b-255">Adicione a classe de serviço para `ConfigureServices` (por exemplo `services.AddSingleton<MyAppData>();`).</span><span class="sxs-lookup"><span data-stu-id="1d09b-255">Add the service class to `ConfigureServices` (for example `services.AddSingleton<MyAppData>();`).</span></span>
3. <span data-ttu-id="1d09b-256">Consuma a classe de serviço de dados em cada controlador:</span><span class="sxs-lookup"><span data-stu-id="1d09b-256">Consume the data service class in each controller:</span></span>

```csharp
public class MyController : Controller
{
    public MyController(MyAppData myService)
    {
        // Do something with the service (read some data from it, 
        // store it in a private field/property, etc.)
    }
} 
```

## <a name="common-errors-when-working-with-session"></a><span data-ttu-id="1d09b-257">Erros comuns ao trabalhar com a sessão</span><span class="sxs-lookup"><span data-stu-id="1d09b-257">Common errors when working with session</span></span>

* <span data-ttu-id="1d09b-258">"Não é possível resolver o serviço para o tipo 'Microsoft.Extensions.Caching.Distributed.IDistributedCache' ao tentar ativar 'Microsoft.AspNetCore.Session.DistributedSessionStore'."</span><span class="sxs-lookup"><span data-stu-id="1d09b-258">"Unable to resolve service for type 'Microsoft.Extensions.Caching.Distributed.IDistributedCache' while attempting to activate 'Microsoft.AspNetCore.Session.DistributedSessionStore'."</span></span>

  <span data-ttu-id="1d09b-259">Isso geralmente é causado por falha ao configurar pelo menos um `IDistributedCache` implementação.</span><span class="sxs-lookup"><span data-stu-id="1d09b-259">This is usually caused by failing to configure at least one `IDistributedCache` implementation.</span></span> <span data-ttu-id="1d09b-260">Para obter mais informações, consulte [trabalhando com um Cache distribuído](xref:performance/caching/distributed) e [no cache de memória](xref:performance/caching/memory).</span><span class="sxs-lookup"><span data-stu-id="1d09b-260">For more information, see [Working with a Distributed Cache](xref:performance/caching/distributed) and [In memory caching](xref:performance/caching/memory).</span></span>

* <span data-ttu-id="1d09b-261">No caso de que a sessão de middleware não conseguir manter uma sessão (por exemplo: se o banco de dados não estiver disponível), ele registra a exceção e ignora.</span><span class="sxs-lookup"><span data-stu-id="1d09b-261">In the event that the session middleware fails to persist a session (for example: if the database is not available), it logs the exception and swallows it.</span></span> <span data-ttu-id="1d09b-262">A solicitação continuará normalmente, o que leva a um comportamento imprevisível muito.</span><span class="sxs-lookup"><span data-stu-id="1d09b-262">The request will then continue normally, which leads to very unpredictable behavior.</span></span>

<span data-ttu-id="1d09b-263">Um exemplo típico:</span><span class="sxs-lookup"><span data-stu-id="1d09b-263">A typical example:</span></span>

<span data-ttu-id="1d09b-264">Alguém armazena um carrinho de compras na sessão.</span><span class="sxs-lookup"><span data-stu-id="1d09b-264">Someone stores a shopping basket in session.</span></span> <span data-ttu-id="1d09b-265">O usuário adiciona um item, mas a confirmação falhará.</span><span class="sxs-lookup"><span data-stu-id="1d09b-265">The user adds an item but the commit fails.</span></span> <span data-ttu-id="1d09b-266">O aplicativo não sabe sobre a falha para relatar a mensagem "o item foi adicionado", que não é true.</span><span class="sxs-lookup"><span data-stu-id="1d09b-266">The app doesn't know about the failure so it reports the message "The item has been added", which isn't true.</span></span>

<span data-ttu-id="1d09b-267">A maneira recomendada para verificar esses erros é chamar `await feature.Session.CommitAsync();` pelo código do aplicativo quando você terminar gravação à sessão.</span><span class="sxs-lookup"><span data-stu-id="1d09b-267">The recommended way to check for such errors is to call `await feature.Session.CommitAsync();` from app code when you're done writing to the session.</span></span> <span data-ttu-id="1d09b-268">Em seguida, você pode fazer o que você gosta com o erro.</span><span class="sxs-lookup"><span data-stu-id="1d09b-268">Then you can do what you like with the error.</span></span> <span data-ttu-id="1d09b-269">Ele funciona da mesma forma, ao chamar `LoadAsync`.</span><span class="sxs-lookup"><span data-stu-id="1d09b-269">It works the same way when calling `LoadAsync`.</span></span>


### <a name="additional-resources"></a><span data-ttu-id="1d09b-270">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="1d09b-270">Additional Resources</span></span>


* [<span data-ttu-id="1d09b-271">ASP.NET Core 1. x: exemplo de código usado neste documento</span><span class="sxs-lookup"><span data-stu-id="1d09b-271">ASP.NET Core 1.x: Sample code used in this document</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/sample/src/WebAppSession)
* [<span data-ttu-id="1d09b-272">ASP.NET Core 2. x: exemplo de código usado neste documento</span><span class="sxs-lookup"><span data-stu-id="1d09b-272">ASP.NET Core 2.x: Sample code used in this document</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/sample/src/WebAppSessionDotNetCore2.0App)

---
title: Estado da sessão e do aplicativo no ASP.NET Core
author: rick-anderson
description: Abordagens para preservar o estado do aplicativo e do usuário (sessão) entre solicitações.
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 11/27/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/app-state
ms.openlocfilehash: 3a9463e5c501b5f32471f002ecab5ad7a81a5c4a
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/03/2018
---
# <a name="session-and-application-state-in-aspnet-core"></a><span data-ttu-id="f012e-103">Estado da sessão e do aplicativo no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f012e-103">Session and application state in ASP.NET Core</span></span>

<span data-ttu-id="f012e-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/) e [Diana LaRose](https://github.com/DianaLaRose)</span><span class="sxs-lookup"><span data-stu-id="f012e-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), and [Diana LaRose](https://github.com/DianaLaRose)</span></span>

<span data-ttu-id="f012e-105">O HTTP é um protocolo sem estado.</span><span class="sxs-lookup"><span data-stu-id="f012e-105">HTTP is a stateless protocol.</span></span> <span data-ttu-id="f012e-106">Um servidor Web trata cada solicitação HTTP como uma solicitação independente e não mantém valores de usuários de solicitações anteriores.</span><span class="sxs-lookup"><span data-stu-id="f012e-106">A web server treats each HTTP request as an independent request and doesn't retain user values from previous requests.</span></span> <span data-ttu-id="f012e-107">Este artigo aborda diferentes maneiras de preservar o estado de sessão e de aplicativo entre solicitações.</span><span class="sxs-lookup"><span data-stu-id="f012e-107">This article discusses different ways to preserve application and session state between requests.</span></span> 

## <a name="session-state"></a><span data-ttu-id="f012e-108">Estado de sessão</span><span class="sxs-lookup"><span data-stu-id="f012e-108">Session state</span></span>

<span data-ttu-id="f012e-109">O estado de sessão é um recurso do ASP.NET Core que você pode usar para salvar e armazenar dados de usuário enquanto o usuário navega seu aplicativo Web.</span><span class="sxs-lookup"><span data-stu-id="f012e-109">Session state is a feature in ASP.NET Core that you can use to save and store user data while the user browses your web app.</span></span> <span data-ttu-id="f012e-110">Composto por um dicionário ou tabela de hash no servidor, o estado de sessão persiste dados entre solicitações de um navegador.</span><span class="sxs-lookup"><span data-stu-id="f012e-110">Consisting of a dictionary or hash table on the server, session state persists data across requests from a browser.</span></span> <span data-ttu-id="f012e-111">É feito um back up dos dados da sessão em um cache.</span><span class="sxs-lookup"><span data-stu-id="f012e-111">The session data is backed by a cache.</span></span>

<span data-ttu-id="f012e-112">O ASP.NET Core mantém o estado de sessão fornecendo ao cliente um cookie que contém a ID da sessão, que é enviada ao servidor com cada solicitação.</span><span class="sxs-lookup"><span data-stu-id="f012e-112">ASP.NET Core maintains session state by giving the client a cookie that contains the session ID, which is sent to the server with each request.</span></span> <span data-ttu-id="f012e-113">O servidor usa a ID da sessão para buscar os dados da sessão.</span><span class="sxs-lookup"><span data-stu-id="f012e-113">The server uses the session ID to fetch the session data.</span></span> <span data-ttu-id="f012e-114">Como o cookie da sessão é específico ao navegador, não é possível compartilhar sessões entre navegadores.</span><span class="sxs-lookup"><span data-stu-id="f012e-114">Because the session cookie is specific to the browser, you cannot share sessions across browsers.</span></span> <span data-ttu-id="f012e-115">Cookies da sessão são excluídos somente quando a sessão do navegador termina.</span><span class="sxs-lookup"><span data-stu-id="f012e-115">Session cookies are deleted only when the browser session ends.</span></span> <span data-ttu-id="f012e-116">Se for recebido um cookie de uma sessão expirada, uma nova sessão que usa o mesmo cookie será criada.</span><span class="sxs-lookup"><span data-stu-id="f012e-116">If a cookie is received for an expired session, a new session that uses the same session cookie  is created.</span></span> 

<span data-ttu-id="f012e-117">O servidor mantém uma sessão por um tempo limitado após a última solicitação.</span><span class="sxs-lookup"><span data-stu-id="f012e-117">The server retains a session for a limited time after the last request.</span></span> <span data-ttu-id="f012e-118">Defina o tempo limite da sessão ou use o valor padrão de 20 minutos.</span><span class="sxs-lookup"><span data-stu-id="f012e-118">Either set the session timeout or use the default value of 20 minutes.</span></span> <span data-ttu-id="f012e-119">O estado de sessão é ideal para armazenar dados do usuário que são específicos a uma sessão específica, mas não precisam ser persistidos permanentemente.</span><span class="sxs-lookup"><span data-stu-id="f012e-119">Session state is ideal for storing user data that's specific to a particular session but doesn't need to be persisted permanently.</span></span> <span data-ttu-id="f012e-120">Dados são excluídos do repositório de backup ao chamar `Session.Clear` ou quando a sessão expira no armazenamento de dados.</span><span class="sxs-lookup"><span data-stu-id="f012e-120">Data is deleted from the backing store either when calling `Session.Clear` or when the session expires in the data store.</span></span> <span data-ttu-id="f012e-121">O servidor não sabe quando o navegador é fechado ou quando o cookie da sessão é excluído.</span><span class="sxs-lookup"><span data-stu-id="f012e-121">The server doesn't know when the browser is closed or when the session cookie is deleted.</span></span>

> [!WARNING]
> <span data-ttu-id="f012e-122">Não armazene dados confidenciais na sessão.</span><span class="sxs-lookup"><span data-stu-id="f012e-122">Don't store sensitive data in session.</span></span> <span data-ttu-id="f012e-123">O cliente pode não fechar o navegador e limpar o cookie da sessão (e alguns navegadores mantêm cookies de sessão ativos entre janelas diferentes).</span><span class="sxs-lookup"><span data-stu-id="f012e-123">The client might not close the browser and clear the session cookie (and some browsers keep session cookies alive across windows).</span></span> <span data-ttu-id="f012e-124">Além disso, uma sessão pode não ficar restrita a um único usuário; o usuário seguinte pode continuar com a mesma sessão.</span><span class="sxs-lookup"><span data-stu-id="f012e-124">Also, a session might not be restricted to a single user; the next user might continue with the same session.</span></span>

<span data-ttu-id="f012e-125">O provedor da sessão na memória armazena dados da sessão no servidor local.</span><span class="sxs-lookup"><span data-stu-id="f012e-125">The in-memory session provider stores session data on the local server.</span></span> <span data-ttu-id="f012e-126">Caso planeje executar seu aplicativo Web em um farm de servidores, você precisará usar sessões autoadesivas para vincular cada sessão a um servidor específico.</span><span class="sxs-lookup"><span data-stu-id="f012e-126">If you plan to run your web app on a server farm, you must use sticky sessions to tie each session to a specific server.</span></span> <span data-ttu-id="f012e-127">A plataforma Sites do Microsoft Azure usa sessões autoadesivas como padrão (Application Request Routing ou ARR).</span><span class="sxs-lookup"><span data-stu-id="f012e-127">The Windows Azure Web Sites platform defaults to sticky sessions (Application Request Routing or ARR).</span></span> <span data-ttu-id="f012e-128">Entretanto, sessões autoadesivas podem afetar a escalabilidade e complicar atualizações de aplicativos Web.</span><span class="sxs-lookup"><span data-stu-id="f012e-128">However, sticky sessions can affect scalability and complicate web app updates.</span></span> <span data-ttu-id="f012e-129">Uma opção melhor é usar os caches distribuídos do Redis ou do SQL Server, que não requerem sessões autoadesivas.</span><span class="sxs-lookup"><span data-stu-id="f012e-129">A better option is to use the Redis or SQL Server distributed caches, which don't require sticky sessions.</span></span> <span data-ttu-id="f012e-130">Para obter mais informações, confira [Trabalhar com um cache distribuído](xref:performance/caching/distributed).</span><span class="sxs-lookup"><span data-stu-id="f012e-130">For more information, see [Work with a Distributed Cache](xref:performance/caching/distributed).</span></span> <span data-ttu-id="f012e-131">Para obter detalhes sobre como configurar provedores de serviço, consulte [Configurando a sessão](#configuring-session) posteriormente neste artigo.</span><span class="sxs-lookup"><span data-stu-id="f012e-131">For details on setting up service providers, see [Configuring Session](#configuring-session) later in this article.</span></span>

<a name="temp"></a>
## <a name="tempdata"></a><span data-ttu-id="f012e-132">TempData</span><span class="sxs-lookup"><span data-stu-id="f012e-132">TempData</span></span>

<span data-ttu-id="f012e-133">O ASP.NET Core MVC expõe a propriedade [TempData](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) em um [controlador](/dotnet/api/microsoft.aspnetcore.mvc.controller?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="f012e-133">ASP.NET Core MVC exposes the [TempData](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) property on a [controller](/dotnet/api/microsoft.aspnetcore.mvc.controller?view=aspnetcore-2.0).</span></span> <span data-ttu-id="f012e-134">Essa propriedade armazena dados até eles serem lidos.</span><span class="sxs-lookup"><span data-stu-id="f012e-134">This property stores data until it's read.</span></span> <span data-ttu-id="f012e-135">Os métodos `Keep` e `Peek` podem ser usados para examinar os dados sem exclusão.</span><span class="sxs-lookup"><span data-stu-id="f012e-135">The `Keep` and `Peek` methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="f012e-136">`TempData` é particularmente útil para redirecionamento em casos em que os dados são necessários para mais de uma única solicitação.</span><span class="sxs-lookup"><span data-stu-id="f012e-136">`TempData` is particularly useful for redirection, when data is needed for more than a single request.</span></span> <span data-ttu-id="f012e-137">`TempData` é implementado por provedores de TempData, por exemplo, usando cookies ou estado de sessão.</span><span class="sxs-lookup"><span data-stu-id="f012e-137">`TempData` is implemented by TempData providers, for example, using either cookies or session state.</span></span>

<a name="tempdata-providers"></a>
### <a name="tempdata-providers"></a><span data-ttu-id="f012e-138">Provedores de TempData</span><span class="sxs-lookup"><span data-stu-id="f012e-138">TempData providers</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f012e-139">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f012e-139">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="f012e-140">No ASP.NET Core 2.0 e posteriores, o provedor de TempData baseado em cookies é usado por padrão para armazenar TempData em cookies.</span><span class="sxs-lookup"><span data-stu-id="f012e-140">In ASP.NET Core 2.0 and later, the cookie-based TempData provider is used by default to store TempData in cookies.</span></span>

<span data-ttu-id="f012e-141">Os dados do cookie são codificados com o [Base64UrlTextEncoder](/dotnet/api/microsoft.aspnetcore.webutilities.base64urltextencoder?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="f012e-141">The cookie data is encoded with the [Base64UrlTextEncoder](/dotnet/api/microsoft.aspnetcore.webutilities.base64urltextencoder?view=aspnetcore-2.0).</span></span> <span data-ttu-id="f012e-142">Como o cookie é criptografado e dividido em partes, o limite de tamanho de cookie único encontrado no ASP.NET Core 1. x não se aplica.</span><span class="sxs-lookup"><span data-stu-id="f012e-142">Because the cookie is encrypted and chunked, the single cookie size limit found in ASP.NET Core 1.x does not apply.</span></span> <span data-ttu-id="f012e-143">Os dados do cookie não são compactados porque a compactação de dados criptografados pode levar a problemas de segurança, como os ataques [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) e [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)).</span><span class="sxs-lookup"><span data-stu-id="f012e-143">The cookie data is not compressed because compressing encrypted data can lead to security problems such as the [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) and [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)) attacks.</span></span> <span data-ttu-id="f012e-144">Para obter mais informações sobre o provedor de TempData baseado em cookie, consulte [CookieTempDataProvider](https://github.com/aspnet/Mvc/blob/dev/src/Microsoft.AspNetCore.Mvc.ViewFeatures/ViewFeatures/CookieTempDataProvider.cs).</span><span class="sxs-lookup"><span data-stu-id="f012e-144">For more information on the cookie-based TempData provider, see [CookieTempDataProvider](https://github.com/aspnet/Mvc/blob/dev/src/Microsoft.AspNetCore.Mvc.ViewFeatures/ViewFeatures/CookieTempDataProvider.cs).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f012e-145">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f012e-145">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="f012e-146">No ASP.NET Core 1.0 e 1.1, o provedor de TempData do estado de sessão é o padrão.</span><span class="sxs-lookup"><span data-stu-id="f012e-146">In ASP.NET Core 1.0 and 1.1, the session state TempData provider is the default.</span></span>

--------------

<a name="choose-temp"></a>
### <a name="choosing-a-tempdata-provider"></a><span data-ttu-id="f012e-147">Escolhendo um provedor de TempData</span><span class="sxs-lookup"><span data-stu-id="f012e-147">Choosing a TempData provider</span></span>

<span data-ttu-id="f012e-148">Escolher um provedor de TempData envolve várias considerações, como:</span><span class="sxs-lookup"><span data-stu-id="f012e-148">Choosing a TempData provider involves several considerations, such as:</span></span>

1. <span data-ttu-id="f012e-149">O aplicativo já usa o estado de sessão para outras finalidades?</span><span class="sxs-lookup"><span data-stu-id="f012e-149">Does the application already use session state for other purposes?</span></span> <span data-ttu-id="f012e-150">Em caso afirmativo, usar o provedor de TempData do estado de sessão não tem custos adicionais para o aplicativo (além do tamanho dos dados).</span><span class="sxs-lookup"><span data-stu-id="f012e-150">If so, using the session state TempData provider has no additional cost to the application (aside from the size of the data).</span></span>
2. <span data-ttu-id="f012e-151">O aplicativo usa TempData raramente, para quantidades relativamente pequenas de dados (até 500 bytes)?</span><span class="sxs-lookup"><span data-stu-id="f012e-151">Does the application use TempData only sparingly, for relatively small amounts of data (up to 500 bytes)?</span></span> <span data-ttu-id="f012e-152">Em caso afirmativo, o provedor de TempData do cookie acrescentará um pequeno custo a cada solicitação que transportar TempData.</span><span class="sxs-lookup"><span data-stu-id="f012e-152">If so, the cookie TempData provider will add a small cost to each request that carries TempData.</span></span> <span data-ttu-id="f012e-153">Caso contrário, o provedor de TempData do estado de sessão pode ser útil para evitar fazer viagens de ida e volta para uma grande quantidade de dados a cada solicitação até que TempData seja consumido.</span><span class="sxs-lookup"><span data-stu-id="f012e-153">If not, the session state TempData provider can be beneficial to avoid round-tripping a large amount of data in each request until the TempData is consumed.</span></span>
3. <span data-ttu-id="f012e-154">O aplicativo é executado em um web farm (vários servidores)?</span><span class="sxs-lookup"><span data-stu-id="f012e-154">Does the application run in a web farm (multiple servers)?</span></span> <span data-ttu-id="f012e-155">Nesse caso, nenhuma configuração adicional é necessária para usar o provedor de TempData do cookie.</span><span class="sxs-lookup"><span data-stu-id="f012e-155">If so, there's no additional configuration needed to use the cookie TempData provider.</span></span>

> [!NOTE]
> <span data-ttu-id="f012e-156">A maioria dos clientes da Web (como navegadores da Web) impõem limites quanto ao tamanho máximo de cada cookie, o número total de cookies ou ambos.</span><span class="sxs-lookup"><span data-stu-id="f012e-156">Most web clients (such as web browsers) enforce limits on the maximum size of each cookie, the total number of cookies, or both.</span></span> <span data-ttu-id="f012e-157">Portanto, ao usar o provedor de TempData do cookie, verifique se o aplicativo não ultrapassará esses limites.</span><span class="sxs-lookup"><span data-stu-id="f012e-157">Therefore, when using the cookie TempData provider, verify the app won't exceed these limits.</span></span> <span data-ttu-id="f012e-158">Considere o tamanho total dos dados, levando em consideração as sobrecargas de criptografia e divisão em partes.</span><span class="sxs-lookup"><span data-stu-id="f012e-158">Consider the total size of the data, accounting for the overheads of encryption and chunking.</span></span>

<a name="config-temp"></a>
### <a name="configure-the-tempdata-provider"></a><span data-ttu-id="f012e-159">Configurar o provedor de TempData</span><span class="sxs-lookup"><span data-stu-id="f012e-159">Configure the TempData provider</span></span>

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f012e-160">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f012e-160">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)
<span data-ttu-id="f012e-161">O provedor de TempData baseado em cookie é habilitado por padrão.</span><span class="sxs-lookup"><span data-stu-id="f012e-161">The cookie-based TempData provider is enabled by default.</span></span> <span data-ttu-id="f012e-162">O código da classe `Startup` a seguir configura o provedor de TempData baseado em sessão:</span><span class="sxs-lookup"><span data-stu-id="f012e-162">The following `Startup` class code configures the session-based TempData provider:</span></span>

[!code-csharp[](app-state/sample/src/WebAppSessionDotNetCore2.0App/StartupTempDataSession.cs?name=snippet_TempDataSession&highlight=4,6,11)]

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f012e-163">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f012e-163">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)
<span data-ttu-id="f012e-164">O código da classe `Startup` a seguir configura o provedor de TempData baseado em sessão:</span><span class="sxs-lookup"><span data-stu-id="f012e-164">The following `Startup` class code configures the session-based TempData provider:</span></span>

[!code-csharp[](app-state/sample/src/WebAppSession/StartupTempDataSession.cs?name=snippet_TempDataSession&highlight=4,9)]

* * *
<span data-ttu-id="f012e-165">A ordenação é crítica para componentes de middleware.</span><span class="sxs-lookup"><span data-stu-id="f012e-165">Ordering is critical for middleware components.</span></span> <span data-ttu-id="f012e-166">No exemplo anterior, uma exceção do tipo `InvalidOperationException` ocorre quando `UseSession` é invocado após `UseMvcWithDefaultRoute`.</span><span class="sxs-lookup"><span data-stu-id="f012e-166">In the preceding example, an exception of type `InvalidOperationException` occurs when `UseSession` is invoked after `UseMvcWithDefaultRoute`.</span></span> <span data-ttu-id="f012e-167">Consulte [Ordenação de Middleware](xref:fundamentals/middleware/index#ordering) para obter mais detalhes.</span><span class="sxs-lookup"><span data-stu-id="f012e-167">See [Middleware Ordering](xref:fundamentals/middleware/index#ordering) for more detail.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f012e-168">Se o alvo for o .NET Framework e o provedor baseado em sessão for usado, adicione o pacote NuGet [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session) ao seu projeto.</span><span class="sxs-lookup"><span data-stu-id="f012e-168">If targeting .NET Framework and using the session-based provider, add the [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session) NuGet package to your project.</span></span>

## <a name="query-strings"></a><span data-ttu-id="f012e-169">Cadeias de consulta</span><span class="sxs-lookup"><span data-stu-id="f012e-169">Query strings</span></span>

<span data-ttu-id="f012e-170">Você pode passar uma quantidade limitada de dados de uma solicitação para outra adicionando-os à cadeia de caracteres de consulta da nova solicitação.</span><span class="sxs-lookup"><span data-stu-id="f012e-170">You can pass a limited amount of data from one request to another by adding it to the new request's query string.</span></span> <span data-ttu-id="f012e-171">Isso é útil para capturar o estado de uma maneira persistente que permita que links com estado inserido sejam compartilhados por email ou por redes sociais.</span><span class="sxs-lookup"><span data-stu-id="f012e-171">This is useful for capturing state in a persistent manner that allows links with embedded state to be shared through email or social networks.</span></span> <span data-ttu-id="f012e-172">No entanto, por esse motivo, você nunca deve usar cadeias de consulta para dados confidenciais.</span><span class="sxs-lookup"><span data-stu-id="f012e-172">However, for this reason, you should never use query strings for sensitive data.</span></span> <span data-ttu-id="f012e-173">Além de serem compartilhadas facilmente, incluir dados em cadeias de consulta pode criar oportunidades para ataques de [CSRF (solicitação intersite forjada)](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)), que podem enganar os usuários para que eles visitem sites mal-intencionados enquanto estão autenticados.</span><span class="sxs-lookup"><span data-stu-id="f012e-173">In addition to being easily shared, including data in query strings can create opportunities for [Cross-Site Request Forgery (CSRF)](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) attacks, which can trick users into visiting malicious sites while authenticated.</span></span> <span data-ttu-id="f012e-174">Invasores podem, então, roubar dados do usuário de seu aplicativo ou executar ações mal-intencionadas em nome do usuário.</span><span class="sxs-lookup"><span data-stu-id="f012e-174">Attackers can then steal user data from your app or take malicious actions on behalf of the user.</span></span> <span data-ttu-id="f012e-175">Qualquer estado de sessão ou aplicativo preservado deve proteger contra ataques CSRF.</span><span class="sxs-lookup"><span data-stu-id="f012e-175">Any preserved application or session state must protect against CSRF attacks.</span></span> <span data-ttu-id="f012e-176">Para obter mais informações sobre ataques de CSRF, confira [Impedir ataques de XSRF/CSRF (solicitação intersite forjada)](xref:security/anti-request-forgery).</span><span class="sxs-lookup"><span data-stu-id="f012e-176">For more information on CSRF attacks, see [Prevent Cross-Site Request Forgery (XSRF/CSRF) attacks](xref:security/anti-request-forgery).</span></span>

## <a name="post-data-and-hidden-fields"></a><span data-ttu-id="f012e-177">Dados de postagem e campos ocultos</span><span class="sxs-lookup"><span data-stu-id="f012e-177">Post data and hidden fields</span></span>

<span data-ttu-id="f012e-178">Dados podem ser salvos em campos de formulário ocultos e postados novamente na solicitação seguinte.</span><span class="sxs-lookup"><span data-stu-id="f012e-178">Data can be saved in hidden form fields and posted back on the next request.</span></span> <span data-ttu-id="f012e-179">Isso é comum em formulários com várias páginas.</span><span class="sxs-lookup"><span data-stu-id="f012e-179">This is common in multi-page forms.</span></span> <span data-ttu-id="f012e-180">No entanto, como o cliente pode adulterar os dados, o servidor sempre deve validá-lo novamente.</span><span class="sxs-lookup"><span data-stu-id="f012e-180">However, because the client can potentially tamper with the data, the server must always revalidate it.</span></span> 

## <a name="cookies"></a><span data-ttu-id="f012e-181">Cookies</span><span class="sxs-lookup"><span data-stu-id="f012e-181">Cookies</span></span>

<span data-ttu-id="f012e-182">Cookies fornecem uma maneira de armazenar dados específicos do usuário em aplicativos Web.</span><span class="sxs-lookup"><span data-stu-id="f012e-182">Cookies provide a way to store user-specific data in web applications.</span></span> <span data-ttu-id="f012e-183">Como os cookies são enviados com cada solicitação, seu tamanho deve ser reduzido ao mínimo.</span><span class="sxs-lookup"><span data-stu-id="f012e-183">Because cookies are sent with every request, their size should be kept to a minimum.</span></span> <span data-ttu-id="f012e-184">Idealmente, somente um identificador deve ser armazenado em um cookie, com os dados reais armazenados no servidor.</span><span class="sxs-lookup"><span data-stu-id="f012e-184">Ideally, only an identifier should be stored in a cookie with the actual data stored on the server.</span></span> <span data-ttu-id="f012e-185">A maioria dos navegadores restringe os cookies a 4096 bytes.</span><span class="sxs-lookup"><span data-stu-id="f012e-185">Most browsers restrict cookies to 4096 bytes.</span></span> <span data-ttu-id="f012e-186">Além disso, somente um número limitado de cookies está disponível para cada domínio.</span><span class="sxs-lookup"><span data-stu-id="f012e-186">In addition, only a limited number of cookies are available for each domain.</span></span>  

<span data-ttu-id="f012e-187">Como cookies estão sujeitos à adulteração, eles devem ser validados no servidor.</span><span class="sxs-lookup"><span data-stu-id="f012e-187">Because cookies are subject to tampering, they must be validated on the server.</span></span> <span data-ttu-id="f012e-188">Embora a durabilidade do cookie em um cliente esteja sujeita à intervenção do usuário e à expiração, normalmente eles são a forma mais durável de persistência de dados no cliente.</span><span class="sxs-lookup"><span data-stu-id="f012e-188">Although the durability of the cookie on a client is subject to user intervention and expiration, they're generally the most durable form of data persistence on the client.</span></span>

<span data-ttu-id="f012e-189">Frequentemente, cookies são usados para personalização quando o conteúdo é personalizado para um usuário conhecido.</span><span class="sxs-lookup"><span data-stu-id="f012e-189">Cookies are often used for personalization, where content is customized for a known user.</span></span> <span data-ttu-id="f012e-190">Como na maioria dos casos o usuário é apenas identificado e não autenticado, normalmente você pode proteger um cookie armazenando o nome de usuário, o nome da conta ou uma ID de usuário único (como um GUID) no cookie.</span><span class="sxs-lookup"><span data-stu-id="f012e-190">Because the user is only identified and not authenticated in most cases, you can typically secure a cookie by storing the user name, account name, or a unique user ID (such as a GUID) in the cookie.</span></span> <span data-ttu-id="f012e-191">É possível, então, usar o cookie para acessar a infraestrutura de personalização de usuários de um site.</span><span class="sxs-lookup"><span data-stu-id="f012e-191">You can then use the cookie to access the user personalization infrastructure of a site.</span></span>

## <a name="httpcontextitems"></a><span data-ttu-id="f012e-192">HttpContext.Items</span><span class="sxs-lookup"><span data-stu-id="f012e-192">HttpContext.Items</span></span>

<span data-ttu-id="f012e-193">A coleção `Items` é um bom local para armazenar dados que são necessários somente ao processar uma solicitação específica.</span><span class="sxs-lookup"><span data-stu-id="f012e-193">The `Items` collection is a good location to store data that's needed only while processing one particular request.</span></span> <span data-ttu-id="f012e-194">O conteúdo da coleção é descartado após cada solicitação.</span><span class="sxs-lookup"><span data-stu-id="f012e-194">The collection's contents are discarded after each request.</span></span> <span data-ttu-id="f012e-195">A coleção `Items` é melhor usada como uma maneira de componentes ou middleware se comunicarem quando operam em momentos diferentes durante uma solicitação e não têm nenhuma maneira direta de passar parâmetros.</span><span class="sxs-lookup"><span data-stu-id="f012e-195">The `Items` collection is best used as a way for components or middleware to communicate when they operate at different points in time during a request and have no direct way to pass parameters.</span></span> <span data-ttu-id="f012e-196">Para obter mais informações, confira [Trabalhar com HttpContext.Items](#working-with-httpcontextitems), mais adiante neste artigo.</span><span class="sxs-lookup"><span data-stu-id="f012e-196">For more information, see [Work with HttpContext.Items](#working-with-httpcontextitems), later in this article.</span></span>

## <a name="cache"></a><span data-ttu-id="f012e-197">Cache</span><span class="sxs-lookup"><span data-stu-id="f012e-197">Cache</span></span>

<span data-ttu-id="f012e-198">O cache é uma maneira eficiente de armazenar e recuperar dados.</span><span class="sxs-lookup"><span data-stu-id="f012e-198">Caching is an efficient way to store and retrieve data.</span></span> <span data-ttu-id="f012e-199">É possível controlar o tempo de vida dos itens em cache com base na hora e em outras considerações.</span><span class="sxs-lookup"><span data-stu-id="f012e-199">You can control the lifetime of cached items based on time and other considerations.</span></span> <span data-ttu-id="f012e-200">Saiba mais sobre [como armazenar em cache](../performance/caching/index.md).</span><span class="sxs-lookup"><span data-stu-id="f012e-200">Learn more about [how to cache](../performance/caching/index.md).</span></span>

<a name="session"></a>
## <a name="working-with-session-state"></a><span data-ttu-id="f012e-201">Trabalhando com o estado de sessão</span><span class="sxs-lookup"><span data-stu-id="f012e-201">Working with Session State</span></span>

### <a name="configuring-session"></a><span data-ttu-id="f012e-202">Configurando a sessão</span><span class="sxs-lookup"><span data-stu-id="f012e-202">Configuring Session</span></span>

<span data-ttu-id="f012e-203">O pacote `Microsoft.AspNetCore.Session` fornece middleware para gerenciar o estado de sessão.</span><span class="sxs-lookup"><span data-stu-id="f012e-203">The `Microsoft.AspNetCore.Session` package provides middleware for managing session state.</span></span> <span data-ttu-id="f012e-204">Para habilitar o middleware da sessão, `Startup` deve conter:</span><span class="sxs-lookup"><span data-stu-id="f012e-204">To enable the session middleware, `Startup` must contain:</span></span>

- <span data-ttu-id="f012e-205">Qualquer um dos caches de memória [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache).</span><span class="sxs-lookup"><span data-stu-id="f012e-205">Any of the [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache) memory caches.</span></span> <span data-ttu-id="f012e-206">A implementação `IDistributedCache` é usada como um repositório de backup para a sessão.</span><span class="sxs-lookup"><span data-stu-id="f012e-206">The `IDistributedCache` implementation is used as a backing store for session.</span></span>
- <span data-ttu-id="f012e-207">Chamada [AddSession](/dotnet/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions#Microsoft_Extensions_DependencyInjection_SessionServiceCollectionExtensions_AddSession_Microsoft_Extensions_DependencyInjection_IServiceCollection_), que requer o pacote NuGet "Microsoft.AspNetCore.Session".</span><span class="sxs-lookup"><span data-stu-id="f012e-207">[AddSession](/dotnet/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions#Microsoft_Extensions_DependencyInjection_SessionServiceCollectionExtensions_AddSession_Microsoft_Extensions_DependencyInjection_IServiceCollection_) call, which requires NuGet package "Microsoft.AspNetCore.Session".</span></span>
- <span data-ttu-id="f012e-208">Chamada [UseSession](/dotnet/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions#methods_).</span><span class="sxs-lookup"><span data-stu-id="f012e-208">[UseSession](/dotnet/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions#methods_) call.</span></span>

<span data-ttu-id="f012e-209">O código a seguir mostra como configurar o provedor de sessão na memória.</span><span class="sxs-lookup"><span data-stu-id="f012e-209">The following code shows how to set up the in-memory session provider.</span></span>

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f012e-210">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f012e-210">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)
[!code-csharp[](app-state/sample/src/WebAppSessionDotNetCore2.0App/Startup.cs?highlight=11-19,24)]

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f012e-211">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f012e-211">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)
[!code-csharp[](app-state/sample/src/WebAppSession/Startup.cs?highlight=11-19,24)]

* * *
<span data-ttu-id="f012e-212">É possível fazer referência à sessão de `HttpContext` após ele ser instalado e configurado.</span><span class="sxs-lookup"><span data-stu-id="f012e-212">You can reference Session from `HttpContext` once it's installed and configured.</span></span>

<span data-ttu-id="f012e-213">Se você tentar acessar `Session` antes que `UseSession` tenha sido chamado, a exceção `InvalidOperationException: Session has not been configured for this application or request` será lançada.</span><span class="sxs-lookup"><span data-stu-id="f012e-213">If you try to access `Session` before `UseSession` has been called, the exception `InvalidOperationException: Session has not been configured for this application or request` is thrown.</span></span>

<span data-ttu-id="f012e-214">Se você tentar criar um novo `Session` (ou seja, nenhum cookie de sessão foi criado) após já ter começado a gravar no fluxo `Response`, a exceção `InvalidOperationException: The session cannot be established after the response has started` será lançada.</span><span class="sxs-lookup"><span data-stu-id="f012e-214">If you try to create a new `Session` (that is, no session cookie has been created) after you have already begun writing to the `Response` stream, the exception `InvalidOperationException: The session cannot be established after the response has started` is thrown.</span></span> <span data-ttu-id="f012e-215">A exceção pode ser encontrada no log do servidor Web; ele não será exibido no navegador.</span><span class="sxs-lookup"><span data-stu-id="f012e-215">The exception can be found in the web server log; it will not be displayed in the browser.</span></span>

### <a name="loading-session-asynchronously"></a><span data-ttu-id="f012e-216">Carregamento a sessão de forma assíncrona</span><span class="sxs-lookup"><span data-stu-id="f012e-216">Loading Session asynchronously</span></span> 

<span data-ttu-id="f012e-217">O provedor de sessão padrão no ASP.NET Core carrega o registro da sessão do repositório [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache) subjacente de forma assíncrona somente se o método [ISession.LoadAsync](/dotnet/api/microsoft.aspnetcore.http.isession#Microsoft_AspNetCore_Http_ISession_LoadAsync) for chamado explicitamente antes dos métodos `TryGetValue`, `Set` ou `Remove`.</span><span class="sxs-lookup"><span data-stu-id="f012e-217">The default session provider in ASP.NET Core loads the session record from the underlying [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache) store asynchronously only if the [ISession.LoadAsync](/dotnet/api/microsoft.aspnetcore.http.isession#Microsoft_AspNetCore_Http_ISession_LoadAsync) method is explicitly called before  the `TryGetValue`, `Set`, or `Remove` methods.</span></span> <span data-ttu-id="f012e-218">Se `LoadAsync` não for chamado primeiro, o registro da sessão subjacente é carregado de forma síncrona, o que poderia afetar a capacidade de dimensionamento do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="f012e-218">If `LoadAsync` isn't called first, the underlying session record is loaded synchronously, which could potentially impact the ability of the app to scale.</span></span>

<span data-ttu-id="f012e-219">Para que aplicativos imponham esse padrão, encapsule as implementações [DistributedSessionStore](/dotnet/api/microsoft.aspnetcore.session.distributedsessionstore) e [DistributedSession](/dotnet/api/microsoft.aspnetcore.session.distributedsession) com versões que geram uma exceção se o método `LoadAsync` não for chamado antes de `TryGetValue`, `Set` ou `Remove`.</span><span class="sxs-lookup"><span data-stu-id="f012e-219">To have applications enforce this pattern, wrap the [DistributedSessionStore](/dotnet/api/microsoft.aspnetcore.session.distributedsessionstore) and [DistributedSession](/dotnet/api/microsoft.aspnetcore.session.distributedsession) implementations with versions that throw an exception if the `LoadAsync` method isn't called before `TryGetValue`, `Set`, or `Remove`.</span></span> <span data-ttu-id="f012e-220">Registre as versões encapsuladas no contêiner de serviços.</span><span class="sxs-lookup"><span data-stu-id="f012e-220">Register the wrapped versions in the services container.</span></span>

### <a name="implementation-details"></a><span data-ttu-id="f012e-221">Detalhes da implementação</span><span class="sxs-lookup"><span data-stu-id="f012e-221">Implementation details</span></span>

<span data-ttu-id="f012e-222">A sessão usa um cookie para rastrear e identificar solicitações de um único navegador.</span><span class="sxs-lookup"><span data-stu-id="f012e-222">Session uses a cookie to track and identify requests from a single browser.</span></span> <span data-ttu-id="f012e-223">Por padrão, esse cookie é denominado ". AspNet.Session" e usa um caminho de "/".</span><span class="sxs-lookup"><span data-stu-id="f012e-223">By default, this cookie is named ".AspNet.Session", and it uses a path of "/".</span></span> <span data-ttu-id="f012e-224">Como o padrão do cookie não especifica um domínio, ele não fica disponível para o script do lado do cliente na página (porque `CookieHttpOnly` tem `true` como padrão).</span><span class="sxs-lookup"><span data-stu-id="f012e-224">Because the cookie default doesn't specify a domain, it's not made available to the client-side script on the page (because `CookieHttpOnly` defaults to `true`).</span></span>

<span data-ttu-id="f012e-225">Para substituir os padrões da sessão, use `SessionOptions`:</span><span class="sxs-lookup"><span data-stu-id="f012e-225">To override session defaults, use `SessionOptions`:</span></span>

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f012e-226">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f012e-226">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)
[!code-csharp[](app-state/sample/src/WebAppSessionDotNetCore2.0App/StartupCopy.cs?name=snippet1&highlight=8-12)]

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f012e-227">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f012e-227">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)
[!code-csharp[](app-state/sample/src/WebAppSession/StartupCopy.cs?name=snippet1&highlight=8-12)]

* * *
<span data-ttu-id="f012e-228">O servidor usa a propriedade `IdleTimeout` para determinar por quanto tempo uma sessão pode ficar ociosa antes que seu conteúdo seja abandonado.</span><span class="sxs-lookup"><span data-stu-id="f012e-228">The server uses the `IdleTimeout` property to determine how long a session can be idle before its contents are abandoned.</span></span> <span data-ttu-id="f012e-229">Essa propriedade é independente da expiração do cookie.</span><span class="sxs-lookup"><span data-stu-id="f012e-229">This property is independent of the cookie expiration.</span></span> <span data-ttu-id="f012e-230">Cada solicitação passada por meio do middleware de Sessão (lida ou gravada) redefine o tempo limite.</span><span class="sxs-lookup"><span data-stu-id="f012e-230">Each request that passes through the Session middleware (read from or written to) resets the timeout.</span></span>

<span data-ttu-id="f012e-231">Como `Session` é *sem bloqueio*, se duas solicitações tentarem modificar o conteúdo da sessão, a última delas substituirá a primeira.</span><span class="sxs-lookup"><span data-stu-id="f012e-231">Because `Session` is *non-locking*, if two requests both attempt to modify the contents of session, the last one overrides the first.</span></span> <span data-ttu-id="f012e-232">`Session` é implementado como uma *sessão coerente*, o que significa que todo o conteúdo é armazenado junto.</span><span class="sxs-lookup"><span data-stu-id="f012e-232">`Session` is implemented as a *coherent session*, which means that all the contents are stored together.</span></span> <span data-ttu-id="f012e-233">Duas solicitações que estão modificando partes diferentes da sessão (chaves diferentes) ainda podem afetar umas às outras.</span><span class="sxs-lookup"><span data-stu-id="f012e-233">Two requests that are modifying different parts of the session (different keys) might still impact each other.</span></span>

### <a name="setting-and-getting-session-values"></a><span data-ttu-id="f012e-234">Configurando e obtendo valores de sessão</span><span class="sxs-lookup"><span data-stu-id="f012e-234">Setting and getting Session values</span></span>

<span data-ttu-id="f012e-235">A sessão é acessada por meio da propriedade `Session` em `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="f012e-235">Session is accessed through the `Session` property on `HttpContext`.</span></span> <span data-ttu-id="f012e-236">Esta propriedade é uma implementação de [ISession](/dotnet/api/microsoft.aspnetcore.http.isession).</span><span class="sxs-lookup"><span data-stu-id="f012e-236">This property is an [ISession](/dotnet/api/microsoft.aspnetcore.http.isession) implementation.</span></span>

<span data-ttu-id="f012e-237">O exemplo a seguir mostra a configuração e a obtenção de um int e uma cadeia de caracteres:</span><span class="sxs-lookup"><span data-stu-id="f012e-237">The following example shows setting and getting an int and a string:</span></span>

[!code-csharp[](app-state/sample/src/WebAppSession/Controllers/HomeController.cs?range=8-27,49)]

<span data-ttu-id="f012e-238">Se adicionar os seguintes métodos de extensão, você poderá definir e obter objetos serializáveis da sessão:</span><span class="sxs-lookup"><span data-stu-id="f012e-238">If you add the following extension methods, you can set and get serializable objects to Session:</span></span>

[!code-csharp[](app-state/sample/src/WebAppSession/Extensions/SessionExtensions.cs)]

<span data-ttu-id="f012e-239">O exemplo a seguir mostra como definir e obter um objeto serializável:</span><span class="sxs-lookup"><span data-stu-id="f012e-239">The following sample shows how to set and get a serializable object:</span></span>

[!code-csharp[](app-state/sample/src/WebAppSession/Controllers/HomeController.cs?name=snippet2)]


## <a name="working-with-httpcontextitems"></a><span data-ttu-id="f012e-240">Trabalhando com HttpContext.Items</span><span class="sxs-lookup"><span data-stu-id="f012e-240">Working with HttpContext.Items</span></span>

<span data-ttu-id="f012e-241">A abstração `HttpContext` dá suporte a uma coleção de dicionário do tipo `IDictionary<object, object>`, chamada `Items`.</span><span class="sxs-lookup"><span data-stu-id="f012e-241">The `HttpContext` abstraction provides support for a dictionary collection of type `IDictionary<object, object>`, called `Items`.</span></span> <span data-ttu-id="f012e-242">Essa coleção está disponível desde o início de um *HttpRequest* e é descartada no final de cada solicitação.</span><span class="sxs-lookup"><span data-stu-id="f012e-242">This collection is available from the start of an *HttpRequest* and is discarded at the end of each request.</span></span> <span data-ttu-id="f012e-243">Você pode acessá-la atribuindo um valor a uma entrada com chave ou solicitando o valor de uma determinada chave.</span><span class="sxs-lookup"><span data-stu-id="f012e-243">You can access it by  assigning a value to a keyed entry, or by requesting the value for a particular key.</span></span>

<span data-ttu-id="f012e-244">No exemplo a seguir, [Middleware](xref:fundamentals/middleware/index) adiciona `isVerified` à coleção `Items`.</span><span class="sxs-lookup"><span data-stu-id="f012e-244">In the following sample, [Middleware](xref:fundamentals/middleware/index) adds `isVerified` to the `Items` collection.</span></span>

```csharp
app.Use(async (context, next) =>
{
    // perform some verification
    context.Items["isVerified"] = true;
    await next.Invoke();
});
```

<span data-ttu-id="f012e-245">Posteriormente no pipeline, outro middleware poderia acessá-la:</span><span class="sxs-lookup"><span data-stu-id="f012e-245">Later in the pipeline, another middleware could access it:</span></span>

```csharp
app.Run(async (context) =>
{
    await context.Response.WriteAsync("Verified request? " + 
        context.Items["isVerified"]);
});
```

<span data-ttu-id="f012e-246">Para middleware que será usado apenas por um único aplicativo, chaves `string` são aceitáveis.</span><span class="sxs-lookup"><span data-stu-id="f012e-246">For middleware that will only be used by a single app, `string` keys are acceptable.</span></span> <span data-ttu-id="f012e-247">No entanto, middleware que será compartilhado entre aplicativos deve usar chaves de objeto exclusivas para evitar qualquer possibilidade de colisões de chaves.</span><span class="sxs-lookup"><span data-stu-id="f012e-247">However, middleware that will be shared between applications should use unique object keys to avoid any chance of key collisions.</span></span> <span data-ttu-id="f012e-248">Se estiver desenvolvendo middleware que precisa funcionar em vários aplicativos, use uma chave de objeto exclusiva definida na classe do middleware, como mostrado abaixo:</span><span class="sxs-lookup"><span data-stu-id="f012e-248">If you are developing middleware that must work across multiple applications, use a unique object key defined in your middleware class as shown below:</span></span>

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

<span data-ttu-id="f012e-249">Outros códigos podem acessar o valor armazenado em `HttpContext.Items` usando a chave exposta pela classe do middleware:</span><span class="sxs-lookup"><span data-stu-id="f012e-249">Other code can access the value stored in `HttpContext.Items` using the key exposed by the middleware class:</span></span>

```csharp
public class HomeController : Controller
{
    public IActionResult Index()
    {
        string value = HttpContext.Items[SampleMiddleware.SampleKey];
    }
}
```

<span data-ttu-id="f012e-250">Essa abordagem também tem a vantagem de eliminar a repetição de "cadeias de caracteres mágicas" em vários locais no código.</span><span class="sxs-lookup"><span data-stu-id="f012e-250">This approach also has the advantage of eliminating repetition of "magic strings" in multiple places in the code.</span></span>

<a name="appstate-errors"></a>

## <a name="application-state-data"></a><span data-ttu-id="f012e-251">Dados de estado do aplicativo</span><span class="sxs-lookup"><span data-stu-id="f012e-251">Application state data</span></span>

<span data-ttu-id="f012e-252">Use a [Injeção de dependência](xref:fundamentals/dependency-injection) para disponibilizar dados para todos os usuários:</span><span class="sxs-lookup"><span data-stu-id="f012e-252">Use [Dependency Injection](xref:fundamentals/dependency-injection) to make data available to all users:</span></span>

1. <span data-ttu-id="f012e-253">Defina um serviço que contém os dados (por exemplo, uma classe chamada `MyAppData`).</span><span class="sxs-lookup"><span data-stu-id="f012e-253">Define a service containing the data (for example, a class named `MyAppData`).</span></span>

```csharp
public class MyAppData
{
    // Declare properties/methods/etc.
} 
```
2. <span data-ttu-id="f012e-254">Adicione a classe de serviço a `ConfigureServices` (por exemplo, `services.AddSingleton<MyAppData>();`).</span><span class="sxs-lookup"><span data-stu-id="f012e-254">Add the service class to `ConfigureServices` (for example `services.AddSingleton<MyAppData>();`).</span></span>
3. <span data-ttu-id="f012e-255">Consuma a classe do serviço de dados em cada controlador:</span><span class="sxs-lookup"><span data-stu-id="f012e-255">Consume the data service class in each controller:</span></span>

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

## <a name="common-errors-when-working-with-session"></a><span data-ttu-id="f012e-256">Erros comuns ao trabalhar com a sessão</span><span class="sxs-lookup"><span data-stu-id="f012e-256">Common errors when working with session</span></span>

* <span data-ttu-id="f012e-257">"Não é possível resolver o serviço para o tipo 'Microsoft.Extensions.Caching.Distributed.IDistributedCache' ao tentar ativar 'Microsoft.AspNetCore.Session.DistributedSessionStore'."</span><span class="sxs-lookup"><span data-stu-id="f012e-257">"Unable to resolve service for type 'Microsoft.Extensions.Caching.Distributed.IDistributedCache' while attempting to activate 'Microsoft.AspNetCore.Session.DistributedSessionStore'."</span></span>

  <span data-ttu-id="f012e-258">Geralmente, isso é causado quando não é configurada pelo menos uma implementação de `IDistributedCache`.</span><span class="sxs-lookup"><span data-stu-id="f012e-258">This is usually caused by failing to configure at least one `IDistributedCache` implementation.</span></span> <span data-ttu-id="f012e-259">Para obter mais informações, confira [Trabalhar com um cache distribuído](xref:performance/caching/distributed) e [Cache na memória](xref:performance/caching/memory).</span><span class="sxs-lookup"><span data-stu-id="f012e-259">For more information, see [Work with a Distributed Cache](xref:performance/caching/distributed) and [In memory caching](xref:performance/caching/memory).</span></span>

* <span data-ttu-id="f012e-260">Caso o middleware da sessão não consiga persistir uma sessão (por exemplo, se o banco de dados não estiver disponível), ele registrará a exceção e a ignorará.</span><span class="sxs-lookup"><span data-stu-id="f012e-260">In the event that the session middleware fails to persist a session (for example: if the database isn't available), it logs the exception and swallows it.</span></span> <span data-ttu-id="f012e-261">A solicitação continuará normalmente, o que leva a um comportamento muito imprevisível.</span><span class="sxs-lookup"><span data-stu-id="f012e-261">The request will then continue normally, which leads to very unpredictable behavior.</span></span>

<span data-ttu-id="f012e-262">Um exemplo típico:</span><span class="sxs-lookup"><span data-stu-id="f012e-262">A typical example:</span></span>

<span data-ttu-id="f012e-263">Alguém armazena um carrinho de compras na sessão.</span><span class="sxs-lookup"><span data-stu-id="f012e-263">Someone stores a shopping basket in session.</span></span> <span data-ttu-id="f012e-264">O usuário adiciona um item, mas a confirmação falha.</span><span class="sxs-lookup"><span data-stu-id="f012e-264">The user adds an item but the commit fails.</span></span> <span data-ttu-id="f012e-265">O aplicativo não sabe sobre a falha e exibe a mensagem "O item foi adicionado", o que não é verdadeiro.</span><span class="sxs-lookup"><span data-stu-id="f012e-265">The app doesn't know about the failure so it reports the message "The item has been added", which isn't true.</span></span>

<span data-ttu-id="f012e-266">A maneira recomendada de verificar esses erros é chamar `await feature.Session.CommitAsync();` do código do aplicativo quando você terminar de gravar a sessão.</span><span class="sxs-lookup"><span data-stu-id="f012e-266">The recommended way to check for such errors is to call `await feature.Session.CommitAsync();` from app code when you're done writing to the session.</span></span> <span data-ttu-id="f012e-267">Em seguida, você pode fazer o que quiser com o erro.</span><span class="sxs-lookup"><span data-stu-id="f012e-267">Then you can do what you like with the error.</span></span> <span data-ttu-id="f012e-268">Isso funciona da mesma forma ao chamar `LoadAsync`.</span><span class="sxs-lookup"><span data-stu-id="f012e-268">It works the same way when calling `LoadAsync`.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="f012e-269">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="f012e-269">Additional resources</span></span>

* [<span data-ttu-id="f012e-270">ASP.NET Core 1. x: código de exemplo usado neste documento</span><span class="sxs-lookup"><span data-stu-id="f012e-270">ASP.NET Core 1.x: Sample code used in this document</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/sample/src/WebAppSession)
* [<span data-ttu-id="f012e-271">ASP.NET Core 2. x: código de exemplo usado neste documento</span><span class="sxs-lookup"><span data-stu-id="f012e-271">ASP.NET Core 2.x: Sample code used in this document</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/sample/src/WebAppSessionDotNetCore2.0App)

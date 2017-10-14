---
title: "Estado de sessão e de aplicativo no núcleo do ASP.NET"
author: rick-anderson
description: "Abordagens para preservar de aplicativo e o estado do usuário (sessão) entre as solicitações."
keywords: "Postagem do ASP.NET Core, o estado do aplicativo, estado de sessão, querystring,"
ms.author: riande
manager: wpickett
ms.date: 10/08/2017
ms.topic: article
ms.assetid: 18cda488-0769-4cb9-82f6-4c6685f2045d
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/app-state
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d4d10ef45d562f34c3f8b5ce025abaf763c862d3
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/13/2017
---
# <a name="introduction-to-session-and-application-state-in-aspnet-core"></a><span data-ttu-id="8a581-104">Introdução ao estado de sessão e de aplicativo no núcleo do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="8a581-104">Introduction to session and application state in ASP.NET Core</span></span>

<span data-ttu-id="8a581-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), e [Diana LaRose](https://github.com/DianaLaRose)</span><span class="sxs-lookup"><span data-stu-id="8a581-105">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), and [Diana LaRose](https://github.com/DianaLaRose)</span></span>

<span data-ttu-id="8a581-106">HTTP é um protocolo sem monitoração de estado.</span><span class="sxs-lookup"><span data-stu-id="8a581-106">HTTP is a stateless protocol.</span></span> <span data-ttu-id="8a581-107">Um servidor web trata cada solicitação HTTP como uma solicitação independente e não manter valores de usuário de solicitações anteriores.</span><span class="sxs-lookup"><span data-stu-id="8a581-107">A web server treats each HTTP request as an independent request and does not retain user values from previous requests.</span></span> <span data-ttu-id="8a581-108">Este artigo aborda diferentes maneiras de preservar o estado da sessão entre as solicitações e aplicativo.</span><span class="sxs-lookup"><span data-stu-id="8a581-108">This article discusses different ways to preserve application and session state between requests.</span></span> 

## <a name="session-state"></a><span data-ttu-id="8a581-109">Estado da sessão</span><span class="sxs-lookup"><span data-stu-id="8a581-109">Session state</span></span>

<span data-ttu-id="8a581-110">O estado de sessão é um recurso do ASP.NET Core que você pode usar para salvar e armazenar dados de usuário enquanto o usuário navega seu aplicativo Web.</span><span class="sxs-lookup"><span data-stu-id="8a581-110">Session state is a feature in ASP.NET Core that you can use to save and store user data while the user browses your web app.</span></span> <span data-ttu-id="8a581-111">Estado de sessão consiste em uma tabela de hash ou dicionário no servidor, persiste dados em solicitações de um navegador.</span><span class="sxs-lookup"><span data-stu-id="8a581-111">Consisting of a dictionary or hash table on the server, session state persists data across requests from a browser.</span></span> <span data-ttu-id="8a581-112">Os dados da sessão são apoiados por um cache.</span><span class="sxs-lookup"><span data-stu-id="8a581-112">The session data is backed by a cache.</span></span>

<span data-ttu-id="8a581-113">ASP.NET Core mantém o estado de sessão, fornecendo um cookie que contém a ID de sessão, que é enviada para o servidor com cada solicitação de cliente.</span><span class="sxs-lookup"><span data-stu-id="8a581-113">ASP.NET Core maintains session state by giving the client a cookie that contains the session ID, which is sent to the server with each request.</span></span> <span data-ttu-id="8a581-114">O servidor usa a ID de sessão para buscar os dados da sessão.</span><span class="sxs-lookup"><span data-stu-id="8a581-114">The server uses the session ID to fetch the session data.</span></span> <span data-ttu-id="8a581-115">Como o cookie de sessão é específico para o navegador, você não pode compartilhar sessões entre navegadores.</span><span class="sxs-lookup"><span data-stu-id="8a581-115">Because the session cookie is specific to the browser, you cannot share sessions across browsers.</span></span> <span data-ttu-id="8a581-116">Cookies de sessão são excluídos somente quando termina a sessão do navegador.</span><span class="sxs-lookup"><span data-stu-id="8a581-116">Session cookies are deleted only when the browser session ends.</span></span> <span data-ttu-id="8a581-117">Se um cookie for recebido de uma sessão expirada, uma nova sessão que usa o mesmo cookie de sessão é criada.</span><span class="sxs-lookup"><span data-stu-id="8a581-117">If a cookie is received for an expired session, a new session that uses the same session cookie  is created.</span></span> 

<span data-ttu-id="8a581-118">O servidor mantém uma sessão por um tempo limitado, após a última solicitação.</span><span class="sxs-lookup"><span data-stu-id="8a581-118">The server retains a session for a limited time after the last request.</span></span> <span data-ttu-id="8a581-119">Você pode definir o tempo limite da sessão ou use o valor padrão de 20 minutos.</span><span class="sxs-lookup"><span data-stu-id="8a581-119">You can either set the session timeout or use the default value of 20 minutes.</span></span> <span data-ttu-id="8a581-120">Estado da sessão é ideal para armazenar dados de usuário que são específico para uma sessão específica, mas não precisam ser mantidos permanentemente.</span><span class="sxs-lookup"><span data-stu-id="8a581-120">Session state is ideal for storing user data that is specific to a particular session but doesn’t need to be persisted permanently.</span></span> <span data-ttu-id="8a581-121">Dados são excluídos do armazenamento de backup ou quando você chamar `Session.Clear` ou quando a sessão expira no repositório de dados.</span><span class="sxs-lookup"><span data-stu-id="8a581-121">Data is deleted from the backing store either when you call `Session.Clear` or when the session expires in the data store.</span></span> <span data-ttu-id="8a581-122">O servidor não sabe quando o navegador for fechado ou quando o cookie de sessão é excluído.</span><span class="sxs-lookup"><span data-stu-id="8a581-122">The server does not know when the browser is closed or when the session cookie is deleted.</span></span>

> [!WARNING]
> <span data-ttu-id="8a581-123">Não armazene dados confidenciais na sessão.</span><span class="sxs-lookup"><span data-stu-id="8a581-123">Do not store sensitive data in session.</span></span> <span data-ttu-id="8a581-124">O cliente não pode fechar o navegador e limpar o cookie de sessão (e alguns navegadores atividade cookies de sessão em windows).</span><span class="sxs-lookup"><span data-stu-id="8a581-124">The client might not close the browser and clear the session cookie (and some browsers keep session cookies alive across windows).</span></span> <span data-ttu-id="8a581-125">Além disso, uma sessão não pode ser restrita a um único usuário; o próximo usuário pode continuar com a mesma sessão.</span><span class="sxs-lookup"><span data-stu-id="8a581-125">Also, a session might not be restricted to a single user; the next user might continue with the same session.</span></span>

<span data-ttu-id="8a581-126">O provedor de sessão na memória armazena dados de sessão no servidor local.</span><span class="sxs-lookup"><span data-stu-id="8a581-126">The in-memory session provider stores session data on the local server.</span></span> <span data-ttu-id="8a581-127">Se você planeja executar seu aplicativo web em um farm de servidores, você deve usar sessões Autoadesivas para ligar cada sessão para um servidor específico.</span><span class="sxs-lookup"><span data-stu-id="8a581-127">If you plan to run your web app on a server farm, you must use sticky sessions to tie each session to a specific server.</span></span> <span data-ttu-id="8a581-128">A plataforma Windows Azure Web Sites padrão sessões Autoadesivas (Application Request Routing ou ARR).</span><span class="sxs-lookup"><span data-stu-id="8a581-128">The Windows Azure Web Sites platform defaults to sticky sessions (Application Request Routing or ARR).</span></span> <span data-ttu-id="8a581-129">Entretanto, sessões Autoadesivas podem afetar a escalabilidade e complicar as atualizações de aplicativo web.</span><span class="sxs-lookup"><span data-stu-id="8a581-129">However, sticky sessions can affect scalability and complicate web app updates.</span></span> <span data-ttu-id="8a581-130">Uma opção melhor é usar o Redis ou distribuídas do SQL Server armazena em cache, que não exigem sessões Autoadesivas.</span><span class="sxs-lookup"><span data-stu-id="8a581-130">A better option is to use the Redis or SQL Server distributed caches, which don't require sticky sessions.</span></span> <span data-ttu-id="8a581-131">Para obter mais informações, consulte [trabalhando com um Cache distribuído](xref:performance/caching/distributed).</span><span class="sxs-lookup"><span data-stu-id="8a581-131">For more information, see [Working with a Distributed Cache](xref:performance/caching/distributed).</span></span> <span data-ttu-id="8a581-132">Para obter detalhes sobre como configurar provedores de serviços, consulte [sessão Configurando](#configuring-session) posteriormente neste artigo.</span><span class="sxs-lookup"><span data-stu-id="8a581-132">For details on setting up service providers, see [Configuring Session](#configuring-session) later in this article.</span></span>


<a name="temp"></a>
## <a name="tempdata"></a><span data-ttu-id="8a581-133">TempData</span><span class="sxs-lookup"><span data-stu-id="8a581-133">TempData</span></span>

<span data-ttu-id="8a581-134">ASP.NET MVC de núcleo expõe o [TempData](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) propriedade em uma [controlador](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.controller?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="8a581-134">ASP.NET Core MVC exposes the [TempData](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) property on a [controller](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.controller?view=aspnetcore-2.0).</span></span> <span data-ttu-id="8a581-135">Essa propriedade armazena dados até eles serem lidos.</span><span class="sxs-lookup"><span data-stu-id="8a581-135">This property stores data until it is read.</span></span> <span data-ttu-id="8a581-136">Os métodos `Keep` e `Peek` podem ser usados para examinar os dados sem exclusão.</span><span class="sxs-lookup"><span data-stu-id="8a581-136">The `Keep` and `Peek` methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="8a581-137">`TempData`é particularmente útil para redirecionamento, quando dados são necessários para mais de uma única solicitação.</span><span class="sxs-lookup"><span data-stu-id="8a581-137">`TempData` is particularly useful for redirection, when data is needed for more than a single request.</span></span> <span data-ttu-id="8a581-138">`TempData`é implementado por provedores de TempData, por exemplo, usar cookies ou estado de sessão.</span><span class="sxs-lookup"><span data-stu-id="8a581-138">`TempData` is implemented by TempData providers, for example, using either cookies or session state.</span></span>

<a name="tempdata-providers"></a>
### <a name="tempdata-providers"></a><span data-ttu-id="8a581-139">Provedores de TempData</span><span class="sxs-lookup"><span data-stu-id="8a581-139">TempData providers</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8a581-140">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8a581-140">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="8a581-141">No ASP.NET Core 2.0 e posterior, o provedor de TempData baseada em cookie é usado por padrão para armazenar TempData em cookies.</span><span class="sxs-lookup"><span data-stu-id="8a581-141">In ASP.NET Core 2.0 and later, the cookie-based TempData provider is used by default to store TempData in cookies.</span></span>

<span data-ttu-id="8a581-142">Os dados do cookie são codificados com o [Base64UrlTextEncoder](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.webutilities.base64urltextencoder?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="8a581-142">The cookie data is encoded with the [Base64UrlTextEncoder](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.webutilities.base64urltextencoder?view=aspnetcore-2.0).</span></span> <span data-ttu-id="8a581-143">Porque o cookie é criptografado e em partes, o cookie único tamanho limite encontrado no núcleo do ASP.NET 1. x não se aplica.</span><span class="sxs-lookup"><span data-stu-id="8a581-143">Because the cookie is encrypted and chunked, the single cookie size limit found in ASP.NET Core 1.x does not apply.</span></span> <span data-ttu-id="8a581-144">Os dados do cookie não são compactados porque a compactação de dados encryped pode levar a problemas de segurança, como o [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) e [violação](https://wikipedia.org/wiki/BREACH_(security_exploit)) ataques.</span><span class="sxs-lookup"><span data-stu-id="8a581-144">The cookie data is not compressed because compressing encryped data can lead to security problems such as the [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) and [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)) attacks.</span></span> <span data-ttu-id="8a581-145">Para obter mais informações sobre o provedor de TempData baseada em cookie, consulte [CookieTempDataProvider](https://github.com/aspnet/Mvc/blob/dev/src/Microsoft.AspNetCore.Mvc.ViewFeatures/ViewFeatures/CookieTempDataProvider.cs).</span><span class="sxs-lookup"><span data-stu-id="8a581-145">For more information on the cookie-based TempData provider, see [CookieTempDataProvider](https://github.com/aspnet/Mvc/blob/dev/src/Microsoft.AspNetCore.Mvc.ViewFeatures/ViewFeatures/CookieTempDataProvider.cs).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8a581-146">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8a581-146">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="8a581-147">No ASP.NET Core 1.0 e 1.1, o provedor de TempData de estado de sessão é o padrão.</span><span class="sxs-lookup"><span data-stu-id="8a581-147">In ASP.NET Core 1.0 and 1.1, the session state TempData provider is the default.</span></span>

--------------

### <a name="choosing-a-tempdata-provider"></a><span data-ttu-id="8a581-148">Escolha de um provedor de TempData</span><span class="sxs-lookup"><span data-stu-id="8a581-148">Choosing a TempData provider</span></span>

<span data-ttu-id="8a581-149">Escolha de um provedor de TempData envolve várias considerações, como:</span><span class="sxs-lookup"><span data-stu-id="8a581-149">Choosing a TempData provider involves several considerations, such as:</span></span>

1. <span data-ttu-id="8a581-150">O aplicativo já usa o estado da sessão para outras finalidades?</span><span class="sxs-lookup"><span data-stu-id="8a581-150">Does the application already use session state for other purposes?</span></span> <span data-ttu-id="8a581-151">Nesse caso, usar o provedor de TempData de estado de sessão tem sem custo adicional para o aplicativo (além do tamanho dos dados).</span><span class="sxs-lookup"><span data-stu-id="8a581-151">If so, using the session state TempData provider has no additional cost to the application (aside from the size of the data).</span></span>
2. <span data-ttu-id="8a581-152">O aplicativo usa TempData somente com moderação, para quantidades relativamente pequenas de dados (até 500 bytes)?</span><span class="sxs-lookup"><span data-stu-id="8a581-152">Does the application use TempData only sparingly, for relatively small amounts of data (up to 500 bytes)?</span></span> <span data-ttu-id="8a581-153">Se assim, o provedor de TempData cookie adicionará um pequeno custo para cada solicitação que transporta TempData.</span><span class="sxs-lookup"><span data-stu-id="8a581-153">If so, the cookie TempData provider will add a small cost to each request that carries TempData.</span></span> <span data-ttu-id="8a581-154">Caso contrário, o provedor de TempData de estado de sessão pode ser útil evitar o ciclo uma grande quantidade de dados em cada solicitação até que o TempData é consumido.</span><span class="sxs-lookup"><span data-stu-id="8a581-154">If not, the session state TempData provider can be beneficial to avoid round-tripping a large amount of data in each request until the TempData is consumed.</span></span>
3. <span data-ttu-id="8a581-155">O aplicativo é executado em um farm da web (vários servidores)?</span><span class="sxs-lookup"><span data-stu-id="8a581-155">Does the application run in a web farm (multiple servers)?</span></span> <span data-ttu-id="8a581-156">Nesse caso, não há nenhuma configuração adicional necessária para usar o provedor de TempData do cookie.</span><span class="sxs-lookup"><span data-stu-id="8a581-156">If so, there is no additional configuration needed to use the cookie TempData provider.</span></span>

> [!NOTE]
> <span data-ttu-id="8a581-157">A maioria dos clientes da web (como navegadores da web) impõem limites sobre o tamanho máximo de cada cookie, o número total de cookies ou ambos.</span><span class="sxs-lookup"><span data-stu-id="8a581-157">Most web clients (such as web browsers) enforce limits on the maximum size of each cookie, the total number of cookies, or both.</span></span> <span data-ttu-id="8a581-158">Portanto, ao usar o provedor de TempData cookies, verifique se que o aplicativo não excede esses limites.</span><span class="sxs-lookup"><span data-stu-id="8a581-158">Therefore, when using the cookie TempData provider, verify the app won't exceed these limits.</span></span> <span data-ttu-id="8a581-159">Considere o tamanho total dos dados, considerando as sobrecargas de criptografia e agrupamento.</span><span class="sxs-lookup"><span data-stu-id="8a581-159">Consider the total size of the data, accounting for the overheads of encryption and chunking.</span></span>

<span data-ttu-id="8a581-160">Para configurar o provedor de TempData para um aplicativo, registrar uma implementação de provedor TempData em `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="8a581-160">To configure the TempData provider for an application, register a TempData provider implementation in `ConfigureServices`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services
        .AddMvc()
        .AddSessionStateTempDataProvider();

    // The Session State TempData Provider requires adding the session state service
    services.AddSession();
}
```

## <a name="query-strings"></a><span data-ttu-id="8a581-161">Cadeias de caracteres de consulta</span><span class="sxs-lookup"><span data-stu-id="8a581-161">Query strings</span></span>

<span data-ttu-id="8a581-162">Você pode passar uma quantidade limitada de dados de uma solicitação para outro, adicionando-a cadeia de caracteres de consulta da nova solicitação.</span><span class="sxs-lookup"><span data-stu-id="8a581-162">You can pass a limited amount of data from one request to another by adding it to the new request’s query string.</span></span> <span data-ttu-id="8a581-163">Isso é útil para capturar o estado de uma maneira persistente que permite que os links com estado inserido deve ser compartilhado por email ou por redes sociais.</span><span class="sxs-lookup"><span data-stu-id="8a581-163">This is useful for capturing state in a persistent manner that allows links with embedded state to be shared through email or social networks.</span></span> <span data-ttu-id="8a581-164">No entanto, por esse motivo, você nunca deve usar cadeias de caracteres de consulta para dados confidenciais.</span><span class="sxs-lookup"><span data-stu-id="8a581-164">However, for this reason,  you should never use query strings for sensitive data.</span></span> <span data-ttu-id="8a581-165">Além de ser facilmente compartilhados, inclusive dados em cadeias de caracteres de consulta pode criar oportunidades de [falsificação de solicitação entre sites (CSRF)](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) ataques, que podem enganar os usuários a visitar sites mal-intencionados enquanto autenticado.</span><span class="sxs-lookup"><span data-stu-id="8a581-165">In addition to being easily shared, including data in query strings can create opportunities for [Cross-Site Request Forgery (CSRF)](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) attacks, which can trick users into visiting malicious sites while authenticated.</span></span> <span data-ttu-id="8a581-166">Os invasores podem roubar dados do usuário de seu aplicativo ou executar ações mal-intencionadas em nome do usuário.</span><span class="sxs-lookup"><span data-stu-id="8a581-166">Attackers can then steal user data from your app or take malicious actions on behalf of the user.</span></span> <span data-ttu-id="8a581-167">Qualquer estado preservado application ou session deve proteger contra ataques CSRF.</span><span class="sxs-lookup"><span data-stu-id="8a581-167">Any preserved application or session state must protect against CSRF attacks.</span></span> <span data-ttu-id="8a581-168">Para obter mais informações sobre ataques CSRF, consulte [ataques impedindo intersite solicitar CSRF (falsificação XSRF /) no núcleo do ASP.NET](../security/anti-request-forgery.md).</span><span class="sxs-lookup"><span data-stu-id="8a581-168">For more information on CSRF attacks, see [Preventing Cross-Site Request Forgery (XSRF/CSRF) Attacks in ASP.NET Core](../security/anti-request-forgery.md).</span></span>

## <a name="post-data-and-hidden-fields"></a><span data-ttu-id="8a581-169">Dados de postagem e campos ocultos</span><span class="sxs-lookup"><span data-stu-id="8a581-169">Post data and hidden fields</span></span>

<span data-ttu-id="8a581-170">Dados podem ser salvos em campos de formulário oculto e lançados novamente na próxima solicitação.</span><span class="sxs-lookup"><span data-stu-id="8a581-170">Data can be saved in hidden form fields and posted back on the next request.</span></span> <span data-ttu-id="8a581-171">Isso é comum em formulários de várias páginas.</span><span class="sxs-lookup"><span data-stu-id="8a581-171">This is common in multipage forms.</span></span> <span data-ttu-id="8a581-172">No entanto, como o cliente pode potencialmente adulterar os dados, o servidor deve sempre validá-lo novamente.</span><span class="sxs-lookup"><span data-stu-id="8a581-172">However, because the  client can potentially tamper with the data, the server must always revalidate it.</span></span> 

## <a name="cookies"></a><span data-ttu-id="8a581-173">Cookies</span><span class="sxs-lookup"><span data-stu-id="8a581-173">Cookies</span></span>

<span data-ttu-id="8a581-174">Cookies fornecem uma maneira de armazenar dados específicos do usuário em aplicativos da web.</span><span class="sxs-lookup"><span data-stu-id="8a581-174">Cookies provide a way to store user-specific data in web applications.</span></span> <span data-ttu-id="8a581-175">Porque os cookies são enviados com cada solicitação, seu tamanho deve ser reduzido ao mínimo.</span><span class="sxs-lookup"><span data-stu-id="8a581-175">Because cookies are sent with every request, their size should be kept to a minimum.</span></span> <span data-ttu-id="8a581-176">Idealmente, apenas um identificador deve ser armazenado em um cookie com os dados reais armazenados no servidor.</span><span class="sxs-lookup"><span data-stu-id="8a581-176">Ideally, only an identifier should be stored in a cookie with the actual data stored on the server.</span></span> <span data-ttu-id="8a581-177">A maioria dos navegadores restringir cookies 4096 bytes.</span><span class="sxs-lookup"><span data-stu-id="8a581-177">Most browsers restrict cookies to 4096 bytes.</span></span> <span data-ttu-id="8a581-178">Além disso, somente um número limitado dos cookies está disponível para cada domínio.</span><span class="sxs-lookup"><span data-stu-id="8a581-178">In addition, only a limited number of cookies are available for each domain.</span></span>  

<span data-ttu-id="8a581-179">Como os cookies estão sujeitos a falsificações, eles devem ser validados no servidor.</span><span class="sxs-lookup"><span data-stu-id="8a581-179">Because cookies are subject to tampering, they must be validated on the server.</span></span> <span data-ttu-id="8a581-180">Embora a durabilidade do cookie em um cliente esteja sujeito a expiração e a intervenção do usuário, elas geralmente são a forma mais durável de persistência de dados no cliente.</span><span class="sxs-lookup"><span data-stu-id="8a581-180">Although the durability of the cookie on a client is subject to user intervention and expiration, they are generally the most durable form of data persistence on the client.</span></span>

<span data-ttu-id="8a581-181">Cookies são usados para personalização, onde o conteúdo é personalizado para um usuário conhecido.</span><span class="sxs-lookup"><span data-stu-id="8a581-181">Cookies are often used for personalization, where content is customized for a known user.</span></span> <span data-ttu-id="8a581-182">Como o usuário é identificado apenas e não autenticado na maioria dos casos, você normalmente pode proteger um cookie ao armazenar o nome de usuário, nome da conta ou uma ID de usuário exclusiva (como um GUID) no cookie.</span><span class="sxs-lookup"><span data-stu-id="8a581-182">Because the user is only identified and not authenticated in most cases, you can typically secure a cookie by storing the user name, account name, or a unique user ID (such as a GUID) in the cookie.</span></span> <span data-ttu-id="8a581-183">Você pode usar o cookie para acessar a infraestrutura de personalização de usuário de um site.</span><span class="sxs-lookup"><span data-stu-id="8a581-183">You can then use the cookie to access the user personalization infrastructure of a site.</span></span>

## <a name="httpcontextitems"></a><span data-ttu-id="8a581-184">HttpContext</span><span class="sxs-lookup"><span data-stu-id="8a581-184">HttpContext.Items</span></span>

<span data-ttu-id="8a581-185">O `Items` coleção é um bom local para armazenar dados que é necessário somente durante processamento de uma determinada solicitação.</span><span class="sxs-lookup"><span data-stu-id="8a581-185">The `Items` collection is a good location to store data that is needed only while processing one particular request.</span></span> <span data-ttu-id="8a581-186">O conteúdo da coleção é descartado após cada solicitação.</span><span class="sxs-lookup"><span data-stu-id="8a581-186">The collection's contents are discarded after each request.</span></span> <span data-ttu-id="8a581-187">O `Items` coleção melhor é usada como uma maneira de componentes ou middleware para comunicar-se quando eles operam em pontos diferentes durante uma solicitação e não têm nenhuma maneira direta para passar parâmetros.</span><span class="sxs-lookup"><span data-stu-id="8a581-187">The `Items` collection is best used as a way for components or middleware to communicate when they operate at different points in time during a request and have no direct way to pass parameters.</span></span> <span data-ttu-id="8a581-188">Para obter mais informações, consulte [trabalhando com HttpContext](#working-with-httpcontextitems), mais adiante neste artigo.</span><span class="sxs-lookup"><span data-stu-id="8a581-188">For more information, see [Working with HttpContext.Items](#working-with-httpcontextitems), later in this article.</span></span>

## <a name="cache"></a><span data-ttu-id="8a581-189">Cache</span><span class="sxs-lookup"><span data-stu-id="8a581-189">Cache</span></span>

<span data-ttu-id="8a581-190">O cache é uma maneira eficiente de armazenar e recuperar dados.</span><span class="sxs-lookup"><span data-stu-id="8a581-190">Caching is an efficient way to store and retrieve data.</span></span> <span data-ttu-id="8a581-191">Você pode controlar o tempo de vida de itens em cache com base na hora e outras considerações.</span><span class="sxs-lookup"><span data-stu-id="8a581-191">You can control the lifetime of cached items based on time and other considerations.</span></span> <span data-ttu-id="8a581-192">Saiba mais sobre [cache](../performance/caching/index.md).</span><span class="sxs-lookup"><span data-stu-id="8a581-192">Learn more about [Caching](../performance/caching/index.md).</span></span>

<a name="session"></a>
## <a name="working-with-session-state"></a><span data-ttu-id="8a581-193">Trabalhando com o estado de sessão</span><span class="sxs-lookup"><span data-stu-id="8a581-193">Working with Session State</span></span>

### <a name="configuring-session"></a><span data-ttu-id="8a581-194">Configuração de sessão</span><span class="sxs-lookup"><span data-stu-id="8a581-194">Configuring Session</span></span>

<span data-ttu-id="8a581-195">O `Microsoft.AspNetCore.Session` pacote fornece middleware para gerenciar o estado da sessão.</span><span class="sxs-lookup"><span data-stu-id="8a581-195">The `Microsoft.AspNetCore.Session` package provides middleware for managing session state.</span></span> <span data-ttu-id="8a581-196">Para habilitar o middleware de sessão, `Startup`deve conter:</span><span class="sxs-lookup"><span data-stu-id="8a581-196">To enable the session middleware, `Startup`must contain:</span></span>

- <span data-ttu-id="8a581-197">Qualquer uma da [IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache) caches de memória.</span><span class="sxs-lookup"><span data-stu-id="8a581-197">Any of the [IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache) memory caches.</span></span> <span data-ttu-id="8a581-198">O `IDistributedCache` implementação é usada como um repositório de backup para a sessão.</span><span class="sxs-lookup"><span data-stu-id="8a581-198">The `IDistributedCache` implementation is used as a backing store for session.</span></span>
- <span data-ttu-id="8a581-199">[AddSession](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions#Microsoft_Extensions_DependencyInjection_SessionServiceCollectionExtensions_AddSession_Microsoft_Extensions_DependencyInjection_IServiceCollection_) chamar, que requer o pacote do NuGet "Microsoft.AspNetCore.Session".</span><span class="sxs-lookup"><span data-stu-id="8a581-199">[AddSession](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions#Microsoft_Extensions_DependencyInjection_SessionServiceCollectionExtensions_AddSession_Microsoft_Extensions_DependencyInjection_IServiceCollection_) call, which requires NuGet package "Microsoft.AspNetCore.Session".</span></span>
- <span data-ttu-id="8a581-200">[UseSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions#methods_) chamar.</span><span class="sxs-lookup"><span data-stu-id="8a581-200">[UseSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions#methods_) call.</span></span>

<span data-ttu-id="8a581-201">O código a seguir mostra como configurar o provedor de sessão na memória.</span><span class="sxs-lookup"><span data-stu-id="8a581-201">The following code shows how to set up the in-memory session provider.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8a581-202">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8a581-202">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](app-state/sample/src/WebAppSessionDotNetCore2.0App/Startup.cs?highlight=11-19,24)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8a581-203">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8a581-203">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](app-state/sample/src/WebAppSession/Startup.cs?highlight=11-19,24)]

---

<span data-ttu-id="8a581-204">Você pode fazer referência a sessão de `HttpContext` quando ele é instalado e configurado.</span><span class="sxs-lookup"><span data-stu-id="8a581-204">You can reference Session from `HttpContext` once it is installed and configured.</span></span>

<span data-ttu-id="8a581-205">Se você tentar acessar `Session` antes de `UseSession` tiver sido chamado, a exceção `InvalidOperationException: Session has not been configured for this application or request` é lançada.</span><span class="sxs-lookup"><span data-stu-id="8a581-205">If you try to access `Session` before `UseSession` has been called, the exception `InvalidOperationException: Session has not been configured for this application or request` is thrown.</span></span>

<span data-ttu-id="8a581-206">Se você tentar criar um novo `Session` (ou seja, nenhum cookie de sessão foi criado) depois que você já começou a gravar o `Response` fluxo, a exceção `InvalidOperationException: The session cannot be established after the response has started` é lançada.</span><span class="sxs-lookup"><span data-stu-id="8a581-206">If you try to create a new `Session` (that is, no session cookie has been created) after you have already begun writing to the `Response` stream, the exception `InvalidOperationException: The session cannot be established after the response has started` is thrown.</span></span> <span data-ttu-id="8a581-207">A exceção pode ser encontrada no log do servidor web; ele não será exibido no navegador.</span><span class="sxs-lookup"><span data-stu-id="8a581-207">The exception can be found in the web server log; it will not be displayed in the browser.</span></span>

### <a name="loading-session-asynchronously"></a><span data-ttu-id="8a581-208">Carregamento de sessão de forma assíncrona</span><span class="sxs-lookup"><span data-stu-id="8a581-208">Loading Session asynchronously</span></span> 

<span data-ttu-id="8a581-209">O provedor de sessão padrão no ASP.NET Core carrega o registro da sessão de subjacente [IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache) repositório de forma assíncrona somente se o [ISession.LoadAsync](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession#Microsoft_AspNetCore_Http_ISession_LoadAsync) método for chamado explicitamente antes  o `TryGetValue`, `Set`, ou `Remove` métodos.</span><span class="sxs-lookup"><span data-stu-id="8a581-209">The default session provider in ASP.NET Core loads the session record from the underlying [IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache) store asynchronously only if the [ISession.LoadAsync](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession#Microsoft_AspNetCore_Http_ISession_LoadAsync) method is explicitly called before  the `TryGetValue`, `Set`, or `Remove` methods.</span></span> <span data-ttu-id="8a581-210">Se `LoadAsync` não for chamado pela primeira vez, a base de registro de sessão é carregado de forma síncrona, que pode afetar a capacidade de dimensionar do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="8a581-210">If `LoadAsync` is not called first, the underlying session record is loaded synchronously, which could potentially impact the ability of the app to scale.</span></span>

<span data-ttu-id="8a581-211">Para que aplicativos impor esse padrão, encapsule o [DistributedSessionStore](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsessionstore) e [DistributedSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsession) implementações com versões que geram uma exceção se o `LoadAsync` método não é chamado antes de `TryGetValue`, `Set`, ou `Remove`.</span><span class="sxs-lookup"><span data-stu-id="8a581-211">To have applications enforce this pattern, wrap the [DistributedSessionStore](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsessionstore) and [DistributedSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsession) implementations with versions that throw an exception if the `LoadAsync` method is not called before `TryGetValue`, `Set`, or `Remove`.</span></span> <span data-ttu-id="8a581-212">Registre as versões encapsuladas no contêiner de serviços.</span><span class="sxs-lookup"><span data-stu-id="8a581-212">Register the wrapped versions in the services container.</span></span>

### <a name="implementation-details"></a><span data-ttu-id="8a581-213">Detalhes de implementação</span><span class="sxs-lookup"><span data-stu-id="8a581-213">Implementation Details</span></span>

<span data-ttu-id="8a581-214">Sessão usa um cookie para controlar e identificar as solicitações de um navegador único.</span><span class="sxs-lookup"><span data-stu-id="8a581-214">Session uses a cookie to track and identify requests from a single browser.</span></span> <span data-ttu-id="8a581-215">Por padrão, esse cookie é denominado ". AspNet.Session"e usa um caminho de"/".</span><span class="sxs-lookup"><span data-stu-id="8a581-215">By default, this cookie is named ".AspNet.Session", and it uses a path of "/".</span></span> <span data-ttu-id="8a581-216">Como o padrão de cookie não especificar um domínio, ele não ficam disponíveis para o script do lado do cliente na página (como `CookieHttpOnly` padrão é `true`).</span><span class="sxs-lookup"><span data-stu-id="8a581-216">Because the cookie default does not specify a domain, it is not made available to the client-side script on the page (because `CookieHttpOnly` defaults to `true`).</span></span>

<span data-ttu-id="8a581-217">Para substituir os padrões de sessão, use `SessionOptions`:</span><span class="sxs-lookup"><span data-stu-id="8a581-217">To override session defaults, use `SessionOptions`:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8a581-218">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8a581-218">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](app-state/sample/src/WebAppSessionDotNetCore2.0App/StartupCopy.cs?name=snippet1&highlight=8-12)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8a581-219">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8a581-219">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](app-state/sample/src/WebAppSession/StartupCopy.cs?name=snippet1&highlight=8-12)]

---

<span data-ttu-id="8a581-220">O servidor usa o `IdleTimeout` propriedade para determinar quanto tempo uma sessão pode ficar ociosa antes de seu conteúdo está sendo abandonado.</span><span class="sxs-lookup"><span data-stu-id="8a581-220">The server uses the `IdleTimeout` property to determine how long a session can be idle before its contents are abandoned.</span></span> <span data-ttu-id="8a581-221">Essa propriedade é independente da expiração do cookie.</span><span class="sxs-lookup"><span data-stu-id="8a581-221">This property is independent of the cookie expiration.</span></span> <span data-ttu-id="8a581-222">Cada solicitação que passa por meio do middleware de sessão (lida ou gravada para) redefine o tempo limite.</span><span class="sxs-lookup"><span data-stu-id="8a581-222">Each request that passes through the Session middleware (read from or written to) resets the timeout.</span></span>

<span data-ttu-id="8a581-223">Porque `Session` é *não bloqueio*, se duas solicitações tentam modificar o conteúdo da sessão, o último deles substitui o primeiro.</span><span class="sxs-lookup"><span data-stu-id="8a581-223">Because `Session` is *non-locking*, if two requests both attempt to modify the contents of session, the last one overrides the first.</span></span> <span data-ttu-id="8a581-224">`Session`é implementado como um *sessão coerente*, que significa que todo o conteúdo é armazenado juntos.</span><span class="sxs-lookup"><span data-stu-id="8a581-224">`Session` is implemented as a *coherent session*, which means that all the contents are stored together.</span></span> <span data-ttu-id="8a581-225">Duas solicitações que estão modificando diferentes partes da sessão (chaves diferentes) ainda podem afetar uns aos outros.</span><span class="sxs-lookup"><span data-stu-id="8a581-225">Two requests that are modifying different parts of the session (different keys) might still impact each other.</span></span>

### <a name="setting-and-getting-session-values"></a><span data-ttu-id="8a581-226">Configuração e obter valores de sessão</span><span class="sxs-lookup"><span data-stu-id="8a581-226">Setting and getting Session values</span></span>

<span data-ttu-id="8a581-227">Sessão é acessada por meio de `Session` propriedade `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="8a581-227">Session is accessed through the `Session` property on `HttpContext`.</span></span> <span data-ttu-id="8a581-228">Esta propriedade é um [ISession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession) implementação.</span><span class="sxs-lookup"><span data-stu-id="8a581-228">This property is an [ISession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession) implementation.</span></span>

<span data-ttu-id="8a581-229">O exemplo a seguir mostra a configuração e a obtenção de um inteiro e uma cadeia de caracteres:</span><span class="sxs-lookup"><span data-stu-id="8a581-229">The following example shows setting and getting an int and a string:</span></span>

[!code-csharp[Main](app-state/sample/src/WebAppSession/Controllers/HomeController.cs?range=8-27,49)]

<span data-ttu-id="8a581-230">Se você adicionar os seguintes métodos de extensão, você pode definir e obter objetos serializáveis a sessão:</span><span class="sxs-lookup"><span data-stu-id="8a581-230">If you add the following extension methods, you can set and get serializable objects to Session:</span></span>

[!code-csharp[Main](app-state/sample/src/WebAppSession/Extensions/SessionExtensions.cs)]

<span data-ttu-id="8a581-231">O exemplo a seguir mostra como definir e obter um objeto serializável:</span><span class="sxs-lookup"><span data-stu-id="8a581-231">The following sample shows how to set and get a serializable object:</span></span>

[!code-csharp[Main](app-state/sample/src/WebAppSession/Controllers/HomeController.cs?name=snippet2)]


## <a name="working-with-httpcontextitems"></a><span data-ttu-id="8a581-232">Trabalhando com HttpContext</span><span class="sxs-lookup"><span data-stu-id="8a581-232">Working with HttpContext.Items</span></span>

<span data-ttu-id="8a581-233">O `HttpContext` abstração oferece suporte para uma coleção de dicionário do tipo `IDictionary<object, object>`, chamado `Items`.</span><span class="sxs-lookup"><span data-stu-id="8a581-233">The `HttpContext` abstraction provides support for a dictionary collection of type `IDictionary<object, object>`, called `Items`.</span></span> <span data-ttu-id="8a581-234">Essa coleção está disponível desde o início de uma *HttpRequest* e serão descartados no final de cada solicitação.</span><span class="sxs-lookup"><span data-stu-id="8a581-234">This collection is available from the start of an *HttpRequest* and is discarded at the end of each request.</span></span> <span data-ttu-id="8a581-235">Você pode acessá-lo, atribuindo um valor a uma entrada de chave, ou solicitando o valor para uma determinada chave.</span><span class="sxs-lookup"><span data-stu-id="8a581-235">You can access it by  assigning a value to a keyed entry, or by requesting the value for a particular key.</span></span>

<span data-ttu-id="8a581-236">No exemplo abaixo, [Middleware](middleware.md) adiciona `isVerified` para o `Items` coleção.</span><span class="sxs-lookup"><span data-stu-id="8a581-236">In the sample below, [Middleware](middleware.md) adds `isVerified` to the `Items` collection.</span></span>

```csharp
app.Use(async (context, next) =>
{
    // perform some verification
    context.Items["isVerified"] = true;
    await next.Invoke();
});
```

<span data-ttu-id="8a581-237">Mais tarde no pipeline, outro middleware pode acessá-lo:</span><span class="sxs-lookup"><span data-stu-id="8a581-237">Later in the pipeline, another middleware could access it:</span></span>

```csharp
app.Run(async (context) =>
{
    await context.Response.WriteAsync("Verified request? " + 
        context.Items["isVerified"]);
});
```

<span data-ttu-id="8a581-238">Para o middleware que será usado apenas por um único aplicativo, `string` chaves são aceitáveis.</span><span class="sxs-lookup"><span data-stu-id="8a581-238">For middleware that will only be used by a single app, `string` keys are acceptable.</span></span> <span data-ttu-id="8a581-239">No entanto, o middleware que será compartilhado entre aplicativos deve usar chaves de objeto exclusivo para evitar qualquer possibilidade de colisões de chaves.</span><span class="sxs-lookup"><span data-stu-id="8a581-239">However, middleware that will be shared between applications should use unique object keys to avoid any chance of key collisions.</span></span> <span data-ttu-id="8a581-240">Se você estiver desenvolvendo o middleware que deve trabalhar em vários aplicativos, use uma chave de objeto exclusivo definida em sua classe de middleware conforme mostrado abaixo:</span><span class="sxs-lookup"><span data-stu-id="8a581-240">If you are developing middleware that must work across multiple applications, use a unique object key defined in your middleware class as shown below:</span></span>

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

<span data-ttu-id="8a581-241">Outro código pode acessar o valor armazenado em `HttpContext.Items` usando a chave exposta pela classe middleware:</span><span class="sxs-lookup"><span data-stu-id="8a581-241">Other code can access the value stored in `HttpContext.Items` using the key exposed by the middleware class:</span></span>

```csharp
public class HomeController : Controller
{
    public IActionResult Index()
    {
        string value = HttpContext.Items[SampleMiddleware.SampleKey];
    }
}
```

<span data-ttu-id="8a581-242">Essa abordagem também tem a vantagem de eliminar a repetição de "magic cadeias de caracteres" em vários locais no código.</span><span class="sxs-lookup"><span data-stu-id="8a581-242">This approach also has the advantage of eliminating repetition of "magic strings" in multiple places in the code.</span></span>

<a name="appstate-errors"></a>

## <a name="application-state-data"></a><span data-ttu-id="8a581-243">Dados de estado do aplicativo</span><span class="sxs-lookup"><span data-stu-id="8a581-243">Application state data</span></span>

<span data-ttu-id="8a581-244">Use [injeção de dependência](xref:fundamentals/dependency-injection) para tornar dados disponíveis a todos os usuários:</span><span class="sxs-lookup"><span data-stu-id="8a581-244">Use [Dependency Injection](xref:fundamentals/dependency-injection) to make data available to all users:</span></span>

1. <span data-ttu-id="8a581-245">Definir um serviço que contém os dados (por exemplo, uma classe denominada `MyAppData`).</span><span class="sxs-lookup"><span data-stu-id="8a581-245">Define a service containing the data (for example, a class named `MyAppData`).</span></span>

```csharp
public class MyAppData
{
    // Declare properties/methods/etc.
} 
```
2. <span data-ttu-id="8a581-246">Adicione a classe de serviço para `ConfigureServices` (por exemplo `services.AddSingleton<MyAppData>();`).</span><span class="sxs-lookup"><span data-stu-id="8a581-246">Add the service class to `ConfigureServices` (for example `services.AddSingleton<MyAppData>();`).</span></span>
3. <span data-ttu-id="8a581-247">Consuma a classe de serviço de dados em cada controlador:</span><span class="sxs-lookup"><span data-stu-id="8a581-247">Consume the data service class in each controller:</span></span>

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

### <a name="common-errors-when-working-with-session"></a><span data-ttu-id="8a581-248">Erros comuns ao trabalhar com a sessão</span><span class="sxs-lookup"><span data-stu-id="8a581-248">Common errors when working with session</span></span>

* <span data-ttu-id="8a581-249">"Não é possível resolver o serviço para o tipo 'Microsoft.Extensions.Caching.Distributed.IDistributedCache' ao tentar ativar 'Microsoft.AspNetCore.Session.DistributedSessionStore'."</span><span class="sxs-lookup"><span data-stu-id="8a581-249">"Unable to resolve service for type 'Microsoft.Extensions.Caching.Distributed.IDistributedCache' while attempting to activate 'Microsoft.AspNetCore.Session.DistributedSessionStore'."</span></span>

  <span data-ttu-id="8a581-250">Isso geralmente é causado por falha ao configurar pelo menos um `IDistributedCache` implementação.</span><span class="sxs-lookup"><span data-stu-id="8a581-250">This is usually caused by failing to configure at least one `IDistributedCache` implementation.</span></span> <span data-ttu-id="8a581-251">Para obter mais informações, consulte [trabalhando com um Cache distribuído](xref:performance/caching/distributed) e [no cache de memória](xref:performance/caching/memory).</span><span class="sxs-lookup"><span data-stu-id="8a581-251">For more information, see [Working with a Distributed Cache](xref:performance/caching/distributed) and [In memory caching](xref:performance/caching/memory).</span></span>

### <a name="additional-resources"></a><span data-ttu-id="8a581-252">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="8a581-252">Additional Resources</span></span>


* [<span data-ttu-id="8a581-253">ASP.NET Core 1. x: exemplo de código usado neste documento</span><span class="sxs-lookup"><span data-stu-id="8a581-253">ASP.NET Core 1.x: Sample code used in this document</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/sample/src/WebAppSession)
* [<span data-ttu-id="8a581-254">ASP.NET Core 2. x: exemplo de código usado neste documento</span><span class="sxs-lookup"><span data-stu-id="8a581-254">ASP.NET Core 2.x: Sample code used in this document</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/sample/src/WebAppSessionDotNetCore2.0App)

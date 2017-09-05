---
title: "Implementação do servidor web kestrel no núcleo do ASP.NET"
author: tdykstra
description: Apresenta Kestrel, o servidor web de plataforma cruzada para o ASP.NET Core com base em libuv.
keywords: ASP.NET Core, Kestrel, libuv, prefixos de url
ms.author: tdykstra
manager: wpickett
ms.date: 08/02/2017
ms.topic: article
ms.assetid: 560bd66f-7dd0-4e68-b5fb-f31477e4b785
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/kestrel
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 451a548403c8fa0ed2befeb6969a3ee28fe34790
ms.sourcegitcommit: 74e22e08e3b08cb576e5184d16f4af5656c13c0c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/25/2017
---
# <a name="introduction-to-kestrel-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="b537d-104">Introdução a implementação do servidor web Kestrel no núcleo do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="b537d-104">Introduction to Kestrel web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="b537d-105">Por [Tom Dykstra](http://github.com/tdykstra), [Ross Carlos](https://github.com/Tratcher), e [Stephen Halter](https://twitter.com/halter73)</span><span class="sxs-lookup"><span data-stu-id="b537d-105">By [Tom Dykstra](http://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), and [Stephen Halter](https://twitter.com/halter73)</span></span>

<span data-ttu-id="b537d-106">Kestrel é uma plataforma cruzada [servidor web para o ASP.NET Core](index.md) com base em [libuv](https://github.com/libuv/libuv), uma biblioteca de e/s assíncrona de plataforma cruzada.</span><span class="sxs-lookup"><span data-stu-id="b537d-106">Kestrel is a cross-platform [web server for ASP.NET Core](index.md) based on [libuv](https://github.com/libuv/libuv), a cross-platform asynchronous I/O library.</span></span> <span data-ttu-id="b537d-107">Kestrel é o servidor web que está incluído por padrão nos modelos de projeto do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b537d-107">Kestrel is the web server that is included by default in ASP.NET Core project templates.</span></span> 

<span data-ttu-id="b537d-108">Kestrel suporta os seguintes recursos:</span><span class="sxs-lookup"><span data-stu-id="b537d-108">Kestrel supports the following features:</span></span>

  * <span data-ttu-id="b537d-109">HTTPS</span><span class="sxs-lookup"><span data-stu-id="b537d-109">HTTPS</span></span>
  * <span data-ttu-id="b537d-110">Atualização opaca usada para habilitar [WebSockets](https://github.com/aspnet/websockets)</span><span class="sxs-lookup"><span data-stu-id="b537d-110">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
  * <span data-ttu-id="b537d-111">Soquetes de UNIX de alto desempenho atrás Nginx</span><span class="sxs-lookup"><span data-stu-id="b537d-111">Unix sockets for high performance behind Nginx</span></span> 

<span data-ttu-id="b537d-112">Kestrel tem suporte em todas as plataformas e versões que dá suporte ao .NET Core.</span><span class="sxs-lookup"><span data-stu-id="b537d-112">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b537d-113">ASP.NET Core 2. x</span><span class="sxs-lookup"><span data-stu-id="b537d-113">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[<span data-ttu-id="b537d-114">Exibir ou baixar o código de exemplo para 2. x</span><span class="sxs-lookup"><span data-stu-id="b537d-114">View or download sample code for 2.x</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b537d-115">ASP.NET Core 1. x</span><span class="sxs-lookup"><span data-stu-id="b537d-115">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[<span data-ttu-id="b537d-116">Exibir ou baixar o exemplo de código 1. x</span><span class="sxs-lookup"><span data-stu-id="b537d-116">View or download sample code for 1.x</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1)

---

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="b537d-117">Quando usar Kestrel com um proxy reverso</span><span class="sxs-lookup"><span data-stu-id="b537d-117">When to use Kestrel with a reverse proxy</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b537d-118">ASP.NET Core 2. x</span><span class="sxs-lookup"><span data-stu-id="b537d-118">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="b537d-119">Você pode usar Kestrel sozinho ou com um *servidor proxy reverso*, como IIS, Nginx ou Apache.</span><span class="sxs-lookup"><span data-stu-id="b537d-119">You can use Kestrel by itself or with a *reverse proxy server*, such as IIS, Nginx, or Apache.</span></span> <span data-ttu-id="b537d-120">Um servidor proxy reverso recebe solicitações HTTP da Internet e encaminha-os para Kestrel após alguns tratamentos preliminar.</span><span class="sxs-lookup"><span data-stu-id="b537d-120">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![Kestrel se comunica diretamente com a Internet, sem um servidor proxy reverso](kestrel/_static/kestrel-to-internet2.png)

![Kestrel comunica-se indiretamente com a Internet através de um servidor proxy reverso, como o IIS, Nginx ou Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="b537d-123">Qualquer configuração &mdash; com ou sem um servidor de proxy reverso &mdash; também pode ser usado se Kestrel é exposta somente a uma rede interna.</span><span class="sxs-lookup"><span data-stu-id="b537d-123">Either configuration &mdash; with or without a reverse proxy server &mdash; can also be used if Kestrel is exposed only to an internal network.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b537d-124">ASP.NET Core 1. x</span><span class="sxs-lookup"><span data-stu-id="b537d-124">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="b537d-125">Se seu aplicativo aceita solicitações somente de uma rede interna, você pode usar Kestrel por si só.</span><span class="sxs-lookup"><span data-stu-id="b537d-125">If your application accepts requests only from an internal network, you can use Kestrel by itself.</span></span>

![Kestrel se comunica diretamente com a rede interna](kestrel/_static/kestrel-to-internal.png)

<span data-ttu-id="b537d-127">Se você expuser seu aplicativo com a Internet, você deve usar o IIS, Nginx ou Apache como um *servidor proxy reverso*.</span><span class="sxs-lookup"><span data-stu-id="b537d-127">If you expose your application to the Internet, you must use IIS, Nginx, or Apache as a *reverse proxy server*.</span></span> <span data-ttu-id="b537d-128">Um servidor proxy reverso recebe solicitações HTTP da Internet e encaminha-os para Kestrel após alguns tratamentos preliminar.</span><span class="sxs-lookup"><span data-stu-id="b537d-128">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![Kestrel comunica-se indiretamente com a Internet através de um servidor proxy reverso, como o IIS, Nginx ou Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="b537d-130">Um proxy reverso é necessário para implantações de borda (expostas ao tráfego da Internet) por motivos de segurança.</span><span class="sxs-lookup"><span data-stu-id="b537d-130">A reverse proxy is required for edge deployments (exposed to traffic from the Internet) for security reasons.</span></span> <span data-ttu-id="b537d-131">As versões 1. x do Kestrel não tem um conjunto completo de proteção contra ataques.</span><span class="sxs-lookup"><span data-stu-id="b537d-131">The 1.x versions of Kestrel don't have a full complement of defenses against attacks.</span></span> <span data-ttu-id="b537d-132">Isso inclui, mas não está limitado a tempos limite apropriado, limites de tamanho e os limites de conexão simultâneas.</span><span class="sxs-lookup"><span data-stu-id="b537d-132">This includes but isn't limited to appropriate timeouts, size limits, and concurrent connection limits.</span></span>

---

<span data-ttu-id="b537d-133">Um cenário que requer um proxy reverso é quando você tiver vários aplicativos que compartilham o mesmo IP e porta em execução em um único servidor.</span><span class="sxs-lookup"><span data-stu-id="b537d-133">A scenario that requires a reverse proxy is when you have multiple applications that share the same IP and port running on a single server.</span></span> <span data-ttu-id="b537d-134">Isso não funcionar com Kestrel diretamente como Kestrel não dá suporte a compartilhar o mesmo IP e porta entre vários processos.</span><span class="sxs-lookup"><span data-stu-id="b537d-134">That doesn't work with Kestrel directly because Kestrel doesn't support sharing the same IP and port between multiple processes.</span></span> <span data-ttu-id="b537d-135">Quando você configura Kestrel para escutar em uma porta, ele trata todo o tráfego para essa porta, independentemente de cabeçalho de host.</span><span class="sxs-lookup"><span data-stu-id="b537d-135">When you configure Kestrel to listen on a port, it handles all traffic for that port regardless of host header.</span></span> <span data-ttu-id="b537d-136">Um proxy reverso que pode compartilhar portas, em seguida, encaminhe para Kestrel em um único IP e porta.</span><span class="sxs-lookup"><span data-stu-id="b537d-136">A reverse proxy that can share ports must then forward to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="b537d-137">Mesmo se um servidor proxy reverso não é necessário, usando um pode ser uma boa opção por outros motivos:</span><span class="sxs-lookup"><span data-stu-id="b537d-137">Even if a reverse proxy server isn't required, using one might be a good choice for other reasons:</span></span>

* <span data-ttu-id="b537d-138">Ele pode limitar a área da superfície exposta.</span><span class="sxs-lookup"><span data-stu-id="b537d-138">It can limit your exposed surface area.</span></span>
* <span data-ttu-id="b537d-139">Ele fornece uma camada de configuração e proteção adicional opcional.</span><span class="sxs-lookup"><span data-stu-id="b537d-139">It provides an optional additional layer of configuration and defense.</span></span>
* <span data-ttu-id="b537d-140">Ele pode integrar melhor com a infraestrutura existente.</span><span class="sxs-lookup"><span data-stu-id="b537d-140">It might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="b537d-141">Ele simplifica a configuração SSL e balanceamento de carga.</span><span class="sxs-lookup"><span data-stu-id="b537d-141">It simplifies load balancing and SSL set-up.</span></span> <span data-ttu-id="b537d-142">Somente o servidor de proxy reverso requer um certificado SSL, e esse servidor pode se comunicar com seus servidores de aplicativos na rede interna usando HTTP simples.</span><span class="sxs-lookup"><span data-stu-id="b537d-142">Only your reverse proxy server requires an SSL certificate, and that server can communicate with your application servers on the internal network using plain HTTP.</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="b537d-143">Como usar Kestrel em aplicativos do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b537d-143">How to use Kestrel in ASP.NET Core apps</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b537d-144">ASP.NET Core 2. x</span><span class="sxs-lookup"><span data-stu-id="b537d-144">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="b537d-145">O [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) pacote está incluído no [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="b537d-145">The [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) package is included in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

<span data-ttu-id="b537d-146">Modelos de projeto do ASP.NET Core usam Kestrel por padrão.</span><span class="sxs-lookup"><span data-stu-id="b537d-146">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="b537d-147">Em *Program.cs*, as chamadas de código de modelo `CreateDefaultBuilder`, que chama [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) em segundo plano.</span><span class="sxs-lookup"><span data-stu-id="b537d-147">In *Program.cs*, the template code calls `CreateDefaultBuilder`, which calls [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) behind the scenes.</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

<span data-ttu-id="b537d-148">Se você precisar configurar as opções de Kestrel, chame `UseKestrel` na *Program.cs* conforme mostrado no exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="b537d-148">If you need to configure Kestrel options, call `UseKestrel` in *Program.cs* as shown in the following example:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b537d-149">ASP.NET Core 1. x</span><span class="sxs-lookup"><span data-stu-id="b537d-149">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="b537d-150">Instalar o [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) pacote NuGet.</span><span class="sxs-lookup"><span data-stu-id="b537d-150">Install the [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) NuGet package.</span></span>

<span data-ttu-id="b537d-151">Chamar o [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) método de extensão no `WebHostBuilder` no seu `Main` método, especificando um [opções Kestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions) que você precisa, como mostrado na próxima seção.</span><span class="sxs-lookup"><span data-stu-id="b537d-151">Call the [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) extension method on `WebHostBuilder` in your `Main` method, specifying any [Kestrel options](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions) that you need, as shown in the next section.</span></span>

[!code-csharp[](kestrel/sample1/Program.cs?name=snippet_Main&highlight=13-19)]

---

### <a name="kestrel-options"></a><span data-ttu-id="b537d-152">Opções de kestrel</span><span class="sxs-lookup"><span data-stu-id="b537d-152">Kestrel options</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b537d-153">ASP.NET Core 2. x</span><span class="sxs-lookup"><span data-stu-id="b537d-153">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="b537d-154">O servidor de web Kestrel tem opções de configuração de restrição são especialmente úteis em implantações para a Internet.</span><span class="sxs-lookup"><span data-stu-id="b537d-154">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span> <span data-ttu-id="b537d-155">Aqui estão alguns dos limites que você pode definir:</span><span class="sxs-lookup"><span data-stu-id="b537d-155">Here are some of the limits you can set:</span></span>

- <span data-ttu-id="b537d-156">Conexões de cliente máximo</span><span class="sxs-lookup"><span data-stu-id="b537d-156">Maximum client connections</span></span>
- <span data-ttu-id="b537d-157">Tamanho do corpo da solicitação máxima</span><span class="sxs-lookup"><span data-stu-id="b537d-157">Maximum request body size</span></span>
- <span data-ttu-id="b537d-158">Taxa de dados de corpo de solicitação mínimo</span><span class="sxs-lookup"><span data-stu-id="b537d-158">Minimum request body data rate</span></span>

<span data-ttu-id="b537d-159">Você define essas restrições e outras o `Limits` propriedade o [KestrelServerOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs) classe.</span><span class="sxs-lookup"><span data-stu-id="b537d-159">You set these constraints and others in the `Limits` property of the [KestrelServerOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs) class.</span></span> <span data-ttu-id="b537d-160">O `Limits` propriedade contém uma instância do [KestrelServerLimits](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs) classe.</span><span class="sxs-lookup"><span data-stu-id="b537d-160">The `Limits` property holds an instance of the [KestrelServerLimits](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs) class.</span></span> 

<span data-ttu-id="b537d-161">**Conexões de cliente máximo**</span><span class="sxs-lookup"><span data-stu-id="b537d-161">**Maximum client connections**</span></span>

<span data-ttu-id="b537d-162">O número máximo de conexões TCP abertas simultâneas pode ser definido para o aplicativo inteiro com o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="b537d-162">The maximum number of concurrent open TCP connections can be set for the entire application with the following code:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="b537d-163">Há um limite separado para conexões que foram atualizados de HTTP ou HTTPS para outro protocolo (por exemplo, em uma solicitação WebSocket).</span><span class="sxs-lookup"><span data-stu-id="b537d-163">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="b537d-164">Depois que uma conexão é atualizado, ele não é contado em relação a `MaxConcurrentConnections` limite.</span><span class="sxs-lookup"><span data-stu-id="b537d-164">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span> 

<span data-ttu-id="b537d-165">O número máximo de conexões é ilimitado (null) por padrão.</span><span class="sxs-lookup"><span data-stu-id="b537d-165">The maximum number of connections is unlimited (null) by default.</span></span>

<span data-ttu-id="b537d-166">**Tamanho do corpo da solicitação máxima**</span><span class="sxs-lookup"><span data-stu-id="b537d-166">**Maximum request body size**</span></span>

<span data-ttu-id="b537d-167">O tamanho do corpo da solicitação máximo padrão é 30.000.000 bytes, que é aproximadamente 28.6 MB.</span><span class="sxs-lookup"><span data-stu-id="b537d-167">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6MB.</span></span> 

<span data-ttu-id="b537d-168">A maneira recomendada para substituir o limite em um aplicativo ASP.NET MVC de núcleo é usar o [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) atributo em um método de ação:</span><span class="sxs-lookup"><span data-stu-id="b537d-168">The recommended way to override the limit in an ASP.NET Core MVC app is to use the [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="b537d-169">Aqui está um exemplo que mostra como configurar o limite para o aplicativo inteiro, cada solicitação:</span><span class="sxs-lookup"><span data-stu-id="b537d-169">Here's an example that shows how to configure the constraint for the entire application, every request:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=5)]

<span data-ttu-id="b537d-170">Você pode substituir a configuração em uma solicitação específica no middleware:</span><span class="sxs-lookup"><span data-stu-id="b537d-170">You can override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Limits&highlight=3-4)]
 
<span data-ttu-id="b537d-171">Uma exceção é gerada se você tentar configurar o limite de uma solicitação depois que o aplicativo começou a ler a solicitação.</span><span class="sxs-lookup"><span data-stu-id="b537d-171">An exception is thrown if you try to configure the limit on a request after the application has started reading the request.</span></span> <span data-ttu-id="b537d-172">Há um `IsReadOnly` propriedade que indica se o `MaxRequestBodySize` propriedade está no estado somente leitura, que significa que é muito tarde para configurar o limite.</span><span class="sxs-lookup"><span data-stu-id="b537d-172">There's an `IsReadOnly` property that tells you if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="b537d-173">**Taxa de dados de corpo de solicitação mínimo**</span><span class="sxs-lookup"><span data-stu-id="b537d-173">**Minimum request body data rate**</span></span>

<span data-ttu-id="b537d-174">Kestrel verifica a cada segundo se dados estão chegando a taxa especificada em bytes/segundo.</span><span class="sxs-lookup"><span data-stu-id="b537d-174">Kestrel checks every second if data is coming in at the specified rate in bytes/second.</span></span> <span data-ttu-id="b537d-175">Se a taxa de cair abaixo do mínimo, a conexão é atingiu o tempo limite. O período de cortesia é a quantidade de tempo que Kestrel dá ao cliente para aumentar sua taxa de envio até o mínimo; a taxa não é verificada durante esse período.</span><span class="sxs-lookup"><span data-stu-id="b537d-175">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate is not checked during that time.</span></span> <span data-ttu-id="b537d-176">O período de cortesia ajuda a evitar a remoção de conexões que inicialmente estão enviando dados em uma taxa baixa devido a partida lenta do TCP.</span><span class="sxs-lookup"><span data-stu-id="b537d-176">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="b537d-177">A taxa mínima de padrão é 240 bytes/segundo, com um período de cortesia de 5 segundos.</span><span class="sxs-lookup"><span data-stu-id="b537d-177">The default minimum rate is 240 bytes/second, with a 5 second grace period.</span></span>

<span data-ttu-id="b537d-178">Uma taxa mínima também se aplica à resposta.</span><span class="sxs-lookup"><span data-stu-id="b537d-178">A minimum rate also applies to the response.</span></span> <span data-ttu-id="b537d-179">O código para definir o limite de solicitação e o limite de resposta é o mesmo, exceto tendo `RequestBody` ou `Response` nos nomes de propriedade e a interface.</span><span class="sxs-lookup"><span data-stu-id="b537d-179">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span> 

<span data-ttu-id="b537d-180">Aqui está um exemplo que mostra como configurar as taxas de dados mínimo no *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="b537d-180">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=6-9)]

<span data-ttu-id="b537d-181">Você pode configurar as taxas por solicitação no middleware:</span><span class="sxs-lookup"><span data-stu-id="b537d-181">You can configure the rates per request in middleware:</span></span>

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Limits&highlight=5-8)]

<span data-ttu-id="b537d-182">Para obter informações sobre outras opções de Kestrel, consulte as seguintes classes:</span><span class="sxs-lookup"><span data-stu-id="b537d-182">For information about other Kestrel options, see the following classes:</span></span>

* [<span data-ttu-id="b537d-183">KestrelServerOptions</span><span class="sxs-lookup"><span data-stu-id="b537d-183">KestrelServerOptions</span></span>](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs)
* [<span data-ttu-id="b537d-184">KestrelServerLimits</span><span class="sxs-lookup"><span data-stu-id="b537d-184">KestrelServerLimits</span></span>](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs)
* [<span data-ttu-id="b537d-185">ListenOptions</span><span class="sxs-lookup"><span data-stu-id="b537d-185">ListenOptions</span></span>](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b537d-186">ASP.NET Core 1. x</span><span class="sxs-lookup"><span data-stu-id="b537d-186">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="b537d-187">Para obter informações sobre as opções de Kestrel, consulte [KestrelServerOptions classe](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions).</span><span class="sxs-lookup"><span data-stu-id="b537d-187">For information about Kestrel options, see [KestrelServerOptions class](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions).</span></span>

---

### <a name="endpoint-configuration"></a><span data-ttu-id="b537d-188">Configuração de ponto de extremidade</span><span class="sxs-lookup"><span data-stu-id="b537d-188">Endpoint configuration</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b537d-189">ASP.NET Core 2. x</span><span class="sxs-lookup"><span data-stu-id="b537d-189">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="b537d-190">Por padrão o ASP.NET Core associa a `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="b537d-190">By default ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="b537d-191">Configurar portas para Kestrel escutar em chamando e prefixos de URL `Listen` ou `ListenUnixSocket` métodos em `KestrelServerOptions`.</span><span class="sxs-lookup"><span data-stu-id="b537d-191">You configure URL prefixes and ports for Kestrel to listen on by calling `Listen` or `ListenUnixSocket` methods on `KestrelServerOptions`.</span></span> <span data-ttu-id="b537d-192">(`UseUrls`, o `urls` argumento de linha de comando e a variável de ambiente ASPNETCORE_URLS também funcionam mas têm limitações observadas [posteriormente neste artigo](#useurls-limitations).)</span><span class="sxs-lookup"><span data-stu-id="b537d-192">(`UseUrls`, the `urls` command-line argument, and the ASPNETCORE_URLS environment variable also work but have the limitations noted [later in this article](#useurls-limitations).)</span></span>

<span data-ttu-id="b537d-193">**Vincular a um soquete TCP**</span><span class="sxs-lookup"><span data-stu-id="b537d-193">**Bind to a TCP socket**</span></span>

<span data-ttu-id="b537d-194">O `Listen` método vincula a um soquete TCP, e um lambda de opções permite que você configure um certificado SSL:</span><span class="sxs-lookup"><span data-stu-id="b537d-194">The `Listen` method binds to a TCP socket, and an options lambda lets you configure an SSL certificate:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

<span data-ttu-id="b537d-195">Observe como este exemplo configura o SSL para um ponto de extremidade específico usando [ListenOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="b537d-195">Notice how this example configures SSL for a particular endpoint by using [ListenOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs).</span></span> <span data-ttu-id="b537d-196">Você pode usar a mesma API para definir outras configurações de Kestrel para pontos de extremidade específicos.</span><span class="sxs-lookup"><span data-stu-id="b537d-196">You can use the same API to configure other Kestrel settings for particular endpoints.</span></span>

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

<span data-ttu-id="b537d-197">**Vincular a um soquete do Unix**</span><span class="sxs-lookup"><span data-stu-id="b537d-197">**Bind to a Unix socket**</span></span>

<span data-ttu-id="b537d-198">Você pode escutar em um soquete de Unix para melhorar o desempenho com Nginx, conforme mostrado neste exemplo:</span><span class="sxs-lookup"><span data-stu-id="b537d-198">You can listen on a Unix socket for improved performance with Nginx, as shown in this example:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_UnixSocket)]

<span data-ttu-id="b537d-199">**Porta 0**</span><span class="sxs-lookup"><span data-stu-id="b537d-199">**Port 0**</span></span>

<span data-ttu-id="b537d-200">Se você especificar o número de porta 0, Kestrel dinamicamente associa a uma porta disponível.</span><span class="sxs-lookup"><span data-stu-id="b537d-200">If you specify port number 0, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="b537d-201">O exemplo a seguir mostra como determinar qual porta Kestrel realmente associado em tempo de execução:</span><span class="sxs-lookup"><span data-stu-id="b537d-201">The following example shows how to determine which port Kestrel actually bound to at runtime:</span></span>

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Configure&highlight=3,13,16-17)]

<a id="useurls-limitations"></a>

<span data-ttu-id="b537d-202">**Limitações de UseUrls**</span><span class="sxs-lookup"><span data-stu-id="b537d-202">**UseUrls limitations**</span></span>

<span data-ttu-id="b537d-203">Você pode configurar pontos de extremidade chamando o `UseUrls` método ou usando o `urls` argumento de linha de comando ou a variável de ambiente ASPNETCORE_URLS.</span><span class="sxs-lookup"><span data-stu-id="b537d-203">You can configure endpoints by calling the `UseUrls` method or using the `urls` command-line argument or the ASPNETCORE_URLS environment variable.</span></span> <span data-ttu-id="b537d-204">Esses métodos são úteis se você quiser que seu código para trabalhar com servidores que não sejam Kestrel.</span><span class="sxs-lookup"><span data-stu-id="b537d-204">These methods are useful if you want your code to work with servers other than Kestrel.</span></span> <span data-ttu-id="b537d-205">No entanto, você deve estar atento essas limitações:</span><span class="sxs-lookup"><span data-stu-id="b537d-205">However, be aware of these limitations:</span></span>

* <span data-ttu-id="b537d-206">Você não pode usar o SSL com esses métodos.</span><span class="sxs-lookup"><span data-stu-id="b537d-206">You can't use SSL with these methods.</span></span>
* <span data-ttu-id="b537d-207">Se você usar o `Listen` método e `UseUrls`, o `Listen` pontos de extremidade de substituem o `UseUrls` pontos de extremidade.</span><span class="sxs-lookup"><span data-stu-id="b537d-207">If you use both the `Listen` method and `UseUrls`, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

<span data-ttu-id="b537d-208">**Configuração de ponto de extremidade para o IIS**</span><span class="sxs-lookup"><span data-stu-id="b537d-208">**Endpoint configuration for IIS**</span></span>

<span data-ttu-id="b537d-209">Se você usar o IIS, as associações de URL para o IIS substituirão quaisquer associações que você definir chamando `Listen` ou `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="b537d-209">If you use IIS, the URL bindings for IIS override any bindings that you set by calling either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="b537d-210">Para obter mais informações, consulte [Introdução ao ASP.NET Core módulo](aspnet-core-module.md).</span><span class="sxs-lookup"><span data-stu-id="b537d-210">For more information, see [Introduction to ASP.NET Core Module](aspnet-core-module.md).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b537d-211">ASP.NET Core 1. x</span><span class="sxs-lookup"><span data-stu-id="b537d-211">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="b537d-212">Por padrão o ASP.NET Core associa a `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="b537d-212">By default ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="b537d-213">Você pode configurar os prefixos de URL e portas para Kestrel escutar usando o `UseUrls` método de extensão, o `urls` argumento de linha de comando ou o sistema de configuração do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b537d-213">You can configure URL prefixes and ports for Kestrel to listen on by using the `UseUrls` extension method, the `urls` command-line argument, or the ASP.NET Core configuration system.</span></span> <span data-ttu-id="b537d-214">Para obter mais informações sobre esses métodos, consulte [hospedagem](../../fundamentals/hosting.md).</span><span class="sxs-lookup"><span data-stu-id="b537d-214">For more information about these methods, see [Hosting](../../fundamentals/hosting.md).</span></span> <span data-ttu-id="b537d-215">Para obter informações sobre como funciona a associação de URL quando você usar o IIS como um proxy reverso, consulte [ASP.NET Core módulo](aspnet-core-module.md).</span><span class="sxs-lookup"><span data-stu-id="b537d-215">For information about how URL binding works when you use IIS as a reverse proxy, see [ASP.NET Core Module](aspnet-core-module.md).</span></span> 

---

### <a name="url-prefixes"></a><span data-ttu-id="b537d-216">Prefixos de URL</span><span class="sxs-lookup"><span data-stu-id="b537d-216">URL prefixes</span></span>

<span data-ttu-id="b537d-217">Se você chamar `UseUrls` ou use o `urls` argumento de linha de comando ou variável de ambiente ASPNETCORE_URLS, os prefixos de URL podem estar em qualquer um dos formatos a seguir.</span><span class="sxs-lookup"><span data-stu-id="b537d-217">If you call `UseUrls` or use the `urls` command-line argument or ASPNETCORE_URLS environment variable, the URL prefixes can be in any of the following formats.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b537d-218">ASP.NET Core 2. x</span><span class="sxs-lookup"><span data-stu-id="b537d-218">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="b537d-219">Somente prefixos de URL HTTP são válidos. Kestrel não dá suporte a SSL quando você configurar as ligações de URL usando `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="b537d-219">Only HTTP URL prefixes are valid; Kestrel does not support SSL when you configure URL bindings by using `UseUrls`.</span></span>

* <span data-ttu-id="b537d-220">Endereço IPv4 com o número da porta</span><span class="sxs-lookup"><span data-stu-id="b537d-220">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="b537d-221">0.0.0.0 é um caso especial que associa a todos os endereços IPv4.</span><span class="sxs-lookup"><span data-stu-id="b537d-221">0.0.0.0 is a special case that binds to all IPv4 addresses.</span></span>


* <span data-ttu-id="b537d-222">Endereço IPv6 com número de porta</span><span class="sxs-lookup"><span data-stu-id="b537d-222">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/ 
  ```

  <span data-ttu-id="b537d-223">[:] é o equivalente de IPv6 do IPv4 0.0.0.0.</span><span class="sxs-lookup"><span data-stu-id="b537d-223">[::] is the IPv6 equivalent of IPv4 0.0.0.0.</span></span>


* <span data-ttu-id="b537d-224">Nome de host com o número da porta</span><span class="sxs-lookup"><span data-stu-id="b537d-224">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="b537d-225">Nomes de host *, e +, não são especiais.</span><span class="sxs-lookup"><span data-stu-id="b537d-225">Host names, *, and +, are not special.</span></span> <span data-ttu-id="b537d-226">Tudo o que não é um endereço IP ou o "localhost" reconhecido associará a todos os IPs de IPv6 e IPv4.</span><span class="sxs-lookup"><span data-stu-id="b537d-226">Anything that is not a recognized IP address or "localhost" will bind to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="b537d-227">Se você precisa associar nomes de host diferentes para diferentes aplicativos ASP.NET Core na mesma porta, use [HTTP.sys](httpsys.md) ou um servidor proxy reverso, como IIS, Nginx ou Apache.</span><span class="sxs-lookup"><span data-stu-id="b537d-227">If you need to bind different host names to different ASP.NET Core applications on the same port, use [HTTP.sys](httpsys.md) or a reverse proxy server such as IIS, Nginx, or Apache.</span></span>

* <span data-ttu-id="b537d-228">Nome do "Localhost" com o IP de loopback ou de número de porta com o número da porta</span><span class="sxs-lookup"><span data-stu-id="b537d-228">"Localhost" name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="b537d-229">Quando `localhost` for especificado, Kestrel tenta associar a interfaces de loopback de IPv4 e IPv6.</span><span class="sxs-lookup"><span data-stu-id="b537d-229">When `localhost` is specified, Kestrel tries to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="b537d-230">Se a porta solicitada está em uso por outro serviço em cada interface de loopback, Kestrel falhar ao iniciar.</span><span class="sxs-lookup"><span data-stu-id="b537d-230">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="b537d-231">Se qualquer uma das interfaces de loopback não estiver disponível por qualquer outro motivo (mais comumente porque não há suporte para IPv6), Kestrel registra um aviso.</span><span class="sxs-lookup"><span data-stu-id="b537d-231">If either loopback interface is unavailable for any other reason (most commonly because IPv6 is not supported), Kestrel logs a warning.</span></span> 

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b537d-232">ASP.NET Core 1. x</span><span class="sxs-lookup"><span data-stu-id="b537d-232">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

* <span data-ttu-id="b537d-233">Endereço IPv4 com o número da porta</span><span class="sxs-lookup"><span data-stu-id="b537d-233">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  https://65.55.39.10:443/
  ```

  <span data-ttu-id="b537d-234">0.0.0.0 é um caso especial que associa a todos os endereços IPv4.</span><span class="sxs-lookup"><span data-stu-id="b537d-234">0.0.0.0 is a special case that binds to all IPv4 addresses.</span></span>


* <span data-ttu-id="b537d-235">Endereço IPv6 com número de porta</span><span class="sxs-lookup"><span data-stu-id="b537d-235">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/ 
  https://[0:0:0:0:0:ffff:4137:270a]:443/ 
  ```

  <span data-ttu-id="b537d-236">[:] é o equivalente de IPv6 do IPv4 0.0.0.0.</span><span class="sxs-lookup"><span data-stu-id="b537d-236">[::] is the IPv6 equivalent of IPv4 0.0.0.0.</span></span>


* <span data-ttu-id="b537d-237">Nome de host com o número da porta</span><span class="sxs-lookup"><span data-stu-id="b537d-237">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  https://contoso.com:443/
  https://*:443/
  ```

  <span data-ttu-id="b537d-238">Nomes de host \*, e + não são especiais.</span><span class="sxs-lookup"><span data-stu-id="b537d-238">Host names, \*, and + aren't special.</span></span> <span data-ttu-id="b537d-239">Tudo o que não é um endereço IP ou o "localhost" reconhecido associa a todos os IPs de IPv6 e IPv4.</span><span class="sxs-lookup"><span data-stu-id="b537d-239">Anything that isn't a recognized IP address or "localhost" binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="b537d-240">Se você precisa associar nomes de host diferentes para diferentes aplicativos ASP.NET Core na mesma porta, use [WebListener](weblistener.md) ou um servidor proxy reverso, como IIS, Nginx ou Apache.</span><span class="sxs-lookup"><span data-stu-id="b537d-240">If you need to bind different host names to different ASP.NET Core applications on the same port, use [WebListener](weblistener.md) or a reverse proxy server such as IIS, Nginx, or Apache.</span></span>

* <span data-ttu-id="b537d-241">Nome do "Localhost" com o IP de loopback ou de número de porta com o número da porta</span><span class="sxs-lookup"><span data-stu-id="b537d-241">"Localhost" name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="b537d-242">Quando `localhost` for especificado, Kestrel tenta associar a interfaces de loopback de IPv4 e IPv6.</span><span class="sxs-lookup"><span data-stu-id="b537d-242">When `localhost` is specified, Kestrel tries to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="b537d-243">Se a porta solicitada está em uso por outro serviço em cada interface de loopback, Kestrel falhar ao iniciar.</span><span class="sxs-lookup"><span data-stu-id="b537d-243">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="b537d-244">Se qualquer uma das interfaces de loopback não estiver disponível por qualquer outro motivo (mais comumente porque não há suporte para IPv6), Kestrel registra um aviso.</span><span class="sxs-lookup"><span data-stu-id="b537d-244">If either loopback interface is unavailable for any other reason (most commonly because IPv6 is not supported), Kestrel logs a warning.</span></span> 

* <span data-ttu-id="b537d-245">Soquete de UNIX</span><span class="sxs-lookup"><span data-stu-id="b537d-245">Unix socket</span></span>

  ```
  http://unix:/run/dan-live.sock  
  ```

<span data-ttu-id="b537d-246">**Porta 0**</span><span class="sxs-lookup"><span data-stu-id="b537d-246">**Port 0**</span></span>

<span data-ttu-id="b537d-247">Se você especificar o número de porta 0, Kestrel dinamicamente associa a uma porta disponível.</span><span class="sxs-lookup"><span data-stu-id="b537d-247">If you specify port number 0, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="b537d-248">Associando a porta 0 é permitido para qualquer nome de host ou IP exceto `localhost` nome.</span><span class="sxs-lookup"><span data-stu-id="b537d-248">Binding to port 0 is allowed for any host name or IP except for `localhost` name.</span></span>

<span data-ttu-id="b537d-249">O exemplo a seguir mostra como determinar qual porta Kestrel realmente associado em tempo de execução:</span><span class="sxs-lookup"><span data-stu-id="b537d-249">The following example shows how to determine which port Kestrel actually bound to at runtime:</span></span>

[!code-csharp[](kestrel/sample1/Startup.cs?name=snippet_Configure)]

<span data-ttu-id="b537d-250">**Prefixos de URL para SSL**</span><span class="sxs-lookup"><span data-stu-id="b537d-250">**URL prefixes for SSL**</span></span>

<span data-ttu-id="b537d-251">Certifique-se de incluir os prefixos de URL com `https:` se você chamar o `UseHttps` método de extensão, conforme mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="b537d-251">Be sure to include URL prefixes with `https:` if you call the `UseHttps` extension method, as shown below.</span></span>

```csharp
var host = new WebHostBuilder() 
    .UseKestrel(options => 
    { 
        options.UseHttps("testCert.pfx", "testPassword"); 
    }) 
   .UseUrls("http://localhost:5000", "https://localhost:5001") 
   .UseContentRoot(Directory.GetCurrentDirectory()) 
   .UseStartup<Startup>() 
   .Build(); 
```

> [!NOTE]
> <span data-ttu-id="b537d-252">HTTPS e HTTP não podem ser hospedado na mesma porta.</span><span class="sxs-lookup"><span data-stu-id="b537d-252">HTTPS and HTTP cannot be hosted on the same port.</span></span>

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

---
## <a name="next-steps"></a><span data-ttu-id="b537d-253">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="b537d-253">Next steps</span></span>

<span data-ttu-id="b537d-254">Para obter mais informações, consulte os seguintes recursos:</span><span class="sxs-lookup"><span data-stu-id="b537d-254">For more information, see the following resources:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b537d-255">ASP.NET Core 2. x</span><span class="sxs-lookup"><span data-stu-id="b537d-255">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

* [<span data-ttu-id="b537d-256">Aplicativo de exemplo para 2. x</span><span class="sxs-lookup"><span data-stu-id="b537d-256">Sample app for 2.x</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2)
* [<span data-ttu-id="b537d-257">Código-fonte kestrel</span><span class="sxs-lookup"><span data-stu-id="b537d-257">Kestrel source code</span></span>](https://github.com/aspnet/KestrelHttpServer)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b537d-258">ASP.NET Core 1. x</span><span class="sxs-lookup"><span data-stu-id="b537d-258">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

* [<span data-ttu-id="b537d-259">Aplicativo de exemplo para 1. x</span><span class="sxs-lookup"><span data-stu-id="b537d-259">Sample app for 1.x</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1)
* [<span data-ttu-id="b537d-260">Código-fonte kestrel</span><span class="sxs-lookup"><span data-stu-id="b537d-260">Kestrel source code</span></span>](https://github.com/aspnet/KestrelHttpServer)

---

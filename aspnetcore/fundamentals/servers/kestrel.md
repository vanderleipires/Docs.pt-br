---
title: "Implementação do servidor web kestrel no núcleo do ASP.NET"
author: tdykstra
description: Apresenta Kestrel, o servidor web de plataforma cruzada para o ASP.NET Core com base em libuv.
ms.author: tdykstra
manager: wpickett
ms.date: 08/02/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/kestrel
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3e2b28f15e47789ac89213e57396060ee356ee33
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
# <a name="introduction-to-kestrel-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="efb0f-103">Introdução a implementação do servidor web Kestrel no núcleo do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="efb0f-103">Introduction to Kestrel web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="efb0f-104">Por [Tom Dykstra](https://github.com/tdykstra), [Ross Carlos](https://github.com/Tratcher), e [Stephen Halter](https://twitter.com/halter73)</span><span class="sxs-lookup"><span data-stu-id="efb0f-104">By [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), and [Stephen Halter](https://twitter.com/halter73)</span></span>

<span data-ttu-id="efb0f-105">Kestrel é uma plataforma cruzada [servidor web para o ASP.NET Core](index.md) com base em [libuv](https://github.com/libuv/libuv), uma biblioteca de e/s assíncrona de plataforma cruzada.</span><span class="sxs-lookup"><span data-stu-id="efb0f-105">Kestrel is a cross-platform [web server for ASP.NET Core](index.md) based on [libuv](https://github.com/libuv/libuv), a cross-platform asynchronous I/O library.</span></span> <span data-ttu-id="efb0f-106">Kestrel é o servidor web que está incluído por padrão nos modelos de projeto do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="efb0f-106">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span> 

<span data-ttu-id="efb0f-107">Kestrel suporta os seguintes recursos:</span><span class="sxs-lookup"><span data-stu-id="efb0f-107">Kestrel supports the following features:</span></span>

  * <span data-ttu-id="efb0f-108">HTTPS</span><span class="sxs-lookup"><span data-stu-id="efb0f-108">HTTPS</span></span>
  * <span data-ttu-id="efb0f-109">Atualização opaca usada para habilitar [WebSockets](https://github.com/aspnet/websockets)</span><span class="sxs-lookup"><span data-stu-id="efb0f-109">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
  * <span data-ttu-id="efb0f-110">Soquetes de UNIX de alto desempenho atrás Nginx</span><span class="sxs-lookup"><span data-stu-id="efb0f-110">Unix sockets for high performance behind Nginx</span></span> 

<span data-ttu-id="efb0f-111">Kestrel tem suporte em todas as plataformas e versões que dá suporte ao .NET Core.</span><span class="sxs-lookup"><span data-stu-id="efb0f-111">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="efb0f-112">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="efb0f-112">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="efb0f-113">[Exibir ou baixar o código de exemplo para 2. x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="efb0f-113">[View or download sample code for 2.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="efb0f-114">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="efb0f-114">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="efb0f-115">[Exibir ou baixar o exemplo de código 1. x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="efb0f-115">[View or download sample code for 1.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

---

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="efb0f-116">Quando usar Kestrel com um proxy reverso</span><span class="sxs-lookup"><span data-stu-id="efb0f-116">When to use Kestrel with a reverse proxy</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="efb0f-117">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="efb0f-117">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="efb0f-118">Você pode usar o Kestrel sozinho ou com um *servidor proxy reverso* como IIS, Nginx ou Apache.</span><span class="sxs-lookup"><span data-stu-id="efb0f-118">You can use Kestrel by itself or with a *reverse proxy server*, such as IIS, Nginx, or Apache.</span></span> <span data-ttu-id="efb0f-119">Um servidor proxy reverso recebe solicitações HTTP da Internet e as encaminha para o Kestrel após algum tratamento preliminar.</span><span class="sxs-lookup"><span data-stu-id="efb0f-119">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![O Kestrel se comunica diretamente com a Internet, sem um servidor proxy reverso](kestrel/_static/kestrel-to-internet2.png)

![O Kestrel se comunica indiretamente com a Internet através de um servidor proxy reverso, tal como o IIS, o Nginx ou o Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="efb0f-122">Qualquer configuração &mdash; com ou sem um servidor proxy reverso &mdash; também pode ser usada se o Kestrel é exposto somente a uma rede interna.</span><span class="sxs-lookup"><span data-stu-id="efb0f-122">Either configuration &mdash; with or without a reverse proxy server &mdash; can also be used if Kestrel is exposed only to an internal network.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="efb0f-123">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="efb0f-123">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="efb0f-124">Se seu aplicativo aceita solicitações somente de uma rede interna, você pode usar o Kestrel sozinho.</span><span class="sxs-lookup"><span data-stu-id="efb0f-124">If your application accepts requests only from an internal network, you can use Kestrel by itself.</span></span>

![O Kestrel se comunica diretamente com a rede interna](kestrel/_static/kestrel-to-internal.png)

<span data-ttu-id="efb0f-126">Se você expuser seu aplicativo à Internet, você deverá usar o IIS, o Nginx ou o Apache como um *servidor proxy reverso*.</span><span class="sxs-lookup"><span data-stu-id="efb0f-126">If you expose your application to the Internet, you must use IIS, Nginx, or Apache as a *reverse proxy server*.</span></span> <span data-ttu-id="efb0f-127">Um servidor proxy reverso recebe solicitações HTTP da Internet e as encaminha para o Kestrel após algum tratamento preliminar.</span><span class="sxs-lookup"><span data-stu-id="efb0f-127">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![O Kestrel se comunica indiretamente com a Internet através de um servidor proxy reverso, tal como o IIS, o Nginx ou o Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="efb0f-129">Um proxy reverso é necessário para implantações de borda (expostas ao tráfego da Internet) por motivos de segurança.</span><span class="sxs-lookup"><span data-stu-id="efb0f-129">A reverse proxy is required for edge deployments (exposed to traffic from the Internet) for security reasons.</span></span> <span data-ttu-id="efb0f-130">As versões 1.x do Kestrel não têm um conjunto completo de proteção contra ataques.</span><span class="sxs-lookup"><span data-stu-id="efb0f-130">The 1.x versions of Kestrel don't have a full complement of defenses against attacks.</span></span> <span data-ttu-id="efb0f-131">Isso inclui, mas não está limitado a tempos limite apropriado, limites de tamanho e os limites de conexão simultâneas.</span><span class="sxs-lookup"><span data-stu-id="efb0f-131">This includes but isn't limited to appropriate timeouts, size limits, and concurrent connection limits.</span></span>

---

<span data-ttu-id="efb0f-132">Um cenário que requer um proxy reverso é quando você tiver vários aplicativos que compartilham o mesmo IP e porta em execução em um único servidor.</span><span class="sxs-lookup"><span data-stu-id="efb0f-132">A scenario that requires a reverse proxy is when you have multiple applications that share the same IP and port running on a single server.</span></span> <span data-ttu-id="efb0f-133">Isso não funcionar com Kestrel diretamente como Kestrel não dá suporte a compartilhar o mesmo IP e porta entre vários processos.</span><span class="sxs-lookup"><span data-stu-id="efb0f-133">That doesn't work with Kestrel directly because Kestrel doesn't support sharing the same IP and port between multiple processes.</span></span> <span data-ttu-id="efb0f-134">Quando você configura Kestrel para escutar em uma porta, ele trata todo o tráfego para essa porta, independentemente de cabeçalho de host.</span><span class="sxs-lookup"><span data-stu-id="efb0f-134">When you configure Kestrel to listen on a port, it handles all traffic for that port regardless of host header.</span></span> <span data-ttu-id="efb0f-135">Um proxy reverso que pode compartilhar portas, em seguida, encaminhe para Kestrel em um único IP e porta.</span><span class="sxs-lookup"><span data-stu-id="efb0f-135">A reverse proxy that can share ports must then forward to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="efb0f-136">Mesmo se um servidor proxy reverso não é necessário, usando um pode ser uma boa opção por outros motivos:</span><span class="sxs-lookup"><span data-stu-id="efb0f-136">Even if a reverse proxy server isn't required, using one might be a good choice for other reasons:</span></span>

* <span data-ttu-id="efb0f-137">Ele pode limitar a área da superfície exposta.</span><span class="sxs-lookup"><span data-stu-id="efb0f-137">It can limit your exposed surface area.</span></span>
* <span data-ttu-id="efb0f-138">Ele fornece uma camada de configuração e proteção adicional opcional.</span><span class="sxs-lookup"><span data-stu-id="efb0f-138">It provides an optional additional layer of configuration and defense.</span></span>
* <span data-ttu-id="efb0f-139">Ele pode integrar melhor com a infraestrutura existente.</span><span class="sxs-lookup"><span data-stu-id="efb0f-139">It might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="efb0f-140">Ele simplifica a configuração SSL e balanceamento de carga.</span><span class="sxs-lookup"><span data-stu-id="efb0f-140">It simplifies load balancing and SSL set-up.</span></span> <span data-ttu-id="efb0f-141">Somente o servidor de proxy reverso requer um certificado SSL, e esse servidor pode se comunicar com seus servidores de aplicativos na rede interna usando HTTP simples.</span><span class="sxs-lookup"><span data-stu-id="efb0f-141">Only your reverse proxy server requires an SSL certificate, and that server can communicate with your application servers on the internal network using plain HTTP.</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="efb0f-142">Como usar Kestrel em aplicativos do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="efb0f-142">How to use Kestrel in ASP.NET Core apps</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="efb0f-143">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="efb0f-143">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="efb0f-144">O [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) pacote está incluído no [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="efb0f-144">The [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) package is included in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

<span data-ttu-id="efb0f-145">Modelos de projeto do ASP.NET Core usam Kestrel por padrão.</span><span class="sxs-lookup"><span data-stu-id="efb0f-145">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="efb0f-146">Em *Program.cs*, as chamadas de código de modelo `CreateDefaultBuilder`, que chama [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) em segundo plano.</span><span class="sxs-lookup"><span data-stu-id="efb0f-146">In *Program.cs*, the template code calls `CreateDefaultBuilder`, which calls [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) behind the scenes.</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

<span data-ttu-id="efb0f-147">Se você precisar configurar as opções de Kestrel, chame `UseKestrel` na *Program.cs* conforme mostrado no exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="efb0f-147">If you need to configure Kestrel options, call `UseKestrel` in *Program.cs* as shown in the following example:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="efb0f-148">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="efb0f-148">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="efb0f-149">Instalar o [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) pacote NuGet.</span><span class="sxs-lookup"><span data-stu-id="efb0f-149">Install the [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) NuGet package.</span></span>

<span data-ttu-id="efb0f-150">Chamar o [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) método de extensão no `WebHostBuilder` no seu `Main` método, especificando um [opções Kestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions) que você precisa, como mostrado na próxima seção.</span><span class="sxs-lookup"><span data-stu-id="efb0f-150">Call the [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) extension method on `WebHostBuilder` in your `Main` method, specifying any [Kestrel options](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions) that you need, as shown in the next section.</span></span>

[!code-csharp[](kestrel/sample1/Program.cs?name=snippet_Main&highlight=13-19)]

---

### <a name="kestrel-options"></a><span data-ttu-id="efb0f-151">Opções de kestrel</span><span class="sxs-lookup"><span data-stu-id="efb0f-151">Kestrel options</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="efb0f-152">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="efb0f-152">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="efb0f-153">O servidor de web Kestrel tem opções de configuração de restrição são especialmente úteis em implantações para a Internet.</span><span class="sxs-lookup"><span data-stu-id="efb0f-153">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span> <span data-ttu-id="efb0f-154">Aqui estão alguns dos limites que você pode definir:</span><span class="sxs-lookup"><span data-stu-id="efb0f-154">Here are some of the limits you can set:</span></span>

- <span data-ttu-id="efb0f-155">Número máximo de conexões de cliente</span><span class="sxs-lookup"><span data-stu-id="efb0f-155">Maximum client connections</span></span>
- <span data-ttu-id="efb0f-156">Tamanho máximo do corpo da solicitação</span><span class="sxs-lookup"><span data-stu-id="efb0f-156">Maximum request body size</span></span>
- <span data-ttu-id="efb0f-157">Taxa de dados mínima do corpo da solicitação</span><span class="sxs-lookup"><span data-stu-id="efb0f-157">Minimum request body data rate</span></span>

<span data-ttu-id="efb0f-158">Você define essas restrições e outras o `Limits` propriedade o [KestrelServerOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs) classe.</span><span class="sxs-lookup"><span data-stu-id="efb0f-158">You set these constraints and others in the `Limits` property of the [KestrelServerOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs) class.</span></span> <span data-ttu-id="efb0f-159">O `Limits` propriedade contém uma instância do [KestrelServerLimits](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs) classe.</span><span class="sxs-lookup"><span data-stu-id="efb0f-159">The `Limits` property holds an instance of the [KestrelServerLimits](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs) class.</span></span> 

<span data-ttu-id="efb0f-160">**Conexões de cliente máximo**</span><span class="sxs-lookup"><span data-stu-id="efb0f-160">**Maximum client connections**</span></span>

<span data-ttu-id="efb0f-161">O número máximo de conexões TCP abertas simultâneas pode ser definido para o aplicativo inteiro com o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="efb0f-161">The maximum number of concurrent open TCP connections can be set for the entire application with the following code:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="efb0f-162">Há um limite separado para conexões que foram atualizados de HTTP ou HTTPS para outro protocolo (por exemplo, em uma solicitação WebSocket).</span><span class="sxs-lookup"><span data-stu-id="efb0f-162">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="efb0f-163">Depois que uma conexão é atualizado, ele não é contado em relação a `MaxConcurrentConnections` limite.</span><span class="sxs-lookup"><span data-stu-id="efb0f-163">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span> 

<span data-ttu-id="efb0f-164">O número máximo de conexões é ilimitado (null) por padrão.</span><span class="sxs-lookup"><span data-stu-id="efb0f-164">The maximum number of connections is unlimited (null) by default.</span></span>

<span data-ttu-id="efb0f-165">**Tamanho do corpo da solicitação máxima**</span><span class="sxs-lookup"><span data-stu-id="efb0f-165">**Maximum request body size**</span></span>

<span data-ttu-id="efb0f-166">O tamanho do corpo da solicitação máximo padrão é 30.000.000 bytes, que é aproximadamente 28.6 MB.</span><span class="sxs-lookup"><span data-stu-id="efb0f-166">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6MB.</span></span> 

<span data-ttu-id="efb0f-167">A maneira recomendada para substituir o limite em um aplicativo ASP.NET MVC de núcleo é usar o [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) atributo em um método de ação:</span><span class="sxs-lookup"><span data-stu-id="efb0f-167">The recommended way to override the limit in an ASP.NET Core MVC app is to use the [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="efb0f-168">Aqui está um exemplo que mostra como configurar o limite para o aplicativo inteiro, cada solicitação:</span><span class="sxs-lookup"><span data-stu-id="efb0f-168">Here's an example that shows how to configure the constraint for the entire application, every request:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=5)]

<span data-ttu-id="efb0f-169">Você pode substituir a configuração em uma solicitação específica no middleware:</span><span class="sxs-lookup"><span data-stu-id="efb0f-169">You can override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Limits&highlight=3-4)]
 
<span data-ttu-id="efb0f-170">Uma exceção é gerada se você tentar configurar o limite de uma solicitação depois que o aplicativo começou a ler a solicitação.</span><span class="sxs-lookup"><span data-stu-id="efb0f-170">An exception is thrown if you try to configure the limit on a request after the application has started reading the request.</span></span> <span data-ttu-id="efb0f-171">Há um `IsReadOnly` propriedade que indica se o `MaxRequestBodySize` propriedade está no estado somente leitura, que significa que é muito tarde para configurar o limite.</span><span class="sxs-lookup"><span data-stu-id="efb0f-171">There's an `IsReadOnly` property that tells you if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="efb0f-172">**Taxa de dados de corpo de solicitação mínimo**</span><span class="sxs-lookup"><span data-stu-id="efb0f-172">**Minimum request body data rate**</span></span>

<span data-ttu-id="efb0f-173">Kestrel verifica a cada segundo se dados estão chegando a taxa especificada em bytes/segundo.</span><span class="sxs-lookup"><span data-stu-id="efb0f-173">Kestrel checks every second if data is coming in at the specified rate in bytes/second.</span></span> <span data-ttu-id="efb0f-174">Se a taxa de cair abaixo do mínimo, a conexão é atingiu o tempo limite. O período de cortesia é a quantidade de tempo que Kestrel dá ao cliente para aumentar sua taxa de envio até o mínimo; a taxa não é verificada durante esse período.</span><span class="sxs-lookup"><span data-stu-id="efb0f-174">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="efb0f-175">O período de cortesia ajuda a evitar a remoção de conexões que inicialmente estão enviando dados em uma taxa baixa devido a partida lenta do TCP.</span><span class="sxs-lookup"><span data-stu-id="efb0f-175">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="efb0f-176">A taxa mínima de padrão é 240 bytes/segundo, com um período de cortesia de 5 segundos.</span><span class="sxs-lookup"><span data-stu-id="efb0f-176">The default minimum rate is 240 bytes/second, with a 5 second grace period.</span></span>

<span data-ttu-id="efb0f-177">Uma taxa mínima também se aplica à resposta.</span><span class="sxs-lookup"><span data-stu-id="efb0f-177">A minimum rate also applies to the response.</span></span> <span data-ttu-id="efb0f-178">O código para definir o limite de solicitação e o limite de resposta é o mesmo, exceto tendo `RequestBody` ou `Response` nos nomes de propriedade e a interface.</span><span class="sxs-lookup"><span data-stu-id="efb0f-178">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span> 

<span data-ttu-id="efb0f-179">Aqui está um exemplo que mostra como configurar as taxas de dados mínimo no *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="efb0f-179">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=6-9)]

<span data-ttu-id="efb0f-180">Você pode configurar as taxas por solicitação no middleware:</span><span class="sxs-lookup"><span data-stu-id="efb0f-180">You can configure the rates per request in middleware:</span></span>

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Limits&highlight=5-8)]

<span data-ttu-id="efb0f-181">Para obter informações sobre outras opções de Kestrel, consulte as seguintes classes:</span><span class="sxs-lookup"><span data-stu-id="efb0f-181">For information about other Kestrel options, see the following classes:</span></span>

* [<span data-ttu-id="efb0f-182">KestrelServerOptions</span><span class="sxs-lookup"><span data-stu-id="efb0f-182">KestrelServerOptions</span></span>](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs)
* [<span data-ttu-id="efb0f-183">KestrelServerLimits</span><span class="sxs-lookup"><span data-stu-id="efb0f-183">KestrelServerLimits</span></span>](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs)
* [<span data-ttu-id="efb0f-184">ListenOptions</span><span class="sxs-lookup"><span data-stu-id="efb0f-184">ListenOptions</span></span>](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="efb0f-185">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="efb0f-185">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="efb0f-186">Para obter informações sobre as opções de Kestrel, consulte [KestrelServerOptions classe](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions).</span><span class="sxs-lookup"><span data-stu-id="efb0f-186">For information about Kestrel options, see [KestrelServerOptions class](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions).</span></span>

---

### <a name="endpoint-configuration"></a><span data-ttu-id="efb0f-187">Configuração de ponto de extremidade</span><span class="sxs-lookup"><span data-stu-id="efb0f-187">Endpoint configuration</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="efb0f-188">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="efb0f-188">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="efb0f-189">Por padrão o ASP.NET Core associa a `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="efb0f-189">By default ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="efb0f-190">Configurar portas para Kestrel escutar em chamando e prefixos de URL `Listen` ou `ListenUnixSocket` métodos em `KestrelServerOptions`.</span><span class="sxs-lookup"><span data-stu-id="efb0f-190">You configure URL prefixes and ports for Kestrel to listen on by calling `Listen` or `ListenUnixSocket` methods on `KestrelServerOptions`.</span></span> <span data-ttu-id="efb0f-191">(`UseUrls`, o `urls` argumento de linha de comando e a variável de ambiente ASPNETCORE_URLS também funcionam mas têm limitações observadas [posteriormente neste artigo](#useurls-limitations).)</span><span class="sxs-lookup"><span data-stu-id="efb0f-191">(`UseUrls`, the `urls` command-line argument, and the ASPNETCORE_URLS environment variable also work but have the limitations noted [later in this article](#useurls-limitations).)</span></span>

<span data-ttu-id="efb0f-192">**Vincular a um soquete TCP**</span><span class="sxs-lookup"><span data-stu-id="efb0f-192">**Bind to a TCP socket**</span></span>

<span data-ttu-id="efb0f-193">O `Listen` método vincula a um soquete TCP, e um lambda de opções permite que você configure um certificado SSL:</span><span class="sxs-lookup"><span data-stu-id="efb0f-193">The `Listen` method binds to a TCP socket, and an options lambda lets you configure an SSL certificate:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

<span data-ttu-id="efb0f-194">Observe como este exemplo configura o SSL para um ponto de extremidade específico usando [ListenOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="efb0f-194">Notice how this example configures SSL for a particular endpoint by using [ListenOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs).</span></span> <span data-ttu-id="efb0f-195">Você pode usar a mesma API para definir outras configurações de Kestrel para pontos de extremidade específicos.</span><span class="sxs-lookup"><span data-stu-id="efb0f-195">You can use the same API to configure other Kestrel settings for particular endpoints.</span></span>

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

<span data-ttu-id="efb0f-196">**Vincular a um soquete do Unix**</span><span class="sxs-lookup"><span data-stu-id="efb0f-196">**Bind to a Unix socket**</span></span>

<span data-ttu-id="efb0f-197">Você pode escutar em um soquete de Unix para melhorar o desempenho com Nginx, conforme mostrado neste exemplo:</span><span class="sxs-lookup"><span data-stu-id="efb0f-197">You can listen on a Unix socket for improved performance with Nginx, as shown in this example:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_UnixSocket)]

<span data-ttu-id="efb0f-198">**Porta 0**</span><span class="sxs-lookup"><span data-stu-id="efb0f-198">**Port 0**</span></span>

<span data-ttu-id="efb0f-199">Se você especificar o número de porta 0, Kestrel dinamicamente associa a uma porta disponível.</span><span class="sxs-lookup"><span data-stu-id="efb0f-199">If you specify port number 0, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="efb0f-200">O exemplo a seguir mostra como determinar qual porta Kestrel realmente associado em tempo de execução:</span><span class="sxs-lookup"><span data-stu-id="efb0f-200">The following example shows how to determine which port Kestrel actually bound to at runtime:</span></span>

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Configure&highlight=3,13,16-17)]

<a id="useurls-limitations"></a>

<span data-ttu-id="efb0f-201">**Limitações de UseUrls**</span><span class="sxs-lookup"><span data-stu-id="efb0f-201">**UseUrls limitations**</span></span>

<span data-ttu-id="efb0f-202">Você pode configurar pontos de extremidade chamando o `UseUrls` método ou usando o `urls` argumento de linha de comando ou a variável de ambiente ASPNETCORE_URLS.</span><span class="sxs-lookup"><span data-stu-id="efb0f-202">You can configure endpoints by calling the `UseUrls` method or using the `urls` command-line argument or the ASPNETCORE_URLS environment variable.</span></span> <span data-ttu-id="efb0f-203">Esses métodos são úteis se você quiser que seu código para trabalhar com servidores que não sejam Kestrel.</span><span class="sxs-lookup"><span data-stu-id="efb0f-203">These methods are useful if you want your code to work with servers other than Kestrel.</span></span> <span data-ttu-id="efb0f-204">No entanto, você deve estar atento essas limitações:</span><span class="sxs-lookup"><span data-stu-id="efb0f-204">However, be aware of these limitations:</span></span>

* <span data-ttu-id="efb0f-205">Você não pode usar o SSL com esses métodos.</span><span class="sxs-lookup"><span data-stu-id="efb0f-205">You can't use SSL with these methods.</span></span>
* <span data-ttu-id="efb0f-206">Se você usar o `Listen` método e `UseUrls`, o `Listen` pontos de extremidade de substituem o `UseUrls` pontos de extremidade.</span><span class="sxs-lookup"><span data-stu-id="efb0f-206">If you use both the `Listen` method and `UseUrls`, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

<span data-ttu-id="efb0f-207">**Configuração de ponto de extremidade para o IIS**</span><span class="sxs-lookup"><span data-stu-id="efb0f-207">**Endpoint configuration for IIS**</span></span>

<span data-ttu-id="efb0f-208">Se você usar o IIS, as associações de URL para o IIS substituirão quaisquer associações que você definir chamando `Listen` ou `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="efb0f-208">If you use IIS, the URL bindings for IIS override any bindings that you set by calling either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="efb0f-209">Para obter mais informações, consulte [Introdução ao ASP.NET Core módulo](aspnet-core-module.md).</span><span class="sxs-lookup"><span data-stu-id="efb0f-209">For more information, see [Introduction to ASP.NET Core Module](aspnet-core-module.md).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="efb0f-210">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="efb0f-210">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="efb0f-211">Por padrão o ASP.NET Core associa a `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="efb0f-211">By default ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="efb0f-212">Você pode configurar os prefixos de URL e portas para Kestrel escutar usando o `UseUrls` método de extensão, o `urls` argumento de linha de comando ou o sistema de configuração do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="efb0f-212">You can configure URL prefixes and ports for Kestrel to listen on by using the `UseUrls` extension method, the `urls` command-line argument, or the ASP.NET Core configuration system.</span></span> <span data-ttu-id="efb0f-213">Para obter mais informações sobre esses métodos, consulte [hospedagem](../../fundamentals/hosting.md).</span><span class="sxs-lookup"><span data-stu-id="efb0f-213">For more information about these methods, see [Hosting](../../fundamentals/hosting.md).</span></span> <span data-ttu-id="efb0f-214">Para obter informações sobre como funciona a associação de URL quando você usar o IIS como um proxy reverso, consulte [ASP.NET Core módulo](aspnet-core-module.md).</span><span class="sxs-lookup"><span data-stu-id="efb0f-214">For information about how URL binding works when you use IIS as a reverse proxy, see [ASP.NET Core Module](aspnet-core-module.md).</span></span> 

---

### <a name="url-prefixes"></a><span data-ttu-id="efb0f-215">Prefixos de URL</span><span class="sxs-lookup"><span data-stu-id="efb0f-215">URL prefixes</span></span>

<span data-ttu-id="efb0f-216">Se você chamar `UseUrls` ou use o `urls` argumento de linha de comando ou variável de ambiente ASPNETCORE_URLS, os prefixos de URL podem estar em qualquer um dos formatos a seguir.</span><span class="sxs-lookup"><span data-stu-id="efb0f-216">If you call `UseUrls` or use the `urls` command-line argument or ASPNETCORE_URLS environment variable, the URL prefixes can be in any of the following formats.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="efb0f-217">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="efb0f-217">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="efb0f-218">Somente prefixos de URL HTTP são válidos. Kestrel não dá suporte a SSL quando você configurar as ligações de URL usando `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="efb0f-218">Only HTTP URL prefixes are valid; Kestrel doesn't support SSL when you configure URL bindings by using `UseUrls`.</span></span>

* <span data-ttu-id="efb0f-219">Endereço IPv4 com o número da porta</span><span class="sxs-lookup"><span data-stu-id="efb0f-219">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="efb0f-220">0.0.0.0 é um caso especial que associa a todos os endereços IPv4.</span><span class="sxs-lookup"><span data-stu-id="efb0f-220">0.0.0.0 is a special case that binds to all IPv4 addresses.</span></span>


* <span data-ttu-id="efb0f-221">Endereço IPv6 com número de porta</span><span class="sxs-lookup"><span data-stu-id="efb0f-221">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/ 
  ```

  <span data-ttu-id="efb0f-222">[:] é o equivalente de IPv6 do IPv4 0.0.0.0.</span><span class="sxs-lookup"><span data-stu-id="efb0f-222">[::] is the IPv6 equivalent of IPv4 0.0.0.0.</span></span>


* <span data-ttu-id="efb0f-223">Nome de host com o número da porta</span><span class="sxs-lookup"><span data-stu-id="efb0f-223">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="efb0f-224">Nomes de host \*, e +, não são especiais.</span><span class="sxs-lookup"><span data-stu-id="efb0f-224">Host names, \*, and +, are not special.</span></span> <span data-ttu-id="efb0f-225">Tudo o que não é um endereço IP ou o "localhost" reconhecido associará a todos os IPs de IPv6 e IPv4.</span><span class="sxs-lookup"><span data-stu-id="efb0f-225">Anything that's not a recognized IP address or "localhost" will bind to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="efb0f-226">Se você precisa associar nomes de host diferentes para diferentes aplicativos ASP.NET Core na mesma porta, use [HTTP.sys](httpsys.md) ou um servidor proxy reverso, como IIS, Nginx ou Apache.</span><span class="sxs-lookup"><span data-stu-id="efb0f-226">If you need to bind different host names to different ASP.NET Core applications on the same port, use [HTTP.sys](httpsys.md) or a reverse proxy server such as IIS, Nginx, or Apache.</span></span>

* <span data-ttu-id="efb0f-227">Nome do "Localhost" com o IP de loopback ou de número de porta com o número da porta</span><span class="sxs-lookup"><span data-stu-id="efb0f-227">"Localhost" name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="efb0f-228">Quando `localhost` for especificado, Kestrel tenta associar a interfaces de loopback de IPv4 e IPv6.</span><span class="sxs-lookup"><span data-stu-id="efb0f-228">When `localhost` is specified, Kestrel tries to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="efb0f-229">Se a porta solicitada está em uso por outro serviço em cada interface de loopback, Kestrel falhar ao iniciar.</span><span class="sxs-lookup"><span data-stu-id="efb0f-229">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="efb0f-230">Se qualquer uma das interfaces de loopback não estiver disponível por qualquer outro motivo (mais comumente porque não há suporte para IPv6), Kestrel registra um aviso.</span><span class="sxs-lookup"><span data-stu-id="efb0f-230">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span> 

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="efb0f-231">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="efb0f-231">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

* <span data-ttu-id="efb0f-232">Endereço IPv4 com o número da porta</span><span class="sxs-lookup"><span data-stu-id="efb0f-232">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  https://65.55.39.10:443/
  ```

  <span data-ttu-id="efb0f-233">0.0.0.0 é um caso especial que associa a todos os endereços IPv4.</span><span class="sxs-lookup"><span data-stu-id="efb0f-233">0.0.0.0 is a special case that binds to all IPv4 addresses.</span></span>


* <span data-ttu-id="efb0f-234">Endereço IPv6 com número de porta</span><span class="sxs-lookup"><span data-stu-id="efb0f-234">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/ 
  https://[0:0:0:0:0:ffff:4137:270a]:443/ 
  ```

  <span data-ttu-id="efb0f-235">[:] é o equivalente de IPv6 do IPv4 0.0.0.0.</span><span class="sxs-lookup"><span data-stu-id="efb0f-235">[::] is the IPv6 equivalent of IPv4 0.0.0.0.</span></span>


* <span data-ttu-id="efb0f-236">Nome de host com o número da porta</span><span class="sxs-lookup"><span data-stu-id="efb0f-236">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  https://contoso.com:443/
  https://*:443/
  ```

  <span data-ttu-id="efb0f-237">Nomes de host \*, e + não são especiais.</span><span class="sxs-lookup"><span data-stu-id="efb0f-237">Host names, \*, and + aren't special.</span></span> <span data-ttu-id="efb0f-238">Tudo o que não é um endereço IP ou o "localhost" reconhecido associa a todos os IPs de IPv6 e IPv4.</span><span class="sxs-lookup"><span data-stu-id="efb0f-238">Anything that isn't a recognized IP address or "localhost" binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="efb0f-239">Se você precisa associar nomes de host diferentes para diferentes aplicativos ASP.NET Core na mesma porta, use [WebListener](weblistener.md) ou um servidor proxy reverso, como IIS, Nginx ou Apache.</span><span class="sxs-lookup"><span data-stu-id="efb0f-239">If you need to bind different host names to different ASP.NET Core applications on the same port, use [WebListener](weblistener.md) or a reverse proxy server such as IIS, Nginx, or Apache.</span></span>

* <span data-ttu-id="efb0f-240">Nome do "Localhost" com o IP de loopback ou de número de porta com o número da porta</span><span class="sxs-lookup"><span data-stu-id="efb0f-240">"Localhost" name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="efb0f-241">Quando `localhost` for especificado, Kestrel tenta associar a interfaces de loopback de IPv4 e IPv6.</span><span class="sxs-lookup"><span data-stu-id="efb0f-241">When `localhost` is specified, Kestrel tries to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="efb0f-242">Se a porta solicitada está em uso por outro serviço em cada interface de loopback, Kestrel falhar ao iniciar.</span><span class="sxs-lookup"><span data-stu-id="efb0f-242">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="efb0f-243">Se qualquer uma das interfaces de loopback não estiver disponível por qualquer outro motivo (mais comumente porque não há suporte para IPv6), Kestrel registra um aviso.</span><span class="sxs-lookup"><span data-stu-id="efb0f-243">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span> 

* <span data-ttu-id="efb0f-244">Soquete de UNIX</span><span class="sxs-lookup"><span data-stu-id="efb0f-244">Unix socket</span></span>

  ```
  http://unix:/run/dan-live.sock  
  ```

<span data-ttu-id="efb0f-245">**Porta 0**</span><span class="sxs-lookup"><span data-stu-id="efb0f-245">**Port 0**</span></span>

<span data-ttu-id="efb0f-246">Se você especificar o número de porta 0, Kestrel dinamicamente associa a uma porta disponível.</span><span class="sxs-lookup"><span data-stu-id="efb0f-246">If you specify port number 0, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="efb0f-247">Associando a porta 0 é permitido para qualquer nome de host ou IP exceto `localhost` nome.</span><span class="sxs-lookup"><span data-stu-id="efb0f-247">Binding to port 0 is allowed for any host name or IP except for `localhost` name.</span></span>

<span data-ttu-id="efb0f-248">O exemplo a seguir mostra como determinar qual porta Kestrel realmente associado em tempo de execução:</span><span class="sxs-lookup"><span data-stu-id="efb0f-248">The following example shows how to determine which port Kestrel actually bound to at runtime:</span></span>

[!code-csharp[](kestrel/sample1/Startup.cs?name=snippet_Configure)]

<span data-ttu-id="efb0f-249">**Prefixos de URL para SSL**</span><span class="sxs-lookup"><span data-stu-id="efb0f-249">**URL prefixes for SSL**</span></span>

<span data-ttu-id="efb0f-250">Certifique-se de incluir os prefixos de URL com `https:` se você chamar o `UseHttps` método de extensão, conforme mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="efb0f-250">Be sure to include URL prefixes with `https:` if you call the `UseHttps` extension method, as shown below.</span></span>

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
> <span data-ttu-id="efb0f-251">HTTPS e HTTP não podem ser hospedado na mesma porta.</span><span class="sxs-lookup"><span data-stu-id="efb0f-251">HTTPS and HTTP cannot be hosted on the same port.</span></span>

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

---
## <a name="next-steps"></a><span data-ttu-id="efb0f-252">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="efb0f-252">Next steps</span></span>

<span data-ttu-id="efb0f-253">Para obter mais informações, consulte os seguintes recursos:</span><span class="sxs-lookup"><span data-stu-id="efb0f-253">For more information, see the following resources:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="efb0f-254">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="efb0f-254">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

* [<span data-ttu-id="efb0f-255">Aplicativo de exemplo para 2. x</span><span class="sxs-lookup"><span data-stu-id="efb0f-255">Sample app for 2.x</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2)
* [<span data-ttu-id="efb0f-256">Código-fonte kestrel</span><span class="sxs-lookup"><span data-stu-id="efb0f-256">Kestrel source code</span></span>](https://github.com/aspnet/KestrelHttpServer)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="efb0f-257">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="efb0f-257">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

* [<span data-ttu-id="efb0f-258">Aplicativo de exemplo para 1. x</span><span class="sxs-lookup"><span data-stu-id="efb0f-258">Sample app for 1.x</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1)
* [<span data-ttu-id="efb0f-259">Código-fonte kestrel</span><span class="sxs-lookup"><span data-stu-id="efb0f-259">Kestrel source code</span></span>](https://github.com/aspnet/KestrelHttpServer)

---

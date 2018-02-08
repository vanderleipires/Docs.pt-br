---
title: "Implementação do servidor Web Kestrel no ASP.NET Core"
author: tdykstra
description: Apresenta o Kestrel, o servidor Web multiplataforma para o ASP.NET Core baseado no libuv.
manager: wpickett
ms.author: tdykstra
ms.custom: H1Hack27Feb2017
ms.date: 08/02/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/kestrel
ms.openlocfilehash: bfe7644891296c7c3485c9a870d90951ba87e9e8
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/30/2018
---
# <a name="introduction-to-kestrel-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="d33d4-103">Introdução à implementação do servidor Web Kestrel no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d33d4-103">Introduction to Kestrel web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="d33d4-104">Por [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher) e [Stephen Halter](https://twitter.com/halter73)</span><span class="sxs-lookup"><span data-stu-id="d33d4-104">By [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), and [Stephen Halter](https://twitter.com/halter73)</span></span>

<span data-ttu-id="d33d4-105">O Kestrel é um [servidor Web multiplataforma para o ASP.NET Core](index.md) baseado no [libuv](https://github.com/libuv/libuv), uma biblioteca de E/S assíncrona multiplataforma.</span><span class="sxs-lookup"><span data-stu-id="d33d4-105">Kestrel is a cross-platform [web server for ASP.NET Core](index.md) based on [libuv](https://github.com/libuv/libuv), a cross-platform asynchronous I/O library.</span></span> <span data-ttu-id="d33d4-106">O Kestrel é o servidor Web que está incluído por padrão em modelos de projeto do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d33d4-106">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span> 

<span data-ttu-id="d33d4-107">O Kestrel dá suporte aos seguintes recursos:</span><span class="sxs-lookup"><span data-stu-id="d33d4-107">Kestrel supports the following features:</span></span>

  * <span data-ttu-id="d33d4-108">HTTPS</span><span class="sxs-lookup"><span data-stu-id="d33d4-108">HTTPS</span></span>
  * <span data-ttu-id="d33d4-109">Atualização do Opaque usado para habilitar o [WebSockets](https://github.com/aspnet/websockets)</span><span class="sxs-lookup"><span data-stu-id="d33d4-109">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
  * <span data-ttu-id="d33d4-110">Soquetes do UNIX para alto desempenho protegidos pelo Nginx</span><span class="sxs-lookup"><span data-stu-id="d33d4-110">Unix sockets for high performance behind Nginx</span></span> 

<span data-ttu-id="d33d4-111">Há suporte para o Kestrel em todas as plataformas e versões compatíveis com o .NET Core.</span><span class="sxs-lookup"><span data-stu-id="d33d4-111">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d33d4-112">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d33d4-112">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="d33d4-113">[Exibir ou baixar um código de exemplo para o 2.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d33d4-113">[View or download sample code for 2.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d33d4-114">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d33d4-114">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="d33d4-115">[Exibir ou baixar um código de exemplo para o 1.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d33d4-115">[View or download sample code for 1.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

---

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="d33d4-116">Quando usar o Kestrel com um proxy reverso</span><span class="sxs-lookup"><span data-stu-id="d33d4-116">When to use Kestrel with a reverse proxy</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d33d4-117">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d33d4-117">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="d33d4-118">Você pode usar o Kestrel sozinho ou com um *servidor proxy reverso* como IIS, Nginx ou Apache.</span><span class="sxs-lookup"><span data-stu-id="d33d4-118">You can use Kestrel by itself or with a *reverse proxy server*, such as IIS, Nginx, or Apache.</span></span> <span data-ttu-id="d33d4-119">Um servidor proxy reverso recebe solicitações HTTP da Internet e as encaminha para o Kestrel após algum tratamento preliminar.</span><span class="sxs-lookup"><span data-stu-id="d33d4-119">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![O Kestrel se comunica diretamente com a Internet, sem um servidor proxy reverso](kestrel/_static/kestrel-to-internet2.png)

![O Kestrel se comunica indiretamente com a Internet através de um servidor proxy reverso, tal como o IIS, o Nginx ou o Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="d33d4-122">Qualquer configuração &mdash; com ou sem um servidor proxy reverso &mdash; também pode ser usada se o Kestrel é exposto somente a uma rede interna.</span><span class="sxs-lookup"><span data-stu-id="d33d4-122">Either configuration &mdash; with or without a reverse proxy server &mdash; can also be used if Kestrel is exposed only to an internal network.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d33d4-123">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d33d4-123">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="d33d4-124">Se seu aplicativo aceita solicitações somente de uma rede interna, você pode usar o Kestrel sozinho.</span><span class="sxs-lookup"><span data-stu-id="d33d4-124">If your application accepts requests only from an internal network, you can use Kestrel by itself.</span></span>

![O Kestrel se comunica diretamente com a rede interna](kestrel/_static/kestrel-to-internal.png)

<span data-ttu-id="d33d4-126">Se você expuser seu aplicativo à Internet, você deverá usar o IIS, o Nginx ou o Apache como um *servidor proxy reverso*.</span><span class="sxs-lookup"><span data-stu-id="d33d4-126">If you expose your application to the Internet, you must use IIS, Nginx, or Apache as a *reverse proxy server*.</span></span> <span data-ttu-id="d33d4-127">Um servidor proxy reverso recebe solicitações HTTP da Internet e as encaminha para o Kestrel após algum tratamento preliminar.</span><span class="sxs-lookup"><span data-stu-id="d33d4-127">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![O Kestrel se comunica indiretamente com a Internet através de um servidor proxy reverso, tal como o IIS, o Nginx ou o Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="d33d4-129">Um proxy reverso é necessário para implantações de borda (expostas ao tráfego da Internet) por motivos de segurança.</span><span class="sxs-lookup"><span data-stu-id="d33d4-129">A reverse proxy is required for edge deployments (exposed to traffic from the Internet) for security reasons.</span></span> <span data-ttu-id="d33d4-130">As versões 1.x do Kestrel não têm um conjunto completo de proteção contra ataques.</span><span class="sxs-lookup"><span data-stu-id="d33d4-130">The 1.x versions of Kestrel don't have a full complement of defenses against attacks.</span></span> <span data-ttu-id="d33d4-131">Isso inclui, mas não se limita aos tempos limite, limites de tamanho e limites de conexões simultâneas apropriados.</span><span class="sxs-lookup"><span data-stu-id="d33d4-131">This includes but isn't limited to appropriate timeouts, size limits, and concurrent connection limits.</span></span>

---

<span data-ttu-id="d33d4-132">Um cenário que exige um proxy reverso é quando você tem vários aplicativos que compartilham o mesmo IP e porta em execução em um único servidor.</span><span class="sxs-lookup"><span data-stu-id="d33d4-132">A scenario that requires a reverse proxy is when you have multiple applications that share the same IP and port running on a single server.</span></span> <span data-ttu-id="d33d4-133">Isso não funciona com o Kestrel diretamente, pois o Kestrel não dá suporte ao compartilhamento do mesmo IP e porta entre vários processos.</span><span class="sxs-lookup"><span data-stu-id="d33d4-133">That doesn't work with Kestrel directly because Kestrel doesn't support sharing the same IP and port between multiple processes.</span></span> <span data-ttu-id="d33d4-134">Quando você configura o Kestrel para escutar em uma porta, ele manipula todo o tráfego para essa porta, independentemente do cabeçalho de host.</span><span class="sxs-lookup"><span data-stu-id="d33d4-134">When you configure Kestrel to listen on a port, it handles all traffic for that port regardless of host header.</span></span> <span data-ttu-id="d33d4-135">Um proxy reverso que pode compartilhar portas, em seguida, precisa encaminhá-las para o Kestrel em um IP e porta exclusivos.</span><span class="sxs-lookup"><span data-stu-id="d33d4-135">A reverse proxy that can share ports must then forward to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="d33d4-136">Mesmo se um servidor proxy reverso não é necessário, o uso de um pode ser uma boa opção por outros motivos:</span><span class="sxs-lookup"><span data-stu-id="d33d4-136">Even if a reverse proxy server isn't required, using one might be a good choice for other reasons:</span></span>

* <span data-ttu-id="d33d4-137">Ele pode limitar a área da superfície exposta.</span><span class="sxs-lookup"><span data-stu-id="d33d4-137">It can limit your exposed surface area.</span></span>
* <span data-ttu-id="d33d4-138">Fornece uma camada de configuração e proteção adicional opcional.</span><span class="sxs-lookup"><span data-stu-id="d33d4-138">It provides an optional additional layer of configuration and defense.</span></span>
* <span data-ttu-id="d33d4-139">Pode ser integrado melhor à infraestrutura existente.</span><span class="sxs-lookup"><span data-stu-id="d33d4-139">It might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="d33d4-140">Simplifica o balanceamento de carga e a configuração do SSL.</span><span class="sxs-lookup"><span data-stu-id="d33d4-140">It simplifies load balancing and SSL set-up.</span></span> <span data-ttu-id="d33d4-141">Somente o servidor proxy reverso exige um certificado SSL e esse servidor pode se comunicar com os servidores de aplicativos na rede interna usando HTTP simples.</span><span class="sxs-lookup"><span data-stu-id="d33d4-141">Only your reverse proxy server requires an SSL certificate, and that server can communicate with your application servers on the internal network using plain HTTP.</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="d33d4-142">Como usar o Kestrel em aplicativos ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d33d4-142">How to use Kestrel in ASP.NET Core apps</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d33d4-143">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d33d4-143">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="d33d4-144">O pacote [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) está incluído no [metapacote Microsoft.AspNetCore.All](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="d33d4-144">The [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) package is included in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

<span data-ttu-id="d33d4-145">Os modelos de projeto do ASP.NET Core usam o Kestrel por padrão.</span><span class="sxs-lookup"><span data-stu-id="d33d4-145">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="d33d4-146">Em *Program.cs*, o código de modelo chama `CreateDefaultBuilder`, que chama [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) em segundo plano.</span><span class="sxs-lookup"><span data-stu-id="d33d4-146">In *Program.cs*, the template code calls `CreateDefaultBuilder`, which calls [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) behind the scenes.</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

<span data-ttu-id="d33d4-147">Caso precise configurar as opções do Kestrel, chame `UseKestrel` em *Program.cs*, conforme mostrado no seguinte exemplo:</span><span class="sxs-lookup"><span data-stu-id="d33d4-147">If you need to configure Kestrel options, call `UseKestrel` in *Program.cs* as shown in the following example:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d33d4-148">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d33d4-148">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="d33d4-149">Instale o pacote NuGet [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/).</span><span class="sxs-lookup"><span data-stu-id="d33d4-149">Install the [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) NuGet package.</span></span>

<span data-ttu-id="d33d4-150">Chame o método de extensão [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) em `WebHostBuilder` no método `Main`, especificando as [opções do Kestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions) necessárias, conforme mostrado na próxima seção.</span><span class="sxs-lookup"><span data-stu-id="d33d4-150">Call the [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) extension method on `WebHostBuilder` in your `Main` method, specifying any [Kestrel options](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions) that you need, as shown in the next section.</span></span>

[!code-csharp[](kestrel/sample1/Program.cs?name=snippet_Main&highlight=13-19)]

---

### <a name="kestrel-options"></a><span data-ttu-id="d33d4-151">Opções do Kestrel</span><span class="sxs-lookup"><span data-stu-id="d33d4-151">Kestrel options</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d33d4-152">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d33d4-152">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="d33d4-153">O servidor Web do Kestrel tem opções de configuração de restrição especialmente úteis em implantações para a Internet.</span><span class="sxs-lookup"><span data-stu-id="d33d4-153">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span> <span data-ttu-id="d33d4-154">Estes são alguns dos limites que podem ser definidos:</span><span class="sxs-lookup"><span data-stu-id="d33d4-154">Here are some of the limits you can set:</span></span>

- <span data-ttu-id="d33d4-155">Número máximo de conexões de cliente</span><span class="sxs-lookup"><span data-stu-id="d33d4-155">Maximum client connections</span></span>
- <span data-ttu-id="d33d4-156">Tamanho máximo do corpo da solicitação</span><span class="sxs-lookup"><span data-stu-id="d33d4-156">Maximum request body size</span></span>
- <span data-ttu-id="d33d4-157">Taxa de dados mínima do corpo da solicitação</span><span class="sxs-lookup"><span data-stu-id="d33d4-157">Minimum request body data rate</span></span>

<span data-ttu-id="d33d4-158">Defina essas restrições e outras na propriedade `Limits` da classe [KestrelServerOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="d33d4-158">You set these constraints and others in the `Limits` property of the [KestrelServerOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs) class.</span></span> <span data-ttu-id="d33d4-159">A propriedade `Limits` contém uma instância da classe [KestrelServerLimits](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs).</span><span class="sxs-lookup"><span data-stu-id="d33d4-159">The `Limits` property holds an instance of the [KestrelServerLimits](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs) class.</span></span> 

<span data-ttu-id="d33d4-160">**Número máximo de conexões de cliente**</span><span class="sxs-lookup"><span data-stu-id="d33d4-160">**Maximum client connections**</span></span>

<span data-ttu-id="d33d4-161">O número máximo de conexões TCP abertas simultâneas pode ser definido para todo o aplicativo com o seguinte código:</span><span class="sxs-lookup"><span data-stu-id="d33d4-161">The maximum number of concurrent open TCP connections can be set for the entire application with the following code:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="d33d4-162">Há um limite separado para conexões que foram atualizadas do HTTP ou HTTPS para outro protocolo (por exemplo, em uma solicitação do WebSockets).</span><span class="sxs-lookup"><span data-stu-id="d33d4-162">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="d33d4-163">Depois que uma conexão é atualizada, ela não é contada em relação ao limite de `MaxConcurrentConnections`.</span><span class="sxs-lookup"><span data-stu-id="d33d4-163">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span> 

<span data-ttu-id="d33d4-164">O número máximo de conexões é ilimitado (nulo) por padrão.</span><span class="sxs-lookup"><span data-stu-id="d33d4-164">The maximum number of connections is unlimited (null) by default.</span></span>

<span data-ttu-id="d33d4-165">**Tamanho máximo do corpo da solicitação**</span><span class="sxs-lookup"><span data-stu-id="d33d4-165">**Maximum request body size**</span></span>

<span data-ttu-id="d33d4-166">O tamanho máximo do corpo da solicitação padrão é de 30.000.000 bytes, que equivale aproximadamente a 28,6 MB.</span><span class="sxs-lookup"><span data-stu-id="d33d4-166">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6MB.</span></span> 

<span data-ttu-id="d33d4-167">A maneira recomendada para substituir o limite em um aplicativo ASP.NET Core MVC é usar o atributo [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) em um método de ação:</span><span class="sxs-lookup"><span data-stu-id="d33d4-167">The recommended way to override the limit in an ASP.NET Core MVC app is to use the [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="d33d4-168">Este é um exemplo que mostra como configurar a restrição para todo o aplicativo, em cada solicitação:</span><span class="sxs-lookup"><span data-stu-id="d33d4-168">Here's an example that shows how to configure the constraint for the entire application, every request:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=5)]

<span data-ttu-id="d33d4-169">Substitua a configuração em uma solicitação específica no middleware:</span><span class="sxs-lookup"><span data-stu-id="d33d4-169">You can override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Limits&highlight=3-4)]
 
<span data-ttu-id="d33d4-170">Uma exceção é gerada se você tenta configurar o limite de uma solicitação depois que o aplicativo iniciou a leitura da solicitação.</span><span class="sxs-lookup"><span data-stu-id="d33d4-170">An exception is thrown if you try to configure the limit on a request after the application has started reading the request.</span></span> <span data-ttu-id="d33d4-171">Há uma propriedade `IsReadOnly` que indica se a propriedade `MaxRequestBodySize` está no estado somente leitura, o que significa que é tarde demais para configurar o limite.</span><span class="sxs-lookup"><span data-stu-id="d33d4-171">There's an `IsReadOnly` property that tells you if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="d33d4-172">**Taxa de dados mínima do corpo da solicitação**</span><span class="sxs-lookup"><span data-stu-id="d33d4-172">**Minimum request body data rate**</span></span>

<span data-ttu-id="d33d4-173">O Kestrel verifica a cada segundo se os dados estão sendo recebidos na taxa especificada em bytes/segundo.</span><span class="sxs-lookup"><span data-stu-id="d33d4-173">Kestrel checks every second if data is coming in at the specified rate in bytes/second.</span></span> <span data-ttu-id="d33d4-174">Se a taxa cair abaixo do mínimo, a conexão atingirá o tempo limite. O período de cortesia é o tempo que o Kestrel fornece ao cliente para aumentar sua taxa de envio até o mínimo; a taxa não é verificada durante esse período.</span><span class="sxs-lookup"><span data-stu-id="d33d4-174">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="d33d4-175">O período de cortesia ajuda a evitar a remoção de conexões que inicialmente enviam dados em uma taxa baixa devido ao início lento do TCP.</span><span class="sxs-lookup"><span data-stu-id="d33d4-175">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="d33d4-176">A taxa mínima padrão é de 240 bytes/segundo, com um período de cortesia de 5 segundos.</span><span class="sxs-lookup"><span data-stu-id="d33d4-176">The default minimum rate is 240 bytes/second, with a 5 second grace period.</span></span>

<span data-ttu-id="d33d4-177">Uma taxa mínima também se aplica à resposta.</span><span class="sxs-lookup"><span data-stu-id="d33d4-177">A minimum rate also applies to the response.</span></span> <span data-ttu-id="d33d4-178">O código para definir o limite de solicitação e o limite de resposta é o mesmo, exceto por ter `RequestBody` ou `Response` nos nomes da propriedade e da interface.</span><span class="sxs-lookup"><span data-stu-id="d33d4-178">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span> 

<span data-ttu-id="d33d4-179">Este é um exemplo que mostra como configurar as taxas mínima de dados em *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="d33d4-179">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=6-9)]

<span data-ttu-id="d33d4-180">Configure as taxas por solicitação no middleware:</span><span class="sxs-lookup"><span data-stu-id="d33d4-180">You can configure the rates per request in middleware:</span></span>

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Limits&highlight=5-8)]

<span data-ttu-id="d33d4-181">Para obter informações sobre outras opções do Kestrel, consulte as seguintes classes:</span><span class="sxs-lookup"><span data-stu-id="d33d4-181">For information about other Kestrel options, see the following classes:</span></span>

* [<span data-ttu-id="d33d4-182">KestrelServerOptions</span><span class="sxs-lookup"><span data-stu-id="d33d4-182">KestrelServerOptions</span></span>](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs)
* [<span data-ttu-id="d33d4-183">KestrelServerLimits</span><span class="sxs-lookup"><span data-stu-id="d33d4-183">KestrelServerLimits</span></span>](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs)
* [<span data-ttu-id="d33d4-184">ListenOptions</span><span class="sxs-lookup"><span data-stu-id="d33d4-184">ListenOptions</span></span>](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d33d4-185">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d33d4-185">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="d33d4-186">Para obter informações sobre as opções do Kestrel, consulte [Classe KestrelServerOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions).</span><span class="sxs-lookup"><span data-stu-id="d33d4-186">For information about Kestrel options, see [KestrelServerOptions class](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions).</span></span>

---

### <a name="endpoint-configuration"></a><span data-ttu-id="d33d4-187">Configuração do ponto de extremidade</span><span class="sxs-lookup"><span data-stu-id="d33d4-187">Endpoint configuration</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d33d4-188">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d33d4-188">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="d33d4-189">Por padrão, o ASP.NET Core é associado a `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="d33d4-189">By default ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="d33d4-190">Configure prefixos de URL e portas nas quais o Kestrel escuta chamando métodos `Listen` ou `ListenUnixSocket` em `KestrelServerOptions`.</span><span class="sxs-lookup"><span data-stu-id="d33d4-190">You configure URL prefixes and ports for Kestrel to listen on by calling `Listen` or `ListenUnixSocket` methods on `KestrelServerOptions`.</span></span> <span data-ttu-id="d33d4-191">(`UseUrls`, o argumento de linha de comando `urls` e a variável de ambiente ASPNETCORE_URLS também funcionam, mas têm as limitações indicadas [mais adiante neste artigo](#useurls-limitations).)</span><span class="sxs-lookup"><span data-stu-id="d33d4-191">(`UseUrls`, the `urls` command-line argument, and the ASPNETCORE_URLS environment variable also work but have the limitations noted [later in this article](#useurls-limitations).)</span></span>

<span data-ttu-id="d33d4-192">**Associar a um soquete TCP**</span><span class="sxs-lookup"><span data-stu-id="d33d4-192">**Bind to a TCP socket**</span></span>

<span data-ttu-id="d33d4-193">O método `Listen` é associado a um soquete TCP e um lambda de opções permite que você configure um certificado SSL:</span><span class="sxs-lookup"><span data-stu-id="d33d4-193">The `Listen` method binds to a TCP socket, and an options lambda lets you configure an SSL certificate:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

<span data-ttu-id="d33d4-194">Observe como esse exemplo configura o SSL de um ponto de extremidade específico usando [ListenOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="d33d4-194">Notice how this example configures SSL for a particular endpoint by using [ListenOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs).</span></span> <span data-ttu-id="d33d4-195">Use a mesma API para definir outras configurações do Kestrel para pontos de extremidade específicos.</span><span class="sxs-lookup"><span data-stu-id="d33d4-195">You can use the same API to configure other Kestrel settings for particular endpoints.</span></span>

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

<span data-ttu-id="d33d4-196">**Associar a um soquete do UNIX**</span><span class="sxs-lookup"><span data-stu-id="d33d4-196">**Bind to a Unix socket**</span></span>

<span data-ttu-id="d33d4-197">Escute em um soquete do UNIX para um melhor desempenho com o Nginx, conforme mostrado neste exemplo:</span><span class="sxs-lookup"><span data-stu-id="d33d4-197">You can listen on a Unix socket for improved performance with Nginx, as shown in this example:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_UnixSocket)]

<span data-ttu-id="d33d4-198">**Porta 0**</span><span class="sxs-lookup"><span data-stu-id="d33d4-198">**Port 0**</span></span>

<span data-ttu-id="d33d4-199">Se você especificar o número de porta 0, o Kestrel associa-o dinamicamente a uma porta disponível.</span><span class="sxs-lookup"><span data-stu-id="d33d4-199">If you specify port number 0, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="d33d4-200">O seguinte exemplo mostra como determinar à qual porta o Kestrel realmente está associado em tempo de execução:</span><span class="sxs-lookup"><span data-stu-id="d33d4-200">The following example shows how to determine which port Kestrel actually bound to at runtime:</span></span>

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Configure&highlight=3,13,16-17)]

<a id="useurls-limitations"></a>

<span data-ttu-id="d33d4-201">**Limitações de UseUrls**</span><span class="sxs-lookup"><span data-stu-id="d33d4-201">**UseUrls limitations**</span></span>

<span data-ttu-id="d33d4-202">Configure pontos de extremidade chamando o método `UseUrls` ou usando o argumento de linha de comando `urls` ou a variável de ambiente ASPNETCORE_URLS.</span><span class="sxs-lookup"><span data-stu-id="d33d4-202">You can configure endpoints by calling the `UseUrls` method or using the `urls` command-line argument or the ASPNETCORE_URLS environment variable.</span></span> <span data-ttu-id="d33d4-203">Esses métodos são úteis se você deseja que o código funcione com servidores que não sejam o Kestrel.</span><span class="sxs-lookup"><span data-stu-id="d33d4-203">These methods are useful if you want your code to work with servers other than Kestrel.</span></span> <span data-ttu-id="d33d4-204">No entanto, esteja ciente dessas limitações:</span><span class="sxs-lookup"><span data-stu-id="d33d4-204">However, be aware of these limitations:</span></span>

* <span data-ttu-id="d33d4-205">Não é possível usar o SSL com esses métodos.</span><span class="sxs-lookup"><span data-stu-id="d33d4-205">You can't use SSL with these methods.</span></span>
* <span data-ttu-id="d33d4-206">Se você usar o método `Listen` e `UseUrls`, os pontos de extremidade `Listen` substituirão os pontos de extremidade `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="d33d4-206">If you use both the `Listen` method and `UseUrls`, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

<span data-ttu-id="d33d4-207">**Configuração de ponto de extremidade para o IIS**</span><span class="sxs-lookup"><span data-stu-id="d33d4-207">**Endpoint configuration for IIS**</span></span>

<span data-ttu-id="d33d4-208">Se você usar o IIS, as associações de URL do IIS substituirão as associações definidas com uma chamada a `Listen` ou `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="d33d4-208">If you use IIS, the URL bindings for IIS override any bindings that you set by calling either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="d33d4-209">Para obter mais informações, consulte [Introdução ao Módulo do ASP.NET Core](aspnet-core-module.md).</span><span class="sxs-lookup"><span data-stu-id="d33d4-209">For more information, see [Introduction to ASP.NET Core Module](aspnet-core-module.md).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d33d4-210">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d33d4-210">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="d33d4-211">Por padrão, o ASP.NET Core é associado a `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="d33d4-211">By default ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="d33d4-212">Configure prefixos de URL e portas nas quais o Kestrel escuta usando o método de extensão `UseUrls`, o argumento de linha de comando `urls` ou o sistema de configuração do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d33d4-212">You can configure URL prefixes and ports for Kestrel to listen on by using the `UseUrls` extension method, the `urls` command-line argument, or the ASP.NET Core configuration system.</span></span> <span data-ttu-id="d33d4-213">Para obter mais informações sobre esses métodos, consulte [Hospedagem](../../fundamentals/hosting.md).</span><span class="sxs-lookup"><span data-stu-id="d33d4-213">For more information about these methods, see [Hosting](../../fundamentals/hosting.md).</span></span> <span data-ttu-id="d33d4-214">Para obter informações sobre como funciona a associação de URL quando o IIS é usado como um proxy reverso, consulte [Módulo do ASP.NET Core](aspnet-core-module.md).</span><span class="sxs-lookup"><span data-stu-id="d33d4-214">For information about how URL binding works when you use IIS as a reverse proxy, see [ASP.NET Core Module](aspnet-core-module.md).</span></span> 

---

### <a name="url-prefixes"></a><span data-ttu-id="d33d4-215">Prefixos de URL</span><span class="sxs-lookup"><span data-stu-id="d33d4-215">URL prefixes</span></span>

<span data-ttu-id="d33d4-216">Se você chamar `UseUrls` ou usar o argumento de linha de comando `urls` ou a variável de ambiente ASPNETCORE_URLS, os prefixos de URL poderão estar em um dos formatos a seguir.</span><span class="sxs-lookup"><span data-stu-id="d33d4-216">If you call `UseUrls` or use the `urls` command-line argument or ASPNETCORE_URLS environment variable, the URL prefixes can be in any of the following formats.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d33d4-217">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d33d4-217">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="d33d4-218">Somente prefixos de URL HTTP são válidos. O Kestrel não dá suporte ao SSL quando associações de URL são configuradas com `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="d33d4-218">Only HTTP URL prefixes are valid; Kestrel doesn't support SSL when you configure URL bindings by using `UseUrls`.</span></span>

* <span data-ttu-id="d33d4-219">Endereço IPv4 com o número da porta</span><span class="sxs-lookup"><span data-stu-id="d33d4-219">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="d33d4-220">0.0.0.0 é um caso especial que é associado a todos os endereços IPv4.</span><span class="sxs-lookup"><span data-stu-id="d33d4-220">0.0.0.0 is a special case that binds to all IPv4 addresses.</span></span>


* <span data-ttu-id="d33d4-221">Endereço IPv6 com número da porta</span><span class="sxs-lookup"><span data-stu-id="d33d4-221">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/ 
  ```

  <span data-ttu-id="d33d4-222">[::] é o equivalente de IPv6 do IPv4 0.0.0.0.</span><span class="sxs-lookup"><span data-stu-id="d33d4-222">[::] is the IPv6 equivalent of IPv4 0.0.0.0.</span></span>


* <span data-ttu-id="d33d4-223">Nome do host com o número da porta</span><span class="sxs-lookup"><span data-stu-id="d33d4-223">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="d33d4-224">Nomes de host, \* e +, não são especiais.</span><span class="sxs-lookup"><span data-stu-id="d33d4-224">Host names, \*, and +, are not special.</span></span> <span data-ttu-id="d33d4-225">Tudo o que não é um endereço IP reconhecido ou o "localhost" será associado a todos os IPs do IPv4 e IPv6.</span><span class="sxs-lookup"><span data-stu-id="d33d4-225">Anything that's not a recognized IP address or "localhost" will bind to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="d33d4-226">Se você precisa associar diferentes nomes de host a diferentes aplicativos ASP.NET Core na mesma porta, use o [HTTP.sys](httpsys.md) ou um servidor proxy reverso, como o IIS, Nginx ou Apache.</span><span class="sxs-lookup"><span data-stu-id="d33d4-226">If you need to bind different host names to different ASP.NET Core applications on the same port, use [HTTP.sys](httpsys.md) or a reverse proxy server such as IIS, Nginx, or Apache.</span></span>

* <span data-ttu-id="d33d4-227">Nome do "localhost" com o número da porta ou IP de loopback com o número da porta</span><span class="sxs-lookup"><span data-stu-id="d33d4-227">"Localhost" name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="d33d4-228">Quando `localhost` é especificado, o Kestrel tenta associar a interfaces de loopback IPv4 e IPv6.</span><span class="sxs-lookup"><span data-stu-id="d33d4-228">When `localhost` is specified, Kestrel tries to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="d33d4-229">Se a porta solicitada está sendo usada por outro serviço em uma das interfaces de loopback, o Kestrel falha ao ser iniciado.</span><span class="sxs-lookup"><span data-stu-id="d33d4-229">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="d33d4-230">Se uma das interfaces de loopback não estiver disponível por qualquer outro motivo (geralmente porque não há suporte para o IPv6), o Kestrel registra um aviso em log.</span><span class="sxs-lookup"><span data-stu-id="d33d4-230">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span> 

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d33d4-231">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d33d4-231">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

* <span data-ttu-id="d33d4-232">Endereço IPv4 com o número da porta</span><span class="sxs-lookup"><span data-stu-id="d33d4-232">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  https://65.55.39.10:443/
  ```

  <span data-ttu-id="d33d4-233">0.0.0.0 é um caso especial que é associado a todos os endereços IPv4.</span><span class="sxs-lookup"><span data-stu-id="d33d4-233">0.0.0.0 is a special case that binds to all IPv4 addresses.</span></span>


* <span data-ttu-id="d33d4-234">Endereço IPv6 com número da porta</span><span class="sxs-lookup"><span data-stu-id="d33d4-234">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/ 
  https://[0:0:0:0:0:ffff:4137:270a]:443/ 
  ```

  <span data-ttu-id="d33d4-235">[::] é o equivalente de IPv6 do IPv4 0.0.0.0.</span><span class="sxs-lookup"><span data-stu-id="d33d4-235">[::] is the IPv6 equivalent of IPv4 0.0.0.0.</span></span>


* <span data-ttu-id="d33d4-236">Nome do host com o número da porta</span><span class="sxs-lookup"><span data-stu-id="d33d4-236">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  https://contoso.com:443/
  https://*:443/
  ```

  <span data-ttu-id="d33d4-237">Nomes de host, \* e + não são especiais.</span><span class="sxs-lookup"><span data-stu-id="d33d4-237">Host names, \*, and + aren't special.</span></span> <span data-ttu-id="d33d4-238">Tudo o que não é um endereço IP reconhecido ou o "localhost" é associado a todos os IPs do IPv4 e IPv6.</span><span class="sxs-lookup"><span data-stu-id="d33d4-238">Anything that isn't a recognized IP address or "localhost" binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="d33d4-239">Se você precisa associar diferentes nomes de host a diferentes aplicativos ASP.NET Core na mesma porta, use o [WebListener](weblistener.md) ou um servidor proxy reverso, como o IIS, Nginx ou Apache.</span><span class="sxs-lookup"><span data-stu-id="d33d4-239">If you need to bind different host names to different ASP.NET Core applications on the same port, use [WebListener](weblistener.md) or a reverse proxy server such as IIS, Nginx, or Apache.</span></span>

* <span data-ttu-id="d33d4-240">Nome do "localhost" com o número da porta ou IP de loopback com o número da porta</span><span class="sxs-lookup"><span data-stu-id="d33d4-240">"Localhost" name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="d33d4-241">Quando `localhost` é especificado, o Kestrel tenta associar a interfaces de loopback IPv4 e IPv6.</span><span class="sxs-lookup"><span data-stu-id="d33d4-241">When `localhost` is specified, Kestrel tries to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="d33d4-242">Se a porta solicitada está sendo usada por outro serviço em uma das interfaces de loopback, o Kestrel falha ao ser iniciado.</span><span class="sxs-lookup"><span data-stu-id="d33d4-242">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="d33d4-243">Se uma das interfaces de loopback não estiver disponível por qualquer outro motivo (geralmente porque não há suporte para o IPv6), o Kestrel registra um aviso em log.</span><span class="sxs-lookup"><span data-stu-id="d33d4-243">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span> 

* <span data-ttu-id="d33d4-244">Soquete do UNIX</span><span class="sxs-lookup"><span data-stu-id="d33d4-244">Unix socket</span></span>

  ```
  http://unix:/run/dan-live.sock  
  ```

<span data-ttu-id="d33d4-245">**Porta 0**</span><span class="sxs-lookup"><span data-stu-id="d33d4-245">**Port 0**</span></span>

<span data-ttu-id="d33d4-246">Se você especificar o número de porta 0, o Kestrel associa-o dinamicamente a uma porta disponível.</span><span class="sxs-lookup"><span data-stu-id="d33d4-246">If you specify port number 0, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="d33d4-247">A associação à porta 0 é permitida para qualquer nome do host ou IP, exceto o nome `localhost`.</span><span class="sxs-lookup"><span data-stu-id="d33d4-247">Binding to port 0 is allowed for any host name or IP except for `localhost` name.</span></span>

<span data-ttu-id="d33d4-248">O seguinte exemplo mostra como determinar à qual porta o Kestrel realmente está associado em tempo de execução:</span><span class="sxs-lookup"><span data-stu-id="d33d4-248">The following example shows how to determine which port Kestrel actually bound to at runtime:</span></span>

[!code-csharp[](kestrel/sample1/Startup.cs?name=snippet_Configure)]

<span data-ttu-id="d33d4-249">**Prefixos de URL para o SSL**</span><span class="sxs-lookup"><span data-stu-id="d33d4-249">**URL prefixes for SSL**</span></span>

<span data-ttu-id="d33d4-250">Lembre-se de incluir os prefixos de URL com `https:` se você chamar o método de extensão `UseHttps`, conforme mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="d33d4-250">Be sure to include URL prefixes with `https:` if you call the `UseHttps` extension method, as shown below.</span></span>

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
> <span data-ttu-id="d33d4-251">HTTPS e HTTP não podem ser hospedados na mesma porta.</span><span class="sxs-lookup"><span data-stu-id="d33d4-251">HTTPS and HTTP cannot be hosted on the same port.</span></span>

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

---
## <a name="next-steps"></a><span data-ttu-id="d33d4-252">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="d33d4-252">Next steps</span></span>

<span data-ttu-id="d33d4-253">Para obter mais informações, consulte os seguintes recursos:</span><span class="sxs-lookup"><span data-stu-id="d33d4-253">For more information, see the following resources:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d33d4-254">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d33d4-254">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

* [<span data-ttu-id="d33d4-255">Aplicativo de exemplo para o 2.x</span><span class="sxs-lookup"><span data-stu-id="d33d4-255">Sample app for 2.x</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2)
* [<span data-ttu-id="d33d4-256">Código-fonte do kestrel</span><span class="sxs-lookup"><span data-stu-id="d33d4-256">Kestrel source code</span></span>](https://github.com/aspnet/KestrelHttpServer)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d33d4-257">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d33d4-257">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

* [<span data-ttu-id="d33d4-258">Aplicativo de exemplo para o 1.x</span><span class="sxs-lookup"><span data-stu-id="d33d4-258">Sample app for 1.x</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1)
* [<span data-ttu-id="d33d4-259">Código-fonte do kestrel</span><span class="sxs-lookup"><span data-stu-id="d33d4-259">Kestrel source code</span></span>](https://github.com/aspnet/KestrelHttpServer)

---

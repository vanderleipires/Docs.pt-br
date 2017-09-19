---
title: "Implementações de servidor Web em ASP.NET Core"
author: tdykstra
description: "Apresenta os servidores Web Kestrel e WebListener para ASP.NET Core. Fornece diretrizes sobre como escolher um e quando usá-lo com um servidor proxy reverso."
keywords: ASP.NET Core, IServer, servidor Web, Kestrel, WebListener, proxy reverso
ms.author: tdykstra
manager: wpickett
ms.date: 08/03/2017
ms.topic: article
ms.assetid: dba74f39-58cd-4dee-a061-6d15f7346959
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/index
ms.openlocfilehash: 54c8e1ad7d4de7f953d9801c214c0bd577304f46
ms.sourcegitcommit: 67f54fabbfa4e3942f5bfe1f8a7fdfe4a7a75358
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/19/2017
---
# <a name="web-server-implementations-in-aspnet-core"></a><span data-ttu-id="c248f-105">Implementações de servidor Web em ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c248f-105">Web server implementations in ASP.NET Core</span></span>

<span data-ttu-id="c248f-106">Por [Tom Dykstra](https://github.com/tdykstra), [Steve Smith](https://ardalis.com/), [Stephen Halter](https://twitter.com/halter73) e [Chris Ross](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="c248f-106">By [Tom Dykstra](https://github.com/tdykstra), [Steve Smith](https://ardalis.com/), [Stephen Halter](https://twitter.com/halter73), and [Chris Ross](https://github.com/Tratcher)</span></span>

<span data-ttu-id="c248f-107">Um aplicativo ASP.NET Core é executado com uma implementação do servidor HTTP em processo.</span><span class="sxs-lookup"><span data-stu-id="c248f-107">An ASP.NET Core application runs with an in-process HTTP server implementation.</span></span> <span data-ttu-id="c248f-108">A implementação do servidor escuta solicitações HTTP e traz essas solicitações à tona para o aplicativo como conjuntos de [recursos de solicitação](https://docs.microsoft.com/aspnet/core/fundamentals/request-features) compostos em um `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="c248f-108">The server implementation listens for HTTP requests and surfaces them to the application as sets of [request features](https://docs.microsoft.com/aspnet/core/fundamentals/request-features) composed into an `HttpContext`.</span></span>

<span data-ttu-id="c248f-109">O ASP.NET Core envia duas implementações de servidor:</span><span class="sxs-lookup"><span data-stu-id="c248f-109">ASP.NET Core ships two server implementations:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c248f-110">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c248f-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

* <span data-ttu-id="c248f-111">O [Kestrel](kestrel.md) é um servidor HTTP de plataforma cruzada com base em [libuv](https://github.com/libuv/libuv), uma biblioteca de E/S assíncrona de plataforma cruzada.</span><span class="sxs-lookup"><span data-stu-id="c248f-111">[Kestrel](kestrel.md) is a cross-platform HTTP server based on [libuv](https://github.com/libuv/libuv), a cross-platform asynchronous I/O library.</span></span>

* <span data-ttu-id="c248f-112">O [HTTP.sys](httpsys.md) é um servidor HTTP somente do Windows com base no [driver do kernel Http.sys](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span><span class="sxs-lookup"><span data-stu-id="c248f-112">[HTTP.sys](httpsys.md) is a Windows-only HTTP server based on the [Http.Sys kernel driver](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c248f-113">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c248f-113">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

* <span data-ttu-id="c248f-114">O [Kestrel](kestrel.md) é um servidor HTTP de plataforma cruzada com base em [libuv](https://github.com/libuv/libuv), uma biblioteca de E/S assíncrona de plataforma cruzada.</span><span class="sxs-lookup"><span data-stu-id="c248f-114">[Kestrel](kestrel.md) is a cross-platform HTTP server based on [libuv](https://github.com/libuv/libuv), a cross-platform asynchronous I/O library.</span></span>

* <span data-ttu-id="c248f-115">O [WebListener](weblistener.md) é um servidor HTTP somente do Windows com base no [driver do kernel Http.sys](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span><span class="sxs-lookup"><span data-stu-id="c248f-115">[WebListener](weblistener.md) is a Windows-only HTTP server based on the [Http.Sys kernel driver](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span>

---

## <a name="kestrel"></a><span data-ttu-id="c248f-116">Kestrel</span><span class="sxs-lookup"><span data-stu-id="c248f-116">Kestrel</span></span>

<span data-ttu-id="c248f-117">O Kestrel é o servidor Web que está incluído por padrão em modelos de novo projeto do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c248f-117">Kestrel is the web server that is included by default in ASP.NET Core new-project templates.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c248f-118">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c248f-118">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="c248f-119">Você pode usar o Kestrel sozinho ou com um *servidor proxy reverso* como IIS, Nginx ou Apache.</span><span class="sxs-lookup"><span data-stu-id="c248f-119">You can use Kestrel by itself or with a *reverse proxy server*, such as IIS, Nginx, or Apache.</span></span> <span data-ttu-id="c248f-120">Um servidor proxy reverso recebe solicitações HTTP da Internet e as encaminha para o Kestrel após algum tratamento preliminar.</span><span class="sxs-lookup"><span data-stu-id="c248f-120">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![O Kestrel se comunica diretamente com a Internet, sem um servidor proxy reverso](kestrel/_static/kestrel-to-internet2.png)

![O Kestrel se comunica indiretamente com a Internet através de um servidor proxy reverso, tal como o IIS, o Nginx ou o Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="c248f-123">Qualquer configuração &mdash; com ou sem um servidor proxy reverso &mdash; também pode ser usada se o Kestrel é exposto somente a uma rede interna.</span><span class="sxs-lookup"><span data-stu-id="c248f-123">Either configuration &mdash; with or without a reverse proxy server &mdash; can also be used if Kestrel is exposed only to an internal network.</span></span>

<span data-ttu-id="c248f-124">Para obter informações sobre quando usar Kestrel com um proxy reverso, consulte [Introdução ao Kestrel](kestrel.md).</span><span class="sxs-lookup"><span data-stu-id="c248f-124">For information about when to use Kestrel with a reverse proxy, see [Introduction to Kestrel](kestrel.md).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c248f-125">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c248f-125">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="c248f-126">Se seu aplicativo aceita solicitações somente de uma rede interna, você pode usar o Kestrel sozinho.</span><span class="sxs-lookup"><span data-stu-id="c248f-126">If your application accepts requests only from an internal network, you can use Kestrel by itself.</span></span>

![O Kestrel se comunica diretamente com a rede interna](kestrel/_static/kestrel-to-internal.png)

<span data-ttu-id="c248f-128">Se você expuser seu aplicativo à Internet, você deverá usar o IIS, o Nginx ou o Apache como um *servidor proxy reverso*.</span><span class="sxs-lookup"><span data-stu-id="c248f-128">If you expose your application to the Internet, you must use IIS, Nginx, or Apache as a *reverse proxy server*.</span></span> <span data-ttu-id="c248f-129">Um servidor proxy reverso recebe solicitações HTTP da Internet e as encaminha para o Kestrel após algum tratamento preliminar, conforme mostrado no diagrama a seguir.</span><span class="sxs-lookup"><span data-stu-id="c248f-129">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling, as shown in the following diagram.</span></span>

![O Kestrel se comunica indiretamente com a Internet através de um servidor proxy reverso, tal como o IIS, o Nginx ou o Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="c248f-131">O motivo mais importante para usar um proxy reverso para implantações de borda (expostas ao tráfego da Internet) é a segurança.</span><span class="sxs-lookup"><span data-stu-id="c248f-131">The most important reason for using a reverse proxy for edge deployments (exposed to traffic from the Internet) is security.</span></span> <span data-ttu-id="c248f-132">As versões 1.x do Kestrel não têm um conjunto completo de proteção contra ataques.</span><span class="sxs-lookup"><span data-stu-id="c248f-132">The 1.x versions of Kestrel don't have a full complement of defenses against attacks.</span></span> <span data-ttu-id="c248f-133">Isso inclui mas não se limita aos tempos limite, limites de tamanho e limites de conexões simultâneas apropriados.</span><span class="sxs-lookup"><span data-stu-id="c248f-133">This includes, but isn't limited to, appropriate timeouts, size limits, and concurrent connection limits.</span></span>

<span data-ttu-id="c248f-134">Para obter informações sobre quando usar Kestrel com um proxy reverso, consulte [Introdução ao Kestrel](kestrel.md).</span><span class="sxs-lookup"><span data-stu-id="c248f-134">For information about when to use Kestrel with a reverse proxy, see [Introduction to Kestrel](kestrel.md).</span></span>

---

<span data-ttu-id="c248f-135">Você não pode usar o IIS, Nginx ou Apache sem o Kestrel ou uma [implementação de servidor personalizado](#custom-servers).</span><span class="sxs-lookup"><span data-stu-id="c248f-135">You can't use IIS, Nginx, or Apache without Kestrel or a [custom server implementation](#custom-servers).</span></span> <span data-ttu-id="c248f-136">O ASP.NET Core foi projetado para ser executado em seu próprio processo, de modo que ele pode se comportar de forma consistente entre plataformas.</span><span class="sxs-lookup"><span data-stu-id="c248f-136">ASP.NET Core was designed to run in its own process so that it can behave consistently across platforms.</span></span> <span data-ttu-id="c248f-137">Cada um entre o IIS, o Nginx e o Apache determina seu próprio processo de inicialização e ambiente; para usá-los diretamente, o ASP.NET Core teria de adaptar-se às necessidades de cada um deles.</span><span class="sxs-lookup"><span data-stu-id="c248f-137">IIS, Nginx, and Apache dictate their own startup process and environment; to use them directly, ASP.NET Core would have to adapt to the needs of each one.</span></span> <span data-ttu-id="c248f-138">Usar uma implementação do servidor Web, tal como Kestrel, oferece ao ASP.NET Core controle sobre o processo de inicialização e o ambiente.</span><span class="sxs-lookup"><span data-stu-id="c248f-138">Using a web server implementation such as Kestrel gives ASP.NET Core control over the startup process and environment.</span></span> <span data-ttu-id="c248f-139">Portanto, em vez de tentar adaptar o ASP.NET Core para o IIS, o Nginx ou o Apache, configure esses servidores Web para solicitações de proxy para Kestrel.</span><span class="sxs-lookup"><span data-stu-id="c248f-139">So rather than trying to adapt ASP.NET Core to IIS, Nginx, or Apache, you just set up those web servers to proxy requests to Kestrel.</span></span> <span data-ttu-id="c248f-140">Esse acordo permite que as classes `Program.Main` e `Startup` sejam essencialmente as mesmas, independentemente do local da implantação.</span><span class="sxs-lookup"><span data-stu-id="c248f-140">This arrangement allows your `Program.Main` and `Startup` classes to be essentially the same no matter where you deploy.</span></span>

### <a name="iis-with-kestrel"></a><span data-ttu-id="c248f-141">IIS com Kestrel</span><span class="sxs-lookup"><span data-stu-id="c248f-141">IIS with Kestrel</span></span>

<span data-ttu-id="c248f-142">Quando você usa o IIS ou IIS Express como um proxy reverso para o ASP.NET Core, o aplicativo ASP.NET Core é executado em um processo separado do processo de trabalho do IIS.</span><span class="sxs-lookup"><span data-stu-id="c248f-142">When you use IIS or IIS Express as a reverse proxy for ASP.NET Core, the ASP.NET Core application runs in a process separate from the IIS worker process.</span></span> <span data-ttu-id="c248f-143">No processo do IIS, um módulo IIS especial é executado para coordenar a relação de proxy reverso.</span><span class="sxs-lookup"><span data-stu-id="c248f-143">In the IIS process, a special IIS module runs to coordinate the reverse proxy relationship.</span></span>  <span data-ttu-id="c248f-144">Esse é o *Módulo do ASP.NET Core*.</span><span class="sxs-lookup"><span data-stu-id="c248f-144">This is the *ASP.NET Core Module*.</span></span> <span data-ttu-id="c248f-145">As funções primárias do módulo do ASP.NET Core são iniciar o aplicativo do ASP.NET Core, reiniciá-lo quando ele falhar e encaminhar o tráfego HTTP para ele.</span><span class="sxs-lookup"><span data-stu-id="c248f-145">The primary functions of the ASP.NET Core Module are to start the ASP.NET Core application, restart it when it crashes, and forward HTTP traffic to it.</span></span> <span data-ttu-id="c248f-146">Para obter mais informações, consulte [Módulo ASP.NET Core](aspnet-core-module.md).</span><span class="sxs-lookup"><span data-stu-id="c248f-146">For more information, see [ASP.NET Core Module](aspnet-core-module.md).</span></span> 

### <a name="nginx-with-kestrel"></a><span data-ttu-id="c248f-147">Nginx com Kestrel</span><span class="sxs-lookup"><span data-stu-id="c248f-147">Nginx with Kestrel</span></span>

<span data-ttu-id="c248f-148">Para obter informações sobre como usar Nginx no Linux como um servidor proxy reverso para Kestrel, consulte [Publicar em um ambiente de produção do Linux](../../publishing/linuxproduction.md).</span><span class="sxs-lookup"><span data-stu-id="c248f-148">For information about how to use Nginx on Linux as a reverse proxy server for Kestrel, see [Publish to a Linux Production Environment](../../publishing/linuxproduction.md).</span></span>

### <a name="apache-with-kestrel"></a><span data-ttu-id="c248f-149">Apache com Kestrel</span><span class="sxs-lookup"><span data-stu-id="c248f-149">Apache with Kestrel</span></span>

<span data-ttu-id="c248f-150">Para obter informações sobre como usar o Apache no Linux como um servidor proxy reverso para Kestrel, consulte [Usando o servidor Web Apache como um proxy reverso](../../publishing/apache-proxy.md).</span><span class="sxs-lookup"><span data-stu-id="c248f-150">For information about how to use Apache on Linux as a reverse proxy server for Kestrel, see [Using Apache Web Server as a reverse proxy](../../publishing/apache-proxy.md).</span></span>

## <a name="httpsys"></a><span data-ttu-id="c248f-151">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="c248f-151">HTTP.sys</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c248f-152">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c248f-152">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="c248f-153">Se você executar seu aplicativo ASP.NET Core no Windows, o HTTP. sys será uma alternativa ao Kestrel.</span><span class="sxs-lookup"><span data-stu-id="c248f-153">If you run your ASP.NET Core app on Windows, HTTP.sys is an alternative to Kestrel.</span></span> <span data-ttu-id="c248f-154">Você pode usar o HTTP. sys para cenários em que você expõe seu aplicativo à Internet e você precisa de recursos do HTTP.sys aos quais o Kestrel não dá suporte.</span><span class="sxs-lookup"><span data-stu-id="c248f-154">You can use HTTP.sys for scenarios where you expose your app to the Internet and you need HTTP.sys features that Kestrel doesn't support.</span></span> 

![O HTTP.sys se comunica diretamente com a Internet](httpsys/_static/httpsys-to-internet.png)

<span data-ttu-id="c248f-156">O HTTP.sys também pode ser usado para aplicativos que são expostos somente a uma rede interna.</span><span class="sxs-lookup"><span data-stu-id="c248f-156">HTTP.sys can also be used for applications that are exposed only to an internal network.</span></span> 

![O HTTP.sys se comunica diretamente com a rede interna](httpsys/_static/httpsys-to-internal.png)

<span data-ttu-id="c248f-158">Para cenários de rede interna, o Kestrel é geralmente recomendado para melhor desempenho; mas, em alguns cenários, você talvez queira usar um recurso oferecido apenas pelo HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="c248f-158">For internal network scenarios, Kestrel is generally recommended for best performance; but in some scenarios, you might want to use a feature that only HTTP.sys offers.</span></span> <span data-ttu-id="c248f-159">Para obter informações sobre os recursos do HTTP.sys, consulte [HTTP.sys](httpsys.md).</span><span class="sxs-lookup"><span data-stu-id="c248f-159">For information about HTTP.sys features, see [HTTP.sys](httpsys.md).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c248f-160">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c248f-160">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="c248f-161">O HTTP.sys é chamado WebListener no ASP.NET Core 1. x.</span><span class="sxs-lookup"><span data-stu-id="c248f-161">HTTP.sys is named WebListener in ASP.NET Core 1.x.</span></span> <span data-ttu-id="c248f-162">Se você executar o aplicativo ASP.NET Core no Windows, o WebListener será uma alternativa que você poderá usar para situações em que você deseje expor seu aplicativo à Internet mas não possa usar o IIS.</span><span class="sxs-lookup"><span data-stu-id="c248f-162">If you run your ASP.NET Core app on Windows, WebListener is an alternative that you can use for scenarios where you want to expose your app to the Internet but you can't use IIS.</span></span>

![O WebListener se comunica diretamente com a Internet](weblistener/_static/weblistener-to-internet.png)

<span data-ttu-id="c248f-164">O WebListener também poderá ser usado no lugar do Kestrel para aplicativos expostos somente a uma rede interna, se você precisar de recursos de WebListener para os quais o Kestrel não dê suporte.</span><span class="sxs-lookup"><span data-stu-id="c248f-164">WebListener can also be used in place of Kestrel for applications that are exposed only to an internal network, if you need WebListener features that Kestrel doesn't support.</span></span> 

![O WebListener se comunica diretamente com a rede interna](weblistener/_static/weblistener-to-internal.png)

<span data-ttu-id="c248f-166">Para cenários de rede interna, o Kestrel é geralmente recomendado para melhor desempenho; mas, em alguns cenários, você talvez queira usar um recurso oferecido apenas pelo WebListener.</span><span class="sxs-lookup"><span data-stu-id="c248f-166">For internal network scenarios, Kestrel is generally recommended for best performance, but in some scenarios you might want to use a feature that only WebListener offers.</span></span> <span data-ttu-id="c248f-167">Para obter informações sobre os recursos do WebListener, consulte [WebListener](weblistener.md).</span><span class="sxs-lookup"><span data-stu-id="c248f-167">For information about WebListener features, see [WebListener](weblistener.md).</span></span>

---

## <a name="notes-about-aspnet-core-server-infrastructure"></a><span data-ttu-id="c248f-168">Observações sobre a infraestrutura de servidor do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c248f-168">Notes about ASP.NET Core server infrastructure</span></span>

<span data-ttu-id="c248f-169">O [`IApplicationBuilder`](/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder) disponível no método `Configure` da classe `Startup` expõe a propriedade `ServerFeatures` do tipo [`IFeatureCollection`](/aspnet/core/api/microsoft.aspnetcore.http.features.ifeaturecollection).</span><span class="sxs-lookup"><span data-stu-id="c248f-169">The [`IApplicationBuilder`](/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder) available in the `Startup` class `Configure` method exposes the `ServerFeatures` property of type [`IFeatureCollection`](/aspnet/core/api/microsoft.aspnetcore.http.features.ifeaturecollection).</span></span> <span data-ttu-id="c248f-170">Kestrel e WebListener expõem apenas um único recurso, [`IServerAddressesFeature`](/aspnet/core/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature), mas diferentes implementações de servidor podem expor uma funcionalidade adicional.</span><span class="sxs-lookup"><span data-stu-id="c248f-170">Kestrel and WebListener both expose only a single feature, [`IServerAddressesFeature`](/aspnet/core/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature), but different server implementations may expose additional functionality.</span></span>

<span data-ttu-id="c248f-171">`IServerAddressesFeature` pode ser usado para descobrir a qual porta a implementação do servidor se acoplou durante o tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="c248f-171">`IServerAddressesFeature` can be used to find out which port the server implementation has bound to at runtime.</span></span>

## <a name="custom-servers"></a><span data-ttu-id="c248f-172">Servidores personalizados</span><span class="sxs-lookup"><span data-stu-id="c248f-172">Custom servers</span></span>

<span data-ttu-id="c248f-173">Se os servidores internos não atenderem às suas necessidades, você poderá criar uma implementação de servidor personalizado.</span><span class="sxs-lookup"><span data-stu-id="c248f-173">If the built-in servers don't meet your needs, you can create a custom server implementation.</span></span> <span data-ttu-id="c248f-174">O [Guia de OWIN (Open Web Interface para .NET)](../owin.md) demonstra como gravar uma implementação de [IServer](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.server.iserver) com base em [Nowin](https://github.com/Bobris/Nowin).</span><span class="sxs-lookup"><span data-stu-id="c248f-174">The [Open Web Interface for .NET (OWIN) guide](../owin.md) demonstrates how to write a [Nowin](https://github.com/Bobris/Nowin)-based [IServer](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.server.iserver) implementation.</span></span> <span data-ttu-id="c248f-175">Você pode implementar apenas as interfaces de recurso de que seu aplicativo precisa, embora você precise, no mínimo, dar suporte a [IHttpRequestFeature](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.ihttprequestfeature) e a [IHttpResponseFeature](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.ihttpresponsefeature).</span><span class="sxs-lookup"><span data-stu-id="c248f-175">You're free to implement just the feature interfaces your application needs, though at a minimum you must support [IHttpRequestFeature](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.ihttprequestfeature) and [IHttpResponseFeature](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.ihttpresponsefeature).</span></span>

## <a name="next-steps"></a><span data-ttu-id="c248f-176">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="c248f-176">Next steps</span></span>

<span data-ttu-id="c248f-177">Para obter mais informações, consulte os seguintes recursos:</span><span class="sxs-lookup"><span data-stu-id="c248f-177">For more information, see the following resources:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c248f-178">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c248f-178">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

- [<span data-ttu-id="c248f-179">Kestrel</span><span class="sxs-lookup"><span data-stu-id="c248f-179">Kestrel</span></span>](kestrel.md)
- [<span data-ttu-id="c248f-180">Kestrel com IIS</span><span class="sxs-lookup"><span data-stu-id="c248f-180">Kestrel with IIS</span></span>](aspnet-core-module.md)
- [<span data-ttu-id="c248f-181">Kestrel com Nginx</span><span class="sxs-lookup"><span data-stu-id="c248f-181">Kestrel with Nginx</span></span>](../../publishing/linuxproduction.md)
- [<span data-ttu-id="c248f-182">Kestrel com Apache</span><span class="sxs-lookup"><span data-stu-id="c248f-182">Kestrel with Apache</span></span>](../../publishing/apache-proxy.md)
- [<span data-ttu-id="c248f-183">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="c248f-183">HTTP.sys</span></span>](httpsys.md)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c248f-184">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c248f-184">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

- [<span data-ttu-id="c248f-185">Kestrel</span><span class="sxs-lookup"><span data-stu-id="c248f-185">Kestrel</span></span>](kestrel.md)
- [<span data-ttu-id="c248f-186">Kestrel com IIS</span><span class="sxs-lookup"><span data-stu-id="c248f-186">Kestrel with IIS</span></span>](aspnet-core-module.md)
- [<span data-ttu-id="c248f-187">Kestrel com Nginx</span><span class="sxs-lookup"><span data-stu-id="c248f-187">Kestrel with Nginx</span></span>](../../publishing/linuxproduction.md)
- [<span data-ttu-id="c248f-188">Kestrel com Apache</span><span class="sxs-lookup"><span data-stu-id="c248f-188">Kestrel with Apache</span></span>](../../publishing/apache-proxy.md)
- [<span data-ttu-id="c248f-189">WebListener</span><span class="sxs-lookup"><span data-stu-id="c248f-189">WebListener</span></span>](weblistener.md)

---

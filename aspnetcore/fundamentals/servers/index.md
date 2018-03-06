---
title: "Implementações de servidor Web em ASP.NET Core"
author: tdykstra
description: "Descubra os servidores Web Kestrel e HTTP.sys para ASP.NET Core. Saiba como escolher um deles e quando usá-lo com um servidor proxy reverso."
manager: wpickett
ms.author: tdykstra
ms.date: 08/03/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/index
ms.openlocfilehash: b9a7fa4e33c56a5973b4bc35f88ca0ebb3d67101
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/02/2018
---
# <a name="web-server-implementations-in-aspnet-core"></a><span data-ttu-id="6f947-104">Implementações de servidor Web em ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6f947-104">Web server implementations in ASP.NET Core</span></span>

<span data-ttu-id="6f947-105">Por [Tom Dykstra](https://github.com/tdykstra), [Steve Smith](https://ardalis.com/), [Stephen Halter](https://twitter.com/halter73) e [Chris Ross](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="6f947-105">By [Tom Dykstra](https://github.com/tdykstra), [Steve Smith](https://ardalis.com/), [Stephen Halter](https://twitter.com/halter73), and [Chris Ross](https://github.com/Tratcher)</span></span>

<span data-ttu-id="6f947-106">Um aplicativo ASP.NET Core é executado com uma implementação do servidor HTTP em processo.</span><span class="sxs-lookup"><span data-stu-id="6f947-106">An ASP.NET Core application runs with an in-process HTTP server implementation.</span></span> <span data-ttu-id="6f947-107">A implementação do servidor escuta solicitações HTTP e traz essas solicitações à tona para o aplicativo como conjuntos de [recursos de solicitação](https://docs.microsoft.com/aspnet/core/fundamentals/request-features) compostos em um `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="6f947-107">The server implementation listens for HTTP requests and surfaces them to the application as sets of [request features](https://docs.microsoft.com/aspnet/core/fundamentals/request-features) composed into an `HttpContext`.</span></span>

<span data-ttu-id="6f947-108">O ASP.NET Core envia duas implementações de servidor:</span><span class="sxs-lookup"><span data-stu-id="6f947-108">ASP.NET Core ships two server implementations:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="6f947-109">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="6f947-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

* <span data-ttu-id="6f947-110">O [Kestrel](kestrel.md) é um servidor HTTP de plataforma cruzada com base em [libuv](https://github.com/libuv/libuv), uma biblioteca de E/S assíncrona de plataforma cruzada.</span><span class="sxs-lookup"><span data-stu-id="6f947-110">[Kestrel](kestrel.md) is a cross-platform HTTP server based on [libuv](https://github.com/libuv/libuv), a cross-platform asynchronous I/O library.</span></span>

* <span data-ttu-id="6f947-111">O [HTTP.sys](httpsys.md) é um servidor HTTP somente do Windows com base no [driver do kernel Http.sys](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span><span class="sxs-lookup"><span data-stu-id="6f947-111">[HTTP.sys](httpsys.md) is a Windows-only HTTP server based on the [Http.Sys kernel driver](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="6f947-112">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="6f947-112">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

* <span data-ttu-id="6f947-113">O [Kestrel](kestrel.md) é um servidor HTTP de plataforma cruzada com base em [libuv](https://github.com/libuv/libuv), uma biblioteca de E/S assíncrona de plataforma cruzada.</span><span class="sxs-lookup"><span data-stu-id="6f947-113">[Kestrel](kestrel.md) is a cross-platform HTTP server based on [libuv](https://github.com/libuv/libuv), a cross-platform asynchronous I/O library.</span></span>

* <span data-ttu-id="6f947-114">O [WebListener](weblistener.md) é um servidor HTTP somente do Windows com base no [driver do kernel Http.sys](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span><span class="sxs-lookup"><span data-stu-id="6f947-114">[WebListener](weblistener.md) is a Windows-only HTTP server based on the [Http.Sys kernel driver](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span>

---

## <a name="kestrel"></a><span data-ttu-id="6f947-115">Kestrel</span><span class="sxs-lookup"><span data-stu-id="6f947-115">Kestrel</span></span>

<span data-ttu-id="6f947-116">O Kestrel é o servidor Web que está incluído por padrão em modelos de novo projeto do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6f947-116">Kestrel is the web server that's included by default in ASP.NET Core new-project templates.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="6f947-117">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="6f947-117">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="6f947-118">Você pode usar o Kestrel sozinho ou com um *servidor proxy reverso* como IIS, Nginx ou Apache.</span><span class="sxs-lookup"><span data-stu-id="6f947-118">You can use Kestrel by itself or with a *reverse proxy server*, such as IIS, Nginx, or Apache.</span></span> <span data-ttu-id="6f947-119">Um servidor proxy reverso recebe solicitações HTTP da Internet e as encaminha para o Kestrel após algum tratamento preliminar.</span><span class="sxs-lookup"><span data-stu-id="6f947-119">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![O Kestrel se comunica diretamente com a Internet, sem um servidor proxy reverso](kestrel/_static/kestrel-to-internet2.png)

![O Kestrel se comunica indiretamente com a Internet através de um servidor proxy reverso, tal como o IIS, o Nginx ou o Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="6f947-122">Qualquer configuração &mdash; com ou sem um servidor proxy reverso &mdash; também pode ser usada se o Kestrel é exposto somente a uma rede interna.</span><span class="sxs-lookup"><span data-stu-id="6f947-122">Either configuration &mdash; with or without a reverse proxy server &mdash; can also be used if Kestrel is exposed only to an internal network.</span></span>

<span data-ttu-id="6f947-123">Para obter informações sobre quando usar Kestrel com um proxy reverso, consulte [Introdução ao Kestrel](kestrel.md).</span><span class="sxs-lookup"><span data-stu-id="6f947-123">For information about when to use Kestrel with a reverse proxy, see [Introduction to Kestrel](kestrel.md).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="6f947-124">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="6f947-124">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="6f947-125">Se seu aplicativo aceita solicitações somente de uma rede interna, você pode usar o Kestrel sozinho.</span><span class="sxs-lookup"><span data-stu-id="6f947-125">If your application accepts requests only from an internal network, you can use Kestrel by itself.</span></span>

![O Kestrel se comunica diretamente com a rede interna](kestrel/_static/kestrel-to-internal.png)

<span data-ttu-id="6f947-127">Se você expuser seu aplicativo à Internet, você deverá usar o IIS, o Nginx ou o Apache como um *servidor proxy reverso*.</span><span class="sxs-lookup"><span data-stu-id="6f947-127">If you expose your application to the Internet, you must use IIS, Nginx, or Apache as a *reverse proxy server*.</span></span> <span data-ttu-id="6f947-128">Um servidor proxy reverso recebe solicitações HTTP da Internet e as encaminha para o Kestrel após algum tratamento preliminar, conforme mostrado no diagrama a seguir.</span><span class="sxs-lookup"><span data-stu-id="6f947-128">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling, as shown in the following diagram.</span></span>

![O Kestrel se comunica indiretamente com a Internet através de um servidor proxy reverso, tal como o IIS, o Nginx ou o Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="6f947-130">O motivo mais importante para usar um proxy reverso para implantações de borda (expostas ao tráfego da Internet) é a segurança.</span><span class="sxs-lookup"><span data-stu-id="6f947-130">The most important reason for using a reverse proxy for edge deployments (exposed to traffic from the Internet) is security.</span></span> <span data-ttu-id="6f947-131">As versões 1.x do Kestrel não têm um conjunto completo de proteção contra ataques.</span><span class="sxs-lookup"><span data-stu-id="6f947-131">The 1.x versions of Kestrel don't have a full complement of defenses against attacks.</span></span> <span data-ttu-id="6f947-132">Isso inclui mas não se limita aos tempos limite, limites de tamanho e limites de conexões simultâneas apropriados.</span><span class="sxs-lookup"><span data-stu-id="6f947-132">This includes, but isn't limited to, appropriate timeouts, size limits, and concurrent connection limits.</span></span>

<span data-ttu-id="6f947-133">Para obter informações sobre quando usar Kestrel com um proxy reverso, consulte [Introdução ao Kestrel](kestrel.md).</span><span class="sxs-lookup"><span data-stu-id="6f947-133">For information about when to use Kestrel with a reverse proxy, see [Introduction to Kestrel](kestrel.md).</span></span>

---

<span data-ttu-id="6f947-134">Você não pode usar o IIS, Nginx ou Apache sem o Kestrel ou uma [implementação de servidor personalizado](#custom-servers).</span><span class="sxs-lookup"><span data-stu-id="6f947-134">You can't use IIS, Nginx, or Apache without Kestrel or a [custom server implementation](#custom-servers).</span></span> <span data-ttu-id="6f947-135">O ASP.NET Core foi projetado para ser executado em seu próprio processo, de modo que ele pode se comportar de forma consistente entre plataformas.</span><span class="sxs-lookup"><span data-stu-id="6f947-135">ASP.NET Core was designed to run in its own process so that it can behave consistently across platforms.</span></span> <span data-ttu-id="6f947-136">Cada um entre o IIS, o Nginx e o Apache determina seu próprio processo de inicialização e ambiente; para usá-los diretamente, o ASP.NET Core teria de adaptar-se às necessidades de cada um deles.</span><span class="sxs-lookup"><span data-stu-id="6f947-136">IIS, Nginx, and Apache dictate their own startup process and environment; to use them directly, ASP.NET Core would have to adapt to the needs of each one.</span></span> <span data-ttu-id="6f947-137">Usar uma implementação do servidor Web, tal como Kestrel, oferece ao ASP.NET Core controle sobre o processo de inicialização e o ambiente.</span><span class="sxs-lookup"><span data-stu-id="6f947-137">Using a web server implementation such as Kestrel gives ASP.NET Core control over the startup process and environment.</span></span> <span data-ttu-id="6f947-138">Portanto, em vez de tentar adaptar o ASP.NET Core para o IIS, o Nginx ou o Apache, configure esses servidores Web para solicitações de proxy para Kestrel.</span><span class="sxs-lookup"><span data-stu-id="6f947-138">So rather than trying to adapt ASP.NET Core to IIS, Nginx, or Apache, you just set up those web servers to proxy requests to Kestrel.</span></span> <span data-ttu-id="6f947-139">Esse acordo permite que as classes `Program.Main` e `Startup` sejam essencialmente as mesmas, independentemente do local da implantação.</span><span class="sxs-lookup"><span data-stu-id="6f947-139">This arrangement allows your `Program.Main` and `Startup` classes to be essentially the same no matter where you deploy.</span></span>

### <a name="iis-with-kestrel"></a><span data-ttu-id="6f947-140">IIS com Kestrel</span><span class="sxs-lookup"><span data-stu-id="6f947-140">IIS with Kestrel</span></span>

<span data-ttu-id="6f947-141">Quando você usa o IIS ou IIS Express como um proxy reverso para o ASP.NET Core, o aplicativo ASP.NET Core é executado em um processo separado do processo de trabalho do IIS.</span><span class="sxs-lookup"><span data-stu-id="6f947-141">When you use IIS or IIS Express as a reverse proxy for ASP.NET Core, the ASP.NET Core application runs in a process separate from the IIS worker process.</span></span> <span data-ttu-id="6f947-142">No processo do IIS, um módulo IIS especial é executado para coordenar a relação de proxy reverso.</span><span class="sxs-lookup"><span data-stu-id="6f947-142">In the IIS process, a special IIS module runs to coordinate the reverse proxy relationship.</span></span>  <span data-ttu-id="6f947-143">Esse é o *Módulo do ASP.NET Core*.</span><span class="sxs-lookup"><span data-stu-id="6f947-143">This is the *ASP.NET Core Module*.</span></span> <span data-ttu-id="6f947-144">As funções primárias do módulo do ASP.NET Core são iniciar o aplicativo do ASP.NET Core, reiniciá-lo quando ele falhar e encaminhar o tráfego HTTP para ele.</span><span class="sxs-lookup"><span data-stu-id="6f947-144">The primary functions of the ASP.NET Core Module are to start the ASP.NET Core application, restart it when it crashes, and forward HTTP traffic to it.</span></span> <span data-ttu-id="6f947-145">Para obter mais informações, consulte [Módulo ASP.NET Core](aspnet-core-module.md).</span><span class="sxs-lookup"><span data-stu-id="6f947-145">For more information, see [ASP.NET Core Module](aspnet-core-module.md).</span></span> 

### <a name="nginx-with-kestrel"></a><span data-ttu-id="6f947-146">Nginx com Kestrel</span><span class="sxs-lookup"><span data-stu-id="6f947-146">Nginx with Kestrel</span></span>

<span data-ttu-id="6f947-147">Para obter informações sobre como usar Nginx no Linux como um servidor proxy reverso para Kestrel, consulte [Hospedar em Linux com o Nginx](xref:host-and-deploy/linux-nginx).</span><span class="sxs-lookup"><span data-stu-id="6f947-147">For information about how to use Nginx on Linux as a reverse proxy server for Kestrel, see [Host on Linux with Nginx](xref:host-and-deploy/linux-nginx).</span></span>

### <a name="apache-with-kestrel"></a><span data-ttu-id="6f947-148">Apache com Kestrel</span><span class="sxs-lookup"><span data-stu-id="6f947-148">Apache with Kestrel</span></span>

<span data-ttu-id="6f947-149">Para obter informações sobre como usar Apache no Linux como um servidor proxy reverso para Kestrel, consulte [Hospedar em Linux com o Apache](xref:host-and-deploy/linux-apache).</span><span class="sxs-lookup"><span data-stu-id="6f947-149">For information about how to use Apache on Linux as a reverse proxy server for Kestrel, see [Host on Linux with Apache](xref:host-and-deploy/linux-apache).</span></span>

## <a name="httpsys"></a><span data-ttu-id="6f947-150">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="6f947-150">HTTP.sys</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="6f947-151">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="6f947-151">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="6f947-152">Se você executar seu aplicativo ASP.NET Core no Windows, o HTTP. sys será uma alternativa ao Kestrel.</span><span class="sxs-lookup"><span data-stu-id="6f947-152">If you run your ASP.NET Core app on Windows, HTTP.sys is an alternative to Kestrel.</span></span> <span data-ttu-id="6f947-153">Você pode usar o HTTP. sys para cenários em que você expõe seu aplicativo à Internet e você precisa de recursos do HTTP.sys aos quais o Kestrel não dá suporte.</span><span class="sxs-lookup"><span data-stu-id="6f947-153">You can use HTTP.sys for scenarios where you expose your app to the Internet and you need HTTP.sys features that Kestrel doesn't support.</span></span> 

![O HTTP.sys se comunica diretamente com a Internet](httpsys/_static/httpsys-to-internet.png)

<span data-ttu-id="6f947-155">O HTTP.sys também pode ser usado para aplicativos que são expostos somente a uma rede interna.</span><span class="sxs-lookup"><span data-stu-id="6f947-155">HTTP.sys can also be used for applications that are exposed only to an internal network.</span></span> 

![O HTTP.sys se comunica diretamente com a rede interna](httpsys/_static/httpsys-to-internal.png)

<span data-ttu-id="6f947-157">Para cenários de rede interna, o Kestrel é geralmente recomendado para melhor desempenho; mas, em alguns cenários, você talvez queira usar um recurso oferecido apenas pelo HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="6f947-157">For internal network scenarios, Kestrel is generally recommended for best performance; but in some scenarios, you might want to use a feature that only HTTP.sys offers.</span></span> <span data-ttu-id="6f947-158">Para obter informações sobre os recursos do HTTP.sys, consulte [HTTP.sys](httpsys.md).</span><span class="sxs-lookup"><span data-stu-id="6f947-158">For information about HTTP.sys features, see [HTTP.sys](httpsys.md).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="6f947-159">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="6f947-159">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="6f947-160">O HTTP.sys é chamado WebListener no ASP.NET Core 1. x.</span><span class="sxs-lookup"><span data-stu-id="6f947-160">HTTP.sys is named WebListener in ASP.NET Core 1.x.</span></span> <span data-ttu-id="6f947-161">Se você executar o aplicativo ASP.NET Core no Windows, o WebListener será uma alternativa que você poderá usar para situações em que você deseje expor seu aplicativo à Internet mas não possa usar o IIS.</span><span class="sxs-lookup"><span data-stu-id="6f947-161">If you run your ASP.NET Core app on Windows, WebListener is an alternative that you can use for scenarios where you want to expose your app to the Internet but you can't use IIS.</span></span>

![O WebListener se comunica diretamente com a Internet](weblistener/_static/weblistener-to-internet.png)

<span data-ttu-id="6f947-163">O WebListener também poderá ser usado no lugar do Kestrel para aplicativos expostos somente a uma rede interna, se você precisar de recursos de WebListener para os quais o Kestrel não dê suporte.</span><span class="sxs-lookup"><span data-stu-id="6f947-163">WebListener can also be used in place of Kestrel for applications that are exposed only to an internal network, if you need WebListener features that Kestrel doesn't support.</span></span> 

![O WebListener se comunica diretamente com a rede interna](weblistener/_static/weblistener-to-internal.png)

<span data-ttu-id="6f947-165">Para cenários de rede interna, o Kestrel é geralmente recomendado para melhor desempenho; mas, em alguns cenários, você talvez queira usar um recurso oferecido apenas pelo WebListener.</span><span class="sxs-lookup"><span data-stu-id="6f947-165">For internal network scenarios, Kestrel is generally recommended for best performance, but in some scenarios you might want to use a feature that only WebListener offers.</span></span> <span data-ttu-id="6f947-166">Para obter informações sobre os recursos do WebListener, consulte [WebListener](weblistener.md).</span><span class="sxs-lookup"><span data-stu-id="6f947-166">For information about WebListener features, see [WebListener](weblistener.md).</span></span>

---

## <a name="notes-about-aspnet-core-server-infrastructure"></a><span data-ttu-id="6f947-167">Observações sobre a infraestrutura de servidor do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6f947-167">Notes about ASP.NET Core server infrastructure</span></span>

<span data-ttu-id="6f947-168">O [`IApplicationBuilder`](/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder) disponível no método `Configure` da classe `Startup` expõe a propriedade `ServerFeatures` do tipo [`IFeatureCollection`](/aspnet/core/api/microsoft.aspnetcore.http.features.ifeaturecollection).</span><span class="sxs-lookup"><span data-stu-id="6f947-168">The [`IApplicationBuilder`](/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder) available in the `Startup` class `Configure` method exposes the `ServerFeatures` property of type [`IFeatureCollection`](/aspnet/core/api/microsoft.aspnetcore.http.features.ifeaturecollection).</span></span> <span data-ttu-id="6f947-169">Kestrel e WebListener expõem apenas um único recurso, [`IServerAddressesFeature`](/aspnet/core/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature), mas diferentes implementações de servidor podem expor uma funcionalidade adicional.</span><span class="sxs-lookup"><span data-stu-id="6f947-169">Kestrel and WebListener both expose only a single feature, [`IServerAddressesFeature`](/aspnet/core/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature), but different server implementations may expose additional functionality.</span></span>

<span data-ttu-id="6f947-170">`IServerAddressesFeature` pode ser usado para descobrir a qual porta a implementação do servidor se acoplou durante o tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="6f947-170">`IServerAddressesFeature` can be used to find out which port the server implementation has bound to at runtime.</span></span>

## <a name="custom-servers"></a><span data-ttu-id="6f947-171">Servidores personalizados</span><span class="sxs-lookup"><span data-stu-id="6f947-171">Custom servers</span></span>

<span data-ttu-id="6f947-172">Se os servidores internos não atenderem às suas necessidades, você poderá criar uma implementação de servidor personalizado.</span><span class="sxs-lookup"><span data-stu-id="6f947-172">If the built-in servers don't meet your needs, you can create a custom server implementation.</span></span> <span data-ttu-id="6f947-173">O [Guia de OWIN (Open Web Interface para .NET)](../owin.md) demonstra como gravar uma implementação de [IServer](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.server.iserver) com base em [Nowin](https://github.com/Bobris/Nowin).</span><span class="sxs-lookup"><span data-stu-id="6f947-173">The [Open Web Interface for .NET (OWIN) guide](../owin.md) demonstrates how to write a [Nowin](https://github.com/Bobris/Nowin)-based [IServer](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.server.iserver) implementation.</span></span> <span data-ttu-id="6f947-174">Você pode implementar apenas as interfaces de recurso de que seu aplicativo precisa, embora você precise, no mínimo, dar suporte a [IHttpRequestFeature](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.ihttprequestfeature) e a [IHttpResponseFeature](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.ihttpresponsefeature).</span><span class="sxs-lookup"><span data-stu-id="6f947-174">You're free to implement just the feature interfaces your application needs, though at a minimum you must support [IHttpRequestFeature](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.ihttprequestfeature) and [IHttpResponseFeature](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.ihttpresponsefeature).</span></span>

## <a name="next-steps"></a><span data-ttu-id="6f947-175">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="6f947-175">Next steps</span></span>

<span data-ttu-id="6f947-176">Para obter mais informações, consulte os seguintes recursos:</span><span class="sxs-lookup"><span data-stu-id="6f947-176">For more information, see the following resources:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="6f947-177">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="6f947-177">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

- [<span data-ttu-id="6f947-178">Kestrel</span><span class="sxs-lookup"><span data-stu-id="6f947-178">Kestrel</span></span>](kestrel.md)
- [<span data-ttu-id="6f947-179">Kestrel com IIS</span><span class="sxs-lookup"><span data-stu-id="6f947-179">Kestrel with IIS</span></span>](aspnet-core-module.md)
- [<span data-ttu-id="6f947-180">Hospedar em Linux com o Nginx</span><span class="sxs-lookup"><span data-stu-id="6f947-180">Host on Linux with Nginx</span></span>](xref:host-and-deploy/linux-nginx)
- [<span data-ttu-id="6f947-181">Hospedar em Linux com o Apache</span><span class="sxs-lookup"><span data-stu-id="6f947-181">Host on Linux with Apache</span></span>](xref:host-and-deploy/linux-apache)
- [<span data-ttu-id="6f947-182">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="6f947-182">HTTP.sys</span></span>](httpsys.md)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="6f947-183">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="6f947-183">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

- [<span data-ttu-id="6f947-184">Kestrel</span><span class="sxs-lookup"><span data-stu-id="6f947-184">Kestrel</span></span>](kestrel.md)
- [<span data-ttu-id="6f947-185">Kestrel com IIS</span><span class="sxs-lookup"><span data-stu-id="6f947-185">Kestrel with IIS</span></span>](aspnet-core-module.md)
- [<span data-ttu-id="6f947-186">Hospedar em Linux com o Nginx</span><span class="sxs-lookup"><span data-stu-id="6f947-186">Host on Linux with Nginx</span></span>](xref:host-and-deploy/linux-nginx)
- [<span data-ttu-id="6f947-187">Hospedar em Linux com o Apache</span><span class="sxs-lookup"><span data-stu-id="6f947-187">Host on Linux with Apache</span></span>](xref:host-and-deploy/linux-apache)
- [<span data-ttu-id="6f947-188">WebListener</span><span class="sxs-lookup"><span data-stu-id="6f947-188">WebListener</span></span>](weblistener.md)

---

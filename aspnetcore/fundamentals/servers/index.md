---
title: Implementações de servidor Web em ASP.NET Core
author: guardrex
description: Descubra os servidores Web Kestrel e HTTP.sys para ASP.NET Core. Saiba como escolher um servidor e quando usar um servidor proxy reverso.
ms.author: tdykstra
ms.custom: mvc
ms.date: 12/01/2018
uid: fundamentals/servers/index
ms.openlocfilehash: 965d69dd071ec71d283284d58e6e1a6e78604f90
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52861350"
---
# <a name="web-server-implementations-in-aspnet-core"></a><span data-ttu-id="8d37a-104">Implementações de servidor Web em ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8d37a-104">Web server implementations in ASP.NET Core</span></span>

<span data-ttu-id="8d37a-105">Por [Tom Dykstra](https://github.com/tdykstra), [Steve Smith](https://ardalis.com/), [Stephen Halter](https://twitter.com/halter73) e [Chris Ross](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="8d37a-105">By [Tom Dykstra](https://github.com/tdykstra), [Steve Smith](https://ardalis.com/), [Stephen Halter](https://twitter.com/halter73), and [Chris Ross](https://github.com/Tratcher)</span></span>

<span data-ttu-id="8d37a-106">Um aplicativo ASP.NET Core é executado com uma implementação do servidor HTTP em processo.</span><span class="sxs-lookup"><span data-stu-id="8d37a-106">An ASP.NET Core app runs with an in-process HTTP server implementation.</span></span> <span data-ttu-id="8d37a-107">A implementação do servidor escuta solicitações HTTP e as coloca na superfície para o aplicativo como conjuntos de [recursos de solicitação](xref:fundamentals/request-features) compostos em um <xref:Microsoft.AspNetCore.Http.HttpContext>.</span><span class="sxs-lookup"><span data-stu-id="8d37a-107">The server implementation listens for HTTP requests and surfaces them to the app as sets of [request features](xref:fundamentals/request-features) composed into an <xref:Microsoft.AspNetCore.Http.HttpContext>.</span></span>

::: moniker range=">= aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="8d37a-108">Windows</span><span class="sxs-lookup"><span data-stu-id="8d37a-108">Windows</span></span>](#tab/windows)

<span data-ttu-id="8d37a-109">O ASP.NET Core vem com os seguintes itens:</span><span class="sxs-lookup"><span data-stu-id="8d37a-109">ASP.NET Core ships with the following:</span></span>

* <span data-ttu-id="8d37a-110">O [servidor Kestrel](xref:fundamentals/servers/kestrel) é o servidor HTTP padrão multiplataforma.</span><span class="sxs-lookup"><span data-stu-id="8d37a-110">[Kestrel server](xref:fundamentals/servers/kestrel) is the default, cross-platform HTTP server.</span></span>
* <span data-ttu-id="8d37a-111">O IIS HTTP Server (`IISHttpServer`) é uma implementação do [servidor em processo do IIS](xref:fundamentals/servers/aspnet-core-module#in-process-hosting-model) usada com o [Módulo do ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="8d37a-111">IIS HTTP Server (`IISHttpServer`) is an [IIS in-process server](xref:fundamentals/servers/aspnet-core-module#in-process-hosting-model) implementation used with the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span>
* <span data-ttu-id="8d37a-112">O [servidor HTTP.sys](xref:fundamentals/servers/httpsys) é um servidor HTTP somente do Windows com base no [driver do kernel HTTP.sys e na API do servidor HTTP](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span><span class="sxs-lookup"><span data-stu-id="8d37a-112">[HTTP.sys server](xref:fundamentals/servers/httpsys) is a Windows-only HTTP server based on the [HTTP.sys kernel driver and HTTP Server API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span> <span data-ttu-id="8d37a-113">O HTTP.sys é chamado [WebListener](xref:fundamentals/servers/weblistener) no ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="8d37a-113">HTTP.sys is called [WebListener](xref:fundamentals/servers/weblistener) in ASP.NET Core 1.x.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="8d37a-114">macOS</span><span class="sxs-lookup"><span data-stu-id="8d37a-114">macOS</span></span>](#tab/macos)

<span data-ttu-id="8d37a-115">O ASP.NET Core vem com o [servidor Kestrel](xref:fundamentals/servers/kestrel), que é o servidor HTTP padrão multiplataforma.</span><span class="sxs-lookup"><span data-stu-id="8d37a-115">ASP.NET Core ships with [Kestrel server](xref:fundamentals/servers/kestrel), which is the default, cross-platform HTTP server.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="8d37a-116">Linux</span><span class="sxs-lookup"><span data-stu-id="8d37a-116">Linux</span></span>](#tab/linux)

<span data-ttu-id="8d37a-117">O ASP.NET Core vem com o [servidor Kestrel](xref:fundamentals/servers/kestrel), que é o servidor HTTP padrão multiplataforma.</span><span class="sxs-lookup"><span data-stu-id="8d37a-117">ASP.NET Core ships with [Kestrel server](xref:fundamentals/servers/kestrel), which is the default, cross-platform HTTP server.</span></span>

---

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="8d37a-118">Windows</span><span class="sxs-lookup"><span data-stu-id="8d37a-118">Windows</span></span>](#tab/windows)

<span data-ttu-id="8d37a-119">O ASP.NET Core vem com os seguintes itens:</span><span class="sxs-lookup"><span data-stu-id="8d37a-119">ASP.NET Core ships with the following:</span></span>

* <span data-ttu-id="8d37a-120">O [servidor Kestrel](xref:fundamentals/servers/kestrel) é o servidor HTTP padrão multiplataforma.</span><span class="sxs-lookup"><span data-stu-id="8d37a-120">[Kestrel server](xref:fundamentals/servers/kestrel) is the default, cross-platform HTTP server.</span></span>
* <span data-ttu-id="8d37a-121">O [servidor HTTP.sys](xref:fundamentals/servers/httpsys) é um servidor HTTP somente do Windows com base no [driver do kernel HTTP.sys e na API do servidor HTTP](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span><span class="sxs-lookup"><span data-stu-id="8d37a-121">[HTTP.sys server](xref:fundamentals/servers/httpsys) is a Windows-only HTTP server based on the [HTTP.sys kernel driver and HTTP Server API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span> <span data-ttu-id="8d37a-122">O HTTP.sys é chamado [WebListener](xref:fundamentals/servers/weblistener) no ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="8d37a-122">HTTP.sys is called [WebListener](xref:fundamentals/servers/weblistener) in ASP.NET Core 1.x.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="8d37a-123">macOS</span><span class="sxs-lookup"><span data-stu-id="8d37a-123">macOS</span></span>](#tab/macos)

<span data-ttu-id="8d37a-124">O ASP.NET Core vem com o [servidor Kestrel](xref:fundamentals/servers/kestrel), que é o servidor HTTP padrão multiplataforma.</span><span class="sxs-lookup"><span data-stu-id="8d37a-124">ASP.NET Core ships with [Kestrel server](xref:fundamentals/servers/kestrel), which is the default, cross-platform HTTP server.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="8d37a-125">Linux</span><span class="sxs-lookup"><span data-stu-id="8d37a-125">Linux</span></span>](#tab/linux)

<span data-ttu-id="8d37a-126">O ASP.NET Core vem com o [servidor Kestrel](xref:fundamentals/servers/kestrel), que é o servidor HTTP padrão multiplataforma.</span><span class="sxs-lookup"><span data-stu-id="8d37a-126">ASP.NET Core ships with [Kestrel server](xref:fundamentals/servers/kestrel), which is the default, cross-platform HTTP server.</span></span>

---

::: moniker-end

## <a name="kestrel"></a><span data-ttu-id="8d37a-127">Kestrel</span><span class="sxs-lookup"><span data-stu-id="8d37a-127">Kestrel</span></span>

<span data-ttu-id="8d37a-128">O Kestrel é o servidor Web padrão incluído nos modelos de projeto do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="8d37a-128">Kestrel is the default web server included in ASP.NET Core project templates.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="8d37a-129">O Kestrel pode ser usado:</span><span class="sxs-lookup"><span data-stu-id="8d37a-129">Kestrel can be used:</span></span>

* <span data-ttu-id="8d37a-130">Sozinho, como um servidor de borda que processa solicitações diretamente de uma rede, incluindo a Internet.</span><span class="sxs-lookup"><span data-stu-id="8d37a-130">By itself as an edge server processing requests directly from a network, including the Internet.</span></span>
* <span data-ttu-id="8d37a-131">Com um *servidor proxy reverso* como [IIS (Serviços de Informações da Internet)](https://www.iis.net/), [Nginx](http://nginx.org) ou [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="8d37a-131">With a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](http://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="8d37a-132">Um servidor proxy reverso recebe solicitações HTTP da Internet e encaminha-as para o Kestrel.</span><span class="sxs-lookup"><span data-stu-id="8d37a-132">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel.</span></span>

![O Kestrel se comunica diretamente com a Internet, sem um servidor proxy reverso](kestrel/_static/kestrel-to-internet2.png)

![O Kestrel se comunica indiretamente com a Internet através de um servidor proxy reverso, tal como o IIS, o Nginx ou o Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="8d37a-135">Qualquer configuração &mdash;com ou sem um servidor proxy reverso&mdash; é uma configuração de hospedagem válida e compatível com o ASP.NET Core 2.0 ou aplicativos posteriores.</span><span class="sxs-lookup"><span data-stu-id="8d37a-135">Either configuration&mdash;with or without a reverse proxy server&mdash;is a valid and supported hosting configuration for ASP.NET Core 2.0 or later apps.</span></span> <span data-ttu-id="8d37a-136">Para obter mais informações, consulte [Quando usar Kestrel com um proxy reverso](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span><span class="sxs-lookup"><span data-stu-id="8d37a-136">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="8d37a-137">Se o aplicativo aceita somente solicitações de uma rede interna, o Kestrel pode ser usado sozinho.</span><span class="sxs-lookup"><span data-stu-id="8d37a-137">If the app only accepts requests from an internal network, Kestrel can be used by itself.</span></span>

![O Kestrel se comunica diretamente com a rede interna](kestrel/_static/kestrel-to-internal.png)

<span data-ttu-id="8d37a-139">Se o aplicativo for exposto à Internet, o Kestrel deverá usar um *servidor proxy reverso*, como o [IIS (Serviços de Informações da Internet)](https://www.iis.net/), [Nginx](http://nginx.org) ou [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="8d37a-139">If the app is exposed to the Internet, Kestrel must use a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](http://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="8d37a-140">Um servidor proxy reverso recebe solicitações HTTP da Internet e encaminha-as para o Kestrel.</span><span class="sxs-lookup"><span data-stu-id="8d37a-140">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel.</span></span>

![O Kestrel se comunica indiretamente com a Internet através de um servidor proxy reverso, tal como o IIS, o Nginx ou o Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="8d37a-142">O motivo mais importante para usar um proxy reverso para implantações de servidores de borda voltados para o público que são expostas diretamente na Internet é a segurança.</span><span class="sxs-lookup"><span data-stu-id="8d37a-142">The most important reason for using a reverse proxy for public-facing edge server deployments that are exposed directly the Internet is security.</span></span> <span data-ttu-id="8d37a-143">As versões 1.x do Kestrel não incluem recursos de segurança importantes para proteção contra ataques da Internet.</span><span class="sxs-lookup"><span data-stu-id="8d37a-143">The 1.x versions of Kestrel don't include important security features to defend against attacks from the Internet.</span></span> <span data-ttu-id="8d37a-144">Isso inclui, mas não se limita aos tempos limite, limites de tamanho da solicitação e limites de conexões simultâneas apropriados.</span><span class="sxs-lookup"><span data-stu-id="8d37a-144">This includes, but isn't limited to, appropriate timeouts, request size limits, and concurrent connection limits.</span></span>

<span data-ttu-id="8d37a-145">Para obter mais informações, consulte [Quando usar Kestrel com um proxy reverso](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span><span class="sxs-lookup"><span data-stu-id="8d37a-145">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

::: moniker-end

### <a name="iis-with-kestrel"></a><span data-ttu-id="8d37a-146">IIS com Kestrel</span><span class="sxs-lookup"><span data-stu-id="8d37a-146">IIS with Kestrel</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="8d37a-147">Ao usar o [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) ou o [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), o aplicativo do ASP.NET Core é executado no mesmo processo que o processo de trabalho do IIS (o modelo de hospedagem *em processo*) ou em um processo de trabalho separado do IIS (o modelo de hospedagem *fora do processo*).</span><span class="sxs-lookup"><span data-stu-id="8d37a-147">When using [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), the ASP.NET Core app either runs in the same process as the IIS worker process (the *in-process* hosting model) or in a process separate from the IIS worker process (the *out-of-process* hosting model).</span></span>

<span data-ttu-id="8d37a-148">O [Módulo do ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) é um módulo nativo do IIS que manipula solicitações nativas do IIS entre o servidor HTTP em processo do IIS ou o servidor Kestrel fora do processo.</span><span class="sxs-lookup"><span data-stu-id="8d37a-148">The [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) is a native IIS module that handles native IIS requests between either the in-process IIS HTTP Server or the out-of-process Kestrel server.</span></span> <span data-ttu-id="8d37a-149">Para obter mais informações, consulte <xref:fundamentals/servers/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="8d37a-149">For more information, see <xref:fundamentals/servers/aspnet-core-module>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="8d37a-150">Ao usas o [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) ou [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) como um proxy reverso para o ASP.NET Core, o aplicativo ASP.NET Core é executado em um processo separado do processo de trabalho do IIS.</span><span class="sxs-lookup"><span data-stu-id="8d37a-150">When using [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) as a reverse proxy for ASP.NET Core, the ASP.NET Core app runs in a process separate from the IIS worker process.</span></span> <span data-ttu-id="8d37a-151">No processo do IIS, o [Módulo do ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) coordena a relação do proxy reverso.</span><span class="sxs-lookup"><span data-stu-id="8d37a-151">In the IIS process, the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) coordinates the reverse proxy relationship.</span></span> <span data-ttu-id="8d37a-152">As funções primárias do módulo do ASP.NET Core são iniciar o aplicativo, reiniciá-lo quando ele falhar e encaminhar o tráfego HTTP para o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="8d37a-152">The primary functions of the ASP.NET Core Module are to start the app, restart the app when it crashes, and forward HTTP traffic to the app.</span></span> <span data-ttu-id="8d37a-153">Para obter mais informações, consulte <xref:fundamentals/servers/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="8d37a-153">For more information, see <xref:fundamentals/servers/aspnet-core-module>.</span></span>

::: moniker-end

### <a name="nginx-with-kestrel"></a><span data-ttu-id="8d37a-154">Nginx com Kestrel</span><span class="sxs-lookup"><span data-stu-id="8d37a-154">Nginx with Kestrel</span></span>

<span data-ttu-id="8d37a-155">Para obter informações sobre como usar Nginx no Linux como um servidor proxy reverso para Kestrel, confira <xref:host-and-deploy/linux-nginx>.</span><span class="sxs-lookup"><span data-stu-id="8d37a-155">For information on how to use Nginx on Linux as a reverse proxy server for Kestrel, see <xref:host-and-deploy/linux-nginx>.</span></span>

### <a name="apache-with-kestrel"></a><span data-ttu-id="8d37a-156">Apache com Kestrel</span><span class="sxs-lookup"><span data-stu-id="8d37a-156">Apache with Kestrel</span></span>

<span data-ttu-id="8d37a-157">Para obter informações sobre como usar Apache no Linux como um servidor proxy reverso para Kestrel, confira <xref:host-and-deploy/linux-apache>.</span><span class="sxs-lookup"><span data-stu-id="8d37a-157">For information on how to use Apache on Linux as a reverse proxy server for Kestrel, see <xref:host-and-deploy/linux-apache>.</span></span>

## <a name="httpsys"></a><span data-ttu-id="8d37a-158">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="8d37a-158">HTTP.sys</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="8d37a-159">Se os aplicativos ASP.NET Core forem executados no Windows, o HTTP.sys será uma alternativa ao Kestrel.</span><span class="sxs-lookup"><span data-stu-id="8d37a-159">If ASP.NET Core apps are run on Windows, HTTP.sys is an alternative to Kestrel.</span></span> <span data-ttu-id="8d37a-160">O Kestrel geralmente é recomendado para melhor desempenho.</span><span class="sxs-lookup"><span data-stu-id="8d37a-160">Kestrel is generally recommended for best performance.</span></span> <span data-ttu-id="8d37a-161">O HTTP.sys pode ser usado em cenários em que o aplicativo é exposto à Internet e as funcionalidades necessárias são compatíveis com HTTP.sys, mas não com Kestrel.</span><span class="sxs-lookup"><span data-stu-id="8d37a-161">HTTP.sys can be used in scenarios where the app is exposed to the Internet and required capabilities are supported by HTTP.sys but not Kestrel.</span></span> <span data-ttu-id="8d37a-162">Para obter mais informações, consulte <xref:fundamentals/servers/httpsys>.</span><span class="sxs-lookup"><span data-stu-id="8d37a-162">For more information, see <xref:fundamentals/servers/httpsys>.</span></span>

![O HTTP.sys se comunica diretamente com a Internet](httpsys/_static/httpsys-to-internet.png)

<span data-ttu-id="8d37a-164">O HTTP.sys também pode ser usado para aplicativos que são expostos somente a uma rede interna.</span><span class="sxs-lookup"><span data-stu-id="8d37a-164">HTTP.sys can also be used for apps that are only exposed to an internal network.</span></span>

![O HTTP.sys se comunica diretamente com a rede interna](httpsys/_static/httpsys-to-internal.png)

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="8d37a-166">O HTTP.sys é chamado [WebListener](xref:fundamentals/servers/weblistener) no ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="8d37a-166">HTTP.sys is named [WebListener](xref:fundamentals/servers/weblistener) in ASP.NET Core 1.x.</span></span> <span data-ttu-id="8d37a-167">Se os aplicativos ASP.NET Core forem executados no Windows, o WebListener será uma alternativa para cenários em que o IIS não está disponível para hospedar aplicativos.</span><span class="sxs-lookup"><span data-stu-id="8d37a-167">If ASP.NET Core apps are run on Windows, WebListener is an alternative for scenarios where IIS isn't available to host apps.</span></span>

![O WebListener se comunica diretamente com a Internet](weblistener/_static/weblistener-to-internet.png)

<span data-ttu-id="8d37a-169">O WebListener também poderá ser usado no lugar do Kestrel para aplicativos expostos somente a uma rede interna se as funcionalidades necessárias forem compatíveis com o WebListener, mas não com o Kestrel.</span><span class="sxs-lookup"><span data-stu-id="8d37a-169">WebListener can also be used in place of Kestrel for apps that are only exposed to an internal network, if required capabilities are supported by WebListener but not Kestrel.</span></span> <span data-ttu-id="8d37a-170">Para obter mais informações sobre o WebListener, consulte [WebListener](xref:fundamentals/servers/weblistener).</span><span class="sxs-lookup"><span data-stu-id="8d37a-170">For information on WebListener, see [WebListener](xref:fundamentals/servers/weblistener).</span></span>

![O WebListener se comunica diretamente com a rede interna](weblistener/_static/weblistener-to-internal.png)

::: moniker-end

## <a name="aspnet-core-server-infrastructure"></a><span data-ttu-id="8d37a-172">Infraestrutura de servidor do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8d37a-172">ASP.NET Core server infrastructure</span></span>

<span data-ttu-id="8d37a-173">O [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) disponível no método `Startup.Configure` expõe a propriedade [ServerFeatures](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.serverfeatures) do tipo [IFeatureCollection](/dotnet/api/microsoft.aspnetcore.http.features.ifeaturecollection).</span><span class="sxs-lookup"><span data-stu-id="8d37a-173">The [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) available in the `Startup.Configure` method exposes the [ServerFeatures](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.serverfeatures) property of type [IFeatureCollection](/dotnet/api/microsoft.aspnetcore.http.features.ifeaturecollection).</span></span> <span data-ttu-id="8d37a-174">Kestrel e HTTP.sys (WebListener no ASP.NET Core 1.x) expõem apenas um único recurso cada, [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature), mas as implementações de servidor diferentes podem expor funcionalidade adicional.</span><span class="sxs-lookup"><span data-stu-id="8d37a-174">Kestrel and HTTP.sys (WebListener in ASP.NET Core 1.x) only expose a single feature each, [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature), but different server implementations may expose additional functionality.</span></span>

<span data-ttu-id="8d37a-175">`IServerAddressesFeature` pode ser usado para descobrir a qual porta a implementação do servidor se acoplou durante o tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="8d37a-175">`IServerAddressesFeature` can be used to find out which port the server implementation has bound at runtime.</span></span>

## <a name="custom-servers"></a><span data-ttu-id="8d37a-176">Servidores personalizados</span><span class="sxs-lookup"><span data-stu-id="8d37a-176">Custom servers</span></span>

<span data-ttu-id="8d37a-177">Se os servidores internos não atenderem aos requisitos do aplicativo, um servidor personalizado poderá ser criado.</span><span class="sxs-lookup"><span data-stu-id="8d37a-177">If the built-in servers don't meet the app's requirements, a custom server implementation can be created.</span></span> <span data-ttu-id="8d37a-178">O [Guia de OWIN (Open Web Interface para .NET)](xref:fundamentals/owin) demonstra como gravar uma implementação de [IServer](/dotnet/api/microsoft.aspnetcore.hosting.server.iserver) com base em [Nowin](https://github.com/Bobris/Nowin).</span><span class="sxs-lookup"><span data-stu-id="8d37a-178">The [Open Web Interface for .NET (OWIN) guide](xref:fundamentals/owin) demonstrates how to write a [Nowin](https://github.com/Bobris/Nowin)-based [IServer](/dotnet/api/microsoft.aspnetcore.hosting.server.iserver) implementation.</span></span> <span data-ttu-id="8d37a-179">Somente as interfaces de recurso que o aplicativo usa exigem implementação. Porém, no mínimo [IHttpRequestFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttprequestfeature) e [IHttpResponseFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttpresponsefeature) devem ter suporte.</span><span class="sxs-lookup"><span data-stu-id="8d37a-179">Only the feature interfaces that the app uses require implementation, though at a minimum [IHttpRequestFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttprequestfeature) and [IHttpResponseFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttpresponsefeature) must be supported.</span></span>

## <a name="server-startup"></a><span data-ttu-id="8d37a-180">Inicialização do servidor</span><span class="sxs-lookup"><span data-stu-id="8d37a-180">Server startup</span></span>

<span data-ttu-id="8d37a-181">Ao usar o [Visual Studio](https://www.visualstudio.com/vs/), o [Visual Studio para Mac](https://www.visualstudio.com/vs/mac/) ou o [Visual Studio Code](https://code.visualstudio.com/), o servidor é iniciado quando o aplicativo é iniciado pelo IDE (ambiente de desenvolvimento integrado).</span><span class="sxs-lookup"><span data-stu-id="8d37a-181">When using [Visual Studio](https://www.visualstudio.com/vs/), [Visual Studio for Mac](https://www.visualstudio.com/vs/mac/), or [Visual Studio Code](https://code.visualstudio.com/), the server is launched when the app is started by the Integrated Development Environment (IDE).</span></span> <span data-ttu-id="8d37a-182">No Visual Studio no Windows, os perfis de inicialização podem ser usados para iniciar o aplicativo e o servidor com o [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)/[Módulo do ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) ou o console.</span><span class="sxs-lookup"><span data-stu-id="8d37a-182">In Visual Studio on Windows, launch profiles can be used to start the app and server with either [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)/[ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) or the console.</span></span> <span data-ttu-id="8d37a-183">No Visual Studio Code, o aplicativo e o servidor são iniciados por [Omnisharp](https://github.com/OmniSharp/omnisharp-vscode), que ativa o depurador CoreCLR.</span><span class="sxs-lookup"><span data-stu-id="8d37a-183">In Visual Studio Code, the app and server are started by [Omnisharp](https://github.com/OmniSharp/omnisharp-vscode), which activates the CoreCLR debugger.</span></span> <span data-ttu-id="8d37a-184">Usando o Visual Studio para Mac, o aplicativo e o servidor são iniciados pelo [depurador de modo reversível Mono](http://www.mono-project.com/docs/advanced/runtime/docs/soft-debugger/).</span><span class="sxs-lookup"><span data-stu-id="8d37a-184">Using Visual Studio for Mac, the app and server are started by the [Mono Soft-Mode Debugger](http://www.mono-project.com/docs/advanced/runtime/docs/soft-debugger/).</span></span>

<span data-ttu-id="8d37a-185">Ao iniciar um aplicativo com um prompt de comando na pasta do projeto, a [execução dotnet](/dotnet/core/tools/dotnet-run) inicia o aplicativo e o servidor (somente no Kestrel e no HTTP.sys).</span><span class="sxs-lookup"><span data-stu-id="8d37a-185">When launching an app from a command prompt in the project's folder, [dotnet run](/dotnet/core/tools/dotnet-run) launches the app and server (Kestrel and HTTP.sys only).</span></span> <span data-ttu-id="8d37a-186">A configuração é especificada pela opção `-c|--configuration`, que é definida como `Debug` (padrão) ou `Release`.</span><span class="sxs-lookup"><span data-stu-id="8d37a-186">The configuration is specified by the `-c|--configuration` option, which is set to either `Debug` (default) or `Release`.</span></span> <span data-ttu-id="8d37a-187">Se os perfis de inicialização estiverem presentes em um arquivo *launchSettings.json*, use a opção `--launch-profile <NAME>` para definir o perfil de inicialização (por exemplo, `Development` ou `Production`).</span><span class="sxs-lookup"><span data-stu-id="8d37a-187">If launch profiles are present in a *launchSettings.json* file, use the `--launch-profile <NAME>` option to set the launch profile (for example, `Development` or `Production`).</span></span> <span data-ttu-id="8d37a-188">Para obter mais informações, veja os tópicos [execução dotnet](/dotnet/core/tools/dotnet-run) e [Empacotamento de distribuição do .NET Core](/dotnet/core/build/distribution-packaging).</span><span class="sxs-lookup"><span data-stu-id="8d37a-188">For more information, see the [dotnet run](/dotnet/core/tools/dotnet-run) and [.NET Core distribution packaging](/dotnet/core/build/distribution-packaging) topics.</span></span>

## <a name="http2-support"></a><span data-ttu-id="8d37a-189">Compatibilidade com HTTP/2</span><span class="sxs-lookup"><span data-stu-id="8d37a-189">HTTP/2 support</span></span>

<span data-ttu-id="8d37a-190">O [HTTP/2](https://httpwg.org/specs/rfc7540.html) é compatível com ASP.NET Core nos seguintes cenários de implantação:</span><span class="sxs-lookup"><span data-stu-id="8d37a-190">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is supported with ASP.NET Core in the following deployment scenarios:</span></span>

::: moniker range=">= aspnetcore-2.2"

* [<span data-ttu-id="8d37a-191">Kestrel</span><span class="sxs-lookup"><span data-stu-id="8d37a-191">Kestrel</span></span>](xref:fundamentals/servers/kestrel#http2-support)
  * <span data-ttu-id="8d37a-192">Sistema operacional</span><span class="sxs-lookup"><span data-stu-id="8d37a-192">Operating system</span></span>
    * <span data-ttu-id="8d37a-193">Windows Server 2016/Windows 10 ou posterior&dagger;</span><span class="sxs-lookup"><span data-stu-id="8d37a-193">Windows Server 2016/Windows 10 or later&dagger;</span></span>
    * <span data-ttu-id="8d37a-194">Linux com OpenSSL 1.0.2 ou posterior (por exemplo, Ubuntu 16.04 ou posterior)</span><span class="sxs-lookup"><span data-stu-id="8d37a-194">Linux with OpenSSL 1.0.2 or later (for example, Ubuntu 16.04 or later)</span></span>
    * <span data-ttu-id="8d37a-195">O HTTP/2 será compatível com macOS em uma versão futura.</span><span class="sxs-lookup"><span data-stu-id="8d37a-195">HTTP/2 will be supported on macOS in a future release.</span></span>
  * <span data-ttu-id="8d37a-196">Estrutura de destino: .NET Core 2.2 ou posterior</span><span class="sxs-lookup"><span data-stu-id="8d37a-196">Target framework: .NET Core 2.2 or later</span></span>
* [<span data-ttu-id="8d37a-197">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="8d37a-197">HTTP.sys</span></span>](xref:fundamentals/servers/httpsys#http2-support)
  * <span data-ttu-id="8d37a-198">Windows Server 2016/Windows 10 ou posterior</span><span class="sxs-lookup"><span data-stu-id="8d37a-198">Windows Server 2016/Windows 10 or later</span></span>
  * <span data-ttu-id="8d37a-199">Estrutura de destino: não aplicável a implantações de HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="8d37a-199">Target framework: Not applicable to HTTP.sys deployments.</span></span>
* [<span data-ttu-id="8d37a-200">IIS (em processo)</span><span class="sxs-lookup"><span data-stu-id="8d37a-200">IIS (in-process)</span></span>](xref:host-and-deploy/iis/index#http2-support)
  * <span data-ttu-id="8d37a-201">Windows Server 2016/Windows 10 ou posterior; IIS 10 ou posterior</span><span class="sxs-lookup"><span data-stu-id="8d37a-201">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="8d37a-202">Estrutura de destino: .NET Core 2.2 ou posterior</span><span class="sxs-lookup"><span data-stu-id="8d37a-202">Target framework: .NET Core 2.2 or later</span></span>
* [<span data-ttu-id="8d37a-203">IIS (fora do processo)</span><span class="sxs-lookup"><span data-stu-id="8d37a-203">IIS (out-of-process)</span></span>](xref:host-and-deploy/iis/index#http2-support)
  * <span data-ttu-id="8d37a-204">Windows Server 2016/Windows 10 ou posterior; IIS 10 ou posterior</span><span class="sxs-lookup"><span data-stu-id="8d37a-204">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="8d37a-205">Conexões de servidor de borda voltadas para o público usam HTTP/2, mas a conexão de proxy reverso para o Kestrel usa HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="8d37a-205">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to Kestrel uses HTTP/1.1.</span></span>
  * <span data-ttu-id="8d37a-206">Estrutura de destino: não aplicável a implantações IIS fora de processo.</span><span class="sxs-lookup"><span data-stu-id="8d37a-206">Target framework: Not applicable to IIS out-of-process deployments.</span></span>

<span data-ttu-id="8d37a-207">&dagger;O Kestrel tem suporte limitado para HTTP/2 no Windows Server 2012 R2 e Windows 8.1.</span><span class="sxs-lookup"><span data-stu-id="8d37a-207">&dagger;Kestrel has limited support for HTTP/2 on Windows Server 2012 R2 and Windows 8.1.</span></span> <span data-ttu-id="8d37a-208">O suporte é limitado porque a lista de conjuntos de codificação TLS disponível nesses sistemas operacionais é limitada.</span><span class="sxs-lookup"><span data-stu-id="8d37a-208">Support is limited because the list of supported TLS cipher suites available on these operating systems is limited.</span></span> <span data-ttu-id="8d37a-209">Um certificado gerado usando um ECDSA (Algoritmo de Assinatura Digital Curva Elíptica) pode ser necessário para proteger conexões TLS.</span><span class="sxs-lookup"><span data-stu-id="8d37a-209">A certificate generated using an Elliptic Curve Digital Signature Algorithm (ECDSA) may be required to secure TLS connections.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* [<span data-ttu-id="8d37a-210">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="8d37a-210">HTTP.sys</span></span>](xref:fundamentals/servers/httpsys#http2-support)
  * <span data-ttu-id="8d37a-211">Windows Server 2016/Windows 10 ou posterior</span><span class="sxs-lookup"><span data-stu-id="8d37a-211">Windows Server 2016/Windows 10 or later</span></span>
  * <span data-ttu-id="8d37a-212">Estrutura de destino: não aplicável a implantações de HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="8d37a-212">Target framework: Not applicable to HTTP.sys deployments.</span></span>
* [<span data-ttu-id="8d37a-213">IIS (fora do processo)</span><span class="sxs-lookup"><span data-stu-id="8d37a-213">IIS (out-of-process)</span></span>](xref:host-and-deploy/iis/index#http2-support)
  * <span data-ttu-id="8d37a-214">Windows Server 2016/Windows 10 ou posterior; IIS 10 ou posterior</span><span class="sxs-lookup"><span data-stu-id="8d37a-214">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="8d37a-215">Conexões de servidor de borda voltadas para o público usam HTTP/2, mas a conexão de proxy reverso para o Kestrel usa HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="8d37a-215">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to Kestrel uses HTTP/1.1.</span></span>
  * <span data-ttu-id="8d37a-216">Estrutura de destino: não aplicável a implantações IIS fora de processo.</span><span class="sxs-lookup"><span data-stu-id="8d37a-216">Target framework: Not applicable to IIS out-of-process deployments.</span></span>

::: moniker-end

<span data-ttu-id="8d37a-217">Uma conexão HTTP/2 precisa usar [ALPN (Application-Layer Protocol Negotiation)](https://tools.ietf.org/html/rfc7301#section-3) e TLS 1.2 ou posterior.</span><span class="sxs-lookup"><span data-stu-id="8d37a-217">An HTTP/2 connection must use [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) and TLS 1.2 or later.</span></span> <span data-ttu-id="8d37a-218">Para obter mais informações, consulte os tópicos referentes aos seus cenários de implantação do servidor.</span><span class="sxs-lookup"><span data-stu-id="8d37a-218">For more information, see the topics that pertain to your server deployment scenarios.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8d37a-219">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="8d37a-219">Additional resources</span></span>

* <xref:fundamentals/servers/kestrel>
* <xref:fundamentals/servers/aspnet-core-module>
* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/linux-nginx>
* <xref:host-and-deploy/linux-apache>
* <span data-ttu-id="8d37a-220"><xref:fundamentals/servers/httpsys> (para ASP.NET Core 1.x, consulte <xref:fundamentals/servers/weblistener>)</span><span class="sxs-lookup"><span data-stu-id="8d37a-220"><xref:fundamentals/servers/httpsys> (for ASP.NET Core 1.x, see <xref:fundamentals/servers/weblistener>)</span></span>

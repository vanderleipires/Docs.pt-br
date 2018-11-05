---
title: Implementações de servidor Web em ASP.NET Core
author: rick-anderson
description: Descubra os servidores Web Kestrel e HTTP.sys para ASP.NET Core. Saiba como escolher um servidor e quando usar um servidor proxy reverso.
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/21/2018
uid: fundamentals/servers/index
ms.openlocfilehash: 6b6ebbe9d31d571ea470fba0989d622dcf6e68af
ms.sourcegitcommit: fc2486ddbeb15ab4969168d99b3fe0fbe91e8661
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/01/2018
ms.locfileid: "50758200"
---
# <a name="web-server-implementations-in-aspnet-core"></a><span data-ttu-id="fd9ab-104">Implementações de servidor Web em ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fd9ab-104">Web server implementations in ASP.NET Core</span></span>

<span data-ttu-id="fd9ab-105">Por [Tom Dykstra](https://github.com/tdykstra), [Steve Smith](https://ardalis.com/), [Stephen Halter](https://twitter.com/halter73) e [Chris Ross](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="fd9ab-105">By [Tom Dykstra](https://github.com/tdykstra), [Steve Smith](https://ardalis.com/), [Stephen Halter](https://twitter.com/halter73), and [Chris Ross](https://github.com/Tratcher)</span></span>

<span data-ttu-id="fd9ab-106">Um aplicativo ASP.NET Core é executado com uma implementação do servidor HTTP em processo.</span><span class="sxs-lookup"><span data-stu-id="fd9ab-106">An ASP.NET Core app runs with an in-process HTTP server implementation.</span></span> <span data-ttu-id="fd9ab-107">A implementação do servidor escuta solicitações HTTP e as coloca na superfície para o aplicativo como conjuntos de [recursos de solicitação](xref:fundamentals/request-features) compostos em um <xref:Microsoft.AspNetCore.Http.HttpContext>.</span><span class="sxs-lookup"><span data-stu-id="fd9ab-107">The server implementation listens for HTTP requests and surfaces them to the app as sets of [request features](xref:fundamentals/request-features) composed into an <xref:Microsoft.AspNetCore.Http.HttpContext>.</span></span>

<span data-ttu-id="fd9ab-108">O ASP.NET Core é fornecido com as seguintes implementações de servidor:</span><span class="sxs-lookup"><span data-stu-id="fd9ab-108">ASP.NET Core ships with the following server implementations:</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="fd9ab-109">O [Kestrel](xref:fundamentals/servers/kestrel) é o servidor HTTP de plataforma cruzada padrão para o ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="fd9ab-109">[Kestrel](xref:fundamentals/servers/kestrel) is the default, cross-platform HTTP server for ASP.NET Core.</span></span>
* <span data-ttu-id="fd9ab-110">`IISHttpServer` é usado com o [modelo de hospedagem em processo](xref:fundamentals/servers/aspnet-core-module#in-process-hosting-model) e o [Módulo do ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) no Windows.</span><span class="sxs-lookup"><span data-stu-id="fd9ab-110">`IISHttpServer` is used with the [in-process hosting model](xref:fundamentals/servers/aspnet-core-module#in-process-hosting-model) and the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) on Windows.</span></span>
* <span data-ttu-id="fd9ab-111">O [HTTP.sys](xref:fundamentals/servers/httpsys) é um servidor HTTP somente do Windows com base no [driver do kernel HTTP.sys e na API do servidor HTTP](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span><span class="sxs-lookup"><span data-stu-id="fd9ab-111">[HTTP.sys](xref:fundamentals/servers/httpsys) is a Windows-only HTTP server based on the [HTTP.sys kernel driver and HTTP Server API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span> <span data-ttu-id="fd9ab-112">(O HTTP.sys é chamado [WebListener](xref:fundamentals/servers/weblistener) no ASP.NET Core 1.x.)</span><span class="sxs-lookup"><span data-stu-id="fd9ab-112">(HTTP.sys is called [WebListener](xref:fundamentals/servers/weblistener) in ASP.NET Core 1.x.)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="fd9ab-113">O [Kestrel](xref:fundamentals/servers/kestrel) é o servidor HTTP de plataforma cruzada padrão para o ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="fd9ab-113">[Kestrel](xref:fundamentals/servers/kestrel) is the default, cross-platform HTTP server for ASP.NET Core.</span></span>
* <span data-ttu-id="fd9ab-114">O [HTTP.sys](xref:fundamentals/servers/httpsys) é um servidor HTTP somente do Windows com base no [driver do kernel HTTP.sys e na API do servidor HTTP](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span><span class="sxs-lookup"><span data-stu-id="fd9ab-114">[HTTP.sys](xref:fundamentals/servers/httpsys) is a Windows-only HTTP server based on the [HTTP.sys kernel driver and HTTP Server API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span> <span data-ttu-id="fd9ab-115">(O HTTP.sys é chamado [WebListener](xref:fundamentals/servers/weblistener) no ASP.NET Core 1.x.)</span><span class="sxs-lookup"><span data-stu-id="fd9ab-115">(HTTP.sys is called [WebListener](xref:fundamentals/servers/weblistener) in ASP.NET Core 1.x.)</span></span>

::: moniker-end

## <a name="kestrel"></a><span data-ttu-id="fd9ab-116">Kestrel</span><span class="sxs-lookup"><span data-stu-id="fd9ab-116">Kestrel</span></span>

<span data-ttu-id="fd9ab-117">O Kestrel é o servidor Web padrão incluído nos modelos de projeto do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="fd9ab-117">Kestrel is the default web server included in ASP.NET Core project templates.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="fd9ab-118">O Kestrel pode ser usado sozinho ou com um *servidor proxy reverso* como IIS, Nginx ou Apache.</span><span class="sxs-lookup"><span data-stu-id="fd9ab-118">Kestrel can be used by itself or with a *reverse proxy server*, such as IIS, Nginx, or Apache.</span></span> <span data-ttu-id="fd9ab-119">Um servidor proxy reverso recebe solicitações HTTP da Internet e as encaminha para o Kestrel após algum tratamento preliminar.</span><span class="sxs-lookup"><span data-stu-id="fd9ab-119">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![O Kestrel se comunica diretamente com a Internet, sem um servidor proxy reverso](kestrel/_static/kestrel-to-internet2.png)

![O Kestrel se comunica indiretamente com a Internet através de um servidor proxy reverso, tal como o IIS, o Nginx ou o Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="fd9ab-122">Qualquer configuração &mdash;com ou sem um servidor proxy reverso&mdash; é uma configuração de hospedagem válida e compatível com o ASP.NET Core 2.0 ou aplicativos posteriores.</span><span class="sxs-lookup"><span data-stu-id="fd9ab-122">Either configuration&mdash;with or without a reverse proxy server&mdash;is a valid and supported hosting configuration for ASP.NET Core 2.0 or later apps.</span></span> <span data-ttu-id="fd9ab-123">Para obter mais informações, consulte [Quando usar Kestrel com um proxy reverso](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span><span class="sxs-lookup"><span data-stu-id="fd9ab-123">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="fd9ab-124">Se o aplicativo aceita somente solicitações de uma rede interna, o Kestrel pode ser usado sozinho.</span><span class="sxs-lookup"><span data-stu-id="fd9ab-124">If the app only accepts requests from an internal network, Kestrel can be used by itself.</span></span>

![O Kestrel se comunica diretamente com a rede interna](kestrel/_static/kestrel-to-internal.png)

<span data-ttu-id="fd9ab-126">Se o aplicativo for exposto à Internet, o Kestrel deverá usar o IIS, o Nginx ou o Apache como um *servidor proxy reverso*.</span><span class="sxs-lookup"><span data-stu-id="fd9ab-126">If the app is exposed to the Internet, Kestrel must use IIS, Nginx, or Apache as a *reverse proxy server*.</span></span> <span data-ttu-id="fd9ab-127">Um servidor proxy reverso recebe solicitações HTTP da Internet e as encaminha para o Kestrel após algum tratamento preliminar, conforme mostrado no diagrama a seguir:</span><span class="sxs-lookup"><span data-stu-id="fd9ab-127">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling, as shown in the following diagram:</span></span>

![O Kestrel se comunica indiretamente com a Internet através de um servidor proxy reverso, tal como o IIS, o Nginx ou o Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="fd9ab-129">O motivo mais importante para usar um proxy reverso para implantações de servidores de borda voltados para o público que são expostas diretamente na Internet é a segurança.</span><span class="sxs-lookup"><span data-stu-id="fd9ab-129">The most important reason for using a reverse proxy for public-facing edge server deployments that are exposed directly the Internet is security.</span></span> <span data-ttu-id="fd9ab-130">As versões 1.x do Kestrel não têm recursos de segurança importantes para proteção contra ataques da Internet.</span><span class="sxs-lookup"><span data-stu-id="fd9ab-130">The 1.x versions of Kestrel don't have important security features to defend against attacks from the Internet.</span></span> <span data-ttu-id="fd9ab-131">Isso inclui, mas não se limita aos tempos limite, limites de tamanho da solicitação e limites de conexões simultâneas apropriados.</span><span class="sxs-lookup"><span data-stu-id="fd9ab-131">This includes, but isn't limited to, appropriate timeouts, request size limits, and concurrent connection limits.</span></span>

<span data-ttu-id="fd9ab-132">Para obter mais informações, consulte [Quando usar Kestrel com um proxy reverso](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span><span class="sxs-lookup"><span data-stu-id="fd9ab-132">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

::: moniker-end

<span data-ttu-id="fd9ab-133">O IIS, o Nginx ou o Apache não podem ser usados sem o Kestrel ou uma [implementação de servidor personalizado](#custom-servers).</span><span class="sxs-lookup"><span data-stu-id="fd9ab-133">IIS, Nginx, and Apache can't be used without Kestrel or a [custom server implementation](#custom-servers).</span></span> <span data-ttu-id="fd9ab-134">O ASP.NET Core foi projetado para ser executado em seu próprio processo, de modo que ele pode se comportar de forma consistente entre plataformas.</span><span class="sxs-lookup"><span data-stu-id="fd9ab-134">ASP.NET Core was designed to run in its own process so that it can behave consistently across platforms.</span></span> <span data-ttu-id="fd9ab-135">O IIS, o Nginx e o Apache determinam seu próprio procedimento de inicialização e o ambiente.</span><span class="sxs-lookup"><span data-stu-id="fd9ab-135">IIS, Nginx, and Apache dictate their own startup procedure and environment.</span></span> <span data-ttu-id="fd9ab-136">Para usar essas tecnologias de servidor diretamente, o ASP.NET Core precisaria adaptar-se aos requisitos de cada servidor.</span><span class="sxs-lookup"><span data-stu-id="fd9ab-136">To use these server technologies directly, ASP.NET Core would need to adapt to the requirements of each server.</span></span> <span data-ttu-id="fd9ab-137">Usando uma implementação do servidor Web, tal como Kestrel, o ASP.NET Core tem controle sobre o processo de inicialização e o ambiente quando hospedado em tecnologias de servidor diferentes.</span><span class="sxs-lookup"><span data-stu-id="fd9ab-137">Using a web server implementation, such as Kestrel, ASP.NET Core has control over the startup process and environment when hosted on different server technologies.</span></span>

### <a name="iis-with-kestrel"></a><span data-ttu-id="fd9ab-138">IIS com Kestrel</span><span class="sxs-lookup"><span data-stu-id="fd9ab-138">IIS with Kestrel</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="fd9ab-139">Ao usar o [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) ou o [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), o aplicativo do ASP.NET Core é executado no mesmo processo que o processo de trabalho do IIS (o modelo de hospedagem *em processo*) ou em um processo de trabalho separado do IIS (o modelo de hospedagem *fora do processo*).</span><span class="sxs-lookup"><span data-stu-id="fd9ab-139">When using [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), the ASP.NET Core app either runs in the same process as the IIS worker process (the *in-process* hosting model) or in a process separate from the IIS worker process (the *out-of-process* hosting model).</span></span>

<span data-ttu-id="fd9ab-140">O [Módulo do ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) é um módulo nativo do IIS que manipula solicitações nativas do IIS entre o servidor HTTP em processo do IIS ou o servidor Kestrel fora do processo.</span><span class="sxs-lookup"><span data-stu-id="fd9ab-140">The [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) is a native IIS module that handles native IIS requests between either the in-process IIS Http Server or the out-of-process Kestrel server.</span></span> <span data-ttu-id="fd9ab-141">Para obter mais informações, consulte <xref:fundamentals/servers/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="fd9ab-141">For more information, see <xref:fundamentals/servers/aspnet-core-module>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="fd9ab-142">Ao usas o [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) ou [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) como um proxy reverso para o ASP.NET Core, o aplicativo ASP.NET Core é executado em um processo separado do processo de trabalho do IIS.</span><span class="sxs-lookup"><span data-stu-id="fd9ab-142">When using [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) as a reverse proxy for ASP.NET Core, the ASP.NET Core app runs in a process separate from the IIS worker process.</span></span> <span data-ttu-id="fd9ab-143">No processo do IIS, o [Módulo do ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) coordena a relação do proxy reverso.</span><span class="sxs-lookup"><span data-stu-id="fd9ab-143">In the IIS process, the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) coordinates the reverse proxy relationship.</span></span> <span data-ttu-id="fd9ab-144">As funções primárias do módulo do ASP.NET Core são iniciar o aplicativo do ASP.NET Core, reiniciá-lo quando ele falhar e encaminhar o tráfego HTTP para o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="fd9ab-144">The primary functions of the ASP.NET Core Module are to start the ASP.NET Core app, restart the app when it crashes, and forward HTTP traffic to the app.</span></span> <span data-ttu-id="fd9ab-145">Para obter mais informações, consulte <xref:fundamentals/servers/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="fd9ab-145">For more information, see <xref:fundamentals/servers/aspnet-core-module>.</span></span>

::: moniker-end

### <a name="nginx-with-kestrel"></a><span data-ttu-id="fd9ab-146">Nginx com Kestrel</span><span class="sxs-lookup"><span data-stu-id="fd9ab-146">Nginx with Kestrel</span></span>

<span data-ttu-id="fd9ab-147">Para obter informações sobre como usar Nginx no Linux como um servidor proxy reverso para Kestrel, veja [Hospedar em Linux com o Nginx](xref:host-and-deploy/linux-nginx).</span><span class="sxs-lookup"><span data-stu-id="fd9ab-147">For information on how to use Nginx on Linux as a reverse proxy server for Kestrel, see [Host on Linux with Nginx](xref:host-and-deploy/linux-nginx).</span></span>

### <a name="apache-with-kestrel"></a><span data-ttu-id="fd9ab-148">Apache com Kestrel</span><span class="sxs-lookup"><span data-stu-id="fd9ab-148">Apache with Kestrel</span></span>

<span data-ttu-id="fd9ab-149">Para obter informações sobre como usar Apache no Linux como um servidor proxy reverso para Kestrel, veja [Hospedar em Linux com o Apache](xref:host-and-deploy/linux-apache).</span><span class="sxs-lookup"><span data-stu-id="fd9ab-149">For information on how to use Apache on Linux as a reverse proxy server for Kestrel, see [Host on Linux with Apache](xref:host-and-deploy/linux-apache).</span></span>

## <a name="httpsys"></a><span data-ttu-id="fd9ab-150">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="fd9ab-150">HTTP.sys</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="fd9ab-151">Se os aplicativos ASP.NET Core forem executados no Windows, o HTTP.sys será uma alternativa ao Kestrel.</span><span class="sxs-lookup"><span data-stu-id="fd9ab-151">If ASP.NET Core apps are run on Windows, HTTP.sys is an alternative to Kestrel.</span></span> <span data-ttu-id="fd9ab-152">O Kestrel geralmente é recomendado para melhor desempenho.</span><span class="sxs-lookup"><span data-stu-id="fd9ab-152">Kestrel is generally recommended for best performance.</span></span> <span data-ttu-id="fd9ab-153">O HTTP.sys pode ser usado em cenários em que o aplicativo é exposto à Internet e as funcionalidades necessárias são compatíveis com HTTP.sys, mas não com Kestrel.</span><span class="sxs-lookup"><span data-stu-id="fd9ab-153">HTTP.sys can be used in scenarios where the app is exposed to the Internet and required capabilities are supported by HTTP.sys but not Kestrel.</span></span> <span data-ttu-id="fd9ab-154">Para obter informações sobre HTTP.sys, veja [HTTP.sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="fd9ab-154">For information on HTTP.sys, see [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

![O HTTP.sys se comunica diretamente com a Internet](httpsys/_static/httpsys-to-internet.png)

<span data-ttu-id="fd9ab-156">O HTTP.sys também pode ser usado para aplicativos que são expostos somente a uma rede interna.</span><span class="sxs-lookup"><span data-stu-id="fd9ab-156">HTTP.sys can also be used for apps that are only exposed to an internal network.</span></span>

![O HTTP.sys se comunica diretamente com a rede interna](httpsys/_static/httpsys-to-internal.png)

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="fd9ab-158">O HTTP.sys é chamado [WebListener](xref:fundamentals/servers/weblistener) no ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="fd9ab-158">HTTP.sys is named [WebListener](xref:fundamentals/servers/weblistener) in ASP.NET Core 1.x.</span></span> <span data-ttu-id="fd9ab-159">Se os aplicativos ASP.NET Core forem executados no Windows, o WebListener será uma alternativa para cenários em que o IIS não está disponível para hospedar aplicativos.</span><span class="sxs-lookup"><span data-stu-id="fd9ab-159">If ASP.NET Core apps are run on Windows, WebListener is an alternative for scenarios where IIS isn't available to host apps.</span></span>

![O WebListener se comunica diretamente com a Internet](weblistener/_static/weblistener-to-internet.png)

<span data-ttu-id="fd9ab-161">O WebListener também poderá ser usado no lugar do Kestrel para aplicativos expostos somente a uma rede interna se as funcionalidades necessárias forem compatíveis com o WebListener, mas não com o Kestrel.</span><span class="sxs-lookup"><span data-stu-id="fd9ab-161">WebListener can also be used in place of Kestrel for apps that are only exposed to an internal network, if required capabilities are supported by WebListener but not Kestrel.</span></span> <span data-ttu-id="fd9ab-162">Para obter mais informações sobre o WebListener, consulte [WebListener](xref:fundamentals/servers/weblistener).</span><span class="sxs-lookup"><span data-stu-id="fd9ab-162">For information on WebListener, see [WebListener](xref:fundamentals/servers/weblistener).</span></span>

![O WebListener se comunica diretamente com a rede interna](weblistener/_static/weblistener-to-internal.png)

::: moniker-end

## <a name="aspnet-core-server-infrastructure"></a><span data-ttu-id="fd9ab-164">Infraestrutura de servidor do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fd9ab-164">ASP.NET Core server infrastructure</span></span>

<span data-ttu-id="fd9ab-165">O [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) disponível no método `Startup.Configure` expõe a propriedade [ServerFeatures](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.serverfeatures) do tipo [IFeatureCollection](/dotnet/api/microsoft.aspnetcore.http.features.ifeaturecollection).</span><span class="sxs-lookup"><span data-stu-id="fd9ab-165">The [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) available in the `Startup.Configure` method exposes the [ServerFeatures](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.serverfeatures) property of type [IFeatureCollection](/dotnet/api/microsoft.aspnetcore.http.features.ifeaturecollection).</span></span> <span data-ttu-id="fd9ab-166">Kestrel e HTTP.sys (WebListener no ASP.NET Core 1.x) expõem apenas um único recurso cada, [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature), mas as implementações de servidor diferentes podem expor funcionalidade adicional.</span><span class="sxs-lookup"><span data-stu-id="fd9ab-166">Kestrel and HTTP.sys (WebListener in ASP.NET Core 1.x) only expose a single feature each, [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature), but different server implementations may expose additional functionality.</span></span>

<span data-ttu-id="fd9ab-167">`IServerAddressesFeature` pode ser usado para descobrir a qual porta a implementação do servidor se acoplou durante o tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="fd9ab-167">`IServerAddressesFeature` can be used to find out which port the server implementation has bound at runtime.</span></span>

## <a name="custom-servers"></a><span data-ttu-id="fd9ab-168">Servidores personalizados</span><span class="sxs-lookup"><span data-stu-id="fd9ab-168">Custom servers</span></span>

<span data-ttu-id="fd9ab-169">Se os servidores internos não atenderem aos requisitos do aplicativo, um servidor personalizado poderá ser criado.</span><span class="sxs-lookup"><span data-stu-id="fd9ab-169">If the built-in servers don't meet the app's requirements, a custom server implementation can be created.</span></span> <span data-ttu-id="fd9ab-170">O [Guia de OWIN (Open Web Interface para .NET)](xref:fundamentals/owin) demonstra como gravar uma implementação de [IServer](/dotnet/api/microsoft.aspnetcore.hosting.server.iserver) com base em [Nowin](https://github.com/Bobris/Nowin).</span><span class="sxs-lookup"><span data-stu-id="fd9ab-170">The [Open Web Interface for .NET (OWIN) guide](xref:fundamentals/owin) demonstrates how to write a [Nowin](https://github.com/Bobris/Nowin)-based [IServer](/dotnet/api/microsoft.aspnetcore.hosting.server.iserver) implementation.</span></span> <span data-ttu-id="fd9ab-171">Somente as interfaces de recurso que o aplicativo usa exigem implementação. Porém, no mínimo [IHttpRequestFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttprequestfeature) e [IHttpResponseFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttpresponsefeature) devem ter suporte.</span><span class="sxs-lookup"><span data-stu-id="fd9ab-171">Only the feature interfaces that the app uses require implementation, though at a minimum [IHttpRequestFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttprequestfeature) and [IHttpResponseFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttpresponsefeature) must be supported.</span></span>

## <a name="server-startup"></a><span data-ttu-id="fd9ab-172">Inicialização do servidor</span><span class="sxs-lookup"><span data-stu-id="fd9ab-172">Server startup</span></span>

<span data-ttu-id="fd9ab-173">Ao usar o [Visual Studio](https://www.visualstudio.com/vs/), o [Visual Studio para Mac](https://www.visualstudio.com/vs/mac/) ou o [Visual Studio Code](https://code.visualstudio.com/), o servidor é iniciado quando o aplicativo é iniciado pelo IDE (ambiente de desenvolvimento integrado).</span><span class="sxs-lookup"><span data-stu-id="fd9ab-173">When using [Visual Studio](https://www.visualstudio.com/vs/), [Visual Studio for Mac](https://www.visualstudio.com/vs/mac/), or [Visual Studio Code](https://code.visualstudio.com/), the server is launched when the app is started by the Integrated Development Environment (IDE).</span></span> <span data-ttu-id="fd9ab-174">No Visual Studio no Windows, os perfis de inicialização podem ser usados para iniciar o aplicativo e o servidor com o [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)/[Módulo do ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) ou o console.</span><span class="sxs-lookup"><span data-stu-id="fd9ab-174">In Visual Studio on Windows, launch profiles can be used to start the app and server with either [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)/[ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) or the console.</span></span> <span data-ttu-id="fd9ab-175">No Visual Studio Code, o aplicativo e o servidor são iniciados por [Omnisharp](https://github.com/OmniSharp/omnisharp-vscode), que ativa o depurador CoreCLR.</span><span class="sxs-lookup"><span data-stu-id="fd9ab-175">In Visual Studio Code, the app and server are started by [Omnisharp](https://github.com/OmniSharp/omnisharp-vscode), which activates the CoreCLR debugger.</span></span> <span data-ttu-id="fd9ab-176">Usando o Visual Studio para Mac, o aplicativo e o servidor são iniciados pelo [depurador de modo reversível Mono](http://www.mono-project.com/docs/advanced/runtime/docs/soft-debugger/).</span><span class="sxs-lookup"><span data-stu-id="fd9ab-176">Using Visual Studio for Mac, the app and server are started by the [Mono Soft-Mode Debugger](http://www.mono-project.com/docs/advanced/runtime/docs/soft-debugger/).</span></span>

<span data-ttu-id="fd9ab-177">Ao iniciar um aplicativo com um prompt de comando na pasta do projeto, a [execução dotnet](/dotnet/core/tools/dotnet-run) inicia o aplicativo e o servidor (somente no Kestrel e no HTTP.sys).</span><span class="sxs-lookup"><span data-stu-id="fd9ab-177">When launching an app from a command prompt in the project's folder, [dotnet run](/dotnet/core/tools/dotnet-run) launches the app and server (Kestrel and HTTP.sys only).</span></span> <span data-ttu-id="fd9ab-178">A configuração é especificada pela opção `-c|--configuration`, que é definida como `Debug` (padrão) ou `Release`.</span><span class="sxs-lookup"><span data-stu-id="fd9ab-178">The configuration is specified by the `-c|--configuration` option, which is set to either `Debug` (default) or `Release`.</span></span> <span data-ttu-id="fd9ab-179">Se os perfis de inicialização estiverem presentes em um arquivo *launchSettings.json*, use a opção `--launch-profile <NAME>` para definir o perfil de inicialização (por exemplo, `Development` ou `Production`).</span><span class="sxs-lookup"><span data-stu-id="fd9ab-179">If launch profiles are present in a *launchSettings.json* file, use the `--launch-profile <NAME>` option to set the launch profile (for example, `Development` or `Production`).</span></span> <span data-ttu-id="fd9ab-180">Para obter mais informações, veja os tópicos [execução dotnet](/dotnet/core/tools/dotnet-run) e [Empacotamento de distribuição do .NET Core](/dotnet/core/build/distribution-packaging).</span><span class="sxs-lookup"><span data-stu-id="fd9ab-180">For more information, see the [dotnet run](/dotnet/core/tools/dotnet-run) and [.NET Core distribution packaging](/dotnet/core/build/distribution-packaging) topics.</span></span>

## <a name="http2-support"></a><span data-ttu-id="fd9ab-181">Compatibilidade com HTTP/2</span><span class="sxs-lookup"><span data-stu-id="fd9ab-181">HTTP/2 support</span></span>

<span data-ttu-id="fd9ab-182">O [HTTP/2](https://httpwg.org/specs/rfc7540.html) é compatível com ASP.NET Core nos seguintes cenários de implantação:</span><span class="sxs-lookup"><span data-stu-id="fd9ab-182">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is supported with ASP.NET Core in the following deployment scenarios:</span></span>

::: moniker range=">= aspnetcore-2.2"

* [<span data-ttu-id="fd9ab-183">Kestrel</span><span class="sxs-lookup"><span data-stu-id="fd9ab-183">Kestrel</span></span>](xref:fundamentals/servers/kestrel#http2-support)
  * <span data-ttu-id="fd9ab-184">Sistema operacional</span><span class="sxs-lookup"><span data-stu-id="fd9ab-184">Operating system</span></span>
    * <span data-ttu-id="fd9ab-185">Windows Server 2012 R2/Windows 8.1 ou posterior</span><span class="sxs-lookup"><span data-stu-id="fd9ab-185">Windows Server 2012 R2/Windows 8.1 or later</span></span>
    * <span data-ttu-id="fd9ab-186">Linux com OpenSSL 1.0.2 ou posterior (por exemplo, Ubuntu 16.04 ou posterior)</span><span class="sxs-lookup"><span data-stu-id="fd9ab-186">Linux with OpenSSL 1.0.2 or later (for example, Ubuntu 16.04 or later)</span></span>
    * <span data-ttu-id="fd9ab-187">O HTTP/2 será compatível com macOS em uma versão futura.</span><span class="sxs-lookup"><span data-stu-id="fd9ab-187">HTTP/2 will be supported on macOS in a future release.</span></span>
  * <span data-ttu-id="fd9ab-188">Estrutura de destino: .NET Core 2.2 ou posterior</span><span class="sxs-lookup"><span data-stu-id="fd9ab-188">Target framework: .NET Core 2.2 or later</span></span>
* [<span data-ttu-id="fd9ab-189">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="fd9ab-189">HTTP.sys</span></span>](xref:fundamentals/servers/httpsys#http2-support)
  * <span data-ttu-id="fd9ab-190">Windows Server 2016/Windows 10 ou posterior</span><span class="sxs-lookup"><span data-stu-id="fd9ab-190">Windows Server 2016/Windows 10 or later</span></span>
  * <span data-ttu-id="fd9ab-191">Estrutura de destino: não aplicável a implantações de HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="fd9ab-191">Target framework: Not applicable to HTTP.sys deployments.</span></span>
* [<span data-ttu-id="fd9ab-192">IIS (em processo)</span><span class="sxs-lookup"><span data-stu-id="fd9ab-192">IIS (in-process)</span></span>](xref:host-and-deploy/iis/index#http2-support)
  * <span data-ttu-id="fd9ab-193">Windows Server 2016/Windows 10 ou posterior; IIS 10 ou posterior</span><span class="sxs-lookup"><span data-stu-id="fd9ab-193">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="fd9ab-194">Estrutura de destino: .NET Core 2.2 ou posterior</span><span class="sxs-lookup"><span data-stu-id="fd9ab-194">Target framework: .NET Core 2.2 or later</span></span>
* [<span data-ttu-id="fd9ab-195">IIS (fora do processo)</span><span class="sxs-lookup"><span data-stu-id="fd9ab-195">IIS (out-of-process)</span></span>](xref:host-and-deploy/iis/index#http2-support)
  * <span data-ttu-id="fd9ab-196">Windows Server 2016/Windows 10 ou posterior; IIS 10 ou posterior</span><span class="sxs-lookup"><span data-stu-id="fd9ab-196">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="fd9ab-197">Conexões de servidor de borda voltadas para o público usam HTTP/2, mas a conexão de proxy reverso para o Kestrel usa HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="fd9ab-197">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to Kestrel uses HTTP/1.1.</span></span>
  * <span data-ttu-id="fd9ab-198">Estrutura de destino: não aplicável a implantações IIS fora de processo.</span><span class="sxs-lookup"><span data-stu-id="fd9ab-198">Target framework: Not applicable to IIS out-of-process deployments.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* [<span data-ttu-id="fd9ab-199">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="fd9ab-199">HTTP.sys</span></span>](xref:fundamentals/servers/httpsys#http2-support)
  * <span data-ttu-id="fd9ab-200">Windows Server 2016/Windows 10 ou posterior</span><span class="sxs-lookup"><span data-stu-id="fd9ab-200">Windows Server 2016/Windows 10 or later</span></span>
  * <span data-ttu-id="fd9ab-201">Estrutura de destino: não aplicável a implantações de HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="fd9ab-201">Target framework: Not applicable to HTTP.sys deployments.</span></span>
* [<span data-ttu-id="fd9ab-202">IIS (fora do processo)</span><span class="sxs-lookup"><span data-stu-id="fd9ab-202">IIS (out-of-process)</span></span>](xref:host-and-deploy/iis/index#http2-support)
  * <span data-ttu-id="fd9ab-203">Windows Server 2016/Windows 10 ou posterior; IIS 10 ou posterior</span><span class="sxs-lookup"><span data-stu-id="fd9ab-203">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="fd9ab-204">Conexões de servidor de borda voltadas para o público usam HTTP/2, mas a conexão de proxy reverso para o Kestrel usa HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="fd9ab-204">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to Kestrel uses HTTP/1.1.</span></span>
  * <span data-ttu-id="fd9ab-205">Estrutura de destino: não aplicável a implantações IIS fora de processo.</span><span class="sxs-lookup"><span data-stu-id="fd9ab-205">Target framework: Not applicable to IIS out-of-process deployments.</span></span>

::: moniker-end

<span data-ttu-id="fd9ab-206">Uma conexão HTTP/2 precisa usar [ALPN (Application-Layer Protocol Negotiation)](https://tools.ietf.org/html/rfc7301#section-3) e TLS 1.2 ou posterior.</span><span class="sxs-lookup"><span data-stu-id="fd9ab-206">An HTTP/2 connection must use [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) and TLS 1.2 or later.</span></span> <span data-ttu-id="fd9ab-207">Para obter mais informações, consulte os tópicos referentes aos seus cenários de implantação do servidor.</span><span class="sxs-lookup"><span data-stu-id="fd9ab-207">For more information, see the topics that pertain to your server deployment scenarios.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="fd9ab-208">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="fd9ab-208">Additional resources</span></span>

* <xref:fundamentals/servers/kestrel>
* <xref:fundamentals/servers/aspnet-core-module>
* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/linux-nginx>
* <xref:host-and-deploy/linux-apache>
* <span data-ttu-id="fd9ab-209"><xref:fundamentals/servers/httpsys> (para ASP.NET Core 1.x, consulte <xref:fundamentals/servers/weblistener>)</span><span class="sxs-lookup"><span data-stu-id="fd9ab-209"><xref:fundamentals/servers/httpsys> (for ASP.NET Core 1.x, see <xref:fundamentals/servers/weblistener>)</span></span>

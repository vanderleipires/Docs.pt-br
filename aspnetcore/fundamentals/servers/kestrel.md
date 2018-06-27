---
title: Implementação do servidor Web Kestrel no ASP.NET Core
author: rick-anderson
description: Saiba mais sobre o Kestrel, o servidor Web multiplataforma do ASP.NET Core.
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 05/02/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/kestrel
ms.openlocfilehash: 39949585dc8fce10c31045ef3013c6bc166e45ba
ms.sourcegitcommit: 4e3497bda0c3e5011ffba3717eb61a1d46c61c15
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/14/2018
ms.locfileid: "35613145"
---
# <a name="kestrel-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="2ff6e-103">Implementação do servidor Web Kestrel no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2ff6e-103">Kestrel web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="2ff6e-104">Por [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher) e [Stephen Halter](https://twitter.com/halter73)</span><span class="sxs-lookup"><span data-stu-id="2ff6e-104">By [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), and [Stephen Halter](https://twitter.com/halter73)</span></span>

<span data-ttu-id="2ff6e-105">O Kestrel é um [servidor Web multiplataforma para o ASP.NET Core](xref:fundamentals/servers/index).</span><span class="sxs-lookup"><span data-stu-id="2ff6e-105">Kestrel is a cross-platform [web server for ASP.NET Core](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="2ff6e-106">O Kestrel é o servidor Web que está incluído por padrão em modelos de projeto do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-106">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span>

<span data-ttu-id="2ff6e-107">O Kestrel dá suporte aos seguintes recursos:</span><span class="sxs-lookup"><span data-stu-id="2ff6e-107">Kestrel supports the following features:</span></span>

* <span data-ttu-id="2ff6e-108">HTTPS</span><span class="sxs-lookup"><span data-stu-id="2ff6e-108">HTTPS</span></span>
* <span data-ttu-id="2ff6e-109">Atualização do Opaque usado para habilitar o [WebSockets](https://github.com/aspnet/websockets)</span><span class="sxs-lookup"><span data-stu-id="2ff6e-109">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
* <span data-ttu-id="2ff6e-110">Soquetes do UNIX para alto desempenho protegidos pelo Nginx</span><span class="sxs-lookup"><span data-stu-id="2ff6e-110">Unix sockets for high performance behind Nginx</span></span>

<span data-ttu-id="2ff6e-111">Há suporte para o Kestrel em todas as plataformas e versões compatíveis com o .NET Core.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-111">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

<span data-ttu-id="2ff6e-112">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="2ff6e-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="2ff6e-113">Quando usar o Kestrel com um proxy reverso</span><span class="sxs-lookup"><span data-stu-id="2ff6e-113">When to use Kestrel with a reverse proxy</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="2ff6e-114">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="2ff6e-114">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="2ff6e-115">Você pode usar o Kestrel sozinho ou com um *servidor proxy reverso* como IIS, Nginx ou Apache.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-115">You can use Kestrel by itself or with a *reverse proxy server*, such as IIS, Nginx, or Apache.</span></span> <span data-ttu-id="2ff6e-116">Um servidor proxy reverso recebe solicitações HTTP da Internet e as encaminha para o Kestrel após algum tratamento preliminar.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-116">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![O Kestrel se comunica diretamente com a Internet, sem um servidor proxy reverso](kestrel/_static/kestrel-to-internet2.png)

![O Kestrel se comunica indiretamente com a Internet através de um servidor proxy reverso, tal como o IIS, o Nginx ou o Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="2ff6e-119">Qualquer configuração &mdash;com ou sem um servidor proxy reverso&mdash; é uma configuração de hospedagem válida e compatível com o ASP.NET Core 2.0 ou aplicativos posteriores.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-119">Either configuration&mdash;with or without a reverse proxy server&mdash;is a valid and supported hosting configuration for ASP.NET Core 2.0 or later apps.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="2ff6e-120">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="2ff6e-120">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="2ff6e-121">Se um aplicativo aceitar solicitações somente de uma rede interna, o Kestrel poderá ser usado diretamente como o servidor do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-121">If an app accepts requests only from an internal network, Kestrel can be used directly as the app's server.</span></span>

![O Kestrel se comunica diretamente com a rede interna](kestrel/_static/kestrel-to-internal.png)

<span data-ttu-id="2ff6e-123">Se você expõe o aplicativo à Internet, use o IIS, o Nginx ou o Apache como um *servidor proxy reverso*.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-123">If you expose your app to the Internet, use IIS, Nginx, or Apache as a *reverse proxy server*.</span></span> <span data-ttu-id="2ff6e-124">Um servidor proxy reverso recebe solicitações HTTP da Internet e as encaminha para o Kestrel após algum tratamento preliminar.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-124">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![O Kestrel se comunica indiretamente com a Internet através de um servidor proxy reverso, tal como o IIS, o Nginx ou o Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="2ff6e-126">Um proxy reverso é necessário para implantações de borda (expostas ao tráfego da Internet) por motivos de segurança.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-126">A reverse proxy is required for edge deployments (exposed to traffic from the Internet) for security reasons.</span></span> <span data-ttu-id="2ff6e-127">As versões 1.x do Kestrel não têm um conjunto completo de defesas contra ataques, como tempos limite, limites de tamanho e limites de conexões simultâneas apropriados.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-127">The 1.x versions of Kestrel don't have a full complement of defenses against attacks, such as appropriate timeouts, size limits, and concurrent connection limits.</span></span>

---

<span data-ttu-id="2ff6e-128">Um cenário de proxy reverso existe quando há vários aplicativos que compartilham o mesmo IP e a mesma porta em execução em um único servidor.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-128">A reverse proxy scenario exists when there are multiple apps that share the same IP and port running on a single server.</span></span> <span data-ttu-id="2ff6e-129">O Kestrel não é compatível com esse cenário porque ele não permite compartilhar o mesmo IP e a mesma porta entre vários processos.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-129">Kestrel doesn't support this scenario because Kestrel doesn't support sharing the same IP and port among multiple processes.</span></span> <span data-ttu-id="2ff6e-130">Quando o Kestrel é configurado para escutar em uma porta, ele lida com todo o tráfego dessa porta, independentemente do cabeçalho do host das solicitações.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-130">When Kestrel is configured to listen on a port, Kestrel handles all of the traffic for that port regardless of requests' host header.</span></span> <span data-ttu-id="2ff6e-131">Um proxy reverso que pode compartilhar portas tem a capacidade de encaminhar solicitações ao Kestrel em um IP e em uma porta exclusivos.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-131">A reverse proxy that can share ports has the ability to forward requests to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="2ff6e-132">Mesmo se um servidor proxy reverso não for necessário, usar um servidor proxy reverso poderá ser uma boa opção:</span><span class="sxs-lookup"><span data-stu-id="2ff6e-132">Even if a reverse proxy server isn't required, using a reverse proxy server might be a good choice:</span></span>

* <span data-ttu-id="2ff6e-133">Ele pode limitar a área da superfície pública exposta dos aplicativos que hospeda.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-133">It can limit the exposed public surface area of the apps that it hosts.</span></span>
* <span data-ttu-id="2ff6e-134">Ele fornece uma camada adicional de defesa e de configuração.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-134">It provides an additional layer of configuration and defense.</span></span>
* <span data-ttu-id="2ff6e-135">Pode ser integrado melhor à infraestrutura existente.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-135">It might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="2ff6e-136">Ele simplifica a configuração de SSL e o balanceamento de carga.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-136">It simplifies load balancing and SSL configuration.</span></span> <span data-ttu-id="2ff6e-137">Somente o servidor proxy reverso exige um certificado SSL e esse servidor pode se comunicar com os servidores de aplicativos na rede interna usando HTTP simples.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-137">Only the reverse proxy server requires an SSL certificate, and that server can communicate with your app servers on the internal network using plain HTTP.</span></span>

> [!WARNING]
> <span data-ttu-id="2ff6e-138">Se você não estiver usando um proxy reverso com a filtragem de host habilitada, a [filtragem de host](#host-filtering) deverá ser habilitada.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-138">If not using a reverse proxy with host filtering enabled, [host filtering](#host-filtering) must be enabled.</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="2ff6e-139">Como usar o Kestrel em aplicativos ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2ff6e-139">How to use Kestrel in ASP.NET Core apps</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="2ff6e-140">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="2ff6e-140">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="2ff6e-141">O pacote [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) está incluído no [metapacote de Microsoft.AspNetCore.App] (xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 ou posterior).</span><span class="sxs-lookup"><span data-stu-id="2ff6e-141">The [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) package is included in the [Microsoft.AspNetCore.App metapackage] (xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later).</span></span>

<span data-ttu-id="2ff6e-142">Os modelos de projeto do ASP.NET Core usam o Kestrel por padrão.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-142">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="2ff6e-143">Em *Program.cs*, o código do modelo chama [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), que chama [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) em segundo plano.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-143">In *Program.cs*, the template code calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), which calls [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) behind the scenes.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="2ff6e-144">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="2ff6e-144">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="2ff6e-145">Instale o pacote NuGet [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/).</span><span class="sxs-lookup"><span data-stu-id="2ff6e-145">Install the [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) NuGet package.</span></span>

<span data-ttu-id="2ff6e-146">Chame o método de extensão [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) no [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder?view=aspnetcore-1.1) no método `Main`, especificando as [opções do Kestrel](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions?view=aspnetcore-1.1) necessárias, conforme é mostrado na próxima seção.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-146">Call the [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) extension method on [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder?view=aspnetcore-1.1) in the `Main` method, specifying any [Kestrel options](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions?view=aspnetcore-1.1) required, as shown in the next section.</span></span>

[!code-csharp[](kestrel/samples/1.x/KestrelSample/Program.cs?name=snippet_Main&highlight=13-19)]

---

### <a name="kestrel-options"></a><span data-ttu-id="2ff6e-147">Opções do Kestrel</span><span class="sxs-lookup"><span data-stu-id="2ff6e-147">Kestrel options</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="2ff6e-148">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="2ff6e-148">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="2ff6e-149">O servidor Web do Kestrel tem opções de configuração de restrição especialmente úteis em implantações para a Internet.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-149">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span> <span data-ttu-id="2ff6e-150">Alguns limites importantes que podem ser personalizados:</span><span class="sxs-lookup"><span data-stu-id="2ff6e-150">A few important limits that can be customized:</span></span>

* <span data-ttu-id="2ff6e-151">Número máximo de conexões de cliente</span><span class="sxs-lookup"><span data-stu-id="2ff6e-151">Maximum client connections</span></span>
* <span data-ttu-id="2ff6e-152">Tamanho máximo do corpo da solicitação</span><span class="sxs-lookup"><span data-stu-id="2ff6e-152">Maximum request body size</span></span>
* <span data-ttu-id="2ff6e-153">Taxa de dados mínima do corpo da solicitação</span><span class="sxs-lookup"><span data-stu-id="2ff6e-153">Minimum request body data rate</span></span>

<span data-ttu-id="2ff6e-154">Defina essas e outras restrições na propriedade [Limites](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.limits) da classe [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions).</span><span class="sxs-lookup"><span data-stu-id="2ff6e-154">Set these and other constraints on the [Limits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.limits) property of the [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) class.</span></span> <span data-ttu-id="2ff6e-155">A propriedade `Limits` contém uma instância da classe [KestrelServerLimits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits).</span><span class="sxs-lookup"><span data-stu-id="2ff6e-155">The `Limits` property holds an instance of the [KestrelServerLimits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits) class.</span></span>

<span data-ttu-id="2ff6e-156">**Número máximo de conexões de cliente**</span><span class="sxs-lookup"><span data-stu-id="2ff6e-156">**Maximum client connections**</span></span>

[<span data-ttu-id="2ff6e-157">MaxConcurrentConnections</span><span class="sxs-lookup"><span data-stu-id="2ff6e-157">MaxConcurrentConnections</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxconcurrentconnections)  
[<span data-ttu-id="2ff6e-158">MaxConcurrentUpgradedConnections</span><span class="sxs-lookup"><span data-stu-id="2ff6e-158">MaxConcurrentUpgradedConnections</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxconcurrentupgradedconnections)

<span data-ttu-id="2ff6e-159">O número máximo de conexões TCP abertas simultâneas pode ser definido para o aplicativo inteiro com o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="2ff6e-159">The maximum number of concurrent open TCP connections can be set for the entire app with the following code:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=3)]

<span data-ttu-id="2ff6e-160">Há um limite separado para conexões que foram atualizadas do HTTP ou HTTPS para outro protocolo (por exemplo, em uma solicitação do WebSockets).</span><span class="sxs-lookup"><span data-stu-id="2ff6e-160">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="2ff6e-161">Depois que uma conexão é atualizada, ela não é contada em relação ao limite de `MaxConcurrentConnections`.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-161">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=4)]

<span data-ttu-id="2ff6e-162">O número máximo de conexões é ilimitado (nulo) por padrão.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-162">The maximum number of connections is unlimited (null) by default.</span></span>

<span data-ttu-id="2ff6e-163">**Tamanho máximo do corpo da solicitação**</span><span class="sxs-lookup"><span data-stu-id="2ff6e-163">**Maximum request body size**</span></span>

[<span data-ttu-id="2ff6e-164">MaxRequestBodySize</span><span class="sxs-lookup"><span data-stu-id="2ff6e-164">MaxRequestBodySize</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize)

<span data-ttu-id="2ff6e-165">O tamanho máximo do corpo da solicitação padrão é de 30.000.000 bytes, que equivale aproximadamente a 28,6 MB.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-165">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span>

<span data-ttu-id="2ff6e-166">A abordagem recomendada para substituir o limite em um aplicativo ASP.NET Core MVC é usar o atributo [RequestSizeLimit](/dotnet/api/microsoft.aspnetcore.mvc.requestsizelimitattribute) em um método de ação:</span><span class="sxs-lookup"><span data-stu-id="2ff6e-166">The recommended approach to override the limit in an ASP.NET Core MVC app is to use the [RequestSizeLimit](/dotnet/api/microsoft.aspnetcore.mvc.requestsizelimitattribute) attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="2ff6e-167">Aqui está um exemplo que mostra como configurar a restrição para o aplicativo em cada solicitação:</span><span class="sxs-lookup"><span data-stu-id="2ff6e-167">Here's an example that shows how to configure the constraint for the app on every request:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=5)]

<span data-ttu-id="2ff6e-168">Substitua a configuração em uma solicitação específica no middleware:</span><span class="sxs-lookup"><span data-stu-id="2ff6e-168">You can override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="2ff6e-169">Uma exceção será gerada se você tentar configurar o limite em uma solicitação depois que o aplicativo começar a ler a solicitação.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-169">An exception is thrown if you attempt to configure the limit on a request after the app has started to read the request.</span></span> <span data-ttu-id="2ff6e-170">Há uma propriedade `IsReadOnly` que indica se a propriedade `MaxRequestBodySize` está no estado somente leitura, o que significa que é tarde demais para configurar o limite.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-170">There's an `IsReadOnly` property that indicates if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="2ff6e-171">**Taxa de dados mínima do corpo da solicitação**</span><span class="sxs-lookup"><span data-stu-id="2ff6e-171">**Minimum request body data rate**</span></span>

[<span data-ttu-id="2ff6e-172">MinRequestBodyDataRate</span><span class="sxs-lookup"><span data-stu-id="2ff6e-172">MinRequestBodyDataRate</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.minrequestbodydatarate)  
[<span data-ttu-id="2ff6e-173">MinResponseDataRate</span><span class="sxs-lookup"><span data-stu-id="2ff6e-173">MinResponseDataRate</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.minresponsedatarate)

<span data-ttu-id="2ff6e-174">O Kestrel verifica a cada segundo se os dados estão sendo recebidos na taxa especificada em bytes/segundo.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-174">Kestrel checks every second if data is arriving at the specified rate in bytes/second.</span></span> <span data-ttu-id="2ff6e-175">Se a taxa cair abaixo do mínimo, a conexão atingirá o tempo limite. O período de cortesia é o tempo que o Kestrel fornece ao cliente para aumentar sua taxa de envio até o mínimo; a taxa não é verificada durante esse período.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-175">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="2ff6e-176">O período de cortesia ajuda a evitar a remoção de conexões que inicialmente enviam dados em uma taxa baixa devido ao início lento do TCP.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-176">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="2ff6e-177">A taxa mínima de padrão é de 240 bytes/segundo com um período de cortesia de 5 segundos.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-177">The default minimum rate is 240 bytes/second with a 5 second grace period.</span></span>

<span data-ttu-id="2ff6e-178">Uma taxa mínima também se aplica à resposta.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-178">A minimum rate also applies to the response.</span></span> <span data-ttu-id="2ff6e-179">O código para definir o limite de solicitação e o limite de resposta é o mesmo, exceto por ter `RequestBody` ou `Response` nos nomes da propriedade e da interface.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-179">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span>

<span data-ttu-id="2ff6e-180">Este é um exemplo que mostra como configurar as taxas mínima de dados em *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="2ff6e-180">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=6-7)]

<span data-ttu-id="2ff6e-181">Configure as taxas por solicitação no middleware:</span><span class="sxs-lookup"><span data-stu-id="2ff6e-181">You can configure the rates per request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=5-8)]

<span data-ttu-id="2ff6e-182">Para obter informações sobre outras opções e limites do Kestrel, confira:</span><span class="sxs-lookup"><span data-stu-id="2ff6e-182">For information about other Kestrel options and limits, see:</span></span>

* [<span data-ttu-id="2ff6e-183">KestrelServerOptions</span><span class="sxs-lookup"><span data-stu-id="2ff6e-183">KestrelServerOptions</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions)
* [<span data-ttu-id="2ff6e-184">KestrelServerLimits</span><span class="sxs-lookup"><span data-stu-id="2ff6e-184">KestrelServerLimits</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits)
* [<span data-ttu-id="2ff6e-185">ListenOptions</span><span class="sxs-lookup"><span data-stu-id="2ff6e-185">ListenOptions</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.listenoptions)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="2ff6e-186">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="2ff6e-186">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="2ff6e-187">Para obter informações sobre as opções e os limites do Kestrel, confira:</span><span class="sxs-lookup"><span data-stu-id="2ff6e-187">For information about Kestrel options and limits, see:</span></span>

* [<span data-ttu-id="2ff6e-188">Classe KestrelServerOptions</span><span class="sxs-lookup"><span data-stu-id="2ff6e-188">KestrelServerOptions class</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions?view=aspnetcore-1.1)
* [<span data-ttu-id="2ff6e-189">KestrelServerLimits</span><span class="sxs-lookup"><span data-stu-id="2ff6e-189">KestrelServerLimits</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserverlimits?view=aspnetcore-1.1)

---

### <a name="endpoint-configuration"></a><span data-ttu-id="2ff6e-190">Configuração do ponto de extremidade</span><span class="sxs-lookup"><span data-stu-id="2ff6e-190">Endpoint configuration</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="2ff6e-191">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="2ff6e-191">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

::: moniker range="= aspnetcore-2.0"
<span data-ttu-id="2ff6e-192">Por padrão, o ASP.NET Core é associado a `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-192">By default, ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="2ff6e-193">Chame os métodos [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) ou [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) em [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) para configurar portas e prefixos de URL para o Kestrel.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-193">Call [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) or [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) methods on [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) to configure URL prefixes and ports for Kestrel.</span></span> <span data-ttu-id="2ff6e-194">`UseUrls`, o argumento de linha de comando `--urls`, a chave de configuração de host `urls` e a variável de ambiente `ASPNETCORE_URLS` também funcionam mas têm limitações que serão indicadas mais adiante nesta seção.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-194">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section.</span></span>

<span data-ttu-id="2ff6e-195">A chave de configuração de host `urls` precisa vir da configuração do host, não da configuração do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-195">The `urls` host configuration key must come from the host configuration, not the app configuration.</span></span> <span data-ttu-id="2ff6e-196">Adicionar uma chave e um valor de `urls` para *appsettings.json* não afeta a configuração do host porque o host está completamente inicializado quando a configuração é lida no arquivo de configuração.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-196">Adding a `urls` key and value to *appsettings.json* doesn't affect host configuration because the host is completely initialized by the time the configuration is read from the configuration file.</span></span> <span data-ttu-id="2ff6e-197">No entanto, uma chave `urls` em *appsettings.json* pode ser usada com [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) no construtor de host para configurar o host:</span><span class="sxs-lookup"><span data-stu-id="2ff6e-197">However, a `urls` key in *appsettings.json* can be used with [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) on the host builder to configure the host:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .SetBasePath(Directory.GetCurrentDirectory())
    .AddJsonFile("appSettings.json", optional: true, reloadOnChange: true)
    .Build();

var host = new WebHostBuilder()
    .UseKestrel()
    .UseConfiguration(config)
    .UseContentRoot(Directory.GetCurrentDirectory())
    .UseStartup<Startup>()
    .Build();
```

::: moniker-end
::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="2ff6e-198">Por padrão, o ASP.NET Core associa a:</span><span class="sxs-lookup"><span data-stu-id="2ff6e-198">By default, ASP.NET Core binds to:</span></span>

* `http://localhost:5000`
* <span data-ttu-id="2ff6e-199">`https://localhost:5001` (quando um certificado de desenvolvimento local está presente)</span><span class="sxs-lookup"><span data-stu-id="2ff6e-199">`https://localhost:5001` (when a local development certificate is present)</span></span>

<span data-ttu-id="2ff6e-200">Um certificado de desenvolvimento é criado:</span><span class="sxs-lookup"><span data-stu-id="2ff6e-200">A development certificate is created:</span></span>

* <span data-ttu-id="2ff6e-201">Quando o [SDK do .NET Core](/dotnet/core/sdk) está instalado.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-201">When the [.NET Core SDK](/dotnet/core/sdk) is installed.</span></span>
* <span data-ttu-id="2ff6e-202">A [ferramenta dev-certs](xref:aspnetcore-2.1#https) é usada para criar um certificado.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-202">The [dev-certs tool](xref:aspnetcore-2.1#https) is used to create a certificate.</span></span>

<span data-ttu-id="2ff6e-203">Alguns navegadores exigem que você conceda permissão explícita para o navegador para confiar no certificado de desenvolvimento local.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-203">Some browsers require that you grant explicit permission to the browser to trust the local development certificate.</span></span>

<span data-ttu-id="2ff6e-204">Os modelos de projeto do ASP.NET Core 2.1 e posteriores configuram os aplicativos para serem executados em HTTPS, por padrão, e incluem o [Redirecionamento de HTTPS e o suporte a HSTS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="2ff6e-204">ASP.NET Core 2.1 and later project templates configure apps to run on HTTPS by default and include [HTTPS redirection and HSTS support](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="2ff6e-205">Chame os métodos [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) ou [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) em [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) para configurar portas e prefixos de URL para o Kestrel.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-205">Call [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) or [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) methods on [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) to configure URL prefixes and ports for Kestrel.</span></span>

<span data-ttu-id="2ff6e-206">`UseUrls`, o argumento de linha de comando `--urls`, a chave de configuração de host `urls` e a variável de ambiente `ASPNETCORE_URLS` também funcionam mas têm limitações que serão indicadas mais adiante nesta seção (um certificado padrão precisa estar disponível para a configuração do ponto de extremidade HTTPS).</span><span class="sxs-lookup"><span data-stu-id="2ff6e-206">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section (a default certificate must be available for HTTPS endpoint configuration).</span></span>

<span data-ttu-id="2ff6e-207">Configuração do ASP.NET Core 2.1 `KestrelServerOptions`:</span><span class="sxs-lookup"><span data-stu-id="2ff6e-207">ASP.NET Core 2.1 `KestrelServerOptions` configuration:</span></span>

<span data-ttu-id="2ff6e-208">**ConfigureEndpointDefaults(Action&lt;ListenOptions&gt;)**</span><span class="sxs-lookup"><span data-stu-id="2ff6e-208">**ConfigureEndpointDefaults(Action&lt;ListenOptions&gt;)**</span></span>  
<span data-ttu-id="2ff6e-209">Especifica uma `Action` de configuração a ser executada para cada ponto de extremidade especificado.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-209">Specifies a configuration `Action` to run for each specified endpoint.</span></span> <span data-ttu-id="2ff6e-210">Chamar `ConfigureEndpointDefaults` várias vezes substitui as `Action`s pela última `Action` especificada.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-210">Calling `ConfigureEndpointDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

<span data-ttu-id="2ff6e-211">**ConfigureHttpsDefaults(Action&lt;HttpsConnectionAdapterOptions&gt;)**</span><span class="sxs-lookup"><span data-stu-id="2ff6e-211">**ConfigureHttpsDefaults(Action&lt;HttpsConnectionAdapterOptions&gt;)**</span></span>  
<span data-ttu-id="2ff6e-212">Especifica uma `Action` de configuração a ser executada para cada ponto de extremidade HTTPS.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-212">Specifies a configuration `Action` to run for each HTTPS endpoint.</span></span> <span data-ttu-id="2ff6e-213">Chamar `ConfigureHttpsDefaults` várias vezes substitui as `Action`s pela última `Action` especificada.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-213">Calling `ConfigureHttpsDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

<span data-ttu-id="2ff6e-214">**Configure(IConfiguration)**</span><span class="sxs-lookup"><span data-stu-id="2ff6e-214">**Configure(IConfiguration)**</span></span>  
<span data-ttu-id="2ff6e-215">Cria um carregador de configuração para configurar o Kestrel que usa uma [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) como entrada.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-215">Creates a configuration loader for setting up Kestrel that takes an [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) as input.</span></span> <span data-ttu-id="2ff6e-216">A configuração precisa estar no escopo da seção de configuração do Kestrel.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-216">The configuration must be scoped to the configuration section for Kestrel.</span></span>

<span data-ttu-id="2ff6e-217">**ListenOptions.UseHttps**</span><span class="sxs-lookup"><span data-stu-id="2ff6e-217">**ListenOptions.UseHttps**</span></span>  
<span data-ttu-id="2ff6e-218">Configure o Kestrel para usar HTTPS.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-218">Configure Kestrel to use HTTPS.</span></span>

<span data-ttu-id="2ff6e-219">Extensões `ListenOptions.UseHttps`:</span><span class="sxs-lookup"><span data-stu-id="2ff6e-219">`ListenOptions.UseHttps` extensions:</span></span>

* <span data-ttu-id="2ff6e-220">`UseHttps` &ndash; Configure o Kestrel para usar HTTPS com o certificado padrão.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-220">`UseHttps` &ndash; Configure Kestrel to use HTTPS with the default certificate.</span></span> <span data-ttu-id="2ff6e-221">Gera uma exceção quando não há nenhum certificado padrão configurado.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-221">Throws an exception if no default certificate is configured.</span></span>
* `UseHttps(string fileName)`
* `UseHttps(string fileName, string password)`
* `UseHttps(string fileName, string password, Action<HttpsConnectionAdapterOptions> configureOptions)`
* `UseHttps(StoreName storeName, string subject)`
* `UseHttps(StoreName storeName, string subject, bool allowInvalid)`
* `UseHttps(StoreName storeName, string subject, bool allowInvalid, StoreLocation location)`
* `UseHttps(StoreName storeName, string subject, bool allowInvalid, StoreLocation location, Action<HttpsConnectionAdapterOptions> configureOptions)`
* `UseHttps(X509Certificate2 serverCertificate)`
* `UseHttps(X509Certificate2 serverCertificate, Action<HttpsConnectionAdapterOptions> configureOptions)`
* `UseHttps(Action<HttpsConnectionAdapterOptions> configureOptions)`

<span data-ttu-id="2ff6e-222">Parâmetros de `ListenOptions.UseHttps`:</span><span class="sxs-lookup"><span data-stu-id="2ff6e-222">`ListenOptions.UseHttps` parameters:</span></span>

* <span data-ttu-id="2ff6e-223">`filename` é o caminho e o nome do arquivo de um arquivo de certificado, relativo ao diretório que contém os arquivos de conteúdo do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-223">`filename` is the path and file name of a certificate file, relative to the directory that contains the app's content files.</span></span>
* <span data-ttu-id="2ff6e-224">`password` é a senha necessária para acessar os dados do certificado X.509 .</span><span class="sxs-lookup"><span data-stu-id="2ff6e-224">`password` is the password required to access the X.509 certificate data.</span></span>
* <span data-ttu-id="2ff6e-225">`configureOptions` é uma `Action` para configurar as `HttpsConnectionAdapterOptions`.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-225">`configureOptions` is an `Action` to configure the `HttpsConnectionAdapterOptions`.</span></span> <span data-ttu-id="2ff6e-226">Retorna o `ListenOptions`.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-226">Returns the `ListenOptions`.</span></span>
* <span data-ttu-id="2ff6e-227">`storeName` é o repositório de certificados do qual o certificado deve ser carregado.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-227">`storeName` is the certificate store from which to load the certificate.</span></span>
* <span data-ttu-id="2ff6e-228">`subject` é o nome da entidade do certificado.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-228">`subject` is the subject name for the certificate.</span></span>
* <span data-ttu-id="2ff6e-229">`allowInvalid` indica se certificados inválidos devem ser considerados, como os certificados autoassinados.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-229">`allowInvalid` indicates if invalid certificates should be considered, such as self-signed certificates.</span></span>
* <span data-ttu-id="2ff6e-230">`location` é o local do repositório do qual o certificado deve ser carregado.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-230">`location` is the store location to load the certificate from.</span></span>
* <span data-ttu-id="2ff6e-231">`serverCertificate` é o certificado X.509.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-231">`serverCertificate` is the X.509 certificate.</span></span>

<span data-ttu-id="2ff6e-232">Em produção, HTTPS precisa ser configurado explicitamente.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-232">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="2ff6e-233">No mínimo, um certificado padrão precisa ser fornecido.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-233">At a minimum, a default certificate must be provided.</span></span>

<span data-ttu-id="2ff6e-234">Configurações com suporte descritas a seguir:</span><span class="sxs-lookup"><span data-stu-id="2ff6e-234">Supported configurations described next:</span></span>

* <span data-ttu-id="2ff6e-235">Nenhuma configuração</span><span class="sxs-lookup"><span data-stu-id="2ff6e-235">No configuration</span></span>
* <span data-ttu-id="2ff6e-236">Substituir o certificado padrão da configuração</span><span class="sxs-lookup"><span data-stu-id="2ff6e-236">Replace the default certificate from configuration</span></span>
* <span data-ttu-id="2ff6e-237">Alterar os padrões no código</span><span class="sxs-lookup"><span data-stu-id="2ff6e-237">Change the defaults in code</span></span>

<span data-ttu-id="2ff6e-238">*Nenhuma configuração*</span><span class="sxs-lookup"><span data-stu-id="2ff6e-238">*No configuration*</span></span>

<span data-ttu-id="2ff6e-239">O Kestrel escuta em `http://localhost:5000` e em `https://localhost:5001` (se houver um certificado padrão disponível).</span><span class="sxs-lookup"><span data-stu-id="2ff6e-239">Kestrel listens on `http://localhost:5000` and `https://localhost:5001` (if a default cert is available).</span></span>

<span data-ttu-id="2ff6e-240">Especificar URLs usando:</span><span class="sxs-lookup"><span data-stu-id="2ff6e-240">Specify URLs using the:</span></span>

* <span data-ttu-id="2ff6e-241">A variável de ambiente `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-241">`ASPNETCORE_URLS` environment variable.</span></span>
* <span data-ttu-id="2ff6e-242">O argumento de linha de comando `--urls`.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-242">`--urls` command-line argument.</span></span>
* <span data-ttu-id="2ff6e-243">A chave de configuração do host `urls`.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-243">`urls` host configuration key.</span></span>
* <span data-ttu-id="2ff6e-244">O método de extensão `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-244">`UseUrls` extension method.</span></span>

<span data-ttu-id="2ff6e-245">Para obter mais informações, confira [URLs de servidor](xref:fundamentals/host/web-host#server-urls) e [Substituir configuração](xref:fundamentals/host/web-host#override-configuration).</span><span class="sxs-lookup"><span data-stu-id="2ff6e-245">For more information, see [Server URLs](xref:fundamentals/host/web-host#server-urls) and [Override configuration](xref:fundamentals/host/web-host#override-configuration).</span></span>

<span data-ttu-id="2ff6e-246">O valor fornecido usando essas abordagens pode ser um ou mais pontos de extremidade HTTP e HTTPS (HTTPS se houver um certificado padrão).</span><span class="sxs-lookup"><span data-stu-id="2ff6e-246">The value provided using these approaches can be one or more HTTP and HTTPS endpoints (HTTPS if a default cert is available).</span></span> <span data-ttu-id="2ff6e-247">Configure o valor como uma lista separada por ponto e vírgula (por exemplo, `"Urls": "http://localhost:8000;http://localhost:8001"`).</span><span class="sxs-lookup"><span data-stu-id="2ff6e-247">Configure the value as a semicolon-separated list (for example, `"Urls": "http://localhost:8000;http://localhost:8001"`).</span></span>

<span data-ttu-id="2ff6e-248">*Substituir o certificado padrão da configuração*</span><span class="sxs-lookup"><span data-stu-id="2ff6e-248">*Replace the default certificate from configuration*</span></span>

<span data-ttu-id="2ff6e-249">[WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) chama `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` por padrão ao carregar a configuração do Kestrel.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-249">[WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) calls `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span> <span data-ttu-id="2ff6e-250">Há um esquema de definições de configurações de aplicativo HTTPS padrão disponível para o Kestrel.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-250">A default HTTPS app settings configuration schema is available for Kestrel.</span></span> <span data-ttu-id="2ff6e-251">Configure vários pontos de extremidade, incluindo URLs e os certificados a serem usados, por meio de um arquivo no disco ou de um repositório de certificados.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-251">Configure multiple endpoints, including the URLs and the certificates to use, either from a file on disk or from a certificate store.</span></span>

<span data-ttu-id="2ff6e-252">No exemplo de *appsettings.json* a seguir:</span><span class="sxs-lookup"><span data-stu-id="2ff6e-252">In the following *appsettings.json* example:</span></span>

* <span data-ttu-id="2ff6e-253">Defina **AllowInvalid** como `true` para permitir o uso de certificados inválidos (por exemplo, os certificados autoassinados).</span><span class="sxs-lookup"><span data-stu-id="2ff6e-253">Set **AllowInvalid** to `true` to permit the use of invalid certificates (for example, self-signed certificates).</span></span>
* <span data-ttu-id="2ff6e-254">Todo ponto de extremidade HTTPS que não especificar um certificado (**HttpsDefaultCert** no exemplo a seguir) será revertido para o certificado definido em **Certificados** > **Padrão** ou para o certificado de desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-254">Any HTTPS endpoint that doesn't specify a certificate (**HttpsDefaultCert** in the example that follows) falls back to the cert defined under **Certificates** > **Default** or the development certificate.</span></span>

```json
{
"Kestrel": {
  "EndPoints": {
    "Http": {
      "Url": "http://localhost:5000"
    },

    "HttpsInlineCertFile": {
      "Url": "https://localhost:5001",
      "Certificate": {
        "Path": "<path to .pfx file>",
        "Password": "<certificate password>"
      }
    },

    "HttpsInlineCertStore": {
      "Url": "https://localhost:5002",
      "Certificate": {
        "Subject": "<subject; required>",
        "Store": "<certificate store; defaults to My>",
        "Location": "<location; defaults to CurrentUser>",
        "AllowInvalid": "<true or false; defaults to false>"
      }
    },

    "HttpsDefaultCert": {
      "Url": "https://localhost:5003"
    },

    "Https": {
      "Url": "https://*:5004",
      "Certificate": {
      "Path": "<path to .pfx file>",
      "Password": "<certificate password>"
      }
    }
    },
    "Certificates": {
      "Default": {
        "Path": "<path to .pfx file>",
        "Password": "<certificate password>"
      }
    }
  }
}
```

<span data-ttu-id="2ff6e-255">Uma alternativa ao uso de **Caminho** e **Senha** para qualquer nó de certificado é especificar o certificado usando campos de repositório de certificados.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-255">An alternative to using **Path** and **Password** for any certificate node is to specify the certificate using certificate store fields.</span></span> <span data-ttu-id="2ff6e-256">Por exemplo, o certificado **Certificados** > **Padrão** pode ser especificado como:</span><span class="sxs-lookup"><span data-stu-id="2ff6e-256">For example, the **Certificates** > **Default** certificate can be specified as:</span></span>

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; defaults to My>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

<span data-ttu-id="2ff6e-257">Observações do esquema:</span><span class="sxs-lookup"><span data-stu-id="2ff6e-257">Schema notes:</span></span>

* <span data-ttu-id="2ff6e-258">Os nomes de pontos de extremidade diferenciam maiúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-258">Endpoints names are case-insensitive.</span></span> <span data-ttu-id="2ff6e-259">Por exemplo, `HTTPS` e `Https` são válidos.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-259">For example, `HTTPS` and `Https` are valid.</span></span>
* <span data-ttu-id="2ff6e-260">O parâmetro `Url` é necessário para cada ponto de extremidade.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-260">The `Url` parameter is required for each endpoint.</span></span> <span data-ttu-id="2ff6e-261">O formato desse parâmetro é o mesmo que o do parâmetro de configuração de `Urls` de nível superior, exceto que ele é limitado a um único valor.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-261">The format for this parameter is the same as the top-level `Urls` configuration parameter except that it's limited to a single value.</span></span>
* <span data-ttu-id="2ff6e-262">Esses pontos de extremidade substituem aqueles definidos na configuração de `Urls` de nível superior em vez de serem adicionados a eles.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-262">These endpoints replace those defined in the top-level `Urls` configuration rather than adding to them.</span></span> <span data-ttu-id="2ff6e-263">Os pontos de extremidade definidos no código por meio de `Listen` são acumulados com os pontos de extremidade definidos na seção de configuração.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-263">Endpoints defined in code via `Listen` are cumulative with the endpoints defined in the configuration section.</span></span>
* <span data-ttu-id="2ff6e-264">A seção `Certificate` é opcional.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-264">The `Certificate` section is optional.</span></span> <span data-ttu-id="2ff6e-265">Se a seção `Certificate` não for especificada, os padrões definidos nos cenários anteriores serão usados.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-265">If the `Certificate` section isn't specified, the defaults defined in earlier scenarios are used.</span></span> <span data-ttu-id="2ff6e-266">Se não houver nenhum padrão disponível, o servidor gerará uma exceção e não poderá ser iniciado.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-266">If no defaults are available, the server throws an exception and fails to start.</span></span>
* <span data-ttu-id="2ff6e-267">A seção `Certificate` é compatível com os certificados de **Caminho**&ndash;**Senha** e **Entidade**&ndash;**Repositório**.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-267">The `Certificate` section supports both **Path**&ndash;**Password** and **Subject**&ndash;**Store** certificates.</span></span>
* <span data-ttu-id="2ff6e-268">Qualquer número de pontos de extremidade pode ser definido dessa forma, contanto que eles não causem conflitos de porta.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-268">Any number of endpoints may be defined in this way so long as they don't cause port conflicts.</span></span>
* <span data-ttu-id="2ff6e-269">`serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` retorna um `KestrelConfigurationLoader` com um método `.Endpoint(string name, options => { })` que pode ser usado para complementar as definições de um ponto de extremidade configurado:</span><span class="sxs-lookup"><span data-stu-id="2ff6e-269">`serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` returns a `KestrelConfigurationLoader` with an `.Endpoint(string name, options => { })` method that can be used to supplement a configured endpoint's settings:</span></span>

  ```csharp
  serverOptions.Configure(context.Configuration.GetSection("Kestrel"))
      .Endpoint("HTTPS", opt =>
      {
          opt.HttpsOptions.SslProtocols = SslProtocols.Tls12;
      });
  ```

  <span data-ttu-id="2ff6e-270">Você também pode acessar o `KestrelServerOptions.ConfigurationLoader` diretamente para continuar a iteração no carregador existente, como aquele fornecido pelo [WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span><span class="sxs-lookup"><span data-stu-id="2ff6e-270">You can also directly access `KestrelServerOptions.ConfigurationLoader` to keep iterating on the existing loader, such as the one provided by [WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span></span>

* <span data-ttu-id="2ff6e-271">A seção de configuração de cada ponto de extremidade está disponível nas opções no método `Endpoint` para que as configurações personalizadas possam ser lidas.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-271">The configuration section for each endpoint is a available on the options in the `Endpoint` method so that custom settings may be read.</span></span>
* <span data-ttu-id="2ff6e-272">Várias configurações podem ser carregadas chamando `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` novamente com outra seção.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-272">Multiple configurations may be loaded by calling `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` again with another section.</span></span> <span data-ttu-id="2ff6e-273">Somente a última configuração será usada, a menos que `Load` seja chamado explicitamente nas instâncias anteriores.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-273">Only the last configuration is used, unless `Load` is explicitly called on prior instances.</span></span> <span data-ttu-id="2ff6e-274">O metapacote não chama `Load`, portanto, sua seção de configuração padrão pode ser substituída.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-274">The metapackage doesn't call `Load` so that its default configuration section may be replaced.</span></span>
* <span data-ttu-id="2ff6e-275">O `KestrelConfigurationLoader` espelha a família de APIs `Listen` de `KestrelServerOptions` como sobrecargas de `Endpoint`, portanto, os pontos de extremidade de código e de configuração podem ser configurados no mesmo local.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-275">`KestrelConfigurationLoader` mirrors the `Listen` family of APIs from `KestrelServerOptions` as `Endpoint` overloads, so code and config endpoints may be configured in the same place.</span></span> <span data-ttu-id="2ff6e-276">Essas sobrecargas não usam nomes e consomem somente as definições padrão da configuração.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-276">These overloads don't use names and only consume default settings from configuration.</span></span>

<span data-ttu-id="2ff6e-277">*Alterar os padrões no código*</span><span class="sxs-lookup"><span data-stu-id="2ff6e-277">*Change the defaults in code*</span></span>

<span data-ttu-id="2ff6e-278">`ConfigureEndpointDefaults` e `ConfigureHttpsDefaults` podem ser usados para alterar as configurações padrão de `ListenOptions` e `HttpsConnectionAdapterOptions`, incluindo a substituição do certificado padrão especificado no cenário anterior.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-278">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` can be used to change default settings for `ListenOptions` and `HttpsConnectionAdapterOptions`, including overriding the default certificate specified in the prior scenario.</span></span> <span data-ttu-id="2ff6e-279">`ConfigureEndpointDefaults` e `ConfigureHttpsDefaults` devem ser chamados antes que qualquer ponto de extremidade seja configurado.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-279">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` should be called before any endpoints are configured.</span></span>

```csharp
options.ConfigureEndpointDefaults(opt =>
{
    opt.NoDelay = true;
});

options.ConfigureHttpsDefaults(httpsOptions =>
{
    httpsOptions.SslProtocols = SslProtocols.Tls12;
});
```

<span data-ttu-id="2ff6e-280">*Suporte do Kestrel para SNI*</span><span class="sxs-lookup"><span data-stu-id="2ff6e-280">*Kestrel support for SNI*</span></span>

<span data-ttu-id="2ff6e-281">A [SNI (Indicação de Nome de Servidor)](https://tools.ietf.org/html/rfc6066#section-3) pode ser usada para hospedar vários domínios no mesmo endereço IP e na mesma porta.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-281">[Server Name Indication (SNI)](https://tools.ietf.org/html/rfc6066#section-3) can be used to host multiple domains on the same IP address and port.</span></span> <span data-ttu-id="2ff6e-282">Para que a SNI funcione, o cliente envia o nome do host da sessão segura para o servidor durante o handshake TLS para que o servidor possa fornecer o certificado correto.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-282">For SNI to function, the client sends the host name for the secure session to the server during the TLS handshake so that the server can provide the correct certificate.</span></span> <span data-ttu-id="2ff6e-283">O cliente usa o certificado fornecido para a comunicação criptografada com o servidor durante a sessão segura que segue o handshake TLS.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-283">The client uses the furnished certificate for encrypted communication with the server during the secure session that follows the TLS handshake.</span></span>

<span data-ttu-id="2ff6e-284">O Kestrel permite a SNI por meio do retorno de chamada do `ServerCertificateSelector`.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-284">Kestrel supports SNI via the `ServerCertificateSelector` callback.</span></span> <span data-ttu-id="2ff6e-285">O retorno de chamada é invocado uma vez por conexão para permitir que o aplicativo inspecione o nome do host e selecione o certificado apropriado.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-285">The callback is invoked once per connection to allow the app to inspect the host name and select the appropriate certificate.</span></span>

<span data-ttu-id="2ff6e-286">O suporte para SNI requer:</span><span class="sxs-lookup"><span data-stu-id="2ff6e-286">SNI support requires:</span></span>

* <span data-ttu-id="2ff6e-287">Execução na estrutura de destino `netcoreapp2.1`.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-287">Running on target framework `netcoreapp2.1`.</span></span> <span data-ttu-id="2ff6e-288">No `netcoreapp2.0` e no `net461`, o retorno de chamada é invocado, mas o `name` é sempre `null`.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-288">On `netcoreapp2.0` and `net461`, the callback is invoked but the `name` is always `null`.</span></span> <span data-ttu-id="2ff6e-289">O `name` também será `null` se o cliente não fornecer o parâmetro de nome do host no handshake TLS.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-289">The `name` is also `null` if the client doesn't provide the host name parameter in the TLS handshake.</span></span>
* <span data-ttu-id="2ff6e-290">Todos os sites executados na mesma instância do Kestrel.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-290">All websites run on the same Kestrel instance.</span></span> <span data-ttu-id="2ff6e-291">Kestrel não é compatível com o compartilhamento de endereço IP e porta entre várias instâncias sem um proxy reverso.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-291">Kestrel doesn't support sharing an IP address and port across multiple instances without a reverse proxy.</span></span>

```csharp
WebHost.CreateDefaultBuilder()
    .UseKestrel((context, options) =>
    {
        options.ListenAnyIP(5005, listenOptions =>
        {
            listenOptions.UseHttps(httpsOptions =>
            {
                var localhostCert = CertificateLoader.LoadFromStoreCert(
                    "localhost", "My", StoreLocation.CurrentUser, 
                    allowInvalid: true);
                var exampleCert = CertificateLoader.LoadFromStoreCert(
                    "example.com", "My", StoreLocation.CurrentUser, 
                    allowInvalid: true);
                var subExampleCert = CertificateLoader.LoadFromStoreCert(
                    "sub.example.com", "My", StoreLocation.CurrentUser, 
                    allowInvalid: true);
                var certs = new Dictionary(StringComparer.OrdinalIgnoreCase);
                certs["localhost"] = localhostCert;
                certs["example.com"] = exampleCert;
                certs["sub.example.com"] = subExampleCert;

                httpsOptions.ServerCertificateSelector = (connectionContext, name) =>
                {
                    if (name != null && certs.TryGetValue(name, out var cert))
                    {
                        return cert;
                    }

                    return exampleCert;
                };
            });
        });
    });
```

::: moniker-end

<span data-ttu-id="2ff6e-292">**Associar a um soquete TCP**</span><span class="sxs-lookup"><span data-stu-id="2ff6e-292">**Bind to a TCP socket**</span></span>

<span data-ttu-id="2ff6e-293">O método [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) é associado a um soquete TCP e um lambda de opções permite a configuração do certificado SSL:</span><span class="sxs-lookup"><span data-stu-id="2ff6e-293">The [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) method binds to a TCP socket, and an options lambda permits SSL certificate configuration:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_TCPSocket&highlight=9-16)]

<span data-ttu-id="2ff6e-294">O exemplo configura o SSL para um ponto de extremidade com [ListenOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.listenoptions).</span><span class="sxs-lookup"><span data-stu-id="2ff6e-294">The example configures SSL for an endpoint with [ListenOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.listenoptions).</span></span> <span data-ttu-id="2ff6e-295">Use a mesma API para definir outras configurações do Kestrel para pontos de extremidade específicos.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-295">Use the same API to configure other Kestrel settings for specific endpoints.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

<span data-ttu-id="2ff6e-296">**Associar a um soquete do UNIX**</span><span class="sxs-lookup"><span data-stu-id="2ff6e-296">**Bind to a Unix socket**</span></span>

<span data-ttu-id="2ff6e-297">Escute em um soquete Unix com [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) para melhorar o desempenho com o Nginx, como é mostrado neste exemplo:</span><span class="sxs-lookup"><span data-stu-id="2ff6e-297">Listen on a Unix socket with [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) for improved performance with Nginx, as shown in this example:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_UnixSocket)]

<span data-ttu-id="2ff6e-298">**Porta 0**</span><span class="sxs-lookup"><span data-stu-id="2ff6e-298">**Port 0**</span></span>

<span data-ttu-id="2ff6e-299">Quando o número da porta `0` for especificado, o Kestrel se associará dinamicamente a uma porta disponível.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-299">When the port number `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="2ff6e-300">O exemplo a seguir mostra como determinar a qual porta o Kestrel realmente se associou no tempo de execução:</span><span class="sxs-lookup"><span data-stu-id="2ff6e-300">The following example shows how to determine which port Kestrel actually bound at runtime:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Port0&highlight=3)]

<span data-ttu-id="2ff6e-301">Quando o aplicativo é executado, a saída da janela do console indica a porta dinâmica na qual o aplicativo pode ser acessado:</span><span class="sxs-lookup"><span data-stu-id="2ff6e-301">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Now listening on: http://127.0.0.1:48508
```

<span data-ttu-id="2ff6e-302">Limitações do **UseUrls, do argumento de linha de comando --urls, da chave de configuração de host urls e da variável de ambiente ASPNETCORE_URLS**</span><span class="sxs-lookup"><span data-stu-id="2ff6e-302">**UseUrls, --urls command-line argument, urls host configuration key, and ASPNETCORE_URLS environment variable limitations**</span></span>

<span data-ttu-id="2ff6e-303">Configure pontos de extremidade com as seguintes abordagens:</span><span class="sxs-lookup"><span data-stu-id="2ff6e-303">Configure endpoints with the following approaches:</span></span>

* [<span data-ttu-id="2ff6e-304">UseUrls</span><span class="sxs-lookup"><span data-stu-id="2ff6e-304">UseUrls</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls)
* <span data-ttu-id="2ff6e-305">O argumento de linha de comando `--urls`</span><span class="sxs-lookup"><span data-stu-id="2ff6e-305">`--urls` command-line argument</span></span>
* <span data-ttu-id="2ff6e-306">A chave de configuração do host `urls`</span><span class="sxs-lookup"><span data-stu-id="2ff6e-306">`urls` host configuration key</span></span>
* <span data-ttu-id="2ff6e-307">A variável de ambiente `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="2ff6e-307">`ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="2ff6e-308">Esses métodos são úteis para fazer com que o código funcione com servidores que não sejam o Kestrel.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-308">These methods are useful for making code work with servers other than Kestrel.</span></span> <span data-ttu-id="2ff6e-309">No entanto, esteja ciente dessas limitações:</span><span class="sxs-lookup"><span data-stu-id="2ff6e-309">However, be aware of these limitations:</span></span>

* <span data-ttu-id="2ff6e-310">O protocolo SSL não pode ser usado com essas abordagens, a menos que um certificado padrão seja fornecido na configuração do ponto de extremidade HTTPS (por exemplo, usando a configuração `KestrelServerOptions` ou um arquivo de configuração, como já foi mostrado neste tópico).</span><span class="sxs-lookup"><span data-stu-id="2ff6e-310">SSL can't be used with these approaches unless a default certificate is provided in the HTTPS endpoint configuration (for example, using `KestrelServerOptions` configuration or a configuration file as shown earlier in this topic).</span></span>
* <span data-ttu-id="2ff6e-311">Quando ambas as abordagens, `Listen` e `UseUrls`, são usadas ao mesmo tempo, os pontos de extremidade de `Listen` substituem os pontos de extremidade de `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-311">When both the `Listen` and `UseUrls` approaches are used simultaneously, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

<span data-ttu-id="2ff6e-312">**Configuração de ponto de extremidade do IIS**</span><span class="sxs-lookup"><span data-stu-id="2ff6e-312">**IIS endpoint configuration**</span></span>

<span data-ttu-id="2ff6e-313">Ao usar o IIS, as associações de URL para IIS substituem as associações definidas por `Listen` ou `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-313">When using IIS, the URL bindings for IIS override bindings are set by either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="2ff6e-314">Para obter mais informações, confira o tópico [Módulo do ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="2ff6e-314">For more information, see the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) topic.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="2ff6e-315">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="2ff6e-315">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="2ff6e-316">Por padrão, o ASP.NET Core é associado a `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-316">By default, ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="2ff6e-317">Configure prefixos de URL e portas para o Kestrel usando:</span><span class="sxs-lookup"><span data-stu-id="2ff6e-317">Configure URL prefixes and ports for Kestrel using:</span></span>

* <span data-ttu-id="2ff6e-318">O método de extensão [UseUrls](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls?view=aspnetcore-1.1)</span><span class="sxs-lookup"><span data-stu-id="2ff6e-318">[UseUrls](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls?view=aspnetcore-1.1) extension method</span></span>
* <span data-ttu-id="2ff6e-319">O argumento de linha de comando `--urls`</span><span class="sxs-lookup"><span data-stu-id="2ff6e-319">`--urls` command-line argument</span></span>
* <span data-ttu-id="2ff6e-320">A chave de configuração do host `urls`</span><span class="sxs-lookup"><span data-stu-id="2ff6e-320">`urls` host configuration key</span></span>
* <span data-ttu-id="2ff6e-321">O sistema de configuração do ASP.NET Core, incluindo a variável de ambiente `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="2ff6e-321">ASP.NET Core configuration system, including `ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="2ff6e-322">Para obter mais informações sobre esses métodos, confira [Hospedagem](xref:fundamentals/host/index).</span><span class="sxs-lookup"><span data-stu-id="2ff6e-322">For more information on these methods, see [Hosting](xref:fundamentals/host/index).</span></span>

<span data-ttu-id="2ff6e-323">**Configuração de ponto de extremidade do IIS**</span><span class="sxs-lookup"><span data-stu-id="2ff6e-323">**IIS endpoint configuration**</span></span>

<span data-ttu-id="2ff6e-324">Ao usar o IIS, as associações de URL para o IIS substituam as associações definidas por `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-324">When using IIS, the URL bindings for IIS override bindings set by `UseUrls`.</span></span> <span data-ttu-id="2ff6e-325">Para obter mais informações, confira o tópico [Módulo do ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="2ff6e-325">For more information, see the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) topic.</span></span>

---

::: moniker range=">= aspnetcore-2.1"

## <a name="transport-configuration"></a><span data-ttu-id="2ff6e-326">Configuração de transporte</span><span class="sxs-lookup"><span data-stu-id="2ff6e-326">Transport configuration</span></span>

<span data-ttu-id="2ff6e-327">Com a liberação do ASP.NET Core 2.1, o transporte padrão do Kestrel deixa de ser baseado no Libuv, baseando-se agora em soquetes gerenciados.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-327">With the release of ASP.NET Core 2.1, Kestrel's default transport is no longer based on Libuv but instead based on managed sockets.</span></span> <span data-ttu-id="2ff6e-328">Essa é uma alteração da falha para aplicativos ASP.NET Core 2.0 que atualizam para o 2.1 que chamam [WebHostBuilderLibuvExtensions.UseLibuv](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderlibuvextensions.uselibuv) e dependem de um dos seguintes pacotes:</span><span class="sxs-lookup"><span data-stu-id="2ff6e-328">This is a breaking change for ASP.NET Core 2.0 apps upgrading to 2.1 that call [WebHostBuilderLibuvExtensions.UseLibuv](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderlibuvextensions.uselibuv) and depend on either of the following packages:</span></span>

* <span data-ttu-id="2ff6e-329">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (referência direta de pacote)</span><span class="sxs-lookup"><span data-stu-id="2ff6e-329">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (direct package reference)</span></span>
* [<span data-ttu-id="2ff6e-330">Microsoft.AspNetCore.App</span><span class="sxs-lookup"><span data-stu-id="2ff6e-330">Microsoft.AspNetCore.App</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)

<span data-ttu-id="2ff6e-331">Para projetos do ASP.NET Core 2.1 ou posteriores que usam o [metapacote Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) e exigem o uso de Libuv:</span><span class="sxs-lookup"><span data-stu-id="2ff6e-331">For ASP.NET Core 2.1 or later projects that use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) and require the use of Libuv:</span></span>

* <span data-ttu-id="2ff6e-332">Adicione uma dependência para o pacote [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) ao arquivo de projeto do aplicativo:</span><span class="sxs-lookup"><span data-stu-id="2ff6e-332">Add a dependency for the [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) package to the app's project file:</span></span>

    ```xml
    <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv" 
                      Version="2.1.0" />
    ```

* <span data-ttu-id="2ff6e-333">Chame [WebHostBuilderLibuvExtensions.UseLibuv](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderlibuvextensions.uselibuv):</span><span class="sxs-lookup"><span data-stu-id="2ff6e-333">Call [WebHostBuilderLibuvExtensions.UseLibuv](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderlibuvextensions.uselibuv):</span></span>

    ```csharp
    public class Program
    {
        public static void Main(string[] args)
        {
            CreateWebHostBuilder(args).Build().Run();
        }

        public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
            WebHost.CreateDefaultBuilder(args)
                .UseLibuv()
                .UseStartup<Startup>();
    }
    ```

::: moniker-end

### <a name="url-prefixes"></a><span data-ttu-id="2ff6e-334">Prefixos de URL</span><span class="sxs-lookup"><span data-stu-id="2ff6e-334">URL prefixes</span></span>

<span data-ttu-id="2ff6e-335">Ao usar `UseUrls`, o argumento de linha de comando `--urls`, a chave de configuração de host `urls` ou a variável de ambiente `ASPNETCORE_URLS`, os prefixos de URL podem estar em um dos formatos a seguir.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-335">When using `UseUrls`, `--urls` command-line argument, `urls` host configuration key, or `ASPNETCORE_URLS` environment variable, the URL prefixes can be in any of the following formats.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="2ff6e-336">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="2ff6e-336">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="2ff6e-337">Somente os prefixos de URL HTTP são válidos.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-337">Only HTTP URL prefixes are valid.</span></span> <span data-ttu-id="2ff6e-338">O Kestrel não é compatível com SSL ao configurar associações de URL usando `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-338">Kestrel doesn't support SSL when configuring URL bindings using `UseUrls`.</span></span>

* <span data-ttu-id="2ff6e-339">Endereço IPv4 com o número da porta</span><span class="sxs-lookup"><span data-stu-id="2ff6e-339">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="2ff6e-340">`0.0.0.0` é um caso especial que associa a todos os endereços IPv4.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-340">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="2ff6e-341">Endereço IPv6 com número da porta</span><span class="sxs-lookup"><span data-stu-id="2ff6e-341">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  <span data-ttu-id="2ff6e-342">`[::]` é o equivalente do IPv6 ao IPv4 `0.0.0.0`.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-342">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="2ff6e-343">Nome do host com o número da porta</span><span class="sxs-lookup"><span data-stu-id="2ff6e-343">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="2ff6e-344">Os nomes de host `*` e `+` não são especiais.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-344">Host names, `*`, and `+`, aren't special.</span></span> <span data-ttu-id="2ff6e-345">Tudo o que não é reconhecido como um endereço IP ou um `localhost` válido é associado a todos os IPs IPv6 e IPv4.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-345">Anything not recognized as a valid IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="2ff6e-346">Para associar nomes de host diferentes a diferentes aplicativos ASP.NET Core na mesma porta, use o [HTTP.sys](xref:fundamentals/servers/httpsys) ou um servidor proxy reverso, como o IIS, o Nginx ou o Apache.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-346">To bind different host names to different ASP.NET Core apps on the same port, use [HTTP.sys](xref:fundamentals/servers/httpsys) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

  > [!WARNING]
  > <span data-ttu-id="2ff6e-347">Se você não está usando um proxy reverso com a filtragem de host habilitada, habilite a [filtragem de host](#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="2ff6e-347">If not using a reverse proxy with host filtering enabled, enable [host filtering](#host-filtering).</span></span>

* <span data-ttu-id="2ff6e-348">Nome do `localhost` do host com o número da porta ou o IP de loopback com o número da porta</span><span class="sxs-lookup"><span data-stu-id="2ff6e-348">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="2ff6e-349">Quando o `localhost` for especificado, o Kestrel tentará se associar às interfaces de loopback IPv4 e IPv6.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-349">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="2ff6e-350">Se a porta solicitada está sendo usada por outro serviço em uma das interfaces de loopback, o Kestrel falha ao ser iniciado.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-350">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="2ff6e-351">Se uma das interfaces de loopback não estiver disponível por qualquer outro motivo (geralmente porque não há suporte para o IPv6), o Kestrel registra um aviso em log.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-351">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="2ff6e-352">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="2ff6e-352">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

* <span data-ttu-id="2ff6e-353">Endereço IPv4 com o número da porta</span><span class="sxs-lookup"><span data-stu-id="2ff6e-353">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  https://65.55.39.10:443/
  ```

  <span data-ttu-id="2ff6e-354">`0.0.0.0` é um caso especial que associa a todos os endereços IPv4.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-354">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="2ff6e-355">Endereço IPv6 com número da porta</span><span class="sxs-lookup"><span data-stu-id="2ff6e-355">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  https://[0:0:0:0:0:ffff:4137:270a]:443/
  ```

  <span data-ttu-id="2ff6e-356">`[::]` é o equivalente do IPv6 ao IPv4 `0.0.0.0`.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-356">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="2ff6e-357">Nome do host com o número da porta</span><span class="sxs-lookup"><span data-stu-id="2ff6e-357">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  https://contoso.com:443/
  https://*:443/
  ```

  <span data-ttu-id="2ff6e-358">Nomes de host, `*` e `+` não são especiais.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-358">Host names, `*`, and `+` aren't special.</span></span> <span data-ttu-id="2ff6e-359">Tudo o que não é um endereço IP ou um `localhost` reconhecido é associado a todos os IPs IPv6 e IPv4.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-359">Anything that isn't a recognized IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="2ff6e-360">Para associar nomes de host diferentes a diferentes aplicativos ASP.NET Core na mesma porta, use [WebListener](xref:fundamentals/servers/weblistener) ou um servidor proxy reverso, como o IIS, o Nginx ou o Apache.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-360">To bind different host names to different ASP.NET Core apps on the same port, use [WebListener](xref:fundamentals/servers/weblistener) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

* <span data-ttu-id="2ff6e-361">Nome do `localhost` do host com o número da porta ou o IP de loopback com o número da porta</span><span class="sxs-lookup"><span data-stu-id="2ff6e-361">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="2ff6e-362">Quando o `localhost` for especificado, o Kestrel tentará se associar às interfaces de loopback IPv4 e IPv6.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-362">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="2ff6e-363">Se a porta solicitada está sendo usada por outro serviço em uma das interfaces de loopback, o Kestrel falha ao ser iniciado.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-363">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="2ff6e-364">Se uma das interfaces de loopback não estiver disponível por qualquer outro motivo (geralmente porque não há suporte para o IPv6), o Kestrel registra um aviso em log.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-364">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

* <span data-ttu-id="2ff6e-365">Soquete do UNIX</span><span class="sxs-lookup"><span data-stu-id="2ff6e-365">Unix socket</span></span>

  ```
  http://unix:/run/dan-live.sock
  ```

<span data-ttu-id="2ff6e-366">**Porta 0**</span><span class="sxs-lookup"><span data-stu-id="2ff6e-366">**Port 0**</span></span>

<span data-ttu-id="2ff6e-367">Quando o número da porta `0` for especificado, o Kestrel se associará dinamicamente a uma porta disponível.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-367">When the port number is `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="2ff6e-368">A associação à porta `0` é permitida para qualquer nome do host ou IP, exceto `localhost`.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-368">Binding to port `0` is allowed for any host name or IP except for `localhost`.</span></span>

<span data-ttu-id="2ff6e-369">Quando o aplicativo é executado, a saída da janela do console indica a porta dinâmica na qual o aplicativo pode ser acessado:</span><span class="sxs-lookup"><span data-stu-id="2ff6e-369">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Now listening on: http://127.0.0.1:48508
```

<span data-ttu-id="2ff6e-370">**Prefixos de URL para o SSL**</span><span class="sxs-lookup"><span data-stu-id="2ff6e-370">**URL prefixes for SSL**</span></span>

<span data-ttu-id="2ff6e-371">Se você estiver chamando o método de extensão `UseHttps`, lembre-se de incluir os prefixos de URL com `https:`:</span><span class="sxs-lookup"><span data-stu-id="2ff6e-371">If calling the `UseHttps` extension method, be sure to include URL prefixes with `https:`:</span></span>

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
> <span data-ttu-id="2ff6e-372">HTTPS e HTTP não podem ser hospedados na mesma porta.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-372">HTTPS and HTTP can't be hosted on the same port.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

---

## <a name="host-filtering"></a><span data-ttu-id="2ff6e-373">Filtragem de host</span><span class="sxs-lookup"><span data-stu-id="2ff6e-373">Host filtering</span></span>

<span data-ttu-id="2ff6e-374">Embora o Kestrel permita a configuração com base em prefixos como `http://example.com:5000`, ele geralmente ignora o nome do host.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-374">While Kestrel supports configuration based on prefixes such as `http://example.com:5000`, Kestrel largely ignores the host name.</span></span> <span data-ttu-id="2ff6e-375">O host `localhost` é um caso especial usado para a associação a endereços de loopback.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-375">Host `localhost` is a special case used for binding to loopback addresses.</span></span> <span data-ttu-id="2ff6e-376">Todo host que não for um endereço IP explícito será associado a todos os endereços IP públicos.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-376">Any host other than an explicit IP address binds to all public IP addresses.</span></span> <span data-ttu-id="2ff6e-377">Nenhuma dessas informações é usada para validar os cabeçalhos do `Host` da solicitação.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-377">None of this information is used to validate request `Host` headers.</span></span>

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="2ff6e-378">Como uma solução alternativa, o host por trás de um proxy reverso com filtragem de cabeçalho do host.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-378">As a workaround, host behind a reverse proxy with host header filtering.</span></span> <span data-ttu-id="2ff6e-379">Esse é o único cenário compatível com Kestrel no ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-379">This is the only supported scenario for Kestrel in ASP.NET Core 1.x.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="2ff6e-380">Como uma solução alternativa, use middleware para filtrar as solicitações pelo cabeçalho `Host`:</span><span class="sxs-lookup"><span data-stu-id="2ff6e-380">As a workaround, use middleware to filter requests by the `Host` header:</span></span>

```csharp
using Microsoft.AspNetCore.Http;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.Logging;
using Microsoft.Extensions.Primitives;
using Microsoft.Net.Http.Headers;
using System;
using System.Collections.Generic;
using System.Threading.Tasks;

// A normal middleware would provide an options type, config binding, extension methods, etc..
// This intentionally does all of the work inside of the middleware so it can be
// easily copy-pasted into docs and other projects.
public class HostFilteringMiddleware
{
    private readonly RequestDelegate _next;
    private readonly IList<string> _hosts;
    private readonly ILogger<HostFilteringMiddleware> _logger;

    public HostFilteringMiddleware(RequestDelegate next, IConfiguration config, ILogger<HostFilteringMiddleware> logger)
    {
        if (config == null)
        {
            throw new ArgumentNullException(nameof(config));
        }

        _next = next ?? throw new ArgumentNullException(nameof(next));
        _logger = logger ?? throw new ArgumentNullException(nameof(logger));

        // A semicolon separated list of host names without the port numbers.
        // IPv6 addresses must use the bounding brackets and be in their normalized form.
        _hosts = config["AllowedHosts"]?.Split(new[] { ';' }, StringSplitOptions.RemoveEmptyEntries);
        if (_hosts == null || _hosts.Count == 0)
        {
            throw new InvalidOperationException("No configuration entry found for AllowedHosts.");
        }
    }

    public Task Invoke(HttpContext context)
    {
        if (!ValidateHost(context))
        {
            context.Response.StatusCode = 400;
            _logger.LogDebug("Request rejected due to incorrect Host header.");
            return Task.CompletedTask;
        }

        return _next(context);
    }

    // This does not duplicate format validations that are expected to be performed by the host.
    private bool ValidateHost(HttpContext context)
    {
        StringSegment host = context.Request.Headers[HeaderNames.Host].ToString().Trim();

        if (StringSegment.IsNullOrEmpty(host))
        {
            // Http/1.0 does not require the Host header.
            // Http/1.1 requires the header but the value may be empty.
            return true;
        }

        // Drop the port

        var colonIndex = host.LastIndexOf(':');

        // IPv6 special case
        if (host.StartsWith("[", StringComparison.Ordinal))
        {
            var endBracketIndex = host.IndexOf(']');
            if (endBracketIndex < 0)
            {
                // Invalid format
                return false;
            }
            if (colonIndex < endBracketIndex)
            {
                // No port, just the IPv6 Host
                colonIndex = -1;
            }
        }

        if (colonIndex > 0)
        {
            host = host.Subsegment(0, colonIndex);
        }

        foreach (var allowedHost in _hosts)
        {
            if (StringSegment.Equals(allowedHost, host, StringComparison.OrdinalIgnoreCase))
            {
                return true;
            }

            // Sub-domain wildcards: *.example.com
            if (allowedHost.StartsWith("*.", StringComparison.Ordinal) && host.Length >= allowedHost.Length)
            {
                // .example.com
                var allowedRoot = new StringSegment(allowedHost, 1, allowedHost.Length - 1);

                var hostRoot = host.Subsegment(host.Length - allowedRoot.Length, allowedRoot.Length);
                if (hostRoot.Equals(allowedRoot, StringComparison.OrdinalIgnoreCase))
                {
                    return true;
                }
            }
        }

        return false;
    }
}
```

<span data-ttu-id="2ff6e-381">Registre o `HostFilteringMiddleware` anterior em `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-381">Register the preceding `HostFilteringMiddleware` in `Startup.Configure`.</span></span> <span data-ttu-id="2ff6e-382">Observe que a [ordem do registro de middleware](xref:fundamentals/middleware/index#ordering) é importante.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-382">Note that the [ordering of middleware registration](xref:fundamentals/middleware/index#ordering) is important.</span></span> <span data-ttu-id="2ff6e-383">O registro deve ocorrer imediatamente após o registro do middleware de diagnóstico (por exemplo, `app.UseExceptionHandler`).</span><span class="sxs-lookup"><span data-stu-id="2ff6e-383">Registration should occur immediately after Diagnostic Middleware registration (for example, `app.UseExceptionHandler`).</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
        app.UseBrowserLink();
    }
    else
    {
        app.UseExceptionHandler("/Home/Error");
    }

    app.UseMiddleware<HostFilteringMiddleware>();

    app.UseMvcWithDefaultRoute();
}
```

<span data-ttu-id="2ff6e-384">O middleware espera uma chave `AllowedHosts` em *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-384">The middleware expects an `AllowedHosts` key in *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="2ff6e-385">O valor dessa chave é uma lista separada por ponto e vírgula de nomes de host sem números de porta:</span><span class="sxs-lookup"><span data-stu-id="2ff6e-385">The value is a semicolon-delimited list of host names without port numbers:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="2ff6e-386">Como uma solução alternativa, use o Middleware de Filtragem de Host.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-386">As a workaround, use Host Filtering Middleware.</span></span> <span data-ttu-id="2ff6e-387">Middleware de Filtragem de Host é fornecido pelo pacote [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering), que está incluído no [metapacote Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 ou posterior).</span><span class="sxs-lookup"><span data-stu-id="2ff6e-387">Host Filtering Middleware is provided by the [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) package, which is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later).</span></span> <span data-ttu-id="2ff6e-388">O middleware é adicionado por [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), que chama [AddHostFiltering](/dotnet/api/microsoft.aspnetcore.builder.hostfilteringservicesextensions.addhostfiltering):</span><span class="sxs-lookup"><span data-stu-id="2ff6e-388">The middleware is added by [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), which calls [AddHostFiltering](/dotnet/api/microsoft.aspnetcore.builder.hostfilteringservicesextensions.addhostfiltering):</span></span>

<span data-ttu-id="2ff6e-389">[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]</span><span class="sxs-lookup"><span data-stu-id="2ff6e-389">[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]</span></span>

<span data-ttu-id="2ff6e-390">Middleware de Filtragem de Host está desabilitado por padrão.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-390">Host Filtering Middleware is disabled by default.</span></span> <span data-ttu-id="2ff6e-391">Para habilitar o middleware, defina uma chave `AllowedHosts` em *appSettings. JSON*/*appsettings.\<EnvironmentName>.json*.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-391">To enable the middleware, define an `AllowedHosts` key in *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="2ff6e-392">O valor dessa chave é uma lista separada por ponto e vírgula de nomes de host sem números de porta:</span><span class="sxs-lookup"><span data-stu-id="2ff6e-392">The value is a semicolon-delimited list of host names without port numbers:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="2ff6e-393">*appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="2ff6e-393">*appsettings.json*:</span></span>

```json
{
  "AllowedHosts": "example.com;localhost"
}
```

> [!NOTE]
> <span data-ttu-id="2ff6e-394">[Middleware de Cabeçalhos Encaminhados](xref:host-and-deploy/proxy-load-balancer) também tem uma opção [ForwardedHeadersOptions.AllowedHosts](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.allowedhosts).</span><span class="sxs-lookup"><span data-stu-id="2ff6e-394">[Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) also has an [ForwardedHeadersOptions.AllowedHosts](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.allowedhosts) option.</span></span> <span data-ttu-id="2ff6e-395">Middleware de Cabeçalhos Encaminhados e Middleware de filtragem de Host têm funcionalidades semelhantes para cenários diferentes.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-395">Forwarded Headers Middleware and Host Filtering Middleware have similar functionality for different scenarios.</span></span> <span data-ttu-id="2ff6e-396">Definir `AllowedHosts` com Middleware de Cabeçalhos Encaminhados é apropriado quando o cabeçalho do Host não é preservado ao encaminhar solicitações com um servidor proxy reverso ou balanceador de carga.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-396">Setting `AllowedHosts` with Forwarded Headers Middleware is appropriate when the Host header isn't preserved while forwarding requests with a reverse proxy server or load balancer.</span></span> <span data-ttu-id="2ff6e-397">Definir `AllowedHosts` com Middleware de Filtragem de Host é apropriado quando Kestrel é usado como um servidor de borda ou quando o cabeçalho de Host é encaminhado diretamente.</span><span class="sxs-lookup"><span data-stu-id="2ff6e-397">Setting `AllowedHosts` with Host Filtering Middleware is appropriate when Kestrel is used as an edge server or when the Host header is directly forwarded.</span></span>
>
> <span data-ttu-id="2ff6e-398">Para obter mais informações sobre Middleware de Cabeçalhos Encaminhados, veja [Configurar o ASP.NET Core para funcionar com servidores proxy e balanceadores de carga](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="2ff6e-398">For more information on Forwarded Headers Middleware, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="2ff6e-399">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="2ff6e-399">Additional resources</span></span>

* [<span data-ttu-id="2ff6e-400">Impor o HTTPS</span><span class="sxs-lookup"><span data-stu-id="2ff6e-400">Enforce HTTPS</span></span>](xref:security/enforcing-ssl)
* [<span data-ttu-id="2ff6e-401">Código-fonte do kestrel</span><span class="sxs-lookup"><span data-stu-id="2ff6e-401">Kestrel source code</span></span>](https://github.com/aspnet/KestrelHttpServer)
* [<span data-ttu-id="2ff6e-402">RFC 7230: Sintaxe e roteamento da mensagem (Seção 5.4: Host)</span><span class="sxs-lookup"><span data-stu-id="2ff6e-402">RFC 7230: Message Syntax and Routing (Section 5.4: Host)</span></span>](https://tools.ietf.org/html/rfc7230#section-5.4)
* [<span data-ttu-id="2ff6e-403">Configurar o ASP.NET Core para trabalhar com servidores proxy e balanceadores de carga</span><span class="sxs-lookup"><span data-stu-id="2ff6e-403">Configure ASP.NET Core to work with proxy servers and load balancers</span></span>](xref:host-and-deploy/proxy-load-balancer)

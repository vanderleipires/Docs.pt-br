---
title: Implementação do servidor Web Kestrel no ASP.NET Core
author: tdykstra
description: Saiba mais sobre o Kestrel, o servidor Web multiplataforma para o ASP.NET Core com base em libuv.
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 04/26/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/kestrel
ms.openlocfilehash: 1709d26f5dfe40d178da70c286d328982f2c39a0
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/03/2018
---
# <a name="kestrel-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="f093f-103">Implementação do servidor Web Kestrel no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f093f-103">Kestrel web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="f093f-104">Por [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher) e [Stephen Halter](https://twitter.com/halter73)</span><span class="sxs-lookup"><span data-stu-id="f093f-104">By [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), and [Stephen Halter](https://twitter.com/halter73)</span></span>

<span data-ttu-id="f093f-105">O Kestrel é um [servidor Web multiplataforma para o ASP.NET Core](xref:fundamentals/servers/index) baseado no [libuv](https://github.com/libuv/libuv), uma biblioteca de E/S assíncrona multiplataforma.</span><span class="sxs-lookup"><span data-stu-id="f093f-105">Kestrel is a cross-platform [web server for ASP.NET Core](xref:fundamentals/servers/index) based on [libuv](https://github.com/libuv/libuv), a cross-platform asynchronous I/O library.</span></span> <span data-ttu-id="f093f-106">O Kestrel é o servidor Web que está incluído por padrão em modelos de projeto do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f093f-106">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span>

<span data-ttu-id="f093f-107">O Kestrel dá suporte aos seguintes recursos:</span><span class="sxs-lookup"><span data-stu-id="f093f-107">Kestrel supports the following features:</span></span>

* <span data-ttu-id="f093f-108">HTTPS</span><span class="sxs-lookup"><span data-stu-id="f093f-108">HTTPS</span></span>
* <span data-ttu-id="f093f-109">Atualização do Opaque usado para habilitar o [WebSockets](https://github.com/aspnet/websockets)</span><span class="sxs-lookup"><span data-stu-id="f093f-109">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
* <span data-ttu-id="f093f-110">Soquetes do UNIX para alto desempenho protegidos pelo Nginx</span><span class="sxs-lookup"><span data-stu-id="f093f-110">Unix sockets for high performance behind Nginx</span></span>

<span data-ttu-id="f093f-111">Há suporte para o Kestrel em todas as plataformas e versões compatíveis com o .NET Core.</span><span class="sxs-lookup"><span data-stu-id="f093f-111">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

<span data-ttu-id="f093f-112">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f093f-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="f093f-113">Quando usar o Kestrel com um proxy reverso</span><span class="sxs-lookup"><span data-stu-id="f093f-113">When to use Kestrel with a reverse proxy</span></span>

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f093f-114">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f093f-114">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="f093f-115">Você pode usar o Kestrel sozinho ou com um *servidor proxy reverso* como IIS, Nginx ou Apache.</span><span class="sxs-lookup"><span data-stu-id="f093f-115">You can use Kestrel by itself or with a *reverse proxy server*, such as IIS, Nginx, or Apache.</span></span> <span data-ttu-id="f093f-116">Um servidor proxy reverso recebe solicitações HTTP da Internet e as encaminha para o Kestrel após algum tratamento preliminar.</span><span class="sxs-lookup"><span data-stu-id="f093f-116">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![O Kestrel se comunica diretamente com a Internet, sem um servidor proxy reverso](kestrel/_static/kestrel-to-internet2.png)

![O Kestrel se comunica indiretamente com a Internet através de um servidor proxy reverso, tal como o IIS, o Nginx ou o Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="f093f-119">É recomendável usar o Kestrel com um servidor proxy reverso, a menos que o Kestrel seja exposto somente a uma rede interna.</span><span class="sxs-lookup"><span data-stu-id="f093f-119">We recommend using Kestrel with a reverse proxy server unless Kestrel is only exposed to an internal network.</span></span>

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f093f-120">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f093f-120">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="f093f-121">Se um aplicativo aceitar solicitações somente de uma rede interna, o Kestrel poderá ser usado diretamente como o servidor do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="f093f-121">If an app accepts requests only from an internal network, Kestrel can be used directly as the app's server.</span></span>

![O Kestrel se comunica diretamente com a rede interna](kestrel/_static/kestrel-to-internal.png)

<span data-ttu-id="f093f-123">Se você expõe o aplicativo à Internet, use o IIS, o Nginx ou o Apache como um *servidor proxy reverso*.</span><span class="sxs-lookup"><span data-stu-id="f093f-123">If you expose your app to the Internet, use IIS, Nginx, or Apache as a *reverse proxy server*.</span></span> <span data-ttu-id="f093f-124">Um servidor proxy reverso recebe solicitações HTTP da Internet e as encaminha para o Kestrel após algum tratamento preliminar.</span><span class="sxs-lookup"><span data-stu-id="f093f-124">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![O Kestrel se comunica indiretamente com a Internet através de um servidor proxy reverso, tal como o IIS, o Nginx ou o Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="f093f-126">Um proxy reverso é necessário para implantações de borda (expostas ao tráfego da Internet) por motivos de segurança.</span><span class="sxs-lookup"><span data-stu-id="f093f-126">A reverse proxy is required for edge deployments (exposed to traffic from the Internet) for security reasons.</span></span> <span data-ttu-id="f093f-127">As versões 1.x do Kestrel não têm um conjunto completo de defesas contra ataques, como tempos limite, limites de tamanho e limites de conexões simultâneas apropriados.</span><span class="sxs-lookup"><span data-stu-id="f093f-127">The 1.x versions of Kestrel don't have a full complement of defenses against attacks, such as appropriate timeouts, size limits, and concurrent connection limits.</span></span>

* * *

<span data-ttu-id="f093f-128">Um cenário de proxy reverso existe quando há vários aplicativos que compartilham o mesmo IP e a mesma porta em execução em um único servidor.</span><span class="sxs-lookup"><span data-stu-id="f093f-128">A reverse proxy scenario exists when there are multiple apps that share the same IP and port running on a single server.</span></span> <span data-ttu-id="f093f-129">O Kestrel não é compatível com esse cenário porque ele não permite compartilhar o mesmo IP e a mesma porta entre vários processos.</span><span class="sxs-lookup"><span data-stu-id="f093f-129">Kestrel doesn't support this scenario because Kestrel doesn't support sharing the same IP and port among multiple processes.</span></span> <span data-ttu-id="f093f-130">Quando o Kestrel é configurado para escutar em uma porta, ele lida com todo o tráfego dessa porta, independentemente do cabeçalho do host das solicitações.</span><span class="sxs-lookup"><span data-stu-id="f093f-130">When Kestrel is configured to listen on a port, Kestrel handles all of the traffic for that port regardless of requests' host header.</span></span> <span data-ttu-id="f093f-131">Um proxy reverso que pode compartilhar portas tem a capacidade de encaminhar solicitações ao Kestrel em um IP e em uma porta exclusivos.</span><span class="sxs-lookup"><span data-stu-id="f093f-131">A reverse proxy that can share ports has the ability to forward requests to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="f093f-132">Mesmo se um servidor proxy reverso não for necessário, usar um servidor proxy reverso poderá ser uma boa opção:</span><span class="sxs-lookup"><span data-stu-id="f093f-132">Even if a reverse proxy server isn't required, using a reverse proxy server might be a good choice:</span></span>

* <span data-ttu-id="f093f-133">Ele pode limitar a área da superfície pública exposta dos aplicativos que hospeda.</span><span class="sxs-lookup"><span data-stu-id="f093f-133">It can limit the exposed public surface area of the apps that it hosts.</span></span>
* <span data-ttu-id="f093f-134">Ele fornece uma camada adicional de defesa e de configuração.</span><span class="sxs-lookup"><span data-stu-id="f093f-134">It provides an additional layer of configuration and defense.</span></span>
* <span data-ttu-id="f093f-135">Pode ser integrado melhor à infraestrutura existente.</span><span class="sxs-lookup"><span data-stu-id="f093f-135">It might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="f093f-136">Ele simplifica a configuração de SSL e o balanceamento de carga.</span><span class="sxs-lookup"><span data-stu-id="f093f-136">It simplifies load balancing and SSL configuration.</span></span> <span data-ttu-id="f093f-137">Somente o servidor proxy reverso exige um certificado SSL e esse servidor pode se comunicar com os servidores de aplicativos na rede interna usando HTTP simples.</span><span class="sxs-lookup"><span data-stu-id="f093f-137">Only the reverse proxy server requires an SSL certificate, and that server can communicate with your app servers on the internal network using plain HTTP.</span></span>

> [!WARNING]
> <span data-ttu-id="f093f-138">Se você não estiver usando um proxy reverso com a filtragem de host habilitada, a [filtragem de host](#host-filtering) deverá ser habilitada.</span><span class="sxs-lookup"><span data-stu-id="f093f-138">If not using a reverse proxy with host filtering enabled, [host filtering](#host-filtering) must be enabled.</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="f093f-139">Como usar o Kestrel em aplicativos ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f093f-139">How to use Kestrel in ASP.NET Core apps</span></span>

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f093f-140">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f093f-140">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="f093f-141">O pacote [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) está incluído no [metapacote Microsoft.AspNetCore.All](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="f093f-141">The [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) package is included in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

<span data-ttu-id="f093f-142">Os modelos de projeto do ASP.NET Core usam o Kestrel por padrão.</span><span class="sxs-lookup"><span data-stu-id="f093f-142">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="f093f-143">Em *Program.cs*, o código do modelo chama [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), que chama [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) em segundo plano.</span><span class="sxs-lookup"><span data-stu-id="f093f-143">In *Program.cs*, the template code calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), which calls [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) behind the scenes.</span></span>

[!code-csharp[](kestrel/samples/2.x/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f093f-144">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f093f-144">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="f093f-145">Instale o pacote NuGet [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/).</span><span class="sxs-lookup"><span data-stu-id="f093f-145">Install the [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) NuGet package.</span></span>

<span data-ttu-id="f093f-146">Chame o método de extensão [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) no [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder?view=aspnetcore-1.1) no método `Main`, especificando as [opções do Kestrel](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions?view=aspnetcore-1.1) necessárias, conforme é mostrado na próxima seção.</span><span class="sxs-lookup"><span data-stu-id="f093f-146">Call the [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) extension method on [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder?view=aspnetcore-1.1) in the `Main` method, specifying any [Kestrel options](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions?view=aspnetcore-1.1) required, as shown in the next section.</span></span>

[!code-csharp[](kestrel/samples/1.x/Program.cs?name=snippet_Main&highlight=13-19)]

* * *

### <a name="kestrel-options"></a><span data-ttu-id="f093f-147">Opções do Kestrel</span><span class="sxs-lookup"><span data-stu-id="f093f-147">Kestrel options</span></span>

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f093f-148">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f093f-148">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="f093f-149">O servidor Web do Kestrel tem opções de configuração de restrição especialmente úteis em implantações para a Internet.</span><span class="sxs-lookup"><span data-stu-id="f093f-149">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span> <span data-ttu-id="f093f-150">Alguns limites importantes que podem ser personalizados:</span><span class="sxs-lookup"><span data-stu-id="f093f-150">A few important limits that can be customized:</span></span>

* <span data-ttu-id="f093f-151">Número máximo de conexões de cliente</span><span class="sxs-lookup"><span data-stu-id="f093f-151">Maximum client connections</span></span>
* <span data-ttu-id="f093f-152">Tamanho máximo do corpo da solicitação</span><span class="sxs-lookup"><span data-stu-id="f093f-152">Maximum request body size</span></span>
* <span data-ttu-id="f093f-153">Taxa de dados mínima do corpo da solicitação</span><span class="sxs-lookup"><span data-stu-id="f093f-153">Minimum request body data rate</span></span>

<span data-ttu-id="f093f-154">Defina essas e outras restrições na propriedade [Limites](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.limits) da classe [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions).</span><span class="sxs-lookup"><span data-stu-id="f093f-154">Set these and other constraints on the [Limits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.limits) property of the [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) class.</span></span> <span data-ttu-id="f093f-155">A propriedade `Limits` contém uma instância da classe [KestrelServerLimits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits).</span><span class="sxs-lookup"><span data-stu-id="f093f-155">The `Limits` property holds an instance of the [KestrelServerLimits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits) class.</span></span>

<span data-ttu-id="f093f-156">**Número máximo de conexões de cliente**</span><span class="sxs-lookup"><span data-stu-id="f093f-156">**Maximum client connections**</span></span>

[<span data-ttu-id="f093f-157">MaxConcurrentConnections</span><span class="sxs-lookup"><span data-stu-id="f093f-157">MaxConcurrentConnections</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxconcurrentconnections)  
[<span data-ttu-id="f093f-158">MaxConcurrentUpgradedConnections</span><span class="sxs-lookup"><span data-stu-id="f093f-158">MaxConcurrentUpgradedConnections</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxconcurrentupgradedconnections)

<span data-ttu-id="f093f-159">O número máximo de conexões TCP abertas simultâneas pode ser definido para o aplicativo inteiro com o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="f093f-159">The maximum number of concurrent open TCP connections can be set for the entire app with the following code:</span></span>

[!code-csharp[](kestrel/samples/2.x/Program.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="f093f-160">Há um limite separado para conexões que foram atualizadas do HTTP ou HTTPS para outro protocolo (por exemplo, em uma solicitação do WebSockets).</span><span class="sxs-lookup"><span data-stu-id="f093f-160">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="f093f-161">Depois que uma conexão é atualizada, ela não é contada em relação ao limite de `MaxConcurrentConnections`.</span><span class="sxs-lookup"><span data-stu-id="f093f-161">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span>

<span data-ttu-id="f093f-162">O número máximo de conexões é ilimitado (nulo) por padrão.</span><span class="sxs-lookup"><span data-stu-id="f093f-162">The maximum number of connections is unlimited (null) by default.</span></span>

<span data-ttu-id="f093f-163">**Tamanho máximo do corpo da solicitação**</span><span class="sxs-lookup"><span data-stu-id="f093f-163">**Maximum request body size**</span></span>

[<span data-ttu-id="f093f-164">MaxRequestBodySize</span><span class="sxs-lookup"><span data-stu-id="f093f-164">MaxRequestBodySize</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize)

<span data-ttu-id="f093f-165">O tamanho máximo do corpo da solicitação padrão é de 30.000.000 bytes, que equivale aproximadamente a 28,6 MB.</span><span class="sxs-lookup"><span data-stu-id="f093f-165">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span>

<span data-ttu-id="f093f-166">A abordagem recomendada para substituir o limite em um aplicativo ASP.NET Core MVC é usar o atributo [RequestSizeLimit](/dotnet/api/microsoft.aspnetcore.mvc.requestsizelimitattribute) em um método de ação:</span><span class="sxs-lookup"><span data-stu-id="f093f-166">The recommended approach to override the limit in an ASP.NET Core MVC app is to use the [RequestSizeLimit](/dotnet/api/microsoft.aspnetcore.mvc.requestsizelimitattribute) attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="f093f-167">Aqui está um exemplo que mostra como configurar a restrição para o aplicativo em cada solicitação:</span><span class="sxs-lookup"><span data-stu-id="f093f-167">Here's an example that shows how to configure the constraint for the app on every request:</span></span>

[!code-csharp[](kestrel/samples/2.x/Program.cs?name=snippet_Limits&highlight=5)]

<span data-ttu-id="f093f-168">Substitua a configuração em uma solicitação específica no middleware:</span><span class="sxs-lookup"><span data-stu-id="f093f-168">You can override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/Startup.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="f093f-169">Uma exceção será gerada se você tentar configurar o limite em uma solicitação depois que o aplicativo começar a ler a solicitação.</span><span class="sxs-lookup"><span data-stu-id="f093f-169">An exception is thrown if you attempt to configure the limit on a request after the app has started to read the request.</span></span> <span data-ttu-id="f093f-170">Há uma propriedade `IsReadOnly` que indica se a propriedade `MaxRequestBodySize` está no estado somente leitura, o que significa que é tarde demais para configurar o limite.</span><span class="sxs-lookup"><span data-stu-id="f093f-170">There's an `IsReadOnly` property that indicates if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="f093f-171">**Taxa de dados mínima do corpo da solicitação**</span><span class="sxs-lookup"><span data-stu-id="f093f-171">**Minimum request body data rate**</span></span>

[<span data-ttu-id="f093f-172">MinRequestBodyDataRate</span><span class="sxs-lookup"><span data-stu-id="f093f-172">MinRequestBodyDataRate</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.minrequestbodydatarate)  
[<span data-ttu-id="f093f-173">MinResponseDataRate</span><span class="sxs-lookup"><span data-stu-id="f093f-173">MinResponseDataRate</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.minresponsedatarate)

<span data-ttu-id="f093f-174">O Kestrel verifica a cada segundo se os dados estão sendo recebidos na taxa especificada em bytes/segundo.</span><span class="sxs-lookup"><span data-stu-id="f093f-174">Kestrel checks every second if data is arriving at the specified rate in bytes/second.</span></span> <span data-ttu-id="f093f-175">Se a taxa cair abaixo do mínimo, a conexão atingirá o tempo limite. O período de cortesia é o tempo que o Kestrel fornece ao cliente para aumentar sua taxa de envio até o mínimo; a taxa não é verificada durante esse período.</span><span class="sxs-lookup"><span data-stu-id="f093f-175">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="f093f-176">O período de cortesia ajuda a evitar a remoção de conexões que inicialmente enviam dados em uma taxa baixa devido ao início lento do TCP.</span><span class="sxs-lookup"><span data-stu-id="f093f-176">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="f093f-177">A taxa mínima de padrão é de 240 bytes/segundo com um período de cortesia de 5 segundos.</span><span class="sxs-lookup"><span data-stu-id="f093f-177">The default minimum rate is 240 bytes/second with a 5 second grace period.</span></span>

<span data-ttu-id="f093f-178">Uma taxa mínima também se aplica à resposta.</span><span class="sxs-lookup"><span data-stu-id="f093f-178">A minimum rate also applies to the response.</span></span> <span data-ttu-id="f093f-179">O código para definir o limite de solicitação e o limite de resposta é o mesmo, exceto por ter `RequestBody` ou `Response` nos nomes da propriedade e da interface.</span><span class="sxs-lookup"><span data-stu-id="f093f-179">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span>

<span data-ttu-id="f093f-180">Este é um exemplo que mostra como configurar as taxas mínima de dados em *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="f093f-180">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

[!code-csharp[](kestrel/samples/2.x/Program.cs?name=snippet_Limits&highlight=6-9)]

<span data-ttu-id="f093f-181">Configure as taxas por solicitação no middleware:</span><span class="sxs-lookup"><span data-stu-id="f093f-181">You can configure the rates per request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/Startup.cs?name=snippet_Limits&highlight=5-8)]

<span data-ttu-id="f093f-182">Para obter informações sobre outras opções e limites do Kestrel, confira:</span><span class="sxs-lookup"><span data-stu-id="f093f-182">For information about other Kestrel options and limits, see:</span></span>

* [<span data-ttu-id="f093f-183">KestrelServerOptions</span><span class="sxs-lookup"><span data-stu-id="f093f-183">KestrelServerOptions</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions)
* [<span data-ttu-id="f093f-184">KestrelServerLimits</span><span class="sxs-lookup"><span data-stu-id="f093f-184">KestrelServerLimits</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits)
* [<span data-ttu-id="f093f-185">ListenOptions</span><span class="sxs-lookup"><span data-stu-id="f093f-185">ListenOptions</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.listenoptions)

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f093f-186">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f093f-186">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="f093f-187">Para obter informações sobre as opções e os limites do Kestrel, confira:</span><span class="sxs-lookup"><span data-stu-id="f093f-187">For information about Kestrel options and limits, see:</span></span>

* [<span data-ttu-id="f093f-188">Classe KestrelServerOptions</span><span class="sxs-lookup"><span data-stu-id="f093f-188">KestrelServerOptions class</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions?view=aspnetcore-1.1)
* [<span data-ttu-id="f093f-189">KestrelServerLimits</span><span class="sxs-lookup"><span data-stu-id="f093f-189">KestrelServerLimits</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserverlimits?view=aspnetcore-1.1)

* * *

### <a name="endpoint-configuration"></a><span data-ttu-id="f093f-190">Configuração do ponto de extremidade</span><span class="sxs-lookup"><span data-stu-id="f093f-190">Endpoint configuration</span></span>

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f093f-191">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f093f-191">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

::: moniker range="= aspnetcore-2.0"
<span data-ttu-id="f093f-192">Por padrão, o ASP.NET Core é associado a `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="f093f-192">By default, ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="f093f-193">Chame os métodos [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) ou [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) em [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) para configurar portas e prefixos de URL para o Kestrel.</span><span class="sxs-lookup"><span data-stu-id="f093f-193">Call [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) or [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) methods on [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) to configure URL prefixes and ports for Kestrel.</span></span> <span data-ttu-id="f093f-194">`UseUrls`, o argumento de linha de comando `--urls`, a chave de configuração de host `urls` e a variável de ambiente `ASPNETCORE_URLS` também funcionam mas têm limitações que serão indicadas mais adiante nesta seção.</span><span class="sxs-lookup"><span data-stu-id="f093f-194">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section.</span></span>

<span data-ttu-id="f093f-195">A chave de configuração de host `urls` precisa vir da configuração do host, não da configuração do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="f093f-195">The `urls` host configuration key must come from the host configuration, not the app configuration.</span></span> <span data-ttu-id="f093f-196">Adicionar uma chave e um valor de `urls` para *appsettings.json* não afeta a configuração do host porque o host está completamente inicializado quando a configuração é lida no arquivo de configuração.</span><span class="sxs-lookup"><span data-stu-id="f093f-196">Adding a `urls` key and value to *appsettings.json* doesn't affect host configuration because the host is completely initialized by the time the configuration is read from the configuration file.</span></span> <span data-ttu-id="f093f-197">No entanto, uma chave `urls` em *appsettings.json* pode ser usada com [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) no construtor de host para configurar o host:</span><span class="sxs-lookup"><span data-stu-id="f093f-197">However, a `urls` key in *appsettings.json* can be used with [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) on the host builder to configure the host:</span></span>

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
[!INCLUDE[](~/includes/2.1.md)]

<span data-ttu-id="f093f-199">Por padrão, o ASP.NET Core associa a:</span><span class="sxs-lookup"><span data-stu-id="f093f-199">By default, ASP.NET Core binds to:</span></span>

* `http://localhost:5000`
* <span data-ttu-id="f093f-200">`https://localhost:5001` (quando um certificado de desenvolvimento local está presente)</span><span class="sxs-lookup"><span data-stu-id="f093f-200">`https://localhost:5001` (when a local development certificate is present)</span></span>

<span data-ttu-id="f093f-201">Um certificado de desenvolvimento é criado:</span><span class="sxs-lookup"><span data-stu-id="f093f-201">A development certificate is created:</span></span>

* <span data-ttu-id="f093f-202">Quando o [SDK do .NET Core](/dotnet/core/sdk) está instalado.</span><span class="sxs-lookup"><span data-stu-id="f093f-202">When the [.NET Core SDK](/dotnet/core/sdk) is installed.</span></span>
* <span data-ttu-id="f093f-203">A [ferramenta dev-certs](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-dev-certs) é usada para criar um certificado.</span><span class="sxs-lookup"><span data-stu-id="f093f-203">The [dev-certs tool](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-dev-certs) is used to create a certificate.</span></span>

<span data-ttu-id="f093f-204">Alguns navegadores exigem que você conceda permissão explícita para o navegador para confiar no certificado de desenvolvimento local.</span><span class="sxs-lookup"><span data-stu-id="f093f-204">Some browsers require that you grant explicit permission to the browser to trust the local development certificate.</span></span>

<span data-ttu-id="f093f-205">Os modelos de projeto do ASP.NET Core 2.1 e posteriores configuram os aplicativos para serem executados em HTTPS, por padrão, e incluem o [Redirecionamento de HTTPS e o suporte a HSTS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="f093f-205">ASP.NET Core 2.1 and later project templates configure apps to run on HTTPS by default and include [HTTPS redirection and HSTS support](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="f093f-206">Chame os métodos [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) ou [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) em [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) para configurar portas e prefixos de URL para o Kestrel.</span><span class="sxs-lookup"><span data-stu-id="f093f-206">Call [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) or [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) methods on [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) to configure URL prefixes and ports for Kestrel.</span></span>

<span data-ttu-id="f093f-207">`UseUrls`, o argumento de linha de comando `--urls`, a chave de configuração de host `urls` e a variável de ambiente `ASPNETCORE_URLS` também funcionam mas têm limitações que serão indicadas mais adiante nesta seção (um certificado padrão precisa estar disponível para a configuração do ponto de extremidade HTTPS).</span><span class="sxs-lookup"><span data-stu-id="f093f-207">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section (a default certificate must be available for HTTPS endpoint configuration).</span></span>

<span data-ttu-id="f093f-208">Configuração do ASP.NET Core 2.1 `KestrelServerOptions`:</span><span class="sxs-lookup"><span data-stu-id="f093f-208">ASP.NET Core 2.1 `KestrelServerOptions` configuration:</span></span>

<span data-ttu-id="f093f-209">**ConfigureEndpointDefaults(Action<ListenOptions>)**</span><span class="sxs-lookup"><span data-stu-id="f093f-209">**ConfigureEndpointDefaults(Action<ListenOptions>)**</span></span>  
<span data-ttu-id="f093f-210">Especifica uma `Action` de configuração a ser executada para cada ponto de extremidade especificado.</span><span class="sxs-lookup"><span data-stu-id="f093f-210">Specifies a configuration `Action` to run for each specified endpoint.</span></span> <span data-ttu-id="f093f-211">Chamar `ConfigureEndpointDefaults` várias vezes substitui as `Action`s pela última `Action` especificada.</span><span class="sxs-lookup"><span data-stu-id="f093f-211">Calling `ConfigureEndpointDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

<span data-ttu-id="f093f-212">**ConfigureHttpsDefaults(Action<HttpsConnectionAdapterOptions>)**</span><span class="sxs-lookup"><span data-stu-id="f093f-212">**ConfigureHttpsDefaults(Action<HttpsConnectionAdapterOptions>)**</span></span>  
<span data-ttu-id="f093f-213">Especifica uma `Action` de configuração a ser executada para cada ponto de extremidade HTTPS.</span><span class="sxs-lookup"><span data-stu-id="f093f-213">Specifies a configuration `Action` to run for each HTTPS endpoint.</span></span> <span data-ttu-id="f093f-214">Chamar `ConfigureHttpsDefaults` várias vezes substitui as `Action`s pela última `Action` especificada.</span><span class="sxs-lookup"><span data-stu-id="f093f-214">Calling `ConfigureHttpsDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

<span data-ttu-id="f093f-215">**Configure(IConfiguration)**</span><span class="sxs-lookup"><span data-stu-id="f093f-215">**Configure(IConfiguration)**</span></span>  
<span data-ttu-id="f093f-216">Cria um carregador de configuração para configurar o Kestrel que usa uma [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) como entrada.</span><span class="sxs-lookup"><span data-stu-id="f093f-216">Creates a configuration loader for setting up Kestrel that takes an [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) as input.</span></span> <span data-ttu-id="f093f-217">A configuração precisa estar no escopo da seção de configuração do Kestrel.</span><span class="sxs-lookup"><span data-stu-id="f093f-217">The configuration must be scoped to the configuration section for Kestrel.</span></span>

<span data-ttu-id="f093f-218">**ListenOptions.UseHttps**</span><span class="sxs-lookup"><span data-stu-id="f093f-218">**ListenOptions.UseHttps**</span></span>  
<span data-ttu-id="f093f-219">Configure o Kestrel para usar HTTPS.</span><span class="sxs-lookup"><span data-stu-id="f093f-219">Configure Kestrel to use HTTPS.</span></span>

<span data-ttu-id="f093f-220">Extensões `ListenOptions.UseHttps`:</span><span class="sxs-lookup"><span data-stu-id="f093f-220">`ListenOptions.UseHttps` extensions:</span></span>

* <span data-ttu-id="f093f-221">`UseHttps` &ndash; Configure o Kestrel para usar HTTPS com o certificado padrão.</span><span class="sxs-lookup"><span data-stu-id="f093f-221">`UseHttps` &ndash; Configure Kestrel to use HTTPS with the default certificate.</span></span> <span data-ttu-id="f093f-222">Gera uma exceção quando não há nenhum certificado padrão configurado.</span><span class="sxs-lookup"><span data-stu-id="f093f-222">Throws an exception if no default certificate is configured.</span></span>
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

<span data-ttu-id="f093f-223">Parâmetros de `ListenOptions.UseHttps`:</span><span class="sxs-lookup"><span data-stu-id="f093f-223">`ListenOptions.UseHttps` parameters:</span></span>

* <span data-ttu-id="f093f-224">`filename` é o caminho e o nome do arquivo de um arquivo de certificado, relativo ao diretório que contém os arquivos de conteúdo do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="f093f-224">`filename` is the path and file name of a certificate file, relative to the directory that contains the app's content files.</span></span>
* <span data-ttu-id="f093f-225">`password` é a senha necessária para acessar os dados do certificado X.509 .</span><span class="sxs-lookup"><span data-stu-id="f093f-225">`password` is the password required to access the X.509 certificate data.</span></span>
* <span data-ttu-id="f093f-226">`configureOptions` é uma `Action` para configurar as `HttpsConnectionAdapterOptions`.</span><span class="sxs-lookup"><span data-stu-id="f093f-226">`configureOptions` is an `Action` to configure the `HttpsConnectionAdapterOptions`.</span></span> <span data-ttu-id="f093f-227">Retorna o `ListenOptions`.</span><span class="sxs-lookup"><span data-stu-id="f093f-227">Returns the `ListenOptions`.</span></span>
* <span data-ttu-id="f093f-228">`storeName` é o repositório de certificados do qual o certificado deve ser carregado.</span><span class="sxs-lookup"><span data-stu-id="f093f-228">`storeName` is the certificate store from which to load the certificate.</span></span>
* <span data-ttu-id="f093f-229">`subject` é o nome da entidade do certificado.</span><span class="sxs-lookup"><span data-stu-id="f093f-229">`subject` is the subject name for the certificate.</span></span>
* <span data-ttu-id="f093f-230">`allowInvalid` indica se certificados inválidos devem ser considerados, como os certificados autoassinados.</span><span class="sxs-lookup"><span data-stu-id="f093f-230">`allowInvalid` indicates if invalid certificates should be considered, such as self-signed certificates.</span></span>
* <span data-ttu-id="f093f-231">`location` é o local do repositório do qual o certificado deve ser carregado.</span><span class="sxs-lookup"><span data-stu-id="f093f-231">`location` is the store location to load the certificate from.</span></span>
* <span data-ttu-id="f093f-232">`serverCertificate` é o certificado X.509.</span><span class="sxs-lookup"><span data-stu-id="f093f-232">`serverCertificate` is the X.509 certificate.</span></span>

<span data-ttu-id="f093f-233">Em produção, HTTPS precisa ser configurado explicitamente.</span><span class="sxs-lookup"><span data-stu-id="f093f-233">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="f093f-234">No mínimo, um certificado padrão precisa ser fornecido.</span><span class="sxs-lookup"><span data-stu-id="f093f-234">At a minimum, a default certificate must be provided.</span></span>

<span data-ttu-id="f093f-235">Configurações com suporte descritas a seguir:</span><span class="sxs-lookup"><span data-stu-id="f093f-235">Supported configurations described next:</span></span>

* <span data-ttu-id="f093f-236">Nenhuma configuração</span><span class="sxs-lookup"><span data-stu-id="f093f-236">No configuration</span></span>
* <span data-ttu-id="f093f-237">Substituir o certificado padrão da configuração</span><span class="sxs-lookup"><span data-stu-id="f093f-237">Replace the default certificate from configuration</span></span>
* <span data-ttu-id="f093f-238">Alterar os padrões no código</span><span class="sxs-lookup"><span data-stu-id="f093f-238">Change the defaults in code</span></span>

<span data-ttu-id="f093f-239">*Nenhuma configuração*</span><span class="sxs-lookup"><span data-stu-id="f093f-239">*No configuration*</span></span>

<span data-ttu-id="f093f-240">O Kestrel escuta em `http://localhost:5000` e em `https://localhost:5001` (se houver um certificado padrão disponível).</span><span class="sxs-lookup"><span data-stu-id="f093f-240">Kestrel listens on `http://localhost:5000` and `https://localhost:5001` (if a default cert is available).</span></span>

<span data-ttu-id="f093f-241">Especificar URLs usando:</span><span class="sxs-lookup"><span data-stu-id="f093f-241">Specify URLs using the:</span></span>

* <span data-ttu-id="f093f-242">A variável de ambiente `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="f093f-242">`ASPNETCORE_URLS` environment variable.</span></span>
* <span data-ttu-id="f093f-243">O argumento de linha de comando `--urls`.</span><span class="sxs-lookup"><span data-stu-id="f093f-243">`--urls` command-line argument.</span></span>
* <span data-ttu-id="f093f-244">A chave de configuração do host `urls`.</span><span class="sxs-lookup"><span data-stu-id="f093f-244">`urls` host configuration key.</span></span>
* <span data-ttu-id="f093f-245">O método de extensão `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="f093f-245">`UseUrls` extension method.</span></span>

<span data-ttu-id="f093f-246">Para obter mais informações, confira [URLs de servidor](xref:fundamentals/hosting#server-urls) e [Substituindo a configuração](xref:fundamentals/hosting#overriding-configuration).</span><span class="sxs-lookup"><span data-stu-id="f093f-246">For more information, see [Server URLs](xref:fundamentals/hosting#server-urls) and [Overriding configuration](xref:fundamentals/hosting#overriding-configuration).</span></span>

<span data-ttu-id="f093f-247">O valor fornecido usando essas abordagens pode ser um ou mais pontos de extremidade HTTP e HTTPS (HTTPS se houver um certificado padrão).</span><span class="sxs-lookup"><span data-stu-id="f093f-247">The value provided using these approaches can be one or more HTTP and HTTPS endpoints (HTTPS if a default cert is available).</span></span> <span data-ttu-id="f093f-248">Configure o valor como uma lista separada por ponto e vírgula (por exemplo, `"Urls": "http://localhost:8000;http://localhost:8001"`).</span><span class="sxs-lookup"><span data-stu-id="f093f-248">Configure the value as a semicolon-separated list (for example, `"Urls": "http://localhost:8000;http://localhost:8001"`).</span></span>

<span data-ttu-id="f093f-249">*Substituir o certificado padrão da configuração*</span><span class="sxs-lookup"><span data-stu-id="f093f-249">*Replace the default certificate from configuration*</span></span>

<span data-ttu-id="f093f-250">[WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) chama `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` por padrão ao carregar a configuração do Kestrel.</span><span class="sxs-lookup"><span data-stu-id="f093f-250">[WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) calls `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span> <span data-ttu-id="f093f-251">Há um esquema de definições de configurações de aplicativo HTTPS padrão disponível para o Kestrel.</span><span class="sxs-lookup"><span data-stu-id="f093f-251">A default HTTPS app settings configuration schema is available for Kestrel.</span></span> <span data-ttu-id="f093f-252">Configure vários pontos de extremidade, incluindo URLs e os certificados a serem usados, por meio de um arquivo no disco ou de um repositório de certificados.</span><span class="sxs-lookup"><span data-stu-id="f093f-252">Configure multiple endpoints, including the URLs and the certificates to use, either from a file on disk or from a certificate store.</span></span>

<span data-ttu-id="f093f-253">No exemplo de *appsettings.json* a seguir:</span><span class="sxs-lookup"><span data-stu-id="f093f-253">In the following *appsettings.json* example:</span></span>

* <span data-ttu-id="f093f-254">Defina **AllowInvalid** como `true` para permitir o uso de certificados inválidos (por exemplo, os certificados autoassinados).</span><span class="sxs-lookup"><span data-stu-id="f093f-254">Set **AllowInvalid** to `true` to permit the use of invalid certificates (for example, self-signed certificates).</span></span>
* <span data-ttu-id="f093f-255">Todo ponto de extremidade HTTPS que não especificar um certificado (**HttpsDefaultCert** no exemplo a seguir) será revertido para o certificado definido em **Certificados** > **Padrão** ou para o certificado de desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="f093f-255">Any HTTPS endpoint that doesn't specify a certificate (**HttpsDefaultCert** in the example that follows) falls back to the cert defined under **Certificates** > **Default** or the development certificate.</span></span>

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

<span data-ttu-id="f093f-256">Uma alternativa ao uso de **Caminho** e **Senha** para qualquer nó de certificado é especificar o certificado usando campos de repositório de certificados.</span><span class="sxs-lookup"><span data-stu-id="f093f-256">An alternative to using **Path** and **Password** for any certificate node is to specify the certificate using certificate store fields.</span></span> <span data-ttu-id="f093f-257">Por exemplo, o certificado **Certificados** > **Padrão** pode ser especificado como:</span><span class="sxs-lookup"><span data-stu-id="f093f-257">For example, the **Certificates** > **Default** certificate can be specified as:</span></span>

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; defaults to My>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

<span data-ttu-id="f093f-258">Observações do esquema:</span><span class="sxs-lookup"><span data-stu-id="f093f-258">Schema notes:</span></span>

* <span data-ttu-id="f093f-259">Os nomes de pontos de extremidade diferenciam maiúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="f093f-259">Endpoints names are case-insensitive.</span></span> <span data-ttu-id="f093f-260">Por exemplo, `HTTPS` e `Https` são válidos.</span><span class="sxs-lookup"><span data-stu-id="f093f-260">For example, `HTTPS` and `Https` are valid.</span></span>
* <span data-ttu-id="f093f-261">O parâmetro `Url` é necessário para cada ponto de extremidade.</span><span class="sxs-lookup"><span data-stu-id="f093f-261">The `Url` parameter is required for each endpoint.</span></span> <span data-ttu-id="f093f-262">O formato desse parâmetro é o mesmo que o do parâmetro de configuração de `Urls` de nível superior, exceto que ele é limitado a um único valor.</span><span class="sxs-lookup"><span data-stu-id="f093f-262">The format for this parameter is the same as the top-level `Urls` configuration parameter except that it's limited to a single value.</span></span>
* <span data-ttu-id="f093f-263">Esses pontos de extremidade substituem aqueles definidos na configuração de `Urls` de nível superior em vez de serem adicionados a eles.</span><span class="sxs-lookup"><span data-stu-id="f093f-263">These endpoints replace those defined in the top-level `Urls` configuration rather than adding to them.</span></span> <span data-ttu-id="f093f-264">Os pontos de extremidade definidos no código por meio de `Listen` são acumulados com os pontos de extremidade definidos na seção de configuração.</span><span class="sxs-lookup"><span data-stu-id="f093f-264">Endpoints defined in code via `Listen` are cumulative with the endpoints defined in the configuration section.</span></span>
* <span data-ttu-id="f093f-265">A seção `Certificate` é opcional.</span><span class="sxs-lookup"><span data-stu-id="f093f-265">The `Certificate` section is optional.</span></span> <span data-ttu-id="f093f-266">Se a seção `Certificate` não for especificada, os padrões definidos nos cenários anteriores serão usados.</span><span class="sxs-lookup"><span data-stu-id="f093f-266">If the `Certificate` section isn't specified, the defaults defined in earlier scenarios are used.</span></span> <span data-ttu-id="f093f-267">Se não houver nenhum padrão disponível, o servidor gerará uma exceção e não poderá ser iniciado.</span><span class="sxs-lookup"><span data-stu-id="f093f-267">If no defaults are available, the server throws an exception and fails to start.</span></span>
* <span data-ttu-id="f093f-268">A seção `Certificate` é compatível com os certificados de **Caminho**&ndash;**Senha** e **Entidade**&ndash;**Repositório**.</span><span class="sxs-lookup"><span data-stu-id="f093f-268">The `Certificate` section supports both **Path**&ndash;**Password** and **Subject**&ndash;**Store** certificates.</span></span>
* <span data-ttu-id="f093f-269">Qualquer número de pontos de extremidade pode ser definido dessa forma, contanto que eles não causem conflitos de porta.</span><span class="sxs-lookup"><span data-stu-id="f093f-269">Any number of endpoints may be defined in this way so long as they don't cause port conflicts.</span></span>
* <span data-ttu-id="f093f-270">`serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` retorna um `KestrelConfigurationLoader` com um método `.Endpoint(string name, options => { })` que pode ser usado para complementar as definições de um ponto de extremidade configurado:</span><span class="sxs-lookup"><span data-stu-id="f093f-270">`serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` returns a `KestrelConfigurationLoader` with an `.Endpoint(string name, options => { })` method that can be used to supplement a configured endpoint's settings:</span></span>

  ```csharp
  serverOptions.Configure(context.Configuration.GetSection("Kestrel"))
      .Endpoint("HTTPS", opt =>
      {
          opt.HttpsOptions.SslProtocols = SslProtocols.Tls12;
      });
  ```

  <span data-ttu-id="f093f-271">Você também pode acessar o `KestrelServerOptions.ConfigurationLoader` diretamente para continuar a iteração no carregador existente, como aquela fornecida pelo `WebHost.CreatedDeafaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="f093f-271">You can also directly access `KestrelServerOptions.ConfigurationLoader` to keep iterating on the existing loader, such as the one provided by `WebHost.CreatedDeafaultBuilder`.</span></span>

* <span data-ttu-id="f093f-272">A seção de configuração de cada ponto de extremidade está disponível nas opções no método `Endpoint` para que as configurações personalizadas possam ser lidas.</span><span class="sxs-lookup"><span data-stu-id="f093f-272">The configuration section for each endpoint is a available on the options in the `Endpoint` method so that custom settings may be read.</span></span>
* <span data-ttu-id="f093f-273">Várias configurações podem ser carregadas chamando `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` novamente com outra seção.</span><span class="sxs-lookup"><span data-stu-id="f093f-273">Multiple configurations may be loaded by calling `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` again with another section.</span></span> <span data-ttu-id="f093f-274">Somente a última configuração será usada, a menos que `Load` seja chamado explicitamente nas instâncias anteriores.</span><span class="sxs-lookup"><span data-stu-id="f093f-274">Only the last configuration is used, unless `Load` is explicitly called on prior instances.</span></span> <span data-ttu-id="f093f-275">O metapacote não chama `Load`, portanto, sua seção de configuração padrão pode ser substituída.</span><span class="sxs-lookup"><span data-stu-id="f093f-275">The metapackage doesn't call `Load` so that its default configuration section may be replaced.</span></span>
* <span data-ttu-id="f093f-276">O `KestrelConfigurationLoader` espelha a família de APIs `Listen` de `KestrelServerOptions` como sobrecargas de `Endpoint`, portanto, os pontos de extremidade de código e de configuração podem ser configurados no mesmo local.</span><span class="sxs-lookup"><span data-stu-id="f093f-276">`KestrelConfigurationLoader` mirrors the `Listen` family of APIs from `KestrelServerOptions` as `Endpoint` overloads, so code and config endpoints may be configured in the same place.</span></span> <span data-ttu-id="f093f-277">Essas sobrecargas não usam nomes e consomem somente as definições padrão da configuração.</span><span class="sxs-lookup"><span data-stu-id="f093f-277">These overloads don't use names and only consume default settings from configuration.</span></span>

<span data-ttu-id="f093f-278">*Alterar os padrões no código*</span><span class="sxs-lookup"><span data-stu-id="f093f-278">*Change the defaults in code*</span></span>

<span data-ttu-id="f093f-279">`ConfigureEndpointDefaults` e `ConfigureHttpsDefaults` podem ser usados para alterar as configurações padrão de `ListenOptions` e `HttpsConnectionAdapterOptions`, incluindo a substituição do certificado padrão especificado no cenário anterior.</span><span class="sxs-lookup"><span data-stu-id="f093f-279">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` can be used to change default settings for `ListenOptions` and `HttpsConnectionAdapterOptions`, including overriding the default certificate specified in the prior scenario.</span></span> <span data-ttu-id="f093f-280">`ConfigureEndpointDefaults` e `ConfigureHttpsDefaults` devem ser chamados antes que qualquer ponto de extremidade seja configurado.</span><span class="sxs-lookup"><span data-stu-id="f093f-280">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` should be called before any endpoints are configured.</span></span>

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

<span data-ttu-id="f093f-281">*Suporte do Kestrel para SNI*</span><span class="sxs-lookup"><span data-stu-id="f093f-281">*Kestrel support for SNI*</span></span>

<span data-ttu-id="f093f-282">A [SNI (Indicação de Nome de Servidor)](https://tools.ietf.org/html/rfc6066#section-3) pode ser usada para hospedar vários domínios no mesmo endereço IP e na mesma porta.</span><span class="sxs-lookup"><span data-stu-id="f093f-282">[Server Name Indication (SNI)](https://tools.ietf.org/html/rfc6066#section-3) can be used to host multiple domains on the same IP address and port.</span></span> <span data-ttu-id="f093f-283">Para que a SNI funcione, o cliente envia o nome do host da sessão segura para o servidor durante o handshake TLS para que o servidor possa fornecer o certificado correto.</span><span class="sxs-lookup"><span data-stu-id="f093f-283">For SNI to function, the client sends the host name for the secure session to the server during the TLS handshake so that the server can provide the correct certificate.</span></span> <span data-ttu-id="f093f-284">O cliente usa o certificado fornecido para a comunicação criptografada com o servidor durante a sessão segura que segue o handshake TLS.</span><span class="sxs-lookup"><span data-stu-id="f093f-284">The client uses the furnished certificate for encrypted communication with the server during the secure session that follows the TLS handshake.</span></span>

<span data-ttu-id="f093f-285">O Kestrel permite a SNI por meio do retorno de chamada do `ServerCertificateSelector`.</span><span class="sxs-lookup"><span data-stu-id="f093f-285">Kestrel supports SNI via the `ServerCertificateSelector` callback.</span></span> <span data-ttu-id="f093f-286">O retorno de chamada é invocado uma vez por conexão para permitir que o aplicativo inspecione o nome do host e selecione o certificado apropriado.</span><span class="sxs-lookup"><span data-stu-id="f093f-286">The callback is invoked once per connection to allow the app to inspect the host name and select the appropriate certificate.</span></span>

<span data-ttu-id="f093f-287">O suporte à SNI requer a execução no `netcoreapp2.1` da estrutura de destino.</span><span class="sxs-lookup"><span data-stu-id="f093f-287">SNI support requires running on target framework `netcoreapp2.1`.</span></span> <span data-ttu-id="f093f-288">No `netcoreapp2.0` e no `net461`, o retorno de chamada é invocado, mas o `name` é sempre `null`.</span><span class="sxs-lookup"><span data-stu-id="f093f-288">On `netcoreapp2.0` and `net461`, the callback is invoked but the `name` is always `null`.</span></span> <span data-ttu-id="f093f-289">O `name` também será `null` se o cliente não fornecer o parâmetro de nome do host no handshake TLS.</span><span class="sxs-lookup"><span data-stu-id="f093f-289">The `name` is also `null` if the client doesn't provide the host name parameter in the TLS handshake.</span></span>

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

<span data-ttu-id="f093f-290">**Associar a um soquete TCP**</span><span class="sxs-lookup"><span data-stu-id="f093f-290">**Bind to a TCP socket**</span></span>

<span data-ttu-id="f093f-291">O método [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) é associado a um soquete TCP e um lambda de opções permite a configuração do certificado SSL:</span><span class="sxs-lookup"><span data-stu-id="f093f-291">The [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) method binds to a TCP socket, and an options lambda permits SSL certificate configuration:</span></span>

[!code-csharp[](kestrel/samples/2.x/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

<span data-ttu-id="f093f-292">Observe como esse exemplo configura o SSL de um ponto de extremidade específico usando [ListenOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.listenoptions).</span><span class="sxs-lookup"><span data-stu-id="f093f-292">Notice how this example configures SSL for a particular endpoint by using [ListenOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.listenoptions).</span></span> <span data-ttu-id="f093f-293">Use a mesma API para definir outras configurações do Kestrel para pontos de extremidade específicos.</span><span class="sxs-lookup"><span data-stu-id="f093f-293">Use the same API to configure other Kestrel settings for specific endpoints.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

<span data-ttu-id="f093f-294">**Associar a um soquete do UNIX**</span><span class="sxs-lookup"><span data-stu-id="f093f-294">**Bind to a Unix socket**</span></span>

<span data-ttu-id="f093f-295">Escute em um soquete Unix com [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) para melhorar o desempenho com o Nginx, como é mostrado neste exemplo:</span><span class="sxs-lookup"><span data-stu-id="f093f-295">Listen on a Unix socket with [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) for improved performance with Nginx, as shown in this example:</span></span>

[!code-csharp[](kestrel/samples/2.x/Program.cs?name=snippet_UnixSocket)]

<span data-ttu-id="f093f-296">**Porta 0**</span><span class="sxs-lookup"><span data-stu-id="f093f-296">**Port 0**</span></span>

<span data-ttu-id="f093f-297">Quando o número da porta `0` for especificado, o Kestrel se associará dinamicamente a uma porta disponível.</span><span class="sxs-lookup"><span data-stu-id="f093f-297">When the port number `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="f093f-298">O exemplo a seguir mostra como determinar a qual porta o Kestrel realmente se associou no tempo de execução:</span><span class="sxs-lookup"><span data-stu-id="f093f-298">The following example shows how to determine which port Kestrel actually bound at runtime:</span></span>

[!code-csharp[](kestrel/samples/2.x/Startup.cs?name=snippet_Port0&highlight=3)]

<span data-ttu-id="f093f-299">Quando o aplicativo é executado, a saída da janela do console indica a porta dinâmica na qual o aplicativo pode ser acessado:</span><span class="sxs-lookup"><span data-stu-id="f093f-299">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Now listening on: http://127.0.0.1:48508
```

<span data-ttu-id="f093f-300">Limitações do **UseUrls, do argumento de linha de comando --urls, da chave de configuração de host urls e da variável de ambiente ASPNETCORE_URLS**</span><span class="sxs-lookup"><span data-stu-id="f093f-300">**UseUrls, --urls command-line argument, urls host configuration key, and ASPNETCORE_URLS environment variable limitations**</span></span>

<span data-ttu-id="f093f-301">Configure pontos de extremidade com as seguintes abordagens:</span><span class="sxs-lookup"><span data-stu-id="f093f-301">Configure endpoints with the following approaches:</span></span>

* [<span data-ttu-id="f093f-302">UseUrls</span><span class="sxs-lookup"><span data-stu-id="f093f-302">UseUrls</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls)
* <span data-ttu-id="f093f-303">O argumento de linha de comando `--urls`</span><span class="sxs-lookup"><span data-stu-id="f093f-303">`--urls` command-line argument</span></span>
* <span data-ttu-id="f093f-304">A chave de configuração do host `urls`</span><span class="sxs-lookup"><span data-stu-id="f093f-304">`urls` host configuration key</span></span>
* <span data-ttu-id="f093f-305">A variável de ambiente `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="f093f-305">`ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="f093f-306">Esses métodos são úteis para fazer com que o código funcione com servidores que não sejam o Kestrel.</span><span class="sxs-lookup"><span data-stu-id="f093f-306">These methods are useful for making code work with servers other than Kestrel.</span></span> <span data-ttu-id="f093f-307">No entanto, esteja ciente dessas limitações:</span><span class="sxs-lookup"><span data-stu-id="f093f-307">However, be aware of these limitations:</span></span>

* <span data-ttu-id="f093f-308">O protocolo SSL não pode ser usado com essas abordagens, a menos que um certificado padrão seja fornecido na configuração do ponto de extremidade HTTPS (por exemplo, usando a configuração `KestrelServerOptions` ou um arquivo de configuração, como já foi mostrado neste tópico).</span><span class="sxs-lookup"><span data-stu-id="f093f-308">SSL can't be used with these approaches unless a default certificate is provided in the HTTPS endpoint configuration (for example, using `KestrelServerOptions` configuration or a configuration file as shown earlier in this topic).</span></span>
* <span data-ttu-id="f093f-309">Quando ambas as abordagens, `Listen` e `UseUrls`, são usadas ao mesmo tempo, os pontos de extremidade de `Listen` substituem os pontos de extremidade de `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="f093f-309">When both the `Listen` and `UseUrls` approaches are used simultaneously, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

<span data-ttu-id="f093f-310">**Configuração de ponto de extremidade do IIS**</span><span class="sxs-lookup"><span data-stu-id="f093f-310">**IIS endpoint configuration**</span></span>

<span data-ttu-id="f093f-311">Ao usar o IIS, as associações de URL para IIS substituem as associações definidas por `Listen` ou `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="f093f-311">When using IIS, the URL bindings for IIS override bindings are set by either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="f093f-312">Para obter mais informações, confira o tópico [Módulo do ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="f093f-312">For more information, see the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) topic.</span></span>

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f093f-313">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f093f-313">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="f093f-314">Por padrão, o ASP.NET Core é associado a `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="f093f-314">By default, ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="f093f-315">Configure prefixos de URL e portas para o Kestrel usando:</span><span class="sxs-lookup"><span data-stu-id="f093f-315">Configure URL prefixes and ports for Kestrel using:</span></span>

* <span data-ttu-id="f093f-316">O método de extensão [UseUrls](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls?view=aspnetcore-1.1)</span><span class="sxs-lookup"><span data-stu-id="f093f-316">[UseUrls](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls?view=aspnetcore-1.1) extension method</span></span>
* <span data-ttu-id="f093f-317">O argumento de linha de comando `--urls`</span><span class="sxs-lookup"><span data-stu-id="f093f-317">`--urls` command-line argument</span></span>
* <span data-ttu-id="f093f-318">A chave de configuração do host `urls`</span><span class="sxs-lookup"><span data-stu-id="f093f-318">`urls` host configuration key</span></span>
* <span data-ttu-id="f093f-319">O sistema de configuração do ASP.NET Core, incluindo a variável de ambiente `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="f093f-319">ASP.NET Core configuration system, including `ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="f093f-320">Para obter mais informações sobre esses métodos, confira [Hospedagem](xref:fundamentals/hosting).</span><span class="sxs-lookup"><span data-stu-id="f093f-320">For more information on these methods, see [Hosting](xref:fundamentals/hosting).</span></span>

<span data-ttu-id="f093f-321">**Configuração de ponto de extremidade do IIS**</span><span class="sxs-lookup"><span data-stu-id="f093f-321">**IIS endpoint configuration**</span></span>

<span data-ttu-id="f093f-322">Ao usar o IIS, as associações de URL para o IIS substituam as associações definidas por `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="f093f-322">When using IIS, the URL bindings for IIS override bindings set by `UseUrls`.</span></span> <span data-ttu-id="f093f-323">Para obter mais informações, confira o tópico [Módulo do ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="f093f-323">For more information, see the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) topic.</span></span>

* * *

### <a name="url-prefixes"></a><span data-ttu-id="f093f-324">Prefixos de URL</span><span class="sxs-lookup"><span data-stu-id="f093f-324">URL prefixes</span></span>

<span data-ttu-id="f093f-325">Ao usar `UseUrls`, o argumento de linha de comando `--urls`, a chave de configuração de host `urls` ou a variável de ambiente `ASPNETCORE_URLS`, os prefixos de URL podem estar em um dos formatos a seguir.</span><span class="sxs-lookup"><span data-stu-id="f093f-325">When using `UseUrls`, `--urls` command-line argument, `urls` host configuration key, or `ASPNETCORE_URLS` environment variable, the URL prefixes can be in any of the following formats.</span></span>

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f093f-326">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f093f-326">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="f093f-327">Somente os prefixos de URL HTTP são válidos.</span><span class="sxs-lookup"><span data-stu-id="f093f-327">Only HTTP URL prefixes are valid.</span></span> <span data-ttu-id="f093f-328">O Kestrel não é compatível com SSL ao configurar associações de URL usando `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="f093f-328">Kestrel doesn't support SSL when configuring URL bindings using `UseUrls`.</span></span>

* <span data-ttu-id="f093f-329">Endereço IPv4 com o número da porta</span><span class="sxs-lookup"><span data-stu-id="f093f-329">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="f093f-330">`0.0.0.0` é um caso especial que associa a todos os endereços IPv4.</span><span class="sxs-lookup"><span data-stu-id="f093f-330">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>


* <span data-ttu-id="f093f-331">Endereço IPv6 com número da porta</span><span class="sxs-lookup"><span data-stu-id="f093f-331">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  <span data-ttu-id="f093f-332">`[::]` é o equivalente do IPv6 ao IPv4 `0.0.0.0`.</span><span class="sxs-lookup"><span data-stu-id="f093f-332">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>


* <span data-ttu-id="f093f-333">Nome do host com o número da porta</span><span class="sxs-lookup"><span data-stu-id="f093f-333">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="f093f-334">Os nomes de host `*` e `+` não são especiais.</span><span class="sxs-lookup"><span data-stu-id="f093f-334">Host names, `*`, and `+`, aren't special.</span></span> <span data-ttu-id="f093f-335">Tudo o que não é reconhecido como um endereço IP ou um `localhost` válido é associado a todos os IPs IPv6 e IPv4.</span><span class="sxs-lookup"><span data-stu-id="f093f-335">Anything not recognized as a valid IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="f093f-336">Para associar nomes de host diferentes a diferentes aplicativos ASP.NET Core na mesma porta, use o [HTTP.sys](xref:fundamentals/servers/httpsys) ou um servidor proxy reverso, como o IIS, o Nginx ou o Apache.</span><span class="sxs-lookup"><span data-stu-id="f093f-336">To bind different host names to different ASP.NET Core apps on the same port, use [HTTP.sys](xref:fundamentals/servers/httpsys) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

  > [!WARNING]
  > <span data-ttu-id="f093f-337">Se você não está usando um proxy reverso com a filtragem de host habilitada, habilite a [filtragem de host](#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="f093f-337">If not using a reverse proxy with host filtering enabled, enable [host filtering](#host-filtering).</span></span>

* <span data-ttu-id="f093f-338">Nome do `localhost` do host com o número da porta ou o IP de loopback com o número da porta</span><span class="sxs-lookup"><span data-stu-id="f093f-338">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="f093f-339">Quando o `localhost` for especificado, o Kestrel tentará se associar às interfaces de loopback IPv4 e IPv6.</span><span class="sxs-lookup"><span data-stu-id="f093f-339">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="f093f-340">Se a porta solicitada está sendo usada por outro serviço em uma das interfaces de loopback, o Kestrel falha ao ser iniciado.</span><span class="sxs-lookup"><span data-stu-id="f093f-340">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="f093f-341">Se uma das interfaces de loopback não estiver disponível por qualquer outro motivo (geralmente porque não há suporte para o IPv6), o Kestrel registra um aviso em log.</span><span class="sxs-lookup"><span data-stu-id="f093f-341">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f093f-342">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f093f-342">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

* <span data-ttu-id="f093f-343">Endereço IPv4 com o número da porta</span><span class="sxs-lookup"><span data-stu-id="f093f-343">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  https://65.55.39.10:443/
  ```

  <span data-ttu-id="f093f-344">`0.0.0.0` é um caso especial que associa a todos os endereços IPv4.</span><span class="sxs-lookup"><span data-stu-id="f093f-344">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="f093f-345">Endereço IPv6 com número da porta</span><span class="sxs-lookup"><span data-stu-id="f093f-345">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  https://[0:0:0:0:0:ffff:4137:270a]:443/
  ```

  <span data-ttu-id="f093f-346">`[::]` é o equivalente do IPv6 ao IPv4 `0.0.0.0`.</span><span class="sxs-lookup"><span data-stu-id="f093f-346">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="f093f-347">Nome do host com o número da porta</span><span class="sxs-lookup"><span data-stu-id="f093f-347">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  https://contoso.com:443/
  https://*:443/
  ```

  <span data-ttu-id="f093f-348">Nomes de host, `*` e `+` não são especiais.</span><span class="sxs-lookup"><span data-stu-id="f093f-348">Host names, `*`, and `+` aren't special.</span></span> <span data-ttu-id="f093f-349">Tudo o que não é um endereço IP ou um `localhost` reconhecido é associado a todos os IPs IPv6 e IPv4.</span><span class="sxs-lookup"><span data-stu-id="f093f-349">Anything that isn't a recognized IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="f093f-350">Para associar nomes de host diferentes a diferentes aplicativos ASP.NET Core na mesma porta, use [WebListener](xref:fundamentals/servers/weblistener) ou um servidor proxy reverso, como o IIS, o Nginx ou o Apache.</span><span class="sxs-lookup"><span data-stu-id="f093f-350">To bind different host names to different ASP.NET Core apps on the same port, use [WebListener](xref:fundamentals/servers/weblistener) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

* <span data-ttu-id="f093f-351">Nome do `localhost` do host com o número da porta ou o IP de loopback com o número da porta</span><span class="sxs-lookup"><span data-stu-id="f093f-351">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="f093f-352">Quando o `localhost` for especificado, o Kestrel tentará se associar às interfaces de loopback IPv4 e IPv6.</span><span class="sxs-lookup"><span data-stu-id="f093f-352">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="f093f-353">Se a porta solicitada está sendo usada por outro serviço em uma das interfaces de loopback, o Kestrel falha ao ser iniciado.</span><span class="sxs-lookup"><span data-stu-id="f093f-353">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="f093f-354">Se uma das interfaces de loopback não estiver disponível por qualquer outro motivo (geralmente porque não há suporte para o IPv6), o Kestrel registra um aviso em log.</span><span class="sxs-lookup"><span data-stu-id="f093f-354">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

* <span data-ttu-id="f093f-355">Soquete do UNIX</span><span class="sxs-lookup"><span data-stu-id="f093f-355">Unix socket</span></span>

  ```
  http://unix:/run/dan-live.sock
  ```

<span data-ttu-id="f093f-356">**Porta 0**</span><span class="sxs-lookup"><span data-stu-id="f093f-356">**Port 0**</span></span>

<span data-ttu-id="f093f-357">Quando o número da porta `0` for especificado, o Kestrel se associará dinamicamente a uma porta disponível.</span><span class="sxs-lookup"><span data-stu-id="f093f-357">When the port number is `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="f093f-358">A associação à porta `0` é permitida para qualquer nome do host ou IP, exceto `localhost`.</span><span class="sxs-lookup"><span data-stu-id="f093f-358">Binding to port `0` is allowed for any host name or IP except for `localhost`.</span></span>

<span data-ttu-id="f093f-359">Quando o aplicativo é executado, a saída da janela do console indica a porta dinâmica na qual o aplicativo pode ser acessado:</span><span class="sxs-lookup"><span data-stu-id="f093f-359">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Now listening on: http://127.0.0.1:48508
```

<span data-ttu-id="f093f-360">**Prefixos de URL para o SSL**</span><span class="sxs-lookup"><span data-stu-id="f093f-360">**URL prefixes for SSL**</span></span>

<span data-ttu-id="f093f-361">Se você estiver chamando o método de extensão `UseHttps`, lembre-se de incluir os prefixos de URL com `https:`:</span><span class="sxs-lookup"><span data-stu-id="f093f-361">If calling the `UseHttps` extension method, be sure to include URL prefixes with `https:`:</span></span>

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
> <span data-ttu-id="f093f-362">HTTPS e HTTP não podem ser hospedados na mesma porta.</span><span class="sxs-lookup"><span data-stu-id="f093f-362">HTTPS and HTTP can't be hosted on the same port.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

* * *

## <a name="host-filtering"></a><span data-ttu-id="f093f-363">Filtragem de host</span><span class="sxs-lookup"><span data-stu-id="f093f-363">Host filtering</span></span>

<span data-ttu-id="f093f-364">Embora o Kestrel permita a configuração com base em prefixos como `http://example.com:5000`, ele geralmente ignora o nome do host.</span><span class="sxs-lookup"><span data-stu-id="f093f-364">While Kestrel supports configuration based on prefixes such as `http://example.com:5000`, Kestrel largely ignores the host name.</span></span> <span data-ttu-id="f093f-365">O host `localhost` é um caso especial usado para a associação a endereços de loopback.</span><span class="sxs-lookup"><span data-stu-id="f093f-365">Host `localhost` is a special case used for binding to loopback addresses.</span></span> <span data-ttu-id="f093f-366">Todo host que não for um endereço IP explícito será associado a todos os endereços IP públicos.</span><span class="sxs-lookup"><span data-stu-id="f093f-366">Any host other than an explicit IP address binds to all public IP addresses.</span></span> <span data-ttu-id="f093f-367">Nenhuma dessas informações é usada para validar os cabeçalhos do `Host` da solicitação.</span><span class="sxs-lookup"><span data-stu-id="f093f-367">None of this information is used to validate request `Host` headers.</span></span>

<span data-ttu-id="f093f-368">Há duas soluções alternativas:</span><span class="sxs-lookup"><span data-stu-id="f093f-368">There are two workarounds:</span></span>

* <span data-ttu-id="f093f-369">O host por trás de um proxy reverso com filtragem de cabeçalho do host.</span><span class="sxs-lookup"><span data-stu-id="f093f-369">Host behind a reverse proxy with host header filtering.</span></span> <span data-ttu-id="f093f-370">Esse era o único cenário compatível com o Kestrel no ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="f093f-370">This was the only supported scenario for Kestrel in ASP.NET Core 1.x.</span></span>
* <span data-ttu-id="f093f-371">Use um middleware para filtrar as solicitações pelo cabeçalho do `Host`.</span><span class="sxs-lookup"><span data-stu-id="f093f-371">Use a middleware to filter requests by the `Host` header.</span></span> <span data-ttu-id="f093f-372">Veja um middleware de exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="f093f-372">A sample middleware follows:</span></span>

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

<span data-ttu-id="f093f-373">Registre o `HostFilteringMiddleware` anterior em `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="f093f-373">Register the preceding `HostFilteringMiddleware` in `Startup.Configure`.</span></span> <span data-ttu-id="f093f-374">Observe que a [ordem do registro de middleware](xref:fundamentals/middleware/index#ordering) é importante.</span><span class="sxs-lookup"><span data-stu-id="f093f-374">Note that the [ordering of middleware registration](xref:fundamentals/middleware/index#ordering) is important.</span></span> <span data-ttu-id="f093f-375">O registro deve ocorrer imediatamente após o registro do middleware de diagnóstico (por exemplo, `app.UseExceptionHandler`).</span><span class="sxs-lookup"><span data-stu-id="f093f-375">Registration should occur immediately after Diagnostic Middleware registration (for example, `app.UseExceptionHandler`).</span></span>

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

<span data-ttu-id="f093f-376">O middleware anterior espera uma chave `AllowedHosts` em *appsettings.\<EnvironmentName>.json*.</span><span class="sxs-lookup"><span data-stu-id="f093f-376">The preceding middleware expects an `AllowedHosts` key in *appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="f093f-377">O valor dessa chave é uma lista separada por ponto-e-vírgula de nomes de host sem os números de porta.</span><span class="sxs-lookup"><span data-stu-id="f093f-377">This key's value is a semicolon-delimited list of host names without the port numbers.</span></span> <span data-ttu-id="f093f-378">Inclua o par chave-valor `AllowedHosts` em *appsettings.Production.json*:</span><span class="sxs-lookup"><span data-stu-id="f093f-378">Include the `AllowedHosts` key-value pair in *appsettings.Production.json*:</span></span>

```json
{
  "AllowedHosts": "example.com"
}
```

<span data-ttu-id="f093f-379">*appsettings.Development.json* (arquivo de configuração do localhost):</span><span class="sxs-lookup"><span data-stu-id="f093f-379">*appsettings.Development.json* (localhost configuration file):</span></span>

```json
{
  "AllowedHosts": "localhost"
}
```

## <a name="additional-resources"></a><span data-ttu-id="f093f-380">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="f093f-380">Additional resources</span></span>

* [<span data-ttu-id="f093f-381">Impor o HTTPS</span><span class="sxs-lookup"><span data-stu-id="f093f-381">Enforce HTTPS</span></span>](xref:security/enforcing-ssl)
* [<span data-ttu-id="f093f-382">Código-fonte do kestrel</span><span class="sxs-lookup"><span data-stu-id="f093f-382">Kestrel source code</span></span>](https://github.com/aspnet/KestrelHttpServer)

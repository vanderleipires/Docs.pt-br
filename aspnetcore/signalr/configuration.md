---
title: Configuração do ASP.NET SignalR Core
author: tdykstra
description: Saiba como configurar aplicativos do SignalR do ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/06/2018
uid: signalr/configuration
ms.openlocfilehash: 855446003ae9d994854d4d8bb7d0f542a22734e4
ms.sourcegitcommit: f43f430a166a7ec137fcad12ded0372747227498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2018
ms.locfileid: "49391096"
---
# <a name="aspnet-core-signalr-configuration"></a><span data-ttu-id="c6117-103">Configuração do ASP.NET SignalR Core</span><span class="sxs-lookup"><span data-stu-id="c6117-103">ASP.NET Core SignalR configuration</span></span>

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="c6117-104">Opções de serialização JSON/MessagePack</span><span class="sxs-lookup"><span data-stu-id="c6117-104">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="c6117-105">SignalR do ASP.NET Core dá suporte a dois protocolos para codificação de mensagens: [JSON](https://www.json.org/) e [MessagePack](https://msgpack.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="c6117-105">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="c6117-106">Cada protocolo tem opções de configuração de serialização.</span><span class="sxs-lookup"><span data-stu-id="c6117-106">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="c6117-107">Serialização JSON pode ser configurada no servidor usando o [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) método de extensão, que pode ser adicionado após [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) em seu `Startup.ConfigureServices` método.</span><span class="sxs-lookup"><span data-stu-id="c6117-107">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method, which can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in your `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="c6117-108">O `AddJsonProtocol` leva um delegado que recebe um `options` objeto.</span><span class="sxs-lookup"><span data-stu-id="c6117-108">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="c6117-109">O [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) propriedade desse objeto é um JSON.NET `JsonSerializerSettings` objeto que pode ser usado para configurar a serialização de argumentos e valores de retorno.</span><span class="sxs-lookup"><span data-stu-id="c6117-109">The [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) property on that object is a JSON.NET `JsonSerializerSettings` object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="c6117-110">Consulte a [documentação do JSON.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm) para obter mais detalhes.</span><span class="sxs-lookup"><span data-stu-id="c6117-110">See the [JSON.NET Documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm) for more details.</span></span>

<span data-ttu-id="c6117-111">Por exemplo, para configurar o serializador para usar nomes de propriedade "PascalCase", em vez dos nomes de "camelCase" padrão, use o seguinte código:</span><span class="sxs-lookup"><span data-stu-id="c6117-111">As an example, to configure the serializer to use "PascalCase" property names, instead of the default "camelCase" names, use the following code:</span></span>

```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver = 
        new DefaultContractResolver();
    });
```

<span data-ttu-id="c6117-112">No cliente do .NET, o mesmo `AddJsonProtocol` método de extensão existe no [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="c6117-112">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="c6117-113">O `Microsoft.Extensions.DependencyInjection` namespace deve ser importado para resolver o método de extensão:</span><span class="sxs-lookup"><span data-stu-id="c6117-113">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

```csharp
// At the top of the file:
using Microsoft.Extensions.DependencyInjection;

// When constructing your connection:
var connection = new HubConnectionBuilder()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver = 
            new DefaultContractResolver();
    })
    .Build();
```

> [!NOTE]
> <span data-ttu-id="c6117-114">Não é possível configurar a serialização JSON no cliente JavaScript neste momento.</span><span class="sxs-lookup"><span data-stu-id="c6117-114">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="c6117-115">Opções de serialização MessagePack</span><span class="sxs-lookup"><span data-stu-id="c6117-115">MessagePack serialization options</span></span>

<span data-ttu-id="c6117-116">MessagePack serialização pode ser configurada fornecendo um delegado para o [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) chamar.</span><span class="sxs-lookup"><span data-stu-id="c6117-116">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="c6117-117">Ver [MessagePack no SignalR](xref:signalr/messagepackhubprotocol) para obter mais detalhes.</span><span class="sxs-lookup"><span data-stu-id="c6117-117">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="c6117-118">Não é possível configurar a serialização de MessagePack no cliente JavaScript neste momento.</span><span class="sxs-lookup"><span data-stu-id="c6117-118">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="c6117-119">Configurar opções de servidor</span><span class="sxs-lookup"><span data-stu-id="c6117-119">Configure server options</span></span>

<span data-ttu-id="c6117-120">A tabela a seguir descreve as opções de configuração hubs do SignalR:</span><span class="sxs-lookup"><span data-stu-id="c6117-120">The following table describes options for configuring SignalR hubs:</span></span>

| <span data-ttu-id="c6117-121">Opção</span><span class="sxs-lookup"><span data-stu-id="c6117-121">Option</span></span> | <span data-ttu-id="c6117-122">Valor padrão</span><span class="sxs-lookup"><span data-stu-id="c6117-122">Default Value</span></span> | <span data-ttu-id="c6117-123">Descrição</span><span class="sxs-lookup"><span data-stu-id="c6117-123">Description</span></span> |
| ------ | ------------- | ----------- |
| `HandshakeTimeout` | <span data-ttu-id="c6117-124">15 segundos</span><span class="sxs-lookup"><span data-stu-id="c6117-124">15 seconds</span></span> | <span data-ttu-id="c6117-125">Se o cliente não envia uma mensagem de handshake inicial dentro deste intervalo de tempo, a conexão será fechada.</span><span class="sxs-lookup"><span data-stu-id="c6117-125">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="c6117-126">Isso é uma configuração avançada que deve ser modificada apenas se os erros de tempo limite de handshake estiverem ocorrendo devido à latência de rede graves.</span><span class="sxs-lookup"><span data-stu-id="c6117-126">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="c6117-127">Para obter mais detalhes sobre o processo de handshake, consulte a [especificação de protocolo de Hub do SignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="c6117-127">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="c6117-128">15 segundos</span><span class="sxs-lookup"><span data-stu-id="c6117-128">15 seconds</span></span> | <span data-ttu-id="c6117-129">Se o servidor não enviou uma mensagem dentro deste intervalo, uma mensagem de ping é enviada automaticamente para manter a conexão aberta.</span><span class="sxs-lookup"><span data-stu-id="c6117-129">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="c6117-130">Ao alterar `KeepAliveInterval`, altere o `ServerTimeout` / `serverTimeoutInMilliseconds` configuração no cliente.</span><span class="sxs-lookup"><span data-stu-id="c6117-130">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="c6117-131">Recomendado `ServerTimeout` / `serverTimeoutInMilliseconds` valor é double o `KeepAliveInterval` valor.</span><span class="sxs-lookup"><span data-stu-id="c6117-131">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="c6117-132">Todos os protocolos</span><span class="sxs-lookup"><span data-stu-id="c6117-132">All installed protocols</span></span> | <span data-ttu-id="c6117-133">Protocolos com suporte por esse hub.</span><span class="sxs-lookup"><span data-stu-id="c6117-133">Protocols supported by this hub.</span></span> <span data-ttu-id="c6117-134">Por padrão, todos os protocolos registrados no servidor são permitidos, mas protocolos podem ser removidos dessa lista para desabilitar os protocolos específicos para hubs individuais.</span><span class="sxs-lookup"><span data-stu-id="c6117-134">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="c6117-135">Se `true`detalhada mensagens de exceção são retornadas aos clientes quando uma exceção é gerada em um método de Hub.</span><span class="sxs-lookup"><span data-stu-id="c6117-135">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="c6117-136">O padrão é `false`, conforme essas mensagens de exceção podem conter informações confidenciais.</span><span class="sxs-lookup"><span data-stu-id="c6117-136">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

<span data-ttu-id="c6117-137">Opções podem ser configuradas para todos os hubs, fornecendo um delegado de opções para o `AddSignalR` chamar em `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="c6117-137">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSignalR(hubOptions =>
    {
        hubOptions.EnableDetailedErrors = true;
        hubOptions.KeepAliveInterval = TimeSpan.FromMinutes(1);
    });
}
```

<span data-ttu-id="c6117-138">Opções para um único hub substituem as opções de globais fornecidas no `AddSignalR` e pode ser configurado usando [AddHubOptions\<T >](/dotnet/api/microsoft.extensions.dependencyinjection.huboptionsdependencyinjectionextensions.addhuboptions):</span><span class="sxs-lookup"><span data-stu-id="c6117-138">Options for a single hub override the global options provided in `AddSignalR` and can be configured using [AddHubOptions\<T>](/dotnet/api/microsoft.extensions.dependencyinjection.huboptionsdependencyinjectionextensions.addhuboptions):</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

<span data-ttu-id="c6117-139">Use `HttpConnectionDispatcherOptions` para definir configurações avançadas relacionadas a transportes e gerenciamento de buffer de memória.</span><span class="sxs-lookup"><span data-stu-id="c6117-139">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="c6117-140">Essas opções são configuradas passando um delegado para [MapHub\<T >](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub).</span><span class="sxs-lookup"><span data-stu-id="c6117-140">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub).</span></span>

| <span data-ttu-id="c6117-141">Opção</span><span class="sxs-lookup"><span data-stu-id="c6117-141">Option</span></span> | <span data-ttu-id="c6117-142">Valor padrão</span><span class="sxs-lookup"><span data-stu-id="c6117-142">Default Value</span></span> | <span data-ttu-id="c6117-143">Descrição</span><span class="sxs-lookup"><span data-stu-id="c6117-143">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="c6117-144">32 KB</span><span class="sxs-lookup"><span data-stu-id="c6117-144">32 KB</span></span> | <span data-ttu-id="c6117-145">O número máximo de bytes recebidos do cliente que os buffers de servidor.</span><span class="sxs-lookup"><span data-stu-id="c6117-145">The maximum number of bytes received from the client that the server buffers.</span></span> <span data-ttu-id="c6117-146">Aumentar esse valor permite que o servidor receber mensagens maiores, mas pode afetar negativamente o consumo de memória.</span><span class="sxs-lookup"><span data-stu-id="c6117-146">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="c6117-147">Dados coletados automaticamente a partir de `Authorize` atributos aplicados à classe Hub.</span><span class="sxs-lookup"><span data-stu-id="c6117-147">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="c6117-148">Uma lista dos [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objetos usados para determinar se um cliente está autorizado a conectar-se ao hub.</span><span class="sxs-lookup"><span data-stu-id="c6117-148">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="c6117-149">32 KB</span><span class="sxs-lookup"><span data-stu-id="c6117-149">32 KB</span></span> | <span data-ttu-id="c6117-150">O número máximo de bytes enviados pelo aplicativo que os buffers de servidor.</span><span class="sxs-lookup"><span data-stu-id="c6117-150">The maximum number of bytes sent by the app that the server buffers.</span></span> <span data-ttu-id="c6117-151">Aumentar esse valor permite que o servidor enviar mensagens maiores, mas pode afetar negativamente o consumo de memória.</span><span class="sxs-lookup"><span data-stu-id="c6117-151">Increasing this value allows the server to send larger messages, but can negatively impact memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="c6117-152">Todos os transportes estão habilitados.</span><span class="sxs-lookup"><span data-stu-id="c6117-152">All Transports are enabled.</span></span> | <span data-ttu-id="c6117-153">Um bitmask de `HttpTransportType` valores que possam restringir os transportes de um cliente pode usar para se conectar.</span><span class="sxs-lookup"><span data-stu-id="c6117-153">A bitmask of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="c6117-154">Veja a seguir.</span><span class="sxs-lookup"><span data-stu-id="c6117-154">See below.</span></span> | <span data-ttu-id="c6117-155">Opções adicionais específicas para o transporte de sondagem longa.</span><span class="sxs-lookup"><span data-stu-id="c6117-155">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="c6117-156">Veja a seguir.</span><span class="sxs-lookup"><span data-stu-id="c6117-156">See below.</span></span> | <span data-ttu-id="c6117-157">Opções adicionais específicas para o transporte de WebSockets.</span><span class="sxs-lookup"><span data-stu-id="c6117-157">Additional options specific to the WebSockets transport.</span></span> |

<span data-ttu-id="c6117-158">O transporte de sondagem longa tem opções adicionais que podem ser configuradas usando o `LongPolling` propriedade:</span><span class="sxs-lookup"><span data-stu-id="c6117-158">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="c6117-159">Opção</span><span class="sxs-lookup"><span data-stu-id="c6117-159">Option</span></span> | <span data-ttu-id="c6117-160">Valor padrão</span><span class="sxs-lookup"><span data-stu-id="c6117-160">Default Value</span></span> | <span data-ttu-id="c6117-161">Descrição</span><span class="sxs-lookup"><span data-stu-id="c6117-161">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="c6117-162">90 segundos</span><span class="sxs-lookup"><span data-stu-id="c6117-162">90 seconds</span></span> | <span data-ttu-id="c6117-163">A quantidade máxima de tempo o servidor aguarda uma mensagem para enviar ao cliente antes de encerrar uma solicitação de pesquisa único.</span><span class="sxs-lookup"><span data-stu-id="c6117-163">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="c6117-164">Diminuir esse valor faz com que o cliente emitir novas solicitações de sondagem com mais frequência.</span><span class="sxs-lookup"><span data-stu-id="c6117-164">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="c6117-165">O transporte de WebSocket tem opções adicionais que podem ser configuradas usando o `WebSockets` propriedade:</span><span class="sxs-lookup"><span data-stu-id="c6117-165">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="c6117-166">Opção</span><span class="sxs-lookup"><span data-stu-id="c6117-166">Option</span></span> | <span data-ttu-id="c6117-167">Valor padrão</span><span class="sxs-lookup"><span data-stu-id="c6117-167">Default Value</span></span> | <span data-ttu-id="c6117-168">Descrição</span><span class="sxs-lookup"><span data-stu-id="c6117-168">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="c6117-169">5 segundos</span><span class="sxs-lookup"><span data-stu-id="c6117-169">5 seconds</span></span> | <span data-ttu-id="c6117-170">Depois de fecha o servidor, se o cliente não conseguir fechar dentro deste intervalo de tempo, a conexão é encerrada.</span><span class="sxs-lookup"><span data-stu-id="c6117-170">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="c6117-171">Um delegado que pode ser usado para definir o `Sec-WebSocket-Protocol` cabeçalho para um valor personalizado.</span><span class="sxs-lookup"><span data-stu-id="c6117-171">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="c6117-172">O delegado recebe os valores solicitados pelo cliente como entrada e deve retornar o valor desejado.</span><span class="sxs-lookup"><span data-stu-id="c6117-172">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="c6117-173">Configurar opções do cliente</span><span class="sxs-lookup"><span data-stu-id="c6117-173">Configure client options</span></span>

<span data-ttu-id="c6117-174">Opções de cliente podem ser configuradas na `HubConnectionBuilder` (disponível em clientes .NET e JavaScript), do tipo, bem como no `HubConnection` em si.</span><span class="sxs-lookup"><span data-stu-id="c6117-174">Client options can be configured on the `HubConnectionBuilder` type (available in both .NET and JavaScript clients), as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="c6117-175">Configurar o registro em log</span><span class="sxs-lookup"><span data-stu-id="c6117-175">Configure logging</span></span>

<span data-ttu-id="c6117-176">O log está configurado no cliente do .NET usando o `ConfigureLogging` método.</span><span class="sxs-lookup"><span data-stu-id="c6117-176">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="c6117-177">Registro em log provedores e filtros pode ser registrado da mesma forma como estão no servidor.</span><span class="sxs-lookup"><span data-stu-id="c6117-177">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="c6117-178">Consulte a [entrar no ASP.NET Core](xref:fundamentals/logging/index) documentação para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="c6117-178">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="c6117-179">Para registrar os provedores de log, você deve instalar os pacotes necessários.</span><span class="sxs-lookup"><span data-stu-id="c6117-179">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="c6117-180">Consulte a [provedores de log internos](xref:fundamentals/logging/index#built-in-logging-providers) seção do docs para obter uma lista completa.</span><span class="sxs-lookup"><span data-stu-id="c6117-180">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="c6117-181">Por exemplo, para habilitar o log de Console, instale o `Microsoft.Extensions.Logging.Console` pacote do NuGet.</span><span class="sxs-lookup"><span data-stu-id="c6117-181">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="c6117-182">Chamar o `AddConsole` método de extensão:</span><span class="sxs-lookup"><span data-stu-id="c6117-182">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="c6117-183">No cliente JavaScript, um semelhante `configureLogging` existe um método.</span><span class="sxs-lookup"><span data-stu-id="c6117-183">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="c6117-184">Forneça um `LogLevel` valor que indica o nível mínimo de mensagens de log para produzir.</span><span class="sxs-lookup"><span data-stu-id="c6117-184">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="c6117-185">Os logs são gravados na janela de console do navegador.</span><span class="sxs-lookup"><span data-stu-id="c6117-185">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

> [!NOTE]
> <span data-ttu-id="c6117-186">Para desabilitar o registro em log totalmente, especifique `signalR.LogLevel.None` no `configureLogging` método.</span><span class="sxs-lookup"><span data-stu-id="c6117-186">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="c6117-187">Níveis de log disponíveis para o cliente JavaScript estão listados abaixo.</span><span class="sxs-lookup"><span data-stu-id="c6117-187">Log levels available to the JavaScript client are listed below.</span></span> <span data-ttu-id="c6117-188">Definindo o nível de log para um desses valores habilita o log de mensagens no **ou superior** desse nível.</span><span class="sxs-lookup"><span data-stu-id="c6117-188">Setting the log level to one of these values enables logging of messages at **or above** that level.</span></span>

| <span data-ttu-id="c6117-189">Nível</span><span class="sxs-lookup"><span data-stu-id="c6117-189">Level</span></span> | <span data-ttu-id="c6117-190">Descrição</span><span class="sxs-lookup"><span data-stu-id="c6117-190">Description</span></span> |
| ----- | ----------- |
| `None` | <span data-ttu-id="c6117-191">Nenhuma mensagem será registrada.</span><span class="sxs-lookup"><span data-stu-id="c6117-191">No messages are logged.</span></span> |
| `Critical` | <span data-ttu-id="c6117-192">Mensagens que indicam uma falha em todo o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="c6117-192">Messages that indicate a failure in the entire app.</span></span> |
| `Error` | <span data-ttu-id="c6117-193">Mensagens que indicam uma falha na operação atual.</span><span class="sxs-lookup"><span data-stu-id="c6117-193">Messages that indicate a failure in the current operation.</span></span> |
| `Warning` | <span data-ttu-id="c6117-194">Mensagens que indicam um problema de não-fatais.</span><span class="sxs-lookup"><span data-stu-id="c6117-194">Messages that indicate a non-fatal problem.</span></span> |
| `Information` | <span data-ttu-id="c6117-195">Mensagens informativas.</span><span class="sxs-lookup"><span data-stu-id="c6117-195">Informational messages.</span></span> |
| `Debug` | <span data-ttu-id="c6117-196">Mensagens de diagnóstico útil para depuração.</span><span class="sxs-lookup"><span data-stu-id="c6117-196">Diagnostic messages useful for debugging.</span></span> |
| `Trace` | <span data-ttu-id="c6117-197">Mensagens de diagnóstico muito detalhadas projetadas para diagnosticar problemas específicos.</span><span class="sxs-lookup"><span data-stu-id="c6117-197">Very detailed diagnostic messages designed for diagnosing specific issues.</span></span> |

### <a name="configure-allowed-transports"></a><span data-ttu-id="c6117-198">Configure transportes permitidos</span><span class="sxs-lookup"><span data-stu-id="c6117-198">Configure allowed transports</span></span>

<span data-ttu-id="c6117-199">Os transportes usados pelo SignalR podem ser configurados na `WithUrl` chamar (`withUrl` em JavaScript).</span><span class="sxs-lookup"><span data-stu-id="c6117-199">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="c6117-200">Um bitwise OR dos valores de `HttpTransportType` pode ser usado para restringir o cliente para usar somente os transportes especificados.</span><span class="sxs-lookup"><span data-stu-id="c6117-200">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="c6117-201">Todos os transportes são habilitados por padrão.</span><span class="sxs-lookup"><span data-stu-id="c6117-201">All transports are enabled by default.</span></span>

<span data-ttu-id="c6117-202">Por exemplo, para desabilitar o transporte de eventos do Server-Sent, mas permita conexões WebSocket e sondagem longa:</span><span class="sxs-lookup"><span data-stu-id="c6117-202">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="c6117-203">No cliente JavaScript, transportes são configurados, definindo o `transport` campo no objeto de opções fornecido ao `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="c6117-203">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

### <a name="configure-bearer-authentication"></a><span data-ttu-id="c6117-204">Configurar a autenticação do portador</span><span class="sxs-lookup"><span data-stu-id="c6117-204">Configure bearer authentication</span></span>

<span data-ttu-id="c6117-205">Para fornecer dados de autenticação junto com as solicitações de SignalR, use o `AccessTokenProvider` opção (`accessTokenFactory` em JavaScript) para especificar uma função que retorna o token de acesso desejado.</span><span class="sxs-lookup"><span data-stu-id="c6117-205">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="c6117-206">No cliente do .NET, esse token de acesso é passado como um HTTP "Autenticação do portador" token (usando o `Authorization` cabeçalho com um tipo de `Bearer`).</span><span class="sxs-lookup"><span data-stu-id="c6117-206">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="c6117-207">No cliente JavaScript, o token de acesso é usado como um token de portador **exceto** em alguns casos em que o navegador APIs restringir a capacidade de aplicar cabeçalhos (especificamente, em solicitações de eventos do Server-sent e WebSockets).</span><span class="sxs-lookup"><span data-stu-id="c6117-207">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="c6117-208">Nesses casos, o token de acesso é fornecido como um valor de cadeia de caracteres de consulta `access_token`.</span><span class="sxs-lookup"><span data-stu-id="c6117-208">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="c6117-209">No cliente do .NET, o `AccessTokenProvider` opção pode ser especificada usando o delegado de opções no `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="c6117-209">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

<span data-ttu-id="c6117-210">No cliente JavaScript, o token de acesso é configurado definindo a `accessTokenFactory` campo no objeto de opções no `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="c6117-210">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        accessTokenFactory: () => {
            // Get and return the access token.
            // This function can return a JavaScript Promise if asynchronous
            // logic is required to retrieve the access token.
        }
    })
    .build();
```

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="c6117-211">Configurar tempo limite e opções de keep alive</span><span class="sxs-lookup"><span data-stu-id="c6117-211">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="c6117-212">Opções adicionais para configurar o tempo limite e o comportamento de keep alive estão disponíveis no `HubConnection` objeto em si:</span><span class="sxs-lookup"><span data-stu-id="c6117-212">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>

| <span data-ttu-id="c6117-213">Opção de .NET</span><span class="sxs-lookup"><span data-stu-id="c6117-213">.NET Option</span></span> | <span data-ttu-id="c6117-214">Opção de JavaScript</span><span class="sxs-lookup"><span data-stu-id="c6117-214">JavaScript Option</span></span> | <span data-ttu-id="c6117-215">Valor padrão</span><span class="sxs-lookup"><span data-stu-id="c6117-215">Default Value</span></span> | <span data-ttu-id="c6117-216">Descrição</span><span class="sxs-lookup"><span data-stu-id="c6117-216">Description</span></span> |
| ----------- | ----------------- | ------------- | ----------- |
| `ServerTimeout` | `serverTimeoutInMilliseconds` | <span data-ttu-id="c6117-217">30 segundos (30.000 milissegundos)</span><span class="sxs-lookup"><span data-stu-id="c6117-217">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="c6117-218">Tempo limite para a atividade do servidor.</span><span class="sxs-lookup"><span data-stu-id="c6117-218">Timeout for server activity.</span></span> <span data-ttu-id="c6117-219">Se o servidor não enviou uma mensagem nesse intervalo, o cliente considera o servidor desconectado e gatilhos do `Closed` evento (`onclose` em JavaScript).</span><span class="sxs-lookup"><span data-stu-id="c6117-219">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="c6117-220">Esse valor deve ser grande o suficiente para uma mensagem de ping a serem enviados do servidor **e** recebida pelo cliente dentro do intervalo de tempo limite.</span><span class="sxs-lookup"><span data-stu-id="c6117-220">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="c6117-221">O valor recomendado é um número pelo menos duas vezes o servidor `KeepAliveInterval` valor, para dar tempo para pings de chegada.</span><span class="sxs-lookup"><span data-stu-id="c6117-221">The recommended value is a number at least double the server's `KeepAliveInterval` value, to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="c6117-222">Não é configurável</span><span class="sxs-lookup"><span data-stu-id="c6117-222">Not configurable</span></span> | <span data-ttu-id="c6117-223">15 segundos</span><span class="sxs-lookup"><span data-stu-id="c6117-223">15 seconds</span></span> | <span data-ttu-id="c6117-224">Tempo limite para o handshake inicial do servidor.</span><span class="sxs-lookup"><span data-stu-id="c6117-224">Timeout for initial server handshake.</span></span> <span data-ttu-id="c6117-225">Se o servidor não enviar uma resposta de handshake nesse intervalo, o cliente cancela o handshake e gatilhos do `Closed` evento (`onclose` em JavaScript).</span><span class="sxs-lookup"><span data-stu-id="c6117-225">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="c6117-226">Isso é uma configuração avançada que deve ser modificada apenas se os erros de tempo limite de handshake estiverem ocorrendo devido à latência de rede graves.</span><span class="sxs-lookup"><span data-stu-id="c6117-226">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="c6117-227">Para obter mais detalhes sobre o processo de Handshake, consulte a [especificação de protocolo de Hub do SignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="c6117-227">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

<span data-ttu-id="c6117-228">No cliente do .NET, os valores de tempo limite são especificados como `TimeSpan` valores.</span><span class="sxs-lookup"><span data-stu-id="c6117-228">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span> <span data-ttu-id="c6117-229">No cliente JavaScript, os valores de tempo limite são especificados como um número que indica a duração em milissegundos.</span><span class="sxs-lookup"><span data-stu-id="c6117-229">In the JavaScript client, timeout values are specified as a number indicating the duration in milliseconds.</span></span>

### <a name="configure-additional-options"></a><span data-ttu-id="c6117-230">Configurar opções adicionais</span><span class="sxs-lookup"><span data-stu-id="c6117-230">Configure additional options</span></span>

<span data-ttu-id="c6117-231">Opções adicionais podem ser configuradas na `WithUrl` (`withUrl` em JavaScript) método em `HubConnectionBuilder`:</span><span class="sxs-lookup"><span data-stu-id="c6117-231">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder`:</span></span>

| <span data-ttu-id="c6117-232">Opção de .NET</span><span class="sxs-lookup"><span data-stu-id="c6117-232">.NET Option</span></span> | <span data-ttu-id="c6117-233">Opção de JavaScript</span><span class="sxs-lookup"><span data-stu-id="c6117-233">JavaScript Option</span></span> | <span data-ttu-id="c6117-234">Valor padrão</span><span class="sxs-lookup"><span data-stu-id="c6117-234">Default Value</span></span> | <span data-ttu-id="c6117-235">Descrição</span><span class="sxs-lookup"><span data-stu-id="c6117-235">Description</span></span> |
| ----------- | ----------------- | ------------- | ----------- |
| `AccessTokenProvider` | `accessTokenFactory` | `null` | <span data-ttu-id="c6117-236">Uma função que retorna uma cadeia de caracteres que é fornecida como um token de autenticação de portador em solicitações HTTP.</span><span class="sxs-lookup"><span data-stu-id="c6117-236">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `skipNegotiation` | `false` | <span data-ttu-id="c6117-237">Defina isso como `true` para ignorar a etapa de negociação.</span><span class="sxs-lookup"><span data-stu-id="c6117-237">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="c6117-238">**Só tem suporte quando o transporte de WebSockets é o único transporte habilitado**.</span><span class="sxs-lookup"><span data-stu-id="c6117-238">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="c6117-239">Essa configuração não pode ser habilitada ao usar o serviço do Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="c6117-239">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="c6117-240">Não é configurável \*</span><span class="sxs-lookup"><span data-stu-id="c6117-240">Not configurable \*</span></span> | <span data-ttu-id="c6117-241">Vazio</span><span class="sxs-lookup"><span data-stu-id="c6117-241">Empty</span></span> | <span data-ttu-id="c6117-242">Uma coleção de certificados TLS para enviar para autenticar solicitações.</span><span class="sxs-lookup"><span data-stu-id="c6117-242">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="c6117-243">Não é configurável \*</span><span class="sxs-lookup"><span data-stu-id="c6117-243">Not configurable \*</span></span> | <span data-ttu-id="c6117-244">Vazio</span><span class="sxs-lookup"><span data-stu-id="c6117-244">Empty</span></span> | <span data-ttu-id="c6117-245">Uma coleção de cookies HTTP a ser enviado com cada solicitação HTTP.</span><span class="sxs-lookup"><span data-stu-id="c6117-245">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="c6117-246">Não é configurável \*</span><span class="sxs-lookup"><span data-stu-id="c6117-246">Not configurable \*</span></span> | <span data-ttu-id="c6117-247">Vazio</span><span class="sxs-lookup"><span data-stu-id="c6117-247">Empty</span></span> | <span data-ttu-id="c6117-248">Credenciais devem ser enviados com cada solicitação HTTP.</span><span class="sxs-lookup"><span data-stu-id="c6117-248">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="c6117-249">Não é configurável \*</span><span class="sxs-lookup"><span data-stu-id="c6117-249">Not configurable \*</span></span> | <span data-ttu-id="c6117-250">5 segundos</span><span class="sxs-lookup"><span data-stu-id="c6117-250">5 seconds</span></span> | <span data-ttu-id="c6117-251">WebSockets.</span><span class="sxs-lookup"><span data-stu-id="c6117-251">WebSockets only.</span></span> <span data-ttu-id="c6117-252">A quantidade máxima de tempo que o cliente aguardará depois de fechamento para o servidor confirmar a solicitação de fechamento.</span><span class="sxs-lookup"><span data-stu-id="c6117-252">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="c6117-253">Se o servidor não reconhece o fechamento dentro desse período, o cliente se desconecta.</span><span class="sxs-lookup"><span data-stu-id="c6117-253">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="c6117-254">Não é configurável \*</span><span class="sxs-lookup"><span data-stu-id="c6117-254">Not configurable \*</span></span> | <span data-ttu-id="c6117-255">Vazio</span><span class="sxs-lookup"><span data-stu-id="c6117-255">Empty</span></span> | <span data-ttu-id="c6117-256">Um dicionário de cabeçalhos HTTP adicionais para enviar com todas as solicitações HTTP.</span><span class="sxs-lookup"><span data-stu-id="c6117-256">A dictionary of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | <span data-ttu-id="c6117-257">Não é configurável \*</span><span class="sxs-lookup"><span data-stu-id="c6117-257">Not configurable \*</span></span> | `null` | <span data-ttu-id="c6117-258">Um delegado que pode ser usado para configurar ou substituir o `HttpMessageHandler` usado para enviar solicitações HTTP.</span><span class="sxs-lookup"><span data-stu-id="c6117-258">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="c6117-259">Não é usado para conexões de WebSocket.</span><span class="sxs-lookup"><span data-stu-id="c6117-259">Not used for WebSocket connections.</span></span> <span data-ttu-id="c6117-260">Esse delegado deve retornar um valor não nulo e recebe o valor padrão como um parâmetro.</span><span class="sxs-lookup"><span data-stu-id="c6117-260">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="c6117-261">Modificar as configurações nesse valor padrão e retorná-lo ou retornar um novo `HttpMessageHandler` instância.</span><span class="sxs-lookup"><span data-stu-id="c6117-261">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="c6117-262">**Ao substituir o manipulador Certifique-se de copiar as configurações que você deseja impedir que o manipulador fornecido, caso contrário, as opções configuradas (como cabeçalhos e Cookies) não se aplicam ao novo manipulador.**</span><span class="sxs-lookup"><span data-stu-id="c6117-262">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | <span data-ttu-id="c6117-263">Não é configurável \*</span><span class="sxs-lookup"><span data-stu-id="c6117-263">Not configurable \*</span></span> | `null` | <span data-ttu-id="c6117-264">Um proxy HTTP a ser usado ao enviar solicitações HTTP.</span><span class="sxs-lookup"><span data-stu-id="c6117-264">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | <span data-ttu-id="c6117-265">Não é configurável \*</span><span class="sxs-lookup"><span data-stu-id="c6117-265">Not configurable \*</span></span> | `false` | <span data-ttu-id="c6117-266">Defina esse valor booliano para enviar as credenciais padrão para solicitações HTTP e WebSockets.</span><span class="sxs-lookup"><span data-stu-id="c6117-266">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="c6117-267">Isso permite que o uso da autenticação do Windows.</span><span class="sxs-lookup"><span data-stu-id="c6117-267">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | <span data-ttu-id="c6117-268">Não é configurável \*</span><span class="sxs-lookup"><span data-stu-id="c6117-268">Not configurable \*</span></span> | `null` | <span data-ttu-id="c6117-269">Um delegado que pode ser usado para configurar opções adicionais de WebSocket.</span><span class="sxs-lookup"><span data-stu-id="c6117-269">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="c6117-270">Recebe uma instância do [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) que pode ser usado para configurar as opções.</span><span class="sxs-lookup"><span data-stu-id="c6117-270">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

<span data-ttu-id="c6117-271">Opções marcadas com um asterisco (\*) não são configuráveis no cliente JavaScript, devido a limitações no navegador APIs.</span><span class="sxs-lookup"><span data-stu-id="c6117-271">Options marked with an asterisk (\*) aren't configurable in the JavaScript client, due to limitations in browser APIs.</span></span>

<span data-ttu-id="c6117-272">No cliente do .NET, essas opções podem ser modificadas pelo delegado opções fornecido para `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="c6117-272">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="c6117-273">No cliente JavaScript, essas opções podem ser fornecidas em um objeto JavaScript fornecido para `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="c6117-273">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

## <a name="additional-resources"></a><span data-ttu-id="c6117-274">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="c6117-274">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>

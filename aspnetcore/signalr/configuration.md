---
title: Configuração do ASP.NET SignalR Core
author: rachelappel
description: Saiba como configurar aplicativos do ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 06/30/2018
uid: signalr/configuration
ms.openlocfilehash: 167b8828efd093d755aeb1c91e080dbd22b5d485
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/26/2018
ms.locfileid: "36961975"
---
# <a name="aspnet-core-signalr-configuration"></a><span data-ttu-id="72edf-103">Configuração do ASP.NET SignalR Core</span><span class="sxs-lookup"><span data-stu-id="72edf-103">ASP.NET Core SignalR configuration</span></span>

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="72edf-104">Opções de serialização JSON/MessagePack</span><span class="sxs-lookup"><span data-stu-id="72edf-104">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="72edf-105">O SignalR do ASP.NET Core dá suporte a dois protocolos para codificação de mensagens: [JSON](https://www.json.org/) e [MessagePack](https://msgpack.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="72edf-105">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="72edf-106">Cada protocolo tem opções de configuração de serialização.</span><span class="sxs-lookup"><span data-stu-id="72edf-106">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="72edf-107">Serialização JSON pode ser configurada no servidor usando o [ `AddJsonProtocol` ](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) método de extensão, que pode ser adicionado depois [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) no seu `Startup.ConfigureServices` método.</span><span class="sxs-lookup"><span data-stu-id="72edf-107">JSON serialization can be configured on the server using the [`AddJsonProtocol`](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method, which can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in your `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="72edf-108">O `AddJsonProtocol` método usa um delegado que recebe um `options` objeto.</span><span class="sxs-lookup"><span data-stu-id="72edf-108">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="72edf-109">O [ `PayloadSerializerSettings` ](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) propriedade no objeto é um JSON.NET `JsonSerializerSettings` objeto que pode ser usado para configurar a serialização de argumentos e valores de retorno.</span><span class="sxs-lookup"><span data-stu-id="72edf-109">The [`PayloadSerializerSettings`](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) property on that object is a JSON.NET `JsonSerializerSettings` object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="72edf-110">Consulte o [JSON.NET documentação](https://www.newtonsoft.com/json/help/html/Introduction.htm) para obter mais detalhes.</span><span class="sxs-lookup"><span data-stu-id="72edf-110">See the [JSON.NET Documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm) for more details.</span></span>

<span data-ttu-id="72edf-111">Por exemplo, para configurar o serializador para usar nomes de propriedade "PascalCase", em vez dos nomes de "camelCase" padrão, use o seguinte código:</span><span class="sxs-lookup"><span data-stu-id="72edf-111">As an example, to configure the serializer to use "PascalCase" property names, instead of the default "camelCase" names, use the following code:</span></span>

```csharp
services.AddSignalR()
    .AddJsonHubProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver = 
        new DefaultContractResolver();
    });
```

<span data-ttu-id="72edf-112">No cliente .NET, o mesmo `AddJsonHubProtocol` existe método de extensão no [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="72edf-112">In the .NET client, the same `AddJsonHubProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="72edf-113">O `Microsoft.Extensions.DependencyInjection` namespace deve ser importado para resolver o método de extensão:</span><span class="sxs-lookup"><span data-stu-id="72edf-113">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

```csharp
// At the top of the file:
using Microsoft.Extensions.DependencyInjection;

// When constructing your connection:
var connection = new HubConnectionBuilder()
.AddJsonHubProtocol(options => {
    options.PayloadSerializerSettings.ContractResolver = 
        new DefaultContractResolver();
});
```

> [!NOTE]
> <span data-ttu-id="72edf-114">Não é possível configurar a serialização JSON em cliente JavaScript no momento.</span><span class="sxs-lookup"><span data-stu-id="72edf-114">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="72edf-115">Opções de serialização MessagePack</span><span class="sxs-lookup"><span data-stu-id="72edf-115">MessagePack serialization options</span></span>

<span data-ttu-id="72edf-116">Serialização de MessagePack pode ser configurada, fornecendo um delegado para o [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) chamar.</span><span class="sxs-lookup"><span data-stu-id="72edf-116">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="72edf-117">Consulte [MessagePack no SignalR](xref:signalr/messagepackhubprotocol) para obter mais detalhes.</span><span class="sxs-lookup"><span data-stu-id="72edf-117">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="72edf-118">Não é possível configurar MessagePack serialização no cliente JavaScript no momento.</span><span class="sxs-lookup"><span data-stu-id="72edf-118">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="72edf-119">Configurar opções de servidor</span><span class="sxs-lookup"><span data-stu-id="72edf-119">Configure server options</span></span>

<span data-ttu-id="72edf-120">A tabela a seguir descreve as opções de configuração de hubs de SignalR:</span><span class="sxs-lookup"><span data-stu-id="72edf-120">The following table describes options for configuring SignalR hubs:</span></span>

| <span data-ttu-id="72edf-121">Opção</span><span class="sxs-lookup"><span data-stu-id="72edf-121">Option</span></span> | <span data-ttu-id="72edf-122">Descrição</span><span class="sxs-lookup"><span data-stu-id="72edf-122">Description</span></span> |
| ------ | ----------- |
| `HandshakeTimeout` | <span data-ttu-id="72edf-123">Se o cliente não enviar uma mensagem de handshake inicial dentro deste intervalo de tempo, a conexão será fechada.</span><span class="sxs-lookup"><span data-stu-id="72edf-123">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="72edf-124">Se o servidor ainda não enviou uma mensagem dentro deste intervalo, uma mensagem de ping é enviada automaticamente para manter a conexão aberta.</span><span class="sxs-lookup"><span data-stu-id="72edf-124">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> |
| `SupportedProtocols` | <span data-ttu-id="72edf-125">Protocolos suportados por esse hub.</span><span class="sxs-lookup"><span data-stu-id="72edf-125">Protocols supported by this hub.</span></span> <span data-ttu-id="72edf-126">Por padrão, todos os protocolos registrados no servidor são permitidos, mas protocolos podem ser removidos da lista para desabilitar protocolos específicos para os hubs individuais.</span><span class="sxs-lookup"><span data-stu-id="72edf-126">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | <span data-ttu-id="72edf-127">Se `true`detalhados mensagens de exceção são retornadas aos clientes quando uma exceção é gerada em um método de Hub.</span><span class="sxs-lookup"><span data-stu-id="72edf-127">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="72edf-128">O padrão é `false`, pois essas mensagens de exceção podem conter informações confidenciais.</span><span class="sxs-lookup"><span data-stu-id="72edf-128">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

<span data-ttu-id="72edf-129">Opções podem ser configuradas para todos os hubs, fornecendo um delegado de opções para o `AddSignalR` chamar `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="72edf-129">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSignalR(hubOptions =>
    {
        hubOptions.EnableDetailedErrors = true;
        hubOptions.KeepAliveInterval = TimeSpan.FromMinutes(1);
    })
}
```

<span data-ttu-id="72edf-130">Opções para um único hub substituem as opções de global fornecidas no `AddSignalR` e podem ser configuradas usando [ `AddHubOptions<T>` ](/dotnet/api/microsoft.extensions.dependencyinjection.huboptionsdependencyinjectionextensions.addhuboptions):</span><span class="sxs-lookup"><span data-stu-id="72edf-130">Options for a single hub override the global options provided in `AddSignalR` and can be configured using [`AddHubOptions<T>`](/dotnet/api/microsoft.extensions.dependencyinjection.huboptionsdependencyinjectionextensions.addhuboptions):</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
}
```

<span data-ttu-id="72edf-131">Use `HttpConnectionDispatcherOptions` para definir configurações avançadas relacionadas a transportes e gerenciamento de buffer de memória.</span><span class="sxs-lookup"><span data-stu-id="72edf-131">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="72edf-132">Essas opções são configuradas passando um delegado para [ `MapHub<T>` ](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub).</span><span class="sxs-lookup"><span data-stu-id="72edf-132">These options are configured by passing a delegate to [`MapHub<T>`](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub).</span></span>

| <span data-ttu-id="72edf-133">Opção</span><span class="sxs-lookup"><span data-stu-id="72edf-133">Option</span></span> | <span data-ttu-id="72edf-134">Descrição</span><span class="sxs-lookup"><span data-stu-id="72edf-134">Description</span></span> |
| ------ | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="72edf-135">O número máximo de bytes recebidos do cliente que os buffers de servidor.</span><span class="sxs-lookup"><span data-stu-id="72edf-135">The maximum number of bytes received from the client that the server buffers.</span></span> <span data-ttu-id="72edf-136">Aumentar esse valor permite que o servidor receber mensagens maiores, mas pode afetar negativamente o consumo de memória.</span><span class="sxs-lookup"><span data-stu-id="72edf-136">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> <span data-ttu-id="72edf-137">O valor padrão é 32KB.</span><span class="sxs-lookup"><span data-stu-id="72edf-137">The default value is 32KB.</span></span> |
| `AuthorizationData` | <span data-ttu-id="72edf-138">Uma lista de [ `IAuthorizeData` ](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objetos usados para determinar se um cliente está autorizado a conectar-se ao hub.</span><span class="sxs-lookup"><span data-stu-id="72edf-138">A list of [`IAuthorizeData`](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> <span data-ttu-id="72edf-139">Por padrão, isso é preenchido com valores da `Authorize` atributos aplicados à classe Hub.</span><span class="sxs-lookup"><span data-stu-id="72edf-139">By default, this is populated with values from the `Authorize` attributes applied to the Hub class.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="72edf-140">O número máximo de bytes enviados pelo aplicativo que os buffers de servidor.</span><span class="sxs-lookup"><span data-stu-id="72edf-140">The maximum number of bytes sent by the app that the server buffers.</span></span> <span data-ttu-id="72edf-141">Aumentar esse valor permite que o servidor enviar mensagens maiores, mas pode afetar negativamente o consumo de memória.</span><span class="sxs-lookup"><span data-stu-id="72edf-141">Increasing this value allows the server to send larger messages, but can negatively impact memory consumption.</span></span> <span data-ttu-id="72edf-142">O valor padrão é 32KB.</span><span class="sxs-lookup"><span data-stu-id="72edf-142">The default value is 32KB.</span></span> |
| `Transports` | <span data-ttu-id="72edf-143">Uma máscara de bits de `HttpTransportType` valores que possam restringir os transportes de um cliente pode usar para se conectar.</span><span class="sxs-lookup"><span data-stu-id="72edf-143">A bitmask of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> <span data-ttu-id="72edf-144">Todos os transportes são habilitados por padrão.</span><span class="sxs-lookup"><span data-stu-id="72edf-144">All transports are enabled by default.</span></span> |
| `LongPolling` | <span data-ttu-id="72edf-145">Opções adicionais específicas para o transporte de sondagem longa.</span><span class="sxs-lookup"><span data-stu-id="72edf-145">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="72edf-146">Opções adicionais específicas para o transporte de WebSocket.</span><span class="sxs-lookup"><span data-stu-id="72edf-146">Additional options specific to the WebSockets transport.</span></span> |

<span data-ttu-id="72edf-147">O transporte de sondagem longa tem opções adicionais que podem ser configuradas usando o `LongPolling` propriedade:</span><span class="sxs-lookup"><span data-stu-id="72edf-147">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="72edf-148">Opção</span><span class="sxs-lookup"><span data-stu-id="72edf-148">Option</span></span> | <span data-ttu-id="72edf-149">Descrição</span><span class="sxs-lookup"><span data-stu-id="72edf-149">Description</span></span> |
| ------ | ----------- |
| `PollTimeout` | <span data-ttu-id="72edf-150">A quantidade máxima de tempo que o servidor espera por uma mensagem para enviar ao cliente antes de encerrar uma solicitação de pesquisa único.</span><span class="sxs-lookup"><span data-stu-id="72edf-150">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="72edf-151">Diminuir esse valor faz com que o cliente emita novas solicitações de sondagem com mais frequência.</span><span class="sxs-lookup"><span data-stu-id="72edf-151">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> <span data-ttu-id="72edf-152">O valor padrão é 90 segundos.</span><span class="sxs-lookup"><span data-stu-id="72edf-152">The default value is 90 seconds.</span></span> |

<span data-ttu-id="72edf-153">O transporte de WebSocket tem opções adicionais que podem ser configuradas usando o `WebSockets` propriedade:</span><span class="sxs-lookup"><span data-stu-id="72edf-153">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="72edf-154">Opção</span><span class="sxs-lookup"><span data-stu-id="72edf-154">Option</span></span> | <span data-ttu-id="72edf-155">Descrição</span><span class="sxs-lookup"><span data-stu-id="72edf-155">Description</span></span> |
| ------ | ----------- |
| `CloseTimeout` | <span data-ttu-id="72edf-156">Depois de fecha o servidor, se o cliente não conseguir fechar dentro deste intervalo de tempo, a conexão será encerrada.</span><span class="sxs-lookup"><span data-stu-id="72edf-156">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | <span data-ttu-id="72edf-157">Um delegado que pode ser usado para definir o `Sec-WebSocket-Protocol` cabeçalho para um valor personalizado.</span><span class="sxs-lookup"><span data-stu-id="72edf-157">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="72edf-158">O representante recebe os valores solicitados pelo cliente como entrada e deve retornar o valor desejado.</span><span class="sxs-lookup"><span data-stu-id="72edf-158">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="72edf-159">Configurar opções do cliente</span><span class="sxs-lookup"><span data-stu-id="72edf-159">Configure client options</span></span>

<span data-ttu-id="72edf-160">Opções do cliente podem ser configuradas no `HubConnectionBuilder` tipo (disponível em clientes .NET e JavaScript), bem como no `HubConnection` em si.</span><span class="sxs-lookup"><span data-stu-id="72edf-160">Client options can be configured on the `HubConnectionBuilder` type (available in both .NET and JavaScript clients), as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="72edf-161">Configurar registro em log</span><span class="sxs-lookup"><span data-stu-id="72edf-161">Configure logging</span></span>

<span data-ttu-id="72edf-162">O log está configurado no cliente do .NET usando o `ConfigureLogging` método.</span><span class="sxs-lookup"><span data-stu-id="72edf-162">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="72edf-163">Provedores e filtros de registro pode ser registrado da mesma maneira como eles estão no servidor.</span><span class="sxs-lookup"><span data-stu-id="72edf-163">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="72edf-164">Consulte o [efetuar login no ASP.NET Core](xref:fundamentals/logging/index#how-to-add-providers) documentação para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="72edf-164">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index#how-to-add-providers) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="72edf-165">Para registrar provedores de log, você deve instalar os pacotes necessários.</span><span class="sxs-lookup"><span data-stu-id="72edf-165">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="72edf-166">Consulte o [provedores de logs interno](xref:fundamentals/logging/index#built-in-logging-providers) seção de documentos para obter uma lista completa.</span><span class="sxs-lookup"><span data-stu-id="72edf-166">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="72edf-167">Por exemplo, para habilitar o log de Console, instale o `Microsoft.Extensions.Logging.Console` pacote NuGet.</span><span class="sxs-lookup"><span data-stu-id="72edf-167">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="72edf-168">Chamar o `AddConsole` método de extensão:</span><span class="sxs-lookup"><span data-stu-id="72edf-168">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="72edf-169">No cliente de JavaScript, semelhante `configureLogging` método existe.</span><span class="sxs-lookup"><span data-stu-id="72edf-169">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="72edf-170">Forneça um `LogLevel` valor que indica o nível mínimo de mensagens de log para produzir.</span><span class="sxs-lookup"><span data-stu-id="72edf-170">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="72edf-171">Logs são gravados na janela de console do navegador.</span><span class="sxs-lookup"><span data-stu-id="72edf-171">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

> [!NOTE]
> <span data-ttu-id="72edf-172">Para desabilitar o registro em log completamente, especifique `signalR.LogLevel.None` no `configureLogging` método.</span><span class="sxs-lookup"><span data-stu-id="72edf-172">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="72edf-173">Níveis de log disponível para o cliente JavaScript estão listados abaixo.</span><span class="sxs-lookup"><span data-stu-id="72edf-173">Log levels available to the JavaScript client are listed below.</span></span> <span data-ttu-id="72edf-174">Definir o nível de log para um desses valores habilita o log de mensagens no **ou acima** nível.</span><span class="sxs-lookup"><span data-stu-id="72edf-174">Setting the log level to one of these values enables logging of messages at **or above** that level.</span></span>

| <span data-ttu-id="72edf-175">Nível</span><span class="sxs-lookup"><span data-stu-id="72edf-175">Level</span></span> | <span data-ttu-id="72edf-176">Descrição</span><span class="sxs-lookup"><span data-stu-id="72edf-176">Description</span></span> |
| ----- | ----------- |
| `None` | <span data-ttu-id="72edf-177">Não são registradas.</span><span class="sxs-lookup"><span data-stu-id="72edf-177">No messages are logged.</span></span> |
| `Critical` | <span data-ttu-id="72edf-178">Mensagens que indicam uma falha no aplicativo inteiro.</span><span class="sxs-lookup"><span data-stu-id="72edf-178">Messages that indicate a failure in the entire app.</span></span> |
| `Error` | <span data-ttu-id="72edf-179">Mensagens que indicam uma falha na operação atual.</span><span class="sxs-lookup"><span data-stu-id="72edf-179">Messages that indicate a failure in the current operation.</span></span> |
| `Warning` | <span data-ttu-id="72edf-180">Mensagens que indicam um problema não fatal.</span><span class="sxs-lookup"><span data-stu-id="72edf-180">Messages that indicate a non-fatal problem.</span></span> |
| `Information` | <span data-ttu-id="72edf-181">Mensagens informativas.</span><span class="sxs-lookup"><span data-stu-id="72edf-181">Informational messages.</span></span> |
| `Debug` | <span data-ttu-id="72edf-182">Mensagens de diagnóstico úteis para depuração.</span><span class="sxs-lookup"><span data-stu-id="72edf-182">Diagnostic messages useful for debugging.</span></span> |
| `Trace` | <span data-ttu-id="72edf-183">Mensagens de diagnóstico muito detalhadas projetadas para diagnosticar problemas específicos.</span><span class="sxs-lookup"><span data-stu-id="72edf-183">Very detailed diagnostic messages designed for diagnosing specific issues.</span></span> |

### <a name="configure-allowed-transports"></a><span data-ttu-id="72edf-184">Configure transportes permitidos</span><span class="sxs-lookup"><span data-stu-id="72edf-184">Configure allowed transports</span></span>

<span data-ttu-id="72edf-185">Os transportes usados pelo SignalR podem ser configurados a `WithUrl` chamar (`withUrl` em JavaScript).</span><span class="sxs-lookup"><span data-stu-id="72edf-185">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="72edf-186">Um bit a bit OR dos valores de `HttpTransportType` pode ser usado para restringir o cliente para usar somente os transportes especificados.</span><span class="sxs-lookup"><span data-stu-id="72edf-186">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="72edf-187">Todos os transportes são habilitados por padrão.</span><span class="sxs-lookup"><span data-stu-id="72edf-187">All transports are enabled by default.</span></span>

<span data-ttu-id="72edf-188">Por exemplo, para desabilitar o transporte Server-Sent eventos, mas permita conexões WebSocket e sondagem longa:</span><span class="sxs-lookup"><span data-stu-id="72edf-188">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="72edf-189">No cliente de JavaScript, os transportes são configurados, definindo o `transport` campo no objeto de opções fornecido para `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="72edf-189">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

### <a name="configure-bearer-authentication"></a><span data-ttu-id="72edf-190">Configurar a autenticação do portador</span><span class="sxs-lookup"><span data-stu-id="72edf-190">Configure bearer authentication</span></span>

<span data-ttu-id="72edf-191">Para fornecer dados junto com o SignalR solicitações de autenticação, use o `AccessTokenProvider` opção (`accessTokenFactory` em JavaScript) para especificar uma função que retorna o token de acesso desejado.</span><span class="sxs-lookup"><span data-stu-id="72edf-191">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="72edf-192">No cliente .NET, esse token de acesso é passado como um HTTP "Autenticação do portador" token (usando o `Authorization` cabeçalho com um tipo de `Bearer`).</span><span class="sxs-lookup"><span data-stu-id="72edf-192">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="72edf-193">O cliente de JavaScript, o token de acesso é usado como um token de portador, **exceto** em alguns casos em que o navegador APIs restringir a capacidade de aplicar cabeçalhos (especificamente, em solicitações de eventos Server-Sent e WebSockets).</span><span class="sxs-lookup"><span data-stu-id="72edf-193">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="72edf-194">Nesses casos, o token de acesso é fornecido como um valor de cadeia de caracteres de consulta `access_token`.</span><span class="sxs-lookup"><span data-stu-id="72edf-194">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="72edf-195">No cliente .NET, o `AccessTokenProvider` opção pode ser especificada usando o delegado de opções no `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="72edf-195">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        }
    })
    .Build();
```

<span data-ttu-id="72edf-196">No cliente de JavaScript, o token de acesso está configurado, definindo o `accessTokenFactory` o objeto de opções no campo `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="72edf-196">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="72edf-197">Configurar o tempo limite e opções de keep-alive</span><span class="sxs-lookup"><span data-stu-id="72edf-197">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="72edf-198">Opções adicionais para configurar o tempo limite e o comportamento de atividade estão disponíveis no `HubConnection` próprio objeto:</span><span class="sxs-lookup"><span data-stu-id="72edf-198">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>

| <span data-ttu-id="72edf-199">Opção de .NET</span><span class="sxs-lookup"><span data-stu-id="72edf-199">.NET Option</span></span> | <span data-ttu-id="72edf-200">Opção de JavaScript</span><span class="sxs-lookup"><span data-stu-id="72edf-200">JavaScript Option</span></span> | <span data-ttu-id="72edf-201">Descrição</span><span class="sxs-lookup"><span data-stu-id="72edf-201">Description</span></span> |
| ----------- | ----------------- | ----------- |
| `ServerTimeout` | `serverTimeoutInMilliseconds` | <span data-ttu-id="72edf-202">Tempo limite para a atividade do servidor.</span><span class="sxs-lookup"><span data-stu-id="72edf-202">Timeout for server activity.</span></span> <span data-ttu-id="72edf-203">Se o servidor ainda não enviou uma mensagem nesse intervalo, o cliente considera que o servidor desconectado e o gatilho de `Closed` evento (`onclose` em JavaScript).</span><span class="sxs-lookup"><span data-stu-id="72edf-203">If the server hasn't sent any message in this interval, the client considers the server disconnected and trigger the `Closed` event (`onclose` in JavaScript).</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="72edf-204">Não é configurável</span><span class="sxs-lookup"><span data-stu-id="72edf-204">Not configurable</span></span> | <span data-ttu-id="72edf-205">Tempo limite de handshake inicial do servidor.</span><span class="sxs-lookup"><span data-stu-id="72edf-205">Timeout for initial server handshake.</span></span> <span data-ttu-id="72edf-206">Se o servidor não enviar uma resposta de handshake nesse intervalo, o cliente cancela o handshake e gatilho o `Closed` evento (`onclose` em JavaScript).</span><span class="sxs-lookup"><span data-stu-id="72edf-206">If the server doesn't send a handshake response in this interval, the client cancels the handshake and trigger the `Closed` event (`onclose` in JavaScript).</span></span> |

<span data-ttu-id="72edf-207">No cliente .NET, os valores de tempo limite são especificados como `TimeSpan` valores.</span><span class="sxs-lookup"><span data-stu-id="72edf-207">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span> <span data-ttu-id="72edf-208">No cliente de JavaScript, os valores de tempo limite são especificados como números.</span><span class="sxs-lookup"><span data-stu-id="72edf-208">In the JavaScript client, timeout values are specified as numbers.</span></span> <span data-ttu-id="72edf-209">Os números representam valores de tempo em milissegundos.</span><span class="sxs-lookup"><span data-stu-id="72edf-209">The numbers represent time values in milliseconds.</span></span>

### <a name="configure-additional-options"></a><span data-ttu-id="72edf-210">Configurar opções adicionais</span><span class="sxs-lookup"><span data-stu-id="72edf-210">Configure additional options</span></span>

<span data-ttu-id="72edf-211">Opções adicionais podem ser configuradas no `WithUrl` (`withUrl` em JavaScript) método `HubConnectionBuilder`:</span><span class="sxs-lookup"><span data-stu-id="72edf-211">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder`:</span></span>

| <span data-ttu-id="72edf-212">Opção de .NET</span><span class="sxs-lookup"><span data-stu-id="72edf-212">.NET Option</span></span> | <span data-ttu-id="72edf-213">Opção de JavaScript</span><span class="sxs-lookup"><span data-stu-id="72edf-213">JavaScript Option</span></span> | <span data-ttu-id="72edf-214">Descrição</span><span class="sxs-lookup"><span data-stu-id="72edf-214">Description</span></span> |
| ----------- | ----------------- | ----------- |
| `AccessTokenProvider` | `accessTokenFactory` | <span data-ttu-id="72edf-215">Uma função que retorne uma cadeia de caracteres que é fornecida como um token de autenticação do portador em solicitações HTTP.</span><span class="sxs-lookup"><span data-stu-id="72edf-215">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `skipNegotiation` | <span data-ttu-id="72edf-216">Defina como `true` para ignorar a etapa de negociação.</span><span class="sxs-lookup"><span data-stu-id="72edf-216">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="72edf-217">**Só tem suporte quando o transporte de WebSocket é o transporte habilitado somente**.</span><span class="sxs-lookup"><span data-stu-id="72edf-217">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="72edf-218">Essa configuração não pode ser habilitada ao usar o serviço do Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="72edf-218">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="72edf-219">Não é configurável \*</span><span class="sxs-lookup"><span data-stu-id="72edf-219">Not configurable \*</span></span> | <span data-ttu-id="72edf-220">Uma coleção de certificados TLS para enviar para autenticar solicitações.</span><span class="sxs-lookup"><span data-stu-id="72edf-220">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="72edf-221">Não é configurável \*</span><span class="sxs-lookup"><span data-stu-id="72edf-221">Not configurable \*</span></span> | <span data-ttu-id="72edf-222">Uma coleção de cookies HTTP para enviar com todas as solicitações HTTP.</span><span class="sxs-lookup"><span data-stu-id="72edf-222">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="72edf-223">Não é configurável \*</span><span class="sxs-lookup"><span data-stu-id="72edf-223">Not configurable \*</span></span> | <span data-ttu-id="72edf-224">Credenciais para serem enviados com cada solicitação HTTP.</span><span class="sxs-lookup"><span data-stu-id="72edf-224">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="72edf-225">Não é configurável \*</span><span class="sxs-lookup"><span data-stu-id="72edf-225">Not configurable \*</span></span> | <span data-ttu-id="72edf-226">Somente o WebSocket.</span><span class="sxs-lookup"><span data-stu-id="72edf-226">WebSockets only.</span></span> <span data-ttu-id="72edf-227">A quantidade máxima de tempo que o cliente aguardará depois de fechamento para o servidor confirmar a solicitação de fechamento.</span><span class="sxs-lookup"><span data-stu-id="72edf-227">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="72edf-228">Se o servidor não reconhece o fechamento dentro desse período, o cliente se desconecta.</span><span class="sxs-lookup"><span data-stu-id="72edf-228">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="72edf-229">Não é configurável \*</span><span class="sxs-lookup"><span data-stu-id="72edf-229">Not configurable \*</span></span> | <span data-ttu-id="72edf-230">Um dicionário de cabeçalhos HTTP adicionais para serem enviados com cada solicitação HTTP.</span><span class="sxs-lookup"><span data-stu-id="72edf-230">A dictionary of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | <span data-ttu-id="72edf-231">Não é configurável \*</span><span class="sxs-lookup"><span data-stu-id="72edf-231">Not configurable \*</span></span> | <span data-ttu-id="72edf-232">Um delegado que pode ser usado para configurar ou substituir o `HttpMessageHandler` usado para enviar solicitações HTTP.</span><span class="sxs-lookup"><span data-stu-id="72edf-232">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="72edf-233">Não é usada para conexões WebSocket.</span><span class="sxs-lookup"><span data-stu-id="72edf-233">Not used for WebSocket connections.</span></span> <span data-ttu-id="72edf-234">Este delegado deve retornar um valor não nulo e recebe o valor padrão como um parâmetro.</span><span class="sxs-lookup"><span data-stu-id="72edf-234">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="72edf-235">Modificar configurações no valor padrão e retorná-lo ou retornar um completamente novo `HttpMessageHandler` instância.</span><span class="sxs-lookup"><span data-stu-id="72edf-235">Either modify settings on that default value and return it, or return a completely new `HttpMessageHandler` instance.</span></span> |
| `Proxy` | <span data-ttu-id="72edf-236">Não é configurável \*</span><span class="sxs-lookup"><span data-stu-id="72edf-236">Not configurable \*</span></span> | <span data-ttu-id="72edf-237">Um proxy HTTP para usar ao enviar solicitações HTTP.</span><span class="sxs-lookup"><span data-stu-id="72edf-237">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | <span data-ttu-id="72edf-238">Não é configurável \*</span><span class="sxs-lookup"><span data-stu-id="72edf-238">Not configurable \*</span></span> | <span data-ttu-id="72edf-239">Defina esse valor booliano para enviar as credenciais padrão para solicitações HTTP e o WebSocket.</span><span class="sxs-lookup"><span data-stu-id="72edf-239">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="72edf-240">Isso permite o uso da autenticação do Windows.</span><span class="sxs-lookup"><span data-stu-id="72edf-240">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | <span data-ttu-id="72edf-241">Não é configurável \*</span><span class="sxs-lookup"><span data-stu-id="72edf-241">Not configurable \*</span></span> | <span data-ttu-id="72edf-242">Um delegado que pode ser usado para configurar opções adicionais de WebSocket.</span><span class="sxs-lookup"><span data-stu-id="72edf-242">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="72edf-243">Recebe uma instância de [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) que pode ser usado para configurar as opções.</span><span class="sxs-lookup"><span data-stu-id="72edf-243">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

<span data-ttu-id="72edf-244">Opções marcadas com um asterisco (\*) não são configuráveis no cliente de JavaScript, devido a limitações no navegador APIs.</span><span class="sxs-lookup"><span data-stu-id="72edf-244">Options marked with an asterisk (\*) aren't configurable in the JavaScript client, due to limitations in browser APIs.</span></span>

<span data-ttu-id="72edf-245">No cliente .NET, essas opções podem ser modificadas pelo delegado opções fornecido para `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="72edf-245">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="72edf-246">No cliente de JavaScript, essas opções podem ser fornecidas em um objeto JavaScript fornecido para `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="72edf-246">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    });
    .build();
```

## <a name="additional-resources"></a><span data-ttu-id="72edf-247">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="72edf-247">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>

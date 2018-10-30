---
title: Diferenças entre o SignalR e SignalR do ASP.NET Core
author: tdykstra
description: Diferenças entre o SignalR e SignalR do ASP.NET Core
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.date: 09/10/2018
uid: signalr/version-differences
ms.openlocfilehash: 3cec37719b743b3c805ada77249f526278e44599
ms.sourcegitcommit: 2ef32676c16f76282f7c23154d13affce8c8bf35
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/30/2018
ms.locfileid: "50234599"
---
# <a name="differences-between-aspnet-signalr-and-aspnet-core-signalr"></a><span data-ttu-id="0e28e-103">Diferenças entre o SignalR do ASP.NET e o SignalR do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0e28e-103">Differences between ASP.NET SignalR and ASP.NET Core SignalR</span></span>

<span data-ttu-id="0e28e-104">SignalR do ASP.NET Core não é compatível com clientes ou servidores para ASP.NET SignalR.</span><span class="sxs-lookup"><span data-stu-id="0e28e-104">ASP.NET Core SignalR isn't compatible with clients or servers for ASP.NET SignalR.</span></span> <span data-ttu-id="0e28e-105">Este artigo fornece detalhes sobre os recursos que foram removidos ou alterados no SignalR do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0e28e-105">This article details features which have been removed or changed in ASP.NET Core SignalR.</span></span>

## <a name="how-to-identify-the-signalr-version"></a><span data-ttu-id="0e28e-106">Como identificar a versão do SignalR</span><span class="sxs-lookup"><span data-stu-id="0e28e-106">How to identify the SignalR version</span></span>

|                      | <span data-ttu-id="0e28e-107">ASP.NET SignalR</span><span class="sxs-lookup"><span data-stu-id="0e28e-107">ASP.NET SignalR</span></span> | <span data-ttu-id="0e28e-108">SignalR do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0e28e-108">ASP.NET Core SignalR</span></span> |
| -------------------- | --------------- | -------------------- |
| <span data-ttu-id="0e28e-109">Pacote do NuGet Server</span><span class="sxs-lookup"><span data-stu-id="0e28e-109">Server NuGet Package</span></span> | [<span data-ttu-id="0e28e-110">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="0e28e-110">Microsoft.AspNet.SignalR</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.SignalR/) | <span data-ttu-id="0e28e-111">[Microsoft](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) (.NET Core)</span><span class="sxs-lookup"><span data-stu-id="0e28e-111">[Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) (.NET Core)</span></span><br><span data-ttu-id="0e28e-112">[Microsoft](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) (.NET Framework)</span><span class="sxs-lookup"><span data-stu-id="0e28e-112">[Microsoft.AspNetCore.SignalR](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) (.NET Framework)</span></span> |
| <span data-ttu-id="0e28e-113">Pacotes NuGet de cliente</span><span class="sxs-lookup"><span data-stu-id="0e28e-113">Client NuGet Packages</span></span> | [<span data-ttu-id="0e28e-114">ASPNET</span><span class="sxs-lookup"><span data-stu-id="0e28e-114">Microsoft.AspNet.SignalR.Client</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.Client/)<br>[<span data-ttu-id="0e28e-115">Microsoft.AspNet.SignalR.JS</span><span class="sxs-lookup"><span data-stu-id="0e28e-115">Microsoft.AspNet.SignalR.JS</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.JS/) | [<span data-ttu-id="0e28e-116">Aspnetcore</span><span class="sxs-lookup"><span data-stu-id="0e28e-116">Microsoft.AspNetCore.SignalR.Client</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client/) |
| <span data-ttu-id="0e28e-117">Pacote npm de cliente</span><span class="sxs-lookup"><span data-stu-id="0e28e-117">Client npm Package</span></span> | [<span data-ttu-id="0e28e-118">SignalR</span><span class="sxs-lookup"><span data-stu-id="0e28e-118">signalr</span></span>](https://www.npmjs.com/package/signalr) | [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) |
| <span data-ttu-id="0e28e-119">Tipo de aplicativo de servidor</span><span class="sxs-lookup"><span data-stu-id="0e28e-119">Server App Type</span></span> | <span data-ttu-id="0e28e-120">ASP.NET (System. Web) ou a auto-hospedagem de OWIN</span><span class="sxs-lookup"><span data-stu-id="0e28e-120">ASP.NET (System.Web) or OWIN Self-Host</span></span> | <span data-ttu-id="0e28e-121">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0e28e-121">ASP.NET Core</span></span> |
| <span data-ttu-id="0e28e-122">Plataformas de servidor com suporte</span><span class="sxs-lookup"><span data-stu-id="0e28e-122">Supported Server Platforms</span></span> | <span data-ttu-id="0e28e-123">.NET framework 4.5 ou posterior</span><span class="sxs-lookup"><span data-stu-id="0e28e-123">.NET Framework 4.5 or later</span></span> | <span data-ttu-id="0e28e-124">.NET Framework 4.6.1 ou posterior</span><span class="sxs-lookup"><span data-stu-id="0e28e-124">.NET Framework 4.6.1 or later</span></span><br><span data-ttu-id="0e28e-125">.NET core 2.1 ou posterior</span><span class="sxs-lookup"><span data-stu-id="0e28e-125">.NET Core 2.1 or later</span></span> |

## <a name="feature-differences"></a><span data-ttu-id="0e28e-126">Diferenças de recursos</span><span class="sxs-lookup"><span data-stu-id="0e28e-126">Feature differences</span></span>

### <a name="automatic-reconnects"></a><span data-ttu-id="0e28e-127">Reconexão automática</span><span class="sxs-lookup"><span data-stu-id="0e28e-127">Automatic reconnects</span></span>

<span data-ttu-id="0e28e-128">Reconexão automática não têm suporte no SignalR do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0e28e-128">Automatic reconnects aren't supported in ASP.NET Core SignalR.</span></span> <span data-ttu-id="0e28e-129">Se o cliente for desconectado, o usuário explicitamente deve iniciar uma nova conexão, se eles desejam se reconectar.</span><span class="sxs-lookup"><span data-stu-id="0e28e-129">If the client is disconnected, the user must explicitly start a new connection if they want to reconnect.</span></span> <span data-ttu-id="0e28e-130">No ASP.NET SignalR, SignalR tentará reconectar-se ao servidor se a conexão for interrompida.</span><span class="sxs-lookup"><span data-stu-id="0e28e-130">In ASP.NET SignalR, SignalR attempts to reconnect to the server if the connection is dropped.</span></span> 

### <a name="protocol-support"></a><span data-ttu-id="0e28e-131">Suporte de protocolo</span><span class="sxs-lookup"><span data-stu-id="0e28e-131">Protocol support</span></span>

<span data-ttu-id="0e28e-132">SignalR do ASP.NET Core dá suporte a JSON, bem como um novo protocolo binário, com base em [MessagePack](xref:signalr/messagepackhubprotocol).</span><span class="sxs-lookup"><span data-stu-id="0e28e-132">ASP.NET Core SignalR supports JSON, as well as a new binary protocol based on [MessagePack](xref:signalr/messagepackhubprotocol).</span></span> <span data-ttu-id="0e28e-133">Além disso, os protocolos personalizados podem ser criados.</span><span class="sxs-lookup"><span data-stu-id="0e28e-133">Additionally, custom protocols can be created.</span></span>

## <a name="differences-on-the-server"></a><span data-ttu-id="0e28e-134">Diferenças no servidor</span><span class="sxs-lookup"><span data-stu-id="0e28e-134">Differences on the server</span></span>

<span data-ttu-id="0e28e-135">As bibliotecas do lado do servidor SignalR do ASP.NET Core são incluídas na [metapacote do Microsoft](xref:fundamentals/metapackage-app) que faz parte do pacote a **aplicativo Web ASP.NET Core** modelo Razor e MVC projetos.</span><span class="sxs-lookup"><span data-stu-id="0e28e-135">The ASP.NET Core SignalR server-side libraries are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) package that's part of the **ASP.NET Core Web Application** template for both Razor and MVC projects.</span></span>

<span data-ttu-id="0e28e-136">SignalR do ASP.NET Core é um middleware do ASP.NET Core, portanto, ele deve ser configurado por meio da chamada [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) em `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="0e28e-136">ASP.NET Core SignalR is an ASP.NET Core middleware, so it must be configured by calling [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in `Startup.ConfigureServices`.</span></span>

```csharp
services.AddSignalR()
```

<span data-ttu-id="0e28e-137">Para configurar o roteamento, mapear as rotas para os hubs de dentro de [UseSignalR](/dotnet/api/microsoft.aspnetcore.builder.signalrappbuilderextensions.usesignalr) chamada de método no `Startup.Configure` método.</span><span class="sxs-lookup"><span data-stu-id="0e28e-137">To configure routing, map routes to hubs inside the [UseSignalR](/dotnet/api/microsoft.aspnetcore.builder.signalrappbuilderextensions.usesignalr) method call in the `Startup.Configure` method.</span></span>

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("/hub");
});
```

### <a name="sticky-sessions"></a><span data-ttu-id="0e28e-138">Sessões temporárias</span><span class="sxs-lookup"><span data-stu-id="0e28e-138">Sticky sessions</span></span>

<span data-ttu-id="0e28e-139">O modelo de expansão do SignalR do ASP.NET permite que os clientes para se reconectar e enviar mensagens para qualquer servidor no farm.</span><span class="sxs-lookup"><span data-stu-id="0e28e-139">The scaleout model for ASP.NET SignalR allows clients to reconnect and send messages to any server in the farm.</span></span> <span data-ttu-id="0e28e-140">No SignalR do ASP.NET Core, o cliente deve interagir com o mesmo servidor durante a conexão.</span><span class="sxs-lookup"><span data-stu-id="0e28e-140">In ASP.NET Core SignalR, the client must interact with the same server for the duration of the connection.</span></span> <span data-ttu-id="0e28e-141">Para escala horizontal usando Redis, isso significa que as sessões temporárias são necessárias.</span><span class="sxs-lookup"><span data-stu-id="0e28e-141">For scaleout using Redis, that means sticky sessions are required.</span></span> <span data-ttu-id="0e28e-142">Para o uso de expansão [serviço do Azure SignalR](/azure/azure-signalr/), sessões temporárias não são necessárias porque o serviço lida com as conexões aos clientes.</span><span class="sxs-lookup"><span data-stu-id="0e28e-142">For scaleout using [Azure SignalR Service](/azure/azure-signalr/), sticky sessions are not required because the service handles connections to clients.</span></span> 

### <a name="single-hub-per-connection"></a><span data-ttu-id="0e28e-143">Hub único por conexão</span><span class="sxs-lookup"><span data-stu-id="0e28e-143">Single hub per connection</span></span>

<span data-ttu-id="0e28e-144">O SignalR do ASP.NET Core, o modelo de conexão foi simplificado.</span><span class="sxs-lookup"><span data-stu-id="0e28e-144">In ASP.NET Core SignalR, the connection model has been simplified.</span></span> <span data-ttu-id="0e28e-145">As conexões são feitas diretamente a um único hub, em vez de uma única conexão está sendo usado para compartilhar o acesso a vários hubs.</span><span class="sxs-lookup"><span data-stu-id="0e28e-145">Connections are made directly to a single hub, rather than a single connection being used to share access to multiple hubs.</span></span>

### <a name="streaming"></a><span data-ttu-id="0e28e-146">Streaming</span><span class="sxs-lookup"><span data-stu-id="0e28e-146">Streaming</span></span>

<span data-ttu-id="0e28e-147">ASP.NET SignalR Core agora dá suporte à [dados de streaming](xref:signalr/streaming) do hub para o cliente.</span><span class="sxs-lookup"><span data-stu-id="0e28e-147">ASP.NET Core SignalR now supports [streaming data](xref:signalr/streaming) from the hub to the client.</span></span>

### <a name="state"></a><span data-ttu-id="0e28e-148">Estado</span><span class="sxs-lookup"><span data-stu-id="0e28e-148">State</span></span>

<span data-ttu-id="0e28e-149">A capacidade de passar o estado arbitrário entre clientes e o hub (geralmente chamado de HubState) foi removida, bem como suporte para mensagens de progresso.</span><span class="sxs-lookup"><span data-stu-id="0e28e-149">The ability to pass arbitrary state between clients and the hub (often called HubState) has been removed, as well as support for progress messages.</span></span> <span data-ttu-id="0e28e-150">Não há nenhum equivalente de proxies de hub no momento.</span><span class="sxs-lookup"><span data-stu-id="0e28e-150">There is no counterpart of hub proxies at the moment.</span></span>

## <a name="differences-on-the-client"></a><span data-ttu-id="0e28e-151">Diferenças no cliente</span><span class="sxs-lookup"><span data-stu-id="0e28e-151">Differences on the client</span></span>

### <a name="typescript"></a><span data-ttu-id="0e28e-152">TypeScript</span><span class="sxs-lookup"><span data-stu-id="0e28e-152">TypeScript</span></span>

<span data-ttu-id="0e28e-153">O cliente SignalR do ASP.NET Core é escrito em [TypeScript](https://www.typescriptlang.org/).</span><span class="sxs-lookup"><span data-stu-id="0e28e-153">The ASP.NET Core SignalR client is written in [TypeScript](https://www.typescriptlang.org/).</span></span> <span data-ttu-id="0e28e-154">Você pode escrever em JavaScript ou TypeScript ao usar o [cliente JavaScript](xref:signalr/javascript-client).</span><span class="sxs-lookup"><span data-stu-id="0e28e-154">You can write in JavaScript or TypeScript when using the [JavaScript client](xref:signalr/javascript-client).</span></span>

### <a name="the-javascript-client-is-hosted-at-npmhttpswwwnpmjscom"></a><span data-ttu-id="0e28e-155">O cliente JavaScript é hospedado em [npm](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="0e28e-155">The JavaScript client is hosted at [npm](https://www.npmjs.com/)</span></span>

<span data-ttu-id="0e28e-156">Nas versões anteriores, o cliente JavaScript foi obtido por meio de um pacote do NuGet no Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="0e28e-156">In previous versions, the JavaScript client was obtained through a NuGet package in Visual Studio.</span></span> <span data-ttu-id="0e28e-157">Para as versões de núcleo, o [ @aspnet/signalr ](https://www.npmjs.com/package/@aspnet/signalr) pacote npm contém as bibliotecas de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="0e28e-157">For the Core versions, the [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) npm package contains the JavaScript libraries.</span></span> <span data-ttu-id="0e28e-158">Este pacote não está incluído na **aplicativo Web ASP.NET Core** modelo.</span><span class="sxs-lookup"><span data-stu-id="0e28e-158">This package isn't included in the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="0e28e-159">Usar npm para obter e instalar o `@aspnet/signalr` pacote npm.</span><span class="sxs-lookup"><span data-stu-id="0e28e-159">Use npm to obtain and install the `@aspnet/signalr` npm package.</span></span>

```console
npm init -y
npm install @aspnet/signalr
```

### <a name="jquery"></a><span data-ttu-id="0e28e-160">jQuery</span><span class="sxs-lookup"><span data-stu-id="0e28e-160">jQuery</span></span>

<span data-ttu-id="0e28e-161">A dependência no jQuery foi removida, no entanto, projetos ainda podem usar jQuery.</span><span class="sxs-lookup"><span data-stu-id="0e28e-161">The dependency on jQuery has been removed, however projects can still use jQuery.</span></span>

### <a name="javascript-client-method-syntax"></a><span data-ttu-id="0e28e-162">Sintaxe de método de cliente JavaScript</span><span class="sxs-lookup"><span data-stu-id="0e28e-162">JavaScript client method syntax</span></span>

<span data-ttu-id="0e28e-163">A sintaxe de JavaScript foi alterado da versão anterior do SignalR.</span><span class="sxs-lookup"><span data-stu-id="0e28e-163">The JavaScript syntax has changed from the previous version of SignalR.</span></span> <span data-ttu-id="0e28e-164">Em vez de usar o `$connection` de objeto, criar uma conexão usando o [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) API.</span><span class="sxs-lookup"><span data-stu-id="0e28e-164">Rather than using the `$connection` object, create a connection using the [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) API.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub")
    .build();
```

<span data-ttu-id="0e28e-165">Use o [em](/javascript/api/@aspnet/signalr/HubConnection#on) método para especificar que o hub pode chamar métodos do cliente.</span><span class="sxs-lookup"><span data-stu-id="0e28e-165">Use the [on](/javascript/api/@aspnet/signalr/HubConnection#on) method to specify client methods that the hub can call.</span></span>

```javascript
connection.on("ReceiveMessage", (user, message) => {
    const msg = message.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
    const encodedMsg = user + " says " + msg;
    log(encodedMsg);
});
```

<span data-ttu-id="0e28e-166">Depois de criar o método do cliente, inicie a conexão de hub.</span><span class="sxs-lookup"><span data-stu-id="0e28e-166">After creating the client method, start the hub connection.</span></span> <span data-ttu-id="0e28e-167">Cadeia de um [catch](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) método para fazer logon ou lidar com erros.</span><span class="sxs-lookup"><span data-stu-id="0e28e-167">Chain a [catch](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) method to log or handle errors.</span></span>

```javascript
connection.start().catch(err => console.error(err.toString()));
```

### <a name="hub-proxies"></a><span data-ttu-id="0e28e-168">Proxies de Hub</span><span class="sxs-lookup"><span data-stu-id="0e28e-168">Hub proxies</span></span>

<span data-ttu-id="0e28e-169">Os proxies de Hub não automaticamente são gerados.</span><span class="sxs-lookup"><span data-stu-id="0e28e-169">Hub proxies are no longer automatically generated.</span></span> <span data-ttu-id="0e28e-170">Em vez disso, o nome do método é passado para o [invocar](/javascript/api/%40aspnet/signalr/hubconnection#invoke) API como uma cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="0e28e-170">Instead, the method name is passed into the [invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) API as a string.</span></span>

### <a name="net-and-other-clients"></a><span data-ttu-id="0e28e-171">.NET e outros clientes</span><span class="sxs-lookup"><span data-stu-id="0e28e-171">.NET and other clients</span></span>

<span data-ttu-id="0e28e-172">O `Microsoft.AspNetCore.SignalR.Client` pacote NuGet contém as bibliotecas de cliente .NET para o SignalR do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0e28e-172">The `Microsoft.AspNetCore.SignalR.Client` NuGet package contains the .NET client libraries for ASP.NET Core SignalR.</span></span>

<span data-ttu-id="0e28e-173">Use o [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder) para criar e compilar uma instância de uma conexão a um hub.</span><span class="sxs-lookup"><span data-stu-id="0e28e-173">Use the [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder) to create and build an instance of a connection to a hub.</span></span>

```csharp
connection = new HubConnectionBuilder()
    .WithUrl("url")
    .Build();
```

## <a name="scaleout-differences"></a><span data-ttu-id="0e28e-174">Diferenças de expansão</span><span class="sxs-lookup"><span data-stu-id="0e28e-174">Scaleout differences</span></span>

<span data-ttu-id="0e28e-175">SignalR do ASP.NET oferece suporte a SQL Server e o Redis.</span><span class="sxs-lookup"><span data-stu-id="0e28e-175">ASP.NET SignalR supports SQL Server and Redis.</span></span> <span data-ttu-id="0e28e-176">SignalR do ASP.NET Core dá suporte ao serviço do Azure SignalR e Redis.</span><span class="sxs-lookup"><span data-stu-id="0e28e-176">ASP.NET Core SignalR supports Azure SignalR Service and Redis.</span></span>

### <a name="aspnet"></a><span data-ttu-id="0e28e-177">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="0e28e-177">ASP.NET</span></span>

* [<span data-ttu-id="0e28e-178">Expansão do SignalR com o barramento de serviço do Azure</span><span class="sxs-lookup"><span data-stu-id="0e28e-178">SignalR scaleout with Azure Service Bus</span></span>](/aspnet/signalr/overview/performance/scaleout-with-windows-azure-service-bus)
* [<span data-ttu-id="0e28e-179">Expansão do SignalR com Redis</span><span class="sxs-lookup"><span data-stu-id="0e28e-179">SignalR scaleout with Redis</span></span>](/aspnet/signalr/overview/performance/scaleout-with-redis)
* [<span data-ttu-id="0e28e-180">Expansão do SignalR com o SQL Server</span><span class="sxs-lookup"><span data-stu-id="0e28e-180">SignalR scaleout with SQL Server</span></span>](/aspnet/signalr/overview/performance/scaleout-with-sql-server)

### <a name="aspnet-core"></a><span data-ttu-id="0e28e-181">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0e28e-181">ASP.NET Core</span></span>

* [<span data-ttu-id="0e28e-182">Serviço Azure SignalR</span><span class="sxs-lookup"><span data-stu-id="0e28e-182">Azure SignalR Service</span></span>](/azure/azure-signalr/)

## <a name="additional-resources"></a><span data-ttu-id="0e28e-183">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="0e28e-183">Additional resources</span></span>

* [<span data-ttu-id="0e28e-184">Hubs</span><span class="sxs-lookup"><span data-stu-id="0e28e-184">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="0e28e-185">Cliente JavaScript</span><span class="sxs-lookup"><span data-stu-id="0e28e-185">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="0e28e-186">Cliente .NET</span><span class="sxs-lookup"><span data-stu-id="0e28e-186">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="0e28e-187">Plataformas compatíveis</span><span class="sxs-lookup"><span data-stu-id="0e28e-187">Supported platforms</span></span>](xref:signalr/supported-platforms)

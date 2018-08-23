---
title: Diferenças entre o SignalR e SignalR do ASP.NET Core
author: tdykstra
description: Diferenças entre o SignalR e SignalR do ASP.NET Core
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.date: 08/20/2018
uid: signalr/version-differences
ms.openlocfilehash: b904f57af3700b6e1e2143913dfa08da9bf8bbd2
ms.sourcegitcommit: d27317c16f113e7c111583042ec7e4c5a26adf6f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/20/2018
ms.locfileid: "41830914"
---
# <a name="differences-between-aspnet-signalr-and-aspnet-core-signalr"></a><span data-ttu-id="4494f-103">Diferenças entre o SignalR do ASP.NET e o SignalR do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4494f-103">Differences between ASP.NET SignalR and ASP.NET Core SignalR</span></span>

<span data-ttu-id="4494f-104">SignalR do ASP.NET Core não é compatível com clientes ou servidores para ASP.NET SignalR.</span><span class="sxs-lookup"><span data-stu-id="4494f-104">ASP.NET Core SignalR isn't compatible with clients or servers for ASP.NET SignalR.</span></span> <span data-ttu-id="4494f-105">Este artigo fornece detalhes sobre os recursos que foram removidos ou alterados no SignalR do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4494f-105">This article details features which have been removed or changed in ASP.NET Core SignalR.</span></span>

## <a name="how-to-identify-the-signalr-version"></a><span data-ttu-id="4494f-106">Como identificar a versão do SignalR</span><span class="sxs-lookup"><span data-stu-id="4494f-106">How to identify the SignalR version</span></span>

|                      | <span data-ttu-id="4494f-107">ASP.NET SignalR</span><span class="sxs-lookup"><span data-stu-id="4494f-107">ASP.NET SignalR</span></span> | <span data-ttu-id="4494f-108">SignalR do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4494f-108">ASP.NET Core SignalR</span></span> |
| -------------------- | --------------- | -------------------- |
| <span data-ttu-id="4494f-109">Pacote do NuGet Server</span><span class="sxs-lookup"><span data-stu-id="4494f-109">Server NuGet Package</span></span> | [<span data-ttu-id="4494f-110">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="4494f-110">Microsoft.AspNet.SignalR</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.SignalR/) | <span data-ttu-id="4494f-111">[Microsoft](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) (.NET Core)</span><span class="sxs-lookup"><span data-stu-id="4494f-111">[Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) (.NET Core)</span></span><br><span data-ttu-id="4494f-112">[Microsoft](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) (.NET Framework)</span><span class="sxs-lookup"><span data-stu-id="4494f-112">[Microsoft.AspNetCore.SignalR](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) (.NET Framework)</span></span> |
| <span data-ttu-id="4494f-113">Pacotes NuGet de cliente</span><span class="sxs-lookup"><span data-stu-id="4494f-113">Client NuGet Packages</span></span> | [<span data-ttu-id="4494f-114">ASPNET</span><span class="sxs-lookup"><span data-stu-id="4494f-114">Microsoft.AspNet.SignalR.Client</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.Client/)<br>[<span data-ttu-id="4494f-115">Microsoft.AspNet.SignalR.JS</span><span class="sxs-lookup"><span data-stu-id="4494f-115">Microsoft.AspNet.SignalR.JS</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.JS/) | [<span data-ttu-id="4494f-116">Aspnetcore</span><span class="sxs-lookup"><span data-stu-id="4494f-116">Microsoft.AspNetCore.SignalR.Client</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client/) |
| <span data-ttu-id="4494f-117">Pacote npm de cliente</span><span class="sxs-lookup"><span data-stu-id="4494f-117">Client npm Package</span></span> | [<span data-ttu-id="4494f-118">SignalR</span><span class="sxs-lookup"><span data-stu-id="4494f-118">signalr</span></span>](https://www.npmjs.com/package/signalr) | [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) |
| <span data-ttu-id="4494f-119">Tipo de aplicativo de servidor</span><span class="sxs-lookup"><span data-stu-id="4494f-119">Server App Type</span></span> | <span data-ttu-id="4494f-120">ASP.NET (System. Web) ou a auto-hospedagem de OWIN</span><span class="sxs-lookup"><span data-stu-id="4494f-120">ASP.NET (System.Web) or OWIN Self-Host</span></span> | <span data-ttu-id="4494f-121">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4494f-121">ASP.NET Core</span></span> |
| <span data-ttu-id="4494f-122">Plataformas de servidor com suporte</span><span class="sxs-lookup"><span data-stu-id="4494f-122">Supported Server Platforms</span></span> | <span data-ttu-id="4494f-123">.NET framework 4.5 ou posterior</span><span class="sxs-lookup"><span data-stu-id="4494f-123">.NET Framework 4.5 or later</span></span> | <span data-ttu-id="4494f-124">.NET Framework 4.6.1 ou posterior</span><span class="sxs-lookup"><span data-stu-id="4494f-124">.NET Framework 4.6.1 or later</span></span><br><span data-ttu-id="4494f-125">.NET core 2.1 ou posterior</span><span class="sxs-lookup"><span data-stu-id="4494f-125">.NET Core 2.1 or later</span></span> |

## <a name="feature-differences"></a><span data-ttu-id="4494f-126">Diferenças de recursos</span><span class="sxs-lookup"><span data-stu-id="4494f-126">Feature differences</span></span>

### <a name="automatic-reconnects"></a><span data-ttu-id="4494f-127">Reconexão automática</span><span class="sxs-lookup"><span data-stu-id="4494f-127">Automatic reconnects</span></span>

<span data-ttu-id="4494f-128">Reconexão automática não têm mais suporte.</span><span class="sxs-lookup"><span data-stu-id="4494f-128">Automatic reconnects are no longer supported.</span></span> <span data-ttu-id="4494f-129">Anteriormente, o SignalR tentou se reconectar ao servidor se a conexão foi descartada.</span><span class="sxs-lookup"><span data-stu-id="4494f-129">Previously, SignalR tried to reconnect to the server if the connection was dropped.</span></span> <span data-ttu-id="4494f-130">Agora, se o cliente é desconectado o usuário explicitamente deve iniciar uma nova conexão se eles desejam se reconectar.</span><span class="sxs-lookup"><span data-stu-id="4494f-130">Now, if the client is disconnected the user must explicitly start a new connection if they want to reconnect.</span></span>

### <a name="protocol-support"></a><span data-ttu-id="4494f-131">Suporte de protocolo</span><span class="sxs-lookup"><span data-stu-id="4494f-131">Protocol support</span></span>

<span data-ttu-id="4494f-132">SignalR do ASP.NET Core dá suporte a JSON, bem como um novo protocolo binário, com base em [MessagePack](xref:signalr/messagepackhubprotocol).</span><span class="sxs-lookup"><span data-stu-id="4494f-132">ASP.NET Core SignalR supports JSON, as well as a new binary protocol based on [MessagePack](xref:signalr/messagepackhubprotocol).</span></span> <span data-ttu-id="4494f-133">Além disso, os protocolos personalizados podem ser criados.</span><span class="sxs-lookup"><span data-stu-id="4494f-133">Additionally, custom protocols can be created.</span></span>

## <a name="differences-on-the-server"></a><span data-ttu-id="4494f-134">Diferenças no servidor</span><span class="sxs-lookup"><span data-stu-id="4494f-134">Differences on the server</span></span>

<span data-ttu-id="4494f-135">As bibliotecas do lado do servidor SignalR do ASP.NET Core são incluídas na [metapacote do Microsoft](xref:fundamentals/metapackage-app) que faz parte do pacote a **aplicativo Web ASP.NET Core** modelo Razor e MVC projetos.</span><span class="sxs-lookup"><span data-stu-id="4494f-135">The ASP.NET Core SignalR server-side libraries are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) package that's part of the **ASP.NET Core Web Application** template for both Razor and MVC projects.</span></span>

<span data-ttu-id="4494f-136">SignalR do ASP.NET Core é um middleware do ASP.NET Core, portanto, ele deve ser configurado por meio da chamada [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) em `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="4494f-136">ASP.NET Core SignalR is an ASP.NET Core middleware, so it must be configured by calling [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in `Startup.ConfigureServices`.</span></span>

```csharp
services.AddSignalR()
```

<span data-ttu-id="4494f-137">Para configurar o roteamento, mapear as rotas para os hubs de dentro de [UseSignalR](/dotnet/api/microsoft.aspnetcore.builder.signalrappbuilderextensions.usesignalr) chamada de método no `Startup.Configure` método.</span><span class="sxs-lookup"><span data-stu-id="4494f-137">To configure routing, map routes to hubs inside the [UseSignalR](/dotnet/api/microsoft.aspnetcore.builder.signalrappbuilderextensions.usesignalr) method call in the `Startup.Configure` method.</span></span>

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("/hub");
});
```

### <a name="sticky-sessions-now-required"></a><span data-ttu-id="4494f-138">Sessões adesivas agora é necessárias</span><span class="sxs-lookup"><span data-stu-id="4494f-138">Sticky sessions now required</span></span>

<span data-ttu-id="4494f-139">Devido a como a expansão funcionou no SignalR do ASP.NET, os clientes podem se reconectar e enviar mensagens para qualquer servidor no farm.</span><span class="sxs-lookup"><span data-stu-id="4494f-139">Because of how scale-out worked in ASP.NET SignalR, clients could reconnect and send messages to any server in the farm.</span></span> <span data-ttu-id="4494f-140">Devido a alterações para o modelo de expansão, bem como a não dar suporte a reconexão, isso não é mais suportado.</span><span class="sxs-lookup"><span data-stu-id="4494f-140">Due to changes to the scale-out model, as well as not supporting reconnects, this is no longer supported.</span></span> <span data-ttu-id="4494f-141">Depois que o cliente se conecta ao servidor, ele deve interagir com o mesmo servidor durante a conexão.</span><span class="sxs-lookup"><span data-stu-id="4494f-141">Once the client connects to the server, it must interact with the same server for the duration of the connection.</span></span>

### <a name="single-hub-per-connection"></a><span data-ttu-id="4494f-142">Hub único por conexão</span><span class="sxs-lookup"><span data-stu-id="4494f-142">Single hub per connection</span></span>

<span data-ttu-id="4494f-143">O SignalR do ASP.NET Core, o modelo de conexão foi simplificado.</span><span class="sxs-lookup"><span data-stu-id="4494f-143">In ASP.NET Core SignalR, the connection model has been simplified.</span></span> <span data-ttu-id="4494f-144">As conexões são feitas diretamente a um único hub, em vez de uma única conexão está sendo usado para compartilhar o acesso a vários hubs.</span><span class="sxs-lookup"><span data-stu-id="4494f-144">Connections are made directly to a single hub, rather than a single connection being used to share access to multiple hubs.</span></span>

### <a name="streaming"></a><span data-ttu-id="4494f-145">Streaming</span><span class="sxs-lookup"><span data-stu-id="4494f-145">Streaming</span></span>

<span data-ttu-id="4494f-146">ASP.NET SignalR Core agora dá suporte à [dados de streaming](xref:signalr/streaming) do hub para o cliente.</span><span class="sxs-lookup"><span data-stu-id="4494f-146">ASP.NET Core SignalR now supports [streaming data](xref:signalr/streaming) from the hub to the client.</span></span>

### <a name="state"></a><span data-ttu-id="4494f-147">Estado</span><span class="sxs-lookup"><span data-stu-id="4494f-147">State</span></span>

<span data-ttu-id="4494f-148">A capacidade de passar o estado arbitrário entre clientes e o hub (geralmente chamado de HubState) foi removida, bem como suporte para mensagens de progresso.</span><span class="sxs-lookup"><span data-stu-id="4494f-148">The ability to pass arbitrary state between clients and the hub (often called HubState) has been removed, as well as support for progress messages.</span></span> <span data-ttu-id="4494f-149">Não há nenhum equivalente de proxies de hub no momento.</span><span class="sxs-lookup"><span data-stu-id="4494f-149">There is no counterpart of hub proxies at the moment.</span></span>

## <a name="differences-on-the-client"></a><span data-ttu-id="4494f-150">Diferenças no cliente</span><span class="sxs-lookup"><span data-stu-id="4494f-150">Differences on the client</span></span>

### <a name="typescript"></a><span data-ttu-id="4494f-151">TypeScript</span><span class="sxs-lookup"><span data-stu-id="4494f-151">TypeScript</span></span>

<span data-ttu-id="4494f-152">O cliente SignalR do ASP.NET Core é escrito em [TypeScript](https://www.typescriptlang.org/).</span><span class="sxs-lookup"><span data-stu-id="4494f-152">The ASP.NET Core SignalR client is written in [TypeScript](https://www.typescriptlang.org/).</span></span> <span data-ttu-id="4494f-153">Você pode escrever em JavaScript ou TypeScript ao usar o [cliente JavaScript](xref:signalr/javascript-client).</span><span class="sxs-lookup"><span data-stu-id="4494f-153">You can write in JavaScript or TypeScript when using the [JavaScript client](xref:signalr/javascript-client).</span></span>

### <a name="the-javascript-client-is-hosted-at-npmhttpswwwnpmjscom"></a><span data-ttu-id="4494f-154">O cliente JavaScript é hospedado em [npm](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="4494f-154">The JavaScript client is hosted at [npm](https://www.npmjs.com/)</span></span>

<span data-ttu-id="4494f-155">Nas versões anteriores, o cliente JavaScript foi obtido por meio de um pacote do NuGet no Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4494f-155">In previous versions, the JavaScript client was obtained through a NuGet package in Visual Studio.</span></span> <span data-ttu-id="4494f-156">Para as versões de núcleo, o [ @aspnet/signalr ](https://www.npmjs.com/package/@aspnet/signalr) pacote npm contém as bibliotecas de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="4494f-156">For the Core versions, the [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) npm package contains the JavaScript libraries.</span></span> <span data-ttu-id="4494f-157">Este pacote não está incluído na **aplicativo Web ASP.NET Core** modelo.</span><span class="sxs-lookup"><span data-stu-id="4494f-157">This package isn't included in the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="4494f-158">Usar npm para obter e instalar o `@aspnet/signalr` pacote npm.</span><span class="sxs-lookup"><span data-stu-id="4494f-158">Use npm to obtain and install the `@aspnet/signalr` npm package.</span></span>

```console
npm init -y
npm install @aspnet/signalr
```

### <a name="jquery"></a><span data-ttu-id="4494f-159">jQuery</span><span class="sxs-lookup"><span data-stu-id="4494f-159">jQuery</span></span>

<span data-ttu-id="4494f-160">A dependência no jQuery foi removida, no entanto, projetos ainda podem usar jQuery.</span><span class="sxs-lookup"><span data-stu-id="4494f-160">The dependency on jQuery has been removed, however projects can still use jQuery.</span></span>

### <a name="javascript-client-method-syntax"></a><span data-ttu-id="4494f-161">Sintaxe de método de cliente JavaScript</span><span class="sxs-lookup"><span data-stu-id="4494f-161">JavaScript client method syntax</span></span>

<span data-ttu-id="4494f-162">A sintaxe de JavaScript foi alterado da versão anterior do SignalR.</span><span class="sxs-lookup"><span data-stu-id="4494f-162">The JavaScript syntax has changed from the previous version of SignalR.</span></span> <span data-ttu-id="4494f-163">Em vez de usar o `$connection` de objeto, criar uma conexão usando o [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) API.</span><span class="sxs-lookup"><span data-stu-id="4494f-163">Rather than using the `$connection` object, create a connection using the [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) API.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub")
    .build();
```

<span data-ttu-id="4494f-164">Use o [em](/javascript/api/@aspnet/signalr/HubConnection#on) método para especificar que o hub pode chamar métodos do cliente.</span><span class="sxs-lookup"><span data-stu-id="4494f-164">Use the [on](/javascript/api/@aspnet/signalr/HubConnection#on) method to specify client methods that the hub can call.</span></span>

```javascript
connection.on("ReceiveMessage", (user, message) => {
    const msg = message.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
    const encodedMsg = user + " says " + msg;
    log(encodedMsg);
});
```

<span data-ttu-id="4494f-165">Depois de criar o método do cliente, inicie a conexão de hub.</span><span class="sxs-lookup"><span data-stu-id="4494f-165">After creating the client method, start the hub connection.</span></span> <span data-ttu-id="4494f-166">Cadeia de um [catch](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) método para fazer logon ou lidar com erros.</span><span class="sxs-lookup"><span data-stu-id="4494f-166">Chain a [catch](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) method to log or handle errors.</span></span>

```javascript
connection.start().catch(err => console.error(err.toString()));
```

### <a name="hub-proxies"></a><span data-ttu-id="4494f-167">Proxies de Hub</span><span class="sxs-lookup"><span data-stu-id="4494f-167">Hub proxies</span></span>

<span data-ttu-id="4494f-168">Os proxies de Hub não automaticamente são gerados.</span><span class="sxs-lookup"><span data-stu-id="4494f-168">Hub proxies are no longer automatically generated.</span></span> <span data-ttu-id="4494f-169">Em vez disso, o nome do método é passado para o [invocar](/javascript/api/%40aspnet/signalr/hubconnection#invoke) API como uma cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="4494f-169">Instead, the method name is passed into the [invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) API as a string.</span></span>

### <a name="net-and-other-clients"></a><span data-ttu-id="4494f-170">.NET e outros clientes</span><span class="sxs-lookup"><span data-stu-id="4494f-170">.NET and other clients</span></span>

<span data-ttu-id="4494f-171">O `Microsoft.AspNetCore.SignalR.Client` pacote NuGet contém as bibliotecas de cliente .NET para o SignalR do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4494f-171">The `Microsoft.AspNetCore.SignalR.Client` NuGet package contains the .NET client libraries for ASP.NET Core SignalR.</span></span>

<span data-ttu-id="4494f-172">Use o [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder) para criar e compilar uma instância de uma conexão a um hub.</span><span class="sxs-lookup"><span data-stu-id="4494f-172">Use the [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder) to create and build an instance of a connection to a hub.</span></span>

```csharp
connection = new HubConnectionBuilder()
    .WithUrl("url")
    .Build();
```

## <a name="additional-resources"></a><span data-ttu-id="4494f-173">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="4494f-173">Additional resources</span></span>

* [<span data-ttu-id="4494f-174">Hubs</span><span class="sxs-lookup"><span data-stu-id="4494f-174">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="4494f-175">Cliente JavaScript</span><span class="sxs-lookup"><span data-stu-id="4494f-175">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="4494f-176">Cliente .NET</span><span class="sxs-lookup"><span data-stu-id="4494f-176">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="4494f-177">Plataformas compatíveis</span><span class="sxs-lookup"><span data-stu-id="4494f-177">Supported platforms</span></span>](xref:signalr/supported-platforms)

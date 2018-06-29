---
title: Diferenças entre o SignalR e SignalR do ASP.NET Core
author: rachelappel
description: Diferenças entre o SignalR e SignalR do ASP.NET Core
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.date: 06/30/2018
uid: signalr/version-differences
ms.openlocfilehash: fd314d93333bd159aef4bb4863be50c646809cf0
ms.sourcegitcommit: 1faf2525902236428dae6a59e375519bafd5d6d7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/28/2018
ms.locfileid: "37090056"
---
# <a name="differences-between-signalr-and-aspnet-core-signalr"></a><span data-ttu-id="ced55-103">Diferenças entre o SignalR e SignalR do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ced55-103">Differences between SignalR and ASP.NET Core SignalR</span></span>

<span data-ttu-id="ced55-104">O SignalR do ASP.NET Core não é compatível com clientes ou servidores do ASP.NET SignalR.</span><span class="sxs-lookup"><span data-stu-id="ced55-104">ASP.NET Core SignalR is not compatible with clients or servers for ASP.NET SignalR.</span></span> <span data-ttu-id="ced55-105">Este artigo fornece detalhes sobre os recursos que foram removidos ou alterados no SignalR do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ced55-105">This article details the features which have been removed or changed in the ASP.NET Core SignalR.</span></span>

## <a name="feature-differences"></a><span data-ttu-id="ced55-106">Diferenças de recursos</span><span class="sxs-lookup"><span data-stu-id="ced55-106">Feature differences</span></span>

### <a name="automatic-reconnects"></a><span data-ttu-id="ced55-107">Reconexão automática</span><span class="sxs-lookup"><span data-stu-id="ced55-107">Automatic reconnects</span></span>

<span data-ttu-id="ced55-108">Não há suporte para a reconexão automática.</span><span class="sxs-lookup"><span data-stu-id="ced55-108">Automatic reconnects are no longer supported.</span></span> <span data-ttu-id="ced55-109">Anteriormente, o SignalR tentou reconectar ao servidor, se a conexão foi descartada.</span><span class="sxs-lookup"><span data-stu-id="ced55-109">Previously, SignalR tried to reconnect to the server if the connection was dropped.</span></span> <span data-ttu-id="ced55-110">Agora, se o cliente for desconectado o usuário explicitamente deve iniciar uma nova conexão se desejar se reconectar.</span><span class="sxs-lookup"><span data-stu-id="ced55-110">Now, if the client is disconnected the user must explicitly start a new connection if they want to reconnect.</span></span>

### <a name="protocol-support"></a><span data-ttu-id="ced55-111">Suporte de protocolo</span><span class="sxs-lookup"><span data-stu-id="ced55-111">Protocol support</span></span>

<span data-ttu-id="ced55-112">O SignalR do ASP.NET Core dá suporte a JSON, bem como um novo protocolo binário com base em [MessagePack](xref:signalr/messagepackhubprotocol).</span><span class="sxs-lookup"><span data-stu-id="ced55-112">ASP.NET Core SignalR supports JSON, as well as a new binary protocol based on [MessagePack](xref:signalr/messagepackhubprotocol).</span></span> <span data-ttu-id="ced55-113">Além disso, os protocolos personalizados podem ser criados.</span><span class="sxs-lookup"><span data-stu-id="ced55-113">Additionally, custom protocols can be created.</span></span>

## <a name="differences-on-the-server"></a><span data-ttu-id="ced55-114">Diferenças no servidor</span><span class="sxs-lookup"><span data-stu-id="ced55-114">Differences on the server</span></span>

<span data-ttu-id="ced55-115">As bibliotecas do lado do servidor SignalR são incluídas no `Microsoft.AspNetCore.App` que faz parte do pacote de **aplicativo Web do ASP.NET Core** modelo para projetos Razor e MVC.</span><span class="sxs-lookup"><span data-stu-id="ced55-115">The SignalR server-side libraries are included in the `Microsoft.AspNetCore.App` package that is part of the **ASP.NET Core Web Application** template for both Razor and MVC projects.</span></span>

<span data-ttu-id="ced55-116">SignalR é um middleware ASP.NET Core, portanto ele deverá ser configurado por meio da chamada `AddSignalR` em `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="ced55-116">SignalR is an ASP.NET Core middleware, so it must be configured by calling `AddSignalR` in `Startup.ConfigureServices`.</span></span>

```csharp
services.AddSignalR();
```

<span data-ttu-id="ced55-117">Para configurar o roteamento, mapear rotas para hubs de dentro do `UseSignalR` da chamada do método `Startup.Configure` método.</span><span class="sxs-lookup"><span data-stu-id="ced55-117">To configure routing, map routes to hubs inside the `UseSignalR` method call in the `Startup.Configure` method.</span></span>

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("/hub");
});
```

### <a name="sticky-sessions-now-required"></a><span data-ttu-id="ced55-118">Sessões Autoadesivas agora</span><span class="sxs-lookup"><span data-stu-id="ced55-118">Sticky sessions now required</span></span>

<span data-ttu-id="ced55-119">Devido a como expansão trabalhado em versões anteriores do SignalR, clientes podem se reconectar e enviar mensagens para qualquer servidor no farm.</span><span class="sxs-lookup"><span data-stu-id="ced55-119">Because of how scale-out worked in the previous versions of SignalR, clients could reconnect and send messages to any server in the farm.</span></span> <span data-ttu-id="ced55-120">Devido a alterações para o modelo de expansão, como também não oferece suporte à reconexão, isso não é mais suportado.</span><span class="sxs-lookup"><span data-stu-id="ced55-120">Due to changes to the scale-out model, as well as not supporting reconnects, this is no longer supported.</span></span> <span data-ttu-id="ced55-121">Agora, depois que o cliente se conecta ao servidor, ele precisa interagir com o mesmo servidor durante a conexão.</span><span class="sxs-lookup"><span data-stu-id="ced55-121">Now, once the client connects to the server it needs to interact with the same server for the duration of the connection.</span></span>

### <a name="single-hub-per-connection"></a><span data-ttu-id="ced55-122">Hub único por conexão</span><span class="sxs-lookup"><span data-stu-id="ced55-122">Single hub per connection</span></span>

<span data-ttu-id="ced55-123">No ASP.NET Core SignalR, o modelo de conexão foi simplificado.</span><span class="sxs-lookup"><span data-stu-id="ced55-123">In ASP.NET Core SignalR, the connection model has been simplified.</span></span> <span data-ttu-id="ced55-124">Conexões são feitas diretamente a um único hub, em vez de uma única conexão que está sendo usado para compartilhar o acesso a vários hubs.</span><span class="sxs-lookup"><span data-stu-id="ced55-124">Connections are made directly to a single hub, rather than a single connection being used to share access to multiple hubs.</span></span>

### <a name="streaming"></a><span data-ttu-id="ced55-125">Streaming</span><span class="sxs-lookup"><span data-stu-id="ced55-125">Streaming</span></span>

<span data-ttu-id="ced55-126">SignalR agora dá suporte a [fluxo de dados](xref:signalr/streaming) do hub para o cliente.</span><span class="sxs-lookup"><span data-stu-id="ced55-126">SignalR now supports [streaming data](xref:signalr/streaming) from the hub to the client.</span></span>

### <a name="state"></a><span data-ttu-id="ced55-127">Estado</span><span class="sxs-lookup"><span data-stu-id="ced55-127">State</span></span>

<span data-ttu-id="ced55-128">A capacidade de passar o estado arbitrário entre clientes e o hub (geralmente chamado de HubState) foi removida, bem como suporte para mensagens de progresso.</span><span class="sxs-lookup"><span data-stu-id="ced55-128">The ability to pass arbitrary state between clients and the hub (often called HubState) has been removed, as well as support for progress messages.</span></span> <span data-ttu-id="ced55-129">Não há nenhum equivalente de proxies de hub no momento.</span><span class="sxs-lookup"><span data-stu-id="ced55-129">There is no counterpart of hub proxies at the moment.</span></span>

## <a name="differences-on-the-client"></a><span data-ttu-id="ced55-130">Diferenças no cliente</span><span class="sxs-lookup"><span data-stu-id="ced55-130">Differences on the client</span></span>

### <a name="typescript"></a><span data-ttu-id="ced55-131">TypeScript</span><span class="sxs-lookup"><span data-stu-id="ced55-131">TypeScript</span></span>

<span data-ttu-id="ced55-132">A versão do ASP.NET Core do SignalR é gravada em [TypeScript](https://www.typescriptlang.org/).</span><span class="sxs-lookup"><span data-stu-id="ced55-132">The ASP.NET Core version of SignalR is written in [TypeScript](https://www.typescriptlang.org/).</span></span> <span data-ttu-id="ced55-133">Você pode escrever em JavaScript ou TypeScript ao usar o [cliente JavaScript](xref:signalr/javascript-client).</span><span class="sxs-lookup"><span data-stu-id="ced55-133">You can write in JavaScript or TypeScript when using the [JavaScript client](xref:signalr/javascript-client).</span></span>

### <a name="the-javascript-client-is-hosted-at-npmhttpswwwnpmjscom"></a><span data-ttu-id="ced55-134">O cliente JavaScript é hospedado em [npm](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="ced55-134">The JavaScript client is hosted at [npm](https://www.npmjs.com/)</span></span>

<span data-ttu-id="ced55-135">Nas versões anteriores, o cliente JavaScript foi obtido por meio de um pacote do NuGet no Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ced55-135">In previous versions, the JavaScript client was obtained through a NuGet package in Visual Studio.</span></span> <span data-ttu-id="ced55-136">Para as versões de núcleo, o [ @aspnet/signalr pacote npm](https://www.npmjs.com/package/@aspnet/signalr) contém as bibliotecas JavaScript.</span><span class="sxs-lookup"><span data-stu-id="ced55-136">For the Core versions, the [@aspnet/signalr npm package](https://www.npmjs.com/package/@aspnet/signalr) contains the JavaScript libraries.</span></span> <span data-ttu-id="ced55-137">Este pacote não está incluído o **aplicativo Web do ASP.NET Core** modelo.</span><span class="sxs-lookup"><span data-stu-id="ced55-137">This package isn't included in the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="ced55-138">Use npm para obter e instalar o `@aspnet/signalr` pacote npm.</span><span class="sxs-lookup"><span data-stu-id="ced55-138">Use npm to obtain and install the `@aspnet/signalr` npm package.</span></span>

```console
npm init -y
npm install @aspnet/signalr
```

### <a name="jquery"></a><span data-ttu-id="ced55-139">jQuery</span><span class="sxs-lookup"><span data-stu-id="ced55-139">jQuery</span></span>

<span data-ttu-id="ced55-140">A dependência no jQuery foi removida, porém projetos ainda podem usar o jQuery.</span><span class="sxs-lookup"><span data-stu-id="ced55-140">The dependency on jQuery has been removed, however projects can still use jQuery.</span></span>

### <a name="javascript-client-method-syntax"></a><span data-ttu-id="ced55-141">Sintaxe de método de cliente JavaScript</span><span class="sxs-lookup"><span data-stu-id="ced55-141">JavaScript client method syntax</span></span>

<span data-ttu-id="ced55-142">A sintaxe JavaScript foi alterado da versão anterior do SignalR.</span><span class="sxs-lookup"><span data-stu-id="ced55-142">The JavaScript syntax has changed from the previous version of SignalR.</span></span> <span data-ttu-id="ced55-143">Em vez de usar o `$connection` de objeto, criar uma conexão usando o `HubConnectionBuilder` API.</span><span class="sxs-lookup"><span data-stu-id="ced55-143">Rather than using the `$connection` object, create a connection using the `HubConnectionBuilder` API.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub")
    .build();
```

<span data-ttu-id="ced55-144">Use `connection.on` para especificar os métodos de cliente que o hub pode chamar.</span><span class="sxs-lookup"><span data-stu-id="ced55-144">Use `connection.on` to specify client methods that the hub can call.</span></span>

```javascript
connection.on("ReceiveMessage", (user, message) => {
    const msg = message.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
    const encodedMsg = user + " says " + msg;
    log(encodedMsg);
});
```

<span data-ttu-id="ced55-145">Depois de criar o método de cliente, inicie a conexão de hub.</span><span class="sxs-lookup"><span data-stu-id="ced55-145">After creating the client method, start the hub connection.</span></span> <span data-ttu-id="ced55-146">Cadeia um `catch` método para registrar ou manipular erros.</span><span class="sxs-lookup"><span data-stu-id="ced55-146">Chain a `catch` method to log or handle errors.</span></span>

```javascript
connection.start().catch(err => console.error(err.toString()));
```

### <a name="hub-proxies"></a><span data-ttu-id="ced55-147">Proxies do hub</span><span class="sxs-lookup"><span data-stu-id="ced55-147">Hub proxies</span></span>

<span data-ttu-id="ced55-148">Os proxies de Hub não automaticamente são gerados.</span><span class="sxs-lookup"><span data-stu-id="ced55-148">Hub proxies are no longer automatically generated.</span></span> <span data-ttu-id="ced55-149">Em vez disso, o nome do método é passado para o `invoke` API como uma cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="ced55-149">Instead, the method name is passed into the `invoke` API as a string.</span></span>

### <a name="net-and-other-clients"></a><span data-ttu-id="ced55-150">.NET e outros clientes</span><span class="sxs-lookup"><span data-stu-id="ced55-150">.NET and other clients</span></span>

<span data-ttu-id="ced55-151">O `Microsoft.AspNetCore.SignalR.Client` pacote NuGet contém as bibliotecas de cliente .NET para o SignalR do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ced55-151">The `Microsoft.AspNetCore.SignalR.Client` NuGet package contains the .NET client libraries for ASP.NET Core SignalR.</span></span>

<span data-ttu-id="ced55-152">Use o `HubConnectionBuilder` gerar e criar uma instância de uma conexão a um hub.</span><span class="sxs-lookup"><span data-stu-id="ced55-152">Use the `HubConnectionBuilder` to create and build an instance of a connection to a hub.</span></span>

```csharp
connection = new HubConnectionBuilder()
    .WithUrl("url")
    .Build();
```

## <a name="additional-resources"></a><span data-ttu-id="ced55-153">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="ced55-153">Additional resources</span></span>

* [<span data-ttu-id="ced55-154">Hubs</span><span class="sxs-lookup"><span data-stu-id="ced55-154">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="ced55-155">Cliente JavaScript</span><span class="sxs-lookup"><span data-stu-id="ced55-155">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="ced55-156">Cliente .NET</span><span class="sxs-lookup"><span data-stu-id="ced55-156">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="ced55-157">Plataformas compatíveis</span><span class="sxs-lookup"><span data-stu-id="ced55-157">Supported platforms</span></span>](xref:signalr/supported-platforms)

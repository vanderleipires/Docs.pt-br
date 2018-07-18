---
title: Usar o streaming em SignalR do ASP.NET Core
author: tdykstra
description: ''
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/07/2018
uid: signalr/streaming
ms.openlocfilehash: 0001eed830249ac46ba35331759187bb4e7e8fd3
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095256"
---
# <a name="use-streaming-in-aspnet-core-signalr"></a><span data-ttu-id="298e2-102">Usar o streaming em SignalR do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="298e2-102">Use streaming in ASP.NET Core SignalR</span></span>

<span data-ttu-id="298e2-103">Por [Brennan Conroy](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="298e2-103">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="298e2-104">SignalR do ASP.NET Core dá suporte a streaming valores de retorno dos métodos de servidor.</span><span class="sxs-lookup"><span data-stu-id="298e2-104">ASP.NET Core SignalR supports streaming return values of server methods.</span></span> <span data-ttu-id="298e2-105">Isso é útil para cenários em que os fragmentos de dados serão apresentadas ao longo do tempo.</span><span class="sxs-lookup"><span data-stu-id="298e2-105">This is useful for scenarios where fragments of data will come in over time.</span></span> <span data-ttu-id="298e2-106">Quando um valor de retorno é transmitido ao cliente, cada fragmento é enviado ao cliente assim que ele se torna disponível, em vez de aguardar que todos os dados fiquem disponíveis.</span><span class="sxs-lookup"><span data-stu-id="298e2-106">When a return value is streamed to the client, each fragment is sent to the client as soon as it becomes available, rather than waiting for all the data to become available.</span></span>

<span data-ttu-id="298e2-107">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="298e2-107">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="set-up-the-hub"></a><span data-ttu-id="298e2-108">Configurar o hub</span><span class="sxs-lookup"><span data-stu-id="298e2-108">Set up the hub</span></span>

<span data-ttu-id="298e2-109">Um método de hub automaticamente se torna um método de hub streaming quando ele retorna um `ChannelReader<T>` ou um `Task<ChannelReader<T>>`.</span><span class="sxs-lookup"><span data-stu-id="298e2-109">A hub method automatically becomes a streaming hub method when it returns a `ChannelReader<T>` or a `Task<ChannelReader<T>>`.</span></span> <span data-ttu-id="298e2-110">Abaixo está um exemplo que mostra os conceitos básicos do fluxo de dados para o cliente.</span><span class="sxs-lookup"><span data-stu-id="298e2-110">Below is a sample that shows the basics of streaming data to the client.</span></span> <span data-ttu-id="298e2-111">Sempre que um objeto é gravado o `ChannelReader` esse objeto é enviado imediatamente para o cliente.</span><span class="sxs-lookup"><span data-stu-id="298e2-111">Whenever an object is written to the `ChannelReader` that object is immediately sent to the client.</span></span> <span data-ttu-id="298e2-112">No final, o `ChannelReader` estiver concluído para dizer ao cliente o fluxo está fechado.</span><span class="sxs-lookup"><span data-stu-id="298e2-112">At the end, the `ChannelReader` is completed to tell the client the stream is closed.</span></span>

> [!NOTE]
> <span data-ttu-id="298e2-113">Gravar o `ChannelReader` em um thread em segundo plano e retorne o `ChannelReader` assim que possível.</span><span class="sxs-lookup"><span data-stu-id="298e2-113">Write to the `ChannelReader` on a background thread and return the `ChannelReader` as soon as possible.</span></span> <span data-ttu-id="298e2-114">Outras chamadas de hub serão bloqueadas até que um `ChannelReader` é retornado.</span><span class="sxs-lookup"><span data-stu-id="298e2-114">Other hub invocations will be blocked until a `ChannelReader` is returned.</span></span>

[!code-csharp[Streaming hub method](streaming/sample/Hubs/StreamHub.cs?range=10-34)]

## <a name="net-client"></a><span data-ttu-id="298e2-115">Cliente .NET</span><span class="sxs-lookup"><span data-stu-id="298e2-115">.NET client</span></span>

<span data-ttu-id="298e2-116">O `StreamAsChannelAsync` método no `HubConnection` é usado para invocar um método de transmissão.</span><span class="sxs-lookup"><span data-stu-id="298e2-116">The `StreamAsChannelAsync` method on `HubConnection` is used to invoke a streaming method.</span></span> <span data-ttu-id="298e2-117">Passe o nome do método de hub e argumentos definidos no método de hub para `StreamAsChannelAsync`.</span><span class="sxs-lookup"><span data-stu-id="298e2-117">Pass the hub method name, and arguments defined in the hub method to `StreamAsChannelAsync`.</span></span> <span data-ttu-id="298e2-118">O parâmetro genérico em `StreamAsChannelAsync<T>` Especifica o tipo de objetos retornados pelo método de transmissão.</span><span class="sxs-lookup"><span data-stu-id="298e2-118">The generic parameter on `StreamAsChannelAsync<T>` specifies the type of objects returned by the streaming method.</span></span> <span data-ttu-id="298e2-119">Um `ChannelReader<T>` é retornada da invocação de fluxo e representa o fluxo no cliente.</span><span class="sxs-lookup"><span data-stu-id="298e2-119">A `ChannelReader<T>` is returned from the stream invocation, and represents the stream on the client.</span></span> <span data-ttu-id="298e2-120">Para ler dados, um padrão comum é executar um loop sobre `WaitToReadAsync` e chamar `TryRead` quando os dados estiverem disponíveis.</span><span class="sxs-lookup"><span data-stu-id="298e2-120">To read data, a common pattern is to loop over `WaitToReadAsync` and call `TryRead` when data is available.</span></span> <span data-ttu-id="298e2-121">O loop terminará quando o fluxo foi fechado pelo servidor ou o token de cancelamento passado para `StreamAsChannelAsync` é cancelada.</span><span class="sxs-lookup"><span data-stu-id="298e2-121">The loop will end when the stream has been closed by the server, or the cancellation token passed to `StreamAsChannelAsync` is canceled.</span></span>

```csharp
var channel = await hubConnection.StreamAsChannelAsync<int>("Counter", 10, 500, CancellationToken.None);

// Wait asynchronously for data to become available
while (await channel.WaitToReadAsync())
{
    // Read all currently available data synchronously, before waiting for more data
    while (channel.TryRead(out var count))
    {
        Console.WriteLine($"{count}");
    }
}

Console.WriteLine("Streaming completed");
```

## <a name="javascript-client"></a><span data-ttu-id="298e2-122">Cliente JavaScript</span><span class="sxs-lookup"><span data-stu-id="298e2-122">JavaScript client</span></span>

<span data-ttu-id="298e2-123">Os clientes JavaScript chamar métodos de streaming em hubs usando `connection.stream`.</span><span class="sxs-lookup"><span data-stu-id="298e2-123">JavaScript clients call streaming methods on hubs by using `connection.stream`.</span></span> <span data-ttu-id="298e2-124">O `stream` método aceita dois argumentos:</span><span class="sxs-lookup"><span data-stu-id="298e2-124">The `stream` method accepts two arguments:</span></span>

* <span data-ttu-id="298e2-125">O nome do método de hub.</span><span class="sxs-lookup"><span data-stu-id="298e2-125">The name of the hub method.</span></span> <span data-ttu-id="298e2-126">No exemplo a seguir, o nome do método de hub é `Counter`.</span><span class="sxs-lookup"><span data-stu-id="298e2-126">In the following example, the hub method name is `Counter`.</span></span>
* <span data-ttu-id="298e2-127">Argumentos definidos no método de hub.</span><span class="sxs-lookup"><span data-stu-id="298e2-127">Arguments defined in the hub method.</span></span> <span data-ttu-id="298e2-128">No exemplo a seguir, os argumentos são: uma contagem do número de itens de fluxo para receber e o atraso entre itens de fluxo.</span><span class="sxs-lookup"><span data-stu-id="298e2-128">In the following example, the arguments are: a count for the number of stream items to receive, and the delay between stream items.</span></span>

<span data-ttu-id="298e2-129">`connection.stream` Retorna um `IStreamResult` que contém um `subscribe` método.</span><span class="sxs-lookup"><span data-stu-id="298e2-129">`connection.stream` returns an `IStreamResult` which contains a `subscribe` method.</span></span> <span data-ttu-id="298e2-130">Passar uma `IStreamSubscriber` para `subscribe` e defina as `next`, `error`, e `complete` retornos de chamada para receber as notificações a `stream` invocação.</span><span class="sxs-lookup"><span data-stu-id="298e2-130">Pass an `IStreamSubscriber` to `subscribe` and set the `next`, `error`, and `complete` callbacks to get notifications from the `stream` invocation.</span></span>

[!code-javascript[Streaming javascript](streaming/sample/wwwroot/js/stream.js?range=19-36)]

<span data-ttu-id="298e2-131">Para encerrar o fluxo da chamada de cliente do `dispose` método na `ISubscription` que é retornado do `subscribe` método.</span><span class="sxs-lookup"><span data-stu-id="298e2-131">To end the stream from the client call the `dispose` method on the `ISubscription` that is returned from the `subscribe` method.</span></span>

## <a name="related-resources"></a><span data-ttu-id="298e2-132">Recursos relacionados</span><span class="sxs-lookup"><span data-stu-id="298e2-132">Related resources</span></span>

* [<span data-ttu-id="298e2-133">Hubs</span><span class="sxs-lookup"><span data-stu-id="298e2-133">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="298e2-134">Cliente .NET</span><span class="sxs-lookup"><span data-stu-id="298e2-134">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="298e2-135">Cliente JavaScript</span><span class="sxs-lookup"><span data-stu-id="298e2-135">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="298e2-136">Publicar no Azure</span><span class="sxs-lookup"><span data-stu-id="298e2-136">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)

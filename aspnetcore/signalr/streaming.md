---
title: Use o SignalR do ASP.NET Core de streaming
author: rachelappel
description: ''
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 06/07/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/streaming
ms.openlocfilehash: e8b32d5f25ae3bf8555fd2375c0fbb99a6f8bd53
ms.sourcegitcommit: 7e87671fea9a5f36ca516616fe3b40b537f428d2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/12/2018
ms.locfileid: "35358416"
---
# <a name="use-streaming-in-aspnet-core-signalr"></a>Use o SignalR do ASP.NET Core de streaming

Por [Brennan Conroy](https://github.com/BrennanConroy)

O SignalR do ASP.NET Core dá suporte a streaming valores de retorno dos métodos de servidor. Isso é útil para cenários onde fragmentos de dados ficará no decorrer do tempo. Quando um valor de retorno é transmitido para o cliente, cada fragmento é enviado ao cliente assim que ele se torna disponível, em vez de aguardar por todos os dados fiquem disponíveis.

[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))

## <a name="set-up-the-hub"></a>Configurar o hub

Um método de hub automaticamente se torna um método de hub streaming quando ele retorna um `ChannelReader<T>` ou `Task<ChannelReader<T>>`. Abaixo está um exemplo que mostra os fundamentos de fluxo de dados para o cliente. Sempre que um objeto é gravado o `ChannelReader` esse objeto é enviado imediatamente para o cliente. No final, o `ChannelReader` é concluída para dizer ao cliente o fluxo está fechado.

> [!NOTE]
> Gravar o `ChannelReader` em um thread em segundo plano e retorne o `ChannelReader` assim que possível. Outras chamadas de hub serão bloqueadas até que um `ChannelReader` é retornado.

[!code-csharp[Streaming hub method](streaming/sample/hubs/streamhub.cs?range=10-34)]

## <a name="net-client"></a>Cliente .NET

O `StreamAsChannelAsync` método `HubConnection` é usado para chamar um método de transmissão. Passe o nome do método de hub e argumentos definidos no método de hub para `StreamAsChannelAsync`. O parâmetro genérico em `StreamAsChannelAsync<T>` Especifica o tipo de objetos retornados pelo método de transmissão. Um `ChannelReader<T>` é retornada da invocação de fluxo e representa o fluxo no cliente. Para ler dados, um padrão comum é para loop sobre `WaitToReadAsync` e chamar `TryRead` quando os dados estão disponíveis. O loop será encerrado quando o fluxo foi fechado pelo servidor ou o token de cancelamento passados para `StreamAsChannelAsync` é cancelada.

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

## <a name="javascript-client"></a>Cliente JavaScript

Os clientes JavaScript chamar métodos de streaming em hubs usando `connection.stream`. O `stream` método aceita dois argumentos:

* O nome do método de hub. No exemplo a seguir, o nome do método de hub é `Counter`.
* Argumentos definidos no método de hub. No exemplo a seguir, os argumentos são: uma contagem do número de itens de fluxo para receber e o atraso entre itens de fluxo.

`connection.stream` Retorna um `IStreamResult` que contém um `subscribe` método. Passar um `IStreamSubscriber` para `subscribe` e defina o `next`, `error`, e `complete` retornos de chamada para receber as notificações do `stream` invocação.

[!code-javascript[Streaming javascript](streaming/sample/wwwroot/js/stream.js?range=19-36)]

Para encerrar o fluxo da chamada de cliente a `dispose` método no `ISubscription` que é retornado o `subscribe` método.

## <a name="related-resources"></a>Recursos relacionados

* [Hubs](xref:signalr/hubs)
* [Cliente .NET](xref:signalr/dotnet-client)
* [Cliente JavaScript](xref:signalr/javascript-client)
* [Publicar no Azure](xref:signalr/publish-to-azure-web-app)
---
title: Cliente de .NET Core ASP.NET SignalR
author: rachelappel
description: Informações sobre o cliente do ASP.NET Core SignalR .NET
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 05/18/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/dotnet-client
ms.openlocfilehash: 412d2362575789f1fb4792940df6d3dd24dbdd5a
ms.sourcegitcommit: 300a1127957dcdbce1b6ad79a7b9dc676f571510
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/23/2018
---
# <a name="aspnet-core-signalr-net-client"></a><span data-ttu-id="8c302-103">Cliente de .NET Core ASP.NET SignalR</span><span class="sxs-lookup"><span data-stu-id="8c302-103">ASP.NET Core SignalR .NET Client</span></span>

<span data-ttu-id="8c302-104">Por [Rachel Appel](http://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="8c302-104">By [Rachel Appel](http://twitter.com/rachelappel)</span></span>

<span data-ttu-id="8c302-105">O cliente do ASP.NET Core SignalR .NET pode ser usado por aplicativos Xamarin, o WPF, o Windows Forms, o Console e o .NET Core.</span><span class="sxs-lookup"><span data-stu-id="8c302-105">The ASP.NET Core SignalR .NET client can be used by Xamarin, WPF, Windows Forms, Console, and .NET Core apps.</span></span> <span data-ttu-id="8c302-106">Como o [cliente JavaScript](xref:signalr/javascript-client), o cliente .NET permite que você receber e enviar e receber mensagens para um hub em tempo real.</span><span class="sxs-lookup"><span data-stu-id="8c302-106">Like the [JavaScript client](xref:signalr/javascript-client), the .NET client enables you to receive and send and receive messages to a hub in real time.</span></span>

<span data-ttu-id="8c302-107">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/dotnet-client/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="8c302-107">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/dotnet-client/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="8c302-108">O exemplo de código neste artigo é um aplicativo do WPF que usa o cliente do ASP.NET Core SignalR .NET.</span><span class="sxs-lookup"><span data-stu-id="8c302-108">The code sample in this article is a WPF app that uses the ASP.NET Core SignalR .NET client.</span></span>

## <a name="setup-client"></a><span data-ttu-id="8c302-109">Instalar o cliente</span><span class="sxs-lookup"><span data-stu-id="8c302-109">Setup client</span></span>

<span data-ttu-id="8c302-110">O `Microsoft.AspNetCore.SignalR.Client` do pacote é necessário para que os clientes .NET conectar-se a hubs de SignalR.</span><span class="sxs-lookup"><span data-stu-id="8c302-110">The `Microsoft.AspNetCore.SignalR.Client` package is needed for .NET clients to connect to SignalR hubs.</span></span> <span data-ttu-id="8c302-111">Para instalar a biblioteca de cliente, execute o seguinte comando **Package Manager Console** janela:</span><span class="sxs-lookup"><span data-stu-id="8c302-111">To install the client library, run the following command in the **Package Manager Console** window:</span></span>

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="8c302-112">Conectar a um hub</span><span class="sxs-lookup"><span data-stu-id="8c302-112">Connect to a hub</span></span>

<span data-ttu-id="8c302-113">Para estabelecer uma conexão, criar um `HubConnectionBuilder` e chame `Build`.</span><span class="sxs-lookup"><span data-stu-id="8c302-113">To establish a connection, create a `HubConnectionBuilder` and call `Build`.</span></span> <span data-ttu-id="8c302-114">A URL do hub, protocolo, tipo de transporte, nível de log, cabeçalhos e outras opções podem ser configuradas durante a criação de uma conexão.</span><span class="sxs-lookup"><span data-stu-id="8c302-114">The hub URL, protocol, transport type, log level, headers, and other options can be configured while building a connection.</span></span> <span data-ttu-id="8c302-115">Configure as opções necessárias, inserindo um do `HubConnectionBuilder` métodos em `Build`.</span><span class="sxs-lookup"><span data-stu-id="8c302-115">Configure any required options by inserting any of the `HubConnectionBuilder` methods into `Build`.</span></span> <span data-ttu-id="8c302-116">Iniciar a conexão com `StartAsync`.</span><span class="sxs-lookup"><span data-stu-id="8c302-116">Start the connection with `StartAsync`.</span></span>

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?highlight=15-17,33)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="8c302-117">Chamar métodos de hub do cliente</span><span class="sxs-lookup"><span data-stu-id="8c302-117">Call hub methods from client</span></span>

<span data-ttu-id="8c302-118">`InvokeAsync` chama os métodos no hub.</span><span class="sxs-lookup"><span data-stu-id="8c302-118">`InvokeAsync` calls methods on the hub.</span></span> <span data-ttu-id="8c302-119">Passe o nome do método de hub e argumentos definidos no método de hub para `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="8c302-119">Pass the hub method name and any arguments defined in the hub method to `InvokeAsync`.</span></span> <span data-ttu-id="8c302-120">SignalR é assíncrono, portanto, use `async` e `await` ao fazer as chamadas.</span><span class="sxs-lookup"><span data-stu-id="8c302-120">SignalR is asynchronous, so use `async` and `await` when making the calls.</span></span>

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=48-49)]

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="8c302-121">Chamar métodos de cliente do hub</span><span class="sxs-lookup"><span data-stu-id="8c302-121">Call client methods from hub</span></span>

<span data-ttu-id="8c302-122">Definir métodos hub chamadas usando `connection.On` após a criação, mas antes de iniciar a conexão.</span><span class="sxs-lookup"><span data-stu-id="8c302-122">Define methods the hub calls using `connection.On` after building, but before starting the connection.</span></span>

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=22-29)]

<span data-ttu-id="8c302-123">O código anterior no `connection.On` é executado quando o código do lado do servidor chamá-lo usando o `SendAsync` método.</span><span class="sxs-lookup"><span data-stu-id="8c302-123">The preceding code in `connection.On` runs when server-side code calls it using the `SendAsync` method.</span></span>

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?range=8-11)]

## <a name="error-handling-and-logging"></a><span data-ttu-id="8c302-124">Registro em log e tratamento de erros</span><span class="sxs-lookup"><span data-stu-id="8c302-124">Error handling and logging</span></span>

<span data-ttu-id="8c302-125">Tratar erros com uma instrução try-catch.</span><span class="sxs-lookup"><span data-stu-id="8c302-125">Handle errors with a try-catch statement.</span></span> <span data-ttu-id="8c302-126">Inspecione o `Exception` objeto para determinar a ação adequada ocorre após a ocorrência de um erro.</span><span class="sxs-lookup"><span data-stu-id="8c302-126">Inspect the `Exception` object to determine the proper action to take after an error occurs.</span></span>

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=46-54)]

## <a name="additional-resources"></a><span data-ttu-id="8c302-127">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="8c302-127">Additional resources</span></span>

* [<span data-ttu-id="8c302-128">Hubs</span><span class="sxs-lookup"><span data-stu-id="8c302-128">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="8c302-129">Cliente JavaScript</span><span class="sxs-lookup"><span data-stu-id="8c302-129">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="8c302-130">Publicar no Azure</span><span class="sxs-lookup"><span data-stu-id="8c302-130">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
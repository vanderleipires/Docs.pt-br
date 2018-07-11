---
title: Cliente de .NET do SignalR do ASP.NET Core
author: rachelappel
description: Informações sobre o cliente do .NET Core ASP.NET SignalR
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 05/29/2018
uid: signalr/dotnet-client
ms.openlocfilehash: 35dc1d3abf0d35e17d1835ec462f8cc4feb728eb
ms.sourcegitcommit: 661d30492d5ef7bbca4f7e709f40d8f3309d2dac
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37938248"
---
# <a name="aspnet-core-signalr-net-client"></a><span data-ttu-id="4ca2b-103">Cliente de .NET do SignalR do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4ca2b-103">ASP.NET Core SignalR .NET Client</span></span>

<span data-ttu-id="4ca2b-104">Por [Rachel Appel](http://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="4ca2b-104">By [Rachel Appel](http://twitter.com/rachelappel)</span></span>

<span data-ttu-id="4ca2b-105">O cliente .NET de SignalR do ASP.NET Core pode ser usado por aplicativos do Xamarin, WPF, Windows Forms, Console e .NET Core.</span><span class="sxs-lookup"><span data-stu-id="4ca2b-105">The ASP.NET Core SignalR .NET client can be used by Xamarin, WPF, Windows Forms, Console, and .NET Core apps.</span></span> <span data-ttu-id="4ca2b-106">Como o [cliente JavaScript](xref:signalr/javascript-client), o cliente .NET permite que você receber e enviar e receber mensagens para um hub em tempo real.</span><span class="sxs-lookup"><span data-stu-id="4ca2b-106">Like the [JavaScript client](xref:signalr/javascript-client), the .NET client enables you to receive and send and receive messages to a hub in real time.</span></span>

<span data-ttu-id="4ca2b-107">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/dotnet-client/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="4ca2b-107">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/dotnet-client/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="4ca2b-108">O exemplo de código neste artigo é um aplicativo do WPF que usa o cliente .NET de SignalR do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4ca2b-108">The code sample in this article is a WPF app that uses the ASP.NET Core SignalR .NET client.</span></span>

## <a name="install-the-signalr-net-client-package"></a><span data-ttu-id="4ca2b-109">Instalar o pacote de cliente .NET do SignalR</span><span class="sxs-lookup"><span data-stu-id="4ca2b-109">Install the SignalR .NET client package</span></span>

<span data-ttu-id="4ca2b-110">O `Microsoft.AspNetCore.SignalR.Client` pacote é necessário para clientes .NET conectar-se aos hubs de SignalR.</span><span class="sxs-lookup"><span data-stu-id="4ca2b-110">The `Microsoft.AspNetCore.SignalR.Client` package is needed for .NET clients to connect to SignalR hubs.</span></span> <span data-ttu-id="4ca2b-111">Para instalar a biblioteca de cliente, execute o seguinte comando na **Package Manager Console** janela:</span><span class="sxs-lookup"><span data-stu-id="4ca2b-111">To install the client library, run the following command in the **Package Manager Console** window:</span></span>

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="4ca2b-112">Conectar a um hub</span><span class="sxs-lookup"><span data-stu-id="4ca2b-112">Connect to a hub</span></span>

<span data-ttu-id="4ca2b-113">Para estabelecer uma conexão, cria uma `HubConnectionBuilder` e chamar `Build`.</span><span class="sxs-lookup"><span data-stu-id="4ca2b-113">To establish a connection, create a `HubConnectionBuilder` and call `Build`.</span></span> <span data-ttu-id="4ca2b-114">A URL do hub, protocolo, o tipo de transporte, nível de log, cabeçalhos e outras opções podem ser configuradas durante a criação de uma conexão.</span><span class="sxs-lookup"><span data-stu-id="4ca2b-114">The hub URL, protocol, transport type, log level, headers, and other options can be configured while building a connection.</span></span> <span data-ttu-id="4ca2b-115">Configurar as opções necessárias, inserindo qualquer um dos `HubConnectionBuilder` métodos em `Build`.</span><span class="sxs-lookup"><span data-stu-id="4ca2b-115">Configure any required options by inserting any of the `HubConnectionBuilder` methods into `Build`.</span></span> <span data-ttu-id="4ca2b-116">Iniciar a conexão com `StartAsync`.</span><span class="sxs-lookup"><span data-stu-id="4ca2b-116">Start the connection with `StartAsync`.</span></span>

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?highlight=15-17,33)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="4ca2b-117">Chamar métodos de hub do cliente</span><span class="sxs-lookup"><span data-stu-id="4ca2b-117">Call hub methods from client</span></span>

<span data-ttu-id="4ca2b-118">`InvokeAsync` chama métodos no hub.</span><span class="sxs-lookup"><span data-stu-id="4ca2b-118">`InvokeAsync` calls methods on the hub.</span></span> <span data-ttu-id="4ca2b-119">Passe o nome do método de hub e quaisquer argumentos definidos no método de hub para `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="4ca2b-119">Pass the hub method name and any arguments defined in the hub method to `InvokeAsync`.</span></span> <span data-ttu-id="4ca2b-120">O SignalR é assíncrono, portanto, use `async` e `await` ao fazer as chamadas.</span><span class="sxs-lookup"><span data-stu-id="4ca2b-120">SignalR is asynchronous, so use `async` and `await` when making the calls.</span></span>

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=48-49)]

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="4ca2b-121">Chamar métodos de cliente do hub</span><span class="sxs-lookup"><span data-stu-id="4ca2b-121">Call client methods from hub</span></span>

<span data-ttu-id="4ca2b-122">Definir métodos de hub de chamadas usando `connection.On` depois de criar, mas antes de iniciar a conexão.</span><span class="sxs-lookup"><span data-stu-id="4ca2b-122">Define methods the hub calls using `connection.On` after building, but before starting the connection.</span></span>

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=22-29)]

<span data-ttu-id="4ca2b-123">O código anterior no `connection.On` é executado quando o código do lado do servidor chama-o usando o `SendAsync` método.</span><span class="sxs-lookup"><span data-stu-id="4ca2b-123">The preceding code in `connection.On` runs when server-side code calls it using the `SendAsync` method.</span></span>

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?range=8-11)]

## <a name="error-handling-and-logging"></a><span data-ttu-id="4ca2b-124">Registro em log e tratamento de erros</span><span class="sxs-lookup"><span data-stu-id="4ca2b-124">Error handling and logging</span></span>

<span data-ttu-id="4ca2b-125">Tratar erros com uma instrução try-catch.</span><span class="sxs-lookup"><span data-stu-id="4ca2b-125">Handle errors with a try-catch statement.</span></span> <span data-ttu-id="4ca2b-126">Inspecione o `Exception` objeto para determinar a ação apropriada a tomar depois de ocorrer um erro.</span><span class="sxs-lookup"><span data-stu-id="4ca2b-126">Inspect the `Exception` object to determine the proper action to take after an error occurs.</span></span>

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=46-54)]

## <a name="additional-resources"></a><span data-ttu-id="4ca2b-127">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="4ca2b-127">Additional resources</span></span>

* [<span data-ttu-id="4ca2b-128">Hubs</span><span class="sxs-lookup"><span data-stu-id="4ca2b-128">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="4ca2b-129">Cliente JavaScript</span><span class="sxs-lookup"><span data-stu-id="4ca2b-129">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="4ca2b-130">Publicar no Azure</span><span class="sxs-lookup"><span data-stu-id="4ca2b-130">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)

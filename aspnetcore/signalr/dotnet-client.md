---
title: Cliente de .NET do SignalR do ASP.NET Core
author: tdykstra
description: Informações sobre o cliente do .NET Core ASP.NET SignalR
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/07/2018
uid: signalr/dotnet-client
ms.openlocfilehash: 970888a410b2486a20f98ce77a8674f8ec357f50
ms.sourcegitcommit: 028ad28c546de706ace98066c76774de33e4ad20
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/08/2018
ms.locfileid: "39655246"
---
# <a name="aspnet-core-signalr-net-client"></a><span data-ttu-id="6882d-103">Cliente de .NET do SignalR do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6882d-103">ASP.NET Core SignalR .NET Client</span></span>

<span data-ttu-id="6882d-104">Por [Rachel Appel](http://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="6882d-104">By [Rachel Appel](http://twitter.com/rachelappel)</span></span>

<span data-ttu-id="6882d-105">O cliente .NET de SignalR do ASP.NET Core pode ser usado por aplicativos do Xamarin, WPF, Windows Forms, Console e .NET Core.</span><span class="sxs-lookup"><span data-stu-id="6882d-105">The ASP.NET Core SignalR .NET client can be used by Xamarin, WPF, Windows Forms, Console, and .NET Core apps.</span></span> <span data-ttu-id="6882d-106">Como o [cliente JavaScript](xref:signalr/javascript-client), o cliente .NET permite que você receber e enviar e receber mensagens para um hub em tempo real.</span><span class="sxs-lookup"><span data-stu-id="6882d-106">Like the [JavaScript client](xref:signalr/javascript-client), the .NET client enables you to receive and send and receive messages to a hub in real time.</span></span>

> [!NOTE]
> <span data-ttu-id="6882d-107">Xamarin tem requisitos especiais para a versão do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6882d-107">Xamarin has special requirements for Visual Studio version.</span></span> <span data-ttu-id="6882d-108">Para obter mais informações, consulte [cliente SignalR 2.1.1 no Xamarin](https://github.com/aspnet/Announcements/issues/305).</span><span class="sxs-lookup"><span data-stu-id="6882d-108">For more information, see [SignalR Client 2.1.1 in Xamarin](https://github.com/aspnet/Announcements/issues/305).</span></span>

<span data-ttu-id="6882d-109">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/dotnet-client/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="6882d-109">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/dotnet-client/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="6882d-110">O exemplo de código neste artigo é um aplicativo do WPF que usa o cliente .NET de SignalR do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6882d-110">The code sample in this article is a WPF app that uses the ASP.NET Core SignalR .NET client.</span></span>

## <a name="install-the-signalr-net-client-package"></a><span data-ttu-id="6882d-111">Instalar o pacote de cliente .NET do SignalR</span><span class="sxs-lookup"><span data-stu-id="6882d-111">Install the SignalR .NET client package</span></span>

<span data-ttu-id="6882d-112">O `Microsoft.AspNetCore.SignalR.Client` pacote é necessário para clientes .NET conectar-se aos hubs de SignalR.</span><span class="sxs-lookup"><span data-stu-id="6882d-112">The `Microsoft.AspNetCore.SignalR.Client` package is needed for .NET clients to connect to SignalR hubs.</span></span> <span data-ttu-id="6882d-113">Para instalar a biblioteca de cliente, execute o seguinte comando na **Package Manager Console** janela:</span><span class="sxs-lookup"><span data-stu-id="6882d-113">To install the client library, run the following command in the **Package Manager Console** window:</span></span>

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="6882d-114">Conectar a um hub</span><span class="sxs-lookup"><span data-stu-id="6882d-114">Connect to a hub</span></span>

<span data-ttu-id="6882d-115">Para estabelecer uma conexão, cria uma `HubConnectionBuilder` e chamar `Build`.</span><span class="sxs-lookup"><span data-stu-id="6882d-115">To establish a connection, create a `HubConnectionBuilder` and call `Build`.</span></span> <span data-ttu-id="6882d-116">A URL do hub, protocolo, o tipo de transporte, nível de log, cabeçalhos e outras opções podem ser configuradas durante a criação de uma conexão.</span><span class="sxs-lookup"><span data-stu-id="6882d-116">The hub URL, protocol, transport type, log level, headers, and other options can be configured while building a connection.</span></span> <span data-ttu-id="6882d-117">Configurar as opções necessárias, inserindo qualquer um dos `HubConnectionBuilder` métodos em `Build`.</span><span class="sxs-lookup"><span data-stu-id="6882d-117">Configure any required options by inserting any of the `HubConnectionBuilder` methods into `Build`.</span></span> <span data-ttu-id="6882d-118">Iniciar a conexão com `StartAsync`.</span><span class="sxs-lookup"><span data-stu-id="6882d-118">Start the connection with `StartAsync`.</span></span>

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?highlight=15-17,33)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="6882d-119">Chamar métodos de hub do cliente</span><span class="sxs-lookup"><span data-stu-id="6882d-119">Call hub methods from client</span></span>

<span data-ttu-id="6882d-120">`InvokeAsync` chama métodos no hub.</span><span class="sxs-lookup"><span data-stu-id="6882d-120">`InvokeAsync` calls methods on the hub.</span></span> <span data-ttu-id="6882d-121">Passe o nome do método de hub e quaisquer argumentos definidos no método de hub para `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="6882d-121">Pass the hub method name and any arguments defined in the hub method to `InvokeAsync`.</span></span> <span data-ttu-id="6882d-122">O SignalR é assíncrono, portanto, use `async` e `await` ao fazer as chamadas.</span><span class="sxs-lookup"><span data-stu-id="6882d-122">SignalR is asynchronous, so use `async` and `await` when making the calls.</span></span>

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=48-49)]

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="6882d-123">Chamar métodos de cliente do hub</span><span class="sxs-lookup"><span data-stu-id="6882d-123">Call client methods from hub</span></span>

<span data-ttu-id="6882d-124">Definir métodos de hub de chamadas usando `connection.On` depois de criar, mas antes de iniciar a conexão.</span><span class="sxs-lookup"><span data-stu-id="6882d-124">Define methods the hub calls using `connection.On` after building, but before starting the connection.</span></span>

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=22-29)]

<span data-ttu-id="6882d-125">O código anterior no `connection.On` é executado quando o código do lado do servidor chama-o usando o `SendAsync` método.</span><span class="sxs-lookup"><span data-stu-id="6882d-125">The preceding code in `connection.On` runs when server-side code calls it using the `SendAsync` method.</span></span>

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?range=8-11)]

## <a name="error-handling-and-logging"></a><span data-ttu-id="6882d-126">Registro em log e tratamento de erros</span><span class="sxs-lookup"><span data-stu-id="6882d-126">Error handling and logging</span></span>

<span data-ttu-id="6882d-127">Tratar erros com uma instrução try-catch.</span><span class="sxs-lookup"><span data-stu-id="6882d-127">Handle errors with a try-catch statement.</span></span> <span data-ttu-id="6882d-128">Inspecione o `Exception` objeto para determinar a ação apropriada a tomar depois de ocorrer um erro.</span><span class="sxs-lookup"><span data-stu-id="6882d-128">Inspect the `Exception` object to determine the proper action to take after an error occurs.</span></span>

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=46-54)]

## <a name="additional-resources"></a><span data-ttu-id="6882d-129">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="6882d-129">Additional resources</span></span>

* [<span data-ttu-id="6882d-130">Hubs</span><span class="sxs-lookup"><span data-stu-id="6882d-130">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="6882d-131">Cliente JavaScript</span><span class="sxs-lookup"><span data-stu-id="6882d-131">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="6882d-132">Publicar no Azure</span><span class="sxs-lookup"><span data-stu-id="6882d-132">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)

---
title: Cliente de .NET do SignalR do ASP.NET Core
author: tdykstra
description: Informações sobre o cliente do .NET Core ASP.NET SignalR
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/10/2018
uid: signalr/dotnet-client
ms.openlocfilehash: 205ca8ca228dcc2cc77f7e9b6431943851a3b152
ms.sourcegitcommit: 1a2fc47fb5d3da0f2a3c3269613ab20eb3b0da2c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/11/2018
ms.locfileid: "44373313"
---
# <a name="aspnet-core-signalr-net-client"></a><span data-ttu-id="38009-103">Cliente de .NET do SignalR do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="38009-103">ASP.NET Core SignalR .NET Client</span></span>

<span data-ttu-id="38009-104">Por [Rachel Appel](http://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="38009-104">By [Rachel Appel](http://twitter.com/rachelappel)</span></span>

<span data-ttu-id="38009-105">A biblioteca de cliente .NET de SignalR do ASP.NET Core permite que você se comunicar com os hubs de SignalR em aplicativos .NET.</span><span class="sxs-lookup"><span data-stu-id="38009-105">The ASP.NET Core SignalR .NET client library lets you communicate with SignalR hubs from .NET apps.</span></span>

> [!NOTE]
> <span data-ttu-id="38009-106">Xamarin tem requisitos especiais para a versão do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="38009-106">Xamarin has special requirements for Visual Studio version.</span></span> <span data-ttu-id="38009-107">Para obter mais informações, consulte [cliente SignalR 2.1.1 no Xamarin](https://github.com/aspnet/Announcements/issues/305).</span><span class="sxs-lookup"><span data-stu-id="38009-107">For more information, see [SignalR Client 2.1.1 in Xamarin](https://github.com/aspnet/Announcements/issues/305).</span></span>

<span data-ttu-id="38009-108">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="38009-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="38009-109">O exemplo de código neste artigo é um aplicativo do WPF que usa o cliente .NET de SignalR do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="38009-109">The code sample in this article is a WPF app that uses the ASP.NET Core SignalR .NET client.</span></span>

## <a name="install-the-signalr-net-client-package"></a><span data-ttu-id="38009-110">Instalar o pacote de cliente .NET do SignalR</span><span class="sxs-lookup"><span data-stu-id="38009-110">Install the SignalR .NET client package</span></span>

<span data-ttu-id="38009-111">O `Microsoft.AspNetCore.SignalR.Client` pacote é necessário para clientes .NET conectar-se aos hubs de SignalR.</span><span class="sxs-lookup"><span data-stu-id="38009-111">The `Microsoft.AspNetCore.SignalR.Client` package is needed for .NET clients to connect to SignalR hubs.</span></span> <span data-ttu-id="38009-112">Para instalar a biblioteca de cliente, execute o seguinte comando na **Package Manager Console** janela:</span><span class="sxs-lookup"><span data-stu-id="38009-112">To install the client library, run the following command in the **Package Manager Console** window:</span></span>

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="38009-113">Conectar a um hub</span><span class="sxs-lookup"><span data-stu-id="38009-113">Connect to a hub</span></span>

<span data-ttu-id="38009-114">Para estabelecer uma conexão, cria uma `HubConnectionBuilder` e chamar `Build`.</span><span class="sxs-lookup"><span data-stu-id="38009-114">To establish a connection, create a `HubConnectionBuilder` and call `Build`.</span></span> <span data-ttu-id="38009-115">A URL do hub, protocolo, o tipo de transporte, nível de log, cabeçalhos e outras opções podem ser configuradas durante a criação de uma conexão.</span><span class="sxs-lookup"><span data-stu-id="38009-115">The hub URL, protocol, transport type, log level, headers, and other options can be configured while building a connection.</span></span> <span data-ttu-id="38009-116">Configurar as opções necessárias, inserindo qualquer um dos `HubConnectionBuilder` métodos em `Build`.</span><span class="sxs-lookup"><span data-stu-id="38009-116">Configure any required options by inserting any of the `HubConnectionBuilder` methods into `Build`.</span></span> <span data-ttu-id="38009-117">Iniciar a conexão com `StartAsync`.</span><span class="sxs-lookup"><span data-stu-id="38009-117">Start the connection with `StartAsync`.</span></span>

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_MainWindowClass&highlight=14-16,32)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="38009-118">Chamar métodos de hub do cliente</span><span class="sxs-lookup"><span data-stu-id="38009-118">Call hub methods from client</span></span>

<span data-ttu-id="38009-119">`InvokeAsync` chama métodos no hub.</span><span class="sxs-lookup"><span data-stu-id="38009-119">`InvokeAsync` calls methods on the hub.</span></span> <span data-ttu-id="38009-120">Passe o nome do método de hub e quaisquer argumentos definidos no método de hub para `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="38009-120">Pass the hub method name and any arguments defined in the hub method to `InvokeAsync`.</span></span> <span data-ttu-id="38009-121">O SignalR é assíncrono, portanto, use `async` e `await` ao fazer as chamadas.</span><span class="sxs-lookup"><span data-stu-id="38009-121">SignalR is asynchronous, so use `async` and `await` when making the calls.</span></span>

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_InvokeAsync)]

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="38009-122">Chamar métodos de cliente do hub</span><span class="sxs-lookup"><span data-stu-id="38009-122">Call client methods from hub</span></span>

<span data-ttu-id="38009-123">Definir métodos de hub de chamadas usando `connection.On` depois de criar, mas antes de iniciar a conexão.</span><span class="sxs-lookup"><span data-stu-id="38009-123">Define methods the hub calls using `connection.On` after building, but before starting the connection.</span></span>

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ConnectionOn)]

<span data-ttu-id="38009-124">O código anterior no `connection.On` é executado quando o código do lado do servidor chama-o usando o `SendAsync` método.</span><span class="sxs-lookup"><span data-stu-id="38009-124">The preceding code in `connection.On` runs when server-side code calls it using the `SendAsync` method.</span></span>

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?name=snippet_SendMessage)]

## <a name="error-handling-and-logging"></a><span data-ttu-id="38009-125">Registro em log e tratamento de erros</span><span class="sxs-lookup"><span data-stu-id="38009-125">Error handling and logging</span></span>

<span data-ttu-id="38009-126">Tratar erros com uma instrução try-catch.</span><span class="sxs-lookup"><span data-stu-id="38009-126">Handle errors with a try-catch statement.</span></span> <span data-ttu-id="38009-127">Inspecione o `Exception` objeto para determinar a ação apropriada a tomar depois de ocorrer um erro.</span><span class="sxs-lookup"><span data-stu-id="38009-127">Inspect the `Exception` object to determine the proper action to take after an error occurs.</span></span>

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ErrorHandling)]

## <a name="additional-resources"></a><span data-ttu-id="38009-128">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="38009-128">Additional resources</span></span>

* [<span data-ttu-id="38009-129">Hubs</span><span class="sxs-lookup"><span data-stu-id="38009-129">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="38009-130">Cliente JavaScript</span><span class="sxs-lookup"><span data-stu-id="38009-130">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="38009-131">Publicar no Azure</span><span class="sxs-lookup"><span data-stu-id="38009-131">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)

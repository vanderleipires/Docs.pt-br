---
title: Cliente de .NET do SignalR do ASP.NET Core
author: tdykstra
description: Informações sobre o cliente do .NET Core ASP.NET SignalR
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/10/2018
uid: signalr/dotnet-client
ms.openlocfilehash: ef84ede2ed45ddc3b64d4ce8f5bd0018a681faf6
ms.sourcegitcommit: 4db337bd47d70c06fff91000c58bc048a491ccec
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/12/2018
ms.locfileid: "44749315"
---
# <a name="aspnet-core-signalr-net-client"></a><span data-ttu-id="394c2-103">Cliente de .NET do SignalR do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="394c2-103">ASP.NET Core SignalR .NET Client</span></span>

<span data-ttu-id="394c2-104">A biblioteca de cliente .NET de SignalR do ASP.NET Core permite que você se comunicar com os hubs de SignalR em aplicativos .NET.</span><span class="sxs-lookup"><span data-stu-id="394c2-104">The ASP.NET Core SignalR .NET client library lets you communicate with SignalR hubs from .NET apps.</span></span>

> [!NOTE]
> <span data-ttu-id="394c2-105">Xamarin tem requisitos especiais para a versão do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="394c2-105">Xamarin has special requirements for Visual Studio version.</span></span> <span data-ttu-id="394c2-106">Para obter mais informações, consulte [cliente SignalR 2.1.1 no Xamarin](https://github.com/aspnet/Announcements/issues/305).</span><span class="sxs-lookup"><span data-stu-id="394c2-106">For more information, see [SignalR Client 2.1.1 in Xamarin](https://github.com/aspnet/Announcements/issues/305).</span></span>

<span data-ttu-id="394c2-107">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="394c2-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="394c2-108">O exemplo de código neste artigo é um aplicativo do WPF que usa o cliente .NET de SignalR do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="394c2-108">The code sample in this article is a WPF app that uses the ASP.NET Core SignalR .NET client.</span></span>

## <a name="install-the-signalr-net-client-package"></a><span data-ttu-id="394c2-109">Instalar o pacote de cliente .NET do SignalR</span><span class="sxs-lookup"><span data-stu-id="394c2-109">Install the SignalR .NET client package</span></span>

<span data-ttu-id="394c2-110">O `Microsoft.AspNetCore.SignalR.Client` pacote é necessário para clientes .NET conectar-se aos hubs de SignalR.</span><span class="sxs-lookup"><span data-stu-id="394c2-110">The `Microsoft.AspNetCore.SignalR.Client` package is needed for .NET clients to connect to SignalR hubs.</span></span> <span data-ttu-id="394c2-111">Para instalar a biblioteca de cliente, execute o seguinte comando na **Package Manager Console** janela:</span><span class="sxs-lookup"><span data-stu-id="394c2-111">To install the client library, run the following command in the **Package Manager Console** window:</span></span>

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="394c2-112">Conectar a um hub</span><span class="sxs-lookup"><span data-stu-id="394c2-112">Connect to a hub</span></span>

<span data-ttu-id="394c2-113">Para estabelecer uma conexão, cria uma `HubConnectionBuilder` e chamar `Build`.</span><span class="sxs-lookup"><span data-stu-id="394c2-113">To establish a connection, create a `HubConnectionBuilder` and call `Build`.</span></span> <span data-ttu-id="394c2-114">A URL do hub, protocolo, o tipo de transporte, nível de log, cabeçalhos e outras opções podem ser configuradas durante a criação de uma conexão.</span><span class="sxs-lookup"><span data-stu-id="394c2-114">The hub URL, protocol, transport type, log level, headers, and other options can be configured while building a connection.</span></span> <span data-ttu-id="394c2-115">Configurar as opções necessárias, inserindo qualquer um dos `HubConnectionBuilder` métodos em `Build`.</span><span class="sxs-lookup"><span data-stu-id="394c2-115">Configure any required options by inserting any of the `HubConnectionBuilder` methods into `Build`.</span></span> <span data-ttu-id="394c2-116">Iniciar a conexão com `StartAsync`.</span><span class="sxs-lookup"><span data-stu-id="394c2-116">Start the connection with `StartAsync`.</span></span>

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_MainWindowClass&highlight=15-17,39)]

## <a name="handle-lost-connection"></a><span data-ttu-id="394c2-117">Lidar com a conexão perdida</span><span class="sxs-lookup"><span data-stu-id="394c2-117">Handle lost connection</span></span>

<span data-ttu-id="394c2-118">Use o <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> eventos para responder a uma conexão perdida.</span><span class="sxs-lookup"><span data-stu-id="394c2-118">Use the <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> event to respond to a lost connection.</span></span> <span data-ttu-id="394c2-119">Por exemplo, você talvez queira automatizar a reconexão.</span><span class="sxs-lookup"><span data-stu-id="394c2-119">For example, you might want to automate reconnection.</span></span>

<span data-ttu-id="394c2-120">O `Closed` evento requer um delegado que retorna um `Task`, que permite que o código assíncrono executar sem usar `async void`.</span><span class="sxs-lookup"><span data-stu-id="394c2-120">The `Closed` event requires a delegate that returns a `Task`, which allows async code to run without using `async void`.</span></span> <span data-ttu-id="394c2-121">Para satisfazer a assinatura do delegado em um `Closed` manipulador de eventos que é executado de forma síncrona, retorna `Task.CompletedTask`:</span><span class="sxs-lookup"><span data-stu-id="394c2-121">To satisfy the delegate signature in a `Closed` event handler that runs synchronously, return `Task.CompletedTask`:</span></span>

```csharp
connection.Closed += (error) => {
    // Do your close logic.
    return Task.CompletedTask;
};
```

<span data-ttu-id="394c2-122">O principal motivo para o suporte assíncrono é portanto, você pode reiniciar a conexão.</span><span class="sxs-lookup"><span data-stu-id="394c2-122">The main reason for the async support is so you can restart the connection.</span></span> <span data-ttu-id="394c2-123">Iniciar uma conexão é uma ação assíncrona.</span><span class="sxs-lookup"><span data-stu-id="394c2-123">Starting a connection is an async action.</span></span>

<span data-ttu-id="394c2-124">Em um `Closed` manipulador que reinicia a conexão, considere aguardar algum atraso aleatório evitar sobrecarregar o servidor, conforme mostrado no exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="394c2-124">In a `Closed` handler that restarts the connection, consider waiting for some random delay to prevent overloading the server, as shown in the following example:</span></span>

[!code-csharp[Use Closed event handler to automate reconnection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ClosedRestart)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="394c2-125">Chamar métodos de hub do cliente</span><span class="sxs-lookup"><span data-stu-id="394c2-125">Call hub methods from client</span></span>

<span data-ttu-id="394c2-126">`InvokeAsync` chama métodos no hub.</span><span class="sxs-lookup"><span data-stu-id="394c2-126">`InvokeAsync` calls methods on the hub.</span></span> <span data-ttu-id="394c2-127">Passe o nome do método de hub e quaisquer argumentos definidos no método de hub para `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="394c2-127">Pass the hub method name and any arguments defined in the hub method to `InvokeAsync`.</span></span> <span data-ttu-id="394c2-128">O SignalR é assíncrono, portanto, use `async` e `await` ao fazer as chamadas.</span><span class="sxs-lookup"><span data-stu-id="394c2-128">SignalR is asynchronous, so use `async` and `await` when making the calls.</span></span>

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_InvokeAsync)]

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="394c2-129">Chamar métodos de cliente do hub</span><span class="sxs-lookup"><span data-stu-id="394c2-129">Call client methods from hub</span></span>

<span data-ttu-id="394c2-130">Definir métodos de hub de chamadas usando `connection.On` depois de criar, mas antes de iniciar a conexão.</span><span class="sxs-lookup"><span data-stu-id="394c2-130">Define methods the hub calls using `connection.On` after building, but before starting the connection.</span></span>

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ConnectionOn)]

<span data-ttu-id="394c2-131">O código anterior no `connection.On` é executado quando o código do lado do servidor chama-o usando o `SendAsync` método.</span><span class="sxs-lookup"><span data-stu-id="394c2-131">The preceding code in `connection.On` runs when server-side code calls it using the `SendAsync` method.</span></span>

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?name=snippet_SendMessage)]

## <a name="error-handling-and-logging"></a><span data-ttu-id="394c2-132">Registro em log e tratamento de erros</span><span class="sxs-lookup"><span data-stu-id="394c2-132">Error handling and logging</span></span>

<span data-ttu-id="394c2-133">Tratar erros com uma instrução try-catch.</span><span class="sxs-lookup"><span data-stu-id="394c2-133">Handle errors with a try-catch statement.</span></span> <span data-ttu-id="394c2-134">Inspecione o `Exception` objeto para determinar a ação apropriada a tomar depois de ocorrer um erro.</span><span class="sxs-lookup"><span data-stu-id="394c2-134">Inspect the `Exception` object to determine the proper action to take after an error occurs.</span></span>

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ErrorHandling)]

## <a name="additional-resources"></a><span data-ttu-id="394c2-135">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="394c2-135">Additional resources</span></span>

* [<span data-ttu-id="394c2-136">Hubs</span><span class="sxs-lookup"><span data-stu-id="394c2-136">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="394c2-137">Cliente JavaScript</span><span class="sxs-lookup"><span data-stu-id="394c2-137">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="394c2-138">Publicar no Azure</span><span class="sxs-lookup"><span data-stu-id="394c2-138">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)

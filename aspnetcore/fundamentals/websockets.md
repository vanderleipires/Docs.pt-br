---
title: "Suporte a WebSockets no núcleo do ASP.NET"
author: tdykstra
description: "O que é o WebSocket suporte no ASP.NET Core e como usá-lo."
keywords: "Núcleo do ASP.NET, WebSockets"
ms.author: tdykstra
manager: wpickett
ms.date: 03/25/2017
ms.topic: article
ms.assetid: 0e0fedcd-a7b4-4479-8ae0-36eab0229d7e
ms.technology: aspnet
ms.prod: aspnet-core
uid: fundamentals/websockets
ms.openlocfilehash: a5914ad0d1056db993fcbfa1f00dd3422fcc4b6e
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/12/2017
---
# <a name="introduction-to-websockets-in-aspnet-core"></a><span data-ttu-id="338a6-104">Introdução ao WebSockets no núcleo do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="338a6-104">Introduction to WebSockets in ASP.NET Core</span></span>

<span data-ttu-id="338a6-105">Por [Tom Dykstra](https://github.com/tdykstra) e [Andrew Stanton-enfermeiro](https://github.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="338a6-105">By [Tom Dykstra](https://github.com/tdykstra) and [Andrew Stanton-Nurse](https://github.com/anurse)</span></span>

<span data-ttu-id="338a6-106">Este artigo explica como começar com WebSockets no núcleo do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="338a6-106">This article explains how to get started with WebSockets in ASP.NET Core.</span></span> <span data-ttu-id="338a6-107">[WebSocket](https://wikipedia.org/wiki/WebSocket) é um protocolo que permite que os canais de comunicação persistente bidirecional em conexões TCP.</span><span class="sxs-lookup"><span data-stu-id="338a6-107">[WebSocket](https://wikipedia.org/wiki/WebSocket) is a protocol that enables two-way persistent communication channels over TCP connections.</span></span> <span data-ttu-id="338a6-108">Ele é usado para aplicativos tais como bate-papo, cotações de ações, jogos, em qualquer lugar você deseja a funcionalidade em tempo real em um aplicativo web.</span><span class="sxs-lookup"><span data-stu-id="338a6-108">It is used for applications such as chat, stock tickers, games, anywhere you want real-time functionality in a web application.</span></span>

<span data-ttu-id="338a6-109">[Exibir ou baixar o código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample).</span><span class="sxs-lookup"><span data-stu-id="338a6-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample).</span></span> <span data-ttu-id="338a6-110">Consulte o [próximas etapas](#next-steps) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="338a6-110">See the [Next Steps](#next-steps) section for more information.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="338a6-111">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="338a6-111">Prerequisites</span></span>

* <span data-ttu-id="338a6-112">ASP.NET Core 1.1 (não é executado em 1.0)</span><span class="sxs-lookup"><span data-stu-id="338a6-112">ASP.NET Core 1.1 (does not run on 1.0)</span></span>
* <span data-ttu-id="338a6-113">Qualquer sistema operacional que executa o ASP.NET Core em:</span><span class="sxs-lookup"><span data-stu-id="338a6-113">Any OS that ASP.NET Core runs on:</span></span>
  
  * <span data-ttu-id="338a6-114">Windows 7 / Windows Server 2008 e posterior</span><span class="sxs-lookup"><span data-stu-id="338a6-114">Windows 7 / Windows Server 2008 and later</span></span>
  * <span data-ttu-id="338a6-115">Linux</span><span class="sxs-lookup"><span data-stu-id="338a6-115">Linux</span></span>
  * <span data-ttu-id="338a6-116">macOS</span><span class="sxs-lookup"><span data-stu-id="338a6-116">macOS</span></span>

* <span data-ttu-id="338a6-117">**Exceção**: se seu aplicativo é executado no Windows com o IIS ou com WebListener, você deve usar:</span><span class="sxs-lookup"><span data-stu-id="338a6-117">**Exception**: If your app runs on Windows with IIS, or with WebListener, you must use:</span></span>

  * <span data-ttu-id="338a6-118">Windows 8 / Windows Server 2012 ou posterior</span><span class="sxs-lookup"><span data-stu-id="338a6-118">Windows 8 / Windows Server 2012 or later</span></span>
  * <span data-ttu-id="338a6-119">IIS 8 / 8 do IIS Express</span><span class="sxs-lookup"><span data-stu-id="338a6-119">IIS 8 / IIS 8 Express</span></span>
  * <span data-ttu-id="338a6-120">WebSocket deve ser habilitada no IIS</span><span class="sxs-lookup"><span data-stu-id="338a6-120">WebSocket must be enabled in IIS</span></span>

* <span data-ttu-id="338a6-121">Para navegadores com suporte, consulte http://caniuse.com/#feat=websockets.</span><span class="sxs-lookup"><span data-stu-id="338a6-121">For supported browsers, see http://caniuse.com/#feat=websockets.</span></span>

## <a name="when-to-use-it"></a><span data-ttu-id="338a6-122">Quando usar</span><span class="sxs-lookup"><span data-stu-id="338a6-122">When to use it</span></span>

<span data-ttu-id="338a6-123">Use WebSockets quando você precisar trabalhar diretamente com uma conexão de soquete.</span><span class="sxs-lookup"><span data-stu-id="338a6-123">Use WebSockets when you need to work directly with a socket connection.</span></span> <span data-ttu-id="338a6-124">Por exemplo, talvez seja necessário o melhor desempenho possível para um jogo em tempo real.</span><span class="sxs-lookup"><span data-stu-id="338a6-124">For example, you might need the best possible performance for a real-time game.</span></span>

<span data-ttu-id="338a6-125">[ASP.NET SignalR](https://docs.microsoft.com/aspnet/signalr/overview/getting-started/introduction-to-signalr) fornece mais um rico modelo de aplicativo para a funcionalidade em tempo real, mas ele é executado somente no ASP.NET, não o ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="338a6-125">[ASP.NET SignalR](https://docs.microsoft.com/aspnet/signalr/overview/getting-started/introduction-to-signalr) provides a richer application model for real-time functionality, but it runs only on ASP.NET, not ASP.NET Core.</span></span> <span data-ttu-id="338a6-126">Uma versão principal do SignalR está em desenvolvimento; para acompanhar seu progresso, consulte o [repositório GitHub para o SignalR Core](https://github.com/aspnet/SignalR).</span><span class="sxs-lookup"><span data-stu-id="338a6-126">A Core version of SignalR is under development; to follow its progress, see the [GitHub repository for SignalR Core](https://github.com/aspnet/SignalR).</span></span>

<span data-ttu-id="338a6-127">Se você não quiser aguardar SignalR Core, você pode usar WebSockets diretamente agora.</span><span class="sxs-lookup"><span data-stu-id="338a6-127">If you don't want to wait for SignalR Core, you can use WebSockets directly now.</span></span> <span data-ttu-id="338a6-128">Mas talvez você precise desenvolver recursos SignalR ofereceria, tais como:</span><span class="sxs-lookup"><span data-stu-id="338a6-128">But you might have to develop features that SignalR would provide, such as:</span></span>

* <span data-ttu-id="338a6-129">Suporte para uma variedade maior de versões de navegador usando fallback automático para métodos de transporte alternativo.</span><span class="sxs-lookup"><span data-stu-id="338a6-129">Support for a broader range of browser versions by using automatic fallback to alternative transport methods.</span></span>
* <span data-ttu-id="338a6-130">Reconexão automática quando uma conexão cair.</span><span class="sxs-lookup"><span data-stu-id="338a6-130">Automatic reconnection when a connection drops.</span></span>
* <span data-ttu-id="338a6-131">Suporte para clientes chamar métodos no servidor ou vice-versa.</span><span class="sxs-lookup"><span data-stu-id="338a6-131">Support for clients calling methods on the server or vice versa.</span></span>
* <span data-ttu-id="338a6-132">Suporte para dimensionamento em vários servidores.</span><span class="sxs-lookup"><span data-stu-id="338a6-132">Support for scaling to multiple servers.</span></span>

## <a name="how-to-use-it"></a><span data-ttu-id="338a6-133">Como usá-lo</span><span class="sxs-lookup"><span data-stu-id="338a6-133">How to use it</span></span>

* <span data-ttu-id="338a6-134">Instalar o [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) pacote.</span><span class="sxs-lookup"><span data-stu-id="338a6-134">Install the [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) package.</span></span>
* <span data-ttu-id="338a6-135">Configure o middleware.</span><span class="sxs-lookup"><span data-stu-id="338a6-135">Configure the middleware.</span></span>
* <span data-ttu-id="338a6-136">Aceite solicitações de WebSocket.</span><span class="sxs-lookup"><span data-stu-id="338a6-136">Accept WebSocket requests.</span></span>
* <span data-ttu-id="338a6-137">Enviar e receber mensagens.</span><span class="sxs-lookup"><span data-stu-id="338a6-137">Send and receive messages.</span></span>

### <a name="configure-the-middleware"></a><span data-ttu-id="338a6-138">Configurar o middleware</span><span class="sxs-lookup"><span data-stu-id="338a6-138">Configure the middleware</span></span>

<span data-ttu-id="338a6-139">Adicionar o middleware de WebSockets no `Configure` método o `Startup` classe.</span><span class="sxs-lookup"><span data-stu-id="338a6-139">Add the WebSockets middleware in the `Configure` method of the `Startup` class.</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSockets)]

<span data-ttu-id="338a6-140">Podem ser definidas as seguintes configurações:</span><span class="sxs-lookup"><span data-stu-id="338a6-140">The following settings can be configured:</span></span>

* <span data-ttu-id="338a6-141">`KeepAliveInterval`-A frequência de enviar quadros "ping" para o cliente, certifique-se de proxies de manter a conexão aberta.</span><span class="sxs-lookup"><span data-stu-id="338a6-141">`KeepAliveInterval` - How frequently to send "ping" frames to the client, to ensure proxies keep the connection open.</span></span>
* <span data-ttu-id="338a6-142">`ReceiveBufferSize`-O tamanho do buffer usado para receber dados.</span><span class="sxs-lookup"><span data-stu-id="338a6-142">`ReceiveBufferSize` - The size of the buffer used to receive data.</span></span> <span data-ttu-id="338a6-143">Somente usuários avançados precisaria alterar isso, para ajuste de desempenho com base no tamanho de seus dados.</span><span class="sxs-lookup"><span data-stu-id="338a6-143">Only advanced users would need to change this, for performance tuning based on the size of their data.</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSocketsOptions)]

### <a name="accept-websocket-requests"></a><span data-ttu-id="338a6-144">Aceitar solicitações de WebSocket</span><span class="sxs-lookup"><span data-stu-id="338a6-144">Accept WebSocket requests</span></span>

<span data-ttu-id="338a6-145">Em algum lugar posteriormente no ciclo de vida de solicitação (posteriormente o `Configure` método ou em uma ação do MVC, por exemplo) Verifique se é uma solicitação WebSocket e aceitar a solicitação WebSocket.</span><span class="sxs-lookup"><span data-stu-id="338a6-145">Somewhere later in the request life cycle (later in the `Configure` method or in an MVC action, for example) check if it's a WebSocket request and accept the WebSocket request.</span></span>

<span data-ttu-id="338a6-146">Este exemplo é de mais tarde no `Configure` método.</span><span class="sxs-lookup"><span data-stu-id="338a6-146">This example is from later in the `Configure` method.</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=AcceptWebSocket&highlight=7)]

<span data-ttu-id="338a6-147">Uma solicitação WebSocket pode entrar em qualquer URL, mas esse código de exemplo só aceita solicitações de `/ws`.</span><span class="sxs-lookup"><span data-stu-id="338a6-147">A WebSocket request could come in on any URL, but this sample code only accepts requests for `/ws`.</span></span>

### <a name="send-and-receive-messages"></a><span data-ttu-id="338a6-148">Enviar e receber mensagens</span><span class="sxs-lookup"><span data-stu-id="338a6-148">Send and receive messages</span></span>

<span data-ttu-id="338a6-149">O `AcceptWebSocketAsync` método atualiza a conexão TCP para uma conexão WebSocket e oferece uma [WebSocket](https://docs.microsoft.com/dotnet/core/api/system.net.websockets.websocket) objeto.</span><span class="sxs-lookup"><span data-stu-id="338a6-149">The `AcceptWebSocketAsync` method upgrades the TCP connection to a WebSocket connection and gives you a [WebSocket](https://docs.microsoft.com/dotnet/core/api/system.net.websockets.websocket) object.</span></span> <span data-ttu-id="338a6-150">Use o objeto WebSocket para enviar e receber mensagens.</span><span class="sxs-lookup"><span data-stu-id="338a6-150">Use the WebSocket object to send and receive messages.</span></span>

<span data-ttu-id="338a6-151">O código mostrado anteriormente que aceita a solicitação WebSocket passa o `WebSocket` o objeto para um `Echo` método; este é o `Echo` método.</span><span class="sxs-lookup"><span data-stu-id="338a6-151">The code shown earlier that accepts the WebSocket request passes the `WebSocket` object to an `Echo` method; here's the `Echo` method.</span></span> <span data-ttu-id="338a6-152">O código recebe uma mensagem e envia imediatamente a mesma mensagem.</span><span class="sxs-lookup"><span data-stu-id="338a6-152">The code receives a message and immediately sends back the same message.</span></span> <span data-ttu-id="338a6-153">Ela permanece em um loop, fazendo isso até que o cliente fecha a conexão.</span><span class="sxs-lookup"><span data-stu-id="338a6-153">It stays in a loop doing that until the client closes the connection.</span></span> 

[!code-csharp[](websockets/sample/Startup.cs?name=Echo)]

<span data-ttu-id="338a6-154">Quando você aceita o WebSocket antes de começar esse loop, o pipeline de middleware termina.</span><span class="sxs-lookup"><span data-stu-id="338a6-154">When you accept the WebSocket before beginning this loop, the middleware pipeline ends.</span></span>  <span data-ttu-id="338a6-155">Ao fechar o soquete, o pipeline esvazia.</span><span class="sxs-lookup"><span data-stu-id="338a6-155">Upon closing the socket, the pipeline unwinds.</span></span> <span data-ttu-id="338a6-156">Ou seja, as paradas de solicitação são encaminhados no pipeline, quando você aceita o WebSocket, assim como faria quando você atinge uma ação do MVC, por exemplo.</span><span class="sxs-lookup"><span data-stu-id="338a6-156">That is, the request stops moving forward in the pipeline when you accept a WebSocket, just as it would when you hit an MVC action, for example.</span></span>  <span data-ttu-id="338a6-157">Mas quando você concluir este loop e fecha o soquete, a solicitação continuará o pipeline de backup.</span><span class="sxs-lookup"><span data-stu-id="338a6-157">But when you finish this loop and close the socket, the request proceeds back up the pipeline.</span></span>

## <a name="next-steps"></a><span data-ttu-id="338a6-158">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="338a6-158">Next steps</span></span>

<span data-ttu-id="338a6-159">O [aplicativo de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) que acompanha este artigo é um aplicativo simples de eco.</span><span class="sxs-lookup"><span data-stu-id="338a6-159">The [sample application](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) that accompanies this article is a simple echo application.</span></span> <span data-ttu-id="338a6-160">Ele tem uma página da web que faz conexões WebSocket e o servidor reenvia apenas para o cliente qualquer mensagem recebida.</span><span class="sxs-lookup"><span data-stu-id="338a6-160">It has a web page that makes WebSocket connections, and the server just resends back to the client any messages it receives.</span></span> <span data-ttu-id="338a6-161">Execute-o em um prompt de comando (ele não configurou a execução do Visual Studio com o IIS Express) e navegue até http://localhost:5000/.</span><span class="sxs-lookup"><span data-stu-id="338a6-161">Run it from a command prompt (it's not set up to run from Visual Studio with IIS Express) and navigate to http://localhost:5000.</span></span> <span data-ttu-id="338a6-162">A página da web exibe o status de conexão no canto superior esquerdo:</span><span class="sxs-lookup"><span data-stu-id="338a6-162">The web page shows connection status at the upper left:</span></span>

![Estado inicial da página da web](websockets/_static/start.png)

<span data-ttu-id="338a6-164">Selecione **conectar** para enviar uma solicitação WebSocket para a URL mostrada.</span><span class="sxs-lookup"><span data-stu-id="338a6-164">Select **Connect** to send a WebSocket request to the URL shown.</span></span>  <span data-ttu-id="338a6-165">Insira uma mensagem de teste e selecione **enviar**.</span><span class="sxs-lookup"><span data-stu-id="338a6-165">Enter a test message and select **Send**.</span></span> <span data-ttu-id="338a6-166">Ao terminar, selecione **fechar soquete**.</span><span class="sxs-lookup"><span data-stu-id="338a6-166">When done, select **Close Socket**.</span></span> <span data-ttu-id="338a6-167">O **comunicação Log** seção informa cada envio aberto, e ação de fechamento como ela ocorre.</span><span class="sxs-lookup"><span data-stu-id="338a6-167">The **Communication Log** section reports each open, send, and close action as it happens.</span></span>

![Estado inicial da página da web](websockets/_static/end.png)

---
title: Suporte ao WebSockets no ASP.NET Core
author: tdykstra
description: "Saiba como começar a usar o WebSockets no ASP.NET Core."
manager: wpickett
ms.author: tdykstra
ms.date: 03/25/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/websockets
ms.openlocfilehash: 306eca28b9f1f66e1ccaf185ccae87db8dea1b01
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/30/2018
---
# <a name="introduction-to-websockets-in-aspnet-core"></a><span data-ttu-id="c32ef-103">Introdução ao WebSockets no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c32ef-103">Introduction to WebSockets in ASP.NET Core</span></span>

<span data-ttu-id="c32ef-104">Por [Tom Dykstra](https://github.com/tdykstra) e [Andrew Stanton-Nurse](https://github.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="c32ef-104">By [Tom Dykstra](https://github.com/tdykstra) and [Andrew Stanton-Nurse](https://github.com/anurse)</span></span>

<span data-ttu-id="c32ef-105">Este artigo explica como começar a usar o WebSockets no ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c32ef-105">This article explains how to get started with WebSockets in ASP.NET Core.</span></span> <span data-ttu-id="c32ef-106">O [WebSocket](https://wikipedia.org/wiki/WebSocket) é um protocolo que permite canais de comunicação persistentes bidirecionais em conexões TCP.</span><span class="sxs-lookup"><span data-stu-id="c32ef-106">[WebSocket](https://wikipedia.org/wiki/WebSocket) is a protocol that enables two-way persistent communication channels over TCP connections.</span></span> <span data-ttu-id="c32ef-107">Ele é usado em aplicativos como chat, cotações da bolsa, jogos e em qualquer lugar que você desejar inserir uma funcionalidade em tempo real em um aplicativo Web.</span><span class="sxs-lookup"><span data-stu-id="c32ef-107">It's used for applications such as chat, stock tickers, games, anywhere you want real-time functionality in a web application.</span></span>

<span data-ttu-id="c32ef-108">[Exibir ou baixar um código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="c32ef-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span> <span data-ttu-id="c32ef-109">Consulte a seção [Próximas etapas](#next-steps) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="c32ef-109">See the [Next Steps](#next-steps) section for more information.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="c32ef-110">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="c32ef-110">Prerequisites</span></span>

* <span data-ttu-id="c32ef-111">ASP.NET Core 1.1 (não executado no 1.0)</span><span class="sxs-lookup"><span data-stu-id="c32ef-111">ASP.NET Core 1.1 (doesn't run on 1.0)</span></span>
* <span data-ttu-id="c32ef-112">Qualquer sistema operacional no qual o ASP.NET Core é executado:</span><span class="sxs-lookup"><span data-stu-id="c32ef-112">Any OS that ASP.NET Core runs on:</span></span>
  
  * <span data-ttu-id="c32ef-113">Windows 7/Windows Server 2008 e posterior</span><span class="sxs-lookup"><span data-stu-id="c32ef-113">Windows 7 / Windows Server 2008 and later</span></span>
  * <span data-ttu-id="c32ef-114">Linux</span><span class="sxs-lookup"><span data-stu-id="c32ef-114">Linux</span></span>
  * <span data-ttu-id="c32ef-115">macOS</span><span class="sxs-lookup"><span data-stu-id="c32ef-115">macOS</span></span>

* <span data-ttu-id="c32ef-116">**Exceção**: caso o aplicativo seja executado no Windows com o IIS ou com WebListener, você precisará usar:</span><span class="sxs-lookup"><span data-stu-id="c32ef-116">**Exception**: If your app runs on Windows with IIS, or with WebListener, you must use:</span></span>

  * <span data-ttu-id="c32ef-117">Windows 8/Windows Server 2012 ou posterior</span><span class="sxs-lookup"><span data-stu-id="c32ef-117">Windows 8 / Windows Server 2012 or later</span></span>
  * <span data-ttu-id="c32ef-118">IIS 8/IIS 8 Express</span><span class="sxs-lookup"><span data-stu-id="c32ef-118">IIS 8 / IIS 8 Express</span></span>
  * <span data-ttu-id="c32ef-119">O WebSocket precisa estar habilitado no IIS</span><span class="sxs-lookup"><span data-stu-id="c32ef-119">WebSocket must be enabled in IIS</span></span>

* <span data-ttu-id="c32ef-120">Para navegadores compatíveis, consulte http://caniuse.com/#feat=websockets.</span><span class="sxs-lookup"><span data-stu-id="c32ef-120">For supported browsers, see http://caniuse.com/#feat=websockets.</span></span>

## <a name="when-to-use-it"></a><span data-ttu-id="c32ef-121">Quando usar</span><span class="sxs-lookup"><span data-stu-id="c32ef-121">When to use it</span></span>

<span data-ttu-id="c32ef-122">Use o WebSockets quando você precisar trabalhar diretamente com uma conexão de soquete.</span><span class="sxs-lookup"><span data-stu-id="c32ef-122">Use WebSockets when you need to work directly with a socket connection.</span></span> <span data-ttu-id="c32ef-123">Por exemplo, talvez você precise obter o melhor desempenho possível para um jogo em tempo real.</span><span class="sxs-lookup"><span data-stu-id="c32ef-123">For example, you might need the best possible performance for a real-time game.</span></span>

<span data-ttu-id="c32ef-124">O [ASP.NET SignalR](https://docs.microsoft.com/aspnet/signalr/overview/getting-started/introduction-to-signalr) fornece um modelo de aplicativo mais avançado para a funcionalidade em tempo real, mas ele é executado somente no ASP.NET, não no ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c32ef-124">[ASP.NET SignalR](https://docs.microsoft.com/aspnet/signalr/overview/getting-started/introduction-to-signalr) provides a richer application model for real-time functionality, but it runs only on ASP.NET, not ASP.NET Core.</span></span> <span data-ttu-id="c32ef-125">Uma versão Core do SignalR está em desenvolvimento; para acompanhar seu progresso, acesse o [repositório GitHub e pesquise SignalR Core](https://github.com/aspnet/SignalR).</span><span class="sxs-lookup"><span data-stu-id="c32ef-125">A Core version of SignalR is under development; to follow its progress, see the [GitHub repository for SignalR Core](https://github.com/aspnet/SignalR).</span></span>

<span data-ttu-id="c32ef-126">Caso não deseje aguardar o SignalR Core, use o WebSockets diretamente agora.</span><span class="sxs-lookup"><span data-stu-id="c32ef-126">If you don't want to wait for SignalR Core, you can use WebSockets directly now.</span></span> <span data-ttu-id="c32ef-127">No entanto, talvez você precise desenvolver recursos fornecidos pelo SignalR, como:</span><span class="sxs-lookup"><span data-stu-id="c32ef-127">But you might have to develop features that SignalR would provide, such as:</span></span>

* <span data-ttu-id="c32ef-128">Suporte para uma variedade maior de versões de navegador usando fallback automático para métodos de transporte alternativos.</span><span class="sxs-lookup"><span data-stu-id="c32ef-128">Support for a broader range of browser versions by using automatic fallback to alternative transport methods.</span></span>
* <span data-ttu-id="c32ef-129">Reconexão automática quando houver uma queda de conexão.</span><span class="sxs-lookup"><span data-stu-id="c32ef-129">Automatic reconnection when a connection drops.</span></span>
* <span data-ttu-id="c32ef-130">Suporte para clientes que chamam métodos no servidor ou vice-versa.</span><span class="sxs-lookup"><span data-stu-id="c32ef-130">Support for clients calling methods on the server or vice versa.</span></span>
* <span data-ttu-id="c32ef-131">Suporte para dimensionamento para vários servidores.</span><span class="sxs-lookup"><span data-stu-id="c32ef-131">Support for scaling to multiple servers.</span></span>

## <a name="how-to-use-it"></a><span data-ttu-id="c32ef-132">Como usá-lo</span><span class="sxs-lookup"><span data-stu-id="c32ef-132">How to use it</span></span>

* <span data-ttu-id="c32ef-133">Instale o pacote [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/).</span><span class="sxs-lookup"><span data-stu-id="c32ef-133">Install the [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) package.</span></span>
* <span data-ttu-id="c32ef-134">Configure o middleware.</span><span class="sxs-lookup"><span data-stu-id="c32ef-134">Configure the middleware.</span></span>
* <span data-ttu-id="c32ef-135">Aceite solicitações do WebSocket.</span><span class="sxs-lookup"><span data-stu-id="c32ef-135">Accept WebSocket requests.</span></span>
* <span data-ttu-id="c32ef-136">Envie e receba mensagens.</span><span class="sxs-lookup"><span data-stu-id="c32ef-136">Send and receive messages.</span></span>

### <a name="configure-the-middleware"></a><span data-ttu-id="c32ef-137">Configurar o middleware</span><span class="sxs-lookup"><span data-stu-id="c32ef-137">Configure the middleware</span></span>

<span data-ttu-id="c32ef-138">Adicione o middleware do WebSockets ao método `Configure` da classe `Startup`.</span><span class="sxs-lookup"><span data-stu-id="c32ef-138">Add the WebSockets middleware in the `Configure` method of the `Startup` class.</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSockets)]

<span data-ttu-id="c32ef-139">As seguintes configurações podem ser definidas:</span><span class="sxs-lookup"><span data-stu-id="c32ef-139">The following settings can be configured:</span></span>

* <span data-ttu-id="c32ef-140">`KeepAliveInterval` – a frequência de envio de quadros “ping” para o cliente, a fim de garantir que os proxies mantenham a conexão aberta.</span><span class="sxs-lookup"><span data-stu-id="c32ef-140">`KeepAliveInterval` - How frequently to send "ping" frames to the client, to ensure proxies keep the connection open.</span></span>
* <span data-ttu-id="c32ef-141">`ReceiveBufferSize` – o tamanho do buffer usado para receber dados.</span><span class="sxs-lookup"><span data-stu-id="c32ef-141">`ReceiveBufferSize` - The size of the buffer used to receive data.</span></span> <span data-ttu-id="c32ef-142">Somente usuários avançados precisam alterar isso, para ajuste de desempenho de acordo com o tamanho de seus dados.</span><span class="sxs-lookup"><span data-stu-id="c32ef-142">Only advanced users would need to change this, for performance tuning based on the size of their data.</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSocketsOptions)]

### <a name="accept-websocket-requests"></a><span data-ttu-id="c32ef-143">Aceitar solicitações do WebSocket</span><span class="sxs-lookup"><span data-stu-id="c32ef-143">Accept WebSocket requests</span></span>

<span data-ttu-id="c32ef-144">Em algum lugar e em um momento futuro no ciclo de vida da solicitação (posteriormente no método `Configure` ou em uma ação do MVC, por exemplo), verifique se é uma solicitação do WebSocket e aceite-a.</span><span class="sxs-lookup"><span data-stu-id="c32ef-144">Somewhere later in the request life cycle (later in the `Configure` method or in an MVC action, for example) check if it's a WebSocket request and accept the WebSocket request.</span></span>

<span data-ttu-id="c32ef-145">Este exemplo é obtido de uma fase posterior do método `Configure`.</span><span class="sxs-lookup"><span data-stu-id="c32ef-145">This example is from later in the `Configure` method.</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=AcceptWebSocket&highlight=7)]

<span data-ttu-id="c32ef-146">Uma solicitação do WebSocket pode entrar em qualquer URL, mas esse código de exemplo aceita apenas solicitações de `/ws`.</span><span class="sxs-lookup"><span data-stu-id="c32ef-146">A WebSocket request could come in on any URL, but this sample code only accepts requests for `/ws`.</span></span>

### <a name="send-and-receive-messages"></a><span data-ttu-id="c32ef-147">Enviar e receber mensagens</span><span class="sxs-lookup"><span data-stu-id="c32ef-147">Send and receive messages</span></span>

<span data-ttu-id="c32ef-148">O método `AcceptWebSocketAsync` faz upgrade da conexão TCP para uma conexão WebSocket e fornece um objeto [WebSocket](https://docs.microsoft.com/dotnet/core/api/system.net.websockets.websocket).</span><span class="sxs-lookup"><span data-stu-id="c32ef-148">The `AcceptWebSocketAsync` method upgrades the TCP connection to a WebSocket connection and gives you a [WebSocket](https://docs.microsoft.com/dotnet/core/api/system.net.websockets.websocket) object.</span></span> <span data-ttu-id="c32ef-149">Use o objeto WebSocket para enviar e receber mensagens.</span><span class="sxs-lookup"><span data-stu-id="c32ef-149">Use the WebSocket object to send and receive messages.</span></span>

<span data-ttu-id="c32ef-150">O código mostrado anteriormente que aceita a solicitação do WebSocket passa o objeto `WebSocket` para um método `Echo`; esse é o método `Echo`.</span><span class="sxs-lookup"><span data-stu-id="c32ef-150">The code shown earlier that accepts the WebSocket request passes the `WebSocket` object to an `Echo` method; here's the `Echo` method.</span></span> <span data-ttu-id="c32ef-151">O código recebe uma mensagem e envia de volta imediatamente a mesma mensagem.</span><span class="sxs-lookup"><span data-stu-id="c32ef-151">The code receives a message and immediately sends back the same message.</span></span> <span data-ttu-id="c32ef-152">Ela permanece em um loop, fazendo isso até que o cliente feche a conexão.</span><span class="sxs-lookup"><span data-stu-id="c32ef-152">It stays in a loop doing that until the client closes the connection.</span></span> 

[!code-csharp[](websockets/sample/Startup.cs?name=Echo)]

<span data-ttu-id="c32ef-153">Quando você aceita o WebSocket antes de iniciar esse loop, o pipeline de middleware termina.</span><span class="sxs-lookup"><span data-stu-id="c32ef-153">When you accept the WebSocket before beginning this loop, the middleware pipeline ends.</span></span>  <span data-ttu-id="c32ef-154">Ao fechar o soquete, o pipeline é desenrolado.</span><span class="sxs-lookup"><span data-stu-id="c32ef-154">Upon closing the socket, the pipeline unwinds.</span></span> <span data-ttu-id="c32ef-155">Ou seja, a solicitação para de avançar no pipeline quando você aceita o WebSocket, assim como faria quando você encontra uma ação do MVC, por exemplo.</span><span class="sxs-lookup"><span data-stu-id="c32ef-155">That is, the request stops moving forward in the pipeline when you accept a WebSocket, just as it would when you hit an MVC action, for example.</span></span>  <span data-ttu-id="c32ef-156">No entanto, quando você conclui esse loop e fecha o soquete, a solicitação volta para o pipeline.</span><span class="sxs-lookup"><span data-stu-id="c32ef-156">But when you finish this loop and close the socket, the request proceeds back up the pipeline.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c32ef-157">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="c32ef-157">Next steps</span></span>

<span data-ttu-id="c32ef-158">O [aplicativo de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) que acompanha este artigo é um aplicativo de eco simples.</span><span class="sxs-lookup"><span data-stu-id="c32ef-158">The [sample application](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) that accompanies this article is a simple echo application.</span></span> <span data-ttu-id="c32ef-159">Ele contém uma página da Web que faz conexões WebSocket e o servidor apenas reenvia para o cliente as mensagens recebidas.</span><span class="sxs-lookup"><span data-stu-id="c32ef-159">It has a web page that makes WebSocket connections, and the server just resends back to the client any messages it receives.</span></span> <span data-ttu-id="c32ef-160">Execute-o em um prompt de comando (ele não está configurado para execução no Visual Studio com o IIS Express) e navegue para http://localhost:5000.</span><span class="sxs-lookup"><span data-stu-id="c32ef-160">Run it from a command prompt (it's not set up to run from Visual Studio with IIS Express) and navigate to http://localhost:5000.</span></span> <span data-ttu-id="c32ef-161">A página da Web mostra o status de conexão no canto superior esquerdo:</span><span class="sxs-lookup"><span data-stu-id="c32ef-161">The web page shows connection status at the upper left:</span></span>

![Estado inicial da página da Web](websockets/_static/start.png)

<span data-ttu-id="c32ef-163">Selecione **Conectar** para enviar uma solicitação WebSocket para a URL exibida.</span><span class="sxs-lookup"><span data-stu-id="c32ef-163">Select **Connect** to send a WebSocket request to the URL shown.</span></span>  <span data-ttu-id="c32ef-164">Insira uma mensagem de teste e selecione **Enviar**.</span><span class="sxs-lookup"><span data-stu-id="c32ef-164">Enter a test message and select **Send**.</span></span> <span data-ttu-id="c32ef-165">Quando terminar, selecione **Fechar Soquete**.</span><span class="sxs-lookup"><span data-stu-id="c32ef-165">When done, select **Close Socket**.</span></span> <span data-ttu-id="c32ef-166">A seção **Log de Comunicação** relata cada ação de abertura, envio e fechamento conforme ela ocorre.</span><span class="sxs-lookup"><span data-stu-id="c32ef-166">The **Communication Log** section reports each open, send, and close action as it happens.</span></span>

![Estado inicial da página da Web](websockets/_static/end.png)

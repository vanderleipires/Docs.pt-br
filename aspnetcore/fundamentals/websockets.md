---
title: Suporte ao WebSockets no ASP.NET Core
author: rick-anderson
description: Saiba como começar a usar o WebSockets no ASP.NET Core.
monikerRange: '>= aspnetcore-1.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/28/2018
uid: fundamentals/websockets
ms.openlocfilehash: b1e2180ed8dc93e2474ecca371d386830b7f3a9f
ms.sourcegitcommit: 6e6002de467cd135a69e5518d4ba9422d693132a
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/16/2018
ms.locfileid: "49348449"
---
# <a name="websockets-support-in-aspnet-core"></a><span data-ttu-id="2a965-103">Suporte ao WebSockets no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2a965-103">WebSockets support in ASP.NET Core</span></span>

<span data-ttu-id="2a965-104">Por [Tom Dykstra](https://github.com/tdykstra) e [Andrew Stanton-Nurse](https://github.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="2a965-104">By [Tom Dykstra](https://github.com/tdykstra) and [Andrew Stanton-Nurse](https://github.com/anurse)</span></span>

<span data-ttu-id="2a965-105">Este artigo explica como começar a usar o WebSockets no ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2a965-105">This article explains how to get started with WebSockets in ASP.NET Core.</span></span> <span data-ttu-id="2a965-106">[WebSocket](https://wikipedia.org/wiki/WebSocket) ([RFC 6455](https://tools.ietf.org/html/rfc6455)) é um protocolo que permite canais de comunicação persistentes bidirecionais em conexões TCP.</span><span class="sxs-lookup"><span data-stu-id="2a965-106">[WebSocket](https://wikipedia.org/wiki/WebSocket) ([RFC 6455](https://tools.ietf.org/html/rfc6455)) is a protocol that enables two-way persistent communication channels over TCP connections.</span></span> <span data-ttu-id="2a965-107">Ele é usado em aplicativos que se beneficiam de comunicação rápida e em tempo real, como chat, painel e aplicativos de jogos.</span><span class="sxs-lookup"><span data-stu-id="2a965-107">It's used in apps that benefit from fast, real-time communication, such as chat, dashboard, and game apps.</span></span>

<span data-ttu-id="2a965-108">[Exibir ou baixar um código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/samples) ([como baixar](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="2a965-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span> <span data-ttu-id="2a965-109">Confira a seção [Próximas etapas](#next-steps) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="2a965-109">See the [Next steps](#next-steps) section for more information.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2a965-110">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="2a965-110">Prerequisites</span></span>

* <span data-ttu-id="2a965-111">ASP.NET Core 1.1 ou posterior</span><span class="sxs-lookup"><span data-stu-id="2a965-111">ASP.NET Core 1.1 or later</span></span>
* <span data-ttu-id="2a965-112">Qualquer sistema operacional compatível com o ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="2a965-112">Any OS that supports ASP.NET Core:</span></span>
  
  * <span data-ttu-id="2a965-113">Windows 7/Windows Server 2008 ou posterior</span><span class="sxs-lookup"><span data-stu-id="2a965-113">Windows 7 / Windows Server 2008 or later</span></span>
  * <span data-ttu-id="2a965-114">Linux</span><span class="sxs-lookup"><span data-stu-id="2a965-114">Linux</span></span>
  * <span data-ttu-id="2a965-115">macOS</span><span class="sxs-lookup"><span data-stu-id="2a965-115">macOS</span></span>
  
* <span data-ttu-id="2a965-116">Se o aplicativo é executado no Windows com o IIS:</span><span class="sxs-lookup"><span data-stu-id="2a965-116">If the app runs on Windows with IIS:</span></span>

  * <span data-ttu-id="2a965-117">Windows 8/Windows Server 2012 ou posterior</span><span class="sxs-lookup"><span data-stu-id="2a965-117">Windows 8 / Windows Server 2012 or later</span></span>
  * <span data-ttu-id="2a965-118">IIS 8/IIS 8 Express</span><span class="sxs-lookup"><span data-stu-id="2a965-118">IIS 8 / IIS 8 Express</span></span>
  * <span data-ttu-id="2a965-119">WebSockets precisam ser habilitados (confira a seção [Suporte para IIS/IIS Express](#iisiis-express-support)).</span><span class="sxs-lookup"><span data-stu-id="2a965-119">WebSockets must be enabled (See the [IIS/IIS Express support](#iisiis-express-support) section.).</span></span>
  
* <span data-ttu-id="2a965-120">Se o aplicativo é executado no [HTTP.sys](xref:fundamentals/servers/httpsys):</span><span class="sxs-lookup"><span data-stu-id="2a965-120">If the app runs on [HTTP.sys](xref:fundamentals/servers/httpsys):</span></span>

  * <span data-ttu-id="2a965-121">Windows 8/Windows Server 2012 ou posterior</span><span class="sxs-lookup"><span data-stu-id="2a965-121">Windows 8 / Windows Server 2012 or later</span></span>

* <span data-ttu-id="2a965-122">Para saber quais são os navegadores compatíveis, confira https://caniuse.com/#feat=websockets.</span><span class="sxs-lookup"><span data-stu-id="2a965-122">For supported browsers, see https://caniuse.com/#feat=websockets.</span></span>

## <a name="when-to-use-websockets"></a><span data-ttu-id="2a965-123">Quando usar WebSockets</span><span class="sxs-lookup"><span data-stu-id="2a965-123">When to use WebSockets</span></span>

<span data-ttu-id="2a965-124">Use WebSockets para trabalhar diretamente com uma conexão de soquete.</span><span class="sxs-lookup"><span data-stu-id="2a965-124">Use WebSockets to work directly with a socket connection.</span></span> <span data-ttu-id="2a965-125">Por exemplo, use WebSockets para obter o melhor desempenho possível em um jogo em tempo real.</span><span class="sxs-lookup"><span data-stu-id="2a965-125">For example, use WebSockets for the best possible performance with a real-time game.</span></span>

<span data-ttu-id="2a965-126">[SignalR do ASP.NET Core](xref:signalr/introduction) é uma biblioteca que simplifica a adição da funcionalidade da Web em tempo real aos aplicativos.</span><span class="sxs-lookup"><span data-stu-id="2a965-126">[ASP.NET Core SignalR](xref:signalr/introduction) is a library that simplifies adding real-time web functionality to apps.</span></span> <span data-ttu-id="2a965-127">Ele usa WebSockets sempre que possível.</span><span class="sxs-lookup"><span data-stu-id="2a965-127">It uses WebSockets whenever possible.</span></span>

## <a name="how-to-use-websockets"></a><span data-ttu-id="2a965-128">Como usar WebSockets</span><span class="sxs-lookup"><span data-stu-id="2a965-128">How to use WebSockets</span></span>

* <span data-ttu-id="2a965-129">Instale o pacote [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/).</span><span class="sxs-lookup"><span data-stu-id="2a965-129">Install the [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) package.</span></span>
* <span data-ttu-id="2a965-130">Configure o middleware.</span><span class="sxs-lookup"><span data-stu-id="2a965-130">Configure the middleware.</span></span>
* <span data-ttu-id="2a965-131">Aceite solicitações do WebSocket.</span><span class="sxs-lookup"><span data-stu-id="2a965-131">Accept WebSocket requests.</span></span>
* <span data-ttu-id="2a965-132">Envie e receba mensagens.</span><span class="sxs-lookup"><span data-stu-id="2a965-132">Send and receive messages.</span></span>

### <a name="configure-the-middleware"></a><span data-ttu-id="2a965-133">Configurar o middleware</span><span class="sxs-lookup"><span data-stu-id="2a965-133">Configure the middleware</span></span>

<span data-ttu-id="2a965-134">Adicione o middleware do WebSockets no método `Configure` da classe `Startup`:</span><span class="sxs-lookup"><span data-stu-id="2a965-134">Add the WebSockets middleware in the `Configure` method of the `Startup` class:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=UseWebSockets)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](websockets/samples/1.x/WebSocketsSample/Startup.cs?name=UseWebSockets)]

::: moniker-end

<span data-ttu-id="2a965-135">As seguintes configurações podem ser definidas:</span><span class="sxs-lookup"><span data-stu-id="2a965-135">The following settings can be configured:</span></span>

* <span data-ttu-id="2a965-136">`KeepAliveInterval` – a frequência para enviar quadros "ping" ao cliente para garantir que os proxies mantenham a conexão aberta.</span><span class="sxs-lookup"><span data-stu-id="2a965-136">`KeepAliveInterval` - How frequently to send "ping" frames to the client to ensure proxies keep the connection open.</span></span>
* <span data-ttu-id="2a965-137">`ReceiveBufferSize` – o tamanho do buffer usado para receber dados.</span><span class="sxs-lookup"><span data-stu-id="2a965-137">`ReceiveBufferSize` - The size of the buffer used to receive data.</span></span> <span data-ttu-id="2a965-138">Os usuários avançados podem precisar alterar isso para ajuste de desempenho com base no tamanho dos dados.</span><span class="sxs-lookup"><span data-stu-id="2a965-138">Advanced users may need to change this for performance tuning based on the size of the data.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=UseWebSocketsOptions)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](websockets/samples/1.x/WebSocketsSample/Startup.cs?name=UseWebSocketsOptions)]

::: moniker-end

### <a name="accept-websocket-requests"></a><span data-ttu-id="2a965-139">Aceitar solicitações do WebSocket</span><span class="sxs-lookup"><span data-stu-id="2a965-139">Accept WebSocket requests</span></span>

<span data-ttu-id="2a965-140">Em algum lugar e em um momento futuro no ciclo de vida da solicitação (posteriormente no método `Configure` ou em uma ação do MVC, por exemplo), verifique se é uma solicitação do WebSocket e aceite-a.</span><span class="sxs-lookup"><span data-stu-id="2a965-140">Somewhere later in the request life cycle (later in the `Configure` method or in an MVC action, for example) check if it's a WebSocket request and accept the WebSocket request.</span></span>

<span data-ttu-id="2a965-141">O exemplo a seguir é de uma fase posterior do método `Configure`:</span><span class="sxs-lookup"><span data-stu-id="2a965-141">The following example is from later in the `Configure` method:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=AcceptWebSocket&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](websockets/samples/1.x/WebSocketsSample/Startup.cs?name=AcceptWebSocket&highlight=7)]

::: moniker-end

<span data-ttu-id="2a965-142">Uma solicitação do WebSocket pode entrar em qualquer URL, mas esse código de exemplo aceita apenas solicitações de `/ws`.</span><span class="sxs-lookup"><span data-stu-id="2a965-142">A WebSocket request could come in on any URL, but this sample code only accepts requests for `/ws`.</span></span>

### <a name="send-and-receive-messages"></a><span data-ttu-id="2a965-143">Enviar e receber mensagens</span><span class="sxs-lookup"><span data-stu-id="2a965-143">Send and receive messages</span></span>

<span data-ttu-id="2a965-144">O método `AcceptWebSocketAsync` atualiza a conexão TCP para uma conexão WebSocket e fornece um objeto [WebSocket](/dotnet/core/api/system.net.websockets.websocket).</span><span class="sxs-lookup"><span data-stu-id="2a965-144">The `AcceptWebSocketAsync` method upgrades the TCP connection to a WebSocket connection and provides a [WebSocket](/dotnet/core/api/system.net.websockets.websocket) object.</span></span> <span data-ttu-id="2a965-145">Use o objeto `WebSocket` para enviar e receber mensagens.</span><span class="sxs-lookup"><span data-stu-id="2a965-145">Use the `WebSocket` object to send and receive messages.</span></span>

<span data-ttu-id="2a965-146">O código mostrado anteriormente que aceita a solicitação do WebSocket passa o objeto `WebSocket` para um método `Echo`.</span><span class="sxs-lookup"><span data-stu-id="2a965-146">The code shown earlier that accepts the WebSocket request passes the `WebSocket` object to an `Echo` method.</span></span> <span data-ttu-id="2a965-147">O código recebe uma mensagem e envia de volta imediatamente a mesma mensagem.</span><span class="sxs-lookup"><span data-stu-id="2a965-147">The code receives a message and immediately sends back the same message.</span></span> <span data-ttu-id="2a965-148">As mensagens são enviadas e recebidas em um loop até que o cliente feche a conexão:</span><span class="sxs-lookup"><span data-stu-id="2a965-148">Messages are sent and received in a loop until the client closes the connection:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=Echo)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](websockets/samples/1.x/WebSocketsSample/Startup.cs?name=Echo)]

::: moniker-end

<span data-ttu-id="2a965-149">Ao aceitar a conexão WebSocket antes de iniciar o loop, o pipeline de middleware é encerrado.</span><span class="sxs-lookup"><span data-stu-id="2a965-149">When accepting the WebSocket connection before beginning the loop, the middleware pipeline ends.</span></span> <span data-ttu-id="2a965-150">Ao fechar o soquete, o pipeline é desenrolado.</span><span class="sxs-lookup"><span data-stu-id="2a965-150">Upon closing the socket, the pipeline unwinds.</span></span> <span data-ttu-id="2a965-151">Ou seja, a solicitação deixa de avançar no pipeline quando o WebSocket é aceito.</span><span class="sxs-lookup"><span data-stu-id="2a965-151">That is, the request stops moving forward in the pipeline when the WebSocket is accepted.</span></span> <span data-ttu-id="2a965-152">Quando o loop é concluído e o soquete é fechado, a solicitação continua a avançar no pipeline.</span><span class="sxs-lookup"><span data-stu-id="2a965-152">When the loop is finished and the socket is closed, the request proceeds back up the pipeline.</span></span>

## <a name="iisiis-express-support"></a><span data-ttu-id="2a965-153">Suporte ao IIS/IIS Express</span><span class="sxs-lookup"><span data-stu-id="2a965-153">IIS/IIS Express support</span></span>

<span data-ttu-id="2a965-154">O Windows Server 2012 ou posterior e o Windows 8 ou posterior com o IIS/IIS Express 8 ou posterior são compatíveis com o protocolo WebSocket.</span><span class="sxs-lookup"><span data-stu-id="2a965-154">Windows Server 2012 or later and Windows 8 or later with IIS/IIS Express 8 or later has support for the WebSocket protocol.</span></span>

> [!NOTE]
> <span data-ttu-id="2a965-155">WebSockets estão sempre habilitados ao usar o IIS Express.</span><span class="sxs-lookup"><span data-stu-id="2a965-155">WebSockets are always enabled when using IIS Express.</span></span>

### <a name="enabling-websockets-on-iis"></a><span data-ttu-id="2a965-156">Habilitar WebSockets no IIS</span><span class="sxs-lookup"><span data-stu-id="2a965-156">Enabling WebSockets on IIS</span></span>

<span data-ttu-id="2a965-157">Para habilitar o suporte para o protocolo WebSocket no Windows Server 2012 ou posterior:</span><span class="sxs-lookup"><span data-stu-id="2a965-157">To enable support for the WebSocket protocol on Windows Server 2012 or later:</span></span>

> [!NOTE]
> <span data-ttu-id="2a965-158">Estas etapas não são necessárias ao usar o IIS Express</span><span class="sxs-lookup"><span data-stu-id="2a965-158">These steps are not required when using IIS Express</span></span>

1. <span data-ttu-id="2a965-159">Use o assistente **Adicionar Funções e Recursos** por meio do menu **Gerenciar** ou do link no **Gerenciador do Servidor**.</span><span class="sxs-lookup"><span data-stu-id="2a965-159">Use the **Add Roles and Features** wizard from the **Manage** menu or the link in **Server Manager**.</span></span>
1. <span data-ttu-id="2a965-160">Selecione **Instalação baseada em função ou em recurso**.</span><span class="sxs-lookup"><span data-stu-id="2a965-160">Select **Role-based or Feature-based Installation**.</span></span> <span data-ttu-id="2a965-161">Selecione **Avançar**.</span><span class="sxs-lookup"><span data-stu-id="2a965-161">Select **Next**.</span></span>
1. <span data-ttu-id="2a965-162">Selecione o servidor apropriado (o servidor local é selecionado por padrão).</span><span class="sxs-lookup"><span data-stu-id="2a965-162">Select the appropriate server (the local server is selected by default).</span></span> <span data-ttu-id="2a965-163">Selecione **Avançar**.</span><span class="sxs-lookup"><span data-stu-id="2a965-163">Select **Next**.</span></span>
1. <span data-ttu-id="2a965-164">Expanda **Servidor Web (IIS)** na árvore **Funções**, expanda **Servidor Web** e, em seguida, expanda **Desenvolvimento de Aplicativos**.</span><span class="sxs-lookup"><span data-stu-id="2a965-164">Expand **Web Server (IIS)** in the **Roles** tree, expand **Web Server**, and then expand **Application Development**.</span></span>
1. <span data-ttu-id="2a965-165">Selecione o **Protocolo WebSocket**.</span><span class="sxs-lookup"><span data-stu-id="2a965-165">Select **WebSocket Protocol**.</span></span> <span data-ttu-id="2a965-166">Selecione **Avançar**.</span><span class="sxs-lookup"><span data-stu-id="2a965-166">Select **Next**.</span></span>
1. <span data-ttu-id="2a965-167">Se não forem necessários recursos adicionais, selecione **Avançar**.</span><span class="sxs-lookup"><span data-stu-id="2a965-167">If additional features aren't needed, select **Next**.</span></span>
1. <span data-ttu-id="2a965-168">Clique em **Instalar**.</span><span class="sxs-lookup"><span data-stu-id="2a965-168">Select **Install**.</span></span>
1. <span data-ttu-id="2a965-169">Quando a instalação for concluída, selecione **Fechar** para sair do assistente.</span><span class="sxs-lookup"><span data-stu-id="2a965-169">When the installation completes, select **Close** to exit the wizard.</span></span>

<span data-ttu-id="2a965-170">Para habilitar o suporte para o protocolo WebSocket no Windows 8 ou posterior:</span><span class="sxs-lookup"><span data-stu-id="2a965-170">To enable support for the WebSocket protocol on Windows 8 or later:</span></span>

> [!NOTE]
> <span data-ttu-id="2a965-171">Estas etapas não são necessárias ao usar o IIS Express</span><span class="sxs-lookup"><span data-stu-id="2a965-171">These steps are not required when using IIS Express</span></span>

1. <span data-ttu-id="2a965-172">Navegue para **Painel de Controle** > **Programas** > **Programas e Recursos** > **Ativar ou desativar recursos do Windows** (lado esquerdo da tela).</span><span class="sxs-lookup"><span data-stu-id="2a965-172">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>
1. <span data-ttu-id="2a965-173">Abra os nós a seguir: **Serviços de Informações da Internet** > **Serviços da World Wide Web** > **Recursos de Desenvolvimento de Aplicativos**.</span><span class="sxs-lookup"><span data-stu-id="2a965-173">Open the following nodes: **Internet Information Services** > **World Wide Web Services** > **Application Development Features**.</span></span>
1. <span data-ttu-id="2a965-174">Selecione o recurso **Protocolo WebSocket**.</span><span class="sxs-lookup"><span data-stu-id="2a965-174">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="2a965-175">Selecione **OK**.</span><span class="sxs-lookup"><span data-stu-id="2a965-175">Select **OK**.</span></span>

### <a name="disable-websocket-when-using-socketio-on-nodejs"></a><span data-ttu-id="2a965-176">Desabilite o WebSocket ao usar o socket.io no Node.js</span><span class="sxs-lookup"><span data-stu-id="2a965-176">Disable WebSocket when using socket.io on Node.js</span></span>

<span data-ttu-id="2a965-177">Se você estiver usando o suporte do WebSocket no [socket.io](https://socket.io/) no [Node.js](https://nodejs.org/), desabilite o módulo do WebSocket do IIS padrão usando o elemento `webSocket` em *web.config* ou em *applicationHost.config*. Se essa etapa não for executada, o módulo do WebSocket do IIS tentará manipular a comunicação do WebSocket em vez do Node.js e o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="2a965-177">If using the WebSocket support in [socket.io](https://socket.io/) on [Node.js](https://nodejs.org/), disable the default IIS WebSocket module using the `webSocket` element in *web.config* or *applicationHost.config*. If this step isn't performed, the IIS WebSocket module attempts to handle the WebSocket communication rather than Node.js and the app.</span></span>

```xml
<system.webServer>
  <webSocket enabled="false" />
</system.webServer>
```

## <a name="next-steps"></a><span data-ttu-id="2a965-178">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="2a965-178">Next steps</span></span>

<span data-ttu-id="2a965-179">O [aplicativo de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/samples) que acompanha este artigo é um aplicativo de eco.</span><span class="sxs-lookup"><span data-stu-id="2a965-179">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/samples) that accompanies this article is an echo app.</span></span> <span data-ttu-id="2a965-180">Ele tem uma página da Web que faz conexões WebSocket e o servidor reenvia para o cliente todas as mensagens recebidas.</span><span class="sxs-lookup"><span data-stu-id="2a965-180">It has a web page that makes WebSocket connections, and the server resends any messages it receives back to the client.</span></span> <span data-ttu-id="2a965-181">Execute o aplicativo em um prompt de comando (ele não está configurado para execução no Visual Studio com o IIS Express) e navegue para http://localhost:5000.</span><span class="sxs-lookup"><span data-stu-id="2a965-181">Run the app from a command prompt (it's not set up to run from Visual Studio with IIS Express) and navigate to http://localhost:5000.</span></span> <span data-ttu-id="2a965-182">A página da Web exibe o status de conexão no canto superior esquerdo:</span><span class="sxs-lookup"><span data-stu-id="2a965-182">The web page shows the connection status in the upper left:</span></span>

![Estado inicial da página da Web](websockets/_static/start.png)

<span data-ttu-id="2a965-184">Selecione **Conectar** para enviar uma solicitação WebSocket para a URL exibida.</span><span class="sxs-lookup"><span data-stu-id="2a965-184">Select **Connect** to send a WebSocket request to the URL shown.</span></span> <span data-ttu-id="2a965-185">Insira uma mensagem de teste e selecione **Enviar**.</span><span class="sxs-lookup"><span data-stu-id="2a965-185">Enter a test message and select **Send**.</span></span> <span data-ttu-id="2a965-186">Quando terminar, selecione **Fechar Soquete**.</span><span class="sxs-lookup"><span data-stu-id="2a965-186">When done, select **Close Socket**.</span></span> <span data-ttu-id="2a965-187">A seção **Log de Comunicação** relata cada ação de abertura, envio e fechamento conforme ela ocorre.</span><span class="sxs-lookup"><span data-stu-id="2a965-187">The **Communication Log** section reports each open, send, and close action as it happens.</span></span>

![Estado inicial da página da Web](websockets/_static/end.png)

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
# <a name="introduction-to-websockets-in-aspnet-core"></a>Introdução ao WebSockets no núcleo do ASP.NET

Por [Tom Dykstra](https://github.com/tdykstra) e [Andrew Stanton-enfermeiro](https://github.com/anurse)

Este artigo explica como começar com WebSockets no núcleo do ASP.NET. [WebSocket](https://wikipedia.org/wiki/WebSocket) é um protocolo que permite que os canais de comunicação persistente bidirecional em conexões TCP. Ele é usado para aplicativos tais como bate-papo, cotações de ações, jogos, em qualquer lugar você deseja a funcionalidade em tempo real em um aplicativo web.

[Exibir ou baixar o código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample). Consulte o [próximas etapas](#next-steps) para obter mais informações.


## <a name="prerequisites"></a>Pré-requisitos

* ASP.NET Core 1.1 (não é executado em 1.0)
* Qualquer sistema operacional que executa o ASP.NET Core em:
  
  * Windows 7 / Windows Server 2008 e posterior
  * Linux
  * macOS

* **Exceção**: se seu aplicativo é executado no Windows com o IIS ou com WebListener, você deve usar:

  * Windows 8 / Windows Server 2012 ou posterior
  * IIS 8 / 8 do IIS Express
  * WebSocket deve ser habilitada no IIS

* Para navegadores com suporte, consulte http://caniuse.com/#feat=websockets.

## <a name="when-to-use-it"></a>Quando usar

Use WebSockets quando você precisar trabalhar diretamente com uma conexão de soquete. Por exemplo, talvez seja necessário o melhor desempenho possível para um jogo em tempo real.

[ASP.NET SignalR](https://docs.microsoft.com/aspnet/signalr/overview/getting-started/introduction-to-signalr) fornece mais um rico modelo de aplicativo para a funcionalidade em tempo real, mas ele é executado somente no ASP.NET, não o ASP.NET Core. Uma versão principal do SignalR está em desenvolvimento; para acompanhar seu progresso, consulte o [repositório GitHub para o SignalR Core](https://github.com/aspnet/SignalR).

Se você não quiser aguardar SignalR Core, você pode usar WebSockets diretamente agora. Mas talvez você precise desenvolver recursos SignalR ofereceria, tais como:

* Suporte para uma variedade maior de versões de navegador usando fallback automático para métodos de transporte alternativo.
* Reconexão automática quando uma conexão cair.
* Suporte para clientes chamar métodos no servidor ou vice-versa.
* Suporte para dimensionamento em vários servidores.

## <a name="how-to-use-it"></a>Como usá-lo

* Instalar o [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) pacote.
* Configure o middleware.
* Aceite solicitações de WebSocket.
* Enviar e receber mensagens.

### <a name="configure-the-middleware"></a>Configurar o middleware

Adicionar o middleware de WebSockets no `Configure` método o `Startup` classe.

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSockets)]

Podem ser definidas as seguintes configurações:

* `KeepAliveInterval`-A frequência de enviar quadros "ping" para o cliente, certifique-se de proxies de manter a conexão aberta.
* `ReceiveBufferSize`-O tamanho do buffer usado para receber dados. Somente usuários avançados precisaria alterar isso, para ajuste de desempenho com base no tamanho de seus dados.

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSocketsOptions)]

### <a name="accept-websocket-requests"></a>Aceitar solicitações de WebSocket

Em algum lugar posteriormente no ciclo de vida de solicitação (posteriormente o `Configure` método ou em uma ação do MVC, por exemplo) Verifique se é uma solicitação WebSocket e aceitar a solicitação WebSocket.

Este exemplo é de mais tarde no `Configure` método.

[!code-csharp[](websockets/sample/Startup.cs?name=AcceptWebSocket&highlight=7)]

Uma solicitação WebSocket pode entrar em qualquer URL, mas esse código de exemplo só aceita solicitações de `/ws`.

### <a name="send-and-receive-messages"></a>Enviar e receber mensagens

O `AcceptWebSocketAsync` método atualiza a conexão TCP para uma conexão WebSocket e oferece uma [WebSocket](https://docs.microsoft.com/dotnet/core/api/system.net.websockets.websocket) objeto. Use o objeto WebSocket para enviar e receber mensagens.

O código mostrado anteriormente que aceita a solicitação WebSocket passa o `WebSocket` o objeto para um `Echo` método; este é o `Echo` método. O código recebe uma mensagem e envia imediatamente a mesma mensagem. Ela permanece em um loop, fazendo isso até que o cliente fecha a conexão. 

[!code-csharp[](websockets/sample/Startup.cs?name=Echo)]

Quando você aceita o WebSocket antes de começar esse loop, o pipeline de middleware termina.  Ao fechar o soquete, o pipeline esvazia. Ou seja, as paradas de solicitação são encaminhados no pipeline, quando você aceita o WebSocket, assim como faria quando você atinge uma ação do MVC, por exemplo.  Mas quando você concluir este loop e fecha o soquete, a solicitação continuará o pipeline de backup.

## <a name="next-steps"></a>Próximas etapas

O [aplicativo de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) que acompanha este artigo é um aplicativo simples de eco. Ele tem uma página da web que faz conexões WebSocket e o servidor reenvia apenas para o cliente qualquer mensagem recebida. Execute-o em um prompt de comando (ele não configurou a execução do Visual Studio com o IIS Express) e navegue até http://localhost:5000/. A página da web exibe o status de conexão no canto superior esquerdo:

![Estado inicial da página da web](websockets/_static/start.png)

Selecione **conectar** para enviar uma solicitação WebSocket para a URL mostrada.  Insira uma mensagem de teste e selecione **enviar**. Ao terminar, selecione **fechar soquete**. O **comunicação Log** seção informa cada envio aberto, e ação de fechamento como ela ocorre.

![Estado inicial da página da web](websockets/_static/end.png)

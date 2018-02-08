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
# <a name="introduction-to-websockets-in-aspnet-core"></a>Introdução ao WebSockets no ASP.NET Core

Por [Tom Dykstra](https://github.com/tdykstra) e [Andrew Stanton-Nurse](https://github.com/anurse)

Este artigo explica como começar a usar o WebSockets no ASP.NET Core. O [WebSocket](https://wikipedia.org/wiki/WebSocket) é um protocolo que permite canais de comunicação persistentes bidirecionais em conexões TCP. Ele é usado em aplicativos como chat, cotações da bolsa, jogos e em qualquer lugar que você desejar inserir uma funcionalidade em tempo real em um aplicativo Web.

[Exibir ou baixar um código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample)). Consulte a seção [Próximas etapas](#next-steps) para obter mais informações.


## <a name="prerequisites"></a>Pré-requisitos

* ASP.NET Core 1.1 (não executado no 1.0)
* Qualquer sistema operacional no qual o ASP.NET Core é executado:
  
  * Windows 7/Windows Server 2008 e posterior
  * Linux
  * macOS

* **Exceção**: caso o aplicativo seja executado no Windows com o IIS ou com WebListener, você precisará usar:

  * Windows 8/Windows Server 2012 ou posterior
  * IIS 8/IIS 8 Express
  * O WebSocket precisa estar habilitado no IIS

* Para navegadores compatíveis, consulte http://caniuse.com/#feat=websockets.

## <a name="when-to-use-it"></a>Quando usar

Use o WebSockets quando você precisar trabalhar diretamente com uma conexão de soquete. Por exemplo, talvez você precise obter o melhor desempenho possível para um jogo em tempo real.

O [ASP.NET SignalR](https://docs.microsoft.com/aspnet/signalr/overview/getting-started/introduction-to-signalr) fornece um modelo de aplicativo mais avançado para a funcionalidade em tempo real, mas ele é executado somente no ASP.NET, não no ASP.NET Core. Uma versão Core do SignalR está em desenvolvimento; para acompanhar seu progresso, acesse o [repositório GitHub e pesquise SignalR Core](https://github.com/aspnet/SignalR).

Caso não deseje aguardar o SignalR Core, use o WebSockets diretamente agora. No entanto, talvez você precise desenvolver recursos fornecidos pelo SignalR, como:

* Suporte para uma variedade maior de versões de navegador usando fallback automático para métodos de transporte alternativos.
* Reconexão automática quando houver uma queda de conexão.
* Suporte para clientes que chamam métodos no servidor ou vice-versa.
* Suporte para dimensionamento para vários servidores.

## <a name="how-to-use-it"></a>Como usá-lo

* Instale o pacote [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/).
* Configure o middleware.
* Aceite solicitações do WebSocket.
* Envie e receba mensagens.

### <a name="configure-the-middleware"></a>Configurar o middleware

Adicione o middleware do WebSockets ao método `Configure` da classe `Startup`.

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSockets)]

As seguintes configurações podem ser definidas:

* `KeepAliveInterval` – a frequência de envio de quadros “ping” para o cliente, a fim de garantir que os proxies mantenham a conexão aberta.
* `ReceiveBufferSize` – o tamanho do buffer usado para receber dados. Somente usuários avançados precisam alterar isso, para ajuste de desempenho de acordo com o tamanho de seus dados.

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSocketsOptions)]

### <a name="accept-websocket-requests"></a>Aceitar solicitações do WebSocket

Em algum lugar e em um momento futuro no ciclo de vida da solicitação (posteriormente no método `Configure` ou em uma ação do MVC, por exemplo), verifique se é uma solicitação do WebSocket e aceite-a.

Este exemplo é obtido de uma fase posterior do método `Configure`.

[!code-csharp[](websockets/sample/Startup.cs?name=AcceptWebSocket&highlight=7)]

Uma solicitação do WebSocket pode entrar em qualquer URL, mas esse código de exemplo aceita apenas solicitações de `/ws`.

### <a name="send-and-receive-messages"></a>Enviar e receber mensagens

O método `AcceptWebSocketAsync` faz upgrade da conexão TCP para uma conexão WebSocket e fornece um objeto [WebSocket](https://docs.microsoft.com/dotnet/core/api/system.net.websockets.websocket). Use o objeto WebSocket para enviar e receber mensagens.

O código mostrado anteriormente que aceita a solicitação do WebSocket passa o objeto `WebSocket` para um método `Echo`; esse é o método `Echo`. O código recebe uma mensagem e envia de volta imediatamente a mesma mensagem. Ela permanece em um loop, fazendo isso até que o cliente feche a conexão. 

[!code-csharp[](websockets/sample/Startup.cs?name=Echo)]

Quando você aceita o WebSocket antes de iniciar esse loop, o pipeline de middleware termina.  Ao fechar o soquete, o pipeline é desenrolado. Ou seja, a solicitação para de avançar no pipeline quando você aceita o WebSocket, assim como faria quando você encontra uma ação do MVC, por exemplo.  No entanto, quando você conclui esse loop e fecha o soquete, a solicitação volta para o pipeline.

## <a name="next-steps"></a>Próximas etapas

O [aplicativo de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) que acompanha este artigo é um aplicativo de eco simples. Ele contém uma página da Web que faz conexões WebSocket e o servidor apenas reenvia para o cliente as mensagens recebidas. Execute-o em um prompt de comando (ele não está configurado para execução no Visual Studio com o IIS Express) e navegue para http://localhost:5000. A página da Web mostra o status de conexão no canto superior esquerdo:

![Estado inicial da página da Web](websockets/_static/start.png)

Selecione **Conectar** para enviar uma solicitação WebSocket para a URL exibida.  Insira uma mensagem de teste e selecione **Enviar**. Quando terminar, selecione **Fechar Soquete**. A seção **Log de Comunicação** relata cada ação de abertura, envio e fechamento conforme ela ocorre.

![Estado inicial da página da Web](websockets/_static/end.png)

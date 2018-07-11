---
title: Suporte ao WebSockets no ASP.NET Core
author: rick-anderson
description: Saiba como começar a usar o WebSockets no ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/28/2018
uid: fundamentals/websockets
ms.openlocfilehash: a9fe13ef7895ea3ab43257dbbaf4521f883c0804
ms.sourcegitcommit: 18339e3cb5a891a3ca36d8146fa83cf91c32e707
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37433981"
---
# <a name="websockets-support-in-aspnet-core"></a>Suporte ao WebSockets no ASP.NET Core

Por [Tom Dykstra](https://github.com/tdykstra) e [Andrew Stanton-Nurse](https://github.com/anurse)

Este artigo explica como começar a usar o WebSockets no ASP.NET Core. [WebSocket](https://wikipedia.org/wiki/WebSocket) ([RFC 6455](https://tools.ietf.org/html/rfc6455)) é um protocolo que permite canais de comunicação persistentes bidirecionais em conexões TCP. Ele é usado em aplicativos que se beneficiam de comunicação rápida e em tempo real, como chat, painel e aplicativos de jogos.

[Exibir ou baixar um código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample)). Confira a seção [Próximas etapas](#next-steps) para obter mais informações.

## <a name="prerequisites"></a>Pré-requisitos

* ASP.NET Core 1.1 ou posterior
* Qualquer sistema operacional compatível com o ASP.NET Core:
  
  * Windows 7/Windows Server 2008 ou posterior
  * Linux
  * macOS
  
* Se o aplicativo é executado no Windows com o IIS:

  * Windows 8/Windows Server 2012 ou posterior
  * IIS 8/IIS 8 Express
  * O WebSocket precisa ser habilitado no IIS (confira a seção [suporte do IIS/IIS Express](#iisiis-express-support).)
  
* Se o aplicativo é executado no [HTTP.sys](xref:fundamentals/servers/httpsys):

  * Windows 8/Windows Server 2012 ou posterior

* Para saber quais são os navegadores compatíveis, confira https://caniuse.com/#feat=websockets.

## <a name="when-to-use-websockets"></a>Quando usar WebSockets

Use WebSockets para trabalhar diretamente com uma conexão de soquete. Por exemplo, use WebSockets para obter o melhor desempenho possível em um jogo em tempo real.

[SignalR do ASP.NET Core](xref:signalr/introduction) é uma biblioteca que simplifica a adição da funcionalidade da Web em tempo real aos aplicativos. Ele usa WebSockets sempre que possível.

## <a name="how-to-use-websockets"></a>Como usar WebSockets

* Instale o pacote [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/).
* Configure o middleware.
* Aceite solicitações do WebSocket.
* Envie e receba mensagens.

### <a name="configure-the-middleware"></a>Configurar o middleware

Adicione o middleware do WebSockets no método `Configure` da classe `Startup`:

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSockets)]

As seguintes configurações podem ser definidas:

* `KeepAliveInterval` – a frequência para enviar quadros "ping" ao cliente para garantir que os proxies mantenham a conexão aberta.
* `ReceiveBufferSize` – o tamanho do buffer usado para receber dados. Os usuários avançados podem precisar alterar isso para ajuste de desempenho com base no tamanho dos dados.

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSocketsOptions)]

### <a name="accept-websocket-requests"></a>Aceitar solicitações do WebSocket

Em algum lugar e em um momento futuro no ciclo de vida da solicitação (posteriormente no método `Configure` ou em uma ação do MVC, por exemplo), verifique se é uma solicitação do WebSocket e aceite-a.

O exemplo a seguir é de uma fase posterior do método `Configure`:

[!code-csharp[](websockets/sample/Startup.cs?name=AcceptWebSocket&highlight=7)]

Uma solicitação do WebSocket pode entrar em qualquer URL, mas esse código de exemplo aceita apenas solicitações de `/ws`.

### <a name="send-and-receive-messages"></a>Enviar e receber mensagens

O método `AcceptWebSocketAsync` atualiza a conexão TCP para uma conexão WebSocket e fornece um objeto [WebSocket](/dotnet/core/api/system.net.websockets.websocket). Use o objeto `WebSocket` para enviar e receber mensagens.

O código mostrado anteriormente que aceita a solicitação do WebSocket passa o objeto `WebSocket` para um método `Echo`. O código recebe uma mensagem e envia de volta imediatamente a mesma mensagem. As mensagens são enviadas e recebidas em um loop até que o cliente feche a conexão:

[!code-csharp[](websockets/sample/Startup.cs?name=Echo)]

Ao aceitar a conexão WebSocket antes de iniciar o loop, o pipeline de middleware é encerrado. Ao fechar o soquete, o pipeline é desenrolado. Ou seja, a solicitação deixa de avançar no pipeline quando o WebSocket é aceito. Quando o loop é concluído e o soquete é fechado, a solicitação continua a avançar no pipeline.

## <a name="iisiis-express-support"></a>Suporte ao IIS/IIS Express

O Windows Server 2012 ou posterior e o Windows 8 ou posterior com o IIS/IIS Express 8 ou posterior são compatíveis com o protocolo WebSocket.

Para habilitar o suporte para o protocolo WebSocket no Windows Server 2012 ou posterior:

1. Use o assistente **Adicionar Funções e Recursos** por meio do menu **Gerenciar** ou do link no **Gerenciador do Servidor**.
1. Selecione **Instalação baseada em função ou em recurso**. Selecione **Avançar**.
1. Selecione o servidor apropriado (o servidor local é selecionado por padrão). Selecione **Avançar**.
1. Expanda **Servidor Web (IIS)** na árvore **Funções**, expanda **Servidor Web** e, em seguida, expanda **Desenvolvimento de Aplicativos**.
1. Selecione o **Protocolo WebSocket**. Selecione **Avançar**.
1. Se não forem necessários recursos adicionais, selecione **Avançar**.
1. Clique em **Instalar**.
1. Quando a instalação for concluída, selecione **Fechar** para sair do assistente.

Para habilitar o suporte para o protocolo WebSocket no Windows 8 ou posterior:

1. Navegue para **Painel de Controle** > **Programas** > **Programas e Recursos** > **Ativar ou desativar recursos do Windows** (lado esquerdo da tela).
1. Abra os nós a seguir: **Serviços de Informações da Internet** > **Serviços da World Wide Web** > **Recursos de Desenvolvimento de Aplicativos**.
1. Selecione o recurso **Protocolo WebSocket**. Selecione **OK**.

**Desabilitar o WebSocket ao usar o socket.io no Node.js**

Se você estiver usando o suporte do WebSocket no [socket.io](https://socket.io/) no [Node.js](https://nodejs.org/), desabilite o módulo do WebSocket do IIS padrão usando o elemento `webSocket` em *web.config* ou em *applicationHost.config*. Se essa etapa não for executada, o módulo do WebSocket do IIS tentará manipular a comunicação do WebSocket em vez do Node.js e o aplicativo.

```xml
<system.webServer>
  <webSocket enabled="false" />
</system.webServer>
```

## <a name="next-steps"></a>Próximas etapas

O [aplicativo de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) que acompanha este artigo é um aplicativo de eco. Ele tem uma página da Web que faz conexões WebSocket e o servidor reenvia para o cliente todas as mensagens recebidas. Execute o aplicativo em um prompt de comando (ele não está configurado para execução no Visual Studio com o IIS Express) e navegue para http://localhost:5000. A página da Web exibe o status de conexão no canto superior esquerdo:

![Estado inicial da página da Web](websockets/_static/start.png)

Selecione **Conectar** para enviar uma solicitação WebSocket para a URL exibida. Insira uma mensagem de teste e selecione **Enviar**. Quando terminar, selecione **Fechar Soquete**. A seção **Log de Comunicação** relata cada ação de abertura, envio e fechamento conforme ela ocorre.

![Estado inicial da página da Web](websockets/_static/end.png)

---
title: ASP.NET Core SignalR JavaScript cliente
author: rachelappel
description: Visão geral do cliente JavaScript de SignalR do ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 05/29/2018
uid: signalr/javascript-client
ms.openlocfilehash: d0eba401b3152d510b9db092169e83f6da2a0523
ms.sourcegitcommit: 661d30492d5ef7bbca4f7e709f40d8f3309d2dac
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37938038"
---
# <a name="aspnet-core-signalr-javascript-client"></a>ASP.NET Core SignalR JavaScript cliente

Por [Rachel Appel](http://twitter.com/rachelappel)

A biblioteca de cliente JavaScript de SignalR do ASP.NET Core permite aos desenvolvedores chamar o código de hub do lado do servidor.

[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))

## <a name="install-the-signalr-client-package"></a>Instalar o pacote de cliente do SignalR

A biblioteca de cliente SignalR JavaScript é entregue como um [npm](https://www.npmjs.com/) pacote. Se você estiver usando o Visual Studio, execute `npm install` do **Package Manager Console** enquanto na pasta raiz. Para Visual Studio Code, execute o comando a partir de **Terminal integrado**.

  ```console
   npm init -y
   npm install @aspnet/signalr
  ```

NPM instala o conteúdo do pacote na *node_modules\\ @aspnet\signalr\dist\browser*  pasta. Criar uma nova pasta chamada *signalr* sob o *wwwroot\\lib* pasta. Cópia de *signalr.js* do arquivo para o *wwwroot\lib\signalr* pasta.

## <a name="use-the-signalr-javascript-client"></a>Usar o cliente SignalR JavaScript

Referência de cliente SignalR JavaScript no `<script>` elemento.

```html
<script src="~/lib/signalr/signalr.js"></script>
```

## <a name="connect-to-a-hub"></a>Conectar a um hub

O código a seguir cria e inicia uma conexão. Nome do hub é diferencia maiusculas de minúsculas.

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=9-12,28)]

### <a name="cross-origin-connections"></a>Conexões entre origens

Normalmente, os navegadores carga das conexões do mesmo domínio que a página solicitada. No entanto, há ocasiões em que é necessária uma conexão para outro domínio.

Para impedir a leitura de dados confidenciais de outro site, um site mal-intencionado [conexões entre origens](xref:security/cors) estão desabilitados por padrão. Para permitir que uma solicitação entre origens, habilitá-lo no `Startup` classe.

[!code-csharp[Cross-origin connections](javascript-client/sample/Startup.cs?highlight=29-35,56)]

## <a name="call-hub-methods-from-client"></a>Chamar métodos de hub do cliente

Os clientes JavaScript chamem métodos públicos em hubs por usando `connection.invoke`. O `invoke` método aceita dois argumentos:

* O nome do método de hub. No exemplo a seguir, o nome do hub é `SendMessage`.
* Quaisquer argumentos definidos no método de hub. No exemplo a seguir, é o nome do argumento `message`.

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=24)]

## <a name="call-client-methods-from-hub"></a>Chamar métodos de cliente do hub

Para receber mensagens do hub, definir um método usando o `connection.on` método.

* O nome do método de cliente JavaScript. No exemplo a seguir, é o nome do método `ReceiveMessage`.
* O hub passa para o método de argumentos. No exemplo a seguir, o valor do argumento é `message`.

[!code-javascript[Receive calls from hub](javascript-client/sample/wwwroot/js/chat.js?range=14-19)]

O código anterior no `connection.on` é executado quando o código do lado do servidor chama-o usando o `SendAsync` método.

[!code-csharp[Call client-side](javascript-client/sample/hubs/chathub.cs?range=8-11)]

O SignalR determina qual método de cliente para chamar, correspondendo o nome do método e argumentos definidos no `SendAsync` e `connection.on`.

> [!NOTE]
> Como prática recomendada, chame `connection.start` depois de `connection.on` para que os manipuladores são registrados antes de todas as mensagens são recebidas.

## <a name="error-handling-and-logging"></a>Registro em log e tratamento de erros

Cadeia de um `catch` método até o final do `connection.start` método para lidar com erros do lado do cliente. Use `console.error` para erros de saída para o console do navegador.

[!code-javascript[Error handling](javascript-client/sample/wwwroot/js/chat.js?range=28)]

Configure o rastreamento de log do lado do cliente, passando um agente de log e o tipo de evento para registrar em log quando a conexão é feita. As mensagens são registradas com o nível de log especificado e superior. Níveis de log disponíveis são da seguinte maneira:

* `signalR.LogLevel.Error` : Mensagens de erro. Logs `Error` somente mensagens.
* `signalR.LogLevel.Warning` : Mensagens de aviso sobre erros em potencial. Os logs `Warning`, e `Error` mensagens.
* `signalR.LogLevel.Information` : Mensagens de status sem erros. Os logs `Information`, `Warning`, e `Error` mensagens.
* `signalR.LogLevel.Trace` : Mensagens de rastreamento. Registra tudo, incluindo dados transportados entre cliente e o hub.

Use o `configureLogging` método no `HubConnectionBuilder` para configurar o nível de log. As mensagens são registradas para o console do navegador.

[!code-javascript[Logging levels](javascript-client/sample/wwwroot/js/chat.js?range=9-12)]

## <a name="related-resources"></a>Recursos relacionados

* [Hubs](xref:signalr/hubs)
* [Cliente .NET](xref:signalr/dotnet-client)
* [Publicar no Azure](xref:signalr/publish-to-azure-web-app)
* [Habilitar solicitações entre origens (CORS) no ASP.NET Core](xref:security/cors)

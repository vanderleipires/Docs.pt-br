---
title: Cliente do ASP.NET Core SignalR JavaScript
author: rachelappel
description: Visão geral do cliente do ASP.NET Core SignalR JavaScript.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 05/29/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/javascript-client
ms.openlocfilehash: 6ff888d3337bb53d435744009f4cc24b327ebcda
ms.sourcegitcommit: 7e87671fea9a5f36ca516616fe3b40b537f428d2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/12/2018
ms.locfileid: "35341932"
---
# <a name="aspnet-core-signalr-javascript-client"></a>Cliente do ASP.NET Core SignalR JavaScript

Por [Rachel Appel](http://twitter.com/rachelappel)

A biblioteca de cliente ASP.NET Core SignalR JavaScript permite que os desenvolvedores chamar o código de hub do lado do servidor.

[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))

## <a name="install-the-signalr-client-package"></a>Instale o pacote de cliente SignalR

A biblioteca de cliente SignalR JavaScript é entregue como um [npm](https://www.npmjs.com/) pacote. Se você estiver usando o Visual Studio, execute `npm install` do **Package Manager Console** enquanto estiver na pasta raiz. Para o código do Visual Studio, execute o comando a partir de **Terminal integrada**.

  ```console
   npm init -y
   npm install @aspnet/signalr
  ```

NPM instala o conteúdo do pacote no *node_modules\\ @aspnet\signalr\dist\browser*  pasta. Criar uma nova pasta chamada *signalr* sob o *wwwroot\\lib* pasta. Copie o *signalr.js* o arquivo para o *wwwroot\lib\signalr* pasta.

## <a name="use-the-signalr-javascript-client"></a>Usar o cliente SignalR JavaScript

Referência do cliente SignalR JavaScript a `<script>` elemento.

```html
<script src="~/lib/signalr/signalr.js"></script>
```

## <a name="connect-to-a-hub"></a>Conectar a um hub

O código a seguir cria e inicia uma conexão. Nome do hub diferencia maiusculas de minúsculas.

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=9-12,28)]

### <a name="cross-origin-connections"></a>Conexões entre origens

Normalmente, navegadores carregar conexões do mesmo domínio que a página solicitada. No entanto, há ocasiões em que é necessária uma conexão para outro domínio.

Para impedir que um site mal-intencionado lendo dados confidenciais de outro site, [conexões entre origens](xref:security/cors) estão desabilitados por padrão. Para permitir que uma solicitação entre origens, habilitá-lo na `Startup` classe.

[!code-csharp[Cross-origin connections](javascript-client/sample/Startup.cs?highlight=29-35,56)]

## <a name="call-hub-methods-from-client"></a>Chamar métodos de hub do cliente

Os clientes JavaScript chamar métodos públicos em hubs por usando `connection.invoke`. O `invoke` método aceita dois argumentos:

* O nome do método de hub. O exemplo a seguir, o nome de hub é `SendMessage`.
* Nenhum argumento definido no método de hub. No exemplo a seguir, é o nome do argumento `message`.

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=24)]

## <a name="call-client-methods-from-hub"></a>Chamar métodos de cliente do hub

Para receber mensagens do hub, definir um método usando o `connection.on` método.

* O nome do método de cliente JavaScript. No exemplo a seguir, é o nome do método `ReceiveMessage`.
* O hub passa para o método de argumentos. No exemplo a seguir, o valor do argumento é `message`.

[!code-javascript[Receive calls from hub](javascript-client/sample/wwwroot/js/chat.js?range=14-19)]

O código anterior no `connection.on` é executado quando o código do lado do servidor chamá-lo usando o `SendAsync` método.

[!code-csharp[Call client-side](javascript-client/sample/hubs/chathub.cs?range=8-11)]

SignalR determina qual método de cliente para chamar correspondendo o nome do método e argumentos definidos no `SendAsync` e `connection.on`.

> [!NOTE]
> Como prática recomendada, chame `connection.start` depois `connection.on` para seus manipuladores são registrados antes de todas as mensagens são recebidas.

## <a name="error-handling-and-logging"></a>Registro em log e tratamento de erros

Cadeia de um `catch` método a fim do `connection.start` método para tratar erros de cliente. Use `console.error` para erros de saída para o console do navegador.

[!code-javascript[Error handling](javascript-client/sample/wwwroot/js/chat.js?range=28)]

Configure o rastreamento de log do lado do cliente, passando um agente de log e o tipo de evento em log quando a conexão é feita. Mensagens são registradas com o nível de log especificado e superior. Níveis de log disponíveis são os seguintes:

* `signalR.LogLevel.Error` : Mensagens de erro. Logs de `Error` mensagens somente.
* `signalR.LogLevel.Warning` : Mensagens de aviso sobre erros em potencial. Logs de `Warning`, e `Error` mensagens.
* `signalR.LogLevel.Information` : Mensagens de status sem erros. Logs de `Information`, `Warning`, e `Error` mensagens.
* `signalR.LogLevel.Trace` : Mensagens de rastreamento. Registra tudo, incluindo dados transportados entre cliente e hub.

Use o `configureLogging` método `HubConnectionBuilder` para configurar o nível de log. Mensagens são registradas para o console do navegador.

[!code-javascript[Logging levels](javascript-client/sample/wwwroot/js/chat.js?range=9-12)]

## <a name="related-resources"></a>Recursos relacionados

* [Hubs](xref:signalr/hubs)
* [Cliente .NET](xref:signalr/dotnet-client)
* [Publicar no Azure](xref:signalr/publish-to-azure-web-app)
* [Habilitar solicitações entre origens (CORS) no núcleo do ASP.NET](xref:security/cors)
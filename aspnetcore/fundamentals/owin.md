---
title: Abra a Interface da Web para .NET (OWIN)
author: ardalis
description: "Descobrir como ASP.NET Core dá suporte à Interface da Web aberto para .NET (OWIN), que permite que aplicativos da web para ser separada dos servidores web."
keywords: "Núcleo do ASP.NET, Interface Web aberta para .NET, OWIN"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 70c4e6bc-a773-4039-96ec-6fe557c9369d
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/owin
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e111a451bcc741f3e77f7ce756356cc1b57a5b52
ms.sourcegitcommit: 732cd2684246e49e796836596643a8d37e20c46d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/01/2017
---
# <a name="introduction-to-open-web-interface-for-net-owin"></a>Introdução ao abrir a Interface da Web para .NET (OWIN)

Por [Steve Smith](https://ardalis.com/) e [Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core ofereça suporte à Interface Web aberta para .NET (OWIN). OWIN permite que os aplicativos da web para ser separada dos servidores web. Define uma maneira padronizada de middleware para ser usada em um pipeline para manipular solicitações e respostas associadas. Middleware e aplicativos do ASP.NET Core podem interoperar com middleware, servidores e aplicativos baseados no OWIN.

OWIN fornece uma camada de dissociação que permite duas estruturas com modelos de objeto diferentes para ser usados juntos. O `Microsoft.AspNetCore.Owin` pacote fornece duas implementações de adaptador:
- ASP.NET Core para OWIN 
- OWIN para ASP.NET Core

Isso permite que o ASP.NET Core ser hospedado em um servidor compatível OWIN/host ou para outros componentes compatíveis OWIN a ser executado sobre o ASP.NET Core.

Observação: Usar esses adaptadores vem com um custo de desempenho. Aplicativos que usam somente os componentes principais do ASP.NET não devem usar o pacote de Owin ou adaptadores.

[Exibir ou baixar o código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))

## <a name="running-owin-middleware-in-the-aspnet-pipeline"></a>Executando OWIN middleware no pipeline do ASP.NET

O suporte do ASP.NET Core OWIN é implantado como parte do `Microsoft.AspNetCore.Owin` pacote. Você pode importar suporte OWIN para seu projeto ao instalar este pacote.

Middleware OWIN está em conformidade com a [especificação OWIN](http://owin.org/spec/spec/owin-1.0.0.html), que requer um `Func<IDictionary<string, object>, Task>` interface e as chaves específicas ser definido (como `owin.ResponseBody`). Middleware OWIN simple seguinte exibe "Hello World":

```csharp
public Task OwinHello(IDictionary<string, object> environment)
{
    string responseText = "Hello World via OWIN";
    byte[] responseBytes = Encoding.UTF8.GetBytes(responseText);

    // OWIN Environment Keys: http://owin.org/spec/spec/owin-1.0.0.html
    var responseStream = (Stream)environment["owin.ResponseBody"];
    var responseHeaders = (IDictionary<string, string[]>)environment["owin.ResponseHeaders"];

    responseHeaders["Content-Length"] = new string[] { responseBytes.Length.ToString(CultureInfo.InvariantCulture) };
    responseHeaders["Content-Type"] = new string[] { "text/plain" };

    return responseStream.WriteAsync(responseBytes, 0, responseBytes.Length);
}
```

A assinatura de exemplo retorna um `Task` e aceita um `IDictionary<string, object>` conforme solicitado pelo OWIN.

O código a seguir mostra como adicionar o `OwinHello` middleware (mostrado acima) para o pipeline do ASP.NET com o `UseOwin` método de extensão.

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseOwin(pipeline =>
    {
        pipeline(next => OwinHello);
    });
}
```

Você pode configurar outras ações para ocorrer no pipeline OWIN.

> [!NOTE]
> Cabeçalhos de resposta só devem ser modificados antes da primeira gravação no fluxo de resposta.

> [!NOTE]
> Diversas chamadas para `UseOwin` não é recomendado por motivos de desempenho. Componentes OWIN funcionarão melhor se agrupados juntos.

```csharp
app.UseOwin(pipeline =>
{
    pipeline(next =>
    {
        // do something before
        return OwinHello;
        // do something after
    });
});
```

<a name=hosting-on-owin></a>

## <a name="using-aspnet-hosting-on-an-owin-based-server"></a>Usando o ASP.NET de hospedagem em um servidor baseado em OWIN

Servidores baseados no OWIN podem hospedar aplicativos ASP.NET. Um desses servidores é [Nowin](https://github.com/Bobris/Nowin), um servidor da web .NET OWIN. O exemplo deste artigo, incluí um projeto que faz referência a Nowin e usa-o para criar um `IServer` capaz de hospedar o ASP.NET Core.

[!code-csharp[Main](owin/sample/src/NowinSample/Program.cs?highlight=15)]

`IServer`é uma interface que requer um `Features` propriedade e um `Start` método.

`Start`é responsável por configurar e iniciar o servidor, que nesse caso é feito por meio de uma série de chamadas de API fluentes que definir endereços analisados a partir de IServerAddressesFeature. Observe que a configuração fluente do `_builder` variável Especifica que as solicitações serão manipuladas pelo `appFunc` definido anteriormente no método. Isso `Func` é chamado em cada solicitação para processar solicitações de entrada.

Nós adicionaremos um `IWebHostBuilder` extensão para tornar mais fácil adicionar e configurar o servidor Nowin.

```csharp
using System;
using Microsoft.AspNetCore.Hosting.Server;
using Microsoft.Extensions.DependencyInjection;
using Nowin;
using NowinSample;

namespace Microsoft.AspNetCore.Hosting
{
    public static class NowinWebHostBuilderExtensions
    {
        public static IWebHostBuilder UseNowin(this IWebHostBuilder builder)
        {
            return builder.ConfigureServices(services =>
            {
                services.AddSingleton<IServer, NowinServer>();
            });
        }

        public static IWebHostBuilder UseNowin(this IWebHostBuilder builder, Action<ServerBuilder> configure)
        {
            builder.ConfigureServices(services =>
            {
                services.Configure(configure);
            });
            return builder.UseNowin();
        }
    }
}
```

Com isso em vigor, tudo o que é necessário para executar um aplicativo ASP.NET usando o servidor personalizado para chamar a extensão na *Program.cs*:

```csharp

using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Hosting;

namespace NowinSample
{
    public class Program
    {
        public static void Main(string[] args)
        {
            var host = new WebHostBuilder()
                .UseNowin()
                .UseContentRoot(Directory.GetCurrentDirectory())
                .UseIISIntegration()
                .UseStartup<Startup>()
                .Build();

            host.Run();
        }
    }
}

```

Saiba mais sobre o ASP.NET [servidores](servers/index.md).

## <a name="run-aspnet-core-on-an-owin-based-server-and-use-its-websockets-support"></a>Executar o ASP.NET Core em um servidor baseado em OWIN e usar seu suporte de WebSockets

Outro exemplo de recursos de servidores como baseados no OWIN pode ser aproveitado por ASP.NET Core é o acesso a recursos como o WebSocket. O servidor de web OWIN .NET usado no exemplo anterior tem suporte para soquetes da Web interna, que podem ser aproveitadas por um aplicativo do ASP.NET Core. O exemplo a seguir mostra um aplicativo simples da web que suporta soquetes da Web e exibe novamente tudo enviados ao servidor por meio de WebSockets.

```csharp
public class Startup
{
    public void Configure(IApplicationBuilder app)
    {
        app.Use(async (context, next) =>
        {
            if (context.WebSockets.IsWebSocketRequest)
            {
                WebSocket webSocket = await context.WebSockets.AcceptWebSocketAsync();
                await EchoWebSocket(webSocket);
            }
            else
            {
                await next();
            }
        });

        app.Run(context =>
        {
            return context.Response.WriteAsync("Hello World");
        });
    }

    private async Task EchoWebSocket(WebSocket webSocket)
    {
        byte[] buffer = new byte[1024];
        WebSocketReceiveResult received = await webSocket.ReceiveAsync(
            new ArraySegment<byte>(buffer), CancellationToken.None);

        while (!webSocket.CloseStatus.HasValue)
        {
            // Echo anything we receive
            await webSocket.SendAsync(new ArraySegment<byte>(buffer, 0, received.Count), 
                received.MessageType, received.EndOfMessage, CancellationToken.None);

            received = await webSocket.ReceiveAsync(new ArraySegment<byte>(buffer), 
                CancellationToken.None);
        }

        await webSocket.CloseAsync(webSocket.CloseStatus.Value, 
            webSocket.CloseStatusDescription, CancellationToken.None);
    }
}
```

Isso [exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) é configurado usando o mesmo `NowinServer` que o anterior - a única diferença está em como o aplicativo está configurado no seu `Configure` método. Um teste usando [um cliente do websocket simples](https://chrome.google.com/webstore/detail/simple-websocket-client/pfdhoblngboilpfeibdedpjgfnlcodoo?hl=en) demonstra o aplicativo:

![Cliente de teste de soquete da Web](owin/_static/websocket-test.png)

## <a name="owin-environment"></a>Ambiente OWIN

Você pode construir um ambiente OWIN usando o `HttpContext`.

```csharp

   var environment = new OwinEnvironment(HttpContext);
   var features = new OwinFeatureCollection(environment);
   ```

## <a name="owin-keys"></a>Chaves OWIN

Depende do OWIN um `IDictionary<string,object>` objeto para comunicar informações ao longo de uma troca de solicitação/resposta HTTP. ASP.NET Core implementa as chaves listadas abaixo. Consulte o [especificação primária, extensões](http://owin.org/#spec), e [OWIN chave diretrizes e chaves comuns](http://owin.org/spec/spec/CommonKeys.html).

### <a name="request-data-owin-v100"></a>Dados de solicitação (OWIN v1.0.0)

| Chave               | Valor (tipo) | Descrição |
| ----------------- | ------------ | ----------- |
| owin. RequestScheme | `String` |  |
| owin. RequestMethod  | `String` | |    
| owin. RequestPathBase  | `String` | |    
| owin. RequestPath | `String` | |     
| owin. RequestQueryString  | `String` | |    
| owin. RequestProtocol  | `String` | |    
| owin. RequestHeaders | `IDictionary<string,string[]>`  | |
| owin. RequestBody | `Stream`  | |

### <a name="request-data-owin-v110"></a>Dados de solicitação (OWIN v 1.1.0)

| Chave               | Valor (tipo) | Descrição |
| ----------------- | ------------ | ----------- |
| owin. RequestId | `String` | Opcional |

### <a name="response-data-owin-v100"></a>Dados de resposta (OWIN v1.0.0)

| Chave               | Valor (tipo) | Descrição |
| ----------------- | ------------ | ----------- |
| owin. ResponseStatusCode | `int` | Opcional |
| owin. ResponseReasonPhrase | `String` | Opcional |
| owin. ResponseHeaders | `IDictionary<string,string[]>`  | |
| owin. ResponseBody | `Stream`  | |


### <a name="other-data-owin-v100"></a>Outros dados (OWIN v1.0.0)

| Chave               | Valor (tipo) | Descrição |
| ----------------- | ------------ | ----------- |
| owin. CallCancelled | `CancellationToken` |  |
| owin. Versão  | `String` | |   


### <a name="common-keys"></a>Chaves comuns

| Chave               | Valor (tipo) | Descrição |
| ----------------- | ------------ | ----------- |
| SSL. ClientCertificate | `X509Certificate` |  |
| SSL. LoadClientCertAsync  | `Func<Task>` | |    
| servidor. RemoteIpAddress  | `String` | |    
| servidor. Porta_remota | `String` | |     
| servidor. LocalIpAddress  | `String` | |    
| servidor. LocalPort  | `String` | |    
| servidor. IsLocal  | `bool` | |    
| servidor. OnSendingHeaders  | `Action<Action<object>,object>` | |


### <a name="sendfiles-v030"></a>SendFiles v0.3.0

| Chave               | Valor (tipo) | Descrição |
| ----------------- | ------------ | ----------- |
| sendfile. SendAsync | Consulte [assinatura do delegado](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm) | Por solicitação |


### <a name="opaque-v030"></a>V0.3.0 opaco

| Chave               | Valor (tipo) | Descrição |
| ----------------- | ------------ | ----------- |
| opaco. Versão | `String` |  |
| opaco. Atualizar | `OpaqueUpgrade` | Consulte [assinatura do delegado](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm) |
| opaco. Fluxo | `Stream` |  |
| opaco. CallCancelled | `CancellationToken` |  |


### <a name="websocket-v030"></a>V0.3.0 WebSocket

| Chave               | Valor (tipo) | Descrição |
| ----------------- | ------------ | ----------- |
| WebSocket. Versão | `String` |  |
| WebSocket. Aceitar | `WebSocketAccept` | Consulte [assinatura do delegado](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm) |
| WebSocket. AcceptAlt |  | Não-spec |
| WebSocket. Subprotocolo | `String` | Consulte [RFC6455 seção 4.2.2](https://tools.ietf.org/html/rfc6455#section-4.2.2) etapa 5.5 |
| WebSocket. SendAsync | `WebSocketSendAsync` | Consulte [assinatura do delegado](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)  |
| WebSocket. ReceiveAsync | `WebSocketReceiveAsync` | Consulte [assinatura do delegado](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)  |
| WebSocket. CloseAsync | `WebSocketCloseAsync` | Consulte [assinatura do delegado](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)  |
| WebSocket. CallCancelled | `CancellationToken` |  |
| WebSocket. ClientCloseStatus | `int` | Opcional |
| WebSocket. ClientCloseDescription | `String` | Opcional |


## <a name="additional-resources"></a>Recursos adicionais

* [Middleware](middleware.md)

* [Servidores](servers/index.md)

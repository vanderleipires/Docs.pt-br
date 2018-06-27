---
title: Configuração do ASP.NET SignalR Core
author: rachelappel
description: Saiba como configurar aplicativos do ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 06/30/2018
uid: signalr/configuration
ms.openlocfilehash: 167b8828efd093d755aeb1c91e080dbd22b5d485
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/26/2018
ms.locfileid: "36961975"
---
# <a name="aspnet-core-signalr-configuration"></a>Configuração do ASP.NET SignalR Core

## <a name="jsonmessagepack-serialization-options"></a>Opções de serialização JSON/MessagePack

O SignalR do ASP.NET Core dá suporte a dois protocolos para codificação de mensagens: [JSON](https://www.json.org/) e [MessagePack](https://msgpack.org/index.html). Cada protocolo tem opções de configuração de serialização.

Serialização JSON pode ser configurada no servidor usando o [ `AddJsonProtocol` ](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) método de extensão, que pode ser adicionado depois [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) no seu `Startup.ConfigureServices` método. O `AddJsonProtocol` método usa um delegado que recebe um `options` objeto. O [ `PayloadSerializerSettings` ](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) propriedade no objeto é um JSON.NET `JsonSerializerSettings` objeto que pode ser usado para configurar a serialização de argumentos e valores de retorno. Consulte o [JSON.NET documentação](https://www.newtonsoft.com/json/help/html/Introduction.htm) para obter mais detalhes.

Por exemplo, para configurar o serializador para usar nomes de propriedade "PascalCase", em vez dos nomes de "camelCase" padrão, use o seguinte código:

```csharp
services.AddSignalR()
    .AddJsonHubProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver = 
        new DefaultContractResolver();
    });
```

No cliente .NET, o mesmo `AddJsonHubProtocol` existe método de extensão no [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder). O `Microsoft.Extensions.DependencyInjection` namespace deve ser importado para resolver o método de extensão:

```csharp
// At the top of the file:
using Microsoft.Extensions.DependencyInjection;

// When constructing your connection:
var connection = new HubConnectionBuilder()
.AddJsonHubProtocol(options => {
    options.PayloadSerializerSettings.ContractResolver = 
        new DefaultContractResolver();
});
```

> [!NOTE]
> Não é possível configurar a serialização JSON em cliente JavaScript no momento.

### <a name="messagepack-serialization-options"></a>Opções de serialização MessagePack

Serialização de MessagePack pode ser configurada, fornecendo um delegado para o [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) chamar. Consulte [MessagePack no SignalR](xref:signalr/messagepackhubprotocol) para obter mais detalhes.

> [!NOTE]
> Não é possível configurar MessagePack serialização no cliente JavaScript no momento.

## <a name="configure-server-options"></a>Configurar opções de servidor

A tabela a seguir descreve as opções de configuração de hubs de SignalR:

| Opção | Descrição |
| ------ | ----------- |
| `HandshakeTimeout` | Se o cliente não enviar uma mensagem de handshake inicial dentro deste intervalo de tempo, a conexão será fechada. |
| `KeepAliveInterval` | Se o servidor ainda não enviou uma mensagem dentro deste intervalo, uma mensagem de ping é enviada automaticamente para manter a conexão aberta. |
| `SupportedProtocols` | Protocolos suportados por esse hub. Por padrão, todos os protocolos registrados no servidor são permitidos, mas protocolos podem ser removidos da lista para desabilitar protocolos específicos para os hubs individuais. |
| `EnableDetailedErrors` | Se `true`detalhados mensagens de exceção são retornadas aos clientes quando uma exceção é gerada em um método de Hub. O padrão é `false`, pois essas mensagens de exceção podem conter informações confidenciais. |

Opções podem ser configuradas para todos os hubs, fornecendo um delegado de opções para o `AddSignalR` chamar `Startup.ConfigureServices`.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSignalR(hubOptions =>
    {
        hubOptions.EnableDetailedErrors = true;
        hubOptions.KeepAliveInterval = TimeSpan.FromMinutes(1);
    })
}
```

Opções para um único hub substituem as opções de global fornecidas no `AddSignalR` e podem ser configuradas usando [ `AddHubOptions<T>` ](/dotnet/api/microsoft.extensions.dependencyinjection.huboptionsdependencyinjectionextensions.addhuboptions):

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
}
```

Use `HttpConnectionDispatcherOptions` para definir configurações avançadas relacionadas a transportes e gerenciamento de buffer de memória. Essas opções são configuradas passando um delegado para [ `MapHub<T>` ](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub).

| Opção | Descrição |
| ------ | ----------- |
| `ApplicationMaxBufferSize` | O número máximo de bytes recebidos do cliente que os buffers de servidor. Aumentar esse valor permite que o servidor receber mensagens maiores, mas pode afetar negativamente o consumo de memória. O valor padrão é 32KB. |
| `AuthorizationData` | Uma lista de [ `IAuthorizeData` ](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objetos usados para determinar se um cliente está autorizado a conectar-se ao hub. Por padrão, isso é preenchido com valores da `Authorize` atributos aplicados à classe Hub. |
| `TransportMaxBufferSize` | O número máximo de bytes enviados pelo aplicativo que os buffers de servidor. Aumentar esse valor permite que o servidor enviar mensagens maiores, mas pode afetar negativamente o consumo de memória. O valor padrão é 32KB. |
| `Transports` | Uma máscara de bits de `HttpTransportType` valores que possam restringir os transportes de um cliente pode usar para se conectar. Todos os transportes são habilitados por padrão. |
| `LongPolling` | Opções adicionais específicas para o transporte de sondagem longa. |
| `WebSockets` | Opções adicionais específicas para o transporte de WebSocket. |

O transporte de sondagem longa tem opções adicionais que podem ser configuradas usando o `LongPolling` propriedade:

| Opção | Descrição |
| ------ | ----------- |
| `PollTimeout` | A quantidade máxima de tempo que o servidor espera por uma mensagem para enviar ao cliente antes de encerrar uma solicitação de pesquisa único. Diminuir esse valor faz com que o cliente emita novas solicitações de sondagem com mais frequência. O valor padrão é 90 segundos. |

O transporte de WebSocket tem opções adicionais que podem ser configuradas usando o `WebSockets` propriedade:

| Opção | Descrição |
| ------ | ----------- |
| `CloseTimeout` | Depois de fecha o servidor, se o cliente não conseguir fechar dentro deste intervalo de tempo, a conexão será encerrada. |
| `SubProtocolSelector` | Um delegado que pode ser usado para definir o `Sec-WebSocket-Protocol` cabeçalho para um valor personalizado. O representante recebe os valores solicitados pelo cliente como entrada e deve retornar o valor desejado. |

## <a name="configure-client-options"></a>Configurar opções do cliente

Opções do cliente podem ser configuradas no `HubConnectionBuilder` tipo (disponível em clientes .NET e JavaScript), bem como no `HubConnection` em si.

### <a name="configure-logging"></a>Configurar registro em log

O log está configurado no cliente do .NET usando o `ConfigureLogging` método. Provedores e filtros de registro pode ser registrado da mesma maneira como eles estão no servidor. Consulte o [efetuar login no ASP.NET Core](xref:fundamentals/logging/index#how-to-add-providers) documentação para obter mais informações.

> [!NOTE]
> Para registrar provedores de log, você deve instalar os pacotes necessários. Consulte o [provedores de logs interno](xref:fundamentals/logging/index#built-in-logging-providers) seção de documentos para obter uma lista completa.

Por exemplo, para habilitar o log de Console, instale o `Microsoft.Extensions.Logging.Console` pacote NuGet. Chamar o `AddConsole` método de extensão:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

No cliente de JavaScript, semelhante `configureLogging` método existe. Forneça um `LogLevel` valor que indica o nível mínimo de mensagens de log para produzir. Logs são gravados na janela de console do navegador.

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

> [!NOTE]
> Para desabilitar o registro em log completamente, especifique `signalR.LogLevel.None` no `configureLogging` método.

Níveis de log disponível para o cliente JavaScript estão listados abaixo. Definir o nível de log para um desses valores habilita o log de mensagens no **ou acima** nível.

| Nível | Descrição |
| ----- | ----------- |
| `None` | Não são registradas. |
| `Critical` | Mensagens que indicam uma falha no aplicativo inteiro. |
| `Error` | Mensagens que indicam uma falha na operação atual. |
| `Warning` | Mensagens que indicam um problema não fatal. |
| `Information` | Mensagens informativas. |
| `Debug` | Mensagens de diagnóstico úteis para depuração. |
| `Trace` | Mensagens de diagnóstico muito detalhadas projetadas para diagnosticar problemas específicos. |

### <a name="configure-allowed-transports"></a>Configure transportes permitidos

Os transportes usados pelo SignalR podem ser configurados a `WithUrl` chamar (`withUrl` em JavaScript). Um bit a bit OR dos valores de `HttpTransportType` pode ser usado para restringir o cliente para usar somente os transportes especificados. Todos os transportes são habilitados por padrão.

Por exemplo, para desabilitar o transporte Server-Sent eventos, mas permita conexões WebSocket e sondagem longa:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

No cliente de JavaScript, os transportes são configurados, definindo o `transport` campo no objeto de opções fornecido para `withUrl`:

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

### <a name="configure-bearer-authentication"></a>Configurar a autenticação do portador

Para fornecer dados junto com o SignalR solicitações de autenticação, use o `AccessTokenProvider` opção (`accessTokenFactory` em JavaScript) para especificar uma função que retorna o token de acesso desejado. No cliente .NET, esse token de acesso é passado como um HTTP "Autenticação do portador" token (usando o `Authorization` cabeçalho com um tipo de `Bearer`). O cliente de JavaScript, o token de acesso é usado como um token de portador, **exceto** em alguns casos em que o navegador APIs restringir a capacidade de aplicar cabeçalhos (especificamente, em solicitações de eventos Server-Sent e WebSockets). Nesses casos, o token de acesso é fornecido como um valor de cadeia de caracteres de consulta `access_token`.

No cliente .NET, o `AccessTokenProvider` opção pode ser especificada usando o delegado de opções no `WithUrl`:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        }
    })
    .Build();
```

No cliente de JavaScript, o token de acesso está configurado, definindo o `accessTokenFactory` o objeto de opções no campo `withUrl`:

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        accessTokenFactory: () => {
            // Get and return the access token.
            // This function can return a JavaScript Promise if asynchronous
            // logic is required to retrieve the access token.
        }
    })
    .build();
```

### <a name="configure-timeout-and-keep-alive-options"></a>Configurar o tempo limite e opções de keep-alive

Opções adicionais para configurar o tempo limite e o comportamento de atividade estão disponíveis no `HubConnection` próprio objeto:

| Opção de .NET | Opção de JavaScript | Descrição |
| ----------- | ----------------- | ----------- |
| `ServerTimeout` | `serverTimeoutInMilliseconds` | Tempo limite para a atividade do servidor. Se o servidor ainda não enviou uma mensagem nesse intervalo, o cliente considera que o servidor desconectado e o gatilho de `Closed` evento (`onclose` em JavaScript). |
| `HandshakeTimeout` | Não é configurável | Tempo limite de handshake inicial do servidor. Se o servidor não enviar uma resposta de handshake nesse intervalo, o cliente cancela o handshake e gatilho o `Closed` evento (`onclose` em JavaScript). |

No cliente .NET, os valores de tempo limite são especificados como `TimeSpan` valores. No cliente de JavaScript, os valores de tempo limite são especificados como números. Os números representam valores de tempo em milissegundos.

### <a name="configure-additional-options"></a>Configurar opções adicionais

Opções adicionais podem ser configuradas no `WithUrl` (`withUrl` em JavaScript) método `HubConnectionBuilder`:

| Opção de .NET | Opção de JavaScript | Descrição |
| ----------- | ----------------- | ----------- |
| `AccessTokenProvider` | `accessTokenFactory` | Uma função que retorne uma cadeia de caracteres que é fornecida como um token de autenticação do portador em solicitações HTTP. |
| `SkipNegotiation` | `skipNegotiation` | Defina como `true` para ignorar a etapa de negociação. **Só tem suporte quando o transporte de WebSocket é o transporte habilitado somente**. Essa configuração não pode ser habilitada ao usar o serviço do Azure SignalR. |
| `ClientCertificates` | Não é configurável * | Uma coleção de certificados TLS para enviar para autenticar solicitações. |
| `Cookies` | Não é configurável * | Uma coleção de cookies HTTP para enviar com todas as solicitações HTTP. |
| `Credentials` | Não é configurável * | Credenciais para serem enviados com cada solicitação HTTP. |
| `CloseTimeout` | Não é configurável * | Somente o WebSocket. A quantidade máxima de tempo que o cliente aguardará depois de fechamento para o servidor confirmar a solicitação de fechamento. Se o servidor não reconhece o fechamento dentro desse período, o cliente se desconecta. |
| `Headers` | Não é configurável * | Um dicionário de cabeçalhos HTTP adicionais para serem enviados com cada solicitação HTTP. |
| `HttpMessageHandlerFactory` | Não é configurável * | Um delegado que pode ser usado para configurar ou substituir o `HttpMessageHandler` usado para enviar solicitações HTTP. Não é usada para conexões WebSocket. Este delegado deve retornar um valor não nulo e recebe o valor padrão como um parâmetro. Modificar configurações no valor padrão e retorná-lo ou retornar um completamente novo `HttpMessageHandler` instância. |
| `Proxy` | Não é configurável * | Um proxy HTTP para usar ao enviar solicitações HTTP. |
| `UseDefaultCredentials` | Não é configurável * | Defina esse valor booliano para enviar as credenciais padrão para solicitações HTTP e o WebSocket. Isso permite o uso da autenticação do Windows. |
| `WebSocketConfiguration` | Não é configurável * | Um delegado que pode ser usado para configurar opções adicionais de WebSocket. Recebe uma instância de [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) que pode ser usado para configurar as opções. |

Opções marcadas com um asterisco (*) não são configuráveis no cliente de JavaScript, devido a limitações no navegador APIs.

No cliente .NET, essas opções podem ser modificadas pelo delegado opções fornecido para `WithUrl`:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

No cliente de JavaScript, essas opções podem ser fornecidas em um objeto JavaScript fornecido para `withUrl`:

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    });
    .build();
```

## <a name="additional-resources"></a>Recursos adicionais

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>

---
title: Diferenças entre o SignalR e SignalR do ASP.NET Core
author: rachelappel
description: Diferenças entre o SignalR e SignalR do ASP.NET Core
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.date: 06/30/2018
uid: signalr/version-differences
ms.openlocfilehash: fd314d93333bd159aef4bb4863be50c646809cf0
ms.sourcegitcommit: 1faf2525902236428dae6a59e375519bafd5d6d7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/28/2018
ms.locfileid: "37090056"
---
# <a name="differences-between-signalr-and-aspnet-core-signalr"></a>Diferenças entre o SignalR e SignalR do ASP.NET Core

O SignalR do ASP.NET Core não é compatível com clientes ou servidores do ASP.NET SignalR. Este artigo fornece detalhes sobre os recursos que foram removidos ou alterados no SignalR do ASP.NET Core.

## <a name="feature-differences"></a>Diferenças de recursos

### <a name="automatic-reconnects"></a>Reconexão automática

Não há suporte para a reconexão automática. Anteriormente, o SignalR tentou reconectar ao servidor, se a conexão foi descartada. Agora, se o cliente for desconectado o usuário explicitamente deve iniciar uma nova conexão se desejar se reconectar.

### <a name="protocol-support"></a>Suporte de protocolo

O SignalR do ASP.NET Core dá suporte a JSON, bem como um novo protocolo binário com base em [MessagePack](xref:signalr/messagepackhubprotocol). Além disso, os protocolos personalizados podem ser criados.

## <a name="differences-on-the-server"></a>Diferenças no servidor

As bibliotecas do lado do servidor SignalR são incluídas no `Microsoft.AspNetCore.App` que faz parte do pacote de **aplicativo Web do ASP.NET Core** modelo para projetos Razor e MVC.

SignalR é um middleware ASP.NET Core, portanto ele deverá ser configurado por meio da chamada `AddSignalR` em `Startup.ConfigureServices`.

```csharp
services.AddSignalR();
```

Para configurar o roteamento, mapear rotas para hubs de dentro do `UseSignalR` da chamada do método `Startup.Configure` método.

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("/hub");
});
```

### <a name="sticky-sessions-now-required"></a>Sessões Autoadesivas agora

Devido a como expansão trabalhado em versões anteriores do SignalR, clientes podem se reconectar e enviar mensagens para qualquer servidor no farm. Devido a alterações para o modelo de expansão, como também não oferece suporte à reconexão, isso não é mais suportado. Agora, depois que o cliente se conecta ao servidor, ele precisa interagir com o mesmo servidor durante a conexão.

### <a name="single-hub-per-connection"></a>Hub único por conexão

No ASP.NET Core SignalR, o modelo de conexão foi simplificado. Conexões são feitas diretamente a um único hub, em vez de uma única conexão que está sendo usado para compartilhar o acesso a vários hubs.

### <a name="streaming"></a>Streaming

SignalR agora dá suporte a [fluxo de dados](xref:signalr/streaming) do hub para o cliente.

### <a name="state"></a>Estado

A capacidade de passar o estado arbitrário entre clientes e o hub (geralmente chamado de HubState) foi removida, bem como suporte para mensagens de progresso. Não há nenhum equivalente de proxies de hub no momento.

## <a name="differences-on-the-client"></a>Diferenças no cliente

### <a name="typescript"></a>TypeScript

A versão do ASP.NET Core do SignalR é gravada em [TypeScript](https://www.typescriptlang.org/). Você pode escrever em JavaScript ou TypeScript ao usar o [cliente JavaScript](xref:signalr/javascript-client).

### <a name="the-javascript-client-is-hosted-at-npmhttpswwwnpmjscom"></a>O cliente JavaScript é hospedado em [npm](https://www.npmjs.com/)

Nas versões anteriores, o cliente JavaScript foi obtido por meio de um pacote do NuGet no Visual Studio. Para as versões de núcleo, o [ @aspnet/signalr pacote npm](https://www.npmjs.com/package/@aspnet/signalr) contém as bibliotecas JavaScript. Este pacote não está incluído o **aplicativo Web do ASP.NET Core** modelo. Use npm para obter e instalar o `@aspnet/signalr` pacote npm.

```console
npm init -y
npm install @aspnet/signalr
```

### <a name="jquery"></a>jQuery

A dependência no jQuery foi removida, porém projetos ainda podem usar o jQuery.

### <a name="javascript-client-method-syntax"></a>Sintaxe de método de cliente JavaScript

A sintaxe JavaScript foi alterado da versão anterior do SignalR. Em vez de usar o `$connection` de objeto, criar uma conexão usando o `HubConnectionBuilder` API.

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub")
    .build();
```

Use `connection.on` para especificar os métodos de cliente que o hub pode chamar.

```javascript
connection.on("ReceiveMessage", (user, message) => {
    const msg = message.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
    const encodedMsg = user + " says " + msg;
    log(encodedMsg);
});
```

Depois de criar o método de cliente, inicie a conexão de hub. Cadeia um `catch` método para registrar ou manipular erros.

```javascript
connection.start().catch(err => console.error(err.toString()));
```

### <a name="hub-proxies"></a>Proxies do hub

Os proxies de Hub não automaticamente são gerados. Em vez disso, o nome do método é passado para o `invoke` API como uma cadeia de caracteres.

### <a name="net-and-other-clients"></a>.NET e outros clientes

O `Microsoft.AspNetCore.SignalR.Client` pacote NuGet contém as bibliotecas de cliente .NET para o SignalR do ASP.NET Core.

Use o `HubConnectionBuilder` gerar e criar uma instância de uma conexão a um hub.

```csharp
connection = new HubConnectionBuilder()
    .WithUrl("url")
    .Build();
```

## <a name="additional-resources"></a>Recursos adicionais

* [Hubs](xref:signalr/hubs)
* [Cliente JavaScript](xref:signalr/javascript-client)
* [Cliente .NET](xref:signalr/dotnet-client)
* [Plataformas compatíveis](xref:signalr/supported-platforms)

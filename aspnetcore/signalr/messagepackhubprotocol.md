---
title: Use o protocolo de Hub MessagePack no SignalR para ASP.NET Core
author: rachelappel
description: Adicione o protocolo de Hub MessagePack para o SignalR do ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 06/04/2018
uid: signalr/messagepackhubprotocol
ms.openlocfilehash: 702c77502868d6666cb2634b6959f029e036d14e
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274983"
---
# <a name="use-messagepack-hub-protocol-in-signalr-for-aspnet-core"></a>Use o protocolo de Hub MessagePack no SignalR para ASP.NET Core

Por [Brennan Conroy](https://github.com/BrennanConroy)

Este artigo pressupõe que o leitor esteja familiarizado com os tópicos abordados [começar](xref:tutorials/signalr).

## <a name="what-is-messagepack"></a>O que é MessagePack?

[MessagePack](https://msgpack.org/index.html) é um formato de serialização binária que é rápido e compact. Ela é útil quando o desempenho e a largura de banda são uma preocupação porque ele cria mensagens menores em comparação comparadas [JSON](https://www.json.org/). Como é um formato binário, mensagens são ilegíveis ao examinar os logs e os rastreamentos de rede, a menos que os bytes são passados por meio de um analisador de MessagePack. O SignalR tem suporte interno para o formato de MessagePack e fornece APIs de cliente e servidor usar.

## <a name="configure-messagepack-on-the-server"></a>Configurar MessagePack no servidor

Para habilitar o protocolo de Hub MessagePack no servidor, instale o `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` pacote em seu aplicativo. Adiciona o arquivo Startup.cs `AddMessagePackProtocol` para o `AddSignalR` chamada para habilitar o suporte de MessagePack no servidor.

> [!NOTE]
> JSON é habilitado por padrão. Adicionar MessagePack habilita o suporte para clientes JSON e MessagePack.

```csharp
services.AddSignalR()
    .AddMessagePackProtocol();
```

Para personalizar como MessagePack formatará a seus dados, `AddMessagePackProtocol` usa um delegado para configurar as opções. Esse delegado, o `FormatterResolvers` propriedade pode ser usada para configurar opções de serialização MessagePack. Para obter mais informações sobre como funcionam os resolvedores, visite a biblioteca MessagePack em [MessagePack CSharp](https://github.com/neuecc/MessagePack-CSharp). Atributos podem ser usados nos objetos que deseja serializar para definir como eles devem ser tratados.

```csharp
services.AddSignalR()
    .AddMessagePackProtocol(options =>
    {
        options.FormatterResolvers = new List<MessagePack.IFormatterResolver>()
        {
            MessagePack.Resolvers.StandardResolver.Instance
        };
    });
```

## <a name="configure-messagepack-on-the-client"></a>Configurar MessagePack no cliente

### <a name="net-client"></a>Cliente .NET

Para habilitar MessagePack no cliente .NET, instale o `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` pacote e chame `AddMessagePackProtocol` em `HubConnectionBuilder`.

```csharp
var hubConnection = new HubConnectionBuilder()
                        .WithUrl("/chatHub")
                        .AddMessagePackProtocol()
                        .Build();
```

> [!NOTE]
> Isso `AddMessagePackProtocol` chamada usa um delegado para configurar opções como o servidor.

### <a name="javascript-client"></a>Cliente JavaScript

MessagePack suporte para o cliente Javascript é fornecido pelo `@aspnet/signalr-protocol-msgpack` pacote NPM.

```console
npm install @aspnet/signalr-protocol-msgpack
```

Depois de instalar o pacote npm, o módulo pode ser usado diretamente por meio de um carregador de módulo de JavaScript ou importado para o navegador referenciando o *node_modules\\ @aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js*  arquivo. Em um navegador de `msgpack5` biblioteca também deve ser referenciada. Use um `<script>` marca para criar uma referência. A biblioteca pode ser encontrada em *node_modules\msgpack5\dist\msgpack5.js*.

> [!NOTE]
> Ao usar o `<script>` elemento, a ordem é importante. Se *signalr de protocolo de msgpack.js* é referenciado antes *msgpack5.js*, ocorrerá um erro ao tentar se conectar com MessagePack. *SignalR.js* também é necessária antes de *signalr de protocolo de msgpack.js*.

```html
<script src="~/lib/signalr/signalr.js"></script>
<script src="~/lib/msgpack5/msgpack5.js"></script>
<script src="~/lib/signalr/signalr-protocol-msgpack.js"></script>
```

Adicionando `.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())` para o `HubConnectionBuilder` irá configurar o cliente para usar o protocolo MessagePack ao se conectar a um servidor.

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())
    .build();
```

> [!NOTE]
> Neste momento, não há nenhuma opção de configuração para o protocolo MessagePack no cliente JavaScript.

## <a name="related-resources"></a>Recursos relacionados

* [Introdução](xref:tutorials/signalr)
* [Cliente .NET](xref:signalr/dotnet-client)
* [Cliente JavaScript](xref:signalr/javascript-client)

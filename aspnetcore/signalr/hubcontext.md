---
title: SignalR HubContext
author: rachelappel
description: Saiba como usar o serviço ASP.NET Core SignalR HubContext para enviar notificações para clientes externos um hub.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 06/13/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/hubcontext
ms.openlocfilehash: 79b91a776a38a2e6810cc89ff0b8d15fe836ce66
ms.sourcegitcommit: 9a35906446af7ffd4ccfc18daec38874b5abbef7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/18/2018
ms.locfileid: "35726064"
---
# <a name="send-messages-from-outside-a-hub"></a>Enviar mensagens de fora de um hub

Por [Mikael Mengistu](https://twitter.com/MikaelM_12)

O hub SignalR é a abstração de núcleo para enviar mensagens para clientes conectados ao servidor do SignalR. Também é possível enviar mensagens em outros locais do seu aplicativo usando o `IHubContext` service. Este artigo explica como acessar um SignalR `IHubContext` para enviar notificações aos clientes de fora de um hub.

[Exibir ou baixar o código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubcontext/sample/) [(como fazer o download)](xref:tutorials/index#how-to-download-a-sample)

## <a name="get-an-instance-of-ihubcontext"></a>Obter uma instância de `IHubContext`

No ASP.NET Core SignalR, você pode acessar uma instância de `IHubContext` por meio de injeção de dependência. Você pode inserir uma instância de `IHubContext` em um controlador, middleware ou outro serviço de injeção de dependência. Use a instância para enviar mensagens aos clientes.

> [!NOTE]
> Isso é diferente do ASP.NET SignalR que usada GlobalHost para fornecer acesso para o `IHubContext`. ASP.NET Core tem uma estrutura de injeção de dependência que elimina a necessidade de neste singleton global.

### <a name="inject-an-instance-of-ihubcontext-in-a-controller"></a>Inserir uma instância de `IHubContext` em um controlador

Você pode inserir uma instância de `IHubContext` em um controlador, adicionando-o para seu construtor:

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=12-19,57)]

Agora, com acesso a uma instância de `IHubContext`, você pode chamar métodos de hub, como se estivesse no hub de si mesmo.

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=21-25)]

### <a name="get-an-instance-of-ihubcontext-in-middleware"></a>Obter uma instância de `IHubContext` no middleware

Acesso a `IHubContext` no pipeline de middleware da seguinte forma:

```csharp
app.Use(next => (context) =>
{
    var hubContext = (IHubContext<MyHub>)context
                        .RequestServices
                        .GetServices<IHubContext<MyHub>>();
    //...
});
```

> [!NOTE]
> Quando os métodos de hub são chamados de fora do `Hub` de classe, não há nenhum chamador associado com a invocação. Portanto, não há nenhum acesso para o `ConnectionId`, `Caller`, e `Others` propriedades.

## <a name="related-resources"></a>Recursos relacionados

* [Introdução](xref:signalr/get-started)
* [Hubs](xref:signalr/hubs)
* [Publicar no Azure](xref:signalr/publish-to-azure-web-app)

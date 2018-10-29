---
title: O SignalR HubContext
author: tdykstra
description: Saiba como usar o serviço HubContext de SignalR do ASP.NET Core para enviar notificações para clientes externos um hub.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/13/2018
uid: signalr/hubcontext
ms.openlocfilehash: 8be888e1f7b16d65ebbaa24b618e84fca029d80b
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207947"
---
# <a name="send-messages-from-outside-a-hub"></a>Enviar mensagens de fora de um hub

Por [Mikael Mengistu](https://twitter.com/MikaelM_12)

O hub do SignalR é a abstração central para enviar mensagens para os clientes conectados ao servidor SignalR. Também é possível enviar mensagens de outros lugares no seu aplicativo usando o `IHubContext` service. Este artigo explica como acessar um SignalR `IHubContext` para enviar notificações para clientes externos um hub.

[Exibir ou baixar o código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubcontext/sample/) [(como fazer o download)](xref:index#how-to-download-a-sample)

## <a name="get-an-instance-of-ihubcontext"></a>Obtenha uma instância de IHubContext

No SignalR do ASP.NET Core, você pode acessar uma instância de `IHubContext` por meio da injeção de dependência. Você pode injetar uma instância do `IHubContext` em um controlador, middleware ou outro serviço de injeção de dependência. Use a instância para enviar mensagens para os clientes.

> [!NOTE]
> Isso é diferente do ASP.NET 4.x SignalR que usado GlobalHost para fornecer acesso ao `IHubContext`. O ASP.NET Core tem uma estrutura de injeção de dependência que remove a necessidade de neste singleton global.

### <a name="inject-an-instance-of-ihubcontext-in-a-controller"></a>Injetar uma instância do IHubContext em um controlador

Você pode injetar uma instância do `IHubContext` em um controlador, adicionando-o para seu construtor:

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=12-19,57)]

Agora, com acesso a uma instância de `IHubContext`, você pode chamar métodos de hub, como se estivessem no próprio hub.

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=21-25)]

### <a name="get-an-instance-of-ihubcontext-in-middleware"></a>Obtenha uma instância de IHubContext no middleware

Acesso a `IHubContext` dentro do pipeline de middleware da seguinte forma:

```csharp
app.Use(next => async (context) =>
{
    var hubContext = context.RequestServices
                            .GetRequiredService<IHubContext<MyHub>>();
    //...
});
```

> [!NOTE]
> Quando os métodos de hub são chamados de fora do `Hub` de classe, não há nenhum chamador associado com a invocação. Portanto, não há nenhum acesso para o `ConnectionId`, `Caller`, e `Others` propriedades.

## <a name="related-resources"></a>Recursos relacionados

* [Introdução](xref:tutorials/signalr)
* [Hubs](xref:signalr/hubs)
* [Publicar no Azure](xref:signalr/publish-to-azure-web-app)

---
title: O SignalR HubContext
author: tdykstra
description: Saiba como usar o serviço HubContext de SignalR do ASP.NET Core para enviar notificações para clientes externos um hub.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/13/2018
uid: signalr/hubcontext
ms.openlocfilehash: bb07a3b5c6e153092635fa4e1283619777865a53
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/15/2018
ms.locfileid: "49325348"
---
# <a name="send-messages-from-outside-a-hub"></a><span data-ttu-id="0b358-103">Enviar mensagens de fora de um hub</span><span class="sxs-lookup"><span data-stu-id="0b358-103">Send messages from outside a hub</span></span>

<span data-ttu-id="0b358-104">Por [Mikael Mengistu](https://twitter.com/MikaelM_12)</span><span class="sxs-lookup"><span data-stu-id="0b358-104">By [Mikael Mengistu](https://twitter.com/MikaelM_12)</span></span>

<span data-ttu-id="0b358-105">O hub do SignalR é a abstração central para enviar mensagens para os clientes conectados ao servidor SignalR.</span><span class="sxs-lookup"><span data-stu-id="0b358-105">The SignalR hub is the core abstraction for sending messages to clients connected to the SignalR server.</span></span> <span data-ttu-id="0b358-106">Também é possível enviar mensagens de outros lugares no seu aplicativo usando o `IHubContext` service.</span><span class="sxs-lookup"><span data-stu-id="0b358-106">It's also possible to send messages from other places in your app using the `IHubContext` service.</span></span> <span data-ttu-id="0b358-107">Este artigo explica como acessar um SignalR `IHubContext` para enviar notificações para clientes externos um hub.</span><span class="sxs-lookup"><span data-stu-id="0b358-107">This article explains how to access a SignalR `IHubContext` to send notifications to clients from outside a hub.</span></span>

<span data-ttu-id="0b358-108">[Exibir ou baixar o código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubcontext/sample/) [(como fazer o download)](xref:tutorials/index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="0b358-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubcontext/sample/) [(how to download)](xref:tutorials/index#how-to-download-a-sample)</span></span>

## <a name="get-an-instance-of-ihubcontext"></a><span data-ttu-id="0b358-109">Obtenha uma instância de IHubContext</span><span class="sxs-lookup"><span data-stu-id="0b358-109">Get an instance of IHubContext</span></span>

<span data-ttu-id="0b358-110">No SignalR do ASP.NET Core, você pode acessar uma instância de `IHubContext` por meio da injeção de dependência.</span><span class="sxs-lookup"><span data-stu-id="0b358-110">In ASP.NET Core SignalR, you can access an instance of `IHubContext` via dependency injection.</span></span> <span data-ttu-id="0b358-111">Você pode injetar uma instância do `IHubContext` em um controlador, middleware ou outro serviço de injeção de dependência.</span><span class="sxs-lookup"><span data-stu-id="0b358-111">You can inject an instance of `IHubContext` into a controller, middleware, or other DI service.</span></span> <span data-ttu-id="0b358-112">Use a instância para enviar mensagens para os clientes.</span><span class="sxs-lookup"><span data-stu-id="0b358-112">Use the instance to send messages to clients.</span></span>

> [!NOTE]
> <span data-ttu-id="0b358-113">Isso é diferente do ASP.NET 4.x SignalR que usado GlobalHost para fornecer acesso ao `IHubContext`.</span><span class="sxs-lookup"><span data-stu-id="0b358-113">This differs from ASP.NET 4.x SignalR which used GlobalHost to provide access to the `IHubContext`.</span></span> <span data-ttu-id="0b358-114">O ASP.NET Core tem uma estrutura de injeção de dependência que remove a necessidade de neste singleton global.</span><span class="sxs-lookup"><span data-stu-id="0b358-114">ASP.NET Core has a dependency injection framework that removes the need for this global singleton.</span></span>

### <a name="inject-an-instance-of-ihubcontext-in-a-controller"></a><span data-ttu-id="0b358-115">Injetar uma instância do IHubContext em um controlador</span><span class="sxs-lookup"><span data-stu-id="0b358-115">Inject an instance of IHubContext in a controller</span></span>

<span data-ttu-id="0b358-116">Você pode injetar uma instância do `IHubContext` em um controlador, adicionando-o para seu construtor:</span><span class="sxs-lookup"><span data-stu-id="0b358-116">You can inject an instance of `IHubContext` into a controller by adding it to your constructor:</span></span>

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=12-19,57)]

<span data-ttu-id="0b358-117">Agora, com acesso a uma instância de `IHubContext`, você pode chamar métodos de hub, como se estivessem no próprio hub.</span><span class="sxs-lookup"><span data-stu-id="0b358-117">Now, with access to an instance of `IHubContext`, you can call hub methods as if you were in the hub itself.</span></span>

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=21-25)]

### <a name="get-an-instance-of-ihubcontext-in-middleware"></a><span data-ttu-id="0b358-118">Obtenha uma instância de IHubContext no middleware</span><span class="sxs-lookup"><span data-stu-id="0b358-118">Get an instance of IHubContext in middleware</span></span>

<span data-ttu-id="0b358-119">Acesso a `IHubContext` dentro do pipeline de middleware da seguinte forma:</span><span class="sxs-lookup"><span data-stu-id="0b358-119">Access the `IHubContext` within the middleware pipeline like so:</span></span>

```csharp
app.Use(next => async (context) =>
{
    var hubContext = context.RequestServices
                            .GetRequiredService<IHubContext<MyHub>>();
    //...
});
```

> [!NOTE]
> <span data-ttu-id="0b358-120">Quando os métodos de hub são chamados de fora do `Hub` de classe, não há nenhum chamador associado com a invocação.</span><span class="sxs-lookup"><span data-stu-id="0b358-120">When hub methods are called from outside of the `Hub` class, there's no caller associated with the invocation.</span></span> <span data-ttu-id="0b358-121">Portanto, não há nenhum acesso para o `ConnectionId`, `Caller`, e `Others` propriedades.</span><span class="sxs-lookup"><span data-stu-id="0b358-121">Therefore, there's no access to the `ConnectionId`, `Caller`, and `Others` properties.</span></span>

## <a name="related-resources"></a><span data-ttu-id="0b358-122">Recursos relacionados</span><span class="sxs-lookup"><span data-stu-id="0b358-122">Related resources</span></span>

* [<span data-ttu-id="0b358-123">Introdução</span><span class="sxs-lookup"><span data-stu-id="0b358-123">Get started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="0b358-124">Hubs</span><span class="sxs-lookup"><span data-stu-id="0b358-124">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="0b358-125">Publicar no Azure</span><span class="sxs-lookup"><span data-stu-id="0b358-125">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)

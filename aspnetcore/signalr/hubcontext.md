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
# <a name="send-messages-from-outside-a-hub"></a><span data-ttu-id="6a47a-103">Enviar mensagens de fora de um hub</span><span class="sxs-lookup"><span data-stu-id="6a47a-103">Send messages from outside a hub</span></span>

<span data-ttu-id="6a47a-104">Por [Mikael Mengistu](https://twitter.com/MikaelM_12)</span><span class="sxs-lookup"><span data-stu-id="6a47a-104">By [Mikael Mengistu](https://twitter.com/MikaelM_12)</span></span>

<span data-ttu-id="6a47a-105">O hub SignalR é a abstração de núcleo para enviar mensagens para clientes conectados ao servidor do SignalR.</span><span class="sxs-lookup"><span data-stu-id="6a47a-105">The SignalR hub is the core abstraction for sending messages to clients connected to the SignalR server.</span></span> <span data-ttu-id="6a47a-106">Também é possível enviar mensagens em outros locais do seu aplicativo usando o `IHubContext` service.</span><span class="sxs-lookup"><span data-stu-id="6a47a-106">It's also possible to send messages from other places in your app using the `IHubContext` service.</span></span> <span data-ttu-id="6a47a-107">Este artigo explica como acessar um SignalR `IHubContext` para enviar notificações aos clientes de fora de um hub.</span><span class="sxs-lookup"><span data-stu-id="6a47a-107">This article explains how to access a SignalR `IHubContext` to send notifications to clients from outside a hub.</span></span>

<span data-ttu-id="6a47a-108">[Exibir ou baixar o código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubcontext/sample/) [(como fazer o download)](xref:tutorials/index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="6a47a-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubcontext/sample/) [(how to download)](xref:tutorials/index#how-to-download-a-sample)</span></span>

## <a name="get-an-instance-of-ihubcontext"></a><span data-ttu-id="6a47a-109">Obter uma instância de `IHubContext`</span><span class="sxs-lookup"><span data-stu-id="6a47a-109">Get an instance of `IHubContext`</span></span>

<span data-ttu-id="6a47a-110">No ASP.NET Core SignalR, você pode acessar uma instância de `IHubContext` por meio de injeção de dependência.</span><span class="sxs-lookup"><span data-stu-id="6a47a-110">In ASP.NET Core SignalR, you can access an instance of `IHubContext` via dependency injection.</span></span> <span data-ttu-id="6a47a-111">Você pode inserir uma instância de `IHubContext` em um controlador, middleware ou outro serviço de injeção de dependência.</span><span class="sxs-lookup"><span data-stu-id="6a47a-111">You can inject an instance of `IHubContext` into a controller, middleware, or other DI service.</span></span> <span data-ttu-id="6a47a-112">Use a instância para enviar mensagens aos clientes.</span><span class="sxs-lookup"><span data-stu-id="6a47a-112">Use the instance to send messages to clients.</span></span>

> [!NOTE]
> <span data-ttu-id="6a47a-113">Isso é diferente do ASP.NET SignalR que usada GlobalHost para fornecer acesso para o `IHubContext`.</span><span class="sxs-lookup"><span data-stu-id="6a47a-113">This differs from ASP.NET SignalR which used GlobalHost to provide access to the `IHubContext`.</span></span> <span data-ttu-id="6a47a-114">ASP.NET Core tem uma estrutura de injeção de dependência que elimina a necessidade de neste singleton global.</span><span class="sxs-lookup"><span data-stu-id="6a47a-114">ASP.NET Core has a dependency injection framework that removes the need for this global singleton.</span></span>

### <a name="inject-an-instance-of-ihubcontext-in-a-controller"></a><span data-ttu-id="6a47a-115">Inserir uma instância de `IHubContext` em um controlador</span><span class="sxs-lookup"><span data-stu-id="6a47a-115">Inject an instance of `IHubContext` in a controller</span></span>

<span data-ttu-id="6a47a-116">Você pode inserir uma instância de `IHubContext` em um controlador, adicionando-o para seu construtor:</span><span class="sxs-lookup"><span data-stu-id="6a47a-116">You can inject an instance of `IHubContext` into a controller by adding it to your constructor:</span></span>

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=12-19,57)]

<span data-ttu-id="6a47a-117">Agora, com acesso a uma instância de `IHubContext`, você pode chamar métodos de hub, como se estivesse no hub de si mesmo.</span><span class="sxs-lookup"><span data-stu-id="6a47a-117">Now, with access to an instance of `IHubContext`, you can call hub methods as if you were in the hub itself.</span></span>

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=21-25)]

### <a name="get-an-instance-of-ihubcontext-in-middleware"></a><span data-ttu-id="6a47a-118">Obter uma instância de `IHubContext` no middleware</span><span class="sxs-lookup"><span data-stu-id="6a47a-118">Get an instance of `IHubContext` in middleware</span></span>

<span data-ttu-id="6a47a-119">Acesso a `IHubContext` no pipeline de middleware da seguinte forma:</span><span class="sxs-lookup"><span data-stu-id="6a47a-119">Access the `IHubContext` within the middleware pipeline like so:</span></span>

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
> <span data-ttu-id="6a47a-120">Quando os métodos de hub são chamados de fora do `Hub` de classe, não há nenhum chamador associado com a invocação.</span><span class="sxs-lookup"><span data-stu-id="6a47a-120">When hub methods are called from outside of the `Hub` class, there's no caller associated with the invocation.</span></span> <span data-ttu-id="6a47a-121">Portanto, não há nenhum acesso para o `ConnectionId`, `Caller`, e `Others` propriedades.</span><span class="sxs-lookup"><span data-stu-id="6a47a-121">Therefore, there's no access to the `ConnectionId`, `Caller`, and `Others` properties.</span></span>

## <a name="related-resources"></a><span data-ttu-id="6a47a-122">Recursos relacionados</span><span class="sxs-lookup"><span data-stu-id="6a47a-122">Related resources</span></span>

* [<span data-ttu-id="6a47a-123">Introdução</span><span class="sxs-lookup"><span data-stu-id="6a47a-123">Get started</span></span>](xref:signalr/get-started)
* [<span data-ttu-id="6a47a-124">Hubs</span><span class="sxs-lookup"><span data-stu-id="6a47a-124">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="6a47a-125">Publicar no Azure</span><span class="sxs-lookup"><span data-stu-id="6a47a-125">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)

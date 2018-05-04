---
title: Usando os hubs de SignalR do ASP.NET Core
author: rachelappel
description: Saiba como usar hubs de SignalR do ASP.NET Core.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 05/01/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/hubs
ms.openlocfilehash: e23d7ef6d5e5e93d5fc69ad4c845a6a896836170
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/03/2018
---
# <a name="use-hubs-in-signalr-for-aspnet-core"></a><span data-ttu-id="c101c-103">Usando os hubs de SignalR para ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c101c-103">Use hubs in SignalR for ASP.NET Core</span></span>

<span data-ttu-id="c101c-104">Por [Rachel Appel](https://twitter.com/rachelappel) e [Griffin Kevin](https://twitter.com/1kevgriff)</span><span class="sxs-lookup"><span data-stu-id="c101c-104">By [Rachel Appel](https://twitter.com/rachelappel) and [Kevin Griffin](https://twitter.com/1kevgriff)</span></span>

[!INCLUDE [2.1 preview notice](~/includes/2.1.md)]

<span data-ttu-id="c101c-105">[Exibir ou baixar o código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(como fazer o download)](xref:tutorials/index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="c101c-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(how to download)](xref:tutorials/index#how-to-download-a-sample)</span></span>

## <a name="what-is-a-signalr-hub"></a><span data-ttu-id="c101c-106">O que é um hub SignalR</span><span class="sxs-lookup"><span data-stu-id="c101c-106">What is a SignalR hub</span></span>

<span data-ttu-id="c101c-107">A API de Hubs de SignalR permite chamar métodos em clientes conectados do servidor.</span><span class="sxs-lookup"><span data-stu-id="c101c-107">The SignalR Hubs API enables you to call methods on connected clients from the server.</span></span> <span data-ttu-id="c101c-108">No código de servidor, você define os métodos que são chamados pelo cliente.</span><span class="sxs-lookup"><span data-stu-id="c101c-108">In the server code, you define methods that are called by client.</span></span> <span data-ttu-id="c101c-109">No código do cliente, você define os métodos que são chamados do servidor.</span><span class="sxs-lookup"><span data-stu-id="c101c-109">In the client code, you define methods that are called from the server.</span></span> <span data-ttu-id="c101c-110">SignalR cuida de tudo nos bastidores que possibilita a comunicação de cliente para servidor e servidor-para-cliente em tempo real.</span><span class="sxs-lookup"><span data-stu-id="c101c-110">SignalR takes care of everything behind the scenes that makes real-time client-to-server and server-to-client communications possible.</span></span>

## <a name="configure-signalr-hubs"></a><span data-ttu-id="c101c-111">Configurar os hubs de SignalR</span><span class="sxs-lookup"><span data-stu-id="c101c-111">Configure SignalR hubs</span></span>

<span data-ttu-id="c101c-112">O middleware SignalR requer alguns serviços, que são configurados por meio da chamada `services.AddSignalR`.</span><span class="sxs-lookup"><span data-stu-id="c101c-112">The SignalR middleware requires some services, which are configured by calling `services.AddSignalR`.</span></span>

[!code-csharp[Configure service](hubs/sample/startup.cs?range=37)]

<span data-ttu-id="c101c-113">Ao adicionar a funcionalidade de SignalR para um aplicativo ASP.NET Core, configurar rotas SignalR chamando `app.UseSignalR` no `Startup.Configure` método.</span><span class="sxs-lookup"><span data-stu-id="c101c-113">When adding SignalR functionality to an ASP.NET Core app, setup SignalR routes by calling `app.UseSignalR` in the `Startup.Configure` method.</span></span>

[!code-csharp[Configure routes to hubs](hubs/sample/startup.cs?range=56-59)]

## <a name="create-and-use-hubs"></a><span data-ttu-id="c101c-114">Criar e usar hubs</span><span class="sxs-lookup"><span data-stu-id="c101c-114">Create and use hubs</span></span>

<span data-ttu-id="c101c-115">Criar um hub declarando uma classe que herda de `Hub`e adicionar métodos públicos a ele.</span><span class="sxs-lookup"><span data-stu-id="c101c-115">Create a hub by declaring a class that inherits from `Hub`, and add public methods to it.</span></span> <span data-ttu-id="c101c-116">Os clientes podem chamar os métodos que são definidos como `public`.</span><span class="sxs-lookup"><span data-stu-id="c101c-116">Clients can call methods that are defined as `public`.</span></span>

[!code-csharp[Create and use hubs](hubs/sample/hubs/chathub.cs?range=8-37)]

<span data-ttu-id="c101c-117">Você pode especificar um tipo de retorno e parâmetros, incluindo tipos complexos e matrizes, como você faria em qualquer método em c#.</span><span class="sxs-lookup"><span data-stu-id="c101c-117">You can specify a return type and parameters, including complex types and arrays, as you would in any C# method.</span></span> <span data-ttu-id="c101c-118">SignalR lida com a serialização e desserialização de objetos complexos e matrizes em seus parâmetros e valores de retorno.</span><span class="sxs-lookup"><span data-stu-id="c101c-118">SignalR handles the serialization and deserialization of complex objects and arrays in your parameters and return values.</span></span>

## <a name="the-clients-object"></a><span data-ttu-id="c101c-119">O objeto de clientes</span><span class="sxs-lookup"><span data-stu-id="c101c-119">The Clients object</span></span>

<span data-ttu-id="c101c-120">Cada instância do `Hub` classe tem uma propriedade denominada `Clients` que contém os seguintes membros para a comunicação entre cliente e servidor:</span><span class="sxs-lookup"><span data-stu-id="c101c-120">Each instance of the `Hub` class has a property named `Clients` that contains the following members for communication between server and client:</span></span>

| <span data-ttu-id="c101c-121">Propriedade</span><span class="sxs-lookup"><span data-stu-id="c101c-121">Property</span></span> | <span data-ttu-id="c101c-122">Descrição</span><span class="sxs-lookup"><span data-stu-id="c101c-122">Description</span></span> |
| ------ | ----------- |
| `All` | <span data-ttu-id="c101c-123">Chama um método em todos os clientes conectados</span><span class="sxs-lookup"><span data-stu-id="c101c-123">Calls a method on all connected clients</span></span> |
| `Caller` | <span data-ttu-id="c101c-124">Chama um método no cliente que invocou o método de hub</span><span class="sxs-lookup"><span data-stu-id="c101c-124">Calls a method on the client that invoked the hub method</span></span> |
| `Others` | <span data-ttu-id="c101c-125">Chama um método em todos os clientes conectados, exceto o cliente que invocou o método</span><span class="sxs-lookup"><span data-stu-id="c101c-125">Calls a method on all connected clients except the client that invoked the method</span></span> |

<span data-ttu-id="c101c-126">Além disso, a `Hub` classe contém os seguintes métodos:</span><span class="sxs-lookup"><span data-stu-id="c101c-126">Additionally, the `Hub` class contains the following methods:</span></span>

| <span data-ttu-id="c101c-127">Método</span><span class="sxs-lookup"><span data-stu-id="c101c-127">Method</span></span> | <span data-ttu-id="c101c-128">Descrição</span><span class="sxs-lookup"><span data-stu-id="c101c-128">Description</span></span> |
| ------ | ----------- |
| `AllExcept` | <span data-ttu-id="c101c-129">Chama um método em todos os clientes conectados, exceto para as conexões especificadas</span><span class="sxs-lookup"><span data-stu-id="c101c-129">Calls a method on all connected clients except for the specified connections</span></span> |
| `Client` | <span data-ttu-id="c101c-130">Chama um método em um cliente conectado específico</span><span class="sxs-lookup"><span data-stu-id="c101c-130">Calls a method on a specific connected client</span></span> |
| `Clients` | <span data-ttu-id="c101c-131">Chama um método específicos clientes conectados</span><span class="sxs-lookup"><span data-stu-id="c101c-131">Calls a method on specific connected clients</span></span> |
| `Group` | <span data-ttu-id="c101c-132">Envia uma mensagem para todas as conexões no grupo especificado</span><span class="sxs-lookup"><span data-stu-id="c101c-132">Sends a message to all connections in the specified group</span></span>  |
| `GroupExcept` | <span data-ttu-id="c101c-133">Envia uma mensagem para todas as conexões no grupo especificado, exceto as conexões especificadas</span><span class="sxs-lookup"><span data-stu-id="c101c-133">Sends a message to all connections in the specified group, except the specified connections</span></span> |
| `Groups` | <span data-ttu-id="c101c-134">Envia uma mensagem para vários grupos de conexões</span><span class="sxs-lookup"><span data-stu-id="c101c-134">Sends a message to multiple groups of connections</span></span>  |
| `OthersInGroup` | <span data-ttu-id="c101c-135">Envia uma mensagem para um grupo de conexões, excluindo o cliente que invocou o método de hub</span><span class="sxs-lookup"><span data-stu-id="c101c-135">Sends a message to a group of connections, excluding the client that invoked the hub method</span></span>  |
| `User` | <span data-ttu-id="c101c-136">Envia uma mensagem para todas as conexões associadas a um usuário específico</span><span class="sxs-lookup"><span data-stu-id="c101c-136">Sends a message to all connections associated with a specific user</span></span> |
| `Users` | <span data-ttu-id="c101c-137">Envia uma mensagem para todas as conexões associadas com os usuários especificados</span><span class="sxs-lookup"><span data-stu-id="c101c-137">Sends a message to all connections associated with the specified users</span></span> |

<span data-ttu-id="c101c-138">Cada propriedade ou método nas tabelas anteriores retorna um objeto com um `SendAsync` método.</span><span class="sxs-lookup"><span data-stu-id="c101c-138">Each property or method in the preceding tables returns an object with a `SendAsync` method.</span></span> <span data-ttu-id="c101c-139">O `SendAsync` método permite que você forneça o nome e os parâmetros do método de cliente para chamar.</span><span class="sxs-lookup"><span data-stu-id="c101c-139">The `SendAsync` method allows you to supply the name and parameters of the client method to call.</span></span>

## <a name="send-messages-to-clients"></a><span data-ttu-id="c101c-140">Enviar mensagens para os clientes</span><span class="sxs-lookup"><span data-stu-id="c101c-140">Send messages to clients</span></span>

<span data-ttu-id="c101c-141">Para fazer chamadas para clientes específicos, use as propriedades do `Clients` objeto.</span><span class="sxs-lookup"><span data-stu-id="c101c-141">To make calls to specific clients, use the properties of the `Clients` object.</span></span> <span data-ttu-id="c101c-142">A seguir no exemplo a seguir, o `SendMessageToCaller` método demonstra enviando uma mensagem para a conexão que invocou o método de hub.</span><span class="sxs-lookup"><span data-stu-id="c101c-142">In the following In the following example, the `SendMessageToCaller` method demonstrates sending a message to the connection that invoked the hub method.</span></span> <span data-ttu-id="c101c-143">O `SendMessageToGroups` método envia uma mensagem para os grupos armazenados em um `List` chamado `groups`.</span><span class="sxs-lookup"><span data-stu-id="c101c-143">The `SendMessageToGroups` method sends a message to the groups stored in a `List` named `groups`.</span></span>

[!code-csharp[Send messages](hubs/sample/hubs/chathub.cs?range=15-24)]

## <a name="handle-events-for-a-connection"></a><span data-ttu-id="c101c-144">Manipular eventos para uma conexão</span><span class="sxs-lookup"><span data-stu-id="c101c-144">Handle events for a connection</span></span>

<span data-ttu-id="c101c-145">A API de Hubs de SignalR fornece o `OnConnectedAsync` e `OnDisconnectedAsync` métodos virtuais para gerenciar e controlar conexões.</span><span class="sxs-lookup"><span data-stu-id="c101c-145">The SignalR Hubs API provides the `OnConnectedAsync` and `OnDisconnectedAsync` virtual methods to manage and track connections.</span></span> <span data-ttu-id="c101c-146">Substituir o `OnConnectedAsync` método virtual para executar ações quando um cliente se conecta ao Hub, como adicioná-lo a um grupo.</span><span class="sxs-lookup"><span data-stu-id="c101c-146">Override the `OnConnectedAsync` virtual method to perform actions when a client connects to the Hub, such as adding it to a group.</span></span>

[!code-csharp[Handle events](hubs/sample/hubs/chathub.cs?range=26-36)]

## <a name="handle-errors"></a><span data-ttu-id="c101c-147">Tratar erros</span><span class="sxs-lookup"><span data-stu-id="c101c-147">Handle errors</span></span>

<span data-ttu-id="c101c-148">Exceções geradas em seus métodos de hub são enviadas ao cliente que invocou o método.</span><span class="sxs-lookup"><span data-stu-id="c101c-148">Exceptions thrown in your hub methods are sent to the client that invoked the method.</span></span> <span data-ttu-id="c101c-149">No cliente JavaScript, o `invoke` método retorna um [promessa JavaScript](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises).</span><span class="sxs-lookup"><span data-stu-id="c101c-149">On the JavaScript client, the `invoke` method returns a [JavaScript Promise](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises).</span></span> <span data-ttu-id="c101c-150">Quando o cliente recebe um erro com um manipulador anexado à promessa usando `catch`, ele foi chamado e passado como um JavaScript `Error` objeto.</span><span class="sxs-lookup"><span data-stu-id="c101c-150">When the client receives an error with a handler attached to the promise using `catch`, it's invoked and passed as a JavaScript `Error` object.</span></span>

[!code-javascript[Error](hubs/sample/wwwroot/js/chat.js?range=22)]

## <a name="related-resources"></a><span data-ttu-id="c101c-151">Recursos relacionados</span><span class="sxs-lookup"><span data-stu-id="c101c-151">Related resources</span></span>

[<span data-ttu-id="c101c-152">Introdução ao ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="c101c-152">Intro to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
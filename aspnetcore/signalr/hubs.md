---
title: Usando os hubs de SignalR do ASP.NET Core
author: tdykstra
description: Saiba como usar os hubs do SignalR do ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 05/01/2018
uid: signalr/hubs
ms.openlocfilehash: e583676ab0eed45aeaf6391d8cdf8c1485aa914e
ms.sourcegitcommit: e7e1e531b80b3f4117ff119caadbebf4dcf5dcb7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/12/2018
ms.locfileid: "44510331"
---
# <a name="use-hubs-in-signalr-for-aspnet-core"></a><span data-ttu-id="60193-103">Usar os hubs no SignalR do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="60193-103">Use hubs in SignalR for ASP.NET Core</span></span>

<span data-ttu-id="60193-104">Por [Rachel Appel](https://twitter.com/rachelappel) e [Kevin Griffin](https://twitter.com/1kevgriff)</span><span class="sxs-lookup"><span data-stu-id="60193-104">By [Rachel Appel](https://twitter.com/rachelappel) and [Kevin Griffin](https://twitter.com/1kevgriff)</span></span>

<span data-ttu-id="60193-105">[Exibir ou baixar o código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(como fazer o download)](xref:tutorials/index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="60193-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(how to download)](xref:tutorials/index#how-to-download-a-sample)</span></span>

## <a name="what-is-a-signalr-hub"></a><span data-ttu-id="60193-106">O que é um hub SignalR</span><span class="sxs-lookup"><span data-stu-id="60193-106">What is a SignalR hub</span></span>

<span data-ttu-id="60193-107">A API de Hubs de SignalR permite chamar métodos em clientes conectados do servidor.</span><span class="sxs-lookup"><span data-stu-id="60193-107">The SignalR Hubs API enables you to call methods on connected clients from the server.</span></span> <span data-ttu-id="60193-108">No código do servidor, você define métodos que são chamados pelo cliente.</span><span class="sxs-lookup"><span data-stu-id="60193-108">In the server code, you define methods that are called by client.</span></span> <span data-ttu-id="60193-109">O código do cliente, você define métodos que são chamados do servidor.</span><span class="sxs-lookup"><span data-stu-id="60193-109">In the client code, you define methods that are called from the server.</span></span> <span data-ttu-id="60193-110">SignalR cuida de tudo o que nos bastidores que possibilita a comunicação de servidor para cliente e servidor-cliente em tempo real.</span><span class="sxs-lookup"><span data-stu-id="60193-110">SignalR takes care of everything behind the scenes that makes real-time client-to-server and server-to-client communications possible.</span></span>

## <a name="configure-signalr-hubs"></a><span data-ttu-id="60193-111">Configurar os hubs de SignalR</span><span class="sxs-lookup"><span data-stu-id="60193-111">Configure SignalR hubs</span></span>

<span data-ttu-id="60193-112">O middleware SignalR requer alguns serviços, que são configurados por meio da chamada `services.AddSignalR`.</span><span class="sxs-lookup"><span data-stu-id="60193-112">The SignalR middleware requires some services, which are configured by calling `services.AddSignalR`.</span></span>

[!code-csharp[Configure service](hubs/sample/startup.cs?range=38)]

<span data-ttu-id="60193-113">Ao adicionar a funcionalidade do SignalR para um aplicativo ASP.NET Core, configurar as rotas do SignalR chamando `app.UseSignalR` no `Startup.Configure` método.</span><span class="sxs-lookup"><span data-stu-id="60193-113">When adding SignalR functionality to an ASP.NET Core app, setup SignalR routes by calling `app.UseSignalR` in the `Startup.Configure` method.</span></span>

[!code-csharp[Configure routes to hubs](hubs/sample/startup.cs?range=57-60)]

## <a name="create-and-use-hubs"></a><span data-ttu-id="60193-114">Criar e usar os hubs</span><span class="sxs-lookup"><span data-stu-id="60193-114">Create and use hubs</span></span>

<span data-ttu-id="60193-115">Criar um hub, declarando uma classe que herda de `Hub`e adicione os métodos públicos a ele.</span><span class="sxs-lookup"><span data-stu-id="60193-115">Create a hub by declaring a class that inherits from `Hub`, and add public methods to it.</span></span> <span data-ttu-id="60193-116">Os clientes poderão chamar métodos que são definidos como `public`.</span><span class="sxs-lookup"><span data-stu-id="60193-116">Clients can call methods that are defined as `public`.</span></span>

[!code-csharp[Create and use hubs](hubs/sample/hubs/chathub.cs?range=8-37)]

<span data-ttu-id="60193-117">Você pode especificar um tipo de retorno e parâmetros, incluindo tipos complexos e matrizes, como você faria em qualquer método em c#.</span><span class="sxs-lookup"><span data-stu-id="60193-117">You can specify a return type and parameters, including complex types and arrays, as you would in any C# method.</span></span> <span data-ttu-id="60193-118">O SignalR lida com a serialização e desserialização de objetos complexos e matrizes em seus valores de retorno e parâmetros.</span><span class="sxs-lookup"><span data-stu-id="60193-118">SignalR handles the serialization and deserialization of complex objects and arrays in your parameters and return values.</span></span>

## <a name="the-context-object"></a><span data-ttu-id="60193-119">O objeto de contexto</span><span class="sxs-lookup"><span data-stu-id="60193-119">The Context object</span></span>

<span data-ttu-id="60193-120">O `Hub` classe tem um `Context` propriedade que contém as propriedades a seguir com informações sobre a conexão:</span><span class="sxs-lookup"><span data-stu-id="60193-120">The `Hub` class has a `Context` property that contains the following properties with information about the connection:</span></span>

| <span data-ttu-id="60193-121">Propriedade</span><span class="sxs-lookup"><span data-stu-id="60193-121">Property</span></span> | <span data-ttu-id="60193-122">Descrição</span><span class="sxs-lookup"><span data-stu-id="60193-122">Description</span></span> |
| ------ | ----------- |
| `ConnectionId` | <span data-ttu-id="60193-123">Obtém a ID exclusiva para a conexão, atribuído pelo SignalR.</span><span class="sxs-lookup"><span data-stu-id="60193-123">Gets the unique ID for the connection, assigned by SignalR.</span></span> <span data-ttu-id="60193-124">Há uma ID de conexão para cada conexão.</span><span class="sxs-lookup"><span data-stu-id="60193-124">There is one connection ID for each connection.</span></span>|
| `UserIdentifier` | <span data-ttu-id="60193-125">Obtém o [identificador de usuário](xref:signalr/groups).</span><span class="sxs-lookup"><span data-stu-id="60193-125">Gets the [user identifier](xref:signalr/groups).</span></span> <span data-ttu-id="60193-126">Por padrão, o SignalR usa o `ClaimTypes.NameIdentifier` do `ClaimsPrincipal` associado com a conexão como o identificador de usuário.</span><span class="sxs-lookup"><span data-stu-id="60193-126">By default, SignalR uses the `ClaimTypes.NameIdentifier` from the `ClaimsPrincipal` associated with the connection as the user identifier.</span></span> |
| `User` | <span data-ttu-id="60193-127">Obtém o `ClaimsPrincipal` associado ao usuário atual.</span><span class="sxs-lookup"><span data-stu-id="60193-127">Gets the `ClaimsPrincipal` associated with the current user.</span></span> |
| `Items` | <span data-ttu-id="60193-128">Obtém uma coleção de chave/valor que pode ser usada para compartilhar dados dentro do escopo dessa conexão.</span><span class="sxs-lookup"><span data-stu-id="60193-128">Gets a key/value collection that can be used to share data within the scope of this connection.</span></span> <span data-ttu-id="60193-129">Dados podem ser armazenados nessa coleção e ela será mantida para a conexão entre as invocações de método de hub diferentes.</span><span class="sxs-lookup"><span data-stu-id="60193-129">Data can be stored in this collection and it will persist for the connection across different hub method invocations.</span></span> |
| `Features` | <span data-ttu-id="60193-130">Obtém a coleção de recursos disponíveis sobre a conexão.</span><span class="sxs-lookup"><span data-stu-id="60193-130">Gets the collection of features available on the connection.</span></span> <span data-ttu-id="60193-131">Por enquanto, essa coleção não é necessária na maioria dos cenários, portanto, ele ainda não está documentado em detalhes.</span><span class="sxs-lookup"><span data-stu-id="60193-131">For now, this collection isn't needed in most scenarios, so it isn't documented in detail yet.</span></span> |
| `ConnectionAborted` | <span data-ttu-id="60193-132">Obtém um `CancellationToken` que notifica quando a conexão será anulada.</span><span class="sxs-lookup"><span data-stu-id="60193-132">Gets a `CancellationToken` that notifies when the connection is aborted.</span></span> |

<span data-ttu-id="60193-133">`Hub.Context` também contém os seguintes métodos:</span><span class="sxs-lookup"><span data-stu-id="60193-133">`Hub.Context` also contains the following methods:</span></span>

| <span data-ttu-id="60193-134">Método</span><span class="sxs-lookup"><span data-stu-id="60193-134">Method</span></span> | <span data-ttu-id="60193-135">Descrição</span><span class="sxs-lookup"><span data-stu-id="60193-135">Description</span></span> |
| ------ | ----------- |
| `GetHttpContext` | <span data-ttu-id="60193-136">Retorna o `HttpContext` para a conexão, ou `null` se a conexão não está associado uma solicitação HTTP.</span><span class="sxs-lookup"><span data-stu-id="60193-136">Returns the `HttpContext` for the connection, or `null` if the connection is not associated with an HTTP request.</span></span> <span data-ttu-id="60193-137">Para conexões HTTP, você pode usar esse método para obter informações como cadeias de caracteres de consulta e cabeçalhos HTTP.</span><span class="sxs-lookup"><span data-stu-id="60193-137">For HTTP connections, you can use this method to get information such as HTTP headers and query strings.</span></span> |
| `Abort` | <span data-ttu-id="60193-138">Anula a conexão.</span><span class="sxs-lookup"><span data-stu-id="60193-138">Aborts the connection.</span></span> |

## <a name="the-clients-object"></a><span data-ttu-id="60193-139">O objeto de clientes</span><span class="sxs-lookup"><span data-stu-id="60193-139">The Clients object</span></span>

<span data-ttu-id="60193-140">O `Hub` classe tem um `Clients` propriedade que contém as seguintes propriedades para a comunicação entre cliente e servidor:</span><span class="sxs-lookup"><span data-stu-id="60193-140">The `Hub` class has a `Clients` property that contains the following properties for communication between server and client:</span></span>

| <span data-ttu-id="60193-141">Propriedade</span><span class="sxs-lookup"><span data-stu-id="60193-141">Property</span></span> | <span data-ttu-id="60193-142">Descrição</span><span class="sxs-lookup"><span data-stu-id="60193-142">Description</span></span> |
| ------ | ----------- |
| `All` | <span data-ttu-id="60193-143">Chama um método em todos os clientes conectados</span><span class="sxs-lookup"><span data-stu-id="60193-143">Calls a method on all connected clients</span></span> |
| `Caller` | <span data-ttu-id="60193-144">Chama um método no cliente que invocou o método de hub</span><span class="sxs-lookup"><span data-stu-id="60193-144">Calls a method on the client that invoked the hub method</span></span> |
| `Others` | <span data-ttu-id="60193-145">Chama um método em todos os clientes conectados, exceto o cliente que invocou o método</span><span class="sxs-lookup"><span data-stu-id="60193-145">Calls a method on all connected clients except the client that invoked the method</span></span> |


<span data-ttu-id="60193-146">`Hub.Clients` também contém os seguintes métodos:</span><span class="sxs-lookup"><span data-stu-id="60193-146">`Hub.Clients` also contains the following methods:</span></span>

| <span data-ttu-id="60193-147">Método</span><span class="sxs-lookup"><span data-stu-id="60193-147">Method</span></span> | <span data-ttu-id="60193-148">Descrição</span><span class="sxs-lookup"><span data-stu-id="60193-148">Description</span></span> |
| ------ | ----------- |
| `AllExcept` | <span data-ttu-id="60193-149">Chama um método em todos os clientes conectados, exceto para as conexões especificadas</span><span class="sxs-lookup"><span data-stu-id="60193-149">Calls a method on all connected clients except for the specified connections</span></span> |
| `Client` | <span data-ttu-id="60193-150">Chama um método em um cliente conectado específico</span><span class="sxs-lookup"><span data-stu-id="60193-150">Calls a method on a specific connected client</span></span> |
| `Clients` | <span data-ttu-id="60193-151">Chama um método em clientes conectados específicos</span><span class="sxs-lookup"><span data-stu-id="60193-151">Calls a method on specific connected clients</span></span> |
| `Group` | <span data-ttu-id="60193-152">Chama um método para todas as conexões no grupo especificado</span><span class="sxs-lookup"><span data-stu-id="60193-152">Calls a method to all connections in the specified group</span></span>  |
| `GroupExcept` | <span data-ttu-id="60193-153">Chama um método para todas as conexões do grupo especificado, exceto as conexões especificadas</span><span class="sxs-lookup"><span data-stu-id="60193-153">Calls a method to all connections in the specified group, except the specified connections</span></span> |
| `Groups` | <span data-ttu-id="60193-154">Chama um método para vários grupos de conexões</span><span class="sxs-lookup"><span data-stu-id="60193-154">Calls a method to multiple groups of connections</span></span>  |
| `OthersInGroup` | <span data-ttu-id="60193-155">Chama um método a um grupo de conexões, excluindo o cliente que invocou o método de hub</span><span class="sxs-lookup"><span data-stu-id="60193-155">Calls a method to a group of connections, excluding the client that invoked the hub method</span></span>  |
| `User` | <span data-ttu-id="60193-156">Chama um método para todas as conexões associadas a um usuário específico</span><span class="sxs-lookup"><span data-stu-id="60193-156">Calls a method to all connections associated with a specific user</span></span> |
| `Users` | <span data-ttu-id="60193-157">Chama um método para todas as conexões associadas com os usuários especificados</span><span class="sxs-lookup"><span data-stu-id="60193-157">Calls a method to all connections associated with the specified users</span></span> |

<span data-ttu-id="60193-158">Cada propriedade ou método nas tabelas anteriores retorna um objeto com um `SendAsync` método.</span><span class="sxs-lookup"><span data-stu-id="60193-158">Each property or method in the preceding tables returns an object with a `SendAsync` method.</span></span> <span data-ttu-id="60193-159">O `SendAsync` método permite que você forneça o nome e parâmetros do método de cliente para chamar.</span><span class="sxs-lookup"><span data-stu-id="60193-159">The `SendAsync` method allows you to supply the name and parameters of the client method to call.</span></span>

## <a name="send-messages-to-clients"></a><span data-ttu-id="60193-160">Enviar mensagens para os clientes</span><span class="sxs-lookup"><span data-stu-id="60193-160">Send messages to clients</span></span>

<span data-ttu-id="60193-161">Para fazer chamadas para clientes específicos, use as propriedades do `Clients` objeto.</span><span class="sxs-lookup"><span data-stu-id="60193-161">To make calls to specific clients, use the properties of the `Clients` object.</span></span> <span data-ttu-id="60193-162">No exemplo a seguir, o `SendMessageToCaller` método demonstra como enviar uma mensagem para a conexão que invocou o método de hub.</span><span class="sxs-lookup"><span data-stu-id="60193-162">In the following example, the `SendMessageToCaller` method demonstrates sending a message to the connection that invoked the hub method.</span></span> <span data-ttu-id="60193-163">O `SendMessageToGroups` método envia uma mensagem para os grupos armazenados em um `List` denominado `groups`.</span><span class="sxs-lookup"><span data-stu-id="60193-163">The `SendMessageToGroups` method sends a message to the groups stored in a `List` named `groups`.</span></span>

[!code-csharp[Send messages](hubs/sample/hubs/chathub.cs?range=15-24)]

## <a name="handle-events-for-a-connection"></a><span data-ttu-id="60193-164">Manipular eventos para uma conexão</span><span class="sxs-lookup"><span data-stu-id="60193-164">Handle events for a connection</span></span>

<span data-ttu-id="60193-165">A API de Hubs de SignalR fornece o `OnConnectedAsync` e `OnDisconnectedAsync` métodos virtuais para gerenciar e controlar conexões.</span><span class="sxs-lookup"><span data-stu-id="60193-165">The SignalR Hubs API provides the `OnConnectedAsync` and `OnDisconnectedAsync` virtual methods to manage and track connections.</span></span> <span data-ttu-id="60193-166">Substituir o `OnConnectedAsync` método virtual para executar ações quando um cliente se conecta ao Hub, como adicioná-lo a um grupo.</span><span class="sxs-lookup"><span data-stu-id="60193-166">Override the `OnConnectedAsync` virtual method to perform actions when a client connects to the Hub, such as adding it to a group.</span></span>

[!code-csharp[Handle events](hubs/sample/hubs/chathub.cs?range=26-36)]

## <a name="handle-errors"></a><span data-ttu-id="60193-167">Tratar erros</span><span class="sxs-lookup"><span data-stu-id="60193-167">Handle errors</span></span>

<span data-ttu-id="60193-168">As exceções geradas em seus métodos de hub são enviadas ao cliente que invocou o método.</span><span class="sxs-lookup"><span data-stu-id="60193-168">Exceptions thrown in your hub methods are sent to the client that invoked the method.</span></span> <span data-ttu-id="60193-169">No cliente JavaScript, o `invoke` método retorna um [promessa JavaScript](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises).</span><span class="sxs-lookup"><span data-stu-id="60193-169">On the JavaScript client, the `invoke` method returns a [JavaScript Promise](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises).</span></span> <span data-ttu-id="60193-170">Quando o cliente recebe um erro com um manipulador anexado à promessa usando `catch`, ele tem chamado e passado como um JavaScript `Error` objeto.</span><span class="sxs-lookup"><span data-stu-id="60193-170">When the client receives an error with a handler attached to the promise using `catch`, it's invoked and passed as a JavaScript `Error` object.</span></span>

[!code-javascript[Error](hubs/sample/wwwroot/js/chat.js?range=23)]

## <a name="related-resources"></a><span data-ttu-id="60193-171">Recursos relacionados</span><span class="sxs-lookup"><span data-stu-id="60193-171">Related resources</span></span>

* [<span data-ttu-id="60193-172">Introdução ao SignalR do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="60193-172">Intro to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
* [<span data-ttu-id="60193-173">Cliente JavaScript</span><span class="sxs-lookup"><span data-stu-id="60193-173">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="60193-174">Publicar no Azure</span><span class="sxs-lookup"><span data-stu-id="60193-174">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)

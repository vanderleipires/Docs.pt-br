---
title: Usando os hubs de SignalR do ASP.NET Core
author: rachelappel
description: Saiba como usar hubs de SignalR do ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 05/01/2018
uid: signalr/hubs
ms.openlocfilehash: 5558a5787396c3aa8055175486369eb2534c1fa2
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277662"
---
# <a name="use-hubs-in-signalr-for-aspnet-core"></a><span data-ttu-id="187c8-103">Usando os hubs de SignalR para ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="187c8-103">Use hubs in SignalR for ASP.NET Core</span></span>

<span data-ttu-id="187c8-104">Por [Rachel Appel](https://twitter.com/rachelappel) e [Griffin Kevin](https://twitter.com/1kevgriff)</span><span class="sxs-lookup"><span data-stu-id="187c8-104">By [Rachel Appel](https://twitter.com/rachelappel) and [Kevin Griffin](https://twitter.com/1kevgriff)</span></span>

<span data-ttu-id="187c8-105">[Exibir ou baixar o código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(como fazer o download)](xref:tutorials/index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="187c8-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(how to download)](xref:tutorials/index#how-to-download-a-sample)</span></span>

## <a name="what-is-a-signalr-hub"></a><span data-ttu-id="187c8-106">O que é um hub SignalR</span><span class="sxs-lookup"><span data-stu-id="187c8-106">What is a SignalR hub</span></span>

<span data-ttu-id="187c8-107">A API de Hubs de SignalR permite chamar métodos em clientes conectados do servidor.</span><span class="sxs-lookup"><span data-stu-id="187c8-107">The SignalR Hubs API enables you to call methods on connected clients from the server.</span></span> <span data-ttu-id="187c8-108">No código de servidor, você define os métodos que são chamados pelo cliente.</span><span class="sxs-lookup"><span data-stu-id="187c8-108">In the server code, you define methods that are called by client.</span></span> <span data-ttu-id="187c8-109">No código do cliente, você define os métodos que são chamados do servidor.</span><span class="sxs-lookup"><span data-stu-id="187c8-109">In the client code, you define methods that are called from the server.</span></span> <span data-ttu-id="187c8-110">SignalR cuida de tudo nos bastidores que possibilita a comunicação de cliente para servidor e servidor-para-cliente em tempo real.</span><span class="sxs-lookup"><span data-stu-id="187c8-110">SignalR takes care of everything behind the scenes that makes real-time client-to-server and server-to-client communications possible.</span></span>

## <a name="configure-signalr-hubs"></a><span data-ttu-id="187c8-111">Configurar os hubs de SignalR</span><span class="sxs-lookup"><span data-stu-id="187c8-111">Configure SignalR hubs</span></span>

<span data-ttu-id="187c8-112">O middleware SignalR requer alguns serviços, que são configurados por meio da chamada `services.AddSignalR`.</span><span class="sxs-lookup"><span data-stu-id="187c8-112">The SignalR middleware requires some services, which are configured by calling `services.AddSignalR`.</span></span>

[!code-csharp[Configure service](hubs/sample/startup.cs?range=38)]

<span data-ttu-id="187c8-113">Ao adicionar a funcionalidade de SignalR para um aplicativo ASP.NET Core, configurar rotas SignalR chamando `app.UseSignalR` no `Startup.Configure` método.</span><span class="sxs-lookup"><span data-stu-id="187c8-113">When adding SignalR functionality to an ASP.NET Core app, setup SignalR routes by calling `app.UseSignalR` in the `Startup.Configure` method.</span></span>

[!code-csharp[Configure routes to hubs](hubs/sample/startup.cs?range=57-60)]

## <a name="create-and-use-hubs"></a><span data-ttu-id="187c8-114">Criar e usar hubs</span><span class="sxs-lookup"><span data-stu-id="187c8-114">Create and use hubs</span></span>

<span data-ttu-id="187c8-115">Criar um hub declarando uma classe que herda de `Hub`e adicionar métodos públicos a ele.</span><span class="sxs-lookup"><span data-stu-id="187c8-115">Create a hub by declaring a class that inherits from `Hub`, and add public methods to it.</span></span> <span data-ttu-id="187c8-116">Os clientes podem chamar os métodos que são definidos como `public`.</span><span class="sxs-lookup"><span data-stu-id="187c8-116">Clients can call methods that are defined as `public`.</span></span>

[!code-csharp[Create and use hubs](hubs/sample/hubs/chathub.cs?range=8-37)]

<span data-ttu-id="187c8-117">Você pode especificar um tipo de retorno e parâmetros, incluindo tipos complexos e matrizes, como você faria em qualquer método em c#.</span><span class="sxs-lookup"><span data-stu-id="187c8-117">You can specify a return type and parameters, including complex types and arrays, as you would in any C# method.</span></span> <span data-ttu-id="187c8-118">SignalR lida com a serialização e desserialização de objetos complexos e matrizes em seus parâmetros e valores de retorno.</span><span class="sxs-lookup"><span data-stu-id="187c8-118">SignalR handles the serialization and deserialization of complex objects and arrays in your parameters and return values.</span></span>

## <a name="the-clients-object"></a><span data-ttu-id="187c8-119">O objeto de clientes</span><span class="sxs-lookup"><span data-stu-id="187c8-119">The Clients object</span></span>

<span data-ttu-id="187c8-120">Cada instância do `Hub` classe tem uma propriedade denominada `Clients` que contém os seguintes membros para a comunicação entre cliente e servidor:</span><span class="sxs-lookup"><span data-stu-id="187c8-120">Each instance of the `Hub` class has a property named `Clients` that contains the following members for communication between server and client:</span></span>

| <span data-ttu-id="187c8-121">Propriedade</span><span class="sxs-lookup"><span data-stu-id="187c8-121">Property</span></span> | <span data-ttu-id="187c8-122">Descrição</span><span class="sxs-lookup"><span data-stu-id="187c8-122">Description</span></span> |
| ------ | ----------- |
| `All` | <span data-ttu-id="187c8-123">Chama um método em todos os clientes conectados</span><span class="sxs-lookup"><span data-stu-id="187c8-123">Calls a method on all connected clients</span></span> |
| `Caller` | <span data-ttu-id="187c8-124">Chama um método no cliente que invocou o método de hub</span><span class="sxs-lookup"><span data-stu-id="187c8-124">Calls a method on the client that invoked the hub method</span></span> |
| `Others` | <span data-ttu-id="187c8-125">Chama um método em todos os clientes conectados, exceto o cliente que invocou o método</span><span class="sxs-lookup"><span data-stu-id="187c8-125">Calls a method on all connected clients except the client that invoked the method</span></span> |


<span data-ttu-id="187c8-126">Além disso, `Hub.Clients` contém os seguintes métodos:</span><span class="sxs-lookup"><span data-stu-id="187c8-126">Additionally, `Hub.Clients` contains the following methods:</span></span>

| <span data-ttu-id="187c8-127">Método</span><span class="sxs-lookup"><span data-stu-id="187c8-127">Method</span></span> | <span data-ttu-id="187c8-128">Descrição</span><span class="sxs-lookup"><span data-stu-id="187c8-128">Description</span></span> |
| ------ | ----------- |
| `AllExcept` | <span data-ttu-id="187c8-129">Chama um método em todos os clientes conectados, exceto para as conexões especificadas</span><span class="sxs-lookup"><span data-stu-id="187c8-129">Calls a method on all connected clients except for the specified connections</span></span> |
| `Client` | <span data-ttu-id="187c8-130">Chama um método em um cliente conectado específico</span><span class="sxs-lookup"><span data-stu-id="187c8-130">Calls a method on a specific connected client</span></span> |
| `Clients` | <span data-ttu-id="187c8-131">Chama um método específicos clientes conectados</span><span class="sxs-lookup"><span data-stu-id="187c8-131">Calls a method on specific connected clients</span></span> |
| `Group` | <span data-ttu-id="187c8-132">Chama um método para todas as conexões no grupo especificado</span><span class="sxs-lookup"><span data-stu-id="187c8-132">Calls a method to all connections in the specified group</span></span>  |
| `GroupExcept` | <span data-ttu-id="187c8-133">Chama um método para todas as conexões no grupo especificado, exceto as conexões especificadas</span><span class="sxs-lookup"><span data-stu-id="187c8-133">Calls a method to all connections in the specified group, except the specified connections</span></span> |
| `Groups` | <span data-ttu-id="187c8-134">Chama um método para vários grupos de conexões</span><span class="sxs-lookup"><span data-stu-id="187c8-134">Calls a method to multiple groups of connections</span></span>  |
| `OthersInGroup` | <span data-ttu-id="187c8-135">Chama um método a um grupo de conexões, excluindo o cliente que invocou o método de hub</span><span class="sxs-lookup"><span data-stu-id="187c8-135">Calls a method to a group of connections, excluding the client that invoked the hub method</span></span>  |
| `User` | <span data-ttu-id="187c8-136">Chama um método para todas as conexões associadas a um usuário específico</span><span class="sxs-lookup"><span data-stu-id="187c8-136">Calls a method to all connections associated with a specific user</span></span> |
| `Users` | <span data-ttu-id="187c8-137">Chama um método para todas as conexões associadas com os usuários especificados</span><span class="sxs-lookup"><span data-stu-id="187c8-137">Calls a method to all connections associated with the specified users</span></span> |

<span data-ttu-id="187c8-138">Cada propriedade ou método nas tabelas anteriores retorna um objeto com um `SendAsync` método.</span><span class="sxs-lookup"><span data-stu-id="187c8-138">Each property or method in the preceding tables returns an object with a `SendAsync` method.</span></span> <span data-ttu-id="187c8-139">O `SendAsync` método permite que você forneça o nome e os parâmetros do método de cliente para chamar.</span><span class="sxs-lookup"><span data-stu-id="187c8-139">The `SendAsync` method allows you to supply the name and parameters of the client method to call.</span></span>

## <a name="send-messages-to-clients"></a><span data-ttu-id="187c8-140">Enviar mensagens para os clientes</span><span class="sxs-lookup"><span data-stu-id="187c8-140">Send messages to clients</span></span>

<span data-ttu-id="187c8-141">Para fazer chamadas para clientes específicos, use as propriedades do `Clients` objeto.</span><span class="sxs-lookup"><span data-stu-id="187c8-141">To make calls to specific clients, use the properties of the `Clients` object.</span></span> <span data-ttu-id="187c8-142">No exemplo a seguir, o `SendMessageToCaller` método demonstra enviando uma mensagem para a conexão que invocou o método de hub.</span><span class="sxs-lookup"><span data-stu-id="187c8-142">In the following example, the `SendMessageToCaller` method demonstrates sending a message to the connection that invoked the hub method.</span></span> <span data-ttu-id="187c8-143">O `SendMessageToGroups` método envia uma mensagem para os grupos armazenados em um `List` chamado `groups`.</span><span class="sxs-lookup"><span data-stu-id="187c8-143">The `SendMessageToGroups` method sends a message to the groups stored in a `List` named `groups`.</span></span>

[!code-csharp[Send messages](hubs/sample/hubs/chathub.cs?range=15-24)]

## <a name="handle-events-for-a-connection"></a><span data-ttu-id="187c8-144">Manipular eventos para uma conexão</span><span class="sxs-lookup"><span data-stu-id="187c8-144">Handle events for a connection</span></span>

<span data-ttu-id="187c8-145">A API de Hubs de SignalR fornece o `OnConnectedAsync` e `OnDisconnectedAsync` métodos virtuais para gerenciar e controlar conexões.</span><span class="sxs-lookup"><span data-stu-id="187c8-145">The SignalR Hubs API provides the `OnConnectedAsync` and `OnDisconnectedAsync` virtual methods to manage and track connections.</span></span> <span data-ttu-id="187c8-146">Substituir o `OnConnectedAsync` método virtual para executar ações quando um cliente se conecta ao Hub, como adicioná-lo a um grupo.</span><span class="sxs-lookup"><span data-stu-id="187c8-146">Override the `OnConnectedAsync` virtual method to perform actions when a client connects to the Hub, such as adding it to a group.</span></span>

[!code-csharp[Handle events](hubs/sample/hubs/chathub.cs?range=26-36)]

## <a name="handle-errors"></a><span data-ttu-id="187c8-147">Tratar erros</span><span class="sxs-lookup"><span data-stu-id="187c8-147">Handle errors</span></span>

<span data-ttu-id="187c8-148">Exceções geradas em seus métodos de hub são enviadas ao cliente que invocou o método.</span><span class="sxs-lookup"><span data-stu-id="187c8-148">Exceptions thrown in your hub methods are sent to the client that invoked the method.</span></span> <span data-ttu-id="187c8-149">No cliente JavaScript, o `invoke` método retorna um [promessa JavaScript](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises).</span><span class="sxs-lookup"><span data-stu-id="187c8-149">On the JavaScript client, the `invoke` method returns a [JavaScript Promise](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises).</span></span> <span data-ttu-id="187c8-150">Quando o cliente recebe um erro com um manipulador anexado à promessa usando `catch`, ele foi chamado e passado como um JavaScript `Error` objeto.</span><span class="sxs-lookup"><span data-stu-id="187c8-150">When the client receives an error with a handler attached to the promise using `catch`, it's invoked and passed as a JavaScript `Error` object.</span></span>

[!code-javascript[Error](hubs/sample/wwwroot/js/chat.js?range=23)]

## <a name="related-resources"></a><span data-ttu-id="187c8-151">Recursos relacionados</span><span class="sxs-lookup"><span data-stu-id="187c8-151">Related resources</span></span>

* [<span data-ttu-id="187c8-152">Introdução ao ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="187c8-152">Intro to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
* [<span data-ttu-id="187c8-153">Cliente JavaScript</span><span class="sxs-lookup"><span data-stu-id="187c8-153">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="187c8-154">Publicar no Azure</span><span class="sxs-lookup"><span data-stu-id="187c8-154">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)

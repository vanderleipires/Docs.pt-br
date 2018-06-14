---
title: Gerenciar usuários e grupos no SignalR
author: rachelappel
description: Visão geral do ASP.NET Core SignalR usuário e grupo de gerenciamento.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 06/04/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/groups
ms.openlocfilehash: 2a2f129863cf7d5cdfa3c0e5b2d901af52144671
ms.sourcegitcommit: 7e87671fea9a5f36ca516616fe3b40b537f428d2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/12/2018
ms.locfileid: "35358420"
---
# <a name="manage-users-and-groups-in-signalr"></a><span data-ttu-id="39958-103">Gerenciar usuários e grupos no SignalR</span><span class="sxs-lookup"><span data-stu-id="39958-103">Manage users and groups in SignalR</span></span>

<span data-ttu-id="39958-104">Por [Brennan Conroy](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="39958-104">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="39958-105">SignalR permite que mensagens sejam enviados para todas as conexões associadas a um usuário específico, bem como grupos de conexões nomeados.</span><span class="sxs-lookup"><span data-stu-id="39958-105">SignalR allows messages to be sent to all connections associated with a specific user, as well as to named groups of connections.</span></span>

<span data-ttu-id="39958-106">[Exibir ou baixar o código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/groups/sample/) [(como fazer o download)](xref:tutorials/index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="39958-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/groups/sample/) [(how to download)](xref:tutorials/index#how-to-download-a-sample)</span></span>

## <a name="users-in-signalr"></a><span data-ttu-id="39958-107">Usuários no SignalR</span><span class="sxs-lookup"><span data-stu-id="39958-107">Users in SignalR</span></span>

<span data-ttu-id="39958-108">SignalR permite que você envie mensagens para todas as conexões associadas a um usuário específico.</span><span class="sxs-lookup"><span data-stu-id="39958-108">SignalR allows you to send messages to all connections associated with a specific user.</span></span> <span data-ttu-id="39958-109">Por padrão o SignalR usa o `ClaimTypes.NameIdentifier` do `ClaimsPrincipal` associado com a conexão como o identificador do usuário.</span><span class="sxs-lookup"><span data-stu-id="39958-109">By default SignalR uses the `ClaimTypes.NameIdentifier` from the `ClaimsPrincipal` associated with the connection as the user identifier.</span></span> <span data-ttu-id="39958-110">Um único usuário pode ter várias conexões a um aplicativo do SignalR.</span><span class="sxs-lookup"><span data-stu-id="39958-110">A single user can have multiple connections to a SignalR application.</span></span> <span data-ttu-id="39958-111">Por exemplo, um usuário pode ser conectado em sua área de trabalho, bem como seu telefone.</span><span class="sxs-lookup"><span data-stu-id="39958-111">For example, a user could be connected on their desktop as well as their phone.</span></span> <span data-ttu-id="39958-112">Cada dispositivo tem uma conexão SignalR separado, mas eles estão todos os associados com o mesmo usuário.</span><span class="sxs-lookup"><span data-stu-id="39958-112">Each device has a separate SignalR connection, but they are all associated with the same user.</span></span> <span data-ttu-id="39958-113">Se uma mensagem é enviada para o usuário, todas as conexões associadas ao usuário receberá a mensagem.</span><span class="sxs-lookup"><span data-stu-id="39958-113">If a message is sent to the user, all of the connections associated with that user will receive the message.</span></span>

<span data-ttu-id="39958-114">Enviar uma mensagem para um usuário específico, passando o identificador do usuário para o `User` função em seu método de hub, conforme mostrado no exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="39958-114">Send a message to a specific user by passing the user identifier to the `User` function in your hub method as shown in the following example:</span></span>

> [!NOTE]
> <span data-ttu-id="39958-115">O identificador de usuário diferencia maiusculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="39958-115">The user identifier is case-sensitive.</span></span>

```csharp
public Task SendPrivateMessage(string user, string message)
{
    return Clients.User(user).SendAsync("ReceiveMessage", message);
}
```

<span data-ttu-id="39958-116">O identificador de usuário pode ser personalizado, criando um `IUserIdProvider`e registrá-lo no `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="39958-116">The user identifier can be customized by creating an `IUserIdProvider`, and registering it in `ConfigureServices`.</span></span>

[!code-csharp[UserIdProvider](groups/sample/customuseridprovider.cs?range=4-10)]

[!code-csharp[Configure service](groups/sample/startup.cs?range=21-22,39-42)]

> [!NOTE]
> <span data-ttu-id="39958-117">AddSignalR deve ser chamado antes de registrar os serviços SignalR personalizados.</span><span class="sxs-lookup"><span data-stu-id="39958-117">AddSignalR must be called before registering your custom SignalR services.</span></span>

## <a name="groups-in-signalr"></a><span data-ttu-id="39958-118">Grupos no SignalR</span><span class="sxs-lookup"><span data-stu-id="39958-118">Groups in SignalR</span></span>

<span data-ttu-id="39958-119">Um grupo é uma coleção de conexões associado com um nome.</span><span class="sxs-lookup"><span data-stu-id="39958-119">A group is a collection of connections associated with a name.</span></span> <span data-ttu-id="39958-120">As mensagens podem ser enviadas a todas as conexões em um grupo.</span><span class="sxs-lookup"><span data-stu-id="39958-120">Messages can be sent to all connections in a group.</span></span> <span data-ttu-id="39958-121">Grupos são a maneira recomendada para enviar para uma conexão ou várias conexões, porque os grupos são gerenciados pelo aplicativo.</span><span class="sxs-lookup"><span data-stu-id="39958-121">Groups are the recommended way to send to a connection or multiple connections because the groups are managed by the application.</span></span> <span data-ttu-id="39958-122">Uma conexão pode ser um membro de vários grupos.</span><span class="sxs-lookup"><span data-stu-id="39958-122">A connection can be a member of multiple groups.</span></span> <span data-ttu-id="39958-123">Isso torna grupos ideal para algo como um aplicativo de bate-papo, onde cada espaço pode ser representado como um grupo.</span><span class="sxs-lookup"><span data-stu-id="39958-123">This makes groups ideal for something like a chat application, where each room can be represented as a group.</span></span> <span data-ttu-id="39958-124">Conexões podem ser adicionados ou removidos de grupos por meio de `AddToGroupAsync` e `RemoveFromGroupAsync` métodos.</span><span class="sxs-lookup"><span data-stu-id="39958-124">Connections can be added to or removed from groups via the `AddToGroupAsync` and `RemoveFromGroupAsync` methods.</span></span>

[!code-csharp[Hub methods](groups/sample/hubs/chathub.cs?range=15-27)]

<span data-ttu-id="39958-125">Associação de grupo não é preservada quando uma conexão é reconectada.</span><span class="sxs-lookup"><span data-stu-id="39958-125">Group membership isn't preserved when a connection reconnects.</span></span> <span data-ttu-id="39958-126">A conexão deve se associar novamente o grupo quando ela é restabelecida.</span><span class="sxs-lookup"><span data-stu-id="39958-126">The connection needs to rejoin the group when it's re-established.</span></span> <span data-ttu-id="39958-127">Não é possível contar os membros de um grupo, porque essas informações não estão disponíveis se o aplicativo é dimensionado para vários servidores.</span><span class="sxs-lookup"><span data-stu-id="39958-127">It's not possible to count the members of a group, since this information is not available if the application is scaled to multiple servers.</span></span>

> [!NOTE]
> <span data-ttu-id="39958-128">Os nomes de grupo diferenciam maiusculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="39958-128">Group names are case-sensitive.</span></span>

## <a name="related-resources"></a><span data-ttu-id="39958-129">Recursos relacionados</span><span class="sxs-lookup"><span data-stu-id="39958-129">Related resources</span></span>

* [<span data-ttu-id="39958-130">Introdução</span><span class="sxs-lookup"><span data-stu-id="39958-130">Get started</span></span>](xref:signalr/get-started)
* [<span data-ttu-id="39958-131">Hubs</span><span class="sxs-lookup"><span data-stu-id="39958-131">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="39958-132">Publicar no Azure</span><span class="sxs-lookup"><span data-stu-id="39958-132">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)

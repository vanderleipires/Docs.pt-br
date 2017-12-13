---
uid: signalr/overview/older-versions/mapping-users-to-connections
title: "Mapeamento de usuários do SignalR para conexões no SignalR 1. x | Microsoft Docs"
author: pfletcher
description: "Este tópico mostra como reter informações sobre usuários e suas conexões."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: ebbc93a8-e6c4-4122-8e0d-3aa42293c747
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/mapping-users-to-connections
msc.type: authoredcontent
ms.openlocfilehash: 561c5739c4e8465efeb4b5d1eaf8a196dab8673f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="mapping-signalr-users-to-connections-in-signalr-1x"></a><span data-ttu-id="57bcb-103">Mapeamento de usuários do SignalR para conexões no SignalR 1. x</span><span class="sxs-lookup"><span data-stu-id="57bcb-103">Mapping SignalR Users to Connections in SignalR 1.x</span></span>
====================
<span data-ttu-id="57bcb-104">por [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="57bcb-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="57bcb-105">Este tópico mostra como reter informações sobre usuários e suas conexões.</span><span class="sxs-lookup"><span data-stu-id="57bcb-105">This topic shows how to retain information about users and their connections.</span></span>


## <a name="introduction"></a><span data-ttu-id="57bcb-106">Introdução</span><span class="sxs-lookup"><span data-stu-id="57bcb-106">Introduction</span></span>

<span data-ttu-id="57bcb-107">Cada cliente que se conecta a um hub passa uma id de conexão exclusivo. Você pode recuperar esse valor no `Context.ConnectionId` propriedade do contexto do hub.</span><span class="sxs-lookup"><span data-stu-id="57bcb-107">Each client connecting to a hub passes a unique connection id. You can retrieve this value in the `Context.ConnectionId` property of the hub context.</span></span> <span data-ttu-id="57bcb-108">Se seu aplicativo precisa mapear um usuário para a id de conexão e manter esse mapeamento, você pode usar um dos seguintes:</span><span class="sxs-lookup"><span data-stu-id="57bcb-108">If your application needs to map a user to the connection id and persist that mapping, you can use one of the following:</span></span>

- <span data-ttu-id="57bcb-109">[Armazenamento na memória](#inmemory), como um dicionário</span><span class="sxs-lookup"><span data-stu-id="57bcb-109">[In-memory storage](#inmemory), such as a dictionary</span></span>
- [<span data-ttu-id="57bcb-110">Grupo de SignalR para cada usuário</span><span class="sxs-lookup"><span data-stu-id="57bcb-110">SignalR group for each user</span></span>](#groups)
- <span data-ttu-id="57bcb-111">[Armazenamento externo permanente](#database), como uma tabela de banco de dados ou armazenamento de tabela do Azure</span><span class="sxs-lookup"><span data-stu-id="57bcb-111">[Permanent, external storage](#database), such as a database table or Azure table storage</span></span>

<span data-ttu-id="57bcb-112">Cada uma dessas implementações é mostrada neste tópico.</span><span class="sxs-lookup"><span data-stu-id="57bcb-112">Each of these implementations is shown in this topic.</span></span> <span data-ttu-id="57bcb-113">Você usa o `OnConnected`, `OnDisconnected`, e `OnReconnected` métodos do `Hub` classe para acompanhar o status de conexão do usuário.</span><span class="sxs-lookup"><span data-stu-id="57bcb-113">You use the `OnConnected`, `OnDisconnected`, and `OnReconnected` methods of the `Hub` class to track the user connection status.</span></span>

<span data-ttu-id="57bcb-114">A melhor abordagem para seu aplicativo depende de:</span><span class="sxs-lookup"><span data-stu-id="57bcb-114">The best approach for your application depends on:</span></span>

- <span data-ttu-id="57bcb-115">O número de servidores web hospedando o seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="57bcb-115">The number of web servers hosting your application.</span></span>
- <span data-ttu-id="57bcb-116">Se você precisa obter uma lista dos usuários conectados no momento.</span><span class="sxs-lookup"><span data-stu-id="57bcb-116">Whether you need to get a list of the currently connected users.</span></span>
- <span data-ttu-id="57bcb-117">Se você precisa manter informações de grupo e usuário quando o aplicativo ou o servidor for reiniciado.</span><span class="sxs-lookup"><span data-stu-id="57bcb-117">Whether you need to persist group and user information when the application or server restarts.</span></span>
- <span data-ttu-id="57bcb-118">Se a latência de chamada de um servidor externo é um problema.</span><span class="sxs-lookup"><span data-stu-id="57bcb-118">Whether the latency of calling an external server is an issue.</span></span>

<span data-ttu-id="57bcb-119">A tabela a seguir mostra qual abordagem funciona para essas considerações.</span><span class="sxs-lookup"><span data-stu-id="57bcb-119">The following table shows which approach works for these considerations.</span></span>

|  | <span data-ttu-id="57bcb-120">Mais de um servidor</span><span class="sxs-lookup"><span data-stu-id="57bcb-120">More than one server</span></span> | <span data-ttu-id="57bcb-121">Obter lista de usuários conectados no momento</span><span class="sxs-lookup"><span data-stu-id="57bcb-121">Get list of currently connected users</span></span> | <span data-ttu-id="57bcb-122">Manter as informações depois que for reiniciado</span><span class="sxs-lookup"><span data-stu-id="57bcb-122">Persist information after restarts</span></span> | <span data-ttu-id="57bcb-123">Desempenho ideal</span><span class="sxs-lookup"><span data-stu-id="57bcb-123">Optimal performance</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="57bcb-124">Na memória</span><span class="sxs-lookup"><span data-stu-id="57bcb-124">In-memory</span></span> |  | ![](mapping-users-to-connections/_static/image1.png) |  | ![](mapping-users-to-connections/_static/image2.png) |
| <span data-ttu-id="57bcb-125">Grupos de usuário único</span><span class="sxs-lookup"><span data-stu-id="57bcb-125">Single-user groups</span></span> | ![](mapping-users-to-connections/_static/image3.png) |  |  | ![](mapping-users-to-connections/_static/image4.png) |
| <span data-ttu-id="57bcb-126">Permanentes, externa</span><span class="sxs-lookup"><span data-stu-id="57bcb-126">Permanent, external</span></span> | ![](mapping-users-to-connections/_static/image5.png) | ![](mapping-users-to-connections/_static/image6.png) | ![](mapping-users-to-connections/_static/image7.png) |  |

<a id="inmemory"></a>

## <a name="in-memory-storage"></a><span data-ttu-id="57bcb-127">Armazenamento na memória</span><span class="sxs-lookup"><span data-stu-id="57bcb-127">In-memory storage</span></span>

<span data-ttu-id="57bcb-128">Os exemplos a seguir mostram como reter informações de conexão e do usuário em um dicionário que é armazenado na memória.</span><span class="sxs-lookup"><span data-stu-id="57bcb-128">The following examples show how to retain connection and user information in a dictionary that is stored in memory.</span></span> <span data-ttu-id="57bcb-129">O dicionário usa um `HashSet` para armazenar a id de conexão. A qualquer momento, um usuário pode ter mais de uma conexão para o aplicativo do SignalR.</span><span class="sxs-lookup"><span data-stu-id="57bcb-129">The dictionary uses a `HashSet` to store the connection id. At any time a user could have more than one connection to the SignalR application.</span></span> <span data-ttu-id="57bcb-130">Por exemplo, um usuário que está conectado por meio de vários dispositivos ou mais de uma guia do navegador teria mais de uma id de conexão.</span><span class="sxs-lookup"><span data-stu-id="57bcb-130">For example, a user who is connected through multiple devices or more than one browser tab would have more than one connection id.</span></span>

<span data-ttu-id="57bcb-131">Se o aplicativo for fechado, todas as informações é perdida, mas ele será novamente preenchido conforme os usuários restabelecer a conexão.</span><span class="sxs-lookup"><span data-stu-id="57bcb-131">If the application shuts down, all of the information is lost, but it will be re-populated as the users re-establish their connections.</span></span> <span data-ttu-id="57bcb-132">Armazenamento na memória não funcionará se o seu ambiente incluir mais de um servidor web, porque cada servidor deve ter um conjunto separado de conexões.</span><span class="sxs-lookup"><span data-stu-id="57bcb-132">In-memory storage does not work if your environment includes more than one web server because each server would have a separate collection of connections.</span></span>

<span data-ttu-id="57bcb-133">O primeiro exemplo mostra uma classe que gerencia o mapeamento de usuários para conexões.</span><span class="sxs-lookup"><span data-stu-id="57bcb-133">The first example shows a class that manages the mapping of users to connections.</span></span> <span data-ttu-id="57bcb-134">A chave para o HashSet será o nome do usuário.</span><span class="sxs-lookup"><span data-stu-id="57bcb-134">The key for the HashSet will be the user's name.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample1.cs)]

<span data-ttu-id="57bcb-135">O exemplo a seguir mostra como usar a classe de mapeamento de conexão de um hub.</span><span class="sxs-lookup"><span data-stu-id="57bcb-135">The next example shows how to use the connection mapping class from a hub.</span></span> <span data-ttu-id="57bcb-136">A instância da classe é armazenada em um nome de variável `_connections`.</span><span class="sxs-lookup"><span data-stu-id="57bcb-136">The instance of the class is stored in a variable name `_connections`.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample2.cs)]

<a id="groups"></a>

## <a name="single-user-groups"></a><span data-ttu-id="57bcb-137">Grupos de usuário único</span><span class="sxs-lookup"><span data-stu-id="57bcb-137">Single-user groups</span></span>

<span data-ttu-id="57bcb-138">Você pode criar um grupo para cada usuário e, em seguida, enviar uma mensagem a esse grupo quando quiser acessar somente esse usuário.</span><span class="sxs-lookup"><span data-stu-id="57bcb-138">You can create a group for each user, and then send a message to that group when you want to reach only that user.</span></span> <span data-ttu-id="57bcb-139">O nome de cada grupo é o nome do usuário.</span><span class="sxs-lookup"><span data-stu-id="57bcb-139">The name of each group is the name of the user.</span></span> <span data-ttu-id="57bcb-140">Se um usuário tiver mais de uma conexão, cada id de conexão é adicionada ao grupo do usuário.</span><span class="sxs-lookup"><span data-stu-id="57bcb-140">If a user has more than one connection, each connection id is added to the user's group.</span></span>

<span data-ttu-id="57bcb-141">Você deve não remover manualmente o usuário do grupo quando o usuário se desconecta.</span><span class="sxs-lookup"><span data-stu-id="57bcb-141">You should not manually remove the user from the group when the user disconnects.</span></span> <span data-ttu-id="57bcb-142">Essa ação é executada automaticamente pela estrutura do SignalR.</span><span class="sxs-lookup"><span data-stu-id="57bcb-142">This action is automatically performed by the SignalR framework.</span></span>

<span data-ttu-id="57bcb-143">O exemplo a seguir mostra como implementar grupos de usuário único.</span><span class="sxs-lookup"><span data-stu-id="57bcb-143">The following example shows how to implement single-user groups.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample3.cs)]

<a id="database"></a>

## <a name="permanent-external-storage"></a><span data-ttu-id="57bcb-144">Armazenamento externo permanente</span><span class="sxs-lookup"><span data-stu-id="57bcb-144">Permanent, external storage</span></span>

<span data-ttu-id="57bcb-145">Este tópico mostra como usar um banco de dados ou o armazenamento de tabela do Azure para armazenar informações de conexão.</span><span class="sxs-lookup"><span data-stu-id="57bcb-145">This topic shows how to use either a database or Azure table storage for storing connection information.</span></span> <span data-ttu-id="57bcb-146">Essa abordagem funciona quando você tiver vários servidores web, porque cada servidor web pode interagir com o mesmo repositório de dados.</span><span class="sxs-lookup"><span data-stu-id="57bcb-146">This approach works when you have multiple web servers because each web server can interact with the same data repository.</span></span> <span data-ttu-id="57bcb-147">Se os servidores web parar de funcionar ou o aplicativo ser reiniciado, o `OnDisconnected` método não for chamado.</span><span class="sxs-lookup"><span data-stu-id="57bcb-147">If your web servers stop working or the application restarts, the `OnDisconnected` method is not called.</span></span> <span data-ttu-id="57bcb-148">Portanto, é possível que o repositório de dados tenha registros para as ids de conexão que não são mais válidas.</span><span class="sxs-lookup"><span data-stu-id="57bcb-148">Therefore, it is possible that your data repository will have records for connection ids that are no longer valid.</span></span> <span data-ttu-id="57bcb-149">Para limpar esses registros órfãos, você poderá invalidar qualquer conexão que foi criado fora do período de tempo que são relevante para seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="57bcb-149">To clean up these orphaned records, you may wish to invalidate any connection that was created outside of a timeframe that is relevant to your application.</span></span> <span data-ttu-id="57bcb-150">Os exemplos nesta seção incluem um valor para controlar quando a conexão foi criada, mas não mostram como limpar registros antigos porque talvez você queira fazer isso como um processo em segundo plano.</span><span class="sxs-lookup"><span data-stu-id="57bcb-150">The examples in this section include a value for tracking when the connection was created, but do not show how to clean up old records because you may want to do that as background process.</span></span>

### <a name="database"></a><span data-ttu-id="57bcb-151">Banco de Dados</span><span class="sxs-lookup"><span data-stu-id="57bcb-151">Database</span></span>

<span data-ttu-id="57bcb-152">Os exemplos a seguir mostram como reter informações de conexão e do usuário em um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="57bcb-152">The following examples show how to retain connection and user information in a database.</span></span> <span data-ttu-id="57bcb-153">Você pode usar qualquer tecnologia de acesso a dados; No entanto, o exemplo a seguir mostra como definir modelos usando o Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="57bcb-153">You can use any data access technology; however, the example below shows how to define models using Entity Framework.</span></span> <span data-ttu-id="57bcb-154">Esses modelos de entidade correspondem a campos e as tabelas de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="57bcb-154">These entity models correspond to database tables and fields.</span></span> <span data-ttu-id="57bcb-155">A estrutura de dados pode variar consideravelmente dependendo dos requisitos do seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="57bcb-155">Your data structure could vary considerably depending on the requirements of your application.</span></span>

<span data-ttu-id="57bcb-156">O primeiro exemplo mostra como definir uma entidade de usuário que pode ser associada a várias entidades de conexão.</span><span class="sxs-lookup"><span data-stu-id="57bcb-156">The first example shows how to define a user entity that can be associated with many connection entities.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample4.cs)]

<span data-ttu-id="57bcb-157">Em seguida, do hub, você pode controlar o estado de cada conexão com o código mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="57bcb-157">Then, from the hub, you can track the state of each connection with the code shown below.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample5.cs)]

### <a name="azure-table-storage"></a><span data-ttu-id="57bcb-158">Armazenamento de tabela do Azure</span><span class="sxs-lookup"><span data-stu-id="57bcb-158">Azure table storage</span></span>

<span data-ttu-id="57bcb-159">O seguinte exemplo de armazenamento de tabela do Azure é semelhante ao exemplo de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="57bcb-159">The following Azure table storage example is similar to the database example.</span></span> <span data-ttu-id="57bcb-160">Ele não inclui todas as informações que você precisa começar com o serviço de armazenamento de tabela do Azure.</span><span class="sxs-lookup"><span data-stu-id="57bcb-160">It does not include all of the information that you would need to get started with Azure Table Storage Service.</span></span> <span data-ttu-id="57bcb-161">Para obter informações, consulte [como usar o armazenamento de tabela do .NET](https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-how-to-use-tables/).</span><span class="sxs-lookup"><span data-stu-id="57bcb-161">For information, see [How to use Table storage from .NET](https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-how-to-use-tables/).</span></span>

<span data-ttu-id="57bcb-162">O exemplo a seguir mostra uma entidade de tabela para armazenar informações de conexão.</span><span class="sxs-lookup"><span data-stu-id="57bcb-162">The following example shows a table entity for storing connection information.</span></span> <span data-ttu-id="57bcb-163">Ela particiona os dados por nome de usuário e identifica cada entidade pela id de conexão, para que um usuário pode ter várias conexões a qualquer momento.</span><span class="sxs-lookup"><span data-stu-id="57bcb-163">It partitions the data by user name, and identifies each entity by the connection id, so a user can have multiple connections at any time.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample6.cs)]

<span data-ttu-id="57bcb-164">No hub, você acompanhar o status de conexão de cada usuário.</span><span class="sxs-lookup"><span data-stu-id="57bcb-164">In the hub, you track the status of each user's connection.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample7.cs)]

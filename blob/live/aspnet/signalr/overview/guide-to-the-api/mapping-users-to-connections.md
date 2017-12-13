---
uid: signalr/overview/guide-to-the-api/mapping-users-to-connections
title: "Mapeamento de usuários do SignalR para conexões | Microsoft Docs"
author: tfitzmac
description: "Este tópico mostra como reter informações sobre usuários e suas conexões. Patrick Fletcher ajudou a gravação neste tópico. Versões de software usadas neste tópico..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/30/2014
ms.topic: article
ms.assetid: f80c08b1-3f1f-432c-980c-c7b6edeb31b1
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/guide-to-the-api/mapping-users-to-connections
msc.type: authoredcontent
ms.openlocfilehash: 9b50d8805beabbc48467e20331c7593de9bc4254
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="mapping-signalr-users-to-connections"></a><span data-ttu-id="30ae2-105">Mapeamento de usuários do SignalR para conexões</span><span class="sxs-lookup"><span data-stu-id="30ae2-105">Mapping SignalR Users to Connections</span></span>
====================
<span data-ttu-id="30ae2-106">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="30ae2-106">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="30ae2-107">Este tópico mostra como reter informações sobre usuários e suas conexões.</span><span class="sxs-lookup"><span data-stu-id="30ae2-107">This topic shows how to retain information about users and their connections.</span></span>
> 
> <span data-ttu-id="30ae2-108">Patrick Fletcher ajudou a gravação neste tópico.</span><span class="sxs-lookup"><span data-stu-id="30ae2-108">Patrick Fletcher helped write this topic.</span></span>
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="30ae2-109">Versões de software usadas neste tópico</span><span class="sxs-lookup"><span data-stu-id="30ae2-109">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="30ae2-110">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="30ae2-110">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="30ae2-111">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="30ae2-111">.NET 4.5</span></span>
> - <span data-ttu-id="30ae2-112">SignalR versão 2</span><span class="sxs-lookup"><span data-stu-id="30ae2-112">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="30ae2-113">Versões anteriores deste tópico</span><span class="sxs-lookup"><span data-stu-id="30ae2-113">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="30ae2-114">Para obter informações sobre versões anteriores do SignalR, consulte [versões mais antigas do SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="30ae2-114">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="30ae2-115">Perguntas e comentários</span><span class="sxs-lookup"><span data-stu-id="30ae2-115">Questions and comments</span></span>
> 
> <span data-ttu-id="30ae2-116">Deixe comentários em como você gostou neste tutorial e o que podemos melhorar nos comentários na parte inferior da página.</span><span class="sxs-lookup"><span data-stu-id="30ae2-116">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="30ae2-117">Se você tiver dúvidas que não estão diretamente relacionadas ao tutorial, você poderá postá-los para o [ASP.NET SignalR fórum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="30ae2-117">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="introduction"></a><span data-ttu-id="30ae2-118">Introdução</span><span class="sxs-lookup"><span data-stu-id="30ae2-118">Introduction</span></span>

<span data-ttu-id="30ae2-119">Cada cliente que se conecta a um hub passa uma id de conexão exclusivo. Você pode recuperar esse valor no `Context.ConnectionId` propriedade do contexto do hub.</span><span class="sxs-lookup"><span data-stu-id="30ae2-119">Each client connecting to a hub passes a unique connection id. You can retrieve this value in the `Context.ConnectionId` property of the hub context.</span></span> <span data-ttu-id="30ae2-120">Se seu aplicativo precisa mapear um usuário para a id de conexão e manter esse mapeamento, você pode usar um dos seguintes:</span><span class="sxs-lookup"><span data-stu-id="30ae2-120">If your application needs to map a user to the connection id and persist that mapping, you can use one of the following:</span></span>

- [<span data-ttu-id="30ae2-121">O provedor de ID de usuário (SignalR 2)</span><span class="sxs-lookup"><span data-stu-id="30ae2-121">The User ID Provider (SignalR 2)</span></span>](#IUserIdProvider)
- <span data-ttu-id="30ae2-122">[Armazenamento na memória](#inmemory), como um dicionário</span><span class="sxs-lookup"><span data-stu-id="30ae2-122">[In-memory storage](#inmemory), such as a dictionary</span></span>
- [<span data-ttu-id="30ae2-123">Grupo de SignalR para cada usuário</span><span class="sxs-lookup"><span data-stu-id="30ae2-123">SignalR group for each user</span></span>](#groups)
- <span data-ttu-id="30ae2-124">[Armazenamento externo permanente](#database), como uma tabela de banco de dados ou armazenamento de tabela do Azure</span><span class="sxs-lookup"><span data-stu-id="30ae2-124">[Permanent, external storage](#database), such as a database table or Azure table storage</span></span>

<span data-ttu-id="30ae2-125">Cada uma dessas implementações é mostrada neste tópico.</span><span class="sxs-lookup"><span data-stu-id="30ae2-125">Each of these implementations is shown in this topic.</span></span> <span data-ttu-id="30ae2-126">Você usa o `OnConnected`, `OnDisconnected`, e `OnReconnected` métodos do `Hub` classe para acompanhar o status de conexão do usuário.</span><span class="sxs-lookup"><span data-stu-id="30ae2-126">You use the `OnConnected`, `OnDisconnected`, and `OnReconnected` methods of the `Hub` class to track the user connection status.</span></span>

<span data-ttu-id="30ae2-127">A melhor abordagem para seu aplicativo depende de:</span><span class="sxs-lookup"><span data-stu-id="30ae2-127">The best approach for your application depends on:</span></span>

- <span data-ttu-id="30ae2-128">O número de servidores web hospedando o seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="30ae2-128">The number of web servers hosting your application.</span></span>
- <span data-ttu-id="30ae2-129">Se você precisa obter uma lista dos usuários conectados no momento.</span><span class="sxs-lookup"><span data-stu-id="30ae2-129">Whether you need to get a list of the currently connected users.</span></span>
- <span data-ttu-id="30ae2-130">Se você precisa manter informações de grupo e usuário quando o aplicativo ou o servidor for reiniciado.</span><span class="sxs-lookup"><span data-stu-id="30ae2-130">Whether you need to persist group and user information when the application or server restarts.</span></span>
- <span data-ttu-id="30ae2-131">Se a latência de chamada de um servidor externo é um problema.</span><span class="sxs-lookup"><span data-stu-id="30ae2-131">Whether the latency of calling an external server is an issue.</span></span>

<span data-ttu-id="30ae2-132">A tabela a seguir mostra qual abordagem funciona para essas considerações.</span><span class="sxs-lookup"><span data-stu-id="30ae2-132">The following table shows which approach works for these considerations.</span></span>

|  | <span data-ttu-id="30ae2-133">Mais de um servidor</span><span class="sxs-lookup"><span data-stu-id="30ae2-133">More than one server</span></span> | <span data-ttu-id="30ae2-134">Obter lista de usuários conectados no momento</span><span class="sxs-lookup"><span data-stu-id="30ae2-134">Get list of currently connected users</span></span> | <span data-ttu-id="30ae2-135">Manter as informações depois que for reiniciado</span><span class="sxs-lookup"><span data-stu-id="30ae2-135">Persist information after restarts</span></span> | <span data-ttu-id="30ae2-136">Desempenho ideal</span><span class="sxs-lookup"><span data-stu-id="30ae2-136">Optimal performance</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="30ae2-137">Provedor de ID de usuário</span><span class="sxs-lookup"><span data-stu-id="30ae2-137">UserID Provider</span></span> | ![](mapping-users-to-connections/_static/image1.png) |  |  | ![](mapping-users-to-connections/_static/image2.png) |
| <span data-ttu-id="30ae2-138">Na memória</span><span class="sxs-lookup"><span data-stu-id="30ae2-138">In-memory</span></span> |  | ![](mapping-users-to-connections/_static/image3.png) |  | ![](mapping-users-to-connections/_static/image4.png) |
| <span data-ttu-id="30ae2-139">Grupos de usuário único</span><span class="sxs-lookup"><span data-stu-id="30ae2-139">Single-user groups</span></span> | ![](mapping-users-to-connections/_static/image5.png) |  |  | ![](mapping-users-to-connections/_static/image6.png) |
| <span data-ttu-id="30ae2-140">Permanentes, externa</span><span class="sxs-lookup"><span data-stu-id="30ae2-140">Permanent, external</span></span> | ![](mapping-users-to-connections/_static/image7.png) | ![](mapping-users-to-connections/_static/image8.png) | ![](mapping-users-to-connections/_static/image9.png) |  |

<a id="IUserIdProvider"></a>

## <a name="iuserid-provider"></a><span data-ttu-id="30ae2-141">Provedor de IUserID</span><span class="sxs-lookup"><span data-stu-id="30ae2-141">IUserID provider</span></span>

<span data-ttu-id="30ae2-142">Esse recurso permite que os usuários especifiquem qual é a ID do usuário com base em um IRequest por meio de uma nova interface IUserIdProvider.</span><span class="sxs-lookup"><span data-stu-id="30ae2-142">This feature allows users to specify what the userId is based on an IRequest via a new interface IUserIdProvider.</span></span>

<span data-ttu-id="30ae2-143">**O IUserIdProvider**</span><span class="sxs-lookup"><span data-stu-id="30ae2-143">**The IUserIdProvider**</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample1.cs)]

<span data-ttu-id="30ae2-144">Por padrão, haverá uma implementação que usa o usuário `IPrincipal.Identity.Name` como o nome de usuário.</span><span class="sxs-lookup"><span data-stu-id="30ae2-144">By default, there will be an implementation that uses the user's `IPrincipal.Identity.Name` as the user name.</span></span> <span data-ttu-id="30ae2-145">Para alterar isso, registrar sua implementação de `IUserIdProvider` com o host global quando o aplicativo for iniciado:</span><span class="sxs-lookup"><span data-stu-id="30ae2-145">To change this, register your implementation of `IUserIdProvider` with the global host when your application starts:</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample2.cs)]

<span data-ttu-id="30ae2-146">De dentro de um hub, você poderá enviar mensagens para esses usuários por meio da API a seguir:</span><span class="sxs-lookup"><span data-stu-id="30ae2-146">From within a hub, you'll be able to send messages to these users via the following API:</span></span>

<span data-ttu-id="30ae2-147">**Enviando uma mensagem para um usuário específico**</span><span class="sxs-lookup"><span data-stu-id="30ae2-147">**Sending a message to a specific user**</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample3.cs?highlight=5)]

<a id="inmemory"></a>

## <a name="in-memory-storage"></a><span data-ttu-id="30ae2-148">Armazenamento na memória</span><span class="sxs-lookup"><span data-stu-id="30ae2-148">In-memory storage</span></span>

<span data-ttu-id="30ae2-149">Os exemplos a seguir mostram como reter informações de conexão e do usuário em um dicionário que é armazenado na memória.</span><span class="sxs-lookup"><span data-stu-id="30ae2-149">The following examples show how to retain connection and user information in a dictionary that is stored in memory.</span></span> <span data-ttu-id="30ae2-150">O dicionário usa um `HashSet` para armazenar a id de conexão. A qualquer momento, um usuário pode ter mais de uma conexão para o aplicativo do SignalR.</span><span class="sxs-lookup"><span data-stu-id="30ae2-150">The dictionary uses a `HashSet` to store the connection id. At any time a user could have more than one connection to the SignalR application.</span></span> <span data-ttu-id="30ae2-151">Por exemplo, um usuário que está conectado por meio de vários dispositivos ou mais de uma guia do navegador teria mais de uma id de conexão.</span><span class="sxs-lookup"><span data-stu-id="30ae2-151">For example, a user who is connected through multiple devices or more than one browser tab would have more than one connection id.</span></span>

<span data-ttu-id="30ae2-152">Se o aplicativo for fechado, todas as informações é perdida, mas ele será novamente preenchido conforme os usuários restabelecer a conexão.</span><span class="sxs-lookup"><span data-stu-id="30ae2-152">If the application shuts down, all of the information is lost, but it will be re-populated as the users re-establish their connections.</span></span> <span data-ttu-id="30ae2-153">Armazenamento na memória não funcionará se o seu ambiente incluir mais de um servidor web, porque cada servidor deve ter um conjunto separado de conexões.</span><span class="sxs-lookup"><span data-stu-id="30ae2-153">In-memory storage does not work if your environment includes more than one web server because each server would have a separate collection of connections.</span></span>

<span data-ttu-id="30ae2-154">O primeiro exemplo mostra uma classe que gerencia o mapeamento de usuários para conexões.</span><span class="sxs-lookup"><span data-stu-id="30ae2-154">The first example shows a class that manages the mapping of users to connections.</span></span> <span data-ttu-id="30ae2-155">A chave para o HashSet será o nome do usuário.</span><span class="sxs-lookup"><span data-stu-id="30ae2-155">The key for the HashSet will be the user's name.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample4.cs)]

<span data-ttu-id="30ae2-156">O exemplo a seguir mostra como usar a classe de mapeamento de conexão de um hub.</span><span class="sxs-lookup"><span data-stu-id="30ae2-156">The next example shows how to use the connection mapping class from a hub.</span></span> <span data-ttu-id="30ae2-157">A instância da classe é armazenada em um nome de variável `_connections`.</span><span class="sxs-lookup"><span data-stu-id="30ae2-157">The instance of the class is stored in a variable name `_connections`.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample5.cs)]

<a id="groups"></a>

## <a name="single-user-groups"></a><span data-ttu-id="30ae2-158">Grupos de usuário único</span><span class="sxs-lookup"><span data-stu-id="30ae2-158">Single-user groups</span></span>

<span data-ttu-id="30ae2-159">Você pode criar um grupo para cada usuário e, em seguida, enviar uma mensagem a esse grupo quando quiser acessar somente esse usuário.</span><span class="sxs-lookup"><span data-stu-id="30ae2-159">You can create a group for each user, and then send a message to that group when you want to reach only that user.</span></span> <span data-ttu-id="30ae2-160">O nome de cada grupo é o nome do usuário.</span><span class="sxs-lookup"><span data-stu-id="30ae2-160">The name of each group is the name of the user.</span></span> <span data-ttu-id="30ae2-161">Se um usuário tiver mais de uma conexão, cada id de conexão é adicionada ao grupo do usuário.</span><span class="sxs-lookup"><span data-stu-id="30ae2-161">If a user has more than one connection, each connection id is added to the user's group.</span></span>

<span data-ttu-id="30ae2-162">Você deve não remover manualmente o usuário do grupo quando o usuário se desconecta.</span><span class="sxs-lookup"><span data-stu-id="30ae2-162">You should not manually remove the user from the group when the user disconnects.</span></span> <span data-ttu-id="30ae2-163">Essa ação é executada automaticamente pela estrutura do SignalR.</span><span class="sxs-lookup"><span data-stu-id="30ae2-163">This action is automatically performed by the SignalR framework.</span></span>

<span data-ttu-id="30ae2-164">O exemplo a seguir mostra como implementar grupos de usuário único.</span><span class="sxs-lookup"><span data-stu-id="30ae2-164">The following example shows how to implement single-user groups.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample6.cs)]

<a id="database"></a>

## <a name="permanent-external-storage"></a><span data-ttu-id="30ae2-165">Armazenamento externo permanente</span><span class="sxs-lookup"><span data-stu-id="30ae2-165">Permanent, external storage</span></span>

<span data-ttu-id="30ae2-166">Este tópico mostra como usar um banco de dados ou o armazenamento de tabela do Azure para armazenar informações de conexão.</span><span class="sxs-lookup"><span data-stu-id="30ae2-166">This topic shows how to use either a database or Azure table storage for storing connection information.</span></span> <span data-ttu-id="30ae2-167">Essa abordagem funciona quando você tiver vários servidores web, porque cada servidor web pode interagir com o mesmo repositório de dados.</span><span class="sxs-lookup"><span data-stu-id="30ae2-167">This approach works when you have multiple web servers because each web server can interact with the same data repository.</span></span> <span data-ttu-id="30ae2-168">Se os servidores web parar de funcionar ou o aplicativo ser reiniciado, o `OnDisconnected` método não for chamado.</span><span class="sxs-lookup"><span data-stu-id="30ae2-168">If your web servers stop working or the application restarts, the `OnDisconnected` method is not called.</span></span> <span data-ttu-id="30ae2-169">Portanto, é possível que o repositório de dados tenha registros para as ids de conexão que não são mais válidas.</span><span class="sxs-lookup"><span data-stu-id="30ae2-169">Therefore, it is possible that your data repository will have records for connection ids that are no longer valid.</span></span> <span data-ttu-id="30ae2-170">Para limpar esses registros órfãos, você poderá invalidar qualquer conexão que foi criado fora do período de tempo que são relevante para seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="30ae2-170">To clean up these orphaned records, you may wish to invalidate any connection that was created outside of a timeframe that is relevant to your application.</span></span> <span data-ttu-id="30ae2-171">Os exemplos nesta seção incluem um valor para controlar quando a conexão foi criada, mas não mostram como limpar registros antigos porque talvez você queira fazer isso como um processo em segundo plano.</span><span class="sxs-lookup"><span data-stu-id="30ae2-171">The examples in this section include a value for tracking when the connection was created, but do not show how to clean up old records because you may want to do that as background process.</span></span>

### <a name="database"></a><span data-ttu-id="30ae2-172">Banco de Dados</span><span class="sxs-lookup"><span data-stu-id="30ae2-172">Database</span></span>

<span data-ttu-id="30ae2-173">Os exemplos a seguir mostram como reter informações de conexão e do usuário em um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="30ae2-173">The following examples show how to retain connection and user information in a database.</span></span> <span data-ttu-id="30ae2-174">Você pode usar qualquer tecnologia de acesso a dados; No entanto, o exemplo a seguir mostra como definir modelos usando o Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="30ae2-174">You can use any data access technology; however, the example below shows how to define models using Entity Framework.</span></span> <span data-ttu-id="30ae2-175">Esses modelos de entidade correspondem a campos e as tabelas de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="30ae2-175">These entity models correspond to database tables and fields.</span></span> <span data-ttu-id="30ae2-176">A estrutura de dados pode variar consideravelmente dependendo dos requisitos do seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="30ae2-176">Your data structure could vary considerably depending on the requirements of your application.</span></span>

<span data-ttu-id="30ae2-177">O primeiro exemplo mostra como definir uma entidade de usuário que pode ser associada a várias entidades de conexão.</span><span class="sxs-lookup"><span data-stu-id="30ae2-177">The first example shows how to define a user entity that can be associated with many connection entities.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample7.cs)]

<span data-ttu-id="30ae2-178">Em seguida, do hub, você pode controlar o estado de cada conexão com o código mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="30ae2-178">Then, from the hub, you can track the state of each connection with the code shown below.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample8.cs)]

<a id="azure"></a>
### <a name="azure-table-storage"></a><span data-ttu-id="30ae2-179">Armazenamento de tabela do Azure</span><span class="sxs-lookup"><span data-stu-id="30ae2-179">Azure table storage</span></span>

<span data-ttu-id="30ae2-180">O seguinte exemplo de armazenamento de tabela do Azure é semelhante ao exemplo de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="30ae2-180">The following Azure table storage example is similar to the database example.</span></span> <span data-ttu-id="30ae2-181">Ele não inclui todas as informações que você precisa começar com o serviço de armazenamento de tabela do Azure.</span><span class="sxs-lookup"><span data-stu-id="30ae2-181">It does not include all of the information that you would need to get started with Azure Table Storage Service.</span></span> <span data-ttu-id="30ae2-182">Para obter informações, consulte [como usar o armazenamento de tabela do .NET](https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-how-to-use-tables/).</span><span class="sxs-lookup"><span data-stu-id="30ae2-182">For information, see [How to use Table storage from .NET](https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-how-to-use-tables/).</span></span>

<span data-ttu-id="30ae2-183">O exemplo a seguir mostra uma entidade de tabela para armazenar informações de conexão.</span><span class="sxs-lookup"><span data-stu-id="30ae2-183">The following example shows a table entity for storing connection information.</span></span> <span data-ttu-id="30ae2-184">Ela particiona os dados por nome de usuário e identifica cada entidade pela id de conexão, para que um usuário pode ter várias conexões a qualquer momento.</span><span class="sxs-lookup"><span data-stu-id="30ae2-184">It partitions the data by user name, and identifies each entity by the connection id, so a user can have multiple connections at any time.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample9.cs)]

<span data-ttu-id="30ae2-185">No hub, você acompanhar o status de conexão de cada usuário.</span><span class="sxs-lookup"><span data-stu-id="30ae2-185">In the hub, you track the status of each user's connection.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample10.cs)]

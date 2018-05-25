---
uid: signalr/overview/older-versions/working-with-groups
title: Trabalhando com grupos no SignalR 1. x | Microsoft Docs
author: pfletcher
description: Este tópico descreve como manter informações de associação de grupo com a API de Hub.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/21/2013
ms.topic: article
ms.assetid: 22929efd-68c9-4609-b76d-f8ba42fda01e
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/working-with-groups
msc.type: authoredcontent
ms.openlocfilehash: 7bc0ff73ade72729cc5e1217b3fe704ac0d8cab8
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="working-with-groups-in-signalr-1x"></a><span data-ttu-id="e2b7f-103">Trabalhando com grupos no SignalR 1. x</span><span class="sxs-lookup"><span data-stu-id="e2b7f-103">Working with Groups in SignalR 1.x</span></span>
====================
<span data-ttu-id="e2b7f-104">por [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="e2b7f-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="e2b7f-105">Este tópico descreve como adicionar usuários a grupos e manter as informações de associação de grupo.</span><span class="sxs-lookup"><span data-stu-id="e2b7f-105">This topic describes how to add users to groups and persist group membership information.</span></span>


## <a name="overview"></a><span data-ttu-id="e2b7f-106">Visão geral</span><span class="sxs-lookup"><span data-stu-id="e2b7f-106">Overview</span></span>

<span data-ttu-id="e2b7f-107">Grupos no SignalR fornecem um método para mensagens de difusão de subconjuntos especificados de clientes conectados.</span><span class="sxs-lookup"><span data-stu-id="e2b7f-107">Groups in SignalR provide a method for broadcasting messages to specified subsets of connected clients.</span></span> <span data-ttu-id="e2b7f-108">Um grupo pode ter qualquer número de clientes e um cliente pode ser um membro de qualquer número de grupos.</span><span class="sxs-lookup"><span data-stu-id="e2b7f-108">A group can have any number of clients, and a client can be a member of any number of groups.</span></span> <span data-ttu-id="e2b7f-109">Você não precisa criar explicitamente grupos.</span><span class="sxs-lookup"><span data-stu-id="e2b7f-109">You don't have to explicitly create groups.</span></span> <span data-ttu-id="e2b7f-110">Na verdade, um grupo é criado automaticamente na primeira vez que você especifique seu nome em uma chamada para Groups.Add, e ela será excluída quando você remover a última conexão de associação nele.</span><span class="sxs-lookup"><span data-stu-id="e2b7f-110">In effect, a group is automatically created the first time you specify its name in a call to Groups.Add, and it is deleted when you remove the last connection from membership in it.</span></span> <span data-ttu-id="e2b7f-111">Para obter uma introdução ao uso de grupos, consulte [como gerenciar a associação de grupo da classe Hub](index.md) na API de Hubs - guia servidor.</span><span class="sxs-lookup"><span data-stu-id="e2b7f-111">For an introduction to using groups, see [How to manage group membership from the Hub class](index.md) in the Hubs API - Server Guide.</span></span>

<span data-ttu-id="e2b7f-112">Há uma API para obter uma lista de associação de grupo ou uma lista de grupos.</span><span class="sxs-lookup"><span data-stu-id="e2b7f-112">There is no API for getting a group membership list or a list of groups.</span></span> <span data-ttu-id="e2b7f-113">SignalR envia mensagens para os clientes e grupos com base em um modelo de publicação/assinatura e o servidor não mantém listas de grupos ou associações de grupo.</span><span class="sxs-lookup"><span data-stu-id="e2b7f-113">SignalR sends messages to clients and groups based on a pub/sub model, and the server does not maintain lists of groups or group memberships.</span></span> <span data-ttu-id="e2b7f-114">Isso ajuda a maximizar a escalabilidade, porque sempre que você adicionar um nó a uma web farm, qualquer estado que mantém o SignalR tem sejam propagadas para o novo nó.</span><span class="sxs-lookup"><span data-stu-id="e2b7f-114">This helps maximize scalability, because whenever you add a node to a web farm, any state that SignalR maintains has to be propagated to the new node.</span></span>

<span data-ttu-id="e2b7f-115">Quando você adiciona um usuário a um grupo usando o `Groups.Add` método, o usuário recebe mensagens direcionadas ao grupo durante a conexão atual, mas a associação do usuário desse grupo não persiste além de conexão atual.</span><span class="sxs-lookup"><span data-stu-id="e2b7f-115">When you add a user to a group using the `Groups.Add` method, the user receives messages directed to that group for the duration of the current connection, but the user's membership in that group is not persisted beyond the current connection.</span></span> <span data-ttu-id="e2b7f-116">Se você deseja manter permanentemente informações sobre grupos e membros do grupo, você deve armazenar dados em um repositório, como um banco de dados ou armazenamento de tabela do Azure.</span><span class="sxs-lookup"><span data-stu-id="e2b7f-116">If you want to permanently retain information about groups and group membership, you must store that data in a repository such as a database or Azure table storage.</span></span> <span data-ttu-id="e2b7f-117">Em seguida, cada vez que um usuário se conecta ao seu aplicativo, você recuperar do repositório de quais grupos o usuário pertence e manualmente adicionar esse usuário a esses grupos.</span><span class="sxs-lookup"><span data-stu-id="e2b7f-117">Then, each time a user connects to your application, you retrieve from the repository which groups the user belongs to, and manually add that user to those groups.</span></span>

<span data-ttu-id="e2b7f-118">Ao reconectar após uma interrupção temporária, o usuário novamente associa automaticamente os grupos atribuídos previamente.</span><span class="sxs-lookup"><span data-stu-id="e2b7f-118">When reconnecting after a temporary disruption, the user automatically re-joins the previously-assigned groups.</span></span> <span data-ttu-id="e2b7f-119">Associando automaticamente um grupo se aplica somente ao reconectar, não ao estabelecer uma nova conexão.</span><span class="sxs-lookup"><span data-stu-id="e2b7f-119">Automatically rejoining a group only applies when reconnecting, not when establishing a new connection.</span></span> <span data-ttu-id="e2b7f-120">Um token assinado digitalmente é passado do cliente que contém a lista de grupos atribuídos previamente.</span><span class="sxs-lookup"><span data-stu-id="e2b7f-120">A digitally-signed token is passed from the client that contains the list of previously-assigned groups.</span></span> <span data-ttu-id="e2b7f-121">Se você quiser verificar se o usuário pertence aos grupos solicitados, você pode substituir o comportamento padrão.</span><span class="sxs-lookup"><span data-stu-id="e2b7f-121">If you want to verify whether the user belongs to the requested groups, you can override the default behavior.</span></span>

<span data-ttu-id="e2b7f-122">Este tópico inclui as seções a seguir:</span><span class="sxs-lookup"><span data-stu-id="e2b7f-122">This topic includes the following sections:</span></span>

- [<span data-ttu-id="e2b7f-123">Adicionando e removendo usuários</span><span class="sxs-lookup"><span data-stu-id="e2b7f-123">Adding and removing users</span></span>](#add)
- [<span data-ttu-id="e2b7f-124">Chamando membros de um grupo</span><span class="sxs-lookup"><span data-stu-id="e2b7f-124">Calling members of a group</span></span>](#call)
- [<span data-ttu-id="e2b7f-125">Associação de grupo de armazenamento em um banco de dados</span><span class="sxs-lookup"><span data-stu-id="e2b7f-125">Storing group membership in a database</span></span>](#storedatabase)
- [<span data-ttu-id="e2b7f-126">Associação de grupo de armazenamento no armazenamento de tabela do Azure</span><span class="sxs-lookup"><span data-stu-id="e2b7f-126">Storing group membership in Azure table storage</span></span>](#storeazuretable)
- [<span data-ttu-id="e2b7f-127">Verificar a associação de grupo ao reconectar</span><span class="sxs-lookup"><span data-stu-id="e2b7f-127">Verifying group membership when reconnecting</span></span>](#verify)

<a id="add"></a>

## <a name="adding-and-removing-users"></a><span data-ttu-id="e2b7f-128">Adicionando e removendo usuários</span><span class="sxs-lookup"><span data-stu-id="e2b7f-128">Adding and removing users</span></span>

<span data-ttu-id="e2b7f-129">Para adicionar ou remover usuários de um grupo, você deve chamar o [adicionar](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) ou [remover](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) métodos e passe a id de conexão do usuário e o nome do grupo como parâmetros.</span><span class="sxs-lookup"><span data-stu-id="e2b7f-129">To add or remove users from a group, you call the [Add](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) or [Remove](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) methods, and pass in the user's connection id and group's name as parameters.</span></span> <span data-ttu-id="e2b7f-130">Você não precisa remover manualmente um usuário de um grupo quando termina a conexão.</span><span class="sxs-lookup"><span data-stu-id="e2b7f-130">You do not need to manually remove a user from a group when the connection ends.</span></span>

<span data-ttu-id="e2b7f-131">A exemplo a seguir mostra o `Groups.Add` e `Groups.Remove` métodos usados em métodos de Hub.</span><span class="sxs-lookup"><span data-stu-id="e2b7f-131">The following example shows the `Groups.Add` and `Groups.Remove` methods used in Hub methods.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample1.cs?highlight=5,10)]

<span data-ttu-id="e2b7f-132">O `Groups.Add` e `Groups.Remove` métodos execute de forma assíncrona.</span><span class="sxs-lookup"><span data-stu-id="e2b7f-132">The `Groups.Add` and `Groups.Remove` methods execute asynchronously.</span></span>

<span data-ttu-id="e2b7f-133">Se você quiser adicionar um cliente a um grupo e enviar imediatamente uma mensagem para o cliente usando o grupo, você precisa certificar-se de que o método Groups.Add termina primeiro.</span><span class="sxs-lookup"><span data-stu-id="e2b7f-133">If you want to add a client to a group and immediately send a message to the client by using the group, you have to make sure that the Groups.Add method finishes first.</span></span> <span data-ttu-id="e2b7f-134">Os exemplos de código a seguir mostram como fazer isso, um por meio de código que funciona no .NET 4.5 e um usando o código que funciona no .NET 4.</span><span class="sxs-lookup"><span data-stu-id="e2b7f-134">The following code examples show how to do that, one by using code that works in .NET 4.5, and one by using code that works in .NET 4.</span></span>

#### <a name="asynchronous-net-45-example"></a><span data-ttu-id="e2b7f-135">Assíncrono .NET 4.5 exemplo</span><span class="sxs-lookup"><span data-stu-id="e2b7f-135">Asynchronous .NET 4.5 Example</span></span>

[!code-csharp[Main](working-with-groups/samples/sample2.cs?highlight=1,3)]

#### <a name="asynchronous-net-4-example"></a><span data-ttu-id="e2b7f-136">Assíncrono .NET 4 exemplo</span><span class="sxs-lookup"><span data-stu-id="e2b7f-136">Asynchronous .NET 4 Example</span></span>

[!code-csharp[Main](working-with-groups/samples/sample3.cs?highlight=3-4)]

<span data-ttu-id="e2b7f-137">Em geral, você não deve incluir `await` ao chamar o `Groups.Remove` método porque a id de conexão que você está tentando remover pode não estar mais disponível.</span><span class="sxs-lookup"><span data-stu-id="e2b7f-137">In general, you should not include `await` when calling the `Groups.Remove` method because the connection id that you are trying to remove might no longer be available.</span></span> <span data-ttu-id="e2b7f-138">Nesse caso, `TaskCanceledException` é acionada depois que a solicitação expire. Se seu aplicativo deve garantir que o usuário foi removido do grupo antes de enviar uma mensagem para o grupo, você pode adicionar `await` antes de Groups.Remove e, em seguida, captura o `TaskCanceledException` exceção que pode ser gerada.</span><span class="sxs-lookup"><span data-stu-id="e2b7f-138">In that case, `TaskCanceledException` is thrown after the request times out. If your application must ensure that the user has been removed from the group before sending a message to the group, you can add `await` before Groups.Remove, and then catch the `TaskCanceledException` exception that might be thrown.</span></span>

<a id="call"></a>

## <a name="calling-members-of-a-group"></a><span data-ttu-id="e2b7f-139">Chamando membros de um grupo</span><span class="sxs-lookup"><span data-stu-id="e2b7f-139">Calling members of a group</span></span>

<span data-ttu-id="e2b7f-140">Você pode enviar mensagens para todos os membros de um grupo ou somente os membros especificados do grupo, conforme mostrado nos exemplos a seguir.</span><span class="sxs-lookup"><span data-stu-id="e2b7f-140">You can send messages to all of the members of a group or only specified members of the group, as shown in the following examples.</span></span>

- <span data-ttu-id="e2b7f-141">**Todos os** em um grupo especificado de clientes conectados.</span><span class="sxs-lookup"><span data-stu-id="e2b7f-141">**All** connected clients in a specified group.</span></span> 

    [!code-css[Main](working-with-groups/samples/sample4.css)]
- <span data-ttu-id="e2b7f-142">Todos os clientes em um grupo especificado conectados **clientes especificados, exceto**, identificado pela ID de conexão.</span><span class="sxs-lookup"><span data-stu-id="e2b7f-142">All connected clients in a specified group **except the specified clients**, identified by connection ID.</span></span> 

    [!code-csharp[Main](working-with-groups/samples/sample5.cs)]
- <span data-ttu-id="e2b7f-143">Todos os clientes em um grupo especificado conectados **exceto o cliente da chamada**.</span><span class="sxs-lookup"><span data-stu-id="e2b7f-143">All connected clients in a specified group **except the calling client**.</span></span> 

    [!code-css[Main](working-with-groups/samples/sample6.css)]

<a id="storedatabase"></a>

## <a name="storing-group-membership-in-a-database"></a><span data-ttu-id="e2b7f-144">Associação de grupo de armazenamento em um banco de dados</span><span class="sxs-lookup"><span data-stu-id="e2b7f-144">Storing group membership in a database</span></span>

<span data-ttu-id="e2b7f-145">Os exemplos a seguir mostram como reter informações de grupo e usuário em um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="e2b7f-145">The following examples show how to retain group and user information in a database.</span></span> <span data-ttu-id="e2b7f-146">Você pode usar qualquer tecnologia de acesso a dados; No entanto, o exemplo a seguir mostra como definir modelos usando o Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="e2b7f-146">You can use any data access technology; however, the example below shows how to define models using Entity Framework.</span></span> <span data-ttu-id="e2b7f-147">Esses modelos de entidade correspondem a campos e as tabelas de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="e2b7f-147">These entity models correspond to database tables and fields.</span></span> <span data-ttu-id="e2b7f-148">A estrutura de dados pode variar consideravelmente dependendo dos requisitos do seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="e2b7f-148">Your data structure could vary considerably depending on the requirements of your application.</span></span> <span data-ttu-id="e2b7f-149">Este exemplo inclui uma classe chamada `ConversationRoom` que deve ser exclusivo para um aplicativo que permite que os usuários ingressem conversas sobre entidades diferentes, como esportes ou ambiente.</span><span class="sxs-lookup"><span data-stu-id="e2b7f-149">This example includes a class named `ConversationRoom` which would be unique to an application that enables users to join conversations about different subjects, such as sports or gardening.</span></span> <span data-ttu-id="e2b7f-150">Este exemplo também inclui uma classe para as conexões.</span><span class="sxs-lookup"><span data-stu-id="e2b7f-150">This example also includes a class for the connections.</span></span> <span data-ttu-id="e2b7f-151">A classe de conexão não é absolutamente necessária para rastrear a associação de grupo, mas frequentemente é parte de uma solução robusta para usuários de controle.</span><span class="sxs-lookup"><span data-stu-id="e2b7f-151">The connection class is not absolutely required for tracking group membership but is frequently part of robust solution to tracking users.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample7.cs)]

<span data-ttu-id="e2b7f-152">Em seguida, no hub, você pode recuperar as informações de grupo e usuário do banco de dados e adicionar manualmente o usuário aos grupos apropriados.</span><span class="sxs-lookup"><span data-stu-id="e2b7f-152">Then, in the hub, you can retrieve the group and user information from the database and manually add the user to the appropriate groups.</span></span> <span data-ttu-id="e2b7f-153">O exemplo não inclui o código para controlar as conexões de usuário.</span><span class="sxs-lookup"><span data-stu-id="e2b7f-153">The example does not include code for tracking the user connections.</span></span> <span data-ttu-id="e2b7f-154">Neste exemplo, o `await` palavra-chave não é aplicado antes `Groups.Add` porque uma mensagem não é enviada imediatamente para membros do grupo.</span><span class="sxs-lookup"><span data-stu-id="e2b7f-154">In this example, the `await` keyword is not applied before `Groups.Add` because a message is not immediately sent to members of the group.</span></span> <span data-ttu-id="e2b7f-155">Se você quiser enviar uma mensagem a todos os membros do grupo imediatamente depois de adicionar o novo membro, você deseja aplicar o `await` palavra-chave para certificar-se de que a operação assíncrona foi concluída.</span><span class="sxs-lookup"><span data-stu-id="e2b7f-155">If you want to send a message to all members of the group immediately after adding the new member, you would want to apply the `await` keyword to make sure the asynchronous operation has completed.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample8.cs)]

<a id="storeazuretable"></a>

## <a name="storing-group-membership-in-azure-table-storage"></a><span data-ttu-id="e2b7f-156">Associação de grupo de armazenamento no armazenamento de tabela do Azure</span><span class="sxs-lookup"><span data-stu-id="e2b7f-156">Storing group membership in Azure table storage</span></span>

<span data-ttu-id="e2b7f-157">Usando o armazenamento de tabela do Azure para armazenar informações de usuário e grupo é semelhante ao uso de um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="e2b7f-157">Using Azure table storage to store group and user information is similar to using a database.</span></span> <span data-ttu-id="e2b7f-158">O exemplo a seguir mostra uma entidade de tabela que armazena o nome de usuário e o nome do grupo.</span><span class="sxs-lookup"><span data-stu-id="e2b7f-158">The following example shows a table entity that stores the user name and group name.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample9.cs)]

<span data-ttu-id="e2b7f-159">No hub, recuperar os grupos atribuídos quando o usuário se conecta.</span><span class="sxs-lookup"><span data-stu-id="e2b7f-159">In the hub, you retrieve the assigned groups when the user connects.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample10.cs)]

<a id="verify"></a>

## <a name="verifying-group-membership-when-reconnecting"></a><span data-ttu-id="e2b7f-160">Verificar a associação de grupo ao reconectar</span><span class="sxs-lookup"><span data-stu-id="e2b7f-160">Verifying group membership when reconnecting</span></span>

<span data-ttu-id="e2b7f-161">Por padrão, o SignalR novamente atribui automaticamente um usuário aos grupos apropriados ao reconectar-se de uma interrupção temporária, como quando uma conexão será descartada e restabelecida antes que a conexão expire. Informações de grupo do usuário são passadas em um token ao reconectar e esse token é verificado no servidor.</span><span class="sxs-lookup"><span data-stu-id="e2b7f-161">By default, SignalR automatically re-assigns a user to the appropriate groups when reconnecting from a temporary disruption, such as when a connection is dropped and re-established before the connection times out. The user's group information is passed in a token when reconnecting, and that token is verified on the server.</span></span> <span data-ttu-id="e2b7f-162">Para obter informações sobre o processo de verificação para incluir novamente grupos de usuários, consulte [incluir novamente grupos ao reconectar](index.md).</span><span class="sxs-lookup"><span data-stu-id="e2b7f-162">For information about the verification process for rejoining users to groups, see [Rejoining groups when reconnecting](index.md).</span></span>

<span data-ttu-id="e2b7f-163">Em geral, você deve usar o comportamento padrão de automaticamente incluir novamente grupos na reconexão.</span><span class="sxs-lookup"><span data-stu-id="e2b7f-163">In general, you should use the default behavior of automatically rejoining groups on reconnect.</span></span> <span data-ttu-id="e2b7f-164">SignalR grupos não são como um mecanismo de segurança para restringir o acesso a dados confidenciais.</span><span class="sxs-lookup"><span data-stu-id="e2b7f-164">SignalR groups are not intended as a security mechanism for restricting access to sensitive data.</span></span> <span data-ttu-id="e2b7f-165">No entanto, se seu aplicativo precisa verificar a associação de grupo do usuário ao reconectar, você pode substituir o comportamento padrão.</span><span class="sxs-lookup"><span data-stu-id="e2b7f-165">However, if your application must double-check a user's group membership when reconnecting, you can override the default behavior.</span></span> <span data-ttu-id="e2b7f-166">Alterar o comportamento padrão pode adicionar uma carga de seu banco de dados porque a associação de grupo do usuário deve ser recuperada para cada reconexão em vez de apenas quando o usuário se conecta.</span><span class="sxs-lookup"><span data-stu-id="e2b7f-166">Changing the default behavior can add a burden to your database because a user's group membership must be retrieved for each reconnection rather than just when the user connects.</span></span>

<span data-ttu-id="e2b7f-167">Se você deve verificar a associação de grupo em reconectar, crie um novo módulo de pipeline de hub que retorna uma lista de grupos atribuídos, conforme mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="e2b7f-167">If you must verify group membership on reconnect, create a new hub pipeline module that returns a list of assigned groups, as shown below.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample11.cs)]

<span data-ttu-id="e2b7f-168">Em seguida, adicione módulo ao pipeline do hub, como destacado abaixo.</span><span class="sxs-lookup"><span data-stu-id="e2b7f-168">Then, add that module to the hub pipeline, as highlighted below.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample12.cs?highlight=10)]

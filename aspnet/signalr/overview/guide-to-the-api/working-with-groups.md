---
uid: signalr/overview/guide-to-the-api/working-with-groups
title: Trabalhando com grupos no SignalR | Microsoft Docs
author: pfletcher
description: "Este tópico descreve como manter informações de associação de grupo com a API de Hub."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: cd378ecd-3e9e-4236-b902-65916d85a048
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/guide-to-the-api/working-with-groups
msc.type: authoredcontent
ms.openlocfilehash: 11f5be1ac4e74b692f0db3daac971a2c9d74a64c
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="working-with-groups-in-signalr"></a>Trabalhando com grupos no SignalR
====================
por [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)

> Este tópico descreve como adicionar usuários a grupos e manter as informações de associação de grupo. 
> 
> ## <a name="software-versions-used-in-this-topic"></a>Versões de software usadas neste tópico
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR versão 2
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a>Versões anteriores deste tópico
> 
> Para obter informações sobre versões anteriores do SignalR, consulte [versões mais antigas do SignalR](../older-versions/index.md).
> 
> ## <a name="questions-and-comments"></a>Perguntas e comentários
> 
> Deixe comentários em como você gostou neste tutorial e o que podemos melhorar nos comentários na parte inferior da página. Se você tiver dúvidas que não estão diretamente relacionadas ao tutorial, você poderá postá-los para o [ASP.NET SignalR fórum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [StackOverflow.com](http://stackoverflow.com/).


## <a name="overview"></a>Visão geral

Grupos no SignalR fornecem um método para mensagens de difusão de subconjuntos especificados de clientes conectados. Um grupo pode ter qualquer número de clientes e um cliente pode ser um membro de qualquer número de grupos. Você não precisa criar explicitamente grupos. Na verdade, um grupo é criado automaticamente na primeira vez que você especifique seu nome em uma chamada para Groups.Add, e ela será excluída quando você remover a última conexão de associação nele. Para obter uma introdução ao uso de grupos, consulte [como gerenciar a associação de grupo da classe Hub](hubs-api-guide-server.md#groupsfromhub) na API de Hubs - guia servidor.

Há uma API para obter uma lista de associação de grupo ou uma lista de grupos. SignalR envia mensagens para os clientes e grupos com base em um modelo de publicação/assinatura e o servidor não mantém listas de grupos ou associações de grupo. Isso ajuda a maximizar a escalabilidade, porque sempre que você adicionar um nó a uma web farm, qualquer estado que mantém o SignalR tem sejam propagadas para o novo nó.

Quando você adiciona um usuário a um grupo usando o `Groups.Add` método, o usuário recebe mensagens direcionadas ao grupo durante a conexão atual, mas a associação do usuário desse grupo não persiste além de conexão atual. Se você deseja manter permanentemente informações sobre grupos e membros do grupo, você deve armazenar dados em um repositório, como um banco de dados ou armazenamento de tabela do Azure. Em seguida, cada vez que um usuário se conecta ao seu aplicativo, você recuperar do repositório de quais grupos o usuário pertence e manualmente adicionar esse usuário a esses grupos.

Ao reconectar após uma interrupção temporária, o usuário novamente associa automaticamente os grupos atribuídos previamente. Associando automaticamente um grupo se aplica somente ao reconectar, não ao estabelecer uma nova conexão. Um token assinado digitalmente é passado do cliente que contém a lista de grupos atribuídos previamente. Se você quiser verificar se o usuário pertence aos grupos solicitados, você pode substituir o comportamento padrão.

Este tópico inclui as seções a seguir:

- [Adicionando e removendo usuários](#add)
- [Chamando membros de um grupo](#call)
- [Associação de grupo de armazenamento em um banco de dados](#storedatabase)
- [Associação de grupo de armazenamento no armazenamento de tabela do Azure](#storeazuretable)
- [Verificar a associação de grupo ao reconectar](#verify)

<a id="add"></a>

## <a name="adding-and-removing-users"></a>Adicionando e removendo usuários

Para adicionar ou remover usuários de um grupo, você deve chamar o [adicionar](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) ou [remover](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) métodos e passe a id de conexão do usuário e o nome do grupo como parâmetros. Você não precisa remover manualmente um usuário de um grupo quando termina a conexão.

A exemplo a seguir mostra o `Groups.Add` e `Groups.Remove` métodos usados em métodos de Hub.

[!code-csharp[Main](working-with-groups/samples/sample1.cs?highlight=5,10)]

O `Groups.Add` e `Groups.Remove` métodos execute de forma assíncrona.

Se você quiser adicionar um cliente a um grupo e enviar imediatamente uma mensagem para o cliente usando o grupo, você precisa certificar-se de que o método Groups.Add termina primeiro. Os exemplos de código a seguir mostram como fazer isso.

[!code-csharp[Main](working-with-groups/samples/sample2.cs?highlight=1,3)]

Em geral, você não deve incluir `await` ao chamar o `Groups.Remove` método porque a id de conexão que você está tentando remover pode não estar mais disponível. Nesse caso, `TaskCanceledException` é acionada depois que a solicitação expire. Se seu aplicativo deve garantir que o usuário foi removido do grupo antes de enviar uma mensagem para o grupo, você pode adicionar `await` antes de Groups.Remove e, em seguida, captura o `TaskCanceledException` exceção que pode ser gerada.

<a id="call"></a>

## <a name="calling-members-of-a-group"></a>Chamando membros de um grupo

Você pode enviar mensagens para todos os membros de um grupo ou somente os membros especificados do grupo, conforme mostrado nos exemplos a seguir.

- **Todos os** em um grupo especificado de clientes conectados. 

    [!code-css[Main](working-with-groups/samples/sample3.css)]
- Todos os clientes em um grupo especificado conectados **clientes especificados, exceto**, identificado pela ID de conexão. 

    [!code-csharp[Main](working-with-groups/samples/sample4.cs)]
- Todos os clientes em um grupo especificado conectados **exceto o cliente da chamada**. 

    [!code-css[Main](working-with-groups/samples/sample5.css)]

<a id="storedatabase"></a>

## <a name="storing-group-membership-in-a-database"></a>Associação de grupo de armazenamento em um banco de dados

Os exemplos a seguir mostram como reter informações de grupo e usuário em um banco de dados. Você pode usar qualquer tecnologia de acesso a dados; No entanto, o exemplo a seguir mostra como definir modelos usando o Entity Framework. Esses modelos de entidade correspondem a campos e as tabelas de banco de dados. A estrutura de dados pode variar consideravelmente dependendo dos requisitos do seu aplicativo. Este exemplo inclui uma classe chamada `ConversationRoom` que deve ser exclusivo para um aplicativo que permite que os usuários ingressem conversas sobre entidades diferentes, como esportes ou ambiente. Este exemplo também inclui uma classe para as conexões. A classe de conexão não é absolutamente necessária para rastrear a associação de grupo, mas frequentemente é parte de uma solução robusta para usuários de controle.

[!code-csharp[Main](working-with-groups/samples/sample6.cs)]

Em seguida, no hub, você pode recuperar as informações de grupo e usuário do banco de dados e adicionar manualmente o usuário aos grupos apropriados. O exemplo não inclui o código para controlar as conexões de usuário. Neste exemplo, o `await` palavra-chave não é aplicado antes `Groups.Add` porque uma mensagem não é enviada imediatamente para membros do grupo. Se você quiser enviar uma mensagem a todos os membros do grupo imediatamente depois de adicionar o novo membro, você deseja aplicar o `await` palavra-chave para certificar-se de que a operação assíncrona foi concluída.

[!code-csharp[Main](working-with-groups/samples/sample7.cs)]

<a id="storeazuretable"></a>

## <a name="storing-group-membership-in-azure-table-storage"></a>Associação de grupo de armazenamento no armazenamento de tabela do Azure

Usando o armazenamento de tabela do Azure para armazenar informações de usuário e grupo é semelhante ao uso de um banco de dados. O exemplo a seguir mostra uma entidade de tabela que armazena o nome de usuário e o nome do grupo.

[!code-csharp[Main](working-with-groups/samples/sample8.cs)]

No hub, recuperar os grupos atribuídos quando o usuário se conecta.

[!code-csharp[Main](working-with-groups/samples/sample9.cs)]

<a id="verify"></a>

## <a name="verifying-group-membership-when-reconnecting"></a>Verificar a associação de grupo ao reconectar

Por padrão, o SignalR novamente atribui automaticamente um usuário aos grupos apropriados ao reconectar-se de uma interrupção temporária, como quando uma conexão será descartada e restabelecida antes que a conexão expire. Informações de grupo do usuário são passadas em um token ao reconectar e esse token é verificado no servidor. Para obter informações sobre o processo de verificação para incluir novamente grupos de usuários, consulte [incluir novamente grupos ao reconectar](../security/introduction-to-security.md#rejoingroup).

Em geral, você deve usar o comportamento padrão de automaticamente incluir novamente grupos na reconexão. SignalR grupos não são como um mecanismo de segurança para restringir o acesso a dados confidenciais. No entanto, se seu aplicativo precisa verificar a associação de grupo do usuário ao reconectar, você pode substituir o comportamento padrão. Alterar o comportamento padrão pode adicionar uma carga de seu banco de dados porque a associação de grupo do usuário deve ser recuperada para cada reconexão em vez de apenas quando o usuário se conecta.

Se você deve verificar a associação de grupo em reconectar, crie um novo módulo de pipeline de hub que retorna uma lista de grupos atribuídos, conforme mostrado abaixo.

[!code-csharp[Main](working-with-groups/samples/sample10.cs)]

Em seguida, adicione módulo ao pipeline do hub, como destacado abaixo.

[!code-csharp[Main](working-with-groups/samples/sample11.cs?highlight=4)]

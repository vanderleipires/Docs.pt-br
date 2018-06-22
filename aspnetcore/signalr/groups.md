---
title: Gerenciar usuários e grupos no SignalR
author: rachelappel
description: Visão geral do ASP.NET Core SignalR usuário e grupo de gerenciamento.
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 06/04/2018
uid: signalr/groups
ms.openlocfilehash: f7d60a906fc238f79c76fd2a4ee693417a348825
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272075"
---
# <a name="manage-users-and-groups-in-signalr"></a>Gerenciar usuários e grupos no SignalR

Por [Brennan Conroy](https://github.com/BrennanConroy)

SignalR permite que mensagens sejam enviados para todas as conexões associadas a um usuário específico, bem como grupos de conexões nomeados.

[Exibir ou baixar o código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/groups/sample/) [(como fazer o download)](xref:tutorials/index#how-to-download-a-sample)

## <a name="users-in-signalr"></a>Usuários no SignalR

SignalR permite que você envie mensagens para todas as conexões associadas a um usuário específico. Por padrão o SignalR usa o `ClaimTypes.NameIdentifier` do `ClaimsPrincipal` associado com a conexão como o identificador do usuário. Um único usuário pode ter várias conexões a um aplicativo do SignalR. Por exemplo, um usuário pode ser conectado em sua área de trabalho, bem como seu telefone. Cada dispositivo tem uma conexão SignalR separado, mas eles estão todos os associados com o mesmo usuário. Se uma mensagem é enviada para o usuário, todas as conexões associadas ao usuário receberá a mensagem.

Enviar uma mensagem para um usuário específico, passando o identificador do usuário para o `User` função em seu método de hub, conforme mostrado no exemplo a seguir:

> [!NOTE]
> O identificador de usuário diferencia maiusculas de minúsculas.

```csharp
public Task SendPrivateMessage(string user, string message)
{
    return Clients.User(user).SendAsync("ReceiveMessage", message);
}
```

O identificador de usuário pode ser personalizado, criando um `IUserIdProvider`e registrá-lo no `ConfigureServices`.

[!code-csharp[UserIdProvider](groups/sample/customuseridprovider.cs?range=4-10)]

[!code-csharp[Configure service](groups/sample/startup.cs?range=21-22,39-42)]

> [!NOTE]
> AddSignalR deve ser chamado antes de registrar os serviços SignalR personalizados.

## <a name="groups-in-signalr"></a>Grupos no SignalR

Um grupo é uma coleção de conexões associado com um nome. As mensagens podem ser enviadas a todas as conexões em um grupo. Grupos são a maneira recomendada para enviar para uma conexão ou várias conexões, porque os grupos são gerenciados pelo aplicativo. Uma conexão pode ser um membro de vários grupos. Isso torna grupos ideal para algo como um aplicativo de bate-papo, onde cada espaço pode ser representado como um grupo. Conexões podem ser adicionados ou removidos de grupos por meio de `AddToGroupAsync` e `RemoveFromGroupAsync` métodos.

[!code-csharp[Hub methods](groups/sample/hubs/chathub.cs?range=15-27)]

Associação de grupo não é preservada quando uma conexão é reconectada. A conexão deve se associar novamente o grupo quando ela é restabelecida. Não é possível contar os membros de um grupo, porque essas informações não estão disponíveis se o aplicativo é dimensionado para vários servidores.

> [!NOTE]
> Os nomes de grupo diferenciam maiusculas de minúsculas.

## <a name="related-resources"></a>Recursos relacionados

* [Introdução](xref:tutorials/signalr)
* [Hubs](xref:signalr/hubs)
* [Publicar no Azure](xref:signalr/publish-to-azure-web-app)

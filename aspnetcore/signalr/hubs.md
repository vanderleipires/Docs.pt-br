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
ms.openlocfilehash: 4810b3a1978f05eb0608320750f31b8bb95d71fd
ms.sourcegitcommit: 74be78285ea88772e7dad112f80146b6ed00e53e
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/10/2018
---
# <a name="use-hubs-in-signalr-for-aspnet-core"></a>Usando os hubs de SignalR para ASP.NET Core

Por [Rachel Appel](https://twitter.com/rachelappel) e [Griffin Kevin](https://twitter.com/1kevgriff)

[Exibir ou baixar o código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(como fazer o download)](xref:tutorials/index#how-to-download-a-sample)

## <a name="what-is-a-signalr-hub"></a>O que é um hub SignalR

A API de Hubs de SignalR permite chamar métodos em clientes conectados do servidor. No código de servidor, você define os métodos que são chamados pelo cliente. No código do cliente, você define os métodos que são chamados do servidor. SignalR cuida de tudo nos bastidores que possibilita a comunicação de cliente para servidor e servidor-para-cliente em tempo real.

## <a name="configure-signalr-hubs"></a>Configurar os hubs de SignalR

O middleware SignalR requer alguns serviços, que são configurados por meio da chamada `services.AddSignalR`.

[!code-csharp[Configure service](hubs/sample/startup.cs?range=38)]

Ao adicionar a funcionalidade de SignalR para um aplicativo ASP.NET Core, configurar rotas SignalR chamando `app.UseSignalR` no `Startup.Configure` método.

[!code-csharp[Configure routes to hubs](hubs/sample/startup.cs?range=56-59)]

## <a name="create-and-use-hubs"></a>Criar e usar hubs

Criar um hub declarando uma classe que herda de `Hub`e adicionar métodos públicos a ele. Os clientes podem chamar os métodos que são definidos como `public`.

[!code-csharp[Create and use hubs](hubs/sample/hubs/chathub.cs?range=8-37)]

Você pode especificar um tipo de retorno e parâmetros, incluindo tipos complexos e matrizes, como você faria em qualquer método em c#. SignalR lida com a serialização e desserialização de objetos complexos e matrizes em seus parâmetros e valores de retorno.

## <a name="the-clients-object"></a>O objeto de clientes

Cada instância do `Hub` classe tem uma propriedade denominada `Clients` que contém os seguintes membros para a comunicação entre cliente e servidor:

| Propriedade | Descrição |
| ------ | ----------- |
| `All` | Chama um método em todos os clientes conectados |
| `Caller` | Chama um método no cliente que invocou o método de hub |
| `Others` | Chama um método em todos os clientes conectados, exceto o cliente que invocou o método |

Além disso, a `Hub` classe contém os seguintes métodos:

| Método | Descrição |
| ------ | ----------- |
| `AllExcept` | Chama um método em todos os clientes conectados, exceto para as conexões especificadas |
| `Client` | Chama um método em um cliente conectado específico |
| `Clients` | Chama um método específicos clientes conectados |
| `Group` | Envia uma mensagem para todas as conexões no grupo especificado  |
| `GroupExcept` | Envia uma mensagem para todas as conexões no grupo especificado, exceto as conexões especificadas |
| `Groups` | Envia uma mensagem para vários grupos de conexões  |
| `OthersInGroup` | Envia uma mensagem para um grupo de conexões, excluindo o cliente que invocou o método de hub  |
| `User` | Envia uma mensagem para todas as conexões associadas a um usuário específico |
| `Users` | Envia uma mensagem para todas as conexões associadas com os usuários especificados |

Cada propriedade ou método nas tabelas anteriores retorna um objeto com um `SendAsync` método. O `SendAsync` método permite que você forneça o nome e os parâmetros do método de cliente para chamar.

## <a name="send-messages-to-clients"></a>Enviar mensagens para os clientes

Para fazer chamadas para clientes específicos, use as propriedades do `Clients` objeto. No exemplo a seguir, o `SendMessageToCaller` método demonstra enviando uma mensagem para a conexão que invocou o método de hub. O `SendMessageToGroups` método envia uma mensagem para os grupos armazenados em um `List` chamado `groups`.

[!code-csharp[Send messages](hubs/sample/hubs/chathub.cs?range=15-24)]

## <a name="handle-events-for-a-connection"></a>Manipular eventos para uma conexão

A API de Hubs de SignalR fornece o `OnConnectedAsync` e `OnDisconnectedAsync` métodos virtuais para gerenciar e controlar conexões. Substituir o `OnConnectedAsync` método virtual para executar ações quando um cliente se conecta ao Hub, como adicioná-lo a um grupo.

[!code-csharp[Handle events](hubs/sample/hubs/chathub.cs?range=26-36)]

## <a name="handle-errors"></a>Tratar erros

Exceções geradas em seus métodos de hub são enviadas ao cliente que invocou o método. No cliente JavaScript, o `invoke` método retorna um [promessa JavaScript](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises). Quando o cliente recebe um erro com um manipulador anexado à promessa usando `catch`, ele foi chamado e passado como um JavaScript `Error` objeto.

[!code-javascript[Error](hubs/sample/wwwroot/js/chat.js?range=23)]

## <a name="related-resources"></a>Recursos relacionados

[Introdução ao ASP.NET Core SignalR](xref:signalr/introduction)

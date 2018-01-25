---
uid: signalr/overview/older-versions/persistent-connection-authorization
title: "Autenticação e autorização para conexões persistentes SignalR (SignalR 1. x) | Microsoft Docs"
author: pfletcher
description: "Este tópico descreve como implantar a autorização em uma conexão persistente. Para obter informações gerais sobre como integrar a segurança em um aplicativo de SignalR,..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/21/2013
ms.topic: article
ms.assetid: c34bc627-41af-4c21-a817-e97a19a7f252
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/persistent-connection-authorization
msc.type: authoredcontent
ms.openlocfilehash: 2e97dfd03c61b110325c41a992b4af490fcd17de
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="authentication-and-authorization-for-signalr-persistent-connections-signalr-1x"></a>Autenticação e autorização para conexões persistentes SignalR (SignalR 1. x)
====================
por [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)

> Este tópico descreve como implantar a autorização em uma conexão persistente. Para obter informações gerais sobre como integrar a segurança em um aplicativo SignalR, consulte [Introdução à segurança](index.md).


## <a name="enforce-authorization"></a>Implantar a autorização

Para impor regras de autorização ao usar um [PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) você deve substituir o `AuthorizeRequest` método. Não é possível usar o `Authorize` atributo com conexões persistentes. O `AuthorizeRequest` método é chamado pelo SignalR Framework antes de cada solicitação para verificar se o usuário está autorizado a executar a ação solicitada. O `AuthorizeRequest` método não é chamado do cliente; em vez disso, autenticar o usuário por meio do mecanismo de autenticação padrão do seu aplicativo.

O exemplo a seguir mostra como limitar solicitações para usuários autenticados.

[!code-csharp[Main](persistent-connection-authorization/samples/sample1.cs)]

Você pode adicionar qualquer lógica de autorização personalizado no método AuthorizeRequest; como verificar se um usuário pertencer a uma função específica.

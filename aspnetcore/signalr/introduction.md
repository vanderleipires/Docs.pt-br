---
title: Introdução ao SignalR do ASP.NET Core
author: tdykstra
description: Saiba como a biblioteca SignalR do ASP.NET Core simplifica a adição de funcionalidade em tempo real aos aplicativos.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 04/25/2018
uid: signalr/introduction
ms.openlocfilehash: bc6f25c3f35e7fb0c2c68220697f2e0fdc6a9958
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095383"
---
# <a name="introduction-to-aspnet-core-signalr"></a>Introdução ao SignalR do ASP.NET Core

Por [Rachel Appel](https://twitter.com/rachelappel)

## <a name="what-is-signalr"></a>O que é o SignalR?

SignalR do ASP.NET Core é uma biblioteca que simplifica a adição da funcionalidade da web em tempo real aos aplicativos. A funcionalidade da web em tempo real permite que o código do lado do servidor para conteúdo de envio por push aos clientes instantaneamente.

Bons candidatos para o SignalR:

* Aplicativos que exigem atualizações de alta frequência do servidor. Exemplos são jogos, redes sociais, de votação, leilão, mapas e aplicativos GPS.
* Os painéis e monitoramento de aplicativos. Exemplos incluem painéis da empresa, atualizações de vendas instantâneas, ou alertas de viagem.
* Aplicativos de colaboração. Quadro de comunicações aplicativos e software de reunião de equipe são exemplos de aplicativos de colaboração.
* Aplicativos que exigem as notificações. Redes sociais, email, bate-papo, jogos, alertas de viagem e muitos outros aplicativos usam notificações.

O SignalR fornece uma API para a criação de servidor para cliente [chamadas de procedimento remoto (RPC)](https://wikipedia.org/wiki/Remote_procedure_call). As RPCs chamam funções JavaScript em clientes do código do lado do servidor do .NET Core.

SignalR para ASP.NET Core:

* Lida com gerenciamento de conexão automaticamente.
* Permite a transmissão de mensagens para todos os clientes conectados simultaneamente. Por exemplo, uma sala de bate-papo.
* Permite o envio de mensagens para clientes específicos ou grupos de clientes.
* É de software livre no [GitHub](https://github.com/aspnet/signalr).
* Escalonável.

A conexão entre o cliente e servidor é persistente, ao contrário de uma conexão HTTP.

## <a name="transports"></a>Transportes

Resumos de SignalR ao longo de várias técnicas para criar aplicativos web em tempo real. [WebSockets](https://tools.ietf.org/html/rfc7118) é o transporte ideal, mas outras técnicas, como sondagem longa e de eventos do Server-Sent podem ser usadas quando aqueles não estão disponíveis. O SignalR detectará automaticamente e inicializar o transporte apropriado com base em recursos com suporte no servidor e cliente.

## <a name="hubs"></a>Hubs

O SignalR usa os hubs para a comunicação entre clientes e servidores.

Um hub é um pipeline de alto nível que permite que seu cliente e servidor chamar métodos em si. O SignalR manipula a expedição entre limites de máquina automaticamente, permitindo que os clientes chamar métodos no servidor como facilmente como métodos locais e vice-versa. Hubs permitem a passagem de parâmetros fortemente tipados para os métodos, que habilita a associação de modelo. O SignalR fornece dois protocolos de hub interno: um protocolo de texto com base em JSON e um protocolo binário, com base em [MessagePack](https://msgpack.org/).  MessagePack geralmente cria mensagens menores que ao usar o JSON. Devem dar suporte a navegadores mais antigos [nível XHR 2](https://caniuse.com/#feat=xhr2) para fornecer suporte de protocolo MessagePack.

Os hubs de chamar o código do lado do cliente enviando mensagens usando o transporte ativo. As mensagens contêm o nome e parâmetros do método do lado do cliente. Os objetos enviados como parâmetros de método são desserializados usando o protocolo configurado. O cliente tenta corresponder o nome a um método no código do lado do cliente. Quando ocorrer uma correspondência, o método do cliente é executado usando os dados de parâmetro desserializado.

## <a name="additional-resources"></a>Recursos adicionais

* [Introdução ao SignalR para ASP.NET Core](xref:tutorials/signalr)
* [Plataformas com suporte](xref:signalr/supported-platforms)
* [Hubs](xref:signalr/hubs)
* [Cliente JavaScript](xref:signalr/javascript-client)

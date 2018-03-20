---
title: "Introdução ao SignalR no ASP.NET Core"
author: rachelappel
description: "Saiba como a biblioteca ASP.NET Core SignalR simplifica a adição de funcionalidade da web em tempo real para aplicativos."
manager: wpickett
ms.author: rachelap
ms.custom: mvc
ms.date: 03/07/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/introduction-signalr-core
ms.openlocfilehash: 3fa70c957b246787d4e457c74f90ad797b3af766
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/15/2018
---
# <a name="introduction-to-signalr"></a>Introdução ao SignalR

Por [Rachel Appel](https://twitter.com/rachelappel)

## <a name="what-is-signalr"></a>O que é o SignalR?

O SignalR do ASP.NET Core é uma biblioteca que simplifica a funcionalidade Adicionar web em tempo real para aplicativos. A funcionalidade da web em tempo real permite que o código do lado do servidor de conteúdo por push aos clientes imediatamente.

Bons candidatos para o SignalR:

* Aplicativos que exigem atualizações de alta frequência do servidor. Os exemplos são jogos, redes sociais, votação, leilão, mapas e aplicativos GPS.
* Painéis de controle e monitoramento de aplicativos. Exemplos incluem painéis da empresa, atualizações de vendas instantâneas, ou alertas de viagem.
* Aplicativos de colaboração. Quadro de comunicações aplicativos e software de reunião de equipe são exemplos de aplicativos de colaboração.
* Aplicativos que exigem as notificações. Redes sociais, email, bate-papo, jogos, alertas de viagem e muitos outros aplicativos usam notificações.

O SignalR fornece uma API para a criação de servidor-para-cliente [chamadas de procedimento remoto (RPC)](https://wikipedia.org/wiki/Remote_procedure_call). Os RPCs chamam funções JavaScript em clientes do código-fonte do .NET do servidor.

SignalR para ASP.NET Core:

* Manipula o gerenciamento de conexão automaticamente.
* Permite a transmissão de mensagens para todos os clientes conectados simultaneamente. Por exemplo, uma sala de bate-papo.
* Permite o envio de mensagens para clientes específicos ou grupos de clientes.
* É o código aberto em [GitHub](https://github.com/aspnet/signalr).
* Escalonável.

A conexão entre o cliente e servidor é persistente, ao contrário de uma conexão HTTP.

## <a name="transports"></a>Transportes

SignalR resumos sobre várias técnicas para criar aplicativos web em tempo real. [O WebSocket](https://tools.ietf.org/html/rfc7118) é o transporte ideal, mas outras técnicas como eventos Server-Sent e sondagem longa podem ser usadas quando os não estão disponíveis. SignalR detectará automaticamente e inicializar o transporte apropriado com base em recursos com suporte no cliente e servidor.

## <a name="hubs-and-endpoints"></a>Hubs e pontos de extremidade

O SignalR usa Hubs e pontos de extremidade para comunicação entre clientes e servidores. A API de Hubs abrange a maioria dos cenários.

Um hub é um pipeline de alto nível construído com a API de ponto de extremidade que permite que o cliente e o servidor chamar métodos em si. SignalR lida com a distribuição entre limites de máquina automaticamente, permitindo que os clientes chamar os métodos no servidor como facilmente como métodos locais e vice-versa. Hubs permitem passar parâmetros fortemente tipadas para métodos, que permite que a associação de modelo. O SignalR fornece dois protocolos de hub interno: um protocolo de texto com base em JSON e um protocolo binário com base em [MessagePack](https://msgpack.org/).  MessagePack geralmente cria mensagens menores do que o uso JSON. Devem oferecer suporte a navegadores mais antigos [nível XHR 2](https://caniuse.com/#feat=xhr2) para oferecer suporte a protocolo MessagePack.

Hubs de chamar o código de cliente, enviando mensagens usando o transporte ativo. As mensagens contêm o nome e os parâmetros do método do lado do cliente. Objetos enviados como parâmetros de método desserializados usando o protocolo configurado. O cliente tenta corresponder o nome a um método no código do lado do cliente. Quando ocorrer uma correspondência, o método do cliente é executado usando os dados de parâmetro desserializado.

Pontos de extremidade fornecem uma API como soquete bruta, habilitá-las para leitura e gravação do cliente. Cabe ao desenvolvedor de lidar com o agrupamento, transmissão e outras funções. A API de Hubs é criada sobre a camada de pontos de extremidade.

O diagrama a seguir mostra a relação entre hubs, pontos de extremidade e clientes.

![Mapa de SignalR](introduction-signalr-core/_static/signalr-core-architecture.png)

## <a name="related-resources"></a>Recursos relacionados

[Introdução ao SignalR para ASP.NET Core](xref:signalr/get-started-signalr-core)

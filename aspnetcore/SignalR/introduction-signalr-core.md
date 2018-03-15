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
# <a name="introduction-to-signalr"></a><span data-ttu-id="48103-103">Introdução ao SignalR</span><span class="sxs-lookup"><span data-stu-id="48103-103">Introduction to SignalR</span></span>

<span data-ttu-id="48103-104">Por [Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="48103-104">By [Rachel Appel](https://twitter.com/rachelappel)</span></span>

## <a name="what-is-signalr"></a><span data-ttu-id="48103-105">O que é o SignalR?</span><span class="sxs-lookup"><span data-stu-id="48103-105">What is SignalR?</span></span>

<span data-ttu-id="48103-106">O SignalR do ASP.NET Core é uma biblioteca que simplifica a funcionalidade Adicionar web em tempo real para aplicativos.</span><span class="sxs-lookup"><span data-stu-id="48103-106">ASP.NET Core SignalR is a library that simplifies adding real-time web functionality to apps.</span></span> <span data-ttu-id="48103-107">A funcionalidade da web em tempo real permite que o código do lado do servidor de conteúdo por push aos clientes imediatamente.</span><span class="sxs-lookup"><span data-stu-id="48103-107">Real-time web functionality enables server-side code to push content to clients instantly.</span></span>

<span data-ttu-id="48103-108">Bons candidatos para o SignalR:</span><span class="sxs-lookup"><span data-stu-id="48103-108">Good candidates for SignalR:</span></span>

* <span data-ttu-id="48103-109">Aplicativos que exigem atualizações de alta frequência do servidor.</span><span class="sxs-lookup"><span data-stu-id="48103-109">Apps that require high frequency updates from the server.</span></span> <span data-ttu-id="48103-110">Os exemplos são jogos, redes sociais, votação, leilão, mapas e aplicativos GPS.</span><span class="sxs-lookup"><span data-stu-id="48103-110">Examples are gaming, social networks, voting, auction, maps, and GPS apps.</span></span>
* <span data-ttu-id="48103-111">Painéis de controle e monitoramento de aplicativos.</span><span class="sxs-lookup"><span data-stu-id="48103-111">Dashboards and monitoring apps.</span></span> <span data-ttu-id="48103-112">Exemplos incluem painéis da empresa, atualizações de vendas instantâneas, ou alertas de viagem.</span><span class="sxs-lookup"><span data-stu-id="48103-112">Examples include company dashboards, instant sales updates, or travel alerts.</span></span>
* <span data-ttu-id="48103-113">Aplicativos de colaboração.</span><span class="sxs-lookup"><span data-stu-id="48103-113">Collaborative apps.</span></span> <span data-ttu-id="48103-114">Quadro de comunicações aplicativos e software de reunião de equipe são exemplos de aplicativos de colaboração.</span><span class="sxs-lookup"><span data-stu-id="48103-114">Whiteboard apps and team meeting software are examples of collaborative apps.</span></span>
* <span data-ttu-id="48103-115">Aplicativos que exigem as notificações.</span><span class="sxs-lookup"><span data-stu-id="48103-115">Apps that require notifications.</span></span> <span data-ttu-id="48103-116">Redes sociais, email, bate-papo, jogos, alertas de viagem e muitos outros aplicativos usam notificações.</span><span class="sxs-lookup"><span data-stu-id="48103-116">Social networks, email, chat, games, travel alerts, and many other apps use notifications.</span></span>

<span data-ttu-id="48103-117">O SignalR fornece uma API para a criação de servidor-para-cliente [chamadas de procedimento remoto (RPC)](https://wikipedia.org/wiki/Remote_procedure_call).</span><span class="sxs-lookup"><span data-stu-id="48103-117">SignalR provides an API for creating server-to-client [remote procedure calls (RPC)](https://wikipedia.org/wiki/Remote_procedure_call).</span></span> <span data-ttu-id="48103-118">Os RPCs chamam funções JavaScript em clientes do código-fonte do .NET do servidor.</span><span class="sxs-lookup"><span data-stu-id="48103-118">The RPCs call JavaScript functions on clients from server-side .NET Core code.</span></span>

<span data-ttu-id="48103-119">SignalR para ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="48103-119">SignalR for ASP.NET Core:</span></span>

* <span data-ttu-id="48103-120">Manipula o gerenciamento de conexão automaticamente.</span><span class="sxs-lookup"><span data-stu-id="48103-120">Handles connection management automatically.</span></span>
* <span data-ttu-id="48103-121">Permite a transmissão de mensagens para todos os clientes conectados simultaneamente.</span><span class="sxs-lookup"><span data-stu-id="48103-121">Enables broadcasting messages to all connected clients simultaneously.</span></span> <span data-ttu-id="48103-122">Por exemplo, uma sala de bate-papo.</span><span class="sxs-lookup"><span data-stu-id="48103-122">For example, a chat room.</span></span>
* <span data-ttu-id="48103-123">Permite o envio de mensagens para clientes específicos ou grupos de clientes.</span><span class="sxs-lookup"><span data-stu-id="48103-123">Enables sending messages to specific clients or groups of clients.</span></span>
* <span data-ttu-id="48103-124">É o código aberto em [GitHub](https://github.com/aspnet/signalr).</span><span class="sxs-lookup"><span data-stu-id="48103-124">Is open-sourced at [GitHub](https://github.com/aspnet/signalr).</span></span>
* <span data-ttu-id="48103-125">Escalonável.</span><span class="sxs-lookup"><span data-stu-id="48103-125">Scalable.</span></span>

<span data-ttu-id="48103-126">A conexão entre o cliente e servidor é persistente, ao contrário de uma conexão HTTP.</span><span class="sxs-lookup"><span data-stu-id="48103-126">The connection between the client and server is persistent, unlike an HTTP connection.</span></span>

## <a name="transports"></a><span data-ttu-id="48103-127">Transportes</span><span class="sxs-lookup"><span data-stu-id="48103-127">Transports</span></span>

<span data-ttu-id="48103-128">SignalR resumos sobre várias técnicas para criar aplicativos web em tempo real.</span><span class="sxs-lookup"><span data-stu-id="48103-128">SignalR abstracts over a number of techniques for building real-time web applications.</span></span> <span data-ttu-id="48103-129">[O WebSocket](https://tools.ietf.org/html/rfc7118) é o transporte ideal, mas outras técnicas como eventos Server-Sent e sondagem longa podem ser usadas quando os não estão disponíveis.</span><span class="sxs-lookup"><span data-stu-id="48103-129">[WebSockets](https://tools.ietf.org/html/rfc7118) is the optimal transport, but other techniques like Server-Sent Events and Long Polling can be used when those aren't available.</span></span> <span data-ttu-id="48103-130">SignalR detectará automaticamente e inicializar o transporte apropriado com base em recursos com suporte no cliente e servidor.</span><span class="sxs-lookup"><span data-stu-id="48103-130">SignalR will automatically detect and initialize the appropriate transport based on features supported on the server and client.</span></span>

## <a name="hubs-and-endpoints"></a><span data-ttu-id="48103-131">Hubs e pontos de extremidade</span><span class="sxs-lookup"><span data-stu-id="48103-131">Hubs and Endpoints</span></span>

<span data-ttu-id="48103-132">O SignalR usa Hubs e pontos de extremidade para comunicação entre clientes e servidores.</span><span class="sxs-lookup"><span data-stu-id="48103-132">SignalR uses Hubs and Endpoints to communicate between clients and servers.</span></span> <span data-ttu-id="48103-133">A API de Hubs abrange a maioria dos cenários.</span><span class="sxs-lookup"><span data-stu-id="48103-133">The Hubs API covers the most scenarios.</span></span>

<span data-ttu-id="48103-134">Um hub é um pipeline de alto nível construído com a API de ponto de extremidade que permite que o cliente e o servidor chamar métodos em si.</span><span class="sxs-lookup"><span data-stu-id="48103-134">A hub is a high-level pipeline built upon the Endpoint API that allows your client and server to call methods on each other.</span></span> <span data-ttu-id="48103-135">SignalR lida com a distribuição entre limites de máquina automaticamente, permitindo que os clientes chamar os métodos no servidor como facilmente como métodos locais e vice-versa.</span><span class="sxs-lookup"><span data-stu-id="48103-135">SignalR handles the dispatching across machine boundaries automatically, allowing clients to call methods on the server as easily as local methods, and vice versa.</span></span> <span data-ttu-id="48103-136">Hubs permitem passar parâmetros fortemente tipadas para métodos, que permite que a associação de modelo.</span><span class="sxs-lookup"><span data-stu-id="48103-136">Hubs allow passing strongly-typed parameters to methods, which enables model binding.</span></span> <span data-ttu-id="48103-137">O SignalR fornece dois protocolos de hub interno: um protocolo de texto com base em JSON e um protocolo binário com base em [MessagePack](https://msgpack.org/).</span><span class="sxs-lookup"><span data-stu-id="48103-137">SignalR provides two built-in hub protocols: a text protocol based on JSON and a binary protocol based on [MessagePack](https://msgpack.org/).</span></span>  <span data-ttu-id="48103-138">MessagePack geralmente cria mensagens menores do que o uso JSON.</span><span class="sxs-lookup"><span data-stu-id="48103-138">MessagePack generally creates smaller messages than when using JSON.</span></span> <span data-ttu-id="48103-139">Devem oferecer suporte a navegadores mais antigos [nível XHR 2](https://caniuse.com/#feat=xhr2) para oferecer suporte a protocolo MessagePack.</span><span class="sxs-lookup"><span data-stu-id="48103-139">Older browsers must support [XHR level 2](https://caniuse.com/#feat=xhr2) to provide MessagePack protocol support.</span></span>

<span data-ttu-id="48103-140">Hubs de chamar o código de cliente, enviando mensagens usando o transporte ativo.</span><span class="sxs-lookup"><span data-stu-id="48103-140">Hubs call client-side code by sending messages using the active transport.</span></span> <span data-ttu-id="48103-141">As mensagens contêm o nome e os parâmetros do método do lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="48103-141">The messages contain the name and parameters of the client-side method.</span></span> <span data-ttu-id="48103-142">Objetos enviados como parâmetros de método desserializados usando o protocolo configurado.</span><span class="sxs-lookup"><span data-stu-id="48103-142">Objects sent as method parameters are deserialized using the configured protocol.</span></span> <span data-ttu-id="48103-143">O cliente tenta corresponder o nome a um método no código do lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="48103-143">The client tries to match the name to a method in the client-side code.</span></span> <span data-ttu-id="48103-144">Quando ocorrer uma correspondência, o método do cliente é executado usando os dados de parâmetro desserializado.</span><span class="sxs-lookup"><span data-stu-id="48103-144">When a match happens, the client method runs using the deserialized parameter data.</span></span>

<span data-ttu-id="48103-145">Pontos de extremidade fornecem uma API como soquete bruta, habilitá-las para leitura e gravação do cliente.</span><span class="sxs-lookup"><span data-stu-id="48103-145">Endpoints provide a raw socket-like API, enabling them to read and write from the client.</span></span> <span data-ttu-id="48103-146">Cabe ao desenvolvedor de lidar com o agrupamento, transmissão e outras funções.</span><span class="sxs-lookup"><span data-stu-id="48103-146">It's up to the developer to handle grouping, broadcasting, and other functions.</span></span> <span data-ttu-id="48103-147">A API de Hubs é criada sobre a camada de pontos de extremidade.</span><span class="sxs-lookup"><span data-stu-id="48103-147">The Hubs API is built on top of the Endpoints layer.</span></span>

<span data-ttu-id="48103-148">O diagrama a seguir mostra a relação entre hubs, pontos de extremidade e clientes.</span><span class="sxs-lookup"><span data-stu-id="48103-148">The following diagram shows the relationship between hubs, endpoints, and clients.</span></span>

![Mapa de SignalR](introduction-signalr-core/_static/signalr-core-architecture.png)

## <a name="related-resources"></a><span data-ttu-id="48103-150">Recursos relacionados</span><span class="sxs-lookup"><span data-stu-id="48103-150">Related resources</span></span>

[<span data-ttu-id="48103-151">Introdução ao SignalR para ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="48103-151">Get started with SignalR for ASP.NET Core</span></span>](xref:signalr/get-started-signalr-core)

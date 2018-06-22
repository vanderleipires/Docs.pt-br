---
title: Introdução ao ASP.NET Core SignalR
author: rachelappel
description: Saiba como a biblioteca ASP.NET Core SignalR simplifica a adicionar funcionalidade em tempo real aos aplicativos.
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 04/25/2018
uid: signalr/introduction
ms.openlocfilehash: 0c833acea139d22883a69a02c2357a71f3ac8db8
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277898"
---
# <a name="introduction-to-aspnet-core-signalr"></a><span data-ttu-id="aa5a2-103">Introdução ao ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="aa5a2-103">Introduction to ASP.NET Core SignalR</span></span>

<span data-ttu-id="aa5a2-104">Por [Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="aa5a2-104">By [Rachel Appel](https://twitter.com/rachelappel)</span></span>

## <a name="what-is-signalr"></a><span data-ttu-id="aa5a2-105">O que é o SignalR?</span><span class="sxs-lookup"><span data-stu-id="aa5a2-105">What is SignalR?</span></span>

<span data-ttu-id="aa5a2-106">O SignalR do ASP.NET Core é uma biblioteca que simplifica a funcionalidade Adicionar web em tempo real para aplicativos.</span><span class="sxs-lookup"><span data-stu-id="aa5a2-106">ASP.NET Core SignalR is a library that simplifies adding real-time web functionality to apps.</span></span> <span data-ttu-id="aa5a2-107">A funcionalidade da web em tempo real permite que o código do lado do servidor de conteúdo por push aos clientes imediatamente.</span><span class="sxs-lookup"><span data-stu-id="aa5a2-107">Real-time web functionality enables server-side code to push content to clients instantly.</span></span>

<span data-ttu-id="aa5a2-108">Bons candidatos para o SignalR:</span><span class="sxs-lookup"><span data-stu-id="aa5a2-108">Good candidates for SignalR:</span></span>

* <span data-ttu-id="aa5a2-109">Aplicativos que exigem atualizações de alta frequência do servidor.</span><span class="sxs-lookup"><span data-stu-id="aa5a2-109">Apps that require high frequency updates from the server.</span></span> <span data-ttu-id="aa5a2-110">Os exemplos são jogos, redes sociais, votação, leilão, mapas e aplicativos GPS.</span><span class="sxs-lookup"><span data-stu-id="aa5a2-110">Examples are gaming, social networks, voting, auction, maps, and GPS apps.</span></span>
* <span data-ttu-id="aa5a2-111">Painéis de controle e monitoramento de aplicativos.</span><span class="sxs-lookup"><span data-stu-id="aa5a2-111">Dashboards and monitoring apps.</span></span> <span data-ttu-id="aa5a2-112">Exemplos incluem painéis da empresa, atualizações de vendas instantâneas, ou alertas de viagem.</span><span class="sxs-lookup"><span data-stu-id="aa5a2-112">Examples include company dashboards, instant sales updates, or travel alerts.</span></span>
* <span data-ttu-id="aa5a2-113">Aplicativos de colaboração.</span><span class="sxs-lookup"><span data-stu-id="aa5a2-113">Collaborative apps.</span></span> <span data-ttu-id="aa5a2-114">Quadro de comunicações aplicativos e software de reunião de equipe são exemplos de aplicativos de colaboração.</span><span class="sxs-lookup"><span data-stu-id="aa5a2-114">Whiteboard apps and team meeting software are examples of collaborative apps.</span></span>
* <span data-ttu-id="aa5a2-115">Aplicativos que exigem as notificações.</span><span class="sxs-lookup"><span data-stu-id="aa5a2-115">Apps that require notifications.</span></span> <span data-ttu-id="aa5a2-116">Redes sociais, email, bate-papo, jogos, alertas de viagem e muitos outros aplicativos usam notificações.</span><span class="sxs-lookup"><span data-stu-id="aa5a2-116">Social networks, email, chat, games, travel alerts, and many other apps use notifications.</span></span>

<span data-ttu-id="aa5a2-117">O SignalR fornece uma API para a criação de servidor-para-cliente [chamadas de procedimento remoto (RPC)](https://wikipedia.org/wiki/Remote_procedure_call).</span><span class="sxs-lookup"><span data-stu-id="aa5a2-117">SignalR provides an API for creating server-to-client [remote procedure calls (RPC)](https://wikipedia.org/wiki/Remote_procedure_call).</span></span> <span data-ttu-id="aa5a2-118">Os RPCs chamam funções JavaScript em clientes do código-fonte do .NET do servidor.</span><span class="sxs-lookup"><span data-stu-id="aa5a2-118">The RPCs call JavaScript functions on clients from server-side .NET Core code.</span></span>

<span data-ttu-id="aa5a2-119">SignalR para ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="aa5a2-119">SignalR for ASP.NET Core:</span></span>

* <span data-ttu-id="aa5a2-120">Manipula o gerenciamento de conexão automaticamente.</span><span class="sxs-lookup"><span data-stu-id="aa5a2-120">Handles connection management automatically.</span></span>
* <span data-ttu-id="aa5a2-121">Permite a transmissão de mensagens para todos os clientes conectados simultaneamente.</span><span class="sxs-lookup"><span data-stu-id="aa5a2-121">Enables broadcasting messages to all connected clients simultaneously.</span></span> <span data-ttu-id="aa5a2-122">Por exemplo, uma sala de bate-papo.</span><span class="sxs-lookup"><span data-stu-id="aa5a2-122">For example, a chat room.</span></span>
* <span data-ttu-id="aa5a2-123">Permite o envio de mensagens para clientes específicos ou grupos de clientes.</span><span class="sxs-lookup"><span data-stu-id="aa5a2-123">Enables sending messages to specific clients or groups of clients.</span></span>
* <span data-ttu-id="aa5a2-124">É o código aberto em [GitHub](https://github.com/aspnet/signalr).</span><span class="sxs-lookup"><span data-stu-id="aa5a2-124">Is open-sourced at [GitHub](https://github.com/aspnet/signalr).</span></span>
* <span data-ttu-id="aa5a2-125">Escalonável.</span><span class="sxs-lookup"><span data-stu-id="aa5a2-125">Scalable.</span></span>

<span data-ttu-id="aa5a2-126">A conexão entre o cliente e servidor é persistente, ao contrário de uma conexão HTTP.</span><span class="sxs-lookup"><span data-stu-id="aa5a2-126">The connection between the client and server is persistent, unlike an HTTP connection.</span></span>

## <a name="transports"></a><span data-ttu-id="aa5a2-127">Transportes</span><span class="sxs-lookup"><span data-stu-id="aa5a2-127">Transports</span></span>

<span data-ttu-id="aa5a2-128">SignalR resumos sobre várias técnicas para criar aplicativos web em tempo real.</span><span class="sxs-lookup"><span data-stu-id="aa5a2-128">SignalR abstracts over a number of techniques for building real-time web applications.</span></span> <span data-ttu-id="aa5a2-129">[O WebSocket](https://tools.ietf.org/html/rfc7118) é o transporte ideal, mas outras técnicas como eventos Server-Sent e sondagem longa podem ser usadas quando os não estão disponíveis.</span><span class="sxs-lookup"><span data-stu-id="aa5a2-129">[WebSockets](https://tools.ietf.org/html/rfc7118) is the optimal transport, but other techniques like Server-Sent Events and Long Polling can be used when those aren't available.</span></span> <span data-ttu-id="aa5a2-130">SignalR detectará automaticamente e inicializar o transporte apropriado com base em recursos com suporte no cliente e servidor.</span><span class="sxs-lookup"><span data-stu-id="aa5a2-130">SignalR will automatically detect and initialize the appropriate transport based on features supported on the server and client.</span></span>

## <a name="hubs"></a><span data-ttu-id="aa5a2-131">Hubs</span><span class="sxs-lookup"><span data-stu-id="aa5a2-131">Hubs</span></span>

<span data-ttu-id="aa5a2-132">O SignalR usa hubs para comunicação entre clientes e servidores.</span><span class="sxs-lookup"><span data-stu-id="aa5a2-132">SignalR uses hubs to communicate between clients and servers.</span></span>

<span data-ttu-id="aa5a2-133">Um hub é um pipeline de alto nível que permite que o cliente e o servidor chamar métodos em si.</span><span class="sxs-lookup"><span data-stu-id="aa5a2-133">A hub is a high-level pipeline that allows your client and server to call methods on each other.</span></span> <span data-ttu-id="aa5a2-134">SignalR lida com a distribuição entre limites de máquina automaticamente, permitindo que os clientes chamar os métodos no servidor como facilmente como métodos locais e vice-versa.</span><span class="sxs-lookup"><span data-stu-id="aa5a2-134">SignalR handles the dispatching across machine boundaries automatically, allowing clients to call methods on the server as easily as local methods, and vice versa.</span></span> <span data-ttu-id="aa5a2-135">Hubs permitem passar parâmetros fortemente tipadas para métodos, que permite que a associação de modelo.</span><span class="sxs-lookup"><span data-stu-id="aa5a2-135">Hubs allow passing strongly-typed parameters to methods, which enables model binding.</span></span> <span data-ttu-id="aa5a2-136">O SignalR fornece dois protocolos de hub interno: um protocolo de texto com base em JSON e um protocolo binário com base em [MessagePack](https://msgpack.org/).</span><span class="sxs-lookup"><span data-stu-id="aa5a2-136">SignalR provides two built-in hub protocols: a text protocol based on JSON and a binary protocol based on [MessagePack](https://msgpack.org/).</span></span>  <span data-ttu-id="aa5a2-137">MessagePack geralmente cria mensagens menores do que o uso JSON.</span><span class="sxs-lookup"><span data-stu-id="aa5a2-137">MessagePack generally creates smaller messages than when using JSON.</span></span> <span data-ttu-id="aa5a2-138">Devem oferecer suporte a navegadores mais antigos [nível XHR 2](https://caniuse.com/#feat=xhr2) para oferecer suporte a protocolo MessagePack.</span><span class="sxs-lookup"><span data-stu-id="aa5a2-138">Older browsers must support [XHR level 2](https://caniuse.com/#feat=xhr2) to provide MessagePack protocol support.</span></span>

<span data-ttu-id="aa5a2-139">Hubs de chamar o código de cliente, enviando mensagens usando o transporte ativo.</span><span class="sxs-lookup"><span data-stu-id="aa5a2-139">Hubs call client-side code by sending messages using the active transport.</span></span> <span data-ttu-id="aa5a2-140">As mensagens contêm o nome e os parâmetros do método do lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="aa5a2-140">The messages contain the name and parameters of the client-side method.</span></span> <span data-ttu-id="aa5a2-141">Objetos enviados como parâmetros de método desserializados usando o protocolo configurado.</span><span class="sxs-lookup"><span data-stu-id="aa5a2-141">Objects sent as method parameters are deserialized using the configured protocol.</span></span> <span data-ttu-id="aa5a2-142">O cliente tenta corresponder o nome a um método no código do lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="aa5a2-142">The client tries to match the name to a method in the client-side code.</span></span> <span data-ttu-id="aa5a2-143">Quando ocorrer uma correspondência, o método do cliente é executado usando os dados de parâmetro desserializado.</span><span class="sxs-lookup"><span data-stu-id="aa5a2-143">When a match happens, the client method runs using the deserialized parameter data.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="aa5a2-144">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="aa5a2-144">Additional resources</span></span>

* [<span data-ttu-id="aa5a2-145">Introdução ao SignalR para ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="aa5a2-145">Get started with SignalR for ASP.NET Core</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="aa5a2-146">Plataformas com suporte</span><span class="sxs-lookup"><span data-stu-id="aa5a2-146">Supported Platforms</span></span>](xref:signalr/supported-platforms)
* [<span data-ttu-id="aa5a2-147">Hubs</span><span class="sxs-lookup"><span data-stu-id="aa5a2-147">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="aa5a2-148">Cliente JavaScript</span><span class="sxs-lookup"><span data-stu-id="aa5a2-148">JavaScript client</span></span>](xref:signalr/javascript-client)

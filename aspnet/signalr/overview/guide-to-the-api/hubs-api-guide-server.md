---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-server
title: Guia de API de Hubs do ASP.NET SignalR - servidor (c#) | Microsoft Docs
author: pfletcher
description: Este documento fornece uma introdução à programação do lado do servidor da API de Hubs de SignalR do ASP.NET para o SignalR versão 2, com exemplos de código demonstrando...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: b19913e5-cd8a-4e4b-a872-5ac7a858a934
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-server
msc.type: authoredcontent
ms.openlocfilehash: c2567d4d39a494daf77a23db5dff83c8fae4925d
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
ms.locfileid: "28039203"
---
<a name="aspnet-signalr-hubs-api-guide---server-c"></a><span data-ttu-id="3313d-103">Guia de API de Hubs do ASP.NET SignalR - servidor (c#)</span><span class="sxs-lookup"><span data-stu-id="3313d-103">ASP.NET SignalR Hubs API Guide - Server (C#)</span></span>
====================
<span data-ttu-id="3313d-104">por [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="3313d-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="3313d-105">Este documento fornece uma introdução à programação do lado do servidor da API de Hubs de SignalR do ASP.NET para o SignalR versão 2, com exemplos de código demonstrando opções comuns.</span><span class="sxs-lookup"><span data-stu-id="3313d-105">This document provides an introduction to programming the server side of the ASP.NET SignalR Hubs API for SignalR version 2, with code samples demonstrating common options.</span></span>
> 
> <span data-ttu-id="3313d-106">A API de Hubs de SignalR permite fazer chamadas de procedimento remoto (RPCs) de um servidor para clientes conectados e de clientes para o servidor.</span><span class="sxs-lookup"><span data-stu-id="3313d-106">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="3313d-107">No código do servidor, você define métodos que podem ser chamados por clientes e chamar os métodos que são executados no cliente.</span><span class="sxs-lookup"><span data-stu-id="3313d-107">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="3313d-108">No código do cliente, você define métodos que podem ser chamados do servidor e chamar os métodos que são executados no servidor.</span><span class="sxs-lookup"><span data-stu-id="3313d-108">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="3313d-109">SignalR cuida de todos os detalhes do cliente para servidor para você.</span><span class="sxs-lookup"><span data-stu-id="3313d-109">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
> 
> <span data-ttu-id="3313d-110">O SignalR também oferece uma API de nível inferior chamada conexões persistentes.</span><span class="sxs-lookup"><span data-stu-id="3313d-110">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="3313d-111">Para obter uma introdução para o SignalR, Hubs e conexões persistentes, consulte [Introdução ao SignalR 2](../getting-started/introduction-to-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="3313d-111">For an introduction to SignalR, Hubs, and Persistent Connections, see [Introduction to SignalR 2](../getting-started/introduction-to-signalr.md).</span></span>
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="3313d-112">Versões de software usadas neste tópico</span><span class="sxs-lookup"><span data-stu-id="3313d-112">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="3313d-113">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="3313d-113">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="3313d-114">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="3313d-114">.NET 4.5</span></span>
> - <span data-ttu-id="3313d-115">SignalR versão 2</span><span class="sxs-lookup"><span data-stu-id="3313d-115">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="topic-versions"></a><span data-ttu-id="3313d-116">Versões do tópico</span><span class="sxs-lookup"><span data-stu-id="3313d-116">Topic versions</span></span>
> 
> <span data-ttu-id="3313d-117">Para obter informações sobre versões anteriores do SignalR, consulte [versões mais antigas do SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="3313d-117">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="3313d-118">Perguntas e comentários</span><span class="sxs-lookup"><span data-stu-id="3313d-118">Questions and comments</span></span>
> 
> <span data-ttu-id="3313d-119">Deixe comentários em como você gostou neste tutorial e o que podemos melhorar nos comentários na parte inferior da página.</span><span class="sxs-lookup"><span data-stu-id="3313d-119">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="3313d-120">Se você tiver dúvidas que não estão diretamente relacionadas ao tutorial, você poderá postá-los para o [ASP.NET SignalR fórum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="3313d-120">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="3313d-121">Visão geral</span><span class="sxs-lookup"><span data-stu-id="3313d-121">Overview</span></span>

<span data-ttu-id="3313d-122">Este documento contém as seguintes seções:</span><span class="sxs-lookup"><span data-stu-id="3313d-122">This document contains the following sections:</span></span>

- [<span data-ttu-id="3313d-123">Como registrar o SignalR middleware</span><span class="sxs-lookup"><span data-stu-id="3313d-123">How to register SignalR middleware</span></span>](#route)

    - [<span data-ttu-id="3313d-124">A URL de /signalr</span><span class="sxs-lookup"><span data-stu-id="3313d-124">The /signalr URL</span></span>](#signalrurl)
    - [<span data-ttu-id="3313d-125">Configurando opções de SignalR</span><span class="sxs-lookup"><span data-stu-id="3313d-125">Configuring SignalR options</span></span>](#options)
- [<span data-ttu-id="3313d-126">Como criar e usar as classes de Hub</span><span class="sxs-lookup"><span data-stu-id="3313d-126">How to create and use Hub classes</span></span>](#hubclass)

    - [<span data-ttu-id="3313d-127">Vida útil do objeto de Hub</span><span class="sxs-lookup"><span data-stu-id="3313d-127">Hub object lifetime</span></span>](#transience)
    - [<span data-ttu-id="3313d-128">Ter maiusculas e minúsculas dos nomes de Hub em clientes de JavaScript</span><span class="sxs-lookup"><span data-stu-id="3313d-128">Camel-casing of Hub names in JavaScript clients</span></span>](#hubnames)
    - [<span data-ttu-id="3313d-129">Vários Hubs</span><span class="sxs-lookup"><span data-stu-id="3313d-129">Multiple Hubs</span></span>](#multiplehubs)
    - [<span data-ttu-id="3313d-130">Hubs fortemente tipada</span><span class="sxs-lookup"><span data-stu-id="3313d-130">Strongly-Typed Hubs</span></span>](#stronglytypedhubs)
- [<span data-ttu-id="3313d-131">Como definir métodos na classe Hub que os clientes poderão chamar</span><span class="sxs-lookup"><span data-stu-id="3313d-131">How to define methods in the Hub class that clients can call</span></span>](#hubmethods)

    - [<span data-ttu-id="3313d-132">Ter maiusculas e minúsculas dos nomes de método em clientes de JavaScript</span><span class="sxs-lookup"><span data-stu-id="3313d-132">Camel-casing of method names in JavaScript clients</span></span>](#methodnames)
    - [<span data-ttu-id="3313d-133">Ao executar de forma assíncrona</span><span class="sxs-lookup"><span data-stu-id="3313d-133">When to execute asynchronously</span></span>](#asyncmethods)
    - [<span data-ttu-id="3313d-134">Definição de sobrecargas</span><span class="sxs-lookup"><span data-stu-id="3313d-134">Defining overloads</span></span>](#overloads)
    - [<span data-ttu-id="3313d-135">Relatar o andamento de invocações do método de hub</span><span class="sxs-lookup"><span data-stu-id="3313d-135">Reporting progress from hub method invocations</span></span>](#progress)
- [<span data-ttu-id="3313d-136">Como chamar métodos de cliente da classe de Hub</span><span class="sxs-lookup"><span data-stu-id="3313d-136">How to call client methods from the Hub class</span></span>](#callfromhub)

    - [<span data-ttu-id="3313d-137">Selecionar quais clientes receberão o RPC</span><span class="sxs-lookup"><span data-stu-id="3313d-137">Selecting which clients will receive the RPC</span></span>](#selectingclients)
    - [<span data-ttu-id="3313d-138">Nenhuma validação de tempo de compilação para nomes de método</span><span class="sxs-lookup"><span data-stu-id="3313d-138">No compile-time validation for method names</span></span>](#dynamicmethodnames)
    - [<span data-ttu-id="3313d-139">Correspondência de nome do método diferencia maiusculas de minúsculas</span><span class="sxs-lookup"><span data-stu-id="3313d-139">Case-insensitive method name matching</span></span>](#caseinsensitive)
    - [<span data-ttu-id="3313d-140">Execução assíncrona</span><span class="sxs-lookup"><span data-stu-id="3313d-140">Asynchronous execution</span></span>](#asyncclient)
- [<span data-ttu-id="3313d-141">Como gerenciar a associação de grupo da classe de Hub</span><span class="sxs-lookup"><span data-stu-id="3313d-141">How to manage group membership from the Hub class</span></span>](#groupsfromhub)

    - [<span data-ttu-id="3313d-142">Execução assíncrona de métodos Add e Remove</span><span class="sxs-lookup"><span data-stu-id="3313d-142">Asynchronous execution of Add and Remove methods</span></span>](#asyncgroupmethods)
    - [<span data-ttu-id="3313d-143">Persistência de associação de grupo</span><span class="sxs-lookup"><span data-stu-id="3313d-143">Group membership persistence</span></span>](#grouppersistence)
    - [<span data-ttu-id="3313d-144">Grupos de usuário único</span><span class="sxs-lookup"><span data-stu-id="3313d-144">Single-user groups</span></span>](#singleusergroups)
- [<span data-ttu-id="3313d-145">Como manipular eventos de tempo de vida da conexão na classe Hub</span><span class="sxs-lookup"><span data-stu-id="3313d-145">How to handle connection lifetime events in the Hub class</span></span>](#connectionlifetime)

    - [<span data-ttu-id="3313d-146">Quando são chamados OnConnected, OnDisconnected e OnReconnected</span><span class="sxs-lookup"><span data-stu-id="3313d-146">When OnConnected, OnDisconnected, and OnReconnected are called</span></span>](#onreconnected)
    - [<span data-ttu-id="3313d-147">Estado do chamador não preenchido</span><span class="sxs-lookup"><span data-stu-id="3313d-147">Caller state not populated</span></span>](#nocallerstate)
- [<span data-ttu-id="3313d-148">Como obter informações sobre o cliente da propriedade de contexto</span><span class="sxs-lookup"><span data-stu-id="3313d-148">How to get information about the client from the Context property</span></span>](#contextproperty)
- [<span data-ttu-id="3313d-149">Como passar o estado entre clientes e a classe de Hub</span><span class="sxs-lookup"><span data-stu-id="3313d-149">How to pass state between clients and the Hub class</span></span>](#passstate)
- [<span data-ttu-id="3313d-150">Como tratar erros na classe Hub</span><span class="sxs-lookup"><span data-stu-id="3313d-150">How to handle errors in the Hub class</span></span>](#handleErrors)
- [<span data-ttu-id="3313d-151">Como chamar métodos de cliente e gerenciar grupos de fora da classe de Hub</span><span class="sxs-lookup"><span data-stu-id="3313d-151">How to call client methods and manage groups from outside the Hub class</span></span>](#callfromoutsidehub)

    - [<span data-ttu-id="3313d-152">Chamando métodos do cliente</span><span class="sxs-lookup"><span data-stu-id="3313d-152">Calling client methods</span></span>](#callingclientsoutsidehub)
    - [<span data-ttu-id="3313d-153">Gerenciar associação de grupo</span><span class="sxs-lookup"><span data-stu-id="3313d-153">Managing group membership</span></span>](#managinggroupsoutsidehub)
- [<span data-ttu-id="3313d-154">Como habilitar o rastreamento</span><span class="sxs-lookup"><span data-stu-id="3313d-154">How to enable tracing</span></span>](#tracing)
- [<span data-ttu-id="3313d-155">Como personalizar o pipeline de Hubs</span><span class="sxs-lookup"><span data-stu-id="3313d-155">How to customize the Hubs pipeline</span></span>](#hubpipeline)

<span data-ttu-id="3313d-156">Para obter a documentação sobre como os clientes do programa, consulte os seguintes recursos:</span><span class="sxs-lookup"><span data-stu-id="3313d-156">For documentation on how to program clients, see the following resources:</span></span>

- [<span data-ttu-id="3313d-157">Guia de API de Hubs de SignalR - cliente JavaScript</span><span class="sxs-lookup"><span data-stu-id="3313d-157">SignalR Hubs API Guide - JavaScript Client</span></span>](hubs-api-guide-javascript-client.md)
- [<span data-ttu-id="3313d-158">Guia de API de Hubs de SignalR - cliente .NET</span><span class="sxs-lookup"><span data-stu-id="3313d-158">SignalR Hubs API Guide - .NET Client</span></span>](hubs-api-guide-net-client.md)

<span data-ttu-id="3313d-159">Os componentes do servidor para o SignalR 2 somente estão disponíveis no .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="3313d-159">The server components for SignalR 2 are only available in .NET 4.5.</span></span> <span data-ttu-id="3313d-160">Servidores que executam o .NET 4.0 devem usar o SignalR v1. x.</span><span class="sxs-lookup"><span data-stu-id="3313d-160">Servers running .NET 4.0 must use SignalR v1.x.</span></span>

<a id="route"></a>

## <a name="how-to-register-signalr-middleware"></a><span data-ttu-id="3313d-161">Como registrar o SignalR middleware</span><span class="sxs-lookup"><span data-stu-id="3313d-161">How to register SignalR middleware</span></span>

<span data-ttu-id="3313d-162">Para definir a rota que os clientes usarão para se conectar ao seu Hub, chame o `MapSignalR` método quando o aplicativo for iniciado.</span><span class="sxs-lookup"><span data-stu-id="3313d-162">To define the route that clients will use to connect to your Hub, call the `MapSignalR` method when the application starts.</span></span> <span data-ttu-id="3313d-163">`MapSignalR`é um [método de extensão](https://msdn.microsoft.com/library/vstudio/bb383977.aspx) para o `OwinExtensions` classe.</span><span class="sxs-lookup"><span data-stu-id="3313d-163">`MapSignalR` is an [extension method](https://msdn.microsoft.com/library/vstudio/bb383977.aspx) for the `OwinExtensions` class.</span></span> <span data-ttu-id="3313d-164">O exemplo a seguir mostra como definir a rota de Hubs de SignalR usando uma classe de inicialização OWIN.</span><span class="sxs-lookup"><span data-stu-id="3313d-164">The following example shows how to define the SignalR Hubs route using an OWIN startup class.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample1.cs)]

<span data-ttu-id="3313d-165">Se você estiver adicionando funcionalidade SignalR para um aplicativo ASP.NET MVC, certifique-se de que a rota SignalR é adicionada antes de outras rotas.</span><span class="sxs-lookup"><span data-stu-id="3313d-165">If you are adding SignalR functionality to an ASP.NET MVC application, make sure that the SignalR route is added before the other routes.</span></span> <span data-ttu-id="3313d-166">Para obter mais informações, consulte [Tutorial: Introdução ao SignalR 2 e 5 MVC](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span><span class="sxs-lookup"><span data-stu-id="3313d-166">For more information, see [Tutorial: Getting Started with SignalR 2 and MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span></span>

<a id="signalrurl"></a>

### <a name="the-signalr-url"></a><span data-ttu-id="3313d-167">A URL de /signalr</span><span class="sxs-lookup"><span data-stu-id="3313d-167">The /signalr URL</span></span>

<span data-ttu-id="3313d-168">Por padrão, a URL da rota que os clientes usarão para se conectar ao seu Hub é "/ signalr".</span><span class="sxs-lookup"><span data-stu-id="3313d-168">By default, the route URL which clients will use to connect to your Hub is "/signalr".</span></span> <span data-ttu-id="3313d-169">(Não confunda essa URL com a URL "hubs do signalr /", que é para o arquivo JavaScript gerado automaticamente.</span><span class="sxs-lookup"><span data-stu-id="3313d-169">(Don't confuse this URL with the "/signalr/hubs" URL, which is for the automatically generated JavaScript file.</span></span> <span data-ttu-id="3313d-170">Para obter mais informações sobre o proxy gerado, consulte [guia de API de Hubs de SignalR - cliente JavaScript - o proxy gerado e o que ele faz para você](hubs-api-guide-javascript-client.md#genproxy).)</span><span class="sxs-lookup"><span data-stu-id="3313d-170">For more information about the generated proxy, see [SignalR Hubs API Guide - JavaScript Client - The generated proxy and what it does for you](hubs-api-guide-javascript-client.md#genproxy).)</span></span>

<span data-ttu-id="3313d-171">Pode haver extraordinários circunstâncias que tornam essa URL base não pode ser usado para o SignalR; Por exemplo, você tem uma pasta no seu projeto chamado *signalr* e você não deseja alterar o nome.</span><span class="sxs-lookup"><span data-stu-id="3313d-171">There might be extraordinary circumstances that make this base URL not usable for SignalR; for example, you have a folder in your project named *signalr* and you don't want to change the name.</span></span> <span data-ttu-id="3313d-172">Nesse caso, você pode alterar a URL base, conforme mostrado nos exemplos a seguir (substitua "/ signalr" no código de exemplo com sua URL desejado).</span><span class="sxs-lookup"><span data-stu-id="3313d-172">In that case, you can change the base URL, as shown in the following examples (replace "/signalr" in the sample code with your desired URL).</span></span>

<span data-ttu-id="3313d-173">**Código que especifica a URL do servidor**</span><span class="sxs-lookup"><span data-stu-id="3313d-173">**Server code that specifies the URL**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample2.cs?highlight=1)]

<span data-ttu-id="3313d-174">**Código de cliente JavaScript que especifica a URL (com o proxy gerado)**</span><span class="sxs-lookup"><span data-stu-id="3313d-174">**JavaScript client code that specifies the URL (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample3.js?highlight=1)]

<span data-ttu-id="3313d-175">**Código de cliente JavaScript que especifica a URL (sem o proxy gerado)**</span><span class="sxs-lookup"><span data-stu-id="3313d-175">**JavaScript client code that specifies the URL (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample4.js?highlight=1)]

<span data-ttu-id="3313d-176">**Código de cliente .NET que especifica a URL**</span><span class="sxs-lookup"><span data-stu-id="3313d-176">**.NET client code that specifies the URL**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample5.cs?highlight=1)]

<a id="options"></a>

### <a name="configuring-signalr-options"></a><span data-ttu-id="3313d-177">Configurando opções de SignalR</span><span class="sxs-lookup"><span data-stu-id="3313d-177">Configuring SignalR Options</span></span>

<span data-ttu-id="3313d-178">Sobrecargas do `MapSignalR` método permitem que você especifique uma URL personalizada, um resolvedor de dependência personalizadas e as opções a seguir:</span><span class="sxs-lookup"><span data-stu-id="3313d-178">Overloads of the `MapSignalR` method enable you to specify a custom URL, a custom dependency resolver, and the following options:</span></span>

- <span data-ttu-id="3313d-179">Habilite as chamadas entre domínios usando CORS ou JSONP clientes do navegador.</span><span class="sxs-lookup"><span data-stu-id="3313d-179">Enable cross-domain calls using CORS or JSONP from browser clients.</span></span>

    <span data-ttu-id="3313d-180">Normalmente se o navegador carrega uma página da `http://contoso.com`, a conexão do SignalR está no mesmo domínio, no `http://contoso.com/signalr`.</span><span class="sxs-lookup"><span data-stu-id="3313d-180">Typically if the browser loads a page from `http://contoso.com`, the SignalR connection is in the same domain, at `http://contoso.com/signalr`.</span></span> <span data-ttu-id="3313d-181">Se a página de `http://contoso.com` faz uma conexão para `http://fabrikam.com/signalr`, que é uma conexão entre domínios.</span><span class="sxs-lookup"><span data-stu-id="3313d-181">If the page from `http://contoso.com` makes a connection to `http://fabrikam.com/signalr`, that is a cross-domain connection.</span></span> <span data-ttu-id="3313d-182">Por motivos de segurança, conexões de domínio cruzado são desabilitadas por padrão.</span><span class="sxs-lookup"><span data-stu-id="3313d-182">For security reasons, cross-domain connections are disabled by default.</span></span> <span data-ttu-id="3313d-183">Para obter mais informações, consulte [guia de API de Hubs do ASP.NET SignalR - cliente JavaScript - como estabelecer uma conexão entre domínios](hubs-api-guide-javascript-client.md#crossdomain).</span><span class="sxs-lookup"><span data-stu-id="3313d-183">For more information, see [ASP.NET SignalR Hubs API Guide - JavaScript Client - How to establish a cross-domain connection](hubs-api-guide-javascript-client.md#crossdomain).</span></span>
- <span data-ttu-id="3313d-184">Habilite mensagens de erro detalhadas.</span><span class="sxs-lookup"><span data-stu-id="3313d-184">Enable detailed error messages.</span></span>

    <span data-ttu-id="3313d-185">Quando ocorrerem erros, o comportamento padrão do SignalR é enviar aos clientes uma mensagem de notificação sem os detalhes sobre o que aconteceu.</span><span class="sxs-lookup"><span data-stu-id="3313d-185">When errors occur, the default behavior of SignalR is to send to clients a notification message without details about what happened.</span></span> <span data-ttu-id="3313d-186">Enviar informações de erro detalhadas para clientes não é recomendável em produção, como usuários mal-intencionados poderá usar as informações em seu aplicativo de ataques.</span><span class="sxs-lookup"><span data-stu-id="3313d-186">Sending detailed error information to clients is not recommended in production, because malicious users might be able to use the information in attacks against your application.</span></span> <span data-ttu-id="3313d-187">Para solucionar problemas, você pode usar essa opção para habilitar o relatório de erro mais informativo temporariamente.</span><span class="sxs-lookup"><span data-stu-id="3313d-187">For troubleshooting, you can use this option to temporarily enable more informative error reporting.</span></span>
- <span data-ttu-id="3313d-188">Desabilite arquivos de proxy JavaScript gerados automaticamente.</span><span class="sxs-lookup"><span data-stu-id="3313d-188">Disable automatically generated JavaScript proxy files.</span></span>

    <span data-ttu-id="3313d-189">Por padrão, um arquivo JavaScript com proxies para as classes de Hub é gerado em resposta à URL "hubs de signalr /".</span><span class="sxs-lookup"><span data-stu-id="3313d-189">By default, a JavaScript file with proxies for your Hub classes is generated in response to the URL "/signalr/hubs".</span></span> <span data-ttu-id="3313d-190">Se você não quiser usar os proxies JavaScript ou se você deseja gerar esse arquivo manualmente e fazer referência a um arquivo físico em seus clientes, você pode usar essa opção para desativar a geração de proxy.</span><span class="sxs-lookup"><span data-stu-id="3313d-190">If you don't want to use the JavaScript proxies, or if you want to generate this file manually and refer to a physical file in your clients, you can use this option to disable proxy generation.</span></span> <span data-ttu-id="3313d-191">Para obter mais informações, consulte [guia de API de Hubs de SignalR - cliente JavaScript - como criar um arquivo físico para o SignalR gerado proxy](hubs-api-guide-javascript-client.md#manualproxy).</span><span class="sxs-lookup"><span data-stu-id="3313d-191">For more information, see [SignalR Hubs API Guide - JavaScript Client - How to create a physical file for the SignalR generated proxy](hubs-api-guide-javascript-client.md#manualproxy).</span></span>

<span data-ttu-id="3313d-192">O exemplo a seguir mostra como especificar a URL de conexão do SignalR e essas opções em uma chamada para o `MapSignalR` método.</span><span class="sxs-lookup"><span data-stu-id="3313d-192">The following example shows how to specify the SignalR connection URL and these options in a call to the `MapSignalR` method.</span></span> <span data-ttu-id="3313d-193">Para especificar uma URL personalizada, substitua "/ signalr" no exemplo com a URL que você deseja usar.</span><span class="sxs-lookup"><span data-stu-id="3313d-193">To specify a custom URL, replace "/signalr" in the example with the URL that you want to use.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample6.cs)]

<a id="hubclass"></a>

## <a name="how-to-create-and-use-hub-classes"></a><span data-ttu-id="3313d-194">Como criar e usar as classes de Hub</span><span class="sxs-lookup"><span data-stu-id="3313d-194">How to create and use Hub classes</span></span>

<span data-ttu-id="3313d-195">Para criar um Hub, crie uma classe que deriva de [Microsoft.Aspnet.Signalr.Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="3313d-195">To create a Hub, create a class that derives from [Microsoft.Aspnet.Signalr.Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx).</span></span> <span data-ttu-id="3313d-196">O exemplo a seguir mostra uma classe simples do Hub para um aplicativo de bate-papo.</span><span class="sxs-lookup"><span data-stu-id="3313d-196">The following example shows a simple Hub class for a chat application.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample7.cs)]

<span data-ttu-id="3313d-197">Neste exemplo, um cliente conectado pode chamar o `NewContosoChatMessage` método, e quando isso acontecer, os dados recebidos são transmitidos para todos os clientes conectados.</span><span class="sxs-lookup"><span data-stu-id="3313d-197">In this example, a connected client can call the `NewContosoChatMessage` method, and when it does, the data received is broadcasted to all connected clients.</span></span>

<a id="transience"></a>

### <a name="hub-object-lifetime"></a><span data-ttu-id="3313d-198">Vida útil do objeto de Hub</span><span class="sxs-lookup"><span data-stu-id="3313d-198">Hub object lifetime</span></span>

<span data-ttu-id="3313d-199">Você não criar uma instância da classe do Hub ou chamar seus métodos de seu próprio código no servidor. tudo isso é feito para você pelo pipeline Hubs do SignalR.</span><span class="sxs-lookup"><span data-stu-id="3313d-199">You don't instantiate the Hub class or call its methods from your own code on the server; all that is done for you by the SignalR Hubs pipeline.</span></span> <span data-ttu-id="3313d-200">SignalR cria uma nova instância da classe do Hub de cada vez que precisa tratar de uma operação de Hub, como quando um cliente se conecta, desconecta ou faz um método de chamada para o servidor.</span><span class="sxs-lookup"><span data-stu-id="3313d-200">SignalR creates a new instance of your Hub class each time it needs to handle a Hub operation such as when a client connects, disconnects, or makes a method call to the server.</span></span>

<span data-ttu-id="3313d-201">Como instâncias da classe Hub são transitórias, você não pode usá-los para manter o estado da chamada de um método para a próxima.</span><span class="sxs-lookup"><span data-stu-id="3313d-201">Because instances of the Hub class are transient, you can't use them to maintain state from one method call to the next.</span></span> <span data-ttu-id="3313d-202">Cada vez que o servidor recebe uma chamada de método em um cliente, uma nova instância de seus processos de classe do Hub a mensagem.</span><span class="sxs-lookup"><span data-stu-id="3313d-202">Each time the server receives a method call from a client, a new instance of your Hub class processes the message.</span></span> <span data-ttu-id="3313d-203">Para manter o estado por meio de várias conexões e chamadas de método, use outro método, como um banco de dados ou uma variável estática na classe Hub ou uma classe diferente que não derivam de `Hub`.</span><span class="sxs-lookup"><span data-stu-id="3313d-203">To maintain state through multiple connections and method calls, use some other method such as a database, or a static variable on the Hub class, or a different class that does not derive from `Hub`.</span></span> <span data-ttu-id="3313d-204">Se você mantiver os dados na memória, usando um método como uma variável estática na classe Hub, os dados serão perdidos quando o domínio de aplicativo é reciclado.</span><span class="sxs-lookup"><span data-stu-id="3313d-204">If you persist data in memory, using a method such as a static variable on the Hub class, the data will be lost when the app domain recycles.</span></span>

<span data-ttu-id="3313d-205">Se você quiser enviar mensagens para os clientes do seu próprio código executado fora da classe de Hub, você não pode fazê-lo criando uma instância da classe de Hub, mas você pode fazê-lo ao obter uma referência para o objeto de contexto SignalR para sua classe de Hub.</span><span class="sxs-lookup"><span data-stu-id="3313d-205">If you want to send messages to clients from your own code that runs outside the Hub class, you can't do it by instantiating a Hub class instance, but you can do it by getting a reference to the SignalR context object for your Hub class.</span></span> <span data-ttu-id="3313d-206">Para obter mais informações, consulte [como chamar métodos de cliente e gerenciar grupos de fora da classe Hub](#callfromoutsidehub) mais adiante neste tópico.</span><span class="sxs-lookup"><span data-stu-id="3313d-206">For more information, see [How to call client methods and manage groups from outside the Hub class](#callfromoutsidehub) later in this topic.</span></span>

<a id="hubnames"></a>

### <a name="camel-casing-of-hub-names-in-javascript-clients"></a><span data-ttu-id="3313d-207">Ter maiusculas e minúsculas dos nomes de Hub em clientes de JavaScript</span><span class="sxs-lookup"><span data-stu-id="3313d-207">Camel-casing of Hub names in JavaScript clients</span></span>

<span data-ttu-id="3313d-208">Por padrão, os clientes JavaScript consultem Hubs usando uma versão concatenados do nome da classe.</span><span class="sxs-lookup"><span data-stu-id="3313d-208">By default, JavaScript clients refer to Hubs by using a camel-cased version of the class name.</span></span> <span data-ttu-id="3313d-209">SignalR automaticamente faz essa alteração para que o código JavaScript pode seguir as convenções de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="3313d-209">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span> <span data-ttu-id="3313d-210">O exemplo anterior seria conhecido como `contosoChatHub` no código JavaScript.</span><span class="sxs-lookup"><span data-stu-id="3313d-210">The previous example would be referred to as `contosoChatHub` in JavaScript code.</span></span>

<span data-ttu-id="3313d-211">**Servidor**</span><span class="sxs-lookup"><span data-stu-id="3313d-211">**Server**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample8.cs?highlight=1)]

<span data-ttu-id="3313d-212">**Cliente JavaScript usando o proxy gerado**</span><span class="sxs-lookup"><span data-stu-id="3313d-212">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample9.js?highlight=1)]

<span data-ttu-id="3313d-213">Se você deseja especificar um nome diferente para os clientes usar, adicione o `HubName` atributo.</span><span class="sxs-lookup"><span data-stu-id="3313d-213">If you want to specify a different name for clients to use, add the `HubName` attribute.</span></span> <span data-ttu-id="3313d-214">Quando você usa um `HubName` de atributo, não há nenhuma alteração de nome para concatenação com maiusculas em clientes JavaScript.</span><span class="sxs-lookup"><span data-stu-id="3313d-214">When you use a `HubName` attribute, there is no name change to camel case on JavaScript clients.</span></span>

<span data-ttu-id="3313d-215">**Servidor**</span><span class="sxs-lookup"><span data-stu-id="3313d-215">**Server**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample10.cs?highlight=1)]

<span data-ttu-id="3313d-216">**Cliente JavaScript usando o proxy gerado**</span><span class="sxs-lookup"><span data-stu-id="3313d-216">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample11.js?highlight=1)]

<a id="multiplehubs"></a>

### <a name="multiple-hubs"></a><span data-ttu-id="3313d-217">Vários Hubs</span><span class="sxs-lookup"><span data-stu-id="3313d-217">Multiple Hubs</span></span>

<span data-ttu-id="3313d-218">Você pode definir várias classes de Hub em um aplicativo.</span><span class="sxs-lookup"><span data-stu-id="3313d-218">You can define multiple Hub classes in an application.</span></span> <span data-ttu-id="3313d-219">Quando você faz isso, a conexão é compartilhada, mas os grupos são separados:</span><span class="sxs-lookup"><span data-stu-id="3313d-219">When you do that, the connection is shared but groups are separate:</span></span>

- <span data-ttu-id="3313d-220">Todos os clientes usarão a mesma URL para estabelecer uma conexão SignalR com seu serviço ("/ signalr" ou o URL personalizado especificado), e que a conexão é usada para todos os Hubs são definidos pelo serviço.</span><span class="sxs-lookup"><span data-stu-id="3313d-220">All clients will use the same URL to establish a SignalR connection with your service ("/signalr" or your custom URL if you specified one), and that connection is used for all Hubs defined by the service.</span></span>

    <span data-ttu-id="3313d-221">Não há nenhuma diferença de desempenho para vários Hubs comparado ao definir todas as funcionalidades de Hub em uma única classe.</span><span class="sxs-lookup"><span data-stu-id="3313d-221">There is no performance difference for multiple Hubs compared to defining all Hub functionality in a single class.</span></span>
- <span data-ttu-id="3313d-222">Todos os Hubs de obtém as mesmas informações de solicitação HTTP.</span><span class="sxs-lookup"><span data-stu-id="3313d-222">All Hubs get the same HTTP request information.</span></span>

    <span data-ttu-id="3313d-223">Como todos os Hubs compartilham a mesma conexão, as únicas informações de solicitação HTTP que obtém o servidor são o que é fornecido na solicitação HTTP original que estabelece a conexão do SignalR.</span><span class="sxs-lookup"><span data-stu-id="3313d-223">Since all Hubs share the same connection, the only HTTP request information that the server gets is what comes in the original HTTP request that establishes the SignalR connection.</span></span> <span data-ttu-id="3313d-224">Se você usar a solicitação de conexão para passar informações do cliente para o servidor especificando uma cadeia de caracteres de consulta, você não pode fornecer cadeias de caracteres de consulta diferentes para diferentes Hubs.</span><span class="sxs-lookup"><span data-stu-id="3313d-224">If you use the connection request to pass information from the client to the server by specifying a query string, you can't provide different query strings to different Hubs.</span></span> <span data-ttu-id="3313d-225">Todos os Hubs receberão as mesmas informações.</span><span class="sxs-lookup"><span data-stu-id="3313d-225">All Hubs will receive the same information.</span></span>
- <span data-ttu-id="3313d-226">O arquivo de proxies JavaScript gerado conterá proxies para todos os Hubs em um arquivo.</span><span class="sxs-lookup"><span data-stu-id="3313d-226">The generated JavaScript proxies file will contain proxies for all Hubs in one file.</span></span>

    <span data-ttu-id="3313d-227">Para obter informações sobre proxies de JavaScript, consulte [guia de API de Hubs de SignalR - cliente JavaScript - o proxy gerado e o que ele faz para você](hubs-api-guide-javascript-client.md#genproxy).</span><span class="sxs-lookup"><span data-stu-id="3313d-227">For information about JavaScript proxies, see [SignalR Hubs API Guide - JavaScript Client - The generated proxy and what it does for you](hubs-api-guide-javascript-client.md#genproxy).</span></span>
- <span data-ttu-id="3313d-228">Grupos são definidos dentro de Hubs.</span><span class="sxs-lookup"><span data-stu-id="3313d-228">Groups are defined within Hubs.</span></span>

    <span data-ttu-id="3313d-229">Chamada de SignalR, que você pode definir grupos para difusão para subconjuntos de clientes conectados.</span><span class="sxs-lookup"><span data-stu-id="3313d-229">In SignalR you can define named groups to broadcast to subsets of connected clients.</span></span> <span data-ttu-id="3313d-230">Grupos são mantidos separadamente para cada Hub.</span><span class="sxs-lookup"><span data-stu-id="3313d-230">Groups are maintained separately for each Hub.</span></span> <span data-ttu-id="3313d-231">Por exemplo, um grupo chamado "Administradores" inclui um conjunto de clientes para o `ContosoChatHub` classe e o mesmo nome de grupo faz referência a um conjunto diferente de clientes para seu `StockTickerHub` classe.</span><span class="sxs-lookup"><span data-stu-id="3313d-231">For example, a group named "Administrators" would include one set of clients for your `ContosoChatHub` class, and the same group name would refer to a different set of clients for your `StockTickerHub` class.</span></span>

<a id="stronglytypedhubs"></a>
### <a name="strongly-typed-hubs"></a><span data-ttu-id="3313d-232">Hubs fortemente tipada</span><span class="sxs-lookup"><span data-stu-id="3313d-232">Strongly-Typed Hubs</span></span>

<span data-ttu-id="3313d-233">Para definir uma interface para seus métodos de hub que seu cliente pode referência (e habilitar o Intellisense em seus métodos de hub), derivar seu hub de `Hub<T>` (introduzido no SignalR 2.1) em vez de `Hub`:</span><span class="sxs-lookup"><span data-stu-id="3313d-233">To define an interface for your hub methods that your client can reference (and enable Intellisense on your hub methods), derive your hub from `Hub<T>` (introduced in SignalR 2.1) rather than `Hub`:</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample12.cs)]

<a id="hubmethods"></a>

## <a name="how-to-define-methods-in-the-hub-class-that-clients-can-call"></a><span data-ttu-id="3313d-234">Como definir métodos na classe Hub que os clientes poderão chamar</span><span class="sxs-lookup"><span data-stu-id="3313d-234">How to define methods in the Hub class that clients can call</span></span>

<span data-ttu-id="3313d-235">Para expor um método no Hub que você deseja ser chamado do cliente, declare um método público, conforme mostrado nos exemplos a seguir.</span><span class="sxs-lookup"><span data-stu-id="3313d-235">To expose a method on the Hub that you want to be callable from the client, declare a public method, as shown in the following examples.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample13.cs?highlight=3)]

[!code-csharp[Main](hubs-api-guide-server/samples/sample14.cs?highlight=3)]

<span data-ttu-id="3313d-236">Você pode especificar um tipo de retorno e parâmetros, incluindo tipos complexos e matrizes, como você faria em qualquer método em c#.</span><span class="sxs-lookup"><span data-stu-id="3313d-236">You can specify a return type and parameters, including complex types and arrays, as you would in any C# method.</span></span> <span data-ttu-id="3313d-237">Todos os dados recebidos em parâmetros ou retornar ao chamador trocados entre o cliente e o servidor usando JSON e SignalR manipula automaticamente a associação de objetos complexos e matrizes de objetos.</span><span class="sxs-lookup"><span data-stu-id="3313d-237">Any data that you receive in parameters or return to the caller is communicated between the client and the server by using JSON, and SignalR handles the binding of complex objects and arrays of objects automatically.</span></span>

<a id="methodnames"></a>

### <a name="camel-casing-of-method-names-in-javascript-clients"></a><span data-ttu-id="3313d-238">Ter maiusculas e minúsculas dos nomes de método em clientes de JavaScript</span><span class="sxs-lookup"><span data-stu-id="3313d-238">Camel-casing of method names in JavaScript clients</span></span>

<span data-ttu-id="3313d-239">Por padrão, os clientes JavaScript consultem métodos de Hub usando uma versão concatenados do nome do método.</span><span class="sxs-lookup"><span data-stu-id="3313d-239">By default, JavaScript clients refer to Hub methods by using a camel-cased version of the method name.</span></span> <span data-ttu-id="3313d-240">SignalR automaticamente faz essa alteração para que o código JavaScript pode seguir as convenções de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="3313d-240">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="3313d-241">**Servidor**</span><span class="sxs-lookup"><span data-stu-id="3313d-241">**Server**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample15.cs?highlight=1)]

<span data-ttu-id="3313d-242">**Cliente JavaScript usando o proxy gerado**</span><span class="sxs-lookup"><span data-stu-id="3313d-242">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample16.js?highlight=1)]

<span data-ttu-id="3313d-243">Se você deseja especificar um nome diferente para os clientes usar, adicione o `HubMethodName` atributo.</span><span class="sxs-lookup"><span data-stu-id="3313d-243">If you want to specify a different name for clients to use, add the `HubMethodName` attribute.</span></span>

<span data-ttu-id="3313d-244">**Servidor**</span><span class="sxs-lookup"><span data-stu-id="3313d-244">**Server**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample17.cs?highlight=1)]

<span data-ttu-id="3313d-245">**Cliente JavaScript usando o proxy gerado**</span><span class="sxs-lookup"><span data-stu-id="3313d-245">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample18.js?highlight=1)]

<a id="asyncmethods"></a>

### <a name="when-to-execute-asynchronously"></a><span data-ttu-id="3313d-246">Ao executar de forma assíncrona</span><span class="sxs-lookup"><span data-stu-id="3313d-246">When to execute asynchronously</span></span>

<span data-ttu-id="3313d-247">Se o método será ser demoradas ou tiver de funcionar seria envolvem espera, como uma pesquisa de banco de dados ou uma chamada de serviço da web, verifique o método de Hub assíncrona, retornando um [tarefa](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) (em vez de `void` retornar) ou [ Tarefa&lt;T&gt; ](https://msdn.microsoft.com/library/dd321424.aspx) objeto (em vez de `T` tipo de retorno).</span><span class="sxs-lookup"><span data-stu-id="3313d-247">If the method will be long-running or has to do work that would involve waiting, such as a database lookup or a web service call, make the Hub method asynchronous by returning a [Task](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) (in place of `void` return) or [Task&lt;T&gt;](https://msdn.microsoft.com/library/dd321424.aspx) object (in place of `T` return type).</span></span> <span data-ttu-id="3313d-248">Ao retornar um `Task` objeto do método, SignalR aguarda o `Task` para ser concluída, e, em seguida, ele envia o resultado desencapsulamento volta ao cliente, portanto, não há nenhuma diferença em como você o código de chamada do método no cliente.</span><span class="sxs-lookup"><span data-stu-id="3313d-248">When you return a `Task` object from the method, SignalR waits for the `Task` to complete, and then it sends the unwrapped result back to the client, so there is no difference in how you code the method call in the client.</span></span>

<span data-ttu-id="3313d-249">Fazer um método de Hub assíncrona evita bloqueando a conexão quando ele usa o transporte de WebSocket.</span><span class="sxs-lookup"><span data-stu-id="3313d-249">Making a Hub method asynchronous avoids blocking the connection when it uses the WebSocket transport.</span></span> <span data-ttu-id="3313d-250">Quando um método de Hub é executado de modo síncrono e o transporte é WebSocket, invocações subsequentes de métodos de Hub do mesmo cliente serão bloqueadas até que o método de Hub é concluído.</span><span class="sxs-lookup"><span data-stu-id="3313d-250">When a Hub method executes synchronously and the transport is WebSocket, subsequent invocations of methods on the Hub from the same client are blocked until the Hub method completes.</span></span>

<span data-ttu-id="3313d-251">A exemplo a seguir mostra o mesmo método codificados para executar de forma síncrona ou assíncrona, seguido do código de cliente JavaScript que funciona para chamar a versão.</span><span class="sxs-lookup"><span data-stu-id="3313d-251">The following example shows the same method coded to run synchronously or asynchronously, followed by JavaScript client code that works for calling either version.</span></span>

<span data-ttu-id="3313d-252">**Síncrono**</span><span class="sxs-lookup"><span data-stu-id="3313d-252">**Synchronous**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample19.cs)]

<span data-ttu-id="3313d-253">**Asynchronous**</span><span class="sxs-lookup"><span data-stu-id="3313d-253">**Asynchronous**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample20.cs?highlight=1,7-8)]

<span data-ttu-id="3313d-254">**Cliente JavaScript usando o proxy gerado**</span><span class="sxs-lookup"><span data-stu-id="3313d-254">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample21.js)]

<span data-ttu-id="3313d-255">Para obter mais informações sobre como usar métodos assíncronos no ASP.NET 4.5, consulte [usando os métodos assíncronos no ASP.NET MVC 4](../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).</span><span class="sxs-lookup"><span data-stu-id="3313d-255">For more information about how to use asynchronous methods in ASP.NET 4.5, see [Using Asynchronous Methods in ASP.NET MVC 4](../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).</span></span>

<a id="overloads"></a>

### <a name="defining-overloads"></a><span data-ttu-id="3313d-256">Definição de sobrecargas</span><span class="sxs-lookup"><span data-stu-id="3313d-256">Defining Overloads</span></span>

<span data-ttu-id="3313d-257">Se você quiser definir sobrecargas de um método, o número de parâmetros em cada sobrecarga deve ser diferente.</span><span class="sxs-lookup"><span data-stu-id="3313d-257">If you want to define overloads for a method, the number of parameters in each overload must be different.</span></span> <span data-ttu-id="3313d-258">Se você diferenciar uma sobrecarga especificando tipos de parâmetro diferente, sua classe Hub serão compilados, mas o serviço SignalR lançará uma exceção em tempo de execução quando os clientes tentam para chamada de uma das sobrecargas.</span><span class="sxs-lookup"><span data-stu-id="3313d-258">If you differentiate an overload just by specifying different parameter types, your Hub class will compile but the SignalR service will throw an exception at run time when clients try to call one of the overloads.</span></span>

<a id="progress"></a>
### <a name="reporting-progress-from-hub-method-invocations"></a><span data-ttu-id="3313d-259">Relatar o andamento de invocações do método de hub</span><span class="sxs-lookup"><span data-stu-id="3313d-259">Reporting progress from hub method invocations</span></span>

<span data-ttu-id="3313d-260">SignalR 2.1 adiciona suporte para o [padrão de relatório de andamento](https://blogs.msdn.com/b/dotnet/archive/2012/06/06/async-in-4-5-enabling-progress-and-cancellation-in-async-apis.aspx) introduzido no .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="3313d-260">SignalR 2.1 adds support for the [progress reporting pattern](https://blogs.msdn.com/b/dotnet/archive/2012/06/06/async-in-4-5-enabling-progress-and-cancellation-in-async-apis.aspx) introduced in .NET 4.5.</span></span> <span data-ttu-id="3313d-261">Para implementar o relatório de andamento, definir um `IProgress<T>` parâmetro para o método de hub que seu cliente pode acessar:</span><span class="sxs-lookup"><span data-stu-id="3313d-261">To implement progress reporting, define an `IProgress<T>` parameter for your hub method that your client can access:</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample22.cs)]

<span data-ttu-id="3313d-262">Ao escrever um método de servidor de longa execução, é importante usar um padrão de programação assíncrono com Async / Await em vez de bloquear o thread de hub.</span><span class="sxs-lookup"><span data-stu-id="3313d-262">When writing a long-running server method, it is important to use an asynchronous programming pattern like Async/ Await rather than blocking the hub thread.</span></span>

<a id="callfromhub"></a>

## <a name="how-to-call-client-methods-from-the-hub-class"></a><span data-ttu-id="3313d-263">Como chamar métodos de cliente da classe de Hub</span><span class="sxs-lookup"><span data-stu-id="3313d-263">How to call client methods from the Hub class</span></span>

<span data-ttu-id="3313d-264">Para chamar métodos de cliente do servidor, use o `Clients` propriedade em um método na classe Hub.</span><span class="sxs-lookup"><span data-stu-id="3313d-264">To call client methods from the server, use the `Clients` property in a method in your Hub class.</span></span> <span data-ttu-id="3313d-265">O exemplo a seguir mostra o código de servidor que chama `addNewMessageToPage` em todos os clientes e o código de cliente que define o método em um cliente JavaScript.</span><span class="sxs-lookup"><span data-stu-id="3313d-265">The following example shows server code that calls `addNewMessageToPage` on all connected clients, and client code that defines the method in a JavaScript client.</span></span>

<span data-ttu-id="3313d-266">**Servidor**</span><span class="sxs-lookup"><span data-stu-id="3313d-266">**Server**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample23.cs?highlight=5)]

<span data-ttu-id="3313d-267">**Cliente JavaScript usando o proxy gerado**</span><span class="sxs-lookup"><span data-stu-id="3313d-267">**JavaScript client using generated proxy**</span></span>

[!code-html[Main](hubs-api-guide-server/samples/sample24.html?highlight=1)]

<span data-ttu-id="3313d-268">Não é possível obter um valor de retorno de um método de cliente; sintaxe como `int x = Clients.All.add(1,1)` não funciona.</span><span class="sxs-lookup"><span data-stu-id="3313d-268">You can't get a return value from a client method; syntax such as `int x = Clients.All.add(1,1)` does not work.</span></span>

<span data-ttu-id="3313d-269">Você pode especificar os tipos complexos e matrizes de parâmetros.</span><span class="sxs-lookup"><span data-stu-id="3313d-269">You can specify complex types and arrays for the parameters.</span></span> <span data-ttu-id="3313d-270">O exemplo a seguir passa um tipo complexo para o cliente em um parâmetro de método.</span><span class="sxs-lookup"><span data-stu-id="3313d-270">The following example passes a complex type to the client in a method parameter.</span></span>

<span data-ttu-id="3313d-271">**Código de servidor que chama um método de cliente usando um objeto complexo**</span><span class="sxs-lookup"><span data-stu-id="3313d-271">**Server code that calls a client method using a complex object**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample25.cs?highlight=3)]

<span data-ttu-id="3313d-272">**Código do servidor que define o objeto complexo**</span><span class="sxs-lookup"><span data-stu-id="3313d-272">**Server code that defines the complex object**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample26.cs?highlight=1)]

<span data-ttu-id="3313d-273">**Cliente JavaScript usando o proxy gerado**</span><span class="sxs-lookup"><span data-stu-id="3313d-273">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample27.js?highlight=2-3)]

<a id="selectingclients"></a>

### <a name="selecting-which-clients-will-receive-the-rpc"></a><span data-ttu-id="3313d-274">Selecionar quais clientes receberão o RPC</span><span class="sxs-lookup"><span data-stu-id="3313d-274">Selecting which clients will receive the RPC</span></span>

<span data-ttu-id="3313d-275">A propriedade retorna clientes um [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx) objeto que fornece várias opções para especificar quais clientes receberão o RPC:</span><span class="sxs-lookup"><span data-stu-id="3313d-275">The Clients property returns a [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx) object that provides several options for specifying which clients will receive the RPC:</span></span>

- <span data-ttu-id="3313d-276">Todos os clientes conectados.</span><span class="sxs-lookup"><span data-stu-id="3313d-276">All connected clients.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample28.cs)]
- <span data-ttu-id="3313d-277">Somente o cliente da chamada.</span><span class="sxs-lookup"><span data-stu-id="3313d-277">Only the calling client.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample29.cs)]
- <span data-ttu-id="3313d-278">Todos os clientes, exceto o cliente da chamada.</span><span class="sxs-lookup"><span data-stu-id="3313d-278">All clients except the calling client.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample30.cs)]
- <span data-ttu-id="3313d-279">Um cliente específico identificado pela ID de conexão.</span><span class="sxs-lookup"><span data-stu-id="3313d-279">A specific client identified by connection ID.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample31.css)]

    <span data-ttu-id="3313d-280">Este exemplo chama `addContosoChatMessageToPage` no cliente da chamada e tem o mesmo efeito que usar `Clients.Caller`.</span><span class="sxs-lookup"><span data-stu-id="3313d-280">This example calls `addContosoChatMessageToPage` on the calling client and has the same effect as using `Clients.Caller`.</span></span>
- <span data-ttu-id="3313d-281">Todos os clientes conectados, exceto clientes especificados, identificados pela ID de conexão.</span><span class="sxs-lookup"><span data-stu-id="3313d-281">All connected clients except the specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample32.cs)]
- <span data-ttu-id="3313d-282">Todos os clientes em um grupo especificado.</span><span class="sxs-lookup"><span data-stu-id="3313d-282">All connected clients in a specified group.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample33.css)]
- <span data-ttu-id="3313d-283">Todos os clientes conectados em um grupo especificado, exceto clientes especificados, identificados pela ID de conexão.</span><span class="sxs-lookup"><span data-stu-id="3313d-283">All connected clients in a specified group except the specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample34.cs)]
- <span data-ttu-id="3313d-284">Todos os clientes conectados em um grupo especificado, exceto o cliente da chamada.</span><span class="sxs-lookup"><span data-stu-id="3313d-284">All connected clients in a specified group except the calling client.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample35.css)]
- <span data-ttu-id="3313d-285">Um usuário específico, identificado pela ID de usuário.</span><span class="sxs-lookup"><span data-stu-id="3313d-285">A specific user, identified by userId.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample36.cs)]

    <span data-ttu-id="3313d-286">Por padrão, isso é `IPrincipal.Identity.Name`, mas isso pode ser alterado por [registrar uma implementação de IUserIdProvider com o host global](mapping-users-to-connections.md#IUserIdProvider).</span><span class="sxs-lookup"><span data-stu-id="3313d-286">By default, this is `IPrincipal.Identity.Name`, but this can be changed by [registering an implementation of IUserIdProvider with the global host](mapping-users-to-connections.md#IUserIdProvider).</span></span>
- <span data-ttu-id="3313d-287">Todos os clientes e grupos em uma lista de IDs de conexão.</span><span class="sxs-lookup"><span data-stu-id="3313d-287">All clients and groups in a list of connection IDs.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample37.css)]
- <span data-ttu-id="3313d-288">Uma lista de grupos.</span><span class="sxs-lookup"><span data-stu-id="3313d-288">A list of groups.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample38.css)]
- <span data-ttu-id="3313d-289">Um usuário por nome.</span><span class="sxs-lookup"><span data-stu-id="3313d-289">A user by name.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample39.cs)]
- <span data-ttu-id="3313d-290">Uma lista de nomes de usuário (introduzido no SignalR 2.1).</span><span class="sxs-lookup"><span data-stu-id="3313d-290">A list of user names (introduced in SignalR 2.1).</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample40.cs)]

<a id="dynamicmethodnames"></a>

### <a name="no-compile-time-validation-for-method-names"></a><span data-ttu-id="3313d-291">Nenhuma validação de tempo de compilação para nomes de método</span><span class="sxs-lookup"><span data-stu-id="3313d-291">No compile-time validation for method names</span></span>

<span data-ttu-id="3313d-292">O nome do método que você especificar será interpretado como um objeto dinâmico, o que significa que não há IntelliSense ou validação de tempo de compilação para ele.</span><span class="sxs-lookup"><span data-stu-id="3313d-292">The method name that you specify is interpreted as a dynamic object, which means there is no IntelliSense or compile-time validation for it.</span></span> <span data-ttu-id="3313d-293">A expressão é avaliada em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="3313d-293">The expression is evaluated at run time.</span></span> <span data-ttu-id="3313d-294">Quando a chamada do método é executado, SignalR envia o nome do método e os valores de parâmetro para o cliente, e se o cliente tiver um método que corresponde ao nome, que o método é chamado e os valores de parâmetro são passados para ele.</span><span class="sxs-lookup"><span data-stu-id="3313d-294">When the method call executes, SignalR sends the method name and the parameter values to the client, and if the client has a method that matches the name, that method is called and the parameter values are passed to it.</span></span> <span data-ttu-id="3313d-295">Se nenhum método correspondente for encontrado no cliente, nenhum erro será gerado.</span><span class="sxs-lookup"><span data-stu-id="3313d-295">If no matching method is found on the client, no error is raised.</span></span> <span data-ttu-id="3313d-296">Para obter informações sobre o formato dos dados que SignalR transmite para o cliente em segundo plano quando você chama um método de cliente, consulte [Introdução ao SignalR](../getting-started/introduction-to-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="3313d-296">For information about the format of the data that SignalR transmits to the client behind the scenes when you call a client method, see [Introduction to SignalR](../getting-started/introduction-to-signalr.md).</span></span>

<a id="caseinsensitive"></a>

### <a name="case-insensitive-method-name-matching"></a><span data-ttu-id="3313d-297">Correspondência de nome do método diferencia maiusculas de minúsculas</span><span class="sxs-lookup"><span data-stu-id="3313d-297">Case-insensitive method name matching</span></span>

<span data-ttu-id="3313d-298">Correspondência de nome do método diferencia maiusculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="3313d-298">Method name matching is case-insensitive.</span></span> <span data-ttu-id="3313d-299">Por exemplo, `Clients.All.addContosoChatMessageToPage` no servidor executará `AddContosoChatMessageToPage`, `addcontosochatmessagetopage`, ou `addContosoChatMessageToPage` no cliente.</span><span class="sxs-lookup"><span data-stu-id="3313d-299">For example, `Clients.All.addContosoChatMessageToPage` on the server will execute `AddContosoChatMessageToPage`, `addcontosochatmessagetopage`, or `addContosoChatMessageToPage` on the client.</span></span>

<a id="asyncclient"></a>

### <a name="asynchronous-execution"></a><span data-ttu-id="3313d-300">Execução assíncrona</span><span class="sxs-lookup"><span data-stu-id="3313d-300">Asynchronous execution</span></span>

<span data-ttu-id="3313d-301">O método que você chamar executa de forma assíncrona.</span><span class="sxs-lookup"><span data-stu-id="3313d-301">The method that you call executes asynchronously.</span></span> <span data-ttu-id="3313d-302">Qualquer código que vem após uma chamada de método a um cliente serão executadas imediatamente sem aguardar o SignalR concluir a transmissão de dados aos clientes, a menos que você especificar que as linhas subsequentes de código deve aguardar a conclusão do método.</span><span class="sxs-lookup"><span data-stu-id="3313d-302">Any code that comes after a method call to a client will execute immediately without waiting for SignalR to finish transmitting data to clients unless you specify that the subsequent lines of code should wait for method completion.</span></span> <span data-ttu-id="3313d-303">O exemplo de código a seguir mostra como executar os dois métodos de cliente em sequência.</span><span class="sxs-lookup"><span data-stu-id="3313d-303">The following code example shows how to execute two client methods sequentially.</span></span>

<span data-ttu-id="3313d-304">**Usando Await (.NET 4.5)**</span><span class="sxs-lookup"><span data-stu-id="3313d-304">**Using Await (.NET 4.5)**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample41.cs?highlight=1,3)]

<span data-ttu-id="3313d-305">Se você usar `await` para esperar até que um método de cliente seja concluída antes de executa a próxima linha de código, isso não significa que os clientes, na verdade, receberá a mensagem antes de executa a próxima linha de código.</span><span class="sxs-lookup"><span data-stu-id="3313d-305">If you use `await` to wait until a client method finishes before the next line of code executes, that does not mean that clients will actually receive the message before the next line of code executes.</span></span> <span data-ttu-id="3313d-306">"Conclusão" de uma chamada de método de cliente significa apenas que o SignalR fez tudo o que é necessário para enviar a mensagem.</span><span class="sxs-lookup"><span data-stu-id="3313d-306">"Completion" of a client method call only means that SignalR has done everything necessary to send the message.</span></span> <span data-ttu-id="3313d-307">Se você precisar que os clientes receberam a mensagem de verificação, você precisa programar esse mecanismo por conta própria.</span><span class="sxs-lookup"><span data-stu-id="3313d-307">If you need verification that clients received the message, you have to program that mechanism yourself.</span></span> <span data-ttu-id="3313d-308">Por exemplo, você pode codificar um `MessageReceived` método no Hub e no `addContosoChatMessageToPage` método no cliente, você poderia chamar `MessageReceived` depois de fazer qualquer trabalho que você precisa fazer no cliente.</span><span class="sxs-lookup"><span data-stu-id="3313d-308">For example, you could code a `MessageReceived` method on the Hub, and in the `addContosoChatMessageToPage` method on the client you could call `MessageReceived` after you do whatever work you need to do on the client.</span></span> <span data-ttu-id="3313d-309">Em `MessageReceived` no Hub, você pode fazer o trabalho depende de recepção de cliente real e processamento da chamada do método original.</span><span class="sxs-lookup"><span data-stu-id="3313d-309">In `MessageReceived` in the Hub you can do whatever work depends on actual client reception and processing of the original method call.</span></span>

### <a name="how-to-use-a-string-variable-as-the-method-name"></a><span data-ttu-id="3313d-310">Como usar uma variável de cadeia de caracteres como o nome do método</span><span class="sxs-lookup"><span data-stu-id="3313d-310">How to use a string variable as the method name</span></span>

<span data-ttu-id="3313d-311">Se você deseja invocar um método de cliente usando uma variável de cadeia de caracteres como o nome do método, converter `Clients.All` (ou `Clients.Others`, `Clients.Caller`, etc.) para `IClientProxy` e, em seguida, chame [Invoke (methodName, args...) ](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.iclientproxy.invoke(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="3313d-311">If you want to invoke a client method by using a string variable as the method name, cast `Clients.All` (or `Clients.Others`, `Clients.Caller`, etc.) to `IClientProxy` and then call [Invoke(methodName, args...)](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.iclientproxy.invoke(v=vs.111).aspx).</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample42.cs)]

<a id="groupsfromhub"></a>

## <a name="how-to-manage-group-membership-from-the-hub-class"></a><span data-ttu-id="3313d-312">Como gerenciar a associação de grupo da classe de Hub</span><span class="sxs-lookup"><span data-stu-id="3313d-312">How to manage group membership from the Hub class</span></span>

<span data-ttu-id="3313d-313">Grupos no SignalR fornecem um método para mensagens de difusão de subconjuntos especificados de clientes conectados.</span><span class="sxs-lookup"><span data-stu-id="3313d-313">Groups in SignalR provide a method for broadcasting messages to specified subsets of connected clients.</span></span> <span data-ttu-id="3313d-314">Um grupo pode ter qualquer número de clientes e um cliente pode ser um membro de qualquer número de grupos.</span><span class="sxs-lookup"><span data-stu-id="3313d-314">A group can have any number of clients, and a client can be a member of any number of groups.</span></span>

<span data-ttu-id="3313d-315">Para gerenciar a associação de grupo, use o [adicionar](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) e [remover](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) métodos fornecidos pelo `Groups` propriedade da classe Hub.</span><span class="sxs-lookup"><span data-stu-id="3313d-315">To manage group membership, use the [Add](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) and [Remove](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) methods provided by the `Groups` property of the Hub class.</span></span> <span data-ttu-id="3313d-316">A exemplo a seguir mostra o `Groups.Add` e `Groups.Remove` métodos usados em métodos de Hub que são chamados pelo código do cliente, seguido do código de cliente JavaScript que o chama.</span><span class="sxs-lookup"><span data-stu-id="3313d-316">The following example shows the `Groups.Add` and `Groups.Remove` methods used in Hub methods that are called by client code, followed by JavaScript client code that calls them.</span></span>

<span data-ttu-id="3313d-317">**Servidor**</span><span class="sxs-lookup"><span data-stu-id="3313d-317">**Server**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample43.cs?highlight=5,10)]

<span data-ttu-id="3313d-318">**Cliente JavaScript usando o proxy gerado**</span><span class="sxs-lookup"><span data-stu-id="3313d-318">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample44.js)]

[!code-javascript[Main](hubs-api-guide-server/samples/sample45.js)]

<span data-ttu-id="3313d-319">Você não precisa criar explicitamente grupos.</span><span class="sxs-lookup"><span data-stu-id="3313d-319">You don't have to explicitly create groups.</span></span> <span data-ttu-id="3313d-320">Na verdade um grupo é criado automaticamente na primeira vez que você especifique seu nome em uma chamada para `Groups.Add`, e ela será excluída quando você remover a última conexão de associação nele.</span><span class="sxs-lookup"><span data-stu-id="3313d-320">In effect a group is automatically created the first time you specify its name in a call to `Groups.Add`, and it is deleted when you remove the last connection from membership in it.</span></span>

<span data-ttu-id="3313d-321">Há uma API para obter uma lista de associação de grupo ou uma lista de grupos.</span><span class="sxs-lookup"><span data-stu-id="3313d-321">There is no API for getting a group membership list or a list of groups.</span></span> <span data-ttu-id="3313d-322">SignalR envia mensagens para os clientes e grupos com base em um [modelo pub/sub](http://en.wikipedia.org/wiki/Publish/subscribe), e o servidor não mantém a lista de grupos ou associações de grupo.</span><span class="sxs-lookup"><span data-stu-id="3313d-322">SignalR sends messages to clients and groups based on a [pub/sub model](http://en.wikipedia.org/wiki/Publish/subscribe), and the server does not maintain lists of groups or group memberships.</span></span> <span data-ttu-id="3313d-323">Isso ajuda a maximizar a escalabilidade, porque sempre que você adicionar um nó a uma web farm, qualquer estado que mantém o SignalR tem sejam propagadas para o novo nó.</span><span class="sxs-lookup"><span data-stu-id="3313d-323">This helps maximize scalability, because whenever you add a node to a web farm, any state that SignalR maintains has to be propagated to the new node.</span></span>

<a id="asyncgroupmethods"></a>

### <a name="asynchronous-execution-of-add-and-remove-methods"></a><span data-ttu-id="3313d-324">Execução assíncrona de métodos Add e Remove</span><span class="sxs-lookup"><span data-stu-id="3313d-324">Asynchronous execution of Add and Remove methods</span></span>

<span data-ttu-id="3313d-325">O `Groups.Add` e `Groups.Remove` métodos execute de forma assíncrona.</span><span class="sxs-lookup"><span data-stu-id="3313d-325">The `Groups.Add` and `Groups.Remove` methods execute asynchronously.</span></span> <span data-ttu-id="3313d-326">Se você quiser adicionar um cliente a um grupo e enviar imediatamente uma mensagem para o cliente usando o grupo, você precisa garantir que o `Groups.Add` método terminar primeiro.</span><span class="sxs-lookup"><span data-stu-id="3313d-326">If you want to add a client to a group and immediately send a message to the client by using the group, you have to make sure that the `Groups.Add` method finishes first.</span></span> <span data-ttu-id="3313d-327">O exemplo de código a seguir mostra como fazer isso.</span><span class="sxs-lookup"><span data-stu-id="3313d-327">The following code example shows how to do that.</span></span>

<span data-ttu-id="3313d-328">**Adicionar um cliente a um grupo e, em seguida, esse cliente de mensagens**</span><span class="sxs-lookup"><span data-stu-id="3313d-328">**Adding a client to a group and then messaging that client**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample46.cs?highlight=1,3)]

<a id="grouppersistence"></a>

### <a name="group-membership-persistence"></a><span data-ttu-id="3313d-329">Persistência de associação de grupo</span><span class="sxs-lookup"><span data-stu-id="3313d-329">Group membership persistence</span></span>

<span data-ttu-id="3313d-330">SignalR rastreia conexões, não aos usuários, portanto, se você deseja que um usuário esteja no mesmo grupo de toda vez que o usuário estabelece uma conexão, você precisa chamar `Groups.Add` toda vez que o usuário estabelece uma nova conexão.</span><span class="sxs-lookup"><span data-stu-id="3313d-330">SignalR tracks connections, not users, so if you want a user to be in the same group every time the user establishes a connection, you have to call `Groups.Add` every time the user establishes a new connection.</span></span>

<span data-ttu-id="3313d-331">Depois de uma perda temporária de conectividade, às vezes SignalR pode restaurar a conexão automaticamente.</span><span class="sxs-lookup"><span data-stu-id="3313d-331">After a temporary loss of connectivity, sometimes SignalR can restore the connection automatically.</span></span> <span data-ttu-id="3313d-332">Nesse caso, SignalR está restaurando a mesma conexão, sem estabelecer uma nova conexão e então a associação de grupo do cliente é automaticamente restaurada.</span><span class="sxs-lookup"><span data-stu-id="3313d-332">In that case, SignalR is restoring the same connection, not establishing a new connection, and so the client's group membership is automatically restored.</span></span> <span data-ttu-id="3313d-333">Isso é possível até mesmo quando a interrupção temporária é o resultado de uma reinicialização do servidor ou a falha, porque o estado de conexão para cada cliente, incluindo associações de grupo, é recuperado para o cliente.</span><span class="sxs-lookup"><span data-stu-id="3313d-333">This is possible even when the temporary break is the result of a server reboot or failure, because connection state for each client, including group memberships, is round-tripped to the client.</span></span> <span data-ttu-id="3313d-334">Se um servidor falhar e for substituído por um novo servidor antes de atingir o tempo limite de conexão, um cliente pode se reconectar automaticamente ao novo servidor e registrar novamente em grupos que ele é um membro de.</span><span class="sxs-lookup"><span data-stu-id="3313d-334">If a server goes down and is replaced by a new server before the connection times out, a client can reconnect automatically to the new server and re-enroll in groups it is a member of.</span></span>

<span data-ttu-id="3313d-335">Quando uma conexão não pode ser restaurado automaticamente depois de uma perda de conectividade, ou quando a conexão for interrompida ou quando o cliente se desconecta (por exemplo, quando um navegador navega para uma nova página), associações de grupo são perdidas.</span><span class="sxs-lookup"><span data-stu-id="3313d-335">When a connection can't be restored automatically after a loss of connectivity, or when the connection times out, or when the client disconnects (for example, when a browser navigates to a new page), group memberships are lost.</span></span> <span data-ttu-id="3313d-336">Na próxima vez que o usuário se conecta será uma nova conexão.</span><span class="sxs-lookup"><span data-stu-id="3313d-336">The next time the user connects will be a new connection.</span></span> <span data-ttu-id="3313d-337">Para manter as associações de grupo quando o mesmo usuário estabelece uma nova conexão, seu aplicativo tiver de controlar as associações entre usuários e grupos e restaurar associações de grupo de cada vez que um usuário estabelece uma nova conexão.</span><span class="sxs-lookup"><span data-stu-id="3313d-337">To maintain group memberships when the same user establishes a new connection, your application has to track the associations between users and groups, and restore group memberships each time a user establishes a new connection.</span></span>

<span data-ttu-id="3313d-338">Para obter mais informações sobre as conexões e reconexões, consulte [como manipular eventos de tempo de vida da conexão na classe Hub](#connectionlifetime) mais adiante neste tópico.</span><span class="sxs-lookup"><span data-stu-id="3313d-338">For more information about connections and reconnections, see [How to handle connection lifetime events in the Hub class](#connectionlifetime) later in this topic.</span></span>

<a id="singleusergroups"></a>

### <a name="single-user-groups"></a><span data-ttu-id="3313d-339">Grupos de usuário único</span><span class="sxs-lookup"><span data-stu-id="3313d-339">Single-user groups</span></span>

<span data-ttu-id="3313d-340">Aplicativos que usam o SignalR normalmente têm que controlar as associações entre usuários e conexões para saber qual usuário enviou uma mensagem e quais usuários devem receber uma mensagem.</span><span class="sxs-lookup"><span data-stu-id="3313d-340">Applications that use SignalR typically have to keep track of the associations between users and connections in order to know which user has sent a message and which user(s) should be receiving a message.</span></span> <span data-ttu-id="3313d-341">Grupos são usados em um dos dois padrões de usados geral para fazer isso.</span><span class="sxs-lookup"><span data-stu-id="3313d-341">Groups are used in one of the two commonly used patterns for doing that.</span></span>

- <span data-ttu-id="3313d-342">Grupos de usuário único.</span><span class="sxs-lookup"><span data-stu-id="3313d-342">Single-user groups.</span></span>

    <span data-ttu-id="3313d-343">Você pode especificar o nome de usuário como o nome do grupo e adicione a ID de conexão atual ao grupo toda vez que o usuário se conecta ou se reconectar.</span><span class="sxs-lookup"><span data-stu-id="3313d-343">You can specify the user name as the group name, and add the current connection ID to the group every time the user connects or reconnects.</span></span> <span data-ttu-id="3313d-344">Para enviar mensagens para o usuário que você envia para o grupo.</span><span class="sxs-lookup"><span data-stu-id="3313d-344">To send messages to the user you send to the group.</span></span> <span data-ttu-id="3313d-345">Uma desvantagem desse método é que o grupo não oferece a você uma maneira de descobrir se o usuário está online ou offline.</span><span class="sxs-lookup"><span data-stu-id="3313d-345">A disadvantage of this method is that the group doesn't provide you with a way to find out if the user is online or offline.</span></span>
- <span data-ttu-id="3313d-346">Controlar as associações entre nomes de usuário e IDs de conexão.</span><span class="sxs-lookup"><span data-stu-id="3313d-346">Track associations between user names and connection IDs.</span></span>

    <span data-ttu-id="3313d-347">Você pode armazenar uma associação entre cada nome de usuário e uma ou mais IDs de conexão em um dicionário ou o banco de dados e atualizar os dados armazenados sempre que o usuário se conecta ou desconecta.</span><span class="sxs-lookup"><span data-stu-id="3313d-347">You can store an association between each user name and one or more connection IDs in a dictionary or database, and update the stored data each time the user connects or disconnects.</span></span> <span data-ttu-id="3313d-348">Para enviar mensagens para o usuário, você especifica a IDs de conexão.</span><span class="sxs-lookup"><span data-stu-id="3313d-348">To send messages to the user you specify the connection IDs.</span></span> <span data-ttu-id="3313d-349">Uma desvantagem desse método é que ele usa mais memória.</span><span class="sxs-lookup"><span data-stu-id="3313d-349">A disadvantage of this method is that it takes more memory.</span></span>

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events-in-the-hub-class"></a><span data-ttu-id="3313d-350">Como manipular eventos de tempo de vida da conexão na classe Hub</span><span class="sxs-lookup"><span data-stu-id="3313d-350">How to handle connection lifetime events in the Hub class</span></span>

<span data-ttu-id="3313d-351">As razões comuns de manipulação de eventos de tempo de vida da conexão são para controlar se um usuário estiver conectado ou não e para controlar a associação entre os nomes de usuário e IDs de conexão.</span><span class="sxs-lookup"><span data-stu-id="3313d-351">Typical reasons for handling connection lifetime events are to keep track of whether a user is connected or not, and to keep track of the association between user names and connection IDs.</span></span> <span data-ttu-id="3313d-352">Para executar seu próprio código, quando os clientes se conectar ou desconectarem, substituir o `OnConnected`, `OnDisconnected`, e `OnReconnected` métodos virtuais do Hub de classe, conforme mostrado no exemplo a seguir.</span><span class="sxs-lookup"><span data-stu-id="3313d-352">To run your own code when clients connect or disconnect, override the `OnConnected`, `OnDisconnected`, and `OnReconnected` virtual methods of the Hub class, as shown in the following example.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample47.cs?highlight=3,14,22)]

<a id="onreconnected"></a>

### <a name="when-onconnected-ondisconnected-and-onreconnected-are-called"></a><span data-ttu-id="3313d-353">Quando são chamados OnConnected, OnDisconnected e OnReconnected</span><span class="sxs-lookup"><span data-stu-id="3313d-353">When OnConnected, OnDisconnected, and OnReconnected are called</span></span>

<span data-ttu-id="3313d-354">Cada vez que um navegador navega para uma nova página, uma nova conexão deve ser estabelecido, o que significa SignalR executará o `OnDisconnected` método seguido de `OnConnected` método.</span><span class="sxs-lookup"><span data-stu-id="3313d-354">Each time a browser navigates to a new page, a new connection has to be established, which means SignalR will execute the `OnDisconnected` method followed by the `OnConnected` method.</span></span> <span data-ttu-id="3313d-355">SignalR sempre cria uma nova ID de conexão quando uma nova conexão é estabelecida.</span><span class="sxs-lookup"><span data-stu-id="3313d-355">SignalR always creates a new connection ID when a new connection is established.</span></span>

<span data-ttu-id="3313d-356">O `OnReconnected` método é chamado quando houve uma interrupção temporária de conectividade SignalR pode se recuperar automaticamente, como quando um cabo está temporariamente desconectado e reconectado antes que a conexão expire. O `OnDisconnected` método é chamado quando o cliente for desconectado e SignalR automaticamente não é possível reconectar, como quando um navegador navega para uma nova página.</span><span class="sxs-lookup"><span data-stu-id="3313d-356">The `OnReconnected` method is called when there has been a temporary break in connectivity that SignalR can automatically recover from, such as when a cable is temporarily disconnected and reconnected before the connection times out. The `OnDisconnected` method is called when the client is disconnected and SignalR can't automatically reconnect, such as when a browser navigates to a new page.</span></span> <span data-ttu-id="3313d-357">Portanto, é uma sequência possíveis de eventos para um determinado cliente `OnConnected`, `OnReconnected`, `OnDisconnected`; ou `OnConnected`, `OnDisconnected`.</span><span class="sxs-lookup"><span data-stu-id="3313d-357">Therefore, a possible sequence of events for a given client is `OnConnected`, `OnReconnected`, `OnDisconnected`; or `OnConnected`, `OnDisconnected`.</span></span> <span data-ttu-id="3313d-358">Você não verá a sequência `OnConnected`, `OnDisconnected`, `OnReconnected` para uma determinada conexão.</span><span class="sxs-lookup"><span data-stu-id="3313d-358">You won't see the sequence `OnConnected`, `OnDisconnected`, `OnReconnected` for a given connection.</span></span>

<span data-ttu-id="3313d-359">O `OnDisconnected` método não é chamado em alguns cenários, como quando um servidor fica inoperante ou o domínio de aplicativo é reciclado.</span><span class="sxs-lookup"><span data-stu-id="3313d-359">The `OnDisconnected` method doesn't get called in some scenarios, such as when a server goes down or the App Domain gets recycled.</span></span> <span data-ttu-id="3313d-360">Quando o outro servidor estiver online ou o domínio de aplicativo conclui sua Lixeira, alguns clientes poderá se reconectar e acionar o `OnReconnected` evento.</span><span class="sxs-lookup"><span data-stu-id="3313d-360">When another server comes on line or the App Domain completes its recycle, some clients may be able to reconnect and fire the `OnReconnected` event.</span></span>

<span data-ttu-id="3313d-361">Para obter mais informações, consulte [compreensão e tratamento de eventos de tempo de vida de Conexão no SignalR](handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="3313d-361">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](handling-connection-lifetime-events.md).</span></span>

<a id="nocallerstate"></a>

### <a name="caller-state-not-populated"></a><span data-ttu-id="3313d-362">Estado do chamador não preenchido</span><span class="sxs-lookup"><span data-stu-id="3313d-362">Caller state not populated</span></span>

<span data-ttu-id="3313d-363">São chamados os métodos de manipulador de eventos de tempo de vida de conexão do servidor, o que significa que qualquer estado que você colocou o `state` objeto no cliente não será preenchido no `Caller` propriedade no servidor.</span><span class="sxs-lookup"><span data-stu-id="3313d-363">The connection lifetime event handler methods are called from the server, which means that any state that you put in the `state` object on the client will not be populated in the `Caller` property on the server.</span></span> <span data-ttu-id="3313d-364">Para obter informações sobre o `state` objeto e o `Caller` propriedade, consulte [como passar o estado entre clientes e a classe de Hub](#passstate) mais adiante neste tópico.</span><span class="sxs-lookup"><span data-stu-id="3313d-364">For information about the `state` object and the `Caller` property, see [How to pass state between clients and the Hub class](#passstate) later in this topic.</span></span>

<a id="contextproperty"></a>

## <a name="how-to-get-information-about-the-client-from-the-context-property"></a><span data-ttu-id="3313d-365">Como obter informações sobre o cliente da propriedade de contexto</span><span class="sxs-lookup"><span data-stu-id="3313d-365">How to get information about the client from the Context property</span></span>

<span data-ttu-id="3313d-366">Para obter informações sobre o cliente, use o `Context` propriedade da classe Hub.</span><span class="sxs-lookup"><span data-stu-id="3313d-366">To get information about the client, use the `Context` property of the Hub class.</span></span> <span data-ttu-id="3313d-367">O `Context` propriedade retorna um [HubCallerContext](https://msdn.microsoft.com/library/jj890883(v=vs.111).aspx) objeto que fornece acesso às seguintes informações:</span><span class="sxs-lookup"><span data-stu-id="3313d-367">The `Context` property returns a [HubCallerContext](https://msdn.microsoft.com/library/jj890883(v=vs.111).aspx) object which provides access to the following information:</span></span>

- <span data-ttu-id="3313d-368">A ID de conexão do cliente da chamada.</span><span class="sxs-lookup"><span data-stu-id="3313d-368">The connection ID of the calling client.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample48.cs?highlight=1)]

    <span data-ttu-id="3313d-369">A ID de conexão é um GUID que é atribuído pelo SignalR (você não pode especificar o valor em seu próprio código).</span><span class="sxs-lookup"><span data-stu-id="3313d-369">The connection ID is a GUID that is assigned by SignalR (you can't specify the value in your own code).</span></span> <span data-ttu-id="3313d-370">Há uma ID de conexão para cada conexão e a mesma conexão que ID é usada por todos os Hubs, se você tiver vários Hubs em seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="3313d-370">There is one connection ID for each connection, and the same connection ID is used by all Hubs if you have multiple Hubs in your application.</span></span>
- <span data-ttu-id="3313d-371">Dados de cabeçalho HTTP.</span><span class="sxs-lookup"><span data-stu-id="3313d-371">HTTP header data.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample49.cs?highlight=1)]

    <span data-ttu-id="3313d-372">Você também pode obter os cabeçalhos HTTP de `Context.Headers`.</span><span class="sxs-lookup"><span data-stu-id="3313d-372">You can also get HTTP headers from `Context.Headers`.</span></span> <span data-ttu-id="3313d-373">A razão para várias referências para a mesma coisa é que `Context.Headers` foi criado pela primeira vez, o `Context.Request` propriedade foi adicionada posteriormente, e `Context.Headers` foi mantido para compatibilidade com versões anteriores.</span><span class="sxs-lookup"><span data-stu-id="3313d-373">The reason for multiple references to the same thing is that `Context.Headers` was created first, the `Context.Request` property was added later, and `Context.Headers` was retained for backward compatibility.</span></span>
- <span data-ttu-id="3313d-374">Dados de cadeia de caracteres de consulta.</span><span class="sxs-lookup"><span data-stu-id="3313d-374">Query string data.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample50.cs?highlight=1)]

    <span data-ttu-id="3313d-375">Você também pode obter dados de cadeia de caracteres de consulta de `Context.QueryString`.</span><span class="sxs-lookup"><span data-stu-id="3313d-375">You can also get query string data from `Context.QueryString`.</span></span>

    <span data-ttu-id="3313d-376">A cadeia de caracteres de consulta que você obtém por esta propriedade é aquele que foi usado com a solicitação HTTP que estabelecer a conexão do SignalR.</span><span class="sxs-lookup"><span data-stu-id="3313d-376">The query string that you get in this property is the one that was used with the HTTP request that established the SignalR connection.</span></span> <span data-ttu-id="3313d-377">Você pode adicionar parâmetros de cadeia de caracteres de consulta no cliente por meio da configuração de conexão, que é uma maneira conveniente para passar dados sobre o cliente do cliente para o servidor.</span><span class="sxs-lookup"><span data-stu-id="3313d-377">You can add query string parameters in the client by configuring the connection, which is a convenient way to pass data about the client from the client to the server.</span></span> <span data-ttu-id="3313d-378">O exemplo a seguir mostra uma maneira de adicionar uma cadeia de caracteres de consulta em um cliente JavaScript ao usar o proxy gerado.</span><span class="sxs-lookup"><span data-stu-id="3313d-378">The following example shows one way to add a query string in a JavaScript client when you use the generated proxy.</span></span>

    [!code-javascript[Main](hubs-api-guide-server/samples/sample51.js?highlight=1)]

    <span data-ttu-id="3313d-379">Para obter mais informações sobre como definir parâmetros de cadeia de caracteres de consulta, consulte os guias de API para o [JavaScript](hubs-api-guide-javascript-client.md) e [.NET](hubs-api-guide-net-client.md) clientes.</span><span class="sxs-lookup"><span data-stu-id="3313d-379">For more information about setting query string parameters, see the API guides for the [JavaScript](hubs-api-guide-javascript-client.md) and [.NET](hubs-api-guide-net-client.md) clients.</span></span>

    <span data-ttu-id="3313d-380">Você pode encontrar o método de transporte usado para a conexão nos dados de cadeia de caracteres de consulta, juntamente com alguns outros valores usados internamente pelo SignalR:</span><span class="sxs-lookup"><span data-stu-id="3313d-380">You can find the transport method used for the connection in the query string data, along with some other values used internally by SignalR:</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample52.cs)]

    <span data-ttu-id="3313d-381">O valor de `transportMethod` será "webSockets", "serverSentEvents", "foreverFrame" ou "longPolling".</span><span class="sxs-lookup"><span data-stu-id="3313d-381">The value of `transportMethod` will be "webSockets", "serverSentEvents", "foreverFrame" or "longPolling".</span></span> <span data-ttu-id="3313d-382">Observe que, se você verificar esse valor `OnConnected` método do manipulador de eventos, em alguns cenários, você pode obter inicialmente um valor de transporte que não seja o método de transporte negociada final para a conexão.</span><span class="sxs-lookup"><span data-stu-id="3313d-382">Note that if you check this value in the `OnConnected` event handler method, in some scenarios you might initially get a transport value that is not the final negotiated transport method for the connection.</span></span> <span data-ttu-id="3313d-383">Nesse caso, o método lançará uma exceção e será chamado novamente mais tarde quando o método de transporte final é estabelecido.</span><span class="sxs-lookup"><span data-stu-id="3313d-383">In that case the method will throw an exception and will be called again later when the final transport method is established.</span></span>
- <span data-ttu-id="3313d-384">Cookies.</span><span class="sxs-lookup"><span data-stu-id="3313d-384">Cookies.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample53.cs?highlight=1)]

    <span data-ttu-id="3313d-385">Você também pode obter cookies do `Context.RequestCookies`.</span><span class="sxs-lookup"><span data-stu-id="3313d-385">You can also get cookies from `Context.RequestCookies`.</span></span>
- <span data-ttu-id="3313d-386">Informações do usuário.</span><span class="sxs-lookup"><span data-stu-id="3313d-386">User information.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample54.cs?highlight=1)]
- <span data-ttu-id="3313d-387">O objeto HttpContext para a solicitação:</span><span class="sxs-lookup"><span data-stu-id="3313d-387">The HttpContext object for the request :</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample55.cs?highlight=1)]

    <span data-ttu-id="3313d-388">Use esse método em vez de obter `HttpContext.Current` para obter o `HttpContext` objeto para a conexão do SignalR.</span><span class="sxs-lookup"><span data-stu-id="3313d-388">Use this method instead of getting `HttpContext.Current` to get the `HttpContext` object for the SignalR connection.</span></span>

<a id="passstate"></a>

## <a name="how-to-pass-state-between-clients-and-the-hub-class"></a><span data-ttu-id="3313d-389">Como passar o estado entre clientes e a classe de Hub</span><span class="sxs-lookup"><span data-stu-id="3313d-389">How to pass state between clients and the Hub class</span></span>

<span data-ttu-id="3313d-390">O proxy do cliente fornece um `state` objeto no qual você pode armazenar dados que você deseja ser transmitido para o servidor com cada chamada de método.</span><span class="sxs-lookup"><span data-stu-id="3313d-390">The client proxy provides a `state` object in which you can store data that you want to be transmitted to the server with each method call.</span></span> <span data-ttu-id="3313d-391">No servidor, você pode acessar esses dados no `Clients.Caller` propriedade nos métodos de Hub são chamados de clientes.</span><span class="sxs-lookup"><span data-stu-id="3313d-391">On the server you can access this data in the `Clients.Caller` property in Hub methods that are called by clients.</span></span> <span data-ttu-id="3313d-392">O `Clients.Caller` propriedade não é preenchida para os métodos de manipulador de eventos de tempo de vida de conexão `OnConnected`, `OnDisconnected`, e `OnReconnected`.</span><span class="sxs-lookup"><span data-stu-id="3313d-392">The `Clients.Caller` property is not populated for the connection lifetime event handler methods `OnConnected`, `OnDisconnected`, and `OnReconnected`.</span></span>

<span data-ttu-id="3313d-393">Criar ou atualizar dados no `state` objeto e o `Clients.Caller` propriedade funciona em ambas as direções.</span><span class="sxs-lookup"><span data-stu-id="3313d-393">Creating or updating data in the `state` object and the `Clients.Caller` property works in both directions.</span></span> <span data-ttu-id="3313d-394">Você pode atualizar os valores no servidor e eles são passados de volta ao cliente.</span><span class="sxs-lookup"><span data-stu-id="3313d-394">You can update values in the server and they are passed back to the client.</span></span>

<span data-ttu-id="3313d-395">O exemplo a seguir mostra o código de cliente JavaScript que armazena o estado de transmissão para o servidor com cada chamada de método.</span><span class="sxs-lookup"><span data-stu-id="3313d-395">The following example shows JavaScript client code that stores state for transmission to the server with every method call.</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample56.js?highlight=1-2)]

<span data-ttu-id="3313d-396">O exemplo a seguir mostra o código equivalente em um cliente .NET.</span><span class="sxs-lookup"><span data-stu-id="3313d-396">The following example shows the equivalent code in a .NET client.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample57.cs?highlight=1-2)]

<span data-ttu-id="3313d-397">Em sua classe de Hub, você pode acessar esses dados no `Clients.Caller` propriedade.</span><span class="sxs-lookup"><span data-stu-id="3313d-397">In your Hub class, you can access this data in the `Clients.Caller` property.</span></span> <span data-ttu-id="3313d-398">O exemplo a seguir mostra o código que recupera o estado conhecido no exemplo anterior.</span><span class="sxs-lookup"><span data-stu-id="3313d-398">The following example shows code that retrieves the state referred to in the previous example.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample58.cs?highlight=3-4)]

> [!NOTE]
> <span data-ttu-id="3313d-399">Esse mecanismo para persistir o estado não é destinado para grandes quantidades de dados, pois tudo o que você colocar o `state` ou `Clients.Caller` propriedade é recuperado com cada invocação de método.</span><span class="sxs-lookup"><span data-stu-id="3313d-399">This mechanism for persisting state is not intended for large amounts of data, since everything you put in the `state` or `Clients.Caller` property is round-tripped with every method invocation.</span></span> <span data-ttu-id="3313d-400">É útil para itens menores, como nomes de usuário ou contadores.</span><span class="sxs-lookup"><span data-stu-id="3313d-400">It's useful for smaller items such as user names or counters.</span></span>


<span data-ttu-id="3313d-401">No VB.NET ou em um hub fortemente tipados, o objeto de estado do chamador não pode ser acessado por meio de `Clients.Caller`; em vez disso, use `Clients.CallerState` (introduzido no SignalR 2.1):</span><span class="sxs-lookup"><span data-stu-id="3313d-401">In VB.NET or in a strongly-typed hub, the caller state object can't be accessed through `Clients.Caller`; instead, use `Clients.CallerState` (introduced in SignalR 2.1):</span></span>

<span data-ttu-id="3313d-402">**Usando CallerState em c#**</span><span class="sxs-lookup"><span data-stu-id="3313d-402">**Using CallerState in C#**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample59.cs?highlight=3-4)]

<span data-ttu-id="3313d-403">**Usando CallerState no Visual Basic**</span><span class="sxs-lookup"><span data-stu-id="3313d-403">**Using CallerState in Visual Basic**</span></span>

[!code-vb[Main](hubs-api-guide-server/samples/sample60.vb)]

<a id="handleErrors"></a>

## <a name="how-to-handle-errors-in-the-hub-class"></a><span data-ttu-id="3313d-404">Como tratar erros na classe Hub</span><span class="sxs-lookup"><span data-stu-id="3313d-404">How to handle errors in the Hub class</span></span>

<span data-ttu-id="3313d-405">Para manipular erros que ocorrem em seus métodos de classe de Hub, use um ou mais dos seguintes métodos:</span><span class="sxs-lookup"><span data-stu-id="3313d-405">To handle errors that occur in your Hub class methods, use one or more of the following methods:</span></span>

- <span data-ttu-id="3313d-406">Encapsular o código do método em blocos try-catch e registrar o objeto de exceção.</span><span class="sxs-lookup"><span data-stu-id="3313d-406">Wrap your method code in try-catch blocks and log the exception object.</span></span> <span data-ttu-id="3313d-407">Para fins de depuração, você pode enviar a exceção para o cliente, mas para segurança motivos enviando informações detalhadas para clientes de produção não são recomendados.</span><span class="sxs-lookup"><span data-stu-id="3313d-407">For debugging purposes you can send the exception to the client, but for security reasons sending detailed information to clients in production is not recommended.</span></span>
- <span data-ttu-id="3313d-408">Criar um módulo de pipeline de Hubs que manipula o [OnIncomingError](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubpipelinemodule.onincomingerror(v=vs.111).aspx) método.</span><span class="sxs-lookup"><span data-stu-id="3313d-408">Create a Hubs pipeline module that handles the [OnIncomingError](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubpipelinemodule.onincomingerror(v=vs.111).aspx) method.</span></span> <span data-ttu-id="3313d-409">O exemplo a seguir mostra um módulo de pipeline que registra erros, seguidos do código em Startup.cs que insere o módulo no pipeline de Hubs.</span><span class="sxs-lookup"><span data-stu-id="3313d-409">The following example shows a pipeline module that logs errors, followed by code in Startup.cs that injects the module into the Hubs pipeline.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample61.cs)]

    [!code-csharp[Main](hubs-api-guide-server/samples/sample62.cs?highlight=4)]
- <span data-ttu-id="3313d-410">Use o `HubException` classe (introduzido no SignalR 2).</span><span class="sxs-lookup"><span data-stu-id="3313d-410">Use the `HubException` class (introduced in SignalR 2).</span></span> <span data-ttu-id="3313d-411">Esse erro pode ser gerado de qualquer invocação de hub.</span><span class="sxs-lookup"><span data-stu-id="3313d-411">This error can be thrown from any hub invocation.</span></span> <span data-ttu-id="3313d-412">O `HubError` construtor recebe uma mensagem de cadeia de caracteres e um objeto para armazenar dados de erro extra.</span><span class="sxs-lookup"><span data-stu-id="3313d-412">The `HubError` constructor takes a string message, and an object for storing extra error data.</span></span> <span data-ttu-id="3313d-413">SignalR irá serializar automaticamente a exceção e enviá-lo para o cliente, onde ele será usado para rejeitar ou não a invocação de método do hub.</span><span class="sxs-lookup"><span data-stu-id="3313d-413">SignalR will auto-serialize the exception and send it to the client, where it will be used to reject or fail the hub method invocation.</span></span>

    <span data-ttu-id="3313d-414">Os exemplos de código a seguir demonstram como gerar um `HubException` durante uma invocação de Hub e como lidar com a exceção em clientes JavaScript e .NET.</span><span class="sxs-lookup"><span data-stu-id="3313d-414">The following code samples demonstrate how to throw a `HubException` during a Hub invocation, and how to handle the exception on JavaScript and .NET clients.</span></span>

    <span data-ttu-id="3313d-415">**Código do servidor demonstrando a classe HubException**</span><span class="sxs-lookup"><span data-stu-id="3313d-415">**Server code demonstrating the HubException class**</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample63.cs)]

    <span data-ttu-id="3313d-416">**Código de cliente JavaScript demonstrando a resposta para gerar um HubException em um hub**</span><span class="sxs-lookup"><span data-stu-id="3313d-416">**JavaScript client code demonstrating response to throwing a HubException in a hub**</span></span>

    [!code-html[Main](hubs-api-guide-server/samples/sample64.html)]

    <span data-ttu-id="3313d-417">**Código de cliente .NET demonstrando a resposta para gerar um HubException em um hub**</span><span class="sxs-lookup"><span data-stu-id="3313d-417">**.NET client code demonstrating response to throwing a HubException in a hub**</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample65.cs)]

<span data-ttu-id="3313d-418">Para obter mais informações sobre os módulos de pipeline do Hub, consulte [como personalizar o pipeline de Hubs](#hubpipeline) mais adiante neste tópico.</span><span class="sxs-lookup"><span data-stu-id="3313d-418">For more information about Hub pipeline modules, see [How to customize the Hubs pipeline](#hubpipeline) later in this topic.</span></span>

<a id="tracing"></a>

## <a name="how-to-enable-tracing"></a><span data-ttu-id="3313d-419">Como habilitar o rastreamento</span><span class="sxs-lookup"><span data-stu-id="3313d-419">How to enable tracing</span></span>

<span data-ttu-id="3313d-420">Para habilitar o rastreamento do lado do servidor, adicione um elemento de System. Diagnostics ao seu arquivo Web. config, conforme mostrado neste exemplo:</span><span class="sxs-lookup"><span data-stu-id="3313d-420">To enable server-side tracing, add a system.diagnostics element to your Web.config file, as shown in this example:</span></span>

[!code-html[Main](hubs-api-guide-server/samples/sample66.html?highlight=17-72)]

<span data-ttu-id="3313d-421">Quando você executa o aplicativo no Visual Studio, você pode exibir os logs de **saída** janela.</span><span class="sxs-lookup"><span data-stu-id="3313d-421">When you run the application in Visual Studio, you can view the logs in the **Output** window.</span></span>

<a id="callfromoutsidehub"></a>

## <a name="how-to-call-client-methods-and-manage-groups-from-outside-the-hub-class"></a><span data-ttu-id="3313d-422">Como chamar métodos de cliente e gerenciar grupos de fora da classe de Hub</span><span class="sxs-lookup"><span data-stu-id="3313d-422">How to call client methods and manage groups from outside the Hub class</span></span>

<span data-ttu-id="3313d-423">Para chamar métodos de cliente de uma classe diferente que a sua classe de Hub, obtenha uma referência para o objeto de contexto SignalR para o Hub e usá-lo para chamar métodos no cliente ou gerenciar grupos.</span><span class="sxs-lookup"><span data-stu-id="3313d-423">To call client methods from a different class than your Hub class, get a reference to the SignalR context object for the Hub and use that to call methods on the client or manage groups.</span></span>

<span data-ttu-id="3313d-424">O exemplo a seguir `StockTicker` classe obtém o objeto de contexto, armazena em uma instância da classe, armazena a instância da classe em uma propriedade estática e usa o contexto da instância de classe singleton para chamar o `updateStockPrice` método clientes conectado a um Hub denominado `StockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="3313d-424">The following sample `StockTicker` class gets the context object, stores it in an instance of the class, stores the class instance in a static property, and uses the context from the singleton class instance to call the `updateStockPrice` method on clients that are connected to a Hub named `StockTickerHub`.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample67.cs?highlight=8,24)]

<span data-ttu-id="3313d-425">Se você precisar usar os contexto várias vezes em um objeto de vida útil longa, obter a referência de uma vez e salvá-lo em vez de fazê-lo novamente cada vez.</span><span class="sxs-lookup"><span data-stu-id="3313d-425">If you need to use the context multiple-times in a long-lived object, get the reference once and save it rather than getting it again each time.</span></span> <span data-ttu-id="3313d-426">Obtendo o contexto de uma vez, você garante que o SignalR envia mensagens para os clientes na mesma sequência em que os métodos de Hub feitas cliente invocações de método.</span><span class="sxs-lookup"><span data-stu-id="3313d-426">Getting the context once ensures that SignalR sends messages to clients in the same sequence in which your Hub methods make client method invocations.</span></span> <span data-ttu-id="3313d-427">Para obter um tutorial que mostra como usar o contexto de SignalR para um Hub, consulte [de difusão de servidor com ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="3313d-427">For a tutorial that shows how to use the SignalR context for a Hub, see [Server Broadcast with ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).</span></span>

<a id="callingclientsoutsidehub"></a>

### <a name="calling-client-methods"></a><span data-ttu-id="3313d-428">Chamando métodos do cliente</span><span class="sxs-lookup"><span data-stu-id="3313d-428">Calling client methods</span></span>

<span data-ttu-id="3313d-429">Você pode especificar quais clientes receberão o RPC, mas você tem menos opções que quando você chama a partir de uma classe de Hub.</span><span class="sxs-lookup"><span data-stu-id="3313d-429">You can specify which clients will receive the RPC, but you have fewer options than when you call from a Hub class.</span></span> <span data-ttu-id="3313d-430">A razão para isso é que o contexto não está associado uma determinada chamada de um cliente para todos os métodos que exigem o conhecimento da ID de conexão atual, como `Clients.Others`, ou `Clients.Caller`, ou `Clients.OthersInGroup`, não estão disponíveis.</span><span class="sxs-lookup"><span data-stu-id="3313d-430">The reason for this is that the context is not associated with a particular call from a client, so any methods that require knowledge of the current connection ID, such as `Clients.Others`, or `Clients.Caller`, or `Clients.OthersInGroup`, are not available.</span></span> <span data-ttu-id="3313d-431">As seguintes opções estão disponíveis:</span><span class="sxs-lookup"><span data-stu-id="3313d-431">The following options are available:</span></span>

- <span data-ttu-id="3313d-432">Todos os clientes conectados.</span><span class="sxs-lookup"><span data-stu-id="3313d-432">All connected clients.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample68.cs)]
- <span data-ttu-id="3313d-433">Um cliente específico identificado pela ID de conexão.</span><span class="sxs-lookup"><span data-stu-id="3313d-433">A specific client identified by connection ID.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample69.css)]
- <span data-ttu-id="3313d-434">Todos os clientes conectados, exceto clientes especificados, identificados pela ID de conexão.</span><span class="sxs-lookup"><span data-stu-id="3313d-434">All connected clients except the specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample70.cs)]
- <span data-ttu-id="3313d-435">Todos os clientes em um grupo especificado.</span><span class="sxs-lookup"><span data-stu-id="3313d-435">All connected clients in a specified group.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample71.css)]
- <span data-ttu-id="3313d-436">Todos os clientes em um grupo especificado, exceto clientes especificados, identificado pela ID de conexão.</span><span class="sxs-lookup"><span data-stu-id="3313d-436">All connected clients in a specified group except specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample72.cs)]

<span data-ttu-id="3313d-437">Se você estiver chamando em sua classe de Hub não dos métodos na classe Hub, você pode passar a ID de conexão atual e usá-lo com `Clients.Client`, `Clients.AllExcept`, ou `Clients.Group` para simular `Clients.Caller`, `Clients.Others`, ou `Clients.OthersInGroup`.</span><span class="sxs-lookup"><span data-stu-id="3313d-437">If you are calling into your non-Hub class from methods in your Hub class, you can pass in the current connection ID and use that with `Clients.Client`, `Clients.AllExcept`, or `Clients.Group` to simulate `Clients.Caller`, `Clients.Others`, or `Clients.OthersInGroup`.</span></span> <span data-ttu-id="3313d-438">No exemplo a seguir, o `MoveShapeHub` classe passa a ID da conexão para o `Broadcaster` classe para que o `Broadcaster` classe pode simular `Clients.Others`.</span><span class="sxs-lookup"><span data-stu-id="3313d-438">In the following example, the `MoveShapeHub` class passes the connection ID to the `Broadcaster` class so that the `Broadcaster` class can simulate `Clients.Others`.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample73.cs?highlight=12,36)]

<a id="managinggroupsoutsidehub"></a>

### <a name="managing-group-membership"></a><span data-ttu-id="3313d-439">Gerenciar associação de grupo</span><span class="sxs-lookup"><span data-stu-id="3313d-439">Managing group membership</span></span>

<span data-ttu-id="3313d-440">Para gerenciar os grupos, você tem as mesmas opções de como você faria em uma classe de Hub.</span><span class="sxs-lookup"><span data-stu-id="3313d-440">For managing groups you have the same options as you do in a Hub class.</span></span>

- <span data-ttu-id="3313d-441">Adicionar um cliente a um grupo</span><span class="sxs-lookup"><span data-stu-id="3313d-441">Add a client to a group</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample74.cs)]
- <span data-ttu-id="3313d-442">Remover um cliente de um grupo</span><span class="sxs-lookup"><span data-stu-id="3313d-442">Remove a client from a group</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample75.css)]

<a id="hubpipeline"></a>

## <a name="how-to-customize-the-hubs-pipeline"></a><span data-ttu-id="3313d-443">Como personalizar o pipeline de Hubs</span><span class="sxs-lookup"><span data-stu-id="3313d-443">How to customize the Hubs pipeline</span></span>

<span data-ttu-id="3313d-444">SignalR permite que você inserir seu próprio código na pipeline do Hub.</span><span class="sxs-lookup"><span data-stu-id="3313d-444">SignalR enables you to inject your own code into the Hub pipeline.</span></span> <span data-ttu-id="3313d-445">O exemplo a seguir mostra um módulo personalizado de pipeline de Hub que registra cada chamada de método de entrada recebida do cliente e saída chamada do método invocado no cliente:</span><span class="sxs-lookup"><span data-stu-id="3313d-445">The following example shows a custom Hub pipeline module that logs each incoming method call received from the client and outgoing method call invoked on the client:</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample76.cs)]

<span data-ttu-id="3313d-446">O código a seguir no *Startup.cs* arquivo registra o módulo para executar o pipeline de Hub:</span><span class="sxs-lookup"><span data-stu-id="3313d-446">The following code in the *Startup.cs* file registers the module to run in the Hub pipeline:</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample77.cs?highlight=3)]

<span data-ttu-id="3313d-447">Há vários métodos diferentes que você pode substituir.</span><span class="sxs-lookup"><span data-stu-id="3313d-447">There are many different methods that you can override.</span></span> <span data-ttu-id="3313d-448">Para obter uma lista completa, consulte [HubPipelineModule métodos](https://msdn.microsoft.com/library/jj918633(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="3313d-448">For a complete list, see [HubPipelineModule Methods](https://msdn.microsoft.com/library/jj918633(v=vs.111).aspx).</span></span>

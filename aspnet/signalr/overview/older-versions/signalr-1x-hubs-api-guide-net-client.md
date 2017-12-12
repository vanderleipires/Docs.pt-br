---
uid: signalr/overview/older-versions/signalr-1x-hubs-api-guide-net-client
title: Guia de API de Hubs de SignalR do ASP.NET - cliente .NET (SignalR 1. x) | Microsoft Docs
author: pfletcher
description: "Este documento fornece uma introdução ao uso da API de Hubs de SignalR versão 2 em clientes .NET, como o armazenamento do Windows (WinRT), WPF, Silverlight e contras..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/17/2013
ms.topic: article
ms.assetid: c334adc3-d6dc-44f3-9f06-f7634475aad3
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/signalr-1x-hubs-api-guide-net-client
msc.type: authoredcontent
ms.openlocfilehash: 9cf99ba7887e7db847097a63c0a964ef5d461a9d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-signalr-hubs-api-guide---net-client-signalr-1x"></a><span data-ttu-id="4dc4a-103">Guia de API de Hubs de SignalR do ASP.NET - cliente .NET (SignalR 1. x)</span><span class="sxs-lookup"><span data-stu-id="4dc4a-103">ASP.NET SignalR Hubs API Guide - .NET Client (SignalR 1.x)</span></span>
====================
<span data-ttu-id="4dc4a-104">por [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="4dc4a-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="4dc4a-105">Este documento fornece uma introdução ao uso da API de Hubs de SignalR versão 2 em clientes do .NET, como aplicativos de console, WPF, Silverlight e da Windows Store (WinRT).</span><span class="sxs-lookup"><span data-stu-id="4dc4a-105">This document provides an introduction to using the Hubs API for SignalR version 2 in .NET clients, such as Windows Store (WinRT), WPF, Silverlight, and console applications.</span></span>
> 
> <span data-ttu-id="4dc4a-106">A API de Hubs de SignalR permite fazer chamadas de procedimento remoto (RPCs) de um servidor para clientes conectados e de clientes para o servidor.</span><span class="sxs-lookup"><span data-stu-id="4dc4a-106">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="4dc4a-107">No código do servidor, você define métodos que podem ser chamados por clientes e chamar os métodos que são executados no cliente.</span><span class="sxs-lookup"><span data-stu-id="4dc4a-107">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="4dc4a-108">No código do cliente, você define métodos que podem ser chamados do servidor e chamar os métodos que são executados no servidor.</span><span class="sxs-lookup"><span data-stu-id="4dc4a-108">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="4dc4a-109">SignalR cuida de todos os detalhes do cliente para servidor para você.</span><span class="sxs-lookup"><span data-stu-id="4dc4a-109">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
> 
> <span data-ttu-id="4dc4a-110">O SignalR também oferece uma API de nível inferior chamada conexões persistentes.</span><span class="sxs-lookup"><span data-stu-id="4dc4a-110">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="4dc4a-111">Para obter uma introdução para o SignalR, Hubs e conexões persistentes, ou para um tutorial que mostra como criar um aplicativo SignalR completo, consulte [SignalR - Introdução](../getting-started/index.md).</span><span class="sxs-lookup"><span data-stu-id="4dc4a-111">For an introduction to SignalR, Hubs, and Persistent Connections, or for a tutorial that shows how to build a complete SignalR application, see [SignalR - Getting Started](../getting-started/index.md).</span></span>


## <a name="overview"></a><span data-ttu-id="4dc4a-112">Visão Geral</span><span class="sxs-lookup"><span data-stu-id="4dc4a-112">Overview</span></span>

<span data-ttu-id="4dc4a-113">Este documento contém as seguintes seções:</span><span class="sxs-lookup"><span data-stu-id="4dc4a-113">This document contains the following sections:</span></span>

- [<span data-ttu-id="4dc4a-114">Instalação do cliente</span><span class="sxs-lookup"><span data-stu-id="4dc4a-114">Client Setup</span></span>](#clientsetup)
- [<span data-ttu-id="4dc4a-115">Como estabelecer uma conexão</span><span class="sxs-lookup"><span data-stu-id="4dc4a-115">How to establish a connection</span></span>](#establishconnection)

    - [<span data-ttu-id="4dc4a-116">Conexões entre domínios de clientes do Silverlight</span><span class="sxs-lookup"><span data-stu-id="4dc4a-116">Cross-domain connections from Silverlight clients</span></span>](#slcrossdomain)
- [<span data-ttu-id="4dc4a-117">Como configurar a conexão</span><span class="sxs-lookup"><span data-stu-id="4dc4a-117">How to configure the connection</span></span>](#configureconnection)

    - [<span data-ttu-id="4dc4a-118">Como definir o número máximo de conexões simultâneas em clientes do WPF</span><span class="sxs-lookup"><span data-stu-id="4dc4a-118">How to set the maximum number of concurrent connections in WPF clients</span></span>](#maxconnections)
    - [<span data-ttu-id="4dc4a-119">Como especificar parâmetros de cadeia de caracteres de consulta</span><span class="sxs-lookup"><span data-stu-id="4dc4a-119">How to specify query string parameters</span></span>](#querystring)
    - [<span data-ttu-id="4dc4a-120">Como especificar o método de transporte</span><span class="sxs-lookup"><span data-stu-id="4dc4a-120">How to specify the transport method</span></span>](#transport)
    - [<span data-ttu-id="4dc4a-121">Como especificar cabeçalhos HTTP</span><span class="sxs-lookup"><span data-stu-id="4dc4a-121">How to specify HTTP headers</span></span>](#httpheaders)
    - [<span data-ttu-id="4dc4a-122">Como especificar os certificados de cliente</span><span class="sxs-lookup"><span data-stu-id="4dc4a-122">How to specify client certificates</span></span>](#clientcertificate)
- [<span data-ttu-id="4dc4a-123">Como criar o proxy do Hub</span><span class="sxs-lookup"><span data-stu-id="4dc4a-123">How to create the Hub proxy</span></span>](#proxy)
- [<span data-ttu-id="4dc4a-124">Como definir métodos no cliente que o servidor pode chamar</span><span class="sxs-lookup"><span data-stu-id="4dc4a-124">How to define methods on the client that the server can call</span></span>](#callclient)

    - [<span data-ttu-id="4dc4a-125">Métodos sem parâmetros</span><span class="sxs-lookup"><span data-stu-id="4dc4a-125">Methods without parameters</span></span>](#clientmethodswithoutparms)
    - [<span data-ttu-id="4dc4a-126">Métodos com parâmetros, especificando os tipos de parâmetro</span><span class="sxs-lookup"><span data-stu-id="4dc4a-126">Methods with parameters, specifying parameter types</span></span>](#clientmethodswithparmtypes)
    - [<span data-ttu-id="4dc4a-127">Métodos com parâmetros, especificando objetos dinâmicos para os parâmetros</span><span class="sxs-lookup"><span data-stu-id="4dc4a-127">Methods with parameters, specifying dynamic objects for the parameters</span></span>](#clientmethodswithdynamparms)
    - [<span data-ttu-id="4dc4a-128">Como remover um manipulador</span><span class="sxs-lookup"><span data-stu-id="4dc4a-128">How to remove a handler</span></span>](#removehandler)
- [<span data-ttu-id="4dc4a-129">Como chamar métodos do servidor do cliente</span><span class="sxs-lookup"><span data-stu-id="4dc4a-129">How to call server methods from the client</span></span>](#callserver)
- [<span data-ttu-id="4dc4a-130">Como manipular eventos de tempo de vida da conexão</span><span class="sxs-lookup"><span data-stu-id="4dc4a-130">How to handle connection lifetime events</span></span>](#connectionlifetime)
- [<span data-ttu-id="4dc4a-131">Como tratar erros</span><span class="sxs-lookup"><span data-stu-id="4dc4a-131">How to handle errors</span></span>](#handleerrors)
- [<span data-ttu-id="4dc4a-132">Como habilitar o registro de cliente</span><span class="sxs-lookup"><span data-stu-id="4dc4a-132">How to enable client-side logging</span></span>](#logging)
- [<span data-ttu-id="4dc4a-133">Exemplos de código de aplicativo de console para métodos de cliente que o servidor pode chamar, Silverlight e WPF</span><span class="sxs-lookup"><span data-stu-id="4dc4a-133">WPF, Silverlight, and console application code samples for client methods that the server can call</span></span>](#wpfsl)

<span data-ttu-id="4dc4a-134">Um exemplo .NET para projetos de cliente, consulte os seguintes recursos:</span><span class="sxs-lookup"><span data-stu-id="4dc4a-134">For a sample .NET client projects, see the following resources:</span></span>

- <span data-ttu-id="4dc4a-135">[Gustavo armenta / SignalR exemplos](https://github.com/gustavo-armenta/SignalR-Samples) em GitHub.com (WinRT, Silverlight, exemplos de aplicativo de console).</span><span class="sxs-lookup"><span data-stu-id="4dc4a-135">[gustavo-armenta / SignalR-Samples](https://github.com/gustavo-armenta/SignalR-Samples) on GitHub.com (WinRT, Silverlight, console app examples).</span></span>
- <span data-ttu-id="4dc4a-136">[DamianEdwards / SignalR MoveShapeDemo / MoveShape.Desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) em GitHub.com (exemplo WPF).</span><span class="sxs-lookup"><span data-stu-id="4dc4a-136">[DamianEdwards / SignalR-MoveShapeDemo / MoveShape.Desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) on GitHub.com (WPF example).</span></span>
- <span data-ttu-id="4dc4a-137">[SignalR / Microsoft.AspNet.SignalR.Client.Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) em GitHub.com (exemplo de aplicativo de Console).</span><span class="sxs-lookup"><span data-stu-id="4dc4a-137">[SignalR / Microsoft.AspNet.SignalR.Client.Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) on GitHub.com (Console app example).</span></span>

<span data-ttu-id="4dc4a-138">Para obter a documentação sobre como programar o servidor ou os clientes JavaScript, consulte os seguintes recursos:</span><span class="sxs-lookup"><span data-stu-id="4dc4a-138">For documentation on how to program the server or JavaScript clients, see the following resources:</span></span>

- [<span data-ttu-id="4dc4a-139">Guia de API de Hubs de SignalR - servidor</span><span class="sxs-lookup"><span data-stu-id="4dc4a-139">SignalR Hubs API Guide - Server</span></span>](../guide-to-the-api/hubs-api-guide-server.md)
- [<span data-ttu-id="4dc4a-140">Guia de API de Hubs de SignalR - cliente JavaScript</span><span class="sxs-lookup"><span data-stu-id="4dc4a-140">SignalR Hubs API Guide - JavaScript Client</span></span>](../guide-to-the-api/hubs-api-guide-javascript-client.md)

<span data-ttu-id="4dc4a-141">Links para tópicos de referência de API são para a versão do .NET 4.5 da API.</span><span class="sxs-lookup"><span data-stu-id="4dc4a-141">Links to API Reference topics are to the .NET 4.5 version of the API.</span></span> <span data-ttu-id="4dc4a-142">Se você estiver usando o .NET 4, consulte [a versão do .NET 4 dos tópicos da API](https://msdn.microsoft.com/en-us/library/jj891075(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="4dc4a-142">If you're using .NET 4, see [the .NET 4 version of the API topics](https://msdn.microsoft.com/en-us/library/jj891075(v=vs.100).aspx).</span></span>

<a id="clientsetup"></a>

## <a name="client-setup"></a><span data-ttu-id="4dc4a-143">Instalação do cliente</span><span class="sxs-lookup"><span data-stu-id="4dc4a-143">Client setup</span></span>

<span data-ttu-id="4dc4a-144">Instalar o [SignalR](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) pacote do NuGet (não o [SignalR](http://nuget.org/packages/microsoft.aspnet.signalr) pacote).</span><span class="sxs-lookup"><span data-stu-id="4dc4a-144">Install the [Microsoft.AspNet.SignalR.Client](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) NuGet package (not the [Microsoft.AspNet.SignalR](http://nuget.org/packages/microsoft.aspnet.signalr) package).</span></span> <span data-ttu-id="4dc4a-145">Este pacote suporta WinRT, Silverlight, WPF, aplicativo de console e os clientes do Windows Phone, para .NET 4 e o .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="4dc4a-145">This package supports WinRT, Silverlight, WPF, console application, and Windows Phone clients, for both .NET 4 and .NET 4.5.</span></span>

<span data-ttu-id="4dc4a-146">Se a versão do SignalR que têm o cliente é diferente da versão que você tem no servidor, SignalR geralmente é capaz de se adaptar à diferença.</span><span class="sxs-lookup"><span data-stu-id="4dc4a-146">If the version of SignalR that you have on the client is different from the version that you have on the server, SignalR is often able to adapt to the difference.</span></span> <span data-ttu-id="4dc4a-147">Por exemplo, quando SignalR versão 2.0 é liberado e que instalar no servidor, o servidor dará suporte a clientes que possuem 1.1 instalado, bem como os clientes que possuem 2.0 instalado.</span><span class="sxs-lookup"><span data-stu-id="4dc4a-147">For example, when SignalR version 2.0 is released and you install that on the server, the server will support clients that have 1.1.x installed as well as clients that have 2.0 installed.</span></span> <span data-ttu-id="4dc4a-148">Se a diferença entre a versão no servidor e a versão do cliente for muito grande, SignalR lança um `InvalidOperationException` exceção quando o cliente tenta estabelecer uma conexão.</span><span class="sxs-lookup"><span data-stu-id="4dc4a-148">If the difference between the version on the server and the version on the client is too great, SignalR throws an `InvalidOperationException` exception when the client tries to establish a connection.</span></span> <span data-ttu-id="4dc4a-149">A mensagem de erro é "`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`".</span><span class="sxs-lookup"><span data-stu-id="4dc4a-149">The error message is "`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`".</span></span>

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a><span data-ttu-id="4dc4a-150">Como estabelecer uma conexão</span><span class="sxs-lookup"><span data-stu-id="4dc4a-150">How to establish a connection</span></span>

<span data-ttu-id="4dc4a-151">Antes de estabelecer uma conexão, você precisa criar um `HubConnection` de objeto e criar um proxy.</span><span class="sxs-lookup"><span data-stu-id="4dc4a-151">Before you can establish a connection, you have to create a `HubConnection` object and create a proxy.</span></span> <span data-ttu-id="4dc4a-152">Para estabelecer a conexão, chame o `Start` método sobre o `HubConnection` objeto.</span><span class="sxs-lookup"><span data-stu-id="4dc4a-152">To establish the connection, call the `Start` method on the `HubConnection` object.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample1.cs?highlight=1,4)]

> [!NOTE]
> <span data-ttu-id="4dc4a-153">Para clientes JavaScript que você precisa registrar pelo menos um manipulador de eventos antes de chamar o `Start` método para estabelecer a conexão.</span><span class="sxs-lookup"><span data-stu-id="4dc4a-153">For JavaScript clients you have to register at least one event handler before calling the `Start` method to establish the connection.</span></span> <span data-ttu-id="4dc4a-154">Isso não é necessário para clientes do .NET.</span><span class="sxs-lookup"><span data-stu-id="4dc4a-154">This is not necessary for .NET clients.</span></span> <span data-ttu-id="4dc4a-155">Para clientes de JavaScript, o código de proxy gerado automaticamente cria proxies para todos os Hubs que existem no servidor e registrar um manipulador é como você indicar quais Hubs o cliente pretende usar.</span><span class="sxs-lookup"><span data-stu-id="4dc4a-155">For JavaScript clients, the generated proxy code automatically creates proxies for all Hubs that exist on the server, and registering a handler is how you indicate which Hubs your client intends to use.</span></span> <span data-ttu-id="4dc4a-156">Mas para um cliente .NET criar proxies Hub manualmente, para que o SignalR pressupõe que você usará qualquer Hub que você criar um proxy para.</span><span class="sxs-lookup"><span data-stu-id="4dc4a-156">But for a .NET client you create Hub proxies manually, so SignalR assumes that you will be using any Hub that you create a proxy for.</span></span>


<span data-ttu-id="4dc4a-157">O código de exemplo usa o padrão "/ signalr" URL para se conectar ao seu serviço SignalR.</span><span class="sxs-lookup"><span data-stu-id="4dc4a-157">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="4dc4a-158">Para obter informações sobre como especificar uma URL base diferente, consulte [ASP.NET SignalR Hubs API guia - Server - a URL /signalr](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="4dc4a-158">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).</span></span>

<span data-ttu-id="4dc4a-159">O `Start` método é executado de forma assíncrona.</span><span class="sxs-lookup"><span data-stu-id="4dc4a-159">The `Start` method executes asynchronously.</span></span> <span data-ttu-id="4dc4a-160">Para certificar-se de que as linhas subsequentes de código não executar até depois que a conexão é estabelecida, use `await` em um método assíncrono do ASP.NET 4.5 ou `.Wait()` em um método síncrono.</span><span class="sxs-lookup"><span data-stu-id="4dc4a-160">To make sure that subsequent lines of code don't execute until after the connection is established, use `await` in an ASP.NET 4.5 asynchronous method or `.Wait()` in a synchronous method.</span></span> <span data-ttu-id="4dc4a-161">Não use `.Wait()` em um cliente do WinRT.</span><span class="sxs-lookup"><span data-stu-id="4dc4a-161">Don't use `.Wait()` in a WinRT client.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample2.cs?highlight=1)]

[!code-css[Main](signalr-1x-hubs-api-guide-net-client/samples/sample3.css?highlight=1)]

<span data-ttu-id="4dc4a-162">O `HubConnection` classe é thread-safe.</span><span class="sxs-lookup"><span data-stu-id="4dc4a-162">The `HubConnection` class is thread-safe.</span></span>

<a id="slcrossdomain"></a>

### <a name="cross-domain-connections-from-silverlight-clients"></a><span data-ttu-id="4dc4a-163">Conexões entre domínios de clientes do Silverlight</span><span class="sxs-lookup"><span data-stu-id="4dc4a-163">Cross-domain connections from Silverlight clients</span></span>

<span data-ttu-id="4dc4a-164">Para obter informações sobre como habilitar conexões entre domínios de clientes do Silverlight, consulte [tornando um serviço disponível através dos limites do domínio](https://msdn.microsoft.com/en-us/library/cc197955(v=vs.95).aspx).</span><span class="sxs-lookup"><span data-stu-id="4dc4a-164">For information about how to enable cross-domain connections from Silverlight clients, see [Making a Service Available Across Domain Boundaries](https://msdn.microsoft.com/en-us/library/cc197955(v=vs.95).aspx).</span></span>

<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a><span data-ttu-id="4dc4a-165">Como configurar a conexão</span><span class="sxs-lookup"><span data-stu-id="4dc4a-165">How to configure the connection</span></span>

<span data-ttu-id="4dc4a-166">Antes de estabelecer uma conexão, você pode especificar qualquer uma das seguintes opções:</span><span class="sxs-lookup"><span data-stu-id="4dc4a-166">Before you establish a connection, you can specify any of the following options:</span></span>

- <span data-ttu-id="4dc4a-167">Limite de conexões simultâneas.</span><span class="sxs-lookup"><span data-stu-id="4dc4a-167">Concurrent connections limit.</span></span>
- <span data-ttu-id="4dc4a-168">Parâmetros de cadeia de caracteres de consulta.</span><span class="sxs-lookup"><span data-stu-id="4dc4a-168">Query string parameters.</span></span>
- <span data-ttu-id="4dc4a-169">O método de transporte.</span><span class="sxs-lookup"><span data-stu-id="4dc4a-169">The transport method.</span></span>
- <span data-ttu-id="4dc4a-170">Cabeçalhos HTTP.</span><span class="sxs-lookup"><span data-stu-id="4dc4a-170">HTTP headers.</span></span>
- <span data-ttu-id="4dc4a-171">Certificados de cliente.</span><span class="sxs-lookup"><span data-stu-id="4dc4a-171">Client certificates.</span></span>

<a id="maxconnections"></a>

### <a name="how-to-set-the-maximum-number-of-concurrent-connections-in-wpf-clients"></a><span data-ttu-id="4dc4a-172">Como definir o número máximo de conexões simultâneas em clientes do WPF</span><span class="sxs-lookup"><span data-stu-id="4dc4a-172">How to set the maximum number of concurrent connections in WPF clients</span></span>

<span data-ttu-id="4dc4a-173">Em clientes do WPF, você talvez precise aumentar o número máximo de conexões simultâneas de seu valor padrão de 2.</span><span class="sxs-lookup"><span data-stu-id="4dc4a-173">In WPF clients, you might have to increase the maximum number of concurrent connections from its default value of 2.</span></span> <span data-ttu-id="4dc4a-174">O valor recomendado é 10.</span><span class="sxs-lookup"><span data-stu-id="4dc4a-174">The recommended value is 10.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample4.cs?highlight=4)]

<span data-ttu-id="4dc4a-175">Para obter mais informações, consulte [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/en-us/library/system.net.servicepointmanager.defaultconnectionlimit.aspx).</span><span class="sxs-lookup"><span data-stu-id="4dc4a-175">For more information, see [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/en-us/library/system.net.servicepointmanager.defaultconnectionlimit.aspx).</span></span>

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a><span data-ttu-id="4dc4a-176">Como especificar parâmetros de cadeia de caracteres de consulta</span><span class="sxs-lookup"><span data-stu-id="4dc4a-176">How to specify query string parameters</span></span>

<span data-ttu-id="4dc4a-177">Se você deseja enviar dados para o servidor quando o cliente se conecta, você pode adicionar parâmetros de cadeia de caracteres de consulta para o objeto de conexão.</span><span class="sxs-lookup"><span data-stu-id="4dc4a-177">If you want to send data to the server when the client connects, you can add query string parameters to the connection object.</span></span> <span data-ttu-id="4dc4a-178">O exemplo a seguir mostra como definir um parâmetro de cadeia de caracteres de consulta no código do cliente.</span><span class="sxs-lookup"><span data-stu-id="4dc4a-178">The following example shows how to set a query string parameter in client code.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample5.cs)]

<span data-ttu-id="4dc4a-179">O exemplo a seguir mostra como ler um parâmetro de cadeia de caracteres de consulta no código do servidor.</span><span class="sxs-lookup"><span data-stu-id="4dc4a-179">The following example shows how to read a query string parameter in server code.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample6.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a><span data-ttu-id="4dc4a-180">Como especificar o método de transporte</span><span class="sxs-lookup"><span data-stu-id="4dc4a-180">How to specify the transport method</span></span>

<span data-ttu-id="4dc4a-181">Como parte do processo de conexão, um cliente SignalR normalmente negocia com o servidor para determinar o melhor transporte com suporte pelo servidor e cliente.</span><span class="sxs-lookup"><span data-stu-id="4dc4a-181">As part of the process of connecting, a SignalR client normally negotiates with the server to determine the best transport that is supported by both server and client.</span></span> <span data-ttu-id="4dc4a-182">Se você já souber qual transporte você deseja usar, você pode ignorar esse processo de negociação.</span><span class="sxs-lookup"><span data-stu-id="4dc4a-182">If you already know which transport you want to use, you can bypass this negotiation process.</span></span> <span data-ttu-id="4dc4a-183">Para especificar o método de transporte, passe um objeto de transporte para o método Start.</span><span class="sxs-lookup"><span data-stu-id="4dc4a-183">To specify the transport method, pass in a transport object to the Start method.</span></span> <span data-ttu-id="4dc4a-184">O exemplo a seguir mostra como especificar o método de transporte no código do cliente.</span><span class="sxs-lookup"><span data-stu-id="4dc4a-184">The following example shows how to specify the transport method in client code.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample7.cs?highlight=4)]

<span data-ttu-id="4dc4a-185">O [Transports](https://msdn.microsoft.com/en-us/library/jj918090(v=vs.111).aspx) namespace inclui as seguintes classes que você pode usar para especificar o transporte.</span><span class="sxs-lookup"><span data-stu-id="4dc4a-185">The [Microsoft.AspNet.SignalR.Client.Transports](https://msdn.microsoft.com/en-us/library/jj918090(v=vs.111).aspx) namespace includes the following classes that you can use to specify the transport.</span></span>

- <span data-ttu-id="4dc4a-186">[LongPollingTransport](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)</span><span class="sxs-lookup"><span data-stu-id="4dc4a-186">[LongPollingTransport](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)</span></span>
- <span data-ttu-id="4dc4a-187">[ServerSentEventsTransport](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)</span><span class="sxs-lookup"><span data-stu-id="4dc4a-187">[ServerSentEventsTransport](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)</span></span>
- <span data-ttu-id="4dc4a-188">[WebSocketTransport](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) (disponível somente quando o servidor e o cliente usam o .NET 4.5).</span><span class="sxs-lookup"><span data-stu-id="4dc4a-188">[WebSocketTransport](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) (Available only when both server and client use .NET 4.5.)</span></span>
- <span data-ttu-id="4dc4a-189">[AutoTransport](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) (escolhe automaticamente o melhor transporte com suporte pelo cliente e o servidor.</span><span class="sxs-lookup"><span data-stu-id="4dc4a-189">[AutoTransport](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) (Automatically chooses the best transport that is supported by both the client and the server.</span></span> <span data-ttu-id="4dc4a-190">Este é o transporte padrão.</span><span class="sxs-lookup"><span data-stu-id="4dc4a-190">This is the default transport.</span></span> <span data-ttu-id="4dc4a-191">Transmitindo-para o `Start` método tem o mesmo efeito que não está transmitindo nada.)</span><span class="sxs-lookup"><span data-stu-id="4dc4a-191">Passing this in to the `Start` method has the same effect as not passing in anything.)</span></span>

<span data-ttu-id="4dc4a-192">O transporte ForeverFrame não está incluído nesta lista porque ele é usado somente por navegadores.</span><span class="sxs-lookup"><span data-stu-id="4dc4a-192">The ForeverFrame transport is not included in this list because it is used only by browsers.</span></span>

<span data-ttu-id="4dc4a-193">Para obter informações sobre como verificar o método de transporte no código do servidor, consulte [guia de API de Hubs do ASP.NET SignalR - Server - como obter informações sobre o cliente da propriedade de contexto](../guide-to-the-api/hubs-api-guide-server.md#contextproperty).</span><span class="sxs-lookup"><span data-stu-id="4dc4a-193">For information about how to check the transport method in server code, see [ASP.NET SignalR Hubs API Guide - Server - How to get information about the client from the Context property](../guide-to-the-api/hubs-api-guide-server.md#contextproperty).</span></span> <span data-ttu-id="4dc4a-194">Para obter mais informações sobre os transportes e restaurações, consulte [Introdução ao SignalR - transportes e Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="4dc4a-194">For more information about transports and fallbacks, see [Introduction to SignalR - Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="httpheaders"></a>

### <a name="how-to-specify-http-headers"></a><span data-ttu-id="4dc4a-195">Como especificar cabeçalhos HTTP</span><span class="sxs-lookup"><span data-stu-id="4dc4a-195">How to specify HTTP headers</span></span>

<span data-ttu-id="4dc4a-196">Para definir cabeçalhos HTTP, use o `Headers` propriedade no objeto de conexão.</span><span class="sxs-lookup"><span data-stu-id="4dc4a-196">To set HTTP headers, use the `Headers` property on the connection object.</span></span> <span data-ttu-id="4dc4a-197">O exemplo a seguir mostra como adicionar um cabeçalho HTTP.</span><span class="sxs-lookup"><span data-stu-id="4dc4a-197">The following example shows how to add an HTTP header.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample8.cs?highlight=2)]

<a id="clientcertificate"></a>

### <a name="how-to-specify-client-certificates"></a><span data-ttu-id="4dc4a-198">Como especificar os certificados de cliente</span><span class="sxs-lookup"><span data-stu-id="4dc4a-198">How to specify client certificates</span></span>

<span data-ttu-id="4dc4a-199">Para adicionar certificados de cliente, use o `AddClientCertificate` método no objeto de conexão.</span><span class="sxs-lookup"><span data-stu-id="4dc4a-199">To add client certificates, use the `AddClientCertificate` method on the connection object.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample9.cs?highlight=2)]

<a id="proxy"></a>

## <a name="how-to-create-the-hub-proxy"></a><span data-ttu-id="4dc4a-200">Como criar o proxy do Hub</span><span class="sxs-lookup"><span data-stu-id="4dc4a-200">How to create the Hub proxy</span></span>

<span data-ttu-id="4dc4a-201">Para definir métodos no cliente que pode chamar um Hub do servidor e para invocar métodos em um Hub no servidor, crie um proxy para o Hub chamando `CreateHubProxy` no objeto de conexão.</span><span class="sxs-lookup"><span data-stu-id="4dc4a-201">In order to define methods on the client that a Hub can call from the server, and to invoke methods on a Hub at the server, create a proxy for the Hub by calling `CreateHubProxy` on the connection object.</span></span> <span data-ttu-id="4dc4a-202">A cadeia de caracteres para o qual você passar `CreateHubProxy` é o nome da classe do Hub ou o nome especificado pelo `HubName` atributo se estava sendo usado no servidor.</span><span class="sxs-lookup"><span data-stu-id="4dc4a-202">The string you pass in to `CreateHubProxy` is the name of your Hub class, or the name specified by the `HubName` attribute if one was used on the server.</span></span> <span data-ttu-id="4dc4a-203">Correspondência de nome diferencia maiusculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="4dc4a-203">Name matching is case-insensitive.</span></span>

<span data-ttu-id="4dc4a-204">**Classe de Hub no servidor**</span><span class="sxs-lookup"><span data-stu-id="4dc4a-204">**Hub class on server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample10.cs?highlight=1)]

<span data-ttu-id="4dc4a-205">**Criar proxy de cliente para a classe de Hub**</span><span class="sxs-lookup"><span data-stu-id="4dc4a-205">**Create client proxy for the Hub class**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample11.cs?highlight=2)]

<span data-ttu-id="4dc4a-206">Se você decora sua classe de Hub com um `HubName` de atributo, use esse nome.</span><span class="sxs-lookup"><span data-stu-id="4dc4a-206">If you decorate your Hub class with a `HubName` attribute, use that name.</span></span>

<span data-ttu-id="4dc4a-207">**Classe de Hub no servidor**</span><span class="sxs-lookup"><span data-stu-id="4dc4a-207">**Hub class on server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample12.cs)]

<span data-ttu-id="4dc4a-208">**Criar proxy de cliente para a classe de Hub**</span><span class="sxs-lookup"><span data-stu-id="4dc4a-208">**Create client proxy for the Hub class**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample13.cs?highlight=2)]

<span data-ttu-id="4dc4a-209">O objeto proxy é thread-safe.</span><span class="sxs-lookup"><span data-stu-id="4dc4a-209">The proxy object is thread-safe.</span></span> <span data-ttu-id="4dc4a-210">Na verdade, se você chamar `HubConnection.CreateHubProxy` várias vezes com o mesmo `hubName`, você obtém a mesma em cache `IHubProxy` objeto.</span><span class="sxs-lookup"><span data-stu-id="4dc4a-210">In fact, if you call `HubConnection.CreateHubProxy` multiple times with the same `hubName`, you get the same cached `IHubProxy` object.</span></span>

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a><span data-ttu-id="4dc4a-211">Como definir métodos no cliente que o servidor pode chamar</span><span class="sxs-lookup"><span data-stu-id="4dc4a-211">How to define methods on the client that the server can call</span></span>

<span data-ttu-id="4dc4a-212">Para definir um método que o servidor pode chamar, use o proxy `On` método para registrar um manipulador de eventos.</span><span class="sxs-lookup"><span data-stu-id="4dc4a-212">To define a method that the server can call, use the proxy's `On` method to register an event handler.</span></span>

<span data-ttu-id="4dc4a-213">Correspondência de nome do método diferencia maiusculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="4dc4a-213">Method name matching is case-insensitive.</span></span> <span data-ttu-id="4dc4a-214">Por exemplo, `Clients.All.UpdateStockPrice` no servidor executará `updateStockPrice`, `updatestockprice`, ou `UpdateStockPrice` no cliente.</span><span class="sxs-lookup"><span data-stu-id="4dc4a-214">For example, `Clients.All.UpdateStockPrice` on the server will execute `updateStockPrice`, `updatestockprice`, or `UpdateStockPrice` on the client.</span></span>

<span data-ttu-id="4dc4a-215">Plataformas de cliente diferentes têm requisitos diferentes como escrever o código do método para atualizar a interface do usuário.</span><span class="sxs-lookup"><span data-stu-id="4dc4a-215">Different client platforms have different requirements for how you write method code to update the UI.</span></span> <span data-ttu-id="4dc4a-216">Os exemplos mostrados são para clientes do WinRT (Windows Store .NET).</span><span class="sxs-lookup"><span data-stu-id="4dc4a-216">The examples shown are for WinRT (Windows Store .NET) clients.</span></span> <span data-ttu-id="4dc4a-217">WPF, Silverlight e exemplos de aplicativos de console são fornecidos nos [uma seção separada neste tópico](#wpfsl).</span><span class="sxs-lookup"><span data-stu-id="4dc4a-217">WPF, Silverlight, and console application examples are provided in [a separate section later in this topic](#wpfsl).</span></span>

<a id="clientmethodswithoutparms"></a>

### <a name="methods-without-parameters"></a><span data-ttu-id="4dc4a-218">Métodos sem parâmetros</span><span class="sxs-lookup"><span data-stu-id="4dc4a-218">Methods without parameters</span></span>

<span data-ttu-id="4dc4a-219">Se o método que você está tratando não tiver parâmetros, use a sobrecarga de não-genérica do `On` método:</span><span class="sxs-lookup"><span data-stu-id="4dc4a-219">If the method you're handling does not have parameters, use the non-generic overload of the `On` method:</span></span>

<span data-ttu-id="4dc4a-220">**Código do servidor chamando o método de cliente sem parâmetros**</span><span class="sxs-lookup"><span data-stu-id="4dc4a-220">**Server code calling client method without parameters**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample14.cs?highlight=5)]

<span data-ttu-id="4dc4a-221">**Código do cliente do WinRT para método de chamada do servidor sem parâmetros ([consulte WPF e Silverlight exemplos mais adiante neste tópico](#wpfsl))**</span><span class="sxs-lookup"><span data-stu-id="4dc4a-221">**WinRT Client code for method called from server without parameters ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample15.cs)]

<a id="clientmethodswithparmtypes"></a>

### <a name="methods-with-parameters-specifying-the-parameter-types"></a><span data-ttu-id="4dc4a-222">Métodos com parâmetros, especificando os tipos de parâmetro</span><span class="sxs-lookup"><span data-stu-id="4dc4a-222">Methods with parameters, specifying the parameter types</span></span>

<span data-ttu-id="4dc4a-223">Se o método que você está lidando com tiver parâmetros, especifique os tipos dos parâmetros como os tipos genéricos do `On` método.</span><span class="sxs-lookup"><span data-stu-id="4dc4a-223">If the method you're handling has parameters, specify the types of the parameters as the generic types of the `On` method.</span></span> <span data-ttu-id="4dc4a-224">Há sobrecargas genéricas do `On` método para permitir que você especifique parâmetros até 8 (4 no Windows Phone 7).</span><span class="sxs-lookup"><span data-stu-id="4dc4a-224">There are generic overloads of the `On` method to enable you to specify up to 8 parameters (4 on Windows Phone 7).</span></span> <span data-ttu-id="4dc4a-225">No exemplo a seguir, um parâmetro é enviado para o `UpdateStockPrice` método.</span><span class="sxs-lookup"><span data-stu-id="4dc4a-225">In the following example, one parameter is sent to the `UpdateStockPrice` method.</span></span>

<span data-ttu-id="4dc4a-226">**Chamar o método de cliente com um parâmetro de código do servidor**</span><span class="sxs-lookup"><span data-stu-id="4dc4a-226">**Server code calling client method with a parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample16.cs?highlight=3)]

<span data-ttu-id="4dc4a-227">**A classe de estoque usada para o parâmetro**</span><span class="sxs-lookup"><span data-stu-id="4dc4a-227">**The Stock class used for the parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample17.cs)]

<span data-ttu-id="4dc4a-228">**Código do cliente do WinRT para um método de chamada do servidor com um parâmetro ([consulte WPF e Silverlight exemplos mais adiante neste tópico](#wpfsl))**</span><span class="sxs-lookup"><span data-stu-id="4dc4a-228">**WinRT Client code for a method called from server with a parameter ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample18.cs?highlight=1,5)]

<a id="clientmethodswithdynamparms"></a>

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a><span data-ttu-id="4dc4a-229">Métodos com parâmetros, especificando objetos dinâmicos para os parâmetros</span><span class="sxs-lookup"><span data-stu-id="4dc4a-229">Methods with parameters, specifying dynamic objects for the parameters</span></span>

<span data-ttu-id="4dc4a-230">Como uma alternativa para especificar parâmetros como tipos genéricos do `On` método, você pode especificar parâmetros como objetos dinâmicos:</span><span class="sxs-lookup"><span data-stu-id="4dc4a-230">As an alternative to specifying parameters as generic types of the `On` method, you can specify parameters as dynamic objects:</span></span>

<span data-ttu-id="4dc4a-231">**Chamar o método de cliente com um parâmetro de código do servidor**</span><span class="sxs-lookup"><span data-stu-id="4dc4a-231">**Server code calling client method with a parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample19.cs?highlight=3)]

<span data-ttu-id="4dc4a-232">**A classe de estoque usada para o parâmetro**</span><span class="sxs-lookup"><span data-stu-id="4dc4a-232">**The Stock class used for the parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample20.cs)]

<span data-ttu-id="4dc4a-233">**Código do cliente do WinRT para um método chamado a partir do servidor com um parâmetro, usando um objeto dinâmico para o parâmetro ([consulte WPF e Silverlight exemplos mais adiante neste tópico](#wpfsl))**</span><span class="sxs-lookup"><span data-stu-id="4dc4a-233">**WinRT Client code for a method called from server with a parameter, using a dynamic object for the parameter ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample21.cs?highlight=1,5)]

<a id="removehandler"></a>

### <a name="how-to-remove-a-handler"></a><span data-ttu-id="4dc4a-234">Como remover um manipulador</span><span class="sxs-lookup"><span data-stu-id="4dc4a-234">How to remove a handler</span></span>

<span data-ttu-id="4dc4a-235">Para remover um manipulador, chame seu `Dispose` método.</span><span class="sxs-lookup"><span data-stu-id="4dc4a-235">To remove a handler, call its `Dispose` method.</span></span>

<span data-ttu-id="4dc4a-236">**Código do cliente para um método chamado a partir do servidor**</span><span class="sxs-lookup"><span data-stu-id="4dc4a-236">**Client code for a method called from server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample22.cs?highlight=1)]

<span data-ttu-id="4dc4a-237">**Código do cliente para remover o manipulador**</span><span class="sxs-lookup"><span data-stu-id="4dc4a-237">**Client code to remove the handler**</span></span>

[!code-css[Main](signalr-1x-hubs-api-guide-net-client/samples/sample23.css?highlight=1)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a><span data-ttu-id="4dc4a-238">Como chamar métodos do servidor do cliente</span><span class="sxs-lookup"><span data-stu-id="4dc4a-238">How to call server methods from the client</span></span>

<span data-ttu-id="4dc4a-239">Para chamar um método no servidor, use o `Invoke` método no proxy do Hub.</span><span class="sxs-lookup"><span data-stu-id="4dc4a-239">To call a method on the server, use the `Invoke` method on the Hub proxy.</span></span>

<span data-ttu-id="4dc4a-240">Se o método de servidor não tem valor retornado, use a sobrecarga de não-genérica do `Invoke` método.</span><span class="sxs-lookup"><span data-stu-id="4dc4a-240">If the server method has no return value, use the non-generic overload of the `Invoke` method.</span></span>

<span data-ttu-id="4dc4a-241">**Código do servidor para um método que não tem nenhum valor de retorno**</span><span class="sxs-lookup"><span data-stu-id="4dc4a-241">**Server code for a method that has no return value**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample24.cs?highlight=3)]

<span data-ttu-id="4dc4a-242">**Código do cliente chamar um método que não tem nenhum valor de retorno**</span><span class="sxs-lookup"><span data-stu-id="4dc4a-242">**Client code calling a method that has no return value**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample25.cs?highlight=1)]

<span data-ttu-id="4dc4a-243">Se o método de servidor tem um valor de retorno, especifique o tipo de retorno do tipo genérico do `Invoke` método.</span><span class="sxs-lookup"><span data-stu-id="4dc4a-243">If the server method has a return value, specify the return type as the generic type of the `Invoke` method.</span></span>

<span data-ttu-id="4dc4a-244">**Código do servidor para um método que tem um valor de retorno e usa um parâmetro de tipo complexo**</span><span class="sxs-lookup"><span data-stu-id="4dc4a-244">**Server code for a method that has a return value and takes a complex type parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample26.cs?highlight=1)]

<span data-ttu-id="4dc4a-245">**A classe de estoque usada para o parâmetro e valor de retorno**</span><span class="sxs-lookup"><span data-stu-id="4dc4a-245">**The Stock class used for the parameter and return value**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample27.cs)]

<span data-ttu-id="4dc4a-246">**Código do cliente chamar um método que tem um valor de retorno e usa um parâmetro de tipo complexo, em um método assíncrono de ASP.NET 4.5**</span><span class="sxs-lookup"><span data-stu-id="4dc4a-246">**Client code calling a method that has a return value and takes a complex type parameter, in an ASP.NET 4.5 async method**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample28.cs?highlight=1-2)]

<span data-ttu-id="4dc4a-247">**Código do cliente chamar um método que tem um valor de retorno e usa um parâmetro de tipo complexo, em um método síncrono**</span><span class="sxs-lookup"><span data-stu-id="4dc4a-247">**Client code calling a method that has a return value and takes a complex type parameter, in a synchronous method**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample29.cs?highlight=1-2)]

<span data-ttu-id="4dc4a-248">O `Invoke` método executa de forma assíncrona e retorna um `Task` objeto.</span><span class="sxs-lookup"><span data-stu-id="4dc4a-248">The `Invoke` method executes asynchronously and returns a `Task` object.</span></span> <span data-ttu-id="4dc4a-249">Se você não especificar `await` ou `.Wait()`, a próxima linha de código será executado antes do método que você chamar concluiu a execução.</span><span class="sxs-lookup"><span data-stu-id="4dc4a-249">If you don't specify `await` or `.Wait()`, the next line of code will execute before the method that you invoke has finished executing.</span></span>

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a><span data-ttu-id="4dc4a-250">Como manipular eventos de tempo de vida da conexão</span><span class="sxs-lookup"><span data-stu-id="4dc4a-250">How to handle connection lifetime events</span></span>

<span data-ttu-id="4dc4a-251">O SignalR fornece a seguinte conexão eventos de tempo de vida que você pode manipular:</span><span class="sxs-lookup"><span data-stu-id="4dc4a-251">SignalR provides the following connection lifetime events that you can handle:</span></span>

- <span data-ttu-id="4dc4a-252">`Received`: Gerado quando nenhum dado é recebido na conexão.</span><span class="sxs-lookup"><span data-stu-id="4dc4a-252">`Received`: Raised when any data is received on the connection.</span></span> <span data-ttu-id="4dc4a-253">Fornece os dados recebidos.</span><span class="sxs-lookup"><span data-stu-id="4dc4a-253">Provides the received data.</span></span>
- <span data-ttu-id="4dc4a-254">`ConnectionSlow`: Gerado quando o cliente detecta uma conexão lenta ou remoção com frequência.</span><span class="sxs-lookup"><span data-stu-id="4dc4a-254">`ConnectionSlow`: Raised when the client detects a slow or frequently dropping connection.</span></span>
- <span data-ttu-id="4dc4a-255">`Reconnecting`: Gerado quando o transporte subjacente inicia a reconexão.</span><span class="sxs-lookup"><span data-stu-id="4dc4a-255">`Reconnecting`: Raised when the underlying transport begins reconnecting.</span></span>
- <span data-ttu-id="4dc4a-256">`Reconnected`: Gerado quando o transporte subjacente foi reconectada.</span><span class="sxs-lookup"><span data-stu-id="4dc4a-256">`Reconnected`: Raised when the underlying transport has reconnected.</span></span>
- <span data-ttu-id="4dc4a-257">`StateChanged`: Gerado quando o estado da conexão é alterado.</span><span class="sxs-lookup"><span data-stu-id="4dc4a-257">`StateChanged`: Raised when the connection state changes.</span></span> <span data-ttu-id="4dc4a-258">Fornece o estado antigo e o novo estado.</span><span class="sxs-lookup"><span data-stu-id="4dc4a-258">Provides the old state and the new state.</span></span> <span data-ttu-id="4dc4a-259">Para obter informações sobre conexão consulte valores de estado [enumeração ConnectionState](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="4dc4a-259">For information about connection state values see [ConnectionState Enumeration](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx).</span></span>
- <span data-ttu-id="4dc4a-260">`Closed`: Gerado quando a conexão foi desconectado.</span><span class="sxs-lookup"><span data-stu-id="4dc4a-260">`Closed`: Raised when the connection has disconnected.</span></span>

<span data-ttu-id="4dc4a-261">Por exemplo, se você quiser exibir mensagens de aviso para erros não fatais mas causam problemas intermitentes de conexão, como lentidão ou frequentes descartando de conexão, tratar o `ConnectionSlow` evento.</span><span class="sxs-lookup"><span data-stu-id="4dc4a-261">For example, if you want to display warning messages for errors that are not fatal but cause intermittent connection problems, such as slowness or frequent dropping of the connection, handle the `ConnectionSlow` event.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample30.cs)]

<span data-ttu-id="4dc4a-262">Para obter mais informações, consulte [compreensão e tratamento de eventos de tempo de vida de Conexão no SignalR](../guide-to-the-api/handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="4dc4a-262">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](../guide-to-the-api/handling-connection-lifetime-events.md).</span></span>

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a><span data-ttu-id="4dc4a-263">Como tratar erros</span><span class="sxs-lookup"><span data-stu-id="4dc4a-263">How to handle errors</span></span>

<span data-ttu-id="4dc4a-264">Se você não habilitar explicitamente as mensagens de erro detalhadas no servidor, o objeto de exceção SignalR retorna após um erro contém algumas informações sobre o erro.</span><span class="sxs-lookup"><span data-stu-id="4dc4a-264">If you don't explicitly enable detailed error messages on the server, the exception object that SignalR returns after an error contains minimal information about the error.</span></span> <span data-ttu-id="4dc4a-265">Por exemplo, se uma chamada para `newContosoChatMessage` falhar, a mensagem de erro no objeto de erro contém "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" enviar mensagens de erro detalhadas para clientes de produção não é recomendado por motivos de segurança, mas se você deseja habilitar as mensagens de erro detalhadas para solução de problemas, use o seguinte código no servidor.</span><span class="sxs-lookup"><span data-stu-id="4dc4a-265">For example, if a call to `newContosoChatMessage` fails, the error message in the error object contains "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" Sending detailed error messages to clients in production is not recommended for security reasons, but if you want to enable detailed error messages for troubleshooting purposes, use the following code on the server.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample31.cs?highlight=2)]

<a id="handleerrors"></a>

<span data-ttu-id="4dc4a-266">Para tratar erros que gera o SignalR, você pode adicionar um manipulador para o `Error` evento no objeto de conexão.</span><span class="sxs-lookup"><span data-stu-id="4dc4a-266">To handle errors that SignalR raises, you can add a handler for the `Error` event on the connection object.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample32.cs)]

<span data-ttu-id="4dc4a-267">Para tratar erros de invocações do método, encapsule o código em um bloco try-catch.</span><span class="sxs-lookup"><span data-stu-id="4dc4a-267">To handle errors from method invocations, wrap the code in a try-catch block.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample33.cs)]

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a><span data-ttu-id="4dc4a-268">Como habilitar o registro de cliente</span><span class="sxs-lookup"><span data-stu-id="4dc4a-268">How to enable client-side logging</span></span>

<span data-ttu-id="4dc4a-269">Para habilitar o registro em log do lado do cliente, defina o `TraceLevel` e `TraceWriter` propriedades no objeto de conexão.</span><span class="sxs-lookup"><span data-stu-id="4dc4a-269">To enable client-side logging, set the `TraceLevel` and `TraceWriter` properties on the connection object.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample34.cs?highlight=2-3)]

<a id="wpfsl"></a>

## <a name="wpf-silverlight-and-console-application-code-samples-for-client-methods-that-the-server-can-call"></a><span data-ttu-id="4dc4a-270">Exemplos de código de aplicativo de console para métodos de cliente que o servidor pode chamar, Silverlight e WPF</span><span class="sxs-lookup"><span data-stu-id="4dc4a-270">WPF, Silverlight, and console application code samples for client methods that the server can call</span></span>

<span data-ttu-id="4dc4a-271">Os exemplos de código mostrados anteriormente para definir métodos de cliente que o servidor pode chamar se aplicam aos clientes do WinRT.</span><span class="sxs-lookup"><span data-stu-id="4dc4a-271">The code samples shown earlier for defining client methods that the server can call apply to WinRT clients.</span></span> <span data-ttu-id="4dc4a-272">Os exemplos a seguir mostram o código equivalente para WPF, Silverlight e clientes de aplicativo de console.</span><span class="sxs-lookup"><span data-stu-id="4dc4a-272">The following samples show the equivalent code for WPF, Silverlight, and console application clients.</span></span>

### <a name="methods-without-parameters"></a><span data-ttu-id="4dc4a-273">Métodos sem parâmetros</span><span class="sxs-lookup"><span data-stu-id="4dc4a-273">Methods without parameters</span></span>

<span data-ttu-id="4dc4a-274">**Código do cliente WPF para o método chamado a partir do servidor sem parâmetros**</span><span class="sxs-lookup"><span data-stu-id="4dc4a-274">**WPF client code for method called from server without parameters**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample35.cs?highlight=1)]

<span data-ttu-id="4dc4a-275">**Código do cliente Silverlight para o método chamado a partir do servidor sem parâmetros**</span><span class="sxs-lookup"><span data-stu-id="4dc4a-275">**Silverlight client code for method called from server without parameters**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample36.cs?highlight=1)]

<span data-ttu-id="4dc4a-276">**Código de cliente do aplicativo de console para o método chamado a partir do servidor sem parâmetros**</span><span class="sxs-lookup"><span data-stu-id="4dc4a-276">**Console application client code for method called from server without parameters**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample37.cs?highlight=1)]

### <a name="methods-with-parameters-specifying-the-parameter-types"></a><span data-ttu-id="4dc4a-277">Métodos com parâmetros, especificando os tipos de parâmetro</span><span class="sxs-lookup"><span data-stu-id="4dc4a-277">Methods with parameters, specifying the parameter types</span></span>

<span data-ttu-id="4dc4a-278">**Código de cliente do WPF para um método chamado a partir do servidor com um parâmetro**</span><span class="sxs-lookup"><span data-stu-id="4dc4a-278">**WPF client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample38.cs?highlight=1,4)]

<span data-ttu-id="4dc4a-279">**Código do cliente Silverlight para um método chamado a partir do servidor com um parâmetro**</span><span class="sxs-lookup"><span data-stu-id="4dc4a-279">**Silverlight client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample39.cs?highlight=1,5)]

<span data-ttu-id="4dc4a-280">**Código de cliente do aplicativo de console para um método chamado a partir do servidor com um parâmetro**</span><span class="sxs-lookup"><span data-stu-id="4dc4a-280">**Console application client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample40.cs?highlight=1-2)]

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a><span data-ttu-id="4dc4a-281">Métodos com parâmetros, especificando objetos dinâmicos para os parâmetros</span><span class="sxs-lookup"><span data-stu-id="4dc4a-281">Methods with parameters, specifying dynamic objects for the parameters</span></span>

<span data-ttu-id="4dc4a-282">**Código de cliente do WPF para um método chamado a partir do servidor com um parâmetro, usando um objeto dinâmico para o parâmetro**</span><span class="sxs-lookup"><span data-stu-id="4dc4a-282">**WPF client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample41.cs?highlight=1,4)]

<span data-ttu-id="4dc4a-283">**Código do cliente Silverlight para um método chamado a partir do servidor com um parâmetro, usando um objeto dinâmico para o parâmetro**</span><span class="sxs-lookup"><span data-stu-id="4dc4a-283">**Silverlight client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample42.cs?highlight=1,5)]

<span data-ttu-id="4dc4a-284">**Código de cliente do aplicativo de console para um método chamado a partir do servidor com um parâmetro, usando um objeto dinâmico para o parâmetro**</span><span class="sxs-lookup"><span data-stu-id="4dc4a-284">**Console application client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample43.cs?highlight=1-2)]

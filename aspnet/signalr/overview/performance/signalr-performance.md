---
uid: signalr/overview/performance/signalr-performance
title: Desempenho do SignalR | Microsoft Docs
author: pfletcher
description: Desempenho do SignalR
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 3751f5e7-59db-4be0-a290-50abc24e5c84
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/performance/signalr-performance
msc.type: authoredcontent
ms.openlocfilehash: 4468ee8031afccca847db67bd4b5b263f0a2c5ac
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="signalr-performance"></a><span data-ttu-id="e11a6-103">Desempenho do SignalR</span><span class="sxs-lookup"><span data-stu-id="e11a6-103">SignalR Performance</span></span>
====================
<span data-ttu-id="e11a6-104">por [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="e11a6-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> <span data-ttu-id="e11a6-105">Este tópico descreve como criar, medir e melhorar o desempenho em um aplicativo do SignalR.</span><span class="sxs-lookup"><span data-stu-id="e11a6-105">This topic describes how to design for, measure, and improve performance in a SignalR application.</span></span>
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="e11a6-106">Versões de software usadas neste tópico</span><span class="sxs-lookup"><span data-stu-id="e11a6-106">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="e11a6-107">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="e11a6-107">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="e11a6-108">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="e11a6-108">.NET 4.5</span></span>
> - <span data-ttu-id="e11a6-109">SignalR versão 2</span><span class="sxs-lookup"><span data-stu-id="e11a6-109">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="e11a6-110">Versões anteriores deste tópico</span><span class="sxs-lookup"><span data-stu-id="e11a6-110">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="e11a6-111">Para obter informações sobre versões anteriores do SignalR, consulte [versões mais antigas do SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="e11a6-111">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="e11a6-112">Perguntas e comentários</span><span class="sxs-lookup"><span data-stu-id="e11a6-112">Questions and comments</span></span>
> 
> <span data-ttu-id="e11a6-113">Deixe comentários em como você gostou neste tutorial e o que podemos melhorar nos comentários na parte inferior da página.</span><span class="sxs-lookup"><span data-stu-id="e11a6-113">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="e11a6-114">Se você tiver dúvidas que não estão diretamente relacionadas ao tutorial, você poderá postá-los para o [ASP.NET SignalR fórum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="e11a6-114">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="e11a6-115">Para uma apresentação recente de dimensionamento e desempenho de SignalR, consulte [dimensionamento Web em tempo real com ASP.NET SignalR](https://channel9.msdn.com/Events/Build/2013/3-502).</span><span class="sxs-lookup"><span data-stu-id="e11a6-115">For a recent presentation on SignalR performance and scaling, see [Scaling the Real-time Web with ASP.NET SignalR](https://channel9.msdn.com/Events/Build/2013/3-502).</span></span>

<span data-ttu-id="e11a6-116">Esse tópico contém as seguintes seções:</span><span class="sxs-lookup"><span data-stu-id="e11a6-116">This topic contains the following sections:</span></span>

- [<span data-ttu-id="e11a6-117">Considerações de design</span><span class="sxs-lookup"><span data-stu-id="e11a6-117">Design considerations</span></span>](#design)
- [<span data-ttu-id="e11a6-118">Ajuste de seu servidor do SignalR para desempenho</span><span class="sxs-lookup"><span data-stu-id="e11a6-118">Tuning your SignalR server for performance</span></span>](#tuning)
- [<span data-ttu-id="e11a6-119">Solucionar problemas de desempenho</span><span class="sxs-lookup"><span data-stu-id="e11a6-119">Troubleshooting performance issues</span></span>](#troubleshooting)
- [<span data-ttu-id="e11a6-120">Usando contadores de desempenho do SignalR</span><span class="sxs-lookup"><span data-stu-id="e11a6-120">Using SignalR performance counters</span></span>](#perfcounters)
- [<span data-ttu-id="e11a6-121">Usando outros contadores de desempenho</span><span class="sxs-lookup"><span data-stu-id="e11a6-121">Using other performance counters</span></span>](#othercounters)
- [<span data-ttu-id="e11a6-122">Outros recursos</span><span class="sxs-lookup"><span data-stu-id="e11a6-122">Other resources</span></span>](#otherresources)

<a id="design"></a>

## <a name="design-considerations"></a><span data-ttu-id="e11a6-123">Considerações de design</span><span class="sxs-lookup"><span data-stu-id="e11a6-123">Design considerations</span></span>

<span data-ttu-id="e11a6-124">Esta seção descreve os padrões que podem ser implementados durante o design de um aplicativo de SignalR, para garantir que o desempenho não é sendo prejudicado por gerar tráfego de rede desnecessário.</span><span class="sxs-lookup"><span data-stu-id="e11a6-124">This section describes patterns that can be implemented during the design of a SignalR application, to ensure that performance is not being hindered by generating unnecessary network traffic.</span></span>

### <a name="throttling-message-frequency"></a><span data-ttu-id="e11a6-125">Frequência de mensagem de limitação</span><span class="sxs-lookup"><span data-stu-id="e11a6-125">Throttling message frequency</span></span>

<span data-ttu-id="e11a6-126">Mesmo em um aplicativo que envia mensagens em uma frequência alta (como um aplicativo de jogos em tempo real), a maioria dos aplicativos não precisa enviar mais do que alguns mensagens por segundo.</span><span class="sxs-lookup"><span data-stu-id="e11a6-126">Even in an application that sends out messages at a high frequency (such as a realtime gaming application), most applications don't need to send more than a few messages a second.</span></span> <span data-ttu-id="e11a6-127">Para reduzir a quantidade de tráfego que cada cliente gera, um loop de mensagens pode ser implementado filas e envia as mensagens não mais frequentemente do que uma taxa fixa (ou seja, até um certo número de mensagens será enviado por segundo, se houver mensagens em que o tempo em intervalo a ser enviado).</span><span class="sxs-lookup"><span data-stu-id="e11a6-127">To reduce the amount of traffic that each client generates, a message loop can be implemented that queues and sends out messages no more frequently than a fixed rate (that is, up to a certain number of messages will be sent every second, if there are messages in that time interval to be sent).</span></span> <span data-ttu-id="e11a6-128">Para um aplicativo de exemplo que limita as mensagens para uma determinada taxa (de cliente e servidor), consulte [alta frequência em tempo real com SignalR](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="e11a6-128">For a sample application that throttles messages to a certain rate (from both client and server), see [High-Frequency Realtime with SignalR](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).</span></span>

### <a name="reducing-message-size"></a><span data-ttu-id="e11a6-129">Reduzindo o tamanho da mensagem</span><span class="sxs-lookup"><span data-stu-id="e11a6-129">Reducing message size</span></span>

<span data-ttu-id="e11a6-130">Você pode reduzir o tamanho de uma mensagem do SignalR, reduzindo o tamanho dos objetos serializados.</span><span class="sxs-lookup"><span data-stu-id="e11a6-130">You can reduce the size of a SignalR message by reducing the size of your serialized objects.</span></span> <span data-ttu-id="e11a6-131">No código do servidor, se você estiver enviando um objeto que contém propriedades que não precisam ser transmitido, impedir que as propriedades que está sendo serializado usando o `JsonIgnore` atributo.</span><span class="sxs-lookup"><span data-stu-id="e11a6-131">In server code, if you're sending an object that contains properties that don't need to be transmitted, prevent those properties from being serialized by using the `JsonIgnore` attribute.</span></span> <span data-ttu-id="e11a6-132">Os nomes das propriedades também são armazenados na mensagem; os nomes das propriedades podem ser reduzidos usando o `JsonProperty` atributo.</span><span class="sxs-lookup"><span data-stu-id="e11a6-132">The names of properties are also stored in the message; the names of properties can be shortened using the `JsonProperty` attribute.</span></span> <span data-ttu-id="e11a6-133">O exemplo de código a seguir demonstra como excluir uma propriedade que está sendo enviado ao cliente e como reduzir os nomes de propriedade:</span><span class="sxs-lookup"><span data-stu-id="e11a6-133">The following code sample demonstrates how to exclude a property from being sent to the client, and how to shorten property names:</span></span>

<span data-ttu-id="e11a6-134">**Código do servidor .NET que demonstra o atributo JsonIgnore para excluir os dados sejam enviados para o cliente e o atributo JsonProperty para reduzir o tamanho de mensagem**</span><span class="sxs-lookup"><span data-stu-id="e11a6-134">**.NET server code that demonstrates the JsonIgnore attribute to exclude data from being sent to the client, and the JsonProperty attribute to reduce message size**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample1.cs?highlight=5,7,10)]

<span data-ttu-id="e11a6-135">Para manter a legibilidade / facilidade de manutenção no código do cliente, os nomes de propriedade abreviada podem ser remapeados para humanos amigável nomes depois que a mensagem for recebida.</span><span class="sxs-lookup"><span data-stu-id="e11a6-135">In order to retain readability/ maintainability in the client code, the abbreviated property names can be remapped to human-friendly names after the message is received.</span></span> <span data-ttu-id="e11a6-136">O exemplo de código a seguir demonstra uma maneira possível de remapeamento reduzidos nomes para os mais longo, definindo um contrato de mensagem (mapeamento) e usando o `reMap` função para aplicar o contrato para a classe de mensagem otimizado:</span><span class="sxs-lookup"><span data-stu-id="e11a6-136">The following code sample demonstrates one possible way of remapping shortened names to longer ones, by defining a message contract (mapping), and using the `reMap` function to apply the contract to the optimized message class:</span></span>

<span data-ttu-id="e11a6-137">**Código de JavaScript do lado do cliente que remapeia reduzido nomes de propriedade nomes legíveis**</span><span class="sxs-lookup"><span data-stu-id="e11a6-137">**Client-side JavaScript code that remaps shortened property names to human-readable names**</span></span>

[!code-javascript[Main](signalr-performance/samples/sample2.js)]

<span data-ttu-id="e11a6-138">Nomes podem ser reduzidos em mensagens do cliente para o servidor, usando o mesmo método.</span><span class="sxs-lookup"><span data-stu-id="e11a6-138">Names can be shortened in messages from the client to the server as well, using the same method.</span></span>

<span data-ttu-id="e11a6-139">Redução da superfície de memória (ou seja, a quantidade de memória usada para a mensagem) da mensagem de objeto também pode melhorar o desempenho.</span><span class="sxs-lookup"><span data-stu-id="e11a6-139">Reducing the memory footprint (that is, the amount of memory used for the message) of the message object can also improve performance.</span></span> <span data-ttu-id="e11a6-140">Por exemplo, se a gama completa de um `int` não é necessária uma `short` ou `byte` pode ser usado em vez disso.</span><span class="sxs-lookup"><span data-stu-id="e11a6-140">For example, if the full range of an `int` is not needed, a `short` or `byte` can be used instead.</span></span>

<span data-ttu-id="e11a6-141">Como as mensagens são armazenadas no barramento de mensagem na memória do servidor, reduzindo o tamanho de mensagens também pode resolver problemas de memória do servidor.</span><span class="sxs-lookup"><span data-stu-id="e11a6-141">Since messages are stored in the message bus in server memory, reducing the size of messages can also address server memory issues.</span></span>

<a id="tuning"></a>

### <a name="tuning-your-signalr-server-for-performance"></a><span data-ttu-id="e11a6-142">Ajuste de seu servidor do SignalR para desempenho</span><span class="sxs-lookup"><span data-stu-id="e11a6-142">Tuning your SignalR server for performance</span></span>

<span data-ttu-id="e11a6-143">As definições de configuração a seguir podem ser usadas para ajustar o servidor para melhorar o desempenho em um aplicativo do SignalR.</span><span class="sxs-lookup"><span data-stu-id="e11a6-143">The following configuration settings can be used to tune your server for better performance in a SignalR application.</span></span> <span data-ttu-id="e11a6-144">Para obter informações gerais sobre como melhorar o desempenho em um aplicativo ASP.NET, consulte [melhorando o desempenho do ASP.NET](https://msdn.microsoft.com/library/ff647787.aspx).</span><span class="sxs-lookup"><span data-stu-id="e11a6-144">For general information on how to improve performance in an ASP.NET application, see [Improving ASP.NET Performance](https://msdn.microsoft.com/library/ff647787.aspx).</span></span>

<span data-ttu-id="e11a6-145">**Definições de configuração de SignalR**</span><span class="sxs-lookup"><span data-stu-id="e11a6-145">**SignalR configuration settings**</span></span>

- <span data-ttu-id="e11a6-146">**DefaultMessageBufferSize**: por padrão, o SignalR retém 1000 mensagens na memória por hub por conexão.</span><span class="sxs-lookup"><span data-stu-id="e11a6-146">**DefaultMessageBufferSize**: By default, SignalR retains 1000 messages in memory per hub per connection.</span></span> <span data-ttu-id="e11a6-147">Se estiverem sendo usada mensagens grandes, isso pode criar problemas de memória que podem ser atenuados por redução desse valor.</span><span class="sxs-lookup"><span data-stu-id="e11a6-147">If large messages are being used, this may create memory issues which can be alleviated by reducing this value.</span></span> <span data-ttu-id="e11a6-148">Essa configuração pode ser definida `Application_Start` manipulador de eventos em um aplicativo ASP.NET ou no `Configuration` método de uma classe de inicialização OWIN em um aplicativo auto-hospedado.</span><span class="sxs-lookup"><span data-stu-id="e11a6-148">This setting can be set in the `Application_Start` event handler in an ASP.NET application, or in the `Configuration` method of an OWIN startup class in a self-hosted application.</span></span> <span data-ttu-id="e11a6-149">O exemplo a seguir demonstra como reduzir esse valor para reduzir o tamanho da memória do seu aplicativo para reduzir a quantidade de memória de servidor usada:</span><span class="sxs-lookup"><span data-stu-id="e11a6-149">The following sample demonstrates how to reduce this value in order to reduce the memory footprint of your application in order to reduce the amount of server memory used:</span></span>

    <span data-ttu-id="e11a6-150">**Código de servidor .NET em Startup.cs para diminuir o tamanho do buffer de mensagem padrão**</span><span class="sxs-lookup"><span data-stu-id="e11a6-150">**.NET server code in Startup.cs for decreasing default message buffer size**</span></span>

    [!code-csharp[Main](signalr-performance/samples/sample3.cs)]

<span data-ttu-id="e11a6-151">**Configurações do IIS**</span><span class="sxs-lookup"><span data-stu-id="e11a6-151">**IIS configuration settings**</span></span>

- <span data-ttu-id="e11a6-152">**Máximo de solicitações simultâneas por aplicativo**: aumentar o número de IIS simultâneo solicitações aumentará de recursos disponíveis para atender às solicitações do servidor.</span><span class="sxs-lookup"><span data-stu-id="e11a6-152">**Max concurrent requests per application**: Increasing the number of concurrent IIS requests will increase server resources available for serving requests.</span></span> <span data-ttu-id="e11a6-153">O valor padrão é 5000; para aumentar essa configuração, execute os seguintes comandos em um prompt de comando elevado:</span><span class="sxs-lookup"><span data-stu-id="e11a6-153">The default value is 5000; to increase this setting, execute the following commands in an elevated command prompt:</span></span>

    [!code-console[Main](signalr-performance/samples/sample4.cmd)]
- <span data-ttu-id="e11a6-154">**ApplicationPool QueueLength**: Este é o número máximo de solicitações HTTP. sys filas de pool de aplicativos.</span><span class="sxs-lookup"><span data-stu-id="e11a6-154">**ApplicationPool QueueLength**: This is the maximum number of requests that Http.sys queues for the application pool.</span></span> <span data-ttu-id="e11a6-155">Quando a fila está cheia, novas solicitações recebem uma resposta 503 "Serviço indisponível".</span><span class="sxs-lookup"><span data-stu-id="e11a6-155">When the queue is full, new requests receive a 503 "Service Unavailable" response.</span></span> <span data-ttu-id="e11a6-156">O valor padrão é 1000.</span><span class="sxs-lookup"><span data-stu-id="e11a6-156">The default value is 1000.</span></span>

    <span data-ttu-id="e11a6-157">Reduzir o comprimento da fila do processo de trabalho no pool de aplicativos hospedando o seu aplicativo para conservar recursos de memória.</span><span class="sxs-lookup"><span data-stu-id="e11a6-157">Shortening the queue length for the worker process in the application pool hosting your application will conserve memory resources.</span></span> <span data-ttu-id="e11a6-158">Para obter mais informações, consulte [Configurando Pools de aplicativos, otimização e gerenciando](https://technet.microsoft.com/library/cc745955.aspx).</span><span class="sxs-lookup"><span data-stu-id="e11a6-158">For more information, see [Managing, Tuning, and Configuring Application Pools](https://technet.microsoft.com/library/cc745955.aspx).</span></span>

<span data-ttu-id="e11a6-159">**Definições de configuração do ASP.NET**</span><span class="sxs-lookup"><span data-stu-id="e11a6-159">**ASP.NET configuration settings**</span></span>

<span data-ttu-id="e11a6-160">Esta seção inclui configurações que podem ser definidas no `aspnet.config` arquivo.</span><span class="sxs-lookup"><span data-stu-id="e11a6-160">This section includes configuration settings that can be set in the `aspnet.config` file.</span></span> <span data-ttu-id="e11a6-161">Esse arquivo está localizado em um dos dois locais, dependendo da plataforma:</span><span class="sxs-lookup"><span data-stu-id="e11a6-161">This file is found in one of two locations, depending on platform:</span></span>

- `%windir%\Microsoft.NET\Framework\v4.0.30319\aspnet.config`
- `%windir%\Microsoft.NET\Framework64\v4.0.30319\aspnet.config`

<span data-ttu-id="e11a6-162">Configurações do ASP.NET que podem melhorar o desempenho do SignalR incluem o seguinte:</span><span class="sxs-lookup"><span data-stu-id="e11a6-162">ASP.NET settings that may improve SignalR performance include the following:</span></span>

- <span data-ttu-id="e11a6-163">**Máximo de solicitações simultâneo por CPU**: aumentar essa configuração pode atenuar os gargalos de desempenho.</span><span class="sxs-lookup"><span data-stu-id="e11a6-163">**Maximum concurrent requests per CPU**: Increasing this setting may alleviate performance bottlenecks.</span></span> <span data-ttu-id="e11a6-164">Para aumentar essa configuração, adicione a seguinte configuração para o `aspnet.config` arquivo:</span><span class="sxs-lookup"><span data-stu-id="e11a6-164">To increase this setting, add the following configuration setting to the `aspnet.config` file:</span></span>

    [!code-xml[Main](signalr-performance/samples/sample5.xml?highlight=4)]
- <span data-ttu-id="e11a6-165">**Limite de fila de solicitações**: quando o número total de conexões excede o `maxConcurrentRequestsPerCPU` configuração, ASP.NET será iniciado usando uma fila de solicitações de limitação.</span><span class="sxs-lookup"><span data-stu-id="e11a6-165">**Request Queue Limit**: When the total number of connections exceeds the `maxConcurrentRequestsPerCPU` setting, ASP.NET will start throttling requests using a queue.</span></span> <span data-ttu-id="e11a6-166">Para aumentar o tamanho da fila, você pode aumentar a `requestQueueLimit` configuração.</span><span class="sxs-lookup"><span data-stu-id="e11a6-166">To increase the size of the queue, you can increase the `requestQueueLimit` setting.</span></span> <span data-ttu-id="e11a6-167">Para fazer isso, adicione a seguinte configuração para o `processModel` nó `config/machine.config` (em vez de `aspnet.config`):</span><span class="sxs-lookup"><span data-stu-id="e11a6-167">To do this, add the following configuration setting to the `processModel` node in `config/machine.config` (rather than `aspnet.config`):</span></span>

    [!code-xml[Main](signalr-performance/samples/sample6.xml)]

<a id="troubleshooting"></a>

## <a name="troubleshooting-performance-issues"></a><span data-ttu-id="e11a6-168">Solucionar problemas de desempenho</span><span class="sxs-lookup"><span data-stu-id="e11a6-168">Troubleshooting performance issues</span></span>

<span data-ttu-id="e11a6-169">Esta seção descreve maneiras de localizar os afunilamentos de desempenho em seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="e11a6-169">This section describes ways to find performance bottlenecks in your application.</span></span>

### <a name="verifying-that-websocket-is-being-used"></a><span data-ttu-id="e11a6-170">Verificando se o WebSocket está sendo usado</span><span class="sxs-lookup"><span data-stu-id="e11a6-170">Verifying that WebSocket is being used</span></span>

<span data-ttu-id="e11a6-171">Enquanto o SignalR pode usar uma variedade de transportes para comunicação entre cliente e servidor, WebSocket oferece uma vantagem de desempenho significativa e deve ser usado se o cliente e o servidor oferecer suporte a ele.</span><span class="sxs-lookup"><span data-stu-id="e11a6-171">While SignalR can use a variety of transports for communication between client and server, WebSocket offers a significant performance advantage, and should be used if the client and server support it.</span></span> <span data-ttu-id="e11a6-172">Para determinar se o cliente e o servidor atendem aos requisitos de WebSocket, consulte [transportes e Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="e11a6-172">To determine if your client and server meet the requirements for WebSocket, see [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span> <span data-ttu-id="e11a6-173">Para determinar qual transporte está sendo usado em seu aplicativo, você pode usar as ferramentas de desenvolvedor do navegador e examine os logs para ver qual transporte está sendo usado para a conexão.</span><span class="sxs-lookup"><span data-stu-id="e11a6-173">To determine what transport is being used in your application, you can use the browser developer tools, and examine the logs to see what transport is being used for the connection.</span></span> <span data-ttu-id="e11a6-174">Para obter informações sobre como usar as ferramentas de desenvolvimento de navegador no Internet Explorer e no Chrome, consulte [transportes e Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="e11a6-174">For information on using the browser development tools in Internet Explorer and Chrome, see [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="perfcounters"></a>

## <a name="using-signalr-performance-counters"></a><span data-ttu-id="e11a6-175">Usando contadores de desempenho do SignalR</span><span class="sxs-lookup"><span data-stu-id="e11a6-175">Using SignalR performance counters</span></span>

<span data-ttu-id="e11a6-176">Esta seção descreve como habilitar e usar contadores de desempenho do SignalR, encontrado no `Microsoft.AspNet.SignalR.Utils` pacote.</span><span class="sxs-lookup"><span data-stu-id="e11a6-176">This section describes how to enable and use SignalR performance counters, found in the `Microsoft.AspNet.SignalR.Utils` package.</span></span>

### <a name="installing-signalrexe"></a><span data-ttu-id="e11a6-177">Instalando signalr.exe</span><span class="sxs-lookup"><span data-stu-id="e11a6-177">Installing signalr.exe</span></span>

<span data-ttu-id="e11a6-178">Contadores de desempenho podem ser adicionados ao servidor usando um utilitário chamado SignalR.exe.</span><span class="sxs-lookup"><span data-stu-id="e11a6-178">Performance counters can be added to the server using a utility called SignalR.exe.</span></span> <span data-ttu-id="e11a6-179">Para instalar este utilitário, siga estas etapas:</span><span class="sxs-lookup"><span data-stu-id="e11a6-179">To install this utility, follow these steps:</span></span>

1. <span data-ttu-id="e11a6-180">Em seu aplicativo do Visual Studio, selecione **ferramentas**, **Gerenciador de biblioteca de pacote**, **gerenciar pacotes NuGet para solução...**</span><span class="sxs-lookup"><span data-stu-id="e11a6-180">In your Visual Studio application, select **Tools**, **Library Package Manager**, **Manage NuGet Packages for Solution...**</span></span>
2. <span data-ttu-id="e11a6-181">Procurar **signalr.utils**e selecione instalar.</span><span class="sxs-lookup"><span data-stu-id="e11a6-181">Search for **signalr.utils**, and select Install.</span></span>

    ![](signalr-performance/_static/image1.png)
3. <span data-ttu-id="e11a6-182">Aceite o contrato de licença para instalar o pacote.</span><span class="sxs-lookup"><span data-stu-id="e11a6-182">Accept the license agreement to install the package.</span></span>
4. <span data-ttu-id="e11a6-183">Será instalado SignalR.exe `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.</span><span class="sxs-lookup"><span data-stu-id="e11a6-183">SignalR.exe will be installed to `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.</span></span>

### <a name="installing-performance-counters-with-signalrexe"></a><span data-ttu-id="e11a6-184">Instalando Contadores de desempenho com SignalR.exe</span><span class="sxs-lookup"><span data-stu-id="e11a6-184">Installing performance counters with SignalR.exe</span></span>

<span data-ttu-id="e11a6-185">Para instalar contadores de desempenho do SignalR, execute SignalR.exe em um prompt de comando com o seguinte parâmetro:</span><span class="sxs-lookup"><span data-stu-id="e11a6-185">To install SignalR performance counters, run SignalR.exe in an elevated command prompt with the following parameter:</span></span>

[!code-console[Main](signalr-performance/samples/sample7.cmd)]

<span data-ttu-id="e11a6-186">Para remover contadores de desempenho do SignalR, execute SignalR.exe em um prompt de comando com o seguinte parâmetro:</span><span class="sxs-lookup"><span data-stu-id="e11a6-186">To remove SignalR performance counters, run SignalR.exe in an elevated command prompt with the following parameter:</span></span>

[!code-console[Main](signalr-performance/samples/sample8.cmd)]

### <a name="signalr-performance-counters"></a><span data-ttu-id="e11a6-187">Contadores de desempenho do SignalR</span><span class="sxs-lookup"><span data-stu-id="e11a6-187">SignalR Performance counters</span></span>

<span data-ttu-id="e11a6-188">O pacote de utilitários instala os seguintes contadores de desempenho.</span><span class="sxs-lookup"><span data-stu-id="e11a6-188">The utilities package installs the following performance counters.</span></span> <span data-ttu-id="e11a6-189">Os contadores de "Total" medem o número de eventos desde o último pool de aplicativos ou o servidor reiniciar.</span><span class="sxs-lookup"><span data-stu-id="e11a6-189">The "Total" counters measure the number of events since the last application pool or server restart.</span></span>

<span data-ttu-id="e11a6-190">**Métricas de Conexão**</span><span class="sxs-lookup"><span data-stu-id="e11a6-190">**Connection metrics**</span></span>

<span data-ttu-id="e11a6-191">As seguintes métricas de medem os eventos de tempo de vida de conexão que ocorrem.</span><span class="sxs-lookup"><span data-stu-id="e11a6-191">The following metrics measure the connection lifetime events that occur.</span></span> <span data-ttu-id="e11a6-192">Para obter mais informações, consulte [compreensão e tratamento de eventos de tempo de vida de Conexão](../guide-to-the-api/handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="e11a6-192">For more information, see [Understanding and Handling Connection Lifetime Events](../guide-to-the-api/handling-connection-lifetime-events.md).</span></span>

- <span data-ttu-id="e11a6-193">**Conexões conectadas**</span><span class="sxs-lookup"><span data-stu-id="e11a6-193">**Connections Connected**</span></span>
- <span data-ttu-id="e11a6-194">**Conexões reconectados**</span><span class="sxs-lookup"><span data-stu-id="e11a6-194">**Connections Reconnected**</span></span>
- <span data-ttu-id="e11a6-195">**Conexões desconectadas**</span><span class="sxs-lookup"><span data-stu-id="e11a6-195">**Connections Disconnected**</span></span>
- <span data-ttu-id="e11a6-196">**Conexões atuais**</span><span class="sxs-lookup"><span data-stu-id="e11a6-196">**Connections Current**</span></span>

<span data-ttu-id="e11a6-197">**Métricas de mensagem**</span><span class="sxs-lookup"><span data-stu-id="e11a6-197">**Message metrics**</span></span>

<span data-ttu-id="e11a6-198">As seguintes métricas de medem o tráfego de mensagens gerado pelo SignalR.</span><span class="sxs-lookup"><span data-stu-id="e11a6-198">The following metrics measure the message traffic generated by SignalR.</span></span>

- <span data-ttu-id="e11a6-199">**Total de mensagens recebidas de Conexão**</span><span class="sxs-lookup"><span data-stu-id="e11a6-199">**Connection Messages Received Total**</span></span>
- <span data-ttu-id="e11a6-200">**Total de mensagens de Conexão enviadas**</span><span class="sxs-lookup"><span data-stu-id="e11a6-200">**Connection Messages Sent Total**</span></span>
- <span data-ttu-id="e11a6-201">**Conexão de mensagens recebidos/s**</span><span class="sxs-lookup"><span data-stu-id="e11a6-201">**Connection Messages Received/Sec**</span></span>
- <span data-ttu-id="e11a6-202">**Conexão de mensagens enviadas/s**</span><span class="sxs-lookup"><span data-stu-id="e11a6-202">**Connection Messages Sent/Sec**</span></span>

<span data-ttu-id="e11a6-203">**Métricas do barramento de mensagem**</span><span class="sxs-lookup"><span data-stu-id="e11a6-203">**Message bus metrics**</span></span>

<span data-ttu-id="e11a6-204">As seguintes métricas de medem o tráfego por meio do barramento de mensagem SignalR interno, a fila na qual todas as mensagens recebidas e enviadas para o SignalR são colocadas.</span><span class="sxs-lookup"><span data-stu-id="e11a6-204">The following metrics measure traffic through the internal SignalR message bus, the queue in which all incoming and outgoing SignalR messages are placed.</span></span> <span data-ttu-id="e11a6-205">Uma mensagem é **publicado** quando ele é enviado ou difusão.</span><span class="sxs-lookup"><span data-stu-id="e11a6-205">A message is **Published** when it is sent or broadcast.</span></span> <span data-ttu-id="e11a6-206">Um **assinante** neste contexto é uma assinatura do barramento de mensagem; isso deve ser igual ao número de clientes mais o próprio servidor.</span><span class="sxs-lookup"><span data-stu-id="e11a6-206">A **Subscriber** in this context is a subscription on the message bus; this should equal the number of clients plus the server itself.</span></span> <span data-ttu-id="e11a6-207">Um **trabalho alocado** é um componente que envia dados para as conexões ativas; um **trabalho ocupado** é aquele que está enviando uma mensagem ativamente.</span><span class="sxs-lookup"><span data-stu-id="e11a6-207">An **Allocated Worker** is a component that sends data to active connections; a **Busy Worker** is one that is actively sending a message.</span></span>

- <span data-ttu-id="e11a6-208">**Recebida Total de mensagens do barramento de mensagem**</span><span class="sxs-lookup"><span data-stu-id="e11a6-208">**Message Bus Messages Received Total**</span></span>
- <span data-ttu-id="e11a6-209">**Mensagens de barramento de mensagem recebidos/s**</span><span class="sxs-lookup"><span data-stu-id="e11a6-209">**Message Bus Messages Received/Sec**</span></span>
- <span data-ttu-id="e11a6-210">**Mensagens do barramento de mensagem publicado Total**</span><span class="sxs-lookup"><span data-stu-id="e11a6-210">**Message Bus Messages Published Total**</span></span>
- <span data-ttu-id="e11a6-211">**Barramento de mensagem mensagens publicadas/s**</span><span class="sxs-lookup"><span data-stu-id="e11a6-211">**Message Bus Messages Published/Sec**</span></span>
- <span data-ttu-id="e11a6-212">**Atual de assinantes do barramento de mensagem**</span><span class="sxs-lookup"><span data-stu-id="e11a6-212">**Message Bus Subscribers Current**</span></span>
- <span data-ttu-id="e11a6-213">**Total de assinantes do barramento de mensagem**</span><span class="sxs-lookup"><span data-stu-id="e11a6-213">**Message Bus Subscribers Total**</span></span>
- <span data-ttu-id="e11a6-214">**Os assinantes do barramento de mensagem/s**</span><span class="sxs-lookup"><span data-stu-id="e11a6-214">**Message Bus Subscribers/Sec**</span></span>
- <span data-ttu-id="e11a6-215">**Barramento de mensagem alocada trabalhadores**</span><span class="sxs-lookup"><span data-stu-id="e11a6-215">**Message Bus Allocated Workers**</span></span>
- <span data-ttu-id="e11a6-216">**Operadores ocupados de barramento de mensagem**</span><span class="sxs-lookup"><span data-stu-id="e11a6-216">**Message Bus Busy Workers**</span></span>
- <span data-ttu-id="e11a6-217">**Atual de tópicos do barramento de mensagem**</span><span class="sxs-lookup"><span data-stu-id="e11a6-217">**Message Bus Topics Current**</span></span>

<span data-ttu-id="e11a6-218">**Métricas de erro**</span><span class="sxs-lookup"><span data-stu-id="e11a6-218">**Error metrics**</span></span>

<span data-ttu-id="e11a6-219">As seguintes métricas de medem os erros gerados pelo tráfego de mensagens do SignalR.</span><span class="sxs-lookup"><span data-stu-id="e11a6-219">The following metrics measure errors generated by SignalR message traffic.</span></span> <span data-ttu-id="e11a6-220">**Resolução de hub** erros ocorrem quando um hub ou um método de hub não pode ser resolvido.</span><span class="sxs-lookup"><span data-stu-id="e11a6-220">**Hub Resolution** errors occur when a hub or hub method cannot be resolved.</span></span> <span data-ttu-id="e11a6-221">**Invocação de hub** erros são exceções geradas durante a invocação de um método de hub.</span><span class="sxs-lookup"><span data-stu-id="e11a6-221">**Hub Invocation** errors are exceptions thrown while invoking a hub method.</span></span> <span data-ttu-id="e11a6-222">**Transporte** erros são erros de conexão gerados durante uma solicitação ou resposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="e11a6-222">**Transport** errors are connection errors thrown during an HTTP request or response.</span></span>

- <span data-ttu-id="e11a6-223">**Erros: Total de todos os**</span><span class="sxs-lookup"><span data-stu-id="e11a6-223">**Errors: All Total**</span></span>
- <span data-ttu-id="e11a6-224">**Erros: All/s**</span><span class="sxs-lookup"><span data-stu-id="e11a6-224">**Errors: All/Sec**</span></span>
- <span data-ttu-id="e11a6-225">**Erros: Total de resolução de Hub**</span><span class="sxs-lookup"><span data-stu-id="e11a6-225">**Errors: Hub Resolution Total**</span></span>
- <span data-ttu-id="e11a6-226">**Erros: Resolução de Hub/s**</span><span class="sxs-lookup"><span data-stu-id="e11a6-226">**Errors: Hub Resolution/Sec**</span></span>
- <span data-ttu-id="e11a6-227">**Erros: Total de invocação de Hub**</span><span class="sxs-lookup"><span data-stu-id="e11a6-227">**Errors: Hub Invocation Total**</span></span>
- <span data-ttu-id="e11a6-228">**Erros: Invocação de Hub / s**</span><span class="sxs-lookup"><span data-stu-id="e11a6-228">**Errors: Hub Invocation/Sec**</span></span>
- <span data-ttu-id="e11a6-229">**Erros: Total de transporte**</span><span class="sxs-lookup"><span data-stu-id="e11a6-229">**Errors: Transport Total**</span></span>
- <span data-ttu-id="e11a6-230">**Erros: Transporte/s**</span><span class="sxs-lookup"><span data-stu-id="e11a6-230">**Errors: Transport/Sec**</span></span>

<a id="scaleout_metrics"></a>

<span data-ttu-id="e11a6-231">**Métricas de expansão**</span><span class="sxs-lookup"><span data-stu-id="e11a6-231">**Scaleout metrics**</span></span>

<span data-ttu-id="e11a6-232">As seguintes métricas medem o tráfego e os erros gerados pelo provedor de expansão.</span><span class="sxs-lookup"><span data-stu-id="e11a6-232">The following metrics measure traffic and errors generated by the scaleout provider.</span></span> <span data-ttu-id="e11a6-233">Um **fluxo** neste contexto é uma unidade de escala usada pelo provedor de expansão; essa é uma tabela se o SQL Server é usado, um tópico que se o barramento de serviço é usado e uma assinatura se Redis for usado.</span><span class="sxs-lookup"><span data-stu-id="e11a6-233">A **Stream** in this context is a scale unit used by the scaleout provider; this is a table if SQL Server is used, a Topic if Service Bus is used, and a Subscription if Redis is used.</span></span> <span data-ttu-id="e11a6-234">Cada fluxo garante ordenada operações de leitura e gravação; um único fluxo é um gargalo potencial na escala, para que o número de fluxos pode ser aumentado para ajudar a reduzir esse afunilamento.</span><span class="sxs-lookup"><span data-stu-id="e11a6-234">Each stream ensures ordered read and write operations; a single stream is a potential scale bottleneck, so the number of streams can be increased to help reduce that bottleneck.</span></span> <span data-ttu-id="e11a6-235">Se forem usados vários fluxos, SignalR distribuirá automaticamente mensagens (fragmento) entre esses fluxos de forma que garante que as mensagens enviadas de qualquer conexão determinado estão em ordem.</span><span class="sxs-lookup"><span data-stu-id="e11a6-235">If multiple streams are used, SignalR will automatically distribute (shard) messages across these streams in a way that ensures messages sent from any given connection are in order.</span></span>

<span data-ttu-id="e11a6-236">O [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx) determina o comprimento da fila de envio de expansão mantida pelo SignalR.</span><span class="sxs-lookup"><span data-stu-id="e11a6-236">The [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx) setting controls the length of the scaleout send queue maintained by SignalR.</span></span> <span data-ttu-id="e11a6-237">Defini-la como um valor maior que 0 colocará todas as mensagens em uma fila de envio a ser enviado um de cada vez ao backplane de mensagens configurado.</span><span class="sxs-lookup"><span data-stu-id="e11a6-237">Setting it to a value greater than 0 will place all messages in a send queue to be sent one at a time to the configured messaging backplane.</span></span> <span data-ttu-id="e11a6-238">Se o tamanho da fila ficar acima do tamanho configurado, as chamadas subsequentes para enviar será imediatamente falharem com um [InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception(v=vs.118).aspx) até que o número de mensagens na fila é menor do que a configuração novamente.</span><span class="sxs-lookup"><span data-stu-id="e11a6-238">If the size of the queue goes above the configured length, subsequent calls to send will fail immediately with an [InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception(v=vs.118).aspx) until the number of messages in the queue is less than the setting again.</span></span> <span data-ttu-id="e11a6-239">Enfileiramento está desabilitado por padrão porque os painéis posteriores implementados geralmente têm seus próprios enfileiramento de mensagens ou o controle de fluxo em vigor.</span><span class="sxs-lookup"><span data-stu-id="e11a6-239">Queueing is disabled by default because the implemented backplanes generally have their own queuing or flow-control in place.</span></span> <span data-ttu-id="e11a6-240">No caso do SQL Server, a pooling de conexão efetivamente limita o número de envios acontecendo a qualquer momento.</span><span class="sxs-lookup"><span data-stu-id="e11a6-240">In the case of SQL Server, connection pooling effectively limits the number of sends going on at any one time.</span></span>

<span data-ttu-id="e11a6-241">Por padrão, apenas um fluxo é usado para o SQL Server e do Redis, cinco fluxos são usados para o barramento de serviço e enfileiramento está desabilitado, mas essas configurações podem ser alteradas por meio de configuração no SQL Server e o barramento de serviço:</span><span class="sxs-lookup"><span data-stu-id="e11a6-241">By default, only one stream is used for SQL Server and Redis, five streams are used for Service Bus, and queueing is disabled, but these settings can be changed through configuration on SQL Server and Service Bus:</span></span>

<span data-ttu-id="e11a6-242">**Código de servidor .NET para configurar o comprimento da fila e contagem de tabela para backplane do SQL Server**</span><span class="sxs-lookup"><span data-stu-id="e11a6-242">**.NET Server Code for configuring table count and queue length for SQL Server backplane**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample9.cs)]

<span data-ttu-id="e11a6-243">**Código de servidor .NET para configurar o comprimento de fila e contagem de tópico para backplane de barramento de serviço**</span><span class="sxs-lookup"><span data-stu-id="e11a6-243">**.NET Server Code for configuring topic count and queue length for Service Bus backplane**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample10.cs)]

<span data-ttu-id="e11a6-244">Um **buffer** fluxo é aquele que entrou em um estado de falha; quando o fluxo está no estado de falha, todas as mensagens enviadas ao backplane falhará imediatamente até que o fluxo não está falhando.</span><span class="sxs-lookup"><span data-stu-id="e11a6-244">A **Buffering** stream is one that has entered a faulted state; when the stream is in the faulted state, all messages sent to the backplane will fail immediately until the stream is no longer faulting.</span></span> <span data-ttu-id="e11a6-245">O **comprimento da fila de envio** é o número de mensagens que foram lançadas, mas ainda não enviadas.</span><span class="sxs-lookup"><span data-stu-id="e11a6-245">The **Send Queue Length** is the number of messages that have been posted but not yet sent.</span></span>

- <span data-ttu-id="e11a6-246">**Barramento de mensagem escalável mensagens recebidos/s**</span><span class="sxs-lookup"><span data-stu-id="e11a6-246">**Scaleout Message Bus Messages Received/Sec**</span></span>
- <span data-ttu-id="e11a6-247">**Total de fluxos de expansão**</span><span class="sxs-lookup"><span data-stu-id="e11a6-247">**Scaleout Streams Total**</span></span>
- <span data-ttu-id="e11a6-248">**Abrir de fluxos de expansão**</span><span class="sxs-lookup"><span data-stu-id="e11a6-248">**Scaleout Streams Open**</span></span>
- <span data-ttu-id="e11a6-249">**Fluxos de expansão de buffer**</span><span class="sxs-lookup"><span data-stu-id="e11a6-249">**Scaleout Streams Buffering**</span></span>
- <span data-ttu-id="e11a6-250">**Total de erros de expansão**</span><span class="sxs-lookup"><span data-stu-id="e11a6-250">**Scaleout Errors Total**</span></span>
- <span data-ttu-id="e11a6-251">**Erros de expansão/s**</span><span class="sxs-lookup"><span data-stu-id="e11a6-251">**Scaleout Errors/Sec**</span></span>
- <span data-ttu-id="e11a6-252">**Comprimento da fila de envio de expansão**</span><span class="sxs-lookup"><span data-stu-id="e11a6-252">**Scaleout Send Queue Length**</span></span>

<span data-ttu-id="e11a6-253">Para obter mais informações sobre o que está sendo medida esses contadores, consulte [SignalR Scaleout com o Azure Service Bus](scaleout-with-windows-azure-service-bus.md).</span><span class="sxs-lookup"><span data-stu-id="e11a6-253">For more information on what these counters are measuring, see [SignalR Scaleout with Azure Service Bus](scaleout-with-windows-azure-service-bus.md).</span></span>

<a id="othercounters"></a>

## <a name="using-other-performance-counters"></a><span data-ttu-id="e11a6-254">Usando outros contadores de desempenho</span><span class="sxs-lookup"><span data-stu-id="e11a6-254">Using other performance counters</span></span>

<span data-ttu-id="e11a6-255">Os seguintes contadores de desempenho também podem ser útil no monitoramento do desempenho do seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="e11a6-255">The following performance counters may also be useful in monitoring your application's performance.</span></span>

<span data-ttu-id="e11a6-256">**Memória**</span><span class="sxs-lookup"><span data-stu-id="e11a6-256">**Memory**</span></span>

- <span data-ttu-id="e11a6-257">.NET CLR Memory\\# bytes em todos os Heaps (para w3wp)</span><span class="sxs-lookup"><span data-stu-id="e11a6-257">.NET CLR Memory\\# bytes in all Heaps (for w3wp)</span></span>

<span data-ttu-id="e11a6-258">**ASP.NET**</span><span class="sxs-lookup"><span data-stu-id="e11a6-258">**ASP.NET**</span></span>

- <span data-ttu-id="e11a6-259">ASP.NET \ solicitações atuais</span><span class="sxs-lookup"><span data-stu-id="e11a6-259">ASP.NET\Requests Current</span></span>
- <span data-ttu-id="e11a6-260">ASP.NET\Queued</span><span class="sxs-lookup"><span data-stu-id="e11a6-260">ASP.NET\Queued</span></span>
- <span data-ttu-id="e11a6-261">ASP.NET\Rejected</span><span class="sxs-lookup"><span data-stu-id="e11a6-261">ASP.NET\Rejected</span></span>

<span data-ttu-id="e11a6-262">**CPU**</span><span class="sxs-lookup"><span data-stu-id="e11a6-262">**CPU**</span></span>

- <span data-ttu-id="e11a6-263">Tempo do processador Information\Processor</span><span class="sxs-lookup"><span data-stu-id="e11a6-263">Processor Information\Processor Time</span></span>

<span data-ttu-id="e11a6-264">**TCP/IP**</span><span class="sxs-lookup"><span data-stu-id="e11a6-264">**TCP/IP**</span></span>

- <span data-ttu-id="e11a6-265">TCPv6/conexões estabelecidas</span><span class="sxs-lookup"><span data-stu-id="e11a6-265">TCPv6/Connections Established</span></span>
- <span data-ttu-id="e11a6-266">TCPv4/conexões estabelecidas</span><span class="sxs-lookup"><span data-stu-id="e11a6-266">TCPv4/Connections Established</span></span>

<span data-ttu-id="e11a6-267">**Serviço Web**</span><span class="sxs-lookup"><span data-stu-id="e11a6-267">**Web Service**</span></span>

- <span data-ttu-id="e11a6-268">Web \ conexões atuais</span><span class="sxs-lookup"><span data-stu-id="e11a6-268">Web Service\Current Connections</span></span>
- <span data-ttu-id="e11a6-269">Conexões de Service\Maximum da Web</span><span class="sxs-lookup"><span data-stu-id="e11a6-269">Web Service\Maximum Connections</span></span>

<span data-ttu-id="e11a6-270">**Threading**</span><span class="sxs-lookup"><span data-stu-id="e11a6-270">**Threading**</span></span>

- <span data-ttu-id="e11a6-271">.NET CLR bloqueios e segmentos\\n º de segmentos lógicos atuais</span><span class="sxs-lookup"><span data-stu-id="e11a6-271">.NET CLR Locks And Threads\\# of current logical Threads</span></span>
- <span data-ttu-id="e11a6-272">.NET CLR bloqueios e segmentos\\n º de segmentos físicos atuais</span><span class="sxs-lookup"><span data-stu-id="e11a6-272">.NET CLR Locks And Threads\\# of current physical Threads</span></span>

<a id="otherresources"></a>

## <a name="other-resources"></a><span data-ttu-id="e11a6-273">Outros recursos</span><span class="sxs-lookup"><span data-stu-id="e11a6-273">Other Resources</span></span>

<span data-ttu-id="e11a6-274">Para obter mais informações sobre monitoramento e ajuste de desempenho do ASP.NET, consulte os tópicos a seguir:</span><span class="sxs-lookup"><span data-stu-id="e11a6-274">For more information on ASP.NET performance monitoring and tuning, see the following topics:</span></span>

- <span data-ttu-id="e11a6-275">[Visão geral do desempenho do ASP.NET](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)</span><span class="sxs-lookup"><span data-stu-id="e11a6-275">[ASP.NET Performance Overview](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)</span></span>
- [<span data-ttu-id="e11a6-276">Uso de Thread do ASP.NET no IIS 6.0, IIS 7.5 e IIS 7.0</span><span class="sxs-lookup"><span data-stu-id="e11a6-276">ASP.NET Thread Usage on IIS 7.5, IIS 7.0, and IIS 6.0</span></span>](https://blogs.msdn.com/b/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx)
- [<span data-ttu-id="e11a6-277">&lt;applicationPool&gt; elemento (configurações da Web)</span><span class="sxs-lookup"><span data-stu-id="e11a6-277">&lt;applicationPool&gt; Element (Web Settings)</span></span>](https://msdn.microsoft.com/library/dd560842.aspx)

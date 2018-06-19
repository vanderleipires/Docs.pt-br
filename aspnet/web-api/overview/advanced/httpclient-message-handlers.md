---
uid: web-api/overview/advanced/httpclient-message-handlers
title: Manipuladores de mensagens de HttpClient na API da Web ASP.NET | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2012
ms.topic: article
ms.assetid: 5a4b6c80-b2e9-4710-8969-d5076f7f82b8
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/httpclient-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: 805741b0ac682b7479ce82127df48b1b9a49a427
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
ms.locfileid: "26506785"
---
<a name="httpclient-message-handlers-in-aspnet-web-api"></a><span data-ttu-id="1a2e2-102">Manipuladores de mensagens de HttpClient na API da Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="1a2e2-102">HttpClient Message Handlers in ASP.NET Web API</span></span>
====================
<span data-ttu-id="1a2e2-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="1a2e2-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="1a2e2-104">Um *manipulador de mensagens* é uma classe que recebe uma solicitação HTTP e retorna uma resposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="1a2e2-104">A *message handler* is a class that receives an HTTP request and returns an HTTP response.</span></span>

<span data-ttu-id="1a2e2-105">Normalmente, uma série de manipuladores de mensagens são encadeados juntos.</span><span class="sxs-lookup"><span data-stu-id="1a2e2-105">Typically, a series of message handlers are chained together.</span></span> <span data-ttu-id="1a2e2-106">O primeiro manipulador recebe uma solicitação HTTP, executa algum processamento e fornece a solicitação para o manipulador de Avançar.</span><span class="sxs-lookup"><span data-stu-id="1a2e2-106">The first handler receives an HTTP request, does some processing, and gives the request to the next handler.</span></span> <span data-ttu-id="1a2e2-107">Em algum momento, a resposta é criada e retorna a cadeia.</span><span class="sxs-lookup"><span data-stu-id="1a2e2-107">At some point, the response is created and goes back up the chain.</span></span> <span data-ttu-id="1a2e2-108">Esse padrão é chamado um *delegando* manipulador.</span><span class="sxs-lookup"><span data-stu-id="1a2e2-108">This pattern is called a *delegating* handler.</span></span>

![](httpclient-message-handlers/_static/image1.png)

<span data-ttu-id="1a2e2-109">No lado do cliente, o **HttpClient** classe usa um manipulador de mensagens para processar solicitações.</span><span class="sxs-lookup"><span data-stu-id="1a2e2-109">On the client side, the **HttpClient** class uses a message handler to process requests.</span></span> <span data-ttu-id="1a2e2-110">O manipulador padrão é **HttpClientHandler**, que envia a solicitação através da rede e obtém a resposta do servidor.</span><span class="sxs-lookup"><span data-stu-id="1a2e2-110">The default handler is **HttpClientHandler**, which sends the request over the network and gets the response from the server.</span></span> <span data-ttu-id="1a2e2-111">Você pode inserir os manipuladores de mensagens personalizadas no pipeline de cliente:</span><span class="sxs-lookup"><span data-stu-id="1a2e2-111">You can insert custom message handlers into the client pipeline:</span></span>

![](httpclient-message-handlers/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="1a2e2-112">ASP.NET Web API também utiliza manipuladores de mensagens no lado do servidor.</span><span class="sxs-lookup"><span data-stu-id="1a2e2-112">ASP.NET Web API also uses message handlers on the server side.</span></span> <span data-ttu-id="1a2e2-113">Para obter mais informações, consulte [manipuladores de mensagens HTTP](http-message-handlers.md).</span><span class="sxs-lookup"><span data-stu-id="1a2e2-113">For more information, see [HTTP Message Handlers](http-message-handlers.md).</span></span>


## <a name="custom-message-handlers"></a><span data-ttu-id="1a2e2-114">Manipuladores de mensagens personalizadas</span><span class="sxs-lookup"><span data-stu-id="1a2e2-114">Custom Message Handlers</span></span>

<span data-ttu-id="1a2e2-115">Para escrever um manipulador de mensagens personalizadas, derivam **System.Net.Http.DelegatingHandler** e substituir o **SendAsync** método.</span><span class="sxs-lookup"><span data-stu-id="1a2e2-115">To write a custom message handler, derive from **System.Net.Http.DelegatingHandler** and override the **SendAsync** method.</span></span> <span data-ttu-id="1a2e2-116">Aqui está a assinatura do método:</span><span class="sxs-lookup"><span data-stu-id="1a2e2-116">Here is the method signature:</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample1.cs)]

<span data-ttu-id="1a2e2-117">O método usa um **HttpRequestMessage** como entrada e retorna de maneira assíncrona um **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="1a2e2-117">The method takes an **HttpRequestMessage** as input and asynchronously returns an **HttpResponseMessage**.</span></span> <span data-ttu-id="1a2e2-118">Uma implementação típica faz o seguinte:</span><span class="sxs-lookup"><span data-stu-id="1a2e2-118">A typical implementation does the following:</span></span>

1. <span data-ttu-id="1a2e2-119">Processe a mensagem de solicitação.</span><span class="sxs-lookup"><span data-stu-id="1a2e2-119">Process the request message.</span></span>
2. <span data-ttu-id="1a2e2-120">Chamar `base.SendAsync` para enviar a solicitação para o manipulador interno.</span><span class="sxs-lookup"><span data-stu-id="1a2e2-120">Call `base.SendAsync` to send the request to the inner handler.</span></span>
3. <span data-ttu-id="1a2e2-121">O manipulador interno retorna uma mensagem de resposta.</span><span class="sxs-lookup"><span data-stu-id="1a2e2-121">The inner handler returns a response message.</span></span> <span data-ttu-id="1a2e2-122">(Esta etapa é assíncrona).</span><span class="sxs-lookup"><span data-stu-id="1a2e2-122">(This step is asynchronous.)</span></span>
4. <span data-ttu-id="1a2e2-123">Processar a resposta e retorná-lo ao chamador.</span><span class="sxs-lookup"><span data-stu-id="1a2e2-123">Process the response and return it to the caller.</span></span>

<span data-ttu-id="1a2e2-124">O exemplo a seguir mostra um manipulador de mensagens que adiciona um cabeçalho personalizado para a solicitação de saída:</span><span class="sxs-lookup"><span data-stu-id="1a2e2-124">The following example shows a message handler that adds a custom header to the outgoing request:</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample2.cs)]

<span data-ttu-id="1a2e2-125">A chamada para `base.SendAsync` é assíncrona.</span><span class="sxs-lookup"><span data-stu-id="1a2e2-125">The call to `base.SendAsync` is asynchronous.</span></span> <span data-ttu-id="1a2e2-126">Se o manipulador faz qualquer trabalho após essa chamada, use o **await** palavra-chave para retomar a execução depois que o método é concluído.</span><span class="sxs-lookup"><span data-stu-id="1a2e2-126">If the handler does any work after this call, use the **await** keyword to resume execution after the method completes.</span></span> <span data-ttu-id="1a2e2-127">O exemplo a seguir mostra um manipulador que registra os códigos de erro.</span><span class="sxs-lookup"><span data-stu-id="1a2e2-127">The following example shows a handler that logs error codes.</span></span> <span data-ttu-id="1a2e2-128">O registro em log em si não é muito interessante, mas o exemplo mostra como obter a resposta dentro do manipulador.</span><span class="sxs-lookup"><span data-stu-id="1a2e2-128">The logging itself is not very interesting, but the example shows how to get at the response inside the handler.</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample3.cs?highlight=10,13)]

## <a name="adding-message-handlers-to-the-client-pipeline"></a><span data-ttu-id="1a2e2-129">Adicionando manipuladores de mensagens para o Pipeline de cliente</span><span class="sxs-lookup"><span data-stu-id="1a2e2-129">Adding Message Handlers to the Client Pipeline</span></span>

<span data-ttu-id="1a2e2-130">Para adicionar manipuladores personalizados para **HttpClient**, use o **HttpClientFactory.Create** método:</span><span class="sxs-lookup"><span data-stu-id="1a2e2-130">To add custom handlers to **HttpClient**, use the **HttpClientFactory.Create** method:</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample4.cs)]

<span data-ttu-id="1a2e2-131">Manipuladores de mensagens são chamados na ordem em que você os transmite para o **criar** método.</span><span class="sxs-lookup"><span data-stu-id="1a2e2-131">Message handlers are called in the order that you pass them into the **Create** method.</span></span> <span data-ttu-id="1a2e2-132">Porque manipuladores são aninhados, a mensagem de resposta são transferidos na outra direção.</span><span class="sxs-lookup"><span data-stu-id="1a2e2-132">Because handlers are nested, the response message travels in the other direction.</span></span> <span data-ttu-id="1a2e2-133">Ou seja, o último manipulador é a primeira a receber a mensagem de resposta.</span><span class="sxs-lookup"><span data-stu-id="1a2e2-133">That is, the last handler is the first to get the response message.</span></span>

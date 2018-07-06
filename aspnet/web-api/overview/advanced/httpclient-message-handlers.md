---
uid: web-api/overview/advanced/httpclient-message-handlers
title: Manipuladores de mensagens de HttpClient na API Web ASP.NET | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 10/01/2012
ms.assetid: 5a4b6c80-b2e9-4710-8969-d5076f7f82b8
msc.legacyurl: /web-api/overview/advanced/httpclient-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: db9edcf4fb31e967c3d4e7635f96c68829aec97d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37831366"
---
<a name="httpclient-message-handlers-in-aspnet-web-api"></a><span data-ttu-id="d12f3-102">Manipuladores de mensagens de HttpClient na API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="d12f3-102">HttpClient Message Handlers in ASP.NET Web API</span></span>
====================
<span data-ttu-id="d12f3-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="d12f3-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="d12f3-104">Um *manipulador de mensagens* é uma classe que recebe uma solicitação HTTP e retorna uma resposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="d12f3-104">A *message handler* is a class that receives an HTTP request and returns an HTTP response.</span></span>

<span data-ttu-id="d12f3-105">Normalmente, uma série de manipuladores de mensagens são encadeados juntos.</span><span class="sxs-lookup"><span data-stu-id="d12f3-105">Typically, a series of message handlers are chained together.</span></span> <span data-ttu-id="d12f3-106">O primeiro manipulador recebe uma solicitação HTTP, executa algum processamento e fornece a solicitação para o próximo manipulador.</span><span class="sxs-lookup"><span data-stu-id="d12f3-106">The first handler receives an HTTP request, does some processing, and gives the request to the next handler.</span></span> <span data-ttu-id="d12f3-107">Em algum momento, a resposta é criada e retorna a cadeia.</span><span class="sxs-lookup"><span data-stu-id="d12f3-107">At some point, the response is created and goes back up the chain.</span></span> <span data-ttu-id="d12f3-108">Esse padrão é chamado de um *delegando* manipulador.</span><span class="sxs-lookup"><span data-stu-id="d12f3-108">This pattern is called a *delegating* handler.</span></span>

![](httpclient-message-handlers/_static/image1.png)

<span data-ttu-id="d12f3-109">No lado do cliente, o **HttpClient** classe usa um manipulador de mensagens para processar solicitações.</span><span class="sxs-lookup"><span data-stu-id="d12f3-109">On the client side, the **HttpClient** class uses a message handler to process requests.</span></span> <span data-ttu-id="d12f3-110">O manipulador padrão é **HttpClientHandler**, que envia a solicitação pela rede e obtém a resposta do servidor.</span><span class="sxs-lookup"><span data-stu-id="d12f3-110">The default handler is **HttpClientHandler**, which sends the request over the network and gets the response from the server.</span></span> <span data-ttu-id="d12f3-111">Você pode inserir os manipuladores de mensagens personalizadas no pipeline do cliente:</span><span class="sxs-lookup"><span data-stu-id="d12f3-111">You can insert custom message handlers into the client pipeline:</span></span>

![](httpclient-message-handlers/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="d12f3-112">API Web ASP.NET também usa os manipuladores de mensagens no lado do servidor.</span><span class="sxs-lookup"><span data-stu-id="d12f3-112">ASP.NET Web API also uses message handlers on the server side.</span></span> <span data-ttu-id="d12f3-113">Para obter mais informações, consulte [manipuladores de mensagens HTTP](http-message-handlers.md).</span><span class="sxs-lookup"><span data-stu-id="d12f3-113">For more information, see [HTTP Message Handlers](http-message-handlers.md).</span></span>


## <a name="custom-message-handlers"></a><span data-ttu-id="d12f3-114">Manipuladores de mensagens personalizado</span><span class="sxs-lookup"><span data-stu-id="d12f3-114">Custom Message Handlers</span></span>

<span data-ttu-id="d12f3-115">Para escrever um manipulador de mensagens personalizadas, derivam **System.Net.Http.DelegatingHandler** e substitua o **SendAsync** método.</span><span class="sxs-lookup"><span data-stu-id="d12f3-115">To write a custom message handler, derive from **System.Net.Http.DelegatingHandler** and override the **SendAsync** method.</span></span> <span data-ttu-id="d12f3-116">Aqui está a assinatura do método:</span><span class="sxs-lookup"><span data-stu-id="d12f3-116">Here is the method signature:</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample1.cs)]

<span data-ttu-id="d12f3-117">O método utiliza um **HttpRequestMessage** como entrada e retorna de maneira assíncrona uma **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="d12f3-117">The method takes an **HttpRequestMessage** as input and asynchronously returns an **HttpResponseMessage**.</span></span> <span data-ttu-id="d12f3-118">Uma implementação típica faz o seguinte:</span><span class="sxs-lookup"><span data-stu-id="d12f3-118">A typical implementation does the following:</span></span>

1. <span data-ttu-id="d12f3-119">Processe a mensagem de solicitação.</span><span class="sxs-lookup"><span data-stu-id="d12f3-119">Process the request message.</span></span>
2. <span data-ttu-id="d12f3-120">Chamar `base.SendAsync` para enviar a solicitação para o manipulador interno.</span><span class="sxs-lookup"><span data-stu-id="d12f3-120">Call `base.SendAsync` to send the request to the inner handler.</span></span>
3. <span data-ttu-id="d12f3-121">O manipulador interno retorna uma mensagem de resposta.</span><span class="sxs-lookup"><span data-stu-id="d12f3-121">The inner handler returns a response message.</span></span> <span data-ttu-id="d12f3-122">(Essa etapa é assíncrona).</span><span class="sxs-lookup"><span data-stu-id="d12f3-122">(This step is asynchronous.)</span></span>
4. <span data-ttu-id="d12f3-123">Processar a resposta e retorná-lo ao chamador.</span><span class="sxs-lookup"><span data-stu-id="d12f3-123">Process the response and return it to the caller.</span></span>

<span data-ttu-id="d12f3-124">O exemplo a seguir mostra um manipulador de mensagens que adiciona um cabeçalho personalizado para a solicitação de saída:</span><span class="sxs-lookup"><span data-stu-id="d12f3-124">The following example shows a message handler that adds a custom header to the outgoing request:</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample2.cs)]

<span data-ttu-id="d12f3-125">A chamada para `base.SendAsync` é assíncrona.</span><span class="sxs-lookup"><span data-stu-id="d12f3-125">The call to `base.SendAsync` is asynchronous.</span></span> <span data-ttu-id="d12f3-126">Se o manipulador faz qualquer trabalho após esta chamada, use o **await** palavra-chave para retomar a execução após a conclusão do método.</span><span class="sxs-lookup"><span data-stu-id="d12f3-126">If the handler does any work after this call, use the **await** keyword to resume execution after the method completes.</span></span> <span data-ttu-id="d12f3-127">O exemplo a seguir mostra um manipulador que registra os códigos de erro.</span><span class="sxs-lookup"><span data-stu-id="d12f3-127">The following example shows a handler that logs error codes.</span></span> <span data-ttu-id="d12f3-128">O registro em log em si não é muito interessante, mas o exemplo mostra como obter a resposta dentro do manipulador.</span><span class="sxs-lookup"><span data-stu-id="d12f3-128">The logging itself is not very interesting, but the example shows how to get at the response inside the handler.</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample3.cs?highlight=10,13)]

## <a name="adding-message-handlers-to-the-client-pipeline"></a><span data-ttu-id="d12f3-129">Adicionando manipuladores de mensagens para o Pipeline de cliente</span><span class="sxs-lookup"><span data-stu-id="d12f3-129">Adding Message Handlers to the Client Pipeline</span></span>

<span data-ttu-id="d12f3-130">Para adicionar manipuladores personalizados para **HttpClient**, use o **HttpClientFactory.Create** método:</span><span class="sxs-lookup"><span data-stu-id="d12f3-130">To add custom handlers to **HttpClient**, use the **HttpClientFactory.Create** method:</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample4.cs)]

<span data-ttu-id="d12f3-131">Manipuladores de mensagens são chamados na ordem em que você os transmite para o **criar** método.</span><span class="sxs-lookup"><span data-stu-id="d12f3-131">Message handlers are called in the order that you pass them into the **Create** method.</span></span> <span data-ttu-id="d12f3-132">Porque os manipuladores são aninhados, a mensagem de resposta viaja na outra direção.</span><span class="sxs-lookup"><span data-stu-id="d12f3-132">Because handlers are nested, the response message travels in the other direction.</span></span> <span data-ttu-id="d12f3-133">Ou seja, o último manipulador é o primeiro para obter a mensagem de resposta.</span><span class="sxs-lookup"><span data-stu-id="d12f3-133">That is, the last handler is the first to get the response message.</span></span>

---
uid: web-api/overview/advanced/http-message-handlers
title: Manipuladores de mensagens de HTTP na API Web ASP.NET | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 02/13/2012
ms.assetid: 9002018b-3aa3-4358-bb1c-fbb5bc751d01
msc.legacyurl: /web-api/overview/advanced/http-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: 5694845d6f7f9d868c7b3f83785fddda863756d9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37821788"
---
<a name="http-message-handlers-in-aspnet-web-api"></a><span data-ttu-id="f394b-102">Manipuladores de mensagens de HTTP na API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="f394b-102">HTTP Message Handlers in ASP.NET Web API</span></span>
====================
<span data-ttu-id="f394b-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="f394b-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="f394b-104">Um *manipulador de mensagens* é uma classe que recebe uma solicitação HTTP e retorna uma resposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="f394b-104">A *message handler* is a class that receives an HTTP request and returns an HTTP response.</span></span> <span data-ttu-id="f394b-105">Manipuladores de mensagens derivam abstrata **HttpMessageHandler** classe.</span><span class="sxs-lookup"><span data-stu-id="f394b-105">Message handlers derive from the abstract **HttpMessageHandler** class.</span></span>

<span data-ttu-id="f394b-106">Normalmente, uma série de manipuladores de mensagens são encadeados juntos.</span><span class="sxs-lookup"><span data-stu-id="f394b-106">Typically, a series of message handlers are chained together.</span></span> <span data-ttu-id="f394b-107">O primeiro manipulador recebe uma solicitação HTTP, executa algum processamento e fornece a solicitação para o próximo manipulador.</span><span class="sxs-lookup"><span data-stu-id="f394b-107">The first handler receives an HTTP request, does some processing, and gives the request to the next handler.</span></span> <span data-ttu-id="f394b-108">Em algum momento, a resposta é criada e retorna a cadeia.</span><span class="sxs-lookup"><span data-stu-id="f394b-108">At some point, the response is created and goes back up the chain.</span></span> <span data-ttu-id="f394b-109">Esse padrão é chamado de um *delegando* manipulador.</span><span class="sxs-lookup"><span data-stu-id="f394b-109">This pattern is called a *delegating* handler.</span></span>

![](http-message-handlers/_static/image1.png)

## <a name="server-side-message-handlers"></a><span data-ttu-id="f394b-110">Manipuladores de mensagens do lado do servidor</span><span class="sxs-lookup"><span data-stu-id="f394b-110">Server-Side Message Handlers</span></span>

<span data-ttu-id="f394b-111">No lado do servidor, o pipeline da API Web usa alguns manipuladores de mensagem interna:</span><span class="sxs-lookup"><span data-stu-id="f394b-111">On the server side, the Web API pipeline uses some built-in message handlers:</span></span>

- <span data-ttu-id="f394b-112">**Servidor HTTP** obtém a solicitação do host.</span><span class="sxs-lookup"><span data-stu-id="f394b-112">**HttpServer** gets the request from the host.</span></span>
- <span data-ttu-id="f394b-113">**HttpRoutingDispatcher** envia a solicitação com base na rota.</span><span class="sxs-lookup"><span data-stu-id="f394b-113">**HttpRoutingDispatcher** dispatches the request based on the route.</span></span>
- <span data-ttu-id="f394b-114">**HttpControllerDispatcher** envia a solicitação para um controlador de API da Web.</span><span class="sxs-lookup"><span data-stu-id="f394b-114">**HttpControllerDispatcher** sends the request to a Web API controller.</span></span>

<span data-ttu-id="f394b-115">Você pode adicionar manipuladores personalizados para o pipeline.</span><span class="sxs-lookup"><span data-stu-id="f394b-115">You can add custom handlers to the pipeline.</span></span> <span data-ttu-id="f394b-116">Manipuladores de mensagens são bons para interesses transversais que operam no nível de HTTP mensagens (em vez de ações do controlador).</span><span class="sxs-lookup"><span data-stu-id="f394b-116">Message handlers are good for cross-cutting concerns that operate at the level of HTTP messages (rather than controller actions).</span></span> <span data-ttu-id="f394b-117">Por exemplo, um manipulador de mensagens pode:</span><span class="sxs-lookup"><span data-stu-id="f394b-117">For example, a message handler might:</span></span>

- <span data-ttu-id="f394b-118">Ler ou modificar os cabeçalhos de solicitação.</span><span class="sxs-lookup"><span data-stu-id="f394b-118">Read or modify request headers.</span></span>
- <span data-ttu-id="f394b-119">Adicione um cabeçalho de resposta para as respostas.</span><span class="sxs-lookup"><span data-stu-id="f394b-119">Add a response header to responses.</span></span>
- <span data-ttu-id="f394b-120">Valide as solicitações antes que elas atinjam o controlador.</span><span class="sxs-lookup"><span data-stu-id="f394b-120">Validate requests before they reach the controller.</span></span>

<span data-ttu-id="f394b-121">Este diagrama mostra dois manipuladores personalizados inseridos no pipeline:</span><span class="sxs-lookup"><span data-stu-id="f394b-121">This diagram shows two custom handlers inserted into the pipeline:</span></span>

![](http-message-handlers/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="f394b-122">No lado do cliente, o HttpClient também utiliza manipuladores de mensagens.</span><span class="sxs-lookup"><span data-stu-id="f394b-122">On the client side, HttpClient also uses message handlers.</span></span> <span data-ttu-id="f394b-123">Para obter mais informações, consulte [manipuladores de mensagens de HttpClient](httpclient-message-handlers.md).</span><span class="sxs-lookup"><span data-stu-id="f394b-123">For more information, see [HttpClient Message Handlers](httpclient-message-handlers.md).</span></span>


## <a name="custom-message-handlers"></a><span data-ttu-id="f394b-124">Manipuladores de mensagens personalizado</span><span class="sxs-lookup"><span data-stu-id="f394b-124">Custom Message Handlers</span></span>

<span data-ttu-id="f394b-125">Para escrever um manipulador de mensagens personalizadas, derivam **System.Net.Http.DelegatingHandler** e substitua o **SendAsync** método.</span><span class="sxs-lookup"><span data-stu-id="f394b-125">To write a custom message handler, derive from **System.Net.Http.DelegatingHandler** and override the **SendAsync** method.</span></span> <span data-ttu-id="f394b-126">Esse método tem a seguinte assinatura:</span><span class="sxs-lookup"><span data-stu-id="f394b-126">This method has the following signature:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample1.cs)]

<span data-ttu-id="f394b-127">O método utiliza um **HttpRequestMessage** como entrada e retorna de maneira assíncrona uma **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="f394b-127">The method takes an **HttpRequestMessage** as input and asynchronously returns an **HttpResponseMessage**.</span></span> <span data-ttu-id="f394b-128">Uma implementação típica faz o seguinte:</span><span class="sxs-lookup"><span data-stu-id="f394b-128">A typical implementation does the following:</span></span>

1. <span data-ttu-id="f394b-129">Processe a mensagem de solicitação.</span><span class="sxs-lookup"><span data-stu-id="f394b-129">Process the request message.</span></span>
2. <span data-ttu-id="f394b-130">Chamar `base.SendAsync` para enviar a solicitação para o manipulador interno.</span><span class="sxs-lookup"><span data-stu-id="f394b-130">Call `base.SendAsync` to send the request to the inner handler.</span></span>
3. <span data-ttu-id="f394b-131">O manipulador interno retorna uma mensagem de resposta.</span><span class="sxs-lookup"><span data-stu-id="f394b-131">The inner handler returns a response message.</span></span> <span data-ttu-id="f394b-132">(Essa etapa é assíncrona).</span><span class="sxs-lookup"><span data-stu-id="f394b-132">(This step is asynchronous.)</span></span>
4. <span data-ttu-id="f394b-133">Processar a resposta e retorná-lo ao chamador.</span><span class="sxs-lookup"><span data-stu-id="f394b-133">Process the response and return it to the caller.</span></span>

<span data-ttu-id="f394b-134">Aqui está um exemplo trivial:</span><span class="sxs-lookup"><span data-stu-id="f394b-134">Here is a trivial example:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample2.cs)]

> [!NOTE]
> <span data-ttu-id="f394b-135">A chamada para `base.SendAsync` é assíncrona.</span><span class="sxs-lookup"><span data-stu-id="f394b-135">The call to `base.SendAsync` is asynchronous.</span></span> <span data-ttu-id="f394b-136">Se o manipulador faz qualquer trabalho após esta chamada, use o **await** palavra-chave, conforme mostrado.</span><span class="sxs-lookup"><span data-stu-id="f394b-136">If the handler does any work after this call, use the **await** keyword, as shown.</span></span>


<span data-ttu-id="f394b-137">Um manipulador de delegação pode também ignorar o manipulador interno e criar diretamente a resposta:</span><span class="sxs-lookup"><span data-stu-id="f394b-137">A delegating handler can also skip the inner handler and directly create the response:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample3.cs)]

<span data-ttu-id="f394b-138">Se a delegação de um manipulador cria a resposta sem chamar `base.SendAsync`, a solicitação irá ignorar o restante do pipeline.</span><span class="sxs-lookup"><span data-stu-id="f394b-138">If a delegating handler creates the response without calling `base.SendAsync`, the request skips the rest of the pipeline.</span></span> <span data-ttu-id="f394b-139">Isso pode ser útil para um manipulador que valida a solicitação (Criando uma resposta de erro).</span><span class="sxs-lookup"><span data-stu-id="f394b-139">This can be useful for a handler that validates the request (creating an error response).</span></span>

![](http-message-handlers/_static/image3.png)

## <a name="adding-a-handler-to-the-pipeline"></a><span data-ttu-id="f394b-140">Adicionando um manipulador para o Pipeline</span><span class="sxs-lookup"><span data-stu-id="f394b-140">Adding a Handler to the Pipeline</span></span>

<span data-ttu-id="f394b-141">Para adicionar um manipulador de mensagens no lado do servidor, adicione o manipulador para o **HttpConfiguration.MessageHandlers** coleção.</span><span class="sxs-lookup"><span data-stu-id="f394b-141">To add a message handler on the server side, add the handler to the **HttpConfiguration.MessageHandlers** collection.</span></span> <span data-ttu-id="f394b-142">Se você usou o modelo "Aplicativo de Web do ASP.NET MVC 4" para criar o projeto, você pode fazer isso dentro de **WebApiConfig** classe:</span><span class="sxs-lookup"><span data-stu-id="f394b-142">If you used the "ASP.NET MVC 4 Web Application" template to create the project, you can do this inside the **WebApiConfig** class:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample4.cs)]

<span data-ttu-id="f394b-143">Manipuladores de mensagens são chamados na mesma ordem em que aparecem no **MessageHandlers** coleção.</span><span class="sxs-lookup"><span data-stu-id="f394b-143">Message handlers are called in the same order that they appear in **MessageHandlers** collection.</span></span> <span data-ttu-id="f394b-144">Porque eles estão aninhados, a mensagem de resposta viaja na outra direção.</span><span class="sxs-lookup"><span data-stu-id="f394b-144">Because they are nested, the response message travels in the other direction.</span></span> <span data-ttu-id="f394b-145">Ou seja, o último manipulador é o primeiro para obter a mensagem de resposta.</span><span class="sxs-lookup"><span data-stu-id="f394b-145">That is, the last handler is the first to get the response message.</span></span>

<span data-ttu-id="f394b-146">Observe que você não precisa definir os manipuladores internos; a estrutura da API Web se conecta automaticamente os manipuladores de mensagem.</span><span class="sxs-lookup"><span data-stu-id="f394b-146">Notice that you don't need to set the inner handlers; the Web API framework automatically connects the message handlers.</span></span>

<span data-ttu-id="f394b-147">Se você estiver [auto-hospedagem](../older-versions/self-host-a-web-api.md), crie uma instância das **HttpSelfHostConfiguration** de classe e adicione os manipuladores para o **MessageHandlers** coleção.</span><span class="sxs-lookup"><span data-stu-id="f394b-147">If you are [self-hosting](../older-versions/self-host-a-web-api.md), create an instance of the **HttpSelfHostConfiguration** class and add the handlers to the **MessageHandlers** collection.</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample5.cs)]

<span data-ttu-id="f394b-148">Agora vamos examinar alguns exemplos de manipuladores de mensagens personalizadas.</span><span class="sxs-lookup"><span data-stu-id="f394b-148">Now let's look at some examples of custom message handlers.</span></span>

## <a name="example-x-http-method-override"></a><span data-ttu-id="f394b-149">Exemplo: X-HTTP-Method-Override</span><span class="sxs-lookup"><span data-stu-id="f394b-149">Example: X-HTTP-Method-Override</span></span>

<span data-ttu-id="f394b-150">X-HTTP-Method-Override é um cabeçalho HTTP não padrão.</span><span class="sxs-lookup"><span data-stu-id="f394b-150">X-HTTP-Method-Override is a non-standard HTTP header.</span></span> <span data-ttu-id="f394b-151">Ele é projetado para clientes que não é possível enviar determinados tipos de solicitação HTTP, como PUT ou DELETE.</span><span class="sxs-lookup"><span data-stu-id="f394b-151">It is designed for clients that cannot send certain HTTP request types, such as PUT or DELETE.</span></span> <span data-ttu-id="f394b-152">Em vez disso, o cliente envia uma solicitação POST e define o cabeçalho X-HTTP-Method-Override para o método desejado.</span><span class="sxs-lookup"><span data-stu-id="f394b-152">Instead, the client sends a POST request and sets the X-HTTP-Method-Override header to the desired method.</span></span> <span data-ttu-id="f394b-153">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="f394b-153">For example:</span></span>

[!code-console[Main](http-message-handlers/samples/sample6.cmd)]

<span data-ttu-id="f394b-154">Aqui está um manipulador de mensagens que adiciona suporte para X-HTTP-Method-Override:</span><span class="sxs-lookup"><span data-stu-id="f394b-154">Here is a message handler that adds support for X-HTTP-Method-Override:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample7.cs)]

<span data-ttu-id="f394b-155">No **SendAsync** método, o manipulador verifica se a mensagem de solicitação é uma solicitação POST e se ele contém o cabeçalho X-HTTP-Method-Override.</span><span class="sxs-lookup"><span data-stu-id="f394b-155">In the **SendAsync** method, the handler checks whether the request message is a POST request, and whether it contains the X-HTTP-Method-Override header.</span></span> <span data-ttu-id="f394b-156">Nesse caso, ele valida o valor do cabeçalho e, em seguida, modifica o método de solicitação.</span><span class="sxs-lookup"><span data-stu-id="f394b-156">If so, it validates the header value, and then modifies the request method.</span></span> <span data-ttu-id="f394b-157">Por fim, chama o manipulador `base.SendAsync` para passar a mensagem para o próximo manipulador.</span><span class="sxs-lookup"><span data-stu-id="f394b-157">Finally, the handler calls `base.SendAsync` to pass the message to the next handler.</span></span>

<span data-ttu-id="f394b-158">Quando a solicitação atinja o **HttpControllerDispatcher** classe, **HttpControllerDispatcher** irá rotear a solicitação com base no método de solicitação atualizado.</span><span class="sxs-lookup"><span data-stu-id="f394b-158">When the request reaches the **HttpControllerDispatcher** class, **HttpControllerDispatcher** will route the request based on the updated request method.</span></span>

## <a name="example-adding-a-custom-response-header"></a><span data-ttu-id="f394b-159">Exemplo: Adicionar um cabeçalho de resposta personalizada</span><span class="sxs-lookup"><span data-stu-id="f394b-159">Example: Adding a Custom Response Header</span></span>

<span data-ttu-id="f394b-160">Aqui está um manipulador de mensagens que adiciona um cabeçalho personalizado para cada mensagem de resposta:</span><span class="sxs-lookup"><span data-stu-id="f394b-160">Here is a message handler that adds a custom header to every response message:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample8.cs)]

<span data-ttu-id="f394b-161">Primeiro, o manipulador chama `base.SendAsync` para passar a solicitação para o manipulador de mensagens internas.</span><span class="sxs-lookup"><span data-stu-id="f394b-161">First, the handler calls `base.SendAsync` to pass the request to the inner message handler.</span></span> <span data-ttu-id="f394b-162">O manipulador interno retorna uma mensagem de resposta, mas ele faz isso de forma assíncrona usando um **tarefa&lt;T&gt;**  objeto.</span><span class="sxs-lookup"><span data-stu-id="f394b-162">The inner handler returns a response message, but it does so asynchronously using a **Task&lt;T&gt;** object.</span></span> <span data-ttu-id="f394b-163">A mensagem de resposta não está disponível até `base.SendAsync` for concluída de forma assíncrona.</span><span class="sxs-lookup"><span data-stu-id="f394b-163">The response message is not available until `base.SendAsync` completes asynchronously.</span></span>

<span data-ttu-id="f394b-164">Este exemplo usa o **await** palavra-chave para executar o trabalho assincronamente após `SendAsync` é concluída.</span><span class="sxs-lookup"><span data-stu-id="f394b-164">This example uses the **await** keyword to perform work asynchronously after `SendAsync` completes.</span></span> <span data-ttu-id="f394b-165">Se você estiver direcionando o .NET Framework 4.0, use o **tarefa**&lt;T&gt;**. ContinueWith** método:</span><span class="sxs-lookup"><span data-stu-id="f394b-165">If you are targeting .NET Framework 4.0, use the **Task**&lt;T&gt;**.ContinueWith** method:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample9.cs)]

## <a name="example-checking-for-an-api-key"></a><span data-ttu-id="f394b-166">Exemplo: Verificação de uma chave de API</span><span class="sxs-lookup"><span data-stu-id="f394b-166">Example: Checking for an API Key</span></span>

<span data-ttu-id="f394b-167">Alguns serviços da web exigem que os clientes incluir uma chave de API na sua solicitação.</span><span class="sxs-lookup"><span data-stu-id="f394b-167">Some web services require clients to include an API key in their request.</span></span> <span data-ttu-id="f394b-168">O exemplo a seguir mostra como um manipulador de mensagens pode verificar as solicitações para uma chave de API válida:</span><span class="sxs-lookup"><span data-stu-id="f394b-168">The following example shows how a message handler can check requests for a valid API key:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample10.cs)]

<span data-ttu-id="f394b-169">Esse manipulador procura a chave de API na cadeia de caracteres de consulta do URI.</span><span class="sxs-lookup"><span data-stu-id="f394b-169">This handler looks for the API key in the URI query string.</span></span> <span data-ttu-id="f394b-170">(Neste exemplo, vamos supor que a chave é uma cadeia de caracteres estática.</span><span class="sxs-lookup"><span data-stu-id="f394b-170">(For this example, we assume that the key is a static string.</span></span> <span data-ttu-id="f394b-171">Uma implementação real provavelmente usaria uma validação mais complexa.) Se a cadeia de caracteres de consulta contém a chave, o manipulador passa a solicitação para o manipulador interno.</span><span class="sxs-lookup"><span data-stu-id="f394b-171">A real implementation would probably use more complex validation.) If the query string contains the key, the handler passes the request to the inner handler.</span></span>

<span data-ttu-id="f394b-172">Se a solicitação não tiver uma chave válida, o manipulador cria uma mensagem de resposta com status 403, proibido.</span><span class="sxs-lookup"><span data-stu-id="f394b-172">If the request does not have a valid key, the handler creates a response message with status 403, Forbidden.</span></span> <span data-ttu-id="f394b-173">Nesse caso, o manipulador não chama `base.SendAsync`, portanto, o manipulador interno nunca recebe a solicitação, nem o controlador.</span><span class="sxs-lookup"><span data-stu-id="f394b-173">In this case, the handler does not call `base.SendAsync`, so the inner handler never receives the request, nor does the controller.</span></span> <span data-ttu-id="f394b-174">Portanto, o controlador pode assumir que todas as solicitações de entrada tem uma chave de API válida.</span><span class="sxs-lookup"><span data-stu-id="f394b-174">Therefore, the controller can assume that all incoming requests have a valid API key.</span></span>

> [!NOTE]
> <span data-ttu-id="f394b-175">Se a chave de API se aplica apenas a determinadas ações do controlador, considere usar um filtro de ação em vez de um manipulador de mensagens.</span><span class="sxs-lookup"><span data-stu-id="f394b-175">If the API key applies only to certain controller actions, consider using an action filter instead of a message handler.</span></span> <span data-ttu-id="f394b-176">Filtros de ação executado após o roteamento do URI é executado.</span><span class="sxs-lookup"><span data-stu-id="f394b-176">Action filters run after URI routing is performed.</span></span>


## <a name="per-route-message-handlers"></a><span data-ttu-id="f394b-177">Manipuladores de mensagens por rota</span><span class="sxs-lookup"><span data-stu-id="f394b-177">Per-Route Message Handlers</span></span>

<span data-ttu-id="f394b-178">Manipuladores de **HttpConfiguration.MessageHandlers** coleção se aplicam globalmente.</span><span class="sxs-lookup"><span data-stu-id="f394b-178">Handlers in the **HttpConfiguration.MessageHandlers** collection apply globally.</span></span>

<span data-ttu-id="f394b-179">Como alternativa, você pode adicionar um manipulador de mensagens para uma rota específica quando você define a rota:</span><span class="sxs-lookup"><span data-stu-id="f394b-179">Alternatively, you can add a message handler to a specific route when you define the route:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample11.cs?highlight=16)]

<span data-ttu-id="f394b-180">Neste exemplo, se o URI da solicitação corresponde a "Route2", a solicitação será enviada para `MessageHandler2`.</span><span class="sxs-lookup"><span data-stu-id="f394b-180">In this example, if the request URI matches "Route2", the request is dispatched to `MessageHandler2`.</span></span> <span data-ttu-id="f394b-181">O diagrama a seguir mostra o pipeline para essas duas rotas:</span><span class="sxs-lookup"><span data-stu-id="f394b-181">The following diagram shows the pipeline for these two routes:</span></span>

![](http-message-handlers/_static/image4.png)

<span data-ttu-id="f394b-182">Observe que `MessageHandler2` substitui o padrão **HttpControllerDispatcher**.</span><span class="sxs-lookup"><span data-stu-id="f394b-182">Notice that `MessageHandler2` replaces the default **HttpControllerDispatcher**.</span></span> <span data-ttu-id="f394b-183">Neste exemplo, `MessageHandler2` cria a resposta e as solicitações que correspondem a "Route2" nunca ir para um controlador.</span><span class="sxs-lookup"><span data-stu-id="f394b-183">In this example, `MessageHandler2` creates the response, and requests that match "Route2" never go to a controller.</span></span> <span data-ttu-id="f394b-184">Isso lhe permite substituir o mecanismo de controlador de API da Web inteiro com seu próprio ponto de extremidade personalizado.</span><span class="sxs-lookup"><span data-stu-id="f394b-184">This lets you replace the entire Web API controller mechanism with your own custom endpoint.</span></span>

<span data-ttu-id="f394b-185">Como alternativa, um manipulador de mensagens por rota pode delegar a **HttpControllerDispatcher**, que então expede para um controlador.</span><span class="sxs-lookup"><span data-stu-id="f394b-185">Alternatively, a per-route message handler can delegate to **HttpControllerDispatcher**, which then dispatches to a controller.</span></span>

![](http-message-handlers/_static/image5.png)

<span data-ttu-id="f394b-186">O código a seguir mostra como configurar essa rota:</span><span class="sxs-lookup"><span data-stu-id="f394b-186">The following code shows how to configure this route:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample12.cs)]

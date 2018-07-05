---
uid: webhooks/receiving/handlers
title: Manipuladores de WebHooks do ASP.NET | Microsoft Docs
author: rick-anderson
description: Como lidar com solicitações de WebHooks do ASP.NET.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: a55b0d20-9c90-4bd3-a471-20da6f569f0c
ms.technology: ''
ms.openlocfilehash: 7e45a97ac9d61b2d046984e5ede3be158b741b7d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37372693"
---
# <a name="aspnet-webhooks-handlers"></a><span data-ttu-id="f99c7-103">Manipuladores de WebHooks do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="f99c7-103">ASP.NET WebHooks handlers</span></span>

<span data-ttu-id="f99c7-104">Depois que as solicitações de WebHooks foi validada por um receptor de WebHook, está pronto para ser processado pelo código do usuário.</span><span class="sxs-lookup"><span data-stu-id="f99c7-104">Once WebHooks requests has been validated by a WebHook receiver, it is ready to be processed by user code.</span></span> <span data-ttu-id="f99c7-105">É aí que *manipuladores de* entram.</span><span class="sxs-lookup"><span data-stu-id="f99c7-105">This is where *handlers* come in.</span></span> <span data-ttu-id="f99c7-106">Manipuladores derivam de [IWebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) interface, mas normalmente usa o [WebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) classe em vez de derivar diretamente da interface do.</span><span class="sxs-lookup"><span data-stu-id="f99c7-106">Handlers derive from the [IWebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) interface but typically uses the [WebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) class instead of deriving directly from the interface.</span></span>

<span data-ttu-id="f99c7-107">Uma solicitação de WebHook pode ser processada por um ou mais manipuladores.</span><span class="sxs-lookup"><span data-stu-id="f99c7-107">A WebHook request can be processed by one or more handlers.</span></span> <span data-ttu-id="f99c7-108">Manipuladores são chamados na ordem com base em suas respectivas *ordem* propriedade indo do mais baixo para o mais alto em que ordem é um inteiro simples (sugerido para ser entre 1 e 100):</span><span class="sxs-lookup"><span data-stu-id="f99c7-108">Handlers are called in order based on their respective *Order* property going from lowest to highest where Order is a simple integer (suggested to be between 1 and 100):</span></span>

![Diagrama de propriedade de ordem do manipulador de WebHook](_static/Handlers.png)

<span data-ttu-id="f99c7-110">Um manipulador pode, opcionalmente, defina as *resposta* propriedade sobre a WebHookHandlerContext que levará o processamento a parada e a resposta a ser enviada de volta como a resposta HTTP para o WebHook.</span><span class="sxs-lookup"><span data-stu-id="f99c7-110">A handler can optionally set the *Response* property on the WebHookHandlerContext which will lead the processing to stop and the response to be sent back as the HTTP response to the WebHook.</span></span> <span data-ttu-id="f99c7-111">No caso acima, não será chamada C de manipulador porque ele tem uma ordem mais alta do que B e B define a resposta.</span><span class="sxs-lookup"><span data-stu-id="f99c7-111">In the case above, Handler C won't get called because it has a higher order than B and B sets the response.</span></span>

<span data-ttu-id="f99c7-112">Definição da resposta normalmente só é relevante para WebHooks em que a resposta pode transportar informações de volta para a API de origem.</span><span class="sxs-lookup"><span data-stu-id="f99c7-112">Setting the response is typically only relevant for WebHooks where the response can carry information back to the originating API.</span></span> <span data-ttu-id="f99c7-113">Por exemplo, esse é o caso com os WebHooks do Slack em que a resposta é postada de volta o canal de onde veio o WebHook.</span><span class="sxs-lookup"><span data-stu-id="f99c7-113">This is for example the case with Slack WebHooks where the response is posted back to the channel where the WebHook came from.</span></span> <span data-ttu-id="f99c7-114">Manipuladores podem definir a propriedade de receptor se desejam receber WebHooks de receptor em particular.</span><span class="sxs-lookup"><span data-stu-id="f99c7-114">Handlers can set the Receiver property if they only want to receive WebHooks from that particular receiver.</span></span> <span data-ttu-id="f99c7-115">Se eles não definir o receptor, eles são chamados para todos eles.</span><span class="sxs-lookup"><span data-stu-id="f99c7-115">If they don't set the receiver they are called for all of them.</span></span>

<span data-ttu-id="f99c7-116">Outro uso comum de uma resposta é usar um *410 perdido* resposta para indicar que o WebHook não está ativo e nenhuma solicitação adicional deve ser enviada.</span><span class="sxs-lookup"><span data-stu-id="f99c7-116">One other common use of a response is to use a *410 Gone* response to indicate that the WebHook no longer is active and no further requests should be submitted.</span></span>

<span data-ttu-id="f99c7-117">Por padrão, um manipulador será chamado por todos os receptores de WebHook.</span><span class="sxs-lookup"><span data-stu-id="f99c7-117">By default a handler will be called by all WebHook receivers.</span></span> <span data-ttu-id="f99c7-118">No entanto, se o *receptor* estiver definida como o nome de um manipulador, em seguida, esse manipulador só receberá solicitações de WebHook do receptor.</span><span class="sxs-lookup"><span data-stu-id="f99c7-118">However, if the *Receiver* property is set to the name of a handler then that handler will only receive WebHook requests from that receiver.</span></span>

## <a name="processing-a-webhook"></a><span data-ttu-id="f99c7-119">Processamento de um WebHook</span><span class="sxs-lookup"><span data-stu-id="f99c7-119">Processing a WebHook</span></span>

<span data-ttu-id="f99c7-120">Quando um manipulador é chamado, ele obtém uma [WebHookHandlerContext](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandlerContext.cs) que contém informações sobre a solicitação de WebHook.</span><span class="sxs-lookup"><span data-stu-id="f99c7-120">When a handler is called, it gets a [WebHookHandlerContext](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandlerContext.cs) containing information about the WebHook request.</span></span> <span data-ttu-id="f99c7-121">Os dados, normalmente o corpo da solicitação HTTP, estão disponíveis na *dados* propriedade.</span><span class="sxs-lookup"><span data-stu-id="f99c7-121">The data, typically the HTTP request body, is available from the *Data* property.</span></span>

<span data-ttu-id="f99c7-122">O tipo de dados é, normalmente, dados de formulário HTML ou JSON, mas é possível converter para um tipo mais específico, se desejado.</span><span class="sxs-lookup"><span data-stu-id="f99c7-122">The type of the data is typically JSON or HTML form data, but it is possible to cast to a more specific type if desired.</span></span> <span data-ttu-id="f99c7-123">Por exemplo, os WebHooks personalizados gerados pelo WebHooks do ASP.NET pode ser convertidos no tipo [CustomNotifications](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers.Custom/WebHooks/CustomNotifications.cs) da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="f99c7-123">For example, the custom WebHooks generated by ASP.NET WebHooks can be cast to the type [CustomNotifications](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers.Custom/WebHooks/CustomNotifications.cs) as follows:</span></span>

```csharp
public class MyWebHookHandler : WebHookHandler
{
    public MyWebHookHandler()
    {
        this.Receiver = "custom";
    }

    public override Task ExecuteAsync(string generator, WebHookHandlerContext context)
    {
        CustomNotifications notifications = context.GetDataOrDefault<CustomNotifications>();
        foreach (var notification in notifications.Notifications)
        {
            ...
        }
        return Task.FromResult(true);
    }
}
```

  ## <a name="queued-processing"></a><span data-ttu-id="f99c7-124">Processamento em fila</span><span class="sxs-lookup"><span data-stu-id="f99c7-124">Queued Processing</span></span>

<span data-ttu-id="f99c7-125">A maioria dos remetentes de WebHook reenviará um WebHook, se uma resposta não é gerada dentro de alguns segundos.</span><span class="sxs-lookup"><span data-stu-id="f99c7-125">Most WebHook senders will resend a WebHook if a response is not generated within a handful of seconds.</span></span> <span data-ttu-id="f99c7-126">Isso significa que seu manipulador deve concluir o processamento dentro desse período, não para que ele seja chamado novamente.</span><span class="sxs-lookup"><span data-stu-id="f99c7-126">This means that your handler must complete the processing within that time frame in order not for it to be called again.</span></span>

<span data-ttu-id="f99c7-127">Se o processamento leva mais tempo ou, melhor é tratado separadamente, o [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) pode ser usado para enviar a solicitação de WebHook a uma fila, por exemplo [fila de armazenamento do Azure](https://msdn.microsoft.com/library/azure/dd179353.aspx).</span><span class="sxs-lookup"><span data-stu-id="f99c7-127">If the processing takes longer, or is better handled separately then the [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) can be used to submit the WebHook request to a queue, for example [Azure Storage Queue](https://msdn.microsoft.com/library/azure/dd179353.aspx).</span></span>

<span data-ttu-id="f99c7-128">Uma estrutura de tópicos de um [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) implementação é fornecida aqui:</span><span class="sxs-lookup"><span data-stu-id="f99c7-128">An outline of a [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) implementation is provided here:</span></span>

```csharp
public class QueueHandler : WebHookQueueHandler
{
    public override Task EnqueueAsync(WebHookQueueContext context)
    {

        // Enqueue WebHookQueueContext to your queuing system of choice

        return Task.FromResult(true);
    }
}
```

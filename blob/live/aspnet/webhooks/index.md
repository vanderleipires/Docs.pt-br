---
uid: webhooks/index
title: "Visão geral do ASP.NET WebHooks | Microsoft Docs"
author: rick-anderson
description: "Uma introdução ao ASP.NET WebHooks."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 5e2843f0-f499-448f-a712-33d4e9858321
ms.technology: 
ms.prod: .net-framework
ms.openlocfilehash: 52399c23cdf393a2f7f94661fd48098ced65948c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
# <a name="aspnet-webhooks-overview"></a><span data-ttu-id="98ceb-103">Visão geral do ASP.NET WebHooks</span><span class="sxs-lookup"><span data-stu-id="98ceb-103">ASP.NET WebHooks overview</span></span>

<span data-ttu-id="98ceb-104">WebHooks é um padrão HTTP leve, fornecendo um modelo de publicação/assinatura simples para conectar os serviços de APIs da Web e SaaS.</span><span class="sxs-lookup"><span data-stu-id="98ceb-104">WebHooks is a lightweight HTTP pattern providing a simple pub/sub model for wiring together Web APIs and SaaS services.</span></span> <span data-ttu-id="98ceb-105">Quando ocorre um evento em um serviço, uma notificação é enviada na forma de uma solicitação HTTP POST para assinantes registrados.</span><span class="sxs-lookup"><span data-stu-id="98ceb-105">When an event happens in a service, a notification is sent in the form of an HTTP POST request to registered subscribers.</span></span> <span data-ttu-id="98ceb-106">A solicitação POST contém informações sobre o evento que torna possível para o receptor funcionar adequadamente.</span><span class="sxs-lookup"><span data-stu-id="98ceb-106">The POST request contains information about the event which makes it possible for the receiver to act accordingly.</span></span>

<span data-ttu-id="98ceb-107">Devido à sua simplicidade, WebHooks já estão expostos por um grande número de serviços incluindo [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp ](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/)e muito mais.</span><span class="sxs-lookup"><span data-stu-id="98ceb-107">Because of their simplicity, WebHooks are already exposed by a large number of services including [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/), and many more.</span></span> <span data-ttu-id="98ceb-108">Por exemplo, um WebHook pode indicar que um arquivo foi alterado no [Dropbox](http://dropbox.com/), ou uma alteração de código foi confirmada no GitHub ou um pagamento foi iniciado no [PayPal](http://www.paypal.com/), ou um cartão foi criado no [ Trello](http://www.trello.com/).</span><span class="sxs-lookup"><span data-stu-id="98ceb-108">For example, a WebHook can indicate that a file has changed in [Dropbox](http://dropbox.com/), or a code change has been committed in GitHub, or a payment has been initiated in [PayPal](http://www.paypal.com/), or a card has been created in [Trello](http://www.trello.com/).</span></span> <span data-ttu-id="98ceb-109">As possibilidades são infinitas!</span><span class="sxs-lookup"><span data-stu-id="98ceb-109">The possibilities are endless!</span></span>

<span data-ttu-id="98ceb-110">Microsoft ASP.NET WebHooks torna mais fácil enviar e receber WebHooks como parte do seu aplicativo ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="98ceb-110">Microsoft ASP.NET WebHooks makes it easier to both send and receive WebHooks as part of your ASP.NET application:</span></span>

* <span data-ttu-id="98ceb-111">No lado de recepção, ele fornece um modelo comum de recebimento e processamento WebHooks de qualquer número de provedores de WebHook.</span><span class="sxs-lookup"><span data-stu-id="98ceb-111">On the receiving side, it provides a common model for receiving and processing WebHooks from any number of WebHook providers.</span></span> <span data-ttu-id="98ceb-112">Ele sair da caixa com suporte para [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Pusher](http://www.pusher.com), [Salesforce](http://www.salesforce.com), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/),[ WordPress](http://www.wordpress.com) e [Zendesk](https://www.zendesk.com/) , mas é fácil adicionar suporte para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="98ceb-112">It comes out of the box with support for [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Pusher](http://www.pusher.com), [Salesforce](http://www.salesforce.com), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/),[WordPress](http://www.wordpress.com) and [Zendesk](https://www.zendesk.com/) but it is easy to add support for more.</span></span>

* <span data-ttu-id="98ceb-113">No lado de envio, ele fornece suporte para gerenciar e armazenar assinaturas, bem como para enviar notificações de eventos para o conjunto certo de assinantes.</span><span class="sxs-lookup"><span data-stu-id="98ceb-113">On the sending side it provides support for managing and storing subscriptions as well as for sending event notifications to the right set of subscribers.</span></span> <span data-ttu-id="98ceb-114">Isso permite que você defina seu próprio conjunto de eventos que os assinantes podem assinar e notificá-lo quando as coisas acontece.</span><span class="sxs-lookup"><span data-stu-id="98ceb-114">This allows you to define your own set of events that subscribers can subscribe to and notify them when things happens.</span></span>

<span data-ttu-id="98ceb-115">As duas partes podem ser usadas em conjunto ou distância dependendo do cenário.</span><span class="sxs-lookup"><span data-stu-id="98ceb-115">The two parts can be used together or apart depending on your scenario.</span></span> <span data-ttu-id="98ceb-116">Se você só precisa receber WebHooks de outros serviços, em seguida, você pode usar apenas a parte do destinatário; Se você quiser expor ganchos para outras pessoas para consumir, você pode fazer exatamente isso.</span><span class="sxs-lookup"><span data-stu-id="98ceb-116">If you only need to receive WebHooks from other services then you can use just the receiver part; if you only want to expose WebHooks for others to consume, then you can do just that.</span></span>

<span data-ttu-id="98ceb-117">O código tem como alvo o ASP.NET Web API 2 e 5 do ASP.NET MVC e está disponível como [OSS no GitHub](https://github.com/aspnet/WebHooks).</span><span class="sxs-lookup"><span data-stu-id="98ceb-117">The code targets ASP.NET Web API 2 and ASP.NET MVC 5 and is available as [OSS on GitHub](https://github.com/aspnet/WebHooks).</span></span>

## <a name="webhooks-overview"></a><span data-ttu-id="98ceb-118">Visão geral de WebHooks</span><span class="sxs-lookup"><span data-stu-id="98ceb-118">WebHooks Overview</span></span>

<span data-ttu-id="98ceb-119">WebHooks é um padrão que significa que, dependendo de como ele é usado do serviço ao serviço, mas a ideia básica é o mesmo.</span><span class="sxs-lookup"><span data-stu-id="98ceb-119">WebHooks is a pattern which means that it varies how it is used from service to service but the basic idea is the same.</span></span> <span data-ttu-id="98ceb-120">Você pode pensar WebHooks como um modelo de publicação/assinatura simples em que um usuário pode inscrever-se para eventos que ocorrem em outro lugar.</span><span class="sxs-lookup"><span data-stu-id="98ceb-120">You can think of WebHooks as a simple pub/sub model where a user can subscribe to events happening elsewhere.</span></span> <span data-ttu-id="98ceb-121">As notificações de eventos são propagadas como solicitações HTTP POST que contém informações sobre o próprio evento.</span><span class="sxs-lookup"><span data-stu-id="98ceb-121">The event notifications are propagated as HTTP POST requests containing information about the event itself.</span></span>

<span data-ttu-id="98ceb-122">Normalmente, a solicitação HTTP POST contém um objeto JSON ou dados do formulário HTML determinados pelo remetente WebHook incluindo informações sobre o evento que causa o WebHook para disparar.</span><span class="sxs-lookup"><span data-stu-id="98ceb-122">Typically the HTTP POST request contains a JSON object or HTML form data determined by the WebHook sender including information about the event causing the WebHook to trigger.</span></span> <span data-ttu-id="98ceb-123">Por exemplo, um exemplo de um corpo de solicitação POST de WebHook de [GitHub](http://www.github.com/) tem esta aparência como resultado de um novo problema que está sendo aberto em um repositório específico:</span><span class="sxs-lookup"><span data-stu-id="98ceb-123">For example, an example of a WebHook POST request body from [GitHub](http://www.github.com/) looks like this as a result of a new issue being opened in a particular repository:</span></span>

```json
{
  "action": "opened",
  "issue": {
      "url": "https://api.github.com/repos/octocat/Hello-World/issues/1347",
      "number": 1347,
      ...
  },
  "repository": {
      "id": 1296269,
      "full_name": "octocat/Hello-World",
      "owner": {
          "login": "octocat",
          "id": 1
          ...
      },
      ...
  },
  "sender": {
      "login": "octocat",
      "id": 1,
      ...
  }
}
```

<span data-ttu-id="98ceb-124">Para garantir que o WebHook é realmente do remetente pretendido, a solicitação POST é protegida de alguma forma e verificada pelo destinatário.</span><span class="sxs-lookup"><span data-stu-id="98ceb-124">To ensure that the WebHook is indeed from the intended sender, the POST request is secured in some way and then verified by the receiver.</span></span> <span data-ttu-id="98ceb-125">Por exemplo, [GitHub WebHooks](https://developer.github.com/webhooks/) inclui um *X-Hub-assinatura* cabeçalho HTTP com um hash do corpo da solicitação que é verificado pela implementação do destinatário, para que você não precisa se preocupar sobre ele.</span><span class="sxs-lookup"><span data-stu-id="98ceb-125">For example, [GitHub WebHooks](https://developer.github.com/webhooks/) includes an *X-Hub-Signature* HTTP header with a hash of the request body which is checked by the receiver implementation so you don't have to worry about it.</span></span>

<span data-ttu-id="98ceb-126">O fluxo de WebHook geralmente é semelhante a esta:</span><span class="sxs-lookup"><span data-stu-id="98ceb-126">The WebHook flow generally goes something like this:</span></span>

* <span data-ttu-id="98ceb-127">O remetente de WebHook expõe eventos que um cliente pode se inscrever.</span><span class="sxs-lookup"><span data-stu-id="98ceb-127">The WebHook sender exposes events that a client can subscribe to.</span></span> <span data-ttu-id="98ceb-128">Os eventos descrevem observáveis alterações no sistema, por exemplo que um novo item de dados tenha sido inseridas, um processo ou algo mais.</span><span class="sxs-lookup"><span data-stu-id="98ceb-128">The events describe observable changes to the system, for example that a new data item has been inserted, that a process has completed, or something else.</span></span>

* <span data-ttu-id="98ceb-129">O receptor de WebHook assina Registrando um WebHook consiste em quatro aspectos:</span><span class="sxs-lookup"><span data-stu-id="98ceb-129">The WebHook receiver subscribes by registering a WebHook consisting of four things:</span></span>

     1. <span data-ttu-id="98ceb-130">Um URI para onde a notificação de evento deve ser lançada na forma de uma solicitação HTTP POST;</span><span class="sxs-lookup"><span data-stu-id="98ceb-130">A URI for where the event notification should be posted in the form of an HTTP POST request;</span></span>

     2. <span data-ttu-id="98ceb-131">Um conjunto de filtros que descreve os eventos específicos para o qual o WebHook deva ser acionado;</span><span class="sxs-lookup"><span data-stu-id="98ceb-131">A set of filters describing the particular events for which the WebHook should be fired;</span></span>

     3. <span data-ttu-id="98ceb-132">Uma chave de segredo que é usada para assinar a solicitação HTTP POST;</span><span class="sxs-lookup"><span data-stu-id="98ceb-132">A secret key which is used to sign the HTTP POST request;</span></span>

     4. <span data-ttu-id="98ceb-133">Dados adicionais que deve ser incluído na solicitação HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="98ceb-133">Additional data which is to be included in the HTTP POST request.</span></span> <span data-ttu-id="98ceb-134">Por exemplo, isso pode ser incluídas no corpo da solicitação HTTP POST de propriedades ou campos adicionais de cabeçalho HTTP.</span><span class="sxs-lookup"><span data-stu-id="98ceb-134">This can for example be additional HTTP header fields or properties included in the HTTP POST request body.</span></span>

* <span data-ttu-id="98ceb-135">Depois que um evento ocorre, os registros de WebHook correspondentes forem encontrados e solicitações HTTP POST são enviadas.</span><span class="sxs-lookup"><span data-stu-id="98ceb-135">Once an event happens, the matching WebHook registrations are found and HTTP POST requests are submitted.</span></span> <span data-ttu-id="98ceb-136">Normalmente, a geração das solicitações HTTP POST são repetidas várias vezes se por algum motivo que o destinatário não está respondendo ou os resultados da solicitação HTTP POST em uma resposta de erro.</span><span class="sxs-lookup"><span data-stu-id="98ceb-136">Typically, the generation of the HTTP POST requests are retried several times if for some reason the recipient is not responding or the HTTP POST request results in an error response.</span></span>

## <a name="webhooks-processing-pipeline"></a><span data-ttu-id="98ceb-137">Pipeline de processamento de WebHooks</span><span class="sxs-lookup"><span data-stu-id="98ceb-137">WebHooks Processing Pipeline</span></span>

<span data-ttu-id="98ceb-138">O pipeline de processamento do Microsoft ASP.NET WebHooks para WebHooks de entrada tem esta aparência:</span><span class="sxs-lookup"><span data-stu-id="98ceb-138">The Microsoft ASP.NET WebHooks processing pipeline for incoming WebHooks looks like this:</span></span>

![Pipeline de processamento do ASP.NET WebHooks](_static/WebHookReceivers.png)

<span data-ttu-id="98ceb-140">Os dois principais conceitos aqui são *receptores* e *manipuladores*:</span><span class="sxs-lookup"><span data-stu-id="98ceb-140">The two key concepts here are *Receivers* and *Handlers*:</span></span>

* <span data-ttu-id="98ceb-141">*Receptores de* é responsável para lidar com o tipo específico de WebHook de um determinado remetente e impor verificações de segurança para garantir que a solicitação de WebHook é realmente do remetente pretendido.</span><span class="sxs-lookup"><span data-stu-id="98ceb-141">*Receivers* are responsible for handling the particular flavor of WebHook from a given sender and for enforcing security checks to ensure that the WebHook request indeed is from the intended sender.</span></span>

* <span data-ttu-id="98ceb-142">*Manipuladores* são normalmente onde o código do usuário executa o processamento de WebHook específico.</span><span class="sxs-lookup"><span data-stu-id="98ceb-142">*Handlers* are typically where user code runs processing the particular WebHook.</span></span>

<span data-ttu-id="98ceb-143">Nos seguintes nós esses conceitos são descritos em mais detalhes.</span><span class="sxs-lookup"><span data-stu-id="98ceb-143">In the following nodes these concepts are described in more details.</span></span>

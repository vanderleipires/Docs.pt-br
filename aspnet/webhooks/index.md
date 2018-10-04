---
uid: webhooks/index
title: Visão geral sobre WebHooks do ASP.NET | Microsoft Docs
author: rick-anderson
description: Uma introdução a WebHooks do ASP.NET.
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 5e2843f0-f499-448f-a712-33d4e9858321
ms.openlocfilehash: 702cc0bf0d0bb887c64bec19e1faf249bd96617a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "48252403"
---
# <a name="aspnet-webhooks-overview"></a><span data-ttu-id="279ee-103">Visão geral de WebHooks do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="279ee-103">ASP.NET WebHooks overview</span></span>

<span data-ttu-id="279ee-104">WebHooks é um padrão HTTP leve, fornecendo um modelo simples de pub/sub para conectar os serviços de SaaS e APIs da Web.</span><span class="sxs-lookup"><span data-stu-id="279ee-104">WebHooks is a lightweight HTTP pattern providing a simple pub/sub model for wiring together Web APIs and SaaS services.</span></span> <span data-ttu-id="279ee-105">Quando ocorre um evento em um serviço, uma notificação é enviada na forma de uma solicitação HTTP POST para assinantes registrados.</span><span class="sxs-lookup"><span data-stu-id="279ee-105">When an event happens in a service, a notification is sent in the form of an HTTP POST request to registered subscribers.</span></span> <span data-ttu-id="279ee-106">A solicitação POST contém informações sobre o evento que torna possível para o destinatário para agir de acordo.</span><span class="sxs-lookup"><span data-stu-id="279ee-106">The POST request contains information about the event which makes it possible for the receiver to act accordingly.</span></span>

<span data-ttu-id="279ee-107">Devido à sua simplicidade, WebHooks já são expostos por um grande número de serviços, incluindo [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp ](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/)e muito mais.</span><span class="sxs-lookup"><span data-stu-id="279ee-107">Because of their simplicity, WebHooks are already exposed by a large number of services including [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/), and many more.</span></span> <span data-ttu-id="279ee-108">Por exemplo, um WebHook pode indicar que um arquivo foi alterado nas [Dropbox](http://dropbox.com/), ou uma alteração de código foi confirmada no GitHub, ou um pagamento foi iniciado no [PayPal](http://www.paypal.com/), ou um cartão foi criado no [ Trello](http://www.trello.com/).</span><span class="sxs-lookup"><span data-stu-id="279ee-108">For example, a WebHook can indicate that a file has changed in [Dropbox](http://dropbox.com/), or a code change has been committed in GitHub, or a payment has been initiated in [PayPal](http://www.paypal.com/), or a card has been created in [Trello](http://www.trello.com/).</span></span> <span data-ttu-id="279ee-109">As possibilidades são infinitas!</span><span class="sxs-lookup"><span data-stu-id="279ee-109">The possibilities are endless!</span></span>

<span data-ttu-id="279ee-110">Microsoft ASP.NET WebHooks torna mais fácil de enviar e receber WebHooks como parte do seu aplicativo ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="279ee-110">Microsoft ASP.NET WebHooks makes it easier to both send and receive WebHooks as part of your ASP.NET application:</span></span>

* <span data-ttu-id="279ee-111">No lado de recepção, ele fornece um modelo comum de recebimento e processamento de WebHooks de qualquer número de provedores de WebHook.</span><span class="sxs-lookup"><span data-stu-id="279ee-111">On the receiving side, it provides a common model for receiving and processing WebHooks from any number of WebHook providers.</span></span> <span data-ttu-id="279ee-112">Ele vem pronta com suporte para [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Pusher](http://www.pusher.com), [Salesforce](http://www.salesforce.com), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/),[ WordPress](http://www.wordpress.com) e [Zendesk](https://www.zendesk.com/) mas é fácil adicionar suporte para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="279ee-112">It comes out of the box with support for [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Pusher](http://www.pusher.com), [Salesforce](http://www.salesforce.com), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/),[WordPress](http://www.wordpress.com) and [Zendesk](https://www.zendesk.com/) but it is easy to add support for more.</span></span>

* <span data-ttu-id="279ee-113">No lado de envio, ele fornece suporte para gerenciar e armazenar as assinaturas, bem como para enviar notificações de eventos para o conjunto certo de assinantes.</span><span class="sxs-lookup"><span data-stu-id="279ee-113">On the sending side it provides support for managing and storing subscriptions as well as for sending event notifications to the right set of subscribers.</span></span> <span data-ttu-id="279ee-114">Isso permite que você defina seu próprio conjunto de eventos que os assinantes podem assinar e notificá-lo quando as coisas acontece.</span><span class="sxs-lookup"><span data-stu-id="279ee-114">This allows you to define your own set of events that subscribers can subscribe to and notify them when things happens.</span></span>

<span data-ttu-id="279ee-115">As duas partes podem ser usadas em conjunto ou separadamente, dependendo do cenário.</span><span class="sxs-lookup"><span data-stu-id="279ee-115">The two parts can be used together or apart depending on your scenario.</span></span> <span data-ttu-id="279ee-116">Se você só precisa receber WebHooks de outros serviços, em seguida, você pode usar apenas a parte do receptor; Se você quiser expor WebHooks para outras pessoas para consumir, em seguida, você pode fazer exatamente isso.</span><span class="sxs-lookup"><span data-stu-id="279ee-116">If you only need to receive WebHooks from other services then you can use just the receiver part; if you only want to expose WebHooks for others to consume, then you can do just that.</span></span>

<span data-ttu-id="279ee-117">O código tem como alvo o API Web 2 ASP.NET e ASP.NET MVC 5 e está disponível como [OSS no GitHub](https://github.com/aspnet/WebHooks).</span><span class="sxs-lookup"><span data-stu-id="279ee-117">The code targets ASP.NET Web API 2 and ASP.NET MVC 5 and is available as [OSS on GitHub](https://github.com/aspnet/WebHooks).</span></span>

## <a name="webhooks-overview"></a><span data-ttu-id="279ee-118">Visão geral de WebHooks</span><span class="sxs-lookup"><span data-stu-id="279ee-118">WebHooks Overview</span></span>

<span data-ttu-id="279ee-119">WebHooks é um padrão que significa que, dependendo de como ele é usado do serviço para serviço, mas a ideia básica é o mesmo.</span><span class="sxs-lookup"><span data-stu-id="279ee-119">WebHooks is a pattern which means that it varies how it is used from service to service but the basic idea is the same.</span></span> <span data-ttu-id="279ee-120">Você pode pensar WebHooks como um modelo simples de pub/sub no qual um usuário pode assinar eventos que ocorrem em outro lugar.</span><span class="sxs-lookup"><span data-stu-id="279ee-120">You can think of WebHooks as a simple pub/sub model where a user can subscribe to events happening elsewhere.</span></span> <span data-ttu-id="279ee-121">As notificações de eventos serão propagadas como solicitações HTTP POST que contém informações sobre o evento propriamente dito.</span><span class="sxs-lookup"><span data-stu-id="279ee-121">The event notifications are propagated as HTTP POST requests containing information about the event itself.</span></span>

<span data-ttu-id="279ee-122">Normalmente, a solicitação HTTP POST contém um objeto JSON ou dados de formulário HTML determinados pelo remetente WebHook, incluindo informações sobre o evento, fazendo com que o WebHook para disparar.</span><span class="sxs-lookup"><span data-stu-id="279ee-122">Typically the HTTP POST request contains a JSON object or HTML form data determined by the WebHook sender including information about the event causing the WebHook to trigger.</span></span> <span data-ttu-id="279ee-123">Por exemplo, um exemplo de um corpo de solicitação de POST do WebHook [GitHub](http://www.github.com/) se parece com isto como resultado de um novo problema seja aberto em um repositório específico:</span><span class="sxs-lookup"><span data-stu-id="279ee-123">For example, an example of a WebHook POST request body from [GitHub](http://www.github.com/) looks like this as a result of a new issue being opened in a particular repository:</span></span>

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

<span data-ttu-id="279ee-124">Para garantir que o WebHook é realmente do remetente pretendido, a solicitação POST é protegida de alguma forma e, em seguida, verificada pelo destinatário.</span><span class="sxs-lookup"><span data-stu-id="279ee-124">To ensure that the WebHook is indeed from the intended sender, the POST request is secured in some way and then verified by the receiver.</span></span> <span data-ttu-id="279ee-125">Por exemplo, [WebHooks do GitHub](https://developer.github.com/webhooks/) inclui um *X-Hub-assinatura* cabeçalho HTTP com um hash do corpo da solicitação que é verificado pela implementação do receptor para que você não precisa se preocupar sobre ele.</span><span class="sxs-lookup"><span data-stu-id="279ee-125">For example, [GitHub WebHooks](https://developer.github.com/webhooks/) includes an *X-Hub-Signature* HTTP header with a hash of the request body which is checked by the receiver implementation so you don't have to worry about it.</span></span>

<span data-ttu-id="279ee-126">O fluxo de WebHook geralmente vai algo parecido com isto:</span><span class="sxs-lookup"><span data-stu-id="279ee-126">The WebHook flow generally goes something like this:</span></span>

* <span data-ttu-id="279ee-127">O remetente de WebHook expõe eventos que um cliente pode se inscrever.</span><span class="sxs-lookup"><span data-stu-id="279ee-127">The WebHook sender exposes events that a client can subscribe to.</span></span> <span data-ttu-id="279ee-128">Os eventos descrevem alterações observáveis no sistema, por exemplo que um novo item de dados tenha sido inserido, que um processo foi concluído ou alguma outra coisa.</span><span class="sxs-lookup"><span data-stu-id="279ee-128">The events describe observable changes to the system, for example that a new data item has been inserted, that a process has completed, or something else.</span></span>

* <span data-ttu-id="279ee-129">Assina o receptor de WebHook, registrando um WebHook que consiste em quatro fatores:</span><span class="sxs-lookup"><span data-stu-id="279ee-129">The WebHook receiver subscribes by registering a WebHook consisting of four things:</span></span>

     1. <span data-ttu-id="279ee-130">Um URI para onde a notificação de eventos deve ser lançada na forma de uma solicitação HTTP POST;</span><span class="sxs-lookup"><span data-stu-id="279ee-130">A URI for where the event notification should be posted in the form of an HTTP POST request;</span></span>

     2. <span data-ttu-id="279ee-131">Um conjunto de filtros que descreve os eventos específicos para o qual o WebHook deva ser acionado;</span><span class="sxs-lookup"><span data-stu-id="279ee-131">A set of filters describing the particular events for which the WebHook should be fired;</span></span>

     3. <span data-ttu-id="279ee-132">Uma chave secreta que é usada para assinar a solicitação HTTP POST;</span><span class="sxs-lookup"><span data-stu-id="279ee-132">A secret key which is used to sign the HTTP POST request;</span></span>

     4. <span data-ttu-id="279ee-133">Dados adicionais que deve ser incluído na solicitação HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="279ee-133">Additional data which is to be included in the HTTP POST request.</span></span> <span data-ttu-id="279ee-134">Por exemplo, isso pode ser campos de cabeçalho HTTP adicionais ou propriedades incluídas no corpo da solicitação HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="279ee-134">This can for example be additional HTTP header fields or properties included in the HTTP POST request body.</span></span>

* <span data-ttu-id="279ee-135">Depois que um evento ocorre, os registros de WebHook correspondentes são encontrados e as solicitações de HTTP POST são enviadas.</span><span class="sxs-lookup"><span data-stu-id="279ee-135">Once an event happens, the matching WebHook registrations are found and HTTP POST requests are submitted.</span></span> <span data-ttu-id="279ee-136">Normalmente, a geração das solicitações HTTP POST serão repetidas várias vezes se por algum motivo, que o destinatário não está respondendo ou os resultados da solicitação HTTP POST em uma resposta de erro.</span><span class="sxs-lookup"><span data-stu-id="279ee-136">Typically, the generation of the HTTP POST requests are retried several times if for some reason the recipient is not responding or the HTTP POST request results in an error response.</span></span>

## <a name="webhooks-processing-pipeline"></a><span data-ttu-id="279ee-137">Pipeline de processamento de WebHooks</span><span class="sxs-lookup"><span data-stu-id="279ee-137">WebHooks Processing Pipeline</span></span>

<span data-ttu-id="279ee-138">O pipeline de processamento do Microsoft ASP.NET WebHooks para WebHooks de entrada tem esta aparência:</span><span class="sxs-lookup"><span data-stu-id="279ee-138">The Microsoft ASP.NET WebHooks processing pipeline for incoming WebHooks looks like this:</span></span>

![Pipeline de processamento de WebHooks do ASP.NET](_static/WebHookReceivers.png)

<span data-ttu-id="279ee-140">Os dois principais conceitos aqui são *receptores* e *manipuladores*:</span><span class="sxs-lookup"><span data-stu-id="279ee-140">The two key concepts here are *Receivers* and *Handlers*:</span></span>

* <span data-ttu-id="279ee-141">*Receptores* é responsável para lidar com o tipo específico de WebHook de um determinado remetente e impor verificações de segurança para garantir que a solicitação de WebHook é de fato do remetente pretendido.</span><span class="sxs-lookup"><span data-stu-id="279ee-141">*Receivers* are responsible for handling the particular flavor of WebHook from a given sender and for enforcing security checks to ensure that the WebHook request indeed is from the intended sender.</span></span>

* <span data-ttu-id="279ee-142">*Manipuladores* normalmente são onde o código do usuário executa o processamento de WebHook específico.</span><span class="sxs-lookup"><span data-stu-id="279ee-142">*Handlers* are typically where user code runs processing the particular WebHook.</span></span>

<span data-ttu-id="279ee-143">Nos seguintes nós, esses conceitos são descritos em mais detalhes.</span><span class="sxs-lookup"><span data-stu-id="279ee-143">In the following nodes these concepts are described in more details.</span></span>

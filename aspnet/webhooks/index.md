---
uid: webhooks/index
title: Visão geral do ASP.NET WebHooks | Microsoft Docs
author: rick-anderson
description: Uma introdução ao ASP.NET WebHooks.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 5e2843f0-f499-448f-a712-33d4e9858321
ms.technology: ''
ms.prod: .net-framework
ms.openlocfilehash: 52399c23cdf393a2f7f94661fd48098ced65948c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
ms.locfileid: "26530045"
---
# <a name="aspnet-webhooks-overview"></a>Visão geral do ASP.NET WebHooks

WebHooks é um padrão HTTP leve, fornecendo um modelo de publicação/assinatura simples para conectar os serviços de APIs da Web e SaaS. Quando ocorre um evento em um serviço, uma notificação é enviada na forma de uma solicitação HTTP POST para assinantes registrados. A solicitação POST contém informações sobre o evento que torna possível para o receptor funcionar adequadamente.

Devido à sua simplicidade, WebHooks já estão expostos por um grande número de serviços incluindo [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp ](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/)e muito mais. Por exemplo, um WebHook pode indicar que um arquivo foi alterado no [Dropbox](http://dropbox.com/), ou uma alteração de código foi confirmada no GitHub ou um pagamento foi iniciado no [PayPal](http://www.paypal.com/), ou um cartão foi criado no [ Trello](http://www.trello.com/). As possibilidades são infinitas!

Microsoft ASP.NET WebHooks torna mais fácil enviar e receber WebHooks como parte do seu aplicativo ASP.NET:

* No lado de recepção, ele fornece um modelo comum de recebimento e processamento WebHooks de qualquer número de provedores de WebHook. Ele sair da caixa com suporte para [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Pusher](http://www.pusher.com), [Salesforce](http://www.salesforce.com), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/),[ WordPress](http://www.wordpress.com) e [Zendesk](https://www.zendesk.com/) , mas é fácil adicionar suporte para obter mais informações.

* No lado de envio, ele fornece suporte para gerenciar e armazenar assinaturas, bem como para enviar notificações de eventos para o conjunto certo de assinantes. Isso permite que você defina seu próprio conjunto de eventos que os assinantes podem assinar e notificá-lo quando as coisas acontece.

As duas partes podem ser usadas em conjunto ou distância dependendo do cenário. Se você só precisa receber WebHooks de outros serviços, em seguida, você pode usar apenas a parte do destinatário; Se você quiser expor ganchos para outras pessoas para consumir, você pode fazer exatamente isso.

O código tem como alvo o ASP.NET Web API 2 e 5 do ASP.NET MVC e está disponível como [OSS no GitHub](https://github.com/aspnet/WebHooks).

## <a name="webhooks-overview"></a>Visão geral de WebHooks

WebHooks é um padrão que significa que, dependendo de como ele é usado do serviço ao serviço, mas a ideia básica é o mesmo. Você pode pensar WebHooks como um modelo de publicação/assinatura simples em que um usuário pode inscrever-se para eventos que ocorrem em outro lugar. As notificações de eventos são propagadas como solicitações HTTP POST que contém informações sobre o próprio evento.

Normalmente, a solicitação HTTP POST contém um objeto JSON ou dados do formulário HTML determinados pelo remetente WebHook incluindo informações sobre o evento que causa o WebHook para disparar. Por exemplo, um exemplo de um corpo de solicitação POST de WebHook de [GitHub](http://www.github.com/) tem esta aparência como resultado de um novo problema que está sendo aberto em um repositório específico:

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

Para garantir que o WebHook é realmente do remetente pretendido, a solicitação POST é protegida de alguma forma e verificada pelo destinatário. Por exemplo, [GitHub WebHooks](https://developer.github.com/webhooks/) inclui um *X-Hub-assinatura* cabeçalho HTTP com um hash do corpo da solicitação que é verificado pela implementação do destinatário, para que você não precisa se preocupar sobre ele.

O fluxo de WebHook geralmente é semelhante a esta:

* O remetente de WebHook expõe eventos que um cliente pode se inscrever. Os eventos descrevem observáveis alterações no sistema, por exemplo que um novo item de dados tenha sido inseridas, um processo ou algo mais.

* O receptor de WebHook assina Registrando um WebHook consiste em quatro aspectos:

     1. Um URI para onde a notificação de evento deve ser lançada na forma de uma solicitação HTTP POST;

     2. Um conjunto de filtros que descreve os eventos específicos para o qual o WebHook deva ser acionado;

     3. Uma chave de segredo que é usada para assinar a solicitação HTTP POST;

     4. Dados adicionais que deve ser incluído na solicitação HTTP POST. Por exemplo, isso pode ser incluídas no corpo da solicitação HTTP POST de propriedades ou campos adicionais de cabeçalho HTTP.

* Depois que um evento ocorre, os registros de WebHook correspondentes forem encontrados e solicitações HTTP POST são enviadas. Normalmente, a geração das solicitações HTTP POST são repetidas várias vezes se por algum motivo que o destinatário não está respondendo ou os resultados da solicitação HTTP POST em uma resposta de erro.

## <a name="webhooks-processing-pipeline"></a>Pipeline de processamento de WebHooks

O pipeline de processamento do Microsoft ASP.NET WebHooks para WebHooks de entrada tem esta aparência:

![Pipeline de processamento do ASP.NET WebHooks](_static/WebHookReceivers.png)

Os dois principais conceitos aqui são *receptores* e *manipuladores*:

* *Receptores de* é responsável para lidar com o tipo específico de WebHook de um determinado remetente e impor verificações de segurança para garantir que a solicitação de WebHook é realmente do remetente pretendido.

* *Manipuladores* são normalmente onde o código do usuário executa o processamento de WebHook específico.

Nos seguintes nós esses conceitos são descritos em mais detalhes.

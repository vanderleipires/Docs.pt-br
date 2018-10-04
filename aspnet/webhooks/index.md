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
# <a name="aspnet-webhooks-overview"></a>Visão geral de WebHooks do ASP.NET

WebHooks é um padrão HTTP leve, fornecendo um modelo simples de pub/sub para conectar os serviços de SaaS e APIs da Web. Quando ocorre um evento em um serviço, uma notificação é enviada na forma de uma solicitação HTTP POST para assinantes registrados. A solicitação POST contém informações sobre o evento que torna possível para o destinatário para agir de acordo.

Devido à sua simplicidade, WebHooks já são expostos por um grande número de serviços, incluindo [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp ](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/)e muito mais. Por exemplo, um WebHook pode indicar que um arquivo foi alterado nas [Dropbox](http://dropbox.com/), ou uma alteração de código foi confirmada no GitHub, ou um pagamento foi iniciado no [PayPal](http://www.paypal.com/), ou um cartão foi criado no [ Trello](http://www.trello.com/). As possibilidades são infinitas!

Microsoft ASP.NET WebHooks torna mais fácil de enviar e receber WebHooks como parte do seu aplicativo ASP.NET:

* No lado de recepção, ele fornece um modelo comum de recebimento e processamento de WebHooks de qualquer número de provedores de WebHook. Ele vem pronta com suporte para [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Pusher](http://www.pusher.com), [Salesforce](http://www.salesforce.com), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/),[ WordPress](http://www.wordpress.com) e [Zendesk](https://www.zendesk.com/) mas é fácil adicionar suporte para obter mais informações.

* No lado de envio, ele fornece suporte para gerenciar e armazenar as assinaturas, bem como para enviar notificações de eventos para o conjunto certo de assinantes. Isso permite que você defina seu próprio conjunto de eventos que os assinantes podem assinar e notificá-lo quando as coisas acontece.

As duas partes podem ser usadas em conjunto ou separadamente, dependendo do cenário. Se você só precisa receber WebHooks de outros serviços, em seguida, você pode usar apenas a parte do receptor; Se você quiser expor WebHooks para outras pessoas para consumir, em seguida, você pode fazer exatamente isso.

O código tem como alvo o API Web 2 ASP.NET e ASP.NET MVC 5 e está disponível como [OSS no GitHub](https://github.com/aspnet/WebHooks).

## <a name="webhooks-overview"></a>Visão geral de WebHooks

WebHooks é um padrão que significa que, dependendo de como ele é usado do serviço para serviço, mas a ideia básica é o mesmo. Você pode pensar WebHooks como um modelo simples de pub/sub no qual um usuário pode assinar eventos que ocorrem em outro lugar. As notificações de eventos serão propagadas como solicitações HTTP POST que contém informações sobre o evento propriamente dito.

Normalmente, a solicitação HTTP POST contém um objeto JSON ou dados de formulário HTML determinados pelo remetente WebHook, incluindo informações sobre o evento, fazendo com que o WebHook para disparar. Por exemplo, um exemplo de um corpo de solicitação de POST do WebHook [GitHub](http://www.github.com/) se parece com isto como resultado de um novo problema seja aberto em um repositório específico:

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

Para garantir que o WebHook é realmente do remetente pretendido, a solicitação POST é protegida de alguma forma e, em seguida, verificada pelo destinatário. Por exemplo, [WebHooks do GitHub](https://developer.github.com/webhooks/) inclui um *X-Hub-assinatura* cabeçalho HTTP com um hash do corpo da solicitação que é verificado pela implementação do receptor para que você não precisa se preocupar sobre ele.

O fluxo de WebHook geralmente vai algo parecido com isto:

* O remetente de WebHook expõe eventos que um cliente pode se inscrever. Os eventos descrevem alterações observáveis no sistema, por exemplo que um novo item de dados tenha sido inserido, que um processo foi concluído ou alguma outra coisa.

* Assina o receptor de WebHook, registrando um WebHook que consiste em quatro fatores:

     1. Um URI para onde a notificação de eventos deve ser lançada na forma de uma solicitação HTTP POST;

     2. Um conjunto de filtros que descreve os eventos específicos para o qual o WebHook deva ser acionado;

     3. Uma chave secreta que é usada para assinar a solicitação HTTP POST;

     4. Dados adicionais que deve ser incluído na solicitação HTTP POST. Por exemplo, isso pode ser campos de cabeçalho HTTP adicionais ou propriedades incluídas no corpo da solicitação HTTP POST.

* Depois que um evento ocorre, os registros de WebHook correspondentes são encontrados e as solicitações de HTTP POST são enviadas. Normalmente, a geração das solicitações HTTP POST serão repetidas várias vezes se por algum motivo, que o destinatário não está respondendo ou os resultados da solicitação HTTP POST em uma resposta de erro.

## <a name="webhooks-processing-pipeline"></a>Pipeline de processamento de WebHooks

O pipeline de processamento do Microsoft ASP.NET WebHooks para WebHooks de entrada tem esta aparência:

![Pipeline de processamento de WebHooks do ASP.NET](_static/WebHookReceivers.png)

Os dois principais conceitos aqui são *receptores* e *manipuladores*:

* *Receptores* é responsável para lidar com o tipo específico de WebHook de um determinado remetente e impor verificações de segurança para garantir que a solicitação de WebHook é de fato do remetente pretendido.

* *Manipuladores* normalmente são onde o código do usuário executa o processamento de WebHook específico.

Nos seguintes nós, esses conceitos são descritos em mais detalhes.

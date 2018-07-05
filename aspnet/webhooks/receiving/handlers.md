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
# <a name="aspnet-webhooks-handlers"></a>Manipuladores de WebHooks do ASP.NET

Depois que as solicitações de WebHooks foi validada por um receptor de WebHook, está pronto para ser processado pelo código do usuário. É aí que *manipuladores de* entram. Manipuladores derivam de [IWebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) interface, mas normalmente usa o [WebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) classe em vez de derivar diretamente da interface do.

Uma solicitação de WebHook pode ser processada por um ou mais manipuladores. Manipuladores são chamados na ordem com base em suas respectivas *ordem* propriedade indo do mais baixo para o mais alto em que ordem é um inteiro simples (sugerido para ser entre 1 e 100):

![Diagrama de propriedade de ordem do manipulador de WebHook](_static/Handlers.png)

Um manipulador pode, opcionalmente, defina as *resposta* propriedade sobre a WebHookHandlerContext que levará o processamento a parada e a resposta a ser enviada de volta como a resposta HTTP para o WebHook. No caso acima, não será chamada C de manipulador porque ele tem uma ordem mais alta do que B e B define a resposta.

Definição da resposta normalmente só é relevante para WebHooks em que a resposta pode transportar informações de volta para a API de origem. Por exemplo, esse é o caso com os WebHooks do Slack em que a resposta é postada de volta o canal de onde veio o WebHook. Manipuladores podem definir a propriedade de receptor se desejam receber WebHooks de receptor em particular. Se eles não definir o receptor, eles são chamados para todos eles.

Outro uso comum de uma resposta é usar um *410 perdido* resposta para indicar que o WebHook não está ativo e nenhuma solicitação adicional deve ser enviada.

Por padrão, um manipulador será chamado por todos os receptores de WebHook. No entanto, se o *receptor* estiver definida como o nome de um manipulador, em seguida, esse manipulador só receberá solicitações de WebHook do receptor.

## <a name="processing-a-webhook"></a>Processamento de um WebHook

Quando um manipulador é chamado, ele obtém uma [WebHookHandlerContext](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandlerContext.cs) que contém informações sobre a solicitação de WebHook. Os dados, normalmente o corpo da solicitação HTTP, estão disponíveis na *dados* propriedade.

O tipo de dados é, normalmente, dados de formulário HTML ou JSON, mas é possível converter para um tipo mais específico, se desejado. Por exemplo, os WebHooks personalizados gerados pelo WebHooks do ASP.NET pode ser convertidos no tipo [CustomNotifications](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers.Custom/WebHooks/CustomNotifications.cs) da seguinte maneira:

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

  ## <a name="queued-processing"></a>Processamento em fila

A maioria dos remetentes de WebHook reenviará um WebHook, se uma resposta não é gerada dentro de alguns segundos. Isso significa que seu manipulador deve concluir o processamento dentro desse período, não para que ele seja chamado novamente.

Se o processamento leva mais tempo ou, melhor é tratado separadamente, o [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) pode ser usado para enviar a solicitação de WebHook a uma fila, por exemplo [fila de armazenamento do Azure](https://msdn.microsoft.com/library/azure/dd179353.aspx).

Uma estrutura de tópicos de um [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) implementação é fornecida aqui:

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

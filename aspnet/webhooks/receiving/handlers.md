---
uid: webhooks/receiving/handlers
title: Manipuladores de ASP.NET WebHooks | Microsoft Docs
author: rick-anderson
description: "Como manipular solicitações em WebHooks do ASP.NET."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: a55b0d20-9c90-4bd3-a471-20da6f569f0c
ms.technology: 
ms.prod: .net-framework
ms.openlocfilehash: 3aaef756ee00d7e44aa757062e1ef297312ecf22
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
# <a name="aspnet-webhooks-handlers"></a>Manipuladores do ASP.NET WebHooks

Depois que as solicitações de WebHooks foi validada por um receptor de WebHook, está pronto para ser processado pelo código do usuário. Isso é onde *manipuladores* entrar. Manipuladores derivam o [IWebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) interface mas, normalmente, usa o [WebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) classe em vez de derivar diretamente na interface do.

Uma solicitação de WebHook pode ser processada por um ou mais manipuladores. Manipuladores são chamados em ordem com base em suas respectivas *ordem* propriedade vai do menor ao maior onde a ordem é um inteiro simple (sugerido para ser entre 1 e 100):

![Diagrama de propriedade de ordem do manipulador de WebHook](_static/Handlers.png)

Opcionalmente, poderá definir um manipulador de *resposta* propriedade o WebHookHandlerContext que levará o processamento a parada e a resposta a ser enviada de volta como a resposta HTTP para o WebHook. Nesse caso, não será chamada C de manipulador porque ela tem uma ordem mais alta que B e B define a resposta.

Definindo a resposta normalmente só é relevante para WebHooks onde a resposta pode transmitir informações de volta para a API de origem. Por exemplo, esse é o caso com atraso WebHooks onde a resposta é postada para o canal de onde veio o WebHook. Manipuladores podem definir a propriedade de destinatário se desejam receber WebHooks esse destinatário específico. Se eles não define o receptor, eles são chamados para todos eles.

Um outro uso comum de uma resposta é usar um *Gone 410* resposta para indicar que o WebHook não está ativo e nenhuma solicitação adicional deve ser enviada.

Por padrão, um manipulador será chamado por todos os destinatários de WebHook. No entanto, se o *receptor* propriedade é definida como o nome de um manipulador, em seguida, esse manipulador só recebe solicitações de WebHook do que o receptor.

## <a name="processing-a-webhook"></a>Um WebHook de processamento

Quando um manipulador é chamado, ele obtém uma [WebHookHandlerContext](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandlerContext.cs) que contém informações sobre a solicitação de WebHook. Os dados, normalmente o corpo da solicitação HTTP, estão disponíveis no *dados* propriedade.

O tipo de dados é normalmente dados do formulário HTML ou JSON, mas é possível converter para um tipo mais específico, se desejado. Por exemplo, o WebHooks personalizados gerados pelo ASP.NET WebHooks pode ser convertido para o tipo [CustomNotifications](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers.Custom/WebHooks/CustomNotifications.cs) da seguinte maneira:

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

A maioria dos remetentes de WebHook reenviaria um WebHook se uma resposta não é gerada dentro de alguns segundos. Isso significa que o manipulador deve concluir o processamento dentro desse período, não para que ele seja chamado novamente.

Se o processamento leva mais tempo, ou melhor é tratado separadamente o [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) pode ser usado para enviar a solicitação de WebHook a uma fila, por exemplo [fila de armazenamento do Azure](https://msdn.microsoft.com/en-us/library/azure/dd179353.aspx).

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

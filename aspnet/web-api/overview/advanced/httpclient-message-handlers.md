---
uid: web-api/overview/advanced/httpclient-message-handlers
title: Manipuladores de mensagens de HttpClient na API Web ASP.NET | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 10/01/2012
ms.assetid: 5a4b6c80-b2e9-4710-8969-d5076f7f82b8
msc.legacyurl: /web-api/overview/advanced/httpclient-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: 764244d1299d8cfcb59c3f15d63b42ebff4f6ac0
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41824813"
---
<a name="httpclient-message-handlers-in-aspnet-web-api"></a>Manipuladores de mensagens de HttpClient na API Web ASP.NET
====================
por [Mike Wasson](https://github.com/MikeWasson)

Um *manipulador de mensagens* é uma classe que recebe uma solicitação HTTP e retorna uma resposta HTTP.

Normalmente, uma série de manipuladores de mensagens são encadeados juntos. O primeiro manipulador recebe uma solicitação HTTP, executa algum processamento e fornece a solicitação para o próximo manipulador. Em algum momento, a resposta é criada e retorna a cadeia. Esse padrão é chamado de um *delegando* manipulador.

![](httpclient-message-handlers/_static/image1.png)

No lado do cliente, o **HttpClient** classe usa um manipulador de mensagens para processar solicitações. O manipulador padrão é **HttpClientHandler**, que envia a solicitação pela rede e obtém a resposta do servidor. Você pode inserir os manipuladores de mensagens personalizadas no pipeline do cliente:

![](httpclient-message-handlers/_static/image2.png)

> [!NOTE]
> API Web ASP.NET também usa os manipuladores de mensagens no lado do servidor. Para obter mais informações, consulte [manipuladores de mensagens HTTP](http-message-handlers.md).


## <a name="custom-message-handlers"></a>Manipuladores de mensagens personalizado

Para escrever um manipulador de mensagens personalizadas, derivam **System.Net.Http.DelegatingHandler** e substitua o **SendAsync** método. Aqui está a assinatura do método:

[!code-csharp[Main](httpclient-message-handlers/samples/sample1.cs)]

O método utiliza um **HttpRequestMessage** como entrada e retorna de maneira assíncrona uma **HttpResponseMessage**. Uma implementação típica faz o seguinte:

1. Processe a mensagem de solicitação.
2. Chamar `base.SendAsync` para enviar a solicitação para o manipulador interno.
3. O manipulador interno retorna uma mensagem de resposta. (Essa etapa é assíncrona).
4. Processar a resposta e retorná-lo ao chamador.

O exemplo a seguir mostra um manipulador de mensagens que adiciona um cabeçalho personalizado para a solicitação de saída:

[!code-csharp[Main](httpclient-message-handlers/samples/sample2.cs)]

A chamada para `base.SendAsync` é assíncrona. Se o manipulador faz qualquer trabalho após esta chamada, use o **await** palavra-chave para retomar a execução após a conclusão do método. O exemplo a seguir mostra um manipulador que registra os códigos de erro. O registro em log em si não é muito interessante, mas o exemplo mostra como obter a resposta dentro do manipulador.

[!code-csharp[Main](httpclient-message-handlers/samples/sample3.cs?highlight=10,13)]

## <a name="adding-message-handlers-to-the-client-pipeline"></a>Adicionando manipuladores de mensagens para o Pipeline de cliente

Para adicionar manipuladores personalizados para **HttpClient**, use o **HttpClientFactory.Create** método:

[!code-csharp[Main](httpclient-message-handlers/samples/sample4.cs)]

Manipuladores de mensagens são chamados na ordem em que você os transmite para o **criar** método. Porque os manipuladores são aninhados, a mensagem de resposta viaja na outra direção. Ou seja, o último manipulador é o primeiro para obter a mensagem de resposta.

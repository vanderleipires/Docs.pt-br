---
uid: web-api/overview/advanced/http-message-handlers
title: Manipuladores de mensagens de HTTP na API da Web ASP.NET | Microsoft Docs
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/13/2012
ms.topic: article
ms.assetid: 9002018b-3aa3-4358-bb1c-fbb5bc751d01
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/http-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: 931ad3330d5e4dc2424ab37b8a6e2a3d123a8684
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="http-message-handlers-in-aspnet-web-api"></a>Manipuladores de mensagens de HTTP na API da Web ASP.NET
====================
por [Mike Wasson](https://github.com/MikeWasson)

Um *manipulador de mensagens* é uma classe que recebe uma solicitação HTTP e retorna uma resposta HTTP. Manipuladores de mensagens derivam abstrata **HttpMessageHandler** classe.

Normalmente, uma série de manipuladores de mensagens são encadeados juntos. O primeiro manipulador recebe uma solicitação HTTP, executa algum processamento e fornece a solicitação para o manipulador de Avançar. Em algum momento, a resposta é criada e retorna a cadeia. Esse padrão é chamado um *delegando* manipulador.

![](http-message-handlers/_static/image1.png)

## <a name="server-side-message-handlers"></a>Manipuladores de mensagens do servidor

No lado do servidor, o pipeline da API Web usa alguns manipuladores de mensagens internas:

- **HttpServer** obtém a solicitação do host.
- **HttpRoutingDispatcher** envia a solicitação com base na rota.
- **HttpControllerDispatcher** envia a solicitação para um controlador Web API.

Você pode adicionar manipuladores personalizados para o pipeline. Manipuladores de mensagens são bons para resolvem preocupações que operam no nível de HTTP mensagens (em vez de ações do controlador). Por exemplo, pode ser um manipulador de mensagens:

- Ler ou modificar os cabeçalhos de solicitação.
- Adicione um cabeçalho de resposta para respostas.
- Valide solicitações antes que elas atinjam o controlador.

Este diagrama mostra dois manipuladores personalizados inseridos no pipeline:

![](http-message-handlers/_static/image2.png)

> [!NOTE]
> No lado do cliente, HttpClient também utiliza manipuladores de mensagens. Para obter mais informações, consulte [manipuladores de mensagens HttpClient](httpclient-message-handlers.md).


## <a name="custom-message-handlers"></a>Manipuladores de mensagens personalizadas

Para escrever um manipulador de mensagens personalizadas, derivam **System.Net.Http.DelegatingHandler** e substituir o **SendAsync** método. Esse método tem a seguinte assinatura:

[!code-csharp[Main](http-message-handlers/samples/sample1.cs)]

O método usa um **HttpRequestMessage** como entrada e retorna de maneira assíncrona um **HttpResponseMessage**. Uma implementação típica faz o seguinte:

1. Processe a mensagem de solicitação.
2. Chamar `base.SendAsync` para enviar a solicitação para o manipulador interno.
3. O manipulador interno retorna uma mensagem de resposta. (Esta etapa é assíncrona).
4. Processar a resposta e retorná-lo ao chamador.

Aqui está um exemplo simples:

[!code-csharp[Main](http-message-handlers/samples/sample2.cs)]

> [!NOTE]
> A chamada para `base.SendAsync` é assíncrona. Se o manipulador faz qualquer trabalho após essa chamada, use o **await** palavra-chave, como mostrado.


Um manipulador de delegação pode também ignorar o manipulador interno e criar diretamente a resposta:

[!code-csharp[Main](http-message-handlers/samples/sample3.cs)]

Se a delegação de um manipulador cria a resposta sem chamar `base.SendAsync`, a solicitação ignora o resto do pipeline. Isso pode ser útil para um manipulador que valida a solicitação (Criando uma resposta de erro).

![](http-message-handlers/_static/image3.png)

## <a name="adding-a-handler-to-the-pipeline"></a>Adicionando um manipulador para o Pipeline

Para adicionar um manipulador de mensagens no lado do servidor, adicione o manipulador para o **HttpConfiguration.MessageHandlers** coleção. Se você usou o modelo de "Aplicativo de Web do ASP.NET MVC 4" para criar o projeto, você pode fazer isso dentro de **WebApiConfig** classe:

[!code-csharp[Main](http-message-handlers/samples/sample4.cs)]

Manipuladores de mensagens são chamadas na mesma ordem em que aparecem na **MessageHandlers** coleção. Porque eles são aninhados, a mensagem de resposta são transferidos na outra direção. Ou seja, o último manipulador é a primeira a receber a mensagem de resposta.

Observe que você não precisa definir os manipuladores internos; a estrutura da API Web conecta-se automaticamente os manipuladores de mensagens.

Se você estiver [auto-hospedagem](../older-versions/self-host-a-web-api.md), criar uma instância do **HttpSelfHostConfiguration** classe e adicione os manipuladores para o **MessageHandlers** coleção.

[!code-csharp[Main](http-message-handlers/samples/sample5.cs)]

Agora vamos examinar alguns exemplos de manipuladores de mensagens personalizadas.

## <a name="example-x-http-method-override"></a>Exemplo: X-HTTP-Method-Override

X-HTTP-Method-Override é um cabeçalho HTTP não padrão. Ele é projetado para clientes que não é possível enviar a determinados tipos de solicitação HTTP, como PUT ou DELETE. Em vez disso, o cliente envia uma solicitação POST e define o cabeçalho X-HTTP-Method-Override para o método desejado. Por exemplo:

[!code-console[Main](http-message-handlers/samples/sample6.cmd)]

Aqui está um manipulador de mensagens que adiciona suporte para X-HTTP-Method-Override:

[!code-csharp[Main](http-message-handlers/samples/sample7.cs)]

No **SendAsync** método, o manipulador verifica se a mensagem de solicitação é uma solicitação POST e se ele contém o cabeçalho X-HTTP-Method-Override. Nesse caso, ele valida o valor do cabeçalho e, em seguida, modifica o método de solicitação. Finalmente, chama o manipulador `base.SendAsync` para passar a mensagem para o manipulador de Avançar.

Quando a solicitação alcança o **HttpControllerDispatcher** classe **HttpControllerDispatcher** encaminhará a solicitação com base no método de solicitação atualizada.

## <a name="example-adding-a-custom-response-header"></a>Exemplo: Adicionar um cabeçalho de resposta personalizados

Aqui está um manipulador de mensagens que adiciona um cabeçalho personalizado para cada mensagem de resposta:

[!code-csharp[Main](http-message-handlers/samples/sample8.cs)]

Primeiro, o manipulador chama `base.SendAsync` para passar a solicitação para o manipulador de mensagens internas. O manipulador interno retorna uma mensagem de resposta, mas ele faz isso de forma assíncrona usando um **tarefa&lt;T&gt;**  objeto. A mensagem de resposta não está disponível até `base.SendAsync` é concluída de forma assíncrona.

Este exemplo usa o **await** palavra-chave para executar o trabalho assincronamente após `SendAsync` é concluída. Se você estiver direcionando o .NET Framework 4.0, use o **tarefa**&lt;T&gt;**. Método ContinueWith** método:

[!code-csharp[Main](http-message-handlers/samples/sample9.cs)]

## <a name="example-checking-for-an-api-key"></a>Exemplo: Verificação de uma chave de API

Alguns serviços da web exigem que os clientes incluir uma chave de API na sua solicitação. O exemplo a seguir mostra como um manipulador de mensagens pode verificar solicitações para uma chave de API válida:

[!code-csharp[Main](http-message-handlers/samples/sample10.cs)]

Este manipulador procura a chave de API na cadeia de caracteres de consulta URI. (Neste exemplo, vamos supor que a chave é uma cadeia de caracteres estática. Uma implementação real provavelmente usaria validação mais complexa.) Se a cadeia de caracteres de consulta contém a chave, o manipulador passa a solicitação para o manipulador interno.

Se a solicitação não tiver uma chave válida, o manipulador cria uma mensagem de resposta com status 403, proibido. Nesse caso, o manipulador não chamar `base.SendAsync`, portanto o manipulador interno nunca recebe a solicitação, nem o controlador. Portanto, o controlador pode presumir que todas as solicitações de entrada tem uma chave de API válida.

> [!NOTE]
> Se a chave de API só se aplica a determinadas ações do controlador, considere usar um filtro de ação em vez de um manipulador de mensagens. Filtros de ação executado após o roteamento do URI é executado.


## <a name="per-route-message-handlers"></a>Manipuladores de mensagens por rota

Manipuladores de **HttpConfiguration.MessageHandlers** coleção se aplicam globalmente.

Como alternativa, você pode adicionar um manipulador de mensagens para uma rota específica quando você define a rota:

[!code-csharp[Main](http-message-handlers/samples/sample11.cs?highlight=16)]

Neste exemplo, se o URI de solicitação corresponde a "Route2", a solicitação é enviada para `MessageHandler2`. O diagrama a seguir mostra o pipeline dessas duas rotas:

![](http-message-handlers/_static/image4.png)

Observe que `MessageHandler2` substitui o padrão **HttpControllerDispatcher**. Neste exemplo, `MessageHandler2` cria a resposta e, em seguida, solicita que corresponde a "Route2" nunca ir para um controlador. Isso permite que você substituir o mecanismo de controlador de API da Web inteiro com seu próprio ponto de extremidade personalizado.

Como alternativa, um manipulador de mensagens por rota pode delegar a **HttpControllerDispatcher**, que então envia a um controlador.

![](http-message-handlers/_static/image5.png)

O código a seguir mostra como configurar esta rota:

[!code-csharp[Main](http-message-handlers/samples/sample12.cs)]

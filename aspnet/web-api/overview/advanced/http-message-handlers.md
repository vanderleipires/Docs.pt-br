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
<a name="http-message-handlers-in-aspnet-web-api"></a>Manipuladores de mensagens de HTTP na API Web ASP.NET
====================
por [Mike Wasson](https://github.com/MikeWasson)

Um *manipulador de mensagens* é uma classe que recebe uma solicitação HTTP e retorna uma resposta HTTP. Manipuladores de mensagens derivam abstrata **HttpMessageHandler** classe.

Normalmente, uma série de manipuladores de mensagens são encadeados juntos. O primeiro manipulador recebe uma solicitação HTTP, executa algum processamento e fornece a solicitação para o próximo manipulador. Em algum momento, a resposta é criada e retorna a cadeia. Esse padrão é chamado de um *delegando* manipulador.

![](http-message-handlers/_static/image1.png)

## <a name="server-side-message-handlers"></a>Manipuladores de mensagens do lado do servidor

No lado do servidor, o pipeline da API Web usa alguns manipuladores de mensagem interna:

- **Servidor HTTP** obtém a solicitação do host.
- **HttpRoutingDispatcher** envia a solicitação com base na rota.
- **HttpControllerDispatcher** envia a solicitação para um controlador de API da Web.

Você pode adicionar manipuladores personalizados para o pipeline. Manipuladores de mensagens são bons para interesses transversais que operam no nível de HTTP mensagens (em vez de ações do controlador). Por exemplo, um manipulador de mensagens pode:

- Ler ou modificar os cabeçalhos de solicitação.
- Adicione um cabeçalho de resposta para as respostas.
- Valide as solicitações antes que elas atinjam o controlador.

Este diagrama mostra dois manipuladores personalizados inseridos no pipeline:

![](http-message-handlers/_static/image2.png)

> [!NOTE]
> No lado do cliente, o HttpClient também utiliza manipuladores de mensagens. Para obter mais informações, consulte [manipuladores de mensagens de HttpClient](httpclient-message-handlers.md).


## <a name="custom-message-handlers"></a>Manipuladores de mensagens personalizado

Para escrever um manipulador de mensagens personalizadas, derivam **System.Net.Http.DelegatingHandler** e substitua o **SendAsync** método. Esse método tem a seguinte assinatura:

[!code-csharp[Main](http-message-handlers/samples/sample1.cs)]

O método utiliza um **HttpRequestMessage** como entrada e retorna de maneira assíncrona uma **HttpResponseMessage**. Uma implementação típica faz o seguinte:

1. Processe a mensagem de solicitação.
2. Chamar `base.SendAsync` para enviar a solicitação para o manipulador interno.
3. O manipulador interno retorna uma mensagem de resposta. (Essa etapa é assíncrona).
4. Processar a resposta e retorná-lo ao chamador.

Aqui está um exemplo trivial:

[!code-csharp[Main](http-message-handlers/samples/sample2.cs)]

> [!NOTE]
> A chamada para `base.SendAsync` é assíncrona. Se o manipulador faz qualquer trabalho após esta chamada, use o **await** palavra-chave, conforme mostrado.


Um manipulador de delegação pode também ignorar o manipulador interno e criar diretamente a resposta:

[!code-csharp[Main](http-message-handlers/samples/sample3.cs)]

Se a delegação de um manipulador cria a resposta sem chamar `base.SendAsync`, a solicitação irá ignorar o restante do pipeline. Isso pode ser útil para um manipulador que valida a solicitação (Criando uma resposta de erro).

![](http-message-handlers/_static/image3.png)

## <a name="adding-a-handler-to-the-pipeline"></a>Adicionando um manipulador para o Pipeline

Para adicionar um manipulador de mensagens no lado do servidor, adicione o manipulador para o **HttpConfiguration.MessageHandlers** coleção. Se você usou o modelo "Aplicativo de Web do ASP.NET MVC 4" para criar o projeto, você pode fazer isso dentro de **WebApiConfig** classe:

[!code-csharp[Main](http-message-handlers/samples/sample4.cs)]

Manipuladores de mensagens são chamados na mesma ordem em que aparecem no **MessageHandlers** coleção. Porque eles estão aninhados, a mensagem de resposta viaja na outra direção. Ou seja, o último manipulador é o primeiro para obter a mensagem de resposta.

Observe que você não precisa definir os manipuladores internos; a estrutura da API Web se conecta automaticamente os manipuladores de mensagem.

Se você estiver [auto-hospedagem](../older-versions/self-host-a-web-api.md), crie uma instância das **HttpSelfHostConfiguration** de classe e adicione os manipuladores para o **MessageHandlers** coleção.

[!code-csharp[Main](http-message-handlers/samples/sample5.cs)]

Agora vamos examinar alguns exemplos de manipuladores de mensagens personalizadas.

## <a name="example-x-http-method-override"></a>Exemplo: X-HTTP-Method-Override

X-HTTP-Method-Override é um cabeçalho HTTP não padrão. Ele é projetado para clientes que não é possível enviar determinados tipos de solicitação HTTP, como PUT ou DELETE. Em vez disso, o cliente envia uma solicitação POST e define o cabeçalho X-HTTP-Method-Override para o método desejado. Por exemplo:

[!code-console[Main](http-message-handlers/samples/sample6.cmd)]

Aqui está um manipulador de mensagens que adiciona suporte para X-HTTP-Method-Override:

[!code-csharp[Main](http-message-handlers/samples/sample7.cs)]

No **SendAsync** método, o manipulador verifica se a mensagem de solicitação é uma solicitação POST e se ele contém o cabeçalho X-HTTP-Method-Override. Nesse caso, ele valida o valor do cabeçalho e, em seguida, modifica o método de solicitação. Por fim, chama o manipulador `base.SendAsync` para passar a mensagem para o próximo manipulador.

Quando a solicitação atinja o **HttpControllerDispatcher** classe, **HttpControllerDispatcher** irá rotear a solicitação com base no método de solicitação atualizado.

## <a name="example-adding-a-custom-response-header"></a>Exemplo: Adicionar um cabeçalho de resposta personalizada

Aqui está um manipulador de mensagens que adiciona um cabeçalho personalizado para cada mensagem de resposta:

[!code-csharp[Main](http-message-handlers/samples/sample8.cs)]

Primeiro, o manipulador chama `base.SendAsync` para passar a solicitação para o manipulador de mensagens internas. O manipulador interno retorna uma mensagem de resposta, mas ele faz isso de forma assíncrona usando um **tarefa&lt;T&gt;**  objeto. A mensagem de resposta não está disponível até `base.SendAsync` for concluída de forma assíncrona.

Este exemplo usa o **await** palavra-chave para executar o trabalho assincronamente após `SendAsync` é concluída. Se você estiver direcionando o .NET Framework 4.0, use o **tarefa**&lt;T&gt;**. ContinueWith** método:

[!code-csharp[Main](http-message-handlers/samples/sample9.cs)]

## <a name="example-checking-for-an-api-key"></a>Exemplo: Verificação de uma chave de API

Alguns serviços da web exigem que os clientes incluir uma chave de API na sua solicitação. O exemplo a seguir mostra como um manipulador de mensagens pode verificar as solicitações para uma chave de API válida:

[!code-csharp[Main](http-message-handlers/samples/sample10.cs)]

Esse manipulador procura a chave de API na cadeia de caracteres de consulta do URI. (Neste exemplo, vamos supor que a chave é uma cadeia de caracteres estática. Uma implementação real provavelmente usaria uma validação mais complexa.) Se a cadeia de caracteres de consulta contém a chave, o manipulador passa a solicitação para o manipulador interno.

Se a solicitação não tiver uma chave válida, o manipulador cria uma mensagem de resposta com status 403, proibido. Nesse caso, o manipulador não chama `base.SendAsync`, portanto, o manipulador interno nunca recebe a solicitação, nem o controlador. Portanto, o controlador pode assumir que todas as solicitações de entrada tem uma chave de API válida.

> [!NOTE]
> Se a chave de API se aplica apenas a determinadas ações do controlador, considere usar um filtro de ação em vez de um manipulador de mensagens. Filtros de ação executado após o roteamento do URI é executado.


## <a name="per-route-message-handlers"></a>Manipuladores de mensagens por rota

Manipuladores de **HttpConfiguration.MessageHandlers** coleção se aplicam globalmente.

Como alternativa, você pode adicionar um manipulador de mensagens para uma rota específica quando você define a rota:

[!code-csharp[Main](http-message-handlers/samples/sample11.cs?highlight=16)]

Neste exemplo, se o URI da solicitação corresponde a "Route2", a solicitação será enviada para `MessageHandler2`. O diagrama a seguir mostra o pipeline para essas duas rotas:

![](http-message-handlers/_static/image4.png)

Observe que `MessageHandler2` substitui o padrão **HttpControllerDispatcher**. Neste exemplo, `MessageHandler2` cria a resposta e as solicitações que correspondem a "Route2" nunca ir para um controlador. Isso lhe permite substituir o mecanismo de controlador de API da Web inteiro com seu próprio ponto de extremidade personalizado.

Como alternativa, um manipulador de mensagens por rota pode delegar a **HttpControllerDispatcher**, que então expede para um controlador.

![](http-message-handlers/_static/image5.png)

O código a seguir mostra como configurar essa rota:

[!code-csharp[Main](http-message-handlers/samples/sample12.cs)]

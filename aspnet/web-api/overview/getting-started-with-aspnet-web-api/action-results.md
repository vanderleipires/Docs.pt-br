---
uid: web-api/overview/getting-started-with-aspnet-web-api/action-results
title: Ação resultará na API Web 2 | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 02/03/2014
ms.assetid: 2fc4797c-38ef-4cc7-926c-ca431c4739e8
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/action-results
msc.type: authoredcontent
ms.openlocfilehash: b2b5ae5e5cef19e75a184aa28ac838a31e5ef1fd
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825017"
---
<a name="action-results-in-web-api-2"></a>Resultados de ação na API Web 2
====================
por [Mike Wasson](https://github.com/MikeWasson)

Este tópico descreve como a API Web ASP.NET converte o valor de retorno de uma ação do controlador em uma mensagem de resposta HTTP.

Uma ação do controlador API Web pode retornar qualquer um dos seguintes:

1. void
2. **HttpResponseMessage**
3. **IHttpActionResult**
4. Algum outro tipo

Dependendo de qual deles é retornada, API Web usa um mecanismo diferente para criar a resposta HTTP.

| Tipo de retorno | Como a API da Web cria a resposta |
| --- | --- |
| void | Retorna vazio 204 (sem conteúdo) |
| **HttpResponseMessage** | Converta diretamente em uma mensagem de resposta HTTP. |
| **IHttpActionResult** | Chame **ExecuteAsync** para criar um **HttpResponseMessage**, em seguida, converter em uma mensagem de resposta HTTP. |
| Outro tipo | Gravar o valor de retorno serializado no corpo da resposta; retorne 200 (Okey). |

O restante deste tópico descreve cada opção em mais detalhes.

## <a name="void"></a>void

Se o tipo de retorno é `void`, API da Web simplesmente retorna uma resposta HTTP vazia com o código de status 204 (sem conteúdo).

Controlador de exemplo:

[!code-csharp[Main](action-results/samples/sample1.cs)]

Resposta HTTP:

[!code-console[Main](action-results/samples/sample2.cmd)]

## <a name="httpresponsemessage"></a>HttpResponseMessage

Se a ação retorna um [HttpResponseMessage](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.aspx), API Web converte o valor de retorno diretamente em uma mensagem de resposta HTTP, usando as propriedades da **HttpResponseMessage** objeto para popular o resposta.

Essa opção fornece muito controle sobre a mensagem de resposta. Por exemplo, a ação de controlador a seguir define o cabeçalho Cache-Control.

[!code-csharp[Main](action-results/samples/sample3.cs)]

Resposta:

[!code-console[Main](action-results/samples/sample4.cmd?highlight=2)]

Se você passar um modelo de domínio para o **CreateResponse** método, a API da Web usa um [formatador de mídia](../formats-and-model-binding/media-formatters.md) para gravar o modelo serializado no corpo da resposta.

[!code-csharp[Main](action-results/samples/sample5.cs)]

API da Web usa o cabeçalho Accept na solicitação para escolher o formatador. Para obter mais informações, consulte [negociação de conteúdo](../formats-and-model-binding/content-negotiation.md).

## <a name="ihttpactionresult"></a>IHttpActionResult

O **IHttpActionResult** interface foi introduzida na API Web 2. Essencialmente, ele define uma **HttpResponseMessage** factory. Aqui estão algumas vantagens de usar o **IHttpActionResult** interface:

- Simplifica [testes de unidade](../testing-and-debugging/unit-testing-controllers-in-web-api.md) seus controladores.
- Move a lógica comum para a criação de respostas HTTP em classes separadas.
- Torna a intenção da ação de controlador mais clara, ocultando os detalhes de nível baixo de construir a resposta.

**IHttpActionResult** contém um único método, **ExecuteAsync**, que cria de maneira assíncrona uma **HttpResponseMessage** instância.

[!code-csharp[Main](action-results/samples/sample6.cs)]

Se uma ação do controlador retorna um **IHttpActionResult**, chamadas de API da Web a **ExecuteAsync** método para criar um **HttpResponseMessage**. Em seguida, ele converte o **HttpResponseMessage** em uma mensagem de resposta HTTP.

Aqui está uma implementação simple da **IHttpActionResult** que cria uma resposta de texto sem formatação:

[!code-csharp[Main](action-results/samples/sample7.cs)]

Ação do controlador de exemplo:

[!code-csharp[Main](action-results/samples/sample8.cs)]

Resposta:

[!code-console[Main](action-results/samples/sample9.cmd)]

Mais frequentemente, você usará o **IHttpActionResult** implementações definidas na **[Results](https://msdn.microsoft.com/library/system.web.http.results.aspx)** namespace. O **ApiController** classe define os métodos auxiliares que retornam esses resultados de ação interna.

No exemplo a seguir, se a solicitação não corresponder a uma ID de produto existente, o controlador chama [ApiController.NotFound](https://msdn.microsoft.com/library/system.web.http.apicontroller.notfound.aspx) para criar uma resposta 404 (não encontrado). Caso contrário, o controlador chama [ApiController.OK](https://msdn.microsoft.com/library/dn314591.aspx), que cria uma resposta de 200 (Okey) que contém o produto.

[!code-csharp[Main](action-results/samples/sample10.cs)]

## <a name="other-return-types"></a>Outros tipos de retorno

Para todos os outros tipos de retorno, a API Web usa um [formatador de mídia](../formats-and-model-binding/media-formatters.md) para serializar o valor de retorno. API da Web grava o valor serializado no corpo da resposta. O código de status de resposta é 200 (Okey).

[!code-csharp[Main](action-results/samples/sample11.cs)]

Uma desvantagem dessa abordagem é que você não pode retornar diretamente um código de erro, como 404. No entanto, você pode lançar uma **HttpResponseException** para códigos de erro. Para obter mais informações, consulte [tratamento de exceções em API Web ASP.NET](../error-handling/exception-handling.md).

API da Web usa o cabeçalho Accept na solicitação para escolher o formatador. Para obter mais informações, consulte [negociação de conteúdo](../formats-and-model-binding/content-negotiation.md).

Exemplo de solicitação

[!code-console[Main](action-results/samples/sample12.cmd)]

Resposta de exemplo:

[!code-console[Main](action-results/samples/sample13.cmd)]

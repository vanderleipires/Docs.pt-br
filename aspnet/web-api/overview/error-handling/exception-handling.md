---
uid: web-api/overview/error-handling/exception-handling
title: Tratamento de exceções em API Web ASP.NET | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 03/12/2012
ms.assetid: cbebeb37-2594-41f2-b71a-f4f26520d512
msc.legacyurl: /web-api/overview/error-handling/exception-handling
msc.type: authoredcontent
ms.openlocfilehash: 2db00c1ab241e0dad051c2a3bcd3b0e4bbf4d5c4
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37807655"
---
<a name="exception-handling-in-aspnet-web-api"></a>Tratamento de exceções em API Web ASP.NET
====================
por [Mike Wasson](https://github.com/MikeWasson)

Este artigo descreve o erro e tratamento de exceções em API Web do ASP.NET.

- [HttpResponseException](#httpresponserexception)
- [Filtros de exceção](#exception_filters)
- [Registrando os filtros de exceção](#registering_exception_filters)
- [HttpError](#httperror)

<a id="httpresponserexception"></a>
## <a name="httpresponseexception"></a>HttpResponseException

O que acontece se um controlador de API da Web lança uma exceção não percebida? Por padrão, a maioria das exceções são convertidas em uma resposta HTTP com código de status 500, erro interno do servidor.

O **HttpResponseException** tipo é um caso especial. Essa exceção retorna qualquer código de status HTTP que você especificar no construtor de exceção. Por exemplo, o método a seguir retorna 404, não encontrado, se o *id* parâmetro não é válido.

[!code-csharp[Main](exception-handling/samples/sample1.cs)]

Para obter mais controle sobre a resposta, você também pode construir a mensagem de resposta inteira e incluí-lo com o **HttpResponseException:** 

[!code-csharp[Main](exception-handling/samples/sample2.cs)]

<a id="exception_filters"></a>
## <a name="exception-filters"></a>Filtros de exceção

Você pode personalizar como a API da Web lida com exceções, escrevendo uma *filtro de exceção*. Um filtro de exceção é executado quando um método de controlador lança uma exceção sem tratamento que é *não* um **HttpResponseException** exceção. O **HttpResponseException** tipo é um caso especial, porque ele foi desenvolvido especificamente para retornar uma resposta HTTP.

Filtros de exceção implementam o **System.Web.Http.Filters.IExceptionFilter** interface. A maneira mais simples de escrever um filtro de exceção é derivar de **System.Web.Http.Filters.ExceptionFilterAttribute** de classe e substituir o **OnException** método.

> [!NOTE]
> Filtros de exceção na API Web ASP.NET são semelhantes no ASP.NET MVC. No entanto, eles são declarados em um namespace separado e função separadamente. Em particular, o **HandleErrorAttribute** classe usada no MVC não manipular exceções lançadas por controladores da API Web.


Aqui está um filtro que converte **NotImplementedException** exceções ao status HTTP 501, não foi implementado de código:

[!code-csharp[Main](exception-handling/samples/sample3.cs)]

O **resposta** propriedade da **HttpActionExecutedContext** objeto contém a mensagem de resposta HTTP que será enviada ao cliente.

<a id="registering_exception_filters"></a>
## <a name="registering-exception-filters"></a>Registrando os filtros de exceção

Há várias maneiras de registrar um filtro de exceção de API da Web:

- Por ação
- Pelo controlador
- Globalmente

Para aplicar o filtro a uma ação específica, adicione o filtro como um atributo para a ação:

[!code-csharp[Main](exception-handling/samples/sample4.cs)]

Para aplicar o filtro para todas as ações em um controlador, adicione o filtro como um atributo à classe do controlador:

[!code-csharp[Main](exception-handling/samples/sample5.cs)]

Para aplicar o filtro globalmente para todos os controladores de API da Web, adicione uma instância do filtro para o **GlobalConfiguration.Configuration.Filters** coleção. Filtros da exceção nesta coleção se aplicam a qualquer ação do controlador de API da Web.

[!code-csharp[Main](exception-handling/samples/sample6.cs)]

Se você usar o modelo de projeto "Aplicativo de Web do ASP.NET MVC 4" para criar seu projeto, coloque o código de configuração de API da Web dentro do `WebApiConfig` classe, que está localizado no aplicativo\_pasta inicial:

[!code-csharp[Main](exception-handling/samples/sample7.cs?highlight=5)]

<a id="httperror"></a>
## <a name="httperror"></a>HttpError

O **HttpError** objeto fornece uma maneira consistente para retornar informações de erro no corpo da resposta. O exemplo a seguir mostra como retornar o código de status HTTP 404 (não encontrado) com um **HttpError** no corpo da resposta.

[!code-csharp[Main](exception-handling/samples/sample8.cs)]

**CreateErrorResponse** é um método de extensão definido em de **System.Net.Http.HttpRequestMessageExtensions** classe. Internamente, **CreateErrorResponse** cria um **HttpError** da instância e, em seguida, cria um **HttpResponseMessage** que contém o **HttpError**.

Neste exemplo, se o método for bem-sucedido, ele retorna o produto na resposta HTTP. Mas se o produto solicitado não for encontrado, a resposta HTTP contém uma **HttpError** no corpo da solicitação. A resposta pode parecer com o seguinte:

[!code-console[Main](exception-handling/samples/sample9.cmd)]

Observe que o **HttpError** foi serializado como JSON neste exemplo. Uma vantagem de usar **HttpError** é que ele percorre o mesmo [a negociação de conteúdo](../formats-and-model-binding/content-negotiation.md) e o processo de serialização como qualquer outro modelo fortemente tipado.

### <a name="httperror-and-model-validation"></a>Validação de modelo e HttpError

Validação do modelo, você pode passar o estado do modelo **CreateErrorResponse**, para incluir os erros de validação na resposta:

[!code-csharp[Main](exception-handling/samples/sample10.cs)]

Este exemplo pode retornar a resposta a seguir:

[!code-console[Main](exception-handling/samples/sample11.cmd)]

Para obter mais informações sobre a validação de modelo, consulte [validação do modelo na API Web ASP.NET](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).

### <a name="using-httperror-with-httpresponseexception"></a>Usando HttpError com HttpResponseException

Os exemplos anteriores retornam um **HttpResponseMessage** mensagem de ação de controlador, mas você também pode usar **HttpResponseException** para retornar uma **HttpError**. Isso permite que você retornar um modelo fortemente tipado no caso de sucesso normal, enquanto ainda está retornando **HttpError** se não houver um erro:

[!code-csharp[Main](exception-handling/samples/sample12.cs)]

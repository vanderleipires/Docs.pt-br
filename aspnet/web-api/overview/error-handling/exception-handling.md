---
uid: web-api/overview/error-handling/exception-handling
title: Tratamento de exceção no ASP.NET Web API | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/12/2012
ms.topic: article
ms.assetid: cbebeb37-2594-41f2-b71a-f4f26520d512
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/error-handling/exception-handling
msc.type: authoredcontent
ms.openlocfilehash: c65ddcca012840d70ab5a33af92edb30041be971
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
ms.locfileid: "26506955"
---
<a name="exception-handling-in-aspnet-web-api"></a>Tratamento de exceção no API da Web do ASP.NET
====================
por [Mike Wasson](https://github.com/MikeWasson)

Este artigo descreve o erro e tratamento de exceção no API Web do ASP.NET.

- [HttpResponseException](#httpresponserexception)
- [Filtros de exceção](#exception_filters)
- [Registro de filtros de exceção](#registering_exception_filters)
- [HttpError](#httperror)

<a id="httpresponserexception"></a>
## <a name="httpresponseexception"></a>HttpResponseException

O que acontece se um controlador Web API lança uma exceção não percebida? Por padrão, a maioria das exceções são convertidos em uma resposta HTTP com código de status 500, erro de servidor interno.

O **HttpResponseException** tipo é um caso especial. Essa exceção retorna qualquer código de status HTTP que você especificar no construtor de exceção. Por exemplo, o método a seguir retorna 404 não encontrado, se o *id* parâmetro não é válido.

[!code-csharp[Main](exception-handling/samples/sample1.cs)]

Para obter mais controle sobre a resposta, você também pode construir a mensagem de resposta inteira e incluí-lo com o **HttpResponseException:** 

[!code-csharp[Main](exception-handling/samples/sample2.cs)]

<a id="exception_filters"></a>
## <a name="exception-filters"></a>Filtros de exceção

Você pode personalizar como a API da Web manipula exceções ao escrever um *filtro de exceção*. Um filtro de exceção é executado quando um método de controlador gera qualquer exceção sem tratamento que é *não* um **HttpResponseException** exceção. O **HttpResponseException** tipo é um caso especial, porque ele é projetado especificamente para retornar uma resposta HTTP.

Filtros de exceção implementam o **System.Web.Http.Filters.IExceptionFilter** interface. A maneira mais simples para escrever um filtro de exceção é derivar o **System.Web.Http.Filters.ExceptionFilterAttribute** classe e substituir o **OnException** método.

> [!NOTE]
> Filtros de exceção na API da Web do ASP.NET são semelhantes do ASP.NET MVC. No entanto, eles são declarados em um namespace separado e função separadamente. Em particular, o **HandleErrorAttribute** classe usada no MVC não manipular exceções lançadas por controladores de API da Web.


Aqui está um filtro que converte **NotImplementedException** exceções no status de HTTP 501, não implementado de código:

[!code-csharp[Main](exception-handling/samples/sample3.cs)]

O **resposta** propriedade o **HttpActionExecutedContext** objeto contém a mensagem de resposta HTTP que será enviada ao cliente.

<a id="registering_exception_filters"></a>
## <a name="registering-exception-filters"></a>Registro de filtros de exceção

Há várias maneiras de registrar um filtro de exceção de API da Web:

- Por ação
- Por controlador
- Globalmente

Para aplicar o filtro a uma ação específica, adicione o filtro como um atributo para a ação:

[!code-csharp[Main](exception-handling/samples/sample4.cs)]

Para aplicar o filtro para todas as ações em um controlador, adicione o filtro como um atributo para a classe do controlador:

[!code-csharp[Main](exception-handling/samples/sample5.cs)]

Para aplicar o filtro globalmente para todos os controladores de API da Web, adicione uma instância do filtro para o **GlobalConfiguration.Configuration.Filters** coleção. Filtros de exceção nesta coleção se aplicam a qualquer ação de controlador de API da Web.

[!code-csharp[Main](exception-handling/samples/sample6.cs)]

Se você usar o modelo de projeto "Aplicativo de Web do ASP.NET MVC 4" para criar seu projeto, coloque o código de configuração de API da Web dentro do `WebApiConfig` classe, que está localizado no aplicativo\_pasta inicial:

[!code-csharp[Main](exception-handling/samples/sample7.cs?highlight=5)]

<a id="httperror"></a>
## <a name="httperror"></a>HttpError

O **HttpError** objeto fornece uma maneira consistente para retornar informações de erro no corpo da resposta. O exemplo a seguir mostra como retornar o código de status HTTP 404 (não encontrado) com um **HttpError** no corpo da resposta.

[!code-csharp[Main](exception-handling/samples/sample8.cs)]

**CreateErrorResponse** é um método de extensão definido no **System.Net.Http.HttpRequestMessageExtensions** classe. Internamente, **CreateErrorResponse** cria um **HttpError** instância e, em seguida, cria um **HttpResponseMessage** que contém o **HttpError**.

Neste exemplo, se o método for bem-sucedida, ela retorna o produto na resposta HTTP. Mas se o produto solicitado não for encontrado, a resposta HTTP contém uma **HttpError** no corpo da solicitação. A resposta pode parecer com o seguinte:

[!code-console[Main](exception-handling/samples/sample9.cmd)]

Observe que o **HttpError** foi serializado como JSON neste exemplo. Uma vantagem de usar **HttpError** é que atravessa o mesmo [negociação de conteúdo](../formats-and-model-binding/content-negotiation.md) e serialização processar como qualquer outro modelo fortemente tipado.

### <a name="httperror-and-model-validation"></a>HttpError e validação de modelo

Para validação de modelo, você pode passar o estado do modelo para **CreateErrorResponse**, para incluir os erros de validação na resposta:

[!code-csharp[Main](exception-handling/samples/sample10.cs)]

Este exemplo pode retornar a resposta a seguir:

[!code-console[Main](exception-handling/samples/sample11.cmd)]

Para obter mais informações sobre a validação de modelo, consulte [validação de modelo no ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).

### <a name="using-httperror-with-httpresponseexception"></a>Usando HttpError com HttpResponseException

Os exemplos anteriores retornam um **HttpResponseMessage** mensagem de ação do controlador, mas você também pode usar **HttpResponseException** para retornar um **HttpError**. Isso lhe permite retornar um modelo fortemente tipado no caso de sucesso normal, enquanto ainda retornando **HttpError** se houver um erro:

[!code-csharp[Main](exception-handling/samples/sample12.cs)]

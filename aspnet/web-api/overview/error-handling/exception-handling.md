---
uid: web-api/overview/error-handling/exception-handling
title: Tratamento de exceções em API Web ASP.NET | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/12/2012
ms.topic: article
ms.assetid: cbebeb37-2594-41f2-b71a-f4f26520d512
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/error-handling/exception-handling
msc.type: authoredcontent
ms.openlocfilehash: ac01a4f35cde99a1f8ec699e6d31bf597f1d334e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37400814"
---
<a name="exception-handling-in-aspnet-web-api"></a><span data-ttu-id="7eb1a-102">Tratamento de exceções em API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="7eb1a-102">Exception Handling in ASP.NET Web API</span></span>
====================
<span data-ttu-id="7eb1a-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="7eb1a-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="7eb1a-104">Este artigo descreve o erro e tratamento de exceções em API Web do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-104">This article describes error and exception handling in ASP.NET Web API.</span></span>

- [<span data-ttu-id="7eb1a-105">HttpResponseException</span><span class="sxs-lookup"><span data-stu-id="7eb1a-105">HttpResponseException</span></span>](#httpresponserexception)
- [<span data-ttu-id="7eb1a-106">Filtros de exceção</span><span class="sxs-lookup"><span data-stu-id="7eb1a-106">Exception Filters</span></span>](#exception_filters)
- [<span data-ttu-id="7eb1a-107">Registrando os filtros de exceção</span><span class="sxs-lookup"><span data-stu-id="7eb1a-107">Registering Exception Filters</span></span>](#registering_exception_filters)
- [<span data-ttu-id="7eb1a-108">HttpError</span><span class="sxs-lookup"><span data-stu-id="7eb1a-108">HttpError</span></span>](#httperror)

<a id="httpresponserexception"></a>
## <a name="httpresponseexception"></a><span data-ttu-id="7eb1a-109">HttpResponseException</span><span class="sxs-lookup"><span data-stu-id="7eb1a-109">HttpResponseException</span></span>

<span data-ttu-id="7eb1a-110">O que acontece se um controlador de API da Web lança uma exceção não percebida?</span><span class="sxs-lookup"><span data-stu-id="7eb1a-110">What happens if a Web API controller throws an uncaught exception?</span></span> <span data-ttu-id="7eb1a-111">Por padrão, a maioria das exceções são convertidas em uma resposta HTTP com código de status 500, erro interno do servidor.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-111">By default, most exceptions are translated into an HTTP response with status code 500, Internal Server Error.</span></span>

<span data-ttu-id="7eb1a-112">O **HttpResponseException** tipo é um caso especial.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-112">The **HttpResponseException** type is a special case.</span></span> <span data-ttu-id="7eb1a-113">Essa exceção retorna qualquer código de status HTTP que você especificar no construtor de exceção.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-113">This exception returns any HTTP status code that you specify in the exception constructor.</span></span> <span data-ttu-id="7eb1a-114">Por exemplo, o método a seguir retorna 404, não encontrado, se o *id* parâmetro não é válido.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-114">For example, the following method returns 404, Not Found, if the *id* parameter is not valid.</span></span>

[!code-csharp[Main](exception-handling/samples/sample1.cs)]

<span data-ttu-id="7eb1a-115">Para obter mais controle sobre a resposta, você também pode construir a mensagem de resposta inteira e incluí-lo com o **HttpResponseException:**</span><span class="sxs-lookup"><span data-stu-id="7eb1a-115">For more control over the response, you can also construct the entire response message and include it with the **HttpResponseException:**</span></span> 

[!code-csharp[Main](exception-handling/samples/sample2.cs)]

<a id="exception_filters"></a>
## <a name="exception-filters"></a><span data-ttu-id="7eb1a-116">Filtros de exceção</span><span class="sxs-lookup"><span data-stu-id="7eb1a-116">Exception Filters</span></span>

<span data-ttu-id="7eb1a-117">Você pode personalizar como a API da Web lida com exceções, escrevendo uma *filtro de exceção*.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-117">You can customize how Web API handles exceptions by writing an *exception filter*.</span></span> <span data-ttu-id="7eb1a-118">Um filtro de exceção é executado quando um método de controlador lança uma exceção sem tratamento que é *não* um **HttpResponseException** exceção.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-118">An exception filter is executed when a controller method throws any unhandled exception that is *not* an **HttpResponseException** exception.</span></span> <span data-ttu-id="7eb1a-119">O **HttpResponseException** tipo é um caso especial, porque ele foi desenvolvido especificamente para retornar uma resposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-119">The **HttpResponseException** type is a special case, because it is designed specifically for returning an HTTP response.</span></span>

<span data-ttu-id="7eb1a-120">Filtros de exceção implementam o **System.Web.Http.Filters.IExceptionFilter** interface.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-120">Exception filters implement the **System.Web.Http.Filters.IExceptionFilter** interface.</span></span> <span data-ttu-id="7eb1a-121">A maneira mais simples de escrever um filtro de exceção é derivar de **System.Web.Http.Filters.ExceptionFilterAttribute** de classe e substituir o **OnException** método.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-121">The simplest way to write an exception filter is to derive from the **System.Web.Http.Filters.ExceptionFilterAttribute** class and override the **OnException** method.</span></span>

> [!NOTE]
> <span data-ttu-id="7eb1a-122">Filtros de exceção na API Web ASP.NET são semelhantes no ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-122">Exception filters in ASP.NET Web API are similar to those in ASP.NET MVC.</span></span> <span data-ttu-id="7eb1a-123">No entanto, eles são declarados em um namespace separado e função separadamente.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-123">However, they are declared in a separate namespace and function separately.</span></span> <span data-ttu-id="7eb1a-124">Em particular, o **HandleErrorAttribute** classe usada no MVC não manipular exceções lançadas por controladores da API Web.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-124">In particular, the **HandleErrorAttribute** class used in MVC does not handle exceptions thrown by Web API controllers.</span></span>


<span data-ttu-id="7eb1a-125">Aqui está um filtro que converte **NotImplementedException** exceções ao status HTTP 501, não foi implementado de código:</span><span class="sxs-lookup"><span data-stu-id="7eb1a-125">Here is a filter that converts **NotImplementedException** exceptions into HTTP status code 501, Not Implemented:</span></span>

[!code-csharp[Main](exception-handling/samples/sample3.cs)]

<span data-ttu-id="7eb1a-126">O **resposta** propriedade da **HttpActionExecutedContext** objeto contém a mensagem de resposta HTTP que será enviada ao cliente.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-126">The **Response** property of the **HttpActionExecutedContext** object contains the HTTP response message that will be sent to the client.</span></span>

<a id="registering_exception_filters"></a>
## <a name="registering-exception-filters"></a><span data-ttu-id="7eb1a-127">Registrando os filtros de exceção</span><span class="sxs-lookup"><span data-stu-id="7eb1a-127">Registering Exception Filters</span></span>

<span data-ttu-id="7eb1a-128">Há várias maneiras de registrar um filtro de exceção de API da Web:</span><span class="sxs-lookup"><span data-stu-id="7eb1a-128">There are several ways to register a Web API exception filter:</span></span>

- <span data-ttu-id="7eb1a-129">Por ação</span><span class="sxs-lookup"><span data-stu-id="7eb1a-129">By action</span></span>
- <span data-ttu-id="7eb1a-130">Pelo controlador</span><span class="sxs-lookup"><span data-stu-id="7eb1a-130">By controller</span></span>
- <span data-ttu-id="7eb1a-131">Globalmente</span><span class="sxs-lookup"><span data-stu-id="7eb1a-131">Globally</span></span>

<span data-ttu-id="7eb1a-132">Para aplicar o filtro a uma ação específica, adicione o filtro como um atributo para a ação:</span><span class="sxs-lookup"><span data-stu-id="7eb1a-132">To apply the filter to a specific action, add the filter as an attribute to the action:</span></span>

[!code-csharp[Main](exception-handling/samples/sample4.cs)]

<span data-ttu-id="7eb1a-133">Para aplicar o filtro para todas as ações em um controlador, adicione o filtro como um atributo à classe do controlador:</span><span class="sxs-lookup"><span data-stu-id="7eb1a-133">To apply the filter to all of the actions on a controller, add the filter as an attribute to the controller class:</span></span>

[!code-csharp[Main](exception-handling/samples/sample5.cs)]

<span data-ttu-id="7eb1a-134">Para aplicar o filtro globalmente para todos os controladores de API da Web, adicione uma instância do filtro para o **GlobalConfiguration.Configuration.Filters** coleção.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-134">To apply the filter globally to all Web API controllers, add an instance of the filter to the **GlobalConfiguration.Configuration.Filters** collection.</span></span> <span data-ttu-id="7eb1a-135">Filtros da exceção nesta coleção se aplicam a qualquer ação do controlador de API da Web.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-135">Exeption filters in this collection apply to any Web API controller action.</span></span>

[!code-csharp[Main](exception-handling/samples/sample6.cs)]

<span data-ttu-id="7eb1a-136">Se você usar o modelo de projeto "Aplicativo de Web do ASP.NET MVC 4" para criar seu projeto, coloque o código de configuração de API da Web dentro do `WebApiConfig` classe, que está localizado no aplicativo\_pasta inicial:</span><span class="sxs-lookup"><span data-stu-id="7eb1a-136">If you use the "ASP.NET MVC 4 Web Application" project template to create your project, put your Web API configuration code inside the `WebApiConfig` class, which is located in the App\_Start folder:</span></span>

[!code-csharp[Main](exception-handling/samples/sample7.cs?highlight=5)]

<a id="httperror"></a>
## <a name="httperror"></a><span data-ttu-id="7eb1a-137">HttpError</span><span class="sxs-lookup"><span data-stu-id="7eb1a-137">HttpError</span></span>

<span data-ttu-id="7eb1a-138">O **HttpError** objeto fornece uma maneira consistente para retornar informações de erro no corpo da resposta.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-138">The **HttpError** object provides a consistent way to return error information in the response body.</span></span> <span data-ttu-id="7eb1a-139">O exemplo a seguir mostra como retornar o código de status HTTP 404 (não encontrado) com um **HttpError** no corpo da resposta.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-139">The following example shows how to return HTTP status code 404 (Not Found) with an **HttpError** in the response body.</span></span>

[!code-csharp[Main](exception-handling/samples/sample8.cs)]

<span data-ttu-id="7eb1a-140">**CreateErrorResponse** é um método de extensão definido em de **System.Net.Http.HttpRequestMessageExtensions** classe.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-140">**CreateErrorResponse** is an extension method defined in the **System.Net.Http.HttpRequestMessageExtensions** class.</span></span> <span data-ttu-id="7eb1a-141">Internamente, **CreateErrorResponse** cria um **HttpError** da instância e, em seguida, cria um **HttpResponseMessage** que contém o **HttpError**.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-141">Internally, **CreateErrorResponse** creates an **HttpError** instance and then creates an **HttpResponseMessage** that contains the **HttpError**.</span></span>

<span data-ttu-id="7eb1a-142">Neste exemplo, se o método for bem-sucedido, ele retorna o produto na resposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-142">In this example, if the method is successful, it returns the product in the HTTP response.</span></span> <span data-ttu-id="7eb1a-143">Mas se o produto solicitado não for encontrado, a resposta HTTP contém uma **HttpError** no corpo da solicitação.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-143">But if the requested product is not found, the HTTP response contains an **HttpError** in the request body.</span></span> <span data-ttu-id="7eb1a-144">A resposta pode parecer com o seguinte:</span><span class="sxs-lookup"><span data-stu-id="7eb1a-144">The response might look like the following:</span></span>

[!code-console[Main](exception-handling/samples/sample9.cmd)]

<span data-ttu-id="7eb1a-145">Observe que o **HttpError** foi serializado como JSON neste exemplo.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-145">Notice that the **HttpError** was serialized to JSON in this example.</span></span> <span data-ttu-id="7eb1a-146">Uma vantagem de usar **HttpError** é que ele percorre o mesmo [a negociação de conteúdo](../formats-and-model-binding/content-negotiation.md) e o processo de serialização como qualquer outro modelo fortemente tipado.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-146">One advantage of using **HttpError** is that it goes through the same [content-negotiation](../formats-and-model-binding/content-negotiation.md) and serialization process as any other strongly-typed model.</span></span>

### <a name="httperror-and-model-validation"></a><span data-ttu-id="7eb1a-147">Validação de modelo e HttpError</span><span class="sxs-lookup"><span data-stu-id="7eb1a-147">HttpError and Model Validation</span></span>

<span data-ttu-id="7eb1a-148">Validação do modelo, você pode passar o estado do modelo **CreateErrorResponse**, para incluir os erros de validação na resposta:</span><span class="sxs-lookup"><span data-stu-id="7eb1a-148">For model validation, you can pass the model state to **CreateErrorResponse**, to include the validation errors in the response:</span></span>

[!code-csharp[Main](exception-handling/samples/sample10.cs)]

<span data-ttu-id="7eb1a-149">Este exemplo pode retornar a resposta a seguir:</span><span class="sxs-lookup"><span data-stu-id="7eb1a-149">This example might return the following response:</span></span>

[!code-console[Main](exception-handling/samples/sample11.cmd)]

<span data-ttu-id="7eb1a-150">Para obter mais informações sobre a validação de modelo, consulte [validação do modelo na API Web ASP.NET](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="7eb1a-150">For more information about model validation, see [Model Validation in ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span></span>

### <a name="using-httperror-with-httpresponseexception"></a><span data-ttu-id="7eb1a-151">Usando HttpError com HttpResponseException</span><span class="sxs-lookup"><span data-stu-id="7eb1a-151">Using HttpError with HttpResponseException</span></span>

<span data-ttu-id="7eb1a-152">Os exemplos anteriores retornam um **HttpResponseMessage** mensagem de ação de controlador, mas você também pode usar **HttpResponseException** para retornar uma **HttpError**.</span><span class="sxs-lookup"><span data-stu-id="7eb1a-152">The previous examples return an **HttpResponseMessage** message from the controller action, but you can also use **HttpResponseException** to return an **HttpError**.</span></span> <span data-ttu-id="7eb1a-153">Isso permite que você retornar um modelo fortemente tipado no caso de sucesso normal, enquanto ainda está retornando **HttpError** se não houver um erro:</span><span class="sxs-lookup"><span data-stu-id="7eb1a-153">This lets you return a strongly-typed model in the normal success case, while still returning **HttpError** if there is an error:</span></span>

[!code-csharp[Main](exception-handling/samples/sample12.cs)]

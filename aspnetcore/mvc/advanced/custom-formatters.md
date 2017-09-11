---
title: "APIs da web personalizados formatadores no ASP.NET MVC de núcleo"
author: tdykstra
description: Saiba como criar e usar formatadores personalizados para APIs da web no ASP.NET Core.
keywords: Api, formatadores personalizados da web do ASP.NET Core
ms.author: tdykstra
manager: wpickett
ms.date: 02/08/2017
ms.topic: article
ms.assetid: 1fb6fdc2-e199-4469-9012-b909d1913422
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/models/custom-formatters
ms.openlocfilehash: 0285b40cfacb79745d3a6488401677130f55a95b
ms.sourcegitcommit: 6ece943781d8a56784bb6160f14da85210d3fcea
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/11/2017
---
# <a name="custom-formatters-in-aspnet-core-mvc-web-apis"></a><span data-ttu-id="aa0a7-104">APIs da web personalizados formatadores no ASP.NET MVC de núcleo</span><span class="sxs-lookup"><span data-stu-id="aa0a7-104">Custom formatters in ASP.NET Core MVC web APIs</span></span>

<span data-ttu-id="aa0a7-105">Por [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="aa0a7-105">By [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="aa0a7-106">Núcleo do ASP.NET MVC tem suporte interno para troca de dados em APIs da web usando os formatos de texto sem formatação, XML ou JSON.</span><span class="sxs-lookup"><span data-stu-id="aa0a7-106">ASP.NET Core MVC has built-in support for data exchange in web APIs by using JSON, XML, or plain text formats.</span></span> <span data-ttu-id="aa0a7-107">Este artigo mostra como adicionar suporte para formatos adicionais criando formatadores personalizados.</span><span class="sxs-lookup"><span data-stu-id="aa0a7-107">This article shows how to add support for additional formats by creating custom formatters.</span></span>

<span data-ttu-id="aa0a7-108">[Exibir ou baixar o exemplo do GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-formatters/sample).</span><span class="sxs-lookup"><span data-stu-id="aa0a7-108">[View or download sample from GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-formatters/sample).</span></span>

## <a name="when-to-use-custom-formatters"></a><span data-ttu-id="aa0a7-109">Quando usar formatadores personalizados</span><span class="sxs-lookup"><span data-stu-id="aa0a7-109">When to use custom formatters</span></span>

<span data-ttu-id="aa0a7-110">Use um formatador personalizado quando quiser o [negociação de conteúdo](xref:mvc/models/formatting) processo para dar suporte a um tipo de conteúdo que não é compatível com os formatadores internos (JSON, XML e texto sem formatação).</span><span class="sxs-lookup"><span data-stu-id="aa0a7-110">Use a custom formatter when you want the [content negotiation](xref:mvc/models/formatting) process to support a content type that isn't supported by the built-in formatters (JSON, XML, and plain text).</span></span>

<span data-ttu-id="aa0a7-111">Por exemplo, se alguns dos clientes para sua API da web puderem manipular o [Protobuf](https://github.com/google/protobuf) formato, você talvez queira usar Protobuf com esses clientes porque é mais eficiente.</span><span class="sxs-lookup"><span data-stu-id="aa0a7-111">For example, if some of the clients for your web API can handle the [Protobuf](https://github.com/google/protobuf) format, you might want to use Protobuf with those clients because it's more efficient.</span></span>  <span data-ttu-id="aa0a7-112">Ou talvez você queira sua API da web para enviar entre em contato com os nomes e endereços em [vCard](https://en.wikipedia.org/wiki/VCard) formato, um formato normalmente usado para a troca de dados de contato.</span><span class="sxs-lookup"><span data-stu-id="aa0a7-112">Or you might want your web API to send contact names and addresses in [vCard](https://en.wikipedia.org/wiki/VCard) format, a commonly used format for exchanging contact data.</span></span> <span data-ttu-id="aa0a7-113">O aplicativo de exemplo fornecido com este artigo implementa um formatador vCard simples.</span><span class="sxs-lookup"><span data-stu-id="aa0a7-113">The sample app provided with this article implements a simple vCard formatter.</span></span>

## <a name="overview-of-how-to-use-a-custom-formatter"></a><span data-ttu-id="aa0a7-114">Visão geral de como usar um formatador personalizado</span><span class="sxs-lookup"><span data-stu-id="aa0a7-114">Overview of how to use a custom formatter</span></span>

<span data-ttu-id="aa0a7-115">Aqui estão as etapas para criar e usar um formatador personalizado:</span><span class="sxs-lookup"><span data-stu-id="aa0a7-115">Here are the steps to create and use a custom formatter:</span></span>

* <span data-ttu-id="aa0a7-116">Crie uma classe de formatador de saída para serializar os dados a serem enviados ao cliente.</span><span class="sxs-lookup"><span data-stu-id="aa0a7-116">Create an output formatter class if you want to serialize data to send to the client.</span></span>
* <span data-ttu-id="aa0a7-117">Crie uma classe de formatador de entrada para desserializar dados recebidos do cliente.</span><span class="sxs-lookup"><span data-stu-id="aa0a7-117">Create an input formatter class if you want to deserialize data received from the client.</span></span> 
* <span data-ttu-id="aa0a7-118">Adicionar instâncias de formatadores a serem o `InputFormatters` e `OutputFormatters` coleções no [MvcOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.mvcoptions).</span><span class="sxs-lookup"><span data-stu-id="aa0a7-118">Add instances of your formatters to the `InputFormatters` and `OutputFormatters` collections in [MvcOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.mvcoptions).</span></span>

<span data-ttu-id="aa0a7-119">As seções a seguir fornecem diretrizes e exemplos de código para cada uma dessas etapas.</span><span class="sxs-lookup"><span data-stu-id="aa0a7-119">The following sections provide guidance and code examples for each of these steps.</span></span>

## <a name="how-to-create-a-custom-formatter-class"></a><span data-ttu-id="aa0a7-120">Como criar uma classe de formatador personalizado</span><span class="sxs-lookup"><span data-stu-id="aa0a7-120">How to create a custom formatter class</span></span>

<span data-ttu-id="aa0a7-121">Para criar um formatador:</span><span class="sxs-lookup"><span data-stu-id="aa0a7-121">To create a formatter:</span></span>

* <span data-ttu-id="aa0a7-122">A classe derivam da classe base apropriada.</span><span class="sxs-lookup"><span data-stu-id="aa0a7-122">Derive the class from the appropriate base class.</span></span>
* <span data-ttu-id="aa0a7-123">Especifica as codificações e tipos de mídia válido no construtor.</span><span class="sxs-lookup"><span data-stu-id="aa0a7-123">Specify valid media types and encodings in the constructor.</span></span>
* <span data-ttu-id="aa0a7-124">Substituir `CanReadType` / `CanWriteType` métodos</span><span class="sxs-lookup"><span data-stu-id="aa0a7-124">Override `CanReadType`/`CanWriteType` methods</span></span>
* <span data-ttu-id="aa0a7-125">Substituir `ReadRequestBodyAsync` / `WriteResponseBodyAsync` métodos</span><span class="sxs-lookup"><span data-stu-id="aa0a7-125">Override `ReadRequestBodyAsync`/`WriteResponseBodyAsync` methods</span></span>
  
### <a name="derive-from-the-appropriate-base-class"></a><span data-ttu-id="aa0a7-126">Derivar da classe base apropriada</span><span class="sxs-lookup"><span data-stu-id="aa0a7-126">Derive from the appropriate base class</span></span>

<span data-ttu-id="aa0a7-127">Para tipos de mídia de texto (por exemplo, vCard), derivam de [TextInputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.textinputformatter) ou [TextOutputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.textoutputformatter) classe base.</span><span class="sxs-lookup"><span data-stu-id="aa0a7-127">For text media types (for example, vCard), derive from the [TextInputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.textinputformatter) or [TextOutputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.textoutputformatter) base class.</span></span>

<span data-ttu-id="aa0a7-128">[!code-csharp[Main](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=classdef)]</span><span class="sxs-lookup"><span data-stu-id="aa0a7-128">[!code-csharp[Main](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=classdef)]</span></span>

<span data-ttu-id="aa0a7-129">Para tipos binários, derivam de [InputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.inputformatter) ou [OutputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.outputformatter) classe base.</span><span class="sxs-lookup"><span data-stu-id="aa0a7-129">For binary types, derive from the [InputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.inputformatter) or [OutputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.outputformatter) base class.</span></span>

### <a name="specify-valid-media-types-and-encodings"></a><span data-ttu-id="aa0a7-130">Especificar as codificações e tipos de mídia válido</span><span class="sxs-lookup"><span data-stu-id="aa0a7-130">Specify valid media types and encodings</span></span>

<span data-ttu-id="aa0a7-131">No construtor, especificar codificações e tipos de mídia válido adicionando-se para o `SupportedMediaTypes` e `SupportedEncodings` coleções.</span><span class="sxs-lookup"><span data-stu-id="aa0a7-131">In the constructor, specify valid media types and encodings by adding to the `SupportedMediaTypes` and `SupportedEncodings` collections.</span></span>

<span data-ttu-id="aa0a7-132">[!code-csharp[Main](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=ctor&highlight=3,5-6)]</span><span class="sxs-lookup"><span data-stu-id="aa0a7-132">[!code-csharp[Main](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=ctor&highlight=3,5-6)]</span></span>

> [!NOTE]  
> <span data-ttu-id="aa0a7-133">Você não pode fazer a injeção de dependência de construtor em uma classe de formatador.</span><span class="sxs-lookup"><span data-stu-id="aa0a7-133">You can't do constructor dependency injection in a formatter class.</span></span> <span data-ttu-id="aa0a7-134">Por exemplo, você não pode obter um agente de log, adicionando um parâmetro de agente de log para o construtor.</span><span class="sxs-lookup"><span data-stu-id="aa0a7-134">For example, you can't get a logger by adding a logger parameter to the constructor.</span></span> <span data-ttu-id="aa0a7-135">Para acessar serviços, você precisa usar o objeto de contexto que é passado para seus métodos.</span><span class="sxs-lookup"><span data-stu-id="aa0a7-135">To access services, you have to use the context object that gets passed in to your methods.</span></span> <span data-ttu-id="aa0a7-136">Um exemplo de código [abaixo](#read-write) mostra como fazer isso.</span><span class="sxs-lookup"><span data-stu-id="aa0a7-136">A code example [below](#read-write) shows how to do this.</span></span>

### <a name="override-canreadtypecanwritetype"></a><span data-ttu-id="aa0a7-137">Substituir CanReadType/CanWriteType</span><span class="sxs-lookup"><span data-stu-id="aa0a7-137">Override CanReadType/CanWriteType</span></span> 

<span data-ttu-id="aa0a7-138">Especifique o tipo é possível desserializar em ou serializar de substituindo o `CanReadType` ou `CanWriteType` métodos.</span><span class="sxs-lookup"><span data-stu-id="aa0a7-138">Specify the type you can deserialize into or serialize from by overriding the `CanReadType` or `CanWriteType` methods.</span></span> <span data-ttu-id="aa0a7-139">Por exemplo, você só poderá criar texto vCard de um `Contact` tipo e vice-versa.</span><span class="sxs-lookup"><span data-stu-id="aa0a7-139">For example, you might only be able to create vCard text from a `Contact` type and vice versa.</span></span>

<span data-ttu-id="aa0a7-140">[!code-csharp[Main](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=canwritetype)]</span><span class="sxs-lookup"><span data-stu-id="aa0a7-140">[!code-csharp[Main](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=canwritetype)]</span></span>

#### <a name="the-canwriteresult-method"></a><span data-ttu-id="aa0a7-141">O método CanWriteResult</span><span class="sxs-lookup"><span data-stu-id="aa0a7-141">The CanWriteResult method</span></span>

<span data-ttu-id="aa0a7-142">Em alguns cenários, você tem que substituir `CanWriteResult` em vez de `CanWriteType`.</span><span class="sxs-lookup"><span data-stu-id="aa0a7-142">In some scenarios you have to override `CanWriteResult` instead of `CanWriteType`.</span></span> <span data-ttu-id="aa0a7-143">Use `CanWriteResult` se as seguintes condições forem verdadeiras:</span><span class="sxs-lookup"><span data-stu-id="aa0a7-143">Use `CanWriteResult` if the following conditions are true:</span></span>

  * <span data-ttu-id="aa0a7-144">O método de ação retorna uma classe de modelo.</span><span class="sxs-lookup"><span data-stu-id="aa0a7-144">Your action method returns a model class.</span></span>
  * <span data-ttu-id="aa0a7-145">Existem classes derivadas que podem ser retornadas em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="aa0a7-145">There are derived classes which might be returned at runtime.</span></span>
  * <span data-ttu-id="aa0a7-146">Você precisa saber em tempo de execução que derivado classe foi retornada pela ação.</span><span class="sxs-lookup"><span data-stu-id="aa0a7-146">You need to know at runtime which derived class was returned by the action.</span></span>  

<span data-ttu-id="aa0a7-147">Por exemplo, suponha que sua assinatura do método de ação retorna um `Person` tipo, mas ele pode retornar um `Student` ou `Instructor` tipo que deriva de `Person`.</span><span class="sxs-lookup"><span data-stu-id="aa0a7-147">For example, suppose your action method signature returns a `Person` type, but it may return a `Student` or `Instructor` type that derives from `Person`.</span></span> <span data-ttu-id="aa0a7-148">Se você quiser que o formatador para lidar com apenas `Student` objetos, verifique o tipo de [objeto](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.outputformattercanwritecontext#Microsoft_AspNetCore_Mvc_Formatters_OutputFormatterCanWriteContext_Object) no objeto de contexto fornecido para o `CanWriteResult` método.</span><span class="sxs-lookup"><span data-stu-id="aa0a7-148">If you want your formatter to handle only `Student` objects, check the type of [Object](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.outputformattercanwritecontext#Microsoft_AspNetCore_Mvc_Formatters_OutputFormatterCanWriteContext_Object) in the context object provided to the `CanWriteResult` method.</span></span> <span data-ttu-id="aa0a7-149">Observe que não é necessário usar `CanWriteResult` quando o método de ação retorna `IActionResult`; nesse caso, o `CanWriteType` método recebe o tipo de tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="aa0a7-149">Note that it's not necessary to use `CanWriteResult` when the action method returns `IActionResult`; in that case, the `CanWriteType` method receives the runtime type.</span></span>

<a id="read-write"></a>
### <a name="override-readrequestbodyasyncwriteresponsebodyasync"></a><span data-ttu-id="aa0a7-150">Substituir ReadRequestBodyAsync/WriteResponseBodyAsync</span><span class="sxs-lookup"><span data-stu-id="aa0a7-150">Override ReadRequestBodyAsync/WriteResponseBodyAsync</span></span> 

<span data-ttu-id="aa0a7-151">Fazer o trabalho real de desserialização ou serialização em `ReadRequestBodyAsync` ou `WriteResponseBodyAsync`.</span><span class="sxs-lookup"><span data-stu-id="aa0a7-151">You do the actual work of deserializing or serializing in `ReadRequestBodyAsync` or `WriteResponseBodyAsync`.</span></span>  <span data-ttu-id="aa0a7-152">As linhas destacadas no exemplo a seguir mostram como obter os serviços do contêiner de injeção de dependência (você não é possível obtê-los do construtor parâmetros).</span><span class="sxs-lookup"><span data-stu-id="aa0a7-152">The highlighted lines in the following example show how to get services from the dependency injection container (you can't get them from constructor parameters).</span></span>

<span data-ttu-id="aa0a7-153">[!code-csharp[Main](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=writeresponse&highlight=3-4)]</span><span class="sxs-lookup"><span data-stu-id="aa0a7-153">[!code-csharp[Main](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=writeresponse&highlight=3-4)]</span></span>

## <a name="how-to-configure-mvc-to-use-a-custom-formatter"></a><span data-ttu-id="aa0a7-154">Como configurar o MVC para usar um formatador personalizado</span><span class="sxs-lookup"><span data-stu-id="aa0a7-154">How to configure MVC to use a custom formatter</span></span>
 
<span data-ttu-id="aa0a7-155">Para usar um formatador personalizado, adicione uma instância da classe de formatador para a `InputFormatters` ou `OutputFormatters` coleção.</span><span class="sxs-lookup"><span data-stu-id="aa0a7-155">To use a custom formatter, add an instance of the formatter class to the `InputFormatters` or `OutputFormatters` collection.</span></span>

<span data-ttu-id="aa0a7-156">[!code-csharp[Main](custom-formatters/sample/Startup.cs?name=mvcoptions&highlight=3-4)]</span><span class="sxs-lookup"><span data-stu-id="aa0a7-156">[!code-csharp[Main](custom-formatters/sample/Startup.cs?name=mvcoptions&highlight=3-4)]</span></span>

<span data-ttu-id="aa0a7-157">Formatadores são avaliadas na ordem em que você inseri-los.</span><span class="sxs-lookup"><span data-stu-id="aa0a7-157">Formatters are evaluated in the order you insert them.</span></span> <span data-ttu-id="aa0a7-158">O primeiro deles terá precedência.</span><span class="sxs-lookup"><span data-stu-id="aa0a7-158">The first one takes precedence.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="aa0a7-159">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="aa0a7-159">Next steps</span></span>

<span data-ttu-id="aa0a7-160">Consulte o [aplicativo de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-formatters/sample), que implementa vCard simples de entrada e saída formatadores.</span><span class="sxs-lookup"><span data-stu-id="aa0a7-160">See the [sample application](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-formatters/sample), which implements simple vCard input and output formatters.</span></span>  <span data-ttu-id="aa0a7-161">O aplicativo lê e grava vCard se parecer com o exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="aa0a7-161">The application reads and writes vCards that look like the following example:</span></span>

```
BEGIN:VCARD
VERSION:2.1
N:Davolio;Nancy
FN:Nancy Davolio
UID:20293482-9240-4d68-b475-325df4a83728
END:VCARD
```

<span data-ttu-id="aa0a7-162">Para ver vCard de saída, execute o aplicativo e envia uma solicitação Get com aceitar cabeçalho "texto/vcard" `http://localhost:63313/api/contacts/` (durante a execução do Visual Studio) ou `http://localhost:5000/api/contacts/` (quando executado na linha de comando).</span><span class="sxs-lookup"><span data-stu-id="aa0a7-162">To see vCard output, run the application and send a Get request with Accept header "text/vcard" to `http://localhost:63313/api/contacts/` (when running from Visual Studio) or `http://localhost:5000/api/contacts/` (when running from the command line).</span></span>

<span data-ttu-id="aa0a7-163">Para adicionar um vCard à coleção de contatos na memória, envie uma solicitação Post para a mesma URL, com cabeçalho Content-Type "texto/vcard" e vCard texto no corpo, formatado como o exemplo acima.</span><span class="sxs-lookup"><span data-stu-id="aa0a7-163">To add a vCard to the in-memory collection of contacts, send a Post request to the same URL, with Content-Type header "text/vcard" and with vCard text in the body, formatted like the example above.</span></span>

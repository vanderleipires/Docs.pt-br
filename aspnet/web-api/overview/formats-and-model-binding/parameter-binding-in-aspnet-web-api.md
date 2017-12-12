---
uid: web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
title: "Parâmetro de associação no ASP.NET Web API | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/11/2013
ms.topic: article
ms.assetid: e42c8388-04ed-4341-9fdb-41b1b4c06320
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: ad052570fb2f168da657cd1263d8342a59d4cab0
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="parameter-binding-in-aspnet-web-api"></a><span data-ttu-id="25c15-102">Parâmetro de associação no ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="25c15-102">Parameter Binding in ASP.NET Web API</span></span>
====================
<span data-ttu-id="25c15-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="25c15-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="25c15-104">Quando a API da Web chama um método em um controlador, ele deve definir valores para os parâmetros, um processo chamado *associação*.</span><span class="sxs-lookup"><span data-stu-id="25c15-104">When Web API calls a method on a controller, it must set values for the parameters, a process called *binding*.</span></span> <span data-ttu-id="25c15-105">Este artigo descreve como a API da Web associa parâmetros e como você pode personalizar o processo de ligação.</span><span class="sxs-lookup"><span data-stu-id="25c15-105">This article describes how Web API binds parameters, and how you can customize the binding process.</span></span>

<span data-ttu-id="25c15-106">Por padrão, a API da Web usa as regras a seguir para associar parâmetros:</span><span class="sxs-lookup"><span data-stu-id="25c15-106">By default, Web API uses the following rules to bind parameters:</span></span>

- <span data-ttu-id="25c15-107">Se o parâmetro for um tipo "simples", a API da Web tenta obter o valor do URI.</span><span class="sxs-lookup"><span data-stu-id="25c15-107">If the parameter is a "simple" type, Web API tries to get the value from the URI.</span></span> <span data-ttu-id="25c15-108">Os tipos simples incluem o .NET [tipos primitivos](https://msdn.microsoft.com/en-us/library/system.type.isprimitive.aspx) (**int**, **bool**, **duplo**, e assim por diante), além de **TimeSpan**, **DateTime**, **Guid**, **decimal**, e **cadeia de caracteres**, *mais* qualquer tipo com um conversor de tipo que possa ser convertido de uma cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="25c15-108">Simple types include the .NET [primitive types](https://msdn.microsoft.com/en-us/library/system.type.isprimitive.aspx) (**int**, **bool**, **double**, and so forth), plus **TimeSpan**, **DateTime**, **Guid**, **decimal**, and **string**, *plus* any type with a type converter that can convert from a string.</span></span> <span data-ttu-id="25c15-109">(Mais informações sobre conversores de tipo posteriormente.)</span><span class="sxs-lookup"><span data-stu-id="25c15-109">(More about type converters later.)</span></span>
- <span data-ttu-id="25c15-110">Para tipos complexos, API da Web tenta ler o valor do corpo da mensagem, usando um [formatador do tipo de mídia](media-formatters.md).</span><span class="sxs-lookup"><span data-stu-id="25c15-110">For complex types, Web API tries to read the value from the message body, using a [media-type formatter](media-formatters.md).</span></span>

<span data-ttu-id="25c15-111">Por exemplo, aqui está um método comum de controlador de API da Web:</span><span class="sxs-lookup"><span data-stu-id="25c15-111">For example, here is a typical Web API controller method:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="25c15-112">O *id* parâmetro é um &quot;simples&quot; tipo, portanto a API da Web tenta obter o valor de URI de solicitação.</span><span class="sxs-lookup"><span data-stu-id="25c15-112">The *id* parameter is a &quot;simple&quot; type, so Web API tries to get the value from the request URI.</span></span> <span data-ttu-id="25c15-113">O *item* parâmetro é um tipo complexo, portanto, API da Web usa um formatador de tipo de mídia para ler o valor do corpo da solicitação.</span><span class="sxs-lookup"><span data-stu-id="25c15-113">The *item* parameter is a complex type, so Web API uses a media-type formatter to read the value from the request body.</span></span>

<span data-ttu-id="25c15-114">Para obter um valor do URI, API da Web examina os dados da rota e a cadeia de caracteres de consulta URI.</span><span class="sxs-lookup"><span data-stu-id="25c15-114">To get a value from the URI, Web API looks in the route data and the URI query string.</span></span> <span data-ttu-id="25c15-115">Os dados da rota são preenchidos quando o sistema de roteamento analisa o URI e corresponde a uma rota.</span><span class="sxs-lookup"><span data-stu-id="25c15-115">The route data is populated when the routing system parses the URI and matches it to a route.</span></span> <span data-ttu-id="25c15-116">Para obter mais informações, consulte [de roteamento e seleção de ação](../web-api-routing-and-actions/routing-and-action-selection.md).</span><span class="sxs-lookup"><span data-stu-id="25c15-116">For more information, see [Routing and Action Selection](../web-api-routing-and-actions/routing-and-action-selection.md).</span></span>

<span data-ttu-id="25c15-117">O restante deste artigo, mostrarei como você pode personalizar o processo de associação de modelo.</span><span class="sxs-lookup"><span data-stu-id="25c15-117">In the rest of this article, I'll show how you can customize the model binding process.</span></span> <span data-ttu-id="25c15-118">Para tipos complexos, no entanto, considere o uso de formatadores de tipo de mídia sempre que possível.</span><span class="sxs-lookup"><span data-stu-id="25c15-118">For complex types, however, consider using media-type formatters whenever possible.</span></span> <span data-ttu-id="25c15-119">Um princípio fundamental de HTTP é que os recursos são enviados no corpo da mensagem, usando a negociação de conteúdo para especificar a representação do recurso.</span><span class="sxs-lookup"><span data-stu-id="25c15-119">A key principle of HTTP is that resources are sent in the message body, using content negotiation to specify the representation of the resource.</span></span> <span data-ttu-id="25c15-120">Formatadores de tipo de mídia foram projetados para essa finalidade.</span><span class="sxs-lookup"><span data-stu-id="25c15-120">Media-type formatters were designed for exactly this purpose.</span></span>

## <a name="using-fromuri"></a><span data-ttu-id="25c15-121">Usando [FromUri]</span><span class="sxs-lookup"><span data-stu-id="25c15-121">Using [FromUri]</span></span>

<span data-ttu-id="25c15-122">Para forçar a API da Web para ler um tipo complexo do URI, adicione o **[FromUri]** ao parâmetro.</span><span class="sxs-lookup"><span data-stu-id="25c15-122">To force Web API to read a complex type from the URI, add the **[FromUri]** attribute to the parameter.</span></span> <span data-ttu-id="25c15-123">O exemplo a seguir define uma `GeoPoint` tipo, junto com um método de controlador que obtém o `GeoPoint` do URI.</span><span class="sxs-lookup"><span data-stu-id="25c15-123">The following example defines a `GeoPoint` type, along with a controller method that gets the `GeoPoint` from the URI.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample2.cs)]

<span data-ttu-id="25c15-124">O cliente pode colocar os valores de Latitude e Longitude na cadeia de caracteres de consulta e a API da Web usará para construir um `GeoPoint`.</span><span class="sxs-lookup"><span data-stu-id="25c15-124">The client can put the Latitude and Longitude values in the query string and Web API will use them to construct a `GeoPoint`.</span></span> <span data-ttu-id="25c15-125">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="25c15-125">For example:</span></span>

`http://localhost/api/values/?Latitude=47.678558&Longitude=-122.130989`

## <a name="using-frombody"></a><span data-ttu-id="25c15-126">Usando [FromBody]</span><span class="sxs-lookup"><span data-stu-id="25c15-126">Using [FromBody]</span></span>

<span data-ttu-id="25c15-127">Para forçar a API da Web para um tipo simples de leitura do corpo da solicitação, adicione o **[FromBody]** ao parâmetro:</span><span class="sxs-lookup"><span data-stu-id="25c15-127">To force Web API to read a simple type from the request body, add the **[FromBody]** attribute to the parameter:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="25c15-128">Neste exemplo, a API da Web usará um formatador de tipo de mídia para ler o valor de *nome* o corpo da solicitação.</span><span class="sxs-lookup"><span data-stu-id="25c15-128">In this example, Web API will use a media-type formatter to read the value of *name* from the request body.</span></span> <span data-ttu-id="25c15-129">Aqui está um exemplo de solicitação de cliente.</span><span class="sxs-lookup"><span data-stu-id="25c15-129">Here is an example client request.</span></span>

[!code-console[Main](parameter-binding-in-aspnet-web-api/samples/sample4.cmd)]

<span data-ttu-id="25c15-130">Quando um parâmetro tiver [FromBody], a API da Web usa o cabeçalho Content-Type para selecionar um formatador.</span><span class="sxs-lookup"><span data-stu-id="25c15-130">When a parameter has [FromBody], Web API uses the Content-Type header to select a formatter.</span></span> <span data-ttu-id="25c15-131">Neste exemplo, o tipo de conteúdo é &quot;aplicativo/json&quot; e o corpo da solicitação é uma cadeia de caracteres JSON bruta (não um objeto JSON).</span><span class="sxs-lookup"><span data-stu-id="25c15-131">In this example, the content type is &quot;application/json&quot; and the request body is a raw JSON string (not a JSON object).</span></span>

<span data-ttu-id="25c15-132">No máximo um parâmetro tem permissão para ler o corpo da mensagem.</span><span class="sxs-lookup"><span data-stu-id="25c15-132">At most one parameter is allowed to read from the message body.</span></span> <span data-ttu-id="25c15-133">Então isso não funcionará:</span><span class="sxs-lookup"><span data-stu-id="25c15-133">So this will not work:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample5.cs)]

<span data-ttu-id="25c15-134">O motivo para essa regra é que o corpo da solicitação pode ser armazenado em um fluxo não armazenado em buffer que só pode ser lidos uma vez.</span><span class="sxs-lookup"><span data-stu-id="25c15-134">The reason for this rule is that the request body might be stored in a non-buffered stream that can only be read once.</span></span>

## <a name="type-converters"></a><span data-ttu-id="25c15-135">Conversores de tipo</span><span class="sxs-lookup"><span data-stu-id="25c15-135">Type Converters</span></span>

<span data-ttu-id="25c15-136">Você pode fazer API da Web tratar uma classe como um tipo simples (de forma que API da Web tenta associá-lo do URI), criando um **TypeConverter** e fornecendo uma conversão de cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="25c15-136">You can make Web API treat a class as a simple type (so that Web API will try to bind it from the URI) by creating a **TypeConverter** and providing a string conversion.</span></span>

<span data-ttu-id="25c15-137">O código a seguir mostra um `GeoPoint` a classe que representa um ponto geográfico, mais um **TypeConverter** que converte de cadeias de caracteres para `GeoPoint` instâncias.</span><span class="sxs-lookup"><span data-stu-id="25c15-137">The following code shows a `GeoPoint` class that represents a geographical point, plus a **TypeConverter** that converts from strings to `GeoPoint` instances.</span></span> <span data-ttu-id="25c15-138">O `GeoPoint` classe é decorada com um **[TypeConverter]** atributo para especificar o conversor de tipo.</span><span class="sxs-lookup"><span data-stu-id="25c15-138">The `GeoPoint` class is decorated with a **[TypeConverter]** attribute to specify the type converter.</span></span> <span data-ttu-id="25c15-139">(Este exemplo foi inspirado pela postagem de blog de Mike Stall [como vincular a objetos personalizados em assinaturas de ação no MVC/WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx).)</span><span class="sxs-lookup"><span data-stu-id="25c15-139">(This example was inspired by Mike Stall's blog post [How to bind to custom objects in action signatures in MVC/WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx).)</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample6.cs)]

<span data-ttu-id="25c15-140">Agora a API da Web tratará `GeoPoint` como um tipo simple, que significa que ele irá tentar associar `GeoPoint` parâmetros do URI.</span><span class="sxs-lookup"><span data-stu-id="25c15-140">Now Web API will treat `GeoPoint` as a simple type, meaning it will try to bind `GeoPoint` parameters from the URI.</span></span> <span data-ttu-id="25c15-141">Você não precisa incluir **[FromUri]** no parâmetro.</span><span class="sxs-lookup"><span data-stu-id="25c15-141">You don't need to include **[FromUri]** on the parameter.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample7.cs)]

<span data-ttu-id="25c15-142">O cliente pode invocar o método com um URI como este:</span><span class="sxs-lookup"><span data-stu-id="25c15-142">The client can invoke the method with a URI like this:</span></span>

`http://localhost/api/values/?location=47.678558,-122.130989`

## <a name="model-binders"></a><span data-ttu-id="25c15-143">Associadores de modelo</span><span class="sxs-lookup"><span data-stu-id="25c15-143">Model Binders</span></span>

<span data-ttu-id="25c15-144">Uma opção mais flexível de um conversor de tipo é criar um associador de modelo personalizado.</span><span class="sxs-lookup"><span data-stu-id="25c15-144">A more flexible option than a type converter is to create a custom model binder.</span></span> <span data-ttu-id="25c15-145">Com um associador de modelo, você tem acesso a coisas como a solicitação HTTP, a descrição da ação e os valores brutos dos dados da rota.</span><span class="sxs-lookup"><span data-stu-id="25c15-145">With a model binder, you have access to things like the HTTP request, the action description, and the raw values from the route data.</span></span>

<span data-ttu-id="25c15-146">Para criar um associador de modelo, implementar a **IModelBinder** interface.</span><span class="sxs-lookup"><span data-stu-id="25c15-146">To create a model binder, implement the **IModelBinder** interface.</span></span> <span data-ttu-id="25c15-147">Essa interface define um único método, **BindModel**:</span><span class="sxs-lookup"><span data-stu-id="25c15-147">This interface defines a single method, **BindModel**:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample8.cs)]

<span data-ttu-id="25c15-148">Aqui está um associador de modelo para `GeoPoint` objetos.</span><span class="sxs-lookup"><span data-stu-id="25c15-148">Here is a model binder for `GeoPoint` objects.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample9.cs)]

<span data-ttu-id="25c15-149">Um associador de modelo obtém valores brutos de entrada de um *provedor de valor*.</span><span class="sxs-lookup"><span data-stu-id="25c15-149">A model binder gets raw input values from a *value provider*.</span></span> <span data-ttu-id="25c15-150">Esse design separa duas funções distintas:</span><span class="sxs-lookup"><span data-stu-id="25c15-150">This design separates two distinct functions:</span></span>

- <span data-ttu-id="25c15-151">O provedor de valor leva a solicitação HTTP e preenche um dicionário de pares chave-valor.</span><span class="sxs-lookup"><span data-stu-id="25c15-151">The value provider takes the HTTP request and populates a dictionary of key-value pairs.</span></span>
- <span data-ttu-id="25c15-152">O associador de modelo usa este dicionário para popular o modelo.</span><span class="sxs-lookup"><span data-stu-id="25c15-152">The model binder uses this dictionary to populate the model.</span></span>

<span data-ttu-id="25c15-153">O provedor de valor padrão na API da Web obtém valores de dados de rota e a cadeia de caracteres de consulta.</span><span class="sxs-lookup"><span data-stu-id="25c15-153">The default value provider in Web API gets values from the route data and the query string.</span></span> <span data-ttu-id="25c15-154">Por exemplo, se o URI é `http://localhost/api/values/1?location=48,-122`, o provedor de valor cria os seguintes pares chave-valor:</span><span class="sxs-lookup"><span data-stu-id="25c15-154">For example, if the URI is `http://localhost/api/values/1?location=48,-122`, the value provider creates the following key-value pairs:</span></span>

- <span data-ttu-id="25c15-155">ID = &quot;1&quot;</span><span class="sxs-lookup"><span data-stu-id="25c15-155">id = &quot;1&quot;</span></span>
- <span data-ttu-id="25c15-156">local = &quot;48,122&quot;</span><span class="sxs-lookup"><span data-stu-id="25c15-156">location = &quot;48,122&quot;</span></span>

<span data-ttu-id="25c15-157">(Estou supondo que o modelo de rota padrão, que é &quot;api / {controller} / {id}&quot;.)</span><span class="sxs-lookup"><span data-stu-id="25c15-157">(I'm assuming the default route template, which is &quot;api/{controller}/{id}&quot;.)</span></span>

<span data-ttu-id="25c15-158">O nome do parâmetro a associar é armazenado no **ModelBindingContext.ModelName** propriedade.</span><span class="sxs-lookup"><span data-stu-id="25c15-158">The name of the parameter to bind is stored in the **ModelBindingContext.ModelName** property.</span></span> <span data-ttu-id="25c15-159">O associador de modelo procura uma chave com esse valor no dicionário.</span><span class="sxs-lookup"><span data-stu-id="25c15-159">The model binder looks for a key with this value in the dictionary.</span></span> <span data-ttu-id="25c15-160">Se o valor existe e pode ser convertido em um `GeoPoint`, o associador de modelo atribui o valor de limite para o **ModelBindingContext.Model** propriedade.</span><span class="sxs-lookup"><span data-stu-id="25c15-160">If the value exists and can be converted into a `GeoPoint`, the model binder assigns the bound value to the **ModelBindingContext.Model** property.</span></span>

<span data-ttu-id="25c15-161">Observe que o associador de modelo não é limitado a uma conversão de tipo simples.</span><span class="sxs-lookup"><span data-stu-id="25c15-161">Notice that the model binder is not limited to a simple type conversion.</span></span> <span data-ttu-id="25c15-162">Neste exemplo, primeiro procura o associador de modelo em uma tabela de locais conhecidos, e se isso falhar, ele usa a conversão de tipo.</span><span class="sxs-lookup"><span data-stu-id="25c15-162">In this example, the model binder first looks in a table of known locations, and if that fails, it uses type conversion.</span></span>

<span data-ttu-id="25c15-163">**Definir o associador de modelo**</span><span class="sxs-lookup"><span data-stu-id="25c15-163">**Setting the Model Binder**</span></span>

<span data-ttu-id="25c15-164">Há várias maneiras de definir um associador de modelo.</span><span class="sxs-lookup"><span data-stu-id="25c15-164">There are several ways to set a model binder.</span></span> <span data-ttu-id="25c15-165">Primeiro, você pode adicionar uma **[ModelBinder]** ao parâmetro.</span><span class="sxs-lookup"><span data-stu-id="25c15-165">First, you can add a **[ModelBinder]** attribute to the parameter.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample10.cs)]

<span data-ttu-id="25c15-166">Você também pode adicionar uma **[ModelBinder]** para o tipo de atributo.</span><span class="sxs-lookup"><span data-stu-id="25c15-166">You can also add a **[ModelBinder]** attribute to the type.</span></span> <span data-ttu-id="25c15-167">API da Web usará o associador de modelo especificado para todos os parâmetros de tipo.</span><span class="sxs-lookup"><span data-stu-id="25c15-167">Web API will use the specified model binder for all parameters of that type.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample11.cs)]

<span data-ttu-id="25c15-168">Por fim, você pode adicionar um provedor de associador de modelo para o **HttpConfiguration**.</span><span class="sxs-lookup"><span data-stu-id="25c15-168">Finally, you can add a model-binder provider to the **HttpConfiguration**.</span></span> <span data-ttu-id="25c15-169">Um provedor de associador de modelo é simplesmente uma classe de fábrica que cria um associador de modelo.</span><span class="sxs-lookup"><span data-stu-id="25c15-169">A model-binder provider is simply a factory class that creates a model binder.</span></span> <span data-ttu-id="25c15-170">Você pode criar um provedor derivando de [ModelBinderProvider](https://msdn.microsoft.com/en-us/library/system.web.http.modelbinding.modelbinderprovider.aspx) classe.</span><span class="sxs-lookup"><span data-stu-id="25c15-170">You can create a provider by deriving from the [ModelBinderProvider](https://msdn.microsoft.com/en-us/library/system.web.http.modelbinding.modelbinderprovider.aspx) class.</span></span> <span data-ttu-id="25c15-171">No entanto, se o associador de modelo manipula um único tipo, é mais fácil de usar interno **SimpleModelBinderProvider**, que é projetado para essa finalidade.</span><span class="sxs-lookup"><span data-stu-id="25c15-171">However, if your model binder handles a single type, it's easier to use the built-in **SimpleModelBinderProvider**, which is designed for this purpose.</span></span> <span data-ttu-id="25c15-172">O código a seguir mostra como fazer isso.</span><span class="sxs-lookup"><span data-stu-id="25c15-172">The following code shows how to do this.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample12.cs)]

<span data-ttu-id="25c15-173">Com um provedor de associação de modelo, você ainda precisa adicionar o **[ModelBinder]** de atributo para o parâmetro, informe a API da Web que ele deve usar um associador de modelo e não um formatador do tipo de mídia.</span><span class="sxs-lookup"><span data-stu-id="25c15-173">With a model-binding provider, you still need to add the **[ModelBinder]** attribute to the parameter, to tell Web API that it should use a model binder and not a media-type formatter.</span></span> <span data-ttu-id="25c15-174">Mas, agora você não precisa especificar o tipo de associador de modelo no atributo:</span><span class="sxs-lookup"><span data-stu-id="25c15-174">But now you don't need to specify the type of model binder in the attribute:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample13.cs)]

## <a name="value-providers"></a><span data-ttu-id="25c15-175">Provedores de valor</span><span class="sxs-lookup"><span data-stu-id="25c15-175">Value Providers</span></span>

<span data-ttu-id="25c15-176">Mencionei que um associador de modelo obtém valores de um provedor de valor.</span><span class="sxs-lookup"><span data-stu-id="25c15-176">I mentioned that a model binder gets values from a value provider.</span></span> <span data-ttu-id="25c15-177">Para escrever um provedor de valor personalizado, implemente o **IValueProvider** interface.</span><span class="sxs-lookup"><span data-stu-id="25c15-177">To write a custom value provider, implement the **IValueProvider** interface.</span></span> <span data-ttu-id="25c15-178">Aqui está um exemplo que recebe os valores dos cookies na solicitação:</span><span class="sxs-lookup"><span data-stu-id="25c15-178">Here is an example that pulls values from the cookies in the request:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample14.cs)]

<span data-ttu-id="25c15-179">Você também precisa criar uma fábrica de provedor de valor derivando de **ValueProviderFactory** classe.</span><span class="sxs-lookup"><span data-stu-id="25c15-179">You also need to create a value provider factory by deriving from the **ValueProviderFactory** class.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample15.cs)]

<span data-ttu-id="25c15-180">Adicionar a fábrica de provedor de valor para o **HttpConfiguration** da seguinte maneira.</span><span class="sxs-lookup"><span data-stu-id="25c15-180">Add the value provider factory to the **HttpConfiguration** as follows.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample16.cs)]

<span data-ttu-id="25c15-181">API da Web compõe a todos os provedores de valor, portanto quando chama um associador de modelo **ValueProvider.GetValue**, o associador de modelo recebe o valor do primeiro provedor de valor que pode produzir a ele.</span><span class="sxs-lookup"><span data-stu-id="25c15-181">Web API composes all of the value providers, so when a model binder calls **ValueProvider.GetValue**, the model binder receives the value from the first value provider that is able to produce it.</span></span>

<span data-ttu-id="25c15-182">Como alternativa, você pode definir a fábrica de provedor de valor no nível de parâmetro usando o **ValueProvider** atributo, da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="25c15-182">Alternatively, you can set the value provider factory at the parameter level by using the **ValueProvider** attribute, as follows:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample17.cs)]

<span data-ttu-id="25c15-183">Isso informa a API da Web para usar a associação de modelo com a fábrica de provedor de valor especificado e para não usar nenhum dos provedores de valor registrado.</span><span class="sxs-lookup"><span data-stu-id="25c15-183">This tells Web API to use model binding with the specified value provider factory, and not to use any of the other registered value providers.</span></span>

## <a name="httpparameterbinding"></a><span data-ttu-id="25c15-184">HttpParameterBinding</span><span class="sxs-lookup"><span data-stu-id="25c15-184">HttpParameterBinding</span></span>

<span data-ttu-id="25c15-185">Associadores de modelo são uma instância específica de um mecanismo mais geral.</span><span class="sxs-lookup"><span data-stu-id="25c15-185">Model binders are a specific instance of a more general mechanism.</span></span> <span data-ttu-id="25c15-186">Se você observar o **[ModelBinder]** atributo, você verá que deriva de abstrata **ParameterBindingAttribute** classe.</span><span class="sxs-lookup"><span data-stu-id="25c15-186">If you look at the **[ModelBinder]** attribute, you will see that it derives from the abstract **ParameterBindingAttribute** class.</span></span> <span data-ttu-id="25c15-187">Essa classe define um único método, **GetBinding**, que retorna um **HttpParameterBinding** objeto:</span><span class="sxs-lookup"><span data-stu-id="25c15-187">This class defines a single method, **GetBinding**, which returns an **HttpParameterBinding** object:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample18.cs)]

<span data-ttu-id="25c15-188">Um **HttpParameterBinding** é responsável por um parâmetro de associação para um valor.</span><span class="sxs-lookup"><span data-stu-id="25c15-188">An **HttpParameterBinding** is responsible for binding a parameter to a value.</span></span> <span data-ttu-id="25c15-189">No caso de **[ModelBinder]**, o atributo retorna um **HttpParameterBinding** implementação que usa um **IModelBinder** para realizar a associação real.</span><span class="sxs-lookup"><span data-stu-id="25c15-189">In the case of **[ModelBinder]**, the attribute returns an **HttpParameterBinding** implementation that uses an **IModelBinder** to perform the actual binding.</span></span> <span data-ttu-id="25c15-190">Você também pode implementar seu próprio **HttpParameterBinding**.</span><span class="sxs-lookup"><span data-stu-id="25c15-190">You can also implement your own **HttpParameterBinding**.</span></span>

<span data-ttu-id="25c15-191">Por exemplo, suponha que você deseja obter ETags de `if-match` e `if-none-match` cabeçalhos na solicitação.</span><span class="sxs-lookup"><span data-stu-id="25c15-191">For example, suppose you want to get ETags from `if-match` and `if-none-match` headers in the request.</span></span> <span data-ttu-id="25c15-192">Vamos começar definindo uma classe para representar ETags.</span><span class="sxs-lookup"><span data-stu-id="25c15-192">We'll start by defining a class to represent ETags.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample19.cs)]

<span data-ttu-id="25c15-193">Também vamos definir uma enumeração para indicar se deve obter o ETag a partir de `if-match` cabeçalho ou o `if-none-match` cabeçalho.</span><span class="sxs-lookup"><span data-stu-id="25c15-193">We'll also define an enumeration to indicate whether to get the ETag from the `if-match` header or the `if-none-match` header.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample20.cs)]

<span data-ttu-id="25c15-194">Aqui está uma **HttpParameterBinding** que obtém a ETag do cabeçalho desejado e o associa a um parâmetro de tipo ETag:</span><span class="sxs-lookup"><span data-stu-id="25c15-194">Here is an **HttpParameterBinding** that gets the ETag from the desired header and binds it to a parameter of type ETag:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample21.cs)]

<span data-ttu-id="25c15-195">O **ExecuteBindingAsync** método faz a associação.</span><span class="sxs-lookup"><span data-stu-id="25c15-195">The **ExecuteBindingAsync** method does the binding.</span></span> <span data-ttu-id="25c15-196">Dentro desse método, adicione o valor de parâmetro associados para o **ActionArgument** dicionário no **HttpActionContext**.</span><span class="sxs-lookup"><span data-stu-id="25c15-196">Within this method, add the bound parameter value to the **ActionArgument** dictionary in the **HttpActionContext**.</span></span>

> [!NOTE]
> <span data-ttu-id="25c15-197">Se seu **ExecuteBindingAsync** método lê o corpo da mensagem de solicitação, substitua o **WillReadBody** propriedade para retornar true.</span><span class="sxs-lookup"><span data-stu-id="25c15-197">If your **ExecuteBindingAsync** method reads the body of the request message, override the **WillReadBody** property to return true.</span></span> <span data-ttu-id="25c15-198">O corpo da solicitação pode ser um fluxo sem buffer que só pode ser lidos uma vez, para a API da Web aplica uma regra que no máximo uma associação pode ler o corpo da mensagem.</span><span class="sxs-lookup"><span data-stu-id="25c15-198">The request body might be an unbuffered stream that can only be read once, so Web API enforces a rule that at most one binding can read the message body.</span></span>


<span data-ttu-id="25c15-199">Para aplicar um personalizado **HttpParameterBinding**, você pode definir um atributo que é derivada de **ParameterBindingAttribute**.</span><span class="sxs-lookup"><span data-stu-id="25c15-199">To apply a custom **HttpParameterBinding**, you can define an attribute that derives from **ParameterBindingAttribute**.</span></span> <span data-ttu-id="25c15-200">Para `ETagParameterBinding`, vamos definir dois atributos, uma para `if-match` cabeçalhos e outro para `if-none-match` cabeçalhos.</span><span class="sxs-lookup"><span data-stu-id="25c15-200">For `ETagParameterBinding`, we'll define two attributes, one for `if-match` headers and one for `if-none-match` headers.</span></span> <span data-ttu-id="25c15-201">Ambos derivam de uma classe base abstrata.</span><span class="sxs-lookup"><span data-stu-id="25c15-201">Both derive from an abstract base class.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample22.cs)]

<span data-ttu-id="25c15-202">Este é um método de controlador que usa o `[IfNoneMatch]` atributo.</span><span class="sxs-lookup"><span data-stu-id="25c15-202">Here is a controller method that uses the `[IfNoneMatch]` attribute.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample23.cs)]

<span data-ttu-id="25c15-203">Além disso **ParameterBindingAttribute**, há outro gancho para adicionar um personalizado **HttpParameterBinding**.</span><span class="sxs-lookup"><span data-stu-id="25c15-203">Besides **ParameterBindingAttribute**, there is another hook for adding a custom **HttpParameterBinding**.</span></span> <span data-ttu-id="25c15-204">Sobre o **HttpConfiguration** objeto, o **ParameterBindingRules** propriedade é uma coleção de funções de anomymous do tipo (**HttpParameterDescriptor**  - &gt; **HttpParameterBinding**).</span><span class="sxs-lookup"><span data-stu-id="25c15-204">On the **HttpConfiguration** object, the **ParameterBindingRules** property is a collection of anomymous functions of type (**HttpParameterDescriptor** -&gt; **HttpParameterBinding**).</span></span> <span data-ttu-id="25c15-205">Por exemplo, você pode adicionar uma regra que usa qualquer parâmetro de ETag em um método GET `ETagParameterBinding` com `if-none-match`:</span><span class="sxs-lookup"><span data-stu-id="25c15-205">For example, you could add a rule that any ETag parameter on a GET method uses `ETagParameterBinding` with `if-none-match`:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample24.cs)]

<span data-ttu-id="25c15-206">A função deve retornar `null` para parâmetros, onde a associação não é aplicável.</span><span class="sxs-lookup"><span data-stu-id="25c15-206">The function should return `null` for parameters where the binding is not applicable.</span></span>

## <a name="iactionvaluebinder"></a><span data-ttu-id="25c15-207">IActionValueBinder</span><span class="sxs-lookup"><span data-stu-id="25c15-207">IActionValueBinder</span></span>

<span data-ttu-id="25c15-208">Todo o processo de associação de parâmetros é controlado por um serviço conectável, **IActionValueBinder**.</span><span class="sxs-lookup"><span data-stu-id="25c15-208">The entire parameter-binding process is controlled by a pluggable service, **IActionValueBinder**.</span></span> <span data-ttu-id="25c15-209">A implementação padrão de **IActionValueBinder** faz o seguinte:</span><span class="sxs-lookup"><span data-stu-id="25c15-209">The default implementation of **IActionValueBinder** does the following:</span></span>

1. <span data-ttu-id="25c15-210">Procure um **ParameterBindingAttribute** no parâmetro.</span><span class="sxs-lookup"><span data-stu-id="25c15-210">Look for a **ParameterBindingAttribute** on the parameter.</span></span> <span data-ttu-id="25c15-211">Isso inclui **[FromBody]**, **[FromUri]**, e **[ModelBinder]**, ou atributos personalizados.</span><span class="sxs-lookup"><span data-stu-id="25c15-211">This includes **[FromBody]**, **[FromUri]**, and **[ModelBinder]**, or custom attributes.</span></span>
2. <span data-ttu-id="25c15-212">Caso contrário, examine **HttpConfiguration.ParameterBindingRules** para uma função que retorna um nulo não **HttpParameterBinding**.</span><span class="sxs-lookup"><span data-stu-id="25c15-212">Otherwise, look in **HttpConfiguration.ParameterBindingRules** for a function that returns a non-null **HttpParameterBinding**.</span></span>
3. <span data-ttu-id="25c15-213">Caso contrário, use as regras padrão que descrita anteriormente.</span><span class="sxs-lookup"><span data-stu-id="25c15-213">Otherwise, use the default rules that I described previously.</span></span> 

    - <span data-ttu-id="25c15-214">Se o tipo de parâmetro é "simple" ou possui um conversor de tipo, associe do URI.</span><span class="sxs-lookup"><span data-stu-id="25c15-214">If the parameter type is "simple"or has a type converter, bind from the URI.</span></span> <span data-ttu-id="25c15-215">Isso é equivalente a colocar o **[FromUri]** atributo no parâmetro.</span><span class="sxs-lookup"><span data-stu-id="25c15-215">This is equivalent to putting the **[FromUri]** attribute on the parameter.</span></span>
    - <span data-ttu-id="25c15-216">Caso contrário, tente ler o parâmetro do corpo da mensagem.</span><span class="sxs-lookup"><span data-stu-id="25c15-216">Otherwise, try to read the parameter from the message body.</span></span> <span data-ttu-id="25c15-217">Isso é equivalente a colocar **[FromBody]** no parâmetro.</span><span class="sxs-lookup"><span data-stu-id="25c15-217">This is equivalent to putting **[FromBody]** on the parameter.</span></span>

<span data-ttu-id="25c15-218">Se desejar, você pode substituir toda a **IActionValueBinder** serviço com uma implementação personalizada.</span><span class="sxs-lookup"><span data-stu-id="25c15-218">If you wanted, you could replace the entire **IActionValueBinder** service with a custom implementation.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="25c15-219">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="25c15-219">Additional Resources</span></span>

[<span data-ttu-id="25c15-220">Exemplo de associação de parâmetro personalizado</span><span class="sxs-lookup"><span data-stu-id="25c15-220">Custom Parameter Binding Sample</span></span>](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/CustomParameterBinding/ReadMe.txt)

<span data-ttu-id="25c15-221">Mike Stall escreveu uma boa série de postagens de blog sobre associação de parâmetros de API da Web:</span><span class="sxs-lookup"><span data-stu-id="25c15-221">Mike Stall wrote a good series of blog posts about Web API parameter binding:</span></span>

- [<span data-ttu-id="25c15-222">Como a API da Web faz a associação de parâmetro</span><span class="sxs-lookup"><span data-stu-id="25c15-222">How Web API does Parameter Binding</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/16/how-webapi-does-parameter-binding.aspx)
- [<span data-ttu-id="25c15-223">Associação de parâmetro de estilo MVC para API da Web</span><span class="sxs-lookup"><span data-stu-id="25c15-223">MVC Style parameter binding for Web API</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/18/mvc-style-parameter-binding-for-webapi.aspx)
- [<span data-ttu-id="25c15-224">Como associar a objetos personalizados em assinaturas de ação na API MVC/da Web</span><span class="sxs-lookup"><span data-stu-id="25c15-224">How to bind to custom objects in action signatures in MVC/Web API</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx)
- [<span data-ttu-id="25c15-225">Como criar um provedor de valor personalizado na API da Web</span><span class="sxs-lookup"><span data-stu-id="25c15-225">How to create a custom value provider in Web API</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx)
- [<span data-ttu-id="25c15-226">Associação de parâmetros de API da Web nos bastidores</span><span class="sxs-lookup"><span data-stu-id="25c15-226">Web API Parameter binding under the hood</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx)

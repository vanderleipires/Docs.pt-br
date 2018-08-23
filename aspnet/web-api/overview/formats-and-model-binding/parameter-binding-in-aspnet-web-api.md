---
uid: web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
title: Associação de parâmetro na API Web ASP.NET | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/11/2013
ms.assetid: e42c8388-04ed-4341-9fdb-41b1b4c06320
msc.legacyurl: /web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 4d29f087cd658faf1fadb0d9a85e9f32c03a2b3f
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825023"
---
<a name="parameter-binding-in-aspnet-web-api"></a><span data-ttu-id="c9229-102">Associação de parâmetro na API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="c9229-102">Parameter Binding in ASP.NET Web API</span></span>
====================
<span data-ttu-id="c9229-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="c9229-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="c9229-104">Quando a API da Web chama um método em um controlador, ele deve definir valores para os parâmetros, um processo chamado *associação*.</span><span class="sxs-lookup"><span data-stu-id="c9229-104">When Web API calls a method on a controller, it must set values for the parameters, a process called *binding*.</span></span> <span data-ttu-id="c9229-105">Este artigo descreve como a API da Web associa parâmetros e como você pode personalizar o processo de associação.</span><span class="sxs-lookup"><span data-stu-id="c9229-105">This article describes how Web API binds parameters, and how you can customize the binding process.</span></span>

<span data-ttu-id="c9229-106">Por padrão, a API da Web usa as regras a seguir para associar parâmetros:</span><span class="sxs-lookup"><span data-stu-id="c9229-106">By default, Web API uses the following rules to bind parameters:</span></span>

- <span data-ttu-id="c9229-107">Se o parâmetro for um tipo de "simple", a API da Web tenta obter o valor do URI.</span><span class="sxs-lookup"><span data-stu-id="c9229-107">If the parameter is a "simple" type, Web API tries to get the value from the URI.</span></span> <span data-ttu-id="c9229-108">Os tipos simples incluem o .NET [tipos primitivos](https://msdn.microsoft.com/library/system.type.isprimitive.aspx) (**int**, **bool**, **double**e assim por diante), além de **TimeSpan**, **DateTime**, **Guid**, **decimal**, e **string**, *plus* qualquer tipo com um conversor de tipo pode converter de uma cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="c9229-108">Simple types include the .NET [primitive types](https://msdn.microsoft.com/library/system.type.isprimitive.aspx) (**int**, **bool**, **double**, and so forth), plus **TimeSpan**, **DateTime**, **Guid**, **decimal**, and **string**, *plus* any type with a type converter that can convert from a string.</span></span> <span data-ttu-id="c9229-109">(Mais informações sobre conversores de tipo posteriormente.)</span><span class="sxs-lookup"><span data-stu-id="c9229-109">(More about type converters later.)</span></span>
- <span data-ttu-id="c9229-110">Para tipos complexos, a API da Web tenta ler o valor do corpo da mensagem, usando um [formatador do tipo de mídia](media-formatters.md).</span><span class="sxs-lookup"><span data-stu-id="c9229-110">For complex types, Web API tries to read the value from the message body, using a [media-type formatter](media-formatters.md).</span></span>

<span data-ttu-id="c9229-111">Por exemplo, aqui está um método de controlador de API da Web típica:</span><span class="sxs-lookup"><span data-stu-id="c9229-111">For example, here is a typical Web API controller method:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="c9229-112">O *identificação* parâmetro é um &quot;simples&quot; digitar, portanto, a API da Web tenta obter o valor do URI de solicitação.</span><span class="sxs-lookup"><span data-stu-id="c9229-112">The *id* parameter is a &quot;simple&quot; type, so Web API tries to get the value from the request URI.</span></span> <span data-ttu-id="c9229-113">O *item* parâmetro é um tipo complexo, portanto, API Web usa um formatador de tipo de mídia para ler o valor do corpo da solicitação.</span><span class="sxs-lookup"><span data-stu-id="c9229-113">The *item* parameter is a complex type, so Web API uses a media-type formatter to read the value from the request body.</span></span>

<span data-ttu-id="c9229-114">Para obter um valor do URI, API Web examina os dados da rota e a cadeia de caracteres de consulta do URI.</span><span class="sxs-lookup"><span data-stu-id="c9229-114">To get a value from the URI, Web API looks in the route data and the URI query string.</span></span> <span data-ttu-id="c9229-115">Os dados da rota são preenchidos quando o sistema de roteamento analisa o URI e corresponde a uma rota.</span><span class="sxs-lookup"><span data-stu-id="c9229-115">The route data is populated when the routing system parses the URI and matches it to a route.</span></span> <span data-ttu-id="c9229-116">Para obter mais informações, consulte [roteamento e seleção de ação](../web-api-routing-and-actions/routing-and-action-selection.md).</span><span class="sxs-lookup"><span data-stu-id="c9229-116">For more information, see [Routing and Action Selection](../web-api-routing-and-actions/routing-and-action-selection.md).</span></span>

<span data-ttu-id="c9229-117">No restante deste artigo, mostrarei como você pode personalizar o processo de associação de modelo.</span><span class="sxs-lookup"><span data-stu-id="c9229-117">In the rest of this article, I'll show how you can customize the model binding process.</span></span> <span data-ttu-id="c9229-118">Para tipos complexos, no entanto, considere o uso de formatadores de tipo de mídia sempre que possível.</span><span class="sxs-lookup"><span data-stu-id="c9229-118">For complex types, however, consider using media-type formatters whenever possible.</span></span> <span data-ttu-id="c9229-119">Um princípio fundamental do HTTP é que os recursos são enviados no corpo da mensagem, usando a negociação de conteúdo para especificar a representação do recurso.</span><span class="sxs-lookup"><span data-stu-id="c9229-119">A key principle of HTTP is that resources are sent in the message body, using content negotiation to specify the representation of the resource.</span></span> <span data-ttu-id="c9229-120">Formatadores de tipo de mídia foram projetados para essa finalidade.</span><span class="sxs-lookup"><span data-stu-id="c9229-120">Media-type formatters were designed for exactly this purpose.</span></span>

## <a name="using-fromuri"></a><span data-ttu-id="c9229-121">Usando [FromUri]</span><span class="sxs-lookup"><span data-stu-id="c9229-121">Using [FromUri]</span></span>

<span data-ttu-id="c9229-122">Para forçar a API da Web para ler um tipo complexo do URI, adicione a **[FromUri]** ao parâmetro.</span><span class="sxs-lookup"><span data-stu-id="c9229-122">To force Web API to read a complex type from the URI, add the **[FromUri]** attribute to the parameter.</span></span> <span data-ttu-id="c9229-123">O exemplo a seguir define uma `GeoPoint` tipo, juntamente com um método controlador que obtém o `GeoPoint` do URI.</span><span class="sxs-lookup"><span data-stu-id="c9229-123">The following example defines a `GeoPoint` type, along with a controller method that gets the `GeoPoint` from the URI.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample2.cs)]

<span data-ttu-id="c9229-124">O cliente pode colocar os valores de Latitude e Longitude na cadeia de caracteres de consulta e a API da Web usará para construir um `GeoPoint`.</span><span class="sxs-lookup"><span data-stu-id="c9229-124">The client can put the Latitude and Longitude values in the query string and Web API will use them to construct a `GeoPoint`.</span></span> <span data-ttu-id="c9229-125">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="c9229-125">For example:</span></span>

`http://localhost/api/values/?Latitude=47.678558&Longitude=-122.130989`

## <a name="using-frombody"></a><span data-ttu-id="c9229-126">Usando [FromBody]</span><span class="sxs-lookup"><span data-stu-id="c9229-126">Using [FromBody]</span></span>

<span data-ttu-id="c9229-127">Para forçar a API da Web para um tipo simples de leitura do corpo da solicitação, adicione a **[FromBody]** ao parâmetro:</span><span class="sxs-lookup"><span data-stu-id="c9229-127">To force Web API to read a simple type from the request body, add the **[FromBody]** attribute to the parameter:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="c9229-128">Neste exemplo, a API da Web usará um formatador de tipo de mídia para ler o valor de *nome* do corpo da solicitação.</span><span class="sxs-lookup"><span data-stu-id="c9229-128">In this example, Web API will use a media-type formatter to read the value of *name* from the request body.</span></span> <span data-ttu-id="c9229-129">Aqui está um exemplo de solicitação de cliente.</span><span class="sxs-lookup"><span data-stu-id="c9229-129">Here is an example client request.</span></span>

[!code-console[Main](parameter-binding-in-aspnet-web-api/samples/sample4.cmd)]

<span data-ttu-id="c9229-130">Quando um parâmetro tiver [FromBody], API Web usa o cabeçalho Content-Type para selecionar um formatador.</span><span class="sxs-lookup"><span data-stu-id="c9229-130">When a parameter has [FromBody], Web API uses the Content-Type header to select a formatter.</span></span> <span data-ttu-id="c9229-131">Neste exemplo, é o tipo de conteúdo &quot;application/json&quot; e o corpo da solicitação é uma cadeia de caracteres JSON bruta (não um objeto JSON).</span><span class="sxs-lookup"><span data-stu-id="c9229-131">In this example, the content type is &quot;application/json&quot; and the request body is a raw JSON string (not a JSON object).</span></span>

<span data-ttu-id="c9229-132">No máximo um parâmetro tem permissão para ler o corpo da mensagem.</span><span class="sxs-lookup"><span data-stu-id="c9229-132">At most one parameter is allowed to read from the message body.</span></span> <span data-ttu-id="c9229-133">Portanto, isso não funcionará:</span><span class="sxs-lookup"><span data-stu-id="c9229-133">So this will not work:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample5.cs)]

<span data-ttu-id="c9229-134">O motivo para essa regra é que o corpo da solicitação pode ser armazenado em um fluxo não armazenado em buffer que só pode ser lidos uma vez.</span><span class="sxs-lookup"><span data-stu-id="c9229-134">The reason for this rule is that the request body might be stored in a non-buffered stream that can only be read once.</span></span>

## <a name="type-converters"></a><span data-ttu-id="c9229-135">Conversores de tipo</span><span class="sxs-lookup"><span data-stu-id="c9229-135">Type Converters</span></span>

<span data-ttu-id="c9229-136">Você pode fazer API da Web trate uma classe como um tipo simples (de modo que API Web tentará associá-lo do URI), criando uma **TypeConverter** e fornecendo uma conversão de cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="c9229-136">You can make Web API treat a class as a simple type (so that Web API will try to bind it from the URI) by creating a **TypeConverter** and providing a string conversion.</span></span>

<span data-ttu-id="c9229-137">O código a seguir mostra uma `GeoPoint` a classe que representa um ponto geográfico, além de um **TypeConverter** que converte de cadeias de caracteres para `GeoPoint` instâncias.</span><span class="sxs-lookup"><span data-stu-id="c9229-137">The following code shows a `GeoPoint` class that represents a geographical point, plus a **TypeConverter** that converts from strings to `GeoPoint` instances.</span></span> <span data-ttu-id="c9229-138">O `GeoPoint` classe é decorada com um **[TypeConverter]** atributo para especificar o conversor de tipo.</span><span class="sxs-lookup"><span data-stu-id="c9229-138">The `GeoPoint` class is decorated with a **[TypeConverter]** attribute to specify the type converter.</span></span> <span data-ttu-id="c9229-139">(Este exemplo foi inspirado pelo postagem de blog de Mike Stall [como associar a objetos personalizados em assinaturas de ação no MVC/WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx).)</span><span class="sxs-lookup"><span data-stu-id="c9229-139">(This example was inspired by Mike Stall's blog post [How to bind to custom objects in action signatures in MVC/WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx).)</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample6.cs)]

<span data-ttu-id="c9229-140">Agora a API da Web tratará `GeoPoint` como um tipo simples, que significa que ele tentará associar `GeoPoint` parâmetros do URI.</span><span class="sxs-lookup"><span data-stu-id="c9229-140">Now Web API will treat `GeoPoint` as a simple type, meaning it will try to bind `GeoPoint` parameters from the URI.</span></span> <span data-ttu-id="c9229-141">Você não precisa incluir **[FromUri]** no parâmetro.</span><span class="sxs-lookup"><span data-stu-id="c9229-141">You don't need to include **[FromUri]** on the parameter.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample7.cs)]

<span data-ttu-id="c9229-142">O cliente pode invocar o método com um URI como este:</span><span class="sxs-lookup"><span data-stu-id="c9229-142">The client can invoke the method with a URI like this:</span></span>

`http://localhost/api/values/?location=47.678558,-122.130989`

## <a name="model-binders"></a><span data-ttu-id="c9229-143">Associadores de modelo</span><span class="sxs-lookup"><span data-stu-id="c9229-143">Model Binders</span></span>

<span data-ttu-id="c9229-144">É uma opção mais flexível do que um conversor de tipo criar um associador de modelo personalizado.</span><span class="sxs-lookup"><span data-stu-id="c9229-144">A more flexible option than a type converter is to create a custom model binder.</span></span> <span data-ttu-id="c9229-145">Com um associador de modelo, você tem acesso a coisas como a solicitação HTTP, a descrição da ação e os valores brutos dos dados de rota.</span><span class="sxs-lookup"><span data-stu-id="c9229-145">With a model binder, you have access to things like the HTTP request, the action description, and the raw values from the route data.</span></span>

<span data-ttu-id="c9229-146">Para criar um associador de modelo, implemente a **IModelBinder** interface.</span><span class="sxs-lookup"><span data-stu-id="c9229-146">To create a model binder, implement the **IModelBinder** interface.</span></span> <span data-ttu-id="c9229-147">Essa interface define um único método, **BindModel**:</span><span class="sxs-lookup"><span data-stu-id="c9229-147">This interface defines a single method, **BindModel**:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample8.cs)]

<span data-ttu-id="c9229-148">Aqui está um associador de modelo para `GeoPoint` objetos.</span><span class="sxs-lookup"><span data-stu-id="c9229-148">Here is a model binder for `GeoPoint` objects.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample9.cs)]

<span data-ttu-id="c9229-149">Um associador de modelo obtém os valores brutos de entrada de um *provedor de valor*.</span><span class="sxs-lookup"><span data-stu-id="c9229-149">A model binder gets raw input values from a *value provider*.</span></span> <span data-ttu-id="c9229-150">Esse design separa duas funções distintas:</span><span class="sxs-lookup"><span data-stu-id="c9229-150">This design separates two distinct functions:</span></span>

- <span data-ttu-id="c9229-151">O provedor de valor usa a solicitação HTTP e popula um dicionário de pares chave-valor.</span><span class="sxs-lookup"><span data-stu-id="c9229-151">The value provider takes the HTTP request and populates a dictionary of key-value pairs.</span></span>
- <span data-ttu-id="c9229-152">O associador de modelo usa este dicionário para popular o modelo.</span><span class="sxs-lookup"><span data-stu-id="c9229-152">The model binder uses this dictionary to populate the model.</span></span>

<span data-ttu-id="c9229-153">O provedor de valor padrão na API da Web obtém valores de dados de rota e a cadeia de caracteres de consulta.</span><span class="sxs-lookup"><span data-stu-id="c9229-153">The default value provider in Web API gets values from the route data and the query string.</span></span> <span data-ttu-id="c9229-154">Por exemplo, se o URI é `http://localhost/api/values/1?location=48,-122`, o provedor de valor cria os seguintes pares chave-valor:</span><span class="sxs-lookup"><span data-stu-id="c9229-154">For example, if the URI is `http://localhost/api/values/1?location=48,-122`, the value provider creates the following key-value pairs:</span></span>

- <span data-ttu-id="c9229-155">id = &quot;1&quot;</span><span class="sxs-lookup"><span data-stu-id="c9229-155">id = &quot;1&quot;</span></span>
- <span data-ttu-id="c9229-156">local = &quot;48,122&quot;</span><span class="sxs-lookup"><span data-stu-id="c9229-156">location = &quot;48,122&quot;</span></span>

<span data-ttu-id="c9229-157">(Estou supondo que o modelo de rota padrão, que é &quot;api / {controller} / {id}&quot;.)</span><span class="sxs-lookup"><span data-stu-id="c9229-157">(I'm assuming the default route template, which is &quot;api/{controller}/{id}&quot;.)</span></span>

<span data-ttu-id="c9229-158">O nome do parâmetro a associar é armazenado na **ModelBindingContext.ModelName** propriedade.</span><span class="sxs-lookup"><span data-stu-id="c9229-158">The name of the parameter to bind is stored in the **ModelBindingContext.ModelName** property.</span></span> <span data-ttu-id="c9229-159">O associador de modelos procura por uma chave com esse valor no dicionário.</span><span class="sxs-lookup"><span data-stu-id="c9229-159">The model binder looks for a key with this value in the dictionary.</span></span> <span data-ttu-id="c9229-160">Se o valor existir e pode ser convertido em um `GeoPoint`, o associador de modelo atribui o valor associado para o **ModelBindingContext.Model** propriedade.</span><span class="sxs-lookup"><span data-stu-id="c9229-160">If the value exists and can be converted into a `GeoPoint`, the model binder assigns the bound value to the **ModelBindingContext.Model** property.</span></span>

<span data-ttu-id="c9229-161">Observe que o associador de modelo não é limitado a uma conversão de tipo simples.</span><span class="sxs-lookup"><span data-stu-id="c9229-161">Notice that the model binder is not limited to a simple type conversion.</span></span> <span data-ttu-id="c9229-162">Neste exemplo, o associador de modelo primeiro procura em uma tabela de locais conhecidos, e se isso falhar, ele usa uma conversão de tipo.</span><span class="sxs-lookup"><span data-stu-id="c9229-162">In this example, the model binder first looks in a table of known locations, and if that fails, it uses type conversion.</span></span>

<span data-ttu-id="c9229-163">**Definindo o associador de modelo**</span><span class="sxs-lookup"><span data-stu-id="c9229-163">**Setting the Model Binder**</span></span>

<span data-ttu-id="c9229-164">Há várias maneiras de definir um associador de modelo.</span><span class="sxs-lookup"><span data-stu-id="c9229-164">There are several ways to set a model binder.</span></span> <span data-ttu-id="c9229-165">Primeiro, você pode adicionar um **[ModelBinder]** ao parâmetro.</span><span class="sxs-lookup"><span data-stu-id="c9229-165">First, you can add a **[ModelBinder]** attribute to the parameter.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample10.cs)]

<span data-ttu-id="c9229-166">Você também pode adicionar um **[ModelBinder]** para o tipo de atributo.</span><span class="sxs-lookup"><span data-stu-id="c9229-166">You can also add a **[ModelBinder]** attribute to the type.</span></span> <span data-ttu-id="c9229-167">API da Web usará o associador de modelo especificado para todos os parâmetros desse tipo.</span><span class="sxs-lookup"><span data-stu-id="c9229-167">Web API will use the specified model binder for all parameters of that type.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample11.cs)]

<span data-ttu-id="c9229-168">Por fim, você pode adicionar um provedor de associador de modelo para o **HttpConfiguration**.</span><span class="sxs-lookup"><span data-stu-id="c9229-168">Finally, you can add a model-binder provider to the **HttpConfiguration**.</span></span> <span data-ttu-id="c9229-169">Um provedor de associador de modelo é simplesmente uma classe de fábrica que cria um associador de modelo.</span><span class="sxs-lookup"><span data-stu-id="c9229-169">A model-binder provider is simply a factory class that creates a model binder.</span></span> <span data-ttu-id="c9229-170">Você pode criar um provedor derivando de [ModelBinderProvider](https://msdn.microsoft.com/library/system.web.http.modelbinding.modelbinderprovider.aspx) classe.</span><span class="sxs-lookup"><span data-stu-id="c9229-170">You can create a provider by deriving from the [ModelBinderProvider](https://msdn.microsoft.com/library/system.web.http.modelbinding.modelbinderprovider.aspx) class.</span></span> <span data-ttu-id="c9229-171">No entanto, se o associador de modelo manipula um único tipo, é mais fácil de usar interno **SimpleModelBinderProvider**, que é projetado para essa finalidade.</span><span class="sxs-lookup"><span data-stu-id="c9229-171">However, if your model binder handles a single type, it's easier to use the built-in **SimpleModelBinderProvider**, which is designed for this purpose.</span></span> <span data-ttu-id="c9229-172">O código a seguir mostra como fazer isso.</span><span class="sxs-lookup"><span data-stu-id="c9229-172">The following code shows how to do this.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample12.cs)]

<span data-ttu-id="c9229-173">Com um provedor de associação de modelos, você ainda precisa adicionar o **[ModelBinder]** de atributo para o parâmetro, para informar a API da Web que ele deve usar um associador de modelo e não um formatador de tipo de mídia.</span><span class="sxs-lookup"><span data-stu-id="c9229-173">With a model-binding provider, you still need to add the **[ModelBinder]** attribute to the parameter, to tell Web API that it should use a model binder and not a media-type formatter.</span></span> <span data-ttu-id="c9229-174">Mas, agora você não precisa especificar o tipo de associador de modelo no atributo:</span><span class="sxs-lookup"><span data-stu-id="c9229-174">But now you don't need to specify the type of model binder in the attribute:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample13.cs)]

## <a name="value-providers"></a><span data-ttu-id="c9229-175">Provedores de valor</span><span class="sxs-lookup"><span data-stu-id="c9229-175">Value Providers</span></span>

<span data-ttu-id="c9229-176">Eu mencionei que um associador de modelo obtém valores de um provedor de valor.</span><span class="sxs-lookup"><span data-stu-id="c9229-176">I mentioned that a model binder gets values from a value provider.</span></span> <span data-ttu-id="c9229-177">Para escrever um provedor de valor personalizado, implemente a **IValueProvider** interface.</span><span class="sxs-lookup"><span data-stu-id="c9229-177">To write a custom value provider, implement the **IValueProvider** interface.</span></span> <span data-ttu-id="c9229-178">Aqui está um exemplo que extrai valores de cookies na solicitação:</span><span class="sxs-lookup"><span data-stu-id="c9229-178">Here is an example that pulls values from the cookies in the request:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample14.cs)]

<span data-ttu-id="c9229-179">Você também precisará criar uma fábrica de provedor de valor derivando de **ValueProviderFactory** classe.</span><span class="sxs-lookup"><span data-stu-id="c9229-179">You also need to create a value provider factory by deriving from the **ValueProviderFactory** class.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample15.cs)]

<span data-ttu-id="c9229-180">Adicionar a fábrica de provedor de valor para o **HttpConfiguration** da seguinte maneira.</span><span class="sxs-lookup"><span data-stu-id="c9229-180">Add the value provider factory to the **HttpConfiguration** as follows.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample16.cs)]

<span data-ttu-id="c9229-181">API da Web compõe todos os provedores de valor, portanto, quando um associador de modelo chama **ValueProvider.GetValue**, o associador de modelo recebe o valor do primeiro provedor de valor que é capaz de gerá-lo.</span><span class="sxs-lookup"><span data-stu-id="c9229-181">Web API composes all of the value providers, so when a model binder calls **ValueProvider.GetValue**, the model binder receives the value from the first value provider that is able to produce it.</span></span>

<span data-ttu-id="c9229-182">Como alternativa, você pode definir a fábrica de provedor de valor no nível de parâmetro usando o **ValueProvider** de atributo, da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="c9229-182">Alternatively, you can set the value provider factory at the parameter level by using the **ValueProvider** attribute, as follows:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample17.cs)]

<span data-ttu-id="c9229-183">Isso informa à API da Web para usar a associação de modelo com a fábrica de provedor de valor especificado e não para usar qualquer um dos outros provedores de valor registrados.</span><span class="sxs-lookup"><span data-stu-id="c9229-183">This tells Web API to use model binding with the specified value provider factory, and not to use any of the other registered value providers.</span></span>

## <a name="httpparameterbinding"></a><span data-ttu-id="c9229-184">HttpParameterBinding</span><span class="sxs-lookup"><span data-stu-id="c9229-184">HttpParameterBinding</span></span>

<span data-ttu-id="c9229-185">Associadores de modelo são uma instância específica de um mecanismo mais geral.</span><span class="sxs-lookup"><span data-stu-id="c9229-185">Model binders are a specific instance of a more general mechanism.</span></span> <span data-ttu-id="c9229-186">Se você examinar a **[ModelBinder]** atributo, você verá que ele deriva abstrata **ParameterBindingAttribute** classe.</span><span class="sxs-lookup"><span data-stu-id="c9229-186">If you look at the **[ModelBinder]** attribute, you will see that it derives from the abstract **ParameterBindingAttribute** class.</span></span> <span data-ttu-id="c9229-187">Essa classe define um único método, **GetBinding**, que retorna um **HttpParameterBinding** objeto:</span><span class="sxs-lookup"><span data-stu-id="c9229-187">This class defines a single method, **GetBinding**, which returns an **HttpParameterBinding** object:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample18.cs)]

<span data-ttu-id="c9229-188">Uma **HttpParameterBinding** é responsável por um parâmetro de associação para um valor.</span><span class="sxs-lookup"><span data-stu-id="c9229-188">An **HttpParameterBinding** is responsible for binding a parameter to a value.</span></span> <span data-ttu-id="c9229-189">No caso de **[ModelBinder]**, o atributo retorna um **HttpParameterBinding** implementação que usa um **IModelBinder** para realizar a associação real.</span><span class="sxs-lookup"><span data-stu-id="c9229-189">In the case of **[ModelBinder]**, the attribute returns an **HttpParameterBinding** implementation that uses an **IModelBinder** to perform the actual binding.</span></span> <span data-ttu-id="c9229-190">Você também pode implementar seu próprio **HttpParameterBinding**.</span><span class="sxs-lookup"><span data-stu-id="c9229-190">You can also implement your own **HttpParameterBinding**.</span></span>

<span data-ttu-id="c9229-191">Por exemplo, suponha que você deseja obter ETags partir `if-match` e `if-none-match` cabeçalhos na solicitação.</span><span class="sxs-lookup"><span data-stu-id="c9229-191">For example, suppose you want to get ETags from `if-match` and `if-none-match` headers in the request.</span></span> <span data-ttu-id="c9229-192">Vamos começar definindo uma classe para representar os ETags.</span><span class="sxs-lookup"><span data-stu-id="c9229-192">We'll start by defining a class to represent ETags.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample19.cs)]

<span data-ttu-id="c9229-193">Também vamos definir uma enumeração para indicar se deve obter a ETag do `if-match` cabeçalho ou o `if-none-match` cabeçalho.</span><span class="sxs-lookup"><span data-stu-id="c9229-193">We'll also define an enumeration to indicate whether to get the ETag from the `if-match` header or the `if-none-match` header.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample20.cs)]

<span data-ttu-id="c9229-194">Aqui está uma **HttpParameterBinding** que obtém a ETag do cabeçalho de desejado e o associa a um parâmetro de tipo ETag:</span><span class="sxs-lookup"><span data-stu-id="c9229-194">Here is an **HttpParameterBinding** that gets the ETag from the desired header and binds it to a parameter of type ETag:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample21.cs)]

<span data-ttu-id="c9229-195">O **ExecuteBindingAsync** método faz a associação.</span><span class="sxs-lookup"><span data-stu-id="c9229-195">The **ExecuteBindingAsync** method does the binding.</span></span> <span data-ttu-id="c9229-196">Dentro desse método, adicione o valor de parâmetro associados para o **ActionArgument** dicionário na **HttpActionContext**.</span><span class="sxs-lookup"><span data-stu-id="c9229-196">Within this method, add the bound parameter value to the **ActionArgument** dictionary in the **HttpActionContext**.</span></span>

> [!NOTE]
> <span data-ttu-id="c9229-197">Se sua **ExecuteBindingAsync** método lê o corpo da mensagem de solicitação, substitua o **WillReadBody** propriedade retornar true.</span><span class="sxs-lookup"><span data-stu-id="c9229-197">If your **ExecuteBindingAsync** method reads the body of the request message, override the **WillReadBody** property to return true.</span></span> <span data-ttu-id="c9229-198">O corpo da solicitação pode ser um fluxo não armazenado em buffer que só pode ser lidos uma vez, portanto, a API da Web impõe uma regra que no máximo uma associação pode ler o corpo da mensagem.</span><span class="sxs-lookup"><span data-stu-id="c9229-198">The request body might be an unbuffered stream that can only be read once, so Web API enforces a rule that at most one binding can read the message body.</span></span>


<span data-ttu-id="c9229-199">Para aplicar um personalizado **HttpParameterBinding**, você pode definir um atributo derivado de **ParameterBindingAttribute**.</span><span class="sxs-lookup"><span data-stu-id="c9229-199">To apply a custom **HttpParameterBinding**, you can define an attribute that derives from **ParameterBindingAttribute**.</span></span> <span data-ttu-id="c9229-200">Para `ETagParameterBinding`, vamos definir dois atributos, um para `if-match` cabeçalhos e outro para `if-none-match` cabeçalhos.</span><span class="sxs-lookup"><span data-stu-id="c9229-200">For `ETagParameterBinding`, we'll define two attributes, one for `if-match` headers and one for `if-none-match` headers.</span></span> <span data-ttu-id="c9229-201">Ambos derivam de uma classe base abstrata.</span><span class="sxs-lookup"><span data-stu-id="c9229-201">Both derive from an abstract base class.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample22.cs)]

<span data-ttu-id="c9229-202">Aqui está um método controlador que usa o `[IfNoneMatch]` atributo.</span><span class="sxs-lookup"><span data-stu-id="c9229-202">Here is a controller method that uses the `[IfNoneMatch]` attribute.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample23.cs)]

<span data-ttu-id="c9229-203">Além disso **ParameterBindingAttribute**, há outro gancho para adicionar um personalizado **HttpParameterBinding**.</span><span class="sxs-lookup"><span data-stu-id="c9229-203">Besides **ParameterBindingAttribute**, there is another hook for adding a custom **HttpParameterBinding**.</span></span> <span data-ttu-id="c9229-204">Sobre o **HttpConfiguration** objeto, o **ParameterBindingRules** propriedade é uma coleção de funções de anomymous do tipo (**HttpParameterDescriptor**  - &gt; **HttpParameterBinding**).</span><span class="sxs-lookup"><span data-stu-id="c9229-204">On the **HttpConfiguration** object, the **ParameterBindingRules** property is a collection of anomymous functions of type (**HttpParameterDescriptor** -&gt; **HttpParameterBinding**).</span></span> <span data-ttu-id="c9229-205">Por exemplo, você pode adicionar uma regra que usa qualquer parâmetro de ETag em um método GET `ETagParameterBinding` com `if-none-match`:</span><span class="sxs-lookup"><span data-stu-id="c9229-205">For example, you could add a rule that any ETag parameter on a GET method uses `ETagParameterBinding` with `if-none-match`:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample24.cs)]

<span data-ttu-id="c9229-206">A função deve retornar `null` para parâmetros em que a associação não é aplicável.</span><span class="sxs-lookup"><span data-stu-id="c9229-206">The function should return `null` for parameters where the binding is not applicable.</span></span>

## <a name="iactionvaluebinder"></a><span data-ttu-id="c9229-207">IActionValueBinder</span><span class="sxs-lookup"><span data-stu-id="c9229-207">IActionValueBinder</span></span>

<span data-ttu-id="c9229-208">Todo o processo de associação de parâmetros é controlado por um serviço de conectável **IActionValueBinder**.</span><span class="sxs-lookup"><span data-stu-id="c9229-208">The entire parameter-binding process is controlled by a pluggable service, **IActionValueBinder**.</span></span> <span data-ttu-id="c9229-209">A implementação padrão de **IActionValueBinder** faz o seguinte:</span><span class="sxs-lookup"><span data-stu-id="c9229-209">The default implementation of **IActionValueBinder** does the following:</span></span>

1. <span data-ttu-id="c9229-210">Procure uma **ParameterBindingAttribute** no parâmetro.</span><span class="sxs-lookup"><span data-stu-id="c9229-210">Look for a **ParameterBindingAttribute** on the parameter.</span></span> <span data-ttu-id="c9229-211">Isso inclui **[FromBody]**, **[FromUri]**, e **[ModelBinder]**, ou atributos personalizados.</span><span class="sxs-lookup"><span data-stu-id="c9229-211">This includes **[FromBody]**, **[FromUri]**, and **[ModelBinder]**, or custom attributes.</span></span>
2. <span data-ttu-id="c9229-212">Caso contrário, examine **HttpConfiguration.ParameterBindingRules** para uma função que retorna não nulo **HttpParameterBinding**.</span><span class="sxs-lookup"><span data-stu-id="c9229-212">Otherwise, look in **HttpConfiguration.ParameterBindingRules** for a function that returns a non-null **HttpParameterBinding**.</span></span>
3. <span data-ttu-id="c9229-213">Caso contrário, use as regras padrão que descrevi anteriormente.</span><span class="sxs-lookup"><span data-stu-id="c9229-213">Otherwise, use the default rules that I described previously.</span></span> 

    - <span data-ttu-id="c9229-214">Se o tipo de parâmetro é "simple" ou um conversor de tipo, associe-se do URI.</span><span class="sxs-lookup"><span data-stu-id="c9229-214">If the parameter type is "simple"or has a type converter, bind from the URI.</span></span> <span data-ttu-id="c9229-215">Isso é equivalente a colocar o **[FromUri]** atributo no parâmetro.</span><span class="sxs-lookup"><span data-stu-id="c9229-215">This is equivalent to putting the **[FromUri]** attribute on the parameter.</span></span>
    - <span data-ttu-id="c9229-216">Caso contrário, tente ler o parâmetro do corpo da mensagem.</span><span class="sxs-lookup"><span data-stu-id="c9229-216">Otherwise, try to read the parameter from the message body.</span></span> <span data-ttu-id="c9229-217">Isso é equivalente a colocar **[FromBody]** no parâmetro.</span><span class="sxs-lookup"><span data-stu-id="c9229-217">This is equivalent to putting **[FromBody]** on the parameter.</span></span>

<span data-ttu-id="c9229-218">Se você quisesse, você poderia substituir todo o **IActionValueBinder** serviço com uma implementação personalizada.</span><span class="sxs-lookup"><span data-stu-id="c9229-218">If you wanted, you could replace the entire **IActionValueBinder** service with a custom implementation.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c9229-219">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="c9229-219">Additional Resources</span></span>

[<span data-ttu-id="c9229-220">Exemplo de associação de parâmetro personalizado</span><span class="sxs-lookup"><span data-stu-id="c9229-220">Custom Parameter Binding Sample</span></span>](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/CustomParameterBinding/ReadMe.txt)

<span data-ttu-id="c9229-221">Mike Stall escreveu uma série de BOM de postagens de blog sobre a associação de parâmetros de API da Web:</span><span class="sxs-lookup"><span data-stu-id="c9229-221">Mike Stall wrote a good series of blog posts about Web API parameter binding:</span></span>

- [<span data-ttu-id="c9229-222">Como a API da Web faz a associação de parâmetro</span><span class="sxs-lookup"><span data-stu-id="c9229-222">How Web API does Parameter Binding</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/16/how-webapi-does-parameter-binding.aspx)
- [<span data-ttu-id="c9229-223">Associação de parâmetro de estilo MVC para API da Web</span><span class="sxs-lookup"><span data-stu-id="c9229-223">MVC Style parameter binding for Web API</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/18/mvc-style-parameter-binding-for-webapi.aspx)
- [<span data-ttu-id="c9229-224">Como associar a objetos personalizados em assinaturas de ação na API da Web do MVC</span><span class="sxs-lookup"><span data-stu-id="c9229-224">How to bind to custom objects in action signatures in MVC/Web API</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx)
- [<span data-ttu-id="c9229-225">Como criar um provedor de valor personalizados na API Web</span><span class="sxs-lookup"><span data-stu-id="c9229-225">How to create a custom value provider in Web API</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx)
- [<span data-ttu-id="c9229-226">Associação de parâmetros de API da Web nos bastidores</span><span class="sxs-lookup"><span data-stu-id="c9229-226">Web API Parameter binding under the hood</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx)

---
title: "Associação de modelo personalizado"
author: ardalis
description: "Personalizando a associação de modelo no ASP.NET MVC de núcleo."
keywords: "ASP.NET Core, associação de modelo, o associador de modelo personalizado"
ms.author: riande
manager: wpickett
ms.date: 04/10/2017
ms.topic: article
ms.assetid: ebd98159-a028-4a94-b06c-43981c79c6be
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/advanced/custom-model-binding
ms.openlocfilehash: f3fc3d624c3b79d49a886dd85ca8b19147631e39
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/22/2017
---
# <a name="custom-model-binding"></a><span data-ttu-id="16cf5-104">Associação de modelo personalizado</span><span class="sxs-lookup"><span data-stu-id="16cf5-104">Custom Model Binding</span></span>

<span data-ttu-id="16cf5-105">Por [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="16cf5-105">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="16cf5-106">Associação de modelo permite que as ações do controlador trabalhar diretamente com os tipos de modelo (transmitidos como argumentos de método), em vez de solicitações HTTP.</span><span class="sxs-lookup"><span data-stu-id="16cf5-106">Model binding allows controller actions to work directly with model types (passed in as method arguments), rather than HTTP requests.</span></span> <span data-ttu-id="16cf5-107">Mapeamento entre modelos de dados e aplicativos de solicitação entrados é tratado pelo associadores de modelo.</span><span class="sxs-lookup"><span data-stu-id="16cf5-107">Mapping between incoming request data and application models is handled by model binders.</span></span> <span data-ttu-id="16cf5-108">Os desenvolvedores podem estender a funcionalidade de associação de modelo interno implementando associadores de modelo personalizado (embora normalmente, você não precisa escrever seu próprio provedor).</span><span class="sxs-lookup"><span data-stu-id="16cf5-108">Developers can extend the built-in model binding functionality by implementing custom model binders (though typically, you don't need to write your own provider).</span></span>

[<span data-ttu-id="16cf5-109">Exibir ou baixar o exemplo do GitHub</span><span class="sxs-lookup"><span data-stu-id="16cf5-109">View or download sample from GitHub</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-model-binding/)

## <a name="default-model-binder-limitations"></a><span data-ttu-id="16cf5-110">Limitações de associador de modelo padrão</span><span class="sxs-lookup"><span data-stu-id="16cf5-110">Default model binder limitations</span></span>

<span data-ttu-id="16cf5-111">Os associadores de modelo padrão dão suporte à maioria dos tipos de dados comuns do .NET Core e devem atender às necessidades da maioria dos desenvolvedores.</span><span class="sxs-lookup"><span data-stu-id="16cf5-111">The default model binders support most of the common .NET Core data types and should meet most developers\` needs.</span></span> <span data-ttu-id="16cf5-112">Esperam associar o texto de entrada da solicitação diretamente a tipos de modelo.</span><span class="sxs-lookup"><span data-stu-id="16cf5-112">They expect to bind text-based input from the request directly to model types.</span></span> <span data-ttu-id="16cf5-113">Talvez seja necessário transformar a entrada antes de associar a ele.</span><span class="sxs-lookup"><span data-stu-id="16cf5-113">You might need to transform the input prior to binding it.</span></span> <span data-ttu-id="16cf5-114">Por exemplo, se você tiver uma chave que pode ser usada para pesquisar dados de modelo.</span><span class="sxs-lookup"><span data-stu-id="16cf5-114">For example, when you have a key that can be used to look up model data.</span></span> <span data-ttu-id="16cf5-115">Você pode usar um associador de modelos personalizados para buscar dados com base na chave.</span><span class="sxs-lookup"><span data-stu-id="16cf5-115">You can use a custom model binder to fetch data based on the key.</span></span>

## <a name="model-binding-review"></a><span data-ttu-id="16cf5-116">Análise de associação de modelo</span><span class="sxs-lookup"><span data-stu-id="16cf5-116">Model binding review</span></span>

<span data-ttu-id="16cf5-117">Associação de modelo usa definições específicas para os tipos que ela opera.</span><span class="sxs-lookup"><span data-stu-id="16cf5-117">Model binding uses specific definitions for the types it operates on.</span></span> <span data-ttu-id="16cf5-118">Um *tipo simples* é convertido de uma única cadeia de caracteres na entrada.</span><span class="sxs-lookup"><span data-stu-id="16cf5-118">A *simple type* is converted from a single string in the input.</span></span> <span data-ttu-id="16cf5-119">Um *tipo complexo* é convertido de vários valores de entrada.</span><span class="sxs-lookup"><span data-stu-id="16cf5-119">A *complex type* is converted from multiple input values.</span></span> <span data-ttu-id="16cf5-120">A estrutura determina a diferença com base na existência de um `TypeConverter`.</span><span class="sxs-lookup"><span data-stu-id="16cf5-120">The framework determines the difference based on the existence of a `TypeConverter`.</span></span> <span data-ttu-id="16cf5-121">É recomendável que você criar um conversor de tipo, se você tiver um simples `string`  ->  `SomeType` mapeamento que não exige recursos externos.</span><span class="sxs-lookup"><span data-stu-id="16cf5-121">We recommended you create a type converter if you have a simple `string` -> `SomeType` mapping that doesn't require external resources.</span></span>

<span data-ttu-id="16cf5-122">Antes de criar seu próprio associador de modelo personalizado, é vale a pena modelo existente como revisão associadores são implementadas.</span><span class="sxs-lookup"><span data-stu-id="16cf5-122">Before creating your own custom model binder, it's worth reviewing how existing model binders are implemented.</span></span> <span data-ttu-id="16cf5-123">Considere o [ByteArrayModelBinder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinder) que pode ser usado para converter cadeias de caracteres codificada em base64 em matrizes de bytes.</span><span class="sxs-lookup"><span data-stu-id="16cf5-123">Consider the [ByteArrayModelBinder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinder) which can be used to convert base64-encoded strings into byte arrays.</span></span> <span data-ttu-id="16cf5-124">As matrizes de bytes geralmente são armazenadas como arquivos ou campos do banco de dados BLOB.</span><span class="sxs-lookup"><span data-stu-id="16cf5-124">The byte arrays are often stored as files or database BLOB fields.</span></span>

### <a name="working-with-the-bytearraymodelbinder"></a><span data-ttu-id="16cf5-125">Trabalhando com o ByteArrayModelBinder</span><span class="sxs-lookup"><span data-stu-id="16cf5-125">Working with the ByteArrayModelBinder</span></span>

<span data-ttu-id="16cf5-126">Cadeias de caracteres codificada em Base64 podem ser usadas para representar dados binários.</span><span class="sxs-lookup"><span data-stu-id="16cf5-126">Base64-encoded strings can be used to represent binary data.</span></span> <span data-ttu-id="16cf5-127">Por exemplo, a imagem a seguir pode ser codificada como uma cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="16cf5-127">For example, the following image can be encoded as a string.</span></span>

<span data-ttu-id="16cf5-128">![dotnet bot](custom-model-binding/images/bot.png "bot dotnet")</span><span class="sxs-lookup"><span data-stu-id="16cf5-128">![dotnet bot](custom-model-binding/images/bot.png "dotnet bot")</span></span>

<span data-ttu-id="16cf5-129">Uma pequena parte da cadeia de caracteres codificada é mostrada na imagem a seguir:</span><span class="sxs-lookup"><span data-stu-id="16cf5-129">A small portion of the encoded string is shown in the following image:</span></span>

<span data-ttu-id="16cf5-130">![dotnet bot codificado](custom-model-binding/images/encoded-bot.png "bot dotnet codificado")</span><span class="sxs-lookup"><span data-stu-id="16cf5-130">![dotnet bot encoded](custom-model-binding/images/encoded-bot.png "dotnet bot encoded")</span></span>

<span data-ttu-id="16cf5-131">Siga as instruções de [Leiame do exemplo](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/advanced/custom-model-binding/sample/CustomModelBindingSample/README.md) para converter a cadeia de caracteres codificada em base64 em um arquivo.</span><span class="sxs-lookup"><span data-stu-id="16cf5-131">Follow the instructions in the [sample's README](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/advanced/custom-model-binding/sample/CustomModelBindingSample/README.md) to convert the base64-encoded string into a file.</span></span>

<span data-ttu-id="16cf5-132">ASP.NET MVC de núcleo pode levar um cadeias de caracteres codificada em base64 e usar um `ByteArrayModelBinder` para convertê-la em uma matriz de bytes.</span><span class="sxs-lookup"><span data-stu-id="16cf5-132">ASP.NET Core MVC can take a base64-encoded strings and use a `ByteArrayModelBinder` to convert it into a byte array.</span></span> <span data-ttu-id="16cf5-133">O [ByteArrayModelBinderProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinderprovider) que implementa [IModelBinderProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.imodelbinderprovider) mapeia `byte[]` argumentos para `ByteArrayModelBinder`:</span><span class="sxs-lookup"><span data-stu-id="16cf5-133">The [ByteArrayModelBinderProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinderprovider) which implements [IModelBinderProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.imodelbinderprovider) maps `byte[]` arguments to `ByteArrayModelBinder`:</span></span>

```csharp
public IModelBinder GetBinder(ModelBinderProviderContext context)
{
    if (context == null)
    {
        throw new ArgumentNullException(nameof(context));
    }

    if (context.Metadata.ModelType == typeof(byte[]))
    {
        return new ByteArrayModelBinder();
    }

    return null;
}
```

<span data-ttu-id="16cf5-134">Ao criar seu próprio associador de modelo personalizado, você pode implementar seu próprio `IModelBinderProvider` digitar ou usar o [ModelBinderAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinderattribute).</span><span class="sxs-lookup"><span data-stu-id="16cf5-134">When creating your own custom model binder, you can implement your own `IModelBinderProvider` type, or use the [ModelBinderAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinderattribute).</span></span>

<span data-ttu-id="16cf5-135">O exemplo a seguir mostra como usar `ByteArrayModelBinder` para converter uma cadeia de caracteres codificada em base64 para um `byte[]` e salvar o resultado em um arquivo:</span><span class="sxs-lookup"><span data-stu-id="16cf5-135">The following example shows how to use `ByteArrayModelBinder` to convert a base64-encoded string to a `byte[]` and save the result to a file:</span></span>

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post1&highlight=3)]

<span data-ttu-id="16cf5-136">Você pode lançar uma cadeia de caracteres codificada em base64 para esse método de api usando uma ferramenta como [carteiro](https://www.getpostman.com/):</span><span class="sxs-lookup"><span data-stu-id="16cf5-136">You can POST a base64-encoded string to this api method using a tool like [Postman](https://www.getpostman.com/):</span></span>

<span data-ttu-id="16cf5-137">![carteiro](custom-model-binding/images/postman.png "carteiro")</span><span class="sxs-lookup"><span data-stu-id="16cf5-137">![postman](custom-model-binding/images/postman.png "postman")</span></span>

<span data-ttu-id="16cf5-138">Como o associador pode associar dados de solicitação para propriedades nomeadas adequadamente ou argumentos, associação de modelo terá êxito.</span><span class="sxs-lookup"><span data-stu-id="16cf5-138">As long as the binder can bind request data to appropriately named properties or arguments, model binding will succeed.</span></span> <span data-ttu-id="16cf5-139">O exemplo a seguir mostra como usar `ByteArrayModelBinder` com um modelo de exibição:</span><span class="sxs-lookup"><span data-stu-id="16cf5-139">The following example shows how to use `ByteArrayModelBinder` with a view model:</span></span>

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post2&highlight=2)]

## <a name="custom-model-binder-sample"></a><span data-ttu-id="16cf5-140">Exemplo de associador de modelo personalizado</span><span class="sxs-lookup"><span data-stu-id="16cf5-140">Custom model binder sample</span></span>

<span data-ttu-id="16cf5-141">Nesta seção, implementaremos um associador de modelo personalizado que:</span><span class="sxs-lookup"><span data-stu-id="16cf5-141">In this section we'll implement a custom model binder that:</span></span>

- <span data-ttu-id="16cf5-142">Converte dados de solicitação de entrada em argumentos de chave fortemente tipados.</span><span class="sxs-lookup"><span data-stu-id="16cf5-142">Converts incoming request data into strongly typed key arguments.</span></span>
- <span data-ttu-id="16cf5-143">Usa o Entity Framework Core para buscar a entidade associada.</span><span class="sxs-lookup"><span data-stu-id="16cf5-143">Uses Entity Framework Core to fetch the associated entity.</span></span>
- <span data-ttu-id="16cf5-144">Passa a entidade associada como um argumento para o método de ação.</span><span class="sxs-lookup"><span data-stu-id="16cf5-144">Passes the associated entity as an argument to the action method.</span></span>

<span data-ttu-id="16cf5-145">O exemplo a seguir usa o `ModelBinder` atributo no `Author` modelo:</span><span class="sxs-lookup"><span data-stu-id="16cf5-145">The following sample uses the `ModelBinder` attribute on the `Author` model:</span></span>

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Data/Author.cs?highlight=10)]

<span data-ttu-id="16cf5-146">No código anterior, o `ModelBinder` atributo especifica o tipo de `IModelBinder` que deve ser usado para associar `Author` parâmetros de ação.</span><span class="sxs-lookup"><span data-stu-id="16cf5-146">In the preceding code, the `ModelBinder` attribute specifies the type of `IModelBinder` that should be used to bind `Author` action parameters.</span></span> 

<span data-ttu-id="16cf5-147">O `AuthorEntityBinder` é usado para associar um `Author` parâmetro buscando a entidade de uma fonte de dados usando o Entity Framework Core e uma `authorId`:</span><span class="sxs-lookup"><span data-stu-id="16cf5-147">The `AuthorEntityBinder` is used to bind an `Author` parameter by fetching the entity from a data source using Entity Framework Core and an `authorId`:</span></span>

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinder.cs?name=demo)]

<span data-ttu-id="16cf5-148">O código a seguir mostra como usar o `AuthorEntityBinder` em um método de ação:</span><span class="sxs-lookup"><span data-stu-id="16cf5-148">The following code shows how to use the `AuthorEntityBinder` in an action method:</span></span>

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo2&highlight=2)]

<span data-ttu-id="16cf5-149">O `ModelBinder` atributo pode ser usado para aplicar o `AuthorEntityBinder` aos parâmetros que não usam convenções padrão:</span><span class="sxs-lookup"><span data-stu-id="16cf5-149">The `ModelBinder` attribute can be used to apply the `AuthorEntityBinder` to parameters that do not use default conventions:</span></span>

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo1&highlight=2)]

<span data-ttu-id="16cf5-150">Neste exemplo, como o nome do argumento não é o padrão `authorId`, ele é especificado no parâmetro usando `ModelBinder` atributo.</span><span class="sxs-lookup"><span data-stu-id="16cf5-150">In this example, since the name of the argument is not the default `authorId`, it's specified on the parameter using `ModelBinder` attribute.</span></span> <span data-ttu-id="16cf5-151">Observe que o controlador e ação de método são simplificadas comparado ao pesquisar a entidade no método de ação.</span><span class="sxs-lookup"><span data-stu-id="16cf5-151">Note that both the controller and action method are simplified compared to looking up the entity in the action method.</span></span> <span data-ttu-id="16cf5-152">A lógica para buscar o autor usando o Entity Framework Core é movida para o associador de modelo.</span><span class="sxs-lookup"><span data-stu-id="16cf5-152">The logic to fetch the author using Entity Framework Core is moved to the model binder.</span></span> <span data-ttu-id="16cf5-153">Isso pode ser considerável simplificação quando há vários métodos que ligar para o modelo de autor e podem ajudá-lo a seguir o [princípio seco](http://deviq.com/don-t-repeat-yourself/).</span><span class="sxs-lookup"><span data-stu-id="16cf5-153">This can be considerable simplification when you have several methods that bind to the author model, and can help you to follow the [DRY principle](http://deviq.com/don-t-repeat-yourself/).</span></span>

<span data-ttu-id="16cf5-154">Você pode aplicar o `ModelBinder` às propriedades de modelo individuais de atributos (como em um viewmodel) ou para parâmetros de método de ação para especificar um determinado associador de modelo ou nome de modelo para apenas esse tipo ou a ação.</span><span class="sxs-lookup"><span data-stu-id="16cf5-154">You can apply the `ModelBinder` attribute to individual model properties (such as on a viewmodel) or to action method parameters to specify a certain model binder or model name for just that type or action.</span></span>

### <a name="implementing-a-modelbinderprovider"></a><span data-ttu-id="16cf5-155">Implementando um ModelBinderProvider</span><span class="sxs-lookup"><span data-stu-id="16cf5-155">Implementing a ModelBinderProvider</span></span>

<span data-ttu-id="16cf5-156">Em vez de aplicar um atributo, você pode implementar `IModelBinderProvider`.</span><span class="sxs-lookup"><span data-stu-id="16cf5-156">Instead of applying an attribute, you can implement `IModelBinderProvider`.</span></span> <span data-ttu-id="16cf5-157">Isso é como os associadores de estrutura interna são implementados.</span><span class="sxs-lookup"><span data-stu-id="16cf5-157">This is how the built-in framework binders are implemented.</span></span> <span data-ttu-id="16cf5-158">Quando você especifica o tipo de seu fichário opera em, especifique o tipo de argumento produz, **não** a entrada seu fichário aceita.</span><span class="sxs-lookup"><span data-stu-id="16cf5-158">When you specify the type your binder operates on, you specify the type of argument it produces, **not** the input your binder accepts.</span></span> <span data-ttu-id="16cf5-159">O provedor de associador seguir funciona com o `AuthorEntityBinder`.</span><span class="sxs-lookup"><span data-stu-id="16cf5-159">The following binder provider works with the `AuthorEntityBinder`.</span></span> <span data-ttu-id="16cf5-160">Quando ele é adicionado à coleção do MVC de provedores, você não precisa usar o `ModelBinder` atributo no `Author` ou `Author` parâmetros digitados.</span><span class="sxs-lookup"><span data-stu-id="16cf5-160">When it's added to MVC's collection of providers, you don't need to use the `ModelBinder` attribute on `Author` or `Author` typed parameters.</span></span>

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinderProvider.cs?highlight=17-20)]

> <span data-ttu-id="16cf5-161">Observação: O código anterior retorna um `BinderTypeModelBinder`.</span><span class="sxs-lookup"><span data-stu-id="16cf5-161">Note: The preceding code returns a `BinderTypeModelBinder`.</span></span> <span data-ttu-id="16cf5-162">`BinderTypeModelBinder`funciona como uma fábrica de associadores de modelo e fornece a injeção de dependência (DI).</span><span class="sxs-lookup"><span data-stu-id="16cf5-162">`BinderTypeModelBinder` acts as a factory for model binders and provides dependency injection (DI).</span></span> <span data-ttu-id="16cf5-163">O `AuthorEntityBinder` requer DI acessem EF Core.</span><span class="sxs-lookup"><span data-stu-id="16cf5-163">The `AuthorEntityBinder` requires DI to access EF Core.</span></span> <span data-ttu-id="16cf5-164">Use `BinderTypeModelBinder` se o associador de modelo requer serviços de injeção de dependência.</span><span class="sxs-lookup"><span data-stu-id="16cf5-164">Use `BinderTypeModelBinder` if your model binder requires services from DI.</span></span>

<span data-ttu-id="16cf5-165">Para usar um provedor de associador de modelo personalizado, adicione-o em `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="16cf5-165">To use a custom model binder provider, add it in `ConfigureServices`:</span></span>

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]

<span data-ttu-id="16cf5-166">Ao avaliar os associadores de modelo, a coleção de provedores é examinada em ordem.</span><span class="sxs-lookup"><span data-stu-id="16cf5-166">When evaluating model binders, the collection of providers is examined in order.</span></span> <span data-ttu-id="16cf5-167">O primeiro provedor que retorna um associador é usado.</span><span class="sxs-lookup"><span data-stu-id="16cf5-167">The first provider that returns a binder is used.</span></span>

<span data-ttu-id="16cf5-168">A imagem a seguir mostra o padrão de associadores de modelo do depurador.</span><span class="sxs-lookup"><span data-stu-id="16cf5-168">The following image shows the default model binders from the debugger.</span></span>

<span data-ttu-id="16cf5-169">![padrão de associadores de modelo](custom-model-binding/images/default-model-binders.png "padrão associadores de modelo")</span><span class="sxs-lookup"><span data-stu-id="16cf5-169">![default model binders](custom-model-binding/images/default-model-binders.png "default model binders")</span></span>

<span data-ttu-id="16cf5-170">Adicionar o provedor ao final da coleção pode resultar em um associador de modelos internos que está sendo chamado antes de seu fichário personalizado tem uma possibilidade.</span><span class="sxs-lookup"><span data-stu-id="16cf5-170">Adding your provider to the end of the collection may result in a built-in model binder being called before your custom binder has a chance.</span></span> <span data-ttu-id="16cf5-171">Neste exemplo, o provedor personalizado é adicionado ao início da coleção para garantir que ele é usado para `Author` argumentos de ação.</span><span class="sxs-lookup"><span data-stu-id="16cf5-171">In this example, the custom provider is added to the beginning of the collection to ensure it is used for `Author` action arguments.</span></span>

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]

## <a name="recommendations-and-best-practices"></a><span data-ttu-id="16cf5-172">Recomendações e práticas recomendadas</span><span class="sxs-lookup"><span data-stu-id="16cf5-172">Recommendations and best practices</span></span>

<span data-ttu-id="16cf5-173">Associadores de modelo personalizado:</span><span class="sxs-lookup"><span data-stu-id="16cf5-173">Custom model binders:</span></span>
- <span data-ttu-id="16cf5-174">Não tente definir códigos de status ou retornar resultados (por exemplo, 404 não encontrado).</span><span class="sxs-lookup"><span data-stu-id="16cf5-174">Should not attempt to set status codes or return results (for example, 404 Not Found).</span></span> <span data-ttu-id="16cf5-175">Se a associação de modelo falhar, um [filtro de ação](xref:mvc/controllers/filters) ou lógica dentro do método de ação deve lidar com falhas.</span><span class="sxs-lookup"><span data-stu-id="16cf5-175">If model binding fails, an [action filter](xref:mvc/controllers/filters) or logic within the action method itself should handle the failure.</span></span>
- <span data-ttu-id="16cf5-176">São mais úteis para eliminar código repetitivo e resolvem preocupações de métodos de ação.</span><span class="sxs-lookup"><span data-stu-id="16cf5-176">Are most useful for eliminating repetitive code and cross-cutting concerns from action methods.</span></span>
- <span data-ttu-id="16cf5-177">Normalmente não deve ser usado para converter uma cadeia de caracteres em um tipo personalizado, uma [ `TypeConverter` ](https://docs.microsoft.com//dotnet/api/system.componentmodel.typeconverter) geralmente é uma opção melhor.</span><span class="sxs-lookup"><span data-stu-id="16cf5-177">Typically should not be used to convert a string into a custom type, a [`TypeConverter`](https://docs.microsoft.com//dotnet/api/system.componentmodel.typeconverter) is usually a better option.</span></span>

---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
title: "Parte 2: Criando modelos de domínio | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/03/2012
ms.topic: article
ms.assetid: fe3ef85f-bdc6-4e10-9768-25aa565c01d0
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
msc.type: authoredcontent
ms.openlocfilehash: 5d4c7d7d02ced5a99db5b59f9e2e1adf6588208a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="part-2-creating-the-domain-models"></a><span data-ttu-id="ba4a5-102">Parte 2: Criando modelos de domínio</span><span class="sxs-lookup"><span data-stu-id="ba4a5-102">Part 2: Creating the Domain Models</span></span>
====================
<span data-ttu-id="ba4a5-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="ba4a5-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="ba4a5-104">Baixe o projeto concluído</span><span class="sxs-lookup"><span data-stu-id="ba4a5-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-models"></a><span data-ttu-id="ba4a5-105">Adicionar modelos</span><span class="sxs-lookup"><span data-stu-id="ba4a5-105">Add Models</span></span>

<span data-ttu-id="ba4a5-106">Há três maneiras de abordagem do Entity Framework:</span><span class="sxs-lookup"><span data-stu-id="ba4a5-106">There are three ways to approach Entity Framework:</span></span>

- <span data-ttu-id="ba4a5-107">Banco de dados, primeiro: iniciar com um banco de dados e gera o código de Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="ba4a5-107">Database-first: You start with a database, and Entity Framework generates the code.</span></span>
- <span data-ttu-id="ba4a5-108">Modelo primeiro: iniciar com um modelo de visual, e o Entity Framework gera o banco de dados e o código.</span><span class="sxs-lookup"><span data-stu-id="ba4a5-108">Model-first: You start with a visual model, and Entity Framework generates both the database and code.</span></span>
- <span data-ttu-id="ba4a5-109">Código primeiro: iniciar com o código e o Entity Framework gera o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="ba4a5-109">Code-first: You start with code, and Entity Framework generates the database.</span></span>

<span data-ttu-id="ba4a5-110">Estamos usando a abordagem de código, portanto começamos definindo nossos objetos de domínio como POCOs (objetos CLR antigo simples).</span><span class="sxs-lookup"><span data-stu-id="ba4a5-110">We are using the code-first approach, so we start by defining our domain objects as POCOs (plain-old CLR objects).</span></span> <span data-ttu-id="ba4a5-111">Com a abordagem de código, objetos de domínio não é necessário nenhum código adicional para dar suporte a camada de banco de dados, como transações ou persistência.</span><span class="sxs-lookup"><span data-stu-id="ba4a5-111">With the code-first approach, domain objects don't need any extra code to support the database layer, such as transactions or persistence.</span></span> <span data-ttu-id="ba4a5-112">(Especificamente, eles não precisa herdar o [EntityObject](https://msdn.microsoft.com/en-us/library/system.data.objects.dataclasses.entityobject.aspx) classe.) Você ainda pode usar anotações de dados para controlar como o Entity Framework cria o esquema de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="ba4a5-112">(Specifically, they do not need to inherit from the [EntityObject](https://msdn.microsoft.com/en-us/library/system.data.objects.dataclasses.entityobject.aspx) class.) You can still use data annotations to control how Entity Framework creates the database schema.</span></span>

<span data-ttu-id="ba4a5-113">Porque o POCOs não contém quaisquer propriedades adicionais que descrevem [estado do banco de dados](https://msdn.microsoft.com/en-us/library/system.data.entitystate.aspx), facilmente pode ser serializados para JSON ou XML.</span><span class="sxs-lookup"><span data-stu-id="ba4a5-113">Because POCOs do not carry any extra properties that describe [database state](https://msdn.microsoft.com/en-us/library/system.data.entitystate.aspx), they can easily be serialized to JSON or XML.</span></span> <span data-ttu-id="ba4a5-114">No entanto, isso não significa você sempre deve expor seus modelos do Entity Framework diretamente aos clientes, como você verá posteriormente no tutorial.</span><span class="sxs-lookup"><span data-stu-id="ba4a5-114">However, that does not mean you should always expose your Entity Framework models directly to clients, as we'll see later in the tutorial.</span></span>

<span data-ttu-id="ba4a5-115">Vamos criar POCOs a seguir:</span><span class="sxs-lookup"><span data-stu-id="ba4a5-115">We will create the following POCOs:</span></span>

- <span data-ttu-id="ba4a5-116">Produto</span><span class="sxs-lookup"><span data-stu-id="ba4a5-116">Product</span></span>
- <span data-ttu-id="ba4a5-117">Pedido</span><span class="sxs-lookup"><span data-stu-id="ba4a5-117">Order</span></span>
- <span data-ttu-id="ba4a5-118">OrderDetail</span><span class="sxs-lookup"><span data-stu-id="ba4a5-118">OrderDetail</span></span>

<span data-ttu-id="ba4a5-119">Para criar cada classe, clique na pasta de modelos no Gerenciador de soluções.</span><span class="sxs-lookup"><span data-stu-id="ba4a5-119">To create each class, right-click the Models folder in Solution Explorer.</span></span> <span data-ttu-id="ba4a5-120">No menu de contexto, selecione **adicionar** e, em seguida, selecione **classe.**</span><span class="sxs-lookup"><span data-stu-id="ba4a5-120">From the context menu, select **Add** and then select **Class.**</span></span>

![](using-web-api-with-entity-framework-part-2/_static/image1.png)

<span data-ttu-id="ba4a5-121">Adicionar um `Product` classe com a implementação a seguir:</span><span class="sxs-lookup"><span data-stu-id="ba4a5-121">Add a `Product` class with the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample1.cs)]

<span data-ttu-id="ba4a5-122">Por convenção, o Entity Framework usa o `Id` a propriedade como a chave primária e mapeia para uma coluna de identidade na tabela de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="ba4a5-122">By convention, Entity Framework uses the `Id` property as the primary key and maps it to an identity column in the database table.</span></span> <span data-ttu-id="ba4a5-123">Quando você cria um novo `Product` instância, você não definir um valor para `Id`, porque o banco de dados gera o valor.</span><span class="sxs-lookup"><span data-stu-id="ba4a5-123">When you create a new `Product` instance, you won't set a value for `Id`, because the database generates the value.</span></span>

<span data-ttu-id="ba4a5-124">O **ScaffoldColumn** atributo informa ao ASP.NET MVC para ignorar o `Id` ao gerar um formulário do editor de propriedade.</span><span class="sxs-lookup"><span data-stu-id="ba4a5-124">The **ScaffoldColumn** attribute tells ASP.NET MVC to skip the `Id` property when generating an editor form.</span></span> <span data-ttu-id="ba4a5-125">O **necessário** atributo é usado para validar o modelo.</span><span class="sxs-lookup"><span data-stu-id="ba4a5-125">The **Required** attribute is used to validate the model.</span></span> <span data-ttu-id="ba4a5-126">Especifica que o `Name` propriedade deve ser uma cadeia de caracteres não vazia.</span><span class="sxs-lookup"><span data-stu-id="ba4a5-126">It specifies that the `Name` property must be a non-empty string.</span></span>

<span data-ttu-id="ba4a5-127">Adicionar o `Order` classe:</span><span class="sxs-lookup"><span data-stu-id="ba4a5-127">Add the `Order` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample2.cs)]

<span data-ttu-id="ba4a5-128">Adicionar o `OrderDetail` classe:</span><span class="sxs-lookup"><span data-stu-id="ba4a5-128">Add the `OrderDetail` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample3.cs)]

## <a name="foreign-key-relations"></a><span data-ttu-id="ba4a5-129">Relações de chave estrangeiras</span><span class="sxs-lookup"><span data-stu-id="ba4a5-129">Foreign Key Relations</span></span>

<span data-ttu-id="ba4a5-130">Um pedido contém muitos detalhes do pedido e cada detalhe do pedido se refere a um único produto.</span><span class="sxs-lookup"><span data-stu-id="ba4a5-130">An order contains many order details, and each order detail refers to a single product.</span></span> <span data-ttu-id="ba4a5-131">Para representar essas relações, o `OrderDetail` classe define propriedades chamadas `OrderId` e `ProductId`.</span><span class="sxs-lookup"><span data-stu-id="ba4a5-131">To represent these relations, the `OrderDetail` class defines properties named `OrderId` and `ProductId`.</span></span> <span data-ttu-id="ba4a5-132">Entity Framework deduzirá que essas propriedades representam as chaves estrangeiras e adicionará as restrições foreign key no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="ba4a5-132">Entity Framework will infer that these properties represent foreign keys, and will add foreign-key constraints to the database.</span></span>

![](using-web-api-with-entity-framework-part-2/_static/image2.png)

<span data-ttu-id="ba4a5-133">O `Order` e `OrderDetail` classes também incluem propriedades de "navegação", que contêm referências a objetos relacionados.</span><span class="sxs-lookup"><span data-stu-id="ba4a5-133">The `Order` and `OrderDetail` classes also include "navigation" properties, which contain references to the related objects.</span></span> <span data-ttu-id="ba4a5-134">Dada uma ordem, você pode navegar para os produtos na ordem, seguindo as propriedades de navegação.</span><span class="sxs-lookup"><span data-stu-id="ba4a5-134">Given an order, you can navigate to the products in the order by following the navigation properties.</span></span>

<span data-ttu-id="ba4a5-135">Compile o projeto agora.</span><span class="sxs-lookup"><span data-stu-id="ba4a5-135">Compile the project now.</span></span> <span data-ttu-id="ba4a5-136">Entity Framework usa reflexão para descobrir as propriedades dos modelos, isso requer um assembly compilado criar o esquema de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="ba4a5-136">Entity Framework uses reflection to discover the properties of the models, so it requires a compiled assembly to create the database schema.</span></span>

## <a name="configure-the-media-type-formatters"></a><span data-ttu-id="ba4a5-137">Configurar os formatadores de tipo de mídia</span><span class="sxs-lookup"><span data-stu-id="ba4a5-137">Configure the Media-Type Formatters</span></span>

<span data-ttu-id="ba4a5-138">Um [formatador do tipo de mídia](../../formats-and-model-binding/media-formatters.md) é um objeto que serializa os dados quando a API da Web grava o corpo da resposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="ba4a5-138">A [media-type formatter](../../formats-and-model-binding/media-formatters.md) is an object that serializes your data when Web API writes the HTTP response body.</span></span> <span data-ttu-id="ba4a5-139">Os formatadores internos oferecem suporte a saída JSON e XML.</span><span class="sxs-lookup"><span data-stu-id="ba4a5-139">The built-in formatters support JSON and XML output.</span></span> <span data-ttu-id="ba4a5-140">Por padrão, ambos esses formatadores serializar todos os objetos por valor.</span><span class="sxs-lookup"><span data-stu-id="ba4a5-140">By default, both of these formatters serialize all objects by value.</span></span>

<span data-ttu-id="ba4a5-141">Serialização por valor cria um problema se um gráfico de objeto contém referências circulares.</span><span class="sxs-lookup"><span data-stu-id="ba4a5-141">Serialization by value creates a problem if an object graph contains circular references.</span></span> <span data-ttu-id="ba4a5-142">Esse é exatamente o caso com o `Order` e `OrderDetail` classes, porque cada uma contém uma referência para o outro.</span><span class="sxs-lookup"><span data-stu-id="ba4a5-142">That's exactly the case with the `Order` and `OrderDetail` classes, because each holds a reference to the other.</span></span> <span data-ttu-id="ba4a5-143">O formatador siga as referências, writing cada objeto de valor e ir em círculos.</span><span class="sxs-lookup"><span data-stu-id="ba4a5-143">The formatter will follow the references, writing each object by value, and go in circles.</span></span> <span data-ttu-id="ba4a5-144">Portanto, é preciso alterar o comportamento padrão.</span><span class="sxs-lookup"><span data-stu-id="ba4a5-144">Therefore, we need to change the default behavior.</span></span>

<span data-ttu-id="ba4a5-145">No Gerenciador de soluções, expanda o aplicativo\_pasta e abra o arquivo chamado WebApiConfig.cs.</span><span class="sxs-lookup"><span data-stu-id="ba4a5-145">In Solution Explorer, expand the App\_Start folder and open the file named WebApiConfig.cs.</span></span> <span data-ttu-id="ba4a5-146">Adicione o seguinte código à classe `WebApiConfig`:</span><span class="sxs-lookup"><span data-stu-id="ba4a5-146">Add the following code to the `WebApiConfig` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample4.cs?highlight=11)]

<span data-ttu-id="ba4a5-147">Esse código define o formatador JSON para preservar as referências de objeto e remove o formatador XML do pipeline inteiramente.</span><span class="sxs-lookup"><span data-stu-id="ba4a5-147">This code sets the JSON formatter to preserve object references, and removes the XML formatter from the pipeline entirely.</span></span> <span data-ttu-id="ba4a5-148">(Você pode configurar o formatador XML para preservar as referências de objeto, mas é um pouco mais trabalho e precisamos apenas JSON para este aplicativo.</span><span class="sxs-lookup"><span data-stu-id="ba4a5-148">(You can configure the XML formatter to preserve object references, but it's a little more work, and we only need JSON for this application.</span></span> <span data-ttu-id="ba4a5-149">Para obter mais informações, consulte [tratamento de referências de objeto Circular](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).)</span><span class="sxs-lookup"><span data-stu-id="ba4a5-149">For more information, see [Handling Circular Object References](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).)</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="ba4a5-150">[Anterior](using-web-api-with-entity-framework-part-1.md)
[Próximo](using-web-api-with-entity-framework-part-3.md)</span><span class="sxs-lookup"><span data-stu-id="ba4a5-150">[Previous](using-web-api-with-entity-framework-part-1.md)
[Next](using-web-api-with-entity-framework-part-3.md)</span></span>

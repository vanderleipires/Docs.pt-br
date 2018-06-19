---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
title: Herança de tipo complexo no OData v4 com o ASP.NET Web API | Microsoft Docs
author: microsoft
description: Acordo com a especificação do OData v4, um tipo complexo pode herdar de outro tipo complexo. (Um tipo complexo é um tipo estruturado sem uma chave.) API da Web...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/16/2014
ms.topic: article
ms.assetid: a00d3600-9c2a-41bc-9460-06cc527904e2
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: be2dbfa82b99b6c48928e4e767716852c14a463b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
ms.locfileid: "26508415"
---
<a name="complex-type-inheritance-in-odata-v4-with-aspnet-web-api"></a><span data-ttu-id="77846-104">Herança de tipo complexo no v4 do OData com a API da Web do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="77846-104">Complex Type Inheritance in OData v4 with ASP.NET Web API</span></span>
====================
<span data-ttu-id="77846-105">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="77846-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="77846-106">Acordo com o OData v4 [especificação](http://www.odata.org/documentation/odata-version-4-0/), um tipo complexo pode herdar de outro tipo complexo.</span><span class="sxs-lookup"><span data-stu-id="77846-106">According to the OData v4 [specification](http://www.odata.org/documentation/odata-version-4-0/), a complex type can inherit from another complex type.</span></span> <span data-ttu-id="77846-107">(Um *complexo* é um tipo estruturado sem uma chave.) API da Web OData 5.3 dá suporte a herança de tipo complexo.</span><span class="sxs-lookup"><span data-stu-id="77846-107">(A *complex* type is a structured type without a key.) Web API OData 5.3 supports complex type inheritance.</span></span>
> 
> <span data-ttu-id="77846-108">Este tópico mostra como criar um modelo de dados de entidade (EDM) com tipos complexos de herança.</span><span class="sxs-lookup"><span data-stu-id="77846-108">This topic shows how to build an entity data model (EDM) with complex inheritance types.</span></span> <span data-ttu-id="77846-109">Para o código-fonte completo, consulte [exemplo de herança de tipo complexo do OData](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).</span><span class="sxs-lookup"><span data-stu-id="77846-109">For the complete source code, see [OData Complex Type Inheritance Sample](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="77846-110">Versões de software usadas no tutorial</span><span class="sxs-lookup"><span data-stu-id="77846-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="77846-111">OData da API Web 5.3</span><span class="sxs-lookup"><span data-stu-id="77846-111">Web API OData 5.3</span></span>
> - <span data-ttu-id="77846-112">OData v4</span><span class="sxs-lookup"><span data-stu-id="77846-112">OData v4</span></span>


## <a name="model-hierarchy"></a><span data-ttu-id="77846-113">Hierarquia de modelo</span><span class="sxs-lookup"><span data-stu-id="77846-113">Model Hierarchy</span></span>

<span data-ttu-id="77846-114">Para ilustrar a herança de tipo complexo, vamos usar a seguinte hierarquia de classe.</span><span class="sxs-lookup"><span data-stu-id="77846-114">To illustrate complex type inheritance, we'll use the following class hierarchy.</span></span>

![](complex-type-inheritance-in-odata-v4/_static/image1.png)

<span data-ttu-id="77846-115">`Shape`é um tipo complexo abstrato.</span><span class="sxs-lookup"><span data-stu-id="77846-115">`Shape` is an abstract complex type.</span></span> <span data-ttu-id="77846-116">`Rectangle`, `Triangle`, e `Circle` são tipos complexos derivados de `Shape`, e `RoundRectangle` deriva de `Rectangle`.</span><span class="sxs-lookup"><span data-stu-id="77846-116">`Rectangle`, `Triangle`, and `Circle` are complex types derived from `Shape`, and `RoundRectangle` derives from `Rectangle`.</span></span> <span data-ttu-id="77846-117">`Window`é um tipo de entidade e contém um `Shape` instância.</span><span class="sxs-lookup"><span data-stu-id="77846-117">`Window` is an entity type and contains a `Shape` instance.</span></span>

<span data-ttu-id="77846-118">Aqui estão as classes CLR que definem esses tipos.</span><span class="sxs-lookup"><span data-stu-id="77846-118">Here are the CLR classes that define these types.</span></span>

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample1.cs)]

## <a name="build-the-edm-model"></a><span data-ttu-id="77846-119">Criar o modelo EDM</span><span class="sxs-lookup"><span data-stu-id="77846-119">Build the EDM Model</span></span>

<span data-ttu-id="77846-120">Para criar o EDM, você pode usar **ODataConventionModelBuilder**, qual infere as relações de herança dos tipos de CLR.</span><span class="sxs-lookup"><span data-stu-id="77846-120">To create the EDM, you can use **ODataConventionModelBuilder**, which infers the inheritance relationships from the CLR types.</span></span>

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample2.cs)]

<span data-ttu-id="77846-121">Você também pode criar EDM explicitamente, usando **ODataModelBuilder**.</span><span class="sxs-lookup"><span data-stu-id="77846-121">You can also build the EDM explicitly, using **ODataModelBuilder**.</span></span> <span data-ttu-id="77846-122">Isso leva mais código, mas oferece mais controle sobre o EDM.</span><span class="sxs-lookup"><span data-stu-id="77846-122">This takes more code, but gives you more control over the EDM.</span></span>

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample3.cs)]

<span data-ttu-id="77846-123">Estes dois exemplos criam o mesmo esquema EDM.</span><span class="sxs-lookup"><span data-stu-id="77846-123">These two examples create the same EDM schema.</span></span>

## <a name="metadata-document"></a><span data-ttu-id="77846-124">Documento de metadados</span><span class="sxs-lookup"><span data-stu-id="77846-124">Metadata Document</span></span>

<span data-ttu-id="77846-125">Aqui está o documento de metadados OData, mostrando a herança de tipo complexo.</span><span class="sxs-lookup"><span data-stu-id="77846-125">Here is the OData metadata document, showing complex type inheritance.</span></span>

[!code-xml[Main](complex-type-inheritance-in-odata-v4/samples/sample4.xml?highlight=13,17,25,30)]

<span data-ttu-id="77846-126">O documento de metadados, você pode ver que:</span><span class="sxs-lookup"><span data-stu-id="77846-126">From the metadata document, you can see that:</span></span>

- <span data-ttu-id="77846-127">O `Shape` tipo complexo é abstrato.</span><span class="sxs-lookup"><span data-stu-id="77846-127">The `Shape` complex type is abstract.</span></span>
- <span data-ttu-id="77846-128">O `Rectangle`, `Triangle`, e `Circle` tipo complexo de ter o tipo base `Shape`.</span><span class="sxs-lookup"><span data-stu-id="77846-128">The `Rectangle`, `Triangle`, and `Circle` complex type have the base type `Shape`.</span></span>
- <span data-ttu-id="77846-129">O `RoundRectangle` tipo tem o tipo base `Rectangle`.</span><span class="sxs-lookup"><span data-stu-id="77846-129">The `RoundRectangle` type has the base type `Rectangle`.</span></span>

## <a name="casting-complex-types"></a><span data-ttu-id="77846-130">Conversão de tipos complexos</span><span class="sxs-lookup"><span data-stu-id="77846-130">Casting Complex Types</span></span>

<span data-ttu-id="77846-131">Agora há suporte para a conversão em tipos complexos.</span><span class="sxs-lookup"><span data-stu-id="77846-131">Casting on complex types is now supported.</span></span> <span data-ttu-id="77846-132">Por exemplo, a consulta a seguir conversões um `Shape` para um `Rectangle`.</span><span class="sxs-lookup"><span data-stu-id="77846-132">For example, the following query casts a `Shape` to a `Rectangle`.</span></span>

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample5.cmd)]

<span data-ttu-id="77846-133">Aqui está a carga de resposta:</span><span class="sxs-lookup"><span data-stu-id="77846-133">Here's the response payload:</span></span>

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample6.cmd)]

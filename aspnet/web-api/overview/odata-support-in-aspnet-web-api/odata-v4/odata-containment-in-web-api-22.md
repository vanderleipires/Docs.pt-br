---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
title: "Contenção em OData v4 usando Web API 2.2 | Microsoft Docs"
author: rick-anderson
description: "Tradicionalmente, uma entidade pode ser acessada apenas se ele foi encapsulado dentro de um conjunto de entidades. Mas o OData v4 fornece duas opções adicionais, Singleton e Con..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/27/2014
ms.topic: article
ms.assetid: 5fbfefad-a17a-4c46-8646-f1ccd154cd56
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 7d3c81bf3d2a43faa3e71155637e031f81143782
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="containment-in-odata-v4-using-web-api-22"></a><span data-ttu-id="da014-104">Contenção em OData v4 usando 2.2 da API da Web</span><span class="sxs-lookup"><span data-stu-id="da014-104">Containment in OData v4 Using Web API 2.2</span></span>
====================
<span data-ttu-id="da014-105">por Jinfu Tan</span><span class="sxs-lookup"><span data-stu-id="da014-105">by Jinfu Tan</span></span>

> <span data-ttu-id="da014-106">Tradicionalmente, uma entidade pode ser acessada apenas se ele foi encapsulado dentro de um conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="da014-106">Traditionally, an entity could only be accessed if it were encapsulated inside an entity set.</span></span> <span data-ttu-id="da014-107">Mas o OData v4 fornece duas opções adicionais, Singleton e contenção, que suporta WebAPI 2.2.</span><span class="sxs-lookup"><span data-stu-id="da014-107">But OData v4 provides two additional options, Singleton and Containment, both of which WebAPI 2.2 supports.</span></span>


<span data-ttu-id="da014-108">Este tópico mostra como definir um conjunto de contenção em um ponto de extremidade OData nas WebApi 2.2.</span><span class="sxs-lookup"><span data-stu-id="da014-108">This topic shows how to define a containment in an OData endpoint in WebApi 2.2.</span></span> <span data-ttu-id="da014-109">Para obter mais informações sobre contenção, consulte [contenção vem com o OData v4](https://blogs.msdn.com/b/odatateam/archive/2014/03/13/containment-is-coming-with-odata-v4.aspx).</span><span class="sxs-lookup"><span data-stu-id="da014-109">For more information about containment, see [Containment is coming with OData v4](https://blogs.msdn.com/b/odatateam/archive/2014/03/13/containment-is-coming-with-odata-v4.aspx).</span></span> <span data-ttu-id="da014-110">Para criar um ponto de extremidade OData V4 na API da Web, consulte [criar uma instalação do OData v4 ponto de extremidade usando ASP.NET Web API 2.2](create-an-odata-v4-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="da014-110">To create an OData V4 endpoint in Web API, see [Create an OData v4 Endpoint Using ASP.NET Web API 2.2](create-an-odata-v4-endpoint.md).</span></span>

<span data-ttu-id="da014-111">Primeiro, vamos criar um modelo de domínio de contenção no serviço do OData, usando esse modelo de dados:</span><span class="sxs-lookup"><span data-stu-id="da014-111">First, we'll create a containment domain model in the OData service, using this data model:</span></span>

![Modelo de dados](odata-containment-in-web-api-22/_static/image1.png)

<span data-ttu-id="da014-113">Uma conta contém muitos PaymentInstruments PI (), mas não definem uma conjunto de entidades para um PI.</span><span class="sxs-lookup"><span data-stu-id="da014-113">An account contains many PaymentInstruments (PI), but we don't define an entity set for a PI.</span></span> <span data-ttu-id="da014-114">Em vez disso, os PIs só podem ser acessados através de uma conta.</span><span class="sxs-lookup"><span data-stu-id="da014-114">Instead, the PIs can only be accessed through an Account.</span></span>

<span data-ttu-id="da014-115">Você pode baixar a solução usada neste tópico de [CodePlex](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/).</span><span class="sxs-lookup"><span data-stu-id="da014-115">You can download the solution used in this topic from [CodePlex](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/).</span></span>

## <a name="defining-the-data-model"></a><span data-ttu-id="da014-116">Definindo o modelo de dados</span><span class="sxs-lookup"><span data-stu-id="da014-116">Defining the data model</span></span>

1. <span data-ttu-id="da014-117">Defina os tipos CLR.</span><span class="sxs-lookup"><span data-stu-id="da014-117">Define the CLR types.</span></span>

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample1.cs)]

    <span data-ttu-id="da014-118">O `Contained` atributo é usado para propriedades de navegação de contenção.</span><span class="sxs-lookup"><span data-stu-id="da014-118">The `Contained` attribute is used for containment navigation properties.</span></span>
2. <span data-ttu-id="da014-119">Gere o modelo EDM com base nos tipos de CLR.</span><span class="sxs-lookup"><span data-stu-id="da014-119">Generate the EDM model based on the CLR types.</span></span>

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample2.cs)]

    <span data-ttu-id="da014-120">O `ODataConventionModelBuilder` tratará a criação do modelo EDM se o `Contained` atributo é adicionado à propriedade de navegação correspondente.</span><span class="sxs-lookup"><span data-stu-id="da014-120">The `ODataConventionModelBuilder` will handle building the EDM model if the `Contained` attribute is added to the corresponding navigation property.</span></span> <span data-ttu-id="da014-121">Se a propriedade é um tipo de coleção, um `GetCount(string NameContains)` função também será criada.</span><span class="sxs-lookup"><span data-stu-id="da014-121">If the property is a collection type, a `GetCount(string NameContains)` function will also be created.</span></span>

    <span data-ttu-id="da014-122">Os metadados gerado serão a seguinte aparência:</span><span class="sxs-lookup"><span data-stu-id="da014-122">The generated metadata will look like the following:</span></span>

    [!code-xml[Main](odata-containment-in-web-api-22/samples/sample3.xml?highlight=10)]

    <span data-ttu-id="da014-123">O `ContainsTarget` atributo indica que a propriedade de navegação é um conjunto de contenção.</span><span class="sxs-lookup"><span data-stu-id="da014-123">The `ContainsTarget` attribute indicates that the navigation property is a containment.</span></span>

## <a name="define-the-containing-entity-set-controller"></a><span data-ttu-id="da014-124">Defina o controlador de conjunto de entidade que contém</span><span class="sxs-lookup"><span data-stu-id="da014-124">Define the containing entity set controller</span></span>

<span data-ttu-id="da014-125">Entidades independentes não tem seu próprio controlador; a ação é definida no controlador de conjunto de entidade que contém.</span><span class="sxs-lookup"><span data-stu-id="da014-125">Contained entities don't have their own controller; the action is defined in the containing entity set controller.</span></span> <span data-ttu-id="da014-126">Neste exemplo, há um AccountsController, mas não PaymentInstrumentsController.</span><span class="sxs-lookup"><span data-stu-id="da014-126">In this sample, there is an AccountsController, but no PaymentInstrumentsController.</span></span>

[!code-csharp[Main](odata-containment-in-web-api-22/samples/sample4.cs)]

<span data-ttu-id="da014-127">Se o caminho OData é 4 ou mais segmentos, atributo somente roteamento funciona, como `[ODataRoute("Accounts({accountId})/PayinPIs({paymentInstrumentId})")]` no controlador acima.</span><span class="sxs-lookup"><span data-stu-id="da014-127">If the OData path is 4 or more segments, only attribute routing works, such as `[ODataRoute("Accounts({accountId})/PayinPIs({paymentInstrumentId})")]` in the above controller.</span></span> <span data-ttu-id="da014-128">Caso contrário, o atributo e o roteamento convencional funciona: por exemplo, `GetPayInPIs(int key)` corresponde `GET ~/Accounts(1)/PayinPIs`.</span><span class="sxs-lookup"><span data-stu-id="da014-128">Otherwise, both attribute and conventional routing works: for instance, `GetPayInPIs(int key)` matches `GET ~/Accounts(1)/PayinPIs`.</span></span>

<span data-ttu-id="da014-129">*Agradecimentos Leo Hu para o conteúdo original deste artigo.*</span><span class="sxs-lookup"><span data-stu-id="da014-129">*Thanks to Leo Hu for the original content of this article.*</span></span>

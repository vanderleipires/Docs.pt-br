---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
title: Contenção no OData v4 usando a API Web 2.2 | Microsoft Docs
author: rick-anderson
description: Tradicionalmente, uma entidade pode ser acessada somente se ele foi encapsulado dentro de um conjunto de entidades. Mas o OData v4 fornece duas opções adicionais, Singleton e Con...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/27/2014
ms.topic: article
ms.assetid: 5fbfefad-a17a-4c46-8646-f1ccd154cd56
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 33ff49f69d70dd3a8179445d2895c418d2185e49
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37365601"
---
<a name="containment-in-odata-v4-using-web-api-22"></a><span data-ttu-id="6d9df-104">Contenção no OData v4 usando a API Web 2.2</span><span class="sxs-lookup"><span data-stu-id="6d9df-104">Containment in OData v4 Using Web API 2.2</span></span>
====================
<span data-ttu-id="6d9df-105">por Jinfu Tan</span><span class="sxs-lookup"><span data-stu-id="6d9df-105">by Jinfu Tan</span></span>

> <span data-ttu-id="6d9df-106">Tradicionalmente, uma entidade pode ser acessada somente se ele foi encapsulado dentro de um conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="6d9df-106">Traditionally, an entity could only be accessed if it were encapsulated inside an entity set.</span></span> <span data-ttu-id="6d9df-107">Mas o OData v4 fornece duas opções adicionais, Singleton e contenção, que oferece suporte da API Web 2.2.</span><span class="sxs-lookup"><span data-stu-id="6d9df-107">But OData v4 provides two additional options, Singleton and Containment, both of which WebAPI 2.2 supports.</span></span>


<span data-ttu-id="6d9df-108">Este tópico mostra como definir um conjunto de contenção em um ponto de extremidade OData na API Web 2.2.</span><span class="sxs-lookup"><span data-stu-id="6d9df-108">This topic shows how to define a containment in an OData endpoint in WebApi 2.2.</span></span> <span data-ttu-id="6d9df-109">Para obter mais informações sobre a contenção, consulte [confinamento estará disponível com o OData v4](https://blogs.msdn.com/b/odatateam/archive/2014/03/13/containment-is-coming-with-odata-v4.aspx).</span><span class="sxs-lookup"><span data-stu-id="6d9df-109">For more information about containment, see [Containment is coming with OData v4](https://blogs.msdn.com/b/odatateam/archive/2014/03/13/containment-is-coming-with-odata-v4.aspx).</span></span> <span data-ttu-id="6d9df-110">Para criar um ponto de extremidade OData V4 no API da Web, consulte [criar um OData v4 ponto de extremidade usando API Web ASP.NET 2.2](create-an-odata-v4-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="6d9df-110">To create an OData V4 endpoint in Web API, see [Create an OData v4 Endpoint Using ASP.NET Web API 2.2](create-an-odata-v4-endpoint.md).</span></span>

<span data-ttu-id="6d9df-111">Primeiro, vamos criar um modelo de domínio de confinamento no serviço do OData, usando esse modelo de dados:</span><span class="sxs-lookup"><span data-stu-id="6d9df-111">First, we'll create a containment domain model in the OData service, using this data model:</span></span>

![Modelo de dados](odata-containment-in-web-api-22/_static/image1.png)

<span data-ttu-id="6d9df-113">Uma conta contém muitos PaymentInstruments (PI), mas não definimos uma conjunto de entidades para um PI.</span><span class="sxs-lookup"><span data-stu-id="6d9df-113">An account contains many PaymentInstruments (PI), but we don't define an entity set for a PI.</span></span> <span data-ttu-id="6d9df-114">Em vez disso, os PIs só podem ser acessados por meio de uma conta.</span><span class="sxs-lookup"><span data-stu-id="6d9df-114">Instead, the PIs can only be accessed through an Account.</span></span>

<span data-ttu-id="6d9df-115">Você pode baixar a solução usada neste tópico de [CodePlex](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/).</span><span class="sxs-lookup"><span data-stu-id="6d9df-115">You can download the solution used in this topic from [CodePlex](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/).</span></span>

## <a name="defining-the-data-model"></a><span data-ttu-id="6d9df-116">Definindo o modelo de dados</span><span class="sxs-lookup"><span data-stu-id="6d9df-116">Defining the data model</span></span>

1. <span data-ttu-id="6d9df-117">Defina os tipos CLR.</span><span class="sxs-lookup"><span data-stu-id="6d9df-117">Define the CLR types.</span></span>

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample1.cs)]

    <span data-ttu-id="6d9df-118">O `Contained` atributo é usado para as propriedades de navegação de contenção.</span><span class="sxs-lookup"><span data-stu-id="6d9df-118">The `Contained` attribute is used for containment navigation properties.</span></span>
2. <span data-ttu-id="6d9df-119">Gere o modelo EDM com base nos tipos de CLR.</span><span class="sxs-lookup"><span data-stu-id="6d9df-119">Generate the EDM model based on the CLR types.</span></span>

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample2.cs)]

    <span data-ttu-id="6d9df-120">O `ODataConventionModelBuilder` manipulará a criação do modelo EDM, se o `Contained` atributo é adicionado à propriedade de navegação correspondente.</span><span class="sxs-lookup"><span data-stu-id="6d9df-120">The `ODataConventionModelBuilder` will handle building the EDM model if the `Contained` attribute is added to the corresponding navigation property.</span></span> <span data-ttu-id="6d9df-121">Se a propriedade for um tipo de coleção, um `GetCount(string NameContains)` função também será criada.</span><span class="sxs-lookup"><span data-stu-id="6d9df-121">If the property is a collection type, a `GetCount(string NameContains)` function will also be created.</span></span>

    <span data-ttu-id="6d9df-122">Os metadados gerados terão a seguinte aparência:</span><span class="sxs-lookup"><span data-stu-id="6d9df-122">The generated metadata will look like the following:</span></span>

    [!code-xml[Main](odata-containment-in-web-api-22/samples/sample3.xml?highlight=10)]

    <span data-ttu-id="6d9df-123">O `ContainsTarget` atributo indica que a propriedade de navegação é um conjunto de contenção.</span><span class="sxs-lookup"><span data-stu-id="6d9df-123">The `ContainsTarget` attribute indicates that the navigation property is a containment.</span></span>

## <a name="define-the-containing-entity-set-controller"></a><span data-ttu-id="6d9df-124">Definir o controlador de conjunto de entidade que contém</span><span class="sxs-lookup"><span data-stu-id="6d9df-124">Define the containing entity set controller</span></span>

<span data-ttu-id="6d9df-125">Entidades independentes não tem seu próprio controlador; a ação está definida no controlador de conjunto de entidade que contém.</span><span class="sxs-lookup"><span data-stu-id="6d9df-125">Contained entities don't have their own controller; the action is defined in the containing entity set controller.</span></span> <span data-ttu-id="6d9df-126">Neste exemplo, há um AccountsController, mas nenhum PaymentInstrumentsController.</span><span class="sxs-lookup"><span data-stu-id="6d9df-126">In this sample, there is an AccountsController, but no PaymentInstrumentsController.</span></span>

[!code-csharp[Main](odata-containment-in-web-api-22/samples/sample4.cs)]

<span data-ttu-id="6d9df-127">Se o caminho do OData é 4 ou mais segmentos, atributo somente roteamento funciona, como `[ODataRoute("Accounts({accountId})/PayinPIs({paymentInstrumentId})")]` no controlador acima.</span><span class="sxs-lookup"><span data-stu-id="6d9df-127">If the OData path is 4 or more segments, only attribute routing works, such as `[ODataRoute("Accounts({accountId})/PayinPIs({paymentInstrumentId})")]` in the above controller.</span></span> <span data-ttu-id="6d9df-128">Caso contrário, o atributo e o roteamento convencional funciona: por exemplo, `GetPayInPIs(int key)` corresponde ao `GET ~/Accounts(1)/PayinPIs`.</span><span class="sxs-lookup"><span data-stu-id="6d9df-128">Otherwise, both attribute and conventional routing works: for instance, `GetPayInPIs(int key)` matches `GET ~/Accounts(1)/PayinPIs`.</span></span>

<span data-ttu-id="6d9df-129">*Graças ao Leo Hu para o conteúdo original deste artigo.*</span><span class="sxs-lookup"><span data-stu-id="6d9df-129">*Thanks to Leo Hu for the original content of this article.*</span></span>

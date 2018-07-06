---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-odata-53
title: O que há de novo no ASP.NET Web API OData 5.3 | Microsoft Docs
author: microsoft
description: ''
ms.author: aspnetcontent
ms.date: 09/16/2014
ms.assetid: e39eaa25-83ff-41dc-869d-3818d59a88ae
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-odata-53
msc.type: authoredcontent
ms.openlocfilehash: c185e7ef9bfe6e21601ab61c418c63c8a81b680a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37827023"
---
<a name="whats-new-in-aspnet-web-api-odata-53"></a><span data-ttu-id="0f695-102">O que há de novo no ASP.NET Web API OData 5.3</span><span class="sxs-lookup"><span data-stu-id="0f695-102">What's New in ASP.NET Web API OData 5.3</span></span>
====================
<span data-ttu-id="0f695-103">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="0f695-103">by [Microsoft](https://github.com/microsoft)</span></span>

<span data-ttu-id="0f695-104">Este tópico descreve o que há de novo para ASP.NET Web API OData 5.3.</span><span class="sxs-lookup"><span data-stu-id="0f695-104">This topic describes what's new for ASP.NET Web API OData 5.3.</span></span>

- [<span data-ttu-id="0f695-105">Baixar</span><span class="sxs-lookup"><span data-stu-id="0f695-105">Download</span></span>](#download)
- [<span data-ttu-id="0f695-106">Documentação</span><span class="sxs-lookup"><span data-stu-id="0f695-106">Documentation</span></span>](#documentation)
- [<span data-ttu-id="0f695-107">Bibliotecas principais do OData</span><span class="sxs-lookup"><span data-stu-id="0f695-107">OData Core Libraries</span></span>](#corelib)
- [<span data-ttu-id="0f695-108">Novos recursos</span><span class="sxs-lookup"><span data-stu-id="0f695-108">New Features</span></span>](#newf)
- [<span data-ttu-id="0f695-109">Problemas conhecidos e alterações recentes</span><span class="sxs-lookup"><span data-stu-id="0f695-109">Known Issues and Breaking Changes</span></span>](#known-issues)
- [<span data-ttu-id="0f695-110">Correções de bugs</span><span class="sxs-lookup"><span data-stu-id="0f695-110">Bug Fixes</span></span>](#bug-fixes)
- [<span data-ttu-id="0f695-111">ASP.NET Web API OData 5.3.1</span><span class="sxs-lookup"><span data-stu-id="0f695-111">ASP.NET Web API OData 5.3.1</span></span>](#OD)

<a id="download"></a>
## <a name="download"></a><span data-ttu-id="0f695-112">Baixar</span><span class="sxs-lookup"><span data-stu-id="0f695-112">Download</span></span>

<span data-ttu-id="0f695-113">Os recursos de tempo de execução são lançados como pacotes NuGet na Galeria do NuGet.</span><span class="sxs-lookup"><span data-stu-id="0f695-113">The runtime features are released as NuGet packages on the NuGet gallery.</span></span> <span data-ttu-id="0f695-114">Você pode instalar ou atualizar para os pacotes do NuGet lançados por meio do Console do Gerenciador de pacotes NuGet:</span><span class="sxs-lookup"><span data-stu-id="0f695-114">You can install or update to the released NuGet packages by using the NuGet Package Manager Console:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a><span data-ttu-id="0f695-115">Documentação</span><span class="sxs-lookup"><span data-stu-id="0f695-115">Documentation</span></span>

<span data-ttu-id="0f695-116">Você pode encontrar tutoriais e outras documentações sobre ASP.NET Web API OData na [site da web ASP.NET](../odata-support-in-aspnet-web-api/index.md).</span><span class="sxs-lookup"><span data-stu-id="0f695-116">You can find tutorials and other documentation about ASP.NET Web API OData at the [ASP.NET web site](../odata-support-in-aspnet-web-api/index.md).</span></span>

<a id="corelib"></a>
## <a name="odata-core-libraries"></a><span data-ttu-id="0f695-117">Bibliotecas principais do OData</span><span class="sxs-lookup"><span data-stu-id="0f695-117">OData Core Libraries</span></span>

<span data-ttu-id="0f695-118">Para OData v4, API Web agora usa o ODataLib versão 6.5.0</span><span class="sxs-lookup"><span data-stu-id="0f695-118">For OData v4, Web API now uses ODataLib version 6.5.0</span></span>

<a id="newf"></a>
## <a name="new-features-in-aspnet-web-api-odata-53"></a><span data-ttu-id="0f695-119">Novos recursos no ASP.NET Web API OData 5.3</span><span class="sxs-lookup"><span data-stu-id="0f695-119">New Features in ASP.NET Web API OData 5.3</span></span>

### <a name="support-for-levels-in-expand"></a><span data-ttu-id="0f695-120">Suporte para $levels em $Expandir</span><span class="sxs-lookup"><span data-stu-id="0f695-120">Support for $levels in $expand</span></span>

<span data-ttu-id="0f695-121">Você pode usar o $levels consulta opção em $expanda queries.</span><span class="sxs-lookup"><span data-stu-id="0f695-121">You can use the $levels query option in $expand queries.</span></span> <span data-ttu-id="0f695-122">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="0f695-122">For example:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample2.cmd)]

<span data-ttu-id="0f695-123">Essa consulta é equivalente a:</span><span class="sxs-lookup"><span data-stu-id="0f695-123">This query is equivalent to:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample3.cmd)]

<a id="open-entity-types"></a>
### <a name="support-for-open-entity-types"></a><span data-ttu-id="0f695-124">Suporte para tipos de entidade aberto</span><span class="sxs-lookup"><span data-stu-id="0f695-124">Support for Open Entity Types</span></span>

<span data-ttu-id="0f695-125">Uma *abrir tipo* é um tipo de stuctured que contém as propriedades dinâmicas, além de quaisquer propriedades que são declaradas na definição de tipo.</span><span class="sxs-lookup"><span data-stu-id="0f695-125">An *open type* is a stuctured type that contains dynamic properties, in addition to any properties that are declared in the type definition.</span></span> <span data-ttu-id="0f695-126">Tipos abertos permitem adicionar flexibilidade aos modelos de dados.</span><span class="sxs-lookup"><span data-stu-id="0f695-126">Open types let you add flexibility to your data models.</span></span> <span data-ttu-id="0f695-127">Para obter mais informações, consulte xxxx.</span><span class="sxs-lookup"><span data-stu-id="0f695-127">For more information, see xxxx.</span></span>

### <a name="support-for-dynamic-collection-properties-in-open-types"></a><span data-ttu-id="0f695-128">Suporte para propriedades de coleção dinâmica em tipos abertos</span><span class="sxs-lookup"><span data-stu-id="0f695-128">Support for dynamic collection properties in open types</span></span>

<span data-ttu-id="0f695-129">Anteriormente, uma propriedade dinâmica tinha que ser um único valor.</span><span class="sxs-lookup"><span data-stu-id="0f695-129">Previously, a dynamic property had to be a single value.</span></span> <span data-ttu-id="0f695-130">Em 5.3, as propriedades dinâmicas podem ter valores de coleção.</span><span class="sxs-lookup"><span data-stu-id="0f695-130">In 5.3, dynamic properties can have collection values.</span></span> <span data-ttu-id="0f695-131">Por exemplo, na carga JSON a seguir, o `Emails` propriedade é uma propriedade dinâmica e de coleção do tipo de cadeia de caracteres:</span><span class="sxs-lookup"><span data-stu-id="0f695-131">For example, in the following JSON payload, the `Emails` property is a dynamic property and is of collection of string type:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample4.cmd)]

### <a name="support-for-inheritance-for-complex-types"></a><span data-ttu-id="0f695-132">Suporte para herança para tipos complexos</span><span class="sxs-lookup"><span data-stu-id="0f695-132">Support for inheritance for complex types</span></span>

<span data-ttu-id="0f695-133">Agora os tipos complexos podem herdar de um tipo base.</span><span class="sxs-lookup"><span data-stu-id="0f695-133">Now complex types can inherit from a base type.</span></span> <span data-ttu-id="0f695-134">Por exemplo, um serviço OData pode definir os seguintes tipos complexos:</span><span class="sxs-lookup"><span data-stu-id="0f695-134">For example, an OData service could define the following complex types:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample5.cs)]

<span data-ttu-id="0f695-135">Aqui está o EDM para este exemplo:</span><span class="sxs-lookup"><span data-stu-id="0f695-135">Here is the EDM for this example:</span></span>

[!code-xml[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample6.xml?highlight=8,15)]

<span data-ttu-id="0f695-136">Para obter mais informações, consulte [exemplo de herança de tipo complexos do OData](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).</span><span class="sxs-lookup"><span data-stu-id="0f695-136">For more information, see [OData Complex Type Inheritance Sample](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).</span></span>

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="0f695-137">Problemas conhecidos e alterações recentes</span><span class="sxs-lookup"><span data-stu-id="0f695-137">Known Issues and Breaking Changes</span></span>

<span data-ttu-id="0f695-138">Esta seção descreve problemas conhecidos e alterações significativas no ASP.NET Web API OData 5.3.</span><span class="sxs-lookup"><span data-stu-id="0f695-138">This section describes known issues and breaking changes in the ASP.NET Web API OData 5.3.</span></span>

### <a name="odata-v4"></a><span data-ttu-id="0f695-139">OData v4</span><span class="sxs-lookup"><span data-stu-id="0f695-139">OData v4</span></span>

#### <a name="query-options"></a><span data-ttu-id="0f695-140">Opções de consulta</span><span class="sxs-lookup"><span data-stu-id="0f695-140">Query Options</span></span>

<span data-ttu-id="0f695-141">Problema: Usando o $ aninhada expanda com $levels = max resulta em uma profundidade de expansão incorreto.</span><span class="sxs-lookup"><span data-stu-id="0f695-141">Issue: Using nested $expand with $levels=max results in an incorrect expansion depth.</span></span>

<span data-ttu-id="0f695-142">Por exemplo, dada a seguinte solicitação:</span><span class="sxs-lookup"><span data-stu-id="0f695-142">For example, given the following request:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample7.cmd)]

<span data-ttu-id="0f695-143">Se `MaxExpansionDepth` for 5, essa consulta pode resultar em uma profundidade de expansão de 6.</span><span class="sxs-lookup"><span data-stu-id="0f695-143">If `MaxExpansionDepth` is 5, this query would result in an expansion depth of 6.</span></span>

<a id="bug-fixes"></a>
## <a name="bug-fixes-and-minor-feature-updates"></a><span data-ttu-id="0f695-144">Correções de bugs e atualizações de recursos secundários</span><span class="sxs-lookup"><span data-stu-id="0f695-144">Bug Fixes and Minor Feature Updates</span></span>

<span data-ttu-id="0f695-145">Esta versão também inclui várias correções de bugs e recursos secundários atualizações.</span><span class="sxs-lookup"><span data-stu-id="0f695-145">This release also includes several bug fixes and minor feature updates.</span></span> <span data-ttu-id="0f695-146">Você pode encontrar a lista completa aqui:</span><span class="sxs-lookup"><span data-stu-id="0f695-146">You can find the complete list here:</span></span>

- [<span data-ttu-id="0f695-147">Correções de bug</span><span class="sxs-lookup"><span data-stu-id="0f695-147">Bug fixes</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.3%20Beta&assignedTo=All&component=Web%20API|Web%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="OD"></a>
## <a name="aspnet-web-api-odata-531"></a><span data-ttu-id="0f695-148">ASP.NET Web API OData 5.3.1</span><span class="sxs-lookup"><span data-stu-id="0f695-148">ASP.NET Web API OData 5.3.1</span></span>

<span data-ttu-id="0f695-149">Nesta versão fizemos uma [correção de bug](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.3.1%20Beta&amp;assignedTo=All&amp;component=Web%20API%20OData&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=All) a algumas das enums AllowedFunctions.</span><span class="sxs-lookup"><span data-stu-id="0f695-149">In this release we made a [bug fix](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.3.1%20Beta&amp;assignedTo=All&amp;component=Web%20API%20OData&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=All) to some of the AllowedFunctions enums.</span></span> <span data-ttu-id="0f695-150">Esta versão não tem outras correções de bug ou novos recursos.</span><span class="sxs-lookup"><span data-stu-id="0f695-150">This release doesn't have any other bug fixes or new features.</span></span>

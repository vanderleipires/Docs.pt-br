---
title: "Auxiliares de marcação internos do ASP.NET Core"
author: pkellner
description: "Auxiliares de marcação internos do ASP.NET Core"
keywords: "ASP.NET Core, auxiliar de marcação"
ms.author: riande
manager: wpickett
ms.date: 09/13/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/Index
ms.openlocfilehash: dd732822a715df19c0ee4b6accad3455ad6537da
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
# <a name="aspnet-core-built-in-tag-helpers"></a><span data-ttu-id="9b9f6-104">Auxiliares de marcação internos do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9b9f6-104">ASP.NET Core built-in Tag Helpers</span></span>

<span data-ttu-id="9b9f6-105">Por [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="9b9f6-105">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="9b9f6-106">O ASP.NET Core inclui diversos auxiliares de marcação internos para aumentar sua produtividade.</span><span class="sxs-lookup"><span data-stu-id="9b9f6-106">ASP.NET Core includes many built-in Tag Helpers to boost your productivity.</span></span> <span data-ttu-id="9b9f6-107">Esta seção fornece uma visão geral dos auxiliares de marcação internos.</span><span class="sxs-lookup"><span data-stu-id="9b9f6-107">This section provides an overview of the built-in Tag Helpers.</span></span>

> [!NOTE]
> <span data-ttu-id="9b9f6-108">Há auxiliares de marcação internos que não são abordados, pois eles são usados internamente pelo mecanismo de exibição do [Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="9b9f6-108">There are built-in Tag Helpers which aren't discussed, since they're used internally by the [Razor](xref:mvc/views/razor) view engine.</span></span> <span data-ttu-id="9b9f6-109">Isso inclui um auxiliar de marcação para o caractere ~, que se expande para o caminho raiz do site.</span><span class="sxs-lookup"><span data-stu-id="9b9f6-109">This includes a Tag Helper for the ~ character, which expands to the root path of the website.</span></span>

## <a name="built-in-aspnet-core-tag-helpers"></a><span data-ttu-id="9b9f6-110">Auxiliares de marcação internos do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9b9f6-110">Built-in ASP.NET Core Tag Helpers</span></span>

<span data-ttu-id="9b9f6-111">**[Auxiliar de marcação de âncora](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="9b9f6-111">**[Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)**</span></span>

<span data-ttu-id="9b9f6-112">**[Auxiliar de marcação de cache](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="9b9f6-112">**[Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)**</span></span>

<span data-ttu-id="9b9f6-113">**[Auxiliar de marcação de cache distribuído](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="9b9f6-113">**[Distributed Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)**</span></span>

<span data-ttu-id="9b9f6-114">**[Auxiliar de marcação de ambiente](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="9b9f6-114">**[Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)**</span></span>

[comment]: **[FormActionTagHelper](xref:mvc/views/tag-helpers/builtin-th/form-action-tag-helper)**

<span data-ttu-id="9b9f6-115">**[Auxiliar de marcação de formulário](xref:mvc/views/working-with-forms#the-form-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="9b9f6-115">**[Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper)**</span></span>

<span data-ttu-id="9b9f6-116">**[Auxiliar de marcação de imagem](xref:mvc/views/tag-helpers/builtin-th/image-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="9b9f6-116">**[Image Tag Helper](xref:mvc/views/tag-helpers/builtin-th/image-tag-helper)**</span></span>

<span data-ttu-id="9b9f6-117">**[Auxiliar de marcação de entrada](xref:mvc/views/working-with-forms#the-input-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="9b9f6-117">**[Input Tag Helper](xref:mvc/views/working-with-forms#the-input-tag-helper)**</span></span>

<span data-ttu-id="9b9f6-118">**[Auxiliar de marcação de rótulo](xref:mvc/views/working-with-forms#the-label-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="9b9f6-118">**[Label Tag Helper](xref:mvc/views/working-with-forms#the-label-tag-helper)**</span></span>

[comment]: **[LinkTagHelper](xref:mvc/views/tag-helpers/builtin-th/link-tag-helper)**

[comment]: **[OptionTagHelper](xref:mvc/views/tag-helpers/builtin-th/option-tag-helper)**

[comment]: **[ScriptTagHelper](xref:mvc/views/tag-helpers/builtin-th/script-tag-helper)**

<span data-ttu-id="9b9f6-119">**[Selecionar o auxiliar de marcação](xref:mvc/views/working-with-forms#the-select-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="9b9f6-119">**[Select Tag Helper](xref:mvc/views/working-with-forms#the-select-tag-helper)**</span></span>

<span data-ttu-id="9b9f6-120">**[Auxiliar de marcação de área de texto](xref:mvc/views/working-with-forms#the-textarea-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="9b9f6-120">**[Textarea Tag Helper](xref:mvc/views/working-with-forms#the-textarea-tag-helper)**</span></span>

<span data-ttu-id="9b9f6-121">**[Auxiliar de marcação de mensagem de validação](xref:mvc/views/working-with-forms#the-validation-message-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="9b9f6-121">**[Validation Message Tag Helper](xref:mvc/views/working-with-forms#the-validation-message-tag-helper)**</span></span>

<span data-ttu-id="9b9f6-122">**[Resumo de validação de auxiliar de marcação](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="9b9f6-122">**[Validation Summary Tag Helper](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper)**</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9b9f6-123">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="9b9f6-123">Additional resources</span></span>

* [<span data-ttu-id="9b9f6-124">Desenvolvimento no Lado do Cliente</span><span class="sxs-lookup"><span data-stu-id="9b9f6-124">Client-Side Development</span></span>](xref:client-side/index)
* [<span data-ttu-id="9b9f6-125">Auxiliares de marcação</span><span class="sxs-lookup"><span data-stu-id="9b9f6-125">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)

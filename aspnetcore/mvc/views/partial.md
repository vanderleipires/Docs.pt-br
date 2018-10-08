---
title: Exibições parciais no ASP.NET Core
author: ardalis
description: Descubra como usar exibições parciais para dividir os arquivos de marcação grandes e reduzir a duplicação de marcações comuns em páginas da Web em aplicativos ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 09/11/2018
uid: mvc/views/partial
ms.openlocfilehash: a836ed073dfe769fc3cc0cd0622b17937747928b
ms.sourcegitcommit: 70fb7c9d5f2ddfcf4747382a9f7159feca7a6aa7
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/14/2018
ms.locfileid: "45601750"
---
# <a name="partial-views-in-aspnet-core"></a><span data-ttu-id="9b684-103">Exibições parciais no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9b684-103">Partial views in ASP.NET Core</span></span>

<span data-ttu-id="9b684-104">Por [Steve Smith](https://ardalis.com/), [Luke Latham](https://github.com/guardrex), [Maher JENDOUBI](https://twitter.com/maherjend), [Rick Anderson](https://twitter.com/RickAndMSFT) e [Scott Sauber](https://twitter.com/scottsauber)</span><span class="sxs-lookup"><span data-stu-id="9b684-104">By [Steve Smith](https://ardalis.com/), [Luke Latham](https://github.com/guardrex), [Maher JENDOUBI](https://twitter.com/maherjend), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Scott Sauber](https://twitter.com/scottsauber)</span></span>

<span data-ttu-id="9b684-105">Uma exibição parcial é um arquivo de marcação [Razor](xref:mvc/views/razor) (*.cshtml*) que renderiza a saída HTML *dentro* da saída processada de outro arquivo de marcação.</span><span class="sxs-lookup"><span data-stu-id="9b684-105">A partial view is a [Razor](xref:mvc/views/razor) markup file (*.cshtml*) that renders HTML output *within* another markup file's rendered output.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="9b684-106">O termo *exibição parcial* é usado durante o desenvolvimento de um aplicativo MVC, no qual os arquivos de marcação são chamados de *exibições*, ou de um aplicativo Razor Pages, no qual os arquivos de marcação são chamados de *páginas*.</span><span class="sxs-lookup"><span data-stu-id="9b684-106">The term *partial view* is used when developing either an MVC app, where markup files are called *views*, or a Razor Pages app, where markup files are called *pages*.</span></span> <span data-ttu-id="9b684-107">Este tópico refere-se genericamente a exibições do MVC e a páginas do Razor Pages como *arquivos de marcação*.</span><span class="sxs-lookup"><span data-stu-id="9b684-107">This topic generically refers to MVC views and Razor Pages pages as *markup files*.</span></span>

::: moniker-end

<span data-ttu-id="9b684-108">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/partial/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="9b684-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/partial/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-partial-views"></a><span data-ttu-id="9b684-109">Quando usar exibições parciais</span><span class="sxs-lookup"><span data-stu-id="9b684-109">When to use partial views</span></span>

<span data-ttu-id="9b684-110">Exibições parciais são uma maneira eficaz de:</span><span class="sxs-lookup"><span data-stu-id="9b684-110">Partial views are an effective way to:</span></span>

* <span data-ttu-id="9b684-111">Dividir arquivos de marcação grandes em componentes menores.</span><span class="sxs-lookup"><span data-stu-id="9b684-111">Break up large markup files into smaller components.</span></span>

  <span data-ttu-id="9b684-112">Aproveitar a vantagem de trabalhar com cada parte isolada em uma exibição parcial em um arquivo de marcação grande e complexo, composto por diversas partes lógicas.</span><span class="sxs-lookup"><span data-stu-id="9b684-112">In a large, complex markup file composed of several logical pieces, there's an advantage to working with each piece isolated into a partial view.</span></span> <span data-ttu-id="9b684-113">O código no arquivo de marcação é gerenciável porque a marcação contém somente a estrutura de página geral e as referências a exibições parciais.</span><span class="sxs-lookup"><span data-stu-id="9b684-113">The code in the markup file is manageable because the markup only contains the overall page structure and references to partial views.</span></span>
* <span data-ttu-id="9b684-114">Reduzir a duplicação de conteúdo de marcação comum em arquivos de marcação.</span><span class="sxs-lookup"><span data-stu-id="9b684-114">Reduce the duplication of common markup content across markup files.</span></span>

  <span data-ttu-id="9b684-115">Quando os mesmos elementos de marcação são usados nos arquivos de marcação, uma exibição parcial remove a duplicação de conteúdo de marcação em um arquivo de exibição parcial.</span><span class="sxs-lookup"><span data-stu-id="9b684-115">When the same markup elements are used across markup files, a partial view removes the duplication of markup content into one partial view file.</span></span> <span data-ttu-id="9b684-116">Quando a marcação é alterada na exibição parcial, ela atualiza a saída renderizada dos arquivos de marcação que usam a exibição parcial.</span><span class="sxs-lookup"><span data-stu-id="9b684-116">When the markup is changed in the partial view, it updates the rendered output of the markup files that use the partial view.</span></span>

<span data-ttu-id="9b684-117">As exibições parciais não podem ser usadas para manter os elementos de layout comuns.</span><span class="sxs-lookup"><span data-stu-id="9b684-117">Partial views shouldn't be used to maintain common layout elements.</span></span> <span data-ttu-id="9b684-118">Os elementos de layout comuns precisam ser especificados nos arquivos [_Layout.cshtml](xref:mvc/views/layout).</span><span class="sxs-lookup"><span data-stu-id="9b684-118">Common layout elements should be specified in [_Layout.cshtml](xref:mvc/views/layout) files.</span></span>

<span data-ttu-id="9b684-119">Não use uma exibição parcial em que a lógica de renderização complexa ou a execução de código são necessárias para renderizar a marcação.</span><span class="sxs-lookup"><span data-stu-id="9b684-119">Don't use a partial view where complex rendering logic or code execution is required to render the markup.</span></span> <span data-ttu-id="9b684-120">Em vez de uma exibição parcial, use um [componente de exibição](xref:mvc/views/view-components).</span><span class="sxs-lookup"><span data-stu-id="9b684-120">Instead of a partial view, use a [view component](xref:mvc/views/view-components).</span></span>

## <a name="declare-partial-views"></a><span data-ttu-id="9b684-121">Declarar exibições parciais</span><span class="sxs-lookup"><span data-stu-id="9b684-121">Declare partial views</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="9b684-122">Uma exibição parcial é um arquivo de marcação *.cshtml* mantido dentro da pasta *Exibições* (MVC) ou *Páginas* (Razor Pages).</span><span class="sxs-lookup"><span data-stu-id="9b684-122">A partial view is a *.cshtml* markup file maintained within the *Views* folder (MVC) or *Pages* folder (Razor Pages).</span></span>

<span data-ttu-id="9b684-123">No ASP.NET Core MVC, um <xref:Microsoft.AspNetCore.Mvc.ViewResult> do controlador é capaz de retornar uma exibição ou uma exibição parcial.</span><span class="sxs-lookup"><span data-stu-id="9b684-123">In ASP.NET Core MVC, a controller's <xref:Microsoft.AspNetCore.Mvc.ViewResult> is capable of returning either a view or a partial view.</span></span> <span data-ttu-id="9b684-124">Uma funcionalidade semelhante está planejada para o Razor Pages no ASP.NET Core 2.2.</span><span class="sxs-lookup"><span data-stu-id="9b684-124">An analogous capability is planned for Razor Pages in ASP.NET Core 2.2.</span></span> <span data-ttu-id="9b684-125">No Razor Pages, um <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> pode retornar um <xref:Microsoft.AspNetCore.Mvc.PartialViewResult>.</span><span class="sxs-lookup"><span data-stu-id="9b684-125">In Razor Pages, a <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> can return a <xref:Microsoft.AspNetCore.Mvc.PartialViewResult>.</span></span> <span data-ttu-id="9b684-126">A referência e a renderização de exibições parciais são descritas na seção [Referenciar uma exibição parcial](#reference-a-partial-view).</span><span class="sxs-lookup"><span data-stu-id="9b684-126">Referencing and rendering partial views is described in the [Reference a partial view](#reference-a-partial-view) section.</span></span>

<span data-ttu-id="9b684-127">Ao contrário da exibição do MVC ou renderização de página, uma exibição parcial não executa *_ViewStart.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="9b684-127">Unlike MVC view or page rendering, a partial view doesn't run *_ViewStart.cshtml*.</span></span> <span data-ttu-id="9b684-128">Para obter mais informações sobre *_ViewStart.cshtml*, consulte <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="9b684-128">For more information on *_ViewStart.cshtml*, see <xref:mvc/views/layout>.</span></span>

<span data-ttu-id="9b684-129">Nomes de arquivos de exibição parcial geralmente começam com um sublinhado (`_`).</span><span class="sxs-lookup"><span data-stu-id="9b684-129">Partial view file names often begin with an underscore (`_`).</span></span> <span data-ttu-id="9b684-130">Essa convenção de nomenclatura não é obrigatória, mas ajuda a diferenciar visualmente as exibições parciais das exibições e das páginas.</span><span class="sxs-lookup"><span data-stu-id="9b684-130">This naming convention isn't required, but it helps to visually differentiate partial views from views and pages.</span></span> <span data-ttu-id="9b684-131">Quando o nome do arquivo começar com um sublinhado, o Razor Pages não processará o arquivo de marcação como uma página do Razor Pages, mesmo quando a marcação do arquivo incluir a diretiva `@page`.</span><span class="sxs-lookup"><span data-stu-id="9b684-131">When the file name starts with an underscore, Razor Pages doesn't process the markup file as a Razor Pages page, even when the file's markup includes the `@page` directive.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="9b684-132">Uma exibição parcial é um arquivo de marcação *.cshtml* mantido dentro da pasta *Exibições*.</span><span class="sxs-lookup"><span data-stu-id="9b684-132">A partial view is a *.cshtml* markup file maintained within the *Views* folder.</span></span>

<span data-ttu-id="9b684-133">Um <xref:Microsoft.AspNetCore.Mvc.ViewResult> do controlador é capaz de retornar uma exibição ou uma exibição parcial.</span><span class="sxs-lookup"><span data-stu-id="9b684-133">A controller's <xref:Microsoft.AspNetCore.Mvc.ViewResult> is capable of returning either a view or a partial view.</span></span>

<span data-ttu-id="9b684-134">Ao contrário da renderização de exibição do MVC, uma exibição parcial não executa *_ViewStart.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="9b684-134">Unlike MVC view rendering, a partial view doesn't run *_ViewStart.cshtml*.</span></span> <span data-ttu-id="9b684-135">Para obter mais informações sobre *_ViewStart.cshtml*, consulte <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="9b684-135">For more information on *_ViewStart.cshtml*, see <xref:mvc/views/layout>.</span></span>

<span data-ttu-id="9b684-136">Nomes de arquivos de exibição parcial geralmente começam com um sublinhado (`_`).</span><span class="sxs-lookup"><span data-stu-id="9b684-136">Partial view file names often begin with an underscore (`_`).</span></span> <span data-ttu-id="9b684-137">Essa convenção de nomenclatura não é obrigatória, mas ajuda a diferenciar visualmente as exibições parciais das exibições.</span><span class="sxs-lookup"><span data-stu-id="9b684-137">This naming convention isn't required, but it helps to visually differentiate partial views from views.</span></span>

::: moniker-end

## <a name="reference-a-partial-view"></a><span data-ttu-id="9b684-138">Referenciar uma exibição parcial</span><span class="sxs-lookup"><span data-stu-id="9b684-138">Reference a partial view</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="9b684-139">Há várias maneiras de referenciar uma exibição parcial em um arquivo de marcação.</span><span class="sxs-lookup"><span data-stu-id="9b684-139">Within a markup file, there are several ways to reference a partial view.</span></span> <span data-ttu-id="9b684-140">É recomendável que os aplicativos usem uma das seguintes abordagens de renderização assíncrona:</span><span class="sxs-lookup"><span data-stu-id="9b684-140">We recommend that apps use one of the following asynchronous rendering approaches:</span></span>

* [<span data-ttu-id="9b684-141">Auxiliar de marcação parcial</span><span class="sxs-lookup"><span data-stu-id="9b684-141">Partial Tag Helper</span></span>](#partial-tag-helper)
* [<span data-ttu-id="9b684-142">Auxiliar de HTML assíncrono</span><span class="sxs-lookup"><span data-stu-id="9b684-142">Asynchronous HTML Helper</span></span>](#asynchronous-html-helper)

::: moniker-end

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="9b684-143">Há duas maneiras de referenciar uma exibição parcial em um arquivo de marcação:</span><span class="sxs-lookup"><span data-stu-id="9b684-143">Within a markup file, there are two ways to reference a partial view:</span></span>

* [<span data-ttu-id="9b684-144">Auxiliar de HTML assíncrono</span><span class="sxs-lookup"><span data-stu-id="9b684-144">Asynchronous HTML Helper</span></span>](#asynchronous-html-helper)
* [<span data-ttu-id="9b684-145">Auxiliar de HTML síncrono</span><span class="sxs-lookup"><span data-stu-id="9b684-145">Synchronous HTML Helper</span></span>](#synchronous-html-helper)

<span data-ttu-id="9b684-146">É recomendável que os aplicativos usem o [Auxiliar de HTML assíncrono](#asynchronous-html-helper).</span><span class="sxs-lookup"><span data-stu-id="9b684-146">We recommend that apps use the [Asynchronous HTML Helper](#asynchronous-html-helper).</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

### <a name="partial-tag-helper"></a><span data-ttu-id="9b684-147">Auxiliar de marca parcial</span><span class="sxs-lookup"><span data-stu-id="9b684-147">Partial Tag Helper</span></span>

<span data-ttu-id="9b684-148">O [Auxiliar de Marca Parcial](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper) exige o ASP.NET Core 2.1 ou posterior.</span><span class="sxs-lookup"><span data-stu-id="9b684-148">The [Partial Tag Helper](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper) requires ASP.NET Core 2.1 or later.</span></span>

<span data-ttu-id="9b684-149">O Auxiliar de Marca Parcial renderiza o conteúdo de forma assíncrona e usa uma sintaxe semelhante a HTML:</span><span class="sxs-lookup"><span data-stu-id="9b684-149">The Partial Tag Helper renders content asynchronously and uses an HTML-like syntax:</span></span>

```cshtml
<partial name="_PartialName" />
```

<span data-ttu-id="9b684-150">Quando houver uma extensão de arquivo, o Auxiliar de Marca fará referência a uma exibição parcial que precisa estar na mesma pasta que o arquivo de marcação que chama a exibição parcial:</span><span class="sxs-lookup"><span data-stu-id="9b684-150">When a file extension is present, the Tag Helper references a partial view that must be in the same folder as the markup file calling the partial view:</span></span>

```cshtml
<partial name="_PartialName.cshtml" />
```

<span data-ttu-id="9b684-151">O exemplo a seguir faz referência a uma exibição parcial da raiz do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="9b684-151">The following example references a partial view from the app root.</span></span> <span data-ttu-id="9b684-152">Caminhos que começam com um til-barra (`~/`) ou uma barra (`/`) referem-se à raiz do aplicativo:</span><span class="sxs-lookup"><span data-stu-id="9b684-152">Paths that start with a tilde-slash (`~/`) or a slash (`/`) refer to the app root:</span></span>

<span data-ttu-id="9b684-153">**Páginas do Razor**</span><span class="sxs-lookup"><span data-stu-id="9b684-153">**Razor Pages**</span></span>

```cshtml
<partial name="~/Pages/Folder/_PartialName.cshtml" />
<partial name="/Pages/Folder/_PartialName.cshtml" />
```

<span data-ttu-id="9b684-154">**MVC**</span><span class="sxs-lookup"><span data-stu-id="9b684-154">**MVC**</span></span>

```cshtml
<partial name="~/Views/Folder/_PartialName.cshtml" />
<partial name="/Views/Folder/_PartialName.cshtml" />
```

<span data-ttu-id="9b684-155">O exemplo a seguir faz referência a uma exibição parcial com um caminho relativo:</span><span class="sxs-lookup"><span data-stu-id="9b684-155">The following example references a partial view with a relative path:</span></span>

```cshtml
<partial name="../Account/_PartialName.cshtml" />
```

<span data-ttu-id="9b684-156">Para obter mais informações, consulte <xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper>.</span><span class="sxs-lookup"><span data-stu-id="9b684-156">For more information, see <xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper>.</span></span>

::: moniker-end

### <a name="asynchronous-html-helper"></a><span data-ttu-id="9b684-157">Auxiliar HTML assíncrono</span><span class="sxs-lookup"><span data-stu-id="9b684-157">Asynchronous HTML Helper</span></span>

<span data-ttu-id="9b684-158">Ao usar um Auxiliar HTML, a melhor prática é usar <xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.PartialAsync*>.</span><span class="sxs-lookup"><span data-stu-id="9b684-158">When using an HTML Helper, the best practice is to use <xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.PartialAsync*>.</span></span> <span data-ttu-id="9b684-159">`PartialAsync` retorna um tipo <xref:Microsoft.AspNetCore.Html.IHtmlContent> encapsulado em um <xref:System.Threading.Tasks.Task`1>.</span><span class="sxs-lookup"><span data-stu-id="9b684-159">`PartialAsync` returns an <xref:Microsoft.AspNetCore.Html.IHtmlContent> type wrapped in a <xref:System.Threading.Tasks.Task`1>.</span></span> <span data-ttu-id="9b684-160">O método é referenciado prefixando a chamada em espera com um caractere `@`:</span><span class="sxs-lookup"><span data-stu-id="9b684-160">The method is referenced by prefixing the awaited call with an `@` character:</span></span>

```cshtml
@await Html.PartialAsync("_PartialName")
```

<span data-ttu-id="9b684-161">Quando houver a extensão de arquivo, o Auxiliar de HTML fará referência a uma exibição parcial que precisa estar na mesma pasta que o arquivo de marcação que chama a exibição parcial:</span><span class="sxs-lookup"><span data-stu-id="9b684-161">When the file extension is present, the HTML Helper references a partial view that must be in the same folder as the markup file calling the partial view:</span></span>

```cshtml
@await Html.PartialAsync("_PartialName.cshtml")
```

<span data-ttu-id="9b684-162">O exemplo a seguir faz referência a uma exibição parcial da raiz do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="9b684-162">The following example references a partial view from the app root.</span></span> <span data-ttu-id="9b684-163">Caminhos que começam com um til-barra (`~/`) ou uma barra (`/`) referem-se à raiz do aplicativo:</span><span class="sxs-lookup"><span data-stu-id="9b684-163">Paths that start with a tilde-slash (`~/`) or a slash (`/`) refer to the app root:</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="9b684-164">**Páginas do Razor**</span><span class="sxs-lookup"><span data-stu-id="9b684-164">**Razor Pages**</span></span>

```cshtml
@await Html.PartialAsync("~/Pages/Folder/_PartialName.cshtml")
@await Html.PartialAsync("/Pages/Folder/_PartialName.cshtml")
```

<span data-ttu-id="9b684-165">**MVC**</span><span class="sxs-lookup"><span data-stu-id="9b684-165">**MVC**</span></span>

::: moniker-end

```cshtml
@await Html.PartialAsync("~/Views/Folder/_PartialName.cshtml")
@await Html.PartialAsync("/Views/Folder/_PartialName.cshtml")
```

<span data-ttu-id="9b684-166">O exemplo a seguir faz referência a uma exibição parcial com um caminho relativo:</span><span class="sxs-lookup"><span data-stu-id="9b684-166">The following example references a partial view with a relative path:</span></span>

```cshtml
@await Html.PartialAsync("../Account/_LoginPartial.cshtml")
```

<span data-ttu-id="9b684-167">Como alternativa, é possível renderizar uma exibição parcial com <xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.RenderPartialAsync*>.</span><span class="sxs-lookup"><span data-stu-id="9b684-167">Alternatively, you can render a partial view with <xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.RenderPartialAsync*>.</span></span> <span data-ttu-id="9b684-168">Esse método não retorna um <xref:Microsoft.AspNetCore.Html.IHtmlContent>.</span><span class="sxs-lookup"><span data-stu-id="9b684-168">This method doesn't return an <xref:Microsoft.AspNetCore.Html.IHtmlContent>.</span></span> <span data-ttu-id="9b684-169">Ele transmite a saída renderizada diretamente para a resposta.</span><span class="sxs-lookup"><span data-stu-id="9b684-169">It streams the rendered output directly to the response.</span></span> <span data-ttu-id="9b684-170">Como o método não retorna nenhum resultado, ele precisa ser chamado dentro de um bloco de código Razor:</span><span class="sxs-lookup"><span data-stu-id="9b684-170">Because the method doesn't return a result, it must be called within a Razor code block:</span></span>

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Home/Discovery.cshtml?name=snippet_RenderPartialAsync)]

<span data-ttu-id="9b684-171">Como `RenderPartialAsync` transmite conteúdo renderizado, ele apresenta melhor desempenho em alguns cenários.</span><span class="sxs-lookup"><span data-stu-id="9b684-171">Since `RenderPartialAsync` streams rendered content, it provides better performance in some scenarios.</span></span> <span data-ttu-id="9b684-172">Em situações cruciais para o desempenho, submeta a página a benchmark usando ambas as abordagens e use aquela que gera uma resposta mais rápida.</span><span class="sxs-lookup"><span data-stu-id="9b684-172">In performance-critical situations, benchmark the page using both approaches and use the approach that generates a faster response.</span></span>

### <a name="synchronous-html-helper"></a><span data-ttu-id="9b684-173">Auxiliar de HTML assíncrono</span><span class="sxs-lookup"><span data-stu-id="9b684-173">Synchronous HTML Helper</span></span>

<span data-ttu-id="9b684-174"><xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.Partial*> e <xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.RenderPartial*> são os equivalentes síncronos de `PartialAsync` e `RenderPartialAsync`, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="9b684-174"><xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.Partial*> and <xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.RenderPartial*> are the synchronous equivalents of `PartialAsync` and `RenderPartialAsync`, respectively.</span></span> <span data-ttu-id="9b684-175">Os equivalentes síncronos não são recomendados porque há cenários em que eles realizam deadlock.</span><span class="sxs-lookup"><span data-stu-id="9b684-175">The synchronous equivalents aren't recommended because there are scenarios in which they deadlock.</span></span> <span data-ttu-id="9b684-176">Os métodos síncronos estão programados para serem removidos em uma versão futura.</span><span class="sxs-lookup"><span data-stu-id="9b684-176">The synchronous methods are targeted for removal in a future release.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="9b684-177">Se for necessário executar código, use um [componente de exibição](xref:mvc/views/view-components) em vez de uma exibição parcial.</span><span class="sxs-lookup"><span data-stu-id="9b684-177">If you need to execute code, use a [view component](xref:mvc/views/view-components) instead of a partial view.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="9b684-178">Chamar `Partial` ou `RenderPartial` resulta em um aviso do analisador do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9b684-178">Calling `Partial` or `RenderPartial` results in a Visual Studio analyzer warning.</span></span> <span data-ttu-id="9b684-179">Por exemplo, a presença de `Partial` produz a seguinte mensagem de aviso:</span><span class="sxs-lookup"><span data-stu-id="9b684-179">For example, the presence of `Partial` yields the following warning message:</span></span>

> <span data-ttu-id="9b684-180">Uso de IHtmlHelper.Partial pode resultar em deadlocks de aplicativo.</span><span class="sxs-lookup"><span data-stu-id="9b684-180">Use of IHtmlHelper.Partial may result in application deadlocks.</span></span> <span data-ttu-id="9b684-181">Considere usar o Auxiliar de Marca &lt;parcial&gt; ou IHtmlHelper.PartialAsync.</span><span class="sxs-lookup"><span data-stu-id="9b684-181">Consider using &lt;partial&gt; Tag Helper or IHtmlHelper.PartialAsync.</span></span>

<span data-ttu-id="9b684-182">Substitua as chamadas para `@Html.Partial` por `@await Html.PartialAsync` ou o [Auxiliar de Marca Parcial](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="9b684-182">Replace calls to `@Html.Partial` with `@await Html.PartialAsync` or the [Partial Tag Helper](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper).</span></span> <span data-ttu-id="9b684-183">Para obter mais informações sobre a migração do auxiliar de marca parcial, consulte [Migrar de um auxiliar HTML](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper#migrate-from-an-html-helper).</span><span class="sxs-lookup"><span data-stu-id="9b684-183">For more information on Partial Tag Helper migration, see [Migrate from an HTML Helper](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper#migrate-from-an-html-helper).</span></span>

::: moniker-end

## <a name="partial-view-discovery"></a><span data-ttu-id="9b684-184">Descoberta de exibição parcial</span><span class="sxs-lookup"><span data-stu-id="9b684-184">Partial view discovery</span></span>

<span data-ttu-id="9b684-185">Quando uma exibição parcial é referenciada pelo nome sem uma extensão de arquivo, os seguintes locais são pesquisados na ordem indicada:</span><span class="sxs-lookup"><span data-stu-id="9b684-185">When a partial view is referenced by name without a file extension, the following locations are searched in the stated order:</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="9b684-186">**Páginas do Razor**</span><span class="sxs-lookup"><span data-stu-id="9b684-186">**Razor Pages**</span></span>

1. <span data-ttu-id="9b684-187">Pasta da página em execução no momento</span><span class="sxs-lookup"><span data-stu-id="9b684-187">Currently executing page's folder</span></span>
1. <span data-ttu-id="9b684-188">Grafo do diretório acima da pasta da página</span><span class="sxs-lookup"><span data-stu-id="9b684-188">Directory graph above the page's folder</span></span>
1. `/Shared`
1. `/Pages/Shared`
1. `/Views/Shared`

<span data-ttu-id="9b684-189">**MVC**</span><span class="sxs-lookup"><span data-stu-id="9b684-189">**MVC**</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

1. `/Areas/<Area-Name>/Views/<Controller-Name>`
1. `/Areas/<Area-Name>/Views/Shared`
1. `/Views/Shared`
1. `/Pages/Shared`

::: moniker-end

::: moniker range="< aspnetcore-2.0"

1. `/Areas/<Area-Name>/Views/<Controller-Name>`
1. `/Areas/<Area-Name>/Views/Shared`
1. `/Views/Shared`

::: moniker-end

<span data-ttu-id="9b684-190">As convenções a seguir se aplicam à descoberta de exibição parcial:</span><span class="sxs-lookup"><span data-stu-id="9b684-190">The following conventions apply to partial view discovery:</span></span>

* <span data-ttu-id="9b684-191">Diferentes exibições parciais com o mesmo nome de arquivo são permitidos quando as exibições parciais estão em pastas diferentes.</span><span class="sxs-lookup"><span data-stu-id="9b684-191">Different partial views with the same file name are allowed when the partial views are in different folders.</span></span>
* <span data-ttu-id="9b684-192">Ao referenciar uma exibição parcial pelo nome sem uma extensão de arquivo e a exibição parcial está presente na pasta do chamador e na pasta *Compartilhada*, a exibição parcial na pasta do chamador fornece a exibição parcial.</span><span class="sxs-lookup"><span data-stu-id="9b684-192">When referencing a partial view by name without a file extension and the partial view is present in both the caller's folder and the *Shared* folder, the partial view in the caller's folder supplies the partial view.</span></span> <span data-ttu-id="9b684-193">Se a exibição parcial não existir na pasta do chamador, ela será fornecida pela pasta *Compartilhada*.</span><span class="sxs-lookup"><span data-stu-id="9b684-193">If the partial view isn't present in the caller's folder, the partial view is provided from the *Shared* folder.</span></span> <span data-ttu-id="9b684-194">Exibições parciais na pasta *Compartilhada* são chamadas de *exibições parciais compartilhadas* ou *exibições parciais padrão*.</span><span class="sxs-lookup"><span data-stu-id="9b684-194">Partial views in the *Shared* folder are called *shared partial views* or *default partial views*.</span></span>
* <span data-ttu-id="9b684-195">Exibições parciais podem ser *encadeadas*&mdash;uma exibição parcial pode chamar outra se uma referência circular não tiver sido formada por chamadas.</span><span class="sxs-lookup"><span data-stu-id="9b684-195">Partial views can be *chained*&mdash;a partial view can call another partial view if a circular reference isn't formed by the calls.</span></span> <span data-ttu-id="9b684-196">Caminhos relativos sempre são relativos ao arquivo atual, não à raiz ou ao pai do arquivo.</span><span class="sxs-lookup"><span data-stu-id="9b684-196">Relative paths are always relative to the current file, not to the root or parent of the file.</span></span>

> [!NOTE]
> <span data-ttu-id="9b684-197">Um [Razor](xref:mvc/views/razor) `section` definido em uma exibição parcial é invisível para arquivos de marcação pai.</span><span class="sxs-lookup"><span data-stu-id="9b684-197">A [Razor](xref:mvc/views/razor) `section` defined in a partial view is invisible to parent markup files.</span></span> <span data-ttu-id="9b684-198">A `section` só é visível para a exibição parcial na qual ela está definida.</span><span class="sxs-lookup"><span data-stu-id="9b684-198">The `section` is only visible to the partial view in which it's defined.</span></span>

## <a name="access-data-from-partial-views"></a><span data-ttu-id="9b684-199">Acessar dados de exibições parciais</span><span class="sxs-lookup"><span data-stu-id="9b684-199">Access data from partial views</span></span>

<span data-ttu-id="9b684-200">Quando uma exibição parcial é instanciada, ela recebe uma *cópia* do dicionário `ViewData` do pai.</span><span class="sxs-lookup"><span data-stu-id="9b684-200">When a partial view is instantiated, it receives a *copy* of the parent's `ViewData` dictionary.</span></span> <span data-ttu-id="9b684-201">As atualizações feitas nos dados dentro da exibição parcial não são persistidas na exibição pai.</span><span class="sxs-lookup"><span data-stu-id="9b684-201">Updates made to the data within the partial view aren't persisted to the parent view.</span></span> <span data-ttu-id="9b684-202">Alterações a `ViewData` em uma exibição parcial são perdidas quando a exibição parcial retorna.</span><span class="sxs-lookup"><span data-stu-id="9b684-202">`ViewData` changes in a partial view are lost when the partial view returns.</span></span>

<span data-ttu-id="9b684-203">O exemplo a seguir demonstra como passar uma instância de [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) para uma exibição parcial:</span><span class="sxs-lookup"><span data-stu-id="9b684-203">The following example demonstrates how to pass an instance of [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) to a partial view:</span></span>

```cshtml
@await Html.PartialAsync("_PartialName", customViewData)
```

<span data-ttu-id="9b684-204">Você pode passar um modelo para uma exibição parcial.</span><span class="sxs-lookup"><span data-stu-id="9b684-204">You can pass a model into a partial view.</span></span> <span data-ttu-id="9b684-205">O modelo pode ser um objeto personalizado.</span><span class="sxs-lookup"><span data-stu-id="9b684-205">The model can be a custom object.</span></span> <span data-ttu-id="9b684-206">Você pode passar um modelo com `PartialAsync` (renderiza um bloco de conteúdo para o chamador) ou `RenderPartialAsync` (transmite o conteúdo para a saída):</span><span class="sxs-lookup"><span data-stu-id="9b684-206">You can pass a model with `PartialAsync` (renders a block of content to the caller) or `RenderPartialAsync` (streams the content to the output):</span></span>

```cshtml
@await Html.PartialAsync("_PartialName", model)
```

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="9b684-207">**Páginas do Razor**</span><span class="sxs-lookup"><span data-stu-id="9b684-207">**Razor Pages**</span></span>

<span data-ttu-id="9b684-208">A marcação a seguir no aplicativo de exemplo é da página *Pages/ArticlesRP/ReadRP.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="9b684-208">The following markup in the sample app is from the *Pages/ArticlesRP/ReadRP.cshtml* page.</span></span> <span data-ttu-id="9b684-209">A página contém duas exibições parciais.</span><span class="sxs-lookup"><span data-stu-id="9b684-209">The page contains two partial views.</span></span> <span data-ttu-id="9b684-210">A segunda exibição parcial passa um modelo e `ViewData` para a exibição parcial.</span><span class="sxs-lookup"><span data-stu-id="9b684-210">The second partial view passes in a model and `ViewData` to the partial view.</span></span> <span data-ttu-id="9b684-211">A sobrecarga do construtor `ViewDataDictionary` é usada para passar um novo dicionário `ViewData`, retendo ainda o dicionário `ViewData` existente.</span><span class="sxs-lookup"><span data-stu-id="9b684-211">The `ViewDataDictionary` constructor overload is used to pass a new `ViewData` dictionary while retaining the existing `ViewData` dictionary.</span></span>

[!code-cshtml[](partial/sample/PartialViewsSample/Pages/ArticlesRP/ReadRP.cshtml?name=snippet_ReadPartialViewRP&highlight=5,15-19)]

<span data-ttu-id="9b684-212">*Pages/Shared/_AuthorPartialRP.cshtml* é a primeira exibição parcial referenciada pelo arquivo de marcação *ReadRP.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="9b684-212">*Pages/Shared/_AuthorPartialRP.cshtml* is the first partial view referenced by the *ReadRP.cshtml* markup file:</span></span>

[!code-cshtml[](partial/sample/PartialViewsSample/Pages/Shared/_AuthorPartialRP.cshtml)]

<span data-ttu-id="9b684-213">*Pages/ArticlesRP/_ArticleSectionRP.cshtml* é a segunda exibição parcial referenciada pelo arquivo de marcação *ReadRP.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="9b684-213">*Pages/ArticlesRP/_ArticleSectionRP.cshtml* is the second partial view referenced by the *ReadRP.cshtml* markup file:</span></span>

[!code-cshtml[](partial/sample/PartialViewsSample/Pages/ArticlesRP/_ArticleSectionRP.cshtml)]

<span data-ttu-id="9b684-214">**MVC**</span><span class="sxs-lookup"><span data-stu-id="9b684-214">**MVC**</span></span>

::: moniker-end

<span data-ttu-id="9b684-215">A marcação a seguir no aplicativo de exemplo mostra a exibição *Views/Articles/Read.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="9b684-215">The following markup in the sample app shows the *Views/Articles/Read.cshtml* view.</span></span> <span data-ttu-id="9b684-216">A exibição contém duas exibições parciais.</span><span class="sxs-lookup"><span data-stu-id="9b684-216">The view contains two partial views.</span></span> <span data-ttu-id="9b684-217">A segunda exibição parcial passa um modelo e `ViewData` para a exibição parcial.</span><span class="sxs-lookup"><span data-stu-id="9b684-217">The second partial view passes in a model and `ViewData` to the partial view.</span></span> <span data-ttu-id="9b684-218">A sobrecarga do construtor `ViewDataDictionary` é usada para passar um novo dicionário `ViewData`, retendo ainda o dicionário `ViewData` existente.</span><span class="sxs-lookup"><span data-stu-id="9b684-218">The `ViewDataDictionary` constructor overload is used to pass a new `ViewData` dictionary while retaining the existing `ViewData` dictionary.</span></span>

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Articles/Read.cshtml?name=snippet_ReadPartialView&highlight=5,15-19)]

<span data-ttu-id="9b684-219">*Views/Shared/_AuthorPartial.cshtml* é a primeira exibição parcial referenciada pelo arquivo de marcação *ReadRP.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="9b684-219">*Views/Shared/_AuthorPartial.cshtml* is the first partial view referenced by the *ReadRP.cshtml* markup file:</span></span>

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Shared/_AuthorPartial.cshtml)]

<span data-ttu-id="9b684-220">*Views/Articles/_ArticleSection.cshtml* é a segunda exibição parcial referenciada pelo arquivo de marcação *Read.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="9b684-220">*Views/Articles/_ArticleSection.cshtml* is the second partial view referenced by the *Read.cshtml* markup file:</span></span>

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Articles/_ArticleSection.cshtml)]

<span data-ttu-id="9b684-221">No tempo de execução, as parciais são renderizadas para a saída renderizada do arquivo de marcação pai que, por sua vez, é renderizada dentro do *_Layout.cshtml* compartilhado.</span><span class="sxs-lookup"><span data-stu-id="9b684-221">At runtime, the partials are rendered into the parent markup file's rendered output, which itself is rendered within the shared *_Layout.cshtml*.</span></span> <span data-ttu-id="9b684-222">A primeira exibição parcial renderiza a data de publicação e o nome do autor do artigo:</span><span class="sxs-lookup"><span data-stu-id="9b684-222">The first partial view renders the article author's name and publication date:</span></span>

> <span data-ttu-id="9b684-223">Abraham Lincoln</span><span class="sxs-lookup"><span data-stu-id="9b684-223">Abraham Lincoln</span></span>
>
> <span data-ttu-id="9b684-224">Esta exibição parcial do &lt;caminho do arquivo de exibição parcial compartilhada&gt;.</span><span class="sxs-lookup"><span data-stu-id="9b684-224">This partial view from &lt;shared partial view file path&gt;.</span></span>
> <span data-ttu-id="9b684-225">19/11/1863 12:00:00 AM</span><span class="sxs-lookup"><span data-stu-id="9b684-225">11/19/1863 12:00:00 AM</span></span>

<span data-ttu-id="9b684-226">A segunda exibição parcial renderiza as seções do artigo:</span><span class="sxs-lookup"><span data-stu-id="9b684-226">The second partial view renders the article's sections:</span></span>

> <span data-ttu-id="9b684-227">Índice da seção um: 0</span><span class="sxs-lookup"><span data-stu-id="9b684-227">Section One Index: 0</span></span>
>
> <span data-ttu-id="9b684-228">Pontuação de quatro e há sete anos...</span><span class="sxs-lookup"><span data-stu-id="9b684-228">Four score and seven years ago ...</span></span>
>
> <span data-ttu-id="9b684-229">Índice da seção dois: 1</span><span class="sxs-lookup"><span data-stu-id="9b684-229">Section Two Index: 1</span></span>
>
> <span data-ttu-id="9b684-230">Agora estamos envolvidos em uma grande guerra civil, testando...</span><span class="sxs-lookup"><span data-stu-id="9b684-230">Now we are engaged in a great civil war, testing ...</span></span>
>
> <span data-ttu-id="9b684-231">Índice da seção três: 2</span><span class="sxs-lookup"><span data-stu-id="9b684-231">Section Three Index: 2</span></span>
>
> <span data-ttu-id="9b684-232">Mas, em um sentido mais amplo, não podemos dedicar...</span><span class="sxs-lookup"><span data-stu-id="9b684-232">But, in a larger sense, we can not dedicate ...</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9b684-233">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="9b684-233">Additional resources</span></span>

::: moniker range=">= aspnetcore-2.1"

* <xref:mvc/views/razor>
* <xref:mvc/views/tag-helpers/intro>
* <xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper>
* <xref:mvc/views/view-components>
* <xref:mvc/controllers/areas>

::: moniker-end

::: moniker range="< aspnetcore-2.1"

* <xref:mvc/views/razor>
* <xref:mvc/views/view-components>
* <xref:mvc/controllers/areas>

::: moniker-end

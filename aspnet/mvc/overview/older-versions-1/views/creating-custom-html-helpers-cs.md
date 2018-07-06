---
uid: aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs
title: Criando auxiliares de HTML personalizados (c#) | Microsoft Docs
author: microsoft
description: O objetivo deste tutorial é demonstrar como você pode criar auxiliares de HTML personalizado que você pode usar em suas exibições MVC. Ao aproveitar o auxiliar HTML...
ms.author: aspnetcontent
ms.date: 10/07/2008
ms.assetid: e454c67d-a86e-4119-a858-eb04bbec2dff
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs
msc.type: authoredcontent
ms.openlocfilehash: 0606ec3b5595fbe73918b82e32b393871e8533a1
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37839403"
---
<a name="creating-custom-html-helpers-c"></a><span data-ttu-id="2f4c3-104">Criando auxiliares de HTML personalizados (c#)</span><span class="sxs-lookup"><span data-stu-id="2f4c3-104">Creating Custom HTML Helpers (C#)</span></span>
====================
<span data-ttu-id="2f4c3-105">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="2f4c3-105">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="2f4c3-106">Baixar PDF</span><span class="sxs-lookup"><span data-stu-id="2f4c3-106">Download PDF</span></span>](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_9_CS.pdf)

> <span data-ttu-id="2f4c3-107">O objetivo deste tutorial é demonstrar como você pode criar auxiliares de HTML personalizado que você pode usar em suas exibições MVC.</span><span class="sxs-lookup"><span data-stu-id="2f4c3-107">The goal of this tutorial is to demonstrate how you can create custom HTML Helpers that you can use within your MVC views.</span></span> <span data-ttu-id="2f4c3-108">Ao tirar proveito dos auxiliares HTML, você pode reduzir a quantidade de digitação entediante de marcas HTML que você deve executar para criar uma página HTML padrão.</span><span class="sxs-lookup"><span data-stu-id="2f4c3-108">By taking advantage of HTML Helpers, you can reduce the amount of tedious typing of HTML tags that you must perform to create a standard HTML page.</span></span>


<span data-ttu-id="2f4c3-109">O objetivo deste tutorial é demonstrar como você pode criar auxiliares de HTML personalizado que você pode usar em suas exibições MVC.</span><span class="sxs-lookup"><span data-stu-id="2f4c3-109">The goal of this tutorial is to demonstrate how you can create custom HTML Helpers that you can use within your MVC views.</span></span> <span data-ttu-id="2f4c3-110">Ao tirar proveito dos auxiliares HTML, você pode reduzir a quantidade de digitação entediante de marcas HTML que você deve executar para criar uma página HTML padrão.</span><span class="sxs-lookup"><span data-stu-id="2f4c3-110">By taking advantage of HTML Helpers, you can reduce the amount of tedious typing of HTML tags that you must perform to create a standard HTML page.</span></span>

<span data-ttu-id="2f4c3-111">Na primeira parte deste tutorial, eu descrevem alguns dos auxiliares HTML existente incluído com o ASP.NET MVC framework.</span><span class="sxs-lookup"><span data-stu-id="2f4c3-111">In the first part of this tutorial, I describe some of the existing HTML Helpers included with the ASP.NET MVC framework.</span></span> <span data-ttu-id="2f4c3-112">Em seguida, descrevo dois métodos de criação de auxiliares de HTML personalizados: explicarei como criar auxiliares de HTML personalizados com a criação de um método estático e criando um método de extensão.</span><span class="sxs-lookup"><span data-stu-id="2f4c3-112">Next, I describe two methods of creating custom HTML Helpers: I explain how to create custom HTML Helpers by creating a static method and by creating an extension method.</span></span>

## <a name="understanding-html-helpers"></a><span data-ttu-id="2f4c3-113">Noções básicas sobre os auxiliares HTML</span><span class="sxs-lookup"><span data-stu-id="2f4c3-113">Understanding HTML Helpers</span></span>

<span data-ttu-id="2f4c3-114">Um auxiliar HTML é apenas um método que retorna uma cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="2f4c3-114">An HTML Helper is just a method that returns a string.</span></span> <span data-ttu-id="2f4c3-115">A cadeia de caracteres pode representar qualquer tipo de conteúdo que você deseja.</span><span class="sxs-lookup"><span data-stu-id="2f4c3-115">The string can represent any type of content that you want.</span></span> <span data-ttu-id="2f4c3-116">Por exemplo, você pode usar os auxiliares HTML para processar marcas HTML padrão, como HTML `<input>` e `<img>` marcas.</span><span class="sxs-lookup"><span data-stu-id="2f4c3-116">For example, you can use HTML Helpers to render standard HTML tags like HTML `<input>` and `<img>` tags.</span></span> <span data-ttu-id="2f4c3-117">Você também pode usar os auxiliares HTML para renderizar o conteúdo mais complexo, como uma faixa da guia ou uma tabela HTML de dados do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="2f4c3-117">You also can use HTML Helpers to render more complex content such as a tab strip or an HTML table of database data.</span></span>

<span data-ttu-id="2f4c3-118">A estrutura ASP.NET MVC inclui o seguinte conjunto de auxiliares de HTML padrão (não é uma lista completa):</span><span class="sxs-lookup"><span data-stu-id="2f4c3-118">The ASP.NET MVC framework includes the following set of standard HTML Helpers (this is not a complete list):</span></span>

- <span data-ttu-id="2f4c3-119">Html.ActionLink()</span><span class="sxs-lookup"><span data-stu-id="2f4c3-119">Html.ActionLink()</span></span>
- <span data-ttu-id="2f4c3-120">Html.BeginForm()</span><span class="sxs-lookup"><span data-stu-id="2f4c3-120">Html.BeginForm()</span></span>
- <span data-ttu-id="2f4c3-121">Html.CheckBox()</span><span class="sxs-lookup"><span data-stu-id="2f4c3-121">Html.CheckBox()</span></span>
- <span data-ttu-id="2f4c3-122">Html.DropDownList()</span><span class="sxs-lookup"><span data-stu-id="2f4c3-122">Html.DropDownList()</span></span>
- <span data-ttu-id="2f4c3-123">Html.EndForm()</span><span class="sxs-lookup"><span data-stu-id="2f4c3-123">Html.EndForm()</span></span>
- <span data-ttu-id="2f4c3-124">Html.Hidden()</span><span class="sxs-lookup"><span data-stu-id="2f4c3-124">Html.Hidden()</span></span>
- <span data-ttu-id="2f4c3-125">Html.ListBox()</span><span class="sxs-lookup"><span data-stu-id="2f4c3-125">Html.ListBox()</span></span>
- <span data-ttu-id="2f4c3-126">Html.Password()</span><span class="sxs-lookup"><span data-stu-id="2f4c3-126">Html.Password()</span></span>
- <span data-ttu-id="2f4c3-127">Html.RadioButton()</span><span class="sxs-lookup"><span data-stu-id="2f4c3-127">Html.RadioButton()</span></span>
- <span data-ttu-id="2f4c3-128">Html.TextArea()</span><span class="sxs-lookup"><span data-stu-id="2f4c3-128">Html.TextArea()</span></span>
- <span data-ttu-id="2f4c3-129">Html.TextBox()</span><span class="sxs-lookup"><span data-stu-id="2f4c3-129">Html.TextBox()</span></span>

<span data-ttu-id="2f4c3-130">Por exemplo, considere o formulário na listagem 1.</span><span class="sxs-lookup"><span data-stu-id="2f4c3-130">For example, consider the form in Listing 1.</span></span> <span data-ttu-id="2f4c3-131">Esse formulário é renderizado com a Ajuda de dois dos auxiliares HTML padrão (veja a Figura 1).</span><span class="sxs-lookup"><span data-stu-id="2f4c3-131">This form is rendered with the help of two of the standard HTML Helpers (see Figure 1).</span></span> <span data-ttu-id="2f4c3-132">O formulário usa o `Html.BeginForm()` e `Html.TextBox()` métodos auxiliares para processar um formulário HTML simples.</span><span class="sxs-lookup"><span data-stu-id="2f4c3-132">This form uses the `Html.BeginForm()` and `Html.TextBox()` Helper methods to render a simple HTML form.</span></span>


<span data-ttu-id="2f4c3-133">[![Página renderizada com auxiliares de HTML](creating-custom-html-helpers-cs/_static/image2.png)](creating-custom-html-helpers-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="2f4c3-133">[![Page rendered with HTML Helpers](creating-custom-html-helpers-cs/_static/image2.png)](creating-custom-html-helpers-cs/_static/image1.png)</span></span>

<span data-ttu-id="2f4c3-134">**Figura 01**: página renderizada com auxiliares HTML ([clique para exibir a imagem em tamanho normal](creating-custom-html-helpers-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="2f4c3-134">**Figure 01**: Page rendered with HTML Helpers ([Click to view full-size image](creating-custom-html-helpers-cs/_static/image3.png))</span></span>


<span data-ttu-id="2f4c3-135">**Listagem 1 – `Views\Home\Index.aspx`**</span><span class="sxs-lookup"><span data-stu-id="2f4c3-135">**Listing 1 – `Views\Home\Index.aspx`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample1.aspx)]

<span data-ttu-id="2f4c3-136">O método auxiliar Html.BeginForm() é usado para criar o HTML de abertura e fechamento `<form>` marcas.</span><span class="sxs-lookup"><span data-stu-id="2f4c3-136">The Html.BeginForm() Helper method is used to create the opening and closing HTML `<form>` tags.</span></span> <span data-ttu-id="2f4c3-137">Observe que o `Html.BeginForm()` método é chamado dentro do uso de uma instrução.</span><span class="sxs-lookup"><span data-stu-id="2f4c3-137">Notice that the `Html.BeginForm()` method is called within a using statement.</span></span> <span data-ttu-id="2f4c3-138">O uso da instrução garante que o `<form>` marca é fechada ao final da usando bloco.</span><span class="sxs-lookup"><span data-stu-id="2f4c3-138">The using statement ensures that the `<form>` tag gets closed at the end of the using block.</span></span>

<span data-ttu-id="2f4c3-139">Se você preferir, em vez de criar um usando o bloco, você pode chamar o método auxiliar Html.EndForm() para fechar o `<form>` marca.</span><span class="sxs-lookup"><span data-stu-id="2f4c3-139">If you prefer, instead of creating a using block, you can call the Html.EndForm() Helper method to close the `<form>` tag.</span></span> <span data-ttu-id="2f4c3-140">Use a abordagem à criação de abertura e fechamento `<form>` marca parece mais intuitiva para você.</span><span class="sxs-lookup"><span data-stu-id="2f4c3-140">Use whichever approach to creating an opening and closing `<form>` tag that seems most intuitive to you.</span></span>

<span data-ttu-id="2f4c3-141">O `Html.TextBox()` métodos auxiliares são utilizados na listagem 1 para renderizar HTML `<input>` marcas.</span><span class="sxs-lookup"><span data-stu-id="2f4c3-141">The `Html.TextBox()` Helper methods are used in Listing 1 to render HTML `<input>` tags.</span></span> <span data-ttu-id="2f4c3-142">Se você selecionar Exibir código-fonte no seu navegador, em seguida, você verá o código-fonte HTML na listagem 2.</span><span class="sxs-lookup"><span data-stu-id="2f4c3-142">If you select view source in your browser then you see the HTML source in Listing 2.</span></span> <span data-ttu-id="2f4c3-143">Observe que a fonte contém marcas HTML padrão.</span><span class="sxs-lookup"><span data-stu-id="2f4c3-143">Notice that the source contains standard HTML tags.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2f4c3-144">Observe que o `Html.TextBox()`-HTML auxiliar é renderizado com `<%= %>` marcas em vez de `<% %>` marcas.</span><span class="sxs-lookup"><span data-stu-id="2f4c3-144">notice that the `Html.TextBox()`-HTML Helper is rendered with `<%= %>` tags instead of `<% %>` tags.</span></span> <span data-ttu-id="2f4c3-145">Se você não incluir o sinal de igual, nada é renderizado no navegador.</span><span class="sxs-lookup"><span data-stu-id="2f4c3-145">If you don't include the equal sign, then nothing gets rendered to the browser.</span></span>

<span data-ttu-id="2f4c3-146">O ASP.NET MVC framework contém um conjunto pequeno de auxiliares.</span><span class="sxs-lookup"><span data-stu-id="2f4c3-146">The ASP.NET MVC framework contains a small set of helpers.</span></span> <span data-ttu-id="2f4c3-147">Provavelmente, você precisará estender a estrutura do MVC com auxiliares de HTML personalizados.</span><span class="sxs-lookup"><span data-stu-id="2f4c3-147">Most likely, you will need to extend the MVC framework with custom HTML Helpers.</span></span> <span data-ttu-id="2f4c3-148">O restante deste tutorial, você aprenderá dois métodos de criação de auxiliares de HTML personalizados.</span><span class="sxs-lookup"><span data-stu-id="2f4c3-148">In the remainder of this tutorial, you learn two methods of creating custom HTML Helpers.</span></span>

<span data-ttu-id="2f4c3-149">**Listagem 2 – `Index.aspx Source`**</span><span class="sxs-lookup"><span data-stu-id="2f4c3-149">**Listing 2 – `Index.aspx Source`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample2.aspx)]

### <a name="creating-html-helpers-with-static-methods"></a><span data-ttu-id="2f4c3-150">Criação de auxiliares de HTML com métodos estáticos</span><span class="sxs-lookup"><span data-stu-id="2f4c3-150">Creating HTML Helpers with Static Methods</span></span>

<span data-ttu-id="2f4c3-151">A maneira mais fácil de criar um novo auxiliar de HTML é criar um método estático que retorna uma cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="2f4c3-151">The easiest way to create a new HTML Helper is to create a static method that returns a string.</span></span> <span data-ttu-id="2f4c3-152">Por exemplo, imagine que você optar por criar um novo auxiliar de HTML que renderiza uma marca HTML `<label>` marca.</span><span class="sxs-lookup"><span data-stu-id="2f4c3-152">Imagine, for example, that you decide to create a new HTML Helper that renders an HTML `<label>` tag.</span></span> <span data-ttu-id="2f4c3-153">Você pode usar a classe na listagem 2 para renderizar um `<label>` .</span><span class="sxs-lookup"><span data-stu-id="2f4c3-153">You can use the class in Listing 2 to render a `<label>` .</span></span>

<span data-ttu-id="2f4c3-154">**Listagem 2 – `Helpers\LabelHelper.cs`**</span><span class="sxs-lookup"><span data-stu-id="2f4c3-154">**Listing 2 – `Helpers\LabelHelper.cs`**</span></span>

[!code-csharp[Main](creating-custom-html-helpers-cs/samples/sample3.cs)]

<span data-ttu-id="2f4c3-155">Não há nada de especial sobre a classe na listagem 2.</span><span class="sxs-lookup"><span data-stu-id="2f4c3-155">There is nothing special about the class in Listing 2.</span></span> <span data-ttu-id="2f4c3-156">O `Label()` método simplesmente retorna uma cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="2f4c3-156">The `Label()` method simply returns a string.</span></span>

<span data-ttu-id="2f4c3-157">O modo de exibição do índice modificado na listagem 3 usa o `LabelHelper` para renderizar HTML `<label>` marcas.</span><span class="sxs-lookup"><span data-stu-id="2f4c3-157">The modified Index view in Listing 3 uses the `LabelHelper` to render HTML `<label>` tags.</span></span> <span data-ttu-id="2f4c3-158">Observe que o modo de exibição inclui um `<%@ imports %>` diretiva importa o `Application1.Helpers` namespace.</span><span class="sxs-lookup"><span data-stu-id="2f4c3-158">Notice that the view includes an `<%@ imports %>` directive that imports the `Application1.Helpers` namespace.</span></span>

<span data-ttu-id="2f4c3-159">**Listagem 2 – `Views\Home\Index2.aspx`**</span><span class="sxs-lookup"><span data-stu-id="2f4c3-159">**Listing 2 – `Views\Home\Index2.aspx`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample4.aspx)]

### <a name="creating-html-helpers-with-extension-methods"></a><span data-ttu-id="2f4c3-160">Criando auxiliares de HTML com os métodos de extensão</span><span class="sxs-lookup"><span data-stu-id="2f4c3-160">Creating HTML Helpers with Extension Methods</span></span>

<span data-ttu-id="2f4c3-161">Se você quiser criar auxiliares HTML que funcionam assim como os auxiliares HTML padrão incluído na estrutura do ASP.NET MVC e em seguida, você precisará criar métodos de extensão.</span><span class="sxs-lookup"><span data-stu-id="2f4c3-161">If you want to create HTML Helpers that work just like the standard HTML Helpers included in the ASP.NET MVC framework then you need to create extension methods.</span></span> <span data-ttu-id="2f4c3-162">Métodos de extensão permitem adicionar novos métodos a uma classe existente.</span><span class="sxs-lookup"><span data-stu-id="2f4c3-162">Extension methods enable you to add new methods to an existing class.</span></span> <span data-ttu-id="2f4c3-163">Ao criar um método auxiliar HTML, você adicionar novos métodos à classe HtmlHelper representada pela propriedade de Html do modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="2f4c3-163">When creating an HTML Helper method, you add new methods to the HtmlHelper class represented by a view's Html property.</span></span>

<span data-ttu-id="2f4c3-164">A classe na listagem 3 adiciona um método de extensão para o `HtmlHelper` classe denominada `Label()`.</span><span class="sxs-lookup"><span data-stu-id="2f4c3-164">The class in Listing 3 adds an extension method to the `HtmlHelper` class named `Label()`.</span></span> <span data-ttu-id="2f4c3-165">Há algumas coisas que você deve observar sobre essa classe.</span><span class="sxs-lookup"><span data-stu-id="2f4c3-165">There are a couple of things that you should notice about this class.</span></span> <span data-ttu-id="2f4c3-166">Primeiro, observe que a classe é uma classe estática.</span><span class="sxs-lookup"><span data-stu-id="2f4c3-166">First, notice that the class is a static class.</span></span> <span data-ttu-id="2f4c3-167">Você deve definir um método de extensão com uma classe estática.</span><span class="sxs-lookup"><span data-stu-id="2f4c3-167">You must define an extension method with a static class.</span></span>

<span data-ttu-id="2f4c3-168">Em segundo lugar, observe que o primeiro parâmetro do `Label()` método é precedido pela palavra-chave `this`.</span><span class="sxs-lookup"><span data-stu-id="2f4c3-168">Second, notice that the first parameter of the `Label()` method is preceded by the keyword `this`.</span></span> <span data-ttu-id="2f4c3-169">O primeiro parâmetro de um método de extensão indica a classe que estende o método de extensão.</span><span class="sxs-lookup"><span data-stu-id="2f4c3-169">The first parameter of an extension method indicates the class that the extension method extends.</span></span>

<span data-ttu-id="2f4c3-170">**Listagem 3 – `Helpers\LabelExtensions.cs`**</span><span class="sxs-lookup"><span data-stu-id="2f4c3-170">**Listing 3 – `Helpers\LabelExtensions.cs`**</span></span>

[!code-csharp[Main](creating-custom-html-helpers-cs/samples/sample5.cs)]

<span data-ttu-id="2f4c3-171">Depois de criar um método de extensão e crie seu aplicativo com êxito, o método de extensão é exibida no Visual Studio Intellisense, como todos os outros métodos de uma classe (consulte a Figura 2).</span><span class="sxs-lookup"><span data-stu-id="2f4c3-171">After you create an extension method, and build your application successfully, the extension method appears in Visual Studio Intellisense like all of the other methods of a class (see Figure 2).</span></span> <span data-ttu-id="2f4c3-172">A única diferença é que essa extensão métodos aparecem com um símbolo especial ao lado deles (um ícone de uma seta para baixo).</span><span class="sxs-lookup"><span data-stu-id="2f4c3-172">The only difference is that extension methods appear with a special symbol next to them (an icon of a downward arrow).</span></span>


<span data-ttu-id="2f4c3-173">[![Usando o método de extensão Html.Label()](creating-custom-html-helpers-cs/_static/image5.png)](creating-custom-html-helpers-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="2f4c3-173">[![Using the Html.Label() extension method](creating-custom-html-helpers-cs/_static/image5.png)](creating-custom-html-helpers-cs/_static/image4.png)</span></span>

<span data-ttu-id="2f4c3-174">**Figura 02**: usando o método de extensão Html.Label() ([clique para exibir a imagem em tamanho normal](creating-custom-html-helpers-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="2f4c3-174">**Figure 02**: Using the Html.Label() extension method  ([Click to view full-size image](creating-custom-html-helpers-cs/_static/image6.png))</span></span>


<span data-ttu-id="2f4c3-175">O modo de exibição do índice modificado na listagem 4 usa o método de extensão Html.Label() para processar todos os seus `<label>` marcas.</span><span class="sxs-lookup"><span data-stu-id="2f4c3-175">The modified Index view in Listing 4 uses the Html.Label() extension method to render all of its `<label>` tags.</span></span>

<span data-ttu-id="2f4c3-176">**Listagem 4 – `Views\Home\Index3.aspx`**</span><span class="sxs-lookup"><span data-stu-id="2f4c3-176">**Listing 4 – `Views\Home\Index3.aspx`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample6.aspx)]

## <a name="summary"></a><span data-ttu-id="2f4c3-177">Resumo</span><span class="sxs-lookup"><span data-stu-id="2f4c3-177">Summary</span></span>

<span data-ttu-id="2f4c3-178">Neste tutorial, você aprendeu a dois métodos de criação de auxiliares de HTML personalizados.</span><span class="sxs-lookup"><span data-stu-id="2f4c3-178">In this tutorial, you learned two methods of creating custom HTML Helpers.</span></span> <span data-ttu-id="2f4c3-179">Primeiro, você aprendeu a criar um personalizado `Label()` auxiliar HTML com a criação de um método estático que retorna uma cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="2f4c3-179">First, you learned how to create a custom `Label()` HTML Helper by creating a static method that returns a string.</span></span> <span data-ttu-id="2f4c3-180">Em seguida, você aprendeu a criar um personalizado `Label()` método auxiliar HTML, criando um método de extensão no `HtmlHelper` classe.</span><span class="sxs-lookup"><span data-stu-id="2f4c3-180">Next, you learned how to create a custom `Label()` HTML Helper method by creating an extension method on the `HtmlHelper` class.</span></span>

<span data-ttu-id="2f4c3-181">Neste tutorial, concentrei-me sobre a criação de um método auxiliar HTML extremamente simple.</span><span class="sxs-lookup"><span data-stu-id="2f4c3-181">In this tutorial, I focused on building an extremely simple HTML Helper method.</span></span> <span data-ttu-id="2f4c3-182">Observe que um auxiliar de HTML podem ser tão complicado como você deseja.</span><span class="sxs-lookup"><span data-stu-id="2f4c3-182">Realize that an HTML Helper can be as complicated as you want.</span></span> <span data-ttu-id="2f4c3-183">Você pode criar auxiliares de HTML que processam conteúdo avançado, como modos de exibição de árvore, menus ou tabelas de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="2f4c3-183">You can build HTML Helpers that render rich content such as tree views, menus, or tables of database data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="2f4c3-184">[Anterior](asp-net-mvc-views-overview-cs.md)
> [Próximo](using-the-tagbuilder-class-to-build-html-helpers-cs.md)</span><span class="sxs-lookup"><span data-stu-id="2f4c3-184">[Previous](asp-net-mvc-views-overview-cs.md)
[Next](using-the-tagbuilder-class-to-build-html-helpers-cs.md)</span></span>

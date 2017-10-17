---
title: "Referência de sintaxe Razor ASP.NET Core"
author: rick-anderson
description: "Saiba mais sobre a sintaxe de marcação Razor para inserir o código de servidor em páginas da Web."
keywords: Diretivas do ASP.NET Core, Razor, Razor
ms.author: riande
manager: wpickett
ms.date: 10/18/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/razor
ms.openlocfilehash: 743c42b26c62d0e24b5d5b487b3154bc249fcff4
ms.sourcegitcommit: a873f862c8e68b2cf2998aaed3dddd93eeba9e0f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2017
---
# <a name="razor-syntax-for-aspnet-core"></a><span data-ttu-id="d808f-104">Sintaxe do Razor para o ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d808f-104">Razor syntax for ASP.NET Core</span></span>

<span data-ttu-id="d808f-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT), [Luke Latham](https://github.com/guardrex), [Taylor Mullen](https://twitter.com/ntaylormullen), e [Dan Vicarel](https://github.com/Rabadash8820)</span><span class="sxs-lookup"><span data-stu-id="d808f-105">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Luke Latham](https://github.com/guardrex),  [Taylor Mullen](https://twitter.com/ntaylormullen), and [Dan Vicarel](https://github.com/Rabadash8820)</span></span>

<span data-ttu-id="d808f-106">Razor é uma sintaxe de marcação para inserir o código de servidor em páginas da Web.</span><span class="sxs-lookup"><span data-stu-id="d808f-106">Razor is a markup syntax for embedding server-based code into webpages.</span></span> <span data-ttu-id="d808f-107">A sintaxe do Razor consiste Razor marcação, c# e HTML.</span><span class="sxs-lookup"><span data-stu-id="d808f-107">The Razor syntax consists of Razor markup, C#, and HTML.</span></span> <span data-ttu-id="d808f-108">Arquivos que contêm Razor geralmente têm um *. cshtml* extensão de arquivo.</span><span class="sxs-lookup"><span data-stu-id="d808f-108">Files containing Razor generally have a *.cshtml* file extension.</span></span>

## <a name="rendering-html"></a><span data-ttu-id="d808f-109">Renderização HTML</span><span class="sxs-lookup"><span data-stu-id="d808f-109">Rendering HTML</span></span>

<span data-ttu-id="d808f-110">O idioma do Razor padrão é HTML.</span><span class="sxs-lookup"><span data-stu-id="d808f-110">The default Razor language is HTML.</span></span> <span data-ttu-id="d808f-111">Renderização HTML da marcação Razor não é diferente de renderização HTML de um arquivo HTML.</span><span class="sxs-lookup"><span data-stu-id="d808f-111">Rendering HTML from Razor markup is no different than rendering HTML from an HTML file.</span></span>  <span data-ttu-id="d808f-112">A marcação HTML em *. cshtml* arquivos do Razor é processado pelo servidor inalterado.</span><span class="sxs-lookup"><span data-stu-id="d808f-112">HTML markup in *.cshtml* Razor files is rendered by the server unchanged.</span></span>

## <a name="razor-syntax"></a><span data-ttu-id="d808f-113">Sintaxe do Razor</span><span class="sxs-lookup"><span data-stu-id="d808f-113">Razor syntax</span></span>

<span data-ttu-id="d808f-114">Razor suporta c# e usa o `@` símbolo para fazer a transição de HTML para c#.</span><span class="sxs-lookup"><span data-stu-id="d808f-114">Razor supports C# and uses the `@` symbol to transition from HTML to C#.</span></span> <span data-ttu-id="d808f-115">Razor avalia expressões c# e renderiza-los na saída HTML.</span><span class="sxs-lookup"><span data-stu-id="d808f-115">Razor evaluates C# expressions and renders them in the HTML output.</span></span>

<span data-ttu-id="d808f-116">Quando um `@` símbolo é seguido por um [Razor reservados a palavra-chave](#razor-reserved-keywords), ele faz a transição para marcação específica do Razor.</span><span class="sxs-lookup"><span data-stu-id="d808f-116">When an `@` symbol is followed by a [Razor reserved keyword](#razor-reserved-keywords), it transitions into Razor-specific markup.</span></span> <span data-ttu-id="d808f-117">Caso contrário, ela faz a transição em c# simples.</span><span class="sxs-lookup"><span data-stu-id="d808f-117">Otherwise, it transitions into plain C#.</span></span>

<span data-ttu-id="d808f-118">Para escape um `@` símbolo na marcação Razor, usar um segundo `@` símbolo:</span><span class="sxs-lookup"><span data-stu-id="d808f-118">To escape an `@` symbol in Razor markup, use a second `@` symbol:</span></span>

```cshtml
<p>@@Username</p>
```

<span data-ttu-id="d808f-119">O código é renderizado em HTML com um único `@` símbolo:</span><span class="sxs-lookup"><span data-stu-id="d808f-119">The code is rendered in HTML with a single `@` symbol:</span></span>

```html
<p>@Username</p>
```

<span data-ttu-id="d808f-120">Atributos HTML e o conteúdo que contém endereços de email não tratam o `@` símbolo como um caractere de transição.</span><span class="sxs-lookup"><span data-stu-id="d808f-120">HTML attributes and content containing email addresses don't treat the `@` symbol as a transition character.</span></span> <span data-ttu-id="d808f-121">Os endereços de email no exemplo a seguir são inalterados por Razor análise:</span><span class="sxs-lookup"><span data-stu-id="d808f-121">The email addresses in the following example are untouched by Razor parsing:</span></span>

```cshtml
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
```

## <a name="implicit-razor-expressions"></a><span data-ttu-id="d808f-122">Expressões implícitas do Razor</span><span class="sxs-lookup"><span data-stu-id="d808f-122">Implicit Razor expressions</span></span>

<span data-ttu-id="d808f-123">As expressões implícitas Razor iniciar com `@` seguido do código do c#:</span><span class="sxs-lookup"><span data-stu-id="d808f-123">Implicit Razor expressions start with `@` followed by C# code:</span></span>

```cshtml
<p>@DateTime.Now</p>
<p>@DateTime.IsLeapYear(2016)</p>
```

<span data-ttu-id="d808f-124">Com exceção do c# `await` palavra-chave, expressões implícitas não devem conter espaços.</span><span class="sxs-lookup"><span data-stu-id="d808f-124">With the exception of the C# `await` keyword, implicit expressions must not contain spaces.</span></span> <span data-ttu-id="d808f-125">Se a instrução c# tem uma terminação clara, espaços podem misturar:</span><span class="sxs-lookup"><span data-stu-id="d808f-125">If the C# statement has a clear ending, spaces can be intermingled:</span></span>

```cshtml
<p>@await DoSomething("hello", "world")</p>
```

<span data-ttu-id="d808f-126">Expressões implícitas **não é possível** contêm c# genéricos, como os caracteres dentro dos colchetes (`<>`) são interpretados como uma marca HTML.</span><span class="sxs-lookup"><span data-stu-id="d808f-126">Implicit expressions **cannot** contain C# generics, as the characters inside the brackets (`<>`) are interpreted as an HTML tag.</span></span> <span data-ttu-id="d808f-127">O código a seguir é **não** válido:</span><span class="sxs-lookup"><span data-stu-id="d808f-127">The following code is **not** valid:</span></span>

```cshtml
<p>@GenericMethod<int>()</p>
```

<span data-ttu-id="d808f-128">O código anterior gera um erro do compilador semelhante a uma das seguintes opções:</span><span class="sxs-lookup"><span data-stu-id="d808f-128">The preceding code generates a compiler error similar to one of the following:</span></span>

 * <span data-ttu-id="d808f-129">O elemento "int" não foi fechado.</span><span class="sxs-lookup"><span data-stu-id="d808f-129">The "int" element was not closed.</span></span>  <span data-ttu-id="d808f-130">Todos os elementos devem ser de fechamento automático ou ter uma correspondência de marca de fim.</span><span class="sxs-lookup"><span data-stu-id="d808f-130">All elements must be either self-closing or have a matching end tag.</span></span>
 *  <span data-ttu-id="d808f-131">Não é possível converter o grupo de métodos 'GenericMethod' para 'object' de tipo não delegado.</span><span class="sxs-lookup"><span data-stu-id="d808f-131">Cannot convert method group 'GenericMethod' to non-delegate type 'object'.</span></span> <span data-ttu-id="d808f-132">Você pretendia chamar o método?'</span><span class="sxs-lookup"><span data-stu-id="d808f-132">Did you intend to invoke the method?\`</span></span> 
 
<span data-ttu-id="d808f-133">Chamadas de método genérico devem ser encapsuladas em um [expressão Razor explícita](#explicit-razor-expressions) ou um [bloco de código Razor](#razor-code-blocks).</span><span class="sxs-lookup"><span data-stu-id="d808f-133">Generic method calls must be wrapped in an [explicit Razor expression](#explicit-razor-expressions) or a [Razor code block](#razor-code-blocks).</span></span> <span data-ttu-id="d808f-134">Essa restrição não se aplica a *. vbhtml* Razor arquivos porque a sintaxe do Visual Basic coloca os parâmetros de tipo genérico em vez de colchetes entre parênteses.</span><span class="sxs-lookup"><span data-stu-id="d808f-134">This restriction doesn't apply to *.vbhtml* Razor files because Visual Basic syntax places parentheses around generic type parameters instead of brackets.</span></span>

## <a name="explicit-razor-expressions"></a><span data-ttu-id="d808f-135">Expressões explícitas Razor</span><span class="sxs-lookup"><span data-stu-id="d808f-135">Explicit Razor expressions</span></span>

<span data-ttu-id="d808f-136">As expressões explícitas Razor consistem em um `@` símbolo com parênteses equilibrada.</span><span class="sxs-lookup"><span data-stu-id="d808f-136">Explicit Razor expressions consist of an `@` symbol with balanced parenthesis.</span></span> <span data-ttu-id="d808f-137">Para renderizar a hora da última semana, a seguinte marcação Razor é usada:</span><span class="sxs-lookup"><span data-stu-id="d808f-137">To render last week's time, the following Razor markup is used:</span></span>

```cshtml
<p>Last week this time: @(DateTime.Now - TimeSpan.FromDays(7))</p>
```

<span data-ttu-id="d808f-138">Qualquer conteúdo dentro do `@()` parênteses é avaliado e renderizado para a saída.</span><span class="sxs-lookup"><span data-stu-id="d808f-138">Any content within the `@()` parenthesis is evaluated and rendered to the output.</span></span>

<span data-ttu-id="d808f-139">Expressões implícitas, descritas na seção anterior, geralmente não podem conter espaços.</span><span class="sxs-lookup"><span data-stu-id="d808f-139">Implicit expressions, described in the previous section, generally can't contain spaces.</span></span> <span data-ttu-id="d808f-140">No código a seguir, uma semana não é subtraída da hora atual:</span><span class="sxs-lookup"><span data-stu-id="d808f-140">In the following code, one week isn't subtracted from the current time:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact.cshtml?range=17)]

<span data-ttu-id="d808f-141">O código processa o HTML a seguir:</span><span class="sxs-lookup"><span data-stu-id="d808f-141">The code renders the following HTML:</span></span>

```html
<p>Last week: 7/7/2016 4:39:52 PM - TimeSpan.FromDays(7)</p>
```

<span data-ttu-id="d808f-142">As expressões explícitas podem ser usadas para concatenação de texto com um resultado da expressão:</span><span class="sxs-lookup"><span data-stu-id="d808f-142">Explicit expressions can be used to concatenate text with an expression result:</span></span>

```cshtml
@{
    var joe = new Person("Joe", 33);
}

<p>Age@(joe.Age)</p>
```

<span data-ttu-id="d808f-143">Sem expressão explícita, `<p>Age@joe.Age</p>` é tratado como um endereço de email, e `<p>Age@joe.Age</p>` é renderizado.</span><span class="sxs-lookup"><span data-stu-id="d808f-143">Without the explicit expression, `<p>Age@joe.Age</p>` is treated as an email address, and `<p>Age@joe.Age</p>` is rendered.</span></span> <span data-ttu-id="d808f-144">Quando gravado como uma expressão explícita, `<p>Age33</p>` é renderizado.</span><span class="sxs-lookup"><span data-stu-id="d808f-144">When written as an explicit expression, `<p>Age33</p>` is rendered.</span></span>


<span data-ttu-id="d808f-145">Expressões explícitas podem ser usadas para processar a saída de métodos genéricos em *. cshtml* arquivos.</span><span class="sxs-lookup"><span data-stu-id="d808f-145">Explicit expressions can be used to render output from generic methods in *.cshtml* files.</span></span> <span data-ttu-id="d808f-146">Em uma expressão implícita, os caracteres dentro dos colchetes (`<>`) são interpretados como uma marca HTML.</span><span class="sxs-lookup"><span data-stu-id="d808f-146">In an implicit expression, the characters inside the brackets (`<>`) are interpreted as an HTML tag.</span></span> <span data-ttu-id="d808f-147">É a seguinte marcação **não** Razor válido:</span><span class="sxs-lookup"><span data-stu-id="d808f-147">The following markup is **not** valid Razor:</span></span>

```cshtml
<p>@GenericMethod<int>()</p>
```

<span data-ttu-id="d808f-148">O código anterior gera um erro do compilador semelhante a uma das seguintes opções:</span><span class="sxs-lookup"><span data-stu-id="d808f-148">The preceding code generates a compiler error similar to one of the following:</span></span>

 * <span data-ttu-id="d808f-149">O elemento "int" não foi fechado.</span><span class="sxs-lookup"><span data-stu-id="d808f-149">The "int" element was not closed.</span></span>  <span data-ttu-id="d808f-150">Todos os elementos devem ser de fechamento automático ou ter uma correspondência de marca de fim.</span><span class="sxs-lookup"><span data-stu-id="d808f-150">All elements must be either self-closing or have a matching end tag.</span></span>
 *  <span data-ttu-id="d808f-151">Não é possível converter o grupo de métodos 'GenericMethod' para 'object' de tipo não delegado.</span><span class="sxs-lookup"><span data-stu-id="d808f-151">Cannot convert method group 'GenericMethod' to non-delegate type 'object'.</span></span> <span data-ttu-id="d808f-152">Você pretendia chamar o método?'</span><span class="sxs-lookup"><span data-stu-id="d808f-152">Did you intend to invoke the method?\`</span></span> 
 
 <span data-ttu-id="d808f-153">A marcação a seguir mostra a gravação de maneira correta esse código.</span><span class="sxs-lookup"><span data-stu-id="d808f-153">The following markup shows the correct way write this code.</span></span>  <span data-ttu-id="d808f-154">O código é escrito como uma expressão explícita:</span><span class="sxs-lookup"><span data-stu-id="d808f-154">The code is written as an explicit expression:</span></span>

```cshtml
<p>@(GenericMethod<int>())</p>
```

<span data-ttu-id="d808f-155">Observação: essa restrição não se aplica a *. vbhtml* arquivos do Razor.</span><span class="sxs-lookup"><span data-stu-id="d808f-155">Note: this restriction doesn't apply to *.vbhtml* Razor files.</span></span>  <span data-ttu-id="d808f-156">Com *. vbhtml* arquivos Razor, sintaxe do Visual Basic coloca os parâmetros de tipo genérico em vez de colchetes entre parênteses.</span><span class="sxs-lookup"><span data-stu-id="d808f-156">With *.vbhtml* Razor files, Visual Basic syntax places parentheses around generic type parameters instead of brackets.</span></span>

## <a name="expression-encoding"></a><span data-ttu-id="d808f-157">Codificação de expressão</span><span class="sxs-lookup"><span data-stu-id="d808f-157">Expression encoding</span></span>

<span data-ttu-id="d808f-158">Expressões em c# que são avaliadas como uma cadeia de caracteres estão codificada em HTML.</span><span class="sxs-lookup"><span data-stu-id="d808f-158">C# expressions that evaluate to a string are HTML encoded.</span></span> <span data-ttu-id="d808f-159">Expressões em c# que avaliam `IHtmlContent` são renderizados diretamente por meio `IHtmlContent.WriteTo`.</span><span class="sxs-lookup"><span data-stu-id="d808f-159">C# expressions that evaluate to `IHtmlContent` are rendered directly through `IHtmlContent.WriteTo`.</span></span> <span data-ttu-id="d808f-160">Expressões em c# que não retornam `IHtmlContent` são convertidos em uma cadeia de caracteres por `ToString` e codificado antes que eles são renderizados.</span><span class="sxs-lookup"><span data-stu-id="d808f-160">C# expressions that don't evaluate to `IHtmlContent` are converted to a string by `ToString` and encoded before they're rendered.</span></span>

```cshtml
@("<span>Hello World</span>")
```

<span data-ttu-id="d808f-161">O código processa o HTML a seguir:</span><span class="sxs-lookup"><span data-stu-id="d808f-161">The code renders the following HTML:</span></span>

```html
&lt;span&gt;Hello World&lt;/span&gt;
```

<span data-ttu-id="d808f-162">O HTML é mostrado no navegador como:</span><span class="sxs-lookup"><span data-stu-id="d808f-162">The HTML is shown in the browser as:</span></span>

```
<span>Hello World</span>
```

<span data-ttu-id="d808f-163">`HtmlHelper.Raw`saída não é codificada mas renderizada como marcação HTML.</span><span class="sxs-lookup"><span data-stu-id="d808f-163">`HtmlHelper.Raw` output isn't encoded but rendered as HTML markup.</span></span>

> [!WARNING]
> <span data-ttu-id="d808f-164">Usando `HtmlHelper.Raw` usuário unsanitized a entrada é um risco de segurança.</span><span class="sxs-lookup"><span data-stu-id="d808f-164">Using `HtmlHelper.Raw` on unsanitized user input is a security risk.</span></span> <span data-ttu-id="d808f-165">Entrada do usuário pode conter outras explorações ou JavaScript mal-intencionados.</span><span class="sxs-lookup"><span data-stu-id="d808f-165">User input might contain malicious JavaScript or other exploits.</span></span> <span data-ttu-id="d808f-166">Limpeza de entrada do usuário é difícil.</span><span class="sxs-lookup"><span data-stu-id="d808f-166">Sanitizing user input is difficult.</span></span> <span data-ttu-id="d808f-167">Evite usar `HtmlHelper.Raw` com entrada do usuário.</span><span class="sxs-lookup"><span data-stu-id="d808f-167">Avoid using `HtmlHelper.Raw` with user input.</span></span>

```cshtml
@Html.Raw("<span>Hello World</span>")
```

<span data-ttu-id="d808f-168">O código processa o HTML a seguir:</span><span class="sxs-lookup"><span data-stu-id="d808f-168">The code renders the following HTML:</span></span>

```html
<span>Hello World</span>
```

## <a name="razor-code-blocks"></a><span data-ttu-id="d808f-169">Blocos de código Razor</span><span class="sxs-lookup"><span data-stu-id="d808f-169">Razor code blocks</span></span>

<span data-ttu-id="d808f-170">Blocos de código Razor iniciar com `@` e são delimitados por `{}`.</span><span class="sxs-lookup"><span data-stu-id="d808f-170">Razor code blocks start with `@` and are enclosed by `{}`.</span></span> <span data-ttu-id="d808f-171">Ao contrário das expressões, o código c# dentro de blocos de código não está processado.</span><span class="sxs-lookup"><span data-stu-id="d808f-171">Unlike expressions, C# code inside code blocks isn't rendered.</span></span> <span data-ttu-id="d808f-172">Blocos de código e expressões em uma exibição compartilhem o mesmo escopo e são definidas em ordem:</span><span class="sxs-lookup"><span data-stu-id="d808f-172">Code blocks and expressions in a view share the same scope and are defined in order:</span></span>

```cshtml
@{
    var quote = "The future depends on what you do today. - Mahatma Gandhi";
}

<p>@quote</p>

@{
    quote = "Hate cannot drive out hate, only love can do that. - Martin Luther King, Jr.";
}

<p>@quote</p>
```

<span data-ttu-id="d808f-173">O código processa o HTML a seguir:</span><span class="sxs-lookup"><span data-stu-id="d808f-173">The code renders the following HTML:</span></span>

```html
<p>The future depends on what you do today. - Mahatma Gandhi</p>
<p>Hate cannot drive out hate, only love can do that. - Martin Luther King, Jr.</p>
```

### <a name="implicit-transitions"></a><span data-ttu-id="d808f-174">Transições implícita</span><span class="sxs-lookup"><span data-stu-id="d808f-174">Implicit transitions</span></span>

<span data-ttu-id="d808f-175">É o idioma padrão em um bloco de código c#, mas a página Razor pode fazer a transição para HTML:</span><span class="sxs-lookup"><span data-stu-id="d808f-175">The default language in a code block is C#, but the Razor Page can transition back to HTML:</span></span>

```cshtml
@{
    var inCSharp = true;
    <p>Now in HTML, was in C# @inCSharp</p>
}
```

### <a name="explicit-delimited-transition"></a><span data-ttu-id="d808f-176">Transição delimitada explícita</span><span class="sxs-lookup"><span data-stu-id="d808f-176">Explicit delimited transition</span></span>

<span data-ttu-id="d808f-177">Para definir uma subseção de um bloco de código que deve renderizar HTML, coloque os caracteres para renderização o Razor  **\<texto >** marca:</span><span class="sxs-lookup"><span data-stu-id="d808f-177">To define a subsection of a code block that should render HTML, surround the characters for rendering with the Razor **\<text>** tag:</span></span>

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <text>Name: @person.Name</text>
}
```

<span data-ttu-id="d808f-178">Use essa abordagem para renderizar HTML que não está circundado por uma marca HTML.</span><span class="sxs-lookup"><span data-stu-id="d808f-178">Use this approach to render HTML that isn't surrounded by an HTML tag.</span></span> <span data-ttu-id="d808f-179">Sem uma marca HTML ou Razor, ocorrerá um erro de tempo de execução do Razor.</span><span class="sxs-lookup"><span data-stu-id="d808f-179">Without an HTML or Razor tag, a Razor runtime error occurs.</span></span>

<span data-ttu-id="d808f-180">O  **\<texto >** marca é útil para controlar o espaço em branco ao renderizar o conteúdo:</span><span class="sxs-lookup"><span data-stu-id="d808f-180">The **\<text>** tag is useful to control whitespace when rendering content:</span></span>

* <span data-ttu-id="d808f-181">Somente o conteúdo entre o  **\<texto >** marca é processada.</span><span class="sxs-lookup"><span data-stu-id="d808f-181">Only the content between the **\<text>** tag is rendered.</span></span> 
* <span data-ttu-id="d808f-182">Não há espaço em branco antes ou depois do  **\<texto >** marca aparece na saída HTML.</span><span class="sxs-lookup"><span data-stu-id="d808f-182">No whitespace before or after the **\<text>** tag appears in the HTML output.</span></span>

### <a name="explicit-line-transition-with-"></a><span data-ttu-id="d808f-183">Transição de linha explícita com @:</span><span class="sxs-lookup"><span data-stu-id="d808f-183">Explicit Line Transition with @:</span></span>

<span data-ttu-id="d808f-184">Para processar o restante de uma linha inteira como HTML dentro de um bloco de código, use o `@:` sintaxe:</span><span class="sxs-lookup"><span data-stu-id="d808f-184">To render the rest of an entire line as HTML inside a code block, use the `@:` syntax:</span></span>

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    @:Name: @person.Name
}
```

<span data-ttu-id="d808f-185">Sem o `@:` no código, será gerado um erro de tempo de execução do Razor.</span><span class="sxs-lookup"><span data-stu-id="d808f-185">Without the `@:` in the code,  a Razor runtime error is generated.</span></span>

<span data-ttu-id="d808f-186">Aviso: Extra `@` caracteres em um arquivo Razor podem causar causar erros de compilador em instruções posteriormente no bloco.</span><span class="sxs-lookup"><span data-stu-id="d808f-186">Warning: Extra `@` characters in a Razor file can cause  cause compiler errors at statements later in the block.</span></span> <span data-ttu-id="d808f-187">Esses erros de compilador podem ser difícil de entender porque o erro real ocorre antes do erro relatado.</span><span class="sxs-lookup"><span data-stu-id="d808f-187">These compiler errors can be difficult to understand because the actual error occurs before the reported error.</span></span>  <span data-ttu-id="d808f-188">Esse erro é comum após a combinação de várias expressões implícitas explícita em um bloco de código único.</span><span class="sxs-lookup"><span data-stu-id="d808f-188">This error is common after combining multiple implicit/explicit expressions into a single code block.</span></span>

## <a name="control-structures"></a><span data-ttu-id="d808f-189">Estruturas de controle</span><span class="sxs-lookup"><span data-stu-id="d808f-189">Control Structures</span></span>

<span data-ttu-id="d808f-190">Estruturas de controle são uma extensão dos blocos de código.</span><span class="sxs-lookup"><span data-stu-id="d808f-190">Control structures are an extension of code blocks.</span></span> <span data-ttu-id="d808f-191">Todos os aspectos dos blocos de código (em transição para marcação, embutido c#) também se aplicam às seguintes estruturas:</span><span class="sxs-lookup"><span data-stu-id="d808f-191">All aspects of code blocks (transitioning to markup, inline C#) also apply to the following structures:</span></span>

### <a name="conditionals-if-else-if-else-and-switch"></a><span data-ttu-id="d808f-192">Condicionais @if, senão se, para outro, e@switch</span><span class="sxs-lookup"><span data-stu-id="d808f-192">Conditionals @if, else if, else, and @switch</span></span>

<span data-ttu-id="d808f-193">`@if`Controla quando o código é executado:</span><span class="sxs-lookup"><span data-stu-id="d808f-193">`@if` controls when code runs:</span></span>

```cshtml
@if (value % 2 == 0)
{
    <p>The value was even.</p>
}
```

<span data-ttu-id="d808f-194">`else`e `else if` não exigem o `@` símbolo:</span><span class="sxs-lookup"><span data-stu-id="d808f-194">`else` and `else if` don't require the `@` symbol:</span></span>

```cshtml
@if (value % 2 == 0)
{
    <p>The value was even.</p>
}
else if (value >= 1337)
{
    <p>The value is large.</p>
}
else
{
    <p>The value is odd and small.</p>
}
```

<span data-ttu-id="d808f-195">A marcação a seguir mostra como usar uma instrução de opção:</span><span class="sxs-lookup"><span data-stu-id="d808f-195">The following markup shows how to use a switch statement:</span></span>

```cshtml
@switch (value)
{
    case 1:
        <p>The value is 1!</p>
        break;
    case 1337:
        <p>Your number is 1337!</p>
        break;
    default:
        <p>Your number wasn't 1 or 1337.</p>
        break;
}
```

### <a name="looping-for-foreach-while-and-do-while"></a><span data-ttu-id="d808f-196">Loop @for, @foreach, @while, e @do enquanto</span><span class="sxs-lookup"><span data-stu-id="d808f-196">Looping @for, @foreach, @while, and @do while</span></span>

<span data-ttu-id="d808f-197">Modelo HTML pode ser renderizado com instruções de controle de loop.</span><span class="sxs-lookup"><span data-stu-id="d808f-197">Templated HTML can be rendered with looping control statements.</span></span>  <span data-ttu-id="d808f-198">Para renderizar uma lista de pessoas:</span><span class="sxs-lookup"><span data-stu-id="d808f-198">To render a list of people:</span></span>

```cshtml
@{
    var people = new Person[]
    {
          new Person("Weston", 33),
          new Person("Johnathon", 41),
          ...
    };
}
```

<span data-ttu-id="d808f-199">Há suporte para as seguintes instruções de loop:</span><span class="sxs-lookup"><span data-stu-id="d808f-199">The following looping statements are supported:</span></span>

`@for`

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>
}
```

`@foreach`

```cshtml
@foreach (var person in people)
{
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>
}
```

`@while`

```cshtml
@{ var i = 0; }
@while (i < people.Length)
{
    var person = people[i];
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>

    i++;
}
```

`@do while`

```cshtml
@{ var i = 0; }
@do
{
    var person = people[i];
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>

    i++;
} while (i < people.Length);
```

### <a name="compound-using"></a><span data-ttu-id="d808f-200">Composta@using</span><span class="sxs-lookup"><span data-stu-id="d808f-200">Compound @using</span></span>

<span data-ttu-id="d808f-201">No c#, um `using` instrução é usada para garantir que um objeto é descartado.</span><span class="sxs-lookup"><span data-stu-id="d808f-201">In C#, a `using` statement is used to ensure an object is disposed.</span></span> <span data-ttu-id="d808f-202">No Razor, o mesmo mecanismo é usado para criar os auxiliares HTML que contém o conteúdo adicional.</span><span class="sxs-lookup"><span data-stu-id="d808f-202">In Razor, the same mechanism is used to create HTML Helpers that contain additional content.</span></span> <span data-ttu-id="d808f-203">No código a seguir, auxiliares HTML renderizar uma marca de formulário com o `@using` instrução:</span><span class="sxs-lookup"><span data-stu-id="d808f-203">In the following code, HTML Helpers render a form tag with the `@using` statement:</span></span>


```cshtml
@using (Html.BeginForm())
{
    <div>
        email:
        <input type="email" id="Email" value="">
        <button>Register</button>
    </div>
}
```

<span data-ttu-id="d808f-204">Ações no nível de escopo podem ser executadas com [auxiliares de marcação](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="d808f-204">Scope-level actions can be performed with [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

### <a name="try-catch-finally"></a><span data-ttu-id="d808f-205">@try, catch, finally</span><span class="sxs-lookup"><span data-stu-id="d808f-205">@try, catch, finally</span></span>

<span data-ttu-id="d808f-206">Tratamento de exceção é semelhante ao c#:</span><span class="sxs-lookup"><span data-stu-id="d808f-206">Exception handling is similar to C#:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact7.cshtml)]

### <a name="lock"></a>@lock

<span data-ttu-id="d808f-207">Razor tem a capacidade de proteger seções críticas com instruções de bloqueio:</span><span class="sxs-lookup"><span data-stu-id="d808f-207">Razor has the capability to protect critical sections with lock statements:</span></span>

```cshtml
@lock (SomeLock)
{
    // Do critical section work
}
```

### <a name="comments"></a><span data-ttu-id="d808f-208">Comentários</span><span class="sxs-lookup"><span data-stu-id="d808f-208">Comments</span></span>

<span data-ttu-id="d808f-209">Razor dá suporte a comentários c# e HTML:</span><span class="sxs-lookup"><span data-stu-id="d808f-209">Razor supports C# and HTML comments:</span></span>

```cshtml
@{
    /* C# comment */
    // Another C# comment
}
<!-- HTML comment -->
```

<span data-ttu-id="d808f-210">O código processa o HTML a seguir:</span><span class="sxs-lookup"><span data-stu-id="d808f-210">The code renders the following HTML:</span></span>

```html
<!-- HTML comment -->
```

<span data-ttu-id="d808f-211">Comentários de Razor serão removidos pelo servidor para a página da Web seja processada.</span><span class="sxs-lookup"><span data-stu-id="d808f-211">Razor comments are removed by the server before the webpage is rendered.</span></span> <span data-ttu-id="d808f-212">Razor usa `@*  *@` para delimitar os comentários.</span><span class="sxs-lookup"><span data-stu-id="d808f-212">Razor uses `@*  *@` to delimit comments.</span></span> <span data-ttu-id="d808f-213">O código a seguir é comentado, para que o servidor não processa qualquer marcação:</span><span class="sxs-lookup"><span data-stu-id="d808f-213">The following code is commented out, so the server doesn't render any markup:</span></span>

```cshtml
@*
    @{
        /* C# comment */
        // Another C# comment
    }
    <!-- HTML comment -->
*@
```

## <a name="directives"></a><span data-ttu-id="d808f-214">Diretivas</span><span class="sxs-lookup"><span data-stu-id="d808f-214">Directives</span></span>

<span data-ttu-id="d808f-215">Diretivas de Razor são representadas por expressões implícitas com os seguintes palavras-chave reservadas do `@` símbolo.</span><span class="sxs-lookup"><span data-stu-id="d808f-215">Razor directives are represented by implicit expressions with reserved keywords following the `@` symbol.</span></span> <span data-ttu-id="d808f-216">Uma diretiva geralmente altera o modo como uma exibição é analisada ou habilita a funcionalidade diferente.</span><span class="sxs-lookup"><span data-stu-id="d808f-216">A directive typically changes the way a view is parsed or enables different functionality.</span></span>

<span data-ttu-id="d808f-217">Noções básicas sobre como Razor gera código para um modo de exibição torna mais fácil de entender como funcionam as diretivas.</span><span class="sxs-lookup"><span data-stu-id="d808f-217">Understanding how Razor generates code for a view makes it easier to understand how directives work.</span></span>

[!code-html[Main](razor/sample/Views/Home/Contact8.cshtml)]

<span data-ttu-id="d808f-218">O código gera uma classe semelhante à seguinte:</span><span class="sxs-lookup"><span data-stu-id="d808f-218">The code generates a class similar to the following:</span></span>

```csharp
public class _Views_Something_cshtml : RazorPage<dynamic>
{
    public override async Task ExecuteAsync()
    {
        var output = "Getting old ain't for wimps! - Anonymous";

        WriteLiteral("/r/n<div>Quote of the Day: ");
        Write(output);
        WriteLiteral("</div>");
    }
}
```

<span data-ttu-id="d808f-219">Posteriormente neste artigo, a seção [exibindo a classe Razor c# gerada para uma exibição](#viewing-the-razor-c-class-generated-for-a-view) explica como exibir essa classe gerada.</span><span class="sxs-lookup"><span data-stu-id="d808f-219">Later in this article, the section [Viewing the Razor C# class generated for a view](#viewing-the-razor-c-class-generated-for-a-view) explains how to view this generated class.</span></span>

### <a name="using"></a>@using

<span data-ttu-id="d808f-220">O `@using` diretiva adiciona c# `using` diretiva para o modo de exibição gerado:</span><span class="sxs-lookup"><span data-stu-id="d808f-220">The `@using` directive adds the C# `using` directive to the generated view:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact9.cshtml)]

### <a name="model"></a>@model

<span data-ttu-id="d808f-221">O `@model` diretiva especifica o tipo de modelo passado para um modo de exibição:</span><span class="sxs-lookup"><span data-stu-id="d808f-221">The `@model` directive specifies the type of the model passed to a view:</span></span>

```cshtml
@model TypeNameOfModel
```

<span data-ttu-id="d808f-222">Em um aplicativo MVC do ASP.NET Core criado com contas de usuário individuais, o *Views/Account/Login.cshtml* exibição contém a declaração de modelo a seguir:</span><span class="sxs-lookup"><span data-stu-id="d808f-222">In an ASP.NET Core MVC app created with individual user accounts, the *Views/Account/Login.cshtml* view contains the following model declaration:</span></span>

```cshtml
@model LoginViewModel
```

<span data-ttu-id="d808f-223">A classe gerada herda de `RazorPage<dynamic>`:</span><span class="sxs-lookup"><span data-stu-id="d808f-223">The class generated inherits from `RazorPage<dynamic>`:</span></span>

```csharp
public class _Views_Account_Login_cshtml : RazorPage<LoginViewModel>
```

<span data-ttu-id="d808f-224">Razor expõe um `Model` propriedade para acessar o modelo passado para o modo de exibição:</span><span class="sxs-lookup"><span data-stu-id="d808f-224">Razor exposes a `Model` property for accessing the model passed to the view:</span></span>

```cshtml
<div>The Login Email: @Model.Email</div>
```

<span data-ttu-id="d808f-225">O `@model` diretiva especifica o tipo desta propriedade.</span><span class="sxs-lookup"><span data-stu-id="d808f-225">The `@model` directive specifies the type of this property.</span></span> <span data-ttu-id="d808f-226">Especifica a diretiva de `T` em `RazorPage<T>` o modo de exibição que gerado classe que deriva.</span><span class="sxs-lookup"><span data-stu-id="d808f-226">The directive specifies the `T` in `RazorPage<T>` that the generated class that the view derives from.</span></span> <span data-ttu-id="d808f-227">Se o `@model` diretiva especificado, o `Model` é de propriedade do tipo `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="d808f-227">If  the `@model` directive iisn't specified, the `Model` property is of type `dynamic`.</span></span> <span data-ttu-id="d808f-228">O valor do modelo é passado do controlador para o modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="d808f-228">The value of the model is passed from the controller to the view.</span></span> <span data-ttu-id="d808f-229">Para obter mais informações, consulte [fortemente tipado modelos e o @model palavra-chave.</span><span class="sxs-lookup"><span data-stu-id="d808f-229">For more information, see [Strongly typed models and the @model keyword.</span></span>

### <a name="inherits"></a>@inherits

<span data-ttu-id="d808f-230">O `@inherits` diretiva fornece controle total da classe herda o modo de exibição:</span><span class="sxs-lookup"><span data-stu-id="d808f-230">The `@inherits` directive provides  full control of the class the view inherits:</span></span>

```cshtml
@inherits TypeNameOfClassToInheritFrom
```

<span data-ttu-id="d808f-231">O código a seguir é um tipo de página Razor personalizado:</span><span class="sxs-lookup"><span data-stu-id="d808f-231">The following code is a custom Razor page type:</span></span>

[!code-csharp[Main](razor/sample/Classes/CustomRazorPage.cs)]

<span data-ttu-id="d808f-232">O `CustomText` é exibido em um modo de exibição:</span><span class="sxs-lookup"><span data-stu-id="d808f-232">The `CustomText` is displayed in a view:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact10.cshtml)]

<span data-ttu-id="d808f-233">O código processa o HTML a seguir:</span><span class="sxs-lookup"><span data-stu-id="d808f-233">The code renders the following HTML:</span></span>

```html
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

 <span data-ttu-id="d808f-234">`@model`e `@inherits` podem ser usados na mesma exibição.</span><span class="sxs-lookup"><span data-stu-id="d808f-234">`@model` and `@inherits` can be used in the same view.</span></span>  <span data-ttu-id="d808f-235">`@inherits`pode estar em um *viewimports. cshtml* arquivo que importa o modo de exibição:</span><span class="sxs-lookup"><span data-stu-id="d808f-235">`@inherits` can be in a *_ViewImports.cshtml* file that the view imports:</span></span>

[!code-cshtml[Main](razor/sample/Views/_ViewImportsModel.cshtml)]

<span data-ttu-id="d808f-236">O código a seguir é um exemplo de uma exibição fortemente tipado:</span><span class="sxs-lookup"><span data-stu-id="d808f-236">The following code is an example of a strongly-typed view:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Login1.cshtml)]

<span data-ttu-id="d808f-237">Se "rick@contoso.com" é passado no modelo, o modo de exibição gera uma marcação HTML a seguir:</span><span class="sxs-lookup"><span data-stu-id="d808f-237">If "rick@contoso.com" is passed in the model, the view generates the following HTML markup:</span></span>

```html
<div>The Login Email: rick@contoso.com</div>
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

### <a name="inject"></a>@inject


<span data-ttu-id="d808f-238">O `@inject` diretiva permite que a página do Razor para injetar um serviço a partir de [contêiner de serviço](xref:fundamentals/dependency-injection) em uma exibição.</span><span class="sxs-lookup"><span data-stu-id="d808f-238">The `@inject` directive enables the Razor Page to inject a service from the [service container](xref:fundamentals/dependency-injection) into a view.</span></span> <span data-ttu-id="d808f-239">Para obter mais informações, consulte [injeção de dependência para modos de exibição](xref:mvc/views/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="d808f-239">For more information, see [Dependency injection into views](xref:mvc/views/dependency-injection).</span></span>

### <a name="functions"></a>@functions

<span data-ttu-id="d808f-240">O `@functions` diretiva permite que uma página do Razor para adicionar conteúdo no nível de função para um modo de exibição:</span><span class="sxs-lookup"><span data-stu-id="d808f-240">The `@functions` directive enables a Razor Page to add function-level content to a view:</span></span>

```cshtml
@functions { // C# Code }
```

<span data-ttu-id="d808f-241">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="d808f-241">For example:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact6.cshtml)]

<span data-ttu-id="d808f-242">O código gera uma marcação HTML a seguir:</span><span class="sxs-lookup"><span data-stu-id="d808f-242">The code generates the following HTML markup:</span></span>

```html
<div>From method: Hello</div>
```

<span data-ttu-id="d808f-243">O código a seguir é a classe gerada Razor c#:</span><span class="sxs-lookup"><span data-stu-id="d808f-243">The following code is the generated Razor C# class:</span></span>

[!code-csharp[Main](razor/sample/Classes/Views_Home_Test_cshtml.cs?range=1-19)]

### <a name="section"></a>@section

<span data-ttu-id="d808f-244">O `@section` diretiva é usada em conjunto com o [layout](xref:mvc/views/layout) para habilitar os modos de exibição renderizar o conteúdo em diferentes partes da página HTML.</span><span class="sxs-lookup"><span data-stu-id="d808f-244">The `@section` directive is used in conjunction with the [layout](xref:mvc/views/layout) to enable views to render content in different parts of the HTML page.</span></span> <span data-ttu-id="d808f-245">Para obter mais informações, consulte [seções](xref:mvc/views/layout#layout-sections-label).</span><span class="sxs-lookup"><span data-stu-id="d808f-245">For more information, see [Sections](xref:mvc/views/layout#layout-sections-label).</span></span>

## <a name="tag-helpers"></a><span data-ttu-id="d808f-246">Auxiliares de marcação</span><span class="sxs-lookup"><span data-stu-id="d808f-246">Tag Helpers</span></span>

<span data-ttu-id="d808f-247">Há três diretivas que pertencem ao [auxiliares de marcação](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="d808f-247">There are three directives that pertain to [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

| <span data-ttu-id="d808f-248">Diretiva</span><span class="sxs-lookup"><span data-stu-id="d808f-248">Directive</span></span> | <span data-ttu-id="d808f-249">Função</span><span class="sxs-lookup"><span data-stu-id="d808f-249">Function</span></span> |
| --------- | -------- |
| [@addTagHelper](xref:mvc/views/tag-helpers/intro#add-helper-label) | <span data-ttu-id="d808f-250">Disponibiliza auxiliares de marcação para um modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="d808f-250">Makes Tag Helpers available to a view.</span></span> |
| [@removeTagHelper](xref:mvc/views/tag-helpers/intro#remove-razor-directives-label) | <span data-ttu-id="d808f-251">Remove os auxiliares de marcação adicionado anteriormente de um modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="d808f-251">Removes Tag Helpers previously added from a view.</span></span> |
| [@tagHelperPrefix](xref:mvc/views/tag-helpers/intro#prefix-razor-directives-label) | <span data-ttu-id="d808f-252">Especifica um prefixo de marca para habilitar o suporte auxiliar de marca e fazer uso do auxiliar de marca explícita.</span><span class="sxs-lookup"><span data-stu-id="d808f-252">Specifies a tag prefix to enable Tag Helper support and to make Tag Helper usage explicit.</span></span> |

## <a name="razor-reserved-keywords"></a><span data-ttu-id="d808f-253">Palavras-chave do Razor reservado</span><span class="sxs-lookup"><span data-stu-id="d808f-253">Razor reserved keywords</span></span>

### <a name="razor-keywords"></a><span data-ttu-id="d808f-254">Palavras-chave do Razor</span><span class="sxs-lookup"><span data-stu-id="d808f-254">Razor keywords</span></span>

* <span data-ttu-id="d808f-255">página (requer o ASP.NET Core 2.0 e posterior)</span><span class="sxs-lookup"><span data-stu-id="d808f-255">page (Requires ASP.NET Core 2.0 and later)</span></span>
* <span data-ttu-id="d808f-256">funções</span><span class="sxs-lookup"><span data-stu-id="d808f-256">functions</span></span>
* <span data-ttu-id="d808f-257">herda</span><span class="sxs-lookup"><span data-stu-id="d808f-257">inherits</span></span>
* <span data-ttu-id="d808f-258">modelo</span><span class="sxs-lookup"><span data-stu-id="d808f-258">model</span></span>
* <span data-ttu-id="d808f-259">section</span><span class="sxs-lookup"><span data-stu-id="d808f-259">section</span></span>
* <span data-ttu-id="d808f-260">auxiliar (atualmente não há suportada pelo ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="d808f-260">helper (Not currently supported by ASP.NET Core)</span></span>

<span data-ttu-id="d808f-261">Palavras-chave do Razor são ignoradas com `@(Razor Keyword)` (por exemplo, `@(functions)`).</span><span class="sxs-lookup"><span data-stu-id="d808f-261">Razor keywords are escaped with `@(Razor Keyword)` (for example, `@(functions)`).</span></span>

### <a name="c-razor-keywords"></a><span data-ttu-id="d808f-262">Palavras-chave do c# Razor</span><span class="sxs-lookup"><span data-stu-id="d808f-262">C# Razor keywords</span></span>

* <span data-ttu-id="d808f-263">case</span><span class="sxs-lookup"><span data-stu-id="d808f-263">case</span></span>
* <span data-ttu-id="d808f-264">do</span><span class="sxs-lookup"><span data-stu-id="d808f-264">do</span></span>
* <span data-ttu-id="d808f-265">default</span><span class="sxs-lookup"><span data-stu-id="d808f-265">default</span></span>
* <span data-ttu-id="d808f-266">for</span><span class="sxs-lookup"><span data-stu-id="d808f-266">for</span></span>
* <span data-ttu-id="d808f-267">foreach</span><span class="sxs-lookup"><span data-stu-id="d808f-267">foreach</span></span>
* <span data-ttu-id="d808f-268">if</span><span class="sxs-lookup"><span data-stu-id="d808f-268">if</span></span>
* <span data-ttu-id="d808f-269">else</span><span class="sxs-lookup"><span data-stu-id="d808f-269">else</span></span>
* <span data-ttu-id="d808f-270">bloqueio</span><span class="sxs-lookup"><span data-stu-id="d808f-270">lock</span></span>
* <span data-ttu-id="d808f-271">switch</span><span class="sxs-lookup"><span data-stu-id="d808f-271">switch</span></span>
* <span data-ttu-id="d808f-272">try</span><span class="sxs-lookup"><span data-stu-id="d808f-272">try</span></span>
* <span data-ttu-id="d808f-273">catch</span><span class="sxs-lookup"><span data-stu-id="d808f-273">catch</span></span>
* <span data-ttu-id="d808f-274">finally</span><span class="sxs-lookup"><span data-stu-id="d808f-274">finally</span></span>
* <span data-ttu-id="d808f-275">using</span><span class="sxs-lookup"><span data-stu-id="d808f-275">using</span></span>
* <span data-ttu-id="d808f-276">while</span><span class="sxs-lookup"><span data-stu-id="d808f-276">while</span></span>

<span data-ttu-id="d808f-277">Palavras-chave do c# Razor devem ser de escape duplo com `@(@C# Razor Keyword)` (por exemplo, `@(@case)`).</span><span class="sxs-lookup"><span data-stu-id="d808f-277">C# Razor keywords must be double-escaped with `@(@C# Razor Keyword)` (for example, `@(@case)`).</span></span> <span data-ttu-id="d808f-278">A primeira `@` ignora o analisador do Razor.</span><span class="sxs-lookup"><span data-stu-id="d808f-278">The first `@` escapes the Razor parser.</span></span> <span data-ttu-id="d808f-279">O segundo `@` ignora o analisador c#.</span><span class="sxs-lookup"><span data-stu-id="d808f-279">The second `@` escapes the C# parser.</span></span>

### <a name="reserved-keywords-not-used-by-razor"></a><span data-ttu-id="d808f-280">Palavras-chave reservadas não usadas pelo Razor</span><span class="sxs-lookup"><span data-stu-id="d808f-280">Reserved keywords not used by Razor</span></span>

* <span data-ttu-id="d808f-281">namespace</span><span class="sxs-lookup"><span data-stu-id="d808f-281">namespace</span></span>
* <span data-ttu-id="d808f-282">classe</span><span class="sxs-lookup"><span data-stu-id="d808f-282">class</span></span>

## <a name="viewing-the-razor-c-class-generated-for-a-view"></a><span data-ttu-id="d808f-283">Exibindo a classe Razor c# gerada para um modo de exibição</span><span class="sxs-lookup"><span data-stu-id="d808f-283">Viewing the Razor C# class generated for a view</span></span>

<span data-ttu-id="d808f-284">Adicione a seguinte classe ao projeto MVC do ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="d808f-284">Add the following class to the ASP.NET Core MVC project:</span></span>

[!code-csharp[Main](razor/sample/Utilities/CustomTemplateEngine.cs)]

<span data-ttu-id="d808f-285">Substituir o `RazorTemplateEngine` adicionado pelo MVC com o `CustomTemplateEngine` classe:</span><span class="sxs-lookup"><span data-stu-id="d808f-285">Override the `RazorTemplateEngine` added by MVC with the `CustomTemplateEngine` class:</span></span>

[!code-csharp[Main](razor/sample/Startup.cs?highlight=4&range=10-14)]

<span data-ttu-id="d808f-286">Definir um ponto de interrupção no `return csharpDocument` instrução `CustomTemplateEngine`.</span><span class="sxs-lookup"><span data-stu-id="d808f-286">Set a break point on the `return csharpDocument` statement of `CustomTemplateEngine`.</span></span> <span data-ttu-id="d808f-287">Quando a execução do programa é interrompida no ponto de interrupção, exibir o valor de `generatedCode`.</span><span class="sxs-lookup"><span data-stu-id="d808f-287">When program execution stops at the break point, view the value of `generatedCode`.</span></span>

![Exibição do Visualizador de texto de generatedCode](razor/_static/tvr.png)

## <a name="view-lookups-and-case-sensitivity"></a><span data-ttu-id="d808f-289">Pesquisas de exibição e diferenciação de maiusculas e minúsculas</span><span class="sxs-lookup"><span data-stu-id="d808f-289">View lookups and case sensitivity</span></span>

<span data-ttu-id="d808f-290">O mecanismo de exibição Razor executa pesquisas diferencia maiusculas de minúsculas para modos de exibição.</span><span class="sxs-lookup"><span data-stu-id="d808f-290">The Razor view engine performs case-sensitive lookups for views.</span></span> <span data-ttu-id="d808f-291">No entanto, a pesquisa real é determinada pelo sistema de arquivos subjacente:</span><span class="sxs-lookup"><span data-stu-id="d808f-291">However, the actual lookup is determined by the underlying file system:</span></span>

* <span data-ttu-id="d808f-292">Arquivo de origem com base:</span><span class="sxs-lookup"><span data-stu-id="d808f-292">File based source:</span></span> 
  * <span data-ttu-id="d808f-293">Em sistemas operacionais com sistemas de arquivos de maiusculas e minúsculas (por exemplo, Windows), pesquisas de provedor de arquivo físico diferenciam maiusculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="d808f-293">On operating systems with case insensitive file systems (for example, Windows), physical file provider lookups are case insensitive.</span></span> <span data-ttu-id="d808f-294">Por exemplo, `return View("Test")` resulta em correspondências para */Views/Home/Test.cshtml*, */Views/home/test.cshtml*e qualquer outra variante de maiusculas e minúsculas.</span><span class="sxs-lookup"><span data-stu-id="d808f-294">For example, `return View("Test")` results in matches for */Views/Home/Test.cshtml*, */Views/home/test.cshtml*, and any other casing variant.</span></span>
  * <span data-ttu-id="d808f-295">Em sistemas de arquivos diferencia maiusculas de minúsculas (por exemplo, Linux, OSX e com `EmbeddedFileProvider`), pesquisas diferenciam maiusculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="d808f-295">On case-sensitive file systems (for example, Linux, OSX, and with `EmbeddedFileProvider`), lookups are case-sensitive.</span></span> <span data-ttu-id="d808f-296">Por exemplo, `return View("Test")` especificamente correspondências */Views/Home/Test.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="d808f-296">For example, `return View("Test")` specifically matches */Views/Home/Test.cshtml*.</span></span>
* <span data-ttu-id="d808f-297">Pré-compilados exibições: com o núcleo do ASP.Net 2.0 e posterior, procurando os modos de exibição pré-compilado diferencia maiusculas de minúsculas em todos os sistemas operacionais.</span><span class="sxs-lookup"><span data-stu-id="d808f-297">Precompiled views: With ASP.Net Core 2.0 and later, looking up precompiled views is case insensitive on all operating systems.</span></span> <span data-ttu-id="d808f-298">O comportamento é idêntico ao comportamento do provedor de arquivo físico no Windows.</span><span class="sxs-lookup"><span data-stu-id="d808f-298">The behavior is identical to physical file provider's behavior on Windows.</span></span> <span data-ttu-id="d808f-299">Se os dois modos de exibição pré-compilado diferem apenas em maiusculas, o resultado da pesquisa é não determinística.</span><span class="sxs-lookup"><span data-stu-id="d808f-299">If two precompiled views differ only in case, the result of lookup is non-deterministic.</span></span>

<span data-ttu-id="d808f-300">Os desenvolvedores são encorajados para coincidir com o uso de maiusculas e minúsculas dos nomes de arquivo e diretório para o uso de maiusculas e minúsculas de:</span><span class="sxs-lookup"><span data-stu-id="d808f-300">Developers are encouraged to match the casing of file and directory names to the casing of:</span></span>

    * <span data-ttu-id="d808f-301">Nomes de área, controlador e ação.</span><span class="sxs-lookup"><span data-stu-id="d808f-301">Area, controller, and action names.</span></span> 
    * <span data-ttu-id="d808f-302">Páginas do Razor.</span><span class="sxs-lookup"><span data-stu-id="d808f-302">Razor Pages.</span></span>
    
<span data-ttu-id="d808f-303">Correspondência caso garante que as implantações de localizar as exibições, independentemente do sistema de arquivos subjacente.</span><span class="sxs-lookup"><span data-stu-id="d808f-303">Matching case ensures the deployments find their views regardless of the underlying file system.</span></span>

---
title: "Referência da sintaxe Razor para ASP.NET Core"
author: rick-anderson
description: "Saiba mais sobre a sintaxe de marcação Razor para inserir código baseado em servidor em páginas da Web."
manager: wpickett
ms.author: riande
ms.date: 10/18/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/razor
ms.openlocfilehash: 98021cc76555f0c1402764c845471a4730b01b20
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/30/2018
---
# <a name="razor-syntax-for-aspnet-core"></a><span data-ttu-id="94c0f-103">Sintaxe Razor para ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="94c0f-103">Razor syntax for ASP.NET Core</span></span>

<span data-ttu-id="94c0f-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT), [Luke Latham](https://github.com/guardrex), [Taylor Mullen](https://twitter.com/ntaylormullen) e [Dan Vicarel](https://github.com/Rabadash8820)</span><span class="sxs-lookup"><span data-stu-id="94c0f-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Luke Latham](https://github.com/guardrex), [Taylor Mullen](https://twitter.com/ntaylormullen), and [Dan Vicarel](https://github.com/Rabadash8820)</span></span>

<span data-ttu-id="94c0f-105">Razor é uma sintaxe de marcação para inserir código baseado em servidor em páginas da Web.</span><span class="sxs-lookup"><span data-stu-id="94c0f-105">Razor is a markup syntax for embedding server-based code into webpages.</span></span> <span data-ttu-id="94c0f-106">A sintaxe Razor é composta pela marcação Razor, por C# e por HTML.</span><span class="sxs-lookup"><span data-stu-id="94c0f-106">The Razor syntax consists of Razor markup, C#, and HTML.</span></span> <span data-ttu-id="94c0f-107">Arquivos que contêm Razor geralmente têm a extensão de arquivo *.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="94c0f-107">Files containing Razor generally have a *.cshtml* file extension.</span></span>

## <a name="rendering-html"></a><span data-ttu-id="94c0f-108">Renderizando HTML</span><span class="sxs-lookup"><span data-stu-id="94c0f-108">Rendering HTML</span></span>

<span data-ttu-id="94c0f-109">A linguagem padrão do Razor padrão é o HTML.</span><span class="sxs-lookup"><span data-stu-id="94c0f-109">The default Razor language is HTML.</span></span> <span data-ttu-id="94c0f-110">Renderizar HTML da marcação Razor não é diferente de renderizar HTML de um arquivo HTML.</span><span class="sxs-lookup"><span data-stu-id="94c0f-110">Rendering HTML from Razor markup is no different than rendering HTML from an HTML file.</span></span> <span data-ttu-id="94c0f-111">A marcação HTML em arquivos Razor *.cshtml* é renderizada pelo servidor inalterado.</span><span class="sxs-lookup"><span data-stu-id="94c0f-111">HTML markup in *.cshtml* Razor files is rendered by the server unchanged.</span></span>

## <a name="razor-syntax"></a><span data-ttu-id="94c0f-112">Sintaxe Razor</span><span class="sxs-lookup"><span data-stu-id="94c0f-112">Razor syntax</span></span>

<span data-ttu-id="94c0f-113">O Razor dá suporte a C# e usa o símbolo `@` para fazer a transição de HTML para C#.</span><span class="sxs-lookup"><span data-stu-id="94c0f-113">Razor supports C# and uses the `@` symbol to transition from HTML to C#.</span></span> <span data-ttu-id="94c0f-114">O Razor avalia expressões em C# e as renderiza na saída HTML.</span><span class="sxs-lookup"><span data-stu-id="94c0f-114">Razor evaluates C# expressions and renders them in the HTML output.</span></span>

<span data-ttu-id="94c0f-115">Quando um símbolo `@` é seguido por uma [palavra-chave reservada do Razor](#razor-reserved-keywords), ele faz a transição para marcação específica do Razor.</span><span class="sxs-lookup"><span data-stu-id="94c0f-115">When an `@` symbol is followed by a [Razor reserved keyword](#razor-reserved-keywords), it transitions into Razor-specific markup.</span></span> <span data-ttu-id="94c0f-116">Caso contrário, ele faz a transição para C# simples.</span><span class="sxs-lookup"><span data-stu-id="94c0f-116">Otherwise, it transitions into plain C#.</span></span>

<span data-ttu-id="94c0f-117">Para fazer o escape de um símbolo `@` na marcação Razor, use um segundo símbolo `@`:</span><span class="sxs-lookup"><span data-stu-id="94c0f-117">To escape an `@` symbol in Razor markup, use a second `@` symbol:</span></span>

```cshtml
<p>@@Username</p>
```

<span data-ttu-id="94c0f-118">O código é renderizado em HTML com um único símbolo `@`:</span><span class="sxs-lookup"><span data-stu-id="94c0f-118">The code is rendered in HTML with a single `@` symbol:</span></span>

```html
<p>@Username</p>
```

<span data-ttu-id="94c0f-119">Conteúdo e atributos HTML que contêm endereços de email não tratam o símbolo `@` como um caractere de transição.</span><span class="sxs-lookup"><span data-stu-id="94c0f-119">HTML attributes and content containing email addresses don't treat the `@` symbol as a transition character.</span></span> <span data-ttu-id="94c0f-120">Os endereços de email no exemplo a seguir não são alterados pela análise do Razor:</span><span class="sxs-lookup"><span data-stu-id="94c0f-120">The email addresses in the following example are untouched by Razor parsing:</span></span>

```cshtml
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
```

## <a name="implicit-razor-expressions"></a><span data-ttu-id="94c0f-121">Expressões Razor implícitas</span><span class="sxs-lookup"><span data-stu-id="94c0f-121">Implicit Razor expressions</span></span>

<span data-ttu-id="94c0f-122">Expressões Razor implícitas são iniciadas por `@` seguido do código C#:</span><span class="sxs-lookup"><span data-stu-id="94c0f-122">Implicit Razor expressions start with `@` followed by C# code:</span></span>

```cshtml
<p>@DateTime.Now</p>
<p>@DateTime.IsLeapYear(2016)</p>
```

<span data-ttu-id="94c0f-123">Com exceção da palavra-chave C# `await`, expressões implícitas não devem conter espaços.</span><span class="sxs-lookup"><span data-stu-id="94c0f-123">With the exception of the C# `await` keyword, implicit expressions must not contain spaces.</span></span> <span data-ttu-id="94c0f-124">Se a instrução C# tiver uma terminação clara, espaços podem ser misturados:</span><span class="sxs-lookup"><span data-stu-id="94c0f-124">If the C# statement has a clear ending, spaces can be intermingled:</span></span>

```cshtml
<p>@await DoSomething("hello", "world")</p>
```

<span data-ttu-id="94c0f-125">Expressões implícitas **não podem** conter elementos genéricos de C#, pois caracteres dentro de colchetes (`<>`) são interpretados como uma marca HTML.</span><span class="sxs-lookup"><span data-stu-id="94c0f-125">Implicit expressions **cannot** contain C# generics, as the characters inside the brackets (`<>`) are interpreted as an HTML tag.</span></span> <span data-ttu-id="94c0f-126">O código a seguir é **inválido**:</span><span class="sxs-lookup"><span data-stu-id="94c0f-126">The following code is **not** valid:</span></span>

```cshtml
<p>@GenericMethod<int>()</p>
```

<span data-ttu-id="94c0f-127">O código anterior gera um erro de compilador semelhante a um dos seguintes:</span><span class="sxs-lookup"><span data-stu-id="94c0f-127">The preceding code generates a compiler error similar to one of the following:</span></span>

 * <span data-ttu-id="94c0f-128">O elemento "int" não foi fechado.</span><span class="sxs-lookup"><span data-stu-id="94c0f-128">The "int" element wasn't closed.</span></span> <span data-ttu-id="94c0f-129">Todos os elementos devem ter fechamento automático ou ter uma marca de fim correspondente.</span><span class="sxs-lookup"><span data-stu-id="94c0f-129">All elements must be either self-closing or have a matching end tag.</span></span>
 *  <span data-ttu-id="94c0f-130">Não é possível converter o grupo de métodos "GenericMethod" em um "object" de tipo não delegado.</span><span class="sxs-lookup"><span data-stu-id="94c0f-130">Cannot convert method group 'GenericMethod' to non-delegate type 'object'.</span></span> <span data-ttu-id="94c0f-131">Você pretendia invocar o método?</span><span class="sxs-lookup"><span data-stu-id="94c0f-131">Did you intend to invoke the method?\`</span></span> 
 
<span data-ttu-id="94c0f-132">Chamadas de método genérico devem ser encapsuladas em uma [expressão Razor explícita](#explicit-razor-expressions) ou em um [bloco de código Razor](#razor-code-blocks).</span><span class="sxs-lookup"><span data-stu-id="94c0f-132">Generic method calls must be wrapped in an [explicit Razor expression](#explicit-razor-expressions) or a [Razor code block](#razor-code-blocks).</span></span>

## <a name="explicit-razor-expressions"></a><span data-ttu-id="94c0f-133">Expressões Razor explícitas</span><span class="sxs-lookup"><span data-stu-id="94c0f-133">Explicit Razor expressions</span></span>

<span data-ttu-id="94c0f-134">Expressões Razor explícitas consistem de um símbolo `@` com parênteses equilibradas.</span><span class="sxs-lookup"><span data-stu-id="94c0f-134">Explicit Razor expressions consist of an `@` symbol with balanced parenthesis.</span></span> <span data-ttu-id="94c0f-135">Para renderizar a hora da última semana, a seguinte marcação Razor é usada:</span><span class="sxs-lookup"><span data-stu-id="94c0f-135">To render last week's time, the following Razor markup is used:</span></span>

```cshtml
<p>Last week this time: @(DateTime.Now - TimeSpan.FromDays(7))</p>
```

<span data-ttu-id="94c0f-136">Qualquer conteúdo dentro dos parênteses `@()` é avaliado e renderizado para a saída.</span><span class="sxs-lookup"><span data-stu-id="94c0f-136">Any content within the `@()` parenthesis is evaluated and rendered to the output.</span></span>

<span data-ttu-id="94c0f-137">Expressões implícitas, descritas na seção anterior, geralmente não podem conter espaços.</span><span class="sxs-lookup"><span data-stu-id="94c0f-137">Implicit expressions, described in the previous section, generally can't contain spaces.</span></span> <span data-ttu-id="94c0f-138">No código a seguir, uma semana não é subtraída da hora atual:</span><span class="sxs-lookup"><span data-stu-id="94c0f-138">In the following code, one week isn't subtracted from the current time:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact.cshtml?range=17)]

<span data-ttu-id="94c0f-139">O código renderiza o HTML a seguir:</span><span class="sxs-lookup"><span data-stu-id="94c0f-139">The code renders the following HTML:</span></span>

```html
<p>Last week: 7/7/2016 4:39:52 PM - TimeSpan.FromDays(7)</p>
```

<span data-ttu-id="94c0f-140">Expressões explícitas podem ser usadas para concatenar texto com um resultado de expressão:</span><span class="sxs-lookup"><span data-stu-id="94c0f-140">Explicit expressions can be used to concatenate text with an expression result:</span></span>

```cshtml
@{
    var joe = new Person("Joe", 33);
}

<p>Age@(joe.Age)</p>
```

<span data-ttu-id="94c0f-141">Sem a expressão explícita, `<p>Age@joe.Age</p>` é tratado como um endereço de email e `<p>Age@joe.Age</p>` é renderizado.</span><span class="sxs-lookup"><span data-stu-id="94c0f-141">Without the explicit expression, `<p>Age@joe.Age</p>` is treated as an email address, and `<p>Age@joe.Age</p>` is rendered.</span></span> <span data-ttu-id="94c0f-142">Quando escrito como uma expressão explícita, `<p>Age33</p>` é renderizado.</span><span class="sxs-lookup"><span data-stu-id="94c0f-142">When written as an explicit expression, `<p>Age33</p>` is rendered.</span></span>


<span data-ttu-id="94c0f-143">Expressões explícitas podem ser usadas para renderizar a saída de métodos genéricos em arquivos *.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="94c0f-143">Explicit expressions can be used to render output from generic methods in *.cshtml* files.</span></span> <span data-ttu-id="94c0f-144">Em uma expressão implícita, os caracteres dentro de colchetes (`<>`) são interpretados como uma marca HTML.</span><span class="sxs-lookup"><span data-stu-id="94c0f-144">In an implicit expression, the characters inside the brackets (`<>`) are interpreted as an HTML tag.</span></span> <span data-ttu-id="94c0f-145">A seguinte marcação Razor é **inválida**:</span><span class="sxs-lookup"><span data-stu-id="94c0f-145">The following markup is **not** valid Razor:</span></span>

```cshtml
<p>@GenericMethod<int>()</p>
```

<span data-ttu-id="94c0f-146">O código anterior gera um erro de compilador semelhante a um dos seguintes:</span><span class="sxs-lookup"><span data-stu-id="94c0f-146">The preceding code generates a compiler error similar to one of the following:</span></span>

 * <span data-ttu-id="94c0f-147">O elemento "int" não foi fechado.</span><span class="sxs-lookup"><span data-stu-id="94c0f-147">The "int" element wasn't closed.</span></span> <span data-ttu-id="94c0f-148">Todos os elementos devem ter fechamento automático ou ter uma marca de fim correspondente.</span><span class="sxs-lookup"><span data-stu-id="94c0f-148">All elements must be either self-closing or have a matching end tag.</span></span>
 *  <span data-ttu-id="94c0f-149">Não é possível converter o grupo de métodos "GenericMethod" em um "object" de tipo não delegado.</span><span class="sxs-lookup"><span data-stu-id="94c0f-149">Cannot convert method group 'GenericMethod' to non-delegate type 'object'.</span></span> <span data-ttu-id="94c0f-150">Você pretendia invocar o método?</span><span class="sxs-lookup"><span data-stu-id="94c0f-150">Did you intend to invoke the method?\`</span></span> 
 
 <span data-ttu-id="94c0f-151">A marcação a seguir mostra a maneira correta de escrever esse código.</span><span class="sxs-lookup"><span data-stu-id="94c0f-151">The following markup shows the correct way write this code.</span></span> <span data-ttu-id="94c0f-152">O código é escrito como uma expressão explícita:</span><span class="sxs-lookup"><span data-stu-id="94c0f-152">The code is written as an explicit expression:</span></span>

```cshtml
<p>@(GenericMethod<int>())</p>
```

## <a name="expression-encoding"></a><span data-ttu-id="94c0f-153">Codificação de expressão</span><span class="sxs-lookup"><span data-stu-id="94c0f-153">Expression encoding</span></span>

<span data-ttu-id="94c0f-154">Expressões em C# que são avaliadas como uma cadeia de caracteres estão codificadas em HTML.</span><span class="sxs-lookup"><span data-stu-id="94c0f-154">C# expressions that evaluate to a string are HTML encoded.</span></span> <span data-ttu-id="94c0f-155">Expressões em C# que são avaliadas como `IHtmlContent` são renderizadas diretamente por meio `IHtmlContent.WriteTo`.</span><span class="sxs-lookup"><span data-stu-id="94c0f-155">C# expressions that evaluate to `IHtmlContent` are rendered directly through `IHtmlContent.WriteTo`.</span></span> <span data-ttu-id="94c0f-156">Expressões em C# que não são avaliadas como `IHtmlContent` são convertidas em uma cadeia de caracteres por `ToString` e codificadas antes que sejam renderizadas.</span><span class="sxs-lookup"><span data-stu-id="94c0f-156">C# expressions that don't evaluate to `IHtmlContent` are converted to a string by `ToString` and encoded before they're rendered.</span></span>

```cshtml
@("<span>Hello World</span>")
```

<span data-ttu-id="94c0f-157">O código renderiza o HTML a seguir:</span><span class="sxs-lookup"><span data-stu-id="94c0f-157">The code renders the following HTML:</span></span>

```html
&lt;span&gt;Hello World&lt;/span&gt;
```

<span data-ttu-id="94c0f-158">O HTML é mostrado no navegador como:</span><span class="sxs-lookup"><span data-stu-id="94c0f-158">The HTML is shown in the browser as:</span></span>

```
<span>Hello World</span>
```

<span data-ttu-id="94c0f-159">A saída `HtmlHelper.Raw` não é codificada, mas renderizada como marcação HTML.</span><span class="sxs-lookup"><span data-stu-id="94c0f-159">`HtmlHelper.Raw` output isn't encoded but rendered as HTML markup.</span></span>

> [!WARNING]
> <span data-ttu-id="94c0f-160">Usar `HtmlHelper.Raw` em uma entrada do usuário que não está limpa é um risco de segurança.</span><span class="sxs-lookup"><span data-stu-id="94c0f-160">Using `HtmlHelper.Raw` on unsanitized user input is a security risk.</span></span> <span data-ttu-id="94c0f-161">A entrada do usuário pode conter JavaScript mal-intencionado ou outras formas de exploração.</span><span class="sxs-lookup"><span data-stu-id="94c0f-161">User input might contain malicious JavaScript or other exploits.</span></span> <span data-ttu-id="94c0f-162">Limpar a entrada do usuário é difícil.</span><span class="sxs-lookup"><span data-stu-id="94c0f-162">Sanitizing user input is difficult.</span></span> <span data-ttu-id="94c0f-163">Evite usar `HtmlHelper.Raw` com a entrada do usuário.</span><span class="sxs-lookup"><span data-stu-id="94c0f-163">Avoid using `HtmlHelper.Raw` with user input.</span></span>

```cshtml
@Html.Raw("<span>Hello World</span>")
```

<span data-ttu-id="94c0f-164">O código renderiza o HTML a seguir:</span><span class="sxs-lookup"><span data-stu-id="94c0f-164">The code renders the following HTML:</span></span>

```html
<span>Hello World</span>
```

## <a name="razor-code-blocks"></a><span data-ttu-id="94c0f-165">Blocos de código Razor</span><span class="sxs-lookup"><span data-stu-id="94c0f-165">Razor code blocks</span></span>

<span data-ttu-id="94c0f-166">Blocos de código Razor são iniciados por `@` e delimitados por `{}`.</span><span class="sxs-lookup"><span data-stu-id="94c0f-166">Razor code blocks start with `@` and are enclosed by `{}`.</span></span> <span data-ttu-id="94c0f-167">Diferente das expressões, o código C# dentro de blocos de código não é renderizado.</span><span class="sxs-lookup"><span data-stu-id="94c0f-167">Unlike expressions, C# code inside code blocks isn't rendered.</span></span> <span data-ttu-id="94c0f-168">Blocos de código e expressões em uma exibição compartilham o mesmo escopo e são definidos em ordem:</span><span class="sxs-lookup"><span data-stu-id="94c0f-168">Code blocks and expressions in a view share the same scope and are defined in order:</span></span>

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

<span data-ttu-id="94c0f-169">O código renderiza o HTML a seguir:</span><span class="sxs-lookup"><span data-stu-id="94c0f-169">The code renders the following HTML:</span></span>

```html
<p>The future depends on what you do today. - Mahatma Gandhi</p>
<p>Hate cannot drive out hate, only love can do that. - Martin Luther King, Jr.</p>
```

### <a name="implicit-transitions"></a><span data-ttu-id="94c0f-170">Transições implícitas</span><span class="sxs-lookup"><span data-stu-id="94c0f-170">Implicit transitions</span></span>

<span data-ttu-id="94c0f-171">A linguagem padrão em um bloco de código é C#, mas a página Razor pode fazer a transição de volta para HTML:</span><span class="sxs-lookup"><span data-stu-id="94c0f-171">The default language in a code block is C#, but the Razor Page can transition back to HTML:</span></span>

```cshtml
@{
    var inCSharp = true;
    <p>Now in HTML, was in C# @inCSharp</p>
}
```

### <a name="explicit-delimited-transition"></a><span data-ttu-id="94c0f-172">Transição delimitada explícita</span><span class="sxs-lookup"><span data-stu-id="94c0f-172">Explicit delimited transition</span></span>

<span data-ttu-id="94c0f-173">Para definir uma subseção de um bloco de código que deve renderizar HTML, circunde os caracteres para renderização com as marca Razor **\<text>**:</span><span class="sxs-lookup"><span data-stu-id="94c0f-173">To define a subsection of a code block that should render HTML, surround the characters for rendering with the Razor **\<text>** tag:</span></span>

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <text>Name: @person.Name</text>
}
```

<span data-ttu-id="94c0f-174">Use essa abordagem para renderizar HTML que não está circundado por uma marca HTML.</span><span class="sxs-lookup"><span data-stu-id="94c0f-174">Use this approach to render HTML that isn't surrounded by an HTML tag.</span></span> <span data-ttu-id="94c0f-175">Sem uma marca HTML ou Razor, ocorrerá um erro de tempo de execução do Razor.</span><span class="sxs-lookup"><span data-stu-id="94c0f-175">Without an HTML or Razor tag, a Razor runtime error occurs.</span></span>

<span data-ttu-id="94c0f-176">A marca **\<text>** é útil para controlar o espaço em branco ao renderizar conteúdo:</span><span class="sxs-lookup"><span data-stu-id="94c0f-176">The **\<text>** tag is useful to control whitespace when rendering content:</span></span>

* <span data-ttu-id="94c0f-177">Somente o conteúdo entre a marca **\<text>** é renderizado.</span><span class="sxs-lookup"><span data-stu-id="94c0f-177">Only the content between the **\<text>** tag is rendered.</span></span> 
* <span data-ttu-id="94c0f-178">Não aparece nenhum espaço em branco antes ou depois da marca **\<text>** na saída HTML.</span><span class="sxs-lookup"><span data-stu-id="94c0f-178">No whitespace before or after the **\<text>** tag appears in the HTML output.</span></span>

### <a name="explicit-line-transition-with-"></a><span data-ttu-id="94c0f-179">Transição de linha explícita com @:</span><span class="sxs-lookup"><span data-stu-id="94c0f-179">Explicit Line Transition with @:</span></span>

<span data-ttu-id="94c0f-180">Para renderizar o restante de uma linha inteira como HTML dentro de um bloco de código, use a sintaxe `@:`:</span><span class="sxs-lookup"><span data-stu-id="94c0f-180">To render the rest of an entire line as HTML inside a code block, use the `@:` syntax:</span></span>

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    @:Name: @person.Name
}
```

<span data-ttu-id="94c0f-181">Sem o `@:` no código, será gerado um erro de tempo de execução do Razor.</span><span class="sxs-lookup"><span data-stu-id="94c0f-181">Without the `@:` in the code, a Razor runtime error is generated.</span></span>

<span data-ttu-id="94c0f-182">Aviso: caracteres `@` extra em um arquivo Razor podem causar erros do compilador em instruções mais adiante no bloco.</span><span class="sxs-lookup"><span data-stu-id="94c0f-182">Warning: Extra `@` characters in a Razor file can cause cause compiler errors at statements later in the block.</span></span> <span data-ttu-id="94c0f-183">Esses erros do compilador podem ser difíceis de entender porque o erro real ocorre antes do erro relatado.</span><span class="sxs-lookup"><span data-stu-id="94c0f-183">These compiler errors can be difficult to understand because the actual error occurs before the reported error.</span></span> <span data-ttu-id="94c0f-184">Esse erro é comum após combinar várias expressões implícitas/explícitas em um bloco de código único.</span><span class="sxs-lookup"><span data-stu-id="94c0f-184">This error is common after combining multiple implicit/explicit expressions into a single code block.</span></span>

## <a name="control-structures"></a><span data-ttu-id="94c0f-185">Estruturas de controle</span><span class="sxs-lookup"><span data-stu-id="94c0f-185">Control Structures</span></span>

<span data-ttu-id="94c0f-186">Estruturas de controle são uma extensão dos blocos de código.</span><span class="sxs-lookup"><span data-stu-id="94c0f-186">Control structures are an extension of code blocks.</span></span> <span data-ttu-id="94c0f-187">Todos os aspectos dos blocos de código (transição para marcação, C# embutido) também se aplicam às seguintes estruturas:</span><span class="sxs-lookup"><span data-stu-id="94c0f-187">All aspects of code blocks (transitioning to markup, inline C#) also apply to the following structures:</span></span>

### <a name="conditionals-if-else-if-else-and-switch"></a><span data-ttu-id="94c0f-188">Condicionais @if, else if, else e @switch</span><span class="sxs-lookup"><span data-stu-id="94c0f-188">Conditionals @if, else if, else, and @switch</span></span>

<span data-ttu-id="94c0f-189">`@if` controla quando o código é executado:</span><span class="sxs-lookup"><span data-stu-id="94c0f-189">`@if` controls when code runs:</span></span>

```cshtml
@if (value % 2 == 0)
{
    <p>The value was even.</p>
}
```

<span data-ttu-id="94c0f-190">`else` e `else if` não exigem o símbolo `@`:</span><span class="sxs-lookup"><span data-stu-id="94c0f-190">`else` and `else if` don't require the `@` symbol:</span></span>

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

<span data-ttu-id="94c0f-191">A marcação a seguir mostra como usar uma instrução switch:</span><span class="sxs-lookup"><span data-stu-id="94c0f-191">The following markup shows how to use a switch statement:</span></span>

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

### <a name="looping-for-foreach-while-and-do-while"></a><span data-ttu-id="94c0f-192">Loop de @for, @foreach, @while e @do while</span><span class="sxs-lookup"><span data-stu-id="94c0f-192">Looping @for, @foreach, @while, and @do while</span></span>

<span data-ttu-id="94c0f-193">O HTML no modelo pode ser renderizado com instruções de controle em loop.</span><span class="sxs-lookup"><span data-stu-id="94c0f-193">Templated HTML can be rendered with looping control statements.</span></span> <span data-ttu-id="94c0f-194">Para renderizar uma lista de pessoas:</span><span class="sxs-lookup"><span data-stu-id="94c0f-194">To render a list of people:</span></span>

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

<span data-ttu-id="94c0f-195">Há suporte para as seguintes instruções em loop:</span><span class="sxs-lookup"><span data-stu-id="94c0f-195">The following looping statements are supported:</span></span>

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

### <a name="compound-using"></a><span data-ttu-id="94c0f-196">@using composto</span><span class="sxs-lookup"><span data-stu-id="94c0f-196">Compound @using</span></span>

<span data-ttu-id="94c0f-197">Em C#, uma instrução `using` é usada para garantir que um objeto seja descartado.</span><span class="sxs-lookup"><span data-stu-id="94c0f-197">In C#, a `using` statement is used to ensure an object is disposed.</span></span> <span data-ttu-id="94c0f-198">No Razor, o mesmo mecanismo é usado para criar Auxiliares HTML que têm conteúdo adicional.</span><span class="sxs-lookup"><span data-stu-id="94c0f-198">In Razor, the same mechanism is used to create HTML Helpers that contain additional content.</span></span> <span data-ttu-id="94c0f-199">No código a seguir, os Auxiliares HTML renderizam uma marca de formulário com a instrução `@using`:</span><span class="sxs-lookup"><span data-stu-id="94c0f-199">In the following code, HTML Helpers render a form tag with the `@using` statement:</span></span>


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

<span data-ttu-id="94c0f-200">Ações no nível de escopo podem ser executadas com [Auxiliares de marca](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="94c0f-200">Scope-level actions can be performed with [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

### <a name="try-catch-finally"></a><span data-ttu-id="94c0f-201">@try, catch, finally</span><span class="sxs-lookup"><span data-stu-id="94c0f-201">@try, catch, finally</span></span>

<span data-ttu-id="94c0f-202">O tratamento de exceções é semelhante ao de C#:</span><span class="sxs-lookup"><span data-stu-id="94c0f-202">Exception handling is similar to C#:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact7.cshtml)]

### <a name="lock"></a>@lock

<span data-ttu-id="94c0f-203">O Razor tem a capacidade de proteger seções críticas com instruções de bloqueio:</span><span class="sxs-lookup"><span data-stu-id="94c0f-203">Razor has the capability to protect critical sections with lock statements:</span></span>

```cshtml
@lock (SomeLock)
{
    // Do critical section work
}
```

### <a name="comments"></a><span data-ttu-id="94c0f-204">Comentários</span><span class="sxs-lookup"><span data-stu-id="94c0f-204">Comments</span></span>

<span data-ttu-id="94c0f-205">O Razor dá suporte a comentários em C# e HTML:</span><span class="sxs-lookup"><span data-stu-id="94c0f-205">Razor supports C# and HTML comments:</span></span>

```cshtml
@{
    /* C# comment */
    // Another C# comment
}
<!-- HTML comment -->
```

<span data-ttu-id="94c0f-206">O código renderiza o HTML a seguir:</span><span class="sxs-lookup"><span data-stu-id="94c0f-206">The code renders the following HTML:</span></span>

```html
<!-- HTML comment -->
```

<span data-ttu-id="94c0f-207">Comentários em Razor são removidos pelo servidor antes que a página da Web seja renderizada.</span><span class="sxs-lookup"><span data-stu-id="94c0f-207">Razor comments are removed by the server before the webpage is rendered.</span></span> <span data-ttu-id="94c0f-208">O Razor usa `@*  *@` para delimitar comentários.</span><span class="sxs-lookup"><span data-stu-id="94c0f-208">Razor uses `@*  *@` to delimit comments.</span></span> <span data-ttu-id="94c0f-209">O código a seguir é comentado, de modo que o servidor não renderiza nenhuma marcação:</span><span class="sxs-lookup"><span data-stu-id="94c0f-209">The following code is commented out, so the server doesn't render any markup:</span></span>

```cshtml
@*
    @{
        /* C# comment */
        // Another C# comment
    }
    <!-- HTML comment -->
*@
```

## <a name="directives"></a><span data-ttu-id="94c0f-210">Diretivas</span><span class="sxs-lookup"><span data-stu-id="94c0f-210">Directives</span></span>

<span data-ttu-id="94c0f-211">Diretivas de Razor são representadas por expressões implícitas com palavras-chave reservadas após o símbolo `@`.</span><span class="sxs-lookup"><span data-stu-id="94c0f-211">Razor directives are represented by implicit expressions with reserved keywords following the `@` symbol.</span></span> <span data-ttu-id="94c0f-212">Uma diretiva geralmente altera o modo como uma exibição é analisada ou habilita uma funcionalidade diferente.</span><span class="sxs-lookup"><span data-stu-id="94c0f-212">A directive typically changes the way a view is parsed or enables different functionality.</span></span>

<span data-ttu-id="94c0f-213">Compreender como o Razor gera código para uma exibição torna mais fácil entender como as diretivas funcionam.</span><span class="sxs-lookup"><span data-stu-id="94c0f-213">Understanding how Razor generates code for a view makes it easier to understand how directives work.</span></span>

[!code-html[Main](razor/sample/Views/Home/Contact8.cshtml)]

<span data-ttu-id="94c0f-214">O código gera uma classe semelhante à seguinte:</span><span class="sxs-lookup"><span data-stu-id="94c0f-214">The code generates a class similar to the following:</span></span>

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

<span data-ttu-id="94c0f-215">Posteriormente neste artigo, a seção [Exibindo a classe C# de Razor gerada para uma exibição](#viewing-the-razor-c-class-generated-for-a-view) explica como exibir essa classe gerada.</span><span class="sxs-lookup"><span data-stu-id="94c0f-215">Later in this article, the section [Viewing the Razor C# class generated for a view](#viewing-the-razor-c-class-generated-for-a-view) explains how to view this generated class.</span></span>

### <a name="using"></a>@using

<span data-ttu-id="94c0f-216">A diretiva `@using` adiciona a diretiva `using` de C# à exibição gerada:</span><span class="sxs-lookup"><span data-stu-id="94c0f-216">The `@using` directive adds the C# `using` directive to the generated view:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact9.cshtml)]

### <a name="model"></a>@model

<span data-ttu-id="94c0f-217">A diretiva `@model` especifica o tipo do modelo passado para uma exibição:</span><span class="sxs-lookup"><span data-stu-id="94c0f-217">The `@model` directive specifies the type of the model passed to a view:</span></span>

```cshtml
@model TypeNameOfModel
```

<span data-ttu-id="94c0f-218">Em um aplicativo do ASP.NET Core MCV criado com contas de usuário individuais, a exibição *Views/Account/Login.cshtml* contém a declaração de modelo a seguir:</span><span class="sxs-lookup"><span data-stu-id="94c0f-218">In an ASP.NET Core MVC app created with individual user accounts, the *Views/Account/Login.cshtml* view contains the following model declaration:</span></span>

```cshtml
@model LoginViewModel
```

<span data-ttu-id="94c0f-219">A classe gerada herda de `RazorPage<dynamic>`:</span><span class="sxs-lookup"><span data-stu-id="94c0f-219">The class generated inherits from `RazorPage<dynamic>`:</span></span>

```csharp
public class _Views_Account_Login_cshtml : RazorPage<LoginViewModel>
```

<span data-ttu-id="94c0f-220">O Razor expõe uma propriedade `Model` para acessar o modelo passado para a exibição:</span><span class="sxs-lookup"><span data-stu-id="94c0f-220">Razor exposes a `Model` property for accessing the model passed to the view:</span></span>

```cshtml
<div>The Login Email: @Model.Email</div>
```

<span data-ttu-id="94c0f-221">A diretiva `@model` especifica o tipo dessa propriedade.</span><span class="sxs-lookup"><span data-stu-id="94c0f-221">The `@model` directive specifies the type of this property.</span></span> <span data-ttu-id="94c0f-222">A diretiva especifica o `T` em `RazorPage<T>` da classe gerada da qual a exibição deriva.</span><span class="sxs-lookup"><span data-stu-id="94c0f-222">The directive specifies the `T` in `RazorPage<T>` that the generated class that the view derives from.</span></span> <span data-ttu-id="94c0f-223">Se a diretiva `@model` não for especificada, a propriedade `Model` será do tipo `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="94c0f-223">If the `@model` directive iisn't specified, the `Model` property is of type `dynamic`.</span></span> <span data-ttu-id="94c0f-224">O valor do modelo é passado do controlador para a exibição.</span><span class="sxs-lookup"><span data-stu-id="94c0f-224">The value of the model is passed from the controller to the view.</span></span> <span data-ttu-id="94c0f-225">Para obter mais informações, consulte [Modelos fortemente tipados e a palavra-chave @model.</span><span class="sxs-lookup"><span data-stu-id="94c0f-225">For more information, see [Strongly typed models and the @model keyword.</span></span>

### <a name="inherits"></a>@inherits

<span data-ttu-id="94c0f-226">A diretiva `@inherits` fornece controle total da classe que a exibição herda:</span><span class="sxs-lookup"><span data-stu-id="94c0f-226">The `@inherits` directive provides full control of the class the view inherits:</span></span>

```cshtml
@inherits TypeNameOfClassToInheritFrom
```

<span data-ttu-id="94c0f-227">O código a seguir é um tipo de página Razor personalizado:</span><span class="sxs-lookup"><span data-stu-id="94c0f-227">The following code is a custom Razor page type:</span></span>

[!code-csharp[Main](razor/sample/Classes/CustomRazorPage.cs)]

<span data-ttu-id="94c0f-228">O `CustomText` é exibido em uma exibição:</span><span class="sxs-lookup"><span data-stu-id="94c0f-228">The `CustomText` is displayed in a view:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact10.cshtml)]

<span data-ttu-id="94c0f-229">O código renderiza o HTML a seguir:</span><span class="sxs-lookup"><span data-stu-id="94c0f-229">The code renders the following HTML:</span></span>

```html
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

 <span data-ttu-id="94c0f-230">`@model` e `@inherits` podem ser usados na mesma exibição.</span><span class="sxs-lookup"><span data-stu-id="94c0f-230">`@model` and `@inherits` can be used in the same view.</span></span> <span data-ttu-id="94c0f-231">`@inherits` pode estar em um arquivo *_ViewImports.cshtml* que a exibição importa:</span><span class="sxs-lookup"><span data-stu-id="94c0f-231">`@inherits` can be in a *_ViewImports.cshtml* file that the view imports:</span></span>

[!code-cshtml[Main](razor/sample/Views/_ViewImportsModel.cshtml)]

<span data-ttu-id="94c0f-232">O código a seguir é um exemplo de exibição fortemente tipada:</span><span class="sxs-lookup"><span data-stu-id="94c0f-232">The following code is an example of a strongly-typed view:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Login1.cshtml)]

<span data-ttu-id="94c0f-233">Se "rick@contoso.com" for passado no modelo, a exibição gerará a seguinte marcação HTML:</span><span class="sxs-lookup"><span data-stu-id="94c0f-233">If "rick@contoso.com" is passed in the model, the view generates the following HTML markup:</span></span>

```html
<div>The Login Email: rick@contoso.com</div>
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

### <a name="inject"></a>@inject


<span data-ttu-id="94c0f-234">A diretiva `@inject` permite que a página do Razor injete um serviço do [contêiner de serviço](xref:fundamentals/dependency-injection) em uma exibição.</span><span class="sxs-lookup"><span data-stu-id="94c0f-234">The `@inject` directive enables the Razor Page to inject a service from the [service container](xref:fundamentals/dependency-injection) into a view.</span></span> <span data-ttu-id="94c0f-235">Para obter mais informações, consulte [Injeção de dependência em exibições](xref:mvc/views/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="94c0f-235">For more information, see [Dependency injection into views](xref:mvc/views/dependency-injection).</span></span>

### <a name="functions"></a>@functions

<span data-ttu-id="94c0f-236">A diretiva `@functions` permite que uma página do Razor adicione conteúdo no nível da função a uma exibição:</span><span class="sxs-lookup"><span data-stu-id="94c0f-236">The `@functions` directive enables a Razor Page to add function-level content to a view:</span></span>

```cshtml
@functions { // C# Code }
```

<span data-ttu-id="94c0f-237">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="94c0f-237">For example:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact6.cshtml)]

<span data-ttu-id="94c0f-238">O código gera a seguinte marcação HTML:</span><span class="sxs-lookup"><span data-stu-id="94c0f-238">The code generates the following HTML markup:</span></span>

```html
<div>From method: Hello</div>
```

<span data-ttu-id="94c0f-239">O código a seguir é a classe C# do Razor gerada:</span><span class="sxs-lookup"><span data-stu-id="94c0f-239">The following code is the generated Razor C# class:</span></span>

[!code-csharp[Main](razor/sample/Classes/Views_Home_Test_cshtml.cs?range=1-19)]

### <a name="section"></a>@section

<span data-ttu-id="94c0f-240">A diretiva `@section` é usada em conjunto com o [layout](xref:mvc/views/layout) para permitir que as exibições renderizem conteúdo em partes diferentes da página HTML.</span><span class="sxs-lookup"><span data-stu-id="94c0f-240">The `@section` directive is used in conjunction with the [layout](xref:mvc/views/layout) to enable views to render content in different parts of the HTML page.</span></span> <span data-ttu-id="94c0f-241">Para obter mais informações, consulte [Seções](xref:mvc/views/layout#layout-sections-label).</span><span class="sxs-lookup"><span data-stu-id="94c0f-241">For more information, see [Sections](xref:mvc/views/layout#layout-sections-label).</span></span>

## <a name="tag-helpers"></a><span data-ttu-id="94c0f-242">Auxiliares de Marca</span><span class="sxs-lookup"><span data-stu-id="94c0f-242">Tag Helpers</span></span>

<span data-ttu-id="94c0f-243">Há três diretivas que relacionadas aos [Auxiliares de marca](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="94c0f-243">There are three directives that pertain to [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

| <span data-ttu-id="94c0f-244">Diretiva</span><span class="sxs-lookup"><span data-stu-id="94c0f-244">Directive</span></span> | <span data-ttu-id="94c0f-245">Função</span><span class="sxs-lookup"><span data-stu-id="94c0f-245">Function</span></span> |
| --------- | -------- |
| [@addTagHelper](xref:mvc/views/tag-helpers/intro#add-helper-label) | <span data-ttu-id="94c0f-246">Disponibiliza os Auxiliares de marca para uma exibição.</span><span class="sxs-lookup"><span data-stu-id="94c0f-246">Makes Tag Helpers available to a view.</span></span> |
| [@removeTagHelper](xref:mvc/views/tag-helpers/intro#remove-razor-directives-label) | <span data-ttu-id="94c0f-247">Remove os Auxiliares de marca adicionados anteriormente de uma exibição.</span><span class="sxs-lookup"><span data-stu-id="94c0f-247">Removes Tag Helpers previously added from a view.</span></span> |
| [@tagHelperPrefix](xref:mvc/views/tag-helpers/intro#prefix-razor-directives-label) | <span data-ttu-id="94c0f-248">Especifica um prefixo de marca para habilitar o suporte do Auxiliar de marca e tornar explícito o uso do Auxiliar de marca.</span><span class="sxs-lookup"><span data-stu-id="94c0f-248">Specifies a tag prefix to enable Tag Helper support and to make Tag Helper usage explicit.</span></span> |

## <a name="razor-reserved-keywords"></a><span data-ttu-id="94c0f-249">Palavras-chave reservadas ao Razor</span><span class="sxs-lookup"><span data-stu-id="94c0f-249">Razor reserved keywords</span></span>

### <a name="razor-keywords"></a><span data-ttu-id="94c0f-250">Palavras-chave do Razor</span><span class="sxs-lookup"><span data-stu-id="94c0f-250">Razor keywords</span></span>

* <span data-ttu-id="94c0f-251">page (requer o ASP.NET Core 2.0 e posteriores)</span><span class="sxs-lookup"><span data-stu-id="94c0f-251">page (Requires ASP.NET Core 2.0 and later)</span></span>
* <span data-ttu-id="94c0f-252">funções</span><span class="sxs-lookup"><span data-stu-id="94c0f-252">functions</span></span>
* <span data-ttu-id="94c0f-253">herda</span><span class="sxs-lookup"><span data-stu-id="94c0f-253">inherits</span></span>
* <span data-ttu-id="94c0f-254">modelo</span><span class="sxs-lookup"><span data-stu-id="94c0f-254">model</span></span>
* <span data-ttu-id="94c0f-255">section</span><span class="sxs-lookup"><span data-stu-id="94c0f-255">section</span></span>
* <span data-ttu-id="94c0f-256">helper (atualmente sem suporte do ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="94c0f-256">helper (Not currently supported by ASP.NET Core)</span></span>

<span data-ttu-id="94c0f-257">Palavras-chave do Razor têm o escape feito com `@(Razor Keyword)` (por exemplo, `@(functions)`).</span><span class="sxs-lookup"><span data-stu-id="94c0f-257">Razor keywords are escaped with `@(Razor Keyword)` (for example, `@(functions)`).</span></span>

### <a name="c-razor-keywords"></a><span data-ttu-id="94c0f-258">Palavras-chave do Razor em C#</span><span class="sxs-lookup"><span data-stu-id="94c0f-258">C# Razor keywords</span></span>

* <span data-ttu-id="94c0f-259">case</span><span class="sxs-lookup"><span data-stu-id="94c0f-259">case</span></span>
* <span data-ttu-id="94c0f-260">do</span><span class="sxs-lookup"><span data-stu-id="94c0f-260">do</span></span>
* <span data-ttu-id="94c0f-261">default</span><span class="sxs-lookup"><span data-stu-id="94c0f-261">default</span></span>
* <span data-ttu-id="94c0f-262">for</span><span class="sxs-lookup"><span data-stu-id="94c0f-262">for</span></span>
* <span data-ttu-id="94c0f-263">foreach</span><span class="sxs-lookup"><span data-stu-id="94c0f-263">foreach</span></span>
* <span data-ttu-id="94c0f-264">if</span><span class="sxs-lookup"><span data-stu-id="94c0f-264">if</span></span>
* <span data-ttu-id="94c0f-265">else</span><span class="sxs-lookup"><span data-stu-id="94c0f-265">else</span></span>
* <span data-ttu-id="94c0f-266">bloqueio</span><span class="sxs-lookup"><span data-stu-id="94c0f-266">lock</span></span>
* <span data-ttu-id="94c0f-267">switch</span><span class="sxs-lookup"><span data-stu-id="94c0f-267">switch</span></span>
* <span data-ttu-id="94c0f-268">try</span><span class="sxs-lookup"><span data-stu-id="94c0f-268">try</span></span>
* <span data-ttu-id="94c0f-269">catch</span><span class="sxs-lookup"><span data-stu-id="94c0f-269">catch</span></span>
* <span data-ttu-id="94c0f-270">finally</span><span class="sxs-lookup"><span data-stu-id="94c0f-270">finally</span></span>
* <span data-ttu-id="94c0f-271">using</span><span class="sxs-lookup"><span data-stu-id="94c0f-271">using</span></span>
* <span data-ttu-id="94c0f-272">while</span><span class="sxs-lookup"><span data-stu-id="94c0f-272">while</span></span>

<span data-ttu-id="94c0f-273">Palavras-chave do Razor em C# precisam ter o escape duplo com `@(@C# Razor Keyword)` (por exemplo, `@(@case)`).</span><span class="sxs-lookup"><span data-stu-id="94c0f-273">C# Razor keywords must be double-escaped with `@(@C# Razor Keyword)` (for example, `@(@case)`).</span></span> <span data-ttu-id="94c0f-274">O primeiro `@` faz o escape do analisador Razor.</span><span class="sxs-lookup"><span data-stu-id="94c0f-274">The first `@` escapes the Razor parser.</span></span> <span data-ttu-id="94c0f-275">O segundo `@` faz o escape do analisador C#.</span><span class="sxs-lookup"><span data-stu-id="94c0f-275">The second `@` escapes the C# parser.</span></span>

### <a name="reserved-keywords-not-used-by-razor"></a><span data-ttu-id="94c0f-276">Palavras-chave reservadas não usadas pelo Razor</span><span class="sxs-lookup"><span data-stu-id="94c0f-276">Reserved keywords not used by Razor</span></span>

* <span data-ttu-id="94c0f-277">namespace</span><span class="sxs-lookup"><span data-stu-id="94c0f-277">namespace</span></span>
* <span data-ttu-id="94c0f-278">classe</span><span class="sxs-lookup"><span data-stu-id="94c0f-278">class</span></span>

## <a name="viewing-the-razor-c-class-generated-for-a-view"></a><span data-ttu-id="94c0f-279">Exibindo a classe C# do Razor gerada para uma exibição</span><span class="sxs-lookup"><span data-stu-id="94c0f-279">Viewing the Razor C# class generated for a view</span></span>

<span data-ttu-id="94c0f-280">Adicione a seguinte classe ao projeto do ASP.NET Core MVC:</span><span class="sxs-lookup"><span data-stu-id="94c0f-280">Add the following class to the ASP.NET Core MVC project:</span></span>

[!code-csharp[Main](razor/sample/Utilities/CustomTemplateEngine.cs)]

<span data-ttu-id="94c0f-281">Substitua o `RazorTemplateEngine` adicionado pelo MVC pela classe `CustomTemplateEngine`:</span><span class="sxs-lookup"><span data-stu-id="94c0f-281">Override the `RazorTemplateEngine` added by MVC with the `CustomTemplateEngine` class:</span></span>

[!code-csharp[Main](razor/sample/Startup.cs?highlight=4&range=10-14)]

<span data-ttu-id="94c0f-282">Defina um ponto de interrupção na instrução `return csharpDocument` de `CustomTemplateEngine`.</span><span class="sxs-lookup"><span data-stu-id="94c0f-282">Set a break point on the `return csharpDocument` statement of `CustomTemplateEngine`.</span></span> <span data-ttu-id="94c0f-283">Quando a execução do programa parar no ponto de interrupção, exiba o valor de `generatedCode`.</span><span class="sxs-lookup"><span data-stu-id="94c0f-283">When program execution stops at the break point, view the value of `generatedCode`.</span></span>

![Exibição do Visualizador de Texto de generatedCode](razor/_static/tvr.png)

## <a name="view-lookups-and-case-sensitivity"></a><span data-ttu-id="94c0f-285">Pesquisas de exibição e diferenciação de maiúsculas e minúsculas</span><span class="sxs-lookup"><span data-stu-id="94c0f-285">View lookups and case sensitivity</span></span>

<span data-ttu-id="94c0f-286">O mecanismo de exibição do Razor executa pesquisas que diferenciam maiúsculas de minúsculas para as exibições.</span><span class="sxs-lookup"><span data-stu-id="94c0f-286">The Razor view engine performs case-sensitive lookups for views.</span></span> <span data-ttu-id="94c0f-287">No entanto, a pesquisa real é determinada pelo sistema de arquivos subjacente:</span><span class="sxs-lookup"><span data-stu-id="94c0f-287">However, the actual lookup is determined by the underlying file system:</span></span>

* <span data-ttu-id="94c0f-288">Origem baseada em arquivo:</span><span class="sxs-lookup"><span data-stu-id="94c0f-288">File based source:</span></span> 
  * <span data-ttu-id="94c0f-289">Em sistemas operacionais com sistemas de arquivos que não diferenciam maiúsculas e minúsculas (por exemplo, Windows), pesquisas no provedor de arquivos físico não diferenciam maiúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="94c0f-289">On operating systems with case insensitive file systems (for example, Windows), physical file provider lookups are case insensitive.</span></span> <span data-ttu-id="94c0f-290">Por exemplo, `return View("Test")` resulta em correspondências para */Views/Home/Test.cshtml*, */Views/home/test.cshtml* e qualquer outra variação de maiúsculas e minúsculas.</span><span class="sxs-lookup"><span data-stu-id="94c0f-290">For example, `return View("Test")` results in matches for */Views/Home/Test.cshtml*, */Views/home/test.cshtml*, and any other casing variant.</span></span>
  * <span data-ttu-id="94c0f-291">Em sistemas de arquivos que diferenciam maiúsculas de minúsculas (por exemplo, Linux, OSX e com `EmbeddedFileProvider`), as pesquisas diferenciam maiúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="94c0f-291">On case-sensitive file systems (for example, Linux, OSX, and with `EmbeddedFileProvider`), lookups are case-sensitive.</span></span> <span data-ttu-id="94c0f-292">Por exemplo, `return View("Test")` corresponde especificamente a */Views/Home/Test.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="94c0f-292">For example, `return View("Test")` specifically matches */Views/Home/Test.cshtml*.</span></span>
* <span data-ttu-id="94c0f-293">Exibições pré-compiladas: com o ASP.NET Core 2.0 e posteriores, pesquisar em exibições pré-compiladas não diferencia maiúsculas de minúsculas em nenhum sistema operacional.</span><span class="sxs-lookup"><span data-stu-id="94c0f-293">Precompiled views: With ASP.NET Core 2.0 and later, looking up precompiled views is case insensitive on all operating systems.</span></span> <span data-ttu-id="94c0f-294">O comportamento é idêntico ao comportamento do provedor de arquivos físico no Windows.</span><span class="sxs-lookup"><span data-stu-id="94c0f-294">The behavior is identical to physical file provider's behavior on Windows.</span></span> <span data-ttu-id="94c0f-295">Se duas exibições pré-compiladas diferirem apenas quanto ao padrão de maiúsculas e minúsculas, o resultado da pesquisa não será determinístico.</span><span class="sxs-lookup"><span data-stu-id="94c0f-295">If two precompiled views differ only in case, the result of lookup is non-deterministic.</span></span>

<span data-ttu-id="94c0f-296">Os desenvolvedores são incentivados a fazer a correspondência entre as maiúsculas e minúsculas dos nomes dos arquivos e de diretórios com o uso de maiúsculas e minúsculas em:</span><span class="sxs-lookup"><span data-stu-id="94c0f-296">Developers are encouraged to match the casing of file and directory names to the casing of:</span></span>

    * <span data-ttu-id="94c0f-297">Nomes de área, controlador e ação.</span><span class="sxs-lookup"><span data-stu-id="94c0f-297">Area, controller, and action names.</span></span> 
    * <span data-ttu-id="94c0f-298">Páginas do Razor.</span><span class="sxs-lookup"><span data-stu-id="94c0f-298">Razor Pages.</span></span>
    
<span data-ttu-id="94c0f-299">Fazer essa correspondência garante que as implantações encontrem suas exibições, independentemente do sistema de arquivos subjacente.</span><span class="sxs-lookup"><span data-stu-id="94c0f-299">Matching case ensures the deployments find their views regardless of the underlying file system.</span></span>

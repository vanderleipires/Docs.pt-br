---
title: "Referência de sintaxe Razor ASP.NET Core"
author: rick-anderson
description: "Saiba mais sobre a sintaxe de marcação Razor para inserir o código de servidor em páginas da Web."
ms.author: riande
manager: wpickett
ms.date: 10/18/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/razor
ms.openlocfilehash: abdbb8112533d42f81180abad52f5ee86e3b280f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
# <a name="razor-syntax-for-aspnet-core"></a><span data-ttu-id="6512e-103">Sintaxe do Razor para o ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6512e-103">Razor syntax for ASP.NET Core</span></span>

<span data-ttu-id="6512e-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT), [Luke Latham](https://github.com/guardrex), [Taylor Mullen](https://twitter.com/ntaylormullen), e [Dan Vicarel](https://github.com/Rabadash8820)</span><span class="sxs-lookup"><span data-stu-id="6512e-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Luke Latham](https://github.com/guardrex), [Taylor Mullen](https://twitter.com/ntaylormullen), and [Dan Vicarel](https://github.com/Rabadash8820)</span></span>

<span data-ttu-id="6512e-105">Razor é uma sintaxe de marcação para inserir o código de servidor em páginas da Web.</span><span class="sxs-lookup"><span data-stu-id="6512e-105">Razor is a markup syntax for embedding server-based code into webpages.</span></span> <span data-ttu-id="6512e-106">A sintaxe do Razor consiste Razor marcação, c# e HTML.</span><span class="sxs-lookup"><span data-stu-id="6512e-106">The Razor syntax consists of Razor markup, C#, and HTML.</span></span> <span data-ttu-id="6512e-107">Arquivos que contêm Razor geralmente têm um *. cshtml* extensão de arquivo.</span><span class="sxs-lookup"><span data-stu-id="6512e-107">Files containing Razor generally have a *.cshtml* file extension.</span></span>

## <a name="rendering-html"></a><span data-ttu-id="6512e-108">Renderização HTML</span><span class="sxs-lookup"><span data-stu-id="6512e-108">Rendering HTML</span></span>

<span data-ttu-id="6512e-109">O idioma do Razor padrão é HTML.</span><span class="sxs-lookup"><span data-stu-id="6512e-109">The default Razor language is HTML.</span></span> <span data-ttu-id="6512e-110">Renderização HTML da marcação Razor não é diferente de renderização HTML de um arquivo HTML.</span><span class="sxs-lookup"><span data-stu-id="6512e-110">Rendering HTML from Razor markup is no different than rendering HTML from an HTML file.</span></span> <span data-ttu-id="6512e-111">A marcação HTML em *. cshtml* arquivos do Razor é processado pelo servidor inalterado.</span><span class="sxs-lookup"><span data-stu-id="6512e-111">HTML markup in *.cshtml* Razor files is rendered by the server unchanged.</span></span>

## <a name="razor-syntax"></a><span data-ttu-id="6512e-112">Sintaxe do Razor</span><span class="sxs-lookup"><span data-stu-id="6512e-112">Razor syntax</span></span>

<span data-ttu-id="6512e-113">Razor suporta c# e usa o `@` símbolo para fazer a transição de HTML para c#.</span><span class="sxs-lookup"><span data-stu-id="6512e-113">Razor supports C# and uses the `@` symbol to transition from HTML to C#.</span></span> <span data-ttu-id="6512e-114">Razor avalia expressões c# e renderiza-los na saída HTML.</span><span class="sxs-lookup"><span data-stu-id="6512e-114">Razor evaluates C# expressions and renders them in the HTML output.</span></span>

<span data-ttu-id="6512e-115">Quando um `@` símbolo é seguido por um [Razor reservados a palavra-chave](#razor-reserved-keywords), ele faz a transição para marcação específica do Razor.</span><span class="sxs-lookup"><span data-stu-id="6512e-115">When an `@` symbol is followed by a [Razor reserved keyword](#razor-reserved-keywords), it transitions into Razor-specific markup.</span></span> <span data-ttu-id="6512e-116">Caso contrário, ela faz a transição em c# simples.</span><span class="sxs-lookup"><span data-stu-id="6512e-116">Otherwise, it transitions into plain C#.</span></span>

<span data-ttu-id="6512e-117">Para escape um `@` símbolo na marcação Razor, usar um segundo `@` símbolo:</span><span class="sxs-lookup"><span data-stu-id="6512e-117">To escape an `@` symbol in Razor markup, use a second `@` symbol:</span></span>

```cshtml
<p>@@Username</p>
```

<span data-ttu-id="6512e-118">O código é renderizado em HTML com um único `@` símbolo:</span><span class="sxs-lookup"><span data-stu-id="6512e-118">The code is rendered in HTML with a single `@` symbol:</span></span>

```html
<p>@Username</p>
```

<span data-ttu-id="6512e-119">Atributos HTML e o conteúdo que contém endereços de email não tratam o `@` símbolo como um caractere de transição.</span><span class="sxs-lookup"><span data-stu-id="6512e-119">HTML attributes and content containing email addresses don't treat the `@` symbol as a transition character.</span></span> <span data-ttu-id="6512e-120">Os endereços de email no exemplo a seguir são inalterados por Razor análise:</span><span class="sxs-lookup"><span data-stu-id="6512e-120">The email addresses in the following example are untouched by Razor parsing:</span></span>

```cshtml
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
```

## <a name="implicit-razor-expressions"></a><span data-ttu-id="6512e-121">Expressões implícitas do Razor</span><span class="sxs-lookup"><span data-stu-id="6512e-121">Implicit Razor expressions</span></span>

<span data-ttu-id="6512e-122">As expressões implícitas Razor iniciar com `@` seguido do código do c#:</span><span class="sxs-lookup"><span data-stu-id="6512e-122">Implicit Razor expressions start with `@` followed by C# code:</span></span>

```cshtml
<p>@DateTime.Now</p>
<p>@DateTime.IsLeapYear(2016)</p>
```

<span data-ttu-id="6512e-123">Com exceção do c# `await` palavra-chave, expressões implícitas não devem conter espaços.</span><span class="sxs-lookup"><span data-stu-id="6512e-123">With the exception of the C# `await` keyword, implicit expressions must not contain spaces.</span></span> <span data-ttu-id="6512e-124">Se a instrução c# tem uma terminação clara, espaços podem misturar:</span><span class="sxs-lookup"><span data-stu-id="6512e-124">If the C# statement has a clear ending, spaces can be intermingled:</span></span>

```cshtml
<p>@await DoSomething("hello", "world")</p>
```

<span data-ttu-id="6512e-125">Expressões implícitas **não é possível** contêm c# genéricos, como os caracteres dentro dos colchetes (`<>`) são interpretados como uma marca HTML.</span><span class="sxs-lookup"><span data-stu-id="6512e-125">Implicit expressions **cannot** contain C# generics, as the characters inside the brackets (`<>`) are interpreted as an HTML tag.</span></span> <span data-ttu-id="6512e-126">O código a seguir é **não** válido:</span><span class="sxs-lookup"><span data-stu-id="6512e-126">The following code is **not** valid:</span></span>

```cshtml
<p>@GenericMethod<int>()</p>
```

<span data-ttu-id="6512e-127">O código anterior gera um erro do compilador semelhante a uma das seguintes opções:</span><span class="sxs-lookup"><span data-stu-id="6512e-127">The preceding code generates a compiler error similar to one of the following:</span></span>

 * <span data-ttu-id="6512e-128">O elemento "int" não foi fechado.</span><span class="sxs-lookup"><span data-stu-id="6512e-128">The "int" element wasn't closed.</span></span> <span data-ttu-id="6512e-129">Todos os elementos devem ser de fechamento automático ou ter uma correspondência de marca de fim.</span><span class="sxs-lookup"><span data-stu-id="6512e-129">All elements must be either self-closing or have a matching end tag.</span></span>
 *  <span data-ttu-id="6512e-130">Não é possível converter o grupo de métodos 'GenericMethod' para 'object' de tipo não delegado.</span><span class="sxs-lookup"><span data-stu-id="6512e-130">Cannot convert method group 'GenericMethod' to non-delegate type 'object'.</span></span> <span data-ttu-id="6512e-131">Você pretendia chamar o método?'</span><span class="sxs-lookup"><span data-stu-id="6512e-131">Did you intend to invoke the method?\`</span></span> 
 
<span data-ttu-id="6512e-132">Chamadas de método genérico devem ser encapsuladas em um [expressão Razor explícita](#explicit-razor-expressions) ou um [bloco de código Razor](#razor-code-blocks).</span><span class="sxs-lookup"><span data-stu-id="6512e-132">Generic method calls must be wrapped in an [explicit Razor expression](#explicit-razor-expressions) or a [Razor code block](#razor-code-blocks).</span></span>

## <a name="explicit-razor-expressions"></a><span data-ttu-id="6512e-133">Expressões explícitas Razor</span><span class="sxs-lookup"><span data-stu-id="6512e-133">Explicit Razor expressions</span></span>

<span data-ttu-id="6512e-134">As expressões explícitas Razor consistem em um `@` símbolo com parênteses equilibrada.</span><span class="sxs-lookup"><span data-stu-id="6512e-134">Explicit Razor expressions consist of an `@` symbol with balanced parenthesis.</span></span> <span data-ttu-id="6512e-135">Para renderizar a hora da última semana, a seguinte marcação Razor é usada:</span><span class="sxs-lookup"><span data-stu-id="6512e-135">To render last week's time, the following Razor markup is used:</span></span>

```cshtml
<p>Last week this time: @(DateTime.Now - TimeSpan.FromDays(7))</p>
```

<span data-ttu-id="6512e-136">Qualquer conteúdo dentro do `@()` parênteses é avaliado e renderizado para a saída.</span><span class="sxs-lookup"><span data-stu-id="6512e-136">Any content within the `@()` parenthesis is evaluated and rendered to the output.</span></span>

<span data-ttu-id="6512e-137">Expressões implícitas, descritas na seção anterior, geralmente não podem conter espaços.</span><span class="sxs-lookup"><span data-stu-id="6512e-137">Implicit expressions, described in the previous section, generally can't contain spaces.</span></span> <span data-ttu-id="6512e-138">No código a seguir, uma semana não é subtraída da hora atual:</span><span class="sxs-lookup"><span data-stu-id="6512e-138">In the following code, one week isn't subtracted from the current time:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact.cshtml?range=17)]

<span data-ttu-id="6512e-139">O código processa o HTML a seguir:</span><span class="sxs-lookup"><span data-stu-id="6512e-139">The code renders the following HTML:</span></span>

```html
<p>Last week: 7/7/2016 4:39:52 PM - TimeSpan.FromDays(7)</p>
```

<span data-ttu-id="6512e-140">As expressões explícitas podem ser usadas para concatenação de texto com um resultado da expressão:</span><span class="sxs-lookup"><span data-stu-id="6512e-140">Explicit expressions can be used to concatenate text with an expression result:</span></span>

```cshtml
@{
    var joe = new Person("Joe", 33);
}

<p>Age@(joe.Age)</p>
```

<span data-ttu-id="6512e-141">Sem expressão explícita, `<p>Age@joe.Age</p>` é tratado como um endereço de email, e `<p>Age@joe.Age</p>` é renderizado.</span><span class="sxs-lookup"><span data-stu-id="6512e-141">Without the explicit expression, `<p>Age@joe.Age</p>` is treated as an email address, and `<p>Age@joe.Age</p>` is rendered.</span></span> <span data-ttu-id="6512e-142">Quando gravado como uma expressão explícita, `<p>Age33</p>` é renderizado.</span><span class="sxs-lookup"><span data-stu-id="6512e-142">When written as an explicit expression, `<p>Age33</p>` is rendered.</span></span>


<span data-ttu-id="6512e-143">Expressões explícitas podem ser usadas para processar a saída de métodos genéricos em *. cshtml* arquivos.</span><span class="sxs-lookup"><span data-stu-id="6512e-143">Explicit expressions can be used to render output from generic methods in *.cshtml* files.</span></span> <span data-ttu-id="6512e-144">Em uma expressão implícita, os caracteres dentro dos colchetes (`<>`) são interpretados como uma marca HTML.</span><span class="sxs-lookup"><span data-stu-id="6512e-144">In an implicit expression, the characters inside the brackets (`<>`) are interpreted as an HTML tag.</span></span> <span data-ttu-id="6512e-145">É a seguinte marcação **não** Razor válido:</span><span class="sxs-lookup"><span data-stu-id="6512e-145">The following markup is **not** valid Razor:</span></span>

```cshtml
<p>@GenericMethod<int>()</p>
```

<span data-ttu-id="6512e-146">O código anterior gera um erro do compilador semelhante a uma das seguintes opções:</span><span class="sxs-lookup"><span data-stu-id="6512e-146">The preceding code generates a compiler error similar to one of the following:</span></span>

 * <span data-ttu-id="6512e-147">O elemento "int" não foi fechado.</span><span class="sxs-lookup"><span data-stu-id="6512e-147">The "int" element wasn't closed.</span></span> <span data-ttu-id="6512e-148">Todos os elementos devem ser de fechamento automático ou ter uma correspondência de marca de fim.</span><span class="sxs-lookup"><span data-stu-id="6512e-148">All elements must be either self-closing or have a matching end tag.</span></span>
 *  <span data-ttu-id="6512e-149">Não é possível converter o grupo de métodos 'GenericMethod' para 'object' de tipo não delegado.</span><span class="sxs-lookup"><span data-stu-id="6512e-149">Cannot convert method group 'GenericMethod' to non-delegate type 'object'.</span></span> <span data-ttu-id="6512e-150">Você pretendia chamar o método?'</span><span class="sxs-lookup"><span data-stu-id="6512e-150">Did you intend to invoke the method?\`</span></span> 
 
 <span data-ttu-id="6512e-151">A marcação a seguir mostra a gravação de maneira correta esse código.</span><span class="sxs-lookup"><span data-stu-id="6512e-151">The following markup shows the correct way write this code.</span></span> <span data-ttu-id="6512e-152">O código é escrito como uma expressão explícita:</span><span class="sxs-lookup"><span data-stu-id="6512e-152">The code is written as an explicit expression:</span></span>

```cshtml
<p>@(GenericMethod<int>())</p>
```

## <a name="expression-encoding"></a><span data-ttu-id="6512e-153">Codificação de expressão</span><span class="sxs-lookup"><span data-stu-id="6512e-153">Expression encoding</span></span>

<span data-ttu-id="6512e-154">Expressões em c# que são avaliadas como uma cadeia de caracteres estão codificada em HTML.</span><span class="sxs-lookup"><span data-stu-id="6512e-154">C# expressions that evaluate to a string are HTML encoded.</span></span> <span data-ttu-id="6512e-155">Expressões em c# que avaliam `IHtmlContent` são renderizados diretamente por meio `IHtmlContent.WriteTo`.</span><span class="sxs-lookup"><span data-stu-id="6512e-155">C# expressions that evaluate to `IHtmlContent` are rendered directly through `IHtmlContent.WriteTo`.</span></span> <span data-ttu-id="6512e-156">Expressões em c# que não retornam `IHtmlContent` são convertidos em uma cadeia de caracteres por `ToString` e codificado antes que eles são renderizados.</span><span class="sxs-lookup"><span data-stu-id="6512e-156">C# expressions that don't evaluate to `IHtmlContent` are converted to a string by `ToString` and encoded before they're rendered.</span></span>

```cshtml
@("<span>Hello World</span>")
```

<span data-ttu-id="6512e-157">O código processa o HTML a seguir:</span><span class="sxs-lookup"><span data-stu-id="6512e-157">The code renders the following HTML:</span></span>

```html
&lt;span&gt;Hello World&lt;/span&gt;
```

<span data-ttu-id="6512e-158">O HTML é mostrado no navegador como:</span><span class="sxs-lookup"><span data-stu-id="6512e-158">The HTML is shown in the browser as:</span></span>

```
<span>Hello World</span>
```

<span data-ttu-id="6512e-159">`HtmlHelper.Raw`saída não é codificada mas renderizada como marcação HTML.</span><span class="sxs-lookup"><span data-stu-id="6512e-159">`HtmlHelper.Raw` output isn't encoded but rendered as HTML markup.</span></span>

> [!WARNING]
> <span data-ttu-id="6512e-160">Usando `HtmlHelper.Raw` usuário unsanitized a entrada é um risco de segurança.</span><span class="sxs-lookup"><span data-stu-id="6512e-160">Using `HtmlHelper.Raw` on unsanitized user input is a security risk.</span></span> <span data-ttu-id="6512e-161">Entrada do usuário pode conter outras explorações ou JavaScript mal-intencionados.</span><span class="sxs-lookup"><span data-stu-id="6512e-161">User input might contain malicious JavaScript or other exploits.</span></span> <span data-ttu-id="6512e-162">Limpeza de entrada do usuário é difícil.</span><span class="sxs-lookup"><span data-stu-id="6512e-162">Sanitizing user input is difficult.</span></span> <span data-ttu-id="6512e-163">Evite usar `HtmlHelper.Raw` com entrada do usuário.</span><span class="sxs-lookup"><span data-stu-id="6512e-163">Avoid using `HtmlHelper.Raw` with user input.</span></span>

```cshtml
@Html.Raw("<span>Hello World</span>")
```

<span data-ttu-id="6512e-164">O código processa o HTML a seguir:</span><span class="sxs-lookup"><span data-stu-id="6512e-164">The code renders the following HTML:</span></span>

```html
<span>Hello World</span>
```

## <a name="razor-code-blocks"></a><span data-ttu-id="6512e-165">Blocos de código Razor</span><span class="sxs-lookup"><span data-stu-id="6512e-165">Razor code blocks</span></span>

<span data-ttu-id="6512e-166">Blocos de código Razor iniciar com `@` e são delimitados por `{}`.</span><span class="sxs-lookup"><span data-stu-id="6512e-166">Razor code blocks start with `@` and are enclosed by `{}`.</span></span> <span data-ttu-id="6512e-167">Ao contrário das expressões, o código c# dentro de blocos de código não está processado.</span><span class="sxs-lookup"><span data-stu-id="6512e-167">Unlike expressions, C# code inside code blocks isn't rendered.</span></span> <span data-ttu-id="6512e-168">Blocos de código e expressões em uma exibição compartilhem o mesmo escopo e são definidas em ordem:</span><span class="sxs-lookup"><span data-stu-id="6512e-168">Code blocks and expressions in a view share the same scope and are defined in order:</span></span>

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

<span data-ttu-id="6512e-169">O código processa o HTML a seguir:</span><span class="sxs-lookup"><span data-stu-id="6512e-169">The code renders the following HTML:</span></span>

```html
<p>The future depends on what you do today. - Mahatma Gandhi</p>
<p>Hate cannot drive out hate, only love can do that. - Martin Luther King, Jr.</p>
```

### <a name="implicit-transitions"></a><span data-ttu-id="6512e-170">Transições implícita</span><span class="sxs-lookup"><span data-stu-id="6512e-170">Implicit transitions</span></span>

<span data-ttu-id="6512e-171">É o idioma padrão em um bloco de código c#, mas a página Razor pode fazer a transição para HTML:</span><span class="sxs-lookup"><span data-stu-id="6512e-171">The default language in a code block is C#, but the Razor Page can transition back to HTML:</span></span>

```cshtml
@{
    var inCSharp = true;
    <p>Now in HTML, was in C# @inCSharp</p>
}
```

### <a name="explicit-delimited-transition"></a><span data-ttu-id="6512e-172">Transição delimitada explícita</span><span class="sxs-lookup"><span data-stu-id="6512e-172">Explicit delimited transition</span></span>

<span data-ttu-id="6512e-173">Para definir uma subseção de um bloco de código que deve renderizar HTML, coloque os caracteres para renderização o Razor  **\<texto >** marca:</span><span class="sxs-lookup"><span data-stu-id="6512e-173">To define a subsection of a code block that should render HTML, surround the characters for rendering with the Razor **\<text>** tag:</span></span>

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <text>Name: @person.Name</text>
}
```

<span data-ttu-id="6512e-174">Use essa abordagem para renderizar HTML que não está circundado por uma marca HTML.</span><span class="sxs-lookup"><span data-stu-id="6512e-174">Use this approach to render HTML that isn't surrounded by an HTML tag.</span></span> <span data-ttu-id="6512e-175">Sem uma marca HTML ou Razor, ocorrerá um erro de tempo de execução do Razor.</span><span class="sxs-lookup"><span data-stu-id="6512e-175">Without an HTML or Razor tag, a Razor runtime error occurs.</span></span>

<span data-ttu-id="6512e-176">O  **\<texto >** marca é útil para controlar o espaço em branco ao renderizar o conteúdo:</span><span class="sxs-lookup"><span data-stu-id="6512e-176">The **\<text>** tag is useful to control whitespace when rendering content:</span></span>

* <span data-ttu-id="6512e-177">Somente o conteúdo entre o  **\<texto >** marca é processada.</span><span class="sxs-lookup"><span data-stu-id="6512e-177">Only the content between the **\<text>** tag is rendered.</span></span> 
* <span data-ttu-id="6512e-178">Não há espaço em branco antes ou depois do  **\<texto >** marca aparece na saída HTML.</span><span class="sxs-lookup"><span data-stu-id="6512e-178">No whitespace before or after the **\<text>** tag appears in the HTML output.</span></span>

### <a name="explicit-line-transition-with-"></a><span data-ttu-id="6512e-179">Transição de linha explícita com @:</span><span class="sxs-lookup"><span data-stu-id="6512e-179">Explicit Line Transition with @:</span></span>

<span data-ttu-id="6512e-180">Para processar o restante de uma linha inteira como HTML dentro de um bloco de código, use o `@:` sintaxe:</span><span class="sxs-lookup"><span data-stu-id="6512e-180">To render the rest of an entire line as HTML inside a code block, use the `@:` syntax:</span></span>

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    @:Name: @person.Name
}
```

<span data-ttu-id="6512e-181">Sem o `@:` no código, será gerado um erro de tempo de execução do Razor.</span><span class="sxs-lookup"><span data-stu-id="6512e-181">Without the `@:` in the code, a Razor runtime error is generated.</span></span>

<span data-ttu-id="6512e-182">Aviso: Extra `@` caracteres em um arquivo Razor podem causar causar erros de compilador em instruções posteriormente no bloco.</span><span class="sxs-lookup"><span data-stu-id="6512e-182">Warning: Extra `@` characters in a Razor file can cause cause compiler errors at statements later in the block.</span></span> <span data-ttu-id="6512e-183">Esses erros de compilador podem ser difícil de entender porque o erro real ocorre antes do erro relatado.</span><span class="sxs-lookup"><span data-stu-id="6512e-183">These compiler errors can be difficult to understand because the actual error occurs before the reported error.</span></span> <span data-ttu-id="6512e-184">Esse erro é comum após a combinação de várias expressões implícitas explícita em um bloco de código único.</span><span class="sxs-lookup"><span data-stu-id="6512e-184">This error is common after combining multiple implicit/explicit expressions into a single code block.</span></span>

## <a name="control-structures"></a><span data-ttu-id="6512e-185">Estruturas de controle</span><span class="sxs-lookup"><span data-stu-id="6512e-185">Control Structures</span></span>

<span data-ttu-id="6512e-186">Estruturas de controle são uma extensão dos blocos de código.</span><span class="sxs-lookup"><span data-stu-id="6512e-186">Control structures are an extension of code blocks.</span></span> <span data-ttu-id="6512e-187">Todos os aspectos dos blocos de código (em transição para marcação, embutido c#) também se aplicam às seguintes estruturas:</span><span class="sxs-lookup"><span data-stu-id="6512e-187">All aspects of code blocks (transitioning to markup, inline C#) also apply to the following structures:</span></span>

### <a name="conditionals-if-else-if-else-and-switch"></a><span data-ttu-id="6512e-188">Condicionais @if, senão se, para outro, e@switch</span><span class="sxs-lookup"><span data-stu-id="6512e-188">Conditionals @if, else if, else, and @switch</span></span>

<span data-ttu-id="6512e-189">`@if`Controla quando o código é executado:</span><span class="sxs-lookup"><span data-stu-id="6512e-189">`@if` controls when code runs:</span></span>

```cshtml
@if (value % 2 == 0)
{
    <p>The value was even.</p>
}
```

<span data-ttu-id="6512e-190">`else`e `else if` não exigem o `@` símbolo:</span><span class="sxs-lookup"><span data-stu-id="6512e-190">`else` and `else if` don't require the `@` symbol:</span></span>

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

<span data-ttu-id="6512e-191">A marcação a seguir mostra como usar uma instrução de opção:</span><span class="sxs-lookup"><span data-stu-id="6512e-191">The following markup shows how to use a switch statement:</span></span>

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

### <a name="looping-for-foreach-while-and-do-while"></a><span data-ttu-id="6512e-192">Loop @for, @foreach, @while, e @do enquanto</span><span class="sxs-lookup"><span data-stu-id="6512e-192">Looping @for, @foreach, @while, and @do while</span></span>

<span data-ttu-id="6512e-193">Modelo HTML pode ser renderizado com instruções de controle de loop.</span><span class="sxs-lookup"><span data-stu-id="6512e-193">Templated HTML can be rendered with looping control statements.</span></span> <span data-ttu-id="6512e-194">Para renderizar uma lista de pessoas:</span><span class="sxs-lookup"><span data-stu-id="6512e-194">To render a list of people:</span></span>

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

<span data-ttu-id="6512e-195">Há suporte para as seguintes instruções de loop:</span><span class="sxs-lookup"><span data-stu-id="6512e-195">The following looping statements are supported:</span></span>

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

### <a name="compound-using"></a><span data-ttu-id="6512e-196">Composta@using</span><span class="sxs-lookup"><span data-stu-id="6512e-196">Compound @using</span></span>

<span data-ttu-id="6512e-197">No c#, um `using` instrução é usada para garantir que um objeto é descartado.</span><span class="sxs-lookup"><span data-stu-id="6512e-197">In C#, a `using` statement is used to ensure an object is disposed.</span></span> <span data-ttu-id="6512e-198">No Razor, o mesmo mecanismo é usado para criar os auxiliares HTML que contém o conteúdo adicional.</span><span class="sxs-lookup"><span data-stu-id="6512e-198">In Razor, the same mechanism is used to create HTML Helpers that contain additional content.</span></span> <span data-ttu-id="6512e-199">No código a seguir, auxiliares HTML renderizar uma marca de formulário com o `@using` instrução:</span><span class="sxs-lookup"><span data-stu-id="6512e-199">In the following code, HTML Helpers render a form tag with the `@using` statement:</span></span>


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

<span data-ttu-id="6512e-200">Ações no nível de escopo podem ser executadas com [auxiliares de marcação](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="6512e-200">Scope-level actions can be performed with [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

### <a name="try-catch-finally"></a><span data-ttu-id="6512e-201">@try, catch, finally</span><span class="sxs-lookup"><span data-stu-id="6512e-201">@try, catch, finally</span></span>

<span data-ttu-id="6512e-202">Tratamento de exceção é semelhante ao c#:</span><span class="sxs-lookup"><span data-stu-id="6512e-202">Exception handling is similar to C#:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact7.cshtml)]

### <a name="lock"></a>@lock

<span data-ttu-id="6512e-203">Razor tem a capacidade de proteger seções críticas com instruções de bloqueio:</span><span class="sxs-lookup"><span data-stu-id="6512e-203">Razor has the capability to protect critical sections with lock statements:</span></span>

```cshtml
@lock (SomeLock)
{
    // Do critical section work
}
```

### <a name="comments"></a><span data-ttu-id="6512e-204">Comentários</span><span class="sxs-lookup"><span data-stu-id="6512e-204">Comments</span></span>

<span data-ttu-id="6512e-205">Razor dá suporte a comentários c# e HTML:</span><span class="sxs-lookup"><span data-stu-id="6512e-205">Razor supports C# and HTML comments:</span></span>

```cshtml
@{
    /* C# comment */
    // Another C# comment
}
<!-- HTML comment -->
```

<span data-ttu-id="6512e-206">O código processa o HTML a seguir:</span><span class="sxs-lookup"><span data-stu-id="6512e-206">The code renders the following HTML:</span></span>

```html
<!-- HTML comment -->
```

<span data-ttu-id="6512e-207">Comentários de Razor serão removidos pelo servidor para a página da Web seja processada.</span><span class="sxs-lookup"><span data-stu-id="6512e-207">Razor comments are removed by the server before the webpage is rendered.</span></span> <span data-ttu-id="6512e-208">Razor usa `@*  *@` para delimitar os comentários.</span><span class="sxs-lookup"><span data-stu-id="6512e-208">Razor uses `@*  *@` to delimit comments.</span></span> <span data-ttu-id="6512e-209">O código a seguir é comentado, para que o servidor não processa qualquer marcação:</span><span class="sxs-lookup"><span data-stu-id="6512e-209">The following code is commented out, so the server doesn't render any markup:</span></span>

```cshtml
@*
    @{
        /* C# comment */
        // Another C# comment
    }
    <!-- HTML comment -->
*@
```

## <a name="directives"></a><span data-ttu-id="6512e-210">Diretivas</span><span class="sxs-lookup"><span data-stu-id="6512e-210">Directives</span></span>

<span data-ttu-id="6512e-211">Diretivas de Razor são representadas por expressões implícitas com os seguintes palavras-chave reservadas do `@` símbolo.</span><span class="sxs-lookup"><span data-stu-id="6512e-211">Razor directives are represented by implicit expressions with reserved keywords following the `@` symbol.</span></span> <span data-ttu-id="6512e-212">Uma diretiva geralmente altera o modo como uma exibição é analisada ou habilita a funcionalidade diferente.</span><span class="sxs-lookup"><span data-stu-id="6512e-212">A directive typically changes the way a view is parsed or enables different functionality.</span></span>

<span data-ttu-id="6512e-213">Noções básicas sobre como Razor gera código para um modo de exibição torna mais fácil de entender como funcionam as diretivas.</span><span class="sxs-lookup"><span data-stu-id="6512e-213">Understanding how Razor generates code for a view makes it easier to understand how directives work.</span></span>

[!code-html[Main](razor/sample/Views/Home/Contact8.cshtml)]

<span data-ttu-id="6512e-214">O código gera uma classe semelhante à seguinte:</span><span class="sxs-lookup"><span data-stu-id="6512e-214">The code generates a class similar to the following:</span></span>

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

<span data-ttu-id="6512e-215">Posteriormente neste artigo, a seção [exibindo a classe Razor c# gerada para uma exibição](#viewing-the-razor-c-class-generated-for-a-view) explica como exibir essa classe gerada.</span><span class="sxs-lookup"><span data-stu-id="6512e-215">Later in this article, the section [Viewing the Razor C# class generated for a view](#viewing-the-razor-c-class-generated-for-a-view) explains how to view this generated class.</span></span>

### <a name="using"></a>@using

<span data-ttu-id="6512e-216">O `@using` diretiva adiciona c# `using` diretiva para o modo de exibição gerado:</span><span class="sxs-lookup"><span data-stu-id="6512e-216">The `@using` directive adds the C# `using` directive to the generated view:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact9.cshtml)]

### <a name="model"></a>@model

<span data-ttu-id="6512e-217">O `@model` diretiva especifica o tipo de modelo passado para um modo de exibição:</span><span class="sxs-lookup"><span data-stu-id="6512e-217">The `@model` directive specifies the type of the model passed to a view:</span></span>

```cshtml
@model TypeNameOfModel
```

<span data-ttu-id="6512e-218">Em um aplicativo MVC do ASP.NET Core criado com contas de usuário individuais, o *Views/Account/Login.cshtml* exibição contém a declaração de modelo a seguir:</span><span class="sxs-lookup"><span data-stu-id="6512e-218">In an ASP.NET Core MVC app created with individual user accounts, the *Views/Account/Login.cshtml* view contains the following model declaration:</span></span>

```cshtml
@model LoginViewModel
```

<span data-ttu-id="6512e-219">A classe gerada herda de `RazorPage<dynamic>`:</span><span class="sxs-lookup"><span data-stu-id="6512e-219">The class generated inherits from `RazorPage<dynamic>`:</span></span>

```csharp
public class _Views_Account_Login_cshtml : RazorPage<LoginViewModel>
```

<span data-ttu-id="6512e-220">Razor expõe um `Model` propriedade para acessar o modelo passado para o modo de exibição:</span><span class="sxs-lookup"><span data-stu-id="6512e-220">Razor exposes a `Model` property for accessing the model passed to the view:</span></span>

```cshtml
<div>The Login Email: @Model.Email</div>
```

<span data-ttu-id="6512e-221">O `@model` diretiva especifica o tipo desta propriedade.</span><span class="sxs-lookup"><span data-stu-id="6512e-221">The `@model` directive specifies the type of this property.</span></span> <span data-ttu-id="6512e-222">Especifica a diretiva de `T` em `RazorPage<T>` o modo de exibição que gerado classe que deriva.</span><span class="sxs-lookup"><span data-stu-id="6512e-222">The directive specifies the `T` in `RazorPage<T>` that the generated class that the view derives from.</span></span> <span data-ttu-id="6512e-223">Se o `@model` diretiva especificado, o `Model` é de propriedade do tipo `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="6512e-223">If the `@model` directive iisn't specified, the `Model` property is of type `dynamic`.</span></span> <span data-ttu-id="6512e-224">O valor do modelo é passado do controlador para o modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="6512e-224">The value of the model is passed from the controller to the view.</span></span> <span data-ttu-id="6512e-225">Para obter mais informações, consulte [fortemente tipado modelos e o @model palavra-chave.</span><span class="sxs-lookup"><span data-stu-id="6512e-225">For more information, see [Strongly typed models and the @model keyword.</span></span>

### <a name="inherits"></a>@inherits

<span data-ttu-id="6512e-226">O `@inherits` diretiva fornece controle total da classe herda o modo de exibição:</span><span class="sxs-lookup"><span data-stu-id="6512e-226">The `@inherits` directive provides full control of the class the view inherits:</span></span>

```cshtml
@inherits TypeNameOfClassToInheritFrom
```

<span data-ttu-id="6512e-227">O código a seguir é um tipo de página Razor personalizado:</span><span class="sxs-lookup"><span data-stu-id="6512e-227">The following code is a custom Razor page type:</span></span>

[!code-csharp[Main](razor/sample/Classes/CustomRazorPage.cs)]

<span data-ttu-id="6512e-228">O `CustomText` é exibido em um modo de exibição:</span><span class="sxs-lookup"><span data-stu-id="6512e-228">The `CustomText` is displayed in a view:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact10.cshtml)]

<span data-ttu-id="6512e-229">O código processa o HTML a seguir:</span><span class="sxs-lookup"><span data-stu-id="6512e-229">The code renders the following HTML:</span></span>

```html
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

 <span data-ttu-id="6512e-230">`@model`e `@inherits` podem ser usados na mesma exibição.</span><span class="sxs-lookup"><span data-stu-id="6512e-230">`@model` and `@inherits` can be used in the same view.</span></span> <span data-ttu-id="6512e-231">`@inherits`pode estar em um *viewimports. cshtml* arquivo que importa o modo de exibição:</span><span class="sxs-lookup"><span data-stu-id="6512e-231">`@inherits` can be in a *_ViewImports.cshtml* file that the view imports:</span></span>

[!code-cshtml[Main](razor/sample/Views/_ViewImportsModel.cshtml)]

<span data-ttu-id="6512e-232">O código a seguir é um exemplo de uma exibição fortemente tipado:</span><span class="sxs-lookup"><span data-stu-id="6512e-232">The following code is an example of a strongly-typed view:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Login1.cshtml)]

<span data-ttu-id="6512e-233">Se "rick@contoso.com" é passado no modelo, o modo de exibição gera uma marcação HTML a seguir:</span><span class="sxs-lookup"><span data-stu-id="6512e-233">If "rick@contoso.com" is passed in the model, the view generates the following HTML markup:</span></span>

```html
<div>The Login Email: rick@contoso.com</div>
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

### <a name="inject"></a>@inject


<span data-ttu-id="6512e-234">O `@inject` diretiva permite que a página do Razor para injetar um serviço a partir de [contêiner de serviço](xref:fundamentals/dependency-injection) em uma exibição.</span><span class="sxs-lookup"><span data-stu-id="6512e-234">The `@inject` directive enables the Razor Page to inject a service from the [service container](xref:fundamentals/dependency-injection) into a view.</span></span> <span data-ttu-id="6512e-235">Para obter mais informações, consulte [injeção de dependência para modos de exibição](xref:mvc/views/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="6512e-235">For more information, see [Dependency injection into views](xref:mvc/views/dependency-injection).</span></span>

### <a name="functions"></a>@functions

<span data-ttu-id="6512e-236">O `@functions` diretiva permite que uma página do Razor para adicionar conteúdo no nível de função para um modo de exibição:</span><span class="sxs-lookup"><span data-stu-id="6512e-236">The `@functions` directive enables a Razor Page to add function-level content to a view:</span></span>

```cshtml
@functions { // C# Code }
```

<span data-ttu-id="6512e-237">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="6512e-237">For example:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact6.cshtml)]

<span data-ttu-id="6512e-238">O código gera uma marcação HTML a seguir:</span><span class="sxs-lookup"><span data-stu-id="6512e-238">The code generates the following HTML markup:</span></span>

```html
<div>From method: Hello</div>
```

<span data-ttu-id="6512e-239">O código a seguir é a classe gerada Razor c#:</span><span class="sxs-lookup"><span data-stu-id="6512e-239">The following code is the generated Razor C# class:</span></span>

[!code-csharp[Main](razor/sample/Classes/Views_Home_Test_cshtml.cs?range=1-19)]

### <a name="section"></a>@section

<span data-ttu-id="6512e-240">O `@section` diretiva é usada em conjunto com o [layout](xref:mvc/views/layout) para habilitar os modos de exibição renderizar o conteúdo em diferentes partes da página HTML.</span><span class="sxs-lookup"><span data-stu-id="6512e-240">The `@section` directive is used in conjunction with the [layout](xref:mvc/views/layout) to enable views to render content in different parts of the HTML page.</span></span> <span data-ttu-id="6512e-241">Para obter mais informações, consulte [seções](xref:mvc/views/layout#layout-sections-label).</span><span class="sxs-lookup"><span data-stu-id="6512e-241">For more information, see [Sections](xref:mvc/views/layout#layout-sections-label).</span></span>

## <a name="tag-helpers"></a><span data-ttu-id="6512e-242">Auxiliares de marcação</span><span class="sxs-lookup"><span data-stu-id="6512e-242">Tag Helpers</span></span>

<span data-ttu-id="6512e-243">Há três diretivas que pertencem ao [auxiliares de marcação](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="6512e-243">There are three directives that pertain to [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

| <span data-ttu-id="6512e-244">Diretiva</span><span class="sxs-lookup"><span data-stu-id="6512e-244">Directive</span></span> | <span data-ttu-id="6512e-245">Função</span><span class="sxs-lookup"><span data-stu-id="6512e-245">Function</span></span> |
| --------- | -------- |
| [@addTagHelper](xref:mvc/views/tag-helpers/intro#add-helper-label) | <span data-ttu-id="6512e-246">Disponibiliza auxiliares de marcação para um modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="6512e-246">Makes Tag Helpers available to a view.</span></span> |
| [@removeTagHelper](xref:mvc/views/tag-helpers/intro#remove-razor-directives-label) | <span data-ttu-id="6512e-247">Remove os auxiliares de marcação adicionado anteriormente de um modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="6512e-247">Removes Tag Helpers previously added from a view.</span></span> |
| [@tagHelperPrefix](xref:mvc/views/tag-helpers/intro#prefix-razor-directives-label) | <span data-ttu-id="6512e-248">Especifica um prefixo de marca para habilitar o suporte auxiliar de marca e fazer uso do auxiliar de marca explícita.</span><span class="sxs-lookup"><span data-stu-id="6512e-248">Specifies a tag prefix to enable Tag Helper support and to make Tag Helper usage explicit.</span></span> |

## <a name="razor-reserved-keywords"></a><span data-ttu-id="6512e-249">Palavras-chave do Razor reservado</span><span class="sxs-lookup"><span data-stu-id="6512e-249">Razor reserved keywords</span></span>

### <a name="razor-keywords"></a><span data-ttu-id="6512e-250">Palavras-chave do Razor</span><span class="sxs-lookup"><span data-stu-id="6512e-250">Razor keywords</span></span>

* <span data-ttu-id="6512e-251">página (requer o ASP.NET Core 2.0 e posterior)</span><span class="sxs-lookup"><span data-stu-id="6512e-251">page (Requires ASP.NET Core 2.0 and later)</span></span>
* <span data-ttu-id="6512e-252">funções</span><span class="sxs-lookup"><span data-stu-id="6512e-252">functions</span></span>
* <span data-ttu-id="6512e-253">herda</span><span class="sxs-lookup"><span data-stu-id="6512e-253">inherits</span></span>
* <span data-ttu-id="6512e-254">modelo</span><span class="sxs-lookup"><span data-stu-id="6512e-254">model</span></span>
* <span data-ttu-id="6512e-255">section</span><span class="sxs-lookup"><span data-stu-id="6512e-255">section</span></span>
* <span data-ttu-id="6512e-256">auxiliar (atualmente não há suportada pelo ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="6512e-256">helper (Not currently supported by ASP.NET Core)</span></span>

<span data-ttu-id="6512e-257">Palavras-chave do Razor são ignoradas com `@(Razor Keyword)` (por exemplo, `@(functions)`).</span><span class="sxs-lookup"><span data-stu-id="6512e-257">Razor keywords are escaped with `@(Razor Keyword)` (for example, `@(functions)`).</span></span>

### <a name="c-razor-keywords"></a><span data-ttu-id="6512e-258">Palavras-chave do c# Razor</span><span class="sxs-lookup"><span data-stu-id="6512e-258">C# Razor keywords</span></span>

* <span data-ttu-id="6512e-259">case</span><span class="sxs-lookup"><span data-stu-id="6512e-259">case</span></span>
* <span data-ttu-id="6512e-260">do</span><span class="sxs-lookup"><span data-stu-id="6512e-260">do</span></span>
* <span data-ttu-id="6512e-261">default</span><span class="sxs-lookup"><span data-stu-id="6512e-261">default</span></span>
* <span data-ttu-id="6512e-262">for</span><span class="sxs-lookup"><span data-stu-id="6512e-262">for</span></span>
* <span data-ttu-id="6512e-263">foreach</span><span class="sxs-lookup"><span data-stu-id="6512e-263">foreach</span></span>
* <span data-ttu-id="6512e-264">if</span><span class="sxs-lookup"><span data-stu-id="6512e-264">if</span></span>
* <span data-ttu-id="6512e-265">else</span><span class="sxs-lookup"><span data-stu-id="6512e-265">else</span></span>
* <span data-ttu-id="6512e-266">bloqueio</span><span class="sxs-lookup"><span data-stu-id="6512e-266">lock</span></span>
* <span data-ttu-id="6512e-267">switch</span><span class="sxs-lookup"><span data-stu-id="6512e-267">switch</span></span>
* <span data-ttu-id="6512e-268">try</span><span class="sxs-lookup"><span data-stu-id="6512e-268">try</span></span>
* <span data-ttu-id="6512e-269">catch</span><span class="sxs-lookup"><span data-stu-id="6512e-269">catch</span></span>
* <span data-ttu-id="6512e-270">finally</span><span class="sxs-lookup"><span data-stu-id="6512e-270">finally</span></span>
* <span data-ttu-id="6512e-271">using</span><span class="sxs-lookup"><span data-stu-id="6512e-271">using</span></span>
* <span data-ttu-id="6512e-272">while</span><span class="sxs-lookup"><span data-stu-id="6512e-272">while</span></span>

<span data-ttu-id="6512e-273">Palavras-chave do c# Razor devem ser de escape duplo com `@(@C# Razor Keyword)` (por exemplo, `@(@case)`).</span><span class="sxs-lookup"><span data-stu-id="6512e-273">C# Razor keywords must be double-escaped with `@(@C# Razor Keyword)` (for example, `@(@case)`).</span></span> <span data-ttu-id="6512e-274">A primeira `@` ignora o analisador do Razor.</span><span class="sxs-lookup"><span data-stu-id="6512e-274">The first `@` escapes the Razor parser.</span></span> <span data-ttu-id="6512e-275">O segundo `@` ignora o analisador c#.</span><span class="sxs-lookup"><span data-stu-id="6512e-275">The second `@` escapes the C# parser.</span></span>

### <a name="reserved-keywords-not-used-by-razor"></a><span data-ttu-id="6512e-276">Palavras-chave reservadas não usadas pelo Razor</span><span class="sxs-lookup"><span data-stu-id="6512e-276">Reserved keywords not used by Razor</span></span>

* <span data-ttu-id="6512e-277">namespace</span><span class="sxs-lookup"><span data-stu-id="6512e-277">namespace</span></span>
* <span data-ttu-id="6512e-278">classe</span><span class="sxs-lookup"><span data-stu-id="6512e-278">class</span></span>

## <a name="viewing-the-razor-c-class-generated-for-a-view"></a><span data-ttu-id="6512e-279">Exibindo a classe Razor c# gerada para um modo de exibição</span><span class="sxs-lookup"><span data-stu-id="6512e-279">Viewing the Razor C# class generated for a view</span></span>

<span data-ttu-id="6512e-280">Adicione a seguinte classe ao projeto MVC do ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="6512e-280">Add the following class to the ASP.NET Core MVC project:</span></span>

[!code-csharp[Main](razor/sample/Utilities/CustomTemplateEngine.cs)]

<span data-ttu-id="6512e-281">Substituir o `RazorTemplateEngine` adicionado pelo MVC com o `CustomTemplateEngine` classe:</span><span class="sxs-lookup"><span data-stu-id="6512e-281">Override the `RazorTemplateEngine` added by MVC with the `CustomTemplateEngine` class:</span></span>

[!code-csharp[Main](razor/sample/Startup.cs?highlight=4&range=10-14)]

<span data-ttu-id="6512e-282">Definir um ponto de interrupção no `return csharpDocument` instrução `CustomTemplateEngine`.</span><span class="sxs-lookup"><span data-stu-id="6512e-282">Set a break point on the `return csharpDocument` statement of `CustomTemplateEngine`.</span></span> <span data-ttu-id="6512e-283">Quando a execução do programa é interrompida no ponto de interrupção, exibir o valor de `generatedCode`.</span><span class="sxs-lookup"><span data-stu-id="6512e-283">When program execution stops at the break point, view the value of `generatedCode`.</span></span>

![Exibição do Visualizador de texto de generatedCode](razor/_static/tvr.png)

## <a name="view-lookups-and-case-sensitivity"></a><span data-ttu-id="6512e-285">Pesquisas de exibição e diferenciação de maiusculas e minúsculas</span><span class="sxs-lookup"><span data-stu-id="6512e-285">View lookups and case sensitivity</span></span>

<span data-ttu-id="6512e-286">O mecanismo de exibição Razor executa pesquisas diferencia maiusculas de minúsculas para modos de exibição.</span><span class="sxs-lookup"><span data-stu-id="6512e-286">The Razor view engine performs case-sensitive lookups for views.</span></span> <span data-ttu-id="6512e-287">No entanto, a pesquisa real é determinada pelo sistema de arquivos subjacente:</span><span class="sxs-lookup"><span data-stu-id="6512e-287">However, the actual lookup is determined by the underlying file system:</span></span>

* <span data-ttu-id="6512e-288">Arquivo de origem com base:</span><span class="sxs-lookup"><span data-stu-id="6512e-288">File based source:</span></span> 
  * <span data-ttu-id="6512e-289">Em sistemas operacionais com sistemas de arquivos de maiusculas e minúsculas (por exemplo, Windows), pesquisas de provedor de arquivo físico diferenciam maiusculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="6512e-289">On operating systems with case insensitive file systems (for example, Windows), physical file provider lookups are case insensitive.</span></span> <span data-ttu-id="6512e-290">Por exemplo, `return View("Test")` resulta em correspondências para */Views/Home/Test.cshtml*, */Views/home/test.cshtml*e qualquer outra variante de maiusculas e minúsculas.</span><span class="sxs-lookup"><span data-stu-id="6512e-290">For example, `return View("Test")` results in matches for */Views/Home/Test.cshtml*, */Views/home/test.cshtml*, and any other casing variant.</span></span>
  * <span data-ttu-id="6512e-291">Em sistemas de arquivos diferencia maiusculas de minúsculas (por exemplo, Linux, OSX e com `EmbeddedFileProvider`), pesquisas diferenciam maiusculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="6512e-291">On case-sensitive file systems (for example, Linux, OSX, and with `EmbeddedFileProvider`), lookups are case-sensitive.</span></span> <span data-ttu-id="6512e-292">Por exemplo, `return View("Test")` especificamente correspondências */Views/Home/Test.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="6512e-292">For example, `return View("Test")` specifically matches */Views/Home/Test.cshtml*.</span></span>
* <span data-ttu-id="6512e-293">Pré-compilados exibições: com o núcleo do ASP.NET 2.0 e posterior, procurando os modos de exibição pré-compilado diferencia maiusculas de minúsculas em todos os sistemas operacionais.</span><span class="sxs-lookup"><span data-stu-id="6512e-293">Precompiled views: With ASP.NET Core 2.0 and later, looking up precompiled views is case insensitive on all operating systems.</span></span> <span data-ttu-id="6512e-294">O comportamento é idêntico ao comportamento do provedor de arquivo físico no Windows.</span><span class="sxs-lookup"><span data-stu-id="6512e-294">The behavior is identical to physical file provider's behavior on Windows.</span></span> <span data-ttu-id="6512e-295">Se os dois modos de exibição pré-compilado diferem apenas em maiusculas, o resultado da pesquisa é não determinística.</span><span class="sxs-lookup"><span data-stu-id="6512e-295">If two precompiled views differ only in case, the result of lookup is non-deterministic.</span></span>

<span data-ttu-id="6512e-296">Os desenvolvedores são encorajados para coincidir com o uso de maiusculas e minúsculas dos nomes de arquivo e diretório para o uso de maiusculas e minúsculas de:</span><span class="sxs-lookup"><span data-stu-id="6512e-296">Developers are encouraged to match the casing of file and directory names to the casing of:</span></span>

    * <span data-ttu-id="6512e-297">Nomes de área, controlador e ação.</span><span class="sxs-lookup"><span data-stu-id="6512e-297">Area, controller, and action names.</span></span> 
    * <span data-ttu-id="6512e-298">Páginas do Razor.</span><span class="sxs-lookup"><span data-stu-id="6512e-298">Razor Pages.</span></span>
    
<span data-ttu-id="6512e-299">Correspondência caso garante que as implantações de localizar as exibições, independentemente do sistema de arquivos subjacente.</span><span class="sxs-lookup"><span data-stu-id="6512e-299">Matching case ensures the deployments find their views regardless of the underlying file system.</span></span>

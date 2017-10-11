---
title: "Referência de sintaxe Razor ASP.NET Core"
author: guardrex
description: "Saiba mais sobre a sintaxe de marcação Razor para inserir o código de servidor em páginas da Web."
keywords: Diretivas do ASP.NET Core, Razor, Razor
ms.author: riande
manager: wpickett
ms.date: 09/29/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/razor
ms.openlocfilehash: 532e278597a0029b5bae93068af5b7b147c35688
ms.sourcegitcommit: e45f8912ce32b4071bf7e83b8f8315cd8bba3520
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/04/2017
---
# <a name="razor-syntax-for-aspnet-core"></a><span data-ttu-id="c0e21-104">Sintaxe do Razor para o ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c0e21-104">Razor syntax for ASP.NET Core</span></span>

<span data-ttu-id="c0e21-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT), [Luke Latham](https://github.com/guardrex), e [Taylor Mullen](https://twitter.com/ntaylormullen)</span><span class="sxs-lookup"><span data-stu-id="c0e21-105">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Luke Latham](https://github.com/guardrex), and [Taylor Mullen](https://twitter.com/ntaylormullen)</span></span>

<span data-ttu-id="c0e21-106">Razor é uma sintaxe de marcação para inserir o código de servidor em páginas da Web.</span><span class="sxs-lookup"><span data-stu-id="c0e21-106">Razor is a markup syntax for embedding server-based code into webpages.</span></span> <span data-ttu-id="c0e21-107">A sintaxe do Razor consiste Razor marcação, c# e HTML.</span><span class="sxs-lookup"><span data-stu-id="c0e21-107">The Razor syntax consists of Razor markup, C#, and HTML.</span></span> <span data-ttu-id="c0e21-108">Arquivos que contêm Razor geralmente têm um *. cshtml* extensão de arquivo.</span><span class="sxs-lookup"><span data-stu-id="c0e21-108">Files containing Razor generally have a *.cshtml* file extension.</span></span>

## <a name="rendering-html"></a><span data-ttu-id="c0e21-109">Renderização HTML</span><span class="sxs-lookup"><span data-stu-id="c0e21-109">Rendering HTML</span></span>

<span data-ttu-id="c0e21-110">O idioma do Razor padrão é HTML.</span><span class="sxs-lookup"><span data-stu-id="c0e21-110">The default Razor language is HTML.</span></span> <span data-ttu-id="c0e21-111">Renderização HTML da marcação Razor não é diferente de renderização HTML de um arquivo HTML.</span><span class="sxs-lookup"><span data-stu-id="c0e21-111">Rendering HTML from Razor markup is no different than rendering HTML from an HTML file.</span></span> <span data-ttu-id="c0e21-112">Se você colocar uma marcação HTML em uma *. cshtml* arquivo Razor, ela é processada pelo servidor inalterado.</span><span class="sxs-lookup"><span data-stu-id="c0e21-112">If you place HTML markup into a *.cshtml* Razor file, it's rendered by the server unchanged.</span></span>

## <a name="razor-syntax"></a><span data-ttu-id="c0e21-113">Sintaxe do Razor</span><span class="sxs-lookup"><span data-stu-id="c0e21-113">Razor syntax</span></span>

<span data-ttu-id="c0e21-114">Razor suporta c# e usa o `@` símbolo para fazer a transição de HTML para c#.</span><span class="sxs-lookup"><span data-stu-id="c0e21-114">Razor supports C# and uses the `@` symbol to transition from HTML to C#.</span></span> <span data-ttu-id="c0e21-115">Razor avalia expressões c# e renderiza-los na saída HTML.</span><span class="sxs-lookup"><span data-stu-id="c0e21-115">Razor evaluates C# expressions and renders them in the HTML output.</span></span>

<span data-ttu-id="c0e21-116">Quando um `@` símbolo é seguido por um [Razor reservados a palavra-chave](#razor-reserved-keywords), ele faz a transição para marcação específica do Razor.</span><span class="sxs-lookup"><span data-stu-id="c0e21-116">When an `@` symbol is followed by a [Razor reserved keyword](#razor-reserved-keywords), it transitions into Razor-specific markup.</span></span> <span data-ttu-id="c0e21-117">Caso contrário, ela faz a transição em c# simples.</span><span class="sxs-lookup"><span data-stu-id="c0e21-117">Otherwise, it transitions into plain C#.</span></span>

<span data-ttu-id="c0e21-118">Para escape um `@` símbolo na marcação Razor, usar um segundo `@` símbolo:</span><span class="sxs-lookup"><span data-stu-id="c0e21-118">To escape an `@` symbol in Razor markup, use a second `@` symbol:</span></span>

```cshtml
<p>@@Username</p>
```

<span data-ttu-id="c0e21-119">O código é renderizado em HTML com um único `@` símbolo:</span><span class="sxs-lookup"><span data-stu-id="c0e21-119">The code is rendered in HTML with a single `@` symbol:</span></span>

```html
<p>@Username</p>
```

<span data-ttu-id="c0e21-120">Atributos HTML e o conteúdo que contém endereços de email não tratam o `@` símbolo como um caractere de transição.</span><span class="sxs-lookup"><span data-stu-id="c0e21-120">HTML attributes and content containing email addresses don't treat the `@` symbol as a transition character.</span></span> <span data-ttu-id="c0e21-121">Os endereços de email no exemplo a seguir são inalterados por Razor análise:</span><span class="sxs-lookup"><span data-stu-id="c0e21-121">The email addresses in the following example are untouched by Razor parsing:</span></span>

```cshtml
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
```

## <a name="implicit-razor-expressions"></a><span data-ttu-id="c0e21-122">Expressões implícitas do Razor</span><span class="sxs-lookup"><span data-stu-id="c0e21-122">Implicit Razor expressions</span></span>

<span data-ttu-id="c0e21-123">As expressões implícitas Razor iniciar com `@` seguido do código do c#:</span><span class="sxs-lookup"><span data-stu-id="c0e21-123">Implicit Razor expressions start with `@` followed by C# code:</span></span>

```cshtml
<p>@DateTime.Now</p>
<p>@DateTime.IsLeapYear(2016)</p>
```

<span data-ttu-id="c0e21-124">Com exceção do c# `await` palavra-chave, expressões implícitas não devem conter espaços.</span><span class="sxs-lookup"><span data-stu-id="c0e21-124">With the exception of the C# `await` keyword, implicit expressions must not contain spaces.</span></span> <span data-ttu-id="c0e21-125">Você pode intermingle espaços se a instrução c# tem uma terminação clara:</span><span class="sxs-lookup"><span data-stu-id="c0e21-125">You can intermingle spaces if the C# statement has a clear ending:</span></span>

```cshtml
<p>@await DoSomething("hello", "world")</p>
```

## <a name="explicit-razor-expressions"></a><span data-ttu-id="c0e21-126">Expressões explícitas Razor</span><span class="sxs-lookup"><span data-stu-id="c0e21-126">Explicit Razor expressions</span></span>

<span data-ttu-id="c0e21-127">As expressões explícitas Razor consistem em um `@` símbolo com parênteses equilibrada.</span><span class="sxs-lookup"><span data-stu-id="c0e21-127">Explicit Razor expressions consist of an `@` symbol with balanced parenthesis.</span></span> <span data-ttu-id="c0e21-128">Para renderizar a hora da última semana, a seguinte marcação Razor é usada:</span><span class="sxs-lookup"><span data-stu-id="c0e21-128">To render last week's time, the following Razor markup is used:</span></span>

```cshtml
<p>Last week this time: @(DateTime.Now - TimeSpan.FromDays(7))</p>
```

<span data-ttu-id="c0e21-129">Qualquer conteúdo dentro do `@()` parênteses é avaliado e renderizado para a saída.</span><span class="sxs-lookup"><span data-stu-id="c0e21-129">Any content within the `@()` parenthesis is evaluated and rendered to the output.</span></span>

<span data-ttu-id="c0e21-130">Expressões implícitas, descritas na seção anterior, geralmente não podem conter espaços.</span><span class="sxs-lookup"><span data-stu-id="c0e21-130">Implicit expressions, described in the previous section, generally can't contain spaces.</span></span> <span data-ttu-id="c0e21-131">No código a seguir, uma semana não é subtraída da hora atual:</span><span class="sxs-lookup"><span data-stu-id="c0e21-131">In the following code, one week isn't subtracted from the current time:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact.cshtml?range=17)]

<span data-ttu-id="c0e21-132">O código processa o HTML a seguir:</span><span class="sxs-lookup"><span data-stu-id="c0e21-132">The code renders the following HTML:</span></span>

```html
<p>Last week: 7/7/2016 4:39:52 PM - TimeSpan.FromDays(7)</p>
```

<span data-ttu-id="c0e21-133">Você pode usar uma expressão explícita para concatenação de texto com um resultado da expressão:</span><span class="sxs-lookup"><span data-stu-id="c0e21-133">You can use an explicit expression to concatenate text with an expression result:</span></span>

```cshtml
@{
    var joe = new Person("Joe", 33);
}

<p>Age@(joe.Age)</p>
```

<span data-ttu-id="c0e21-134">Sem expressão explícita, `<p>Age@joe.Age</p>` é tratado como um endereço de email, e `<p>Age@joe.Age</p>` é renderizado.</span><span class="sxs-lookup"><span data-stu-id="c0e21-134">Without the explicit expression, `<p>Age@joe.Age</p>` is treated as an email address, and `<p>Age@joe.Age</p>` is rendered.</span></span> <span data-ttu-id="c0e21-135">Quando gravado como uma expressão explícita, `<p>Age33</p>` é renderizado.</span><span class="sxs-lookup"><span data-stu-id="c0e21-135">When written as an explicit expression, `<p>Age33</p>` is rendered.</span></span>

## <a name="expression-encoding"></a><span data-ttu-id="c0e21-136">Codificação de expressão</span><span class="sxs-lookup"><span data-stu-id="c0e21-136">Expression encoding</span></span>

<span data-ttu-id="c0e21-137">Expressões em c# que são avaliadas como uma cadeia de caracteres estão codificada em HTML.</span><span class="sxs-lookup"><span data-stu-id="c0e21-137">C# expressions that evaluate to a string are HTML encoded.</span></span> <span data-ttu-id="c0e21-138">Expressões em c# que avaliam `IHtmlContent` são renderizados diretamente por meio `IHtmlContent.WriteTo`.</span><span class="sxs-lookup"><span data-stu-id="c0e21-138">C# expressions that evaluate to `IHtmlContent` are rendered directly through `IHtmlContent.WriteTo`.</span></span> <span data-ttu-id="c0e21-139">Expressões em c# que não retornam `IHtmlContent` são convertidos em uma cadeia de caracteres por `ToString` e codificado antes que eles são renderizados.</span><span class="sxs-lookup"><span data-stu-id="c0e21-139">C# expressions that don't evaluate to `IHtmlContent` are converted to a string by `ToString` and encoded before they're rendered.</span></span>

```cshtml
@("<span>Hello World</span>")
```

<span data-ttu-id="c0e21-140">O código processa o HTML a seguir:</span><span class="sxs-lookup"><span data-stu-id="c0e21-140">The code renders the following HTML:</span></span>

```html
&lt;span&gt;Hello World&lt;/span&gt;
```

<span data-ttu-id="c0e21-141">O HTML é mostrado no navegador como:</span><span class="sxs-lookup"><span data-stu-id="c0e21-141">The HTML is shown in the browser as:</span></span>

```
<span>Hello World</span>
```

<span data-ttu-id="c0e21-142">`HtmlHelper.Raw`saída não é codificada mas renderizada como marcação HTML.</span><span class="sxs-lookup"><span data-stu-id="c0e21-142">`HtmlHelper.Raw` output isn't encoded but rendered as HTML markup.</span></span>

> [!WARNING]
> <span data-ttu-id="c0e21-143">Usando `HtmlHelper.Raw` usuário unsanitized a entrada é um risco de segurança.</span><span class="sxs-lookup"><span data-stu-id="c0e21-143">Using `HtmlHelper.Raw` on unsanitized user input is a security risk.</span></span> <span data-ttu-id="c0e21-144">Entrada do usuário pode conter outras explorações ou JavaScript mal-intencionados.</span><span class="sxs-lookup"><span data-stu-id="c0e21-144">User input might contain malicious JavaScript or other exploits.</span></span> <span data-ttu-id="c0e21-145">Limpeza de entrada do usuário é difícil.</span><span class="sxs-lookup"><span data-stu-id="c0e21-145">Sanitizing user input is difficult.</span></span> <span data-ttu-id="c0e21-146">Evite usar `HtmlHelper.Raw` com entrada do usuário.</span><span class="sxs-lookup"><span data-stu-id="c0e21-146">Avoid using `HtmlHelper.Raw` with user input.</span></span>

```cshtml
@Html.Raw("<span>Hello World</span>")
```

<span data-ttu-id="c0e21-147">O código processa o HTML a seguir:</span><span class="sxs-lookup"><span data-stu-id="c0e21-147">The code renders the following HTML:</span></span>

```html
<span>Hello World</span>
```

## <a name="razor-code-blocks"></a><span data-ttu-id="c0e21-148">Blocos de código Razor</span><span class="sxs-lookup"><span data-stu-id="c0e21-148">Razor code blocks</span></span>

<span data-ttu-id="c0e21-149">Blocos de código Razor iniciar com `@` e são delimitados por `{}`.</span><span class="sxs-lookup"><span data-stu-id="c0e21-149">Razor code blocks start with `@` and are enclosed by `{}`.</span></span> <span data-ttu-id="c0e21-150">Ao contrário das expressões, o código c# dentro de blocos de código não está processado.</span><span class="sxs-lookup"><span data-stu-id="c0e21-150">Unlike expressions, C# code inside code blocks isn't rendered.</span></span> <span data-ttu-id="c0e21-151">Blocos de código e expressões em uma exibição compartilhem o mesmo escopo e são definidas em ordem:</span><span class="sxs-lookup"><span data-stu-id="c0e21-151">Code blocks and expressions in a view share the same scope and are defined in order:</span></span>

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

<span data-ttu-id="c0e21-152">O código processa o HTML a seguir:</span><span class="sxs-lookup"><span data-stu-id="c0e21-152">The code renders the following HTML:</span></span>

```html
<p>The future depends on what you do today. - Mahatma Gandhi</p>
<p>Hate cannot drive out hate, only love can do that. - Martin Luther King, Jr.</p>
```

### <a name="implicit-transitions"></a><span data-ttu-id="c0e21-153">Transições implícita</span><span class="sxs-lookup"><span data-stu-id="c0e21-153">Implicit transitions</span></span>

<span data-ttu-id="c0e21-154">É o idioma padrão em um bloco de código c#, mas você pode fazer a transição para HTML:</span><span class="sxs-lookup"><span data-stu-id="c0e21-154">The default language in a code block is C#, but you can transition back to HTML:</span></span>

```cshtml
@{
    var inCSharp = true;
    <p>Now in HTML, was in C# @inCSharp</p>
}
```

### <a name="explicit-delimited-transition"></a><span data-ttu-id="c0e21-155">Transição delimitada explícita</span><span class="sxs-lookup"><span data-stu-id="c0e21-155">Explicit delimited transition</span></span>

<span data-ttu-id="c0e21-156">Para definir uma subseção de um bloco de código que deve renderizar HTML, coloque os caracteres para renderização o Razor  **\<texto >** marca:</span><span class="sxs-lookup"><span data-stu-id="c0e21-156">To define a sub-section of a code block that should render HTML, surround the characters for rendering with the Razor **\<text>** tag:</span></span>

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <text>Name: @person.Name</text>
}
```

<span data-ttu-id="c0e21-157">Use essa abordagem quando você deseja renderizar HTML que não está circundado por uma marca HTML.</span><span class="sxs-lookup"><span data-stu-id="c0e21-157">Use this approach when you want to render HTML that isn't surrounded by an HTML tag.</span></span> <span data-ttu-id="c0e21-158">Sem uma marca HTML ou Razor, você receberá um erro de tempo de execução do Razor.</span><span class="sxs-lookup"><span data-stu-id="c0e21-158">Without an HTML or Razor tag, you receive a Razor runtime error.</span></span>

<span data-ttu-id="c0e21-159">O  **\<texto >** marca também é útil para controlar o espaço em branco ao renderizar o conteúdo.</span><span class="sxs-lookup"><span data-stu-id="c0e21-159">The **\<text>** tag is also useful to control whitespace when rendering content.</span></span> <span data-ttu-id="c0e21-160">Somente o conteúdo entre o  **\<texto >** marcas é processado e nenhum espaço em branco antes ou depois do  **\<texto >** marcas aparece na saída HTML.</span><span class="sxs-lookup"><span data-stu-id="c0e21-160">Only the content between the **\<text>** tags is rendered, and no whitespace before or after the **\<text>** tags appears in the HTML output.</span></span>

### <a name="explicit-line-transition-with-"></a><span data-ttu-id="c0e21-161">Transição de linha explícita com @:</span><span class="sxs-lookup"><span data-stu-id="c0e21-161">Explicit Line Transition with @:</span></span>

<span data-ttu-id="c0e21-162">Para processar o restante de uma linha inteira como HTML dentro de um bloco de código, use o `@:` sintaxe:</span><span class="sxs-lookup"><span data-stu-id="c0e21-162">To render the rest of an entire line as HTML inside a code block, use the `@:` syntax:</span></span>

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    @:Name: @person.Name
}
```

<span data-ttu-id="c0e21-163">Sem o `@:` no código, você receberá um erro de tempo de execução do Razor.</span><span class="sxs-lookup"><span data-stu-id="c0e21-163">Without the `@:` in the code, you recieve a Razor runtime error.</span></span>

## <a name="control-structures"></a><span data-ttu-id="c0e21-164">Estruturas de controle</span><span class="sxs-lookup"><span data-stu-id="c0e21-164">Control Structures</span></span>

<span data-ttu-id="c0e21-165">Estruturas de controle são uma extensão dos blocos de código.</span><span class="sxs-lookup"><span data-stu-id="c0e21-165">Control structures are an extension of code blocks.</span></span> <span data-ttu-id="c0e21-166">Todos os aspectos dos blocos de código (em transição para marcação, embutido c#) também se aplicam aos seguintes estruturas.</span><span class="sxs-lookup"><span data-stu-id="c0e21-166">All aspects of code blocks (transitioning to markup, inline C#) also apply to the following structures.</span></span>

### <a name="conditionals-if-else-if-else-and-switch"></a><span data-ttu-id="c0e21-167">Condicionais @if, senão se, para outro, e@switch</span><span class="sxs-lookup"><span data-stu-id="c0e21-167">Conditionals @if, else if, else, and @switch</span></span>

<span data-ttu-id="c0e21-168">`@if`Controla quando o código é executado:</span><span class="sxs-lookup"><span data-stu-id="c0e21-168">`@if` controls when code runs:</span></span>

```cshtml
@if (value % 2 == 0)
{
    <p>The value was even.</p>
}
```

<span data-ttu-id="c0e21-169">`else`e `else if` não exigem o `@` símbolo:</span><span class="sxs-lookup"><span data-stu-id="c0e21-169">`else` and `else if` don't require the `@` symbol:</span></span>

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

<span data-ttu-id="c0e21-170">Você pode usar uma instrução switch como esta:</span><span class="sxs-lookup"><span data-stu-id="c0e21-170">You can use a switch statement like this:</span></span>

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

### <a name="looping-for-foreach-while-and-do-while"></a><span data-ttu-id="c0e21-171">Loop @for, @foreach, @while, e @do enquanto</span><span class="sxs-lookup"><span data-stu-id="c0e21-171">Looping @for, @foreach, @while, and @do while</span></span>

<span data-ttu-id="c0e21-172">Você pode renderizar HTML modelado com instruções de controle de loop.</span><span class="sxs-lookup"><span data-stu-id="c0e21-172">You can render templated HTML with looping control statements.</span></span> <span data-ttu-id="c0e21-173">Para renderizar uma lista de pessoas:</span><span class="sxs-lookup"><span data-stu-id="c0e21-173">To render a list of people:</span></span>

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

<span data-ttu-id="c0e21-174">Você pode usar qualquer uma das seguintes instruções em loop:</span><span class="sxs-lookup"><span data-stu-id="c0e21-174">You can use any of the following looping statements:</span></span>

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

### <a name="compound-using"></a><span data-ttu-id="c0e21-175">Composta@using</span><span class="sxs-lookup"><span data-stu-id="c0e21-175">Compound @using</span></span>

<span data-ttu-id="c0e21-176">No c#, um `using` instrução é usada para garantir que um objeto é descartado.</span><span class="sxs-lookup"><span data-stu-id="c0e21-176">In C#, a `using` statement is used to ensure an object is disposed.</span></span> <span data-ttu-id="c0e21-177">No Razor, o mesmo mecanismo é usado para criar os auxiliares HTML que contém o conteúdo adicional.</span><span class="sxs-lookup"><span data-stu-id="c0e21-177">In Razor, the same mechanism is used to create HTML Helpers that contain additional content.</span></span> <span data-ttu-id="c0e21-178">Por exemplo, você pode utilizar os auxiliares HTML para renderizar uma marca de formulário com o `@using` instrução:</span><span class="sxs-lookup"><span data-stu-id="c0e21-178">For instance, you can utilize HTML Helpers to render a form tag with the `@using` statement:</span></span>

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

<span data-ttu-id="c0e21-179">Você também pode executar ações no nível de escopo com [auxiliares de marcação](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="c0e21-179">You can also perform scope-level actions with [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

### <a name="try-catch-finally"></a><span data-ttu-id="c0e21-180">@try, catch, finally</span><span class="sxs-lookup"><span data-stu-id="c0e21-180">@try, catch, finally</span></span>

<span data-ttu-id="c0e21-181">Tratamento de exceção é semelhante ao c#:</span><span class="sxs-lookup"><span data-stu-id="c0e21-181">Exception handling is similar to C#:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact7.cshtml)]

### <a name="lock"></a>@lock

<span data-ttu-id="c0e21-182">Razor tem a capacidade de proteger seções críticas com instruções de bloqueio:</span><span class="sxs-lookup"><span data-stu-id="c0e21-182">Razor has the capability to protect critical sections with lock statements:</span></span>

```cshtml
@lock (SomeLock)
{
    // Do critical section work
}
```

### <a name="comments"></a><span data-ttu-id="c0e21-183">Comentários</span><span class="sxs-lookup"><span data-stu-id="c0e21-183">Comments</span></span>

<span data-ttu-id="c0e21-184">Razor dá suporte a comentários c# e HTML:</span><span class="sxs-lookup"><span data-stu-id="c0e21-184">Razor supports C# and HTML comments:</span></span>

```cshtml
@{
    /* C# comment */
    // Another C# comment
}
<!-- HTML comment -->
```

<span data-ttu-id="c0e21-185">O código processa o HTML a seguir:</span><span class="sxs-lookup"><span data-stu-id="c0e21-185">The code renders the following HTML:</span></span>

```html
<!-- HTML comment -->
```

<span data-ttu-id="c0e21-186">Comentários de Razor serão removidos pelo servidor para a página da Web seja processada.</span><span class="sxs-lookup"><span data-stu-id="c0e21-186">Razor comments are removed by the server before the webpage is rendered.</span></span> <span data-ttu-id="c0e21-187">Razor usa `@*  *@` para delimitar os comentários.</span><span class="sxs-lookup"><span data-stu-id="c0e21-187">Razor uses `@*  *@` to delimit comments.</span></span> <span data-ttu-id="c0e21-188">O código a seguir é comentado, para que o servidor não processa qualquer marcação:</span><span class="sxs-lookup"><span data-stu-id="c0e21-188">The following code is commented out, so the server doesn't render any markup:</span></span>

```cshtml
@*
    @{
        /* C# comment */
        // Another C# comment
    }
    <!-- HTML comment -->
*@
```

## <a name="directives"></a><span data-ttu-id="c0e21-189">Diretivas</span><span class="sxs-lookup"><span data-stu-id="c0e21-189">Directives</span></span>

<span data-ttu-id="c0e21-190">Diretivas de Razor são representadas por expressões implícitas com os seguintes palavras-chave reservadas do `@` símbolo.</span><span class="sxs-lookup"><span data-stu-id="c0e21-190">Razor directives are represented by implicit expressions with reserved keywords following the `@` symbol.</span></span> <span data-ttu-id="c0e21-191">Uma diretiva geralmente altera o modo como uma exibição é analisada ou habilita a funcionalidade diferente.</span><span class="sxs-lookup"><span data-stu-id="c0e21-191">A directive typically changes the way a view is parsed or enables different functionality.</span></span>

<span data-ttu-id="c0e21-192">Noções básicas sobre como Razor gera código para um modo de exibição torna mais fácil de entender como funcionam as diretivas.</span><span class="sxs-lookup"><span data-stu-id="c0e21-192">Understanding how Razor generates code for a view makes it easier to understand how directives work.</span></span>

[!code-html[Main](razor/sample/Views/Home/Contact8.cshtml)]

<span data-ttu-id="c0e21-193">O código gera uma classe semelhante à seguinte:</span><span class="sxs-lookup"><span data-stu-id="c0e21-193">The code generates a class similar to the following:</span></span>

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

<span data-ttu-id="c0e21-194">Posteriormente neste artigo, a seção [exibindo a classe Razor c# gerada para uma exibição](#viewing-the-razor-c-class-generated-for-a-view) explica como exibir essa classe gerada.</span><span class="sxs-lookup"><span data-stu-id="c0e21-194">Later in this article, the section [Viewing the Razor C# class generated for a view](#viewing-the-razor-c-class-generated-for-a-view) explains how to view this generated class.</span></span>

### <a name="using"></a>@using

<span data-ttu-id="c0e21-195">O `@using` diretiva adiciona c# `using` diretiva para o modo de exibição gerado:</span><span class="sxs-lookup"><span data-stu-id="c0e21-195">The `@using` directive adds the C# `using` directive to the generated view:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact9.cshtml)]

### <a name="model"></a>@model

<span data-ttu-id="c0e21-196">O `@model` diretiva especifica o tipo de modelo passado para um modo de exibição:</span><span class="sxs-lookup"><span data-stu-id="c0e21-196">The `@model` directive specifies the type of the model passed to a view:</span></span>

```cshtml
@model TypeNameOfModel
```

<span data-ttu-id="c0e21-197">Se você criar um aplicativo ASP.NET MVC de núcleo com contas de usuário individuais, o *Views/Account/Login.cshtml* exibição contém a declaração de modelo a seguir:</span><span class="sxs-lookup"><span data-stu-id="c0e21-197">If you create an ASP.NET Core MVC app with individual user accounts, the *Views/Account/Login.cshtml* view contains the following model declaration:</span></span>

```cshtml
@model LoginViewModel
```

<span data-ttu-id="c0e21-198">A classe gerada herda de `RazorPage<dynamic>`:</span><span class="sxs-lookup"><span data-stu-id="c0e21-198">The class generated inherits from `RazorPage<dynamic>`:</span></span>

```csharp
public class _Views_Account_Login_cshtml : RazorPage<LoginViewModel>
```

<span data-ttu-id="c0e21-199">Razor expõe um `Model` propriedade para acessar o modelo passado para o modo de exibição:</span><span class="sxs-lookup"><span data-stu-id="c0e21-199">Razor exposes a `Model` property for accessing the model passed to the view:</span></span>

```cshtml
<div>The Login Email: @Model.Email</div>
```

<span data-ttu-id="c0e21-200">O `@model` diretiva especifica o tipo desta propriedade.</span><span class="sxs-lookup"><span data-stu-id="c0e21-200">The `@model` directive specifies the type of this property.</span></span> <span data-ttu-id="c0e21-201">Especifica a diretiva de `T` em `RazorPage<T>` o modo de exibição que gerado classe que deriva.</span><span class="sxs-lookup"><span data-stu-id="c0e21-201">The directive specifies the `T` in `RazorPage<T>` that the generated class that your view derives from.</span></span> <span data-ttu-id="c0e21-202">Se você não especificar o `@model` diretiva, o `Model` é de propriedade do tipo `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="c0e21-202">If you don't specify the `@model` directive, the `Model` property is of type `dynamic`.</span></span> <span data-ttu-id="c0e21-203">O valor do modelo é passado do controlador para o modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="c0e21-203">The value of the model is passed from the controller to the view.</span></span> <span data-ttu-id="c0e21-204">Consulte [fortemente tipado modelos e o @model palavra-chave](xref:tutorials/first-mvc-app/adding-model#strongly-typed-models-keyword-label) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="c0e21-204">See [Strongly typed models and the @model keyword](xref:tutorials/first-mvc-app/adding-model#strongly-typed-models-keyword-label) for more information.</span></span>

### <a name="inherits"></a>@inherits

<span data-ttu-id="c0e21-205">O `@inherits` diretiva lhe dá controle total da classe herda o modo de exibição:</span><span class="sxs-lookup"><span data-stu-id="c0e21-205">The `@inherits` directive gives you full control of the class your view inherits:</span></span>

```cshtml
@inherits TypeNameOfClassToInheritFrom
```

<span data-ttu-id="c0e21-206">Este é um tipo de página Razor personalizado:</span><span class="sxs-lookup"><span data-stu-id="c0e21-206">The following is a custom Razor page type:</span></span>

[!code-csharp[Main](razor/sample/Classes/CustomRazorPage.cs)]

<span data-ttu-id="c0e21-207">O `CustomText` é exibido em um modo de exibição:</span><span class="sxs-lookup"><span data-stu-id="c0e21-207">The `CustomText` is displayed in a view:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact10.cshtml)]

<span data-ttu-id="c0e21-208">O código processa o HTML a seguir:</span><span class="sxs-lookup"><span data-stu-id="c0e21-208">The code renders the following HTML:</span></span>

```html
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

<span data-ttu-id="c0e21-209">Não é possível usar `@model` e `@inherits` na mesma exibição.</span><span class="sxs-lookup"><span data-stu-id="c0e21-209">You can't use `@model` and `@inherits` in the same view.</span></span> <span data-ttu-id="c0e21-210">Você pode ter `@inherits` em uma *viewimports. cshtml* arquivo que importa o modo de exibição:</span><span class="sxs-lookup"><span data-stu-id="c0e21-210">You can have `@inherits` in a *_ViewImports.cshtml* file that the view imports:</span></span>

[!code-cshtml[Main](razor/sample/Views/_ViewImportsModel.cshtml)]

<span data-ttu-id="c0e21-211">Este é um exemplo de uma exibição fortemente tipado:</span><span class="sxs-lookup"><span data-stu-id="c0e21-211">The following is an example of a strongly-typed view:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Login1.cshtml)]

<span data-ttu-id="c0e21-212">Se "rick@contoso.com" é passado no modelo, o modo de exibição gera uma marcação HTML a seguir:</span><span class="sxs-lookup"><span data-stu-id="c0e21-212">If "rick@contoso.com" is passed in the model, the view generates the following HTML markup:</span></span>

```html
<div>The Login Email: rick@contoso.com</div>
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

### <a name="inject"></a>@inject

<span data-ttu-id="c0e21-213">O `@inject` diretiva permite que você inserir um serviço da sua [contêiner de serviço](xref:fundamentals/dependency-injection) em seu modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="c0e21-213">The `@inject` directive enables you to inject a service from your [service container](xref:fundamentals/dependency-injection) into your view.</span></span> <span data-ttu-id="c0e21-214">Consulte [injeção de dependência para exibições](xref:mvc/views/dependency-injection) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="c0e21-214">See [Dependency injection into views](xref:mvc/views/dependency-injection) for more information.</span></span>

### <a name="functions"></a>@functions

<span data-ttu-id="c0e21-215">O `@functions` diretiva permite que você adicione conteúdo no nível de função para um modo de exibição:</span><span class="sxs-lookup"><span data-stu-id="c0e21-215">The `@functions` directive enables you to add function-level content to a view:</span></span>

```cshtml
@functions { // C# Code }
```

<span data-ttu-id="c0e21-216">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="c0e21-216">For example:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact6.cshtml)]

<span data-ttu-id="c0e21-217">O código gera uma marcação HTML a seguir:</span><span class="sxs-lookup"><span data-stu-id="c0e21-217">The code generates the following HTML markup:</span></span>

```html
<div>From method: Hello</div>
```

<span data-ttu-id="c0e21-218">O código a seguir é a classe gerada Razor c#:</span><span class="sxs-lookup"><span data-stu-id="c0e21-218">The following code is the generated Razor C# class:</span></span>

[!code-csharp[Main](razor/sample/Classes/Views_Home_Test_cshtml.cs?range=1-19)]

### <a name="section"></a>@section

<span data-ttu-id="c0e21-219">O `@section` diretiva é usada em conjunto com o [layout](xref:mvc/views/layout) para habilitar os modos de exibição renderizar o conteúdo em diferentes partes da página HTML.</span><span class="sxs-lookup"><span data-stu-id="c0e21-219">The `@section` directive is used in conjunction with the [layout](xref:mvc/views/layout) to enable views to render content in different parts of the HTML page.</span></span> <span data-ttu-id="c0e21-220">Consulte [seções](xref:mvc/views/layout#layout-sections-label) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="c0e21-220">See [Sections](xref:mvc/views/layout#layout-sections-label) for more information.</span></span>

## <a name="tag-helpers"></a><span data-ttu-id="c0e21-221">Auxiliares de marcação</span><span class="sxs-lookup"><span data-stu-id="c0e21-221">Tag Helpers</span></span>

<span data-ttu-id="c0e21-222">Há três diretivas que pertencem ao [auxiliares de marcação](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="c0e21-222">There are three directives that pertain to [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

| <span data-ttu-id="c0e21-223">Diretiva</span><span class="sxs-lookup"><span data-stu-id="c0e21-223">Directive</span></span> | <span data-ttu-id="c0e21-224">Função</span><span class="sxs-lookup"><span data-stu-id="c0e21-224">Function</span></span> |
| --------- | -------- |
| [@addTagHelper](xref:mvc/views/tag-helpers/intro#add-helper-label) | <span data-ttu-id="c0e21-225">Disponibiliza auxiliares de marcação para um modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="c0e21-225">Makes Tag Helpers available to a view.</span></span> |
| [@removeTagHelper](xref:mvc/views/tag-helpers/intro#remove-razor-directives-label) | <span data-ttu-id="c0e21-226">Remove os auxiliares de marcação adicionado anteriormente de um modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="c0e21-226">Removes Tag Helpers previously added from a view.</span></span> |
| [@tagHelperPrefix](xref:mvc/views/tag-helpers/intro#prefix-razor-directives-label) | <span data-ttu-id="c0e21-227">Especifica um prefixo de marca para habilitar o suporte auxiliar de marca e fazer uso do auxiliar de marca explícita.</span><span class="sxs-lookup"><span data-stu-id="c0e21-227">Specifies a tag prefix to enable Tag Helper support and to make Tag Helper usage explicit.</span></span> |

## <a name="razor-reserved-keywords"></a><span data-ttu-id="c0e21-228">Palavras-chave do Razor reservado</span><span class="sxs-lookup"><span data-stu-id="c0e21-228">Razor reserved keywords</span></span>

### <a name="razor-keywords"></a><span data-ttu-id="c0e21-229">Palavras-chave do Razor</span><span class="sxs-lookup"><span data-stu-id="c0e21-229">Razor keywords</span></span>

* <span data-ttu-id="c0e21-230">página (requer o ASP.NET Core 2.0 e posterior)</span><span class="sxs-lookup"><span data-stu-id="c0e21-230">page (Requires ASP.NET Core 2.0 and later)</span></span>
* <span data-ttu-id="c0e21-231">funções</span><span class="sxs-lookup"><span data-stu-id="c0e21-231">functions</span></span>
* <span data-ttu-id="c0e21-232">herda</span><span class="sxs-lookup"><span data-stu-id="c0e21-232">inherits</span></span>
* <span data-ttu-id="c0e21-233">modelo</span><span class="sxs-lookup"><span data-stu-id="c0e21-233">model</span></span>
* <span data-ttu-id="c0e21-234">section</span><span class="sxs-lookup"><span data-stu-id="c0e21-234">section</span></span>
* <span data-ttu-id="c0e21-235">auxiliar (atualmente não há suportada pelo ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="c0e21-235">helper (Not currently supported by ASP.NET Core)</span></span>

<span data-ttu-id="c0e21-236">Palavras-chave do Razor são ignoradas com `@(Razor Keyword)` (por exemplo, `@(functions)`).</span><span class="sxs-lookup"><span data-stu-id="c0e21-236">Razor keywords are escaped with `@(Razor Keyword)` (for example, `@(functions)`).</span></span>

### <a name="c-razor-keywords"></a><span data-ttu-id="c0e21-237">Palavras-chave do c# Razor</span><span class="sxs-lookup"><span data-stu-id="c0e21-237">C# Razor keywords</span></span>

* <span data-ttu-id="c0e21-238">case</span><span class="sxs-lookup"><span data-stu-id="c0e21-238">case</span></span>
* <span data-ttu-id="c0e21-239">do</span><span class="sxs-lookup"><span data-stu-id="c0e21-239">do</span></span>
* <span data-ttu-id="c0e21-240">default</span><span class="sxs-lookup"><span data-stu-id="c0e21-240">default</span></span>
* <span data-ttu-id="c0e21-241">for</span><span class="sxs-lookup"><span data-stu-id="c0e21-241">for</span></span>
* <span data-ttu-id="c0e21-242">foreach</span><span class="sxs-lookup"><span data-stu-id="c0e21-242">foreach</span></span>
* <span data-ttu-id="c0e21-243">if</span><span class="sxs-lookup"><span data-stu-id="c0e21-243">if</span></span>
* <span data-ttu-id="c0e21-244">else</span><span class="sxs-lookup"><span data-stu-id="c0e21-244">else</span></span>
* <span data-ttu-id="c0e21-245">bloqueio</span><span class="sxs-lookup"><span data-stu-id="c0e21-245">lock</span></span>
* <span data-ttu-id="c0e21-246">switch</span><span class="sxs-lookup"><span data-stu-id="c0e21-246">switch</span></span>
* <span data-ttu-id="c0e21-247">try</span><span class="sxs-lookup"><span data-stu-id="c0e21-247">try</span></span>
* <span data-ttu-id="c0e21-248">catch</span><span class="sxs-lookup"><span data-stu-id="c0e21-248">catch</span></span>
* <span data-ttu-id="c0e21-249">finally</span><span class="sxs-lookup"><span data-stu-id="c0e21-249">finally</span></span>
* <span data-ttu-id="c0e21-250">using</span><span class="sxs-lookup"><span data-stu-id="c0e21-250">using</span></span>
* <span data-ttu-id="c0e21-251">while</span><span class="sxs-lookup"><span data-stu-id="c0e21-251">while</span></span>

<span data-ttu-id="c0e21-252">Palavras-chave do c# Razor devem ser de escape duplo com `@(@C# Razor Keyword)` (por exemplo, `@(@case)`).</span><span class="sxs-lookup"><span data-stu-id="c0e21-252">C# Razor keywords must be double-escaped with `@(@C# Razor Keyword)` (for example, `@(@case)`).</span></span> <span data-ttu-id="c0e21-253">A primeira `@` ignora o analisador do Razor.</span><span class="sxs-lookup"><span data-stu-id="c0e21-253">The first `@` escapes the Razor parser.</span></span> <span data-ttu-id="c0e21-254">O segundo `@` ignora o analisador c#.</span><span class="sxs-lookup"><span data-stu-id="c0e21-254">The second `@` escapes the C# parser.</span></span>

### <a name="reserved-keywords-not-used-by-razor"></a><span data-ttu-id="c0e21-255">Palavras-chave reservadas não usadas pelo Razor</span><span class="sxs-lookup"><span data-stu-id="c0e21-255">Reserved keywords not used by Razor</span></span>

* <span data-ttu-id="c0e21-256">namespace</span><span class="sxs-lookup"><span data-stu-id="c0e21-256">namespace</span></span>
* <span data-ttu-id="c0e21-257">classe</span><span class="sxs-lookup"><span data-stu-id="c0e21-257">class</span></span>

## <a name="viewing-the-razor-c-class-generated-for-a-view"></a><span data-ttu-id="c0e21-258">Exibindo a classe Razor c# gerada para um modo de exibição</span><span class="sxs-lookup"><span data-stu-id="c0e21-258">Viewing the Razor C# class generated for a view</span></span>

<span data-ttu-id="c0e21-259">Adicione a seguinte classe ao seu projeto MVC do ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="c0e21-259">Add the following class to your ASP.NET Core MVC project:</span></span>

[!code-csharp[Main](razor/sample/Utilities/CustomTemplateEngine.cs)]

<span data-ttu-id="c0e21-260">Substituir o `RazorTemplateEngine` adicionado pelo MVC com o `CustomTemplateEngine` classe:</span><span class="sxs-lookup"><span data-stu-id="c0e21-260">Override the `RazorTemplateEngine` added by MVC with the `CustomTemplateEngine` class:</span></span>

[!code-csharp[Main](razor/sample/Startup.cs?highlight=4&range=10-14)]

<span data-ttu-id="c0e21-261">Definir um ponto de interrupção no `return csharpDocument` instrução `CustomTemplateEngine`.</span><span class="sxs-lookup"><span data-stu-id="c0e21-261">Set a break point on the `return csharpDocument` statement of `CustomTemplateEngine`.</span></span> <span data-ttu-id="c0e21-262">Quando a execução do programa é interrompida no ponto de interrupção, exibir o valor de `generatedCode`.</span><span class="sxs-lookup"><span data-stu-id="c0e21-262">When program execution stops at the break point, view the value of `generatedCode`.</span></span>

![Exibição do Visualizador de texto de generatedCode](razor/_static/tvr.png)

## <a name="view-lookups-and-case-sensitivity"></a><span data-ttu-id="c0e21-264">Pesquisas de exibição e diferenciação de maiusculas e minúsculas</span><span class="sxs-lookup"><span data-stu-id="c0e21-264">View lookups and case sensitivity</span></span>

<span data-ttu-id="c0e21-265">O mecanismo de exibição Razor executa pesquisas diferencia maiusculas de minúsculas para modos de exibição.</span><span class="sxs-lookup"><span data-stu-id="c0e21-265">The Razor view engine performs case-sensitive lookups for views.</span></span> <span data-ttu-id="c0e21-266">No entanto, a pesquisa real é determinada pelo sistema de arquivos subjacente:</span><span class="sxs-lookup"><span data-stu-id="c0e21-266">However, the actual lookup is determined by the underlying file system:</span></span>

* <span data-ttu-id="c0e21-267">Arquivo de origem com base:</span><span class="sxs-lookup"><span data-stu-id="c0e21-267">File based source:</span></span> 
  * <span data-ttu-id="c0e21-268">Em sistemas operacionais com sistemas de arquivos de maiusculas e minúsculas (por exemplo, Windows), pesquisas de provedor de arquivo físico diferenciam maiusculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="c0e21-268">On operating systems with case insensitive file systems (for example, Windows), physical file provider lookups are case insensitive.</span></span> <span data-ttu-id="c0e21-269">Por exemplo, `return View("Test")` resulta em correspondências para */Views/Home/Test.cshtml*, */Views/home/test.cshtml*e qualquer outra variante de maiusculas e minúsculas.</span><span class="sxs-lookup"><span data-stu-id="c0e21-269">For example, `return View("Test")` results in matches for */Views/Home/Test.cshtml*, */Views/home/test.cshtml*, and any other casing variant.</span></span>
  * <span data-ttu-id="c0e21-270">Em sistemas de arquivos diferencia maiusculas de minúsculas (por exemplo, Linux, OSX e com `EmbeddedFileProvider`), pesquisas diferenciam maiusculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="c0e21-270">On case sensitive file systems (for example, Linux, OSX, and with `EmbeddedFileProvider`), lookups are case sensitive.</span></span> <span data-ttu-id="c0e21-271">Por exemplo, `return View("Test")` especificamente correspondências */Views/Home/Test.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="c0e21-271">For example, `return View("Test")` specifically matches */Views/Home/Test.cshtml*.</span></span>
* <span data-ttu-id="c0e21-272">Pré-compilados exibições: com o núcleo do ASP.Net 2.0 e posterior, procurando os modos de exibição pré-compilado diferencia maiusculas de minúsculas em todos os sistemas operacionais.</span><span class="sxs-lookup"><span data-stu-id="c0e21-272">Precompiled views: With ASP.Net Core 2.0 and later, looking up precompiled views is case insensitive on all operating systems.</span></span> <span data-ttu-id="c0e21-273">O comportamento é idêntico ao comportamento do provedor de arquivo físico no Windows.</span><span class="sxs-lookup"><span data-stu-id="c0e21-273">The behavior is identical to physical file provider's behavior on Windows.</span></span> <span data-ttu-id="c0e21-274">Se os dois modos de exibição pré-compilado diferem apenas em maiusculas, o resultado da pesquisa é não determinística.</span><span class="sxs-lookup"><span data-stu-id="c0e21-274">If two precompiled views differ only in case, the result of lookup is non-deterministic.</span></span>

<span data-ttu-id="c0e21-275">Os desenvolvedores são encorajados para coincidir com o uso de maiusculas e minúsculas dos nomes de arquivo e diretório para o uso de maiusculas e minúsculas dos nomes de área, controlador e ação.</span><span class="sxs-lookup"><span data-stu-id="c0e21-275">Developers are encouraged to match the casing of file and directory names to the casing of area, controller, and action names.</span></span> <span data-ttu-id="c0e21-276">Isso garante que as implantações encontrará seus modos de exibição, independentemente do sistema de arquivos subjacente.</span><span class="sxs-lookup"><span data-stu-id="c0e21-276">This ensures your deployments will find their views regardless of the underlying file system.</span></span>

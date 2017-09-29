---
title: "Referência de sintaxe do Razor para o ASP.NET Core"
author: rick-anderson
description: Fornece detalhes sobre a sintaxe do Razor
keywords: "Núcleo do ASP.NET Razor"
ms.author: riande
manager: wpickett
ms.date: 07/04/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/razor
ms.openlocfilehash: 0e65f0e9f672f9f93256ebc039ea0db2e4ef5ae0
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/28/2017
---
# <a name="razor-syntax-for-aspnet-core"></a><span data-ttu-id="10c5f-104">Sintaxe do Razor para o ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="10c5f-104">Razor syntax for ASP.NET Core</span></span>

<span data-ttu-id="10c5f-105">Por [Taylor Mullen](https://twitter.com/ntaylormullen) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="10c5f-105">By [Taylor Mullen](https://twitter.com/ntaylormullen) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

## <a name="what-is-razor"></a><span data-ttu-id="10c5f-106">O que é Razor?</span><span class="sxs-lookup"><span data-stu-id="10c5f-106">What is Razor?</span></span>

<span data-ttu-id="10c5f-107">Razor é uma sintaxe de marcação para inserir o código de servidor com base em páginas da web.</span><span class="sxs-lookup"><span data-stu-id="10c5f-107">Razor is a markup syntax for embedding server based code into web pages.</span></span> <span data-ttu-id="10c5f-108">A sintaxe do Razor consiste em marcação Razor, c# e HTML.</span><span class="sxs-lookup"><span data-stu-id="10c5f-108">The Razor syntax consists of Razor markup, C# and HTML.</span></span> <span data-ttu-id="10c5f-109">Arquivos que contêm Razor geralmente têm um *. cshtml* extensão de arquivo.</span><span class="sxs-lookup"><span data-stu-id="10c5f-109">Files containing Razor generally have a *.cshtml* file extension.</span></span>

## <a name="rendering-html"></a><span data-ttu-id="10c5f-110">Renderização HTML</span><span class="sxs-lookup"><span data-stu-id="10c5f-110">Rendering HTML</span></span>

<span data-ttu-id="10c5f-111">O idioma do Razor padrão é HTML.</span><span class="sxs-lookup"><span data-stu-id="10c5f-111">The default Razor language is HTML.</span></span> <span data-ttu-id="10c5f-112">Renderização HTML do Razor não é diferente de em um arquivo HTML.</span><span class="sxs-lookup"><span data-stu-id="10c5f-112">Rendering HTML from Razor is no different than in an HTML file.</span></span> <span data-ttu-id="10c5f-113">Um arquivo Razor com a seguinte marcação:</span><span class="sxs-lookup"><span data-stu-id="10c5f-113">A Razor file with the following markup:</span></span>

```html
<p>Hello World</p>
```

<span data-ttu-id="10c5f-114">É renderizado inalterado como `<p>Hello World</p>` pelo servidor.</span><span class="sxs-lookup"><span data-stu-id="10c5f-114">Is rendered unchanged as `<p>Hello World</p>` by the server.</span></span>

## <a name="razor-syntax"></a><span data-ttu-id="10c5f-115">Sintaxe do Razor</span><span class="sxs-lookup"><span data-stu-id="10c5f-115">Razor syntax</span></span>

<span data-ttu-id="10c5f-116">Razor suporta c# e usa o `@` símbolo para fazer a transição de HTML para c#.</span><span class="sxs-lookup"><span data-stu-id="10c5f-116">Razor supports C# and uses the `@` symbol to transition from HTML to C#.</span></span> <span data-ttu-id="10c5f-117">Razor avalia expressões c# e renderiza-los na saída HTML.</span><span class="sxs-lookup"><span data-stu-id="10c5f-117">Razor evaluates C# expressions and renders them in the HTML output.</span></span> <span data-ttu-id="10c5f-118">O Razor pode fazer a transição do HTML em C# ou em marcação específica do Razor.</span><span class="sxs-lookup"><span data-stu-id="10c5f-118">Razor can transition from HTML into C# or into Razor-specific markup.</span></span> <span data-ttu-id="10c5f-119">Quando um `@` símbolo é seguido por um [Razor reservados a palavra-chave](#razor-reserved-keywords) ela faz a transição para marcação Razor específico, caso contrário, ela faz a transição em c# simples.</span><span class="sxs-lookup"><span data-stu-id="10c5f-119">When an `@` symbol is followed by a [Razor reserved keyword](#razor-reserved-keywords) it transitions into Razor-specific markup, otherwise it transitions into plain C#.</span></span>

<a name=escape-at-label></a>

<span data-ttu-id="10c5f-120">HTML que contém `@` símbolos talvez precise ser substituídos com um segundo `@` símbolo.</span><span class="sxs-lookup"><span data-stu-id="10c5f-120">HTML containing `@` symbols may need to be escaped with a second `@` symbol.</span></span> <span data-ttu-id="10c5f-121">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="10c5f-121">For example:</span></span>

```cshtml
<p>@@Username</p>
```

<span data-ttu-id="10c5f-122">seria renderizar HTML a seguir:</span><span class="sxs-lookup"><span data-stu-id="10c5f-122">would render the following HTML:</span></span>

```cshtml
<p>@Username</p>
```

<a name=razor-email-ref></a>

<span data-ttu-id="10c5f-123">Atributos HTML e o conteúdo que contém endereços de email não tratam o `@` símbolo como um caractere de transição.</span><span class="sxs-lookup"><span data-stu-id="10c5f-123">HTML attributes and content containing email addresses don’t treat the `@` symbol as a transition character.</span></span>

   `<a href="mailto:Support@contoso.com">Support@contoso.com</a>`

## <a name="implicit-razor-expressions"></a><span data-ttu-id="10c5f-124">Expressões implícitas do Razor</span><span class="sxs-lookup"><span data-stu-id="10c5f-124">Implicit Razor expressions</span></span>

<span data-ttu-id="10c5f-125">As expressões implícitas Razor iniciar com `@` seguido do código c#.</span><span class="sxs-lookup"><span data-stu-id="10c5f-125">Implicit Razor expressions start with `@` followed by C# code.</span></span> <span data-ttu-id="10c5f-126">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="10c5f-126">For example:</span></span>

```html
<p>@DateTime.Now</p>
<p>@DateTime.IsLeapYear(2016)</p>
```

<span data-ttu-id="10c5f-127">Com exceção do c# `await` expressões implícitas de palavra-chave não devem conter espaços.</span><span class="sxs-lookup"><span data-stu-id="10c5f-127">With the exception of the C# `await` keyword implicit expressions must not contain spaces.</span></span> <span data-ttu-id="10c5f-128">Por exemplo, você pode intermingle espaços como a instrução c# tem uma terminação clara:</span><span class="sxs-lookup"><span data-stu-id="10c5f-128">For example, you can intermingle spaces as long as the C# statement has a clear ending:</span></span>

```html
<p>@await DoSomething("hello", "world")</p>
```

## <a name="explicit-razor-expressions"></a><span data-ttu-id="10c5f-129">Expressões explícitas Razor</span><span class="sxs-lookup"><span data-stu-id="10c5f-129">Explicit Razor expressions</span></span>

<span data-ttu-id="10c5f-130">As expressões explícitas Razor consiste em um símbolo com parênteses equilibrada @.</span><span class="sxs-lookup"><span data-stu-id="10c5f-130">Explicit Razor expressions consists of an @ symbol with balanced parenthesis.</span></span> <span data-ttu-id="10c5f-131">Por exemplo, para renderizar a hora da última semana:</span><span class="sxs-lookup"><span data-stu-id="10c5f-131">For example, to render last week's time:</span></span>

```html
<p>Last week this time: @(DateTime.Now - TimeSpan.FromDays(7))</p>
```

<span data-ttu-id="10c5f-132">Qualquer conteúdo dentro do @ () parênteses é avaliado e renderizado para a saída.</span><span class="sxs-lookup"><span data-stu-id="10c5f-132">Any content within the @() parenthesis is evaluated and rendered to the output.</span></span>

<span data-ttu-id="10c5f-133">Em geral, expressões implícitas não podem conter espaços.</span><span class="sxs-lookup"><span data-stu-id="10c5f-133">Implicit expressions generally cannot contain spaces.</span></span> <span data-ttu-id="10c5f-134">Por exemplo, no código a seguir, uma semana não é subtraída da hora atual:</span><span class="sxs-lookup"><span data-stu-id="10c5f-134">For example, in the code below, one week is not subtracted from the current time:</span></span>

[!code-html[Main](razor/sample/Views/Home/Contact.cshtml?range=20)]

<span data-ttu-id="10c5f-135">Que renderiza HTML a seguir:</span><span class="sxs-lookup"><span data-stu-id="10c5f-135">Which renders the following HTML:</span></span>

```html
<p>Last week: 7/7/2016 4:39:52 PM - TimeSpan.FromDays(7)</p>
```

<span data-ttu-id="10c5f-136">Você pode usar uma expressão explícita para concatenação de texto com um resultado da expressão:</span><span class="sxs-lookup"><span data-stu-id="10c5f-136">You can use an explicit expression to concatenate text with an expression result:</span></span>

```cshtml
@{
    var joe = new Person("Joe", 33);
}

<p>Age@(joe.Age)</p>
```

<span data-ttu-id="10c5f-137">Sem expressão explícita, `<p>Age@joe.Age</p>` seria tratada como um endereço de email e `<p>Age@joe.Age</p>` seriam processados.</span><span class="sxs-lookup"><span data-stu-id="10c5f-137">Without the explicit expression, `<p>Age@joe.Age</p>` would be treated as an email address and `<p>Age@joe.Age</p>` would be rendered.</span></span> <span data-ttu-id="10c5f-138">Quando gravado como uma expressão explícita, `<p>Age33</p>` é renderizado.</span><span class="sxs-lookup"><span data-stu-id="10c5f-138">When written as an explicit expression, `<p>Age33</p>` is rendered.</span></span>

<a name=expression-encoding-label></a>

## <a name="expression-encoding"></a><span data-ttu-id="10c5f-139">Codificação de expressão</span><span class="sxs-lookup"><span data-stu-id="10c5f-139">Expression encoding</span></span>

<span data-ttu-id="10c5f-140">Expressões em c# que são avaliadas como uma cadeia de caracteres estão codificada em HTML.</span><span class="sxs-lookup"><span data-stu-id="10c5f-140">C# expressions that evaluate to a string are HTML encoded.</span></span> <span data-ttu-id="10c5f-141">Expressões em c# que avaliam `IHtmlContent` são renderizados diretamente por meio *IHtmlContent.WriteTo*.</span><span class="sxs-lookup"><span data-stu-id="10c5f-141">C# expressions that evaluate to `IHtmlContent` are rendered directly through *IHtmlContent.WriteTo*.</span></span> <span data-ttu-id="10c5f-142">Expressões em c# que não retornam *IHtmlContent* são convertidos em uma cadeia de caracteres (por *ToString*) e codificado antes de serem processadas.</span><span class="sxs-lookup"><span data-stu-id="10c5f-142">C# expressions that don't evaluate to *IHtmlContent* are converted to a string (by *ToString*) and encoded before they are rendered.</span></span> <span data-ttu-id="10c5f-143">Por exemplo, a seguinte marcação Razor:</span><span class="sxs-lookup"><span data-stu-id="10c5f-143">For example, the following Razor markup:</span></span>

```cshtml
@("<span>Hello World</span>")
```

<span data-ttu-id="10c5f-144">Renderiza HTML:</span><span class="sxs-lookup"><span data-stu-id="10c5f-144">Renders this HTML:</span></span>

```html
&lt;span&gt;Hello World&lt;/span&gt;
```

<span data-ttu-id="10c5f-145">Qual o navegador renderiza como:</span><span class="sxs-lookup"><span data-stu-id="10c5f-145">Which the browser renders as:</span></span>

`<span>Hello World</span>`

<span data-ttu-id="10c5f-146">`HtmlHelper``Raw` saída não é codificada mas renderizada como marcação HTML.</span><span class="sxs-lookup"><span data-stu-id="10c5f-146">`HtmlHelper` `Raw` output is not encoded but rendered as HTML markup.</span></span>

>[!WARNING]
> <span data-ttu-id="10c5f-147">Usando `HtmlHelper.Raw` usuário unsanitized a entrada é um risco de segurança.</span><span class="sxs-lookup"><span data-stu-id="10c5f-147">Using `HtmlHelper.Raw` on unsanitized user input is a security risk.</span></span> <span data-ttu-id="10c5f-148">Entrada do usuário pode conter outras explorações ou JavaScript mal-intencionados.</span><span class="sxs-lookup"><span data-stu-id="10c5f-148">User input might contain malicious JavaScript or other exploits.</span></span> <span data-ttu-id="10c5f-149">Limpeza de entrada do usuário é difícil, evite usar `HtmlHelper.Raw` na entrada do usuário.</span><span class="sxs-lookup"><span data-stu-id="10c5f-149">Sanitizing user input is difficult, avoid using `HtmlHelper.Raw` on user input.</span></span>

<span data-ttu-id="10c5f-150">A seguinte marcação Razor:</span><span class="sxs-lookup"><span data-stu-id="10c5f-150">The following Razor markup:</span></span>

```cshtml
@Html.Raw("<span>Hello World</span>")
```

<span data-ttu-id="10c5f-151">Renderiza HTML:</span><span class="sxs-lookup"><span data-stu-id="10c5f-151">Renders this HTML:</span></span>

```html
<span>Hello World</span>
```

<a name=razor-code-blocks-label></a>

## <a name="razor-code-blocks"></a><span data-ttu-id="10c5f-152">Blocos de código Razor</span><span class="sxs-lookup"><span data-stu-id="10c5f-152">Razor code blocks</span></span>

<span data-ttu-id="10c5f-153">Blocos de código Razor iniciar com `@` e são delimitados por `{}`.</span><span class="sxs-lookup"><span data-stu-id="10c5f-153">Razor code blocks start with `@` and are enclosed by `{}`.</span></span> <span data-ttu-id="10c5f-154">Ao contrário das expressões, o código c# dentro de blocos de código não será renderizado.</span><span class="sxs-lookup"><span data-stu-id="10c5f-154">Unlike expressions, C# code inside code blocks is not rendered.</span></span> <span data-ttu-id="10c5f-155">Blocos de código e expressões em uma página de Razor compartilhem o mesmo escopo e são definidas em ordem (ou seja, declarações em um bloco de código estará no escopo para expressões e blocos de código mais recente).</span><span class="sxs-lookup"><span data-stu-id="10c5f-155">Code blocks and expressions in a Razor page share the same scope and are defined in order (that is, declarations in a code block will be in scope for later code blocks and expressions).</span></span>

```none
@{
    var output = "Hello World";
}

<p>The rendered result: @output</p>
```

<span data-ttu-id="10c5f-156">Geraria:</span><span class="sxs-lookup"><span data-stu-id="10c5f-156">Would render:</span></span>

```html
<p>The rendered result: Hello World</p>
```

<a name=implicit-transitions-label></a>

### <a name="implicit-transitions"></a><span data-ttu-id="10c5f-157">Transições implícita</span><span class="sxs-lookup"><span data-stu-id="10c5f-157">Implicit transitions</span></span>

<span data-ttu-id="10c5f-158">É o idioma padrão em um bloco de código c#, mas você pode fazer a transição para HTML.</span><span class="sxs-lookup"><span data-stu-id="10c5f-158">The default language in a code block is C#, but you can transition back to HTML.</span></span> <span data-ttu-id="10c5f-159">HTML dentro de um bloco de código fará a transição para renderizar HTML:</span><span class="sxs-lookup"><span data-stu-id="10c5f-159">HTML within a code block will transition back into rendering HTML:</span></span>

```cshtml
@{
    var inCSharp = true;
    <p>Now in HTML, was in C# @inCSharp</p>
}
```

<a name=explicit-delimited-transition-label></a>

### <a name="explicit-delimited-transition"></a><span data-ttu-id="10c5f-160">Transição delimitada explícita</span><span class="sxs-lookup"><span data-stu-id="10c5f-160">Explicit delimited transition</span></span>

<span data-ttu-id="10c5f-161">Para definir uma subseção de um bloco de código que deve renderizar HTML, coloque os caracteres a ser processado com o Razor `<text>` marca:</span><span class="sxs-lookup"><span data-stu-id="10c5f-161">To define a sub-section of a code block that should render HTML, surround the characters to be rendered with the Razor `<text>` tag:</span></span>

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <text>Name: @person.Name</text>
}
```

<span data-ttu-id="10c5f-162">Você geralmente usa essa abordagem quando você deseja renderizar HTML que não está circundado por uma marca HTML.</span><span class="sxs-lookup"><span data-stu-id="10c5f-162">You generally use this approach when you want to render HTML that is not surrounded by an HTML tag.</span></span> <span data-ttu-id="10c5f-163">Sem uma marca HTML ou Razor, você obtém um erro de tempo de execução do Razor.</span><span class="sxs-lookup"><span data-stu-id="10c5f-163">Without an HTML or Razor tag, you get a Razor runtime error.</span></span>

<a name=explicit-line-transition-with-label></a>

### <a name="explicit-line-transition-with-"></a><span data-ttu-id="10c5f-164">Transição de linha explícita com`@:`</span><span class="sxs-lookup"><span data-stu-id="10c5f-164">Explicit Line Transition with `@:`</span></span>

<span data-ttu-id="10c5f-165">Para processar o restante de uma linha inteira como HTML dentro de um bloco de código, use o `@:` sintaxe:</span><span class="sxs-lookup"><span data-stu-id="10c5f-165">To render the rest of an entire line as HTML inside a code block, use the `@:` syntax:</span></span>

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    @:Name: @person.Name
}
```

<span data-ttu-id="10c5f-166">Sem o `@:` no código acima, você receberia um erro de tempo de execução de Razor.</span><span class="sxs-lookup"><span data-stu-id="10c5f-166">Without the `@:` in the code above, you'd get a Razor run time error.</span></span>

<a name=control-structures-razor-label></a>

## <a name="control-structures"></a><span data-ttu-id="10c5f-167">Estruturas de controle</span><span class="sxs-lookup"><span data-stu-id="10c5f-167">Control Structures</span></span>

<span data-ttu-id="10c5f-168">Estruturas de controle são uma extensão dos blocos de código.</span><span class="sxs-lookup"><span data-stu-id="10c5f-168">Control structures are an extension of code blocks.</span></span> <span data-ttu-id="10c5f-169">Todos os aspectos dos blocos de código (em transição para marcação, embutido c#) também se aplicam aos seguintes estruturas.</span><span class="sxs-lookup"><span data-stu-id="10c5f-169">All aspects of code blocks (transitioning to markup, inline C#) also apply to the following structures.</span></span>

### <a name="conditionals-if-else-if-else-and-switch"></a><span data-ttu-id="10c5f-170">Condicionais `@if`, `else if`, `else` e`@switch`</span><span class="sxs-lookup"><span data-stu-id="10c5f-170">Conditionals `@if`, `else if`, `else` and `@switch`</span></span>

<span data-ttu-id="10c5f-171">O `@if` família controla quando o código é executado:</span><span class="sxs-lookup"><span data-stu-id="10c5f-171">The `@if` family controls when code runs:</span></span>

```cshtml
@if (value % 2 == 0)
{
    <p>The value was even</p>
}
```

<span data-ttu-id="10c5f-172">`else`e `else if` não exigem o `@` símbolo:</span><span class="sxs-lookup"><span data-stu-id="10c5f-172">`else` and `else if` don't require the `@` symbol:</span></span>

```cshtml
@if (value % 2 == 0)
{
    <p>The value was even</p>
}
else if (value >= 1337)
{
    <p>The value is large.</p>
}
else
{
    <p>The value was not large and is odd.</p>
}
```

<span data-ttu-id="10c5f-173">Você pode usar uma instrução switch como esta:</span><span class="sxs-lookup"><span data-stu-id="10c5f-173">You can use a switch statement like this:</span></span>

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
        <p>Your number was not 1 or 1337.</p>
        break;
}
```

### <a name="looping-for-foreach-while-and-do-while"></a><span data-ttu-id="10c5f-174">Loop `@for`, `@foreach`, `@while`, e`@do while`</span><span class="sxs-lookup"><span data-stu-id="10c5f-174">Looping `@for`, `@foreach`, `@while`, and `@do while`</span></span>

<span data-ttu-id="10c5f-175">Você pode renderizar HTML modelado com instruções de controle de loop.</span><span class="sxs-lookup"><span data-stu-id="10c5f-175">You can render templated HTML with looping control statements.</span></span> <span data-ttu-id="10c5f-176">Por exemplo, para renderizar uma lista de pessoas:</span><span class="sxs-lookup"><span data-stu-id="10c5f-176">For example, to render a list of people:</span></span>

```cshtml
@{
    var people = new Person[]
    {
          new Person("John", 33),
          new Person("Doe", 41),
    };
}
```

<span data-ttu-id="10c5f-177">Você pode usar qualquer uma das seguintes instruções em loop:</span><span class="sxs-lookup"><span data-stu-id="10c5f-177">You can use any of the following looping statements:</span></span>

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

### <a name="compound-using"></a><span data-ttu-id="10c5f-178">Composta`@using`</span><span class="sxs-lookup"><span data-stu-id="10c5f-178">Compound `@using`</span></span>

<span data-ttu-id="10c5f-179">No c# um usando a instrução é usada para garantir que um objeto é descartado.</span><span class="sxs-lookup"><span data-stu-id="10c5f-179">In C# a using statement is used to ensure an object is disposed.</span></span> <span data-ttu-id="10c5f-180">No Razor o mesmo mecanismo pode ser usado para criar auxiliares HTML que contém o conteúdo adicional.</span><span class="sxs-lookup"><span data-stu-id="10c5f-180">In Razor this same mechanism can be used to create HTML helpers that contain additional content.</span></span> <span data-ttu-id="10c5f-181">Por exemplo, é possível utilizar auxiliares HTML para renderizar uma marca de formulário com o `@using` instrução:</span><span class="sxs-lookup"><span data-stu-id="10c5f-181">For instance, we can utilize HTML Helpers to render a form tag with the `@using` statement:</span></span>

```cshtml
@using (Html.BeginForm())
{
    <div>
        email:
        <input type="email" id="Email" name="Email" value="" />
        <button type="submit"> Register </button>
    </div>
}
```

<span data-ttu-id="10c5f-182">Você também pode executar ações de nível de escopo como acima com [auxiliares de marcação](tag-helpers/index.md).</span><span class="sxs-lookup"><span data-stu-id="10c5f-182">You can also perform scope level actions like the above with [Tag Helpers](tag-helpers/index.md).</span></span>

### <a name="try-catch-finally"></a><span data-ttu-id="10c5f-183">`@try`, `catch`, `finally`</span><span class="sxs-lookup"><span data-stu-id="10c5f-183">`@try`, `catch`, `finally`</span></span>

<span data-ttu-id="10c5f-184">Tratamento de exceção é semelhante ao c#:</span><span class="sxs-lookup"><span data-stu-id="10c5f-184">Exception handling is similar to  C#:</span></span>

[!code-html[Main](razor/sample/Views/Home/Contact7.cshtml)]

### `@lock`

<span data-ttu-id="10c5f-185">Razor tem a capacidade de proteger seções críticas com instruções de bloqueio:</span><span class="sxs-lookup"><span data-stu-id="10c5f-185">Razor has the capability to protect critical sections with lock statements:</span></span>

```cshtml
@lock (SomeLock)
{
    // Do critical section work
}
```

### <a name="comments"></a><span data-ttu-id="10c5f-186">Comentários</span><span class="sxs-lookup"><span data-stu-id="10c5f-186">Comments</span></span>

<span data-ttu-id="10c5f-187">Razor dá suporte a comentários c# e HTML.</span><span class="sxs-lookup"><span data-stu-id="10c5f-187">Razor supports C# and HTML comments.</span></span> <span data-ttu-id="10c5f-188">A marcação a seguir:</span><span class="sxs-lookup"><span data-stu-id="10c5f-188">The following markup:</span></span>

```cshtml
@{
    /* C# comment. */
    // Another C# comment.
}
<!-- HTML comment -->
```

<span data-ttu-id="10c5f-189">É processado pelo servidor como:</span><span class="sxs-lookup"><span data-stu-id="10c5f-189">Is rendered by the server as:</span></span>

```cshtml
<!-- HTML comment -->
```

<span data-ttu-id="10c5f-190">Comentários de Razor são removidos pelo servidor antes da página é renderizada.</span><span class="sxs-lookup"><span data-stu-id="10c5f-190">Razor comments are removed by the server before the page is rendered.</span></span> <span data-ttu-id="10c5f-191">Razor usa `@*  *@` para delimitar os comentários.</span><span class="sxs-lookup"><span data-stu-id="10c5f-191">Razor uses `@*  *@` to delimit comments.</span></span> <span data-ttu-id="10c5f-192">O código a seguir é comentado, para que o servidor não processará qualquer marcação:</span><span class="sxs-lookup"><span data-stu-id="10c5f-192">The following code is commented out, so the server will not render any markup:</span></span>

```cshtml
@*
 @{
     /* C# comment. */
     // Another C# comment.
 }
 <!-- HTML comment -->
*@
```

<a name=razor-directives-label></a>

## <a name="directives"></a><span data-ttu-id="10c5f-193">Diretivas</span><span class="sxs-lookup"><span data-stu-id="10c5f-193">Directives</span></span>

<span data-ttu-id="10c5f-194">Diretivas de Razor são representadas por expressões implícitas com os seguintes palavras-chave reservadas do `@` símbolo.</span><span class="sxs-lookup"><span data-stu-id="10c5f-194">Razor directives are represented by implicit expressions with reserved keywords following the `@` symbol.</span></span> <span data-ttu-id="10c5f-195">Uma diretiva será normalmente alterar a maneira como uma página é analisada ou habilitar a funcionalidade diferente dentro de sua página de Razor.</span><span class="sxs-lookup"><span data-stu-id="10c5f-195">A directive will typically change the way a page is parsed or enable different functionality within your Razor page.</span></span>

<span data-ttu-id="10c5f-196">Noções básicas sobre como Razor gera código para um modo de exibição tornará mais fácil de entender como funcionam as diretivas.</span><span class="sxs-lookup"><span data-stu-id="10c5f-196">Understanding how Razor generates code for a view will make it easier to understand how directives work.</span></span> <span data-ttu-id="10c5f-197">Uma página do Razor é usada para gerar um arquivo c#.</span><span class="sxs-lookup"><span data-stu-id="10c5f-197">A Razor page is used to generate a C# file.</span></span> <span data-ttu-id="10c5f-198">Por exemplo, essa página Razor:</span><span class="sxs-lookup"><span data-stu-id="10c5f-198">For example, this Razor page:</span></span>

[!code-html[Main](razor/sample/Views/Home/Contact8.cshtml)]

<span data-ttu-id="10c5f-199">Gera uma classe semelhante à seguinte:</span><span class="sxs-lookup"><span data-stu-id="10c5f-199">Generates a class similar to the following:</span></span>

```csharp
public class _Views_Something_cshtml : RazorPage<dynamic>
{
    public override async Task ExecuteAsync()
    {
        var output = "Hello World";

        WriteLiteral("/r/n<div>Output: ");
        Write(output);
        WriteLiteral("</div>");
    }
}
```

<span data-ttu-id="10c5f-200">[Exibindo a classe Razor c# gerada para uma exibição](#razor-customcompilationservice-label) explica como exibir essa classe gerada.</span><span class="sxs-lookup"><span data-stu-id="10c5f-200">[Viewing the Razor C# class generated for a view](#razor-customcompilationservice-label) explains how to view this generated class.</span></span>

### `@using`

<span data-ttu-id="10c5f-201">O `@using` diretiva adicionará o c# `using` diretiva para a página de razor gerado:</span><span class="sxs-lookup"><span data-stu-id="10c5f-201">The `@using` directive will add the c# `using` directive to the generated razor page:</span></span>

[!code-html[Main](razor/sample/Views/Home/Contact9.cshtml)]

### `@model`

<span data-ttu-id="10c5f-202">O `@model` diretiva especifica o tipo de modelo passado para a página do Razor.</span><span class="sxs-lookup"><span data-stu-id="10c5f-202">The `@model` directive specifies the type of the model passed to the Razor page.</span></span> <span data-ttu-id="10c5f-203">Ela usa a seguinte sintaxe:</span><span class="sxs-lookup"><span data-stu-id="10c5f-203">It uses the following syntax:</span></span>

```cshtml
@model TypeNameOfModel
```

<span data-ttu-id="10c5f-204">Por exemplo, se você criar um aplicativo ASP.NET MVC de núcleo com contas de usuário individuais, o *Views/Account/Login.cshtml* exibição Razor contém a seguinte declaração de modelo:</span><span class="sxs-lookup"><span data-stu-id="10c5f-204">For example, if you create an ASP.NET Core MVC app with individual user accounts, the *Views/Account/Login.cshtml* Razor view contains the following model declaration:</span></span>

```cshtml
@model LoginViewModel
```

<span data-ttu-id="10c5f-205">No exemplo anterior de classe, a classe gerada herda de `RazorPage<dynamic>`.</span><span class="sxs-lookup"><span data-stu-id="10c5f-205">In the preceding class example, the class generated inherits from `RazorPage<dynamic>`.</span></span> <span data-ttu-id="10c5f-206">Adicionando um `@model` você controlar o que é herdado.</span><span class="sxs-lookup"><span data-stu-id="10c5f-206">By adding an `@model` you control what’s inherited.</span></span> <span data-ttu-id="10c5f-207">Por exemplo</span><span class="sxs-lookup"><span data-stu-id="10c5f-207">For example</span></span>

```cshtml
@model LoginViewModel
```

<span data-ttu-id="10c5f-208">Gera a seguinte classe</span><span class="sxs-lookup"><span data-stu-id="10c5f-208">Generates the following class</span></span>

```csharp
public class _Views_Account_Login_cshtml : RazorPage<LoginViewModel>
```

<span data-ttu-id="10c5f-209">Páginas Razor expor um `Model` propriedade para acessar o modelo é passada para a página.</span><span class="sxs-lookup"><span data-stu-id="10c5f-209">Razor pages expose a `Model` property for accessing the model passed to the page.</span></span>

```cshtml
<div>The Login Email: @Model.Email</div>
```

<span data-ttu-id="10c5f-210">O `@model` diretiva especificado o tipo desta propriedade (especificando o `T` em `RazorPage<T>` que deriva de classe gerada para a página).</span><span class="sxs-lookup"><span data-stu-id="10c5f-210">The `@model` directive specified the type of this property (by specifying the `T` in `RazorPage<T>` that the generated class for your page derives from).</span></span> <span data-ttu-id="10c5f-211">Se você não especificar o `@model` diretiva a `Model` propriedade será do tipo `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="10c5f-211">If you don't specify the `@model` directive the `Model` property will be of type `dynamic`.</span></span> <span data-ttu-id="10c5f-212">O valor do modelo é passado do controlador para o modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="10c5f-212">The value of the model is passed from the controller to the view.</span></span> <span data-ttu-id="10c5f-213">Consulte [fortemente tipado modelos e o @model palavra-chave](../../tutorials/first-mvc-app/adding-model.md#strongly-typed-models-keyword-label) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="10c5f-213">See [Strongly typed models and the @model keyword](../../tutorials/first-mvc-app/adding-model.md#strongly-typed-models-keyword-label) for more information.</span></span>

### `@inherits`

<span data-ttu-id="10c5f-214">O `@inherits` diretiva lhe dá controle total sobre a classe que herda de sua página Razor:</span><span class="sxs-lookup"><span data-stu-id="10c5f-214">The `@inherits` directive gives you full control of the class your Razor page inherits:</span></span>

```cshtml
@inherits TypeNameOfClassToInheritFrom
```

<span data-ttu-id="10c5f-215">Por exemplo, digamos que tivemos que o seguinte tipo de página Razor personalizado:</span><span class="sxs-lookup"><span data-stu-id="10c5f-215">For instance, let’s say we had the following custom Razor page type:</span></span>

[!code-csharp[Main](razor/sample/Classes/CustomRazorPage.cs)]

<span data-ttu-id="10c5f-216">O Razor seguir geraria `<div>Custom text: Hello World</div>`.</span><span class="sxs-lookup"><span data-stu-id="10c5f-216">The following Razor would generate `<div>Custom text: Hello World</div>`.</span></span>

[!code-html[Main](razor/sample/Views/Home/Contact10.cshtml)]

<span data-ttu-id="10c5f-217">Não é possível usar `@model` e `@inherits` na mesma página.</span><span class="sxs-lookup"><span data-stu-id="10c5f-217">You can't use `@model` and `@inherits` on the same page.</span></span> <span data-ttu-id="10c5f-218">Você pode ter `@inherits` em uma *viewimports. cshtml* arquivo importa a página do Razor.</span><span class="sxs-lookup"><span data-stu-id="10c5f-218">You can have `@inherits` in a *_ViewImports.cshtml* file that the Razor page imports.</span></span> <span data-ttu-id="10c5f-219">Por exemplo, se o modo de exibição Razor importado o seguinte *viewimports. cshtml* arquivo:</span><span class="sxs-lookup"><span data-stu-id="10c5f-219">For example, if your Razor view imported the following *_ViewImports.cshtml* file:</span></span>

[!code-html[Main](razor/sample/Views/_ViewImportsModel.cshtml)]

<span data-ttu-id="10c5f-220">A seguinte página Razor fortemente tipada</span><span class="sxs-lookup"><span data-stu-id="10c5f-220">The following strongly typed Razor page</span></span>

[!code-html[Main](razor/sample/Views/Home/Login1.cshtml)]

<span data-ttu-id="10c5f-221">Gera essa marcação HTML:</span><span class="sxs-lookup"><span data-stu-id="10c5f-221">Generates this HTML markup:</span></span>

```cshtml
<div>The Login Email: Rick@contoso.com</div>
<div>Custom text: Hello World</div>
```

<span data-ttu-id="10c5f-222">Quando passado "[Rick@contoso.com](mailto:Rick@contoso.com)" no modelo:</span><span class="sxs-lookup"><span data-stu-id="10c5f-222">When passed "[Rick@contoso.com](mailto:Rick@contoso.com)" in the model:</span></span>

   <span data-ttu-id="10c5f-223">Veja [Layout](layout.md) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="10c5f-223">See [Layout](layout.md) for more information.</span></span>

### `@inject`

<span data-ttu-id="10c5f-224">O `@inject` diretiva permite que você inserir um serviço da sua [contêiner de serviço](../../fundamentals/dependency-injection.md) em sua página do Razor para uso.</span><span class="sxs-lookup"><span data-stu-id="10c5f-224">The `@inject` directive enables you to inject a service from your [service container](../../fundamentals/dependency-injection.md)  into your Razor page for use.</span></span> <span data-ttu-id="10c5f-225">Consulte [injeção de dependência para modos de exibição](dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="10c5f-225">See [Dependency injection into views](dependency-injection.md).</span></span>

<a name="functions"></a>

### `@functions`

<span data-ttu-id="10c5f-226">O `@functions` diretiva permite que você adicione conteúdo de nível de função para a página do Razor.</span><span class="sxs-lookup"><span data-stu-id="10c5f-226">The `@functions` directive enables you to add function level content to your Razor page.</span></span> <span data-ttu-id="10c5f-227">A sintaxe é:</span><span class="sxs-lookup"><span data-stu-id="10c5f-227">The syntax is:</span></span>

```cshtml
@functions { // C# Code }
```

<span data-ttu-id="10c5f-228">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="10c5f-228">For example:</span></span>

[!code-html[Main](razor/sample/Views/Home/Contact6.cshtml)]

<span data-ttu-id="10c5f-229">Gera uma marcação HTML a seguir:</span><span class="sxs-lookup"><span data-stu-id="10c5f-229">Generates the following HTML markup:</span></span>

```cshtml
<div>From method: Hello</div>
```

<span data-ttu-id="10c5f-230">Gerado Razor c# é semelhante a:</span><span class="sxs-lookup"><span data-stu-id="10c5f-230">The generated Razor C# looks like:</span></span>

[!code-csharp[Main](razor/sample/Classes/Views_Home_Test_cshtml.cs?range=1-19)]

### `@section`

<span data-ttu-id="10c5f-231">O `@section` diretiva é usada em conjunto com o [página de layout](layout.md) para habilitar os modos de exibição renderizar o conteúdo em diferentes partes da página HTML renderizada.</span><span class="sxs-lookup"><span data-stu-id="10c5f-231">The `@section` directive is used in conjunction with the [layout page](layout.md) to enable views to render content in different parts of the rendered HTML page.</span></span> <span data-ttu-id="10c5f-232">Consulte [seções](layout.md#layout-sections-label) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="10c5f-232">See [Sections](layout.md#layout-sections-label) for more information.</span></span>

## <a name="tag-helpers"></a><span data-ttu-id="10c5f-233">Auxiliares de marcação</span><span class="sxs-lookup"><span data-stu-id="10c5f-233">Tag Helpers</span></span>

<span data-ttu-id="10c5f-234">O seguinte [auxiliares de marcação](tag-helpers/index.md) diretivas detalhadas dos links fornecidos.</span><span class="sxs-lookup"><span data-stu-id="10c5f-234">The following [Tag Helpers](tag-helpers/index.md) directives are detailed in the links provided.</span></span>

* [@addTagHelper](tag-helpers/intro.md#add-helper-label)
* [@removeTagHelper](tag-helpers/intro.md#remove-razor-directives-label)
* [@tagHelperPrefix](tag-helpers/intro.md#prefix-razor-directives-label)

<a name=razor-reserved-keywords-label></a>

## <a name="razor-reserved-keywords"></a><span data-ttu-id="10c5f-235">Palavras-chave do Razor reservado</span><span class="sxs-lookup"><span data-stu-id="10c5f-235">Razor reserved keywords</span></span>

### <a name="razor-keywords"></a><span data-ttu-id="10c5f-236">Palavras-chave do Razor</span><span class="sxs-lookup"><span data-stu-id="10c5f-236">Razor keywords</span></span>

* <span data-ttu-id="10c5f-237">página (requer o ASP.NET Core 2.0 e posterior)</span><span class="sxs-lookup"><span data-stu-id="10c5f-237">page (Requires ASP.NET Core 2.0 and later)</span></span>
* <span data-ttu-id="10c5f-238">funções</span><span class="sxs-lookup"><span data-stu-id="10c5f-238">functions</span></span>
* <span data-ttu-id="10c5f-239">herda</span><span class="sxs-lookup"><span data-stu-id="10c5f-239">inherits</span></span>
* <span data-ttu-id="10c5f-240">modelo</span><span class="sxs-lookup"><span data-stu-id="10c5f-240">model</span></span>
* <span data-ttu-id="10c5f-241">section</span><span class="sxs-lookup"><span data-stu-id="10c5f-241">section</span></span>
* <span data-ttu-id="10c5f-242">auxiliar (sem suporte pelo ASP.NET Core.)</span><span class="sxs-lookup"><span data-stu-id="10c5f-242">helper   (Not supported by ASP.NET Core.)</span></span>

<span data-ttu-id="10c5f-243">Palavras-chave do Razor podem ser ignoradas com `@(Razor Keyword)`, por exemplo `@(functions)`.</span><span class="sxs-lookup"><span data-stu-id="10c5f-243">Razor keywords can be escaped with `@(Razor Keyword)`, for example `@(functions)`.</span></span> <span data-ttu-id="10c5f-244">Consulte o exemplo completo abaixo.</span><span class="sxs-lookup"><span data-stu-id="10c5f-244">See the complete sample below.</span></span>

### <a name="c-razor-keywords"></a><span data-ttu-id="10c5f-245">Palavras-chave do c# Razor</span><span class="sxs-lookup"><span data-stu-id="10c5f-245">C# Razor keywords</span></span>

* <span data-ttu-id="10c5f-246">case</span><span class="sxs-lookup"><span data-stu-id="10c5f-246">case</span></span>
* <span data-ttu-id="10c5f-247">do</span><span class="sxs-lookup"><span data-stu-id="10c5f-247">do</span></span>
* <span data-ttu-id="10c5f-248">default</span><span class="sxs-lookup"><span data-stu-id="10c5f-248">default</span></span>
* <span data-ttu-id="10c5f-249">for</span><span class="sxs-lookup"><span data-stu-id="10c5f-249">for</span></span>
* <span data-ttu-id="10c5f-250">foreach</span><span class="sxs-lookup"><span data-stu-id="10c5f-250">foreach</span></span>
* <span data-ttu-id="10c5f-251">if</span><span class="sxs-lookup"><span data-stu-id="10c5f-251">if</span></span>
* <span data-ttu-id="10c5f-252">else</span><span class="sxs-lookup"><span data-stu-id="10c5f-252">else</span></span>
* <span data-ttu-id="10c5f-253">bloqueio</span><span class="sxs-lookup"><span data-stu-id="10c5f-253">lock</span></span>
* <span data-ttu-id="10c5f-254">switch</span><span class="sxs-lookup"><span data-stu-id="10c5f-254">switch</span></span>
* <span data-ttu-id="10c5f-255">try</span><span class="sxs-lookup"><span data-stu-id="10c5f-255">try</span></span>
* <span data-ttu-id="10c5f-256">catch</span><span class="sxs-lookup"><span data-stu-id="10c5f-256">catch</span></span>
* <span data-ttu-id="10c5f-257">finally</span><span class="sxs-lookup"><span data-stu-id="10c5f-257">finally</span></span>
* <span data-ttu-id="10c5f-258">using</span><span class="sxs-lookup"><span data-stu-id="10c5f-258">using</span></span>
* <span data-ttu-id="10c5f-259">while</span><span class="sxs-lookup"><span data-stu-id="10c5f-259">while</span></span>

<span data-ttu-id="10c5f-260">Palavras-chave do c# Razor precisam ser duplo o caractere de escape `@(@C# Razor Keyword)`, por exemplo `@(@case)`.</span><span class="sxs-lookup"><span data-stu-id="10c5f-260">C# Razor keywords need to be double escaped with `@(@C# Razor Keyword)`, for example `@(@case)`.</span></span> <span data-ttu-id="10c5f-261">A primeira `@` ignora o analisador do Razor, o segundo `@` ignora o analisador c#.</span><span class="sxs-lookup"><span data-stu-id="10c5f-261">The first `@` escapes the Razor parser, the second `@` escapes the C# parser.</span></span> <span data-ttu-id="10c5f-262">Consulte o exemplo completo abaixo.</span><span class="sxs-lookup"><span data-stu-id="10c5f-262">See the complete sample below.</span></span>

### <a name="reserved-keywords-not-used-by-razor"></a><span data-ttu-id="10c5f-263">Palavras-chave reservadas não usadas pelo Razor</span><span class="sxs-lookup"><span data-stu-id="10c5f-263">Reserved keywords not used by Razor</span></span>

* <span data-ttu-id="10c5f-264">namespace</span><span class="sxs-lookup"><span data-stu-id="10c5f-264">namespace</span></span>
* <span data-ttu-id="10c5f-265">classe</span><span class="sxs-lookup"><span data-stu-id="10c5f-265">class</span></span>

<a name=razor-customcompilationservice-label></a>

## <a name="viewing-the-razor-c-class-generated-for-a-view"></a><span data-ttu-id="10c5f-266">Exibindo a classe Razor c# gerada para um modo de exibição</span><span class="sxs-lookup"><span data-stu-id="10c5f-266">Viewing the Razor C# class generated for a view</span></span>

<span data-ttu-id="10c5f-267">Adicione a seguinte classe ao seu projeto MVC do ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="10c5f-267">Add the following class to your ASP.NET Core MVC project:</span></span>

[!code-csharp[Main](razor/sample/Services/CustomCompilationService.cs)]

<span data-ttu-id="10c5f-268">Substituir o `ICompilationService` adicionado pelo MVC com a classe acima.</span><span class="sxs-lookup"><span data-stu-id="10c5f-268">Override the `ICompilationService` added by MVC with the above class;</span></span>

[!code-csharp[Main](razor/sample/Startup.cs?highlight=4&range=29-33)]

<span data-ttu-id="10c5f-269">Definir um ponto de interrupção no `Compile` método `CustomCompilationService` e `compilationContent`.</span><span class="sxs-lookup"><span data-stu-id="10c5f-269">Set a break point on the `Compile` method of `CustomCompilationService` and view `compilationContent`.</span></span>

![Exibição do Visualizador de texto de compilationContent](razor/_static/tvr.png)

<a name="case"></a>
## <a name="view-lookups-and-case-sensitivity"></a><span data-ttu-id="10c5f-271">Pesquisas de exibição e diferenciação de maiusculas e minúsculas</span><span class="sxs-lookup"><span data-stu-id="10c5f-271">View lookups and case sensitivity</span></span>

<span data-ttu-id="10c5f-272">O mecanismo de exibição Razor executa pesquisas diferencia maiusculas de minúsculas para modos de exibição.</span><span class="sxs-lookup"><span data-stu-id="10c5f-272">The Razor view engine performs case-sensitive lookups for views.</span></span> <span data-ttu-id="10c5f-273">No entanto, a pesquisa real é determinada pela origem subjacente:</span><span class="sxs-lookup"><span data-stu-id="10c5f-273">However, the actual lookup is determined by the underlying source:</span></span>

* <span data-ttu-id="10c5f-274">Arquivo de origem com base:</span><span class="sxs-lookup"><span data-stu-id="10c5f-274">File based source:</span></span> 

    * <span data-ttu-id="10c5f-275">Em sistemas operacionais com sistemas de arquivos de maiusculas e minúsculas (como Windows), pesquisas de provedor de arquivo físico diferenciam maiusculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="10c5f-275">On operating systems with case insensitive file systems (like Windows), physical file provider lookups are case insensitive.</span></span> <span data-ttu-id="10c5f-276">Por exemplo `return View("Test")` resultaria em `/Views/Home/Test.cshtml`, `/Views/home/test.cshtml` e todas as variantes de maiusculas e minúsculas deve ser descobertas.</span><span class="sxs-lookup"><span data-stu-id="10c5f-276">For example `return View("Test")` would result in `/Views/Home/Test.cshtml`, `/Views/home/test.cshtml` and all other casing variants would be discovered.</span></span>
    * <span data-ttu-id="10c5f-277">Em sistemas de arquivos diferencia maiusculas de minúsculas, que inclui o Linux, OSX e `EmbeddedFileProvider`, pesquisas diferenciam maiusculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="10c5f-277">On case sensitive file systems, which includes Linux, OSX and `EmbeddedFileProvider`, lookups are case sensitive.</span></span> <span data-ttu-id="10c5f-278">Por exemplo, `return View("Test")` seria especificamente `/Views/Home/Test.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="10c5f-278">For example, `return View("Test")` would specifically look for `/Views/Home/Test.cshtml`.</span></span>
        
* <span data-ttu-id="10c5f-279">Modos de exibição pré-compilado:</span><span class="sxs-lookup"><span data-stu-id="10c5f-279">Precompiled views:</span></span>

   * <span data-ttu-id="10c5f-280">Com o ASP.Net Core 2.0 e posterior, procurando os modos de exibição pré-compilado diferencia maiusculas de minúsculas em todos os sistemas operacionais.</span><span class="sxs-lookup"><span data-stu-id="10c5f-280">With ASP.Net Core 2.0 and later, looking up precompiled views is case insensitive on all operating systems.</span></span> <span data-ttu-id="10c5f-281">O comportamento é idêntico ao comportamento do provedor de arquivo físico no Windows.</span><span class="sxs-lookup"><span data-stu-id="10c5f-281">The behavior is identical to physical file provider's behavior on Windows.</span></span> 
   <span data-ttu-id="10c5f-282">Observação: Se os dois modos de exibição pré-compilado diferem apenas em maiusculas, o resultado da pesquisa é não determinística.</span><span class="sxs-lookup"><span data-stu-id="10c5f-282">Note: If two precompiled views differ only in case, the result of lookup is non-deterministic.</span></span>

<span data-ttu-id="10c5f-283">Os desenvolvedores são encorajados para coincidir com o uso de maiusculas e minúsculas dos nomes de arquivo e diretório para o uso de maiusculas e minúsculas dos nomes de área, controlador e ação.</span><span class="sxs-lookup"><span data-stu-id="10c5f-283">Developers are encouraged to match the casing of file and directory names to the casing of area, controller and action names.</span></span> <span data-ttu-id="10c5f-284">Isso garantiria que suas implantações permanecem independente do sistema de arquivos subjacente.</span><span class="sxs-lookup"><span data-stu-id="10c5f-284">This would ensure your deployments remain agnostic of the underlying file system.</span></span>

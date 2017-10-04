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
# <a name="razor-syntax-for-aspnet-core"></a>Sintaxe do Razor para o ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT), [Luke Latham](https://github.com/guardrex), e [Taylor Mullen](https://twitter.com/ntaylormullen)

Razor é uma sintaxe de marcação para inserir o código de servidor em páginas da Web. A sintaxe do Razor consiste Razor marcação, c# e HTML. Arquivos que contêm Razor geralmente têm um *. cshtml* extensão de arquivo.

## <a name="rendering-html"></a>Renderização HTML

O idioma do Razor padrão é HTML. Renderização HTML da marcação Razor não é diferente de renderização HTML de um arquivo HTML. Se você colocar uma marcação HTML em uma *. cshtml* arquivo Razor, ela é processada pelo servidor inalterado.

## <a name="razor-syntax"></a>Sintaxe do Razor

Razor suporta c# e usa o `@` símbolo para fazer a transição de HTML para c#. Razor avalia expressões c# e renderiza-los na saída HTML.

Quando um `@` símbolo é seguido por um [Razor reservados a palavra-chave](#razor-reserved-keywords), ele faz a transição para marcação específica do Razor. Caso contrário, ela faz a transição em c# simples.

Para escape um `@` símbolo na marcação Razor, usar um segundo `@` símbolo:

```cshtml
<p>@@Username</p>
```

O código é renderizado em HTML com um único `@` símbolo:

```html
<p>@Username</p>
```

Atributos HTML e o conteúdo que contém endereços de email não tratam o `@` símbolo como um caractere de transição. Os endereços de email no exemplo a seguir são inalterados por Razor análise:

```cshtml
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
```

## <a name="implicit-razor-expressions"></a>Expressões implícitas do Razor

As expressões implícitas Razor iniciar com `@` seguido do código do c#:

```cshtml
<p>@DateTime.Now</p>
<p>@DateTime.IsLeapYear(2016)</p>
```

Com exceção do c# `await` palavra-chave, expressões implícitas não devem conter espaços. Você pode intermingle espaços se a instrução c# tem uma terminação clara:

```cshtml
<p>@await DoSomething("hello", "world")</p>
```

## <a name="explicit-razor-expressions"></a>Expressões explícitas Razor

As expressões explícitas Razor consistem em um `@` símbolo com parênteses equilibrada. Para renderizar a hora da última semana, a seguinte marcação Razor é usada:

```cshtml
<p>Last week this time: @(DateTime.Now - TimeSpan.FromDays(7))</p>
```

Qualquer conteúdo dentro do `@()` parênteses é avaliado e renderizado para a saída.

Expressões implícitas, descritas na seção anterior, geralmente não podem conter espaços. No código a seguir, uma semana não é subtraída da hora atual:

[!code-cshtml[Main](razor/sample/Views/Home/Contact.cshtml?range=17)]

O código processa o HTML a seguir:

```html
<p>Last week: 7/7/2016 4:39:52 PM - TimeSpan.FromDays(7)</p>
```

Você pode usar uma expressão explícita para concatenação de texto com um resultado da expressão:

```cshtml
@{
    var joe = new Person("Joe", 33);
}

<p>Age@(joe.Age)</p>
```

Sem expressão explícita, `<p>Age@joe.Age</p>` é tratado como um endereço de email, e `<p>Age@joe.Age</p>` é renderizado. Quando gravado como uma expressão explícita, `<p>Age33</p>` é renderizado.

## <a name="expression-encoding"></a>Codificação de expressão

Expressões em c# que são avaliadas como uma cadeia de caracteres estão codificada em HTML. Expressões em c# que avaliam `IHtmlContent` são renderizados diretamente por meio `IHtmlContent.WriteTo`. Expressões em c# que não retornam `IHtmlContent` são convertidos em uma cadeia de caracteres por `ToString` e codificado antes que eles são renderizados.

```cshtml
@("<span>Hello World</span>")
```

O código processa o HTML a seguir:

```html
&lt;span&gt;Hello World&lt;/span&gt;
```

O HTML é mostrado no navegador como:

```
<span>Hello World</span>
```

`HtmlHelper.Raw`saída não é codificada mas renderizada como marcação HTML.

> [!WARNING]
> Usando `HtmlHelper.Raw` usuário unsanitized a entrada é um risco de segurança. Entrada do usuário pode conter outras explorações ou JavaScript mal-intencionados. Limpeza de entrada do usuário é difícil. Evite usar `HtmlHelper.Raw` com entrada do usuário.

```cshtml
@Html.Raw("<span>Hello World</span>")
```

O código processa o HTML a seguir:

```html
<span>Hello World</span>
```

## <a name="razor-code-blocks"></a>Blocos de código Razor

Blocos de código Razor iniciar com `@` e são delimitados por `{}`. Ao contrário das expressões, o código c# dentro de blocos de código não está processado. Blocos de código e expressões em uma exibição compartilhem o mesmo escopo e são definidas em ordem:

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

O código processa o HTML a seguir:

```html
<p>The future depends on what you do today. - Mahatma Gandhi</p>
<p>Hate cannot drive out hate, only love can do that. - Martin Luther King, Jr.</p>
```

### <a name="implicit-transitions"></a>Transições implícita

É o idioma padrão em um bloco de código c#, mas você pode fazer a transição para HTML:

```cshtml
@{
    var inCSharp = true;
    <p>Now in HTML, was in C# @inCSharp</p>
}
```

### <a name="explicit-delimited-transition"></a>Transição delimitada explícita

Para definir uma subseção de um bloco de código que deve renderizar HTML, coloque os caracteres para renderização o Razor  **\<texto >** marca:

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <text>Name: @person.Name</text>
}
```

Use essa abordagem quando você deseja renderizar HTML que não está circundado por uma marca HTML. Sem uma marca HTML ou Razor, você receberá um erro de tempo de execução do Razor.

O  **\<texto >** marca também é útil para controlar o espaço em branco ao renderizar o conteúdo. Somente o conteúdo entre o  **\<texto >** marcas é processado e nenhum espaço em branco antes ou depois do  **\<texto >** marcas aparece na saída HTML.

### <a name="explicit-line-transition-with-"></a>Transição de linha explícita com @:

Para processar o restante de uma linha inteira como HTML dentro de um bloco de código, use o `@:` sintaxe:

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    @:Name: @person.Name
}
```

Sem o `@:` no código, você receberá um erro de tempo de execução do Razor.

## <a name="control-structures"></a>Estruturas de controle

Estruturas de controle são uma extensão dos blocos de código. Todos os aspectos dos blocos de código (em transição para marcação, embutido c#) também se aplicam aos seguintes estruturas.

### <a name="conditionals-if-else-if-else-and-switch"></a>Condicionais @if, senão se, para outro, e@switch

`@if`Controla quando o código é executado:

```cshtml
@if (value % 2 == 0)
{
    <p>The value was even.</p>
}
```

`else`e `else if` não exigem o `@` símbolo:

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

Você pode usar uma instrução switch como esta:

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

### <a name="looping-for-foreach-while-and-do-while"></a>Loop @for, @foreach, @while, e @do enquanto

Você pode renderizar HTML modelado com instruções de controle de loop. Para renderizar uma lista de pessoas:

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

Você pode usar qualquer uma das seguintes instruções em loop:

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

### <a name="compound-using"></a>Composta@using

No c#, um `using` instrução é usada para garantir que um objeto é descartado. No Razor, o mesmo mecanismo é usado para criar os auxiliares HTML que contém o conteúdo adicional. Por exemplo, você pode utilizar os auxiliares HTML para renderizar uma marca de formulário com o `@using` instrução:

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

Você também pode executar ações no nível de escopo com [auxiliares de marcação](xref:mvc/views/tag-helpers/intro).

### <a name="try-catch-finally"></a>@try, catch, finally

Tratamento de exceção é semelhante ao c#:

[!code-cshtml[Main](razor/sample/Views/Home/Contact7.cshtml)]

### <a name="lock"></a>@lock

Razor tem a capacidade de proteger seções críticas com instruções de bloqueio:

```cshtml
@lock (SomeLock)
{
    // Do critical section work
}
```

### <a name="comments"></a>Comentários

Razor dá suporte a comentários c# e HTML:

```cshtml
@{
    /* C# comment */
    // Another C# comment
}
<!-- HTML comment -->
```

O código processa o HTML a seguir:

```html
<!-- HTML comment -->
```

Comentários de Razor serão removidos pelo servidor para a página da Web seja processada. Razor usa `@*  *@` para delimitar os comentários. O código a seguir é comentado, para que o servidor não processa qualquer marcação:

```cshtml
@*
    @{
        /* C# comment */
        // Another C# comment
    }
    <!-- HTML comment -->
*@
```

## <a name="directives"></a>Diretivas

Diretivas de Razor são representadas por expressões implícitas com os seguintes palavras-chave reservadas do `@` símbolo. Uma diretiva geralmente altera o modo como uma exibição é analisada ou habilita a funcionalidade diferente.

Noções básicas sobre como Razor gera código para um modo de exibição torna mais fácil de entender como funcionam as diretivas.

[!code-html[Main](razor/sample/Views/Home/Contact8.cshtml)]

O código gera uma classe semelhante à seguinte:

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

Posteriormente neste artigo, a seção [exibindo a classe Razor c# gerada para uma exibição](#viewing-the-razor-c-class-generated-for-a-view) explica como exibir essa classe gerada.

### <a name="using"></a>@using

O `@using` diretiva adiciona c# `using` diretiva para o modo de exibição gerado:

[!code-cshtml[Main](razor/sample/Views/Home/Contact9.cshtml)]

### <a name="model"></a>@model

O `@model` diretiva especifica o tipo de modelo passado para um modo de exibição:

```cshtml
@model TypeNameOfModel
```

Se você criar um aplicativo ASP.NET MVC de núcleo com contas de usuário individuais, o *Views/Account/Login.cshtml* exibição contém a declaração de modelo a seguir:

```cshtml
@model LoginViewModel
```

A classe gerada herda de `RazorPage<dynamic>`:

```csharp
public class _Views_Account_Login_cshtml : RazorPage<LoginViewModel>
```

Razor expõe um `Model` propriedade para acessar o modelo passado para o modo de exibição:

```cshtml
<div>The Login Email: @Model.Email</div>
```

O `@model` diretiva especifica o tipo desta propriedade. Especifica a diretiva de `T` em `RazorPage<T>` o modo de exibição que gerado classe que deriva. Se você não especificar o `@model` diretiva, o `Model` é de propriedade do tipo `dynamic`. O valor do modelo é passado do controlador para o modo de exibição. Consulte [fortemente tipado modelos e o @model palavra-chave](xref:tutorials/first-mvc-app/adding-model#strongly-typed-models-keyword-label) para obter mais informações.

### <a name="inherits"></a>@inherits

O `@inherits` diretiva lhe dá controle total da classe herda o modo de exibição:

```cshtml
@inherits TypeNameOfClassToInheritFrom
```

Este é um tipo de página Razor personalizado:

[!code-csharp[Main](razor/sample/Classes/CustomRazorPage.cs)]

O `CustomText` é exibido em um modo de exibição:

[!code-cshtml[Main](razor/sample/Views/Home/Contact10.cshtml)]

O código processa o HTML a seguir:

```html
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

Não é possível usar `@model` e `@inherits` na mesma exibição. Você pode ter `@inherits` em uma *viewimports. cshtml* arquivo que importa o modo de exibição:

[!code-cshtml[Main](razor/sample/Views/_ViewImportsModel.cshtml)]

Este é um exemplo de uma exibição fortemente tipado:

[!code-cshtml[Main](razor/sample/Views/Home/Login1.cshtml)]

Se "rick@contoso.com" é passado no modelo, o modo de exibição gera uma marcação HTML a seguir:

```html
<div>The Login Email: rick@contoso.com</div>
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

### <a name="inject"></a>@inject

O `@inject` diretiva permite que você inserir um serviço da sua [contêiner de serviço](xref:fundamentals/dependency-injection) em seu modo de exibição. Consulte [injeção de dependência para exibições](xref:mvc/views/dependency-injection) para obter mais informações.

### <a name="functions"></a>@functions

O `@functions` diretiva permite que você adicione conteúdo no nível de função para um modo de exibição:

```cshtml
@functions { // C# Code }
```

Por exemplo:

[!code-cshtml[Main](razor/sample/Views/Home/Contact6.cshtml)]

O código gera uma marcação HTML a seguir:

```html
<div>From method: Hello</div>
```

O código a seguir é a classe gerada Razor c#:

[!code-csharp[Main](razor/sample/Classes/Views_Home_Test_cshtml.cs?range=1-19)]

### <a name="section"></a>@section

O `@section` diretiva é usada em conjunto com o [layout](xref:mvc/views/layout) para habilitar os modos de exibição renderizar o conteúdo em diferentes partes da página HTML. Consulte [seções](xref:mvc/views/layout#layout-sections-label) para obter mais informações.

## <a name="tag-helpers"></a>Auxiliares de marcação

Há três diretivas que pertencem ao [auxiliares de marcação](xref:mvc/views/tag-helpers/intro).

| Diretiva | Função |
| --------- | -------- |
| [@addTagHelper](xref:mvc/views/tag-helpers/intro#add-helper-label) | Disponibiliza auxiliares de marcação para um modo de exibição. |
| [@removeTagHelper](xref:mvc/views/tag-helpers/intro#remove-razor-directives-label) | Remove os auxiliares de marcação adicionado anteriormente de um modo de exibição. |
| [@tagHelperPrefix](xref:mvc/views/tag-helpers/intro#prefix-razor-directives-label) | Especifica um prefixo de marca para habilitar o suporte auxiliar de marca e fazer uso do auxiliar de marca explícita. |

## <a name="razor-reserved-keywords"></a>Palavras-chave do Razor reservado

### <a name="razor-keywords"></a>Palavras-chave do Razor

* página (requer o ASP.NET Core 2.0 e posterior)
* funções
* herda
* modelo
* section
* auxiliar (atualmente não há suportada pelo ASP.NET Core)

Palavras-chave do Razor são ignoradas com `@(Razor Keyword)` (por exemplo, `@(functions)`).

### <a name="c-razor-keywords"></a>Palavras-chave do c# Razor

* case
* do
* default
* for
* foreach
* if
* else
* bloqueio
* switch
* try
* catch
* finally
* using
* while

Palavras-chave do c# Razor devem ser de escape duplo com `@(@C# Razor Keyword)` (por exemplo, `@(@case)`). A primeira `@` ignora o analisador do Razor. O segundo `@` ignora o analisador c#.

### <a name="reserved-keywords-not-used-by-razor"></a>Palavras-chave reservadas não usadas pelo Razor

* namespace
* classe

## <a name="viewing-the-razor-c-class-generated-for-a-view"></a>Exibindo a classe Razor c# gerada para um modo de exibição

Adicione a seguinte classe ao seu projeto MVC do ASP.NET Core:

[!code-csharp[Main](razor/sample/Utilities/CustomTemplateEngine.cs)]

Substituir o `RazorTemplateEngine` adicionado pelo MVC com o `CustomTemplateEngine` classe:

[!code-csharp[Main](razor/sample/Startup.cs?highlight=4&range=10-14)]

Definir um ponto de interrupção no `return csharpDocument` instrução `CustomTemplateEngine`. Quando a execução do programa é interrompida no ponto de interrupção, exibir o valor de `generatedCode`.

![Exibição do Visualizador de texto de generatedCode](razor/_static/tvr.png)

## <a name="view-lookups-and-case-sensitivity"></a>Pesquisas de exibição e diferenciação de maiusculas e minúsculas

O mecanismo de exibição Razor executa pesquisas diferencia maiusculas de minúsculas para modos de exibição. No entanto, a pesquisa real é determinada pelo sistema de arquivos subjacente:

* Arquivo de origem com base: 
  * Em sistemas operacionais com sistemas de arquivos de maiusculas e minúsculas (por exemplo, Windows), pesquisas de provedor de arquivo físico diferenciam maiusculas de minúsculas. Por exemplo, `return View("Test")` resulta em correspondências para */Views/Home/Test.cshtml*, */Views/home/test.cshtml*e qualquer outra variante de maiusculas e minúsculas.
  * Em sistemas de arquivos diferencia maiusculas de minúsculas (por exemplo, Linux, OSX e com `EmbeddedFileProvider`), pesquisas diferenciam maiusculas de minúsculas. Por exemplo, `return View("Test")` especificamente correspondências */Views/Home/Test.cshtml*.
* Pré-compilados exibições: com o núcleo do ASP.Net 2.0 e posterior, procurando os modos de exibição pré-compilado diferencia maiusculas de minúsculas em todos os sistemas operacionais. O comportamento é idêntico ao comportamento do provedor de arquivo físico no Windows. Se os dois modos de exibição pré-compilado diferem apenas em maiusculas, o resultado da pesquisa é não determinística.

Os desenvolvedores são encorajados para coincidir com o uso de maiusculas e minúsculas dos nomes de arquivo e diretório para o uso de maiusculas e minúsculas dos nomes de área, controlador e ação. Isso garante que as implantações encontrará seus modos de exibição, independentemente do sistema de arquivos subjacente.

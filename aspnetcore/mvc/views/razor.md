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
# <a name="razor-syntax-for-aspnet-core"></a>Sintaxe do Razor para o ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT), [Luke Latham](https://github.com/guardrex), [Taylor Mullen](https://twitter.com/ntaylormullen), e [Dan Vicarel](https://github.com/Rabadash8820)

Razor é uma sintaxe de marcação para inserir o código de servidor em páginas da Web. A sintaxe do Razor consiste Razor marcação, c# e HTML. Arquivos que contêm Razor geralmente têm um *. cshtml* extensão de arquivo.

## <a name="rendering-html"></a>Renderização HTML

O idioma do Razor padrão é HTML. Renderização HTML da marcação Razor não é diferente de renderização HTML de um arquivo HTML. A marcação HTML em *. cshtml* arquivos do Razor é processado pelo servidor inalterado.

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

Com exceção do c# `await` palavra-chave, expressões implícitas não devem conter espaços. Se a instrução c# tem uma terminação clara, espaços podem misturar:

```cshtml
<p>@await DoSomething("hello", "world")</p>
```

Expressões implícitas **não é possível** contêm c# genéricos, como os caracteres dentro dos colchetes (`<>`) são interpretados como uma marca HTML. O código a seguir é **não** válido:

```cshtml
<p>@GenericMethod<int>()</p>
```

O código anterior gera um erro do compilador semelhante a uma das seguintes opções:

 * O elemento "int" não foi fechado. Todos os elementos devem ser de fechamento automático ou ter uma correspondência de marca de fim.
 *  Não é possível converter o grupo de métodos 'GenericMethod' para 'object' de tipo não delegado. Você pretendia chamar o método?' 
 
Chamadas de método genérico devem ser encapsuladas em um [expressão Razor explícita](#explicit-razor-expressions) ou um [bloco de código Razor](#razor-code-blocks).

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

As expressões explícitas podem ser usadas para concatenação de texto com um resultado da expressão:

```cshtml
@{
    var joe = new Person("Joe", 33);
}

<p>Age@(joe.Age)</p>
```

Sem expressão explícita, `<p>Age@joe.Age</p>` é tratado como um endereço de email, e `<p>Age@joe.Age</p>` é renderizado. Quando gravado como uma expressão explícita, `<p>Age33</p>` é renderizado.


Expressões explícitas podem ser usadas para processar a saída de métodos genéricos em *. cshtml* arquivos. Em uma expressão implícita, os caracteres dentro dos colchetes (`<>`) são interpretados como uma marca HTML. É a seguinte marcação **não** Razor válido:

```cshtml
<p>@GenericMethod<int>()</p>
```

O código anterior gera um erro do compilador semelhante a uma das seguintes opções:

 * O elemento "int" não foi fechado. Todos os elementos devem ser de fechamento automático ou ter uma correspondência de marca de fim.
 *  Não é possível converter o grupo de métodos 'GenericMethod' para 'object' de tipo não delegado. Você pretendia chamar o método?' 
 
 A marcação a seguir mostra a gravação de maneira correta esse código. O código é escrito como uma expressão explícita:

```cshtml
<p>@(GenericMethod<int>())</p>
```

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

É o idioma padrão em um bloco de código c#, mas a página Razor pode fazer a transição para HTML:

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

Use essa abordagem para renderizar HTML que não está circundado por uma marca HTML. Sem uma marca HTML ou Razor, ocorrerá um erro de tempo de execução do Razor.

O  **\<texto >** marca é útil para controlar o espaço em branco ao renderizar o conteúdo:

* Somente o conteúdo entre o  **\<texto >** marca é processada. 
* Não há espaço em branco antes ou depois do  **\<texto >** marca aparece na saída HTML.

### <a name="explicit-line-transition-with-"></a>Transição de linha explícita com @:

Para processar o restante de uma linha inteira como HTML dentro de um bloco de código, use o `@:` sintaxe:

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    @:Name: @person.Name
}
```

Sem o `@:` no código, será gerado um erro de tempo de execução do Razor.

Aviso: Extra `@` caracteres em um arquivo Razor podem causar causar erros de compilador em instruções posteriormente no bloco. Esses erros de compilador podem ser difícil de entender porque o erro real ocorre antes do erro relatado. Esse erro é comum após a combinação de várias expressões implícitas explícita em um bloco de código único.

## <a name="control-structures"></a>Estruturas de controle

Estruturas de controle são uma extensão dos blocos de código. Todos os aspectos dos blocos de código (em transição para marcação, embutido c#) também se aplicam às seguintes estruturas:

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

A marcação a seguir mostra como usar uma instrução de opção:

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

Modelo HTML pode ser renderizado com instruções de controle de loop. Para renderizar uma lista de pessoas:

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

Há suporte para as seguintes instruções de loop:

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

No c#, um `using` instrução é usada para garantir que um objeto é descartado. No Razor, o mesmo mecanismo é usado para criar os auxiliares HTML que contém o conteúdo adicional. No código a seguir, auxiliares HTML renderizar uma marca de formulário com o `@using` instrução:


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

Ações no nível de escopo podem ser executadas com [auxiliares de marcação](xref:mvc/views/tag-helpers/intro).

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

Em um aplicativo MVC do ASP.NET Core criado com contas de usuário individuais, o *Views/Account/Login.cshtml* exibição contém a declaração de modelo a seguir:

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

O `@model` diretiva especifica o tipo desta propriedade. Especifica a diretiva de `T` em `RazorPage<T>` o modo de exibição que gerado classe que deriva. Se o `@model` diretiva especificado, o `Model` é de propriedade do tipo `dynamic`. O valor do modelo é passado do controlador para o modo de exibição. Para obter mais informações, consulte [fortemente tipado modelos e o @model palavra-chave.

### <a name="inherits"></a>@inherits

O `@inherits` diretiva fornece controle total da classe herda o modo de exibição:

```cshtml
@inherits TypeNameOfClassToInheritFrom
```

O código a seguir é um tipo de página Razor personalizado:

[!code-csharp[Main](razor/sample/Classes/CustomRazorPage.cs)]

O `CustomText` é exibido em um modo de exibição:

[!code-cshtml[Main](razor/sample/Views/Home/Contact10.cshtml)]

O código processa o HTML a seguir:

```html
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

 `@model`e `@inherits` podem ser usados na mesma exibição. `@inherits`pode estar em um *viewimports. cshtml* arquivo que importa o modo de exibição:

[!code-cshtml[Main](razor/sample/Views/_ViewImportsModel.cshtml)]

O código a seguir é um exemplo de uma exibição fortemente tipado:

[!code-cshtml[Main](razor/sample/Views/Home/Login1.cshtml)]

Se "rick@contoso.com" é passado no modelo, o modo de exibição gera uma marcação HTML a seguir:

```html
<div>The Login Email: rick@contoso.com</div>
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

### <a name="inject"></a>@inject


O `@inject` diretiva permite que a página do Razor para injetar um serviço a partir de [contêiner de serviço](xref:fundamentals/dependency-injection) em uma exibição. Para obter mais informações, consulte [injeção de dependência para modos de exibição](xref:mvc/views/dependency-injection).

### <a name="functions"></a>@functions

O `@functions` diretiva permite que uma página do Razor para adicionar conteúdo no nível de função para um modo de exibição:

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

O `@section` diretiva é usada em conjunto com o [layout](xref:mvc/views/layout) para habilitar os modos de exibição renderizar o conteúdo em diferentes partes da página HTML. Para obter mais informações, consulte [seções](xref:mvc/views/layout#layout-sections-label).

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

Adicione a seguinte classe ao projeto MVC do ASP.NET Core:

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
* Pré-compilados exibições: com o núcleo do ASP.NET 2.0 e posterior, procurando os modos de exibição pré-compilado diferencia maiusculas de minúsculas em todos os sistemas operacionais. O comportamento é idêntico ao comportamento do provedor de arquivo físico no Windows. Se os dois modos de exibição pré-compilado diferem apenas em maiusculas, o resultado da pesquisa é não determinística.

Os desenvolvedores são encorajados para coincidir com o uso de maiusculas e minúsculas dos nomes de arquivo e diretório para o uso de maiusculas e minúsculas de:

    * Nomes de área, controlador e ação. 
    * Páginas do Razor.
    
Correspondência caso garante que as implantações de localizar as exibições, independentemente do sistema de arquivos subjacente.

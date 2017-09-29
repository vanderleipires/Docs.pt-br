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
# <a name="razor-syntax-for-aspnet-core"></a>Sintaxe do Razor para o ASP.NET Core

Por [Taylor Mullen](https://twitter.com/ntaylormullen) e [Rick Anderson](https://twitter.com/RickAndMSFT)

## <a name="what-is-razor"></a>O que é Razor?

Razor é uma sintaxe de marcação para inserir o código de servidor com base em páginas da web. A sintaxe do Razor consiste em marcação Razor, c# e HTML. Arquivos que contêm Razor geralmente têm um *. cshtml* extensão de arquivo.

## <a name="rendering-html"></a>Renderização HTML

O idioma do Razor padrão é HTML. Renderização HTML do Razor não é diferente de em um arquivo HTML. Um arquivo Razor com a seguinte marcação:

```html
<p>Hello World</p>
```

É renderizado inalterado como `<p>Hello World</p>` pelo servidor.

## <a name="razor-syntax"></a>Sintaxe do Razor

Razor suporta c# e usa o `@` símbolo para fazer a transição de HTML para c#. Razor avalia expressões c# e renderiza-los na saída HTML. O Razor pode fazer a transição do HTML em C# ou em marcação específica do Razor. Quando um `@` símbolo é seguido por um [Razor reservados a palavra-chave](#razor-reserved-keywords) ela faz a transição para marcação Razor específico, caso contrário, ela faz a transição em c# simples.

<a name=escape-at-label></a>

HTML que contém `@` símbolos talvez precise ser substituídos com um segundo `@` símbolo. Por exemplo:

```cshtml
<p>@@Username</p>
```

seria renderizar HTML a seguir:

```cshtml
<p>@Username</p>
```

<a name=razor-email-ref></a>

Atributos HTML e o conteúdo que contém endereços de email não tratam o `@` símbolo como um caractere de transição.

   `<a href="mailto:Support@contoso.com">Support@contoso.com</a>`

## <a name="implicit-razor-expressions"></a>Expressões implícitas do Razor

As expressões implícitas Razor iniciar com `@` seguido do código c#. Por exemplo:

```html
<p>@DateTime.Now</p>
<p>@DateTime.IsLeapYear(2016)</p>
```

Com exceção do c# `await` expressões implícitas de palavra-chave não devem conter espaços. Por exemplo, você pode intermingle espaços como a instrução c# tem uma terminação clara:

```html
<p>@await DoSomething("hello", "world")</p>
```

## <a name="explicit-razor-expressions"></a>Expressões explícitas Razor

As expressões explícitas Razor consiste em um símbolo com parênteses equilibrada @. Por exemplo, para renderizar a hora da última semana:

```html
<p>Last week this time: @(DateTime.Now - TimeSpan.FromDays(7))</p>
```

Qualquer conteúdo dentro do @ () parênteses é avaliado e renderizado para a saída.

Em geral, expressões implícitas não podem conter espaços. Por exemplo, no código a seguir, uma semana não é subtraída da hora atual:

[!code-html[Main](razor/sample/Views/Home/Contact.cshtml?range=20)]

Que renderiza HTML a seguir:

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

Sem expressão explícita, `<p>Age@joe.Age</p>` seria tratada como um endereço de email e `<p>Age@joe.Age</p>` seriam processados. Quando gravado como uma expressão explícita, `<p>Age33</p>` é renderizado.

<a name=expression-encoding-label></a>

## <a name="expression-encoding"></a>Codificação de expressão

Expressões em c# que são avaliadas como uma cadeia de caracteres estão codificada em HTML. Expressões em c# que avaliam `IHtmlContent` são renderizados diretamente por meio *IHtmlContent.WriteTo*. Expressões em c# que não retornam *IHtmlContent* são convertidos em uma cadeia de caracteres (por *ToString*) e codificado antes de serem processadas. Por exemplo, a seguinte marcação Razor:

```cshtml
@("<span>Hello World</span>")
```

Renderiza HTML:

```html
&lt;span&gt;Hello World&lt;/span&gt;
```

Qual o navegador renderiza como:

`<span>Hello World</span>`

`HtmlHelper``Raw` saída não é codificada mas renderizada como marcação HTML.

>[!WARNING]
> Usando `HtmlHelper.Raw` usuário unsanitized a entrada é um risco de segurança. Entrada do usuário pode conter outras explorações ou JavaScript mal-intencionados. Limpeza de entrada do usuário é difícil, evite usar `HtmlHelper.Raw` na entrada do usuário.

A seguinte marcação Razor:

```cshtml
@Html.Raw("<span>Hello World</span>")
```

Renderiza HTML:

```html
<span>Hello World</span>
```

<a name=razor-code-blocks-label></a>

## <a name="razor-code-blocks"></a>Blocos de código Razor

Blocos de código Razor iniciar com `@` e são delimitados por `{}`. Ao contrário das expressões, o código c# dentro de blocos de código não será renderizado. Blocos de código e expressões em uma página de Razor compartilhem o mesmo escopo e são definidas em ordem (ou seja, declarações em um bloco de código estará no escopo para expressões e blocos de código mais recente).

```none
@{
    var output = "Hello World";
}

<p>The rendered result: @output</p>
```

Geraria:

```html
<p>The rendered result: Hello World</p>
```

<a name=implicit-transitions-label></a>

### <a name="implicit-transitions"></a>Transições implícita

É o idioma padrão em um bloco de código c#, mas você pode fazer a transição para HTML. HTML dentro de um bloco de código fará a transição para renderizar HTML:

```cshtml
@{
    var inCSharp = true;
    <p>Now in HTML, was in C# @inCSharp</p>
}
```

<a name=explicit-delimited-transition-label></a>

### <a name="explicit-delimited-transition"></a>Transição delimitada explícita

Para definir uma subseção de um bloco de código que deve renderizar HTML, coloque os caracteres a ser processado com o Razor `<text>` marca:

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <text>Name: @person.Name</text>
}
```

Você geralmente usa essa abordagem quando você deseja renderizar HTML que não está circundado por uma marca HTML. Sem uma marca HTML ou Razor, você obtém um erro de tempo de execução do Razor.

<a name=explicit-line-transition-with-label></a>

### <a name="explicit-line-transition-with-"></a>Transição de linha explícita com`@:`

Para processar o restante de uma linha inteira como HTML dentro de um bloco de código, use o `@:` sintaxe:

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    @:Name: @person.Name
}
```

Sem o `@:` no código acima, você receberia um erro de tempo de execução de Razor.

<a name=control-structures-razor-label></a>

## <a name="control-structures"></a>Estruturas de controle

Estruturas de controle são uma extensão dos blocos de código. Todos os aspectos dos blocos de código (em transição para marcação, embutido c#) também se aplicam aos seguintes estruturas.

### <a name="conditionals-if-else-if-else-and-switch"></a>Condicionais `@if`, `else if`, `else` e`@switch`

O `@if` família controla quando o código é executado:

```cshtml
@if (value % 2 == 0)
{
    <p>The value was even</p>
}
```

`else`e `else if` não exigem o `@` símbolo:

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
        <p>Your number was not 1 or 1337.</p>
        break;
}
```

### <a name="looping-for-foreach-while-and-do-while"></a>Loop `@for`, `@foreach`, `@while`, e`@do while`

Você pode renderizar HTML modelado com instruções de controle de loop. Por exemplo, para renderizar uma lista de pessoas:

```cshtml
@{
    var people = new Person[]
    {
          new Person("John", 33),
          new Person("Doe", 41),
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

### <a name="compound-using"></a>Composta`@using`

No c# um usando a instrução é usada para garantir que um objeto é descartado. No Razor o mesmo mecanismo pode ser usado para criar auxiliares HTML que contém o conteúdo adicional. Por exemplo, é possível utilizar auxiliares HTML para renderizar uma marca de formulário com o `@using` instrução:

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

Você também pode executar ações de nível de escopo como acima com [auxiliares de marcação](tag-helpers/index.md).

### <a name="try-catch-finally"></a>`@try`, `catch`, `finally`

Tratamento de exceção é semelhante ao c#:

[!code-html[Main](razor/sample/Views/Home/Contact7.cshtml)]

### `@lock`

Razor tem a capacidade de proteger seções críticas com instruções de bloqueio:

```cshtml
@lock (SomeLock)
{
    // Do critical section work
}
```

### <a name="comments"></a>Comentários

Razor dá suporte a comentários c# e HTML. A marcação a seguir:

```cshtml
@{
    /* C# comment. */
    // Another C# comment.
}
<!-- HTML comment -->
```

É processado pelo servidor como:

```cshtml
<!-- HTML comment -->
```

Comentários de Razor são removidos pelo servidor antes da página é renderizada. Razor usa `@*  *@` para delimitar os comentários. O código a seguir é comentado, para que o servidor não processará qualquer marcação:

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

## <a name="directives"></a>Diretivas

Diretivas de Razor são representadas por expressões implícitas com os seguintes palavras-chave reservadas do `@` símbolo. Uma diretiva será normalmente alterar a maneira como uma página é analisada ou habilitar a funcionalidade diferente dentro de sua página de Razor.

Noções básicas sobre como Razor gera código para um modo de exibição tornará mais fácil de entender como funcionam as diretivas. Uma página do Razor é usada para gerar um arquivo c#. Por exemplo, essa página Razor:

[!code-html[Main](razor/sample/Views/Home/Contact8.cshtml)]

Gera uma classe semelhante à seguinte:

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

[Exibindo a classe Razor c# gerada para uma exibição](#razor-customcompilationservice-label) explica como exibir essa classe gerada.

### `@using`

O `@using` diretiva adicionará o c# `using` diretiva para a página de razor gerado:

[!code-html[Main](razor/sample/Views/Home/Contact9.cshtml)]

### `@model`

O `@model` diretiva especifica o tipo de modelo passado para a página do Razor. Ela usa a seguinte sintaxe:

```cshtml
@model TypeNameOfModel
```

Por exemplo, se você criar um aplicativo ASP.NET MVC de núcleo com contas de usuário individuais, o *Views/Account/Login.cshtml* exibição Razor contém a seguinte declaração de modelo:

```cshtml
@model LoginViewModel
```

No exemplo anterior de classe, a classe gerada herda de `RazorPage<dynamic>`. Adicionando um `@model` você controlar o que é herdado. Por exemplo

```cshtml
@model LoginViewModel
```

Gera a seguinte classe

```csharp
public class _Views_Account_Login_cshtml : RazorPage<LoginViewModel>
```

Páginas Razor expor um `Model` propriedade para acessar o modelo é passada para a página.

```cshtml
<div>The Login Email: @Model.Email</div>
```

O `@model` diretiva especificado o tipo desta propriedade (especificando o `T` em `RazorPage<T>` que deriva de classe gerada para a página). Se você não especificar o `@model` diretiva a `Model` propriedade será do tipo `dynamic`. O valor do modelo é passado do controlador para o modo de exibição. Consulte [fortemente tipado modelos e o @model palavra-chave](../../tutorials/first-mvc-app/adding-model.md#strongly-typed-models-keyword-label) para obter mais informações.

### `@inherits`

O `@inherits` diretiva lhe dá controle total sobre a classe que herda de sua página Razor:

```cshtml
@inherits TypeNameOfClassToInheritFrom
```

Por exemplo, digamos que tivemos que o seguinte tipo de página Razor personalizado:

[!code-csharp[Main](razor/sample/Classes/CustomRazorPage.cs)]

O Razor seguir geraria `<div>Custom text: Hello World</div>`.

[!code-html[Main](razor/sample/Views/Home/Contact10.cshtml)]

Não é possível usar `@model` e `@inherits` na mesma página. Você pode ter `@inherits` em uma *viewimports. cshtml* arquivo importa a página do Razor. Por exemplo, se o modo de exibição Razor importado o seguinte *viewimports. cshtml* arquivo:

[!code-html[Main](razor/sample/Views/_ViewImportsModel.cshtml)]

A seguinte página Razor fortemente tipada

[!code-html[Main](razor/sample/Views/Home/Login1.cshtml)]

Gera essa marcação HTML:

```cshtml
<div>The Login Email: Rick@contoso.com</div>
<div>Custom text: Hello World</div>
```

Quando passado "[Rick@contoso.com](mailto:Rick@contoso.com)" no modelo:

   Veja [Layout](layout.md) para obter mais informações.

### `@inject`

O `@inject` diretiva permite que você inserir um serviço da sua [contêiner de serviço](../../fundamentals/dependency-injection.md) em sua página do Razor para uso. Consulte [injeção de dependência para modos de exibição](dependency-injection.md).

<a name="functions"></a>

### `@functions`

O `@functions` diretiva permite que você adicione conteúdo de nível de função para a página do Razor. A sintaxe é:

```cshtml
@functions { // C# Code }
```

Por exemplo:

[!code-html[Main](razor/sample/Views/Home/Contact6.cshtml)]

Gera uma marcação HTML a seguir:

```cshtml
<div>From method: Hello</div>
```

Gerado Razor c# é semelhante a:

[!code-csharp[Main](razor/sample/Classes/Views_Home_Test_cshtml.cs?range=1-19)]

### `@section`

O `@section` diretiva é usada em conjunto com o [página de layout](layout.md) para habilitar os modos de exibição renderizar o conteúdo em diferentes partes da página HTML renderizada. Consulte [seções](layout.md#layout-sections-label) para obter mais informações.

## <a name="tag-helpers"></a>Auxiliares de marcação

O seguinte [auxiliares de marcação](tag-helpers/index.md) diretivas detalhadas dos links fornecidos.

* [@addTagHelper](tag-helpers/intro.md#add-helper-label)
* [@removeTagHelper](tag-helpers/intro.md#remove-razor-directives-label)
* [@tagHelperPrefix](tag-helpers/intro.md#prefix-razor-directives-label)

<a name=razor-reserved-keywords-label></a>

## <a name="razor-reserved-keywords"></a>Palavras-chave do Razor reservado

### <a name="razor-keywords"></a>Palavras-chave do Razor

* página (requer o ASP.NET Core 2.0 e posterior)
* funções
* herda
* modelo
* section
* auxiliar (sem suporte pelo ASP.NET Core.)

Palavras-chave do Razor podem ser ignoradas com `@(Razor Keyword)`, por exemplo `@(functions)`. Consulte o exemplo completo abaixo.

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

Palavras-chave do c# Razor precisam ser duplo o caractere de escape `@(@C# Razor Keyword)`, por exemplo `@(@case)`. A primeira `@` ignora o analisador do Razor, o segundo `@` ignora o analisador c#. Consulte o exemplo completo abaixo.

### <a name="reserved-keywords-not-used-by-razor"></a>Palavras-chave reservadas não usadas pelo Razor

* namespace
* classe

<a name=razor-customcompilationservice-label></a>

## <a name="viewing-the-razor-c-class-generated-for-a-view"></a>Exibindo a classe Razor c# gerada para um modo de exibição

Adicione a seguinte classe ao seu projeto MVC do ASP.NET Core:

[!code-csharp[Main](razor/sample/Services/CustomCompilationService.cs)]

Substituir o `ICompilationService` adicionado pelo MVC com a classe acima.

[!code-csharp[Main](razor/sample/Startup.cs?highlight=4&range=29-33)]

Definir um ponto de interrupção no `Compile` método `CustomCompilationService` e `compilationContent`.

![Exibição do Visualizador de texto de compilationContent](razor/_static/tvr.png)

<a name="case"></a>
## <a name="view-lookups-and-case-sensitivity"></a>Pesquisas de exibição e diferenciação de maiusculas e minúsculas

O mecanismo de exibição Razor executa pesquisas diferencia maiusculas de minúsculas para modos de exibição. No entanto, a pesquisa real é determinada pela origem subjacente:

* Arquivo de origem com base: 

    * Em sistemas operacionais com sistemas de arquivos de maiusculas e minúsculas (como Windows), pesquisas de provedor de arquivo físico diferenciam maiusculas de minúsculas. Por exemplo `return View("Test")` resultaria em `/Views/Home/Test.cshtml`, `/Views/home/test.cshtml` e todas as variantes de maiusculas e minúsculas deve ser descobertas.
    * Em sistemas de arquivos diferencia maiusculas de minúsculas, que inclui o Linux, OSX e `EmbeddedFileProvider`, pesquisas diferenciam maiusculas de minúsculas. Por exemplo, `return View("Test")` seria especificamente `/Views/Home/Test.cshtml`.
        
* Modos de exibição pré-compilado:

   * Com o ASP.Net Core 2.0 e posterior, procurando os modos de exibição pré-compilado diferencia maiusculas de minúsculas em todos os sistemas operacionais. O comportamento é idêntico ao comportamento do provedor de arquivo físico no Windows. 
   Observação: Se os dois modos de exibição pré-compilado diferem apenas em maiusculas, o resultado da pesquisa é não determinística.

Os desenvolvedores são encorajados para coincidir com o uso de maiusculas e minúsculas dos nomes de arquivo e diretório para o uso de maiusculas e minúsculas dos nomes de área, controlador e ação. Isso garantiria que suas implantações permanecem independente do sistema de arquivos subjacente.

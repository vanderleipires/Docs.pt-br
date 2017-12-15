---
title: "Modos de exibição no núcleo do ASP.NET MVC"
author: ardalis
description: "Saiba como exibições de lidar com a apresentação de dados do aplicativo e a interação do usuário no ASP.NET MVC de núcleo."
keywords: ASP.NET Core, exibir, MVC, razor, viewmodel, viewbag, viewdata
ms.author: riande
manager: wpickett
ms.date: 12/12/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/overview
ms.openlocfilehash: 2562d4e5fb85159e6ccb47990f54448ddc188077
ms.sourcegitcommit: 198fb0488e961048bfa376cf58cb853ef1d1cb91
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/14/2017
---
# <a name="views-in-aspnet-core-mvc"></a>Modos de exibição no núcleo do ASP.NET MVC

Por [Steve Smith](https://ardalis.com/) e [Luke Latham](https://github.com/guardrex)

Este documento explica os modos de exibição usados em aplicativos ASP.NET MVC de núcleo. Para obter informações sobre páginas Razor, consulte [Introdução a páginas Razor](xref:mvc/razor-pages/index).

No **M**odelo -**V**ibir -**C**ontroller (MVC) padrão, o *exibição* trata a interação de usuário e a apresentação de dados do aplicativo. Uma exibição é um modelo HTML com inseridos [marcação Razor](xref:mvc/views/razor). Marcação Razor é um código que interage com a marcação HTML para produzir uma página da Web que é enviada ao cliente.

No ASP.NET MVC de núcleo, modos de exibição são *. cshtml* arquivos que usam o [linguagem de programação em c#](/dotnet/csharp/) na marcação Razor. Geralmente, arquivos de exibição são agrupados em pastas denominadas para cada do aplicativo [controladores](xref:mvc/controllers/actions). As pastas são armazenadas em um *exibições* pasta na raiz do aplicativo:

![Pasta de modos de exibição no Gerenciador de soluções do Visual Studio é aberta com a pasta base aberta para mostrar os arquivos cshtml, Contact.cshtml e About.cshtml](overview/_static/views_solution_explorer.png)

O *início* controlador é representado por um *início* pasta dentro de *exibições* pasta. O *início* pasta contém os modos de exibição para o *sobre*, *contato*, e *índice* (home page) páginas da Web. Quando um usuário solicita uma dessas três páginas da Web, ações do controlador no *início* controlador determinar qual dos três modos de exibição é usado para criar e retornar uma página da Web para o usuário.

Use [layouts](xref:mvc/views/layout) para fornecer seções da página da Web consistente e reduzir as repetições de código. Layouts geralmente contêm o cabeçalho, elementos de navegação e o menu e o rodapé. O cabeçalho e rodapé geralmente contêm marcação boilerplate muitos elementos de metadados e links para recursos de script e estilo. Layouts de ajudam a evitar essa marcação clichê em exibições.

[Exibições parciais](xref:mvc/views/partial) reduzir a duplicação de código por meio do gerenciamento de componentes reutilizáveis de modos de exibição. Por exemplo, uma exibição parcial é útil para uma biografia do autor em um site de blog que aparece em vários modos de exibição. Uma biografia do autor é comum exibir conteúdo e não requer código a ser executado para produzir o conteúdo da página da Web. Conteúdo de biografia do autor está disponível para o modo de exibição usado sozinho, associação de modelo para que usar uma exibição parcial para este tipo de conteúdo é ideal.

[Exibir componentes](xref:mvc/views/view-components) são modos de exibição semelhantes para parcial em que elas permitem que você reduza código repetitivo, mas eles são apropriados para exibir o conteúdo que requer a execução de código do servidor para renderizar a página da Web. Exiba os componentes são úteis quando o conteúdo renderizado requer interação do banco de dados, como para um site do carrinho de compras. Componentes do modo de exibição não são limitados para associação de modelo para produzir saída de página da Web.

## <a name="benefits-of-using-views"></a>Benefícios do uso de exibições

Modos de exibição ajudam a estabelecer um [ **S**eparation **o**f **C**oncerns (SoC) design](http://deviq.com/separation-of-concerns/) dentro de um aplicativo MVC, separando a marcação de interface de usuário do outras partes do aplicativo. SoC design a seguir faz com que seu aplicativo modular, que fornece vários benefícios:

* O aplicativo é mais fácil manter porque ela foi organizada melhor. Modos de exibição geralmente são agrupados pelo recurso de aplicativo. Isso torna mais fácil de localizar os modos de exibição relacionados ao trabalhar em um recurso.
* As partes do aplicativo são acoplados de forma flexível. Você pode criar e atualizar modos de exibição do aplicativo separadamente dos componentes de acesso de dados e lógica de negócios. Você pode modificar os modos de exibição do aplicativo sem precisar necessariamente atualizar outras partes do aplicativo.
* É mais fácil de testar as partes da interface de usuário do aplicativo, como os modos de exibição são unidades separadas.
* Devido à melhor organização, é menos provável que você vai acidentalmente seções de repetição da interface do usuário.

## <a name="creating-a-view"></a>Criar um modo de exibição

Exibições que são específicas para um controlador são criadas no *modos de exibição / [ControllerName]* pasta. Modos de exibição que são compartilhados entre os controladores são colocados no *exibições/compartilhadas* pasta. Para criar um modo de exibição, adicionar um novo arquivo e dê a ele o mesmo nome que sua ação de controlador associado com o *. cshtml* extensão de arquivo. Para criar uma exibição que corresponde ao *sobre* ação no *início* controlador, crie um *About.cshtml* de arquivos no *exibições/inicial*pasta:

[!code-cshtml[Main](../../common/samples/WebApplication1/Views/Home/About.cshtml)]

*Razor* marcação começa com o `@` símbolo. Código de execução c# instruções colocando c# em [blocos de código Razor](xref:mvc/views/razor#razor-code-blocks) definir chaves (`{ ... }`). Por exemplo, consulte a atribuição de "Sobre" `ViewData["Title"]` mostrado acima. Você pode exibir valores em HTML, simplesmente referenciando o valor com o `@` símbolo. Consulte o conteúdo de `<h2>` e `<h3>` elementos acima.

O conteúdo da exibição mostrado acima é apenas parte da página inteira que é renderizada para o usuário. O resto do layout da página e outros aspectos comuns do modo de exibição são especificados em outros arquivos de exibição. Para obter mais informações, consulte o [tópico Layout](xref:mvc/views/layout).

## <a name="how-controllers-specify-views"></a>Como os controladores de especificam os modos de exibição

Modos de exibição normalmente retornados por ações como um [ViewResult](/aspnet/core/api/microsoft.aspnetcore.mvc.viewresult), que é um tipo de [ActionResult](/aspnet/core/api/microsoft.aspnetcore.mvc.actionresult). O método de ação pode criar e retornar um `ViewResult` diretamente, mas que normalmente não é feito. Desde que a maioria dos controladores herdam [controlador](/aspnet/core/api/microsoft.aspnetcore.mvc.controller), basta usar o `View` método auxiliar para retornar o `ViewResult`:

*HomeController*

[!code-csharp[Main](../../common/samples/WebApplication1/Controllers/HomeController.cs?highlight=5&range=16-21)]

Quando essa ação retorna, o *About.cshtml* exibição mostrada na seção anterior é renderizada como a seguinte página da Web:

![Sobre a página renderizada no navegador Edge](overview/_static/about-page.png)

O `View` método auxiliar tem várias sobrecargas. Opcionalmente, você pode especificar:

* Uma exibição explícita para retornar:

  ```csharp
  return View("Orders");
  ```
* Um [modelo](xref:mvc/models/model-binding) para passar para o modo de exibição:

  ```csharp
  return View(Orders);
  ```
* Um modo de exibição e um modelo:

  ```csharp
  return View("Orders", Orders);
  ```

### <a name="view-discovery"></a>Descoberta do modo de exibição

Quando uma ação retorna uma exibição, um processo chamado *descoberta do modo de exibição* ocorra. Esse processo determina qual arquivo de exibição é usado com base no nome do modo de exibição. 

O comportamento padrão da `View` método (`return View();`) deve retornar uma exibição com o mesmo nome que o método de ação do qual ele é chamado. Por exemplo, o *sobre* `ActionResult` nome do método do controlador é usado para pesquisar um arquivo de modo de exibição denominado *About.cshtml*. Primeiro, o tempo de execução examina o *modos de exibição / [ControllerName]* pasta para o modo de exibição. Se não encontrar uma exibição de correspondência, ele procura o *compartilhado* pasta para o modo de exibição.

Não importa se você retornar implicitamente o `ViewResult` com `return View();` ou passar explicitamente o nome de exibição para o `View` método com `return View("<ViewName>");`. Em ambos os casos, a exibição descoberta pesquisa um arquivo de exibição correspondente nesta ordem:

   1. *Modos de exibição /\[ControllerName]\[ViewName]. cshtml*
   1. *Exibições/compartilhadas/\[ViewName]. cshtml*

Um caminho de arquivo do modo de exibição pode ser fornecido em vez de um nome de exibição. Se usar um caminho absoluto começando na raiz do aplicativo (se desejar começar com "/" ou "~ /"), o *. cshtml* extensão deve ser especificada:

```csharp
return View("Views/Home/About.cshtml");
```

Você também pode usar um caminho relativo para especificar os modos de exibição em diretórios diferentes sem o *. cshtml* extensão. Dentro de `HomeController`, você pode retornar o *índice* modo de exibição de seu *gerenciar* exibições com um caminho relativo:

```csharp
return View("../Manage/Index");
```

Da mesma forma, você pode indicar o diretório atual do controlador específico com o ". /" prefixo:

```csharp
return View("./About");
```

[Exibições parciais](xref:mvc/views/partial) e [exibir componentes](xref:mvc/views/view-components) usar mecanismos de descoberta semelhantes (mas não idêntica).

Você pode personalizar a convenção padrão para como os modos de exibição estão localizados dentro do aplicativo usando um personalizado [IViewLocationExpander](/aspnet/core/api/microsoft.aspnetcore.mvc.razor.iviewlocationexpander).

Descoberta do modo de exibição depende Localizando exibir arquivos por nome de arquivo. Se o sistema de arquivos subjacente diferencia maiusculas de minúsculas, nomes de exibição são provavelmente diferencia maiusculas de minúsculas. Para compatibilidade em sistemas operacionais, diferenciar maiusculas de minúsculas entre o controlador e ação nomes e as pastas de exibição associada e arquivos. Se você encontrar um erro que não é possível encontrar um arquivo de exibição ao trabalhar com um sistema de arquivos diferencia maiusculas de minúsculas, confirme se o uso de maiusculas e minúsculas corresponde entre o arquivo de modo de exibição solicitado e o nome do arquivo de exibição atual.

Siga a prática recomendada de organizar a estrutura de arquivos nos modos de exibição refletir as relações entre os controladores, ações e modos de exibição para facilidade de manutenção e clareza.

## <a name="passing-data-to-views"></a>Transmitindo dados para modos de exibição

Você pode passar dados para exibições com várias abordagens. A abordagem mais eficiente é especificar um [modelo](xref:mvc/models/model-binding) tipo na exibição. Esse modelo é conhecido como um *viewmodel*. Você pode passar uma instância do tipo viewmodel para o modo de exibição da ação.

Usar um viewmodel para passar dados para um modo de exibição permite que o modo de exibição tirar proveito dos *forte* verificação de tipo. *Tipagem forte* (ou *fortemente tipada*) significa que todas as variáveis e constante tem um tipo definido explicitamente (por exemplo, `string`, `int`, ou `DateTime`). A validade dos tipos usados em uma exibição é verificada em tempo de compilação.

[O Visual Studio](https://www.visualstudio.com/vs/) e [código do Visual Studio](https://code.visualstudio.com/) listam os membros de classe fortemente tipada usando um recurso chamado [IntelliSense](/visualstudio/ide/using-intellisense). Quando você deseja ver as propriedades de um viewmodel, digite o nome da variável para o viewmodel seguido por um período (`.`). Isso ajuda você a escrever código mais rápido com menos erros.

Especifique um modelo usando o `@model` diretiva. Usar o modelo com `@Model`:

```cshtml
@model WebApplication1.ViewModels.Address

<h2>Contact</h2>
<address>
    @Model.Street<br>
    @Model.City, @Model.State @Model.PostalCode<br>
    <abbr title="Phone">P:</abbr> 425.555.0100
</address>
```

Para fornecer o modelo para o modo de exibição, o controlador de passá-lo como um parâmetro:

```csharp
public IActionResult Contact()
{
    ViewData["Message"] = "Your contact page.";

    var viewModel = new Address()
    {
        Name = "Microsoft",
        Street = "One Microsoft Way",
        City = "Redmond",
        State = "WA",
        PostalCode = "98052-6399"
    };

    return View(viewModel);
}
```

Não existem restrições sobre os tipos de modelo que você pode fornecer a uma exibição. É recomendável usar **P**lain **O**ld **C**LR **O**viewmodels bjeto (POCO) com pouca ou nenhuma comportamento (métodos) definido. Geralmente, classes viewmodel são armazenadas no *modelos* pasta ou um separado *ViewModels* pasta na raiz do aplicativo. O *endereço* viewmodel usado no exemplo acima é um viewmodel POCO armazenado em um arquivo chamado *Address.cs*:

```csharp
namespace WebApplication1.ViewModels
{
    public class Address
    {
        public string Name { get; set; }
        public string Street { get; set; }
        public string City { get; set; }
        public string State { get; set; }
        public string PostalCode { get; set; }
    }
}
```

> [!NOTE]
> Nada impede que você use as mesmas classes para seus tipos de viewmodel e seus tipos de modelo de negócios. No entanto, o uso de modelos separados permite que seus modos de exibição variar independentemente da lógica de negócios e dados de partes de acesso do seu aplicativo. Separação de modelos e viewmodels também oferece benefícios de segurança quando os modelos usam [associação de modelo](xref:mvc/models/model-binding) e [validação](xref:mvc/models/validation) para dados enviados para o aplicativo pelo usuário.


<a name="VD_VB"></a>

### <a name="weakly-typed-data-viewdata-and-viewbag"></a>Dados de tipo fraco (ViewData e ViewBag)

Observação: `ViewBag` não está disponível nas páginas do Razor.

Além dos modos de exibição fortemente tipada, modos de exibição têm acesso a um *tipo fraco* (também chamado de *tipadas vagamente*) coleta de dados. Diferentemente dos tipos de alta seguras, *tipos fracos* (ou *perder tipos*) significa que você não declarar explicitamente o tipo de dados que você está usando. Você pode usar a coleta de dados de tipo fraco para a passagem de pequenas quantidades de dados dentro e fora de controladores e exibições.

| Transmitindo dados entre um...                        | Exemplo                                                                        |
| ------------------------------------------------- | ------------------------------------------------------------------------------ |
| Controlador e uma exibição                             | Preencher uma lista suspensa com dados.                                          |
| Modo de exibição e um [exibição de layout](xref:mvc/views/layout)   | Definindo o  **\<título >** conteúdo de elemento na exibição de layout de um arquivo de modo de exibição.  |
| [Exibição parcial](xref:mvc/views/partial) e um modo de exibição | Um widget que exibe dados com base na página da Web que o usuário solicitou.      |

Essa coleção pode ser referenciada por meio de `ViewData` ou `ViewBag` propriedades nos controladores e exibições. O `ViewData` propriedade é um dicionário de objetos do tipo fraco. O `ViewBag` propriedade é um wrapper em torno de `ViewData` que fornece propriedades dinâmicas para subjacente `ViewData` coleção.

`ViewData`e `ViewBag` são resolvidos dinamicamente em tempo de execução. Já que não oferecem a verificação de tipo de tempo de compilação, ambos são geralmente mais propensas a erros que o uso de um viewmodel. Por esse motivo, alguns desenvolvedores preferem usar minimamente ou nunca `ViewData` e `ViewBag`.


<a name="VD"></a>

**ViewData**

`ViewData`é um [ViewDataDictionary](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) acessado por meio do objeto `string` chaves. Dados de cadeia de caracteres podem ser armazenados e usados diretamente, sem a necessidade de uma conversão, mas você deve converter outros `ViewData` valores para tipos específicos de objeto quando você extraí-los. Você pode usar `ViewData` para passar dados de controladores para modos de exibição e em modos de exibição, incluindo [exibições parciais](xref:mvc/views/partial) e [layouts](xref:mvc/views/layout).

A seguir está um exemplo que define valores para uma saudação e um endereço usando `ViewData` em uma ação:

```csharp
public IActionResult SomeAction()
{
    ViewData["Greeting"] = "Hello";
    ViewData["Address"]  = new Address()
    {
        Name = "Steve",
        Street = "123 Main St",
        City = "Hudson",
        State = "OH",
        PostalCode = "44236"
    };

    return View();
}
```

Trabalhar com os dados em um modo de exibição:

```cshtml
@{
    // Since Address isn't a string, it requires a cast.
    var address = ViewData["Address"] as Address;
}

@ViewData["Greeting"] World!

<address>
    @address.Name<br>
    @address.Street<br>
    @address.City, @address.State @address.PostalCode
</address>
```

**ViewBag**

Observação: `ViewBag` não está disponível nas páginas do Razor.

`ViewBag`é um [DynamicViewData](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.internal.dynamicviewdata) objeto que fornece acesso dinâmico para os objetos armazenados em `ViewData`. `ViewBag`pode ser mais conveniente trabalhar com, pois ele não requer conversão. O exemplo a seguir mostra como usar `ViewBag` com o mesmo resultado usando `ViewData` acima:

```csharp
public IActionResult SomeAction()
{
    ViewBag.Greeting = "Hello";
    ViewBag.Address  = new Address()
    {
        Name = "Steve",
        Street = "123 Main St",
        City = "Hudson",
        State = "OH",
        PostalCode = "44236"
    };

    return View();
}
```

```cshtml
@ViewBag.Greeting World!

<address>
    @ViewBag.Address.Name<br>
    @ViewBag.Address.Street<br>
    @ViewBag.Address.City, @ViewBag.Address.State @ViewBag.Address.PostalCode
</address>
```

**Usando ViewData e ViewBag simultaneamente**

Observação: `ViewBag` não está disponível nas páginas do Razor.

Como `ViewData` e `ViewBag` consulte subjacente mesmo `ViewData` coleção, você pode usar ambos `ViewData` e `ViewBag` e combinação de entre elas ao ler e gravar valores.

Definir o título usando `ViewBag` e a descrição usando `ViewData` na parte superior de um *About.cshtml* exibição:

```cshtml
@{
    Layout = "/Views/Shared/_Layout.cshtml";
    ViewBag.Title = "About Contoso";
    ViewData["Description"] = "Let us tell you about Contoso's philosophy and mission.";
}
```

Ler as propriedades, mas reverter o uso de `ViewData` e `ViewBag`. No *cshtml* de arquivos, obtenha o título usando `ViewData` e obter a descrição usando `ViewBag`:

```cshtml
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ViewData["Title"]</title>
    <meta name="description" content="@ViewBag.Description">
    ...
```

Lembre-se de que cadeias de caracteres não exigem uma conversão para `ViewData`. Você pode usar `@ViewData["Title"]` sem conversão.

Usando `ViewData` e `ViewBag` o mesmo Works de tempo, como faz a combinação de ler e gravar as propriedades. A seguinte marcação é processada:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>About Contoso</title>
    <meta name="description" content="Let us tell you about Contoso's philosophy and mission.">
    ...
```

**Resumo das diferenças entre ViewData e ViewBag**

 `ViewBag`não está disponível nas páginas do Razor.

* `ViewData`
  * Deriva [ViewDataDictionary](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary); portanto, tem as propriedades de dicionário que podem ser útil, por exemplo, `ContainsKey`, `Add`, `Remove`, e `Clear`.
  * Chaves no dicionário são cadeias de caracteres, por isso é permitido espaço em branco. Exemplo: `ViewData["Some Key With Whitespace"]`
  * Qualquer tipo diferente de um `string` devem ser convertidos no modo de exibição para usar `ViewData`.
* `ViewBag`
  * Deriva [DynamicViewData](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.internal.dynamicviewdata), que permite a criação de propriedades dinâmicas usando a notação de ponto (`@ViewBag.SomeKey = <value or object>`), e nenhuma conversão é necessária. A sintaxe de `ViewBag` torna mais rápida adicionar controladores e exibições.
  * Mais simples verificar se há valores nulos. Exemplo: `@ViewBag.Person?.Name`

**Quando usar ViewData ou ViewBag**

Ambos `ViewData` e `ViewBag` são igualmente abordagens válidas para transmitir pequenas quantidades de dados entre os controladores e exibições. A escolha de qual delas usar é com base na preferência. Você pode misturar e combinar `ViewData` e `ViewBag` objetos, no entanto, o código é mais fácil de ler e manter com uma abordagem usada de maneira consistente. Ambas as abordagens são resolvidos dinamicamente em tempo de execução e, portanto, propenso a erros de tempo de execução. Algumas equipes de desenvolvimento evitá-los.

### <a name="dynamic-views"></a>Modos de exibição dinâmicos

Modos de exibição que não declare um modelo de tipo usando `@model` , mas que tem uma instância de modelo passada a eles (por exemplo, `return View(Address);`) pode fazer referência a propriedades da instância dinamicamente:

```cshtml
<address>
    @Model.Street<br>
    @Model.City, @Model.State @Model.PostalCode<br>
    <abbr title="Phone">P:</abbr> 425.555.0100
</address>
```

Esse recurso oferece a flexibilidade, mas não oferece proteção de compilação ou o IntelliSense. Se a propriedade não existir, falha na geração de página da Web em tempo de execução.

## <a name="more-view-features"></a>Mais recursos de exibição

[Auxiliares da marca](xref:mvc/views/tag-helpers/intro) mais fácil adicionar o comportamento do lado do servidor para marcas HTML existentes. O uso de auxiliares de marcação evita a necessidade de escrever código personalizado ou auxiliares em suas exibições. Auxiliares de marca são aplicados como atributos para elementos HTML e são ignorados por editores que não podem processá-los. Isso permite que você edite e renderizar a marcação de exibição em uma variedade de ferramentas.

Gera a marcação HTML personalizada pode ser obtido com muitos auxiliares HTML interno. Lógica de interface de usuário mais complexa pode ser tratada por [exibir componentes](xref:mvc/views/view-components). Exibir componentes fornecem o mesmo SoC que controladores e oferecem exibições. Eles podem eliminar a necessidade de ações e modos de exibição que lidam com dados usados por elementos de interface de usuário comuns.

Como muitos outros aspectos do ASP.NET Core, exibições de suporte [injeção de dependência](xref:fundamentals/dependency-injection), permitindo que os serviços estejam [injetadas nos modos de exibição](xref:mvc/views/dependency-injection).

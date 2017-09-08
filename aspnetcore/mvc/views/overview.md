---
title: "Visão geral de modos de exibição"
author: ardalis
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 668c320d-c050-45e3-8161-2f460dc93b2f
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/overview
ms.openlocfilehash: a93ee8165be52e33c2e7da4d3fee2c8225864db9
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/11/2017
---
# <a name="rendering-html-with-views-in-aspnet-core-mvc"></a>Renderização HTML com exibições do MVC do ASP.NET Core

Por [Steve Smith](http://ardalis.com)

Controladores do ASP.NET MVC Core podem retornar resultados formatados usando *exibições*.

## <a name="what-are-views"></a>Quais são os modos de exibição?

O padrão Model-View-Controller (MVC), o *exibição* encapsula os detalhes de apresentação de interação do usuário com o aplicativo. Modos de exibição são modelos HTML com o código inserido que geram conteúdo para enviar ao cliente. Exibições usam [sintaxe Razor](razor.md), que permite que o código interagir com HTML com o mínimo de código ou cerimônia.

Modos de exibição do ASP.NET MVC de núcleo são *. cshtml* arquivos armazenados por padrão em um *exibições* pasta dentro do aplicativo. Normalmente, cada controlador terá sua própria pasta, na qual são modos de exibição para ações do controlador específico.

![Pasta de modos de exibição no Gerenciador de soluções](overview/_static/views_solution_explorer.png)

Além dos modos de exibição de ação específica, [exibições parciais](partial.md), [layouts e outros arquivos de modo de exibição especial](layout.md) pode ser usado para ajudar a reduzir a repetição e permitir a reutilização em modos de exibição do aplicativo.

## <a name="benefits-of-using-views"></a>Benefícios do uso de exibições

Fornecem exibições [separação de preocupações](http://deviq.com/separation-of-concerns/) dentro de um aplicativo MVC, encapsulando a marcação de nível de interface do usuário separadamente de lógica de negócios. ASP.NET MVC exibições usam [sintaxe Razor](razor.md) para alternar entre HTML marcação e o servidor lógica simples. Comuns, repetitivos aspectos da interface de usuário do aplicativo podem ser reutilizados facilmente entre exibições usando [diretivas compartilhadas e layout](layout.md) ou [exibições parciais](partial.md).

## <a name="creating-a-view"></a>Criar um modo de exibição

Exibições que são específicas para um controlador são criadas no *modos de exibição / [ControllerName]* pasta. Modos de exibição que são compartilhados entre os controladores são colocados no */exibições/compartilhado* pasta. Nomeie o arquivo de exibição o mesmo que sua ação de controlador associado e adicione o *. cshtml* extensão de arquivo. Por exemplo, para criar uma exibição para o *sobre* ação o *início* controlador, você deve criar o *About.cshtml* do arquivo no   */exibições/inicial*pasta.

Um exemplo de arquivo de exibição (*About.cshtml*):

[!code-html[Main](../../common/samples/WebApplication1/Views/Home/About.cshtml)]

*Razor* código é indicado pelo `@` símbolo. C# instruções são executadas dentro de blocos de código definir chaves do Razor (`{` `}`), como a atribuição de "Sobre" para o `ViewData["Title"]` elemento mostrado acima. Razor pode ser usado para exibir valores em HTML, simplesmente referenciando o valor com o `@` símbolo, conforme mostrado no `<h2>` e `<h3>` elementos acima.

Essa exibição se concentra na parte da saída para os quais é responsável. O resto do layout da página e outros aspectos comuns da exibição, são especificados em outro lugar. Saiba mais sobre [layout e a lógica de modo de exibição compartilhado](layout.md).

## <a name="how-do-controllers-specify-views"></a>Como fazer controladores especificar modos de exibição?

Modos de exibição normalmente retornados por ações como um `ViewResult`. O método de ação pode criar e retornar um `ViewResult` diretamente, mas mais comumente se herda seu controlador de `Controller`, você simplesmente usará o `View` método auxiliar, como este exemplo demonstra:

*HomeController*

[!code-csharp[Main](../../common/samples/WebApplication1/Controllers/HomeController.cs?highlight=5&range=16-21)]

O `View` método auxiliar tem várias sobrecargas para facilitar o retorno de modos de exibição para os desenvolvedores de aplicativos. Opcionalmente, você pode especificar um modo de exibição para retornar, bem como um objeto de modelo para passar para o modo de exibição.

Quando essa ação retorna, o *About.cshtml* exibição mostrada acima é renderizada:

![Sobre a página](overview/_static/about-page.png)

### <a name="view-discovery"></a>Descoberta do modo de exibição

Quando uma ação retorna uma exibição, um processo chamado *descoberta do modo de exibição* ocorra. Esse processo determina qual arquivo de exibição será usado. A menos que um arquivo de modo de exibição específico for especificado, o tempo de execução procura por um modo de exibição específicos do controlador, em seguida, procura primeiro nome de exibição correspondente no *compartilhado* pasta.

Quando uma ação retorna o `View` método, da seguinte forma `return View();`, o nome da ação é usado como o nome da exibição. Por exemplo, se isso foi chamado de um método de ação denominado "Index", seria equivalente para passar um nome de exibição de "Index". Um nome de exibição pode ser passado explicitamente para o método (`return View("SomeView");`). Em ambos os casos, a descoberta do modo de exibição procura um arquivo de exibição correspondente no:

   1. Modos de exibição /<ControllerName>/<ViewName>. cshtml

   2. Exibições/compartilhadas/<ViewName>. cshtml

>[!TIP]
> É recomendável seguir a convenção de simplesmente retornar `View()` de ações quando possível, já que resulta em mais flexível e mais fácil para refatorar o código.

Um caminho de arquivo do modo de exibição pode ser fornecido, em vez de um nome de exibição. Nesse caso, o *. cshtml* extensão deve ser especificada como parte do caminho do arquivo. O caminho deve ser relativo à raiz do aplicativo (e, opcionalmente, pode começar com "/" ou "~ /"). Por exemplo: `return View("Views/Home/About.cshtml");`

> [!NOTE]
> [Exibições parciais](partial.md) e [exibir componentes](view-components.md) usar mecanismos de descoberta semelhantes (mas não idêntica).

> [!NOTE]
> Você pode personalizar a convenção padrão sobre onde os modos de exibição estão localizados dentro do aplicativo usando um personalizado `IViewLocationExpander`.

>[!TIP]
> Nomes de exibição podem diferenciar maiusculas de minúsculas dependendo do sistema de arquivos subjacente. Para compatibilidade em sistemas operacionais, sempre corresponde caso entre o controlador e nomes de ação e as pastas de exibição associada e nomes de arquivos.

## <a name="passing-data-to-views"></a>Transmitindo dados para modos de exibição

Você pode passar dados para modos de exibição usando vários mecanismos. A abordagem mais eficiente é especificar um *modelo* tipo no modo de exibição (conhecido como um *viewmodel*, para diferenciá-lo dos tipos de modelo de domínio de negócios) e, em seguida, passar uma instância deste tipo para o modo de exibição da ação. Recomendamos que você use um modelo ou um modelo de exibição para passar dados para um modo de exibição. Isso permite que o modo de exibição aproveitar a verificação de tipo forte. Você pode especificar um modelo para um modo de exibição usando o `@model` diretiva:

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "html", "highlight_args": {"hl_lines": [1]}} -->

```html
@model WebApplication1.ViewModels.Address
   <h2>Contact</h2>
   <address>
       @Model.Street<br />
       @Model.City, @Model.State @Model.PostalCode<br />
       <abbr title="Phone">P:</abbr>
       425.555.0100
   </address>
   ```

Depois que um modelo tiver sido especificado para um modo de exibição, a instância enviada para o modo de exibição pode ser acessada em uma maneira fortemente tipado usando `@Model` como mostrado acima. Para fornecer uma instância do tipo de modelo para o modo de exibição, o controlador de passá-lo como um parâmetro:

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "csharp", "highlight_args": {"hl_lines": [13]}} -->

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

Não existem restrições sobre os tipos que podem ser fornecidos para um modo de exibição como um modelo. É recomendável passando simples antigo CLR Object (POCO) Exibir modelos com o comportamento de pouca ou nenhuma lógica de negócios pode ser encapsulada em outro lugar no aplicativo. Um exemplo dessa abordagem é o *endereço* viewmodel usado no exemplo acima:

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "csharp", "highlight_args": {"hl_lines": [13]}} -->

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
> Nada impede que você use as mesmas classes como seus tipos de modelo de negócios e seus tipos de modelo de exibição. No entanto, mantê-los separada permite que seus modos de exibição variar independentemente do seu modelo de domínio ou de persistência e pode oferecer alguns benefícios de segurança (para modelos que os usuários enviará para o aplicativo usando [modelo associação](../models/model-binding.md)).

### <a name="loosely-typed-data"></a>Sem rigidez de tipos de dados

Além dos modos de exibição fortemente tipados, todas as exibições têm acesso a uma coleção sem rigidez de tipos de dados. Essa mesma coleção pode ser referenciada através de `ViewData` ou `ViewBag` propriedades nos controladores e exibições. O `ViewBag` propriedade é um wrapper em torno de `ViewData` que fornece uma exibição dinâmica nessa coleção. Não é uma coleção separada.

`ViewData`um objeto de dicionário que é acessado por meio de `string` chaves. Você pode armazenar e recuperar objetos nele, e você precisará converta-os para um tipo específico, quando você extraí-los. Você pode usar `ViewData` para passar dados de um controlador para modos de exibição, bem como em exibições (exibições parciais e layouts). Dados de cadeia de caracteres podem ser armazenados e usados diretamente, sem a necessidade de uma conversão.

Definir alguns valores para `ViewData` em uma ação:

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

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "html", "highlight_args": {"hl_lines": [3, 6]}} -->

```html
@{
       // Requires cast
       var address = ViewData["Address"] as Address;
   }

   @ViewData["Greeting"] World!

   <address>
       @address.Name<br />
       @address.Street<br />
       @address.City, @address.State @address.PostalCode
   </address>
   ```

O `ViewBag` objetos fornece acesso dinâmico para os objetos armazenados em `ViewData`. Isso pode ser mais conveniente trabalhar com, pois ele não requer conversão. O mesmo exemplo acima, usando `ViewBag` em vez de um fortemente tipada `address` instância no modo de exibição:

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "html", "highlight_args": {"hl_lines": [1, 4, 5, 6]}} -->

```html
@ViewBag.Greeting World!

   <address>
       @ViewBag.Address.Name<br />
       @ViewBag.Address.Street<br />
       @ViewBag.Address.City, @ViewBag.Address.State @ViewBag.Address.PostalCode
   </address>
   ```

> [!NOTE]
> Desde que se referem ao mesmo subjacente `ViewData` coleção, você pode misturar e corresponder entre `ViewData` e `ViewBag` ao ler e gravar valores, se for conveniente.

### <a name="dynamic-views"></a>Modos de exibição dinâmicos

Modos de exibição que não declare um tipo de modelo, mas tem uma instância de modelo passada para elas podem fazer referência a esta instância dinamicamente. Por exemplo, se uma instância do `Address` é passado para uma exibição que não declara um `@model`, a exibição ainda será capaz de fazer referência às propriedades da instância dinamicamente conforme mostrado:

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "html", "highlight_args": {"hl_lines": [13, 16, 17, 18]}} -->

```html
<address>
       @Model.Street<br />
       @Model.City, @Model.State @Model.PostalCode<br />
       <abbr title="Phone">P:</abbr>
       425.555.0100
   </address>
   ```

Esse recurso pode oferecer alguma flexibilidade, mas não oferece nenhuma proteção de compilação ou o IntelliSense. Se a propriedade não existir, a página falhará em tempo de execução.

## <a name="more-view-features"></a>Mais recursos de exibição

[Auxiliares da marca](tag-helpers/intro.md) mais fácil adicionar o comportamento do lado do servidor para marcas HTML existentes, evitando a necessidade de usar código personalizado ou auxiliares em modos de exibição. Os auxiliares de marca são aplicados como atributos para elementos HTML, que são ignorados por editores que não estão familiarizados com eles, permitindo que a marcação de exibição a ser editado e renderizados em uma variedade de ferramentas. Auxiliares de marcação têm muitos usos e, em particular pode tornar [trabalhar com formulários](working-with-forms.md) muito mais fácil.

Gera a marcação HTML personalizada pode ser obtido com muitos auxiliares HTML interno e uma lógica mais complexa da interface do usuário (possivelmente com seus próprios requisitos de dados) pode ser encapsulada em [exibir componentes](view-components.md). Exibir componentes fornecem a mesma separação de preocupações que oferecem controladores e exibições e podem eliminar a necessidade de ações e modos de exibição para lidar com dados usados por elementos de interface de usuário comuns.

Como muitos outros aspectos do ASP.NET Core, exibições de suporte [injeção de dependência](../../fundamentals/dependency-injection.md), permitindo que os serviços estejam [injetadas nos modos de exibição](dependency-injection.md).

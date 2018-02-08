---
title: "Exibições parciais"
author: ardalis
description: "Usando exibições parciais no ASP.NET Core MVC"
manager: wpickett
ms.author: riande
ms.date: 03/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/partial
ms.openlocfilehash: 169948e5d7dc8068463ed61114666148b785b217
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/30/2018
---
# <a name="partial-views"></a>Exibições parciais

Por [Steve Smith](https://ardalis.com/), [Maher JENDOUBI](https://twitter.com/maherjend) e [Rick Anderson](https://twitter.com/RickAndMSFT)

O ASP.NET Core MVC dá suporte a exibições parciais, que são úteis quando você tem partes reutilizáveis de páginas da Web que deseja compartilhar entre diferentes exibições.

[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/partial/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-are-partial-views"></a>O que são Exibições Parciais?

Uma exibição parcial é uma exibição renderizada dentro de outra exibição. A saída HTML gerada pela execução da exibição parcial é renderizada na exibição de chamada (ou pai). Assim como exibições, as exibições parciais usam a extensão de arquivo *.cshtml*.

## <a name="when-should-i-use-partial-views"></a>Quando é necessário usar Exibições Parciais?

As exibições parciais são uma maneira eficiente de dividir exibições grandes em componentes menores. Elas podem reduzir a duplicação do conteúdo de exibição e permitir que os elementos de exibição sejam reutilizados. Os elementos de layout comuns devem ser especificados em [_Layout.cshtml](layout.md). O conteúdo reutilizável que não pertence ao layout pode ser encapsulado em exibições parciais.

Se você tem uma página complexa composta por várias partes lógicas, pode ser útil trabalhar com cada parte como sua própria exibição parcial. Cada parte da página pode ser exibida em isolamento do restante da página e a exibição para a página propriamente dita fica muito mais simples, porque contém apenas a estrutura da página geral e chamadas para renderizar as exibições parciais.

Dica: siga o [Princípio Don't Repeat Yourself](http://deviq.com/don-t-repeat-yourself/) nas exibições.

## <a name="declaring-partial-views"></a>Declarando exibições parciais

As exibições parciais são criadas como qualquer outra exibição: crie um arquivo *.cshtml* dentro da pasta *Views* . Não há nenhuma diferença semântica entre uma exibição parcial e uma exibição normal – apenas que são renderizadas de maneira diferente. Você pode ter uma exibição que é retornada diretamente do `ViewResult` de um controlador e a mesma exibição pode ser usada como uma exibição parcial. A principal diferença entre como uma exibição e uma exibição parcial é renderizada é que as exibições parciais não executam *_ViewStart.cshtml* (ao contrário das exibições – saiba mais sobre *_ViewStart.cshtml* em [Layout](layout.md)).

## <a name="referencing-a-partial-view"></a>Referenciando uma exibição parcial

Em uma página de exibição, há várias maneiras de renderizar uma exibição parcial. É mais simples usar `Html.Partial`, que retorna uma `IHtmlString` e pode ser referenciada com a colocação de prefixo na chamada com `@`:

[!code-html[Main](partial/sample/src/PartialViewsSample/Views/Home/About.cshtml?range=9)]

O método `PartialAsync` está disponível para exibições parciais que contêm um código assíncrono (embora, em geral, não seja recomendado que as exibições contenham um código):

[!code-html[Main](partial/sample/src/PartialViewsSample/Views/Home/About.cshtml?range=8)]

Renderize uma exibição parcial com `RenderPartial`. Esse método não retorna um resultado; ele transmite a saída renderizada diretamente para a resposta. Como ele não retorna um resultado, ele precisa ser chamado dentro de um bloco de código Razor (também chame `RenderPartialAsync`, se necessário):

[!code-html[Main](partial/sample/src/PartialViewsSample/Views/Home/About.cshtml?range=10-12)]

Já que ele transmite o resultado diretamente, `RenderPartial` e `RenderPartialAsync` podem ter um melhor desempenho em alguns cenários. No entanto, na maioria dos casos, é recomendável o uso de `Partial` e `PartialAsync`.

> [!NOTE]
> Se as exibições precisam executar o código, o padrão recomendado é usar um [componente de exibição](view-components.md), em vez de uma exibição parcial.

### <a name="partial-view-discovery"></a>Descoberta de exibição parcial

Ao referenciar uma exibição parcial, você pode se referir ao seu local de várias maneiras:

```text
// Uses a view in current folder with this name
// If none is found, searches the Shared folder
@Html.Partial("ViewName")

// A view with this name must be in the same folder
@Html.Partial("ViewName.cshtml")

// Locate the view based on the application root
// Paths that start with "/" or "~/" refer to the application root
@Html.Partial("~/Views/Folder/ViewName.cshtml")
@Html.Partial("/Views/Folder/ViewName.cshtml")

// Locate the view using relative paths
@Html.Partial("../Account/LoginPartial.cshtml")
```

Você pode ter diferentes exibições parciais com o mesmo nome em pastas de exibição diferentes. Ao referenciar as exibições por nome (sem a extensão de arquivo), as exibições em cada pasta usarão a exibição parcial na mesma pasta com elas. Também especifique uma exibição parcial padrão a ser usada, colocando-a na pasta *Shared*. A exibição parcial compartilhada será usada pelas exibições que não têm sua própria versão da exibição parcial. Você pode ter uma exibição parcial padrão (em *Shared*), que é substituída por uma exibição parcial com o mesmo nome na mesma pasta da exibição pai.

As exibições parciais podem ser *encadeadas*. Ou seja, uma exibição parcial pode chamar outra exibição parcial (desde que você não crie um loop). Dentro de cada exibição ou exibição parcial, os caminhos relativos são sempre relativos a essa exibição, não à exibição raiz ou pai.

> [!NOTE]
> Se você declarar uma `section` do [Razor](razor.md) em uma exibição parcial, ela não será visível para seus pais; será limitada à exibição parcial.

## <a name="accessing-data-from-partial-views"></a>Acessando dados em exibições parciais

Quando é criada uma instância de uma exibição parcial, ela recebe uma cópia do dicionário `ViewData` da exibição pai. As atualizações feitas nos dados dentro da exibição parcial não são persistidas na exibição pai. Os `ViewData` alterados em um parcial exibição são perdidos quando a exibição parcial é retornada.

Passe uma instância de `ViewDataDictionary` para a exibição parcial:

```csharp
@Html.Partial("PartialName", customViewData)
   ```

Também passe um modelo para uma exibição parcial. Isso pode ser o modelo de exibição da página, ou uma parte dele, ou um objeto personalizado. Passe um modelo para `Partial`, `PartialAsync`, `RenderPartial` ou `RenderPartialAsync`:

```csharp
@Html.Partial("PartialName", viewModel)
   ```

Passe uma instância de `ViewDataDictionary` e um modelo de exibição para uma exibição parcial:

[!code-html[Main](partial/sample/src/PartialViewsSample/Views/Articles/Read.cshtml?range=15-16)]

A marcação abaixo mostra a exibição *Views/Articles/Read.cshtml* que contém duas exibições parciais. A segunda exibição parcial passa um modelo e `ViewData` para a exibição parcial. Passe um novo dicionário `ViewData` mantendo os `ViewData` existentes se você usar a sobrecarga de construtor do `ViewDataDictionary` realçado abaixo:

[!code-html[Main](partial/sample/src/PartialViewsSample/Views/Articles/Read.cshtml)]

*Views/Shared/AuthorPartial*:

[!code-html[Main](partial/sample/src/PartialViewsSample/Views/Shared/AuthorPartial.cshtml)]

A parcial *ArticleSection*:

[!code-html[Main](partial/sample/src/PartialViewsSample/Views/Articles/ArticleSection.cshtml)]

Em tempo de execução, as parciais são renderizadas para a exibição pai, que, por sua vez, é renderizada na saída da exibição parcial *_Layout.cshtml*

![compartilhada](partial/_static/output.png)

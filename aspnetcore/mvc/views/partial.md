---
title: "Exibições parciais"
author: ardalis
description: "Usando exibições parciais no ASP.NET MVC de núcleo"
keywords: "Parcial do ASP.NET Core, exibições parciais,"
ms.author: riande
manager: wpickett
ms.date: 03/14/2017
ms.topic: article
ms.assetid: 4be1b12c-b74e-44ff-826b-99ce86e8d464
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/partial
ms.openlocfilehash: 60f5255ca31accbffffec18053b29810977a5ff1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
# <a name="partial-views"></a>Exibições parciais

Por [Steve Smith](https://ardalis.com/), [Maher JENDOUBI](https://twitter.com/maherjend), e [Rick Anderson](https://twitter.com/RickAndMSFT)

Núcleo do ASP.NET MVC oferece suporte a modos de exibição parciais, que são úteis quando você tem componentes reutilizáveis de páginas da web que você deseja compartilhar entre diferentes modos de exibição.

[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/partial/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-are-partial-views"></a>Quais são os modos de exibição parcial?

Uma exibição parcial é uma exibição que é renderizada dentro de outro modo de exibição. A saída HTML gerada por executar a exibição parcial é renderizada no modo de exibição de chamada (ou pai). Como exibições, exibições parciais usam o *. cshtml* extensão de arquivo.

## <a name="when-should-i-use-partial-views"></a>Quando devo usar exibições parciais?

Exibições parciais são uma maneira eficiente de separação de modos de exibição grandes em componentes menores. Podem reduzir a duplicação de exibir conteúdo e permitir que os elementos de exibição ser reutilizado. Elementos de layout comuns devem ser especificados no [cshtml](layout.md). Conteúdo reutilizável de layout não pode ser encapsulado em modos de exibição parciais.

Se você tiver uma página complexa composta por várias partes lógicas, ele pode ser útil trabalhar com cada parte como seu próprio modo de exibição parcial. Cada parte da página pode ser exibido no isolamento do restante da página e o modo de exibição para a página propriamente dita se torna muito mais simples porque ele contém apenas a estrutura da página geral e a chamadas para renderizar os modos de exibição parciais.

Dica: Execute o [não repita por conta própria princípio](http://deviq.com/don-t-repeat-yourself/) em exibições.

## <a name="declaring-partial-views"></a>Declarando exibições parciais

Exibições parciais são criadas como qualquer outro modo de exibição: criar um *. cshtml* dentro do arquivo de *exibições* pasta. Não há nenhuma diferença semântica entre uma exibição parcial em uma exibição normal - apenas são renderizados diferente. Você pode ter uma exibição que é retornada diretamente de um controlador `ViewResult`, e o mesmo modo de exibição pode ser usado como uma exibição parcial. A principal diferença entre como um modo de exibição e uma exibição parcial são renderizados é exibições parciais não executem *viewstart* (enquanto exibições - Saiba mais sobre *viewstart* na [Layout ](layout.md)).

## <a name="referencing-a-partial-view"></a>Fazendo referência a uma exibição parcial

De dentro de uma página de exibição, há várias maneiras em que você pode renderizar uma exibição parcial. É mais simples usar `Html.Partial`, que retorna um `IHtmlString` e pode ser referenciado, prefixando a chamada com `@`:

[!code-html[Main](partial/sample/src/PartialViewsSample/Views/Home/About.cshtml?range=9)]

O `PartialAsync` método está disponível para parcial exibições contendo código assíncrono (embora o código nos modos de exibição não é geralmente recomendado):

[!code-html[Main](partial/sample/src/PartialViewsSample/Views/Home/About.cshtml?range=8)]

Você pode renderizar uma exibição parcial com `RenderPartial`. Esse método não retorna um resultado; Ela transmite a saída renderizada diretamente para a resposta. Porque ele não retorna um resultado, ele deve ser chamado dentro de um bloco de código Razor (você também pode chamar `RenderPartialAsync` se necessário):

[!code-html[Main](partial/sample/src/PartialViewsSample/Views/Home/About.cshtml?range=10-12)]

Porque ele transmite o resultado diretamente, `RenderPartial` e `RenderPartialAsync` podem executar melhor em alguns cenários. No entanto, na maioria dos casos, recomenda-se você usar `Partial` e `PartialAsync`.

> [!NOTE]
> Se precisam de seus modos de exibição executar o código, o padrão recomendado é usar um [componentes da exibição](view-components.md) em vez de uma exibição parcial.

### <a name="partial-view-discovery"></a>Descoberta do modo de exibição parcial

Ao fazer referência a uma exibição parcial, você pode consultar para seu local de várias maneiras:

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

Você pode ter diferentes modos de exibição parciais com o mesmo nome em pastas de exibição diferente. Ao fazer referência as exibições por nome (sem a extensão de arquivo), modos de exibição em cada pasta usará o modo de exibição parcial na mesma pasta com eles. Você também pode especificar um modo de exibição parcial padrão a ser usado, colocá-lo no *compartilhado* pasta. O modo de exibição parcial compartilhado será usado por todas as exibições que não têm sua própria versão do modo de exibição parcial. Você pode ter uma exibição parcial padrão (em *compartilhado*), que é substituído por uma exibição parcial com o mesmo nome na mesma pasta que o modo de exibição do pai.

Exibições parciais podem ser *encadeados*. Isto é, uma exibição parcial pode chamar outro modo de exibição parcial (desde que você não criar um loop). Dentro de cada exibição ou uma exibição parcial, os caminhos relativos são sempre em relação a esse modo de exibição do modo de exibição, e não a raiz ou pai.

> [!NOTE]
> Se você declarar um [Razor](razor.md) `section` em uma exibição parcial, não será visível para seus pais; ele será limitado a exibição parcial.

## <a name="accessing-data-from-partial-views"></a>Acessando dados em exibições parciais

Quando uma exibição parcial é instanciada, ela recebe uma cópia do modo de exibição pai `ViewData` dicionário. Atualizações feitas nos dados no modo de exibição parcial não são mantidas para a exibição pai. `ViewData`alterado em um parcial exibição é perdida quando a exibição parcial retorna.

Você pode passar uma instância de `ViewDataDictionary` para a exibição parcial:

```csharp
@Html.Partial("PartialName", customViewData)
   ```

Você também pode passar um modelo em uma exibição parcial. Isso pode ser o modelo de exibição da página, ou uma parte dele ou um objeto personalizado. Você pode passar um modelo para `Partial`,`PartialAsync`, `RenderPartial`, ou `RenderPartialAsync`:

```csharp
@Html.Partial("PartialName", viewModel)
   ```

Você pode passar uma instância de `ViewDataDictionary` e um modelo de exibição para uma exibição parcial:

[!code-html[Main](partial/sample/src/PartialViewsSample/Views/Articles/Read.cshtml?range=15-16)]

A marcação abaixo mostra o *Views/Articles/Read.cshtml* exibição que contém dois modos de exibição parciais. O segundo modo de exibição parcial passa em um modelo e `ViewData` para a exibição parcial. Você pode passar novos `ViewData` dicionário mantendo existente `ViewData` se você usar a sobrecarga de construtor do `ViewDataDictionary` destacado abaixo:

[!code-html[Main](partial/sample/src/PartialViewsSample/Views/Articles/Read.cshtml)]

*Exibições/compartilhadas/AuthorPartial*:

[!code-html[Main](partial/sample/src/PartialViewsSample/Views/Shared/AuthorPartial.cshtml)]

O *ArticleSection* parcial:

[!code-html[Main](partial/sample/src/PartialViewsSample/Views/Articles/ArticleSection.cshtml)]

Em tempo de execução, existe o meio-termo é renderizado no modo de exibição pai, que por si só é renderizado dentro compartilhado *layout. cshtml*

![saída de exibição parcial](partial/_static/output.png)

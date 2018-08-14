---
title: Exibições parciais no ASP.NET Core
author: ardalis
description: Saiba por que uma exibição parcial é uma exibição renderizada dentro de outra exibição e quando elas devem ser usadas em aplicativos ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 07/06/2018
uid: mvc/views/partial
ms.openlocfilehash: 2223f3c6e42927def4b91ff9da58c228e5904756
ms.sourcegitcommit: 028ad28c546de706ace98066c76774de33e4ad20
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/08/2018
ms.locfileid: "39655317"
---
# <a name="partial-views-in-aspnet-core"></a>Exibições parciais no ASP.NET Core

Por [Steve Smith](https://ardalis.com/), [Maher JENDOUBI](https://twitter.com/maherjend), [Rick Anderson](https://twitter.com/RickAndMSFT) e [Scott Sauber](https://twitter.com/scottsauber)

O ASP.NET Core oferece suporte a exibições parciais. As exibições parciais são usadas para compartilhar partes reutilizáveis ​​de páginas da Web em diferentes exibições.

[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/partial/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-are-partial-views"></a>O que são exibições parciais

Uma exibição parcial é uma exibição renderizada dentro de outra exibição. A saída HTML gerada pela execução da exibição parcial é renderizada na exibição de chamada (ou pai). Assim como exibições, as exibições parciais usam a extensão de arquivo *.cshtml*.

Por exemplo, o modelo de projeto do **aplicativo Web** ASP.NET Core 2.1 inclui uma exibição parcial *_CookieConsentPartial.cshtml*. A exibição parcial é carregada de dentro de *_Layout.cshtml*:

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Shared/_Layout.cshtml?name=snippet_CookieConsentPartial)]

## <a name="when-to-use-partial-views"></a>Quando usar exibições parciais

As exibições parciais são uma maneira eficiente de dividir exibições grandes em componentes menores. Elas podem reduzir a duplicação do conteúdo de exibição e permitir que os elementos de exibição sejam reutilizados. Os elementos de layout comuns devem ser especificados em [_Layout.cshtml](xref:mvc/views/layout). O conteúdo reutilizável que não pertence ao layout pode ser encapsulado em exibições parciais.

Em uma página complexa composta por várias partes lógicas, é útil trabalhar com cada parte como sua própria exibição parcial. Cada parte da página pode ser exibida isoladamente do restante da página. A exibição para a página propriamente dita se torna mais simples, já que ele contém apenas a estrutura geral da página e chamadas para renderizar as exibições parciais.

Os controladores ASP.NET Core MVC têm um método [PartialView](/dotnet/api/microsoft.aspnetcore.mvc.controller.partialview#Microsoft_AspNetCore_Mvc_Controller_PartialView) que é chamado de um método de ação. As Razor Pages não possuem um método `PartialView` equivalente no [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel).

## <a name="declare-partial-views"></a>Declarar exibições parciais

As exibições parciais são criadas como uma exibição comum&mdash;criando-se um arquivo *.cshtml* dentro da pasta *Views*. Não há nenhuma diferença semântica entre uma exibição parcial e uma exibição normal, no entanto, elas são renderizadas de maneira diferente. Você pode ter uma exibição que é retornada diretamente do [ViewResult](/dotnet/api/microsoft.aspnetcore.mvc.viewresult) de um controlador e a mesma exibição pode ser usada como uma exibição parcial. A principal diferença entre como uma exibição e uma exibição parcial é renderizada é que as exibições parciais não executam *_ViewStart.cshtml*. As exibições normais executam *_ViewStart.cshtml*. Saiba mais sobre *_ViewStart.cshtml* em [Layout](xref:mvc/views/layout)).

Como uma convenção, nomes de arquivo de exibição parcial geralmente começam com `_`. Essa convenção de nomenclatura não é um requisito, mas ajuda a diferenciar visualmente as exibições parciais de exibições normais.

## <a name="reference-a-partial-view"></a>Referenciar uma exibição parcial

Em uma página de exibição, há várias maneiras de renderizar uma exibição parcial. A prática recomendada é usar a renderização assíncrona.

::: moniker range=">= aspnetcore-2.1"

### <a name="partial-tag-helper"></a>Auxiliar de marca parcial

O auxiliar de marca parcial exige o ASP.NET Core 2.1 ou posterior. Ele renderiza de forma assíncrona e usa uma sintaxe semelhante ao HTML:

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Home/Discovery.cshtml?name=snippet_PartialTagHelper)]

Para obter mais informações, consulte <xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper>.

::: moniker-end

### <a name="asynchronous-html-helper"></a>Auxiliar HTML assíncrono

Ao usar um auxiliar HTML, a prática recomendada é usar [PartialAsync](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.partialasync#Microsoft_AspNetCore_Mvc_Rendering_HtmlHelperPartialExtensions_PartialAsync_Microsoft_AspNetCore_Mvc_Rendering_IHtmlHelper_System_String_). Ele retorna um tipo [IHtmlContent](/dotnet/api/microsoft.aspnetcore.html.ihtmlcontent) encapsulado em um `Task`. O método é referenciado prefixando a chamada com `@`:

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Home/Discovery.cshtml?name=snippet_PartialAsync)]

Como alternativa, você pode renderizar uma exibição parcial com [RenderPartialAsync](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.renderpartialasync). Esse método não retorna uma consulta. Ele transmite a saída renderizada diretamente para a resposta. Como o método não retorna nenhum resultado, ele precisa ser chamado dentro de um bloco de código Razor:

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Home/Discovery.cshtml?name=snippet_RenderPartialAsync)]

Já que ele transmite o resultado diretamente, o `RenderPartialAsync` pode ter um desempenho melhor em alguns cenários. No entanto, é recomendável usar o `PartialAsync`.

### <a name="synchronous-html-helper"></a>Auxiliar de HTML assíncrono

[Partial](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.partial) e [RenderPartial](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.renderpartial) são os equivalentes síncronos de `PartialAsync` e `RenderPartialAsync`, respectivamente. O uso de equivalentes síncronos não é recomendado porque há cenários em que eles realizam deadlock. Versões futuras não contêm os métodos síncronos.

> [!IMPORTANT]
> Se as exibições precisam executar código use um [componente de exibição](xref:mvc/views/view-components), em vez de uma exibição parcial.

::: moniker range=">= aspnetcore-2.1"

No ASP.NET Core 2.1 ou posterior, chamar `Partial` ou `RenderPartial` resulta em um aviso de analisador. Por exemplo, o uso de `Partial` produz a seguinte mensagem de aviso:

> Uso de IHtmlHelper.Partial pode resultar em deadlocks de aplicativo. Considere a possibilidade de usar o auxiliar de marca `<partial>` ou `IHtmlHelper.PartialAsync`.

Substitua as chamadas para `@Html.Partial` com `@await Html.PartialAsync` ou o auxiliar de marca parcial. Para obter mais informações sobre a migração do auxiliar de marca parcial, consulte [Migrar de um auxiliar HTML](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper#migrate-from-an-html-helper).

::: moniker-end

## <a name="partial-view-discovery"></a>Descoberta de exibição parcial

Ao referenciar uma exibição parcial, você pode se referir ao seu local de várias maneiras. Por exemplo:

::: moniker range=">= aspnetcore-2.1"

```cshtml
// Uses a view in current folder with this name.
// If none is found, searches the Shared folder.
<partial name="_ViewName" />

// A view with this name must be in the same folder
<partial name="_ViewName.cshtml" />

// Locate the view based on the app root.
// Paths that start with "/" or "~/" refer to the app root.
<partial name="~/Views/Folder/_ViewName.cshtml" />
<partial name="/Views/Folder/_ViewName.cshtml" />

// Locate the view using a relative path
<partial name="../Account/_LoginPartial.cshtml" />
```

O exemplo anterior usa o auxiliar de marca parcial, o que exige o ASP.NET Core 2.1 ou posterior. O exemplo a seguir usa auxiliares HTML assíncronos para realizar a mesma tarefa.

::: moniker-end

```cshtml
// Uses a view in current folder with this name.
// If none is found, searches the Shared folder.
@await Html.PartialAsync("_ViewName")

// A view with this name must be in the same folder
@await Html.PartialAsync("_ViewName.cshtml")

// Locate the view based on the app root.
// Paths that start with "/" or "~/" refer to the app root.
@await Html.PartialAsync("~/Views/Folder/_ViewName.cshtml")
@await Html.PartialAsync("/Views/Folder/_ViewName.cshtml")

// Locate the view using a relative path
@await Html.PartialAsync("../Account/_LoginPartial.cshtml")
```

Você pode ter diferentes exibições parciais com o mesmo nome do arquivo em pastas de exibição diferentes. Ao referenciar as exibições por nome (sem uma extensão de arquivo), as exibições em cada pasta usam a exibição parcial na mesma pasta com elas. Também especifique uma exibição parcial padrão a ser usada, colocando-a na pasta *Shared*. A exibição parcial compartilhada é usada pelas exibições que não têm sua própria versão da exibição parcial. Você pode ter uma exibição parcial padrão (em *Shared*), que é substituída por uma exibição parcial com o mesmo nome na mesma pasta da exibição pai.

Exibições parciais podem ser *encadeadas*&mdash;uma exibição parcial pode chamar outra exibição parcial (desde que você não crie um loop). Dentro de cada exibição ou exibição parcial, os caminhos relativos são sempre relativos a essa exibição, não à exibição raiz ou pai.

> [!NOTE]
> Uma `section` do [Razor](xref:mvc/views/razor) definida em uma exibição parcial é invisível para exibições pai. A `section` só é visível para a exibição parcial na qual ela está definida.

## <a name="access-data-from-partial-views"></a>Acessar dados de exibições parciais

Quando é criada uma instância de uma exibição parcial, ela recebe uma cópia do dicionário `ViewData` da exibição pai. As atualizações feitas nos dados dentro da exibição parcial não são persistidas na exibição pai. Alterações a `ViewData` em uma exibição parcial são perdidas quando a exibição parcial retorna.

Você pode passar uma instância de [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) para a exibição parcial:

```cshtml
@await Html.PartialAsync("_PartialName", customViewData)
```

Você pode passar um modelo para uma exibição parcial. O modelo pode ser o modelo de exibição da página ou um objeto personalizado. Você pode passar um modelo para `PartialAsync` ou `RenderPartialAsync`:

```cshtml
@await Html.PartialAsync("_PartialName", viewModel)
```

Passe uma instância de `ViewDataDictionary` e um modelo de exibição para uma exibição parcial:

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Articles/Read.cshtml?name=snippet_PartialAsync)]

A marcação a seguir mostra a exibição *Views/Articles/Read.cshtml*, que contém duas exibições parciais. A segunda exibição parcial passa um modelo e `ViewData` para a exibição parcial. Use a sobrecarga de construtor `ViewDataDictionary` destacada para passar um novo dicionário `ViewData`, retendo ainda o dicionário `ViewData` existente.

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Articles/Read.cshtml?name=snippet_ReadPartialView&highlight=17-20)]

*Views/Shared/_AuthorPartial*:

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Shared/_AuthorPartial.cshtml)]

A parcial *_ArticleSection*:

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Articles/_ArticleSection.cshtml)]

Em tempo de execução, as parciais são renderizadas para a exibição pai, que, por sua vez, é renderizada na saída da exibição parcial *_Layout.cshtml*.

![compartilhada](partial/_static/output.png)

## <a name="additional-resources"></a>Recursos adicionais

::: moniker range=">= aspnetcore-2.1"

* <xref:mvc/views/razor>
* <xref:mvc/views/tag-helpers/intro>
* <xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper>
* <xref:mvc/views/view-components>

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

* <xref:mvc/views/razor>
* <xref:mvc/views/view-components>

::: moniker-end

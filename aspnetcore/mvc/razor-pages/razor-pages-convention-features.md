---
title: Recursos de convenção de rota e aplicativo das Páginas do Razor no ASP.NET Core
author: guardrex
description: Descubra como os recursos de convenção do provedor de modelo de aplicativo e rota ajudam você a controlar o roteamento, a descoberta e o processamento de página.
manager: wpickett
ms.author: riande
ms.date: 10/23/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/razor-pages/razor-pages-convention-features
ms.openlocfilehash: bf1c895fc972310d5541d0098226d58b8183e320
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/30/2018
---
# <a name="razor-pages-route-and-app-convention-features-in-aspnet-core"></a>Recursos de convenção de rota e aplicativo das Páginas do Razor no ASP.NET Core

Por [Luke Latham](https://github.com/guardrex)

Saiba como usar os recursos de convenção do provedor de modelo de aplicativo e rota de página para controlar o roteamento, a descoberta e o processamento de página nos aplicativos das Páginas do Razor. Quando precisar configurar rotas de página personalizadas para páginas individuais, configure o roteamento para páginas com a [convenção AddPageRoute](#configure-a-page-route) descrita mais adiante neste tópico.

Use o [aplicativo de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/razor-pages-convention-features/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample)) para explorar os recursos descritos neste tópico.

| Recursos | A amostra explica... |
| -------- | --------------------------- |
| [Convenções de modelo de rota e aplicativo](#add-route-and-app-model-conventions)<br><br>Conventions.Add<ul><li>IPageRouteModelConvention</li><li>IPageApplicationModelConvention</li></ul> | Como adicionar um modelo de rota e cabeçalho às páginas de um aplicativo. |
| [Convenções de ação da rota de página](#page-route-action-conventions)<ul><li>AddFolderRouteModelConvention</li><li>AddPageRouteModelConvention</li><li>AddPageRoute</li></ul> | Como adicionar um modelo de rota às páginas de uma pasta e a uma única página. |
| [Convenções de ação do modelo de página](#page-model-action-conventions)<ul><li>AddFolderApplicationModelConvention</li><li>AddPageApplicationModelConvention</li><li>ConfigureFilter (classe de filtro, expressão lambda ou alocador de filtro)</li></ul> | Adicionar um cabeçalho às páginas de uma pasta, adicionar um cabeçalho a uma única página e configurar um [alocador de filtro](xref:mvc/controllers/filters#ifilterfactory) para adicionar um cabeçalho às páginas de um aplicativo. |
| [Provedor de modelo de aplicativo de página padrão](#replace-the-default-page-app-model-provider) | Como substituir o provedor de modelo de página padrão para alterar as convenções de nomenclatura do manipulador. |

## <a name="add-route-and-app-model-conventions"></a>Adicionar convenções de modelo de rota e aplicativo

Adicione um delegado a [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) para adicionar convenções de modelo de rota e aplicativo que se aplicam às Páginas do Razor.

**Adicionar uma convenção de modelo de rota a todas as páginas**

Use [Conventions](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions) para criar e adicionar uma [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention) à coleção de instâncias [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) que são aplicadas durante a construção do modelo de rota e de página.

O aplicativo de exemplo adiciona um modelo de rota `{globalTemplate?}` a todas as páginas no aplicativo:

[!code-csharp[Main](razor-pages-convention-features/sample/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

> [!NOTE]
> A propriedade `Order` para o `AttributeRouteModel` é definido como `0` (zero). Isso garante que esse modelo tenha prioridade para a primeira posição de valor de dados de rota quando um único valor de rota é fornecido. Por exemplo, a amostra adiciona um modelo de rota `{aboutTemplate?}` mais adiante no tópico. O modelo `{aboutTemplate?}` recebe uma `Order` de `1`. Quando a página About é solicitada no `/About/RouteDataValue`, "RouteDataValue" é carregado no `RouteData.Values["globalTemplate"]` (`Order = 0`) e não `RouteData.Values["aboutTemplate"]` (`Order = 1`) devido à configuração da propriedade `Order`.

*Startup.cs*:

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet1)]

Solicite a página About da amostra em `localhost:5000/About/GlobalRouteValue` e inspecione o resultado:

![A página About é solicitada com um segmento de rota GlobalRouteValue. A página renderizada mostra que o valor de dados de rota é capturado no método OnGet da página.](razor-pages-convention-features/_static/about-page-global-template.png)

**Adicionar uma convenção de modelo de aplicativo a todas as páginas**

Use [Conventions](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions) para criar e adicionar uma [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention) à coleção de instâncias [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) que são aplicadas durante a construção do modelo de rota e de página.

Para demonstrar isso e outras convenções mais adiante no tópico, o aplicativo de exemplo inclui uma classe `AddHeaderAttribute`. O construtor de classe aceita uma cadeia de caracteres `name` e uma matriz de cadeia de caracteres `values`. Esses valores são usados em seu método `OnResultExecuting` para definir um cabeçalho de resposta. A classe completa é mostrada na seção [Convenções de ação do modelo de página](#page-model-action-conventions) mais adiante no tópico.

O aplicativo de exemplo usa a classe `AddHeaderAttribute` para adicionar um cabeçalho, `GlobalHeader`, a todas as páginas no aplicativo:

[!code-csharp[Main](razor-pages-convention-features/sample/Conventions/GlobalHeaderPageApplicationModelConvention.cs?name=snippet1)]

*Startup.cs*:

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet2)]

Solicite a página About da amostra em `localhost:5000/About` e inspecione os cabeçalhos para exibir o resultado:

![Os cabeçalhos de resposta da página About mostram que o GlobalHeader foi adicionado.](razor-pages-convention-features/_static/about-page-global-header.png)

## <a name="page-route-action-conventions"></a>Convenções de ação da rota de página

O provedor de modelo de rota padrão derivado de [IPageRouteModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelprovider) invoca convenções que foram projetadas para fornecer pontos de extensibilidade para configuração de rotas de página.

**Convenção de modelo de rota de pasta**

Use [AddFolderRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addfolderroutemodelconvention) para criar e adicionar uma [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention) que invoca uma ação no [PageRouteModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageroutemodel) para todas as páginas da pasta especificada.

O aplicativo de exemplo usa `AddFolderRouteModelConvention` para adicionar um modelo de rota `{otherPagesTemplate?}` às páginas da pasta *OtherPages*:

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet3)]

> [!NOTE]
> A propriedade `Order` do `AttributeRouteModel` é definida como `1`. Isso garante que o modelo para `{globalTemplate?}` (definido anteriormente no tópico) tenha prioridade para a primeira posição de valor de dados de rota quando um único valor de rota é fornecido. Se a página Page1 é solicitada no `/OtherPages/Page1/RouteDataValue`, "RouteDataValue" é carregado no `RouteData.Values["globalTemplate"]` (`Order = 0`) e não `RouteData.Values["otherPagesTemplate"]` (`Order = 1`) devido à configuração da propriedade `Order`.

Solicite a página Page1 da amostra em `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` e inspecione o resultado:

![A Page1 da pasta OtherPages é solicitada com um segmento de rota GlobalRouteValue e OtherPagesRouteValue. A página renderizada mostra que os valores de dados de rota são capturados no método OnGet da página.](razor-pages-convention-features/_static/otherpages-page1-global-and-otherpages-templates.png)

**Convenção de modelo de rota de página**

Use [AddPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addpageroutemodelconvention) para criar e adicionar uma [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention) que invoca uma ação no [PageRouteModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageroutemodel) para a página com o nome especificado.

O aplicativo de exemplo usa `AddPageRouteModelConvention` para adicionar um modelo de rota `{aboutTemplate?}` à página About:

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet4)]

> [!NOTE]
> A propriedade `Order` do `AttributeRouteModel` é definida como `1`. Isso garante que o modelo para `{globalTemplate?}` (definido anteriormente no tópico) tenha prioridade para a primeira posição de valor de dados de rota quando um único valor de rota é fornecido. Se a página About é solicitada no `/About/RouteDataValue`, "RouteDataValue" é carregado no `RouteData.Values["globalTemplate"]` (`Order = 0`) e não `RouteData.Values["aboutTemplate"]` (`Order = 1`) devido à configuração da propriedade `Order`.

Solicite a página About da amostra em `localhost:5000/About/GlobalRouteValue/AboutRouteValue` e inspecione o resultado:

![A página About é solicitada com segmentos de rota para GlobalRouteValue e AboutRouteValue. A página renderizada mostra que os valores de dados de rota são capturados no método OnGet da página.](razor-pages-convention-features/_static/about-page-global-and-about-templates.png)

## <a name="configure-a-page-route"></a>Configurar uma rota de página

Use [AddPageRoute](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.addpageroute) para configurar uma rota para uma página no caminho de página especificado. Os links gerados para a página usam a rota especificada. `AddPageRoute` usa `AddPageRouteModelConvention` para estabelecer a rota.

O aplicativo de exemplo cria uma rota para `/TheContactPage` para *Contact.cshtml*:

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet5)]

A página Contact também pode ser acessada em `/Contact` por meio de sua rota padrão.

A rota personalizada do aplicativo de exemplo para a página Contact permite um segmento de rota `text` opcional (`{text?}`). A página também inclui esse segmento opcional em sua diretiva `@page`, caso o visitante acesse a página em sua rota `/Contact`:

[!code-cshtml[Main](razor-pages-convention-features/sample/Pages/Contact.cshtml?highlight=1)]

Observe que a URL gerada para o link **Contato** na página renderizada reflete a rota atualizada:

![Link Contato do aplicativo de exemplo na barra de navegação](razor-pages-convention-features/_static/contact-link.png)

![A inspeção do link Contato no HTML renderizado indica que o href é definido como '/TheContactPage'](razor-pages-convention-features/_static/contact-link-source.png)

Visite a página Contact em sua rota comum, `/Contact`, ou na rota personalizada, `/TheContactPage`. Se você fornecer um segmento de rota `text` adicional, a página mostrará o segmento codificado em HTML fornecido:

![Exemplo do navegador Microsoft Edge de fornecimento de um segmento de rota 'text' opcional de 'TextValue' na URL. A página renderizada mostra o valor de segmento 'text'.](razor-pages-convention-features/_static/route-segment-with-custom-route.png)

## <a name="page-model-action-conventions"></a>Convenções de ação do modelo de página

O provedor de modelo de página padrão que implementa [IPageApplicationModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelprovider) invoca convenções que foram projetadas para fornecer pontos de extensibilidade para configuração de modelos de página. Essas convenções são úteis ao criar e modificar recursos de descoberta e processamento de página.

Para os exemplos desta seção, o aplicativo de exemplo usa uma classe `AddHeaderAttribute`, que é um [ResultFilterAttribute](/dotnet/api/microsoft.aspnetcore.mvc.filters.resultfilterattribute), aplicável a um cabeçalho de resposta:

[!code-csharp[Main](razor-pages-convention-features/sample/Filters/AddHeader.cs?name=snippet1)]

Usando convenções, a amostra explica como aplicar o atributo a todas as páginas de uma pasta e a uma única página.

**Convenção de modelo de aplicativo de pasta**

Use [AddFolderApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addfolderapplicationmodelconvention) para criar e adicionar uma [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention) que invoca uma ação em instâncias de [PageApplicationModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageapplicationmodel) em todas as páginas na pasta especificada.

A amostra explica o uso de `AddFolderApplicationModelConvention` adicionando um cabeçalho, `OtherPagesHeader`, às páginas dentro da pasta *OtherPages* do aplicativo:

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet6)]

Solicite a página Page1 da amostra em `localhost:5000/OtherPages/Page1` e inspecione os cabeçalhos para exibir o resultado:

![Os cabeçalhos de resposta da página OtherPages/Page1 mostram que o OtherPagesHeader foi adicionado.](razor-pages-convention-features/_static/page1-otherpages-header.png)

**Convenção de modelo de aplicativo de página**

Use [AddPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addpageapplicationmodelconvention) para criar e adicionar uma [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention) que invoca uma ação no [PageApplicationModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageapplicationmodel) para a página com o nome especificado.

A amostra explica o uso de `AddPageApplicationModelConvention` adicionando um cabeçalho, `AboutHeader`, à página About:

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet7)]

Solicite a página About da amostra em `localhost:5000/About` e inspecione os cabeçalhos para exibir o resultado:

![Os cabeçalhos de resposta da página About mostram que o AboutHeader foi adicionado.](razor-pages-convention-features/_static/about-page-about-header.png)

**Configurar um filtro**

[ConfigureFilter](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.configurefilter) configura o filtro especificado a ser aplicado. É possível implementar uma classe de filtro, mas o aplicativo de exemplo mostra como implementar um filtro em uma expressão lambda, que é implementado em segundo plano como um alocador que retorna um filtro:

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet8)]

O modelo de aplicativo de página é usado para verificar o caminho relativo para segmentos que levam à página Page2 na pasta *OtherPages*. Se a condição é aprovada, um cabeçalho é adicionado. Caso contrário, o `EmptyFilter` é aplicado.

`EmptyFilter` é um [filtro de Ação](xref:mvc/controllers/filters#action-filters). Como os filtros de Ação são ignorados pelas Páginas do Razor, o `EmptyFilter` não funciona conforme esperado se o caminho não contém `OtherPages/Page2`.

Solicite a página Page2 da amostra em `localhost:5000/OtherPages/Page2` e inspecione os cabeçalhos para exibir o resultado:

![O OtherPagesPage2Header é adicionado à resposta para Page2.](razor-pages-convention-features/_static/page2-filter-header.png)

**Configurar um alocador de filtro**

[ConfigureFilter](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.configurefilter?view=aspnetcore-2.0#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_ConfigureFilter_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_Func_Microsoft_AspNetCore_Mvc_ApplicationModels_PageApplicationModel_Microsoft_AspNetCore_Mvc_Filters_IFilterMetadata__) configura o alocador especificado para aplicar [filtros](xref:mvc/controllers/filters) a todas as Páginas do Razor.

O aplicativo de exemplo fornece um exemplo de como usar um [alocador de filtro](xref:mvc/controllers/filters#ifilterfactory) adicionando um cabeçalho, `FilterFactoryHeader`, com dois valores para as páginas do aplicativo:

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet9)]

*AddHeaderWithFactory.cs*:

[!code-csharp[Main](razor-pages-convention-features/sample/Factories/AddHeaderWithFactory.cs?name=snippet1)]

Solicite a página About da amostra em `localhost:5000/About` e inspecione os cabeçalhos para exibir o resultado:

![Os cabeçalhos de resposta da página About mostram que dois cabeçalhos FilterFactoryHeader foram adicionados.](razor-pages-convention-features/_static/about-page-filter-factory-header.png)

## <a name="replace-the-default-page-app-model-provider"></a>Substituir o provedor de modelo de aplicativo de página padrão

As Páginas do Razor usam a interface `IPageApplicationModelProvider` para criar um [DefaultPageApplicationModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.internal.defaultpageapplicationmodelprovider). Herde do provedor de modelo padrão para fornecer sua própria lógica de implementação para descoberta e processamento do manipulador. A implementação padrão ([fonte de referência](https://github.com/aspnet/Mvc/blob/rel/2.0.1/src/Microsoft.AspNetCore.Mvc.RazorPages/Internal/DefaultPageApplicationModelProvider.cs)) estabelece convenções para a nomenclatura *sem nome* e *nomeado* do manipulador, que é descrita abaixo.

**Métodos de manipulador sem nome padrão**

Os métodos de manipulador para verbos HTTP (métodos de manipulador "sem nome") seguem uma convenção: `On<HTTP verb>[Async]` (o acréscimo de `Async` é opcional, mas recomendado para métodos assíncronos).

| Método de manipulador sem nome     | Operação                      |
| -------------------------- | ------------------------------ |
| `OnGet`/`OnGetAsync`       | Inicializar o estado da página.     |
| `OnPost`/`OnPostAsync`     | Manipular solicitações POST.          |
| `OnDelete`/`OnDeleteAsync` | Manipular solicitações DELETE&#8224;. |
| `OnPut`/`OnPutAsync`       | Manipular solicitações PUT&#8224;.    |
| `OnPatch`/`OnPatchAsync`   | Manipular solicitações PATCH8224;.  |

&#8224;Usado para fazer chamadas à API para a página.

**Métodos de manipulador nomeado padrão**

Os métodos de manipulador fornecidos pelo desenvolvedor (métodos de manipulador "nomeado") seguem uma convenção semelhante. O nome do manipulador é exibido após o verbo HTTP ou entre o verbo HTTP e `Async`: `On<HTTP verb><handler name>[Async]` (o acréscimo de `Async` é opcional, mas recomendado para métodos assíncronos). Por exemplo, os métodos que processam mensagens podem usar a nomenclatura mostrada na tabela abaixo.

| Método do manipulador nomeado de exemplo             | Operação de exemplo        |
| ---------------------------------------- | ------------------------ |
| `OnGetMessage`/`OnGetMessageAsync`       | Obter uma mensagem.        |
| `OnPostMessage`/`OnPostMessageAsync`     | Executar POST em uma mensagem.          |
| `OnDeleteMessage`/`OnDeleteMessageAsync` | Executar DELETE em uma mensagem&#8224;. |
| `OnPutMessage`/`OnPutMessageAsync`       | Executar PUT em uma mensagem&#8224;.    |
| `OnPatchMessage`/`OnPatchMessageAsync`   | Executar PATCH em uma mensagem&#8224;.  |

&#8224;Usado para fazer chamadas à API para a página.

**Personalizar os nomes do método de manipulador**

Suponha que você prefira alterar a maneira como os métodos de manipulador nomeado e não nomeado são nomeados. Um esquema de nomenclatura alternativo é evitar que os nomes de método comecem com "On" e usar o primeiro segmento de palavra para determinar o verbo HTTP. Faça outras alterações, como converter os verbos DELETE, PUT e PATCH em POST. Um esquema desse tipo fornece os nomes de método mostrados na tabela a seguir.

| Método do manipulador                       | Operação                      |
| ------------------------------------ | ------------------------------ |
| `Get`                                | Inicializar o estado da página.     |
| `Post`/`PostAsync`                   | Manipular solicitações POST.          |
| `Delete`/`DeleteAsync`               | Manipular solicitações DELETE&#8224;. |
| `Put`/`PutAsync`                     | Manipular solicitações PUT&#8224;.    |
| `Patch`/`PatchAsync`                 | Manipular solicitações PATCH8224;.  |
| `GetMessage`                         | Obter uma mensagem.              |
| `PostMessage`/`PostMessageAsync`     | Executar POST em uma mensagem.                |
| `DeleteMessage`/`DeleteMessageAsync` | Executar POST em uma mensagem a ser excluída.      |
| `PutMessage`/`PutMessageAsync`       | Executar POST em uma mensagem a ser colocada.         |
| `PatchMessage`/`PatchMessageAsync`   | Executar POST em uma mensagem a ser corrigida.       |

&#8224;Usado para fazer chamadas à API para a página.

Para estabelecer esse esquema, herde da classe `DefaultPageApplicationModelProvider` e substitua o método [CreateHandlerModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.internal.defaultpageapplicationmodelprovider.createhandlermodel) para fornecer uma lógica personalizada para resolver os nomes de manipulador de [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel). O aplicativo de exemplo mostra como isso é feito em sua classe `CustomPageApplicationModelProvider`:

[!code-csharp[Main](razor-pages-convention-features/sample/CustomPageApplicationModelProvider.cs?name=snippet1&highlight=1-2,45-46,64-68,78-85,87,92,106)]

Destaques da classe incluem:

* A classe herda de `DefaultPageApplicationModelProvider`.
* O `TryParseHandlerMethod` processa um manipulador para determinar o verbo HTTP (`httpMethod`) e o nome do manipulador nomeado (`handlerName`) ao criar o `PageHandlerModel`.
  * Um `Async` pós-fixado é ignorado, se presente.
  * O uso de maiúsculas é adotado para analisar o verbo HTTP com base no nome do método.
  * Quando o nome do método (sem `Async`) é igual ao nome do verbo HTTP, não há um manipulador nomeado. O `handlerName` é definido como `null` e o nome do método é `Get`, `Post`, `Delete`, `Put` ou `Patch`.
  * Quando o nome do método (sem `Async`) é maior que o nome do verbo HTTP, há um manipulador nomeado. O `handlerName` é definido como `<method name (less 'Async', if present)>`. Por exemplo, "GetMessage" e "GetMessageAsync" geram um nome de manipulador "GetMessage".
  * Os verbos HTTP DELETE, PUT e PATCH são convertidos em POST.

Registre o `CustomPageApplicationModelProvider` na classe `Startup`:

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet10)]

O modelo de página em *Index.cshtml.cs* mostra como as convenções de nomenclatura comuns de método de manipulador são alteradas para as páginas no aplicativo. A nomenclatura de prefixo comum "On" usada com as Páginas do Razor é removida. O método que inicializa o estado da página é agora nomeado `Get`. Você poderá ver essa convenção usada em todo o aplicativo se abrir qualquer modelo de página de uma das páginas.

Cada um dos outros métodos começam com o verbo HTTP que descreve seu processamento. Os dois métodos que começam com `Delete` normalmente são tratados como verbos HTTP DELETE, mas a lógica em `TryParseHandlerMethod` define de forma explícita o verbo como POST para ambos os manipuladores.

Observe que `Async` é opcional entre `DeleteAllMessages` e `DeleteMessageAsync`. Os dois são métodos assíncronos, mas você pode optar por usar o `Async` pós-fixado ou não; recomendamos que você faça isso. `DeleteAllMessages` é usado aqui para fins de demonstração, mas recomendamos que você nomeie um método desse tipo `DeleteAllMessagesAsync`. Isso não afeta o processamento da implementação da amostra, mas o uso do `Async` pós-fixado indica que se trata de um método assíncrono.

[!code-csharp[Main](razor-pages-convention-features/sample/Pages/Index.cshtml.cs?name=snippet1&highlight=1,6,16,29)]

Observe que os nomes de manipulador fornecidos em *Index.cshtml* correspondem aos métodos de manipulador `DeleteAllMessages` e `DeleteMessageAsync`:

[!code-cshtml[Main](razor-pages-convention-features/sample/Pages/Index.cshtml?range=29-60&highlight=7-8,24-25)]

`Async` no nome do método de manipulador `DeleteMessageAsync` é excluído do `TryParseHandlerMethod` para a correspondência de manipulador da solicitação POST com o método. É feita a correspondência do nome `asp-page-handler` de `DeleteMessage` ao método de manipulador `DeleteMessageAsync`.

## <a name="mvc-filters-and-the-page-filter-ipagefilter"></a>Filtros MVC e o filtro de Página (IPageFilter)

Os [filtros de Ação](xref:mvc/controllers/filters#action-filters) MVC são ignorados pelas Páginas do Razor, pois elas usam métodos de manipulador. Outros tipos de filtros MVC estão disponíveis para uso: [Autorização](xref:mvc/controllers/filters#authorization-filters), [Exceção](xref:mvc/controllers/filters#exception-filters), [Recurso](xref:mvc/controllers/filters#resource-filters) e [Resultado](xref:mvc/controllers/filters#result-filters). Para obter mais informações, consulte o tópico [Filtros](xref:mvc/controllers/filters).

O filtro de Página ([IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter)) é um filtro que se aplica às Páginas do Razor. Ele envolve a execução de um método de manipulador de página. Permite que você processe um código personalizado em estágios da execução do método de manipulador de página. Este é um exemplo do aplicativo de exemplo:

[!code-csharp[Main](razor-pages-convention-features/sample/Filters/ReplaceRouteValueFilterAttribute.cs?name=snippet1)]

Esse filtro procura um valor de rota `globalTemplate` igual a "TriggerValue" e troca-o em "ReplacementValue".

O atributo `ReplaceRouteValueFilter` pode ser aplicado diretamente a um `PageModel`:

[!code-csharp[Main](razor-pages-convention-features/sample/Pages/OtherPages/Page3.cshtml.cs?range=10-12&highlight=1)]

Solicite a página Page3 do aplicativo de amostra com um `localhost:5000/OtherPages/Page3/TriggerValue`. Observe como o filtro substitui o valor de rota:

![Solicitação para OtherPages/Page3 com resultados de segmento de rota de TriggerValue no filtro substituindo o valor de rota por ReplacementValue.](razor-pages-convention-features/_static/otherpages-page3-filter-replacement-value.png)

## <a name="see-also"></a>Consulte também

* [Convenções de autorização de Páginas Razor](xref:security/authorization/razor-pages-authorization)

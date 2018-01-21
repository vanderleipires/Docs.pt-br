---
title: "Razor páginas rota e aplicativo convenção recursos no ASP.NET Core"
author: guardrex
description: "Descubra como você ajudam roteamento de página de controle, descoberta e processamento de rota e aplicativo recursos de convenção de provedor de modelo."
ms.author: riande
manager: wpickett
ms.date: 10/23/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/razor-pages/razor-pages-convention-features
ms.openlocfilehash: 69475ca9abd4e732dc704ad6a8a2fffe219984f7
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/19/2018
---
# <a name="razor-pages-route-and-app-convention-features-in-aspnet-core"></a>Razor páginas rota e aplicativo convenção recursos no ASP.NET Core

Por [Luke Latham](https://github.com/guardrex)

Saiba como usar a página rota e aplicativo provedor convenção recursos de modelo para controlar o processamento em páginas Razor aplicativos, descoberta e roteamento de página. Quando você precisa configurar rotas de página personalizado para páginas individuais, configurar o roteamento para páginas com a [AddPageRoute convenção](#configure-a-page-route) descrito posteriormente neste tópico.

Use o [aplicativo de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/razor-pages-convention-features/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample)) para explorar os recursos descritos neste tópico.

| Recursos | O exemplo demonstra... |
| -------- | --------------------------- |
| [Convenções de modelo de rota e de aplicativo](#add-route-and-app-model-conventions)<br><br>Conventions.Add<ul><li>IPageRouteModelConvention</li><li>IPageApplicationModelConvention</li></ul> | Adicionando um modelo de rota e o cabeçalho para páginas do aplicativo. |
| [Convenções de ação de rota de página](#page-route-action-conventions)<ul><li>AddFolderRouteModelConvention</li><li>AddPageRouteModelConvention</li><li>AddPageRoute</li></ul> | Adicionando um modelo de rota para páginas em uma pasta e para uma única página. |
| [Convenções de ação do modelo de página](#page-model-action-conventions)<ul><li>AddFolderApplicationModelConvention</li><li>AddPageApplicationModelConvention</li><li>ConfigureFilter (a classe de filtro, expressão lambda ou fábrica de filtro)</li></ul> | Adicionar um cabeçalho de páginas em uma pasta, adicionar um cabeçalho para uma única página e configurar um [fábrica de filtro](xref:mvc/controllers/filters#ifilterfactory) para adicionar um cabeçalho para páginas do aplicativo. |
| [Provedor de modelo de aplicativo de página padrão](#replace-the-default-page-app-model-provider) | Substituindo o provedor de modelo de página padrão para alterar as convenções de nomenclatura do manipulador. |

## <a name="add-route-and-app-model-conventions"></a>Adicionar as convenções de modelo de rota e de aplicativo

Adicionar um delegado para [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) adicionar convenções de modelo de rota e de aplicativo que se aplicam a páginas Razor.

**Adicionar uma convenção de modelo de rota para todas as páginas**

Use [convenções](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions) para criar e adicionar um [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention) à coleção de [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) instâncias que são aplicadas durante o modelo de rota e de página construção.

O aplicativo de exemplo adiciona um `{globalTemplate?}` modelo de rota para todas as páginas no aplicativo:

[!code-csharp[Main](razor-pages-convention-features/sample/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

> [!NOTE]
> O `Order` propriedade para o `AttributeRouteModel` é definido como `0` (zero). Isso garante que esse modelo tem prioridade para a primeira posição de valor de dados de rota quando é fornecido um valor único de rota. Por exemplo, o exemplo adiciona um `{aboutTemplate?}` modelo de rota posteriormente no tópico. O `{aboutTemplate?}` modelo é fornecido um `Order` de `1`. Quando a página sobre é solicitada no `/About/RouteDataValue`, "RouteDataValue" é carregado no `RouteData.Values["globalTemplate"]` (`Order = 0`) e não `RouteData.Values["aboutTemplate"]` (`Order = 1`) devido à configuração de `Order` propriedade.

*Startup.cs*:

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet1)]

Solicitar a página sobre da amostra em `localhost:5000/About/GlobalRouteValue` e inspecione o resultado:

![A página sobre é solicitada com um segmento de rota de GlobalRouteValue. A página renderizada mostra que o valor de dados de rota for capturado no método OnGet da página.](razor-pages-convention-features/_static/about-page-global-template.png)

**Adicionar uma convenção de modelo de aplicativo para todas as páginas**

Use [convenções](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions) para criar e adicionar um [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention) à coleção de [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) instâncias que são aplicadas durante a rota e de página construção de modelo.

Para demonstrar isso e outras convenções posteriormente no tópico, o aplicativo de exemplo inclui um `AddHeaderAttribute` classe. O construtor de classe aceitar um `name` cadeia de caracteres e um `values` matriz de cadeia de caracteres. Esses valores são usados em seu `OnResultExecuting` método para definir um cabeçalho de resposta. A classe completa é mostrada no [página convenções de ação do modelo](#page-model-action-conventions) posteriormente no tópico.

O aplicativo de exemplo usa o `AddHeaderAttribute` classe para adicionar um cabeçalho, `GlobalHeader`, para todas as páginas no aplicativo:

[!code-csharp[Main](razor-pages-convention-features/sample/Conventions/GlobalHeaderPageApplicationModelConvention.cs?name=snippet1)]

*Startup.cs*:

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet2)]

Solicitar a página sobre da amostra em `localhost:5000/About` e inspecione os cabeçalhos para exibir o resultado:

![Cabeçalhos de resposta da página sobre mostram que o GlobalHeader foi adicionado.](razor-pages-convention-features/_static/about-page-global-header.png)

## <a name="page-route-action-conventions"></a>Convenções de ação de rota de página

O provedor de modelo de rota padrão que é derivada de [IPageRouteModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelprovider) invoca convenções que são projetadas para fornecer pontos de extensibilidade para configurar rotas de página.

**Convenção de pasta de modelo de rota**

Use [AddFolderRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addfolderroutemodelconvention) para criar e adicionar um [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention) que invoca uma ação no [PageRouteModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageroutemodel) para todas as páginas no pasta especificada.

O aplicativo de exemplo usa `AddFolderRouteModelConvention` para adicionar um `{otherPagesTemplate?}` modelo de rota para as páginas de *OtherPages* pasta:

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet3)]

> [!NOTE]
> O `Order` propriedade para o `AttributeRouteModel` é definido como `1`. Isso garante que o modelo para `{globalTemplate?}` (configurada anteriormente o tópico) tem prioridade para os dados da rota primeiro valor posição quando é fornecido um valor único de rota. Se a página Page1 é solicitada no `/OtherPages/Page1/RouteDataValue`, "RouteDataValue" é carregado no `RouteData.Values["globalTemplate"]` (`Order = 0`) e não `RouteData.Values["otherPagesTemplate"]` (`Order = 1`) devido à configuração de `Order` propriedade.

Solicitação de página de Página1 do exemplo em `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` e inspecione o resultado:

![Página1 na pasta OtherPages é solicitado com um segmento de rota de GlobalRouteValue e OtherPagesRouteValue. A página renderizada mostra que os valores de dados de rota são capturados no método OnGet da página.](razor-pages-convention-features/_static/otherpages-page1-global-and-otherpages-templates.png)

**Convenção de página de modelo de rota**

Use [AddPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addpageroutemodelconvention) para criar e adicionar um [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention) que invoca uma ação no [PageRouteModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageroutemodel) para a página com especificado nome.

O aplicativo de exemplo usa `AddPageRouteModelConvention` para adicionar um `{aboutTemplate?}` modelo de rota para a página sobre:

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet4)]

> [!NOTE]
> O `Order` propriedade para o `AttributeRouteModel` é definido como `1`. Isso garante que o modelo para `{globalTemplate?}` (configurada anteriormente o tópico) tem prioridade para os dados da rota primeiro valor posição quando é fornecido um valor único de rota. Se a página sobre é solicitada no `/About/RouteDataValue`, "RouteDataValue" é carregado no `RouteData.Values["globalTemplate"]` (`Order = 0`) e não `RouteData.Values["aboutTemplate"]` (`Order = 1`) devido à configuração de `Order` propriedade.

Solicitar a página sobre da amostra em `localhost:5000/About/GlobalRouteValue/AboutRouteValue` e inspecione o resultado:

![Sobre a página é solicitada com segmentos de rota, para GlobalRouteValue e AboutRouteValue. A página renderizada mostra que os valores de dados de rota são capturados no método OnGet da página.](razor-pages-convention-features/_static/about-page-global-and-about-templates.png)

## <a name="configure-a-page-route"></a>Configurar uma rota de página

Use [AddPageRoute](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.addpageroute) para configurar uma rota para uma página no caminho de página especificada. Links gerados para a página usam a rota especificada. `AddPageRoute`usa `AddPageRouteModelConvention` para estabelecer a rota.

O aplicativo de exemplo cria uma rota para `/TheContactPage` para *Contact.cshtml*:

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet5)]

A página de contato também pode ser contatada pelo `/Contact` por meio da rota padrão.

Permite que a rota personalizados do aplicativo de exemplo para a página de contato para um recurso opcional `text` segmento de rota (`{text?}`). A página também inclui esse segmento opcional no seu `@page` diretiva caso o visitante acessa a página em seu `/Contact` rota:

[!code-cshtml[Main](razor-pages-convention-features/sample/Pages/Contact.cshtml?highlight=1)]

Observe que a URL gerada para o **contato** link na página renderizada reflete a rota atualizada:

![Exemplo de link de contato do aplicativo na barra de navegação](razor-pages-convention-features/_static/contact-link.png)

![Inspecionar o link de contato no HTML renderizado indica que o href é definido como ' / TheContactPage'](razor-pages-convention-features/_static/contact-link-source.png)

Visite a página de contato no seu roteiro comum, `/Contact`, ou a rota personalizada, `/TheContactPage`. Se você fornecer adicional `text` segmento de rota, a página mostra o segmento codificada em HTML que você forneça:

![Exemplo de navegador de borda de fornecimento de um segmento de rota 'texto' opcional ValorTexto na URL. A página renderizada mostra o valor de segmento de 'text'.](razor-pages-convention-features/_static/route-segment-with-custom-route.png)

## <a name="page-model-action-conventions"></a>Convenções de ação do modelo de página

O provedor de modelo de página padrão que implementa [IPageApplicationModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelprovider) invoca convenções que são projetadas para fornecer pontos de extensibilidade para configurar modelos de página. As seguintes convenções são úteis ao criar e modificar recursos de processamento e detecção de página.

Para os exemplos nesta seção, o aplicativo de exemplo usa um `AddHeaderAttribute` classe, que é um [ResultFilterAttribute](/dotnet/api/microsoft.aspnetcore.mvc.filters.resultfilterattribute), que se aplica a um cabeçalho de resposta:

[!code-csharp[Main](razor-pages-convention-features/sample/Filters/AddHeader.cs?name=snippet1)]

Usando convenções, o exemplo demonstra como aplicar o atributo para todas as páginas em uma pasta e uma única página.

**Convenção de pasta de modelo de aplicativo**

Use [AddFolderApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addfolderapplicationmodelconvention) para criar e adicionar um [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention) que invoca uma ação em [PageApplicationModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageapplicationmodel) instâncias todas as páginas na pasta especificada.

O exemplo demonstra o uso de `AddFolderApplicationModelConvention` adicionando um cabeçalho, `OtherPagesHeader`, para as páginas dentro de *OtherPages* pasta do aplicativo:

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet6)]

Solicitação de página de Página1 do exemplo em `localhost:5000/OtherPages/Page1` e inspecione os cabeçalhos para exibir o resultado:

![Cabeçalhos de resposta da página OtherPages/Página1 mostram que o OtherPagesHeader foi adicionado.](razor-pages-convention-features/_static/page1-otherpages-header.png)

**Convenção de modelo de aplicativo de página**

Use [AddPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addpageapplicationmodelconvention) para criar e adicionar um [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention) que invoca uma ação sobre o [PageApplicationModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageapplicationmodel) para a página com o nome speciifed.

O exemplo demonstra o uso de `AddPageApplicationModelConvention` adicionando um cabeçalho, `AboutHeader`, para a página sobre:

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet7)]

Solicitar a página sobre da amostra em `localhost:5000/About` e inspecione os cabeçalhos para exibir o resultado:

![Cabeçalhos de resposta da página sobre mostram que o AboutHeader foi adicionado.](razor-pages-convention-features/_static/about-page-about-header.png)

**Configurar um filtro**

[ConfigureFilter](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.configurefilter) configura o filtro especificado a ser aplicado. Você pode implementar uma classe de filtro, mas o aplicativo de exemplo mostra como implementar um filtro em uma expressão lambda, que é implementada bastidores como uma fábrica que retorna um filtro:

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet8)]

O modelo de aplicativo de página é usado para verificar o caminho relativo para segmentos que levam para a página Página2 o *OtherPages* pasta. Se a condição for aprovada, um cabeçalho é adicionado. Caso contrário, o `EmptyFilter` é aplicada.

`EmptyFilter`é um [filtro de ação](xref:mvc/controllers/filters#action-filters). Como os filtros de ação são ignorados por páginas Razor, o `EmptyFilter` ops não conforme o esperado se o caminho não contém `OtherPages/Page2`.

Solicitação de página de Page2 do exemplo em `localhost:5000/OtherPages/Page2` e inspecione os cabeçalhos para exibir o resultado:

![O OtherPagesPage2Header é adicionada à resposta para Página2.](razor-pages-convention-features/_static/page2-filter-header.png)

**Configurar uma fábrica de filtro**

[ConfigureFilter](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.configurefilter?view=aspnetcore-2.0#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_ConfigureFilter_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_Func_Microsoft_AspNetCore_Mvc_ApplicationModels_PageApplicationModel_Microsoft_AspNetCore_Mvc_Filters_IFilterMetadata__) configura a fábrica especificada para aplicar [filtros](xref:mvc/controllers/filters) a todas as páginas Razor.

O aplicativo de exemplo fornece um exemplo de como usar um [fábrica de filtro](xref:mvc/controllers/filters#ifilterfactory) adicionando um cabeçalho, `FilterFactoryHeader`, com dois valores para as páginas do aplicativo:

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet9)]

*AddHeaderWithFactory.cs*:

[!code-csharp[Main](razor-pages-convention-features/sample/Factories/AddHeaderWithFactory.cs?name=snippet1)]

Solicitar a página sobre da amostra em `localhost:5000/About` e inspecione os cabeçalhos para exibir o resultado:

![Cabeçalhos de resposta da página sobre mostram que foram adicionados dois cabeçalhos FilterFactoryHeader.](razor-pages-convention-features/_static/about-page-filter-factory-header.png)

## <a name="replace-the-default-page-app-model-provider"></a>Substitua o provedor de modelo de aplicativo de página padrão

Páginas Razor usa o `IPageApplicationModelProvider` interface para criar um [DefaultPageApplicationModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.internal.defaultpageapplicationmodelprovider). Você pode herdar de provedor de modelo padrão para fornecer sua própria lógica de implementação para processamento e a descoberta de manipulador. A implementação padrão ([fonte de referência](https://github.com/aspnet/Mvc/blob/rel/2.0.1/src/Microsoft.AspNetCore.Mvc.RazorPages/Internal/DefaultPageApplicationModelProvider.cs)) estabelece convenções para *sem nome* e *chamado* manipulador de nomeação, que é descrita abaixo.

**Padrão de métodos do manipulador sem nome**

Métodos de manipulador para verbos HTTP (métodos do manipulador "sem nome") seguem uma convenção: `On<HTTP verb>[Async]` (acrescentando `Async` é opcional mas recomendado para métodos assíncronos).

| Método do manipulador sem nome     | Operação                      |
| -------------------------- | ------------------------------ |
| `OnGet`/`OnGetAsync`       | Inicialize o estado da página.     |
| `OnPost`/`OnPostAsync`     | Lidar com solicitações POST.          |
| `OnDelete`/`OnDeleteAsync` | Manipule solicitações DELETE &#8224;. |
| `OnPut`/`OnPutAsync`       | Lidar com solicitações PUT &#8224;.    |
| `OnPatch`/`OnPatchAsync`   | Lidar com solicitações PATCH &#8224;.  |

&#8224; Usado para fazer chamadas de API para a página.

**Métodos do manipulador de nomeado padrão**

Métodos do manipulador fornecidos pelo desenvolvedor ("nomeado" métodos do manipulador) siga uma convenção semelhante. O nome do manipulador é exibida após o verbo HTTP ou entre o verbo HTTP e `Async`: `On<HTTP verb><handler name>[Async]` (acrescentando `Async` é opcional mas recomendado para métodos assíncronos). Por exemplo, os métodos que processam mensagens podem levar a nomenclatura mostradas na tabela a seguir.

| Chamada de método do manipulador de exemplo             | Exemplo de operação        |
| ---------------------------------------- | ------------------------ |
| `OnGetMessage`/`OnGetMessageAsync`       | Obtenha uma mensagem.        |
| `OnPostMessage`/`OnPostMessageAsync`     | POSTE uma mensagem.          |
| `OnDeleteMessage`/`OnDeleteMessageAsync` | Exclua uma mensagem &#8224;. |
| `OnPutMessage`/`OnPutMessageAsync`       | COLOCAR uma mensagem &#8224;.    |
| `OnPatchMessage`/`OnPatchMessageAsync`   | Uma mensagem &#8224; o PATCH.  |

&#8224; Usado para fazer chamadas de API para a página.

**Personalizar os nomes de método do manipulador**

Suponha que você prefere alterar o modo como os métodos do manipulador nomeados e não são nomeados. Um esquema de nomenclatura alternativo é evitar iniciar os nomes de método com "On" e usar o primeiro segmento do word para determinar o verbo HTTP. Você pode fazer outras alterações, como converter os verbos para exclusão, PUT e POST do PATCH. Um esquema desse tipo fornece os nomes de método mostrados na tabela a seguir.

| Método de manipulador                       | Operação                      |
| ------------------------------------ | ------------------------------ |
| `Get`                                | Inicialize o estado da página.     |
| `Post`/`PostAsync`                   | Lidar com solicitações POST.          |
| `Delete`/`DeleteAsync`               | Manipule solicitações DELETE &#8224;. |
| `Put`/`PutAsync`                     | Lidar com solicitações PUT &#8224;.    |
| `Patch`/`PatchAsync`                 | Lidar com solicitações PATCH &#8224;.  |
| `GetMessage`                         | Obtenha uma mensagem.              |
| `PostMessage`/`PostMessageAsync`     | POSTE uma mensagem.                |
| `DeleteMessage`/`DeleteMessageAsync` | POSTE uma mensagem para excluir.      |
| `PutMessage`/`PutMessageAsync`       | POSTE uma mensagem para colocar.         |
| `PatchMessage`/`PatchMessageAsync`   | POSTE uma mensagem para o patch.       |

&#8224; Usado para fazer chamadas de API para a página.

Para estabelecer esse esquema, herdam o `DefaultPageApplicationModelProvider` classe e substituir o [CreateHandlerModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.internal.defaultpageapplicationmodelprovider.createhandlermodel) método para fornecer lógica personalizada para resolver [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) os nomes de manipulador. O aplicativo de exemplo mostra como fazer isso seu `CustomPageApplicationModelProvider` classe:

[!code-csharp[Main](razor-pages-convention-features/sample/CustomPageApplicationModelProvider.cs?name=snippet1&highlight=1-2,45-46,64-68,78-85,87,92,106)]

Destaques da classe:

* A classe herda de `DefaultPageApplicationModelProvider`.
* O `TryParseHandlerMethod` processa um manipulador para determinar o verbo HTTP (`httpMethod`) e o nome do manipulador nomeado (`handlerName`) ao criar o `PageHandlerModel`.
  * Um `Async` sufixo é ignorado, se presente.
  * Maiusculas e minúsculas é usada para analisar o verbo HTTP do nome do método.
  * Quando o nome do método (sem `Async`) é igual ao nome do verbo HTTP, há um manipulador nomeado. O `handlerName` é definido como `null`, e o nome do método é `Get`, `Post`, `Delete`, `Put`, ou `Patch`.
  * Quando o nome do método (sem `Async`) é maior do que o nome do verbo HTTP, há um manipulador nomeado. O `handlerName` é definido como `<method name (less 'Async', if present)>`. Por exemplo, "GetMessage" e "GetMessageAsync" geram um nome de manipulador de "GetMessage".
  * Verbos HTTP PATCH, PUT e DELETE são convertidos em POST.

Registrar o `CustomPageApplicationModelProvider` no `Startup` classe:

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet10)]

O arquivo code-behind *Index.cshtml.cs* mostra como as convenções de nomenclatura de método do manipulador comuns são alteradas para as páginas do aplicativo. Comum "Em" prefixo de nomenclatura usada com Razor páginas é removido. O método que inicializa o estado da página é nomeado `Get`. Você pode ver essa convenção usada em todo o aplicativo, se você abrir qualquer arquivo de code-behind para qualquer uma das páginas.

Cada um dos outros métodos começar com o verbo HTTP que descreve seu processamento. Os dois métodos que começam com `Delete` normalmente seria tratada como verbos HTTP DELETE, mas a lógica em `TryParseHandlerMethod` define explicitamente o verbo POST para ambos os manipuladores.

Observe que `Async` é opcional entre `DeleteAllMessages` e `DeleteMessageAsync`. Elas são ambos os métodos assíncronos, mas você pode optar por usar o `Async` sufixo ou não; é recomendável que você faça. `DeleteAllMessages`é usado aqui para fins de demonstração, mas é recomendável nomear tal método `DeleteAllMessagesAsync`. Ela não afeta o processamento da implementação do exemplo mas usando o `Async` sufixo chama o fato de que se trata de um método assíncrono.

[!code-csharp[Main](razor-pages-convention-features/sample/Pages/Index.cshtml.cs?name=snippet1&highlight=1,6,16,29)]

Observe os nomes de manipulador fornecidos no *cshtml* corresponde a `DeleteAllMessages` e `DeleteMessageAsync` métodos do manipulador:

[!code-cshtml[Main](razor-pages-convention-features/sample/Pages/Index.cshtml?range=29-60&highlight=7-8,24-25)]

`Async`no nome do método do manipulador `DeleteMessageAsync` são fatorados fora pelo `TryParseHandlerMethod` para correspondência de manipulador de solicitação POST para o método. O `asp-page-handler` nome de `DeleteMessage` é correspondente ao método do manipulador de `DeleteMessageAsync`.

## <a name="mvc-filters-and-the-page-filter-ipagefilter"></a>Filtros de MVC e o filtro de página (IPageFilter)

MVC [filtros de ação](xref:mvc/controllers/filters#action-filters) são ignorados por páginas Razor, como páginas Razor usam métodos do manipulador. Outros tipos de filtros MVC estão disponíveis para uso: [autorização](xref:mvc/controllers/filters#authorization-filters), [exceção](xref:mvc/controllers/filters#exception-filters), [recurso](xref:mvc/controllers/filters#resource-filters), e [resultado](xref:mvc/controllers/filters#result-filters). Para obter mais informações, consulte o [filtros](xref:mvc/controllers/filters) tópico.

O filtro de página ([IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter)) é um filtro que se aplica a páginas Razor. Ele envolve a execução de um método de manipulador de página. Ele permite que você processe o código personalizado em estágios da execução do método de manipulador de página. Aqui está um exemplo de aplicativo de exemplo:

[!code-csharp[Main](razor-pages-convention-features/sample/Filters/ReplaceRouteValueFilterAttribute.cs?name=snippet1)]

Este filtro procura um `globalTemplate` valor de "TriggerValue" e as trocas de rota "ReplacementValue".

O `ReplaceRouteValueFilter` atributo pode ser aplicado diretamente a um `PageModel` no code-behind:

[!code-csharp[Main](razor-pages-convention-features/sample/Pages/OtherPages/Page3.cshtml.cs?range=10-12&highlight=1)]

Solicitar a página Page3 de amostra de aplicativo com em `localhost:5000/OtherPages/Page3/TriggerValue`. Observe como o filtro substitui o valor de rota:

![Solicitação para OtherPages/Page3 com resultados de segmento de rota um TriggerValue no filtro substituindo o valor de rota com valor de substituição.](razor-pages-convention-features/_static/otherpages-page3-filter-replacement-value.png)

## <a name="see-also"></a>Consulte também

* [Convenções de autorização de Páginas Razor](xref:security/authorization/razor-pages-authorization)

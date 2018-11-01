---
title: Roteamento no ASP.NET Core
author: ardalis
description: Descubra como a funcionalidade de roteamento do ASP.NET Core é responsável por mapear uma solicitação de entrada para um manipulador de rotas.
ms.author: riande
ms.custom: mvc
ms.date: 10/01/2018
uid: fundamentals/routing
ms.openlocfilehash: 06059d720bd4444b1ec12e42d466ee54d1658203
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207750"
---
# <a name="routing-in-aspnet-core"></a>Roteamento no ASP.NET Core

Por [Ryan Nowak](https://github.com/rynowak), [Steve Smith](https://ardalis.com/) e [Rick Anderson](https://twitter.com/RickAndMSFT)

A funcionalidade de roteamento é responsável por mapear uma solicitação de entrada para um manipulador de rotas. As rotas são definidas no aplicativo e configuradas quando o aplicativo é iniciado. Uma rota pode opcionalmente extrair os valores da URL contida na solicitação e esses valores podem então ser usados para o processamento da solicitação. Usando as informações de rota do aplicativo, a funcionalidade de roteamento também é capaz de gerar URLs que são mapeadas para manipuladores de rotas. Portanto, o roteamento pode encontrar um manipulador de rotas com base em uma URL ou encontrar a URL correspondente a um determinado manipulador de rotas com base nas informações do manipulador de rotas.

> [!IMPORTANT]
> Este documento aborda o roteamento de nível inferior do ASP.NET Core. Para obter informações sobre o roteamento do ASP.NET Core MVC, confira <xref:mvc/controllers/routing>.

[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/routing/samples) ([como baixar](xref:index#how-to-download-a-sample))

## <a name="routing-basics"></a>Conceitos básicos sobre roteamento

O roteamento usa *rotas* (implementações de <xref:Microsoft.AspNetCore.Routing.IRouter>) para:

* Mapear solicitações de entrada para *manipuladores de rotas*.
* Gerar as URLs usadas nas respostas.

Em geral, um aplicativo tem uma única coleção de rotas. Quando uma solicitação é recebida, a coleção de rotas é processada na ordem. A solicitação de entrada procura uma rota que corresponde à URL de solicitação chamando o método <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> em cada rota disponível na coleção de rotas. Por outro lado, uma resposta pode usar o roteamento para gerar URLs (por exemplo, para redirecionamento ou links) com base nas informações de rotas e evitar a necessidade de embutir as URLs em código, o que ajuda na facilidade de manutenção.

O roteamento está conectado ao pipeline do [middleware](xref:fundamentals/middleware/index) pela classe <xref:Microsoft.AspNetCore.Builder.RouterMiddleware>. O [ASP.NET Core MVC](xref:mvc/overview) adiciona o roteamento ao pipeline do middleware como parte de sua configuração. Para saber mais sobre o uso do roteamento como um componente autônomo, confira a seção [Usar o middleware de roteamento](#use-routing-middleware).

### <a name="url-matching"></a>Correspondência de URL

Correspondência de URL é o processo pelo qual o roteamento expede uma solicitação de entrada para um *manipulador*. Esse processo se baseia nos dados do caminho da URL, mas pode ser estendido para considerar qualquer dado na solicitação. A capacidade de expedir solicitações para manipuladores separados é fundamental para dimensionar o tamanho e a complexidade de um aplicativo.

As solicitações de entrada entram no `RouterMiddleware`, que chama o método <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> em cada rota na sequência. A instância <xref:Microsoft.AspNetCore.Routing.IRouter> escolhe se deseja *manipular* a solicitação definindo o [RouteContext.Handler](xref:Microsoft.AspNetCore.Routing.RouteContext.Handler*) como um <xref:Microsoft.AspNetCore.Http.RequestDelegate> não nulo. Se uma rota definir um manipulador para a solicitação, o processamento de rotas será interrompido e o manipulador será invocado para processar a solicitação. Se todas as rotas forem testadas e nenhum manipulador for encontrado para a solicitação, o middleware chamará o *próximo* e o próximo middleware no pipeline da solicitação será invocado.

A entrada primária para `RouteAsync` é o [RouteContext.HttpContext](xref:Microsoft.AspNetCore.Routing.RouteContext.HttpContext*) associado à solicitação atual. O `RouteContext.Handler` e o [RouteContext.RouteData](xref:Microsoft.AspNetCore.Routing.RouteContext.RouteData*) são os resultados definidos depois que uma rota é correspondida.

Uma correspondência durante `RouteAsync` também define as propriedades de `RouteContext.RouteData` para os valores apropriados com base no processamento da solicitação executado até o momento. Se uma rota corresponde a uma solicitação, o `RouteContext.RouteData` contém informações de estado importantes sobre o *resultado*.

[RouteData.Values](xref:Microsoft.AspNetCore.Routing.RouteData.Values*) é um dicionário de *valores de rota* produzido por meio da rota. Esses valores geralmente são determinados pela criação de token da URL e podem ser usados para aceitar a entrada do usuário ou tomar outras decisões de expedição dentro do aplicativo.

[RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) é um recipiente de propriedades de dados adicionais relacionados à rota correspondente. `DataTokens` são fornecidos para dar suporte à associação de dados de estado com cada rota para que o aplicativo possa tomar decisões posteriormente com base em qual rota foi correspondida. Esses valores são definidos pelo desenvolvedor e **não** afetam de forma alguma o comportamento do roteamento. Além disso, os valores armazenados em stash em `RouteData.DataTokens` podem ser de qualquer tipo, ao contrário de `RouteData.Values`, que precisa ser facilmente conversível de/em cadeias de caracteres.

[RouteData.Routers](xref:Microsoft.AspNetCore.Routing.RouteData.Routers*) é uma lista das rotas que participaram da correspondência bem-sucedida da solicitação. As rotas podem ser aninhadas uma dentro da outra. A propriedade `Routers` reflete o caminho pela árvore lógica de rotas que resultou em uma correspondência. Em geral, o primeiro item em `Routers` é a coleção de rotas e deve ser usado para a geração de URL. O último item em `Routers` é o manipulador de rotas que teve uma correspondência.

### <a name="url-generation"></a>Geração de URL

Geração de URL é o processo pelo qual o roteamento pode criar um caminho de URL de acordo com um conjunto de valores de rota. Isso permite uma separação lógica entre os manipuladores e as URLs que os acessam.

A geração de URL segue um processo iterativo semelhante, mas começa com o código da estrutura ou do usuário chamando o método <xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> da coleção de rotas. Em seguida, cada *rota* terá seu método `GetVirtualPath` chamado em sequência, até que um <xref:Microsoft.AspNetCore.Routing.VirtualPathData> não nulo seja retornado.

As entradas primárias para `GetVirtualPath` são:

* [VirtualPathContext.HttpContext](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.HttpContext*)
* [VirtualPathContext.Values](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.Values*)
* [VirtualPathContext.AmbientValues](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.AmbientValues*)

As rotas usam principalmente os valores de rota fornecidos por `Values` e `AmbientValues` para decidir se é possível gerar uma URL e quais valores serão incluídos. Os `AmbientValues` são o conjunto de valores de rota produzidos pela correspondência da solicitação atual com o sistema de roteamento. Por outro lado, `Values` são os valores de rota que especificam como gerar a URL desejada para a operação atual. O `HttpContext` é fornecido para o caso de uma rota precisar obter serviços ou dados adicionais associados ao contexto atual.

> [!TIP]
> Considere [VirtualPathContext.Values](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.Values*) como um conjunto de substituições de [VirtualPathContext.AmbientValues](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.AmbientValues*). A geração de URL tenta reutilizar os valores de rota da solicitação atual para facilitar a geração de URLs para links usando a mesma rota ou os mesmos valores de rota.

A saída de `GetVirtualPath` é um `VirtualPathData`. `VirtualPathData` é um paralelo de `RouteData`. `VirtualPathData` contém o `VirtualPath` da URL de saída e algumas propriedades adicionais que devem ser definidas pela rota.

A propriedade [VirtualPathData.VirtualPath](xref:Microsoft.AspNetCore.Routing.VirtualPathData.VirtualPath*) contém o *caminho virtual* produzido pela rota. Dependendo das suas necessidades, talvez você precise processar ainda mais o caminho. Se você desejar renderizar a URL gerada em HTML, preceda o caminho base do aplicativo.

O [VirtualPathData.Router](xref:Microsoft.AspNetCore.Routing.VirtualPathData.Router*) é uma referência à rota que gerou a URL com êxito.

As propriedades de [VirtualPathData.DataTokens](xref:Microsoft.AspNetCore.Routing.VirtualPathData.DataTokens*) são um dicionário de dados adicionais relacionados à rota que gerou a URL. Isso é o paralelo de [RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*).

### <a name="creating-routes"></a>Criando rotas

O roteamento fornece a classe <xref:Microsoft.AspNetCore.Routing.Route> como a implementação padrão de <xref:Microsoft.AspNetCore.Routing.IRouter>. `Route` usa a sintaxe de *modelo de rota* para definir padrões que corresponderão ao caminho da URL quando <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> for chamado. `Route` usa o mesmo modelo de rota para gerar uma URL quando `GetVirtualPath` é chamado.

A maioria dos aplicativos cria rotas chamando <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> ou um dos métodos de extensão semelhantes definidos em <xref:Microsoft.AspNetCore.Routing.IRouteBuilder>. Todos esses métodos criam uma instância de <xref:Microsoft.AspNetCore.Routing.Route> e adicionam-na à coleção de rotas.

`MapRoute` não usa um parâmetro de manipulador de rota. `MapRoute` apenas adiciona rotas que são manipuladas pelo <xref:Microsoft.AspNetCore.Routing.RouteBuilder.DefaultHandler*>. Como o manipulador padrão é um `IRouter`, ele pode optar por não manipular a solicitação. Por exemplo, o ASP.NET Core MVC normalmente é configurado como um manipulador padrão que só manipula as solicitações que correspondem a um controlador e a uma ação disponíveis. Para saber mais sobre o roteamento para o MVC, confira <xref:mvc/controllers/routing>.

O exemplo de código a seguir é um exemplo de uma chamada `MapRoute` usada por uma definição de rota típica do ASP.NET Core MVC:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

Esse modelo corresponde a um caminho de URL, como `/Products/Details/17` e extrai os valores de rota `{ controller = Products, action = Details, id = 17 }`. Os valores de rota são determinados pela divisão do caminho da URL em segmentos e pela correspondência de cada segmento com o nome *parâmetro de rota* no modelo de rota. Os parâmetros de rota são nomeados. Eles são definidos com a colocação do nome do parâmetro em chaves `{ ... }`.

O modelo anterior também poderia corresponder ao caminho da URL `/` e produziria os valores `{ controller = Home, action = Index }`. Isso ocorre porque os parâmetros de rota `{controller}` e `{action}` têm valores padrão e o parâmetro de rota `id` é opcional. Um sinal de igual `=` seguido de um valor após o nome do parâmetro de rota define um valor padrão para o parâmetro. Um ponto de interrogação `?` após o nome do parâmetro de rota define o parâmetro como opcional. Os parâmetros de rota com um valor padrão *sempre* produzem um valor de rota quando a rota corresponde. Os parâmetros opcionais não produzem um valor de rota quando não há nenhum segmento de caminho de URL correspondente.

Consulte [route-template-reference](#route-template-reference) para obter uma descrição completa dos recursos e da sintaxe de modelo de rota.

Este exemplo inclui uma *restrição de rota*:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id:int}");
```

Esse modelo corresponde a um caminho de URL, como `/Products/Details/17`, mas não como `/Products/Details/Apples`. A definição do parâmetro de rota `{id:int}` define uma [restrição de rota](#route-constraint-reference) para o parâmetro de rota `id`. As restrições de rota implementam <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> e inspecionam valores de rota para verificá-los. Neste exemplo, o valor de rota `id` precisa ser conversível em um inteiro. Consulte [route-constraint-reference](#route-constraint-reference) para obter uma explicação mais detalhada das restrições de rota que são fornecidas pela estrutura.

Sobrecargas adicionais de `MapRoute` aceitam valores para `constraints`, `dataTokens` e `defaults`. Esses parâmetros adicionais de `MapRoute` são definidos como o tipo `object`. O uso típico desses parâmetros é passar um objeto de tipo anônimo, no qual os nomes da propriedade do tipo anônimo correspondem aos nomes do parâmetro de rota.

Os dois seguintes exemplos criam rotas equivalentes:

```csharp
routes.MapRoute(
    name: "default_route",
    template: "{controller}/{action}/{id?}",
    defaults: new { controller = "Home", action = "Index" });

routes.MapRoute(
    name: "default_route",
    template: "{controller=Home}/{action=Index}/{id?}");
```

> [!TIP]
> A sintaxe embutida para a definição de restrições e padrões pode ser interessante para rotas simples. No entanto, há recursos, como tokens de dados, que não têm suporte na sintaxe embutida.

O exemplo a seguir demonstra mais alguns cenários:

```csharp
routes.MapRoute(
    name: "blog",
    template: "Blog/{*article}",
    defaults: new { controller = "Blog", action = "ReadArticle" });
```

Esse modelo corresponde a um caminho de URL, como `/Blog/All-About-Routing/Introduction`, e extrai os valores `{ controller = Blog, action = ReadArticle, article = All-About-Routing/Introduction }`. Os valores de rota padrão para `controller` e `action` são produzidos pela rota, mesmo que não haja nenhum parâmetro de rota correspondente no modelo. Os valores padrão podem ser especificados no modelo de rota. O parâmetro de rota `article` é definido como um *catch-all* pela aparência de um asterisco `*` antes do nome do parâmetro de rota. Os parâmetros de rota catch-all capturam o restante do caminho de URL e também podem corresponder à cadeia de caracteres vazia.

Este exemplo adiciona restrições de rota e tokens de dados:

```csharp
routes.MapRoute(
    name: "us_english_products",
    template: "en-US/Products/{id}",
    defaults: new { controller = "Products", action = "Details" },
    constraints: new { id = new IntRouteConstraint() },
    dataTokens: new { locale = "en-US" });
```

Este modelo corresponde a um caminho de URL como `/en-US/Products/5` e extrai os valores de `{ controller = Products, action = Details, id = 5 }` e os tokens de dados `{ locale = en-US }`.

![Tokens locais do Windows](routing/_static/tokens.png)

### <a name="url-generation"></a>Geração de URL

A classe `Route` também pode executar a geração de URL pela combinação de um conjunto de valores de rota com seu modelo de rota. Logicamente, este é o processo inverso de correspondência do caminho de URL.

> [!TIP]
> Para entender melhor a geração de URL, imagine qual URL você deseja gerar e, em seguida, pense em como um modelo de rota corresponderia a essa URL. Quais valores serão produzidos? Este é o equivalente aproximado de como funciona a geração de URL na classe `Route`.

Este exemplo usa uma rota de estilo básica do ASP.NET Core MVC:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

Com os valores de rota `{ controller = Products, action = List }`, essa rota gera a URL `/Products/List`. Os valores de rota são substituídos pelos parâmetros de rota correspondentes para formar o caminho de URL. Como `id` é um parâmetro de rota opcional, não há problema algum se ele não tem um valor.

Com os valores de rota `{ controller = Home, action = Index }`, essa rota gera a URL `/`. Os valores de rota fornecidos correspondem aos valores padrão para que os segmentos correspondentes a esses valores possam ser omitidos com segurança. As duas URLs geradas fazem uma viagem de ida e volta com essa definição de rota e produzem os mesmos valores de rota que foram usados para gerar a URL.

> [!TIP]
> Um aplicativo que usa o ASP.NET Core MVC deve usar <xref:Microsoft.AspNetCore.Mvc.Routing.UrlHelper> para gerar URLs, em vez de chamar o roteamento diretamente.

Para saber mais sobre a geração de URL, confira [url-generation-reference](#url-generation-reference).

## <a name="use-routing-middleware"></a>Usar o middleware de roteamento

::: moniker range=">= aspnetcore-2.1"

Referencie o [metapacote Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) no arquivo de projeto do aplicativo.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Referencie o [metapacote Microsoft.AspNetCore.All](xref:fundamentals/metapackage) no arquivo de projeto do aplicativo.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Referencie o [Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/) no arquivo de projeto do aplicativo.

::: moniker-end

Adicione o roteamento ao contêiner de serviço em `Startup.ConfigureServices`:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](routing/samples/1.x/RoutingSample/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

::: moniker-end

As rotas precisam ser configuradas no método `Startup.Configure`. O aplicativo de exemplo usa estas APIs:

* `RouteBuilder`
* `Build`
* `MapGet` &ndash; Corresponde apenas às solicitações HTTP GET.
* `UseRouter`

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_RouteHandler)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](routing/samples/1.x/RoutingSample/Startup.cs?name=snippet_RouteHandler)]

::: moniker-end

A tabela a seguir mostra as respostas com os URIs fornecidos.

| URI                  | Resposta                                          |
| -------------------- | ------------------------------------------------- |
| /package/create/3    | Olá! Valores de rota: [operation, create], [id, 3] |
| /package/track/-3    | Olá! Valores de rota: [operation, track], [id, -3] |
| /package/track/-3/   | Olá! Valores de rota: [operation, track], [id, -3] |
| /package/track/      | &lt;Fall through, no match&gt;                    |
| GET /hello/Joe       | Olá, Joe!                                          |
| POST /hello/Joe      | &lt;Fall through, matches HTTP GET only&gt;       |
| GET /hello/Joe/Smith | &lt;Fall through, no match&gt;                    |

Se você estiver configurando uma única rota, chame <xref:Microsoft.AspNetCore.Builder.RoutingBuilderExtensions.UseRouter*> passando uma instância de `IRouter`. Você não precisa usar <xref:Microsoft.AspNetCore.Routing.RouteBuilder>.

O framework fornece um conjunto de métodos de extensão para a criação de rotas, como:

* `MapRoute`
* `MapGet`
* `MapPost`
* `MapPut`
* `MapDelete`
* `MapVerb`

Alguns desses métodos, como `MapGet`, exigem que um `RequestDelegate` seja fornecido. O `RequestDelegate` é usado como o *manipulador de rotas* quando a rota corresponde. Outros métodos nesta família permitem configurar um pipeline de middleware a ser usado como o manipulador de rotas. Se o método *Map* não aceitar um manipulador, como `MapRoute`, ele usará o <xref:Microsoft.AspNetCore.Routing.RouteBuilder.DefaultHandler*>.

Os métodos `Map[Verb]` usam restrições para limitar a rota ao Verbo HTTP no nome do método. Por exemplo, veja <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*> e <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapVerb*>.

## <a name="route-template-reference"></a>Referência de modelo de rota

Os tokens entre chaves (`{ ... }`) definem os *parâmetros de rota* que serão associados se a rota for correspondida. Você pode definir mais de um parâmetro de rota em um segmento de rota, mas eles precisam ser separados por um valor literal. Por exemplo, `{controller=Home}{action=Index}` não é uma rota válida, já que não há nenhum valor literal entre `{controller}` e `{action}`. Esses parâmetros de rota precisam ter um nome e podem ter atributos adicionais especificados.

Um texto literal diferente dos parâmetros de rota (por exemplo, `{id}`) e do separador de caminho `/` precisa corresponder ao texto na URL. A correspondência de texto não diferencia maiúsculas de minúsculas e se baseia na representação decodificada do caminho de URLs. Para encontrar a correspondência do delimitador de parâmetro de rota literal `{` ou `}`, faça seu escape repetindo o caractere (`{{` ou `}}`).

Padrões de URL que tentam capturar um nome de arquivo com uma extensão de arquivo opcional apresentam considerações adicionais. Por exemplo, considere o modelo `files/{filename}.{ext?}`. Quando `filename` e `ext` existem, ambos os valores são populados. Se apenas `filename` existir na URL, a rota encontrará a correspondência, pois o ponto à direita `.` é opcional. As URLs a seguir correspondem a essa rota:

* `/files/myFile.txt`
* `/files/myFile`

Você pode usar o caractere `*` como um prefixo para um parâmetro de rota a ser associado ao restante do URI. Isso é chamado de parâmetro *catch-all*. Por exemplo, `blog/{*slug}` corresponde a qualquer URI que começa com `/blog` e tem qualquer valor depois dele (que é atribuído ao valor de rota `slug`). Os parâmetros catch-all também podem corresponder à cadeia de caracteres vazia.

::: moniker range=">= aspnetcore-2.2"

O parâmetro catch-all faz o escape dos caracteres corretos quando a rota é usada para gerar uma URL, incluindo os caracteres separadores de caminho (`/`). Por exemplo, a rota `foo/{*path}` com valores de rota `{ path = "my/path" }` gera `foo/my%2Fpath`. Observe o escape da barra invertida. Para fazer a viagem de ida e volta dos caracteres separadores de caminho, use o prefixo do parâmetro da rota `**`. A rota `foo/{**path}` com `{ path = "my/path" }` gera `foo/my/path`.

::: moniker-end

Os parâmetros de rota podem ter *valores padrão*, designados pela especificação do padrão após o nome do parâmetro, separados por um sinal de igual (`=`). Por exemplo, `{controller=Home}` define `Home` como o valor padrão de `controller`. O valor padrão é usado se nenhum valor está presente na URL para o parâmetro. Além dos valores padrão, os parâmetros de rota podem ser opcionais, especificados ao acrescentar um ponto de interrogação (`?`) ao final do nome do parâmetro, como em `id?`. A diferença entre os parâmetro de rota de valores opcionais e os padrão é que um parâmetro de rota com um valor padrão sempre produz um valor e um parâmetro opcional somente tem um valor quando ele é fornecido pela URL de solicitação.

::: moniker range=">= aspnetcore-2.2"

Os parâmetros de rota podem ter restrições, que precisam corresponder ao valor de rota associado da URL. A adição de dois-pontos (`:`) e do nome da restrição após o nome do parâmetro de rota especifica uma *restrição embutida* em um parâmetro de rota. Se a restrição exigir argumentos, eles ficarão entre parênteses `( )` após o nome da restrição. Várias restrições embutidas podem ser especificadas por meio do acréscimo de outros dois-pontos (`:`) e do nome da restrição. O nome da restrição e os argumentos são passados para o serviço <xref:Microsoft.AspNetCore.Routing.IInlineConstraintResolver> para criar uma instância de <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> a ser usada no processamento de URL. Se o construtor de restrições exigir serviços, eles serão resolvidos nos serviços de aplicativos da injeção da dependência. Por exemplo, o modelo de rota `blog/{article:minlength(10)}` especifica uma restrição `minlength` com o argumento `10`. Para obter mais informações sobre as restrições de rota e uma listagem das restrições fornecidas pela estrutura, confira a seção [Referência de restrição de rota](#route-constraint-reference).

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Os parâmetros de rota podem ter restrições, que precisam corresponder ao valor de rota associado da URL. A adição de dois-pontos (`:`) e do nome da restrição após o nome do parâmetro de rota especifica uma *restrição embutida* em um parâmetro de rota. Se a restrição exigir argumentos, eles ficarão entre parênteses `( )` após o nome da restrição. Várias restrições embutidas podem ser especificadas por meio do acréscimo de outros dois-pontos (`:`) e do nome da restrição. O nome da restrição e os argumentos são passados para o serviço <xref:Microsoft.AspNetCore.Routing.IInlineConstraintResolver> para criar uma instância de <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> a ser usada no processamento de URL. Por exemplo, o modelo de rota `blog/{article:minlength(10)}` especifica uma restrição `minlength` com o argumento `10`. Para obter mais informações sobre as restrições de rota e uma listagem das restrições fornecidas pela estrutura, confira a seção [Referência de restrição de rota](#route-constraint-reference).

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

Os parâmetros de rota também podem ter transformadores de parâmetro, que transformam o valor de um parâmetro ao gerar links e combinar ações e páginas com URIs. Assim como as restrições, os transformadores de parâmetro podem ser adicionados embutidos a um parâmetro de rota colocando dois-pontos (`:`) e o nome do transformador após o nome do parâmetro de rota. Por exemplo, o modelo de rota `blog/{article:slugify}` especifica um transformador `slugify`.

::: moniker-end

A tabela a seguir demonstra alguns modelos de rota e seu comportamento.

| Modelo de rota                         | URL de correspondência de exemplo  | Observações                                                                  |
| -------------------------------------- | --------------------- | ---------------------------------------------------------------------- |
| hello                                  | /hello                | Somente corresponde ao caminho único `/hello`                                  |
| {Page=Home}                            | /                     | Faz a correspondência e define `Page` como `Home`                                      |
| {Page=Home}                            | /Contact              | Faz a correspondência e define `Page` como `Contact`                                   |
| {controller}/{action}/{id?}            | /Products/List        | É mapeado para o controlador `Products` e a ação `List`                       |
| {controller}/{action}/{id?}            | /Products/Details/123 |  É mapeado para o controlador `Products` e a ação `Details`.  `id` definido como 123 |
| {controller=Home}/{action=Index}/{id?} | /                     |  É mapeado para o controlador `Home` e o método `Index`; `id` é ignorado.       |

Em geral, o uso de um modelo é a abordagem mais simples para o roteamento. Restrições e padrões também podem ser especificados fora do modelo de rota.

> [!TIP]
> Habilite o [Log](xref:fundamentals/logging/index) para ver como as implementações de roteamento internas, como `Route`, correspondem às solicitações.

## <a name="reserved-routing-names"></a>Nomes reservados de roteamento

As seguintes palavras-chave são nomes reservados e não podem ser usadas como nomes de rota ou parâmetros:

* `action`
* `area`
* `controller`
* `handler`
* `page`

## <a name="route-constraint-reference"></a>Referência de restrição de rota

As restrições da rota são executadas quando uma `Route` correspondeu à sintaxe da URL de entrada e criou um token do caminho de URL para valores de rota. Em geral, as restrições da rota inspecionam o valor de rota associado por meio do modelo de rota e tomam uma decisão do tipo "sim/não" sobre se o valor é aceitável ou não. Algumas restrições da rota usam dados fora do valor de rota para considerar se a solicitação pode ser encaminhada. Por exemplo, a <xref:Microsoft.AspNetCore.Routing.Constraints.HttpMethodRouteConstraint> pode aceitar ou rejeitar uma solicitação de acordo com o verbo HTTP.

> [!WARNING]
> Evite usar restrições para **validação de entrada** porque isso significa que uma entrada inválida resultará em um *404 – Não Encontrado* e não em um *400 – Solicitação Inválida* com uma mensagem de erro apropriada. As restrições de rota são usadas para **desfazer a ambiguidade** entre rotas semelhantes, não para validar as entradas de uma rota específica.

A tabela a seguir demonstra algumas restrições de rota e seu comportamento esperado.

| restrição | Exemplo | Correspondências de exemplo | Observações |
| ---------- | ------- | --------------- | ----- |
| `int` | `{id:int}` | `123456789`, `-123456789`  | Corresponde a qualquer inteiro |
| `bool` | `{active:bool}` | `true`, `FALSE` | Corresponde a `true` ou `false` (não diferencia maiúsculas de minúsculas) |
| `datetime` | `{dob:datetime}` | `2016-12-31`, `2016-12-31 7:32pm`  | Corresponde a um valor `DateTime` válido (na cultura invariável – veja o aviso) |
| `decimal` | `{price:decimal}` | `49.99`, `-1,000.01` | Corresponde a um valor `decimal` válido (na cultura invariável – veja o aviso) |
| `double` | `{weight:double}` | `1.234`, `-1,001.01e8` | Corresponde a um valor `double` válido (na cultura invariável – veja o aviso) |
| `float` | `{weight:float}` | `1.234`, `-1,001.01e8` | Corresponde a um valor `float` válido (na cultura invariável – veja o aviso) |
| `guid` | `{id:guid}` | `CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}` | Corresponde a um valor `Guid` válido |
| `long` | `{ticks:long}` | `123456789`, `-123456789` | Corresponde a um valor `long` válido |
| `minlength(value)` | `{username:minlength(4)}` | `Rick` | A cadeia de caracteres deve ter, no mínimo, 4 caracteres |
| `maxlength(value)` | `{filename:maxlength(8)}` | `Richard` | A cadeia de caracteres não pode ser maior que 8 caracteres |
| `length(length)` | `{filename:length(12)}` | `somefile.txt` | A cadeia de caracteres deve ter exatamente 12 caracteres |
| `length(min,max)` | `{filename:length(8,16)}` | `somefile.txt` | A cadeia de caracteres deve ter, pelo menos, 8 e não mais de 16 caracteres |
| `min(value)` | `{age:min(18)}` | `19` | O valor inteiro deve ser, pelo menos, 18 |
| `max(value)` | `{age:max(120)}` | `91` | O valor inteiro não deve ser maior que 120 |
| `range(min,max)` | `{age:range(18,120)}` | `91` | O valor inteiro deve ser, pelo menos, 18, mas não maior que 120 |
| `alpha` | `{name:alpha}` | `Rick` | A cadeia de caracteres deve consistir em um ou mais caracteres alfabéticos (`a`-`z`, não diferencia maiúsculas de minúsculas) |
| `regex(expression)` | `{ssn:regex(^\\d{{3}}-\\d{{2}}-\\d{{4}}$)}` | `123-45-6789` | A cadeia de caracteres deve corresponder à expressão regular (veja as dicas sobre como definir uma expressão regular) |
| `required` | `{name:required}` | `Rick` |  Usado para impor que um valor não parâmetro está presente durante a geração de URL |

Várias restrições delimitadas por vírgula podem ser aplicadas a um único parâmetro. Por exemplo, a restrição a seguir restringe um parâmetro para um valor inteiro de 1 ou maior:

```csharp
[Route("users/{id:int:min(1)}")]
public User GetUserById(int id) { }
```

> [!WARNING]
> As restrições de rota que verificam a URL e são convertidas em um tipo CLR (como `int` ou `DateTime`) sempre usam a cultura invariável. Essas restrições consideram que a URL não é localizável. As restrições de rota fornecidas pela estrutura não modificam os valores armazenados nos valores de rota. Todos os valores de rota analisados com base na URL são armazenados como cadeias de caracteres. Por exemplo, a restrição `float` tenta converter o valor de rota em um float, mas o valor convertido é usado somente para verificar se ele pode ser convertido em um float.

## <a name="regular-expressions"></a>Expressões regulares

A estrutura do ASP.NET Core adiciona `RegexOptions.IgnoreCase | RegexOptions.Compiled | RegexOptions.CultureInvariant` ao construtor de expressão regular. Confira <xref:System.Text.RegularExpressions.RegexOptions> para obter uma descrição desses membros.

As expressões regulares usam delimitadores e tokens semelhantes aos usados pelo Roteamento e pela linguagem C#. Os tokens de expressão regular precisam ter escape. Para usar a expressão regular `^\d{3}-\d{2}-\d{4}$` no Roteamento, ela precisa ter os caracteres `\` digitados como `\\` no arquivo de origem C# para fazer o escape do caractere de escape da cadeia de caracteres `\` (a menos que [literais de cadeia de caracteres textuais](/dotnet/csharp/language-reference/keywords/string) sejam usados). Os caracteres `{`, `}`, `[` e `]` precisam ser inseridos duas vezes para fazer o escape dos caracteres delimitadores do parâmetro de Roteamento. A tabela abaixo mostra uma expressão regular e a versão com escape.

| Expressão            | Com escape                        |
| --------------------- | ------------------------------ |
| `^\d{3}-\d{2}-\d{4}$` | `^\\d{{3}}-\\d{{2}}-\\d{{4}}$` |
| `^[a-z]{2}$`          | `^[[a-z]]{{2}}$`               |

As expressões regulares usadas no roteamento geralmente começam com o caractere `^` (corresponde à posição inicial da cadeia de caracteres) e terminam com o caractere `$` (corresponde à posição final da cadeia de caracteres). Os caracteres `^` e `$` garantem que a expressão regular corresponde a todo o valor do parâmetro de rota. Sem os caracteres `^` e `$`, a expressão regular corresponde a qualquer subcadeia de caracteres na cadeia de caracteres, o que geralmente não é o desejado. A tabela abaixo mostra alguns exemplos e explica por que eles encontram ou não uma correspondência.

| Expressão   | Cadeia de Caracteres    | Corresponder a | Comentário               |
| ------------ | --------- |  ---- |  -------------------- |
| `[a-z]{2}`   | hello     | Sim   | A subcadeia de caracteres corresponde     |
| `[a-z]{2}`   | 123abc456 | Sim   | A subcadeia de caracteres corresponde     |
| `[a-z]{2}`   | mz        | Sim   | Corresponde à expressão    |
| `[a-z]{2}`   | MZ        | Sim   | Não diferencia maiúsculas de minúsculas    |
| `^[a-z]{2}$` |  hello    | Não    | Confira `^` e `$` acima |
| `^[a-z]{2}$` | 123abc456 | Não    | Confira `^` e `$` acima |

Para saber mais sobre a sintaxe de expressões regulares, confira [Expressões regulares do .NET Framework](/dotnet/standard/base-types/regular-expression-language-quick-reference).

Para restringir um parâmetro a um conjunto conhecido de valores possíveis, use uma expressão regular. Por exemplo, `{action:regex(^(list|get|create)$)}` apenas corresponde o valor da rota `action` a `list`, `get` ou `create`. Se passada para o dicionário de restrições, a cadeia de caracteres `^(list|get|create)$` é equivalente. As restrições passadas para o dicionário de restrições (não embutidas em um modelo) que não correspondem a uma das restrições conhecidas também são tratadas como expressões regulares.

::: moniker range=">= aspnetcore-2.2"

## <a name="parameter-transformer-reference"></a>Referência de parâmetro de transformador

Transformadores de parâmetro:

* Executar ao gerar um link para um `Route`.
* Implementar `Microsoft.AspNetCore.Routing.IOutboundParameterTransformer`.
* São configurados usando <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap>.
* Usam o valor de rota do parâmetro e o transformam em um novo valor de cadeia de caracteres.
* O valor transformado é usado no link gerado.

Por exemplo, um transformador de parâmetro `slugify` personalizado em padrão de rota `blog\{article:slugify}` com `Url.Action(new { article = "MyTestArticle" })` gera `blog\my-test-article`.

Os transformadores de parâmetro também são usados pelas estruturas para transformar o URI em que o ponto de extremidade é resolvido. Por exemplo, o ASP.NET Core MVC usa os transformadores de parâmetro para transformar o valor de rota usado para corresponder a um `area`, `controller`, `action` e `page`.

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home:slugify}/{action=Index:slugify}/{id?}");
```

Com a rota anterior, a ação `SubscriptionManagementController.GetAll()` é combinada com o URI `/subscription-management/get-all`. Um transformador de parâmetro não altera os valores de rota usados para gerar um link. `Url.Action("GetAll", "SubscriptionManagement")` gera `/subscription-management/get-all`.

ASP.NET Core fornece convenções de API para usar transformadores de parâmetro com as rotas geradas:

* ASP.NET Core MVC tem a convenção de API `Microsoft.AspNetCore.Mvc.ApplicationModels.RouteTokenTransformerConvention`. Essa convenção aplica um transformador de parâmetro especificado a todas as rotas de atributo no aplicativo. O transformador de parâmetro transforma os tokens de rota do atributo conforme elas são substituídas. Para obter mais informações, confira [Usar um transformador de parâmetro para personalizar a substituição de token](/aspnet/core/mvc/controllers/routing#use-a-parameter-transformer-to-customize-token-replacement).
* Razor Pages têm a convenção de API `Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteTransformerConvention`. Essa convenção aplica-se um transformador de parâmetro especificado para todas as Razor Pages descobertas automaticamente. O transformador de parâmetro transforma a pasta e segmentos de nome de arquivo de rotas de página do Razor. Para obter mais informações, confira [Usar um transformador de parâmetros para personalizar rotas de página](/aspnet/core/razor-pages/razor-pages-conventions#use-a-parameter-transformer-to-customize-page-routes).

::: moniker-end

## <a name="url-generation-reference"></a>Referência de geração de URL

O exemplo a seguir mostra como gerar um link para uma rota com base em um dicionário de valores de rota e em um <xref:Microsoft.AspNetCore.Routing.RouteCollection>.

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_Dictionary)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](routing/samples/1.x/RoutingSample/Startup.cs?name=snippet_Dictionary)]

::: moniker-end

O `VirtualPath` gerado no final do exemplo anterior é `/package/create/123`. O dicionário fornece os valores de rota `operation` e `id` do modelo "Rastrear rota do pacote", `package/{operation}/{id}`. Para obter detalhes, consulte o código de exemplo na seção [Usar o middleware de roteamento](#use-routing-middleware) ou no [aplicativo de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/routing/samples).

O segundo parâmetro para o construtor `VirtualPathContext` é uma coleção de *valores de ambiente*. Os valores de ambiente proporcionam conveniência, limitando o número de valores que um desenvolvedor precisa especificar em determinado contexto de solicitação. Os valores de rota atuais da solicitação atual são considerados valores de ambiente para a geração de link. Em um aplicativo ASP.NET Core MVC, se você está na ação `About` do `HomeController`, não é necessário especificar o valor de rota do controlador a ser vinculado à ação `Index`. O valor de ambiente `Home` é usado.

Os valores de ambiente que não correspondem a um parâmetro são ignorados e os valores de ambiente também são ignorados quando um valor fornecido de forma explícita o substitui, seguindo da esquerda para a direita na URL.

Os valores que são fornecidos de forma explícita, mas que não correspondem a nada, são adicionados à cadeia de caracteres de consulta. A tabela a seguir mostra o resultado do uso do modelo de rota `{controller}/{action}/{id?}`.

| Valores de ambiente                | Valores explícitos                   | Resultado                  |
| ----------------------------- | --------------------------------- | ----------------------- |
| controller="Home"             | action="About"                    | `/Home/About`           |
| controller="Home"             | controller="Order",action="About" | `/Order/About`          |
| controller="Home",color="Red" | action="About"                    | `/Home/About`           |
| controller="Home"             | action="About",color="Red"        | `/Home/About?color=Red` |

Se uma rota tem um valor padrão que não corresponde a um parâmetro e esse valor é fornecido de forma explícita, ele precisa corresponder ao valor padrão:

```csharp
routes.MapRoute("blog_route", "blog/{*slug}",
    defaults: new { controller = "Blog", action = "ReadPost" });
```

A geração de link somente gera um link para essa rota quando os valores correspondentes do controlador e da ação são fornecidos.

---
title: Roteamento no ASP.NET Core
author: ardalis
description: Descubra como a funcionalidade de roteamento do ASP.NET Core é responsável por mapear uma solicitação de entrada para um manipulador de rotas.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/routing
ms.openlocfilehash: 2e1257639ec41f657093439c5245b50adbad34dc
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/03/2018
---
# <a name="routing-in-aspnet-core"></a>Roteamento no ASP.NET Core

Por [Ryan Nowak](https://github.com/rynowak), [Steve Smith](https://ardalis.com/) e [Rick Anderson](https://twitter.com/RickAndMSFT)

A funcionalidade de roteamento é responsável por mapear uma solicitação de entrada para um manipulador de rotas. As rotas são definidas no aplicativo ASP.NET e configuradas quando o aplicativo é iniciado. Uma rota pode opcionalmente extrair os valores da URL contida na solicitação e esses valores podem então ser usados para o processamento da solicitação. Usando as informações de rota do aplicativo ASP.NET, a funcionalidade de roteamento também pode gerar URLs que são mapeadas para manipuladores de rotas. Portanto, o roteamento pode encontrar um manipulador de rotas com base em uma URL ou a URL correspondente a um manipulador de rotas especificado com base nas informações do manipulador de rotas.

>[!IMPORTANT]
> Este documento abrange o roteamento de nível inferior do ASP.NET Core. Para o roteamento do ASP.NET Core MVC, confira [Rotear para ações do controlador](../mvc/controllers/routing.md)

[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/routing/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))

## <a name="routing-basics"></a>Conceitos básicos sobre roteamento

O roteamento usa *rotas* (implementações de [IRouter](/dotnet/api/microsoft.aspnetcore.routing.irouter)) para:

* mapear solicitações de entrada para *manipuladores de rotas*

* gerar as URLs usadas nas respostas

Em geral, um aplicativo tem uma única coleção de rotas. Quando uma solicitação é recebida, a coleção de rotas é processada na ordem. A solicitação de entrada procura uma rota que corresponde à URL de solicitação chamando o método `RouteAsync` em cada rota disponível na coleção de rotas. Por outro lado, uma resposta pode usar o roteamento para gerar URLs (por exemplo, para redirecionamento ou links) com base nas informações de rotas e evitar a necessidade de embutir as URLs em código, o que ajuda a facilidade de manutenção.

O roteamento está conectado ao pipeline do [middleware](xref:fundamentals/middleware/index) pela classe `RouterMiddleware`. O [ASP.NET Core MVC](xref:mvc/overview) adiciona o roteamento ao pipeline do middleware como parte de sua configuração. Para saber mais sobre como usar o roteamento como um componente autônomo, confira [Usando o middleware de roteamento](#using-routing-middleware).

<a name="url-matching-ref"></a>

### <a name="url-matching"></a>Correspondência de URL

Correspondência de URL é o processo pelo qual o roteamento expede uma solicitação de entrada para um *manipulador*. Esse processo geralmente se baseia nos dados do caminho da URL, mas pode ser estendido para considerar outros dados na solicitação. A capacidade de expedir solicitações para manipuladores separados é fundamental no dimensionamento do tamanho e da complexidade de um aplicativo.

As solicitações de entrada entram no `RouterMiddleware`, que chama o método `RouteAsync` em cada rota na sequência. A instância `IRouter` escolhe se deseja *manipular* a solicitação definindo o `RouteContext.Handler` como `RequestDelegate` não nulo. Se uma rota definir um manipulador para a solicitação, o processamento de rotas será interrompido e o manipulador será invocado para processar a solicitação. Se todas as rotas forem testadas e nenhum manipulador for encontrado para a solicitação, o middleware chamará o *próximo* e o próximo middleware no pipeline da solicitação será invocado.

A entrada primária para `RouteAsync` é o `RouteContext.HttpContext` associado à solicitação atual. O `RouteContext.Handler` e `RouteContext.RouteData` são saídas que serão definidas após a correspondência de uma rota.

Uma correspondência durante `RouteAsync` também definirá as propriedades dos `RouteContext.RouteData` com os valores apropriados de acordo com o processamento da solicitação concluído até o momento. Se uma rota corresponder a uma solicitação, os `RouteContext.RouteData` conterão informações de estado importantes sobre o *resultado*.

`RouteData.Values` é um dicionário de *valores de rota* produzidos com base na rota. Esses valores geralmente são determinados pela criação de tokens da URL e podem ser usados para aceitar a entrada do usuário ou para tomar outras decisões de expedição dentro do aplicativo.

`RouteData.DataTokens` é um recipiente de propriedades de dados adicionais relacionados à rota que teve uma correspondência. `DataTokens` são fornecidos para dar suporte à associação dos dados de estado a cada rota, de modo que o aplicativo possa tomar decisões posteriormente com base em qual rota teve uma correspondência. Esses valores são definidos pelo desenvolvedor e **não** afetam de forma alguma o comportamento do roteamento. Além disso, os valores armazenados em stash nos tokens de dados podem ser de qualquer tipo, ao contrário dos valores de rota, quem devem ser conversíveis com facilidade bidirecionalmente em cadeias de caracteres.

`RouteData.Routers` é uma lista das rotas que participaram da correspondência bem-sucedida da solicitação. As rotas podem ser aninhadas dentro uma da outra e a propriedade `Routers` reflete o caminho pela árvore lógica de rotas que resultou em uma correspondência. Geralmente, o primeiro item em `Routers` é a coleção de rotas e deve ser usado para a geração de URL. O último item em `Routers` é o manipulador de rotas que teve uma correspondência.

### <a name="url-generation"></a>Geração de URL

Geração de URL é o processo pelo qual o roteamento pode criar um caminho de URL de acordo com um conjunto de valores de rota. Isso permite uma separação lógica entre os manipuladores e as URLs que os acessam.

A geração de URL segue um processo iterativo semelhante, mas começa com a chamada pelo código de estrutura ou usuário ao método `GetVirtualPath` da coleção de rotas. Em seguida, cada *rota* terá seu método `GetVirtualPath` chamado na sequência, até que um `VirtualPathData` não nulo seja retornado.

As entradas primárias para `GetVirtualPath` são:

* `VirtualPathContext.HttpContext`

* `VirtualPathContext.Values`

* `VirtualPathContext.AmbientValues`

As rotas usam principalmente os valores de rota fornecidos pelo `Values` e `AmbientValues` para decidir em que local é possível gerar uma URL e quais valores serão incluídos. Os `AmbientValues` são o conjunto de valores de rota produzidos pela correspondência da solicitação atual com o sistema de roteamento. Por outro lado, `Values` são os valores de rota que especificam como gerar a URL desejada para a operação atual. O `HttpContext` é fornecido no caso de uma rota precisar obter serviços ou dados adicionais associados ao contexto atual.

Dica: considere `Values` como sendo um conjunto de substituições para os `AmbientValues`. A geração de URL tenta reutilizar os valores de rota da solicitação atual para facilitar a geração de URLs para links usando a mesma rota ou valores de rota.

A saída de `GetVirtualPath` é um `VirtualPathData`. `VirtualPathData` é um paralelo do `RouteData`; ele contém o `VirtualPath` para a URL de saída, bem como algumas propriedades adicionais que devem ser definidas pela rota.

A propriedade `VirtualPathData.VirtualPath` contém o *caminho virtual* produzido pela rota. Dependendo de suas necessidades, talvez você precise processar ainda mais o caminho. Por exemplo, se você deseja renderizar a URL gerada em HTML, precisa preceder o caminho base do aplicativo.

O `VirtualPathData.Router` é uma referência à rota que é gerou a URL com êxito.

As propriedades `VirtualPathData.DataTokens` são um dicionário de dados adicionais relacionados à rota que gerou a URL. Isso é o paralelo de `RouteData.DataTokens`.

### <a name="creating-routes"></a>Criando rotas

O roteamento fornece a classe `Route` como a implementação padrão de `IRouter`. `Route` usa a sintaxe de *modelo de rota* para definir padrões que corresponderão ao caminho de URL quando `RouteAsync` for chamado. `Route` usará o mesmo modelo de rota para gerar uma URL quando `GetVirtualPath` for chamado.

A maioria dos aplicativos criará rotas chamando `MapRoute` ou um dos métodos de extensão semelhante definidos em `IRouteBuilder`. Todos esses métodos criarão uma instância de `Route` e a adicionarão à coleção de rotas.

Observação: `MapRoute` não usa um parâmetro de manipulador de rotas – ele apenas adiciona rotas que serão manipuladas pelo `DefaultHandler`. Como o manipulador padrão é um `IRouter`, ele pode optar por não manipular a solicitação. Por exemplo, o ASP.NET MVC normalmente é configurado como um manipulador padrão que só manipula as solicitações que correspondem a um controlador e uma ação disponíveis. Para saber mais sobre o roteamento para o MVC, confira [Rotear para ações do controlador](../mvc/controllers/routing.md).

Este é um exemplo de uma chamada `MapRoute` usada por uma definição de rota típica do ASP.NET MVC:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

Esse modelo corresponderá a um caminho de URL como `/Products/Details/17` e extrairá os valores de rota `{ controller = Products, action = Details, id = 17 }`. Os valores de rota são determinados pela divisão do caminho de URL em segmentos e pela correspondência de cada segmento com o nome do *parâmetro de rota* do modelo de rota. Os parâmetros de rota são nomeados. Eles são definidos com a colocação do nome do parâmetro em chaves `{ }`.

O modelo acima também pode corresponder ao caminho de URL `/` e produzirá valores `{ controller = Home, action = Index }`. Isso ocorre porque os parâmetros de rota `{controller}` e `{action}` têm valores padrão e o parâmetro de rota `id` é opcional. Um sinal de igual `=` seguido de um valor após o nome do parâmetro de rota define um valor padrão para o parâmetro. Um ponto de interrogação `?` após o nome do parâmetro de rota define o parâmetro como opcional. Parâmetros de rota com um valor padrão *sempre* produzem um valor de rota quando a rota encontra uma correspondência – os parâmetros opcionais não produzem um valor de rota se não há nenhum segmento de caminho de URL correspondente.

Consulte [route-template-reference](#route-template-reference) para obter uma descrição completa dos recursos e da sintaxe de modelo de rota.

Este exemplo inclui uma *restrição de rota*:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id:int}");
```

Este modelo corresponderá a um caminho de URL como `/Products/Details/17`, mas não a `/Products/Details/Apples`. A definição do parâmetro de rota `{id:int}` define uma *restrição de rota* para o parâmetro de rota `id`. As restrições de rota implementam `IRouteConstraint` e inspecionam valores de rota para verificá-los. Neste exemplo, o valor de rota `id` precisa ser conversível em um inteiro. Consulte [route-constraint-reference](#route-constraint-reference) para obter uma explicação mais detalhada das restrições de rota que são fornecidas pela estrutura.

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

Dica: a sintaxe embutida para definição de restrições e padrões pode ser mais conveniente para rotas simples. No entanto, existem recursos, como tokens de dados, que não têm suporte na sintaxe embutida.

Este exemplo demonstra alguns outros recursos:

```csharp
routes.MapRoute(
  name: "blog",
  template: "Blog/{*article}",
  defaults: new { controller = "Blog", action = "ReadArticle" });
```

Esse modelo corresponderá a um caminho de URL como `/Blog/All-About-Routing/Introduction` e extrairá os valores `{ controller = Blog, action = ReadArticle, article = All-About-Routing/Introduction }`. Os valores de rota padrão para `controller` e `action` são produzidos pela rota, mesmo que não haja nenhum parâmetro de rota correspondente no modelo. Os valores padrão podem ser especificados no modelo de rota. O parâmetro de rota `article` é definido como um *catch-all* pela aparência de um asterisco `*` antes do nome do parâmetro de rota. Os parâmetros de rota catch-all capturam o restante do caminho de URL e também podem corresponder à cadeia de caracteres vazia.

Este exemplo adiciona restrições de rota e tokens de dados:

```csharp
routes.MapRoute(
    name: "us_english_products",
    template: "en-US/Products/{id}",
    defaults: new { controller = "Products", action = "Details" },
    constraints: new { id = new IntRouteConstraint() },
    dataTokens: new { locale = "en-US" });
```

Esse modelo corresponderá a um caminho de URL como `/Products/5` e extrairá os valores `{ controller = Products, action = Details, id = 5 }` e os tokens de dados `{ locale = en-US }`.

![Tokens locais do Windows](routing/_static/tokens.png)

<a name="id1"></a>

### <a name="url-generation"></a>Geração de URL

A classe `Route` também pode executar a geração de URL pela combinação de um conjunto de valores de rota com seu modelo de rota. Logicamente, este é o processo inverso de correspondência do caminho de URL.

Dica: para entender melhor a geração de URL, imagine qual URL você deseja gerar e, em seguida, considere como um modelo de rota corresponderá a essa URL. Quais valores serão produzidos? Este é o equivalente aproximado de como funciona a geração de URL na classe `Route`.

Este exemplo usa uma rota de estilo básica do ASP.NET MVC:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

Com os valores de rota `{ controller = Products, action = List }`, essa rota gerará a URL `/Products/List`. Os valores de rota são substituídos pelos parâmetros de rota correspondentes para formar o caminho de URL. Como `id` é um parâmetro de rota opcional, não há problema algum se ele não tem um valor.

Com os valores de rota `{ controller = Home, action = Index }`, essa rota gerará a URL `/`. Os valores de rota que foram fornecidos correspondem aos valores padrão, de modo que os segmentos correspondentes a esses valores possam ser omitidos com segurança. Observe que as duas URLs geradas farão uma ida e vinda com essa definição de rota e produzirão os mesmos valores de rota que foram usados para gerar a URL.

Dica: um aplicativo que usa o ASP.NET MVC deve usar `UrlHelper` para gerar URLs, em vez de chamar o roteamento diretamente.

Para obter mais detalhes sobre o processo de geração de URL, consulte [url-generation-reference](#url-generation-reference).

## <a name="using-routing-middleware"></a>Usando o middleware de roteamento

Adicione o pacote NuGet "Microsoft.AspNetCore.Routing".

Adicione o roteamento ao contêiner de serviço em *Startup.cs*:

[!code-csharp[](../fundamentals/routing/sample/RoutingSample/Startup.cs?highlight=3&start=11&end=14)]

As rotas precisam ser configuradas no método `Configure` na classe `Startup`. A amostra abaixo usa essas APIs:

* `RouteBuilder`
* `Build`
* `MapGet` Faz a correspondência apenas de solicitações HTTP GET
* `UseRouter`

```csharp
public void Configure(IApplicationBuilder app, ILoggerFactory loggerFactory)
{
    var trackPackageRouteHandler = new RouteHandler(context =>
    {
        var routeValues = context.GetRouteData().Values;
        return context.Response.WriteAsync(
            $"Hello! Route values: {string.Join(", ", routeValues)}");
    });

    var routeBuilder = new RouteBuilder(app, trackPackageRouteHandler);

    routeBuilder.MapRoute(
        "Track Package Route",
        "package/{operation:regex(^(track|create|detonate)$)}/{id:int}");

    routeBuilder.MapGet("hello/{name}", context =>
    {
        var name = context.GetRouteValue("name");
        // This is the route handler when HTTP GET "hello/<anything>"  matches
        // To match HTTP GET "hello/<anything>/<anything>,
        // use routeBuilder.MapGet("hello/{*name}"
        return context.Response.WriteAsync($"Hi, {name}!");
    });

    var routes = routeBuilder.Build();
    app.UseRouter(routes);
}
```

A tabela abaixo mostra as respostas com os URIs fornecidos.

| URI | Resposta  |
| ------- | -------- |
| /package/create/3  | Olá! Valores de rota: [operation, create], [id, 3] |
| /package/track/-3  | Olá! Valores de rota: [operation, track], [id, -3] |
| /package/track/-3/ | Olá! Valores de rota: [operation, track], [id, -3]  |
| /package/track/ | \<São passados, nenhuma correspondência> |
| GET /hello/Joe | Olá, Joe! |
| POST /hello/Joe | \<São passados, corresponde apenas a HTTP GET> |
| GET /hello/Joe/Smith | \<São passados, nenhuma correspondência> |

Se estiver configurando uma única rota, chame `app.UseRouter` passando uma instância `IRouter`. Você não precisará chamar `RouteBuilder`.

A estrutura fornece um conjunto de métodos de extensão para criar rotas, como:

* `MapRoute`
* `MapGet`
* `MapPost`
* `MapPut`
* `MapDelete`
* `MapVerb`

Alguns desses métodos, como `MapGet`, exigem que um `RequestDelegate` seja fornecido. O `RequestDelegate` será usado como o *manipulador de rotas* quando a rota encontrar a correspondência. Outros métodos nesta família permitem configurar um pipeline do middleware que será usado como o manipulador de rotas. Se o método *Map* não aceitar um manipulador, como `MapRoute`, ele usará o `DefaultHandler`.

Os métodos `Map[Verb]` usam restrições para limitar a rota ao Verbo HTTP no nome do método. Por exemplo, consulte [MapGet](https://github.com/aspnet/Routing/blob/1.0.0/src/Microsoft.AspNetCore.Routing/RequestDelegateRouteBuilderExtensions.cs#L85-L88) e [MapVerb](https://github.com/aspnet/Routing/blob/1.0.0/src/Microsoft.AspNetCore.Routing/RequestDelegateRouteBuilderExtensions.cs#L156-L180).

## <a name="route-template-reference"></a>Referência de modelo de rota

Os tokens entre chaves (`{ }`) definem os *parâmetros de rota* que serão associados se for encontrada uma correspondência para a rota. Você pode definir mais de um parâmetro de rota em um segmento de rota, mas eles precisam ser separados por um valor literal. Por exemplo, `{controller=Home}{action=Index}` não será uma rota válida, porque não há nenhum valor literal entre `{controller}` e `{action}`. Esses parâmetros de rota precisam ter um nome e podem ter atributos adicionais especificados.

Um texto literal diferente dos parâmetros de rota (por exemplo, `{id}`) e do separador de caminho `/` precisa corresponder ao texto na URL. A correspondência de texto não diferencia maiúsculas de minúsculas e se baseia na representação decodificada do caminho de URLs. Para encontrar a correspondência do delimitador de parâmetro de rota literal `{` ou `}`, faça seu escape repetindo o caractere (`{{` ou `}}`).

Padrões de URL que tentam capturar um nome de arquivo com uma extensão de arquivo opcional apresentam considerações adicionais. Por exemplo, o uso do modelo `files/{filename}.{ext?}` – quando `filename` e `ext` existirem, os dois valores serão populados. Se apenas `filename` existir na URL, a rota encontrará a correspondência, pois o ponto à direita `.` é opcional. As seguintes URLs corresponderão a essa rota:

* `/files/myFile.txt`
* `/files/myFile.`
* `/files/myFile`

Use o caractere `*` como um prefixo para um parâmetro de rota para associá-lo ao restante do URI – isso é chamado de um parâmetro *catch-all*. Por exemplo, `blog/{*slug}` corresponderá a qualquer URI que começa com `/blog` e tem qualquer valor (que será atribuído ao valor de rota `slug`). Os parâmetros catch-all também podem corresponder à cadeia de caracteres vazia.

Os parâmetros de rota podem ter *valores padrão*, designados por meio da especificação do padrão após o nome do parâmetro, separado por um `=`. Por exemplo, `{controller=Home}` definirá `Home` como o valor padrão para `controller`. O valor padrão é usado se nenhum valor está presente na URL para o parâmetro. Além dos valores padrão, os parâmetros de rota podem ser opcionais (especificados por meio do acréscimo de um `?` ao final do nome do parâmetro, como em `id?`). A diferença entre opcional e "tem o padrão" é que um parâmetro de rota com um valor padrão sempre produz um valor; um parâmetro opcional tem um valor somente quando ele é fornecido.

Os parâmetros de rota também podem ter restrições, que precisam corresponder ao valor de rota associado da URL. A adição de dois-pontos `:` e do nome da restrição após o nome do parâmetro de rota especifica uma *restrição embutida* em um parâmetro de rota. Se a restrição exigir argumentos, eles serão fornecidos entre parênteses `( )` após o nome da restrição. Várias restrições embutidas podem ser especificadas por meio do acréscimo de outros dois-pontos `:` e do nome da restrição. O nome da restrição é passado para o serviço `IInlineConstraintResolver` para criar uma instância de `IRouteConstraint` a ser usada no processamento de URL. Por exemplo, o modelo de rota `blog/{article:minlength(10)}` especifica a restrição `minlength` com o argumento `10`. Para obter uma descrição mais detalhada das restrições de rota, bem como uma listagem das restrições fornecidas pela estrutura, consulte [route-constraint-reference](#route-constraint-reference).

A tabela a seguir demonstra alguns modelos de rota e seu comportamento.

| Modelo de rota | URL de correspondência de exemplo | Observações |
| -------- | -------- | ------- |
| hello  | /hello  | Somente corresponde ao caminho único `/hello` |
| {Page=Home} | / | Faz a correspondência e define `Page` como `Home` |
| {Page=Home}  | /Contact  | Faz a correspondência e define `Page` como `Contact` |
| {controller}/{action}/{id?} | /Products/List | É mapeado para o controlador `Products` e a ação `List` |
| {controller}/{action}/{id?} | /Products/Details/123  |  É mapeado para o controlador `Products` e a ação `Details`.  `id` definido como 123 |
| {controller=Home}/{action=Index}/{id?} | /  |  É mapeado para o controlador `Home` e o método `Index`; `id` é ignorado. |

Em geral, o uso de um modelo é a abordagem mais simples para o roteamento. Restrições e padrões também podem ser especificados fora do modelo de rota.

Dica: habilite o [Log](xref:fundamentals/logging/index) para ver como as implementações de roteamento internas, como `Route`, fazem a correspondência de solicitações.

## <a name="route-constraint-reference"></a>Referência de restrições de rota

As restrições da rota são executadas quando uma `Route` correspondeu à sintaxe da URL de entrada e criou um token do caminho de URL para valores de rota. Em geral, as restrições da rota inspecionam o valor de rota associado por meio do modelo de rota e tomam uma decisão simples do tipo "sim/não" sobre se o valor é aceitável ou não. Algumas restrições da rota usam dados fora do valor de rota para considerar se a solicitação pode ser encaminhada. Por exemplo, a `HttpMethodRouteConstraint` pode aceitar ou rejeitar uma solicitação de acordo com o verbo HTTP.

>[!WARNING]
> Evite usar restrições para **validação de entrada**, porque fazer isso significa que uma entrada inválida resultará em um 404 (Não Encontrado), em vez de um 400 com uma mensagem de erro apropriada. As restrições da rota devem ser usadas para **desfazer a ambiguidade** entre rotas semelhantes, não para validar as entradas de uma rota específica.

A tabela a seguir demonstra algumas restrições de rota e seu comportamento esperado.

| restrição | Exemplo | Correspondências de exemplo | Observações |
| --------   | ------- | ------------- | ----- |
| `int` | `{id:int}` | `123456789`, `-123456789`  | Corresponde a qualquer inteiro |
| `bool`  | `{active:bool}` | `true`, `FALSE` | Corresponde a `true` ou `false` (não diferencia maiúsculas de minúsculas) |
| `datetime` | `{dob:datetime}` | `2016-12-31`, `2016-12-31 7:32pm`  | Corresponde a um valor `DateTime` válido (na cultura invariável – veja o aviso) |
| `decimal` | `{price:decimal}` | `49.99`, `-1,000.01` | Corresponde a um valor `decimal` válido (na cultura invariável – veja o aviso) |
| `double`  | `{weight:double}` | `1.234`, `-1,001.01e8` | Corresponde a um valor `double` válido (na cultura invariável – veja o aviso) |
| `float`  | `{weight:float}` | `1.234`, `-1,001.01e8` | Corresponde a um valor `float` válido (na cultura invariável – veja o aviso) |
| `guid`  | `{id:guid}` | `CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}` | Corresponde a um valor `Guid` válido |
| `long` | `{ticks:long}` | `123456789`, `-123456789` | Corresponde a um valor `long` válido |
| `minlength(value)` | `{username:minlength(4)}` | `Rick` | A cadeia de caracteres deve ter, no mínimo, 4 caracteres |
| `maxlength(value)` | `{filename:maxlength(8)}` | `Richard` | A cadeia de caracteres não pode ser maior que 8 caracteres |
| `length(length)` | `{filename:length(12)}` | `somefile.txt` | A cadeia de caracteres deve ter exatamente 12 caracteres |
| `length(min,max)` | `{filename:length(8,16)}` | `somefile.txt` | A cadeia de caracteres deve ter, pelo menos, 8 e não mais de 16 caracteres |
| `min(value)` | `{age:min(18)}` | `19` | O valor inteiro deve ser, pelo menos, 18 |
| `max(value)` | `{age:max(120)}` |  `91` | O valor inteiro não deve ser maior que 120 |
| `range(min,max)` | `{age:range(18,120)}` | `91` | O valor inteiro deve ser, pelo menos, 18, mas não maior que 120 |
| `alpha` | `{name:alpha}` | `Rick` | A cadeia de caracteres deve consistir em um ou mais caracteres alfabéticos (`a`-`z`, não diferencia maiúsculas de minúsculas) |
| `regex(expression)` | `{ssn:regex(^\\d{{3}}-\\d{{2}}-\\d{{4}}$)}` | `123-45-6789` | A cadeia de caracteres deve corresponder à expressão regular (veja as dicas sobre como definir uma expressão regular) |
| `required`  | `{name:required}` | `Rick` |  Usado para impor que um valor não parâmetro está presente durante a geração de URL |

>[!WARNING]
> Restrições de rota que verificam se a URL pode ser convertida em um tipo CLR (como `int` ou `DateTime`) sempre usam a cultura invariável – elas supõem que a URL não é localizável. As restrições de rota fornecidas pela estrutura não modificam os valores armazenados nos valores de rota. Todos os valores de rota analisados com base na URL serão armazenados como cadeias de caracteres. Por exemplo, a [restrição de rota Float](https://github.com/aspnet/Routing/blob/1.0.0/src/Microsoft.AspNetCore.Routing/Constraints/FloatRouteConstraint.cs#L44-L60) tentará converter o valor de rota em um float, mas o valor convertido é usado somente para verificar se ele pode ser convertido em um float.

## <a name="regular-expressions"></a>Expressões regulares 

A estrutura do ASP.NET Core adiciona `RegexOptions.IgnoreCase | RegexOptions.Compiled | RegexOptions.CultureInvariant` ao construtor de expressão regular. Consulte [Enumeração de RegexOptions](/dotnet/api/system.text.regularexpressions.regexoptions) para obter uma descrição desses membros.

As expressões regulares usam delimitadores e tokens semelhantes aos usados pelo Roteamento e pela linguagem C#. Os tokens de expressão regular precisam ter escape. Por exemplo, para usar a expressão regular `^\d{3}-\d{2}-\d{4}$` no Roteamento, ela precisa ter os caracteres `\` digitados como `\\` no arquivo de origem C# para fazer o escape do caractere de escape da cadeia de caracteres `\` (a menos que [literais de cadeia de caracteres textuais](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/string) estejam sendo usados). Os caracteres `{`, `}`, '[' e ']' precisam ter o escape com aspas duplas para fazer o escape dos caracteres de delimitador do parâmetro de Roteamento.  A tabela abaixo mostra uma expressão regular e a versão com escape.

| Expressão               | Observação |
| ----------------- | ------------ | 
| `^\d{3}-\d{2}-\d{4}$` | Expressão regular |
| `^\\d{{3}}-\\d{{2}}-\\d{{4}}$` | Com escape  |
| `^[a-z]{2}$` | Expressão regular |
| `^[[a-z]]{{2}}$` | Com escape  |

As expressões regulares usadas no roteamento geralmente começarão com o caractere `^` (corresponde à posição inicial da cadeia de caracteres) e terminarão com o caractere `$` (corresponde à posição final da cadeia de caracteres). Os caracteres `^` e `$` garantem que a expressão regular corresponde a todo o valor do parâmetro de rota. Sem os caracteres `^` e `$`, a expressão regular corresponderá a qualquer subcadeia de caracteres na cadeia de caracteres, o que geralmente não é o desejado. A tabela abaixo mostra alguns exemplos e explica por que eles encontram ou não uma correspondência.

| Expressão               | Cadeia de Caracteres | Corresponder a | Comentário |
| ----------------- | ------------ |  ------------ |  ------------ | 
| `[a-z]{2}` | hello | sim | correspondências com a subcadeia de caracteres |
| `[a-z]{2}` | 123abc456 | sim | correspondências com a subcadeia de caracteres |
| `[a-z]{2}` | mz | sim | correspondência com a expressão |
| `[a-z]{2}` | MZ | sim | não diferencia maiúsculas de minúsculas |
| `^[a-z]{2}$` |  hello | no | consulte `^` e `$` acima |
| `^[a-z]{2}$` |  123abc456 | no | consulte `^` e `$` acima |

Consulte [Expressões regulares do .NET Framework](https://docs.microsoft.com/dotnet/standard/base-types/regular-expression-language-quick-reference) para obter mais informações sobre a sintaxe de expressão regular.

Para restringir um parâmetro a um conjunto conhecido de valores possíveis, use uma expressão regular. Por exemplo, `{action:regex(^(list|get|create)$)}` corresponde o valor de rota `action` apenas a `list`, `get` ou `create`. Se for passada para o dicionário de restrições, a cadeia de caracteres "^(list|get|create)$" será equivalente. As restrições passadas para o dicionário de restrições (não embutidas em um modelo) que não correspondem a uma das restrições conhecidas também são tratadas como expressões regulares.

## <a name="url-generation-reference"></a>Referência de geração de URL

O exemplo abaixo mostra como gerar um link para uma rota com um dicionário de valores de rota e uma `RouteCollection`.

[!code-csharp[](../fundamentals/routing/sample/RoutingSample/Startup.cs?range=45-59)]

O `VirtualPath` gerado no final da amostra acima é `/package/create/123`.

O segundo parâmetro para o construtor `VirtualPathContext` é uma coleção de *valores de ambiente*. Os valores de ambiente proporcionam conveniência, limitando o número de valores que um desenvolvedor precisa especificar em determinado contexto de solicitação. Os valores de rota atuais da solicitação atual são considerados valores de ambiente para a geração de link. Por exemplo, em um aplicativo ASP.NET MVC, se você estiver na ação `About` do `HomeController`, não precisará especificar o valor de rota do controlador a ser vinculado à ação `Index` (o valor de ambiente `Home` será usado).

Os valores de ambiente que não correspondem a um parâmetro são ignorados e os valores de ambiente também são ignorados quando um valor fornecido de forma explícita o substitui, seguindo da esquerda para a direita na URL.

Os valores que são fornecidos de forma explícita, mas que não correspondem a nada, são adicionados à cadeia de caracteres de consulta. A tabela a seguir mostra o resultado do uso do modelo de rota `{controller}/{action}/{id?}`.

| Valores de ambiente | Valores explícitos | Resultado |
| -------------   | -------------- | ------ |
| controller="Home" | action="About" | `/Home/About` |
| controller="Home" | controller="Order",action="About" | `/Order/About` |
| controller="Home",color="Red" | action="About" | `/Home/About` |
| controller="Home" | action="About",color="Red" | `/Home/About?color=Red`

Se uma rota tem um valor padrão que não corresponde a um parâmetro e esse valor é fornecido de forma explícita, ele precisa corresponder ao valor padrão. Por exemplo:

```csharp
routes.MapRoute("blog_route", "blog/{*slug}",
  defaults: new { controller = "Blog", action = "ReadPost" });
```

A geração de link gera um link para essa rota quando os valores correspondentes do controlador e da ação são fornecidos.

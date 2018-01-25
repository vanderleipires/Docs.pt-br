---
title: "Roteamento no núcleo do ASP.NET"
author: ardalis
description: "Descobrir como a funcionalidade de roteamento do ASP.NET Core é responsável para mapear uma solicitação de entrada para um manipulador de rota."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/routing
ms.openlocfilehash: 8f6f4fac89afe14d83d629128fc3e4632ae95510
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
# <a name="routing-in-aspnet-core"></a>Roteamento no núcleo do ASP.NET

Por [Ryan Nowak](https://github.com/rynowak), [Steve Smith](https://ardalis.com/), e [Rick Anderson](https://twitter.com/RickAndMSFT)

Funcionalidade de roteamento é responsável por uma solicitação de entrada de mapeamento para um manipulador de rota. Rotas são definidas no aplicativo do ASP.NET e configuradas quando o aplicativo é iniciado. Uma rota opcionalmente pode extrair os valores da URL contida na solicitação, e esses valores, em seguida, podem ser usados para o processamento da solicitação. Usando informações de rota do aplicativo ASP.NET, a funcionalidade de roteamento também é capaz de gerar URLs que são mapeados para manipuladores de rotas. Portanto, o roteamento pode encontrar um manipulador de rota com base em uma URL ou a URL correspondente a um manipulador de rota em questão com base nas informações do manipulador de rota.

>[!IMPORTANT]
> Este documento abrange o nível inferior ASP.NET Core roteamento. Para o roteamento MVC do ASP.NET Core, consulte [roteamento para ações do controlador](../mvc/controllers/routing.md)

[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/routing/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))

## <a name="routing-basics"></a>Noções básicas sobre roteamento

Usa roteamento *rotas* (implementações de [IRouter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.routing.irouter)) para:

* solicitações de entrada do mapa *manipuladores de rota*

* gerar as URLs usadas nas respostas

Em geral, um aplicativo tem uma única coleção de rotas. Quando uma solicitação chega, a coleção de rotas é processada na ordem. A solicitação de entrada procura uma rota que corresponde à URL de solicitação chamando o `RouteAsync` método em cada rota disponível na coleção de rotas. Por outro lado, uma resposta pode usar o roteamento para gerar URLs (por exemplo, para redirecionamento ou links) com base nas informações de rota e, assim, evite ter URLs de embutir em código, que ajuda a facilidade de manutenção.

Roteamento está conectado a [middleware](middleware.md) pipeline pela `RouterMiddleware` classe. [ASP.NET MVC](../mvc/overview.md) adiciona o roteamento para o pipeline de middleware como parte da sua configuração. Para obter informações sobre como usar o roteamento como um componente autônomo, consulte [middleware de roteamento usando](#using-routing-middleware).

<a name="url-matching-ref"></a>

### <a name="url-matching"></a>Correspondência de URL

Correspondência de URL é o processo pelo qual expede roteamento uma entrada de solicitação para um *manipulador*. Esse processo geralmente com base nos dados no caminho da URL, mas pode ser estendido para considerar todos os dados na solicitação. A capacidade de enviar solicitações para separar manipuladores é fundamental para dimensionar o tamanho e a complexidade de um aplicativo.

Insira as solicitações de entrada a `RouterMiddleware`, que chama o `RouteAsync` método em cada rota na sequência. O `IRouter` instância escolhe se deseja *tratar* a solicitação, definindo o `RouteContext.Handler` para não-nulo `RequestDelegate`. Se uma rota define um manipulador para a solicitação, o processamento será interrompido e o manipulador de rota será invocada para processar a solicitação. Se todas as rotas serão tentadas e nenhum manipulador foi encontrado para a solicitação, o middleware chama *próximo* e o próximo middleware no pipeline de solicitação é invocado.

A entrada principal para `RouteAsync` é o `RouteContext.HttpContext` associado à solicitação atual. O `RouteContext.Handler` e `RouteContext.RouteData` são saídas que serão definidas depois que uma rota corresponde.

Uma correspondência durante `RouteAsync` também definirá as propriedades do `RouteContext.RouteData` para os valores apropriados com base no processamento de solicitação concluído até o momento. Se uma rota corresponde a uma solicitação, o `RouteContext.RouteData` conterá informações de estado importantes sobre o *resultado*.

`RouteData.Values`é um dicionário de *valores de rota* produzidos de rota. Esses valores geralmente são determinados pelo gerar tokens para a URL e podem ser usados para aceitar a entrada do usuário ou para tomar decisões mais expedição dentro do aplicativo.

`RouteData.DataTokens`é um recipiente de propriedades de dados adicionais relacionados à rota correspondente. `DataTokens`são fornecidos para oferecer suporte ao estado associando dados com cada rota para que o aplicativo possa tomar decisões mais tarde com base em qual rota correspondente. Esses valores são definidos pelo desenvolvedor e **não** afetam o comportamento de roteamento de qualquer forma. Além disso, os valores armazenados nos tokens de dados podem ser de qualquer tipo, em contraste com valores de rota, que deve ser conversível em cadeias de caracteres.

`RouteData.Routers`é uma lista das rotas que participou na correspondência com êxito a solicitação. Rotas podem ser aninhadas dentro de um do outro e o `Routers` propriedade reflete o caminho até a árvore lógica de rotas que resultou em uma correspondência. Geralmente o primeiro item na `Routers` é a coleção de rotas e deve ser usada para geração de URL. O último item no `Routers` é o manipulador de rota correspondente.

### <a name="url-generation"></a>Geração de URL

Geração de URL é o processo pelo qual o roteamento pode criar um caminho de URL com base em um conjunto de valores de rota. Isso permite uma separação lógica entre seus manipuladores e as URLs que acessá-los.

Geração de URL segue um processo iterativo semelhante, mas começa com o código de usuário ou do framework chamando o `GetVirtualPath` método de coleção de rotas. Cada *rota* terá seu `GetVirtualPath` método chamado em sequência até não null `VirtualPathData` é retornado.

O principal entradas para `GetVirtualPath` são:

* `VirtualPathContext.HttpContext`

* `VirtualPathContext.Values`

* `VirtualPathContext.AmbientValues`

Rotas usem principalmente os valores de rota fornecidos pelo `Values` e `AmbientValues` para decidir onde é possível gerar uma URL e os valores a serem incluídos. O `AmbientValues` são o conjunto de valores de rota que foram produzidas pela correspondência a solicitação atual com o sistema de roteamento. Por outro lado, `Values` são os valores de rota que especificam como gerar a URL desejada para a operação atual. O `HttpContext` é fornecido no caso de uma rota precisa obter serviços ou dados adicionais associados ao contexto atual.

Dica: Pensar `Values` como sendo um conjunto de substituições para o `AmbientValues`. Geração de URL tenta reutilizar valores de rota da solicitação atual para tornar mais fácil gerar URLs para links usando o mesmo roteiro ou valores de rota.

A saída de `GetVirtualPath` é um `VirtualPathData`. `VirtualPathData`é um paralelo do `RouteData`; ele contém o `VirtualPath` para a URL de saída, bem como algumas propriedades adicionais que devem ser definidas pela rota.

O `VirtualPathData.VirtualPath` propriedade contém o *caminho virtual* produzido pela rota. Dependendo de suas necessidades, talvez seja necessário processar ainda mais o caminho. Por exemplo, se você deseja processar a URL gerada em HTML, você precisa colocar o caminho base do aplicativo.

O `VirtualPathData.Router` é uma referência para a rota que é gerado com êxito a URL.

O `VirtualPathData.DataTokens` propriedades é um dicionário de dados adicionais relacionados à rota que gerou a URL. Isso é paralelo de `RouteData.DataTokens`.

### <a name="creating-routes"></a>Criar rotas

O roteamento fornece o `Route` classe como a implementação padrão de `IRouter`. `Route`usa o *modelo de rota* sintaxe para definir padrões que fará a correspondência com o caminho de URL quando `RouteAsync` é chamado. `Route`usará o mesmo modelo de rota para gerar uma URL quando `GetVirtualPath` é chamado.

A maioria dos aplicativos criará rotas chamando `MapRoute` ou um dos métodos de extensão semelhante definidos em `IRouteBuilder`. Todos esses métodos criará uma instância de `Route` e adicioná-la à coleção de rotas.

Observação: `MapRoute` não tem um parâmetro de manipulador de rota - ela apenas adiciona as rotas que serão tratadas pelo `DefaultHandler`. Como o manipulador padrão é um `IRouter`, ele pode decidir por não manipular a solicitação. Por exemplo, ASP.NET MVC normalmente é configurado como um manipulador padrão que só trata as solicitações que correspondem um controlador disponível e a ação. Para saber mais sobre o roteamento para MVC, consulte [roteamento para ações do controlador](../mvc/controllers/routing.md).

Este é um exemplo de um `MapRoute` chamada usada por uma definição de rota ASP.NET MVC típica:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

Este modelo corresponderá a um caminho de URL como `/Products/Details/17` e extrair os valores de rota `{ controller = Products, action = Details, id = 17 }`. Os valores de rota são determinados pelo dividir o caminho da URL em segmentos e correspondência de cada segmento com o *parâmetro de rota* nome do modelo de rota. Parâmetros de rota são nomeados. Eles estão definidos, colocando o nome do parâmetro chaves `{ }`.

O modelo acima também pode corresponder ao caminho da URL `/` e produzir valores `{ controller = Home, action = Index }`. Isso acontece porque o `{controller}` e `{action}` parâmetros de rota têm valores padrão e o `id` parâmetro de rota é opcional. Igual a `=` sinal seguido por um valor após o nome do parâmetro de rota define um valor padrão para o parâmetro. Um ponto de interrogação `?` depois que o nome do parâmetro de rota define o parâmetro como opcional. Parâmetros com um valor padrão de rota *sempre* produzir um valor de rota quando a rota corresponde - parâmetros opcionais não produzem um valor de rota se não houver nenhum segmento de caminho de URL correspondente.

Consulte [referência de modelo de rota](#route-template-reference) para obter uma descrição completa dos recursos de modelo de rota e sintaxe.

Este exemplo inclui uma *restrição de rota*:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id:int}");
```

Este modelo corresponderá a um caminho de URL como `/Products/Details/17`, mas não `/Products/Details/Apples`. A definição do parâmetro de rota `{id:int}` define uma *restrição de rota* para o `id` parâmetro de rota. Implementam restrições da rota `IRouteConstraint` e inspecionar os valores de rota para verificá-los. Neste exemplo, o valor de rota `id` deve ser conversível em inteiro. Consulte [referência de restrição de rota](#route-constraint-reference) para obter uma explicação mais detalhada de restrições de rota que são fornecidos pela estrutura.

Sobrecargas adicionais de `MapRoute` aceitam valores de `constraints`, `dataTokens`, e `defaults`. Esses parâmetros adicionais de `MapRoute` são definidos como tipo `object`. O uso típico desses parâmetros é passar um objeto tipo anônimos, onde os nomes de propriedade da correspondência de tipo anônimo nomes de parâmetro de rota.

Os exemplos a seguir criam rotas equivalentes:

```csharp
routes.MapRoute(
    name: "default_route",
    template: "{controller}/{action}/{id?}",
    defaults: new { controller = "Home", action = "Index" });

routes.MapRoute(
    name: "default_route",
    template: "{controller=Home}/{action=Index}/{id?}");
```

Dica: A sintaxe embutida para definir padrões e restrições pode ser mais conveniente para rotas simples. No entanto, existem recursos, como tokens de dados que não são suportados pela sintaxe embutida.

Este exemplo demonstra alguns dos recursos mais:

```csharp
routes.MapRoute(
  name: "blog",
  template: "Blog/{*article}",
  defaults: new { controller = "Blog", action = "ReadArticle" });
```

Este modelo corresponderá a um caminho de URL como `/Blog/All-About-Routing/Introduction` e extrairá os valores `{ controller = Blog, action = ReadArticle, article = All-About-Routing/Introduction }`. Valores de rota padrão para `controller` e `action` são produzidas pela rota, mesmo que não haja nenhum parâmetro de rota correspondente no modelo. Valores padrão podem ser especificados no modelo de rota. O `article` parâmetro de rota é definido como um *pega-tudo* pela aparência de um asterisco `*` antes do nome de parâmetro de rota. Parâmetros de rota pega-tudo capturar o restante do caminho da URL e também podem corresponder a cadeia de caracteres vazia.

Este exemplo adiciona tokens de dados e restrições da rota:

```csharp
routes.MapRoute(
    name: "us_english_products",
    template: "en-US/Products/{id}",
    defaults: new { controller = "Products", action = "Details" },
    constraints: new { id = new IntRouteConstraint() },
    dataTokens: new { locale = "en-US" });
```

Este modelo corresponderá a um caminho de URL como `/Products/5` e extrairá os valores `{ controller = Products, action = Details, id = 5 }` e os tokens de dados `{ locale = en-US }`.

![Tokens do Windows locais](routing/_static/tokens.png)

<a name="id1"></a>

### <a name="url-generation"></a>Geração de URL

O `Route` classe também pode executar a geração de URL, a combinação de um conjunto de valores de rota com seu modelo de rota. Logicamente, este é o processo inverso de correspondência, o caminho da URL.

Dica: Para entender melhor a geração de URL, imagine que você deseja gerar e, em seguida, considere como um modelo de rota corresponderia a essa URL de URL. Quais valores deve ser produzido? Este é o equivalente de como funciona a geração de URL a `Route` classe.

Este exemplo usa uma rota de estilo do ASP.NET MVC básica:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

Com os valores de rota `{ controller = Products, action = List }`, essa rota irá gerar a URL `/Products/List`. Os valores de rota são substituídos para os parâmetros de rota correspondente formar o caminho da URL. Como `id` é um parâmetro de rota, não há problema se ele não tem um valor.

Com os valores de rota `{ controller = Home, action = Index }`, essa rota irá gerar a URL `/`. Os valores de rota que foram fornecidos correspondem os valores padrão para que os segmentos correspondentes a esses valores podem ser omitidos com segurança. Observe que as duas URLs geradas seriam ida e volta com essa definição de rota e produzem os mesmos valores de rota que foram usados para gerar a URL.

Dica: Um aplicativo usando o ASP.NET MVC deve usar `UrlHelper` para gerar URLs, em vez de chamar diretamente o roteamento.

Para obter mais detalhes sobre o processo de geração de URL, consulte [referência de geração de url](#url-generation-reference).

## <a name="using-routing-middleware"></a>Usando o roteamento de Middleware

Adicione o pacote do NuGet "Microsoft.AspNetCore.Routing".

Adicionar roteamento para o contêiner de serviço no *Startup.cs*:

[!code-csharp[Main](../fundamentals/routing/sample/RoutingSample/Startup.cs?highlight=3&start=11&end=14)]

Rotas devem ser configuradas no `Configure` método o `Startup` classe. O exemplo a seguir usa essas APIs:

* `RouteBuilder`
* `Build`
* `MapGet`Faz a correspondência apenas das solicitações HTTP GET
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

A tabela a seguir mostra as respostas com os URIs determinado.

| URI | Resposta  |
| ------- | -------- |
| /package/create/3  | Olá! Valores de rota: [operação, criar], [id-3] |
| pacote/faixa /-3  | Olá! Valores de rota: [operação, acompanhar], [id -3] |
| / pacote/acompanhar/3 / | Olá! Valores de rota: [operação, acompanhar], [id -3]  |
| /Package./faixa / | \<Passar, nenhuma correspondência > |
| OBTER /hello/Joe | Olá, Joe! |
| Lançar /hello/Joe | \<Passar, corresponde a HTTP GET apenas > |
| OBTER /hello/Joe/Smith | \<Passar, nenhuma correspondência > |

Se você estiver configurando uma única rota, chame `app.UseRouter` passando um `IRouter` instância. Você não precisará chamar `RouteBuilder`.

O framework fornece um conjunto de métodos de extensão para criar rotas, como:

* `MapRoute`
* `MapGet`
* `MapPost`
* `MapPut`
* `MapDelete`
* `MapVerb`

Alguns desses métodos, como `MapGet` exigem um `RequestDelegate` a ser fornecido. O `RequestDelegate` será usado como o *manipulador de rota* quando a rota corresponde. Outros métodos nesta família permitem configurar um pipeline de middleware que será usado como o manipulador de rota. Se o *mapa* método não aceita como um manipulador, `MapRoute`, em seguida, ele usará o `DefaultHandler`.

O `Map[Verb]` métodos usam restrições para limitar a rota para o verbo HTTP no nome do método. Por exemplo, consulte [MapGet](https://github.com/aspnet/Routing/blob/1.0.0/src/Microsoft.AspNetCore.Routing/RequestDelegateRouteBuilderExtensions.cs#L85-L88) e [MapVerb](https://github.com/aspnet/Routing/blob/1.0.0/src/Microsoft.AspNetCore.Routing/RequestDelegateRouteBuilderExtensions.cs#L156-L180).

## <a name="route-template-reference"></a>Referência de modelo de rota

Tokens entre chaves (`{ }`) definir *parâmetros de rota* que será associado se a rota for atendida. Você pode definir mais de um parâmetro de rota em um segmento de rota, mas eles devem ser separados por um valor literal. Por exemplo `{controller=Home}{action=Index}` não seria uma rota válida, porque não há nenhum valor literal entre `{controller}` e `{action}`. Esses parâmetros de rota devem ter um nome, e podem ter atributos adicionais especificado.

Texto literal diferente de parâmetros de rota (por exemplo, `{id}`) e o separador de caminho `/` devem coincidir com o texto na URL. Correspondência de texto é com base na representação decodificada do caminho URLs e diferencia maiusculas de minúsculas. Para corresponder ao delimitador de parâmetro de rota literal `{` ou `}`, escape-o pelo caractere de repetição (`{{` ou `}}`).

Padrões de URL que tentam capturar um nome de arquivo com uma extensão de arquivo opcional têm considerações adicionais. Por exemplo, usando o modelo `files/{filename}.{ext?}` - quando ambos `filename` e `ext` existir, os dois valores serão preenchidos. Se apenas `filename` existe na URL, as correspondências de rota, pois o ponto à direita `.` é opcional. As seguintes URLs corresponderia essa rota:

* `/files/myFile.txt`
* `/files/myFile.`
* `/files/myFile`

Você pode usar o `*` caractere como um prefixo para um parâmetro de rota para ligar para o restante do URI - isso é chamado de um *pega-tudo* parâmetro. Por exemplo, `blog/{*slug}` corresponderia qualquer URI que foi iniciado com `/blog` e tinha qualquer valor (que seriam atribuído ao `slug` rotear valor). Captura todos os parâmetros também podem corresponder a cadeia de caracteres vazia.

Parâmetros de rota podem ter *valores padrão*, designado, especificando o padrão após o nome do parâmetro, separado por um `=`. Por exemplo, `{controller=Home}` definiria `Home` como o valor padrão para `controller`. O valor padrão é usado se nenhum valor estiver presente na URL para o parâmetro. Além dos valores padrão, os parâmetros de rota podem ser opcionais (especificado por meio do acréscimo um `?` ao final do nome do parâmetro, como em `id?`). A diferença entre opcional e "possui padrão" é que um parâmetro de rota com um valor padrão sempre produz um valor; um parâmetro opcional tem um valor somente quando é fornecida.

Parâmetros de rota também podem ter restrições, que devem corresponder ao valor de rota associado da URL. Adição de dois-pontos `:` e depois que o nome do parâmetro de rota especifica o nome da restrição um *restrição embutida* em um parâmetro de rota. Se a restrição requer argumentos aqueles fornecidos entre parênteses `( )` após o nome da restrição. Várias restrições embutido podem ser especificadas adicionando outra vírgula `:` e o nome da restrição. O nome da restrição é passado para o `IInlineConstraintResolver` service para criar uma instância de `IRouteConstraint` para usar no processamento de URL. Por exemplo, o modelo de rota `blog/{article:minlength(10)}` Especifica o `minlength` restrição com o argumento `10`. Para restrições da rota mais descrição e uma lista das restrições fornecidos pelo framework, consulte [referência de restrição de rota](#route-constraint-reference).

A tabela a seguir demonstra alguns modelos de rota e seu comportamento.

| Modelo de rota | Exemplo de correspondência de URL | Observações |
| -------- | -------- | ------- |
| hello  | /hello  | Somente corresponda ao caminho único`/hello` |
| {Page=Home} | / | Faz a correspondência e define `Page` para`Home` |
| {Page=Home}  | / Contato  | Faz a correspondência e define `Page` para`Contact` |
| {controller}/{action}/{id?} | / / Lista de produtos | Mapeia para `Products` controlador e `List` ação |
| {controller}/{action}/{id?} | /Products/Details/123  |  Mapeia para `Products` controlador e `Details` ação.  `id`definido como 123 |
| {controller=Home}/{action=Index}/{id?} | /  |  Mapeia para `Home` controlador e `Index` método; `id` será ignorado. |

Usando um modelo geralmente é a abordagem mais simples para roteamento. Restrições e padrões também podem ser especificados fora o modelo de rota.

Dica: Habilitar [log](xref:fundamentals/logging/index) para ver como o integradas em implementações de roteamentos, como `Route`, comparar solicitações.

## <a name="route-constraint-reference"></a>Referência de restrição de rota

Restrições da rota executar quando um `Route` tem correspondeu a sintaxe da URL de entrada e convertidos o caminho da URL em valores de rota. Restrições da rota geralmente inspecionar o valor de rota associado por meio do modelo de rota e fazer com que um simples Sim/não decisão sobre se o valor for aceitável. Algumas restrições da rota usam dados fora do valor de rota para considerar se a solicitação pode ser roteada. Por exemplo, o `HttpMethodRouteConstraint` pode aceitar ou rejeitar uma solicitação de acordo com o verbo HTTP.

>[!WARNING]
> Evite usar restrições de **validação de entrada**porque fazendo isso, uma entrada inválida resultará em 404 (não encontrado) em vez de um 400 com uma mensagem de erro apropriado. Restrições da rota devem ser usadas para **ambiguidade** entre as rotas semelhantes, não para validar as entradas para uma rota específica.

A tabela a seguir demonstra alguns restrições da rota e o comportamento esperado.

| restrição | Exemplo | Correspondências de exemplo | Observações |
| --------   | ------- | ------------- | ----- |
| `int` | `{id:int}` | `123456789`, `-123456789`  | Corresponde a qualquer inteiro |
| `bool`  | `{active:bool}` | `true`, `FALSE` | Correspondências `true` ou `false` (diferencia maiusculas de minúsculas) |
| `datetime` | `{dob:datetime}` | `2016-12-31`, `2016-12-31 7:32pm`  | Corresponde a uma opção válida `DateTime` valor (a cultura invariável - consulte aviso) |
| `decimal` | `{price:decimal}` | `49.99`, `-1,000.01` | Corresponde a uma opção válida `decimal` valor (a cultura invariável - consulte aviso) |
| `double`  | `{weight:double}` | `1.234`, `-1,001.01e8` | Corresponde a uma opção válida `double` valor (a cultura invariável - consulte aviso) |
| `float`  | `{weight:float}` | `1.234`, `-1,001.01e8` | Corresponde a uma opção válida `float` valor (a cultura invariável - consulte aviso) |
| `guid`  | `{id:guid}` | `CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}` | Corresponde a uma opção válida `Guid` valor |
| `long` | `{ticks:long}` | `123456789`, `-123456789` | Corresponde a uma opção válida `long` valor |
| `minlength(value)` | `{username:minlength(4)}` | `Rick` | Cadeia de caracteres deve ter pelo menos 4 caracteres |
| `maxlength(value)` | `{filename:maxlength(8)}` | `Richard` | Cadeia de caracteres deve ter não mais de 8 caracteres |
| `length(length)` | `{filename:length(12)}` | `somefile.txt` | Cadeia de caracteres deve ter exatamente 12 caracteres |
| `length(min,max)` | `{filename:length(8,16)}` | `somefile.txt` | Cadeia de caracteres deve ter pelo menos 8 e não mais de 16 caracteres |
| `min(value)` | `{age:min(18)}` | `19` | Valor inteiro deve ser pelo menos 18 |
| `max(value)` | `{age:max(120)}` |  `91` | Valor inteiro deve ser não mais de 120 |
| `range(min,max)` | `{age:range(18,120)}` | `91` | Valor inteiro deve ser pelo menos 18, mas não mais de 120 |
| `alpha` | `{name:alpha}` | `Rick` | Cadeia de caracteres deve consistir em um ou mais caracteres alfabéticos (`a`-`z`, diferencia maiusculas de minúsculas) |
| `regex(expression)` | `{ssn:regex(^\\d{{3}}-\\d{{2}}-\\d{{4}}$)}` | `123-45-6789` | Cadeia de caracteres deve corresponder à expressão regular (consulte dicas sobre como definir uma expressão regular) |
| `required`  | `{name:required}` | `Rick` |  Usado para impor que um valor de parâmetro não está presente durante a geração de URL |

>[!WARNING]
> Restrições da rota que verifique a URL podem ser convertidas em um tipo CLR (como `int` ou `DateTime`) sempre usam a cultura invariável - assumem a URL é não localizáveis. As restrições da rota fornecida pelo framework não modifique os valores armazenados nos valores de rota. Todos os valores de rota analisados a partir de URL serão armazenados como cadeias de caracteres. Por exemplo, o [restrição de rota Float](https://github.com/aspnet/Routing/blob/1.0.0/src/Microsoft.AspNetCore.Routing/Constraints/FloatRouteConstraint.cs#L44-L60) tentará converter o valor de rota em um float, mas o valor convertido é usado somente para verificar se ele pode ser convertido em float.

## <a name="regular-expressions"></a>Expressões regulares 

Adiciona a estrutura do ASP.NET Core `RegexOptions.IgnoreCase | RegexOptions.Compiled | RegexOptions.CultureInvariant` para o construtor de expressão regular. Consulte [RegexOptions Enumeration](https://docs.microsoft.com/dotnet/api/system.text.regularexpressions.regexoptions) para obter uma descrição desses membros.

Expressões regulares usam delimitadores e símbolos semelhantes aos usados pelo roteamento e a linguagem c#. Tokens de expressão regular devem ser de escape. Por exemplo, para usar a expressão regular `^\d{3}-\d{2}-\d{4}$` roteamento, ele precisa ter o `\` caracteres digitados no como `\\` no arquivo de origem c# para escapar o `\` caractere de escape da cadeia de caracteres (a menos que usando [textuais literais de cadeia de caracteres](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/string). O `{` , `}` , ' [' e ']' caracteres precisam ser substituídas por aspas duplas para escapar os caracteres de delimitador de parâmetro de roteamento.  A tabela a seguir mostra uma expressão regular e a versão de escape.

| Expressão               | Observação |
| ----------------- | ------------ | 
| `^\d{3}-\d{2}-\d{4}$` | Expressão regular |
| `^\\d{{3}}-\\d{{2}}-\\d{{4}}$` | Escape  |
| `^[a-z]{2}$` | Expressão regular |
| `^[[a-z]]{{2}}$` | Escape  |

Expressões regulares usadas em roteamento geralmente começa com o `^` caractere (correspondência de início da cadeia de caracteres) e terminam com o `$` caractere (correspondência terminando a posição da cadeia de caracteres). O `^` e `$` caracteres Certifique-se de que o valor do parâmetro de rota toda a correspondência da expressão regular. Sem o `^` e `$` caracteres de expressão regular corresponderá a qualquer subcadeia de caracteres na cadeia de caracteres, que é geralmente não é o desejado. A tabela a seguir mostra alguns exemplos e explica por que eles correspondam ou não corresponde.

| Expressão               | Cadeia de Caracteres | Corresponder a | Comentário |
| ----------------- | ------------ |  ------------ |  ------------ | 
| `[a-z]{2}` | hello | sim | correspondência de subcadeia de caracteres |
| `[a-z]{2}` | 123abc456 | sim | correspondência de subcadeia de caracteres |
| `[a-z]{2}` | mz | sim | coincide com a expressão |
| `[a-z]{2}` | MZ | sim | não entre maiusculas e minúsculas |
| `^[a-z]{2}$` |  hello | no | consulte `^` e `$` acima |
| `^[a-z]{2}$` |  123abc456 | no | consulte `^` e `$` acima |

Consulte [expressões regulares do .NET Framework](https://docs.microsoft.com/dotnet/standard/base-types/regular-expression-language-quick-reference) para obter mais informações sobre a sintaxe de expressão regular.

Para restringir um parâmetro para um conjunto conhecido de valores possíveis, use uma expressão regular. Por exemplo `{action:regex(^(list|get|create)$)}` corresponde apenas a `action` rotear o valor para `list`, `get`, ou `create`. Se passado para o dicionário de restrições, a cadeia de caracteres "^ (lista | get | criar) $" seria equivalente. Restrições que são passadas no dicionário de restrições (não embutido dentro de um modelo) que não correspondem a uma das restrições conhecidas também são tratadas como expressões regulares.

## <a name="url-generation-reference"></a>Referência de geração de URL

O exemplo a seguir mostra como gerar um link para uma rota com um dicionário de valores de rota e uma `RouteCollection`.

[!code-csharp[Main](../fundamentals/routing/sample/RoutingSample/Startup.cs?range=45-59)]

O `VirtualPath` gerado no final do exemplo acima é `/package/create/123`.

O segundo parâmetro para o `VirtualPathContext` construtor é uma coleção de *valores ambiente*. Valores de ambiente proporcionar conveniência, limitando o número de valores que deve especificar um desenvolvedor em um determinado contexto de solicitação. Os valores de rota atual da solicitação atual são considerados valores de ambiente para a geração de link. Por exemplo, em um aplicativo ASP.NET MVC, se você estiver no `About` ação do `HomeController`, você não precisa especificar o valor de rota do controlador para vincular ao `Index` ação (o valor de ambiente de `Home` será usado).

Valores de ambiente não corresponderem a um parâmetro são ignorados e ambiente valores também são ignorados quando um valor explicitamente fornecido substituições, vai da esquerda para a direita na URL.

Valores que são explicitamente fornecidas, mas que não corresponde a qualquer coisa são adicionados à cadeia de caracteres de consulta. A tabela a seguir mostra o resultado ao usar o modelo de rota `{controller}/{action}/{id?}`.

| Valores de ambiente | Valores explícitos | Resultado |
| -------------   | -------------- | ------ |
| controller="Home" | action="About" | `/Home/About` |
| controller="Home" | controller="Order",action="About" | `/Order/About` |
| controller="Home",color="Red" | action="About" | `/Home/About` |
| controller="Home" | action="About",color="Red" | `/Home/About?color=Red`

Se uma rota tem um valor padrão que não corresponde a um parâmetro e esse valor é explicitamente fornecido, ele deve corresponder o valor padrão. Por exemplo:

```csharp
routes.MapRoute("blog_route", "blog/{*slug}",
  defaults: new { controller = "Blog", action = "ReadPost" });
```

Geração de link só poderia gerar um link para essa rota quando os valores correspondentes para o controlador e ação são fornecidos.

---
title: "O roteamento para ações do controlador"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 03/14/2017
ms.topic: article
ms.assetid: 26250a4d-bf62-4d45-8549-26801cf956e9
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/controllers/routing
ms.openlocfilehash: da67124ffc874c4f83fff077c6429e9f3e571587
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/11/2017
---
# <a name="routing-to-controller-actions"></a>O roteamento para ações do controlador

Por [Ryan Nowak](https://github.com/rynowak) e [Rick Anderson](https://twitter.com/RickAndMSFT)

Núcleo do ASP.NET MVC usa o roteamento [middleware](../../fundamentals/middleware.md) para corresponder as URLs de solicitações de entrada e mapeá-los para ações. Rotas são definidas no código de inicialização ou atributos. Rotas descrevem como os caminhos de URL devem corresponder às ações. Rotas também são usadas para gerar URLs (para links) enviados em respostas. 

Ações ou são roteadas convencionalmente ou atributo roteadas. Colocar uma rota no controlador ou ação torna atributo roteado. Consulte [misto roteamento](#routing-mixed-ref-label) para obter mais informações.

Este documento explicará as interações entre MVC e roteamento e disponibilizar os aplicativos MVC típico como usar os recursos de roteamentos. Consulte [roteamento](xref:fundamentals/routing) para obter detalhes sobre roteamento avançado.

## <a name="setting-up-routing-middleware"></a>Configurar o roteamento de Middleware

No seu *configurar* método, você pode ver código semelhante a:

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

Dentro da chamada para `UseMvc`, `MapRoute` é usado para criar uma única rota, vamos analisar como o `default` rota. A maioria dos aplicativos MVC usará uma rota com um modelo semelhante para o `default` rota.

O modelo de rota `"{controller=Home}/{action=Index}/{id?}"` pode corresponder a um caminho de URL como `/Products/Details/5` e extrairá os valores de rota `{ controller = Products, action = Details, id = 5 }` por gerar tokens para o caminho. MVC tentará localizar um controlador nomeado `ProductsController` e execute a ação de `Details`:

```csharp
public class ProductsController : Controller
{
   public IActionResult Details(int id) { ... }
}
```

Observe que, neste exemplo, associação de modelo usariam o valor de `id = 5` para definir o `id` parâmetro `5` ao invocar essa ação. Consulte o [modelo associação](../models/model-binding.md) para obter mais detalhes.

Usando o `default` rota:

```csharp
routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

O modelo de rota:

* `{controller=Home}`define `Home` como padrão`controller`

* `{action=Index}`define `Index` como padrão`action`

* `{id?}`define `id` como opcionais

Padrão e parâmetros de rota opcional não precisa estar presente no caminho da URL para uma correspondência. Consulte [referência de modelo de rota](../../fundamentals/routing.md#route-template-reference) para obter uma descrição detalhada da sintaxe de modelo de rota.

`"{controller=Home}/{action=Index}/{id?}"`pode corresponder ao caminho de URL `/` e produzirá os valores de rota `{ controller = Home, action = Index }`. Os valores para `controller` e `action` fazer uso de valores padrão, `id` não produz um valor porque não há nenhum segmento correspondente no caminho da URL. MVC usa esses valores de rota para selecionar o `HomeController` e `Index` ação:

```csharp
public class HomeController : Controller
{
  public IActionResult Index() { ... }
}
```

Usando essa definição de controlador e o modelo de rota, o `HomeController.Index` a ação deve ser executada para qualquer um dos seguintes caminhos de URL:

* `/Home/Index/17`

* `/Home/Index`

* `/Home`

* `/`

O método de conveniência `UseMvcWithDefaultRoute`:

```csharp
app.UseMvcWithDefaultRoute();
```

Pode ser usado para substituir:

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

`UseMvc`e `UseMvcWithDefaultRoute` adicionar uma instância de `RouterMiddleware` para o pipeline de middleware. MVC não interage diretamente com middleware e usa o roteamento para tratar as solicitações. MVC está conectado às rotas por meio de uma instância de `MvcRouteHandler`. O código dentro de `UseMvc` é semelhante à seguinte:

<!-- literal_block {"ids": [], "names": [], "backrefs": [], "dupnames": [], "xml:space": "preserve", "classes": []} -->

```csharp
var routes = new RouteBuilder(app);

// Add connection to MVC, will be hooked up by calls to MapRoute.
routes.DefaultHandler = new MvcRouteHandler(...);

// Execute callback to register routes.
// routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");

// Create route collection and add the middleware.
app.UseRouter(routes.Build());
```

`UseMvc`não define diretamente todas as rotas, ele adiciona um espaço reservado para a coleção de rotas para o `attribute` rota. A sobrecarga `UseMvc(Action<IRouteBuilder>)` permite que você adicione suas próprias rotas e também dá suporte a roteamento de atributo.  `UseMvc`e todas as variações adiciona um espaço reservado para a rota de atributo - roteamento de atributo está sempre disponível, independentemente de como você configura `UseMvc`. `UseMvcWithDefaultRoute`define uma rota padrão e dá suporte a roteamento de atributo. O [roteamento de atributo](#attribute-routing-ref-label) seção inclui mais detalhes sobre o roteamento de atributo.

<a name=routing-conventional-ref-label></a>

## <a name="conventional-routing"></a>Roteamento convencional

O `default` rota:

<!-- literal_block {"ids": [], "names": [], "backrefs": [], "dupnames": [], "xml:space": "preserve", "classes": []} -->

```csharp
routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

é um exemplo de um *roteamento convencional*. Chamamos esse estilo *roteamento convencional* porque ele estabelece um *convenção* para caminhos de URL:

* o primeiro segmento de caminho é mapeado para o nome do controlador

* o segundo é mapeado para o nome da ação.

* o terceiro segmento é usado para um recurso opcional `id` usado para mapear para uma entidade de modelo

Usando esse `default` rota, o caminho da URL `/Products/List` mapeia para o `ProductsController.List` ação, e `/Blog/Article/17` mapeia para `BlogController.Article`. Esse mapeamento é baseado nos nomes de controlador e ação **somente** e não se baseia em namespaces, locais de arquivo de origem ou parâmetros de método.

> [!TIP]
> Usar o roteamento convencional com a rota padrão permite que você compilar o aplicativo rapidamente sem a necessidade de criar um novo padrão de URL para cada ação que você definir. Para um aplicativo com ações de estilo CRUD, com consistência para as URLs em seus controladores pode ajudar a simplificar seu código e fazer sua interface do usuário mais previsível.

> [!WARNING]
> O `id` é definido como opcional pelo modelo de rota, o que significa que suas ações podem executar sem a ID fornecida como parte da URL. Geralmente o que acontecerá se `id` for omitido da URL é que ele será definido como `0` pela associação de modelo e, assim nenhuma entidade será encontrada na correspondência de banco de dados `id == 0`. Roteamento de atributo pode fornecer controle refinado para tornar a ID necessária para algumas ações e não para outras pessoas. Por convenção, a documentação incluirá parâmetros opcionais, como `id` quando eles são provavelmente aparecerão no uso correto.

## <a name="multiple-routes"></a>Várias rotas

Você pode adicionar várias rotas dentro `UseMvc` adicionando mais chamadas para `MapRoute`. Isso permite que você definir várias convenções ou adicionar rotas convencionais que são dedicadas a uma ação específica, como:

<!-- literal_block {"ids": [], "names": [], "backrefs": [], "dupnames": [], "xml:space": "preserve", "classes": []} -->

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("blog", "blog/{*article}",
            defaults: new { controller = "Blog", action = "Article" });
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
}
```

O `blog` rota aqui é um *dedicada rota convencional*, que significa que ele usa o sistema de roteamento convencional, mas é dedicado a uma ação específica. Como `controller` e `action` não aparecem no modelo de rota como parâmetros, eles só podem ter os valores padrão e, portanto, essa rota sempre será mapeado para a ação `BlogController.Article`.

Rotas na coleção de rotas são ordenadas e serão processadas na ordem em que eles são adicionados. Portanto neste exemplo, o `blog` rota será tentada antes do `default` rota.

> [!NOTE]
> *Dedicado rotas convencionais* geralmente usam parâmetros de rota pega-tudo como `{*article}` para capturar a parte restante do caminho da URL. Isso pode fazer com que uma rota "muito greedy" significando corresponde URLs que você se destina a ser correspondido por outras rotas. Coloque as rotas 'greedy' posteriormente na tabela de rotas para solucionar esse problema.

### <a name="fallback"></a>Fallback

Como parte do processamento da solicitação, o MVC verificará se os valores de rota podem ser usados para localizar um controlador e ação em seu aplicativo. Se os valores de rota não corresponderem a uma ação, em seguida, a rota não é considerada uma correspondência e a próxima rota será tentada. Isso é chamado de *fallback*, e ele foi destinado para simplificar a casos em que se sobrepõem rotas convencionais.

### <a name="disambiguating-actions"></a>Ações desambiguação

Quando duas ações correspondem por meio do roteamento, deve resolver a ambiguidade de MVC para escolher o candidato 'as' ou lançar uma exceção. Por exemplo:

<!-- literal_block {"ids": [], "names": [], "backrefs": [], "dupnames": [], "xml:space": "preserve", "classes": []} -->

```csharp
public class ProductsController : Controller
{
   public IActionResult Edit(int id) { ... }

   [HttpPost]
   public IActionResult Edit(int id, Product product) { ... }
}
```

Esse controlador define duas ações que deve corresponder ao caminho de URL `/Products/Edit/17` e encaminhar dados `{ controller = Products, action = Edit, id = 17 }`. Este é um padrão comum para controladores MVC onde `Edit(int)` mostra um formulário para editar um produto e `Edit(int, Product)` processa o formulário postado. Para que isso aconteça MVC precisa escolher `Edit(int, Product)` quando a solicitação é um HTTP `POST` e `Edit(int)` quando o verbo HTTP for outro número.

O `HttpPostAttribute` ( `[HttpPost]` ) é uma implementação de `IActionConstraint` que só permitirá que a ação a ser selecionado quando o verbo HTTP é `POST`. A presença de um `IActionConstraint` faz o `Edit(int, Product)` corresponde a um melhor que `Edit(int)`, portanto `Edit(int, Product)` será tentada primeiro.

Você só precisará escrever personalizado `IActionConstraint` implementações em cenários especializados, mas do importante compreender a função de atributos, como `HttpPostAttribute` -atributos semelhantes são definidas para outros verbos HTTP. Em roteamento convencional é comum para ações usar o mesmo nome de ação quando eles fazem parte de um `show form -> submit form` fluxo de trabalho. A conveniência deste padrão se tornará mais aparente depois de revisar o [Noções básicas sobre IActionConstraint](#understanding-iactionconstraint) seção.

Se correspondem a várias rotas e MVC não é possível encontrar uma rota 'as', ela irá gerar um `AmbiguousActionException`.

<a name=routing-route-name-ref-label></a>

### <a name="route-names"></a>Nomes de rota

As cadeias de caracteres `"blog"` e `"default"` nos exemplos a seguir são os nomes de rota:


```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("blog", "blog/{*article}",
               defaults: new { controller = "Blog", action = "Article" });
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

Os nomes de rota dar um nome lógico a rota para que a rota nomeada pode ser usada para a geração de URL. Isso simplifica a criação de URL quando a ordenação de rotas pode tornar complicada de geração de URL. Nomes de rotas devem ser exclusivos de nível de aplicativo.

Os nomes de rota não têm impacto em URL correspondente ou tratamento de solicitações; eles são usados apenas para geração de URL. [Roteamento](xref:fundamentals/routing) tem informações mais detalhadas sobre geração de URL, incluindo geração de URL em auxiliares MVC específicos.

<a name=attribute-routing-ref-label></a>

## <a name="attribute-routing"></a>Roteamento de atributo

Roteamento de atributo usa um conjunto de atributos para mapear ações diretamente para modelos de rota. No exemplo a seguir, `app.UseMvc();` é usado no `Configure` método e nenhuma rota é passado. O `HomeController` corresponderá a um conjunto de URLs semelhantes à qual a rota padrão `{controller=Home}/{action=Index}/{id?}` corresponderia:

```csharp
public class HomeController : Controller
{
   [Route("")]
   [Route("Home")]
   [Route("Home/Index")]
   public IActionResult Index()
   {
      return View();
   }
   [Route("Home/About")]
   public IActionResult About()
   {
      return View();
   }
   [Route("Home/Contact")]
   public IActionResult Contact()
   {
      return View();
   }
}
```

O `HomeController.Index()` ação será executada para todos os caminhos de URL `/`, `/Home`, ou `/Home/Index`.

> [!NOTE]
> Este exemplo realça uma diferença importante programação entre o atributo de roteamento e roteamento convencional. Roteamento de atributo requer mais informações para especificar uma rota; a rota padrão convencional manipula rotas de forma mais sucinta. No entanto, o roteamento de atributo permite (e exige) controle preciso dos quais modelos de rota se aplicam a cada ação.

Com o nome do controlador e os nomes de ação de roteamento de atributo reproduzir **sem** função na qual a ação é selecionada. Este exemplo corresponderá as mesmas URLs de exemplo anterior.

```csharp
public class MyDemoController : Controller
{
   [Route("")]
   [Route("Home")]
   [Route("Home/Index")]
   public IActionResult MyIndex()
   {
      return View("Index");
   }
   [Route("Home/About")]
   public IActionResult MyAbout()
   {
      return View("About");
   }
   [Route("Home/Contact")]
   public IActionResult MyContact()
   {
      return View("Contact");
   }
}
```

> [!NOTE]
> Os modelos de rota acima não definem os parâmetros de rota para `action`, `area`, e `controller`. Na verdade, esses parâmetros de rota não são permitidos em rotas de atributo. Desde que o modelo de rota já está associado uma ação, não faz sentido para analisar o nome da ação da URL.

## <a name="attribute-routing-with-httpverb-attributes"></a>Atributo de roteamento com atributos de Http [verbo]

Roteamento de atributo também pode fazer uso do `Http[Verb]` atributos como `HttpPostAttribute`. Todos esses atributos podem aceitar um modelo de rota. Este exemplo mostra duas ações que correspondam ao mesmo modelo de rota:

<!-- literal_block {"ids": [], "names": [], "highlight_args": {}, "backrefs": [], "dupnames": [], "linenos": false, "classes": [], "xml:space": "preserve", "language": "c#"} -->

```csharp
[HttpGet("/products")]
public IActionResult ListProducts()
{
   // ...
}

[HttpPost("/products")]
public IActionResult CreateProduct(...)
{
   // ...
}
```

Para um caminho de URL como `/products` o `ProductsApi.ListProducts` ação será executada quando o verbo HTTP é `GET` e `ProductsApi.CreateProduct` será executado quando o verbo HTTP é `POST`. Primeiro, o roteamento de atributo corresponde à URL com o conjunto de modelos de rota definidas por atributos de rota. Depois que um modelo de rota corresponde, `IActionConstraint` restrições são aplicadas para determinar quais ações podem ser executadas.

> [!TIP]
> Ao criar uma API REST, é raro que você deseja usar `[Route(...)]` em um método de ação. É melhor usar o mais específico `Http*Verb*Attributes` para ser preciso sobre o que suporta a sua API. Esperam-se que os clientes de APIs REST saber o que mapeiam caminhos e verbos HTTP para determinadas operações lógicas.

Como uma rota de atributos se aplica a uma ação específica, é fácil fazer parâmetros necessários como parte da definição de modelo de rota. Neste exemplo, `id` é exigido como parte do caminho da URL.

```csharp
public class ProductsApiController : Controller
{
   [HttpGet("/products/{id}", Name = "Products_List")]
   public IActionResult GetProduct(int id) { ... }
}
```

O `ProductsApi.GetProduct(int)` ação será executada para um caminho de URL como `/products/3` , mas não para um caminho de URL como `/products`. Consulte [roteamento](../../fundamentals/routing.md) para obter uma descrição completa de modelos de rota e as opções relacionadas.

## <a name="route-name"></a>Nome da rota

O código a seguir define uma *nome da rota* de `Products_List`:

```csharp
public class ProductsApiController : Controller
{
   [HttpGet("/products/{id}", Name = "Products_List")]
   public IActionResult GetProduct(int id) { ... }
}
```

Nomes de rota podem ser usados para gerar uma URL com base em uma rota específica. Nomes de rota não têm impacto sobre a correspondência de comportamento de roteamento de URL e só são usados para geração de URL. Os nomes de rota devem ser exclusivo do nível de aplicativo.

> [!NOTE]
> Compare isso com o convencional *rota padrão*, que define o `id` parâmetro como opcional (`{id?}`). Essa capacidade de especificar precisamente APIs tem vantagens, como permitindo `/products` e `/products/5` deve ser distribuída para ações diferentes.

<a name=routing-combining-ref-label></a>

### <a name="combining-routes"></a>Rotas de combinação

Para tornar o roteamento de atributo menos repetitivas, atributos de rota do controlador são combinados com atributos de rota as ações individuais. Os modelos de rota definidos no controlador são pré-anexados modelos nas ações de rota. Colocar um atributo da rota no controlador torna **todos os** ações no controlador de usam o roteamento de atributo.

<!-- literal_block {"ids": [], "names": [], "highlight_args": {}, "backrefs": [], "dupnames": [], "linenos": false, "classes": [], "xml:space": "preserve", "language": "c#"} -->

```csharp
[Route("products")]
public class ProductsApiController : Controller
{
   [HttpGet]
   public IActionResult ListProducts() { ... }

   [HttpGet("{id}")]
   public ActionResult GetProduct(int id) { ... }
}
```

Neste exemplo, o caminho da URL `/products` pode corresponder a `ProductsApi.ListProducts`e o caminho da URL `/products/5` pode corresponder a `ProductsApi.GetProduct(int)`. Ambas as ações correspondem apenas HTTP `GET` porque eles são decorados com o `HttpGetAttribute`.

Rotear modelos aplicados a uma ação que começam com um `/` não combinada com modelos de rota aplicados ao controlador. Este exemplo corresponde a um conjunto de caminhos de URL semelhante de *rota padrão*.

```csharp
[Route("Home")]
public class HomeController : Controller
{
    [Route("")]      // Combines to define the route template "Home"
    [Route("Index")] // Combines to define the route template "Home/Index"
    [Route("/")]     // Does not combine, defines the route template ""
    public IActionResult Index()
    {
        ViewData["Message"] = "Home index";
        var url = Url.Action("Index", "Home");
        ViewData["Message"] = "Home index" + "var url = Url.Action; =  " + url;
        return View();
    }

    [Route("About")] // Combines to define the route template "Home/About"
    public IActionResult About()
    {
        return View();
    }   
}
```

<a name=routing-ordering-ref-label></a>

### <a name="ordering-attribute-routes"></a>Rotas de atributo de ordenação

Em contraste com rotas convencionais que execute em uma ordem definida, o roteamento de atributo criará uma árvore e corresponde a todas as rotas simultaneamente. Isso se comporta como-se as entradas de rota foram colocadas em uma ordem ideal; as rotas mais específicas tenham a oportunidade de executar antes das rotas mais gerais.

Por exemplo, uma rota como `blog/search/{topic}` é mais específico que uma rota como `blog/{*article}`. Falando logicamente o `blog/search/{topic}` rota 'executa' primeiro, por padrão, porque essa é a sensato somente ordenação. Usando o roteamento convencional, o desenvolvedor é responsável pela colocação de rotas na ordem desejada.

Rotas de atributo podem configurar uma ordem, usando o `Order` propriedade de todos os atributos de rota do framework fornecido. Rotas são processadas de acordo com a crescente de classificação de `Order` propriedade. A ordem padrão é `0`. Definindo uma rota usando `Order = -1` será executado antes de rotas que não definem um pedido. Definindo uma rota usando `Order = 1` será executado após a ordenação de rota padrão.

> [!TIP]
> Evite dependendo `Order`. Se o seu espaço de URL requer valores de ordem explícita para rotear corretamente, é provavelmente confuso para os clientes. Em geral a roteamento de atributo selecionará a rota correta com URL correspondente. Se a ordem padrão usada para a geração de URL não está funcionando, usando o nome da rota como uma substituição é geralmente mais simples do que a aplicação de `Order` propriedade.

<a name=routing-token-replacement-templates-ref-label></a>

## <a name="token-replacement-in-route-templates-controller-action-area"></a>Token de substituição em modelos de rota ([controller] [ação] [área])

Para sua conveniência, rotas de atributo suportam *substituição de token* colocando um token quadrado chaves (`[`, `]`). Os tokens `[action]`, `[area]`, e `[controller]` substituirá os valores do nome do controlador da ação em que a rota é definida, o nome da ação e o nome da área. Neste exemplo as ações podem corresponder a caminhos de URL conforme descrito nos comentários:

[!code-csharp[Main](routing/sample/main/Controllers/ProductsController.cs?range=7-11,13-17,20-22)]

Substituição do token ocorre como a última etapa de compilação as rotas de atributo. O exemplo acima irão se comportar o mesmo que o código a seguir:

[!code-csharp[Main](routing/sample/main/Controllers/ProductsController2.cs?range=7-11,13-17,20-22)]

Rotas de atributo também podem ser combinadas com herança. Isso é especialmente eficiente, combinado com a substituição do token.

```csharp
[Route("api/[controller]")]
public abstract class MyBaseController : Controller { ... }

public class ProductsController : MyBaseController
{
   [HttpGet] // Matches '/api/Products'
   public IActionResult List() { ... }

   [HttpPost("{id}")] // Matches '/api/Products/{id}'
   public IActionResult Edit(int id) { ... }
}
```

Substituição do token também se aplica a nomes de rotas definidos por rotas de atributo. `[Route("[controller]/[action]", Name="[controller]_[action]")]`irá gerar um nome exclusivo de rota para cada ação.

Para corresponder ao delimitador de literal de substituição de token `[` ou `]`, escape-o pelo caractere de repetição (`[[` ou `]]`).

<a name=routing-multiple-routes-ref-label></a>

### <a name="multiple-routes"></a>Várias rotas

Dá suporte a roteamento definindo várias rotas que atingem a mesma ação de atributo. O uso mais comum é para simular o comportamento do *rota convencional* conforme mostrado no exemplo a seguir:

<!-- literal_block {"ids": [], "names": [], "highlight_args": {}, "backrefs": [], "dupnames": [], "linenos": false, "classes": [], "xml:space": "preserve", "language": "c#"} -->

```csharp
[Route("[controller]")]
public class ProductsController : Controller
{
   [Route("")]     // Matches 'Products'
   [Route("Index")] // Matches 'Products/Index'
   public IActionResult Index()
}
```

Colocar vários atributos de rota no controlador significa que cada um deles serão combinadas com cada um dos atributos de rota sobre os métodos de ação.

<!-- literal_block {"ids": [], "names": [], "highlight_args": {}, "backrefs": [], "dupnames": [], "linenos": false, "classes": [], "xml:space": "preserve", "language": "c#"} -->

```csharp
[Route("Store")]
[Route("[controller]")]
public class ProductsController : Controller
{
   [HttpPost("Buy")]     // Matches 'Products/Buy' and 'Store/Buy'
   [HttpPost("Checkout")] // Matches 'Products/Checkout' and 'Store/Checkout'
   public IActionResult Buy()
}
```

Quando vários atributos de rota (que implementam `IActionConstraint`) são colocados em uma ação, em seguida, cada restrição ação combina com o modelo de rota do atributo que definiu.

<!-- literal_block {"ids": [], "names": [], "highlight_args": {}, "backrefs": [], "dupnames": [], "linenos": false, "classes": [], "xml:space": "preserve", "language": "c#"} -->

```csharp
[Route("api/[controller]")]
public class ProductsController : Controller
{
   [HttpPut("Buy")]      // Matches PUT 'api/Products/Buy'
   [HttpPost("Checkout")] // Matches POST 'api/Products/Checkout'
   public IActionResult Buy()
}
```

> [!TIP]
> Embora usar várias rotas nas ações pode parecer avançado, é melhor manter o espaço de URL do aplicativo simples e bem definidos. Use várias rotas nas ações somente quando necessário, por exemplo, para dar suporte a clientes existentes.

<a name=routing-attr-options></a>

### <a name="specifying-attribute-route-optional-parameters-default-values-and-constraints"></a>Especificando parâmetros opcionais de rota de atributo, valores padrão e restrições

Rotas de atributo oferecem suporte a mesma sintaxe embutida convencionais rotas para especificar parâmetros opcionais, valores padrão e restrições.

```csharp
[HttpPost("product/{id:int}")]
public IActionResult ShowProduct(int id)
{
   // ...
}
```

Consulte [referência de modelo de rota](../../fundamentals/routing.md#route-template-reference) para obter uma descrição detalhada da sintaxe de modelo de rota.

<a name=routing-cust-rt-attr-irt-ref-label></a>

### <a name="custom-route-attributes-using-iroutetemplateprovider"></a>Atributos de rota personalizados usando`IRouteTemplateProvider`

Todos os atributos de rota fornecidos no framework ( `[Route(...)]`, `[HttpGet(...)]` , etc.) implementa a `IRouteTemplateProvider` interface. MVC procurará atributos em classes de controlador e os métodos de ação quando o aplicativo é iniciado e usa os que implementam `IRouteTemplateProvider` para criar o conjunto inicial de rotas.

Você pode implementar `IRouteTemplateProvider` para definir seus próprios atributos de rota. Cada `IRouteTemplateProvider` permite que você defina uma única rota com um modelo de rota personalizados, solicitar e nome:

```csharp
public class MyApiControllerAttribute : Attribute, IRouteTemplateProvider
{
   public string Template => "api/[controller]";

   public int? Order { get; set; }

   public string Name { get; set; }
}
```

O atributo do exemplo acima configura automaticamente o `Template` para `"api/[controller]"` quando `[MyApiController]` é aplicada.

<a name=routing-app-model-ref-label></a>

### <a name="using-application-model-to-customize-attribute-routes"></a>Usando o modelo de aplicativo para personalizar as rotas de atributo

O *modelo de aplicativo* é um modelo de objeto criado durante a inicialização com todos os metadados usados pelo MVC para rotear e executar ações. O *modelo de aplicativo* inclui todos os dados coletados a partir de atributos de rota (por meio de `IRouteTemplateProvider`). Você pode escrever *convenções* para modificar o modelo de aplicativo no momento da inicialização personalize o comportamento do roteamento. Esta seção mostra um exemplo simples de personalização de roteamento usando o modelo de aplicativo.

[!code-csharp[Main](routing/sample/main/NamespaceRoutingConvention.cs)]

<a name=routing-mixed-ref-label></a>

## <a name="mixed-routing-attribute-routing-vs-conventional-routing"></a>Misto roteamento: atributo roteamento roteamento convencional do vs

Aplicativos MVC podem combinar o uso de roteamento convencional e roteamento de atributo. É comum usar as rotas convencionais para controladores servindo páginas HTML para navegadores e roteamento para controladores que serve de APIs REST de atributo.

Ações ou são roteadas convencionalmente ou atributo roteadas. Colocar uma rota no controlador ou ação torna atributo roteado. Ações que definem rotas de atributo não podem ser acessadas por meio de rotas convencionais e vice-versa. **Qualquer** atributo da rota no controlador faz com que todas as ações no atributo controlador roteado.

> [!NOTE]
> O que distingue os dois tipos de sistemas de roteamentos é o processo aplicado depois que uma URL corresponde a um modelo de rota. No roteamento convencional, os valores de rota de correspondência são usados para escolher a ação e o controlador de uma tabela de pesquisa de todas as ações roteadas convencionais. No roteamento de atributo, cada modelo já está associado uma ação e nenhuma pesquisa adicional é necessária.

<a name=routing-url-gen-ref-label></a>

## <a name="url-generation"></a>Geração de URL

Aplicativos MVC podem usar recursos de geração de URL do roteamento para gerar links de URL para ações. Gerar URLs elimina codificar URLs, tornar o seu código mais robusto e sustentável. Esta seção enfoca os recursos de geração de URL fornecidos pelo MVC e só aborda Noções básicas de como funciona a geração de URL. Consulte [roteamento](../../fundamentals/routing.md) para obter uma descrição detalhada da geração de URL.

O `IUrlHelper` interface é a parte subjacente da infraestrutura entre MVC e roteamento para geração de URL. Você encontrará uma instância de `IUrlHelper` disponíveis por meio de `Url` propriedade em controladores, exibições e componentes do modo de exibição.

Neste exemplo, o `IUrlHelper` interface é usada por meio de `Controller.Url` propriedade para gerar uma URL para outra ação.

[!code-csharp[Main](routing/sample/main/Controllers/UrlGenerationController.cs?name=snippet_1)]

Se o aplicativo está usando o padrão convencional de rota, o valor de `url` variável será a cadeia de caracteres de caminho de URL `/UrlGeneration/Destination`. Esse caminho de URL é criado pelo roteamento combinando os valores de rota da solicitação atual (valores de ambiente), com os valores passados para `Url.Action` e substituindo os valores para o modelo de rota:

```
ambient values: { controller = "UrlGeneration", action = "Source" }
values passed to Url.Action: { controller = "UrlGeneration", action = "Destination" }
route template: {controller}/{action}/{id?}

result: /UrlGeneration/Destination
```

Cada parâmetro de rota no modelo de rota tem seu valor substituído pelos nomes correspondentes com os valores e ambiente. Um parâmetro de rota que não tem um valor pode usar um valor padrão se ele tem um ou ser ignorado se for opcional (como no caso de `id` neste exemplo). Geração de URL falhará se qualquer parâmetro de rota necessária não tem um valor correspondente. Se a falha na geração de URL para uma rota, a próxima rota é tentada até que todas as rotas tentou ou uma correspondência for encontrada.

O exemplo de `Url.Action` acima pressupõe roteamento convencional, mas URL geração funciona da mesma forma com o roteamento de atributo, embora os conceitos sejam diferentes. Com o roteamento convencionais, os valores de rota são usados para expandir um modelo e os valores de rota para `controller` e `action` normalmente são exibidos no modelo - isso funciona porque as URLs correspondidas pelo roteamento aderem a um *convenção*. No roteamento de atributo, valores de rota para `controller` e `action` não podem ser exibidas no modelo - eles são usados para procurar o modelo a ser usado.

Este exemplo usa o roteamento de atributo:

[!code-csharp[Main](routing/sample/main/StartupUseMvc.cs?name=snippet_1)]

[!code-csharp[Main](routing/sample/main/Controllers/UrlGenerationControllerAttr.cs?name=snippet_1)]

MVC cria uma tabela de pesquisa de todas as ações de atributo roteada e corresponderá a `controller` e `action` valores para selecionar o modelo de rota a ser usada para geração de URL. No exemplo acima, `custom/url/to/destination` é gerado.

### <a name="generating-urls-by-action-name"></a>Gerar URLs, o nome da ação

`Url.Action` (`IUrlHelper` . `Action`) e todos os relacionados sobrecargas todos são baseados nessa ideia que você deseja especificar o que você está vinculando especificando um nome de controlador e ação.

> [!NOTE]
> Ao usar `Url.Action`, valores de rota atual para `controller` e `action` especificados para você, o valor de `controller` e `action` fazem parte de ambos *valores ambiente* **e** *valores*. O método `Url.Action`, sempre usa os valores atuais das `action` e `controller` e irá gerar um caminho de URL que roteia para a ação atual.

As tentativas de usar os valores em valores de ambiente para preencher informações que você não fornecer ao gerar uma URL de roteamento. Usando uma rota como `{a}/{b}/{c}/{d}` e valores de ambiente `{ a = Alice, b = Bob, c = Carol, d = David }`, roteamento tem informações suficientes para gerar uma URL sem valores adicionais – desde que todas as rotas parâmetros têm um valor. Se você adicionou o valor `{ d = Donovan }`, o valor `{ d = David }` seria ignorado, e o caminho da URL gerado seria `Alice/Bob/Carol/Donovan`.

> [!WARNING]
> Caminhos de URL são hierárquicos. No exemplo acima, se você adicionou o valor `{ c = Cheryl }`, ambos os valores `{ c = Carol, d = David }` será ignorado. Nesse caso não temos mais um valor para `d` e haverá falha na geração de URL. Você precisa especificar o valor desejado de `c` e `d`.  Você pode esperar atingir esse problema com a rota padrão (`{controller}/{action}/{id?}`)- mas raramente você encontrará esse comportamento na prática como `Url.Action` será sempre especificar explicitamente um `controller` e `action` valor.

Mais sobrecargas de `Url.Action` também levar adicional *valores de rota* objeto para fornecer valores para parâmetros de rota que `controller` e `action`. Você verá mais isso usado com `id` como `Url.Action("Buy", "Products", new { id = 17 })`. Por convenção o *valores de rota* geralmente é um objeto de tipo anônimo, mas ele também pode ser um `IDictionary<>` ou um *simples objeto .NET antigo*. Quaisquer valores de rota adicionais que não coincidem com os parâmetros de rota são colocados na cadeia de caracteres de consulta.

[!code-csharp[Main](routing/sample/main/Controllers/TestController.cs)]

> [!TIP]
> Para criar uma URL absoluta, use uma sobrecarga que aceita um `protocol`:`Url.Action("Buy", "Products", new { id = 17 }, protocol: Request.Scheme)`

<a name=routing-gen-urls-route-ref-label></a>

### <a name="generating-urls-by-route"></a>Gerar URLs pela rota

O código acima demonstrado a geração de uma URL, passando o nome do controlador e ação. `IUrlHelper`também fornece o `Url.RouteUrl` família dos métodos. Esses métodos são semelhantes às `Url.Action`, mas eles não copiar os valores atuais das `action` e `controller` para os valores de rota. O uso mais comum é especificar um nome de rota para usar uma rota específica para gerar a URL, geralmente *sem* especificando um nome de controlador ou ação.

[!code-csharp[Main](routing/sample/main/Controllers/UrlGenerationControllerRouting.cs?name=snippet_1)]

<a name=routing-gen-urls-html-ref-label></a>

### <a name="generating-urls-in-html"></a>Gerar URLs em HTML

`IHtmlHelper`fornece o `HtmlHelper` métodos `Html.BeginForm` e `Html.ActionLink` para gerar `<form>` e `<a>` elementos respectivamente. Esses métodos usam o `Url.Action` método para gerar uma URL e eles aceitam argumentos semelhantes. O `Url.RouteUrl` programas para `HtmlHelper` são `Html.BeginRouteForm` e `Html.RouteLink` que tem uma funcionalidade semelhante.

TagHelpers gerar URLs através de `form` TagHelper e `<a>` TagHelper. Ambos usam `IUrlHelper` para sua implementação. Consulte [trabalhar com formulários](../views/working-with-forms.md) para obter mais informações.

Em modos de exibição, o `IUrlHelper` está disponível por meio de `Url` propriedade para a geração de URL qualquer ad hoc não coberta por acima.

<a name=routing-gen-urls-action-ref-label></a>

### <a name="generating-urls-in-action-results"></a>Gerar URLS nos resultados da ação

Os exemplos anteriores mostraram usando `IUrlHelper` em um controlador, enquanto o uso mais comum em um controlador está gerar uma URL como parte do resultado de uma ação.

O `ControllerBase` e `Controller` classes base fornecem métodos de conveniência para resultados de ação que fazem referência a outra ação. Um uso típico é para redirecionar após aceitar a entrada do usuário.

```csharp
public Task<IActionResult> Edit(int id, Customer customer)
{
    if (ModelState.IsValid)
    {
        // Update DB with new details.
        return RedirectToAction("Index");
    }
}
```

Os métodos de fábrica de resultados de ação sigam um padrão semelhante aos métodos `IUrlHelper`.

<a name=routing-dedicated-ref-label></a>

### <a name="special-case-for-dedicated-conventional-routes"></a>Caso especial para rotas convencionais dedicados

Roteamento convencional pode usar um tipo especial de definição da rota chamado um *dedicada rota convencional*. No exemplo a seguir, a rota chamada `blog` é uma rota convencional dedicada.

<!-- literal_block {"ids": [], "names": [], "highlight_args": {}, "backrefs": [], "dupnames": [], "linenos": false, "classes": [], "xml:space": "preserve", "language": "c#"} -->

```csharp
app.UseMvc(routes =>
{
    routes.MapRoute("blog", "blog/{*article}",
        defaults: new { controller = "Blog", action = "Article" });
    routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

Usando essas definições de rota, `Url.Action("Index", "Home")` irá gerar o caminho da URL `/` com o `default` rota, mas por quê? Você pode imaginar os valores de rota `{ controller = Home, action = Index }` deve ser suficiente para gerar uma URL usando `blog`, e o resultado seria `/blog?action=Index&controller=Home`.

Rotas convencionais dedicadas dependem de um comportamento especial de valores padrão que não tem um parâmetro de rota correspondente que impede que a rota "muito greedy" com a geração de URL. Nesse caso, os valores padrão são `{ controller = Blog, action = Article }`e nem `controller` nem `action` aparece como um parâmetro de rota. Quando o roteamento executa geração de URL, os valores fornecidos devem corresponder aos valores de padrão. Geração de URL usando `blog` falhará porque os valores `{ controller = Home, action = Index }` não correspondem ao `{ controller = Blog, action = Article }`. Roteamento, em seguida, volte para tentar `default`, que é bem-sucedida.

<a name=routing-areas-ref-label></a>

## <a name="areas"></a>Áreas

[Áreas](areas.md) são um recurso MVC usado para organizar as funcionalidades relacionadas em um grupo como um roteamento-namespace separado (para ações do controlador) e a estrutura de pasta (para modos de exibição). Permite usar áreas que um aplicativo tiver vários controladores com o mesmo nome - desde que eles têm diferentes *áreas*. Usando áreas cria uma hierarquia com a finalidade de roteamento, adicionando outro parâmetro de rota, `area` para `controller` e `action`. Esta seção aborda como o roteamento interage com áreas - consulte [áreas](areas.md) para obter detalhes sobre como as áreas são usadas com exibições.

O exemplo a seguir configura o MVC para usar o padrão de rota convencional e um *de rota* para uma área chamada `Blog`:

[!code-csharp[Main](routing/sample/AreasRouting/Startup.cs?name=snippet1)]

Durante a correspondência de um caminho de URL como `/Manage/Users/AddUser`, a primeira rota produzirá os valores de rota `{ area = Blog, controller = Users, action = AddUser }`. O `area` valor de rota é produzido por um valor padrão para `area`, na verdade a rota criada pelo `MapAreaRoute` é equivalente à seguinte:

[!code-csharp[Main](routing/sample/AreasRouting/Startup.cs?name=snippet2)]

`MapAreaRoute`cria uma rota usando um valor padrão e a restrição de `area` usando o nome da área fornecido, nesse caso `Blog`. O valor padrão garante que a rota sempre produz `{ area = Blog, ... }`, a restrição requer que o valor `{ area = Blog, ... }` para geração de URL.

> [!TIP]
> Roteamento convencional é dependente de ordem. Em geral, rotas com áreas devem ser colocadas anteriormente na tabela de rotas que elas sejam mais específicas que rotas sem uma área.

Usando o exemplo acima, os valores de rota corresponderia a ação a seguir:

[!code-csharp[Main](routing/sample/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

O `AreaAttribute` é o que indica que um controlador como parte de uma área, dizemos que esse controlador está no `Blog` área. Controladores sem um `[Area]` atributo não são membros de qualquer área e será **não** corresponder quando o `area` valor de rota é fornecido pelo roteamento. No exemplo a seguir, somente o primeiro controlador listado pode corresponder aos valores de rota `{ area = Blog, controller = Users, action = AddUser }`.

[!code-csharp[Main](routing/sample/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

[!code-csharp[Main](routing/sample/AreasRouting/Areas/Zebra/Controllers/UsersController.cs)]

[!code-csharp[Main](routing/sample/AreasRouting/Controllers/UsersController.cs)]

> [!NOTE]
> O namespace de cada controlador é mostrado aqui para fins de integridade - caso contrário, os controladores teria uma nomenclatura de conflito e gerar um erro do compilador. Namespaces de classe não têm efeito sobre roteamento do MVC.

Os primeiros dois controladores são membros das áreas e corresponder somente quando o nome do respectivos área é fornecido pelo `area` valor de rota. O terceiro controlador não é um membro de qualquer área e pode única correspondência quando nenhum valor para `area` é fornecido pelo roteamento.

> [!NOTE]
> Em termos de correspondência *nenhum valor*, a ausência do `area` valor é o mesmo que o valor de `area` era nulo ou cadeia de caracteres vazia.

Ao executar uma ação dentro de uma área, o valor para a rota `area` estarão disponíveis como um *valor ambiente* para roteamento para usar para a geração de URL. Isso significa que, por padrão áreas atuam *Autoadesivas* para geração de URL, como demonstrado pelo exemplo a seguir.

[!code-csharp[Main](routing/sample/AreasRouting/Startup.cs?name=snippet3)]

<!-- literal_block {"ids": [], "names": [], "highlight_args": {"linenostart": 1}, "backrefs": [], "dupnames": [], "linenos": false, "classes": [], "xml:space": "preserve", "language": "c#", "source": "/Users/shirhatti/src/Docs/aspnet/mvc/controllers/routing/sample/AreasRouting/Areas/Duck/Controllers/UsersController.cs"} -->

[!code-csharp[Main](routing/sample/AreasRouting/Areas/Duck/Controllers/UsersController.cs)]

<a name=iactionconstraint-ref-label></a>

## <a name="understanding-iactionconstraint"></a>Noções básicas sobre IActionConstraint

> [!NOTE]
> Esta seção é uma análise aprofundada no framework internos e como MVC escolhe uma ação a ser executada. Um aplicativo típico não será necessário um personalizado`IActionConstraint`

Você provavelmente já usou `IActionConstraint` mesmo se você não estiver familiarizado com a interface. O `[HttpGet]` atributo e semelhante `[Http-VERB]` atributos implementam `IActionConstraint` para limitar a execução de um método de ação.

```csharp
public class ProductsController : Controller
{
    [HttpGet]
    public IActionResult Edit() { }

    public IActionResult Edit(...) { }
}
```

Supondo que a rota convencional do padrão, o caminho da URL `/Products/Edit` pode produzir valores `{ controller = Products, action = Edit }`, que corresponderia **ambos** das ações mostradas aqui. Em `IActionConstraint` terminologia dizemos que ambas as ações são consideradas candidatos - como ambos correspondem aos dados de rota.

Quando o `HttpGetAttribute` é executado, ele indicará que *Edit()* é uma correspondência para *obter* e não é uma correspondência para qualquer outro verbo HTTP. O `Edit(...)` ação não tem restrições definidas e portanto corresponderá a qualquer verbo HTTP. Portanto supondo uma `POST` - somente `Edit(...)` corresponde. Mas, para um `GET` ambas as ações podem ainda corresponder - no entanto, uma ação com um `IActionConstraint` é sempre considerada *melhor* que uma ação sem. Assim como `Edit()` tem `[HttpGet]` ele é considerado mais específico e será selecionado se podem corresponder a ambas as ações.

Conceitualmente, `IActionConstraint` é uma forma de *sobrecarga*, mas em vez de métodos com o mesmo nome de sobrecarga, sobrecarga entre ações que correspondam a mesma URL. Roteamento de atributo também usa `IActionConstraint` e pode resultar em ações de controladores diferentes, ambos sendo considerados candidatos.

<a name=iactionconstraint-impl-ref-label></a>

### <a name="implementing-iactionconstraint"></a>Implementando IActionConstraint

A maneira mais simples de implementar um `IActionConstraint` é criar uma classe derivada de `System.Attribute` e colocá-lo em suas ações e controladores. MVC descobrirá automaticamente qualquer `IActionConstraint` que são aplicadas como atributos. Você pode usar o modelo de aplicativo para aplicar restrições, e isso se deve provavelmente a abordagem mais flexível que permite a você para metaprogram como elas são aplicadas.

No exemplo a seguir, uma restrição escolhe uma ação com base em um *código de país* dos dados de rota. O [completo de exemplo no GitHub](https://github.com/aspnet/Entropy/blob/dev/samples/Mvc.ActionConstraintSample.Web/CountrySpecificAttribute.cs).

```csharp
public class CountrySpecificAttribute : Attribute, IActionConstraint
{
    private readonly string _countryCode;

    public CountrySpecificAttribute(string countryCode)
    {
        _countryCode = countryCode;
    }

    public int Order
    {
        get
        {
            return 0;
        }
    }

    public bool Accept(ActionConstraintContext context)
    {
        return string.Equals(
            context.RouteContext.RouteData.Values["country"].ToString(),
            _countryCode,
            StringComparison.OrdinalIgnoreCase);
    }
}
```

Você é responsável por implementar o `Accept` método e escolhendo um 'Order' para a restrição para executar. Nesse caso, o `Accept` método `true` para indicar que a ação é uma correspondência quando o `country` rotear correspondências de valor. Isso é diferente de um `RouteValueAttribute` que permite fallback para uma ação não atribuído. O exemplo mostra que se você definir um `en-US` ação e um código de país como `fr-FR` volte a um controlador mais genérico que não tem `[CountrySpecific(...)]` aplicado.

O `Order` propriedade decide qual *estágio* a restrição é parte do. Restrições da ação executar em grupos com base no `Order`. Por exemplo, todos o framework fornecidos atributos de método HTTP a usar o mesmo `Order` valor para que eles executados no mesmo estágio. Você pode ter tantos estágios conforme necessário implementar suas políticas desejadas.

> [!TIP]
> Para decidir sobre um valor para `Order` pense ou não a restrição deve ser aplicada antes de métodos HTTP. Números inferiores executado primeiro.

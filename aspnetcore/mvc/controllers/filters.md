---
title: Filtros
author: ardalis
description: "Saiba como * * filtros funcionam e como usá-los no ASP.NET MVC de núcleo."
keywords: "ASP.NET Core, filtros, filtros de mvc, filtros de ação, filtros de autorização, filtros de recurso, os filtros de resultado, filtros de exceção"
ms.author: tdykstra
manager: wpickett
ms.date: 12/12/2016
ms.topic: article
ms.assetid: 531bda08-aa5b-4471-8f08-96add22c8683
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/controllers/filters
ms.openlocfilehash: 34a5be6e77f8558c9b3c257575272e167ed95ea4
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/13/2017
---
# <a name="filters"></a>Filtros

Por [Tom Dykstra](https://github.com/tdykstra/) e [Steve Smith](https://ardalis.com/)

*Filtros* no MVC do ASP.NET Core permitem executar código antes ou depois de determinados estágios do pipeline de processamento de solicitação.

 Tarefas do identificador de filtros internos como autorização (impedindo o acesso a recursos que não está autorizado a um usuário), garantindo que todas as solicitações de usam HTTPS e a resposta de cache (curto-circuito do pipeline de solicitação para retornar uma resposta em cache). 

Você pode criar filtros personalizados para lidar com preocupações abrangentes para seu aplicativo. Sempre que você deseja evitar a duplicação de código em ações, os filtros são a solução. Por exemplo, você pode consolidar o código em um filtro de exceção de tratamento de erros.

[Exibir ou baixar o exemplo do GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/filters/sample).

## <a name="how-do-filters-work"></a>Como os filtros funcionam?

Filtros são executados dentro a *pipeline de invocação de ação do MVC*, às vezes chamado de *pipeline de filtro*.  O pipeline de filtro é executado depois MVC seleciona a ação a ser executada.

![A solicitação é processada por meio de outro Middleware, Middleware de roteamento, seleção de ação e o Pipeline de invocação de ação do MVC. Continua o processamento de solicitação através de seleção de ação, roteamento Middleware e vários Middleware outros antes de se tornar uma resposta enviada ao cliente.](filters/_static/filter-pipeline-1.png)

### <a name="filter-types"></a>Tipos de filtro

Cada tipo de filtro é executado em um estágio diferente no pipeline de filtro.

* [Filtros de autorização](#authorization-filters) executados primeiro e são usados para determinar se o usuário atual tem autorização para a solicitação atual. Eles podem curto-circuito do pipeline se uma solicitação não está autorizada. 

* [Filtros de recurso](#resource-filters) são as primeiras a lidar com uma solicitação após a autorização.  Eles podem executar código antes do restante do pipeline de filtro e, depois que o restante do pipeline foi concluída. Eles são úteis para implementar o armazenamento em cache ou outra forma de curto-circuito do pipeline de filtro por motivos de desempenho. Desde que eles são executados antes da associação de modelo, eles são úteis para tudo que precisa para influenciar a associação de modelo.

* [Filtros de ação](#action-filters) pode executar código imediatamente antes e depois de um método de ação individual é chamado. Eles podem ser usados para manipular os argumentos passados para uma ação e o resultado da ação.

* [Filtros de exceção](#exception-filters) são usados para aplicar políticas globais para exceções não tratadas que ocorrem antes que nada foi gravado no corpo da resposta.

* [Filtros de resultado](#result-filters) pode executar código imediatamente antes e após a execução de resultados de ação individual. Eles executar somente quando o método de ação foi executada com êxito e são úteis para a lógica que deve delimitar a execução de modo de exibição ou formatador.

O diagrama a seguir mostra como esses tipos de filtro interagem no pipeline de filtro.

![A solicitação é processada por meio de filtros de autorização, filtros de recurso, associação de modelo, filtros de ação, ação de execução e conversão do resultado de ação, filtros de exceção, filtros de resultado e resultado da execução. Na saída, a solicitação é processada somente por filtros de resultado e de recurso antes de se tornar uma resposta enviada ao cliente.](filters/_static/filter-pipeline-2.png)

## <a name="implementation"></a>Implementação

Filtros de dar suporte a implementações síncronas e assíncronas através de definições de interface diferente. Escolha a sincronização ou async variante dependendo do tipo de tarefa que você precisa executar. 

Filtros síncronos que podem executar código antes e depois de definir o estágio do pipeline as*estágio*executando e, na*estágio*executado métodos. Por exemplo, `OnActionExecuting` é chamado antes do método de ação é chamado, e `OnActionExecuted` é chamado depois que o método de ação retorna.

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/SampleActionFilter.cs?highlight=6,8,13)]

Filtros assíncronos definem uma única em*estágio*ExecutionAsync método. Esse método usa um *FilterType*delegado ExecutionDelegate que executa o estágio do pipeline do filtro. Por exemplo, `ActionExecutionDelegate` chama o método de ação e você pode executar código antes e depois de você chamá-lo.

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/SampleAsyncActionFilter.cs?highlight=6,8-10,13)]

Você pode implementar interfaces para vários estágios de filtro em uma única classe. Por exemplo, o [ActionFilterAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.filters.actionfilterattribute) classe abstrata implementa ambos `IActionFilter` e `IResultFilter`, bem como seus equivalentes assíncrono.

> [!NOTE]
> Implementar **ou** síncronos ou a versão assíncrona de uma interface de filtro, não ambos. A estrutura primeiro verifica para ver se o filtro implementa a interface assíncrona e, nesse caso, ele chama que. Caso contrário, ele chama o método da interface síncrona. Se você implementar ambas as interfaces de uma classe, somente o método assíncrono será chamado. Ao usar as classes abstratas como [ActionFilterAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.filters.actionfilterattribute) você substituiria apenas os métodos síncronos ou o método assíncrono para cada tipo de filtro.

### <a name="ifilterfactory"></a>IFilterFactory

`IFilterFactory`implementa `IFilter`. Portanto, um `IFilterFactory` instância pode ser usada como um `IFilter` instância em qualquer lugar no pipeline de filtro. Quando a estrutura se prepara para invocar o filtro, ele tenta convertê-lo para um `IFilterFactory`. Se essa conversão for bem-sucedida, o `CreateInstance` método é chamado para criar o `IFilter` instância que será chamada. Isso fornece um design muito flexível, desde que o pipeline de filtro preciso não precisa ser definida explicitamente quando o aplicativo for iniciado.

Você pode implementar `IFilterFactory` em suas próprias implementações de atributo como outra abordagem para a criação de filtros:

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/AddHeaderWithFactoryAttribute.cs?name=snippet_IFilterFactory&highlight=1,4,5,6,7)]

### <a name="built-in-filter-attributes"></a>Atributos de filtro interno

O framework inclui filtros internos baseado em atributos que você pode subclasse e personalizar. Por exemplo, o filtro de resultados a seguir adiciona um cabeçalho para a resposta.

<a name="add-header-attribute"></a>

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/AddHeaderAttribute.cs?highlight=5,16)]

Atributos permitem que os filtros aceitar argumentos, conforme mostrado no exemplo acima. Você deve adicionar esse atributo para um método de ação ou controlador e especifique o nome e o valor do cabeçalho HTTP:

[!code-csharp[Main](./filters/sample/src/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1)]

O resultado da `Index` ação é mostrada abaixo - os cabeçalhos de resposta são exibidos na parte inferior direita.

![Desenvolvedor ferramentas do Microsoft Edge mostrando cabeçalhos de resposta, inclusive o autor Steve Smith@ardalis](filters/_static/add-header.png)

Várias interfaces de filtro tem atributos correspondentes que podem ser usados como classes base para implementações personalizadas.

Atributos de filtro:

* `ActionFilterAttribute`
* `ExceptionFilterAttribute`
* `ResultFilterAttribute`
* `FormatFilterAttribute`
* `ServiceFilterAttribute`
* `TypeFilterAttribute`

`TypeFilterAttribute`e `ServiceFilterAttribute` são explicadas [posteriormente neste artigo](#dependency-injection).

## <a name="filter-scopes-and-order-of-execution"></a>Escopos de filtro e ordem de execução

Um filtro pode ser adicionado para o pipeline em um dos três *escopos*. Você pode adicionar um filtro a um método de ação específica ou a uma classe de controlador usando um atributo. Ou você pode registrar um filtro global (para todos os controladores e ações), adicionando-o para o `MvcOptions.Filters` coleção no `ConfigureServices` método o `Startup` classe:

[!code-csharp[Main](./filters/sample/src/FiltersSample/Startup.cs?name=snippet_ConfigureServices&highlight=5-8)]

### <a name="default-order-of-execution"></a>Ordem padrão de execução

Quando há vários filtros para um determinado estágio do pipeline, escopo determina a ordem padrão de execução de filtro.  Filtros globais coloque os filtros de classe, que por sua vez, coloque os filtros de método. Isso é, às vezes, mencionado como aninhamento "Boneca russa", como cada aumento no escopo é encapsulado em torno do escopo anterior, como um [aninhamento boneca](https://wikipedia.org/wiki/Matryoshka_doll). Você geralmente pode obter o comportamento de substituição desejado sem precisar determinar explicitamente a ordenação.

Como resultado dessa aninhamento, o *depois* código de filtros é executado na ordem inversa do *antes de* código. A sequência tem esta aparência:

* O *antes de* código de filtros aplicados globalmente
  * O *antes de* código de filtros aplicados a controladores
    * O *antes de* código de filtros aplicados aos métodos de ação
    * O *depois* código de filtros aplicados aos métodos de ação
  * O *depois* código de filtros aplicados a controladores
* O *depois* código de filtros aplicados globalmente
  
Aqui está um exemplo que ilustra a ordem na qual filtro métodos são chamados para filtros de ação síncronos.

| Sequência | Escopo do filtro | Método Filter |
|:--------:|:------------:|:-------------:|
| 1 | Global | `OnActionExecuting` |
| 2 | Controlador | `OnActionExecuting` |
| 3 | Método | `OnActionExecuting` |
| 4 | Método | `OnActionExecuted` |
| 5 | Controlador | `OnActionExecuted` |
| 6 | Global | `OnActionExecuted` |

Esta sequência mostra que o filtro de método é aninhado dentro do filtro de controlador e o filtro de controlador é aninhado dentro de filtros globais. Para colocá-lo outra forma, se você estiver dentro de um filtro de async*estágio*ExecutionAsync método, todos os filtros com um escopo maior executado enquanto o seu código estiver na pilha.

> [!NOTE]
> Cada controlador que herda o `Controller` classe base inclui `OnActionExecuting` e `OnActionExecuted` métodos. Esses métodos encapsular os filtros que são executados para uma determinada ação: `OnActionExecuting` é chamado antes de qualquer um dos filtros, e `OnActionExecuted` é chamado após todos os filtros.

### <a name="overriding-the-default-order"></a>Substituindo a ordem padrão

Você pode substituir o padrão de execução implementando `IOrderedFilter`. Essa interface expõe um `Order` propriedade que tem precedência sobre o escopo para determinar a ordem de execução. Um filtro com baixo `Order` valor terá seu *antes de* código executado antes que um filtro com um valor mais alto de `Order`. Um filtro com baixo `Order` valor terá seu *depois* código executado depois que um filtro com um maior `Order` valor. Você pode definir o `Order` propriedade usando um parâmetro de construtor:

```csharp
[MyFilter(Name = "Controller Level Attribute", Order=1)]
```

Se você tiver o mesmo mostrados no exemplo anterior mas conjunto de filtros de ação 3 o `Order` filtros de propriedade do controlador e globais para 1 e 2 respectivamente, a ordem de execução deve ser invertida.

| Sequência | Escopo do filtro | Propriedade `Order` | Método Filter |
|:--------:|:------------:|:-----------------:|:-------------:|
| 1 | Método | 0 | `OnActionExecuting` |
| 2 | Controlador | 1  | `OnActionExecuting` |
| 3 | Global | 2  | `OnActionExecuting` |
| 4 | Global | 2  | `OnActionExecuted` |
| 5 | Controlador | 1  | `OnActionExecuted` |
| 6 | Método | 0  | `OnActionExecuted` |

O `Order` propriedade supera o escopo ao determinar a ordem na qual os filtros serão executados. Os filtros são classificados primeiro por ordem e escopo é usado para romper ties. Todos os filtros internos implementam `IOrderedFilter` e defina o padrão `Order` valor como 0, para o escopo determina a ordem, a menos que você definir `Order` para um valor diferente de zero.

## <a name="cancellation-and-short-circuiting"></a>Circuito pequeno e cancelamento

Você pode encurta o pipeline de filtro a qualquer momento, definindo o `Result` propriedade o `context` parâmetro fornecido para o método de filtro. Por exemplo, o filtro de recursos a seguir impede que o resto do pipeline de execução.

<a name="short-circuiting-resource-filter"></a>

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/ShortCircuitingResourceFilterAttribute.cs?highlight=12,13,14,15)]

No código a seguir, tanto o `ShortCircuitingResourceFilter` e `AddHeader` destino de filtro de `SomeResource` método de ação. No entanto, porque o `ShortCircuitingResourceFilter` é executado primeiro (porque ele é um filtro de recurso e `AddHeader` é um filtro de ação) e reduz o resto do pipeline, o `AddHeader` filtro nunca é executado para o `SomeResource` ação. Esse comportamento deve ser o mesmo se ambos os filtros foram aplicados no nível de método de ação, fornecido o `ShortCircuitingResourceFilter` executado primeiro (devido ao seu tipo de filtro, ou explícita de uso de `Order` propriedade, por exemplo).

[!code-csharp[Main](./filters/sample/src/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1,9)]

## <a name="dependency-injection"></a>Injeção de dependência

Filtros podem ser adicionados por tipo ou instância. Se você adicionar uma instância, essa instância será usada para cada solicitação. Se você adicionar um tipo, ele será ativado pelo tipo, que significa que uma instância será criada para cada solicitação e as dependências de construtor serão populadas pelo [injeção de dependência](../../fundamentals/dependency-injection.md) (DI). Adicionar um filtro por tipo é equivalente a `filters.Add(new TypeFilterAttribute(typeof(MyFilter)))`.

Os filtros são implementados como atributos e adicionados diretamente às classes de controlador ou métodos de ação não podem ter dependências de construtor fornecidas pelo [injeção de dependência](../../fundamentals/dependency-injection.md) (DI). Isso ocorre porque os atributos devem ter os parâmetros do construtor fornecidos em que elas são aplicadas. Essa é uma limitação de como funcionam os atributos.

Se seus filtros têm dependências que você precisa acessar de DI, há várias abordagens com suporte. Você pode aplicar o filtro a um classe ou método de ação usando um dos seguintes:

* `ServiceFilterAttribute`
* `TypeFilterAttribute`
* `IFilterFactory`implementado em seu atributo

> [!NOTE]
> Uma dependência, que talvez você queira obter de injeção de dependência é um agente de log. No entanto, evite criar e usar filtros meramente para fins de registro em log, como o [recursos de estrutura interna log](../../fundamentals/logging.md) já pode fornecer o que você precisa. Se você pretende adicionar o registro em log para seus filtros, deve se concentrar em questões de domínio de negócios ou comportamento específico para o filtro, em vez de ações do MVC ou outros eventos do framework.

### <a name="servicefilterattribute"></a>ServiceFilterAttribute

Um `ServiceFilter` recupera uma instância do filtro de injeção de dependência. Adicione o filtro para o contêiner no `ConfigureServices`e a referenciá-lo em um `ServiceFilter` atributo

[!code-csharp[Main](./filters/sample/src/FiltersSample/Startup.cs?name=snippet_ConfigureServices&highlight=11)]

[!code-csharp[Main](../../mvc/controllers/filters/sample/src/FiltersSample/Controllers/HomeController.cs?name=snippet_ServiceFilter&highlight=1)]

Usando `ServiceFilter` sem registrar os resultados do tipo de filtro em uma exceção:

```
System.InvalidOperationException: No service for type
'FiltersSample.Filters.AddHeaderFilterWithDI' has been registered.
```

`ServiceFilterAttribute`implementa `IFilterFactory`, que expõe um único método para criar um `IFilter` instância. No caso de `ServiceFilterAttribute`, o `IFilterFactory` da interface `CreateInstance` método é implementado para carregar o tipo especificado do contêiner de serviços (DI).

### <a name="typefilterattribute"></a>TypeFilterAttribute

`TypeFilterAttribute`é muito semelhante ao `ServiceFilterAttribute` (e também implementa `IFilterFactory`), mas seu tipo não for resolvido diretamente do contêiner de injeção de dependência. Em vez disso, ele cria uma instância do tipo usando `Microsoft.Extensions.DependencyInjection.ObjectFactory`.

Devido a essa diferença, os tipos que são referenciados usando a `TypeFilterAttribute` não precisam ser registrados com o contêiner primeiro (mas ainda terão suas dependências atendidas pelo contêiner). Além disso, `TypeFilterAttribute` opcionalmente pode aceitar argumentos de construtor para o tipo em questão. O exemplo a seguir demonstra como passar argumentos para um tipo usando `TypeFilterAttribute`:

[!code-csharp[Main](../../mvc/controllers/filters/sample/src/FiltersSample/Controllers/HomeController.cs?name=snippet_TypeFilter&highlight=1,2)]

Se você tiver um filtro que não exige argumentos, mas que tem dependências de construtor que precisam ser preenchidos pelo DI, você pode usar seu próprio atributo nomeado em classes e métodos em vez de `[TypeFilter(typeof(FilterType))]`). O filtro a seguir mostra como isso pode ser implementado:

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/SampleActionFilterAttribute.cs?name=snippet_TypeFilterAttribute&highlight=1,3,7)]

Esse filtro pode ser aplicado a classes ou métodos usando o `[SampleActionFilter]` sintaxe, em vez de usar `[TypeFilter]` ou `[ServiceFilter]`.

## <a name="authorization-filters"></a>Filtros de autorização

*Filtros de autorização* controlar o acesso aos métodos de ação e os filtros primeiro a ser executado dentro do pipeline de filtro. Eles têm apenas um antes de método, ao contrário da maioria dos filtros que suportam antes e depois de métodos. Você deve gravar somente um filtro de autorização personalizada se você estiver escrevendo sua própria estrutura de autorização. Prefira configurar as políticas de autorização ou gravar uma política de autorização personalizada em relação a gravar um filtro personalizado. A implementação de filtro interno é apenas responsável por chamar o sistema de autorização.

Observe que você não deve lançar exceções em filtros de autorização, desde que nada tratará a exceção (filtros de exceção não tratá-los). Em vez disso, execute um desafio ou encontrar outra maneira.

Saiba mais sobre [autorização](../../security/authorization/index.md).

## <a name="resource-filters"></a>Filtros de recurso

*Filtros de recurso* implementar o o `IResourceFilter` ou `IAsyncResourceFilter` interface e sua execução encapsula a maior parte do pipeline de filtro. (Somente [filtros de autorização](#authorization-filters) executar antes delas.) Filtros de recurso são especialmente úteis se você precisar maioria do trabalho que está fazendo uma solicitação de curto-circuito. Por exemplo, um filtro de cache pode evitar o resto do pipeline de se a resposta já está no cache.

O [curto circuito filtro de recurso](#short-circuiting-resource-filter) mostrado anteriormente é um exemplo de um filtro de recurso. Outro exemplo é [DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/1.1.1/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs), que impede que a associação de modelo de acessar os dados do formulário. É útil para casos em que você sabe que vai receber transferências de arquivos grandes e desejar impedir que o formulário que está sendo lidos na memória.

## <a name="action-filters"></a>Filtros de ação

*Filtros de ação* implementar o o `IActionFilter` ou `IAsyncActionFilter` interface e sua execução envolve a execução de métodos de ação.

Aqui está um filtro de ação de exemplo:

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/SampleActionFilter.cs?name=snippet_ActionFilter)]

O [ActionExecutingContext](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.filters.actionexecutingcontext) fornece as seguintes propriedades:

* `ActionArguments`-permite manipular as entradas para a ação.
* `Controller`-permite manipular a ocorrência do controlador. 
* `Result`-Essa configuração reduz a execução do método de ação e filtros de ação subsequentes. Gerar uma exceção também evita a execução do método de ação e filtros subsequentes, mas é tratado como uma falha em vez de um resultado com êxito.

O [ActionExecutedContext](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.filters.actionexecutedcontext) fornece `Controller` e `Result` mais as propriedades a seguir:

* `Canceled`-será true se a execução da ação foi curto-circuito por outro filtro.
* `Exception`-será não nulo se a ação ou um filtro de ação subsequentes lançou uma exceção. A definição dessa propriedade como nula efetivamente 'handles' uma exceção, e `Result` será executada como se ele foi retornado do método de ação normalmente.

Para uma `IAsyncActionFilter`, uma chamada para o `ActionExecutionDelegate` executa qualquer filtro de ação subsequentes e o método de ação, retornando um `ActionExecutedContext`. Curto-circuito, atribuir `ActionExecutingContext.Result` para alguns resultados de instância e não chamar o `ActionExecutionDelegate`.

O framework fornece um resumo `ActionFilterAttribute` que você pode subclasse. 

Você pode usar um filtro de ação para validar o estado do modelo automaticamente e retornar erros se o estado é inválido:

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/ValidateModelAttribute.cs)]

O `OnActionExecuted` execuções do método após o método de ação e podem ver e manipular os resultados da ação por meio do `ActionExecutedContext.Result` propriedade. `ActionExecutedContext.Canceled`será definido como verdadeiro se a execução da ação foi curto-circuito por outro filtro. `ActionExecutedContext.Exception`será definido como um valor não nulo se a ação ou um filtro de ação subsequentes lançou uma exceção. Configuração `ActionExecutedContext.Exception` como nulo efetivamente 'handles' uma exceção, e `ActionExectedContext.Result` , em seguida, será executada como se ele foi retornado do método de ação normalmente.

## <a name="exception-filters"></a>Filtros de exceção

*Filtros de exceção* implementar o o `IExceptionFilter` ou `IAsyncExceptionFilter` interface. Eles podem ser usados para implementar as políticas para um aplicativo de tratamento de erros comuns. 

O filtro de exceção de exemplo a seguir usa um modo de exibição de erro do desenvolvedor personalizado para exibir detalhes sobre exceções que ocorrem quando o aplicativo está em desenvolvimento:

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/CustomExceptionFilterAttribute.cs?name=snippet_ExceptionFilter&highlight=1,14)]

Filtros de exceção não tem dois eventos (antes e depois) - somente implementam `OnException` (ou `OnExceptionAsync`). 

Filtros de exceção lidar com exceções não tratadas que ocorrem na criação do controlador, [associação de modelo](../models/model-binding.md), filtros de ação ou métodos de ação. Eles não capturem exceções que ocorrem em filtros de recurso, os filtros de resultado ou execução do resultado do MVC.

Para manipular uma exceção, defina o `ExceptionContext.ExceptionHandled` propriedade como true ou gravar uma resposta. Isso interrompe a propagação da exceção. Observe que um filtro de exceção não é possível ativar uma exceção em um "sucesso". Um filtro de ação pode fazer isso.

> [!NOTE]
> No ASP.NET 1.1, a resposta não é enviada se você definir `ExceptionHandled` como true **e** gravar uma resposta. Nesse cenário, ASP.NET Core 1.0 enviar a resposta e ASP.NET Core 1.1.2 retornará ao 1.0 comportamento. Para obter mais informações, consulte [emitir #5594](https://github.com/aspnet/Mvc/issues/5594) no repositório do GitHub. 

Filtros de exceção são bons para interceptar exceções que ocorrem em ações do MVC, mas eles não são mais flexíveis middleware de tratamento de erros. Preferir middleware para o caso geral e usar filtros apenas em que você precisa fazer o tratamento de erros *diferentemente* com base em qual ação MVC foi escolhida. Por exemplo, seu aplicativo pode ter métodos de ação para os pontos de extremidade de API e modos de exibição/HTML. Os pontos de extremidade de API podem retornar informações de erro como JSON, enquanto as ações com base em modo de exibição podem retornar uma página de erro como HTML.

O framework fornece um resumo `ExceptionFilterAttribute` que você pode subclasse. 

## <a name="result-filters"></a>Filtros de resultado

*Filtros de resultado* implementar o o `IResultFilter` ou `IAsyncResultFilter` interface e sua execução envolve a execução de resultados da ação. 

Aqui está um exemplo de um filtro de resultados que adiciona um cabeçalho HTTP.

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

O tipo de resultado que está sendo executado depende da ação em questão. Uma ação do MVC retornando um modo de exibição inclui todos os razor processamento como parte do `ViewResult` que está sendo executada. Um método de API pode executar alguns serialização como parte da execução do resultado. Saiba mais sobre [resultados da ação](actions.md)

Filtros de resultado são executados somente para resultados positivos - quando a ação ou filtros de ação produzem o resultado de uma ação. Filtros de resultado não são executados quando filtros de exceção manipulam uma exceção.

O `OnResultExecuting` método pode curto-circuito execução do resultado de ação e os filtros de resultados subsequentes definindo `ResultExecutingContext.Cancel` como true. Em geral, você deve escrever para o objeto de resposta quando curto-circuito para evitar a geração de uma resposta vazia. Gerar uma exceção também impedirá a execução do resultado de ação e filtros subsequentes, mas será tratado como uma falha em vez de um resultado com êxito.

Quando o `OnResultExecuted` execuções do método, a resposta provável foi enviada ao cliente e não pode ser alterado mais (a menos que uma exceção foi lançada). `ResultExecutedContext.Canceled`será definido como verdadeiro se a execução do resultado de ação foi curto-circuito por outro filtro.

`ResultExecutedContext.Exception`será definido como um valor não nulo se o resultado da ação ou um filtro de resultados subsequentes lançou uma exceção. Definindo `Exception` para null 'handles' uma exceção com eficiência e impede a exceção de que está sendo lançada novamente pelo MVC posteriormente no pipeline. Quando você está tratando uma exceção em um filtro de resultados, você não poderá gravar todos os dados para a resposta. Se o resultado da ação gera parcialmente por meio de sua execução, e os cabeçalhos já foram liberados para o cliente, não há nenhum mecanismo confiável para enviar um código de falha.

Para uma `IAsyncResultFilter` uma chamada para `await next()` no `ResultExecutionDelegate` executa qualquer filtro de resultados subsequentes e o resultado da ação. Curto-circuito, defina `ResultExecutingContext.Cancel` para true e não chamar o `ResultExectionDelegate`.

O framework fornece um resumo `ResultFilterAttribute` que você pode subclasse. O [AddHeaderAttribute](#add-header-attribute) classe mostrada anteriormente é um exemplo de um atributo de filtro de resultados.

## <a name="using-middleware-in-the-filter-pipeline"></a>Usando middleware no pipeline de filtro

Filtros de recursos de trabalho como [middleware](../../fundamentals/middleware.md) em que eles envolvem a execução de tudo o que é fornecida posteriormente no pipeline. Mas filtros são diferentes de middleware eles fazem parte do MVC, o que significa que eles têm acesso ao contexto do MVC e construtores.

No ASP.NET Core 1.1, você pode usar o middleware no pipeline de filtro. Você talvez queira fazer isso, se você tiver um componente de middleware que precisa de acesso a dados de rota do MVC ou que deve ser executado somente para determinados controladores ou ações.

Para usar o middleware como um filtro, crie um tipo com um `Configure` método que especifica o middleware que você deseja inserir no pipeline de filtro. Aqui está um exemplo que usa o middleware de localização para estabelecer a cultura atual para uma solicitação:

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/LocalizationPipeline.cs?name=snippet_MiddlewareFilter&highlight=3,21)]

Você pode usar o `MiddlewareFilterAttribute` para executar o middleware para um controlador selecionado ou a ação ou globalmente:

[!code-csharp[Main](./filters/sample/src/FiltersSample/Controllers/HomeController.cs?name=snippet_MiddlewareFilter&highlight=2)]

Middleware filtros são executados no mesmo estágio do pipeline de filtro como filtros de recurso, antes da associação de modelo e depois o restante do pipeline.

## <a name="next-actions"></a>Próximas ações

Para fazer experiências com filtros, [baixar, testar e modificar o exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/filters/sample).

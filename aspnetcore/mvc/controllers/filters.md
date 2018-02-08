---
title: Filtros
author: ardalis
description: "Saiba como os *Filtros* funcionam e como usá-los no ASP.NET Core MVC."
manager: wpickett
ms.author: tdykstra
ms.date: 12/12/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/controllers/filters
ms.openlocfilehash: 2ba3c226cc57f8a3fb26b4119ae9e575eff522f9
ms.sourcegitcommit: f2a11a89037471a77ad68a67533754b7bb8303e2
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/01/2018
---
# <a name="filters"></a>Filtros

Por [Tom Dykstra](https://github.com/tdykstra/) e [Steve Smith](https://ardalis.com/)

*Filtros* no ASP.NET Core MVC permitem executar código antes ou depois de determinados estágios do pipeline de processamento de solicitações.

 Filtros internos lidam com tarefas como autorização (impedindo o acesso a recursos para os quais o usuário não está autorizado), garantir que todas as solicitações usem HTTPS e cache de respostas (fazendo um curto-circuito no pipeline de solicitações para retornar uma resposta em cache). 

É possível criar filtros personalizados para lidar com questões relacionadas a características transversais de seu aplicativo. Sempre que você quiser evitar a duplicação de código entre ações, filtros são a solução. Por exemplo, é possível consolidar o código de tratamento de erro em um filtro de exceção.

[Exibir ou baixar amostra do GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/filters/sample).

## <a name="how-do-filters-work"></a>Como os filtros funcionam?

Os filtros são executados dentro do *pipeline de invocação de ações do MVC*, às vezes chamado de *pipeline de filtros*.  O pipeline de filtros é executado após o MVC selecionar a ação a ser executada.

![A solicitação é processada por meio de Outro Middleware, do Middleware de Roteamento, da Seleção de Ação e do Pipeline de Invocação de Ações do MVC. O processamento de solicitações continua por meio da Seleção de Ação, do Middleware de Roteamento e de diversos Outros Middlewares antes de se tornar uma resposta enviada ao cliente.](filters/_static/filter-pipeline-1.png)

### <a name="filter-types"></a>Tipos de filtro

Cada tipo de filtro é executado em um estágio diferente no pipeline de filtros.

* [Filtros de autorização](#authorization-filters) são executados primeiro e são usados para determinar se o usuário atual tem autorização para a solicitação atual. Eles podem fazer um curto-circuito do pipeline quando uma solicitação não é autorizada. 

* [Filtros de recurso](#resource-filters) são os primeiros a lidar com uma solicitação após a autorização.  Eles podem executar código antes do restante do pipeline de filtros e após o restante do pipeline ser concluído. Esses filtros são úteis para implementar o cache ou para, de alguma forma, fazer o curto-circuito do pipeline de filtros por motivos de desempenho. Como são executados antes da associação de modelos, eles são úteis para qualquer coisa que precise influenciá-la.

* [Filtros de ação](#action-filters) podem executar código imediatamente antes e depois de um método de ação individual ser chamado. Eles podem ser usados para manipular os argumentos passados para uma ação, bem como o resultado da ação.

* [Filtros de exceção](#exception-filters) são usados para aplicar políticas globais para exceções sem tratamento que ocorrem antes que qualquer coisa tenha sido gravada no corpo da resposta.

* [Filtros de resposta](#result-filters) podem executar código imediatamente antes e depois da execução de resultados de ações individuais. Eles são executados somente quando o método de ação foi executado com êxito e são úteis para lógicas que precisam delimitar a execução formatador ou modo de exibição.

O diagrama a seguir mostra como esses tipos de filtro interagem no pipeline de filtros.

![A solicitação é processada por meio de Filtros de autorização, Filtros de recurso, Associação de modelos, Filtros de ação, Execução de ação e Conversão do resultado de ação, Filtros de exceção, Filtros de resultado e Execução de resultado. Na saída, a solicitação é processada somente por Filtros de resultado e Filtros de recurso antes de se tornar uma resposta enviada ao cliente.](filters/_static/filter-pipeline-2.png)

## <a name="implementation"></a>Implementação

Os filtros dão suporte a implementações síncronas e assíncronas por meio de diferentes definições de interface. Escolha a variante síncrona ou assíncrona dependendo do tipo de tarefa que você precisa executar. 

Filtros síncronos que podem executar código antes e depois do estágio do pipeline definem os métodos On*Stage*Executing e On*Stage*Executed. Por exemplo, `OnActionExecuting` é chamado antes que o método de ação seja chamado e `OnActionExecuted` é chamado após o método de ação retornar.

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/SampleActionFilter.cs?highlight=6,8,13)]

Filtros assíncronos definem um único método On*Stage*ExecutionAsync. Esse método usa um delegado *FilterType*ExecutionDelegate, que executa o estágio de pipeline do filtro. Por exemplo, `ActionExecutionDelegate` chama o método de ação e você pode executar código antes e depois de chamá-lo.

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/SampleAsyncActionFilter.cs?highlight=6,8-10,13)]

É possível implementar interfaces para vários estágios do filtro em uma única classe. Por exemplo, a classe abstrata [ActionFilterAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.filters.actionfilterattribute) implementa `IActionFilter` e `IResultFilter`, bem como seus equivalentes assíncronos.

> [!NOTE]
> Implemente **ou** a versão assíncrona ou a versão síncrona de uma interface de filtro, não ambas. Primeiro, a estrutura verifica se o filtro implementa a interface assíncrona e, se for esse o caso, a chama. Caso contrário, ela chama os métodos da interface síncrona. Se você implementasse as duas interfaces em uma classe, somente o método assíncrono seria chamado. Ao usar classes abstratas como [ActionFilterAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.filters.actionfilterattribute), você substituiria apenas os métodos síncronos ou o método assíncrono para cada tipo de filtro.

### <a name="ifilterfactory"></a>IFilterFactory

`IFilterFactory` implementa `IFilter`. Portanto, uma instância `IFilterFactory` pode ser usada como uma instância `IFilter` em qualquer parte do pipeline de filtro. Quando se prepara para invocar o filtro, a estrutura tenta convertê-lo em um `IFilterFactory`. Se essa conversão for bem-sucedida, o método `CreateInstance` será chamado para criar a instância `IFilter` que será invocada. Isso fornece um design muito flexível, uma vez que o pipeline de filtro preciso não precisa ser definido explicitamente quando o aplicativo é iniciado.

Você pode implementar `IFilterFactory` em suas próprias implementações de atributo como outra abordagem à criação de filtros:

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/AddHeaderWithFactoryAttribute.cs?name=snippet_IFilterFactory&highlight=1,4,5,6,7)]

### <a name="built-in-filter-attributes"></a>Atributos de filtro internos

A estrutura inclui filtros internos baseados em atributos que você pode organizar em subclasses e personalizar. Por exemplo, o seguinte Filtro de resultado adiciona um cabeçalho à resposta.

<a name="add-header-attribute"></a>

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/AddHeaderAttribute.cs?highlight=5,16)]

Os atributos permitem que os filtros aceitem argumentos, conforme mostrado no exemplo acima. Você adicionaria esse atributo a um método de ação ou controlador e especificaria o nome e o valor do cabeçalho HTTP:

[!code-csharp[Main](./filters/sample/src/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1)]

O resultado da ação `Index` é mostrado abaixo – os cabeçalhos de resposta são exibidos na parte inferior direita.

![Ferramentas para Desenvolvedores do Microsoft Edge mostrando cabeçalhos de resposta, inclusive o autor Steve Smith @ardalis](filters/_static/add-header.png)

Várias interfaces de filtro têm atributos correspondentes que podem ser usados como classes base para implementações personalizadas.

Atributos de filtro:

* `ActionFilterAttribute`
* `ExceptionFilterAttribute`
* `ResultFilterAttribute`
* `FormatFilterAttribute`
* `ServiceFilterAttribute`
* `TypeFilterAttribute`

`TypeFilterAttribute` e `ServiceFilterAttribute` são explicados [posteriormente neste artigo](#dependency-injection).

## <a name="filter-scopes-and-order-of-execution"></a>Escopos e ordem de execução dos filtros

Um filtro pode ser adicionado ao pipeline com um de três *escopos*. É possível adicionar um filtro a um método de ação específico ou a uma classe de controlador usando um atributo. Também é possível registrar um filtro globalmente (para todos os controladores e ações) adicionando-o à coleção `MvcOptions.Filters` no método `ConfigureServices` na classe `Startup`:

[!code-csharp[Main](./filters/sample/src/FiltersSample/Startup.cs?name=snippet_ConfigureServices&highlight=5-8)]

### <a name="default-order-of-execution"></a>Ordem padrão de execução

Quando há vários filtros para um determinado estágio do pipeline, o escopo determina a ordem padrão de execução dos filtros.  Filtros globais circundam filtros de classe, que, por sua vez, circundam filtros de método. Isso é, às vezes, chamado de aninhamento de "bonecas russas", pois cada aumento de escopo é encapsulado em torno do escopo anterior, como uma [matriosca](https://wikipedia.org/wiki/Matryoshka_doll). Normalmente, você obtém o comportamento de substituição desejado sem precisar determinar uma ordem explicitamente.

Como resultado desse aninhamento, o código *posterior* dos filtros é executado na ordem inversa do código *anterior*. A sequência é semelhante a esta:

* O código *anterior* de filtros aplicados globalmente
  * O código *anterior* de filtros aplicados a controladores
    * O código *anterior* de filtros aplicados a métodos de ação
    * O código *posterior* de filtros aplicados a métodos de ação
  * O código *posterior* de filtros aplicados a controladores
* O código *posterior* de filtros aplicados globalmente
  
Este é um exemplo que ilustra a ordem na qual métodos de filtro são chamados para filtros de ação síncronos.

| Sequência | Escopo do filtro | Método do filtro |
|:--------:|:------------:|:-------------:|
| 1 | Global | `OnActionExecuting` |
| 2 | Controlador | `OnActionExecuting` |
| 3 | Método | `OnActionExecuting` |
| 4 | Método | `OnActionExecuted` |
| 5 | Controlador | `OnActionExecuted` |
| 6 | Global | `OnActionExecuted` |

Esta sequência mostra que o filtro do método está aninhado no filtro do controlador e que o filtro do controlador está aninhado no filtro global. Para colocar de outra forma, se você estiver dentro do método On*Stage*ExecutionAsync de um filtro assíncrono, todos os filtros com escopo mais estreito serão executados enquanto seu código estiver na pilha.

> [!NOTE]
> Cada controlador que herda da classe base `Controller` inclui os métodos `OnActionExecuting` e `OnActionExecuted`. Esses métodos encapsulam os filtros que são executados para uma determinada ação: `OnActionExecuting` é chamado antes de qualquer um dos filtros e `OnActionExecuted` é chamado após todos os filtros.

### <a name="overriding-the-default-order"></a>Substituindo a ordem padrão

É possível substituir a sequência padrão de execução implementando `IOrderedFilter`. Essa interface expõe uma propriedade `Order` que tem precedência sobre o escopo para determinar a ordem de execução. Um filtro com um valor mais baixo de `Order` terá seu código *anterior* executado antes que um filtro com um valor mais alto de `Order`. Um filtro com um valor mais baixo de `Order` terá seu código *posterior* executado depois que um filtro com um valor mais alto de `Order`. Você pode definir a propriedade `Order` usando um parâmetro de construtor:

```csharp
[MyFilter(Name = "Controller Level Attribute", Order=1)]
```

Se você tiver os mesmos três filtros de ação mostrados no exemplo anterior, mas definir a propriedade `Order` dos filtros de controlador e global como 1 e 2 respectivamente, a ordem de execução será invertida.

| Sequência | Escopo do filtro | Propriedade `Order` | Método do filtro |
|:--------:|:------------:|:-----------------:|:-------------:|
| 1 | Método | 0 | `OnActionExecuting` |
| 2 | Controlador | 1  | `OnActionExecuting` |
| 3 | Global | 2  | `OnActionExecuting` |
| 4 | Global | 2  | `OnActionExecuted` |
| 5 | Controlador | 1  | `OnActionExecuted` |
| 6 | Método | 0  | `OnActionExecuted` |

A propriedade `Order` tem precedência sobre o escopo ao determinar a ordem na qual os filtros serão executados. Os filtros são classificados primeiro pela ordem e o escopo é usado para desempatar. Todos os filtros internos implementam `IOrderedFilter` e definem o valor padrão de `Order` como 0, de forma que o escopo determina a ordem, a menos que você defina `Order` como um valor diferente de zero.

## <a name="cancellation-and-short-circuiting"></a>Cancelamento e curto-circuito

Você pode fazer um curto-circuito no pipeline de filtros a qualquer momento, definindo a propriedade `Result` no parâmetro `context` fornecido ao método do filtro. Por exemplo, o filtro de recurso a seguir impede que o resto do pipeline seja executado.

<a name="short-circuiting-resource-filter"></a>

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/ShortCircuitingResourceFilterAttribute.cs?highlight=12,13,14,15)]

No código a seguir, os filtros `ShortCircuitingResourceFilter` e `AddHeader` têm como destino o método de ação `SomeResource`. No entanto, como `ShortCircuitingResourceFilter` é executado primeiro (porque ele é um filtro de recurso e `AddHeader` é um filtro de ação) e faz o curto-circuito do resto do pipeline, o filtro `AddHeader` nunca é executado para a ação `SomeResource`. Esse comportamento seria o mesmo se os dois filtros fossem aplicados no nível do método de ação, desde que `ShortCircuitingResourceFilter` fosse executado primeiro (devido ao seu tipo de filtro ou ao uso explícito da propriedade `Order`, por exemplo).

[!code-csharp[Main](./filters/sample/src/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1,9)]

## <a name="dependency-injection"></a>Injeção de dependência

Filtros podem ser adicionados por tipo ou por instância. Se você adicionar uma instância, ela será usada para cada solicitação. Se você adicionar um tipo, ele será ativado pelo tipo, o que significa que uma instância será criada para cada solicitação e as dependências de construtor serão populadas pela DI ([injeção de dependência](../../fundamentals/dependency-injection.md)). Adicionar um filtro por tipo é equivalente a `filters.Add(new TypeFilterAttribute(typeof(MyFilter)))`.

Filtros que são implementados como atributos e adicionados diretamente a classes de controlador ou métodos de ação não podem ter dependências de construtor fornecidas pela DI ([injeção de dependência](../../fundamentals/dependency-injection.md)). Isso ocorre porque os atributos precisam ter os parâmetros de construtor fornecidos quando eles são aplicados. Essa é uma limitação do funcionamento dos atributos.

Se seus filtros tiverem dependências que você precisa acessar da DI, há várias abordagens com suporte. É possível aplicar o filtro a um método de ação ou classe usando uma das opções a seguir:

* `ServiceFilterAttribute`
* `TypeFilterAttribute`
* `IFilterFactory` implementado em seu atributo

> [!NOTE]
> Uma dependência que talvez você queira obter da DI é um agente. No entanto, evite criar e usar filtros apenas para fins de registro em log, pois é possível que os [recursos de registro em log da estrutura interna](xref:fundamentals/logging/index) já forneçam o que você precisa. Se você for adicionar o registro em log a seus filtros, ele deve se concentrar em questões referentes ao domínio de negócios ou ao comportamento específico de seu filtro, em vez de ações do MVC ou outros eventos da estrutura.

### <a name="servicefilterattribute"></a>ServiceFilterAttribute

Um `ServiceFilter` recupera uma instância do filtro da DI. Adicione o filtro ao contêiner em `ConfigureServices` e faça uma referência a ele em um atributo `ServiceFilter`

[!code-csharp[Main](./filters/sample/src/FiltersSample/Startup.cs?name=snippet_ConfigureServices&highlight=11)]

[!code-csharp[Main](../../mvc/controllers/filters/sample/src/FiltersSample/Controllers/HomeController.cs?name=snippet_ServiceFilter&highlight=1)]

Usar `ServiceFilter` sem registrar o tipo de filtro gera uma exceção:

```
System.InvalidOperationException: No service for type
'FiltersSample.Filters.AddHeaderFilterWithDI' has been registered.
```

`ServiceFilterAttribute` implementa `IFilterFactory`, que expõe um único método para criar uma instância de `IFilter`. No caso de `ServiceFilterAttribute`, o método `CreateInstance` da interface `IFilterFactory` é implementado para carregar o tipo especificado do contêiner de serviços (DI).

### <a name="typefilterattribute"></a>TypeFilterAttribute

`TypeFilterAttribute` é muito semelhante a `ServiceFilterAttribute` (e também implementa `IFilterFactory`), mas seu tipo não é resolvido diretamente do contêiner de DI. Em vez disso, ele cria uma instância do tipo usando `Microsoft.Extensions.DependencyInjection.ObjectFactory`.

Devido a essa diferença, os tipos que são referenciados usando o `TypeFilterAttribute` não precisam ser registrados com o contêiner primeiro (mas eles ainda terão suas dependências atendidas pelo contêiner). Além disso, `TypeFilterAttribute` também pode aceitar argumentos de construtor para o tipo em questão. O exemplo a seguir demonstra como passar argumentos para um tipo usando `TypeFilterAttribute`:

[!code-csharp[Main](../../mvc/controllers/filters/sample/src/FiltersSample/Controllers/HomeController.cs?name=snippet_TypeFilter&highlight=1,2)]

Se tiver um filtro que não requer nenhum argumento, mas que tem dependências de construtor que precisam ser atendidas pela DI, você poderá usar seu próprio atributo nomeado em classes e métodos em vez de `[TypeFilter(typeof(FilterType))]`). O filtro a seguir mostra como isso pode ser implementado:

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/SampleActionFilterAttribute.cs?name=snippet_TypeFilterAttribute&highlight=1,3,7)]

Esse filtro pode ser aplicado a classes ou métodos usando a sintaxe `[SampleActionFilter]`, em vez de precisar usar `[TypeFilter]` ou `[ServiceFilter]`.

## <a name="authorization-filters"></a>Filtros de autorização

*Filtros de autorização* controlam o acesso aos métodos de ação e são os primeiros filtros a serem executados dentro do pipeline de filtros. Eles têm apenas um método anterior, diferente da maioria dos filtros que dão suporte a métodos anteriores e posteriores. Você deve escrever somente um filtro de autorização personalizado se estiver escrevendo sua própria estrutura de autorização. Prefira configurar suas políticas de autorização ou escrever uma política de autorização personalizada em vez de escrever um filtro personalizado. A implementação do filtro interno é responsável somente por chamar o sistema de autorização.

Observe que você não deve gerar exceções dentro de filtros de autorização, uma vez que nada tratará a exceção (filtros de exceção não as tratarão). Em vez disso, emita um desafio ou encontre outra maneira.

Saiba mais sobre [Autorização](../../security/authorization/index.md).

## <a name="resource-filters"></a>Filtros de recurso

*Filtros de recurso* implementam a interface `IResourceFilter` ou `IAsyncResourceFilter` e sua execução encapsula a maior parte do pipeline de filtros. (Somente [Filtros de autorização](#authorization-filters) são executados antes deles.) Filtros de recurso são especialmente úteis se você precisa fazer o curto-circuito da maioria do trabalho que uma solicitação está fazendo. Por exemplo, um filtro de cache poderá evitar o resto do pipeline se a resposta já estiver no cache.

O [filtro de recurso com curto-circuito](#short-circuiting-resource-filter) mostrado anteriormente é um exemplo de filtro de recurso. Outro exemplo é [DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/1.1.1/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs), que impede que a associação de modelos acesse os dados do formulário. Ele é útil para casos em que você sabe que vai receber uploads de arquivos grandes e quer que o formulário seja lido na memória.

## <a name="action-filters"></a>Filtros de ação

*Filtros de ação* implementam a interface `IActionFilter` ou `IAsyncActionFilter` e sua execução envolve a execução de métodos de ação.

Veja um exemplo de filtro de ação:

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/SampleActionFilter.cs?name=snippet_ActionFilter)]

[ActionExecutingContext](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.filters.actionexecutingcontext) fornece as seguintes propriedades:

* `ActionArguments` – permite manipular as entradas para a ação.
* `Controller` – permite manipular a instância do controlador. 
* `Result` – defini-lo faz um curto-circuito da execução do método de ação e dos filtros de ação posteriores. Apresentar uma exceção também impete a execução do método de ação e dos filtros posteriores, mas isso é tratado como uma falha, e não como um resultado bem-sucedido.

[ActionExecutedContext](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.filters.actionexecutedcontext) fornece `Controller` e `Result`, bem como as seguintes propriedades:

* `Canceled` – será verdadeiro se a execução da ação tiver sofrido curto-circuito por outro filtro.
* `Exception` – não será nulo se a ação ou um filtro de ação posterior tiver apresentado uma exceção. Definir essa propriedade como nula “manipula” uma exceção efetivamente e `Result` será executado como se tivesse sido retornado do método de ação normalmente.

Para um `IAsyncActionFilter`, uma chamada para `ActionExecutionDelegate` executa qualquer filtro de ação posterior e o método da ação, retornando um `ActionExecutedContext`. Para fazer um curto-circuito, atribua `ActionExecutingContext.Result` a uma instância de resultado e não chame o `ActionExecutionDelegate`.

A estrutura fornece um `ActionFilterAttribute` abstrato que você pode colocar em uma subclasse. 

Você pode usar um filtro de ação para validar automaticamente o estado do modelo e retornar erros se o estado for inválido:

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/ValidateModelAttribute.cs)]

O método `OnActionExecuted` é executado depois do método de ação e pode ver e manipular os resultados da ação por meio da propriedade `ActionExecutedContext.Result`. `ActionExecutedContext.Canceled` será definido como verdadeiro se a execução da ação tiver sofrido curto-circuito por outro filtro. `ActionExecutedContext.Exception` será definido como um valor não nulo se a ação ou um filtro de ação posterior tiver apresentado uma exceção. Definir `ActionExecutedContext.Exception` como nulo “manipula” uma exceção efetivamente e `ActionExectedContext.Result` será executado como se tivesse sido retornado do método de ação normalmente.

## <a name="exception-filters"></a>Filtros de exceção

*Filtros de exceção* implementam a interface `IExceptionFilter` ou `IAsyncExceptionFilter`. Eles podem ser usados para implementar políticas de tratamento de erro comuns para um aplicativo. 

O exemplo de filtro de exceção a seguir usa uma exibição de erro de desenvolvedor personalizada para exibir detalhes sobre exceções que ocorrem quando o aplicativo está em desenvolvimento:

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/CustomExceptionFilterAttribute.cs?name=snippet_ExceptionFilter&highlight=1,14)]

Filtros de exceção não têm dois eventos (para antes e depois); eles implementam somente `OnException` (ou `OnExceptionAsync`). 

Filtros de exceção tratam exceções sem tratamento que ocorrem na criação do controlador, [associação de modelos](../models/model-binding.md), filtros de ação ou métodos de ação. Eles não capturam exceções que ocorrem em Filtros de recurso, Filtros de resultado ou na Execução do resultado do MVC.

Para tratar uma exceção, defina a propriedade `ExceptionContext.ExceptionHandled` como verdadeiro ou grave uma resposta. Isso interrompe a propagação da exceção. Observe que um filtro de exceção não pode transformar uma exceção em um "sucesso". Somente um filtro de ação pode fazer isso.

> [!NOTE]
> No ASP.NET 1.1, a resposta não será enviada se você definir `ExceptionHandled` como verdadeiro **e** gravar uma resposta. Nesse cenário, o ASP.NET Core 1.0 envia a resposta e o ASP.NET Core 1.1.2 retorna ao comportamento do 1.0. Para obter mais informações, consulte [problema #5594](https://github.com/aspnet/Mvc/issues/5594) no repositório do GitHub. 

Filtros de exceção são bons para interceptar exceções que ocorrem em ações do MVC, mas não são tão flexíveis quanto o middleware de tratamento de erro. Dê preferência ao uso de middleware para o caso geral e use filtros apenas quando precisar fazer o tratamento de erro de modo *diferente*, dependendo da ação do MVC escolhida. Por exemplo, seu aplicativo pode ter métodos de ação para os pontos de extremidade da API e para modos de exibição/HTML. Os pontos de extremidade da API podem retornar informações de erro como JSON, enquanto as ações baseadas em modo de exibição podem retornar uma página de erro como HTML.

A estrutura fornece um `ExceptionFilterAttribute` abstrato que você pode colocar em uma subclasse. 

## <a name="result-filters"></a>Filtros de resultado

*Filtros de resultado* implementam a interface `IResultFilter` ou `IAsyncResultFilter` e sua execução envolve a execução de resultados da ação. 

Veja um exemplo de um filtro de resultado que adiciona um cabeçalho HTTP.

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

O tipo de resultado que está sendo executado depende da ação em questão. Uma ação de MVC que retorna um modo de exibição incluiria todo o processamento de Razor como parte do `ViewResult` em execução. Um método de API pode executar alguma serialização como parte da execução do resultado. Saiba mais sobre [resultados de ação](actions.md)

Filtros de resultado são executados somente para resultados bem-sucedidos – quando a ação ou filtros da ação produzem um resultado de ação. Filtros de resultado não são executados quando filtros de exceção tratam uma exceção.

O método `OnResultExecuting` pode fazer o curto-circuito da execução do resultado da ação e dos filtros de resultados posteriores definindo `ResultExecutingContext.Cancel` como verdadeiro. De modo geral, você deve gravar no objeto de resposta ao fazer um curto-circuito para evitar gerar uma resposta vazia. Apresentar uma exceção também impede a execução do resultado da ação e dos filtros posteriores, mas isso é tratado como uma falha, e não como um resultado bem-sucedido.

Quando o método `OnResultExecuted` é executado, a resposta provavelmente foi enviada ao cliente e não pode mais ser alterada (a menos que uma exceção tenha sido apresentada). `ResultExecutedContext.Canceled` será definido como verdadeiro se a execução do resultado da ação tiver sofrido curto-circuito por outro filtro.

`ResultExecutedContext.Exception` será definido como um valor não nulo se o resultado da ação ou um filtro de resultado posterior tiver apresentado uma exceção. Definir `Exception` para como nulo “trata” uma exceção com eficiência e impede que a exceção seja apresentada novamente pelo MVC posteriormente no pipeline. Quando está tratando uma exceção em um filtro de resultado, talvez você não possa gravar dados na resposta. Se o resultado da ação for apresentado durante sua execução e os cabeçalhos já tiverem sido liberados para o cliente, não haverá nenhum mecanismo confiável para enviar um código de falha.

Para um `IAsyncResultFilter`, uma chamada para `await next()` no `ResultExecutionDelegate` executa qualquer filtro de resultado posterior e o resultado da ação. Para fazer um curto-circuito, defina `ResultExecutingContext.Cancel` para verdadeiro e não chame `ResultExectionDelegate`.

A estrutura fornece um `ResultFilterAttribute` abstrato que você pode colocar em uma subclasse. A classe [AddHeaderAttribute](#add-header-attribute) mostrada anteriormente é um exemplo de atributo de filtro de resultado.

## <a name="using-middleware-in-the-filter-pipeline"></a>Usando middleware no pipeline de filtros

Filtros de recursos funcionam como [middleware](xref:fundamentals/middleware/index), no sentido em que envolvem a execução de tudo o que vem depois no pipeline. Mas os filtros diferem do middleware porque fazem parte do MVC, o que significa que eles têm acesso ao contexto e a constructos do MVC.

No ASP.NET Core 1.1, você pode usar o middleware no pipeline de filtros. Talvez você queira fazer isso se tiver um componente de middleware que precisa acessar dados de rota do MVC ou que precisa ser executado somente para determinados controladores ou ações.

Para usar o middleware como um filtro, crie um tipo com um método `Configure` que especifica o middleware que você deseja injetar no pipeline de filtros. Veja um exemplo que usa o middleware de localização para estabelecer a cultura atual para uma solicitação:

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/LocalizationPipeline.cs?name=snippet_MiddlewareFilter&highlight=3,21)]

Depois, você pode usar o `MiddlewareFilterAttribute` para executar o middleware para um controlador ou ação selecionada ou para executá-lo globalmente:

[!code-csharp[Main](./filters/sample/src/FiltersSample/Controllers/HomeController.cs?name=snippet_MiddlewareFilter&highlight=2)]

Filtros de middleware são executados no mesmo estágio do pipeline de filtros que filtros de recurso, antes da associação de modelos e depois do restante do pipeline.

## <a name="next-actions"></a>Próximas ações

Para fazer experiências com filtros, [baixe, teste e modifique o exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/filters/sample).

---
title: "Associação de modelo"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: b355a48e-a15c-4d58-b69c-899763613a97
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/models/model-binding
ms.openlocfilehash: 930ea062ffb914cbd4f1500308b813167c1f601b
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/11/2017
---
# <a name="model-binding"></a>Associação de modelo

Por [Rachel Appel](http://github.com/rachelappel)

## <a name="introduction-to-model-binding"></a>Introdução à associação de modelo

Associação de modelo no MVC do ASP.NET Core mapeia dados de solicitações HTTP para os parâmetros de método de ação. Os parâmetros podem ser tipos simples, como cadeias de caracteres, inteiros ou flutuações, ou eles podem ser tipos complexos. Isso é um ótimo recurso do MVC como mapear os dados de entrada para um representante é um cenário repetido com frequência, independentemente do tamanho ou a complexidade dos dados. MVC resolve esse problema removendo associação para que os desenvolvedores não precisam manter reconfiguração de uma versão ligeiramente diferente desse mesmo código em todos os aplicativos. Escrevendo seu próprio texto para o código de conversor de tipo é entediante e propenso a erros.

## <a name="how-model-binding-works"></a>Como funciona a associação de modelo

Quando MVC recebe uma solicitação HTTP, ele encaminha a um método de ação específica de um controlador. Determina qual método de ação para executar com base no que é nos dados de rota e, em seguida, ele associa valores da solicitação de HTTP para os parâmetros do método de ação. Por exemplo, considere a seguinte URL:

`http://contoso.com/movies/edit/2`

Como o modelo de rota se parece com isso, `{controller=Home}/{action=Index}/{id?}`, `movies/edit/2` encaminha para o `Movies` controlador e sua `Edit` método de ação. Ele também aceita um parâmetro opcional chamado `id`. O código para o método de ação deve ter esta aparência:

<!-- literal_block {"ids": [], "linenos": true, "xml:space": "preserve", "language": "csharp"} -->

```csharp
public IActionResult Edit(int? id)
   ```

Observação: As cadeias de caracteres na rota de URL não diferenciam maiusculas de minúsculas.

MVC tentará associar dados de solicitação para os parâmetros de ação por nome. MVC procurará valores para cada parâmetro usando o nome do parâmetro e os nomes de suas propriedades configuráveis públicas. No exemplo acima, o parâmetro de ação só é chamado `id`, que associa MVC para o valor com o mesmo nome nos valores de rota. Além dos valores de rota MVC associar dados de várias partes da solicitação e isso é feito em uma determinada ordem. Abaixo está uma lista das fontes de dados na ordem em que a associação de modelo examina-los:

1. `Form values`: Estes são valores de formulário que entram na solicitação HTTP usando o método POST. (incluindo as solicitações POST jQuery).

2. `Route values`: O conjunto de valores de rota fornecida pelo [roteamento](../../fundamentals/routing.md)

3. `Query strings`: A parte da cadeia de caracteres de consulta do URI.

<!-- DocFX BUG
The link works but generates an error when building with DocFX
@fundamentals/routing
[Routing](xref:fundamentals/routing)
-->

Observação: Formulário valores, dados de rota e consulta cadeias de caracteres são armazenadas como pares nome-valor.

Como uma chave chamada solicitou a associação de modelo `id` e não há nada chamado `id` nos valores de formulário, ele movido para os valores de rota procurando essa chave. Em nosso exemplo, é uma correspondência. Associação ocorre, e o valor é convertido para o inteiro de 2. A mesma solicitação usando Editar (id da cadeia de caracteres) seria converter a cadeia de caracteres "2".

Até agora, o exemplo usa tipos simples. No MVC tipos simples são qualquer tipo primitivo .NET ou com um conversor de tipo de cadeia de caracteres. Se o parâmetro do método de ação fosse uma classe, como o `Movie` tipo, que contém tipos simples e complexos, como propriedades, será de associação de modelo do MVC ainda tratá-la perfeitamente. Ele usa reflexão e recursão para percorrer as propriedades de tipos complexos procurando correspondências. Associação de modelo procura o padrão de *parameter_name.property_name* para associar valores a propriedades. Se ele não encontrar os valores correspondentes deste formulário, ele tentará estabelecer uma ligação usando apenas o nome da propriedade. Para esses tipos como `Collection` tipos de associação de modelo procura correspondências para *parameter_name [index]* ou apenas *[index]*. Trata da associação de modelo `Dictionary` tipos da mesma forma, pedindo *parameter_name [chave]* ou apenas *[chave]*, contanto que as chaves são tipos simples. As chaves que há suporte para correspondem os nomes de campo HTML e os auxiliares de marcação gerados para o mesmo tipo de modelo. Isso permite que os valores de ciclo para que os campos do formulário permaneçam preenchidos com a entrada do usuário para sua conveniência, por exemplo, quando os dados associados de criar ou editar não passam na validação.

Em ordem de associação para a classe deve ter um construtor padrão público e membro a ser associado deve ser públicas propriedades graváveis. Quando a associação de modelo ocorrer que a classe só será instanciada usando o construtor padrão público, as propriedades podem ser definidas.

Quando um parâmetro é vinculado, associação de modelo para procurar por valores com esse nome e ele prossegue para associar o próximo parâmetro. Se a ligação falha, o MVC não gerará um erro. Você pode consultar os erros de estado de modelo, verificando o `ModelState.IsValid` propriedade.

Observação: Cada entrada no controlador de `ModelState` propriedade é um `ModelStateEntry` que contém um `Errors property`. Raramente é necessário para esta coleção de consulta por conta própria. Use `ModelState.IsValid` em seu lugar.

Além disso, há alguns tipos de dados especiais que MVC deve considerar ao realizar a associação de modelo:

* `IFormFile`, `IEnumerable<IFormFile>`: Um ou mais arquivos carregados que fazem parte da solicitação HTTP.

* `CancelationToken`: Usado para cancelar a atividade em controladores de assíncronas.

Esses tipos podem estar vinculados aos parâmetros de ação ou propriedades em um tipo de classe.

Após a conclusão, associação de modelo [validação](validation.md) ocorre. Associação de modelo padrão funciona bem para a maioria dos cenários de desenvolvimento. Também é extensível e se você tiver necessidades exclusivas, você pode personalizar o comportamento interno.

## <a name="customize-model-binding-behavior-with-attributes"></a>Personalizar o comportamento de associação de modelo com atributos

MVC contém vários atributos que podem ser usados para direcionar seu comportamento de associação de modelo padrão para uma fonte diferente. Por exemplo, você pode especificar se a associação é exigida para uma propriedade, ou se ele nunca deve acontecer em todos os usando o `[BindRequired]` ou `[BindNever]` atributos. Como alternativa, você pode substituir a fonte de dados padrão e especificar a fonte de dados do associador de modelo. Abaixo está uma lista dos atributos de associação de modelo:

* `[BindRequired]`: Este atributo adiciona um erro de estado de modelo se a associação não pode ocorrer.

* `[BindNever]`: Informa o associador de modelo nunca vincular a esse parâmetro.

* `[FromHeader]`, `[FromQuery]`, `[FromRoute]`, `[FromForm]`: Usá-los para especificar a origem de associação exata que deseja aplicar.

* `[FromServices]`: Este atributo utiliza [injeção de dependência](../../fundamentals/dependency-injection.md) para associar parâmetros de serviços.

* `[FromBody]`: Use os formatadores configurados para associar dados do corpo da solicitação. O formatador é selecionado com base no tipo de conteúdo da solicitação.

* `[ModelBinder]`: Usado para substituir o associador de modelo padrão, a origem de associação e o nome.

Os atributos são ferramentas muito úteis quando você precisa substituir o comportamento padrão de associação de modelo.

## <a name="binding-formatted-data-from-the-request-body"></a>Associação de dados formatados de corpo da solicitação

Solicitação de dados podem vir de uma variedade de formatos como JSON, XML e muitos outros. Quando você usa o atributo [FromBody] para indicar que você deseja associar um parâmetro de dados no corpo da solicitação, o MVC usa um conjunto configurado de formatadores de tratar os dados de solicitação com base em seu tipo de conteúdo. Por padrão MVC inclui um `JsonInputFormatter` classe para manipular dados JSON, mas você pode adicionar formatadores adicionais para lidar com XML e outros formatos personalizados.

> [!NOTE]
> Pode haver no máximo um parâmetro por ação decorado com `[FromBody]`. Tempo de execução do ASP.NET Core MVC delega a responsabilidade de ler o fluxo da solicitação para o formatador. Depois que o fluxo da solicitação é lido para um parâmetro, geralmente não é possível ler o fluxo da solicitação novamente para associar outros `[FromBody]` parâmetros.

> [!NOTE]
> O `JsonInputFormatter` é o formatador padrão e é baseada em [Json.NET](http://www.newtonsoft.com/json).

ASP.NET seleciona formatadores de entrada com base no [Content-Type](https://www.w3.org/Protocols/rfc1341/4_Content-Type.html) cabeçalho e o tipo do parâmetro, a menos que haja um atributo aplicado a ele especificando caso contrário. Se você gostaria de usar XML ou outro formato você deve configurá-lo no *Startup.cs* arquivo, mas você pode ser necessário que obter uma referência para `Microsoft.AspNetCore.Mvc.Formatters.Xml` usando o NuGet. O código de inicialização deve ter esta aparência:

<!-- literal_block {"ids": [], "linenos": true, "xml:space": "preserve", "language": "csharp"} -->

```csharp
public void ConfigureServices(IServiceCollection services)
   {
       services.AddMvc()
          .AddXmlSerializerFormatters();
   }
   ```

O código no *Startup.cs* arquivo contém um `ConfigureServices` método com um `services` argumento que você pode usar para criar serviços para seu aplicativo ASP.NET. No exemplo, estamos adicionando um formatador XML como um serviço que forneça MVC para este aplicativo. O `options` argumento passado para o `AddMvc` método permite que você adicionar e gerenciar filtros, formatadores e outras opções de sistema do MVC após a inicialização do aplicativo. Em seguida, aplique a `Consumes` de atributo para classes do controlador ou métodos de ação para trabalhar com o formato desejado.

### <a name="custom-model-binding"></a>Associação de modelo personalizado

Você pode estender a associação de modelo, escrevendo seus próprio associadores de modelo personalizado. Saiba mais sobre [associação de modelo personalizado](../advanced/custom-model-binding.md).

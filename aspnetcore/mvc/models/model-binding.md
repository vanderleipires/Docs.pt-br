---
title: Associação de modelos no ASP.NET Core
author: rachelappel
description: Saiba como a associação de modelos no ASP.NET Core MVC mapeia dados de solicitações HTTP para os parâmetros de método de ação.
ms.assetid: 0be164aa-1d72-4192-bd6b-192c9c301164
ms.author: rachelap
ms.date: 01/22/2018
uid: mvc/models/model-binding
ms.openlocfilehash: 4c1cfddf82e077e22e9069777393bc5e6086de83
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36278379"
---
# <a name="model-binding-in-aspnet-core"></a>Associação de modelos no ASP.NET Core

Por [Rachel Appel](https://github.com/rachelappel)

## <a name="introduction-to-model-binding"></a>Introdução à associação de modelos

A associação de modelos do ASP.NET Core MVC mapeia dados de solicitações HTTP para parâmetros de método de ação. Os parâmetros podem ser tipos simples, como cadeias de caracteres, inteiros ou floats, ou podem ser tipos complexos. Esse é um ótimo recurso do MVC, pois o mapeamento dos dados de entrada para um equivalente é um cenário repetido com frequência, independentemente do tamanho ou da complexidade dos dados. O MVC resolve esse problema com a abstração da associação, de modo que os desenvolvedores não precisem continuar reconfigurando uma versão ligeiramente diferente desse mesmo código em cada aplicativo. A escrita de seu próprio texto para tipar o código de conversor é entediante e propensa a erros.

## <a name="how-model-binding-works"></a>Como funciona a associação de modelos

Quando o MVC recebe uma solicitação HTTP, ele encaminha-a a um método de ação específico de um controlador. Ele determina qual método de ação será executado com base no que está nos dados de rota e, em seguida, ele associa valores da solicitação HTTP aos parâmetros desse método de ação. Por exemplo, considere a seguinte URL:

`http://contoso.com/movies/edit/2`

Como o modelo de rota é semelhante a isto, `{controller=Home}/{action=Index}/{id?}`, `movies/edit/2` encaminha para o controlador `Movies` e seu método de ação `Edit`. Ele também aceita um parâmetro opcional chamado `id`. O código para o método de ação deve ter esta aparência:

```csharp
public IActionResult Edit(int? id)
   ```

Observação: as cadeias de caracteres na rota de URL não diferenciam maiúsculas de minúsculas.

O MVC tentará associar os dados de solicitação aos parâmetros de ação por nome. O MVC procurará valores para cada parâmetro usando o nome do parâmetro e os nomes de suas propriedades configuráveis públicas. No exemplo acima, o único parâmetro de ação é chamado `id`, que o MVC associa ao valor com o mesmo nome nos valores de rota. Além dos valores de rota, o MVC associará dados de várias partes da solicitação e faz isso em uma ordem específica. Veja abaixo uma lista das fontes de dados na ordem em que a associação de modelos examina-as:

1. `Form values`: esses são valores de formulário que entram na solicitação HTTP usando o método POST. (incluindo as solicitações POST jQuery).

2. `Route values`: o conjunto de valores de rota fornecido pelo [Roteamento](xref:fundamentals/routing)

3. `Query strings`: a parte da cadeia de caracteres de consulta do URI.

<!-- DocFX BUG
The link works but generates an error when building with DocFX
@fundamentals/routing
[Routing](xref:fundamentals/routing)
-->

Observação: valores de formulário, dados de rota e cadeias de consulta são todos armazenados como pares nome-valor.

Como a associação de modelos solicitou uma chave chamada `id` e não há nada chamado `id` nos valores de formulário, ela passou para os valores de rota procurando essa chave. Em nosso exemplo, isso é uma correspondência. A associação ocorre e o valor é convertido no inteiro 2. A mesma solicitação que usa Edit(string id) converterá na cadeia de caracteres "2".

Até agora, o exemplo usa tipos simples. No MVC, os tipos simples são qualquer tipo primitivo do .NET ou um tipo com um conversor de tipo de cadeia de caracteres. Se o parâmetro do método de ação for uma classe, como o tipo `Movie`, que contém tipos simples e complexos como propriedades, a associação de modelos do MVC ainda o manipulará perfeitamente. Ele usa a reflexão e recursão para percorrer as propriedades de tipos complexos procurando correspondências. A associação de modelos procura o padrão *nome_do_parâmetro.nome_da_propriedade* para associar valores a propriedades. Se ela não encontrar os valores correspondentes desse formulário, ela tentará associar usando apenas o nome da propriedade. Para esses tipos como tipos `Collection`, a associação de modelos procura correspondências para *parameter_name [index]* ou apenas *[index]*. A associação de modelos trata tipos `Dictionary` da mesma forma, solicitando *parameter_name[key]* ou apenas *[key]*, desde que as chaves sejam tipos simples. As chaves compatíveis correspondem o HTML dos nomes de campo e os auxiliares de marca gerados para o mesmo tipo de modelo. Isso permite a ida e vinda dos valores, de modo que os campos de formulário permaneçam preenchidos com a entrada do usuário para sua conveniência, por exemplo, quando os dados associados de uma criação ou edição não são aprovados na validação.

Para que a associação ocorra, a classe precisa ter um construtor padrão público e o membro a ser associado precisa ser propriedades graváveis públicas. Quando a associação de modelos ocorrer, só será criada uma instância da classe com o construtor padrão público e, em seguida, as propriedades poderão ser definidas.

Quando um parâmetro é associado, a associação de modelos para de procurar valores com esse nome e ela passa para associar o próximo parâmetro. Caso contrário, o comportamento padrão da associação de modelos define parâmetros com seus valores padrão, dependendo de seu tipo:

* `T[]`: com a exceção de matrizes do tipo `byte[]`, a associação define parâmetros do tipo `T[]` como `Array.Empty<T>()`. Matrizes do tipo `byte[]` são definidas como `null`.

* Tipos de referência: a associação cria uma instância de uma classe com o construtor padrão sem configurar propriedades. No entanto, a associação de modelos define parâmetros `string` como `null`.

* Tipos que permitem valor nulo: os tipos que permitem valor nulo são definidos como `null`. No exemplo acima, a associação de modelos define `id` como `null`, pois ele é do tipo `int?`.

* Tipos de valor: os tipos de valor que não permitem valor nulo do tipo `T` são definidos como `default(T)`. Por exemplo, a associação de modelos definirá um parâmetro `int id` como 0. Considere o uso da validação de modelo ou de tipos que permitem valor nulo, em vez de depender de valores padrão.

Se a associação falhar, o MVC não gerará um erro. Todas as ações que aceitam a entrada do usuário devem verificar a propriedade `ModelState.IsValid`.

Observação: cada entrada na propriedade `ModelState` do controlador é uma `ModelStateEntry` que contém uma propriedade `Errors`. Raramente é necessário consultar essa coleção por conta própria. Use `ModelState.IsValid` em seu lugar.

Além disso, há alguns tipos de dados especiais que o MVC precisa considerar ao realizar a associação de modelos:

* `IFormFile`, `IEnumerable<IFormFile>`: um ou mais arquivos carregados que fazem parte da solicitação HTTP.

* `CancellationToken`: usado para cancelar a atividade em controladores assíncronos.

Esses tipos podem ser associados a parâmetros de ação ou a propriedades em um tipo de classe.

Após a conclusão da associação de modelos, a [Validação](validation.md) ocorrerá. A associação de modelos padrão funciona bem para a maioria dos cenários de desenvolvimento. Também é extensível e, portanto, se você tiver necessidades exclusivas, poderá personalizar o comportamento interno.

## <a name="customize-model-binding-behavior-with-attributes"></a>Personalizar o comportamento da associação de modelos com atributos

O MVC contém vários atributos que podem ser usados para direcionar seu comportamento de associação de modelos padrão para outra fonte. Por exemplo, você pode especificar se a associação é obrigatória para uma propriedade ou se ela nunca deve ocorrer usando os atributos `[BindRequired]` ou `[BindNever]`. Como alternativa, você pode substituir a fonte de dados padrão e especificar a fonte de dados do associador de modelos. Veja abaixo uma lista dos atributos de associação de modelos:

* `[BindRequired]`: esse atributo adiciona um erro de estado do modelo se a associação não pode ocorrer.

* `[BindNever]`: instrui o associador de modelos a nunca associar a esse parâmetro.

* `[FromHeader]`, `[FromQuery]`, `[FromRoute]`, `[FromForm]`: use-os para especificar a origem da associação exata que deseja aplicar.

* `[FromServices]`: esse atributo usa a [injeção de dependência](../../fundamentals/dependency-injection.md) para associar parâmetros de serviços.

* `[FromBody]`: use os formatadores configurados para associar dados do corpo da solicitação. O formatador é selecionado de acordo com o tipo de conteúdo da solicitação.

* `[ModelBinder]`: usado para substituir o associador de modelos padrão, a origem da associação e o nome.

Os atributos são ferramentas muito úteis quando você precisa substituir o comportamento padrão da associação de modelos.

## <a name="bind-formatted-data-from-the-request-body"></a>Associar dados formatados do corpo da solicitação

Os dados de solicitação podem ser recebidos em uma variedade de formatos, incluindo JSON, XML e muitos outros. Quando você usa o atributo [FromBody] para indicar que deseja associar um parâmetro a dados no corpo da solicitação, o MVC usa um conjunto configurado de formatadores para manipular os dados de solicitação de acordo com seu tipo de conteúdo. Por padrão, o MVC inclui uma classe `JsonInputFormatter` para manipular dados JSON, mas você pode adicionar outros formatadores para manipular XML e outros formatos personalizados.

> [!NOTE]
> Pode haver, no máximo, um parâmetro por ação decorado com `[FromBody]`. O tempo de execução do ASP.NET Core MVC delega a responsabilidade de ler o fluxo da solicitação ao formatador. Depois que o fluxo da solicitação é lido para um parâmetro, geralmente, não é possível ler o fluxo da solicitação novamente para associar outros parâmetros `[FromBody]`.

> [!NOTE]
> O `JsonInputFormatter` é o formatador padrão e se baseia no [Json.NET](https://www.newtonsoft.com/json).

O ASP.NET seleciona formatadores de entrada de acordo com o cabeçalho [Content-Type](https://www.w3.org/Protocols/rfc1341/4_Content-Type.html) e o tipo do parâmetro, a menos que haja um atributo aplicado a ele especificando o contrário. Se deseja usar XML ou outro formato, configure-o no arquivo *Startup.cs*, mas talvez você precise obter primeiro uma referência a `Microsoft.AspNetCore.Mvc.Formatters.Xml` usando o NuGet. O código de inicialização deverá ter uma aparência semelhante a esta:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc()
        .AddXmlSerializerFormatters();
   }
```

O código do arquivo *Startup.cs* contém um método `ConfigureServices` com um argumento `services` que você pode usar para criar serviços para o aplicativo ASP.NET. No exemplo, estamos adicionando um formatador XML como um serviço que o MVC fornecerá para este aplicativo. O argumento `options` passado para o método `AddMvc` permite que você adicione e gerencie filtros, formatadores e outras opções do sistema no MVC após a inicialização do aplicativo. Em seguida, aplique o atributo `Consumes` a classes do controlador ou métodos de ação para trabalhar com o formato desejado.

### <a name="custom-model-binding"></a>Associação de modelos personalizada

Estenda a associação de modelos escrevendo seus próprios associadores de modelos personalizados. Saiba mais sobre a [associação de modelos personalizada](../advanced/custom-model-binding.md).

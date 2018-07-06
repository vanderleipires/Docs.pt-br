---
uid: web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
title: Associação de parâmetro na API Web ASP.NET | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 07/11/2013
ms.assetid: e42c8388-04ed-4341-9fdb-41b1b4c06320
msc.legacyurl: /web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 97785db962691c25bac03f904924321af2f6b500
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37810076"
---
<a name="parameter-binding-in-aspnet-web-api"></a>Associação de parâmetro na API Web ASP.NET
====================
por [Mike Wasson](https://github.com/MikeWasson)

Quando a API da Web chama um método em um controlador, ele deve definir valores para os parâmetros, um processo chamado *associação*. Este artigo descreve como a API da Web associa parâmetros e como você pode personalizar o processo de associação.

Por padrão, a API da Web usa as regras a seguir para associar parâmetros:

- Se o parâmetro for um tipo de "simple", a API da Web tenta obter o valor do URI. Os tipos simples incluem o .NET [tipos primitivos](https://msdn.microsoft.com/library/system.type.isprimitive.aspx) (**int**, **bool**, **double**e assim por diante), além de **TimeSpan**, **DateTime**, **Guid**, **decimal**, e **string**, *plus* qualquer tipo com um conversor de tipo pode converter de uma cadeia de caracteres. (Mais informações sobre conversores de tipo posteriormente.)
- Para tipos complexos, a API da Web tenta ler o valor do corpo da mensagem, usando um [formatador do tipo de mídia](media-formatters.md).

Por exemplo, aqui está um método de controlador de API da Web típica:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample1.cs)]

O *identificação* parâmetro é um &quot;simples&quot; digitar, portanto, a API da Web tenta obter o valor do URI de solicitação. O *item* parâmetro é um tipo complexo, portanto, API Web usa um formatador de tipo de mídia para ler o valor do corpo da solicitação.

Para obter um valor do URI, API Web examina os dados da rota e a cadeia de caracteres de consulta do URI. Os dados da rota são preenchidos quando o sistema de roteamento analisa o URI e corresponde a uma rota. Para obter mais informações, consulte [roteamento e seleção de ação](../web-api-routing-and-actions/routing-and-action-selection.md).

No restante deste artigo, mostrarei como você pode personalizar o processo de associação de modelo. Para tipos complexos, no entanto, considere o uso de formatadores de tipo de mídia sempre que possível. Um princípio fundamental do HTTP é que os recursos são enviados no corpo da mensagem, usando a negociação de conteúdo para especificar a representação do recurso. Formatadores de tipo de mídia foram projetados para essa finalidade.

## <a name="using-fromuri"></a>Usando [FromUri]

Para forçar a API da Web para ler um tipo complexo do URI, adicione a **[FromUri]** ao parâmetro. O exemplo a seguir define uma `GeoPoint` tipo, juntamente com um método controlador que obtém o `GeoPoint` do URI.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample2.cs)]

O cliente pode colocar os valores de Latitude e Longitude na cadeia de caracteres de consulta e a API da Web usará para construir um `GeoPoint`. Por exemplo:

`http://localhost/api/values/?Latitude=47.678558&Longitude=-122.130989`

## <a name="using-frombody"></a>Usando [FromBody]

Para forçar a API da Web para um tipo simples de leitura do corpo da solicitação, adicione a **[FromBody]** ao parâmetro:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample3.cs)]

Neste exemplo, a API da Web usará um formatador de tipo de mídia para ler o valor de *nome* do corpo da solicitação. Aqui está um exemplo de solicitação de cliente.

[!code-console[Main](parameter-binding-in-aspnet-web-api/samples/sample4.cmd)]

Quando um parâmetro tiver [FromBody], API Web usa o cabeçalho Content-Type para selecionar um formatador. Neste exemplo, é o tipo de conteúdo &quot;application/json&quot; e o corpo da solicitação é uma cadeia de caracteres JSON bruta (não um objeto JSON).

No máximo um parâmetro tem permissão para ler o corpo da mensagem. Portanto, isso não funcionará:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample5.cs)]

O motivo para essa regra é que o corpo da solicitação pode ser armazenado em um fluxo não armazenado em buffer que só pode ser lidos uma vez.

## <a name="type-converters"></a>Conversores de tipo

Você pode fazer API da Web trate uma classe como um tipo simples (de modo que API Web tentará associá-lo do URI), criando uma **TypeConverter** e fornecendo uma conversão de cadeia de caracteres.

O código a seguir mostra uma `GeoPoint` a classe que representa um ponto geográfico, além de um **TypeConverter** que converte de cadeias de caracteres para `GeoPoint` instâncias. O `GeoPoint` classe é decorada com um **[TypeConverter]** atributo para especificar o conversor de tipo. (Este exemplo foi inspirado pelo postagem de blog de Mike Stall [como associar a objetos personalizados em assinaturas de ação no MVC/WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx).)

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample6.cs)]

Agora a API da Web tratará `GeoPoint` como um tipo simples, que significa que ele tentará associar `GeoPoint` parâmetros do URI. Você não precisa incluir **[FromUri]** no parâmetro.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample7.cs)]

O cliente pode invocar o método com um URI como este:

`http://localhost/api/values/?location=47.678558,-122.130989`

## <a name="model-binders"></a>Associadores de modelo

É uma opção mais flexível do que um conversor de tipo criar um associador de modelo personalizado. Com um associador de modelo, você tem acesso a coisas como a solicitação HTTP, a descrição da ação e os valores brutos dos dados de rota.

Para criar um associador de modelo, implemente a **IModelBinder** interface. Essa interface define um único método, **BindModel**:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample8.cs)]

Aqui está um associador de modelo para `GeoPoint` objetos.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample9.cs)]

Um associador de modelo obtém os valores brutos de entrada de um *provedor de valor*. Esse design separa duas funções distintas:

- O provedor de valor usa a solicitação HTTP e popula um dicionário de pares chave-valor.
- O associador de modelo usa este dicionário para popular o modelo.

O provedor de valor padrão na API da Web obtém valores de dados de rota e a cadeia de caracteres de consulta. Por exemplo, se o URI é `http://localhost/api/values/1?location=48,-122`, o provedor de valor cria os seguintes pares chave-valor:

- id = &quot;1&quot;
- local = &quot;48,122&quot;

(Estou supondo que o modelo de rota padrão, que é &quot;api / {controller} / {id}&quot;.)

O nome do parâmetro a associar é armazenado na **ModelBindingContext.ModelName** propriedade. O associador de modelos procura por uma chave com esse valor no dicionário. Se o valor existir e pode ser convertido em um `GeoPoint`, o associador de modelo atribui o valor associado para o **ModelBindingContext.Model** propriedade.

Observe que o associador de modelo não é limitado a uma conversão de tipo simples. Neste exemplo, o associador de modelo primeiro procura em uma tabela de locais conhecidos, e se isso falhar, ele usa uma conversão de tipo.

**Definindo o associador de modelo**

Há várias maneiras de definir um associador de modelo. Primeiro, você pode adicionar um **[ModelBinder]** ao parâmetro.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample10.cs)]

Você também pode adicionar um **[ModelBinder]** para o tipo de atributo. API da Web usará o associador de modelo especificado para todos os parâmetros desse tipo.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample11.cs)]

Por fim, você pode adicionar um provedor de associador de modelo para o **HttpConfiguration**. Um provedor de associador de modelo é simplesmente uma classe de fábrica que cria um associador de modelo. Você pode criar um provedor derivando de [ModelBinderProvider](https://msdn.microsoft.com/library/system.web.http.modelbinding.modelbinderprovider.aspx) classe. No entanto, se o associador de modelo manipula um único tipo, é mais fácil de usar interno **SimpleModelBinderProvider**, que é projetado para essa finalidade. O código a seguir mostra como fazer isso.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample12.cs)]

Com um provedor de associação de modelos, você ainda precisa adicionar o **[ModelBinder]** de atributo para o parâmetro, para informar a API da Web que ele deve usar um associador de modelo e não um formatador de tipo de mídia. Mas, agora você não precisa especificar o tipo de associador de modelo no atributo:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample13.cs)]

## <a name="value-providers"></a>Provedores de valor

Eu mencionei que um associador de modelo obtém valores de um provedor de valor. Para escrever um provedor de valor personalizado, implemente a **IValueProvider** interface. Aqui está um exemplo que extrai valores de cookies na solicitação:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample14.cs)]

Você também precisará criar uma fábrica de provedor de valor derivando de **ValueProviderFactory** classe.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample15.cs)]

Adicionar a fábrica de provedor de valor para o **HttpConfiguration** da seguinte maneira.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample16.cs)]

API da Web compõe todos os provedores de valor, portanto, quando um associador de modelo chama **ValueProvider.GetValue**, o associador de modelo recebe o valor do primeiro provedor de valor que é capaz de gerá-lo.

Como alternativa, você pode definir a fábrica de provedor de valor no nível de parâmetro usando o **ValueProvider** de atributo, da seguinte maneira:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample17.cs)]

Isso informa à API da Web para usar a associação de modelo com a fábrica de provedor de valor especificado e não para usar qualquer um dos outros provedores de valor registrados.

## <a name="httpparameterbinding"></a>HttpParameterBinding

Associadores de modelo são uma instância específica de um mecanismo mais geral. Se você examinar a **[ModelBinder]** atributo, você verá que ele deriva abstrata **ParameterBindingAttribute** classe. Essa classe define um único método, **GetBinding**, que retorna um **HttpParameterBinding** objeto:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample18.cs)]

Uma **HttpParameterBinding** é responsável por um parâmetro de associação para um valor. No caso de **[ModelBinder]**, o atributo retorna um **HttpParameterBinding** implementação que usa um **IModelBinder** para realizar a associação real. Você também pode implementar seu próprio **HttpParameterBinding**.

Por exemplo, suponha que você deseja obter ETags partir `if-match` e `if-none-match` cabeçalhos na solicitação. Vamos começar definindo uma classe para representar os ETags.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample19.cs)]

Também vamos definir uma enumeração para indicar se deve obter a ETag do `if-match` cabeçalho ou o `if-none-match` cabeçalho.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample20.cs)]

Aqui está uma **HttpParameterBinding** que obtém a ETag do cabeçalho de desejado e o associa a um parâmetro de tipo ETag:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample21.cs)]

O **ExecuteBindingAsync** método faz a associação. Dentro desse método, adicione o valor de parâmetro associados para o **ActionArgument** dicionário na **HttpActionContext**.

> [!NOTE]
> Se sua **ExecuteBindingAsync** método lê o corpo da mensagem de solicitação, substitua o **WillReadBody** propriedade retornar true. O corpo da solicitação pode ser um fluxo não armazenado em buffer que só pode ser lidos uma vez, portanto, a API da Web impõe uma regra que no máximo uma associação pode ler o corpo da mensagem.


Para aplicar um personalizado **HttpParameterBinding**, você pode definir um atributo derivado de **ParameterBindingAttribute**. Para `ETagParameterBinding`, vamos definir dois atributos, um para `if-match` cabeçalhos e outro para `if-none-match` cabeçalhos. Ambos derivam de uma classe base abstrata.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample22.cs)]

Aqui está um método controlador que usa o `[IfNoneMatch]` atributo.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample23.cs)]

Além disso **ParameterBindingAttribute**, há outro gancho para adicionar um personalizado **HttpParameterBinding**. Sobre o **HttpConfiguration** objeto, o **ParameterBindingRules** propriedade é uma coleção de funções de anomymous do tipo (**HttpParameterDescriptor**  - &gt; **HttpParameterBinding**). Por exemplo, você pode adicionar uma regra que usa qualquer parâmetro de ETag em um método GET `ETagParameterBinding` com `if-none-match`:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample24.cs)]

A função deve retornar `null` para parâmetros em que a associação não é aplicável.

## <a name="iactionvaluebinder"></a>IActionValueBinder

Todo o processo de associação de parâmetros é controlado por um serviço de conectável **IActionValueBinder**. A implementação padrão de **IActionValueBinder** faz o seguinte:

1. Procure uma **ParameterBindingAttribute** no parâmetro. Isso inclui **[FromBody]**, **[FromUri]**, e **[ModelBinder]**, ou atributos personalizados.
2. Caso contrário, examine **HttpConfiguration.ParameterBindingRules** para uma função que retorna não nulo **HttpParameterBinding**.
3. Caso contrário, use as regras padrão que descrevi anteriormente. 

    - Se o tipo de parâmetro é "simple" ou um conversor de tipo, associe-se do URI. Isso é equivalente a colocar o **[FromUri]** atributo no parâmetro.
    - Caso contrário, tente ler o parâmetro do corpo da mensagem. Isso é equivalente a colocar **[FromBody]** no parâmetro.

Se você quisesse, você poderia substituir todo o **IActionValueBinder** serviço com uma implementação personalizada.

## <a name="additional-resources"></a>Recursos adicionais

[Exemplo de associação de parâmetro personalizado](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/CustomParameterBinding/ReadMe.txt)

Mike Stall escreveu uma série de BOM de postagens de blog sobre a associação de parâmetros de API da Web:

- [Como a API da Web faz a associação de parâmetro](https://blogs.msdn.com/b/jmstall/archive/2012/04/16/how-webapi-does-parameter-binding.aspx)
- [Associação de parâmetro de estilo MVC para API da Web](https://blogs.msdn.com/b/jmstall/archive/2012/04/18/mvc-style-parameter-binding-for-webapi.aspx)
- [Como associar a objetos personalizados em assinaturas de ação na API da Web do MVC](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx)
- [Como criar um provedor de valor personalizados na API Web](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx)
- [Associação de parâmetros de API da Web nos bastidores](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx)

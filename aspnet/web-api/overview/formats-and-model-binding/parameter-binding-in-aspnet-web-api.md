---
uid: web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
title: "Parâmetro de associação no ASP.NET Web API | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/11/2013
ms.topic: article
ms.assetid: e42c8388-04ed-4341-9fdb-41b1b4c06320
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: ad052570fb2f168da657cd1263d8342a59d4cab0
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="parameter-binding-in-aspnet-web-api"></a>Parâmetro de associação no ASP.NET Web API
====================
por [Mike Wasson](https://github.com/MikeWasson)

Quando a API da Web chama um método em um controlador, ele deve definir valores para os parâmetros, um processo chamado *associação*. Este artigo descreve como a API da Web associa parâmetros e como você pode personalizar o processo de ligação.

Por padrão, a API da Web usa as regras a seguir para associar parâmetros:

- Se o parâmetro for um tipo "simples", a API da Web tenta obter o valor do URI. Os tipos simples incluem o .NET [tipos primitivos](https://msdn.microsoft.com/en-us/library/system.type.isprimitive.aspx) (**int**, **bool**, **duplo**, e assim por diante), além de **TimeSpan**, **DateTime**, **Guid**, **decimal**, e **cadeia de caracteres**, *mais* qualquer tipo com um conversor de tipo que possa ser convertido de uma cadeia de caracteres. (Mais informações sobre conversores de tipo posteriormente.)
- Para tipos complexos, API da Web tenta ler o valor do corpo da mensagem, usando um [formatador do tipo de mídia](media-formatters.md).

Por exemplo, aqui está um método comum de controlador de API da Web:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample1.cs)]

O *id* parâmetro é um &quot;simples&quot; tipo, portanto a API da Web tenta obter o valor de URI de solicitação. O *item* parâmetro é um tipo complexo, portanto, API da Web usa um formatador de tipo de mídia para ler o valor do corpo da solicitação.

Para obter um valor do URI, API da Web examina os dados da rota e a cadeia de caracteres de consulta URI. Os dados da rota são preenchidos quando o sistema de roteamento analisa o URI e corresponde a uma rota. Para obter mais informações, consulte [de roteamento e seleção de ação](../web-api-routing-and-actions/routing-and-action-selection.md).

O restante deste artigo, mostrarei como você pode personalizar o processo de associação de modelo. Para tipos complexos, no entanto, considere o uso de formatadores de tipo de mídia sempre que possível. Um princípio fundamental de HTTP é que os recursos são enviados no corpo da mensagem, usando a negociação de conteúdo para especificar a representação do recurso. Formatadores de tipo de mídia foram projetados para essa finalidade.

## <a name="using-fromuri"></a>Usando [FromUri]

Para forçar a API da Web para ler um tipo complexo do URI, adicione o **[FromUri]** ao parâmetro. O exemplo a seguir define uma `GeoPoint` tipo, junto com um método de controlador que obtém o `GeoPoint` do URI.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample2.cs)]

O cliente pode colocar os valores de Latitude e Longitude na cadeia de caracteres de consulta e a API da Web usará para construir um `GeoPoint`. Por exemplo:

`http://localhost/api/values/?Latitude=47.678558&Longitude=-122.130989`

## <a name="using-frombody"></a>Usando [FromBody]

Para forçar a API da Web para um tipo simples de leitura do corpo da solicitação, adicione o **[FromBody]** ao parâmetro:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample3.cs)]

Neste exemplo, a API da Web usará um formatador de tipo de mídia para ler o valor de *nome* o corpo da solicitação. Aqui está um exemplo de solicitação de cliente.

[!code-console[Main](parameter-binding-in-aspnet-web-api/samples/sample4.cmd)]

Quando um parâmetro tiver [FromBody], a API da Web usa o cabeçalho Content-Type para selecionar um formatador. Neste exemplo, o tipo de conteúdo é &quot;aplicativo/json&quot; e o corpo da solicitação é uma cadeia de caracteres JSON bruta (não um objeto JSON).

No máximo um parâmetro tem permissão para ler o corpo da mensagem. Então isso não funcionará:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample5.cs)]

O motivo para essa regra é que o corpo da solicitação pode ser armazenado em um fluxo não armazenado em buffer que só pode ser lidos uma vez.

## <a name="type-converters"></a>Conversores de tipo

Você pode fazer API da Web tratar uma classe como um tipo simples (de forma que API da Web tenta associá-lo do URI), criando um **TypeConverter** e fornecendo uma conversão de cadeia de caracteres.

O código a seguir mostra um `GeoPoint` a classe que representa um ponto geográfico, mais um **TypeConverter** que converte de cadeias de caracteres para `GeoPoint` instâncias. O `GeoPoint` classe é decorada com um **[TypeConverter]** atributo para especificar o conversor de tipo. (Este exemplo foi inspirado pela postagem de blog de Mike Stall [como vincular a objetos personalizados em assinaturas de ação no MVC/WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx).)

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample6.cs)]

Agora a API da Web tratará `GeoPoint` como um tipo simple, que significa que ele irá tentar associar `GeoPoint` parâmetros do URI. Você não precisa incluir **[FromUri]** no parâmetro.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample7.cs)]

O cliente pode invocar o método com um URI como este:

`http://localhost/api/values/?location=47.678558,-122.130989`

## <a name="model-binders"></a>Associadores de modelo

Uma opção mais flexível de um conversor de tipo é criar um associador de modelo personalizado. Com um associador de modelo, você tem acesso a coisas como a solicitação HTTP, a descrição da ação e os valores brutos dos dados da rota.

Para criar um associador de modelo, implementar a **IModelBinder** interface. Essa interface define um único método, **BindModel**:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample8.cs)]

Aqui está um associador de modelo para `GeoPoint` objetos.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample9.cs)]

Um associador de modelo obtém valores brutos de entrada de um *provedor de valor*. Esse design separa duas funções distintas:

- O provedor de valor leva a solicitação HTTP e preenche um dicionário de pares chave-valor.
- O associador de modelo usa este dicionário para popular o modelo.

O provedor de valor padrão na API da Web obtém valores de dados de rota e a cadeia de caracteres de consulta. Por exemplo, se o URI é `http://localhost/api/values/1?location=48,-122`, o provedor de valor cria os seguintes pares chave-valor:

- ID = &quot;1&quot;
- local = &quot;48,122&quot;

(Estou supondo que o modelo de rota padrão, que é &quot;api / {controller} / {id}&quot;.)

O nome do parâmetro a associar é armazenado no **ModelBindingContext.ModelName** propriedade. O associador de modelo procura uma chave com esse valor no dicionário. Se o valor existe e pode ser convertido em um `GeoPoint`, o associador de modelo atribui o valor de limite para o **ModelBindingContext.Model** propriedade.

Observe que o associador de modelo não é limitado a uma conversão de tipo simples. Neste exemplo, primeiro procura o associador de modelo em uma tabela de locais conhecidos, e se isso falhar, ele usa a conversão de tipo.

**Definir o associador de modelo**

Há várias maneiras de definir um associador de modelo. Primeiro, você pode adicionar uma **[ModelBinder]** ao parâmetro.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample10.cs)]

Você também pode adicionar uma **[ModelBinder]** para o tipo de atributo. API da Web usará o associador de modelo especificado para todos os parâmetros de tipo.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample11.cs)]

Por fim, você pode adicionar um provedor de associador de modelo para o **HttpConfiguration**. Um provedor de associador de modelo é simplesmente uma classe de fábrica que cria um associador de modelo. Você pode criar um provedor derivando de [ModelBinderProvider](https://msdn.microsoft.com/en-us/library/system.web.http.modelbinding.modelbinderprovider.aspx) classe. No entanto, se o associador de modelo manipula um único tipo, é mais fácil de usar interno **SimpleModelBinderProvider**, que é projetado para essa finalidade. O código a seguir mostra como fazer isso.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample12.cs)]

Com um provedor de associação de modelo, você ainda precisa adicionar o **[ModelBinder]** de atributo para o parâmetro, informe a API da Web que ele deve usar um associador de modelo e não um formatador do tipo de mídia. Mas, agora você não precisa especificar o tipo de associador de modelo no atributo:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample13.cs)]

## <a name="value-providers"></a>Provedores de valor

Mencionei que um associador de modelo obtém valores de um provedor de valor. Para escrever um provedor de valor personalizado, implemente o **IValueProvider** interface. Aqui está um exemplo que recebe os valores dos cookies na solicitação:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample14.cs)]

Você também precisa criar uma fábrica de provedor de valor derivando de **ValueProviderFactory** classe.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample15.cs)]

Adicionar a fábrica de provedor de valor para o **HttpConfiguration** da seguinte maneira.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample16.cs)]

API da Web compõe a todos os provedores de valor, portanto quando chama um associador de modelo **ValueProvider.GetValue**, o associador de modelo recebe o valor do primeiro provedor de valor que pode produzir a ele.

Como alternativa, você pode definir a fábrica de provedor de valor no nível de parâmetro usando o **ValueProvider** atributo, da seguinte maneira:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample17.cs)]

Isso informa a API da Web para usar a associação de modelo com a fábrica de provedor de valor especificado e para não usar nenhum dos provedores de valor registrado.

## <a name="httpparameterbinding"></a>HttpParameterBinding

Associadores de modelo são uma instância específica de um mecanismo mais geral. Se você observar o **[ModelBinder]** atributo, você verá que deriva de abstrata **ParameterBindingAttribute** classe. Essa classe define um único método, **GetBinding**, que retorna um **HttpParameterBinding** objeto:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample18.cs)]

Um **HttpParameterBinding** é responsável por um parâmetro de associação para um valor. No caso de **[ModelBinder]**, o atributo retorna um **HttpParameterBinding** implementação que usa um **IModelBinder** para realizar a associação real. Você também pode implementar seu próprio **HttpParameterBinding**.

Por exemplo, suponha que você deseja obter ETags de `if-match` e `if-none-match` cabeçalhos na solicitação. Vamos começar definindo uma classe para representar ETags.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample19.cs)]

Também vamos definir uma enumeração para indicar se deve obter o ETag a partir de `if-match` cabeçalho ou o `if-none-match` cabeçalho.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample20.cs)]

Aqui está uma **HttpParameterBinding** que obtém a ETag do cabeçalho desejado e o associa a um parâmetro de tipo ETag:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample21.cs)]

O **ExecuteBindingAsync** método faz a associação. Dentro desse método, adicione o valor de parâmetro associados para o **ActionArgument** dicionário no **HttpActionContext**.

> [!NOTE]
> Se seu **ExecuteBindingAsync** método lê o corpo da mensagem de solicitação, substitua o **WillReadBody** propriedade para retornar true. O corpo da solicitação pode ser um fluxo sem buffer que só pode ser lidos uma vez, para a API da Web aplica uma regra que no máximo uma associação pode ler o corpo da mensagem.


Para aplicar um personalizado **HttpParameterBinding**, você pode definir um atributo que é derivada de **ParameterBindingAttribute**. Para `ETagParameterBinding`, vamos definir dois atributos, uma para `if-match` cabeçalhos e outro para `if-none-match` cabeçalhos. Ambos derivam de uma classe base abstrata.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample22.cs)]

Este é um método de controlador que usa o `[IfNoneMatch]` atributo.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample23.cs)]

Além disso **ParameterBindingAttribute**, há outro gancho para adicionar um personalizado **HttpParameterBinding**. Sobre o **HttpConfiguration** objeto, o **ParameterBindingRules** propriedade é uma coleção de funções de anomymous do tipo (**HttpParameterDescriptor**  - &gt; **HttpParameterBinding**). Por exemplo, você pode adicionar uma regra que usa qualquer parâmetro de ETag em um método GET `ETagParameterBinding` com `if-none-match`:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample24.cs)]

A função deve retornar `null` para parâmetros, onde a associação não é aplicável.

## <a name="iactionvaluebinder"></a>IActionValueBinder

Todo o processo de associação de parâmetros é controlado por um serviço conectável, **IActionValueBinder**. A implementação padrão de **IActionValueBinder** faz o seguinte:

1. Procure um **ParameterBindingAttribute** no parâmetro. Isso inclui **[FromBody]**, **[FromUri]**, e **[ModelBinder]**, ou atributos personalizados.
2. Caso contrário, examine **HttpConfiguration.ParameterBindingRules** para uma função que retorna um nulo não **HttpParameterBinding**.
3. Caso contrário, use as regras padrão que descrita anteriormente. 

    - Se o tipo de parâmetro é "simple" ou possui um conversor de tipo, associe do URI. Isso é equivalente a colocar o **[FromUri]** atributo no parâmetro.
    - Caso contrário, tente ler o parâmetro do corpo da mensagem. Isso é equivalente a colocar **[FromBody]** no parâmetro.

Se desejar, você pode substituir toda a **IActionValueBinder** serviço com uma implementação personalizada.

## <a name="additional-resources"></a>Recursos adicionais

[Exemplo de associação de parâmetro personalizado](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/CustomParameterBinding/ReadMe.txt)

Mike Stall escreveu uma boa série de postagens de blog sobre associação de parâmetros de API da Web:

- [Como a API da Web faz a associação de parâmetro](https://blogs.msdn.com/b/jmstall/archive/2012/04/16/how-webapi-does-parameter-binding.aspx)
- [Associação de parâmetro de estilo MVC para API da Web](https://blogs.msdn.com/b/jmstall/archive/2012/04/18/mvc-style-parameter-binding-for-webapi.aspx)
- [Como associar a objetos personalizados em assinaturas de ação na API MVC/da Web](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx)
- [Como criar um provedor de valor personalizado na API da Web](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx)
- [Associação de parâmetros de API da Web nos bastidores](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx)

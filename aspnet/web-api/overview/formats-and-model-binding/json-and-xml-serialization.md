---
uid: web-api/overview/formats-and-model-binding/json-and-xml-serialization
title: JSON e a serialização de XML na API Web ASP.NET | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/30/2012
ms.topic: article
ms.assetid: 1cd7525d-de5e-4ab6-94f0-51480d3255d1
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/formats-and-model-binding/json-and-xml-serialization
msc.type: authoredcontent
ms.openlocfilehash: e7fbcd41d64651255763c7629f0232788dcb3d30
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37400915"
---
<a name="json-and-xml-serialization-in-aspnet-web-api"></a>JSON e a serialização de XML na API Web ASP.NET
====================
por [Mike Wasson](https://github.com/MikeWasson)

Este artigo descreve os formatadores JSON e XML na API Web ASP.NET.

Na API Web ASP.NET, uma *formatador do tipo de mídia* é um objeto que pode:

- Corpo da mensagem de objetos CLR de leitura de um HTTP
- Gravar objetos CLR em um corpo de mensagem HTTP

API da Web fornece formatadores de tipo de mídia para JSON e XML. O framework insere essas formatadores no pipeline por padrão. Os clientes podem solicitar o JSON ou XML no cabeçalho Accept da solicitação HTTP.

## <a name="contents"></a>Conteúdo

- [Formatador de tipo de mídia do JSON](#json_media_type_formatter)

    - [Propriedades somente leitura](#json_readonly)
    - [Datas](#json_dates)
    - [Recuar](#json_indenting)
    - [Concatenação com maiusculas e minúsculas](#json_camelcasing)
    - [Objetos anônimos e com tipagem fraca](#json_anon)
- [Formatador de tipo de mídia do XML](#xml_media_type_formatter)

    - [Propriedades somente leitura](#xml_readonly)
    - [Datas](#xml_dates)
    - [Recuar](#xml_indenting)
    - [Serializadores XML de por tipo de configuração](#xml_pertype)
- [Removendo o formatador XML ou JSON](#removing_the_json_or_xml_formatter)
- [Tratamento de referências de objeto Circular](#handling_circular_object_references)
- [Testando a serialização de objeto](#testing_object_serialization)

<a id="json_media_type_formatter"></a>
## <a name="json-media-type-formatter"></a>Formatador de tipo de mídia do JSON

Formatação do JSON é fornecido pelo **JsonMediaTypeFormatter** classe. Por padrão, **JsonMediaTypeFormatter** usa o [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) biblioteca para realizar a serialização. Json.NET é um projeto de software livre de terceiros.

Se você preferir, você pode configurar o **JsonMediaTypeFormatter** classe para usar o **DataContractJsonSerializer** em vez de Json.NET. Para fazer isso, defina as **UseDataContractJsonSerializer** propriedade **verdadeiro**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample1.cs)]

### <a name="json-serialization"></a>Serialização JSON

Esta seção descreve alguns comportamentos específicos do formatador JSON, usando o padrão [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) serializador. Isso não deve ser uma documentação abrangente da biblioteca Json.NET; Para obter mais informações, consulte o [documentação do Json.NET](http://james.newtonking.com/projects/json/help/).

#### <a name="what-gets-serialized"></a>O que é serializado?

Por padrão, todas as propriedades e campos públicos são incluídos no JSON serializado. Para omitir uma propriedade ou campo, decorá-la com o **JsonIgnore** atributo.

[!code-csharp[Main](json-and-xml-serialization/samples/sample2.cs)]

Se você preferir uma &quot;aceitação&quot; abordar, decore a classe com o **DataContract** atributo. Se esse atributo estiver presente, os membros serão ignorados a menos que tenham o **DataMember**. Você também pode usar **DataMember** para serializar os membros privados.

[!code-csharp[Main](json-and-xml-serialization/samples/sample3.cs)]

<a id="json_readonly"></a>
### <a name="read-only-properties"></a>Propriedades somente leitura

Propriedades somente leitura são serializadas por padrão.

<a id="json_dates"></a>
### <a name="dates"></a>Datas

Por padrão, o Json.NET grava as datas [ISO 8601](http://www.w3.org/TR/NOTE-datetime) formato. As datas em UTC (Coordinated Universal Time) são gravadas com um sufixo "Z". As datas no horário local incluem um deslocamento de fuso horário. Por exemplo:

[!code-console[Main](json-and-xml-serialization/samples/sample4.cmd)]

Por padrão, o Json.NET preserva o fuso horário. Você pode substituir isso definindo a propriedade DateTimeZoneHandling:

[!code-csharp[Main](json-and-xml-serialization/samples/sample5.cs)]

Se você preferir usar [formato de data JSON Microsoft](https://msdn.microsoft.com/library/bb299886.aspx#intro_to_json_sidebarb) (`"\/Date(ticks)\/"`) em vez de ISO 8601, defina as **DateFormatHandling** propriedade nas configurações do serializador:

[!code-csharp[Main](json-and-xml-serialization/samples/sample6.cs)]

<a id="json_indenting"></a>
### <a name="indenting"></a>Recuar

Para gravar recuado JSON, defina as **formatação** definir como **Formatting.Indented**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample7.cs)]

<a id="json_camelcasing"></a>
### <a name="camel-casing"></a>Concatenação com maiusculas e minúsculas

Para gravar os nomes de propriedade JSON com concatenação com maiusculas e minúsculas, sem alterar seu modelo de dados, defina a **CamelCasePropertyNamesContractResolver** sobre o serializador:

[!code-csharp[Main](json-and-xml-serialization/samples/sample8.cs)]

<a id="json_anon"></a>
### <a name="anonymous-and-weakly-typed-objects"></a>Objetos anônimos e com tipagem fraca

Um método de ação pode retornar um objeto anônimo e serializá-lo em JSON. Por exemplo:

[!code-csharp[Main](json-and-xml-serialization/samples/sample9.cs)]

O corpo da mensagem de resposta conterá o JSON a seguir:

[!code-json[Main](json-and-xml-serialization/samples/sample10.json)]

Se sua web API recebe vagamente estruturados objetos JSON de clientes, é possível desserializar o corpo da solicitação para um **Newtonsoft.Json.Linq.JObject** tipo.

[!code-csharp[Main](json-and-xml-serialization/samples/sample11.cs)]

No entanto, é melhor usar objetos com rigidez de tipos de dados. Em seguida, você não precisa analisar os dados por conta própria, e você obtém os benefícios da validação do modelo.

O serializador XML não oferece suporte a tipos anônimos ou **JObject** instâncias. Se você usar esses recursos para os dados JSON, você deve remover o formatador XML do pipeline, conforme descrito mais adiante neste artigo.

<a id="xml_media_type_formatter"></a>
## <a name="xml-media-type-formatter"></a>Formatador de tipo de mídia do XML

Formatação XML é fornecido pelo **XmlMediaTypeFormatter** classe. Por padrão, **XmlMediaTypeFormatter** usa o **DataContractSerializer** classe para executar a serialização.

Se você preferir, você pode configurar o **XmlMediaTypeFormatter** usar o **XmlSerializer** em vez da **DataContractSerializer**. Para fazer isso, defina as **UseXmlSerializer** propriedade **verdadeiro**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample12.cs)]

O **XmlSerializer** classe dá suporte a um conjunto mais estreito de tipos que **DataContractSerializer**, mas oferece mais controle sobre o XML resultante. Considere o uso de **XmlSerializer** se você precisar corresponder a um esquema XML existente.

### <a name="xml-serialization"></a>Serialização XML

Esta seção descreve alguns comportamentos específicos do formatador XML, usando o padrão **DataContractSerializer**.

Por padrão, o DataContractSerializer se comporta da seguinte maneira:

- Todos os campos e propriedades de leitura/gravação pública são serializados. Para omitir uma propriedade ou campo, decorá-la com o **IgnoreDataMember** atributo.
- Membros particulares e protegidos não são serializados.
- Propriedades somente leitura não são serializadas. (No entanto, o conteúdo de uma propriedade de coleção somente leitura é serializado.)
- Nomes de classe e membro são gravados no XML exatamente como aparecem na declaração da classe.
- Um namespace XML padrão é usado.

Se você precisar de mais controle sobre a serialização, você pode decorar a classe com o **DataContract** atributo. Quando esse atributo estiver presente, a classe é serializada da seguinte maneira:

- &quot;Aceitar&quot; abordagem: propriedades e campos não são serializados por padrão. Para serializar uma propriedade ou campo, decorá-la com o **DataMember** atributo.
- Para serializar um membro particular ou protegido, decorá-la com o **DataMember** atributo.
- Propriedades somente leitura não são serializadas.
- Para alterar como o nome de classe aparece no XML, defina as *nome* parâmetro na **DataContract** atributo.
- Para alterar a aparência de um nome de membro no XML, defina as *nome* parâmetro na **DataMember** atributo.
- Para alterar o namespace de XML, defina as *Namespace* parâmetro na **DataContract** classe.

<a id="xml_readonly"></a>
### <a name="read-only-properties"></a>Propriedades somente leitura

Propriedades somente leitura não são serializadas. Se uma propriedade somente leitura tem um campo de suporte particular, você pode marcar um campo particular com o **DataMember** atributo. Essa abordagem requer o **DataContract** atributo na classe.

[!code-csharp[Main](json-and-xml-serialization/samples/sample13.cs)]

<a id="xml_dates"></a>
### <a name="dates"></a>Datas

As datas são gravadas no formato ISO 8601. Por exemplo, &quot;2012-05-23T20:21:37.9116538Z&quot;.

<a id="xml_indenting"></a>
### <a name="indenting"></a>Recuar

Para gravar o XML recuado, defina as **Recuar** propriedade **verdadeiro**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample14.cs)]

<a id="xml_pertype"></a>
## <a name="setting-per-type-xml-serializers"></a>Serializadores XML de por tipo de configuração

Você pode definir diferentes serializadores XML para diferentes tipos CLR. Por exemplo, você pode ter um objeto de dados específico que requer **XmlSerializer** para compatibilidade com versões anteriores. Você pode usar **XmlSerializer** para este objeto e continuar a usar **DataContractSerializer** para outros tipos.

Para definir um serializador XML para um tipo específico, chame **SetSerializer**.

[!code-csharp[Main](json-and-xml-serialization/samples/sample15.cs)]

Você pode especificar uma **XmlSerializer** ou qualquer objeto derivado de **XmlObjectSerializer**.

<a id="removing_the_json_or_xml_formatter"></a>
## <a name="removing-the-json-or-xml-formatter"></a>Removendo o formatador XML ou JSON

Você pode remover o formatador JSON ou o formatador XML da lista de formatadores, se você não quiser usá-los. As principais razões para fazer isso são:

- Para restringir suas respostas de API da web para um determinado tipo de mídia. Por exemplo, você pode decidir dar suporte a apenas as respostas em JSON e remover o formatador XML.
- Para substituir o formatador padrão por um formatador personalizado. Por exemplo, você poderia substituir o formatador JSON com sua própria implementação personalizada de um formatador JSON.

O código a seguir mostra como remover os formatadores de padrão. Chamá-lo do seu **Application\_iniciar** método, definido no global. asax.

[!code-csharp[Main](json-and-xml-serialization/samples/sample16.cs)]

<a id="handling_circular_object_references"></a>
## <a name="handling-circular-object-references"></a>Tratamento de referências de objeto Circular

Por padrão, os formatadores JSON e XML gravam todos os objetos como valores. Se duas propriedades se referem ao mesmo objeto, ou se o mesmo objeto aparece duas vezes em uma coleção, o formatador de serializar o objeto duas vezes. Isso é um problema específico se seu gráfico de objeto contém ciclos, pois o serializador lançará uma exceção quando ela detecta um loop no gráfico.

Considere os seguintes modelos de objeto e o controlador.

[!code-csharp[Main](json-and-xml-serialization/samples/sample17.cs)]

Invocar essa ação fará com que o formatador a ser gerada uma exceção, o que resulta em um status código 500 (erro interno do servidor) de resposta ao cliente.

Para preservar as referências de objeto em JSON, adicione o seguinte código ao **Application\_iniciar** método no arquivo global. asax:

[!code-csharp[Main](json-and-xml-serialization/samples/sample18.cs)]

Agora, a ação do controlador retornará JSON tem esta aparência:

[!code-json[Main](json-and-xml-serialization/samples/sample19.json)]

Observe que o serializador adiciona uma &quot;$id&quot; propriedade para os dois objetos. Além disso, ele detecta que a propriedade Employee.Department cria um loop, portanto, ele substitui o valor com uma referência de objeto: {&quot;$ref&quot;:&quot;1&quot;}.

> [!NOTE]
> Referências de objeto não são padronizadas em JSON. Antes de usar esse recurso, considere se os clientes poderão analisar os resultados. Talvez seja melhor simplesmente remover ciclos do gráfico. Por exemplo, o link do funcionário para departamento não é realmente necessário neste exemplo.


Para preservar as referências de objeto no XML, você tem duas opções. A opção mais simples é adicionar `[DataContract(IsReference=true)]` à sua classe de modelo. O *IsReference* parâmetro permite que as referências de objeto. Lembre-se de que **DataContract** torna serialização opt-in, portanto, você também precisará adicionar **DataMember** atributos para as propriedades:

[!code-csharp[Main](json-and-xml-serialization/samples/sample20.cs)]

Agora, o formatador produzirá XML semelhante ao seguinte:

[!code-xml[Main](json-and-xml-serialization/samples/sample21.xml)]

Se você quiser evitar atributos em sua classe de modelo, há outra opção: criar um novo tipo específico **DataContractSerializer** da instância e defina *preserveObjectReferences* para **true**  no construtor. Defina essa instância como um serializador por tipo no formatador XML tipo de mídia. O código a seguir mostram como fazer isso:

[!code-csharp[Main](json-and-xml-serialization/samples/sample22.cs?highlight=3)]

<a id="testing_object_serialization"></a>
## <a name="testing-object-serialization"></a>Testando a serialização de objeto

Ao projetar sua API da web, é útil testar como seus objetos de dados serão serializados. Você pode fazer isso sem precisar criar um controlador ou invocar uma ação do controlador.

[!code-csharp[Main](json-and-xml-serialization/samples/sample23.cs)]

---
uid: web-api/overview/formats-and-model-binding/json-and-xml-serialization
title: Serialização de XML na API da Web ASP.NET e JSON | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/30/2012
ms.topic: article
ms.assetid: 1cd7525d-de5e-4ab6-94f0-51480d3255d1
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/json-and-xml-serialization
msc.type: authoredcontent
ms.openlocfilehash: b1fcaf70cc38d73da0a454764520197b97f34b26
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="json-and-xml-serialization-in-aspnet-web-api"></a>Serialização de XML na API da Web ASP.NET e JSON
====================
por [Mike Wasson](https://github.com/MikeWasson)

Este artigo descreve os formatadores JSON e XML na API da Web do ASP.NET.

Na API da Web do ASP.NET, um *formatador do tipo de mídia* é um objeto que pode:

- Corpo da mensagem de objetos CLR de leitura de um HTTP
- Gravar objetos CLR em um corpo de mensagem HTTP

API da Web fornece formatadores de tipo de mídia para JSON e XML. A estrutura insere essas formatadores no pipeline por padrão. Os clientes podem solicitar JSON ou XML no cabeçalho de aceitação da solicitação HTTP.

## <a name="contents"></a>Conteúdo

- [Formatador do tipo de mídia JSON](#json_media_type_formatter)

    - [Propriedades somente leitura](#json_readonly)
    - [Datas](#json_dates)
    - [Recuar](#json_indenting)
    - [Concatenação com maiusculas e minúsculas](#json_camelcasing)
    - [Objetos anônimos e tipo fraco](#json_anon)
- [Formatador do tipo de mídia XML](#xml_media_type_formatter)

    - [Propriedades somente leitura](#xml_readonly)
    - [Datas](#xml_dates)
    - [Recuar](#xml_indenting)
    - [Configuração por tipo XML serializadores](#xml_pertype)
- [Removendo o formatador XML ou JSON](#removing_the_json_or_xml_formatter)
- [Tratamento de referências de objeto Circular](#handling_circular_object_references)
- [Teste de serialização do objeto](#testing_object_serialization)

<a id="json_media_type_formatter"></a>
## <a name="json-media-type-formatter"></a>Formatador do tipo de mídia JSON

Formatação JSON é fornecido pelo **JsonMediaTypeFormatter** classe. Por padrão, **JsonMediaTypeFormatter** usa o [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) biblioteca para executar a serialização. Json.NET é um projeto de software livre de terceiros.

Se preferir, você pode configurar o **JsonMediaTypeFormatter** classe para usar o **DataContractJsonSerializer** em vez de Json.NET. Para fazer isso, defina o **UseDataContractJsonSerializer** propriedade **true**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample1.cs)]

### <a name="json-serialization"></a>Serialização JSON

Esta seção descreve alguns comportamentos específicos do formatador JSON, usando o padrão [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) serializador. Isso não deve ser a documentação abrangente de biblioteca do Json.NET; Para obter mais informações, consulte o [Json.NET documentação](http://james.newtonking.com/projects/json/help/).

#### <a name="what-gets-serialized"></a>O que é serializado?

Por padrão, todas as propriedades públicas e campos são incluídos em JSON serializado. Para omitir um campo ou propriedade, Decore-o com o **JsonIgnore** atributo.

[!code-csharp[Main](json-and-xml-serialization/samples/sample2.cs)]

Se você preferir um &quot;aceitar&quot; abordagem, decorar a classe com o **DataContract** atributo. Se esse atributo estiver presente, os membros serão ignorados a menos que tenham o **DataMember**. Você também pode usar **DataMember** para serializar membros particulares.

[!code-csharp[Main](json-and-xml-serialization/samples/sample3.cs)]

<a id="json_readonly"></a>
### <a name="read-only-properties"></a>Propriedades somente leitura

Propriedades somente leitura são serializadas por padrão.

<a id="json_dates"></a>
### <a name="dates"></a>datas

Por padrão, o Json.NET grava datas [ISO 8601](http://www.w3.org/TR/NOTE-datetime) formato. Datas em UTC (Coordinated Universal Time) são gravadas com um sufixo "Z". Datas no horário local incluem um deslocamento de fuso horário. Por exemplo:

[!code-console[Main](json-and-xml-serialization/samples/sample4.cmd)]

Por padrão, o Json.NET preserva o fuso horário. Você pode substituir isso definindo a propriedade DateTimeZoneHandling:

[!code-csharp[Main](json-and-xml-serialization/samples/sample5.cs)]

Se você preferir usar [formato de data do Microsoft JSON](https://msdn.microsoft.com/library/bb299886.aspx#intro_to_json_sidebarb) (`"\/Date(ticks)\/"`) em vez de ISO 8601, definir o **DateFormatHandling** propriedade nas configurações do serializador:

[!code-csharp[Main](json-and-xml-serialization/samples/sample6.cs)]

<a id="json_indenting"></a>
### <a name="indenting"></a>Recuar

Para gravar recuado JSON, defina o **formatação** definindo como **Formatting.Indented**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample7.cs)]

<a id="json_camelcasing"></a>
### <a name="camel-casing"></a>Concatenação com maiusculas e minúsculas

Para gravar os nomes de propriedade JSON com concatenação com maiusculas e minúsculas, sem alterar o modelo de dados, defina o **CamelCasePropertyNamesContractResolver** no serializador:

[!code-csharp[Main](json-and-xml-serialization/samples/sample8.cs)]

<a id="json_anon"></a>
### <a name="anonymous-and-weakly-typed-objects"></a>Objetos anônimos e tipo fraco

Um método de ação pode retornar um objeto anônimo e serializado para JSON. Por exemplo:

[!code-csharp[Main](json-and-xml-serialization/samples/sample9.cs)]

O corpo da mensagem de resposta conterá o JSON a seguir:

[!code-json[Main](json-and-xml-serialization/samples/sample10.json)]

Se sua web API recebe livremente estruturados objetos JSON de clientes, você pode desserializar o corpo da solicitação para um **Newtonsoft.Json.Linq.JObject** tipo.

[!code-csharp[Main](json-and-xml-serialization/samples/sample11.cs)]

No entanto, é melhor usar objetos de dados fortemente tipados. Em seguida, você não precisa analisar os dados por conta própria e obter os benefícios de validação do modelo.

O serializador XML não oferece suporte a tipos anônimos ou **JObject** instâncias. Se você usar esses recursos para os dados JSON, você deve remover o formatador XML do pipeline, conforme descrito posteriormente neste artigo.

<a id="xml_media_type_formatter"></a>
## <a name="xml-media-type-formatter"></a>Formatador do tipo de mídia XML

Formatação XML é fornecida pelo **XmlMediaTypeFormatter** classe. Por padrão, **XmlMediaTypeFormatter** usa o **DataContractSerializer** classe para realizar a serialização.

Se preferir, você pode configurar o **XmlMediaTypeFormatter** para usar o **XmlSerializer** em vez do **DataContractSerializer**. Para fazer isso, defina o **UseXmlSerializer** propriedade **true**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample12.cs)]

O **XmlSerializer** classe oferece suporte a um conjunto mais restrito de tipos de **DataContractSerializer**, mas oferece mais controle sobre o XML resultante. Considere o uso de **XmlSerializer** se você precisa corresponder a um esquema XML existente.

### <a name="xml-serialization"></a>Serialização XML

Esta seção descreve alguns comportamentos específicos do formatador XML, usando o padrão **DataContractSerializer**.

Por padrão, o DataContractSerializer se comporta da seguinte maneira:

- Todos os campos e propriedades públicas de leitura/gravação são serializados. Para omitir um campo ou propriedade, Decore-o com o **IgnoreDataMember** atributo.
- Membros Private e protected não são serializados.
- Propriedades somente leitura não são serializadas. (No entanto, o conteúdo de uma propriedade de coleção somente leitura é serializado.)
- Nomes de classe e de membro são gravados no XML exatamente como aparecem na declaração da classe.
- Um namespace XML padrão é usado.

Se você precisar de mais controle sobre a serialização, você pode decorar a classe com o **DataContract** atributo. Quando esse atributo estiver presente, a classe é serializada da seguinte maneira:

- &quot;Aceitar&quot; abordagem: propriedades e campos não são serializados por padrão. Para serializar uma propriedade ou campo, Decore-o com o **DataMember** atributo.
- Para serializar um membro particular ou protegido, Decore-o com o **DataMember** atributo.
- Propriedades somente leitura não são serializadas.
- Para alterar como o nome da classe aparece no XML, defina o *nome* parâmetro o **DataContract** atributo.
- Para alterar como um nome de membro aparece no XML, defina o *nome* parâmetro o **DataMember** atributo.
- Para alterar o namespace XML, defina o *Namespace* parâmetro o **DataContract** classe.

<a id="xml_readonly"></a>
### <a name="read-only-properties"></a>Propriedades somente leitura

Propriedades somente leitura não são serializadas. Se uma propriedade somente leitura tem um campo particular de backup, você pode marcar o campo privado com o **DataMember** atributo. Essa abordagem requer o **DataContract** atributo da classe.

[!code-csharp[Main](json-and-xml-serialization/samples/sample13.cs)]

<a id="xml_dates"></a>
### <a name="dates"></a>datas

As datas são gravadas no formato ISO 8601. Por exemplo, &quot;2012-05-23T20:21:37.9116538Z&quot;.

<a id="xml_indenting"></a>
### <a name="indenting"></a>Recuar

Para gravar XML recuado, defina o **recuo** propriedade **true**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample14.cs)]

<a id="xml_pertype"></a>
## <a name="setting-per-type-xml-serializers"></a>Configuração por tipo XML serializadores

Você pode definir os serializadores XML diferentes para diferentes tipos CLR. Por exemplo, você pode ter um objeto de dados específico que requer **XmlSerializer** para compatibilidade com versões anteriores. Você pode usar **XmlSerializer** para este objeto e continuar a usar **DataContractSerializer** para outros tipos.

Para definir um serializador XML para um tipo específico, chame **SetSerializer**.

[!code-csharp[Main](json-and-xml-serialization/samples/sample15.cs)]

Você pode especificar um **XmlSerializer** ou qualquer objeto derivado de **XmlObjectSerializer**.

<a id="removing_the_json_or_xml_formatter"></a>
## <a name="removing-the-json-or-xml-formatter"></a>Removendo o formatador XML ou JSON

Você pode remover o formatador JSON ou o formatador XML da lista de formatadores, se você não quiser usá-los. As principais razões para isso são:

- Para restringir suas respostas de API da web para um determinado tipo de mídia. Por exemplo, você pode decidir dar suporte a apenas respostas em JSON e remover o formatador XML.
- Para substituir o formatador padrão com um formatador personalizado. Por exemplo, você pode substituir o formatador JSON com sua própria implementação personalizada de um formatador JSON.

O código a seguir mostra como remover os formatadores padrão. Chamá-lo do seu **aplicativo\_iniciar** método definido em global. asax.

[!code-csharp[Main](json-and-xml-serialization/samples/sample16.cs)]

<a id="handling_circular_object_references"></a>
## <a name="handling-circular-object-references"></a>Tratamento de referências de objeto Circular

Por padrão, os formatadores JSON e XML gravam todos os objetos como valores. Se duas propriedades se referem ao mesmo objeto, ou se o mesmo objeto aparece duas vezes em uma coleção, o formatador serializa o objeto duas vezes. Isso é um problema específico se seu gráfico de objeto contém ciclos, porque o serializador lançará uma exceção quando ela detecta um loop no gráfico.

Considere os seguintes modelos de objeto e o controlador.

[!code-csharp[Main](json-and-xml-serialization/samples/sample17.cs)]

Invocar essa ação fará com que o formatador a ser gerada uma exceção, que converte um status código 500 (erro interno do servidor) na resposta ao cliente.

Para preservar as referências de objeto em JSON, adicione o seguinte código para **aplicativo\_iniciar** método no arquivo global. asax:

[!code-csharp[Main](json-and-xml-serialization/samples/sample18.cs)]

Agora, a ação de controlador retornará JSON tem esta aparência:

[!code-json[Main](json-and-xml-serialization/samples/sample19.json)]

Observe que o serializador adiciona um &quot;$id&quot; propriedade para os dois objetos. Além disso, ele detecta que a propriedade Employee.Department cria um loop, portanto, ele substitui o valor com uma referência de objeto: {&quot;$ref&quot;:&quot;1&quot;}.

> [!NOTE]
> Referências de objeto não são padronizadas em JSON. Antes de usar esse recurso, considere se os seus clientes serão capazes de analisar os resultados. Talvez seja melhor simplesmente remover ciclos do gráfico. Por exemplo, o link do funcionário para departamento não é realmente necessária neste exemplo.


Para preservar as referências de objeto em XML, você tem duas opções. A opção mais simples é adicionar `[DataContract(IsReference=true)]` à sua classe de modelo. O *IsReference* parâmetro permite referências de objeto. Lembre-se de que **DataContract** torna serialização participar, portanto, também será necessário adicionar **DataMember** atributos para as propriedades:

[!code-csharp[Main](json-and-xml-serialization/samples/sample20.cs)]

Agora o formatador produzirá XML semelhante ao seguinte:

[!code-xml[Main](json-and-xml-serialization/samples/sample21.xml)]

Se você quiser evitar atributos em sua classe de modelo, há outra opção: criar um novo tipo específico **DataContractSerializer** de instância e defina *preserveObjectReferences* para **true**  no construtor. Em seguida, defina essa instância como um serializador por tipo no formatador do tipo de mídia XML. O código a seguir mostram como fazer isso:

[!code-csharp[Main](json-and-xml-serialization/samples/sample22.cs?highlight=3)]

<a id="testing_object_serialization"></a>
## <a name="testing-object-serialization"></a>Teste de serialização do objeto

Ao projetar sua API da web, é útil testar como os objetos de dados serão serializados. Você pode fazer isso sem criar um controlador ou invocar uma ação do controlador.

[!code-csharp[Main](json-and-xml-serialization/samples/sample23.cs)]

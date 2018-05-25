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
<a name="json-and-xml-serialization-in-aspnet-web-api"></a><span data-ttu-id="beb7a-102">Serialização de XML na API da Web ASP.NET e JSON</span><span class="sxs-lookup"><span data-stu-id="beb7a-102">JSON and XML Serialization in ASP.NET Web API</span></span>
====================
<span data-ttu-id="beb7a-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="beb7a-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="beb7a-104">Este artigo descreve os formatadores JSON e XML na API da Web do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="beb7a-104">This article describes the JSON and XML formatters in ASP.NET Web API.</span></span>

<span data-ttu-id="beb7a-105">Na API da Web do ASP.NET, um *formatador do tipo de mídia* é um objeto que pode:</span><span class="sxs-lookup"><span data-stu-id="beb7a-105">In ASP.NET Web API, a *media-type formatter* is an object that can:</span></span>

- <span data-ttu-id="beb7a-106">Corpo da mensagem de objetos CLR de leitura de um HTTP</span><span class="sxs-lookup"><span data-stu-id="beb7a-106">Read CLR objects from an HTTP message body</span></span>
- <span data-ttu-id="beb7a-107">Gravar objetos CLR em um corpo de mensagem HTTP</span><span class="sxs-lookup"><span data-stu-id="beb7a-107">Write CLR objects into an HTTP message body</span></span>

<span data-ttu-id="beb7a-108">API da Web fornece formatadores de tipo de mídia para JSON e XML.</span><span class="sxs-lookup"><span data-stu-id="beb7a-108">Web API provides media-type formatters for both JSON and XML.</span></span> <span data-ttu-id="beb7a-109">A estrutura insere essas formatadores no pipeline por padrão.</span><span class="sxs-lookup"><span data-stu-id="beb7a-109">The framework inserts these formatters into the pipeline by default.</span></span> <span data-ttu-id="beb7a-110">Os clientes podem solicitar JSON ou XML no cabeçalho de aceitação da solicitação HTTP.</span><span class="sxs-lookup"><span data-stu-id="beb7a-110">Clients can request either JSON or XML in the Accept header of the HTTP request.</span></span>

## <a name="contents"></a><span data-ttu-id="beb7a-111">Conteúdo</span><span class="sxs-lookup"><span data-stu-id="beb7a-111">Contents</span></span>

- [<span data-ttu-id="beb7a-112">Formatador do tipo de mídia JSON</span><span class="sxs-lookup"><span data-stu-id="beb7a-112">JSON Media-Type Formatter</span></span>](#json_media_type_formatter)

    - [<span data-ttu-id="beb7a-113">Propriedades somente leitura</span><span class="sxs-lookup"><span data-stu-id="beb7a-113">Read-Only Properties</span></span>](#json_readonly)
    - [<span data-ttu-id="beb7a-114">Datas</span><span class="sxs-lookup"><span data-stu-id="beb7a-114">Dates</span></span>](#json_dates)
    - [<span data-ttu-id="beb7a-115">Recuar</span><span class="sxs-lookup"><span data-stu-id="beb7a-115">Indenting</span></span>](#json_indenting)
    - [<span data-ttu-id="beb7a-116">Concatenação com maiusculas e minúsculas</span><span class="sxs-lookup"><span data-stu-id="beb7a-116">Camel Casing</span></span>](#json_camelcasing)
    - [<span data-ttu-id="beb7a-117">Objetos anônimos e tipo fraco</span><span class="sxs-lookup"><span data-stu-id="beb7a-117">Anonymous and Weakly-Typed Objects</span></span>](#json_anon)
- [<span data-ttu-id="beb7a-118">Formatador do tipo de mídia XML</span><span class="sxs-lookup"><span data-stu-id="beb7a-118">XML Media-Type Formatter</span></span>](#xml_media_type_formatter)

    - [<span data-ttu-id="beb7a-119">Propriedades somente leitura</span><span class="sxs-lookup"><span data-stu-id="beb7a-119">Read-Only Properties</span></span>](#xml_readonly)
    - [<span data-ttu-id="beb7a-120">Datas</span><span class="sxs-lookup"><span data-stu-id="beb7a-120">Dates</span></span>](#xml_dates)
    - [<span data-ttu-id="beb7a-121">Recuar</span><span class="sxs-lookup"><span data-stu-id="beb7a-121">Indenting</span></span>](#xml_indenting)
    - [<span data-ttu-id="beb7a-122">Configuração por tipo XML serializadores</span><span class="sxs-lookup"><span data-stu-id="beb7a-122">Setting Per-Type XML Serializers</span></span>](#xml_pertype)
- [<span data-ttu-id="beb7a-123">Removendo o formatador XML ou JSON</span><span class="sxs-lookup"><span data-stu-id="beb7a-123">Removing the JSON or XML Formatter</span></span>](#removing_the_json_or_xml_formatter)
- [<span data-ttu-id="beb7a-124">Tratamento de referências de objeto Circular</span><span class="sxs-lookup"><span data-stu-id="beb7a-124">Handling Circular Object References</span></span>](#handling_circular_object_references)
- [<span data-ttu-id="beb7a-125">Teste de serialização do objeto</span><span class="sxs-lookup"><span data-stu-id="beb7a-125">Testing Object Serialization</span></span>](#testing_object_serialization)

<a id="json_media_type_formatter"></a>
## <a name="json-media-type-formatter"></a><span data-ttu-id="beb7a-126">Formatador do tipo de mídia JSON</span><span class="sxs-lookup"><span data-stu-id="beb7a-126">JSON Media-Type Formatter</span></span>

<span data-ttu-id="beb7a-127">Formatação JSON é fornecido pelo **JsonMediaTypeFormatter** classe.</span><span class="sxs-lookup"><span data-stu-id="beb7a-127">JSON formatting is provided by the **JsonMediaTypeFormatter** class.</span></span> <span data-ttu-id="beb7a-128">Por padrão, **JsonMediaTypeFormatter** usa o [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) biblioteca para executar a serialização.</span><span class="sxs-lookup"><span data-stu-id="beb7a-128">By default, **JsonMediaTypeFormatter** uses the [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) library to perform serialization.</span></span> <span data-ttu-id="beb7a-129">Json.NET é um projeto de software livre de terceiros.</span><span class="sxs-lookup"><span data-stu-id="beb7a-129">Json.NET is a third-party open source project.</span></span>

<span data-ttu-id="beb7a-130">Se preferir, você pode configurar o **JsonMediaTypeFormatter** classe para usar o **DataContractJsonSerializer** em vez de Json.NET.</span><span class="sxs-lookup"><span data-stu-id="beb7a-130">If you prefer, you can configure the **JsonMediaTypeFormatter** class to use the **DataContractJsonSerializer** instead of Json.NET.</span></span> <span data-ttu-id="beb7a-131">Para fazer isso, defina o **UseDataContractJsonSerializer** propriedade **true**:</span><span class="sxs-lookup"><span data-stu-id="beb7a-131">To do so, set the **UseDataContractJsonSerializer** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample1.cs)]

### <a name="json-serialization"></a><span data-ttu-id="beb7a-132">Serialização JSON</span><span class="sxs-lookup"><span data-stu-id="beb7a-132">JSON Serialization</span></span>

<span data-ttu-id="beb7a-133">Esta seção descreve alguns comportamentos específicos do formatador JSON, usando o padrão [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) serializador.</span><span class="sxs-lookup"><span data-stu-id="beb7a-133">This section describes some specific behaviors of the JSON formatter, using the default [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) serializer.</span></span> <span data-ttu-id="beb7a-134">Isso não deve ser a documentação abrangente de biblioteca do Json.NET; Para obter mais informações, consulte o [Json.NET documentação](http://james.newtonking.com/projects/json/help/).</span><span class="sxs-lookup"><span data-stu-id="beb7a-134">This is not meant to be comprehensive documentation of the Json.NET library; for more information, see the [Json.NET Documentation](http://james.newtonking.com/projects/json/help/).</span></span>

#### <a name="what-gets-serialized"></a><span data-ttu-id="beb7a-135">O que é serializado?</span><span class="sxs-lookup"><span data-stu-id="beb7a-135">What Gets Serialized?</span></span>

<span data-ttu-id="beb7a-136">Por padrão, todas as propriedades públicas e campos são incluídos em JSON serializado.</span><span class="sxs-lookup"><span data-stu-id="beb7a-136">By default, all public properties and fields are included in the serialized JSON.</span></span> <span data-ttu-id="beb7a-137">Para omitir um campo ou propriedade, Decore-o com o **JsonIgnore** atributo.</span><span class="sxs-lookup"><span data-stu-id="beb7a-137">To omit a property or field, decorate it with the **JsonIgnore** attribute.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample2.cs)]

<span data-ttu-id="beb7a-138">Se você preferir um &quot;aceitar&quot; abordagem, decorar a classe com o **DataContract** atributo.</span><span class="sxs-lookup"><span data-stu-id="beb7a-138">If you prefer an &quot;opt-in&quot; approach, decorate the class with the **DataContract** attribute.</span></span> <span data-ttu-id="beb7a-139">Se esse atributo estiver presente, os membros serão ignorados a menos que tenham o **DataMember**.</span><span class="sxs-lookup"><span data-stu-id="beb7a-139">If this attribute is present, members are ignored unless they have the **DataMember**.</span></span> <span data-ttu-id="beb7a-140">Você também pode usar **DataMember** para serializar membros particulares.</span><span class="sxs-lookup"><span data-stu-id="beb7a-140">You can also use **DataMember** to serialize private members.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample3.cs)]

<a id="json_readonly"></a>
### <a name="read-only-properties"></a><span data-ttu-id="beb7a-141">Propriedades somente leitura</span><span class="sxs-lookup"><span data-stu-id="beb7a-141">Read-Only Properties</span></span>

<span data-ttu-id="beb7a-142">Propriedades somente leitura são serializadas por padrão.</span><span class="sxs-lookup"><span data-stu-id="beb7a-142">Read-only properties are serialized by default.</span></span>

<a id="json_dates"></a>
### <a name="dates"></a><span data-ttu-id="beb7a-143">datas</span><span class="sxs-lookup"><span data-stu-id="beb7a-143">Dates</span></span>

<span data-ttu-id="beb7a-144">Por padrão, o Json.NET grava datas [ISO 8601](http://www.w3.org/TR/NOTE-datetime) formato.</span><span class="sxs-lookup"><span data-stu-id="beb7a-144">By default, Json.NET writes dates in [ISO 8601](http://www.w3.org/TR/NOTE-datetime) format.</span></span> <span data-ttu-id="beb7a-145">Datas em UTC (Coordinated Universal Time) são gravadas com um sufixo "Z".</span><span class="sxs-lookup"><span data-stu-id="beb7a-145">Dates in UTC (Coordinated Universal Time) are written with a "Z" suffix.</span></span> <span data-ttu-id="beb7a-146">Datas no horário local incluem um deslocamento de fuso horário.</span><span class="sxs-lookup"><span data-stu-id="beb7a-146">Dates in local time include a time-zone offset.</span></span> <span data-ttu-id="beb7a-147">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="beb7a-147">For example:</span></span>

[!code-console[Main](json-and-xml-serialization/samples/sample4.cmd)]

<span data-ttu-id="beb7a-148">Por padrão, o Json.NET preserva o fuso horário.</span><span class="sxs-lookup"><span data-stu-id="beb7a-148">By default, Json.NET preserves the time zone.</span></span> <span data-ttu-id="beb7a-149">Você pode substituir isso definindo a propriedade DateTimeZoneHandling:</span><span class="sxs-lookup"><span data-stu-id="beb7a-149">You can override this by setting the DateTimeZoneHandling property:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample5.cs)]

<span data-ttu-id="beb7a-150">Se você preferir usar [formato de data do Microsoft JSON](https://msdn.microsoft.com/library/bb299886.aspx#intro_to_json_sidebarb) (`"\/Date(ticks)\/"`) em vez de ISO 8601, definir o **DateFormatHandling** propriedade nas configurações do serializador:</span><span class="sxs-lookup"><span data-stu-id="beb7a-150">If you prefer to use [Microsoft JSON date format](https://msdn.microsoft.com/library/bb299886.aspx#intro_to_json_sidebarb) (`"\/Date(ticks)\/"`) instead of ISO 8601, set the **DateFormatHandling** property on the serializer settings:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample6.cs)]

<a id="json_indenting"></a>
### <a name="indenting"></a><span data-ttu-id="beb7a-151">Recuar</span><span class="sxs-lookup"><span data-stu-id="beb7a-151">Indenting</span></span>

<span data-ttu-id="beb7a-152">Para gravar recuado JSON, defina o **formatação** definindo como **Formatting.Indented**:</span><span class="sxs-lookup"><span data-stu-id="beb7a-152">To write indented JSON, set the **Formatting** setting to **Formatting.Indented**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample7.cs)]

<a id="json_camelcasing"></a>
### <a name="camel-casing"></a><span data-ttu-id="beb7a-153">Concatenação com maiusculas e minúsculas</span><span class="sxs-lookup"><span data-stu-id="beb7a-153">Camel Casing</span></span>

<span data-ttu-id="beb7a-154">Para gravar os nomes de propriedade JSON com concatenação com maiusculas e minúsculas, sem alterar o modelo de dados, defina o **CamelCasePropertyNamesContractResolver** no serializador:</span><span class="sxs-lookup"><span data-stu-id="beb7a-154">To write JSON property names with camel casing, without changing your data model, set the **CamelCasePropertyNamesContractResolver** on the serializer:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample8.cs)]

<a id="json_anon"></a>
### <a name="anonymous-and-weakly-typed-objects"></a><span data-ttu-id="beb7a-155">Objetos anônimos e tipo fraco</span><span class="sxs-lookup"><span data-stu-id="beb7a-155">Anonymous and Weakly-Typed Objects</span></span>

<span data-ttu-id="beb7a-156">Um método de ação pode retornar um objeto anônimo e serializado para JSON.</span><span class="sxs-lookup"><span data-stu-id="beb7a-156">An action method can return an anonymous object and serialize it to JSON.</span></span> <span data-ttu-id="beb7a-157">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="beb7a-157">For example:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample9.cs)]

<span data-ttu-id="beb7a-158">O corpo da mensagem de resposta conterá o JSON a seguir:</span><span class="sxs-lookup"><span data-stu-id="beb7a-158">The response message body will contain the following JSON:</span></span>

[!code-json[Main](json-and-xml-serialization/samples/sample10.json)]

<span data-ttu-id="beb7a-159">Se sua web API recebe livremente estruturados objetos JSON de clientes, você pode desserializar o corpo da solicitação para um **Newtonsoft.Json.Linq.JObject** tipo.</span><span class="sxs-lookup"><span data-stu-id="beb7a-159">If your web API receives loosely structured JSON objects from clients, you can deserialize the request body to a **Newtonsoft.Json.Linq.JObject** type.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample11.cs)]

<span data-ttu-id="beb7a-160">No entanto, é melhor usar objetos de dados fortemente tipados.</span><span class="sxs-lookup"><span data-stu-id="beb7a-160">However, it is usually better to use strongly typed data objects.</span></span> <span data-ttu-id="beb7a-161">Em seguida, você não precisa analisar os dados por conta própria e obter os benefícios de validação do modelo.</span><span class="sxs-lookup"><span data-stu-id="beb7a-161">Then you don't need to parse the data yourself, and you get the benefits of model validation.</span></span>

<span data-ttu-id="beb7a-162">O serializador XML não oferece suporte a tipos anônimos ou **JObject** instâncias.</span><span class="sxs-lookup"><span data-stu-id="beb7a-162">The XML serializer does not support anonymous types or **JObject** instances.</span></span> <span data-ttu-id="beb7a-163">Se você usar esses recursos para os dados JSON, você deve remover o formatador XML do pipeline, conforme descrito posteriormente neste artigo.</span><span class="sxs-lookup"><span data-stu-id="beb7a-163">If you use these features for your JSON data, you should remove the XML formatter from the pipeline, as described later in this article.</span></span>

<a id="xml_media_type_formatter"></a>
## <a name="xml-media-type-formatter"></a><span data-ttu-id="beb7a-164">Formatador do tipo de mídia XML</span><span class="sxs-lookup"><span data-stu-id="beb7a-164">XML Media-Type Formatter</span></span>

<span data-ttu-id="beb7a-165">Formatação XML é fornecida pelo **XmlMediaTypeFormatter** classe.</span><span class="sxs-lookup"><span data-stu-id="beb7a-165">XML formatting is provided by the **XmlMediaTypeFormatter** class.</span></span> <span data-ttu-id="beb7a-166">Por padrão, **XmlMediaTypeFormatter** usa o **DataContractSerializer** classe para realizar a serialização.</span><span class="sxs-lookup"><span data-stu-id="beb7a-166">By default, **XmlMediaTypeFormatter** uses the **DataContractSerializer** class to perform serialization.</span></span>

<span data-ttu-id="beb7a-167">Se preferir, você pode configurar o **XmlMediaTypeFormatter** para usar o **XmlSerializer** em vez do **DataContractSerializer**.</span><span class="sxs-lookup"><span data-stu-id="beb7a-167">If you prefer, you can configure the **XmlMediaTypeFormatter** to use the **XmlSerializer** instead of the **DataContractSerializer**.</span></span> <span data-ttu-id="beb7a-168">Para fazer isso, defina o **UseXmlSerializer** propriedade **true**:</span><span class="sxs-lookup"><span data-stu-id="beb7a-168">To do so, set the **UseXmlSerializer** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample12.cs)]

<span data-ttu-id="beb7a-169">O **XmlSerializer** classe oferece suporte a um conjunto mais restrito de tipos de **DataContractSerializer**, mas oferece mais controle sobre o XML resultante.</span><span class="sxs-lookup"><span data-stu-id="beb7a-169">The **XmlSerializer** class supports a narrower set of types than **DataContractSerializer**, but gives more control over the resulting XML.</span></span> <span data-ttu-id="beb7a-170">Considere o uso de **XmlSerializer** se você precisa corresponder a um esquema XML existente.</span><span class="sxs-lookup"><span data-stu-id="beb7a-170">Consider using **XmlSerializer** if you need to match an existing XML schema.</span></span>

### <a name="xml-serialization"></a><span data-ttu-id="beb7a-171">Serialização XML</span><span class="sxs-lookup"><span data-stu-id="beb7a-171">XML Serialization</span></span>

<span data-ttu-id="beb7a-172">Esta seção descreve alguns comportamentos específicos do formatador XML, usando o padrão **DataContractSerializer**.</span><span class="sxs-lookup"><span data-stu-id="beb7a-172">This section describes some specific behaviors of the XML formatter, using the default **DataContractSerializer**.</span></span>

<span data-ttu-id="beb7a-173">Por padrão, o DataContractSerializer se comporta da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="beb7a-173">By default, the DataContractSerializer behaves as follows:</span></span>

- <span data-ttu-id="beb7a-174">Todos os campos e propriedades públicas de leitura/gravação são serializados.</span><span class="sxs-lookup"><span data-stu-id="beb7a-174">All public read/write properties and fields are serialized.</span></span> <span data-ttu-id="beb7a-175">Para omitir um campo ou propriedade, Decore-o com o **IgnoreDataMember** atributo.</span><span class="sxs-lookup"><span data-stu-id="beb7a-175">To omit a property or field, decorate it with the **IgnoreDataMember** attribute.</span></span>
- <span data-ttu-id="beb7a-176">Membros Private e protected não são serializados.</span><span class="sxs-lookup"><span data-stu-id="beb7a-176">Private and protected members are not serialized.</span></span>
- <span data-ttu-id="beb7a-177">Propriedades somente leitura não são serializadas.</span><span class="sxs-lookup"><span data-stu-id="beb7a-177">Read-only properties are not serialized.</span></span> <span data-ttu-id="beb7a-178">(No entanto, o conteúdo de uma propriedade de coleção somente leitura é serializado.)</span><span class="sxs-lookup"><span data-stu-id="beb7a-178">(However, the contents of a read-only collection property are serialized.)</span></span>
- <span data-ttu-id="beb7a-179">Nomes de classe e de membro são gravados no XML exatamente como aparecem na declaração da classe.</span><span class="sxs-lookup"><span data-stu-id="beb7a-179">Class and member names are written in the XML exactly as they appear in the class declaration.</span></span>
- <span data-ttu-id="beb7a-180">Um namespace XML padrão é usado.</span><span class="sxs-lookup"><span data-stu-id="beb7a-180">A default XML namespace is used.</span></span>

<span data-ttu-id="beb7a-181">Se você precisar de mais controle sobre a serialização, você pode decorar a classe com o **DataContract** atributo.</span><span class="sxs-lookup"><span data-stu-id="beb7a-181">If you need more control over the serialization, you can decorate the class with the **DataContract** attribute.</span></span> <span data-ttu-id="beb7a-182">Quando esse atributo estiver presente, a classe é serializada da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="beb7a-182">When this attribute is present, the class is serialized as follows:</span></span>

- <span data-ttu-id="beb7a-183">&quot;Aceitar&quot; abordagem: propriedades e campos não são serializados por padrão.</span><span class="sxs-lookup"><span data-stu-id="beb7a-183">&quot;Opt in&quot; approach: Properties and fields are not serialized by default.</span></span> <span data-ttu-id="beb7a-184">Para serializar uma propriedade ou campo, Decore-o com o **DataMember** atributo.</span><span class="sxs-lookup"><span data-stu-id="beb7a-184">To serialize a property or field, decorate it with the **DataMember** attribute.</span></span>
- <span data-ttu-id="beb7a-185">Para serializar um membro particular ou protegido, Decore-o com o **DataMember** atributo.</span><span class="sxs-lookup"><span data-stu-id="beb7a-185">To serialize a private or protected member, decorate it with the **DataMember** attribute.</span></span>
- <span data-ttu-id="beb7a-186">Propriedades somente leitura não são serializadas.</span><span class="sxs-lookup"><span data-stu-id="beb7a-186">Read-only properties are not serialized.</span></span>
- <span data-ttu-id="beb7a-187">Para alterar como o nome da classe aparece no XML, defina o *nome* parâmetro o **DataContract** atributo.</span><span class="sxs-lookup"><span data-stu-id="beb7a-187">To change how the class name appears in the XML, set the *Name* parameter in the **DataContract** attribute.</span></span>
- <span data-ttu-id="beb7a-188">Para alterar como um nome de membro aparece no XML, defina o *nome* parâmetro o **DataMember** atributo.</span><span class="sxs-lookup"><span data-stu-id="beb7a-188">To change how a member name appears in the XML, set the *Name* parameter in the **DataMember** attribute.</span></span>
- <span data-ttu-id="beb7a-189">Para alterar o namespace XML, defina o *Namespace* parâmetro o **DataContract** classe.</span><span class="sxs-lookup"><span data-stu-id="beb7a-189">To change the XML namespace, set the *Namespace* parameter in the **DataContract** class.</span></span>

<a id="xml_readonly"></a>
### <a name="read-only-properties"></a><span data-ttu-id="beb7a-190">Propriedades somente leitura</span><span class="sxs-lookup"><span data-stu-id="beb7a-190">Read-Only Properties</span></span>

<span data-ttu-id="beb7a-191">Propriedades somente leitura não são serializadas.</span><span class="sxs-lookup"><span data-stu-id="beb7a-191">Read-only properties are not serialized.</span></span> <span data-ttu-id="beb7a-192">Se uma propriedade somente leitura tem um campo particular de backup, você pode marcar o campo privado com o **DataMember** atributo.</span><span class="sxs-lookup"><span data-stu-id="beb7a-192">If a read-only property has a backing private field, you can mark the private field with the **DataMember** attribute.</span></span> <span data-ttu-id="beb7a-193">Essa abordagem requer o **DataContract** atributo da classe.</span><span class="sxs-lookup"><span data-stu-id="beb7a-193">This approach requires the **DataContract** attribute on the class.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample13.cs)]

<a id="xml_dates"></a>
### <a name="dates"></a><span data-ttu-id="beb7a-194">datas</span><span class="sxs-lookup"><span data-stu-id="beb7a-194">Dates</span></span>

<span data-ttu-id="beb7a-195">As datas são gravadas no formato ISO 8601.</span><span class="sxs-lookup"><span data-stu-id="beb7a-195">Dates are written in ISO 8601 format.</span></span> <span data-ttu-id="beb7a-196">Por exemplo, &quot;2012-05-23T20:21:37.9116538Z&quot;.</span><span class="sxs-lookup"><span data-stu-id="beb7a-196">For example, &quot;2012-05-23T20:21:37.9116538Z&quot;.</span></span>

<a id="xml_indenting"></a>
### <a name="indenting"></a><span data-ttu-id="beb7a-197">Recuar</span><span class="sxs-lookup"><span data-stu-id="beb7a-197">Indenting</span></span>

<span data-ttu-id="beb7a-198">Para gravar XML recuado, defina o **recuo** propriedade **true**:</span><span class="sxs-lookup"><span data-stu-id="beb7a-198">To write indented XML, set the **Indent** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample14.cs)]

<a id="xml_pertype"></a>
## <a name="setting-per-type-xml-serializers"></a><span data-ttu-id="beb7a-199">Configuração por tipo XML serializadores</span><span class="sxs-lookup"><span data-stu-id="beb7a-199">Setting Per-Type XML Serializers</span></span>

<span data-ttu-id="beb7a-200">Você pode definir os serializadores XML diferentes para diferentes tipos CLR.</span><span class="sxs-lookup"><span data-stu-id="beb7a-200">You can set different XML serializers for different CLR types.</span></span> <span data-ttu-id="beb7a-201">Por exemplo, você pode ter um objeto de dados específico que requer **XmlSerializer** para compatibilidade com versões anteriores.</span><span class="sxs-lookup"><span data-stu-id="beb7a-201">For example, you might have a particular data object that requires **XmlSerializer** for backward compatibility.</span></span> <span data-ttu-id="beb7a-202">Você pode usar **XmlSerializer** para este objeto e continuar a usar **DataContractSerializer** para outros tipos.</span><span class="sxs-lookup"><span data-stu-id="beb7a-202">You can use **XmlSerializer** for this object and continue to use **DataContractSerializer** for other types.</span></span>

<span data-ttu-id="beb7a-203">Para definir um serializador XML para um tipo específico, chame **SetSerializer**.</span><span class="sxs-lookup"><span data-stu-id="beb7a-203">To set an XML serializer for a particular type, call **SetSerializer**.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample15.cs)]

<span data-ttu-id="beb7a-204">Você pode especificar um **XmlSerializer** ou qualquer objeto derivado de **XmlObjectSerializer**.</span><span class="sxs-lookup"><span data-stu-id="beb7a-204">You can specify an **XmlSerializer** or any object that derives from **XmlObjectSerializer**.</span></span>

<a id="removing_the_json_or_xml_formatter"></a>
## <a name="removing-the-json-or-xml-formatter"></a><span data-ttu-id="beb7a-205">Removendo o formatador XML ou JSON</span><span class="sxs-lookup"><span data-stu-id="beb7a-205">Removing the JSON or XML Formatter</span></span>

<span data-ttu-id="beb7a-206">Você pode remover o formatador JSON ou o formatador XML da lista de formatadores, se você não quiser usá-los.</span><span class="sxs-lookup"><span data-stu-id="beb7a-206">You can remove the JSON formatter or the XML formatter from the list of formatters, if you do not want to use them.</span></span> <span data-ttu-id="beb7a-207">As principais razões para isso são:</span><span class="sxs-lookup"><span data-stu-id="beb7a-207">The main reasons to do this are:</span></span>

- <span data-ttu-id="beb7a-208">Para restringir suas respostas de API da web para um determinado tipo de mídia.</span><span class="sxs-lookup"><span data-stu-id="beb7a-208">To restrict your web API responses to a particular media type.</span></span> <span data-ttu-id="beb7a-209">Por exemplo, você pode decidir dar suporte a apenas respostas em JSON e remover o formatador XML.</span><span class="sxs-lookup"><span data-stu-id="beb7a-209">For example, you might decide to support only JSON responses, and remove the XML formatter.</span></span>
- <span data-ttu-id="beb7a-210">Para substituir o formatador padrão com um formatador personalizado.</span><span class="sxs-lookup"><span data-stu-id="beb7a-210">To replace the default formatter with a custom formatter.</span></span> <span data-ttu-id="beb7a-211">Por exemplo, você pode substituir o formatador JSON com sua própria implementação personalizada de um formatador JSON.</span><span class="sxs-lookup"><span data-stu-id="beb7a-211">For example, you could replace the JSON formatter with your own custom implementation of a JSON formatter.</span></span>

<span data-ttu-id="beb7a-212">O código a seguir mostra como remover os formatadores padrão.</span><span class="sxs-lookup"><span data-stu-id="beb7a-212">The following code shows how to remove the default formatters.</span></span> <span data-ttu-id="beb7a-213">Chamá-lo do seu **aplicativo\_iniciar** método definido em global. asax.</span><span class="sxs-lookup"><span data-stu-id="beb7a-213">Call this from your **Application\_Start** method, defined in Global.asax.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample16.cs)]

<a id="handling_circular_object_references"></a>
## <a name="handling-circular-object-references"></a><span data-ttu-id="beb7a-214">Tratamento de referências de objeto Circular</span><span class="sxs-lookup"><span data-stu-id="beb7a-214">Handling Circular Object References</span></span>

<span data-ttu-id="beb7a-215">Por padrão, os formatadores JSON e XML gravam todos os objetos como valores.</span><span class="sxs-lookup"><span data-stu-id="beb7a-215">By default, the JSON and XML formatters write all objects as values.</span></span> <span data-ttu-id="beb7a-216">Se duas propriedades se referem ao mesmo objeto, ou se o mesmo objeto aparece duas vezes em uma coleção, o formatador serializa o objeto duas vezes.</span><span class="sxs-lookup"><span data-stu-id="beb7a-216">If two properties refer to the same object, or if the same object appears twice in a collection, the formatter will serialize the object twice.</span></span> <span data-ttu-id="beb7a-217">Isso é um problema específico se seu gráfico de objeto contém ciclos, porque o serializador lançará uma exceção quando ela detecta um loop no gráfico.</span><span class="sxs-lookup"><span data-stu-id="beb7a-217">This is a particular problem if your object graph contains cycles, because the serializer will throw an exception when it detects a loop in the graph.</span></span>

<span data-ttu-id="beb7a-218">Considere os seguintes modelos de objeto e o controlador.</span><span class="sxs-lookup"><span data-stu-id="beb7a-218">Consider the following object models and controller.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample17.cs)]

<span data-ttu-id="beb7a-219">Invocar essa ação fará com que o formatador a ser gerada uma exceção, que converte um status código 500 (erro interno do servidor) na resposta ao cliente.</span><span class="sxs-lookup"><span data-stu-id="beb7a-219">Invoking this action will cause the formatter to thrown an exception, which translates to a status code 500 (Internal Server Error) response to the client.</span></span>

<span data-ttu-id="beb7a-220">Para preservar as referências de objeto em JSON, adicione o seguinte código para **aplicativo\_iniciar** método no arquivo global. asax:</span><span class="sxs-lookup"><span data-stu-id="beb7a-220">To preserve object references in JSON, add the following code to **Application\_Start** method in the Global.asax file:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample18.cs)]

<span data-ttu-id="beb7a-221">Agora, a ação de controlador retornará JSON tem esta aparência:</span><span class="sxs-lookup"><span data-stu-id="beb7a-221">Now the controller action will return JSON that looks like this:</span></span>

[!code-json[Main](json-and-xml-serialization/samples/sample19.json)]

<span data-ttu-id="beb7a-222">Observe que o serializador adiciona um &quot;$id&quot; propriedade para os dois objetos.</span><span class="sxs-lookup"><span data-stu-id="beb7a-222">Notice that the serializer adds an &quot;$id&quot; property to both objects.</span></span> <span data-ttu-id="beb7a-223">Além disso, ele detecta que a propriedade Employee.Department cria um loop, portanto, ele substitui o valor com uma referência de objeto: {&quot;$ref&quot;:&quot;1&quot;}.</span><span class="sxs-lookup"><span data-stu-id="beb7a-223">Also, it detects that the Employee.Department property creates a loop, so it replaces the value with an object reference: {&quot;$ref&quot;:&quot;1&quot;}.</span></span>

> [!NOTE]
> <span data-ttu-id="beb7a-224">Referências de objeto não são padronizadas em JSON.</span><span class="sxs-lookup"><span data-stu-id="beb7a-224">Object references are not standard in JSON.</span></span> <span data-ttu-id="beb7a-225">Antes de usar esse recurso, considere se os seus clientes serão capazes de analisar os resultados.</span><span class="sxs-lookup"><span data-stu-id="beb7a-225">Before using this feature, consider whether your clients will be able to parse the results.</span></span> <span data-ttu-id="beb7a-226">Talvez seja melhor simplesmente remover ciclos do gráfico.</span><span class="sxs-lookup"><span data-stu-id="beb7a-226">It might be better simply to remove cycles from the graph.</span></span> <span data-ttu-id="beb7a-227">Por exemplo, o link do funcionário para departamento não é realmente necessária neste exemplo.</span><span class="sxs-lookup"><span data-stu-id="beb7a-227">For example, the link from Employee back to Department is not really needed in this example.</span></span>


<span data-ttu-id="beb7a-228">Para preservar as referências de objeto em XML, você tem duas opções.</span><span class="sxs-lookup"><span data-stu-id="beb7a-228">To preserve object references in XML, you have two options.</span></span> <span data-ttu-id="beb7a-229">A opção mais simples é adicionar `[DataContract(IsReference=true)]` à sua classe de modelo.</span><span class="sxs-lookup"><span data-stu-id="beb7a-229">The simpler option is to add `[DataContract(IsReference=true)]` to your model class.</span></span> <span data-ttu-id="beb7a-230">O *IsReference* parâmetro permite referências de objeto.</span><span class="sxs-lookup"><span data-stu-id="beb7a-230">The *IsReference* parameter enables object references.</span></span> <span data-ttu-id="beb7a-231">Lembre-se de que **DataContract** torna serialização participar, portanto, também será necessário adicionar **DataMember** atributos para as propriedades:</span><span class="sxs-lookup"><span data-stu-id="beb7a-231">Remember that **DataContract** makes serialization opt-in, so you will also need to add **DataMember** attributes to the properties:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample20.cs)]

<span data-ttu-id="beb7a-232">Agora o formatador produzirá XML semelhante ao seguinte:</span><span class="sxs-lookup"><span data-stu-id="beb7a-232">Now the formatter will produce XML similar to following:</span></span>

[!code-xml[Main](json-and-xml-serialization/samples/sample21.xml)]

<span data-ttu-id="beb7a-233">Se você quiser evitar atributos em sua classe de modelo, há outra opção: criar um novo tipo específico **DataContractSerializer** de instância e defina *preserveObjectReferences* para **true**  no construtor.</span><span class="sxs-lookup"><span data-stu-id="beb7a-233">If you want to avoid attributes on your model class, there is another option: Create a new type-specific **DataContractSerializer** instance and set *preserveObjectReferences* to **true** in the constructor.</span></span> <span data-ttu-id="beb7a-234">Em seguida, defina essa instância como um serializador por tipo no formatador do tipo de mídia XML.</span><span class="sxs-lookup"><span data-stu-id="beb7a-234">Then set this instance as a per-type serializer on the XML media-type formatter.</span></span> <span data-ttu-id="beb7a-235">O código a seguir mostram como fazer isso:</span><span class="sxs-lookup"><span data-stu-id="beb7a-235">The following code show how to do this:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample22.cs?highlight=3)]

<a id="testing_object_serialization"></a>
## <a name="testing-object-serialization"></a><span data-ttu-id="beb7a-236">Teste de serialização do objeto</span><span class="sxs-lookup"><span data-stu-id="beb7a-236">Testing Object Serialization</span></span>

<span data-ttu-id="beb7a-237">Ao projetar sua API da web, é útil testar como os objetos de dados serão serializados.</span><span class="sxs-lookup"><span data-stu-id="beb7a-237">As you design your web API, it is useful to test how your data objects will be serialized.</span></span> <span data-ttu-id="beb7a-238">Você pode fazer isso sem criar um controlador ou invocar uma ação do controlador.</span><span class="sxs-lookup"><span data-stu-id="beb7a-238">You can do this without creating a controller or invoking a controller action.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample23.cs)]

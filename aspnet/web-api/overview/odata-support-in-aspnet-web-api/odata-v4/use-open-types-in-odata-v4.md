---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/use-open-types-in-odata-v4
title: Abrir tipos no OData v4 com o ASP.NET Web API | Microsoft Docs
author: microsoft
description: "Em OData v4, um tipo aberto é um tipo de stuctured que contém as propriedades dinâmicas, além de quaisquer propriedades que são declaradas na definição do tipo. Abrir..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/15/2014
ms.topic: article
ms.assetid: f25f5ac5-4800-4950-abe5-c97750a27fc6
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/use-open-types-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: c2d7454534ff0e9e0a80365793800ab7c45d3b6e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="open-types-in-odata-v4-with-aspnet-web-api"></a>Abrir tipos no v4 do OData com a API da Web do ASP.NET
====================
por [Microsoft](https://github.com/microsoft)

> Em OData v4, um *abrir tipo* é um tipo de stuctured que contém as propriedades dinâmicas, além de quaisquer propriedades que são declaradas na definição do tipo. Tipos abertos permitem adicionar flexibilidade a seus modelos de dados. Este tutorial mostra como usar tipos abertos no OData da API da Web ASP.NET.
> 
> Este tutorial presume que você já sabe como criar um ponto de extremidade OData na API da Web do ASP.NET. Se não, comece lendo [criar um ponto de extremidade OData v4](create-an-odata-v4-endpoint.md) primeiro.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - OData da API Web 5.3
> - OData v4


Primeiro, terminologia OData:

- Tipo de entidade: um tipo estruturado com uma chave.
- Tipo complexo: um tipo estruturado sem uma chave.
- Abra o tipo: um tipo com propriedades dinâmicas. Tipos de entidade e tipos complexos podem ser abertos.

O valor de uma propriedade dinâmica pode ser um tipo primitivo, um tipo complexo ou um tipo de enumeração; ou uma coleção de qualquer um desses tipos. Para obter mais informações sobre tipos abertos, consulte o [especificação do OData v4](http://www.odata.org/documentation/odata-version-4-0/).

## <a name="install-the-web-odata-libraries"></a>Instalar as bibliotecas de OData na Web

Use o NuGet Package Manager para instalar as bibliotecas de OData da API da Web mais recentes. Da janela do Console do Gerenciador de pacotes:

[!code-console[Main](use-open-types-in-odata-v4/samples/sample1.cmd)]

## <a name="define-the-clr-types"></a>Definir os tipos CLR

Comece definindo os modelos EDM como tipos CLR.

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample2.cs)]

Quando o modelo de dados de entidade (EDM) é criado,

- `Category`é um tipo de enumeração.
- `Address`é um tipo complexo. (Ele não tem uma chave, portanto, não é um tipo de entidade.)
- `Customer`é um tipo de entidade. (Ele tem uma chave).
- `Press`é um tipo complexo aberto.
- `Book`é um tipo de entidade aberto.

Para criar um tipo aberto, o tipo CLR deve ter uma propriedade de tipo `IDictionary<string, object>`, que contém as propriedades dinâmicas.

## <a name="build-the-edm-model"></a>Criar o modelo EDM

Se você usar **ODataConventionModelBuilder** para criar o EDM `Press` e `Book` são automaticamente adicionados como tipos abertos, com base na presença de um `IDictionary<string, object>` propriedade.

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample3.cs)]

Você também pode criar EDM explicitamente, usando **ODataModelBuilder**.

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample4.cs)]

## <a name="add-an-odata-controller"></a>Adicionar um controlador OData

Em seguida, adicione um controlador OData. Para este tutorial, vamos usar um controlador simplificado que suporta apenas obter POST solicitações e usa uma lista de memória para armazenar entidades.

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample5.cs)]

Observe que a primeira `Book` instância não tem nenhuma propriedade dinâmica. O segundo `Book` instância tem as seguintes propriedades dinâmicas:

- "Publicado": tipo primitivo
- "Autores": a coleção de tipos primitivos
- "OtherCategories": a coleção de tipos de enumeração.

Além disso, o `Press` propriedade que `Book` instância tem as seguintes propriedades dinâmicas:

- "Blog": tipo primitivo
- "Address": tipo complexo

## <a name="query-the-metadata"></a>Os metadados de consulta

Para obter o documento de metadados do OData, envie uma solicitação GET para `~/$metadata`. O corpo da resposta deve ser semelhante a este:

[!code-xml[Main](use-open-types-in-odata-v4/samples/sample6.xml?highlight=5,21)]

O documento de metadados, você pode ver que:

- Para o `Book` e `Press` tipos, o valor de `OpenType` atributo é true. O `Customer` e `Address` tipos não têm esse atributo.
- O `Book` tipo de entidade tem três propriedades declaradas: ISBN, título e pressione. Os metadados do OData não incluir o `Book.Properties` propriedade da classe CLR.
- Da mesma forma, o `Press` tem apenas duas propriedades declaradas: nome e categoria. Os metadados não tem o `Press.DynamicProperties` propriedade da classe CLR.

## <a name="query-an-entity"></a>Consultar uma entidade

Para obter o catálogo com ISBN igual a "978-0-7356-7942-9", o envio envie uma solicitação GET para `~/Books('978-0-7356-7942-9')`. O corpo da resposta deve ser semelhante ao seguinte. (Recuado para torná-lo mais legível).

[!code-console[Main](use-open-types-in-odata-v4/samples/sample7.cmd?highlight=8-13,15-23)]

Observe que as propriedades dinâmicas são incluídos em linha com as propriedades declaradas.

## <a name="post-an-entity"></a>POSTE uma entidade

Para adicionar uma entidade de catálogo, enviar uma solicitação POST para `~/Books`. O cliente pode definir propriedades dinâmicas na carga de solicitação.

Aqui está um exemplo de solicitação. Observe as propriedades "Price" e "Publicado".

[!code-console[Main](use-open-types-in-odata-v4/samples/sample8.cmd?highlight=10)]

Se você definir um ponto de interrupção no método do controlador, você pode ver que a API da Web adicionado essas propriedades para o `Properties` dicionário.

![](use-open-types-in-odata-v4/_static/image1.png)

## <a name="additional-resources"></a>Recursos adicionais

[Exemplo de tipo aberto de OData](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataOpenTypeSample/ReadMe.txt)

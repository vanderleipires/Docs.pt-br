---
uid: web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value
title: Usando $select, $expand e $value no ASP.NET Web API 2 OData | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 10/11/2013
ms.assetid: 43279a80-a96c-4564-b6ea-ad992a2d6828
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value
msc.type: authoredcontent
ms.openlocfilehash: d198ecf40155cba36204bc0810f4735aae6b100b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834504"
---
<a name="using-select-expand-and-value-in-aspnet-web-api-2-odata"></a>Usando $select, $expand e $value no ASP.NET Web API 2 OData
====================
por [Mike Wasson](https://github.com/MikeWasson)

API Web 2 adiciona suporte para o $expand, $select e opções de $value no OData. Essas opções permitem que um cliente controlar a representação de que ele obtém de volta do servidor.

- **$expand** faz com que as entidades relacionadas ao ser incluídas embutidas na resposta.
- **$select** seleciona um subconjunto de propriedades para incluir na resposta.
- **$value** obtém o valor bruto de uma propriedade.

## <a name="example-schema"></a>Esquema de exemplo

Neste artigo, vou usar um serviço OData que define três entidades: produto, fornecedor e categoria. Cada produto tem uma categoria e um fornecedor.

![](using-select-expand-and-value/_static/image1.png)

Aqui estão as classes do c# que definem os modelos de entidade:

[!code-csharp[Main](using-select-expand-and-value/samples/sample1.cs)]

Observe que o `Product` classe define propriedades de navegação para o `Supplier` e `Category`. O `Category` classe define uma propriedade de navegação para os produtos em cada categoria.

Para criar um ponto de extremidade OData para este esquema, use o scaffolding do Visual Studio 2013, conforme descrito em [criando um ponto de extremidade OData na API Web ASP.NET](odata-v3/creating-an-odata-endpoint.md). Adicione controladores separados para fornecedor, categoria e produto.

## <a name="enabling-expand-and-select"></a>Habilitando $expandir e $select

No Visual Studio 2013, o scaffolding de Web API OData cria um controlador que automaticamente dá suporte a $expand e $select. Para referência, aqui estão os requisitos para dar suporte a $expandem e $select em um controlador.

Para coleções, o controlador `Get` método deve retornar um **IQueryable**.

[!code-csharp[Main](using-select-expand-and-value/samples/sample2.cs)]

Para entidades únicas, retornar um **SingleResult&lt;T&gt;**, onde T é um **IQueryable** que contém zero ou uma entidade.

[!code-csharp[Main](using-select-expand-and-value/samples/sample3.cs)]

Além disso, decorar seu `Get` métodos com o **[Queryable]** de atributo, conforme mostrado nos trechos de código anterior. Como alternativa, chame **EnableQuerySupport** sobre o **HttpConfiguration** objeto na inicialização. (Para obter mais informações, consulte [habilitando opções de consulta OData](supporting-odata-query-options.md#enable).)

## <a name="using-expand"></a>Usando $expand

Quando você consulta a uma entidade OData ou coleção, a resposta padrão não inclui entidades relacionadas. Por exemplo, aqui está a resposta padrão para o conjunto de entidades de categorias:

[!code-console[Main](using-select-expand-and-value/samples/sample4.cmd)]

Como você pode ver, a resposta não inclui quaisquer produtos, mesmo que a entidade da categoria tem um link de navegação de produtos. No entanto, o cliente pode usar $expandir para obter a lista de produtos para cada categoria. A opção $expand vai para a cadeia de caracteres de consulta da solicitação:

[!code-console[Main](using-select-expand-and-value/samples/sample5.cmd)]

Agora o servidor incluirá os produtos para cada categoria, embutido com as categorias. Aqui está a carga de resposta:

[!code-console[Main](using-select-expand-and-value/samples/sample6.cmd)]

Observe que cada entrada na matriz "value" contém uma lista de produtos.

O $expand opção leva uma separada por vírgulas lista de propriedades de navegação a expandir. A solicitação a seguir expande a categoria e o fornecedor para um produto.

[!code-console[Main](using-select-expand-and-value/samples/sample7.cmd)]

Aqui está o corpo da resposta:

[!code-console[Main](using-select-expand-and-value/samples/sample8.cmd)]

Você pode expandir mais de um nível de propriedade de navegação. O exemplo a seguir inclui todos os produtos para uma categoria e também o fornecedor para cada produto.

[!code-console[Main](using-select-expand-and-value/samples/sample9.cmd)]

Aqui está o corpo da resposta:

[!code-console[Main](using-select-expand-and-value/samples/sample10.cmd)]

Por padrão, a API da Web limita a profundidade máxima de expansão para 2. Que evita que o cliente envie solicitações complexas, como `$expand=Orders/OrderDetails/Product/Supplier/Region`, que pode ser ineficiente para consultar e criar respostas grandes. Para substituir o padrão, defina as **MaxExpansionDepth** propriedade sobre o **[Queryable]** atributo.

[!code-csharp[Main](using-select-expand-and-value/samples/sample11.cs)]

Para obter mais informações sobre a $expand opção, consulte [opção de consulta de sistema expanda ($expand)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#46_Expand_System_Query_Option_expand) na documentação oficial do OData.

## <a name="using-select"></a>Usando $select

A opção de $select Especifica um subconjunto de propriedades a serem incluídas no corpo da resposta. Por exemplo, para que somente o nome e o preço de cada produto, use a seguinte consulta:

[!code-console[Main](using-select-expand-and-value/samples/sample12.cmd)]

Aqui está o corpo da resposta:

[!code-console[Main](using-select-expand-and-value/samples/sample13.cmd)]

Você pode combinar $select e $expand na mesma consulta. Certifique-se de incluir a propriedade expandida na opção $select. Por exemplo, a solicitação a seguir obtém o nome do produto e o fornecedor.

[!code-console[Main](using-select-expand-and-value/samples/sample14.cmd)]

Aqui está o corpo da resposta:

[!code-console[Main](using-select-expand-and-value/samples/sample15.cmd)]

Você também pode selecionar as propriedades dentro de uma propriedade expandida. A solicitação a seguir expande os produtos e seleciona o nome de categoria e produto.

[!code-console[Main](using-select-expand-and-value/samples/sample16.cmd)]

Aqui está o corpo da resposta:

[!code-console[Main](using-select-expand-and-value/samples/sample17.cmd)]

Para obter mais informações sobre a opção $select, consulte [Selecionar opção de consulta do sistema ($select)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#48_Select_System_Query_Option_select) na documentação oficial do OData.

## <a name="getting-individual-properties-of-an-entity-value"></a>Obtendo propriedades individuais de uma entidade ($value)

Há duas maneiras para um cliente OData obter uma propriedade individual de uma entidade. O cliente pode obter o valor no formato OData ou obter o valor bruto da propriedade.

A solicitação a seguir obtém uma propriedade no formato OData.

[!code-console[Main](using-select-expand-and-value/samples/sample18.cmd)]

Aqui está um exemplo de resposta no formato JSON:

[!code-console[Main](using-select-expand-and-value/samples/sample19.cmd)]

Para obter o valor bruto da propriedade, acrescente $value para o URI:

[!code-console[Main](using-select-expand-and-value/samples/sample20.cmd)]

Aqui está a resposta. Observe que o tipo de conteúdo "text/plain", não é JSON.

[!code-console[Main](using-select-expand-and-value/samples/sample21.cmd)]

Para dar suporte a essas consultas no seu controlador de OData, adicione um método chamado `GetProperty`, onde `Property` é o nome da propriedade. Por exemplo, o método para obter a propriedade de nome seria nomeado `GetName`. O método deve retornar o valor dessa propriedade:

[!code-csharp[Main](using-select-expand-and-value/samples/sample22.cs)]

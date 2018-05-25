---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-routing-conventions
title: Convenções de roteamento de ASP.NET Web API 2 Odata | Microsoft Docs
author: MikeWasson
description: Este artigo descreve as convenções de roteamento API Web usa para pontos de extremidade OData.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/31/2013
ms.topic: article
ms.assetid: adbc175a-14eb-4ab2-a441-d056ffa8266f
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-routing-conventions
msc.type: authoredcontent
ms.openlocfilehash: 0ab99dd443040b90ffefd2f5b9261a63b91e9463
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="routing-conventions-in-aspnet-web-api-2-odata"></a>Convenções de roteamento de ASP.NET Web API 2 Odata
====================
por [Mike Wasson](https://github.com/MikeWasson)

> Este artigo descreve as convenções de roteamento API Web usa para pontos de extremidade OData.


Quando a API da Web recebe uma solicitação de OData, ele mapeia a solicitação para um nome de controlador e um nome de ação. O mapeamento baseia-se o método HTTP e o URI. Por exemplo, `GET /odata/Products(1)` mapeia para `ProductsController.GetProduct`.

Na parte 1 deste artigo, descrevo as convenções de roteamento OData internas. Estas convenções são projetadas especificamente para pontos de extremidade de OData, e elas substituem o sistema de roteamento de API da Web padrão. (A substituição acontece quando você chamar **MapODataRoute**.)

Na parte 2, mostrar como adicionar as convenções de roteamento personalizadas. No momento as convenções internas não abrangem o intervalo inteiro de OData URIs, mas você pode estendê-las para lidar com casos adicionais.

- [Convenções de roteamento internas](#conventions)
- [Convenções de roteamento personalizadas](#custom)

<a id="conventions"></a>
## <a name="built-in-routing-conventions"></a>Convenções de roteamento internas

Antes que descrevem as convenções de roteamento OData na API da Web, é útil entender OData URIs. Um [URI do OData](http://www.odata.org/documentation/odata-v3-documentation/url-conventions/) consiste em:

- A raiz do serviço
- O caminho do recurso
- Opções de consulta

![](odata-routing-conventions/_static/image1.png)

Roteamento, a parte importante é o caminho do recurso. O caminho do recurso é dividido em segmentos. Por exemplo, `/Products(1)/Supplier` tem três segmentos:

- `Products`refere-se a um conjunto de entidade nomeado "Produtos".
- `1`é uma chave de entidade, selecionando uma única entidade do conjunto.
- `Supplier`é uma propriedade de navegação que seleciona uma entidade relacionada.

Portanto esse caminho escolhe out o fornecedor do produto 1.

> [!NOTE]
> Segmentos de caminho OData nem sempre correspondem aos segmentos URI. Por exemplo, "1" é considerado um segmento de caminho.


**Nomes de controlador.** O nome do controlador sempre é derivado da entidade definida na raiz do caminho do recurso. Por exemplo, se o caminho do recurso é `/Products(1)/Supplier`, API da Web procura um controlador chamado `ProductsController`.

**Nomes de ação.** Nomes de ação são derivados dos segmentos de caminho mais o modelo de dados de entidade (EDM), conforme listado nas tabelas a seguir. Em alguns casos, você tem duas opções para o nome da ação. Por exemplo, "Get" ou &quot;GetProducts&quot;.

**Consultando entidades**

| Solicitação | URI de exemplo | Nome da ação | Ação de exemplo |
| --- | --- | --- | --- |
| OBTER /entityset | E produtos | GetEntitySet ou Get | GetProducts |
| OBTER /entityset(key) | /Products(1) | GetEntityType ou Get | GetProduct |
| OBTER /entityset (chave) / converter | /Products(1)/Models.Book | GetEntityType ou Get | GetBook |

Para obter mais informações, consulte [criar um ponto de extremidade de OData de somente leitura](odata-v3/creating-an-odata-endpoint.md).

**Criando, atualizando e excluindo entidades**

| Solicitação | URI de exemplo | Nome da ação | Ação de exemplo |
| --- | --- | --- | --- |
| Lançar /entityset | E produtos | PostEntityType ou Post | PostProduct |
| COLOCAR /entityset(key) | /Products(1) | PutEntityType ou Put | PutProduct |
| COLOCAR /entityset (chave) / converter | /Products(1)/Models.Book | PutEntityType ou Put | PutBook |
| PATCH /entityset(key) | /Products(1) | PatchEntityType ou Patch | PatchProduct |
| /Entityset (key) do PATCH / converter | /Products(1)/Models.Book | PatchEntityType ou Patch | PatchBook |
| Excluir /entityset(key) | /Products(1) | DeleteEntityType ou Delete | DeleteProduct |
| DELETE /entityset(key)/cast | /Products(1)/Models.Book | DeleteEntityType ou Delete | DeleteBook |

**Consultar uma propriedade de navegação**

| Solicitação | URI de exemplo | Nome da ação | Ação de exemplo |
| --- | --- | --- | --- |
| GET /entityset (chave) / navegação | /Products(1)/Supplier | GetNavigationFromEntityType ou GetNavigation | GetSupplierFromProduct |
| OBTER navegação cast//entityset (chave) | /Products(1)/Models.Book/Author | GetNavigationFromEntityType ou GetNavigation | GetAuthorFromBook |

Para obter mais informações, consulte [trabalhando com relações de entidade](odata-v3/working-with-entity-relations.md).

**Criação e exclusão de Links**

| Solicitação | URI de exemplo | Nome da ação |
| --- | --- | --- |
| POST /entityset (chave) / $links/navegação | /Products(1)/$links/Supplier | CreateLink |
| PUT /entityset (chave) / $links/navegação | /Products(1)/$links/Supplier | CreateLink |
| DELETE /entityset(key)/$links/navigation | /Products(1)/$links/Supplier | DeleteLink |
| DELETE /entityset(key)/$links/navigation(relatedKey) | /Products/(1)/$links/Suppliers(1) | DeleteLink |

Para obter mais informações, consulte [trabalhando com relações de entidade](odata-v3/working-with-entity-relations.md).

**Propriedades**

*Requer Web API 2*

| Solicitação | URI de exemplo | Nome da ação | Ação de exemplo |
| --- | --- | --- | --- |
| GET /entityset (chave) / propriedade | /Products(1)/Name | GetPropertyFromEntityType ou GetProperty | GetNameFromProduct |
| OBTER a propriedade cast//entityset (chave) | /Products(1)/Models.Book/Author | GetPropertyFromEntityType ou GetProperty | GetTitleFromBook |

**Ações**

| Solicitação | URI de exemplo | Nome da ação | Ação de exemplo |
| --- | --- | --- | --- |
| POST /entityset (chave) / ação | / Produtos (1) / taxa | ActionNameOnEntityType ou o nome da ação | RateOnProduct |
| Lançar ação cast//entityset (chave) | /Products(1)/Models.Book/CheckOut | ActionNameOnEntityType ou o nome da ação | CheckOutOnBook |

Para obter mais informações, consulte [ações de OData](odata-v3/odata-actions.md).

**Assinaturas de método**

Aqui estão algumas regras para as assinaturas de método:

- Se o caminho contiver uma chave, a ação deve ter um parâmetro denominado *chave*.
- Se o caminho contiver uma chave em uma propriedade de navegação, a ação deve ter um parâmetro denominado *relatedKey*.
- Decore *chave* e *relatedKey* parâmetros com o **[FromODataUri]** parâmetro.
- POST e PUT solicitações recebem um parâmetro do tipo de entidade.
- Solicitações de PATCH usam um parâmetro de tipo **Delta&lt;T&gt;**, onde *T* é o tipo de entidade.

Para referência, aqui está um exemplo que mostra as assinaturas de método para cada convenção de roteamento OData interna.

[!code-csharp[Main](odata-routing-conventions/samples/sample1.cs)]

<a id="custom"></a>
## <a name="custom-routing-conventions"></a>Convenções de roteamento personalizadas

No momento as convenções internas não abrangem todos os OData URIs possíveis. Você pode adicionar novas convenções Implementando o **IODataRoutingConvention** interface. Essa interface possui dois métodos:

[!code-csharp[Main](odata-routing-conventions/samples/sample2.cs)]

- **SelectController** retorna o nome do controlador.
- **SelectAction** retorna o nome da ação.

Para ambos os métodos, se a convenção não se aplicam a essa solicitação, o método deve retornar nulo.

O **ODataPath** parâmetro representa o caminho do recurso de OData analisado. Ele contém uma lista de **[ODataPathSegment](https://msdn.microsoft.com/library/system.web.http.odata.routing.odatapathsegment.aspx)** instâncias, uma para cada segmento do caminho do recurso. **ODataPathSegment** é uma classe abstrata; cada tipo de segmento é representado por uma classe que deriva de **ODataPathSegment**.

O **ODataPath.TemplatePath** propriedade é uma cadeia de caracteres que representa a concatenação de todos os segmentos de caminho. Por exemplo, se o URI é `/Products(1)/Supplier`, o modelo de caminho é &quot;~/entityset/key/navigation&quot;. Observe que os segmentos não correspondem diretamente aos segmentos de URI. Por exemplo, a chave de entidade (1) é representada como seu próprio **ODataPathSegment**.

Normalmente, uma implementação de **IODataRoutingConvention** faz o seguinte:

1. Compare o modelo de caminho para ver se esta convenção aplica-se a solicitação atual. Se não for aplicável, retorne nulo.
2. Se a convenção se aplica, use propriedades do **ODataPathSegment** instâncias para gerar nomes de controlador e ação.
3. Para ações, adicione todos os valores no dicionário de rota devem ser associados aos parâmetros de ação (geralmente as chaves da entidade).

Vejamos um exemplo específico. As convenções de roteamento internas não dão suporte a indexação em uma coleção de navegação. Em outras palavras, não há nenhum convenção para URIs semelhante ao seguinte:

[!code-javascript[Main](odata-routing-conventions/samples/sample3.js)]

Aqui está uma convenção de roteamento personalizada para lidar com esse tipo de consulta.

[!code-csharp[Main](odata-routing-conventions/samples/sample4.cs)]

Notas:

1. Derivam de **EntitySetRoutingConvention**, pois o **SelectController** método nessa classe é apropriado para essa nova convenção de roteamento. Isso significa que não preciso reimplementar **SelectController**.
2. A convenção se aplica apenas às solicitações GET, e somente quando o modelo de caminho é &quot;~/entityset/key/navigation/key&quot;.
3. O nome da ação é &quot;obter {EntityType}&quot;, onde *{EntityType}* é o tipo de coleção de navegação. Por exemplo, &quot;GetSupplier&quot;. Você pode usar qualquer convenção de nomenclatura que você deseja & #8212; Verifique se as ações do controlador corresponder.
4. A ação utiliza dois parâmetros denominados *chave* e *relatedKey*. (Para obter uma lista de alguns nomes de parâmetro predefinidos, consulte [ODataRouteConstants](https://msdn.microsoft.com/library/system.web.http.odata.routing.odatarouteconstants.aspx).)

A próxima etapa é adicionar a nova convenção à lista de convenções de roteamento. Isso ocorre durante a configuração, conforme mostrado no código a seguir:

[!code-csharp[Main](odata-routing-conventions/samples/sample5.cs)]

Aqui estão algumas outras exemplo convenções de roteamento que ser úteis para estudar a:

- [CompositeKeyRoutingConvention](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataCompositeKeySample/ODataCompositeKeySample/Extensions/CompositeKeyRoutingConvention.cs)
- [CustomNavigationRoutingConvention](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataServiceSample/ODataService/Extensions/CustomNavigationRoutingConvention.cs)
- [NonBindableActionRoutingConvention](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataActionsSample/ODataActionsSample/NonBindableActionRoutingConvention.cs)
- [ODataVersionRouteConstraint](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataVersioningSample/ODataVersioningSample/Extensions/ODataVersionRouteConstraint.cs)

E claro própria API da Web é o código-fonte aberto, para que você possa ver o [código-fonte](http://aspnetwebstack.codeplex.com/) para as convenções de roteamento internas. Eles são definidos no **System.Web.Http.OData.Routing.Conventions** namespace.

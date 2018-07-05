---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-routing-conventions
title: Convenções de roteamento na API Web ASP.NET 2 Odata | Microsoft Docs
author: MikeWasson
description: Este artigo descreve as convenções de roteamento API Web usa para pontos de extremidade OData.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/31/2013
ms.topic: article
ms.assetid: adbc175a-14eb-4ab2-a441-d056ffa8266f
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-routing-conventions
msc.type: authoredcontent
ms.openlocfilehash: 63a0e4f4f61580ea9da3bd491e7a45f20cd7aaae
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37370533"
---
<a name="routing-conventions-in-aspnet-web-api-2-odata"></a>Convenções de roteamento na API Web ASP.NET 2 Odata
====================
por [Mike Wasson](https://github.com/MikeWasson)

> Este artigo descreve as convenções de roteamento API Web usa para pontos de extremidade OData.


Quando a API Web recebe uma solicitação de OData, ele mapeia a solicitação para um nome de controlador e um nome de ação. O mapeamento é baseado no método HTTP e o URI. Por exemplo, `GET /odata/Products(1)` mapeia para `ProductsController.GetProduct`.

Na parte 1 deste artigo, descrevo as convenções de roteamento internas do OData. Essas convenções são projetadas especificamente para pontos de extremidade OData e substituir o sistema de roteamento de API da Web padrão. (A substituição acontece quando você chama **MapODataRoute**.)

Na parte 2, mostro como adicionar convenções de roteamentos personalizadas. Atualmente, as convenções internas não abrangem o intervalo inteiro de OData URIs, mas você pode estendê-las para lidar com casos adicionais.

- [Convenções de roteamento internas](#conventions)
- [Convenções de roteamentos personalizadas](#custom)

<a id="conventions"></a>
## <a name="built-in-routing-conventions"></a>Convenções de roteamento internas

Antes de eu descrevem as convenções de roteamento OData na API da Web, é útil entender os URIs de OData. Uma [URI do OData](http://www.odata.org/documentation/odata-v3-documentation/url-conventions/) consiste em:

- A raiz do serviço
- O caminho do recurso
- Opções de consulta

![](odata-routing-conventions/_static/image1.png)

Para o roteamento, a parte importante é o caminho do recurso. O caminho do recurso é dividido em segmentos. Por exemplo, `/Products(1)/Supplier` tem três segmentos:

- `Products` se refere a um conjunto de entidades chamado "Produtos".
- `1` é uma chave de entidade, selecionando uma única entidade do conjunto.
- `Supplier` é uma propriedade de navegação que seleciona uma entidade relacionada.

Portanto, esse caminho selecionará o fornecedor do produto 1.

> [!NOTE]
> Segmentos de caminho OData nem sempre correspondem aos segmentos URI. Por exemplo, "1" é considerado um segmento de caminho.


**Nomes do controlador.** O nome de controlador sempre é derivado da entidade definidas na raiz do caminho do recurso. Por exemplo, se o caminho do recurso é `/Products(1)/Supplier`, API da Web procura por um controlador chamado `ProductsController`.

**Nomes de ação.** Nomes de ação são derivados de segmentos de caminho mais o modelo de dados de entidade (EDM), conforme listado nas tabelas a seguir. Em alguns casos, você terá duas opções para o nome da ação. Por exemplo, "Get" ou &quot;GetProducts&quot;.

**Consultar entidades**

| Solicitação | Exemplo de URI | Nome da ação | Exemplo de ação |
| --- | --- | --- | --- |
| OBTER /entityset | / Produtos | GetEntitySet ou Get | GetProducts |
| OBTER /entityset(key) | /Products(1) | GetEntityType ou Get | GetProduct |
| OBTER /entityset (chave) / convertido | / /Models.Book produtos (1) | GetEntityType ou Get | GetBook |

Para obter mais informações, consulte [criar um ponto de extremidade de OData de somente leitura](odata-v3/creating-an-odata-endpoint.md).

**Criando, atualizando e excluindo entidades**

| Solicitação | Exemplo de URI | Nome da ação | Exemplo de ação |
| --- | --- | --- | --- |
| POSTAR /entityset | / Produtos | PostEntityType ou Post | PostProduct |
| COLOCAR /entityset(key) | /Products(1) | PutEntityType ou Put | PutProduct |
| COLOCAR /entityset (chave) / convertido | / /Models.Book produtos (1) | PutEntityType ou Put | PutBook |
| PATCH /entityset(key) | /Products(1) | PatchEntityType ou Patch | PatchProduct |
| PATCH /entityset (chave) / convertido | / /Models.Book produtos (1) | PatchEntityType ou Patch | PatchBook |
| Excluir /entityset(key) | /Products(1) | DeleteEntityType ou Delete | DeleteProduct |
| Excluir /entityset (chave) / convertido | / /Models.Book produtos (1) | DeleteEntityType ou Delete | DeleteBook |

**Consultar uma propriedade de navegação**

| Solicitação | Exemplo de URI | Nome da ação | Exemplo de ação |
| --- | --- | --- | --- |
| GET /entityset (chave) / navegação | / Produtos (1) / fornecedor | GetNavigationFromEntityType ou GetNavigation | GetSupplierFromProduct |
| OBTER a navegação/cast//entityset (chave) | / /Models.Book/Author produtos (1) | GetNavigationFromEntityType ou GetNavigation | GetAuthorFromBook |

Para obter mais informações, consulte [trabalhando com relações de entidade](odata-v3/working-with-entity-relations.md).

**Criação e exclusão de Links**

| Solicitação | Exemplo de URI | Nome da ação |
| --- | --- | --- |
| POST /entityset (chave) / $links/navegação | / Produtos (1) / $links/fornecedor | CreateLink |
| PUT /entityset (chave) / $links/navegação | / Produtos (1) / $links/fornecedor | CreateLink |
| Excluir /entityset (chave) / $links/navegação | / Produtos (1) / $links/fornecedor | DeleteLink |
| DELETE /entityset(key)/$links/navigation(relatedKey) | /Products/(1)/$links/Suppliers(1) | DeleteLink |

Para obter mais informações, consulte [trabalhando com relações de entidade](odata-v3/working-with-entity-relations.md).

**Propriedades**

*Requer a API Web 2*

| Solicitação | Exemplo de URI | Nome da ação | Exemplo de ação |
| --- | --- | --- | --- |
| GET /entityset (chave) / propriedade | / Produtos (1) / nome | GetPropertyFromEntityType ou GetProperty | GetNameFromProduct |
| OBTER propriedade/cast//entityset (chave) | / /Models.Book/Author produtos (1) | GetPropertyFromEntityType ou GetProperty | GetTitleFromBook |

**Ações**

| Solicitação | Exemplo de URI | Nome da ação | Exemplo de ação |
| --- | --- | --- | --- |
| POST /entityset (chave) / ação | / Produtos (1) / taxa | ActionNameOnEntityType ou o nome da ação | RateOnProduct |
| /Entityset (chave) / cast/ação posterior | /Products(1)/Models.Book/CheckOut | ActionNameOnEntityType ou o nome da ação | CheckOutOnBook |

Para obter mais informações, consulte [ações de OData](odata-v3/odata-actions.md).

**Assinaturas de método**

Aqui estão algumas regras para as assinaturas de método:

- Se o caminho contiver uma chave, a ação deve ter um parâmetro denominado *chave*.
- Se o caminho contiver uma chave em uma propriedade de navegação, a ação deve ter um parâmetro denominado *relatedKey*.
- Decore *chave* e *relatedKey* parâmetros com o **[FromODataUri]** parâmetro.
- POST e PUT solicitações usam um parâmetro do tipo de entidade.
- Solicitações de PATCH utilizam um parâmetro do tipo **Delta&lt;T&gt;**, onde *T* é o tipo de entidade.

Para referência, aqui está um exemplo que mostra as assinaturas de método para cada convenção de roteamento interna do OData.

[!code-csharp[Main](odata-routing-conventions/samples/sample1.cs)]

<a id="custom"></a>
## <a name="custom-routing-conventions"></a>Convenções de roteamentos personalizadas

As convenções internas não cobre atualmente todos os URIs do OData possíveis. Você pode adicionar novas convenções de Implementando a **IODataRoutingConvention** interface. Essa interface tem dois métodos:

[!code-csharp[Main](odata-routing-conventions/samples/sample2.cs)]

- **SelectController** retorna o nome do controlador.
- **SelectAction** retorna o nome da ação.

Para ambos os métodos, se a convenção não se aplica a essa solicitação, o método deve retornar nulo.

O **ODataPath** parâmetro representa o caminho de recurso de OData analisado. Ele contém uma lista de **[ODataPathSegment](https://msdn.microsoft.com/library/system.web.http.odata.routing.odatapathsegment.aspx)** instâncias, uma para cada segmento do caminho do recurso. **ODataPathSegment** é uma classe abstrata; cada tipo de segmento é representado por uma classe que deriva de **ODataPathSegment**.

O **ODataPath.TemplatePath** propriedade é uma cadeia de caracteres que representa a concatenação de todos os segmentos de caminho. Por exemplo, se o URI é `/Products(1)/Supplier`, é o modelo de caminho &quot;~/entityset/key/navigation&quot;. Observe que os segmentos não correspondem diretamente aos segmentos de URI. Por exemplo, a chave de entidade (1) é representada como seu próprio **ODataPathSegment**.

Normalmente, uma implementação de **IODataRoutingConvention** faz o seguinte:

1. Compare o modelo de caminho para ver se essa convenção aplica-se a solicitação atual. Se não for aplicável, retorne nulo.
2. Se aplica-se a convenção, use as propriedades do **ODataPathSegment** instâncias para derivar os nomes de controlador e ação.
3. Para ações, adicione todos os valores no dicionário de rota deve associar aos parâmetros de ação (normalmente chaves de entidade).

Vamos examinar um exemplo específico. As convenções de roteamento internas não dão suporte a indexação em uma coleção de navegação. Em outras palavras, não há nenhum convenção para URIs semelhante ao seguinte:

[!code-javascript[Main](odata-routing-conventions/samples/sample3.js)]

Aqui está uma convenção de roteamento personalizada para lidar com esse tipo de consulta.

[!code-csharp[Main](odata-routing-conventions/samples/sample4.cs)]

Notas:

1. Eu derivam **EntitySetRoutingConvention**, porque o **SelectController** método nessa classe é apropriado para essa nova convenção de roteamento. Isso significa que eu não preciso reimplementar **SelectController**.
2. A convenção aplica-se apenas a solicitações GET, e somente quando o modelo de caminho é &quot;~/entityset/key/navigation/key&quot;.
3. É o nome da ação &quot;obter {EntityType}&quot;, onde *{EntityType}* é o tipo de coleção de navegação. Por exemplo, &quot;GetSupplier&quot;. Você pode usar qualquer convenção de nomenclatura que desejar &#8212; apenas certifique-se de ações do controlador correspondem.
4. A ação utiliza dois parâmetros chamados *chave* e *relatedKey*. (Para obter uma lista de alguns nomes de parâmetro predefinidos, consulte [ODataRouteConstants](https://msdn.microsoft.com/library/system.web.http.odata.routing.odatarouteconstants.aspx).)

A próxima etapa é adicionar a nova convenção à lista de convenções de roteamento. Isso acontece durante a configuração, conforme mostrado no código a seguir:

[!code-csharp[Main](odata-routing-conventions/samples/sample5.cs)]

Aqui estão alguns outras exemplo convenções de roteamento que ser úteis para estudo:

- [CompositeKeyRoutingConvention](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataCompositeKeySample/ODataCompositeKeySample/Extensions/CompositeKeyRoutingConvention.cs)
- [CustomNavigationRoutingConvention](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataServiceSample/ODataService/Extensions/CustomNavigationRoutingConvention.cs)
- [NonBindableActionRoutingConvention](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataActionsSample/ODataActionsSample/NonBindableActionRoutingConvention.cs)
- [ODataVersionRouteConstraint](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataVersioningSample/ODataVersioningSample/Extensions/ODataVersionRouteConstraint.cs)

E claro própria API Web é o código-fonte aberto, para que você possa ver os [código-fonte](http://aspnetwebstack.codeplex.com/) para as convenções de roteamento internas. Eles são definidos na **System.Web.Http.OData.Routing.Conventions** namespace.

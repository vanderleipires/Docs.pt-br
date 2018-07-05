---
uid: web-api/overview/odata-support-in-aspnet-web-api/supporting-odata-query-options
title: Suporte a opções de consulta OData na API Web ASP.NET 2 | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/04/2013
ms.topic: article
ms.assetid: 50e6e62b-e72e-4a29-8293-4b67377bd21f
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/supporting-odata-query-options
msc.type: authoredcontent
ms.openlocfilehash: 3ce2b38a13e8684a88bb0ce6183671fae98795c7
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37380837"
---
<a name="supporting-odata-query-options-in-aspnet-web-api-2"></a>Suporte a opções de consulta OData na API Web ASP.NET 2
====================
por [Mike Wasson](https://github.com/MikeWasson)

OData define os parâmetros que podem ser usados para modificar uma consulta de OData. O cliente envia esses parâmetros na cadeia de caracteres de consulta do URI da solicitação. Por exemplo, para classificar os resultados, um cliente usa o parâmetro $orderby:

`http://localhost/Products?$orderby=Name`

A especificação de OData chama esses parâmetros *opções de consulta*. Você pode habilitar as opções de consulta OData para qualquer controlador de API da Web em seu projeto &#8212; o controlador não precisa ser um ponto de extremidade OData. Isso lhe dá uma maneira conveniente de adicionar recursos como filtragem e classificação para qualquer aplicativo de API da Web.

Antes de habilitar as opções de consulta, leia o tópico [diretrizes de segurança do OData](odata-security-guidance.md).

- [Habilitar opções de consulta OData](#enable)
- [Consultas de exemplo](#examples)
- [Paginação orientado para servidor](#server-paging)
- [Limitando as opções de consulta](#limiting_query_options)
- [Opções de consulta invocando diretamente](#ODataQueryOptions)
- [Validação de consulta](#query-validation)

<a id="enable"></a>
## <a name="enabling-odata-query-options"></a>Habilitar opções de consulta OData

API Web suporta as seguintes opções de consulta do OData:

| Opção | Descrição |
| --- | --- |
| $expand | Expande as entidades relacionadas embutido. |
| $filter | Filtra os resultados com base em uma condição booleana. |
| $inlinecount | Informa ao servidor para incluir a contagem total de entidades correspondentes na resposta. (Útil para paginação do lado do servidor). |
| $orderby | Classifica os resultados. |
| $select | Seleciona quais propriedades a serem incluídas na resposta. |
| $skip | Ignora os primeiros n resultados. |
| $top | Retorna somente as primeiras n os resultados. |

Para usar opções de consulta OData, você deve habilitá-las explicitamente. Você pode habilitá-los globalmente para todo o aplicativo ou habilitá-los para controladores específicos ou ações específicas.

Para habilitar as opções de consulta OData globalmente, chame **EnableQuerySupport** sobre o **HttpConfiguration** classe na inicialização:

[!code-csharp[Main](supporting-odata-query-options/samples/sample1.cs)]

O **EnableQuerySupport** método permite que as opções de consulta globalmente para qualquer ação do controlador que retorna um **IQueryable** tipo. Se você não quiser que as opções de consulta habilitadas para o aplicativo inteiro, você poderá habilitá-las para ações do controlador específico, adicionando os **[Queryable]** atributo ao método de ação.

[!code-csharp[Main](supporting-odata-query-options/samples/sample2.cs)]

<a id="examples"></a>
## <a name="example-queries"></a>Consultas de exemplo

Esta seção mostra os tipos de consultas que são possíveis usando as opções de consulta OData. Para obter detalhes específicos sobre as opções de consulta, consulte a documentação do OData em [www.odata.org](http://www.odata.org/).

Para obter informações sobre $expandir e $select, consulte [usando $select, $expand e $value no ASP.NET Web API OData](using-select-expand-and-value.md).

**Paginação controlada por cliente**

Para grandes conjuntos de entidade, o cliente talvez queira limitar o número de resultados. Por exemplo, um cliente pode mostrar 10 entradas por vez, com links "Avançar" para a próxima página de resultados. Para fazer isso, o cliente usa as opções $top e $skip.

`http://localhost/Products?$top=10&$skip=20`

A opção de $top fornece o número máximo de entradas a serem retornadas e a opção $skip fornece o número de entradas a serem ignoradas. O exemplo anterior busca de entradas de 21 a 30.

**Filtragem**

A opção $filter permite que um cliente filtrar os resultados aplicando uma expressão booliana. As expressões de filtro são muito poderosas; Eles incluem operadores aritméticos e lógicos, funções de cadeia de caracteres e funções de data.

| Retorne todos os produtos com a categoria igual a "Toys". | `http://localhost/Products?$filter=Category` EQ 'Toys' |
| --- | --- |
| Retorne todos os produtos com preços inferiores a 10. | `http://localhost/Products?$filter=Price` lt 10 |
| Operadores lógicos: retornar todos os produtos em que o preço > = 5 e preço < = 15. | `http://localhost/Products?$filter=Price` GE 5 e preço le 15 |
| Funções de cadeia de caracteres: retorne todos os produtos com "zz" no nome. | `http://localhost/Products?$filter=substringof('zz',Name)` |
| Funções de data: retornar todos os produtos com ReleaseDate após 2005. | `http://localhost/Products?$filter=year(ReleaseDate)` gt 2005 |

**Classificação**

Para classificar os resultados, use o filtro de $orderby.

| Classificar por preço. | `http://localhost/Products?$orderby=Price` |
| --- | --- |
| Classificar por preço em decrescente (do mais alto ao mais baixo). | `http://localhost/Products?$orderby=Price desc` |
| Classificar por categoria e, em seguida, classificar por preço em decrescente ordem dentro de categorias. | `http://localhost/odata/Products?$orderby=Category,Price desc` |

<a id="server-paging"></a>
## <a name="server-driven-paging"></a>Paginação orientado para servidor

Se seu banco de dados contiver milhões de registros, você não deseja enviá-los todos em uma carga. Para evitar isso, o servidor pode limitar o número de entradas que ele envia em uma única resposta. Para habilitar a paginação de servidor, defina as **PageSize** propriedade no **Queryable** atributo. O valor é o número máximo de entradas a serem retornadas.

[!code-csharp[Main](supporting-odata-query-options/samples/sample3.cs)]

Se o controlador retorna o formato OData, o corpo da resposta conterá um link para a próxima página de dados:

[!code-json[Main](supporting-odata-query-options/samples/sample4.json?highlight=8)]

O cliente pode usar este link para buscar a próxima página. Para saber o número total de entradas no conjunto de resultados, o cliente pode definir a opção de consulta $inlinecount com o valor "allpages".

`http://localhost/Products?$inlinecount=allpages`

O valor "allpages" informa ao servidor para incluir a contagem total na resposta:

[!code-json[Main](supporting-odata-query-options/samples/sample5.json?highlight=3)]

> [!NOTE]
> Links de próxima página e a contagem embutida exigem o formato OData. O motivo é que o OData define campos especiais no corpo da resposta para manter o link e a contagem.


Para formatos não OData, ainda é possível dar suporte à contagem de links e embutida de próxima página, encapsulando os resultados da consulta em uma **PageResult&lt;T&gt;**  objeto. No entanto, ele requer um pouco mais de código. Veja um exemplo:

[!code-csharp[Main](supporting-odata-query-options/samples/sample6.cs)]

Aqui está um exemplo de resposta JSON:

[!code-json[Main](supporting-odata-query-options/samples/sample7.json)]

<a id="limiting_query_options"></a>
## <a name="limiting-the-query-options"></a>Limitando as opções de consulta

As opções de consulta oferecem uma enorme de controle sobre a consulta que é executada no servidor de cliente. Em alguns casos, você talvez queira limitar as opções disponíveis por motivos de segurança ou o desempenho. O **[Queryable]** atributo tem alguns compilados nas propriedades para isso. Aqui estão alguns exemplos.

Permitir apenas $skip Skip e $ $top, para dar suporte à paginação e nada mais:

[!code-csharp[Main](supporting-odata-query-options/samples/sample8.cs)]

Permitir ordenação somente por determinadas propriedades, para impedir a classificação nas propriedades que não são indexadas no banco de dados:

[!code-csharp[Main](supporting-odata-query-options/samples/sample9.cs)]

Permitir que a função de lógica "eq", mas não há outras funções lógicas:

[!code-csharp[Main](supporting-odata-query-options/samples/sample10.cs)]

Não permitir todos os operadores aritméticos:

[!code-csharp[Main](supporting-odata-query-options/samples/sample11.cs)]

Você pode restringir opções globalmente, criando uma **QueryableAttribute** instância e passá-lo para o **EnableQuerySupport** função:

[!code-csharp[Main](supporting-odata-query-options/samples/sample12.cs)]

<a id="ODataQueryOptions"></a>
## <a name="invoking-query-options-directly"></a>Opções de consulta invocando diretamente

Em vez de usar o **[Queryable]** atributo, você pode invocar as opções de consulta diretamente no seu controlador. Para fazer isso, adicione uma **ODataQueryOptions** parâmetro para o método do controlador. Nesse caso, você não precisa de **[Queryable]** atributo.

[!code-csharp[Main](supporting-odata-query-options/samples/sample13.cs)]

API da Web preenche o **ODataQueryOptions** do URI de cadeia de caracteres de consulta. Para aplicar a consulta, passe uma **IQueryable** para o **ApplyTo** método. O método retorna outra **IQueryable**.

Para cenários avançados, se você não tiver um **IQueryable** provedor de consulta, você pode examinar as **ODataQueryOptions** e traduzir as opções de consulta em outra forma. (Por exemplo, consulte de RaghuRam Nadiminti postagem de blog [consultas traduzindo OData para HQL](https://blogs.msdn.com/b/webdev/archive/2013/02/25/translating-odata-queries-to-hql.aspx), que também inclui um [exemplo](http://aspnet.codeplex.com/SourceControl/changeset/view/75a56ec99968#Samples/WebApi/NHibernateQueryableSample/Readme.txt).)

<a id="query-validation"></a>
## <a name="query-validation"></a>Validação de consulta

O **[Queryable]** atributo valida a consulta antes de executá-lo. A etapa de validação é executada na **QueryableAttribute.ValidateQuery** método. Você também pode personalizar o processo de validação.

Consulte também [diretrizes de segurança do OData](odata-security-guidance.md).

Em primeiro lugar, a substituição de uma do validador de classes que é definida na **Web.Http.OData.Query.Validators** namespace. Por exemplo, a seguinte classe de validador desabilita a opção 'desc' para a opção $orderby.

[!code-csharp[Main](supporting-odata-query-options/samples/sample14.cs)]

Subclasse de **[Queryable]** atributo para substituir a **ValidateQuery** método.

[!code-csharp[Main](supporting-odata-query-options/samples/sample15.cs)]

Em seguida, defina o atributo personalizado seja globalmente ou por controlador:

[!code-csharp[Main](supporting-odata-query-options/samples/sample16.cs)]

Se você estiver usando **ODataQueryOptions** diretamente, defina o validador nas opções:

[!code-csharp[Main](supporting-odata-query-options/samples/sample17.cs)]

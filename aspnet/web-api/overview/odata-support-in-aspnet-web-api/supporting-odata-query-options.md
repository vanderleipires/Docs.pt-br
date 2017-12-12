---
uid: web-api/overview/odata-support-in-aspnet-web-api/supporting-odata-query-options
title: "Suporte a opções de consulta OData no ASP.NET Web API 2 | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/04/2013
ms.topic: article
ms.assetid: 50e6e62b-e72e-4a29-8293-4b67377bd21f
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/supporting-odata-query-options
msc.type: authoredcontent
ms.openlocfilehash: 004c029db6f01627f7cadff26aaf5554ce2b93a5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="supporting-odata-query-options-in-aspnet-web-api-2"></a><span data-ttu-id="2b912-102">Dando suporte a opções de consulta OData em ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="2b912-102">Supporting OData Query Options in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="2b912-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="2b912-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="2b912-104">OData define os parâmetros que podem ser usados para modificar uma consulta OData.</span><span class="sxs-lookup"><span data-stu-id="2b912-104">OData defines parameters that can be used to modify an OData query.</span></span> <span data-ttu-id="2b912-105">O cliente envia esses parâmetros na cadeia de caracteres de consulta do URI da solicitação.</span><span class="sxs-lookup"><span data-stu-id="2b912-105">The client sends these parameters in the query string of the request URI.</span></span> <span data-ttu-id="2b912-106">Por exemplo, para classificar os resultados, um cliente usa o parâmetro $orderby:</span><span class="sxs-lookup"><span data-stu-id="2b912-106">For example, to sort the results, a client uses the $orderby parameter:</span></span>

`http://localhost/Products?$orderby=Name`

<span data-ttu-id="2b912-107">A especificação de OData chama esses parâmetros *opções de consulta*.</span><span class="sxs-lookup"><span data-stu-id="2b912-107">The OData specification calls these parameters *query options*.</span></span> <span data-ttu-id="2b912-108">Você pode habilitar as opções de consulta OData para qualquer controlador de API da Web em seu projeto &#8212; o controlador não precisa ser um ponto de extremidade OData.</span><span class="sxs-lookup"><span data-stu-id="2b912-108">You can enable OData query options for any Web API controller in your project &#8212; the controller does not need to be an OData endpoint.</span></span> <span data-ttu-id="2b912-109">Isso fornece uma maneira conveniente para adicionar recursos como filtragem e classificação a qualquer aplicativo de API da Web.</span><span class="sxs-lookup"><span data-stu-id="2b912-109">This gives you a convenient way to add features such as filtering and sorting to any Web API application.</span></span>

<span data-ttu-id="2b912-110">Antes de habilitar as opções de consulta, leia o tópico [orientações de segurança do OData](odata-security-guidance.md).</span><span class="sxs-lookup"><span data-stu-id="2b912-110">Before enabling query options, please read the topic [OData Security Guidance](odata-security-guidance.md).</span></span>

- [<span data-ttu-id="2b912-111">Habilitar opções de consulta OData</span><span class="sxs-lookup"><span data-stu-id="2b912-111">Enabling OData Query Options</span></span>](#enable)
- [<span data-ttu-id="2b912-112">Consultas de exemplo</span><span class="sxs-lookup"><span data-stu-id="2b912-112">Example Queries</span></span>](#examples)
- [<span data-ttu-id="2b912-113">Paginação orientado para servidor</span><span class="sxs-lookup"><span data-stu-id="2b912-113">Server-Driven Paging</span></span>](#server-paging)
- [<span data-ttu-id="2b912-114">Limitando as opções de consulta</span><span class="sxs-lookup"><span data-stu-id="2b912-114">Limiting the Query Options</span></span>](#limiting_query_options)
- [<span data-ttu-id="2b912-115">Opções de consulta chamada diretamente</span><span class="sxs-lookup"><span data-stu-id="2b912-115">Invoking Query Options Directly</span></span>](#ODataQueryOptions)
- [<span data-ttu-id="2b912-116">Validação de consulta</span><span class="sxs-lookup"><span data-stu-id="2b912-116">Query Validation</span></span>](#query-validation)

<a id="enable"></a>
## <a name="enabling-odata-query-options"></a><span data-ttu-id="2b912-117">Habilitar opções de consulta OData</span><span class="sxs-lookup"><span data-stu-id="2b912-117">Enabling OData Query Options</span></span>

<span data-ttu-id="2b912-118">As seguintes opções de consulta OData oferece suporte a API da Web:</span><span class="sxs-lookup"><span data-stu-id="2b912-118">Web API supports the following OData query options:</span></span>

| <span data-ttu-id="2b912-119">Opção</span><span class="sxs-lookup"><span data-stu-id="2b912-119">Option</span></span> | <span data-ttu-id="2b912-120">Descrição</span><span class="sxs-lookup"><span data-stu-id="2b912-120">Description</span></span> |
| --- | --- |
| <span data-ttu-id="2b912-121">$expand</span><span class="sxs-lookup"><span data-stu-id="2b912-121">$expand</span></span> | <span data-ttu-id="2b912-122">Expande embutido de entidades relacionadas.</span><span class="sxs-lookup"><span data-stu-id="2b912-122">Expands related entities inline.</span></span> |
| <span data-ttu-id="2b912-123">$filter</span><span class="sxs-lookup"><span data-stu-id="2b912-123">$filter</span></span> | <span data-ttu-id="2b912-124">Filtra os resultados com base em uma condição booleana.</span><span class="sxs-lookup"><span data-stu-id="2b912-124">Filters the results, based on a Boolean condition.</span></span> |
| <span data-ttu-id="2b912-125">$inlinecount</span><span class="sxs-lookup"><span data-stu-id="2b912-125">$inlinecount</span></span> | <span data-ttu-id="2b912-126">Informa ao servidor para incluir a contagem total de entidades correspondentes na resposta.</span><span class="sxs-lookup"><span data-stu-id="2b912-126">Tells the server to include the total count of matching entities in the response.</span></span> <span data-ttu-id="2b912-127">(Útil para paginação de servidor).</span><span class="sxs-lookup"><span data-stu-id="2b912-127">(Useful for server-side paging.)</span></span> |
| <span data-ttu-id="2b912-128">$orderby</span><span class="sxs-lookup"><span data-stu-id="2b912-128">$orderby</span></span> | <span data-ttu-id="2b912-129">Classifica os resultados.</span><span class="sxs-lookup"><span data-stu-id="2b912-129">Sorts the results.</span></span> |
| <span data-ttu-id="2b912-130">$select</span><span class="sxs-lookup"><span data-stu-id="2b912-130">$select</span></span> | <span data-ttu-id="2b912-131">Seleciona quais propriedades a serem incluídas na resposta.</span><span class="sxs-lookup"><span data-stu-id="2b912-131">Selects which properties to include in the response.</span></span> |
| <span data-ttu-id="2b912-132">$skip</span><span class="sxs-lookup"><span data-stu-id="2b912-132">$skip</span></span> | <span data-ttu-id="2b912-133">Ignora os primeiros n resultados.</span><span class="sxs-lookup"><span data-stu-id="2b912-133">Skips the first n results.</span></span> |
| <span data-ttu-id="2b912-134">$top</span><span class="sxs-lookup"><span data-stu-id="2b912-134">$top</span></span> | <span data-ttu-id="2b912-135">Retorna apenas o n primeiro os resultados.</span><span class="sxs-lookup"><span data-stu-id="2b912-135">Returns only the first n the results.</span></span> |

<span data-ttu-id="2b912-136">Para usar opções de consulta OData, você deve habilitá-las explicitamente.</span><span class="sxs-lookup"><span data-stu-id="2b912-136">To use OData query options, you must enable them explicitly.</span></span> <span data-ttu-id="2b912-137">Você pode habilitá-las globalmente para o aplicativo inteiro, ou habilitá-las para controladores específicos ou ações específicas.</span><span class="sxs-lookup"><span data-stu-id="2b912-137">You can enable them globally for the entire application, or enable them for specific controllers or specific actions.</span></span>

<span data-ttu-id="2b912-138">Para habilitar as opções de consulta OData globalmente, chame **EnableQuerySupport** no **HttpConfiguration** classe durante a inicialização:</span><span class="sxs-lookup"><span data-stu-id="2b912-138">To enable OData query options globally, call **EnableQuerySupport** on the **HttpConfiguration** class at startup:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample1.cs)]

<span data-ttu-id="2b912-139">O **EnableQuerySupport** método habilita as opções de consulta global para qualquer ação de controlador que retorna um **IQueryable** tipo.</span><span class="sxs-lookup"><span data-stu-id="2b912-139">The **EnableQuerySupport** method enables query options globally for any controller action that returns an **IQueryable** type.</span></span> <span data-ttu-id="2b912-140">Se não quiser que as opções de consulta habilitadas para o aplicativo inteiro, você pode habilitá-las para ações do controlador específico adicionando o **[Queryable]** atributo ao método de ação.</span><span class="sxs-lookup"><span data-stu-id="2b912-140">If you don't want query options enabled for the entire application, you can enable them for specific controller actions by adding the **[Queryable]** attribute to the action method.</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample2.cs)]

<a id="examples"></a>
## <a name="example-queries"></a><span data-ttu-id="2b912-141">Consultas de exemplo</span><span class="sxs-lookup"><span data-stu-id="2b912-141">Example Queries</span></span>

<span data-ttu-id="2b912-142">Esta seção mostra os tipos de consultas que são possíveis usando as opções de consulta OData.</span><span class="sxs-lookup"><span data-stu-id="2b912-142">This section shows the types of queries that are possible using the OData query options.</span></span> <span data-ttu-id="2b912-143">Para obter detalhes específicos sobre as opções de consulta, consulte a documentação de OData em [www.odata.org](http://www.odata.org/).</span><span class="sxs-lookup"><span data-stu-id="2b912-143">For specific details about the query options, refer to the OData documentation at [www.odata.org](http://www.odata.org/).</span></span>

<span data-ttu-id="2b912-144">Para obter informações sobre $expandir e $select, consulte [usando $select, $expand e $value no ASP.NET Web API OData](using-select-expand-and-value.md).</span><span class="sxs-lookup"><span data-stu-id="2b912-144">For information about $expand and $select, see [Using $select, $expand, and $value in ASP.NET Web API OData](using-select-expand-and-value.md).</span></span>

<span data-ttu-id="2b912-145">**Paginação controlada por cliente**</span><span class="sxs-lookup"><span data-stu-id="2b912-145">**Client-Driven Paging**</span></span>

<span data-ttu-id="2b912-146">Para grandes conjuntos de entidade, o cliente pode querer limitar o número de resultados.</span><span class="sxs-lookup"><span data-stu-id="2b912-146">For large entity sets, the client might want to limit the number of results.</span></span> <span data-ttu-id="2b912-147">Por exemplo, um cliente pode mostrar 10 entradas por vez, com links "Avançar" para a próxima página de resultados.</span><span class="sxs-lookup"><span data-stu-id="2b912-147">For example, a client might show 10 entries at a time, with "next" links to get the next page of results.</span></span> <span data-ttu-id="2b912-148">Para fazer isso, o cliente usa as opções $top e $skip.</span><span class="sxs-lookup"><span data-stu-id="2b912-148">To do this, the client uses the $top and $skip options.</span></span>

`http://localhost/Products?$top=10&$skip=20`

<span data-ttu-id="2b912-149">A opção $top fornece o número máximo de entradas a serem retornadas e a opção $skip retorna o número de entradas a pular.</span><span class="sxs-lookup"><span data-stu-id="2b912-149">The $top option gives the maximum number of entries to return, and the $skip option gives the number of entries to skip.</span></span> <span data-ttu-id="2b912-150">O exemplo anterior busca de entradas de 21 a 30.</span><span class="sxs-lookup"><span data-stu-id="2b912-150">The previous example fetches entries 21 through 30.</span></span>

<span data-ttu-id="2b912-151">**Filtragem**</span><span class="sxs-lookup"><span data-stu-id="2b912-151">**Filtering**</span></span>

<span data-ttu-id="2b912-152">A opção $filter permite que um cliente filtrar os resultados, aplicando uma expressão booleana.</span><span class="sxs-lookup"><span data-stu-id="2b912-152">The $filter option lets a client filter the results by applying a Boolean expression.</span></span> <span data-ttu-id="2b912-153">As expressões de filtro são muito eficientes; Elas incluem os operadores aritméticos e lógicos, funções de cadeia de caracteres e funções de data.</span><span class="sxs-lookup"><span data-stu-id="2b912-153">The filter expressions are quite powerful; they include logical and arithmetic operators, string functions, and date functions.</span></span>

| <span data-ttu-id="2b912-154">Retorne todos os produtos com categoria igual a "Toys".</span><span class="sxs-lookup"><span data-stu-id="2b912-154">Return all products with category equal to "Toys".</span></span> | <span data-ttu-id="2b912-155">`http://localhost/Products?$filter=Category`EQ 'Toys'</span><span class="sxs-lookup"><span data-stu-id="2b912-155">`http://localhost/Products?$filter=Category` eq 'Toys'</span></span> |
| --- | --- |
| <span data-ttu-id="2b912-156">Retorne todos os produtos com preços inferiores a 10.</span><span class="sxs-lookup"><span data-stu-id="2b912-156">Return all products with price less than 10.</span></span> | <span data-ttu-id="2b912-157">`http://localhost/Products?$filter=Price`lt 10</span><span class="sxs-lookup"><span data-stu-id="2b912-157">`http://localhost/Products?$filter=Price` lt 10</span></span> |
| <span data-ttu-id="2b912-158">Operadores lógicos: retornar todos os produtos onde price > = 5 e preço < = 15.</span><span class="sxs-lookup"><span data-stu-id="2b912-158">Logical operators: Return all products where price >= 5 and price <= 15.</span></span> | <span data-ttu-id="2b912-159">`http://localhost/Products?$filter=Price`GE 5 e le preço 15</span><span class="sxs-lookup"><span data-stu-id="2b912-159">`http://localhost/Products?$filter=Price` ge 5 and Price le 15</span></span> |
| <span data-ttu-id="2b912-160">Funções de cadeia de caracteres: retornar todos os produtos com "zz" no nome.</span><span class="sxs-lookup"><span data-stu-id="2b912-160">String functions: Return all products with "zz" in the name.</span></span> | `http://localhost/Products?$filter=substringof('zz',Name)` |
| <span data-ttu-id="2b912-161">Funções de data: retornar todos os produtos com ReleaseDate após 2005.</span><span class="sxs-lookup"><span data-stu-id="2b912-161">Date functions: Return all products with ReleaseDate after 2005.</span></span> | <span data-ttu-id="2b912-162">`http://localhost/Products?$filter=year(ReleaseDate)`gt 2005</span><span class="sxs-lookup"><span data-stu-id="2b912-162">`http://localhost/Products?$filter=year(ReleaseDate)` gt 2005</span></span> |

<span data-ttu-id="2b912-163">**Classificação**</span><span class="sxs-lookup"><span data-stu-id="2b912-163">**Sorting**</span></span>

<span data-ttu-id="2b912-164">Para classificar os resultados, use o filtro de $orderby.</span><span class="sxs-lookup"><span data-stu-id="2b912-164">To sort the results, use the $orderby filter.</span></span>

| <span data-ttu-id="2b912-165">Classificar por preço.</span><span class="sxs-lookup"><span data-stu-id="2b912-165">Sort by price.</span></span> | `http://localhost/Products?$orderby=Price` |
| --- | --- |
| <span data-ttu-id="2b912-166">Classificar por preço em decrescente (do mais alto ao mais baixo).</span><span class="sxs-lookup"><span data-stu-id="2b912-166">Sort by price in descending order (highest to lowest).</span></span> | `http://localhost/Products?$orderby=Price desc` |
| <span data-ttu-id="2b912-167">Classificar por categoria e, em seguida, classificar por preço em decrescente em categorias.</span><span class="sxs-lookup"><span data-stu-id="2b912-167">Sort by category, then sort by price in descending order within categories.</span></span> | `http://localhost/odata/Products?$orderby=Category,Price desc` |

<a id="server-paging"></a>
## <a name="server-driven-paging"></a><span data-ttu-id="2b912-168">Paginação orientado para servidor</span><span class="sxs-lookup"><span data-stu-id="2b912-168">Server-Driven Paging</span></span>

<span data-ttu-id="2b912-169">Se seu banco de dados contém milhões de registros, você não deseja enviá-los em uma carga.</span><span class="sxs-lookup"><span data-stu-id="2b912-169">If your database contains millions of records, you don't want to send them all in one payload.</span></span> <span data-ttu-id="2b912-170">Para evitar isso, o servidor pode limitar o número de entradas que ele envia em uma única resposta.</span><span class="sxs-lookup"><span data-stu-id="2b912-170">To prevent this, the server can limit the number of entries that it sends in a single response.</span></span> <span data-ttu-id="2b912-171">Para habilitar a paginação de servidor, defina o **PageSize** propriedade o **Queryable** atributo.</span><span class="sxs-lookup"><span data-stu-id="2b912-171">To enable server paging, set the **PageSize** property in the **Queryable** attribute.</span></span> <span data-ttu-id="2b912-172">O valor é o número máximo de entradas a serem retornadas.</span><span class="sxs-lookup"><span data-stu-id="2b912-172">The value is the maximum number of entries to return.</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample3.cs)]

<span data-ttu-id="2b912-173">Se o controlador retorna o formato do OData, o corpo da resposta conterá um link para a próxima página de dados:</span><span class="sxs-lookup"><span data-stu-id="2b912-173">If your controller returns OData format, the response body will contain a link to the next page of data:</span></span>

[!code-json[Main](supporting-odata-query-options/samples/sample4.json?highlight=8)]

<span data-ttu-id="2b912-174">O cliente pode usar este link para buscar a próxima página.</span><span class="sxs-lookup"><span data-stu-id="2b912-174">The client can use this link to fetch the next page.</span></span> <span data-ttu-id="2b912-175">Para saber o número total de entradas no conjunto de resultados, o cliente pode definir a opção de consulta $inlinecount com o valor "allpages".</span><span class="sxs-lookup"><span data-stu-id="2b912-175">To learn the total number of entries in the result set, the client can set the $inlinecount query option with the value "allpages".</span></span>

`http://localhost/Products?$inlinecount=allpages`

<span data-ttu-id="2b912-176">O valor de "allpages" informa o servidor para incluir a contagem total na resposta:</span><span class="sxs-lookup"><span data-stu-id="2b912-176">The value "allpages" tells the server to include the total count in the response:</span></span>

[!code-json[Main](supporting-odata-query-options/samples/sample5.json?highlight=3)]

> [!NOTE]
> <span data-ttu-id="2b912-177">Links de próxima página e a contagem embutida requerem o formato do OData.</span><span class="sxs-lookup"><span data-stu-id="2b912-177">Next-page links and inline count both require OData format.</span></span> <span data-ttu-id="2b912-178">O motivo é que o OData define os campos especiais no corpo da resposta para manter o link e a contagem.</span><span class="sxs-lookup"><span data-stu-id="2b912-178">The reason is that OData defines special fields in the response body to hold the link and count.</span></span>


<span data-ttu-id="2b912-179">Para formatos de OData não, ainda é possível dar suporte a contagem de links e embutido de próxima página, encapsulando os resultados da consulta em uma **PageResult&lt;T&gt;**  objeto.</span><span class="sxs-lookup"><span data-stu-id="2b912-179">For non-OData formats, it is still possible to support next-page links and inline count, by wrapping the query results in a **PageResult&lt;T&gt;** object.</span></span> <span data-ttu-id="2b912-180">No entanto, ele exige um pouco mais de código.</span><span class="sxs-lookup"><span data-stu-id="2b912-180">However, it requires a bit more code.</span></span> <span data-ttu-id="2b912-181">Veja um exemplo:</span><span class="sxs-lookup"><span data-stu-id="2b912-181">Here is an example:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample6.cs)]

<span data-ttu-id="2b912-182">Aqui está um exemplo de resposta de JSON:</span><span class="sxs-lookup"><span data-stu-id="2b912-182">Here is an example JSON response:</span></span>

[!code-json[Main](supporting-odata-query-options/samples/sample7.json)]

<a id="limiting_query_options"></a>
## <a name="limiting-the-query-options"></a><span data-ttu-id="2b912-183">Limitando as opções de consulta</span><span class="sxs-lookup"><span data-stu-id="2b912-183">Limiting the Query Options</span></span>

<span data-ttu-id="2b912-184">As opções de consulta oferecem o cliente muito controle sobre a consulta que é executada no servidor.</span><span class="sxs-lookup"><span data-stu-id="2b912-184">The query options give the client a lot of control over the query that is run on the server.</span></span> <span data-ttu-id="2b912-185">Em alguns casos, você talvez queira limitar as opções disponíveis por motivos de segurança ou desempenho.</span><span class="sxs-lookup"><span data-stu-id="2b912-185">In some cases, you might want to limit the available options for security or performance reasons.</span></span> <span data-ttu-id="2b912-186">O **[Queryable]** atributo alguns criou nas propriedades para isso.</span><span class="sxs-lookup"><span data-stu-id="2b912-186">The **[Queryable]** attribute has some built in properties for this.</span></span> <span data-ttu-id="2b912-187">Aqui estão alguns exemplos.</span><span class="sxs-lookup"><span data-stu-id="2b912-187">Here are some examples.</span></span>

<span data-ttu-id="2b912-188">Permitir apenas $skip Skip e $ $top, para dar suporte a paginação e nada mais:</span><span class="sxs-lookup"><span data-stu-id="2b912-188">Allow only $skip and $top, to support paging and nothing else:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample8.cs)]

<span data-ttu-id="2b912-189">Permitir pedidos somente por algumas propriedades impedir a classificação em propriedades que não são indexadas no banco de dados:</span><span class="sxs-lookup"><span data-stu-id="2b912-189">Allow ordering only by certain properties, to prevent sorting on properties that are not indexed in the database:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample9.cs)]

<span data-ttu-id="2b912-190">Permitir que a função lógica "eq", mas não outras funções lógicas:</span><span class="sxs-lookup"><span data-stu-id="2b912-190">Allow the "eq" logical function but no other logical functions:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample10.cs)]

<span data-ttu-id="2b912-191">Não permite que os operadores aritméticos:</span><span class="sxs-lookup"><span data-stu-id="2b912-191">Do not allow any arithmetic operators:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample11.cs)]

<span data-ttu-id="2b912-192">Você pode restringir opções globalmente, criando um **QueryableAttribute** instância e passá-lo para o **EnableQuerySupport** função:</span><span class="sxs-lookup"><span data-stu-id="2b912-192">You can restrict options globally by constructing a **QueryableAttribute** instance and passing it to the **EnableQuerySupport** function:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample12.cs)]

<a id="ODataQueryOptions"></a>
## <a name="invoking-query-options-directly"></a><span data-ttu-id="2b912-193">Opções de consulta chamada diretamente</span><span class="sxs-lookup"><span data-stu-id="2b912-193">Invoking Query Options Directly</span></span>

<span data-ttu-id="2b912-194">Em vez de usar o **[Queryable]** atributo, você pode invocar as opções de consulta diretamente no seu controlador.</span><span class="sxs-lookup"><span data-stu-id="2b912-194">Instead of using the **[Queryable]** attribute, you can invoke the query options directly in your controller.</span></span> <span data-ttu-id="2b912-195">Para fazer isso, adicione um **ODataQueryOptions** parâmetro para o método do controlador.</span><span class="sxs-lookup"><span data-stu-id="2b912-195">To do so, add an **ODataQueryOptions** parameter to the controller method.</span></span> <span data-ttu-id="2b912-196">Nesse caso, não é necessário o **[Queryable]** atributo.</span><span class="sxs-lookup"><span data-stu-id="2b912-196">In this case, you don't need the **[Queryable]** attribute.</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample13.cs)]

<span data-ttu-id="2b912-197">API da Web preenche o **ODataQueryOptions** do URI de cadeia de caracteres de consulta.</span><span class="sxs-lookup"><span data-stu-id="2b912-197">Web API populates the **ODataQueryOptions** from the URI query string.</span></span> <span data-ttu-id="2b912-198">Para aplicar a consulta, passar um **IQueryable** para o **ApplyTo** método.</span><span class="sxs-lookup"><span data-stu-id="2b912-198">To apply the query, pass an **IQueryable** to the **ApplyTo** method.</span></span> <span data-ttu-id="2b912-199">O método retorna outro **IQueryable**.</span><span class="sxs-lookup"><span data-stu-id="2b912-199">The method returns another **IQueryable**.</span></span>

<span data-ttu-id="2b912-200">Para cenários avançados, se você não tiver um **IQueryable** provedor de consulta, você pode examinar o **ODataQueryOptions** e traduzir as opções de consulta em outra forma.</span><span class="sxs-lookup"><span data-stu-id="2b912-200">For advanced scenarios, if you do not have an **IQueryable** query provider, you can examine the **ODataQueryOptions** and translate the query options into another form.</span></span> <span data-ttu-id="2b912-201">(Por exemplo, consulte de RaghuRam Nadiminti postagem de blog [consultas convertendo OData para HQL](https://blogs.msdn.com/b/webdev/archive/2013/02/25/translating-odata-queries-to-hql.aspx), que também inclui um [exemplo](http://aspnet.codeplex.com/SourceControl/changeset/view/75a56ec99968#Samples/WebApi/NHibernateQueryableSample/Readme.txt).)</span><span class="sxs-lookup"><span data-stu-id="2b912-201">(For example, see RaghuRam Nadiminti's blog post [Translating OData queries to HQL](https://blogs.msdn.com/b/webdev/archive/2013/02/25/translating-odata-queries-to-hql.aspx), which also includes a [sample](http://aspnet.codeplex.com/SourceControl/changeset/view/75a56ec99968#Samples/WebApi/NHibernateQueryableSample/Readme.txt).)</span></span>

<a id="query-validation"></a>
## <a name="query-validation"></a><span data-ttu-id="2b912-202">Validação de consulta</span><span class="sxs-lookup"><span data-stu-id="2b912-202">Query Validation</span></span>

<span data-ttu-id="2b912-203">O **[Queryable]** atributo valida a consulta antes da execução.</span><span class="sxs-lookup"><span data-stu-id="2b912-203">The **[Queryable]** attribute validates the query before executing it.</span></span> <span data-ttu-id="2b912-204">A etapa de validação é executada no **QueryableAttribute.ValidateQuery** método.</span><span class="sxs-lookup"><span data-stu-id="2b912-204">The validation step is performed in the **QueryableAttribute.ValidateQuery** method.</span></span> <span data-ttu-id="2b912-205">Você também pode personalizar o processo de validação.</span><span class="sxs-lookup"><span data-stu-id="2b912-205">You can also customize the validation process.</span></span>

<span data-ttu-id="2b912-206">Consulte também [orientações de segurança do OData](odata-security-guidance.md).</span><span class="sxs-lookup"><span data-stu-id="2b912-206">Also see [OData Security Guidance](odata-security-guidance.md).</span></span>

<span data-ttu-id="2b912-207">Primeiro, a substituição de um validador de classes que é definida no **Web.Http.OData.Query.Validators** namespace.</span><span class="sxs-lookup"><span data-stu-id="2b912-207">First, override one of the validator classes that is defined in the **Web.Http.OData.Query.Validators** namespace.</span></span> <span data-ttu-id="2b912-208">Por exemplo, a seguinte classe de validador desabilita a opção 'desc' para a opção $orderby.</span><span class="sxs-lookup"><span data-stu-id="2b912-208">For example, the following validator class disables the 'desc' option for the $orderby option.</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample14.cs)]

<span data-ttu-id="2b912-209">Subclasse o **[Queryable]** atributo para substituir o **ValidateQuery** método.</span><span class="sxs-lookup"><span data-stu-id="2b912-209">Subclass the **[Queryable]** attribute to override the **ValidateQuery** method.</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample15.cs)]

<span data-ttu-id="2b912-210">Em seguida, definir o atributo personalizado ou global ou por controlador:</span><span class="sxs-lookup"><span data-stu-id="2b912-210">Then set your custom attribute either globally or per-controller:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample16.cs)]

<span data-ttu-id="2b912-211">Se você estiver usando **ODataQueryOptions** diretamente, defina o validador nas opções:</span><span class="sxs-lookup"><span data-stu-id="2b912-211">If you are using **ODataQueryOptions** directly, set the validator on the options:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample17.cs)]

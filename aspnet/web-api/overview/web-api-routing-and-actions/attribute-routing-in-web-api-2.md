---
uid: web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
title: Atributo de roteamento na API Web ASP.NET 2 | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 01/20/2014
ms.assetid: 979d6c9f-0129-4e5b-ae56-4507b281b86d
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: f13720c5e9de99fb4ae5b27a757c257cac881f89
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41824385"
---
<a name="attribute-routing-in-aspnet-web-api-2"></a><span data-ttu-id="f9edb-102">Roteamento de atributo na API Web ASP.NET 2</span><span class="sxs-lookup"><span data-stu-id="f9edb-102">Attribute Routing in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="f9edb-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="f9edb-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="f9edb-104">*Roteamento* é como API Web corresponde a um URI para uma ação.</span><span class="sxs-lookup"><span data-stu-id="f9edb-104">*Routing* is how Web API matches a URI to an action.</span></span> <span data-ttu-id="f9edb-105">API Web 2 dá suporte a um novo tipo de roteamento, chamado *roteamento de atributo*.</span><span class="sxs-lookup"><span data-stu-id="f9edb-105">Web API 2 supports a new type of routing, called *attribute routing*.</span></span> <span data-ttu-id="f9edb-106">Como o nome implica, roteamento de atributo usa atributos para definir rotas.</span><span class="sxs-lookup"><span data-stu-id="f9edb-106">As the name implies, attribute routing uses attributes to define routes.</span></span> <span data-ttu-id="f9edb-107">Roteamento de atributo fornece mais controle sobre os URIs na API web.</span><span class="sxs-lookup"><span data-stu-id="f9edb-107">Attribute routing gives you more control over the URIs in your web API.</span></span> <span data-ttu-id="f9edb-108">Por exemplo, você pode criar facilmente os URIs que descrevem as hierarquias de recursos.</span><span class="sxs-lookup"><span data-stu-id="f9edb-108">For example, you can easily create URIs that describe hierarchies of resources.</span></span>

<span data-ttu-id="f9edb-109">O estilo anterior de roteamento, chamado baseado em convenção de roteamento, ainda é totalmente suportado.</span><span class="sxs-lookup"><span data-stu-id="f9edb-109">The earlier style of routing, called convention-based routing, is still fully supported.</span></span> <span data-ttu-id="f9edb-110">Na verdade, você pode combinar as duas técnicas no mesmo projeto.</span><span class="sxs-lookup"><span data-stu-id="f9edb-110">In fact, you can combine both techniques in the same project.</span></span>

<span data-ttu-id="f9edb-111">Este tópico mostra como habilitar o roteamento de atributo e descreve as várias opções para o roteamento de atributo.</span><span class="sxs-lookup"><span data-stu-id="f9edb-111">This topic shows how to enable attribute routing and describes the various options for attribute routing.</span></span> <span data-ttu-id="f9edb-112">Para obter um tutorial de ponta a ponta que usa o roteamento de atributo, consulte [criar uma API REST com roteamento de atributo na API Web 2](create-a-rest-api-with-attribute-routing.md).</span><span class="sxs-lookup"><span data-stu-id="f9edb-112">For an end-to-end tutorial that uses attribute routing, see [Create a REST API with Attribute Routing in Web API 2](create-a-rest-api-with-attribute-routing.md).</span></span>


## <a name="prerequisites"></a><span data-ttu-id="f9edb-113">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="f9edb-113">Prerequisites</span></span>

<span data-ttu-id="f9edb-114">[Visual Studio 2017](https://www.visualstudio.com/vs/) Community, Professional ou Enterprise Edition</span><span class="sxs-lookup"><span data-stu-id="f9edb-114">[Visual Studio 2017](https://www.visualstudio.com/vs/) Community, Professional, or Enterprise Edition</span></span>

<span data-ttu-id="f9edb-115">Como alternativa, use o Gerenciador de pacotes NuGet para instalar os pacotes necessários.</span><span class="sxs-lookup"><span data-stu-id="f9edb-115">Alternatively, use NuGet Package Manager to install the necessary packages.</span></span> <span data-ttu-id="f9edb-116">Dos **ferramentas** menu no Visual Studio, selecione **Gerenciador de pacotes de biblioteca**, em seguida, selecione **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="f9edb-116">From the **Tools** menu in Visual Studio, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="f9edb-117">Digite o seguinte comando na janela do Console do Gerenciador de pacotes:</span><span class="sxs-lookup"><span data-stu-id="f9edb-117">Enter the following command in the Package Manager Console window:</span></span>

`Install-Package Microsoft.AspNet.WebApi.WebHost`

<a id="why"></a>
## <a name="why-attribute-routing"></a><span data-ttu-id="f9edb-118">Por que roteamento de atributo?</span><span class="sxs-lookup"><span data-stu-id="f9edb-118">Why Attribute Routing?</span></span>

<span data-ttu-id="f9edb-119">A primeira versão da API da Web usado *baseado em convenção* roteamento.</span><span class="sxs-lookup"><span data-stu-id="f9edb-119">The first release of Web API used *convention-based* routing.</span></span> <span data-ttu-id="f9edb-120">Esse tipo de roteamento, você definir um ou mais modelos de rota, que são basicamente com parâmetros de cadeias de caracteres.</span><span class="sxs-lookup"><span data-stu-id="f9edb-120">In that type of routing, you define one or more route templates, which are basically parameterized strings.</span></span> <span data-ttu-id="f9edb-121">Quando o framework recebe uma solicitação, ele corresponde ao URI em relação ao modelo de rota.</span><span class="sxs-lookup"><span data-stu-id="f9edb-121">When the framework receives a request, it matches the URI against the route template.</span></span> <span data-ttu-id="f9edb-122">(Para obter mais informações sobre roteamento baseado em convenção, consulte [roteamento na API Web ASP.NET](routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="f9edb-122">(For more information about convention-based routing, see [Routing in ASP.NET Web API](routing-in-aspnet-web-api.md).</span></span>

<span data-ttu-id="f9edb-123">Uma vantagem do roteamento baseado em convenção é que os modelos são definidos em um único lugar e as regras de roteamento são aplicadas de forma consistente em todos os controladores.</span><span class="sxs-lookup"><span data-stu-id="f9edb-123">One advantage of convention-based routing is that templates are defined in a single place, and the routing rules are applied consistently across all controllers.</span></span> <span data-ttu-id="f9edb-124">Infelizmente, o roteamento baseado em convenção torna difícil dar suporte a determinados padrões URI que são comuns em APIs RESTful.</span><span class="sxs-lookup"><span data-stu-id="f9edb-124">Unfortunately, convention-based routing makes it hard to support certain URI patterns that are common in RESTful APIs.</span></span> <span data-ttu-id="f9edb-125">Por exemplo, recursos geralmente contêm recursos filho: os clientes tiverem pedidos, filmes tem atores, têm os autores de livros e assim por diante.</span><span class="sxs-lookup"><span data-stu-id="f9edb-125">For example, resources often contain child resources: Customers have orders, movies have actors, books have authors, and so forth.</span></span> <span data-ttu-id="f9edb-126">É natural para criar URIs que refletem essas relações:</span><span class="sxs-lookup"><span data-stu-id="f9edb-126">It's natural to create URIs that reflect these relations:</span></span>

`/customers/1/orders`

<span data-ttu-id="f9edb-127">Esse tipo de URI é difícil criar usando o roteamento baseado em convenção.</span><span class="sxs-lookup"><span data-stu-id="f9edb-127">This type of URI is difficult to create using convention-based routing.</span></span> <span data-ttu-id="f9edb-128">Embora possa ser feito, os resultados não escalam bem se você tiver muitos controladores ou tipos de recursos.</span><span class="sxs-lookup"><span data-stu-id="f9edb-128">Although it can be done, the results don't scale well if you have many controllers or resource types.</span></span>

<span data-ttu-id="f9edb-129">Com o roteamento de atributo, é trivial para definir uma rota para esse URI.</span><span class="sxs-lookup"><span data-stu-id="f9edb-129">With attribute routing, it's trivial to define a route for this URI.</span></span> <span data-ttu-id="f9edb-130">Você simplesmente adicionar um atributo para a ação do controlador:</span><span class="sxs-lookup"><span data-stu-id="f9edb-130">You simply add an attribute to the controller action:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample1.cs)]

<span data-ttu-id="f9edb-131">Aqui estão alguns outros padrões que atributo faz roteamento fácil.</span><span class="sxs-lookup"><span data-stu-id="f9edb-131">Here are some other patterns that attribute routing makes easy.</span></span>

<span data-ttu-id="f9edb-132">**Controle de versão de API**</span><span class="sxs-lookup"><span data-stu-id="f9edb-132">**API versioning**</span></span>

<span data-ttu-id="f9edb-133">Neste exemplo, "/ produtos/api/v1" seria roteado para um controlador diferente que "/ v2/api/produtos".</span><span class="sxs-lookup"><span data-stu-id="f9edb-133">In this example, "/api/v1/products" would be routed to a different controller than "/api/v2/products".</span></span>

`/api/v1/products`  
`/api/v2/products`

<span data-ttu-id="f9edb-134">**Segmentos de URI sobrecarregados**</span><span class="sxs-lookup"><span data-stu-id="f9edb-134">**Overloaded URI segments**</span></span>

<span data-ttu-id="f9edb-135">Neste exemplo, "1" é um número de pedido, mas mapeia "pendente" para uma coleção.</span><span class="sxs-lookup"><span data-stu-id="f9edb-135">In this example, "1" is an order number, but "pending" maps to a collection.</span></span>

`/orders/1`  
`/orders/pending`

<span data-ttu-id="f9edb-136">**Vários tipos de parâmetro**</span><span class="sxs-lookup"><span data-stu-id="f9edb-136">**Multiple parameter types**</span></span>

<span data-ttu-id="f9edb-137">Neste exemplo, "1" é um número de pedido, mas "2013/06/16" Especifica uma data.</span><span class="sxs-lookup"><span data-stu-id="f9edb-137">In this example, "1" is an order number, but "2013/06/16" specifies a date.</span></span>

`/orders/1`  
`/orders/2013/06/16`

<a id="enable"></a>
## <a name="enabling-attribute-routing"></a><span data-ttu-id="f9edb-138">Habilitar o roteamento de atributo</span><span class="sxs-lookup"><span data-stu-id="f9edb-138">Enabling Attribute Routing</span></span>

<span data-ttu-id="f9edb-139">Para habilitar o roteamento de atributo, chame **MapHttpAttributeRoutes** durante a configuração.</span><span class="sxs-lookup"><span data-stu-id="f9edb-139">To enable attribute routing, call **MapHttpAttributeRoutes** during configuration.</span></span> <span data-ttu-id="f9edb-140">Esse método de extensão é definido na **System.Web.Http.HttpConfigurationExtensions** classe.</span><span class="sxs-lookup"><span data-stu-id="f9edb-140">This extension method is defined in the **System.Web.Http.HttpConfigurationExtensions** class.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample2.cs)]

<span data-ttu-id="f9edb-141">Roteamento de atributo pode ser combinado com [baseado em convenção](routing-in-aspnet-web-api.md) roteamento.</span><span class="sxs-lookup"><span data-stu-id="f9edb-141">Attribute routing can be combined with [convention-based](routing-in-aspnet-web-api.md) routing.</span></span> <span data-ttu-id="f9edb-142">Para definir rotas baseado em convenção, chame o **MapHttpRoute** método.</span><span class="sxs-lookup"><span data-stu-id="f9edb-142">To define convention-based routes, call the **MapHttpRoute** method.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample3.cs)]

<span data-ttu-id="f9edb-143">Para obter mais informações sobre como configurar a API da Web, consulte [Configurando o ASP.NET Web API 2](../advanced/configuring-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="f9edb-143">For more information about configuring Web API, see [Configuring ASP.NET Web API 2](../advanced/configuring-aspnet-web-api.md).</span></span>

<a id="config"></a>
### <a name="note-migrating-from-web-api-1"></a><span data-ttu-id="f9edb-144">Observação: Migrando da API Web 1</span><span class="sxs-lookup"><span data-stu-id="f9edb-144">Note: Migrating From Web API 1</span></span>

<span data-ttu-id="f9edb-145">Antes da API Web 2, os modelos de projeto de API da Web gerado código como este:</span><span class="sxs-lookup"><span data-stu-id="f9edb-145">Prior to Web API 2, the Web API project templates generated code like this:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample4.cs)]

<span data-ttu-id="f9edb-146">Se o roteamento de atributo estiver habilitado, esse código lançará uma exceção.</span><span class="sxs-lookup"><span data-stu-id="f9edb-146">If attribute routing is enabled, this code will throw an exception.</span></span> <span data-ttu-id="f9edb-147">Se você atualizar um projeto de API da Web existente para usar o roteamento de atributo, certifique-se atualizar esse código de configuração para o seguinte:</span><span class="sxs-lookup"><span data-stu-id="f9edb-147">If you upgrade an existing Web API project to use attribute routing, make sure to update this configuration code to the following:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample5.cs?highlight=4)]

> [!NOTE]
> <span data-ttu-id="f9edb-148">Para obter mais informações, consulte [Configurando a API da Web com ASP.NET de hospedagem](../advanced/configuring-aspnet-web-api.md#webhost).</span><span class="sxs-lookup"><span data-stu-id="f9edb-148">For more information, see [Configuring Web API with ASP.NET Hosting](../advanced/configuring-aspnet-web-api.md#webhost).</span></span>


<a id="add-routes"></a>
## <a name="adding-route-attributes"></a><span data-ttu-id="f9edb-149">Adicionando atributos de rota</span><span class="sxs-lookup"><span data-stu-id="f9edb-149">Adding Route Attributes</span></span>

<span data-ttu-id="f9edb-150">Aqui está um exemplo de uma rota definida usando um atributo:</span><span class="sxs-lookup"><span data-stu-id="f9edb-150">Here is an example of a route defined using an attribute:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample6.cs)]

<span data-ttu-id="f9edb-151">A cadeia de caracteres &quot;clientes / {customerId} / ordena&quot; é o modelo de URI para a rota.</span><span class="sxs-lookup"><span data-stu-id="f9edb-151">The string &quot;customers/{customerId}/orders&quot; is the URI template for the route.</span></span> <span data-ttu-id="f9edb-152">API da Web tenta corresponder o URI de solicitação para o modelo.</span><span class="sxs-lookup"><span data-stu-id="f9edb-152">Web API tries to match the request URI to the template.</span></span> <span data-ttu-id="f9edb-153">Neste exemplo, "clientes" e "orders" são segmentos literais e "{customerId}" é um parâmetro de variável.</span><span class="sxs-lookup"><span data-stu-id="f9edb-153">In this example, "customers" and "orders" are literal segments, and "{customerId}" is a variable parameter.</span></span> <span data-ttu-id="f9edb-154">Os URIs a seguir corresponderia a este modelo:</span><span class="sxs-lookup"><span data-stu-id="f9edb-154">The following URIs would match this template:</span></span>

- `http://localhost/customers/1/orders`
- `http://localhost/customers/bob/orders`
- `http://localhost/customers/1234-5678/orders`

<span data-ttu-id="f9edb-155">Você pode restringir a correspondência usando [restrições](#constraints), descrita posteriormente neste tópico.</span><span class="sxs-lookup"><span data-stu-id="f9edb-155">You can restrict the matching by using [constraints](#constraints), described later in this topic.</span></span>

<span data-ttu-id="f9edb-156">Observe que o &quot;{customerId}&quot; parâmetro no modelo de rota corresponde ao nome da *customerId* parâmetro do método.</span><span class="sxs-lookup"><span data-stu-id="f9edb-156">Notice that the &quot;{customerId}&quot; parameter in the route template matches the name of the *customerId* parameter in the method.</span></span> <span data-ttu-id="f9edb-157">Quando a API da Web invoca a ação do controlador, ele tenta associar os parâmetros de rota.</span><span class="sxs-lookup"><span data-stu-id="f9edb-157">When Web API invokes the controller action, it tries to bind the route parameters.</span></span> <span data-ttu-id="f9edb-158">Por exemplo, se o URI é `http://example.com/customers/1/orders`, API da Web tenta associar o valor "1" para o *customerId* parâmetro na ação.</span><span class="sxs-lookup"><span data-stu-id="f9edb-158">For example, if the URI is `http://example.com/customers/1/orders`, Web API tries to bind the value "1" to the *customerId* parameter in the action.</span></span>

<span data-ttu-id="f9edb-159">Um modelo de URI pode ter vários parâmetros:</span><span class="sxs-lookup"><span data-stu-id="f9edb-159">A URI template can have several parameters:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample7.cs)]

<span data-ttu-id="f9edb-160">Qualquer método de controlador que não têm um atributo de rota usa roteamento baseado em convenção.</span><span class="sxs-lookup"><span data-stu-id="f9edb-160">Any controller methods that do not have a route attribute use convention-based routing.</span></span> <span data-ttu-id="f9edb-161">Dessa forma, você pode combinar os dois tipos de roteamento no mesmo projeto.</span><span class="sxs-lookup"><span data-stu-id="f9edb-161">That way, you can combine both types of routing in the same project.</span></span>

## <a name="http-methods"></a><span data-ttu-id="f9edb-162">Métodos HTTP</span><span class="sxs-lookup"><span data-stu-id="f9edb-162">HTTP Methods</span></span>

<span data-ttu-id="f9edb-163">API da Web também seleciona ações com base no método HTTP da solicitação (GET, POST, etc.).</span><span class="sxs-lookup"><span data-stu-id="f9edb-163">Web API also selects actions based on the HTTP method of the request (GET, POST, etc).</span></span> <span data-ttu-id="f9edb-164">Por padrão, a API da Web procura uma correspondência que diferencia maiusculas de minúsculas com o início do nome do método do controlador.</span><span class="sxs-lookup"><span data-stu-id="f9edb-164">By default, Web API looks for a case-insensitive match with the start of the controller method name.</span></span> <span data-ttu-id="f9edb-165">Por exemplo, um método de controlador chamado `PutCustomers` corresponde a uma solicitação HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="f9edb-165">For example, a controller method named `PutCustomers` matches an HTTP PUT request.</span></span>

<span data-ttu-id="f9edb-166">Você pode substituir essa convenção, decorar o método com qualquer os seguintes atributos:</span><span class="sxs-lookup"><span data-stu-id="f9edb-166">You can override this convention by decorating the method with any the following attributes:</span></span>

- <span data-ttu-id="f9edb-167">**[HttpDelete]**</span><span class="sxs-lookup"><span data-stu-id="f9edb-167">**[HttpDelete]**</span></span>
- <span data-ttu-id="f9edb-168">**[HttpGet]**</span><span class="sxs-lookup"><span data-stu-id="f9edb-168">**[HttpGet]**</span></span>
- <span data-ttu-id="f9edb-169">**[HttpHead]**</span><span class="sxs-lookup"><span data-stu-id="f9edb-169">**[HttpHead]**</span></span>
- <span data-ttu-id="f9edb-170">**[HttpOptions]**</span><span class="sxs-lookup"><span data-stu-id="f9edb-170">**[HttpOptions]**</span></span>
- <span data-ttu-id="f9edb-171">**[HttpPatch]**</span><span class="sxs-lookup"><span data-stu-id="f9edb-171">**[HttpPatch]**</span></span>
- <span data-ttu-id="f9edb-172">**[HttpPost]**</span><span class="sxs-lookup"><span data-stu-id="f9edb-172">**[HttpPost]**</span></span>
- <span data-ttu-id="f9edb-173">**[HttpPut]**</span><span class="sxs-lookup"><span data-stu-id="f9edb-173">**[HttpPut]**</span></span>

<span data-ttu-id="f9edb-174">O exemplo a seguir mapeia o método CreateBook para solicitações HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="f9edb-174">The following example maps the CreateBook method to HTTP POST requests.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample8.cs)]

<span data-ttu-id="f9edb-175">Para todos os outros métodos HTTP, incluindo métodos não padrão, usem o **AcceptVerbs** atributo, que utiliza uma lista de métodos HTTP.</span><span class="sxs-lookup"><span data-stu-id="f9edb-175">For all other HTTP methods, including non-standard methods, use the **AcceptVerbs** attribute, which takes a list of HTTP methods.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample9.cs)]

<a id="prefixes"></a>
## <a name="route-prefixes"></a><span data-ttu-id="f9edb-176">Prefixos de rota</span><span class="sxs-lookup"><span data-stu-id="f9edb-176">Route Prefixes</span></span>

<span data-ttu-id="f9edb-177">Muitas vezes, as rotas em um controlador todas começam com o mesmo prefixo.</span><span class="sxs-lookup"><span data-stu-id="f9edb-177">Often, the routes in a controller all start with the same prefix.</span></span> <span data-ttu-id="f9edb-178">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="f9edb-178">For example:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample10.cs)]

<span data-ttu-id="f9edb-179">Você pode definir um prefixo comum para um controlador inteiro usando o **[RoutePrefix]** atributo:</span><span class="sxs-lookup"><span data-stu-id="f9edb-179">You can set a common prefix for an entire controller by using the **[RoutePrefix]** attribute:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample11.cs)]

<span data-ttu-id="f9edb-180">Use um til (~) no atributo de método para substituir o prefixo da rota:</span><span class="sxs-lookup"><span data-stu-id="f9edb-180">Use a tilde (~) on the method attribute to override the route prefix:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample12.cs)]

<span data-ttu-id="f9edb-181">O prefixo da rota pode incluir parâmetros:</span><span class="sxs-lookup"><span data-stu-id="f9edb-181">The route prefix can include parameters:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample13.cs)]

<a id="constraints"></a>
## <a name="route-constraints"></a><span data-ttu-id="f9edb-182">Restrições de rota</span><span class="sxs-lookup"><span data-stu-id="f9edb-182">Route Constraints</span></span>

<span data-ttu-id="f9edb-183">Restrições da rota permitem restringir como os parâmetros no modelo de rota são correspondidos.</span><span class="sxs-lookup"><span data-stu-id="f9edb-183">Route constraints let you restrict how the parameters in the route template are matched.</span></span> <span data-ttu-id="f9edb-184">A sintaxe geral é &quot;{: restrição de parâmetro}&quot;.</span><span class="sxs-lookup"><span data-stu-id="f9edb-184">The general syntax is &quot;{parameter:constraint}&quot;.</span></span> <span data-ttu-id="f9edb-185">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="f9edb-185">For example:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample14.cs)]

<span data-ttu-id="f9edb-186">Aqui, a primeira rota só será selecionada se o &quot;id&quot; segmento do URI é um inteiro.</span><span class="sxs-lookup"><span data-stu-id="f9edb-186">Here, the first route will only be selected if the &quot;id&quot; segment of the URI is an integer.</span></span> <span data-ttu-id="f9edb-187">Caso contrário, a segunda rota será escolhida.</span><span class="sxs-lookup"><span data-stu-id="f9edb-187">Otherwise, the second route will be chosen.</span></span>

<span data-ttu-id="f9edb-188">A tabela a seguir lista as restrições que são suportadas.</span><span class="sxs-lookup"><span data-stu-id="f9edb-188">The following table lists the constraints that are supported.</span></span>

| <span data-ttu-id="f9edb-189">Restrição</span><span class="sxs-lookup"><span data-stu-id="f9edb-189">Constraint</span></span> | <span data-ttu-id="f9edb-190">Descrição</span><span class="sxs-lookup"><span data-stu-id="f9edb-190">Description</span></span> | <span data-ttu-id="f9edb-191">Exemplo</span><span class="sxs-lookup"><span data-stu-id="f9edb-191">Example</span></span> |
| --- | --- | --- |
| <span data-ttu-id="f9edb-192">alfa</span><span class="sxs-lookup"><span data-stu-id="f9edb-192">alpha</span></span> | <span data-ttu-id="f9edb-193">Correspondências de maiusculas ou minúsculas (a-z, A-Z) de caracteres do alfabeto latino</span><span class="sxs-lookup"><span data-stu-id="f9edb-193">Matches uppercase or lowercase Latin alphabet characters (a-z, A-Z)</span></span> | <span data-ttu-id="f9edb-194">{x:alpha}</span><span class="sxs-lookup"><span data-stu-id="f9edb-194">{x:alpha}</span></span> |
| <span data-ttu-id="f9edb-195">bool</span><span class="sxs-lookup"><span data-stu-id="f9edb-195">bool</span></span> | <span data-ttu-id="f9edb-196">Corresponde a um valor booliano.</span><span class="sxs-lookup"><span data-stu-id="f9edb-196">Matches a Boolean value.</span></span> | <span data-ttu-id="f9edb-197">{x: bool}</span><span class="sxs-lookup"><span data-stu-id="f9edb-197">{x:bool}</span></span> |
| <span data-ttu-id="f9edb-198">datetime</span><span class="sxs-lookup"><span data-stu-id="f9edb-198">datetime</span></span> | <span data-ttu-id="f9edb-199">Correspondências de uma **DateTime** valor.</span><span class="sxs-lookup"><span data-stu-id="f9edb-199">Matches a **DateTime** value.</span></span> | <span data-ttu-id="f9edb-200">{x:datetime}</span><span class="sxs-lookup"><span data-stu-id="f9edb-200">{x:datetime}</span></span> |
| <span data-ttu-id="f9edb-201">decimal</span><span class="sxs-lookup"><span data-stu-id="f9edb-201">decimal</span></span> | <span data-ttu-id="f9edb-202">Corresponde a um valor decimal.</span><span class="sxs-lookup"><span data-stu-id="f9edb-202">Matches a decimal value.</span></span> | <span data-ttu-id="f9edb-203">{x:decimal}</span><span class="sxs-lookup"><span data-stu-id="f9edb-203">{x:decimal}</span></span> |
| <span data-ttu-id="f9edb-204">double</span><span class="sxs-lookup"><span data-stu-id="f9edb-204">double</span></span> | <span data-ttu-id="f9edb-205">Corresponde a um valor de ponto flutuante de 64 bits.</span><span class="sxs-lookup"><span data-stu-id="f9edb-205">Matches a 64-bit floating-point value.</span></span> | <span data-ttu-id="f9edb-206">{x: duplo}</span><span class="sxs-lookup"><span data-stu-id="f9edb-206">{x:double}</span></span> |
| <span data-ttu-id="f9edb-207">float</span><span class="sxs-lookup"><span data-stu-id="f9edb-207">float</span></span> | <span data-ttu-id="f9edb-208">Corresponde a um valor de ponto flutuante de 32 bits.</span><span class="sxs-lookup"><span data-stu-id="f9edb-208">Matches a 32-bit floating-point value.</span></span> | <span data-ttu-id="f9edb-209">{x:float}</span><span class="sxs-lookup"><span data-stu-id="f9edb-209">{x:float}</span></span> |
| <span data-ttu-id="f9edb-210">GUID</span><span class="sxs-lookup"><span data-stu-id="f9edb-210">guid</span></span> | <span data-ttu-id="f9edb-211">Corresponde a um valor de GUID.</span><span class="sxs-lookup"><span data-stu-id="f9edb-211">Matches a GUID value.</span></span> | <span data-ttu-id="f9edb-212">{x:guid}</span><span class="sxs-lookup"><span data-stu-id="f9edb-212">{x:guid}</span></span> |
| <span data-ttu-id="f9edb-213">int</span><span class="sxs-lookup"><span data-stu-id="f9edb-213">int</span></span> | <span data-ttu-id="f9edb-214">Corresponde a um valor inteiro de 32 bits.</span><span class="sxs-lookup"><span data-stu-id="f9edb-214">Matches a 32-bit integer value.</span></span> | <span data-ttu-id="f9edb-215">{x:int}</span><span class="sxs-lookup"><span data-stu-id="f9edb-215">{x:int}</span></span> |
| <span data-ttu-id="f9edb-216">length</span><span class="sxs-lookup"><span data-stu-id="f9edb-216">length</span></span> | <span data-ttu-id="f9edb-217">Corresponde a uma cadeia de caracteres com o comprimento especificado ou dentro de um intervalo de comprimentos especificado.</span><span class="sxs-lookup"><span data-stu-id="f9edb-217">Matches a string with the specified length or within a specified range of lengths.</span></span> | <span data-ttu-id="f9edb-218">{x: length(6)} {x: length(1,20)}</span><span class="sxs-lookup"><span data-stu-id="f9edb-218">{x:length(6)} {x:length(1,20)}</span></span> |
| <span data-ttu-id="f9edb-219">long</span><span class="sxs-lookup"><span data-stu-id="f9edb-219">long</span></span> | <span data-ttu-id="f9edb-220">Corresponde a um valor inteiro de 64 bits.</span><span class="sxs-lookup"><span data-stu-id="f9edb-220">Matches a 64-bit integer value.</span></span> | <span data-ttu-id="f9edb-221">{x:long}</span><span class="sxs-lookup"><span data-stu-id="f9edb-221">{x:long}</span></span> |
| <span data-ttu-id="f9edb-222">max</span><span class="sxs-lookup"><span data-stu-id="f9edb-222">max</span></span> | <span data-ttu-id="f9edb-223">Corresponde a um inteiro com um valor máximo.</span><span class="sxs-lookup"><span data-stu-id="f9edb-223">Matches an integer with a maximum value.</span></span> | <span data-ttu-id="f9edb-224">{x:max(10)}</span><span class="sxs-lookup"><span data-stu-id="f9edb-224">{x:max(10)}</span></span> |
| <span data-ttu-id="f9edb-225">MaxLength</span><span class="sxs-lookup"><span data-stu-id="f9edb-225">maxlength</span></span> | <span data-ttu-id="f9edb-226">Corresponde a uma cadeia de caracteres com um comprimento máximo.</span><span class="sxs-lookup"><span data-stu-id="f9edb-226">Matches a string with a maximum length.</span></span> | <span data-ttu-id="f9edb-227">{x:maxlength(10)}</span><span class="sxs-lookup"><span data-stu-id="f9edb-227">{x:maxlength(10)}</span></span> |
| <span data-ttu-id="f9edb-228">min</span><span class="sxs-lookup"><span data-stu-id="f9edb-228">min</span></span> | <span data-ttu-id="f9edb-229">Corresponde a um inteiro com um valor mínimo.</span><span class="sxs-lookup"><span data-stu-id="f9edb-229">Matches an integer with a minimum value.</span></span> | <span data-ttu-id="f9edb-230">{x:min(10)}</span><span class="sxs-lookup"><span data-stu-id="f9edb-230">{x:min(10)}</span></span> |
| <span data-ttu-id="f9edb-231">minLength</span><span class="sxs-lookup"><span data-stu-id="f9edb-231">minlength</span></span> | <span data-ttu-id="f9edb-232">Corresponde a uma cadeia de caracteres com um comprimento mínimo.</span><span class="sxs-lookup"><span data-stu-id="f9edb-232">Matches a string with a minimum length.</span></span> | <span data-ttu-id="f9edb-233">{x:minlength(10)}</span><span class="sxs-lookup"><span data-stu-id="f9edb-233">{x:minlength(10)}</span></span> |
| <span data-ttu-id="f9edb-234">range</span><span class="sxs-lookup"><span data-stu-id="f9edb-234">range</span></span> | <span data-ttu-id="f9edb-235">Corresponde a um número inteiro dentro de um intervalo de valores.</span><span class="sxs-lookup"><span data-stu-id="f9edb-235">Matches an integer within a range of values.</span></span> | <span data-ttu-id="f9edb-236">{x:range(10,50)}</span><span class="sxs-lookup"><span data-stu-id="f9edb-236">{x:range(10,50)}</span></span> |
| <span data-ttu-id="f9edb-237">Regex</span><span class="sxs-lookup"><span data-stu-id="f9edb-237">regex</span></span> | <span data-ttu-id="f9edb-238">Corresponde a uma expressão regular.</span><span class="sxs-lookup"><span data-stu-id="f9edb-238">Matches a regular expression.</span></span> | <span data-ttu-id="f9edb-239">{x: regex(^\d{3}-\d{3}-\d{4}$)}</span><span class="sxs-lookup"><span data-stu-id="f9edb-239">{x:regex(^\d{3}-\d{3}-\d{4}$)}</span></span> |

<span data-ttu-id="f9edb-240">Observe que algumas das restrições, como &quot;min&quot;, usam argumentos entre parênteses.</span><span class="sxs-lookup"><span data-stu-id="f9edb-240">Notice that some of the constraints, such as &quot;min&quot;, take arguments in parentheses.</span></span> <span data-ttu-id="f9edb-241">Você pode aplicar várias restrições a um parâmetro, separado por dois-pontos.</span><span class="sxs-lookup"><span data-stu-id="f9edb-241">You can apply multiple constraints to a parameter, separated by a colon.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample15.cs)]

### <a name="custom-route-constraints"></a><span data-ttu-id="f9edb-242">Restrições de rota personalizada</span><span class="sxs-lookup"><span data-stu-id="f9edb-242">Custom Route Constraints</span></span>

<span data-ttu-id="f9edb-243">Você pode criar restrições de rota personalizada Implementando a **IHttpRouteConstraint** interface.</span><span class="sxs-lookup"><span data-stu-id="f9edb-243">You can create custom route constraints by implementing the **IHttpRouteConstraint** interface.</span></span> <span data-ttu-id="f9edb-244">Por exemplo, a restrição a seguir restringe um parâmetro para um valor inteiro diferente de zero.</span><span class="sxs-lookup"><span data-stu-id="f9edb-244">For example, the following constraint restricts a parameter to a non-zero integer value.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample16.cs)]

<span data-ttu-id="f9edb-245">O código a seguir mostra como registrar a restrição:</span><span class="sxs-lookup"><span data-stu-id="f9edb-245">The following code shows how to register the constraint:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample17.cs)]

<span data-ttu-id="f9edb-246">Agora você pode aplicar a restrição suas rotas:</span><span class="sxs-lookup"><span data-stu-id="f9edb-246">Now you can apply the constraint in your routes:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample18.cs)]

<span data-ttu-id="f9edb-247">Você também pode substituir todo o **DefaultInlineConstraintResolver** classe implementando o **IInlineConstraintResolver** interface.</span><span class="sxs-lookup"><span data-stu-id="f9edb-247">You can also replace the entire **DefaultInlineConstraintResolver** class by implementing the **IInlineConstraintResolver** interface.</span></span> <span data-ttu-id="f9edb-248">Isso substituirá todas as restrições internas, a menos que sua implementação de **IInlineConstraintResolver** especificamente adicioná-los.</span><span class="sxs-lookup"><span data-stu-id="f9edb-248">Doing so will replace all of the built-in constraints, unless your implementation of **IInlineConstraintResolver** specifically adds them.</span></span>

<a id="optional"></a>
## <a name="optional-uri-parameters-and-default-values"></a><span data-ttu-id="f9edb-249">Parâmetros de URI opcional e os valores padrão</span><span class="sxs-lookup"><span data-stu-id="f9edb-249">Optional URI Parameters and Default Values</span></span>

<span data-ttu-id="f9edb-250">Você pode fazer um parâmetro URI opcional com a adição de um ponto de interrogação para o parâmetro de rota.</span><span class="sxs-lookup"><span data-stu-id="f9edb-250">You can make a URI parameter optional by adding a question mark to the route parameter.</span></span> <span data-ttu-id="f9edb-251">Se um parâmetro de rota é opcional, você deve definir um valor padrão para o parâmetro do método.</span><span class="sxs-lookup"><span data-stu-id="f9edb-251">If a route parameter is optional, you must define a default value for the method parameter.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample19.cs)]

<span data-ttu-id="f9edb-252">Neste exemplo, `/api/books/locale/1033` e `/api/books/locale` retornam o mesmo recurso.</span><span class="sxs-lookup"><span data-stu-id="f9edb-252">In this example, `/api/books/locale/1033` and `/api/books/locale` return the same resource.</span></span>

<span data-ttu-id="f9edb-253">Como alternativa, você pode especificar um valor padrão dentro do modelo de rota, da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="f9edb-253">Alternatively, you can specify a default value inside the route template, as follows:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample20.cs)]

<span data-ttu-id="f9edb-254">Isso é quase o mesmo do exemplo anterior, mas há uma pequena diferença de comportamento quando o valor padrão é aplicado.</span><span class="sxs-lookup"><span data-stu-id="f9edb-254">This is almost the same as the previous example, but there is a slight difference of behavior when the default value is applied.</span></span>

- <span data-ttu-id="f9edb-255">No primeiro exemplo ("{lcid?}"), o valor padrão de 1033 é atribuído diretamente para o parâmetro do método, portanto, o parâmetro terá esse valor exato.</span><span class="sxs-lookup"><span data-stu-id="f9edb-255">In the first example ("{lcid?}"), the default value of 1033 is assigned directly to the method parameter, so the parameter will have this exact value.</span></span>
- <span data-ttu-id="f9edb-256">No segundo exemplo ("{lcid = 1033}"), o valor padrão de "1033" percorre o processo de associação de modelos.</span><span class="sxs-lookup"><span data-stu-id="f9edb-256">In the second example ("{lcid=1033}"), the default value of "1033" goes through the model-binding process.</span></span> <span data-ttu-id="f9edb-257">O associador de modelo padrão será converter "1033" para o valor numérico 1033.</span><span class="sxs-lookup"><span data-stu-id="f9edb-257">The default model-binder will convert "1033" to the numeric value 1033.</span></span> <span data-ttu-id="f9edb-258">No entanto, você pode conectar um associador de modelo personalizado, que pode fazer algo diferente.</span><span class="sxs-lookup"><span data-stu-id="f9edb-258">However, you could plug in a custom model binder, which might do something different.</span></span>

<span data-ttu-id="f9edb-259">(Na maioria dos casos, a menos que você tenha associadores de modelos personalizados em seu pipeline, as duas formas será equivalentes.)</span><span class="sxs-lookup"><span data-stu-id="f9edb-259">(In most cases, unless you have custom model binders in your pipeline, the two forms will be equivalent.)</span></span>

<a id="route-names"></a>
## <a name="route-names"></a><span data-ttu-id="f9edb-260">Nomes de rota</span><span class="sxs-lookup"><span data-stu-id="f9edb-260">Route Names</span></span>

<span data-ttu-id="f9edb-261">Na API da Web, cada rota tem um nome.</span><span class="sxs-lookup"><span data-stu-id="f9edb-261">In Web API, every route has a name.</span></span> <span data-ttu-id="f9edb-262">Nomes de rota são úteis para a geração de links, para que você possa incluir um link em uma resposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="f9edb-262">Route names are useful for generating links, so that you can include a link in an HTTP response.</span></span>

<span data-ttu-id="f9edb-263">Para especificar o nome da rota, defina as **nome** propriedade do atributo.</span><span class="sxs-lookup"><span data-stu-id="f9edb-263">To specify the route name, set the **Name** property on the attribute.</span></span> <span data-ttu-id="f9edb-264">O exemplo a seguir mostra como definir o nome da rota e também como usar o nome da rota ao gerar um link.</span><span class="sxs-lookup"><span data-stu-id="f9edb-264">The following example shows how to set the route name, and also how to use the route name when generating a link.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample21.cs)]

<a id="order"></a>
## <a name="route-order"></a><span data-ttu-id="f9edb-265">Ordem de rota</span><span class="sxs-lookup"><span data-stu-id="f9edb-265">Route Order</span></span>

<span data-ttu-id="f9edb-266">Quando o framework tenta corresponder a um URI com uma rota, ele avalia as rotas em uma ordem específica.</span><span class="sxs-lookup"><span data-stu-id="f9edb-266">When the framework tries to match a URI with a route, it evaluates the routes in a particular order.</span></span> <span data-ttu-id="f9edb-267">Para especificar a ordem, defina as **RouteOrder** propriedade no atributo de rota.</span><span class="sxs-lookup"><span data-stu-id="f9edb-267">To specify the order, set the **RouteOrder** property on the route attribute.</span></span> <span data-ttu-id="f9edb-268">Valores mais baixos são avaliados primeiro.</span><span class="sxs-lookup"><span data-stu-id="f9edb-268">Lower values are evaluated first.</span></span> <span data-ttu-id="f9edb-269">O valor de ordem padrão é zero.</span><span class="sxs-lookup"><span data-stu-id="f9edb-269">The default order value is zero.</span></span>

<span data-ttu-id="f9edb-270">Aqui está como a ordenação total é determinada:</span><span class="sxs-lookup"><span data-stu-id="f9edb-270">Here is how the total ordering is determined:</span></span>

1. <span data-ttu-id="f9edb-271">Comparar as **RouteOrder** propriedade do atributo de rota.</span><span class="sxs-lookup"><span data-stu-id="f9edb-271">Compare the **RouteOrder** property of the route attribute.</span></span>
2. <span data-ttu-id="f9edb-272">Examinar cada segmento do URI no modelo de rota.</span><span class="sxs-lookup"><span data-stu-id="f9edb-272">Look at each URI segment in the route template.</span></span> <span data-ttu-id="f9edb-273">Para cada segmento, ordem da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="f9edb-273">For each segment, order as follows:</span></span> 

    1. <span data-ttu-id="f9edb-274">Segmentos de literais.</span><span class="sxs-lookup"><span data-stu-id="f9edb-274">Literal segments.</span></span>
    2. <span data-ttu-id="f9edb-275">Parâmetros com restrições de rota.</span><span class="sxs-lookup"><span data-stu-id="f9edb-275">Route parameters with constraints.</span></span>
    3. <span data-ttu-id="f9edb-276">Parâmetros de rota sem restrições.</span><span class="sxs-lookup"><span data-stu-id="f9edb-276">Route parameters without constraints.</span></span>
    4. <span data-ttu-id="f9edb-277">Segmentos de parâmetro de curinga com restrições.</span><span class="sxs-lookup"><span data-stu-id="f9edb-277">Wildcard parameter segments with constraints.</span></span>
    5. <span data-ttu-id="f9edb-278">Curinga segmentos de parâmetro sem restrições.</span><span class="sxs-lookup"><span data-stu-id="f9edb-278">Wildcard parameter segments without constraints.</span></span>
3. <span data-ttu-id="f9edb-279">No caso de empate, as rotas são ordenadas por uma comparação de cadeia de caracteres ordinais diferencia maiusculas de minúsculas ([OrdinalIgnoreCase](https://msdn.microsoft.com/library/system.stringcomparer.ordinalignorecase.aspx)) do modelo de rota.</span><span class="sxs-lookup"><span data-stu-id="f9edb-279">In the case of a tie, routes are ordered by a case-insensitive ordinal string comparison ([OrdinalIgnoreCase](https://msdn.microsoft.com/library/system.stringcomparer.ordinalignorecase.aspx)) of the route template.</span></span>

<span data-ttu-id="f9edb-280">Vejamos um exemplo.</span><span class="sxs-lookup"><span data-stu-id="f9edb-280">Here is an example.</span></span> <span data-ttu-id="f9edb-281">Suponha que você definir o controlador a seguir:</span><span class="sxs-lookup"><span data-stu-id="f9edb-281">Suppose you define the following controller:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample22.cs)]

<span data-ttu-id="f9edb-282">Essas rotas são ordenadas da seguinte maneira.</span><span class="sxs-lookup"><span data-stu-id="f9edb-282">These routes are ordered as follows.</span></span>

1. <span data-ttu-id="f9edb-283">detalhes dos pedidos /</span><span class="sxs-lookup"><span data-stu-id="f9edb-283">orders/details</span></span>
2. <span data-ttu-id="f9edb-284">pedidos / {id}</span><span class="sxs-lookup"><span data-stu-id="f9edb-284">orders/{id}</span></span>
3. <span data-ttu-id="f9edb-285">orders/{customerName}</span><span class="sxs-lookup"><span data-stu-id="f9edb-285">orders/{customerName}</span></span>
4. <span data-ttu-id="f9edb-286">pedidos / {\*data}</span><span class="sxs-lookup"><span data-stu-id="f9edb-286">orders/{\*date}</span></span>
5. <span data-ttu-id="f9edb-287">pedidos / pendente</span><span class="sxs-lookup"><span data-stu-id="f9edb-287">orders/pending</span></span>

<span data-ttu-id="f9edb-288">Observe que "Detalhes" é um segmento literal e aparece antes de "{id}", mas "pendente" será exibida pela última vez porque o **RouteOrder** propriedade é 1.</span><span class="sxs-lookup"><span data-stu-id="f9edb-288">Notice that "details" is a literal segment and appears before "{id}", but "pending" appears last because the **RouteOrder** property is 1.</span></span> <span data-ttu-id="f9edb-289">(Este exemplo assume que há é nenhum cliente chamada "Detalhes" ou "pendente".</span><span class="sxs-lookup"><span data-stu-id="f9edb-289">(This example assumes there are no customers named "details" or "pending".</span></span> <span data-ttu-id="f9edb-290">Em geral, tente evitar rotas ambíguas.</span><span class="sxs-lookup"><span data-stu-id="f9edb-290">In general, try to avoid ambiguous routes.</span></span> <span data-ttu-id="f9edb-291">Neste exemplo, um modelo de rota melhor para `GetByCustomer` é "clientes / {customerName}")</span><span class="sxs-lookup"><span data-stu-id="f9edb-291">In this example, a better route template for `GetByCustomer` is "customers/{customerName}" )</span></span>

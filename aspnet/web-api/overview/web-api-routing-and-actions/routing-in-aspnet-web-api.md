---
uid: web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
title: Roteamento na API da Web ASP.NET | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/11/2012
ms.topic: article
ms.assetid: 0675bdc7-282f-4f47-b7f3-7e02133940ca
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: aa0ecc96029051fef6a81ac08f7fcf52a24de59c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="routing-in-aspnet-web-api"></a><span data-ttu-id="fc52f-102">Roteamento na API da Web do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="fc52f-102">Routing in ASP.NET Web API</span></span>
====================
<span data-ttu-id="fc52f-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="fc52f-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="fc52f-104">Este artigo descreve como o ASP.NET Web API encaminha solicitações HTTP para os controladores.</span><span class="sxs-lookup"><span data-stu-id="fc52f-104">This article describes how ASP.NET Web API routes HTTP requests to controllers.</span></span>

> [!NOTE]
> <span data-ttu-id="fc52f-105">Se você estiver familiarizado com o ASP.NET MVC, roteamento API Web é muito semelhante ao roteamento MVC.</span><span class="sxs-lookup"><span data-stu-id="fc52f-105">If you are familiar with ASP.NET MVC, Web API routing is very similar to MVC routing.</span></span> <span data-ttu-id="fc52f-106">A principal diferença é que a API da Web usa o método HTTP, não o caminho URI, para selecionar a ação.</span><span class="sxs-lookup"><span data-stu-id="fc52f-106">The main difference is that Web API uses the HTTP method, not the URI path, to select the action.</span></span> <span data-ttu-id="fc52f-107">Você também pode usar o estilo MVC roteamento na API da Web.</span><span class="sxs-lookup"><span data-stu-id="fc52f-107">You can also use MVC-style routing in Web API.</span></span> <span data-ttu-id="fc52f-108">Este artigo não assume qualquer conhecimento de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="fc52f-108">This article does not assume any knowledge of ASP.NET MVC.</span></span>


## <a name="routing-tables"></a><span data-ttu-id="fc52f-109">Tabelas de roteamento</span><span class="sxs-lookup"><span data-stu-id="fc52f-109">Routing Tables</span></span>

<span data-ttu-id="fc52f-110">Na API da Web do ASP.NET, um *controlador* é uma classe que trata as solicitações HTTP.</span><span class="sxs-lookup"><span data-stu-id="fc52f-110">In ASP.NET Web API, a *controller* is a class that handles HTTP requests.</span></span> <span data-ttu-id="fc52f-111">Os métodos públicos do controlador são chamados *métodos de ação* ou simplesmente *ações*.</span><span class="sxs-lookup"><span data-stu-id="fc52f-111">The public methods of the controller are called *action methods* or simply *actions*.</span></span> <span data-ttu-id="fc52f-112">Quando a estrutura da API Web recebe uma solicitação, ele encaminha a solicitação para uma ação.</span><span class="sxs-lookup"><span data-stu-id="fc52f-112">When the Web API framework receives a request, it routes the request to an action.</span></span>

<span data-ttu-id="fc52f-113">Para determinar qual ação a ser invocada, a estrutura usa um *tabela de roteamento*.</span><span class="sxs-lookup"><span data-stu-id="fc52f-113">To determine which action to invoke, the framework uses a *routing table*.</span></span> <span data-ttu-id="fc52f-114">O modelo de projeto do Visual Studio para API da Web cria uma rota padrão:</span><span class="sxs-lookup"><span data-stu-id="fc52f-114">The Visual Studio project template for Web API creates a default route:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="fc52f-115">Essa rota é definida no arquivo WebApiConfig.cs, que é colocado no aplicativo\_diretório de início:</span><span class="sxs-lookup"><span data-stu-id="fc52f-115">This route is defined in the WebApiConfig.cs file, which is placed in the App\_Start directory:</span></span>

![](routing-in-aspnet-web-api/_static/image1.png)

<span data-ttu-id="fc52f-116">Para obter mais informações sobre o **WebApiConfig** de classe, consulte [Configurando ASP.NET Web API](../advanced/configuring-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="fc52f-116">For more information about the **WebApiConfig** class, see [Configuring ASP.NET Web API](../advanced/configuring-aspnet-web-api.md).</span></span>

<span data-ttu-id="fc52f-117">Se você hospedar o API da Web, você deve definir a tabela de roteamento diretamente o **HttpSelfHostConfiguration** objeto.</span><span class="sxs-lookup"><span data-stu-id="fc52f-117">If you self-host Web API, you must set the routing table directly on the **HttpSelfHostConfiguration** object.</span></span> <span data-ttu-id="fc52f-118">Para obter mais informações, consulte [auto-host uma API da Web](../older-versions/self-host-a-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="fc52f-118">For more information, see [Self-Host a Web API](../older-versions/self-host-a-web-api.md).</span></span>

<span data-ttu-id="fc52f-119">Cada entrada na tabela de roteamento contém um *modelo de rota*.</span><span class="sxs-lookup"><span data-stu-id="fc52f-119">Each entry in the routing table contains a *route template*.</span></span> <span data-ttu-id="fc52f-120">O modelo de rota padrão para a API da Web é &quot;api / {controller} / {id}&quot;.</span><span class="sxs-lookup"><span data-stu-id="fc52f-120">The default route template for Web API is &quot;api/{controller}/{id}&quot;.</span></span> <span data-ttu-id="fc52f-121">Neste modelo, &quot;api&quot; é um segmento de caminho literal e {controlador} e {id} são variáveis de espaço reservado.</span><span class="sxs-lookup"><span data-stu-id="fc52f-121">In this template, &quot;api&quot; is a literal path segment, and {controller} and {id} are placeholder variables.</span></span>

<span data-ttu-id="fc52f-122">Quando a estrutura da API Web recebe uma solicitação HTTP, ele tenta corresponder o URI em relação a um dos modelos de rota na tabela de roteamento.</span><span class="sxs-lookup"><span data-stu-id="fc52f-122">When the Web API framework receives an HTTP request, it tries to match the URI against one of the route templates in the routing table.</span></span> <span data-ttu-id="fc52f-123">Se nenhuma rota corresponde, o cliente recebe um erro 404.</span><span class="sxs-lookup"><span data-stu-id="fc52f-123">If no route matches, the client receives a 404 error.</span></span> <span data-ttu-id="fc52f-124">Por exemplo, os URIs a seguir correspondem a rota padrão:</span><span class="sxs-lookup"><span data-stu-id="fc52f-124">For example, the following URIs match the default route:</span></span>

- <span data-ttu-id="fc52f-125">api/contatos</span><span class="sxs-lookup"><span data-stu-id="fc52f-125">/api/contacts</span></span>
- <span data-ttu-id="fc52f-126">/API/Contacts/1</span><span class="sxs-lookup"><span data-stu-id="fc52f-126">/api/contacts/1</span></span>
- <span data-ttu-id="fc52f-127">/API/Products/gizmo1</span><span class="sxs-lookup"><span data-stu-id="fc52f-127">/api/products/gizmo1</span></span>

<span data-ttu-id="fc52f-128">No entanto, o seguinte URI não corresponde, porque ele não tem o &quot;api&quot; segmento:</span><span class="sxs-lookup"><span data-stu-id="fc52f-128">However, the following URI does not match, because it lacks the &quot;api&quot; segment:</span></span>

- <span data-ttu-id="fc52f-129">Contatos/1</span><span class="sxs-lookup"><span data-stu-id="fc52f-129">/contacts/1</span></span>

> [!NOTE]
> <span data-ttu-id="fc52f-130">O motivo para usar "api" na rota é evitar colisões com o roteamento do ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="fc52f-130">The reason for using "api" in the route is to avoid collisions with ASP.NET MVC routing.</span></span> <span data-ttu-id="fc52f-131">Dessa forma, você pode ter &quot;/entra em contato com&quot; acesse um controlador MVC, e &quot;/api/contatos&quot; acesse um controlador Web API.</span><span class="sxs-lookup"><span data-stu-id="fc52f-131">That way, you can have &quot;/contacts&quot; go to an MVC controller, and &quot;/api/contacts&quot; go to a Web API controller.</span></span> <span data-ttu-id="fc52f-132">É claro que, se não desejar que essa convenção, você pode alterar a tabela de rota padrão.</span><span class="sxs-lookup"><span data-stu-id="fc52f-132">Of course, if you don't like this convention, you can change the default route table.</span></span>

<span data-ttu-id="fc52f-133">Depois que uma rota correspondente for encontrada, a API da Web seleciona o controlador e a ação:</span><span class="sxs-lookup"><span data-stu-id="fc52f-133">Once a matching route is found, Web API selects the controller and the action:</span></span>

- <span data-ttu-id="fc52f-134">Para localizar o controlador, a API da Web adiciona &quot;controlador&quot; para o valor da *{controlador}* variável.</span><span class="sxs-lookup"><span data-stu-id="fc52f-134">To find the controller, Web API adds &quot;Controller&quot; to the value of the *{controller}* variable.</span></span>
- <span data-ttu-id="fc52f-135">Para encontrar a ação, API da Web examina o método HTTP e, em seguida, procura por uma ação cujo nome começa com esse nome de método HTTP.</span><span class="sxs-lookup"><span data-stu-id="fc52f-135">To find the action, Web API looks at the HTTP method, and then looks for an action whose name begins with that HTTP method name.</span></span> <span data-ttu-id="fc52f-136">Por exemplo, API da Web com uma solicitação GET, procura por uma ação que inicia com &quot;obter... &quot;, como &quot;GetContact&quot; ou &quot;GetAllContacts&quot;.</span><span class="sxs-lookup"><span data-stu-id="fc52f-136">For example, with a GET request, Web API looks for an action that starts with &quot;Get...&quot;, such as &quot;GetContact&quot; or &quot;GetAllContacts&quot;.</span></span> <span data-ttu-id="fc52f-137">Essa convenção se aplica apenas às GET, POST, PUT e DELETE métodos.</span><span class="sxs-lookup"><span data-stu-id="fc52f-137">This convention applies only to GET, POST, PUT, and DELETE methods.</span></span> <span data-ttu-id="fc52f-138">Você pode habilitar outros métodos HTTP por meio de atributos em seu controlador.</span><span class="sxs-lookup"><span data-stu-id="fc52f-138">You can enable other HTTP methods by using attributes on your controller.</span></span> <span data-ttu-id="fc52f-139">Veremos um exemplo disso posteriormente.</span><span class="sxs-lookup"><span data-stu-id="fc52f-139">We'll see an example of that later.</span></span>
- <span data-ttu-id="fc52f-140">Outras variáveis de espaço reservado no modelo de rota, como *{id},* são mapeados para parâmetros de ação.</span><span class="sxs-lookup"><span data-stu-id="fc52f-140">Other placeholder variables in the route template, such as *{id},* are mapped to action parameters.</span></span>

<span data-ttu-id="fc52f-141">Vejamos um exemplo.</span><span class="sxs-lookup"><span data-stu-id="fc52f-141">Let's look at an example.</span></span> <span data-ttu-id="fc52f-142">Suponha que você defina o controlador a seguir:</span><span class="sxs-lookup"><span data-stu-id="fc52f-142">Suppose that you define the following controller:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample2.cs)]

<span data-ttu-id="fc52f-143">Aqui estão algumas solicitações HTTP possíveis, junto com a ação que é invocada para cada um:</span><span class="sxs-lookup"><span data-stu-id="fc52f-143">Here are some possible HTTP requests, along with the action that gets invoked for each:</span></span>

| <span data-ttu-id="fc52f-144">Método HTTP</span><span class="sxs-lookup"><span data-stu-id="fc52f-144">HTTP Method</span></span> | <span data-ttu-id="fc52f-145">Caminho de URI</span><span class="sxs-lookup"><span data-stu-id="fc52f-145">URI Path</span></span> | <span data-ttu-id="fc52f-146">Ação</span><span class="sxs-lookup"><span data-stu-id="fc52f-146">Action</span></span> | <span data-ttu-id="fc52f-147">Parâmetro</span><span class="sxs-lookup"><span data-stu-id="fc52f-147">Parameter</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="fc52f-148">OBTER</span><span class="sxs-lookup"><span data-stu-id="fc52f-148">GET</span></span> | <span data-ttu-id="fc52f-149">API e produtos</span><span class="sxs-lookup"><span data-stu-id="fc52f-149">api/products</span></span> | <span data-ttu-id="fc52f-150">GetAllProducts</span><span class="sxs-lookup"><span data-stu-id="fc52f-150">GetAllProducts</span></span> | <span data-ttu-id="fc52f-151">*(nenhum)*</span><span class="sxs-lookup"><span data-stu-id="fc52f-151">*(none)*</span></span> |
| <span data-ttu-id="fc52f-152">OBTER</span><span class="sxs-lookup"><span data-stu-id="fc52f-152">GET</span></span> | <span data-ttu-id="fc52f-153">produtos/API/4</span><span class="sxs-lookup"><span data-stu-id="fc52f-153">api/products/4</span></span> | <span data-ttu-id="fc52f-154">GetProductById</span><span class="sxs-lookup"><span data-stu-id="fc52f-154">GetProductById</span></span> | <span data-ttu-id="fc52f-155">4</span><span class="sxs-lookup"><span data-stu-id="fc52f-155">4</span></span> |
| <span data-ttu-id="fc52f-156">DELETE</span><span class="sxs-lookup"><span data-stu-id="fc52f-156">DELETE</span></span> | <span data-ttu-id="fc52f-157">produtos/API/4</span><span class="sxs-lookup"><span data-stu-id="fc52f-157">api/products/4</span></span> | <span data-ttu-id="fc52f-158">DeleteProduct</span><span class="sxs-lookup"><span data-stu-id="fc52f-158">DeleteProduct</span></span> | <span data-ttu-id="fc52f-159">4</span><span class="sxs-lookup"><span data-stu-id="fc52f-159">4</span></span> |
| <span data-ttu-id="fc52f-160">POSTAR</span><span class="sxs-lookup"><span data-stu-id="fc52f-160">POST</span></span> | <span data-ttu-id="fc52f-161">API e produtos</span><span class="sxs-lookup"><span data-stu-id="fc52f-161">api/products</span></span> | <span data-ttu-id="fc52f-162">*(nenhuma correspondência)*</span><span class="sxs-lookup"><span data-stu-id="fc52f-162">*(no match)*</span></span> |  |

<span data-ttu-id="fc52f-163">Observe que o *{id}* segmento do URI, se presente, é mapeado para o *id* parâmetro da ação.</span><span class="sxs-lookup"><span data-stu-id="fc52f-163">Notice that the *{id}* segment of the URI, if present, is mapped to the *id* parameter of the action.</span></span> <span data-ttu-id="fc52f-164">Neste exemplo, o controlador define dois métodos GET, um com um *id* parâmetro e um sem parâmetros.</span><span class="sxs-lookup"><span data-stu-id="fc52f-164">In this example, the controller defines two GET methods, one with an *id* parameter and one with no parameters.</span></span>

<span data-ttu-id="fc52f-165">Além disso, observe que a solicitação POST falhará, pois o controlador não define uma &quot;Post... &quot; método.</span><span class="sxs-lookup"><span data-stu-id="fc52f-165">Also, note that the POST request will fail, because the controller does not define a &quot;Post...&quot; method.</span></span>

## <a name="routing-variations"></a><span data-ttu-id="fc52f-166">Variações de roteamentos</span><span class="sxs-lookup"><span data-stu-id="fc52f-166">Routing Variations</span></span>

<span data-ttu-id="fc52f-167">A seção anterior descreveu o mecanismo de roteamento básico para API Web do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="fc52f-167">The previous section described the basic routing mechanism for ASP.NET Web API.</span></span> <span data-ttu-id="fc52f-168">Esta seção descreve algumas variações.</span><span class="sxs-lookup"><span data-stu-id="fc52f-168">This section describes some variations.</span></span>

### <a name="http-methods"></a><span data-ttu-id="fc52f-169">Métodos HTTP</span><span class="sxs-lookup"><span data-stu-id="fc52f-169">HTTP Methods</span></span>

<span data-ttu-id="fc52f-170">Em vez de usar a convenção de nomenclatura para métodos HTTP, você pode especificar explicitamente o método HTTP para uma ação por decorar o método de ação com o **HttpGet**, **HttpPut**, **HttpPost** , ou **HttpDelete** atributo.</span><span class="sxs-lookup"><span data-stu-id="fc52f-170">Instead of using the naming convention for HTTP methods, you can explicitly specify the HTTP method for an action by decorating the action method with the **HttpGet**, **HttpPut**, **HttpPost**, or **HttpDelete** attribute.</span></span>

<span data-ttu-id="fc52f-171">No exemplo a seguir, o método FindProduct é mapeado para solicitações GET:</span><span class="sxs-lookup"><span data-stu-id="fc52f-171">In the following example, the FindProduct method is mapped to GET requests:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="fc52f-172">Para permitir que vários métodos HTTP para uma ação, ou para permitir que os métodos HTTP diferente de GET, PUT, POST e DELETE, use o **AcceptVerbs** atributo, que usa uma lista de métodos HTTP.</span><span class="sxs-lookup"><span data-stu-id="fc52f-172">To allow multiple HTTP methods for an action, or to allow HTTP methods other than GET, PUT, POST, and DELETE, use the **AcceptVerbs** attribute, which takes a list of HTTP methods.</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample4.cs)]

<a id="routing_by_action_name"></a>
### <a name="routing-by-action-name"></a><span data-ttu-id="fc52f-173">Roteamento pelo nome da ação</span><span class="sxs-lookup"><span data-stu-id="fc52f-173">Routing by Action Name</span></span>

<span data-ttu-id="fc52f-174">Com o modelo de roteamento padrão, a API da Web usa o método HTTP para selecionar a ação.</span><span class="sxs-lookup"><span data-stu-id="fc52f-174">With the default routing template, Web API uses the HTTP method to select the action.</span></span> <span data-ttu-id="fc52f-175">No entanto, você também pode criar uma rota onde o nome da ação é incluído no URI:</span><span class="sxs-lookup"><span data-stu-id="fc52f-175">However, you can also create a route where the action name is included in the URI:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample5.cs)]

<span data-ttu-id="fc52f-176">Nesse modelo de rota, o *{action}* nomes de parâmetro do método de ação no controlador.</span><span class="sxs-lookup"><span data-stu-id="fc52f-176">In this route template, the *{action}* parameter names the action method on the controller.</span></span> <span data-ttu-id="fc52f-177">Com esse estilo de roteamento, use atributos para especificar os métodos HTTP permitidos.</span><span class="sxs-lookup"><span data-stu-id="fc52f-177">With this style of routing, use attributes to specify the allowed HTTP methods.</span></span> <span data-ttu-id="fc52f-178">Por exemplo, suponha que o controlador tem o seguinte método:</span><span class="sxs-lookup"><span data-stu-id="fc52f-178">For example, suppose your controller has the following method:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample6.cs)]

<span data-ttu-id="fc52f-179">Nesse caso, uma solicitação GET para a "api/produtos/detalhes/1" seria mapeados para o método de detalhes.</span><span class="sxs-lookup"><span data-stu-id="fc52f-179">In this case, a GET request for "api/products/details/1" would map to the Details method.</span></span> <span data-ttu-id="fc52f-180">Esse estilo de roteamento é semelhante ao ASP.NET MVC e pode ser apropriado para uma API de estilo RPC.</span><span class="sxs-lookup"><span data-stu-id="fc52f-180">This style of routing is similar to ASP.NET MVC, and may be appropriate for an RPC-style API.</span></span>

<span data-ttu-id="fc52f-181">Você pode substituir o nome da ação usando o **ActionName** atributo.</span><span class="sxs-lookup"><span data-stu-id="fc52f-181">You can override the action name by using the **ActionName** attribute.</span></span> <span data-ttu-id="fc52f-182">No exemplo a seguir, há duas ações que são mapeados para &quot;produtos/api/miniatura/*id*. Um suporta GET e o outro suporta POST:</span><span class="sxs-lookup"><span data-stu-id="fc52f-182">In the following example, there are two actions that map to &quot;api/products/thumbnail/*id*. One supports GET and the other supports POST:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample7.cs)]

### <a name="non-actions"></a><span data-ttu-id="fc52f-183">Ações não</span><span class="sxs-lookup"><span data-stu-id="fc52f-183">Non-Actions</span></span>

<span data-ttu-id="fc52f-184">Para impedir que um método sendo invocada como uma ação, use o **NonAction** atributo.</span><span class="sxs-lookup"><span data-stu-id="fc52f-184">To prevent a method from getting invoked as an action, use the **NonAction** attribute.</span></span> <span data-ttu-id="fc52f-185">Isso sinaliza para a estrutura que o método não é uma ação, mesmo que ele tenha correspondência as regras de roteamento.</span><span class="sxs-lookup"><span data-stu-id="fc52f-185">This signals to the framework that the method is not an action, even if it would otherwise match the routing rules.</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample8.cs)]

## <a name="further-reading"></a><span data-ttu-id="fc52f-186">Leitura adicional</span><span class="sxs-lookup"><span data-stu-id="fc52f-186">Further Reading</span></span>

<span data-ttu-id="fc52f-187">Este tópico forneceu uma visão geral do roteamento.</span><span class="sxs-lookup"><span data-stu-id="fc52f-187">This topic provided a high-level view of routing.</span></span> <span data-ttu-id="fc52f-188">Para obter mais detalhes, consulte [de roteamento e seleção de ação](routing-and-action-selection.md), que descreve exatamente como o framework corresponde a um URI para uma rota, seleciona um controlador e, em seguida, seleciona a ação a ser invocada.</span><span class="sxs-lookup"><span data-stu-id="fc52f-188">For more detail, see [Routing and Action Selection](routing-and-action-selection.md), which describes exactly how the framework matches a URI to a route, selects a controller, and then selects the action to invoke.</span></span>

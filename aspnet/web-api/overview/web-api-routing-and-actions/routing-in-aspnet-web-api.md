---
uid: web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
title: Roteamento na API Web ASP.NET | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/11/2012
ms.topic: article
ms.assetid: 0675bdc7-282f-4f47-b7f3-7e02133940ca
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 5e3bba70993dafcdd93feed52813ee80697b1038
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37374199"
---
<a name="routing-in-aspnet-web-api"></a><span data-ttu-id="ebfeb-102">Roteamento na API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="ebfeb-102">Routing in ASP.NET Web API</span></span>
====================
<span data-ttu-id="ebfeb-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="ebfeb-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="ebfeb-104">Este artigo descreve como o API Web ASP.NET encaminha solicitações HTTP para controladores.</span><span class="sxs-lookup"><span data-stu-id="ebfeb-104">This article describes how ASP.NET Web API routes HTTP requests to controllers.</span></span>

> [!NOTE]
> <span data-ttu-id="ebfeb-105">Se você estiver familiarizado com o ASP.NET MVC, roteamento API Web é muito semelhante ao roteamento MVC.</span><span class="sxs-lookup"><span data-stu-id="ebfeb-105">If you are familiar with ASP.NET MVC, Web API routing is very similar to MVC routing.</span></span> <span data-ttu-id="ebfeb-106">A principal diferença é que a API Web usa o método HTTP, não o caminho URI, para selecionar a ação.</span><span class="sxs-lookup"><span data-stu-id="ebfeb-106">The main difference is that Web API uses the HTTP method, not the URI path, to select the action.</span></span> <span data-ttu-id="ebfeb-107">Você também pode usar o roteamento na API Web no estilo MVC.</span><span class="sxs-lookup"><span data-stu-id="ebfeb-107">You can also use MVC-style routing in Web API.</span></span> <span data-ttu-id="ebfeb-108">Este artigo não assume nenhum conhecimento do ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="ebfeb-108">This article does not assume any knowledge of ASP.NET MVC.</span></span>


## <a name="routing-tables"></a><span data-ttu-id="ebfeb-109">Tabelas de roteamento</span><span class="sxs-lookup"><span data-stu-id="ebfeb-109">Routing Tables</span></span>

<span data-ttu-id="ebfeb-110">Na API Web ASP.NET, uma *controlador* é uma classe que manipula as solicitações HTTP.</span><span class="sxs-lookup"><span data-stu-id="ebfeb-110">In ASP.NET Web API, a *controller* is a class that handles HTTP requests.</span></span> <span data-ttu-id="ebfeb-111">Os métodos públicos do controlador são chamados *métodos de ação* ou simplesmente *ações*.</span><span class="sxs-lookup"><span data-stu-id="ebfeb-111">The public methods of the controller are called *action methods* or simply *actions*.</span></span> <span data-ttu-id="ebfeb-112">Quando a estrutura da API Web recebe uma solicitação, ele encaminha a solicitação para uma ação.</span><span class="sxs-lookup"><span data-stu-id="ebfeb-112">When the Web API framework receives a request, it routes the request to an action.</span></span>

<span data-ttu-id="ebfeb-113">Para determinar qual ação a ser invocada, a estrutura usa um *tabela de roteamento*.</span><span class="sxs-lookup"><span data-stu-id="ebfeb-113">To determine which action to invoke, the framework uses a *routing table*.</span></span> <span data-ttu-id="ebfeb-114">O modelo de projeto do Visual Studio para API da Web cria uma rota padrão:</span><span class="sxs-lookup"><span data-stu-id="ebfeb-114">The Visual Studio project template for Web API creates a default route:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="ebfeb-115">Essa rota é definida no arquivo WebApiConfig.cs, que é colocado no aplicativo\_diretório inicial:</span><span class="sxs-lookup"><span data-stu-id="ebfeb-115">This route is defined in the WebApiConfig.cs file, which is placed in the App\_Start directory:</span></span>

![](routing-in-aspnet-web-api/_static/image1.png)

<span data-ttu-id="ebfeb-116">Para obter mais informações sobre o **WebApiConfig** classe, consulte [Configurando o ASP.NET Web API](../advanced/configuring-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="ebfeb-116">For more information about the **WebApiConfig** class, see [Configuring ASP.NET Web API](../advanced/configuring-aspnet-web-api.md).</span></span>

<span data-ttu-id="ebfeb-117">Se você hospedar internamente o API da Web, você deve definir a tabela de roteamento diretamente na **HttpSelfHostConfiguration** objeto.</span><span class="sxs-lookup"><span data-stu-id="ebfeb-117">If you self-host Web API, you must set the routing table directly on the **HttpSelfHostConfiguration** object.</span></span> <span data-ttu-id="ebfeb-118">Para obter mais informações, consulte [hospedar internamente uma API Web](../older-versions/self-host-a-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="ebfeb-118">For more information, see [Self-Host a Web API](../older-versions/self-host-a-web-api.md).</span></span>

<span data-ttu-id="ebfeb-119">Cada entrada na tabela de roteamento contém um *modelo de rota*.</span><span class="sxs-lookup"><span data-stu-id="ebfeb-119">Each entry in the routing table contains a *route template*.</span></span> <span data-ttu-id="ebfeb-120">É o modelo de rota padrão para a API da Web &quot;api / {controller} / {id}&quot;.</span><span class="sxs-lookup"><span data-stu-id="ebfeb-120">The default route template for Web API is &quot;api/{controller}/{id}&quot;.</span></span> <span data-ttu-id="ebfeb-121">Neste modelo, &quot;api&quot; é um segmento de caminho literal e {controlador} e {id} são variáveis de espaço reservado.</span><span class="sxs-lookup"><span data-stu-id="ebfeb-121">In this template, &quot;api&quot; is a literal path segment, and {controller} and {id} are placeholder variables.</span></span>

<span data-ttu-id="ebfeb-122">Quando a estrutura da API Web recebe uma solicitação HTTP, ele tenta corresponder o URI em relação a um dos modelos de rota na tabela de roteamento.</span><span class="sxs-lookup"><span data-stu-id="ebfeb-122">When the Web API framework receives an HTTP request, it tries to match the URI against one of the route templates in the routing table.</span></span> <span data-ttu-id="ebfeb-123">Se nenhuma rota corresponde, o cliente recebe um erro 404.</span><span class="sxs-lookup"><span data-stu-id="ebfeb-123">If no route matches, the client receives a 404 error.</span></span> <span data-ttu-id="ebfeb-124">Por exemplo, os URIs a seguir corresponde a rota padrão:</span><span class="sxs-lookup"><span data-stu-id="ebfeb-124">For example, the following URIs match the default route:</span></span>

- <span data-ttu-id="ebfeb-125">/ api/contatos</span><span class="sxs-lookup"><span data-stu-id="ebfeb-125">/api/contacts</span></span>
- <span data-ttu-id="ebfeb-126">/API/Contacts/1</span><span class="sxs-lookup"><span data-stu-id="ebfeb-126">/api/contacts/1</span></span>
- <span data-ttu-id="ebfeb-127">/API/Products/gizmo1</span><span class="sxs-lookup"><span data-stu-id="ebfeb-127">/api/products/gizmo1</span></span>

<span data-ttu-id="ebfeb-128">No entanto, o seguinte URI não corresponde, porque ele não tem o &quot;api&quot; segmento:</span><span class="sxs-lookup"><span data-stu-id="ebfeb-128">However, the following URI does not match, because it lacks the &quot;api&quot; segment:</span></span>

- <span data-ttu-id="ebfeb-129">/ Contatos/1</span><span class="sxs-lookup"><span data-stu-id="ebfeb-129">/contacts/1</span></span>

> [!NOTE]
> <span data-ttu-id="ebfeb-130">O motivo para usar "api" na rota é evitar colisões com o roteamento do ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="ebfeb-130">The reason for using "api" in the route is to avoid collisions with ASP.NET MVC routing.</span></span> <span data-ttu-id="ebfeb-131">Dessa forma, você pode ter &quot;/entra em contato com&quot; vá para um controlador MVC, e &quot;/api/contatos&quot; vá para um controlador de API da Web.</span><span class="sxs-lookup"><span data-stu-id="ebfeb-131">That way, you can have &quot;/contacts&quot; go to an MVC controller, and &quot;/api/contacts&quot; go to a Web API controller.</span></span> <span data-ttu-id="ebfeb-132">É claro que, se você não gostar essa convenção, você pode alterar a tabela de rotas padrão.</span><span class="sxs-lookup"><span data-stu-id="ebfeb-132">Of course, if you don't like this convention, you can change the default route table.</span></span>

<span data-ttu-id="ebfeb-133">Depois que uma rota correspondente for encontrada, a API da Web seleciona o controlador e a ação:</span><span class="sxs-lookup"><span data-stu-id="ebfeb-133">Once a matching route is found, Web API selects the controller and the action:</span></span>

- <span data-ttu-id="ebfeb-134">Para localizar o controlador, a API da Web adiciona &quot;controlador&quot; para o valor da *{controlador}* variável.</span><span class="sxs-lookup"><span data-stu-id="ebfeb-134">To find the controller, Web API adds &quot;Controller&quot; to the value of the *{controller}* variable.</span></span>
- <span data-ttu-id="ebfeb-135">Para localizar a ação, a API da Web examina o método HTTP e, em seguida, procura por uma ação cujo nome começa com esse nome de método HTTP.</span><span class="sxs-lookup"><span data-stu-id="ebfeb-135">To find the action, Web API looks at the HTTP method, and then looks for an action whose name begins with that HTTP method name.</span></span> <span data-ttu-id="ebfeb-136">Por exemplo, API da Web com uma solicitação GET, procura por uma ação que inicia com &quot;obter... &quot;, como &quot;GetContact&quot; ou &quot;GetAllContacts&quot;.</span><span class="sxs-lookup"><span data-stu-id="ebfeb-136">For example, with a GET request, Web API looks for an action that starts with &quot;Get...&quot;, such as &quot;GetContact&quot; or &quot;GetAllContacts&quot;.</span></span> <span data-ttu-id="ebfeb-137">Essa convenção aplica-se somente para GET, POST, PUT e DELETE métodos.</span><span class="sxs-lookup"><span data-stu-id="ebfeb-137">This convention applies only to GET, POST, PUT, and DELETE methods.</span></span> <span data-ttu-id="ebfeb-138">Você pode habilitar outros métodos HTTP por meio de atributos em seu controlador.</span><span class="sxs-lookup"><span data-stu-id="ebfeb-138">You can enable other HTTP methods by using attributes on your controller.</span></span> <span data-ttu-id="ebfeb-139">Veremos um exemplo disso mais tarde.</span><span class="sxs-lookup"><span data-stu-id="ebfeb-139">We'll see an example of that later.</span></span>
- <span data-ttu-id="ebfeb-140">Outras variáveis de espaço reservado no modelo de rota, como *{id},* são mapeados para parâmetros de ação.</span><span class="sxs-lookup"><span data-stu-id="ebfeb-140">Other placeholder variables in the route template, such as *{id},* are mapped to action parameters.</span></span>

<span data-ttu-id="ebfeb-141">Vamos examinar um exemplo.</span><span class="sxs-lookup"><span data-stu-id="ebfeb-141">Let's look at an example.</span></span> <span data-ttu-id="ebfeb-142">Suponha que você defina o controlador a seguir:</span><span class="sxs-lookup"><span data-stu-id="ebfeb-142">Suppose that you define the following controller:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample2.cs)]

<span data-ttu-id="ebfeb-143">Aqui estão algumas solicitações HTTP possíveis, junto com a ação que é invocado para cada:</span><span class="sxs-lookup"><span data-stu-id="ebfeb-143">Here are some possible HTTP requests, along with the action that gets invoked for each:</span></span>

| <span data-ttu-id="ebfeb-144">Método HTTP</span><span class="sxs-lookup"><span data-stu-id="ebfeb-144">HTTP Method</span></span> | <span data-ttu-id="ebfeb-145">Caminho de URI</span><span class="sxs-lookup"><span data-stu-id="ebfeb-145">URI Path</span></span> | <span data-ttu-id="ebfeb-146">Ação</span><span class="sxs-lookup"><span data-stu-id="ebfeb-146">Action</span></span> | <span data-ttu-id="ebfeb-147">Parâmetro</span><span class="sxs-lookup"><span data-stu-id="ebfeb-147">Parameter</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="ebfeb-148">OBTER</span><span class="sxs-lookup"><span data-stu-id="ebfeb-148">GET</span></span> | <span data-ttu-id="ebfeb-149">API/produtos</span><span class="sxs-lookup"><span data-stu-id="ebfeb-149">api/products</span></span> | <span data-ttu-id="ebfeb-150">GetAllProducts</span><span class="sxs-lookup"><span data-stu-id="ebfeb-150">GetAllProducts</span></span> | <span data-ttu-id="ebfeb-151">*(nenhum)*</span><span class="sxs-lookup"><span data-stu-id="ebfeb-151">*(none)*</span></span> |
| <span data-ttu-id="ebfeb-152">OBTER</span><span class="sxs-lookup"><span data-stu-id="ebfeb-152">GET</span></span> | <span data-ttu-id="ebfeb-153">produtos/API/4</span><span class="sxs-lookup"><span data-stu-id="ebfeb-153">api/products/4</span></span> | <span data-ttu-id="ebfeb-154">GetProductById</span><span class="sxs-lookup"><span data-stu-id="ebfeb-154">GetProductById</span></span> | <span data-ttu-id="ebfeb-155">4</span><span class="sxs-lookup"><span data-stu-id="ebfeb-155">4</span></span> |
| <span data-ttu-id="ebfeb-156">DELETE</span><span class="sxs-lookup"><span data-stu-id="ebfeb-156">DELETE</span></span> | <span data-ttu-id="ebfeb-157">produtos/API/4</span><span class="sxs-lookup"><span data-stu-id="ebfeb-157">api/products/4</span></span> | <span data-ttu-id="ebfeb-158">DeleteProduct</span><span class="sxs-lookup"><span data-stu-id="ebfeb-158">DeleteProduct</span></span> | <span data-ttu-id="ebfeb-159">4</span><span class="sxs-lookup"><span data-stu-id="ebfeb-159">4</span></span> |
| <span data-ttu-id="ebfeb-160">POSTAR</span><span class="sxs-lookup"><span data-stu-id="ebfeb-160">POST</span></span> | <span data-ttu-id="ebfeb-161">API/produtos</span><span class="sxs-lookup"><span data-stu-id="ebfeb-161">api/products</span></span> | <span data-ttu-id="ebfeb-162">*(nenhuma correspondência)*</span><span class="sxs-lookup"><span data-stu-id="ebfeb-162">*(no match)*</span></span> |  |

<span data-ttu-id="ebfeb-163">Observe que o *{id}* segmento do URI, se presente, é mapeado para o *id* parâmetro da ação.</span><span class="sxs-lookup"><span data-stu-id="ebfeb-163">Notice that the *{id}* segment of the URI, if present, is mapped to the *id* parameter of the action.</span></span> <span data-ttu-id="ebfeb-164">Neste exemplo, o controlador define dois métodos GET com um *id* parâmetro e um sem parâmetros.</span><span class="sxs-lookup"><span data-stu-id="ebfeb-164">In this example, the controller defines two GET methods, one with an *id* parameter and one with no parameters.</span></span>

<span data-ttu-id="ebfeb-165">Além disso, observe que a solicitação POST falhará, pois o controlador não define uma &quot;Post... &quot; método.</span><span class="sxs-lookup"><span data-stu-id="ebfeb-165">Also, note that the POST request will fail, because the controller does not define a &quot;Post...&quot; method.</span></span>

## <a name="routing-variations"></a><span data-ttu-id="ebfeb-166">Variações de roteamentos</span><span class="sxs-lookup"><span data-stu-id="ebfeb-166">Routing Variations</span></span>

<span data-ttu-id="ebfeb-167">A seção anterior descreveu o mecanismo de roteamento básico de API Web do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="ebfeb-167">The previous section described the basic routing mechanism for ASP.NET Web API.</span></span> <span data-ttu-id="ebfeb-168">Esta seção descreve algumas variações.</span><span class="sxs-lookup"><span data-stu-id="ebfeb-168">This section describes some variations.</span></span>

### <a name="http-methods"></a><span data-ttu-id="ebfeb-169">Métodos HTTP</span><span class="sxs-lookup"><span data-stu-id="ebfeb-169">HTTP Methods</span></span>

<span data-ttu-id="ebfeb-170">Em vez de usar a convenção de nomenclatura para métodos HTTP, você pode especificar explicitamente o método HTTP para uma ação decorando o método de ação com o **HttpGet**, **HttpPut**, **HttpPost** , ou **HttpDelete** atributo.</span><span class="sxs-lookup"><span data-stu-id="ebfeb-170">Instead of using the naming convention for HTTP methods, you can explicitly specify the HTTP method for an action by decorating the action method with the **HttpGet**, **HttpPut**, **HttpPost**, or **HttpDelete** attribute.</span></span>

<span data-ttu-id="ebfeb-171">No exemplo a seguir, o método FindProduct é mapeado para solicitações GET:</span><span class="sxs-lookup"><span data-stu-id="ebfeb-171">In the following example, the FindProduct method is mapped to GET requests:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="ebfeb-172">Para permitir que vários métodos HTTP para uma ação ou para permitir que os métodos HTTP diferente de GET, PUT, POST e DELETE, use o **AcceptVerbs** atributo, que utiliza uma lista de métodos HTTP.</span><span class="sxs-lookup"><span data-stu-id="ebfeb-172">To allow multiple HTTP methods for an action, or to allow HTTP methods other than GET, PUT, POST, and DELETE, use the **AcceptVerbs** attribute, which takes a list of HTTP methods.</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample4.cs)]

<a id="routing_by_action_name"></a>
### <a name="routing-by-action-name"></a><span data-ttu-id="ebfeb-173">Roteamento pelo nome da ação</span><span class="sxs-lookup"><span data-stu-id="ebfeb-173">Routing by Action Name</span></span>

<span data-ttu-id="ebfeb-174">Com o modelo de roteamento padrão, a API da Web usa o método HTTP para selecionar a ação.</span><span class="sxs-lookup"><span data-stu-id="ebfeb-174">With the default routing template, Web API uses the HTTP method to select the action.</span></span> <span data-ttu-id="ebfeb-175">No entanto, você também pode criar uma rota em que o nome da ação está incluído no URI:</span><span class="sxs-lookup"><span data-stu-id="ebfeb-175">However, you can also create a route where the action name is included in the URI:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample5.cs)]

<span data-ttu-id="ebfeb-176">Neste modelo de rota, o *{action}* nomes de parâmetro de método de ação no controlador.</span><span class="sxs-lookup"><span data-stu-id="ebfeb-176">In this route template, the *{action}* parameter names the action method on the controller.</span></span> <span data-ttu-id="ebfeb-177">Com esse estilo de roteamento, use atributos para especificar os métodos HTTP permitidos.</span><span class="sxs-lookup"><span data-stu-id="ebfeb-177">With this style of routing, use attributes to specify the allowed HTTP methods.</span></span> <span data-ttu-id="ebfeb-178">Por exemplo, suponha que seu controlador tem o seguinte método:</span><span class="sxs-lookup"><span data-stu-id="ebfeb-178">For example, suppose your controller has the following method:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample6.cs)]

<span data-ttu-id="ebfeb-179">Nesse caso, uma solicitação GET para "api/produtos/detalhes/1" seria mapeados para o método de detalhes.</span><span class="sxs-lookup"><span data-stu-id="ebfeb-179">In this case, a GET request for "api/products/details/1" would map to the Details method.</span></span> <span data-ttu-id="ebfeb-180">Esse estilo de roteamento é semelhante ao ASP.NET MVC e pode ser apropriado para uma API de estilo RPC.</span><span class="sxs-lookup"><span data-stu-id="ebfeb-180">This style of routing is similar to ASP.NET MVC, and may be appropriate for an RPC-style API.</span></span>

<span data-ttu-id="ebfeb-181">Você pode substituir o nome da ação usando o **ActionName** atributo.</span><span class="sxs-lookup"><span data-stu-id="ebfeb-181">You can override the action name by using the **ActionName** attribute.</span></span> <span data-ttu-id="ebfeb-182">No exemplo a seguir, há duas ações que mapeiam &quot;produtos/api/miniatura/*id*. Um oferece suporte a GET e o outro oferece suporte a POSTAGEM:</span><span class="sxs-lookup"><span data-stu-id="ebfeb-182">In the following example, there are two actions that map to &quot;api/products/thumbnail/*id*. One supports GET and the other supports POST:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample7.cs)]

### <a name="non-actions"></a><span data-ttu-id="ebfeb-183">Não ações</span><span class="sxs-lookup"><span data-stu-id="ebfeb-183">Non-Actions</span></span>

<span data-ttu-id="ebfeb-184">Para impedir que um método sendo invocada como uma ação, use o **NonAction** atributo.</span><span class="sxs-lookup"><span data-stu-id="ebfeb-184">To prevent a method from getting invoked as an action, use the **NonAction** attribute.</span></span> <span data-ttu-id="ebfeb-185">Isso sinaliza para o framework que o método não é uma ação, mesmo que ele tenha correspondência as regras de roteamento.</span><span class="sxs-lookup"><span data-stu-id="ebfeb-185">This signals to the framework that the method is not an action, even if it would otherwise match the routing rules.</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample8.cs)]

## <a name="further-reading"></a><span data-ttu-id="ebfeb-186">Leitura adicional</span><span class="sxs-lookup"><span data-stu-id="ebfeb-186">Further Reading</span></span>

<span data-ttu-id="ebfeb-187">Este tópico forneceu uma visão geral do roteamento.</span><span class="sxs-lookup"><span data-stu-id="ebfeb-187">This topic provided a high-level view of routing.</span></span> <span data-ttu-id="ebfeb-188">Para obter mais detalhes, consulte [roteamento e seleção de ação](routing-and-action-selection.md), que descreve exatamente como o framework corresponde a um URI para uma rota, seleciona um controlador e, em seguida, seleciona a ação a ser invocada.</span><span class="sxs-lookup"><span data-stu-id="ebfeb-188">For more detail, see [Routing and Action Selection](routing-and-action-selection.md), which describes exactly how the framework matches a URI to a route, selects a controller, and then selects the action to invoke.</span></span>

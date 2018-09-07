---
title: Lista segura IP do cliente para o ASP.NET Core
author: damienbod
description: Saiba como filtros de ação ou de Middleware para validar os endereços IP remotos em relação a uma lista de endereços IP aprovados de gravação.
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/31/2018
uid: security/ip-safelist
ms.openlocfilehash: 40fe7b67359efd1692490099c3fb529ba4a6148f
ms.sourcegitcommit: 08bf41d4b3e696ab512b044970e8304816f8cc56
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/06/2018
ms.locfileid: "44040102"
---
# <a name="client-ip-safelist-for-aspnet-core"></a><span data-ttu-id="9a719-103">Lista segura IP do cliente para o ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9a719-103">Client IP safelist for ASP.NET Core</span></span>

<span data-ttu-id="9a719-104">Por [Damien Bowden](https://twitter.com/damien_bod) e [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="9a719-104">By [Damien Bowden](https://twitter.com/damien_bod) and [Tom Dykstra](https://github.com/tdykstra)</span></span>
 
<span data-ttu-id="9a719-105">Este artigo mostra duas maneiras de implementar uma lista segura IP (também conhecido como uma lista de permissões):</span><span class="sxs-lookup"><span data-stu-id="9a719-105">This article shows two ways to implement an IP safelist (also known as a whitelist):</span></span>

* <span data-ttu-id="9a719-106">Usando o middleware do ASP.NET Core para verificar o endereço IP remoto de cada solicitação.</span><span class="sxs-lookup"><span data-stu-id="9a719-106">By using ASP.NET Core middleware to check the remote IP address of every request.</span></span>
* <span data-ttu-id="9a719-107">Usando filtros de ação do ASP.NET Core para verificar o endereço IP remoto de solicitações para os métodos de ação específica.</span><span class="sxs-lookup"><span data-stu-id="9a719-107">By using ASP.NET Core action filters to check the remote IP address of requests for specific action methods.</span></span>

<span data-ttu-id="9a719-108">O aplicativo de exemplo ilustra as duas abordagens.</span><span class="sxs-lookup"><span data-stu-id="9a719-108">The sample app illustrates both approaches.</span></span> <span data-ttu-id="9a719-109">Em cada caso, uma cadeia de caracteres que contém os endereços IP de cliente aprovados é armazenada em uma configuração de aplicativo.</span><span class="sxs-lookup"><span data-stu-id="9a719-109">In each case, a string containing approved client IP addresses is stored in an app setting.</span></span> <span data-ttu-id="9a719-110">O middleware ou filtro analisa a cadeia de caracteres em uma lista e verifica se o IP remoto estiver na lista.</span><span class="sxs-lookup"><span data-stu-id="9a719-110">The middleware or filter parses the string into a list and  checks if the remote IP is in the list.</span></span> <span data-ttu-id="9a719-111">Caso contrário, será retornado um código de status HTTP 403 Proibido.</span><span class="sxs-lookup"><span data-stu-id="9a719-111">If not, an HTTP 403 Forbidden status code is returned.</span></span>

<span data-ttu-id="9a719-112">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/ip-safelist/samples/2.x/ClientIpAspNetCore) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="9a719-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/ip-safelist/samples/2.x/ClientIpAspNetCore) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="the-safelist"></a><span data-ttu-id="9a719-113">A lista segura</span><span class="sxs-lookup"><span data-stu-id="9a719-113">The safelist</span></span>

<span data-ttu-id="9a719-114">A lista é configurada na *appSettings. JSON* arquivo.</span><span class="sxs-lookup"><span data-stu-id="9a719-114">The list is configured in the *appsettings.json* file.</span></span> <span data-ttu-id="9a719-115">Ele é uma lista delimitada por ponto e vírgula e pode conter endereços IPv4 e IPv6.</span><span class="sxs-lookup"><span data-stu-id="9a719-115">It's a semicolon-delimited list and can contain IPv4 and IPv6 addresses.</span></span>

[!code-json[](ip-safelist/samples/2.x/ClientIpAspNetCore/appsettings.json?highlight=2)]

## <a name="middleware"></a><span data-ttu-id="9a719-116">Middleware</span><span class="sxs-lookup"><span data-stu-id="9a719-116">Middleware</span></span>

<span data-ttu-id="9a719-117">O `Configure` método adiciona o middleware e passa a cadeia de caracteres de lista segura para ele em um parâmetro de construtor.</span><span class="sxs-lookup"><span data-stu-id="9a719-117">The `Configure` method adds the middleware and passes the safelist string to it in a constructor parameter.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_Configure&highlight=7)]

<span data-ttu-id="9a719-118">O middleware analisa a cadeia de caracteres em uma matriz e procura o endereço IP remoto na matriz.</span><span class="sxs-lookup"><span data-stu-id="9a719-118">The middleware parses the string into an array and looks for the remote IP address in the array.</span></span> <span data-ttu-id="9a719-119">Se o endereço IP remoto não for encontrado, o middleware retornará HTTP 401 proibido.</span><span class="sxs-lookup"><span data-stu-id="9a719-119">If the remote IP address is not found, the middleware returns HTTP 401 Forbidden.</span></span> <span data-ttu-id="9a719-120">Esse processo de validação é ignorado para solicitações HTTP Get.</span><span class="sxs-lookup"><span data-stu-id="9a719-120">This validation process is bypassed for HTTP Get requests.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/AdminSafeListMiddleware.cs?name=snippet_ClassOnly)]

## <a name="action-filter"></a><span data-ttu-id="9a719-121">Filtro de ação</span><span class="sxs-lookup"><span data-stu-id="9a719-121">Action filter</span></span>

<span data-ttu-id="9a719-122">Se você quiser uma lista segura somente para controladores específicos ou métodos de ação, use um filtro de ação.</span><span class="sxs-lookup"><span data-stu-id="9a719-122">If you want a safelist only for specific controllers or action methods, use an action filter.</span></span> <span data-ttu-id="9a719-123">Veja um exemplo:</span><span class="sxs-lookup"><span data-stu-id="9a719-123">Here's an example:</span></span> 

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Filters/ClientIdCheckFilter.cs)]

<span data-ttu-id="9a719-124">O filtro de ação é adicionado ao contêiner de serviços.</span><span class="sxs-lookup"><span data-stu-id="9a719-124">The action filter is added to the services container.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

<span data-ttu-id="9a719-125">O filtro, em seguida, pode ser usado em um método de ação ou controlador.</span><span class="sxs-lookup"><span data-stu-id="9a719-125">The filter can then be used on a controller or action method.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Controllers/ValuesController.cs?name=snippet_Filter&highlight=1)]

<span data-ttu-id="9a719-126">No aplicativo de exemplo, o filtro é aplicado para o `Get` método.</span><span class="sxs-lookup"><span data-stu-id="9a719-126">In the sample app, the filter is applied to the `Get` method.</span></span> <span data-ttu-id="9a719-127">Portanto, quando você testar o aplicativo, enviando um `Get` solicitação de API, o atributo é validar o endereço IP do cliente.</span><span class="sxs-lookup"><span data-stu-id="9a719-127">So when you test the app by sending a `Get` API request, the attribute is validating the client IP address.</span></span> <span data-ttu-id="9a719-128">Quando você testa chamando a API com qualquer outro método HTTP, o middleware é validar o IP do cliente.</span><span class="sxs-lookup"><span data-stu-id="9a719-128">When you test by calling the API with any other HTTP method, the middleware is validating the client IP.</span></span>

## <a name="razor-pages-filter"></a><span data-ttu-id="9a719-129">Filtro de páginas do Razor</span><span class="sxs-lookup"><span data-stu-id="9a719-129">Razor Pages filter</span></span> 

<span data-ttu-id="9a719-130">Se você quiser uma lista segura para um aplicativo páginas Razor, use um filtro de páginas do Razor.</span><span class="sxs-lookup"><span data-stu-id="9a719-130">If you want a safelist for a Razor Pages app, use a Razor Pages filter.</span></span> <span data-ttu-id="9a719-131">Veja um exemplo:</span><span class="sxs-lookup"><span data-stu-id="9a719-131">Here's an example:</span></span> 

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Filters/ClientIdCheckPageFilter.cs)]

<span data-ttu-id="9a719-132">Esse filtro é habilitado adicionando-o à coleção de filtros MVC.</span><span class="sxs-lookup"><span data-stu-id="9a719-132">This filter is enabled by adding it to the MVC Filters collection.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServices&highlight=7-9)]

<span data-ttu-id="9a719-133">Quando você executa o aplicativo e solicita uma página Razor, o filtro de páginas do Razor é validar o IP do cliente.</span><span class="sxs-lookup"><span data-stu-id="9a719-133">When you run the app and request a Razor page, the Razor Pages filter is validating the client IP.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9a719-134">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="9a719-134">Next steps</span></span>

<span data-ttu-id="9a719-135">[Saiba mais sobre o Middleware do ASP.NET Core](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="9a719-135">[Learn more about ASP.NET Core Middleware](xref:fundamentals/middleware/index).</span></span>

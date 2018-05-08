---
title: Auxiliar de Marca de Cache no ASP.NET Core MVC
author: pkellner
description: Mostra como trabalhar com o Auxiliar de Marca de Cache
manager: wpickett
ms.author: riande
ms.date: 02/14/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/cache-tag-helper
ms.openlocfilehash: 6f19a989c9bdfddea7609c5571cdd49de29e036b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
# <a name="cache-tag-helper-in-aspnet-core-mvc"></a><span data-ttu-id="6430d-103">Auxiliar de Marca de Cache no ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="6430d-103">Cache Tag Helper in ASP.NET Core MVC</span></span>

<span data-ttu-id="6430d-104">Por [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="6430d-104">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="6430d-105">O Auxiliar de Marca de Cache possibilita melhorar consideravelmente o desempenho de seu aplicativo ASP.NET Core armazenando seu conteúdo em cache no provedor de cache interno do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6430d-105">The Cache Tag Helper provides the ability to dramatically improve the performance of your ASP.NET Core app by caching its content to the internal ASP.NET Core cache provider.</span></span>

<span data-ttu-id="6430d-106">O Mecanismo de Exibição do Razor define o `expires-after` padrão como vinte minutos.</span><span class="sxs-lookup"><span data-stu-id="6430d-106">The Razor View Engine sets the default `expires-after` to twenty minutes.</span></span>

<span data-ttu-id="6430d-107">A seguinte marcação Razor armazena em cache a data/hora:</span><span class="sxs-lookup"><span data-stu-id="6430d-107">The following Razor markup caches the date/time:</span></span>

```cshtml
<cache>@DateTime.Now</cache>
```

<span data-ttu-id="6430d-108">A primeira solicitação para a página que contém `CacheTagHelper` exibirá a data/hora atual.</span><span class="sxs-lookup"><span data-stu-id="6430d-108">The first request to the page that contains `CacheTagHelper` will display the current date/time.</span></span> <span data-ttu-id="6430d-109">Solicitações adicionais mostrarão o valor armazenado em cache até que o cache expire (padrão de 20 minutos) ou seja removido por demanda de memória.</span><span class="sxs-lookup"><span data-stu-id="6430d-109">Additional requests will show the cached value until the cache expires (default 20 minutes) or is evicted by memory pressure.</span></span>

<span data-ttu-id="6430d-110">Você pode definir a duração do cache com os seguintes atributos:</span><span class="sxs-lookup"><span data-stu-id="6430d-110">You can set the cache duration with the following attributes:</span></span>

## <a name="cache-tag-helper-attributes"></a><span data-ttu-id="6430d-111">Atributos do Auxiliar de Marca de Cache</span><span class="sxs-lookup"><span data-stu-id="6430d-111">Cache Tag Helper Attributes</span></span>

- - -

### <a name="enabled"></a><span data-ttu-id="6430d-112">habilitado</span><span class="sxs-lookup"><span data-stu-id="6430d-112">enabled</span></span>    


| <span data-ttu-id="6430d-113">Tipo de atributo</span><span class="sxs-lookup"><span data-stu-id="6430d-113">Attribute Type</span></span>    | <span data-ttu-id="6430d-114">Valores válidos</span><span class="sxs-lookup"><span data-stu-id="6430d-114">Valid Values</span></span>      |
|----------------   |----------------   |
| <span data-ttu-id="6430d-115">boolean</span><span class="sxs-lookup"><span data-stu-id="6430d-115">boolean</span></span>           | <span data-ttu-id="6430d-116">"true" (padrão)</span><span class="sxs-lookup"><span data-stu-id="6430d-116">"true" (default)</span></span>  |
|                   | <span data-ttu-id="6430d-117">"false"</span><span class="sxs-lookup"><span data-stu-id="6430d-117">"false"</span></span>   |


<span data-ttu-id="6430d-118">Determina se o conteúdo circundado pelo Auxiliar de Marca de Cache é armazenado em cache.</span><span class="sxs-lookup"><span data-stu-id="6430d-118">Determines whether the content enclosed by the Cache Tag Helper is cached.</span></span> <span data-ttu-id="6430d-119">O padrão é `true`.</span><span class="sxs-lookup"><span data-stu-id="6430d-119">The default is `true`.</span></span>  <span data-ttu-id="6430d-120">Se for definido como `false`, o Auxiliar de Marca de Cache não terá efeito de armazenamento em cache na saída renderizada.</span><span class="sxs-lookup"><span data-stu-id="6430d-120">If set to `false` this Cache Tag Helper will have no caching effect on the rendered output.</span></span>

<span data-ttu-id="6430d-121">Exemplo:</span><span class="sxs-lookup"><span data-stu-id="6430d-121">Example:</span></span>

```cshtml
<cache enabled="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-on"></a><span data-ttu-id="6430d-122">expires-on</span><span class="sxs-lookup"><span data-stu-id="6430d-122">expires-on</span></span> 

| <span data-ttu-id="6430d-123">Tipo de atributo</span><span class="sxs-lookup"><span data-stu-id="6430d-123">Attribute Type</span></span> |           <span data-ttu-id="6430d-124">Valor de exemplo</span><span class="sxs-lookup"><span data-stu-id="6430d-124">Example Value</span></span>            |
|----------------|------------------------------------|
| <span data-ttu-id="6430d-125">DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="6430d-125">DateTimeOffset</span></span> | <span data-ttu-id="6430d-126">"@new DateTime(2025,1,29,17,02,0)"</span><span class="sxs-lookup"><span data-stu-id="6430d-126">"@new DateTime(2025,1,29,17,02,0)"</span></span> |

<span data-ttu-id="6430d-127">Define uma data de expiração absoluta.</span><span class="sxs-lookup"><span data-stu-id="6430d-127">Sets an absolute expiration date.</span></span> <span data-ttu-id="6430d-128">O exemplo a seguir armazenará em cache o conteúdo do Auxiliar de Marca de Cache até 17:02 de 29 de janeiro de 2025.</span><span class="sxs-lookup"><span data-stu-id="6430d-128">The following example will cache the contents of the Cache Tag Helper until 5:02 PM on January 29, 2025.</span></span>

<span data-ttu-id="6430d-129">Exemplo:</span><span class="sxs-lookup"><span data-stu-id="6430d-129">Example:</span></span>

```cshtml
<cache expires-on="@new DateTime(2025,1,29,17,02,0)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-after"></a><span data-ttu-id="6430d-130">expires-after</span><span class="sxs-lookup"><span data-stu-id="6430d-130">expires-after</span></span>

| <span data-ttu-id="6430d-131">Tipo de atributo</span><span class="sxs-lookup"><span data-stu-id="6430d-131">Attribute Type</span></span> |        <span data-ttu-id="6430d-132">Valor de exemplo</span><span class="sxs-lookup"><span data-stu-id="6430d-132">Example Value</span></span>         |
|----------------|------------------------------|
|    <span data-ttu-id="6430d-133">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="6430d-133">TimeSpan</span></span>    | <span data-ttu-id="6430d-134">"@TimeSpan.FromSeconds(120)"</span><span class="sxs-lookup"><span data-stu-id="6430d-134">"@TimeSpan.FromSeconds(120)"</span></span> |

<span data-ttu-id="6430d-135">Define o tempo decorrido desde a primeira solicitação para armazenar o conteúdo em cache.</span><span class="sxs-lookup"><span data-stu-id="6430d-135">Sets the length of time from the first request time to cache the contents.</span></span> 

<span data-ttu-id="6430d-136">Exemplo:</span><span class="sxs-lookup"><span data-stu-id="6430d-136">Example:</span></span>

```cshtml
<cache expires-after="@TimeSpan.FromSeconds(120)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-sliding"></a><span data-ttu-id="6430d-137">expires-sliding</span><span class="sxs-lookup"><span data-stu-id="6430d-137">expires-sliding</span></span>

| <span data-ttu-id="6430d-138">Tipo de atributo</span><span class="sxs-lookup"><span data-stu-id="6430d-138">Attribute Type</span></span> |        <span data-ttu-id="6430d-139">Valor de exemplo</span><span class="sxs-lookup"><span data-stu-id="6430d-139">Example Value</span></span>        |
|----------------|-----------------------------|
|    <span data-ttu-id="6430d-140">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="6430d-140">TimeSpan</span></span>    | <span data-ttu-id="6430d-141">"@TimeSpan.FromSeconds(60)"</span><span class="sxs-lookup"><span data-stu-id="6430d-141">"@TimeSpan.FromSeconds(60)"</span></span> |

<span data-ttu-id="6430d-142">Define a hora em que uma entrada de cache deve ser removida se não tiver sido acessada.</span><span class="sxs-lookup"><span data-stu-id="6430d-142">Sets the time that a cache entry should be evicted if it has not been accessed.</span></span>

<span data-ttu-id="6430d-143">Exemplo:</span><span class="sxs-lookup"><span data-stu-id="6430d-143">Example:</span></span>

```cshtml
<cache expires-sliding="@TimeSpan.FromSeconds(60)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-header"></a><span data-ttu-id="6430d-144">vary-by-header</span><span class="sxs-lookup"><span data-stu-id="6430d-144">vary-by-header</span></span>

| <span data-ttu-id="6430d-145">Tipo de atributo</span><span class="sxs-lookup"><span data-stu-id="6430d-145">Attribute Type</span></span>    | <span data-ttu-id="6430d-146">Valores de exemplo</span><span class="sxs-lookup"><span data-stu-id="6430d-146">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="6430d-147">Cadeia de Caracteres</span><span class="sxs-lookup"><span data-stu-id="6430d-147">String</span></span>            | <span data-ttu-id="6430d-148">"User-Agent"</span><span class="sxs-lookup"><span data-stu-id="6430d-148">"User-Agent"</span></span>                  |
|                   | <span data-ttu-id="6430d-149">"User-Agent,content-encoding"</span><span class="sxs-lookup"><span data-stu-id="6430d-149">"User-Agent,content-encoding"</span></span> |

<span data-ttu-id="6430d-150">Aceita um único valor de cabeçalho ou uma lista separada por vírgulas de valores de cabeçalho que disparam uma atualização do cache quando são alterados.</span><span class="sxs-lookup"><span data-stu-id="6430d-150">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when they change.</span></span> <span data-ttu-id="6430d-151">O exemplo a seguir monitora o valor do cabeçalho `User-Agent`.</span><span class="sxs-lookup"><span data-stu-id="6430d-151">The following example monitors the header value `User-Agent`.</span></span> <span data-ttu-id="6430d-152">O exemplo armazenará em cache o conteúdo para todos os `User-Agent` diferentes apresentado ao servidor Web.</span><span class="sxs-lookup"><span data-stu-id="6430d-152">The example will cache the content for every different `User-Agent` presented to the web server.</span></span>

<span data-ttu-id="6430d-153">Exemplo:</span><span class="sxs-lookup"><span data-stu-id="6430d-153">Example:</span></span>

```cshtml
<cache vary-by-header="User-Agent">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-query"></a><span data-ttu-id="6430d-154">vary-by-query</span><span class="sxs-lookup"><span data-stu-id="6430d-154">vary-by-query</span></span>

| <span data-ttu-id="6430d-155">Tipo de atributo</span><span class="sxs-lookup"><span data-stu-id="6430d-155">Attribute Type</span></span>    | <span data-ttu-id="6430d-156">Valores de exemplo</span><span class="sxs-lookup"><span data-stu-id="6430d-156">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="6430d-157">Cadeia de Caracteres</span><span class="sxs-lookup"><span data-stu-id="6430d-157">String</span></span>            | <span data-ttu-id="6430d-158">"Make"</span><span class="sxs-lookup"><span data-stu-id="6430d-158">"Make"</span></span>                |
|                   | <span data-ttu-id="6430d-159">"Make,Model"</span><span class="sxs-lookup"><span data-stu-id="6430d-159">"Make,Model"</span></span> |

<span data-ttu-id="6430d-160">Aceita um único valor de cabeçalho ou uma lista separada por vírgulas de valores de cabeçalho que disparam uma atualização do cache quando o valor do cabeçalho é alterado.</span><span class="sxs-lookup"><span data-stu-id="6430d-160">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the header value changes.</span></span> <span data-ttu-id="6430d-161">O exemplo a seguir examina os valores de `Make` e `Model`.</span><span class="sxs-lookup"><span data-stu-id="6430d-161">The following example looks at the values of `Make` and `Model`.</span></span>

<span data-ttu-id="6430d-162">Exemplo:</span><span class="sxs-lookup"><span data-stu-id="6430d-162">Example:</span></span>

```cshtml
<cache vary-by-query="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-route"></a><span data-ttu-id="6430d-163">vary-by-route</span><span class="sxs-lookup"><span data-stu-id="6430d-163">vary-by-route</span></span>

| <span data-ttu-id="6430d-164">Tipo de atributo</span><span class="sxs-lookup"><span data-stu-id="6430d-164">Attribute Type</span></span>    | <span data-ttu-id="6430d-165">Valores de exemplo</span><span class="sxs-lookup"><span data-stu-id="6430d-165">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="6430d-166">Cadeia de Caracteres</span><span class="sxs-lookup"><span data-stu-id="6430d-166">String</span></span>            | <span data-ttu-id="6430d-167">"Make"</span><span class="sxs-lookup"><span data-stu-id="6430d-167">"Make"</span></span>                |
|                   | <span data-ttu-id="6430d-168">"Make,Model"</span><span class="sxs-lookup"><span data-stu-id="6430d-168">"Make,Model"</span></span> |

<span data-ttu-id="6430d-169">Aceita um único valor de cabeçalho ou uma lista separada por vírgulas de valores de cabeçalho que disparam uma atualização do cache quando os valores do parâmetro de dados de rota do cabeçalho são alterados.</span><span class="sxs-lookup"><span data-stu-id="6430d-169">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the route data parameter value(s) change.</span></span> <span data-ttu-id="6430d-170">Exemplo:</span><span class="sxs-lookup"><span data-stu-id="6430d-170">Example:</span></span>

<span data-ttu-id="6430d-171">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="6430d-171">*Startup.cs*</span></span> 

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{Make?}/{Model?}");
```

<span data-ttu-id="6430d-172">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="6430d-172">*Index.cshtml*</span></span>

```cshtml
<cache vary-by-route="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-cookie"></a><span data-ttu-id="6430d-173">vary-by-cookie</span><span class="sxs-lookup"><span data-stu-id="6430d-173">vary-by-cookie</span></span>

| <span data-ttu-id="6430d-174">Tipo de atributo</span><span class="sxs-lookup"><span data-stu-id="6430d-174">Attribute Type</span></span>    | <span data-ttu-id="6430d-175">Valores de exemplo</span><span class="sxs-lookup"><span data-stu-id="6430d-175">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="6430d-176">Cadeia de Caracteres</span><span class="sxs-lookup"><span data-stu-id="6430d-176">String</span></span>            | <span data-ttu-id="6430d-177">".AspNetCore.Identity.Application"</span><span class="sxs-lookup"><span data-stu-id="6430d-177">".AspNetCore.Identity.Application"</span></span>                |
|                   | <span data-ttu-id="6430d-178">".AspNetCore.Identity.Application,HairColor"</span><span class="sxs-lookup"><span data-stu-id="6430d-178">".AspNetCore.Identity.Application,HairColor"</span></span> |

<span data-ttu-id="6430d-179">Aceita um único valor de cabeçalho ou uma lista separada por vírgulas de valores de cabeçalho que disparam uma atualização do cache quando os valores do cabeçalho são alterados.</span><span class="sxs-lookup"><span data-stu-id="6430d-179">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the header values(s) change.</span></span> <span data-ttu-id="6430d-180">O exemplo a seguir examina o cookie associado à identidade do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="6430d-180">The following example looks at the cookie associated with ASP.NET Identity.</span></span> <span data-ttu-id="6430d-181">Quando um usuário é autenticado, o cookie de solicitação é definido, o que dispara uma atualização do cache.</span><span class="sxs-lookup"><span data-stu-id="6430d-181">When a user is authenticated the request cookie to be set which triggers a cache refresh.</span></span>

<span data-ttu-id="6430d-182">Exemplo:</span><span class="sxs-lookup"><span data-stu-id="6430d-182">Example:</span></span>

```cshtml
<cache vary-by-cookie=".AspNetCore.Identity.Application">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-user"></a><span data-ttu-id="6430d-183">vary-by-user</span><span class="sxs-lookup"><span data-stu-id="6430d-183">vary-by-user</span></span>

| <span data-ttu-id="6430d-184">Tipo de atributo</span><span class="sxs-lookup"><span data-stu-id="6430d-184">Attribute Type</span></span>    | <span data-ttu-id="6430d-185">Valores de exemplo</span><span class="sxs-lookup"><span data-stu-id="6430d-185">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="6430d-186">Boolean</span><span class="sxs-lookup"><span data-stu-id="6430d-186">Boolean</span></span>             | <span data-ttu-id="6430d-187">"true"</span><span class="sxs-lookup"><span data-stu-id="6430d-187">"true"</span></span>                  |
|                     | <span data-ttu-id="6430d-188">"false" (padrão)</span><span class="sxs-lookup"><span data-stu-id="6430d-188">"false" (default)</span></span> |

<span data-ttu-id="6430d-189">Especifica se o cache deve ou não ser redefinido quando o usuário conectado (ou a entidade de contexto) é alterado.</span><span class="sxs-lookup"><span data-stu-id="6430d-189">Specifies whether or not the cache should reset when the logged-in user (or Context Principal) changes.</span></span> <span data-ttu-id="6430d-190">O usuário atual também é conhecido como a Entidade do contexto de solicitação e pode ser exibido em um modo de exibição do Razor referenciando `@User.Identity.Name`.</span><span class="sxs-lookup"><span data-stu-id="6430d-190">The current user is also known as the Request Context Principal and can be viewed in a Razor view by referencing `@User.Identity.Name`.</span></span>

<span data-ttu-id="6430d-191">O exemplo a seguir examina o usuário conectado no momento.</span><span class="sxs-lookup"><span data-stu-id="6430d-191">The following example looks at the current logged in user.</span></span>  

<span data-ttu-id="6430d-192">Exemplo:</span><span class="sxs-lookup"><span data-stu-id="6430d-192">Example:</span></span>

```cshtml
<cache vary-by-user="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

<span data-ttu-id="6430d-193">Usar esse atributo mantém o conteúdo no cache durante um ciclo de logon e logoff.</span><span class="sxs-lookup"><span data-stu-id="6430d-193">Using this attribute maintains the contents in cache through a log-in and log-out cycle.</span></span>  <span data-ttu-id="6430d-194">Ao usar `vary-by-user="true"`, uma ação de logon e logoff invalida o cache para o usuário autenticado.</span><span class="sxs-lookup"><span data-stu-id="6430d-194">When using `vary-by-user="true"`, a log-in and log-out action invalidates the cache for the authenticated user.</span></span>  <span data-ttu-id="6430d-195">O cache é invalidado porque um novo valor de cookie exclusivo é gerado no logon.</span><span class="sxs-lookup"><span data-stu-id="6430d-195">The cache is invalidated because a new unique cookie value is generated on login.</span></span> <span data-ttu-id="6430d-196">O cache é mantido para o estado anônimo quando nenhum cookie está presente ou expirou.</span><span class="sxs-lookup"><span data-stu-id="6430d-196">Cache is maintained for the anonymous state when no cookie is present or has expired.</span></span> <span data-ttu-id="6430d-197">Isso significa que se nenhum usuário estiver conectado, o cache será mantido.</span><span class="sxs-lookup"><span data-stu-id="6430d-197">This means if no user is logged in, the cache will be maintained.</span></span>

- - -

### <a name="vary-by"></a><span data-ttu-id="6430d-198">vary-by</span><span class="sxs-lookup"><span data-stu-id="6430d-198">vary-by</span></span>

| <span data-ttu-id="6430d-199">Tipo de atributo</span><span class="sxs-lookup"><span data-stu-id="6430d-199">Attribute Type</span></span> | <span data-ttu-id="6430d-200">Valores de exemplo</span><span class="sxs-lookup"><span data-stu-id="6430d-200">Example Values</span></span> |
|----------------|----------------|
|     <span data-ttu-id="6430d-201">Cadeia de Caracteres</span><span class="sxs-lookup"><span data-stu-id="6430d-201">String</span></span>     |    <span data-ttu-id="6430d-202">"@Model"</span><span class="sxs-lookup"><span data-stu-id="6430d-202">"@Model"</span></span>    |

<span data-ttu-id="6430d-203">Permite a personalização de quais dados são armazenados no cache.</span><span class="sxs-lookup"><span data-stu-id="6430d-203">Allows for customization of what data gets cached.</span></span> <span data-ttu-id="6430d-204">Quando o objeto referenciado pelo valor de cadeia de caracteres do atributo é alterado, o conteúdo do Auxiliar de Marca de Cache é atualizado.</span><span class="sxs-lookup"><span data-stu-id="6430d-204">When the object referenced by the attribute's string value changes, the content of the Cache Tag Helper is updated.</span></span> <span data-ttu-id="6430d-205">Frequentemente, uma concatenação de cadeia de caracteres de valores do modelo é atribuída a este atributo.</span><span class="sxs-lookup"><span data-stu-id="6430d-205">Often a string-concatenation of model values are assigned to this attribute.</span></span>  <span data-ttu-id="6430d-206">Na verdade, isso significa que uma atualização de qualquer um dos valores concatenados invalida o cache.</span><span class="sxs-lookup"><span data-stu-id="6430d-206">Effectively, that means an update to any of the concatenated values invalidates the cache.</span></span>

<span data-ttu-id="6430d-207">O exemplo a seguir supõe que o método do controlador que renderiza a exibição somas o valor inteiro dos dois parâmetros de rota, `myParam1` e `myParam2`, e os retorna como a propriedade de modelo única.</span><span class="sxs-lookup"><span data-stu-id="6430d-207">The following example assumes the controller method rendering the view sums the integer value of the two route parameters, `myParam1` and `myParam2`, and returns that as the single model property.</span></span> <span data-ttu-id="6430d-208">Quando essa soma é alterada, o conteúdo do Auxiliar de Marca de Cache é renderizado e armazenado em cache novamente.</span><span class="sxs-lookup"><span data-stu-id="6430d-208">When this sum changes, the content of the Cache Tag Helper is rendered and cached again.</span></span>  

<span data-ttu-id="6430d-209">Exemplo:</span><span class="sxs-lookup"><span data-stu-id="6430d-209">Example:</span></span>

<span data-ttu-id="6430d-210">Ação:</span><span class="sxs-lookup"><span data-stu-id="6430d-210">Action:</span></span>

```csharp
public IActionResult Index(string myParam1,string myParam2,string myParam3)
{
    int num1;
    int num2;
    int.TryParse(myParam1, out num1);
    int.TryParse(myParam2, out num2);
    return View(viewName, num1 + num2);
}
```

<span data-ttu-id="6430d-211">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="6430d-211">*Index.cshtml*</span></span>

```cshtml
<cache vary-by="@Model"">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="priority"></a><span data-ttu-id="6430d-212">priority</span><span class="sxs-lookup"><span data-stu-id="6430d-212">priority</span></span>

| <span data-ttu-id="6430d-213">Tipo de atributo</span><span class="sxs-lookup"><span data-stu-id="6430d-213">Attribute Type</span></span>    | <span data-ttu-id="6430d-214">Valores de exemplo</span><span class="sxs-lookup"><span data-stu-id="6430d-214">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="6430d-215">CacheItemPriority</span><span class="sxs-lookup"><span data-stu-id="6430d-215">CacheItemPriority</span></span>  | <span data-ttu-id="6430d-216">"High"</span><span class="sxs-lookup"><span data-stu-id="6430d-216">"High"</span></span>                   |
|                    | <span data-ttu-id="6430d-217">"Low"</span><span class="sxs-lookup"><span data-stu-id="6430d-217">"Low"</span></span> |
|                    | <span data-ttu-id="6430d-218">"NeverRemove"</span><span class="sxs-lookup"><span data-stu-id="6430d-218">"NeverRemove"</span></span> |
|                    | <span data-ttu-id="6430d-219">"Normal"</span><span class="sxs-lookup"><span data-stu-id="6430d-219">"Normal"</span></span> |

<span data-ttu-id="6430d-220">Fornece diretrizes de remoção do cache para o provedor de cache interno.</span><span class="sxs-lookup"><span data-stu-id="6430d-220">Provides cache eviction guidance to the built-in cache provider.</span></span> <span data-ttu-id="6430d-221">O servidor Web removerá entradas de cache `Low` primeiro quando estiver sob demanda de memória.</span><span class="sxs-lookup"><span data-stu-id="6430d-221">The web server will evict `Low` cache entries first when it's under memory pressure.</span></span>

<span data-ttu-id="6430d-222">Exemplo:</span><span class="sxs-lookup"><span data-stu-id="6430d-222">Example:</span></span>

```cshtml
<cache priority="High">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

<span data-ttu-id="6430d-223">O atributo `priority` não assegura um nível específico de retenção de cache.</span><span class="sxs-lookup"><span data-stu-id="6430d-223">The `priority` attribute doesn't guarantee a specific level of cache retention.</span></span> <span data-ttu-id="6430d-224">`CacheItemPriority` é apenas uma sugestão.</span><span class="sxs-lookup"><span data-stu-id="6430d-224">`CacheItemPriority` is only a suggestion.</span></span> <span data-ttu-id="6430d-225">Configurar esse atributo como `NeverRemove` não assegura que o cache sempre será retido.</span><span class="sxs-lookup"><span data-stu-id="6430d-225">Setting this attribute to `NeverRemove` doesn't guarantee that the cache will always be retained.</span></span> <span data-ttu-id="6430d-226">Consulte [Recursos adicionais](#additional-resources) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="6430d-226">See [Additional Resources](#additional-resources) for more information.</span></span>

<span data-ttu-id="6430d-227">O Auxiliar de Marca de Cache é dependente do [serviço de cache de memória](xref:performance/caching/memory).</span><span class="sxs-lookup"><span data-stu-id="6430d-227">The Cache Tag Helper is dependent on the [memory cache service](xref:performance/caching/memory).</span></span> <span data-ttu-id="6430d-228">O Auxiliar de Marca de Cache adiciona o serviço se ele não tiver sido adicionado.</span><span class="sxs-lookup"><span data-stu-id="6430d-228">The Cache Tag Helper adds the service if it has not been added.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6430d-229">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="6430d-229">Additional resources</span></span>

* [<span data-ttu-id="6430d-230">Cache na memória</span><span class="sxs-lookup"><span data-stu-id="6430d-230">Cache in-memory</span></span>](xref:performance/caching/memory)
* [<span data-ttu-id="6430d-231">Introdução ao Identity</span><span class="sxs-lookup"><span data-stu-id="6430d-231">Introduction to Identity</span></span>](xref:security/authentication/identity)

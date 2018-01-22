---
title: "Cache auxiliar de marca no núcleo do ASP.NET MVC"
author: pkellner
description: Mostra como trabalhar com o auxiliar de marca de Cache
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/cache-tag-helper
ms.openlocfilehash: dfd9c3c0c4e50a99e4f8703b01bd9b384930b87a
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/19/2018
---
# <a name="cache-tag-helper-in-aspnet-core-mvc"></a><span data-ttu-id="bac5b-103">Cache auxiliar de marca no núcleo do ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="bac5b-103">Cache Tag Helper in ASP.NET Core MVC</span></span>

<span data-ttu-id="bac5b-104">Por [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="bac5b-104">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="bac5b-105">O auxiliar de marca de Cache fornece a capacidade de melhorar drasticamente o desempenho do seu aplicativo ASP.NET Core armazenando em cache seu conteúdo para o provedor de cache interno do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="bac5b-105">The Cache Tag Helper provides the ability to dramatically improve the performance of your ASP.NET Core app by caching its content to the internal ASP.NET Core cache provider.</span></span>

<span data-ttu-id="bac5b-106">O mecanismo de exibição Razor define o padrão `expires-after` vinte minutos.</span><span class="sxs-lookup"><span data-stu-id="bac5b-106">The Razor View Engine sets the default `expires-after` to twenty minutes.</span></span>

<span data-ttu-id="bac5b-107">A seguinte marcação Razor armazena em cache a data/hora:</span><span class="sxs-lookup"><span data-stu-id="bac5b-107">The following Razor markup caches the date/time:</span></span>

```cshtml
<cache>@DateTime.Now</cache>
```

<span data-ttu-id="bac5b-108">A primeira solicitação para a página que contém `CacheTagHelper` exibirá a data/hora atual.</span><span class="sxs-lookup"><span data-stu-id="bac5b-108">The first request to the page that contains `CacheTagHelper` will display the current date/time.</span></span> <span data-ttu-id="bac5b-109">Solicitações adicionais mostrará o valor armazenado em cache até que o cache expira (padrão de 20 minutos) ou é removido por pressão de memória.</span><span class="sxs-lookup"><span data-stu-id="bac5b-109">Additional requests will show the cached value until the cache expires (default 20 minutes) or is evicted by memory pressure.</span></span>

<span data-ttu-id="bac5b-110">Você pode definir a duração do cache com os seguintes atributos:</span><span class="sxs-lookup"><span data-stu-id="bac5b-110">You can set the cache duration with the following attributes:</span></span>

## <a name="cache-tag-helper-attributes"></a><span data-ttu-id="bac5b-111">Atributos do auxiliar de marca de cache</span><span class="sxs-lookup"><span data-stu-id="bac5b-111">Cache Tag Helper Attributes</span></span>

- - -

### <a name="enabled"></a><span data-ttu-id="bac5b-112">habilitado</span><span class="sxs-lookup"><span data-stu-id="bac5b-112">enabled</span></span>    


| <span data-ttu-id="bac5b-113">Tipo de atributo</span><span class="sxs-lookup"><span data-stu-id="bac5b-113">Attribute Type</span></span>    | <span data-ttu-id="bac5b-114">Valores válidos</span><span class="sxs-lookup"><span data-stu-id="bac5b-114">Valid Values</span></span>      |
|----------------   |----------------   |
| <span data-ttu-id="bac5b-115">boolean</span><span class="sxs-lookup"><span data-stu-id="bac5b-115">boolean</span></span>           | <span data-ttu-id="bac5b-116">"true" (padrão)</span><span class="sxs-lookup"><span data-stu-id="bac5b-116">"true" (default)</span></span>  |
|                   | <span data-ttu-id="bac5b-117">"false"</span><span class="sxs-lookup"><span data-stu-id="bac5b-117">"false"</span></span>   |


<span data-ttu-id="bac5b-118">Determina se o conteúdo entre o auxiliar de marca de Cache é armazenado em cache.</span><span class="sxs-lookup"><span data-stu-id="bac5b-118">Determines whether the content enclosed by the Cache Tag Helper is cached.</span></span> <span data-ttu-id="bac5b-119">O padrão é `true`.</span><span class="sxs-lookup"><span data-stu-id="bac5b-119">The default is `true`.</span></span>  <span data-ttu-id="bac5b-120">Se definido como `false` este auxiliar de marca de Cache não tem cache efeito na saída renderizada.</span><span class="sxs-lookup"><span data-stu-id="bac5b-120">If set to `false` this Cache Tag Helper will have no caching effect on the rendered output.</span></span>

<span data-ttu-id="bac5b-121">Exemplo:</span><span class="sxs-lookup"><span data-stu-id="bac5b-121">Example:</span></span>

```cshtml
<cache enabled="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-on"></a><span data-ttu-id="bac5b-122">expires-on</span><span class="sxs-lookup"><span data-stu-id="bac5b-122">expires-on</span></span> 

| <span data-ttu-id="bac5b-123">Tipo de atributo</span><span class="sxs-lookup"><span data-stu-id="bac5b-123">Attribute Type</span></span>    | <span data-ttu-id="bac5b-124">Valor de exemplo</span><span class="sxs-lookup"><span data-stu-id="bac5b-124">Example Value</span></span>     |
|----------------   |----------------   |
| <span data-ttu-id="bac5b-125">DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="bac5b-125">DateTimeOffset</span></span>    | <span data-ttu-id="bac5b-126">"@new DateTime(2025,1,29,17,02,0)"</span><span class="sxs-lookup"><span data-stu-id="bac5b-126">"@new DateTime(2025,1,29,17,02,0)"</span></span>    |


<span data-ttu-id="bac5b-127">Define uma data de expiração absoluta.</span><span class="sxs-lookup"><span data-stu-id="bac5b-127">Sets an absolute expiration date.</span></span> <span data-ttu-id="bac5b-128">O exemplo a seguir armazenará em cache o conteúdo do auxiliar de marca de Cache até 17:02:00 em 29 de janeiro de 2025.</span><span class="sxs-lookup"><span data-stu-id="bac5b-128">The following example will cache the contents of the Cache Tag Helper until 5:02 PM on January 29, 2025.</span></span>

<span data-ttu-id="bac5b-129">Exemplo:</span><span class="sxs-lookup"><span data-stu-id="bac5b-129">Example:</span></span>

```cshtml
<cache expires-on="@new DateTime(2025,1,29,17,02,0)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-after"></a><span data-ttu-id="bac5b-130">expires-after</span><span class="sxs-lookup"><span data-stu-id="bac5b-130">expires-after</span></span>

| <span data-ttu-id="bac5b-131">Tipo de atributo</span><span class="sxs-lookup"><span data-stu-id="bac5b-131">Attribute Type</span></span>    | <span data-ttu-id="bac5b-132">Valor de exemplo</span><span class="sxs-lookup"><span data-stu-id="bac5b-132">Example Value</span></span>     |
|----------------   |----------------   |
| <span data-ttu-id="bac5b-133">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="bac5b-133">TimeSpan</span></span>    | <span data-ttu-id="bac5b-134">"@TimeSpan.FromSeconds(120)"</span><span class="sxs-lookup"><span data-stu-id="bac5b-134">"@TimeSpan.FromSeconds(120)"</span></span>    |


<span data-ttu-id="bac5b-135">Define o período de tempo desde a primeira vez de solicitação para armazenar em cache o conteúdo.</span><span class="sxs-lookup"><span data-stu-id="bac5b-135">Sets the length of time from the first request time to cache the contents.</span></span> 

<span data-ttu-id="bac5b-136">Exemplo:</span><span class="sxs-lookup"><span data-stu-id="bac5b-136">Example:</span></span>

```cshtml
<cache expires-after="@TimeSpan.FromSeconds(120)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-sliding"></a><span data-ttu-id="bac5b-137">expires-sliding</span><span class="sxs-lookup"><span data-stu-id="bac5b-137">expires-sliding</span></span>

| <span data-ttu-id="bac5b-138">Tipo de atributo</span><span class="sxs-lookup"><span data-stu-id="bac5b-138">Attribute Type</span></span>    | <span data-ttu-id="bac5b-139">Valor de exemplo</span><span class="sxs-lookup"><span data-stu-id="bac5b-139">Example Value</span></span>     |
|----------------   |----------------   |
| <span data-ttu-id="bac5b-140">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="bac5b-140">TimeSpan</span></span>    | <span data-ttu-id="bac5b-141">"@TimeSpan.FromSeconds(60)"</span><span class="sxs-lookup"><span data-stu-id="bac5b-141">"@TimeSpan.FromSeconds(60)"</span></span>     |


<span data-ttu-id="bac5b-142">Define a hora em que uma entrada de cache deve ser removida se não tiver sido acessada.</span><span class="sxs-lookup"><span data-stu-id="bac5b-142">Sets the time that a cache entry should be evicted if it has not been accessed.</span></span>

<span data-ttu-id="bac5b-143">Exemplo:</span><span class="sxs-lookup"><span data-stu-id="bac5b-143">Example:</span></span>

```cshtml
<cache expires-sliding="@TimeSpan.FromSeconds(60)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-header"></a><span data-ttu-id="bac5b-144">variar por cabeçalho</span><span class="sxs-lookup"><span data-stu-id="bac5b-144">vary-by-header</span></span>

| <span data-ttu-id="bac5b-145">Tipo de atributo</span><span class="sxs-lookup"><span data-stu-id="bac5b-145">Attribute Type</span></span>    | <span data-ttu-id="bac5b-146">Valores de exemplo</span><span class="sxs-lookup"><span data-stu-id="bac5b-146">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="bac5b-147">Cadeia de Caracteres</span><span class="sxs-lookup"><span data-stu-id="bac5b-147">String</span></span>            | <span data-ttu-id="bac5b-148">"User-Agent"</span><span class="sxs-lookup"><span data-stu-id="bac5b-148">"User-Agent"</span></span>                  |
|                   | <span data-ttu-id="bac5b-149">"User-Agent,content-encoding"</span><span class="sxs-lookup"><span data-stu-id="bac5b-149">"User-Agent,content-encoding"</span></span> |

<span data-ttu-id="bac5b-150">Aceita um valor de cabeçalho único ou uma lista separada por vírgulas de valores de cabeçalho que disparam uma atualização do cache quando elas forem alteradas.</span><span class="sxs-lookup"><span data-stu-id="bac5b-150">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when they change.</span></span> <span data-ttu-id="bac5b-151">O exemplo a seguir monitora o valor do cabeçalho `User-Agent`.</span><span class="sxs-lookup"><span data-stu-id="bac5b-151">The following example monitors the header value `User-Agent`.</span></span> <span data-ttu-id="bac5b-152">O exemplo armazenará em cache o conteúdo para todos os diferentes `User-Agent` apresentado para o servidor web.</span><span class="sxs-lookup"><span data-stu-id="bac5b-152">The example will cache the content for every different `User-Agent` presented to the web server.</span></span>

<span data-ttu-id="bac5b-153">Exemplo:</span><span class="sxs-lookup"><span data-stu-id="bac5b-153">Example:</span></span>

```cshtml
<cache vary-by-header="User-Agent">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-query"></a><span data-ttu-id="bac5b-154">vary-by-query</span><span class="sxs-lookup"><span data-stu-id="bac5b-154">vary-by-query</span></span>

| <span data-ttu-id="bac5b-155">Tipo de atributo</span><span class="sxs-lookup"><span data-stu-id="bac5b-155">Attribute Type</span></span>    | <span data-ttu-id="bac5b-156">Valores de exemplo</span><span class="sxs-lookup"><span data-stu-id="bac5b-156">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="bac5b-157">Cadeia de Caracteres</span><span class="sxs-lookup"><span data-stu-id="bac5b-157">String</span></span>            | <span data-ttu-id="bac5b-158">"Tornar"</span><span class="sxs-lookup"><span data-stu-id="bac5b-158">"Make"</span></span>                |
|                   | <span data-ttu-id="bac5b-159">Modelo de "criar"</span><span class="sxs-lookup"><span data-stu-id="bac5b-159">"Make,Model"</span></span> |

<span data-ttu-id="bac5b-160">Aceita um valor de cabeçalho único ou uma lista separada por vírgulas de valores de cabeçalho que disparam uma atualização do cache quando o valor do cabeçalho é alterado.</span><span class="sxs-lookup"><span data-stu-id="bac5b-160">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the header value changes.</span></span> <span data-ttu-id="bac5b-161">O exemplo a seguir examina os valores de `Make` e `Model`.</span><span class="sxs-lookup"><span data-stu-id="bac5b-161">The following example looks at the values of `Make` and `Model`.</span></span>

<span data-ttu-id="bac5b-162">Exemplo:</span><span class="sxs-lookup"><span data-stu-id="bac5b-162">Example:</span></span>

```cshtml
<cache vary-by-query="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-route"></a><span data-ttu-id="bac5b-163">variar por rota</span><span class="sxs-lookup"><span data-stu-id="bac5b-163">vary-by-route</span></span>

| <span data-ttu-id="bac5b-164">Tipo de atributo</span><span class="sxs-lookup"><span data-stu-id="bac5b-164">Attribute Type</span></span>    | <span data-ttu-id="bac5b-165">Valores de exemplo</span><span class="sxs-lookup"><span data-stu-id="bac5b-165">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="bac5b-166">Cadeia de Caracteres</span><span class="sxs-lookup"><span data-stu-id="bac5b-166">String</span></span>            | <span data-ttu-id="bac5b-167">"Tornar"</span><span class="sxs-lookup"><span data-stu-id="bac5b-167">"Make"</span></span>                |
|                   | <span data-ttu-id="bac5b-168">Modelo de "criar"</span><span class="sxs-lookup"><span data-stu-id="bac5b-168">"Make,Model"</span></span> |

<span data-ttu-id="bac5b-169">Aceita um valor de cabeçalho único ou uma lista separada por vírgulas de valores de cabeçalho que disparam uma atualização de cache quando a alteração de valores de parâmetro de dados de rota.</span><span class="sxs-lookup"><span data-stu-id="bac5b-169">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the route data parameter value(s) change.</span></span> <span data-ttu-id="bac5b-170">Exemplo:</span><span class="sxs-lookup"><span data-stu-id="bac5b-170">Example:</span></span>

<span data-ttu-id="bac5b-171">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="bac5b-171">*Startup.cs*</span></span> 

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{Make?}/{Model?}");
```
  
<span data-ttu-id="bac5b-172">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="bac5b-172">*Index.cshtml*</span></span>

```cshtml
<cache vary-by-route="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-cookie"></a><span data-ttu-id="bac5b-173">variar por cookie</span><span class="sxs-lookup"><span data-stu-id="bac5b-173">vary-by-cookie</span></span>

| <span data-ttu-id="bac5b-174">Tipo de atributo</span><span class="sxs-lookup"><span data-stu-id="bac5b-174">Attribute Type</span></span>    | <span data-ttu-id="bac5b-175">Valores de exemplo</span><span class="sxs-lookup"><span data-stu-id="bac5b-175">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="bac5b-176">Cadeia de Caracteres</span><span class="sxs-lookup"><span data-stu-id="bac5b-176">String</span></span>            | <span data-ttu-id="bac5b-177">".AspNetCore.Identity.Application"</span><span class="sxs-lookup"><span data-stu-id="bac5b-177">".AspNetCore.Identity.Application"</span></span>                |
|                   | <span data-ttu-id="bac5b-178">".AspNetCore.Identity.Application,HairColor"</span><span class="sxs-lookup"><span data-stu-id="bac5b-178">".AspNetCore.Identity.Application,HairColor"</span></span> |

<span data-ttu-id="bac5b-179">Aceita um valor de cabeçalho único ou uma lista separada por vírgulas de valores de cabeçalho que disparam uma atualização do cache quando os valores de alteração (s).</span><span class="sxs-lookup"><span data-stu-id="bac5b-179">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the header values(s) change.</span></span> <span data-ttu-id="bac5b-180">O exemplo a seguir examina o cookie associado à identidade do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="bac5b-180">The following example looks at the cookie associated with ASP.NET Identity.</span></span> <span data-ttu-id="bac5b-181">Quando um usuário é autenticado o cookie de solicitação a ser definido que dispara uma atualização do cache.</span><span class="sxs-lookup"><span data-stu-id="bac5b-181">When a user is authenticated the request cookie to be set which triggers a cache refresh.</span></span>

<span data-ttu-id="bac5b-182">Exemplo:</span><span class="sxs-lookup"><span data-stu-id="bac5b-182">Example:</span></span>

```cshtml
<cache vary-by-cookie=".AspNetCore.Identity.Application">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-user"></a><span data-ttu-id="bac5b-183">variar por usuário</span><span class="sxs-lookup"><span data-stu-id="bac5b-183">vary-by-user</span></span>

| <span data-ttu-id="bac5b-184">Tipo de atributo</span><span class="sxs-lookup"><span data-stu-id="bac5b-184">Attribute Type</span></span>    | <span data-ttu-id="bac5b-185">Valores de exemplo</span><span class="sxs-lookup"><span data-stu-id="bac5b-185">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="bac5b-186">Boolean</span><span class="sxs-lookup"><span data-stu-id="bac5b-186">Boolean</span></span>             | <span data-ttu-id="bac5b-187">"true"</span><span class="sxs-lookup"><span data-stu-id="bac5b-187">"true"</span></span>                  |
|                     | <span data-ttu-id="bac5b-188">"falso" (padrão)</span><span class="sxs-lookup"><span data-stu-id="bac5b-188">"false" (default)</span></span> |

<span data-ttu-id="bac5b-189">Especifica se ou não o cache deve ser redefinido quando o usuário conectado (ou entidade de contexto) é alterado.</span><span class="sxs-lookup"><span data-stu-id="bac5b-189">Specifies whether or not the cache should reset when the logged-in user (or Context Principal) changes.</span></span> <span data-ttu-id="bac5b-190">O usuário atual é também conhecido como a entidade de contexto de solicitação e podem ser exibido em um modo de exibição Razor referenciando `@User.Identity.Name`.</span><span class="sxs-lookup"><span data-stu-id="bac5b-190">The current user is also known as the Request Context Principal and can be viewed in a Razor view by referencing `@User.Identity.Name`.</span></span>

<span data-ttu-id="bac5b-191">O exemplo a seguir examina o usuário conectado no momento.</span><span class="sxs-lookup"><span data-stu-id="bac5b-191">The following example looks at the current logged in user.</span></span>  

<span data-ttu-id="bac5b-192">Exemplo:</span><span class="sxs-lookup"><span data-stu-id="bac5b-192">Example:</span></span>

```cshtml
<cache vary-by-user="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

<span data-ttu-id="bac5b-193">Usar esse atributo mantém o conteúdo em cache por um ciclo de logon e logoff.</span><span class="sxs-lookup"><span data-stu-id="bac5b-193">Using this attribute maintains the contents in cache through a log-in and log-out cycle.</span></span>  <span data-ttu-id="bac5b-194">Ao usar `vary-by-user="true"`, uma ação de logon e logoff invalida o cache para o usuário autenticado.</span><span class="sxs-lookup"><span data-stu-id="bac5b-194">When using `vary-by-user="true"`, a log-in and log-out action invalidates the cache for the authenticated user.</span></span>  <span data-ttu-id="bac5b-195">O cache é invalidado porque um novo valor de cookie exclusivo é gerado no logon.</span><span class="sxs-lookup"><span data-stu-id="bac5b-195">The cache is invalidated because a new unique cookie value is generated on login.</span></span> <span data-ttu-id="bac5b-196">Cache é mantido para o estado anônimo quando nenhum cookie está presente ou expirou.</span><span class="sxs-lookup"><span data-stu-id="bac5b-196">Cache is maintained for the anonymous state when no cookie is present or has expired.</span></span> <span data-ttu-id="bac5b-197">Isso significa que se nenhum usuário estiver conectado, o cache será mantido.</span><span class="sxs-lookup"><span data-stu-id="bac5b-197">This means if no user is logged in, the cache will be maintained.</span></span>

- - -

### <a name="vary-by"></a><span data-ttu-id="bac5b-198">vary-by</span><span class="sxs-lookup"><span data-stu-id="bac5b-198">vary-by</span></span>

| <span data-ttu-id="bac5b-199">Tipo de atributo</span><span class="sxs-lookup"><span data-stu-id="bac5b-199">Attribute Type</span></span>    | <span data-ttu-id="bac5b-200">Valores de exemplo</span><span class="sxs-lookup"><span data-stu-id="bac5b-200">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="bac5b-201">Cadeia de Caracteres</span><span class="sxs-lookup"><span data-stu-id="bac5b-201">String</span></span>             | <span data-ttu-id="bac5b-202">"@Model"</span><span class="sxs-lookup"><span data-stu-id="bac5b-202">"@Model"</span></span>                 |


<span data-ttu-id="bac5b-203">Permite a personalização de dados que é armazenado em cache.</span><span class="sxs-lookup"><span data-stu-id="bac5b-203">Allows for customization of what data gets cached.</span></span> <span data-ttu-id="bac5b-204">Quando o objeto referenciado por alterações de valor de cadeia de caracteres do atributo, o conteúdo do auxiliar de marca de Cache é atualizado.</span><span class="sxs-lookup"><span data-stu-id="bac5b-204">When the object referenced by the attribute's string value changes, the content of the Cache Tag Helper is updated.</span></span> <span data-ttu-id="bac5b-205">Geralmente uma concatenação de cadeia de caracteres de valores de modelo são atribuídos a este atributo.</span><span class="sxs-lookup"><span data-stu-id="bac5b-205">Often a string-concatenation of model values are assigned to this attribute.</span></span>  <span data-ttu-id="bac5b-206">Na verdade, isso significa que uma atualização para qualquer um dos valores concatenados invalida o cache.</span><span class="sxs-lookup"><span data-stu-id="bac5b-206">Effectively, that means an update to any of the concatenated values invalidates the cache.</span></span>

<span data-ttu-id="bac5b-207">O exemplo a seguir supõe que o método do controlador renderização as somas de exibir o valor de inteiro de dois parâmetros de rota, `myParam1` e `myParam2`e retorna que como a propriedade de modelo único.</span><span class="sxs-lookup"><span data-stu-id="bac5b-207">The following example assumes the controller method rendering the view sums the integer value of the two route parameters, `myParam1` and `myParam2`, and returns that as the single model property.</span></span> <span data-ttu-id="bac5b-208">Quando essa soma é alterado, o conteúdo do auxiliar de marca de Cache é renderizado e armazenado em cache novamente.</span><span class="sxs-lookup"><span data-stu-id="bac5b-208">When this sum changes, the content of the Cache Tag Helper is rendered and cached again.</span></span>  

<span data-ttu-id="bac5b-209">Exemplo:</span><span class="sxs-lookup"><span data-stu-id="bac5b-209">Example:</span></span>

<span data-ttu-id="bac5b-210">Ação:</span><span class="sxs-lookup"><span data-stu-id="bac5b-210">Action:</span></span>

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

<span data-ttu-id="bac5b-211">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="bac5b-211">*Index.cshtml*</span></span>

```cshtml
<cache vary-by="@Model"">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="priority"></a><span data-ttu-id="bac5b-212">priority</span><span class="sxs-lookup"><span data-stu-id="bac5b-212">priority</span></span>

| <span data-ttu-id="bac5b-213">Tipo de atributo</span><span class="sxs-lookup"><span data-stu-id="bac5b-213">Attribute Type</span></span>    | <span data-ttu-id="bac5b-214">Valores de exemplo</span><span class="sxs-lookup"><span data-stu-id="bac5b-214">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="bac5b-215">CacheItemPriority</span><span class="sxs-lookup"><span data-stu-id="bac5b-215">CacheItemPriority</span></span>  | <span data-ttu-id="bac5b-216">"Alta"</span><span class="sxs-lookup"><span data-stu-id="bac5b-216">"High"</span></span>                   |
|                    | <span data-ttu-id="bac5b-217">"Baixo"</span><span class="sxs-lookup"><span data-stu-id="bac5b-217">"Low"</span></span> |
|                    | <span data-ttu-id="bac5b-218">"NeverRemove"</span><span class="sxs-lookup"><span data-stu-id="bac5b-218">"NeverRemove"</span></span> |
|                    | <span data-ttu-id="bac5b-219">"Normal"</span><span class="sxs-lookup"><span data-stu-id="bac5b-219">"Normal"</span></span> |

<span data-ttu-id="bac5b-220">Fornece orientação de remoção de cache para o provedor de cache interno.</span><span class="sxs-lookup"><span data-stu-id="bac5b-220">Provides cache eviction guidance to the built-in cache provider.</span></span> <span data-ttu-id="bac5b-221">O servidor web irá remover `Low` entradas de cache primeiro quando ele estiver sob pressão de memória.</span><span class="sxs-lookup"><span data-stu-id="bac5b-221">The web server will evict `Low` cache entries first when it's under memory pressure.</span></span>

<span data-ttu-id="bac5b-222">Exemplo:</span><span class="sxs-lookup"><span data-stu-id="bac5b-222">Example:</span></span>

```cshtml
<cache priority="High">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

<span data-ttu-id="bac5b-223">O `priority` atributo não garante que um nível específico de retenção de cache.</span><span class="sxs-lookup"><span data-stu-id="bac5b-223">The `priority` attribute does not guarantee a specific level of cache retention.</span></span> <span data-ttu-id="bac5b-224">`CacheItemPriority`é apenas uma sugestão.</span><span class="sxs-lookup"><span data-stu-id="bac5b-224">`CacheItemPriority` is only a suggestion.</span></span> <span data-ttu-id="bac5b-225">A configuração desse atributo `NeverRemove` não garante que o cache sempre será retido.</span><span class="sxs-lookup"><span data-stu-id="bac5b-225">Setting this attribute to `NeverRemove` does not guarantee that the cache will always be retained.</span></span> <span data-ttu-id="bac5b-226">Consulte [recursos adicionais](#additional-resources) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="bac5b-226">See [Additional Resources](#additional-resources) for more information.</span></span>

<span data-ttu-id="bac5b-227">O auxiliar de marca de Cache é dependente de [serviço de cache de memória](xref:performance/caching/memory).</span><span class="sxs-lookup"><span data-stu-id="bac5b-227">The Cache Tag Helper is dependent on the [memory cache service](xref:performance/caching/memory).</span></span> <span data-ttu-id="bac5b-228">O auxiliar de marca de Cache adiciona o serviço se não tiver sido adicionado.</span><span class="sxs-lookup"><span data-stu-id="bac5b-228">The Cache Tag Helper adds the service if it has not been added.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="bac5b-229">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="bac5b-229">Additional resources</span></span>

* <xref:performance/caching/memory>
* <xref:security/authentication/identity>

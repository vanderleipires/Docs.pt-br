---
title: "Cache auxiliar de marca no núcleo do ASP.NET MVC"
author: pkellner
description: Mostra como trabalhar com o auxiliar de marca de Cache
keywords: "ASP.NET Core, auxiliar de marcação"
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.assetid: c045d485-d1dc-4cea-a675-46be83b7a012
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/cache-tag-helper
ms.openlocfilehash: da5b7b3bf1aa01ee22edf9bd003d8f79a00a5d0b
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/28/2017
---
# <a name="cache-tag-helper-in-aspnet-core-mvc"></a><span data-ttu-id="e2b56-104">Cache auxiliar de marca no núcleo do ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="e2b56-104">Cache Tag Helper in ASP.NET Core MVC</span></span>

<span data-ttu-id="e2b56-105">Por [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="e2b56-105">By [Peter Kellner](http://peterkellner.net)</span></span> 


<span data-ttu-id="e2b56-106">O auxiliar de marca de Cache fornece a capacidade de melhorar drasticamente o desempenho do seu aplicativo ASP.NET Core armazenando em cache seu conteúdo para o provedor de cache interno do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e2b56-106">The  Cache Tag Helper provides the ability to dramatically improve the performance of your ASP.NET Core app by caching its content to the internal ASP.NET Core cache provider.</span></span>

<span data-ttu-id="e2b56-107">O mecanismo de exibição Razor define o padrão `expires-after` vinte minutos.</span><span class="sxs-lookup"><span data-stu-id="e2b56-107">The Razor View Engine sets the default `expires-after` to twenty minutes.</span></span>

<span data-ttu-id="e2b56-108">A seguinte marcação Razor armazena em cache a data/hora:</span><span class="sxs-lookup"><span data-stu-id="e2b56-108">The following Razor markup caches the date/time:</span></span>

```cshtml
<Cache>@DateTime.Now<Cache>
```

<span data-ttu-id="e2b56-109">A primeira solicitação para a página que contém `CacheTagHelper` exibirá a data/hora atual.</span><span class="sxs-lookup"><span data-stu-id="e2b56-109">The first request to the page that contains `CacheTagHelper` will display the current date/time.</span></span> <span data-ttu-id="e2b56-110">Solicitações adicionais mostrará o valor armazenado em cache até que o cache expira (padrão de 20 minutos) ou é removido por pressão de memória.</span><span class="sxs-lookup"><span data-stu-id="e2b56-110">Additional requests will show the cached value until the cache expires (default 20 minutes) or is evicted by memory pressure.</span></span>

<span data-ttu-id="e2b56-111">Você pode definir a duração do cache com os seguintes atributos:</span><span class="sxs-lookup"><span data-stu-id="e2b56-111">You can set the cache duration with the following attributes:</span></span>

## <a name="cache-tag-helper-attributes"></a><span data-ttu-id="e2b56-112">Atributos do auxiliar de marca de cache</span><span class="sxs-lookup"><span data-stu-id="e2b56-112">Cache Tag Helper Attributes</span></span>

- - -

### <a name="enabled"></a><span data-ttu-id="e2b56-113">Habilitado</span><span class="sxs-lookup"><span data-stu-id="e2b56-113">enabled</span></span>    


| <span data-ttu-id="e2b56-114">Tipo de atributo</span><span class="sxs-lookup"><span data-stu-id="e2b56-114">Attribute Type</span></span>    | <span data-ttu-id="e2b56-115">Valores válidos</span><span class="sxs-lookup"><span data-stu-id="e2b56-115">Valid Values</span></span>      |
|----------------   |----------------   |
| <span data-ttu-id="e2b56-116">boolean</span><span class="sxs-lookup"><span data-stu-id="e2b56-116">boolean</span></span>           | <span data-ttu-id="e2b56-117">"true" (padrão)</span><span class="sxs-lookup"><span data-stu-id="e2b56-117">"true" (default)</span></span>  |
|                   | <span data-ttu-id="e2b56-118">"false"</span><span class="sxs-lookup"><span data-stu-id="e2b56-118">"false"</span></span>   |


<span data-ttu-id="e2b56-119">Determina se o conteúdo entre o auxiliar de marca de Cache é armazenado em cache.</span><span class="sxs-lookup"><span data-stu-id="e2b56-119">Determines whether the content enclosed by the Cache Tag Helper is cached.</span></span> <span data-ttu-id="e2b56-120">O padrão é `true`.</span><span class="sxs-lookup"><span data-stu-id="e2b56-120">The default is `true`.</span></span>  <span data-ttu-id="e2b56-121">Se definido como `false` este auxiliar de marca de Cache não tem cache efeito na saída renderizada.</span><span class="sxs-lookup"><span data-stu-id="e2b56-121">If set to `false` this Cache Tag Helper will have no caching effect on the rendered output.</span></span>

<span data-ttu-id="e2b56-122">Exemplo:</span><span class="sxs-lookup"><span data-stu-id="e2b56-122">Example:</span></span>

```cshtml
<Cache enabled="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="expires-on"></a><span data-ttu-id="e2b56-123">expira em</span><span class="sxs-lookup"><span data-stu-id="e2b56-123">expires-on</span></span> 

| <span data-ttu-id="e2b56-124">Tipo de atributo</span><span class="sxs-lookup"><span data-stu-id="e2b56-124">Attribute Type</span></span>    | <span data-ttu-id="e2b56-125">Valor de exemplo</span><span class="sxs-lookup"><span data-stu-id="e2b56-125">Example Value</span></span>     |
|----------------   |----------------   |
| <span data-ttu-id="e2b56-126">DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="e2b56-126">DateTimeOffset</span></span>    | <span data-ttu-id="e2b56-127">"@new DateTime(2025,1,29,17,02,0)"</span><span class="sxs-lookup"><span data-stu-id="e2b56-127">"@new DateTime(2025,1,29,17,02,0)"</span></span>    |


<span data-ttu-id="e2b56-128">Define uma data de expiração absoluta.</span><span class="sxs-lookup"><span data-stu-id="e2b56-128">Sets an absolute expiration date.</span></span> <span data-ttu-id="e2b56-129">O exemplo a seguir armazenará em cache o conteúdo do auxiliar de marca de Cache até 17:02:00 em 29 de janeiro de 2025.</span><span class="sxs-lookup"><span data-stu-id="e2b56-129">The following example will cache the contents of the Cache Tag Helper until 5:02 PM on January 29, 2025.</span></span>

<span data-ttu-id="e2b56-130">Exemplo:</span><span class="sxs-lookup"><span data-stu-id="e2b56-130">Example:</span></span>

```cshtml
<Cache expires-on="@new DateTime(2025,1,29,17,02,0)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="expires-after"></a><span data-ttu-id="e2b56-131">expira após</span><span class="sxs-lookup"><span data-stu-id="e2b56-131">expires-after</span></span>

| <span data-ttu-id="e2b56-132">Tipo de atributo</span><span class="sxs-lookup"><span data-stu-id="e2b56-132">Attribute Type</span></span>    | <span data-ttu-id="e2b56-133">Valor de exemplo</span><span class="sxs-lookup"><span data-stu-id="e2b56-133">Example Value</span></span>     |
|----------------   |----------------   |
| <span data-ttu-id="e2b56-134">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="e2b56-134">TimeSpan</span></span>    | <span data-ttu-id="e2b56-135">" @TimeSpan.FromSeconds (120)"</span><span class="sxs-lookup"><span data-stu-id="e2b56-135">"@TimeSpan.FromSeconds(120)"</span></span>    |


<span data-ttu-id="e2b56-136">Define o período de tempo desde a primeira vez de solicitação para armazenar em cache o conteúdo.</span><span class="sxs-lookup"><span data-stu-id="e2b56-136">Sets the length of time from the first request time to cache the contents.</span></span> 

<span data-ttu-id="e2b56-137">Exemplo:</span><span class="sxs-lookup"><span data-stu-id="e2b56-137">Example:</span></span>

```cshtml
<Cache expires-on="@TimeSpan.FromSeconds(120)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="expires-sliding"></a><span data-ttu-id="e2b56-138">expiração deslizante</span><span class="sxs-lookup"><span data-stu-id="e2b56-138">expires-sliding</span></span>

| <span data-ttu-id="e2b56-139">Tipo de atributo</span><span class="sxs-lookup"><span data-stu-id="e2b56-139">Attribute Type</span></span>    | <span data-ttu-id="e2b56-140">Valor de exemplo</span><span class="sxs-lookup"><span data-stu-id="e2b56-140">Example Value</span></span>     |
|----------------   |----------------   |
| <span data-ttu-id="e2b56-141">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="e2b56-141">TimeSpan</span></span>    | <span data-ttu-id="e2b56-142">" @TimeSpan.FromSeconds (60)"</span><span class="sxs-lookup"><span data-stu-id="e2b56-142">"@TimeSpan.FromSeconds(60)"</span></span>     |


<span data-ttu-id="e2b56-143">Define a hora em que uma entrada de cache deve ser removida se não tiver sido acessada.</span><span class="sxs-lookup"><span data-stu-id="e2b56-143">Sets the time that a cache entry should be evicted if it has not been accessed.</span></span>

<span data-ttu-id="e2b56-144">Exemplo:</span><span class="sxs-lookup"><span data-stu-id="e2b56-144">Example:</span></span>

```cshtml
<Cache expires-sliding="@TimeSpan.FromSeconds(60)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="vary-by-header"></a><span data-ttu-id="e2b56-145">variar por cabeçalho</span><span class="sxs-lookup"><span data-stu-id="e2b56-145">vary-by-header</span></span>

| <span data-ttu-id="e2b56-146">Tipo de atributo</span><span class="sxs-lookup"><span data-stu-id="e2b56-146">Attribute Type</span></span>    | <span data-ttu-id="e2b56-147">Valores de exemplo</span><span class="sxs-lookup"><span data-stu-id="e2b56-147">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="e2b56-148">Cadeia de caracteres</span><span class="sxs-lookup"><span data-stu-id="e2b56-148">String</span></span>            | <span data-ttu-id="e2b56-149">"Agente de usuário"</span><span class="sxs-lookup"><span data-stu-id="e2b56-149">"User-Agent"</span></span>                  |
|                   | <span data-ttu-id="e2b56-150">"Agente de usuário, codificação de conteúdo"</span><span class="sxs-lookup"><span data-stu-id="e2b56-150">"User-Agent,content-encoding"</span></span> |

<span data-ttu-id="e2b56-151">Aceita um valor de cabeçalho único ou uma lista separada por vírgulas de valores de cabeçalho que disparam uma atualização do cache quando elas forem alteradas.</span><span class="sxs-lookup"><span data-stu-id="e2b56-151">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when they change.</span></span> <span data-ttu-id="e2b56-152">O exemplo a seguir monitora o valor do cabeçalho `User-Agent`.</span><span class="sxs-lookup"><span data-stu-id="e2b56-152">The following example monitors the header value `User-Agent`.</span></span> <span data-ttu-id="e2b56-153">O exemplo armazenará em cache o conteúdo para todos os diferentes `User-Agent` apresentado para o servidor web.</span><span class="sxs-lookup"><span data-stu-id="e2b56-153">The example will cache the content for every different `User-Agent` presented to the web server.</span></span>

<span data-ttu-id="e2b56-154">Exemplo:</span><span class="sxs-lookup"><span data-stu-id="e2b56-154">Example:</span></span>

```cshtml
<Cache vary-by-header="User-Agent">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="vary-by-query"></a><span data-ttu-id="e2b56-155">variar por consulta</span><span class="sxs-lookup"><span data-stu-id="e2b56-155">vary-by-query</span></span>

| <span data-ttu-id="e2b56-156">Tipo de atributo</span><span class="sxs-lookup"><span data-stu-id="e2b56-156">Attribute Type</span></span>    | <span data-ttu-id="e2b56-157">Valores de exemplo</span><span class="sxs-lookup"><span data-stu-id="e2b56-157">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="e2b56-158">Cadeia de caracteres</span><span class="sxs-lookup"><span data-stu-id="e2b56-158">String</span></span>            | <span data-ttu-id="e2b56-159">"Tornar"</span><span class="sxs-lookup"><span data-stu-id="e2b56-159">"Make"</span></span>                |
|                   | <span data-ttu-id="e2b56-160">Modelo de "criar"</span><span class="sxs-lookup"><span data-stu-id="e2b56-160">"Make,Model"</span></span> |

<span data-ttu-id="e2b56-161">Aceita um valor de cabeçalho único ou uma lista separada por vírgulas de valores de cabeçalho que disparam uma atualização do cache quando o valor do cabeçalho é alterado.</span><span class="sxs-lookup"><span data-stu-id="e2b56-161">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the header value changes.</span></span> <span data-ttu-id="e2b56-162">O exemplo a seguir examina os valores de `Make` e `Model`.</span><span class="sxs-lookup"><span data-stu-id="e2b56-162">The following example looks at the values of `Make` and `Model`.</span></span>

<span data-ttu-id="e2b56-163">Exemplo:</span><span class="sxs-lookup"><span data-stu-id="e2b56-163">Example:</span></span>

```cshtml
<Cache vary-by-query="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="vary-by-route"></a><span data-ttu-id="e2b56-164">variar por rota</span><span class="sxs-lookup"><span data-stu-id="e2b56-164">vary-by-route</span></span>

| <span data-ttu-id="e2b56-165">Tipo de atributo</span><span class="sxs-lookup"><span data-stu-id="e2b56-165">Attribute Type</span></span>    | <span data-ttu-id="e2b56-166">Valores de exemplo</span><span class="sxs-lookup"><span data-stu-id="e2b56-166">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="e2b56-167">Cadeia de caracteres</span><span class="sxs-lookup"><span data-stu-id="e2b56-167">String</span></span>            | <span data-ttu-id="e2b56-168">"Tornar"</span><span class="sxs-lookup"><span data-stu-id="e2b56-168">"Make"</span></span>                |
|                   | <span data-ttu-id="e2b56-169">Modelo de "criar"</span><span class="sxs-lookup"><span data-stu-id="e2b56-169">"Make,Model"</span></span> |

<span data-ttu-id="e2b56-170">Aceita um valor de cabeçalho único ou uma lista separada por vírgulas de valores de cabeçalho que disparam uma atualização de cache quando a alteração de valores de parâmetro de dados de rota.</span><span class="sxs-lookup"><span data-stu-id="e2b56-170">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the route data parameter value(s) change.</span></span> <span data-ttu-id="e2b56-171">Exemplo:</span><span class="sxs-lookup"><span data-stu-id="e2b56-171">Example:</span></span>

<span data-ttu-id="e2b56-172">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="e2b56-172">*Startup.cs*</span></span> 

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{Make?}/{Model?}");
```
  
<span data-ttu-id="e2b56-173">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="e2b56-173">*Index.cshtml*</span></span>

```cshtml
<Cache vary-by-route="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="vary-by-cookie"></a><span data-ttu-id="e2b56-174">variar por cookie</span><span class="sxs-lookup"><span data-stu-id="e2b56-174">vary-by-cookie</span></span>

| <span data-ttu-id="e2b56-175">Tipo de atributo</span><span class="sxs-lookup"><span data-stu-id="e2b56-175">Attribute Type</span></span>    | <span data-ttu-id="e2b56-176">Valores de exemplo</span><span class="sxs-lookup"><span data-stu-id="e2b56-176">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="e2b56-177">Cadeia de caracteres</span><span class="sxs-lookup"><span data-stu-id="e2b56-177">String</span></span>            | <span data-ttu-id="e2b56-178">". AspNetCore.Identity.Application"</span><span class="sxs-lookup"><span data-stu-id="e2b56-178">".AspNetCore.Identity.Application"</span></span>                |
|                   | <span data-ttu-id="e2b56-179">". AspNetCore.Identity.Application,HairColor"</span><span class="sxs-lookup"><span data-stu-id="e2b56-179">".AspNetCore.Identity.Application,HairColor"</span></span> |

<span data-ttu-id="e2b56-180">Aceita um valor de cabeçalho único ou uma lista separada por vírgulas de valores de cabeçalho que disparam uma atualização do cache quando os valores de alteração (s).</span><span class="sxs-lookup"><span data-stu-id="e2b56-180">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the header values(s) change.</span></span> <span data-ttu-id="e2b56-181">O exemplo a seguir examina o cookie associado à identidade do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="e2b56-181">The following example looks at the cookie associated with ASP.NET Identity.</span></span> <span data-ttu-id="e2b56-182">Quando um usuário é autenticado o cookie de solicitação a ser definido que dispara uma atualização do cache.</span><span class="sxs-lookup"><span data-stu-id="e2b56-182">When a user is authenticated the request cookie to be set which triggers a cache refresh.</span></span>

<span data-ttu-id="e2b56-183">Exemplo:</span><span class="sxs-lookup"><span data-stu-id="e2b56-183">Example:</span></span>

```cshtml
<Cache vary-by-cookie=".AspNetCore.Identity.Application">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="vary-by-user"></a><span data-ttu-id="e2b56-184">variar por usuário</span><span class="sxs-lookup"><span data-stu-id="e2b56-184">vary-by-user</span></span>

| <span data-ttu-id="e2b56-185">Tipo de atributo</span><span class="sxs-lookup"><span data-stu-id="e2b56-185">Attribute Type</span></span>    | <span data-ttu-id="e2b56-186">Valores de exemplo</span><span class="sxs-lookup"><span data-stu-id="e2b56-186">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="e2b56-187">Boolean</span><span class="sxs-lookup"><span data-stu-id="e2b56-187">Boolean</span></span>             | <span data-ttu-id="e2b56-188">"true"</span><span class="sxs-lookup"><span data-stu-id="e2b56-188">"true"</span></span>                  |
|                     | <span data-ttu-id="e2b56-189">"falso" (padrão)</span><span class="sxs-lookup"><span data-stu-id="e2b56-189">"false" (default)</span></span> |

<span data-ttu-id="e2b56-190">Especifica se ou não o cache deve ser redefinido quando o usuário conectado (ou entidade de contexto) é alterado.</span><span class="sxs-lookup"><span data-stu-id="e2b56-190">Specifies whether or not the cache should reset when the logged-in user (or Context Principal) changes.</span></span> <span data-ttu-id="e2b56-191">O usuário atual é também conhecido como a entidade de contexto de solicitação e podem ser exibido em um modo de exibição Razor referenciando `@User.Identity.Name`.</span><span class="sxs-lookup"><span data-stu-id="e2b56-191">The current user is also known as the Request Context Principal and can be viewed in a Razor view by referencing `@User.Identity.Name`.</span></span>

<span data-ttu-id="e2b56-192">O exemplo a seguir examina o usuário conectado no momento.</span><span class="sxs-lookup"><span data-stu-id="e2b56-192">The following example looks at the current logged in user.</span></span>  

<span data-ttu-id="e2b56-193">Exemplo:</span><span class="sxs-lookup"><span data-stu-id="e2b56-193">Example:</span></span>

```cshtml
<Cache vary-by-user="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

<span data-ttu-id="e2b56-194">Usar esse atributo mantém o conteúdo em cache por um ciclo de logon e logoff.</span><span class="sxs-lookup"><span data-stu-id="e2b56-194">Using this attribute maintains the contents in cache through a log-in and log-out cycle.</span></span>  <span data-ttu-id="e2b56-195">Ao usar `vary-by-user="true"`, uma ação de logon e logoff invalida o cache para o usuário autenticado.</span><span class="sxs-lookup"><span data-stu-id="e2b56-195">When using `vary-by-user="true"`, a log-in and log-out action invalidates the cache for the authenticated user.</span></span>  <span data-ttu-id="e2b56-196">O cache é invalidado porque um novo valor de cookie exclusivo é gerado no logon.</span><span class="sxs-lookup"><span data-stu-id="e2b56-196">The cache is invalidated because a new unique cookie value is generated on login.</span></span> <span data-ttu-id="e2b56-197">Cache é mantido para o estado anônimo quando nenhum cookie está presente ou expirou.</span><span class="sxs-lookup"><span data-stu-id="e2b56-197">Cache is maintained for the anonymous state when no cookie is present or has expired.</span></span> <span data-ttu-id="e2b56-198">Isso significa que se nenhum usuário estiver conectado, o cache será mantido.</span><span class="sxs-lookup"><span data-stu-id="e2b56-198">This means if no user is logged in, the cache will be maintained.</span></span>

- - -

### <a name="vary-by"></a><span data-ttu-id="e2b56-199">variar por</span><span class="sxs-lookup"><span data-stu-id="e2b56-199">vary-by</span></span>

| <span data-ttu-id="e2b56-200">Tipo de atributo</span><span class="sxs-lookup"><span data-stu-id="e2b56-200">Attribute Type</span></span>    | <span data-ttu-id="e2b56-201">Valores de exemplo</span><span class="sxs-lookup"><span data-stu-id="e2b56-201">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="e2b56-202">Cadeia de caracteres</span><span class="sxs-lookup"><span data-stu-id="e2b56-202">String</span></span>             | <span data-ttu-id="e2b56-203">" @Model "</span><span class="sxs-lookup"><span data-stu-id="e2b56-203">"@Model"</span></span>                 |


<span data-ttu-id="e2b56-204">Permite a personalização de dados que é armazenado em cache.</span><span class="sxs-lookup"><span data-stu-id="e2b56-204">Allows for customization of what data gets cached.</span></span> <span data-ttu-id="e2b56-205">Quando o objeto referenciado por alterações de valor de cadeia de caracteres do atributo, o conteúdo do auxiliar de marca de Cache é atualizado.</span><span class="sxs-lookup"><span data-stu-id="e2b56-205">When the object referenced by the attribute's string value changes, the content of the Cache Tag Helper is updated.</span></span> <span data-ttu-id="e2b56-206">Geralmente uma concatenação de cadeia de caracteres de valores de modelo são atribuídos a este atributo.</span><span class="sxs-lookup"><span data-stu-id="e2b56-206">Often a string-concatenation of model values are assigned to this attribute.</span></span>  <span data-ttu-id="e2b56-207">Na verdade, isso significa que uma atualização para qualquer um dos valores concatenados invalida o cache.</span><span class="sxs-lookup"><span data-stu-id="e2b56-207">Effectively, that means an update to any of the concatenated values invalidates the cache.</span></span>

<span data-ttu-id="e2b56-208">O exemplo a seguir supõe que o método do controlador renderização as somas de exibir o valor de inteiro de dois parâmetros de rota, `myParam1` e `myParam2`e retorna que como a propriedade de modelo único.</span><span class="sxs-lookup"><span data-stu-id="e2b56-208">The following example assumes the controller method rendering the view sums the integer value of the two route parameters, `myParam1` and `myParam2`, and returns that as the single model property.</span></span> <span data-ttu-id="e2b56-209">Quando essa soma é alterado, o conteúdo do auxiliar de marca de Cache é renderizado e armazenado em cache novamente.</span><span class="sxs-lookup"><span data-stu-id="e2b56-209">When this sum changes, the content of the Cache Tag Helper is rendered and cached again.</span></span>  

<span data-ttu-id="e2b56-210">Exemplo:</span><span class="sxs-lookup"><span data-stu-id="e2b56-210">Example:</span></span>

<span data-ttu-id="e2b56-211">Ação:</span><span class="sxs-lookup"><span data-stu-id="e2b56-211">Action:</span></span>

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

<span data-ttu-id="e2b56-212">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="e2b56-212">*Index.cshtml*</span></span>

```cshtml
<Cache vary-by="@Model"">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="priority"></a><span data-ttu-id="e2b56-213">priority</span><span class="sxs-lookup"><span data-stu-id="e2b56-213">priority</span></span>

| <span data-ttu-id="e2b56-214">Tipo de atributo</span><span class="sxs-lookup"><span data-stu-id="e2b56-214">Attribute Type</span></span>    | <span data-ttu-id="e2b56-215">Valores de exemplo</span><span class="sxs-lookup"><span data-stu-id="e2b56-215">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="e2b56-216">CacheItemPriority</span><span class="sxs-lookup"><span data-stu-id="e2b56-216">CacheItemPriority</span></span>  | <span data-ttu-id="e2b56-217">"Alta"</span><span class="sxs-lookup"><span data-stu-id="e2b56-217">"High"</span></span>                   |
|                    | <span data-ttu-id="e2b56-218">"Baixo"</span><span class="sxs-lookup"><span data-stu-id="e2b56-218">"Low"</span></span> |
|                    | <span data-ttu-id="e2b56-219">"NeverRemove"</span><span class="sxs-lookup"><span data-stu-id="e2b56-219">"NeverRemove"</span></span> |
|                    | <span data-ttu-id="e2b56-220">"Normal"</span><span class="sxs-lookup"><span data-stu-id="e2b56-220">"Normal"</span></span> |

<span data-ttu-id="e2b56-221">Fornece orientação de remoção de cache para o provedor de cache interno.</span><span class="sxs-lookup"><span data-stu-id="e2b56-221">Provides cache eviction guidance to the built-in cache provider.</span></span> <span data-ttu-id="e2b56-222">O servidor web irá remover `Low` entradas de cache primeiro quando ele estiver sob pressão de memória.</span><span class="sxs-lookup"><span data-stu-id="e2b56-222">The web server will evict `Low` cache entries first when it's under memory pressure.</span></span>

<span data-ttu-id="e2b56-223">Exemplo:</span><span class="sxs-lookup"><span data-stu-id="e2b56-223">Example:</span></span>

```cshtml
<Cache priority="High">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

<span data-ttu-id="e2b56-224">O `priority` atributo não garante que um nível específico de retenção de cache.</span><span class="sxs-lookup"><span data-stu-id="e2b56-224">The `priority` attribute does not guarantee a specific level of cache retention.</span></span> <span data-ttu-id="e2b56-225">`CacheItemPriority`é apenas uma sugestão.</span><span class="sxs-lookup"><span data-stu-id="e2b56-225">`CacheItemPriority` is only a suggestion.</span></span> <span data-ttu-id="e2b56-226">A configuração desse atributo `NeverRemove` não garante que o cache sempre será retido.</span><span class="sxs-lookup"><span data-stu-id="e2b56-226">Setting this attribute to `NeverRemove` does not guarantee that the cache will always be retained.</span></span> <span data-ttu-id="e2b56-227">Consulte [recursos adicionais](#additional-resources) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="e2b56-227">See [Additional Resources](#additional-resources) for more information.</span></span>

<span data-ttu-id="e2b56-228">O auxiliar de marca de Cache é dependente de [serviço de cache de memória](xref:performance/caching/memory).</span><span class="sxs-lookup"><span data-stu-id="e2b56-228">The Cache Tag Helper is dependent on the [memory cache service](xref:performance/caching/memory).</span></span> <span data-ttu-id="e2b56-229">O auxiliar de marca de Cache adiciona o serviço se não tiver sido adicionado.</span><span class="sxs-lookup"><span data-stu-id="e2b56-229">The Cache Tag Helper adds the service if it has not been added.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e2b56-230">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="e2b56-230">Additional resources</span></span>

* <xref:performance/caching/memory>
* <xref:security/authentication/identity>

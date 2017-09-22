---
title: O cache de resposta
author: rick-anderson
description: Explica como usar o cache para reduzir a largura de banda e melhorar o desempenho de resposta.
keywords: "ASP.NET Core, cache, cabeçalhos HTTP de resposta"
ms.author: riande
manager: wpickett
ms.date: 07/10/2017
ms.topic: article
ms.assetid: cb42035a-60b0-472e-a614-cb79f443f654
ms.prod: asp.net-core
uid: performance/caching/response
ms.openlocfilehash: af114401d2f6f183291caba3c015359afb737d93
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/22/2017
---
# <a name="response-caching"></a><span data-ttu-id="0ee19-104">O cache de resposta</span><span class="sxs-lookup"><span data-stu-id="0ee19-104">Response Caching</span></span>

<span data-ttu-id="0ee19-105">Por [John Luo](https://github.com/JunTaoLuo), [Rick Anderson](https://twitter.com/RickAndMSFT), e [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="0ee19-105">By [John Luo](https://github.com/JunTaoLuo), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Steve Smith](https://ardalis.com/)</span></span>

[<span data-ttu-id="0ee19-106">Exibir ou baixar o código de exemplo</span><span class="sxs-lookup"><span data-stu-id="0ee19-106">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/response/sample)

## <a name="what-is-response-caching"></a><span data-ttu-id="0ee19-107">O que é o cache de resposta</span><span class="sxs-lookup"><span data-stu-id="0ee19-107">What is Response Caching</span></span>

<span data-ttu-id="0ee19-108">*O cache de resposta* adiciona cabeçalhos relacionados às respostas.</span><span class="sxs-lookup"><span data-stu-id="0ee19-108">*Response caching* adds cache-related headers to responses.</span></span> <span data-ttu-id="0ee19-109">Esses cabeçalhos especificam como você deseja o cliente, o proxy e o middleware para respostas de cache.</span><span class="sxs-lookup"><span data-stu-id="0ee19-109">These headers specify how you want client, proxy and middleware to cache responses.</span></span> <span data-ttu-id="0ee19-110">O cache de resposta pode reduzir o número de solicitações que faz a um cliente ou um proxy para o servidor web.</span><span class="sxs-lookup"><span data-stu-id="0ee19-110">Response caching can reduce the number of requests a client or proxy makes to the web server.</span></span> <span data-ttu-id="0ee19-111">O cache de resposta também pode reduzir a quantidade de trabalho do servidor web executa para gerar a resposta.</span><span class="sxs-lookup"><span data-stu-id="0ee19-111">Response caching can also reduce the amount of work the web server performs to generate the response.</span></span> 

<span data-ttu-id="0ee19-112">O cabeçalho HTTP principal usado para armazenar em cache é `Cache-Control`.</span><span class="sxs-lookup"><span data-stu-id="0ee19-112">The primary HTTP header used for caching is `Cache-Control`.</span></span> <span data-ttu-id="0ee19-113">Consulte o [HTTP 1.1 cache](https://tools.ietf.org/html/rfc7234#section-5.2) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="0ee19-113">See the [HTTP 1.1 Caching](https://tools.ietf.org/html/rfc7234#section-5.2) for more information.</span></span> <span data-ttu-id="0ee19-114">Diretivas de cache comuns:</span><span class="sxs-lookup"><span data-stu-id="0ee19-114">Common cache directives:</span></span>

* [<span data-ttu-id="0ee19-115">public</span><span class="sxs-lookup"><span data-stu-id="0ee19-115">public</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.2.5)
* [<span data-ttu-id="0ee19-116">private</span><span class="sxs-lookup"><span data-stu-id="0ee19-116">private</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.2.6)
* [<span data-ttu-id="0ee19-117">sem cache</span><span class="sxs-lookup"><span data-stu-id="0ee19-117">no-cache</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.1.4)
* [<span data-ttu-id="0ee19-118">Pragma</span><span class="sxs-lookup"><span data-stu-id="0ee19-118">Pragma</span></span>](https://tools.ietf.org/html/rfc7234#section-5.4)
* [<span data-ttu-id="0ee19-119">Variar</span><span class="sxs-lookup"><span data-stu-id="0ee19-119">Vary</span></span>](https://tools.ietf.org/html/rfc7231#section-7.1.4)

<span data-ttu-id="0ee19-120">O servidor web pode armazenar em cache respostas adicionando a resposta do cache de middleware.</span><span class="sxs-lookup"><span data-stu-id="0ee19-120">The web server can cache responses by adding the response caching middleware.</span></span> <span data-ttu-id="0ee19-121">Consulte [resposta cache middleware](middleware.md) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="0ee19-121">See [Response caching middleware](middleware.md) for more information.</span></span>

## <a name="distributed-cache-tag-helper"></a><span data-ttu-id="0ee19-122">Auxiliar de marca de Cache distribuído</span><span class="sxs-lookup"><span data-stu-id="0ee19-122">Distributed Cache Tag Helper</span></span>

<span data-ttu-id="0ee19-123">O [auxiliar de marca de Cache distribuído](xref:mvc/views/tag-helpers/builtin-th/DistributedCacheTagHelper) Habilita cache distribuído.</span><span class="sxs-lookup"><span data-stu-id="0ee19-123">The [Distributed Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/DistributedCacheTagHelper) enables distributed caching.</span></span>


## <a name="responsecache-attribute"></a><span data-ttu-id="0ee19-124">Atributo ResponseCache</span><span class="sxs-lookup"><span data-stu-id="0ee19-124">ResponseCache Attribute</span></span>

<span data-ttu-id="0ee19-125">O `ResponseCacheAttribute` Especifica os parâmetros necessários para definir os cabeçalhos apropriados no cache de resposta.</span><span class="sxs-lookup"><span data-stu-id="0ee19-125">The `ResponseCacheAttribute` specifies the parameters necessary for setting appropriate headers in response caching.</span></span> <span data-ttu-id="0ee19-126">Consulte [ResponseCacheAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.responsecacheattribute) para obter uma descrição dos parâmetros.</span><span class="sxs-lookup"><span data-stu-id="0ee19-126">See [ResponseCacheAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.responsecacheattribute)  for a description of the parameters.</span></span>

>[!WARNING]
> <span data-ttu-id="0ee19-127">Desabilite o cache de conteúdo que contém informações para clientes autenticados.</span><span class="sxs-lookup"><span data-stu-id="0ee19-127">Disable caching for content that contains information for authenticated clients.</span></span> <span data-ttu-id="0ee19-128">Cache só deve ser habilitado para conteúdo que não muda com base na identidade do usuário, ou se um usuário está conectado.</span><span class="sxs-lookup"><span data-stu-id="0ee19-128">Caching should only be enabled for content that does not change based on a user's identity, or whether a user is logged in.</span></span>

<span data-ttu-id="0ee19-129">`VaryByQueryKeys string[]`(requer o ASP.NET Core 1.1.0 e superior): quando definido, a resposta do cache de middleware irão variar a resposta armazenada pelos valores de determinada lista de chaves de consulta.</span><span class="sxs-lookup"><span data-stu-id="0ee19-129">`VaryByQueryKeys string[]` (requires ASP.NET Core 1.1.0 and higher): When set, the response caching middleware will vary the stored response by the values of the given list of query keys.</span></span> <span data-ttu-id="0ee19-130">A resposta do cache de middleware deve ser habilitada para definir o `VaryByQueryKeys` propriedade, caso contrário, uma exceção de tempo de execução será gerada.</span><span class="sxs-lookup"><span data-stu-id="0ee19-130">The response caching middleware must be enabled to set the `VaryByQueryKeys` property, otherwise a runtime exception will be thrown.</span></span> <span data-ttu-id="0ee19-131">Não há nenhum cabeçalho HTTP correspondente para o `VaryByQueryKeys` propriedade.</span><span class="sxs-lookup"><span data-stu-id="0ee19-131">There is no corresponding HTTP header for the `VaryByQueryKeys` property.</span></span> <span data-ttu-id="0ee19-132">Essa propriedade é um recurso HTTP tratado pela resposta de cache de middleware.</span><span class="sxs-lookup"><span data-stu-id="0ee19-132">This property is an HTTP feature handled by the response caching middleware.</span></span> <span data-ttu-id="0ee19-133">Para o middleware servir uma resposta em cache, a cadeia de caracteres de consulta e o valor de cadeia de caracteres de consulta devem corresponder uma solicitação anterior.</span><span class="sxs-lookup"><span data-stu-id="0ee19-133">For the middleware to serve a cached response, the query string and query string value must match a previous request.</span></span> <span data-ttu-id="0ee19-134">Por exemplo, considere a seguinte sequência:</span><span class="sxs-lookup"><span data-stu-id="0ee19-134">For example, consider the following sequence:</span></span>

| <span data-ttu-id="0ee19-135">Solicitação</span><span class="sxs-lookup"><span data-stu-id="0ee19-135">Request</span></span>          | <span data-ttu-id="0ee19-136">Resultado</span><span class="sxs-lookup"><span data-stu-id="0ee19-136">Result</span></span> |
| ----------------- | ------------ | 
| `http://example.com?key1=value1` | <span data-ttu-id="0ee19-137">retornado do servidor</span><span class="sxs-lookup"><span data-stu-id="0ee19-137">returned from server</span></span> |
| `http://example.com?key1=value1` | <span data-ttu-id="0ee19-138">retornado de middleware</span><span class="sxs-lookup"><span data-stu-id="0ee19-138">returned from middleware</span></span> |
| `http://example.com?key1=value2` | <span data-ttu-id="0ee19-139">retornado do servidor</span><span class="sxs-lookup"><span data-stu-id="0ee19-139">returned from server</span></span> |

<span data-ttu-id="0ee19-140">A primeira solicitação é retornada pelo servidor e armazenados em cache no middleware.</span><span class="sxs-lookup"><span data-stu-id="0ee19-140">The first request is returned by the server and cached in middleware.</span></span> <span data-ttu-id="0ee19-141">A segunda solicitação é retornada pelo middleware porque a cadeia de caracteres de consulta corresponde a solicitação anterior.</span><span class="sxs-lookup"><span data-stu-id="0ee19-141">The second request is returned by middleware because the query string matches the previous request.</span></span> <span data-ttu-id="0ee19-142">A terceira solicitação não está no cache de middleware porque o valor de cadeia de caracteres de consulta não corresponde a uma solicitação anterior.</span><span class="sxs-lookup"><span data-stu-id="0ee19-142">The third request is not in the middleware cache because the query string value doesn't match a previous request.</span></span> 

<span data-ttu-id="0ee19-143">O `ResponseCacheAttribute` é usado para configurar e criar (por meio de `IFilterFactory`) um `ResponseCacheFilter`.</span><span class="sxs-lookup"><span data-stu-id="0ee19-143">The `ResponseCacheAttribute` is used to configure and create (via `IFilterFactory`) a `ResponseCacheFilter`.</span></span> <span data-ttu-id="0ee19-144">O `ResponseCacheFilter` realiza o trabalho de atualização de cabeçalhos HTTP apropriados e os recursos da resposta.</span><span class="sxs-lookup"><span data-stu-id="0ee19-144">The `ResponseCacheFilter` performs the work of updating the appropriate HTTP headers and features of the response.</span></span> <span data-ttu-id="0ee19-145">O filtro:</span><span class="sxs-lookup"><span data-stu-id="0ee19-145">The filter:</span></span>

* <span data-ttu-id="0ee19-146">Remove qualquer cabeçalho existente para `Vary`, `Cache-Control`, e `Pragma`.</span><span class="sxs-lookup"><span data-stu-id="0ee19-146">Removes any existing headers for `Vary`, `Cache-Control`, and `Pragma`.</span></span> 
* <span data-ttu-id="0ee19-147">Grava os cabeçalhos apropriados com base nas propriedades definidas no `ResponseCacheAttribute`.</span><span class="sxs-lookup"><span data-stu-id="0ee19-147">Writes out the appropriate headers based on the properties set in the `ResponseCacheAttribute`.</span></span> 
* <span data-ttu-id="0ee19-148">Atualiza a resposta de armazenamento em cache o recurso HTTP se `VaryByQueryKeys` está definido.</span><span class="sxs-lookup"><span data-stu-id="0ee19-148">Updates the response caching HTTP feature if `VaryByQueryKeys` is set.</span></span>

### <a name="vary"></a><span data-ttu-id="0ee19-149">Variar</span><span class="sxs-lookup"><span data-stu-id="0ee19-149">Vary</span></span>

<span data-ttu-id="0ee19-150">Esse cabeçalho é apenas gravado quando o `VaryByHeader` está definida.</span><span class="sxs-lookup"><span data-stu-id="0ee19-150">This header is only written when the `VaryByHeader` property is set.</span></span> <span data-ttu-id="0ee19-151">Ele é definido como o `Vary` valor da propriedade.</span><span class="sxs-lookup"><span data-stu-id="0ee19-151">It is set to the `Vary` property's value.</span></span> <span data-ttu-id="0ee19-152">O exemplo a seguir usa o `VaryByHeader` propriedade.</span><span class="sxs-lookup"><span data-stu-id="0ee19-152">The following sample uses the `VaryByHeader` property.</span></span>

[!code-csharp[Main](response/sample/Controllers/HomeController.cs?name=snippet_VaryByHeader&highlight=1)]

<span data-ttu-id="0ee19-153">Você pode exibir os cabeçalhos de resposta com as ferramentas de rede de navegadores.</span><span class="sxs-lookup"><span data-stu-id="0ee19-153">You can view the response headers with your browsers network tools.</span></span> <span data-ttu-id="0ee19-154">A imagem a seguir mostra o F12 borda que saída o **rede** guia quando o `About2` método de ação é atualizado.</span><span class="sxs-lookup"><span data-stu-id="0ee19-154">The following image shows the Edge F12 output on the **Network** tab when the `About2` action method is refreshed.</span></span> 

![Borda F12 saída no * * * * de rede guia quando é chamado o método de ação 'About2'](response/_static/vary.png)

### <a name="nostore-and-locationnone"></a><span data-ttu-id="0ee19-156">NoStore e Location.None</span><span class="sxs-lookup"><span data-stu-id="0ee19-156">NoStore and Location.None</span></span>

<span data-ttu-id="0ee19-157">`NoStore`substitui a maioria das outras propriedades.</span><span class="sxs-lookup"><span data-stu-id="0ee19-157">`NoStore` overrides most of the other properties.</span></span> <span data-ttu-id="0ee19-158">Quando essa propriedade é definida como `true`, o `Cache-Control` cabeçalho será definido como "nenhum repositório".</span><span class="sxs-lookup"><span data-stu-id="0ee19-158">When this property is set to `true`, the `Cache-Control` header will be set to "no-store".</span></span> <span data-ttu-id="0ee19-159">Se `Location` é definido como `None`:</span><span class="sxs-lookup"><span data-stu-id="0ee19-159">If `Location` is set to `None`:</span></span>

* <span data-ttu-id="0ee19-160">`Cache-Control` é definido como `"no-store, no-cache"`.</span><span class="sxs-lookup"><span data-stu-id="0ee19-160">`Cache-Control` is set to `"no-store, no-cache"`.</span></span> 
* <span data-ttu-id="0ee19-161">`Pragma` é definido como `no-cache`.</span><span class="sxs-lookup"><span data-stu-id="0ee19-161">`Pragma` is set to `no-cache`.</span></span> 

<span data-ttu-id="0ee19-162">Se `NoStore` é `false` e `Location` é `None`, `Cache-Control` e `Pragma` será definida como `no-cache`.</span><span class="sxs-lookup"><span data-stu-id="0ee19-162">If `NoStore` is `false` and `Location` is `None`,  `Cache-Control` and `Pragma` will be set to `no-cache`.</span></span>

<span data-ttu-id="0ee19-163">Você normalmente define `NoStore` para `true` nas páginas de erro.</span><span class="sxs-lookup"><span data-stu-id="0ee19-163">You typically set `NoStore` to `true` on error pages.</span></span> <span data-ttu-id="0ee19-164">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="0ee19-164">For example:</span></span>

[!code-csharp[Main](response/sample/Controllers/HomeController.cs?name=snippet1&highlight=1)]

<span data-ttu-id="0ee19-165">Isso fará com que os seguintes cabeçalhos:</span><span class="sxs-lookup"><span data-stu-id="0ee19-165">This will result in the following headers:</span></span>

```javascript
Cache-Control: no-store,no-cache
Pragma: no-cache
```

### <a name="location-and-duration"></a><span data-ttu-id="0ee19-166">Local e duração</span><span class="sxs-lookup"><span data-stu-id="0ee19-166">Location and Duration</span></span>

<span data-ttu-id="0ee19-167">Para habilitar o cache, `Duration` deve ser definido como um valor positivo e `Location` devem ser `Any` (o padrão) ou `Client`.</span><span class="sxs-lookup"><span data-stu-id="0ee19-167">To enable caching, `Duration` must be set to a positive value and `Location` must be either `Any` (the default) or `Client`.</span></span> <span data-ttu-id="0ee19-168">Nesse caso, o `Cache-Control` cabeçalho será definido como o valor do local seguido de "max-age" da resposta.</span><span class="sxs-lookup"><span data-stu-id="0ee19-168">In this case, the `Cache-Control` header will be set to the location value followed by the "max-age" of the response.</span></span>

> [!NOTE]
> <span data-ttu-id="0ee19-169">`Location`do opções de `Any` e `Client` se traduz em `Cache-Control` valores de cabeçalho de `public` e `private`, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="0ee19-169">`Location`'s options of `Any` and `Client` translate into `Cache-Control` header values of `public` and `private`, respectively.</span></span> <span data-ttu-id="0ee19-170">Conforme observado anteriormente, definindo `Location` para `None` definirá ambos `Cache-Control` e `Pragma` cabeçalhos para `no-cache`.</span><span class="sxs-lookup"><span data-stu-id="0ee19-170">As noted previously, setting `Location` to `None` will set both `Cache-Control` and `Pragma` headers to `no-cache`.</span></span>

<span data-ttu-id="0ee19-171">Abaixo está um exemplo que mostra os cabeçalhos produzido definindo `Duration` e deixar o padrão `Location` valor.</span><span class="sxs-lookup"><span data-stu-id="0ee19-171">Below is an example showing the headers produced by setting `Duration` and leaving the default `Location` value.</span></span>

[!code-csharp[Main](response/sample/Controllers/HomeController.cs?name=snippet_duration&highlight=1)]

<span data-ttu-id="0ee19-172">Produz os seguintes cabeçalhos:</span><span class="sxs-lookup"><span data-stu-id="0ee19-172">Produces the following headers:</span></span>

```javascript
Cache-Control: public,max-age=60
   ```

### <a name="cache-profiles"></a><span data-ttu-id="0ee19-173">Perfis de cache</span><span class="sxs-lookup"><span data-stu-id="0ee19-173">Cache Profiles</span></span>

<span data-ttu-id="0ee19-174">Em vez de duplicar `ResponseCache` configurações em muitos atributos de ação do controlador, perfis de cache podem ser configuradas como opções ao configurar MVC no `ConfigureServices` método `Startup`.</span><span class="sxs-lookup"><span data-stu-id="0ee19-174">Instead of duplicating `ResponseCache` settings on many controller action attributes, cache profiles can be configured as options when setting up MVC in the `ConfigureServices` method in `Startup`.</span></span> <span data-ttu-id="0ee19-175">Valores encontrados em um perfil de cache referenciado serão usados como padrão pelo `ResponseCache` de atributos e será substituído por quaisquer propriedades especificadas no atributo.</span><span class="sxs-lookup"><span data-stu-id="0ee19-175">Values found in a referenced cache profile will be used as the defaults by the `ResponseCache` attribute, and will be overridden by any properties specified on the attribute.</span></span>

<span data-ttu-id="0ee19-176">Configurando um perfil de cache:</span><span class="sxs-lookup"><span data-stu-id="0ee19-176">Setting up a cache profile:</span></span>

[!code-csharp[Main](response/sample/Startup.cs?name=snippet1)] 

<span data-ttu-id="0ee19-177">Um perfil de cache de referência:</span><span class="sxs-lookup"><span data-stu-id="0ee19-177">Referencing a cache profile:</span></span>

[!code-csharp[Main](response/sample/Controllers/HomeController.cs?name=snippet_controller&highlight=1,4)]

<span data-ttu-id="0ee19-178">O `ResponseCache` atributo pode ser aplicado a ações (métodos), bem como controladores (classes).</span><span class="sxs-lookup"><span data-stu-id="0ee19-178">The `ResponseCache` attribute can be applied both to actions (methods) as well as controllers (classes).</span></span> <span data-ttu-id="0ee19-179">Atributos de nível de método substituirão as configurações especificadas em atributos de nível de classe.</span><span class="sxs-lookup"><span data-stu-id="0ee19-179">Method-level attributes will override the settings specified in class-level attributes.</span></span>

<span data-ttu-id="0ee19-180">No exemplo acima, um atributo de nível de classe especifica uma duração de 30 segundos, enquanto um atributos de nível de método faz referência a um perfil de cache com uma duração definida como 60 segundos.</span><span class="sxs-lookup"><span data-stu-id="0ee19-180">In the above example, a class-level attribute specifies a duration of 30 seconds while a method-level attributes references a cache profile with a duration set to 60 seconds.</span></span>

<span data-ttu-id="0ee19-181">O cabeçalho resultante:</span><span class="sxs-lookup"><span data-stu-id="0ee19-181">The resulting header:</span></span>

```
Cache-Control: public,max-age=60
   ```

  ### <a name="additional-resources"></a><span data-ttu-id="0ee19-182">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="0ee19-182">Additional Resources</span></span>

* [<span data-ttu-id="0ee19-183">Armazenamento em cache em HTTP da especificação</span><span class="sxs-lookup"><span data-stu-id="0ee19-183">Caching in HTTP from the specification</span></span>](https://tools.ietf.org/html/rfc7234#section-3)
* [<span data-ttu-id="0ee19-184">Controle de cache</span><span class="sxs-lookup"><span data-stu-id="0ee19-184">Cache-Control</span></span>](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)

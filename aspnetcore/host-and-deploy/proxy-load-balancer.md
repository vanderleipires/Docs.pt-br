---
title: Configurar o ASP.NET Core para trabalhar com servidores proxy e balanceadores de carga
author: guardrex
description: Saiba mais sobre a configuração para aplicativos hospedados por trás de servidores proxy e balanceadores de carga, que geralmente podem ocultar informações importantes de solicitação.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 03/26/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/proxy-load-balancer
ms.openlocfilehash: b153a7406ae1b31a2aa453135c6bd0e5ce0b2997
ms.sourcegitcommit: d45d766504c2c5aad2453f01f089bc6b696b5576
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/30/2018
---
# <a name="configure-aspnet-core-to-work-with-proxy-servers-and-load-balancers"></a><span data-ttu-id="eef7f-103">Configurar o ASP.NET Core para trabalhar com servidores proxy e balanceadores de carga</span><span class="sxs-lookup"><span data-stu-id="eef7f-103">Configure ASP.NET Core to work with proxy servers and load balancers</span></span>

<span data-ttu-id="eef7f-104">Por [Luke Latham](https://github.com/guardrex) e [Ross Carlos](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="eef7f-104">By [Luke Latham](https://github.com/guardrex) and [Chris Ross](https://github.com/Tratcher)</span></span>

<span data-ttu-id="eef7f-105">Na configuração recomendada para o ASP.NET Core, o aplicativo é hospedado usando o módulo de núcleo IIS/ASP.NET, Nginx ou Apache.</span><span class="sxs-lookup"><span data-stu-id="eef7f-105">In the recommended configuration for ASP.NET Core, the app is hosted using IIS/ASP.NET Core Module, Nginx, or Apache.</span></span> <span data-ttu-id="eef7f-106">Servidores proxy, balanceadores de carga e outros dispositivos de rede geralmente ocultar informações sobre a solicitação antes de alcançar o aplicativo:</span><span class="sxs-lookup"><span data-stu-id="eef7f-106">Proxy servers, load balancers, and other network appliances often obscure information about the request before it reaches the app:</span></span>

* <span data-ttu-id="eef7f-107">Quando solicitações HTTPS são delegadas sobre HTTP, o esquema original (HTTPS) é perdido e deve ser encaminhado em um cabeçalho.</span><span class="sxs-lookup"><span data-stu-id="eef7f-107">When HTTPS requests are proxied over HTTP, the original scheme (HTTPS) is lost and must be forwarded in a header.</span></span>
* <span data-ttu-id="eef7f-108">Como um aplicativo recebe uma solicitação de proxy e não sua fonte verdadeira na Internet ou rede corporativa, o endereço IP do cliente de origem também deve ser encaminhado em um cabeçalho.</span><span class="sxs-lookup"><span data-stu-id="eef7f-108">Because an app receives a request from the proxy and not its true source on the Internet or corporate network, the originating client IP address must also be forwarded in a header.</span></span>

<span data-ttu-id="eef7f-109">Essas informações podem ser importantes no processamento de solicitações, por exemplo na redirecionamentos, autenticação, geração de link, avaliação de política e geoloation do cliente.</span><span class="sxs-lookup"><span data-stu-id="eef7f-109">This information may be important in request processing, for example in redirects, authentication, link generation, policy evaluation, and client geoloation.</span></span>

## <a name="forwarded-headers"></a><span data-ttu-id="eef7f-110">Cabeçalhos encaminhados</span><span class="sxs-lookup"><span data-stu-id="eef7f-110">Forwarded headers</span></span>

<span data-ttu-id="eef7f-111">Por convenção, os proxies encaminham informações em cabeçalhos HTTP.</span><span class="sxs-lookup"><span data-stu-id="eef7f-111">By convention, proxies forward information in HTTP headers.</span></span>

| <span data-ttu-id="eef7f-112">Cabeçalho</span><span class="sxs-lookup"><span data-stu-id="eef7f-112">Header</span></span> | <span data-ttu-id="eef7f-113">Descrição</span><span class="sxs-lookup"><span data-stu-id="eef7f-113">Description</span></span> |
| ------ | ----------- |
| <span data-ttu-id="eef7f-114">X-Forwarded-For</span><span class="sxs-lookup"><span data-stu-id="eef7f-114">X-Forwarded-For</span></span> | <span data-ttu-id="eef7f-115">Contém informações sobre o cliente que iniciou a solicitação e proxies subsequentes em uma cadeia de proxies.</span><span class="sxs-lookup"><span data-stu-id="eef7f-115">Holds information about the client that initiated the request and subsequent proxies in a chain of proxies.</span></span> <span data-ttu-id="eef7f-116">Esse parâmetro pode conter IP endereços (e, opcionalmente, os números de porta).</span><span class="sxs-lookup"><span data-stu-id="eef7f-116">This parameter may contain IP addresses (and, optionally, port numbers).</span></span> <span data-ttu-id="eef7f-117">Em uma cadeia de servidores proxy, o primeiro parâmetro indica o cliente em que a solicitação foi feita pela primeira vez.</span><span class="sxs-lookup"><span data-stu-id="eef7f-117">In a chain of proxy servers, the first parameter indicates the client where the request was first made.</span></span> <span data-ttu-id="eef7f-118">Siga os identificadores de proxy subsequentes.</span><span class="sxs-lookup"><span data-stu-id="eef7f-118">Subsequent proxy identifiers follow.</span></span> <span data-ttu-id="eef7f-119">O último proxy na cadeia não está na lista de parâmetros.</span><span class="sxs-lookup"><span data-stu-id="eef7f-119">The last proxy in the chain isn't in the list of parameters.</span></span> <span data-ttu-id="eef7f-120">Endereço IP do proxy última e, opcionalmente, um número de porta estão disponíveis como o endereço IP remoto na camada de transporte.</span><span class="sxs-lookup"><span data-stu-id="eef7f-120">The last proxy's IP address, and optionally a port number, are available as the remote IP address at the transport layer.</span></span> |
| <span data-ttu-id="eef7f-121">X-Forwarded-Proto</span><span class="sxs-lookup"><span data-stu-id="eef7f-121">X-Forwarded-Proto</span></span> | <span data-ttu-id="eef7f-122">O valor do esquema de origem (HTTP/HTTPS).</span><span class="sxs-lookup"><span data-stu-id="eef7f-122">The value of the originating scheme (HTTP/HTTPS).</span></span> <span data-ttu-id="eef7f-123">O valor também pode ser uma lista de esquemas, se a solicitação percorreu vários proxies.</span><span class="sxs-lookup"><span data-stu-id="eef7f-123">The value may also be a list of schemes if the request has traversed multiple proxies.</span></span> |
| <span data-ttu-id="eef7f-124">X-Forwarded-Host</span><span class="sxs-lookup"><span data-stu-id="eef7f-124">X-Forwarded-Host</span></span> | <span data-ttu-id="eef7f-125">O valor original do campo de cabeçalho de Host.</span><span class="sxs-lookup"><span data-stu-id="eef7f-125">The original value of the Host header field.</span></span> <span data-ttu-id="eef7f-126">Normalmente, os proxies não modifique o cabeçalho de Host.</span><span class="sxs-lookup"><span data-stu-id="eef7f-126">Usually, proxies don't modify the Host header.</span></span> <span data-ttu-id="eef7f-127">Consulte [Microsoft Security Advisory CVE-2018-0787](https://github.com/aspnet/Announcements/issues/295) para obter informações sobre uma vulnerabilidade de elevação de privilégios que afeta os sistemas em que o proxy não valida ou restict cabeçalhos de Host para valores de BOM conhecidos.</span><span class="sxs-lookup"><span data-stu-id="eef7f-127">See [Microsoft Security Advisory CVE-2018-0787](https://github.com/aspnet/Announcements/issues/295) for information on an elevation-of-privileges vulnerability that affects systems where the proxy doesn't validate or restict Host headers to known good values.</span></span> |

<span data-ttu-id="eef7f-128">O Middleware de cabeçalhos encaminhados do [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) empacotar, lê esses cabeçalhos e preenche os campos associados em [HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext).</span><span class="sxs-lookup"><span data-stu-id="eef7f-128">The Forwarded Headers Middleware, from the [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) package, reads these headers and fills in the associated fields on [HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext).</span></span> 

<span data-ttu-id="eef7f-129">As atualizações de middleware:</span><span class="sxs-lookup"><span data-stu-id="eef7f-129">The middleware updates:</span></span>

* <span data-ttu-id="eef7f-130">[HttpContext.Connection.RemoteIpAddress](/dotnet/api/microsoft.aspnetcore.http.connectioninfo.remoteipaddress) &ndash; definido usando o `X-Forwarded-For` o valor do cabeçalho.</span><span class="sxs-lookup"><span data-stu-id="eef7f-130">[HttpContext.Connection.RemoteIpAddress](/dotnet/api/microsoft.aspnetcore.http.connectioninfo.remoteipaddress) &ndash; Set using the `X-Forwarded-For` header value.</span></span> <span data-ttu-id="eef7f-131">Configurações adicionais influenciam como o middleware define `RemoteIpAddress`.</span><span class="sxs-lookup"><span data-stu-id="eef7f-131">Additional settings influence how the middleware sets `RemoteIpAddress`.</span></span> <span data-ttu-id="eef7f-132">Para obter detalhes, consulte o [opções de Middleware de cabeçalhos encaminhados](#forwarded-headers-middleware-options).</span><span class="sxs-lookup"><span data-stu-id="eef7f-132">For details, see the [Forwarded Headers Middleware options](#forwarded-headers-middleware-options).</span></span>
* <span data-ttu-id="eef7f-133">[HttpContext.Request.Scheme](/dotnet/api/microsoft.aspnetcore.http.httprequest.scheme) &ndash; definido usando o `X-Forwarded-Proto` o valor do cabeçalho.</span><span class="sxs-lookup"><span data-stu-id="eef7f-133">[HttpContext.Request.Scheme](/dotnet/api/microsoft.aspnetcore.http.httprequest.scheme) &ndash; Set using the `X-Forwarded-Proto` header value.</span></span>
* <span data-ttu-id="eef7f-134">[HttpContext.Request.Host](/dotnet/api/microsoft.aspnetcore.http.httprequest.host) &ndash; definido usando o `X-Forwarded-Host` o valor do cabeçalho.</span><span class="sxs-lookup"><span data-stu-id="eef7f-134">[HttpContext.Request.Host](/dotnet/api/microsoft.aspnetcore.http.httprequest.host) &ndash; Set using the `X-Forwarded-Host` header value.</span></span>

<span data-ttu-id="eef7f-135">Observe que nem todos os dispositivos de rede adicionar o `X-Forwarded-For` e `X-Forwarded-Proto` cabeçalhos sem configuração adicional.</span><span class="sxs-lookup"><span data-stu-id="eef7f-135">Note that not all network appliances add the `X-Forwarded-For` and `X-Forwarded-Proto` headers without additional configuration.</span></span> <span data-ttu-id="eef7f-136">Consulte o guia do fabricante do dispositivo se as solicitações de proxies não tiver esses cabeçalhos quando atingem o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="eef7f-136">Consult your appliance manufacturer's guidance if the proxied requests don't contain these headers when they reach the app.</span></span>

<span data-ttu-id="eef7f-137">Encaminhado cabeçalhos Middleware [configurações padrão](#forwarded-headers-middleware-options) pode ser configurado.</span><span class="sxs-lookup"><span data-stu-id="eef7f-137">Forwarded Headers Middleware [default settings](#forwarded-headers-middleware-options) can be configured.</span></span> <span data-ttu-id="eef7f-138">As configurações padrão são:</span><span class="sxs-lookup"><span data-stu-id="eef7f-138">The default settings are:</span></span>

* <span data-ttu-id="eef7f-139">Há apenas *um proxy* entre o aplicativo e a fonte das solicitações.</span><span class="sxs-lookup"><span data-stu-id="eef7f-139">There is only *one proxy* between the app and the source of the requests.</span></span>
* <span data-ttu-id="eef7f-140">Somente os endereços de loopback são configurados para proxies conhecidos e conhecidos redes.</span><span class="sxs-lookup"><span data-stu-id="eef7f-140">Only loopback addresses are configured for known proxies and known networks.</span></span>

## <a name="iisiis-express-and-aspnet-core-module"></a><span data-ttu-id="eef7f-141">O IIS/IIS Express e o módulo principal do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="eef7f-141">IIS/IIS Express and ASP.NET Core Module</span></span>

<span data-ttu-id="eef7f-142">Encaminhada Middleware de cabeçalhos é habilitado por padrão pelo Middleware de integração do IIS quando o aplicativo é executado por trás do IIS e o módulo do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="eef7f-142">Forwarded Headers Middleware is enabled by default by IIS Integration Middleware when the app is run behind IIS and the ASP.NET Core Module.</span></span> <span data-ttu-id="eef7f-143">Middleware de cabeçalhos encaminhada está ativado para ser executado primeiro no pipeline de middleware com uma configuração restrita específica para o módulo do núcleo do ASP.NET devido a questões de confiança com cabeçalhos encaminhados (por exemplo, [falsificação de IP](https://www.iplocation.net/ip-spoofing)).</span><span class="sxs-lookup"><span data-stu-id="eef7f-143">Forwarded Headers Middleware is activated to run first in the middleware pipeline with a restricted configuration specific to the ASP.NET Core Module due to trust concerns with forwarded headers (for example, [IP spoofing](https://www.iplocation.net/ip-spoofing)).</span></span> <span data-ttu-id="eef7f-144">O middleware está configurado para encaminhar o `X-Forwarded-For` e `X-Forwarded-Proto` cabeçalhos e é restrita a um proxy localhost único.</span><span class="sxs-lookup"><span data-stu-id="eef7f-144">The middleware is configured to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers and is restricted to a single localhost proxy.</span></span> <span data-ttu-id="eef7f-145">Se a configuração adicional é necessária, consulte o [opções de Middleware de cabeçalhos encaminhados](#forwarded-headers-middleware-options).</span><span class="sxs-lookup"><span data-stu-id="eef7f-145">If additional configuration is required, see the [Forwarded Headers Middleware options](#forwarded-headers-middleware-options).</span></span>

## <a name="other-proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="eef7f-146">Outro servidor de proxy e cenários de Balanceador de carga</span><span class="sxs-lookup"><span data-stu-id="eef7f-146">Other proxy server and load balancer scenarios</span></span>

<span data-ttu-id="eef7f-147">Além do uso de Middleware de integração do IIS, encaminhados cabeçalhos Middleware não está habilitado por padrão.</span><span class="sxs-lookup"><span data-stu-id="eef7f-147">Outside of using IIS Integration Middleware, Forwarded Headers Middleware isn't enabled by default.</span></span> <span data-ttu-id="eef7f-148">Encaminhada Middleware de cabeçalhos deve ser habilitado para um aplicativo para processar encaminhado cabeçalhos com [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders).</span><span class="sxs-lookup"><span data-stu-id="eef7f-148">Forwarded Headers Middleware must be enabled for an app to process forwarded headers with [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders).</span></span> <span data-ttu-id="eef7f-149">Depois de habilitar o middleware se nenhum [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) são especificados para o middleware, o padrão [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) são [ForwardedHeaders.None ](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders).</span><span class="sxs-lookup"><span data-stu-id="eef7f-149">After enabling the middleware if no [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) are specified to the middleware, the default [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) are [ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders).</span></span>

<span data-ttu-id="eef7f-150">Configurar o middleware com [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) para encaminhar o `X-Forwarded-For` e `X-Forwarded-Proto` cabeçalhos no `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="eef7f-150">Configure the middleware with [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="eef7f-151">Invocar o [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) método `Startup.Configure` antes de chamar outro middleware:</span><span class="sxs-lookup"><span data-stu-id="eef7f-151">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling other middleware:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();
    
    services.Configure<ForwardedHeadersOptions>(options =>
    {
        options.ForwardedHeaders = 
            ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto;
    });
}

public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseForwardedHeaders();

    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }
    else
    {
        app.UseExceptionHandler("/Home/Error");
    }

    app.UseStaticFiles();
    // In ASP.NET Core 1.x, replace the following line with: app.UseIdentity();
    app.UseAuthentication();
    app.UseMvc();
}
```

> [!NOTE]
> <span data-ttu-id="eef7f-152">Se nenhum [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) são especificadas em `Startup.ConfigureServices` ou diretamente para o método de extensão com [UseForwardedHeaders (IApplicationBuilder, ForwardedHeadersOptions)](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ForwardedHeadersExtensions_UseForwardedHeaders_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_ForwardedHeadersOptions_), o padrão cabeçalhos para encaminhar são [ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders).</span><span class="sxs-lookup"><span data-stu-id="eef7f-152">If no [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) are specified in `Startup.ConfigureServices` or directly to the extension method with [UseForwardedHeaders(IApplicationBuilder, ForwardedHeadersOptions)](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ForwardedHeadersExtensions_UseForwardedHeaders_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_ForwardedHeadersOptions_), the default headers to forward are [ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders).</span></span> <span data-ttu-id="eef7f-153">O [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) propriedade deve ser configurada com os cabeçalhos para encaminhar.</span><span class="sxs-lookup"><span data-stu-id="eef7f-153">The [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) property must be configured with the headers to forward.</span></span>

## <a name="forwarded-headers-middleware-options"></a><span data-ttu-id="eef7f-154">Opções de Middleware cabeçalhos encaminhadas</span><span class="sxs-lookup"><span data-stu-id="eef7f-154">Forwarded Headers Middleware options</span></span>

<span data-ttu-id="eef7f-155">[ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) controlar o comportamento do Middleware encaminhados cabeçalhos:</span><span class="sxs-lookup"><span data-stu-id="eef7f-155">[ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) control the behavior of the Forwarded Headers Middleware:</span></span>

```csharp
services.Configure<ForwardedHeadersOptions>(options =>
{
    options.ForwardLimit = 2;
    options.KnownProxies.Add(IPAddress.Parse("127.0.10.1"));
    options.ForwardedForHeaderName = "X-Forwarded-For-Custom-Header-Name";
});
```

| <span data-ttu-id="eef7f-156">Opção</span><span class="sxs-lookup"><span data-stu-id="eef7f-156">Option</span></span> | <span data-ttu-id="eef7f-157">Descrição</span><span class="sxs-lookup"><span data-stu-id="eef7f-157">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="eef7f-158">ForwardedForHeaderName</span><span class="sxs-lookup"><span data-stu-id="eef7f-158">ForwardedForHeaderName</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedforheadername) | <span data-ttu-id="eef7f-159">Use o cabeçalho especificado por essa propriedade, em vez de um especificado por [ForwardedHeadersDefaults.XForwardedForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedforheadername).</span><span class="sxs-lookup"><span data-stu-id="eef7f-159">Use the header specified by this property instead of the one specified by [ForwardedHeadersDefaults.XForwardedForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedforheadername).</span></span><br><br><span data-ttu-id="eef7f-160">O padrão é `X-Forwarded-For`.</span><span class="sxs-lookup"><span data-stu-id="eef7f-160">The default is `X-Forwarded-For`.</span></span> |
| [<span data-ttu-id="eef7f-161">ForwardedHeaders</span><span class="sxs-lookup"><span data-stu-id="eef7f-161">ForwardedHeaders</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) | <span data-ttu-id="eef7f-162">Identifica qual encaminhadores devem ser processados.</span><span class="sxs-lookup"><span data-stu-id="eef7f-162">Identifies which forwarders should be processed.</span></span> <span data-ttu-id="eef7f-163">Consulte o [ForwardedHeaders Enum](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders) para a lista de campos que se aplicam.</span><span class="sxs-lookup"><span data-stu-id="eef7f-163">See the [ForwardedHeaders Enum](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders) for the list of fields that apply.</span></span> <span data-ttu-id="eef7f-164">Os valores típicos atribuídos a essa propriedade são <code>ForwardedHeaders.XForwardedFor &#124; ForwardedHeaders.XForwardedProto</code>.</span><span class="sxs-lookup"><span data-stu-id="eef7f-164">Typical values assigned to this property are <code>ForwardedHeaders.XForwardedFor &#124; ForwardedHeaders.XForwardedProto</code>.</span></span><br><br><span data-ttu-id="eef7f-165">O valor padrão é [ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders).</span><span class="sxs-lookup"><span data-stu-id="eef7f-165">The default value is [ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders).</span></span> |
| [<span data-ttu-id="eef7f-166">ForwardedHostHeaderName</span><span class="sxs-lookup"><span data-stu-id="eef7f-166">ForwardedHostHeaderName</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedhostheadername) | <span data-ttu-id="eef7f-167">Use o cabeçalho especificado por essa propriedade, em vez de um especificado por [ForwardedHeadersDefaults.XForwardedHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedhostheadername).</span><span class="sxs-lookup"><span data-stu-id="eef7f-167">Use the header specified by this property instead of the one specified by [ForwardedHeadersDefaults.XForwardedHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedhostheadername).</span></span><br><br><span data-ttu-id="eef7f-168">O padrão é `X-Forwarded-Host`.</span><span class="sxs-lookup"><span data-stu-id="eef7f-168">The default is `X-Forwarded-Host`.</span></span> |
| [<span data-ttu-id="eef7f-169">ForwardedProtoHeaderName</span><span class="sxs-lookup"><span data-stu-id="eef7f-169">ForwardedProtoHeaderName</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedprotoheadername) | <span data-ttu-id="eef7f-170">Use o cabeçalho especificado por essa propriedade, em vez de um especificado por [ForwardedHeadersDefaults.XForwardedProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedprotoheadername).</span><span class="sxs-lookup"><span data-stu-id="eef7f-170">Use the header specified by this property instead of the one specified by [ForwardedHeadersDefaults.XForwardedProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedprotoheadername).</span></span><br><br><span data-ttu-id="eef7f-171">O padrão é `X-Forwarded-Proto`.</span><span class="sxs-lookup"><span data-stu-id="eef7f-171">The default is `X-Forwarded-Proto`.</span></span> |
| [<span data-ttu-id="eef7f-172">ForwardLimit</span><span class="sxs-lookup"><span data-stu-id="eef7f-172">ForwardLimit</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardlimit) | <span data-ttu-id="eef7f-173">Limita o número de entradas nos cabeçalhos que são processados.</span><span class="sxs-lookup"><span data-stu-id="eef7f-173">Limits the number of entries in the headers that are processed.</span></span> <span data-ttu-id="eef7f-174">Definido como `null` desabilitar o limite, mas isso só deve ser feito se `KnownProxies` ou `KnownNetworks` estão configurados.</span><span class="sxs-lookup"><span data-stu-id="eef7f-174">Set to `null` to disable the limit, but this should only be done if `KnownProxies` or `KnownNetworks` are configured.</span></span><br><br><span data-ttu-id="eef7f-175">O padrão é 1.</span><span class="sxs-lookup"><span data-stu-id="eef7f-175">The default is 1.</span></span> |
| [<span data-ttu-id="eef7f-176">KnownNetworks</span><span class="sxs-lookup"><span data-stu-id="eef7f-176">KnownNetworks</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.knownnetworks) | <span data-ttu-id="eef7f-177">Intervalos de proxies conhecidos para aceitar encaminhados cabeçalhos de endereços.</span><span class="sxs-lookup"><span data-stu-id="eef7f-177">Address ranges of known proxies to accept forwarded headers from.</span></span> <span data-ttu-id="eef7f-178">Forneça os intervalos IP usando notação de roteamento entre domínios (CIDR).</span><span class="sxs-lookup"><span data-stu-id="eef7f-178">Provide IP ranges using Classless Interdomain Routing (CIDR) notation.</span></span><br><br><span data-ttu-id="eef7f-179">O padrão é um [IList](/dotnet/api/system.collections.generic.ilist-1)\<[IPNetwork](/dotnet/api/microsoft.aspnetcore.httpoverrides.ipnetwork)> que contém uma única entrada para `IPAddress.Loopback`.</span><span class="sxs-lookup"><span data-stu-id="eef7f-179">The default is an [IList](/dotnet/api/system.collections.generic.ilist-1)\<[IPNetwork](/dotnet/api/microsoft.aspnetcore.httpoverrides.ipnetwork)> containing a single entry for `IPAddress.Loopback`.</span></span> |
| [<span data-ttu-id="eef7f-180">KnownProxies</span><span class="sxs-lookup"><span data-stu-id="eef7f-180">KnownProxies</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.knownproxies) | <span data-ttu-id="eef7f-181">Endereços de proxies conhecidos para aceitar cabeçalhos encaminhados do.</span><span class="sxs-lookup"><span data-stu-id="eef7f-181">Addresses of known proxies to accept forwarded headers from.</span></span> <span data-ttu-id="eef7f-182">Use `KnownProxies` especificar o endereço IP corresponde.</span><span class="sxs-lookup"><span data-stu-id="eef7f-182">Use `KnownProxies` to specify exact IP address matches.</span></span><br><br><span data-ttu-id="eef7f-183">O padrão é um [IList](/dotnet/api/system.collections.generic.ilist-1)\<[IPAddress](/dotnet/api/system.net.ipaddress)> que contém uma única entrada para `IPAddress.IPv6Loopback`.</span><span class="sxs-lookup"><span data-stu-id="eef7f-183">The default is an [IList](/dotnet/api/system.collections.generic.ilist-1)\<[IPAddress](/dotnet/api/system.net.ipaddress)> containing a single entry for `IPAddress.IPv6Loopback`.</span></span> |
| [<span data-ttu-id="eef7f-184">OriginalForHeaderName</span><span class="sxs-lookup"><span data-stu-id="eef7f-184">OriginalForHeaderName</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalforheadername) | <span data-ttu-id="eef7f-185">Use o cabeçalho especificado por essa propriedade, em vez de um especificado por [ForwardedHeadersDefaults.XOriginalForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalforheadername).</span><span class="sxs-lookup"><span data-stu-id="eef7f-185">Use the header specified by this property instead of the one specified by [ForwardedHeadersDefaults.XOriginalForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalforheadername).</span></span><br><br><span data-ttu-id="eef7f-186">O padrão é `X-Original-For`.</span><span class="sxs-lookup"><span data-stu-id="eef7f-186">The default is `X-Original-For`.</span></span> |
| [<span data-ttu-id="eef7f-187">OriginalHostHeaderName</span><span class="sxs-lookup"><span data-stu-id="eef7f-187">OriginalHostHeaderName</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalhostheadername) | <span data-ttu-id="eef7f-188">Use o cabeçalho especificado por essa propriedade, em vez de um especificado por [ForwardedHeadersDefaults.XOriginalHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalhostheadername).</span><span class="sxs-lookup"><span data-stu-id="eef7f-188">Use the header specified by this property instead of the one specified by [ForwardedHeadersDefaults.XOriginalHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalhostheadername).</span></span><br><br><span data-ttu-id="eef7f-189">O padrão é `X-Original-Host`.</span><span class="sxs-lookup"><span data-stu-id="eef7f-189">The default is `X-Original-Host`.</span></span> |
| [<span data-ttu-id="eef7f-190">OriginalProtoHeaderName</span><span class="sxs-lookup"><span data-stu-id="eef7f-190">OriginalProtoHeaderName</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalprotoheadername) | <span data-ttu-id="eef7f-191">Use o cabeçalho especificado por essa propriedade, em vez de um especificado por [ForwardedHeadersDefaults.XOriginalProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalprotoheadername).</span><span class="sxs-lookup"><span data-stu-id="eef7f-191">Use the header specified by this property instead of the one specified by [ForwardedHeadersDefaults.XOriginalProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalprotoheadername).</span></span><br><br><span data-ttu-id="eef7f-192">O padrão é `X-Original-Proto`.</span><span class="sxs-lookup"><span data-stu-id="eef7f-192">The default is `X-Original-Proto`.</span></span> |
| [<span data-ttu-id="eef7f-193">RequireHeaderSymmetry</span><span class="sxs-lookup"><span data-stu-id="eef7f-193">RequireHeaderSymmetry</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.requireheadersymmetry) | <span data-ttu-id="eef7f-194">Exigem o número de valores de cabeçalho a serem sincronizados entre o [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) sendo processada.</span><span class="sxs-lookup"><span data-stu-id="eef7f-194">Require the number of header values to be in sync between the [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) being processed.</span></span><br><br><span data-ttu-id="eef7f-195">O padrão no núcleo do ASP.NET 1. x é `true`.</span><span class="sxs-lookup"><span data-stu-id="eef7f-195">The default in ASP.NET Core 1.x is `true`.</span></span> <span data-ttu-id="eef7f-196">O padrão do ASP.NET Core 2.0 ou posterior é `false`.</span><span class="sxs-lookup"><span data-stu-id="eef7f-196">The default in ASP.NET Core 2.0 or later is `false`.</span></span> |

## <a name="scenarios-and-use-cases"></a><span data-ttu-id="eef7f-197">Cenários e casos de uso</span><span class="sxs-lookup"><span data-stu-id="eef7f-197">Scenarios and use cases</span></span>

### <a name="when-it-isnt-possible-to-add-forwarded-headers-and-all-requests-are-secure"></a><span data-ttu-id="eef7f-198">Quando não é possível adicionar encaminhada cabeçalhos e todas as solicitações são seguras</span><span class="sxs-lookup"><span data-stu-id="eef7f-198">When it isn't possible to add forwarded headers and all requests are secure</span></span>

<span data-ttu-id="eef7f-199">Em alguns casos, pode não ser possível adicionar cabeçalhos encaminhados para as solicitações de proxies para o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="eef7f-199">In some cases, it might not be possible to add forwarded headers to the requests proxied to the app.</span></span> <span data-ttu-id="eef7f-200">Se o proxy é impor que todas as solicitações externas públicas são HTTPS, o esquema pode ser definido manualmente em `Startup.Configure` antes de usar qualquer tipo de middleware:</span><span class="sxs-lookup"><span data-stu-id="eef7f-200">If the proxy is enforcing that all public external requests are HTTPS, the scheme can be manually set in `Startup.Configure` before using any type of middleware:</span></span>

```csharp
app.Use((context, next) =>
{
    context.Request.Scheme = "https";
    return next();
});
```

<span data-ttu-id="eef7f-201">Esse código pode ser desabilitado com uma variável de ambiente ou outra configuração em um ambiente de preparo ou de desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="eef7f-201">This code can be disabled with an environment variable or other configuration setting in a development or staging environment.</span></span>

### <a name="deal-with-path-base-and-proxies-that-change-the-request-path"></a><span data-ttu-id="eef7f-202">Lidar com o caminho base e proxies que alterar o caminho da solicitação</span><span class="sxs-lookup"><span data-stu-id="eef7f-202">Deal with path base and proxies that change the request path</span></span>

<span data-ttu-id="eef7f-203">Alguns proxies passam o caminho intactos, mas com um aplicativo caminho base deve ser removido para que o roteamento funcione corretamente.</span><span class="sxs-lookup"><span data-stu-id="eef7f-203">Some proxies pass the path intact but with an app base path that should be removed so that routing works properly.</span></span> <span data-ttu-id="eef7f-204">[UsePathBaseExtensions.UsePathBase](/dotnet/api/microsoft.aspnetcore.builder.usepathbaseextensions.usepathbase) middleware divide o caminho em [HttpRequest.Path](/dotnet/api/microsoft.aspnetcore.http.httprequest.path) e o caminho base do aplicativo em [HttpRequest.PathBase](/dotnet/api/microsoft.aspnetcore.http.httprequest.pathbase).</span><span class="sxs-lookup"><span data-stu-id="eef7f-204">[UsePathBaseExtensions.UsePathBase](/dotnet/api/microsoft.aspnetcore.builder.usepathbaseextensions.usepathbase) middleware splits the path into [HttpRequest.Path](/dotnet/api/microsoft.aspnetcore.http.httprequest.path) and the app base path into [HttpRequest.PathBase](/dotnet/api/microsoft.aspnetcore.http.httprequest.pathbase).</span></span>

<span data-ttu-id="eef7f-205">Se `/foo` é o caminho base do aplicativo para um caminho de proxy passado como `/foo/api/1`, os conjuntos de middleware `Request.PathBase` para `/foo` e `Request.Path` para `/api/1` com o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="eef7f-205">If `/foo` is the app base path for a proxy path passed as `/foo/api/1`, the middleware sets `Request.PathBase` to `/foo` and `Request.Path` to `/api/1` with the following command:</span></span>

```csharp
app.UsePathBase("/foo");
```

<span data-ttu-id="eef7f-206">O caminho original e o caminho base são reaplicadas quando o middleware é chamado novamente na ordem inversa.</span><span class="sxs-lookup"><span data-stu-id="eef7f-206">The original path and path base are reapplied when the middleware is called again in reverse.</span></span> <span data-ttu-id="eef7f-207">Para obter mais informações sobre o processamento de pedidos de middleware, consulte [Middleware](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="eef7f-207">For more information on middleware order processing, see [Middleware](xref:fundamentals/middleware/index).</span></span>

<span data-ttu-id="eef7f-208">Se o proxy corta o caminho (por exemplo, encaminhamento `/foo/api/1` para `/api/1`), correção redireciona e links, definindo a solicitação [PathBase](/dotnet/api/microsoft.aspnetcore.http.httprequest.pathbase) propriedade:</span><span class="sxs-lookup"><span data-stu-id="eef7f-208">If the proxy trims the path (for example, forwarding `/foo/api/1` to `/api/1`), fix redirects and links by setting the request's [PathBase](/dotnet/api/microsoft.aspnetcore.http.httprequest.pathbase) property:</span></span>

```csharp
app.Use((context, next) =>
{
    context.Request.PathBase = new PathString("/foo");
    return next();
});
```

<span data-ttu-id="eef7f-209">Se o proxy está adicionando dados de caminho, descarte a parte do caminho para corrigir redirecionamentos e links usando [StartsWithSegments (PathString, PathString)](/dotnet/api/microsoft.aspnetcore.http.pathstring.startswithsegments#Microsoft_AspNetCore_Http_PathString_StartsWithSegments_Microsoft_AspNetCore_Http_PathString_Microsoft_AspNetCore_Http_PathString__) e de atribuição para o [caminho](/dotnet/api/microsoft.aspnetcore.http.httprequest.path) propriedade:</span><span class="sxs-lookup"><span data-stu-id="eef7f-209">If the proxy is adding path data, discard part of the path to fix redirects and links by using [StartsWithSegments(PathString, PathString)](/dotnet/api/microsoft.aspnetcore.http.pathstring.startswithsegments#Microsoft_AspNetCore_Http_PathString_StartsWithSegments_Microsoft_AspNetCore_Http_PathString_Microsoft_AspNetCore_Http_PathString__) and assigning to the [Path](/dotnet/api/microsoft.aspnetcore.http.httprequest.path) property:</span></span>

```csharp
app.Use((context, next) =>
{
    if (context.Request.Path.StartsWithSegments("/foo", out var remainder))
    {
        context.Request.Path = remainder;
    }

    return next();
});
```

## <a name="troubleshoot"></a><span data-ttu-id="eef7f-210">Solução de problemas</span><span class="sxs-lookup"><span data-stu-id="eef7f-210">Troubleshoot</span></span>

<span data-ttu-id="eef7f-211">Quando os cabeçalhos não são encaminhados conforme o esperado, habilite [log](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="eef7f-211">When headers aren't forwarded as expected, enable [logging](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="eef7f-212">Se os logs não fornecerem informações suficientes para solucionar o problema, enumere os cabeçalhos de solicitação recebidos pelo servidor.</span><span class="sxs-lookup"><span data-stu-id="eef7f-212">If the logs don't provide sufficient information to troubleshoot the problem, enumerate the request headers received by the server.</span></span> <span data-ttu-id="eef7f-213">Os cabeçalhos podem ser gravados em uma resposta de aplicativo usando middleware embutido:</span><span class="sxs-lookup"><span data-stu-id="eef7f-213">The headers can be written to an app response using inline middleware:</span></span>

```csharp
public void Configure(IApplicationBuilder app, ILoggerFactory loggerfactory)
{
    app.Run(async (context) =>
    {
        context.Response.ContentType = "text/plain";

        // Request method, scheme, and path
        await context.Response.WriteAsync(
            $"Request Method: {context.Request.Method}{Environment.NewLine}");
        await context.Response.WriteAsync(
            $"Request Scheme: {context.Request.Scheme}{Environment.NewLine}");
        await context.Response.WriteAsync(
            $"Request Path: {context.Request.Path}{Environment.NewLine}");

        // Headers
        await context.Response.WriteAsync($"Request Headers:{Environment.NewLine}");

        foreach (var header in context.Request.Headers)
        {
            await context.Response.WriteAsync($"{header.Key}: " +
                $"{header.Value}{Environment.NewLine}");
        }

        await context.Response.WriteAsync(Environment.NewLine);

        // Connection: RemoteIp
        await context.Response.WriteAsync(
            $"Request RemoteIp: {context.Connection.RemoteIpAddress}");
    }
}
```

<span data-ttu-id="eef7f-214">Certifique-se de que o X - encaminhado-\* cabeçalhos são recebidos pelo servidor com os valores esperados.</span><span class="sxs-lookup"><span data-stu-id="eef7f-214">Ensure that the X-Forwarded-\* headers are received by the server with the expected values.</span></span> <span data-ttu-id="eef7f-215">Se houver vários valores em um determinado cabeçalho, observe encaminhados cabeçalhos Middleware processos cabeçalhos na ordem inversa da direita para esquerda.</span><span class="sxs-lookup"><span data-stu-id="eef7f-215">If there are multiple values in a given header, note Forwarded Headers Middleware processes headers in reverse order from right to left.</span></span>

<span data-ttu-id="eef7f-216">IP de remoto original da solicitação deve corresponder a uma entrada no `KnownProxies` ou `KnownNetworks` lista antes de X-encaminhado-para serem processado.</span><span class="sxs-lookup"><span data-stu-id="eef7f-216">The request's original remote IP must match an entry in the `KnownProxies` or `KnownNetworks` lists before X-Forwarded-For is processed.</span></span> <span data-ttu-id="eef7f-217">Isso limita a falsificação de cabeçalho por não aceitar encaminhadores de proxies não confiáveis.</span><span class="sxs-lookup"><span data-stu-id="eef7f-217">This limits header spoofing by not accepting forwarders from untrusted proxies.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="eef7f-218">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="eef7f-218">Additional resources</span></span>

* [<span data-ttu-id="eef7f-219">Microsoft Security Advisory CVE-2018-0787: ASP.NET Core vulnerabilidade de elevação de privilégio</span><span class="sxs-lookup"><span data-stu-id="eef7f-219">Microsoft Security Advisory CVE-2018-0787: ASP.NET Core Elevation Of Privilege Vulnerability</span></span>](https://github.com/aspnet/Announcements/issues/295)

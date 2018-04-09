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
# <a name="configure-aspnet-core-to-work-with-proxy-servers-and-load-balancers"></a>Configurar o ASP.NET Core para trabalhar com servidores proxy e balanceadores de carga

Por [Luke Latham](https://github.com/guardrex) e [Ross Carlos](https://github.com/Tratcher)

Na configuração recomendada para o ASP.NET Core, o aplicativo é hospedado usando o módulo de núcleo IIS/ASP.NET, Nginx ou Apache. Servidores proxy, balanceadores de carga e outros dispositivos de rede geralmente ocultar informações sobre a solicitação antes de alcançar o aplicativo:

* Quando solicitações HTTPS são delegadas sobre HTTP, o esquema original (HTTPS) é perdido e deve ser encaminhado em um cabeçalho.
* Como um aplicativo recebe uma solicitação de proxy e não sua fonte verdadeira na Internet ou rede corporativa, o endereço IP do cliente de origem também deve ser encaminhado em um cabeçalho.

Essas informações podem ser importantes no processamento de solicitações, por exemplo na redirecionamentos, autenticação, geração de link, avaliação de política e geoloation do cliente.

## <a name="forwarded-headers"></a>Cabeçalhos encaminhados

Por convenção, os proxies encaminham informações em cabeçalhos HTTP.

| Cabeçalho | Descrição |
| ------ | ----------- |
| X-Forwarded-For | Contém informações sobre o cliente que iniciou a solicitação e proxies subsequentes em uma cadeia de proxies. Esse parâmetro pode conter IP endereços (e, opcionalmente, os números de porta). Em uma cadeia de servidores proxy, o primeiro parâmetro indica o cliente em que a solicitação foi feita pela primeira vez. Siga os identificadores de proxy subsequentes. O último proxy na cadeia não está na lista de parâmetros. Endereço IP do proxy última e, opcionalmente, um número de porta estão disponíveis como o endereço IP remoto na camada de transporte. |
| X-Forwarded-Proto | O valor do esquema de origem (HTTP/HTTPS). O valor também pode ser uma lista de esquemas, se a solicitação percorreu vários proxies. |
| X-Forwarded-Host | O valor original do campo de cabeçalho de Host. Normalmente, os proxies não modifique o cabeçalho de Host. Consulte [Microsoft Security Advisory CVE-2018-0787](https://github.com/aspnet/Announcements/issues/295) para obter informações sobre uma vulnerabilidade de elevação de privilégios que afeta os sistemas em que o proxy não valida ou restict cabeçalhos de Host para valores de BOM conhecidos. |

O Middleware de cabeçalhos encaminhados do [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) empacotar, lê esses cabeçalhos e preenche os campos associados em [HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext). 

As atualizações de middleware:

* [HttpContext.Connection.RemoteIpAddress](/dotnet/api/microsoft.aspnetcore.http.connectioninfo.remoteipaddress) &ndash; definido usando o `X-Forwarded-For` o valor do cabeçalho. Configurações adicionais influenciam como o middleware define `RemoteIpAddress`. Para obter detalhes, consulte o [opções de Middleware de cabeçalhos encaminhados](#forwarded-headers-middleware-options).
* [HttpContext.Request.Scheme](/dotnet/api/microsoft.aspnetcore.http.httprequest.scheme) &ndash; definido usando o `X-Forwarded-Proto` o valor do cabeçalho.
* [HttpContext.Request.Host](/dotnet/api/microsoft.aspnetcore.http.httprequest.host) &ndash; definido usando o `X-Forwarded-Host` o valor do cabeçalho.

Observe que nem todos os dispositivos de rede adicionar o `X-Forwarded-For` e `X-Forwarded-Proto` cabeçalhos sem configuração adicional. Consulte o guia do fabricante do dispositivo se as solicitações de proxies não tiver esses cabeçalhos quando atingem o aplicativo.

Encaminhado cabeçalhos Middleware [configurações padrão](#forwarded-headers-middleware-options) pode ser configurado. As configurações padrão são:

* Há apenas *um proxy* entre o aplicativo e a fonte das solicitações.
* Somente os endereços de loopback são configurados para proxies conhecidos e conhecidos redes.

## <a name="iisiis-express-and-aspnet-core-module"></a>O IIS/IIS Express e o módulo principal do ASP.NET

Encaminhada Middleware de cabeçalhos é habilitado por padrão pelo Middleware de integração do IIS quando o aplicativo é executado por trás do IIS e o módulo do ASP.NET Core. Middleware de cabeçalhos encaminhada está ativado para ser executado primeiro no pipeline de middleware com uma configuração restrita específica para o módulo do núcleo do ASP.NET devido a questões de confiança com cabeçalhos encaminhados (por exemplo, [falsificação de IP](https://www.iplocation.net/ip-spoofing)). O middleware está configurado para encaminhar o `X-Forwarded-For` e `X-Forwarded-Proto` cabeçalhos e é restrita a um proxy localhost único. Se a configuração adicional é necessária, consulte o [opções de Middleware de cabeçalhos encaminhados](#forwarded-headers-middleware-options).

## <a name="other-proxy-server-and-load-balancer-scenarios"></a>Outro servidor de proxy e cenários de Balanceador de carga

Além do uso de Middleware de integração do IIS, encaminhados cabeçalhos Middleware não está habilitado por padrão. Encaminhada Middleware de cabeçalhos deve ser habilitado para um aplicativo para processar encaminhado cabeçalhos com [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders). Depois de habilitar o middleware se nenhum [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) são especificados para o middleware, o padrão [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) são [ForwardedHeaders.None ](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders).

Configurar o middleware com [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) para encaminhar o `X-Forwarded-For` e `X-Forwarded-Proto` cabeçalhos no `Startup.ConfigureServices`. Invocar o [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) método `Startup.Configure` antes de chamar outro middleware:

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
> Se nenhum [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) são especificadas em `Startup.ConfigureServices` ou diretamente para o método de extensão com [UseForwardedHeaders (IApplicationBuilder, ForwardedHeadersOptions)](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ForwardedHeadersExtensions_UseForwardedHeaders_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_ForwardedHeadersOptions_), o padrão cabeçalhos para encaminhar são [ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders). O [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) propriedade deve ser configurada com os cabeçalhos para encaminhar.

## <a name="forwarded-headers-middleware-options"></a>Opções de Middleware cabeçalhos encaminhadas

[ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) controlar o comportamento do Middleware encaminhados cabeçalhos:

```csharp
services.Configure<ForwardedHeadersOptions>(options =>
{
    options.ForwardLimit = 2;
    options.KnownProxies.Add(IPAddress.Parse("127.0.10.1"));
    options.ForwardedForHeaderName = "X-Forwarded-For-Custom-Header-Name";
});
```

| Opção | Descrição |
| ------ | ----------- |
| [ForwardedForHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedforheadername) | Use o cabeçalho especificado por essa propriedade, em vez de um especificado por [ForwardedHeadersDefaults.XForwardedForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedforheadername).<br><br>O padrão é `X-Forwarded-For`. |
| [ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) | Identifica qual encaminhadores devem ser processados. Consulte o [ForwardedHeaders Enum](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders) para a lista de campos que se aplicam. Os valores típicos atribuídos a essa propriedade são <code>ForwardedHeaders.XForwardedFor &#124; ForwardedHeaders.XForwardedProto</code>.<br><br>O valor padrão é [ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders). |
| [ForwardedHostHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedhostheadername) | Use o cabeçalho especificado por essa propriedade, em vez de um especificado por [ForwardedHeadersDefaults.XForwardedHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedhostheadername).<br><br>O padrão é `X-Forwarded-Host`. |
| [ForwardedProtoHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedprotoheadername) | Use o cabeçalho especificado por essa propriedade, em vez de um especificado por [ForwardedHeadersDefaults.XForwardedProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedprotoheadername).<br><br>O padrão é `X-Forwarded-Proto`. |
| [ForwardLimit](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardlimit) | Limita o número de entradas nos cabeçalhos que são processados. Definido como `null` desabilitar o limite, mas isso só deve ser feito se `KnownProxies` ou `KnownNetworks` estão configurados.<br><br>O padrão é 1. |
| [KnownNetworks](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.knownnetworks) | Intervalos de proxies conhecidos para aceitar encaminhados cabeçalhos de endereços. Forneça os intervalos IP usando notação de roteamento entre domínios (CIDR).<br><br>O padrão é um [IList](/dotnet/api/system.collections.generic.ilist-1)\<[IPNetwork](/dotnet/api/microsoft.aspnetcore.httpoverrides.ipnetwork)> que contém uma única entrada para `IPAddress.Loopback`. |
| [KnownProxies](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.knownproxies) | Endereços de proxies conhecidos para aceitar cabeçalhos encaminhados do. Use `KnownProxies` especificar o endereço IP corresponde.<br><br>O padrão é um [IList](/dotnet/api/system.collections.generic.ilist-1)\<[IPAddress](/dotnet/api/system.net.ipaddress)> que contém uma única entrada para `IPAddress.IPv6Loopback`. |
| [OriginalForHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalforheadername) | Use o cabeçalho especificado por essa propriedade, em vez de um especificado por [ForwardedHeadersDefaults.XOriginalForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalforheadername).<br><br>O padrão é `X-Original-For`. |
| [OriginalHostHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalhostheadername) | Use o cabeçalho especificado por essa propriedade, em vez de um especificado por [ForwardedHeadersDefaults.XOriginalHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalhostheadername).<br><br>O padrão é `X-Original-Host`. |
| [OriginalProtoHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalprotoheadername) | Use o cabeçalho especificado por essa propriedade, em vez de um especificado por [ForwardedHeadersDefaults.XOriginalProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalprotoheadername).<br><br>O padrão é `X-Original-Proto`. |
| [RequireHeaderSymmetry](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.requireheadersymmetry) | Exigem o número de valores de cabeçalho a serem sincronizados entre o [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) sendo processada.<br><br>O padrão no núcleo do ASP.NET 1. x é `true`. O padrão do ASP.NET Core 2.0 ou posterior é `false`. |

## <a name="scenarios-and-use-cases"></a>Cenários e casos de uso

### <a name="when-it-isnt-possible-to-add-forwarded-headers-and-all-requests-are-secure"></a>Quando não é possível adicionar encaminhada cabeçalhos e todas as solicitações são seguras

Em alguns casos, pode não ser possível adicionar cabeçalhos encaminhados para as solicitações de proxies para o aplicativo. Se o proxy é impor que todas as solicitações externas públicas são HTTPS, o esquema pode ser definido manualmente em `Startup.Configure` antes de usar qualquer tipo de middleware:

```csharp
app.Use((context, next) =>
{
    context.Request.Scheme = "https";
    return next();
});
```

Esse código pode ser desabilitado com uma variável de ambiente ou outra configuração em um ambiente de preparo ou de desenvolvimento.

### <a name="deal-with-path-base-and-proxies-that-change-the-request-path"></a>Lidar com o caminho base e proxies que alterar o caminho da solicitação

Alguns proxies passam o caminho intactos, mas com um aplicativo caminho base deve ser removido para que o roteamento funcione corretamente. [UsePathBaseExtensions.UsePathBase](/dotnet/api/microsoft.aspnetcore.builder.usepathbaseextensions.usepathbase) middleware divide o caminho em [HttpRequest.Path](/dotnet/api/microsoft.aspnetcore.http.httprequest.path) e o caminho base do aplicativo em [HttpRequest.PathBase](/dotnet/api/microsoft.aspnetcore.http.httprequest.pathbase).

Se `/foo` é o caminho base do aplicativo para um caminho de proxy passado como `/foo/api/1`, os conjuntos de middleware `Request.PathBase` para `/foo` e `Request.Path` para `/api/1` com o seguinte comando:

```csharp
app.UsePathBase("/foo");
```

O caminho original e o caminho base são reaplicadas quando o middleware é chamado novamente na ordem inversa. Para obter mais informações sobre o processamento de pedidos de middleware, consulte [Middleware](xref:fundamentals/middleware/index).

Se o proxy corta o caminho (por exemplo, encaminhamento `/foo/api/1` para `/api/1`), correção redireciona e links, definindo a solicitação [PathBase](/dotnet/api/microsoft.aspnetcore.http.httprequest.pathbase) propriedade:

```csharp
app.Use((context, next) =>
{
    context.Request.PathBase = new PathString("/foo");
    return next();
});
```

Se o proxy está adicionando dados de caminho, descarte a parte do caminho para corrigir redirecionamentos e links usando [StartsWithSegments (PathString, PathString)](/dotnet/api/microsoft.aspnetcore.http.pathstring.startswithsegments#Microsoft_AspNetCore_Http_PathString_StartsWithSegments_Microsoft_AspNetCore_Http_PathString_Microsoft_AspNetCore_Http_PathString__) e de atribuição para o [caminho](/dotnet/api/microsoft.aspnetcore.http.httprequest.path) propriedade:

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

## <a name="troubleshoot"></a>Solução de problemas

Quando os cabeçalhos não são encaminhados conforme o esperado, habilite [log](xref:fundamentals/logging/index). Se os logs não fornecerem informações suficientes para solucionar o problema, enumere os cabeçalhos de solicitação recebidos pelo servidor. Os cabeçalhos podem ser gravados em uma resposta de aplicativo usando middleware embutido:

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

Certifique-se de que o X - encaminhado-* cabeçalhos são recebidos pelo servidor com os valores esperados. Se houver vários valores em um determinado cabeçalho, observe encaminhados cabeçalhos Middleware processos cabeçalhos na ordem inversa da direita para esquerda.

IP de remoto original da solicitação deve corresponder a uma entrada no `KnownProxies` ou `KnownNetworks` lista antes de X-encaminhado-para serem processado. Isso limita a falsificação de cabeçalho por não aceitar encaminhadores de proxies não confiáveis.

## <a name="additional-resources"></a>Recursos adicionais

* [Microsoft Security Advisory CVE-2018-0787: ASP.NET Core vulnerabilidade de elevação de privilégio](https://github.com/aspnet/Announcements/issues/295)

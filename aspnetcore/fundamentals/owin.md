---
title: OWIN (Open Web Interface para .NET)
author: ardalis
description: "Descobrir como ASP.NET Core dá suporte à Interface da Web aberto para .NET (OWIN), que permite que aplicativos da web para ser separada dos servidores web."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/owin
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e819037e2ebd1566c778879516e20de8dc7603ea
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/19/2018
---
# <a name="introduction-to-open-web-interface-for-net-owin"></a><span data-ttu-id="cc3fe-103">Introdução ao abrir a Interface da Web para .NET (OWIN)</span><span class="sxs-lookup"><span data-stu-id="cc3fe-103">Introduction to Open Web Interface for .NET (OWIN)</span></span>

<span data-ttu-id="cc3fe-104">Por [Steve Smith](https://ardalis.com/) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="cc3fe-104">By [Steve Smith](https://ardalis.com/) and  [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="cc3fe-105">O ASP.NET Core dá suporte para OWIN (Open Web Interface para .NET).</span><span class="sxs-lookup"><span data-stu-id="cc3fe-105">ASP.NET Core supports the Open Web Interface for .NET (OWIN).</span></span> <span data-ttu-id="cc3fe-106">O OWIN permite que os aplicativos Web sejam separados dos servidores Web.</span><span class="sxs-lookup"><span data-stu-id="cc3fe-106">OWIN allows web apps to be decoupled from web servers.</span></span> <span data-ttu-id="cc3fe-107">Define uma maneira padronizada de middleware para ser usada em um pipeline para manipular solicitações e respostas associadas.</span><span class="sxs-lookup"><span data-stu-id="cc3fe-107">It defines a standard way for middleware to be used in a pipeline to handle requests and associated responses.</span></span> <span data-ttu-id="cc3fe-108">Middleware e aplicativos do ASP.NET Core podem interoperar com middleware, servidores e aplicativos baseados no OWIN.</span><span class="sxs-lookup"><span data-stu-id="cc3fe-108">ASP.NET Core applications and middleware can interoperate with OWIN-based applications, servers, and middleware.</span></span>

<span data-ttu-id="cc3fe-109">OWIN fornece uma camada de dissociação que permite duas estruturas com modelos de objeto diferentes para ser usados juntos.</span><span class="sxs-lookup"><span data-stu-id="cc3fe-109">OWIN provides a decoupling layer that allows two frameworks with disparate object models to be used together.</span></span> <span data-ttu-id="cc3fe-110">O `Microsoft.AspNetCore.Owin` pacote fornece duas implementações de adaptador:</span><span class="sxs-lookup"><span data-stu-id="cc3fe-110">The `Microsoft.AspNetCore.Owin` package provides two adapter implementations:</span></span>
- <span data-ttu-id="cc3fe-111">ASP.NET Core para OWIN</span><span class="sxs-lookup"><span data-stu-id="cc3fe-111">ASP.NET Core to OWIN</span></span> 
- <span data-ttu-id="cc3fe-112">OWIN para ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="cc3fe-112">OWIN to ASP.NET Core</span></span>

<span data-ttu-id="cc3fe-113">Isso permite que o ASP.NET Core ser hospedado em um servidor compatível OWIN/host ou para outros componentes compatíveis OWIN a ser executado sobre o ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="cc3fe-113">This allows ASP.NET Core to be hosted on top of an OWIN compatible server/host, or for other OWIN compatible components to be run on top of ASP.NET Core.</span></span>

<span data-ttu-id="cc3fe-114">Observação: Usar esses adaptadores vem com um custo de desempenho.</span><span class="sxs-lookup"><span data-stu-id="cc3fe-114">Note: Using these adapters comes with a performance cost.</span></span> <span data-ttu-id="cc3fe-115">Aplicativos que usam somente os componentes principais do ASP.NET não devem usar o pacote de Owin ou adaptadores.</span><span class="sxs-lookup"><span data-stu-id="cc3fe-115">Applications using only ASP.NET Core components should not use the Owin package or adapters.</span></span>

<span data-ttu-id="cc3fe-116">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="cc3fe-116">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="running-owin-middleware-in-the-aspnet-pipeline"></a><span data-ttu-id="cc3fe-117">Executando OWIN middleware no pipeline do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="cc3fe-117">Running OWIN middleware in the ASP.NET pipeline</span></span>

<span data-ttu-id="cc3fe-118">O suporte do ASP.NET Core OWIN é implantado como parte do `Microsoft.AspNetCore.Owin` pacote.</span><span class="sxs-lookup"><span data-stu-id="cc3fe-118">ASP.NET Core's OWIN support is deployed as part of the `Microsoft.AspNetCore.Owin` package.</span></span> <span data-ttu-id="cc3fe-119">Você pode importar suporte OWIN para seu projeto ao instalar este pacote.</span><span class="sxs-lookup"><span data-stu-id="cc3fe-119">You can import OWIN support into your project by installing this package.</span></span>

<span data-ttu-id="cc3fe-120">Middleware OWIN está em conformidade com a [especificação OWIN](http://owin.org/spec/spec/owin-1.0.0.html), que requer um `Func<IDictionary<string, object>, Task>` interface e as chaves específicas ser definido (como `owin.ResponseBody`).</span><span class="sxs-lookup"><span data-stu-id="cc3fe-120">OWIN middleware conforms to the [OWIN specification](http://owin.org/spec/spec/owin-1.0.0.html), which requires a `Func<IDictionary<string, object>, Task>` interface, and specific keys be set (such as `owin.ResponseBody`).</span></span> <span data-ttu-id="cc3fe-121">Middleware OWIN simple seguinte exibe "Hello World":</span><span class="sxs-lookup"><span data-stu-id="cc3fe-121">The following simple OWIN middleware displays "Hello World":</span></span>

```csharp
public Task OwinHello(IDictionary<string, object> environment)
{
    string responseText = "Hello World via OWIN";
    byte[] responseBytes = Encoding.UTF8.GetBytes(responseText);

    // OWIN Environment Keys: http://owin.org/spec/spec/owin-1.0.0.html
    var responseStream = (Stream)environment["owin.ResponseBody"];
    var responseHeaders = (IDictionary<string, string[]>)environment["owin.ResponseHeaders"];

    responseHeaders["Content-Length"] = new string[] { responseBytes.Length.ToString(CultureInfo.InvariantCulture) };
    responseHeaders["Content-Type"] = new string[] { "text/plain" };

    return responseStream.WriteAsync(responseBytes, 0, responseBytes.Length);
}
```

<span data-ttu-id="cc3fe-122">A assinatura de exemplo retorna um `Task` e aceita um `IDictionary<string, object>` conforme solicitado pelo OWIN.</span><span class="sxs-lookup"><span data-stu-id="cc3fe-122">The sample signature returns a `Task` and accepts an `IDictionary<string, object>` as required by OWIN.</span></span>

<span data-ttu-id="cc3fe-123">O código a seguir mostra como adicionar o `OwinHello` middleware (mostrado acima) para o pipeline do ASP.NET com o `UseOwin` método de extensão.</span><span class="sxs-lookup"><span data-stu-id="cc3fe-123">The following code shows how to add the `OwinHello` middleware (shown above) to the ASP.NET pipeline with the `UseOwin` extension method.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseOwin(pipeline =>
    {
        pipeline(next => OwinHello);
    });
}
```

<span data-ttu-id="cc3fe-124">Você pode configurar outras ações para ocorrer no pipeline OWIN.</span><span class="sxs-lookup"><span data-stu-id="cc3fe-124">You can configure other actions to take place within the OWIN pipeline.</span></span>

> [!NOTE]
> <span data-ttu-id="cc3fe-125">Cabeçalhos de resposta só devem ser modificados antes da primeira gravação no fluxo de resposta.</span><span class="sxs-lookup"><span data-stu-id="cc3fe-125">Response headers should only be modified prior to the first write to the response stream.</span></span>

> [!NOTE]
> <span data-ttu-id="cc3fe-126">Diversas chamadas para `UseOwin` não é recomendado por motivos de desempenho.</span><span class="sxs-lookup"><span data-stu-id="cc3fe-126">Multiple calls to `UseOwin` is discouraged for performance reasons.</span></span> <span data-ttu-id="cc3fe-127">Componentes OWIN funcionarão melhor se agrupados juntos.</span><span class="sxs-lookup"><span data-stu-id="cc3fe-127">OWIN components will operate best if grouped together.</span></span>

```csharp
app.UseOwin(pipeline =>
{
    pipeline(next =>
    {
        // do something before
        return OwinHello;
        // do something after
    });
});
```

<a name="hosting-on-owin"></a>

## <a name="using-aspnet-hosting-on-an-owin-based-server"></a><span data-ttu-id="cc3fe-128">Usando o ASP.NET de hospedagem em um servidor baseado em OWIN</span><span class="sxs-lookup"><span data-stu-id="cc3fe-128">Using ASP.NET Hosting on an OWIN-based server</span></span>

<span data-ttu-id="cc3fe-129">Servidores baseados no OWIN podem hospedar aplicativos ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="cc3fe-129">OWIN-based servers can host ASP.NET applications.</span></span> <span data-ttu-id="cc3fe-130">Um desses servidores é [Nowin](https://github.com/Bobris/Nowin), um servidor da web .NET OWIN.</span><span class="sxs-lookup"><span data-stu-id="cc3fe-130">One such server is [Nowin](https://github.com/Bobris/Nowin), a .NET OWIN web server.</span></span> <span data-ttu-id="cc3fe-131">O exemplo deste artigo, incluí um projeto que faz referência a Nowin e usa-o para criar um `IServer` capaz de hospedar o ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="cc3fe-131">In the sample for this article, I've included a project that references Nowin and uses it to create an `IServer` capable of self-hosting ASP.NET Core.</span></span>

[!code-csharp[Main](owin/sample/src/NowinSample/Program.cs?highlight=15)]

<span data-ttu-id="cc3fe-132">`IServer`é uma interface que requer um `Features` propriedade e um `Start` método.</span><span class="sxs-lookup"><span data-stu-id="cc3fe-132">`IServer` is an interface that requires an `Features` property and a `Start` method.</span></span>

<span data-ttu-id="cc3fe-133">`Start`é responsável por configurar e iniciar o servidor, que nesse caso é feito por meio de uma série de chamadas de API fluentes que definir endereços analisados a partir de IServerAddressesFeature.</span><span class="sxs-lookup"><span data-stu-id="cc3fe-133">`Start` is responsible for configuring and starting the server, which in this case is done through a series of fluent API calls that set addresses parsed from the IServerAddressesFeature.</span></span> <span data-ttu-id="cc3fe-134">Observe que a configuração fluente do `_builder` variável Especifica que as solicitações serão manipuladas pelo `appFunc` definido anteriormente no método.</span><span class="sxs-lookup"><span data-stu-id="cc3fe-134">Note that the fluent configuration of the `_builder` variable specifies that requests will be handled by the `appFunc` defined earlier in the method.</span></span> <span data-ttu-id="cc3fe-135">Isso `Func` é chamado em cada solicitação para processar solicitações de entrada.</span><span class="sxs-lookup"><span data-stu-id="cc3fe-135">This `Func` is called on each request to process incoming requests.</span></span>

<span data-ttu-id="cc3fe-136">Nós adicionaremos um `IWebHostBuilder` extensão para tornar mais fácil adicionar e configurar o servidor Nowin.</span><span class="sxs-lookup"><span data-stu-id="cc3fe-136">We'll also add an `IWebHostBuilder` extension to make it easy to add and configure the Nowin server.</span></span>

```csharp
using System;
using Microsoft.AspNetCore.Hosting.Server;
using Microsoft.Extensions.DependencyInjection;
using Nowin;
using NowinSample;

namespace Microsoft.AspNetCore.Hosting
{
    public static class NowinWebHostBuilderExtensions
    {
        public static IWebHostBuilder UseNowin(this IWebHostBuilder builder)
        {
            return builder.ConfigureServices(services =>
            {
                services.AddSingleton<IServer, NowinServer>();
            });
        }

        public static IWebHostBuilder UseNowin(this IWebHostBuilder builder, Action<ServerBuilder> configure)
        {
            builder.ConfigureServices(services =>
            {
                services.Configure(configure);
            });
            return builder.UseNowin();
        }
    }
}
```

<span data-ttu-id="cc3fe-137">Com isso em vigor, tudo o que é necessário para executar um aplicativo ASP.NET usando o servidor personalizado para chamar a extensão na *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="cc3fe-137">With this in place, all that's required to run an ASP.NET application using this custom server to call the extension in *Program.cs*:</span></span>

```csharp

using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Hosting;

namespace NowinSample
{
    public class Program
    {
        public static void Main(string[] args)
        {
            var host = new WebHostBuilder()
                .UseNowin()
                .UseContentRoot(Directory.GetCurrentDirectory())
                .UseIISIntegration()
                .UseStartup<Startup>()
                .Build();

            host.Run();
        }
    }
}

```

<span data-ttu-id="cc3fe-138">Saiba mais sobre o ASP.NET [servidores](servers/index.md).</span><span class="sxs-lookup"><span data-stu-id="cc3fe-138">Learn more about ASP.NET [Servers](servers/index.md).</span></span>

## <a name="run-aspnet-core-on-an-owin-based-server-and-use-its-websockets-support"></a><span data-ttu-id="cc3fe-139">Executar o ASP.NET Core em um servidor baseado em OWIN e usar seu suporte de WebSockets</span><span class="sxs-lookup"><span data-stu-id="cc3fe-139">Run ASP.NET Core on an OWIN-based server and use its WebSockets support</span></span>

<span data-ttu-id="cc3fe-140">Outro exemplo de recursos de servidores como baseados no OWIN pode ser aproveitado por ASP.NET Core é o acesso a recursos como o WebSocket.</span><span class="sxs-lookup"><span data-stu-id="cc3fe-140">Another example of how OWIN-based servers' features can be leveraged by ASP.NET Core is access to features like WebSockets.</span></span> <span data-ttu-id="cc3fe-141">O servidor de web OWIN .NET usado no exemplo anterior tem suporte para soquetes da Web interna, que podem ser aproveitadas por um aplicativo do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="cc3fe-141">The .NET OWIN web server used in the previous example has support for Web Sockets built in, which can be leveraged by an ASP.NET Core application.</span></span> <span data-ttu-id="cc3fe-142">O exemplo a seguir mostra um aplicativo simples da web que suporta soquetes da Web e exibe novamente tudo enviados ao servidor por meio de WebSockets.</span><span class="sxs-lookup"><span data-stu-id="cc3fe-142">The example below shows a simple web app that supports Web Sockets and echoes back everything sent to the server through WebSockets.</span></span>

```csharp
public class Startup
{
    public void Configure(IApplicationBuilder app)
    {
        app.Use(async (context, next) =>
        {
            if (context.WebSockets.IsWebSocketRequest)
            {
                WebSocket webSocket = await context.WebSockets.AcceptWebSocketAsync();
                await EchoWebSocket(webSocket);
            }
            else
            {
                await next();
            }
        });

        app.Run(context =>
        {
            return context.Response.WriteAsync("Hello World");
        });
    }

    private async Task EchoWebSocket(WebSocket webSocket)
    {
        byte[] buffer = new byte[1024];
        WebSocketReceiveResult received = await webSocket.ReceiveAsync(
            new ArraySegment<byte>(buffer), CancellationToken.None);

        while (!webSocket.CloseStatus.HasValue)
        {
            // Echo anything we receive
            await webSocket.SendAsync(new ArraySegment<byte>(buffer, 0, received.Count), 
                received.MessageType, received.EndOfMessage, CancellationToken.None);

            received = await webSocket.ReceiveAsync(new ArraySegment<byte>(buffer), 
                CancellationToken.None);
        }

        await webSocket.CloseAsync(webSocket.CloseStatus.Value, 
            webSocket.CloseStatusDescription, CancellationToken.None);
    }
}
```

<span data-ttu-id="cc3fe-143">Isso [exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) é configurado usando o mesmo `NowinServer` que o anterior - a única diferença está em como o aplicativo está configurado no seu `Configure` método.</span><span class="sxs-lookup"><span data-stu-id="cc3fe-143">This [sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) is configured using the same `NowinServer` as the previous one - the only difference is in how the application is configured in its `Configure` method.</span></span> <span data-ttu-id="cc3fe-144">Um teste usando [um cliente do websocket simples](https://chrome.google.com/webstore/detail/simple-websocket-client/pfdhoblngboilpfeibdedpjgfnlcodoo?hl=en) demonstra o aplicativo:</span><span class="sxs-lookup"><span data-stu-id="cc3fe-144">A test using [a simple websocket client](https://chrome.google.com/webstore/detail/simple-websocket-client/pfdhoblngboilpfeibdedpjgfnlcodoo?hl=en) demonstrates  the application:</span></span>

![Cliente de teste de soquete da Web](owin/_static/websocket-test.png)

## <a name="owin-environment"></a><span data-ttu-id="cc3fe-146">Ambiente OWIN</span><span class="sxs-lookup"><span data-stu-id="cc3fe-146">OWIN environment</span></span>

<span data-ttu-id="cc3fe-147">Você pode construir um ambiente OWIN usando o `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="cc3fe-147">You can construct a OWIN environment using the `HttpContext`.</span></span>

```csharp

   var environment = new OwinEnvironment(HttpContext);
   var features = new OwinFeatureCollection(environment);
   ```

## <a name="owin-keys"></a><span data-ttu-id="cc3fe-148">Chaves OWIN</span><span class="sxs-lookup"><span data-stu-id="cc3fe-148">OWIN keys</span></span>

<span data-ttu-id="cc3fe-149">Depende do OWIN um `IDictionary<string,object>` objeto para comunicar informações ao longo de uma troca de solicitação/resposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="cc3fe-149">OWIN depends on an `IDictionary<string,object>` object to communicate information throughout an HTTP Request/Response exchange.</span></span> <span data-ttu-id="cc3fe-150">ASP.NET Core implementa as chaves listadas abaixo.</span><span class="sxs-lookup"><span data-stu-id="cc3fe-150">ASP.NET Core implements the keys listed below.</span></span> <span data-ttu-id="cc3fe-151">Consulte o [especificação primária, extensões](http://owin.org/#spec), e [OWIN chave diretrizes e chaves comuns](http://owin.org/spec/spec/CommonKeys.html).</span><span class="sxs-lookup"><span data-stu-id="cc3fe-151">See the [primary specification, extensions](http://owin.org/#spec), and [OWIN Key Guidelines and Common Keys](http://owin.org/spec/spec/CommonKeys.html).</span></span>

### <a name="request-data-owin-v100"></a><span data-ttu-id="cc3fe-152">Dados de solicitação (OWIN v1.0.0)</span><span class="sxs-lookup"><span data-stu-id="cc3fe-152">Request Data (OWIN v1.0.0)</span></span>

| <span data-ttu-id="cc3fe-153">Chave</span><span class="sxs-lookup"><span data-stu-id="cc3fe-153">Key</span></span>               | <span data-ttu-id="cc3fe-154">Valor (tipo)</span><span class="sxs-lookup"><span data-stu-id="cc3fe-154">Value (type)</span></span> | <span data-ttu-id="cc3fe-155">Descrição</span><span class="sxs-lookup"><span data-stu-id="cc3fe-155">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="cc3fe-156">owin.RequestScheme</span><span class="sxs-lookup"><span data-stu-id="cc3fe-156">owin.RequestScheme</span></span> | `String` |  |
| <span data-ttu-id="cc3fe-157">owin.RequestMethod</span><span class="sxs-lookup"><span data-stu-id="cc3fe-157">owin.RequestMethod</span></span>  | `String` | |    
| <span data-ttu-id="cc3fe-158">owin.RequestPathBase</span><span class="sxs-lookup"><span data-stu-id="cc3fe-158">owin.RequestPathBase</span></span>  | `String` | |    
| <span data-ttu-id="cc3fe-159">owin.RequestPath</span><span class="sxs-lookup"><span data-stu-id="cc3fe-159">owin.RequestPath</span></span> | `String` | |     
| <span data-ttu-id="cc3fe-160">owin.RequestQueryString</span><span class="sxs-lookup"><span data-stu-id="cc3fe-160">owin.RequestQueryString</span></span>  | `String` | |    
| <span data-ttu-id="cc3fe-161">owin.RequestProtocol</span><span class="sxs-lookup"><span data-stu-id="cc3fe-161">owin.RequestProtocol</span></span>  | `String` | |    
| <span data-ttu-id="cc3fe-162">owin.RequestHeaders</span><span class="sxs-lookup"><span data-stu-id="cc3fe-162">owin.RequestHeaders</span></span> | `IDictionary<string,string[]>`  | |
| <span data-ttu-id="cc3fe-163">owin.RequestBody</span><span class="sxs-lookup"><span data-stu-id="cc3fe-163">owin.RequestBody</span></span> | `Stream`  | |

### <a name="request-data-owin-v110"></a><span data-ttu-id="cc3fe-164">Dados de solicitação (OWIN v 1.1.0)</span><span class="sxs-lookup"><span data-stu-id="cc3fe-164">Request Data (OWIN v1.1.0)</span></span>

| <span data-ttu-id="cc3fe-165">Chave</span><span class="sxs-lookup"><span data-stu-id="cc3fe-165">Key</span></span>               | <span data-ttu-id="cc3fe-166">Valor (tipo)</span><span class="sxs-lookup"><span data-stu-id="cc3fe-166">Value (type)</span></span> | <span data-ttu-id="cc3fe-167">Descrição</span><span class="sxs-lookup"><span data-stu-id="cc3fe-167">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="cc3fe-168">owin.RequestId</span><span class="sxs-lookup"><span data-stu-id="cc3fe-168">owin.RequestId</span></span> | `String` | <span data-ttu-id="cc3fe-169">Opcional</span><span class="sxs-lookup"><span data-stu-id="cc3fe-169">Optional</span></span> |

### <a name="response-data-owin-v100"></a><span data-ttu-id="cc3fe-170">Dados de resposta (OWIN v1.0.0)</span><span class="sxs-lookup"><span data-stu-id="cc3fe-170">Response Data (OWIN v1.0.0)</span></span>

| <span data-ttu-id="cc3fe-171">Chave</span><span class="sxs-lookup"><span data-stu-id="cc3fe-171">Key</span></span>               | <span data-ttu-id="cc3fe-172">Valor (tipo)</span><span class="sxs-lookup"><span data-stu-id="cc3fe-172">Value (type)</span></span> | <span data-ttu-id="cc3fe-173">Descrição</span><span class="sxs-lookup"><span data-stu-id="cc3fe-173">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="cc3fe-174">owin.ResponseStatusCode</span><span class="sxs-lookup"><span data-stu-id="cc3fe-174">owin.ResponseStatusCode</span></span> | `int` | <span data-ttu-id="cc3fe-175">Opcional</span><span class="sxs-lookup"><span data-stu-id="cc3fe-175">Optional</span></span> |
| <span data-ttu-id="cc3fe-176">owin.ResponseReasonPhrase</span><span class="sxs-lookup"><span data-stu-id="cc3fe-176">owin.ResponseReasonPhrase</span></span> | `String` | <span data-ttu-id="cc3fe-177">Opcional</span><span class="sxs-lookup"><span data-stu-id="cc3fe-177">Optional</span></span> |
| <span data-ttu-id="cc3fe-178">owin.ResponseHeaders</span><span class="sxs-lookup"><span data-stu-id="cc3fe-178">owin.ResponseHeaders</span></span> | `IDictionary<string,string[]>`  | |
| <span data-ttu-id="cc3fe-179">owin.ResponseBody</span><span class="sxs-lookup"><span data-stu-id="cc3fe-179">owin.ResponseBody</span></span> | `Stream`  | |


### <a name="other-data-owin-v100"></a><span data-ttu-id="cc3fe-180">Outros dados (OWIN v1.0.0)</span><span class="sxs-lookup"><span data-stu-id="cc3fe-180">Other Data (OWIN v1.0.0)</span></span>

| <span data-ttu-id="cc3fe-181">Chave</span><span class="sxs-lookup"><span data-stu-id="cc3fe-181">Key</span></span>               | <span data-ttu-id="cc3fe-182">Valor (tipo)</span><span class="sxs-lookup"><span data-stu-id="cc3fe-182">Value (type)</span></span> | <span data-ttu-id="cc3fe-183">Descrição</span><span class="sxs-lookup"><span data-stu-id="cc3fe-183">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="cc3fe-184">owin.CallCancelled</span><span class="sxs-lookup"><span data-stu-id="cc3fe-184">owin.CallCancelled</span></span> | `CancellationToken` |  |
| <span data-ttu-id="cc3fe-185">owin.Version</span><span class="sxs-lookup"><span data-stu-id="cc3fe-185">owin.Version</span></span>  | `String` | |   


### <a name="common-keys"></a><span data-ttu-id="cc3fe-186">Chaves comuns</span><span class="sxs-lookup"><span data-stu-id="cc3fe-186">Common Keys</span></span>

| <span data-ttu-id="cc3fe-187">Chave</span><span class="sxs-lookup"><span data-stu-id="cc3fe-187">Key</span></span>               | <span data-ttu-id="cc3fe-188">Valor (tipo)</span><span class="sxs-lookup"><span data-stu-id="cc3fe-188">Value (type)</span></span> | <span data-ttu-id="cc3fe-189">Descrição</span><span class="sxs-lookup"><span data-stu-id="cc3fe-189">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="cc3fe-190">ssl.ClientCertificate</span><span class="sxs-lookup"><span data-stu-id="cc3fe-190">ssl.ClientCertificate</span></span> | `X509Certificate` |  |
| <span data-ttu-id="cc3fe-191">ssl.LoadClientCertAsync</span><span class="sxs-lookup"><span data-stu-id="cc3fe-191">ssl.LoadClientCertAsync</span></span>  | `Func<Task>` | |    
| <span data-ttu-id="cc3fe-192">server.RemoteIpAddress</span><span class="sxs-lookup"><span data-stu-id="cc3fe-192">server.RemoteIpAddress</span></span>  | `String` | |    
| <span data-ttu-id="cc3fe-193">server.RemotePort</span><span class="sxs-lookup"><span data-stu-id="cc3fe-193">server.RemotePort</span></span> | `String` | |     
| <span data-ttu-id="cc3fe-194">server.LocalIpAddress</span><span class="sxs-lookup"><span data-stu-id="cc3fe-194">server.LocalIpAddress</span></span>  | `String` | |    
| <span data-ttu-id="cc3fe-195">server.LocalPort</span><span class="sxs-lookup"><span data-stu-id="cc3fe-195">server.LocalPort</span></span>  | `String` | |    
| <span data-ttu-id="cc3fe-196">server.IsLocal</span><span class="sxs-lookup"><span data-stu-id="cc3fe-196">server.IsLocal</span></span>  | `bool` | |    
| <span data-ttu-id="cc3fe-197">server.OnSendingHeaders</span><span class="sxs-lookup"><span data-stu-id="cc3fe-197">server.OnSendingHeaders</span></span>  | `Action<Action<object>,object>` | |


### <a name="sendfiles-v030"></a><span data-ttu-id="cc3fe-198">SendFiles v0.3.0</span><span class="sxs-lookup"><span data-stu-id="cc3fe-198">SendFiles v0.3.0</span></span>

| <span data-ttu-id="cc3fe-199">Chave</span><span class="sxs-lookup"><span data-stu-id="cc3fe-199">Key</span></span>               | <span data-ttu-id="cc3fe-200">Valor (tipo)</span><span class="sxs-lookup"><span data-stu-id="cc3fe-200">Value (type)</span></span> | <span data-ttu-id="cc3fe-201">Descrição</span><span class="sxs-lookup"><span data-stu-id="cc3fe-201">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="cc3fe-202">sendfile.SendAsync</span><span class="sxs-lookup"><span data-stu-id="cc3fe-202">sendfile.SendAsync</span></span> | <span data-ttu-id="cc3fe-203">Consulte [assinatura do delegado](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="cc3fe-203">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span> | <span data-ttu-id="cc3fe-204">Por solicitação</span><span class="sxs-lookup"><span data-stu-id="cc3fe-204">Per Request</span></span> |


### <a name="opaque-v030"></a><span data-ttu-id="cc3fe-205">V0.3.0 opaco</span><span class="sxs-lookup"><span data-stu-id="cc3fe-205">Opaque v0.3.0</span></span>

| <span data-ttu-id="cc3fe-206">Chave</span><span class="sxs-lookup"><span data-stu-id="cc3fe-206">Key</span></span>               | <span data-ttu-id="cc3fe-207">Valor (tipo)</span><span class="sxs-lookup"><span data-stu-id="cc3fe-207">Value (type)</span></span> | <span data-ttu-id="cc3fe-208">Descrição</span><span class="sxs-lookup"><span data-stu-id="cc3fe-208">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="cc3fe-209">opaque.Version</span><span class="sxs-lookup"><span data-stu-id="cc3fe-209">opaque.Version</span></span> | `String` |  |
| <span data-ttu-id="cc3fe-210">opaque.Upgrade</span><span class="sxs-lookup"><span data-stu-id="cc3fe-210">opaque.Upgrade</span></span> | `OpaqueUpgrade` | <span data-ttu-id="cc3fe-211">Consulte [assinatura do delegado](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="cc3fe-211">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span> |
| <span data-ttu-id="cc3fe-212">opaque.Stream</span><span class="sxs-lookup"><span data-stu-id="cc3fe-212">opaque.Stream</span></span> | `Stream` |  |
| <span data-ttu-id="cc3fe-213">opaque.CallCancelled</span><span class="sxs-lookup"><span data-stu-id="cc3fe-213">opaque.CallCancelled</span></span> | `CancellationToken` |  |


### <a name="websocket-v030"></a><span data-ttu-id="cc3fe-214">V0.3.0 WebSocket</span><span class="sxs-lookup"><span data-stu-id="cc3fe-214">WebSocket v0.3.0</span></span>

| <span data-ttu-id="cc3fe-215">Chave</span><span class="sxs-lookup"><span data-stu-id="cc3fe-215">Key</span></span>               | <span data-ttu-id="cc3fe-216">Valor (tipo)</span><span class="sxs-lookup"><span data-stu-id="cc3fe-216">Value (type)</span></span> | <span data-ttu-id="cc3fe-217">Descrição</span><span class="sxs-lookup"><span data-stu-id="cc3fe-217">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="cc3fe-218">websocket.Version</span><span class="sxs-lookup"><span data-stu-id="cc3fe-218">websocket.Version</span></span> | `String` |  |
| <span data-ttu-id="cc3fe-219">websocket.Accept</span><span class="sxs-lookup"><span data-stu-id="cc3fe-219">websocket.Accept</span></span> | `WebSocketAccept` | <span data-ttu-id="cc3fe-220">Consulte [assinatura do delegado](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="cc3fe-220">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span> |
| <span data-ttu-id="cc3fe-221">websocket.AcceptAlt</span><span class="sxs-lookup"><span data-stu-id="cc3fe-221">websocket.AcceptAlt</span></span> |  | <span data-ttu-id="cc3fe-222">Não-spec</span><span class="sxs-lookup"><span data-stu-id="cc3fe-222">Non-spec</span></span> |
| <span data-ttu-id="cc3fe-223">websocket.SubProtocol</span><span class="sxs-lookup"><span data-stu-id="cc3fe-223">websocket.SubProtocol</span></span> | `String` | <span data-ttu-id="cc3fe-224">Consulte [RFC6455 seção 4.2.2](https://tools.ietf.org/html/rfc6455#section-4.2.2) etapa 5.5</span><span class="sxs-lookup"><span data-stu-id="cc3fe-224">See [RFC6455 Section 4.2.2](https://tools.ietf.org/html/rfc6455#section-4.2.2) Step 5.5</span></span> |
| <span data-ttu-id="cc3fe-225">websocket.SendAsync</span><span class="sxs-lookup"><span data-stu-id="cc3fe-225">websocket.SendAsync</span></span> | `WebSocketSendAsync` | <span data-ttu-id="cc3fe-226">Consulte [assinatura do delegado](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="cc3fe-226">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span>  |
| <span data-ttu-id="cc3fe-227">websocket.ReceiveAsync</span><span class="sxs-lookup"><span data-stu-id="cc3fe-227">websocket.ReceiveAsync</span></span> | `WebSocketReceiveAsync` | <span data-ttu-id="cc3fe-228">Consulte [assinatura do delegado](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="cc3fe-228">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span>  |
| <span data-ttu-id="cc3fe-229">websocket.CloseAsync</span><span class="sxs-lookup"><span data-stu-id="cc3fe-229">websocket.CloseAsync</span></span> | `WebSocketCloseAsync` | <span data-ttu-id="cc3fe-230">Consulte [assinatura do delegado](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="cc3fe-230">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span>  |
| <span data-ttu-id="cc3fe-231">websocket.CallCancelled</span><span class="sxs-lookup"><span data-stu-id="cc3fe-231">websocket.CallCancelled</span></span> | `CancellationToken` |  |
| <span data-ttu-id="cc3fe-232">websocket.ClientCloseStatus</span><span class="sxs-lookup"><span data-stu-id="cc3fe-232">websocket.ClientCloseStatus</span></span> | `int` | <span data-ttu-id="cc3fe-233">Opcional</span><span class="sxs-lookup"><span data-stu-id="cc3fe-233">Optional</span></span> |
| <span data-ttu-id="cc3fe-234">websocket.ClientCloseDescription</span><span class="sxs-lookup"><span data-stu-id="cc3fe-234">websocket.ClientCloseDescription</span></span> | `String` | <span data-ttu-id="cc3fe-235">Opcional</span><span class="sxs-lookup"><span data-stu-id="cc3fe-235">Optional</span></span> |


## <a name="additional-resources"></a><span data-ttu-id="cc3fe-236">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="cc3fe-236">Additional Resources</span></span>

* [<span data-ttu-id="cc3fe-237">Middleware</span><span class="sxs-lookup"><span data-stu-id="cc3fe-237">Middleware</span></span>](middleware.md)

* [<span data-ttu-id="cc3fe-238">Servidores</span><span class="sxs-lookup"><span data-stu-id="cc3fe-238">Servers</span></span>](servers/index.md)

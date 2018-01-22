---
title: "Implementação do servidor de web HTTP. sys no núcleo do ASP.NET"
author: rick-anderson
description: "Apresenta o HTTP. sys, um servidor web para o ASP.NET Core no Windows. Criado com o driver de modo kernel HTTP. sys, o HTTP. sys é uma alternativa ao Kestrel que pode ser usada para conexão direta com a Internet sem o IIS."
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/httpsys
ms.openlocfilehash: 60301e1e3eb96f51e86ef9f8be61f5fd8a4c009c
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/19/2018
---
# <a name="httpsys-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="f509f-104">Implementação do servidor de web HTTP. sys no núcleo do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="f509f-104">HTTP.sys web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="f509f-105">Por [Tom Dykstra](https://github.com/tdykstra) e [Ross Carlos](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="f509f-105">By [Tom Dykstra](https://github.com/tdykstra) and [Chris Ross](https://github.com/Tratcher)</span></span>

> [!NOTE]
> <span data-ttu-id="f509f-106">Este tópico se aplica somente ao ASP.NET Core 2.0 e versões posteriores.</span><span class="sxs-lookup"><span data-stu-id="f509f-106">This topic applies only to ASP.NET Core 2.0 and later.</span></span> <span data-ttu-id="f509f-107">Em versões anteriores do ASP.NET Core, é denominado HTTP. sys [WebListener](xref:fundamentals/servers/weblistener).</span><span class="sxs-lookup"><span data-stu-id="f509f-107">In earlier versions of ASP.NET Core, HTTP.sys is named [WebListener](xref:fundamentals/servers/weblistener).</span></span>

<span data-ttu-id="f509f-108">HTTP.sys é um [servidor web para o ASP.NET Core](index.md) que é executado somente no Windows.</span><span class="sxs-lookup"><span data-stu-id="f509f-108">HTTP.sys is a [web server for ASP.NET Core](index.md) that runs only on Windows.</span></span> <span data-ttu-id="f509f-109">Ele é criado no [driver de modo kernel HTTP. sys](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span><span class="sxs-lookup"><span data-stu-id="f509f-109">It's built on the [Http.Sys kernel mode driver](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span> <span data-ttu-id="f509f-110">HTTP.sys é uma alternativa ao [Kestrel](kestrel.md) que oferece alguns recursos que não Kestel.</span><span class="sxs-lookup"><span data-stu-id="f509f-110">HTTP.sys is an alternative to [Kestrel](kestrel.md) that offers some features that Kestel doesn't.</span></span> <span data-ttu-id="f509f-111">**HTTP.sys não pode ser usado com o IIS ou IIS Express, pois ele é incompatível com o [ASP.NET Core módulo](aspnet-core-module.md).**</span><span class="sxs-lookup"><span data-stu-id="f509f-111">**HTTP.sys can't be used with IIS or IIS Express, as it's incompatible with the [ASP.NET Core Module](aspnet-core-module.md).**</span></span>

<span data-ttu-id="f509f-112">HTTP.sys aceita os seguintes recursos:</span><span class="sxs-lookup"><span data-stu-id="f509f-112">HTTP.sys supports the following features:</span></span>

- [<span data-ttu-id="f509f-113">Autenticação do Windows</span><span class="sxs-lookup"><span data-stu-id="f509f-113">Windows Authentication</span></span>](xref:security/authentication/windowsauth)
- <span data-ttu-id="f509f-114">Compartilhamento de porta</span><span class="sxs-lookup"><span data-stu-id="f509f-114">Port sharing</span></span>
- <span data-ttu-id="f509f-115">HTTPS com SNI</span><span class="sxs-lookup"><span data-stu-id="f509f-115">HTTPS with SNI</span></span>
- <span data-ttu-id="f509f-116">HTTP/2 por TLS (Windows 10)</span><span class="sxs-lookup"><span data-stu-id="f509f-116">HTTP/2 over TLS (Windows 10)</span></span>
- <span data-ttu-id="f509f-117">Transmissão de arquivo direto</span><span class="sxs-lookup"><span data-stu-id="f509f-117">Direct file transmission</span></span>
- <span data-ttu-id="f509f-118">O cache de resposta</span><span class="sxs-lookup"><span data-stu-id="f509f-118">Response caching</span></span>
- <span data-ttu-id="f509f-119">WebSocket (Windows 8)</span><span class="sxs-lookup"><span data-stu-id="f509f-119">WebSockets (Windows 8)</span></span>

<span data-ttu-id="f509f-120">Versões com suporte do Windows:</span><span class="sxs-lookup"><span data-stu-id="f509f-120">Supported Windows versions:</span></span>

- <span data-ttu-id="f509f-121">Windows 7 e Windows Server 2008 R2 e posterior</span><span class="sxs-lookup"><span data-stu-id="f509f-121">Windows 7 and Windows Server 2008 R2 and later</span></span>

<span data-ttu-id="f509f-122">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f509f-122">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-httpsys"></a><span data-ttu-id="f509f-123">Quando usar o HTTP. sys</span><span class="sxs-lookup"><span data-stu-id="f509f-123">When to use HTTP.sys</span></span>

<span data-ttu-id="f509f-124">O HTTP. sys é útil para implantações em que você precisa expor o servidor diretamente à Internet sem usar o IIS.</span><span class="sxs-lookup"><span data-stu-id="f509f-124">HTTP.sys is useful for deployments where you need to expose the server directly to the Internet without using IIS.</span></span>

![O HTTP.sys se comunica diretamente com a Internet](httpsys/_static/httpsys-to-internet.png)

<span data-ttu-id="f509f-126">Como ele se baseia no HTTP. sys, HTTP.sys não requer um servidor de proxy reverso para proteção contra ataques.</span><span class="sxs-lookup"><span data-stu-id="f509f-126">Because it's built on Http.Sys, HTTP.sys doesn't require a reverse proxy server for protection against attacks.</span></span> <span data-ttu-id="f509f-127">Http. sys é uma tecnologia desenvolvida que protege contra vários tipos de ataques e fornece a robustez, segurança e escalabilidade de um servidor web completo.</span><span class="sxs-lookup"><span data-stu-id="f509f-127">Http.Sys is mature technology that protects against many kinds of attacks and provides the robustness, security, and scalability of a full-featured web server.</span></span> <span data-ttu-id="f509f-128">O próprio IIS é executado como um ouvinte HTTP sobre HTTP. sys.</span><span class="sxs-lookup"><span data-stu-id="f509f-128">IIS itself runs as an HTTP listener on top of Http.Sys.</span></span> 

<span data-ttu-id="f509f-129">HTTP.sys é uma boa escolha para implantações internas quando precisar de um recurso não está disponível no Kestrel, como autenticação do Windows.</span><span class="sxs-lookup"><span data-stu-id="f509f-129">HTTP.sys is a good choice for internal deployments when you need a feature not available in Kestrel, such as Windows authentication.</span></span>

![O HTTP.sys se comunica diretamente com a rede interna](httpsys/_static/httpsys-to-internal.png)

## <a name="how-to-use-httpsys"></a><span data-ttu-id="f509f-131">Como usar o HTTP. sys</span><span class="sxs-lookup"><span data-stu-id="f509f-131">How to use HTTP.sys</span></span>

<span data-ttu-id="f509f-132">Aqui está uma visão geral das tarefas de configuração para o sistema operacional do host e seu aplicativo ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f509f-132">Here's an overview of setup tasks for the host OS and your ASP.NET Core application.</span></span>

### <a name="configure-windows-server"></a><span data-ttu-id="f509f-133">Configurar o Windows Server</span><span class="sxs-lookup"><span data-stu-id="f509f-133">Configure Windows Server</span></span>

* <span data-ttu-id="f509f-134">Instale a versão do .NET que seu aplicativo requer, como [.NET Core](https://www.microsoft.com/net/download/core) ou [do .NET Framework](https://www.microsoft.com/net/download/framework).</span><span class="sxs-lookup"><span data-stu-id="f509f-134">Install the version of .NET that your application requires, such as [.NET Core](https://www.microsoft.com/net/download/core) or [.NET Framework](https://www.microsoft.com/net/download/framework).</span></span>

* <span data-ttu-id="f509f-135">Pré-registrar prefixos de URL para associar ao HTTP. sys e configurar certificados SSL</span><span class="sxs-lookup"><span data-stu-id="f509f-135">Preregister URL prefixes to bind to HTTP.sys, and set up SSL certificates</span></span>

   <span data-ttu-id="f509f-136">Se você não pré-registrar prefixos de URL no Windows, você precisa executar seu aplicativo com privilégios de administrador.</span><span class="sxs-lookup"><span data-stu-id="f509f-136">If you don't preregister URL prefixes in Windows, you have to run your application with administrator privileges.</span></span> <span data-ttu-id="f509f-137">A única exceção é se você associar ao localhost usando HTTP (não HTTPS) com um número de porta maior que 1024; Nesse caso, privilégios de administrador não são necessários.</span><span class="sxs-lookup"><span data-stu-id="f509f-137">The only exception is if you bind to localhost using HTTP (not HTTPS) with a port number greater than 1024; in that case, administrator privileges aren't required.</span></span>

   <span data-ttu-id="f509f-138">Para obter detalhes, consulte [como pré-registrar prefixos e configurar o SSL](#preregister-url-prefixes-and-configure-ssl) posteriormente neste artigo.</span><span class="sxs-lookup"><span data-stu-id="f509f-138">For details, see [How to preregister prefixes and configure SSL](#preregister-url-prefixes-and-configure-ssl) later in this article.</span></span>

* <span data-ttu-id="f509f-139">Abrir portas de firewall para permitir o tráfego alcançar o HTTP. sys.</span><span class="sxs-lookup"><span data-stu-id="f509f-139">Open firewall ports to allow traffic to reach HTTP.sys.</span></span>

   <span data-ttu-id="f509f-140">Você pode usar *netsh.exe* ou [cmdlets do PowerShell](https://technet.microsoft.com/library/jj554906).</span><span class="sxs-lookup"><span data-stu-id="f509f-140">You can use *netsh.exe* or [PowerShell cmdlets](https://technet.microsoft.com/library/jj554906).</span></span>

<span data-ttu-id="f509f-141">Também há [configurações de registro Http.Sys](https://support.microsoft.com/kb/820129).</span><span class="sxs-lookup"><span data-stu-id="f509f-141">There are also [Http.Sys registry settings](https://support.microsoft.com/kb/820129).</span></span>

### <a name="configure-your-aspnet-core-application-to-use-httpsys"></a><span data-ttu-id="f509f-142">Configurar seu aplicativo ASP.NET Core para usar o HTTP. sys</span><span class="sxs-lookup"><span data-stu-id="f509f-142">Configure your ASP.NET Core application to use HTTP.sys</span></span>

* <span data-ttu-id="f509f-143">Nenhuma instalação de pacote é necessária se você usar o [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage.</span><span class="sxs-lookup"><span data-stu-id="f509f-143">No package install is needed if you use the [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage.</span></span> <span data-ttu-id="f509f-144">O [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/) pacote está incluído no metapackage.</span><span class="sxs-lookup"><span data-stu-id="f509f-144">The [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/) package is included in the metapackage.</span></span>

* <span data-ttu-id="f509f-145">Chamar o `UseHttpSys` método de extensão no `WebHostBuilder` no seu `Main` método, especificando um [opções de HTTP. sys](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs) que você precisa, como mostrado no exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="f509f-145">Call the `UseHttpSys` extension method on `WebHostBuilder` in your `Main` method, specifying any [HTTP.sys options](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs) that you need, as shown in the following example:</span></span>

  [!code-csharp[](httpsys/sample/Program.cs?name=snippet_Main&highlight=11-19)]

### <a name="configure-httpsys-options"></a><span data-ttu-id="f509f-146">Configurar opções de HTTP. sys</span><span class="sxs-lookup"><span data-stu-id="f509f-146">Configure HTTP.sys options</span></span>

<span data-ttu-id="f509f-147">Aqui estão algumas das configurações de HTTP. sys e limites que você pode configurar.</span><span class="sxs-lookup"><span data-stu-id="f509f-147">Here are some of the HTTP.sys settings and limits that you can configure.</span></span>

<span data-ttu-id="f509f-148">**Conexões de cliente máximo**</span><span class="sxs-lookup"><span data-stu-id="f509f-148">**Maximum client connections**</span></span>

<span data-ttu-id="f509f-149">O número máximo de conexões simultâneas de TCP abertas pode ser definido para todo o aplicativo com o código a seguir no *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="f509f-149">The maximum number of concurrent open TCP connections can be set for the entire application with the following code in *Program.cs*:</span></span>

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Options&highlight=5)]

<span data-ttu-id="f509f-150">O número máximo de conexões é ilimitado (null) por padrão.</span><span class="sxs-lookup"><span data-stu-id="f509f-150">The maximum number of connections is unlimited (null) by default.</span></span>

<span data-ttu-id="f509f-151">**Tamanho do corpo da solicitação máxima**</span><span class="sxs-lookup"><span data-stu-id="f509f-151">**Maximum request body size**</span></span>

<span data-ttu-id="f509f-152">O tamanho do corpo da solicitação máximo padrão é 30.000.000 bytes, que é aproximadamente 28.6 MB.</span><span class="sxs-lookup"><span data-stu-id="f509f-152">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6MB.</span></span>

<span data-ttu-id="f509f-153">A maneira recomendada para substituir o limite em um aplicativo ASP.NET MVC de núcleo é usar o [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) atributo em um método de ação:</span><span class="sxs-lookup"><span data-stu-id="f509f-153">The recommended way to override the limit in an ASP.NET Core MVC app is to use the [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="f509f-154">Aqui está um exemplo que mostra como configurar o limite para o aplicativo inteiro, cada solicitação:</span><span class="sxs-lookup"><span data-stu-id="f509f-154">Here's an example that shows how to configure the constraint for the entire application, every request:</span></span>

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Options&highlight=6)]

<span data-ttu-id="f509f-155">Você pode substituir a configuração de uma solicitação específica *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="f509f-155">You can override the setting on a specific request in *Startup.cs*:</span></span>

[!code-csharp[](httpsys/sample/Startup.cs?name=snippet_Configure&highlight=9-10)]
 
<span data-ttu-id="f509f-156">Uma exceção é gerada se você tentar configurar o limite de uma solicitação depois que o aplicativo começou a ler a solicitação.</span><span class="sxs-lookup"><span data-stu-id="f509f-156">An exception is thrown if you try to configure the limit on a request after the application has started reading the request.</span></span> <span data-ttu-id="f509f-157">Há um `IsReadOnly` propriedade que indica se o `MaxRequestBodySize` propriedade está no estado somente leitura, que significa que é muito tarde para configurar o limite.</span><span class="sxs-lookup"><span data-stu-id="f509f-157">There's an `IsReadOnly` property that tells you if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="f509f-158">Para obter informações sobre outras opções de HTTP. sys, consulte [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="f509f-158">For information about other HTTP.sys options, see [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs).</span></span> 

### <a name="configure-urls-and-ports-to-listen-on"></a><span data-ttu-id="f509f-159">Configurar URLs e portas para escutar em</span><span class="sxs-lookup"><span data-stu-id="f509f-159">Configure URLs and ports to listen on</span></span> 

<span data-ttu-id="f509f-160">Por padrão o ASP.NET Core associa a `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="f509f-160">By default ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="f509f-161">Para configurar portas e prefixos de URL, você pode usar o `UseUrls` método de extensão, o `urls` argumento de linha de comando, a variável de ambiente ASPNETCORE_URLS ou `UrlPrefixes` propriedade [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="f509f-161">To configure URL prefixes and ports, you can use the `UseUrls` extension method, the `urls` command-line argument, the ASPNETCORE_URLS environment variable, or the `UrlPrefixes` property on [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs).</span></span> <span data-ttu-id="f509f-162">O seguinte exemplo de código usa `UrlPrefixes`.</span><span class="sxs-lookup"><span data-stu-id="f509f-162">The following code example uses `UrlPrefixes`.</span></span>

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Main&highlight=17)]

<span data-ttu-id="f509f-163">Uma vantagem de `UrlPrefixes` é que você receber uma mensagem de erro imediatamente se você tentar adicionar um prefixo que é formatado errado.</span><span class="sxs-lookup"><span data-stu-id="f509f-163">An advantage of `UrlPrefixes` is that you get an error message immediately if you try to add a prefix that is formatted wrong.</span></span> <span data-ttu-id="f509f-164">Uma vantagem de `UseUrls` (compartilhado com `urls` e ASPNETCORE_URLS) é que você pode mais facilmente alternar entre Kestrel e HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="f509f-164">An advantage of `UseUrls` (shared with `urls` and ASPNETCORE_URLS) is that you can more easily switch between Kestrel and HTTP.sys.</span></span>

<span data-ttu-id="f509f-165">Se você usar ambos `UseUrls` (ou `urls` ou ASPNETCORE_URLS) e `UrlPrefixes`, as configurações no `UrlPrefixes` substituir os `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="f509f-165">If you use both `UseUrls` (or `urls` or ASPNETCORE_URLS) and `UrlPrefixes`, the settings in `UrlPrefixes` override the ones in `UseUrls`.</span></span> <span data-ttu-id="f509f-166">Para obter mais informações, consulte [Hospedagem](xref:fundamentals/hosting).</span><span class="sxs-lookup"><span data-stu-id="f509f-166">For more information, see [Hosting](xref:fundamentals/hosting).</span></span>

<span data-ttu-id="f509f-167">Usa o HTTP.sys o [HTTP Server API UrlPrefix formatos de cadeia de caracteres](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span><span class="sxs-lookup"><span data-stu-id="f509f-167">HTTP.sys uses the [HTTP Server API UrlPrefix string formats](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="f509f-168">Certifique-se de que você especificar as cadeias de caracteres de prefixo mesmo em `UseUrls` ou `UrlPrefixes` que pré-registrar no servidor.</span><span class="sxs-lookup"><span data-stu-id="f509f-168">Make sure that you specify the same prefix strings in `UseUrls` or `UrlPrefixes` that you preregister on the server.</span></span> 

### <a name="dont-use-iis"></a><span data-ttu-id="f509f-169">Não use o IIS</span><span class="sxs-lookup"><span data-stu-id="f509f-169">Don't use IIS</span></span>

<span data-ttu-id="f509f-170">Verifique se que seu aplicativo não está configurado para executar o IIS ou IIS Express.</span><span class="sxs-lookup"><span data-stu-id="f509f-170">Make sure your application isn't configured to run IIS or IIS Express.</span></span>

<span data-ttu-id="f509f-171">No Visual Studio, o perfil de inicialização padrão é para o IIS Express.</span><span class="sxs-lookup"><span data-stu-id="f509f-171">In Visual Studio, the default launch profile is for IIS Express.</span></span> <span data-ttu-id="f509f-172">Para executar o projeto como um aplicativo de console, altere manualmente o perfil selecionado, conforme mostrado na captura de tela a seguir.</span><span class="sxs-lookup"><span data-stu-id="f509f-172">To run the project as a console application, manually change the selected profile, as shown in the following screen shot.</span></span>

![Selecione o perfil de aplicativo de console](httpsys/_static/vs-choose-profile.png)

## <a name="preregister-url-prefixes-and-configure-ssl"></a><span data-ttu-id="f509f-174">Pré-registrar prefixos de URL e configurar o SSL</span><span class="sxs-lookup"><span data-stu-id="f509f-174">Preregister URL prefixes and configure SSL</span></span>

<span data-ttu-id="f509f-175">O IIS e o HTTP. sys contam com o driver de modo de kernel HTTP. sys subjacente para escutar solicitações e processamento inicial.</span><span class="sxs-lookup"><span data-stu-id="f509f-175">Both IIS and HTTP.sys rely on the underlying Http.Sys kernel mode driver to listen for requests and do initial processing.</span></span> <span data-ttu-id="f509f-176">No IIS, a interface do usuário de gerenciamento fornece uma maneira relativamente fácil de configurar tudo.</span><span class="sxs-lookup"><span data-stu-id="f509f-176">In IIS, the management UI gives you a relatively easy way to configure everything.</span></span> <span data-ttu-id="f509f-177">No entanto, você precisa configurar o HTTP. sys por conta própria.</span><span class="sxs-lookup"><span data-stu-id="f509f-177">However, you need to configure Http.Sys yourself.</span></span> <span data-ttu-id="f509f-178">A ferramenta interna para fazer isto é *netsh.exe*.</span><span class="sxs-lookup"><span data-stu-id="f509f-178">The built-in tool for doing that is *netsh.exe*.</span></span> 

<span data-ttu-id="f509f-179">Com *netsh.exe* pode reservar prefixos de URL e atribuir certificados SSL.</span><span class="sxs-lookup"><span data-stu-id="f509f-179">With *netsh.exe* you can reserve URL prefixes and assign SSL certificates.</span></span> <span data-ttu-id="f509f-180">A ferramenta requer privilégios administrativos.</span><span class="sxs-lookup"><span data-stu-id="f509f-180">The tool requires administrative privileges.</span></span>

<span data-ttu-id="f509f-181">O exemplo a seguir mostra o mínimo necessário para reservar os prefixos de URL para as portas 80 e 443:</span><span class="sxs-lookup"><span data-stu-id="f509f-181">The following example shows the minimum needed to reserve URL prefixes for ports 80 and 443:</span></span>

```console
netsh http add urlacl url=http://+:80/ user=Users
netsh http add urlacl url=https://+:443/ user=Users
```

<span data-ttu-id="f509f-182">O exemplo a seguir mostra como atribuir um certificado SSL:</span><span class="sxs-lookup"><span data-stu-id="f509f-182">The following example shows how to assign an SSL certificate:</span></span>

```console
netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid={00000000-0000-0000-0000-000000000000}"
```

<span data-ttu-id="f509f-183">Aqui está a documentação de referência *netsh.exe*:</span><span class="sxs-lookup"><span data-stu-id="f509f-183">Here is the reference documentation for *netsh.exe*:</span></span>

* [<span data-ttu-id="f509f-184">Comandos Netsh para Hypertext Transfer protocolo (HTTP)</span><span class="sxs-lookup"><span data-stu-id="f509f-184">Netsh Commands for Hypertext Transfer Protocol (HTTP)</span></span>](https://technet.microsoft.com/library/cc725882.aspx)
* [<span data-ttu-id="f509f-185">Cadeias de caracteres de UrlPrefix</span><span class="sxs-lookup"><span data-stu-id="f509f-185">UrlPrefix Strings</span></span>](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

<span data-ttu-id="f509f-186">Os recursos a seguir fornecem instruções detalhadas para vários cenários.</span><span class="sxs-lookup"><span data-stu-id="f509f-186">The following resources provide detailed instructions for several scenarios.</span></span> <span data-ttu-id="f509f-187">Os artigos que fazem referência a HttpListener se aplicam igualmente para http. sys, pois ambas se baseiam em http. sys.</span><span class="sxs-lookup"><span data-stu-id="f509f-187">Articles that refer to HttpListener apply equally to HTTP.sys, as both are based on Http.Sys.</span></span>

* [<span data-ttu-id="f509f-188">Como configurar uma porta com um certificado SSL</span><span class="sxs-lookup"><span data-stu-id="f509f-188">How to: Configure a Port with an SSL Certificate</span></span>](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-configure-a-port-with-an-ssl-certificate)
* <span data-ttu-id="f509f-189">[A comunicação HTTPS - HttpListener com base em certificação de cliente e hospedagem](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) isso é um blog de terceiros e é bastante antigo, mas ainda tem informações úteis.</span><span class="sxs-lookup"><span data-stu-id="f509f-189">[HTTPS Communication - HttpListener based Hosting and Client Certification](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) This is a third-party blog and is fairly old but still has useful information.</span></span>
* <span data-ttu-id="f509f-190">[Como: Passo a passo usando HttpListener ou o servidor Http não gerenciado código (C++) como um servidor simples SSL](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) muito trata um blog mais antigo com informações úteis.</span><span class="sxs-lookup"><span data-stu-id="f509f-190">[How To: Walkthrough Using HttpListener or Http Server unmanaged code (C++) as an SSL Simple Server](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) This too is an older blog with useful information.</span></span>

<span data-ttu-id="f509f-191">Aqui estão algumas ferramentas de terceiros que podem ser mais fácil de usar do que o *netsh.exe* linha de comando.</span><span class="sxs-lookup"><span data-stu-id="f509f-191">Here are some third-party tools that can be easier to use than the *netsh.exe* command line.</span></span> <span data-ttu-id="f509f-192">Eles não são fornecidos pelo ou aprovados pela Microsoft.</span><span class="sxs-lookup"><span data-stu-id="f509f-192">These are not provided by or endorsed by Microsoft.</span></span> <span data-ttu-id="f509f-193">As ferramentas de executar como administrador por padrão, como *netsh.exe* em si requer privilégios de administrador.</span><span class="sxs-lookup"><span data-stu-id="f509f-193">The tools run as administrator by default, since *netsh.exe* itself requires administrator privileges.</span></span>

* <span data-ttu-id="f509f-194">[Gerenciador de HTTP. sys](http://httpsysmanager.codeplex.com/) fornece a interface de usuário para a listagem e configurar certificados SSL e opções, as reservas de prefixo e listas de certificados confiáveis.</span><span class="sxs-lookup"><span data-stu-id="f509f-194">[http.sys Manager](http://httpsysmanager.codeplex.com/) provides UI for listing and configuring SSL certificates and options, prefix reservations, and certificate trust lists.</span></span> 
* <span data-ttu-id="f509f-195">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) permite que você lista ou configurar certificados SSL e prefixos de URL.</span><span class="sxs-lookup"><span data-stu-id="f509f-195">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) lets you list or configure SSL certificates and URL prefixes.</span></span> <span data-ttu-id="f509f-196">A interface do usuário é mais refinada do que o HTTP. sys Gerenciador e expõe algumas opções adicionais de configuração, mas caso contrário, ele fornece uma funcionalidade semelhante.</span><span class="sxs-lookup"><span data-stu-id="f509f-196">The UI is more refined than http.sys Manager and exposes a few more configuration options, but otherwise it provides similar functionality.</span></span> <span data-ttu-id="f509f-197">Não é possível criar uma nova lista de certificados confiáveis (CTL), mas pode atribuir os existentes.</span><span class="sxs-lookup"><span data-stu-id="f509f-197">It cannot create a new certificate trust list (CTL), but can assign existing ones.</span></span>

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

## <a name="next-steps"></a><span data-ttu-id="f509f-198">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="f509f-198">Next steps</span></span>

<span data-ttu-id="f509f-199">Para obter mais informações, consulte os seguintes recursos:</span><span class="sxs-lookup"><span data-stu-id="f509f-199">For more information, see the following resources:</span></span>

* [<span data-ttu-id="f509f-200">Aplicativo de exemplo para este artigo</span><span class="sxs-lookup"><span data-stu-id="f509f-200">Sample app for this article</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample)
* [<span data-ttu-id="f509f-201">Código-fonte HTTP. sys</span><span class="sxs-lookup"><span data-stu-id="f509f-201">HTTP.sys source code</span></span>](https://github.com/aspnet/HttpSysServer/)
* [<span data-ttu-id="f509f-202">Hospedagem</span><span class="sxs-lookup"><span data-stu-id="f509f-202">Hosting</span></span>](xref:fundamentals/hosting)

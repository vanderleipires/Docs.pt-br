---
title: "Implementação do servidor Web HTTP.sys no ASP.NET Core"
author: rick-anderson
description: "Apresenta o HTTP.sys, um servidor Web para o ASP.NET Core no Windows. Desenvolvido com base no driver de modo kernel Http.Sys, o HTTP.sys é uma alternativa ao Kestrel que pode ser usada para conexão direta com a Internet sem o IIS."
manager: wpickett
ms.author: riande
ms.date: 08/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/httpsys
ms.openlocfilehash: f36a86fc67e715165afd0c38992f9767f90cf3b4
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/30/2018
---
# <a name="httpsys-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="35861-104">Implementação do servidor Web HTTP.sys no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="35861-104">HTTP.sys web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="35861-105">Por [Tom Dykstra](https://github.com/tdykstra) e [Chris Ross](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="35861-105">By [Tom Dykstra](https://github.com/tdykstra) and [Chris Ross](https://github.com/Tratcher)</span></span>

> [!NOTE]
> <span data-ttu-id="35861-106">Este tópico se aplica somente ao ASP.NET Core 2.0 e posterior.</span><span class="sxs-lookup"><span data-stu-id="35861-106">This topic applies only to ASP.NET Core 2.0 and later.</span></span> <span data-ttu-id="35861-107">Em versões anteriores do ASP.NET Core, o HTTP.sys é chamado [WebListener](xref:fundamentals/servers/weblistener).</span><span class="sxs-lookup"><span data-stu-id="35861-107">In earlier versions of ASP.NET Core, HTTP.sys is named [WebListener](xref:fundamentals/servers/weblistener).</span></span>

<span data-ttu-id="35861-108">O HTTP.sys é um [servidor Web para o ASP.NET Core](index.md) executado somente no Windows.</span><span class="sxs-lookup"><span data-stu-id="35861-108">HTTP.sys is a [web server for ASP.NET Core](index.md) that runs only on Windows.</span></span> <span data-ttu-id="35861-109">Foi desenvolvido com base no [driver de modo kernel Http.Sys](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span><span class="sxs-lookup"><span data-stu-id="35861-109">It's built on the [Http.Sys kernel mode driver](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span> <span data-ttu-id="35861-110">O HTTP.sys é uma alternativa ao [Kestrel](kestrel.md) que oferece alguns recursos que não são oferecidos pelo Kestel.</span><span class="sxs-lookup"><span data-stu-id="35861-110">HTTP.sys is an alternative to [Kestrel](kestrel.md) that offers some features that Kestel doesn't.</span></span> <span data-ttu-id="35861-111">**O HTTP.sys não pode ser usado com o IIS ou o IIS Express, pois é incompatível com o [Módulo do ASP.NET Core](aspnet-core-module.md).**</span><span class="sxs-lookup"><span data-stu-id="35861-111">**HTTP.sys can't be used with IIS or IIS Express, as it's incompatible with the [ASP.NET Core Module](aspnet-core-module.md).**</span></span>

<span data-ttu-id="35861-112">O HTTP.sys dá suporte aos seguintes recursos:</span><span class="sxs-lookup"><span data-stu-id="35861-112">HTTP.sys supports the following features:</span></span>

- [<span data-ttu-id="35861-113">Autenticação do Windows</span><span class="sxs-lookup"><span data-stu-id="35861-113">Windows Authentication</span></span>](xref:security/authentication/windowsauth)
- <span data-ttu-id="35861-114">Compartilhamento de porta</span><span class="sxs-lookup"><span data-stu-id="35861-114">Port sharing</span></span>
- <span data-ttu-id="35861-115">HTTPS com SNI</span><span class="sxs-lookup"><span data-stu-id="35861-115">HTTPS with SNI</span></span>
- <span data-ttu-id="35861-116">HTTP/2 por TLS (Windows 10)</span><span class="sxs-lookup"><span data-stu-id="35861-116">HTTP/2 over TLS (Windows 10)</span></span>
- <span data-ttu-id="35861-117">Transmissão direta de arquivo</span><span class="sxs-lookup"><span data-stu-id="35861-117">Direct file transmission</span></span>
- <span data-ttu-id="35861-118">Cache de resposta</span><span class="sxs-lookup"><span data-stu-id="35861-118">Response caching</span></span>
- <span data-ttu-id="35861-119">WebSockets (Windows 8)</span><span class="sxs-lookup"><span data-stu-id="35861-119">WebSockets (Windows 8)</span></span>

<span data-ttu-id="35861-120">Versões do Windows compatíveis:</span><span class="sxs-lookup"><span data-stu-id="35861-120">Supported Windows versions:</span></span>

- <span data-ttu-id="35861-121">Windows 7 e Windows Server 2008 R2 e posterior</span><span class="sxs-lookup"><span data-stu-id="35861-121">Windows 7 and Windows Server 2008 R2 and later</span></span>

<span data-ttu-id="35861-122">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="35861-122">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-httpsys"></a><span data-ttu-id="35861-123">Quando usar o HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="35861-123">When to use HTTP.sys</span></span>

<span data-ttu-id="35861-124">O HTTP.sys é útil para implantações em que você precisa expor o servidor diretamente à Internet sem usar o IIS.</span><span class="sxs-lookup"><span data-stu-id="35861-124">HTTP.sys is useful for deployments where you need to expose the server directly to the Internet without using IIS.</span></span>

![O HTTP.sys se comunica diretamente com a Internet](httpsys/_static/httpsys-to-internet.png)

<span data-ttu-id="35861-126">Como se baseia no Http.Sys, o HTTP.sys não exige um servidor proxy reverso para proteção contra ataques.</span><span class="sxs-lookup"><span data-stu-id="35861-126">Because it's built on Http.Sys, HTTP.sys doesn't require a reverse proxy server for protection against attacks.</span></span> <span data-ttu-id="35861-127">O Http.Sys é uma tecnologia madura que protege contra muitos tipos de ataques e fornece a robustez, segurança e escalabilidade de um servidor Web completo.</span><span class="sxs-lookup"><span data-stu-id="35861-127">Http.Sys is mature technology that protects against many kinds of attacks and provides the robustness, security, and scalability of a full-featured web server.</span></span> <span data-ttu-id="35861-128">O próprio IIS é executado como um ouvinte HTTP no Http.Sys.</span><span class="sxs-lookup"><span data-stu-id="35861-128">IIS itself runs as an HTTP listener on top of Http.Sys.</span></span> 

<span data-ttu-id="35861-129">O HTTP.sys é uma boa escolha para implantações internas quando você precisa de um recurso que não está disponível no Kestrel, como a autenticação do Windows.</span><span class="sxs-lookup"><span data-stu-id="35861-129">HTTP.sys is a good choice for internal deployments when you need a feature not available in Kestrel, such as Windows authentication.</span></span>

![O HTTP.sys se comunica diretamente com a rede interna](httpsys/_static/httpsys-to-internal.png)

## <a name="how-to-use-httpsys"></a><span data-ttu-id="35861-131">Como usar o HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="35861-131">How to use HTTP.sys</span></span>

<span data-ttu-id="35861-132">Veja a seguir uma visão geral das tarefas de configuração para o sistema operacional do host e o aplicativo ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="35861-132">Here's an overview of setup tasks for the host OS and your ASP.NET Core application.</span></span>

### <a name="configure-windows-server"></a><span data-ttu-id="35861-133">Configurar o Windows Server</span><span class="sxs-lookup"><span data-stu-id="35861-133">Configure Windows Server</span></span>

* <span data-ttu-id="35861-134">Instale a versão do .NET exigida para o aplicativo, como o [.NET Core](https://www.microsoft.com/net/download/core) ou o [.NET Framework](https://www.microsoft.com/net/download/framework).</span><span class="sxs-lookup"><span data-stu-id="35861-134">Install the version of .NET that your application requires, such as [.NET Core](https://www.microsoft.com/net/download/core) or [.NET Framework](https://www.microsoft.com/net/download/framework).</span></span>

* <span data-ttu-id="35861-135">Pré-registrar prefixos de URL para associá-los ao HTTP.sys e configurar certificados SSL</span><span class="sxs-lookup"><span data-stu-id="35861-135">Preregister URL prefixes to bind to HTTP.sys, and set up SSL certificates</span></span>

   <span data-ttu-id="35861-136">Se você não pré-registrar prefixos de URL no Windows, precisará executar o aplicativo com privilégios de administrador.</span><span class="sxs-lookup"><span data-stu-id="35861-136">If you don't preregister URL prefixes in Windows, you have to run your application with administrator privileges.</span></span> <span data-ttu-id="35861-137">A única exceção é se você associar ao localhost usando HTTP (não HTTPS) com um número de porta maior que 1024; nesse caso, os privilégios de administrador não são necessários.</span><span class="sxs-lookup"><span data-stu-id="35861-137">The only exception is if you bind to localhost using HTTP (not HTTPS) with a port number greater than 1024; in that case, administrator privileges aren't required.</span></span>

   <span data-ttu-id="35861-138">Para obter detalhes, consulte [Como pré-registrar prefixos e configurar o SSL](#preregister-url-prefixes-and-configure-ssl) mais adiante neste artigo.</span><span class="sxs-lookup"><span data-stu-id="35861-138">For details, see [How to preregister prefixes and configure SSL](#preregister-url-prefixes-and-configure-ssl) later in this article.</span></span>

* <span data-ttu-id="35861-139">Abra as portas do firewall para permitir que o tráfego chegue ao HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="35861-139">Open firewall ports to allow traffic to reach HTTP.sys.</span></span>

   <span data-ttu-id="35861-140">Use o *netsh.exe* ou [cmdlets do PowerShell](https://technet.microsoft.com/library/jj554906).</span><span class="sxs-lookup"><span data-stu-id="35861-140">You can use *netsh.exe* or [PowerShell cmdlets](https://technet.microsoft.com/library/jj554906).</span></span>

<span data-ttu-id="35861-141">Também há [configurações de registro do Http.Sys](https://support.microsoft.com/kb/820129).</span><span class="sxs-lookup"><span data-stu-id="35861-141">There are also [Http.Sys registry settings](https://support.microsoft.com/kb/820129).</span></span>

### <a name="configure-your-aspnet-core-application-to-use-httpsys"></a><span data-ttu-id="35861-142">Configurar seu aplicativo ASP.NET Core para usar o HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="35861-142">Configure your ASP.NET Core application to use HTTP.sys</span></span>

* <span data-ttu-id="35861-143">Nenhuma instalação de pacote é necessária se o metapacote [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) for usado.</span><span class="sxs-lookup"><span data-stu-id="35861-143">No package install is needed if you use the [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage.</span></span> <span data-ttu-id="35861-144">O pacote [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/) está incluído no metapacote.</span><span class="sxs-lookup"><span data-stu-id="35861-144">The [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/) package is included in the metapackage.</span></span>

* <span data-ttu-id="35861-145">Chame o método de extensão `UseHttpSys` em `WebHostBuilder` no método `Main`, especificando as [opções do HTTP.sys](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs) necessárias, conforme mostrado no seguinte exemplo:</span><span class="sxs-lookup"><span data-stu-id="35861-145">Call the `UseHttpSys` extension method on `WebHostBuilder` in your `Main` method, specifying any [HTTP.sys options](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs) that you need, as shown in the following example:</span></span>

  [!code-csharp[](httpsys/sample/Program.cs?name=snippet_Main&highlight=11-19)]

### <a name="configure-httpsys-options"></a><span data-ttu-id="35861-146">Configurar as opções do HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="35861-146">Configure HTTP.sys options</span></span>

<span data-ttu-id="35861-147">Veja a seguir algumas das configurações e limites do HTTP.sys que você pode definir.</span><span class="sxs-lookup"><span data-stu-id="35861-147">Here are some of the HTTP.sys settings and limits that you can configure.</span></span>

<span data-ttu-id="35861-148">**Número máximo de conexões de cliente**</span><span class="sxs-lookup"><span data-stu-id="35861-148">**Maximum client connections**</span></span>

<span data-ttu-id="35861-149">O número máximo de conexões TCP abertas simultâneas pode ser definido para todo o aplicativo com o seguinte código em *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="35861-149">The maximum number of concurrent open TCP connections can be set for the entire application with the following code in *Program.cs*:</span></span>

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Options&highlight=5)]

<span data-ttu-id="35861-150">O número máximo de conexões é ilimitado (nulo) por padrão.</span><span class="sxs-lookup"><span data-stu-id="35861-150">The maximum number of connections is unlimited (null) by default.</span></span>

<span data-ttu-id="35861-151">**Tamanho máximo do corpo da solicitação**</span><span class="sxs-lookup"><span data-stu-id="35861-151">**Maximum request body size**</span></span>

<span data-ttu-id="35861-152">O tamanho máximo do corpo da solicitação padrão é de 30.000.000 bytes, que equivale aproximadamente a 28,6 MB.</span><span class="sxs-lookup"><span data-stu-id="35861-152">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6MB.</span></span>

<span data-ttu-id="35861-153">A maneira recomendada para substituir o limite em um aplicativo ASP.NET Core MVC é usar o atributo [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) em um método de ação:</span><span class="sxs-lookup"><span data-stu-id="35861-153">The recommended way to override the limit in an ASP.NET Core MVC app is to use the [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="35861-154">Este é um exemplo que mostra como configurar a restrição para todo o aplicativo, em cada solicitação:</span><span class="sxs-lookup"><span data-stu-id="35861-154">Here's an example that shows how to configure the constraint for the entire application, every request:</span></span>

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Options&highlight=6)]

<span data-ttu-id="35861-155">Substitua a configuração em uma solicitação específica em *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="35861-155">You can override the setting on a specific request in *Startup.cs*:</span></span>

[!code-csharp[](httpsys/sample/Startup.cs?name=snippet_Configure&highlight=9-10)]
 
<span data-ttu-id="35861-156">Uma exceção é gerada se você tenta configurar o limite de uma solicitação depois que o aplicativo iniciou a leitura da solicitação.</span><span class="sxs-lookup"><span data-stu-id="35861-156">An exception is thrown if you try to configure the limit on a request after the application has started reading the request.</span></span> <span data-ttu-id="35861-157">Há uma propriedade `IsReadOnly` que indica se a propriedade `MaxRequestBodySize` está no estado somente leitura, o que significa que é tarde demais para configurar o limite.</span><span class="sxs-lookup"><span data-stu-id="35861-157">There's an `IsReadOnly` property that tells you if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="35861-158">Para obter informações sobre outras opções do HTTP.sys, consulte [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="35861-158">For information about other HTTP.sys options, see [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs).</span></span> 

### <a name="configure-urls-and-ports-to-listen-on"></a><span data-ttu-id="35861-159">Configurar URLs e portas nas quais escutar</span><span class="sxs-lookup"><span data-stu-id="35861-159">Configure URLs and ports to listen on</span></span> 

<span data-ttu-id="35861-160">Por padrão, o ASP.NET Core é associado a `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="35861-160">By default ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="35861-161">Para configurar portas e prefixos de URL, use o método de extensão `UseUrls`, o argumento de linha de comando `urls`, a variável de ambiente ASPNETCORE_URLS ou a propriedade `UrlPrefixes` em [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="35861-161">To configure URL prefixes and ports, you can use the `UseUrls` extension method, the `urls` command-line argument, the ASPNETCORE_URLS environment variable, or the `UrlPrefixes` property on [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs).</span></span> <span data-ttu-id="35861-162">O exemplo de código a seguir usa `UrlPrefixes`.</span><span class="sxs-lookup"><span data-stu-id="35861-162">The following code example uses `UrlPrefixes`.</span></span>

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Main&highlight=17)]

<span data-ttu-id="35861-163">Uma vantagem de `UrlPrefixes` é que você receberá uma mensagem de erro imediatamente se tentar adicionar um prefixo que está formatado incorretamente.</span><span class="sxs-lookup"><span data-stu-id="35861-163">An advantage of `UrlPrefixes` is that you get an error message immediately if you try to add a prefix that's formatted wrong.</span></span> <span data-ttu-id="35861-164">Uma vantagem de `UseUrls` (compartilhado com `urls` e ASPNETCORE_URLS) é que você pode mudar entre o Kestrel e o HTTP.sys com mais facilidade.</span><span class="sxs-lookup"><span data-stu-id="35861-164">An advantage of `UseUrls` (shared with `urls` and ASPNETCORE_URLS) is that you can more easily switch between Kestrel and HTTP.sys.</span></span>

<span data-ttu-id="35861-165">Se você usar `UseUrls` (`urls` ou ASPNETCORE_URLS) e `UrlPrefixes`, as configurações em `UrlPrefixes` substituirão as configurações em `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="35861-165">If you use both `UseUrls` (or `urls` or ASPNETCORE_URLS) and `UrlPrefixes`, the settings in `UrlPrefixes` override the ones in `UseUrls`.</span></span> <span data-ttu-id="35861-166">Para obter mais informações, consulte [Hospedagem](xref:fundamentals/hosting).</span><span class="sxs-lookup"><span data-stu-id="35861-166">For more information, see [Hosting](xref:fundamentals/hosting).</span></span>

<span data-ttu-id="35861-167">O HTTP.sys usa os [formatos de cadeia de caracteres UrlPrefix da API do Servidor HTTP](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span><span class="sxs-lookup"><span data-stu-id="35861-167">HTTP.sys uses the [HTTP Server API UrlPrefix string formats](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="35861-168">Especifique as mesmas cadeias de caracteres de prefixo em `UseUrls` ou `UrlPrefixes` que você pré-registrou no servidor.</span><span class="sxs-lookup"><span data-stu-id="35861-168">Make sure that you specify the same prefix strings in `UseUrls` or `UrlPrefixes` that you preregister on the server.</span></span> 

### <a name="dont-use-iis"></a><span data-ttu-id="35861-169">Não usar o IIS</span><span class="sxs-lookup"><span data-stu-id="35861-169">Don't use IIS</span></span>

<span data-ttu-id="35861-170">Verifique se o aplicativo não está configurado para executar o IIS ou o IIS Express.</span><span class="sxs-lookup"><span data-stu-id="35861-170">Make sure your application isn't configured to run IIS or IIS Express.</span></span>

<span data-ttu-id="35861-171">No Visual Studio, o perfil de inicialização padrão destina-se ao IIS Express.</span><span class="sxs-lookup"><span data-stu-id="35861-171">In Visual Studio, the default launch profile is for IIS Express.</span></span> <span data-ttu-id="35861-172">Para executar o projeto como um aplicativo de console, altere manualmente o perfil selecionado, conforme mostrado na captura de tela a seguir.</span><span class="sxs-lookup"><span data-stu-id="35861-172">To run the project as a console application, manually change the selected profile, as shown in the following screen shot.</span></span>

![Selecionar o perfil do aplicativo de console](httpsys/_static/vs-choose-profile.png)

## <a name="preregister-url-prefixes-and-configure-ssl"></a><span data-ttu-id="35861-174">Pré-registrar prefixos de URL e configurar o SSL</span><span class="sxs-lookup"><span data-stu-id="35861-174">Preregister URL prefixes and configure SSL</span></span>

<span data-ttu-id="35861-175">O IIS e o HTTP.sys dependem do driver de modo kernel Http.Sys subjacente para escutar solicitações e fazer o processamento inicial.</span><span class="sxs-lookup"><span data-stu-id="35861-175">Both IIS and HTTP.sys rely on the underlying Http.Sys kernel mode driver to listen for requests and do initial processing.</span></span> <span data-ttu-id="35861-176">No IIS, a interface do usuário de gerenciamento fornece uma maneira relativamente fácil de configurar tudo.</span><span class="sxs-lookup"><span data-stu-id="35861-176">In IIS, the management UI gives you a relatively easy way to configure everything.</span></span> <span data-ttu-id="35861-177">No entanto, você precisa configurar o Http.Sys por conta própria.</span><span class="sxs-lookup"><span data-stu-id="35861-177">However, you need to configure Http.Sys yourself.</span></span> <span data-ttu-id="35861-178">A ferramenta interna para fazer isso é o *netsh.exe*.</span><span class="sxs-lookup"><span data-stu-id="35861-178">The built-in tool for doing that's *netsh.exe*.</span></span> 

<span data-ttu-id="35861-179">Com o *netsh.exe*, você pode reservar prefixos de URL e atribuir certificados SSL.</span><span class="sxs-lookup"><span data-stu-id="35861-179">With *netsh.exe* you can reserve URL prefixes and assign SSL certificates.</span></span> <span data-ttu-id="35861-180">A ferramenta exige privilégios administrativos.</span><span class="sxs-lookup"><span data-stu-id="35861-180">The tool requires administrative privileges.</span></span>

<span data-ttu-id="35861-181">O seguinte exemplo mostra o mínimo necessário para reservar prefixos de URL para as portas 80 e 443:</span><span class="sxs-lookup"><span data-stu-id="35861-181">The following example shows the minimum needed to reserve URL prefixes for ports 80 and 443:</span></span>

```console
netsh http add urlacl url=http://+:80/ user=Users
netsh http add urlacl url=https://+:443/ user=Users
```

<span data-ttu-id="35861-182">O seguinte exemplo mostra como atribuir um certificado SSL:</span><span class="sxs-lookup"><span data-stu-id="35861-182">The following example shows how to assign an SSL certificate:</span></span>

```console
netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid={00000000-0000-0000-0000-000000000000}"
```

<span data-ttu-id="35861-183">Esta é a documentação de referência do *netsh.exe*:</span><span class="sxs-lookup"><span data-stu-id="35861-183">Here is the reference documentation for *netsh.exe*:</span></span>

* [<span data-ttu-id="35861-184">Comandos do Netsh para o protocolo HTTP</span><span class="sxs-lookup"><span data-stu-id="35861-184">Netsh Commands for Hypertext Transfer Protocol (HTTP)</span></span>](https://technet.microsoft.com/library/cc725882.aspx)
* [<span data-ttu-id="35861-185">Cadeias de caracteres de UrlPrefix</span><span class="sxs-lookup"><span data-stu-id="35861-185">UrlPrefix Strings</span></span>](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

<span data-ttu-id="35861-186">Os recursos a seguir fornecem instruções detalhadas para vários cenários.</span><span class="sxs-lookup"><span data-stu-id="35861-186">The following resources provide detailed instructions for several scenarios.</span></span> <span data-ttu-id="35861-187">Os artigos que se referem ao HttpListener se aplicam igualmente ao HTTP.sys, pois ambos se baseiam no Http.Sys.</span><span class="sxs-lookup"><span data-stu-id="35861-187">Articles that refer to HttpListener apply equally to HTTP.sys, as both are based on Http.Sys.</span></span>

* [<span data-ttu-id="35861-188">Como configurar uma porta com um certificado SSL</span><span class="sxs-lookup"><span data-stu-id="35861-188">How to: Configure a Port with an SSL Certificate</span></span>](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-configure-a-port-with-an-ssl-certificate)
* <span data-ttu-id="35861-189">[HTTPS Communication – HttpListener based Hosting and Client Certification](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) (Comunicação HTTPS – Certificação de cliente e hospedagem baseada no HttpListener) Esse é um blog de terceiros e é bem antigo, mas ainda traz informações úteis.</span><span class="sxs-lookup"><span data-stu-id="35861-189">[HTTPS Communication - HttpListener based Hosting and Client Certification](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) This is a third-party blog and is fairly old but still has useful information.</span></span>
* <span data-ttu-id="35861-190">[How To: Walkthrough Using HttpListener or Http Server unmanaged code (C++) as an SSL Simple Server](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) [Passo a passo: usando um código não gerenciado (C++) do HttpListener ou do Servidor HTTP como um Servidor Simples SSL] Esse também é um blog mais antigo com informações úteis.</span><span class="sxs-lookup"><span data-stu-id="35861-190">[How To: Walkthrough Using HttpListener or Http Server unmanaged code (C++) as an SSL Simple Server](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) This too is an older blog with useful information.</span></span>

<span data-ttu-id="35861-191">Veja a seguir algumas ferramentas de terceiros que podem ser mais fáceis de usar que a linha de comando do *netsh.exe*.</span><span class="sxs-lookup"><span data-stu-id="35861-191">Here are some third-party tools that can be easier to use than the *netsh.exe* command line.</span></span> <span data-ttu-id="35861-192">Elas não são fornecidas nem endossadas pela Microsoft.</span><span class="sxs-lookup"><span data-stu-id="35861-192">These are not provided by or endorsed by Microsoft.</span></span> <span data-ttu-id="35861-193">As ferramentas são executadas como administrador por padrão, pois o próprio *netsh.exe* exige privilégios de administrador.</span><span class="sxs-lookup"><span data-stu-id="35861-193">The tools run as administrator by default, since *netsh.exe* itself requires administrator privileges.</span></span>

* <span data-ttu-id="35861-194">O [Gerenciador do http.sys](http://httpsysmanager.codeplex.com/) fornece a interface do usuário para a listagem e configuração de certificados SSL e opções, reservas de prefixo e listas de certificados confiáveis.</span><span class="sxs-lookup"><span data-stu-id="35861-194">[http.sys Manager](http://httpsysmanager.codeplex.com/) provides UI for listing and configuring SSL certificates and options, prefix reservations, and certificate trust lists.</span></span> 
* <span data-ttu-id="35861-195">O [HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) permite listar ou configurar certificados SSL e prefixos de URL.</span><span class="sxs-lookup"><span data-stu-id="35861-195">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) lets you list or configure SSL certificates and URL prefixes.</span></span> <span data-ttu-id="35861-196">A interface do usuário é mais refinada comparada ao Gerenciador do http.sys e expõe algumas outras opções de configuração, mas de outro modo fornece uma funcionalidade semelhante.</span><span class="sxs-lookup"><span data-stu-id="35861-196">The UI is more refined than http.sys Manager and exposes a few more configuration options, but otherwise it provides similar functionality.</span></span> <span data-ttu-id="35861-197">Ela não pode criar uma nova CTL (lista de certificados confiáveis), mas pode atribuir as existentes.</span><span class="sxs-lookup"><span data-stu-id="35861-197">It cannot create a new certificate trust list (CTL), but can assign existing ones.</span></span>

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

## <a name="next-steps"></a><span data-ttu-id="35861-198">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="35861-198">Next steps</span></span>

<span data-ttu-id="35861-199">Para obter mais informações, consulte os seguintes recursos:</span><span class="sxs-lookup"><span data-stu-id="35861-199">For more information, see the following resources:</span></span>

* [<span data-ttu-id="35861-200">Aplicativo de exemplo para este artigo</span><span class="sxs-lookup"><span data-stu-id="35861-200">Sample app for this article</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample)
* [<span data-ttu-id="35861-201">Código-fonte do HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="35861-201">HTTP.sys source code</span></span>](https://github.com/aspnet/HttpSysServer/)
* [<span data-ttu-id="35861-202">Hospedagem</span><span class="sxs-lookup"><span data-stu-id="35861-202">Hosting</span></span>](xref:fundamentals/hosting)

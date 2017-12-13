---
title: "Implementação do servidor web WebListener no núcleo do ASP.NET"
author: rick-anderson
description: "Apresenta WebListener, um servidor web para o ASP.NET Core no Windows. Criado com o driver de modo kernel HTTP. sys, WebListener é uma alternativa ao Kestrel que pode ser usada para conexão direta com a Internet sem o IIS."
keywords: ASP.NET Core, WebListener, HttpListener, prefixos de url, SSL
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: article
ms.assetid: 0a7286e4-6428-424e-b5c4-5c98815cf61c
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/weblistener
ms.openlocfilehash: f1abb3558546cd907c78b44d9353d9c9f1f5aff1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
# <a name="weblistener-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="79252-105">Implementação do servidor web WebListener no núcleo do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="79252-105">WebListener web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="79252-106">Por [Tom Dykstra](https://github.com/tdykstra) e [Ross Carlos](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="79252-106">By [Tom Dykstra](https://github.com/tdykstra) and [Chris Ross](https://github.com/Tratcher)</span></span>

> [!NOTE]
> <span data-ttu-id="79252-107">Este tópico se aplica somente ao ASP.NET Core 1. x.</span><span class="sxs-lookup"><span data-stu-id="79252-107">This topic applies only to ASP.NET Core 1.x.</span></span> <span data-ttu-id="79252-108">No ASP.NET 2.0 de núcleo, chamado WebListener [HTTP.sys](httpsys.md).</span><span class="sxs-lookup"><span data-stu-id="79252-108">In ASP.NET Core 2.0, WebListener is named [HTTP.sys](httpsys.md).</span></span>

<span data-ttu-id="79252-109">WebListener é um [servidor web para o ASP.NET Core](index.md) que é executado somente no Windows.</span><span class="sxs-lookup"><span data-stu-id="79252-109">WebListener is a [web server for ASP.NET Core](index.md) that runs only on Windows.</span></span> <span data-ttu-id="79252-110">Ele é criado no [driver de modo kernel HTTP. sys](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span><span class="sxs-lookup"><span data-stu-id="79252-110">It's built on the [Http.Sys kernel mode driver](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span> <span data-ttu-id="79252-111">WebListener é uma alternativa ao [Kestrel](kestrel.md) que pode ser usado para conexão direta com a Internet sem depender de IIS como um servidor de proxy reverso.</span><span class="sxs-lookup"><span data-stu-id="79252-111">WebListener is an alternative to [Kestrel](kestrel.md) that can be used for direct connection to the Internet without relying on IIS as a reverse proxy server.</span></span> <span data-ttu-id="79252-112">Na verdade, **WebListener não pode ser usado com o IIS ou IIS Express, pois ele não é compatível com o [ASP.NET Core módulo](aspnet-core-module.md).**</span><span class="sxs-lookup"><span data-stu-id="79252-112">In fact, **WebListener can't be used with IIS or IIS Express, as it isn't compatible with the [ASP.NET Core Module](aspnet-core-module.md).**</span></span>

<span data-ttu-id="79252-113">Embora WebListener foi desenvolvido para o ASP.NET Core, ele pode ser usado diretamente em qualquer aplicativo .NET Core ou do .NET Framework por meio de [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) pacote NuGet.</span><span class="sxs-lookup"><span data-stu-id="79252-113">Although WebListener was developed for ASP.NET Core, it can be used directly in any .NET Core or .NET Framework application via the [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet package.</span></span>

<span data-ttu-id="79252-114">WebListener suporta os seguintes recursos:</span><span class="sxs-lookup"><span data-stu-id="79252-114">WebListener supports the following features:</span></span>

- [<span data-ttu-id="79252-115">Autenticação do Windows</span><span class="sxs-lookup"><span data-stu-id="79252-115">Windows Authentication</span></span>](xref:security/authentication/windowsauth)
- <span data-ttu-id="79252-116">Compartilhamento de porta</span><span class="sxs-lookup"><span data-stu-id="79252-116">Port sharing</span></span>
- <span data-ttu-id="79252-117">HTTPS com SNI</span><span class="sxs-lookup"><span data-stu-id="79252-117">HTTPS with SNI</span></span>
- <span data-ttu-id="79252-118">HTTP/2 por TLS (Windows 10)</span><span class="sxs-lookup"><span data-stu-id="79252-118">HTTP/2 over TLS (Windows 10)</span></span>
- <span data-ttu-id="79252-119">Transmissão de arquivo direto</span><span class="sxs-lookup"><span data-stu-id="79252-119">Direct file transmission</span></span>
- <span data-ttu-id="79252-120">O cache de resposta</span><span class="sxs-lookup"><span data-stu-id="79252-120">Response caching</span></span>
- <span data-ttu-id="79252-121">WebSocket (Windows 8)</span><span class="sxs-lookup"><span data-stu-id="79252-121">WebSockets (Windows 8)</span></span>

<span data-ttu-id="79252-122">Versões com suporte do Windows:</span><span class="sxs-lookup"><span data-stu-id="79252-122">Supported Windows versions:</span></span>

- <span data-ttu-id="79252-123">Windows 7 e Windows Server 2008 R2 e posterior</span><span class="sxs-lookup"><span data-stu-id="79252-123">Windows 7 and Windows Server 2008 R2 and later</span></span>

<span data-ttu-id="79252-124">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="79252-124">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-weblistener"></a><span data-ttu-id="79252-125">Quando usar WebListener</span><span class="sxs-lookup"><span data-stu-id="79252-125">When to use WebListener</span></span>

<span data-ttu-id="79252-126">WebListener é útil para implantações em que você precisa expor o servidor diretamente à Internet sem usar o IIS.</span><span class="sxs-lookup"><span data-stu-id="79252-126">WebListener is useful for deployments where you need to expose the server directly to the Internet without using IIS.</span></span>

![O WebListener se comunica diretamente com a Internet](weblistener/_static/weblistener-to-internet.png)

<span data-ttu-id="79252-128">Como ele se baseia no HTTP. sys, WebListener não requer um servidor de proxy reverso para proteção contra ataques.</span><span class="sxs-lookup"><span data-stu-id="79252-128">Because it's built on Http.Sys, WebListener doesn't require a reverse proxy server for protection against attacks.</span></span> <span data-ttu-id="79252-129">Http. sys é uma tecnologia desenvolvida que protege contra vários tipos de ataques e fornece a robustez, segurança e escalabilidade de um servidor web completo.</span><span class="sxs-lookup"><span data-stu-id="79252-129">Http.Sys is mature technology that protects against many kinds of attacks and provides the robustness, security, and scalability of a full-featured web server.</span></span> <span data-ttu-id="79252-130">O próprio IIS é executado como um ouvinte HTTP sobre HTTP. sys.</span><span class="sxs-lookup"><span data-stu-id="79252-130">IIS itself runs as an HTTP listener on top of Http.Sys.</span></span> 

<span data-ttu-id="79252-131">WebListener também é uma boa escolha para implantações internas quando você precisa de um dos recursos que você não pode obter usando Kestrel oferece.</span><span class="sxs-lookup"><span data-stu-id="79252-131">WebListener is also a good choice for internal deployments when you need one of the features it offers that you can't get by using Kestrel.</span></span>

![O WebListener se comunica diretamente com a rede interna](weblistener/_static/weblistener-to-internal.png)

## <a name="how-to-use-weblistener"></a><span data-ttu-id="79252-133">Como usar WebListener</span><span class="sxs-lookup"><span data-stu-id="79252-133">How to use WebListener</span></span>

<span data-ttu-id="79252-134">Aqui está uma visão geral das tarefas de configuração para o sistema operacional do host e seu aplicativo ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="79252-134">Here's an overview of setup tasks for the host OS and your ASP.NET Core application.</span></span>

### <a name="configure-windows-server"></a><span data-ttu-id="79252-135">Configurar o Windows Server</span><span class="sxs-lookup"><span data-stu-id="79252-135">Configure Windows Server</span></span>

* <span data-ttu-id="79252-136">Instale a versão do .NET que seu aplicativo requer, como [.NET Core](https://download.microsoft.com/download/0/A/3/0A372822-205D-4A86-BFA7-084D2CBE9EDF/DotNetCore.1.0.1-SDK.1.0.0.Preview2-003133-x64.exe) ou .NET Framework 4.5.1.</span><span class="sxs-lookup"><span data-stu-id="79252-136">Install the version of .NET that your application requires, such as [.NET Core](https://download.microsoft.com/download/0/A/3/0A372822-205D-4A86-BFA7-084D2CBE9EDF/DotNetCore.1.0.1-SDK.1.0.0.Preview2-003133-x64.exe) or .NET Framework 4.5.1.</span></span>

* <span data-ttu-id="79252-137">Pré-registrar prefixos de URL para vincular a WebListener e configurar certificados SSL</span><span class="sxs-lookup"><span data-stu-id="79252-137">Preregister URL prefixes to bind to WebListener, and set up SSL certificates</span></span>

   <span data-ttu-id="79252-138">Se você não pré-registrar prefixos de URL no Windows, você precisa executar seu aplicativo com privilégios de administrador.</span><span class="sxs-lookup"><span data-stu-id="79252-138">If you don't preregister URL prefixes in Windows, you have to run your application with administrator privileges.</span></span> <span data-ttu-id="79252-139">A única exceção é se você associar ao localhost usando HTTP (não HTTPS) com um número de porta maior que 1024; Nesse caso privilégios de administrador não são necessários.</span><span class="sxs-lookup"><span data-stu-id="79252-139">The only exception is if you bind to localhost using HTTP (not HTTPS) with a port number greater than 1024; in that case administrator privileges aren't required.</span></span>

   <span data-ttu-id="79252-140">Para obter detalhes, consulte [como pré-registrar prefixos e configurar o SSL](#preregister-url-prefixes-and-configure-ssl) posteriormente neste artigo.</span><span class="sxs-lookup"><span data-stu-id="79252-140">For details, see [How to preregister prefixes and configure SSL](#preregister-url-prefixes-and-configure-ssl) later in this article.</span></span>

* <span data-ttu-id="79252-141">Abrir portas de firewall para permitir o tráfego alcançar WebListener.</span><span class="sxs-lookup"><span data-stu-id="79252-141">Open firewall ports to allow traffic to reach WebListener.</span></span>

   <span data-ttu-id="79252-142">Você pode usar netsh.exe ou [cmdlets do PowerShell](https://technet.microsoft.com/library/jj554906).</span><span class="sxs-lookup"><span data-stu-id="79252-142">You can use netsh.exe or [PowerShell cmdlets](https://technet.microsoft.com/library/jj554906).</span></span>

<span data-ttu-id="79252-143">Também há [configurações de registro Http.Sys](https://support.microsoft.com/kb/820129).</span><span class="sxs-lookup"><span data-stu-id="79252-143">There are also [Http.Sys registry settings](https://support.microsoft.com/kb/820129).</span></span>

### <a name="configure-your-aspnet-core-application"></a><span data-ttu-id="79252-144">Configurar seu aplicativo ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="79252-144">Configure your ASP.NET Core application</span></span>

* <span data-ttu-id="79252-145">Instale o pacote NuGet [Microsoft.AspNetCore.Server.WebListener](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.WebListener/).</span><span class="sxs-lookup"><span data-stu-id="79252-145">Install the NuGet package [Microsoft.AspNetCore.Server.WebListener](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.WebListener/).</span></span> <span data-ttu-id="79252-146">Isso também instala [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) como uma dependência.</span><span class="sxs-lookup"><span data-stu-id="79252-146">This also installs [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) as a dependency.</span></span>

* <span data-ttu-id="79252-147">Chamar o `UseWebListener` método de extensão no [WebHostBuilder](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilder) no seu `Main` método, especificar qualquer WebListener [opções](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.AspNetCore.Server.WebListener/WebListenerOptions.cs) e [configurações](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.Net.Http.Server/WebListenerSettings.cs) que você precisa , conforme mostrado no exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="79252-147">Call the `UseWebListener` extension method on [WebHostBuilder](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilder) in your `Main` method, specifying any WebListener [options](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.AspNetCore.Server.WebListener/WebListenerOptions.cs) and [settings](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.Net.Http.Server/WebListenerSettings.cs) that you need, as shown in the following example:</span></span>

  [!code-csharp[](weblistener/sample/Program.cs?name=snippet_Main&highlight=13-17)]

* <span data-ttu-id="79252-148">Configurar URLs e portas para escutar em</span><span class="sxs-lookup"><span data-stu-id="79252-148">Configure URLs and ports to listen on</span></span> 

  <span data-ttu-id="79252-149">Por padrão o ASP.NET Core associa a `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="79252-149">By default ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="79252-150">Para configurar portas e prefixos de URL, você pode usar o `UseURLs` método de extensão, o `urls` argumento de linha de comando ou o sistema de configuração do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="79252-150">To configure URL prefixes and ports, you can use the `UseURLs` extension method, the `urls` command-line argument or the ASP.NET Core configuration system.</span></span> <span data-ttu-id="79252-151">Para obter mais informações, consulte [Hospedagem](../../fundamentals/hosting.md).</span><span class="sxs-lookup"><span data-stu-id="79252-151">For more information, see [Hosting](../../fundamentals/hosting.md).</span></span>

  <span data-ttu-id="79252-152">Web usa ouvinte a [formatos de cadeia de caracteres de prefixo de HTTP. sys](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span><span class="sxs-lookup"><span data-stu-id="79252-152">Web Listener uses the [Http.Sys prefix string formats](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span></span> <span data-ttu-id="79252-153">Não há nenhum requisitos de formato de cadeia de caracteres de prefixo que são específicos para WebListener.</span><span class="sxs-lookup"><span data-stu-id="79252-153">There are no prefix string format requirements that are specific to WebListener.</span></span>

  > [!NOTE]
  > <span data-ttu-id="79252-154">Certifique-se de que você especificar as cadeias de caracteres de prefixo mesmo em `UseUrls` que pré-registrar no servidor.</span><span class="sxs-lookup"><span data-stu-id="79252-154">Make sure that you specify the same prefix strings in `UseUrls` that you preregister on the server.</span></span> 

* <span data-ttu-id="79252-155">Verifique se que seu aplicativo não está configurado para executar o IIS ou IIS Express.</span><span class="sxs-lookup"><span data-stu-id="79252-155">Make sure your application is not configured to run IIS or IIS Express.</span></span>

  <span data-ttu-id="79252-156">No Visual Studio, o perfil de inicialização padrão é para o IIS Express.</span><span class="sxs-lookup"><span data-stu-id="79252-156">In Visual Studio, the default launch profile is for IIS Express.</span></span>  <span data-ttu-id="79252-157">Para executar o projeto como um aplicativo de console você precisa alterar manualmente o perfil selecionado, conforme mostrado na captura de tela a seguir.</span><span class="sxs-lookup"><span data-stu-id="79252-157">To run the project as a console application you have to manually change the selected profile, as shown in the following screen shot.</span></span>

  ![Selecione o perfil de aplicativo de console](weblistener/_static/vs-choose-profile.png)

## <a name="how-to-use-weblistener-outside-of-aspnet-core"></a><span data-ttu-id="79252-159">Como usar WebListener fora do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="79252-159">How to use WebListener outside of ASP.NET Core</span></span>

* <span data-ttu-id="79252-160">Instalar o [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) pacote NuGet.</span><span class="sxs-lookup"><span data-stu-id="79252-160">Install the [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet package.</span></span>

* <span data-ttu-id="79252-161">[Pré-registrar prefixos de URL para vincular a WebListener e configurar certificados SSL](#preregister-url-prefixes-and-configure-ssl) como você faria para uso em ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="79252-161">[Preregister URL prefixes to bind to WebListener, and set up SSL certificates](#preregister-url-prefixes-and-configure-ssl) as you would for use in ASP.NET Core.</span></span>

<span data-ttu-id="79252-162">Também há [configurações de registro Http.Sys](https://support.microsoft.com/kb/820129).</span><span class="sxs-lookup"><span data-stu-id="79252-162">There are also [Http.Sys registry settings](https://support.microsoft.com/kb/820129).</span></span>


<span data-ttu-id="79252-163">Aqui está um exemplo de código que demonstra o uso de WebListener fora do ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="79252-163">Here's a code sample that demonstrates WebListener use outside of ASP.NET Core:</span></span>

```csharp
var settings = new WebListenerSettings();
settings.UrlPrefixes.Add("http://localhost:8080");

using (WebListener listener = new WebListener(settings))
{
    listener.Start();

    while (true)
    {
        var context = await listener.AcceptAsync();
        byte[] bytes = Encoding.ASCII.GetBytes("Hello World: " + DateTime.Now);
        context.Response.ContentLength = bytes.Length;
        context.Response.ContentType = "text/plain";

        await context.Response.Body.WriteAsync(bytes, 0, bytes.Length);
        context.Dispose();
    }
}
```

## <a name="preregister-url-prefixes-and-configure-ssl"></a><span data-ttu-id="79252-164">Pré-registrar prefixos de URL e configurar o SSL</span><span class="sxs-lookup"><span data-stu-id="79252-164">Preregister URL prefixes and configure SSL</span></span>

<span data-ttu-id="79252-165">IIS e WebListener contam com o driver de modo de kernel HTTP. sys subjacente para escutar solicitações e processamento inicial.</span><span class="sxs-lookup"><span data-stu-id="79252-165">Both IIS and WebListener rely on the underlying Http.Sys kernel mode driver to listen for requests and do initial processing.</span></span> <span data-ttu-id="79252-166">No IIS, a interface do usuário de gerenciamento fornece uma maneira relativamente fácil de configurar tudo.</span><span class="sxs-lookup"><span data-stu-id="79252-166">In IIS, the management UI gives you a relatively easy way to configure everything.</span></span> <span data-ttu-id="79252-167">No entanto, se você estiver usando WebListener, você precisa configurar o HTTP. sys por conta própria.</span><span class="sxs-lookup"><span data-stu-id="79252-167">However, if you're using WebListener you need to configure Http.Sys yourself.</span></span> <span data-ttu-id="79252-168">A ferramenta interna para fazer isso é netsh.exe.</span><span class="sxs-lookup"><span data-stu-id="79252-168">The built-in tool for doing that is netsh.exe.</span></span> 

<span data-ttu-id="79252-169">As tarefas mais comuns que você precisa usar netsh.exe para são reservar prefixos de URL e a atribuição de certificados SSL.</span><span class="sxs-lookup"><span data-stu-id="79252-169">The most common tasks you need to use netsh.exe for are reserving URL prefixes and assigning SSL certificates.</span></span>

<span data-ttu-id="79252-170">NetSh.exe não é uma ferramenta fácil de usar para iniciantes.</span><span class="sxs-lookup"><span data-stu-id="79252-170">NetSh.exe is not an easy tool to use for beginners.</span></span> <span data-ttu-id="79252-171">O exemplo a seguir mostra o mínimo necessário para reservar os prefixos de URL para as portas 80 e 443:</span><span class="sxs-lookup"><span data-stu-id="79252-171">The following example shows the bare minimum needed to reserve URL prefixes for ports 80 and 443:</span></span>

```console
netsh http add urlacl url=http://+:80/ user=Users
netsh http add urlacl url=https://+:443/ user=Users
```

<span data-ttu-id="79252-172">O exemplo a seguir mostra como atribuir um certificado SSL:</span><span class="sxs-lookup"><span data-stu-id="79252-172">The following example shows how to assign an SSL certificate:</span></span>

```console
netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid={00000000-0000-0000-0000-000000000000}".
```

<span data-ttu-id="79252-173">Aqui está a documentação oficial:</span><span class="sxs-lookup"><span data-stu-id="79252-173">Here is the official reference documentation:</span></span>

* [<span data-ttu-id="79252-174">Comandos Netsh para Hypertext Transfer protocolo (HTTP)</span><span class="sxs-lookup"><span data-stu-id="79252-174">Netsh Commands for Hypertext Transfer Protocol (HTTP)</span></span>](https://technet.microsoft.com/library/cc725882.aspx)
* [<span data-ttu-id="79252-175">Cadeias de caracteres de UrlPrefix</span><span class="sxs-lookup"><span data-stu-id="79252-175">UrlPrefix Strings</span></span>](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

<span data-ttu-id="79252-176">Os recursos a seguir fornecem instruções detalhadas para vários cenários.</span><span class="sxs-lookup"><span data-stu-id="79252-176">The following resources provide detailed instructions for several scenarios.</span></span> <span data-ttu-id="79252-177">Os artigos que fazem referência a `HttpListener` se aplicam igualmente ao `WebListener`, pois ambas se baseiam em Http.Sys.</span><span class="sxs-lookup"><span data-stu-id="79252-177">Articles that refer to `HttpListener` apply equally to `WebListener`, as both are based on Http.Sys.</span></span>

* [<span data-ttu-id="79252-178">Como: configurar uma porta com um certificado SSL</span><span class="sxs-lookup"><span data-stu-id="79252-178">How to: Configure a Port with an SSL Certificate</span></span>](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-configure-a-port-with-an-ssl-certificate)
* <span data-ttu-id="79252-179">[A comunicação HTTPS - HttpListener com base em certificação de cliente e hospedagem](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) isso é um blog de terceiros e é bastante antigo, mas ainda tem informações úteis.</span><span class="sxs-lookup"><span data-stu-id="79252-179">[HTTPS Communication - HttpListener based Hosting and Client Certification](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) This is a third-party blog and is fairly old but still has useful information.</span></span>
* <span data-ttu-id="79252-180">[Como: Passo a passo usando HttpListener ou o servidor Http não gerenciado código (C++) como um servidor simples SSL](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) muito trata um blog mais antigo com informações úteis.</span><span class="sxs-lookup"><span data-stu-id="79252-180">[How To: Walkthrough Using HttpListener or Http Server unmanaged code (C++) as an SSL Simple Server](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) This too is an older blog with useful information.</span></span>
* [<span data-ttu-id="79252-181">Como configurar um WebListener de núcleo do .NET com SSL?</span><span class="sxs-lookup"><span data-stu-id="79252-181">How Do I Set Up A .NET Core WebListener With SSL?</span></span>](https://blogs.msdn.microsoft.com/timomta/2016/11/04/how-do-i-set-up-a-net-core-weblistener-with-ssl/)

<span data-ttu-id="79252-182">Aqui estão algumas ferramentas de terceiros que podem ser mais fácil de usar que a linha de comando netsh.exe.</span><span class="sxs-lookup"><span data-stu-id="79252-182">Here are some third-party tools that can be easier to use than the netsh.exe command line.</span></span> <span data-ttu-id="79252-183">Eles não são fornecidos pelo ou aprovados pela Microsoft.</span><span class="sxs-lookup"><span data-stu-id="79252-183">These are not provided by or endorsed by Microsoft.</span></span> <span data-ttu-id="79252-184">As ferramentas de executar como administrador por padrão, como netsh.exe em si requer privilégios de administrador.</span><span class="sxs-lookup"><span data-stu-id="79252-184">The tools run as administrator by default, since netsh.exe itself requires administrator privileges.</span></span>

* <span data-ttu-id="79252-185">[Gerenciador de HTTP. sys](http://httpsysmanager.codeplex.com/) fornece a interface de usuário para a listagem e configurar certificados SSL e opções, as reservas de prefixo e listas de certificados confiáveis.</span><span class="sxs-lookup"><span data-stu-id="79252-185">[http.sys Manager](http://httpsysmanager.codeplex.com/) provides UI for listing and configuring SSL certificates and options, prefix reservations, and certificate trust lists.</span></span> 
* <span data-ttu-id="79252-186">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) permite que você lista ou configurar certificados SSL e prefixos de URL.</span><span class="sxs-lookup"><span data-stu-id="79252-186">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) lets you list or configure SSL certificates and URL prefixes.</span></span> <span data-ttu-id="79252-187">A interface do usuário é mais refinada do que o HTTP. sys Gerenciador e expõe algumas opções adicionais de configuração, mas caso contrário, ele fornece uma funcionalidade semelhante.</span><span class="sxs-lookup"><span data-stu-id="79252-187">The UI is more refined than http.sys Manager and exposes a few more configuration options, but otherwise it provides similar functionality.</span></span> <span data-ttu-id="79252-188">Não é possível criar uma nova lista de certificados confiáveis (CTL), mas pode atribuir os existentes.</span><span class="sxs-lookup"><span data-stu-id="79252-188">It cannot create a new certificate trust list (CTL), but can assign existing ones.</span></span>

<span data-ttu-id="79252-189">Para gerar certificados SSL autoassinados, a Microsoft fornece ferramentas de linha de comando: [MakeCert.exe](https://msdn.microsoft.com/library/windows/desktop/aa386968) e o cmdlet do PowerShell [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pki/new-selfsignedcertificate).</span><span class="sxs-lookup"><span data-stu-id="79252-189">For generating self-signed SSL certificates, Microsoft provides command-line tools: [MakeCert.exe](https://msdn.microsoft.com/library/windows/desktop/aa386968) and the PowerShell cmdlet [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pki/new-selfsignedcertificate).</span></span> <span data-ttu-id="79252-190">Também existem ferramentas de interface de usuário de terceiros que facilitam para gerar certificados SSL autoassinados:</span><span class="sxs-lookup"><span data-stu-id="79252-190">There are also third-party UI tools that make it easier for you to generate self-signed SSL certificates:</span></span>

* [<span data-ttu-id="79252-191">SelfCert</span><span class="sxs-lookup"><span data-stu-id="79252-191">SelfCert</span></span>](https://www.pluralsight.com/blog/software-development/selfcert-create-a-self-signed-certificate-interactively-gui-or-programmatically-in-net)
* [<span data-ttu-id="79252-192">Interface de usuário Makecert</span><span class="sxs-lookup"><span data-stu-id="79252-192">Makecert UI</span></span>](http://makecertui.codeplex.com/)

## <a name="next-steps"></a><span data-ttu-id="79252-193">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="79252-193">Next steps</span></span>

<span data-ttu-id="79252-194">Para obter mais informações, consulte os seguintes recursos:</span><span class="sxs-lookup"><span data-stu-id="79252-194">For more information, see the following resources:</span></span>

* [<span data-ttu-id="79252-195">Aplicativo de exemplo para este artigo</span><span class="sxs-lookup"><span data-stu-id="79252-195">Sample app for this article</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample)
* [<span data-ttu-id="79252-196">Código-fonte WebListener</span><span class="sxs-lookup"><span data-stu-id="79252-196">WebListener source code</span></span>](https://github.com/aspnet/HttpSysServer/)
* [<span data-ttu-id="79252-197">Hospedagem</span><span class="sxs-lookup"><span data-stu-id="79252-197">Hosting</span></span>](../hosting.md)

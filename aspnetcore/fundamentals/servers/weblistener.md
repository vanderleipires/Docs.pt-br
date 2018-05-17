---
title: Implementação do servidor Web WebListener no ASP.NET Core
author: rick-anderson
description: Saiba mais sobre o WebListener, um servidor Web para o ASP.NET Core no Windows que pode ser usado para a conexão direta com a Internet sem o IIS.
manager: wpickett
ms.author: riande
ms.date: 03/13/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/weblistener
ms.openlocfilehash: d40243454632550147a7d42ab26a8f1d2d100db2
ms.sourcegitcommit: a19261eb82b948af6e4a1664fcfb8dabb16150e3
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/14/2018
---
# <a name="weblistener-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="0d6ae-103">Implementação do servidor Web WebListener no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0d6ae-103">WebListener web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="0d6ae-104">Por [Tom Dykstra](https://github.com/tdykstra) e [Chris Ross](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="0d6ae-104">By [Tom Dykstra](https://github.com/tdykstra) and [Chris Ross](https://github.com/Tratcher)</span></span>

> [!NOTE]
> <span data-ttu-id="0d6ae-105">Este tópico se aplica somente ao ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="0d6ae-105">This topic applies only to ASP.NET Core 1.x.</span></span> <span data-ttu-id="0d6ae-106">No ASP.NET Core 2.0, o WebListener é chamado [HTTP.sys](httpsys.md).</span><span class="sxs-lookup"><span data-stu-id="0d6ae-106">In ASP.NET Core 2.0, WebListener is named [HTTP.sys](httpsys.md).</span></span>

<span data-ttu-id="0d6ae-107">O WebListener é um [servidor Web para o ASP.NET Core](index.md) executado somente no Windows.</span><span class="sxs-lookup"><span data-stu-id="0d6ae-107">WebListener is a [web server for ASP.NET Core](index.md) that runs only on Windows.</span></span> <span data-ttu-id="0d6ae-108">Foi desenvolvido com base no [driver de modo kernel Http.Sys](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span><span class="sxs-lookup"><span data-stu-id="0d6ae-108">It's built on the [Http.Sys kernel mode driver](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span> <span data-ttu-id="0d6ae-109">O WebListener é uma alternativa ao [Kestrel](kestrel.md) que pode ser usada para conexão direta com a Internet sem depender do IIS como um servidor proxy reverso.</span><span class="sxs-lookup"><span data-stu-id="0d6ae-109">WebListener is an alternative to [Kestrel](kestrel.md) that can be used for direct connection to the Internet without relying on IIS as a reverse proxy server.</span></span> <span data-ttu-id="0d6ae-110">Na verdade, o **WebListener não pode ser usado com o IIS nem o IIS Express, pois não é compatível com o [Módulo do ASP.NET Core](aspnet-core-module.md).**</span><span class="sxs-lookup"><span data-stu-id="0d6ae-110">In fact, **WebListener can't be used with IIS or IIS Express, as it isn't compatible with the [ASP.NET Core Module](aspnet-core-module.md).**</span></span>

<span data-ttu-id="0d6ae-111">Embora o WebListener tenha sido desenvolvido para o ASP.NET Core, ele pode ser usado diretamente em qualquer aplicativo .NET Core ou .NET Framework por meio do pacote NuGet [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/).</span><span class="sxs-lookup"><span data-stu-id="0d6ae-111">Although WebListener was developed for ASP.NET Core, it can be used directly in any .NET Core or .NET Framework application via the [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet package.</span></span>

<span data-ttu-id="0d6ae-112">O WebListener dá suporte aos seguintes recursos:</span><span class="sxs-lookup"><span data-stu-id="0d6ae-112">WebListener supports the following features:</span></span>

- [<span data-ttu-id="0d6ae-113">Autenticação do Windows</span><span class="sxs-lookup"><span data-stu-id="0d6ae-113">Windows Authentication</span></span>](xref:security/authentication/windowsauth)
- <span data-ttu-id="0d6ae-114">Compartilhamento de porta</span><span class="sxs-lookup"><span data-stu-id="0d6ae-114">Port sharing</span></span>
- <span data-ttu-id="0d6ae-115">HTTPS com SNI</span><span class="sxs-lookup"><span data-stu-id="0d6ae-115">HTTPS with SNI</span></span>
- <span data-ttu-id="0d6ae-116">HTTP/2 por TLS (Windows 10)</span><span class="sxs-lookup"><span data-stu-id="0d6ae-116">HTTP/2 over TLS (Windows 10)</span></span>
- <span data-ttu-id="0d6ae-117">Transmissão direta de arquivo</span><span class="sxs-lookup"><span data-stu-id="0d6ae-117">Direct file transmission</span></span>
- <span data-ttu-id="0d6ae-118">Cache de resposta</span><span class="sxs-lookup"><span data-stu-id="0d6ae-118">Response caching</span></span>
- <span data-ttu-id="0d6ae-119">WebSockets (Windows 8)</span><span class="sxs-lookup"><span data-stu-id="0d6ae-119">WebSockets (Windows 8)</span></span>

<span data-ttu-id="0d6ae-120">Versões do Windows compatíveis:</span><span class="sxs-lookup"><span data-stu-id="0d6ae-120">Supported Windows versions:</span></span>

- <span data-ttu-id="0d6ae-121">Windows 7 e Windows Server 2008 R2 e posterior</span><span class="sxs-lookup"><span data-stu-id="0d6ae-121">Windows 7 and Windows Server 2008 R2 and later</span></span>

<span data-ttu-id="0d6ae-122">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="0d6ae-122">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-weblistener"></a><span data-ttu-id="0d6ae-123">Quando usar o WebListener</span><span class="sxs-lookup"><span data-stu-id="0d6ae-123">When to use WebListener</span></span>

<span data-ttu-id="0d6ae-124">O WebListener é útil para implantações em que você precisa expor o servidor diretamente à Internet sem usar o IIS.</span><span class="sxs-lookup"><span data-stu-id="0d6ae-124">WebListener is useful for deployments where you need to expose the server directly to the Internet without using IIS.</span></span>

![O WebListener se comunica diretamente com a Internet](weblistener/_static/weblistener-to-internet.png)

<span data-ttu-id="0d6ae-126">Como se baseia no Http.Sys, o WebListener não exige um servidor proxy reverso para proteção contra ataques.</span><span class="sxs-lookup"><span data-stu-id="0d6ae-126">Because it's built on Http.Sys, WebListener doesn't require a reverse proxy server for protection against attacks.</span></span> <span data-ttu-id="0d6ae-127">O Http.Sys é uma tecnologia madura que protege contra muitos tipos de ataques e fornece a robustez, segurança e escalabilidade de um servidor Web completo.</span><span class="sxs-lookup"><span data-stu-id="0d6ae-127">Http.Sys is mature technology that protects against many kinds of attacks and provides the robustness, security, and scalability of a full-featured web server.</span></span> <span data-ttu-id="0d6ae-128">O próprio IIS é executado como um ouvinte HTTP no Http.Sys.</span><span class="sxs-lookup"><span data-stu-id="0d6ae-128">IIS itself runs as an HTTP listener on top of Http.Sys.</span></span> 

<span data-ttu-id="0d6ae-129">O WebListener também é uma boa escolha para implantações internas, quando você precisa de um dos recursos oferecidos por ele que não é possível obter usando o Kestrel.</span><span class="sxs-lookup"><span data-stu-id="0d6ae-129">WebListener is also a good choice for internal deployments when you need one of the features it offers that you can't get by using Kestrel.</span></span>

![O WebListener se comunica diretamente com a rede interna](weblistener/_static/weblistener-to-internal.png)

## <a name="how-to-use-weblistener"></a><span data-ttu-id="0d6ae-131">Como usar o WebListener</span><span class="sxs-lookup"><span data-stu-id="0d6ae-131">How to use WebListener</span></span>

<span data-ttu-id="0d6ae-132">Veja a seguir uma visão geral das tarefas de configuração para o sistema operacional do host e o aplicativo ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0d6ae-132">Here's an overview of setup tasks for the host OS and your ASP.NET Core application.</span></span>

### <a name="configure-windows-server"></a><span data-ttu-id="0d6ae-133">Configurar o Windows Server</span><span class="sxs-lookup"><span data-stu-id="0d6ae-133">Configure Windows Server</span></span>

* <span data-ttu-id="0d6ae-134">Instale a versão do .NET exigida para o aplicativo, como o [.NET Core](https://download.microsoft.com/download/0/A/3/0A372822-205D-4A86-BFA7-084D2CBE9EDF/DotNetCore.1.0.1-SDK.1.0.0.Preview2-003133-x64.exe) ou o .NET Framework 4.5.1.</span><span class="sxs-lookup"><span data-stu-id="0d6ae-134">Install the version of .NET that your application requires, such as [.NET Core](https://download.microsoft.com/download/0/A/3/0A372822-205D-4A86-BFA7-084D2CBE9EDF/DotNetCore.1.0.1-SDK.1.0.0.Preview2-003133-x64.exe) or .NET Framework 4.5.1.</span></span>

* <span data-ttu-id="0d6ae-135">Pré-registrar prefixos de URL para associá-los ao WebListener e configurar certificados SSL</span><span class="sxs-lookup"><span data-stu-id="0d6ae-135">Preregister URL prefixes to bind to WebListener, and set up SSL certificates</span></span>

   <span data-ttu-id="0d6ae-136">Se você não pré-registrar prefixos de URL no Windows, precisará executar o aplicativo com privilégios de administrador.</span><span class="sxs-lookup"><span data-stu-id="0d6ae-136">If you don't preregister URL prefixes in Windows, you have to run your application with administrator privileges.</span></span> <span data-ttu-id="0d6ae-137">A única exceção é se você associar ao localhost usando HTTP (não HTTPS) com um número de porta maior que 1024; nesse caso, os privilégios de administrador não são necessários.</span><span class="sxs-lookup"><span data-stu-id="0d6ae-137">The only exception is if you bind to localhost using HTTP (not HTTPS) with a port number greater than 1024; in that case administrator privileges aren't required.</span></span>

   <span data-ttu-id="0d6ae-138">Para obter detalhes, consulte [Como pré-registrar prefixos e configurar o SSL](#preregister-url-prefixes-and-configure-ssl) mais adiante neste artigo.</span><span class="sxs-lookup"><span data-stu-id="0d6ae-138">For details, see [How to preregister prefixes and configure SSL](#preregister-url-prefixes-and-configure-ssl) later in this article.</span></span>

* <span data-ttu-id="0d6ae-139">Abra as portas do firewall para permitir que o tráfego chegue ao WebListener.</span><span class="sxs-lookup"><span data-stu-id="0d6ae-139">Open firewall ports to allow traffic to reach WebListener.</span></span>

   <span data-ttu-id="0d6ae-140">Use o netsh.exe ou [cmdlets do PowerShell](https://technet.microsoft.com/library/jj554906).</span><span class="sxs-lookup"><span data-stu-id="0d6ae-140">You can use netsh.exe or [PowerShell cmdlets](https://technet.microsoft.com/library/jj554906).</span></span>

<span data-ttu-id="0d6ae-141">Também há [configurações de registro do Http.Sys](https://support.microsoft.com/kb/820129).</span><span class="sxs-lookup"><span data-stu-id="0d6ae-141">There are also [Http.Sys registry settings](https://support.microsoft.com/kb/820129).</span></span>

### <a name="configure-your-aspnet-core-application"></a><span data-ttu-id="0d6ae-142">Configurar o aplicativo ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0d6ae-142">Configure your ASP.NET Core application</span></span>

* <span data-ttu-id="0d6ae-143">Instale o pacote NuGet [Microsoft.AspNetCore.Server.WebListener](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.WebListener/).</span><span class="sxs-lookup"><span data-stu-id="0d6ae-143">Install the NuGet package [Microsoft.AspNetCore.Server.WebListener](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.WebListener/).</span></span> <span data-ttu-id="0d6ae-144">Isso também instala [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) como uma dependência.</span><span class="sxs-lookup"><span data-stu-id="0d6ae-144">This also installs [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) as a dependency.</span></span>

* <span data-ttu-id="0d6ae-145">Chame o método de extensão `UseWebListener` no [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) no método `Main`, especificando as [opções](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.AspNetCore.Server.WebListener/WebListenerOptions.cs) e [configurações](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.Net.Http.Server/WebListenerSettings.cs) do WebListener necessárias, conforme mostrado no seguinte exemplo:</span><span class="sxs-lookup"><span data-stu-id="0d6ae-145">Call the `UseWebListener` extension method on [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) in your `Main` method, specifying any WebListener [options](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.AspNetCore.Server.WebListener/WebListenerOptions.cs) and [settings](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.Net.Http.Server/WebListenerSettings.cs) that you need, as shown in the following example:</span></span>

  [!code-csharp[](weblistener/sample/Program.cs?name=snippet_Main&highlight=13-17)]

* <span data-ttu-id="0d6ae-146">Configurar URLs e portas nas quais escutar</span><span class="sxs-lookup"><span data-stu-id="0d6ae-146">Configure URLs and ports to listen on</span></span> 

  <span data-ttu-id="0d6ae-147">Por padrão, o ASP.NET Core é associado a `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="0d6ae-147">By default ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="0d6ae-148">Para configurar portas e prefixos de URL, use o método de extensão `UseURLs`, o argumento de linha de comando `urls` ou o sistema de configuração do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0d6ae-148">To configure URL prefixes and ports, you can use the `UseURLs` extension method, the `urls` command-line argument or the ASP.NET Core configuration system.</span></span> <span data-ttu-id="0d6ae-149">Para obter mais informações, consulte [Hospedagem](../../fundamentals/hosting.md).</span><span class="sxs-lookup"><span data-stu-id="0d6ae-149">For more information, see [Hosting](../../fundamentals/hosting.md).</span></span>

  <span data-ttu-id="0d6ae-150">O Ouvinte Web usa os [formatos de cadeia de caracteres de prefixo do Http.Sys](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span><span class="sxs-lookup"><span data-stu-id="0d6ae-150">Web Listener uses the [Http.Sys prefix string formats](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span></span> <span data-ttu-id="0d6ae-151">Não há nenhum requisito de formato de cadeia de caracteres de prefixo específico ao WebListener.</span><span class="sxs-lookup"><span data-stu-id="0d6ae-151">There are no prefix string format requirements that are specific to WebListener.</span></span>

  > [!WARNING]
  > <span data-ttu-id="0d6ae-152">Associações de curinga de nível superior (`http://*:80/` e `http://+:80`) **não** devem ser usadas.</span><span class="sxs-lookup"><span data-stu-id="0d6ae-152">Top-level wildcard bindings (`http://*:80/` and `http://+:80`) should **not** be used.</span></span> <span data-ttu-id="0d6ae-153">Associações de curinga de nível superior podem abrir o aplicativo para vulnerabilidades de segurança.</span><span class="sxs-lookup"><span data-stu-id="0d6ae-153">Top-level wildcard bindings can open up your app to security vulnerabilities.</span></span> <span data-ttu-id="0d6ae-154">Isso se aplica a curingas fortes e fracos.</span><span class="sxs-lookup"><span data-stu-id="0d6ae-154">This applies to both strong and weak wildcards.</span></span> <span data-ttu-id="0d6ae-155">Use nomes de host explícitos em vez de curingas.</span><span class="sxs-lookup"><span data-stu-id="0d6ae-155">Use explicit host names rather than wildcards.</span></span> <span data-ttu-id="0d6ae-156">Associações de curinga de subdomínio (por exemplo, `*.mysub.com`) não têm esse risco de segurança se você controlar o domínio pai completo (em vez de `*.com`, o qual é vulnerável).</span><span class="sxs-lookup"><span data-stu-id="0d6ae-156">Subdomain wildcard binding (for example, `*.mysub.com`) doesn't have this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="0d6ae-157">Veja [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="0d6ae-157">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

  > [!NOTE]
  > <span data-ttu-id="0d6ae-158">Especifique as mesmas cadeias de caracteres de prefixo em `UseUrls` que você pré-registrou no servidor.</span><span class="sxs-lookup"><span data-stu-id="0d6ae-158">Make sure that you specify the same prefix strings in `UseUrls` that you preregister on the server.</span></span> 

* <span data-ttu-id="0d6ae-159">Verifique se o aplicativo não está configurado para executar o IIS ou o IIS Express.</span><span class="sxs-lookup"><span data-stu-id="0d6ae-159">Make sure your application isn't configured to run IIS or IIS Express.</span></span>

  <span data-ttu-id="0d6ae-160">No Visual Studio, o perfil de inicialização padrão destina-se ao IIS Express.</span><span class="sxs-lookup"><span data-stu-id="0d6ae-160">In Visual Studio, the default launch profile is for IIS Express.</span></span>  <span data-ttu-id="0d6ae-161">Para executar o projeto como um aplicativo de console, você precisa alterar manualmente o perfil selecionado, conforme mostrado na captura de tela a seguir.</span><span class="sxs-lookup"><span data-stu-id="0d6ae-161">To run the project as a console application you have to manually change the selected profile, as shown in the following screen shot.</span></span>

  ![Selecionar o perfil do aplicativo de console](weblistener/_static/vs-choose-profile.png)

## <a name="how-to-use-weblistener-outside-of-aspnet-core"></a><span data-ttu-id="0d6ae-163">Como usar o WebListener fora do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0d6ae-163">How to use WebListener outside of ASP.NET Core</span></span>

* <span data-ttu-id="0d6ae-164">Instale o pacote NuGet [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/).</span><span class="sxs-lookup"><span data-stu-id="0d6ae-164">Install the [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet package.</span></span>

* <span data-ttu-id="0d6ae-165">[Pré-registre prefixos de URL a serem associados ao WebListener e configure certificados SSL](#preregister-url-prefixes-and-configure-ssl) como você faria para uso no ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0d6ae-165">[Preregister URL prefixes to bind to WebListener, and set up SSL certificates](#preregister-url-prefixes-and-configure-ssl) as you would for use in ASP.NET Core.</span></span>

<span data-ttu-id="0d6ae-166">Também há [configurações de registro do Http.Sys](https://support.microsoft.com/kb/820129).</span><span class="sxs-lookup"><span data-stu-id="0d6ae-166">There are also [Http.Sys registry settings](https://support.microsoft.com/kb/820129).</span></span>


<span data-ttu-id="0d6ae-167">Este é um exemplo de código que demonstra o uso do WebListener fora do ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="0d6ae-167">Here's a code sample that demonstrates WebListener use outside of ASP.NET Core:</span></span>

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

## <a name="preregister-url-prefixes-and-configure-ssl"></a><span data-ttu-id="0d6ae-168">Pré-registrar prefixos de URL e configurar o SSL</span><span class="sxs-lookup"><span data-stu-id="0d6ae-168">Preregister URL prefixes and configure SSL</span></span>

<span data-ttu-id="0d6ae-169">O IIS e o WebListener dependem do driver de modo kernel Http.Sys subjacente para escutar solicitações e fazer o processamento inicial.</span><span class="sxs-lookup"><span data-stu-id="0d6ae-169">Both IIS and WebListener rely on the underlying Http.Sys kernel mode driver to listen for requests and do initial processing.</span></span> <span data-ttu-id="0d6ae-170">No IIS, a interface do usuário de gerenciamento fornece uma maneira relativamente fácil de configurar tudo.</span><span class="sxs-lookup"><span data-stu-id="0d6ae-170">In IIS, the management UI gives you a relatively easy way to configure everything.</span></span> <span data-ttu-id="0d6ae-171">No entanto, se estiver usando o WebListener, você precisará configurar o Http.Sys por conta própria.</span><span class="sxs-lookup"><span data-stu-id="0d6ae-171">However, if you're using WebListener you need to configure Http.Sys yourself.</span></span> <span data-ttu-id="0d6ae-172">A ferramenta interna para fazer isso é o netsh.exe.</span><span class="sxs-lookup"><span data-stu-id="0d6ae-172">The built-in tool for doing that's netsh.exe.</span></span> 

<span data-ttu-id="0d6ae-173">As tarefas mais comuns para as quais você precisa usar o netsh.exe são reservar prefixos de URL e atribuir certificados SSL.</span><span class="sxs-lookup"><span data-stu-id="0d6ae-173">The most common tasks you need to use netsh.exe for are reserving URL prefixes and assigning SSL certificates.</span></span>

<span data-ttu-id="0d6ae-174">O NetSh.exe não é uma ferramenta fácil de usar para iniciantes.</span><span class="sxs-lookup"><span data-stu-id="0d6ae-174">NetSh.exe isn't an easy tool to use for beginners.</span></span> <span data-ttu-id="0d6ae-175">O seguinte exemplo mostra o mínimo necessário para reservar prefixos de URL para as portas 80 e 443:</span><span class="sxs-lookup"><span data-stu-id="0d6ae-175">The following example shows the bare minimum needed to reserve URL prefixes for ports 80 and 443:</span></span>

```console
netsh http add urlacl url=http://+:80/ user=Users
netsh http add urlacl url=https://+:443/ user=Users
```

<span data-ttu-id="0d6ae-176">O seguinte exemplo mostra como atribuir um certificado SSL:</span><span class="sxs-lookup"><span data-stu-id="0d6ae-176">The following example shows how to assign an SSL certificate:</span></span>

```console
netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid="{00000000-0000-0000-0000-000000000000}".
```

<span data-ttu-id="0d6ae-177">Esta é a documentação de referência oficial:</span><span class="sxs-lookup"><span data-stu-id="0d6ae-177">Here is the official reference documentation:</span></span>

* [<span data-ttu-id="0d6ae-178">Comandos do Netsh para o protocolo HTTP</span><span class="sxs-lookup"><span data-stu-id="0d6ae-178">Netsh Commands for Hypertext Transfer Protocol (HTTP)</span></span>](https://technet.microsoft.com/library/cc725882.aspx)
* [<span data-ttu-id="0d6ae-179">Cadeias de caracteres de UrlPrefix</span><span class="sxs-lookup"><span data-stu-id="0d6ae-179">UrlPrefix Strings</span></span>](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

<span data-ttu-id="0d6ae-180">Os recursos a seguir fornecem instruções detalhadas para vários cenários.</span><span class="sxs-lookup"><span data-stu-id="0d6ae-180">The following resources provide detailed instructions for several scenarios.</span></span> <span data-ttu-id="0d6ae-181">Os artigos que se referem ao `HttpListener` se aplicam igualmente ao `WebListener`, pois ambos se baseiam no Http.Sys.</span><span class="sxs-lookup"><span data-stu-id="0d6ae-181">Articles that refer to `HttpListener` apply equally to `WebListener`, as both are based on Http.Sys.</span></span>

* [<span data-ttu-id="0d6ae-182">Como configurar uma porta com um certificado SSL</span><span class="sxs-lookup"><span data-stu-id="0d6ae-182">How to: Configure a Port with an SSL Certificate</span></span>](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-configure-a-port-with-an-ssl-certificate)
* <span data-ttu-id="0d6ae-183">[HTTPS Communication – HttpListener based Hosting and Client Certification](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) (Comunicação HTTPS – Certificação de cliente e hospedagem baseada no HttpListener) Esse é um blog de terceiros e é bem antigo, mas ainda traz informações úteis.</span><span class="sxs-lookup"><span data-stu-id="0d6ae-183">[HTTPS Communication - HttpListener based Hosting and Client Certification](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) This is a third-party blog and is fairly old but still has useful information.</span></span>
* <span data-ttu-id="0d6ae-184">[How To: Walkthrough Using HttpListener or Http Server unmanaged code (C++) as an SSL Simple Server](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) [Passo a passo: usando um código não gerenciado (C++) do HttpListener ou do Servidor HTTP como um Servidor Simples SSL] Esse também é um blog mais antigo com informações úteis.</span><span class="sxs-lookup"><span data-stu-id="0d6ae-184">[How To: Walkthrough Using HttpListener or Http Server unmanaged code (C++) as an SSL Simple Server](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) This too is an older blog with useful information.</span></span>
* [<span data-ttu-id="0d6ae-185">Como fazer para configurar um WebListener do .NET Core com SSL?</span><span class="sxs-lookup"><span data-stu-id="0d6ae-185">How Do I Set Up A .NET Core WebListener With SSL?</span></span>](https://blogs.msdn.microsoft.com/timomta/2016/11/04/how-do-i-set-up-a-net-core-weblistener-with-ssl/)

<span data-ttu-id="0d6ae-186">Veja a seguir algumas ferramentas de terceiros que podem ser mais fáceis de usar que a linha de comando do netsh.exe.</span><span class="sxs-lookup"><span data-stu-id="0d6ae-186">Here are some third-party tools that can be easier to use than the netsh.exe command line.</span></span> <span data-ttu-id="0d6ae-187">Elas não são fornecidas nem endossadas pela Microsoft.</span><span class="sxs-lookup"><span data-stu-id="0d6ae-187">These are not provided by or endorsed by Microsoft.</span></span> <span data-ttu-id="0d6ae-188">As ferramentas são executadas como administrador por padrão, pois o próprio netsh.exe exige privilégios de administrador.</span><span class="sxs-lookup"><span data-stu-id="0d6ae-188">The tools run as administrator by default, since netsh.exe itself requires administrator privileges.</span></span>

* <span data-ttu-id="0d6ae-189">O [Gerenciador do http.sys](http://httpsysmanager.codeplex.com/) fornece a interface do usuário para a listagem e configuração de certificados SSL e opções, reservas de prefixo e listas de certificados confiáveis.</span><span class="sxs-lookup"><span data-stu-id="0d6ae-189">[http.sys Manager](http://httpsysmanager.codeplex.com/) provides UI for listing and configuring SSL certificates and options, prefix reservations, and certificate trust lists.</span></span> 
* <span data-ttu-id="0d6ae-190">O [HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) permite listar ou configurar certificados SSL e prefixos de URL.</span><span class="sxs-lookup"><span data-stu-id="0d6ae-190">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) lets you list or configure SSL certificates and URL prefixes.</span></span> <span data-ttu-id="0d6ae-191">A interface do usuário é mais refinada comparada ao Gerenciador do http.sys e expõe algumas outras opções de configuração, mas de outro modo fornece uma funcionalidade semelhante.</span><span class="sxs-lookup"><span data-stu-id="0d6ae-191">The UI is more refined than http.sys Manager and exposes a few more configuration options, but otherwise it provides similar functionality.</span></span> <span data-ttu-id="0d6ae-192">Ela não pode criar uma nova CTL (lista de certificados confiáveis), mas pode atribuir as existentes.</span><span class="sxs-lookup"><span data-stu-id="0d6ae-192">It cannot create a new certificate trust list (CTL), but can assign existing ones.</span></span>

<span data-ttu-id="0d6ae-193">Para gerar certificados SSL autoassinados, a Microsoft fornece ferramentas de linha de comando: [MakeCert.exe](https://msdn.microsoft.com/library/windows/desktop/aa386968) e o cmdlet do PowerShell [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pki/new-selfsignedcertificate).</span><span class="sxs-lookup"><span data-stu-id="0d6ae-193">For generating self-signed SSL certificates, Microsoft provides command-line tools: [MakeCert.exe](https://msdn.microsoft.com/library/windows/desktop/aa386968) and the PowerShell cmdlet [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pki/new-selfsignedcertificate).</span></span> <span data-ttu-id="0d6ae-194">Também existem ferramentas de interface do usuário de terceiros que facilitam a geração de certificados SSL autoassinados:</span><span class="sxs-lookup"><span data-stu-id="0d6ae-194">There are also third-party UI tools that make it easier for you to generate self-signed SSL certificates:</span></span>

* [<span data-ttu-id="0d6ae-195">SelfCert</span><span class="sxs-lookup"><span data-stu-id="0d6ae-195">SelfCert</span></span>](https://www.pluralsight.com/blog/software-development/selfcert-create-a-self-signed-certificate-interactively-gui-or-programmatically-in-net)
* [<span data-ttu-id="0d6ae-196">Interface do usuário do Makecert</span><span class="sxs-lookup"><span data-stu-id="0d6ae-196">Makecert UI</span></span>](http://makecertui.codeplex.com/)

## <a name="next-steps"></a><span data-ttu-id="0d6ae-197">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="0d6ae-197">Next steps</span></span>

<span data-ttu-id="0d6ae-198">Para obter mais informações, consulte os seguintes recursos:</span><span class="sxs-lookup"><span data-stu-id="0d6ae-198">For more information, see the following resources:</span></span>

* [<span data-ttu-id="0d6ae-199">Aplicativo de exemplo para este artigo</span><span class="sxs-lookup"><span data-stu-id="0d6ae-199">Sample app for this article</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample)
* [<span data-ttu-id="0d6ae-200">Código-fonte do WebListener</span><span class="sxs-lookup"><span data-stu-id="0d6ae-200">WebListener source code</span></span>](https://github.com/aspnet/HttpSysServer/)
* [<span data-ttu-id="0d6ae-201">Hospedagem</span><span class="sxs-lookup"><span data-stu-id="0d6ae-201">Hosting</span></span>](../hosting.md)

---
title: Host ASP.NET Core no Linux com Nginx
author: rick-anderson
description: Descreve como configurar Nginx como um proxy reverso no Ubuntu 16.04 para encaminhar o tráfego HTTP para um aplicativo web do ASP.NET Core em execução no Kestrel.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 03/13/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/linux-nginx
ms.openlocfilehash: fe772203e5e3fceb7489e0a5866f60ea914b7329
ms.sourcegitcommit: 74be78285ea88772e7dad112f80146b6ed00e53e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/10/2018
---
# <a name="host-aspnet-core-on-linux-with-nginx"></a><span data-ttu-id="1b475-103">Host ASP.NET Core no Linux com Nginx</span><span class="sxs-lookup"><span data-stu-id="1b475-103">Host ASP.NET Core on Linux with Nginx</span></span>

<span data-ttu-id="1b475-104">Por [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="1b475-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="1b475-105">Este guia explica como configurar um ambiente do ASP.NET Core pronto para produção em um servidor do Ubuntu 16.04.</span><span class="sxs-lookup"><span data-stu-id="1b475-105">This guide explains setting up a production-ready ASP.NET Core environment on an Ubuntu 16.04 Server.</span></span>

> [!NOTE]
> <span data-ttu-id="1b475-106">Para Ubuntu 14.04 *supervisord* é recomendado como uma solução para monitorar o processo de Kestrel.</span><span class="sxs-lookup"><span data-stu-id="1b475-106">For Ubuntu 14.04, *supervisord* is recommended as a solution for monitoring the Kestrel process.</span></span> <span data-ttu-id="1b475-107">*systemd* não está disponível no Ubuntu 14.04.</span><span class="sxs-lookup"><span data-stu-id="1b475-107">*systemd* isn't available on Ubuntu 14.04.</span></span> <span data-ttu-id="1b475-108">[Consulte a versão anterior deste documento](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md).</span><span class="sxs-lookup"><span data-stu-id="1b475-108">[See previous version of this document](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md).</span></span>

<span data-ttu-id="1b475-109">Este guia:</span><span class="sxs-lookup"><span data-stu-id="1b475-109">This guide:</span></span>

* <span data-ttu-id="1b475-110">Coloca um aplicativo ASP.NET Core existente em um servidor proxy reverso.</span><span class="sxs-lookup"><span data-stu-id="1b475-110">Places an existing ASP.NET Core app behind a reverse proxy server.</span></span>
* <span data-ttu-id="1b475-111">Configura o servidor proxy inverso para encaminhar solicitações ao servidor da web Kestrel.</span><span class="sxs-lookup"><span data-stu-id="1b475-111">Sets up the reverse proxy server to forward requests to the Kestrel web server.</span></span>
* <span data-ttu-id="1b475-112">Garante que o aplicativo web é executado na inicialização como um daemon.</span><span class="sxs-lookup"><span data-stu-id="1b475-112">Ensures the web app runs on startup as a daemon.</span></span>
* <span data-ttu-id="1b475-113">Configura uma ferramenta de gerenciamento de processo para ajudar a reiniciar o aplicativo web.</span><span class="sxs-lookup"><span data-stu-id="1b475-113">Configures a process management tool to help restart the web app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1b475-114">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="1b475-114">Prerequisites</span></span>

1. <span data-ttu-id="1b475-115">Acesso a um Servidor Ubuntu 16.04 com uma conta de usuário padrão com privilégio sudo</span><span class="sxs-lookup"><span data-stu-id="1b475-115">Access to an Ubuntu 16.04 Server with a standard user account with sudo privilege</span></span>
1. <span data-ttu-id="1b475-116">Um aplicativo ASP.NET Core existente</span><span class="sxs-lookup"><span data-stu-id="1b475-116">An existing ASP.NET Core app</span></span>

## <a name="copy-over-the-app"></a><span data-ttu-id="1b475-117">Copie o aplicativo</span><span class="sxs-lookup"><span data-stu-id="1b475-117">Copy over the app</span></span>

<span data-ttu-id="1b475-118">Executar [dotnet publicar](/dotnet/core/tools/dotnet-publish) do ambiente de desenvolvimento para empacotar um aplicativo em um diretório independente que pode ser executados no servidor.</span><span class="sxs-lookup"><span data-stu-id="1b475-118">Run [dotnet publish](/dotnet/core/tools/dotnet-publish) from the dev environment to package an app into a self-contained directory that can run on the server.</span></span>

<span data-ttu-id="1b475-119">O aplicativo ASP.NET Core para o servidor usando qualquer ferramenta de cópia se integra ao fluxo de trabalho da organização (por exemplo, "SCP", "FTP").</span><span class="sxs-lookup"><span data-stu-id="1b475-119">Copy the ASP.NET Core app to the server using whatever tool integrates into the organization's workflow (for example, SCP, FTP).</span></span> <span data-ttu-id="1b475-120">Teste o aplicativo, por exemplo:</span><span class="sxs-lookup"><span data-stu-id="1b475-120">Test the app, for example:</span></span>

* <span data-ttu-id="1b475-121">Na linha de comando, execute `dotnet <app_assembly>.dll`.</span><span class="sxs-lookup"><span data-stu-id="1b475-121">From the command line, run `dotnet <app_assembly>.dll`.</span></span>
* <span data-ttu-id="1b475-122">Em um navegador, navegue até `http://<serveraddress>:<port>` para verificar se o aplicativo funciona no Linux.</span><span class="sxs-lookup"><span data-stu-id="1b475-122">In a browser, navigate to `http://<serveraddress>:<port>` to verify the app works on Linux.</span></span> 
 
## <a name="configure-a-reverse-proxy-server"></a><span data-ttu-id="1b475-123">Configurar um servidor proxy reverso</span><span class="sxs-lookup"><span data-stu-id="1b475-123">Configure a reverse proxy server</span></span>

<span data-ttu-id="1b475-124">Um proxy reverso é uma instalação comum para que serve a aplicativos web dinâmicos.</span><span class="sxs-lookup"><span data-stu-id="1b475-124">A reverse proxy is a common setup for serving dynamic web apps.</span></span> <span data-ttu-id="1b475-125">Um proxy reverso encerra a solicitação HTTP e as encaminha para o aplicativo ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1b475-125">A reverse proxy terminates the HTTP request and forwards it to the ASP.NET Core app.</span></span>

### <a name="why-use-a-reverse-proxy-server"></a><span data-ttu-id="1b475-126">Por que usar um servidor proxy reverso?</span><span class="sxs-lookup"><span data-stu-id="1b475-126">Why use a reverse proxy server?</span></span>

<span data-ttu-id="1b475-127">Kestrel é excelente para servir conteúdo dinâmico do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1b475-127">Kestrel is great for serving dynamic content from ASP.NET Core.</span></span> <span data-ttu-id="1b475-128">No entanto, os recursos de servidor web não são como muitos recursos como servidores, como o IIS, o Apache ou Nginx.</span><span class="sxs-lookup"><span data-stu-id="1b475-128">However, the web serving capabilities aren't as feature rich as servers such as IIS, Apache, or Nginx.</span></span> <span data-ttu-id="1b475-129">Um servidor proxy reverso pode descarregar o trabalho, como que serve o conteúdo estático, solicitações de cache, a compactação de solicitações e a terminação de SSL do servidor HTTP.</span><span class="sxs-lookup"><span data-stu-id="1b475-129">A reverse proxy server can offload work such as serving static content, caching requests, compressing requests, and SSL termination from the HTTP server.</span></span> <span data-ttu-id="1b475-130">Um servidor proxy reverso pode residir em um computador dedicado ou pode ser implantado junto com um servidor HTTP.</span><span class="sxs-lookup"><span data-stu-id="1b475-130">A reverse proxy server may reside on a dedicated machine or may be deployed alongside an HTTP server.</span></span>

<span data-ttu-id="1b475-131">Para os fins deste guia, uma única instância de Nginx é usada.</span><span class="sxs-lookup"><span data-stu-id="1b475-131">For the purposes of this guide, a single instance of Nginx is used.</span></span> <span data-ttu-id="1b475-132">Ela é executada no mesmo servidor, junto com o servidor HTTP.</span><span class="sxs-lookup"><span data-stu-id="1b475-132">It runs on the same server, alongside the HTTP server.</span></span> <span data-ttu-id="1b475-133">Com base nos requisitos, uma configuração diferente pode ser escolhida.</span><span class="sxs-lookup"><span data-stu-id="1b475-133">Based on requirements, a different setup may be chosen.</span></span>

<span data-ttu-id="1b475-134">Como as solicitações são encaminhadas pelo proxy reverso, use o Middleware de cabeçalhos encaminhados do [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) pacote.</span><span class="sxs-lookup"><span data-stu-id="1b475-134">Because requests are forwarded by reverse proxy, use the Forwarded Headers Middleware from the [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) package.</span></span> <span data-ttu-id="1b475-135">As atualizações de middleware de `Request.Scheme`, usando o `X-Forwarded-Proto` cabeçalho, de forma que URIs de redirecionamento e outras políticas de segurança funcionam corretamente.</span><span class="sxs-lookup"><span data-stu-id="1b475-135">The middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header, so that redirect URIs and other security policies work correctly.</span></span>

<span data-ttu-id="1b475-136">Ao usar qualquer tipo de middleware de autenticação, o Middleware de cabeçalhos encaminhado deve ser executado primeiro.</span><span class="sxs-lookup"><span data-stu-id="1b475-136">When using any type of authentication middleware, the Forwarded Headers Middleware must run first.</span></span> <span data-ttu-id="1b475-137">Essa ordenação garantirá que o middleware de autenticação pode consumir os valores de cabeçalho e gerar URIs de redirecionamento correto.</span><span class="sxs-lookup"><span data-stu-id="1b475-137">This ordering ensures that the authentication middleware can consume the header values and generate correct redirect URIs.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="1b475-138">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="1b475-138">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="1b475-139">Invocar o [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) método `Startup.Configure` antes de chamar [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) ou semelhante middleware de esquema de autenticação.</span><span class="sxs-lookup"><span data-stu-id="1b475-139">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="1b475-140">Configurar o middleware para encaminhar o `X-Forwarded-For` e `X-Forwarded-Proto` cabeçalhos:</span><span class="sxs-lookup"><span data-stu-id="1b475-140">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="1b475-141">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="1b475-141">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="1b475-142">Invocar o [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) método `Startup.Configure` antes de chamar [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) e [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) ou esquema de autenticação semelhantes middleware.</span><span class="sxs-lookup"><span data-stu-id="1b475-142">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) and [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="1b475-143">Configurar o middleware para encaminhar o `X-Forwarded-For` e `X-Forwarded-Proto` cabeçalhos:</span><span class="sxs-lookup"><span data-stu-id="1b475-143">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseIdentity();
app.UseFacebookAuthentication(new FacebookOptions()
{
    AppId = Configuration["Authentication:Facebook:AppId"],
    AppSecret = Configuration["Authentication:Facebook:AppSecret"]
});
```

---

<span data-ttu-id="1b475-144">Se nenhum [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) são especificados para o middleware, os cabeçalhos padrão para encaminhar são `None`.</span><span class="sxs-lookup"><span data-stu-id="1b475-144">If no [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) are specified to the middleware, the default headers to forward are `None`.</span></span>

<span data-ttu-id="1b475-145">Configuração adicional pode ser necessária para aplicativos hospedados atrás de servidores proxy e balanceadores de carga.</span><span class="sxs-lookup"><span data-stu-id="1b475-145">Additional configuration might be required for apps hosted behind proxy servers and load balancers.</span></span> <span data-ttu-id="1b475-146">Para obter mais informações, veja [Configurar o ASP.NET Core para trabalhar com servidores proxy e balanceadores de carga](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="1b475-146">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

### <a name="install-nginx"></a><span data-ttu-id="1b475-147">Instalar o Nginx</span><span class="sxs-lookup"><span data-stu-id="1b475-147">Install Nginx</span></span>

```bash
sudo apt-get install nginx
```

> [!NOTE]
> <span data-ttu-id="1b475-148">Se módulos Nginx opcionais serão instalados, pode ser necessário criar Nginx da fonte.</span><span class="sxs-lookup"><span data-stu-id="1b475-148">If optional Nginx modules will be installed, building Nginx from source might be required.</span></span>

<span data-ttu-id="1b475-149">Use `apt-get` para instalar o Nginx.</span><span class="sxs-lookup"><span data-stu-id="1b475-149">Use `apt-get` to install Nginx.</span></span> <span data-ttu-id="1b475-150">O instalador cria um script de inicialização System V que executa o Nginx como daemon na inicialização do sistema.</span><span class="sxs-lookup"><span data-stu-id="1b475-150">The installer creates a System V init script that runs Nginx as daemon on system startup.</span></span> <span data-ttu-id="1b475-151">Já que Nginx foi instalado pela primeira vez, inicie-o explicitamente executando:</span><span class="sxs-lookup"><span data-stu-id="1b475-151">Since Nginx was installed for the first time, explicitly start it by running:</span></span>

```bash
sudo service nginx start
```

<span data-ttu-id="1b475-152">Verifique se um navegador exibe a página de aterrissagem padrão do Nginx.</span><span class="sxs-lookup"><span data-stu-id="1b475-152">Verify a browser displays the default landing page for Nginx.</span></span>

### <a name="configure-nginx"></a><span data-ttu-id="1b475-153">Configurar o Nginx</span><span class="sxs-lookup"><span data-stu-id="1b475-153">Configure Nginx</span></span>

<span data-ttu-id="1b475-154">Para configurar Nginx como um proxy inverso para encaminhar solicitações para seu aplicativo ASP.NET Core, modifique */etc/nginx/sites-available/default*.</span><span class="sxs-lookup"><span data-stu-id="1b475-154">To configure Nginx as a reverse proxy to forward requests to your ASP.NET Core app, modify */etc/nginx/sites-available/default*.</span></span> <span data-ttu-id="1b475-155">Abra-o em um editor de texto arquivo e substitua o conteúdo pelo mostrado a seguir:</span><span class="sxs-lookup"><span data-stu-id="1b475-155">Open it in a text editor, and replace the contents with the following:</span></span>

```nginx
server {
    listen        80;
    server_name   example.com *.example.com;
    location / {
        proxy_pass         http://localhost:5000;
        proxy_http_version 1.1;
        proxy_set_header   Upgrade $http_upgrade;
        proxy_set_header   Connection keep-alive;
        proxy_set_header   Host $http_host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

<span data-ttu-id="1b475-156">Quando nenhum `server_name` correspondências, Nginx usa o servidor padrão.</span><span class="sxs-lookup"><span data-stu-id="1b475-156">When no `server_name` matches, Nginx uses the default server.</span></span> <span data-ttu-id="1b475-157">Se nenhum servidor padrão for definido, o primeiro servidor no arquivo de configuração é o servidor padrão.</span><span class="sxs-lookup"><span data-stu-id="1b475-157">If no default server is defined, the first server in the configuration file is the default server.</span></span> <span data-ttu-id="1b475-158">Como prática recomendada, adicione um servidor de padrão específico que retorna um código de status de 444 no arquivo de configuração.</span><span class="sxs-lookup"><span data-stu-id="1b475-158">As a best practice, add a specific default server which returns a status code of 444 in your configuration file.</span></span> <span data-ttu-id="1b475-159">Um exemplo de configuração de servidor padrão é:</span><span class="sxs-lookup"><span data-stu-id="1b475-159">A default server configuration example is:</span></span>

```nginx
server {
    listen   80 default_server;
    # listen [::]:80 default_server deferred;
    return   444;
}
```

<span data-ttu-id="1b475-160">Com anterior servidor de configuração de arquivo e padrão, Nginx aceita tráfego público na porta 80 com um cabeçalho de host `example.com` ou `*.example.com`.</span><span class="sxs-lookup"><span data-stu-id="1b475-160">With the preceding configuration file and default server, Nginx accepts public traffic on port 80 with host header `example.com` or `*.example.com`.</span></span> <span data-ttu-id="1b475-161">Solicitações que não correspondem a esses hosts não forem encaminhadas para Kestrel.</span><span class="sxs-lookup"><span data-stu-id="1b475-161">Requests not matching these hosts won't get forwarded to Kestrel.</span></span> <span data-ttu-id="1b475-162">Nginx encaminha as solicitações de correspondência para Kestrel em `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="1b475-162">Nginx forwards the matching requests to Kestrel at `http://localhost:5000`.</span></span> <span data-ttu-id="1b475-163">Consulte [como nginx processa uma solicitação](https://nginx.org/docs/http/request_processing.html) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="1b475-163">See [How nginx processes a request](https://nginx.org/docs/http/request_processing.html) for more information.</span></span>

> [!WARNING]
> <span data-ttu-id="1b475-164">Falha ao especificar apropriadas [server_name diretiva](https://nginx.org/docs/http/server_names.html) expõe seu aplicativo para vulnerabilidades de segurança.</span><span class="sxs-lookup"><span data-stu-id="1b475-164">Failure to specify a proper [server_name directive](https://nginx.org/docs/http/server_names.html) exposes your app to security vulnerabilities.</span></span> <span data-ttu-id="1b475-165">Associação de curinga de subdomínio (por exemplo, `*.example.com`) não apresenta esse risco de segurança se você controlar o domínio pai inteira (em vez de `*.com`, que é vulnerável).</span><span class="sxs-lookup"><span data-stu-id="1b475-165">Subdomain wildcard binding (for example, `*.example.com`) doesn't pose this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="1b475-166">Veja [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="1b475-166">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

<span data-ttu-id="1b475-167">Quando a configuração Nginx é estabelecida, execute `sudo nginx -t` para verificar a sintaxe dos arquivos de configuração.</span><span class="sxs-lookup"><span data-stu-id="1b475-167">Once the Nginx configuration is established, run `sudo nginx -t` to verify the syntax of the configuration files.</span></span> <span data-ttu-id="1b475-168">Se o teste do arquivo de configuração for bem-sucedida, forçar Nginx para acompanhar as alterações executando `sudo nginx -s reload`.</span><span class="sxs-lookup"><span data-stu-id="1b475-168">If the configuration file test is successful, force Nginx to pick up the changes by running `sudo nginx -s reload`.</span></span>

## <a name="monitoring-the-app"></a><span data-ttu-id="1b475-169">Monitoramento do aplicativo</span><span class="sxs-lookup"><span data-stu-id="1b475-169">Monitoring the app</span></span>

<span data-ttu-id="1b475-170">O servidor está configurado para encaminhar solicitações feitas a `http://<serveraddress>:80` para o aplicativo ASP.NET Core em execução em Kestrel em `http://127.0.0.1:5000`.</span><span class="sxs-lookup"><span data-stu-id="1b475-170">The server is setup to forward requests made to `http://<serveraddress>:80` on to the ASP.NET Core app running on Kestrel at `http://127.0.0.1:5000`.</span></span> <span data-ttu-id="1b475-171">No entanto, Nginx não está configurado para gerenciar o processo de Kestrel.</span><span class="sxs-lookup"><span data-stu-id="1b475-171">However, Nginx isn't set up to manage the Kestrel process.</span></span> <span data-ttu-id="1b475-172">*systemd* pode ser usado para criar um arquivo de serviço para iniciar e monitorar o aplicativo web subjacente.</span><span class="sxs-lookup"><span data-stu-id="1b475-172">*systemd* can be used to create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="1b475-173">*systemd* é um sistema de inicialização que fornece muitos recursos poderosos para iniciar, parar e gerenciar processos.</span><span class="sxs-lookup"><span data-stu-id="1b475-173">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span> 

### <a name="create-the-service-file"></a><span data-ttu-id="1b475-174">Criar o arquivo de serviço</span><span class="sxs-lookup"><span data-stu-id="1b475-174">Create the service file</span></span>

<span data-ttu-id="1b475-175">Crie o arquivo de definição de serviço:</span><span class="sxs-lookup"><span data-stu-id="1b475-175">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

<span data-ttu-id="1b475-176">Este é um exemplo de arquivo de serviço para o aplicativo:</span><span class="sxs-lookup"><span data-stu-id="1b475-176">The following is an example service file for the app:</span></span>

```ini
[Unit]
Description=Example .NET Web API App running on Ubuntu

[Service]
WorkingDirectory=/var/aspnetcore/hellomvc
ExecStart=/usr/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
Restart=always
RestartSec=10  # Restart service after 10 seconds if dotnet service crashes
SyslogIdentifier=dotnet-example
User=www-data
Environment=ASPNETCORE_ENVIRONMENT=Production
Environment=DOTNET_PRINT_TELEMETRY_MESSAGE=false

[Install]
WantedBy=multi-user.target
```

<span data-ttu-id="1b475-177">**Observação:** se o usuário *www dados* não é usado pela configuração do usuário definido aqui deve ser criado primeiro e dado propriedade adequada para arquivos.</span><span class="sxs-lookup"><span data-stu-id="1b475-177">**Note:** If the user *www-data* isn't used by the configuration, the user defined here must be created first and given proper ownership for files.</span></span>
<span data-ttu-id="1b475-178">**Observação:** Linux tem um sistema de arquivos diferencia maiusculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="1b475-178">**Note:** Linux has a case-sensitive file system.</span></span> <span data-ttu-id="1b475-179">Definindo ASPNETCORE_ENVIRONMENT como resultados de "Produção" na pesquisa do arquivo de configuração *appsettings. Production.JSON*, não *appsettings.production.json*.</span><span class="sxs-lookup"><span data-stu-id="1b475-179">Setting ASPNETCORE_ENVIRONMENT to "Production" results in searching for the configuration file *appsettings.Production.json*, not *appsettings.production.json*.</span></span>

> [!NOTE]
> <span data-ttu-id="1b475-180">Alguns valores (por exemplo, cadeias de conexão de SQL) devem ser substituídas para os provedores de configuração ler as variáveis de ambiente.</span><span class="sxs-lookup"><span data-stu-id="1b475-180">Some values (for example, SQL connection strings) must be escaped for the configuration providers to read the environment variables.</span></span> <span data-ttu-id="1b475-181">Use o seguinte comando para gerar um valor corretamente com caracteres de escape para uso no arquivo de configuração:</span><span class="sxs-lookup"><span data-stu-id="1b475-181">Use the following command to generate a properly escaped value for use in the configuration file:</span></span>
>
> ```console
> systemd-escape "<value-to-escape>"
> ```

<span data-ttu-id="1b475-182">Salve o arquivo e habilite o serviço.</span><span class="sxs-lookup"><span data-stu-id="1b475-182">Save the file and enable the service.</span></span>

```bash
systemctl enable kestrel-hellomvc.service
```

<span data-ttu-id="1b475-183">Inicie o serviço e verifique se ele está em execução.</span><span class="sxs-lookup"><span data-stu-id="1b475-183">Start the service and verify that it's running.</span></span>

```
systemctl start kestrel-hellomvc.service
systemctl status kestrel-hellomvc.service

● kestrel-hellomvc.service - Example .NET Web API App running on Ubuntu
    Loaded: loaded (/etc/systemd/system/kestrel-hellomvc.service; enabled)
    Active: active (running) since Thu 2016-10-18 04:09:35 NZDT; 35s ago
Main PID: 9021 (dotnet)
    CGroup: /system.slice/kestrel-hellomvc.service
            └─9021 /usr/local/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
```

<span data-ttu-id="1b475-184">Com o proxy reverso configurado e Kestrel gerenciadas por meio de systemd, o aplicativo web está totalmente configurado e pode ser acessado a partir de um navegador no computador local em `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="1b475-184">With the reverse proxy configured and Kestrel managed through systemd, the web app is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="1b475-185">Também é acessível a partir de um computador remoto, bloqueio qualquer firewall pode estar bloqueando.</span><span class="sxs-lookup"><span data-stu-id="1b475-185">It's also accessible from a remote machine, barring any firewall that might be blocking.</span></span> <span data-ttu-id="1b475-186">Inspecionando os cabeçalhos de resposta, o `Server` cabeçalho mostra o aplicativo do ASP.NET Core sendo servido por Kestrel.</span><span class="sxs-lookup"><span data-stu-id="1b475-186">Inspecting the response headers, the `Server` header shows the ASP.NET Core app being served by Kestrel.</span></span>

```text
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a><span data-ttu-id="1b475-187">Exibindo logs</span><span class="sxs-lookup"><span data-stu-id="1b475-187">Viewing logs</span></span>

<span data-ttu-id="1b475-188">Desde que o aplicativo web usar Kestrel é gerenciada usando `systemd`, todos os eventos e os processos são registrados para um diário centralizado.</span><span class="sxs-lookup"><span data-stu-id="1b475-188">Since the web app using Kestrel is managed using `systemd`, all events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="1b475-189">No entanto, esse diário contém todas as entradas para todos os serviços e processos gerenciados pelo `systemd`.</span><span class="sxs-lookup"><span data-stu-id="1b475-189">However, this journal includes all entries for all services and processes managed by `systemd`.</span></span> <span data-ttu-id="1b475-190">Para exibir os itens específicos de `kestrel-hellomvc.service`, use o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="1b475-190">To view the `kestrel-hellomvc.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

<span data-ttu-id="1b475-191">Para obter mais filtragem, opções de hora como `--since today`, `--until 1 hour ago` ou uma combinação delas, pode reduzir a quantidade de entradas retornadas.</span><span class="sxs-lookup"><span data-stu-id="1b475-191">For further filtering, time options such as `--since today`, `--until 1 hour ago` or a combination of these can reduce the amount of entries returned.</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-the-app"></a><span data-ttu-id="1b475-192">Protegendo o aplicativo</span><span class="sxs-lookup"><span data-stu-id="1b475-192">Securing the app</span></span>

### <a name="enable-apparmor"></a><span data-ttu-id="1b475-193">Habilitar AppArmor</span><span class="sxs-lookup"><span data-stu-id="1b475-193">Enable AppArmor</span></span>

<span data-ttu-id="1b475-194">Módulos de segurança do Linux (LSM) é uma estrutura que é parte do kernel do Linux desde 2.6 do Linux.</span><span class="sxs-lookup"><span data-stu-id="1b475-194">Linux Security Modules (LSM) is a framework that's part of the Linux kernel since Linux 2.6.</span></span> <span data-ttu-id="1b475-195">O LSM dá suporte a diferentes implementações de módulos de segurança.</span><span class="sxs-lookup"><span data-stu-id="1b475-195">LSM supports different implementations of security modules.</span></span> <span data-ttu-id="1b475-196">O [AppArmor](https://wiki.ubuntu.com/AppArmor) é um LSM que implementa um sistema de controle de acesso obrigatório que permite restringir o programa a um conjunto limitado de recursos.</span><span class="sxs-lookup"><span data-stu-id="1b475-196">[AppArmor](https://wiki.ubuntu.com/AppArmor) is a LSM that implements a Mandatory Access Control system which allows confining the program to a limited set of resources.</span></span> <span data-ttu-id="1b475-197">Verifique se o AppArmor está habilitado e configurado corretamente.</span><span class="sxs-lookup"><span data-stu-id="1b475-197">Ensure AppArmor is enabled and properly configured.</span></span>

### <a name="configuring-the-firewall"></a><span data-ttu-id="1b475-198">Configurando o firewall</span><span class="sxs-lookup"><span data-stu-id="1b475-198">Configuring the firewall</span></span>

<span data-ttu-id="1b475-199">Feche todas as portas externas que não estão em uso.</span><span class="sxs-lookup"><span data-stu-id="1b475-199">Close off all external ports that are not in use.</span></span> <span data-ttu-id="1b475-200">Firewall descomplicado (ufw) fornece um front-end para `iptables` fornecendo uma interface de linha de comando para configurar o firewall.</span><span class="sxs-lookup"><span data-stu-id="1b475-200">Uncomplicated firewall (ufw) provides a front end for `iptables` by providing a command line interface for configuring the firewall.</span></span> <span data-ttu-id="1b475-201">Verifique `ufw` está configurado para permitir o tráfego em todas as portas necessárias.</span><span class="sxs-lookup"><span data-stu-id="1b475-201">Verify that `ufw` is configured to allow traffic on any ports needed.</span></span>

```bash
sudo apt-get install ufw
sudo ufw enable

sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
```

### <a name="securing-nginx"></a><span data-ttu-id="1b475-202">Protegendo o Nginx</span><span class="sxs-lookup"><span data-stu-id="1b475-202">Securing Nginx</span></span>

<span data-ttu-id="1b475-203">A distribuição padrão do Nginx não habilita o SSL.</span><span class="sxs-lookup"><span data-stu-id="1b475-203">The default distribution of Nginx doesn't enable SSL.</span></span> <span data-ttu-id="1b475-204">Para habilitar recursos de segurança adicionais, compile da origem.</span><span class="sxs-lookup"><span data-stu-id="1b475-204">To enable additional security features, build from source.</span></span>

#### <a name="download-the-source-and-install-the-build-dependencies"></a><span data-ttu-id="1b475-205">Baixe a origem e instale as dependências de build</span><span class="sxs-lookup"><span data-stu-id="1b475-205">Download the source and install the build dependencies</span></span>

```bash
# Install the build dependencies
sudo apt-get update
sudo apt-get install build-essential zlib1g-dev libpcre3-dev libssl-dev libxslt1-dev libxml2-dev libgd2-xpm-dev libgeoip-dev libgoogle-perftools-dev libperl-dev

# Download Nginx 1.10.0 or latest
wget http://www.nginx.org/download/nginx-1.10.0.tar.gz
tar zxf nginx-1.10.0.tar.gz
```

#### <a name="change-the-nginx-response-name"></a><span data-ttu-id="1b475-206">Alterar o nome da resposta do Nginx</span><span class="sxs-lookup"><span data-stu-id="1b475-206">Change the Nginx response name</span></span>

<span data-ttu-id="1b475-207">Edite *src/http/ngx_http_header_filter_module.c*:</span><span class="sxs-lookup"><span data-stu-id="1b475-207">Edit *src/http/ngx_http_header_filter_module.c*:</span></span>

```c
static char ngx_http_server_string[] = "Server: Web Server" CRLF;
static char ngx_http_server_full_string[] = "Server: Web Server" CRLF;
```

#### <a name="configure-the-options-and-build"></a><span data-ttu-id="1b475-208">Configurar as opções e compilar</span><span class="sxs-lookup"><span data-stu-id="1b475-208">Configure the options and build</span></span>

<span data-ttu-id="1b475-209">A biblioteca PCRE é necessária para expressões regulares.</span><span class="sxs-lookup"><span data-stu-id="1b475-209">The PCRE library is required for regular expressions.</span></span> <span data-ttu-id="1b475-210">Expressões regulares são usadas na diretiva local para o ngx_http_rewrite_module.</span><span class="sxs-lookup"><span data-stu-id="1b475-210">Regular expressions are used in the location directive for the ngx_http_rewrite_module.</span></span> <span data-ttu-id="1b475-211">O http_ssl_module adiciona suporte a protocolo HTTPS.</span><span class="sxs-lookup"><span data-stu-id="1b475-211">The http_ssl_module adds HTTPS protocol support.</span></span>

<span data-ttu-id="1b475-212">Considere o uso de um firewall de aplicativo da web como *ModSecurity* para proteger o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="1b475-212">Consider using a web app firewall like *ModSecurity* to harden the app.</span></span>

```bash
./configure
--with-pcre=../pcre-8.38
--with-zlib=../zlib-1.2.8
--with-http_ssl_module
--with-stream
--with-mail=dynamic
```

#### <a name="configure-ssl"></a><span data-ttu-id="1b475-213">Configurar o SSL</span><span class="sxs-lookup"><span data-stu-id="1b475-213">Configure SSL</span></span>

* <span data-ttu-id="1b475-214">Configurar o servidor para ouvir o tráfego HTTPS na porta `443` especificando um certificado válido emitido por uma autoridade de certificação confiável (CA).</span><span class="sxs-lookup"><span data-stu-id="1b475-214">Configure the server to listen to HTTPS traffic on port `443` by specifying a valid certificate issued by a trusted Certificate Authority (CA).</span></span>

* <span data-ttu-id="1b475-215">Proteger a segurança, empregando algumas das práticas descritas na seguinte */etc/nginx/nginx.conf* arquivo.</span><span class="sxs-lookup"><span data-stu-id="1b475-215">Harden the security by employing some of the practices depicted in the following */etc/nginx/nginx.conf* file.</span></span> <span data-ttu-id="1b475-216">Exemplos incluem a escolha de uma criptografia mais forte e o redirecionamento de todo o tráfego por meio de HTTP para HTTPS.</span><span class="sxs-lookup"><span data-stu-id="1b475-216">Examples include choosing a stronger cipher and redirecting all traffic over HTTP to HTTPS.</span></span>

* <span data-ttu-id="1b475-217">Adicionar um cabeçalho `HTTP Strict-Transport-Security` (HSTS) garante que todas as solicitações subsequentes feitas pelo cliente sejam apenas via HTTPS.</span><span class="sxs-lookup"><span data-stu-id="1b475-217">Adding an `HTTP Strict-Transport-Security` (HSTS) header ensures all subsequent requests made by the client are over HTTPS only.</span></span>

* <span data-ttu-id="1b475-218">Não adicione o cabeçalho de segurança de transporte Strict ou escolher um apropriado `max-age` se SSL será desabilitado no futuro.</span><span class="sxs-lookup"><span data-stu-id="1b475-218">Don't add the Strict-Transport-Security header or chose an appropriate `max-age` if SSL will be disabled in the future.</span></span>

<span data-ttu-id="1b475-219">Adicione o arquivo de configuração */etc/nginx/proxy.conf*:</span><span class="sxs-lookup"><span data-stu-id="1b475-219">Add the */etc/nginx/proxy.conf* configuration file:</span></span>

[!code-nginx[](linux-nginx/proxy.conf)]

<span data-ttu-id="1b475-220">Edite o arquivo de configuração */etc/nginx/nginx.conf*.</span><span class="sxs-lookup"><span data-stu-id="1b475-220">Edit the */etc/nginx/nginx.conf* configuration file.</span></span> <span data-ttu-id="1b475-221">O exemplo contém ambas as seções `http` e `server` em um arquivo de configuração.</span><span class="sxs-lookup"><span data-stu-id="1b475-221">The example contains both `http` and `server` sections in one configuration file.</span></span>

[!code-nginx[](linux-nginx/nginx.conf?highlight=2)]

#### <a name="secure-nginx-from-clickjacking"></a><span data-ttu-id="1b475-222">Proteger o Nginx de clickjacking</span><span class="sxs-lookup"><span data-stu-id="1b475-222">Secure Nginx from clickjacking</span></span>
<span data-ttu-id="1b475-223">Clickjacking é uma técnica mal-intencionada para coletar cliques de um usuário infectado.</span><span class="sxs-lookup"><span data-stu-id="1b475-223">Clickjacking is a malicious technique to collect an infected user's clicks.</span></span> <span data-ttu-id="1b475-224">O clickjacking engana a vítima (visitante) fazendo-a clicar em um site infectado.</span><span class="sxs-lookup"><span data-stu-id="1b475-224">Clickjacking tricks the victim (visitor) into clicking on an infected site.</span></span> <span data-ttu-id="1b475-225">Use X-FRAME-OPTIONS para proteger o site.</span><span class="sxs-lookup"><span data-stu-id="1b475-225">Use X-FRAME-OPTIONS to secure the site.</span></span>

<span data-ttu-id="1b475-226">Edite o arquivo *nginx.conf*:</span><span class="sxs-lookup"><span data-stu-id="1b475-226">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="1b475-227">Adicione a linha `add_header X-Frame-Options "SAMEORIGIN";` e salve o arquivo, depois reinicie o Nginx.</span><span class="sxs-lookup"><span data-stu-id="1b475-227">Add the line `add_header X-Frame-Options "SAMEORIGIN";` and save the file, then restart Nginx.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="1b475-228">Detecção de tipo MIME</span><span class="sxs-lookup"><span data-stu-id="1b475-228">MIME-type sniffing</span></span>

<span data-ttu-id="1b475-229">Esse cabeçalho evita que a maioria dos navegadores faça detecção MIME de uma resposta distante do tipo de conteúdo declarado, visto que o cabeçalho instrui o navegador para não substituir o tipo de conteúdo de resposta.</span><span class="sxs-lookup"><span data-stu-id="1b475-229">This header prevents most browsers from MIME-sniffing a response away from the declared content type, as the header instructs the browser not to override the response content type.</span></span> <span data-ttu-id="1b475-230">Com a opção `nosniff`, se o servidor informa que o conteúdo é "text/html", o navegador renderiza-a como "text/html".</span><span class="sxs-lookup"><span data-stu-id="1b475-230">With the `nosniff` option, if the server says the content is "text/html", the browser renders it as "text/html".</span></span>

<span data-ttu-id="1b475-231">Edite o arquivo *nginx.conf*:</span><span class="sxs-lookup"><span data-stu-id="1b475-231">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="1b475-232">Adicione a linha `add_header X-Content-Type-Options "nosniff";` e salve o arquivo, depois reinicie o Nginx.</span><span class="sxs-lookup"><span data-stu-id="1b475-232">Add the line `add_header X-Content-Type-Options "nosniff";` and save the file, then restart Nginx.</span></span>

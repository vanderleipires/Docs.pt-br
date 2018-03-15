---
title: Hospedar o ASP.NET Core no Linux com o Apache
description: "Saiba como configurar o Apache como um servidor de proxy reverso CentOS para redirecionar o tráfego HTTP para um aplicativo web do ASP.NET Core em execução no Kestrel."
author: spboyer
manager: wpickett
ms.author: spboyer
ms.custom: mvc
ms.date: 03/13/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/linux-apache
ms.openlocfilehash: 033adddc586b60c9f7453df5434617aa838737f8
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/15/2018
---
# <a name="host-aspnet-core-on-linux-with-apache"></a><span data-ttu-id="ed375-103">Hospedar o ASP.NET Core no Linux com o Apache</span><span class="sxs-lookup"><span data-stu-id="ed375-103">Host ASP.NET Core on Linux with Apache</span></span>

<span data-ttu-id="ed375-104">Por [Shayne Boyer](https://github.com/spboyer)</span><span class="sxs-lookup"><span data-stu-id="ed375-104">By [Shayne Boyer](https://github.com/spboyer)</span></span>

<span data-ttu-id="ed375-105">Usando este guia, saiba como configurar [Apache](https://httpd.apache.org/) como um servidor de proxy reverso na [CentOS 7](https://www.centos.org/) para redirecionar o tráfego de HTTP para um aplicativo web do ASP.NET Core em execução no [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="ed375-105">Using this guide, learn how to set up [Apache](https://httpd.apache.org/) as a reverse proxy server on [CentOS 7](https://www.centos.org/) to redirect HTTP traffic to an ASP.NET Core web app running on [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="ed375-106">O [mod_proxy extensão](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html) e módulos relacionados Criar proxy reverso do servidor.</span><span class="sxs-lookup"><span data-stu-id="ed375-106">The [mod_proxy extension](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html) and related modules create the server's reverse proxy.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ed375-107">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="ed375-107">Prerequisites</span></span>

1. <span data-ttu-id="ed375-108">Servidor que executa o CentOS 7 com uma conta de usuário padrão com privilégios sudo</span><span class="sxs-lookup"><span data-stu-id="ed375-108">Server running CentOS 7 with a standard user account with sudo privilege</span></span>
2. <span data-ttu-id="ed375-109">Aplicativo do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ed375-109">ASP.NET Core app</span></span>

## <a name="publish-the-app"></a><span data-ttu-id="ed375-110">Publique o aplicativo</span><span class="sxs-lookup"><span data-stu-id="ed375-110">Publish the app</span></span>

<span data-ttu-id="ed375-111">Publicar o aplicativo como um [implantação autossuficiente](/dotnet/core/deploying/#self-contained-deployments-scd) na configuração de versão para o tempo de execução do CentOS 7 (`centos.7-x64`).</span><span class="sxs-lookup"><span data-stu-id="ed375-111">Publish the app as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) in Release configuration for the CentOS 7 runtime (`centos.7-x64`).</span></span> <span data-ttu-id="ed375-112">Copie o conteúdo de *bin/Release/netcoreapp2.0/centos.7-x64/publish* pasta ao servidor usando o SCP, FTP ou outro método de transferência de arquivo.</span><span class="sxs-lookup"><span data-stu-id="ed375-112">Copy the contents of the *bin/Release/netcoreapp2.0/centos.7-x64/publish* folder to the server using SCP, FTP, or other file transfer method.</span></span>

> [!NOTE]
> <span data-ttu-id="ed375-113">Em um cenário de implantação de produção, um fluxo de trabalho de integração contínua faz o trabalho de publicação do aplicativo e copiar os ativos para o servidor.</span><span class="sxs-lookup"><span data-stu-id="ed375-113">Under a production deployment scenario, a continuous integration workflow does the work of publishing the app and copying the assets to the server.</span></span> 

## <a name="configure-a-proxy-server"></a><span data-ttu-id="ed375-114">Configurar um servidor proxy</span><span class="sxs-lookup"><span data-stu-id="ed375-114">Configure a proxy server</span></span>

<span data-ttu-id="ed375-115">Um proxy reverso é uma instalação comum para que serve a aplicativos web dinâmicos.</span><span class="sxs-lookup"><span data-stu-id="ed375-115">A reverse proxy is a common setup for serving dynamic web apps.</span></span> <span data-ttu-id="ed375-116">O proxy inverso encerra a solicitação HTTP e as encaminha para o aplicativo ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="ed375-116">The reverse proxy terminates the HTTP request and forwards it to the ASP.NET app.</span></span>

<span data-ttu-id="ed375-117">Um servidor proxy é uma que encaminha as solicitações de cliente para outro servidor, em vez de atender às solicitações em si.</span><span class="sxs-lookup"><span data-stu-id="ed375-117">A proxy server is one which forwards client requests to another server instead of fulfilling requests itself.</span></span> <span data-ttu-id="ed375-118">Um proxy reverso encaminha para um destino fixo, normalmente, em nome de clientes arbitrários.</span><span class="sxs-lookup"><span data-stu-id="ed375-118">A reverse proxy forwards to a fixed destination, typically on behalf of arbitrary clients.</span></span> <span data-ttu-id="ed375-119">Neste guia, Apache é configurado como o proxy inverso em execução no mesmo servidor que Kestrel está atendendo o aplicativo ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ed375-119">In this guide, Apache is configured as the reverse proxy running on the same server that Kestrel is serving the ASP.NET Core app.</span></span>

<span data-ttu-id="ed375-120">Como as solicitações são encaminhadas pelo proxy reverso, use o Middleware de cabeçalhos encaminhados do [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) pacote.</span><span class="sxs-lookup"><span data-stu-id="ed375-120">Because requests are forwarded by reverse proxy, use the Forwarded Headers Middleware from the [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) package.</span></span> <span data-ttu-id="ed375-121">As atualizações de middleware de `Request.Scheme`, usando o `X-Forwarded-Proto` cabeçalho, de forma que URIs de redirecionamento e outras políticas de segurança funcionam corretamente.</span><span class="sxs-lookup"><span data-stu-id="ed375-121">The middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header, so that redirect URIs and other security policies work correctly.</span></span>

<span data-ttu-id="ed375-122">Ao usar qualquer tipo de middleware de autenticação, o Middleware de cabeçalhos encaminhado deve ser executado primeiro.</span><span class="sxs-lookup"><span data-stu-id="ed375-122">When using any type of authentication middleware, the Forwarded Headers Middleware must run first.</span></span> <span data-ttu-id="ed375-123">Essa ordenação garantirá que o middleware de autenticação pode consumir os valores de cabeçalho e gerar URIs de redirecionamento correto.</span><span class="sxs-lookup"><span data-stu-id="ed375-123">This ordering ensures that the authentication middleware can consume the header values and generate correct redirect URIs.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ed375-124">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ed375-124">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="ed375-125">Invocar o [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) método `Startup.Configure` antes de chamar [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) ou semelhante middleware de esquema de autenticação:</span><span class="sxs-lookup"><span data-stu-id="ed375-125">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) or similar authentication scheme middleware:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ed375-126">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ed375-126">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="ed375-127">Invocar o [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) método `Startup.Configure` antes de chamar [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) e [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) ou esquema de autenticação semelhantes middleware:</span><span class="sxs-lookup"><span data-stu-id="ed375-127">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) and [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) or similar authentication scheme middleware:</span></span>

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

<span data-ttu-id="ed375-128">Se nenhum [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) são especificados para o middleware, os cabeçalhos padrão para encaminhar são `None`.</span><span class="sxs-lookup"><span data-stu-id="ed375-128">If no [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) are specified to the middleware, the default headers to forward are `None`.</span></span>

### <a name="install-apache"></a><span data-ttu-id="ed375-129">Instalar o Apache</span><span class="sxs-lookup"><span data-stu-id="ed375-129">Install Apache</span></span>

<span data-ttu-id="ed375-130">CentOS pacotes de atualização de suas versões estável mais recente:</span><span class="sxs-lookup"><span data-stu-id="ed375-130">Update CentOS packages to their latest stable versions:</span></span>

```bash
sudo yum update -y
```

<span data-ttu-id="ed375-131">Instalar o servidor web Apache em CentOS com um único `yum` comando:</span><span class="sxs-lookup"><span data-stu-id="ed375-131">Install the Apache web server on CentOS with a single `yum` command:</span></span>

```bash
sudo yum -y install httpd mod_ssl
```

<span data-ttu-id="ed375-132">Exemplo de saída depois de executar o comando:</span><span class="sxs-lookup"><span data-stu-id="ed375-132">Sample output after running the command:</span></span>

```bash
Downloading packages:
httpd-2.4.6-40.el7.centos.4.x86_64.rpm               | 2.7 MB  00:00:01
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
Installing : httpd-2.4.6-40.el7.centos.4.x86_64      1/1 
Verifying  : httpd-2.4.6-40.el7.centos.4.x86_64      1/1 

Installed:
httpd.x86_64 0:2.4.6-40.el7.centos.4

Complete!
```

> [!NOTE]
> <span data-ttu-id="ed375-133">Neste exemplo, a saída reflete httpd.86_64 desde a versão CentOS 7 é de 64 bits.</span><span class="sxs-lookup"><span data-stu-id="ed375-133">In this example, the output reflects httpd.86_64 since the CentOS 7 version is 64 bit.</span></span> <span data-ttu-id="ed375-134">Para verificar o local em que o Apache está instalado, execute `whereis httpd` em um prompt de comando.</span><span class="sxs-lookup"><span data-stu-id="ed375-134">To verify where Apache is installed, run `whereis httpd` from a command prompt.</span></span>

### <a name="configure-apache-for-reverse-proxy"></a><span data-ttu-id="ed375-135">Configurar o Apache para proxy reverso</span><span class="sxs-lookup"><span data-stu-id="ed375-135">Configure Apache for reverse proxy</span></span>

<span data-ttu-id="ed375-136">Os arquivos de configuração do Apache estão localizados no diretório `/etc/httpd/conf.d/`.</span><span class="sxs-lookup"><span data-stu-id="ed375-136">Configuration files for Apache are located within the `/etc/httpd/conf.d/` directory.</span></span> <span data-ttu-id="ed375-137">Qualquer arquivo com o *.conf* extensão é processada em ordem alfabética, além dos arquivos de configuração do módulo em `/etc/httpd/conf.modules.d/`, que contém todas as configurações de arquivos necessário para carregar módulos.</span><span class="sxs-lookup"><span data-stu-id="ed375-137">Any file with the *.conf* extension is processed in alphabetical order in addition to the module configuration files in `/etc/httpd/conf.modules.d/`, which contains any configuration files necessary to load modules.</span></span>

<span data-ttu-id="ed375-138">Crie um arquivo de configuração, chamado *hellomvc.conf*, para o aplicativo:</span><span class="sxs-lookup"><span data-stu-id="ed375-138">Create a configuration file, named *hellomvc.conf*, for the app:</span></span>

```
<VirtualHost *:80>
    ProxyPreserveHost On
    ProxyPass / http://127.0.0.1:5000/
    ProxyPassReverse / http://127.0.0.1:5000/
    ServerName www.example.com
    ServerAlias *.example.com
    ErrorLog ${APACHE_LOG_DIR}hellomvc-error.log
    CustomLog ${APACHE_LOG_DIR}hellomvc-access.log common
</VirtualHost>
```

<span data-ttu-id="ed375-139">O `VirtualHost` bloco pode aparecer várias vezes, em um ou mais arquivos em um servidor.</span><span class="sxs-lookup"><span data-stu-id="ed375-139">The `VirtualHost` block can appear multiple times, in one or more files on a server.</span></span> <span data-ttu-id="ed375-140">No arquivo de configuração anterior, Apache aceita tráfego público na porta 80.</span><span class="sxs-lookup"><span data-stu-id="ed375-140">In the preceding configuration file, Apache accepts public traffic on port 80.</span></span> <span data-ttu-id="ed375-141">O domínio `www.example.com` é mantido e o `*.example.com` alias resolve para o mesmo site.</span><span class="sxs-lookup"><span data-stu-id="ed375-141">The domain `www.example.com` is being served, and the `*.example.com` alias resolves to the same website.</span></span> <span data-ttu-id="ed375-142">Consulte [suporte baseado em nome de host virtual](https://httpd.apache.org/docs/current/vhosts/name-based.html) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="ed375-142">See [Name-based virtual host support](https://httpd.apache.org/docs/current/vhosts/name-based.html) for more information.</span></span> <span data-ttu-id="ed375-143">As solicitações são delegadas na raiz para a porta 5000 do servidor no 127.0.0.1.</span><span class="sxs-lookup"><span data-stu-id="ed375-143">Requests are proxied at the root to port 5000 of the server at 127.0.0.1.</span></span> <span data-ttu-id="ed375-144">Para a comunicação bidirecional, `ProxyPass` e `ProxyPassReverse` são necessários.</span><span class="sxs-lookup"><span data-stu-id="ed375-144">For bi-directional communication, `ProxyPass` and `ProxyPassReverse` are required.</span></span>

> [!WARNING]
> <span data-ttu-id="ed375-145">Falha ao especificar apropriadas [diretiva ServerName](https://httpd.apache.org/docs/current/mod/core.html#servername) no **VirtualHost** bloco expõe seu aplicativo para vulnerabilidades de segurança.</span><span class="sxs-lookup"><span data-stu-id="ed375-145">Failure to specify a proper [ServerName directive](https://httpd.apache.org/docs/current/mod/core.html#servername) in the **VirtualHost** block exposes your app to security vulnerabilities.</span></span> <span data-ttu-id="ed375-146">Associação de curinga de subdomínio (por exemplo, `*.example.com`) não apresenta esse risco de segurança se você controlar o domínio pai inteira (em vez de `*.com`, que é vulnerável).</span><span class="sxs-lookup"><span data-stu-id="ed375-146">Subdomain wildcard binding (for example, `*.example.com`) doesn't pose this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="ed375-147">Veja [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="ed375-147">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

<span data-ttu-id="ed375-148">Registro em log pode ser configurado por `VirtualHost` usando `ErrorLog` e `CustomLog` diretivas.</span><span class="sxs-lookup"><span data-stu-id="ed375-148">Logging can be configured per `VirtualHost` using `ErrorLog` and `CustomLog` directives.</span></span> <span data-ttu-id="ed375-149">`ErrorLog` é o local onde o servidor registra erros, e `CustomLog` define o nome de arquivo e o formato do arquivo de log.</span><span class="sxs-lookup"><span data-stu-id="ed375-149">`ErrorLog` is the location where the server logs errors, and `CustomLog` sets the filename and format of log file.</span></span> <span data-ttu-id="ed375-150">Nesse caso, isso é onde as informações de solicitação são registradas.</span><span class="sxs-lookup"><span data-stu-id="ed375-150">In this case, this is where request information is logged.</span></span> <span data-ttu-id="ed375-151">Há uma linha para cada solicitação.</span><span class="sxs-lookup"><span data-stu-id="ed375-151">There's one line for each request.</span></span>

<span data-ttu-id="ed375-152">Salve o arquivo e testar a configuração.</span><span class="sxs-lookup"><span data-stu-id="ed375-152">Save the file and test the configuration.</span></span> <span data-ttu-id="ed375-153">Se tudo passar, a resposta deverá ser `Syntax [OK]`.</span><span class="sxs-lookup"><span data-stu-id="ed375-153">If everything passes, the response should be `Syntax [OK]`.</span></span>

```bash
sudo service httpd configtest
```

<span data-ttu-id="ed375-154">Reinicie o Apache:</span><span class="sxs-lookup"><span data-stu-id="ed375-154">Restart Apache:</span></span>

```bash
sudo systemctl restart httpd
sudo systemctl enable httpd
```

## <a name="monitoring-the-app"></a><span data-ttu-id="ed375-155">Monitoramento do aplicativo</span><span class="sxs-lookup"><span data-stu-id="ed375-155">Monitoring the app</span></span>

<span data-ttu-id="ed375-156">Apache agora está configurado para encaminhar solicitações feitas a `http://localhost:80` para o aplicativo ASP.NET Core em execução em Kestrel em `http://127.0.0.1:5000`.</span><span class="sxs-lookup"><span data-stu-id="ed375-156">Apache is now setup to forward requests made to `http://localhost:80` to the ASP.NET Core app running on Kestrel at `http://127.0.0.1:5000`.</span></span>  <span data-ttu-id="ed375-157">No entanto, o Apache não está configurado para gerenciar o processo de Kestrel.</span><span class="sxs-lookup"><span data-stu-id="ed375-157">However, Apache isn't set up to manage the Kestrel process.</span></span> <span data-ttu-id="ed375-158">Use *systemd* e crie um arquivo de serviço para iniciar e monitorar o aplicativo web subjacente.</span><span class="sxs-lookup"><span data-stu-id="ed375-158">Use *systemd* and create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="ed375-159">*systemd* é um sistema de inicialização que fornece muitos recursos poderosos para iniciar, parar e gerenciar processos.</span><span class="sxs-lookup"><span data-stu-id="ed375-159">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span> 


### <a name="create-the-service-file"></a><span data-ttu-id="ed375-160">Criar o arquivo de serviço</span><span class="sxs-lookup"><span data-stu-id="ed375-160">Create the service file</span></span>

<span data-ttu-id="ed375-161">Crie o arquivo de definição de serviço:</span><span class="sxs-lookup"><span data-stu-id="ed375-161">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

<span data-ttu-id="ed375-162">Um arquivo de serviço de exemplo para o aplicativo:</span><span class="sxs-lookup"><span data-stu-id="ed375-162">An example service file for the app:</span></span>

```
[Unit]
Description=Example .NET Web API App running on CentOS 7

[Service]
WorkingDirectory=/var/aspnetcore/hellomvc
ExecStart=/usr/local/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
Restart=always
# Restart service after 10 seconds if dotnet service crashes
RestartSec=10
SyslogIdentifier=dotnet-example
User=apache
Environment=ASPNETCORE_ENVIRONMENT=Production 

[Install]
WantedBy=multi-user.target
```

> [!NOTE]
> <span data-ttu-id="ed375-163">**Usuário** &mdash; se o usuário *apache* não é usado pela configuração, o usuário deve criado pela primeira vez e determinado propriedade adequada de arquivos.</span><span class="sxs-lookup"><span data-stu-id="ed375-163">**User** &mdash; If the user *apache* isn't used by the configuration, the user must be created first and given proper ownership for files.</span></span>

<span data-ttu-id="ed375-164">Salve o arquivo e habilitar o serviço:</span><span class="sxs-lookup"><span data-stu-id="ed375-164">Save the file and enable the service:</span></span>

```bash
systemctl enable kestrel-hellomvc.service
```

<span data-ttu-id="ed375-165">Inicie o serviço e verifique se ele está em execução:</span><span class="sxs-lookup"><span data-stu-id="ed375-165">Start the service and verify that it's running:</span></span>

```bash
systemctl start kestrel-hellomvc.service
systemctl status kestrel-hellomvc.service

● kestrel-hellomvc.service - Example .NET Web API App running on CentOS 7
    Loaded: loaded (/etc/systemd/system/kestrel-hellomvc.service; enabled)
    Active: active (running) since Thu 2016-10-18 04:09:35 NZDT; 35s ago
Main PID: 9021 (dotnet)
    CGroup: /system.slice/kestrel-hellomvc.service
            └─9021 /usr/local/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
```

<span data-ttu-id="ed375-166">Com o proxy reverso configurado e gerenciadas por meio de Kestrel *systemd*, o aplicativo web está totalmente configurado e pode ser acessado a partir de um navegador no computador local em `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="ed375-166">With the reverse proxy configured and Kestrel managed through *systemd*, the web app is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="ed375-167">Inspecionando os cabeçalhos de resposta, o **Server** cabeçalho indica que o aplicativo ASP.NET Core é atendido por Kestrel:</span><span class="sxs-lookup"><span data-stu-id="ed375-167">Inspecting the response headers, the **Server** header indicates that the ASP.NET Core app is served by Kestrel:</span></span>

```
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a><span data-ttu-id="ed375-168">Exibindo logs</span><span class="sxs-lookup"><span data-stu-id="ed375-168">Viewing logs</span></span>

<span data-ttu-id="ed375-169">Desde que o aplicativo web usar Kestrel é gerenciada usando *systemd*, eventos e os processos são registrados para um diário centralizado.</span><span class="sxs-lookup"><span data-stu-id="ed375-169">Since the web app using Kestrel is managed using *systemd*, events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="ed375-170">No entanto, esse diário contém entradas para todos os serviços e processos gerenciados pelo *systemd*.</span><span class="sxs-lookup"><span data-stu-id="ed375-170">However, this journal includes entries for all of the services and processes managed by *systemd*.</span></span> <span data-ttu-id="ed375-171">Para exibir os itens específicos de `kestrel-hellomvc.service`, use o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="ed375-171">To view the `kestrel-hellomvc.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

<span data-ttu-id="ed375-172">Para filtragem de hora, especifica opções de tempo com o comando.</span><span class="sxs-lookup"><span data-stu-id="ed375-172">For time filtering, specify time options with the command.</span></span> <span data-ttu-id="ed375-173">Por exemplo, use `--since today` para filtrar o dia atual ou `--until 1 hour ago` para ver as entradas da hora anterior.</span><span class="sxs-lookup"><span data-stu-id="ed375-173">For example, use `--since today` to filter for the current day or `--until 1 hour ago` to see the previous hour's entries.</span></span> <span data-ttu-id="ed375-174">Para obter mais informações, consulte o [página para journalctl](https://www.unix.com/man-page/centos/1/journalctl/).</span><span class="sxs-lookup"><span data-stu-id="ed375-174">For more information, see the [man page for journalctl](https://www.unix.com/man-page/centos/1/journalctl/).</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-the-app"></a><span data-ttu-id="ed375-175">Protegendo o aplicativo</span><span class="sxs-lookup"><span data-stu-id="ed375-175">Securing the app</span></span>

### <a name="configure-firewall"></a><span data-ttu-id="ed375-176">Configurar o firewall</span><span class="sxs-lookup"><span data-stu-id="ed375-176">Configure firewall</span></span>

<span data-ttu-id="ed375-177">*Firewalld* é um daemon dinâmico para gerenciar o firewall com suporte para zonas de rede.</span><span class="sxs-lookup"><span data-stu-id="ed375-177">*Firewalld* is a dynamic daemon to manage the firewall with support for network zones.</span></span> <span data-ttu-id="ed375-178">Portas e filtragem de pacote ainda podem ser gerenciados pelo iptables.</span><span class="sxs-lookup"><span data-stu-id="ed375-178">Ports and packet filtering can still be managed by iptables.</span></span> <span data-ttu-id="ed375-179">*Firewalld* devem ser instalados por padrão.</span><span class="sxs-lookup"><span data-stu-id="ed375-179">*Firewalld* should be installed by default.</span></span> <span data-ttu-id="ed375-180">`yum` pode ser usado para instalar o pacote ou verifique se que ele está instalado.</span><span class="sxs-lookup"><span data-stu-id="ed375-180">`yum` can be used to install the package or verify it's installed.</span></span>

```bash
sudo yum install firewalld -y
```

<span data-ttu-id="ed375-181">Use `firewalld` para abrir somente as portas necessárias para o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="ed375-181">Use `firewalld` to open only the ports needed for the app.</span></span> <span data-ttu-id="ed375-182">Nesse caso, as portas 80 e 443 são usadas.</span><span class="sxs-lookup"><span data-stu-id="ed375-182">In this case, port 80 and 443 are used.</span></span> <span data-ttu-id="ed375-183">Os comandos a seguir definem permanentemente as portas 80 e 443 para abrir:</span><span class="sxs-lookup"><span data-stu-id="ed375-183">The following commands permanently set ports 80 and 443 to open:</span></span>

```bash
sudo firewall-cmd --add-port=80/tcp --permanent
sudo firewall-cmd --add-port=443/tcp --permanent
```

<span data-ttu-id="ed375-184">Recarrega as configurações de firewall.</span><span class="sxs-lookup"><span data-stu-id="ed375-184">Reload the firewall settings.</span></span> <span data-ttu-id="ed375-185">Verifique os serviços disponíveis e as portas na zona padrão.</span><span class="sxs-lookup"><span data-stu-id="ed375-185">Check the available services and ports in the default zone.</span></span> <span data-ttu-id="ed375-186">Opções estão disponíveis por meio da inspeção `firewall-cmd -h`.</span><span class="sxs-lookup"><span data-stu-id="ed375-186">Options are available by inspecting `firewall-cmd -h`.</span></span>

```bash 
sudo firewall-cmd --reload
sudo firewall-cmd --list-all
```

```bash
public (default, active)
interfaces: eth0
sources: 
services: dhcpv6-client
ports: 443/tcp 80/tcp
masquerade: no
forward-ports: 
icmp-blocks: 
rich rules: 
```

### <a name="ssl-configuration"></a><span data-ttu-id="ed375-187">Configuração do SSL</span><span class="sxs-lookup"><span data-stu-id="ed375-187">SSL configuration</span></span>

<span data-ttu-id="ed375-188">Para configurar o Apache para SSL, o *mod_ssl* módulo é usado.</span><span class="sxs-lookup"><span data-stu-id="ed375-188">To configure Apache for SSL, the *mod_ssl* module is used.</span></span> <span data-ttu-id="ed375-189">Quando o *httpd* módulo foi instalado, o *mod_ssl* módulo também foi instalado.</span><span class="sxs-lookup"><span data-stu-id="ed375-189">When the *httpd* module was installed, the *mod_ssl* module was also installed.</span></span> <span data-ttu-id="ed375-190">Se ele não foi instalado, use `yum` para adicioná-lo para a configuração.</span><span class="sxs-lookup"><span data-stu-id="ed375-190">If it wasn't installed, use `yum` to add it to the configuration.</span></span>

```bash
sudo yum install mod_ssl
```
<span data-ttu-id="ed375-191">Para impor a SSL, instale o `mod_rewrite` módulo para habilitar a regravação de URL:</span><span class="sxs-lookup"><span data-stu-id="ed375-191">To enforce SSL, install the `mod_rewrite` module to enable URL rewriting:</span></span>

```bash
sudo yum install mod_rewrite
```

<span data-ttu-id="ed375-192">Modificar o *hellomvc.conf* arquivo para habilitar a regravação de URL e proteger a comunicação na porta 443:</span><span class="sxs-lookup"><span data-stu-id="ed375-192">Modify the *hellomvc.conf* file to enable URL rewriting and secure communication on port 443:</span></span>

```
<VirtualHost *:80>
    RewriteEngine On
    RewriteCond %{HTTPS} !=on
    RewriteRule ^/?(.*) https://%{SERVER_NAME}/ [R,L]
</VirtualHost>

<VirtualHost *:443>
    ProxyPreserveHost On
    ProxyPass / http://127.0.0.1:5000/
    ProxyPassReverse / http://127.0.0.1:5000/
    ErrorLog /var/log/httpd/hellomvc-error.log
    CustomLog /var/log/httpd/hellomvc-access.log common
    SSLEngine on
    SSLProtocol all -SSLv2
    SSLCipherSuite ALL:!ADH:!EXPORT:!SSLv2:!RC4+RSA:+HIGH:+MEDIUM:!LOW:!RC4
    SSLCertificateFile /etc/pki/tls/certs/localhost.crt
    SSLCertificateKeyFile /etc/pki/tls/private/localhost.key
</VirtualHost>
```

> [!NOTE]
> <span data-ttu-id="ed375-193">Este exemplo está usando um certificado gerado localmente.</span><span class="sxs-lookup"><span data-stu-id="ed375-193">This example is using a locally-generated certificate.</span></span> <span data-ttu-id="ed375-194">**SSLCertificateFile** deve ser o arquivo de certificado primário para o nome de domínio.</span><span class="sxs-lookup"><span data-stu-id="ed375-194">**SSLCertificateFile** should be the primary certificate file for the domain name.</span></span> <span data-ttu-id="ed375-195">**SSLCertificateKeyFile** deve ser o arquivo de chave gerado quando CSR é criado.</span><span class="sxs-lookup"><span data-stu-id="ed375-195">**SSLCertificateKeyFile** should be the key file generated when CSR is created.</span></span> <span data-ttu-id="ed375-196">**SSLCertificateChainFile** deve ser o arquivo de certificado intermediário (se houver) que foi fornecido pela autoridade de certificação.</span><span class="sxs-lookup"><span data-stu-id="ed375-196">**SSLCertificateChainFile** should be the intermediate certificate file (if any) that was supplied by the certificate authority.</span></span>

<span data-ttu-id="ed375-197">Salve o arquivo e a configuração de teste:</span><span class="sxs-lookup"><span data-stu-id="ed375-197">Save the file and test the configuration:</span></span>

```bash
sudo service httpd configtest
```

<span data-ttu-id="ed375-198">Reinicie o Apache:</span><span class="sxs-lookup"><span data-stu-id="ed375-198">Restart Apache:</span></span>

```bash
sudo systemctl restart httpd
```

## <a name="additional-apache-suggestions"></a><span data-ttu-id="ed375-199">Sugestões adicionais do Apache</span><span class="sxs-lookup"><span data-stu-id="ed375-199">Additional Apache suggestions</span></span>

### <a name="additional-headers"></a><span data-ttu-id="ed375-200">Cabeçalhos adicionais</span><span class="sxs-lookup"><span data-stu-id="ed375-200">Additional headers</span></span>

<span data-ttu-id="ed375-201">Para proteger contra ataques mal-intencionados, há alguns cabeçalhos que devem ser modificados ou adicionados.</span><span class="sxs-lookup"><span data-stu-id="ed375-201">In order to secure against malicious attacks, there are a few headers that should either be modified or added.</span></span> <span data-ttu-id="ed375-202">Certifique-se de que o `mod_headers` módulo está instalado:</span><span class="sxs-lookup"><span data-stu-id="ed375-202">Ensure that the `mod_headers` module is installed:</span></span>

```bash
sudo yum install mod_headers
```

#### <a name="secure-apache-from-clickjacking-attacks"></a><span data-ttu-id="ed375-203">Apache seguro contra ataques de clickjacking</span><span class="sxs-lookup"><span data-stu-id="ed375-203">Secure Apache from clickjacking attacks</span></span>

<span data-ttu-id="ed375-204">[Clickjacking](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), também conhecido como um *UI redress ataque*, é um ataque mal-intencionado em que o visitante do site seja levado a clicar em um link ou botão em uma página diferente daquele que está visitando atualmente.</span><span class="sxs-lookup"><span data-stu-id="ed375-204">[Clickjacking](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), also known as a *UI redress attack*, is a malicious attack where a website visitor is tricked into clicking a link or button on a different page than they're currently visiting.</span></span> <span data-ttu-id="ed375-205">Use `X-FRAME-OPTIONS` para proteger o site.</span><span class="sxs-lookup"><span data-stu-id="ed375-205">Use `X-FRAME-OPTIONS` to secure the site.</span></span>

<span data-ttu-id="ed375-206">Editar o *httpd* arquivo:</span><span class="sxs-lookup"><span data-stu-id="ed375-206">Edit the *httpd.conf* file:</span></span>

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="ed375-207">Adicione a linha `Header append X-FRAME-OPTIONS "SAMEORIGIN"`.</span><span class="sxs-lookup"><span data-stu-id="ed375-207">Add the line `Header append X-FRAME-OPTIONS "SAMEORIGIN"`.</span></span> <span data-ttu-id="ed375-208">Salve o arquivo.</span><span class="sxs-lookup"><span data-stu-id="ed375-208">Save the file.</span></span> <span data-ttu-id="ed375-209">Reinicie o Apache.</span><span class="sxs-lookup"><span data-stu-id="ed375-209">Restart Apache.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="ed375-210">Detecção de tipo MIME</span><span class="sxs-lookup"><span data-stu-id="ed375-210">MIME-type sniffing</span></span>

<span data-ttu-id="ed375-211">O `X-Content-Type-Options` cabeçalho impedirá que o Internet Explorer *MIME-sniffing* (determinando um arquivo `Content-Type` do conteúdo do arquivo).</span><span class="sxs-lookup"><span data-stu-id="ed375-211">The `X-Content-Type-Options` header prevents Internet Explorer from *MIME-sniffing* (determining a file's `Content-Type` from the file's content).</span></span> <span data-ttu-id="ed375-212">Se o servidor define a `Content-Type` cabeçalho para `text/html` com o `nosniff` opção definida, o Internet Explorer renderiza o conteúdo como `text/html` , independentemente do conteúdo do arquivo.</span><span class="sxs-lookup"><span data-stu-id="ed375-212">If the server sets the `Content-Type` header to `text/html` with the `nosniff` option set, Internet Explorer renders the content as `text/html` regardless of the file's content.</span></span>

<span data-ttu-id="ed375-213">Editar o *httpd* arquivo:</span><span class="sxs-lookup"><span data-stu-id="ed375-213">Edit the *httpd.conf* file:</span></span>

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="ed375-214">Adicione a linha `Header set X-Content-Type-Options "nosniff"`.</span><span class="sxs-lookup"><span data-stu-id="ed375-214">Add the line `Header set X-Content-Type-Options "nosniff"`.</span></span> <span data-ttu-id="ed375-215">Salve o arquivo.</span><span class="sxs-lookup"><span data-stu-id="ed375-215">Save the file.</span></span> <span data-ttu-id="ed375-216">Reinicie o Apache.</span><span class="sxs-lookup"><span data-stu-id="ed375-216">Restart Apache.</span></span>

### <a name="load-balancing"></a><span data-ttu-id="ed375-217">Balanceamento de carga</span><span class="sxs-lookup"><span data-stu-id="ed375-217">Load Balancing</span></span> 

<span data-ttu-id="ed375-218">Este exemplo mostra como instalar e configurar o Apache no CentOS 7 e no Kestrel no mesmo computador da instância.</span><span class="sxs-lookup"><span data-stu-id="ed375-218">This example shows how to setup and configure Apache on CentOS 7 and Kestrel on the same instance machine.</span></span> <span data-ttu-id="ed375-219">Para não ter um ponto único de falha; usando *mod_proxy_balancer* e modificando o **VirtualHost** permitiria para gerenciar várias instâncias dos aplicativos web por trás do servidor de proxy do Apache.</span><span class="sxs-lookup"><span data-stu-id="ed375-219">In order to not have a single point of failure; using *mod_proxy_balancer* and modifying the **VirtualHost** would allow for managing multiple instances of the web apps behind the Apache proxy server.</span></span>

```bash
sudo yum install mod_proxy_balancer
```

<span data-ttu-id="ed375-220">No arquivo de configuração mostrada abaixo, uma instância adicional do `hellomvc` aplicativo está configurado para ser executado na porta 5001.</span><span class="sxs-lookup"><span data-stu-id="ed375-220">In the configuration file shown below, an additional instance of the `hellomvc` app is setup to run on port 5001.</span></span> <span data-ttu-id="ed375-221">O *Proxy* seção é definida com uma configuração de Balanceador com dois membros para balancear a carga *byrequests*.</span><span class="sxs-lookup"><span data-stu-id="ed375-221">The *Proxy* section is set with a balancer configuration with two members to load balance *byrequests*.</span></span>

```
<VirtualHost *:80>
    RewriteEngine On
    RewriteCond %{HTTPS} !=on
    RewriteRule ^/?(.*) https://%{SERVER_NAME}/ [R,L]
</VirtualHost>

<VirtualHost *:443>
    ProxyPass / balancer://mycluster/ 

    ProxyPassReverse / http://127.0.0.1:5000/
    ProxyPassReverse / http://127.0.0.1:5001/

    <Proxy balancer://mycluster>
        BalancerMember http://127.0.0.1:5000
        BalancerMember http://127.0.0.1:5001 
        ProxySet lbmethod=byrequests
    </Proxy>

    <Location />
        SetHandler balancer
    </Location>
    ErrorLog /var/log/httpd/hellomvc-error.log
    CustomLog /var/log/httpd/hellomvc-access.log common
    SSLEngine on
    SSLProtocol all -SSLv2
    SSLCipherSuite ALL:!ADH:!EXPORT:!SSLv2:!RC4+RSA:+HIGH:+MEDIUM:!LOW:!RC4
    SSLCertificateFile /etc/pki/tls/certs/localhost.crt
    SSLCertificateKeyFile /etc/pki/tls/private/localhost.key
</VirtualHost>
```

### <a name="rate-limits"></a><span data-ttu-id="ed375-222">Limites de taxa</span><span class="sxs-lookup"><span data-stu-id="ed375-222">Rate Limits</span></span>
<span data-ttu-id="ed375-223">Usando *mod_ratelimit*, que está incluído o *httpd* módulo, a largura de banda de clientes pode ser limitada:</span><span class="sxs-lookup"><span data-stu-id="ed375-223">Using *mod_ratelimit*, which is included in the *httpd* module, the bandwidth of clients can be limited:</span></span>

```bash
sudo nano /etc/httpd/conf.d/ratelimit.conf
```
<span data-ttu-id="ed375-224">O arquivo de exemplo limita a largura de banda como 600 KB/s no local da raiz:</span><span class="sxs-lookup"><span data-stu-id="ed375-224">The example file limits bandwidth as 600 KB/sec under the root location:</span></span>

```
<IfModule mod_ratelimit.c>
    <Location />
        SetOutputFilter RATE_LIMIT
        SetEnv rate-limit 600
    </Location>
</IfModule>
```

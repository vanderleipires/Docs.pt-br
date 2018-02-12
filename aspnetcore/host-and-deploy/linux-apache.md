---
title: Hospedar o ASP.NET Core no Linux com o Apache
description: "Saiba como configurar o Apache como um servidor de proxy reverso CentOS para redirecionar o tráfego HTTP para um aplicativo web do ASP.NET Core em execução no Kestrel."
author: spboyer
manager: wpickett
ms.author: spboyer
ms.custom: mvc
ms.date: 10/19/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/linux-apache
ms.openlocfilehash: 61827f456ba01ffa726f3446401156409b29111d
ms.sourcegitcommit: b83a5f731a9c02bdb1cc1e3f9a8bf273eb5b33e0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/11/2018
---
# <a name="host-aspnet-core-on-linux-with-apache"></a><span data-ttu-id="319f6-103">Hospedar o ASP.NET Core no Linux com o Apache</span><span class="sxs-lookup"><span data-stu-id="319f6-103">Host ASP.NET Core on Linux with Apache</span></span>

<span data-ttu-id="319f6-104">Por [Shayne Boyer](https://github.com/spboyer)</span><span class="sxs-lookup"><span data-stu-id="319f6-104">By [Shayne Boyer](https://github.com/spboyer)</span></span>

<span data-ttu-id="319f6-105">Usando este guia, saiba como configurar [Apache](https://httpd.apache.org/) como um servidor de proxy reverso na [CentOS 7](https://www.centos.org/) para redirecionar o tráfego de HTTP para um aplicativo web do ASP.NET Core em execução no [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="319f6-105">Using this guide, learn how to set up [Apache](https://httpd.apache.org/) as a reverse proxy server on [CentOS 7](https://www.centos.org/) to redirect HTTP traffic to an ASP.NET Core web app running on [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="319f6-106">O [mod_proxy extensão](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html) e módulos relacionados Criar proxy reverso do servidor.</span><span class="sxs-lookup"><span data-stu-id="319f6-106">The [mod_proxy extension](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html) and related modules create the server's reverse proxy.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="319f6-107">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="319f6-107">Prerequisites</span></span>

1. <span data-ttu-id="319f6-108">Servidor que executa o CentOS 7 com uma conta de usuário padrão com privilégios sudo</span><span class="sxs-lookup"><span data-stu-id="319f6-108">Server running CentOS 7 with a standard user account with sudo privilege</span></span>
2. <span data-ttu-id="319f6-109">Aplicativo do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="319f6-109">ASP.NET Core app</span></span>

## <a name="publish-the-app"></a><span data-ttu-id="319f6-110">Publique o aplicativo</span><span class="sxs-lookup"><span data-stu-id="319f6-110">Publish the app</span></span>

<span data-ttu-id="319f6-111">Publicar o aplicativo como um [implantação autossuficiente](/dotnet/core/deploying/#self-contained-deployments-scd) na configuração de versão para o tempo de execução do CentOS 7 (`centos.7-x64`).</span><span class="sxs-lookup"><span data-stu-id="319f6-111">Publish the app as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) in Release configuration for the CentOS 7 runtime (`centos.7-x64`).</span></span> <span data-ttu-id="319f6-112">Copie o conteúdo de *bin/Release/netcoreapp2.0/centos.7-x64/publish* pasta ao servidor usando o SCP, FTP ou outro método de transferência de arquivo.</span><span class="sxs-lookup"><span data-stu-id="319f6-112">Copy the contents of the *bin/Release/netcoreapp2.0/centos.7-x64/publish* folder to the server using SCP, FTP, or other file transfer method.</span></span>

> [!NOTE]
> <span data-ttu-id="319f6-113">Em um cenário de implantação de produção, um fluxo de trabalho de integração contínua faz o trabalho de publicação do aplicativo e copiar os ativos para o servidor.</span><span class="sxs-lookup"><span data-stu-id="319f6-113">Under a production deployment scenario, a continuous integration workflow does the work of publishing the app and copying the assets to the server.</span></span> 

## <a name="configure-a-proxy-server"></a><span data-ttu-id="319f6-114">Configurar um servidor proxy</span><span class="sxs-lookup"><span data-stu-id="319f6-114">Configure a proxy server</span></span>

<span data-ttu-id="319f6-115">Um proxy reverso é uma instalação comum para que serve a aplicativos web dinâmicos.</span><span class="sxs-lookup"><span data-stu-id="319f6-115">A reverse proxy is a common setup for serving dynamic web apps.</span></span> <span data-ttu-id="319f6-116">O proxy inverso encerra a solicitação HTTP e as encaminha para o aplicativo ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="319f6-116">The reverse proxy terminates the HTTP request and forwards it to the ASP.NET app.</span></span>

<span data-ttu-id="319f6-117">Um servidor proxy é uma que encaminha as solicitações de cliente para outro servidor, em vez de atender às solicitações em si.</span><span class="sxs-lookup"><span data-stu-id="319f6-117">A proxy server is one which forwards client requests to another server instead of fulfilling requests itself.</span></span> <span data-ttu-id="319f6-118">Um proxy reverso encaminha para um destino fixo, normalmente, em nome de clientes arbitrários.</span><span class="sxs-lookup"><span data-stu-id="319f6-118">A reverse proxy forwards to a fixed destination, typically on behalf of arbitrary clients.</span></span> <span data-ttu-id="319f6-119">Neste guia, Apache é configurado como o proxy inverso em execução no mesmo servidor que Kestrel está atendendo o aplicativo ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="319f6-119">In this guide, Apache is configured as the reverse proxy running on the same server that Kestrel is serving the ASP.NET Core app.</span></span>

<span data-ttu-id="319f6-120">Como as solicitações são encaminhadas pelo proxy reverso, use o Middleware de cabeçalhos encaminhados do [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) pacote.</span><span class="sxs-lookup"><span data-stu-id="319f6-120">Because requests are forwarded by reverse proxy, use the Forwarded Headers Middleware from the [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) package.</span></span> <span data-ttu-id="319f6-121">As atualizações de middleware de `Request.Scheme`, usando o `X-Forwarded-Proto` cabeçalho, de forma que URIs de redirecionamento e outras políticas de segurança funcionam corretamente.</span><span class="sxs-lookup"><span data-stu-id="319f6-121">The middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header, so that redirect URIs and other security policies work correctly.</span></span>

<span data-ttu-id="319f6-122">Ao usar qualquer tipo de middleware de autenticação, o Middleware de cabeçalhos encaminhado deve ser executado primeiro.</span><span class="sxs-lookup"><span data-stu-id="319f6-122">When using any type of authentication middleware, the Forwarded Headers Middleware must run first.</span></span> <span data-ttu-id="319f6-123">Essa ordenação garantirá que o middleware de autenticação pode consumir os valores de cabeçalho e gerar URIs de redirecionamento correto.</span><span class="sxs-lookup"><span data-stu-id="319f6-123">This ordering ensures that the authentication middleware can consume the header values and generate correct redirect URIs.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="319f6-124">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="319f6-124">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="319f6-125">Invocar o [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) método `Startup.Configure` antes de chamar [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) ou semelhante middleware de esquema de autenticação:</span><span class="sxs-lookup"><span data-stu-id="319f6-125">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) or similar authentication scheme middleware:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="319f6-126">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="319f6-126">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="319f6-127">Invocar o [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) método `Startup.Configure` antes de chamar [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) e [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) ou esquema de autenticação semelhantes middleware:</span><span class="sxs-lookup"><span data-stu-id="319f6-127">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) and [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) or similar authentication scheme middleware:</span></span>

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

<span data-ttu-id="319f6-128">Se nenhum [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) são especificados para o middleware, os cabeçalhos padrão para encaminhar são `None`.</span><span class="sxs-lookup"><span data-stu-id="319f6-128">If no [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) are specified to the middleware, the default headers to forward are `None`.</span></span>

### <a name="install-apache"></a><span data-ttu-id="319f6-129">Instalar o Apache</span><span class="sxs-lookup"><span data-stu-id="319f6-129">Install Apache</span></span>

<span data-ttu-id="319f6-130">CentOS pacotes de atualização de suas versões estável mais recente:</span><span class="sxs-lookup"><span data-stu-id="319f6-130">Update CentOS packages to their latest stable versions:</span></span>

```bash
sudo yum update -y
```

<span data-ttu-id="319f6-131">Instalar o servidor web Apache em CentOS com um único `yum` comando:</span><span class="sxs-lookup"><span data-stu-id="319f6-131">Install the Apache web server on CentOS with a single `yum` command:</span></span>

```bash
sudo yum -y install httpd mod_ssl
```

<span data-ttu-id="319f6-132">Exemplo de saída depois de executar o comando:</span><span class="sxs-lookup"><span data-stu-id="319f6-132">Sample output after running the command:</span></span>

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
> <span data-ttu-id="319f6-133">Neste exemplo, a saída reflete httpd.86_64 desde a versão CentOS 7 é de 64 bits.</span><span class="sxs-lookup"><span data-stu-id="319f6-133">In this example, the output reflects httpd.86_64 since the CentOS 7 version is 64 bit.</span></span> <span data-ttu-id="319f6-134">Para verificar o local em que o Apache está instalado, execute `whereis httpd` em um prompt de comando.</span><span class="sxs-lookup"><span data-stu-id="319f6-134">To verify where Apache is installed, run `whereis httpd` from a command prompt.</span></span> 

### <a name="configure-apache-for-reverse-proxy"></a><span data-ttu-id="319f6-135">Configurar o Apache para proxy reverso</span><span class="sxs-lookup"><span data-stu-id="319f6-135">Configure Apache for reverse proxy</span></span>

<span data-ttu-id="319f6-136">Os arquivos de configuração do Apache estão localizados no diretório `/etc/httpd/conf.d/`.</span><span class="sxs-lookup"><span data-stu-id="319f6-136">Configuration files for Apache are located within the `/etc/httpd/conf.d/` directory.</span></span> <span data-ttu-id="319f6-137">Qualquer arquivo com o *.conf* extensão é processada em ordem alfabética, além dos arquivos de configuração do módulo em `/etc/httpd/conf.modules.d/`, que contém todas as configurações de arquivos necessário para carregar módulos.</span><span class="sxs-lookup"><span data-stu-id="319f6-137">Any file with the *.conf* extension is processed in alphabetical order in addition to the module configuration files in `/etc/httpd/conf.modules.d/`, which contains any configuration files necessary to load modules.</span></span>

<span data-ttu-id="319f6-138">Criar um arquivo de configuração para o aplicativo chamado `hellomvc.conf`:</span><span class="sxs-lookup"><span data-stu-id="319f6-138">Create a configuration file for the app named `hellomvc.conf`:</span></span>

```
<VirtualHost *:80>
    ProxyPreserveHost On
    ProxyPass / http://127.0.0.1:5000/
    ProxyPassReverse / http://127.0.0.1:5000/
    ErrorLog /var/log/httpd/hellomvc-error.log
    CustomLog /var/log/httpd/hellomvc-access.log common
</VirtualHost>
```

<span data-ttu-id="319f6-139">O **VirtualHost** nó pode aparecer várias vezes em um ou mais arquivos em um servidor.</span><span class="sxs-lookup"><span data-stu-id="319f6-139">The **VirtualHost** node can appear multiple times in one or more files on a server.</span></span> <span data-ttu-id="319f6-140">**VirtualHost** está definido para escutar em qualquer endereço IP usando a porta 80.</span><span class="sxs-lookup"><span data-stu-id="319f6-140">**VirtualHost** is set to listen on any IP address using port 80.</span></span> <span data-ttu-id="319f6-141">As duas linhas são definidas para solicitações de proxy na raiz do servidor no 127.0.0.1 na porta 5000.</span><span class="sxs-lookup"><span data-stu-id="319f6-141">The next two lines are set to proxy requests at the root to the server at 127.0.0.1 on port 5000.</span></span> <span data-ttu-id="319f6-142">Para a comunicação bidirecional, *ProxyPass* e *ProxyPassReverse* são necessários.</span><span class="sxs-lookup"><span data-stu-id="319f6-142">For bi-directional communication, *ProxyPass* and *ProxyPassReverse* are required.</span></span>

<span data-ttu-id="319f6-143">Registro em log pode ser configurado por **VirtualHost** usando **ErrorLog** e **CustomLog** diretivas.</span><span class="sxs-lookup"><span data-stu-id="319f6-143">Logging can be configured per **VirtualHost** using **ErrorLog** and **CustomLog** directives.</span></span> <span data-ttu-id="319f6-144">**Log de erros** é o local onde o servidor registra erros, e **CustomLog** define o nome de arquivo e o formato do arquivo de log.</span><span class="sxs-lookup"><span data-stu-id="319f6-144">**ErrorLog** is the location where the server logs errors, and **CustomLog** sets the filename and format of log file.</span></span> <span data-ttu-id="319f6-145">Nesse caso, isso é onde as informações de solicitação são registradas.</span><span class="sxs-lookup"><span data-stu-id="319f6-145">In this case, this is where request information is logged.</span></span> <span data-ttu-id="319f6-146">Há uma linha para cada solicitação.</span><span class="sxs-lookup"><span data-stu-id="319f6-146">There's one line for each request.</span></span>

<span data-ttu-id="319f6-147">Salve o arquivo e testar a configuração.</span><span class="sxs-lookup"><span data-stu-id="319f6-147">Save the file and test the configuration.</span></span> <span data-ttu-id="319f6-148">Se tudo passar, a resposta deverá ser `Syntax [OK]`.</span><span class="sxs-lookup"><span data-stu-id="319f6-148">If everything passes, the response should be `Syntax [OK]`.</span></span>

```bash
sudo service httpd configtest
```

<span data-ttu-id="319f6-149">Reinicie o Apache:</span><span class="sxs-lookup"><span data-stu-id="319f6-149">Restart Apache:</span></span>

```bash
sudo systemctl restart httpd
sudo systemctl enable httpd
```

## <a name="monitoring-the-app"></a><span data-ttu-id="319f6-150">Monitoramento do aplicativo</span><span class="sxs-lookup"><span data-stu-id="319f6-150">Monitoring the app</span></span>

<span data-ttu-id="319f6-151">Apache agora está configurado para encaminhar solicitações feitas a `http://localhost:80` para o aplicativo ASP.NET Core em execução em Kestrel em `http://127.0.0.1:5000`.</span><span class="sxs-lookup"><span data-stu-id="319f6-151">Apache is now setup to forward requests made to `http://localhost:80` to the ASP.NET Core app running on Kestrel at `http://127.0.0.1:5000`.</span></span>  <span data-ttu-id="319f6-152">No entanto, o Apache não está configurado para gerenciar o processo de Kestrel.</span><span class="sxs-lookup"><span data-stu-id="319f6-152">However, Apache isn't set up to manage the Kestrel process.</span></span> <span data-ttu-id="319f6-153">Use *systemd* e crie um arquivo de serviço para iniciar e monitorar o aplicativo web subjacente.</span><span class="sxs-lookup"><span data-stu-id="319f6-153">Use *systemd* and create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="319f6-154">*systemd* é um sistema de inicialização que fornece muitos recursos poderosos para iniciar, parar e gerenciar processos.</span><span class="sxs-lookup"><span data-stu-id="319f6-154">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span> 


### <a name="create-the-service-file"></a><span data-ttu-id="319f6-155">Criar o arquivo de serviço</span><span class="sxs-lookup"><span data-stu-id="319f6-155">Create the service file</span></span>

<span data-ttu-id="319f6-156">Crie o arquivo de definição de serviço:</span><span class="sxs-lookup"><span data-stu-id="319f6-156">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

<span data-ttu-id="319f6-157">Um arquivo de serviço de exemplo para o aplicativo:</span><span class="sxs-lookup"><span data-stu-id="319f6-157">An example service file for the app:</span></span>

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
> <span data-ttu-id="319f6-158">**Usuário** &mdash; se o usuário *apache* não é usado pela configuração, o usuário deve criado pela primeira vez e determinado propriedade adequada de arquivos.</span><span class="sxs-lookup"><span data-stu-id="319f6-158">**User** &mdash; If the user *apache* isn't used by the configuration, the user must be created first and given proper ownership for files.</span></span>

<span data-ttu-id="319f6-159">Salve o arquivo e habilitar o serviço:</span><span class="sxs-lookup"><span data-stu-id="319f6-159">Save the file and enable the service:</span></span>

```bash
systemctl enable kestrel-hellomvc.service
```

<span data-ttu-id="319f6-160">Inicie o serviço e verifique se ele está em execução:</span><span class="sxs-lookup"><span data-stu-id="319f6-160">Start the service and verify that it's running:</span></span>

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

<span data-ttu-id="319f6-161">Com o proxy reverso configurado e gerenciadas por meio de Kestrel *systemd*, o aplicativo web está totalmente configurado e pode ser acessado a partir de um navegador no computador local em `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="319f6-161">With the reverse proxy configured and Kestrel managed through *systemd*, the web app is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="319f6-162">Inspecionando os cabeçalhos de resposta, o **Server** cabeçalho indica que o aplicativo ASP.NET Core é atendido por Kestrel:</span><span class="sxs-lookup"><span data-stu-id="319f6-162">Inspecting the response headers, the **Server** header indicates that the ASP.NET Core app is served by Kestrel:</span></span>

```
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a><span data-ttu-id="319f6-163">Exibindo logs</span><span class="sxs-lookup"><span data-stu-id="319f6-163">Viewing logs</span></span>

<span data-ttu-id="319f6-164">Desde que o aplicativo web usar Kestrel é gerenciada usando *systemd*, eventos e os processos são registrados para um diário centralizado.</span><span class="sxs-lookup"><span data-stu-id="319f6-164">Since the web app using Kestrel is managed using *systemd*, events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="319f6-165">No entanto, esse diário contém entradas para todos os serviços e processos gerenciados pelo *systemd*.</span><span class="sxs-lookup"><span data-stu-id="319f6-165">However, this journal includes entries for all of the services and processes managed by *systemd*.</span></span> <span data-ttu-id="319f6-166">Para exibir os itens específicos de `kestrel-hellomvc.service`, use o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="319f6-166">To view the `kestrel-hellomvc.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

<span data-ttu-id="319f6-167">Para filtragem de hora, especifica opções de tempo com o comando.</span><span class="sxs-lookup"><span data-stu-id="319f6-167">For time filtering, specify time options with the command.</span></span> <span data-ttu-id="319f6-168">Por exemplo, use `--since today` para filtrar o dia atual ou `--until 1 hour ago` para ver as entradas da hora anterior.</span><span class="sxs-lookup"><span data-stu-id="319f6-168">For example, use `--since today` to filter for the current day or `--until 1 hour ago` to see the previous hour's entries.</span></span> <span data-ttu-id="319f6-169">Para obter mais informações, consulte o [página para journalctl](https://www.unix.com/man-page/centos/1/journalctl/).</span><span class="sxs-lookup"><span data-stu-id="319f6-169">For more information, see the [man page for journalctl](https://www.unix.com/man-page/centos/1/journalctl/).</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-the-app"></a><span data-ttu-id="319f6-170">Protegendo o aplicativo</span><span class="sxs-lookup"><span data-stu-id="319f6-170">Securing the app</span></span>

### <a name="configure-firewall"></a><span data-ttu-id="319f6-171">Configurar o firewall</span><span class="sxs-lookup"><span data-stu-id="319f6-171">Configure firewall</span></span>

<span data-ttu-id="319f6-172">*Firewalld* é um daemon dinâmico para gerenciar o firewall com suporte para zonas de rede.</span><span class="sxs-lookup"><span data-stu-id="319f6-172">*Firewalld* is a dynamic daemon to manage the firewall with support for network zones.</span></span> <span data-ttu-id="319f6-173">Portas e filtragem de pacote ainda podem ser gerenciados pelo iptables.</span><span class="sxs-lookup"><span data-stu-id="319f6-173">Ports and packet filtering can still be managed by iptables.</span></span> <span data-ttu-id="319f6-174">*Firewalld* devem ser instalados por padrão.</span><span class="sxs-lookup"><span data-stu-id="319f6-174">*Firewalld* should be installed by default.</span></span> <span data-ttu-id="319f6-175">`yum`pode ser usado para instalar o pacote ou verifique se que ele está instalado.</span><span class="sxs-lookup"><span data-stu-id="319f6-175">`yum` can be used to install the package or verify it's installed.</span></span>

```bash
sudo yum install firewalld -y
```

<span data-ttu-id="319f6-176">Use `firewalld` para abrir somente as portas necessárias para o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="319f6-176">Use `firewalld` to open only the ports needed for the app.</span></span> <span data-ttu-id="319f6-177">Nesse caso, as portas 80 e 443 são usadas.</span><span class="sxs-lookup"><span data-stu-id="319f6-177">In this case, port 80 and 443 are used.</span></span> <span data-ttu-id="319f6-178">Os comandos a seguir definem permanentemente as portas 80 e 443 para abrir:</span><span class="sxs-lookup"><span data-stu-id="319f6-178">The following commands permanently set ports 80 and 443 to open:</span></span>

```bash
sudo firewall-cmd --add-port=80/tcp --permanent
sudo firewall-cmd --add-port=443/tcp --permanent
```

<span data-ttu-id="319f6-179">Recarrega as configurações de firewall.</span><span class="sxs-lookup"><span data-stu-id="319f6-179">Reload the firewall settings.</span></span> <span data-ttu-id="319f6-180">Verifique os serviços disponíveis e as portas na zona padrão.</span><span class="sxs-lookup"><span data-stu-id="319f6-180">Check the available services and ports in the default zone.</span></span> <span data-ttu-id="319f6-181">Opções estão disponíveis por meio da inspeção `firewall-cmd -h`.</span><span class="sxs-lookup"><span data-stu-id="319f6-181">Options are available by inspecting `firewall-cmd -h`.</span></span>

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

### <a name="ssl-configuration"></a><span data-ttu-id="319f6-182">Configuração do SSL</span><span class="sxs-lookup"><span data-stu-id="319f6-182">SSL configuration</span></span>

<span data-ttu-id="319f6-183">Para configurar o Apache para SSL, o *mod_ssl* módulo é usado.</span><span class="sxs-lookup"><span data-stu-id="319f6-183">To configure Apache for SSL, the *mod_ssl* module is used.</span></span> <span data-ttu-id="319f6-184">Quando o *httpd* módulo foi instalado, o *mod_ssl* módulo também foi instalado.</span><span class="sxs-lookup"><span data-stu-id="319f6-184">When the *httpd* module was installed, the *mod_ssl* module was also installed.</span></span> <span data-ttu-id="319f6-185">Se ele não foi instalado, use `yum` para adicioná-lo para a configuração.</span><span class="sxs-lookup"><span data-stu-id="319f6-185">If it wasn't installed, use `yum` to add it to the configuration.</span></span>

```bash
sudo yum install mod_ssl
```
<span data-ttu-id="319f6-186">Para impor a SSL, instale o `mod_rewrite` módulo para habilitar a regravação de URL:</span><span class="sxs-lookup"><span data-stu-id="319f6-186">To enforce SSL, install the `mod_rewrite` module to enable URL rewriting:</span></span>

```bash
sudo yum install mod_rewrite
```

<span data-ttu-id="319f6-187">Modificar o *hellomvc.conf* arquivo para habilitar a regravação de URL e proteger a comunicação na porta 443:</span><span class="sxs-lookup"><span data-stu-id="319f6-187">Modify the *hellomvc.conf* file to enable URL rewriting and secure communication on port 443:</span></span>

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
> <span data-ttu-id="319f6-188">Este exemplo está usando um certificado gerado localmente.</span><span class="sxs-lookup"><span data-stu-id="319f6-188">This example is using a locally-generated certificate.</span></span> <span data-ttu-id="319f6-189">**SSLCertificateFile** deve ser o arquivo de certificado primário para o nome de domínio.</span><span class="sxs-lookup"><span data-stu-id="319f6-189">**SSLCertificateFile** should be the primary certificate file for the domain name.</span></span> <span data-ttu-id="319f6-190">**SSLCertificateKeyFile** deve ser o arquivo de chave gerado quando CSR é criado.</span><span class="sxs-lookup"><span data-stu-id="319f6-190">**SSLCertificateKeyFile** should be the key file generated when CSR is created.</span></span> <span data-ttu-id="319f6-191">**SSLCertificateChainFile** deve ser o arquivo de certificado intermediário (se houver) que foi fornecido pela autoridade de certificação.</span><span class="sxs-lookup"><span data-stu-id="319f6-191">**SSLCertificateChainFile** should be the intermediate certificate file (if any) that was supplied by the certificate authority.</span></span>

<span data-ttu-id="319f6-192">Salve o arquivo e a configuração de teste:</span><span class="sxs-lookup"><span data-stu-id="319f6-192">Save the file and test the configuration:</span></span>

```bash
sudo service httpd configtest
```

<span data-ttu-id="319f6-193">Reinicie o Apache:</span><span class="sxs-lookup"><span data-stu-id="319f6-193">Restart Apache:</span></span>

```bash
sudo systemctl restart httpd
```

## <a name="additional-apache-suggestions"></a><span data-ttu-id="319f6-194">Sugestões adicionais do Apache</span><span class="sxs-lookup"><span data-stu-id="319f6-194">Additional Apache suggestions</span></span>

### <a name="additional-headers"></a><span data-ttu-id="319f6-195">Cabeçalhos adicionais</span><span class="sxs-lookup"><span data-stu-id="319f6-195">Additional headers</span></span>

<span data-ttu-id="319f6-196">Para proteger contra ataques mal-intencionados, há alguns cabeçalhos que devem ser modificados ou adicionados.</span><span class="sxs-lookup"><span data-stu-id="319f6-196">In order to secure against malicious attacks, there are a few headers that should either be modified or added.</span></span> <span data-ttu-id="319f6-197">Certifique-se de que o `mod_headers` módulo está instalado:</span><span class="sxs-lookup"><span data-stu-id="319f6-197">Ensure that the `mod_headers` module is installed:</span></span>

```bash
sudo yum install mod_headers
```

#### <a name="secure-apache-from-clickjacking-attacks"></a><span data-ttu-id="319f6-198">Apache seguro contra ataques de clickjacking</span><span class="sxs-lookup"><span data-stu-id="319f6-198">Secure Apache from clickjacking attacks</span></span>

<span data-ttu-id="319f6-199">[Clickjacking](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), também conhecido como um *UI redress ataque*, é um ataque mal-intencionado em que o visitante do site seja levado a clicar em um link ou botão em uma página diferente daquele que está visitando atualmente.</span><span class="sxs-lookup"><span data-stu-id="319f6-199">[Clickjacking](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), also known as a *UI redress attack*, is a malicious attack where a website visitor is tricked into clicking a link or button on a different page than they're currently visiting.</span></span> <span data-ttu-id="319f6-200">Use `X-FRAME-OPTIONS` para proteger o site.</span><span class="sxs-lookup"><span data-stu-id="319f6-200">Use `X-FRAME-OPTIONS` to secure the site.</span></span>

<span data-ttu-id="319f6-201">Editar o *httpd* arquivo:</span><span class="sxs-lookup"><span data-stu-id="319f6-201">Edit the *httpd.conf* file:</span></span>

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="319f6-202">Adicione a linha `Header append X-FRAME-OPTIONS "SAMEORIGIN"`.</span><span class="sxs-lookup"><span data-stu-id="319f6-202">Add the line `Header append X-FRAME-OPTIONS "SAMEORIGIN"`.</span></span> <span data-ttu-id="319f6-203">Salve o arquivo.</span><span class="sxs-lookup"><span data-stu-id="319f6-203">Save the file.</span></span> <span data-ttu-id="319f6-204">Reinicie o Apache.</span><span class="sxs-lookup"><span data-stu-id="319f6-204">Restart Apache.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="319f6-205">Detecção de tipo MIME</span><span class="sxs-lookup"><span data-stu-id="319f6-205">MIME-type sniffing</span></span>

<span data-ttu-id="319f6-206">O `X-Content-Type-Options` cabeçalho impedirá que o Internet Explorer *MIME-sniffing* (determinando um arquivo `Content-Type` do conteúdo do arquivo).</span><span class="sxs-lookup"><span data-stu-id="319f6-206">The `X-Content-Type-Options` header prevents Internet Explorer from *MIME-sniffing* (determing a file's `Content-Type` from the file's content).</span></span> <span data-ttu-id="319f6-207">Se o servidor define a `Content-Type` cabeçalho para `text/html` com o `nosniff` opção definida, o Internet Explorer renderiza o conteúdo como `text/html` , independentemente do conteúdo do arquivo.</span><span class="sxs-lookup"><span data-stu-id="319f6-207">If the server sets the `Content-Type` header to `text/html` with the `nosniff` option set, Internet Explorer renders the content as `text/html` regardless of the file's content.</span></span>

<span data-ttu-id="319f6-208">Editar o *httpd* arquivo:</span><span class="sxs-lookup"><span data-stu-id="319f6-208">Edit the *httpd.conf* file:</span></span>

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="319f6-209">Adicione a linha `Header set X-Content-Type-Options "nosniff"`.</span><span class="sxs-lookup"><span data-stu-id="319f6-209">Add the line `Header set X-Content-Type-Options "nosniff"`.</span></span> <span data-ttu-id="319f6-210">Salve o arquivo.</span><span class="sxs-lookup"><span data-stu-id="319f6-210">Save the file.</span></span> <span data-ttu-id="319f6-211">Reinicie o Apache.</span><span class="sxs-lookup"><span data-stu-id="319f6-211">Restart Apache.</span></span>

### <a name="load-balancing"></a><span data-ttu-id="319f6-212">Balanceamento de carga</span><span class="sxs-lookup"><span data-stu-id="319f6-212">Load Balancing</span></span> 

<span data-ttu-id="319f6-213">Este exemplo mostra como instalar e configurar o Apache no CentOS 7 e no Kestrel no mesmo computador da instância.</span><span class="sxs-lookup"><span data-stu-id="319f6-213">This example shows how to setup and configure Apache on CentOS 7 and Kestrel on the same instance machine.</span></span> <span data-ttu-id="319f6-214">Para não ter um ponto único de falha; usando *mod_proxy_balancer* e modificando o **VirtualHost** permitiria para gerenciar instâncias de vários aplicativos web por trás do servidor de proxy do Apache.</span><span class="sxs-lookup"><span data-stu-id="319f6-214">In order to not have a single point of failure; using *mod_proxy_balancer* and modifying the **VirtualHost** would allow for managing mutliple instances of the web apps behind the Apache proxy server.</span></span>

```bash
sudo yum install mod_proxy_balancer
```

<span data-ttu-id="319f6-215">No arquivo de configuração mostrada abaixo, uma instância adicional do `hellomvc` aplicativo está configurado para ser executado na porta 5001.</span><span class="sxs-lookup"><span data-stu-id="319f6-215">In the configuration file shown below, an additional instance of the `hellomvc` app is setup to run on port 5001.</span></span> <span data-ttu-id="319f6-216">O *Proxy* seção é definida com uma configuração de Balanceador com dois membros para balancear a carga *byrequests*.</span><span class="sxs-lookup"><span data-stu-id="319f6-216">The *Proxy* section is set with a balancer configuration with two members to load balance *byrequests*.</span></span>

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

### <a name="rate-limits"></a><span data-ttu-id="319f6-217">Limites de taxa</span><span class="sxs-lookup"><span data-stu-id="319f6-217">Rate Limits</span></span>
<span data-ttu-id="319f6-218">Usando *mod_ratelimit*, que está incluído o *httpd* módulo, a largura de banda de clientes pode ser limitada:</span><span class="sxs-lookup"><span data-stu-id="319f6-218">Using *mod_ratelimit*, which is included in the *httpd* module, the bandwidth of clients can be limited:</span></span>

```bash
sudo nano /etc/httpd/conf.d/ratelimit.conf
```
<span data-ttu-id="319f6-219">O arquivo de exemplo limita a largura de banda como 600 KB/s no local da raiz:</span><span class="sxs-lookup"><span data-stu-id="319f6-219">The example file limits bandwidth as 600 KB/sec under the root location:</span></span>

```
<IfModule mod_ratelimit.c>
    <Location />
        SetOutputFilter RATE_LIMIT
        SetEnv rate-limit 600
    </Location>
</IfModule>
```

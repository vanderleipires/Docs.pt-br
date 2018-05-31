---
title: Hospedar o ASP.NET Core no Linux com o Apache
description: Saiba como configurar o Apache como um servidor proxy reverso no CentOS para redirecionar o tráfego HTTP para um aplicativo do Web ASP.NET Core em execução no Kestrel.
author: spboyer
manager: wpickett
ms.author: spboyer
ms.custom: mvc
ms.date: 03/13/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/linux-apache
ms.openlocfilehash: 473585f1be180645395c14a154c9c017ca50edab
ms.sourcegitcommit: 74be78285ea88772e7dad112f80146b6ed00e53e
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/10/2018
ms.locfileid: "33962811"
---
# <a name="host-aspnet-core-on-linux-with-apache"></a><span data-ttu-id="cefe4-103">Hospedar o ASP.NET Core no Linux com o Apache</span><span class="sxs-lookup"><span data-stu-id="cefe4-103">Host ASP.NET Core on Linux with Apache</span></span>

<span data-ttu-id="cefe4-104">Por [Shayne Boyer](https://github.com/spboyer)</span><span class="sxs-lookup"><span data-stu-id="cefe4-104">By [Shayne Boyer](https://github.com/spboyer)</span></span>

<span data-ttu-id="cefe4-105">Usando este guia, aprenda como configurar o [Apache](https://httpd.apache.org/) como um servidor proxy reverso no [CentOS 7](https://www.centos.org/) para redirecionar o tráfego HTTP para um aplicativo Web ASP.NET Core em execução no [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="cefe4-105">Using this guide, learn how to set up [Apache](https://httpd.apache.org/) as a reverse proxy server on [CentOS 7](https://www.centos.org/) to redirect HTTP traffic to an ASP.NET Core web app running on [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="cefe4-106">A [extensão mod_proxy](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html) e os módulos relacionados criam o proxy reverso do servidor.</span><span class="sxs-lookup"><span data-stu-id="cefe4-106">The [mod_proxy extension](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html) and related modules create the server's reverse proxy.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cefe4-107">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="cefe4-107">Prerequisites</span></span>

1. <span data-ttu-id="cefe4-108">Um servidor que executa o CentOS 7, com uma conta de usuário padrão com privilégio sudo</span><span class="sxs-lookup"><span data-stu-id="cefe4-108">Server running CentOS 7 with a standard user account with sudo privilege</span></span>
2. <span data-ttu-id="cefe4-109">Aplicativo ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="cefe4-109">ASP.NET Core app</span></span>

## <a name="publish-the-app"></a><span data-ttu-id="cefe4-110">Publique o aplicativo</span><span class="sxs-lookup"><span data-stu-id="cefe4-110">Publish the app</span></span>

<span data-ttu-id="cefe4-111">Publique o aplicativo como uma [implantação autossuficiente](/dotnet/core/deploying/#self-contained-deployments-scd) na Configuração de versão para o tempo de execução do CentOS 7 (`centos.7-x64`).</span><span class="sxs-lookup"><span data-stu-id="cefe4-111">Publish the app as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) in Release configuration for the CentOS 7 runtime (`centos.7-x64`).</span></span> <span data-ttu-id="cefe4-112">Copie o conteúdo da pasta *bin/Release/netcoreapp2.0/centos.7-x64/publish* para o servidor usando SCP, FTP ou outro método de transferência de arquivos.</span><span class="sxs-lookup"><span data-stu-id="cefe4-112">Copy the contents of the *bin/Release/netcoreapp2.0/centos.7-x64/publish* folder to the server using SCP, FTP, or other file transfer method.</span></span>

> [!NOTE]
> <span data-ttu-id="cefe4-113">Em um cenário de implantação de produção, um fluxo de trabalho de integração contínua faz o trabalho de publicar o aplicativo e copiar os ativos para o servidor.</span><span class="sxs-lookup"><span data-stu-id="cefe4-113">Under a production deployment scenario, a continuous integration workflow does the work of publishing the app and copying the assets to the server.</span></span> 

## <a name="configure-a-proxy-server"></a><span data-ttu-id="cefe4-114">Configurar um servidor proxy</span><span class="sxs-lookup"><span data-stu-id="cefe4-114">Configure a proxy server</span></span>

<span data-ttu-id="cefe4-115">Um proxy reverso é uma configuração comum para atender a aplicativos Web dinâmicos.</span><span class="sxs-lookup"><span data-stu-id="cefe4-115">A reverse proxy is a common setup for serving dynamic web apps.</span></span> <span data-ttu-id="cefe4-116">O proxy reverso encerra a solicitação HTTP e a encaminha para o aplicativo ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="cefe4-116">The reverse proxy terminates the HTTP request and forwards it to the ASP.NET app.</span></span>

<span data-ttu-id="cefe4-117">Um servidor proxy é aquele que encaminha as solicitações de cliente para outro servidor, em vez de atendê-las por conta própria.</span><span class="sxs-lookup"><span data-stu-id="cefe4-117">A proxy server is one which forwards client requests to another server instead of fulfilling requests itself.</span></span> <span data-ttu-id="cefe4-118">Um proxy reverso encaminha para um destino fixo, normalmente, em nome de clientes arbitrários.</span><span class="sxs-lookup"><span data-stu-id="cefe4-118">A reverse proxy forwards to a fixed destination, typically on behalf of arbitrary clients.</span></span> <span data-ttu-id="cefe4-119">Neste guia, o Apache é configurado como o proxy reverso em execução no mesmo servidor em que o Kestrel está servindo o aplicativo ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="cefe4-119">In this guide, Apache is configured as the reverse proxy running on the same server that Kestrel is serving the ASP.NET Core app.</span></span>

<span data-ttu-id="cefe4-120">Já que as solicitações são encaminhadas pelo proxy reverso, use o middleware de cabeçalhos encaminhados do pacote [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/).</span><span class="sxs-lookup"><span data-stu-id="cefe4-120">Because requests are forwarded by reverse proxy, use the Forwarded Headers Middleware from the [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) package.</span></span> <span data-ttu-id="cefe4-121">O middleware atualiza o `Request.Scheme` usando o cabeçalho `X-Forwarded-Proto`, de forma que URIs de redirecionamento e outras políticas de segurança funcionam corretamente.</span><span class="sxs-lookup"><span data-stu-id="cefe4-121">The middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header, so that redirect URIs and other security policies work correctly.</span></span>

<span data-ttu-id="cefe4-122">Ao usar qualquer tipo de middleware de autenticação, o Middleware de cabeçalhos encaminhados deve ser executado primeiro.</span><span class="sxs-lookup"><span data-stu-id="cefe4-122">When using any type of authentication middleware, the Forwarded Headers Middleware must run first.</span></span> <span data-ttu-id="cefe4-123">Essa ordenação garantirá que o middleware de autenticação possa consumir os valores de cabeçalho e gerar URIs de redirecionamento corretos.</span><span class="sxs-lookup"><span data-stu-id="cefe4-123">This ordering ensures that the authentication middleware can consume the header values and generate correct redirect URIs.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="cefe4-124">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="cefe4-124">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="cefe4-125">Invoque o método [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) em `Startup.Configure` antes de chamar [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) ou um middleware de esquema de autenticação semelhante.</span><span class="sxs-lookup"><span data-stu-id="cefe4-125">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="cefe4-126">Configure o middleware para encaminhar os cabeçalhos `X-Forwarded-For` e `X-Forwarded-Proto`:</span><span class="sxs-lookup"><span data-stu-id="cefe4-126">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="cefe4-127">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="cefe4-127">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="cefe4-128">Invoque o método [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) em `Startup.Configure` antes de chamar [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) e [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) ou um middleware de esquema de autenticação semelhante.</span><span class="sxs-lookup"><span data-stu-id="cefe4-128">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) and [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="cefe4-129">Configure o middleware para encaminhar os cabeçalhos `X-Forwarded-For` e `X-Forwarded-Proto`:</span><span class="sxs-lookup"><span data-stu-id="cefe4-129">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

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

<span data-ttu-id="cefe4-130">Se nenhum [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) for especificado para o middleware, os cabeçalhos padrão para encaminhar serão `None`.</span><span class="sxs-lookup"><span data-stu-id="cefe4-130">If no [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) are specified to the middleware, the default headers to forward are `None`.</span></span>

<span data-ttu-id="cefe4-131">Configuração adicional pode ser necessária para aplicativos hospedados atrás de servidores proxy e balanceadores de carga.</span><span class="sxs-lookup"><span data-stu-id="cefe4-131">Additional configuration might be required for apps hosted behind proxy servers and load balancers.</span></span> <span data-ttu-id="cefe4-132">Para obter mais informações, veja [Configurar o ASP.NET Core para trabalhar com servidores proxy e balanceadores de carga](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="cefe4-132">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

### <a name="install-apache"></a><span data-ttu-id="cefe4-133">Instalar o Apache</span><span class="sxs-lookup"><span data-stu-id="cefe4-133">Install Apache</span></span>

<span data-ttu-id="cefe4-134">Atualize os pacotes CentOS para as respectivas versões estáveis mais recentes:</span><span class="sxs-lookup"><span data-stu-id="cefe4-134">Update CentOS packages to their latest stable versions:</span></span>

```bash
sudo yum update -y
```

<span data-ttu-id="cefe4-135">Instale o servidor Web do Apache no CentOS com um único comando `yum`:</span><span class="sxs-lookup"><span data-stu-id="cefe4-135">Install the Apache web server on CentOS with a single `yum` command:</span></span>

```bash
sudo yum -y install httpd mod_ssl
```

<span data-ttu-id="cefe4-136">Saída de exemplo depois de executar o comando:</span><span class="sxs-lookup"><span data-stu-id="cefe4-136">Sample output after running the command:</span></span>

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
> <span data-ttu-id="cefe4-137">Neste exemplo, o resultado reflete httpd.86_64, pois a versão do CentOS 7 é de 64 bits.</span><span class="sxs-lookup"><span data-stu-id="cefe4-137">In this example, the output reflects httpd.86_64 since the CentOS 7 version is 64 bit.</span></span> <span data-ttu-id="cefe4-138">Para verificar o local em que o Apache está instalado, execute `whereis httpd` em um prompt de comando.</span><span class="sxs-lookup"><span data-stu-id="cefe4-138">To verify where Apache is installed, run `whereis httpd` from a command prompt.</span></span>

### <a name="configure-apache-for-reverse-proxy"></a><span data-ttu-id="cefe4-139">Configurar o Apache para proxy reverso</span><span class="sxs-lookup"><span data-stu-id="cefe4-139">Configure Apache for reverse proxy</span></span>

<span data-ttu-id="cefe4-140">Os arquivos de configuração do Apache estão localizados no diretório `/etc/httpd/conf.d/`.</span><span class="sxs-lookup"><span data-stu-id="cefe4-140">Configuration files for Apache are located within the `/etc/httpd/conf.d/` directory.</span></span> <span data-ttu-id="cefe4-141">Qualquer arquivo com a extensão *.conf* é processado em ordem alfabética, além dos arquivos de configuração do módulo em `/etc/httpd/conf.modules.d/`, que contém todos os arquivos de configuração necessários para carregar os módulos.</span><span class="sxs-lookup"><span data-stu-id="cefe4-141">Any file with the *.conf* extension is processed in alphabetical order in addition to the module configuration files in `/etc/httpd/conf.modules.d/`, which contains any configuration files necessary to load modules.</span></span>

<span data-ttu-id="cefe4-142">Crie um arquivo de configuração chamado *hellomvc.conf* para o aplicativo:</span><span class="sxs-lookup"><span data-stu-id="cefe4-142">Create a configuration file, named *hellomvc.conf*, for the app:</span></span>

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

<span data-ttu-id="cefe4-143">O bloco `VirtualHost` pode aparecer várias vezes, em um ou mais arquivos em um servidor.</span><span class="sxs-lookup"><span data-stu-id="cefe4-143">The `VirtualHost` block can appear multiple times, in one or more files on a server.</span></span> <span data-ttu-id="cefe4-144">No arquivo de configuração anterior, o Apache aceita tráfego público na porta 80.</span><span class="sxs-lookup"><span data-stu-id="cefe4-144">In the preceding configuration file, Apache accepts public traffic on port 80.</span></span> <span data-ttu-id="cefe4-145">O domínio `www.example.com` está sendo atendido e o alias `*.example.com` é resolvido para o mesmo site.</span><span class="sxs-lookup"><span data-stu-id="cefe4-145">The domain `www.example.com` is being served, and the `*.example.com` alias resolves to the same website.</span></span> <span data-ttu-id="cefe4-146">Veja [Suporte a host virtual baseado em nome](https://httpd.apache.org/docs/current/vhosts/name-based.html) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="cefe4-146">See [Name-based virtual host support](https://httpd.apache.org/docs/current/vhosts/name-based.html) for more information.</span></span> <span data-ttu-id="cefe4-147">As solicitações passadas por proxy na raiz para a porta 5000 do servidor em 127.0.0.1.</span><span class="sxs-lookup"><span data-stu-id="cefe4-147">Requests are proxied at the root to port 5000 of the server at 127.0.0.1.</span></span> <span data-ttu-id="cefe4-148">Para a comunicação bidirecional, `ProxyPass` e `ProxyPassReverse` são necessários.</span><span class="sxs-lookup"><span data-stu-id="cefe4-148">For bi-directional communication, `ProxyPass` and `ProxyPassReverse` are required.</span></span>

> [!WARNING]
> <span data-ttu-id="cefe4-149">Falha ao especificar uma [diretiva ServerName](https://httpd.apache.org/docs/current/mod/core.html#servername) no bloco **VirtualHost** expõe seu aplicativo para vulnerabilidades de segurança.</span><span class="sxs-lookup"><span data-stu-id="cefe4-149">Failure to specify a proper [ServerName directive](https://httpd.apache.org/docs/current/mod/core.html#servername) in the **VirtualHost** block exposes your app to security vulnerabilities.</span></span> <span data-ttu-id="cefe4-150">Associações de curinga de subdomínio (por exemplo, `*.example.com`) não oferecerão esse risco de segurança se você controlar o domínio pai completo (em vez de `*.com`, o qual é vulnerável).</span><span class="sxs-lookup"><span data-stu-id="cefe4-150">Subdomain wildcard binding (for example, `*.example.com`) doesn't pose this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="cefe4-151">Veja [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="cefe4-151">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

<span data-ttu-id="cefe4-152">O registro em log pode ser configurado por `VirtualHost` usando diretivas `ErrorLog` e `CustomLog`.</span><span class="sxs-lookup"><span data-stu-id="cefe4-152">Logging can be configured per `VirtualHost` using `ErrorLog` and `CustomLog` directives.</span></span> <span data-ttu-id="cefe4-153">`ErrorLog` é o local em que o servidor registrará em log os erros, enquanto `CustomLog` define o nome de arquivo e o formato do arquivo de log.</span><span class="sxs-lookup"><span data-stu-id="cefe4-153">`ErrorLog` is the location where the server logs errors, and `CustomLog` sets the filename and format of log file.</span></span> <span data-ttu-id="cefe4-154">Nesse caso, esse é o local em que as informações de solicitação são registradas em log.</span><span class="sxs-lookup"><span data-stu-id="cefe4-154">In this case, this is where request information is logged.</span></span> <span data-ttu-id="cefe4-155">Há uma linha para cada solicitação.</span><span class="sxs-lookup"><span data-stu-id="cefe4-155">There's one line for each request.</span></span>

<span data-ttu-id="cefe4-156">Salve o arquivo e teste a configuração.</span><span class="sxs-lookup"><span data-stu-id="cefe4-156">Save the file and test the configuration.</span></span> <span data-ttu-id="cefe4-157">Se tudo passar, a resposta deverá ser `Syntax [OK]`.</span><span class="sxs-lookup"><span data-stu-id="cefe4-157">If everything passes, the response should be `Syntax [OK]`.</span></span>

```bash
sudo service httpd configtest
```

<span data-ttu-id="cefe4-158">Reinicie o Apache:</span><span class="sxs-lookup"><span data-stu-id="cefe4-158">Restart Apache:</span></span>

```bash
sudo systemctl restart httpd
sudo systemctl enable httpd
```

## <a name="monitoring-the-app"></a><span data-ttu-id="cefe4-159">Monitoramento o aplicativo</span><span class="sxs-lookup"><span data-stu-id="cefe4-159">Monitoring the app</span></span>

<span data-ttu-id="cefe4-160">O Apache agora está configurado para encaminhar solicitações feitas a `http://localhost:80` para o aplicativo ASP.NET Core em execução no Kestrel em `http://127.0.0.1:5000`.</span><span class="sxs-lookup"><span data-stu-id="cefe4-160">Apache is now setup to forward requests made to `http://localhost:80` to the ASP.NET Core app running on Kestrel at `http://127.0.0.1:5000`.</span></span>  <span data-ttu-id="cefe4-161">No entanto, o Apache não está configurado para gerenciar o processo do Kestrel.</span><span class="sxs-lookup"><span data-stu-id="cefe4-161">However, Apache isn't set up to manage the Kestrel process.</span></span> <span data-ttu-id="cefe4-162">Use *systemd* e crie um arquivo de serviço para iniciar e monitorar o aplicativo Web subjacente.</span><span class="sxs-lookup"><span data-stu-id="cefe4-162">Use *systemd* and create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="cefe4-163">*systemd* é um sistema de inicialização que fornece muitos recursos poderosos para iniciar, parar e gerenciar processos.</span><span class="sxs-lookup"><span data-stu-id="cefe4-163">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span> 


### <a name="create-the-service-file"></a><span data-ttu-id="cefe4-164">Criar o arquivo de serviço</span><span class="sxs-lookup"><span data-stu-id="cefe4-164">Create the service file</span></span>

<span data-ttu-id="cefe4-165">Crie o arquivo de definição de serviço:</span><span class="sxs-lookup"><span data-stu-id="cefe4-165">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

<span data-ttu-id="cefe4-166">Eis um exemplo de arquivo de serviço para o aplicativo:</span><span class="sxs-lookup"><span data-stu-id="cefe4-166">An example service file for the app:</span></span>

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
> <span data-ttu-id="cefe4-167">**Usuário** &mdash; Se o usuário *apache* não é usado pela configuração, o usuário deve ser criado primeiro e a propriedade adequada dos arquivos deve ser concedida a ele.</span><span class="sxs-lookup"><span data-stu-id="cefe4-167">**User** &mdash; If the user *apache* isn't used by the configuration, the user must be created first and given proper ownership for files.</span></span>

> [!NOTE]
> <span data-ttu-id="cefe4-168">Alguns valores (por exemplo, cadeias de conexão de SQL) devem ser escapadas para que os provedores de configuração leiam as variáveis de ambiente.</span><span class="sxs-lookup"><span data-stu-id="cefe4-168">Some values (for example, SQL connection strings) must be escaped for the configuration providers to read the environment variables.</span></span> <span data-ttu-id="cefe4-169">Use o seguinte comando para gerar um valor corretamente com caracteres de escape para uso no arquivo de configuração:</span><span class="sxs-lookup"><span data-stu-id="cefe4-169">Use the following command to generate a properly escaped value for use in the configuration file:</span></span>
>
> ```console
> systemd-escape "<value-to-escape>"
> ```

<span data-ttu-id="cefe4-170">Salve o arquivo e habilite o serviço:</span><span class="sxs-lookup"><span data-stu-id="cefe4-170">Save the file and enable the service:</span></span>

```bash
systemctl enable kestrel-hellomvc.service
```

<span data-ttu-id="cefe4-171">Inicie o serviço e verifique se ele está em execução:</span><span class="sxs-lookup"><span data-stu-id="cefe4-171">Start the service and verify that it's running:</span></span>

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

<span data-ttu-id="cefe4-172">Com o proxy reverso configurado e o Kestrel gerenciado por meio de *systemd*, o aplicativo Web está totalmente configurado e pode ser acessado em um navegador no computador local em `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="cefe4-172">With the reverse proxy configured and Kestrel managed through *systemd*, the web app is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="cefe4-173">Inspecionando os cabeçalhos de resposta, o cabeçalho **Server** indica que o aplicativo ASP.NET Core é servido pelo Kestrel:</span><span class="sxs-lookup"><span data-stu-id="cefe4-173">Inspecting the response headers, the **Server** header indicates that the ASP.NET Core app is served by Kestrel:</span></span>

```
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a><span data-ttu-id="cefe4-174">Exibindo logs</span><span class="sxs-lookup"><span data-stu-id="cefe4-174">Viewing logs</span></span>

<span data-ttu-id="cefe4-175">Já que o aplicativo Web usando Kestrel é gerenciado usando *systemd*, os eventos e os processos são registrados em um diário centralizado.</span><span class="sxs-lookup"><span data-stu-id="cefe4-175">Since the web app using Kestrel is managed using *systemd*, events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="cefe4-176">No entanto, esse diário contém todas as entradas para todos os serviços e processos gerenciados por *systemd*.</span><span class="sxs-lookup"><span data-stu-id="cefe4-176">However, this journal includes entries for all of the services and processes managed by *systemd*.</span></span> <span data-ttu-id="cefe4-177">Para exibir os itens específicos de `kestrel-hellomvc.service`, use o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="cefe4-177">To view the `kestrel-hellomvc.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

<span data-ttu-id="cefe4-178">Para filtragem de hora, especifique opções de tempo com o comando.</span><span class="sxs-lookup"><span data-stu-id="cefe4-178">For time filtering, specify time options with the command.</span></span> <span data-ttu-id="cefe4-179">Por exemplo, use `--since today` para filtrar o dia atual ou `--until 1 hour ago` para ver as entradas da hora anterior.</span><span class="sxs-lookup"><span data-stu-id="cefe4-179">For example, use `--since today` to filter for the current day or `--until 1 hour ago` to see the previous hour's entries.</span></span> <span data-ttu-id="cefe4-180">Para obter mais informações, confira a [página de manual para journalctl](https://www.unix.com/man-page/centos/1/journalctl/).</span><span class="sxs-lookup"><span data-stu-id="cefe4-180">For more information, see the [man page for journalctl](https://www.unix.com/man-page/centos/1/journalctl/).</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-the-app"></a><span data-ttu-id="cefe4-181">Protegendo o aplicativo</span><span class="sxs-lookup"><span data-stu-id="cefe4-181">Securing the app</span></span>

### <a name="configure-firewall"></a><span data-ttu-id="cefe4-182">Configurar o firewall</span><span class="sxs-lookup"><span data-stu-id="cefe4-182">Configure firewall</span></span>

<span data-ttu-id="cefe4-183">*Firewalld* é um daemon dinâmico para gerenciar o firewall com suporte para zonas de rede.</span><span class="sxs-lookup"><span data-stu-id="cefe4-183">*Firewalld* is a dynamic daemon to manage the firewall with support for network zones.</span></span> <span data-ttu-id="cefe4-184">Portas e filtragem de pacote ainda podem ser gerenciados pelo iptables.</span><span class="sxs-lookup"><span data-stu-id="cefe4-184">Ports and packet filtering can still be managed by iptables.</span></span> <span data-ttu-id="cefe4-185">*Firewalld* deve ser instalado por padrão.</span><span class="sxs-lookup"><span data-stu-id="cefe4-185">*Firewalld* should be installed by default.</span></span> <span data-ttu-id="cefe4-186">`yum` pode ser usado para instalar o pacote ou verificar se ele está instalado.</span><span class="sxs-lookup"><span data-stu-id="cefe4-186">`yum` can be used to install the package or verify it's installed.</span></span>

```bash
sudo yum install firewalld -y
```

<span data-ttu-id="cefe4-187">Use `firewalld` para abrir somente as portas necessárias para o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="cefe4-187">Use `firewalld` to open only the ports needed for the app.</span></span> <span data-ttu-id="cefe4-188">Nesse caso, as portas 80 e 443 são usadas.</span><span class="sxs-lookup"><span data-stu-id="cefe4-188">In this case, port 80 and 443 are used.</span></span> <span data-ttu-id="cefe4-189">Os comandos a seguir definem permanentemente as portas 80 e 443 para estarem abertas:</span><span class="sxs-lookup"><span data-stu-id="cefe4-189">The following commands permanently set ports 80 and 443 to open:</span></span>

```bash
sudo firewall-cmd --add-port=80/tcp --permanent
sudo firewall-cmd --add-port=443/tcp --permanent
```

<span data-ttu-id="cefe4-190">Recarregue as configurações de firewall.</span><span class="sxs-lookup"><span data-stu-id="cefe4-190">Reload the firewall settings.</span></span> <span data-ttu-id="cefe4-191">Verifique os serviços e as portas disponíveis na zona padrão.</span><span class="sxs-lookup"><span data-stu-id="cefe4-191">Check the available services and ports in the default zone.</span></span> <span data-ttu-id="cefe4-192">As opções estão disponíveis por meio da inspeção de `firewall-cmd -h`.</span><span class="sxs-lookup"><span data-stu-id="cefe4-192">Options are available by inspecting `firewall-cmd -h`.</span></span>

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

### <a name="ssl-configuration"></a><span data-ttu-id="cefe4-193">Configuração do SSL</span><span class="sxs-lookup"><span data-stu-id="cefe4-193">SSL configuration</span></span>

<span data-ttu-id="cefe4-194">Para configurar o Apache para SSL, o módulo *mod_ssl* é usado.</span><span class="sxs-lookup"><span data-stu-id="cefe4-194">To configure Apache for SSL, the *mod_ssl* module is used.</span></span> <span data-ttu-id="cefe4-195">Quando o módulo *httpd* foi instalado, o módulo *mod_ssl* também foi instalado.</span><span class="sxs-lookup"><span data-stu-id="cefe4-195">When the *httpd* module was installed, the *mod_ssl* module was also installed.</span></span> <span data-ttu-id="cefe4-196">Se ele não foi instalado, use `yum` para adicioná-lo à configuração.</span><span class="sxs-lookup"><span data-stu-id="cefe4-196">If it wasn't installed, use `yum` to add it to the configuration.</span></span>

```bash
sudo yum install mod_ssl
```
<span data-ttu-id="cefe4-197">Para impor o SSL, instale o módulo `mod_rewrite` para habilitar a regravação de URL:</span><span class="sxs-lookup"><span data-stu-id="cefe4-197">To enforce SSL, install the `mod_rewrite` module to enable URL rewriting:</span></span>

```bash
sudo yum install mod_rewrite
```

<span data-ttu-id="cefe4-198">Modifique o arquivo *hellomvc.conf* para habilitar a regravação de URL e proteger a comunicação na porta 443:</span><span class="sxs-lookup"><span data-stu-id="cefe4-198">Modify the *hellomvc.conf* file to enable URL rewriting and secure communication on port 443:</span></span>

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
> <span data-ttu-id="cefe4-199">Este exemplo usa um certificado gerado localmente.</span><span class="sxs-lookup"><span data-stu-id="cefe4-199">This example is using a locally-generated certificate.</span></span> <span data-ttu-id="cefe4-200">**SSLCertificateFile** deve ser o arquivo de certificado primário para o nome de domínio.</span><span class="sxs-lookup"><span data-stu-id="cefe4-200">**SSLCertificateFile** should be the primary certificate file for the domain name.</span></span> <span data-ttu-id="cefe4-201">**SSLCertificateKeyFile** deve ser o arquivo de chave gerado quando o CSR é criado.</span><span class="sxs-lookup"><span data-stu-id="cefe4-201">**SSLCertificateKeyFile** should be the key file generated when CSR is created.</span></span> <span data-ttu-id="cefe4-202">**SSLCertificateChainFile** deve ser o arquivo de certificado intermediário (se houver) fornecido pela autoridade de certificação.</span><span class="sxs-lookup"><span data-stu-id="cefe4-202">**SSLCertificateChainFile** should be the intermediate certificate file (if any) that was supplied by the certificate authority.</span></span>

<span data-ttu-id="cefe4-203">Salve o arquivo e teste a configuração:</span><span class="sxs-lookup"><span data-stu-id="cefe4-203">Save the file and test the configuration:</span></span>

```bash
sudo service httpd configtest
```

<span data-ttu-id="cefe4-204">Reinicie o Apache:</span><span class="sxs-lookup"><span data-stu-id="cefe4-204">Restart Apache:</span></span>

```bash
sudo systemctl restart httpd
```

## <a name="additional-apache-suggestions"></a><span data-ttu-id="cefe4-205">Sugestões adicionais do Apache</span><span class="sxs-lookup"><span data-stu-id="cefe4-205">Additional Apache suggestions</span></span>

### <a name="additional-headers"></a><span data-ttu-id="cefe4-206">Cabeçalhos adicionais</span><span class="sxs-lookup"><span data-stu-id="cefe4-206">Additional headers</span></span>

<span data-ttu-id="cefe4-207">Para proteção contra ataques mal-intencionados, há alguns cabeçalhos que devem ser modificados ou adicionados.</span><span class="sxs-lookup"><span data-stu-id="cefe4-207">In order to secure against malicious attacks, there are a few headers that should either be modified or added.</span></span> <span data-ttu-id="cefe4-208">Verifique se o módulo `mod_headers` está instalado:</span><span class="sxs-lookup"><span data-stu-id="cefe4-208">Ensure that the `mod_headers` module is installed:</span></span>

```bash
sudo yum install mod_headers
```

#### <a name="secure-apache-from-clickjacking-attacks"></a><span data-ttu-id="cefe4-209">Proteger o Apache contra ataques de clickjacking</span><span class="sxs-lookup"><span data-stu-id="cefe4-209">Secure Apache from clickjacking attacks</span></span>

<span data-ttu-id="cefe4-210">[Clickjacking](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), também conhecido como um *ataque por inferência na interface do usuário*, é um ataque mal-intencionado em que o visitante do site é levado a clicar em um link ou botão em uma página diferente daquela que está visitando atualmente.</span><span class="sxs-lookup"><span data-stu-id="cefe4-210">[Clickjacking](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), also known as a *UI redress attack*, is a malicious attack where a website visitor is tricked into clicking a link or button on a different page than they're currently visiting.</span></span> <span data-ttu-id="cefe4-211">Use `X-FRAME-OPTIONS` para proteger o site.</span><span class="sxs-lookup"><span data-stu-id="cefe4-211">Use `X-FRAME-OPTIONS` to secure the site.</span></span>

<span data-ttu-id="cefe4-212">Edite o arquivo *httpd.conf*:</span><span class="sxs-lookup"><span data-stu-id="cefe4-212">Edit the *httpd.conf* file:</span></span>

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="cefe4-213">Adicione a linha `Header append X-FRAME-OPTIONS "SAMEORIGIN"`.</span><span class="sxs-lookup"><span data-stu-id="cefe4-213">Add the line `Header append X-FRAME-OPTIONS "SAMEORIGIN"`.</span></span> <span data-ttu-id="cefe4-214">Salve o arquivo.</span><span class="sxs-lookup"><span data-stu-id="cefe4-214">Save the file.</span></span> <span data-ttu-id="cefe4-215">Reinicie o Apache.</span><span class="sxs-lookup"><span data-stu-id="cefe4-215">Restart Apache.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="cefe4-216">Detecção de tipo MIME</span><span class="sxs-lookup"><span data-stu-id="cefe4-216">MIME-type sniffing</span></span>

<span data-ttu-id="cefe4-217">O cabeçalho `X-Content-Type-Options` impedirá o Internet Explorer de *farejar por MIME* (determinar o `Content-Type` de um arquivo com base no conteúdo do arquivo).</span><span class="sxs-lookup"><span data-stu-id="cefe4-217">The `X-Content-Type-Options` header prevents Internet Explorer from *MIME-sniffing* (determining a file's `Content-Type` from the file's content).</span></span> <span data-ttu-id="cefe4-218">Se o servidor define o cabeçalho `Content-Type` para `text/html` com a opção `nosniff` definida, o Internet Explorer renderiza o conteúdo como `text/html`, independentemente do conteúdo do arquivo.</span><span class="sxs-lookup"><span data-stu-id="cefe4-218">If the server sets the `Content-Type` header to `text/html` with the `nosniff` option set, Internet Explorer renders the content as `text/html` regardless of the file's content.</span></span>

<span data-ttu-id="cefe4-219">Edite o arquivo *httpd.conf*:</span><span class="sxs-lookup"><span data-stu-id="cefe4-219">Edit the *httpd.conf* file:</span></span>

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="cefe4-220">Adicione a linha `Header set X-Content-Type-Options "nosniff"`.</span><span class="sxs-lookup"><span data-stu-id="cefe4-220">Add the line `Header set X-Content-Type-Options "nosniff"`.</span></span> <span data-ttu-id="cefe4-221">Salve o arquivo.</span><span class="sxs-lookup"><span data-stu-id="cefe4-221">Save the file.</span></span> <span data-ttu-id="cefe4-222">Reinicie o Apache.</span><span class="sxs-lookup"><span data-stu-id="cefe4-222">Restart Apache.</span></span>

### <a name="load-balancing"></a><span data-ttu-id="cefe4-223">Balanceamento de carga</span><span class="sxs-lookup"><span data-stu-id="cefe4-223">Load Balancing</span></span> 

<span data-ttu-id="cefe4-224">Este exemplo mostra como instalar e configurar o Apache no CentOS 7 e no Kestrel no mesmo computador da instância.</span><span class="sxs-lookup"><span data-stu-id="cefe4-224">This example shows how to setup and configure Apache on CentOS 7 and Kestrel on the same instance machine.</span></span> <span data-ttu-id="cefe4-225">Para não ter um ponto único de falha, o uso de *mod_proxy_balancer* e a modificação do **VirtualHost** permitiriam o gerenciamento de várias instâncias dos aplicativos Web protegidos pelo servidor proxy do Apache.</span><span class="sxs-lookup"><span data-stu-id="cefe4-225">In order to not have a single point of failure; using *mod_proxy_balancer* and modifying the **VirtualHost** would allow for managing multiple instances of the web apps behind the Apache proxy server.</span></span>

```bash
sudo yum install mod_proxy_balancer
```

<span data-ttu-id="cefe4-226">No arquivo de configuração mostrado abaixo, uma instância adicional do aplicativo `hellomvc` está configurada para ser executada na porta 5001.</span><span class="sxs-lookup"><span data-stu-id="cefe4-226">In the configuration file shown below, an additional instance of the `hellomvc` app is setup to run on port 5001.</span></span> <span data-ttu-id="cefe4-227">A seção *Proxy* é definida com uma configuração de balanceador com dois membros para balancear carga de *byrequests*.</span><span class="sxs-lookup"><span data-stu-id="cefe4-227">The *Proxy* section is set with a balancer configuration with two members to load balance *byrequests*.</span></span>

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

### <a name="rate-limits"></a><span data-ttu-id="cefe4-228">Limites de taxa</span><span class="sxs-lookup"><span data-stu-id="cefe4-228">Rate Limits</span></span>
<span data-ttu-id="cefe4-229">Usando *mod_ratelimit*, que está incluído no módulo *httpd*, a largura de banda de clientes pode ser limitada:</span><span class="sxs-lookup"><span data-stu-id="cefe4-229">Using *mod_ratelimit*, which is included in the *httpd* module, the bandwidth of clients can be limited:</span></span>

```bash
sudo nano /etc/httpd/conf.d/ratelimit.conf
```
<span data-ttu-id="cefe4-230">O arquivo de exemplo limita a largura de banda a 600 KB/s no local raiz:</span><span class="sxs-lookup"><span data-stu-id="cefe4-230">The example file limits bandwidth as 600 KB/sec under the root location:</span></span>

```
<IfModule mod_ratelimit.c>
    <Location />
        SetOutputFilter RATE_LIMIT
        SetEnv rate-limit 600
    </Location>
</IfModule>
```

---
title: Hospedar o ASP.NET Core no Linux com o Apache
description: "Saiba como configurar o Apache como um servidor proxy reverso no CentOS para redirecionar o tráfego HTTP para um aplicativo Web ASP.NET Core em execução no Kestrel."
keywords: ASP.NET Core, Apache, CentOS, Proxy Reverso, Linux, mod_proxy, httpd, hospedagem
author: spboyer
ms.author: spboyer
manager: wpickett
ms.date: 10/19/2016
ms.topic: article
ms.assetid: fa9b0cb7-afb3-4361-9e7e-33afffeaca0c
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/apache-proxy
ms.openlocfilehash: 9dc22ea20a6ae2e2477f9e6db95ddabecc038dcb
ms.sourcegitcommit: f8f6b5934bd071a349f5bc1e389365c52b1c00fa
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/14/2017
---
# <a name="set-up-a-hosting-environment-for-aspnet-core-on-linux-with-apache-and-deploy-to-it"></a><span data-ttu-id="c1bb0-104">Configurar um ambiente de hospedagem para o ASP.NET Core no Linux com o Apache e implantar nele</span><span class="sxs-lookup"><span data-stu-id="c1bb0-104">Set up a hosting environment for ASP.NET Core on Linux with Apache, and deploy to it</span></span>

<span data-ttu-id="c1bb0-105">Por [Shayne Boyer](https://github.com/spboyer)</span><span class="sxs-lookup"><span data-stu-id="c1bb0-105">By [Shayne Boyer](https://github.com/spboyer)</span></span>

<span data-ttu-id="c1bb0-106">O Apache é um servidor HTTP muito popular e pode ser configurado como um proxy para redirecionar o tráfego HTTP de forma semelhante ao Nginx.</span><span class="sxs-lookup"><span data-stu-id="c1bb0-106">Apache is a very popular HTTP server and can be configured as a proxy to redirect HTTP traffic similar to nginx.</span></span> <span data-ttu-id="c1bb0-107">Neste guia, aprenderemos a configurar o Apache no CentOS 7 e usá-lo como um proxy reverso para receber conexões de entrada e redirecioná-las para o aplicativo ASP.NET Core em execução no Kestrel.</span><span class="sxs-lookup"><span data-stu-id="c1bb0-107">In this guide, we will learn how to set up Apache on CentOS 7 and use it as a reverse proxy to welcome incoming connections and redirect them to the ASP.NET Core application running on Kestrel.</span></span> <span data-ttu-id="c1bb0-108">Para essa finalidade, usaremos a extensão *mod_proxy* e outros módulos do Apache relacionados.</span><span class="sxs-lookup"><span data-stu-id="c1bb0-108">For this purpose, we will use the *mod_proxy* extension and other related Apache modules.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c1bb0-109">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="c1bb0-109">Prerequisites</span></span>

1. <span data-ttu-id="c1bb0-110">Um servidor que executa o CentOS 7, com uma conta de usuário padrão com privilégios sudo.</span><span class="sxs-lookup"><span data-stu-id="c1bb0-110">A server running CentOS 7, with a standard user account with sudo privilege.</span></span>
2. <span data-ttu-id="c1bb0-111">Um aplicativo ASP.NET Core existente.</span><span class="sxs-lookup"><span data-stu-id="c1bb0-111">An existing ASP.NET Core application.</span></span> 

## <a name="publish-your-application"></a><span data-ttu-id="c1bb0-112">Publicar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="c1bb0-112">Publish your application</span></span>

<span data-ttu-id="c1bb0-113">Execute `dotnet publish -c Release` no ambiente de desenvolvimento para empacotar o aplicativo em um diretório independente que pode ser executado no servidor.</span><span class="sxs-lookup"><span data-stu-id="c1bb0-113">Run `dotnet publish -c Release` from your development environment to package your application into a self-contained directory that can run on your server.</span></span> <span data-ttu-id="c1bb0-114">Em seguida, o aplicativo publicado deve ser copiado para o servidor usando o SCP, FTP ou outro método de transferência de arquivo.</span><span class="sxs-lookup"><span data-stu-id="c1bb0-114">The published application must then be copied to the server using SCP, FTP or other file transfer method.</span></span> 

> [!NOTE]
> <span data-ttu-id="c1bb0-115">Em um cenário de implantação de produção, um fluxo de trabalho de integração contínua faz o trabalho de publicar o aplicativo e copiar os ativos para o servidor.</span><span class="sxs-lookup"><span data-stu-id="c1bb0-115">Under a production deployment scenario, a continuous integration workflow does the work of publishing the application and copying the assets to the server.</span></span> 

## <a name="configure-a-proxy-server"></a><span data-ttu-id="c1bb0-116">Configurar um servidor proxy</span><span class="sxs-lookup"><span data-stu-id="c1bb0-116">Configure a proxy server</span></span>

<span data-ttu-id="c1bb0-117">Um proxy reverso é uma configuração comum para atender a aplicativos Web dinâmicos.</span><span class="sxs-lookup"><span data-stu-id="c1bb0-117">A reverse proxy is a common setup for serving dynamic web applications.</span></span> <span data-ttu-id="c1bb0-118">O proxy reverso encerra a solicitação HTTP e a encaminha para o aplicativo ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c1bb0-118">The reverse proxy terminates the HTTP request and forwards it to the ASP.NET application.</span></span>

<span data-ttu-id="c1bb0-119">Um servidor proxy é aquele que encaminha as solicitações de cliente para outro servidor, em vez de atendê-las por conta própria.</span><span class="sxs-lookup"><span data-stu-id="c1bb0-119">A proxy server is one which forwards client requests to another server instead of fulfilling them itself.</span></span> <span data-ttu-id="c1bb0-120">Um proxy reverso encaminha para um destino fixo, normalmente, em nome de clientes arbitrários.</span><span class="sxs-lookup"><span data-stu-id="c1bb0-120">A reverse proxy forwards to a fixed destination, typically on behalf of arbitrary clients.</span></span> <span data-ttu-id="c1bb0-121">Neste guia, o Apache está sendo configurado como o proxy reverso em execução no mesmo servidor em que o Kestrel está atendendo o aplicativo ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c1bb0-121">In this guide, Apache is being configured as the reverse proxy running on the same server that Kestrel is serving the ASP.NET Core application.</span></span> 

<span data-ttu-id="c1bb0-122">Cada parte do aplicativo pode existir em computadores físicos separados, contêineres do Docker ou em uma combinação de configurações, dependendo de suas necessidades ou restrições de arquitetura.</span><span class="sxs-lookup"><span data-stu-id="c1bb0-122">Each piece of the application can exist on separate physical machines, Docker containers, or a combination of configurations depending on your architectural needs or restrictions.</span></span>

### <a name="install-apache"></a><span data-ttu-id="c1bb0-123">Instalar o Apache</span><span class="sxs-lookup"><span data-stu-id="c1bb0-123">Install Apache</span></span>

<span data-ttu-id="c1bb0-124">A instalação do servidor Web Apache no CentOS é feita com um único comando, mas primeiro vamos atualizar nossos pacotes.</span><span class="sxs-lookup"><span data-stu-id="c1bb0-124">Installing the Apache web server on CentOS is a single command, but first let's update our packages.</span></span>

```bash
    sudo yum update -y
```

<span data-ttu-id="c1bb0-125">Isso garante que todos os pacotes instalados são atualizados para sua última versão.</span><span class="sxs-lookup"><span data-stu-id="c1bb0-125">This ensures that all of the installed packages are updated to their latest version.</span></span> <span data-ttu-id="c1bb0-126">Instalar o Apache usando `yum`</span><span class="sxs-lookup"><span data-stu-id="c1bb0-126">Install Apache using `yum`</span></span>

```bash
    sudo yum -y install httpd mod_ssl
```

<span data-ttu-id="c1bb0-127">O resultado deve refletir algo semelhante ao mostrado a seguir.</span><span class="sxs-lookup"><span data-stu-id="c1bb0-127">The output should reflect something similar to the following.</span></span>

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
> <span data-ttu-id="c1bb0-128">Neste exemplo, o resultado reflete httpd.86_64, pois a versão do CentOS 7 é de 64 bits.</span><span class="sxs-lookup"><span data-stu-id="c1bb0-128">In this example the output reflects httpd.86_64 since the CentOS 7 version is 64 bit.</span></span> <span data-ttu-id="c1bb0-129">O resultado pode ser diferente para seu servidor.</span><span class="sxs-lookup"><span data-stu-id="c1bb0-129">The output may be different for your server.</span></span> <span data-ttu-id="c1bb0-130">Para verificar o local em que o Apache está instalado, execute `whereis httpd` em um prompt de comando.</span><span class="sxs-lookup"><span data-stu-id="c1bb0-130">To verify where Apache is installed, run `whereis httpd` from a command prompt.</span></span> 

### <a name="configure-apache-for-reverse-proxy"></a><span data-ttu-id="c1bb0-131">Configurar o Apache para proxy reverso</span><span class="sxs-lookup"><span data-stu-id="c1bb0-131">Configure Apache for reverse proxy</span></span>

<span data-ttu-id="c1bb0-132">Os arquivos de configuração do Apache estão localizados no diretório `/etc/httpd/conf.d/`.</span><span class="sxs-lookup"><span data-stu-id="c1bb0-132">Configuration files for Apache are located within the `/etc/httpd/conf.d/` directory.</span></span> <span data-ttu-id="c1bb0-133">Qualquer arquivo com a extensão **.conf** será processado em ordem alfabética, além dos arquivos de configuração do módulo em `/etc/httpd/conf.modules.d/`, que contém todos os arquivos de configuração necessários para carregar os módulos.</span><span class="sxs-lookup"><span data-stu-id="c1bb0-133">Any file with the **.conf** extension will be processed in alphabetical order in addition to the module configuration files in `/etc/httpd/conf.modules.d/`, which contains any configuration files necessary to load modules.</span></span>

<span data-ttu-id="c1bb0-134">Crie um arquivo de configuração para seu aplicativo; neste exemplo, vamos chamá-lo `hellomvc.conf`</span><span class="sxs-lookup"><span data-stu-id="c1bb0-134">Create a configuration file for your app, for this example we'll call it `hellomvc.conf`</span></span>

```text
    <VirtualHost *:80>
        ProxyPreserveHost On
        ProxyPass / http://127.0.0.1:5000/
        ProxyPassReverse / http://127.0.0.1:5000/
        ErrorLog /var/log/httpd/hellomvc-error.log
        CustomLog /var/log/httpd/hellomvc-access.log common
    </VirtualHost>
```

<span data-ttu-id="c1bb0-135">O nó *VirtualHost*, que pode haver vários em um arquivo ou em um servidor em vários arquivos, é definido para escutar em qualquer endereço IP usando a porta 80.</span><span class="sxs-lookup"><span data-stu-id="c1bb0-135">The *VirtualHost* node, of which there can be multiple in a file or on a server in many files, is set to listen on any IP address using port 80.</span></span> <span data-ttu-id="c1bb0-136">As duas próximas linhas são definidas para passar todas as solicitações recebidas na raiz para o computador 127.0.0.1 na porta 5000 e na ordem inversa.</span><span class="sxs-lookup"><span data-stu-id="c1bb0-136">The next two lines are set to pass all requests received at the root to the machine 127.0.0.1 port 5000 and in reverse.</span></span> <span data-ttu-id="c1bb0-137">Para que haja comunicação bidirecional, ambas as configurações *ProxyPass* e *ProxyPassReverse* são necessárias.</span><span class="sxs-lookup"><span data-stu-id="c1bb0-137">For there to be bi-directional communication, both settings *ProxyPass* and *ProxyPassReverse* are required.</span></span>

<span data-ttu-id="c1bb0-138">O log pode ser configurado por VirtualHost usando as diretivas *ErrorLog* e *CustomLog*.</span><span class="sxs-lookup"><span data-stu-id="c1bb0-138">Logging can be configured per VirtualHost using *ErrorLog* and *CustomLog* directives.</span></span> <span data-ttu-id="c1bb0-139">*ErrorLog* é o local em que o servidor registrará os erros e *CustomLog* define o nome de arquivo e o formato do arquivo de log.</span><span class="sxs-lookup"><span data-stu-id="c1bb0-139">*ErrorLog* is the location where the server will log errors and *CustomLog* sets the filename and format of log file.</span></span> <span data-ttu-id="c1bb0-140">Em nosso caso, esse é o local em que as informações de solicitação serão registradas.</span><span class="sxs-lookup"><span data-stu-id="c1bb0-140">In our case this is where request information will be logged.</span></span> <span data-ttu-id="c1bb0-141">Haverá uma linha para cada solicitação.</span><span class="sxs-lookup"><span data-stu-id="c1bb0-141">There will be one line for each request.</span></span>

<span data-ttu-id="c1bb0-142">Salve o arquivo e teste a configuração.</span><span class="sxs-lookup"><span data-stu-id="c1bb0-142">Save the file, and test the configuration.</span></span> <span data-ttu-id="c1bb0-143">Se tudo passar, a resposta deverá ser `Syntax [OK]`.</span><span class="sxs-lookup"><span data-stu-id="c1bb0-143">If everything passes, the response should be `Syntax [OK]`.</span></span>

```bash
    sudo service httpd configtest
```

<span data-ttu-id="c1bb0-144">Reinicie o Apache.</span><span class="sxs-lookup"><span data-stu-id="c1bb0-144">Restart Apache.</span></span>

```text
    sudo systemctl restart httpd
    sudo systemctl enable httpd
```

## <a name="monitoring-our-application"></a><span data-ttu-id="c1bb0-145">Monitorando nosso aplicativo</span><span class="sxs-lookup"><span data-stu-id="c1bb0-145">Monitoring our application</span></span>

<span data-ttu-id="c1bb0-146">O Apache agora está configurado para encaminhar solicitações feitas a `http://localhost:80` para o aplicativo ASP.NET Core em execução no Kestrel em `http://127.0.0.1:5000`.</span><span class="sxs-lookup"><span data-stu-id="c1bb0-146">Apache is now setup to forward requests made to `http://localhost:80` on to the ASP.NET Core application running on Kestrel at `http://127.0.0.1:5000`.</span></span>  <span data-ttu-id="c1bb0-147">No entanto, o Apache não está configurado para gerenciar o processo do Kestrel.</span><span class="sxs-lookup"><span data-stu-id="c1bb0-147">However, Apache is not set up to manage the Kestrel process.</span></span> <span data-ttu-id="c1bb0-148">Usaremos *systemd* e criaremos um arquivo de serviço para iniciar e monitorar o aplicativo Web subjacente.</span><span class="sxs-lookup"><span data-stu-id="c1bb0-148">We will use *systemd* and create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="c1bb0-149">*systemd* é um sistema de inicialização que fornece muitos recursos avançados para iniciar, interromper e gerenciar processos.</span><span class="sxs-lookup"><span data-stu-id="c1bb0-149">*systemd* is an init system that provides many powerful features for starting, stopping and managing processes.</span></span> 


### <a name="create-the-service-file"></a><span data-ttu-id="c1bb0-150">Criar o arquivo de serviço</span><span class="sxs-lookup"><span data-stu-id="c1bb0-150">Create the service file</span></span>

<span data-ttu-id="c1bb0-151">Criar o arquivo de definição de serviço</span><span class="sxs-lookup"><span data-stu-id="c1bb0-151">Create the service definition file</span></span> 

```bash
    sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

<span data-ttu-id="c1bb0-152">Um arquivo de serviço de exemplo para nosso aplicativo.</span><span class="sxs-lookup"><span data-stu-id="c1bb0-152">An example service file for our application.</span></span>

```text
[Unit]
    Description=Example .NET Web API Application running on CentOS 7

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
> <span data-ttu-id="c1bb0-153">**Usuário** – se o usuário *apache* não é usado pela configuração, o usuário definido aqui deve ser criado primeiro e a propriedade adequada dos arquivos deve ser concedida a ele</span><span class="sxs-lookup"><span data-stu-id="c1bb0-153">**User** -- If the user *apache* is not used by your configuration, the user defined here must be created first and given proper ownership for files</span></span>

<span data-ttu-id="c1bb0-154">Salve o arquivo e habilite o serviço.</span><span class="sxs-lookup"><span data-stu-id="c1bb0-154">Save the file and enable the service.</span></span>

```bash
    systemctl enable kestrel-hellomvc.service
```

<span data-ttu-id="c1bb0-155">Inicie o serviço e verifique se ele está em execução.</span><span class="sxs-lookup"><span data-stu-id="c1bb0-155">Start the service and verify that it is running.</span></span>

```
    systemctl start kestrel-hellomvc.service
    systemctl status kestrel-hellomvc.service

    ● kestrel-hellomvc.service - Example .NET Web API Application running on CentOS 7
        Loaded: loaded (/etc/systemd/system/kestrel-hellomvc.service; enabled)
        Active: active (running) since Thu 2016-10-18 04:09:35 NZDT; 35s ago
    Main PID: 9021 (dotnet)
        CGroup: /system.slice/kestrel-hellomvc.service
                └─9021 /usr/local/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
```

<span data-ttu-id="c1bb0-156">Com o proxy reverso configurado e o Kestrel gerenciado por meio de systemd, o aplicativo Web está totalmente configurado e pode ser acessado em um navegador no computador local em `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="c1bb0-156">With the reverse proxy configured and Kestrel managed through systemd, the web application is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="c1bb0-157">Inspecionando os cabeçalhos de resposta, o **Server** ainda mostra o aplicativo ASP.NET Core sendo atendido pelo Kestrel.</span><span class="sxs-lookup"><span data-stu-id="c1bb0-157">Inspecting the response headers, the **Server** still shows the ASP.NET Core application being served by Kestrel.</span></span>

```text
    HTTP/1.1 200 OK
    Date: Tue, 11 Oct 2016 16:22:23 GMT
    Server: Kestrel
    Keep-Alive: timeout=5, max=98
    Connection: Keep-Alive
    Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a><span data-ttu-id="c1bb0-158">Exibindo logs</span><span class="sxs-lookup"><span data-stu-id="c1bb0-158">Viewing logs</span></span>

<span data-ttu-id="c1bb0-159">Já que o aplicativo Web que usa o Kestrel é gerenciado usando systemd, todos os eventos e processos são registrados em um diário centralizado.</span><span class="sxs-lookup"><span data-stu-id="c1bb0-159">Since the web application using Kestrel is managed using systemd, all events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="c1bb0-160">No entanto, esse diário contém todas as entradas para todos os serviços e processos gerenciados pelo systemd.</span><span class="sxs-lookup"><span data-stu-id="c1bb0-160">However, this journal includes all entries for all services and processes managed by systemd.</span></span> <span data-ttu-id="c1bb0-161">Para exibir os itens específicos de `kestrel-hellomvc.service`, use o comando a seguir.</span><span class="sxs-lookup"><span data-stu-id="c1bb0-161">To view the `kestrel-hellomvc.service` specific items, use the following command.</span></span>

```bash
    sudo journalctl -fu kestrel-hellomvc.service
```

<span data-ttu-id="c1bb0-162">Para obter mais filtragem, opções de hora como `--since today`, `--until 1 hour ago` ou uma combinação delas, pode reduzir a quantidade de entradas retornadas.</span><span class="sxs-lookup"><span data-stu-id="c1bb0-162">For further filtering, time options such as `--since today`, `--until 1 hour ago` or a combination of these can reduce the amount of entries returned.</span></span>

```bash
    sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-our-application"></a><span data-ttu-id="c1bb0-163">Protegendo nosso aplicativo</span><span class="sxs-lookup"><span data-stu-id="c1bb0-163">Securing our application</span></span>

### <a name="configure-firewall"></a><span data-ttu-id="c1bb0-164">Configurar o firewall</span><span class="sxs-lookup"><span data-stu-id="c1bb0-164">Configure firewall</span></span>

<span data-ttu-id="c1bb0-165">*Firewalld* é um daemon dinâmico para gerenciar o firewall com suporte para zonas de rede, embora ainda seja possível usar iptables para gerenciar portas e a filtragem de pacotes.</span><span class="sxs-lookup"><span data-stu-id="c1bb0-165">*Firewalld* is a dynamic daemon to manage firewall with support for network zones, although you can still use iptables to manage ports and packet filtering.</span></span> <span data-ttu-id="c1bb0-166">Firewalld deve ser instalado por padrão e `yum` pode ser usado para instalar o pacote ou verificar.</span><span class="sxs-lookup"><span data-stu-id="c1bb0-166">Firewalld should be installed by default, `yum` can be used to install the package or verify.</span></span>

```bash
    sudo yum install firewalld -y
```

<span data-ttu-id="c1bb0-167">Usando `firewalld`, você pode abrir somente as portas necessárias para o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="c1bb0-167">Using `firewalld` you can open only the ports needed for the application.</span></span> <span data-ttu-id="c1bb0-168">Nesse caso, as portas 80 e 443 são usadas.</span><span class="sxs-lookup"><span data-stu-id="c1bb0-168">In this case, port 80 and 443 are used.</span></span> <span data-ttu-id="c1bb0-169">Os comandos a seguir definem permanentemente essas portas para estarem abertas.</span><span class="sxs-lookup"><span data-stu-id="c1bb0-169">The following commands permanently sets these to open.</span></span>

```bash
    sudo firewall-cmd --add-port=80/tcp --permanent
    sudo firewall-cmd --add-port=443/tcp --permanent
```

<span data-ttu-id="c1bb0-170">Recarregue as configurações do firewall e verifique os serviços e as portas disponíveis na zona padrão.</span><span class="sxs-lookup"><span data-stu-id="c1bb0-170">Reload the firewall settings, and check the available services and ports in the default zone.</span></span> <span data-ttu-id="c1bb0-171">As opções estão disponíveis por meio da inspeção de `firewall-cmd -h`</span><span class="sxs-lookup"><span data-stu-id="c1bb0-171">Options are available by inspecting `firewall-cmd -h`</span></span>

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

### <a name="ssl-configuration"></a><span data-ttu-id="c1bb0-172">Configuração do SSL</span><span class="sxs-lookup"><span data-stu-id="c1bb0-172">SSL configuration</span></span>

<span data-ttu-id="c1bb0-173">Para configurar o Apache para SSL, o módulo mod_ssl é usado.</span><span class="sxs-lookup"><span data-stu-id="c1bb0-173">To configure Apache for SSL, the mod_ssl module is used.</span></span>  <span data-ttu-id="c1bb0-174">Isso foi instalado inicialmente quando instalamos o módulo `httpd`.</span><span class="sxs-lookup"><span data-stu-id="c1bb0-174">This was installed initially when we installed the `httpd` module.</span></span> <span data-ttu-id="c1bb0-175">Se ele não foi incluído ou instalado, use o yum para adicioná-lo à configuração.</span><span class="sxs-lookup"><span data-stu-id="c1bb0-175">If it was missed or not installed, use yum to add it to your configuration.</span></span>

```bash
    sudo yum install mod_ssl
```
<span data-ttu-id="c1bb0-176">Para impor o SSL, instale `mod_rewrite`</span><span class="sxs-lookup"><span data-stu-id="c1bb0-176">To enforce SSL, install `mod_rewrite`</span></span>

```bash
    sudo yum install mod_rewrite
```

<span data-ttu-id="c1bb0-177">O arquivo `hellomvc.conf` criado para este exemplo precisa ser modificado para permitir a reescrita, bem como a adição da nova seção **VirtualHost** para HTTPS.</span><span class="sxs-lookup"><span data-stu-id="c1bb0-177">The `hellomvc.conf` file that was created for this example needs to be modified to enable the rewrite as well as adding the new **VirtualHost** section for HTTPS.</span></span>

```text
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
> <span data-ttu-id="c1bb0-178">Este exemplo usa um certificado gerado localmente.</span><span class="sxs-lookup"><span data-stu-id="c1bb0-178">This example is using a locally generated certificate.</span></span> <span data-ttu-id="c1bb0-179">**SSLCertificateFile** deve ser o arquivo de certificado primário para o nome de domínio.</span><span class="sxs-lookup"><span data-stu-id="c1bb0-179">**SSLCertificateFile** should be your primary certificate file for your domain name.</span></span> <span data-ttu-id="c1bb0-180">**SSLCertificateKeyFile** deve ser o arquivo de chave gerado quando o CSR foi criado.</span><span class="sxs-lookup"><span data-stu-id="c1bb0-180">**SSLCertificateKeyFile** should be the key file generated when you created the CSR.</span></span> <span data-ttu-id="c1bb0-181">**SSLCertificateChainFile** deve ser o arquivo de certificado intermediário (se houver) fornecido pela autoridade de certificação</span><span class="sxs-lookup"><span data-stu-id="c1bb0-181">**SSLCertificateChainFile** should be the intermediate certificate file (if any) that was supplied by your certificate authority</span></span>

<span data-ttu-id="c1bb0-182">Salve o arquivo e teste a configuração.</span><span class="sxs-lookup"><span data-stu-id="c1bb0-182">Save the file, and test the configuration.</span></span>

```bash
    sudo service httpd configtest
```

<span data-ttu-id="c1bb0-183">Reinicie o Apache.</span><span class="sxs-lookup"><span data-stu-id="c1bb0-183">Restart Apache.</span></span>

```bash
    sudo systemctl restart httpd
```

## <a name="additional-apache-suggestions"></a><span data-ttu-id="c1bb0-184">Sugestões adicionais do Apache</span><span class="sxs-lookup"><span data-stu-id="c1bb0-184">Additional Apache suggestions</span></span>

### <a name="additional-headers"></a><span data-ttu-id="c1bb0-185">Cabeçalhos adicionais</span><span class="sxs-lookup"><span data-stu-id="c1bb0-185">Additional Headers</span></span> 
<span data-ttu-id="c1bb0-186">Para proteção contra ataques mal-intencionados, há alguns cabeçalhos que devem ser modificados ou adicionados.</span><span class="sxs-lookup"><span data-stu-id="c1bb0-186">In order to secure against malicious attacks there are a few headers that should either be modified or added.</span></span> <span data-ttu-id="c1bb0-187">Verifique se o módulo `mod_headers` está instalado.</span><span class="sxs-lookup"><span data-stu-id="c1bb0-187">Ensure that the `mod_headers` module is installed.</span></span>

```bash
    sudo yum install mod_headers
```

#### <a name="secure-apache-from-clickjacking"></a><span data-ttu-id="c1bb0-188">Proteger o Apache contra clickjacking</span><span class="sxs-lookup"><span data-stu-id="c1bb0-188">Secure Apache from clickjacking</span></span>
<span data-ttu-id="c1bb0-189">Clickjacking é uma técnica mal-intencionada usada para coletar os cliques de um usuário infectado.</span><span class="sxs-lookup"><span data-stu-id="c1bb0-189">Clickjacking is a malicious technique to collect an infected user's clicks.</span></span> <span data-ttu-id="c1bb0-190">O clickjacking engana a vítima (visitante) fazendo-a clicar em um site infectado.</span><span class="sxs-lookup"><span data-stu-id="c1bb0-190">Clickjacking tricks the victim (visitor) into clicking on an infected site.</span></span> <span data-ttu-id="c1bb0-191">Use X-FRAME-OPTIONS para proteger o site.</span><span class="sxs-lookup"><span data-stu-id="c1bb0-191">Use X-FRAME-OPTIONS to secure your site.</span></span>

<span data-ttu-id="c1bb0-192">Edite o arquivo httpd.conf.</span><span class="sxs-lookup"><span data-stu-id="c1bb0-192">Edit the httpd.conf file.</span></span>

```bash
    sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="c1bb0-193">Adicione a linha `Header append X-FRAME-OPTIONS "SAMEORIGIN"` e salve o arquivo e, depois, reinicie o Apache.</span><span class="sxs-lookup"><span data-stu-id="c1bb0-193">Add the line `Header append X-FRAME-OPTIONS "SAMEORIGIN"` and save the file, then restart Apache.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="c1bb0-194">Detecção de tipo MIME</span><span class="sxs-lookup"><span data-stu-id="c1bb0-194">MIME-type sniffing</span></span>

<span data-ttu-id="c1bb0-195">Esse cabeçalho previne que o Internet Explorer faça uma detecção MIME de uma resposta distante do tipo de conteúdo declarado, já que o cabeçalho instrui o navegador a não substituir o tipo de conteúdo de resposta.</span><span class="sxs-lookup"><span data-stu-id="c1bb0-195">This header prevents Internet Explorer from MIME-sniffing a response away from the declared content-type as the header instructs the browser not to override the response content type.</span></span> <span data-ttu-id="c1bb0-196">Com a opção nosniff, se o servidor informar que o conteúdo é text/html, o navegador a renderizará como text/html.</span><span class="sxs-lookup"><span data-stu-id="c1bb0-196">With the nosniff option, if the server says the content is text/html, the browser will render it as text/html.</span></span>

<span data-ttu-id="c1bb0-197">Edite o arquivo httpd.conf.</span><span class="sxs-lookup"><span data-stu-id="c1bb0-197">Edit the httpd.conf file.</span></span>

```bash
    sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="c1bb0-198">Adicione a linha `Header set X-Content-Type-Options "nosniff"` e salve o arquivo e, depois, reinicie o Apache.</span><span class="sxs-lookup"><span data-stu-id="c1bb0-198">Add the line `Header set X-Content-Type-Options "nosniff"` and save the file, then restart Apache.</span></span>

### <a name="load-balancing"></a><span data-ttu-id="c1bb0-199">Balanceamento de carga</span><span class="sxs-lookup"><span data-stu-id="c1bb0-199">Load Balancing</span></span> 

<span data-ttu-id="c1bb0-200">Este exemplo mostra como instalar e configurar o Apache no CentOS 7 e no Kestrel no mesmo computador da instância.</span><span class="sxs-lookup"><span data-stu-id="c1bb0-200">This example shows how to setup and configure Apache on CentOS 7 and Kestrel on the same instance machine.</span></span>  <span data-ttu-id="c1bb0-201">No entanto, para não ter um ponto único de falha, o uso de *mod_proxy_balancer* e a modificação do VirtualHost permitirão o gerenciamento de várias instâncias dos aplicativos Web protegidos pelo servidor proxy do Apache.</span><span class="sxs-lookup"><span data-stu-id="c1bb0-201">However, in order to not have a single point of failure; using *mod_proxy_balancer* and modifying the VirtualHost would allow for managing mutliple instances of the web applications behind the Apache proxy server.</span></span>

```bash
    sudo yum install mod_proxy_balancer
```

<span data-ttu-id="c1bb0-202">No arquivo de configuração, uma instância adicional do aplicativo `hellomvc` foi configurada para ser executada na porta 5001 e a seção *Proxy* foi definida com uma configuração de balanceador com dois membros para balancear a carga de *byrequests*.</span><span class="sxs-lookup"><span data-stu-id="c1bb0-202">In the configuration file, an additional instance of the `hellomvc` app has been setup to run on port 5001 and the *Proxy* section has been set with a balancer configuration with two members to load balance *byrequests*.</span></span>

```text
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

### <a name="rate-limits"></a><span data-ttu-id="c1bb0-203">Limites de taxa</span><span class="sxs-lookup"><span data-stu-id="c1bb0-203">Rate Limits</span></span>
<span data-ttu-id="c1bb0-204">Usando `mod_ratelimit`, que está incluído no módulo `htttpd`, você pode limitar a quantidade de largura de banda de clientes.</span><span class="sxs-lookup"><span data-stu-id="c1bb0-204">Using `mod_ratelimit`, which is included in the `htttpd` module you can limit the amount of bandwidth of clients.</span></span> 

```bash
    sudo nano /etc/httpd/conf.d/ratelimit.conf
```
<span data-ttu-id="c1bb0-205">O arquivo de exemplo limita a largura de banda a 600 KB/s no local raiz.</span><span class="sxs-lookup"><span data-stu-id="c1bb0-205">The example file limits bandwidth as 600 KB/sec under the root location.</span></span>

```text
    <IfModule mod_ratelimit.c>
        <Location />
            SetOutputFilter RATE_LIMIT
            SetEnv rate-limit 600
        </Location>
    </IfModule>
```

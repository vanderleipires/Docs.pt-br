---
title: Host ASP.NET Core no Linux com Nginx
description: "Descreve como configurar Nginx como um proxy reverso no Ubuntu 16.04 para encaminhar o tráfego HTTP para um aplicativo Web do ASP.NET Core em execução no Kestrel."
keywords: ASP.NET Core, Linux, nginx, Ubuntu, Proxy Reverso
author: rick-anderson
ms.author: riande
manager: wpickett
ms.date: 08/21/2017
ms.topic: article
ms.assetid: 1c33e576-33de-481a-8ad3-896b94fde0e3
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/linuxproduction
ms.openlocfilehash: 7c7b949fc922c605aa4554c158200a4123c4eb1c
ms.sourcegitcommit: fc98e93464ccf37d9904e89a71cdddbd4bbdb86a
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/05/2018
---
# <a name="set-up-a-hosting-environment-for-aspnet-core-on-linux-with-nginx-and-deploy-to-it"></a>Configurar um ambiente de hospedagem para o ASP.NET Core no Linux com Nginx e implantar nele

Por [Sourabh Shirhatti](https://twitter.com/sshirhatti)

Este guia explica como configurar um ambiente do ASP.NET Core pronto para produção em um servidor do Ubuntu 16.04.

**Observação:** para Ubuntu 14.04, o supervisord é recomendado como uma solução para monitorar o processo do Kestrel. O systemd não está disponível no Ubuntu 14.04. [Consulte a versão anterior deste documento](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md)

Este guia:

* Coloca um aplicativo ASP.NET Core existente em um servidor proxy reverso
* Configura o servidor proxy reverso para encaminhar solicitações ao servidor Web Kestrel
* Assegura que o aplicativo Web seja executado na inicialização como um daemon
* Configura uma ferramenta de gerenciamento de processo para ajudar a reiniciar o aplicativo Web

## <a name="prerequisites"></a>Pré-requisitos

1. Acesso a um Servidor Ubuntu 16.04 com uma conta de usuário padrão com privilégio sudo
2. Um aplicativo ASP.NET Core existente

## <a name="copy-over-your-app"></a>Copiar seu aplicativo

Executar `dotnet publish` do ambiente de desenvolvimento para empacotar um aplicativo em um diretório autocontido que pode ser executado no servidor.

Copie o aplicativo do ASP.NET Core para o servidor usando qualquer ferramenta (SCP, FTP, etc.) que se integre ao fluxo de trabalho. Teste o aplicativo, por exemplo:
 - Da linha de comando, execute `dotnet yourapp.dll`
 - Em um navegador, navegue até `http://<serveraddress>:<port>` para verificar se o aplicativo funciona no Linux. 
 
## <a name="configure-a-reverse-proxy-server"></a>Configurar um servidor proxy reverso

Um proxy reverso é uma configuração comum para atender a aplicativos Web dinâmicos. Um proxy reverso encerra a solicitação HTTP e a encaminha para o aplicativo ASP.NET Core.

### <a name="why-use-a-reverse-proxy-server"></a>Por que usar um servidor proxy reverso?

O Kestrel é ótimo para servir conteúdo dinâmico do ASP.NET Core; no entanto, as partes de atendimento à Web não são tão ricas em recursos quanto servidores como IIS, Apache ou Nginx. Um servidor proxy reverso pode descarregar trabalho como servir conteúdo estático, armazenar solicitações em cache, compactar solicitações e terminar SSL do servidor HTTP. Um servidor proxy reverso pode residir em um computador dedicado ou pode ser implantado junto com um servidor HTTP.

Para os fins deste guia, uma única instância de Nginx é usada. Ela é executada no mesmo servidor, junto com o servidor HTTP. De acordo com suas necessidades, você pode escolher uma configuração diferente.

Já que as solicitações são encaminhadas pelo proxy reverso, use o middleware `ForwardedHeaders` do pacote `Microsoft.AspNetCore.HttpOverrides`. Este middleware atualiza `Request.Scheme`, usando o cabeçalho `X-Forwarded-Proto`, de forma que URIs de redirecionamento e outras políticas de segurança funcionam corretamente.

Ao configurar um servidor proxy reverso, o middleware de autenticação precisa de `UseForwardedHeaders` para ser executado primeiro. Essa ordenação garantirá que o middleware de autenticação possa consumir os valores afetados e gerar URIs de redirecionamento corretos.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Invoque o método `UseForwardedHeaders` (no método `Configure` de *Startup.cs*) antes de chamar `UseAuthentication` ou outro middleware de esquema de autenticação semelhante:

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Invoque o método `UseForwardedHeaders` (no método `Configure` de *Startup.cs*) antes de chamar `UseIdentity` e `UseFacebookAuthentication` ou outro middleware de esquema de autenticação semelhante:

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

### <a name="install-nginx"></a>Instalar o Nginx

```bash
sudo apt-get install nginx
```

> [!NOTE]
> Se você planeja instalar os módulos de Nginx opcionais, pode ser solicitado que você compile o Nginx da origem.

Use `apt-get` para instalar o Nginx. O instalador cria um script de inicialização System V que executa o Nginx como daemon na inicialização do sistema. Já que Nginx foi instalado pela primeira vez, inicie-o explicitamente executando:

```bash
sudo service nginx start
```

Verifique se um navegador exibe a página de aterrissagem padrão do Nginx.

### <a name="configure-nginx"></a>Configurar o Nginx

Para configurar o Nginx como um proxy inverso para encaminhar solicitações para o nosso aplicativo ASP.NET Core, modifique `/etc/nginx/sites-available/default`. Abra-o em um editor de texto arquivo e substitua o conteúdo pelo mostrado a seguir:

```nginx
server {
    listen 80;
    location / {
        proxy_pass http://localhost:5000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection keep-alive;
        proxy_set_header Host $http_host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

Esse arquivo de configuração do Nginx encaminha tráfego público de entrada da porta `80` para a porta `5000`.

Depois que você terminar de fazer alterações em sua configuração de Nginx, você pode executar `sudo nginx -t` para verificar a sintaxe dos seus arquivos de configuração. Se o teste do arquivo de configuração for bem-sucedido, você poderá pedir ao Nginx que acompanhe as alterações executando `sudo nginx -s reload`.

## <a name="monitoring-our-application"></a>Monitorando nosso aplicativo

O Nginx agora está configurado para encaminhar solicitações feitas a `http://yourhost:80` ao aplicativo ASP.NET Core em execução no Kestrel em `http://127.0.0.1:5000`. No entanto, o Nginx não está configurado para gerenciar o processo do Kestrel. Você pode usar *systemd* e criar um arquivo de serviço para iniciar e monitorar o aplicativo Web subjacente. *systemd* é um sistema de inicialização que fornece muitos recursos poderosos para iniciar, parar e gerenciar processos. 

### <a name="create-the-service-file"></a>Criar o arquivo de serviço

Crie o arquivo de definição de serviço:

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

A seguir, um exemplo de arquivo de serviço para nosso aplicativo:

```ini
[Unit]
Description=Example .NET Web API Application running on Ubuntu

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

**Observação:** se o usuário *www-data* não é usado pela sua configuração, o usuário definido aqui deve ser criado primeiro e a propriedade adequada dos arquivos deve ser concedida a ele.

Salve o arquivo e habilite o serviço.

```bash
systemctl enable kestrel-hellomvc.service
```

Inicie o serviço e verifique se ele está em execução.

```
systemctl start kestrel-hellomvc.service
systemctl status kestrel-hellomvc.service

● kestrel-hellomvc.service - Example .NET Web API Application running on Ubuntu
    Loaded: loaded (/etc/systemd/system/kestrel-hellomvc.service; enabled)
    Active: active (running) since Thu 2016-10-18 04:09:35 NZDT; 35s ago
Main PID: 9021 (dotnet)
    CGroup: /system.slice/kestrel-hellomvc.service
            └─9021 /usr/local/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
```

Com o proxy reverso configurado e o Kestrel gerenciado por systemd, o aplicativo Web está totalmente configurado e pode ser acessado por meio de um navegador no computador local em `http://localhost`. Ele também é acessível por meio de um computador remoto, bloqueando qualquer firewall que o possa estar bloqueando. Inspecionando os cabeçalhos de resposta, o cabeçalho `Server` mostra o aplicativo do ASP.NET Core sendo servido por Kestrel.

```text
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a>Exibindo logs

Já que o aplicativo Web usando Kestrel é gerenciado usando `systemd`, todos os eventos e os processos são registrados em um diário centralizado. No entanto, esse diário contém todas as entradas para todos os serviços e processos gerenciados pelo `systemd`. Para exibir os itens específicos de `kestrel-hellomvc.service`, use o seguinte comando:

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

Para obter mais filtragem, opções de tempo como `--since today`, `--until 1 hour ago` ou uma combinação desses fatores pode reduzir a quantidade de entradas retornadas.

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-our-application"></a>Protegendo nosso aplicativo

### <a name="enable-apparmor"></a>Habilitar AppArmor

O LSM (Módulos de Segurança do Linux) é uma estrutura que é parte do kernel do Linux desde o Linux 2.6. O LSM dá suporte a diferentes implementações de módulos de segurança. O [AppArmor](https://wiki.ubuntu.com/AppArmor) é um LSM que implementa um sistema de controle de acesso obrigatório que permite restringir o programa a um conjunto limitado de recursos. Verifique se o AppArmor está habilitado e configurado corretamente.

### <a name="configuring-our-firewall"></a>Configurando nosso firewall

Feche todas as portas externas que não estão em uso. Firewall descomplicado (ufw) fornece um front-end para `iptables` fornecendo uma interface de linha de comando para configurar o firewall. Verifique se `ufw` está configurado para permitir o tráfego em quaisquer portas que você precise.

```bash
sudo apt-get install ufw
sudo ufw enable

sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
```

### <a name="securing-nginx"></a>Protegendo o Nginx

A distribuição padrão do Nginx não habilita o SSL. Para habilitar recursos de segurança adicionais, compile da origem.

#### <a name="download-the-source-and-install-the-build-dependencies"></a>Baixe a origem e instale as dependências de build

```bash
# Install the build dependencies
sudo apt-get update
sudo apt-get install build-essential zlib1g-dev libpcre3-dev libssl-dev libxslt1-dev libxml2-dev libgd2-xpm-dev libgeoip-dev libgoogle-perftools-dev libperl-dev

# Download nginx 1.10.0 or latest
wget http://www.nginx.org/download/nginx-1.10.0.tar.gz
tar zxf nginx-1.10.0.tar.gz
```

#### <a name="change-the-nginx-response-name"></a>Alterar o nome da resposta do Nginx

Edite *src/http/ngx_http_header_filter_module.c*:

```c
static char ngx_http_server_string[] = "Server: Your Web Server" CRLF;
static char ngx_http_server_full_string[] = "Server: Your Web Server" CRLF;
```

#### <a name="configure-the-options-and-build"></a>Configurar as opções e compilar

A biblioteca PCRE é necessária para expressões regulares. Expressões regulares são usadas na diretiva local para o ngx_http_rewrite_module. O http_ssl_module adiciona suporte a protocolo HTTPS.

Considere o uso de um firewall de aplicativo Web como *ModSecurity* para proteger seu aplicativo.

```bash
./configure
--with-pcre=../pcre-8.38
--with-zlib=../zlib-1.2.8
--with-http_ssl_module
--with-stream
--with-mail=dynamic
```

#### <a name="configure-ssl"></a>Configurar o SSL

* Configure o servidor para ouvir tráfego HTTPS na porta `443` especificando um certificado válido emitido por uma AC (autoridade de certificação) confiável.

* Aprimore sua segurança, empregando algumas das práticas descritas no arquivo */etc/nginx/nginx.conf* a seguir. Exemplos incluem a escolha de uma criptografia mais forte e o redirecionamento de todo o tráfego por meio de HTTP para HTTPS.

* Adicionar um cabeçalho `HTTP Strict-Transport-Security` (HSTS) garante que todas as solicitações subsequentes feitas pelo cliente sejam apenas via HTTPS.

* Não adicione o cabeçalho de Segurança de Transporte Estrito nem escolha um `max-age` apropriado se você planeja desabilitar o SSL no futuro.

Adicione o arquivo de configuração */etc/nginx/proxy.conf*:

[!code-nginx[Main](linuxproduction/proxy.conf)]

Edite o arquivo de configuração */etc/nginx/nginx.conf*. O exemplo contém ambas as seções `http` e `server` em um arquivo de configuração.

[!code-nginx[Main](../publishing/linuxproduction/nginx.conf?highlight=2)]

#### <a name="secure-nginx-from-clickjacking"></a>Proteger o Nginx de clickjacking
Clickjacking é uma técnica mal-intencionada para coletar cliques de um usuário infectado. O clickjacking engana a vítima (visitante) fazendo-a clicar em um site infectado. Use X-FRAME-OPTIONS para proteger o site.

Edite o arquivo *nginx.conf*:

```bash
sudo nano /etc/nginx/nginx.conf
```

Adicione a linha `add_header X-Frame-Options "SAMEORIGIN";` e salve o arquivo, depois reinicie o Nginx.

#### <a name="mime-type-sniffing"></a>Detecção de tipo MIME

Esse cabeçalho evita que a maioria dos navegadores faça detecção MIME de uma resposta distante do tipo de conteúdo declarado, visto que o cabeçalho instrui o navegador para não substituir o tipo de conteúdo de resposta. Com a opção `nosniff`, se o servidor informa que o conteúdo é "text/html", o navegador renderiza-a como "text/html".

Edite o arquivo *nginx.conf*:

```bash
sudo nano /etc/nginx/nginx.conf
```

Adicione a linha `add_header X-Content-Type-Options "nosniff";` e salve o arquivo, depois reinicie o Nginx.

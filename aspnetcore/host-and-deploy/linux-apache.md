---
title: Hospedar o ASP.NET Core no Linux com o Apache
description: Saiba como configurar o Apache como um servidor proxy reverso no CentOS para redirecionar o tráfego HTTP para um aplicativo do Web ASP.NET Core em execução no Kestrel.
author: spboyer
ms.author: spboyer
ms.custom: mvc
ms.date: 03/13/2018
uid: host-and-deploy/linux-apache
ms.openlocfilehash: c46c0aa578867ce306adc67a2e0d8b650b5fa5bd
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/26/2018
ms.locfileid: "36960886"
---
# <a name="host-aspnet-core-on-linux-with-apache"></a>Hospedar o ASP.NET Core no Linux com o Apache

Por [Shayne Boyer](https://github.com/spboyer)

Usando este guia, aprenda como configurar o [Apache](https://httpd.apache.org/) como um servidor proxy reverso no [CentOS 7](https://www.centos.org/) para redirecionar o tráfego HTTP para um aplicativo Web ASP.NET Core em execução no [Kestrel](xref:fundamentals/servers/kestrel). A [extensão mod_proxy](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html) e os módulos relacionados criam o proxy reverso do servidor.

## <a name="prerequisites"></a>Pré-requisitos

1. Servidor que executa o CentOS 7 com uma conta de usuário padrão com privilégio sudo.
1. Instale o tempo de execução do .NET Core no servidor.
   1. Acesse a [página Todos os Downloads do .NET Core](https://www.microsoft.com/net/download/all).
   1. Selecione o tempo de execução não de versão prévia mais recente da lista em **Tempo de Execução**.
   1. Selecione e siga as instruções para CentOS/Oracle.
1. Um aplicativo ASP.NET Core existente.

## <a name="publish-and-copy-over-the-app"></a>Publicar e copiar o aplicativo

Configurar o aplicativo para um [implantação dependente de estrutura](/dotnet/core/deploying/#framework-dependent-deployments-fdd).

Execute [dotnet publish](/dotnet/core/tools/dotnet-publish) do ambiente de desenvolvimento para empacotar um aplicativo em um diretório (por exemplo, *bin/Release/&lt;target_framework_moniker&gt;/publish*) que pode ser executado no servidor:

```console
dotnet publish --configuration Release
```

O aplicativo também poderá ser publicado como uma [implantação autossuficiente](/dotnet/core/deploying/#self-contained-deployments-scd) se você preferir não manter o tempo de execução do .NET Core no servidor.

Copie o aplicativo ASP.NET Core para o servidor usando uma ferramenta que se integre ao fluxo de trabalho da organização (por exemplo, SCP, SFTP). É comum para localizar os aplicativos Web no diretório *var* (por exemplo, *aspnetcore/var/hellomvc*).

> [!NOTE]
> Em um cenário de implantação de produção, um fluxo de trabalho de integração contínua faz o trabalho de publicar o aplicativo e copiar os ativos para o servidor.

## <a name="configure-a-proxy-server"></a>Configurar um servidor proxy

Um proxy reverso é uma configuração comum para atender a aplicativos Web dinâmicos. O proxy reverso encerra a solicitação HTTP e a encaminha para o aplicativo ASP.NET.

Um servidor proxy é aquele que encaminha as solicitações de cliente para outro servidor, em vez de atendê-las por conta própria. Um proxy reverso encaminha para um destino fixo, normalmente, em nome de clientes arbitrários. Neste guia, o Apache é configurado como o proxy reverso em execução no mesmo servidor em que o Kestrel está servindo o aplicativo ASP.NET Core.

Uma vez que as solicitações são encaminhadas pelo proxy reverso, use o [Middleware de Cabeçalhos Encaminhados](xref:host-and-deploy/proxy-load-balancer) do pacote [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/). O middleware atualiza o `Request.Scheme` usando o cabeçalho `X-Forwarded-Proto`, de forma que URIs de redirecionamento e outras políticas de segurança funcionam corretamente.

Qualquer componente que dependa do esquema, como autenticação, geração de link, redirecionamentos e localização geográfica, deverá ser colocado depois de invocar o Middleware de Cabeçalhos Encaminhados. Como regra geral, o Middleware de Cabeçalhos Encaminhados deve ser executado antes de outro middleware, exceto middleware de tratamento de erro e de diagnóstico. Essa ordenação garantirá que o middleware conte com informações de cabeçalhos encaminhadas que podem consumir os valores de cabeçalho para processamento.

::: moniker range=">= aspnetcore-2.0"
> [!NOTE]
> Qualquer configuração &mdash;com ou sem um servidor proxy reverso&mdash; é uma configuração de hospedagem válida e compatível com o ASP.NET Core 2.0 ou aplicativos posteriores. Para obter mais informações, consulte [Quando usar Kestrel com um proxy reverso](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).
::: moniker-end

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Invoque o método [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) em `Startup.Configure` antes de chamar [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) ou um middleware de esquema de autenticação semelhante. Configure o middleware para encaminhar os cabeçalhos `X-Forwarded-For` e `X-Forwarded-Proto`:

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Invoque o método [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) em `Startup.Configure` antes de chamar [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) e [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) ou um middleware de esquema de autenticação semelhante. Configure o middleware para encaminhar os cabeçalhos `X-Forwarded-For` e `X-Forwarded-Proto`:

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

Se nenhum [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) for especificado para o middleware, os cabeçalhos padrão para encaminhar serão `None`.

Configuração adicional pode ser necessária para aplicativos hospedados atrás de servidores proxy e balanceadores de carga. Para obter mais informações, veja [Configurar o ASP.NET Core para trabalhar com servidores proxy e balanceadores de carga](xref:host-and-deploy/proxy-load-balancer).

### <a name="install-apache"></a>Instalar o Apache

Atualize os pacotes CentOS para as respectivas versões estáveis mais recentes:

```bash
sudo yum update -y
```

Instale o servidor Web do Apache no CentOS com um único comando `yum`:

```bash
sudo yum -y install httpd mod_ssl
```

Saída de exemplo depois de executar o comando:

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
> Neste exemplo, o resultado reflete httpd.86_64, pois a versão do CentOS 7 é de 64 bits. Para verificar o local em que o Apache está instalado, execute `whereis httpd` em um prompt de comando.

### <a name="configure-apache"></a>Configurar o Apache

Os arquivos de configuração do Apache estão localizados no diretório `/etc/httpd/conf.d/`. Qualquer arquivo com a extensão *.conf* é processado em ordem alfabética, além dos arquivos de configuração do módulo em `/etc/httpd/conf.modules.d/`, que contém todos os arquivos de configuração necessários para carregar os módulos.

Crie um arquivo de configuração chamado *hellomvc.conf* para o aplicativo:

```
<VirtualHost *:*>
    RequestHeader set "X-Forwarded-Proto" expr=%{REQUEST_SCHEME}
</VirtualHost>

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

O bloco `VirtualHost` pode aparecer várias vezes, em um ou mais arquivos em um servidor. No arquivo de configuração anterior, o Apache aceita tráfego público na porta 80. O domínio `www.example.com` está sendo atendido e o alias `*.example.com` é resolvido para o mesmo site. Veja [Suporte a host virtual baseado em nome](https://httpd.apache.org/docs/current/vhosts/name-based.html) para obter mais informações. As solicitações passadas por proxy na raiz para a porta 5000 do servidor em 127.0.0.1. Para a comunicação bidirecional, `ProxyPass` e `ProxyPassReverse` são necessários.

> [!WARNING]
> Falha ao especificar uma [diretiva ServerName](https://httpd.apache.org/docs/current/mod/core.html#servername) no bloco **VirtualHost** expõe seu aplicativo para vulnerabilidades de segurança. Associações de curinga de subdomínio (por exemplo, `*.example.com`) não oferecerão esse risco de segurança se você controlar o domínio pai completo (em vez de `*.com`, o qual é vulnerável). Veja [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) para obter mais informações.

O registro em log pode ser configurado por `VirtualHost` usando diretivas `ErrorLog` e `CustomLog`. `ErrorLog` é o local em que o servidor registrará em log os erros, enquanto `CustomLog` define o nome de arquivo e o formato do arquivo de log. Nesse caso, esse é o local em que as informações de solicitação são registradas em log. Há uma linha para cada solicitação.

Salve o arquivo e teste a configuração. Se tudo passar, a resposta deverá ser `Syntax [OK]`.

```bash
sudo service httpd configtest
```

Reinicie o Apache:

```bash
sudo systemctl restart httpd
sudo systemctl enable httpd
```

## <a name="monitoring-the-app"></a>Monitoramento o aplicativo

O Apache agora está configurado para encaminhar solicitações feitas a `http://localhost:80` para o aplicativo ASP.NET Core em execução no Kestrel em `http://127.0.0.1:5000`.  No entanto, o Apache não está configurado para gerenciar o processo do Kestrel. Use *systemd* e crie um arquivo de serviço para iniciar e monitorar o aplicativo Web subjacente. *systemd* é um sistema de inicialização que fornece muitos recursos poderosos para iniciar, parar e gerenciar processos. 

### <a name="create-the-service-file"></a>Criar o arquivo de serviço

Crie o arquivo de definição de serviço:

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

Eis um exemplo de arquivo de serviço para o aplicativo:

```
[Unit]
Description=Example .NET Web API App running on CentOS 7

[Service]
WorkingDirectory=/var/aspnetcore/hellomvc
ExecStart=/usr/local/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
Restart=always
# Restart service after 10 seconds if the dotnet service crashes:
RestartSec=10
SyslogIdentifier=dotnet-example
User=apache
Environment=ASPNETCORE_ENVIRONMENT=Production 

[Install]
WantedBy=multi-user.target
```

> [!NOTE]
> **Usuário** &mdash; Se o usuário *apache* não é usado pela configuração, o usuário deve ser criado primeiro e a propriedade adequada dos arquivos deve ser concedida a ele.

> [!NOTE]
> Alguns valores (por exemplo, cadeias de conexão de SQL) devem ser escapadas para que os provedores de configuração leiam as variáveis de ambiente. Use o seguinte comando para gerar um valor corretamente com caracteres de escape para uso no arquivo de configuração:
>
> ```console
> systemd-escape "<value-to-escape>"
> ```

Salve o arquivo e habilite o serviço:

```bash
systemctl enable kestrel-hellomvc.service
```

Inicie o serviço e verifique se ele está em execução:

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

Com o proxy reverso configurado e o Kestrel gerenciado por meio de *systemd*, o aplicativo Web está totalmente configurado e pode ser acessado em um navegador no computador local em `http://localhost`. Inspecionando os cabeçalhos de resposta, o cabeçalho **Server** indica que o aplicativo ASP.NET Core é servido pelo Kestrel:

```
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a>Exibindo logs

Já que o aplicativo Web usando Kestrel é gerenciado usando *systemd*, os eventos e os processos são registrados em um diário centralizado. No entanto, esse diário contém todas as entradas para todos os serviços e processos gerenciados por *systemd*. Para exibir os itens específicos de `kestrel-hellomvc.service`, use o seguinte comando:

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

Para filtragem de hora, especifique opções de tempo com o comando. Por exemplo, use `--since today` para filtrar o dia atual ou `--until 1 hour ago` para ver as entradas da hora anterior. Para obter mais informações, confira a [página de manual para journalctl](https://www.unix.com/man-page/centos/1/journalctl/).

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-the-app"></a>Protegendo o aplicativo

### <a name="configure-firewall"></a>Configurar o firewall

*Firewalld* é um daemon dinâmico para gerenciar o firewall com suporte para zonas de rede. Portas e filtragem de pacote ainda podem ser gerenciados pelo iptables. *Firewalld* deve ser instalado por padrão. `yum` pode ser usado para instalar o pacote ou verificar se ele está instalado.

```bash
sudo yum install firewalld -y
```

Use `firewalld` para abrir somente as portas necessárias para o aplicativo. Nesse caso, as portas 80 e 443 são usadas. Os comandos a seguir definem permanentemente as portas 80 e 443 para estarem abertas:

```bash
sudo firewall-cmd --add-port=80/tcp --permanent
sudo firewall-cmd --add-port=443/tcp --permanent
```

Recarregue as configurações de firewall. Verifique os serviços e as portas disponíveis na zona padrão. As opções estão disponíveis por meio da inspeção de `firewall-cmd -h`.

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

### <a name="ssl-configuration"></a>Configuração do SSL

Para configurar o Apache para SSL, o módulo *mod_ssl* é usado. Quando o módulo *httpd* foi instalado, o módulo *mod_ssl* também foi instalado. Se ele não foi instalado, use `yum` para adicioná-lo à configuração.

```bash
sudo yum install mod_ssl
```

Para impor o SSL, instale o módulo `mod_rewrite` para habilitar a regravação de URL:

```bash
sudo yum install mod_rewrite
```

Modifique o arquivo *hellomvc.conf* para habilitar a regravação de URL e proteger a comunicação na porta 443:

```
<VirtualHost *:*>
    RequestHeader set "X-Forwarded-Proto" expr=%{REQUEST_SCHEME}
</VirtualHost>

<VirtualHost *:80>
    RewriteEngine On
    RewriteCond %{HTTPS} !=on
    RewriteRule ^/?(.*) https://%{SERVER_NAME}/$1 [R,L]
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
> Este exemplo usa um certificado gerado localmente. **SSLCertificateFile** deve ser o arquivo de certificado primário para o nome de domínio. **SSLCertificateKeyFile** deve ser o arquivo de chave gerado quando o CSR é criado. **SSLCertificateChainFile** deve ser o arquivo de certificado intermediário (se houver) fornecido pela autoridade de certificação.

Salve o arquivo e teste a configuração:

```bash
sudo service httpd configtest
```

Reinicie o Apache:

```bash
sudo systemctl restart httpd
```

## <a name="additional-apache-suggestions"></a>Sugestões adicionais do Apache

### <a name="additional-headers"></a>Cabeçalhos adicionais

Para proteção contra ataques mal-intencionados, há alguns cabeçalhos que devem ser modificados ou adicionados. Verifique se o módulo `mod_headers` está instalado:

```bash
sudo yum install mod_headers
```

#### <a name="secure-apache-from-clickjacking-attacks"></a>Proteger o Apache contra ataques de clickjacking

[Clickjacking](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), também conhecido como um *ataque por inferência na interface do usuário*, é um ataque mal-intencionado em que o visitante do site é levado a clicar em um link ou botão em uma página diferente daquela que está visitando atualmente. Use `X-FRAME-OPTIONS` para proteger o site.

Edite o arquivo *httpd.conf*:

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

Adicione a linha `Header append X-FRAME-OPTIONS "SAMEORIGIN"`. Salve o arquivo. Reinicie o Apache.

#### <a name="mime-type-sniffing"></a>Detecção de tipo MIME

O cabeçalho `X-Content-Type-Options` impedirá o Internet Explorer de *farejar por MIME* (determinar o `Content-Type` de um arquivo com base no conteúdo do arquivo). Se o servidor define o cabeçalho `Content-Type` para `text/html` com a opção `nosniff` definida, o Internet Explorer renderiza o conteúdo como `text/html`, independentemente do conteúdo do arquivo.

Edite o arquivo *httpd.conf*:

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

Adicione a linha `Header set X-Content-Type-Options "nosniff"`. Salve o arquivo. Reinicie o Apache.

### <a name="load-balancing"></a>Balanceamento de carga

Este exemplo mostra como instalar e configurar o Apache no CentOS 7 e no Kestrel no mesmo computador da instância. Para não ter um ponto único de falha, o uso de *mod_proxy_balancer* e a modificação do **VirtualHost** permitiriam o gerenciamento de várias instâncias dos aplicativos Web protegidos pelo servidor proxy do Apache.

```bash
sudo yum install mod_proxy_balancer
```

No arquivo de configuração mostrado abaixo, uma instância adicional do aplicativo `hellomvc` está configurada para ser executada na porta 5001. A seção *Proxy* é definida com uma configuração de balanceador com dois membros para balancear carga de *byrequests*.

```
<VirtualHost *:*>
    RequestHeader set "X-Forwarded-Proto" expr=%{REQUEST_SCHEME}
</VirtualHost>

<VirtualHost *:80>
    RewriteEngine On
    RewriteCond %{HTTPS} !=on
    RewriteRule ^/?(.*) https://%{SERVER_NAME}/$1 [R,L]
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

### <a name="rate-limits"></a>Limites de taxa

Usando *mod_ratelimit*, que está incluído no módulo *httpd*, a largura de banda de clientes pode ser limitada:

```bash
sudo nano /etc/httpd/conf.d/ratelimit.conf
```
O arquivo de exemplo limita a largura de banda a 600 KB/s no local raiz:

```
<IfModule mod_ratelimit.c>
    <Location />
        SetOutputFilter RATE_LIMIT
        SetEnv rate-limit 600
    </Location>
</IfModule>
```

## <a name="additional-resources"></a>Recursos adicionais

* [Configurar o ASP.NET Core para trabalhar com servidores proxy e balanceadores de carga](xref:host-and-deploy/proxy-load-balancer)

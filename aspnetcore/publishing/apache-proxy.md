---
title: Hospedar o ASP.NET Core no Linux com o Apache
description: "Saiba como configurar o Apache como um servidor proxy reverso no CentOS para redirecionar o tráfego HTTP para um aplicativo Web ASP.NET Core em execução no Kestrel."
keywords: ASP.NET Core,Apache,CentOS,Reverse Proxy,Linux,mod_proxy,httpd,hospedagem
author: spboyer
ms.author: spboyer
manager: wpickett
ms.date: 10/19/2016
ms.topic: article
ms.assetid: fa9b0cb7-afb3-4361-9e7e-33afffeaca0c
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/apache-proxy
ms.openlocfilehash: 34ede2fdebe0e9516f62e39618e7adba3c8c89db
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/22/2017
---
# <a name="set-up-a-hosting-environment-for-aspnet-core-on-linux-with-apache-and-deploy-to-it"></a>Configurar um ambiente de hospedagem para o ASP.NET Core no Linux com o Apache e implantar nele

Por [Shayne Boyer](https://github.com/spboyer)

O Apache é um servidor HTTP muito popular e pode ser configurado como um proxy para redirecionar o tráfego HTTP de forma semelhante ao Nginx. Neste guia, aprenderemos a configurar o Apache no CentOS 7 e usá-lo como um proxy reverso para receber conexões de entrada e redirecioná-las para o aplicativo ASP.NET Core em execução no Kestrel. Para essa finalidade, usaremos a extensão *mod_proxy* e outros módulos do Apache relacionados.

## <a name="prerequisites"></a>Pré-requisitos

1. Um servidor que executa o CentOS 7, com uma conta de usuário padrão com privilégios sudo.
2. Um aplicativo ASP.NET Core existente. 

## <a name="publish-your-application"></a>Publicar o aplicativo

Execute `dotnet publish -c Release` no ambiente de desenvolvimento para empacotar o aplicativo em um diretório independente que pode ser executado no servidor. Em seguida, o aplicativo publicado deve ser copiado para o servidor usando o SCP, FTP ou outro método de transferência de arquivo. 

> [!NOTE]
> Em um cenário de implantação de produção, um fluxo de trabalho de integração contínua faz o trabalho de publicar o aplicativo e copiar os ativos para o servidor. 

## <a name="configure-a-proxy-server"></a>Configurar um servidor proxy

Um proxy reverso é uma configuração comum para atender a aplicativos Web dinâmicos. O proxy reverso encerra a solicitação HTTP e a encaminha para o aplicativo ASP.NET.

Um servidor proxy é aquele que encaminha as solicitações de cliente para outro servidor, em vez de atendê-las por conta própria. Um proxy reverso encaminha para um destino fixo, normalmente, em nome de clientes arbitrários. Neste guia, o Apache está sendo configurado como o proxy reverso em execução no mesmo servidor em que o Kestrel está atendendo o aplicativo ASP.NET Core. 

Cada parte do aplicativo pode existir em computadores físicos separados, contêineres do Docker ou em uma combinação de configurações, dependendo de suas necessidades ou restrições de arquitetura.

### <a name="install-apache"></a>Instalar o Apache

A instalação do servidor Web Apache no CentOS é feita com um único comando, mas primeiro vamos atualizar nossos pacotes.

```bash
    sudo yum update -y
```

Isso garante que todos os pacotes instalados são atualizados para sua última versão. Instalar o Apache usando `yum`

```bash
    sudo yum -y install httpd mod_ssl
```

O resultado deve refletir algo semelhante ao mostrado a seguir.

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
> Neste exemplo, o resultado reflete httpd.86_64, pois a versão do CentOS 7 é de 64 bits. O resultado pode ser diferente para seu servidor. Para verificar o local em que o Apache está instalado, execute `whereis httpd` em um prompt de comando. 

### <a name="configure-apache-for-reverse-proxy"></a>Configurar o Apache para proxy reverso

Os arquivos de configuração do Apache estão localizados no diretório `/etc/httpd/conf.d/`. Qualquer arquivo com a extensão **.conf** será processado em ordem alfabética, além dos arquivos de configuração do módulo em `/etc/httpd/conf.modules.d/`, que contém todos os arquivos de configuração necessários para carregar os módulos.

Crie um arquivo de configuração para seu aplicativo; neste exemplo, vamos chamá-lo `hellomvc.conf`

```text
    <VirtualHost *:80>
        ProxyPreserveHost On
        ProxyPass / http://127.0.0.1:5000/
        ProxyPassReverse / http://127.0.0.1:5000/
        ErrorLog /var/log/httpd/hellomvc-error.log
        CustomLog /var/log/httpd/hellomvc-access.log common
    </VirtualHost>
```

O nó *VirtualHost*, que pode haver vários em um arquivo ou em um servidor em vários arquivos, é definido para escutar em qualquer endereço IP usando a porta 80. As duas próximas linhas são definidas para passar todas as solicitações recebidas na raiz para o computador 127.0.0.1 na porta 5000 e na ordem inversa. Para que haja comunicação bidirecional, ambas as configurações *ProxyPass* e *ProxyPassReverse* são necessárias.

O log pode ser configurado por VirtualHost usando as diretivas *ErrorLog* e *CustomLog*. *ErrorLog* é o local em que o servidor registrará os erros e *CustomLog* define o nome de arquivo e o formato do arquivo de log. Em nosso caso, esse é o local em que as informações de solicitação serão registradas. Haverá uma linha para cada solicitação.

Salve o arquivo e teste a configuração. Se tudo passar, a resposta deverá ser `Syntax [OK]`.

```bash
    sudo service httpd configtest
```

Reinicie o Apache.

```text
    sudo systemctl restart httpd
    sudo systemctl enable httpd
```

## <a name="monitoring-our-application"></a>Monitorando nosso aplicativo

O Apache agora está configurado para encaminhar solicitações feitas a `http://localhost:80` para o aplicativo ASP.NET Core em execução no Kestrel em `http://127.0.0.1:5000`.  No entanto, o Apache não está configurado para gerenciar o processo do Kestrel. Usaremos *systemd* e criaremos um arquivo de serviço para iniciar e monitorar o aplicativo Web subjacente. *systemd* é um sistema de inicialização que fornece muitos recursos avançados para iniciar, interromper e gerenciar processos. 


### <a name="create-the-service-file"></a>Criar o arquivo de serviço

Criar o arquivo de definição de serviço 

```bash
    sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

Um arquivo de serviço de exemplo para nosso aplicativo.

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
> **Usuário** – se o usuário *apache* não é usado pela configuração, o usuário definido aqui deve ser criado primeiro e a propriedade adequada dos arquivos deve ser concedida a ele

Salve o arquivo e habilite o serviço.

```bash
    systemctl enable kestrel-hellomvc.service
```

Inicie o serviço e verifique se ele está em execução.

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

Com o proxy reverso configurado e o Kestrel gerenciado por meio de systemd, o aplicativo Web está totalmente configurado e pode ser acessado em um navegador no computador local em `http://localhost`. Inspecionando os cabeçalhos de resposta, o **Server** ainda mostra o aplicativo ASP.NET Core sendo atendido pelo Kestrel.

```text
    HTTP/1.1 200 OK
    Date: Tue, 11 Oct 2016 16:22:23 GMT
    Server: Kestrel
    Keep-Alive: timeout=5, max=98
    Connection: Keep-Alive
    Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a>Exibindo logs

Já que o aplicativo Web que usa o Kestrel é gerenciado usando systemd, todos os eventos e processos são registrados em um diário centralizado. No entanto, esse diário contém todas as entradas para todos os serviços e processos gerenciados pelo systemd. Para exibir os itens específicos de `kestrel-hellomvc.service`, use o comando a seguir.

```bash
    sudo journalctl -fu kestrel-hellomvc.service
```

Para obter mais filtragem, opções de hora como `--since today`, `--until 1 hour ago` ou uma combinação delas, pode reduzir a quantidade de entradas retornadas.

```bash
    sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-our-application"></a>Protegendo nosso aplicativo

### <a name="configure-firewall"></a>Configurar o firewall

*Firewalld* é um daemon dinâmico para gerenciar o firewall com suporte para zonas de rede, embora ainda seja possível usar iptables para gerenciar portas e a filtragem de pacotes. Firewalld deve ser instalado por padrão e `yum` pode ser usado para instalar o pacote ou verificar.

```bash
    sudo yum install firewalld -y
```

Usando `firewalld`, você pode abrir somente as portas necessárias para o aplicativo. Nesse caso, as portas 80 e 443 são usadas. Os comandos a seguir definem permanentemente essas portas para estarem abertas.

```bash
    sudo firewall-cmd --add-port=80/tcp --permanent
    sudo firewall-cmd --add-port=443/tcp --permanent
```

Recarregue as configurações do firewall e verifique os serviços e as portas disponíveis na zona padrão. As opções estão disponíveis por meio da inspeção de `firewall-cmd -h`

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

Para configurar o Apache para SSL, o módulo mod_ssl é usado.  Isso foi instalado inicialmente quando instalamos o módulo `httpd`. Se ele não foi incluído ou instalado, use o yum para adicioná-lo à configuração.

```bash
    sudo yum install mod_ssl
```
Para impor o SSL, instale `mod_rewrite`

```bash
    sudo yum install mod_rewrite
```

O arquivo `hellomvc.conf` criado para este exemplo precisa ser modificado para permitir a reescrita, bem como a adição da nova seção **VirtualHost** para HTTPS.

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
> Este exemplo usa um certificado gerado localmente. **SSLCertificateFile** deve ser o arquivo de certificado primário para o nome de domínio. **SSLCertificateKeyFile** deve ser o arquivo de chave gerado quando o CSR foi criado. **SSLCertificateChainFile** deve ser o arquivo de certificado intermediário (se houver) fornecido pela autoridade de certificação

Salve o arquivo e teste a configuração.

```bash
    sudo service httpd configtest
```

Reinicie o Apache.

```bash
    sudo systemctl restart httpd
```

## <a name="additional-apache-suggestions"></a>Sugestões adicionais do Apache

### <a name="additional-headers"></a>Cabeçalhos adicionais 
Para proteção contra ataques mal-intencionados, há alguns cabeçalhos que devem ser modificados ou adicionados. Verifique se o módulo `mod_headers` está instalado.

```bash
    sudo yum install mod_headers
```

#### <a name="secure-apache-from-clickjacking"></a>Proteger o Apache contra clickjacking
Clickjacking é uma técnica mal-intencionada usada para coletar os cliques de um usuário infectado. O clickjacking engana a vítima (visitante) fazendo-a clicar em um site infectado. Use X-FRAME-OPTIONS para proteger o site.

Edite o arquivo httpd.conf.

```bash
    sudo nano /etc/httpd/conf/httpd.conf
```

Adicione a linha `Header append X-FRAME-OPTIONS "SAMEORIGIN"` e salve o arquivo e, depois, reinicie o Apache.

#### <a name="mime-type-sniffing"></a>Detecção de tipo MIME

Esse cabeçalho previne que o Internet Explorer faça uma detecção MIME de uma resposta distante do tipo de conteúdo declarado, já que o cabeçalho instrui o navegador a não substituir o tipo de conteúdo de resposta. Com a opção nosniff, se o servidor informar que o conteúdo é text/html, o navegador a renderizará como text/html.

Edite o arquivo httpd.conf.

```bash
    sudo nano /etc/httpd/conf/httpd.conf
```

Adicione a linha `Header set X-Content-Type-Options "nosniff"` e salve o arquivo e, depois, reinicie o Apache.

### <a name="load-balancing"></a>Balanceamento de carga 

Este exemplo mostra como instalar e configurar o Apache no CentOS 7 e no Kestrel no mesmo computador da instância.  No entanto, para não ter um ponto único de falha, o uso de *mod_proxy_balancer* e a modificação do VirtualHost permitirão o gerenciamento de várias instâncias dos aplicativos Web protegidos pelo servidor proxy do Apache.

```bash
    sudo yum install mod_proxy_balancer
```

No arquivo de configuração, uma instância adicional do aplicativo `hellomvc` foi configurada para ser executada na porta 5001 e a seção *Proxy* foi definida com uma configuração de balanceador com dois membros para balancear a carga de *byrequests*.

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

### <a name="rate-limits"></a>Limites de taxa
Usando `mod_ratelimit`, que está incluído no módulo `htttpd`, você pode limitar a quantidade de largura de banda de clientes. 

```bash
    sudo nano /etc/httpd/conf.d/ratelimit.conf
```
O arquivo de exemplo limita a largura de banda a 600 KB/s no local raiz.

```text
    <IfModule mod_ratelimit.c>
        <Location />
            SetOutputFilter RATE_LIMIT
            SetEnv rate-limit 600
        </Location>
    </IfModule>
```

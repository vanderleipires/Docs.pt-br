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
ms.sourcegitcommit: 732cd2684246e49e796836596643a8d37e20c46d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/01/2017
---
# <a name="weblistener-web-server-implementation-in-aspnet-core"></a>Implementação do servidor web WebListener no núcleo do ASP.NET

Por [Tom Dykstra](https://github.com/tdykstra) e [Ross Carlos](https://github.com/Tratcher)

> [!NOTE]
> Este tópico se aplica somente ao ASP.NET Core 1. x. No ASP.NET 2.0 de núcleo, chamado WebListener [HTTP.sys](httpsys.md).

WebListener é um [servidor web para o ASP.NET Core](index.md) que é executado somente no Windows. Ele é criado no [driver de modo kernel HTTP. sys](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx). WebListener é uma alternativa ao [Kestrel](kestrel.md) que pode ser usado para conexão direta com a Internet sem depender de IIS como um servidor de proxy reverso. Na verdade, **WebListener não pode ser usado com o IIS ou IIS Express, pois ele não é compatível com o [ASP.NET Core módulo](aspnet-core-module.md).**

Embora WebListener foi desenvolvido para o ASP.NET Core, ele pode ser usado diretamente em qualquer aplicativo .NET Core ou do .NET Framework por meio de [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) pacote NuGet.

WebListener suporta os seguintes recursos:

- [Autenticação do Windows](xref:security/authentication/windowsauth)
- Compartilhamento de porta
- HTTPS com SNI
- HTTP/2 por TLS (Windows 10)
- Transmissão de arquivo direto
- O cache de resposta
- WebSocket (Windows 8)

Versões com suporte do Windows:

- Windows 7 e Windows Server 2008 R2 e posterior

[Exibir ou baixar o código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))

## <a name="when-to-use-weblistener"></a>Quando usar WebListener

WebListener é útil para implantações em que você precisa expor o servidor diretamente à Internet sem usar o IIS.

![O WebListener se comunica diretamente com a Internet](weblistener/_static/weblistener-to-internet.png)

Como ele se baseia no HTTP. sys, WebListener não requer um servidor de proxy reverso para proteção contra ataques. Http. sys é uma tecnologia desenvolvida que protege contra vários tipos de ataques e fornece a robustez, segurança e escalabilidade de um servidor web completo. O próprio IIS é executado como um ouvinte HTTP sobre HTTP. sys. 

WebListener também é uma boa escolha para implantações internas quando você precisa de um dos recursos que você não pode obter usando Kestrel oferece.

![O WebListener se comunica diretamente com a rede interna](weblistener/_static/weblistener-to-internal.png)

## <a name="how-to-use-weblistener"></a>Como usar WebListener

Aqui está uma visão geral das tarefas de configuração para o sistema operacional do host e seu aplicativo ASP.NET Core.

### <a name="configure-windows-server"></a>Configurar o Windows Server

* Instale a versão do .NET que seu aplicativo requer, como [.NET Core](https://download.microsoft.com/download/0/A/3/0A372822-205D-4A86-BFA7-084D2CBE9EDF/DotNetCore.1.0.1-SDK.1.0.0.Preview2-003133-x64.exe) ou .NET Framework 4.5.1.

* Pré-registrar prefixos de URL para vincular a WebListener e configurar certificados SSL

   Se você não pré-registrar prefixos de URL no Windows, você precisa executar seu aplicativo com privilégios de administrador. A única exceção é se você associar ao localhost usando HTTP (não HTTPS) com um número de porta maior que 1024; Nesse caso privilégios de administrador não são necessários.

   Para obter detalhes, consulte [como pré-registrar prefixos e configurar o SSL](#preregister-url-prefixes-and-configure-ssl) posteriormente neste artigo.

* Abrir portas de firewall para permitir o tráfego alcançar WebListener.

   Você pode usar netsh.exe ou [cmdlets do PowerShell](https://technet.microsoft.com/library/jj554906).

Também há [configurações de registro Http.Sys](https://support.microsoft.com/kb/820129).

### <a name="configure-your-aspnet-core-application"></a>Configurar seu aplicativo ASP.NET Core

* Instale o pacote NuGet [Microsoft.AspNetCore.Server.WebListener](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.WebListener/). Isso também instala [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) como uma dependência.

* Chamar o `UseWebListener` método de extensão no [WebHostBuilder](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilder) no seu `Main` método, especificar qualquer WebListener [opções](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.AspNetCore.Server.WebListener/WebListenerOptions.cs) e [configurações](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.Net.Http.Server/WebListenerSettings.cs) que você precisa , conforme mostrado no exemplo a seguir:

  [!code-csharp[](weblistener/sample/Program.cs?name=snippet_Main&highlight=13-17)]

* Configurar URLs e portas para escutar em 

  Por padrão o ASP.NET Core associa a `http://localhost:5000`. Para configurar portas e prefixos de URL, você pode usar o `UseURLs` método de extensão, o `urls` argumento de linha de comando ou o sistema de configuração do ASP.NET Core. Para obter mais informações, consulte [hospedagem](../../fundamentals/hosting.md).

  Web usa ouvinte a [formatos de cadeia de caracteres de prefixo de HTTP. sys](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx). Não há nenhum requisitos de formato de cadeia de caracteres de prefixo que são específicos para WebListener.

  > [!NOTE]
  > Certifique-se de que você especificar as cadeias de caracteres de prefixo mesmo em `UseUrls` que pré-registrar no servidor. 

* Verifique se que seu aplicativo não está configurado para executar o IIS ou IIS Express.

  No Visual Studio, o perfil de inicialização padrão é para o IIS Express.  Para executar o projeto como um aplicativo de console você precisa alterar manualmente o perfil selecionado, conforme mostrado na captura de tela a seguir.

  ![Selecione o perfil de aplicativo de console](weblistener/_static/vs-choose-profile.png)

## <a name="how-to-use-weblistener-outside-of-aspnet-core"></a>Como usar WebListener fora do ASP.NET Core

* Instalar o [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) pacote NuGet.

* [Pré-registrar prefixos de URL para vincular a WebListener e configurar certificados SSL](#preregister-url-prefixes-and-configure-ssl) como você faria para uso em ASP.NET Core.

Também há [configurações de registro Http.Sys](https://support.microsoft.com/kb/820129).


Aqui está um exemplo de código que demonstra o uso de WebListener fora do ASP.NET Core:

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

## <a name="preregister-url-prefixes-and-configure-ssl"></a>Pré-registrar prefixos de URL e configurar o SSL

IIS e WebListener contam com o driver de modo de kernel HTTP. sys subjacente para escutar solicitações e processamento inicial. No IIS, a interface do usuário de gerenciamento fornece uma maneira relativamente fácil de configurar tudo. No entanto, se você estiver usando WebListener, você precisa configurar o HTTP. sys por conta própria. A ferramenta interna para fazer isso é netsh.exe. 

As tarefas mais comuns que você precisa usar netsh.exe para são reservar prefixos de URL e a atribuição de certificados SSL.

NetSh.exe não é uma ferramenta fácil de usar para iniciantes. O exemplo a seguir mostra o mínimo necessário para reservar os prefixos de URL para as portas 80 e 443:

```console
netsh http add urlacl url=http://+:80/ user=Users
netsh http add urlacl url=https://+:443/ user=Users
```

O exemplo a seguir mostra como atribuir um certificado SSL:

```console
netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid={00000000-0000-0000-0000-000000000000}".
```

Aqui está a documentação oficial:

* [Comandos Netsh para Hypertext Transfer protocolo (HTTP)](https://technet.microsoft.com/library/cc725882.aspx)
* [Cadeias de caracteres de UrlPrefix](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

Os recursos a seguir fornecem instruções detalhadas para vários cenários. Os artigos que fazem referência a `HttpListener` se aplicam igualmente ao `WebListener`, pois ambas se baseiam em Http.Sys.

* [Como: configurar uma porta com um certificado SSL](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-configure-a-port-with-an-ssl-certificate)
* [A comunicação HTTPS - HttpListener com base em certificação de cliente e hospedagem](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) isso é um blog de terceiros e é bastante antigo, mas ainda tem informações úteis.
* [Como: Passo a passo usando HttpListener ou o servidor Http não gerenciado código (C++) como um servidor simples SSL](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) muito trata um blog mais antigo com informações úteis.
* [Como configurar um WebListener de núcleo do .NET com SSL?](https://blogs.msdn.microsoft.com/timomta/2016/11/04/how-do-i-set-up-a-net-core-weblistener-with-ssl/)

Aqui estão algumas ferramentas de terceiros que podem ser mais fácil de usar que a linha de comando netsh.exe. Eles não são fornecidos pelo ou aprovados pela Microsoft. As ferramentas de executar como administrador por padrão, como netsh.exe em si requer privilégios de administrador.

* [Gerenciador de HTTP. sys](http://httpsysmanager.codeplex.com/) fornece a interface de usuário para a listagem e configurar certificados SSL e opções, as reservas de prefixo e listas de certificados confiáveis. 
* [HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) permite que você lista ou configurar certificados SSL e prefixos de URL. A interface do usuário é mais refinada do que o HTTP. sys Gerenciador e expõe algumas opções adicionais de configuração, mas caso contrário, ele fornece uma funcionalidade semelhante. Não é possível criar uma nova lista de certificados confiáveis (CTL), mas pode atribuir os existentes.

Para gerar certificados SSL autoassinados, a Microsoft fornece ferramentas de linha de comando: [MakeCert.exe](https://msdn.microsoft.com/library/windows/desktop/aa386968) e o cmdlet do PowerShell [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pki/new-selfsignedcertificate). Também existem ferramentas de interface de usuário de terceiros que facilitam para gerar certificados SSL autoassinados:

* [SelfCert](https://www.pluralsight.com/blog/software-development/selfcert-create-a-self-signed-certificate-interactively-gui-or-programmatically-in-net)
* [Interface de usuário Makecert](http://makecertui.codeplex.com/)

## <a name="next-steps"></a>Próximas etapas

Para obter mais informações, consulte os seguintes recursos:

* [Aplicativo de exemplo para este artigo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample)
* [Código-fonte WebListener](https://github.com/aspnet/HttpSysServer/)
* [Hospedagem](../hosting.md)

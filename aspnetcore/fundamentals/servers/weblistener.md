---
title: Implementação do servidor Web WebListener no ASP.NET Core
author: guardrex
description: Saiba mais sobre o WebListener, um servidor Web para o ASP.NET Core no Windows que pode ser usado para a conexão direta com a Internet sem o IIS.
monikerRange: < aspnetcore-2.0
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: fundamentals/servers/weblistener
ms.openlocfilehash: 92a2e567e968cce59ba7b6f374ebd4bc189b81ee
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52862116"
---
# <a name="weblistener-web-server-implementation-in-aspnet-core"></a>Implementação do servidor Web WebListener no ASP.NET Core

Por [Tom Dykstra](https://github.com/tdykstra) e [Chris Ross](https://github.com/Tratcher)

> [!NOTE]
> Este tópico se aplica somente ao ASP.NET Core 1.x. No ASP.NET Core 2.0, o WebListener é chamado [HTTP.sys](httpsys.md).

O WebListener é um [servidor Web para o ASP.NET Core](index.md) executado somente no Windows. Foi desenvolvido com base no [driver de modo kernel Http.Sys](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx). O WebListener é uma alternativa ao [Kestrel](kestrel.md) que pode ser usada para conexão direta com a Internet sem depender do IIS como um servidor proxy reverso. Na verdade, o **WebListener não pode ser usado com o IIS nem o IIS Express, pois não é compatível com o [Módulo do ASP.NET Core](aspnet-core-module.md).**

Embora o WebListener tenha sido desenvolvido para o ASP.NET Core, ele pode ser usado diretamente em qualquer aplicativo .NET Core ou .NET Framework por meio do pacote NuGet [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/).

O WebListener dá suporte aos seguintes recursos:

- [Autenticação do Windows](xref:security/authentication/windowsauth)
- Compartilhamento de porta
- HTTPS com SNI
- HTTP/2 por TLS (Windows 10)
- Transmissão direta de arquivo
- Cache de resposta
- WebSockets (Windows 8)

Versões do Windows compatíveis:

- Windows 7 e Windows Server 2008 R2 e posterior

[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample) ([como baixar](xref:index#how-to-download-a-sample))

## <a name="when-to-use-weblistener"></a>Quando usar o WebListener

O WebListener é útil para implantações em que você precisa expor o servidor diretamente à Internet sem usar o IIS.

![O WebListener se comunica diretamente com a Internet](weblistener/_static/weblistener-to-internet.png)

Como se baseia no Http.Sys, o WebListener não exige um servidor proxy reverso para proteção contra ataques. O Http.Sys é uma tecnologia madura que protege contra muitos tipos de ataques e fornece a robustez, segurança e escalabilidade de um servidor Web completo. O próprio IIS é executado como um ouvinte HTTP no Http.Sys.

O WebListener também é uma boa escolha para implantações internas, quando você precisa de um dos recursos oferecidos por ele que não é possível obter usando o Kestrel.

![O WebListener se comunica diretamente com a rede interna](weblistener/_static/weblistener-to-internal.png)

## <a name="kernel-mode-authentication-with-kerberos"></a>Autenticação de modo kernel com Kerberos

O WebListener delega à autenticação de modo kernel com o protocolo de autenticação Kerberos. Não há suporte para autenticação de modo de usuário com o Kerberos e o WebListener. A conta do computador precisa ser usada para descriptografar o token/tíquete do Kerberos que é obtido do Active Directory e encaminhado pelo cliente ao servidor para autenticar o usuário. Registre o SPN (nome da entidade de serviço) do host, não do usuário do aplicativo.

## <a name="how-to-use-weblistener"></a>Como usar o WebListener

Veja a seguir uma visão geral das tarefas de configuração para o sistema operacional do host e o aplicativo ASP.NET Core.

### <a name="configure-windows-server"></a>Configurar o Windows Server

* Instale a versão do .NET exigida para o aplicativo, como o [.NET Core](https://download.microsoft.com/download/0/A/3/0A372822-205D-4A86-BFA7-084D2CBE9EDF/DotNetCore.1.0.1-SDK.1.0.0.Preview2-003133-x64.exe) ou o .NET Framework 4.5.1.

* Pré-registrar prefixos de URL para associá-los ao WebListener e configurar certificados SSL

   Se você não pré-registrar prefixos de URL no Windows, precisará executar o aplicativo com privilégios de administrador. A única exceção é se você associar ao localhost usando HTTP (não HTTPS) com um número de porta maior que 1024; nesse caso, os privilégios de administrador não são necessários.

   Para obter detalhes, consulte [Como pré-registrar prefixos e configurar o SSL](#preregister-url-prefixes-and-configure-ssl) mais adiante neste artigo.

* Abra as portas do firewall para permitir que o tráfego chegue ao WebListener.

   Use o netsh.exe ou [cmdlets do PowerShell](https://technet.microsoft.com/library/jj554906).

Também há [configurações de registro do Http.Sys](https://support.microsoft.com/kb/820129).

### <a name="configure-your-aspnet-core-application"></a>Configurar o aplicativo ASP.NET Core

* Instale o pacote NuGet [Microsoft.AspNetCore.Server.WebListener](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.WebListener/). Isso também instala [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) como uma dependência.

* Chame o método de extensão `UseWebListener` no [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) no método `Main`, especificando as [opções](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.AspNetCore.Server.WebListener/WebListenerOptions.cs) e [configurações](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.Net.Http.Server/WebListenerSettings.cs) do WebListener necessárias, conforme mostrado no seguinte exemplo:

  [!code-csharp[](weblistener/sample/Program.cs?name=snippet_Main&highlight=13-17)]

* Configurar URLs e portas nas quais escutar 

  Por padrão, o ASP.NET Core é associado a `http://localhost:5000`. Para configurar portas e prefixos de URL, use o método de extensão `UseURLs`, o argumento de linha de comando `urls` ou o sistema de configuração do ASP.NET Core. Para saber mais, confira [Host no ASP.NET Core](xref:fundamentals/host/index).

  O Ouvinte Web usa os [formatos de cadeia de caracteres de prefixo do Http.Sys](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx). Não há nenhum requisito de formato de cadeia de caracteres de prefixo específico ao WebListener.

  > [!WARNING]
  > Associações de curinga de nível superior (`http://*:80/` e `http://+:80`) **não** devem ser usadas. Associações de curinga de nível superior podem abrir o aplicativo para vulnerabilidades de segurança. Isso se aplica a curingas fortes e fracos. Use nomes de host explícitos em vez de curingas. Associações de curinga de subdomínio (por exemplo, `*.mysub.com`) não têm esse risco de segurança se você controlar o domínio pai completo (em vez de `*.com`, o qual é vulnerável). Veja [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) para obter mais informações.

  > [!NOTE]
  > Especifique as mesmas cadeias de caracteres de prefixo em `UseUrls` que você pré-registrou no servidor. 

* Verifique se o aplicativo não está configurado para executar o IIS ou o IIS Express.

  No Visual Studio, o perfil de inicialização padrão destina-se ao IIS Express.  Para executar o projeto como um aplicativo de console, você precisa alterar manualmente o perfil selecionado, conforme mostrado na captura de tela a seguir.

  ![Selecionar o perfil do aplicativo de console](weblistener/_static/vs-choose-profile.png)

## <a name="how-to-use-weblistener-outside-of-aspnet-core"></a>Como usar o WebListener fora do ASP.NET Core

* Instale o pacote NuGet [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/).

* [Pré-registre prefixos de URL a serem associados ao WebListener e configure certificados SSL](#preregister-url-prefixes-and-configure-ssl) como você faria para uso no ASP.NET Core.

Também há [configurações de registro do Http.Sys](https://support.microsoft.com/kb/820129).


Este é um exemplo de código que demonstra o uso do WebListener fora do ASP.NET Core:

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

O IIS e o WebListener dependem do driver de modo kernel Http.Sys subjacente para escutar solicitações e fazer o processamento inicial. No IIS, a interface do usuário de gerenciamento fornece uma maneira relativamente fácil de configurar tudo. No entanto, se estiver usando o WebListener, você precisará configurar o Http.Sys por conta própria. A ferramenta interna para fazer isso é o netsh.exe. 

As tarefas mais comuns para as quais você precisa usar o netsh.exe são reservar prefixos de URL e atribuir certificados SSL.

O NetSh.exe não é uma ferramenta fácil de usar para iniciantes. O seguinte exemplo mostra o mínimo necessário para reservar prefixos de URL para as portas 80 e 443:

```console
netsh http add urlacl url=http://+:80/ user=Users
netsh http add urlacl url=https://+:443/ user=Users
```

O seguinte exemplo mostra como atribuir um certificado SSL:

```console
netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid="{00000000-0000-0000-0000-000000000000}".
```

Esta é a documentação de referência oficial:

* [Comandos do Netsh para o protocolo HTTP](https://technet.microsoft.com/library/cc725882.aspx)
* [Cadeias de caracteres de UrlPrefix](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

Os recursos a seguir fornecem instruções detalhadas para vários cenários. Os artigos que se referem ao `HttpListener` se aplicam igualmente ao `WebListener`, pois ambos se baseiam no Http.Sys.

* [Como configurar uma porta com um certificado SSL](/dotnet/framework/wcf/feature-details/how-to-configure-a-port-with-an-ssl-certificate)
* [HTTPS Communication – HttpListener based Hosting and Client Certification](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) (Comunicação HTTPS – Certificação de cliente e hospedagem baseada no HttpListener) Esse é um blog de terceiros e é bem antigo, mas ainda traz informações úteis.
* [How To: Walkthrough Using HttpListener or Http Server unmanaged code (C++) as an SSL Simple Server](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) [Passo a passo: usando um código não gerenciado (C++) do HttpListener ou do Servidor HTTP como um Servidor Simples SSL] Esse também é um blog mais antigo com informações úteis.
* [Como fazer para configurar um WebListener do .NET Core com SSL?](https://blogs.msdn.microsoft.com/timomta/2016/11/04/how-do-i-set-up-a-net-core-weblistener-with-ssl/)

Veja a seguir algumas ferramentas de terceiros que podem ser mais fáceis de usar que a linha de comando do netsh.exe. Elas não são fornecidas nem endossadas pela Microsoft. As ferramentas são executadas como administrador por padrão, pois o próprio netsh.exe exige privilégios de administrador.

* O [Gerenciador do http.sys](http://httpsysmanager.codeplex.com/) fornece a interface do usuário para a listagem e configuração de certificados SSL e opções, reservas de prefixo e listas de certificados confiáveis. 
* O [HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) permite listar ou configurar certificados SSL e prefixos de URL. A interface do usuário é mais refinada comparada ao Gerenciador do http.sys e expõe algumas outras opções de configuração, mas de outro modo fornece uma funcionalidade semelhante. Ela não pode criar uma nova CTL (lista de certificados confiáveis), mas pode atribuir as existentes.

Para gerar certificados SSL autoassinados, a Microsoft fornece ferramentas de linha de comando: [MakeCert.exe](https://msdn.microsoft.com/library/windows/desktop/aa386968) e o cmdlet do PowerShell [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pki/new-selfsignedcertificate). Também existem ferramentas de interface do usuário de terceiros que facilitam a geração de certificados SSL autoassinados:

* [SelfCert](https://www.pluralsight.com/blog/software-development/selfcert-create-a-self-signed-certificate-interactively-gui-or-programmatically-in-net)
* [Interface do usuário do Makecert](http://makecertui.codeplex.com/)

## <a name="next-steps"></a>Próximas etapas

Para obter mais informações, consulte os seguintes recursos:

* [Aplicativo de exemplo para este artigo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample)
* [Código-fonte do WebListener](https://github.com/aspnet/HttpSysServer/)
* [Hospedagem](xref:fundamentals/host/index)

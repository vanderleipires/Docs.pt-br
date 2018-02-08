---
title: "Implementação do servidor Web HTTP.sys no ASP.NET Core"
author: rick-anderson
description: "Apresenta o HTTP.sys, um servidor Web para o ASP.NET Core no Windows. Desenvolvido com base no driver de modo kernel Http.Sys, o HTTP.sys é uma alternativa ao Kestrel que pode ser usada para conexão direta com a Internet sem o IIS."
manager: wpickett
ms.author: riande
ms.date: 08/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/httpsys
ms.openlocfilehash: f36a86fc67e715165afd0c38992f9767f90cf3b4
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/30/2018
---
# <a name="httpsys-web-server-implementation-in-aspnet-core"></a>Implementação do servidor Web HTTP.sys no ASP.NET Core

Por [Tom Dykstra](https://github.com/tdykstra) e [Chris Ross](https://github.com/Tratcher)

> [!NOTE]
> Este tópico se aplica somente ao ASP.NET Core 2.0 e posterior. Em versões anteriores do ASP.NET Core, o HTTP.sys é chamado [WebListener](xref:fundamentals/servers/weblistener).

O HTTP.sys é um [servidor Web para o ASP.NET Core](index.md) executado somente no Windows. Foi desenvolvido com base no [driver de modo kernel Http.Sys](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx). O HTTP.sys é uma alternativa ao [Kestrel](kestrel.md) que oferece alguns recursos que não são oferecidos pelo Kestel. **O HTTP.sys não pode ser usado com o IIS ou o IIS Express, pois é incompatível com o [Módulo do ASP.NET Core](aspnet-core-module.md).**

O HTTP.sys dá suporte aos seguintes recursos:

- [Autenticação do Windows](xref:security/authentication/windowsauth)
- Compartilhamento de porta
- HTTPS com SNI
- HTTP/2 por TLS (Windows 10)
- Transmissão direta de arquivo
- Cache de resposta
- WebSockets (Windows 8)

Versões do Windows compatíveis:

- Windows 7 e Windows Server 2008 R2 e posterior

[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))

## <a name="when-to-use-httpsys"></a>Quando usar o HTTP.sys

O HTTP.sys é útil para implantações em que você precisa expor o servidor diretamente à Internet sem usar o IIS.

![O HTTP.sys se comunica diretamente com a Internet](httpsys/_static/httpsys-to-internet.png)

Como se baseia no Http.Sys, o HTTP.sys não exige um servidor proxy reverso para proteção contra ataques. O Http.Sys é uma tecnologia madura que protege contra muitos tipos de ataques e fornece a robustez, segurança e escalabilidade de um servidor Web completo. O próprio IIS é executado como um ouvinte HTTP no Http.Sys. 

O HTTP.sys é uma boa escolha para implantações internas quando você precisa de um recurso que não está disponível no Kestrel, como a autenticação do Windows.

![O HTTP.sys se comunica diretamente com a rede interna](httpsys/_static/httpsys-to-internal.png)

## <a name="how-to-use-httpsys"></a>Como usar o HTTP.sys

Veja a seguir uma visão geral das tarefas de configuração para o sistema operacional do host e o aplicativo ASP.NET Core.

### <a name="configure-windows-server"></a>Configurar o Windows Server

* Instale a versão do .NET exigida para o aplicativo, como o [.NET Core](https://www.microsoft.com/net/download/core) ou o [.NET Framework](https://www.microsoft.com/net/download/framework).

* Pré-registrar prefixos de URL para associá-los ao HTTP.sys e configurar certificados SSL

   Se você não pré-registrar prefixos de URL no Windows, precisará executar o aplicativo com privilégios de administrador. A única exceção é se você associar ao localhost usando HTTP (não HTTPS) com um número de porta maior que 1024; nesse caso, os privilégios de administrador não são necessários.

   Para obter detalhes, consulte [Como pré-registrar prefixos e configurar o SSL](#preregister-url-prefixes-and-configure-ssl) mais adiante neste artigo.

* Abra as portas do firewall para permitir que o tráfego chegue ao HTTP.sys.

   Use o *netsh.exe* ou [cmdlets do PowerShell](https://technet.microsoft.com/library/jj554906).

Também há [configurações de registro do Http.Sys](https://support.microsoft.com/kb/820129).

### <a name="configure-your-aspnet-core-application-to-use-httpsys"></a>Configurar seu aplicativo ASP.NET Core para usar o HTTP.sys

* Nenhuma instalação de pacote é necessária se o metapacote [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) for usado. O pacote [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/) está incluído no metapacote.

* Chame o método de extensão `UseHttpSys` em `WebHostBuilder` no método `Main`, especificando as [opções do HTTP.sys](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs) necessárias, conforme mostrado no seguinte exemplo:

  [!code-csharp[](httpsys/sample/Program.cs?name=snippet_Main&highlight=11-19)]

### <a name="configure-httpsys-options"></a>Configurar as opções do HTTP.sys

Veja a seguir algumas das configurações e limites do HTTP.sys que você pode definir.

**Número máximo de conexões de cliente**

O número máximo de conexões TCP abertas simultâneas pode ser definido para todo o aplicativo com o seguinte código em *Program.cs*:

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Options&highlight=5)]

O número máximo de conexões é ilimitado (nulo) por padrão.

**Tamanho máximo do corpo da solicitação**

O tamanho máximo do corpo da solicitação padrão é de 30.000.000 bytes, que equivale aproximadamente a 28,6 MB.

A maneira recomendada para substituir o limite em um aplicativo ASP.NET Core MVC é usar o atributo [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) em um método de ação:

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

Este é um exemplo que mostra como configurar a restrição para todo o aplicativo, em cada solicitação:

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Options&highlight=6)]

Substitua a configuração em uma solicitação específica em *Startup.cs*:

[!code-csharp[](httpsys/sample/Startup.cs?name=snippet_Configure&highlight=9-10)]
 
Uma exceção é gerada se você tenta configurar o limite de uma solicitação depois que o aplicativo iniciou a leitura da solicitação. Há uma propriedade `IsReadOnly` que indica se a propriedade `MaxRequestBodySize` está no estado somente leitura, o que significa que é tarde demais para configurar o limite.

Para obter informações sobre outras opções do HTTP.sys, consulte [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs). 

### <a name="configure-urls-and-ports-to-listen-on"></a>Configurar URLs e portas nas quais escutar 

Por padrão, o ASP.NET Core é associado a `http://localhost:5000`. Para configurar portas e prefixos de URL, use o método de extensão `UseUrls`, o argumento de linha de comando `urls`, a variável de ambiente ASPNETCORE_URLS ou a propriedade `UrlPrefixes` em [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs). O exemplo de código a seguir usa `UrlPrefixes`.

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Main&highlight=17)]

Uma vantagem de `UrlPrefixes` é que você receberá uma mensagem de erro imediatamente se tentar adicionar um prefixo que está formatado incorretamente. Uma vantagem de `UseUrls` (compartilhado com `urls` e ASPNETCORE_URLS) é que você pode mudar entre o Kestrel e o HTTP.sys com mais facilidade.

Se você usar `UseUrls` (`urls` ou ASPNETCORE_URLS) e `UrlPrefixes`, as configurações em `UrlPrefixes` substituirão as configurações em `UseUrls`. Para obter mais informações, consulte [Hospedagem](xref:fundamentals/hosting).

O HTTP.sys usa os [formatos de cadeia de caracteres UrlPrefix da API do Servidor HTTP](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).

> [!NOTE]
> Especifique as mesmas cadeias de caracteres de prefixo em `UseUrls` ou `UrlPrefixes` que você pré-registrou no servidor. 

### <a name="dont-use-iis"></a>Não usar o IIS

Verifique se o aplicativo não está configurado para executar o IIS ou o IIS Express.

No Visual Studio, o perfil de inicialização padrão destina-se ao IIS Express. Para executar o projeto como um aplicativo de console, altere manualmente o perfil selecionado, conforme mostrado na captura de tela a seguir.

![Selecionar o perfil do aplicativo de console](httpsys/_static/vs-choose-profile.png)

## <a name="preregister-url-prefixes-and-configure-ssl"></a>Pré-registrar prefixos de URL e configurar o SSL

O IIS e o HTTP.sys dependem do driver de modo kernel Http.Sys subjacente para escutar solicitações e fazer o processamento inicial. No IIS, a interface do usuário de gerenciamento fornece uma maneira relativamente fácil de configurar tudo. No entanto, você precisa configurar o Http.Sys por conta própria. A ferramenta interna para fazer isso é o *netsh.exe*. 

Com o *netsh.exe*, você pode reservar prefixos de URL e atribuir certificados SSL. A ferramenta exige privilégios administrativos.

O seguinte exemplo mostra o mínimo necessário para reservar prefixos de URL para as portas 80 e 443:

```console
netsh http add urlacl url=http://+:80/ user=Users
netsh http add urlacl url=https://+:443/ user=Users
```

O seguinte exemplo mostra como atribuir um certificado SSL:

```console
netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid={00000000-0000-0000-0000-000000000000}"
```

Esta é a documentação de referência do *netsh.exe*:

* [Comandos do Netsh para o protocolo HTTP](https://technet.microsoft.com/library/cc725882.aspx)
* [Cadeias de caracteres de UrlPrefix](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

Os recursos a seguir fornecem instruções detalhadas para vários cenários. Os artigos que se referem ao HttpListener se aplicam igualmente ao HTTP.sys, pois ambos se baseiam no Http.Sys.

* [Como configurar uma porta com um certificado SSL](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-configure-a-port-with-an-ssl-certificate)
* [HTTPS Communication – HttpListener based Hosting and Client Certification](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) (Comunicação HTTPS – Certificação de cliente e hospedagem baseada no HttpListener) Esse é um blog de terceiros e é bem antigo, mas ainda traz informações úteis.
* [How To: Walkthrough Using HttpListener or Http Server unmanaged code (C++) as an SSL Simple Server](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) [Passo a passo: usando um código não gerenciado (C++) do HttpListener ou do Servidor HTTP como um Servidor Simples SSL] Esse também é um blog mais antigo com informações úteis.

Veja a seguir algumas ferramentas de terceiros que podem ser mais fáceis de usar que a linha de comando do *netsh.exe*. Elas não são fornecidas nem endossadas pela Microsoft. As ferramentas são executadas como administrador por padrão, pois o próprio *netsh.exe* exige privilégios de administrador.

* O [Gerenciador do http.sys](http://httpsysmanager.codeplex.com/) fornece a interface do usuário para a listagem e configuração de certificados SSL e opções, reservas de prefixo e listas de certificados confiáveis. 
* O [HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) permite listar ou configurar certificados SSL e prefixos de URL. A interface do usuário é mais refinada comparada ao Gerenciador do http.sys e expõe algumas outras opções de configuração, mas de outro modo fornece uma funcionalidade semelhante. Ela não pode criar uma nova CTL (lista de certificados confiáveis), mas pode atribuir as existentes.

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

## <a name="next-steps"></a>Próximas etapas

Para obter mais informações, consulte os seguintes recursos:

* [Aplicativo de exemplo para este artigo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample)
* [Código-fonte do HTTP.sys](https://github.com/aspnet/HttpSysServer/)
* [Hospedagem](xref:fundamentals/hosting)

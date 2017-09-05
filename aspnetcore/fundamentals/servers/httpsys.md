---
title: "Implementação do servidor de web HTTP. sys no núcleo do ASP.NET"
author: rick-anderson
description: "Apresenta o HTTP. sys, um servidor web para o ASP.NET Core no Windows. Criado com o driver de modo kernel HTTP. sys, o HTTP. sys é uma alternativa ao Kestrel que pode ser usada para conexão direta com a Internet sem o IIS."
keywords: ASP.NET Core, HttpSys, HTTP.sys, HttpListener, prefixos de url, SSL
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: article
ms.assetid: 0a7286e4-6428-424e-b5c4-5c98815cf61c
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/httpsys
ms.openlocfilehash: cff6f171432febac5ec3e7adf9cf77953e0ece2d
ms.sourcegitcommit: 4e84d8bf5f404bb77f3d41665cf7e7374fc39142
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/05/2017
---
# <a name="httpsys-web-server-implementation-in-aspnet-core"></a>Implementação do servidor de web HTTP. sys no núcleo do ASP.NET

Por [Tom Dykstra](http://github.com/tdykstra) e [Ross Carlos](https://github.com/Tratcher)

> [!NOTE]
> Este tópico se aplica somente ao ASP.NET Core 2.0 e versões posteriores. Em versões anteriores do ASP.NET Core, é denominado HTTP. sys [WebListener](xref:fundamentals/servers/weblistener).

HTTP.sys é um [servidor web para o ASP.NET Core](index.md) que é executado somente no Windows. Ele é criado no [driver de modo kernel HTTP. sys](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx). HTTP.sys é uma alternativa ao [Kestrel](kestrel.md) que oferece alguns recursos que não Kestel. **HTTP.sys não pode ser usado com o IIS ou IIS Express, pois ele não é compatível com o [ASP.NET Core módulo](aspnet-core-module.md).**

HTTP.sys aceita os seguintes recursos:

- [Autenticação do Windows](xref:security/authentication/windowsauth)
- Compartilhamento de porta
- HTTPS com SNI
- HTTP/2 por TLS (Windows 10)
- Transmissão de arquivo direto
- O cache de resposta
- WebSocket (Windows 8)

Versões com suporte do Windows:

- Windows 7 e Windows Server 2008 R2 e posterior

[Exibir ou baixar o código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample)

## <a name="when-to-use-httpsys"></a>Quando usar o HTTP. sys

O HTTP. sys é útil para implantações em que você precisa expor o servidor diretamente à Internet sem usar o IIS.

![O HTTP. sys se comunica diretamente com a Internet](httpsys/_static/httpsys-to-internet.png)

Como ele se baseia no HTTP. sys, HTTP.sys não requer um servidor de proxy reverso para proteção contra ataques. Http. sys é uma tecnologia desenvolvida que protege contra vários tipos de ataques e fornece a robustez, segurança e escalabilidade de um servidor web completo. O próprio IIS é executado como um ouvinte HTTP sobre HTTP. sys. 

HTTP.sys é uma boa escolha para implantações internas quando precisar de um recurso não está disponível no Kestrel, como autenticação do Windows.

![O HTTP. sys se comunica diretamente com a rede interna](httpsys/_static/httpsys-to-internal.png)

## <a name="how-to-use-httpsys"></a>Como usar o HTTP. sys

Aqui está uma visão geral das tarefas de configuração para o sistema operacional do host e seu aplicativo ASP.NET Core.

### <a name="configure-windows-server"></a>Configurar o Windows Server

* Instale a versão do .NET que seu aplicativo requer, como [.NET Core](https://www.microsoft.com/net/download/core) ou [do .NET Framework](https://www.microsoft.com/net/download/framework).

* Pré-registrar prefixos de URL para associar ao HTTP. sys e configurar certificados SSL

   Se você não pré-registrar prefixos de URL no Windows, você precisa executar seu aplicativo com privilégios de administrador. A única exceção é se você associar ao localhost usando HTTP (não HTTPS) com um número de porta maior que 1024; Nesse caso, privilégios de administrador não são necessários.

   Para obter detalhes, consulte [como pré-registrar prefixos e configurar o SSL](#preregister-url-prefixes-and-configure-ssl) posteriormente neste artigo.

* Abrir portas de firewall para permitir o tráfego alcançar o HTTP. sys.

   Você pode usar *netsh.exe* ou [cmdlets do PowerShell](https://technet.microsoft.com/library/jj554906).

Também há [configurações de registro Http.Sys](https://support.microsoft.com/kb/820129).

### <a name="configure-your-aspnet-core-application-to-use-httpsys"></a>Configurar seu aplicativo ASP.NET Core para usar o HTTP. sys

* Nenhuma instalação de pacote é necessária se você usar o [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage. O [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/) pacote está incluído no metapackage.

* Chamar o `UseHttpSys` método de extensão no `WebHostBuilder` no seu `Main` método, especificando um [opções de HTTP. sys](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs) que você precisa, como mostrado no exemplo a seguir:

  [!code-csharp[](httpsys/sample/Program.cs?name=snippet_Main&highlight=11-19)]

### <a name="configure-httpsys-options"></a>Configurar opções de HTTP. sys

Aqui estão algumas das configurações de HTTP. sys e limites que você pode configurar.

**Conexões de cliente máximo**

O número máximo de conexões simultâneas de TCP abertas pode ser definido para todo o aplicativo com o código a seguir no *Program.cs*:

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Options&highlight=5)]

O número máximo de conexões é ilimitado (null) por padrão.

**Tamanho do corpo da solicitação máxima**

O tamanho do corpo da solicitação máximo padrão é 30.000.000 bytes, que é aproximadamente 28.6 MB.

A maneira recomendada para substituir o limite em um aplicativo ASP.NET MVC de núcleo é usar o [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) atributo em um método de ação:

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

Aqui está um exemplo que mostra como configurar o limite para o aplicativo inteiro, cada solicitação:

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Options&highlight=6)]

Você pode substituir a configuração de uma solicitação específica *Startup.cs*:

[!code-csharp[](httpsys/sample/Startup.cs?name=snippet_Configure&highlight=9-10)]
 
Uma exceção é gerada se você tentar configurar o limite de uma solicitação depois que o aplicativo começou a ler a solicitação. Há um `IsReadOnly` propriedade que indica se o `MaxRequestBodySize` propriedade está no estado somente leitura, que significa que é muito tarde para configurar o limite.

Para obter informações sobre outras opções de HTTP. sys, consulte [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs). 

### <a name="configure-urls-and-ports-to-listen-on"></a>Configurar URLs e portas para escutar em 

Por padrão o ASP.NET Core associa a `http://localhost:5000`. Para configurar portas e prefixos de URL, você pode usar o `UseUrls` método de extensão, o `urls` argumento de linha de comando, a variável de ambiente ASPNETCORE_URLS ou `UrlPrefixes` propriedade [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs). O seguinte exemplo de código usa `UrlPrefixes`.

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Main&highlight=17)]

Uma vantagem de `UrlPrefixes` é que você receber uma mensagem de erro imediatamente se você tentar adicionar um prefixo que é formatado errado. Uma vantagem de `UseUrls` (compartilhado com `urls` e ASPNETCORE_URLS) é que você pode mais facilmente alternar entre Kestrel e HTTP.sys.

Se você usar ambos `UseUrls` (ou `urls` ou ASPNETCORE_URLS) e `UrlPrefixes`, as configurações no `UrlPrefixes` substituir os `UseUrls`. Para obter mais informações, consulte [hospedagem](xref:fundamentals/hosting).

Usa o HTTP.sys o [HTTP Server API UrlPrefix formatos de cadeia de caracteres](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).

> [!NOTE]
> Certifique-se de que você especificar as cadeias de caracteres de prefixo mesmo em `UseUrls` ou `UrlPrefixes` que pré-registrar no servidor. 

### <a name="dont-use-iis"></a>Não use o IIS

Verifique se que seu aplicativo não está configurado para executar o IIS ou IIS Express.

No Visual Studio, o perfil de inicialização padrão é para o IIS Express. Para executar o projeto como um aplicativo de console, altere manualmente o perfil selecionado, conforme mostrado na captura de tela a seguir.

![Selecione o perfil de aplicativo de console](httpsys/_static/vs-choose-profile.png)

## <a name="preregister-url-prefixes-and-configure-ssl"></a>Pré-registrar prefixos de URL e configurar o SSL

O IIS e o HTTP. sys contam com o driver de modo de kernel HTTP. sys subjacente para escutar solicitações e processamento inicial. No IIS, a interface do usuário de gerenciamento fornece uma maneira relativamente fácil de configurar tudo. No entanto, você precisa configurar o HTTP. sys por conta própria. A ferramenta interna para fazer isto é *netsh.exe*. 

Com *netsh.exe* pode reservar prefixos de URL e atribuir certificados SSL. A ferramenta requer privilégios administrativos.

O exemplo a seguir mostra o mínimo necessário para reservar os prefixos de URL para as portas 80 e 443:

```console
netsh http add urlacl url=http://+:80/ user=Users
netsh http add urlacl url=https://+:443/ user=Users
```

O exemplo a seguir mostra como atribuir um certificado SSL:

```console
netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid={00000000-0000-0000-0000-000000000000}"
```

Aqui está a documentação de referência *netsh.exe*:

* [Comandos Netsh para Hypertext Transfer protocolo (HTTP)](http://technet.microsoft.com/library/cc725882.aspx)
* [Cadeias de caracteres de UrlPrefix](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

Os recursos a seguir fornecem instruções detalhadas para vários cenários. Os artigos que fazem referência a HttpListener se aplicam igualmente para http. sys, pois ambas se baseiam em http. sys.

* [Como: configurar uma porta com um certificado SSL](http://msdn.microsoft.com/library/ms733791.aspx)
* [A comunicação HTTPS - HttpListener com base em certificação de cliente e hospedagem](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) isso é um blog de terceiros e é bastante antigo, mas ainda tem informações úteis.
* [Como: Passo a passo usando HttpListener ou o servidor Http não gerenciado código (C++) como um servidor simples SSL](http://blogs.msdn.com/b/jpsanders/archive/2009/09/29/walkthrough-using-httplistener-as-an-ssl-simple-server.aspx) muito trata um blog mais antigo com informações úteis.

Aqui estão algumas ferramentas de terceiros que podem ser mais fácil de usar do que o *netsh.exe* linha de comando. Eles não são fornecidos pelo ou aprovados pela Microsoft. As ferramentas de executar como administrador por padrão, como *netsh.exe* em si requer privilégios de administrador.

* [Gerenciador de HTTP. sys](http://httpsysmanager.codeplex.com/) fornece a interface de usuário para a listagem e configurar certificados SSL e opções, as reservas de prefixo e listas de certificados confiáveis. 
* [HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) permite que você lista ou configurar certificados SSL e prefixos de URL. A interface do usuário é mais refinada do que o HTTP. sys Gerenciador e expõe algumas opções adicionais de configuração, mas caso contrário, ele fornece uma funcionalidade semelhante. Não é possível criar uma nova lista de certificados confiáveis (CTL), mas pode atribuir os existentes.

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

## <a name="next-steps"></a>Próximas etapas

Para obter mais informações, consulte os seguintes recursos:

* [Aplicativo de exemplo para este artigo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/HttpSys/sample)
* [Código-fonte HTTP. sys](https://github.com/aspnet/HttpSysServer/)
* [Hospedagem](xref:fundamentals/hosting)

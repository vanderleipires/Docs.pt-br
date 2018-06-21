---
title: Implementações de servidor Web em ASP.NET Core
author: rick-anderson
description: Descubra os servidores Web Kestrel e HTTP.sys para ASP.NET Core. Saiba como escolher um servidor e quando usar um servidor proxy reverso.
ms.author: tdykstra
ms.custom: mvc
ms.date: 03/13/2018
uid: fundamentals/servers/index
ms.openlocfilehash: bb0331d7201d4e979e6c6524cbf630280c4eaeb6
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274437"
---
# <a name="web-server-implementations-in-aspnet-core"></a>Implementações de servidor Web em ASP.NET Core

Por [Tom Dykstra](https://github.com/tdykstra), [Steve Smith](https://ardalis.com/), [Stephen Halter](https://twitter.com/halter73) e [Chris Ross](https://github.com/Tratcher)

Um aplicativo ASP.NET Core é executado com uma implementação do servidor HTTP em processo. A implementação do servidor escuta solicitações HTTP e traz essas solicitações à tona para o aplicativo como conjuntos de [recursos de solicitação](xref:fundamentals/request-features) compostos em um [HttpContext](/dotnet/api/system.web.httpcontext).

O ASP.NET Core envia duas implementações de servidor:

* O [Kestrel](xref:fundamentals/servers/kestrel) é o servidor HTTP de plataforma cruzada padrão para o ASP.NET Core.
* O [HTTP.sys](xref:fundamentals/servers/httpsys) é um servidor HTTP somente do Windows com base no [driver do kernel HTTP.sys e na API do servidor HTTP](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx). (O HTTP.sys é chamado [WebListener](xref:fundamentals/servers/weblistener) no ASP.NET Core 1.x.)

## <a name="kestrel"></a>Kestrel

O Kestrel é o servidor Web padrão incluído nos modelos de projeto do ASP.NET Core.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

O Kestrel pode ser usado sozinho ou com um *servidor proxy reverso* como IIS, Nginx ou Apache. Um servidor proxy reverso recebe solicitações HTTP da Internet e as encaminha para o Kestrel após algum tratamento preliminar.

![O Kestrel se comunica diretamente com a Internet, sem um servidor proxy reverso](kestrel/_static/kestrel-to-internet2.png)

![O Kestrel se comunica indiretamente com a Internet através de um servidor proxy reverso, tal como o IIS, o Nginx ou o Apache](kestrel/_static/kestrel-to-internet.png)

Qualquer configuração &mdash;com ou sem um servidor proxy reverso&mdash; é uma configuração de hospedagem válida e compatível com o ASP.NET Core 2.0 ou aplicativos posteriores. Para obter mais informações, consulte [Quando usar Kestrel com um proxy reverso](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Se o aplicativo aceita somente solicitações de uma rede interna, o Kestrel pode ser usado sozinho.

![O Kestrel se comunica diretamente com a rede interna](kestrel/_static/kestrel-to-internal.png)

Se o aplicativo for exposto à Internet, o Kestrel deverá usar o IIS, o Nginx ou o Apache como um *servidor proxy reverso*. Um servidor proxy reverso recebe solicitações HTTP da Internet e as encaminha para o Kestrel após algum tratamento preliminar, conforme mostrado no diagrama a seguir:

![O Kestrel se comunica indiretamente com a Internet através de um servidor proxy reverso, tal como o IIS, o Nginx ou o Apache](kestrel/_static/kestrel-to-internet.png)

O motivo mais importante para usar um proxy reverso para implantações de borda (expostas ao tráfego da Internet) é a segurança. As versões 1.x do Kestrel não têm recursos de segurança importantes para proteção contra ataques da Internet. Isso inclui, mas não se limita aos tempos limite, limites de tamanho da solicitação e limites de conexões simultâneas apropriados.

Para obter mais informações, consulte [Quando usar Kestrel com um proxy reverso](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).

---

O IIS, o Nginx ou o Apache não podem ser usados sem o Kestrel ou uma [implementação de servidor personalizado](#custom-servers). O ASP.NET Core foi projetado para ser executado em seu próprio processo, de modo que ele pode se comportar de forma consistente entre plataformas. O IIS, o Nginx e o Apache determinam seu próprio procedimento de inicialização e o ambiente. Para usar essas tecnologias de servidor diretamente, o ASP.NET Core precisaria adaptar-se aos requisitos de cada servidor. Usando uma implementação do servidor Web, tal como Kestrel, o ASP.NET Core tem controle sobre o processo de inicialização e o ambiente quando hospedado em tecnologias de servidor diferentes.

### <a name="iis-with-kestrel"></a>IIS com Kestrel

Ao usas o [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) ou [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) como um proxy reverso para o ASP.NET Core, o aplicativo ASP.NET Core é executado em um processo separado do processo de trabalho do IIS. No processo do IIS, o [Módulo do ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) coordena a relação do proxy reverso. As funções primárias do módulo do ASP.NET Core são iniciar o aplicativo do ASP.NET Core, reiniciá-lo quando ele falhar e encaminhar o tráfego HTTP para o aplicativo. Para obter mais informações, consulte [Módulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module). 

### <a name="nginx-with-kestrel"></a>Nginx com Kestrel

Para obter informações sobre como usar Nginx no Linux como um servidor proxy reverso para Kestrel, veja [Hospedar em Linux com o Nginx](xref:host-and-deploy/linux-nginx).

### <a name="apache-with-kestrel"></a>Apache com Kestrel

Para obter informações sobre como usar Apache no Linux como um servidor proxy reverso para Kestrel, veja [Hospedar em Linux com o Apache](xref:host-and-deploy/linux-apache).

## <a name="httpsys"></a>HTTP.sys

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Se os aplicativos ASP.NET Core forem executados no Windows, o HTTP.sys será uma alternativa ao Kestrel. O Kestrel geralmente é recomendado para melhor desempenho. O HTTP.sys pode ser usado em cenários em que o aplicativo é exposto à Internet e os recursos necessários são compatíveis com HTTP.sys, mas não com Kestrel. Para obter informações sobre os recursos do HTTP.sys, veja [HTTP.sys](xref:fundamentals/servers/httpsys).

![O HTTP.sys se comunica diretamente com a Internet](httpsys/_static/httpsys-to-internet.png)

O HTTP.sys também pode ser usado para aplicativos que são expostos somente a uma rede interna. 

![O HTTP.sys se comunica diretamente com a rede interna](httpsys/_static/httpsys-to-internal.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

O HTTP.sys é chamado [WebListener](xref:fundamentals/servers/weblistener) no ASP.NET Core 1.x. Se os aplicativos ASP.NET Core forem executados no Windows, o WebListener será uma alternativa para cenários em que o IIS não está disponível para hospedar aplicativos.

![O WebListener se comunica diretamente com a Internet](weblistener/_static/weblistener-to-internet.png)

O WebListener também poderá ser usado no lugar do Kestrel para aplicativos expostos somente a uma rede interna se os recursos necessários forem compatíveis com WebListener, mas não com Kestrel. Para obter informações sobre os recursos do WebListener, veja [WebListener](xref:fundamentals/servers/weblistener).

![O WebListener se comunica diretamente com a rede interna](weblistener/_static/weblistener-to-internal.png)

---

## <a name="aspnet-core-server-infrastructure"></a>Infraestrutura de servidor do ASP.NET Core

O [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) disponível no método `Startup.Configure` expõe a propriedade [ServerFeatures](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.serverfeatures) do tipo [IFeatureCollection](/dotnet/api/microsoft.aspnetcore.http.features.ifeaturecollection). Kestrel e HTTP.sys (WebListener no ASP.NET Core 1.x) expõem apenas um único recurso cada, [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature), mas as implementações de servidor diferentes podem expor funcionalidade adicional.

`IServerAddressesFeature` pode ser usado para descobrir a qual porta a implementação do servidor se acoplou durante o tempo de execução.

## <a name="custom-servers"></a>Servidores personalizados

Se os servidores internos não atenderem aos requisitos do aplicativo, um servidor personalizado poderá ser criado. O [Guia de OWIN (Open Web Interface para .NET)](xref:fundamentals/owin) demonstra como gravar uma implementação de [IServer](/dotnet/api/microsoft.aspnetcore.hosting.server.iserver) com base em [Nowin](https://github.com/Bobris/Nowin). Somente as interfaces de recurso que o aplicativo usa exigem implementação. Porém, no mínimo [IHttpRequestFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttprequestfeature) e [IHttpResponseFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttpresponsefeature) devem ter suporte.

## <a name="server-startup"></a>Inicialização do servidor

Ao usar o [Visual Studio](https://www.visualstudio.com/vs/), o [Visual Studio para Mac](https://www.visualstudio.com/vs/mac/) ou o [Visual Studio Code](https://code.visualstudio.com/), o servidor é iniciado quando o aplicativo é iniciado pelo IDE (ambiente de desenvolvimento integrado). No Visual Studio no Windows, os perfis de inicialização podem ser usados para iniciar o aplicativo e o servidor com o [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)/[Módulo do ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) ou o console. No Visual Studio Code, o aplicativo e o servidor são iniciados por [Omnisharp](https://github.com/OmniSharp/omnisharp-vscode), que ativa o depurador CoreCLR. Usando o Visual Studio para Mac, o aplicativo e o servidor são iniciados pelo [depurador de modo reversível Mono](http://www.mono-project.com/docs/advanced/runtime/docs/soft-debugger/).

Ao iniciar um aplicativo com um prompt de comando na pasta do projeto, a [execução dotnet](/dotnet/core/tools/dotnet-run) inicia o aplicativo e o servidor (somente no Kestrel e no HTTP.sys). A configuração é especificada pela opção `-c|--configuration`, que é definida como `Debug` (padrão) ou `Release`. Se os perfis de inicialização estiverem presentes em um arquivo *launchSettings.json*, use a opção `--launch-profile <NAME>` para definir o perfil de inicialização (por exemplo, `Development` ou `Production`). Para obter mais informações, veja os tópicos [execução dotnet](/dotnet/core/tools/dotnet-run) e [Empacotamento de distribuição do .NET Core](/dotnet/core/build/distribution-packaging).

## <a name="additional-resources"></a>Recursos adicionais

* [Kestrel](xref:fundamentals/servers/kestrel)
* [Kestrel com IIS](xref:fundamentals/servers/aspnet-core-module)
* [Hospedar em Linux com o Nginx](xref:host-and-deploy/linux-nginx)
* [Hospedar em Linux com o Apache](xref:host-and-deploy/linux-apache)
* [HTTP.sys](xref:fundamentals/servers/httpsys) (para o ASP.NET Core 1.x, veja [WebListener](xref:fundamentals/servers/weblistener))

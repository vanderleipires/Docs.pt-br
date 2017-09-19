---
title: "Implementações de servidor Web em ASP.NET Core"
author: tdykstra
description: "Apresenta os servidores Web Kestrel e WebListener para ASP.NET Core. Fornece diretrizes sobre como escolher um e quando usá-lo com um servidor proxy reverso."
keywords: ASP.NET Core, IServer, servidor Web, Kestrel, WebListener, proxy reverso
ms.author: tdykstra
manager: wpickett
ms.date: 08/03/2017
ms.topic: article
ms.assetid: dba74f39-58cd-4dee-a061-6d15f7346959
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/index
ms.openlocfilehash: 54c8e1ad7d4de7f953d9801c214c0bd577304f46
ms.sourcegitcommit: 67f54fabbfa4e3942f5bfe1f8a7fdfe4a7a75358
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/19/2017
---
# <a name="web-server-implementations-in-aspnet-core"></a>Implementações de servidor Web em ASP.NET Core

Por [Tom Dykstra](https://github.com/tdykstra), [Steve Smith](https://ardalis.com/), [Stephen Halter](https://twitter.com/halter73) e [Chris Ross](https://github.com/Tratcher)

Um aplicativo ASP.NET Core é executado com uma implementação do servidor HTTP em processo. A implementação do servidor escuta solicitações HTTP e traz essas solicitações à tona para o aplicativo como conjuntos de [recursos de solicitação](https://docs.microsoft.com/aspnet/core/fundamentals/request-features) compostos em um `HttpContext`.

O ASP.NET Core envia duas implementações de servidor:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

* O [Kestrel](kestrel.md) é um servidor HTTP de plataforma cruzada com base em [libuv](https://github.com/libuv/libuv), uma biblioteca de E/S assíncrona de plataforma cruzada.

* O [HTTP.sys](httpsys.md) é um servidor HTTP somente do Windows com base no [driver do kernel Http.sys](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

* O [Kestrel](kestrel.md) é um servidor HTTP de plataforma cruzada com base em [libuv](https://github.com/libuv/libuv), uma biblioteca de E/S assíncrona de plataforma cruzada.

* O [WebListener](weblistener.md) é um servidor HTTP somente do Windows com base no [driver do kernel Http.sys](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).

---

## <a name="kestrel"></a>Kestrel

O Kestrel é o servidor Web que está incluído por padrão em modelos de novo projeto do ASP.NET Core. 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Você pode usar o Kestrel sozinho ou com um *servidor proxy reverso* como IIS, Nginx ou Apache. Um servidor proxy reverso recebe solicitações HTTP da Internet e as encaminha para o Kestrel após algum tratamento preliminar.

![O Kestrel se comunica diretamente com a Internet, sem um servidor proxy reverso](kestrel/_static/kestrel-to-internet2.png)

![O Kestrel se comunica indiretamente com a Internet através de um servidor proxy reverso, tal como o IIS, o Nginx ou o Apache](kestrel/_static/kestrel-to-internet.png)

Qualquer configuração &mdash; com ou sem um servidor proxy reverso &mdash; também pode ser usada se o Kestrel é exposto somente a uma rede interna.

Para obter informações sobre quando usar Kestrel com um proxy reverso, consulte [Introdução ao Kestrel](kestrel.md).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Se seu aplicativo aceita solicitações somente de uma rede interna, você pode usar o Kestrel sozinho.

![O Kestrel se comunica diretamente com a rede interna](kestrel/_static/kestrel-to-internal.png)

Se você expuser seu aplicativo à Internet, você deverá usar o IIS, o Nginx ou o Apache como um *servidor proxy reverso*. Um servidor proxy reverso recebe solicitações HTTP da Internet e as encaminha para o Kestrel após algum tratamento preliminar, conforme mostrado no diagrama a seguir.

![O Kestrel se comunica indiretamente com a Internet através de um servidor proxy reverso, tal como o IIS, o Nginx ou o Apache](kestrel/_static/kestrel-to-internet.png)

O motivo mais importante para usar um proxy reverso para implantações de borda (expostas ao tráfego da Internet) é a segurança. As versões 1.x do Kestrel não têm um conjunto completo de proteção contra ataques. Isso inclui mas não se limita aos tempos limite, limites de tamanho e limites de conexões simultâneas apropriados.

Para obter informações sobre quando usar Kestrel com um proxy reverso, consulte [Introdução ao Kestrel](kestrel.md).

---

Você não pode usar o IIS, Nginx ou Apache sem o Kestrel ou uma [implementação de servidor personalizado](#custom-servers). O ASP.NET Core foi projetado para ser executado em seu próprio processo, de modo que ele pode se comportar de forma consistente entre plataformas. Cada um entre o IIS, o Nginx e o Apache determina seu próprio processo de inicialização e ambiente; para usá-los diretamente, o ASP.NET Core teria de adaptar-se às necessidades de cada um deles. Usar uma implementação do servidor Web, tal como Kestrel, oferece ao ASP.NET Core controle sobre o processo de inicialização e o ambiente. Portanto, em vez de tentar adaptar o ASP.NET Core para o IIS, o Nginx ou o Apache, configure esses servidores Web para solicitações de proxy para Kestrel. Esse acordo permite que as classes `Program.Main` e `Startup` sejam essencialmente as mesmas, independentemente do local da implantação.

### <a name="iis-with-kestrel"></a>IIS com Kestrel

Quando você usa o IIS ou IIS Express como um proxy reverso para o ASP.NET Core, o aplicativo ASP.NET Core é executado em um processo separado do processo de trabalho do IIS. No processo do IIS, um módulo IIS especial é executado para coordenar a relação de proxy reverso.  Esse é o *Módulo do ASP.NET Core*. As funções primárias do módulo do ASP.NET Core são iniciar o aplicativo do ASP.NET Core, reiniciá-lo quando ele falhar e encaminhar o tráfego HTTP para ele. Para obter mais informações, consulte [Módulo ASP.NET Core](aspnet-core-module.md). 

### <a name="nginx-with-kestrel"></a>Nginx com Kestrel

Para obter informações sobre como usar Nginx no Linux como um servidor proxy reverso para Kestrel, consulte [Publicar em um ambiente de produção do Linux](../../publishing/linuxproduction.md).

### <a name="apache-with-kestrel"></a>Apache com Kestrel

Para obter informações sobre como usar o Apache no Linux como um servidor proxy reverso para Kestrel, consulte [Usando o servidor Web Apache como um proxy reverso](../../publishing/apache-proxy.md).

## <a name="httpsys"></a>HTTP.sys

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Se você executar seu aplicativo ASP.NET Core no Windows, o HTTP. sys será uma alternativa ao Kestrel. Você pode usar o HTTP. sys para cenários em que você expõe seu aplicativo à Internet e você precisa de recursos do HTTP.sys aos quais o Kestrel não dá suporte. 

![O HTTP.sys se comunica diretamente com a Internet](httpsys/_static/httpsys-to-internet.png)

O HTTP.sys também pode ser usado para aplicativos que são expostos somente a uma rede interna. 

![O HTTP.sys se comunica diretamente com a rede interna](httpsys/_static/httpsys-to-internal.png)

Para cenários de rede interna, o Kestrel é geralmente recomendado para melhor desempenho; mas, em alguns cenários, você talvez queira usar um recurso oferecido apenas pelo HTTP.sys. Para obter informações sobre os recursos do HTTP.sys, consulte [HTTP.sys](httpsys.md).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

O HTTP.sys é chamado WebListener no ASP.NET Core 1. x. Se você executar o aplicativo ASP.NET Core no Windows, o WebListener será uma alternativa que você poderá usar para situações em que você deseje expor seu aplicativo à Internet mas não possa usar o IIS.

![O WebListener se comunica diretamente com a Internet](weblistener/_static/weblistener-to-internet.png)

O WebListener também poderá ser usado no lugar do Kestrel para aplicativos expostos somente a uma rede interna, se você precisar de recursos de WebListener para os quais o Kestrel não dê suporte. 

![O WebListener se comunica diretamente com a rede interna](weblistener/_static/weblistener-to-internal.png)

Para cenários de rede interna, o Kestrel é geralmente recomendado para melhor desempenho; mas, em alguns cenários, você talvez queira usar um recurso oferecido apenas pelo WebListener. Para obter informações sobre os recursos do WebListener, consulte [WebListener](weblistener.md).

---

## <a name="notes-about-aspnet-core-server-infrastructure"></a>Observações sobre a infraestrutura de servidor do ASP.NET Core

O [`IApplicationBuilder`](/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder) disponível no método `Configure` da classe `Startup` expõe a propriedade `ServerFeatures` do tipo [`IFeatureCollection`](/aspnet/core/api/microsoft.aspnetcore.http.features.ifeaturecollection). Kestrel e WebListener expõem apenas um único recurso, [`IServerAddressesFeature`](/aspnet/core/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature), mas diferentes implementações de servidor podem expor uma funcionalidade adicional.

`IServerAddressesFeature` pode ser usado para descobrir a qual porta a implementação do servidor se acoplou durante o tempo de execução.

## <a name="custom-servers"></a>Servidores personalizados

Se os servidores internos não atenderem às suas necessidades, você poderá criar uma implementação de servidor personalizado. O [Guia de OWIN (Open Web Interface para .NET)](../owin.md) demonstra como gravar uma implementação de [IServer](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.server.iserver) com base em [Nowin](https://github.com/Bobris/Nowin). Você pode implementar apenas as interfaces de recurso de que seu aplicativo precisa, embora você precise, no mínimo, dar suporte a [IHttpRequestFeature](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.ihttprequestfeature) e a [IHttpResponseFeature](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.ihttpresponsefeature).

## <a name="next-steps"></a>Próximas etapas

Para obter mais informações, consulte os seguintes recursos:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

- [Kestrel](kestrel.md)
- [Kestrel com IIS](aspnet-core-module.md)
- [Kestrel com Nginx](../../publishing/linuxproduction.md)
- [Kestrel com Apache](../../publishing/apache-proxy.md)
- [HTTP.sys](httpsys.md)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

- [Kestrel](kestrel.md)
- [Kestrel com IIS](aspnet-core-module.md)
- [Kestrel com Nginx](../../publishing/linuxproduction.md)
- [Kestrel com Apache](../../publishing/apache-proxy.md)
- [WebListener](weblistener.md)

---

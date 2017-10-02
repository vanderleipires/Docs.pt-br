---
title: "Módulo principal do ASP.NET"
author: tdykstra
description: "Apresenta o ASP.NET Core módulo (ANCM), um módulo do IIS que permite que o servidor de web Kestrel usar o IIS ou IIS Express como um servidor de proxy reverso."
keywords: "Módulo do núcleo do ASP.NET, IIS, IIS Express,ASP.NET Core, UseIISIntegration"
ms.author: tdykstra
manager: wpickett
ms.date: 08/03/2017
ms.topic: article
ms.assetid: 4661af33-34c5-4d71-93a0-8c7632f43580
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/aspnet-core-module
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 8ced1e667acb7d11954aea27de7701db89091fd9
ms.sourcegitcommit: 732cd2684246e49e796836596643a8d37e20c46d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/01/2017
---
# <a name="introduction-to-aspnet-core-module"></a>Introdução ao módulo principal do ASP.NET

Por [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), e [Ross Carlos](https://github.com/Tratcher) 

ASP.NET Core módulo (ANCM) permite que você executar o ASP.NET Core aplicativos por trás do IIS, usando o IIS para o que é bom (segurança, gerenciamento e muito mais) e usar [Kestrel](kestrel.md) para o que é bom (sendo realmente rápido) e obter o benefícios de ambas as tecnologias ao mesmo tempo. **ANCM só funciona com Kestrel; não é compatível com WebListener (no núcleo do ASP.NET 1. x) ou HTTP. sys (em 2. x).** 

Versões com suporte do Windows:

* Windows 7 e Windows Server 2008 R2 e posterior

[Exibir ou baixar o código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/aspnet-core-module/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-aspnet-core-module-does"></a>O que faz o módulo do ASP.NET Core

ANCM é um módulo nativo do IIS que se conecta ao pipeline do IIS e redireciona o tráfego para o aplicativo do ASP.NET Core de back-end. A maioria dos outros módulos, como autenticação do windows, ainda obtém uma chance de ser executado. ANCM apenas assume o controle quando um manipulador é selecionado para a solicitação e mapeamento de manipulador está definido no aplicativo *Web. config* arquivo.

Como separam aplicativos ASP.NET Core são executados em um processo do processo de trabalho do IIS, ANCM também gerenciamento de processo. ANCM inicia o processo para o aplicativo ASP.NET Core quando a primeira solicitação chega e reinicia-o quando ele falha. Isso é essencialmente o mesmo comportamento que os aplicativos ASP.NET clássicos que são executados no processo do IIS e são gerenciados pelo WAS (serviço de ativação do Windows).

Aqui está um diagrama que ilustra o relacionamento entre aplicativos do IIS, ANCM e ASP.NET Core.

![Módulo principal do ASP.NET](aspnet-core-module/_static/ancm.png)

Solicitações provenientes da Web e pressione o driver HTTP. sys modo de kernel que roteia no IIS, a porta primária (80) ou a porta SSL (443). ANCM encaminha as solicitações para o aplicativo ASP.NET Core na porta HTTP configurado para o aplicativo, que não é a porta 80/443.

Kestrel ouve o tráfego proveniente ANCM.  ANCM Especifica a porta por meio da variável de ambiente na inicialização e o [UseIISIntegration](#call-useiisintegration) método configura o servidor para escutar em `http://localhost:{port}`. Há verificações adicionais para rejeitar as solicitações não do ANCM. (ANCM não oferece suporte HTTPS de encaminhamento, portanto solicitações são encaminhadas por HTTP, mesmo se recebido pelo IIS por HTTPS.)

Kestrel pega solicitações do ANCM e envia-os no pipeline de middleware ASP.NET Core, que manipula e passa-los como `HttpContext` instâncias a lógica do aplicativo. Respostas do aplicativo são passadas de volta para o IIS, quais verificações-los de volta para o cliente HTTP que iniciou as solicitações.

ANCM tem algumas outras funções:

* Define as variáveis de ambiente.
* Logs de `stdout` para o armazenamento de arquivo de saída.
* Encaminha os tokens de autenticação do Windows.

## <a name="how-to-use-ancm-in-aspnet-core-apps"></a>Como usar ANCM em aplicativos do ASP.NET Core

Esta seção fornece uma visão geral do processo para configurar um servidor IIS e o aplicativo do ASP.NET Core. Para obter instruções detalhadas, consulte [publicar no IIS](../../publishing/iis.md).

### <a name="install-ancm"></a>Instalar ANCM

O módulo do ASP.NET Core precisa estar instalado no IIS em servidores e no IIS Express em seus computadores de desenvolvimento. Para servidores, ANCM está incluído no [pacote de hospedagem do .NET Core Windows Server](https://aka.ms/dotnetcore.2.0.0-windowshosting). Para computadores de desenvolvimento, o Visual Studio instala automaticamente ANCM no IIS Express e no IIS se ele já estiver instalado no computador.

### <a name="install-the-iisintegration-nuget-package"></a>Instale o pacote IISIntegration NuGet

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

O [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) pacote está incluído no metapackages ASP.NET Core ([Microsoft.AspNetCore](https://www.nuget.org/packages/Microsoft.AspNetCore/) e [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) ). Se você não usar uma da metapackages, instalar `Microsoft.AspNetCore.Server.IISIntegration` separadamente. O `IISIntegration` pacote é um pacote de interoperabilidade que lê as variáveis de ambiente difusão por ANCM para configurar seu aplicativo. As variáveis de ambiente fornecem informações de configuração, como a porta a ser escutada. 

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Em seu aplicativo, instalar [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/). O `IISIntegration` pacote é um pacote de interoperabilidade que lê as variáveis de ambiente difusão por ANCM para configurar seu aplicativo. As variáveis de ambiente fornecem informações de configuração, como a porta a ser escutada. 

---

### <a name="call-useiisintegration"></a>Chamar UseIISIntegration

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

O `UseIISIntegration` método de extensão no [ `WebHostBuilder` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilder) é chamado automaticamente quando você executa com o IIS.

Se você não estiver usando um dos metapackages ASP.NET Core e não tiver instalado o `Microsoft.AspNetCore.Server.IISIntegration` pacote, você obtém um erro de tempo de execução. Se você chamar `UseIISIntegration` explicitamente, você receberá um erro de tempo de compilação se o pacote não está instalado.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Em seu aplicativo `Main` método, chame o `UseIISIntegration` método de extensão no [ `WebHostBuilder` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilder). 

[!code-csharp[](aspnet-core-module/sample/Program.cs?name=snippet_Main&highlight=12)]

---

O `UseIISIntegration` método procura variáveis de ambiente que define ANCM e ele não ops se eles não são encontrados. Esse comportamento facilita cenários como o desenvolvimento e teste em macOS ou Linux e implantando em um servidor que executa o IIS. Durante a execução em macOS ou Linux, Kestrel atua como o servidor web; mas quando o aplicativo é implantado no ambiente do IIS, ela usa automaticamente ANCM e o IIS.

### <a name="ancm-port-binding-overrides-other-port-bindings"></a>Associação de porta ANCM substitui outras associações de porta

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

ANCM gera uma porta dinâmica para atribuir ao processo de back-end. O `UseIISIntegration` método pega essa porta dinâmica e configura Kestrel para escutar em `http://locahost:{dynamicPort}/`. Isso substitui outras configurações de URL, como chamadas ao `UseUrls` ou [API de escuta do Kestrel](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration). Portanto, não é necessário chamar `UseUrls` ou do Kestrel `Listen` API quando você usar ANCM. Se você chamar `UseUrls` ou `Listen`, Kestrel escuta na porta especificada quando você executa o aplicativo sem o IIS.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

ANCM gera uma porta dinâmica para atribuir ao processo de back-end. O `UseIISIntegration` método pega essa porta dinâmica e configura Kestrel para escutar em `http://locahost:{dynamicPort}/`. Isso substitui outras configurações de URL, como chamadas ao `UseUrls`. Portanto, não é necessário chamar `UseUrls` quando você usa ANCM. Se você chamar `UseUrls`, Kestrel escuta na porta especificada quando você executa o aplicativo sem o IIS.

No ASP.NET Core 1.0, se você chamar `UseUrls`, chamá-lo **antes de** chamar `UseIISIntegration` para que a porta configurada ANCM não é substituída. Essa ordem de chamada não é necessário no ASP.NET Core 1.1, porque a configuração de ANCM substitui `UseUrls`.

---

### <a name="configure-ancm-options-in-webconfig"></a>Configurar opções de ANCM no Web. config

Configuração para o módulo do ASP.NET Core é armazenada no *Web. config* arquivo está localizado na pasta raiz do aplicativo. Configurações nesse arquivo apontam para o comando de inicialização e os argumentos que iniciar seu aplicativo ASP.NET Core. Para código da Web. config de exemplo e obter orientação sobre as opções de configuração, consulte [referência de configuração do ASP.NET Core Module](../../hosting/aspnet-core-module.md).

### <a name="run-with-iis-express-in-development"></a>Executar com o IIS Express em desenvolvimento

O IIS Express pode ser iniciado pelo Visual Studio usando o perfil padrão definido pelos modelos de ASP.NET Core.

## <a name="next-steps"></a>Próximas etapas

Para obter mais informações, consulte os seguintes recursos:

* [Aplicativo de exemplo para este artigo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/aspnet-core-module/sample)
* [Código de origem de módulo principal do ASP.NET](https://github.com/aspnet/AspNetCoreModule)
* [Referência de configuração do módulo do ASP.NET Core](../../hosting/aspnet-core-module.md)
* [Publicação para o IIS](../../publishing/iis.md)

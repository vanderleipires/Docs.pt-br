---
title: "Módulo do ASP.NET Core"
author: tdykstra
description: "Apresenta o ANCM (Módulo do ASP.NET Core), um módulo do IIS que permite que o servidor Web Kestrel use o IIS ou IIS Express como um servidor proxy reverso."
manager: wpickett
ms.author: tdykstra
ms.custom: H1Hack27Feb2017
ms.date: 08/03/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/aspnet-core-module
ms.openlocfilehash: 4337bc42c5454d6a9634a396d9c89f3518af148b
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/30/2018
---
# <a name="introduction-to-aspnet-core-module"></a>Introdução ao Módulo do ASP.NET Core

Por [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl) e [Chris Ross](https://github.com/Tratcher) 

O ANCM (Módulo do ASP.NET Core) permite que você execute aplicativos ASP.NET Core protegidos pelo IIS, usando o IIS para o que ele é bom (segurança, capacidade de gerenciamento e muito mais) e usando o [Kestrel](kestrel.md) para o que ele é bom (ser realmente rápido) e obtenha os benefícios de ambas as tecnologias ao mesmo tempo. **O ANCM funciona apenas com o Kestrel; não é compatível com o WebListener (no ASP.NET Core 1.x) ou HTTP.sys (no 2.x).** 

Versões do Windows compatíveis:

* Windows 7 e Windows Server 2008 R2 e posterior

[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/aspnet-core-module/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-aspnet-core-module-does"></a>O que o Módulo do ASP.NET Core faz

O ANCM é um módulo nativo do IIS que é vinculado ao pipeline do IIS e redireciona o tráfego para o aplicativo ASP.NET Core back-end. A maioria dos outros módulos, como a autenticação do Windows, ainda obtém uma oportunidade de ser executada. O ANCM apenas assume o controle quando um manipulador é selecionado para a solicitação e o mapeamento de manipulador é definido no arquivo *web.config* do aplicativo.

Como os aplicativos ASP.NET Core são executados em um processo separado do processo de trabalho do IIS, o ANCM também realiza o gerenciamento de processos. O ANCM inicia o processo para o aplicativo ASP.NET Core quando a primeira solicitação é recebida e reinicia-o quando ele falha. Basicamente, esse é o mesmo comportamento dos aplicativos ASP.NET clássicos executados em processo no IIS e gerenciados pelo WAS (Serviço de Ativação do Windows).

Veja a seguir um diagrama que ilustra a relação entre o IIS, o ANCM e aplicativos ASP.NET Core.

![Módulo do ASP.NET Core](aspnet-core-module/_static/ancm.png)

As solicitações são recebidas da Web e chegam ao driver Http.Sys de modo kernel, que encaminha-as para o IIS na porta primária (80) ou porta SSL (443). O ANCM encaminha as solicitações para o aplicativo ASP.NET Core na porta HTTP configurada para o aplicativo, que não é a porta 80/443.

O Kestrel ouve o tráfego proveniente do ANCM.  O ANCM especifica a porta por meio da variável de ambiente na inicialização e o método [UseIISIntegration](#call-useiisintegration) configura o servidor para escutar em `http://localhost:{port}`. Existem verificações adicionais para rejeitar as solicitações não provenientes do ANCM. (O ANCM não dá suporte ao encaminhamento de HTTPS e, portanto, as solicitações são encaminhadas por HTTP, mesmo se forem recebidas pelo IIS por HTTPS.)

O Kestrel coleta solicitações do ANCM e envia-as por push para o pipeline de middleware do ASP.NET Core, que as manipula e as passa como instâncias `HttpContext` para a lógica do aplicativo. As respostas do aplicativo são passadas novamente para o IIS, que as envia novamente para o cliente HTTP que iniciou as solicitações.

O ANCM também tem algumas outras funções:

* Define as variáveis de ambiente.
* Registra em log a saída `stdout` no armazenamento de arquivos.
* Encaminha tokens de autenticação do Windows.

## <a name="how-to-use-ancm-in-aspnet-core-apps"></a>Como usar o ANCM em aplicativos ASP.NET Core

Esta seção fornece uma visão geral do processo para configurar um servidor IIS e um aplicativo ASP.NET Core. Para obter instruções detalhadas, consulte [Hospedar no Windows com o IIS](xref:host-and-deploy/iis/index).

### <a name="install-ancm"></a>Instalar o ANCM


O Módulo do ASP.NET Core precisa ser instalado no IIS nos servidores e no IIS Express nos computadores de desenvolvimento. Para servidores, o ANCM está incluído no [pacote de Hospedagem do Windows Server do .NET Core](https://aka.ms/dotnetcore-2-windowshosting). Para computadores de desenvolvimento, o Visual Studio instala o ANCM automaticamente no IIS Express e no IIS, caso ele já esteja instalado no computador.

### <a name="install-the-iisintegration-nuget-package"></a>Instalar o pacote NuGet IISIntegration

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

O pacote [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) está incluído nos metapacotes do ASP.NET Core ([Microsoft.AspNetCore](https://www.nuget.org/packages/Microsoft.AspNetCore/) e [Microsoft.AspNetCore.All](xref:fundamentals/metapackage)). Caso não use um dos metapacotes, instale `Microsoft.AspNetCore.Server.IISIntegration` separadamente. O pacote `IISIntegration` é um pacote de interoperabilidade que lê as variáveis de ambiente difundidas pelo ANCM para configurar seu aplicativo. As variáveis de ambiente fornecem informações de configuração, como a porta na qual escutar. 

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

No aplicativo, instale [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/). O pacote `IISIntegration` é um pacote de interoperabilidade que lê as variáveis de ambiente difundidas pelo ANCM para configurar seu aplicativo. As variáveis de ambiente fornecem informações de configuração, como a porta na qual escutar. 

---

### <a name="call-useiisintegration"></a>Chamar UseIISIntegration

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

O método de extensão `UseIISIntegration` no [`WebHostBuilder`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilder) é chamado automaticamente quando você executa com o IIS.

Caso não esteja usando um dos metapacotes do ASP.NET Core e não tenha instalado o pacote `Microsoft.AspNetCore.Server.IISIntegration`, você obterá um erro de tempo de execução. Se você chamar `UseIISIntegration` de forma explícita, receberá um erro de tempo de compilação se o pacote não estiver instalado.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

No método `Main` do aplicativo, chame o método de extensão `UseIISIntegration` no [`WebHostBuilder`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilder). 

[!code-csharp[](aspnet-core-module/sample/Program.cs?name=snippet_Main&highlight=12)]

---

O método `UseIISIntegration` procura as variáveis de ambiente definidas pelo ANCM e fica inoperante caso elas não sejam encontradas. Esse comportamento facilita cenários como o desenvolvimento e teste no macOS ou Linux e a implantação em um servidor que executa o IIS. Durante a execução no macOS ou Linux, o Kestrel atua como o servidor Web; mas quando o aplicativo é implantado no ambiente do IIS, ele usa o ANCM e o IIS automaticamente.

### <a name="ancm-port-binding-overrides-other-port-bindings"></a>A associação de porta do ANCM substitui outras associações de porta

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

O ANCM gera uma porta dinâmica a ser atribuída ao processo de back-end. O método `UseIISIntegration` seleciona essa porta dinâmica e configura o Kestrel para escutar em `http://locahost:{dynamicPort}/`. Isso substitui outras configurações de URL, como chamadas a `UseUrls` ou à [API de Escuta do Kestrel](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration). Portanto, não é necessário chamar `UseUrls` ou a API `Listen` do Kestrel quando você usar o ANCM. Se você chamar `UseUrls` ou `Listen`, o Kestrel escutará na porta especificada quando você executar o aplicativo sem o IIS.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

O ANCM gera uma porta dinâmica a ser atribuída ao processo de back-end. O método `UseIISIntegration` seleciona essa porta dinâmica e configura o Kestrel para escutar em `http://locahost:{dynamicPort}/`. Isso substitui outras configurações de URL, como chamadas a `UseUrls`. Portanto, não é necessário chamar `UseUrls` quando usar o ANCM. Se você chamar `UseUrls`, o Kestrel escutará na porta especificada quando você executar o aplicativo sem o IIS.

No ASP.NET Core 1.0, se você chamar `UseUrls`, chame-o **antes** de chamar `UseIISIntegration`, de modo que a porta configurada pelo ANCM não seja substituída. Essa ordem de chamada não é necessária no ASP.NET Core 1.1, porque a configuração do ANCM substitui `UseUrls`.

---

### <a name="configure-ancm-options-in-webconfig"></a>Configurar opções do ANCM em Web.config

A configuração do Módulo do ASP.NET Core é armazenada no arquivo *web.config* localizado na pasta raiz do aplicativo. As configurações desse arquivo apontam para o comando de inicialização e os argumentos que iniciam o aplicativo ASP.NET Core. Para obter um código de exemplo de *web.config* e diretrizes sobre opções de configuração, consulte [Referência de configuração do Módulo do ASP.NET Core](xref:host-and-deploy/aspnet-core-module).

### <a name="run-with-iis-express-in-development"></a>Executar com o IIS Express em desenvolvimento

O IIS Express pode ser iniciado pelo Visual Studio usando o perfil padrão definido pelos modelos do ASP.NET Core.

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a>A configuração de proxy usa o protocolo HTTP e um token de emparelhamento

O proxy criado entre o ANCM e o Kestrel usa o protocolo HTTP. O uso de HTTP é uma otimização de desempenho na qual o tráfego entre o ANCM e o Kestrel ocorre em um endereço de loopback fora do adaptador de rede. Não há nenhum risco de interceptação do tráfego entre o ANCM e o Kestrel em um local fora do servidor.

Um token de emparelhamento é usado para assegurar que as solicitações recebidas pelo Kestrel foram transmitidas por proxy pelo IIS e que não são provenientes de outra origem. O token de emparelhamento é criado e definido em uma variável de ambiente (`ASPNETCORE_TOKEN`) pelo ANCM. O token de emparelhamento também é definido em um cabeçalho (`MSAspNetCoreToken`) em cada solicitação com proxy. O Middleware do IIS verifica cada solicitação recebida para confirmar se o valor de cabeçalho do token de emparelhamento corresponde ao valor da variável de ambiente. Se os valores do token forem incompatíveis, a solicitação será registrada em log e rejeitada. A variável de ambiente do token de emparelhamento e o tráfego entre o ANCM e o Kestrel não são acessíveis em um local fora do servidor. Sem saber o valor do token de emparelhamento, um invasor não pode enviar solicitações que ignoram a verificação no Middleware do IIS.

## <a name="next-steps"></a>Próximas etapas

Para obter mais informações, consulte os seguintes recursos:

* [Aplicativo de exemplo para este artigo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/aspnet-core-module/sample)
* [Código-fonte do Módulo do ASP.NET Core](https://github.com/aspnet/AspNetCoreModule)
* [Referência de configuração do módulo do ASP.NET Core](xref:host-and-deploy/aspnet-core-module)
* [Hospedar no Windows com o IIS](xref:host-and-deploy/iis/index)

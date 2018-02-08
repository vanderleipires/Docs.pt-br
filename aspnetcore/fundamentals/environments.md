---
title: "Trabalhando com vários ambientes no ASP.NET Core"
author: rick-anderson
description: "Saiba como o ASP.NET Core fornece suporte para controlar o comportamento do aplicativo em vários ambientes."
manager: wpickett
ms.author: riande
ms.date: 12/25/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/environments
ms.openlocfilehash: b40ee9b1c6feae4942f05d22dab776d3cf6c26a0
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/30/2018
---
# <a name="working-with-multiple-environments"></a>Trabalhando com vários ambientes

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

O ASP.NET Core fornece suporte para a configuração do comportamento do aplicativo em tempo de execução com variáveis de ambiente.

[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))

## <a name="environments"></a>Ambientes

O ASP.NET Core lê a variável de ambiente `ASPNETCORE_ENVIRONMENT` na inicialização do aplicativo e armazena esse valor em [IHostingEnvironment.EnvironmentName](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName). `ASPNETCORE_ENVIRONMENT` pode ser definido com qualquer valor, mas há suporte para [três valores](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname?view=aspnetcore-2.0) na estrutura: [Desenvolvimento](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development?view=aspnetcore-2.0), [Preparo](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging?view=aspnetcore-2.0) e [Produção](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production?view=aspnetcore-2.0). Se `ASPNETCORE_ENVIRONMENT` não estiver definido, ele usará `Production` como padrão.

[!code-csharp[Main](environments/sample/WebApp1/Startup.cs?name=snippet)]

O código anterior:

* Chama [UseDeveloperExceptionPage](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_DeveloperExceptionPageExtensions_UseDeveloperExceptionPage_Microsoft_AspNetCore_Builder_IApplicationBuilder_) e [UseBrowserLink](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_BrowserLinkExtensions_UseBrowserLink_Microsoft_AspNetCore_Builder_IApplicationBuilder_) quando `ASPNETCORE_ENVIRONMENT` é definido como `Development`.
* Chama [UseExceptionHandler](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ExceptionHandlerExtensions_UseExceptionHandler_Microsoft_AspNetCore_Builder_IApplicationBuilder_) quando o valor de `ASPNETCORE_ENVIRONMENT` é definido com um dos seguintes:

    * `Staging`
    * `Production`
    * `Staging_2`

O [Auxiliar de Marca de Ambiente ](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) usa o valor de `IHostingEnvironment.EnvironmentName` para incluir ou excluir a marcação no elemento:

[!code-html[Main](environments/sample/WebApp1/Pages/About.cshtml)]

Observação: no Windows e macOS, valores e variáveis de ambiente não diferenciam maiúsculas de minúsculas. Valores e variáveis de ambiente do Linux **diferenciam maiúsculas de minúsculas** por padrão.

### <a name="development"></a>Desenvolvimento

O ambiente de desenvolvimento pode habilitar recursos que não devem ser expostos em produção. Por exemplo, os modelos do ASP.NET Core habilitam a [página de exceção do desenvolvedor](xref:fundamentals/error-handling#the-developer-exception-page) no ambiente de desenvolvimento.

O ambiente de desenvolvimento do computador local pode ser definido no arquivo *Properties\launchSettings.json* do projeto. Os valores de ambiente definidos em *launchSettings.json* substituem os valores definidos no ambiente do sistema.

O seguinte JSON mostra três perfis de um arquivo *launchSettings.json*:

[!code-json[Main](environments/sample/WebApp1/Properties/launchSettings.json?highlight=10,11,18,26)]

Quando o aplicativo for iniciado com `dotnet run`, o primeiro perfil com `"commandName": "Project"` será usado. O valor de `commandName` especifica o servidor Web a ser iniciado. `commandName` pode ser um destes:

* IIS Express
* IIS
* Projeto (que inicia o Kestrel)

Quando um aplicativo é iniciado com `dotnet run`:

* *launchSettings.json* é lido, se está disponível. As configurações de `environmentVariables` em *launchSettings.json* substituem as variáveis de ambiente.
* O ambiente de hospedagem é exibido.


O seguinte resultado mostra um aplicativo iniciado com `dotnet run`:
```bash
PS C:\Webs\WebApp1> dotnet run
Using launch settings from C:\Webs\WebApp1\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Webs\WebApp1
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

A guia **Depurar** do Visual Studio fornece uma GUI para editar o arquivo *launchSettings.json*:

![Configurando variáveis de ambiente das propriedades do projeto](environments/_static/project-properties-debug.png)

As alterações feitas nos perfis do projeto poderão não ter efeito até que o servidor Web seja reiniciado. O Kestrel precisa ser reiniciado antes de detectar as alterações feitas em seu ambiente.

>[!WARNING]
> *launchSettings.json* não deve armazenar segredos. A [ferramenta Secret Manager](xref:security/app-secrets) pode ser usado para armazenar segredos de desenvolvimento local.

### <a name="production"></a>Produção

O ambiente de produção deve ser configurado para maximizar a segurança, o desempenho e a robustez do aplicativo. Algumas configurações comuns que um ambiente de produção pode ter que são diferentes do desenvolvimento incluem:

* Cache.
* Recursos do lado do cliente são agrupados, minimizados e potencialmente atendidos por meio de uma CDN.
* Páginas de erro de diagnóstico desabilitadas.
* Páginas de erro amigáveis habilitadas.
* Log de produção e monitoramento habilitados. Por exemplo, [Application Insights](/azure/application-insights/app-insights-asp-net-core).

## <a name="setting-the-environment"></a>Configurando o ambiente

Geralmente, é útil definir um ambiente específico para teste. Se o ambiente não for definido, ele usará `Production` como padrão, o que desabilita a maioria dos recursos de depuração.

O método para configurar o ambiente depende do sistema operacional.

### <a name="azure"></a>Azure

Para o Serviço de Aplicativo do Azure:

* Selecione a folha **Configurações de Aplicativo**.
* Adicione a chave e o valor em **Configurações de aplicativo**.


### <a name="windows"></a>Windows
Para definir o `ASPNETCORE_ENVIRONMENT` para a sessão atual, se o aplicativo é iniciado com `dotnet run`, os seguintes comandos são usados

**Linha de comando**
```
set ASPNETCORE_ENVIRONMENT=Development
```
**PowerShell**
```
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

Esses comandos têm efeito somente para a janela atual. Quando a janela é fechada, a configuração ASPNETCORE_ENVIRONMENT é revertida para a configuração padrão ou o valor do computador. Para definir o valor globalmente no Windows, abra o **Painel de Controle** > **Sistema** > **Configurações avançadas do sistema** e adicione ou edite o valor `ASPNETCORE_ENVIRONMENT`.

![Propriedades avançadas do sistema](environments/_static/systemsetting_environment.png)

![Variável de ambiente do ASP.NET Core](environments/_static/windows_aspnetcore_environment.png)


**web.config**

Consulte a seção *Configurando variáveis de ambiente* do tópico [Referência de configuração do Módulo do ASP.NET Core](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).

**Por pool de aplicativos do IIS**

Para definir variáveis de ambiente para aplicativos individuais executados em Pools de Aplicativos isolados (compatíveis com o IIS 10.0+), consulte a seção *Comando AppCmd.exe* do tópico [Variáveis de ambiente \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe).

### <a name="macos"></a>macOS
A configuração do ambiente atual para o macOS pode ser feita em linha durante a execução do aplicativo

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```
ou usando `export` para configurá-lo antes da execução do aplicativo.

```bash
export ASPNETCORE_ENVIRONMENT=Development
```
As variáveis de ambiente no nível do computador são definidas no arquivo *.bashrc* ou *.bash_profile*. Edite o arquivo usando qualquer editor de texto e adicione a instrução a seguir.

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

### <a name="linux"></a>Linux
Para distribuições Linux, use o comando `export` na linha de comando para as configurações de variável baseadas na sessão e o arquivo *bash_profile* para as configurações de ambiente no nível do computador.

### <a name="configuration-by-environment"></a>Configuração por ambiente

Consulte [Configuração por ambiente](xref:fundamentals/configuration/index#configuration-by-environment) para obter mais informações.

<a name="startup-conventions"></a>
## <a name="environment-based-startup-class-and-methods"></a>Classe Startup e métodos baseados no ambiente

Quando um aplicativo ASP.NET Core é iniciado, a [classe Startup](xref:fundamentals/startup) inicia o aplicativo. Se houver uma classe `Startup{EnvironmentName}`, essa classe será chamada para esse `EnvironmentName`:

[!code-csharp[Main](environments/sample/WebApp1/StartupDev.cs?name=snippet&highlight=1)]

Observação: a chamada a [WebHostBuilder.UseStartup<TStartup> ](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) substitui as seções de configuração.

[Configure](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_StartupBase_Configure_Microsoft_AspNetCore_Builder_IApplicationBuilder_) e [ConfigureServices](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices?view=aspnetcore-2.0) são compatíveis com versões específicas ao ambiente do formato `Configure{EnvironmentName}` e `Configure{EnvironmentName}Services`:

[!code-csharp[Main](environments/sample/WebApp1/Startup.cs?name=snippet_all&highlight=15,37)]

## <a name="additional-resources"></a>Recursos adicionais

* [Inicialização de aplicativos](xref:fundamentals/startup)
* [Configuração](xref:fundamentals/configuration/index)
* [IHostingEnvironment.EnvironmentName](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName)

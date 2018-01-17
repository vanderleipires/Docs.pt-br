---
title: "Trabalhando com vários ambientes no núcleo do ASP.NET"
author: rick-anderson
description: "Saiba como o ASP.NET Core fornece suporte para controlar o comportamento do aplicativo em vários ambientes."
keywords: "Núcleo do ASP.NET, configurações de ambiente, ASPNETCORE_ENVIRONMENT"
ms.author: riande
manager: wpickett
ms.date: 12/25/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/environments
ms.openlocfilehash: 784d176145c3e4e44ddc0ea06b6702f70cd4b08c
ms.sourcegitcommit: 87168cdc409e7a7257f92a0f48f9c5ab320b5b28
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/17/2018
---
# <a name="working-with-multiple-environments"></a>Trabalhando com vários ambientes

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core fornece suporte para a configuração de comportamento do aplicativo em tempo de execução com variáveis de ambiente.

[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))

## <a name="environments"></a>Ambientes

ASP.NET Core lê a variável de ambiente `ASPNETCORE_ENVIRONMENT` na inicialização do aplicativo e armazena esse valor em [IHostingEnvironment.EnvironmentName](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName). `ASPNETCORE_ENVIRONMENT`pode ser definida como qualquer valor, mas [três valores](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname?view=aspnetcore-2.0) com suporte do framework: [desenvolvimento](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development?view=aspnetcore-2.0), [preparo](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging?view=aspnetcore-2.0), e [produção](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production?view=aspnetcore-2.0). Se `ASPNETCORE_ENVIRONMENT` não está definida, o padrão será `Production`.

[!code-csharp[Main](environments/sample/WebApp1/Startup.cs?name=snippet)]

O código anterior:

* Chamadas [UseDeveloperExceptionPage](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_DeveloperExceptionPageExtensions_UseDeveloperExceptionPage_Microsoft_AspNetCore_Builder_IApplicationBuilder_) e [UseBrowserLink](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_BrowserLinkExtensions_UseBrowserLink_Microsoft_AspNetCore_Builder_IApplicationBuilder_) quando `ASPNETCORE_ENVIRONMENT` é definido como `Development`.
* Chamadas [UseExceptionHandler](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ExceptionHandlerExtensions_UseExceptionHandler_Microsoft_AspNetCore_Builder_IApplicationBuilder_) quando o valor de `ASPNETCORE_ENVIRONMENT` é definido um dos seguintes:

    * `Staging`
    * `Production`
    * `Staging_2`

O [auxiliar de marca de ambiente ](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) usa o valor de `IHostingEnvironment.EnvironmentName` para incluir ou excluir marcação no elemento:

[!code-html[Main](environments/sample/WebApp1/Pages/About.cshtml)]

Observação: No Windows e macOS, valores e variáveis de ambiente não são diferencia maiusculas de minúsculas. Variáveis de ambiente do Linux e os valores são **diferencia maiusculas de minúsculas** por padrão.

### <a name="development"></a>Desenvolvimento

O ambiente de desenvolvimento pode habilitar recursos que não devem ser expostos na produção. Por exemplo, os modelos do ASP.NET Core habilitam o [página de exceção de desenvolvedor](xref:fundamentals/error-handling#the-developer-exception-page) no ambiente de desenvolvimento.

O ambiente de desenvolvimento do computador local pode ser definido na *Properties\launchSettings.json* arquivo do projeto. Definir valores de ambiente em *launchSettings.json* substituir valores definidos no ambiente do sistema.

O XML a seguir mostra três perfis de um *launchSettings.json* arquivo:

[!code-xml[Main](environments/sample/WebApp1/Properties/launchSettings.json?highlight=10,11,18,26)]

Quando o aplicativo é iniciado com `dotnet run`, o primeiro perfil com `"commandName": "Project"` será usado. O valor de `commandName` Especifica o servidor web para iniciar. `commandName`pode ser um destes:

* IIS Express
* IIS
* Projeto (o que inicia Kestrel)

Quando um aplicativo é iniciado com `dotnet run`:

* *launchSettings.json* é lido se disponível. `environmentVariables`as configurações no *launchSettings.json* substituir variáveis de ambiente.
* O ambiente de hospedagem é exibido.


A saída a seguir mostra um aplicativo iniciado com `dotnet run`:
```bash
PS C:\Webs\WebApp1> dotnet run
Using launch settings from C:\Webs\WebApp1\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Webs\WebApp1
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

O Visual Studio **depurar** guia fornece uma interface gráfica do usuário para editar o *launchSettings.json* arquivo:

![Variáveis de ambiente de configuração de propriedades do projeto](environments/_static/project-properties-debug.png)

As alterações feitas aos perfis de projeto não podem ser aplicadas até que o servidor web seja reiniciado. Kestrel deve ser reiniciado antes de ele detectará as alterações feitas em seu ambiente.

>[!WARNING]
> *launchSettings.json* não deve armazenar segredos. O [ferramenta Gerenciador de segredo](xref:security/app-secrets) pode ser usado para armazenar segredos de desenvolvimento local.

### <a name="production"></a>Produção

O ambiente de produção deve ser configurado para maximizar a segurança, desempenho e eficiência do aplicativo. Algumas configurações comuns que pode ter um ambiente de produção que seriam diferentes de desenvolvimento incluem:

* Armazenamento em cache.
* Recursos do cliente são agrupados, minimizados e potencialmente atendidos a partir de uma CDN.
* Páginas de erro de diagnóstico desabilitadas.
* Páginas de erro amigável habilitadas.
* Log de produção e o monitoramento habilitado. Por exemplo, [Application Insights](https://azure.microsoft.com/documentation/articles/app-insights-asp-net-five/).

## <a name="setting-the-environment"></a>Configurando o ambiente

Geralmente é útil definir um ambiente específico para teste. Se o ambiente não for definido, o padrão será `Production` que desativa a maioria dos recursos de depuração.

O método para definir o ambiente depende do sistema operacional.

### <a name="azure"></a>Azure

Para o serviço de aplicativo do Azure:

* Selecione o **configurações de aplicativo** folha.
* Adicionar a chave e valor em **configurações do aplicativo**.


### <a name="windows"></a>Windows
Para definir o `ASPNETCORE_ENVIRONMENT` para a sessão atual, se o aplicativo é iniciado usando `dotnet run`, os comandos a seguir são usados

**Linha de comando**
```
set ASPNETCORE_ENVIRONMENT=Development
```
**PowerShell**
```
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

Esses comandos têm efeito somente para a janela atual. Quando a janela for fechada, a configuração ASPNETCORE_ENVIRONMENT é revertida para a configuração padrão ou o valor de máquina. Para definir o valor global em janelas abertas do **painel de controle** > **sistema** > **configurações avançadas do sistema** e adicionar ou editar o `ASPNETCORE_ENVIRONMENT` valor.

![Propriedades avançadas do sistema](environments/_static/systemsetting_environment.png)

![Variável de ambiente do ASPNET Core](environments/_static/windows_aspnetcore_environment.png)


**web.config**

Consulte o *definir variáveis de ambiente* seção o [referência de configuração do módulo do ASP.NET Core](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) tópico.

**Por Pool de aplicativos do IIS**

Para definir variáveis de ambiente para aplicativos individuais executados em Pools de aplicativos isolados (com suporte no IIS 10.0 +), consulte o *comando AppCmd.exe* seção o [variáveis de ambiente \< environmentVariables >](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) tópico.

### <a name="macos"></a>macOS
Configurar o ambiente atual para macOS pode ser feito na linha ao executar o aplicativo.

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```
ou usar `export` para defini-lo antes de executar o aplicativo.

```bash
export ASPNETCORE_ENVIRONMENT=Development
```
Variáveis de ambiente de nível de máquina são definidas no *. bashrc* ou *. bash_profile* arquivo. Editar o arquivo usando qualquer editor de texto e adicione a seguinte instrução.

```
export ASPNETCORE_ENVIRONMENT=Development
```

### <a name="linux"></a>Linux
Para distribuições de Linux, use o `export` para sessão com base em configurações de variável de comando na linha de comando e *bash_profile* arquivo de configurações de ambiente de nível de máquina.

### <a name="configuration-by-environment"></a>Configuração do ambiente

Consulte [configuração pelo ambiente](xref:fundamentals/configuration/index#configuration-by-environment) para obter mais informações.

<a name="startup-conventions"></a>
## <a name="environment-based-startup-class-and-methods"></a>Ambiente com base em métodos e classe de inicialização

Quando um aplicativo do ASP.NET Core é iniciado, o [classe inicialização](xref:fundamentals/startup) inicializa o aplicativo. Se uma classe `Startup{EnvironmentName}` existe, se a classe será chamada para que `EnvironmentName`:

[!code-csharp[Main](environments/sample/WebApp1/StartupDev.cs?name=snippet&highlight=1)]

Observação: Ao chamar [WebHostBuilder.UseStartup<TStartup> ](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) substitui seções de configuração.

[Configurar](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_StartupBase_Configure_Microsoft_AspNetCore_Builder_IApplicationBuilder_) e [ConfigureServices](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices?view=aspnetcore-2.0) suporte a versões específicas do ambiente do formulário `Configure{EnvironmentName}` e `Configure{EnvironmentName}Services`:

[!code-csharp[Main](environments/sample/WebApp1/Startup.cs?name=snippet_all&highlight=15,37)]

## <a name="additional-resources"></a>Recursos adicionais

* [Inicialização de aplicativos](xref:fundamentals/startup)
* [Configuração](xref:fundamentals/configuration/index)
* [IHostingEnvironment.EnvironmentName](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName)

---
title: "Trabalhando com vários ambientes no núcleo do ASP.NET"
author: ardalis
description: "Saiba como o ASP.NET Core fornece suporte para controlar o comportamento do aplicativo em vários ambientes."
keywords: "Núcleo do ASP.NET, configurações de ambiente, ASPNETCORE_ENVIRONMENT"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: b5bba985-be12-4464-9a01-df3599b2a6f1
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/environments
ms.openlocfilehash: 2a92c50085e70b4a505913c86348ba5fe54f6d13
ms.sourcegitcommit: 67811da1278c75cb10994602c13bd5adec3f0907
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/04/2017
---
# <a name="working-with-multiple-environments"></a>Trabalhando com vários ambientes

Por [Steve Smith](https://ardalis.com/)

ASP.NET Core fornece suporte para controlar o comportamento do aplicativo em vários ambientes, como desenvolvimento, teste e produção. Variáveis de ambiente são usadas para indicar o ambiente de tempo de execução, permitindo que o aplicativo a ser configurado para esse ambiente.

[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))

## <a name="development-staging-production"></a>Desenvolvimento, teste, produção

ASP.NET Core faz referência a uma variável de ambiente `ASPNETCORE_ENVIRONMENT` para descrever o ambiente em que o aplicativo está sendo executado no. Essa variável pode ser definida como qualquer valor que você deseja, mas três valores são usados por convenção: `Development`, `Staging`, e `Production`. Você encontrará esses valores usados nos exemplos e os modelos fornecidos com o ASP.NET Core.

A configuração atual do ambiente pode ser detectada programaticamente de dentro de seu aplicativo. Além disso, você pode usar o ambiente [auxiliar de marca](../mvc/views/tag-helpers/index.md) para incluir algumas seções no seu [exibição](../mvc/views/index.md) com base no ambiente do aplicativo atual.

Observação: No Windows e macOS, o nome de ambiente especificada diferencia maiusculas de minúsculas. Se você definir a variável `Development` ou `development` ou `DEVELOPMENT` os resultados serão os mesmos. No entanto, o Linux é um **diferencia maiusculas de minúsculas** sistema operacional por padrão. Variáveis de ambiente, nomes de arquivos e configurações requerem a diferenciação de maiusculas e minúsculas.

### <a name="development"></a>Desenvolvimento

Isso deve ser o ambiente usado ao desenvolver um aplicativo. Ela é normalmente usada para habilitar os recursos que você não iria querer estar disponível quando o aplicativo for executado em produção, como o [página de exceção de desenvolvedor](xref:fundamentals/error-handling#the-developer-exception-page).

Se você estiver usando o Visual Studio, o ambiente pode ser configurado em perfis de depuração do projeto. Perfis de depuração especificar o [server](xref:fundamentals/servers/index) para usar ao iniciar o aplicativo e as variáveis de ambiente a ser definido. Seu projeto pode ter vários perfis de depuração que definir variáveis de ambiente de modo diferente. Gerenciar esses perfis usando o **depurar** guia de seu projeto de aplicativo web **propriedades** menu. Os valores definidos nas propriedades do projeto são persistentes no *launchSettings.json* arquivo e você também pode configurar perfis, editando o arquivo diretamente.

O perfil para o IIS Express é mostrado aqui:

![Variáveis de ambiente de configuração de propriedades do projeto](environments/_static/project-properties-debug.png)

Aqui está uma `launchSettings.json` arquivo que inclui perfis para `Development` e `Staging`:

[!code-json[Main](../fundamentals/environments/sample/src/Environments/Properties/launchSettings.json?highlight=15,22)]

As alterações feitas aos perfis de projeto podem não ter efeito até que o servidor web usado seja reiniciado (em particular, Kestrel deve ser reiniciado para que ele detectará as alterações feitas em seu ambiente).

>[!WARNING]
> Variáveis de ambiente são armazenados em *launchSettings.json* não estão protegidos de qualquer forma e fará parte do repositório de código de origem para o seu projeto, se você usar um. **Nunca armazene credenciais ou outros dados secretos nesse arquivo.** Se você precisar de um local para armazenar esses dados, use o *Manager segredo* ferramenta descrita no [armazenamento seguro de segredos do aplicativo durante o desenvolvimento](../security/app-secrets.md#security-app-secrets).

### <a name="staging"></a>Preparação

Por convenção, um `Staging` ambiente é um ambiente de pré-produção usado para teste final antes da implantação de produção. Idealmente, suas características físicas devem ser espelhada de produção, para que os problemas que podem surgir em produção ocorrem primeiro no ambiente de preparo, onde podem ser resolvidas sem causar impacto aos usuários.

### <a name="production"></a>Produção

O `Production` ambiente é o ambiente no qual o aplicativo é executado quando ele está ativo e sendo usado por usuários finais. Esse ambiente deve ser configurado para maximizar a segurança, desempenho e eficiência do aplicativo. Algumas configurações comuns que pode ter um ambiente de produção que seriam diferentes de desenvolvimento incluem:

* Ativar cache

* Verifique se todos os recursos do lado do cliente são agrupados, minimizados e potencialmente atendidos a partir de uma CDN

* Desativar ErrorPages diagnóstico

* Ativar páginas de erro amigável

* Habilitar registro em log e monitoramento de produção (por exemplo, [Application Insights](https://azure.microsoft.com/documentation/articles/app-insights-asp-net-five/))

Isso não deve ser uma lista completa. É melhor evitar dispersão verificações de ambiente em muitas partes do seu aplicativo. Em vez disso, a abordagem recomendada é executar essas verificações dentro do aplicativo `Startup` classe (s) sempre que possível

## <a name="setting-the-environment"></a>Configurando o ambiente

O método para definir o ambiente depende do sistema operacional.

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

**Web. config**

Consulte o *definir variáveis de ambiente* seção o [referência de configuração do módulo do ASP.NET Core](xref:hosting/aspnet-core-module#setting-environment-variables) tópico.

**Por Pool de aplicativos do IIS**

Se precisar definir variáveis de ambiente para aplicativos individuais executados em Pools de Aplicativos isolados (com suporte no IIS 10.0+), consulte a seção *comando AppCmd.exe* do tópico [Variáveis de ambiente \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) na documentação de referência do IIS.

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

## <a name="determining-the-environment-at-runtime"></a>Determinando o ambiente em tempo de execução

O `IHostingEnvironment` serviço fornece a abstração de núcleo para trabalhar com ambientes. Esse serviço é fornecido pelo ASP.NET hospedagem da camada e pode ser inserida na sua lógica de inicialização por meio de [injeção de dependência](dependency-injection.md). O modelo de site da web do ASP.NET Core no Visual Studio usa essa abordagem para carregar arquivos de configuração específicos ao ambiente (se houver) e para personalizar as configurações de tratamento de erros do aplicativo. Em ambos os casos, esse comportamento é obtido consultando o ambiente no momento especificado chamando `EnvironmentName` ou `IsEnvironment` na instância do `IHostingEnvironment` passado para o método apropriado.

> [!NOTE]
> Se você precisa verificar se o aplicativo está em execução em um ambiente específico, use `env.IsEnvironment("environmentname")` desde que ele irá ignorar corretamente caso (em vez de verificar se `env.EnvironmentName == "Development"` por exemplo).

Por exemplo, você pode usar o código a seguir em seu método de configurar para configurar o tratamento de erro específico do ambiente:

[!code-csharp[Main](environments/sample/src/Environments/Startup.cs?range=19-30)]

Se o aplicativo for executado em um `Development` ambiente, ele habilita o suporte de tempo de execução necessário para usar o recurso de "BrowserLink" no Visual Studio, as páginas de erro específico de desenvolvimento (que normalmente não devem ser executadas em produção) e o erro de banco de dados especial páginas (que fornecem uma maneira de aplicar as migrações e devem, portanto, ser usadas apenas em desenvolvimento). Caso contrário, se o aplicativo não está em execução em um ambiente de desenvolvimento, uma página de tratamento de erro padrão é configurada para ser exibido em resposta a todas as exceções sem tratamento.

Talvez seja necessário determinar qual conteúdo para enviar ao cliente em tempo de execução, dependendo do ambiente atual. Por exemplo, em um ambiente de desenvolvimento é geralmente servir não minimizado scripts e folhas de estilo, o que torna mais fácil de depuração. Ambientes de produção e de teste devem atender as versões minimizadas e geralmente de uma CDN. Você pode fazer isso usando o ambiente [auxiliar de marca](../mvc/views/tag-helpers/intro.md). O auxiliar de marca de ambiente somente processar seu conteúdo se o ambiente atual corresponde a um dos ambientes especificados usando o `names` atributo.

[!code-html[Main](environments/sample/src/Environments/Views/Shared/_Layout.cshtml?range=13-22)]

Para começar a usar os auxiliares de marcação no seu aplicativo, consulte [Introdução ao auxiliares de marcação](../mvc/views/tag-helpers/intro.md).

## <a name="startup-conventions"></a>Convenções de inicialização

ASP.NET Core dá suporte a uma abordagem baseado em convenção para configurar a inicialização de um aplicativo com base no ambiente atual. Também por meio de programação, você pode controlar como o seu aplicativo se comporta de acordo com o ambiente é, permitindo que você criar e gerenciar suas próprias convenções.

Quando um aplicativo do ASP.NET Core é iniciado, o `Startup` classe é usada para inicializar o aplicativo, carregue suas definições de configuração, etc. ([saber mais sobre a inicialização do ASP.NET](startup.md)). No entanto, se existir uma classe denominada `Startup{EnvironmentName}` (por exemplo `StartupDevelopment`) e o `ASPNETCORE_ENVIRONMENT` esse nome, em seguida, que corresponde a variável de ambiente `Startup` classe é usada em vez disso. Assim, você pode configurar `Startup` para o desenvolvimento, mas possuem um separado `StartupProduction` que seria usada quando o aplicativo é executado em produção. Ou vice-versa.

> [!NOTE]
> Chamando `WebHostBuilder.UseStartup<TStartup>()` substitui seções de configuração.

Além de usar totalmente separados `Startup` classe com base no ambiente atual, você também pode fazer ajustes como o aplicativo é configurado em um `Startup` classe. O `Configure()` e `ConfigureServices()` métodos oferecem suporte a versões de específico do ambiente semelhante de `Startup` classe em si, de forma `Configure{EnvironmentName}()` e `Configure{EnvironmentName}Services()`. Se você definir um método `ConfigureDevelopment()` será chamado em vez de `Configure()` quando o ambiente é definido para o desenvolvimento. Da mesma forma, `ConfigureDevelopmentServices()` deve ser chamado em vez de `ConfigureServices()` no mesmo ambiente.

## <a name="summary"></a>Resumo

ASP.NET Core fornece uma série de recursos e convenções que permitem aos desenvolvedores controlar facilmente como seus aplicativos se comportam em ambientes diferentes. Ao publicar um aplicativo de desenvolvimento para teste em produção, conjunto de variáveis de ambiente adequadamente para o ambiente permitir para a otimização do aplicativo para uso de depuração, teste ou produção, conforme apropriado.

## <a name="additional-resources"></a>Recursos adicionais

* [Configuração](configuration.md)

* [Introdução aos auxiliares de marcação](../mvc/views/tag-helpers/intro.md)

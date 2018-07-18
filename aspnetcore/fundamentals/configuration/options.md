---
title: Padrão de opções no ASP.NET Core
author: guardrex
description: Descubra como usar o padrão de opções para representar grupos de configurações relacionadas em aplicativos ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 11/28/2017
uid: fundamentals/configuration/options
ms.openlocfilehash: c996ac6ab05b98bcca72d0993fe412f553b58106
ms.sourcegitcommit: 19cbda409bdbbe42553dc385ea72d2a8e246509c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38992958"
---
# <a name="options-pattern-in-aspnet-core"></a>Padrão de opções no ASP.NET Core

Por [Luke Latham](https://github.com/guardrex)

O padrão de opções usa classes para representar grupos de configurações relacionadas. Quando as definições de configuração são isoladas por recurso em classes separadas, o aplicativo segue dois princípios importantes de engenharia de software:

* O [ISP (Princípio de Segregação da Interface)](http://deviq.com/interface-segregation-principle/): os recursos (classes) que dependem de definições de configuração dependem apenas das definições de configuração usadas por eles.
* [Separação de Interesses](http://deviq.com/separation-of-concerns/): as configurações para diferentes partes do aplicativo não são dependentes nem acopladas entre si.

[Exiba ou baixe o código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample)) Este artigo é mais fácil de ser acompanhado com o aplicativo de exemplo.

## <a name="basic-options-configuration"></a>Configuração de opções básicas

A configuração de opções básicas é demonstrada como o Exemplo &num;1 no [aplicativo de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

Uma classe de opções deve ser não abstrata com um construtor público sem parâmetros. A classe a seguir, `MyOptions`, tem duas propriedades, `Option1` e `Option2`. A configuração de valores padrão é opcional, mas o construtor de classe no exemplo a seguir define o valor padrão de `Option1`. `Option2` tem um valor padrão definido com a inicialização da propriedade diretamente (*Models/MyOptions.cs*):

[!code-csharp[](options/sample/Models/MyOptions.cs?name=snippet1)]

A classe `MyOptions` é adicionada ao contêiner de serviço com [Configure&lt;TOptions&gt;(IServiceCollection, IConfiguration)](/dotnet/api/microsoft.extensions.dependencyinjection.optionsconfigurationservicecollectionextensions.configure#Microsoft_Extensions_DependencyInjection_OptionsConfigurationServiceCollectionExtensions_Configure__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_Microsoft_Extensions_Configuration_IConfiguration_) e é associada à configuração:

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example1)]

O seguinte modelo de página usa a [injeção de dependência de construtor](xref:fundamentals/dependency-injection#what-is-dependency-injection) com [IOptions&lt;TOptions&gt;](/dotnet/api/Microsoft.Extensions.Options.IOptions-1) para acessar as configurações (*Pages/Index.cshtml.cs*):

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example1)]

O arquivo *appsettings.json* de exemplo especifica valores para `option1` e `option2`:

[!code-json[](options/sample/appsettings.json?highlight=2-3)]

Quando o aplicativo é executado, o método `OnGet` do modelo de página retorna uma cadeia de caracteres que mostra os valores da classe de opção:

```html
option1 = value1_from_json, option2 = -1
```

> [!NOTE]
> Ao usar um [ConfigurationBuilder](/dotnet/api/system.configuration.configurationbuilder) personalizado para carregar opções de configuração de um arquivo de configuração, confirme se o caminho de base está corretamente configurado:
>
> ```csharp
> var configBuilder = new ConfigurationBuilder()
>    .SetBasePath(Directory.GetCurrentDirectory())
>    .AddJsonFile("appsettings.json", optional: true);
> var config = configBuilder.Build();
>
> services.Configure<MyOptions>(config);
> ```
>
> Não é necessário configurar o caminho de base explicitamente ao carregar opções de configuração do arquivo de configuração por meio do [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).

## <a name="configure-simple-options-with-a-delegate"></a>Configurar opções simples com um delegado

A configuração de opções simples com um delegado é demonstrada como o Exemplo &num;2 no [aplicativo de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

Use um delegado para definir valores de opções. O aplicativo de exemplo usa a classe `MyOptionsWithDelegateConfig` (*Models/MyOptionsWithDelegateConfig.cs*):

[!code-csharp[](options/sample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

No código a seguir, um segundo serviço `IConfigureOptions<TOptions>` é adicionado ao contêiner de serviço. Ele usa um delegado para configurar a associação com `MyOptionsWithDelegateConfig`:

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example2)]

*Index.cshtml.cs*:

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example2)]

Adicione vários provedores de configuração. Provedores de configuração estão disponíveis em pacotes NuGet. Eles são aplicados para que sejam registrados.

Cada chamada a [Configure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1.configure) adiciona um serviço `IConfigureOptions<TOptions>` ao contêiner de serviço. No exemplo anterior, os valores `Option1` e `Option2` são especificados em *appsettings.json*, mas os valores `Option1` e `Option2` são substituídos pelo delegado configurado.

Quando mais de um serviço de configuração é habilitado, a última fonte de configuração especificada *vence* e define o valor de configuração. Quando o aplicativo é executado, o método `OnGet` do modelo de página retorna uma cadeia de caracteres que mostra os valores da classe de opção:

```html
delegate_option1 = value1_configured_by_delgate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a>Configuração de subopções

A configuração de subopções básicas é demonstrada como o Exemplo &num;3 no [aplicativo de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

Os aplicativos devem criar classes de opções que pertencem a grupos de recursos específicos (classes) no aplicativo. Partes do aplicativo que exigem valores de configuração devem ter acesso apenas aos valores de configuração usados por elas.

Ao associar opções à configuração, cada propriedade no tipo de opções é associada a uma chave de configuração do formato `property[:sub-property:]`. Por exemplo, a propriedade `MyOptions.Option1` é associada à chave `Option1`, que é lida da propriedade `option1` em *appsettings.json*.

No código a seguir, um terceiro serviço `IConfigureOptions<TOptions>` é adicionado ao contêiner de serviço. Ele associa `MySubOptions` à seção `subsection` do arquivo *appsettings.json*:

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example3)]

O método de extensão `GetSection` exige o pacote NuGet [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/). Se o aplicativo usa o [metapacote Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 ou posterior), o pacote é automaticamente incluído.

O arquivo *appsettings.json* de exemplo define um membro `subsection` com chaves para `suboption1` e `suboption2`:

[!code-json[](options/sample/appsettings.json?highlight=4-7)]

A classe `MySubOptions` define propriedades, `SubOption1` e `SubOption2`, para armazenar os valores de subopção (*Models/MySubOptions.cs*):

[!code-csharp[](options/sample/Models/MySubOptions.cs?name=snippet1)]

O método `OnGet` do modelo de página retorna uma cadeia de caracteres com os valores de subopção (*Pages/Index.cshtml.cs*):

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example3)]

Quando o aplicativo é executado, o método `OnGet` retorna uma cadeia de caracteres que mostra os valores da classe de subopção:

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-provided-by-a-view-model-or-with-direct-view-injection"></a>Opções fornecidas por um modelo de exibição ou com a injeção de exibição direta

As opções fornecidas por um modelo de exibição ou com a injeção de exibição direta são demonstradas como o Exemplo &num;4 no [aplicativo de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

As opções podem ser fornecidas em um modelo de exibição ou pela injeção de `IOptions<TOptions>` diretamente em uma exibição (*Pages/Index.cshtml.cs*):

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example4)]

Para a injeção direta, injete `IOptions<MyOptions>` com uma diretiva `@inject`:

[!code-cshtml[](options/sample/Pages/Index.cshtml?range=1-10&highlight=5)]

Quando o aplicativo é executado, os valores de opções são mostrados na página renderizada:

![Os valores de opções Option1: value1_from_json e Option2: -1 são carregados do modelo e pela injeção na exibição.](options/_static/view.png)

::: moniker range=">= aspnetcore-1.1"

## <a name="reload-configuration-data-with-ioptionssnapshot"></a>Recarregar dados de configuração com IOptionsSnapshot

O recarregamento de dados de configuração com `IOptionsSnapshot` é demonstrado no Exemplo &num;5 no [aplicativo de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

[IOptionsSnapshot](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1) dá suporte a opções de recarregamento com sobrecarga mínima de processamento.

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

As opções são calculadas uma vez por solicitação, quando acessadas e armazenadas em cache durante o tempo de vida da solicitação.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

`IOptionsSnapshot` é um instantâneo de [IOptionsMonitor&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) e é atualizado automaticamente sempre que o monitor dispara alterações com base na alteração da fonte de dados.

::: moniker-end

::: moniker range=">= aspnetcore-1.1"

O exemplo a seguir demonstra como um novo `IOptionsSnapshot` é criado após a alteração de *appsettings.json* (*Pages/Index.cshtml.cs*). Várias solicitações ao servidor retornam valores de constante fornecidos pelo arquivo *appsettings.json*, até que o arquivo seja alterado e a configuração seja recarregada.

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example5)]

A seguinte imagem mostra is valores `option1` e `option2` iniciais carregados do arquivo *appsettings.json*:

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

Altere os valores no arquivo *appsettings.json* para `value1_from_json UPDATED` e `200`. Salve o arquivo *appsettings.json*. Atualize o navegador para ver se os valores de opções foram atualizados:

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

## <a name="named-options-support-with-iconfigurenamedoptions"></a>Suporte de opções nomeadas com IConfigureNamedOptions

O suporte de opções nomeadas com [IConfigureNamedOptions](/dotnet/api/microsoft.extensions.options.iconfigurenamedoptions-1) é demonstrado como o Exemplo &num;6 no [aplicativo de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

O suporte de *opções nomeadas* permite que o aplicativo faça a distinção entre as configurações de opções nomeadas. No aplicativo de exemplo, as opções nomeadas são declaradas com a [OptionsServiceCollectionExtensions.Configure&lt;TOptions&gt;(IServiceCollection, String, Action&lt;TOptions&gt;)](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configure), que, por sua vez, chama o método [ConfigureNamedOptions&lt;TOptions&gt;.Configure](/dotnet/api/microsoft.extensions.options.configurenamedoptions-1.configure) do método de extensão:

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example6)]

O aplicativo de exemplo acessa as opções nomeadas com [IOptionsSnapshot&lt;TOptions&gt;.Get](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1.get) (*Pages/Index.cshtml.cs*):

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example6)]

Executando o aplicativo de exemplo, as opções nomeadas são retornadas:

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

Os valores `named_options_1` são fornecidos pela configuração, que são carregados do arquivo *appsettings.json*. Os valores `named_options_2` são fornecidos pelo:

* Delegado `named_options_2` em `ConfigureServices` para `Option1`.
* Valor padrão para `Option2` fornecido pela classe `MyOptions`.

Configure todas as instâncias de opções nomeadas com o método [OptionsServiceCollectionExtensions.ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall). O código a seguir configura `Option1` para todas as instâncias de configuração nomeadas com um valor comum. Adicione o seguinte código manualmente ao método `Configure`:

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

A execução do aplicativo de exemplo após a adição do código produz o seguinte resultado:

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> Todas as opções são instâncias nomeadas. As instâncias `IConfigureOption` existentes são tratadas como sendo direcionadas à instância `Options.DefaultName`, que é `string.Empty`. `IConfigureNamedOptions` também implementa `IConfigureOptions`. A implementação padrão de [IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) ([fonte de referência](https://github.com/aspnet/Options/blob/release/2.0/src/Microsoft.Extensions.Options/IOptionsFactory.cs) tem uma lógica para usar cada uma de forma adequada. A opção nomeada `null` é usada para direcionar todas as instâncias nomeadas, em vez de uma instância nomeada específica ([ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) e [PostConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) usa essa convenção).

## <a name="ipostconfigureoptions"></a>IPostConfigureOptions

Defina a pós-configuração com [IPostConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1). A pós-configuração é executada depois que ocorre toda a configuração de [IConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1):

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

[PostConfigure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1.postconfigure) está disponível para pós-configurar opções nomeadas:

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

Use [PostConfigureAll&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) para pós-configurar todas as instâncias de configuração nomeadas:

```csharp
services.PostConfigureAll<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

::: moniker-end

## <a name="options-factory-monitoring-and-cache"></a>Alocador de opções, monitoramento e cache

[IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) é usado para notificações quando instâncias `TOptions` são alteradas. `IOptionsMonitor` dá suporte a opções recarregáveis, notificações de alteração e `IPostConfigureOptions`.

::: moniker range=">= aspnetcore-2.0"

[IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) é responsável por criar novas instâncias de opção. Ele tem um único método [Create](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1.create). A implementação padrão usa todos os `IConfigureOptions` e `IPostConfigureOptions` registrados e executa toda a configuração primeiro, seguido da pós-configuração. Ela faz distinção entre `IConfigureNamedOptions` e `IConfigureOptions` e chama apenas a interface apropriada.

[IOptionsMonitorCache&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1) é usado por `IOptionsMonitor` para armazenar instâncias `TOptions` em cache. O `IOptionsMonitorCache` invalida as instâncias de opções no monitor, de modo que o valor seja recalculado ([TryRemove](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryremove)). Os valores também podem ser manualmente inseridos com [TryAdd](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryadd). O método [Clear](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.clear) é usado quando todas as instâncias nomeadas devem ser recriadas sob demanda.

::: moniker-end

## <a name="accessing-options-during-startup"></a>Acessando opções durante a inicialização

`IOptions` pode ser usado em `Configure`, pois os serviços são criados antes da execução do método `Configure`. Se um provedor de serviços for criado em `ConfigureServices` para acessar as opções, ele não conterá nenhuma configuração de opções fornecida após sua criação. Portanto, pode haver um estado inconsistente de opções devido à ordenação dos registros de serviço.

Como as opções geralmente são carregadas da configuração, a configuração pode ser usada na inicialização em `Configure` e `ConfigureServices`. Para obter exemplos de como usar a configuração durante a inicialização, consulte o tópico [Inicialização do aplicativo](xref:fundamentals/startup).

## <a name="see-also"></a>Consulte também

* [Configuração](xref:fundamentals/configuration/index)

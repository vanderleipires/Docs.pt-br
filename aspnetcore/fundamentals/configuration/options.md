---
title: "Padrão de opções no núcleo do ASP.NET"
author: guardrex
description: "Saiba como usar o padrão de opções para representar grupos de configurações relacionadas em aplicativos do ASP.NET Core."
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 11/28/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/configuration/options
ms.openlocfilehash: aab96b5313a8632950e51f5586612c1d0d3d176e
ms.sourcegitcommit: 83b5a4715fd25e4eb6f7c8427c0ef03850a7fa07
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/25/2018
---
# <a name="options-pattern-in-aspnet-core"></a>Padrão de opções no núcleo do ASP.NET

Por [Luke Latham](https://github.com/guardrex)

O padrão de opções usa classes de opções para representar grupos de configurações relacionadas. Quando as definições de configuração são isoladas pelo recurso em classes de opções separadas, o aplicativo cumpre dois princípios de engenharia de software importantes:

* O [princípio de diferenciação de Interface (ISP)](http://deviq.com/interface-segregation-principle/): recursos (classes) que dependem de definições de configuração dependem apenas as definições de configuração que eles usam.
* [Separação de preocupações](http://deviq.com/separation-of-concerns/): as configurações diferentes partes do aplicativo não são dependentes ou acoplado um ao outro.

[Exibir ou baixar o código de exemplo](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample)) Este artigo é mais fácil de seguir com o aplicativo de exemplo.

## <a name="basic-options-configuration"></a>Configuração de opções básicas

Configuração de opções básicas é demonstrada como exemplo &num;1 no [aplicativo de exemplo](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

Uma classe de opções deve ser não-abstrato com um construtor público sem parâmetros. A seguinte classe, `MyOptions`, tem duas propriedades, `Option1` e `Option2`. Definindo valores padrão é opcional, mas o construtor de classe no exemplo a seguir define o valor padrão de `Option1`. `Option2`tem um valor padrão definido por inicializar a propriedade diretamente (*Models/MyOptions.cs*):

[!code-csharp[Main](options/sample/Models/MyOptions.cs?name=snippet1)]

O `MyOptions` classe é adicionada ao contêiner de serviço com [IConfigureOptions&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) e associado à configuração:

[!code-csharp[Main](options/sample/Startup.cs?name=snippet_Example1)]

A seguinte página modelo usa [injeção de dependência de construtor](xref:fundamentals/dependency-injection#what-is-dependency-injection) com [IOptions&lt;TOptions&gt; ](/dotnet/api/Microsoft.Extensions.Options.IOptions-1) para acessar as configurações (*Pages/Index.cshtml.cs*):

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example1)]

O exemplo *appSettings. JSON* arquivo Especifica valores para `option1` e `option2`:

[!code-json[Main](options/sample/appsettings.json)]

Quando o aplicativo é executado, o modelo de página `OnGet` método retorna uma cadeia de caracteres que mostra os valores de opção de classe:

```html
option1 = value1_from_json, option2 = -1
```

## <a name="configure-simple-options-with-a-delegate"></a>Configurar opções simples com um delegado

Configurando opções simples com um delegado é demonstrado como exemplo &num;2 no [aplicativo de exemplo](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

Use um delegado para definir valores de opções. O aplicativo de exemplo usa o `MyOptionsWithDelegateConfig` classe (*Models/MyOptionsWithDelegateConfig.cs*):

[!code-csharp[Main](options/sample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

No código a seguir, um segundo `IConfigureOptions<TOptions>` serviço é adicionado ao contêiner de serviço. Ele usa um delegado para configurar a associação com `MyOptionsWithDelegateConfig`:

[!code-csharp[Main](options/sample/Startup.cs?name=snippet_Example2)]

*Index.cshtml.cs*:

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example2)]

Você pode adicionar vários provedores de configuração. Provedores de configuração estão disponíveis em pacotes do NuGet. São aplicados para que eles são registrados.

Cada chamada para [configurar&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1.configure) adiciona um `IConfigureOptions<TOptions>` serviço ao contêiner de serviço. No exemplo anterior, os valores de `Option1` e `Option2` estão especificados na *appSettings. JSON*, mas os valores de `Option1` e `Option2` são substituídos pelo delegado configurado.

Quando mais de um serviço de configuração é habilitado, a última fonte de configuração especificada *wins* e define o valor de configuração. Quando o aplicativo é executado, o modelo de página `OnGet` método retorna uma cadeia de caracteres que mostra os valores de opção de classe:

```html
delegate_option1 = value1_configured_by_delgate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a>Configuração de subopções

Configuração de subopções é demonstrada como exemplo &num;3 no [aplicativo de exemplo](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

Aplicativos devem criar classes de opções que pertencem aos grupos de recurso específico (classes) no aplicativo. Partes do aplicativo que exigem valores de configuração só devem ter acesso para os valores de configuração que eles usam.

Ao associar opções de configuração, cada propriedade no tipo de opções está associada a uma chave de configuração do formulário `property[:sub-property:]`. Por exemplo, o `MyOptions.Option1` propriedade está associada à chave `Option1`, que são lidos a partir de `option1` propriedade em *appSettings. JSON*.

No código a seguir, um terceiro `IConfigureOptions<TOptions>` serviço é adicionado ao contêiner de serviço. Ele é ligado `MySubOptions` a seção `subsection` do *appSettings. JSON* arquivo:

[!code-csharp[Main](options/sample/Startup.cs?name=snippet_Example3)]

O `GetSection` requer o método de extensão de [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) pacote NuGet. Se o aplicativo usa o [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) metapackage, o pacote é incluído automaticamente.

O exemplo *appSettings. JSON* arquivo define uma `subsection` membro com chaves para `suboption1` e `suboption2`:

[!code-json[Main](options/sample/appsettings.json?highlight=4-7)]

O `MySubOptions` classe define propriedades, `SubOption1` e `SubOption2`, para manter os valores de opção sub (*Models/MySubOptions.cs*):

[!code-csharp[Main](options/sample/Models/MySubOptions.cs?name=snippet1)]

O modelo de página `OnGet` método retorna uma cadeia de caracteres com os valores de opção sub (*Pages/Index.cshtml.cs*):

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example3)]

Quando o aplicativo é executado, o `OnGet` método retorna uma cadeia de caracteres com a opção sub valores de classe:

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-provided-by-a-view-model-or-with-direct-view-injection"></a>Opções fornecidas por um modelo de exibição ou com a injeção de modo direto

Opções fornecidas por um modelo de exibição ou com a injeção de modo direto é demonstrado como exemplo &num;4 a [aplicativo de exemplo](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

Opções podem ser fornecidas em um modelo de exibição ou injetando `IOptions<TOptions>` diretamente em um modo de exibição (*Pages/Index.cshtml.cs*):

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example4)]

Para a injeção do direct injetar `IOptions<MyOptions>` com um `@inject` diretiva:

[!code-cshtml[Main](options/sample/Pages/Index.cshtml?range=1-10&highlight=5)]

Quando o aplicativo é executado, os valores de opção são mostrados na página renderizada:

![Opções de valores de opção 1: value1_from_json e Option2: -1 são carregados do modelo e pela inclusão no modo de exibição.](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a>Recarregue os dados de configuração com IOptionsSnapshot

Recarregar os dados de configuração com `IOptionsSnapshot` é demonstrado no exemplo &num;5 a [aplicativo de exemplo](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

*Exige o ASP.NET Core 1.1 ou posterior.*

[IOptionsSnapshot](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1) oferece suporte a opções com sobrecarga mínima de processamento de recarregar. No ASP.NET Core 1.1, `IOptionsSnapshot` é um instantâneo de [IOptionsMonitor&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) e atualizações automaticamente sempre que o monitor aciona a alterações de acordo com a alteração de fonte de dados. No ASP.NET Core 2.0 e posterior, opções são calculadas uma vez por solicitação quando acessados e armazenados em cache para o tempo de vida da solicitação.

O exemplo a seguir demonstra como um novo `IOptionsSnapshot` é criada após *appSettings. JSON* alterações (*Pages/Index.cshtml.cs*). Várias solicitações ao servidor retornam valores de constantes fornecidos pelo *appSettings. JSON* até que o arquivo é alterado e recargas de configuração de arquivo.

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example5)]

A imagem a seguir mostra a inicial `option1` e `option2` valores carregados a partir de *appSettings. JSON* arquivo:

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

Alterar os valores de *appSettings. JSON* o arquivo para `value1_from_json UPDATED` e `200`. Salve o *appSettings. JSON* arquivo. Atualize o navegador para ver se os valores de opções são atualizados:

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a>Opções de suporte com IConfigureNamedOptions de chamada

Chamado opções de suporte com [IConfigureNamedOptions](/dotnet/api/microsoft.extensions.options.iconfigurenamedoptions-1) é demonstrado como exemplo &num;6 a [aplicativo de exemplo](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

*Exige o ASP.NET Core 2.0 ou posterior.*

*Opções de chamada* suporte permite que o aplicativo distinguir entre as configurações de opções nomeada. No aplicativo de exemplo, as opções nomeadas são declaradas com a [ConfigureNamedOptions&lt;TOptions&gt;. Configurar](/dotnet/api/microsoft.extensions.options.configurenamedoptions-1.configure) método:

[!code-csharp[Main](options/sample/Startup.cs?name=snippet_Example6)]

O aplicativo de exemplo acessa as opções nomeadas com [IOptionsSnapshot&lt;TOptions&gt;. Obter](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1.get) (*Pages/Index.cshtml.cs*):

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example6)]

Executando o aplicativo de exemplo, as opções nomeadas são retornadas:

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

`named_options_1`os valores são fornecidos de configuração, que é carregado a partir de *appSettings. JSON* arquivo. `named_options_2`os valores são fornecidos por:

* O `named_options_2` delegar em `ConfigureServices` para `Option1`.
* O valor padrão para `Option2` fornecidos pelo `MyOptions` classe.

Configure todas as instâncias nomeadas opções com a [OptionsServiceCollectionExtensions.ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) método. O código a seguir configura `Option1` para todas as instâncias de configuração com um valor comum nomeadas. Adicione o seguinte código manualmente para o `Configure` método:

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

Executando o aplicativo de exemplo depois de adicionar o código produz o seguinte resultado:

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> No ASP.NET Core 2.0 e posterior, todas as opções são instâncias nomeadas. Existente `IConfigureOption` instâncias são tratadas como direcionamento o `Options.DefaultName` instância, que é `string.Empty`. `IConfigureNamedOptions`também implementa `IConfigureOptions`. A implementação padrão da [IOptionsFactory&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) ([fonte de referência](https://github.com/aspnet/Options/blob/release/2.0.0/src/Microsoft.Extensions.Options/OptionsFactory.cs)) tem lógica para usar cada adequadamente. O `null` opção nomeada é usada para direcionar todas as instâncias nomeadas, em vez de uma determinada instância nomeada ([ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) e [PostConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) usa essa convenção).

## <a name="ipostconfigureoptions"></a>IPostConfigureOptions

*Exige o ASP.NET Core 2.0 ou posterior.*

Definir postconfiguration com [IPostConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1). Postconfiguration é executado depois que todos os [IConfigureOptions&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) configuração ocorre:

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

[PostConfigure&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1.postconfigure) está disponível para pós-configurar opções nomeadas:

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

Use [PostConfigureAll&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) após configurar todos denominado instâncias de configuração:

```csharp
services.PostConfigureAll<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="options-factory-monitoring-and-cache"></a>Fábrica de opções, o monitoramento e o cache

[IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) é usado para notificações quando `TOptions` instâncias de alteração. `IOptionsMonitor`oferece suporte a opções de reloadable, alterar notificações, e `IPostConfigureOptions`.

[IOptionsFactory&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) (núcleo ASP.NET 2.0 ou posterior) é responsável por criar novas instâncias de opções. Ele tem um único [criar](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1.create) método. A implementação padrão usa todos os `IConfigureOptions` e `IPostConfigureOptions` e executa todos os o configura o primeiro, seguido de pós-configura. Ele faz distinção entre `IConfigureNamedOptions` e `IConfigureOptions` e só chama a interface apropriada.

[IOptionsMonitorCache&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1) (núcleo ASP.NET 2.0 ou posterior) é usado por `IOptionsMonitor` cache `TOptions` instâncias. O `IOptionsMonitorCache` invalida as instâncias de opções no monitor de forma que o valor é recalculado ([TryRemove](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryremove)). Os valores podem ser manualmente introduzidos bem com [TryAdd](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryadd). O [limpar](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.clear) método é usado quando todas as instâncias nomeadas devem ser recriadas sob demanda.

## <a name="accessing-options-during-startup"></a>Acessando opções durante a inicialização

`IOptions`pode ser usado em `Configure`, uma vez que os serviços são criados antes do `Configure` método é executado. Se um provedor de serviço é criado `ConfigureServices` para acessar as opções, não conter nenhum opções configurações fornecidas após o provedor de serviço é criado. Portanto, um estado inconsistente opções pode existir devido a ordenação dos registros de serviço.

Como opções geralmente são carregadas de configuração, a configuração pode ser usada na inicialização no `Configure` e `ConfigureServices`. Para obter exemplos do uso da configuração durante a inicialização, consulte o [inicialização do aplicativo](xref:fundamentals/startup) tópico.

## <a name="see-also"></a>Consulte também

* [Configuração](xref:fundamentals/configuration/index)

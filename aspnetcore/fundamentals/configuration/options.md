---
title: Padrão de opções no ASP.NET Core
author: guardrex
description: Descubra como usar o padrão de opções para representar grupos de configurações relacionadas em aplicativos ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 11/28/2017
uid: fundamentals/configuration/options
ms.openlocfilehash: ef6b0117b88c4c79771f0280267bd99993028ac8
ms.sourcegitcommit: 028ad28c546de706ace98066c76774de33e4ad20
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/08/2018
ms.locfileid: "39655414"
---
# <a name="options-pattern-in-aspnet-core"></a><span data-ttu-id="8738c-103">Padrão de opções no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8738c-103">Options pattern in ASP.NET Core</span></span>

<span data-ttu-id="8738c-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="8738c-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="8738c-105">O padrão de opções usa classes para representar grupos de configurações relacionadas.</span><span class="sxs-lookup"><span data-stu-id="8738c-105">The options pattern uses classes to represent groups of related settings.</span></span> <span data-ttu-id="8738c-106">Quando as definições de configuração são isoladas por recurso em classes separadas, o aplicativo segue dois princípios importantes de engenharia de software:</span><span class="sxs-lookup"><span data-stu-id="8738c-106">When configuration settings are isolated by feature into separate classes, the app adheres to two important software engineering principles:</span></span>

* <span data-ttu-id="8738c-107">O [ISP (Princípio de Segregação da Interface)](http://deviq.com/interface-segregation-principle/): os recursos (classes) que dependem de definições de configuração dependem apenas das definições de configuração usadas por eles.</span><span class="sxs-lookup"><span data-stu-id="8738c-107">The [Interface Segregation Principle (ISP)](http://deviq.com/interface-segregation-principle/): Features (classes) that depend on configuration settings depend only on the configuration settings that they use.</span></span>
* <span data-ttu-id="8738c-108">[Separação de Interesses](http://deviq.com/separation-of-concerns/): as configurações para diferentes partes do aplicativo não são dependentes nem acopladas entre si.</span><span class="sxs-lookup"><span data-stu-id="8738c-108">[Separation of Concerns](http://deviq.com/separation-of-concerns/): Settings for different parts of the app aren't dependent or coupled to one another.</span></span>

<span data-ttu-id="8738c-109">[Exiba ou baixe o código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample)) Este artigo é mais fácil de ser acompanhado com o aplicativo de exemplo.</span><span class="sxs-lookup"><span data-stu-id="8738c-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)) This article is easier to follow with the sample app.</span></span>

## <a name="basic-options-configuration"></a><span data-ttu-id="8738c-110">Configuração de opções básicas</span><span class="sxs-lookup"><span data-stu-id="8738c-110">Basic options configuration</span></span>

<span data-ttu-id="8738c-111">A configuração de opções básicas é demonstrada como o Exemplo &num;1 no [aplicativo de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="8738c-111">Basic options configuration is demonstrated as Example &num;1 in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="8738c-112">Uma classe de opções deve ser não abstrata com um construtor público sem parâmetros.</span><span class="sxs-lookup"><span data-stu-id="8738c-112">An options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="8738c-113">A classe a seguir, `MyOptions`, tem duas propriedades, `Option1` e `Option2`.</span><span class="sxs-lookup"><span data-stu-id="8738c-113">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span></span> <span data-ttu-id="8738c-114">A configuração de valores padrão é opcional, mas o construtor de classe no exemplo a seguir define o valor padrão de `Option1`.</span><span class="sxs-lookup"><span data-stu-id="8738c-114">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span></span> <span data-ttu-id="8738c-115">`Option2` tem um valor padrão definido com a inicialização da propriedade diretamente (*Models/MyOptions.cs*):</span><span class="sxs-lookup"><span data-stu-id="8738c-115">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span></span>

[!code-csharp[](options/sample/Models/MyOptions.cs?name=snippet1)]

<span data-ttu-id="8738c-116">A classe `MyOptions` é adicionada ao contêiner de serviço com [Configure&lt;TOptions&gt;(IServiceCollection, IConfiguration)](/dotnet/api/microsoft.extensions.dependencyinjection.optionsconfigurationservicecollectionextensions.configure#Microsoft_Extensions_DependencyInjection_OptionsConfigurationServiceCollectionExtensions_Configure__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_Microsoft_Extensions_Configuration_IConfiguration_) e é associada à configuração:</span><span class="sxs-lookup"><span data-stu-id="8738c-116">The `MyOptions` class is added to the service container with [Configure&lt;TOptions&gt;(IServiceCollection, IConfiguration)](/dotnet/api/microsoft.extensions.dependencyinjection.optionsconfigurationservicecollectionextensions.configure#Microsoft_Extensions_DependencyInjection_OptionsConfigurationServiceCollectionExtensions_Configure__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_Microsoft_Extensions_Configuration_IConfiguration_) and bound to configuration:</span></span>

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example1)]

<span data-ttu-id="8738c-117">O seguinte modelo de página usa a [injeção de dependência de construtor](xref:mvc/controllers/dependency-injection) com [IOptions&lt;TOptions&gt;](/dotnet/api/Microsoft.Extensions.Options.IOptions-1) para acessar as configurações (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="8738c-117">The following page model uses [constructor dependency injection](xref:mvc/controllers/dependency-injection) with [IOptions&lt;TOptions&gt;](/dotnet/api/Microsoft.Extensions.Options.IOptions-1) to access the settings (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example1)]

<span data-ttu-id="8738c-118">O arquivo *appsettings.json* de exemplo especifica valores para `option1` e `option2`:</span><span class="sxs-lookup"><span data-stu-id="8738c-118">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span></span>

[!code-json[](options/sample/appsettings.json?highlight=2-3)]

<span data-ttu-id="8738c-119">Quando o aplicativo é executado, o método `OnGet` do modelo de página retorna uma cadeia de caracteres que mostra os valores da classe de opção:</span><span class="sxs-lookup"><span data-stu-id="8738c-119">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
option1 = value1_from_json, option2 = -1
```

> [!NOTE]
> <span data-ttu-id="8738c-120">Ao usar um [ConfigurationBuilder](/dotnet/api/system.configuration.configurationbuilder) personalizado para carregar opções de configuração de um arquivo de configuração, confirme se o caminho de base está corretamente configurado:</span><span class="sxs-lookup"><span data-stu-id="8738c-120">When using a custom [ConfigurationBuilder](/dotnet/api/system.configuration.configurationbuilder) to load options configuration from a settings file, confirm that the base path is set correctly:</span></span>
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
> <span data-ttu-id="8738c-121">Não é necessário configurar o caminho de base explicitamente ao carregar opções de configuração do arquivo de configuração por meio do [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span><span class="sxs-lookup"><span data-stu-id="8738c-121">Explicitly setting the base path isn't required when loading options configuration from the settings file via [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span></span>

## <a name="configure-simple-options-with-a-delegate"></a><span data-ttu-id="8738c-122">Configurar opções simples com um delegado</span><span class="sxs-lookup"><span data-stu-id="8738c-122">Configure simple options with a delegate</span></span>

<span data-ttu-id="8738c-123">A configuração de opções simples com um delegado é demonstrada como o Exemplo &num;2 no [aplicativo de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="8738c-123">Configuring simple options with a delegate is demonstrated as Example &num;2 in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="8738c-124">Use um delegado para definir valores de opções.</span><span class="sxs-lookup"><span data-stu-id="8738c-124">Use a delegate to set options values.</span></span> <span data-ttu-id="8738c-125">O aplicativo de exemplo usa a classe `MyOptionsWithDelegateConfig` (*Models/MyOptionsWithDelegateConfig.cs*):</span><span class="sxs-lookup"><span data-stu-id="8738c-125">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span></span>

[!code-csharp[](options/sample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

<span data-ttu-id="8738c-126">No código a seguir, um segundo serviço `IConfigureOptions<TOptions>` é adicionado ao contêiner de serviço.</span><span class="sxs-lookup"><span data-stu-id="8738c-126">In the following code, a second `IConfigureOptions<TOptions>` service is added to the service container.</span></span> <span data-ttu-id="8738c-127">Ele usa um delegado para configurar a associação com `MyOptionsWithDelegateConfig`:</span><span class="sxs-lookup"><span data-stu-id="8738c-127">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span></span>

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example2)]

<span data-ttu-id="8738c-128">*Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="8738c-128">*Index.cshtml.cs*:</span></span>

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example2)]

<span data-ttu-id="8738c-129">Adicione vários provedores de configuração.</span><span class="sxs-lookup"><span data-stu-id="8738c-129">You can add multiple configuration providers.</span></span> <span data-ttu-id="8738c-130">Provedores de configuração estão disponíveis em pacotes NuGet.</span><span class="sxs-lookup"><span data-stu-id="8738c-130">Configuration providers are available in NuGet packages.</span></span> <span data-ttu-id="8738c-131">Eles são aplicados para que sejam registrados.</span><span class="sxs-lookup"><span data-stu-id="8738c-131">They're applied in order that they're registered.</span></span>

<span data-ttu-id="8738c-132">Cada chamada a [Configure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1.configure) adiciona um serviço `IConfigureOptions<TOptions>` ao contêiner de serviço.</span><span class="sxs-lookup"><span data-stu-id="8738c-132">Each call to [Configure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1.configure) adds an `IConfigureOptions<TOptions>` service to the service container.</span></span> <span data-ttu-id="8738c-133">No exemplo anterior, os valores `Option1` e `Option2` são especificados em *appsettings.json*, mas os valores `Option1` e `Option2` são substituídos pelo delegado configurado.</span><span class="sxs-lookup"><span data-stu-id="8738c-133">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span></span>

<span data-ttu-id="8738c-134">Quando mais de um serviço de configuração é habilitado, a última fonte de configuração especificada *vence* e define o valor de configuração.</span><span class="sxs-lookup"><span data-stu-id="8738c-134">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span></span> <span data-ttu-id="8738c-135">Quando o aplicativo é executado, o método `OnGet` do modelo de página retorna uma cadeia de caracteres que mostra os valores da classe de opção:</span><span class="sxs-lookup"><span data-stu-id="8738c-135">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
delegate_option1 = value1_configured_by_delgate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a><span data-ttu-id="8738c-136">Configuração de subopções</span><span class="sxs-lookup"><span data-stu-id="8738c-136">Suboptions configuration</span></span>

<span data-ttu-id="8738c-137">A configuração de subopções básicas é demonstrada como o Exemplo &num;3 no [aplicativo de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="8738c-137">Suboptions configuration is demonstrated as Example &num;3 in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="8738c-138">Os aplicativos devem criar classes de opções que pertencem a grupos de recursos específicos (classes) no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="8738c-138">Apps should create options classes that pertain to specific feature groups (classes) in the app.</span></span> <span data-ttu-id="8738c-139">Partes do aplicativo que exigem valores de configuração devem ter acesso apenas aos valores de configuração usados por elas.</span><span class="sxs-lookup"><span data-stu-id="8738c-139">Parts of the app that require configuration values should only have access to the configuration values that they use.</span></span>

<span data-ttu-id="8738c-140">Ao associar opções à configuração, cada propriedade no tipo de opções é associada a uma chave de configuração do formato `property[:sub-property:]`.</span><span class="sxs-lookup"><span data-stu-id="8738c-140">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="8738c-141">Por exemplo, a propriedade `MyOptions.Option1` é associada à chave `Option1`, que é lida da propriedade `option1` em *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="8738c-141">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span>

<span data-ttu-id="8738c-142">No código a seguir, um terceiro serviço `IConfigureOptions<TOptions>` é adicionado ao contêiner de serviço.</span><span class="sxs-lookup"><span data-stu-id="8738c-142">In the following code, a third `IConfigureOptions<TOptions>` service is added to the service container.</span></span> <span data-ttu-id="8738c-143">Ele associa `MySubOptions` à seção `subsection` do arquivo *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="8738c-143">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example3)]

<span data-ttu-id="8738c-144">O método de extensão `GetSection` exige o pacote NuGet [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/).</span><span class="sxs-lookup"><span data-stu-id="8738c-144">The `GetSection` extension method requires the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) NuGet package.</span></span> <span data-ttu-id="8738c-145">Se o aplicativo usa o [metapacote Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 ou posterior), o pacote é automaticamente incluído.</span><span class="sxs-lookup"><span data-stu-id="8738c-145">If the app uses the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later), the package is automatically included.</span></span>

<span data-ttu-id="8738c-146">O arquivo *appsettings.json* de exemplo define um membro `subsection` com chaves para `suboption1` e `suboption2`:</span><span class="sxs-lookup"><span data-stu-id="8738c-146">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span></span>

[!code-json[](options/sample/appsettings.json?highlight=4-7)]

<span data-ttu-id="8738c-147">A classe `MySubOptions` define as propriedades `SubOption1` e `SubOption2`, para armazenar os valores de opções (*Models/MySubOptions.cs*):</span><span class="sxs-lookup"><span data-stu-id="8738c-147">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the options values (*Models/MySubOptions.cs*):</span></span>

[!code-csharp[](options/sample/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="8738c-148">O método `OnGet` do modelo da página retorna uma cadeia de caracteres com os valores de opções (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="8738c-148">The page model's `OnGet` method returns a string with the options values (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example3)]

<span data-ttu-id="8738c-149">Quando o aplicativo é executado, o método `OnGet` retorna uma cadeia de caracteres que mostra os valores da classe de subopção:</span><span class="sxs-lookup"><span data-stu-id="8738c-149">When the app is run, the `OnGet` method returns a string showing the sub-option class values:</span></span>

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-provided-by-a-view-model-or-with-direct-view-injection"></a><span data-ttu-id="8738c-150">Opções fornecidas por um modelo de exibição ou com a injeção de exibição direta</span><span class="sxs-lookup"><span data-stu-id="8738c-150">Options provided by a view model or with direct view injection</span></span>

<span data-ttu-id="8738c-151">As opções fornecidas por um modelo de exibição ou com a injeção de exibição direta são demonstradas como o Exemplo &num;4 no [aplicativo de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="8738c-151">Options provided by a view model or with direct view injection is demonstrated as Example &num;4 in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="8738c-152">As opções podem ser fornecidas em um modelo de exibição ou pela injeção de `IOptions<TOptions>` diretamente em uma exibição (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="8738c-152">Options can be supplied in a view model or by injecting `IOptions<TOptions>` directly into a view (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example4)]

<span data-ttu-id="8738c-153">Para a injeção direta, injete `IOptions<MyOptions>` com uma diretiva `@inject`:</span><span class="sxs-lookup"><span data-stu-id="8738c-153">For direct injection, inject `IOptions<MyOptions>` with an `@inject` directive:</span></span>

[!code-cshtml[](options/sample/Pages/Index.cshtml?range=1-10&highlight=5)]

<span data-ttu-id="8738c-154">Quando o aplicativo é executado, os valores de opções são mostrados na página renderizada:</span><span class="sxs-lookup"><span data-stu-id="8738c-154">When the app is run, the options values are shown in the rendered page:</span></span>

![Os valores de opções Option1: value1_from_json e Option2: -1 são carregados do modelo e pela injeção na exibição.](options/_static/view.png)

::: moniker range=">= aspnetcore-1.1"

## <a name="reload-configuration-data-with-ioptionssnapshot"></a><span data-ttu-id="8738c-156">Recarregar dados de configuração com IOptionsSnapshot</span><span class="sxs-lookup"><span data-stu-id="8738c-156">Reload configuration data with IOptionsSnapshot</span></span>

<span data-ttu-id="8738c-157">O recarregamento de dados de configuração com `IOptionsSnapshot` é demonstrado no Exemplo &num;5 no [aplicativo de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="8738c-157">Reloading configuration data with `IOptionsSnapshot` is demonstrated in Example &num;5 in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="8738c-158">[IOptionsSnapshot](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1) dá suporte a opções de recarregamento com sobrecarga mínima de processamento.</span><span class="sxs-lookup"><span data-stu-id="8738c-158">[IOptionsSnapshot](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1) supports reloading options with minimal processing overhead.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="8738c-159">As opções são calculadas uma vez por solicitação, quando acessadas e armazenadas em cache durante o tempo de vida da solicitação.</span><span class="sxs-lookup"><span data-stu-id="8738c-159">Options are computed once per request when accessed and cached for the lifetime of the request.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="8738c-160">`IOptionsSnapshot` é um instantâneo de [IOptionsMonitor&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) e é atualizado automaticamente sempre que o monitor dispara alterações com base na alteração da fonte de dados.</span><span class="sxs-lookup"><span data-stu-id="8738c-160">`IOptionsSnapshot` is a snapshot of [IOptionsMonitor&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) and updates automatically whenever the monitor triggers changes based on the data source changing.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-1.1"

<span data-ttu-id="8738c-161">O exemplo a seguir demonstra como um novo `IOptionsSnapshot` é criado após a alteração de *appsettings.json* (*Pages/Index.cshtml.cs*).</span><span class="sxs-lookup"><span data-stu-id="8738c-161">The following example demonstrates how a new `IOptionsSnapshot` is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span></span> <span data-ttu-id="8738c-162">Várias solicitações ao servidor retornam valores de constante fornecidos pelo arquivo *appsettings.json*, até que o arquivo seja alterado e a configuração seja recarregada.</span><span class="sxs-lookup"><span data-stu-id="8738c-162">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span></span>

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example5)]

<span data-ttu-id="8738c-163">A seguinte imagem mostra is valores `option1` e `option2` iniciais carregados do arquivo *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="8738c-163">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span></span>

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

<span data-ttu-id="8738c-164">Altere os valores no arquivo *appsettings.json* para `value1_from_json UPDATED` e `200`.</span><span class="sxs-lookup"><span data-stu-id="8738c-164">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span></span> <span data-ttu-id="8738c-165">Salve o arquivo *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="8738c-165">Save the *appsettings.json* file.</span></span> <span data-ttu-id="8738c-166">Atualize o navegador para ver se os valores de opções foram atualizados:</span><span class="sxs-lookup"><span data-stu-id="8738c-166">Refresh the browser to see that the options values are updated:</span></span>

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

## <a name="named-options-support-with-iconfigurenamedoptions"></a><span data-ttu-id="8738c-167">Suporte de opções nomeadas com IConfigureNamedOptions</span><span class="sxs-lookup"><span data-stu-id="8738c-167">Named options support with IConfigureNamedOptions</span></span>

<span data-ttu-id="8738c-168">O suporte de opções nomeadas com [IConfigureNamedOptions](/dotnet/api/microsoft.extensions.options.iconfigurenamedoptions-1) é demonstrado como o Exemplo &num;6 no [aplicativo de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="8738c-168">Named options support with [IConfigureNamedOptions](/dotnet/api/microsoft.extensions.options.iconfigurenamedoptions-1) is demonstrated as Example &num;6 in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="8738c-169">O suporte de *opções nomeadas* permite que o aplicativo faça a distinção entre as configurações de opções nomeadas.</span><span class="sxs-lookup"><span data-stu-id="8738c-169">*Named options* support allows the app to distinguish between named options configurations.</span></span> <span data-ttu-id="8738c-170">No aplicativo de exemplo, as opções nomeadas são declaradas com a [OptionsServiceCollectionExtensions.Configure&lt;TOptions&gt;(IServiceCollection, String, Action&lt;TOptions&gt;)](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configure), que, por sua vez, chama o método [ConfigureNamedOptions&lt;TOptions&gt;.Configure](/dotnet/api/microsoft.extensions.options.configurenamedoptions-1.configure) do método de extensão:</span><span class="sxs-lookup"><span data-stu-id="8738c-170">In the sample app, named options are declared with the [OptionsServiceCollectionExtensions.Configure&lt;TOptions&gt;(IServiceCollection, String, Action&lt;TOptions&gt;)](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configure) which in turn calls the extension method [ConfigureNamedOptions&lt;TOptions&gt;.Configure](/dotnet/api/microsoft.extensions.options.configurenamedoptions-1.configure) method:</span></span>

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example6)]

<span data-ttu-id="8738c-171">O aplicativo de exemplo acessa as opções nomeadas com [IOptionsSnapshot&lt;TOptions&gt;.Get](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1.get) (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="8738c-171">The sample app accesses the named options with [IOptionsSnapshot&lt;TOptions&gt;.Get](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1.get) (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example6)]

<span data-ttu-id="8738c-172">Executando o aplicativo de exemplo, as opções nomeadas são retornadas:</span><span class="sxs-lookup"><span data-stu-id="8738c-172">Running the sample app, the named options are returned:</span></span>

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

<span data-ttu-id="8738c-173">Os valores `named_options_1` são fornecidos pela configuração, que são carregados do arquivo *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="8738c-173">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span></span> <span data-ttu-id="8738c-174">Os valores `named_options_2` são fornecidos pelo:</span><span class="sxs-lookup"><span data-stu-id="8738c-174">`named_options_2` values are provided by:</span></span>

* <span data-ttu-id="8738c-175">Delegado `named_options_2` em `ConfigureServices` para `Option1`.</span><span class="sxs-lookup"><span data-stu-id="8738c-175">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span></span>
* <span data-ttu-id="8738c-176">Valor padrão para `Option2` fornecido pela classe `MyOptions`.</span><span class="sxs-lookup"><span data-stu-id="8738c-176">The default value for `Option2` provided by the `MyOptions` class.</span></span>

<span data-ttu-id="8738c-177">Configure todas as instâncias de opções nomeadas com o método [OptionsServiceCollectionExtensions.ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall).</span><span class="sxs-lookup"><span data-stu-id="8738c-177">Configure all named options instances with the [OptionsServiceCollectionExtensions.ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) method.</span></span> <span data-ttu-id="8738c-178">O código a seguir configura `Option1` para todas as instâncias de configuração nomeadas com um valor comum.</span><span class="sxs-lookup"><span data-stu-id="8738c-178">The following code configures `Option1` for all named configuration instances with a common value.</span></span> <span data-ttu-id="8738c-179">Adicione o seguinte código manualmente ao método `Configure`:</span><span class="sxs-lookup"><span data-stu-id="8738c-179">Add the following code manually to the `Configure` method:</span></span>

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

<span data-ttu-id="8738c-180">A execução do aplicativo de exemplo após a adição do código produz o seguinte resultado:</span><span class="sxs-lookup"><span data-stu-id="8738c-180">Running the sample app after adding the code produces the following result:</span></span>

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> <span data-ttu-id="8738c-181">Todas as opções são instâncias nomeadas.</span><span class="sxs-lookup"><span data-stu-id="8738c-181">All options are named instances.</span></span> <span data-ttu-id="8738c-182">As instâncias `IConfigureOption` existentes são tratadas como sendo direcionadas à instância `Options.DefaultName`, que é `string.Empty`.</span><span class="sxs-lookup"><span data-stu-id="8738c-182">Existing `IConfigureOption` instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span></span> <span data-ttu-id="8738c-183">`IConfigureNamedOptions` também implementa `IConfigureOptions`.</span><span class="sxs-lookup"><span data-stu-id="8738c-183">`IConfigureNamedOptions` also implements `IConfigureOptions`.</span></span> <span data-ttu-id="8738c-184">A implementação padrão de [IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) ([fonte de referência](https://github.com/aspnet/Options/blob/release/2.0/src/Microsoft.Extensions.Options/IOptionsFactory.cs) tem uma lógica para usar cada uma de forma adequada.</span><span class="sxs-lookup"><span data-stu-id="8738c-184">The default implementation of the [IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) ([reference source](https://github.com/aspnet/Options/blob/release/2.0/src/Microsoft.Extensions.Options/IOptionsFactory.cs) has logic to use each appropriately.</span></span> <span data-ttu-id="8738c-185">A opção nomeada `null` é usada para direcionar todas as instâncias nomeadas, em vez de uma instância nomeada específica ([ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) e [PostConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) usa essa convenção).</span><span class="sxs-lookup"><span data-stu-id="8738c-185">The `null` named option is used to target all of the named instances instead of a specific named instance ([ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) and [PostConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) use this convention).</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

## <a name="options-validation"></a><span data-ttu-id="8738c-186">Validação de opções</span><span class="sxs-lookup"><span data-stu-id="8738c-186">Options validation</span></span>

<span data-ttu-id="8738c-187">A validação de opções permite validar as opções depois de configurá-las.</span><span class="sxs-lookup"><span data-stu-id="8738c-187">Options validation allows you to validate options when options are configured.</span></span> <span data-ttu-id="8738c-188">Chame `Validate` com um método de validação que retorna `true` quando as opções são válidas, e `false` quando não são:</span><span class="sxs-lookup"><span data-stu-id="8738c-188">Call `Validate` with a validation method that returns `true` if options are valid and `false` if they aren't valid:</span></span>

```csharp
// Registration
services.AddOptions<MyOptions>("optionalOptionsName")
    .Configure(o => { }) // Configure the options
    .Validate(o => YourValidationShouldReturnTrueIfValid(o), 
        "custom error");
        
// Consumption
var monitor = services.BuildServiceProvider()
    .GetService<IOptionsMonitor<MyOptions>>();
  
try
{
    var options = monitor.Get("optionalOptionsName");
} 
catch (OptionsValidationException e) 
{
   // e.OptionsName returns "optionalOptionsName"
   // e.OptionsType returns typeof(MyOptions)
   // e.Failures returns a list of errors, which would contain 
   //     "custom error"
}
```

<span data-ttu-id="8738c-189">O exemplo anterior define a instância de opções nomeadas como `optionalOptionsName`.</span><span class="sxs-lookup"><span data-stu-id="8738c-189">The preceding example sets the named options instance to `optionalOptionsName`.</span></span> <span data-ttu-id="8738c-190">A instância de opções padrão é `Options.DefaultName`.</span><span class="sxs-lookup"><span data-stu-id="8738c-190">The default options instance is `Options.DefaultName`.</span></span>

<span data-ttu-id="8738c-191">A validação é executada após a criação da instância de opções.</span><span class="sxs-lookup"><span data-stu-id="8738c-191">Validation runs when the options instance is created.</span></span> <span data-ttu-id="8738c-192">A instância de opções tem garantia de passar pela validação, na primeira vez em que for acessada.</span><span class="sxs-lookup"><span data-stu-id="8738c-192">Your options instance is guaranteed to pass validation the first time it's accessed.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="8738c-193">A validação não protege contra modificações de opções, depois que as opções são inicialmente configuradas e validadas.</span><span class="sxs-lookup"><span data-stu-id="8738c-193">Options validation doesn't guard against options modifications after the options are initially configured and validated.</span></span>

<span data-ttu-id="8738c-194">O método `Validate` aceita um `Func<TOptions, bool>`.</span><span class="sxs-lookup"><span data-stu-id="8738c-194">The `Validate` method accepts a `Func<TOptions, bool>`.</span></span> <span data-ttu-id="8738c-195">Para personalizar totalmente a validação, implemente `IValidateOptions<TOptions>`, que permite:</span><span class="sxs-lookup"><span data-stu-id="8738c-195">To fully customize validation, implement `IValidateOptions<TOptions>`, which allows:</span></span>

* <span data-ttu-id="8738c-196">A validação de vários tipos de opções: `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`</span><span class="sxs-lookup"><span data-stu-id="8738c-196">Validation of multiple options types: `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`</span></span>
* <span data-ttu-id="8738c-197">A validação que depende de outro tipo de opção: `public DependsOnAnotherOptionValidator(IOptions<AnotherOption> options)`</span><span class="sxs-lookup"><span data-stu-id="8738c-197">Validation that depends on another option type: `public DependsOnAnotherOptionValidator(IOptions<AnotherOption> options)`</span></span>

<span data-ttu-id="8738c-198">`IValidateOptions` valida:</span><span class="sxs-lookup"><span data-stu-id="8738c-198">`IValidateOptions` validates:</span></span>

* <span data-ttu-id="8738c-199">Uma instância específica de opções nomeadas.</span><span class="sxs-lookup"><span data-stu-id="8738c-199">A specific named options instance.</span></span>
* <span data-ttu-id="8738c-200">Todas as opções quando `name` for `null`.</span><span class="sxs-lookup"><span data-stu-id="8738c-200">All options when `name` is `null`.</span></span>

<span data-ttu-id="8738c-201">Retornar um `ValidateOptionsResult` da implementação da interface:</span><span class="sxs-lookup"><span data-stu-id="8738c-201">Return a `ValidateOptionsResult` from your implementation of the interface:</span></span>

```csharp
public interface IValidateOptions<TOptions> where TOptions : class
{
    ValidateOptionsResult Validate(string name, TOptions options);
}
```

<span data-ttu-id="8738c-202">A validação adiantada (fail fast na inicialização) e a validação de dados baseada em anotação estão agendadas para um lançamento futuro.</span><span class="sxs-lookup"><span data-stu-id="8738c-202">Eager validation (fail fast at startup) and data annotation-based validation are scheduled for a future release.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

## <a name="ipostconfigureoptions"></a><span data-ttu-id="8738c-203">IPostConfigureOptions</span><span class="sxs-lookup"><span data-stu-id="8738c-203">IPostConfigureOptions</span></span>

<span data-ttu-id="8738c-204">Defina a pós-configuração com [IPostConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1).</span><span class="sxs-lookup"><span data-stu-id="8738c-204">Set postconfiguration with [IPostConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1).</span></span> <span data-ttu-id="8738c-205">A pós-configuração é executada depois que ocorre toda a configuração de [IConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1):</span><span class="sxs-lookup"><span data-stu-id="8738c-205">Postconfiguration runs after all [IConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) configuration occurs:</span></span>

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="8738c-206">[PostConfigure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1.postconfigure) está disponível para pós-configurar opções nomeadas:</span><span class="sxs-lookup"><span data-stu-id="8738c-206">[PostConfigure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1.postconfigure) is available to post-configure named options:</span></span>

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="8738c-207">Use [PostConfigureAll&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) para pós-configurar todas as instâncias de configuração nomeadas:</span><span class="sxs-lookup"><span data-stu-id="8738c-207">Use [PostConfigureAll&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) to post-configure all named configuration instances:</span></span>

```csharp
services.PostConfigureAll<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

::: moniker-end

## <a name="options-factory-monitoring-and-cache"></a><span data-ttu-id="8738c-208">Alocador de opções, monitoramento e cache</span><span class="sxs-lookup"><span data-stu-id="8738c-208">Options factory, monitoring, and cache</span></span>

<span data-ttu-id="8738c-209">[IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) é usado para notificações quando instâncias `TOptions` são alteradas.</span><span class="sxs-lookup"><span data-stu-id="8738c-209">[IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) is used for notifications when `TOptions` instances change.</span></span> <span data-ttu-id="8738c-210">`IOptionsMonitor` dá suporte a opções recarregáveis, notificações de alteração e `IPostConfigureOptions`.</span><span class="sxs-lookup"><span data-stu-id="8738c-210">`IOptionsMonitor` supports reloadable options, change notifications, and `IPostConfigureOptions`.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="8738c-211">[IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) é responsável por criar novas instâncias de opção.</span><span class="sxs-lookup"><span data-stu-id="8738c-211">[IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) is responsible for creating new options instances.</span></span> <span data-ttu-id="8738c-212">Ele tem um único método [Create](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1.create).</span><span class="sxs-lookup"><span data-stu-id="8738c-212">It has a single [Create](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1.create) method.</span></span> <span data-ttu-id="8738c-213">A implementação padrão usa todos os `IConfigureOptions` e `IPostConfigureOptions` registrados e executa toda a configuração primeiro, seguido da pós-configuração.</span><span class="sxs-lookup"><span data-stu-id="8738c-213">The default implementation takes all registered `IConfigureOptions` and `IPostConfigureOptions` and runs all the configures first, followed by the post-configures.</span></span> <span data-ttu-id="8738c-214">Ela faz distinção entre `IConfigureNamedOptions` e `IConfigureOptions` e chama apenas a interface apropriada.</span><span class="sxs-lookup"><span data-stu-id="8738c-214">It distinguishes between `IConfigureNamedOptions` and `IConfigureOptions` and only calls the appropriate interface.</span></span>

<span data-ttu-id="8738c-215">[IOptionsMonitorCache&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1) é usado por `IOptionsMonitor` para armazenar instâncias `TOptions` em cache.</span><span class="sxs-lookup"><span data-stu-id="8738c-215">[IOptionsMonitorCache&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1) is used by `IOptionsMonitor` to cache `TOptions` instances.</span></span> <span data-ttu-id="8738c-216">O `IOptionsMonitorCache` invalida as instâncias de opções no monitor, de modo que o valor seja recalculado ([TryRemove](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryremove)).</span><span class="sxs-lookup"><span data-stu-id="8738c-216">The `IOptionsMonitorCache` invalidates options instances in the monitor so that the value is recomputed ([TryRemove](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryremove)).</span></span> <span data-ttu-id="8738c-217">Os valores também podem ser manualmente inseridos com [TryAdd](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryadd).</span><span class="sxs-lookup"><span data-stu-id="8738c-217">Values can be manually introduced as well with [TryAdd](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryadd).</span></span> <span data-ttu-id="8738c-218">O método [Clear](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.clear) é usado quando todas as instâncias nomeadas devem ser recriadas sob demanda.</span><span class="sxs-lookup"><span data-stu-id="8738c-218">The [Clear](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.clear) method is used when all named instances should be recreated on demand.</span></span>

::: moniker-end

## <a name="accessing-options-during-startup"></a><span data-ttu-id="8738c-219">Acessando opções durante a inicialização</span><span class="sxs-lookup"><span data-stu-id="8738c-219">Accessing options during startup</span></span>

<span data-ttu-id="8738c-220">`IOptions` pode ser usado em `Startup.Configure`, pois os serviços são criados antes da execução do método `Configure`.</span><span class="sxs-lookup"><span data-stu-id="8738c-220">`IOptions` can be used in `Startup.Configure`, since services are built before the `Configure` method executes.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IOptions<MyOptions> optionsAccessor)
{
    var option1 = optionsAccessor.Value.Option1;
}
```

<span data-ttu-id="8738c-221">`IOptions` não deve ser usado em `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="8738c-221">`IOptions` shouldn't be used in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="8738c-222">Pode haver um estado inconsistente de opções devido à ordenação dos registros de serviço.</span><span class="sxs-lookup"><span data-stu-id="8738c-222">An inconsistent options state may exist due to the ordering of service registrations.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8738c-223">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="8738c-223">Additional resources</span></span>

* <xref:fundamentals/configuration/index>

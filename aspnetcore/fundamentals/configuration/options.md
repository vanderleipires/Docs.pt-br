---
title: Padrão de opções no ASP.NET Core
author: guardrex
description: Descubra como usar o padrão de opções para representar grupos de configurações relacionadas em aplicativos ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 11/28/2017
uid: fundamentals/configuration/options
ms.openlocfilehash: 96d7d2956fa9bf72706cde0532ee7f4ff753b72c
ms.sourcegitcommit: 2941e24d7f3fd3d5e88d27e5f852aaedd564deda
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37126255"
---
# <a name="options-pattern-in-aspnet-core"></a><span data-ttu-id="2e4d1-103">Padrão de opções no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2e4d1-103">Options pattern in ASP.NET Core</span></span>

<span data-ttu-id="2e4d1-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="2e4d1-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="2e4d1-105">O padrão de opções usa classes para representar grupos de configurações relacionadas.</span><span class="sxs-lookup"><span data-stu-id="2e4d1-105">The options pattern uses classes to represent groups of related settings.</span></span> <span data-ttu-id="2e4d1-106">Quando as definições de configuração são isoladas por recurso em classes separadas, o aplicativo segue dois princípios importantes de engenharia de software:</span><span class="sxs-lookup"><span data-stu-id="2e4d1-106">When configuration settings are isolated by feature into separate classes, the app adheres to two important software engineering principles:</span></span>

* <span data-ttu-id="2e4d1-107">O [ISP (Princípio de Segregação da Interface)](http://deviq.com/interface-segregation-principle/): os recursos (classes) que dependem de definições de configuração dependem apenas das definições de configuração usadas por eles.</span><span class="sxs-lookup"><span data-stu-id="2e4d1-107">The [Interface Segregation Principle (ISP)](http://deviq.com/interface-segregation-principle/): Features (classes) that depend on configuration settings depend only on the configuration settings that they use.</span></span>
* <span data-ttu-id="2e4d1-108">[Separação de Interesses](http://deviq.com/separation-of-concerns/): as configurações para diferentes partes do aplicativo não são dependentes nem acopladas entre si.</span><span class="sxs-lookup"><span data-stu-id="2e4d1-108">[Separation of Concerns](http://deviq.com/separation-of-concerns/): Settings for different parts of the app aren't dependent or coupled to one another.</span></span>

<span data-ttu-id="2e4d1-109">[Exiba ou baixe o código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample)) Este artigo é mais fácil de ser acompanhado com o aplicativo de exemplo.</span><span class="sxs-lookup"><span data-stu-id="2e4d1-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)) This article is easier to follow with the sample app.</span></span>

## <a name="basic-options-configuration"></a><span data-ttu-id="2e4d1-110">Configuração de opções básicas</span><span class="sxs-lookup"><span data-stu-id="2e4d1-110">Basic options configuration</span></span>

<span data-ttu-id="2e4d1-111">A configuração de opções básicas é demonstrada como o Exemplo &num;1 no [aplicativo de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="2e4d1-111">Basic options configuration is demonstrated as Example &num;1 in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="2e4d1-112">Uma classe de opções deve ser não abstrata com um construtor público sem parâmetros.</span><span class="sxs-lookup"><span data-stu-id="2e4d1-112">An options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="2e4d1-113">A classe a seguir, `MyOptions`, tem duas propriedades, `Option1` e `Option2`.</span><span class="sxs-lookup"><span data-stu-id="2e4d1-113">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span></span> <span data-ttu-id="2e4d1-114">A configuração de valores padrão é opcional, mas o construtor de classe no exemplo a seguir define o valor padrão de `Option1`.</span><span class="sxs-lookup"><span data-stu-id="2e4d1-114">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span></span> <span data-ttu-id="2e4d1-115">`Option2` tem um valor padrão definido com a inicialização da propriedade diretamente (*Models/MyOptions.cs*):</span><span class="sxs-lookup"><span data-stu-id="2e4d1-115">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span></span>

[!code-csharp[](options/sample/Models/MyOptions.cs?name=snippet1)]

<span data-ttu-id="2e4d1-116">A classe `MyOptions` é adicionada ao contêiner de serviço com [Configure&lt;TOptions&gt;(IServiceCollection, IConfiguration)](/dotnet/api/microsoft.extensions.dependencyinjection.optionsconfigurationservicecollectionextensions.configure#Microsoft_Extensions_DependencyInjection_OptionsConfigurationServiceCollectionExtensions_Configure__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_Microsoft_Extensions_Configuration_IConfiguration_) e é associada à configuração:</span><span class="sxs-lookup"><span data-stu-id="2e4d1-116">The `MyOptions` class is added to the service container with [Configure&lt;TOptions&gt;(IServiceCollection, IConfiguration)](/dotnet/api/microsoft.extensions.dependencyinjection.optionsconfigurationservicecollectionextensions.configure#Microsoft_Extensions_DependencyInjection_OptionsConfigurationServiceCollectionExtensions_Configure__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_Microsoft_Extensions_Configuration_IConfiguration_) and bound to configuration:</span></span>

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example1)]

<span data-ttu-id="2e4d1-117">O seguinte modelo de página usa a [injeção de dependência de construtor](xref:fundamentals/dependency-injection#what-is-dependency-injection) com [IOptions&lt;TOptions&gt;](/dotnet/api/Microsoft.Extensions.Options.IOptions-1) para acessar as configurações (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="2e4d1-117">The following page model uses [constructor dependency injection](xref:fundamentals/dependency-injection#what-is-dependency-injection) with [IOptions&lt;TOptions&gt;](/dotnet/api/Microsoft.Extensions.Options.IOptions-1) to access the settings (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example1)]

<span data-ttu-id="2e4d1-118">O arquivo *appsettings.json* de exemplo especifica valores para `option1` e `option2`:</span><span class="sxs-lookup"><span data-stu-id="2e4d1-118">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span></span>

[!code-json[](options/sample/appsettings.json?highlight=2-3)]

<span data-ttu-id="2e4d1-119">Quando o aplicativo é executado, o método `OnGet` do modelo de página retorna uma cadeia de caracteres que mostra os valores da classe de opção:</span><span class="sxs-lookup"><span data-stu-id="2e4d1-119">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
option1 = value1_from_json, option2 = -1
```

> [!NOTE]
> <span data-ttu-id="2e4d1-120">Ao usar um [ConfigurationBuilder](/dotnet/api/system.configuration.configurationbuilder) personalizado para carregar opções de configuração de um arquivo de configuração, confirme se o caminho de base está corretamente configurado:</span><span class="sxs-lookup"><span data-stu-id="2e4d1-120">When using a custom [ConfigurationBuilder](/dotnet/api/system.configuration.configurationbuilder) to load options configuration from a settings file, confirm that the base path is set correctly:</span></span>
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
> <span data-ttu-id="2e4d1-121">Não é necessário configurar o caminho de base explicitamente ao carregar opções de configuração do arquivo de configuração por meio do [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span><span class="sxs-lookup"><span data-stu-id="2e4d1-121">Explicitly setting the base path isn't required when loading options configuration from the settings file via [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span></span>

## <a name="configure-simple-options-with-a-delegate"></a><span data-ttu-id="2e4d1-122">Configurar opções simples com um delegado</span><span class="sxs-lookup"><span data-stu-id="2e4d1-122">Configure simple options with a delegate</span></span>

<span data-ttu-id="2e4d1-123">A configuração de opções simples com um delegado é demonstrada como o Exemplo &num;2 no [aplicativo de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="2e4d1-123">Configuring simple options with a delegate is demonstrated as Example &num;2 in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="2e4d1-124">Use um delegado para definir valores de opções.</span><span class="sxs-lookup"><span data-stu-id="2e4d1-124">Use a delegate to set options values.</span></span> <span data-ttu-id="2e4d1-125">O aplicativo de exemplo usa a classe `MyOptionsWithDelegateConfig` (*Models/MyOptionsWithDelegateConfig.cs*):</span><span class="sxs-lookup"><span data-stu-id="2e4d1-125">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span></span>

[!code-csharp[](options/sample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

<span data-ttu-id="2e4d1-126">No código a seguir, um segundo serviço `IConfigureOptions<TOptions>` é adicionado ao contêiner de serviço.</span><span class="sxs-lookup"><span data-stu-id="2e4d1-126">In the following code, a second `IConfigureOptions<TOptions>` service is added to the service container.</span></span> <span data-ttu-id="2e4d1-127">Ele usa um delegado para configurar a associação com `MyOptionsWithDelegateConfig`:</span><span class="sxs-lookup"><span data-stu-id="2e4d1-127">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span></span>

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example2)]

<span data-ttu-id="2e4d1-128">*Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="2e4d1-128">*Index.cshtml.cs*:</span></span>

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example2)]

<span data-ttu-id="2e4d1-129">Adicione vários provedores de configuração.</span><span class="sxs-lookup"><span data-stu-id="2e4d1-129">You can add multiple configuration providers.</span></span> <span data-ttu-id="2e4d1-130">Provedores de configuração estão disponíveis em pacotes NuGet.</span><span class="sxs-lookup"><span data-stu-id="2e4d1-130">Configuration providers are available in NuGet packages.</span></span> <span data-ttu-id="2e4d1-131">Eles são aplicados para que sejam registrados.</span><span class="sxs-lookup"><span data-stu-id="2e4d1-131">They're applied in order that they're registered.</span></span>

<span data-ttu-id="2e4d1-132">Cada chamada a [Configure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1.configure) adiciona um serviço `IConfigureOptions<TOptions>` ao contêiner de serviço.</span><span class="sxs-lookup"><span data-stu-id="2e4d1-132">Each call to [Configure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1.configure) adds an `IConfigureOptions<TOptions>` service to the service container.</span></span> <span data-ttu-id="2e4d1-133">No exemplo anterior, os valores `Option1` e `Option2` são especificados em *appsettings.json*, mas os valores `Option1` e `Option2` são substituídos pelo delegado configurado.</span><span class="sxs-lookup"><span data-stu-id="2e4d1-133">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span></span>

<span data-ttu-id="2e4d1-134">Quando mais de um serviço de configuração é habilitado, a última fonte de configuração especificada *vence* e define o valor de configuração.</span><span class="sxs-lookup"><span data-stu-id="2e4d1-134">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span></span> <span data-ttu-id="2e4d1-135">Quando o aplicativo é executado, o método `OnGet` do modelo de página retorna uma cadeia de caracteres que mostra os valores da classe de opção:</span><span class="sxs-lookup"><span data-stu-id="2e4d1-135">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
delegate_option1 = value1_configured_by_delgate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a><span data-ttu-id="2e4d1-136">Configuração de subopções</span><span class="sxs-lookup"><span data-stu-id="2e4d1-136">Suboptions configuration</span></span>

<span data-ttu-id="2e4d1-137">A configuração de subopções básicas é demonstrada como o Exemplo &num;3 no [aplicativo de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="2e4d1-137">Suboptions configuration is demonstrated as Example &num;3 in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="2e4d1-138">Os aplicativos devem criar classes de opções que pertencem a grupos de recursos específicos (classes) no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="2e4d1-138">Apps should create options classes that pertain to specific feature groups (classes) in the app.</span></span> <span data-ttu-id="2e4d1-139">Partes do aplicativo que exigem valores de configuração devem ter acesso apenas aos valores de configuração usados por elas.</span><span class="sxs-lookup"><span data-stu-id="2e4d1-139">Parts of the app that require configuration values should only have access to the configuration values that they use.</span></span>

<span data-ttu-id="2e4d1-140">Ao associar opções à configuração, cada propriedade no tipo de opções é associada a uma chave de configuração do formato `property[:sub-property:]`.</span><span class="sxs-lookup"><span data-stu-id="2e4d1-140">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="2e4d1-141">Por exemplo, a propriedade `MyOptions.Option1` é associada à chave `Option1`, que é lida da propriedade `option1` em *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="2e4d1-141">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span>

<span data-ttu-id="2e4d1-142">No código a seguir, um terceiro serviço `IConfigureOptions<TOptions>` é adicionado ao contêiner de serviço.</span><span class="sxs-lookup"><span data-stu-id="2e4d1-142">In the following code, a third `IConfigureOptions<TOptions>` service is added to the service container.</span></span> <span data-ttu-id="2e4d1-143">Ele associa `MySubOptions` à seção `subsection` do arquivo *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="2e4d1-143">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example3)]

<span data-ttu-id="2e4d1-144">O método de extensão `GetSection` exige o pacote NuGet [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/).</span><span class="sxs-lookup"><span data-stu-id="2e4d1-144">The `GetSection` extension method requires the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) NuGet package.</span></span> <span data-ttu-id="2e4d1-145">Se o aplicativo usa o [metapacote Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 ou posterior), o pacote é automaticamente incluído.</span><span class="sxs-lookup"><span data-stu-id="2e4d1-145">If the app uses the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later), the package is automatically included.</span></span>

<span data-ttu-id="2e4d1-146">O arquivo *appsettings.json* de exemplo define um membro `subsection` com chaves para `suboption1` e `suboption2`:</span><span class="sxs-lookup"><span data-stu-id="2e4d1-146">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span></span>

[!code-json[](options/sample/appsettings.json?highlight=4-7)]

<span data-ttu-id="2e4d1-147">A classe `MySubOptions` define propriedades, `SubOption1` e `SubOption2`, para armazenar os valores de subopção (*Models/MySubOptions.cs*):</span><span class="sxs-lookup"><span data-stu-id="2e4d1-147">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the sub-option values (*Models/MySubOptions.cs*):</span></span>

[!code-csharp[](options/sample/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="2e4d1-148">O método `OnGet` do modelo de página retorna uma cadeia de caracteres com os valores de subopção (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="2e4d1-148">The page model's `OnGet` method returns a string with the sub-option values (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example3)]

<span data-ttu-id="2e4d1-149">Quando o aplicativo é executado, o método `OnGet` retorna uma cadeia de caracteres que mostra os valores da classe de subopção:</span><span class="sxs-lookup"><span data-stu-id="2e4d1-149">When the app is run, the `OnGet` method returns a string showing the sub-option class values:</span></span>

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-provided-by-a-view-model-or-with-direct-view-injection"></a><span data-ttu-id="2e4d1-150">Opções fornecidas por um modelo de exibição ou com a injeção de exibição direta</span><span class="sxs-lookup"><span data-stu-id="2e4d1-150">Options provided by a view model or with direct view injection</span></span>

<span data-ttu-id="2e4d1-151">As opções fornecidas por um modelo de exibição ou com a injeção de exibição direta são demonstradas como o Exemplo &num;4 no [aplicativo de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="2e4d1-151">Options provided by a view model or with direct view injection is demonstrated as Example &num;4 in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="2e4d1-152">As opções podem ser fornecidas em um modelo de exibição ou pela injeção de `IOptions<TOptions>` diretamente em uma exibição (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="2e4d1-152">Options can be supplied in a view model or by injecting `IOptions<TOptions>` directly into a view (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example4)]

<span data-ttu-id="2e4d1-153">Para a injeção direta, injete `IOptions<MyOptions>` com uma diretiva `@inject`:</span><span class="sxs-lookup"><span data-stu-id="2e4d1-153">For direct injection, inject `IOptions<MyOptions>` with an `@inject` directive:</span></span>

[!code-cshtml[](options/sample/Pages/Index.cshtml?range=1-10&highlight=5)]

<span data-ttu-id="2e4d1-154">Quando o aplicativo é executado, os valores de opções são mostrados na página renderizada:</span><span class="sxs-lookup"><span data-stu-id="2e4d1-154">When the app is run, the option values are shown in the rendered page:</span></span>

![Os valores de opções Option1: value1_from_json e Option2: -1 são carregados do modelo e pela injeção na exibição.](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a><span data-ttu-id="2e4d1-156">Recarregar dados de configuração com IOptionsSnapshot</span><span class="sxs-lookup"><span data-stu-id="2e4d1-156">Reload configuration data with IOptionsSnapshot</span></span>

<span data-ttu-id="2e4d1-157">O recarregamento de dados de configuração com `IOptionsSnapshot` é demonstrado no Exemplo &num;5 no [aplicativo de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="2e4d1-157">Reloading configuration data with `IOptionsSnapshot` is demonstrated in Example &num;5 in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="2e4d1-158">*Exige o ASP.NET Core 1.1 ou posterior.*</span><span class="sxs-lookup"><span data-stu-id="2e4d1-158">*Requires ASP.NET Core 1.1 or later.*</span></span>

<span data-ttu-id="2e4d1-159">[IOptionsSnapshot](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1) dá suporte a opções de recarregamento com sobrecarga mínima de processamento.</span><span class="sxs-lookup"><span data-stu-id="2e4d1-159">[IOptionsSnapshot](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1) supports reloading options with minimal processing overhead.</span></span> <span data-ttu-id="2e4d1-160">No ASP.NET Core 1.1, `IOptionsSnapshot` é um instantâneo de [IOptionsMonitor&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) e é atualizado automaticamente sempre que o monitor dispara alterações com base na alteração da fonte de dados.</span><span class="sxs-lookup"><span data-stu-id="2e4d1-160">In ASP.NET Core 1.1, `IOptionsSnapshot` is a snapshot of [IOptionsMonitor&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) and updates automatically whenever the monitor triggers changes based on the data source changing.</span></span> <span data-ttu-id="2e4d1-161">No ASP.NET Core 2.0 e posterior, as opções são calculadas uma vez por solicitação, quando acessadas e armazenadas em cache durante o tempo de vida da solicitação.</span><span class="sxs-lookup"><span data-stu-id="2e4d1-161">In ASP.NET Core 2.0 and later, options are computed once per request when accessed and cached for the lifetime of the request.</span></span>

<span data-ttu-id="2e4d1-162">O exemplo a seguir demonstra como um novo `IOptionsSnapshot` é criado após a alteração de *appsettings.json* (*Pages/Index.cshtml.cs*).</span><span class="sxs-lookup"><span data-stu-id="2e4d1-162">The following example demonstrates how a new `IOptionsSnapshot` is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span></span> <span data-ttu-id="2e4d1-163">Várias solicitações ao servidor retornam valores de constante fornecidos pelo arquivo *appsettings.json*, até que o arquivo seja alterado e a configuração seja recarregada.</span><span class="sxs-lookup"><span data-stu-id="2e4d1-163">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span></span>

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example5)]

<span data-ttu-id="2e4d1-164">A seguinte imagem mostra is valores `option1` e `option2` iniciais carregados do arquivo *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="2e4d1-164">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span></span>

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

<span data-ttu-id="2e4d1-165">Altere os valores no arquivo *appsettings.json* para `value1_from_json UPDATED` e `200`.</span><span class="sxs-lookup"><span data-stu-id="2e4d1-165">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span></span> <span data-ttu-id="2e4d1-166">Salve o arquivo *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="2e4d1-166">Save the *appsettings.json* file.</span></span> <span data-ttu-id="2e4d1-167">Atualize o navegador para ver se os valores de opções foram atualizados:</span><span class="sxs-lookup"><span data-stu-id="2e4d1-167">Refresh the browser to see that the options values are updated:</span></span>

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a><span data-ttu-id="2e4d1-168">Suporte de opções nomeadas com IConfigureNamedOptions</span><span class="sxs-lookup"><span data-stu-id="2e4d1-168">Named options support with IConfigureNamedOptions</span></span>

<span data-ttu-id="2e4d1-169">O suporte de opções nomeadas com [IConfigureNamedOptions](/dotnet/api/microsoft.extensions.options.iconfigurenamedoptions-1) é demonstrado como o Exemplo &num;6 no [aplicativo de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="2e4d1-169">Named options support with [IConfigureNamedOptions](/dotnet/api/microsoft.extensions.options.iconfigurenamedoptions-1) is demonstrated as Example &num;6 in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="2e4d1-170">*Exige o ASP.NET Core 2.0 ou posterior.*</span><span class="sxs-lookup"><span data-stu-id="2e4d1-170">*Requires ASP.NET Core 2.0 or later.*</span></span>

<span data-ttu-id="2e4d1-171">O suporte de *opções nomeadas* permite que o aplicativo faça a distinção entre as configurações de opções nomeadas.</span><span class="sxs-lookup"><span data-stu-id="2e4d1-171">*Named options* support allows the app to distinguish between named options configurations.</span></span> <span data-ttu-id="2e4d1-172">No aplicativo de exemplo, as opções nomeadas são declaradas com a [OptionsServiceCollectionExtensions.Configure&lt;TOptions&gt;(IServiceCollection, String, Action&lt;TOptions&gt;)](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configure), que, por sua vez, chama o método [ConfigureNamedOptions&lt;TOptions&gt;.Configure](/dotnet/api/microsoft.extensions.options.configurenamedoptions-1.configure) do método de extensão:</span><span class="sxs-lookup"><span data-stu-id="2e4d1-172">In the sample app, named options are declared with the [OptionsServiceCollectionExtensions.Configure&lt;TOptions&gt;(IServiceCollection, String, Action&lt;TOptions&gt;)](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configure) which in turn calls the extension method [ConfigureNamedOptions&lt;TOptions&gt;.Configure](/dotnet/api/microsoft.extensions.options.configurenamedoptions-1.configure) method:</span></span>

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example6)]

<span data-ttu-id="2e4d1-173">O aplicativo de exemplo acessa as opções nomeadas com [IOptionsSnapshot&lt;TOptions&gt;.Get](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1.get) (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="2e4d1-173">The sample app accesses the named options with [IOptionsSnapshot&lt;TOptions&gt;.Get](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1.get) (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example6)]

<span data-ttu-id="2e4d1-174">Executando o aplicativo de exemplo, as opções nomeadas são retornadas:</span><span class="sxs-lookup"><span data-stu-id="2e4d1-174">Running the sample app, the named options are returned:</span></span>

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

<span data-ttu-id="2e4d1-175">Os valores `named_options_1` são fornecidos pela configuração, que são carregados do arquivo *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="2e4d1-175">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span></span> <span data-ttu-id="2e4d1-176">Os valores `named_options_2` são fornecidos pelo:</span><span class="sxs-lookup"><span data-stu-id="2e4d1-176">`named_options_2` values are provided by:</span></span>

* <span data-ttu-id="2e4d1-177">Delegado `named_options_2` em `ConfigureServices` para `Option1`.</span><span class="sxs-lookup"><span data-stu-id="2e4d1-177">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span></span>
* <span data-ttu-id="2e4d1-178">Valor padrão para `Option2` fornecido pela classe `MyOptions`.</span><span class="sxs-lookup"><span data-stu-id="2e4d1-178">The default value for `Option2` provided by the `MyOptions` class.</span></span>

<span data-ttu-id="2e4d1-179">Configure todas as instâncias de opções nomeadas com o método [OptionsServiceCollectionExtensions.ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall).</span><span class="sxs-lookup"><span data-stu-id="2e4d1-179">Configure all named options instances with the [OptionsServiceCollectionExtensions.ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) method.</span></span> <span data-ttu-id="2e4d1-180">O código a seguir configura `Option1` para todas as instâncias de configuração nomeadas com um valor comum.</span><span class="sxs-lookup"><span data-stu-id="2e4d1-180">The following code configures `Option1` for all named configuration instances with a common value.</span></span> <span data-ttu-id="2e4d1-181">Adicione o seguinte código manualmente ao método `Configure`:</span><span class="sxs-lookup"><span data-stu-id="2e4d1-181">Add the following code manually to the `Configure` method:</span></span>

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

<span data-ttu-id="2e4d1-182">A execução do aplicativo de exemplo após a adição do código produz o seguinte resultado:</span><span class="sxs-lookup"><span data-stu-id="2e4d1-182">Running the sample app after adding the code produces the following result:</span></span>

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> <span data-ttu-id="2e4d1-183">No ASP.NET Core 2.0 e posterior, todas as opções são instâncias nomeadas.</span><span class="sxs-lookup"><span data-stu-id="2e4d1-183">In ASP.NET Core 2.0 and later, all options are named instances.</span></span> <span data-ttu-id="2e4d1-184">As instâncias `IConfigureOption` existentes são tratadas como sendo direcionadas à instância `Options.DefaultName`, que é `string.Empty`.</span><span class="sxs-lookup"><span data-stu-id="2e4d1-184">Existing `IConfigureOption` instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span></span> <span data-ttu-id="2e4d1-185">`IConfigureNamedOptions` também implementa `IConfigureOptions`.</span><span class="sxs-lookup"><span data-stu-id="2e4d1-185">`IConfigureNamedOptions` also implements `IConfigureOptions`.</span></span> <span data-ttu-id="2e4d1-186">A implementação padrão de [IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) ([fonte de referência](https://github.com/aspnet/Options/blob/release/2.0/src/Microsoft.Extensions.Options/IOptionsFactory.cs) tem uma lógica para usar cada uma de forma adequada.</span><span class="sxs-lookup"><span data-stu-id="2e4d1-186">The default implementation of the [IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) ([reference source](https://github.com/aspnet/Options/blob/release/2.0/src/Microsoft.Extensions.Options/IOptionsFactory.cs) has logic to use each appropriately.</span></span> <span data-ttu-id="2e4d1-187">A opção nomeada `null` é usada para direcionar todas as instâncias nomeadas, em vez de uma instância nomeada específica ([ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) e [PostConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) usa essa convenção).</span><span class="sxs-lookup"><span data-stu-id="2e4d1-187">The `null` named option is used to target all of the named instances instead of a specific named instance ([ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) and [PostConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) use this convention).</span></span>

## <a name="ipostconfigureoptions"></a><span data-ttu-id="2e4d1-188">IPostConfigureOptions</span><span class="sxs-lookup"><span data-stu-id="2e4d1-188">IPostConfigureOptions</span></span>

<span data-ttu-id="2e4d1-189">*Exige o ASP.NET Core 2.0 ou posterior.*</span><span class="sxs-lookup"><span data-stu-id="2e4d1-189">*Requires ASP.NET Core 2.0 or later.*</span></span>

<span data-ttu-id="2e4d1-190">Defina a pós-configuração com [IPostConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1).</span><span class="sxs-lookup"><span data-stu-id="2e4d1-190">Set postconfiguration with [IPostConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1).</span></span> <span data-ttu-id="2e4d1-191">A pós-configuração é executada depois que ocorre toda a configuração de [IConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1):</span><span class="sxs-lookup"><span data-stu-id="2e4d1-191">Postconfiguration runs after all [IConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) configuration occurs:</span></span>

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="2e4d1-192">[PostConfigure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1.postconfigure) está disponível para pós-configurar opções nomeadas:</span><span class="sxs-lookup"><span data-stu-id="2e4d1-192">[PostConfigure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1.postconfigure) is available to post-configure named options:</span></span>

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="2e4d1-193">Use [PostConfigureAll&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) para pós-configurar todas as instâncias de configuração nomeadas:</span><span class="sxs-lookup"><span data-stu-id="2e4d1-193">Use [PostConfigureAll&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) to post-configure all named configuration instances:</span></span>

```csharp
services.PostConfigureAll<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="options-factory-monitoring-and-cache"></a><span data-ttu-id="2e4d1-194">Alocador de opções, monitoramento e cache</span><span class="sxs-lookup"><span data-stu-id="2e4d1-194">Options factory, monitoring, and cache</span></span>

<span data-ttu-id="2e4d1-195">[IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) é usado para notificações quando instâncias `TOptions` são alteradas.</span><span class="sxs-lookup"><span data-stu-id="2e4d1-195">[IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) is used for notifications when `TOptions` instances change.</span></span> <span data-ttu-id="2e4d1-196">`IOptionsMonitor` dá suporte a opções recarregáveis, notificações de alteração e `IPostConfigureOptions`.</span><span class="sxs-lookup"><span data-stu-id="2e4d1-196">`IOptionsMonitor` supports reloadable options, change notifications, and `IPostConfigureOptions`.</span></span>

<span data-ttu-id="2e4d1-197">[IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) (ASP.NET Core 2.0 ou posterior) é responsável por criar novas instâncias de opções.</span><span class="sxs-lookup"><span data-stu-id="2e4d1-197">[IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) (ASP.NET Core 2.0 or later) is responsible for creating new options instances.</span></span> <span data-ttu-id="2e4d1-198">Ele tem um único método [Create](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1.create).</span><span class="sxs-lookup"><span data-stu-id="2e4d1-198">It has a single [Create](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1.create) method.</span></span> <span data-ttu-id="2e4d1-199">A implementação padrão usa todos os `IConfigureOptions` e `IPostConfigureOptions` registrados e executa toda a configuração primeiro, seguido da pós-configuração.</span><span class="sxs-lookup"><span data-stu-id="2e4d1-199">The default implementation takes all registered `IConfigureOptions` and `IPostConfigureOptions` and runs all the configures first, followed by the post-configures.</span></span> <span data-ttu-id="2e4d1-200">Ela faz distinção entre `IConfigureNamedOptions` e `IConfigureOptions` e chama apenas a interface apropriada.</span><span class="sxs-lookup"><span data-stu-id="2e4d1-200">It distinguishes between `IConfigureNamedOptions` and `IConfigureOptions` and only calls the appropriate interface.</span></span>

<span data-ttu-id="2e4d1-201">[IOptionsMonitorCache&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1) (ASP.NET Core 2.0 ou posterior) é usado por `IOptionsMonitor` para armazenar instâncias `TOptions` em cache.</span><span class="sxs-lookup"><span data-stu-id="2e4d1-201">[IOptionsMonitorCache&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1) (ASP.NET Core 2.0 or later) is used by `IOptionsMonitor` to cache `TOptions` instances.</span></span> <span data-ttu-id="2e4d1-202">O `IOptionsMonitorCache` invalida as instâncias de opções no monitor, de modo que o valor seja recalculado ([TryRemove](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryremove)).</span><span class="sxs-lookup"><span data-stu-id="2e4d1-202">The `IOptionsMonitorCache` invalidates options instances in the monitor so that the value is recomputed ([TryRemove](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryremove)).</span></span> <span data-ttu-id="2e4d1-203">Os valores também podem ser manualmente inseridos com [TryAdd](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryadd).</span><span class="sxs-lookup"><span data-stu-id="2e4d1-203">Values can be manually introduced as well with [TryAdd](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryadd).</span></span> <span data-ttu-id="2e4d1-204">O método [Clear](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.clear) é usado quando todas as instâncias nomeadas devem ser recriadas sob demanda.</span><span class="sxs-lookup"><span data-stu-id="2e4d1-204">The [Clear](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.clear) method is used when all named instances should be recreated on demand.</span></span>

## <a name="accessing-options-during-startup"></a><span data-ttu-id="2e4d1-205">Acessando opções durante a inicialização</span><span class="sxs-lookup"><span data-stu-id="2e4d1-205">Accessing options during startup</span></span>

<span data-ttu-id="2e4d1-206">`IOptions` pode ser usado em `Configure`, pois os serviços são criados antes da execução do método `Configure`.</span><span class="sxs-lookup"><span data-stu-id="2e4d1-206">`IOptions` can be used in `Configure`, since services are built before the `Configure` method executes.</span></span> <span data-ttu-id="2e4d1-207">Se um provedor de serviços for criado em `ConfigureServices` para acessar as opções, ele não conterá nenhuma configuração de opções fornecida após sua criação.</span><span class="sxs-lookup"><span data-stu-id="2e4d1-207">If a service provider is built in `ConfigureServices` to access options, it wouldn't contain any options configurations provided after the service provider is built.</span></span> <span data-ttu-id="2e4d1-208">Portanto, pode haver um estado inconsistente de opções devido à ordenação dos registros de serviço.</span><span class="sxs-lookup"><span data-stu-id="2e4d1-208">Therefore, an inconsistent options state may exist due to the ordering of service registrations.</span></span>

<span data-ttu-id="2e4d1-209">Como as opções geralmente são carregadas da configuração, a configuração pode ser usada na inicialização em `Configure` e `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="2e4d1-209">Since options are typically loaded from configuration, configuration can be used in startup in both `Configure` and `ConfigureServices`.</span></span> <span data-ttu-id="2e4d1-210">Para obter exemplos de como usar a configuração durante a inicialização, consulte o tópico [Inicialização do aplicativo](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="2e4d1-210">For examples of using configuration during startup, see the [Application startup](xref:fundamentals/startup) topic.</span></span>

## <a name="see-also"></a><span data-ttu-id="2e4d1-211">Consulte também</span><span class="sxs-lookup"><span data-stu-id="2e4d1-211">See also</span></span>

* [<span data-ttu-id="2e4d1-212">Configuração</span><span class="sxs-lookup"><span data-stu-id="2e4d1-212">Configuration</span></span>](xref:fundamentals/configuration/index)

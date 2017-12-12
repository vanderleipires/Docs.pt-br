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
ms.openlocfilehash: 7d89416626433bf737b63eda4b17e65b089ae142
ms.sourcegitcommit: 8f42ab93402c1b8044815e1e48d0bb84c81f8b59
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/29/2017
---
# <a name="options-pattern-in-aspnet-core"></a><span data-ttu-id="b1fd3-103">Padrão de opções no núcleo do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="b1fd3-103">Options pattern in ASP.NET Core</span></span>

<span data-ttu-id="b1fd3-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="b1fd3-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="b1fd3-105">O padrão de opções usa as classes de opções para representar grupos de configurações relacionadas.</span><span class="sxs-lookup"><span data-stu-id="b1fd3-105">The options pattern uses options classes to represent groups of related settings.</span></span> <span data-ttu-id="b1fd3-106">Quando as definições de configuração são isoladas pelo recurso em classes de opções separadas, o aplicativo cumpre dois princípios de engenharia de software importantes:</span><span class="sxs-lookup"><span data-stu-id="b1fd3-106">When configuration settings are isolated by feature into separate options classes, the app adheres to two important software engineering principles:</span></span>

* <span data-ttu-id="b1fd3-107">O [princípio de diferenciação de Interface (ISP)](http://deviq.com/interface-segregation-principle/): recursos (classes) que dependem de definições de configuração dependem apenas as definições de configuração que eles usam.</span><span class="sxs-lookup"><span data-stu-id="b1fd3-107">The [Interface Segregation Principle (ISP)](http://deviq.com/interface-segregation-principle/): Features (classes) that depend on configuration settings depend only on the configuration settings that they use.</span></span>
* <span data-ttu-id="b1fd3-108">[Separação de preocupações](http://deviq.com/separation-of-concerns/): as configurações diferentes partes do aplicativo não são dependentes ou acoplado um ao outro.</span><span class="sxs-lookup"><span data-stu-id="b1fd3-108">[Separation of Concerns](http://deviq.com/separation-of-concerns/): Settings for different parts of the app aren't dependent or coupled to one another.</span></span>

<span data-ttu-id="b1fd3-109">[Exibir ou baixar o código de exemplo](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample)) Este artigo é mais fácil de seguir com o aplicativo de exemplo.</span><span class="sxs-lookup"><span data-stu-id="b1fd3-109">[View or download sample code](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)) This article is easier to follow with the sample app.</span></span>

## <a name="basic-options-configuration"></a><span data-ttu-id="b1fd3-110">Configuração de opções básicas</span><span class="sxs-lookup"><span data-stu-id="b1fd3-110">Basic options configuration</span></span>

<span data-ttu-id="b1fd3-111">Configuração de opções básicas é demonstrada como exemplo &num;1 no [aplicativo de exemplo](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="b1fd3-111">Basic options configuration is demonstrated as Example &num;1 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="b1fd3-112">Uma classe de opções deve ser não-abstrato com um construtor público sem parâmetros.</span><span class="sxs-lookup"><span data-stu-id="b1fd3-112">An options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="b1fd3-113">A seguinte classe, `MyOptions`, tem duas propriedades, `Option1` e `Option2`.</span><span class="sxs-lookup"><span data-stu-id="b1fd3-113">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span></span> <span data-ttu-id="b1fd3-114">Definindo valores padrão é opcional, mas o construtor de classe no exemplo a seguir define o valor padrão de `Option1`.</span><span class="sxs-lookup"><span data-stu-id="b1fd3-114">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span></span> <span data-ttu-id="b1fd3-115">`Option2`tem um valor padrão definido por inicializar a propriedade diretamente (*Models/MyOptions.cs*):</span><span class="sxs-lookup"><span data-stu-id="b1fd3-115">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span></span>

[!code-csharp[Main](options/sample/Models/MyOptions.cs?name=snippet1)]

<span data-ttu-id="b1fd3-116">O `MyOptions` classe é adicionada ao contêiner de serviço com [IConfigureOptions&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) e associado à configuração:</span><span class="sxs-lookup"><span data-stu-id="b1fd3-116">The `MyOptions` class is added to the service container with [IConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) and bound to configuration:</span></span>

[!code-csharp[Main](options/sample/Startup.cs?name=snippet_Example1)]

<span data-ttu-id="b1fd3-117">A seguinte página modelo usa [injeção de dependência de construtor](xref:fundamentals/dependency-injection#what-is-dependency-injection) com [IOptions&lt;TOptions&gt; ](/dotnet/api/Microsoft.Extensions.Options.IOptions-1) para acessar as configurações (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="b1fd3-117">The following page model uses [constructor dependency injection](xref:fundamentals/dependency-injection#what-is-dependency-injection) with [IOptions&lt;TOptions&gt;](/dotnet/api/Microsoft.Extensions.Options.IOptions-1) to access the settings (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example1)]

<span data-ttu-id="b1fd3-118">O exemplo *appSettings. JSON* arquivo Especifica valores para `option1` e `option2`:</span><span class="sxs-lookup"><span data-stu-id="b1fd3-118">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span></span>

[!code-json[Main](options/sample/appsettings.json)]

<span data-ttu-id="b1fd3-119">Quando o aplicativo é executado, o modelo de página `OnGet` método retorna uma cadeia de caracteres que mostra os valores de opção de classe:</span><span class="sxs-lookup"><span data-stu-id="b1fd3-119">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
option1 = value1_from_json, option2 = -1
```

## <a name="configure-simple-options-with-a-delegate"></a><span data-ttu-id="b1fd3-120">Configurar opções simples com um delegado</span><span class="sxs-lookup"><span data-stu-id="b1fd3-120">Configure simple options with a delegate</span></span>

<span data-ttu-id="b1fd3-121">Configurando opções simples com um delegado é demonstrado como exemplo &num;2 no [aplicativo de exemplo](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="b1fd3-121">Configuring simple options with a delegate is demonstrated as Example &num;2 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="b1fd3-122">Use um delegado para definir valores de opções.</span><span class="sxs-lookup"><span data-stu-id="b1fd3-122">Use a delegate to set options values.</span></span> <span data-ttu-id="b1fd3-123">O aplicativo de exemplo usa o `MyOptionsWithDelegateConfig` classe (*Models/MyOptionsWithDelegateConfig.cs*):</span><span class="sxs-lookup"><span data-stu-id="b1fd3-123">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span></span>

[!code-csharp[Main](options/sample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

<span data-ttu-id="b1fd3-124">No código a seguir, um segundo `IConfigureOptions<TOptions>` serviço é adicionado ao contêiner de serviço.</span><span class="sxs-lookup"><span data-stu-id="b1fd3-124">In the following code, a second `IConfigureOptions<TOptions>` service is added to the service container.</span></span> <span data-ttu-id="b1fd3-125">Ele usa um delegado para configurar a associação com `MyOptionsWithDelegateConfig`:</span><span class="sxs-lookup"><span data-stu-id="b1fd3-125">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span></span>

[!code-csharp[Main](options/sample/Startup.cs?name=snippet_Example2)]

<span data-ttu-id="b1fd3-126">*Index.cshtml.CS*:</span><span class="sxs-lookup"><span data-stu-id="b1fd3-126">*Index.cshtml.cs*:</span></span>

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example2)]

<span data-ttu-id="b1fd3-127">Você pode adicionar vários provedores de configuração.</span><span class="sxs-lookup"><span data-stu-id="b1fd3-127">You can add multiple configuration providers.</span></span> <span data-ttu-id="b1fd3-128">Provedores de configuração estão disponíveis em pacotes do NuGet.</span><span class="sxs-lookup"><span data-stu-id="b1fd3-128">Configuration providers are available in NuGet packages.</span></span> <span data-ttu-id="b1fd3-129">São aplicados para que eles são registrados.</span><span class="sxs-lookup"><span data-stu-id="b1fd3-129">They're applied in order that they're registered.</span></span>

<span data-ttu-id="b1fd3-130">Cada chamada para [configurar&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1.configure) adiciona um `IConfigureOptions<TOptions>` serviço ao contêiner de serviço.</span><span class="sxs-lookup"><span data-stu-id="b1fd3-130">Each call to [Configure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1.configure) adds an `IConfigureOptions<TOptions>` service to the service container.</span></span> <span data-ttu-id="b1fd3-131">No exemplo anterior, os valores de `Option1` e `Option2` estão especificados na *appSettings. JSON*, mas os valores de `Option1` e `Option2` são substituídos pelo delegado configurado.</span><span class="sxs-lookup"><span data-stu-id="b1fd3-131">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span></span>

<span data-ttu-id="b1fd3-132">Quando mais de um serviço de configuração é habilitado, a última fonte de configuração especificada *wins* e define o valor de configuração.</span><span class="sxs-lookup"><span data-stu-id="b1fd3-132">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span></span> <span data-ttu-id="b1fd3-133">Quando o aplicativo é executado, o modelo de página `OnGet` método retorna uma cadeia de caracteres que mostra os valores de opção de classe:</span><span class="sxs-lookup"><span data-stu-id="b1fd3-133">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
delegate_option1 = value1_configured_by_delgate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a><span data-ttu-id="b1fd3-134">Configuração de subopções</span><span class="sxs-lookup"><span data-stu-id="b1fd3-134">Suboptions configuration</span></span>

<span data-ttu-id="b1fd3-135">Configuração de subopções é demonstrada como exemplo &num;3 no [aplicativo de exemplo](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="b1fd3-135">Suboptions configuration is demonstrated as Example &num;3 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="b1fd3-136">Aplicativos devem criar classes de opções que pertencem aos grupos de recurso específico (classes) no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="b1fd3-136">Apps should create options classes that pertain to specific feature groups (classes) in the app.</span></span> <span data-ttu-id="b1fd3-137">Partes do aplicativo que exigem valores de configuração só devem ter acesso para os valores de configuração que eles usam.</span><span class="sxs-lookup"><span data-stu-id="b1fd3-137">Parts of the app that require configuration values should only have access to the configuration values that they use.</span></span>

<span data-ttu-id="b1fd3-138">Ao associar opções de configuração, cada propriedade no tipo de opções está associada a uma chave de configuração do formulário `property[:sub-property:]`.</span><span class="sxs-lookup"><span data-stu-id="b1fd3-138">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="b1fd3-139">Por exemplo, o `MyOptions.Option1` propriedade está associada à chave `Option1`, que são lidos a partir de `option1` propriedade em *appSettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="b1fd3-139">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span>

<span data-ttu-id="b1fd3-140">No código a seguir, um terceiro `IConfigureOptions<TOptions>` serviço é adicionado ao contêiner de serviço.</span><span class="sxs-lookup"><span data-stu-id="b1fd3-140">In the following code, a third `IConfigureOptions<TOptions>` service is added to the service container.</span></span> <span data-ttu-id="b1fd3-141">Ele é ligado `MySubOptions` a seção `subsection` do *appSettings. JSON* arquivo:</span><span class="sxs-lookup"><span data-stu-id="b1fd3-141">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[Main](options/sample/Startup.cs?name=snippet_Example3)]

<span data-ttu-id="b1fd3-142">O `GetSection` requer o método de extensão de [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) pacote NuGet.</span><span class="sxs-lookup"><span data-stu-id="b1fd3-142">The `GetSection` extension method requires the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) NuGet package.</span></span> <span data-ttu-id="b1fd3-143">Se o aplicativo usa o [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) metapackage, o pacote é incluído automaticamente.</span><span class="sxs-lookup"><span data-stu-id="b1fd3-143">If the app uses the [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) metapackage, the package is automatically included.</span></span>

<span data-ttu-id="b1fd3-144">O exemplo *appSettings. JSON* arquivo define uma `subsection` membro com chaves para `suboption1` e `suboption2`:</span><span class="sxs-lookup"><span data-stu-id="b1fd3-144">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span></span>

[!code-json[Main](options/sample/appsettings.json?highlight=4-7)]

<span data-ttu-id="b1fd3-145">O `MySubOptions` classe define propriedades, `SubOption1` e `SubOption2`, para manter os valores de opção sub (*Models/MySubOptions.cs*):</span><span class="sxs-lookup"><span data-stu-id="b1fd3-145">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the sub-option values (*Models/MySubOptions.cs*):</span></span>

[!code-csharp[Main](options/sample/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="b1fd3-146">O modelo de página `OnGet` método retorna uma cadeia de caracteres com os valores de opção sub (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="b1fd3-146">The page model's `OnGet` method returns a string with the sub-option values (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example3)]

<span data-ttu-id="b1fd3-147">Quando o aplicativo é executado, o `OnGet` método retorna uma cadeia de caracteres com a opção sub valores de classe:</span><span class="sxs-lookup"><span data-stu-id="b1fd3-147">When the app is run, the `OnGet` method returns a string showing the sub-option class values:</span></span>

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-provided-by-a-view-model-or-with-direct-view-injection"></a><span data-ttu-id="b1fd3-148">Opções fornecidas por um modelo de exibição ou com a injeção de modo direto</span><span class="sxs-lookup"><span data-stu-id="b1fd3-148">Options provided by a view model or with direct view injection</span></span>

<span data-ttu-id="b1fd3-149">Opções fornecidas por um modelo de exibição ou com a injeção de modo direto é demonstrado como exemplo &num;4 a [aplicativo de exemplo](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="b1fd3-149">Options provided by a view model or with direct view injection is demonstrated as Example &num;4 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="b1fd3-150">Opções podem ser fornecidas em um modelo de exibição ou injetando `IOptions<TOptions>` diretamente em um modo de exibição (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="b1fd3-150">Options can be supplied in a view model or by injecting `IOptions<TOptions>` directly into a view (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example4)]

<span data-ttu-id="b1fd3-151">Para a injeção do direct injetar `IOptions<MyOptions>` com um `@inject` diretiva:</span><span class="sxs-lookup"><span data-stu-id="b1fd3-151">For direct injection, inject `IOptions<MyOptions>` with an `@inject` directive:</span></span>

[!code-cshtml[Main](options/sample/Pages/Index.cshtml?range=1-10&highlight=5)]

<span data-ttu-id="b1fd3-152">Quando o aplicativo é executado, os valores de opção são mostrados na página renderizada:</span><span class="sxs-lookup"><span data-stu-id="b1fd3-152">When the app is run, the option values are shown in the rendered page:</span></span>

![Opções de valores de opção 1: value1_from_json e Option2: -1 são carregados do modelo e pela inclusão no modo de exibição.](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a><span data-ttu-id="b1fd3-154">Recarregue os dados de configuração com IOptionsSnapshot</span><span class="sxs-lookup"><span data-stu-id="b1fd3-154">Reload configuration data with IOptionsSnapshot</span></span>

<span data-ttu-id="b1fd3-155">Recarregar os dados de configuração com `IOptionsSnapshot` é demonstrado no exemplo &num;5 a [aplicativo de exemplo](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="b1fd3-155">Reloading configuration data with `IOptionsSnapshot` is demonstrated in Example &num;5 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="b1fd3-156">*Exige o ASP.NET Core 1.1 ou posterior.*</span><span class="sxs-lookup"><span data-stu-id="b1fd3-156">*Requires ASP.NET Core 1.1 or later.*</span></span>

<span data-ttu-id="b1fd3-157">[IOptionsSnapshot](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1) oferece suporte a opções com sobrecarga mínima de processamento de recarregar.</span><span class="sxs-lookup"><span data-stu-id="b1fd3-157">[IOptionsSnapshot](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1) supports reloading options with minimal processing overhead.</span></span> <span data-ttu-id="b1fd3-158">No ASP.NET Core 1.1, `IOptionsSnapshot` é um instantâneo de [IOptionsMonitor&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) e atualizações automaticamente sempre que o monitor aciona a alterações de acordo com a alteração de fonte de dados.</span><span class="sxs-lookup"><span data-stu-id="b1fd3-158">In ASP.NET Core 1.1, `IOptionsSnapshot` is a snapshot of [IOptionsMonitor&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) and updates automatically whenever the monitor triggers changes based on the data source changing.</span></span> <span data-ttu-id="b1fd3-159">No ASP.NET Core 2.0 e posterior, opções são calculadas uma vez por solicitação quando acessados e armazenados em cache para o tempo de vida da solicitação.</span><span class="sxs-lookup"><span data-stu-id="b1fd3-159">In ASP.NET Core 2.0 and later, options are computed once per request when accessed and cached for the lifetime of the request.</span></span>

<span data-ttu-id="b1fd3-160">O exemplo a seguir demonstra como um novo `IOptionsSnapshot` é criada após *appSettings. JSON* alterações (*Pages/Index.cshtml.cs*).</span><span class="sxs-lookup"><span data-stu-id="b1fd3-160">The following example demonstrates how a new `IOptionsSnapshot` is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span></span> <span data-ttu-id="b1fd3-161">Várias solicitações ao servidor retornam valores de constantes fornecidos pelo *appSettings. JSON* até que o arquivo é alterado e recargas de configuração de arquivo.</span><span class="sxs-lookup"><span data-stu-id="b1fd3-161">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span></span>

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example5)]

<span data-ttu-id="b1fd3-162">A imagem a seguir mostra a inicial `option1` e `option2` valores carregados a partir de *appSettings. JSON* arquivo:</span><span class="sxs-lookup"><span data-stu-id="b1fd3-162">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span></span>

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

<span data-ttu-id="b1fd3-163">Alterar os valores de *appSettings. JSON* o arquivo para `value1_from_json UPDATED` e `200`.</span><span class="sxs-lookup"><span data-stu-id="b1fd3-163">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span></span> <span data-ttu-id="b1fd3-164">Salve o *appSettings. JSON* arquivo.</span><span class="sxs-lookup"><span data-stu-id="b1fd3-164">Save the *appsettings.json* file.</span></span> <span data-ttu-id="b1fd3-165">Atualize o navegador para ver se os valores de opções são atualizados:</span><span class="sxs-lookup"><span data-stu-id="b1fd3-165">Refresh the browser to see that the options values are updated:</span></span>

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a><span data-ttu-id="b1fd3-166">Opções de suporte com IConfigureNamedOptions de chamada</span><span class="sxs-lookup"><span data-stu-id="b1fd3-166">Named options support with IConfigureNamedOptions</span></span>

<span data-ttu-id="b1fd3-167">Chamado opções de suporte com [IConfigureNamedOptions](/dotnet/api/microsoft.extensions.options.iconfigurenamedoptions-1) é demonstrado como exemplo &num;6 a [aplicativo de exemplo](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="b1fd3-167">Named options support with [IConfigureNamedOptions](/dotnet/api/microsoft.extensions.options.iconfigurenamedoptions-1) is demonstrated as Example &num;6 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="b1fd3-168">*Exige o ASP.NET Core 2.0 ou posterior.*</span><span class="sxs-lookup"><span data-stu-id="b1fd3-168">*Requires ASP.NET Core 2.0 or later.*</span></span>

<span data-ttu-id="b1fd3-169">*Opções de chamada* suporte permite que o aplicativo distinguir entre as configurações de opções nomeada.</span><span class="sxs-lookup"><span data-stu-id="b1fd3-169">*Named options* support allows the app to distinguish between named options configurations.</span></span> <span data-ttu-id="b1fd3-170">No aplicativo de exemplo, as opções nomeadas são declaradas com a [ConfigureNamedOptions&lt;TOptions&gt;. Configurar](/dotnet/api/microsoft.extensions.options.configurenamedoptions-1.configure) método:</span><span class="sxs-lookup"><span data-stu-id="b1fd3-170">In the sample app, named options are declared with the [ConfigureNamedOptions&lt;TOptions&gt;.Configure](/dotnet/api/microsoft.extensions.options.configurenamedoptions-1.configure) method:</span></span>

[!code-csharp[Main](options/sample/Startup.cs?name=snippet_Example6)]

<span data-ttu-id="b1fd3-171">O aplicativo de exemplo acessa as opções nomeadas com [IOptionsSnapshot&lt;TOptions&gt;. Obter](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1.get) (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="b1fd3-171">The sample app accesses the named options with [IOptionsSnapshot&lt;TOptions&gt;.Get](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1.get) (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example6)]

<span data-ttu-id="b1fd3-172">Executando o aplicativo de exemplo, as opções nomeadas são retornadas:</span><span class="sxs-lookup"><span data-stu-id="b1fd3-172">Running the sample app, the named options are returned:</span></span>

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

<span data-ttu-id="b1fd3-173">`named_options_1`os valores são fornecidos de configuração, que é carregado a partir de *appSettings. JSON* arquivo.</span><span class="sxs-lookup"><span data-stu-id="b1fd3-173">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span></span> <span data-ttu-id="b1fd3-174">`named_options_2`os valores são fornecidos por:</span><span class="sxs-lookup"><span data-stu-id="b1fd3-174">`named_options_2` values are provided by:</span></span>

* <span data-ttu-id="b1fd3-175">O `named_options_2` delegar em `ConfigureServices` para `Option1`.</span><span class="sxs-lookup"><span data-stu-id="b1fd3-175">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span></span>
* <span data-ttu-id="b1fd3-176">O valor padrão para `Option2` fornecidos pelo `MyOptions` classe.</span><span class="sxs-lookup"><span data-stu-id="b1fd3-176">The default value for `Option2` provided by the `MyOptions` class.</span></span>

<span data-ttu-id="b1fd3-177">Configure todas as instâncias nomeadas opções com a [OptionsServiceCollectionExtensions.ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) método.</span><span class="sxs-lookup"><span data-stu-id="b1fd3-177">Configure all named options instances with the [OptionsServiceCollectionExtensions.ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) method.</span></span> <span data-ttu-id="b1fd3-178">O código a seguir configura `Option1` para todas as instâncias de configuração com um valor comum nomeadas.</span><span class="sxs-lookup"><span data-stu-id="b1fd3-178">The following code configures `Option1` for all named configuration instances with a common value.</span></span> <span data-ttu-id="b1fd3-179">Adicione o seguinte código manualmente para o `Configure` método:</span><span class="sxs-lookup"><span data-stu-id="b1fd3-179">Add the following code manually to the `Configure` method:</span></span>

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

<span data-ttu-id="b1fd3-180">Executando o aplicativo de exemplo depois de adicionar o código produz o seguinte resultado:</span><span class="sxs-lookup"><span data-stu-id="b1fd3-180">Running the sample app after adding the code produces the following result:</span></span>

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> <span data-ttu-id="b1fd3-181">No ASP.NET Core 2.0 e posterior, todas as opções são instâncias nomeadas.</span><span class="sxs-lookup"><span data-stu-id="b1fd3-181">In ASP.NET Core 2.0 and later, all options are named instances.</span></span> <span data-ttu-id="b1fd3-182">Existente `IConfigureOption` instâncias são tratadas como direcionamento o `Options.DefaultName` instância, que é `string.Empty`.</span><span class="sxs-lookup"><span data-stu-id="b1fd3-182">Existing `IConfigureOption` instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span></span> <span data-ttu-id="b1fd3-183">`IConfigureNamedOptions`também implementa `IConfigureOptions`.</span><span class="sxs-lookup"><span data-stu-id="b1fd3-183">`IConfigureNamedOptions` also implements `IConfigureOptions`.</span></span> <span data-ttu-id="b1fd3-184">A implementação padrão da [IOptionsFactory&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) ([fonte de referência](https://github.com/aspnet/Options/blob/release/2.0.0/src/Microsoft.Extensions.Options/OptionsFactory.cs)) tem lógica para usar cada adequadamente.</span><span class="sxs-lookup"><span data-stu-id="b1fd3-184">The default implementation of the [IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) ([reference source](https://github.com/aspnet/Options/blob/release/2.0.0/src/Microsoft.Extensions.Options/OptionsFactory.cs)) has logic to use each appropriately.</span></span> <span data-ttu-id="b1fd3-185">O `null` opção nomeada é usada para direcionar todas as instâncias nomeadas, em vez de uma determinada instância nomeada ([ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) e [PostConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) usa essa convenção).</span><span class="sxs-lookup"><span data-stu-id="b1fd3-185">The `null` named option is used to target all of the named instances instead of a specific named instance ([ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) and [PostConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) use this convention).</span></span>

## <a name="ipostconfigureoptions"></a><span data-ttu-id="b1fd3-186">IPostConfigureOptions</span><span class="sxs-lookup"><span data-stu-id="b1fd3-186">IPostConfigureOptions</span></span>

<span data-ttu-id="b1fd3-187">*Exige o ASP.NET Core 2.0 ou posterior.*</span><span class="sxs-lookup"><span data-stu-id="b1fd3-187">*Requires ASP.NET Core 2.0 or later.*</span></span>

<span data-ttu-id="b1fd3-188">Definir postconfiguration com [IPostConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1).</span><span class="sxs-lookup"><span data-stu-id="b1fd3-188">Set postconfiguration with [IPostConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1).</span></span> <span data-ttu-id="b1fd3-189">Postconfiguration é executado depois que todos os [IConfigureOptions&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) configuração ocorre:</span><span class="sxs-lookup"><span data-stu-id="b1fd3-189">Postconfiguration runs after all [IConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) configuration occurs:</span></span>

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="b1fd3-190">[PostConfigure&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1.postconfigure) está disponível para pós-configurar opções nomeadas:</span><span class="sxs-lookup"><span data-stu-id="b1fd3-190">[PostConfigure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1.postconfigure) is available to post-configure named options:</span></span>

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="b1fd3-191">Use [PostConfigureAll&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) após configurar todos denominado instâncias de configuração:</span><span class="sxs-lookup"><span data-stu-id="b1fd3-191">Use [PostConfigureAll&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) to post-configure all named configuration instances:</span></span>

```csharp
services.PostConfigureAll<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="options-factory-monitoring-and-cache"></a><span data-ttu-id="b1fd3-192">Fábrica de opções, o monitoramento e o cache</span><span class="sxs-lookup"><span data-stu-id="b1fd3-192">Options factory, monitoring, and cache</span></span>

<span data-ttu-id="b1fd3-193">[IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) é usado para notificações quando `TOptions` instâncias de alteração.</span><span class="sxs-lookup"><span data-stu-id="b1fd3-193">[IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) is used for notifications when `TOptions` instances change.</span></span> <span data-ttu-id="b1fd3-194">`IOptionsMonitor`oferece suporte a opções de reloadable, alterar notificações, e `IPostConfigureOptions`.</span><span class="sxs-lookup"><span data-stu-id="b1fd3-194">`IOptionsMonitor` supports reloadable options, change notifications, and `IPostConfigureOptions`.</span></span>

<span data-ttu-id="b1fd3-195">[IOptionsFactory&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) (núcleo ASP.NET 2.0 ou posterior) é responsável por criar novas instâncias de opções.</span><span class="sxs-lookup"><span data-stu-id="b1fd3-195">[IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) (ASP.NET Core 2.0 or later) is responsible for creating new options instances.</span></span> <span data-ttu-id="b1fd3-196">Ele tem um único [criar](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1.create) método.</span><span class="sxs-lookup"><span data-stu-id="b1fd3-196">It has a single [Create](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1.create) method.</span></span> <span data-ttu-id="b1fd3-197">A implementação padrão usa todos os `IConfigureOptions` e `IPostConfigureOptions` e executa todos os o configura o primeiro, seguido de pós-configura.</span><span class="sxs-lookup"><span data-stu-id="b1fd3-197">The default implementation takes all registered `IConfigureOptions` and `IPostConfigureOptions` and runs all the configures first, followed by the post-configures.</span></span> <span data-ttu-id="b1fd3-198">Ele faz distinção entre `IConfigureNamedOptions` e `IConfigureOptions` e só chama a interface apropriada.</span><span class="sxs-lookup"><span data-stu-id="b1fd3-198">It distinguishes between `IConfigureNamedOptions` and `IConfigureOptions` and only calls the appropriate interface.</span></span>

<span data-ttu-id="b1fd3-199">[IOptionsMonitorCache&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1) (núcleo ASP.NET 2.0 ou posterior) é usado por `IOptionsMonitor` cache `TOptions` instâncias.</span><span class="sxs-lookup"><span data-stu-id="b1fd3-199">[IOptionsMonitorCache&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1) (ASP.NET Core 2.0 or later) is used by `IOptionsMonitor` to cache `TOptions` instances.</span></span> <span data-ttu-id="b1fd3-200">O `IOptionsMonitorCache` invalida as instâncias de opções no monitor de forma que o valor é recalculado ([TryRemove](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryremove)).</span><span class="sxs-lookup"><span data-stu-id="b1fd3-200">The `IOptionsMonitorCache` invalidates options instances in the monitor so that the value is recomputed ([TryRemove](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryremove)).</span></span> <span data-ttu-id="b1fd3-201">Os valores podem ser manualmente introduzidos bem com [TryAdd](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryadd).</span><span class="sxs-lookup"><span data-stu-id="b1fd3-201">Values can be manually introduced as well with [TryAdd](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryadd).</span></span> <span data-ttu-id="b1fd3-202">O [limpar](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.clear) método é usado quando todas as instâncias nomeadas devem ser recriadas sob demanda.</span><span class="sxs-lookup"><span data-stu-id="b1fd3-202">The [Clear](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.clear) method is used when all named instances should be recreated on demand.</span></span>

## <a name="see-also"></a><span data-ttu-id="b1fd3-203">Consulte também</span><span class="sxs-lookup"><span data-stu-id="b1fd3-203">See also</span></span>

* [<span data-ttu-id="b1fd3-204">Configuração</span><span class="sxs-lookup"><span data-stu-id="b1fd3-204">Configuration</span></span>](xref:fundamentals/configuration/index)

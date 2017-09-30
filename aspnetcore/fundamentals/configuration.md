---
title: "Configuração no ASP.NET Core"
author: rick-anderson
description: "Saiba como usar a API de configuração para configurar um aplicativo ASP.NET Core de várias fontes."
keywords: "ASP.NET Core, configuração, JSON, configuração"
ms.author: riande
manager: wpickett
ms.date: 6/24/2017
ms.topic: article
ms.assetid: b3a5984d-e172-42eb-8a48-547e4acb6806
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/configuration
ms.openlocfilehash: 379030df4ca91a38fce251aeaab9c5dfaf11e915
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/28/2017
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="96e27-104">Configuração no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="96e27-104">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="96e27-105">[Rick Anderson](https://twitter.com/RickAndMSFT), [marca Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), [Daniel Roth](https://github.com/danroth27), e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="96e27-105">[Rick Anderson](https://twitter.com/RickAndMSFT), [Mark Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), [Daniel Roth](https://github.com/danroth27), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="96e27-106">A API de configuração fornece uma maneira de configurar um aplicativo baseado em uma lista de pares nome-valor.</span><span class="sxs-lookup"><span data-stu-id="96e27-106">The Configuration API provides a way of configuring an app based on a list of name-value pairs.</span></span> <span data-ttu-id="96e27-107">Configuração é lida em tempo de execução de várias fontes.</span><span class="sxs-lookup"><span data-stu-id="96e27-107">Configuration is read at runtime from multiple sources.</span></span> <span data-ttu-id="96e27-108">Os pares nome-valor podem ser agrupados em uma hierarquia de vários níveis.</span><span class="sxs-lookup"><span data-stu-id="96e27-108">The name-value pairs can be grouped into a multi-level hierarchy.</span></span> <span data-ttu-id="96e27-109">Há provedores de configuração para:</span><span class="sxs-lookup"><span data-stu-id="96e27-109">There are configuration providers for:</span></span>

* <span data-ttu-id="96e27-110">Formatos de arquivo (INI, JSON e XML)</span><span class="sxs-lookup"><span data-stu-id="96e27-110">File formats (INI, JSON, and XML)</span></span>
* <span data-ttu-id="96e27-111">Argumentos de linha de comando</span><span class="sxs-lookup"><span data-stu-id="96e27-111">Command-line arguments</span></span>
* <span data-ttu-id="96e27-112">Variáveis de ambiente</span><span class="sxs-lookup"><span data-stu-id="96e27-112">Environment variables</span></span>
* <span data-ttu-id="96e27-113">Objetos do .NET na memória</span><span class="sxs-lookup"><span data-stu-id="96e27-113">In-memory .NET objects</span></span>
* <span data-ttu-id="96e27-114">Um repositório de usuário criptografado</span><span class="sxs-lookup"><span data-stu-id="96e27-114">An encrypted user store</span></span>
* [<span data-ttu-id="96e27-115">Cofre de chaves do Azure</span><span class="sxs-lookup"><span data-stu-id="96e27-115">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
* <span data-ttu-id="96e27-116">Provedores personalizados, que você instala ou criar</span><span class="sxs-lookup"><span data-stu-id="96e27-116">Custom providers, which you install or create</span></span>

<span data-ttu-id="96e27-117">Cada valor de configuração é mapeado para uma chave de cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="96e27-117">Each configuration value maps to a string key.</span></span> <span data-ttu-id="96e27-118">Não há suporte de ligação interna ao desserializar as configurações em um personalizado [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) objeto (uma classe .NET simple com propriedades).</span><span class="sxs-lookup"><span data-stu-id="96e27-118">There's built-in binding support to deserialize settings into a custom [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) object (a simple .NET class with properties).</span></span>

[<span data-ttu-id="96e27-119">Exibir ou baixar o código de exemplo</span><span class="sxs-lookup"><span data-stu-id="96e27-119">View or download sample code</span></span>](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/sample)

## <a name="simple-configuration"></a><span data-ttu-id="96e27-120">Configuração simples</span><span class="sxs-lookup"><span data-stu-id="96e27-120">Simple configuration</span></span>

<span data-ttu-id="96e27-121">O aplicativo de console a seguir usa o provedor de configuração JSON:</span><span class="sxs-lookup"><span data-stu-id="96e27-121">The following console app uses the JSON configuration provider:</span></span>

[!code-csharp[Main](configuration/sample/ConfigJson/Program.cs)]

<span data-ttu-id="96e27-122">O aplicativo lê e exibe as seguintes configurações:</span><span class="sxs-lookup"><span data-stu-id="96e27-122">The app reads and displays the following configuration settings:</span></span>

[!code-json[Main](configuration/sample/ConfigJson/appsettings.json)]

<span data-ttu-id="96e27-123">Configuração consiste em uma lista hierárquica de pares nome-valor no qual os nós são separados por dois-pontos.</span><span class="sxs-lookup"><span data-stu-id="96e27-123">Configuration consists of a hierarchical list of name-value pairs in which the nodes are separated by a colon.</span></span> <span data-ttu-id="96e27-124">Para recuperar um valor, acessar o `Configuration` indexador com a chave do item correspondente:</span><span class="sxs-lookup"><span data-stu-id="96e27-124">To retrieve a value, access the `Configuration` indexer with the corresponding item's key:</span></span>

```csharp
Console.WriteLine($"option1 = {Configuration["subsection:suboption1"]}");
```

<span data-ttu-id="96e27-125">Para trabalhar com matrizes em fontes de configuração formatados em JSON, use um índice de matriz como parte da cadeia de caracteres separada por dois-pontos.</span><span class="sxs-lookup"><span data-stu-id="96e27-125">To work with arrays in JSON-formatted configuration sources, use a array index as part of the colon-separated string.</span></span> <span data-ttu-id="96e27-126">O exemplo a seguir obtém o nome do primeiro item na anterior `wizards` matriz:</span><span class="sxs-lookup"><span data-stu-id="96e27-126">The following example gets the name of the first item in the preceding `wizards` array:</span></span>

```csharp
Console.Write($"{Configuration["wizards:0:Name"]}, ");
```

<span data-ttu-id="96e27-127">Pares de nome-valor gravados interna em `Configuration` provedores são **não** persistentes, no entanto, você pode criar um provedor personalizado que salva valores.</span><span class="sxs-lookup"><span data-stu-id="96e27-127">Name-value pairs written to the built in `Configuration` providers are **not** persisted, however, you can create a custom provider that saves values.</span></span> <span data-ttu-id="96e27-128">Consulte [provedor de configuração personalizado](xref:fundamentals/configuration#custom-config-providers).</span><span class="sxs-lookup"><span data-stu-id="96e27-128">See [custom configuration provider](xref:fundamentals/configuration#custom-config-providers).</span></span>

<span data-ttu-id="96e27-129">O exemplo anterior usa o indexador de configuração para ler valores.</span><span class="sxs-lookup"><span data-stu-id="96e27-129">The preceding sample uses the configuration indexer to read values.</span></span> <span data-ttu-id="96e27-130">A configuração de acesso fora de `Startup`, use o [padrão de opções](xref:fundamentals/configuration#options-config-objects).</span><span class="sxs-lookup"><span data-stu-id="96e27-130">To access configuration outside of `Startup`, use the [options pattern](xref:fundamentals/configuration#options-config-objects).</span></span> <span data-ttu-id="96e27-131">O *padrão de opções* mostrado mais adiante neste artigo.</span><span class="sxs-lookup"><span data-stu-id="96e27-131">The *options pattern* is shown later in this article.</span></span>

<span data-ttu-id="96e27-132">É comum ter configurações diferentes para ambientes diferentes, por exemplo, desenvolvimento, teste e produção.</span><span class="sxs-lookup"><span data-stu-id="96e27-132">It's typical to have different configuration settings for different environments, for example, development, test, and production.</span></span> <span data-ttu-id="96e27-133">O `CreateDefaultBuilder` método de extensão em um aplicativo do ASP.NET Core 2. x (ou usando `AddJsonFile` e `AddEnvironmentVariables` diretamente em um aplicativo do ASP.NET Core 1. x) adiciona provedores de configuração para ler arquivos JSON e sistema de fontes de configuração:</span><span class="sxs-lookup"><span data-stu-id="96e27-133">The `CreateDefaultBuilder` extension method in an ASP.NET Core 2.x app (or using `AddJsonFile` and `AddEnvironmentVariables` directly in an ASP.NET Core 1.x app) adds configuration providers for reading JSON files and system configuration sources:</span></span>

* <span data-ttu-id="96e27-134">*appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="96e27-134">*appsettings.json*</span></span>
* <span data-ttu-id="96e27-135">*appSettings. \<EnvironmentName >. JSON*</span><span class="sxs-lookup"><span data-stu-id="96e27-135">*appsettings.\<EnvironmentName>.json*</span></span>
* <span data-ttu-id="96e27-136">variáveis de ambiente</span><span class="sxs-lookup"><span data-stu-id="96e27-136">environment variables</span></span>

<span data-ttu-id="96e27-137">Consulte [AddJsonFile](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.jsonconfigurationextensions) para obter uma explicação dos parâmetros.</span><span class="sxs-lookup"><span data-stu-id="96e27-137">See [AddJsonFile](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.jsonconfigurationextensions) for an explanation of the parameters.</span></span> <span data-ttu-id="96e27-138">`reloadOnChange`só é suportado no ASP.NET Core 1.1 e superior.</span><span class="sxs-lookup"><span data-stu-id="96e27-138">`reloadOnChange` is only supported in ASP.NET Core 1.1 and higher.</span></span> 

<span data-ttu-id="96e27-139">Fontes de configuração são lidos na ordem em que eles sejam especificados.</span><span class="sxs-lookup"><span data-stu-id="96e27-139">Configuration sources are read in the order they are specified.</span></span> <span data-ttu-id="96e27-140">No código acima, as variáveis de ambiente são lidos pela última.</span><span class="sxs-lookup"><span data-stu-id="96e27-140">In the code above, the environment variables are read last.</span></span> <span data-ttu-id="96e27-141">Quaisquer valores de configuração definido através do ambiente substituiria aquelas definidas em dois provedores anteriores.</span><span class="sxs-lookup"><span data-stu-id="96e27-141">Any configuration values set through the environment would replace those set in the two previous providers.</span></span>

<span data-ttu-id="96e27-142">O ambiente normalmente é definido como um dos `Development`, `Staging`, ou `Production`.</span><span class="sxs-lookup"><span data-stu-id="96e27-142">The environment is typically set to one of `Development`, `Staging`, or `Production`.</span></span> <span data-ttu-id="96e27-143">Consulte [trabalhando com vários ambientes](environments.md) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="96e27-143">See [Working with Multiple Environments](environments.md) for more information.</span></span>

<span data-ttu-id="96e27-144">Considerações de configuração:</span><span class="sxs-lookup"><span data-stu-id="96e27-144">Configuration considerations:</span></span>

* <span data-ttu-id="96e27-145">`IOptionsSnapshot`pode recarregar os dados de configuração quando ele é alterado.</span><span class="sxs-lookup"><span data-stu-id="96e27-145">`IOptionsSnapshot` can reload configuration data when it changes.</span></span> <span data-ttu-id="96e27-146">Use `IOptionsSnapshot` se você precisar recarregar os dados de configuração.</span><span class="sxs-lookup"><span data-stu-id="96e27-146">Use `IOptionsSnapshot` if you need to reload configuration data.</span></span>  <span data-ttu-id="96e27-147">Consulte [IOptionsSnapshot](#ioptionssnapshot) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="96e27-147">See [IOptionsSnapshot](#ioptionssnapshot) for more information.</span></span>
* <span data-ttu-id="96e27-148">Chaves de configuração diferenciam maiusculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="96e27-148">Configuration keys are case insensitive.</span></span>
* <span data-ttu-id="96e27-149">Uma prática recomendada é especificar variáveis de ambiente por último, para que o ambiente local pode substituir qualquer coisa definidas nos arquivos de configuração implantada.</span><span class="sxs-lookup"><span data-stu-id="96e27-149">A best practice is to specify environment variables last, so that the local environment can override anything set in deployed configuration files.</span></span>
* <span data-ttu-id="96e27-150">**Nunca** armazenar senhas ou outros dados confidenciais no código do provedor de configuração ou nos arquivos de configuração de texto sem formatação.</span><span class="sxs-lookup"><span data-stu-id="96e27-150">**Never** store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span> <span data-ttu-id="96e27-151">Não usa segredos de produção no desenvolvimento ou ambientes de teste.</span><span class="sxs-lookup"><span data-stu-id="96e27-151">Don't use production secrets in your development or test environments.</span></span> <span data-ttu-id="96e27-152">Em vez disso, especifique segredos fora da árvore de projeto para que eles não puderem ser acidentalmente confirmados para o repositório.</span><span class="sxs-lookup"><span data-stu-id="96e27-152">Instead, specify secrets outside the project tree, so they cannot be accidentally committed into your repository.</span></span> <span data-ttu-id="96e27-153">Saiba mais sobre [trabalhando com vários ambientes](environments.md) e gerenciando [armazenamento seguro de segredos do aplicativo durante o desenvolvimento](../security/app-secrets.md).</span><span class="sxs-lookup"><span data-stu-id="96e27-153">Learn more about [Working with Multiple Environments](environments.md) and managing [safe storage of app secrets during development](../security/app-secrets.md).</span></span>
* <span data-ttu-id="96e27-154">Se `:` não pode ser usado em variáveis de ambiente em seu sistema, substitua `:` com `__` (sublinhado duplo).</span><span class="sxs-lookup"><span data-stu-id="96e27-154">If `:` cannot be used in environment variables in your system,  replace `:`  with `__` (double underscore).</span></span>

<a name=options-config-objects></a>

## <a name="using-options-and-configuration-objects"></a><span data-ttu-id="96e27-155">Usando opções e objetos de configuração</span><span class="sxs-lookup"><span data-stu-id="96e27-155">Using Options and configuration objects</span></span>

<span data-ttu-id="96e27-156">O padrão de opções usa as classes de opções personalizada para representar um grupo de configurações relacionadas.</span><span class="sxs-lookup"><span data-stu-id="96e27-156">The options pattern uses custom options classes to represent a group of related settings.</span></span> <span data-ttu-id="96e27-157">É recomendável que você crie classes separadas para cada recurso dentro de seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="96e27-157">We recommended that you create decoupled classes for each feature within your app.</span></span> <span data-ttu-id="96e27-158">Execute as classes separadas:</span><span class="sxs-lookup"><span data-stu-id="96e27-158">Decoupled classes follow:</span></span>

* <span data-ttu-id="96e27-159">O [princípio de diferenciação de Interface (ISP)](http://deviq.com/interface-segregation-principle/) : Classes dependem apenas as definições de configuração que eles usam.</span><span class="sxs-lookup"><span data-stu-id="96e27-159">The [Interface Segregation Principle (ISP)](http://deviq.com/interface-segregation-principle/) : Classes depend only on the configuration settings they use.</span></span>
* <span data-ttu-id="96e27-160">[Separação de preocupações](http://deviq.com/separation-of-concerns/) : configurações para diferentes partes do seu aplicativo não são dependentes ou combinado com uma da outra.</span><span class="sxs-lookup"><span data-stu-id="96e27-160">[Separation of Concerns](http://deviq.com/separation-of-concerns/) : Settings for different parts of your app are not dependent or coupled with one another.</span></span>

<span data-ttu-id="96e27-161">A classe de opções deve ser não-abstrato com um construtor público sem parâmetros.</span><span class="sxs-lookup"><span data-stu-id="96e27-161">The options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="96e27-162">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="96e27-162">For example:</span></span>

[!code-csharp[Main](configuration/sample/UsingOptions/Models/MyOptions.cs)]

<a name=options-example></a>

<span data-ttu-id="96e27-163">No código a seguir, o provedor de configuração JSON está habilitado.</span><span class="sxs-lookup"><span data-stu-id="96e27-163">In the following code, the JSON configuration provider is enabled.</span></span> <span data-ttu-id="96e27-164">O `MyOptions` classe é adicionada ao contêiner de serviço e associada à configuração.</span><span class="sxs-lookup"><span data-stu-id="96e27-164">The `MyOptions` class is added to the service container and bound to configuration.</span></span>

[!code-csharp[Main](configuration/sample/UsingOptions/Startup.cs?name=snippet1&highlight=8,20-21)]

<span data-ttu-id="96e27-165">O seguinte [controlador](../mvc/controllers/index.md) usa [construtor injeção de dependência](xref:fundamentals/dependency-injection#what-is-dependency-injection) na [ `IOptions<TOptions>` ](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.options.ioptions-1) para acessar as configurações:</span><span class="sxs-lookup"><span data-stu-id="96e27-165">The following [controller](../mvc/controllers/index.md)  uses [constructor Dependency Injection](xref:fundamentals/dependency-injection#what-is-dependency-injection) on [`IOptions<TOptions>`](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.options.ioptions-1) to access settings:</span></span>

[!code-csharp[Main](configuration/sample/UsingOptions/Controllers/HomeController.cs?name=snippet1)]

<span data-ttu-id="96e27-166">Com o seguinte *appSettings. JSON* arquivo:</span><span class="sxs-lookup"><span data-stu-id="96e27-166">With the following *appsettings.json* file:</span></span>

[!code-json[Main](configuration/sample/UsingOptions/appsettings1.json)]

<span data-ttu-id="96e27-167">O `HomeController.Index` método retornará `option1 = value1_from_json, option2 = 2`.</span><span class="sxs-lookup"><span data-stu-id="96e27-167">The `HomeController.Index` method returns `option1 = value1_from_json, option2 = 2`.</span></span>

<span data-ttu-id="96e27-168">Aplicativos típicos não associar toda a configuração para um arquivo de opções de único.</span><span class="sxs-lookup"><span data-stu-id="96e27-168">Typical apps won't bind the entire configuration to a single options file.</span></span> <span data-ttu-id="96e27-169">Posteriormente, mostrarei como usar `GetSection` para vincular a uma seção.</span><span class="sxs-lookup"><span data-stu-id="96e27-169">Later on I'll show how to use `GetSection` to bind to a section.</span></span>

<span data-ttu-id="96e27-170">No código a seguir, um segundo `IConfigureOptions<TOptions>` serviço é adicionado ao contêiner de serviço.</span><span class="sxs-lookup"><span data-stu-id="96e27-170">In the following code, a second `IConfigureOptions<TOptions>` service is added to the service container.</span></span> <span data-ttu-id="96e27-171">Ele usa um delegado para configurar a associação com `MyOptions`.</span><span class="sxs-lookup"><span data-stu-id="96e27-171">It uses a delegate to configure the binding with `MyOptions`.</span></span>

[!code-csharp[Main](configuration/sample/UsingOptions/Startup2.cs?name=snippet1&highlight=9-13)]

<span data-ttu-id="96e27-172">Você pode adicionar vários provedores de configuração.</span><span class="sxs-lookup"><span data-stu-id="96e27-172">You can add multiple configuration providers.</span></span> <span data-ttu-id="96e27-173">Provedores de configuração estão disponíveis em pacotes do NuGet.</span><span class="sxs-lookup"><span data-stu-id="96e27-173">Configuration providers are available in NuGet packages.</span></span> <span data-ttu-id="96e27-174">Elas são aplicadas na ordem que eles são registrados.</span><span class="sxs-lookup"><span data-stu-id="96e27-174">They are applied in order they are registered.</span></span>

<span data-ttu-id="96e27-175">Cada chamada para `Configure<TOptions>` adiciona um `IConfigureOptions<TOptions>` serviço ao contêiner de serviço.</span><span class="sxs-lookup"><span data-stu-id="96e27-175">Each call to `Configure<TOptions>` adds an `IConfigureOptions<TOptions>` service to the service container.</span></span> <span data-ttu-id="96e27-176">No exemplo anterior, os valores de `Option1` e `Option2` estão especificados na *appSettings. JSON* –, mas o valor de `Option1` é substituído pelo delegado configurado.</span><span class="sxs-lookup"><span data-stu-id="96e27-176">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json* -- but the value of `Option1` is overridden by the configured delegate.</span></span> 

<span data-ttu-id="96e27-177">Quando mais de um serviço de configuração é habilitado, a última fonte de configuração especificado "wins" (define o valor de configuração).</span><span class="sxs-lookup"><span data-stu-id="96e27-177">When more than one configuration service is enabled, the last configuration source specified "wins" (sets the configuration value).</span></span> <span data-ttu-id="96e27-178">No código anterior, o `HomeController.Index` método retornará `option1 = value1_from_action, option2 = 2`.</span><span class="sxs-lookup"><span data-stu-id="96e27-178">In the preceding code, the `HomeController.Index` method returns `option1 = value1_from_action, option2 = 2`.</span></span>

<span data-ttu-id="96e27-179">Quando você associa opções de configuração, cada propriedade no seu tipo de opções está associada a uma chave de configuração do formulário `property[:sub-property:]`.</span><span class="sxs-lookup"><span data-stu-id="96e27-179">When you bind options to configuration, each property in your options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="96e27-180">Por exemplo, o `MyOptions.Option1` propriedade está associada à chave `Option1`, que são lidos a partir de `option1` propriedade em *appSettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="96e27-180">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span> <span data-ttu-id="96e27-181">Um exemplo de subpropriedade é mostrado mais adiante neste artigo.</span><span class="sxs-lookup"><span data-stu-id="96e27-181">A sub-property sample is shown later in this article.</span></span>

<span data-ttu-id="96e27-182">No código a seguir, um terceiro `IConfigureOptions<TOptions>` serviço é adicionado ao contêiner de serviço.</span><span class="sxs-lookup"><span data-stu-id="96e27-182">In the following code, a third `IConfigureOptions<TOptions>` service is added to the service container.</span></span> <span data-ttu-id="96e27-183">Ele é ligado `MySubOptions` a seção `subsection` do *appSettings. JSON* arquivo:</span><span class="sxs-lookup"><span data-stu-id="96e27-183">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[Main](configuration/sample/UsingOptions/Startup3.cs?name=snippet1&highlight=16-17)]

<span data-ttu-id="96e27-184">Observação: Esse método de extensão exige o `Microsoft.Extensions.Options.ConfigurationExtensions` pacote NuGet.</span><span class="sxs-lookup"><span data-stu-id="96e27-184">Note: This extension method requires the `Microsoft.Extensions.Options.ConfigurationExtensions` NuGet package.</span></span>

<span data-ttu-id="96e27-185">Usando o seguinte *appSettings. JSON* arquivo:</span><span class="sxs-lookup"><span data-stu-id="96e27-185">Using the following *appsettings.json* file:</span></span>

[!code-json[Main](configuration/sample/UsingOptions/appsettings.json)]

<span data-ttu-id="96e27-186">O `MySubOptions` classe:</span><span class="sxs-lookup"><span data-stu-id="96e27-186">The `MySubOptions` class:</span></span>

[!code-csharp[Main](configuration/sample/UsingOptions/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="96e27-187">Com os seguintes `Controller`:</span><span class="sxs-lookup"><span data-stu-id="96e27-187">With the following `Controller`:</span></span>

[!code-csharp[Main](configuration/sample/UsingOptions/Controllers/HomeController2.cs?name=snippet1)]

<span data-ttu-id="96e27-188">`subOption1 = subvalue1_from_json, subOption2 = 200`é retornado.</span><span class="sxs-lookup"><span data-stu-id="96e27-188">`subOption1 = subvalue1_from_json, subOption2 = 200` is returned.</span></span>

<span data-ttu-id="96e27-189">Você também pode fornecer opções em um modelo de exibição ou injetar `IOptions<TOptions>` diretamente em um modo de exibição:</span><span class="sxs-lookup"><span data-stu-id="96e27-189">You can also supply options in a view model or inject `IOptions<TOptions>` directly into a view:</span></span>

[!code-html[Main](configuration/sample/UsingOptions/Views/Home/Index.cshtml?highlight=3-4,16-17,20-21)]

<a name=in-memory-provider></a>

## <a name="ioptionssnapshot"></a><span data-ttu-id="96e27-190">IOptionsSnapshot</span><span class="sxs-lookup"><span data-stu-id="96e27-190">IOptionsSnapshot</span></span>

<span data-ttu-id="96e27-191">*Exige o ASP.NET Core 1.1 ou posterior.*</span><span class="sxs-lookup"><span data-stu-id="96e27-191">*Requires ASP.NET Core 1.1 or higher.*</span></span>

<span data-ttu-id="96e27-192">`IOptionsSnapshot`dá suporte a recarregar os dados de configuração quando o arquivo de configuração foi alterada.</span><span class="sxs-lookup"><span data-stu-id="96e27-192">`IOptionsSnapshot` supports reloading configuration data when the configuration file has changed.</span></span> <span data-ttu-id="96e27-193">Ele tem uma sobrecarga mínima.</span><span class="sxs-lookup"><span data-stu-id="96e27-193">It has minimal overhead.</span></span> <span data-ttu-id="96e27-194">Usando `IOptionsSnapshot` com `reloadOnChange: true`, as opções são vinculadas a `IConfiguration` e recarregados quando alterado.</span><span class="sxs-lookup"><span data-stu-id="96e27-194">Using `IOptionsSnapshot` with `reloadOnChange: true`, the options are bound to `IConfiguration` and reloaded when changed.</span></span>

<span data-ttu-id="96e27-195">O exemplo a seguir demonstra como um novo `IOptionsSnapshot` é criada após *config. JSON* alterações.</span><span class="sxs-lookup"><span data-stu-id="96e27-195">The following sample demonstrates how a new `IOptionsSnapshot` is created after *config.json* changes.</span></span> <span data-ttu-id="96e27-196">Solicitações para o servidor retornará o mesmo tempo quando *config. JSON* tem **não** alterado.</span><span class="sxs-lookup"><span data-stu-id="96e27-196">Requests to the server will return the same time when *config.json* has **not** changed.</span></span> <span data-ttu-id="96e27-197">A primeira solicitação após *config. JSON* alterações mostrará uma nova hora.</span><span class="sxs-lookup"><span data-stu-id="96e27-197">The first request after *config.json* changes will show a new time.</span></span>

[!code-csharp[Main](configuration/sample/IOptionsSnapshot2/Startup.cs?name=snippet1&highlight=1-9,13-18,32,33,52,53)]

<span data-ttu-id="96e27-198">A imagem a seguir mostra a saída do servidor:</span><span class="sxs-lookup"><span data-stu-id="96e27-198">The following image shows the server output:</span></span>

![mostrando de imagem do navegador "última atualização: 22/11/2016 16:43:00"](configuration/_static/first.png)

<span data-ttu-id="96e27-200">Atualizar o navegador não altera o valor de mensagem ou hora exibida (quando *config. JSON* não foi alterado).</span><span class="sxs-lookup"><span data-stu-id="96e27-200">Refreshing the browser doesn't change the message value or time displayed (when *config.json* has not changed).</span></span>

<span data-ttu-id="96e27-201">Alterar e salvar o *config. JSON* e, em seguida, atualize o navegador:</span><span class="sxs-lookup"><span data-stu-id="96e27-201">Change and save the  *config.json* and then refresh the browser:</span></span>

![mostrando de imagem do navegador "última atualização para e: 22/11/2016 4:53 PM"](configuration/_static/change.png)

## <a name="in-memory-provider-and-binding-to-a-poco-class"></a><span data-ttu-id="96e27-203">Provedor na memória e a associação a uma classe POCO</span><span class="sxs-lookup"><span data-stu-id="96e27-203">In-memory provider and binding to a POCO class</span></span>

<span data-ttu-id="96e27-204">O exemplo a seguir mostra como usar o provedor na memória e vincular a uma classe:</span><span class="sxs-lookup"><span data-stu-id="96e27-204">The following sample shows how to use the in-memory provider and bind to a class:</span></span>

[!code-csharp[Main](configuration/sample/InMemory/Program.cs)]

<span data-ttu-id="96e27-205">Os valores de configuração são retornados como cadeias de caracteres, mas a associação permite que a construção de objetos.</span><span class="sxs-lookup"><span data-stu-id="96e27-205">Configuration values are returned as strings, but binding enables the construction of objects.</span></span> <span data-ttu-id="96e27-206">Associação permite que você recupere POCO objetos ou gráficos de objeto inteiro até mesmo.</span><span class="sxs-lookup"><span data-stu-id="96e27-206">Binding allows you to retrieve POCO objects or even entire object graphs.</span></span> <span data-ttu-id="96e27-207">O exemplo a seguir mostra como associar a `MyWindow` e usar o padrão de opções com um aplicativo MVC do ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="96e27-207">The following sample shows how to bind to `MyWindow` and use the options pattern with a ASP.NET Core MVC app:</span></span>

[!code-csharp[Main](configuration/sample/WebConfigBind/MyWindow.cs)]

[!code-json[Main](configuration/sample/WebConfigBind/appsettings.json)]

<span data-ttu-id="96e27-208">Associar a classe personalizada em `ConfigureServices` quando estiver criando o host:</span><span class="sxs-lookup"><span data-stu-id="96e27-208">Bind the custom class in `ConfigureServices` when building the host:</span></span>

[!code-csharp[Main](configuration/sample/WebConfigBind/Program.cs?name=snippet1&highlight=3-4)]

<span data-ttu-id="96e27-209">Exibir as configurações de `HomeController`:</span><span class="sxs-lookup"><span data-stu-id="96e27-209">Display the settings from the `HomeController`:</span></span>

[!code-csharp[Main](configuration/sample/WebConfigBind/Controllers/HomeController.cs)]

### <a name="getvalue"></a><span data-ttu-id="96e27-210">GetValue</span><span class="sxs-lookup"><span data-stu-id="96e27-210">GetValue</span></span>

<span data-ttu-id="96e27-211">O exemplo a seguir demonstra o [GetValue<T> ](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationbinder#Microsoft_Extensions_Configuration_ConfigurationBinder_GetValue_Microsoft_Extensions_Configuration_IConfiguration_System_Type_System_String_System_Object_) método de extensão:</span><span class="sxs-lookup"><span data-stu-id="96e27-211">The following sample demonstrates the [GetValue<T>](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationbinder#Microsoft_Extensions_Configuration_ConfigurationBinder_GetValue_Microsoft_Extensions_Configuration_IConfiguration_System_Type_System_String_System_Object_) extension method:</span></span>

[!code-csharp[Main](configuration/sample/InMemoryGetValue/Program.cs?highlight=27-29)]

<span data-ttu-id="96e27-212">O ConfigurationBinder `GetValue<T>` método permite que você especifique um valor padrão (80 no exemplo).</span><span class="sxs-lookup"><span data-stu-id="96e27-212">The ConfigurationBinder's `GetValue<T>` method allows you to specify a default value (80 in the sample).</span></span> <span data-ttu-id="96e27-213">`GetValue<T>`para cenários simples e não vincular ao seções inteiras.</span><span class="sxs-lookup"><span data-stu-id="96e27-213">`GetValue<T>` is for simple scenarios and does not bind to entire sections.</span></span> <span data-ttu-id="96e27-214">`GetValue<T>`Obtém os valores escalares de `GetSection(key).Value` convertido em um tipo específico.</span><span class="sxs-lookup"><span data-stu-id="96e27-214">`GetValue<T>` gets scalar values from `GetSection(key).Value` converted to a specific type.</span></span>

## <a name="binding-to-an-object-graph"></a><span data-ttu-id="96e27-215">Associando a um gráfico de objeto</span><span class="sxs-lookup"><span data-stu-id="96e27-215">Binding to an object graph</span></span>

<span data-ttu-id="96e27-216">Você pode vincular recursivamente para cada objeto em uma classe.</span><span class="sxs-lookup"><span data-stu-id="96e27-216">You can recursively bind to each object in a class.</span></span> <span data-ttu-id="96e27-217">Considere o seguinte `AppOptions` classe:</span><span class="sxs-lookup"><span data-stu-id="96e27-217">Consider the following `AppOptions` class:</span></span>

[!code-csharp[Main](configuration/sample/ObjectGraph/AppOptions.cs)]

<span data-ttu-id="96e27-218">O exemplo a seguir associa ao `AppOptions` classe:</span><span class="sxs-lookup"><span data-stu-id="96e27-218">The following sample binds to the `AppOptions` class:</span></span>

[!code-csharp[Main](configuration/sample/ObjectGraph/Program.cs?highlight=15-16)]

<span data-ttu-id="96e27-219">**ASP.NET Core 1.1** e superior podem usar `Get<T>`, que trabalha com seções inteiras.</span><span class="sxs-lookup"><span data-stu-id="96e27-219">**ASP.NET Core 1.1** and higher can use  `Get<T>`, which works with entire sections.</span></span> <span data-ttu-id="96e27-220">`Get<T>`pode ser mais convienent que usar `Bind`.</span><span class="sxs-lookup"><span data-stu-id="96e27-220">`Get<T>` can be more convienent than using `Bind`.</span></span> <span data-ttu-id="96e27-221">O código a seguir mostra como usar `Get<T>` com o exemplo acima:</span><span class="sxs-lookup"><span data-stu-id="96e27-221">The following code shows how to use `Get<T>` with the sample above:</span></span>

```csharp
var appConfig = config.GetSection("App").Get<AppOptions>();
```

<span data-ttu-id="96e27-222">Usando o seguinte *appSettings. JSON* arquivo:</span><span class="sxs-lookup"><span data-stu-id="96e27-222">Using the following *appsettings.json* file:</span></span>

[!code-json[Main](configuration/sample/ObjectGraph/appsettings.json)]

<span data-ttu-id="96e27-223">O programa exibe `Height 11`.</span><span class="sxs-lookup"><span data-stu-id="96e27-223">The program displays `Height 11`.</span></span>

<span data-ttu-id="96e27-224">O código a seguir pode ser usado para a unidade a configuração de teste:</span><span class="sxs-lookup"><span data-stu-id="96e27-224">The following code can be used to unit test the configuration:</span></span>

```csharp
[Fact]
public void CanBindObjectTree()
{
    var dict = new Dictionary<string, string>
        {
            {"App:Profile:Machine", "Rick"},
            {"App:Connection:Value", "connectionstring"},
            {"App:Window:Height", "11"},
            {"App:Window:Width", "11"}
        };
    var builder = new ConfigurationBuilder();
    builder.AddInMemoryCollection(dict);
    var config = builder.Build();

    var options = new AppOptions();
    config.GetSection("App").Bind(options);

    Assert.Equal("Rick", options.Profile.Machine);
    Assert.Equal(11, options.Window.Height);
    Assert.Equal(11, options.Window.Width);
    Assert.Equal("connectionstring", options.Connection.Value);
}
```

<a name=custom-config-providers></a>

## <a name="basic-sample-of-entity-framework-custom-provider"></a><span data-ttu-id="96e27-225">Exemplo básico do provedor personalizado do Entity Framework</span><span class="sxs-lookup"><span data-stu-id="96e27-225">Basic sample of Entity Framework custom provider</span></span>

<span data-ttu-id="96e27-226">Nesta seção, é criado um provedor de configuração básica que lê os pares nome-valor de um banco de dados usando EF.</span><span class="sxs-lookup"><span data-stu-id="96e27-226">In this section, a basic configuration provider that reads name-value pairs from a database using EF is created.</span></span> 

<span data-ttu-id="96e27-227">Definir um `ConfigurationValue` entidade para armazenar valores de configuração no banco de dados:</span><span class="sxs-lookup"><span data-stu-id="96e27-227">Define a `ConfigurationValue` entity for storing configuration values in the database:</span></span>

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/ConfigurationValue.cs)]

<span data-ttu-id="96e27-228">Adicionar um `ConfigurationContext` para armazenar e acessar os valores configurados:</span><span class="sxs-lookup"><span data-stu-id="96e27-228">Add a `ConfigurationContext` to store and access the configured values:</span></span>

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/ConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="96e27-229">Criar uma classe que implementa [IConfigurationSource](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.iconfigurationsource):</span><span class="sxs-lookup"><span data-stu-id="96e27-229">Create an class that implements [IConfigurationSource](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.iconfigurationsource):</span></span>

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/EntityFrameworkConfigurationSource.cs?highlight=7)]

<span data-ttu-id="96e27-230">Criar o provedor de configuração personalizado herdando de [ConfigurationProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationprovider).</span><span class="sxs-lookup"><span data-stu-id="96e27-230">Create the custom configuration provider by inheriting from [ConfigurationProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationprovider).</span></span>  <span data-ttu-id="96e27-231">O provedor de configuração inicializa o banco de dados quando ela estiver vazia:</span><span class="sxs-lookup"><span data-stu-id="96e27-231">The configuration provider initializes the database when it's empty:</span></span>

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/EntityFrameworkConfigurationProvider.cs?highlight=9,18-31,38-39)]

<span data-ttu-id="96e27-232">Os valores realçados do banco de dados ("value_from_ef_1" e "value_from_ef_2") são exibidos quando o exemplo for executado.</span><span class="sxs-lookup"><span data-stu-id="96e27-232">The highlighted values from the database ("value_from_ef_1" and "value_from_ef_2") are displayed when the sample is run.</span></span>

<span data-ttu-id="96e27-233">Você pode adicionar um `EFConfigSource` método de extensão para adicionar a fonte de configuração:</span><span class="sxs-lookup"><span data-stu-id="96e27-233">You can add an `EFConfigSource` extension method for adding the configuration source:</span></span>

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/EntityFrameworkExtensions.cs?highlight=12)]

<span data-ttu-id="96e27-234">O código a seguir mostra como usar personalizado `EFConfigProvider`:</span><span class="sxs-lookup"><span data-stu-id="96e27-234">The following code shows how to use the custom `EFConfigProvider`:</span></span>

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/Program.cs?highlight=21-26)]

<span data-ttu-id="96e27-235">Observe que o exemplo adiciona personalizado `EFConfigProvider` depois que o provedor JSON, então todas as configurações do banco de dados irá substituir as configurações do *appSettings. JSON* arquivo.</span><span class="sxs-lookup"><span data-stu-id="96e27-235">Note the sample adds the custom `EFConfigProvider` after the JSON provider, so any settings from the database will override settings from the *appsettings.json* file.</span></span>

<span data-ttu-id="96e27-236">Usando o seguinte *appSettings. JSON* arquivo:</span><span class="sxs-lookup"><span data-stu-id="96e27-236">Using the following *appsettings.json* file:</span></span>

[!code-json[Main](configuration/sample/CustomConfigurationProvider/appsettings.json)]

<span data-ttu-id="96e27-237">O seguinte é exibido:</span><span class="sxs-lookup"><span data-stu-id="96e27-237">The following is displayed:</span></span>

```console
key1=value_from_ef_1
key2=value_from_ef_2
key3=value_from_json_3
```

## <a name="commandline-configuration-provider"></a><span data-ttu-id="96e27-238">Provedor de configuração de linha de comando</span><span class="sxs-lookup"><span data-stu-id="96e27-238">CommandLine configuration provider</span></span>

<span data-ttu-id="96e27-239">O [provedor de configuração de linha de comando](/aspnet/core/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider) recebe pares de chave-valor do argumento de linha de comando para a configuração em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="96e27-239">The [CommandLine configuration provider](/aspnet/core/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider) receives command-line argument key-value pairs for configuration at runtime.</span></span>

[<span data-ttu-id="96e27-240">Exibir ou baixar o exemplo de configuração de linha de comando</span><span class="sxs-lookup"><span data-stu-id="96e27-240">View or download the CommandLine configuration sample</span></span>](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/sample/CommandLine)

### <a name="setting-up-the-provider"></a><span data-ttu-id="96e27-241">Configurando o provedor</span><span class="sxs-lookup"><span data-stu-id="96e27-241">Setting up the provider</span></span>

# <a name="basic-configurationtabbasicconfiguration"></a>[<span data-ttu-id="96e27-242">Configuração básica</span><span class="sxs-lookup"><span data-stu-id="96e27-242">Basic Configuration</span></span>](#tab/basicconfiguration)

<span data-ttu-id="96e27-243">Para ativar a configuração de linha de comando, chame o `AddCommandLine` método de extensão em uma instância do [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder):</span><span class="sxs-lookup"><span data-stu-id="96e27-243">To activate command-line configuration, call the `AddCommandLine` extension method on an instance of [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder):</span></span>

[!code-csharp[Main](configuration/sample_snapshot/CommandLine/Program.cs?highlight=18,21)]

<span data-ttu-id="96e27-244">Executar o código, a seguinte saída é exibida:</span><span class="sxs-lookup"><span data-stu-id="96e27-244">Running the code, the following output is displayed:</span></span>

```console
MachineName: MairaPC
Left: 1980
```

<span data-ttu-id="96e27-245">Passando pares chave-valor do argumento na linha de comando altera os valores de `Profile:MachineName` e `App:MainWindow:Left`:</span><span class="sxs-lookup"><span data-stu-id="96e27-245">Passing argument key-value pairs on the command line changes the values of `Profile:MachineName` and `App:MainWindow:Left`:</span></span>

```console
dotnet run Profile:MachineName=BartPC App:MainWindow:Left=1979
```

<span data-ttu-id="96e27-246">Exibe a janela do console:</span><span class="sxs-lookup"><span data-stu-id="96e27-246">The console window displays:</span></span>

```console
MachineName: BartPC
Left: 1979
```

<span data-ttu-id="96e27-247">Para substituir a configuração fornecida por outros provedores de configuração com a configuração de linha de comando, chame `AddCommandLine` último em `ConfigurationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="96e27-247">To override configuration provided by other configuration providers with command-line configuration, call `AddCommandLine` last on `ConfigurationBuilder`:</span></span>

[!code-csharp[Main](configuration/sample_snapshot/CommandLine/Program2.cs?range=11-16&highlight=1,5)]

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="96e27-248">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="96e27-248">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="96e27-249">Aplicativos típicos de 2. x ASP.NET Core usam o método estático de conveniência `CreateDefaultBuilder` para criar o host:</span><span class="sxs-lookup"><span data-stu-id="96e27-249">Typical ASP.NET Core 2.x apps use the static convenience method `CreateDefaultBuilder` to build the host:</span></span>

[!code-csharp[Main](configuration/sample_snapshot/Program.cs?highlight=12)]

<span data-ttu-id="96e27-250">`CreateDefaultBuilder`carrega a configuração opcional de *appSettings. JSON*, *appsettings. { . JSON de ambiente}*, [segredos do usuário](xref:security/app-secrets) (no `Development` ambiente), variáveis de ambiente e os argumentos de linha de comando.</span><span class="sxs-lookup"><span data-stu-id="96e27-250">`CreateDefaultBuilder` loads optional configuration from *appsettings.json*, *appsettings.{Environment}.json*, [user secrets](xref:security/app-secrets) (in the `Development` environment), environment variables, and command-line arguments.</span></span> <span data-ttu-id="96e27-251">O provedor de configuração de linha de comando é chamado por último.</span><span class="sxs-lookup"><span data-stu-id="96e27-251">The CommandLine configuration provider is called last.</span></span> <span data-ttu-id="96e27-252">Chamando o provedor última permite que os argumentos de linha de comando passados em tempo de execução para substituir a configuração definida por outros provedores de configuração chamado anteriormente.</span><span class="sxs-lookup"><span data-stu-id="96e27-252">Calling the provider last allows the command-line arguments passed at runtime to override configuration set by the other configuration providers called earlier.</span></span>

<span data-ttu-id="96e27-253">Observe que para *appsettings* arquivos `reloadOnChange` está habilitado.</span><span class="sxs-lookup"><span data-stu-id="96e27-253">Note that for *appsettings* files that `reloadOnChange` is enabled.</span></span> <span data-ttu-id="96e27-254">Argumentos de linha de comando são substituídos se o valor de configuração correspondente em uma *appsettings* arquivo for alterado depois que o aplicativo é iniciado.</span><span class="sxs-lookup"><span data-stu-id="96e27-254">Command-line arguments are overridden if a matching configuration value in an *appsettings* file is changed after the app starts.</span></span>

> [!NOTE]
> <span data-ttu-id="96e27-255">Como uma alternativa ao uso de `CreateDefaultBuilder` método, criando um host usando [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) e criar manualmente a configuração com [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) é suportado no ASP.NET Core 2. x.</span><span class="sxs-lookup"><span data-stu-id="96e27-255">As an alternative to using the `CreateDefaultBuilder` method, creating a host using [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) and manually building configuration with [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) is supported in ASP.NET Core 2.x.</span></span> <span data-ttu-id="96e27-256">Consulte a guia de 1. x do ASP.NET Core para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="96e27-256">See the ASP.NET Core 1.x tab for more information.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="96e27-257">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="96e27-257">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="96e27-258">Criar um [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) e chamar o `AddCommandLine` método para usar o provedor de configuração de linha de comando.</span><span class="sxs-lookup"><span data-stu-id="96e27-258">Create a [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) and call the `AddCommandLine` method to use the CommandLine configuration provider.</span></span> <span data-ttu-id="96e27-259">Chamando o provedor última permite que os argumentos de linha de comando passados em tempo de execução para substituir a configuração definida por outros provedores de configuração chamado anteriormente.</span><span class="sxs-lookup"><span data-stu-id="96e27-259">Calling the provider last allows the command-line arguments passed at runtime to override configuration set by the other configuration providers called earlier.</span></span> <span data-ttu-id="96e27-260">Aplicar a configuração de [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) com o `UseConfiguration` método:</span><span class="sxs-lookup"><span data-stu-id="96e27-260">Apply the configuration to [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) with the `UseConfiguration` method:</span></span>

[!code-csharp[Main](configuration/sample_snapshot/CommandLine/Program2.cs?highlight=11,15,19)]

---

### <a name="arguments"></a><span data-ttu-id="96e27-261">Arguments</span><span class="sxs-lookup"><span data-stu-id="96e27-261">Arguments</span></span>

<span data-ttu-id="96e27-262">Argumentos passados na linha de comando devem estar de acordo com um dos dois formatos mostrados na tabela a seguir.</span><span class="sxs-lookup"><span data-stu-id="96e27-262">Arguments passed on the command line must conform to one of two formats shown in the following table.</span></span>

| <span data-ttu-id="96e27-263">Formato de argumento</span><span class="sxs-lookup"><span data-stu-id="96e27-263">Argument format</span></span>                                                     | <span data-ttu-id="96e27-264">Exemplo</span><span class="sxs-lookup"><span data-stu-id="96e27-264">Example</span></span>        |
| ------------------------------------------------------------------- | :------------: |
| <span data-ttu-id="96e27-265">Um único argumento: um par chave-valor separados por um sinal de igual (`=`)</span><span class="sxs-lookup"><span data-stu-id="96e27-265">Single argument: a key-value pair separated by an equals sign (`=`)</span></span> | `key1=value`   |
| <span data-ttu-id="96e27-266">Sequência de dois argumentos: um par chave-valor separados por um espaço</span><span class="sxs-lookup"><span data-stu-id="96e27-266">Sequence of two arguments: a key-value pair separated by a space</span></span>    | `/key1 value1` |

<span data-ttu-id="96e27-267">**Argumento único**</span><span class="sxs-lookup"><span data-stu-id="96e27-267">**Single argument**</span></span>

<span data-ttu-id="96e27-268">O valor deve seguir um sinal de igual (`=`).</span><span class="sxs-lookup"><span data-stu-id="96e27-268">The value must follow an equals sign (`=`).</span></span> <span data-ttu-id="96e27-269">O valor pode ser nulo (por exemplo, `mykey=`).</span><span class="sxs-lookup"><span data-stu-id="96e27-269">The value can be null (for example, `mykey=`).</span></span>

<span data-ttu-id="96e27-270">A chave pode ter um prefixo.</span><span class="sxs-lookup"><span data-stu-id="96e27-270">The key may have a prefix.</span></span>

| <span data-ttu-id="96e27-271">Prefixo da chave</span><span class="sxs-lookup"><span data-stu-id="96e27-271">Key prefix</span></span>               | <span data-ttu-id="96e27-272">Exemplo</span><span class="sxs-lookup"><span data-stu-id="96e27-272">Example</span></span>         |
| ------------------------ | :-------------: |
| <span data-ttu-id="96e27-273">Nenhum prefixo</span><span class="sxs-lookup"><span data-stu-id="96e27-273">No prefix</span></span>                | `key1=value1`   |
| <span data-ttu-id="96e27-274">Um único traço (`-`) &#8224;</span><span class="sxs-lookup"><span data-stu-id="96e27-274">Single dash (`-`)&#8224;</span></span> | `-key2=value2`  |
| <span data-ttu-id="96e27-275">Dois traços (`--`)</span><span class="sxs-lookup"><span data-stu-id="96e27-275">Two dashes (`--`)</span></span>        | `--key3=value3` |
| <span data-ttu-id="96e27-276">Barra (`/`)</span><span class="sxs-lookup"><span data-stu-id="96e27-276">Forward slash (`/`)</span></span>      | `/key4=value4`  |

<span data-ttu-id="96e27-277">&#8224; Uma chave com um prefixo de traço único (`-`) devem ser fornecidos em [alternar mapeamentos](#switch-mappings), descrito abaixo.</span><span class="sxs-lookup"><span data-stu-id="96e27-277">&#8224;A key with a single dash prefix (`-`) must be provided in [switch mappings](#switch-mappings), described below.</span></span>

<span data-ttu-id="96e27-278">Exemplo de comando:</span><span class="sxs-lookup"><span data-stu-id="96e27-278">Example command:</span></span>

```console
dotnet run key1=value1 -key2=value2 --key3=value3 /key4=value4
```

<span data-ttu-id="96e27-279">Observação: Se `-key1` não está presente no [alternar mapeamentos](#switch-mappings) fornecido para o provedor de configuração, um `FormatException` é gerada.</span><span class="sxs-lookup"><span data-stu-id="96e27-279">Note: If `-key1` isn't present in the [switch mappings](#switch-mappings) given to the configuration provider, a `FormatException` is thrown.</span></span>

<span data-ttu-id="96e27-280">**Sequência de dois argumentos**</span><span class="sxs-lookup"><span data-stu-id="96e27-280">**Sequence of two arguments**</span></span>

<span data-ttu-id="96e27-281">O valor não pode ser nulo e deve seguir a chave separada por um espaço.</span><span class="sxs-lookup"><span data-stu-id="96e27-281">The value can't be null and must follow the key separated by a space.</span></span>

<span data-ttu-id="96e27-282">A chave deve ter um prefixo.</span><span class="sxs-lookup"><span data-stu-id="96e27-282">The key must have a prefix.</span></span>

| <span data-ttu-id="96e27-283">Prefixo da chave</span><span class="sxs-lookup"><span data-stu-id="96e27-283">Key prefix</span></span>               | <span data-ttu-id="96e27-284">Exemplo</span><span class="sxs-lookup"><span data-stu-id="96e27-284">Example</span></span>         |
| ------------------------ | :-------------: |
| <span data-ttu-id="96e27-285">Um único traço (`-`) &#8224;</span><span class="sxs-lookup"><span data-stu-id="96e27-285">Single dash (`-`)&#8224;</span></span> | `-key1 value1`  |
| <span data-ttu-id="96e27-286">Dois traços (`--`)</span><span class="sxs-lookup"><span data-stu-id="96e27-286">Two dashes (`--`)</span></span>        | `--key2 value2` |
| <span data-ttu-id="96e27-287">Barra (`/`)</span><span class="sxs-lookup"><span data-stu-id="96e27-287">Forward slash (`/`)</span></span>      | `/key3 value3`  |

<span data-ttu-id="96e27-288">&#8224; Uma chave com um prefixo de traço único (`-`) devem ser fornecidos em [alternar mapeamentos](#switch-mappings), descrito abaixo.</span><span class="sxs-lookup"><span data-stu-id="96e27-288">&#8224;A key with a single dash prefix (`-`) must be provided in [switch mappings](#switch-mappings), described below.</span></span>

<span data-ttu-id="96e27-289">Exemplo de comando:</span><span class="sxs-lookup"><span data-stu-id="96e27-289">Example command:</span></span>

```console
dotnet run -key1 value1 --key2 value2 /key3 value3
```

<span data-ttu-id="96e27-290">Observação: Se `-key1` não está presente no [alternar mapeamentos](#switch-mappings) fornecido para o provedor de configuração, um `FormatException` é gerada.</span><span class="sxs-lookup"><span data-stu-id="96e27-290">Note: If `-key1` isn't present in the [switch mappings](#switch-mappings) given to the configuration provider, a `FormatException` is thrown.</span></span>

### <a name="duplicate-keys"></a><span data-ttu-id="96e27-291">Chaves duplicadas</span><span class="sxs-lookup"><span data-stu-id="96e27-291">Duplicate keys</span></span>

<span data-ttu-id="96e27-292">Se as chaves duplicadas são fornecidas, o último par chave-valor é usado.</span><span class="sxs-lookup"><span data-stu-id="96e27-292">If duplicate keys are provided, the last key-value pair is used.</span></span>

### <a name="switch-mappings"></a><span data-ttu-id="96e27-293">Mapeamentos de comutador</span><span class="sxs-lookup"><span data-stu-id="96e27-293">Switch mappings</span></span>

<span data-ttu-id="96e27-294">Ao criar manualmente a configuração com `ConfigurationBuilder`, opcionalmente, você pode fornecer um dicionário de mapeamentos de comutador para o `AddCommandLine` método.</span><span class="sxs-lookup"><span data-stu-id="96e27-294">When manually building configuration with `ConfigurationBuilder`, you can optionally provide a switch mappings dictionary to the `AddCommandLine` method.</span></span> <span data-ttu-id="96e27-295">Mapeamentos de comutador permitem que você forneça a lógica de substituição do nome da chave.</span><span class="sxs-lookup"><span data-stu-id="96e27-295">Switch mappings allow you to provide key name replacement logic.</span></span>

<span data-ttu-id="96e27-296">Quando o dicionário de mapeamentos de comutador é usado, o dicionário é verificado para uma chave que corresponde à chave fornecida por um argumento de linha de comando.</span><span class="sxs-lookup"><span data-stu-id="96e27-296">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="96e27-297">Se a chave de linha de comando é encontrada no dicionário, o valor do dicionário (de substituição de chave) é passado para definir a configuração.</span><span class="sxs-lookup"><span data-stu-id="96e27-297">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the configuration.</span></span> <span data-ttu-id="96e27-298">Um mapeamento de comutador é necessário para qualquer chave de linha de comando prefixado com um único traço (`-`).</span><span class="sxs-lookup"><span data-stu-id="96e27-298">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="96e27-299">Regras de chave de dicionário de mapeamentos do comutador:</span><span class="sxs-lookup"><span data-stu-id="96e27-299">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="96e27-300">Comutadores devem começar com um traço (`-`) ou traço duplo (`--`).</span><span class="sxs-lookup"><span data-stu-id="96e27-300">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="96e27-301">O dicionário de mapeamentos de chave não deve conter chaves duplicadas.</span><span class="sxs-lookup"><span data-stu-id="96e27-301">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="96e27-302">No exemplo a seguir, o `GetSwitchMappings` método permite que seus argumentos de linha de comando usar um único traço (`-`) chave prefixo e evitar prefixos subchaves à esquerda.</span><span class="sxs-lookup"><span data-stu-id="96e27-302">In the following example, the `GetSwitchMappings` method allows your command-line arguments to use a single dash (`-`) key prefix and avoid leading subkey prefixes.</span></span>

[!code-csharp[Main](configuration/sample/CommandLine/Program.cs?highlight=10-19,32)]

<span data-ttu-id="96e27-303">Sem fornecer argumentos de linha de comando, o dicionário fornecido para `AddInMemoryCollection` define os valores de configuração.</span><span class="sxs-lookup"><span data-stu-id="96e27-303">Without providing command-line arguments, the dictionary provided to `AddInMemoryCollection` sets the configuration values.</span></span> <span data-ttu-id="96e27-304">Execute o aplicativo com o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="96e27-304">Run the app with the following command:</span></span>

```console
dotnet run
```

<span data-ttu-id="96e27-305">Exibe a janela do console:</span><span class="sxs-lookup"><span data-stu-id="96e27-305">The console window displays:</span></span>

```console
MachineName: RickPC
Left: 1980
```

<span data-ttu-id="96e27-306">Use o seguinte para passar parâmetros de configuração:</span><span class="sxs-lookup"><span data-stu-id="96e27-306">Use the following to pass in configuration settings:</span></span>

```console
dotnet run /Profile:MachineName=DahliaPC /App:MainWindow:Left=1984
```

<span data-ttu-id="96e27-307">Exibe a janela do console:</span><span class="sxs-lookup"><span data-stu-id="96e27-307">The console window displays:</span></span>

```console
MachineName: DahliaPC
Left: 1984
```

<span data-ttu-id="96e27-308">Depois que o dicionário de mapeamentos de chave é criado, ele contém os dados mostrados na tabela a seguir.</span><span class="sxs-lookup"><span data-stu-id="96e27-308">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="96e27-309">Chave</span><span class="sxs-lookup"><span data-stu-id="96e27-309">Key</span></span>            | <span data-ttu-id="96e27-310">Valor</span><span class="sxs-lookup"><span data-stu-id="96e27-310">Value</span></span>                 |
| -------------- | --------------------- |
| `-MachineName` | `Profile:MachineName` |
| `-Left`        | `App:MainWindow:Left` |

<span data-ttu-id="96e27-311">Para demonstrar a troca de chaves usando o dicionário, execute o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="96e27-311">To demonstrate key switching using the dictionary, run the following command:</span></span>

```console
dotnet run -MachineName=ChadPC -Left=1988
```

<span data-ttu-id="96e27-312">As chaves de linha de comando são trocadas.</span><span class="sxs-lookup"><span data-stu-id="96e27-312">The command-line keys are swapped.</span></span> <span data-ttu-id="96e27-313">A janela de console exibe os valores de configuração `Profile:MachineName` e `App:MainWindow:Left`:</span><span class="sxs-lookup"><span data-stu-id="96e27-313">The console window displays the configuration values for `Profile:MachineName` and `App:MainWindow:Left`:</span></span>

```console
MachineName: ChadPC
Left: 1988
```

## <a name="the-webconfig-file"></a><span data-ttu-id="96e27-314">O arquivo Web. config</span><span class="sxs-lookup"><span data-stu-id="96e27-314">The web.config file</span></span>

<span data-ttu-id="96e27-315">Um *Web. config* arquivo é necessário quando você hospeda o aplicativo no IIS ou IIS Express.</span><span class="sxs-lookup"><span data-stu-id="96e27-315">A *web.config* file is required when you host the app in IIS or IIS-Express.</span></span> <span data-ttu-id="96e27-316">*Web. config* ativa AspNetCoreModule no IIS para iniciar seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="96e27-316">*web.config* turns on the AspNetCoreModule in IIS to launch your app.</span></span> <span data-ttu-id="96e27-317">As configurações no *Web. config* habilitar AspNetCoreModule no IIS para iniciar seu aplicativo e definir outras configurações do IIS e módulos.</span><span class="sxs-lookup"><span data-stu-id="96e27-317">Settings in *web.config* enable the AspNetCoreModule in IIS to launch your app and configure other IIS settings and modules.</span></span> <span data-ttu-id="96e27-318">Se você estiver usando o Visual Studio e excluir *Web. config*, Visual Studio irá criar um novo.</span><span class="sxs-lookup"><span data-stu-id="96e27-318">If you are using Visual Studio and delete *web.config*, Visual Studio will create a new one.</span></span>

## <a name="additional-notes"></a><span data-ttu-id="96e27-319">Observações adicionais</span><span class="sxs-lookup"><span data-stu-id="96e27-319">Additional notes</span></span>

* <span data-ttu-id="96e27-320">Injeção de dependência (DI) não está definido até depois `ConfigureServices` é invocado.</span><span class="sxs-lookup"><span data-stu-id="96e27-320">Dependency Injection (DI) is not set up until after `ConfigureServices` is invoked.</span></span>
* <span data-ttu-id="96e27-321">O sistema de configuração não está ciente de injeção de dependência.</span><span class="sxs-lookup"><span data-stu-id="96e27-321">The configuration system is not DI aware.</span></span>
* <span data-ttu-id="96e27-322">`IConfiguration`tem dois especializações:</span><span class="sxs-lookup"><span data-stu-id="96e27-322">`IConfiguration` has two specializations:</span></span>
  * <span data-ttu-id="96e27-323">`IConfigurationRoot`Usado para o nó raiz.</span><span class="sxs-lookup"><span data-stu-id="96e27-323">`IConfigurationRoot`  Used for the root node.</span></span> <span data-ttu-id="96e27-324">Pode disparar um recarregamento.</span><span class="sxs-lookup"><span data-stu-id="96e27-324">Can trigger a reload.</span></span>
  * <span data-ttu-id="96e27-325">`IConfigurationSection`Representa uma seção de valores de configuração.</span><span class="sxs-lookup"><span data-stu-id="96e27-325">`IConfigurationSection`  Represents a section of configuration values.</span></span> <span data-ttu-id="96e27-326">O `GetSection` e `GetChildren` métodos retornam um `IConfigurationSection`.</span><span class="sxs-lookup"><span data-stu-id="96e27-326">The `GetSection` and `GetChildren` methods return an `IConfigurationSection`.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="96e27-327">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="96e27-327">Additional resources</span></span>

* [<span data-ttu-id="96e27-328">Trabalhando com vários ambientes</span><span class="sxs-lookup"><span data-stu-id="96e27-328">Working with Multiple Environments</span></span>](environments.md)
* [<span data-ttu-id="96e27-329">Armazenamento seguro dos segredos do aplicativo durante o desenvolvimento</span><span class="sxs-lookup"><span data-stu-id="96e27-329">Safe storage of app secrets during development</span></span>](../security/app-secrets.md)
* [<span data-ttu-id="96e27-330">Hospedagem no núcleo do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="96e27-330">Hosting in ASP.NET Core</span></span>](xref:fundamentals/hosting)
* [<span data-ttu-id="96e27-331">Injeção de dependência</span><span class="sxs-lookup"><span data-stu-id="96e27-331">Dependency Injection</span></span>](dependency-injection.md)
* [<span data-ttu-id="96e27-332">Provedor de configuração do Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="96e27-332">Azure Key Vault configuration provider</span></span>](xref:security/key-vault-configuration)
